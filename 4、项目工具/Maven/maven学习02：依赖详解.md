# Maven学习02

## 1、<span style="color:brown">依赖管理&依赖传递：</span>

**1.1、依赖配置格式：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E4%BE%9D%E8%B5%96%E9%85%8D%E7%BD%AE.png" alt="image-20220706212926346" style="zoom:67%;" />

**1.2、依赖传递：**

在需要导入依赖的资源的<dependency></dependency>中，**配置导入资源的坐标**

<u>**依赖具有传递性**</u>：

- 直接依赖：在当前项目中，通过依赖配置建立的依赖关系
- 间接依赖：被引用的资源如果依赖其他资源，当前项目间接依赖其他资源

**1.3、解决依赖传递冲突问题：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E4%BE%9D%E8%B5%96%E5%86%B2%E7%AA%81%E9%97%AE%E9%A2%98.png" />

**1.4、可选依赖：**

```apl
对外隐藏当前所依赖的资源 ----- 不透明
```

![image-20220706214426716](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8F%AF%E9%80%89%E4%BE%9D%E8%B5%96.png)

**1.5、排除依赖：**

```apl
主动断开依赖的资源, 被排除的资源无需指定版本  -----  不需要
```



![image-20220706214629979](C:\Users\root-bine\AppData\Roaming\Typora\typora-user-images\image-20220706214629979.png)



## 2、<span style="color:brown">依赖范围：</span>

**2.1、概述：**

```apl
依赖的jar默认情况下, 可以在任何地方使用, 也可以使用<scope></scope>标签设定范围
```

**2.2、作用范围：**

- **主程序范围**有效【main文件范围】
- **测试程序范围**有效【test文件范围】
- 是否参与打包【package指令范围】

**2.3、scope标签属性：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E4%BE%9D%E8%B5%96%E8%8C%83%E5%9B%B4.png" alt="image-20220706215418931" style="zoom: 80%;" />



## 3、<span style="color:brown">生命周期与插件：</span>

**3.1、生命周期划分：**

- `clean`：清理工作
- `default`：核心工作，如：编译、测试、打包、部署等
- `site`：产生报告、发布站点等

**3.2、生命周期详解：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/clean%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png" alt="image-20220706221211288" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/default%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png" alt="image-20220706221341939" style="zoom:80%;" />

**3.3、插件：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Maven%E6%8F%92%E4%BB%B6.png" alt="image-20220706222249466" style="zoom:80%;" />