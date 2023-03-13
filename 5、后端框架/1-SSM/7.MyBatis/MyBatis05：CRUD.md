### <!--以下内容只需要修改：映射文件UserMapper.xml、测试类MybatisTest-->

## 1、<span style="color:brown">查询数据操作</span>

在UserMapper.xml文件的根标签<mapper></mapper>中添加以下内容：

```xml
<select id="findAll" resultType="com.zgy.domain.User">
    select * from user
</select>
```

在MybatisTest类中，加入测试方法：

```java
@Test
public void test01() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = build.openSession();
    List<Object> objects = sqlSession.selectList("userMapper.findAll");
    System.out.println(objects);
    sqlSession.close();
}
```



## 2、<span style="color:brown">新增数据操作</span>

在UserMapper.xml文件的根标签<mapper></mapper>中添加以下内容：

```xml
<insert id="save" parameterType="com.zgy.domain.User">
    insert into user values(#{id}, #{username}, #{password})
</insert>
```

在MybatisTest类中，加入测试方法：

```java
@Test
public void test02() throws IOException {
    // 模拟user对象
    User user = new User();
    user.setName("Tom");
    user.setPwd("abc");
    
    InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = build.openSession();
    
    int insert = sqlSession.insert("userMapper.save", user);
    // Mybatis执行更新操作  提交事务
    sqlSession.commit();
    
    sqlSession.close();
}
```



## 3、<span style="color:brown">修改数据操作</span>

在UserMapper.xml文件的根标签<mapper></mapper>中添加以下内容：

```xml
<update id="update" parameterType="com.zgy.domain.User">
    update user set username = #{username}, password = #{password} where id = #{id}
</update>
```

在MybatisTest类中，加入测试方法：

```java
@Test
public void test03() throws IOException {
    // 模拟user对象
    User user = new User();
    user.setId(6);
    user.setUsername("Lucy");
    user.setPassword("123456");

    InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = build.openSession();
    
    int update = sqlSession.update("userMapper.update", user);
    // 提交事务
    sqlSession.commit();

    sqlSession.close();
}
```



## 4、<span style="color:brown">删除数据操作</span>

在UserMapper.xml文件的根标签<mapper></mapper>中添加以下内容：

```xml
<delete id="delete" parameterType="java.lang.Integer">
    delete from user where id = #{id}
</delete>
```

在MybatisTest类中，加入测试方法：

```java
@Test
public void test04() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
    // 获取sqlSession工厂对象
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
    // 获取sqlSession对象
    SqlSession sqlSession = build.openSession();
    // 执行sql语句  参数: namespace + id
    sqlSession.delete("userMapper.delete",6);
    // 提交事务
    sqlSession.commit();
    // 释放资源
    sqlSession.close();
}
```



## 5、<span style="color:brown">注意事项</span>

**5.1、程序正常运行，但获取的结果是null值？**

```apl
实体类的属性名称, 最好跟创建的数据库表中的字段名称一致
```

**5.2、关于对数据进行了新增，但最后在表中没有数据，而查询结果显示存在该数据的问题？**

```apl
Mybatis跟JDBC不一样, 'Mybatis进行数据修改, 需要手动提交事务'！！！！
```

**5.3、插入操作的注意事项：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C%E7%9A%84%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.png" alt="image-20220925001117046"  />

**5.4、修改操作的注意事项：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E4%BF%AE%E6%94%B9%E6%93%8D%E4%BD%9C%E7%9A%84%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.png" alt="image-20220925002202201"  />

**5.5、删除操作的注意事项：**

![image-20220925003259269](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E5%88%A0%E9%99%A4%E6%93%8D%E4%BD%9C%E7%9A%84%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.png)



## 6、<span style="color:brown">开启运行日志</span>

### <!--基于SSM项目, 配置Mybatis运行日志-->

**6.1、导入log4J依赖：**

```xml
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.12</version>
</dependency>
```

**6.2、配置文件编写：**

在resources中新建一个`log4j.properties`文件，内容如下：

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
# 日志输出的文件
log4j.appender.file.File=./log/mybatis.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

**6.3、setting设置日志实现：**

在mybatis-config.xml或者SqlMapconfig.xml中，配置：

```xml
<!--配置mybatis运行日志(一)-->
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```
