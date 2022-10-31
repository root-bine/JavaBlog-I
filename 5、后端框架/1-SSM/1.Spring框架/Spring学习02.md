# Spring学习02

## 1、<span style="color:brown">Spring配置数据源：</span>

**1.1、数据源开发步骤：**

![image-20220714205831429](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%95%B0%E6%8D%AE%E6%BA%90%E5%BC%80%E5%8F%91%E6%AD%A5%E9%AA%A4.png)

**1.2、C3P0和Druid配置数据源：**

```properties
jdbc.driverClassName=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/db3?true&characterEncoding=utf-8&useSSL=false&&serverTimezone=GMT
jdbc.username=root
jdbc.password=123456
```

```java
public class Datasource {
    /**
     * 测试C3P0连接池
     * @throws Exception
     */
    @Test
    public void test1() throws Exception {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass("com.mysql.cj.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/db2");
        dataSource.setUser("root");
        dataSource.setPassword("123456");
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
        connection.close();
    }

    /**
     * 测试Druid连接池
     * @throws SQLException
     */
    @Test
    public void test2() throws SQLException {
        // 读取配置文件
        ResourceBundle rb = ResourceBundle.getBundle("jdbc");
        String driverClassName = rb.getString("jdbc.driverClassName");
        String url = rb.getString("jdbc.url");
        String username = rb.getString("jdbc.username");
        String password = rb.getString("jdbc.password");
        // 创建数据源对象, 设置连接参数
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName(driverClassName);
        druidDataSource.setUrl(url);
        druidDataSource.setUsername(username);
        druidDataSource.setPassword(password);
        DruidPooledConnection connection = druidDataSource.getConnection();
        System.out.println(connection);
        connection.close();
    }
}
```

**1.3、Spring配置数据源：**

<u>**DataSource的创建权由Spring容器去创建！！！**</u>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 		    http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.cj.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/db2"></property>
        <property name="user" value="root"></property>
        <property name="password" value="123456"></property>
    </bean>
</beans>
```

```java
public class Datasource {
    /**
     * 使用Spring容器产生数据源
     * @throws Exception
     */
    @Test
    public void test3() throws Exception {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        DataSource dataSource = (DataSource) app.getBean("dataSource");
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
        connection.close();
    }
}
```

## 2、<span style="color:brown">Spring注解开发：</span>原始注解

**2.1、Spring原始注解：**

<span style="color:green">**Spring原始注解主要是替代<Bean>标签**！！！</span>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E5%8E%9F%E5%A7%8B%E6%B3%A8%E8%A7%A3.png" alt="image-20220717213321259" style="zoom:80%;" />

![image-20220717215236855](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E7%BB%84%E4%BB%B6%E6%89%AB%E6%8F%8F.png)

```apl
通常情况下,创建一个spring的项目,如果注册bean对象是"通过注解注册而非配置文件的话",在配置文件当中都会看到<context:component-scan>这个注解。
------------------------------------------------------------------------------------------------------------
# 这个注解的作用大:扫描包
开启注解扫描,即: 注册bean对象
当配置完这个标签之后,spring就会自动扫描base-package属性下面的所有包。
如果扫描的包中有被@Service, @Controller, @Repository, @Component注解的类的话,根据spring控制反转的功能，spring会把这些类注册成bean。
```

**2.2、原始注解入门操作：**

**省略了UserDao、UserService接口的代码，以及application.xml文件中的组件扫描配置**

```java
//@Component("userService")
@Service("userService")
public class UserServiceImpl implements UserService {
    @Value("${db.driverClassName}")
    private String driver;
    
    @Autowired
    @Qualifier("userDao")
    private UserDao userDao;
	/*如果是xml配置文件注入, 就需要创建setter方法; 而使用注解可以直接省略*/
//    public void setUserDao(UserDao userDao) {
//        this.userDao = userDao;
//    }

    @Override
    public void save() {
        userDao.save();
        System.out.println(driver);
    }
}
```

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
public class UserController {
    public static void main(String[] args) {
        ApplicationContext app = new ClassPathXmlApplicationContext("application.xml");
        UserService service = app.getBean(UserService.class);
        service.save();
    }
}
```

**2.3原始注解详解：**

![image-20220717225555596](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8E%9F%E5%A7%8B%E6%B3%A8%E8%A7%A301.png)

<u>此处是从db.properties文件中，获取jdbc.driver的数值注入到private String driver变量中</u>

![image-20220717230042051](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8E%9F%E5%A7%8B%E6%B3%A8%E8%A7%A302.png)

## 3、<span style="color:brown">Spring注解开发：</span>新注解

**3.1、新注解详解：**

@Import注解中参数是一个数组，当传入多个外部资源时，写法为：`@Import({xxx.class, yyy.class})`

![image-20220717231105248](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E6%96%B0%E6%B3%A8%E8%A7%A3.png)

**3.2、代码演示：**

此处省略了UserDao、UserService、UserDaoImpl、UserServiceImpl代码内容

<span style="color:orange">**本次代码核心在于将db.properties、application.xml文件中的内容，通过注解的方式**，<u>实现全面替代</u>！！！</span>

Spring核心配置文件：

```java
// 标志该类是Spring的核心配置文件
@Configuration
// 组件扫描
//<context:component-scan base-package="com.zgy"/>
@ComponentScan("com.zgy")
// 将次要配置文件加载入Spring核心配置文件
@Import(DataSourceConfiguration.class)
public class SpringConfiguration { }
```

Spring次要配置文件：

```java
// <!--加载外部配置文件-->
// <context:property-placeholder location="classpath:db.properties"/>
@PropertySource("classpath:db.properties")
public class DataSourceConfiguration {
    @Value("${db.driverClassName}")
    private String driver;
    @Value("${db.url}")
    private String url;
    @Value("${db.username}")
    private String user;
    @Value("${db.password}")
    private String password;
    //此处替换application.xml中，关于使用Spring容器代为操作C3P0连接池生成数据源
    @Bean("dataSource")//Spring会将当前方法的返回值，以指定的名称储存到容器中
    public DataSource getDataSource() throws PropertyVetoException {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass(driver);
        dataSource.setJdbcUrl(url);
        dataSource.setUser(user);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```

main方法：

```java
public class UserController {
    public static void main(String[] args) throws PropertyVetoException {
        // ApplicationContext app = new ClassPathXmlApplicationContext("application.xml");
        ApplicationContext app = new AnnotationConfigApplicationContext(SpringConfiguration.class);
        UserService service = app.getBean(UserService.class);
        service.save();
------------------------------------------------------------------------------------------------------------
        DataSourceConfiguration dataSourceConfiguration = new DataSourceConfiguration();
        DataSource dataSource = dataSourceConfiguration.getDataSource();
        System.out.println(dataSource);
    }
}
```

