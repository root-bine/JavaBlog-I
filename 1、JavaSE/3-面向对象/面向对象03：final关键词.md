## 1、<span style="color:brown">final关键词概述：</span>

### <!--对于类、方法来说，abstract 和 final 关键词不能后同时出现-->

**1.1、含义：**

```apl
final代表的是: 最终的、不可变更的
```

**1.2、使用方法：**

- 类：使用final修饰的类是最终类，不能被继承。例如：`final class MyClass { }`
- 方法：使用final修饰的方法是最终方法，不能被子类重写。例如：`public final void myMethod() { }`
- 变量：使用final修饰的变量是常量，一旦赋值后就不能再修改。可以在声明时初始化，或在构造函数中初始化。例如：`final int myVariable = 10;`
- 参数：使用final修饰的方法参数表示该参数是只读的，不能在方法内部被修改。例如：`public void myMethod(final int param) { }`
- 局部变量：使用final修饰的局部变量表示该变量是只读的，不能再次赋值。例如：`final int myVariable = 10;`



## 2、<span style="color:brown">final修饰局部变量：</span>

**2.1、实例分析：**

```java
public class Demo01 {
    public static void main(String[] args) {
        int num1 = 20;
        System.out.println(num1);
        num1 = 30;
        System.out.println(num1);
        //一次赋值，终身不变
        final int  num2 = 100;
        System.out.println(num2);
        //错误写法
        //num2 = 200;
        //num2 = 100;
        Student stu1 = new Student("Java");
        stu1.getName();//Java
        //引用类型的地址值不可以改变
        //但是对象所指向对象的内容可以改变
        stu1.setName("PHP")//PHP
        //final修饰的引用类型变量的地址值，不可改变
        final Student stu2 = new Student("Python");
        stu2.getName();//Python
        //错误写法
        //stu2 = new Student("C++");
    }
}
```

**2.2、范围的定义：**

```Java
1.对于基本数据类型，不可变是指:
    变量的数值不可改变;
2.对于引用类型来，不可变是指:
    变量的地址值不可改变;
```



## 3、<span style="color:brown">final修饰成员变量：</span>

**3.1、使用：**

```java
1.由于成员变量具有默认值, 所以被final修饰后, 必须手动赋值(此时成员变量没有默认值了);
2.对于final的成员变量, 要么直接赋值, 要么构造方法赋值;
3.必须保证类中的所有重载方法, 都最终会对final的成员变量赋值;
```

**3.2、案例实现：**

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

   

