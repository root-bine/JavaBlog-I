## 1、<span style="color:brown">**Date类：**</span>Java.util

## Date类是一个抽象类

**1.1、概述：**

1. <span style="color:blue">**表示日期和时间的类**</span>；
2. <span style="color:green">**毫秒值的作用：可以对时间和日期进行计算**</span>；

**1.2、构造方法：**

- <font color="red">**`Date()`**</font>：获取的就是当前系统的时间和日期

  ```java
  public void demo01() {
  	Date date = new Date();
  	System.out.println(date);
  }
  ```

- <font color="red">**`Date(long time)`**</font>：传递毫秒值，把毫秒转换为Date日期

  ```java
  public void demo01() {
  	Date date = new Date(3000L);
  	System.out.println(date);
  }
  ```

**1.3、成员方法：**

<font color="orange">**`long  getTime()`**</font>：把日期转换成毫秒值

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

## <span style="color:purple">**DateFormat类是一个抽象类，SimpleDateFormat类 extends DateFormat！！！！**</span>

**2.1、概述：**

1. <span style="color:red">日期/时间格式化子类的**抽象类**</span>；
2. <span style="color:green">**DateFormat类展示的是现实当中的时间和日期展示方式**</span>；

**2.2、使用步骤：**

1. 格式化：Date日期——>String类型文本；
2. 解析：String文本——>Date日期；
3. 标准化：规范日期输出的格式；

**2.3、成员方法：**

- `String format(Date date)`：
  - <span style="color:green">**按照指定模式，把Date日期格式化为符合模式的字符串**</span>；

- `Date parse(String sourse)`：
  - <span style="color:red">**把符合模式的字符串解析成为Date日期**</span>；



## 3、<span style="color:orange">***SimpleDateFormat类：***</span>Java.test

### <!--SimpleDateFromat具体代替DateFormat类的使用过程-->

**3.1、构造方法：**

`SimpleDateFormat( String  pattern )`：

- 用给定的模式和默认语言环境的日期格式构造SimpleDateFormat；

- <span style="color:red">**String   pattern----------->传递指定的模式**</span>；

- <span style="color:red">**模式区分大小写**</span>，且写对应的模式会把模式替换成对应的日期和时间；

  ***"yyyy-MM-dd  HH:mm:ss" 或者 "yyyy年MM月dd日HH时mm分ss秒"***

**3.2、具体使用：**

1. 使用`SimpleDateFormat`类创建对象；
2. 使用`Date`类构造方法来获取或者转化时间和日期；
3. 使用`SimpleDateFormat`类创建对象调用：`String format(Date date)`；

4. 使用`SimpleDateFormat`类创建对象调用：`Date parse(String sourse)`；
   - <span style="color:red">**由于parse方法具有异常**</span>，所以需要抛出异常：try——catch  或者   throws  Exception；

**3.3、请使用时间与日期的API，计算出一个人已经出生了多少天？**

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
