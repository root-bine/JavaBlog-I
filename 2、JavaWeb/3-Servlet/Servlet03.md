# Servlet补充

## 1、<span style="color:brown">Servlet4.0注解配置：</span>

**1.1、创建JavaWeb项目有两种方式：**

1. 【有web.xml】创建一个普通的Java项目，然后添加Web其他部分：

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Web%E5%88%9B%E5%BB%BA01.png" alt="image-20220609171556751" style="zoom: 67%;" />

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Web%E5%88%9B%E5%BB%BA02.png" alt="image-20220609171712691" style="zoom:67%;" />

2. 【无web.xml】创建一个Java Enterprise项目，在定义好项目的Name、Location、Project template之后，下一个界面勾选Servlet即可：

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Web%E5%88%9B%E5%BB%BA03.png" alt="image-20220609171905890" style="zoom:67%;" />

   ![image-20220609171954222](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Web%E5%88%9B%E5%BB%BA04.png)

   

**1.2、Servlet4.0配置：**<span style="color:red">设置WebServlet注解，就可以不需要再配置web.xml文件中的Servlet配置</span>

在Servlet接口的实现类中加入注解：`@WebServlet(...)`

![image-20220609172244798](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Web%E9%85%8D%E7%BD%AE%E5%8F%82%E6%95%B0.png)

范例：

```java
@WebServlet(name = "FilterServlet", urlPatterns = "/FilterServlet",
initParams = {@WebInitParam(name = "password",value = "123"),
			  @WebInitParam(name = "name",value = "value")}
			)
public class ServletDemo implements Servlet{
    ...
}
```



## 2、<span style="color:brown">Servlet体系结构：</span>

**2.1、Servlet体系组成：**

```apl
HttpServlet抽象类  extends  GenericServlet抽象类   implements  Servlet接口
```

**2.2、GenericServlet抽象类：**

GenericServlet实现类Servlet接口，但是**只保留Servlet中的抽象方法**。

<u>对于其他的Servlet接口中的抽象方法，都做了空方法构造</u>。

**2.3、HttpServlet：**

> <span style="color:blue">**对HTTP协议的一种封装**</span>

1. 定义一个类，继承HttpServlet抽象类；
2. 覆盖重写`doGet()/doPost()方法`；
3. 通过浏览器直接访问，是调用的doGet()方法；

**2.4、Servlet访问路径：**

一个servlet可以定义多个访问路径：

```apl
//使用{}将多个路径囊括在一起
@WebServlet({"/servlet02","/aaa","/bbb"})
```

对于urlPatterns，有相应的定义规则：

1. 完全路径匹配：

   ```apl
   /aaa
   ```

2. 目录匹配：

   ```apl
   1. /aaa/bbb
   2. /aaa/*      在浏览器中，输入完/aaa之后，输入任意值, 都可以访问到资源类
   3. /*		   直接在浏览器中的'/'之后输入随意值, 都可以访问到资源类
   ```

3. 扩展名匹配：

   ```apl
   1. *.do
   2. *.action
   3. *.jsp
   ...
   ```

   