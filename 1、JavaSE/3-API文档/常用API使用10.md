# 常用API使用10

## 1、<span style='color:brown'>**包装类：java.lang**</span>

- **概述：**

  - 数据类型        包装类

  - byte				Byte
  - short               Short
  - int                    <span style='color:red'>**Integer**</span>
  - long                 Long
  - float                 Float
  - double             Double
  - char                  <span style='color:red'>**Character**</span>
  - boolean            Boolean

- ***装箱与拆箱：***【以Integer包装类为例】

  - 装箱：把基本数据类型的数据包装到包装类中

    - 基本数据类型 ---------> 包装类

    - 构造方法：
      1. Integer（ int  value ）
         - 构造一个新分配的Integer对象，它表示指定的int值
      2. Integer（ String  str）
         - 构造一个新分配的Integer对象，它表示String参数所指示的int值
    - 静态方法：
      1. public  static  Integer  valueOf( int  i )
         - 返回一个表示指定的int值的Integer实例
      2. public  static  Integer  valueOf( String  str )
         - 返回保存指定的String值的Integer对象

  - 拆箱：在包装类中取出基本数据类型的数据

    - 包装类 -----------> 基本数据类型

    - 成员方法：

      public  int   intValue( )

      - 以int类型返回该Integer值

    ```java
    public class Demo04 {
        public static void main(String[] args) {
            //装箱
            Integer in1 = new Integer(1);
            System.out.println(in1);
            Integer in2 = new Integer("123");
            System.out.println(in2);
            Integer in3 = Integer.valueOf(2);
            System.out.println(in3);
            Integer in4 = Integer.valueOf("456");
            System.out.println(in4);
            //拆箱
            int i = in1.intValue();
            System.out.println(i);
        }
    }
    ```
  
- ***自动装箱与拆箱：***JDK1.5以后出现的新特性

  ```java
  import java.util.ArrayList;
  public class Demo05 {
      public static void main(String[] args) {
          /**
           * 自动装箱
           * Integer in = new Integer(1);
           */
          Integer in = 1;
          /**
           * 自动拆箱
           * in+2 == in.intValue()+2
           * in = in+2 相当于 in = new Integer(3)
           */
          in = in+2;
          System.out.println(in);
          /**
           * ArrayList无法存储整数，但可以存储包装类
           */
          ArrayList<Integer> list = new ArrayList<>();
          //自动装箱
          list.add(123);
          System.out.println(list);
          //自动拆箱
          int i = list.get(0);
          System.out.println(i);
  
      }
  }
  ```


## 2、<span style="color:brown">基本数据类型与String类型的数据转换</span>

- **基本数据类型 -------> 字符串**

  1. <span style="color:violet">**基本数据类型+"  "**</span>
  2. 静态方法：static   String   toString( int i )
  3. 静态方法：Static   String   valueOf( int i )

- **字符串 ------> 基本数据类型**

  使用paresXXX( String s)方法，这个方法除了Character包装类没有，其他7类都具有此方法；

  其中的XXX对应要转换的基本数据类型；

```java
package Stringpractice;
public class Demo06 {
    public static void main(String[] args) {
        //基本数据类型-------->字符串
        String str = 100+"";
        System.out.println(str+200);
        String str1 = Integer.toString(100);
        System.out.println(str1+200);
        String str2 = String.valueOf(100);
        System.out.println(str+200);
        //字符串---------->基本数据类型
        int i = Integer.parseInt("100");
        System.out.println(i+200);
    }
}
```



