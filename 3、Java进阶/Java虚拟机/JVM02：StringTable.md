## 1、<span style="color:brown">StringTable的特性：</span>

### <!--别名: 串池、字符串常量池、Stringpool-->

### <!--串池为Hash表结构-->

- 字符串池可以避免重复创建字符串对象；

  ```java
  String s1 = "a";
  String s2 = "a";
  boolean isEqual = (s1 == s2);//为true，因为都是字符串池中的一个对象
  ```

- 常量池中的字符串仅是符号，第一次用到时才变为对象；

- 字符串变量拼接原理：StringBuilder（JDK1.8）；

- 字符串常量拼接原理：编译期优化；

- 使用intern方法，将串池中还没有的字符串对象放入串池；

## 2、<span style="color:brown">特性验证：</span>

**2.1、字符串变量拼接：**

<u>*底层原理：*</u>

```scss
创建StringBuilder对象：new StringBuilder()，然后再调用append()，最后调用toString()

由于toString()方法的返回值为：return new String(value, 0, count)，相当于new String(...)
```

---

<u>*代码分析：*</u>

```java
String s1 = "a";
String s2 = "b";
String s3 = "ab";
String s4 = s1 + s2;
System.out.println(s3 == s4);// false
```

<u>*变量s3位于串池中，而s4位于堆中*</u>，位置不一致。

**2.2、字符串常量拼接：**

```java
String s1 = "ab";
String s2 = 'a' + 'b';
System.out.println(s1 == s2);//true
```

底层原理：***javac在编译期间的优化***，结果已经在编译器确定为ab！！！

**2.3、字符串加载延迟：**

在IDEA采用断点+DeBug，在控制台的Memory验证字符串的延迟加载

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%8A%A0%E8%BD%BD%E5%BB%B6%E8%BF%9F.png" alt="image-20221116220746532" style="zoom:67%;" />

结论：常量字符串被引用时，它是缓加载的，遇到了才会加载
