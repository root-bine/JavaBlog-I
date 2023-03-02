```scss
1. 多表之间的关系有: 一对一, 一对多, 多对一, 多对多，每一种都有建表的原则,以'用户-订单模型'为例
2. 利用传统的方法进行多表查询无非是通过id来连接表然后封装返回结果，MyBatis中也是如此
		# 在Mapper文件中写好表字段之间的映射关系, 定义好类型即可
		# 只不过这一过程有点复杂, 但一次配好之后即可极大减少硬编码问题, 提高效率
```

## 1、<span style="color:brown">一对一：</span>

**1.1、模型范例：**

一个用户有一张订单

![image-20221002230212058](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E4%B9%8B%E4%B8%80%E5%AF%B9%E4%B8%80.png)

首先建好实体类，写好接口方法，配置Mapper文件，而多表操作的麻烦点就在于配置文件，这里通过例子细说一下

**1.2、建表：**<span style="color:blue">**根据数据库表的信息，建立相应的实体类**</span>

```sql
CREATE TABLE orders (
id INT PRIMARY KEY ,
ordertime VARCHAR(20) NOT NULL DEFAULT '',
total DOUBLE,
uid INT);

INSERT INTO orders VALUES(1,2020,2000,1);
INSERT INTO orders VALUES(2,2021,3000,2);
INSERT INTO orders VALUES(3,2022,4000,3);

CREATE TABLE USER (
id INT PRIMARY KEY ,
username VARCHAR(50) NOT NULL DEFAULT '',
passwords VARCHAR(50) NOT NULL DEFAULT '');

INSERT INTO USER VALUES(1,'lyy',333);
INSERT INTO USER VALUES(2,'myy',444);
INSERT INTO USER VALUES(3,'xyy',555);
```

**1.3、编写OrdersMapper.xml：**

在写实体类时，**要把一个实体写到另一个实体的属性里面，这样才体现关联性**，<u>就比如`订单是用户拥有的`</u>。

正因为这种关系我们才会在**订单实体类Orders里面写上`private User user;`这一属性**，这样根据id连接的两个实体才能完美对接！

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E5%A4%9A%E8%A1%A8%E6%93%8D%E4%BD%9C01.png" alt="image-20221002230719776" style="zoom:80%;" />

**通过`<association>`把两张表对应的实体类连接起来**，但是<u>主键ID要用单独的标签</u>

- `property`: 当前实体(order)中的属性名称(private User user)
- `javaType`: 当前实体(order)中的属性的类型(User)，此处在SqlMapConfig文件中进行了别名配置

然后就是写一对一的SQL：<span style="color:red">需要在OrderMapper接口中创建`List<Order> findAll();`</span>

```xml
<select id="findAll" resultMap="orderMap">
   SELECT *,o.id oid FROM orders o,USER u WHERE o.uid=u.id
</select>
```

SQL环节和原来没什么区别，同样也是通过`resultMap`把字段和属性映射封装：

```xml
<resultMap id="orderMap" type="order">
        <!--手动指定字段与实体属性的映射关系
            column: 数据表的字段名称
            property：实体的属性名称
        -->
        <id column="oid" property="id"></id>
        <result column="ordertime" property="ordertime"></result>
        <result column="total" property="total"></result>
    
    	<!--方式一-->
        <!--<id column="uid" property="user.id"></id>
            <result column="username" property="user.username"></result>
            <result column="password" property="user.password"></result>
            <result column="birthday" property="user.birthday"></result>
		-->
    	<!--方式二-->
        <association property="user" javaType="user">
            <!--此处的uid是根据数据库中的显示出的字段名称-->
            <id column="uid" property="id"></id>
            <result column="username" property="username"></result>
            <result column="password" property="password"></result>
            <result column="birthday" property="birthday"></result>
        </association>
</resultMap>
```



## 2、<span style="color:brown">一对多：</span>

**2.1、模型范例：**

一个用户有多张订单

![image-20221002231411391](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E5%A4%9A%E8%A1%A8%E6%93%8D%E4%BD%9C02.png)

---

**2.2、编写UserMapper.xml：**

首先，在**原有的User实体中得加上一个表示“用户有哪些订单的属性**”`private List<Order> orderList;`，目的是为了把订单的信息封装到用户的这个属性里。

在UserMapper.xml文件中体现：

```xml
<resultMap id="userMap" type="user">
    <id column="uid" property="id"></id>
    <result column="username" property="username"></result>
    <result column="password" property="password"></result>
    
    <!--封装order的数据-->
    <collection property="orderList" ofType="order">
        <id column="oid" property="id"></id>
        <result column="ordertime" property="ordertime"></result>
        <result column="total" property="total"></result>
    </collection>
</resultMap>
```

- `property`:集合名称，User实体中的orderlist属性
- `ofType`：当前集合中的数据类型，就是order实体

然后就是写一对多的SQL：<span style="color:red">需要在UserMapper接口中创建`List<User> findAll();`</span>

```xml
<select id="findAll" resultMap="userMap">
    SELECT *,o.id oid FROM USER u,orders o WHERE u.id=o.uid
</select>
```

<u>**总结来看，一对多相比于一对一就是在那个“一”中增添了封装“多”的属性而已，然后稍微调整一下SQL**</u>



## 3、<span style="color:brown">多对多：</span>

**3.1、模型范例：**<span style="color:green">**这里需要创建一个user-role和role表，以及创建实体类Role**</span>

多用户多角色

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E5%A4%9A%E8%A1%A8%E6%93%8D%E4%BD%9C03.png" alt="image-20221002233424555" style="zoom:80%;" />

**多对多的建表原则是引入一张中间表，用于维护外键**，就是<u>一张表通过中间表找到另一张表</u>.

---

**3.2、编写UserMapper.xml：**

与一对多的模型类似，**先在User实体类中增添一个“用户具备哪些角色”的属性**`private List<Role> roleList;`

其次配置UserMapper.xml：

```xml
<resultMap id="userRoleMap" type="user">
    <!--user的信息-->
    <id column="userId" property="id"></id>
    
    <result column="username" property="username"></result>
    <result column="password" property="password"></result>
    <result column="birthday" property="birthday"></result>
    
    <!--封装role的数据-->
    <!--user内部的roleList信息-->
    <collection property="roleList" ofType="role">
        <id column="roleId" property="id"></id>
        
        <result column="roleName" property="roleName"></result>
        <result column="roleDesc" property="roleDesc"></result>
    </collection>
</resultMap>
```

多表的连接是靠中间表，这点在Mapper文件中通过映射实现，具体是把两张外表的id（userId和roleId）在id标签中配置成同一个属性，就像这样：

```xml
<id column="userId" property="id"></id>
------------------------------------------------------------------------------------------------------------
<id column="roleId" property="id"></id>
```

然后就是写一对多的SQL：<span style="color:red">需要在UserMapper接口中创建`List<User> findUserAndRoleAll();`</span>

```xml
<select id="findUserAndRoleAll" resultMap="userRoleMap">
    SELECT * FROM USER u,user-role ur,role r WHERE u.id=ur.userId AND ur.roleId=r.id
</select>
```



## 4、<span style="color:brown">代码内容补充：</span>

- SqlMapConfig.xml

  ```xml
  <!--通过properties标签加载外部properties文件-->
  <properties resource="jdbc.properties"></properties>
  
  <!--自定义别名-->
  <typeAliases>
      <typeAlias type="com.zgy.domain.User" alias="user"></typeAlias>
      <typeAlias type="com.zgy.domain.Order" alias="order"></typeAlias>
      <typeAlias type="com.zgy.domain.Role" alias="role"></typeAlias>
  </typeAliases>
  
  <!--数据源环境-->
  <environments default="developement">
      <environment id="developement">
          <transactionManager type="JDBC"></transactionManager>
          <dataSource type="POOLED">
              <property name="driver" value="${jdbc.driver}"/>
              <property name="url" value="${jdbc.url}"/>
              <property name="username" value="${jdbc.username}"/>
              <property name="password" value="${jdbc.password}"/>
          </dataSource>
      </environment>
  </environments>
  ```

- jdbc.properties

  ```properties
  jdbc.driver=com.mysql.jdbc.Driver
  jdbc.url=jdbc:mysql://localhost:3306/test
  jdbc.username=root
  jdbc.password=root
  ```
