## 1、<span style="color:brown">继承：</span>实现代码的重用和扩展

### <!--继承是多态的前提之一-->

- 继承关键词：extends；

- 类与类之间的关系定义：父类、子类；
- <span style="color:red">**继承的核心**</span>：<span style="color:">**共性抽取**</span>
  - 子类继承父类的所有**可继承的“内容”**；
  - 子类除了父类继承的内容以外，还可以拥有**自己的专属内容**；



## 2、<span style="color:brown">继承格式：</span>

***父类格式定义：***

```java
public class 父类名称 {   
	......
}
```

***子类格式定义：***

```java
public class 子类名称 extends 父类名称 {
    ......
}
```



## 3、<span style="color:brown">继承中成员变量的访问特点：</span>

1、创建子类对象时，成员变量不重名：

​	**父类成员变量输出情况：**只显示父类的成员变量，不会出现子类的内容；

​	**子类成员变量输出情况：**既显示自己的的内容，也显示父类的内容;

```java
public class Fu {
    int numFu = 20;
}
```

```java
public class Zi extends Fu{
    int numZi = 10;
}
```

```java
public class Text02 {
    public static void main(String[] args) {
        Fu fu = new Fu();
        System.out.println(fu.numFu);
        System.out.println("========");
        Zi zi = new Zi();
        System.out.println(zi.numZi);
        System.out.println(zi.numFu);
    }
}
```

2、创建子类对象时，成员变量重名：

- <span style="color:red">直接通过**子类对象**访问成员变量</span>;

  - <span style="color:green">等号左边是谁，就优先用谁</span>;
  - <span style="color:green">如果没有，则向上找</span>;

  ```java
  public class Fu {
      int numFu = 20;
      int num = 100;
  }
  ```

  ```java
  public class Zi extends Fu{
      int numZi = 10;
      int num = 200;
  }
  ```

  ```java
  public class Text02 {
      public static void main(String[] args) {
          //等号左边是谁，就优先用谁
          Zi zi = new Zi();
          System.out.println(zi.num);
          System.out.println("===========");
          Fu fu = new Zi();
          System.out.println(fu.num);
  
      }
  }
  ```

- <span style="color:red">间接通过**成员方法**访问成员变量</span>;

  - <span style="color:green">该方法属于谁，就优先用谁</span>;
  - <span style="color:green">如果没有，则向上查找</span>;

  ```java
  public class Fu {
      int numFu = 20;
      int num = 100;
      public void  methodFu(){
          System.out.println(num);
      }
  }
  ```

  ```java
  public class Zi extends Fu{
      int numZi = 10;
      int num = 200;
      public void methodZi(){
          System.out.println(num);
      }
  }
  ```

  ```java
  public class Text02 {
      public static void main(String[] args) {
          Fu fu = new Fu();
          Zi zi = new Zi();
          //200
          zi.methodZi();
          //100
          zi.methodFu();
          fu.methodFu();
      }
  }
  ```



## 4、<span style="color:brown">区分子类方法中重名的三种变量：</span>

<span style="color:red">父类与子类重名的三种变量，子类中调用三种变量的访问方式</span>：

**本类的局部变量：**<span style="color:blue">直接编写</span>;

**本类的成员变量：**<span style="color:blue">this.成员变量</span>;

**父类的成员变量：**<span style="color:blue">super.成员变量</span>;

```java
public class Fu {
    int num = 10;
}
```

```java
public class Zi extends Fu{
    int num = 20;
    public void method(){
        //本类的局部变量
        int num = 30;
        System.out.println(num);
        //本类的成员变量
        System.out.println(this.num);
        //在子类中调用父类的重名成员变量，关键词:super
        System.out.println(super.num);
    }
}
```

```java
public class Text {
    public static void main(String[] args) {
        Zi zi = new Zi();
        zi.method();
    }
}
```



## 5、<span style="color:brown">继承中成员方法的访问特点：</span>

**在父子类的继承关系中，创建子类对象，访问成员方法的规则：**

1. <span style="color:red">方法不重名的情况：</span>

   - 子类既可以调用自己的成员方法，也可以调用父类的成员方法;

   ```java
   //父类
   public class Fu {
       public void methodFu(){
           System.out.println("父类方法执行！！");
       }
   }
   
   //子类
   public class Zi extends Fu{
       public void methodZi(){
           System.out.println("子类方法执行！！");
       }
   }
   
   //测试类
   public class Demo01 {
       public static void main(String[] args) {
           Zi zi = new Zi();
           zi.methodFu();
           zi.methodZi();
       }
   }
   ```

2. <span style="color:red">方法重名情况：</span>

   - <span style="color:green">创建的对象是谁，谁就优先使用(即：new 类名)</span>;
   - <span style="color:green">如果没有，则向上查找</span>;

   ```java
   //父类
   public class Fu {
       public void method(){
           System.out.println("父类重名方法执行！！！！");
       }
   }
   
   //子类
   public class Zi extends Fu{
       @Override
       public void method(){
           System.out.println("子类重名方法执行！！！！");
       }
   }
   
   //测试类
   public class Demo01 {
       public static void main(String[] args) {
           Zi zi = new Zi();
           zi.method();
           Fu fu = new Zi();
           fu.method();
           Fu fu1 = new Fu();
           fu1.method();
       }
   }
   ```

## 注意：<span style="color:violet">无论是成员方法还是成员变量，如果没有都是向上查找父类，绝不会向下查找子类！！！</span>



## 6、<span style="color:brown">覆盖重写（Overwrite）：</span>

**6.1、概念与特点：**

**重写的概念**：也称为<span style="color:orange">**覆盖、覆写**</span>

- 一般重写的方法前面会带有：@Override
  - 检测是否是一个有效的覆盖重写;
  - 这相当于一个标签;
- <span style="color:red">在继承关系中，方法名称一样，参数列表也一样！！</span>
  - 区分于Overload（重载）：方法名称一样，但参数列表不一样;

**重写的特点：**

- <span style="color:orange">**如果创建的是子类对象，则优先使用子类方法**</span>;
  - 即：new 类名，就优先使用该类的方法;

**6.2、注意事项：**

1. <span style="color:orange">**必须保证父类与子类之间，方法名称相同，参数列表相同**</span>;

2. <span style="color:orange">**子类方法的返回值  <=  父类返回值范围**</span>;
3. <span style="color:orange">**子类方法的权限(修饰符)  >=  父类的权限(修饰符)**</span>;
   - public > protected > (default) >private
     - <span style="color:red">(default)不是关键词default，而是什么都不写，留空</span>;

**6.3、应用场景：**		

```java
//旧手机
public class Phone {
    public void call(){
        System.out.println("打电话");
    }
    public void send(){
        System.out.println("发消息");
    }
    public void show(){
        System.out.println("显示号码");
    }
}
```

```java
//新手机
public class NewPhone extends Phone{
    @Override
    public void show() {
        super.show();
        System.out.println("显示头像");
        System.out.println("显示照片");
    }
}									                                                                                                 
```

```java
//测试类
public class Demo01 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        phone.call();
        phone.send();
        phone.show();
        System.out.println("=========");
        NewPhone newPhone = new NewPhone();
        newPhone.call();
        newPhone.send();
        newPhone.show();
    }
}
```



## 7、<span style="color:brown">继承中构造方法的访问：</span>

**7.1、继承关系中，父类与子类构造方法的访问特点：**

1. 子类构造方法中，有一个默认的**super( )**<span style="color:green">[子类中一般不会显示]</span>调用。因此，<span style="color:red">先调用父类构造，再调用子类构造</span>；

   ```java
   //父类
   public class Fu {
       public Fu() {
           System.out.println("父类构造方法");
       }
   }
   //子类
   public class Zi extends Fu{
       public Zi() {
           /**子类构造方法中默认的d父类无参构造方法
             *此类语句必须是该方法中的第一句！！！！！
             *super();------->一般子类中不显示
             *子类调用父类有参数的构造函数*/
           System.out.println("子类构造方法");
       }
   }
   //测试类
   public class Demo02 {
       public static void main(String[] args) {
           Zi zi = new Zi();
       }
   }
   ```

2. 通过super关键词，可以实现父类中**有参数的构造函数**调用；

   ```java
   //父类
   public class Fu {
       public Fu(int num) {
           System.out.println("父类有参构造方法");
       }
       public Fu() {
           System.out.println("父类无参构造方法");
       }
   }
   //子类
   public class Zi extends Fu{
       public Zi() {
           /**子类构造方法中默认的掉用父类无参构造方法
             *此语句必须是该方法中的第一句！！！！！
             */
           super(10);//如果此处不加入数字，调用的就是父类无参构造
           System.out.println("子类构造方法");
       }
   }
   //测试类
   public class Demo02 {
       public static void main(String[] args) {
           Zi zi = new Zi();
       }
   }
   ```

3. <span style="color:red">**super的父类构造调用，必须是子类构造方法中的第一句！！！！**</span>

   - <span style="color:green">**只有子类构造方法才能够调用父类的构造方法**</span>；
   - <span style="color:green">对于super( ) 与 super(参数)，子类的**同一个构造方法中只能出现一个**</span>;

**7.2、总结：子类调用父类构造方法：**

- 如果调用<span style="color:orange">**父类的无参构造方法**</span>，子类只需要继承父类即可。因为子类中有默认的 `super()` 构造；
- 如果调用父类的<span style="color:orange">**有参构造方法**</span>，子类中必须编写 `super(参数)` 构造；
- <span style="color:violet">**super( )构造只能有一个，且必须是子类构造方法中的第一个**</span>！！



## 8、<span style="color:brown">Java继承的特点：</span>

1. <span style="color:orange">单继承的</span>，<span style="color:violet">一个类的直接父类只能是一个</span>；

   ```xml
   class A{}
   class B extends A{} //正确
   class C{}
   class D extends C,A{} //错误
   ```

2. <span style="color:orange">多级继承</span>；

   ```xml
   class A{}
   class B extends A{} //正确
   class C extends B{} //正确
   ```

3. <span style="color:green">继承关系是is-a关系</span>；

   - 继承关系表示一个类是另一个类的一种特殊类型，例如，狗是动物的一种，学生是人的一种；
   - 使得子类对象可以被当作父类对象使用，实现了Java的多态特性；
