## 1、<span style="color:brown">SpringMVC的数据请求：</span>参数使用的问题解决方案

**1.1、静态资源访问的开启：**

在对于使用Ajax情形的集合参数获取时，会出现**出现无法访问到静态资源(如Jq的文件)的问题**，

其中的问题原因：
<u>***我们这是MVC程序，这里配置的核心控制器***</u>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E6%97%A0%E6%B3%95%E8%AE%BF%E9%97%AE.png" alt="image-20220726232146346" style="zoom:67%;" />

**下面是缺省"/"**，表示`所有的请求都会过这里，让这里进行处理`，<span style="color:green">然后我们设置的静态资源的路径也会被当成`@RequestMapping`的映射地址来处理，自然找不到</span>。

---

解决方法：

```xml
<!--在spring-mvc.xml文件中进行配置-->
<mvc:default-servlet-handler/>
```

**1.2、请求数据的乱码问题：**

在进行post的请求时，一般情况下在控制台会出现乱码情况。只需要在`web.xml文件`中进行如下配置：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%AF%B7%E6%B1%82%E6%95%B0%E6%8D%AE%E7%9A%84%E4%B9%B1%E7%A0%81%E9%97%AE%E9%A2%98.png" alt="image-20220726232724269" style="zoom: 80%;" />

**1.3、参数绑定的注解：**

![image-20220726233407988](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8F%82%E6%95%B0%E7%BB%91%E5%AE%9A%E6%B3%A8%E8%A7%A3.png)

**1.4、获得`Restful风格`的参数：**

- 概述：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Restful%E9%A3%8E%E6%A0%BC%E6%A6%82%E8%BF%B0.png" alt="image-20220726234438984" style="zoom:80%;" />

- 演示：

  ![image-20220726234529568](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Restful%E9%A3%8E%E6%A0%BC%E8%AF%A6%E8%A7%A3.png)



## 2、<span style="color:brown">SpringMVC的数据请求：</span>自定义转换器【了解】

**2.1、使用自定义转换器的原因：**

```apl
1. 我们在前端页面向后台传递的参数值，都是以字符串的形式传递到后端
2. 但是我们可以看到我们定义的整形的参数，也可以接受到前端页面传递的整形的参数。
# 这是因为MVC会将我们前端的某些字符进行初步类型转换。
```

**2.2、创建自定义转换器的步骤：**

1. 定义转换器类实现`Converter接口`
2. 在配置文件中声明转换器
3. 在其中引用转换器

**2.3、代码演示：**

此处是以**自定义一个Date类型的数据转换作为例子**：

![image-20220726235553428](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BD%AC%E6%8D%A2%E5%99%A801.png)

- 定义转换器类实现`Converter接口`：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BD%AC%E6%8D%A2%E5%99%A802.png" alt="image-20220726235708186" style="zoom:67%;" />

- 在`spring-mvc.xml配置文件`中声明转换器：

  ![image-20220727000047404](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BD%AC%E6%8D%A2%E5%99%A803.png)

- 在其中引用转换器，即：在MVC注解驱动中加入部分内容：

  ![image-20220727000556160](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BD%AC%E6%8D%A2%E5%99%A804.png)

- 结果：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BD%AC%E6%8D%A2%E5%99%A805.png" alt="image-20220727000642243" style="zoom: 67%;" />



## 3、<span style="color:brown">SpringMVC的数据请求：</span>其他设置方案

**3.1、获取Servlet相关API：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%8E%B7%E5%8F%96Servlet%E7%9B%B8%E5%85%B3API.png" alt="image-20220727001156565" style="zoom: 80%;" />

<span style="color:red"><u>***直接形参传入，就可直接使用，MVC看到我们需要，则会自动帮我们在这里注入这些常用的东西***</u></span>

**3.2、获取请求头：**

- **`@RequestHeader`**：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/RequestHeader.png" alt="image-20220727002421375" style="zoom:80%;" />

- **`@CookieValue`**：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/CookieValue.png" alt="image-20220727002535900" style="zoom:80%;" />

