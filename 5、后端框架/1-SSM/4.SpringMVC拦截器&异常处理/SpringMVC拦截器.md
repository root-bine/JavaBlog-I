## 1、<span style="color:brown">基础内容：</span>

**1.1、拦截器【interceptor】的作用：**

![image-20220809205207072](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E6%8B%A6%E6%88%AA%E5%99%A8%E7%9A%84%E4%BD%9C%E7%94%A8.png)

**1.2、拦截器与过滤器的区别：**

![image-20220809210212904](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%8B%A6%E6%88%AA%E5%99%A8%E4%B8%8E%E8%BF%87%E6%BB%A4%E5%99%A8%E7%9A%84%E5%8C%BA%E5%88%AB.png)



## 2、<span style="color:brown">拦截器快速入门：</span>

**2.1、自定义拦截器的开发步骤：**

![image-20220809210606479](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8B%A6%E6%88%AA%E5%99%A8%E5%BC%80%E5%8F%91%E6%AD%A5%E9%AA%A4.png)

**2.2、代码实现：**

对于Spring项目，基础的坐标导入有：`spring-context、spring-web、spring-webmvc、junit、spring-test、servlet-api、javax.servlet.jsp-api`。然后就是***在web.xml文件中配置Spring的前端控制器DispatcherServlet***：

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>Dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>Dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

之后就需要创建一个**spring-mvc.xml文件**，内容如下：

```xml
<!--组件扫描Controller-->
<context:component-scan base-package="com.zgy.Controller"/>

<!--视图解析器-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="suffix" value=".jsp"/>
    <property name="prefix" value="/index/"/>
</bean>

<!--MVC注册驱动-->
<mvc:annotation-driven/>

<!--静态资源访问的开启-->
<mvc:default-servlet-handler/>

<!--配置拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
        <!--对哪些资源进行拦截-->
        <mvc:mapping path="/**"/>
        <bean class="com.zgy.Interceptor.MyInterceptor1"/>
    </mvc:interceptor>
</mvc:interceptors>
```

需要创建*目标资源类和JSP文件页面*，以实现后期的拦截器功能验证：

- JSP文件

  ```jsp
  <%--添加isELIgnored="false", 防止${}标签无法读取数据--%>
  <%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false"%>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
  <h2>Hello World!${name}</h2>
  </body>
  </html>
  ```

- 目标资源类

  ```java
  @Controller
  public class TargetController {
      @RequestMapping("/target")
      public ModelAndView show(){
          System.out.println("目标资源执行...");
          ModelAndView modelAndView = new ModelAndView();
          modelAndView.setViewName("resource");
          modelAndView.addObject("name","itZGY");
          return modelAndView;
      }
  }
  ```

之后就是<span style="color:violet">**拦截器实现类的创建，以及关于各方法的执行情况**</span>：

```java
public class MyInterceptor1 implements HandlerInterceptor {
    @Override
    // 在目标资源执行之前, 执行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle...");
        //return false; //设置为false, 就直接拦截所有资源, 不进行后续操作
        return true;
    }

    @Override
    // 在目标资源执行之后, 视图对象返回之前, 执行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...");
    }

    @Override
    // 在整个流程都执行完毕之后, 执行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...");
    }
}
```

拦截效果显示：【`boolean preHandle()`方法返回值为false，那么控制台只输出preHandle...，而浏览器只是空白界面】

![image-20220809230118254](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%8B%A6%E6%88%AA%E6%95%88%E6%9E%9C01.png)



## 3、<span style="color:brown">拦截器详解：</span>

**3.1、拦截器实现类中方法的解释：**

![image-20220809232137690](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%8B%A6%E6%88%AA%E5%99%A8%E8%AF%A6%E8%A7%A301.png)

**3.2、拦截器链：**

![image-20220809232457784](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%8B%A6%E6%88%AA%E5%99%A8%E9%93%BE.png)