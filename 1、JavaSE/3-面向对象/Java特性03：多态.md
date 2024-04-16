## 1、<span style="color:brown">多态：</span>提高代码的灵活性、可扩展性

**1.1、概述&前提：**

多态是指：<span style="color:red">**<u>同一个方法调用可以在不同的对象上产生不同的行为</u>**</span>，其实现的前提分别为：<span style="color:green"><u>*继承、接口*</u></span>！！！

**1.2、实现方式：**🍔🍔🍔

<span style="color:green">**方法的重载**</span>和<span style="color:green">**方法的重写**</span>都是实现多态的方式，区别在于：

|      定义      | Overload是在同一个类中定义多个方法，它们具有相同的方法名但参数列表不同，<br>Override是在子类中重新定义父类中已有的方法，方法名和参数列表必须相同 |
| :------------: | :----------------------------------------------------------: |
| **发生的位置** | **Overload发生在同一个类中，可以在一个类中定义多个重载的方法，<br>Override发生在子类中，子类重写父类的方法** |
|    **关系**    | **Overload是在同一个类中对方法进行重载，不涉及继承关系，<br>Override是子类对父类方法的重写，涉及继承关系** |
|  **参数列表**  |   **OverLoad要求参数列表不同，Override的参数列表必须相同**   |
|  **返回类型**  | **Overload可以有相同或不同的返回值类型，<br>Override要求返回值类型相同，或是其子类** |
|    **功能**    | **OverLoad用于实现相似功能但参数不同的方法，<br>Override用于在子类中修改或扩展父类的方法实现** |

**1.3、分类：**✨✨✨

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

- 运行时多态（动态多态）：在程序运行时根据对象的实际类型确定调用的方法，而不是根据方法的声明类型，采用Override实现

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



## 3、<span style="color:brown">对象的转型：</span>

**3.1、对象转型格式：**

```java
// 向上转型
父类  对象名  =  new  子类();
```

```java
// 向下转型
子类 对象名 = (子类) 父类对象;
```

**3.2、向上转型详解：**

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

**3.3、向下转型详解：**

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



## 4、<span style="color:brown">instanceof关键词的使用：</span>

**4.1、使用格式：**

> 结果为一个**boolean类型的值**，判断：<u>对象</u>与<u>类型</u>是否相符合

```java
对象 instanceof 类名称;
```

**4.2、演示：**

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

