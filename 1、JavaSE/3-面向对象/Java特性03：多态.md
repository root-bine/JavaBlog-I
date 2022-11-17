## 1、<span style="color:brown">多态的概述：</span>

<span style="color:violet">**继承的extends或者接口的implements的实现，是多态的前提！！！**</span>

<span style="color:orange">**多态性主要是针对于对象来说！！！**</span>

<span style="color:green">**方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性！！！**</span>

```apl
现在有三个类，Student、Employee、Humen。
    其中，Humen是其他两个类的父类。
    对于Student类来说，创建的对象，既有学生的形态，也有人的形态。
```



## 2、<span style="color:brown">多态的格式与使用：</span>

### 代码中体现多态性，一般是：**父类引用指向子类对象**！！

**2.1、格式：**类似于前期学习继承中，父类与子类的成员方法重名规律讲解

```java
//类与类
父类  对象名  =  new  子类();
```

```java
//类与接口
接口  对象名  =  new  实现类();
```

**2.2、使用：**

```java
public class Fu {
    public void method(){
        System.out.println("父类方法！！");
    }
    public void show(){
        System.out.println("父类独有方法！！！");
    }
}
```

```java
public class Zi extends Fu{
    @Override
    public void method() {
        System.out.println("子类方法!!");
    }
}
```

```java
public class Demo01Multi {
    public static void main(String[] args) {
        Fu obj = new Zi();
        //子类方法!!
        obj.method();
        //父类独有方法！！！
        obj.show();
    }
}
```



## 3、<span style="color:brown">多态中成员变量的使用特点：</span>

### 在继承中，父类与子类的成员变量重名一般采用：对象直接调用、成员方法调用！！！

1. 子类对象直接调用：

   ```java
   public class Fu {
       int num = 10;
   }
   ```

   ```java
   public class Zi extends Fu{
       int num = 20;
   }
   ```

   ```java
   public class Demo01Multi {
       public static void main(String[] args) {
           Fu obj = new Zi();
           //获取的结果:10
           System.out.println(obj.num);
       }
   }
   ```

2. 成员方法调用：

   ```java
   public class Fu {
       int num_Fu = 50;
       public void method(){
           System.out.println("父类中的成员变量"+this.num_Fu);
       }
   }
   ```

   ```java
   public class Zi extends Fu{
       int num_Zi = 100;
       public void test(){
           System.out.println("子类的成员变量"+this.num_Zi);
       }
   }
   ```

   ```java
   public class Demo01Multi {
       public static void main(String[] args) {
           Fu obj1 = new Zi();
           Zi obj2 = new Zi();
           Fu obj3 = new Fu();
           //父类中的成员变量:50
           obj1.method();
           //子类的成员变量:100
           obj2.test();
           //父类中的成员变量:50
           obj3.method();
       }
   }
   ```



## 4、<span style="color:brown">多态中成员方法的使用特点：</span>

### 在继承中，针对于父类与子类的方法重名，一般是：new  类名称(  )，是谁就是调用的谁的成员方法！！！

````java
public class Fu {
    public void method(){
        System.out.println("父类的成员方法");
    }
    public void method_Fu(){
        System.out.println("父类特有方法");
    }
}
````

```java
public class Zi extends Fu{
    @Override
    public void method(){
        System.out.println("子类的成员方法");
    }
    public void method_Zi(){
        System.out.println("子类特有方法");
    }
}
```

```java
public class Demo01Multi {
    public static void main(String[] args) {
        Fu obj1 = new Zi();
        //子类的成员方法
        obj1.method();
        
        Fu obj2 = new Fu();
        //父类的成员方法
        obj2.method();
    }
}
```



## 5、<span style="color:brown">多态总结：</span>

```apl
如果不使用多态，只是用子类:
Teacher one = new Teacher();
one.work();//授课
Assistant two = new Assistant();
two.work();//辅导
```

```apl
使用多态的情况:
Employee one = new Teacher();
one.work();//授课
Employee two = new Assistant();
two.work();//辅导
```

### <span style="color:violet">好处：无论右边new的是哪个子类对象，左边调用方法始终不变！！！</span>



## 6、<span style="color:brown">对象的转型：</span>

**6.1、对象的向上转型：**

```java
父类  对象名  =  new  子类();
```

1. <span style="color:green">创建一个子类对象，然后当作一个父类对象来使用；</span>
2. <span style="color:red">向上转型的过程是很安全的；</span>
   - 从小范围转换到大范围；
3. <span style="color:red">一旦向上转型，子类就无法调用自己特有的内容</span>，只能使用：
   - 在子类中覆盖重写的抽象类的抽象方法；
   - 子类继承的父类方法；

```java
public abstract class Animal {
    public abstract void eat();
}
```

```java
public class Cat extends Animal{
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }
}
```

```java
public class Demo01Multi {
    public static void main(String[] args) {
        //这里是把猫的对象看成动物
        //从小范围，换到了大范围
        Animal animal = new Cat();
        //猫吃鱼
        animal.eat();
    }
}
```

**6.2、对象的向下转型：**

### 主要是用于将已经向上转型的，再次回归到自己的对象范围！！ 

```java
子类 对象名 = (子类) 父类对象;
```

**<span style="color:green">将父类对象还原成子类对象</span>**

```java
public abstract class Animal {
    public abstract void eat();
    public void drink(){
        System.out.println("喝水");
    }
}
```

```java
public class Cat extends Animal{
    public void Fish(){
        System.out.println("鱼很好吃");
    }
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }
}
```

```java
public class Demo01Multi {
    public static void main(String[] args) {
        //这里是把猫的对象看成动物
        //从小范围，换到了大范围
        //此时，animal对象只能调用父类的专属内容
        Animal animal = new Cat();
        animal.eat();
        animal.drink();
        System.out.println("============");
        //向下转型，将猫还原成猫所属的范围
        //此时，猫的对象又可以调用自己特有的内容
        Cat cat = (Cat) animal;
        cat.eat();
        cat.drink();
        cat.Fish();
    }
}
```

```apl
猫吃鱼
喝水
===========
猫吃鱼
喝水
鱼很好吃
```



## 7、<span style="color:brown">instanceof关键词的使用：</span>

**7.1、使用格式：**

### 最后得到一个boolean类型的值，判断：【对象】与【类型】是否相符合

```java
对象 instanceof 类名称;
```

**7.2、具体使用：**

```java
public abstract class Animal {
    public abstract void eat();
    public void drink(){
        System.out.println("喝水");
    }
}
```

```java
public class Dog extends Animal{
    public void Shit(){
        System.out.println("SHIT SO BAD");
    }
    @Override
    public void eat() {
        System.out.println("狗吃SHIT");
    }
}
```

```java
public class Cat extends Animal{
    public void Fish(){
        System.out.println("鱼很好吃");
    }
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }
}
```

```java
public class Demo02Multi {
    public static void main(String[] args) {
        Animal animal = new Cat();
        animal.eat();
        animal.drink();
        System.out.println("=======");
        if(animal instanceof Dog){
            Dog dog = (Dog)animal;
            dog.Shit();
        }
        if(animal instanceof Cat){
            Cat cat = (Cat)animal;
            cat.Fish();
        }
    }
}
```

```apl
猫吃鱼
喝水
=======
鱼很好吃
```



## 8、笔记本USB接口案例：

**8.1、笔记本电脑：**

```apl
1.笔记本通常具备使用USB设备的功能;
2.定义USB接口，具备最基本的 开启功能和关闭功能;
3.鼠标和键盘要在电脑上使用，那么电脑和鼠标必须要遵循USB规范，实现USB接口;
```

**8.2、案例分析：**

进行描述笔记本类，实现USB鼠标和USB键盘

```apl
USB接口，包含:打开设备功能、关闭设备功能;
笔记本类，包含:开机功能、关机功能、使用USB设备功能;
鼠标类，要实现USB接口，并且具备点击功能;
键盘类，要实现USB接口，并且具备敲击功能;
```

**8.3、案例实现：**

```java
public interface USB {
    public abstract void open();//打开设备
    public abstract void close();//关闭设备
}
```

```java
public class Computer{
    public void powerOn(){
        System.out.println("笔记本开机");
    }
    public void powerOff(){
        System.out.println("笔记本关机");
    }
    //使用USB设备
    public void useUSB(USB usb){
        usb.open();
        if(usb instanceof Mouse){
            Mouse mouse = (Mouse)usb;
            mouse.click();
        }else if(usb instanceof Keybroad){
            Keybroad keybroad = (Keybroad) usb;
            keybroad.type();
        }
        usb.close();
    }
}
```

```java
public class Mouse implements USB{
    @Override
    public void open() {
        System.out.println("打开鼠标");
    }
    @Override
    public void close() {
        System.out.println("关闭鼠标");
    }
    public void click(){
        System.out.println("鼠标点击");
    }
}
```

```java
public class Keybroad implements USB{
    @Override
    public void open() {
        System.out.println("打开键盘");
    }
    @Override
    public void close() {
        System.out.println("关闭键盘");
    }
    public void type(){
        System.out.println("键盘输入");
    }
}
```

```java
public class DemoMain {
    public static void main(String[] args) {
        //创建一个笔记本对象
        Computer computer = new Computer();
        computer.powerOn();
        //准备一个鼠标
        Mouse mouse = new Mouse();
        //首先向上转型(多态写法)
        USB usb = new Mouse();
        //将鼠标的usb参数传入笔记本电脑
        computer.useUSB(usb);
        //准备一个键盘
        //首先向上转型(普通写法)
        USB keybroad = new Keybroad();
        //将键盘的对象传入笔记本电脑
        computer.useUSB(keybroad);
        computer.powerOff();
    }
}
```

```apl
笔记本开机
打开鼠标
鼠标点击
关闭鼠标
打开键盘
键盘输入
关闭键盘
笔记本关机
```

