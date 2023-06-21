## 1、<span style="color:brown">SQL执行频率：</span>

​		MySQL客户端连接成功后，通过操作命令可以提供<u>**服务器状态信息**</u>，即：<span style="color:green">查看当前数据库的insert、update、delete、select的访问频次</span>，命令如下：

```sql
# session表示当前会话状态信息, global表示全局状态信息
# 下划线总共七个
show [global|session] status like'Com_______';
```

执行结果如下：MySQL_Pro

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro13.png" alt="image-20230614181156457" style="zoom:67%;" />



## 2、<span style="color:brown">慢查询日志：</span>

**2.1、概述：**

慢查询日志记录了<span style="color:blue">**所有执行时间超过指定参数**</span>(`long_query_time`，单位：秒，默认10秒）的所有SQL语句的日志。

**2.2、查看slow_query_log状态：**

<span style="color:red">MySQL的慢查询日志默认没有开启</span>，可执行命令查看：`show variables like 'slow_query_log';`。

**2.3、开启慢查询日志：**

在MySQL的配置文件（`/etc/my.cnf`〉中配置如下信息：

![image-20230620225204522](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro14.png)
