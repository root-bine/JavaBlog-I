# Maven学习01

## 1、<span style="color:brown">Maven基础：</span>

**1.1、maven概述：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven%E6%A6%82%E8%BF%B0.png" alt="image-20220706181509484" style="zoom:67%;" />

**1.2、Maven的作用：**

- **项目构建**：

  <u>提供标准的、跨平台的自动化项目构建方式</u>

- **依赖管理**：

  <u>管理项目依赖资源的jar包，避免版本冲突</u>

- **统一开发结构**：

  <u>提供标准的、统一的开发结构</u>

**1.3、Maven基础概念：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven%E4%BB%93%E5%BA%93.png" alt="image-20220706182959099" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven%E5%9D%90%E6%A0%87.png" alt="image-20220706194601816" style="zoom:80%;" />

## 2、<span style="color:brown">Maven配置：</span>

**2.1、Maven基础项目设置：**

首先在IDEA中创建一个基础的Maven项目，注意main、test两个文件中文件的颜色是否正确，然后在setting中配置maven，如下：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE.png" alt="image-20220706202455349" style="zoom: 67%;" />

在后期如果需要对Maven项目进行DBug测试的时候，需要配置测试、清理的功能，如下：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven%E6%96%AD%E7%82%B9%E6%B5%8B%E8%AF%95.png" alt="image-20220706202648949" style="zoom:67%;" />

**2.2、Maven使用模板创建子模块：**

常用两个子模块：

- Java项目模板：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven%E6%A8%A1%E6%9D%BF1.png" alt="image-20220706203342606" style="zoom:150%;" />

- web项目模板：

![image-20220706203433059](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven%E6%A8%A1%E6%9D%BF2.png)

**2.3、Maven的pom.xml内容分析：**

![image-20220706210812152](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Pom%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9.png)

**2.4、导入Tomcat插件：**

![image-20220706210919932](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/TomcatMaven.png)

