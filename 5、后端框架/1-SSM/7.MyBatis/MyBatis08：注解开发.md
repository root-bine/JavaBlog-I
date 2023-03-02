## 1、<span style="color:brown">Mybatis常用注解：</span>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E5%B8%B8%E7%94%A8%E6%B3%A8%E8%A7%A3.png" alt="image-20221003002754000" style="zoom:80%;" />



## 2、<span style="color:brown">注解实现 CRUD：</span>

<u>***项目目录结构：***</u>

![image-20221003003559322](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E9%A1%B9%E7%9B%AE%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png)

<u>***编写实体类：domain层：***</u>

- User类

  ```java
  public class User implements Serializable {
      private Integer id;
      private String username;
      private String address;
      private String sex;
      private Date birthday;
  	// Getter and Setter
      // toString()
  }
  ```

<u>***注解开发dao层接口：***</u>

<span style="color:red">**通过注解方式，就不需要再去编写 UserMapper.xml 映射文件了！！！**</span>

- UserMapper接口

  ```java
  public interface UserMapper {
  	// 查询所有用户
      @Select("select * from user")
      List<User> findAll();
  	
      // 保存用户
      @Insert("insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday})")
      void saveUser(User user);
  
      // 更新用户
      @Update("update user set username=#{username},sex=#{sex},birthday=#{birthday},address=#{address} where id=#{id}")
      void updateUser(User user);
  
  	// 删除用户
      @Delete("delete from user where id=#{id}")
      void deleteUser(Integer userId);
  
  	// 根据id查询用户
      @Select("select * from user where id=#{id}")
      User findById(Integer userId);
  
  	// 根据用户名称模糊查询
      // @Select("select * from user where username like #{username}") //占位符
      @Select("select * from user where username like '%${value}%'")  //字符串拼接
      List<User> findByName(String username);
  
  	// 查询总数量
      @Select("select count(*) from user")
      int findTotal();
  
  }
  ```

<u>***编写 SqlMapConfig.xml、jdbc.properties：resource包***</u>

- SqlMapConfig.xml

  ```xml
  <!--引入外部配置文件-->
  <properties resource="jdbcConfig.properties"></properties>
      <!--配置别名-->
      <typeAliases>
          <package name="com.zgy.domain"/>
      </typeAliases>
      <!--配置环境-->
      <environments default="mysql">
          <environment id="mysql">
              <transactionManager type="JDBC"></transactionManager>
              <dataSource type="POOLED">
                  <property name="driver" value="${jdbc.driver}"></property>
                  <property name="url" value="${jdbc.url}"></property>
                  <property name="username" value="${jdbc.username}"></property>
                  <property name="password" value="${jdbc.password}"></property>
              </dataSource>
          </environment>
  </environments>
  <!--指定带有注解的dao接口所在位置-->
  <mappers>
      <package name="com.zgy.dao"></package>
  </mappers>
  ```

- jdbc.properties

  ```properties
  jdbc.driver=com.mysql.jdbc.Driver
  jdbc.url=jdbc:mysql://localhost:3306/test
  jdbc.username=root
  jdbc.password=root
  ```

<u>***编写测试类：***</u>

- AnnotationCRUDTest：

  ```java
  public class AnnotationCRUDTest {
      private InputStream in;
      private SqlSessionFactory factory;
      private SqlSession session;
      private IUserDao userDao;
  
      @Before
      public void init() throws Exception{
          in = Resources.getResourceAsStream("SqlMapConfig.xml");
          factory = new SqlSessionFactoryBuilder().build(in);
          session = factory.openSession();
          userDao = session.getMapper(IUserDao.class);
      }
  
      @After
      public void destory() throws Exception{
          session.commit();
          session.close();
          in.close();
      }
  
      @Test
      public void testSave(){
          User user = new User();
          user.setUsername("mybatis annotation");
          user.setAddress("北京");
  
          userDao.saveUser(user);
      }
  
      @Test
      public void testUpdate(){
          User user = new User();
          user.setId(55);
          user.setUsername("mybatis annotation");
          user.setAddress("北京");
          user.setSex("男");
          user.setBirthday(new Date());
  
          userDao.updateUser(user);
      }
  
      @Test
      public void testDelete(){
          userDao.deleteUser(54);
      }
  
      @Test
      public void testFindOne(){
          User user = userDao.findById(55);
          System.out.println(user);
      }
  
      @Test
      public void testFindByName(){
          //List<User> users = userDao.findByName("%Keafmd%");
          List<User> users = userDao.findByName("Keafmd");
          for (User user : users) {
              System.out.println(user);
          }
      }
  
      @Test
      public void testFindTotal(){
         int total = userDao.findTotal();
          System.out.println(total);
      }
  }
  ```



## 3、<span style="color:brown">注解实现复杂关系映射：</span>

```apl
1. 实现复杂关系映射之前, 我们可以在映射文件中通过配置<resultMap>来实现;
2. 在使用注解开发时, 我们需要借助@Results 注解, @Result 注解, @One 注解, @Many 注解;
```

### 复杂关系映射的注解说明：

<span style="color:green">**@Results 注解
**代替的是标签`<resultMap>`
该注解中可以使用单个@Result 注解，也可以使用@Result 集合
@Results（{@Result（），@Result（）}）或@Results（@Result（））</span>

<span style="color:green">**@Result 注解
**代替了`<id>` 标签和`<result>`标签</span>

---

### @Result 中的属性介绍：

| **@Result 中的属性** |                      **介绍**                       |
| :------------------: | :-------------------------------------------------: |
|          id          |                   是否是主键字段                    |
|        column        |                    数据库的列名                     |
|       property       |                  需要装配的属性名                   |
|         one          |  需要使用的@One 注解（@Result（one=@One）（）））   |
|         many         | 需要使用的@Many 注解（@Result（many=@many）（））） |

---

---

### @One 注解（一对一）

**代替了<assocation>标签，是多表查询的关键，在注解中用来指定子查询返回单一对象。**
@One 注解属性介绍：
select 指定用来多表查询的 sqlmapper
fetchType 会覆盖全局的配置参数 lazyLoadingEnabled。
<u>使用格式：@Result(column=" “,property=”",one=@One(select=""))</u>

### @Many 注解（多对一）
**代替了<collection>标签,是是多表查询的关键，在注解中用来指定子查询返回对象集合。**
注意：

聚集元素用来处理“一对多”的关系。需要指定映射的 Java 实体类的属性，属性的 javaType（一般为 ArrayList）但是注解中可以不定义。
<u>使用格式：@Result(property="",column="",many=@Many(select=""))</u>



## 4、<span style="color:brown">复杂关系映射开发：</span>

### 项目目录：

![image-20221003010142244](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E9%A1%B9%E7%9B%AE%E7%9B%AE%E5%BD%95.png)

### 添加 User 实体类及 Account 实体类：

- User：

  ```java
  public class User implements Serializable {
      private Integer userId;
      private String userName;
      private String userAddress;
      private String userSex;
      private Date userBirthday;
  	
      //一对多关系映射：一个用户对应多个账户
      private List<Account> accounts;
      /**
        *  Getter and Setter
        *  toString()
        */
  }
  ```
  
- Account：

  ```java
  public class Account implements Serializable {
      private Integer id;
      private Integer uid;
      private Double money;
  
      //多对一（mybatis中称之为一对一）的映射，一个账户只能属于一个用户 *
      private User user;
  	/**
        *  Getter and Setter
        *  toString()
        */
  }
  ```

### 添加账户的持久层接口并使用注解配置：dao层

- AccountMapper接口：

  ```java
  public interface AccountMapper {
  	// 查询所有账户, 并且获取每个账户下的用户信息 [一对一]
      // com.zgy.dao.UserMapper.findById, 根据用户得id进行关联
      @Select("select * from account")
      @Results(id="accountMap",value = {
              @Result(id = true,column = "id",property = "id"),
              @Result(column = "uid",property = "uid"),
              @Result(column = "money",property = "money"),
              @Result(property = "user",column = "uid",one=@One(select="com.zgy.dao.UserMapper.findById",fetchType= FetchType.EAGER))
      })
      List<Account> findAll();
      
  	// 根据用户id查询账户信息
      @Select("select * from account where uid = #{userId}")
      @ResultMap(value={"accountMap"})
      List<Account> findAccountByUid(Integer userId);
  }
  ```

### 添加用户的持久层接口并使用注解配置：dao层

- UserMapper接口：

  ```java
  public interface UserMapper {
  	// 查询所有用户
      @Select("select * from user")
      @Results(id="userMap",value={
              @Result(id = true,column = "id",property = "userId"),
              @Result(column = "id",property = "userId"),
              @Result(column = "username",property = "userName"),
              @Result(column = "sex",property = "userSex"),
              @Result(column = "birthday",property = "userBirthday")
          	@Result(property = "accounts" ,column = "id",
                      many = @Many(select = "com.zgy.dao.AccountMapper.findAccountByUid",
                              fetchType = FetchType.LAZY))
  
      })
      List<User> findAll();
  
  	// 根据id查询用户
      @Select("select * from user where id=#{id}")
      @ResultMap(value={"userMap"})
      User findById(Integer userId);
  
  	// 根据用户名称模糊查询
      @Select("select * from user where username like #{username}") //占位符
      @ResultMap("userMap")
      List<User> findByName(String username);
  
  }
  ```

### 测试：

- AccountTest：

  ```java
  public class AccountTest {
      private InputStream in;
      private SqlSessionFactory factory;
      private SqlSession session;
      private AccountMapper accountMapper;
      private UserMapper userMapper;
  
      @Before
      public void init() throws Exception{
          in = Resources.getResourceAsStream("SqlMapConfig.xml");
          factory = new SqlSessionFactoryBuilder().build(in);
          session = factory.openSession();
          accountMapper = session.getMapper(AccountMapper.class);
          userMapper = session.getMapper(UserMapper.class);
      }
  
      @After
      public void destory() throws Exception{
          session.commit();
          session.close();
          in.close();
      }
  
      @Test
      public void testFindAll(){
          List<Account> accounts = accountMapper.findAll();
          for (Account account : accounts) {
              System.out.println("-----每个账户信息-----");
              System.out.println(account);
              System.out.println(account.getUser());
          }
      }
      @Test
      public void testFindAll(){
          List<User> users = userMapper.findAll();
          for (User user : users) {
              System.out.println("-----每个用户的信息");
              System.out.println(user);
              System.out.println(user.getAccounts());
          }
      }
  }
  ```

### 运行结果：

- 一对一：

![image-20221003012141939](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E5%A4%9A%E8%A1%A8%E6%93%8D%E4%BD%9C%EF%BC%9A%E8%BF%90%E8%A1%8C%E7%BB%93%E6%9E%9C01.png)

- 一对多：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E5%A4%9A%E8%A1%A8%E6%93%8D%E4%BD%9C%EF%BC%9A%E8%BF%90%E8%A1%8C%E7%BB%93%E6%9E%9C02.png" alt="image-20221003012834427" style="zoom:150%;" />