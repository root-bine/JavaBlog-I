## 1、<span style="color:brown">Garbage-First概述：</span>

**1.1、演替过程：**

> JDK1.8默认GC回收器不是G1，需要手动设置

- 2004论文发布
- 2009 JDK 6u14体验 
- 2012 JDK7u4官方支持
- 2017 JDK9默认，舍弃CMS

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

**2.1、G1如何避免全堆扫描？**

> 对于HotSpot JVM，使用了卡标记（Card Marking）技术来解决老年代到新生代的引用问题。具体是，<u>使用卡表（Card Table）和写屏障（Write Barrier）来进行标记并加快对GC Roots的扫描</u>

 <u>G1收集器的Region之间的对象引用</u>，和<u>其他收集器的新生代与老年代之间的对象引用</u>，虚拟机都是<u>使用`Remembered Set`（记忆集合）</u>来避免全堆扫描。

---

<span style="color:green">在Garbage-First收集器中，每一个Region都有一个对应的Remembered Set</span>。当<u>JVM发现程序在对Reference类型的数据</u>进行**写操作**时，会产生一个<u>`Write Barrier`（写屏障）</u>来**暂时中断写操作**，然后*检查Reference引用的对象是否处于不同Region之间*：

```scss
如果是, 则通过卡'表(Cardable)'把相关引用信息记录到'被引用对象'所属的Region的Remembered Set
```

当进行GC时，在<u>`GC Roots`（GC 根节点）的枚举范围</u>中加入Remembered Set，即可***保证不对全堆扫描，也不会遗漏***！！！

**2.2、G1收集器运行过程：**

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
  重新标记是'修正并发标记期间因用户程序继续运行', 导致'标记发生变动的部分对象'的记录
  ```

- 筛选回收（需要`Stop The World`）

  ```scss
  对'各个region的回收价值和成本'进行排序, 根据用户所期望的GC停顿时间制定回收计划
  ```


**2.3、G1垃圾回收阶段：**

![image-20221202223535231](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/G1%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E9%98%B6%E6%AE%B5.png)

