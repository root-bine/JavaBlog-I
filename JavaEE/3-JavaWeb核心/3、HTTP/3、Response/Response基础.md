# Response基础

## 1、<span style="color:brown">设置响应行：</span>

**1.1、格式：**

```apl
HTTP/1.1 200 ok
```

**1.2、核心方法：**

- 设置状态码：

  ```java
  void setStatus(int sc)
  ```

  

## 2、<span style="color:brown">设置响应头：</span>

```java
void setHeader(String name, String value)
```



## 3、<span style="color:brown">设置响应体：</span>

<u>**使用步骤**</u>

1. 获取输出流：

   ```java
   PrintWriter getWriter()
   ```

   ```java
   ServletOutputStream getOutputStream()
   ```

2. 使用输出流，将数据从服务器转移到浏览器；