## 1、<span style="color:brown">打印流：</span>

**1.1、概述：**

1. 平时控制台的打印数据，调用的时`print和println方法`；
2. print和println方法，主要来自于java.io.PrintStream类；
3. `java.io.PrintStream  extends  OutputStream`；

**1.2、特点：**

1. 只负责数据输出，不负责数据读取；
2. 永远不会抛出 IOException异常；



## 2、<span style="color:brown">主要内容：</span>

**2.1、构造方法：**

```java
PrintStream(String filename)
    使用指定的文件名，创建一个打印流;
```

```java
PrintStream(File file)
```

```java
PrintStream(OutputStream out)
```

**2.2、特有成员方法：**

```java
print(任意类型的数据)
println(任意类型的数据+换行)
```

**2.3、共性成员方法：**

```java
void write(int b)
```

```java
void write(byte[] b)
```

```java
void write(byte[] b, int offset, int len)
```

```java
void flush()
```

```java
void close()
```

**<span style="color:orange">2.4、注意事项：</span>**

1. 如果使用继承自父类的write方法，那么查询数据时会自动查询编码表：97 --> a；
2. 如果使用自己的特有方法：print、println，写的数据原样输出：97 --> 97；



## 3、<span style="color:brown">范例：</span>

**3.1、测试PrintStream类中的各方法：**

```java
public class Demo05 {
    public static void main(String[] args) throws FileNotFoundException {
        PrintStream ps = new PrintStream("D:\\JavaCode\\study_code\\start_code\\Learning\\print.txt");
        ps.write(97);//a
        ps.println(986);//986
        ps.println("Hello");//Hello
        ps.close();
    }
}
```

**3.2、改变输出语句的目的地：**

<u>*使用System.setOut方法，把语句的输出路径改变为参数打印流路径*</u>：

`public static void setOut(PrintStream ps)`

```java
public class Demo05 {
    public static void main(String[] args) throws FileNotFoundException {
        System.out.println("控制台输出！！");//控制台输出！！【控制台中查看】
        PrintStream ps = new PrintStream("D:\\JavaCode\\study_code\\start_code\\Learning\\change.txt");
        System.setOut(ps);
        System.out.println("在打印流路径输出！！！");//在打印流路径输出！！！【此处在change.txt文档中查看】
        ps.close();
    }
}
```