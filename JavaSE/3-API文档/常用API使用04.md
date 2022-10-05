# 常用API使用04

## 1、<span style='color:brown'>**static 静态变量和静态方法:**</span>

- **static静态关键字作用：**

- <span style="color:orange">一旦使用static关键词，此时所赋值的内容**不再归于对象所有，由所在类主导！！**</span>

- **static关键词修饰成员变量:**

  1. 成员变量一旦被static修饰，那么此变量只能有所在类主导使用!!

  2. 多个对象共享同一份数据！！

  3. <font color='red'>一般IDEA不允许用对象调用static修饰的成员变量，但可以使用类名调用该变量</font>

```java
public class StaticDemo01 {
    public static void main(String[] args) {
        Student one = new Student("郭靖", 50);
        Student two = new Student("黄蓉", 56);
        Student.room = "101教室";
        System.out.println("姓名"+one.getName()+
                ",年龄"+one.getAge()+",所在教室"+Student.room+
                ",学号"+one.getId());
        System.out.println("姓名"+two.getName()+
                ",年龄"+two.getAge()+",所在教室"+Student.room+
                ",学号"+two.getId());
    }
}
```

```java
public class Student {
    /**姓名*/
    private String name;
    /**年龄*/
    private int age;
    /**所在教室*/
    static String room;
    private int id;
    private static int idCounter = 0;
    public Student(){
        this.id = ++idCounter;
    }
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        this.id = ++idCounter;
    }

    public String getName() {
        return name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

- **static关键词修饰成员方法:**

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

- **static关键词的注意事项:**

  - <font color='red'>**静态不能够直接访问非静态**</font>
    - 原因：<font color='bulue'>**因为在内存中是先有静态，之后才有了非静态；**</font>

  - <font color='blue'>**静态方法中不能使用this关键字**</font>

## 2、<span style='color:brown'>**static静态代码块:**</span>

- **格式:**

  public   class   类名称{

  ​		static{

  ​			//静态代码块内容

  ​		}

  } 

-  **具体作用:**

  - <span style='color:orange'>当第一次用到本类时,**静态代码块执行唯一的一次**,而***第二次使用该类的时候静态代码块就不再执行了***;</span>

  - ***静态内容总是优先于非静态内容***，因此：<span style='color:red'>**静态代码块比构造方法先执行**</span>;

```java
public class StaticDemo03 {
    public static void main(String[] args) {
        Person one = new Person();
        Person two = new Person();
        
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

