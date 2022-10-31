# SpringMVC学习04

## 1、<span style="color:brown">SpringMVC的数据请求：</span>文件上传

**1.1、文件上传客户端三要素：**

在`webapp文件目录`下，创建一个upload.jsp文件，实现一个表单功能！！！

此处表单中的文件模块，其中**upload.jsp的属性值名称，要跟UserController类中方法的参数列表的名称一致**。

![image-20220806104331687](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%B8%89%E8%A6%81%E7%B4%A0.png)

**1.2、文件上传的原理：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E7%9A%84%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E5%8E%9F%E7%90%86.png" alt="image-20220806105034556" style="zoom:80%;" />



## 2、<span style="color:brown">SpringMVC的数据请求：</span>单文件上传

**2.1、单文件上传步骤：**

![image-20220806105412886](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8D%95%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E6%AD%A5%E9%AA%A4.png)

**2.2、注意事项：**

在pom.xml文件中导入的坐标除了：`spring-context、spring-web、spring-webmvc、servlet-api、javax.servlet.jsp-api`

如果需要**完成文件上传**，还要导入：<u>`commons-fileupload、commons-io`</u>两个坐标

```xml
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.4</version>
</dependency>
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>1.4</version>
</dependency>
```

---

之后就需要在spring-mvc.xml配置文件中配置：***<u>文件上传解析器</u>***，内容如下：

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!--上传文件总大小-->
    <property name="maxUploadSize" value="5242800"/>
    <!--上传单个文件的大小-->
    <property name="maxUploadSizePerFile" value="5242800"/>
    <!--上传文件的编码类型-->
    <property name="defaultEncoding" value="UTF-8"/>
</bean>
```

---

最后，在UserController中**<u>编写文件上传代码</u>**，内容如下：

```java
@RequestMapping(value = "/upload")
@ResponseBody
// 参数名称, 与upload.jsp的属性值一致
public void upload(String name, MultipartFile uploadFile) throws IOException {
    // 获取文件名称
    String originalFilename = uploadFile.getOriginalFilename();
    // 保存文件
    uploadFile.transferTo(new File("C:\\Users\\root-bine\\Desktop\\upload\\"+originalFilename));
}
```

**2.3、结果：**

提前编写一个**有内容的普通txt文件**和**一个空白文件夹**，然后再浏览器中输入: http://localhost:8080/Spring_Web/upload.jsp，就会直接跳转到事先编写好的表单中，之后输入名称和找寻需要上传的文件即可



## 3、<span style="color:brown">SpringMVC的数据请求：</span>多文件上传

**3.1、在upload.jsp文件中编写表单：**

```xml
<form action="${pageContext.request.contextPath}/user/upload" method="post" enctype="multipart/form-data">
    名称<input type="text" name="username"><br/>
    文件1<input type="file" name="uploadFile"><br/>
    文件2<input type="file" name="uploadFile"><br/>
    <input type="submit" value="提交">
</form>
```

**3.2、UserController类的编写：**

由于需要上传多个文件，我们无需为每一个文件的上传，在表单中再设置其他的名字。因此可以再form表单中保持文件名称一致，在UserController类中的`upload()`方法中可以传入`MultipartFile[] uploadFile`的一个数组变量

```java
public void upload(String name, MultipartFile[] uploadFile) throws IOException {
    for (MultipartFile multipartFile : uploadFile) {
        String originalFilename = multipartFile.getOriginalFilename();
        multipartFile.transferTo(new File("C:\\Users\\root-bine\\Desktop\\upload\\"+originalFilename));
    }
}
```
