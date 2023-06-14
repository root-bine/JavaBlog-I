## 1、<span style="color:brown">基础内容：</span>

**1.1、案例分析：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP01.png" alt="image-20221011144755210" style="zoom:80%;" />

**1.2、开发流程：**

![image-20221011144848760](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP02.png)



## 2、<span style="color:brown">模块搭建：</span>

**2.1、添加依赖：**

- 勾选SpringMVC坐标：`Spring Web`
- 勾选MySQL坐标：`MySQL Driver`
- 勾选MyBatis-Plus坐标：`MyBatisPlus FrameWork`

**2.2、手动添加Druid依赖：**

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.13</version>
</dependency>
```

**2.3、修改项目部分结构：**

修改配置文件为yml格式，并配置端口号：

```yaml
# 设置端口号
server:
  port: 80
```



## 3、<span style="color:brown">实体类开发：</span>

**3.1、创建数据库表格：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP03.png" alt="image-20221011151053561" style="zoom:80%;" />

**3.2、Lombok详解：**

- Lombok介绍：

  ![image-20221011153159474](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP04.png)

- **采用Lombok开发模式创建实体类**，需要手动导入依赖：

  ```xml
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.24</version>
      <scope>provided</scope>
  </dependency>
  ```

- Lombok注解：

  |           注解           |                             解释                             |
  | :----------------------: | :----------------------------------------------------------: |
  |         @Setter          | **注解在类或字段**，注解在类时为所有字段生成setter方法，注解在字段上时只为该字段生成setter方法 |
  |         @Getter          |         `【使用方法同上】`区别在于生成的是getter方法         |
  |        @ToString         |                **注解在类**，添加toString方法                |
  |    @EqualsAndHashCode    |            **注解在类**，生成hashCode和equals方法            |
  |    @NoArgsConstructor    |               **注解在类**，生成无参的构造方法               |
  | @RequiredArgsConstructor | **注解在类**，为类中需要特殊处理的字段生成构造方法，比如final和被@NonNull注解的字段 |
  |   @AllArgsConstructor    |         **注解在类**，生成包含类中所有字段的构造方法         |
  |          @Data           | **注解在类**，生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法 |

**3.3、实体类：**<span style="color:red">Ctrl+F12查看当前类中的方法</span>

```java
// 由于数据库表名称有前缀, 采用该注解指定表名称, 避免无法识别
@TableName("tb_book")
// lombok开发
/*// 生成Setter方法
@Setter
// 生成Getter方法
@Getter
// 生成toString()
@ToString*/
// 生成除开构造方法的一切方法
@Data
public class Book {
    // 在使用BaseMapper<T>时, 为避免因为默认id是雪花算法递增方式而报错
    // 因此, 采用该注解标明数据库表中id的增加方式
    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```



## 4、<span style="color:brown">数据层标准开发：</span>基础CRUD、分页、按条件查询

**4.1、基础配置：**

由于要进行数据层开发，必须实现导入MyBatis-Plus、Druid、MySQL Driver这三个的坐标。

同时还需要在配置文件application.yml中，进行相关的配置处理：

```yaml
# Druid配置
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/db2?serverTimezone=UTC
      username: root
      password: 123456

# MyBatis-Plus配置
# mybatis-plus:
#  global-config:
#    db-config:
      # 由于使用了@TableName("tb_book"), 此配置舍弃
#      table-prefix: tb_
      # 数据库表中id设置为自增属性, 可以被@TableId(value= " ", type= idType.xxx )
#      id-type: auto
```

**4.2、@TableId()注解：**

这个注释主要用于**对应数据库表的实体类**中的*<u>主键属性</u>*。

![image-20221011161027855](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP05.png)

<span style="color:green">**注解中type的值的含义：**</span>

|        type值        |                             含义                             |
| :------------------: | :----------------------------------------------------------: |
| IdType.ID_WORKER_STR |           默认的;底层使用了雪花算法；类型为Integer           |
|     IdType.AUTO      |               数据库自增；数据库上也要勾上自增               |
|     IdType.NONE      | **没有设置主键类型；跟随全局；全局的主键策略如果没有设置，默认是雪花算法** |
|     IdType.INPUT     |     手动输入【*<u>必须手动输入，数据库自增也没用</u>*】      |
|     IdType.UUID      |                  全局唯一id；无序;字符串；                   |
|    ID_WORKER_STR     |              全局唯一（idWorker的字符串表示)；               |

**4.3、开启日志：**

 SpringBoot项目，在`application.yml`文件中：

- <u>MP运行日志</u>：

  ```yaml
  mybatis-plus:
    configuration:
      log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  ```

---

选择测试`testGetAll()`方法，结果展示如下：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP06.png" alt="image-20221011165158701" style="zoom:67%;" />

**4.4、实现数据层的基础CRUD功能：**

- 数据层：dao

  > 如果**想要每个接口都要变成实现类**，那么需要在每个接口类上加上`@Mapper`注解
  >
  > 解决办法：在SpringBoot启动类使用`@MapperScan(...)`注解

  ```java
  @Mapper
  public interface BookDao extends BaseMapper<Book> {
  }
  ```

- 测试类：

  ```java
  @SpringBootTest
  class SpringbootSsmpApplicationTests {
      @Autowired
      private BookDao bookDao;
      // 根据id查找数据
      @Test
      void testGetById() {
          System.out.println(bookDao.selectById(2));
      }
  	// 添加数据
      @Test
      void testSave() {
          // 进行数据操作时,需要事先在 配置文件/实体类 中标明id增长方式, 否则会出现错误
          Book book = new Book();
          book.setType("测试数据123");
          book.setName("测试数据123");
          book.setDescription("测试数据123");
          bookDao.insert(book);
      }
      // 更新数据
      @Test
      void testUpdate() {
          Book book = new Book();
          book.setId(10);
          book.setType("更新数据");
          book.setName("测试数据456");
          book.setDescription("测试数据456");
          bookDao.updateById(book);
      }
      // 删除数据
      @Test
      void testDelete() {
          bookDao.deleteById(9);
      }
      // 查询所有数据
      @Test
      void testGetAll() {
          System.out.println(bookDao.selectList(null));
      }
  }
  ```

**4.5、实现分页操作：**<span style="color:red">在层次结构中，查询子页面的子类型【Ctrl+h】</span>

<span style="color:violet">分页操作是**在MyBatisPlus的常规操作基础上增强得到**</span>，**内部是动态的拼写SQL语句**！！！

- 分页插件：`config包`

  ```java
  @Configuration
  public class MPConfig {
      // 编写分页插件, 交给Spring容器处理
      @Bean
      public MybatisPlusInterceptor mybatisPlusInterceptor(){
          MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
          // 定义分页内部拦截器
          mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
          return mybatisPlusInterceptor;
      }
  }
  ```

- 测试类：

  ```java
  @SpringBootTest
  class SpringbootSSmpApplicationTests {
      @Autowired
      private BookDao bookDao;
      
      @Test
      void testGetPage() {
          // 设置分页的当前页、每页显示数据条数
          IPage<Book> iPage = new Page<>(1,5);
          bookDao.selectPage(iPage,null);
          
          // 当前页码
          System.out.println(iPage.getCurrent());
          // 当前页中的数据条数
          System.out.println(iPage.getSize());
          // 总共有多少条数据
          System.out.println(iPage.getTotal());
          // 总共有多少页
          System.out.println(iPage.getPages());
      }
  }
  ```

- 结果：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP07.png" alt="image-20221013160127186" style="zoom:67%;" />

**4.6、按条件查询 + 分页操作：**

***方式1：***

- 测试类：

  ```java
  @SpringBootTest
  class SpringbootSSmpApplicationTests {
      @Autowired
      private BookDao bookDao;
      @Test
      void testGetBy1() {
          // 分页功能
          IPage<Book> iPage = new Page<>(1,5);
          // 按条件查询
          QueryWrapper<Book> qw = new QueryWrapper<>();
          qw.like("name","Spring");
          bookDao.selectPage(iPage,qw);
      }
  }
  ```

- 结果：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP08.png" alt="image-20221013162102218"  />

***方式2：***

- 测试类：

  ```java
  @SpringBootTest
  class SpringbootSSmpApplicationTests {
      @Autowired
      private BookDao bookDao;
      @Test
      void testGetBy2() {
          String name = "Spring";
          IPage<Book> iPage = new Page<>(1,2);
          // 避免输入参数错误, 而造成无法访问数据库
          LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<>();
          lqw.like(name!=null,Book::getName,name);
          bookDao.selectPage(iPage,lqw);
      }
  }
  ```

- 结果：

  ![image-20221013162803038](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP09.png)

