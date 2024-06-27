## 1、<span style="color:brown">前置准备：</span>

下载MySQL安装包，网址：https://dev.mysql.com/downloads/，选择：`MySQL Community Server`。

进入社区后，在`Select Operating System`，选择：`Microsoft Windows`后，找到**对应系统版本**即可。

下载好压缩包之后，在本地创建MySQL文件夹，将压缩文件内容解压到MySQL中。



## 2、<span style="color:brown">相关配置：</span>

**2.1、my.ini文件**

```mysql
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录   ----------是你的文件路径-------------
basedir=D:\Java\Database\mysql
# 设置mysql数据库的数据的存放目录  ---------是你的文件路径data文件夹自行创建
datadir=D:\Java\Database\mysql\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
# 服务端使用的字符集默认为utf8mb4
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
#mysql_native_password
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```

**2.2、初始化数据库**

切换到**`bin`目录**下，以**管理员身份**打开**`CMD`**，执行命令：

```ABAP
mysqld --initialize --console
```

此时界面会在**`root@localhost`**处显示一个**随机初始密码**，须事先记录下来。

**2.3、安装mysql服务并启动**

在**`bin`目录**下，以<u>**管理员身份**</u>进行，执行命令：

```ABAP
mysqld --install mysql
```

**2.4、连接MySQL服务**

在**`bin`目录**下，以<u>**管理员身份**</u>进行，执行命令：

```ABAP
net start mysql
```

然后进行登录操作，执行命令：

```ABAP
mysql -uroot -p
```

此时会显示输入密码，即：<u>之前的**随机初始密码**</u>。若密码校验成功，此时可以修改密码：

```ABAP
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```



## 3、<span style="color:brown">配置环境变量</span>

在系统环境变量中，新增一个变量：`MYSQL_HOME`，其值为：`D:\Java\Database\mysql`。

然后在系统变量里面找到path变量，添加`%MYSQL_HOME%\bin`。

可在**`CMD命令面板`**中，执行：`MySQL -V`，查看MySQL是否安装成功。

