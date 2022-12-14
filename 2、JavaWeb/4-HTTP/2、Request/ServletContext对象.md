# ServletContext对象

## 1、<span style="color:brown">基础内容：</span>

**1.1、概念：**

```apl
代表整个web应用, 可以和服务器通信
```

**1.2、获取：**

- 通过request对象获取：

  ```apl
  req.getServletContext()
  ```

- 通过HttpServlet获取：

  ```apl
  this.getServletContext()
  ```

<span style="color:Orange">**注意：**</span><u>以上两种方式获取的ServletContext对象，是同一个对象</u>

**1.3、功能：**

1. 获取`MIME类型`；
2. <u>ServletContext对象是一个域对象</u>：**共享数据**；
3. <u>**获取文件的真实路径**，即：服务器路径</u>；



## 2、<span style="color:brown">获取MIME类型：</span>

**2.1、什么是MIME类型？**

```apl
在互联网通信过程中, 定义的一种'文件数据类型'
```

**2.2、获取MIME的方法：**

```java
String getMimeType(String file)
```

**2.3、演示：**

```java
@WebServlet("/context01")
public class ContextDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取ServletContext对象
        ServletContext context = this.getServletContext();
        //定义一个文件名称
        String filename = "a.jpg";
        //获取MIME类型
        String mimeType = context.getMimeType(filename);
        System.out.println(mimeType);//image/jpeg
    }
}
```



## 3、<span style="color:brown">ServletContext对象是域对象：</span>共享数据

**3.1、介绍：**

<u>ServletContext对象的范围 ：</u><span style="color:green">**可以访问所有用户的所有数据**</span>

**3.2、方法：**

```java
void setAttribute(String name, Object obj)
```

```java
Object getAttribute(String name)
```

```java
void removeAttribute(String name)
```

**3.3、演示：**在ServletContext对象中，不需要`RequestDispatcher对象`实现转发

在访问完"localhost:8080/TomcatProgram/context02"之后，在新开一个网页输入"localhost:8080/TomcatProgram/context03"，数据依旧可以获取成功！！！

```java
@WebServlet("/context02")
public class ContextDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        context.setAttribute("msg","Hello,world");
    }
}
```

```java
@WebServlet("/context03")
public class ContextDemo03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        Object mag = context.getAttribute("msg");
        System.out.println(mag);//Hello,world
    }
}
```



## 4、<span style="color:brown">获取文件的真实路径：</span>

**4.1、资源文件位置解析：**

1. 存放在<u>web文件</u>；
2. 存放在<u>WEB-INF文件</u>；
3. 存放在<u>src文件</u>：
   - src文件下的内容，最终会存储`在/web/WEB_INF/classes文件目录`下；
   - 因此，可以直接获取`/web/WEB_INF/classes文件目录`下的资源文件；

**4.2、方法：**

```java
String getRealPath(String path);
```

**4.3、演示：**

```java
@WebServlet("/context04")
public class ContextDemo04 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        //web目录的资源访问
        String b = context.getRealPath("/b.txt");
        //WEB-INF目录的资源访问
        String c = context.getRealPath("/WEB-INF/c.txt");
        //src目录的资源访问
        String a = context.getRealPath("/WEB-INF/classes/a.txt");
        System.out.println(a);
        System.out.println(b);
        System.out.println(c);
    }
}
```
