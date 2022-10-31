# 常用API使用09

## 1、<span style="color:brown">**StringBuilder类：**</span>Java.lang、被final修饰

![image-20221027201155435](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/String%E3%80%81StringBuilder%E3%80%81StringBuffer.png)

- ***String类与StringBuilder类的区别：***

  - String类：

    1. 字符串是常量，被关键词final修饰，不可改变；

  
  ​		   2. 进行字符串相加，占用空间多，且效率低下；
  
  - StringBuilder类：
    1. <span style="color:red">**字符缓冲区**</span>，可以提高字符串的操作效率；
    2. <span style="color:orange">**底层是一个数组，不被final修饰，且长度可以变化**</span>；
    3. <span style="color:violet">**StringBuilder类的初始化容量是16，若超出该范围，会自动进行扩容**</span>;
  
- ***构造方法：***

  1. public   StringBuilder( )
     - 构造一个空的StringBuilder容器；
  2. public   StringBuilder( String  str )
     - 构造一个StringBuilder容器，并将字符串添加进去；
     - <font color="red">**String对象 -------> StringBuilder对象**</font>;

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
  
- ***成员方法：***

  1. `public StringBuilder append( ...... )`------->链式编程方法！！！！
     
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
     
  2. `public String toString()`
     
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
     
  3. `public StringBuilder reverse()`
     
     - 将传入StringBuilder的数据，**倒序输出**；
     
     ```java
     public class demo {
         public static void main(String []args){
             StringBuilder stringBuilder = new StringBuilder();
             stringBuilder.append(1).append(2).append("nihao");
             StringBuilder reverse = stringBuilder.reverse();
             System.out.println(reverse);
         }
     }
     -----------------------------------------------------------------------------------------------------
     oahin21
     ```
