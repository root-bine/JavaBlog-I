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



## 2、<span style="color:brown">注意事项：</span>

1. 抽象类不能直接创建对象；
   
2. <span style="color:green">抽象类中可以有**构造方法和静态代码块**</span>；

3. 抽象类中可以没有抽象方法，但有抽象方法的类一定是抽象类；
4. 抽象类的子类，必须覆盖重写父类中所有的抽象方法；



## 3、<span style="color:brown">综合实例：</span>

**4.1、群主发红包：**

规则为：

1. 群主的一笔金额，从群主的余额中扣除，平均分成n份，让成员领取；
2. 成员领取红包后，保存在成员的余额中；

**4.2、题目分析：**

```java
1、有三个类，分别是:用户类、群主类、成员类；
    用户类：余额、用户名称
    群主类：发红包方法
    成员类：收红包方法
    
2、发红包方法
    返回值类型：ArrayList<Integer>
    方法名称：send
    参数列表：int money,int count
    
3、收红包方法
    返回值类型：void
    方法名称：receive
    参数列表：ArrayList<Integer> list
```

**4.3、具体代码实现：**

```java
public class User {
    private String name;
    private int money;
    // constructor、Getter and Setter
    public void show(){
        System.out.println("我叫:"+this.name+",余额是:"+this.money+"元");
    }
}
```

```java
public class Manager extends User{
    public Manager(String name, int money) {
        super(name, money);
    }
    public ArrayList<Integer> send(int money,int count){
        /*
        * 群主扣钱操作
        * */
        //首先需要一个集合存储若干红包的金额
        ArrayList<Integer> list = new ArrayList<>();
        //查看群主的余额
        int managerMoney = super.getMoney();
        if(money>managerMoney){
            System.out.println("余额不足！！！");
            //如果符合这个条件，返回一个空集合
            return list;
        }
        //扣钱就是重置余额
        super.setMoney(managerMoney-money);
        /*
        * 发红包操作
        * 需要平均拆分为count份
        * */
        //每一份的金额数
        int avg = money/count;
        //分完后的零头金额数。
        // 剩下的零头包在最后一个红包中
        int mod = money%count;
        //将每份金额放入集合中(最后一个单独处理)
        for (int i = 0; i < count-1; i++) {
            list.add(avg);
        }
        int last = avg+mod;
        list.add(last);
        return list;
    }
}
```

```java
public class Member extends User{
    public Member(String name, int money) {
        super(name, money);
    }
    public void receive(ArrayList<Integer> list){
        //从多个红包中随机抽取一个
        //获取一个随机的集合编号
        int index = new Random().nextInt(list.size());
        //根据索引，从集合中删除这个红包
        //然后将红包给自己
        int  removeMoney = list.remove(index);
        //查看自己的余额
        int memberMoney = super.getMoney();
        //重新设置自己的余额
        super.setMoney(memberMoney+removeMoney);
    }
}
```

```java
public class Demo01 {
    public static void main(String[] args) {
        Manager manager = new Manager("群主",100);
        Member member1 = new Member("成员A",0);
        Member member2 = new Member("成员B",0);
        Member member3 = new Member("成员C",0);
        manager.show();	//我叫:群主,余额是:100元
        member1.show();	//我叫:成员A,余额是:0元
        member2.show();	//我叫:成员B,余额是:0元
        member3.show();	//我叫:成员C,余额是:0元
        System.out.println("================");
        ArrayList<Integer> send = manager.send(20, 3);
        member1.receive(send);
        member2.receive(send);
        member3.receive(send);
        manager.show();	//我叫:群主,余额是:80元
        member1.show();	//我叫:成员A,余额是:8元
        member2.show();	//我叫:成员B,余额是:6元
        member3.show();	//我叫:成员C,余额是:6元
    }
}
```

