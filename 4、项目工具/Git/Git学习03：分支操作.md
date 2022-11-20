## 1、<span style="color:brown">分支管理：</span>

## <span style="color:violet">所有的分支，在项目开发过程中会逐渐合并到main分支中</span>！！！

**1.1、基础命令：**

```apl
git branch					# 查看本地仓库分支
------------------------------------------------------------------------------------------------------------
git branch -r				# 查看远程仓库分支
------------------------------------------------------------------------------------------------------------
git branch -a				# 列出所有的本地仓库和远程仓库的分支
```

**1.2、分支操作：**

```apl
git branch   分支名		  			# 创建分支
git checkout 分支名		  			# 切换分支
git branch -d 分支名		  			# 删除分支
------------------------------------------------------------------------------------------------------------
git push remote-name branch-name  	  # 推送分支到远端仓库
------------------------------------------------------------------------------------------------------------
git merge 被合并的分支名	   		   	  # 合并分支
git merge -m'xxx' 被合并的分支名		  [避免进入vim编辑模式]
```

**1.3、注意事项：**

1. 在删除分支之前，**必须要切换出当前的分支，否则会出现删除不成功的情况**；
2. 在合并分支之后，<u>*一定要把合并后的文件上传到线上仓库中*</u>；



## 2、<span style="color:brown">冲突的产生与解决：</span>

**2.1、模拟冲突案例：**

<u>1、同事A下班后在线上仓库进行了部分内容的修改</u>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%86%B2%E7%AA%81%E6%A1%88%E4%BE%8B01.png" alt="image-20220708221147336" style="zoom:67%;" />

<u>注意此时我本地与线上内容不一致：</u>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%86%B2%E7%AA%81%E6%A1%88%E4%BE%8B02.png" alt="image-20220708221334415" style="zoom:67%;" />

<u>2、第二天上班，我没有进行`git pull操作`，而是直接在本地仓库的该文件中进行修改：</u>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%86%B2%E7%AA%81%E6%A1%88%E4%BE%8B03.png" alt="image-20220708221645845" style="zoom:67%;" />

<u>3、下班时，需要将修改了内容的文件，上传到线上仓库中：</u>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%86%B2%E7%AA%81%E6%A1%88%E4%BE%8B04.png" alt="image-20220708222200564" style="zoom:67%;" />

`提示我们在git push之前，需要进行git pull`

**2.2、解决问题：**

1、使用指令：`git pull`

![image-20220708223102561](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%86%B2%E7%AA%81%E8%A7%A3%E5%86%B301.png)

2、打开冲突文件，与同事进行协商，对文件进行进一步修改和优化：

![image-20220708223217356](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%86%B2%E7%AA%81%E8%A7%A3%E5%86%B302.png)

3、完成上述操作之后，重新进行提交；
