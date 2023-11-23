## 1、<span style="color:brown">Windows：</span>

**1.1、如何进行项目打包？**【有潜在问题】

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%89%93%E5%8C%85%E4%B8%8E%E8%BF%90%E8%A1%8C01.png" alt="image-20221018212500417" style="zoom: 67%;" />

**1.2、执行与关闭打包文件：**

在IDEA中找到打包好的项目文件，然后右键找到`open in`，选择Explore。

然后**在当前资源管理器路径中，执行CMD命令**。进入CMD面板之后，输入：

```apl
java -jar Xxxx.jar				[在输入项目名称时, 可输入首字母, 然后点击Tab犍进行补全]
```

---

在需要关闭该程序在Windows上的运行时，在CMD面板中执行按键：Ctrl + C，而cls可以清除当前显示窗口中的内容！！！

**1.3、新增数据问题：**

在完成上述操作之后，并成功启动项目，就可以在登录页面，但<span style="color:red">**发现在页面中出现了许多新数据**：</span>

![image-20221018213448129](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%89%93%E5%8C%85%E4%B8%8E%E8%BF%90%E8%A1%8C02.png)

---

出现上述问题的原因在于：<u>***Spring Boot项目在进行打包时，会执行测试操作，从而产生了许多新的数据***</u>！！！

因此，可以使用IDEA生命周期的工具进行跳过tests操作：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%BF%90%E8%A1%8C%E4%B8%8E%E6%89%93%E5%8C%8503.png" alt="image-20221018213942305" style="zoom:67%;" />

**1.4、端口被占用问题：**

*<u>问题描述：</u>*

```apl
有时在运行程序时, 可能会出现端口被占用, 且无法关闭的情况
```

<u>*解决方案：*</u>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%89%93%E5%8C%85%E4%B8%8E%E8%BF%90%E8%A1%8C04.png" alt="image-20221019113855083" style="zoom:80%;" />



## 2、<span style="color:brown">打包插件：</span>

**2.1、作用：**

<u>***jar支持命令行启动需要依赖maven插件支持，请确认打包时是否具有SpringBoot对应的maven插件***</u>

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <version>2.3.7.RELEASE</version>
</plugin>
```

**2.2、是否使用打包插件的区别：**

如下图，则为是否使用打包插件的区别：

<!--点击BOOT.INF > 点击classes, 就会显示出跟右侧目录内容一样的结构-->

<!--在BOOT.INF中的ilb下, 存放着springboot项目所有的依赖-->

<!--而org文件, 包含了启动Boot程序对应的一些类加载器, 比如: JarLauncher.class-->

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%89%93%E5%8C%85%E4%B8%8E%E8%BF%90%E8%A1%8C05.png" alt="image-20221019110258771" style="zoom:67%;" />

而两种情况都有**META-INF文件**，其内部都有`MANIFEST.MF`文件，其内容对比为：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%89%93%E5%8C%85%E4%B8%8E%E8%BF%90%E8%A1%8C06.png" alt="image-20221019111353717" style="zoom: 50%;" />

---

*<u>现在运行没有打包插件的jar文件</u>*，会出现如下情况：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%89%93%E5%8C%85%E4%B8%8E%E8%BF%90%E8%A1%8C07.png" alt="image-20221019110344506" style="zoom:67%;" />

那么由上述内容分析可知，SpringBoot打包文件能够正常运行依赖于：

1. 在BOOT.INF下：
   - **classes文件存放<u>所有的程序</u>**；
   - **lib文件存放<u>所有的依赖</u>**；
2. 在org文件中，存放一个独立运行Boot程序的工具；
