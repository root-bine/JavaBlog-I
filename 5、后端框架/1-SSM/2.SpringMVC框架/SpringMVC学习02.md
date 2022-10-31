# SpringMVC学习02

## 1、<span style="color:brown">SpringMVC的数据响应：</span>

**1.1、响应方式：**

|          页面跳转          |      回写数据      |
| :------------------------: | :----------------: |
|       直接返回字符串       |   直接返回字符串   |
| 通过`ModelAndView对象`返回 | 返回`对象或者集合` |

**1.2、页面跳转详解：**

1. <span style="color:green"><u>直接返回字符串：</u></span>

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E5%93%8D%E5%BA%94%E4%B9%8B%E9%A1%B5%E9%9D%A2%E8%B7%B3%E8%BD%AC01.png" alt="image-20220723213720542" style="zoom:80%;" />

2. <span style="color:violet">通过`ModelAndView对象`返回：</span>

   ```apl
   Model：模型，封装数据
   ---------------------------------------------------------------------------------------------------------
   View：视图，展示数据
   ```


3. 代码演示：

   <u>***此处的Web.xml、spring-mvc.xml文件内容不变***</u>！！！

   ```xml
   <!--success.jsp-->
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>success</title>
   </head>
   <body>
       <h1>spring-MVC启动成功...${username}</h1>
   </body>
   </html>
   ```

   ```java
   @Controller
   @RequestMapping("/user")
   public class UserController {
       // 第一种方式: 通过创建ModelAndView对象,并作为返回值
       @RequestMapping(value = "/model1")
       public ModelAndView reSave1(){
           ModelAndView modelAndView = new ModelAndView();
           // 设置视图名称
           // 原本字符串设置为: /JSP/success.jsp
           // 于在spring-mvc中设置了视图解析器,将前缀和后缀进行了声明
           // 因此,这里就是只需要编写"success"即可
           modelAndView.setViewName("success");
           // 设置模型数据,此处在success.jsp中进行获取
           modelAndView.addObject("username","itZGY");
           return modelAndView;
       }
       
       // 第二种方式: 通过传入ModelAndView对象,并作为返回值
       @RequestMapping(value = "/model2")
       public ModelAndView reSave2(ModelAndView modelAndView){
           modelAndView.setViewName("success");
           modelAndView.addObject("username","itRoot");
           return modelAndView;
       }
       
       // 第三种方式: 通过将视图与模型进行分割,参数传入Model对象,返回一个字符串数据
       @RequestMapping(value = "/model3")
       public String reSave3(Model model){
           model.addAttribute("username","itBine");
           return "success";
       }
       
       // 第四种方式: 不通过SpringMVC进行数据封装,使用最原始的数据域封装
       @RequestMapping(value = "/model4")
       public String reSave4(HttpServletRequest request){
           request.setAttribute("username","root-bine");
           return "success";
       }
   ---------------------------------------------------------------------------------------------------------
       /*SpringMVC数据响应方式 ====> 返回字符串*/
       @RequestMapping(value = "/quack",method = RequestMethod.GET,params = {"username"})
       public String save(){
           System.out.println("Controller save running...");
           // 重定向
           //return "redirect:/JSP/success.jsp";
           return "success";
       }
   }
   ```

**1.3、回写数据详解：**

1. <span style="color:green"><u>返回**普通类型的字符串**：</u></span>**@ResponseBody**注解的功能：<u>`告知SpringMVC框架，不进行视图跳转，直接数据响应`</u>

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E5%9B%9E%E5%86%99%E6%95%B0%E6%8D%AE1.png" alt="image-20220724202242784" style="zoom:80%;" />

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E5%9B%9E%E5%86%99%E6%95%B0%E6%8D%AE2.png" alt="image-20220724202308346" style="zoom:80%;" />

2. <span style="color:green"><u>返回**Json类型的字符串**：</u></span>

   一般如果直接返回*`Json字符串`*，写法如：

   ```java
   // 斜杠表示转义
   return "{\"name\":\"zhangsan\",\"age\":18}"
   ```

   而在SpringMVC的框架下，要想使用**Jackson转换工具**，需要在<u>*pom.xml文件中导入jackson-core坐标、jackson-databind坐标、jackson-annotations坐标*</u>。

3. <span style="color:orange">返回`对象或者集合`：</span>

   如果<u>*需要将User类的user对象，通过SpringMVC执行，最终以Json类型字符串返回*</u>，那么就需要在spring-mvc.xml文件中进行**处理器映射器配置**：

   ```xml
   <!--配置处理器映射器-->
   <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
     <property name="messageConverters">
        <list>
           <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
        </list>
      </property>
   </bean>
   ```

   但是，可以使用**MVC的注解驱动**，简化了上述Json数据转化的过程，*`由于普通xml文件的命名空间没有mvc`*，需要配置：

   ```apl
   xmlns:mvc="http://www.springframework.org/schema/mvc"
   ---------------------------------------------------------------------------------------------------------
   http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
   ```

   <u>**关于**`<mvc:annotation-driven/>`**详解**：</u>

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MVC%E6%B3%A8%E8%A7%A3%E9%A9%B1%E5%8A%A8.png" alt="image-20220724225058240" style="zoom:80%;" />

**1.4、代码展示：**web.xml、success.jsp两个文件的代码不变！！！

- ***spring-mvc.xml文件部分内容：***

  ```apl
  <!--配置处理器映射器-->
  <!--<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
      <property name="messageConverters">
          <list>
              <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
          </list>
      </property>
  </bean>-->
  <mvc:annotation-driven/>
  ```

- ***UserController类：***

  ```java
  @Controller
  @RequestMapping("/user")
  public class UserController {
      // 回写数据: 普通字符串格式
      @RequestMapping(value = "/data1")
      public void source1(HttpServletResponse response) throws IOException {
          response.getWriter().print("hello root-bine!!!");
      }
      
      @RequestMapping(value = "/data2")
      @ResponseBody
      public String source2() {
          return "hello root-bine!!!";
      }
  ---------------------------------------------------------------------------------------------------------
      // 回写数据: Json数据格式
      @RequestMapping(value = "/data3")
      @ResponseBody
      public String source3() throws IOException {
          // 此处的User类包含String name、int age
          User user = new User();
          user.setName("lisi");
          user.setAge(18);
          // 使用json的转换工具, 将对象转换成json数据格式的字符串并返回
          ObjectMapper objectMapper = new ObjectMapper();
          String json = objectMapper.writeValueAsString(user);
          //return "{\"name\":\"zhangsan\",\"age\":18}";
          return json;
      }
  ---------------------------------------------------------------------------------------------------------
      // 回写数据: 返回对象 or 集合
      @RequestMapping(value = "/data4")
      @ResponseBody
      // 期望SpringMVC自动将User转换成Json格式字符串
      public User source4() {
          // 此处的User类包含String name、int age
          User user = new User();
          user.setName("lisi");
          user.setAge(18);
          return user;
      }
  }
  ```



## 2、<span style="color:brown">SpringMVC的数据请求：</span>参数的获取

**2.1、获得请求参数：**

![image-20220726210554541](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringMVC%E8%AF%B7%E6%B1%82%E6%95%B0%E6%8D%AE01.png)

**2.2、获得基本数据类型参数：**

![image-20220726211729910](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%8E%B7%E5%8F%96%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E5%8F%82%E6%95%B0.png)

这里需要加上@ResponseBody注解，起到**不进行数据跳转，直接回写数据**。

然后按照上述方式，只需要在***访问URL加上相应参数***，就可以在控制台打印出在浏览器页面的参数数据。

**2.3、获得POJO类型参数：**

![image-20220726214719314](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%8E%B7%E5%8F%96POJO%E7%B1%BB%E5%9E%8B%E5%8F%82%E6%95%B0.png)

<u>此类型数据的请求，***与基本数据类型参数的获得不同***</u>。

在方法中传入实体类参数的对象，<u>只需要在访问路径中加入`实体类中的参数名称`</u>，就可以实现在控制台打印数据。

**2.4、获得数组类型参数：**

![image-20220726215137290](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%8E%B7%E5%8F%96%E6%95%B0%E7%BB%84%E7%B1%BB%E5%9E%8B%E5%8F%82%E6%95%B0.png)

**2.5、获得集合类型参数：**

**<span style="color:blue">2.5.1、情形一：</span>**

集合的特殊性，我们不能直接注入，我们需要一种间接的方式【通过对象的属性来注入】：
首先创建一个参数由String username，int age的User类，然后再创建一个VO类：

```java
public class VO {
    private List<User> userList;
    public List<User> getUserList() {
        return userList;
    }
    public void setUserList(List<User> userList) {
        this.userList = userList;
    }
    @Override
    public String toString() {
        return "VO{" +"userList=" + userList +'}';
    }
}
```

<u>***我们要做的就是利用前台传入的参数创建并或得由多个Use对象组成的List集合***</u>，我们把我们创建的类放进去，所以，这里就等于是`获得POJO类型的参数，即通过请求参数构建对象`这一步的东西了：

```java
@RequestMapping(value = "/quack4")
@ResponseBody
public void save4(VO vo){
    System.out.println(vo);
}
```

然后，我们通过前端来向VO类中传入数据来构建List
我们要把对应的参数值传入对应的位置，那么我们就需要在获取参数上动手脚，看前端：

![image-20220726230556061](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/from%E6%BC%94%E7%A4%BA.png)

演示页面展示

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E9%9B%86%E5%90%88%E7%B1%BB%E5%9E%8B%E5%8F%82%E6%95%B0%E7%BB%93%E6%9E%9C01.png" alt="image-20220726230909800" style="zoom:80%;" />

![image-20220726230953197](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E9%9B%86%E5%90%88%E7%B1%BB%E5%9E%8B%E5%8F%82%E6%95%B0%E7%BB%93%E6%9E%9C02.png)

**<span style="color:blue">2.5.2、情形二：</span>**

<u>***这里，我们在一种特殊的情况下，我们直接在servlet中用集合的形参去接收前端返回回来的集合***</u>

`当使用ajax提交时`，可以设置Contentype为Json形式，那么在方法参数位置使用@RequestBody，可以直接接收集合数据而无需使用POJO进行包装。

- 前端部分：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E9%9B%86%E5%90%88%E7%B1%BB%E5%9E%8B%E5%8F%82%E6%95%B0%E5%89%8D%E7%AB%AF%E9%83%A8%E5%88%86.png" alt="image-20220726231440259" style="zoom:67%;" />

- 后端控制器部分：

  ![image-20220726231534107](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E9%9B%86%E5%90%88%E5%8F%82%E6%95%B0%E7%B1%BB%E5%9E%8B%E5%90%8E%E7%AB%AF%E6%8E%A7%E5%88%B6%E5%99%A8%E9%83%A8%E5%88%86.png)

- 结果：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E9%9B%86%E5%90%88%E7%B1%BB%E5%9E%8B%E5%8F%82%E6%95%B0Ajax%E6%83%85%E5%BD%A2%E7%BB%93%E6%9E%9C%E6%BC%94%E7%A4%BA.png" alt="image-20220726231619873" style="zoom:67%;" />
