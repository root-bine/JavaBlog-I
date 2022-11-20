## 1、<span style="color:brown">忽略文件操作：</span>

**1.1、应用场景：**

```apl
1. 在项目目录下有很多万年不变的文件目录, 例如css、js、images 等;
2. 还有一些目录即便有改动, 我们也不想让其提交到远程仓库的文档;
------------------------------------------------------------------------------------------------------------
# 此时我们可以使用“忽略文件”机制来实现需求
```

**1.2、具体使用：**

<u>忽略文件需要新建一个名为`.gitignore的文件`</u>：

```apl
# 该文件用于声明忽略文件/不忽略文件的规则;
# 规则对当前目录及其子目录生效;
```

<span style="color:red">**注意：**</span>***该文件因为没有文件名，没办法直接在 windows目录下直接创建***：<u>`可以通过命令行Git Bash来 touch创建`</u>

**1.3、常用命令行：**

```apl
/mtk/						# 过滤整个文件夹 [mtk代表文件夹名称] 
------------------------------------------------------------------------------------------------------------
*.zip						# 过滤所有.zip文件
------------------------------------------------------------------------------------------------------------
/mtk/do.c					# 过滤某个具体文件 [do.c代表文件的全名称]
!index.php					# 不过滤某个具体的文件 [文件名称前面加上!]
```

**1.4、案例模拟：**

1、在本地仓库中创建一个js文件夹，以及其目录下的js文件

2、依次提交本地与线上

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%BF%BD%E7%95%A5%E6%96%87%E4%BB%B601.png" alt="image-20220708230005708" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%BF%BD%E7%95%A5%E6%96%87%E4%BB%B602.png" alt="image-20220708225941676" style="zoom: 50%;" />

3、新增`.gitignore文件`

![image-20220708230446508](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%BF%BD%E7%95%A5%E6%96%87%E4%BB%B603.png)

4、编写**.gitignore文件**中的过滤规则

![image-20220708230853615](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%BF%BD%E7%95%A5%E6%96%87%E4%BB%B604.png)

5、编写一个index.js文件，并再次提交，查看线上仓库是否存在该文件

![image-20220708231516874](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%BF%BD%E7%95%A5%E6%96%87%E4%BB%B605.png)

## 2、<span style="color:brown">标签的使用：</span>

**2.1、基础命令：**

```apl
git tag										# 列出已有标签
------------------------------------------------------------------------------------------------------------
git tag [name]								# 创建标签
```

**2.2、标签推送：**

```apl
git push [shortname] [name]				# 将标签推送至远端仓库
```

**2.3、检出标签：**

<span style="color:red">**检出标签时需要新建一个分支来指向某个标签**</span>！！！

```apl
git checkout -b [branch] [name] 		# 检出标签【显示标签的状态】
```

![image-20221026184817236](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%A0%87%E7%AD%BE%E6%A3%80%E5%87%BA.png)

---

![image-20221026184943174](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%A0%87%E7%AD%BE%E6%A3%80%E5%87%BA%E7%BB%93%E6%9E%9C.png)
