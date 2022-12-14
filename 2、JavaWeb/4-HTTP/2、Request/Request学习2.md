# Request进阶

## 1、<span style="color:brown">获取请求头数据：</span>

**1.1、<u>方法：</u>**

- 通过**请求头的名称**，获取请求头的值：

  ```java
  String getHeader(String name)
  ```

- 获取所有请求头名称：

  ```java
  Enumeration<String> getHeaderNames()
  ```

**1.2、Enumeration<E>：**

**<u>功能：</u>**

```apl
该接口的功能由'Iterator<E>接口'复制
```

**<u>方法：</u>**

类似于Iterator<E>接口，如下：

```java
boolean hasMoreElements()
```

```java
E nextElement()
```

**1.3、演示：**

```java
@WebServlet("/request02")
public class requestDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Enumeration<String> names = req.getHeaderNames();
        while(names.hasMoreElements()){
            String element = names.nextElement();
            String header = req.getHeader(element);
            System.out.println(header+"---"+element);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

```apl
localhost:8080---host
keep-alive---connection
" Not A;Brand";v="99", "Chromium";v="102", "Microsoft Edge";v="102"---sec-ch-ua
?0---sec-ch-ua-mobile
"Windows"---sec-ch-ua-platform
1---upgrade-insecure-requests
......
```



## 2、<span style="color:brown">获取请求体数据：</span>

**2.1、请求体：**

```apl
1. 只有'POST请求方式', 才会有请求体的存在

2. 在请求体中，封装了POST请求的'请求参数'
```

**2.2、获取请求参数：**

1. 获取流对象：

   ```java
   BufferedReader getReader()
       获取'字符输入流', 只能操作'字符数据'
   ```

   ```java
   ServletInputStream getInputStream()
       1. 获取'字节输入流', 可以操作'所有类型的数据'
       2. 文件上传中常用
   ```

2. 从流对象拿取数据；

**2.3、演示：**

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
</head>
<body>
    <form action="/TomcatProgram/request03" method="post">
        <input type="user" placeholder="请输入用户名:" name="username"><br>
        <input type="pwd" placeholder="请输入密码:" name="password"><br>
        <input type="submit" value="注册">
    </form>
</body>
</html>
```

```java
@WebServlet("/request03")
public class requestDemo03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        BufferedReader reader = req.getReader();
        String line = null;
        while((line = reader.readLine())!=null){
            System.out.println(line);//username=admin&password=123
        }
    }
}
```

