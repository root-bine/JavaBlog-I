# 入门案例解析

## 1、<span style="color:brown">SpringBoot简介：</span>

![image-20221004165256582](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E7%AE%80%E4%BB%8B.png)

**SpringBoot通过：<span style="color:green">parent、starter、引导类、内嵌Tomcat</span>，这个四部分完成如上三点步骤**！！！

|     parent     | SpringBoot中常见项目名称，<span style="color:green">定义了当前项目使用的所有依赖坐标</span>，以达到<span style="color:brown">减少依赖配置</span>的目的。 |
| :------------: | ------------------------------------------------------------ |
|  **starter**   | **所有SpringBoot项目要继承的项目，<span style="color:red">定义了若干个坐标版本号(依赖管理，而非依赖)</span>，以达到<span style="color:brown">减少依赖冲突</span>的目的** |
|   **引导类**   | **SpringBoot的引导类是Boot工程的执行入口，运行main方法就可以启动项目** |
| **内嵌Tomcat** | **SpringBoot的辅助功能之一**                                 |



## 2、<span style="color:brown">parent：</span>

**2.1、parent详解：**

在SpringBoot中，pom.xml文件：

```xml
<parent>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-parent</artifactId>
   <version>2.7.1</version>
   <relativePath/> <!-- lookup parent from repository -->
</parent>
```

其中的`spring-boot-starter-parent`定义了若干依赖，继承了`spring-boot-dependencies`的内容。

在**spring-boot-dependencies**中，定义了<u>一系列的properties自定义属性名</u>，还**定义了许多的坐标管理来引用这些属性**。

**2.2、Parent版本比较：**

`spring-boot-starter-parent`各版本间存在着诸多坐标版本不同！！！

如果采用**文件对比的方法**，操作如下：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%96%87%E4%BB%B6%E5%AF%B9%E6%AF%94.png" alt="image-20221004172554433" style="zoom:67%;" />



## 3、<span style="color:brown">starter：</span>

![image-20221004215243364](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/starter.png)

在实际开发中，使用情况如下：

```apl
1. 使用任意坐标时, 仅书写GAV中的G和A, v由SpringBoot提供, 除非SpringBoot未提供对应版本V
2. 如发生坐标错误, 再指定Version(要小心版本冲突)
```



## 4、<span style="color:brown">引导类：</span>

![image-20221004221653706](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBootConfiguration.png)

## 5、<span style="color:brown">内嵌Tomcat：</span>

**5.1、原理：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E7%9A%84%E8%BE%85%E5%8A%A9%E5%8A%9F%E8%83%BD%E4%B9%8B%E4%B8%80.png" alt="image-20221005002431135" style="zoom:80%;" />

**5.2、SpringBoot如何修改内嵌服务器：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E4%BF%AE%E6%94%B9%E5%86%85%E5%B5%8C%E6%9C%8D%E5%8A%A1%E5%99%A8.png" alt="image-20221005002811831" style="zoom:80%;" />

**5.3、SpringBoot内置服务器：**

|   种类   |                          分析                          |
| :------: | :----------------------------------------------------: |
|  Tomcat  | apache出品，受众较多，应用面更广，负载了若干较重的组件 |
|  Jetty   |             更轻量级，负载性能远不及Tomcat             |
| Undertow |                 负载性能勉强跑赢Tomcat                 |

