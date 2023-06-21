## 1、<span style='color:brown'>封装：</span>隐藏内部实现细节，只暴露必要的接口给外部使用

**1.1、概述：**

利用<u>抽象数据类型</u>将<u>数据和基于数据的操作</u>封装在一起，使其构成一个不可分割的**独立实体**。

**1.2、优点：**

|          减少耦合          |         可以独立地开发、测试、优化、使用、理解和修改         |
| :------------------------: | :----------------------------------------------------------: |
|     **减轻维护的负担**     | **可以更容易被程序员理解，并且在调试的时候可以不影响其他模块** |
|     **有效地调节性能**     |         **可以通过剖析确定哪些模块影响了系统的性能**         |
|   **提高软件的可重用性**   |                              /                               |
| **降低构建大型系统的风险** |  **即使整个系统不可用，但是这些独立的模块却有可能是可用的**  |

**1.3、使用详解：**

1、一个方法相当于封装一个功能；

2、private修饰也算是封装的作用；

- <span style='color:orange'>**private关键字详解：**</span>

  1. **private的使用：**一旦私有化，外界不能够进行修改，只能通过内部设置的方法进行部分操作！！

     ```java
     private String name;
     private int age;
     ```
     

​			补充：<font color="#0099ff">快捷键：Alt+Insert--------->生成Getter和Setter方法</font>

- **private使用的过程注意：**

  在使用boolean类型时，如果要设置Getter和setter方法，<span style='color:violet'>Getter方法一般写成isXxx的形式</span>，<span style='color:red'>Setter方法一般写成setXxx的形式</span>。



## 2、<span style='color:brown'>**this关键字详解：**</span>

**2.1、关键词this的使用：**

当<u>方法的局部变量与类中的成员变量重名</u>时，根据“就近原则”，优先使用局部变量。

如果需要访问本类当中的成员变量，格式：this.成员变量名。

```java
public class text{
    int age;
    public void show(int num){
        System.out.println("你的年龄是:"+num+",他的年龄是:"+this.age);
    }
}
```

**2.2、this关键词的作用：**

<span style='color:red'>通过谁调用的方法，谁就是this</span>；

<span style='color:orange'>this关键词主要是区分局部变量在与成员变量重名时，会出现局部变量覆盖成员变量的情况</span>；



## 3、<span style='color:brown'>**构造方法：**</span>

**3.1、格式：**

```java
public 类名称（参数列表）｛
		方法体;
｝
```

**3.2、演示：**

<span style='color:green'>**在构造方法的主体中，this关键词可以省略！！！**</span>

```java
public class DEMO {
    private String name;
    public DEMO(String s_name) {
        name = s_name;
    }
}
---------------------------------------------------------------------------------------------------------
public class DEMO {
    private String name;
    public DEMO(String s_name) {
        this.name = s_name;
    }
}
```

**3.3、注意事项：**

- <span style='color:red'>构造方法的名称与类名称一致</span>；

- <span style='color:red'>构造方法不能写返回值，void也不需要</span>；

- <span style='color:red'>构造方法中不能够return一个具体的返回值</span>；

- <span style='color:red'>如果方法中没有具体的构造方法，但是会有一个默认的构造方法</span>；

- <span style='color:green'>构造方法也可以重载，即：名称相同，参数列表不同</span>；
