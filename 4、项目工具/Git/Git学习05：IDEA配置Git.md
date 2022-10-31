# Git学习05

## 1、<span style="color:brown">在IDEA中使用Git：</span>

**1.1、IDEA配置Git：**

在File中选择Settings，搜索Git，界面如下：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%20%E9%85%8D%E7%BD%AEGit.png" alt="image-20221026203533509" style="zoom: 67%;" />

**1.2、创建本地仓库：**

在IDEA的上菜单栏中找到VCS，找到其中的**Create Git Repository**，稍做配置，界面如下：

![image-20221026205649890](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E5%88%9B%E5%BB%BA%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93.png)

<u>***在完成上述操作之后，原本项目目录中，会增加一个`.git`的文件！！！***</u>

**1.3、从远程仓库克隆：**

点击IDEA上菜单栏的VCS，找到`Clone`，就可以进入如下页面：

![image-20221026210251888](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E8%8E%B7%E5%8F%96%E8%BF%9C%E7%AB%AF%E4%BB%93%E5%BA%93.png)



## 2、<span style="color:brown">在IDEA的本地仓库操作：</span>

**2.1、将文件加入暂存区：**

- 创建新文件之后，会自动弹出Add提示框，直接Add即可；
- 后续如果想将文件加入暂存区，选中项目，右键可以找到Git选项，点击其中的Add即可；

**2.2、将暂存区文件提交到版本库：**

- 提交**单个/多个**文件：
  - 只需要选中一个项目，右键可以找到Git选项，找到Commit files选项，其中可以执行单个或多个提交；
- 提交整个项目：
  - 项目右键找到Git，选择Commit Directory；

<span style="color:green"><u>快捷方式：</u>在IDEA上菜单栏有commit提交快捷键</span>！！！

**2.3、查看日志：**

![image-20221026212412833](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93%E6%9F%A5%E7%9C%8BLog.png)



## 3、<span style="color:brown">在IDEA的远程仓库操作：</span>

**3.1、查看远程仓库、添加远程仓库：**

选中需要查看的项目，右键找到Git选项，其中有`Manage Remotes...`，打开内容如下：

![image-20221026213006841](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E8%BF%9C%E7%AB%AF%E4%BB%93%E5%BA%93%E6%93%8D%E4%BD%9C.png)

如果需要新增远程仓库管理，`点击+`；而如果想接触本地仓库与远程仓库的关联关系，`点击-`。

**3.2、推送至远程仓库：**

找到需要上传的项目，右键选择Git，点击push。【或者在IDEA上菜单栏找到push快捷键】

<span style="color:blue">推送被拒解决方案：</span>

```apl
在idea内ALT+F12打开命令面板，输入下面三段:
 
git pull
 
git pull origin master
 
git pull origin master --allow-unrelated-histories
```



**3.3、从远程仓库拉取数据：**

在IDEA拉取数据，项目右键选择Git，点击pull。【或者在IDEA上菜单栏找到Update Project快捷键】

## 4、<span style="color:brown">分支操作：</span>

**4.1、查看/创建 分支：**

选择项目，右键选择Git，找到`Branches...`，界面如下：

![image-20221026214840150](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E5%88%86%E6%94%AF%E6%93%8D%E4%BD%9C01.png)

或者在IDEA的右下角也可以设置**分支查看/创建**！！！

**4.2、分支切换：**

在创建分支时，可以直接在本界面直接替换分支：

![image-20221026215442969](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E5%88%86%E6%94%AF%E6%93%8D%E4%BD%9C02.png)

如果想切换回某分支，可以在**查看分支界面**，点击需要切换到的分支，选择`checkout`即可

**4.3、推送分支到远程仓库：**

在查看分支界面，点击需要推送的分支，可以找到`push选项`，执行选项即可：

![image-20221026215801684](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E5%88%86%E6%94%AF%E6%93%8D%E4%BD%9C03.png)

**4.4、合并分支：**

在查看分支界面，**点击需要合并的分支**，找到`Merge...`：

![image-20221026220056071](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/IDEA%E5%88%86%E6%94%AF%E6%93%8D%E4%BD%9C04.png)