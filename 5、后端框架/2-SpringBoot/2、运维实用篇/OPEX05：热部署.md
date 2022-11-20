## 1、<span style="color:brown">手动开启hot deployment：</span>

**1.1、开启开发者工具：**`pom.xml`

```xml
<!--热部署-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

**1.2、激活热部署：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/hot%20deployment01.png" alt="image-20221021183157392" style="zoom:67%;" />

**1.3、关于热部署：**

<u>*分析：*</u>

- **重启(Restart)**:自定义开发代码，包含类、页面、配置文件等，加载位置restart类加载器
- **重载(ReLoad)** : jar包，加载位置base类加载器

---

<u>*区别：*</u>

- 热部署：只有重启，只加载资源文件等，耗时更短 

- 重新启动：重启+重载,加载资源文件和jar包,耗时更长



## 2、<span style="color:brown">自动开启热部署：</span>

**2.1、开启开发者工具：**`pom.xml`

```xml
<!--热部署-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

**2.2、IDEA配置：**

- 点击File，选择Settings：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/hot%20development02.png" alt="image-20221021190152122" style="zoom: 67%;" />

- 在Settings界面中，找到`Advanced  Settings`：

  ![image-20221021191021466](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/hot%20development03.png)

**2.3、激活热部署：**

***<span style="color:cyan">IDEA失去焦点5秒后，自动进行热部署</span>***



## 3、<span style="color:brown">热部署范围配置与关闭：</span>

**3.1、默认不触发重启的目录列表：**

![image-20221021214944315](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/hot%20development04.png)

**3.2、自定义不参与重启排除项：**

```yaml
devtools:
  restart:
    exclude: static/**, public/**, resource/**
```

**3.3、关闭热部署功能：**

直接在Spring Boot项目的启动类中进行属性配置【参考：[配置高级]：1.2、属性加载优先级】：

![image-20221021220453276](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/hot%20development05.png)
