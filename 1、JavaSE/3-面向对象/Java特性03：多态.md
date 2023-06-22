## 1、<span style="color:brown">多态：</span>提高代码的灵活性、可扩展性

**1.1、多态的实现前提：**

| 继承 | 接口 |
| :--: | :--: |

**1.2、多态的实现方式：**🍔🍔🍔

<span style="color:green">**方法的重载**和**方法的重写**都是实现多态的方式</span>，区别在于：

|      定义      | Overload是在同一个类中定义多个方法，它们具有相同的方法名但参数列表不同，<br>Overwrite是在子类中重新定义父类中已有的方法，方法名和参数列表必须相同 |
| :------------: | :----------------------------------------------------------: |
| **发生的位置** | **Overload发生在同一个类中，可以在一个类中定义多个重载的方法，<br>Overwrite发生在子类中，子类重写父类的方法** |
|    **关系**    | **Overload是在同一个类中对方法进行重载，不涉及继承关系，<br>Overwrite是子类对父类方法的重写，涉及继承关系** |
|  **参数列表**  |  **OverLoad要求参数列表不同，Overwrite的参数列表必须相同**   |
|  **返回类型**  | **Overload可以有相同或不同的返回值类型，<br>Overwrite要求返回值类型相同，或是其子类** |
|    **功能**    | **OverLoad用于实现相似功能但参数不同的方法，<br>Overwrite用于在子类中修改或扩展父类的方法实现** |

**1.3、多态的分类：**✨✨✨

- 编译时多态（静态多态）：在编译阶段确定调用的方法，根据方法的声明类型来决定调用哪个方法，采用Overload方式实现

  ```java
  public class Example {
      public void print(int num) {
          System.out.println("Printing integer: " + num);
      }
      
      public void print(String str) {
          System.out.println("Printing string: " + str);
      }
      
      public static void main(String[] args) {
          Example example = new Example();
          example.print(10); // 编译时确定调用print(int num)方法
          example.print("Hello"); // 编译时确定调用print(String str)方法
      }
  }
  ```

- 运行时多态（动态多态）：在程序运行时根据对象的实际类型确定调用的方法，而不是根据方法的声明类型，采用Overwrite实现

  ```java
  public class Animal {
      public void makeSound() {
          System.out.println("Animal is making a sound");
      }
  }
  
  public class Dog extends Animal {
      @Override
      public void makeSound() {
          System.out.println("Dog is barking");
      }
  }
  
  public class Cat extends Animal {
      @Override
      public void makeSound() {
          System.out.println("Cat is meowing");
      }
  }
  
  public class Main {
      public static void main(String[] args) {
          Animal animal1 = new Dog();
          Animal animal2 = new Cat();
          
          animal1.makeSound(); // 运行时确定调用Dog类的makeSound方法
          animal2.makeSound(); // 运行时确定调用Cat类的makeSound方法
      }
  }
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



## 5、<span style="color:brown">对象的转型：</span>

**5.1、对象转型格式：**

```java
// 向上转型
父类  对象名  =  new  子类();
```

```java
// 向下转型
子类 对象名 = (子类) 父类对象;
```

**5.2、向上转型详解：**

- <span style="color:green">创建一个子类对象，然后当作一个父类对象来使用</span>；

- <span style="color:red">向上转型的过程是很安全的</span>：
  - 从小范围转换到大范围；

- <span style="color:red">一旦向上转型，子类就**无法调用自己特有的内容**</span>，只能使用：
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

**5.2、向下转型详解：**

- 用于将已经向上转型的，再次回归到自己的对象范围；
- 向下转型的过程是<u>不安全的</u>；
- **<span style="color:green">将父类对象还原成子类对象</span>**；

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
        Animal animal = new Cat();
        animal.eat();		//猫吃鱼
        animal.drink();		//喝水
        System.out.println("============");
        Cat cat = (Cat) animal;
        cat.eat();			//猫吃鱼
        cat.drink();		//喝水
        cat.Fish();			//鱼很好吃
    }
}
```



## 6、<span style="color:brown">instanceof关键词的使用：</span>

**6.1、使用格式：**

> 结果为一个**boolean类型的值**，判断：<u>对象</u>与<u>类型</u>是否相符合

```java
对象 instanceof 类名称;
```

**6.2、演示：**

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
        animal.eat();		//猫吃鱼
        animal.drink();		//喝水
        System.out.println("=======");
        if(animal instanceof Dog){
            Dog dog = (Dog)animal;
            dog.Shit();
        }
        if(animal instanceof Cat){
            Cat cat = (Cat)animal;
            cat.Fish();		//鱼很好吃
        }
    }
}
```



## 8、笔记本USB接口案例：

**8.1、笔记本电脑：**

```scss
1.笔记本通常具备使用USB设备的功能;
2.定义USB接口, 具备最基本的'开启功能和关闭功能';
3.鼠标和键盘要在电脑上使用, 那么'键盘和鼠标'必须要遵循USB规范，实现USB接口;
```

**8.2、案例分析：**

进行描述笔记本类，实现USB鼠标和USB键盘

```scss
USB接口, 包含: 打开设备功能、关闭设备功能;
笔记本类, 包含: 开机功能、关机功能、使用USB设备功能;
鼠标类, 要实现USB接口, 并且具备点击功能;
键盘类, 要实现USB接口, 并且具备敲击功能;
```

**8.3、案例实现：**

```java
public interface USB {
    public abstract void open();//打开设备
    public abstract void close();//关闭设备
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
public class DemoMain {
    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.powerOn();			//笔记本开机
        USB usb = new Mouse();
        computer.useUSB(usb);		//打开鼠标、鼠标点击、关闭鼠标
        USB keybroad = new Keybroad();
        computer.useUSB(keybroad);	//打开键盘、键盘输入、关闭键盘
        computer.powerOff();		//笔记本关机
    }
}
```

