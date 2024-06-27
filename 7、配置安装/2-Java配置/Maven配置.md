## 1、<span style="color:brown">安装Maven：</span>

在官网下载Maven压缩包：https://maven.apache.org/download.cgi，在本地创建Maven文件夹用于存放解压内容。

![image-20240628011746966](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven_Configuration.png)



## 2、<span style="color:brown">配置环境变量：</span>

在环境变量中，添加变量：`MAVEN_HOME`，其值为：`D:\Java\Maven\apache-maven-3.8.4`。

进入Path，添加内容：`%MAVEN_HOME%\bin`。

最后，`win+R`运行`cmd`，输入`mvn -version`，如图所示则配置成功。



## 3、<span style="color:brown">配置本地仓库：</span>

在**`D:\Java\Maven`目录**下，创建**`maven-repository`文件夹**，<u>用作`maven`的本地库</u>。。

在**`D:\Java\Maven\apache-maven-3.8.4\conf`目录**下，找到`settings.xml`文件：

- 找到节点**`localRepository`**，在注释外添加：

  ```xml
  <localRepository>D:\Java\Maven\maven-repository</localRepository>
  ```



## 4、<span style="color:brown">配置镜像：</span>

1. 在`settings.xml`配置文件中找到**`mirrors`节点**；
2. 添加如下配置（注意要添加在**<mirrors>和</mirrors>**两个标签之间，其它配置同理）；

```xml
<!-- 阿里云仓库 -->
<mirror>
	<id>alimaven</id>
	<mirrorOf>central</mirrorOf>
	<name>aliyun maven</name>
	<url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
</mirror>
```



## 5、<span style="color:brown">配置JDK：</span>

1. 在`settings.xml`配置文件中找到`profiles`节点

2. 添加如下配置：

   ```xml
   <!-- java版本 --> 
   <profile>
   	  <id>jdk-1.8</id>
   	  <activation>
   		<activeByDefault>true</activeByDefault>
   		<jdk>1.8</jdk>
   	  </activation>
   
   	  <properties>
   		<maven.compiler.source>1.8</maven.compiler.source>
   		<maven.compiler.target>1.8</maven.compiler.target>
   		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
   	  </properties>
   </profile>
   ```

   
