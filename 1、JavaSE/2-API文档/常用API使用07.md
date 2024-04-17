## 1、<span style="color:brown">**Date类：**</span>Java.util

**1.1、概述：**

`Date` 类是 Java 中<span style="color:red">用于**表示日期和时间的类**，是<u>一个抽象类，不能直接实例化</u></span>！！！

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
void demo02() {
	Date d1 = new Date();
    long time = d1.getTime();
    System.out.println(time);
}
```



## 2、<span style="color:brown">**DateFormat类：**</span>Java.text

## <span style="color:purple">**DateFormat类是一个抽象类，SimpleDateFormat类 extends DateFormat！！！！**</span>

**2.1、概述：**

`DateFormat` 类是 Java 中用于<span style="color:green"><u>**格式化**和**解析**日期和时间的抽象基类</u>，是一个抽象类，不能直接实例化</span>！！！

**2.2、使用步骤：**

1. 格式化：Date日期——>String类型文本；
2. 解析：String文本——>Date日期；
3. 标准化：规范日期输出的格式；

**2.3、成员方法：**

- `String format(Date date)`：
  - <span style="color:green">**按照指定模式，把Date日期格式化为符合模式的字符串**</span>；

- `Date parse(String sourse)`：
  - <span style="color:red">**把标准格式的日期字符串解析成为Date日期**</span>；



## 3、<span style="color:orange">***SimpleDateFormat类：***</span>Java.test

### <!--SimpleDateFromat具体代替DateFormat类的使用过程-->

**3.1、构造方法：**

`SimpleDateFormat(String  pattern)`：

- 用给定的模式和默认语言环境的日期格式构造SimpleDateFormat；
- <span style="color:red">**String   pattern----------->传递指定的模式**</span>；
- <span style="color:red">**模式区分大小写**</span>，且写对应的模式会把模式替换成对应的日期和时间；
- ***"yyyy-MM-dd  HH:mm:ss" 或者 "yyyy年MM月dd日HH时mm分ss秒"***

**3.2、计算出一个人已经出生了多少天？**

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



## 4、<span style="color:brown">**LoclDate类：**</span>Java.time

**4.1、介绍：**

`LocalDate`类是`Java 8`中`java.time`包中的一个类，它代表了<span style="color:green">一个`ISO-8601`日历系统中的日期，没有时区信息</span>。

**4.2、常见格式：**

|            预定义格式            |                          日期格式                          |
| :------------------------------: | :--------------------------------------------------------: |
| **ISO_DATE**、**ISO_LOCAL_DATE** |                       **yyyy-MM-dd**                       |
|        **ISO_LOCAL_TIME**        |                      **HH:mm:ss.SSS**                      |
|     **ISO_LOCAL_DATE_TIME**      | **yyyy-MM-dd<span style="color:red">T</span>HH:mm:ss.SSS** |

**4.3、获取方法：**

- `public static LocalDate now()`：获取当前系统日期；
- `public static LocalDate of(int year, Month month, int dayOfMonth)`：指定年、月、日创建一个`LocalDate`对象；
- `public static LocalDate parse(CharSequence text, DateTimeFormatter formatter)`：将日期字符串解析为`LocalDate`对象，并且设置日期格式；
- `String format(DateTimeFormatter formatter)`：将日期格式化为指定的格式；
- `int getYear()`：获取年份；
- `int getMonthValue()`：获取月份，返回值`1-12`；
- `int getDayOfYear()`：获取年份中的天数；
- `int getDayOfMonth()`：获取月份中的天数；
- `DayOfWeek getDayOfWeek()`：获取星期几，返回值为`DayOfWeek`枚举类型；
- `boolean isLeapYear()`：判断是否为闰年；

**4.4、操作方法：**

- `LocalDate plusDays(long daysToAdd)`：增加指定天数；
- `LocalDate minusDays(long daysToSubtract)`：减少指定天数；
- `boolean isEqual(ChronoLocalDate other)`：比较两个日期对象的年、月、日是否完全相同；
- `boolean isBefore(ChronoLocalDate other)`：比较当前日期是否在指定日期之钱的方法；
- `boolean isAfter(ChronoLocalDate other)`：比较当前日期是否在指定日期之后的方法；

**4.5、代码演示：**

```java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String inputDate = scanner.nextLine(); // 2024-09-08
        LocalDate date = LocalDate.parse(inputDate, DateTimeFormatter.ISO_DATE);
        int dayOfYear = date.getDayOfYear();
        int dayOfMonth = date.getDayOfMonth();
        DayOfWeek dayOfWeek = date.getDayOfWeek();
        // 输出结果
        System.out.println("这一天是本年的第 " + dayOfYear + " 天"); // 252
        System.out.println("这一天是本月的第 " + dayOfMonth + " 天"); // 8
        System.out.println("这一天是星期" + dayOfWeek.getValue()); // 7
    }
}
```
