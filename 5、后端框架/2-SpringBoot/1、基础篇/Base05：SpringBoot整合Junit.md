## 1、<span style="color:brown">如何整合Junit？</span>

**1.1、整合Junit分析：**

无论是<u>**创建一个拥有完整依赖的spring boot项目, 还是创建一个无添加依赖的spring boot项目**</u>，都存在依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
</dependency>
```

在完全无依赖的spring boot项目中，默认导入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
```

项目结构为：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/springboot%E6%95%B4%E5%90%88Junit01.png" alt="image-20221008211431250" style="zoom:80%;" />

**1.2、@SpringBootTest：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88Junit02.png" alt="image-20221008215611318" style="zoom:80%;" />

**1.3、创建项目：**

- dao层：

  - BookDao：

    ```java
    public interface BookDao {
        void save();
    }
    ```

  - BookDaoImpl：

    ```java
    @Repository
    public class BookDaoImpl implements BookDao {
        @Override
        public void save() {
            System.out.println("sava running...");
        }
    }
    ```

- test目录：

  ```java
  @SpringBootTest
  class SpringbootJunitApplicationTests {
      @Autowired
      private BookDao bookDao;
      @Test
      void contextLoads() {
          bookDao.save();
      }
  }
  ```



## 2、<span style="color:brown">classes属性</span>

## <!--Junit测试类原属：com.zgy.SpringbootJunitTest，如果将该类移到com下，则无法运行-->

**2.1、错误展示：**

![image-20221008221503772](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88Junit03.png)

- 问题描述：

  ```apl
  1. 不能找到@SpringBootConfiguration这个注解
  2. 你需要使用@ContextConfiguration(...)或者@SpringBootTest(...)来进行指定
  ```

**2.2、根据Spring整合Junit原理分析：**

1. 使用：`@RunWith(SpringJUnit4ClassRunner.class)`，指定**运行器**；
2. 使用`@ContextConfiguration(classes = {配置文件  or  配置类.class})`，指定**配置文件或者配置类**；

所以：<span style="color:red">当SpringBootJunitTest测试类和SpringBootApplication启动类, 不在同一个Package目录下</span>, 可以在测试类进行如下配置：

```apl
@SpringBootTest(classes = 启动类.class)     【推荐】

------------------------------------------------------------------------------------------------------------

@SpringBootTest
@ContextConfiguration(classes = 启动类.class)
```

---

关于**@SpringBootConfiguration**，在启动类的@SpringBootApplication注解中，就存在这个注解：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88Junit04.png" alt="image-20221008223237393" style="zoom:80%;" />

**2.3、classes属性总结：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88Junit05.png" alt="image-20221008222858465" style="zoom:80%;" />
