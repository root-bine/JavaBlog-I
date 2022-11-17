## 1、<span style='color:brown'>**包装类：java.lang**</span>

**1.1、概述：**

| 基本数据类型 |                    包装类                    |
| :----------: | :------------------------------------------: |
|     byte     |                     Byte                     |
|    short     |                    Short                     |
|     int      |  <span style='color:red'>**Integer**</span>  |
|     long     |                     Long                     |
|     char     | <span style='color:red'>**Character**</span> |
|    float     |                    Float                     |
|    double    |                    Double                    |
|   boolean    |                   Boolean                    |

**1.2、装箱与拆箱：**

**装箱**指的是将基本数据类型转为包装类；

**拆箱**指的是将包装类转为基本数据类型；

```java
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


## 2、<span style="color:brown">包装类类型转换：</span>

**2.1、int 与 Integer 之间的转换**

```java
// int > Integer
int i = 13;
Integer integer = Integer.valueOf(i);
------------------------------------------------------------------------------------------------------------
// Integer > int
Integer integer = 10;
int i = integer.intValue();
```

**2.2、float 与 Float 之间的转换**

```java
// float > Float
float i = 13;
Float fo = Float.valueOf(i);
------------------------------------------------------------------------------------------------------------
// Float > float
Float fo = new Float(13);
float i = fo.floatValue();
```

**2.3、char 与 Character 之间的转换**

```java
// char > Character
char i = 's';
Character fo = Character.valueOf(i);
------------------------------------------------------------------------------------------------------------
// Character > char
Character character = new Character('s');
char c = character.charValue();
```

**2.4、boolean 与 Boolean 之间的转换**

```java
// boolean > Boolean
boolean i = true;
Boolean aBoolean = Boolean.valueOf(i);
------------------------------------------------------------------------------------------------------------
// Boolean > boolean
Boolean aBoolean = new Boolean(true);
boolean c = aBoolean.booleanValue();
```

**2.5、包装类与 String 类型的转换**

```java
// 包装类 > String 
// 用 Integer来举例, 其它包装类的转换方式与 Integer 相同
Integer integer = 13;
String string = integer.toString();
------------------------------------------------------------------------------------------------------------
// String > 包装类
String string = "13";
Integer integer = Integer.valueOf(string);
```

