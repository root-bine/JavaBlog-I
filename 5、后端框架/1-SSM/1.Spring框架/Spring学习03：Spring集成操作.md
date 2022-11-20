## 1、<span style="color:brown">Spring集成Junit：</span>

**1.1、Spring集成Junit的原因？**

在原始Junit测试Spring的问题上，都必须由如下内容：

![image-20220722152920762](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E9%9B%86%E6%88%90Junit%E7%9A%84%E5%8E%9F%E5%9B%A0.png)

**1.2、Spring集成Junit的步骤：**

![image-20220722154808742](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E9%9B%86%E6%88%90Junit%E7%9A%84%E6%AD%A5%E9%AA%A4.png)

**1.3、代码实现：**

**需要在pom.xml文件中导入junit、spring-test的依赖**，`然后不展示：UserService、UserServiceImpl、UserDao、UserDaoImpl、SpringConfiguration、DataSourceConfiguration的代码`

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {SpringConfiguration.class})
public class SpringJunit {
    @Autowired
    private UserService userService;
    @Autowired
    private DataSource dataSource;

    @Test
    public void test() throws SQLException {
        userService.save();
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
    }
}
```



## 2、<span style="color:brown">Spring集成Web环境：</span>自定义监听器ContextLoaderListener

### <span style="color:green">**除去最基础的Spring依赖：spring-context、Junit、Spring-test，还需要导入：servlet-api、javax.servlet.jsp-api**</span>！！！

**2.1、ApplicationContext【应用上下文】获取方式：**

![image-20220722171548950](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/ApplicationContext%E8%8E%B7%E5%8F%96%E4%B8%8A%E4%B8%8B%E9%97%AE%E7%9A%84%E6%96%B9%E5%BC%8F.png)

**2.2、自定义ContextLoaderListener：**

<span style="color:violet">**`此部分代码使用的是全注解形式，代码部分省去了UserService、UserDao接口的代码`**！！！</span>

```java
@Component("userDao")
public class UserDaoIpml implements UserDao {
    @Override
    public void save() {
        System.out.println("save running......");
    }
}
```

```java
@Service("userService")
public class UserServiceImpl implements UserService {
    @Autowired
    @Qualifier("userDao")
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void save() {
        userDao.save();
    }
}
```

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >
<web-app
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                  http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
id="WebApp_ID" version="4.0">
    <display-name>Archetype Created Web Application</display-name>
    <!--配置UserServlet类的镜像-->
    <servlet>
      <servlet-name>UserServlet</servlet-name>
      <servlet-class>com.zgy.web.UserServlet</servlet-class>
    </servlet>
    <servlet-mapping>
      <servlet-name>UserServlet</servlet-name>
      <url-pattern>/userServlet</url-pattern>
    </servlet-mapping>
    <!--配置监听器-->
    <listener>
      <listener-class>com.zgy.listener.ContextLoaderListener</listener-class>
    </listener>
</web-app>
```

```java
//Spring核心配置文件
@Configuration
@ComponentScan("com.zgy")
public class SpringConfig {
}
```

```java
//监听器
public class ContextLoaderListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        ApplicationContext app = new AnnotationConfigApplicationContext(SpringConfig.class);
        ServletContext servletContext = servletContextEvent.getServletContext();
        servletContext.setAttribute("app",app);
        System.out.println("Spring容器创建成功...");
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {

    }
}
```

```java
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        ServletContext servletContext = this.getServletContext();
        ApplicationContext app = (ApplicationContext) servletContext.getAttribute("app");
        UserService service = app.getBean(UserService.class);
        service.save();
    }
}
```



## 3、<span style="color:brown">Spring集成Web环境：</span>Spring定义监听器【未使用全注解形式】

**3.1、优化自定义ContextLoaderListener类的部分代码：**

```apl
在完成基本功能实现的过程后, 需要针对于如下内容进行优化:
1. ContextLoaderListener类中的“:
		ClassPathXmlApplicationContext("applicationContext.xml")
------------------------------------------------------------------------------------------------------------
2. UserServlet类中的:
		servletContext.getAttribute("app")
```

首先在web.xml中加入以下内容：

![image-20220722204659567](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/web.xml%E4%B8%AD%E7%9A%84%E5%85%A8%E5%B1%80%E5%88%9D%E5%A7%8B%E5%8C%96%E5%8F%82%E6%95%B0.png)

然后将ContextLoaderListener中的部分代码替换成：

![image-20220722204830305](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9B%91%E5%90%AC%E5%99%A8%E7%9A%84%E4%BC%98%E5%8C%9601.png)

之后再存放监听器的pacage中，再**定义一个WebApplicationContextUtils类**，内容如下：

![image-20220722205010472](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9B%91%E5%90%AC%E5%99%A8%E4%BC%98%E5%8C%9602.png)

最后，在ContextLoaderListener类中的`servletContext.setAttribute("app",app)`不变，而在UserServlet类中获取域中的”app“属性变化为：

![image-20220722205319400](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9B%91%E5%90%AC%E5%99%A8%E4%BB%A3%E7%A0%81%E4%BC%98%E5%8C%9603.png)

**3.2、Spring提供获取应用上下文的工具：**

![image-20220722210333918](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E5%AE%9A%E4%B9%89%E7%9B%91%E5%90%AC%E5%99%A8.png)

**<u>操作如下：</u>**

在pom.xml中导入spring-web的坐标，然后在<u>web.xml文件</u>中将**配置监听器**和**全局变量加载**进行修改：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E6%8F%90%E4%BE%9B%E5%BA%94%E7%94%A8%E4%B8%8A%E4%B8%8B%E6%96%8701.png" alt="image-20220722211403148" style="zoom:67%;" />



之后就是直接到UserServlet类中修改代码：

![image-20220722211726530](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E6%8F%90%E4%BE%9B%E5%BA%94%E7%94%A8%E4%B8%8A%E4%B8%8B%E6%96%8702.png)
