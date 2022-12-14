# HTTP进阶

## 1、<span style="color:brown">HTTP常用方法：</span>

**1.1、方法分析：**

![image-20220613143641104](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/HTTP%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95.png)

**1.2、get和post的区别：**

1. URL可见性：

   ```apl
   get，参数url可见；
   
   post，url参数不可见
   ```

2. 数据传输上：

   ```apl
   get，通过拼接url进行传递参数
   
   post，通过body体传输参数
   ```

3. 缓存性：

   ```apl
   get请求是可以缓存的
   
   post请求不可以缓存
   ```

4. 后退页面的反应：

   ```apl
   get请求页面后退时，不产生影响
   
   post请求页面后退时，会重新提交请求
   ```

5. 传输数据大小：

   ```apl
   get一般传输数据大小不超过2k-4k（根据浏览器不同，限制不一样，但相差不大）
   
   post请求传输数据的大小根据php.ini配置文件设定，可以无限大
   ```

6. 数据包：

   ```apl
   GET产生一个TCP数据包
   
   POST产生两个TCP数据包
   ```

   

## 2、<span style="color:brown">HTTP状态码：</span>

**HTTP 状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型**。

响应分为五类：

- 信息响应(100–199)；
- 成功响应(200–299)；
- 重定向(300–399)；
- 客户端错误(400–499)；
- 服务器错误 (500–599)；

![image-20220613144253204](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/HTTP%E7%8A%B6%E6%80%81%E7%A0%81.png)

## 3、<span style="color:brown">HTTPS工作原理：</span>

1. HTTP请求**服务端生成证书**，客户端对证书的有效期、合法性、域名是否与请求的域名、证书的公钥（RSA加密）等进行校验；
2. 客户端如果校验通过后，就**根据证书的公钥生成随机数**，随机数使用公钥进行加密（RSA加密）；
3. 消息体产生的后，对它的摘要进行MD5（或者SHA1）算法加密，此时就得到了RSA签名；
4. 发送给服务端，此时只有服务端（RSA私钥）能解密；
5. 解密得到的随机数，再用AES加密，作为密钥（此时的密钥只有客户端和服务端知道）；

