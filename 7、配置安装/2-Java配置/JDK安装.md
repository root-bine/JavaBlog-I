## 1、<span style="color:brown">前置准备：</span>

下载`JDK1.8`版本/`JDK11`的启动程序：

- https://www.oracle.com/java/technologies/downloads/?er=221886

J提前在本地创建一个`JDK`文件夹：

- `JDK1.8`：里面分别创建一个`jdk`和`jre`文件夹；
- `JDK11`：直接存放安装内容即可；



## 2、<span style="color:brown">环境配置：</span>

在系统变量中添加`JAVA_HOME`，内容为

- `JDK1.8`：
  - `JAVA_HOME`：`D:\Java\JDK\JDK8\jdk1.8`；
  - `CLASSPATH`：`.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar`；
- `JDK11`：`D:\Java\JDK\JDK11`；

之后在`Path`中添加如下内容：`%JAVA_HOME%\bin`。