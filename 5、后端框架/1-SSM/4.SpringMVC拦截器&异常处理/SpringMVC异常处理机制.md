## 1、<span style="color:brown">基础内容：</span>

**1.1、异常处理的思路：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86%E7%9A%84%E6%80%9D%E8%B7%AF.png" alt="image-20220810204122991" style="zoom:80%;" />

**1.2、异常处理的方式：**

![image-20220810204312501](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86%E7%9A%84%E6%96%B9%E5%BC%8F.png)



## 2、<span style="color:brown">简单异常处理器：</span>SimpleMappingExceptionResolver

**2.1、原理讲解：**

<u>SpringMVC已经定义好了该类型转换器，在使用时可以根据项目情况进行相应异常与视图的映射配置！！！</u>

在`spring-mvc.xml`文件中进行如下配置：

```xml
<!--配置简单映射异常处理器-->
<bean class=“org.springframework.web.servlet.handler.SimpleMappingExceptionResolver”>   
    <!--默认错误视图-->
    <property name=“defaultErrorView” value=“error”/>  
    <property name=“exceptionMappings”>
        <map>	     
            <!--配置: 异常类型、错误视图-->
            <entry key="com.wange.exception.MyException" value="error"/>
            <entry key="java.lang.ClassCastException" value="error"/>
        </map>
    </property>
</bean>

```

**2.2、代码实现：**

首先在web.xml文件中，配置SpringMVC的前端控制器：DispatcherServlet。之后在spring-mvc.xml文件中，定义如下内容：

```xml
<!--组件扫描Controller-->
<context:component-scan base-package="com.zgy.Controller"/>
<context:component-scan base-package="com.zgy"></context:component-scan>

<!--视图解析器-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!--<property name="suffix" value=".jsp"/>
    <property name="prefix" value="/index/"/>-->
    <property name="prefix" value="/"/>
    <property name="suffix" value=".jsp"/>
</bean>

<!--MVC注册驱动-->
<mvc:annotation-driven/>

<!--静态资源访问的开启-->
<mvc:default-servlet-handler/>

<!--配置异常处理器-->
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="defaultErrorView" value="error"/>
    <property name="exceptionMappings">
        <map>
            <entry key="java.lang.ClassCastException" value="error1"/>
        </map>
    </property>
</bean>
```

在src中新建Controller、Service、Exception三个文件目录，在webapp目录下新建error.jsp、error1.jsp文件

- Source目录内容：

  - DemoService：

    ```java
    public interface DemoService {
        void show1();
        void show2();
        void show3() throws FileNotFoundException;
        void show4();
        void show5() throws MyException;
    }
    ```

  - DemoServiceImpl：

    ```java
    @Service
    public class DemoServiceImpl implements DemoService{
        @Override
        public void show1() {
            System.out.println("抛出类型转换异常...");
            Object str = "zhangsan";
            Integer num = (Integer) str;
        }
    
        @Override
        public void show2() {
            System.out.println("抛出除零异常...");
            int i = 1/0;
        }
    
        @Override
        public void show3() throws FileNotFoundException {
            System.out.println("文件找不到异常...");
            FileInputStream fileInputStream = new FileInputStream("C:\\xxx\\xx");
        }
    
        @Override
        public void show4() {
            System.out.println("空指针异常...");
            String str = null;
            str.length();
        }
    
        @Override
        public void show5() throws MyException {
            System.out.println("自定义异常...");
            throw new MyException();
        }
    }
    ```

- Exception目录内容：

  - MyException：

    ```java
    public class MyException extends Exception{ }
    ```

- Controller目录内容：

  - DemoController：

    ```java
    @Controller
    public class DemoController {
        @Autowired
        private DemoService demoService;
        @RequestMapping("/show")
        public String show(){
            System.out.println("show running...");
            demoService.show1();
            return "index";
        }
    }
    ```

- webapp目录内容：

  - error：

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <h1>通用异常处理界面...</h1>
    </body>
    </html>
    ```

  - error1：

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <h1>类型转换异常...</h1>
    </body>
    </html>
    ```



## 3、<span style="color:brown">自定义异常处理器：</span>HandlerExceptionResolver

1）创建异常处理器类实现HandlerExceptionResolver

```java
public class MyExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, 
                                          HttpServletResponse response, Object handler, Exception ex) {
        //处理异常的代码实现
        //创建ModelAndView对象
        ModelAndView modelAndView = new ModelAndView(); 
        modelAndView.setViewName("exceptionPage");
        return modelAndView;
    }
}
```

2）配置异常处理器：`在spring-mvc.xml文件中补充`

```xml
<bean id="exceptionResolver" class="com.wange.exception.MyExceptionResolver"/>
```

3）编写异常页面：`exceptionPage.jsp`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>

<head>
	<title>Title</title>
</head>

<body>
	这是一个最终异常的显示页面
</body>
</html>
```

4）测试异常跳转

```java
@RequestMapping("/save")
@ResponseBody
public void saveMethod() throws IOException, ParseException {
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd"); 
    simpleDateFormat.parse("abcde");
}
```

