## 1、<span style="color:brown">整合Mybatis-Plus：</span>

**1.1、整合方式01：**

新建模块，在该界面中**修改Server URL的内容**：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88MP01.png" alt="image-20221009164020550" style="zoom: 50%;" />

然后就可以在完成基础配置之后，搜索MyBatis-Plus，就会显示如下内容：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88MP02.png" alt="image-20221009163110829" style="zoom: 80%;" />

**1.2、整合方式02：**

进入模块创建界面，配置好基础信息，然后**仅导入MySQL Driver坐标**。在创建好基础项目之后，找到`pom.xml文件`，进行手动导入MyBatis-Plus的坐标【[Maven依赖网站查询](https://mvnrepository.com/)】：

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>
```



## 2、<span style="color:brown">项目内容：</span>

**2.1、错误展示：**

- 数据库：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88MP03.png" alt="image-20221009165325914"  />

- application.yml：

  ```yaml
  spring:
    datasource:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/db2?serverTimezone=UTC
      username: root
      password: 123456
  ```

- Book：

  ```java
  public class Book {
      private Integer id;
      private String type;
      private String name;
      private String description;
      
      // Getter and Setter
      ...
      // toString()
      ...
  }
  ```

- <span style="color:red">BookDao</span>：

  ```java
  @Mapper
  public interface BookDao extends BaseMapper<Book> {
  }
  ```

- 测试类：

  ```Java
  @SpringBootTest
  class SpringbootMybatisPlusApplicationTests {
      @Autowired
      private BookDao bookDao;
      @Test
      void contextLoads() {
          bookDao.selectById(2);
      }
  }
  ```

<u>***运行结果：***</u>

![image-20221009165730310](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SpringBoot%E6%95%B4%E5%90%88MP04.png)

**2.2、问题分析：**<span style="color:red">此时使用的数据库表名称为【tb_book】</span>；若是表名为【book】,则不会出现此错误！！！

由上图可知：**在使用Mybatis-Plus技术下进行数据库操作，无法查找到db2下的book表**

*解决方法：*

1. 在application.yml中添加如下内容：

   ```yaml
   mybatis-plus:
     global-config:
       db-config:
         table-prefix: tb_
   ```

2. 在数据封装**实体类**上加上注解：**@TableName("表名称")**

   ```java
   @TableName("tb_book")
   public class Book {
      ...
   }
   ```



## 3、<span style="color:brown">BaseMapper<T>讲解：</span>

## Insert

```java
// 插入一条记录
int insert(T entity);
复制代码
```

## Delete

```java
// 根据 entity 条件，删除记录
int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);
// 删除（根据ID 批量删除）
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
// 根据 ID 删除
int deleteById(Serializable id);
// 根据 columnMap 条件，删除记录
int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
复制代码
```

## Update

```java
// 根据 whereEntity 条件，更新记录
int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);
// 根据 ID 修改
int updateById(@Param(Constants.ENTITY) T entity);
复制代码
```

## Select

```java
// 根据 ID 查询
T selectById(Serializable id);
// 根据 entity 条件，查询一条记录
T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 查询（根据ID 批量查询）
List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
// 根据 entity 条件，查询全部记录
List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
// 查询（根据 columnMap 条件）
List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
// 根据 Wrapper 条件，查询全部记录
List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询全部记录。注意： 只返回第一个字段的值
List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 entity 条件，查询全部记录（并翻页）
IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询全部记录（并翻页）
IPage<Map<String, Object>> selectMapsPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询总记录数
Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
复制代码
```

| 类型                                 | 参数名    | 描述                                     |
| ------------------------------------ | --------- | ---------------------------------------- |
| `Wrapper <T>`                        | wrapper   | 实体对象封装操作类（可以为 null）        |
| `Collection<? extends Serializable>` | idList    | 主键ID列表(不能为 null 以及 empty)       |
| `Serializable`                       | id        | 主键ID                                   |
| `Map<String, Object>`                | columnMap | 表字段 map 对象                          |
| `IPage<T>`                           | page      | 分页查询条件（可以为 RowBounds.DEFAULT） |