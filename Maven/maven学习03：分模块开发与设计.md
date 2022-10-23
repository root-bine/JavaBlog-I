# Maven学习03

## 1、<span style="color:brown">分模块开发：</span>

**1.1、模块分割开发：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module01.png" alt="image-20221023134442724" style="zoom: 50%;" />

**1.2、配置maven项目应用运行：**

如果采用Maven分模块开发与设计，那么服务器的启动必须是以Maven为基础，设置为：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module02.png" alt="image-20221023135727875" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module03.png" alt="image-20221023140631201" style="zoom: 50%;" />

在添加tomcat插件前，查看maven中的Plugins没有加入tomcat插件，因此需要在pom.xml添加：

```xml
<build>
  	<plugins>
  <!-- tomcat插件控制 -->
	<plugin>
	    <groupId>org.apache.tomcat.maven</groupId>
	    <artifactId>tomcat7-maven-plugin</artifactId>
	    <version>2.2</version>
	    <configuration>
            <--端口控制-->
			<port>8080</port>
            <--项目路径控制意味着http://localhost:8080/abc-->
			<path>/abc</path>
            <--编码-->
			<uriEncoding>UTF-8</uriEncoding>
		</configuration>
	</plugin>
  <!-- maven插件控制 -->
  		<plugin>
  			<groupId>org.apache.maven.plugins</groupId>
  			<artifactId>maven-compiler-plugin</artifactId>
  			<version>3.1</version>
  			<configuration>
  				<source>1.8</source>
  				<target>1.8</target>
  				<encoding>utf-8</encoding>
  			</configuration>
  		</plugin>
	
	</plugins>
</build>
```


## 2、<span style="color:brown">分模块设计：</span>

 **2.1、分割domain层实体类：**`ssm_pojo`

新建一个模块，采用Maven结构框架，不需要选择create from archetype。

<u>*在拆解模块时，根据需要拆解的模块，创建一样的包，便于文件的复制移动*</u>！！！

完成上述步骤之后，**只需要使用新建立的模块生命周期——compiles，如果编译通过，则已完成拆解**！！

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module04.png" alt="image-20221023141456350" style="zoom: 50%;" />

**2.2、分割dao层：**`ssm_dao`

<!--始终保留applicationContext.xml, 名称需要变化, 其内容也根据拆分内容发生变化-->

拆分步骤与Pojo一样，操作如下：复制代码、pom.xml文件依赖导入、资源文件配置等。

<span style="color:cyan">因为在dao层的数据类中，调用了实体类，但是实体类已经拆分成一个新的Maven模块，因此需要进行导入Pojo的坐标</span>！！！

<u>*关键点：*</u>

1. 在dao层的pom.xml中加入Pojo的坐标GAV；
2. 完成坐标导入后，Pojo还未进入仓库中，因此需要点击Pojo模块的声明周期——install，将其加载进仓库！！！
3. 检查资源文件中是否存在错误、是否存在不必要配置；

**2.3、分割service层：**`ssm_service`

<!--始终保留applicationContext.xml, 名称需要变化, 其内容也根据拆分内容发生变化化-->

按照Pojo、dao的操作完成：代码复制、pom.xml文件依赖导入、资源文件配置等。

在service层需要调用dao层的类，因此**需要将dao层模块的坐标导入到service的pom.xml中**，<u>*然后执行dao层生命周期——install*</u>！！！

<u>*如果需要进行单元测试，关键问题在于加载配置文件applicationContext.xml*</u>，方式有两种：

1. 将dao层的配置文件、service的配置文件写在`@ContextConfiguration(locations={...,...})`中；
2. 直接写一个完整的applicationContext.xml文件，包含dao层和service层全部内容；

**2.4、分割controller层：**`ssm_controller`

<!--保留sprinmvc.xml文件-->

由于controller是为表现层使用，因此在创建新模块时需要选择：`create for archetype --->  maven-archetype-webapp`。

操作基本跟前几个模块分割一致，如：代码复制、pom.xml依赖导入、资源文件配置、**webapp文件内容保留**等

在controller层需要调用service层的类，因此**需要将service层模块的坐标导入到controller的pom.xml中**，<u>*然后执行dao层生命周期——install*</u>！！！

**<u>隐藏问题：</u>**在webapp文件的web.xml文件中需要修改如下内容：![image-20221023151050047](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module05.png)

由于dao层的配置文件为：`applicationContext-dao.xml`、service层的配置文件为：`applicationContext-service.xml`！！！
