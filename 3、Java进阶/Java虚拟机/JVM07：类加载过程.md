## 1、<span style="color:brown">类加载过程简介：</span>

**1.1、类的生命周期：**

一个类的完整生命周期如下：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/ClassLoading01.png" alt="image-20221209214224339" style="zoom:80%;" />

**1.2、类加载过程：**

<span style="color:brown"><u>*Class 文件需要加载到虚拟机中之后才能运行和使用*</u></span>，那么虚拟机是如何加载这些 Class 文件呢？

系统加载 Class 类型的文件主要三步：**加载->连接->初始化**。连接过程又可分为三步：**验证->准备->解析**。

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/ClassLoading02.png" alt="image-20221209214303432" style="zoom:80%;" />

**1.3、卸载：**

<span style="color:brown">卸载类即该类的 Class 对象被 GC</span>。

卸载类需要满足 3 个要求:

1. 该类的所有的实例对象都已被 GC，即：堆不存在该类的实例对象；
2. 该类没有在其他任何地方被引用；
3. 该类的类加载器的实例已被 GC；

---

在 JVM 生命周期内，由 <span style="color:red">**jvm 自带的类加载器**</span>加载的类是不会被卸载的。但是由 <span style="color:green">**自定义的类加载器**</span>加载的类是可能被卸载的。

因此，<u>jdk 自带的 `BootstrapClassLoader`, `ExtClassLoader`, `AppClassLoader`负责加载 jdk 提供的类</u> ，所以类加载器的实例肯定不会被回收。而<u>自定义的类加载器的实例是可以被回收的，所以使用自定义加载器加载的类是可以被卸载掉的</u>。



## 2、<span style="color:brown">类加载过程详解：</span>

### [#](#加载) 加载

<span style="color:brown">类加载过程的第一步</span>，主要完成下面 3 件事情：

1. 通过**全类名**获取<u>定义此类的二进制字节流</u>；
2. 将**字节流所代表的静态存储结构**转换为**方法区的运行时数据结构**；
3. 在内存中生成一个**代表该类的 `Class` 对象**，作为<u>*方法区这些数据的访问入口*</u>；

<span style="color:green">一个非数组类的加载阶段（加载阶段获取类的二进制字节流的动作）是可控性最强的阶段</span>：

```scss
1. 这一步可以'自定义类加载器去控制字节流的获取方式'(重写一个类加载器的loadClass()方法);
2. 数组类型不能通过类加载器创建, 它由 Java 虚拟机直接创建;
```

<span style="color:red">**加载阶段和连接阶段的部分内容是交叉进行的**，加载阶段尚未结束，连接阶段可能就已经开始</span>！！！

### [#](#验证)验证

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/ClassLoading03.png" alt="image-20221209221043920" style="zoom:80%;" />

### [#](#准备)准备

<span style="color:brown">准备阶段是正式<u>为**类变量**分配内存</u>并<u>设置**类变量**初始值</u>的阶段</span>，**这些内存都将在方法区中分配**。对于该阶段有以下几点需要注意：

1. 此时进行内存分配的仅包括<span style="color:green">类变量（ 即静态变量，被 `static` 关键字修饰的变量），而不包括实例变量</span>；

   - 实例变量会在对象实例化时，随着对象一块分配在 Java 堆中

2. 从概念上讲，类变量所使用的内存都应当在 **方法区** 中进行分配：

   - JDK 7 之前，HotSpot 使用永久代来实现方法区的时候，实现是完全符合这种逻辑概念的。；
   - JDK 7 及之后，HotSpot 已经把原本放在永久代的字符串常量池、静态变量等移动到堆中，此时类变量则会随着 Class 对象一起存放在 Java 堆中；

3. 这里所设置的初始值"通常情况"下是数据类型默认的零值（如 0、0L、null、false 等）；

   ```scss
   # 定义public static int value=111, 则'value变量在准备阶段的初始值就是0而不是 111'(初始化阶段才会赋值)
   # 定义public static final int value=111, 则'在准备阶段value的值就被赋值为 111'
   ```

![image-20221209222009385](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/ClassLoading04.png)

### [#](#解析) 解析

<span style="color:brown">解析阶段是虚拟机将**常量池内的符号引用**替换为**直接引用**的过程</span>，主要针对7 类符号引用：**类或接口、字段、类方法、接口方法、方法类型、方法句柄、调用限定符**进行。

|   名称   |                           定义                           |
| :------: | :------------------------------------------------------: |
| 符号引用 |           一组符号来描述目标，可以是任何字面量           |
| 直接引用 | 直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄 |

在程序实际运行时，只有符号引用是不够的，举个例子：在程序执行方法时，系统需要明确知道这个方法所在的位置。Java 虚拟机为每个类都准备了一张方法表来存放类中所有的方法。当需要调用一个类的方法的时候，只要知道这个方法在方法表中的偏移量就可以直接调用该方法了。通过解析操作符号引用就可以直接转变为目标方法在类中方法表的位置，从而使得方法可以被调用。

综上，<span style="color:green">解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程，也就是<u>*得到类或者字段、方法在内存中的指针或者偏移量*</u></span>。

### [#](#初始化) 初始化

> 说明： `<clinit> ()`方法是编译之后自动生成的

<span style="color:brown">初始化阶段是**执行初始化方法 `<clinit> ()`方法的过程**，是类加载的最后一步</span>，此时 JVM 才开始真正执行类中定义的 <u>Java 程序代码(字节码)</u>。

**对于`<clinit> ()` 方法的调用，虚拟机会自己确保其在多线程环境中的安全性**。因为 `<clinit> ()` 方法是带锁线程安全，所以在多线程环境下进行类初始化的话可能会引起多个线程阻塞，并且这种阻塞很难被发现。

---

对于初始化阶段，虚拟机严格规范了有且只有 6 种情况下，必须对类进行初始化(只有主动去使用类才会初始化类)：

1. 当遇到`new`、`getstatic`、`putstatic`或 `invokestatic`这 4 条直接码指令时：
   - 当 jvm 执行 `new` 指令时会初始化类，即：当程序创建一个类的实例对象；
   - 当 jvm 执行 `getstatic` 指令时会初始化类，即：程序访问类的静态变量(不是静态常量，常量会被加载到运行时常量池)；
   - 当 jvm 执行 `putstatic` 指令时会初始化类，即：程序给类的静态变量赋值；
   - 当 jvm 执行 `invokestatic` 指令时会初始化类，即程序调用类的静态方法；
2. <u>使用 `java.lang.reflect` 包的方法</u>对类进行反射调用时如 `Class.forname("...")`, `newInstance()` 等等，如果类没初始化，需要触发其初始化
3. 初始化一个类，如果其父类还未初始化，则先触发该父类的初始化
4. 当虚拟机启动时，用户需要定义一个要执行的主类 (包含 `main` 方法的那个类)，虚拟机会先初始化这个类
5. <u>*`MethodHandle` 和 `VarHandle` 可以看作是轻量级的反射调用机制*</u>，而要想使用这 2 个调用， 就必须先使用 `findStaticVarHandle` 来初始化要调用的类
6. 当一个接口中定义了 JDK8 新加入的默认方法（被 default 关键字修饰的接口方法）时，如果这个接口的实现类发生了初始化，那该接口要在其之前被初始化

