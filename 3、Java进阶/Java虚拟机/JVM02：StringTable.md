## 1、<span style="color:brown">StringTable概述：</span>

**1.1、引言：**

StringTable别名：串池、字符串常量池、Stringpool，<u>*串池为Hash表存储结构*</u>。

**1.2、特征：**

- 避免重复创建字符串对象；

  ```java
  String s1 = "a";
  String s2 = "a";
  boolean isEqual = (s1 == s2);//为true, 因为都是字符串池中的一个对象
  ```

- 常量池中的字符串仅是符号，第一次用到时才变为对象；

- 字符串变量拼接原理：StringBuilder（JDK1.8）；

- 字符串常量拼接原理：编译期优化；

- 使用intern方法，将<u>*串池中还没有的字符串对象放入串池*</u>；

**1.3、位置：**

​	在JDK1.6及其以前，StringTable存在在方法区中，但由于方法的常量池空间默认大小为4M，当存储内容超过这个空间大小就会抛出OutOfMemoryError异常。因而在JDK1.7、1.8以后，将StringTable存放到了Heap中。



## 2、<span style="color:brown">特性验证：</span>

**2.1、字符串变量拼接：**

<u>*底层原理：*</u>

> toString()方法的返回值为：`return new String(value, 0, count)`，相当于`new String(...)`

​	创建一个StringBuilder对象： `new StringBuilder(String str)`， 然后再调用`append()`将字符串逐个添加到`StringBuilder`对象中。最后，通过调用`toString()`方法将`StringBuilder`对象转换为一个新的`String`对象。

<u>*代码分析：*</u>

```java
String s1 = "a";
String s2 = "b";
String s3 = "ab";
String s4 = s1 + s2;
// ==比较的是地址是否相同
System.out.println(s3 == s4);// false
```

<u>*变量s3位于串池中，而s4位于堆中*</u>，位置不一致。

**2.2、字符串常量拼接：**

```java
String s1 = "ab";
String s2 = 'a' + 'b';
System.out.println(s1 == s2);//true
```

底层原理：***javac在编译期间的优化***，结果已经在编译器确定为ab！！！

**2.3、字符串加载延迟：**

在IDEA采用断点+DeBug，在控制台的Memory验证字符串的延迟加载

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%8A%A0%E8%BD%BD%E5%BB%B6%E8%BF%9F.png" alt="image-20221116220746532" style="zoom:67%;" />

结论：<u>*常量字符串被引用时，它是缓加载的，遇到了才会加载*</u>！！！



## 3、<span style="color:brown">intern原理：</span>

**3.1、方法详解：**

```java
// java.lang.String
public native String intern()  
```

当intern()方法被调用的时候：

- 如果字符串常量池中已经存在这个字符串对象了，就*返回常量池中该字符串对象的地址*；
- 如果字符串常量池中不存在，就在常量池中<u>***创建一个指向该对象堆中实例的引用，并返回这个引用地址***</u>；

**3.2、intern of JDK1.8：**

```java
// String对象在Heap中, 而指向的String s则在Stack中
String s = new String("a") + new String("b");
String s1 = s.intern();
System.out.println(s1=="ab");//true
System.out.println(s=="ab");//true
System.out.println(s1==s);//true
```

执行第一行代码，在堆中创建5个对象：

- 在堆中创建两个匿名对象，然后将"a"、"b"存储到StringTable中；
- 在Heap中创建一个String对象，其内容为："ab"，并**<u>指向栈中引用对象s</u>**；

然后执行`s.intern()`，在 StringTable中查找内容为"ab"的字符串对象，但不存在，就将<u>***堆中String对象的引用地址***</u>存储到StringTable，其内容为："ab"，并返回该引用地址。

综上所述：

1. s1 == "ab"/s == "ab"，由于"ab"已经存在于StringTable中，且其地址与引用对象s1/s相同；
2. s1 == s，由于StringTable中没有内容为"ab"的字符串对象，就将<u>***堆中String对象的引用地址***</u>存储到StringTable，其内容为："ab"，并返回该引用地址，并指向s1；

---

```java
String str7 = new String("wang") + new String("zhen");
String str8 = "wangzhen";
System.out.println(str7.intern() == str8); // true
System.out.println(str7 == str8); // false
System.out.println(str8 == "wangzhen"); // true
```

创建两个匿名对象，然后将"wang"、"zhen"存储到StringTable中，然后创建一个String对象，内容为："wangzhen"，并指向str7。

而`String str8 = "wangzhen"`直接将内容存储到StringTable，并指向str8。

执行`str7.intern()`，在StringTable中查找内容为"wangzhen"的字符串对象，但已经存在，则返回字符串常量池中"wangzhen"的引用地址。

对于`str8 == "wangzhen"`，此处的"wangzhen"也是指StringTable中已经存在这个字符串对象地址。

---

```java
String s1 = "a";
String s2 = "b";
String s3 = "a" + "b";
String s4 = s1 + s2;
String s5 = "ab";
String s6 = s4.intern();
System.out.println(s3 ==s4);// false
System.out.println(s3 ==s5);// true
System.out.println(s3 ==s6);// true
String x2 = new String("c") + new String("d");
String x1 = "cd";
x2.intern();
System.out.println(x1 == x2);// false
```

从引用对象s1执行到s5，在字符串常量池中存储的有：内容为"a"的字符串对象指向s1、内容为"b"的字符串对象指向s2、内容为"ab"的字符串对象指向s3、s5，在Heap中创建一个内容为"ab"的String对象，指向s4。

执行`s4.intern()`，在StringTable中查找内容为"ab"的字符串对象，但已经存在，则返回内容为"ab"的字符串对象的地址，指向s6。

对于`String x2 = new String("c") + new String("d")`，会首先创建两个匿名对象，然后将内容分别为"c"、"d"的两个字符串对象放入StringTable中，然后在Heap中创建一个内容为"cd"的String对象，然后指向x2。

而`String x1 = "cd"`直接将内容为"cd"的字符串对象指向x1，并存储到StringTable中。

虽然`x2.intern()`，在StringTable能查到内容为"cd"的字符串对象，会返回内容为"cd"的字符串对象的地址，但后续比较的是x1 ==x2，x1在StringTable中，x2在Heap中，因为两者地址完全不同！！



## 4、<span style="color:brown">StringTable垃圾回收：</span>

**4.1、串池内存设置：**

在Idea的Edit Configurations中的VM option配置`-Xmx10m -XX:+PrintStringTableStatistics -XX:+PrintGCDetails -verbose:gc`，主要作用是：划分堆内存空间为10M、打印串池的字符串个数，大小等信息、打印垃圾回收的详细信息。

**4.2、演示：**

```java
public class Test {
    public static void main(String[] args) throws InterruptedException{
        int i = 0;
        try {
            for (int j = 0; j < 1000; j++) {
                String.valueOf(j).intern();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            System.out.println(i);
        }
    }
}
```

**4.3、结果：**

当划分串池内存空间为：10M时，由于执行上述代码，导致串池空间耗尽，因而会执行一次垃圾回收，内容展示如下：

```scss
[GC (Allocation Failure) [PSYoungGen: 1024K->504K(1536K)] 1024K->612K(5632K), 0.0023232 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 
```



## 5、<span style="color:brown">StringTable性能调优：</span>

**5.1、概述：**

> 数组中的单个元素数称为桶

**桶跟最大存储数量没关系**，限<u>制StringTable的最大存储量的是堆内存的大小</u>，设置方式：`-XX: StringTableSize=桶个数`。

桶子越多，每个桶放的东东就越少，性能越高。

**5.2、调优操作：**

设置：-Xmx500m -XX:+PrintStringTableStatistics -XX:+PrintGCDetails -XX: StringTableSize = 20000

![image-20221130153329513](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/StringTable%E8%B0%83%E4%BC%98.png)

上述代码的内存分布占用情况，可以用**jvirsualvm工具**查看，结果为：<u>`java.lang.String`和`char[]`两部分占比80%，近300M</u>。

而将上述代码的**address.add(line)**修改为：`address.add(line.intern())`，将字符串对象先进行入串池操作，然后再将串池中的对象返回给list集合。这样操作的结果为：<u>`java.lang.String`和`char[]`占比约为30%，`java.lang.Object[]`占比约为50%</u>。

**5.3、应用场景：**

在应用中，如果<u>*存在大量的字符串对象，且可能重复*</u>的情况，可以使用`intern()`方法进行入串池操作，减少堆内存的消耗！！！

