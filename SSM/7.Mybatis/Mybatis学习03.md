# Mybatis映射文件：UserMapper.xml

## 1、<span style="color:brown">Mybatis的映射文件：</span>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E7%9A%84%E6%98%A0%E5%B0%84%E6%96%87%E4%BB%B6.png" alt="image-20220924232538887" style="zoom:80%;" />



## 2、<span style="color:brown">Mybatis的映射文件深入：</span>

**2.1、Dynamic SQL<!--动态SQL-->的概述和环境搭建：**

```apl
Mybatis的映射文件中, 前面我们的SQL都是比较简单的, '有些时候业务逻辑复杂时, 我们的SQL是动态变化的'.
```

**2.2、动态SQL之<if>：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8A%A8%E6%80%81SQL%E4%B9%8Bif.png" alt="image-20220926235243136" style="zoom:80%;" />

**2.3、动态SQL之<foreach>：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8A%A8%E6%80%81SQL%E4%B9%8Bforeach.png" alt="image-20220927001013817" style="zoom:80%;" />

**2.4、SQL的片段抽取：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SQL%E7%9A%84%E7%89%87%E6%AE%B5%E6%8A%BD%E5%8F%96.png" alt="image-20220927003632745" style="zoom:80%;" />

**2.5、代码实现：**<!--接口代理开发-->

#### <!--SqlMapConfig.xml、实体类User、pom.xml、jdbc.properties，这些文件内容不变-->

- UserMapper接口：`dao包`

  ```java
  public interface UserMapper {
      List<User> findAll(User user);
      List<User> findByIds(List<Integer> ids);
  }
  ```

- UserMapper.xml：`resources/mapper`

  ```xml
  <!--抽取sql片段-->
  <sql id="selectUser">select * from user</sql>
  <!--SQL之if-->
  <select id="findAll" parameterType="user" resultType="user">
      <include refid="selectUser"/>
      <where>
          <if test="id != 0">
              and id = #{id}
          </if>
          <if test="username != null">
              and username = #{username}
          </if>
          <if test="password != null">
              and password = #{password}
          </if>
      </where>
  </select>
  <!--SQL之foreach-->
  <select id="findByIds" parameterType="list" resultType="user">
      <include refid="selectUser"/>
      <where>
          <foreach collection="list" open="id in (" close=")" item="id" separator=",">
              #{id}
          </foreach>
      </where>
  </select>
  ```

- DynamicTest：测试类

  ```java
  @Test
  public void testUser() throws IOException {
      InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
      SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
      SqlSession sqlSession = sessionFactory.openSession();
      UserMapper mapper = sqlSession.getMapper(UserMapper.class);
      // 模拟条件user
      User user = new User();
      user.setUsername("zhangsan");
      List<User> all = mapper.findAll(user);
      System.out.println(all);
      sqlSession.close();
  }
  @Test
  public void testList() throws IOException {
      InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");
      SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
      SqlSession sqlSession = sessionFactory.openSession();
      UserMapper mapper = sqlSession.getMapper(UserMapper.class);
      // 模拟ids数据
      List<Integer> list = new ArrayList<Integer>();
      list.add(1);
      list.add(2);
      list.add(3);
      List<User> byIds = mapper.findByIds(list);
      System.out.println(byIds);
      sqlSession.close();
  }
  ```
