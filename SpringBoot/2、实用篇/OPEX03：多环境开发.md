# 多环境开发

## 1、<span style="color:brown">YAML版：</span>

**1.1、在多环境开发中，如何编写配置文件：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/development01.png" alt="image-20221020164251787" style="zoom: 67%;" />

**1.2、结果范例：**

![image-20221020164902862](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/development02.png)

**1.3、潜在问题解决：**

<u>*问题描述：*</u>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/development03.png" alt="image-20221020170856115" style="zoom: 67%;" />

<u>*解决方案：*</u>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/development04.png" alt="image-20221020171241341" style="zoom:67%;" />



## 2、<span style="color:brown">properties版：</span>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/development11.png" alt="image-20221020171657639" style="zoom:80%;" />

## 3、<span style="color:brown">多环境分组管理：</span>

**3.1、分组原则：**

根据**功能**对*<u>配置文件中的信息</u>*进行拆分，并制作成独立的配置文件，例如：

- application-devDB.yml
- application-devRedis.yml
- application-devMvC.yml

**3.2、<span style="color:red">加载配置信息：</span>**

使用`include属性`在激活指定环境的情况下，同时对多个环境进行加载使其生效，多个环境间使用逗号分隔

```yaml
spring:
	profiles:
		active: dev
		include: devDB, devRedis, devMvc
```

执行顺序为：`devDB, devRedis, devMvc, dev`，最终是：**dev生效**

---

从SpringBoot2.4版本之后，使用**group属性代替了include属性**

```yaml
spring:
	profiles:
		active: dev
		group: 
			"dev": devDB, devRedis, devMvc
			"pro": proDB, proRedis, deMvc
			"test": testDB, testRedis, testMvc
```

执行顺序为：`dev,  devDB, devRedis, devMvc`，最终是：**devMvc生效**



## 4、<span style="color:brown">多环境开发控制：</span>

**4.1、Maven与SpringBoot配置兼容问题：**

1. 在Maven的`pom.xml`配置**多环境属性**：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/development05.png" alt="image-20221020210428983" style="zoom:67%;" />

2. 在springboot中引用Maven属性：

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/development06.png" alt="image-20221020210652427" style="zoom:67%;" />

3. 执行Maven打包指令，并在生成的boot打包文件.jar文件中查看对应信息

**4.2、IDEA缓存问题：**

在Maven的pom.xml中配置多环境属性，但在实际运行时会出现：**无论如何修改默认启动环境，显示的启动结果可能始终不变**。

<u>***之后尝试使用maven生命周期的clean，但依旧无效***</u>。

此时需要执行生命周期-----`compile【手工编译】`，<span style="color:orange">重新加载pom.xml中的属性</span>！！！

