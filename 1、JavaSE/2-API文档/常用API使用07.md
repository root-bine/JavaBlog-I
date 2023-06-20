## 1、<span style="color:brown">**Date类：**</span>Java.util

- **Date类的介绍:**

  1. <span style="color:blue">**表示日期和时间的类**</span>；
  2. <span style="color:green">**毫秒值的作用：可以对时间和日期进行计算**</span>；
  
- **构造方法：**

  1. <font color="red">**Date（）获取的就是当前系统的时间和日期**</font>；

     ```java
     public class Demo01 {
         public static void main(String[] args) {
             demo01();
         }
         private static void demo01() {
             Date date = new Date();
             System.out.println(date);
         }
     }
     ```
  
  2. <font color="red">**Date（Long date）传递毫秒值，把毫秒转换为Date日期**</font>；
  
     ```java
     public class Demo01 {
         public static void main(String[] args) {
             demo01();
         }
         private static void demo01() {
             Date date = new Date(0L);
             System.out.println(date);
         }
     }
     ```
  
- **Date类的成员方法：**

  <font color="orange">**public  long  getTime (  )：把日期转换成毫秒值**</font>;

  ```java
  public class Demo01 {
      public static void main(String[] args) {
          demo01();
          demo02();
      }
      private static void demo02() {
          Date d1 = new Date();
          long time = d1.getTime();
          System.out.println(time);
      }
      private static void demo01() {
          Date date = new Date(0L);
          System.out.println(date);
      }
  }
  ```


## 2、<span style="color:brown">**DateFormat类：**</span>Java.text

## <span style="color:purple">**注意：DateFormat类是一个抽象类，无法直接创建对象使用其中的成员方法，因此选择使用它的子类：SimpleDateFormat类！！！！**</span>

- **DateFormat类的介绍：**
  1. <span style="color:red">**日期/时间格式化子类的抽象类**</span>;
  2. <span style="color:orange">**主要展示的结果不是Date类展示的纯英文日期和时间**</span>，<span style="color:green">**DateFormat类展示的是我们现实当中的时间和日期展示方式**</span>;

- **DateFormat类的作用：**
  1. 格式化：Date日期------------>String类型文本；
  2. 解析：文本-------------->Date日期；
  3. 标准化：将得到的日期按照一种输出模式输出出来；

- **DateFormat类的成员方法：**
  1. public   String   format  ( Date   date )------->格式化
     - <span style="color:red">**按照指定模式，把Date日期格式化为符合模式的字符串**</span>；
  2. public   Date   parse  ( String   sourse )-------->解析
     - <span style="color:red">**把符合模式的字符串解析成为Date日期**</span>；



## 3、<span style="color:orange">***SimpleDateFormat类：***</span>Java.test

### <!--SimpleDateFromat具体代替DateFormat类的使用过程-->

- **构造方法：**

  SimpleDateFormat( String   pattern )

  - 用给定的模式和默认语言环境的日期格式构造SimpleDateFormat

  - <span style="color:red">**String   pattern----------->传递指定的模式**</span>

  - <span style="color:red">**模式区分大小写**</span>，且写对应的模式会把模式替换成对应的日期和时间

    ***"yyyy-MM-dd  HH:mm:ss" 或者 "yyyy年MM月dd日HH时mm分ss秒"***

- **具体使用：**
  
  1. 使用SimpleDateFormat类创建对象；
  2. 使用Date类构造方法来获取或者转化时间和日期；
  3. 使用SimpleDateFormat类创建对象调用：public   String   format( Date date );
  
  4. 使用SimpleDateFormat类创建对象调用：public   Date   parse( String   sourse );
     - <span style="color:red">**由于parse方法具有异常**</span>，所以需要抛出异常：try--------catch--------或者   throws  Exception; 
  
- **题目：**<span style="color:red">**请使用时间与日期的API，计算出一个人已经出生了多少天？**</span>

  ```java
  public class Demo04 {
      public static void main(String[] args) throws Exception{
          //获取出生日期
          System.out.println("请输入出生日期,格式为:yyyy-MM-dd");
          Scanner sc = new Scanner(System.in);
          String str = sc.nextLine();
          //将出生日期解析成为Date格式日期
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
          Date date = sdf.parse(str);
          //将出生日期的Date格式转换成毫秒值
          long time = date.getTime();
          //将今天日期转换成毫秒值
          Date date1 = new Date();
          long todaytime = date1.getTime();
          //计算出出生多少天
          long texttime = todaytime-time;
          System.out.println(texttime/1000/60/60/24);
      }
  }
  ```
  
  

