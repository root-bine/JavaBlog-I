## 1、<span style="color:brown">基础内容：</span>

**1.1、概述：**

Spring框架对JDBC的封装，提供了**一个JDBCTemplate对象来简化JDBC开发**。

**1.2、使用：**

1. 导入jar包：

   commons-logging-1.2.jar、spring-beans-5.3.8.jar、spring-core-5.3.8.jar、

   spring-jdbc-5.3.8.jar、spring-tx-5.3.7.jar、mysql-connector-java-8.0.19.jar

2. 创建JDBCTemplate对象，传入参数DataSource对象：

   ```java
   JDBCTemplate template = new JDBCTemplate(ds)
   ```

3. 调用JDBCTemplate对象的方法：

   - `update(...)`：执行DML语句，增、删、改
     - 参数是：sql语句、给sql语句中的？赋值
   - `queryForMap(...)`：查询结果，并将结果封装成Map集合
     - 查询的结果只能是一条记录
     - 参数是：sql语句、给sql语句中的？赋值
   - `queryForList(...)`：查询结果，并将结果封装成List集合
     - 将<span style="color:green">**各条数据封装成Map集合作为List集合的泛型**</span>，可以查询多条记录
   - `queryForObject(...)`：查询结果，并将结果封装成对象
     - 传入的参数有：`sql 、封装的返回值类型.class`
   - `query(...)`：查询结果，并将结果封装成JavaBean对象
     - 该方法的参数是：`RowMapper<T>`
     - 由于BeanPropertyRowMapper<T>  implements  RowMapper<T>，因此一般使用<span style="color:red">**BeanPropertyRowMapper**<T></span>;

## 2、<span style="color:brown">BeanPropertyRowMapper<T>与RowMapper<T>：</span>

**2.1、使用RowMapper<T>情况：**

```java
List<Account> list = template.query(sql, new RowMapper<Account>() {
    @Override
    public Account mapRow(ResultSet resultSet, int i) throws SQLException {
        Account account = new Account();
        int id = account.getId();
        String name = account.getName();
        Double balance = account.getBalance();
        account.setId(id);
        account.setName(name);
        account.setBalance(balance);
        return account;
    }
});
```

**2.2、使用BeanPropertyRowMapper<T>的情况：**

```java
List<Account> list = template.query(sql,new BeanPropertyRowMapper<Account>(Account.class));
```

## 3、<span style="color:brown">案例综合：</span>

**2.1、基础使用：**

<u>**JDBCUtils类是Druid工具类！！！**</u>

```java
public class JDBCTemplateDemo01 {
    public static void main(String[] args) {
        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
        String sql = "update account set balance = 500 where id = ?";
        int count = template.update(sql, 1);
        System.out.println(count);
    }
}
```

**2.2、执行DML语句：**

```java
public class JDBCTemplateDemo01 {
    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
    @Test
    public void test1(){
        String sql = "update account set balance = balance + ? where id = ?";
        int count = template.update(sql, 500,1);
        System.out.println(count);
    }
    @Test
    public void test2(){
        String sql = "insert into account(name,balance) values(?,?)";
        int count = template.update(sql, "wangwu", 1500);
        System.out.println(count);
    }
    @Test
    public void test3(){
        String sql = "delete from account where id = ?";
        int count = template.update(sql, 4);
        System.out.println(count);
    }
}
```

**2.3、执行DQL语句：**基础数据封装

```java
public class JDBCTemplateDemo01 {
    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
    @Test
    public void test1(){
        String sql = "select * from account where id = ?";
        Map<String, Object> map = template.queryForMap(sql, 1);
        //{id=1, name=zhangsan, balance=1500.0}
        System.out.println(map);
    }
    @Test
    /**
     * {id=1, name=zhangsan, balance=1500.0}
     * {id=2, name=lisi, balance=1000.0}
     * {id=3, name=sunliu, balance=1500.0}
     */
    public void test2(){
        String sql = "select * from account";
        List<Map<String, Object>> list = template.queryForList(sql);
        for (Map<String, Object> i:list) {
            System.out.println(i);
        }
    }
}
```

**2.4、执行DQL语句：**业务数据封装

```java
public class JDBCTemplateDemo01 {
    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
    @Test
    public void test1(){
        String sql = "select * from account";
        List<Account> list = template.query(sql,new BeanPropertyRowMapper<Account>(Account.class));
        for (Account i:list) {
            System.out.println(i);
        }
    }
    @Test
    //查询总记录数
    public void test2(){
        String sql = "select count(id) from account";
        //由于是搜索数据总条数，所以封装返回值则是Long
        Long count = template.queryForObject(sql, Long.class);
        //数据总记录：3
        System.out.println("数据总记录："+count);
    }
}
```
