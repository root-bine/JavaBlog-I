## 1、<span style="color:brown">模块聚合：</span>

**1.1、多模块构建维护：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module06.png" alt="image-20221023151655430" style="zoom: 50%;" />

上述存在如下问题：<u>*如果其中任意模块发生改变，重新install一次，其他模块并不知道这一信息*</u>！！！

因此需要采取模块管理方式：<span style="color:green">**聚合——用于快速构建Maven工程，一次性构建多个项目/模块**</span>。

**1.2、如何进行模块聚合？**

创建一个新的空白Maven项目，内容仅保留pom.xml文件即可。

<u>*pom.xml文件的操作：*</u>

1. 定义该工程用于构建管理：

   ```xml
   <packaging>pom</packaging>
   ```

2. 管理的工程列表：

   ####   <!--参与聚合操作的模块最终执行顺序与模块间的依赖关系有关，与配置顺序无关-->

   ```xml
   <modules>
       <!--具体的工程名称-->
   	<module>../ssm_pojo</module>
       <module>../ssm_dao</module>
       <module>../ssm_service</module>
       <module>../ssm_controller</module>
   </modules>
   ```

**1.3、编译结果分析：**

点击聚合工程项目的生命周期——compiles，内容如下：

![image-20221023153302505](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Moudle07.png)

---

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module08.png" alt="image-20221023153339618"  />

**1.4、packaging标签：**

maven作为一种XML标记语言，标签通常成对存在，目前packaging标签有3种配置：

```xml
<packaging>pom</packaging>
<packaging>jar</packaging>
<packaging>war</packaging>
```

- <packaging>pom</packaging>

  - 在父级项目中的pom.xml文件使用的packaging配置一定为pom；

  - ***父级的pom文件只作项目的子模块的整合【聚合】，在maven install时不会生成jar/war压缩包***；

- <packaging>jar</packaging>

  - jar包是最为常见的打包方式，当pom文件中没有设置packaging参数时，默认使用jar方式打包；
  - 在**maven build时会将项目中的所有java文件都译形成.class文件**，且按*照原来的java文件层级结构放置*，<u>最终压缩成jar文件</u>；

- <packaging>war</packaging>

  - war包与jar包非常相似，同样是编译后的.class文件按层级结构形成文件树后打包形成的压缩包。
  - 不同的是：
    - WEB-INF/lib：放置项目中依赖的所有jar包
    - WEB-INF/classes：放置代码的编译后形成的内容



## 2、<span style="color:brown">模块继承：</span>

**2.1、模块依赖关系维护：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module10.png" alt="image-20221023162355109" style="zoom:50%;" />

**2.2、模块继承操作：**

在聚合工程中的pom.xml文件上，进行如下操作：

1. 声明依赖管理：<span style="color:orange">dependencyManagement标签</span>

   ```xml
   <dependencyManagement>
   	<!--具体的依赖内容:GAV-->
   </dependencyManagement>
   ```

2. 声明插件管理：<span style="color:orange">pluginManagement标签</span>

   ```xml
   <build>
       <pluginManagement>
   		<plugins>
       		<plugin>...</plugin>
       	</plugins>
       </pluginManagement>
   </build>
   ```

3. 在分散模块中**添加父工程**：ssm_pojo、ssm_dao、ssm_service、ssm_controller

   ```xml
   <parent>
       <!--域名-->
   	<groupId>com.zgy</groupId>
       <!--项目名/模块名-->
       <artifactId>ssm</artifactId>
       <!--版本号-->
       <version>1.0-SNAPSHOT</version>
       <!--查找该父项目pom.xml的(相对)路径-->
       <relativePath>../ssm/pom.xml</relativePath>
   </parent>
   ```

   由于子工程与父工程**原则上属于同一组织ID** ，同时**子工程的版本应该与父工程保持一致**！！！

   因此，子工程可以省去：`<groupId></groupId>`、`<version></version>`的配置。

   另外，由于已经继承了父工程，那么跟父工程依赖相同的内容，可以省去依赖的版本号。

**2.3、聚合与继承的区别：**

![image-20221023162449793](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module09.png)

## 3、<span style="color:brown">属性：</span>

**3.1、属性类别：**

![image-20221023164755766](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module11.png)

**3.2、各类属性详解：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module12.png" alt="image-20221023164926133" style="zoom: 67%;" />

---

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module13.png" alt="image-20221023164948725" style="zoom:80%;" />

---

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module14.png" alt="image-20221023165010647" style="zoom:80%;" />

---

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module15.png" alt="image-20221023165027838" style="zoom:80%;" />

---

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Module16.png" alt="image-20221023165208423" style="zoom:80%;" />
