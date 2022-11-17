## 1、<span style="color:brown">**Calendar类：**</span>Java.util

## <span style="color:purple">**Calendar类中的许多方法比Date类的方法更为方便**</span>

## <span style="color:red">**Calendar类是一个抽象类，无法直接创建对象**</span>

- ***创建对象：***

  - <span style="color:green">**public   static   Calendar   getInstance( )**</span>
  - 返回了Calendar类的子类对象
    
  - 获取日历类对象
  
  ```java
  public class Demo05 {
      public static void main(String[] args) {
          Calendar c = Calendar.getInstance();
          System.out.println(c);
      }
  }
  ```
  
- ***成员方法：***

  1. `public int get(int field)`  

     - 获取给定日历字段的值；
     - 参数：传递指定的日历字段（YEAR、DATE.......）；

     ```java
     public class Demo06 {
         public static void main(String[] args) {
             demo06();
         }
         private static void demo06() {
             //使用getInstance()获取Calender对象
             Calendar c = Calendar.getInstance();
             //获取年的日历字段值
             //系统按照的是西方计时算法：月份0-11月
             int year = c.get(Calendar.YEAR);
             System.out.println(year);
             //获取月的日历字段值
             int month = c.get(Calendar.MONTH);
             System.out.println(month);
             //获取日的日历字段值
             int day = c.get(Calendar.DAY_OF_MONTH);
             int day1 = c.get(Calendar.DATE);
             System.out.println(day);
             System.out.println(day1);
         }
     }
2. `public void set(int field, int value)`
  
   -  将指定日历字段设置为指定值；
   
     ```java
     public class Demo07 {
         public static void main(String[] args) {
             demo07();
         }
         private static void demo07() {
             //使用getInstance()获取Calender对象
             Calendar c = Calendar.getInstance();
             //将年设置成9999
             c.set(Calendar.YEAR,9999);
             //将月设置成9
             c.set(Calendar.DAY_OF_MONTH,9);
             c.set(Calendar.MONTH,9);
             //将日设置成9
             c.set(Calendar.DATE,9);
             //同时修改年月日
             //c.set(8888,8,8);
             //获取年的日历字段值
             int year = c.get(Calendar.YEAR);
             System.out.println(year);
             //获取月的日历字段值
             int month = c.get(Calendar.MONTH);
             System.out.println(month);
             //获取日的日历字段值
             int day = c.get(Calendar.DAY_OF_MONTH);
             int day1 = c.get(Calendar.DATE);
             System.out.println(day);
             System.out.println(day1);
     
         }
     }
     ```
     
   
  3. `public abstract void add(int field, int value)` 

     - 将指定日历字段<u>增加/减少</u>指定值；
     
       ```java
       public class Demo08 {
           public static void main(String[] args) {
               demo08();
           }
           private static void demo08() {
               //使用getInstance()获取Calender对象
               Calendar c = Calendar.getInstance();
               //把年增加2年
               c.add(Calendar.YEAR,2);
               //把月减少5月
               c.add(Calendar.MONTH,-5);
               //把日增加5天
               c.add(Calendar.DATE,5);
               //获取年的日历字段值
               int year = c.get(Calendar.YEAR);
               System.out.println(year);
               //获取月的日历字段值
               int month = c.get(Calendar.MONTH);
               System.out.println(month);
               //获取日的日历字段值
               int day = c.get(Calendar.DAY_OF_MONTH);
               int day1 = c.get(Calendar.DATE);
               System.out.println(day);
               System.out.println(day1);
           }
       }
       ```
     
  4. `public Date getTime()`
     
     - 返回一个Calendar时间值的Date对象；
     
     - 把日历对象转换成Date对象；
     
       ```java
       public class Demo09 {
           public static void main(String[] args) {
               demo09();
           }
           private static void demo09() {
               Calendar c = Calendar.getInstance();
               Date date = c.getTime();
               System.out.println(date);
           }
       }
       ```

- ***静态成员变量：***

  	public  static   final   int   YEAR = 1;  // 年
  	
  	public  static   final   int   MONTH = 2;  // 月
  	
  	public  static   final   int   DATE = 5;  // 日
  	
  	public  static   final   int   DAY_OF_MONTH = 5;  // 月中某一天
  	
  	public  static   final   int   HOUR = 10;  // 时
  	
  	public  static   final   int   MINUTE = 12;  // 分
  	
  	public  static   final   int   SECOND = 13;  // 秒

## 2、<span style="color:brown">**System类：**</span>Java.lang

## <span style="color:orange">**System类直接使用类名调用方法即可，无需构造对象！！！！！**</span>

- **介绍：**

  1. 提供了大量的静态方法；
  2. 获取与系统相关信息或者系统操作；

- **常用方法：**

  1. `public static long currentTimeMillis()`
  
     - 返回毫秒值为单位的当前时间；
     - 获取当前系统的时间；
     - 用来测试程序的效率；
  
     ```java
     //验证for循环打印数字1-999所需要的时间（毫秒）
     public class Demo10 {
         public static void main(String[] args) {
             demo10();
         }
         private static void demo10() {
             //程序执行前，获取一次毫秒值
             long l = System.currentTimeMillis();
             for (int i = 1; i < 999; i++) {
                 System.out.println(i);
             }
             //程序执行后，获取一次毫秒值
             long e = System.currentTimeMillis();
             System.out.println("程序工号是:"+(e-l)+"毫秒");
         }
     }
     ```
  
  2. `public static native void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)`
  
     <!--Arrays类的copyOf()应用了该方法的原理-->
  
     - 将数组中指定的数据拷贝到另一个数组中；
     - src --------> 源数组
     - srcPos ---------> 源数组中的起始位置
     - dest ---------> 目标数组
     - destPos --------> 目标数组中的起始位置
     - length ----------> 要复制的数组元素的数量
  
     ```java
     //将src数组的前三个元素复制到dest数组的前三个位置上
     public class Demo11 {
         public static void main(String[] args) {
             demo11();
         }
         private static void demo11() {
             int [] src = {1,2,3,4,5};
             int [] dest = {6,7,8,9,10};
             System.out.println("复制前:"+ Arrays.toString(src));
             System.arraycopy(src,0,dest,0,3);
             System.out.println("复制后:"+Arrays.toString(dest));
         }
     }
     ```
  
  3. `public static void gc()`
  
     - 源码：
  
       ```java
       public static void gc() {
           Runtime.getRuntime().gc();
       }
       ```
  
     - 功能：
  
       ```scss
       # 在一个Java 程序中的执行是自动的, 不能强制执行
       1. 即使程序员能明确地判断出有一块内存已经无用了, 是应该回收的, 序员也不能强制垃圾收集器回收该内存块;
       2. 程序员唯一能做的就是'通过调用System.gc()方法':
              ---> 来"建议"垃圾收集器去进行垃圾回收处理，但其是否可以执行，什么时候执行却都是不可知的。
       ```
       
  
  4. `public native String intern()`
  
     - 如果池中已经包含一个等于该String对象的字符串，由equals(Object)方法确定，则返回池中的字符串；
     - 否则，将此String对象添加到池中并返回对该String对象的引用；
  

