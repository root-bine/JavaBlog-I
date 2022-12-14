# Servlet基础

### <!--Servlet是一个接口, 定义了Java类被服务器【Tomcat】识别的规则-->

## 1、<span style="color:brown">基础操作：</span>

**1.1、概述：**

服务器资源分为：静态资源、动态资源。

而动态资源需要由<u>**Java代码编写逻辑性**</u>来进行维持，因此动态资源的运作依赖于一个Java类。

但是这个Java类，需要<u>依赖于服务器才能够运行</u>，所以它需要<span style="color:red">遵循一定的规则【接口】，才能被服务器所识别</span>！！

**1.2、Servlet接口方法：**

```java
public abstract void service(ServletRequest req, ServletResponse res)
```

```java
public abstract void init(ServletConfig config)  
```

```java
public abstract void destroy() 
```

```java
public abstract ServletConfig getServletConfig()  
```

```java
public abstract String getServletInfo()  
```

**1.3、快速入门：**

1. 创建一个Java类，实现Servlet接口

2. 实现接口中的抽象方法

   - 此处只需要对`service()`方法进行修改；

     ```java
     @Override
     public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws                  ServletException, IOException {
                  System.out.println("Hello Servlet!!");
            }
     ```

3. 配置Servlet：在web.xml文件中进行`Servlet配置和路径映射`

   ```xml
   <!--配置Servlet-->
       <servlet>
           <servlet-name>Demo01</servlet-name>
           <servlet-class>Servlet.ServletDemo01</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>Demo01</servlet-name>
           <url-pattern>/servlet</url-pattern>
       </servlet-mapping>
   ```

**测试结果：**

浏览器页面能够正常运行，输入：`http://localhost:8080/TomcatProgram_war_exploded/servlet1`，页面没有显示内容。

但是在IDEA浏览器控制台，显示出：`Hello Servlet!!`



## 2、<span style="color:brown">Servlet执行原理：</span>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Servlet%E6%89%A7%E8%A1%8C%E5%8E%9F%E7%90%86.png" alt="image-20220608172303588" style="zoom:67%;" />

```scss
1. 当服务器接收到客户端的请求时, 会解析'请求的URL路径', 从而获取访问Servlet的资源路径;
2. 查找web.xml文件, 是否有与资源路径一致的<url-pattern>标签内容;
3. 如果存在, 则Tomcat会将<servlet-class>中全类名对应的字节码文件加载入内存;
4. 创建对象, 重写并调用Servlet接口中的抽象方法;
```



## 3、<span style="color:brown">web.xml文件配置：</span>

```xml
<web-app 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
						http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" 
	id="WebApp_ID" version="4.0">
</web-app>

```

