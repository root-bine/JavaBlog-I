## 1、<span style="color:brown">日志基础操作：</span>

**1.1、Log的作用：**

- 编程期调试代码：【在SSMP整合项目中，开启MP日志】
- 运维期记录信息：
  - 记录日常运维信息
    - 峰值流量、平均响应时间......
  - 记录应用报错信息：
    - 错误栈堆......
  - 记录运维过程数据：
    - 扩容、宕机、报警......

**1.2、添加日志操作记录：**

### <!--Logger：import org.slf4j.Logger-->

```java
@RestController
@RequestMapping("/books")
public class BookController {
    private final Logger logger = LoggerFactory.getLogger(BookController.class);
    @GetMapping
    public String getById(){
        System.out.println("springboot is running...");
        logger.info("消息记录...");
        logger.error("报错记录...");
        logger.warn("警告记录...");
        logger.debug("调试记录...");
        return "springboot is running...";
    }
}
```

**1.3、日志级别详解：**

```js
All < Trace < `DEBUG < INFO < WARN < ERROR` < Fatal < OFF
 
- OFF   | 关闭：最高级别，不打印日志。 
- FATAL | 致命：指明非常严重的可能会导致应用终止执行错误事件。
- ERROR | 错误：指明错误事件，但应用可能还能继续运行。 
- WARN | 警告：指明可能潜在的危险状况。 
- INFO | 信息：指明描述信息，从粗粒度上描述了应用运行过程。 
- DEBUG | 调试：指明细致的事件信息，对调试应用最有用。
- TRACE | 跟踪：指明程序运行轨迹，比DEBUG级别的粒度更细。 
- ALL | 所有：所有日志级别，包括定制级别。
 
日志级别由低到高:  `日志级别越高输出的日志信息越多`
```

*springboot默认是`INFO`，因此低于`INFO`的`TRACE`和`DEBUG`都不会输出*

**1.4、设置日志输出级别：**

- <u>*基本设置：*</u>

  ```yaml
  # 开启debug模式, 输出调试信息, 常用于检查系统运行状况
  debug: true
  
  # 设置日志级别, root表示根节点, 即: 整体应用日志级别
  logging:
    level:
      root: debug
  ```

- <u>*设置日志组：*</u>

  ####  <!--控制指定包对应的日志输出级别，也可以直接控制指定包对应的日志输出级别-->

  ```yaml
  logging:
    # 设置日志组
    group:
      # 自定义组名, 设置当前组所包含的包
      ebank: com.zgy.controller, com.zgy.service, com.zgy.dao
    level:
      root: warn
      # 设置对应组的日志级别
      ebank: debug
      # 设置对应包的日志级别
      com.zgy.controller: debug
  ```

  

## 2、<span style="color:brown">日志对象创建：</span>

**2.1、方式1：**

直接在需要进行日志信息输出的地方创建：

```java
private final Logger logger = LoggerFactory.getLogger(BookController.class);
```

但是<u>***这种创建方式存在代码严重复用的情况***</u>！！！

**2.2、方式2：**

创建一个工具类，让需要打印日志信息的类继承：`public class BookController extends BaseClass`。

<u>*工具类：*</u>

```java
public class BaseClass {
    public Class claszz;
    public Logger logger;

    public BaseClass() {
        claszz = this.getClass();
        logger = LoggerFactory.getLogger(claszz);
    }
}
```

**2.3、方式3：**

采用Lombok开发注解：`@Slf4j`，进行简化开发。

在pom.xml中导入Lombok依赖，之后只需要在需要进行日志打印的类上机上注解即可。

之后再类中以log对象进行日志信息调用、打印、记录。

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Slf4j.png" alt="image-20221021164520069" style="zoom:80%;" />



## 3、<span style="color:brown">日志输出格式控制：</span>

**3.1、格式展示：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%20Logout01.png" alt="image-20221021165656848" style="zoom:80%;" />

**3.2、设置日志输出格式：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Logout02.png" alt="image-20221021171152400" style="zoom:80%;" />



## 4、<span style="color:brown">日志内容保存：</span>

**4.1、设置日志文件：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Logout03.png" alt="image-20221021172704089"  />

完成保存日志内容的文件配置，可以在项目文件根目录下找到：server.log，其中就包含日志信息！！！

**4.2、日志文件详细配置：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Logout04.png" alt="image-20221021172843246" style="zoom:80%;" />
