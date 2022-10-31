# SpringBoot整合04

## 1、<span style="color:brown">整合Druid：</span>

**1.1、项目搭建：**

进入Module界面，配置项目的基础信息，然后选择导入的依赖：MyBatis FrameWork、MySQL Driver。之后需要手动导入Druid依赖，内容如下：

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.13</version>
</dependency>
```

**1.2、Application.yml文件配置：**

1. Druid配置01：

   ```yaml
   spring:
     datasource:
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/db2?serverTimezone=UTC
       username: root
       password: 123456
       type: com.alibaba.druid.pool.DruidDataSource
   ```

2. Druid配置02：<span style="color:red">**【推荐】**</span>

   ```yaml
   spring:
     datasource:
       druid:
         driver-class-name: com.mysql.cj.jdbc.Driver
         url: jdbc:mysql://localhost:3306/db2?serverTimezone=UTC
         username: root
         password: 123456
   ```



## 2、<span style="color:brown">项目内容：</span>

**2.1、代码展示：**

- Book【数据封装实体类】：

  ```java
  package com.zgy.domain;
  
  public class Book {
      private Integer id;
      private String type;
      private String name;
      private String description;
  	...
  }
  ```

- BookDao【接口】：

  ```java
  @Mapper
  public interface BookDao {
      @Select("select * from tb_book where id = #{id}")
      public abstract Book getById(Integer id);
  }
  ```

- 测试类：

  ```java
  @SpringBootTest
  class SpringbootDruidApplicationTests {
      @Autowired
      private BookDao bookDao;
      @Test
      void contextLoads() {
          System.out.println(bookDao.getById(2));
      }
  }
  ```

**2.2、结果展示：**

![image-20221009221056425](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88Druid.png)
