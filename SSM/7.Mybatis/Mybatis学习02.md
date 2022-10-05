# Mybatis快速入门

## 1、<span style="color:brown">Mybatis开发步骤：</span>

![image-20220924204716858](C:\Users\root-bine\AppData\Roaming\Typora\typora-user-images\image-20220924204716858.png)



## 2、<span style="color:brown">功能实现：</span>

**2.1、查询操作的注意事项：**

1. Mybatis配置文件：

   ```apl
   1. 在'resources文件'下创建一个mapper包, 建立UserMapping.xml;
   		---> 在resources目录下创建包, 需要使用'/'分割;
   2. 直接在'resources文件'下创建SqlMapConfig.xml;
   ```

2. mapping文件约束头：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="">
       
   </mapper>
   ```

3. 关于mapper里面id爆红名的解决：

   ```apl
   右键file ------>点击setting------>找到点击editor
   ----->点击 Inspections ----->找到点击mybatis
   ----->点击取消勾选 Mapper xml inspection
   ```

4. config文件约束头：

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       
   </configuration>
   ```

**2.2、代码范例：**

- 添加mybatis坐标：

  ```xml
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.27</version>
  </dependency>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
  </dependency>
  ```

- 创建数据库：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A1%A8%E8%8C%83%E4%BE%8B.png" alt="image-20220924231644410" style="zoom:80%;" />

- 创建实体类：

  ```java
  public class User {
      private int id;
      private String name;
      private String pwd;
      public int getId() {
          return id;
      }
      public void setId(int id) {
          this.id = id;
      }
      public String getName() {
          return name;
      }
      public void setName(String name) {
          this.name = name;
      }
      public String getPwd() {
          return pwd;
      }
      public void setPwd(String pwd) {
          this.pwd = pwd;
      }
      @Override
      public String toString() {
          return "User{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  ", pwd='" + pwd + '\'' +
                  '}';
      }
  }
  ```

- 创建数据源配置信息文件jdbc.properties

  ```properties
  jdbc.driver = com.mysql.cj.jdbc.Driver
  jdbc.url = jdbc:mysql://localhost:3306/test
  jdbc.username = root
  jdbc.password = 123456
  ```

- 映射文件UserMapper.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="userMapper">
      <select id="findAll" resultType="user">
          select * from user
      </select>
  </mapper>
  ```

- 核心文件SqlMapConfig.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
      <!--通过properties标签加载jdbc.properties文件-->
      <properties resource="jdbc.properties"/>
      
      <!--别名处理器-->
      <typeAliases>
      	<typeAlias type="com.zgy.domain.User" alias="user"/>
  	</typeAliases>
      
      <!--数据源的环境-->
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"></transactionManager>
              <dataSource type="POOLED">
                  <property name="driver" value="${jdbc.driver}"/>
                  <property name="url" value="${jdbc.url}"/>
                  <property name="username" value="${jdbc.username}"/>
                  <property name="password" value="${jdbc.password}"/>
              </dataSource>
          </environment>
      </environments>
      
      <!--加载映射文件-->
      <mappers>
          <mapper resource="mapper/UserMapper.xml"/>
      </mappers>
  </configuration>
  ```

- 测试类：

  ```java
  public class MybatisTest {
      @Test
      public void test01() throws IOException {
          // 加载核心配置文件
          InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
          // 获取sqlSession工厂对象
          SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
          // 获取sqlSession对象
          SqlSession sqlSession = build.openSession();
          // 执行sql语句  参数: namespace + id
          List<Object> objects = sqlSession.selectList("userMapper.findAll");
          // 打印结果
          System.out.println(objects);
          // 释放资源
          sqlSession.close();
      }
  }
  ```

