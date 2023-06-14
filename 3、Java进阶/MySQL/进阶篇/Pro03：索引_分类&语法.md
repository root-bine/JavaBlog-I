## 1、<span style="color:brown">索引分类：</span>

**1.1、按作用划分：**

> 对某一字段限定唯一约束，会给该字段自动创建一个唯一索引

|   分类   | 含义                                                 | 特点                     |  关键字  |
| :------: | :--------------------------------------------------- | :----------------------- | :------: |
| 主键索引 | 针对于表中主键创建的索引                             | 默认自动创建，只能有一个 | PRIMARY  |
| 唯一索引 | 避免同一个表中某数据列中的值重复                     | 可以有多个               |  UNIQUE  |
| 常规索引 | 快速定位特定数据                                     | 可以有多个               |          |
| 全文索引 | 全文索引查找的是文本中的关键词，而不是比较索引中的值 | 可以有多个               | FULLTEXT |

**1.2、按存储形式划分：**

在InnoDB存储引擎中，根据索引的存储形式，又可以分为以下两种：

|            分类             | 含义                                                         | 特点                 |
| :-------------------------: | ------------------------------------------------------------ | -------------------- |
| 聚集索引(`Clustered Index`) | 将<span style="color:brown">**数据存储与索引放到一块**</span>，索引结构的叶子节点保存了行数据 | 必须有，而且只有一个 |
| 二级索引(`Secondary Index`) | 将<span style="color:green">**数据与索引分开存储**</span>，索引结构的叶子节点关联的是对应的主键 | 可以存在多个         |

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro11.png" alt="image-20230305163308225" style="zoom: 50%;" />

**1.3、聚集索引选取规则：**

- 如果存在主键，主键索引就是聚集索引；
- 如果不存在主键，将使用<u>*第一个UNIQUE索引*</u>作为聚集索引；
- 如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引；



## 2、<span style="color:brown">回表查询：</span>

**2.1、演示：**

执行SQL语句：`select * from user where name = "Arm";`，过程为：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro12.png" alt="image-20230305163613452" style="zoom: 67%;" />

​		进入二级索引，比较`Lee`首字母为`L`与`Arm`首字母为`A`，排在前面，因而进入左节点。然后比较`Geek`和`Jack`的首字母：`G、J`，排在它们之前，从而进入左节点，最后找到`Arm`数据，即：id值。之后进入聚集索引，id值小于15，进入左节点：[10,12]，然后进入中节点[10,11]，找到id为10的数据。

**2.2、以下SQL语句，那个执行效率高？为什么？**🤩🤩🤩

```sql
# id为主键, name字段创建的有索引
1. select * from user where id = 10;
2. select * from user where name ='Arm';
```

第一个效率高，因为聚集索引直接根据主键id查询数据，而二次索引需要进行回表查询才能间接获取数据！！！

**2.3、InnoDB主键索引的B+Tree高度是多少？**😎😎😎

> InnoDB中，指针占用空间大小为6字节，key大小取决于主键类型

​		假设一行数据的大小为1k，<u>一页就能存放16行数据</u>。指针占用6个字节，主键类型为bigint，占用8字节空间。设key的个数为n，得出等式：
$$
InnoDB逻辑存储结构中，1页大小为16k。
\\此时高度为2，则只有一个非叶子节点: n*8+(n+1)*6 = 16*1024，key的个数为: 1170。
\\若高度为3，则每个非叶子节点下都有n+1个叶子节点，此时数据总数为: 1171*1171*16
$$



## 3、<span style="color:brown">索引语法：</span>

**3.1、语法结构：**

> 创建索引时，若无索引关键字，则直接创建**<u>常规索引</u>**

```sql
# 创建Index
CREATE [UNIQUE|FULLTEXT] INDEX 索引名称(idx_表名_字段名) On 表名 (字段名);
# 查看Index
SHOW INDEX FROM 表名;
# 删除Index
DROP INDEX 索引名称(idx_表名_字段名) ON 表名;
```

**3.2、案例分析：**

```sql
CREATE TABLE `t_student` (
  `student_no` int NOT NULL COMMENT '学号',
  `student_name` varchar(20) DEFAULT NULL COMMENT '姓名',
  `student_sex` varchar(255) DEFAULT NULL COMMENT '性别',
  `stu_pass` varchar(12) DEFAULT NULL COMMENT '删除状态',
  PRIMARY KEY (`student_no`),
  KEY `index_student_name` (`student_name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3
```

- 查询t_student表的索引

  `show index from t_student;`

- 给student_name字段创建常规索引

  `create index idx_student_name on t_student(student_name);`

- 删除student_name字段的常规索引

  `drop index idx_student_name on t_student;`

- 为student_name、student_sex创建**联合索引**

  `create index idx_student_name_sex on t_student(student_name,student_sex);`

