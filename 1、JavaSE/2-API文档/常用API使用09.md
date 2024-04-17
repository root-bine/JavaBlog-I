## 1、<span style='color:brown'>**包装类：java.lang**</span>

**1.1、概述：**

`Number`类（`java.lang`）是<span style="color:red">Java中的一个抽象类</span>，它是<span style="color:blue">所有数值类型</span>的基类。

在包装类中，除<u>`Character类`和`Boolean类`</u>外，<span style="color:green">其他包装类都继承了`Number`类</span>。

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

**1.2、自动装箱&自动拆箱：**

**装箱**指的是将基本数据类型转为包装类；

**拆箱**指的是将包装类转为基本数据类型；

​	当进行`int`和`Integer`之间的转换时，需要注意<span style="color:red">空指针异常</span>。如果`Integer`对象为`null`，在进行拆箱操作时，就会抛出一个异常`NullPointerException`。因此，<u>***在进行拆箱操作前，需要先进行null的判断***</u>。

---

- int转换为Integer：

  ```java
  int num = 10;
  Integer integer = num; // 自动装箱
  ```

- Integer转换为int：

  ```java
  Integer integer = 10;
  int num = integer; // 自动拆箱
  ```

**1.3、int和Integer有什么区别，二者在做==运算时会得到什么结果？**🎉🎋🎋

​	`int`是基本数据类型，`Integer`是`int`的包装类。<span style="color:blue">二者在做==运算时，`Integer`会自动拆箱为`int`类型，然后再进行比较</span>。届时，如果两个`int`值相等则返回`true`，否则就返回`false`。



## 2、<span style="color:brown">常用方法：</span>

**2.1、包装类通用方法：**

|           valueOf(...)            | <span style="color:red">一个静态方法</span>，用于将基本数据类型转换为对应的包装类对象 |
| :-------------------------------: | :----------------------------------------------------------: |
|          **xxxValue()**           |              **将包装类对象转换回基本数据类型**              |
| **parseXxx(String s, int radix)** |             **将字符串转换为对应的基本数据类型**             |

演示：

```java
// int > Integer
int i = 13;
Integer integer = Integer.valueOf(i);
------------------------------------------------------------------------------------------------------------
// Integer > int
Integer integer = 10;
int i = integer.intValue();
```

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

**2.2、整数转换成二进制字符串：**

```java
public static String toBinaryString(int i)
```

该方法是 `Integer` 类独有的！！！



## 3、<span style="color:brown">经典问题：</span>🎟️🎟️🎟️

**3.1、分析==在Integer与int之间比较的结果？**

```java
Integer i1 = new Integer(12);
Integer i2 = new Integer(12);
System.out.println(i1 == i2);//false
Integer i3 = 127;
Integer i4 = 127;
int i5 = 127;
System.out.println(i3 == i4);//true
System.out.println(i3 == i5);//true
Integer i6 = 128;
Integer i7 = 128;
int i8 = 128;
System.out.println(i6 == i7);//false
System.out.println(i6 == i8);//true
```

**`结果分析`**：

> <span style="color:green">Integer类的内部维护了一个缓存池，范围是 `-128` 到 `127`</span>

**`i1 == i2`**： i1 和 i2 是通过 `new Integer(12)` 创建的两个不同的对象，虽然它们的值相同，但是比较的是对象的引用；

**`i3 == i4`**：i3 和 i4 的值都是 127，**都取自`Integer`的缓存池中，且未超出范围**；

**`i3 == i5`**：i3类型为`Integer`，i5类型为`int`，在比较时i3会自动拆箱为`int`，最后是两个`int`类型数值比较；

**`i6 == i7`**：i6 和 i7 的值都是 128，<u>超出缓存池范围，就会**创建两个不同对象，此时比较的就是对象的引用**</u>；

**`i6 == i8`**：i6类型为`Integer`，i8类型为`int`，在比较时i6会自动拆箱为`int`，最后是两个`int`类型数值比较；

**3.2、字符串变形：**

对一个包含空格的字符串，将空格隔开的单词反序，并将字母大小写互换。

```java
public String trans (String s, int strLength) {
    if(s == null || s.length() == 0) {
        return s;
    }
    char[] chars = s.toCharArray();
    reverse(chars, 0, strLength-1); // 将整个字符反转
    int start = 0;
    for (int i = 0; i < strLength; i++) {
        if(chars[i] == ' ') { // 将每一个单词返回正确顺序
            reverse(chars, start, i-1);
            start = i + 1;
        }
    }
    reverse(chars, start, strLength - 1); // 反转最后一个单词
    swap(chars, 0, strLength-1);
    return new String(chars);
}
// 反转字符串
public void reverse(char[] chars, int start, int end) {
    while(start < end) {
        char temp = chars[start];
        chars[start] = chars[end];
        chars[end] = temp;
        start++;
        end--;
    }
}
// 大小写互换
public void swap(char[] chars, int start, int end) {
    for (int i = start; i <= end; i++) {
        if(Character.isUpperCase(chars[i])) {
            chars[i] = Character.toLowerCase(chars[i]);
        }else if(Character.isLowerCase(chars[i])) {
            chars[i] = Character.toUpperCase(chars[i]);
        }
    }
}
```

输入测试用例：`This is a simple`、`16`，演示步骤如下：

字符串转变成字符数组：`chars = ['T', 'h', 'i', 's', ' ', 'i', 's', ' ', 'a', ' ', 's', 'i', 'm', 'p', 'l', 'e']`

调用`reserse()`方法得到：`chars = ['e', 'l', 'p', 'm', 'i', 's', 'a', ' ', 's', 'i', ' ', 's', 'i', 'h', 'T']`

遍历当前的字符数组`chars`，在`chars[i]==' '`时进行反转：

- `i=7`：
  - 遇到一个空格，调用`reverse()`方法反转`[0,6]`范围内字符；
  - 更新`start = i+1 = 8`；
- `i=10`：
  - 遇到第二个空格，调用`reverse()`方法反转`[8,9]`范围内字符
  - 更新`start = i+1 = 11`；

之后不再出现空格，跳出循环，<u>再次调用`reverse()`方法，将剩余内容进行反转</u>，`chars`数组内容为：`simple is a this`。

调用`swap()`方法，将字符数组的内容大小写互换，得到`SIMPLE A IS tHIS`。

**3.3、偶数反转二进制表示？**

输入一串以空格分隔的自然数，将其中的偶数转换成二进制格式，然后输出反转之后的结果。例如输入`1 6 5`，其中`6`是偶数，二进制表示为`110`，反转后为`011`，表示`3`，最后是输出内容为：`1 3 5`。

```java
public class Main {
    // 将一个整数转换为二进制字符串
    public static String toBinaryString(int n) {
        return Integer.toBinaryString(n);
    }
    // 将一个二进制字符串反转
    public static String reverseBinaryString(String binaryString) {
        return new StringBuilder(binaryString).reverse().toString();
    }
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String inputLine = scanner.nextLine(); // 1 6 5
        String[] inputs = inputLine.split(" ");// [1,6,5]
        ArrayList<Integer> result = new ArrayList<>();
        for (String input : inputs) {
            int num = Integer.parseInt(input); 
            if (num % 2 == 0) { // int:6
                String binaryString = toBinaryString(num); // 110
                String reversedBinaryString = reverseBinaryString(binaryString); //011
                int reversedNum = Integer.parseInt(reversedBinaryString, 2); // 3
                result.add(reversedNum);
            } else {
                result.add(num);
            }
        }
        for (int num : result) {
            System.out.print(num + " "); // 1 3 5 
        }
    }
}
```

