## 1、<span style="color:brown">属性配置：</span>

**1.1、修改端口号：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%B1%9E%E6%80%A7%E9%85%8D%E7%BD%AE.png" alt="image-20221006214034832" style="zoom:67%;" />

修改服务器端口号：

```properties
server.port = 80
```

**1.2、修改其他配置：**

![image-20221006220753492](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/202211032120670.png)

以上均为基础设置的部分内容，详情见：[Application Properties文件参数配置](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties)

**1.3、配置文件类型：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B.png" alt="image-20221006221549884" style="zoom:80%;" />

<u>在三种配置文件同时存在时，SpringBoot读取优先级为：</u>

```apl
application.properties  >  application.yml  >  application.yaml 
```

<u>当这种情况出现时，三种文件中的配置信息的读取方式为：</u>

<span style="color:blue">不同配置文件中相同配置，按照加载优先级相互覆盖，而不同配置文件中不同配置全部保留</span>

**1.4、配置文件无法显示属性提示？**

点击File，选择Project Structure选项，然后配置如下：

![image-20221006224633828](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E9%85%8D%E7%BD%AE02.png)



## 2、<span style="color:brown">YAML数据格式：</span>

**2.1、简介：**

YAML ( YAML Ain't Markup Language) ，**一种数据序列化格式**。

- 优点：

  ```apl
  1. 容易阅读
  2. 容易与脚本语言交互
  3. 以数据为核心, '重数据轻格式'
  ```

- YAML文件扩展名：

  ```vbscript
  .yml (主流)
  
  .yaml
  ```

**2.2、YAML语法原则：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/YAML%E8%AF%AD%E6%B3%95%E5%8E%9F%E5%88%99.png" alt="image-20221006230951793" style="zoom:80%;" />

**2.3、YAML数据写法：**

- 字面值表示方式：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%AD%97%E9%9D%A2%E5%80%BC%E8%A1%A8%E7%A4%BA%E6%96%B9%E5%BC%8F.png" alt="image-20221006231349868" style="zoom:80%;" />

- 数组表示方式：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%95%B0%E7%BB%84%E8%A1%A8%E7%A4%BA%E6%96%B9%E5%BC%8F.png" alt="image-20221006231532260" style="zoom: 50%;" />



## 3、<span style="color:brown">YAML读取数据：</span>

**3.1、注解补充：**

| `@ConfigurationProperties(profix="...")` | 用于获取配置文件中的属性定义并绑定到Java Bean或属性中     |
| ---------------------------------------- | --------------------------------------------------------- |
| **`@Value("${...}")`**                   | **用于获取配置文件中的属性定义并绑定到Java Bean或属性中** |

**3.2、读取单一数据：**

<u>*使用`@Value`读取单个数据，属性名引用方式:${一级属性名.二级属性名.....}*</u>

- application.yml

  ```yaml
  country: china
  user:
    - name: zgy
      age: 18
    - name: ithema
      age: 20
  user1:
    name: itzgy
    age: 19
  ```

- Book:

  ```java
  @RestController
  @RequestMapping("/book")
  public class Book {
      @Value("${country}")
      private String nation;
      @Value("${user[0].name}")
      private String name1;
      @Value("${user[1].age}")
      private String age1;
      @Value("${user1.name}")
      private String name2;
      @Value("${user1.age}")
      private String age2;
  
      @GetMapping
      public String start(){
          System.out.println("Spring Boot is Running...");
          System.out.println("Country===>"+nation);
          System.out.println("user===>"+name1+","+age1);
          System.out.println("user1===>"+name2+","+age2);
          return "Spring Boot is Running...";
      }
  }
  ```

- 控制台：

  Spring Boot is Running...
  Country===>china
  user===>zgy,20
  user1===>itzgy,19

**3.3、变量引用：**

- application.yml：

  ```yaml
  baseDir: C:\win10
  tempDir1: ${baseDir}\temp
  tempDir2: "${baseDir}\temp"
  ```

- Book：

  ```java
  @RestController
  @RequestMapping("/book")
  public class Book {
      @Value("${tempDir1}")
      private String dir1;
      @Value("${tempDir2}")
      private String dir2;
  
      @GetMapping
      public String start(){
          System.out.println("Spring Boot is Running...");
          System.out.println("Dir1===>"+dir1);
          System.out.println("Dir2===>"+dir2);
          return "Spring Boot is Running...";
      }
  }
  ```

- 控制台：<span style="color:red">\t是转义字符</span>

  Spring Boot is Running...
  Dir1===>C:\win10\temp
  Dir2===>C:\win10	emp

**3.4、读取全部属性：**

```apl
1. 使用Environment对象封装'全部配置信息'；

2. 使用@Autowired自动装配数据到Environment对象中；
```

- Application.yml:

  ```yaml
  server:
    port: 8081
  country: china
  user:
    - name: zgy
      age: 18
    - name: ithema
      age: 20
  user1:
    name: itzgy
    age: 19
  
  baseDir: C:\win10
  tempDir1: ${baseDir}\temp
  tempDir2: "${baseDir}\temp"
  ```

- Book:

  ```java
  @RestController
  @RequestMapping("/book")
  public class Book {
      @Autowired
      private Environment evn;
  
      @GetMapping
      public String start(){
          System.out.println("Spring Boot is Running...");
          System.out.println(evn.getProperty("user[0].name"));
          System.out.println(evn.getProperty("user[1].age"));
          System.out.println(evn.getProperty("user1.name"));
          System.out.println(evn.getProperty("user1.age"));
          System.out.println(evn.getProperty("tempDir1"));
          System.out.println(evn.getProperty("tempDir2"));
          System.out.println(evn.getProperty("country"));
          System.out.println(evn.getProperty("server.port"));
          return "Spring Boot is Running...";
      }
  }
  ```

- 控制台：

  Spring Boot is Running...
  zgy
  20
  itzgy
  19
  C:\win10\temp
  C:\win10	emp
  china
  8081

**3.5、读取引用类型数据：**

```scss
1. 创建类, 用于封装YAML文件的数据
2. 定义为Spring管控的bean: @Component
3. 指定加载的数据: @ConfigurationProperties("...")
4. 业务层调用数据:
    @Autowired
    private MyDataSource datasource;
```

- application.yml：

  ```yaml
  datasource:
    driver: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springboot_db
    username: root
    password: 123456
  ```

- MyDataSource：

  ```java
  @Component
  @ConfigurationProperties("datasource")
  public class MyDataSource {
      private String driver;
      private String url;
      private String username;
      private String password;
      @Override
      public String toString() {
          return "MyDatasource{" +
                  "driver='" + driver + '\'' +
                  ", url='" + url + '\'' +
                  ", username='" + username + '\'' +
                  ", password='" + password + '\'' +
                  '}';
      }
      public String getDriver() {
          return driver;
      }
      public void setDriver(String driver) {
          this.driver = driver;
      }
      public String getUrl() {
          return url;
      }
      public void setUrl(String url) {
          this.url = url;
      }
      public String getUsername() {
          return username;
      }
      public void setUsername(String username) {
          this.username = username;
      }
      public String getPassword() {
          return password;
      }
      public void setPassword(String password) {
          this.password = password;
      }
  }
  ```

- Book：

  ```java
  @RestController
  @RequestMapping("/book")
  public class Book {
      @Autowired
      private MyDataSource datasource;
      @GetMapping
      public String start(){
          System.out.println("Spring Boot is Running...");
          System.out.println(datasource);
          return "Spring Boot is Running...";
      }
  }
  ```

- 控制台：

  Spring Boot is Running...
  MyDatasource{driver='com.mysql.cj.jdbc.Driver', url='jdbc:mysql://localhost:3306/springboot_db', username='root', password='123456'}