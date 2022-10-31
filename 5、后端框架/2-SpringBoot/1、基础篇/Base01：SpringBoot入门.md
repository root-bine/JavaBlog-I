# 快速上手SpringBoot

## 1、<span style="color:brown">SpringBoot入门案例：</span>idea联网版

**1.1、创建方式：**

通过IDEA创建一个空白项目，然后在**File中选择Setting选项中搜索Mavan**，设置Maven的基本配置。

进入SpringBoot项目创建页面有两种方式：

1. File -> Project Structure -> Modules -> 选中项目，鼠标右键 -> add -> new Module -> 进入Module配置界面，选择Spring Initializr 

2. 在空白项目界面，选择当前项目，鼠标右键 -> 点击Module -> 进入Module配置界面，选择Spring Initializr

**1.2、Spring Initializr配置：**

在使用IDEA创建SpringBoot项目时，可以设置Service URL的参数：

```apl
【国内】https://start.aliyun.com
【国外】https://start.spring.io
```



<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Spring%20Initializr.png" alt="image-20221004113311736" style="zoom:80%;" />

选择next，进入依赖导入界面，首先确定Spring Boot版本，然后选择相应的依赖进行导入！！！

**1.3、相关注解详解：**

|     @Controller     | 负责标识这个类是个Controller类，Spring检测到@Controller注解时，将其自动注册为Controller对象 |
| :-----------------: | :----------------------------------------------------------: |
| **@RestController** | **相当于@Controller + @ResposeBody**<span style="color:brown">**【REST风格开发模式使用】**</span> |
|   **@GetMapping**   |          **处理get请求，@GetMapping("/get/{id}")**           |
|  **@PostMapping**   |          **处理post请求，PostMapping("/get/{id}")**          |
|   **@PutMapping**   | 和PostMapping作用等同，都是用来向服务器提交信息。如果是添加信息，倾向于用@PostMapping，如果是更新信息，倾向于用@PutMapping。两者差别不是很明显 |
| **@DeleteMapping**  | 对于不需要的数据信息，客户端可以通过HTTP DELETE请求来要移除某个资源。而 @DeleteMapping 注解就能够非常便捷的声明能够处理DELETE请求的方法 |
| **@RequestMapping** | 是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。 |
|  **@ResponseBody**  | 将方法的返回值以 `json/xml` 的数据格式返回给客户端，如果是`JavaBean`对象，调用 `getxxx()`方法获取属性值，如果是 `map`集合，调用 `get(key)`方法获取属性值，然后以键值对的方式转成 `json字符串` |
|  **@RequestBody**   | 通过HttpMessageConverter读取Request Body并反序列化为Object（泛指）对象 |

​	

## 2、<span style="color:brown">SpringBoot入门案例：</span>官网创建版		

**2.1、创建方式：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%AE%98%E7%BD%91%E5%88%9B%E5%BB%BASpringBoot.png" alt="image-20221004154704798" style="zoom:80%;" />

上述设置完成之后，点击下方的`Generate按钮`，之后会在网页中下载一个文件压缩包。

**2.2、项目导入：**

点击File，选择Project Structure -> 进入界面，点击Modules -> 添加解压后的项目 

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E5%AF%BC%E5%85%A5%E5%A4%96%E6%9D%A5%E9%A1%B9%E7%9B%AE.png" alt="image-20221004160124276" style="zoom:80%;" />



## 3、<span style="color:brown">SpringBoot：</span>隐藏文件或者文件夹

**3.1、原因：**

在创建SpringBoot基础项目时，无论采用何种方式，都会产生如下无用文件：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E9%A1%B9%E7%9B%AE%E7%9A%84%E6%97%A0%E7%94%A8%E6%96%87%E4%BB%B6.png" alt="image-20221004163344106" style="zoom:80%;" />

因此，可以在IDEA中进行配置，让这些文件进行隐藏。这样在之后的创建SpringBoot项目时，就只会显示核心重要的文件！！！

**3.2、操作：**

点击File，选择Settings -> 搜索File Types

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%BF%BD%E7%95%A5%E6%96%87%E4%BB%B6%E5%92%8C%E7%9B%AE%E5%BD%95%E6%93%8D%E4%BD%9C.png" alt="image-20221004163910776" style="zoom: 67%;" />

在其中输入：

```apl
.mvn
.gitignore
HELP.md
mvnw
mvnw.cmd
*.iml
```



## 4、<span style="color:brown">SpringBoot：</span>复制工程

**4.1、原则：**

- 保留工程的基础结构
- 抹除原始工程的痕迹

**4.2、修改过程：**

![image-20221006005556089](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E4%B9%8B%E5%A4%8D%E5%88%B6%E5%B7%A5%E7%A8%8B.png)
