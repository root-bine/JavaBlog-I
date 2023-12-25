## 1、<span style="color:brown">概述：</span>

**1.1、概述：**

final代表：最终的、不可变更的。对于类、方法来说，<span style="color:red">`static、abstract、final`不能同时出现</span>！！！

**1.2、使用方法：**

- 类：使用final修饰的类是最终类，<span style="color:red">不能被继承</span>；
- 方法：使用final修饰的方法是最终方法，<span style="color:red">不能被子类重写</span>；
- 成员变量：<span style="color:green">**使用final修饰的变量是常量，一旦赋值后就不能再修改**</span>：
  - 在声明时初始化；
  - 在构造函数中初始化；
- 局部变量：使用final修饰的局部变量表示该变量是<span style="color:blue">只读的，不能再次赋值</span>；
- 方法参数：使用final修饰的方法参数表示该参数是<span style="color:blue">只读的，不能在方法内部被修改</span>；

**1.3、final、finalize 和 finally 的不同之处?**😴😴😴

- `final`是一个修饰符，可以修饰变量、方法、类；
- `Java`技术允许使用`finalize()`方法<u>在垃圾收集器将对象从内存中清除出去之前</u>做必要的清理工作；
- `finally` 是一个关键字，与`try-catch 一起用于异常的处理；



## 2、<span style="color:brown">使用：</span>

**2.1、综合案例：**

```java
public class FinalExample {
    // final 修饰成员变量
    final int MAX_COUNT = 10;

    // final 修饰方法
    public final void printMaxCount() {
        System.out.println("MAX_COUNT: " + MAX_COUNT);
    }

    // final 修饰类
    final class InnerClass {
        // ...
    }

    public static void main(String[] args) {
        // final 修饰局部变量
        final int num = 100;
        System.out.println("num: " + num);

        // 尝试修改 final 变量的值会导致编译错误
        // num = 200;
    }
}
```

**3.2、成员变量赋值：**

1. 直接赋值：

   ```java
   public class Person {
       private  final String name = "Java";
       public String getName() {
           return name;
       }
       //由于是被final修饰，此处值不可发生值的改变
       //public void setName(String name) {
       //    this.name = name;
       //}
   }
   ```

2. 构造方法赋值：

   ```java
   public class Person {
       private  final String name;
       public Person() {
           name = "Java";
       }
       public Person(String name) {
           this.name = name;
       }
       public String getName() {
           return name;
       }
       //由于是被final修饰，此处值不可发生值的改变
       //public void setName(String name) {
       //    this.name = name;
       //}
   }
   ```

   

