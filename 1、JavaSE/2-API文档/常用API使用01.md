## 1、<span style='color:brown'>Scanner类：</span>java.util

**1.1、Scanner介绍：**

定义格式为：`Scanner 对象名 = new Scanner(System.in)`，一般在输入结束后，使用<u>`对象名.close()`</u>释放资源。

**1.2、Scanner详解：**

> <span style='color:red'>next()不能读取带有空白的单词，而nextLine()则可以</span>

- next()：读取输入的下一个单词；

- hasNext()：检测输入中是否还有其他单词；

- nextLine()：读取下一行内容；

- hasNextLine()：检测是否还有下一个表示字符串的序列；


<span style='color:green'>后续的nextInt()、nextDouble()、nextByte()、hasNextInt()、hasNextDouble()、hasNextByte()等类似于上述作用及功能。</span>

```java
public class Demo02_hasNexts {
    public static void main(String[] args) {
        //创建一个扫描器对象，用于接收键盘数据
        Scanner scanner = new Scanner(System.in);
        //判断是否有数据输入
        if(scanner.hasNextLine()){
            String str=scanner.nextLine();
            System.out.println();
        }
        //结束运行，释放空间
        scanner.close();
    }
}
```

**1.3、处理换行符：**

​	<span style='color:brown'>在读取完**整数**或**其他类型**的输入后，通常会留下一个换行符在输入缓冲区中</span>，这会造成后续内容输入为null值。为了避免这个问题，可以在读取完整数后使用`nextLine()`来消耗掉换行符！！！

```java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int number = scanner.nextInt();
        // 消耗掉换行符
        scanner.nextLine();
        
        String text = scanner.nextLine();
        System.out.println("整数: " + number);
        System.out.println("字符串: " + text);
        scanner.close();
    }
}
```



## 2、<span style='color:brown'>匿名对象</span>

**2.1、格式：**

> <span style='color:red'>**匿名对象只能使用唯一的一次，下次使用就是另外一个新的对象**</span>

<u>`new 类名称()`</u>

**2.2、演示：**

```java
public class Person {
    String name;
    public void showName(String name){
        System.out.println("你好，"+name+"!我叫"+this.name);
    }
}
```

```java
public class Demo04 {
    public static void main(String[] args) {
        Person one = new Person();
        one.name="java";			//Java
        one.showName("李明");        //你好李明！我叫java
        System.out.println("======匿名对象======");
        new Person().name="Python";		//Python
        new Person().showName("咸鱼");        //你好咸鱼！我叫null
    }
}
```

**2.3、匿名对象作为方法的参数和返回值：**

```java
public class Demo05 {
    public static void main(String[] args) {
        //普通方式
//        Scanner sc = new Scanner(System.in);
//        int num=sc.nextInt();

        //匿名对象方式
//        int num = new Scanner(System.in).nextInt();
//        System.out.println("输入的是:"+num);

        //作为方法参数
//        Scanner sc = new Scanner(System.in);
//        methodParam(sc);

        //使用匿名对象写入参数
        methodParam(new Scanner(System.in));
    }
    public static void methodParam(Scanner sc){
        int num=sc.nextInt();
        System.out.println("输入是:"+num);
    }
}
```

```java
public class Demo06 {
    public static void main(String[] args) {
        Scanner sc = methodParam();
        int num=sc.nextInt();
        System.out.println("输入是:"+num);
    }
    public static Scanner methodParam(){
        return new Scanner(System.in);
    }
}
```



## 3、<span style='color:brown'>Character：</span>java.lang

**3.1、概述：**

​	`Character类`通常用于<u>*处理单个字符的操作*</u>，提供了许多有用的<span style='color:red'>**<u>静态方法</u>**</span>和<span style='color:green'>**<u>常量</u>**</span>来操作字符数据。创建**<u>Character对象</u>**有两种方式，其中一种在JDK9之后已标记为过时，另一种较为常用：

```java
// 第1种[不推荐]
Character ch = new Character(char c);
// 第2种
Character ch = 'A';
// 第3种
Character ch = Character.valueOf(char c);
```

**3.2、常用方法：**

- 判断字符是否为数字

  ```java
  public static boolean isDigit(char ch)
  ```

- 判断字符是否为字母

  ```java
  public static boolean isLetter(char ch)
  ```

- 判断字符是否为<u>**大写**或**小写**字母</u>

  ```java
  public static boolean isUpperCase(char ch)
  ```

  ```java
  public static boolean isLowerCase(char ch)
  ```

- 将字符转换为<u>**大写**或**小写**形式</u>

  ```java
  public static char toUpperCase(char ch)
  ```

  ```java
  public static char toLowerCase(char ch)
  ```

- 判断字符是否为空白字符（空格、制表符等）

  ```java
  public static boolean isWhitespace(char ch)
  ```

-  获取字符的数值表示（如果字符不是数字，则返回`-1`）

  ```java
  public static int getNumericValue(char ch)
  ```

**3.3、UnicodeBlock类：**🎟️🎟️🎟️

`java.lang.Character.UnicodeBlock` 类，是`Character`类中的静态内部类：

```java
public static final class UnicodeBlock extends Subset {}
```

这个类主要用于处理**<u>Unicode字符块</u>**相关内容，涉及***Unicode编码***：

|   ASCII编码是   | 最早的字符编码标准            | 7位二进制数（共128个编码）        |
| :-------------: | ----------------------------- | --------------------------------- |
| **Unicode编码** | **全球通用的字符编码标准**    | **16位二进制数（共65536个编码）** |
|  **UTF-8编码**  | **一种变长的Unicode编码方式** | **UTF-8编码可以兼容ASCII编码**    |

在`Java`中，使用**`Unicode编码`**表示<u>字符</u>，格式：`\u`+`4位16进制数`。
