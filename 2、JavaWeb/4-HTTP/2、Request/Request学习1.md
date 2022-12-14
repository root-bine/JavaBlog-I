# Request基础

## 1、<span style="color:brown">基础内容：</span>

**1.1、request对象和response对象：**

- request对象和response对象，**由服务器创建**，程序员只需要使用；
- request对象，用来获取请求信息；
- response对象，用来设置响应信息；

**1.2、Request继承体系：**

```apl
org.apache.catalina.connector.RequestFacade类
												extends
														  HttpServletRequest接口  extends  ServletRequest接口
```

**1.3、URI与URL：**

```apl
URI: 统一资源'标识符'

URL: 统一资源'定位符'
```

***补充：***`URI的检索范围  >  URL的检索范围`

## 2、<span style="color:brown">获取请求行数据：</span>

**2.1、请求行示例：**

`GET /day14/demo01?name=ZGY HTTP/1.1`

<u>方法如下：</u>

- 获取请求方法：`GET`

  ```java
  String getMethod()
  ```

- 获取虚拟目录：`/day14`

  ```java
  String getContextPath()
  ```

- 获取Servlet路径：`/demo01`

  ```java
  String getServletPath()
  ```

- 获取get方式请求的参数：`name=ZGY`

  ```java
  String getQueryString()
  ```

- 获取请求URI或者URL：

  - `/day14/demo01`

    ```java
    String getRequestURI()
    ```

  - `http://localhost/day14/demo01`

    ```java
    StringBuffer getRequestURL()
    ```

- 获取协议及其版本号：`HTTP/1.1`

  ```java
  String getProtocol()
  ```

- 获取客户机IP地址

  ```java
  String getRemoteAddr()
  ```

**2.2、演示：**

<u>代码：</u>

```java
@WebServlet(name = "requestServlet",urlPatterns = "/request")
public class requestDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getMethod();
        System.out.println("请求方法:"+method);
        String contextPath = req.getContextPath();
        System.out.println("虚拟目录:"+contextPath);
        String servletPath = req.getServletPath();
        System.out.println("Servlet路径:"+servletPath);
        String requestURI = req.getRequestURI();
        System.out.println("获取请求的URI:"+requestURI);
        StringBuffer requestURL = req.getRequestURL();
        System.out.println("获取请求的URL:"+requestURL);
        String protocol = req.getProtocol();
        System.out.println("获取版本协议及其信息:"+protocol);
        String remoteAddr = req.getRemoteAddr();
        System.out.println("获取客户机的IP地址:"+remoteAddr);
        String queryString = req.getQueryString();
        System.out.println("获取get方式的请求参数:"+queryString);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

<u>结果：</u>

```apl
请求方法:GET
虚拟目录:/TomcatProgram
Servlet路径:/request
获取请求的URI:/TomcatProgram/request
获取请求的URL:http://localhost:8080/TomcatProgram/request
获取版本协议及其信息:HTTP/1.1
获取客户机的IP地址:0:0:0:0:0:0:0:1
获取get方式的请求参数:null
```

