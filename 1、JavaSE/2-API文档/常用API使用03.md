## 1、<span style='color:brown'>String 类：</span>java.lang

### <!--String类重写了hashCode()、equals()、toString()三个方法-->

**1.1、概述：**

- Java程序中的<u>所有字符串的***字面值***</u> 都是作为<u>此类的实例</u>来实现，即：`String str = "Hello world"`；
- <font color="red">**字符串内容是不可变的**</font>，但字符串可以<font color="green">**共享的**</font>；
- <font color="orange">`String`类在Java中是被`final`修饰的，即`String`类不能被继承</font>。这是由于需要考虑**字符串的不可变性**！！！

**1.2、字符串的加减：**

```java
String str1 = "9"  int num = 6   String str2 = "2"
str1 + str2 // "92"
str1 - str2 // 7
str1 - num // 3
str2 + num // "26"
```

**1.3、创建String字符串方法：**

- `public  String( )`，创建一个空白字符串；

- `public  String(char[ ] array)`，根据字符数组，创建对应的字符串；

- `public  String(char[ ] array, int offset, int count)`，offset：起始位置索引，count：读取的个数；

- `public  String(byte[ ] array)`,   根据字节数组，创建对应的字符串；

- `public  String(byte[ ] array, int offset, int count)`，offset：起始位置索引，count：读取的个数；

- 直接调用：`String  str = "Hello"`；

  ```java
  public class DemoString01 {
      public static void main(String[] args) {
          //第一种构造方法创建
          String str1 = new String();
          System.out.println("第一中构造方法创建:"+str1);
          //第二种构造方法创建
          char [] charArray = {'A','B','C'};
          String str2 = new String(charArray);
          System.out.println("第二种构造方法创建:"+str2);
          //第二种构造方法创建
          byte [] byteArray = {'0','1','6'};
          String str3 = new String(byteArray);
          System.out.println("第二种构造方法创建:"+str3);
          //直接创建
          String str = "Hello";
          System.out.println("第三种构造方法创建:"+str);
      }
  }
  
  ```
  

**1.4、字符串的常量池:**

> JDK1.6在方法区中，JDK1.7以后在堆中

```java
public class DemoString02 {
    public static void main(String[] args) {
        String str1 = "a";
        String str2 = "b";
		// 根据JVM串池的变量和常量拼接的底层原理
        char[] charArray = {'a','b'};
        String str3 = new String(charArray);
		String str4 = "ab";
        String str5 = s1 + s2;
        System.out.println(str1 == str2);//true
        System.out.println(str1 == str3);//false
        System.out.println(str4 == str5);//false
    }
}
```

`String a = "abc"`与`new String("abc")`的创建过程？🎈🎈🎈

```scss
1. 'JVM会使用常量池来管理字符串直接量'. 在执行这句话时, JVM会先检查常量池中是否已经存有"abc", 若没有则将"abc"存入常量池, 否则就复用常量池中已有的"abc", 将其引用赋值给变量a.
2. JVM会先使用常量池来管理字符串直接量, 即将"abc"存入常量池. 然后再创建一个新的String对象, 这个对象会被保存在堆内存中. 并且, 堆中对象的数据会指向常量池中的直接量.
```

**1.5、常用比较方法：**5个

1. `boolean equals(Object  obj)`：在判断时区分大小写
2. `boolean equalsIgnoreCase(Object  obj)`：在判断时不区分大小写
   - 参数可以是任何对象；
   - 只有参数是一个字符串，并且内容相同才会返回true，否则为false；
   - 任何对象都可以用Object进行接收；
   - <span style='color:orange'>**“==”在引用类型中比较的是地址值，而比较内容就需要使用equals( )方法；**</span>
3. `boolean stratsWith(String prefix, int offset)`
   - 比较字符串是否是以【prefix】为前缀；
   - 而【offset】表示的是开始查找位置；
4. `boolean endsWith(String prefix, int offset)`
   - 判断字符串是否以prefix为后缀；
5. `boolean contains(String str)`
   - 判断字符串是否包含某个字符串；

**1.6、常用获取方法：**5个

- `int length()`
  - 获取字符串中的字符个数，拿到字符串长度；
- `String concat(String  str)`
  - 将当前字符串和参数字符串拼接成一个新的字符串，并返回；
  - <span style='color:red'>**此方法只是单纯的拼接字符串，并不是改变了字符串的内容;**</span>
  - <span style='color:red'>**字符串的内容是不可以改变的;**</span>
- `char charAt(int  index)`
  - 获取索引位置的单个字符  (索引从0开始)；
- `int indexOf(String  str)`
  - 查找参数字符串在本字符串当中首次出现的索引位置；
  - 如果没有返回-1；
- `String intern()`
  - 如果池中已经包含一个等于该String对象的字符串（由equals(Object)方法确定），则返回池中的字符串；
  - 否则，将此String对象添加到池中并返回对该String对象的引用；

**1.7、截取方法：**2个

1. `String substring(int  index)`

   - 截取从参数位置开始到字符串结尾处，并返回一个新的字符串；

2. `String subsring(int begin, int end)`

   - <span style='color:violet'>**截取一个范围：[ begin，end )，从begin位置开始，到end-1位置处结束**</span>；
   - 结果返回一个新的字符串；

   ```java
   public class Demo06 {
       public static void main(String []args){
           String str1 = "Hello World,";
           String str2 = "Java,Python";
           String s1 = str1.substring(6);
           String s2 = str2.substring(0,4);
           System.out.println(s1+s2);//World,Java
       }
   }
   ```

**1.8、转换方法：**5个

1. `char[] toCharArray()`
   - 将字符串转换成字符数组；
   
2. `byte[] getBytes()`
   - 获取字符串底层的字节数组；
   
3. `String replace(CharSequence  oldString ,CharSequence  newString)`
   - 将所有出现的旧字符串替换成新的字符串，并返回新字符串；
   - <span style='color:orange'>**CharSequence  :  可以接受字符串类型**</span>；


  4. `String toLowerCase()`
     - 将字符串转换成小写格式
  5. `String toUpperCase()`
     - 将字符串转换成大写格式

**1.9、分割方法：**1个

`String[] split (String regex)`

- 按照参数的规则，把字符串分割成若干部分
- <span style='color:violet'>**String  regex  :  分割目标，以该目标为分界线来分割字符串，也是正则表达式。但是如果要以英语句点" . "作为分割，必须改写成为" \\\\. "**</span>

```java
public class Demo07 {
    public static void main(String []args){
        System.out.println("正确演示：");
        String str1 = "aaa,bbb,ccc";
        String[] split1 = str1.split(",");
        for (int i = 0; i < split1.length; i++) {
            System.out.println(split1[i]);
        }
        String str2 = "aaa bbb ccc";
        String[] split2 = str2.split(" ");
        for (int i = 0; i < split2.length; i++) {
            System.out.println(split2[i]);
        }
        System.out.println("英语句号错误演示：");
        String str3 = "aaa.bbb.ccc";
        String[] split3 = str3.split(".");
        System.out.println(split3.length);
        for (int i = 0; i < split3.length; i++) {
            System.out.println("结果:"+split3[i]);
        }
        System.out.println("英语句号正确演示：");
        String str4 = "aaa.bbb.ccc";
        String[] split4 = str4.split("\\.");
        System.out.println(split4.length);
        for (int i = 0; i < split4.length; i++) {
            System.out.println("结果:"+split4[i]);
        }
    }
}
```



## 2、<span style="color:brown">**StringBuilder类：**</span>Java.lang

**2.1、String、StringBuffer、StringBuilder的区别：**

![image-20221027201155435](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/String%E3%80%81StringBuilder%E3%80%81StringBuffer.png)

**2.2、StringBuilder底层：**

1. StringBuilder和StringBuffer都可作为<span style="color:red">**字符缓冲区**</span>，可以提高字符串的操作效率；
2. <span style="color:orange">**底层是一个char[]，且长度可以变化**</span>；
3. <span style="color:violet">StringBuilder类的初始化容量是**16字符**，若超出该范围，会自动进行扩容</span>；

**2.3、构造方法：**

1. `StringBuilder()`
   - 构造一个空的StringBuilder容器；
2. `StringBuilder(String str)`
   - 构造一个StringBuilder容器，并将字符串添加进去；
   - <font color="red">**String对象 -------> StringBuilder对象**</font>；

```java
public class Demo01 {
    public static void main(String[] args) {
        StringBuilder bu1 = new StringBuilder();
        System.out.println("bu1:"+bu1);
        StringBuilder bu2 = new StringBuilder("Java");
        System.out.println("bu2:"+bu2);
    }
}
```

**2.4、成员方法：**

1. `StringBuilder append( ...... )`------->链式编程方法！！！！

   - 参数：<span style="color:red">**可以同时添加任意数据类型的数据**</span>；
   - 返回当前对象自身；
   - 为StringBuilder添加数据的作用；

   ```java
   public class Demo02 {
       public static void main(String[] args) {
           StringBuilder bu1 = new StringBuilder();
           //StringBuilder bu2 = bu1.append("abc");
           //abc
           //System.out.println(bu1);
           //abc
           //System.out.println(bu2);
           //两个对象的地址值相同
           //true
           //System.out.println(bu1==bu2);
           //根据代码理解，后续使用append（）方法无需接受返回值
           //bu1.append(1);
           //bu1.append('C');
           //bu1.append(true);
           //bu1.append(5.2);
           //1Ctrue5.2
           //System.out.println(bu1);
           /*
               链式编程：方法的返回值是一个对象，可以根据对象继续调用方法
            */
           bu1.append("abc").append(123).append(5.3).append(true);
           //abc1235.3true
           System.out.println(bu1);
       }
   }
   ```

2. `String toString()`

   - 将StringBuilder对象转换成String对象；

   ```java
   public class Demo03 {
       public static void main(String[] args) {
           //String ------> StringBuilder
           String str = "hello";
           StringBuilder bu1 = new StringBuilder(str);
           System.out.println("str:"+bu1);
           //StringBuilder ------> String
           StringBuilder bu2 = new StringBuilder(str);
           bu2.append(" world");
           System.out.println("bu2:"+bu2);
           String str1 = bu2.toString();
           System.out.println(str1);
       }
   }
   ```

3. `StringBuilder reverse()`

   - 将传入StringBuilder的数据，**倒序排列**；

   ```java
   public class demo {
       public static void main(String []args){
           StringBuilder stringBuilder = new StringBuilder();
           stringBuilder.append(1).append(2).append("nihao");
           StringBuilder reverse = stringBuilder.reverse();
           System.out.println(reverse);//oahin21
       }
   }
   ```

