# Spring学习01

## 1、<span style="color:brown">Spring简介：</span>

**1.1、Spring是什么？**

![image-20220713195755742](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E7%9A%84%E5%90%AB%E4%B9%89.png)

**1.2、Spring的优势：**

- **<u>方便解耦，简化开发</u>**

  ```apl
  1. 通过Spring提供的loC容器，可以将对象间的依赖关系交由Spring进行控制，避免硬编码所造成的过度耦合
  2. 用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用
  ```

- **<u>AOP编程的支持</u>**

  ```apl
  通过Spring的AOP功能，方便进行面向切面编程，许多不容易用传统OOP实现的功能可以通过AOP轻松实现。
  ```

- **<u>声明式事务的支持</u>**

  ```apl
  可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活的进行事务管理，提高开发效率和质量。
  ```

- <u>**方便编程测试**</u>

  ```apl
  可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情。
  ```

- **<u>方便集成各种优秀的框架</u>**

  ```apl
  Spring对各种优秀框架(Struts、Hibernate、Hessian、Quartz等)的支持。
  ```

**1.3、Spring体系结构：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84.png" alt="image-20220713210421314" style="zoom:80%;" />



## 2、<span style="color:brown">Spring快速入门：</span>

**2.1、Spring开发过程：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E5%BC%80%E5%8F%91%E8%BF%87%E7%A8%8B.png" alt="image-20220713211729193" style="zoom:67%;" />

**2.2、Spring开发步骤：**

![image-20220713212350023](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E5%BC%80%E5%8F%91%E6%AD%A5%E9%AA%A4.png)

## 3、<span style="color:brown">Spring配置文件：</span>

**3.1、Bean标签的基本配置：**

![image-20220714162905400](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E6%A0%87%E7%AD%BE%E7%9A%84%E9%85%8D%E7%BD%AE.png)

**3.2、Bean标签使用范围：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E6%A0%87%E7%AD%BE%E8%8C%83%E5%9B%B4.png" alt="image-20220714163159434" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E6%A0%87%E7%AD%BE%E8%AF%A6%E8%A7%A3.png" alt="image-20220714173026482" style="zoom: 80%;" />

**3.3、Bean标签的生命周期：**

![image-20220714173811514](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E6%A0%87%E7%AD%BE%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

**3.4、Bean的依赖注入概念：**

![image-20220714185840501](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%E6%A6%82%E5%BF%B5.png)

**3.5、Bean依赖注入的方式：**

<u>怎么将UserDao注入到UserService内部中？</u>

1. 构造方法

   ```apl
   在UserServiceImpl中定义:private UserDao userdao, 然后构建UserServiceImpl中定义有参和无参构造。然后再applicationContext.xml文件中配置:
   <bean id="userService" class="UserServiceImpl的全类名">
   	<!--name是UserServiceImpl中的有参构造函数的参数名称-->
   	<!--ref的值是UserDaoImpl类在配置文件中的id标识名称-->
   	<constructor name="userDao" ref="userDao"></constructor>
   </bean>
   ```

2. set方法

   ```apl
   在UserServiceImpl中定义:private UserDao userdao, 然后构建setter方法:setUserDao。之后再applicationContext.xml中进行配置:
   <bean id="userService" class="UserServiceImpl的全类名">
   	<!--name是构造的setter方法去掉set前缀,保留后面部分且首字母小写-->
   	<!--ref的值是UserDaoImpl类在配置文件中的id标识名称-->
   	<property name="userDao" ref="userDao"/>
   </bean>
   ```

**3.6、Bean依赖注入的数据类型：**

- 普通数据类型

  ```apl
  在UserDaoImpl中编写两个变量:String name、int age,然后对这两个变量构建getter和setter方法。同时将本类中的save方法内容编写为:System.out.println(name+"--->"+age)。之后再配置文件中,编写如下配置:
  <bean id="userDao" class="UserDaoImpl全类名">
  	<property name="name" value="zhangsan"/>
  	<property name="age" value="18"/>
  </bean>
  ```

- 引用数据类型：

  ```apl
  类似于将UserDao注入到UserService中
  ```

- 集合数据类型

  在UserDaoImpl类中，创建如下内容：

  ![image-20220714201920350](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E6%B3%A8%E5%85%A5%E9%9B%86%E5%90%88%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B01.png)

  然后在类中的save方法中打印这几个变量，之后到applicationContext.xml文件中进行注入配置：

  **<u>List集合：</u>**

  ![image-20220714202150949](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E6%B3%A8%E5%85%A5%E9%9B%86%E5%90%88%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%8402.png)

  **<u>Map集合，其中key是普通数据类型，value是引用了一个User类的属性：</u>**

![image-20220714202611494](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E6%B3%A8%E5%85%A5%E9%9B%86%E5%90%88%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B03.png)

**<u>Properties类型：</u>**

![image-20220714202739196](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Bean%E6%B3%A8%E5%85%A5%E9%9B%86%E5%90%88%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B04.png)

## 4、<span style="color:brown">Spring相关API：</span>

```java
ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
// UserDao userdao = app.getBean(UserDao.class);
UserDao userdao = (UserDao) app.getBean("userDao");
userdao.save();
```

**4.1、ApplicationContext实现类：**

```apl
# 从类的根路径下, 加载配置文件【推荐】
ClassPathXmlApplicationContext
------------------------------------------------------------------------------------------------------------
# 从磁盘路径下, 加载配置文件
FileSystemXmlApplicationContext
```

**4.2、`getBean()`方法：**

![image-20220714204439687](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/getBean%E6%96%B9%E6%B3%95%E4%BD%BF%E7%94%A8.png)

1. <u>当***参数的数据类型是字符串***时，表示根据Bean的id从容器中获得Bean实例，返回是Object，**需要强转**</u>。
2. 当***参数的数据类型是Class类型***时，表示根据类型从容器中匹配Bean实例，当容器中相同类型的Bean有多个时，则此方法会报错。
