# 常用API使用05

## <span style="color:violet">***注意：Arrays和Math都是直接用自己的类名调用方法，无法创建对象！！！！***</span>

## 1、<span style="color:brown">**数组工具类Arrays：java.util**</span>

- **Arrays工具类的作用:**
  - <span style="color:orange">**与数组相关的工具类，用来实现数组的常见操作**</span>；

- **Arrays工具类的常用方法:**
  - public   static   String   toString(数组)
    - <span style="color:red">**参数数组----->String；**</span>
    - 默认格式：[元素1，元素2，元素3  .....]
  
  - public   static   void   sort(数组)
  
    - 对数组元素进行排序；
  
    - <span style="color:red">**排序规则:**</span>
      - <span style="color:green">**如果是数值，按照升序从小到大排序；**</span>
      - <span style="color:green">**如果是字符串，按照字母升序排列；**</span>
      - <span style="color:green">**如果是自定义类型，那么这个自定义的类就需要有**Compareable或者Comparetor接口**的支持；**</span>
    
  - public static int[] copyOf  (int[] original,   int newLength)
  
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
    
    - 原理：
    
      - `传回的数组是新的数组对象，改变传回数组中的元素值，不会影响原来的数组`；
    
      - int[] original ：新的数组对象的前身；
    
      -  int newLength：新的数组对象的长度；
  
- **对象数据的拷贝：**

  - 源码：

    ```java
    public static native void arraycopy(Object src,  int  srcPos,
                                        Object dest, int destPos,
                                        int length);
    ```

  - 原理：

    - Object src：原数组
    - int  srcPos：原数组的起始位置
    - Object dest：目标数组
    - int destPos：目标数组的起始位置
    - int length：要复制的长度

---

**题目1:**请使用Arrays相关的API，将一个随机字符串中的所有字符进行排序，然后**倒序**打印出来；（此处暂时不用Random，后续学完StringBuilder再进行编写）

- <span style="color:red">**String------->数组：str.toCharArray( );**</span>

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

**题目2：**实现删除数组中某一元素的算法

本题的详细要求如下：

1) 当给定一个数组和该数组中的某一元素的位置时，利用算法将该数组中的该位置的元素删除。如: 有一个数组，其元素内容为 {2,3,4,5,6}，当删除位置为1的元素后，该数组的元素内容更改为{2,4,5,6}。

2) 上述描述中的算法，要求通过一个方法来实现，该方法的声明如下所示：int[] remove(int[] ary, int index){}

​			上述方法声明中:

​					参数ary引用的是原数组；

​					参数index表示想要删除的元素的位置；

​					返回值为删除掉指定位置元素后的数组；

```java
public class Test {
    public static void main(String []args){
        int number[]={2,3,4,5,6};
        int[] remove = remove(number, 1);
        System.out.println(Arrays.toString(remove));
    }
    public static int [] remove(int[] arr,int index){
        /*for(int i=index;i<arr.length-1;i++){
            arr[i]=arr[i+1];
        }*/
        arr=Arrays.copyOf(arr, arr.length-1);
        return arr;
    }
}
```




## 2、<span style="color:brown">**数学工具类Math：java.lang**</span>

- **Math工具类的作用:**
  - <span style="color:orange">**与数学计算相关的工具类，用来实现数组的常见操作；**</span>
- **Math工具类常用方法:**

  - public   static   **double**   abs  (double   num)
    - 获取绝对值，有多重重载；
  - public   static   **double**   ceil  (double   num)
    - 向上取整；
    - 正数实质就是整数部分加1，无论小数点后面是否满足四舍五入；
    - 小数部分直接改为0；
  - public   static   **double**   floor  (double   num)
    - 向下取整；
    - 保留整数部分；
    - 正数实质就是小数部分添加零，无论小数点后面是否满足四舍五入；
  - public   static   **long**   round  (double   num)
    - 四舍五入；
    - 只是判断小数点后一位的数值，大于5进1，小于5不变；

**解释：关于ceil 与 floor在负数上取整的使用**

```java
ceil:
	在原本的基础上直接保留整数部分，小数部分直接改成0
        -4.2            -4.0
        -5.0            -5.0
        
floor:
     在原有基础上，整数部分加1，小数部分直接改为0
         -4.5           -5.0

```

### 范例

```java
public class Demo01 {
    public static void main(String[] args) {
        System.out.println("===取绝对值===");
        System.out.println(Math.abs(-12));
        System.out.println(Math.abs(0));
        System.out.println(Math.abs(3.14));
        System.out.println("===向上取整===");
        System.out.println(Math.ceil(3.9));
        System.out.println(Math.ceil(0.9));
        System.out.println("===向下取整===");
        System.out.println(Math.floor(31.5));
        System.out.println(Math.floor(32.0));
        System.out.println(Math.floor(36.9));
        System.out.println("===四舍五入===");
        System.out.println(Math.round(4.15));
        System.out.println(Math.round(4.6));
        System.out.println(Math.round(4.46));
    }
}
```

```java
===取绝对值===
12
0
3.14
===向上取整===
4.0
1.0
===向下取整===
31.0
32.0
36.0
===四舍五入===
4
5
4
```

- **Math工具类中的常见常量：**
  - 调用方法：Math.PI
    - 代表近似的圆周率常量

```java
package MathPractice;
public class Demo02 {
    public static void main(String[] args) {
        System.out.println(Math.PI);
    }
}
```

- 练习题:
  - 题目:
    - 计算在-10.8到5.9之间，绝对值大于6或者小于2.1的整数有多少个？

```java
public class Practice {
    public static void main(String[] args) {
        int count = 0;
        double num1 = Math.ceil(-10.8);
        double num2 = Math.floor(5.9);
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

