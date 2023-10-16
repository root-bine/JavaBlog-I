## 1、<span style="color:brown">抽象：</span>定义一组规范和约束，使得对象的行为和属性更加统一和规范

**1.1、抽象方法：**

```Java
修饰符 abstract 返回值类型 方法名称(参数列表) {
	//...
}
```

**1.2、抽象类：**

>  抽象方法所在的类，必须是抽象类
>
> 接口是一种完全抽象的类，它只包含抽象方法和常量的声明，没有任何实现

```java
public abstract class 类名称 {
    //...
}
```

**1.3、具体使用：**

具体操作：

1. <span style="color:violet">不能直接创建抽象对象</span>；

2. <span style="color:violet">必须要使用一个子类来继承这个父类</span>；

3. <span style="color:violet">子类必须覆盖**重写**父类中所有的抽象方法</span>；

```java
public abstract class Animal {
    public abstract void eat();
}
```

```java
public class Cat extends Animal{
    @Override
    public void eat() {
        System.out.println("猫吃鱼！！");
    }
}
```

```java
public class Demo01 {
    public static void main(String[] args) {
        Cat cat = new Cat();
        cat.eat();
    }
}
```



## 2、<span style="color:brown">接口和抽象的区别？</span>🎋🎋🎋

- 接口里只包含<u>抽象方法、静态方法、默认方法、私有方法（无普通方法）</u>，而抽象类则完全包含；
- 接口里只能定义静态常量，而<u>抽象类可以定义普通成员变量和静态常量</u>；
- 接口里不包含构造器、代码块，抽象类里可以包含；

<span style="color:red">本质上，接口体现的是一种规范，抽象类体现的是一种模板式设计</span>！！！


