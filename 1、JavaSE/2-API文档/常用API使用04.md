## 1、<span style='color:brown'> static关键词</span>

**1.1、static关键字作用：**

<span style="color:orange">一旦使用static关键词，此时所赋值的内容**不再归于对象所有，由所在类主导**</span>！！！

**1.2、static关键词修饰成员变量:**

1. 成员变量一旦被static修饰，那么此变量只能有所在类主导使用!!

2. 多个对象共享同一份数据！！

3. <font color='red'>一般IDEA不允许用对象调用static修饰的成员变量，但可以使用类名调用该变量</font>；

**1.3、static关键词修饰成员方法:**

- 一旦方法被static修饰，那么该方法就成为了**静态方法**，随所在类一起加载!!

- <font color='red'>**IDEA不允许直接使用类创建的对象调用类中的静态方法，建议使用：类名.静态方法( );**</font>
- <font color='red'>**对于本类自己中调用静态方法，可以省去类名调用，直接使用方法名；**</font>

```java
public class StaticDemo02 {
    public static void main(String[] args) {
        MyClass one = new MyClass();
        one.method();
        MyClass.methodStatic();
        methodCall();
    }
    public static void methodCall(){
        System.out.println("自己的方法!!");
    }
}
```

```java
public class MyClass {
    public void method(){
        System.out.println("这是一个普通成员方法!!");
    }
    public static void methodStatic(){
        System.out.println("这是一个静态方法!!");
    }
}
```

**1.4、static关键词的注意事项：**

> <font color='bulue'>**在内存中是先有静态，之后才有非静态**</font>！！！

- **静态不能够直接访问非静态**；
  
- **静态方法中不能使用this关键字**；



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

```java
public class Main {　　
    static int value = 33;
    public static void main(String[] args) throws Exception{
        new Main().printValue();
    }
    private static void printValue(){
        int value = 3;
        System.out.println(this.value);
    }
}
```

<u>所有的</u>*静态方法和静态变量*都可以通过对象访问，**只要访问权限足够**。

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

- 找到main方法入口，main方法是程序入口，但在执行main方法之前，要先加载Test类

- 加载Test类的时候，发现Test类继承Base类，于是先去加载Base类

- 加载Base类的时候，发现Base类有static块，而是先执行static块，输出**base static**结果

- Base类加载完成后，再去加载Test类，发现Test类也有static块，而是执行Test类中的static块，输出**test static**结果

- Base类和Test类加载完成后，然后执行main方法中的new Test()，<u>调用子类构造器之前会先调用父类构造器</u>

- 调用父类构造器，输出**base constructor**结果

- 然后再调用子类构造器，输出**test constructor**结果

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

- 找到main方法入口，main方法是程序入口，但在执行new MyClass()之前，要先加载Test类
- 加载Test类的时候，发现Test类有static块，而是先执行static块，输出**test static**结果
- 加载MyClass类，发现MyClass类有static块，而是先执行static块，输出myclass static结果
- 然后调用MyClass类的构造器生成对象，在生成对象前，需要先初始化父类Test的成员变量，而是执行Person person = new Person("Test")代码，发现Person类没有加载
- 加载Person类，发现Person类有static块，而是先执行static块，输出**person static**结果
- 接着执行Person构造器，输出**person Test**结果
- 然后调用父类Test构造器，输出**test constructor**结果，这样就完成了父类Test的初始化了
- 再初始化MyClass类成员变量，执行Person构造器，输出**person MyClass**结果
- 最后调用MyClass类构造器，输出**myclass constructor**结果，这样就完成了MyClass类的初始化了

**3.4、这段代码的输出结果是什么？**

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
