## 1、<span style="color:brown">SpringMVC简介：</span>

**1.1、什么是MVC？**
MVC是一种软件架构思想，将软件按照**模型、视图控制器**来划分。因此，<u>***SpringMVC是首选为JavaEE的表述层开发的首选方案！！***</u>

SprinMVC是在<span style="color:blue">**基于java实现MVC设计模式的web框架**</span>，提高代码的复用性简,降低程序耦合度,简化开发，所<span style="color:brown">**设计出的web框架**</span>！！！

```apl
M: Module(模型层)指工程中的JavaBean,用来处理数据库映射数据
	JavaBean分为两类:
		一类是实体类Bean: 专门存储业务数据的,如: Student、User
		一类是业务逻辑Bean: 指Service与Dao类,用来处理业务之间的逻辑和数据访问
------------------------------------------------------------------------------------------------------------
V: View(视图层) 指html与jsp等页面,用于与用户进行交互,展示数据
------------------------------------------------------------------------------------------------------------
C: Controller(控制层) 指的是Servlet程序,用于接收客户端发送的请求与响应浏览器
```

**1.2、SpringMVC的特点：**

- Spring家族的原生产品，与IOC等基础设施无缝衔接
- **基于原生的Servlet**，通过<u>**功能强大的前端控制器DispatcherServlet**</u>,对请求和响应进行统一处理



## 2、<span style="color:brown">SpringMVC快速入门：</span>

**2.1、SpringMVC开发步骤：**

![image-20220723143734543](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8.png)

**2.2、代码及其操作：**

首先，需要在pom.xml文件中，<u>导入`spring-context坐标`、`spring-webmvc坐标`、`spring-web坐标`、`servlet-api坐标`、``javax.servlet.jsp-api坐标`</u>。

然后，在web.xml文件中，进行如下配置：

```xml
<!--配置SpringMVC的映射路径, 即:配置SpringMVC的核心控制器DispatcherServlet-->
<servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--这里是告知前端控制器,spring-mvc.xml文件的位置-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <!--此处的访问路径为缺省的状态-->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

然后在webapp目录下创建一个JSP文件夹，放入success.jsp文件：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>success</title>
</head>
<body>
    <h1>spring-MVC启动成功...</h1>
</body>
</html>
```

**<u>在真正的企业开发过程中，一般为了便于维护，因此对不同的内容所需要的XML文件，都会进行分开单独创建</u>**，因此spring-mvc.xml文件内容如下：【<span style="color:red">由于基本的xml文件是没有context命名空间的，因此需要单独加上</span>】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--Controller的组件扫描-->
    <context:component-scan base-package="com.zgy.controller"></context:component-scan>
    <!--配置内部资源视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/JSP/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

最后创建一个Controller包，在其中创建一个UserController类，内容如下：

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @RequestMapping("/quack")
    public String save(){
        System.out.println("Controller save running...");
        // 重定向 或者 转发
        return "success";
    }
}
```

结果展示：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E5%90%AF%E5%8A%A8%E7%BB%93%E6%9E%9C.png" alt="image-20220723154750997" style="zoom:67%;" />

**2.3、程序执行流程：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B.png" alt="image-20220723160212369" style="zoom:150%;" />

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E6%B5%81%E7%A8%8B%E5%9B%BE.png" alt="image-20220723160431143" style="zoom:80%;" />



## 3、<span style="color:brown">SpringMVC组件解析：</span>

**3.1、SpringMVC的各部分组件执行流程：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E7%BB%84%E4%BB%B6%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B.png" alt="image-20220723161140021" style="zoom:80%;" />

<span style="color:orange"><u>**处理器Handler，本质上就是后去在Controller包中的内容，也常为后端控制器**</u></span>

**3.2、@RequestMapping注解解析：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/RequestMapping%E6%B3%A8%E8%A7%A3.png" alt="image-20220723165437647" style="zoom:80%;" />

如果在<u>类上面也有RequestMapping注解</u>，那么此时如果进行**重定向操作**，就需要设置为：`return “/xxx.jsp”`，即：在重定向的内容前面加上`/`。

按照上述设置，内容更新如下：

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @RequestMapping(value = "/quack",method = RequestMethod.GET,params = {"username"})
    public String save(){
        System.out.println("Controller save running...");
        // 重定向 或者 转发
        return "/success.jsp";
    }
}
```

那么在浏览器进行访问时，就需要输入以下内容才能实现正常访问：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/RequestMapping%E7%9A%84%E5%BA%94%E7%94%A8.png" alt="image-20220723170402007" style="zoom:80%;" />

**3.3、spring-mvc.xml之组件扫描：**

```apl
<context:component-scan base-package="被扫描的包位置"></context:component-scan>
---------------------------------------------------------------------------------------------------------
如果"被扫描的包位置"是一个很大的范围,并没有指定的包进行扫描,那么可以在其中进行更深一步操作:
	1. <context:exclude-filter type="annotation" expression=""/>	:	扫描排除该指定目标
	2. <context:include-filter type="annotation" expression=""/>	:	扫描该指定的目标
```

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E7%9B%B8%E5%85%B3%E8%AE%BE%E7%BD%AE.png" alt="image-20220723195005606" style="zoom:80%;" />



**3.4、spring-mvc.xml之视图解析器：**

针对于UserController类中的`return "/success.jsp"`，其含义是重定向。因此，此处省略了部分内容：

```apl
return "/success.jsp"
---------------------------------------------------------------------------------------------------------
# 转发
return "forward:/success.jsp"
---------------------------------------------------------------------------------------------------------
# 重定向
return "redirect:/success.jsp"
```

如果在webapp目录下创建一个JSP文件夹，将success.jsp文件存放到其中，那么UserController的写法为：

```apl
return "/JSP/success.jsp"
```

如果后期不想编写`"/JSP/success.jsp"`，可以在spring-mvc.xml文件中配置**内部资源视图解析器**，最终只需要return "success"即可已完成重定向：

```xml
<!--配置内部资源视图解析器-->
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    	<!--前缀-->
        <property name="prefix" value="/JSP/"/>
    	<!--后缀-->
        <property name="suffix" value=".jsp"/>
</bean>
```

***<u>综上所述：</u>***

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%A7%86%E5%9B%BE%E8%A7%A3%E6%9E%90%E5%99%A8.png" alt="image-20220723210743094" style="zoom:80%;" />

