### <span style="color:green">除开<u>传统开发方式</u>、<u>接口代理开发</u>，还有就是：非接口模式开发【Mybatis02】</span>

## 1、<span style="color:brown">传统开发方式：</span>

### <!--在resources文件下，mapper/UserMapper.xml、SqlMapConfig.xml、jdbc.properties不变-->

### <!--在domain包下，实体类User不变，且pom.xml文件内容也不变-->

- 创建UserDao接口、UserDaoImpl实现类：

  ```Java
  public interface UserDao {
      public abstract List<User> findAll() throws IOException;
  }
  ```

  ```java
  public class UserDaoImpl implements UserDao {
      @Override
      public List<User> findAll() throws IOException {
          InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
          SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
          SqlSession sqlSession = build.openSession();
          List<User> objects = sqlSession.selectList("userMapper.findAll");
          return objects;
      }
  }
  ```

- 创建测试类：

  ```java
  public class Test {
      public static void main(String []args) throws IOException {
          UserDaoImpl userDao = new UserDaoImpl();
          List<User> all = userDao.findAll();
          System.out.println(all);
      }
  }
  ```

  

## 2、<span style="color:brown">接口代理方式：</span>

**2.1、接口代理开发的简介：**

![image-20220926213610458](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E6%8E%A5%E5%8F%A3%E4%BB%A3%E7%90%86%E5%BC%80%E5%8F%91%E6%96%B9%E5%BC%8F%E7%AE%80%E4%BB%8B.png)

**2.2、Mapper接口的开发原则：**

![image-20220926213709488](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mapper%E6%8E%A5%E5%8F%A3%E7%9A%84%E5%BC%80%E5%8F%91%E5%8E%9F%E5%88%99.png)

**2.3、代码实现：**

### <!--Mapper.xml文件中的namespace，与Mapper的全类名相同-->

### <!--SqlMapConfig.xml、jdbc.properties不变、实体类User、pom.xml，不变-->

- SqlMapConfig.xml

  ```xml
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

- jdbc.properties

  ```properties
  jdbc.driver = com.mysql.cj.jdbc.Driver
  jdbc.url = jdbc:mysql://localhost:3306/test
  jdbc.username = root
  jdbc.password = 123456
  ```

- 实体类User

  ```java
  public class User {
      private int id;
      private String name;
      private String pwd;
  	// Getter and Setter
      // toString()
  }
  ```

---

- dao包下的Mapper接口：`UserMapper`

  ```java
  public interface UserMapper {
      public abstract List<User> findAll();
  }
  ```

- UserMapper.xml

  ```xml
  <mapper namespace="com.zgy.dao.UserMapper">
      <!--查询操作, 并别名处理-->
      <select id="findAll" resultType="user">
          select * from user
      </select>
  </mapper>
  ```
  
- 测试类：`MybatisTest.xml`

  ```java
  public class MybatisTest {
      @Test
      public void FactoryTest() throws IOException {
          InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
          SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
          SqlSession sqlSession = sqlSessionFactory.openSession();
          // 获得Mybatis框架生成的UserMapper接口的实现类
          UserMapper mapper = sqlSession.getMapper(UserMapper.class);
          List<User> all = mapper.findAll();
          System.out.println(all);
          sqlSession.close();
      }
  }
  ```