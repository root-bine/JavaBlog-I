# Request补充

## 1、<span style="color:brown">获取请求参数：</span>【通用模式】不论是GET/POST方式，都允许！！

**1.1、通用请求参数的方法：**

- 根据参数名称，获取参数的值：

  ```java
  String getParameter(String name)
  ```

- 根据参数名称，获取参数的数值数组：

  ```java
  String[] getParameterValues(String name)
  ```

- 获取所有的请求参数名称：

  ```java
  Enumeration<String> getParameternames()
  ```

- 获取所有参数的map集合：

  ```java
  Map<String,String[]> getParameterMap()
  ```

**1.2、通用获取参数的中文乱码问题：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
</head>
<body>
    <!--此处method取值: get  /   post-->
    <form action="/TomcatProgram/request04" method="get">
        <input type="user" placeholder="请输入用户名:" name="username"><br>
        <input type="pwd" placeholder="请输入密码:" name="password"><br>
        <input type="checkbox" name="hobby" value="study">学习
        <input type="checkbox" name="hobby" value="game">游戏
        <br>
        <input type="submit" value="注册">
    </form>
</body>
</html>
```

![img](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%8101.png)

- GET方式：

  ```apl
  Tomcat8以后, 已经解决了关于中文乱码的问题
  ```

  代码：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%8102.png" alt="image-20220619153627187" style="zoom:67%;" />

控制台：

```java
张三
```

- POST方式：

  ```apl
  出现乱码问题, 设置:
  	req.setCharacterEncoding("utf-8")
  ```

  最终代码：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%8103.png" alt="image-20220619154323480" style="zoom:67%;" />

控制台输出：

```java
未修改:
		？？
修改后:
		张三
```



## 2、<span style="color:brown">请求转发：</span>

**2.1、概念：**

<u>一种在服务器内部的资源跳转方式</u>

**2.2、步骤：**

1. 通过`request对象`获取**请求转发器对象**：

   ```java
   RequestDispatcher getRequestDispatcher(String var1)
       String var1: Servlet访问路径
   ```

2. 使用`RequestDispatcher对象`实现转发：

   ```java
   void forward(ServletRequest var1, ServletResponse var2)
   	ServletRequest var1与ServletRequest var2:
   		传入request和response对象
   ```

<span style="color:red">**2.3、特点：**</span>

[^资源转移]: 与重定向相反

1. 浏览器路径栏中，访问地址不变；

2. 只能转发当前服务器的内部资源；

3. <u>转发是一次请求</u>：

   ```apl
   可以使用Requset对象共享数据
   ```

**2.4、演示：**

```java
@WebServlet("/request05")
public class requestDemo05 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("accept demo05...");
        //将Demo05的资源, 转发到demo06
        RequestDispatcher dispatcher = req.getRequestDispatcher("/request06");
        dispatcher.forward(req,resp);
    }
}
```

```java
@WebServlet("/request06")
public class requestDemo06 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("accept demo06...");
    }
}
```

控制台：

```apl
accept demo05...
accept demo06...
```



## 3、<span style="color:brown">共享资源：</span>

**3.1、域对象：**

```apl
一个有作用范围的对象, 在作用范围内可以共享数据
```

**3.2、request域：**

```apl
1. 代表一次请求的范围; 

2. 一般在请求转达多个资源中共享资源;
```

**3.3、核心方法：**

- 存储数据：

  ```java
  void setAttribute(String name, Object obj)
  ```

- 获取数据：

  ```java
  Object getAttribute(String name)
  ```

- 移除数据：

  ```java
  void removeAttribute(String name)
  ```

**3.4、演示：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE01.png" alt="image-20220619164408468" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE02.png" alt="image-20220619164440870" style="zoom:67%;" />



