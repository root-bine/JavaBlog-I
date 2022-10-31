# 配置高级

## 1、<span style="color:brown">临时属性：</span>

**1.1、CMD环境下如何设置临时属性？**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure.png" alt="image-20221020133311389" style="zoom:80%;" />

**1.2、属性加载优先级：**

设置的临时属性，可以覆盖项目配置文件中的一些属性，原因在于：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure01.png" alt="image-20221020134009495" style="zoom: 80%;" />

**1.3、IDEA环境下如何测试临时属性？**

- 带属性启动SpringBoot程序，为程序添加运行属性：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure02.png" alt="image-20221020135938510" style="zoom:67%;" />

- 通过编程形式为带参数启动SpringBoot程序，添加新的运行参数：

  ![image-20221020140230893](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure03.png)

- 不携带参数启动SpringBoot程序：

  ![image-20221020140347472](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure04.png)

## 2、<span style="color:brown">配置文件分级：</span>

**2.1、Spring Boot中配置文件4级：**

![image-20221020160157131](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure05.png)

虽然文件类型相同，但是优先级不一样。因此，针对于文件的内容读取原则就是：

```apl
1. 相同配置内容, 优先级高的覆盖较低的;
2. 各自没有的配置内容, 互相兼容;
```

**2.2、作用：**

![image-20221020160300158](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure06.png)

**2.3、自定义配置文件：**

- 通过启动参数加载配置文件：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure07.png" alt="image-20221020162150277" style="zoom:80%;" />

- 通过启动参数**加载指定路径下**的配置文件：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure08.png" alt="image-20221020162243651" style="zoom: 80%;" />

- 启动参数*同时加载**多个**指定路径下的配置文件*：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure09.png" alt="image-20221020162434406" style="zoom:80%;" />

**2.3、自定义配置文件的使用需求：**

![image-20221020162733542](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/configure10.png)