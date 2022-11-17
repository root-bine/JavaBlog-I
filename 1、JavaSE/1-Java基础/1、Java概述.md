## <!--Java编程语言只有值传递，并没有引用传递-->

## 1、<span style="color:brown">基础内容：</span>

- 栈【Stack】：

  - <font color="red">**方法中的局部变量**，存放在栈中</font>；

  - <font color="#0099ff">方法是要在栈中运行</font>
  - 局部变量：方法中的参数或者方法｛｝内部的变量；
  - 作用域：一旦超出作用域，立刻就会从内存中释放；
- 堆【Heap】：

  - <font color="red">**数组和new的实例对象**，都存放在Heap中</font>；

  - <font color="orange">**字符串常量池也存在Heap中**</font>；【JDK1.7之后】
  - <u>方法中的局部 变量使用 final 修饰后，放在堆中</u>；
  - 堆内存中的东西都有一个<font color="#0099">**地址值**</font>：16进制；
  - 堆内存中的数据都有一个默认值，规则：
    - 如果是整数					默认为0；
    - 如果是浮点数				默认为0.0；
    - 如果是字符					默认为'\u0000'；
    - 如果是布尔值				默认为false；
    - 如果是引用类型			默认为null；
- 方法区【Method Area】：
  - <font color="#0099ff">**存储  .class文件**</font>；
  - Class文件中，除了有<u>类版本、字段、方法、接口</u>等信息，还包含了**常量池，用于存放<u>*<span style="color:green">编译期生成的各种字面值和符号</span>*</u>**。
- 本地方法栈【Native Method Stack】：与操作系统有关；
- 寄存器【pc Register】：与CPU相关；



## 2、<span style="color:brown"> heap 和 stack 的区别：</span>

**Java 的内存分为两类，一类是栈内存，一类是堆内存**。

- 栈内存：

  - 程序进入一个方法 时，会为这个方法单独分配一块私属存储空间，用于存储这个方法内部的局部变量；

  - 当这个方法结束时，分配给这个方法的栈会释放，这个栈中的变量也将随之释放；

- 堆内存：

  堆是与栈作用不同的内存，一般用于存放不放在当前方法栈中的那些数据，<u>**它不会随方法的结束而消失**</u>；



## 3、<span style="color:brown"> 定义一个标准类：</span>

- <span style='color:orange'>***格式：***</span>

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



## 4、<span style="color:brown"> Java部件组成：</span>

**3.1、Java三大版本：**

- JavaSE：标准版（桌面程序、控制台开发......）

- JavaME：嵌入式开发（手机、小家电......）

- JavaEE：E企业级开发（web端、服务器开发......）

**3.2、JDK ：**

> 简单说，就是 JDK 包含 JRE 包含 JVM

DK 即为 Java 开发工具包，包含编写 Java 程序所必须的编译、运行等开发工具以及 JRE。开发工具如：

- 用于编译 Java 程序的 javac 命令。
- 用于启动 JVM 运行 Java 程序的 Java 命令。
- 用于生成文档的 Javadoc 命令。
- 用于打包的 jar 命令等等。

**3.3、JRE：** 

> 简单说，就是 JRE 包含 JVM

JRE 即为 Java 运行环境，提供了运行 Java 应用程序所必须的软件环境，包含有 Java 虚拟机（JVM）和丰富的系统类库。系统类库即为 Java 提前封装好的功能类，只需拿来直接使用即可，可以大大的提高开发效率。

**3.4、JVM：**

JVM 即为 Java 虚拟机，提供了字节码文件(`.class`)的运行环境支持。