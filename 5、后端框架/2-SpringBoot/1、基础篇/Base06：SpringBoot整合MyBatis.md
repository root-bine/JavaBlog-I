## 1、<span style="color:brown">整合MyBatis</span>

**1.1、整合的内容：**

- 核心配置：数据库连接相关信息（连什么？连谁？什么权限？)
- 映射配置：SQL映射
  - XML：详见SSM中Mybatis内容
  - 注解

**1.2、项目配置：**

创建一个新的Module，进行项目名称、JDK选择等基础配置，之后选择整合MyBatis所需要的依赖：

- MyBatis FrameWork
- MySQL Driver

然后将配置文件的默认后缀修改为：`yml` 或者 `yaml`，并**配置数据源信息**：

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db2?serverTimezone=UTC
    username: root
    password: 123456
```

**1.3、在SpringBoot开启Mybatis日志：**

```yml
mybatis:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

**1.4、别名设置：**

​		在传统mybatis使用中，一般在SqlMapConfig.xml文件的<typeAliases>标签中配置。之后再Mapper.xml文件中，<u>resultMap的type、parameterType、resultType</u>会引用一些实体类，此时可以使用设置的别名。

​		而在SpringBoot项目中，别名只需在application.properties或application.yml中配置，然后在Mapper.xml文件中使`类名`即可：

```yml
# 搜索指定包别名
mybatis:
  type-aliases-package: com.zgy.springboot_byte.domain
```





## 2、<span style="color:brown">问题与总结</span>

**2.1、@Mapper注解：**

- 作用：

  ```scss
  '给mapper接口自动生成一个实现类', 让spring对mapper接口的bean进行管理, 并且可以省略去写复杂的xml文件
  ```

- 目的：

  <span style="color:green">**数据库SQL映射，需要添加@Mapper注解，来让容器识别到**</span>

**2.2、Bug01：**

<u>问题展示：</u>

![image-20221009154510050](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88MyBatis02.png)

解决方法：

1. 在`.yml`文件中，数据库连接的密码设置错误；
2. `yml`是区分数据类型的，所以如果用户名或者密码是数字的话，就必须要慎重对待：
   - **如果`password`为`000000`的话，最终获取到的值是`0`，显然不对**
   - 那么`6`个`0`怎么表示呢？
     - 用双引号`"000000"`
     - 用单引号`'000000'`

**2.3、Bug02：**

1. 设置时区：

   *<u>在SpringBoot整合MyBatis中，如果将<parent></parent>中的版本号下调，可能会出现如下问题：</u>*

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88MyBatis03.png" alt="image-20221009160020679" style="zoom:80%;" />

修改`.yml文件中的url内容`即可：

```scss
jdbc:mysql://localhost:3306/xxx?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
```

2. 驱动版本过时：

   如果仅修改url的时区，可以正常运行程序，但是仍会出现如下内容：

   ![image-20221009160640137](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88MyBatis04.png)

解决方法：

```scss
将driver-class-name的内容, 修改为: com.mysql.cj.jdbc.Driver
```



## 3、<span style="color:brown">范例演示</span>

### <!--此处实现SQL数据查询方式，参照MyBatis接口代理方式开发-->

- 数据库：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88MyBatis01.png" alt="image-20221009153050863" style="zoom: 80%;" />

- Book【数据实体类】：`domain`

  ```java
  public class Book {
      private Integer id;
      private String type;
      private String name;
      private String description;
      // Getter and Setter
      // toString()
  }
  ```

- BookDao【接口】：`dao`

  ```java
  @Mapper
  public interface BookDao {
      @Select("select * from book where id = #{id}")
      public abstract Book getById(Integer id);
  }
  ```

- SpringBootMyBatisTest【测试类】：`com.zgy`

  ```java
  @SpringBootTest
  class SpringbootMybatisApplicationTests {
      @Autowired
      private BookDao bookDao;
      @Test
      void contextLoads() {
          System.out.println(bookDao.getById(4));
      }
  }
  ```

