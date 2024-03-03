## 1、<span style='color:brown'>String 类：</span>java.lang

### <!--String类重写了hashCode()、equals()、toString()三个方法-->

**1.1、概述：**

- Java程序中的<u>所有字符串的***字面值***</u> 都是作为<u>此类的实例</u>来实现，即：`String str = "Hello world"`；
- <font color="reen">**字符串内容是不可变的**</font>，但字符串可以<font color="green">**共享的**</font>；
- <font color="orange">`String`类在Java中是被`final`修饰的，即`String`类不能被继承</font>。这是由于需要考虑**字符串的不可变性**！！！

**1.2、字符串相加：**

```java
String str1 = "9"  String str1 = "2"   int num = 6   char ch = 'c'
str1 + str2 // "92"      
str1 + num // "96"
str1 + ch // "9c"
```

**1.3、创建String字符串方法：**

- `String()`，创建一个空白字符串；

- `String(char[ ] array)`，根据字符数组，创建对应的字符串；

- `String(char[ ] array, int offset, int count)`，offset：起始位置索引，count：读取的个数；

- `String(byte[ ] array)`,   根据字节数组，创建对应的字符串；

- `String(byte[ ] array, int offset, int count)`，offset：起始位置索引，count：读取的个数；

- 直接调用：`String  str = "Hello"`；


**1.4、字符串的常量池:**

> JDK1.6在方法区中，JDK1.7以后在堆中

```java
String str1 = "a";
String str2 = "b";
// 根据JVM串池的变量和常量拼接的底层原理
char[] charArray = {'a','b'};
String str3 = new String(charArray);
String str4 = "ab";
String str5 = str1 + str2;
System.out.println(str1 == str2);//true
System.out.println(str1 == str3);//false
System.out.println(str4 == str5);//false
```

`String a = "abc"`与`new String("abc")`的创建过程？🎈🎈🎈

```scss
1. 'JVM会使用常量池来管理字符串直接量'. 在执行这句话时, JVM会先检查常量池中是否已经存有"abc", 若没有则将"abc"存入常量池, 否则就复用常量池中已有的"abc", 将其引用赋值给变量a.
2. JVM会先使用常量池来管理字符串直接量, 即将"abc"存入常量池. 然后再创建一个新的String对象, 这个对象会被保存在堆内存中. 并且, 堆中对象的数据会指向常量池中的直接量.
```

**1.5、常用比较方法：**7个

1. `boolean equals(Object  obj)`：在判断时区分大小写
2. `boolean equalsIgnoreCase(Object  obj)`：在判断时不区分大小写
   - 参数可以是任何对象；
   - 只有参数是一个字符串，并且内容相同才会返回true，否则为false；
   - 任何对象都可以用Object进行接收；
   - <span style='color:orange'>**“==”在引用类型中比较的是地址值，而比较内容就需要使用equals( )方法；**</span>
3. `boolean isEmpty()`：判断一个字符串是否为空
   - 长度为0；
   - 内容为`null`；
4. `boolean isBlank()`：判断一个字符串是否为空白
   - 长度为0；
   - 内容为`null`；
   - 只包含除字符以外的其他内容，例如：空格、制表符、换行符等；
5. `boolean stratsWith(String prefix, int offset)`
   - 比较字符串是否是以【prefix】为前缀；
   - 而【offset】表示的是开始查找位置；
6. `boolean endsWith(String prefix, int offset)`
   - 判断字符串是否以prefix为后缀；
7. `boolean contains(String str)`
   - 判断字符串是否包含某个字符串；

**1.6、常用获取方法：**5个

- `int length()`
  - 获取字符串中的字符个数，拿到字符串长度；
- `String concat(String  str)`
  - 将当前字符串和参数字符串拼接成一个新的字符串，并返回；
  - <span style='color:red'>**此方法只是单纯的拼接字符串，并不是改变了字符串的内容;**</span>
  - <span style='color:red'>**字符串的内容是不可以改变的;**</span>
- `char charAt(int  index)`
  - 获取索引位置的单个字符  (索引从0开始)；
- `int indexOf(String  str)`
  - 查找参数字符串在本字符串当中首次出现的索引位置；
  - 如果没有返回-1；
- `String intern()`
  - 如果池中已经包含一个等于该String对象的字符串（由equals(Object)方法确定），则返回池中的字符串；
  - 否则，将此String对象添加到池中并返回对该String对象的引用；

**1.7、截取方法：**2个

1. `String substring(int  index)`

   - 截取从参数位置开始到字符串结尾处，并返回一个新的字符串；

2. `String subsring(int begin, int end)`

   - <span style='color:violet'>**截取一个范围：[ begin，end )，从begin位置开始，到end-1位置处结束**</span>；
   - 结果返回一个<u>**`新的字符串`**</u>；

**1.8、转换方法：**6个

1. `char[] toCharArray()`
   - 将字符串转换成字符数组；
2. `byte[] getBytes()`
   - 获取字符串底层的字节数组；
3. `String replace(CharSequence  oldString, CharSequence  newString)`
   - 将所有出现的旧字符串替换成新的字符串，并返回新字符串；
   - <span style='color:orange'>**CharSequence  :  可以接受字符串类型**</span>；
4. `String replaceAll(String regex, String replacement)`
   - 替换字符串中满足正则表达式 `regex` 的部分为指定的替换字符串
5. `String toLowerCase()`
   - 将字符串转换成小写格式

6. `String toUpperCase()`
   - 将字符串转换成大写格式

**1.9、分割方法：**1个

`String[] split (String regex)`

- 按照参数的规则，把字符串分割成若干部分；
- <span style='color:green'>**String  regex  :  分割目标，以该目标为分界线来分割字符串，但是如果要以英语句点" . "作为分割，必须改写成为" \\\\. "**</span>；



## 2、<span style="color:brown">**StringBuilder类：**</span>Java.lang

**2.1、String、StringBuffer、StringBuilder的区别：**

![image-20221027201155435](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/String%E3%80%81StringBuilder%E3%80%81StringBuffer.png)

**2.2、详解：**

1. <span style="color:red">**`StringBuilder和StringBuffer都可作为字符缓冲区`**</span>，可以提高字符串的操作效率；
2. 底层是<span style="color:green">**char类型数组**</span>，且<span style="color:orange">**长度可以变化**</span>；
3. 初始化容量是<span style="color:violet">**16字符**</span>，<u>若超出该范围，会自动进行扩容</u>；

**2.3、构造方法：**

1. `StringBuilder()`
   - 构造一个空的StringBuilder容器；
2. `StringBuilder(String str)`
   - 构造一个StringBuilder容器，并将字符串添加进去；
   - <font color="red">**String对象 -------> StringBuilder对象**</font>；

```java
StringBuilder bu1 = new StringBuilder();
System.out.println("bu1:"+bu1);
StringBuilder bu2 = new StringBuilder("Java");
System.out.println("bu2:"+bu2);
```

**2.4、成员方法：**

1. `StringBuilder append(...)`------->链式编程方法！！！！

   - 参数：<span style="color:red">**可以同时添加任意数据类型的数据**</span>；
   - 返回当前对象自身；
   - 为StringBuilder添加数据的作用；

   ```java
   StringBuilder bu1 = new StringBuilder();
   bu1.append("abc").append(123).append(5.3).append(true); // 多个对象的地址值相同
   System.out.println(bu1); // abc1235.3true
   ```
   
2. `String toString()`

   - 将StringBuilder对象转换成String对象；

   ```java
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
   ```
   
3. `StringBuilder reverse()`

   - 将传入StringBuilder的数据，**倒序排列**；

   ```java
   StringBuilder stringBuilder = new StringBuilder();
   stringBuilder.append(1).append(2).append("nihao");
   StringBuilder reverse = stringBuilder.reverse();
   System.out.println(reverse);//oahin21
   ```



## 3、<span style="color:brown">**经典案例：**</span>

**3.1、从第一个字符串中删除第二个字符串的全部字符？**

```java
public void check(String str, String target) {
    ArrayList<String> list = new ArrayList<>();
    for (int i = 0; i < str.length(); i++) {
        char c = str.charAt(i);
        if(!target.contains("" + c)) {
            list.add(c+"");
        }
    }
    for (int i = 0; i < list.size(); i++) {
        System.out.print(list.get(i));
    }
}
```

**3.2、判断回文串？**

回文串是指正读和反读都相同的字符串，例如：`moom`、`level`等。先将`chars`字符串进行处理：

- 字符串转换为小写；
- 正则表达式替换，`[^a-z0-9]`：匹配除了小写字母和数字之外的任意字符；

然后使用**`双指针方案`**，从<u>左右两端向中间遍历</u>，判断是否左右相等

```java
public static boolean isPalStr(String chars) {
    String str = chars.toLowerCase().replaceAll("[^a-z0-9]", "");
    int left = 0;
    int right = str.length() - 1;
    while(left < right) {
        if(str.charAt(left) != str.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

**3.3、解码字符串？**

解析给定格式的字符串：`k{string}`，表示其中的string重复k次。

```java
String decodeString(String s) {
    // 保存重复次数
    Stack<Integer> countStack = new Stack<>();
    // 保存字符串
    Stack<StringBuilder> stringStack = new Stack<>();
    // 构建当前的字符串
    StringBuilder currentString = new StringBuilder();
    // 保存当前的重复次数
    int currentCount = 0;
    for (char ch : s.toCharArray()) {
        // 判断字符是否为数字
        if(Character.isDigit(ch)) {
            /*考虑数字可能为多位数*/
            currentCount = currentCount*10 + (ch - '0');
        }else if(ch == '{') {
            countStack.push(currentCount);
            stringStack.push(currentString);
            // 重置变量 (主要关注currentString)
            currentCount = 0;
            currentString = new StringBuilder();
        }else if(ch == '}') {
            StringBuilder pop = stringStack.pop();
            int count = countStack.pop();
            for (int i = 0; i < count; i++) {
                pop.append(currentString);
            }
            currentString = pop;
        }else {
            currentString.append(ch);
        }
    }
    return currentString.toString();
}
```

输入`"2{ab}"`，进入循环体遍历字符数组：

- 通过字符类方法`boolean isDigit(char c)`判断字符是否为数字，然后<u>将字符转换成整数</u>（<span style="color:red">考虑数字为多位数</span>）

- 遇到字符`'{'`，将`currentCount=2`和`currentString=null`分别压入两个`Stack`中，并<u>主要重置`currentString=null`备用</u>；

- 之后会遍历到字符`'a'`和`'b'`，<u>直接调用`currentString.append(ch)`，添加到`currentString`中</u>；

- 当遇到字符`'}'`，进行两个`Stack`的出栈操作，此时`pop=null`和`count=2`，进入重复循环体：

  1. 当`i=0`，追加`currentString="ab"`到`pop`，此时`pop="ab"`；
  2. 当`i=1`，再次追加`currentString`到`pop`，此时`pop="abab"`；

  跳出循环，执行`currentString = pop`，更新`currentString="abab"`。

**3.4、找出不含重复字符的最长子串？**

在一个`String`字符串中，找出**<u>不含重复字符</u>**的最长字串的长度：

在代码`index[s.charAt(j)]`中，`s.charAt(j)`返回值为<u>`char`类型</u>，可<span style="color:red"><u>**自动转换成ACII码**</u></span>。另外，`index[s.charAt(j)] = j + 1`将`j`的值加`1`，主要是为了<span style="color:green">避免每次更新左边界都需要加`1`的操作</span>！！！

```java
int lengthOfSubstring(String s) {
    int n = s.length();
    int maxLength = 0;
    int[] index = new int[128]; // 用于记录字符的索引位置
    for (int i = 0, j = 0; j < n; j++) {
        i = Math.max(index[s.charAt(j)], i); // 更新左边界i
        maxLength = Math.max(maxLength, j - i + 1); // 更新最大长度
        index[s.charAt(j)] = j + 1; // 记录字符的索引位置
    }
    return maxLength;
}
```

以**<u>`abcabcb`</u>**为例，代码执行过程：

1. 初始时，`i = 0`，`j = 0`，`maxLength = 0`，`index`数组全为0。
2. 第一次循环：
   - 字符 'a' 在位置0，`index['a']`为0，更新 `i = Math.max(0, 0) = 0`。
   - `maxLength = Math.max(0, 0 - 0 + 1) = 1`，更新最大长度为1。
   - `index['a'] = 1`，记录字符 'a' 在位置0的索引。
3. 第二次循环：
   - 字符 'b' 在位置1，`index['b']`为0，更新 `i = Math.max(0, 0) = 0`。
   - `maxLength = Math.max(1, 1 - 0 + 1) = 2`，更新最大长度为2。
   - `index['b'] = 2`，记录字符 'b' 在位置1的索引。
4. 第三次循环：
   - 字符 'c' 在位置2，`index['c']`为0，更新 `i = Math.max(0, 0) = 0`。
   - `maxLength = Math.max(2, 2 - 0 + 1) = 3`，更新最大长度为3。
   - `index['c'] = 3`，记录字符 'c' 在位置2的索引。
5. 第四次循环：
   - 字符 'a' 在位置3，`index['a']`为1，更新 `i = Math.max(1, 0) = 1`。
   - `maxLength = Math.max(3, 3 - 1 + 1) = 3`，最大长度不变。
   - `index['a'] = 4`，记录字符 'a' 在位置3的索引。
6. 以此类推，得到最长的字串为**abc**，长度为3。
