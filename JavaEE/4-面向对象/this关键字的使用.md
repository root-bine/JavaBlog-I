# this关键字的使用总结

# <span style="color:purple">this关键字主要是用于访问本类的内容！！</span>

## 1、<span style="color:brown">第一种用法：</span>

<span style="color:orange">在本类成员方法中，**访问本类的成员变量**</span>;

```java
//父类
public class Fu {
   int num = 10;
}
//子类
public class Zi extends Fu{
   int num = 20;
   public void method(){
       System.out.println("本类的num为:"+this.num);
       System.out.println("父类的num为:"+super.num);
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

## 2、<span style="color:brown">第二种用法：</span>

<span style="color:orange">在本类成员方法中，**访问本类的另一个成员方法**</span>;

```java
//父类
public class Fu {
   int num = 10;
}
//子类
public class Zi extends Fu{
   int num = 20;
   public void method(){
       System.out.println("本类的num为:"+this.num);
       System.out.println("父类的num为:"+super.num);
   }
   public void methodA(){
       System.out.println("这是A方法！");
   }
   public void methodB(){
       this.methodA();
       System.out.println("这是B方法！");
   }
}
//测试类
public class Demo02 {
    public static void main(String[] args) {
        Zi zi = new Zi();
        zi.method();
        zi.methodB();
    }
}
```

## 3、<span style="color:brown">第三种用法：</span>

<span style="color:orange">在本类的构造方法中，**访问本类的另一个构造方法**</span>;

- this（....）调用也必须是构造方法的第一句，也是唯一一个；
- this 和 super 两种构造调用，不能同时出现。因此，super( )这种无参调用就不再赠送；

```java
//父类
public class Fu {
   int num = 10;
}
//子类
public class Zi extends Fu{
   int num = 20;
    
   public Zi() {
        //本类的无参构造调用有参构造
        this(100);
    }

   public Zi(int num) {
        this.num = num;
   }
    
   public void method(){
       System.out.println("本类的num为:"+this.num);
       System.out.println("父类的num为:"+super.num);
   }
   public void methodA(){
       System.out.println("这是A方法！");
   }
   public void methodB(){
       this.methodA();
       System.out.println("这是B方法！");
   }
}
//测试类
public class Demo02 {
    public static void main(String[] args) {
        Zi zi = new Zi();
        zi.method();
        zi.methodB();
    }
}
```

