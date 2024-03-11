## 1、<span style="color:brown">**数组工具类Arrays：**</span>java.util

### <!--与数组相关的工具类，用来实现数组的常见操作-->

**1.1、Arrays工具类的常用方法：**

- `public static String toString(数组)`
  
  - <span style="color:red">**参数数组----->String；**</span>
  - 默认格式：[元素1，元素2，元素3  .....]
  
- `public static void sort(数组)`

  - 对数组元素进行排序；

  - <span style="color:red">**排序规则:**</span>
    - <span style="color:green">**如果是数值，按照升序从小到大排序；**</span>
    - <span style="color:green">**如果是字符串，按照字母升序排列；**</span>
    - <span style="color:green">**如果是自定义类型，那么这个自定义的类就需要有**Compareable或者Comparetor接口**的支持；**</span>
  
- `public static int[] copyOf(int[] original, int newLength)`

  - 源码：

    ```java
    // Arrays.copyof是用于数组进行复制时常使用的方法, 本身在Arrays类里面
    // 而之所以能这么使用而不用创建对象, 在于该方法本身由static修饰
    // 被static修饰的方法可以在该类创建对象前载入jvm
    public static int[] copyOf(int[] original, int newLength) {
        int[] copy = new int[newLength];
        System.arraycopy(original, 0, copy, 0,
                         Math.min(original.length, newLength));
        return copy;
    }
    ```
  
  - 解析：
  
    - `传回的数组是新的数组对象，改变传回数组中的元素值，不会影响原来的数组`；
  
    - int[] original ：新的数组对象的前身；
  
    -  int newLength：新的数组对象的长度；
    
  - 底层原理：**对象数据的拷贝**
  
    ```java
    public static native void arraycopy(Object src,  int  srcPos,
                                        Object dest, int destPos,
                                        int length);
    - Object src: 原数组
    - int  srcPos: 原数组的起始位置
    - Object dest: 目标数组
    - int destPos: 目标数组的起始位置
    - int length: 要复制的长度
    ```

**1.2、倒序打印数据：**

请使用Arrays相关的API，将一个随机字符串中的所有字符进行排序，然后**倒序**打印出来

- <span style="color:orange">**快捷方法：**</span>
  - 正序---->数组名.fori
  - 倒序------>数组名.forr

```java
public class Practice {
    public static void main(String[] args) {
        String str = "a48f5v8af5bat448a";
        char [] array = str.toCharArray();
        Arrays.sort(array);
        /**正序*/
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i]+" ");
        }
        System.out.println();
        /**倒序*/
        for (int i = array.length - 1; i >= 0; i--) {
            System.out.print(array[i]+" ");
        }
    }
}
```

**1.3、数组的数据复制：**

假设有一个数组中保存了 5 个成绩，现在需要在一个新数组中保存这 5 个成绩，同时留 3 个空余的元素供后期开发使用

```java
public class Test19{
    public static void main(String[] args) {
        // 定义长度为 5 的数组
        int scores[] = new int[]{57,81,68,75,91};
        // 输出原数组
        System.out.println("原数组内容如下：");
        // 循环遍历原数组
        for(int i=0;i<scores.length;i++) {
            // 将数组元素输出
            System.out.print(scores[i]+"\t");		//57    81    68    75    91  
        }
        // 定义一个新的数组，将 scores 数组中的 5 个元素复制过来
        // 同时留 3 个内存空间供以后开发使用
        int[] newScores = (int[])Arrays.copyOf(scores,8);
        System.out.println("\n复制的新数组内容如下：");
        // 循环遍历复制后的新数组
        for(int j=0;j<newScores.length;j++) {
            // 将新数组的元素输出
            System.out.print(newScores[j]+"\t");	//57    81    68    75    91    0    0    0
        }
    }
}
```



## 2、<span style="color:brown">**数学工具类Math：**</span>java.lang

### <!--与数学计算相关的工具类，用来实现数组的常见操作--> 

**2.1、Math工具类常用方法：**

- `public static double abs(double num)`
  - 获取绝对值；
- `public static double pow(double base, double exponent)`
  - 用于计算一个数的指定次幂
  - base为底数，exponent是指数
- `public static double sqrt(double num)`
  - 计算一个数的平方根
- `public static double ceil(double num)`
  - 向上取整；
  - 正数实质就是整数部分加1，无论小数点后面是否满足四舍五入；
  - 小数部分直接改为0；
- `public static double floor(double num)`
  - 向下取整；
  - 保留整数部分；
  - 正数实质就是小数部分添加零，无论小数点后面是否满足四舍五入；
- `public static long round(double num)`、`public static int round(float num)`
  - 四舍五入；
  - 数值+0.5，然后整体向下取整；
- `Math.PI`
  - 代表近似的圆周率常量

**2.2、关于ceil 与 floor在负数上取整的使用：**

```java
ceil:
	在原本的基础上直接保留整数部分，小数部分直接改成0
        -4.2            -4.0
        -5.0            -5.0
        
floor:
     在原有基础上，整数部分加1，小数部分直接改为0
         -4.5           -5.0
```

**2.3、计算在-10.8到5.9之间，绝对值大于6或者小于2.1的整数有多少个？**

```java
public class Practice {
    public static void main(String[] args) {
        int count = 0;
        double num1 = Math.ceil(-10.8); // -10
        double num2 = Math.floor(5.9); // 5.0
        for (int i = (int)num1; i < (int)num2; i++) {
            int num = Math.abs(i);
            if(num>6 || num<2.1){
                count++;
            }
        }
        System.out.println("最后得出个数为:"+count);
    }
}
```

