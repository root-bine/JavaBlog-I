## <span style="color:green">ssh是本地项目上传git云，http是git云下载项目到本地</span>

## 1、<span style="color:brown">基础内容：</span>

**1.1、概述：**

Git是目前最先进的<u>**分布式版本控制系统**</u>

**1.2、Git的三个区域：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Git%E5%88%86%E5%8C%BA.png" alt="image-20220708155644794" style="zoom:80%;" />

**1.3、Git工作流程演示：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Git%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B.png" alt="image-20220708155920087" style="zoom:80%;" />

**1.4、Git工作区文件状态：**

![image-20221026152537127](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Git%E5%B7%A5%E4%BD%9C%E5%8C%BA%E6%96%87%E4%BB%B6%E7%8A%B6%E6%80%81.png)



## 2、<span style="color:brown">本地仓库操作：</span>

**2.1、仓库的概述：**

```apl
# 什么是仓库呢?
------------------------------------------------------------------------------------------------------------
1. 仓库又名版本库, 英文名repository;
2. 我们可以简单理解成是一个目录, 用于存放代码的;
3. 这个目录里面的所有文件都可以被Git管理起来, 每个文件的修改、删除等操作Git都能跟踪到;
```

**2.2、操作：**

1. <u>**建立连接：**</u>

   如果是首次操作Git，就需要与GitHub建立连接，进行全局配置，命令如下：

   ```apl
   git config --global user.name "xxx"                       # 配置用户名
   git config --global user.email "xxx@xxx.com"              # 配置邮件
   git config --list										  # 查看配置信息
   ```

2. <u>**初始化本地仓库：**</u>

   在任意目录下，鼠标右键打开Git bath：

   <!--执行完初始化仓库命令后, 会在创建好的空仓库中隐藏一个.git文件, 可以通过文件中的查看将其排查-->

   ```apl
   git init                  # 初始化本地git仓库
   ```



## 3、<span style="color:brown">远程仓库操作：</span>基于http协议

需要在GItHub上面创建一个新的空repository，然后进行如下操作：

1. 基于http协议
2. 基于ssh协议

**3.1、克隆线上仓库到本地仓库：**

- 创建一个空目录，并进入该目录

  ![image-20220708174620383](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%BC%94%E7%A4%BA1.png)

- 使用clone指令，将远程仓库克隆到本地：

  ```apl
  git clone 线上本地仓库地址
  ```

  线上本地仓库地址：

  ![image-20220708174833381](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E7%BA%BF%E4%B8%8A%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93%E5%9C%B0%E5%9D%80.png)

**3.2、远程仓库操作：**

查看本地仓库是否与远程仓库关联：

```apl
git remote
```

显示远程仓库详细信息：

```apl
git remote -v
```

添加远程仓库：shortname为远程仓库的简称，而url是远程仓库的地址

```apl
git remote add [shortname] [url]
```

**3.3、上传文件到线上仓库&拉取线上数据：**

<!--提交暂存区、提交本地仓库、提交线上仓库、拉取线上仓库-->

**<u>提交文件到线上仓库的指令：</u>**

```apl
# 根据远程仓库名称【git remote】和分支名称
git push [shortname] [branch]
------------------------------------------------------------------------------------------------------------
# 无需任何设置
git push
```

**别人在线上仓库创建了一个新文件**，<u>如果需要在本地查看，需要从线上仓库拉取数据</u>：

```apl
# 根据远程仓库名称【git remote】和分支名称
git pull [shortname] [branch]
------------------------------------------------------------------------------------------------------------
git pull
```

**3.3、演示：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E7%BA%BF%E4%B8%8A%E4%BB%93%E5%BA%93%E6%93%8D%E4%BD%9C01.png" alt="image-20220708182925773" style="zoom:80%;" />



## 4、<span style="color:brown">远程仓库操作：</span>基于ssh协议

**4.1、ssh概述：**

![image-20220708183906527](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/ssh%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.png)

**4.2、使用步骤：**

1. 使用指令生产ssh私钥文件：

   ```apl
   ssh-keygen -t rsa -C "你的邮箱地址"
   ```

2. 生成如下界面，根据界面显示的私钥文件：`id_rsa.pub`

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/ssh%E7%94%9F%E6%88%90%E7%A7%81%E9%92%A5.png" alt="image-20220708184343420" style="zoom:80%;" />

3. 在Github上面的settings中，找到【SSH  and  GPG keys】,上传【id_rsa.pub】内容，最终界面如下：

   <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSH%E5%85%AC%E9%92%A5.png" alt="image-20220708185137495" style="zoom:67%;" />

4. <span style="color:orange">执行后续git操作，操作方式与**基于http协议**相似</span>

**4.3、演示：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E7%BA%BF%E4%B8%8A%E4%BB%93%E5%BA%93%E6%93%8D%E4%BD%9C02.png" alt="image-20220708191114019" style="zoom: 80%;" />