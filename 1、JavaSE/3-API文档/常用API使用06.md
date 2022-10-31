# 常用API使用06

## <span style="color:purple">Object类中常用的toString( )和equals( )方法，由于直接使用没有价值，因此需要对这两个方法进行重写实现类中的属性进行进一步优化！！</span>

## 1、<span style="color:brown">**Object类：**</span>java.lang

- <span style="color:red">**Object类的地位：**</span>

  - <span style="color:orange">***Java语言中所有类的根类，也就是：所有类的父类；***</span>

  - <span style="color:orange">***如果一个类没有指明其父类，一般默认是Object类为父类；***</span>

- **Object类中常用两种方法:**

  - public String toString()-------->快捷键：Alt+Insert，找到toString( )方法

    - 返回对象的字符串表示；
    - <span style="color:violet">***主要作用就是获取对象的地址值；***</span>

    ```java
    public class Person {
        private String name;
        private int age;
    
        public Person() {
        }
        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
        public String getName() {
            return name;
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
    
  ```java
    public class Demo01 {
        public static void main(String[] args) {
            Person one = new Person("张三", 56);
            String str = one.toString();
            System.out.println(one);
            System.out.println(str);
        }
    }
  ```
  
    - <span style="color:green">***一般打印地址值是没有意义的，所以需要重写 Object类中的toString( )方法来打印类中的属性值；***</span>

    ```java
  public class Person {
        private String name;
        private int age;
        public Person() {
        }
        @Override
        //重写toString()方法
        public String toString() {
            return "Person(name="+name+",age="+age+")";
        }
        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
        public String getName() {
            return name;
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
  
    ```java
    public class Demo01 {
        public static void main(String[] args) {
            Person one = new Person("张三", 56);
            String str = one.toString();
            System.out.println(one);
            System.out.println(str);
        }
    }
    ```
  
    
  
    - <span style="color:orange">***检测是否重写Object的toString( )方法：{直接打印这个类对应的对象名}***</span>，结果如下:
      1. 如果没有重写了toString( )方法，则打印出来的结果就是对象的地址值；
      2. 如果重写了toString( )方法，则直接按照重写的格式打印即可；

  - public boolean equals(Object obj)
  
    - 此方法默认***比较两个对象的地址值是否相等***；
  
    ```java
    public class PersonAll {
        private String name;
        private int age;
        public PersonAll() {
        }
        public PersonAll(String name, int age) {
            this.name = name;
            this.age = age;
        }
        public String getName() {
            return name;
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
    
    ```java
    public class Demo02 {
        public static void main(String[] args) {
            PersonAll p1 = new PersonAll("Java", 120);
            PersonAll p2 = new PersonAll("Python", 80);
    
            boolean b = p1.equals(p2);
            System.out.println(b);
        }
    }
    ```
    
    
  
    - <span style="color:orange">***重写equals( )方法，实现比较两个对象的属性值是否相等；***</span>
      - 由于此处的方法括号内是Object类型，因此需要强制转换为Person类型【向下转型】
      - 类名    对象1  =   (类名)  对象2；
      - 引用类型变量 instanceof （类、抽象类或接口）
    
    ```java
    public class PersonAll {
        private String name;
        private int age;
        @Override
        public boolean equals(Object obj) {
            //把Object类型转换为PersonAll类型
            PersonAll p = (PersonAll)obj;
            //比较两个对象的属性
            boolean b = this.name.equals(p.name) && this.age == p.age;
            return b;
        }
        public PersonAll() {
        }
        public PersonAll(String name, int age) {
            this.name = name;
            this.age = age;
        }
        public String getName() {
            return name;
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
    
    ```java
    public class Demo02 {
        public static void main(String[] args) {
            PersonAll p1 = new PersonAll("Java", 120);
            PersonAll p2 = new PersonAll("Java", 120);
    
            boolean b = p1.equals(p2);
            System.out.println(b);
            System.out.println(p1);
            System.out.println(p2);
        }
    }
    ```
    
    
    
  - equals()方法使用快捷键的操作：Alt+Insert
  
    1. Template选择【IntelliJ Default】选项，然后一直next；
  
       ```java
       @Override
           public boolean equals(Object o) {
               if (this == o) {return true;}
               //getClass()!=o.getClass()使用了反射技术，判断o是否为			//PersonAll类型 等效于 obj instanceof PersonAll
               if (o == null || getClass() != o.getClass()) {
                   return false;
               }
               PersonAll personAll = (PersonAll) o;
               if (age != personAll.age) {
                   return false;
               }
               return name != null ? name.equals(personAll.name) : 			personAll.name == null;
           }
       ```
  
    2. Template选择【java(7+)】选项，然后一直next；<span style="color:red">{**建议使用**}</span>
  
       ```java
       @Override
           public boolean equals(Object o) {
               if (this == o) {
                   return true;
               }
               if (o == null || getClass() != o.getClass()) {
                   return false;
               }
               PersonAll personAll = (PersonAll) o;
               return age == personAll.age && Objects.equals(name, personAll.name);
           }
       ```

## 2、<span style="color:brown">Objects类：</span>java.util

## <span style="color:purple">Objects类中的方法是用区别于Object类，在Object类中一般是在其他类中重写toString( )方法和equals( )方法，然后主类中的对象进行调用</span>;<span style="color:orange">而Objects类中一般是采用：Objects.equals(Object a, Object b)!!!</span>

- **public static boolean equals( Object a, Object b)方法:**
  - 比较两个对象的<font color="red">**内容**</font>是否相同；
    - ***注意：***
      1. 如下列代码，假如使用boolean b = s1.equals(s2)，会出现一个NullPointException的空指针异常；
      2. <span style="color:green">***由于前面叙述的情况，在JDK7的时候加入了一个Objects的工具类，其中的方法是null-save（空指针安全）或null-tolerant（容忍空指针）的***</span>

```java
public class Demo03 {
    public static void main(String[] args) {
        String s1 = null;
        String s2 = "abc";
        boolean b = Objects.equals(s1, s2);
        System.out.println(b);
    }
}
```

## 2、<span style="color:brown">Objects类与Object类在异常与多线程</span>

### <!--注意：该类在异常章节，也有所涉及-->

**2.1、Objects类在异常处理章节的应用：**

```java
public static <T> T requireNonNull(T obj)
    查看引用对象是不是null;
```



**2.2、Object类在线程的等待唤醒机制中的应用：**

```java
public void wait()
    在其他线程调用该对象的notify方法前,导致当前线程等待;
```

```java
public void notify()
    1.唤醒此对象监视器上等待的单个线程;
	2.唤醒之后,继续执行wait之后的代码;
```

```java
public void wait(Long timeout)
    在其他线程调用notify或者notifyAll,或者指定时间结束前,线程处于休眠状态;
```

```java
public void notifyAll()
    唤醒在此等待监视器上所有的线程;
```



