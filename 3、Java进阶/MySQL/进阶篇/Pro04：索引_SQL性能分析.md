## 1、<span style="color:brown">SQL执行频率：</span>

​		MySQL客户端连接成功后，通过操作命令可以提供<u>**服务器状态信息**</u>，即：查看当前数据库的insert、update、delete、select的访问频次，命令如下：

```sql
# session表示当前会话状态信息, global表示全局状态信息
# 下划线总共七个
show [global|session] status like'Com_______';
```

执行结果如下：MySQL_Pro

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro13.png" alt="image-20230614181156457" style="zoom:67%;" />

