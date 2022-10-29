# final关键字讲解

## 1、<span style="color:brown">final关键词概述：</span>

**1.1、含义：**

```apl
final代表的是: 最终的、不可变更的
```

**1.2、使用方法：**

```apl
1.修饰一个类;
2.修饰一个方法;
3.修饰局部变量;
4.修饰成员变量;
```

## 2、<span style="color:brown">final修饰一个类：</span>

**2.1、格式：**

```apl
public final class 类名称{
	//...
}
```

**2.2、含义：**

```apl
这个类不能有任何子类, 但有父类存在（实在没有，一定有Object作为父类）
```

<span style="color:red">**2.3、注意事项：**</span>

1. **被final修饰的类，其中的所有方法都不能被覆盖重写;**

2. **被final修饰的类，可以覆盖重写自身继承父类的方法;**

## 3、<span style="color:brown">final修饰一个方法：</span>

**3.1、格式：**

```apl
修饰符 final 返回值类型 方法名称(参数列表){
	//方法体
}
```

**3.2、含义：**

```apl
当一个方法被final修饰, 这个方法就是最终方法, 即:这个方法不能被覆盖重写！！！
```

**3.3、注意事项：**

<span style="color:red">对于类、方法来说， **abstract  和  final  关键词不能后同时出现**</span>

## 4、<span style="color:brown">final修饰局部变量：</span>

**4.1、实例分析：**

```java
public class Demo01 {
    public static void main(String[] args) {
        int num1 = 20;
        System.out.println(num1);
        num1 = 30;
        System.out.println(num1);
        //一次复制，终身不变
        final int  num2 = 100;
        System.out.println(num2);
        //错误写法
        //num2 = 200;
        //num2 = 100;
        Student stu1 = new Student("Java");
        stu1.getName();//Java
        //引用类型的地址值不可以改变
        //但是对象所指向对象的内容可以改变
        stu1.setName("PHP")//PHP
        //final修饰的引用类型变量的地址值，不可改变
        final Student stu2 = new Student("Python");
        stu2.getName();//Python
        //错误写法
        //stu2 = new Student("C++");
    }
}
```

**4.2、范围的定义：**

```Java
1.对于基本数据类型，不可变是指:
    变量的数值不可改变;
2.对于引用类型来，不可变是指:
    变量的地址值不可改变;
```

## 5、<span style="color:brown">final修饰成员变量：</span>

**5.1、使用：**

```java
1.由于成员变量具有默认值, 所以被final修饰后, 必须手动赋值(此时成员变量没有默认值了);
2.对于final的成员变量, 要么直接赋值, 要么构造方法赋值;
3.必须保证类中的所有重载方法, 都最终会对final的成员变量赋值;
```

**5.2、案例实现：**

1. 直接赋值：

   ```java
   public class Person {
       private  final String name = "Java";
       public String getName() {
           return name;
       }
       //由于是被final修饰，此处值不可发生值的改变
       //public void setName(String name) {
       //    this.name = name;
       //}
   }
   ```

2. 构造方法赋值：

   ```java
   public class Person {
       private  final String name;
       public Person() {
           name = "Java";
       }
       public Person(String name) {
           this.name = name;
       }
       public String getName() {
           return name;
       }
       //由于是被final修饰，此处值不可发生值的改变
       //public void setName(String name) {
       //    this.name = name;
       //}
   }
   ```

   

