## 1、<span style="color:brown">Spring JDBCTemplate基础：</span>

**1.1、JDBCTempalte开发步骤：**

![image-20220808215127313](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%20JDBCTemplate%E5%BC%80%E5%8F%91%E6%AD%A5%E9%AA%A4.png)

**1.2、开发实现：**

​	除开：`mysql-connector-java、spring-context、spring-web、spring-webmvc、servlet-api、javax.servlet.jsp-api、C3P0`坐标外，还需要额外导入坐标：<u>***spring-jdbc、spring-tx两个坐标***</u>，内容如下：

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.0.15.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.0.15.RELEASE</version>
</dependency>
```

之后就是创建一个account表【test数据库】，其中属性为：name、money。然后就是创建具体的实体：Account类，创建如下内容：

```
private String name;
private int money;
```

并为这两个参数设置`Getter&Setter`方法，以及`toString()`方法。

---

前期工作准备完毕之后，就需要创建一个测试类，来**实现JDBCTemplate的快速入门**：

```java
public class JDBCTemplateTest {
    @Test
    public void test01() throws PropertyVetoException {
        // 创建数据源对象: C3P0
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass("com.mysql.cj.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test?true&characterEncoding=utf-8&useSSL=false&&serverTimezone=GMT");
        dataSource.setUser("root");
        dataSource.setPassword("123456");
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        
        // 设置数据源对象, 告知数据库的位置
        jdbcTemplate.setDataSource(dataSource);
        // 执行操作
        int row = jdbcTemplate.update("insert into account values(?,?)", "zhangsan", 2500);
        System.out.println(row);
    }
}
```



## 2、<span style="color:brown">Spring产生模板对象：</span>

**2.1、将DataSource对象，注入到JDBCTemplate对象：**

在resources目录下，创建一个application.xml文件，在其中配置：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E4%BA%A7%E7%94%9F%E6%A8%A1%E6%9D%BF%E5%AF%B9%E8%B1%A101.png" alt="image-20220809202055344" style="zoom:80%;" />

然后在JDBCTemplateTest类中，编写测试代码：

![image-20220809202325358](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E4%BA%A7%E7%94%9F%E6%A8%A1%E6%9D%BF%E5%AF%B9%E8%B1%A102.png)

以上代码可以<u>**使用注解**</u>进行操作：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E4%BA%A7%E7%94%9F%E6%A8%A1%E6%9D%BF.png" alt="image-20220809203613031" style="zoom:80%;" />

**2.2、简化IOC容器中，标识为dataSource的Bean实例对象：**

创建一个jdbc.properties文件，内容如下：

```properties
jdbc.driverClassName=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/db3?true&characterEncoding=utf-8&useSSL=false&&serverTimezone=GMT
jdbc.username=root
jdbc.password=123456
```

在普通的XML文件中，没有context命名空间，因此需要添加：

```
xmlns:context="http://www.springframework.org/schema/context"
http://www.springframework.org/schema/context/spring-context-4.2.xsd
```

然后需要在application.xml文件中，添加并修改如下内容：

![image-20220809203040479](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%E4%BA%A7%E7%94%9F%E6%A8%A1%E6%9D%BF%E5%AF%B9%E8%B1%A103.png)

