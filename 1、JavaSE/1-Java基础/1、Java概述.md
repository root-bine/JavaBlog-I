## 1、<span style="color:brown">基础内容：</span>😶‍🌫️😶‍🌫️😶‍🌫️

**1.1、Java组成概述：**

- `Java`三大版本

  `JavaSE`：标准版（桌面程序、控制台开发......）

  `JavaME`：嵌入式开发（手机、小家电......）

  `JavaEE`：E企业级开发（web端、服务器开发......）

- `JDK、JRE、JVM`之间的关系？
  1. `JDK`是整个`Java`的核心，包括了`Java`运行环境`JRE`、`Java`工具和`Java`基础类库；
  2. `JRE`是运行`Java`程序所必须的环境的集合，包含`JVM`、`Java`核心类库；
  3. `JVM` 即为 **`Java 虚拟机`**，是整个`java`实现跨平台的最核心的部分；

**1.2、Java是值传递还是引用传递？编码是什么？**

- <span style="color:red">Java中只有**值传递**，没有引用传递</span>；

- <span style="color:green">Java语言采用**Unicode编码标准**，它为每个字符制订了一个唯一的数值</span>；

**1.3、为什么Java代码可以实现一次编写、到处运行？**

​		`JVM`（`Java`虚拟机）是`Java`跨平台的关键。

​		在程序运行前，<u>`Java`源代码`(.java)`</u>需要经过编译器编译成<u>字节码`(.class)`</u>。在程序运行时，**不同平台的`JVM`**负责将字节码翻译成特定平台下的机器码并运行。

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Java%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.png" alt="java运行机制" style="zoom:67%;" />

## 2、<span style="color:brown">UnicodeBolck详解：</span>🍔🍔🍔

**2.1、什么是UnicodeBlock？** 

​	`UnicodeBlock`是**`Java中的一个枚举类型`**，用于表示**`Unicode字符的块范围`**。它提供了一系列常量，每个常量代表一个Unicode字符块的范围。

**2.2、UnicodeBlock有哪些常见的字符块？ **

基本拉丁字母、汉字、日文假名、希腊字母等，还包括一些特殊的字符块，如控制字符、私用区等。

**2.3、如何判断一个字符属于哪个UnicodeBlock？**

```Java
public static UnicodeBlock of(char c) {
    return of((int)c);
}
```

**2.4、输入一个字符串，计算字符串中字符个数、汉字个数、空格个数、其它字符个数？**

**`原理分析`**：

-  中文汉字判断：
   是否属于中日韩越统一表意文字`(CJK_UNIFIED_IDEOGRAPHS)`，若是,则代表是中文

-  英文字母判断：
   是否属于小写字母的ASCII值范围：`[97,122]`，或者大写字母的ASCII值范围：`[65,90]`

-  空格判断：
   是否等于空格对应的`ASCII=32`
-  数字判断：
   是否属于数字对应的ASCII值范围：`[48,57]`

```java
public class Main {
    public static void main(String[] args) {
        Scanner scanner=new Scanner(System.in);
        String a=scanner.nextLine();
        char[] charArrays=a.toCharArray();
        //UnicodeBlock是Java中的一个枚举类型，用于表示Unicode字符的块范围。在这段代码中，ub被赋值为null，表示还没有指定具体的Unicode块范围。
        Character.UnicodeBlock ub=null;
        int chineseCount=0,englishCount=0,blankCount=0,numberCount=0,otherCharacterCount=0;
        for(int i=0;i<charArrays.length;i++){
            if(Character.UnicodeBlock.of(charArrays[i])==Character.UnicodeBlock.CJK_UNIFIED_IDEOGRAPHS){
                chineseCount++;
                continue;
            }
            if((charArrays[i]>=65&&charArrays[i]<=90)||(charArrays[i]>=97&&charArrays[i]<=122)){
                englishCount++;
                continue;
            }
            if(charArrays[i]==32){
                blankCount++;
                continue;
            }
            if((charArrays[i]>=48&&charArrays[i]<=57)){
                numberCount++;
                continue;
            }
            otherCharacterCount++;
        }
        System.out.println("中文汉字的个数为: "+chineseCount);
        System.out.println("英文字母的个数为: "+englishCount);
        System.out.println("空格的个数为: "+blankCount);
        System.out.println("数字的个数为: "+numberCount);
        System.out.println("其他字符的个数为: "+otherCharacterCount);
    }
}
```





## 3、<span style="color:brown">Java内存分布概述：</span>🛺🛺🛺

**3.1、Java内存：**

- 栈`(Stack)`：

  - <font color="red">**方法中的局部变量**，存放在栈中</font>；

  - <font color="#0099ff">方法是要在栈中运行</font>；
  - 局部变量：方法中的参数或者方法｛｝内部的变量；
  - 作用域：一旦超出作用域，立刻就会从内存中释放；
- 堆`(Heap)`：

  - <font color="red">**数组和new的实例对象**，都存放在Heap中</font>；

  - <font color="orange">**字符串常量池也存在Heap中**</font>；【`JDK1.7`之后】
  - <u>方法中的局部变量使用 final 修饰后，放在堆中</u>；
  - 堆内存中的东西都有一个<font color="#0099">**地址值**</font>：16进制；
  - 堆内存中的数据都有一个默认值，规则：
    - 如果是整数					默认为0；
    - 如果是浮点数				默认为0.0；
    - 如果是字符					默认为'\u0000'；
    - 如果是布尔值				默认为false；
    - 如果是引用类型			默认为null；
- 方法区`(Method Area)`：
  - <font color="#0099ff">**存储  .class文件**</font>；
  - Class文件中，除了有<u>类版本、字段、方法、接口</u>等信息，还包含了**常量池，用于存放<u>*<span style="color:green">编译期生成的各种字面值和符号</span>*</u>**。
- 本地方法栈`(Native Method Stack)`：与操作系统有关；
- 程序计数器`(pc Register)`：与CPU相关；

**3.2、heap 和 stack 的区别？**

> **Java 的内存分为两类，一类是栈内存，一类是堆内存**。

- 栈内存：

  - 程序进入一个方法 时，会为这个方法单独分配一块私属存储空间，用于存储这个方法内部的局部变量；

  - 当这个方法结束时，分配给这个方法的栈会释放，这个栈中的变量也将随之释放；

- 堆内存：

  堆是与栈作用不同的内存，一般用于存放不放在当前方法栈中的那些数据，<u>**它不会随方法的结束而消失**</u>；



## 4、<span style="color:brown"> 定义一个标准类：</span>

1. 所有的成员变量全部使用<span style='color:blue'>private关键词私有化</span>；
2. 为每一个成员变量编写<span style='color:blue'>一对Getter和Setter</span>；
3. 编写一个<span style='color:blue'>无参数构造方法</span>；
4. 编写一个<span style='color:blue'>有参数构造方法</span>；

```java
public class Student {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

```

```java
public class DemoClass {
    public static void main(String[] args) {
        Student stu1 = new Student();
        stu1.setName("Java");
        stu1.setAge(120);
        System.out.println("姓名"+stu1.getName()+"，年龄"+stu1.getAge());
        System.out.println("=============");
        Student stu2 = new Student("Python",150);
        System.out.println("姓名"+stu2.getName()+"，年龄"+stu2.getAge());
    }
}

```



