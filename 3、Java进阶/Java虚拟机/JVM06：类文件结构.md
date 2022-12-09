## 1、<span style="color:brown">Class文件介绍：</span>

**1.1、Class文件结构：**

> 展示内容依次为：类型、名称、数量，未标注则数量默认为1

```scss 
ClassFile {
    u4             magic; //Class 文件的标志
    u2             minor_version;//Class 的小版本号
    u2             major_version;//Class 的大版本号
    u2             constant_pool_count;//常量池的数量
    cp_info        constant_pool[constant_pool_count-1];//常量池
    u2             access_flags;//Class 的访问标记
    u2             this_class;//当前类
    u2             super_class;//父类
    u2             interfaces_count;//接口
    u2             interfaces[interfaces_count];//一个类可以实现多个接口
    u2             fields_count;//Class 文件的字段属性
    field_info     fields[fields_count];//一个类可以有多个字段
    u2             methods_count;//Class 文件的方法数量
    method_info    methods[methods_count];//一个类可以有个多个方法
    u2             attributes_count;//此类的属性表中的属性数
    attribute_info attributes[attributes_count];//属性表集合
}
```

**1.2、Class文件概述：**

Class文件是<span style="color:green">一组以8位字节为基础单位的二进制流</span>，各个数据项严格按照顺序紧凑的排列在一起，中间没有任何分隔符号。当遇到**需要占用8个字节以上空间的数据项**时，会按照<u>*Big-Endian顺序*</u>的方式分割成若干8位字节进行存储。

[^Big-Endian]: 最高位字节位于地址的最低位，最低位的字节位于地址的最高位

根据Java虚拟机规范的规定，Class文件格式**采用一种类似于C语言结构体的伪结构**存储数据，而该结构的数据类型：<u>无符号数、表</u>。

**1.3、无符号数与表：**

<span style="color:red">无符号数属于基本数据类型</span>。u1、u2、u4、u8分别代表1个字节、2个字节、4个字节、8个字节的无符号数，可以用来描述<u>*数字、索引引用、数量值、按照UTF-8编码构成的字符串值*</u>。

---

<span style="color:red">表由**多个无符号数**或者**其他表作为数据项**构成的复合数据类型</span>，而<span style="color:green">**对于整个Class文件，本质上类文件就是一张表**</span>！！！

所有表均以`_info`结尾，用于描述**有层次关系的复合结构的数据**。

**1.4、Class文件的组成：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Class-Flies01.png" alt="image-20221208211928571" style="zoom: 50%;" />



## 2、<span style="color:brown">Class文件详解(一)：</span>

**2.1 魔数：**

> `u4             magic;` //Class 文件的标志

每个 Class 文件的<span style="color:brown">**头 4 个字节**</span>称为魔数（Magic Number）,它的唯一作用是**确定这个文件是否为一个能被虚拟机接收的 Class 文件**。

**2.2、Class 文件版本号：**

> `u2             minor_version;`//Class 的小版本号
> `u2             major_version;`//Class 的大版本号

<span style="color:brown">**紧接着魔数的四个字节**</span>存储的是 Class 文件的版本号：<u>第 5 和第 6 位是**次版本号**，第 7 和第 8 位是**主版本号**</u>。

<span style="color:green">每当 Java 发布大版本的时候，主版本号都会加 1</span>，可以使用 `javap -v` 命令来快速查看 Class 文件的版本号信息。

高版本的 Java 虚拟机可以执行低版本编译器生成的 Class 文件，但是低版本的 Java 虚拟机不能执行高版本编译器生成的 Class 文件。

**2.3、常量池：**

> `u2             constant_pool_count;`//常量池的数量
> `cp_info        constant_pool[constant_pool_count-1]`;//常量池

<span style="color:brown">**紧接着主次版本号之后**</span>的是常量池，常量池的数量是 `constant_pool_count-1`（**常量池计数器是从 1 开始计数的，将第 0 项常量空出来是有特殊考虑的，索引值为 0 代表“不引用任何一个常量池项”**）

常量池主要存放两大常量：

| 常量名称 | 内容                                                       |
| :------: | ---------------------------------------------------------- |
|  字面量  | 文本字符串、声明为 final 的常量值等                        |
| 符号引用 | 类和接口的全限定名、字段的名称和描述符、方法的名称和描述符 |

---

常量池中每一项常量都是一个表，这 14 种表有一个共同的特点：**表开始的第一位是一个 u1 类型的标志位 (tag) 来标识常量的类型，代表当前这个常量属于哪种常量类型**。

`.class` 文件可以通过指令来查看常量池中的信息：

```scss
javap -v 文件名称.java
javap -v class类名-> temp.txt					将结果输出到 temp.txt 文件
```



## 3、<span style="color:brown">Class文件详解(二)：</span>

**3.1、访问标志：**

> `u2     access_flags;`//Class 的访问标记

<span style="color:brown">**在常量池结束之后，紧接着的两个字节**</span>代表访问标志。这个标志用于<u>*识别一些类或者接口层次的访问信息*</u>，包括：这个 Class 是类还是接口，是否为 `public` 或者 `abstract` 类型，如果是类的话是否声明为 `final` 等等

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Class-Files02.png" alt="image-20221208211706703" style="zoom:67%;" />

**3.2、当前类，父类，接口索引集合**

> `u2             this_class;`//当前类
> `u2             super_class;`//父类
> `u2             interfaces_count;`//接口
> `u2             interfaces[interfaces_count];`//一个类可以实现多个接口

**当前类索引**用于确定这个类的全限定名，**父类索引**用于确定这个类的父类的全限定名。

<span style="color:green">由于 Java 语言的单继承，所以父类索引只有一个。除了 `java.lang.Object` 之外，所有的 java 类都有父类。因此除了 `java.lang.Object` 外，所有 Java 类的父类索引都不为 0</span>。

**接口索引集合**用来描述这个类实现了那些接口，<u>*这些被实现的接口将按 `implements` (如果这个类本身是接口的话则是`extends`) 后的接口顺序*</u>从<span style="color:blue">左到右排列在接口索引集合</span>中



## 4、<span style="color:brown">Class文件详解(三)：</span>

**4.1、字段表集合：**

> `u2             fields_count;`//Class 文件的字段的个数
> `field_info     fields[fields_count];`//一个类会可以有个字段

<span style="color:brown">**字段表用于描述接口或类中声明的变量**</span>。字段包括<u>类级变量以及实例变量</u>，但不包括在方法内部声明的局部变量。

---

字段表的结构:

![image-20221209162044103](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Class-Files03.png)

**access_flags:** 字段的作用域（`public` ,`private`,`protected`修饰符），是实例变量还是类变量（`static`修饰符）,可否被序列化（transient 修饰符）,可变性（final）,可见性（volatile 修饰符，是否强制从主内存读写）。

**name_index:** 对常量池的引用，表示的字段的名称；

**descriptor_index:** 对常量池的引用，表示字段和方法的描述符；

**attributes_count:** 一个字段还会拥有一些额外的属性，attributes_count 存放属性的个数；

**attributes[attributes_count]:** 存放具体属性具体内容。

**4.2、方法表集合：**

> `u2             methods_count;`//Class 文件的方法的数量
> `method_info    methods[methods_count];`//一个类可以有个多个方法

<span style="color:brown">Class 文件存储格式中对方法的描述与对字段的描述几乎采用了完全一致</span>。方法表的结构如同字段表一样，依次包括了<u>*访问标志、名称索引、描述符索引、属性表集合。*</u>

方法表的结构:（<u>*各部分描述与字段表集合一致*</u>）

![image-20221209162901939](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Class-Files04.png)

**4.3、属性表集合：**

>    `u2             attributes_count;`//此类的属性表中的属性数
>    `attribute_info attributes[attributes_count];`//属性表集合

<span style="color:brown">在 Class 文件，字段表，方法表中都可以携带自己的属性表集合，以用于描述某些场景专有的信息</span>。

与 Class 文件中其它的数据项目要求的顺序、长度和内容不同，<span style="color:green">属性表集合不再要求各个属性表具有严格的顺序，并且只要不与已有的属性名重复</span>。因此，任何人实现的编译器都可以向属性表中写 入自定义的属性信息，而Java 虚拟机运行时会忽略掉它不认识的属性。

