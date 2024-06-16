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

**1.5、字符串比较：**

 <span style='color:orange'>**“==”在引用类型中比较的是地址值，而比较内容就需要使用equals( )方法**</span>。

|                      方法                       |               功能                | 说明                                                       |
| :---------------------------------------------: | :-------------------------------: | ---------------------------------------------------------- |
|          `boolean equals(Object  obj)`          |           比较是否相等            | 任何对象都用Object进行接收；<br>在判断时**不区分大小写**； |
|     `boolean equalsIgnoreCase(Object  obj)`     |           比较是否相等            | 在判断时**区分大小写**                                     |
|               `boolean isEmpty()`               |   判断一个<u>字符串是否为空</u>   | <u>长度为0</u>，或者<u>内容为NULL</u>                      |
|               `boolean isBlank()`               |  判断一个<u>字符串是否为空白</u>  |                                                            |
| `boolean stratsWith(String prefix, int offset)` |   比较字符串**前缀**为`prefix`    | `offset`表示开始查找位置                                   |
|  `boolean endsWith(String prefix, int offset)`  |   判断字符串**后缀**为`prefix`    |                                                            |
|         `boolean contains(String str)`          | 判断字符串<u>是否包含某字符串</u> |                                                            |

**1.6、字符串获取：**

|             方法             |                             功能                             | 说明                                                         |
| :--------------------------: | :----------------------------------------------------------: | ------------------------------------------------------------ |
|        `int length()`        |            获取字符串中的字符个数，拿到字符串长度            |                                                              |
| `String concat(String  str)` |  将<u>**当前字符串**和**`str`**</u>拼接成一个**`New str`**   | 由于<span style='color:red'>字符串的不可变性</span>，<br><u>**不能改变字符串内容**</u> |
|  `char charAt(int  index)`   |                    获取索引位置的单个字符                    | `Index`从0开始                                               |
|  `int indexOf(String  str)`  | 查找**`str`在本字符串当中**<span style='color:red'>`first`出现的`Index`</span> | 没有，则返回-1                                               |
|      `String intern()`       | 1、**字符串常量池**中已经<u>包含一个等于<br>该String对象的字符串</u>，则返回<span style='color:red'>池中的字符串</span>；<br>2、**<u>如果没有</u>**，将**此String对象**<u>添加到池中</u>，<br><u>返回对该String对象的引用</u>； | 等于字符串<u>**由equals(Object)方法**</u>确定                |
| `int compareTo(String str)`  | 按字典顺序比较两个字符串；<br>字典顺序，是从`A(a)`到`Z(z)`的顺序； | 返回一个整数：<br><0：当前字符串小于 `str`；<br>=0：当前字符串等于 `str`；<br>>0：当前字符串大于 `str`；<br> |

**1.7、截取字符串：**

|                 方法                  |                             功能                             | 说明                     |
| :-----------------------------------: | :----------------------------------------------------------: | ------------------------ |
|    `String substring(int  index)`     |         截取`[index, length)`，并返回一个`New str`；         |                          |
| `String subsring(int begin, int end)` | 截取一个范围：<span style='color:green'>**`[begin, end)`**</span>，<br>并返回一个`New str`； | 从`begin`，到`end-1`结束 |

**1.8、字符串转换：**

| 方法                                                         | 功能                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `char[] toCharArray()`                                       | 将字符串转换成字符数组                                       |
| `byte[] getBytes()`                                          | 获取字符串底层的字节数组                                     |
| `String replace(CharSequence  oldString, CharSequence  newString)` | 将所有的`oldString`替换成newString，<br>并返回一个`New str`； |
| `String replaceAll(String regex, String replacement)`        | 将字符串满足<u>正则表达式 `regex` 的部分</u>，<br>替换为`replacement` |
| `String toLowerCase()`                                       | 将字符串转换成**小写格式**                                   |
| `String toUpperCase()`                                       | 将字符串转换成**大写格式**                                   |

**1.9、分割字符串：**

|              方法               |                        功能                         | 说明                                                         |
| :-----------------------------: | :-------------------------------------------------: | ------------------------------------------------------------ |
| `String[] split (String regex)` | 按照`String  regex`规则，把**字符串分割成若干部分** | <span style='color:green'>**以`String  regex`为分界线**</span>来**分割字符串**，但是如果要<u>以英语句点" `.` "作为分割</u>，必须改写成为" `\\.` "； |

**1.10、去空格字符**

|           方法           |            功能             | 说明                                                         |
| :----------------------: | :-------------------------: | ------------------------------------------------------------ |
|     `String trim()`      |  去除字符串两端的空白字符   | 1、返回一个字符串的副本，忽略前导空白和尾部空白；<br/>2、**空白字符**：<u>空格（' '）、制表符（'\t'）、换行符（'\n'）、<br>回车符（'\r'）等</u> |
|     `String strip()`     | 去除**所有Unicode空白字符** |                                                              |
| `String stripLeading()`  |    去除**前导空白字符**     |                                                              |
| `String stripTrailing()` |    去除**尾部空白字符**     |                                                              |



## 2、<span style="color:brown">**StringBuilder类：**</span>Java.lang

**2.1、String、StringBuffer、StringBuilder的区别：**

![image-20221027201155435](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/String%E3%80%81StringBuilder%E3%80%81StringBuffer.png)

**2.2、详解：**

1. <span style="color:red">**`StringBuilder和StringBuffer都可作为字符缓冲区`**</span>，可以提高字符串的操作效率；
2. 底层是<span style="color:green">**char类型数组**</span>，且<span style="color:orange">**长度可以变化**</span>；
3. 初始化容量是<span style="color:violet">**16字符**</span>，<u>若超出该范围，会自动进行扩容</u>；

**2.3、构造方法：**

|            方法             |                         功能                          | 说明                                                         |
| :-------------------------: | :---------------------------------------------------: | ------------------------------------------------------------ |
|      `StringBuilder()`      |      构造一个<u>**空的**`StringBuilder`容器</u>       |                                                              |
| `StringBuilder(String str)` | 构造一个`StringBuilder`容器，并<u>将`str`添加进去</u> | <font color="red">**String对象 -------> StringBuilder对象**</font> |

```java
StringBuilder bu1 = new StringBuilder();
System.out.println("bu1:"+bu1);
StringBuilder bu2 = new StringBuilder("Java");
System.out.println("bu2:"+bu2);
```

**2.4、成员方法：**

​	使用`append()`方法，相当于将<u>**两个字符串`str1`和`str2`拼接在一起**</u>，但会<font color="blue">`New str`中产生多余的空格</font>。因此，在特定的编程需要时，<u>需要使用`trim()`方法，去除`New str`两端的空格字符</u>！！！

| 方法                               | 功能                                                    | 说明                                                         |
| ---------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| `StringBuilder append(Object obj)` | 为`StringBuilder`添加数据的作用                         | 链式编程方法：<br>`sbl.append(1).append("Hello").`<br>`append('C').append(6.2)` |
| `String toString()`                | 转换<u>`StringBuilder`对象</u>**为**<u>`String`对象</u> |                                                              |
| `StringBuilder reverse()`          | 将传入StringBuilder的数据，**倒序排列**                 |                                                              |

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

**3.5、压缩字符串，若压缩后长度大于原始长度则输出原字符串？**

```java
public static String compressString(String str) {
    if (str == null || str.isEmpty()) {
        return str;
    }
    StringBuilder compressed = new StringBuilder();
    int countConsecutive = 0;
    for (int i = 0; i < str.length(); i++) {
        countConsecutive++;
        if (i + 1 >= str.length() || str.charAt(i) != str.charAt(i + 1)) {
            compressed.append(str.charAt(i));
            compressed.append(countConsecutive);
            countConsecutive = 0;
        }
    }
    return compressed.length() < str.length() ? compressed.toString() : str;
}
```

**<u>*思路解析：*</u>**

​	采用`StringBuilder`类中`append()`进行累计添加，目标是将多个重复的字符以**<u>字符+数字</u>**表示。定义`countConsecutive`用于<span style="color:red">**统计字符重复次数**</span>。执行过程如下：

1. 整体遍历目标字符串**`str`**，然后一开始就进行`countConsecutive++`；
2. 判定进行`append()`累加的条件是：
   - 当前位置`Idex+1`，若超出str.length()；
   - 当前`Idex`对应字符，不等于下一位置的字符；
3. 若执行了`append()`，则将`countConsecutive`重置为`0`，目的是为了<span style="color:green"><u>对**新字符重复次数**进行统计</u></span>；

​	当完全遍历str之后，进行一个<u>**题意条件判定**</u>：`compressed.length() < str.length() ? compressed.toString() : str`，结果如下：

- 压缩字符串 < 原字符串，则输出压缩结果；
- 压缩字符串 >= 原字符串，则输出原字符串；

