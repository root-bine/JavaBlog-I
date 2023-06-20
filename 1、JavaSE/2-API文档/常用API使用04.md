## 1、<span style='color:brown'> static关键词</span>：<font color='bulue'>**在内存中是先有静态，之后才有非静态**</font>！！！

**1.1、static关键词修饰成员变量:**

- <span style='color:red'>**静态性**</span>：`static`修饰的成员变量属于类本身，而不是属于类的实例；
- <span style='color:green'>共享性</span>：由于静态成员变量只有一份拷贝，所以它被所有类的实例共享；
- <span style='color:green'>生命周期</span>：静态成员变量的生命周期与类的生命周期相同，它在类加载时被初始化，直到程序结束或类被卸载时才会销毁；

- 可以<span style='color:purple'>**通过类名直接访问**</span>；
- 可以<span style='color:green'>用于共享数据或常量</span>；

**1.2、static关键词修饰成员方法:**

- <span style='color:red'>**静态性**</span>：`static`修饰的成员方法属于类本身，而不是属于类的实例；

- 可以<span style='color:purple'>**通过类名直接访问**</span>；

- <span style='color:violet'>不能访问非静态成员</span>：静态方法只能访问静态成员（包括静态成员变量和静态方法），这是因为<u>**非静态成员是属于类的实例的**</u>；

- <span style='color:violet'>不能被重写</span>：当子类中定义一个与父类具有相同名称和参数列表的静态方法时，并不是重写父类的静态方法，而是在子类中定义了一个新的静态方法；

- <span style='color:violet'>可以访问静态成员</span>：静态方法可以直接访问类的静态成员变量和静态方法，无需通过实例来访问；

```java
public class MathUtils {
    public static final double PI = 3.14159;

    public static double multiply(double num1, double num2) {
        return num1 * num2 * PI;
    }
}
/*静态方法multiply通过类名MathUtils直接访问了静态变量PI, 并将其用于计算结果*/
public class Main {
    public static void main(String[] args) {
        double result = MathUtils.multiply(2, 3);
        System.out.println(result); // 输出：18.84954
    }
}
```



## 2、<span style='color:brown'>**static静态代码块:**</span>

**2.1、格式:**

```java
public class 类名称{
    static{
        //静态代码块内容
    }
}
```

**2.2、作用:**

- <span style='color:orange'>**静态代码块只执行一次**，*第二次使用该类的时候静态代码块就不再执行了*</span>；

- ***静态内容总是优先于非静态内容***，因此：<span style='color:red'>**静态代码块比构造方法先执行**</span>；

```java
public class StaticDemo03 {
    public static void main(String[] args) {
        Person one = new Person();
    }
}
```

```java
public class Person {
    static {
        System.out.println("静态代码块执行！！");
    }
    public Person() {
        System.out.println("构造方法执行！！");
    }
}
```



## 3、<span style='color:brown'>**static面试合集:**</span>

**3.1、静态变量能被this访问吗？**

​		<span style='color:red'>**不能**</span>。`this`关键字是一个引用，它指向当前对象的实例。而静态变量是属于类本身的，不依赖于任何对象的实例。

**3.2、下列代码的结果：**

```java
public class Test extends Base{
    static{
        System.out.println("test static");
    }
    public Test(){
        System.out.println("test constructor");
    }
    public static void main(String[] args) {
        new Test();
    }
}
class Base{
    static{
        System.out.println("base static");
    }
    public Base(){
        System.out.println("base constructor");
    }
}
```

分析：

- 首先，程序执行`main`方法，创建一个`Test`对象。

- 在创建`Test`对象之前，由于`Test`类继承自`Base`类，所以会先加载`Base`类。
- 在加载`Base`类时，会执行静态代码块`static{}`，输出`base static`。
- 接着，创建`Base`对象，调用`Base`类的构造方法，输出`base constructor`。
- `Base`对象创建完成后，回到`Test`类的创建过程。
- 在创建`Test`对象时，会执行静态代码块`static{}`，输出`test static`。
- 接着，调用`Test`类的构造方法，输出`test constructor`。
- `Test`对象创建完成，程序执行结束。

**3.3、以下代码输出结果：**

```java
public class Test {
    Person person = new Person("Test");
    static{
        System.out.println("test static");
    }
    public Test() {
        System.out.println("test constructor");
    }
    public static void main(String[] args) {
        new MyClass();
    }
}
class Person{
    static{
        System.out.println("person static");
    }
    public Person(String str) {
        System.out.println("person "+str);
    }
}
class MyClass extends Test {
    Person person = new Person("MyClass");
    static{
        System.out.println("myclass static");
    }
    public MyClass() {
        System.out.println("myclass constructor");
    }
}
```

分析：

- 首先，程序执行`main`方法，创建一个`MyClass`对象。
- 在创建`MyClass`对象之前，由于`MyClass`类继承自`Test`类，所以会先加载`Test`类。
- 在加载`Test`类时，会执行静态代码块`static{}`，输出`test static`。
- 接着，创建`Test`对象，调用`Test`类的构造方法。
- 在创建`Test`对象时，会先执行`Test`类中的实例变量初始化，即`Person person = new Person("Test")`。这会调用`Person`类的构造方法。
- 在加载`Person`类时，会执行静态代码块`static{}`，输出`person static`。
- 接着，调用`Person`类的构造方法，输出`person Test`。
- `Person`对象创建完成后，回到`Test`类的构造方法，输出`test constructor`。
- `Test`对象创建完成后，回到`MyClass`类的创建过程。
- 在创建`MyClass`对象时，会执行静态代码块`static{}`，输出`myclass static`。
- 接着，创建`MyClass`对象，调用`MyClass`类的构造方法。
- 在创建`MyClass`对象时，会先执行`MyClass`类中的实例变量初始化，即`Person person = new Person("MyClass")`。这会调用`Person`类的构造方法。
- 在加载`Person`类时，由于已经加载过，所以不会再执行静态代码块。
- 接着，调用`Person`类的构造方法，输出`person MyClass`。
- `Person`对象创建完成后，回到`MyClass`类的构造方法，输出`myclass constructor`。
- `MyClass`对象创建完成，程序执行结束。

**3.4、以下代码输出结果：**

```java
public class Test {
    static{
        System.out.println("test static 1");
    }
    public static void main(String[] args) {

    }
    static{
        System.out.println("test static 2");
    }
}
```

分析：

- 虽然main方法中没有任何功能执行，但是被static修饰的方法会随着JVM的启动，而加载进入内存
- static块可以出现类中的任何地方【<u>只要不是任何方法内部</u>】，并且执行是按照static块的顺序执行的
