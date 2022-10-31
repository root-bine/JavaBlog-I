# 常用API使用03

## 1、<span style='color:brown'>String 类：</span>java.lang、被final修饰

- **概述：**
  - Java程序中的所有字符串的***字面值*** 都是作为此类的实例来实现，即：<font color="red">**凡事双引号的String字符串都是String类的对象(没有new，也照样是对象)**</font>
  - 由于<font color="red">**字符串内容是不可变的**</font>，则字符串可以<font color="blue">**共享的**</font>

- **创建String字符串方法：**

  - <span style='color:orange'>**六种构造方法:**</span>

    - public  String( )，创建一个空白字符串【无内容】；

    - public  String(char [ ] array)，根据字符数组，创建对应的字符串；

    - `public  String(char [ ] array，int offset，int  count)，offset:起始位置索引，count：读取的个数`

    - public  String(byte [ ] array),   根据字节数组，创建对应的字符串；

    - `public  String( byte[ ] array，int offset，int  count)，offset:起始位置索引，count：读取的个数`
    
    - 直接调用：String  str = "Hello";
    
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
      
      ```
      第一中构造方法创建:
      第二种构造方法创建:ABC
      第二种构造方法创建:016
      第三种构造方法创建:Hello
      ```
  
- **字符串的常量池:**

  ```java
  public class DemoString02 {
      public static void main(String[] args) {
          String str1 = "abc";
          String str2 = "abc";
  
          char [] charArray = {'a','b','c'};
          String str3 = new String(charArray);
  
          System.out.println(str1 == str2);
          System.out.println(str1 == str3);
          System.out.println(str2 == str3);
      }
  }
  ```
  
  ```
  true
  false
  false
  ```
  
  - <span style='color:orange'>**程序中直接写上双引号的字符串，就在字符串的常量池中；**</span>
  - <span style='color:green'>**字符串常量池在Heap中；**</span>
  - <span style='color:red'>**"=="在**引用类型*** 和*** 基本类型**的区别：**</span>
    - 基本类型：数值间的比较；
    - 引用类型：地址值的比较；
  
- **字符串的常用比较方法：**

  1. `public  boolean  equals(Object  obj)`：在判断时区分大小写
  2. `public  boolean  equalsIgnoreCase(Object  obj)`：在判断时不区分大小写
     - 参数可以是任何对象；
     - 只有参数是一个字符串，并且内容相同才会返回true，否则为false；
     - 任何对象都可以用Object进行接收；
     - <span style='color:orange'>**“==”在引用类型中比较的是地址值，而比较内容就需要使用equals( )方法；**</span>
  3. `public  boolean  stratsWith(String prefix, int offset)`
     - 比较字符串是否是以【prefix】为前缀；
     - 而【offset】表示的是开始查找位置；
  4. `public boolean  endsWith(String prefix, int offset)`
     - 判断字符串是否以prefix为后缀
  5. `public native String intern()`
     - 如果池中已经包含一个等于该String对象的字符串，由equals(Object)方法确定，则返回池中的字符串；
     - 否则，将此String对象添加到池中并返回对该String对象的引用；
  
- **字符串的常用获取方法：**

  - `public  int  length()`
    - 获取字符串中的字符个数，拿到字符串长度；
  - `public  String  concat (String  str)`
    - 将当前字符串和参数字符串拼接成一个新的字符串，并返回；
    - <span style='color:red'>**此方法只是单纯的拼接字符串，并不是改变了字符串的内容;**</span>
    - <span style='color:red'>**字符串的内容是不可以改变的;**</span>
  - `public  char  charAt (int  index)`
    - 获取索引位置的单个字符  (索引从0开始)；
  - `public  int  indexOf (String  str)`
    - 查找参数字符串在本字符串当中首次出现的索引位置；
    - 如果没有返回-1；
  
- **字符串中的截取方法：**

  1. `public  String  substring( int  index )`

     - 截取从参数位置开始到字符串结尾处，并返回一个新的字符串；

  2. `public  String  subsring( int  begin , int  end )`

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

- **字符串中的转换方法：**

  1. `public  char[ ]  toCharArray( )`
     - 将字符串转换成字符数组；
     
  2. public  byte[ ] getBytes( )
  
     - 获取字符串底层的字节数组；
  
  3. `public  String  replace ( CharSequence  oldString , CharSequence  newString )`
     - 将所有出现的旧字符串替换成新的字符串，并返回新字符串；
     - <span style='color:orange'>**CharSequence  :  可以接受字符串类型**</span>；
  
  
  4. `public  String  toLowerCase( )`
     - 将字符串转换成小写格式
  5. `public  String  toUpperCase( )`
     - 将字符串转换成大写格式
  6. `public  boolean  contains(String   str)`
     - 判断字符串是否包含某个字符串；
  
- **字符串中分割字符串的方法：**

  1. `public  String[  ]  split ( String regex )`

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

     ```java
     正确演示：
     aaa
     bbb
     ccc
     aaa
     bbb
     ccc
     英语句号错误演示：
     0
     英语句号正确演示：
     3
     结果:aaa
     结果:bbb
     结果:ccc
     ```

- 题目1：定义一个方法，使数组{1，2，3}按指定格式拼接成新的字符串，格式例如：[word1#word2#word3]

  ```java
  /*
  * 定义方法三要素：
  * 方法名：toArray
  * 参数列表：array[]
  * 返回值类型：String
  * */
  public class Demo08 {
      public static void main(String[] args) {
          int []array = {1,2,3};
          String s = toArray(array);
          System.out.println(s);
      }
      public static String toArray(int array[]){
          String str = "[";
          for (int i = 0; i < array.length; i++) {
              if(i == array.length-1){
                  str+="word"+array[i]+"]";
              }else{
                  str+="word"+array[i]+"#";
              }
          }
          return str;
      }
  }
  ```

  ```xml
  [word1#word2#word3]
  ```

  
