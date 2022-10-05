# Mybatis核心文件：SqlMapConfig.xml

## 1、<span style="color:brown">Mybatis的核心文件：</span>

**1.1、environments标签：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/environments%E6%A0%87%E7%AD%BE.png" alt="image-20220925204927067" style="zoom:80%;" />

<!--详解-->：

- 事务管理器（transactionManager）的类型有两种：

  - JDBC

    ```apl
    '直接使用了JDBC的提交和回滚设置', 它依赖于从数据源得到的连接来管理事务作用域
    ```

  - MANAGED 【基本不用！！！】

    

- 数据源（dataSource）的类型有三种：

  - UNPOOLED

    ```apl
    这个数据源的实现只是每次被请求时, 打开和关闭连接
    ```

  - POOLED

    ```apl
    这种数据源的实现利用“池”的概念, 将JDBC连接对象组织起来
    ```

  - JNDI

    ```apl
    这个数据源的实现是为了'能在如: EJB 或 应用服务器这类容器中使用'
    容器可以'集中或在外部配置数据源', 然后放置一个JNDI上下文的引用
    ```

**1.2、mapper标签：**

![image-20220925210634402](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/mapper%E6%A0%87%E7%AD%BE.png)

**1.3、Properties标签：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Properties%E6%A0%87%E7%AD%BE%E7%9A%84%E4%BD%BF%E7%94%A8.png" alt="image-20220925211954717" style="zoom:80%;" />

**1.4、typeAliases标签：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/typeAliases%E6%A0%87%E7%AD%BE%E8%AF%A6%E8%A7%A3.png" alt="image-20220925212759953" style="zoom:80%;" />



##  2、<span style="color:brown">Mybatis的核心文件深入：</span>typeHandlers

**2.1、typeHandlers标签：**

### <!--'类型处理器', 将获取的值以合适的方式转换成 Java 类型, 架起"数据库类型与Java类型一一映射的桥梁"-->

```apl
可以将Java类型转换成数据库兼容的类型, 也可以将数据库类型转换成Java兼容的类型
```

**2.2、typeHandlers开发步骤：**

![image-20221001174545338](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/typeHandlers%E4%BD%BF%E7%94%A8%E6%AD%A5%E9%AA%A4.png)

**2.3、代码实现：**

### 在User实体类中，编写`private Data birthday`[getter and setter]，然后在数据库中加入字段：`birthday  Bigint`

- UersMapper.xml

  ```xml
  <insert id="save" parameterType="user">
          insert into user values(#{id}, #{username}, #{password}, #{birthday})
  </insert>
  ```

- SqlMapConfig.xml

  ![image-20221001180116525](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/typeHandlers%E4%B9%8B%E6%A0%B8%E5%BF%83%E9%85%8D%E7%BD%AE.png)

- 自定义转换类：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/typeHandler%E7%9A%84%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BD%AC%E6%8D%A2%E5%99%A8%E5%AE%9E%E7%8E%B0.png" alt="image-20221001175852787" style="zoom:80%;" />

- 测试类

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/typeHandlers%E4%B9%8B%E6%B5%8B%E8%AF%95%E7%B1%BB.png" alt="image-20221001180212940" style="zoom:80%;" />



## 3、<span style="color:brown">Mybatis的核心文件深入：</span>plugins

**3.1、plugins标签：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/plugins%E6%A0%87%E7%AD%BE.png" alt="image-20221001181626780" style="zoom:80%;" />

**3.2、注意事项：**

**PageHelper插件4.0.0以后的版本支持自动识别使用的数据库**，所以`不必再指定方言`！！！

其次呢，<u>不能使用pagehelper，而应该改为PageInterceptor</u>：

```apl
PageHelper5.0版本pagehelper是继承了PageMethod和实现了Dialect，PageInterceptor是实现了Interceptor接口
```

**3.3、代码实现：**

- pom.xml

  ```xml
  <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.27</version>
      </dependency>
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.6</version>
      </dependency>
      <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>5.3.2</version>
      </dependency>
      <dependency>
        <groupId>com.github.jsqlparser</groupId>
        <artifactId>jsqlparser</artifactId>
        <version>4.5</version>
      </dependency>
  ```

- UserMapper.xml【部分】

  ```xml
  <select id="search" resultType="user">
          select * from user
  </select>
  ```

- SqlMapConfig.xml【部分】

  ```xml
  <!--配置分页助手-->
  <plugins>
          <plugin interceptor="com.github.pagehelper.PageInterceptor">
              <!--指定方言-->
              <!--<property name="dialect" value="mysql"/>-->
          </plugin>
  </plugins>
  ```

- UserMapper接口

  ```java
  public interface UserMapper {
      List<User> search();
  }
  ```

- 测试类：

  ```java
  @Test
  public void testPlugins() throws IOException {
      InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
      SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
      SqlSession sqlSession = sessionFactory.openSession();
      UserMapper mapper = sqlSession.getMapper(UserMapper.class);
      
      // 设置分页的参数  当前页+每页显示的条数
      PageHelper.startPage(1,3);
      
      List<User> all = mapper.search();
      for (User user : all) {
          System.out.println(user);
      }
      
      // 获得与分页相关的参数
      PageInfo<User> pageInfo = new PageInfo<>(all);
      System.out.println("当前页: "+pageInfo.getPageNum());
      System.out.println("每页显示条数: "+pageInfo.getPageSize());
      System.out.println("总条数: "+pageInfo.getTotal());
      System.out.println("总页数: "+pageInfo.getPages());
      System.out.println("上一页是: "+pageInfo.getPrePage());
      System.out.println("下一页是: "+pageInfo.getNextPage());
      System.out.println("是否是第一页: "+pageInfo.isIsFirstPage());
      System.out.println("是否是最后一页: "+pageInfo.isIsLastPage());
      sqlSession.close();
  }
  ```

