# <span style="color:violet">super关键字主要是用于访问父类的内容！！</span>

## 1、<span style="color:brown">第一种用法：</span>

<span style="color:orange">在子类成员方法中，**访问父类的成员变量**</span>;

```java
//父类
public class Fu {
   int num = 10;
}
//子类
public class Zi extends Fu{
   int num = 20;
   public void method(){
       System.out.println("This Zi is "+this.num+"years old, but Fu is "+super.num+"years old!!!");
   }
}
//测试类
public class Demo02 {
    public static void main(String[] args) {
        Zi zi = new Zi();
        System.out.println(zi.num);
        Fu fu = new Zi();
        System.out.println(fu.num);
        Fu fu1 = new Fu();
        System.out.println(fu1.num);
        zi.method();

    }
}
```

## 2、<span style="color:brown">第二种用法：</span>

<span style="color:red">在子类成员方法中，**访问父类的成员方法**</span>;

```java
//父类
public class Fu {
   int num = 10;
   public void method(){
      System.out.println("父类方法");
   }
}
//子类
public class Zi extends Fu{
   int num = 20;
   @Override
   public void method(){
       super.method();
       System.out.println("This Zi is "+this.num+" years old, but Fu is "+super.num+" years old!!!");
   }
}
//测试类
public class Demo02 {
    public static void main(String[] args) {
        Zi zi = new Zi();
        zi.method();
    }
}
```

## 3、<span style="color:brown">第三种用法：</span>

<span style="color:green">在子类构造方法中，**访问父类的构造方法**</span>;

```java
详见：Java特性02:继承
```

