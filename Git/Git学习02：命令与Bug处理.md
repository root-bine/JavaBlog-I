# Git学习02

## 1、<span style="color:brown">Git常用命令：</span>

**1.1、文件夹/文件操作：**

```apl
mkdir [filename]				# 创建新的空目录
------------------------------------------------------------------------------------------------------------
touch 文件名称.后缀	        # 创建文件
------------------------------------------------------------------------------------------------------------
cd    [filename]				# 进入目录
------------------------------------------------------------------------------------------------------------
Ctrl + L					# 清屏
```

**1.2、Git本地仓库文件上传/拉取操作：**

```apl
git status                                                # 查看当前版本状态（是否修改）
------------------------------------------------------------------------------------------------------------
git add [filename]                                          # 添加一个文件至缓存区
git add .                                                 # 增加当前子目录下所有更改过的文件至缓存区
------------------------------------------------------------------------------------------------------------
git commit [filename]										  # 提交文件至版本库
git commit -m 'xxx' [filename]                             # 提交文件至版本库, 同时提交描述
```

**1.3、版本回退：**

```apl
1. 将暂存区的文件暂时'取消暂存'/切换到指定版本
	git reset [filename]
2. 查询提交记录
	git log   或者   git log --pretty=oneline
---------------------------------------------------------------------------------------------------------
3. 回退操作
	git reset --hard 提交编号
```

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Git%E6%9F%A5%E8%AF%A2%E6%8F%90%E4%BA%A4%E8%AE%B0%E5%BD%95.png" alt="image-20220708171333036" style="zoom:80%;" />

## 2、<span style="color:brown">Git报错解决：</span>

**2.1、无法提交文件到远程仓库：**

git push时报了这个错：fatal: unable to access 'https://github.com/.......': OpenSSL SSL_read: Connection was reset, errno 10054

![image-20221026165750319](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%97%A0%E6%B3%95%E6%8F%90%E4%BA%A4%E6%96%87%E4%BB%B6%E5%88%B0remote.png)

**产生原因：**<u>*一般是这是因为服务器的SSL证书没有经过第三方机构的签署，所以才报错*</u>

参考网上解决办法：解除ssl验证后，再次git即可

```apl
git config --global http.sslVerify "false"
```

**2.2、本地仓库未关联远程仓库时，拉取数据：**

![image-20221026171916561](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%9C%AA%E5%85%B3%E8%81%94remote%E6%8B%89%E5%8F%96%E6%95%B0%E6%8D%AE.png)
