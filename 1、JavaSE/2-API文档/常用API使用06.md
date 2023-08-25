## 1、<span style="color:brown">**Object类：**</span>java.lang👮‍♂️👮‍♂️👮‍♂️

**1.1、什么是Object类？ **

<span style="color:brown">Object类是Java中所有类的超类</span>，它定义了所有对象都具有的通用方法和属性。所有的类都直接或间接地继承自Object类。

**1.2、Object类常用方法：**7类

- `boolean equals(Object obj)`：默认情况下，用于比较两个<u>*对象的引用*</u>是否相等
- `int hashCode()`：返回对象的哈希码值
- `String toString()`：返回对象的字符串表示
- `Class<?> getClass()`：返回对象的运行时类
- `Object clone()`：创建并返回对象的副本
- `void finalize()`：在对象被垃圾回收器回收之前调用
  - 通常用于释放对象占用的资源，比如关闭文件、释放网络连接等
- `void wait(long timeout)`、`void wait()`、`void notify()`、`void notifyAll()`：用于线程间的通信

**1.3、为什么在重写 equals 方法的时候需要重写 hashCode 方法？**

<span style="color:red">重写equals()方法是为了判断对象内容的相等性，而重写hashCode()方法是为了保证相等的对象具有相等的哈希码值</span>！！！

**1.4、有没有可能两个不相等的对象有相同的 hashcode?**

是的，存在两个不相等的对象具有相同的hashCode()值的情况，这种情况被称为哈希冲突。

```java
public class Demo {
    public static void main(String[] args) throws Exception {
        String str1 = "重地";
        String str2 = "通话";
        System.out.println(str1 == str2);// false
        System.out.println(str1.hashCode() == str2.hashCode());// true
    }
}
```



## 2、<span style="color:brown">Objects类：</span>java.util

### <span style="color:green">**在JDK7加入了一个Objects的工具类，其中的方法是<u>null-save（空指针安全）或null-tolerant（容忍空指针）</u>**</span>！！！

- **boolean equals( Object a, Object b)**
  
  - 比较两个对象的<font color="red">**内容**</font>是否相同
  
  ```java
  String str1 = "Hello";
  String str2 = "Hello";
  boolean result = Objects.equals(str1, str2);// true
  ```

- **public static <T> T requireNonNull(T obj)**

  - 用于检查一个对象是否为null
    - 如果传入的对象为null，则会抛出NullPointerException异常；
    - 如果对象不为null，则会返回该对象；

  ```java
  String name = null;
  String result = Objects.requireNonNull(name);
  ```

  











