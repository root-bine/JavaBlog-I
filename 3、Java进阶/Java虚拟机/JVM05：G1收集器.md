## 1、<span style="color:brown">Garbage-First简介：</span>

**1.1、演替过程：**

> JDK1.8默认GC回收器不是G1，需要手动设置
>
> <span style="color:green">G1收集器，工作于**新生代和老年代**</span>，并且因为筛选回收这个过程，又称：<u>***垃圾优先收集器***</u>！！！

| 2004   论文发布 | 2009    JDK 6u14体验 | 2012   JDK7u4官方支持 | 2017   JDK9默认，舍弃CMS |
| --------------- | -------------------- | --------------------- | ------------------------ |

**1.2、特点：**

- 并发与并行

  ```scss
  1. G1在多CPU、多核环境下, 能够'缩短STW的时间'
  2. G1收集器依旧'采用并发的方式', 与用户线程一起执行
  ```

- 分代收集

  ```scss
  在G1之前的其他GC收集器进行收集的范围是: 整个新生代或者老年代
  
  G1的收集范围是: 
  	1. 将整个Java Heap内存分成多个相等的独立区域(Region)
  	2. 保留新生代和老年代的概念, 但不再是物理隔离, 而是一部分Region'不需要连续的集合'
  ```

- 空间整合

  ```scss
  整体上是Mark-Compact算法, 局部(两个Region之间)是Coping算法, 两中算法表明: 
  		# G1运行期间'不会产生空间内存碎片', 且'收集后能提供规整的可用内存';
  		# 有利于程序长时间运行, 在'分配大对象时不会因无法找到连续内存空间', 而触发下一次FullGC
  ```

- 可预测的停顿

  ```scss
  同时注重'吞吐量(Throughput）和低延迟(Low latency)', 默认的暂停目标是200 ms
  G1实现可预测的停顿的基础:
  		1. 保留了'低停顿'的特性
  		2. 建立了'可预测的停顿时间模型': 明确指定'在一个长度为M毫秒的时间段内', 消耗在垃圾收集的时间不得超过N毫秒
  ```



## 2、<span style="color:brown">Garbage-First过程：</span>

**2.1、G1内存分布：**

<u>*G1对于整个堆空间所有的`Region`区不会在一开始就全部分配完*</u>，<span style="color:green">无论是新生代、幸存区以及年老代在最开始都是会有**初始数量**的，在程序运行过程中会**根据需求**不断<u>增加每个分代区域的`Region`数量</u></span>。

`Humongous`区存在的意义：

```scss
'避免一些的巨型对象直接进入年老代, 节约年老代的内存空间', 可以有效避免年老代因空间不足时的GC开销
```

 <u>当堆空间发生全局GC(`FullGC`)时，除开回收新生代和年老代之外，也会对`Humongous`区进行回收</u>！！！

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/G1%E5%88%92%E5%88%86Heap%E5%86%85%E5%AD%98.png" alt="image-20221204162301379" style="zoom:67%;" />

**2.2、G1如何避免全堆扫描？**

> 使用卡表（Card Table）和写屏障（Write Barrier）来**进行标记**并**加快对GC Roots的扫描**

 <u>G1收集器的Region之间的对象引用</u>，和<u>其他收集器的新生代与老年代之间的对象引用</u>，虚拟机都是<u>使用`Remembered Set`（双向卡表）</u>来避免全堆扫描。

---

<span style="color:green">在Garbage-First收集器中，每一个Region都有一个对应的Remembered Set</span>。当<u>JVM发现程序在对Reference类型的数据</u>进行**写操作**时，会产生一个<u>`Write Barrier`（写屏障）</u>来**暂时中断写操作**，然后*检查Reference引用的对象是否处于不同Region之间*：

```scss
如果是, 则通过'卡表(Cardable)'把相关引用信息记录到'被引用对象'所属的Region的Remembered Set
```

当进行GC时，在<u>`GC Roots`（GC 根节点）的枚举范围</u>中加入Remembered Set，即可***保证不对全堆扫描，也不会遗漏***！！！

**2.3、G1收集器运行过程：**

![image-20221204175505680](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/G1%E6%94%B6%E9%9B%86%E5%99%A8%E7%9A%84%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B.png)

- 初始标记（需要`Stop The World`，耗时很短）

  ```scss
  1. 标记GC Roots能够直接关联到的对象
  2. 修改TAMS(Next Top at Mark Start)的值, 让下一阶段用户并发运行时, 能在正确可用的Region中创建对象
  ```

- 并发标记

  ```scss
  # 从GC Roots开始对堆中对象进行可达性分析, 找出存活的对象
  # 该阶段'耗时较长, 可与用户线程并发执行'
  ```

- 最终标记（需要`Stop The World`）

  ```scss
  # 最终标记是'修正并发标记期间因用户程序继续运行', 导致'标记发生变动的部分对象'的记录, 该过程'可与用户线程并发执行'
  # 该阶段需要把'这段时间对象变化'记录到线程Remember Set Logs, 然后将Remember Set Logs的数据合并到Remembered Set
  ```

- 筛选回收（需要`Stop The World`）

  ```scss
  # 对'各个region的回收价值和成本'进行排序, 根据用户所期望的GC停顿时间制定回收计划
  # 筛选回收阶段在G1收集器中是'停止所有用户线程后, 采用多线程并行回收的', 这个过程是'可与用户线程并发执行'
  # 由于'G1只回收一部分Region区, 且停顿时间是可控的', 因此: 停止用户线程后回收效率会大幅度提高
  ```

**2.4、G1中的参数：**

| 格式                               | 作用                       | 说明                                                         |
| ---------------------------------- | -------------------------- | ------------------------------------------------------------ |
| -XX :+UseG1GC                      | 开启G1收集器               | JDK1.8默认收集器不是G1                                       |
| -XX:G1HeapRegionSize=size          | 强制指定Region的大小       | <span style="color:red">不推荐</span>，**默认的计算方式**得出的是<u>*最适合管理堆空间*</u>的大小 |
| -XX:MaxGCPauseMillis=time          | 设定GC预期回收STW值        | 如果未使用该参数，G1默认STW值为：200ms                       |
| -XX:InitiatingHeapOccupancyPercent | 设置**老年代占Heap的比例** | 默认值为：45%                                                |



## 3、<span style="color:brown">G1的GC类型：</span>

**3.1、YoungGC：**

G1收集器在发生`YoungGC`时，<span style="color:green">**复制移动对象时是采用的多线程并行复制，以此来换取更优异的GC性能**</span>！！！

---

在G1中，当新生代区域被用完时，<u>**G1会大概计算一下回收当前的新生代空间需要花费多少时间**</u>，如果<span style="color:red">回收时间远远小于`STW设定的值`，就不会触发YoungGC，而是会继续为新生代增加新的Region区用于存放新分配的对象实例</span>。

直至<u>*某次Eden区空间再次被放满*</u>并且<u>*计算后的回收的耗时接近STW设定的值*</u>，那么才会触发YoungGC。

- <u>*执行过程*</u>：

  ```scss
  # 在'分配一般对象(非巨型对象)'时, 当所有Eden Region使用达到'最大阀值并且无法申请足够内存'时, 会触发一次YoungGC
  
  # 每次YoungGC会回收所有Eden以及Survivor区之from, 并且将存活对象复制到Old区以及Survivor区之to
  ```

**3.2、MixedGC：**

当<span style="color:green">**整个堆中年老代的区域占有率**达到<u>设定的值后触发`MixedGC`</u></span>，会回收：

- 所有新生代`Region`区；
- 部分年老代`Region`区（会根据期望的GC停顿时间选择合适的年老代`Region`区优先回收）；
- 大对象`Humongous`区；

正常情况下，G1垃圾收集时会<span style="color:blue">先发生`MixedGC`</span>，<u>*主要采用Coping算法*</u>。

```scss
# 在触发MixedGC时, 先将要回收的Region区中存活的对象, 拷贝至别的Region区内
# 拷贝过程中, '如果发现没有足够多的空闲Region区承载拷贝对象, 就会触发一次FullGC'
```

**3.3、FullGC：**

> 该过程是单线程串行收集的，因此这个过程非常耗时的

当<span style="color:green">**<u>整个堆空间中的空闲`Region`不足以支撑拷贝对象</u>或<u>由于元数据空间满了</u>等原因**</span>，就会触发FullGC。

在发生`FullGC`时，**G1首先会停止系统所有用户线程**，然后<u>*采用单线程*</u>进行标记、清理和压缩整理内存，以便于清理出足够多的空闲`Region`来供下一次`MixedGC`使用。

**3.4、G1之FullGC的本质：**

<span style="color:red">G1收集器中并没有FullGC</span>，而是采用***Serial Old收集器的FullGC***。

由于G1在设计时的初衷就是要避免发生FullGC，而触发YoungGC、MixedGC后***是无法使得程序恢复正常执行***，最终就会触发SerialOld收集器的FullGC。



## 4、<span style="color:brown">跨代引用：</span>

**4.1、实现记忆集的方式：**

| 收集器版本                      | 数据结构                                                |
| ------------------------------- | ------------------------------------------------------- |
| CMS以及之前的大部分的分代收集器 | 采用的是<u>**卡表**`CardTable`的</u>                    |
| G1，又称：垃圾优先收集器        | 采用的是<u>**双向卡表**`Remembered Set`</u>，简称：RSet |

**4.2、卡表的本质：**

> `RSet`相当于`CardTable`的进阶实现方式

- G1收集器：

  ```scss
  # 在每个Region区中都存在一个RSet, 其中记录了'其他Region中的对象'引用'当前Region中对象'的关系
  # 记录'谁引用了我的对象', 属于points-into结构
  ```

- CMS，及其之前的分代收集器：

  ```scss
  记录'我引用了谁的对象', 属于points-out结构
  ```

G1中的`RSet`本质上就是一个哈希表结构（`HashTable`）：

| 键值对  | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| `Key`   | 其他引用当前区内对象的`Region`起始地址                       |
| `Value` | 是一个集合，里面的元素为其他`Region`中每个引用当前区内对象的地址 |

**4.3、Remembered Set操作：**

当<u>*发生`YoungGC`和扫描标记对象*</u>时，只需要**选定目标`Young Region`的`RSet`作为根集**，并且其中记录了`Old → Young`的跨代引用。

这样在发生`YoungGC`时，只需要扫描这些`RSet`即可，从而避免了扫描整个`Old Region`。

---

当<u>*G1发生`MixedGC`*</u>时，`Old Region`的`RSet`记录`Old → Old`的引用关系，而`Young Region`的`RSet`记录`Old → Young`的跨代引用。

这样在发生`MixedGC`时也不会出现整堆扫描的情况，所以引入`RSet`在很大程度上减少了大量的GC工作。



# 5、<span style="color:brown">G1总结：</span>
