## 1、<span style="color:brown">JVM详解</span>🦺🦺🦺

**1.1、本质：**

Java虚拟机，就是<u>*Java二进制字节码的运行环境*</u>。它保证了<span style="color:red">**Java跨平台特性**</span>，<u>负责自动管理内存，提供垃圾回收机制</u>！！！

**1.3、JDK、JRE、JVM之间的关联：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/JVM-JRE-JDK.png" alt="image-20221114165935547" style="zoom: 50%;" />

**1.4、JVM是如何运行的？**

1. `JVM`的装入环境和配置；
2. 装载`JVM`；
3. 初始化`JVM`，获得本地调用接口；
4. 运行`Java`程序；

**1.5、JVM内部组成：**

​		JVM 主要由四大部分组成：`ClassLoader`（类加载器），`Runtime Data Area`（运行时数据区，内存分区），`Execution Engine`（执行引擎），`Native Interface`（本地库接口）

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/JVM-Structure.png" alt="image-20221114171449060" style="zoom: 67%;" />



## 2、<span style="color:brown">程序计数器：</span>Program Counter Register

**2.1、存储内容：**

程序计数器是<u>当前线程所执行的字节码的*行号指示器*</u>。

当<span style="color:orange">字节码解释器</span>工作时，就是**改变程序计数器中的数值**来<u>*选取下一条需要执行的字节码指令*</u>。

因此：<span style="color:red">程序计数器存储的是下一条JVM字节码指令的执行地址</span>！！！

**2.2、JVM的多线程实现：**

通过**线程轮流切换**， 并**分配处理器执行时间**来实现。

因此：<u>*在任何一个确定的时刻，一个处理器都会执行一条线程中的指令*</u>。

**2.3、特点：**

- **<span style="color:green">线程私有</span>**；

  ```scss
  为了'线程切换后能恢复正确的执行位置', 每条线程都需要一个'独立的程序计数器', 这样可以保证每条线程之间计数器互不影响, 独立存储
  ```
  
- **<span style="color:green">不会存在内存溢出</span>**；



## 3、<span style="color:brown">虚拟机栈、本地方法栈：</span>

**3.1、分析：**

​		每一个线程都共享进程的堆内存和方法区，但<u>各线程都具备私有的程序计数器、虚拟机栈、本地方法栈</u>。其中<span style="color:red">虚拟机栈、本地方法栈的生命周期与线程相同</span>！！！

​		<span style="color:purple">每个线程在创建的时候都会创建一个虚拟机栈，而物理内存是固定的，栈内存划分的越大，可分配的线程数就越少</span>！！！

|      |                Java Virtual Machine Stacks                 |      Native Method Stack       |
| :--: | :--------------------------------------------------------: | :----------------------------: |
| 作用 | **<span style="color:blue">Java方法执行的内存模型</span>** |    Native方法执行的内存模型    |
| 区别 |           为虚拟机执行的*Java方法 (字节码)* 服务           | 为虚拟机执行的*Native方法*服务 |

**3.2、栈帧：**

> 每一个方法从调用到执行完成，对应*一个栈帧在虚拟机栈中入栈到出栈的过程*。

每一个方法在执行的同时，都会创建一个栈帧来<u>存储方法相关的信息</u>，包括：

```apl
局部变量表、操作数栈、动态链接、方法出口......
```

<span style="color:green">栈内存中栈帧是**自动弹出**，因此<u>不需要进行垃圾回收</u></span>！！！

**3.3、局部变量表的<u>线程安全分析</u>：**

用于存放*<u>方法内的局部变量</u>*，其线程安全分析为：

1. 方法内局部变量没有逃离方法的作用范围，线程安全；
2. 局部变量引用了对象，且逃离了方法作用范围，需要考虑线程安全；

**3.4、局部变量表中的<u>局部变量类型</u>：**

- 编译期可知的各种基本数据类型；

  ```scss
  1. 64位(8字节)长度的long、double类型的数据会占用2个'局部变量空间(Slot)';
  2. 其余数据类型只占用1个;
  ```

- 对象引用；

  ```scss
  referencd类型, 不等同于对象本身, 可能是: 
  	1. 指向对象起始地址的引用地址;
  	2. 其他与此对象相关的位置;
  ```

- returnAddress类型；

  ```apl
  指向一条'字节码指令的地址'
  ```

**3.5、异常状况：**

根据JVM规范，在**虚拟机栈**与**本地方法栈**会有两种异常状况：

1. StackOverflowError异常

   ```apl
   当线程请求的栈深度 > 虚拟机允许的深度
   ```

2. OutOfMemoryError异常

   ```apl
   当'虚拟机栈'进行动态扩展时, 无法申请足够的内存
   ```



## 4、<span style="color:brown">堆：</span>Heap

### <!--JDK1.7以后, 方法区存在于Heap的元空间中-->

![image-20221115165510181](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Heap-Xms.png)

**4.1、作用：**

存放*<u>对象实例</u>*，类别有：

- 使用new关键词创建的实例对象；
- 数组；

**4.2、特征：**

- JVM内存管理中最大的一块；
- 堆内存被所有线程共享；
- 在虚拟机启动时，堆内存被创建；
- 有垃圾回收机制，因此也被称为：GC堆；

**4.3、划分：**

现在的收集器基本采用：<u>**分代收集算法**</u>，所以Java堆可以划分为：新生代、老年代。

之后还可以更进一步划分许多区域，这样有利于：<u>*更好的内存回收、更快的内存分配*</u>！！！

**4.4、特点：**

Java堆内存可以处于<u>物理上不连续的内存空间</u>，只需<u>逻辑上连续</u>即可。

在实现时，可以实现成**固定大小**，也可以是**可扩展的**。



## 5、<span style="color:brown">方法区：</span>Method Area

### <!--在HotSpot虚拟机上, 该区域被称为: 永久代-->

### <!--自JDK1.7以后, 方法区的常量池的逻辑地址还是方法区, 但物理地址在Heap-->

**5.1、作用：**

用于存储已被JVM加载的**类信息、常量、静态变量、即时编译器编译后的代码**等数据。

**5.2、特征：**

1. 被所有线程共享；
2. 有垃圾回收机制；

**5.3、特点：**

除了和`Heap`一样<u>不需要连续的内存</u>和<u>可以选择**固定大小**或者**可扩展**</u>外，还可以<span style="color:green"><u>*选择不实现垃圾回收*</u></span>。

**5.4、回收目标：**

主要是<span style="color:orange">对常量池的回收</span>和<span style="color:orange">对类型的卸载</span>，尽管在后者回收上效果不佳，但这是必须的回收！！！

**5.5、常量池分类：**

- 静态常量池，又名：Class常量池、常量池
- 运行时常量池
- 字符串常量池



## 6、<span style="color:brown">运行时常量池：</span>Runtime Constant Pool

**6.1、Class文件的内容：**

包含：<u>类版本、字段、方法、接口</u>等信息，还包含了<span style="color:violet">常量池</span>，用于<u>*存放**编译期生成的各种字面值和符号***</u>。

**6.2、常量池：**

常量池相当于一张表，JVM根据这张表找到类名、方法名、参数类型、字面值等。

当类被加载后，<u>Class文件中常量池的内容就会进入方法区的运行时常量池</u>存放！！！

**6.3、特点：**

- 具备<span style="color:orange">**动态性**</span>；

  ```scss
  并非'预置入Class文件中常量池的内容'才能进入方法区的运行时常量池, 在运行期间也是可以将'新的常量放入池中'
  			- 如: String的intern()方法'
  ```

- <span style="color:orange">**属于方法区的一部分**</span>；



## 7、<span style="color:brown">直接内存：</span>Direct Memory

**7.1、概述：**

>  直接内存并不属于JVM，而是操作系统的内存

- 常用于**NIO操作**，在进行NIO数据读写时，用于数据缓冲区；
- 分配回收成本高，但读写能力强；
- 不受JVM内存回收管理；

**7.2、演示：**

<u>*方式1*</u>：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/direct-memory01.png" alt="image-20221130161724008" style="zoom:80%;" />

<u>*方式2*</u>：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/direct-memory02.png" alt="image-20221130162244652" style="zoom:80%;" />

*<u>内存模型对比</u>*：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/directmemory-model.png" alt="image-20221130163934355" style="zoom:80%;" />

**7.3、内存释放分析：**

```java
public static final int _1Gb = 1024*1024*1024;
public static void main(String[] args) throws IOException{
    ByteBuffer byteBuffer = ByteBuffer.allocateDirect(_1Gb);
    System.out.println("分配完成...");
    System.in.read();
    System.out.println("开始释放...");
    System.in.read();
    byteBuffer = null;
    System.gc();
    System.in.read();
}
```

*通过查看ByteBuffer源码中的一个静态方法*：

`public static ByteBuffer allocateDirect(int capacity) {return new DirectByteBuffer(capacity);}`

<span style="color:red">然后查看**DirectByteBuffer类**，在<u>*构造器方法*</u>调用`base = unsafe.allocateMemory(size)`，完成***直接内存的分配任务***</span>。

---

<u>*而在构造器方法中`cleaner = Cleaner.create(this, new Deallocator(base, size, cap));`*</u>表明<span style="color:green">在 **回调类Deallocator** 的`run()`方法中，直接调用了`unsafe.freeMemory(address)`</span>，而Cleaner类在Java类库中是一种**虚引用类型**，当它所关联的<u>*对象(this)*</u>被回收时，就会触发<u>虚引用对象Cleaner</u>的clean()方法来**<u>实现直接内存的释放</u>**；

---

使用了<u>Unsafe对象</u>完成直接内存的分配回收，并且回收需要主动调用freeMemory方法；

ByteBuffer的实现类内部，使用了Cleaner(虚引用)来监测ByteBuffer对象，一旦ByteBuffer对象被垃圾回收，那么就会由ReferenceHandler线程通过Cleaner的clean方法调用freeMemory 来释放直接内存；

**7.4、直接内存调优：**

采用<u>禁止显示内存回收机制</u>，而`System.gc()`是一种**显示内存回收机制**，此时会触发Full GC，并且影响直接内存性能。

- 可以在VM options中设置：`-XX:+DisableExplicitGC`，禁用掉代码中的`System.gc()`，直接进行直接内存的垃圾回收；

- 手动创建Unsafe对象，然后调用freeMemory()方法；

  ```java
  public static final int _1Gb = 1024*1024*1024;
  public static void main(String[] args) throws IOException{
  	Unsafe unsafe = Unsafe.getUnsafe();
  	long l = unsafe.allocateMemory(_1Gb);
  	unsafe.freeMemory(l);
  }
  ```
