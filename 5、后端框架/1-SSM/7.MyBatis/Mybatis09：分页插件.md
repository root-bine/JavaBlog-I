## 1、<span style="color:brown">引入分页插件</span>

在pom.xml中添加如下依赖：

- SpringBoot

  ```xml
  <dependency>
  	<groupId>com.github.pagehelper</groupId>
  	<artifactId>pagehelper-spring-boot-starter</artifactId>
  	<version>1.4.1</version>
  </dependency>
  ```

- SpringMVC

  ```xml
  <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>5.1.10</version>
  </dependency>
  ```

  

## 2、<span style="color:brown">pageHelper配置</span>

**2.1、SpringBoot：application.yml**

``` yml
# mybatis 相关配置
mybatis:
  #... 其他配置信息
  configuration-properties:
    offsetAsPageNum: true
    rowBoundsWithCount: true
    reasonable: true
  mapper-locations: mybatis/mapper/*.xml
```

**2.2、普通springmvc项目配置：mybatis-config.xml**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor">
      <!-- 该参数默认为false -->
      <!-- 设置为true时，会将RowBounds第一个参数offset当成pageNum页码使用 -->
      <!-- 和startPage中的pageNum效果一样-->
      <property name="offsetAsPageNum" value="true"/>
      <!-- 该参数默认为false -->
      <!-- 设置为true时，使用RowBounds分页会进行count查询 -->
      <property name="rowBoundsWithCount" value="true"/>
      <!-- 设置为true时，如果pageSize=0或者RowBounds.limit = 0就会查询出全部的结果 -->
      <!-- （相当于没有执行分页查询，但是返回结果仍然是Page类型）-->
      <property name="pageSizeZero" value="true"/>
      <!-- 3.3.0版本可用 - 分页参数合理化，默认false禁用 -->
      <!-- 启用合理化时，如果pageNum<1会查询第一页，如果pageNum>pages会查询最后一页 -->
      <!-- 禁用合理化时，如果pageNum<1或pageNum>pages会返回空数据 -->
      <property name="reasonable" value="true"/>
      <!-- 3.5.0版本可用 - 为了支持startPage(Object params)方法 -->
      <!-- 增加了一个`params`参数来配置参数映射，用于从Map或ServletRequest中取值 -->
      <!-- 可以配置pageNum,pageSize,count,pageSizeZero,reasonable,orderBy,不配置映射的用默认值 -->
      <!-- 不理解该含义的前提下，不要随便复制该配置 -->
      <property name="params" value="pageNum=start;pageSize=limit;"/>
      <!-- 支持通过Mapper接口参数来传递分页参数 -->
      <property name="supportMethodsArguments" value="true"/>
      <!-- always总是返回PageInfo类型,check检查返回类型是否为PageInfo,none返回Page -->
      <property name="returnPageInfo" value="check"/>
    </plugin>
  </plugins>
</configuration>
```



## 3、<span style="color:brown">pageHelper使用</span>

　使用的时候，只需在查询list前，调用 startPage 设置分页信息，即可使用分页功能。

```java
public Object getUsers(int pageNum, int pageSize) {
	PageHelper.startPage(pageNum, pageSize);
	// 不带分页的查询
 	List<UserEntity> list = userMapper.selectAllWithPage(null);
	// 可以将结果转换为 Page , 然后获取 count 和其他结果值
	com.github.pagehelper.Page listWithPage = (com.github.pagehelper.Page) list;
	System.out.println("listCnt:" + listWithPage.getTotal());
	return list;
}
```

即：使用时，只需提前声明要分页的信息，得到的结果就是有分页信息的了.。如果不想进行count，只要查分页数据，则调用: `PageHelper.startPage(pageNum, pageSize, false);` 即可，避免了不必要的count消耗。
