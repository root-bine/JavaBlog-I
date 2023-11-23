## 1、<span style="color:brown">MySQL体系结构：</span>

**1.1、图示：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro01.png" alt="image-20230302193335957" style="zoom:67%;" />

**1.2、详解：**

- 连接层

  最上层是一些客户端和链接服务，主要完成一些类似于连接处理、授权认证、及相关的安全方案。同时，服务器也会为安全接入的每个客户端验证它所具有的操作权限。

- 服务层

  第二层架构主要完成大多数的核心服务功能，如：<u>*SQL接口，完成缓存的查询，SQL的分析和优化，部分内置函数的执行*</u>。所有跨存储引擎的功能也在这一层实现，如过程、函数等。

- 引擎层

  > `索引(Index)`是在引擎层实现

  **存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API和存储引擎进行通信**。

- 存储层

  主要是将数据存储在文件系统之上，并完成与存储引擎的交互。



## 2、<span style="color:brown">存储引擎：</span>

**2.1、概述：**

> 在MySQL5.5版本之后，默认存储引擎为：`InnoDB`

​		存储引擎就是*<u>存储数据、建立索引、更新/查询数据等技术</u>的实现方式*。存储引擎是<span style="color:red">基于表的</span>，而不是基于库的，所以存储引擎也可被称为**表类型**。

| `show create table 表名;` |           查看建表语句           |
| :-----------------------: | :------------------------------: |
|    **`show engine;`**     | **查看当前数据库支持的存储引擎** |

**2.2、分析：**

> 在建表时，如果想指定存储引擎，只需修改<u>属性ENGINE</u>即可

```sql
# 字段或列的注释，是用属性COMMENT来添加
CREATE TABLE `account` (
  `id` int NOT NULL AUTO_INCREMENT COMMENT "主键",
  `name` varchar(20) DEFAULT NULL COMMENT "姓名",
  `balance` double DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb3 COMMENT="账户表";
```

- `ENGINE=InnoDB：存储引擎为InnoDB`
- `AUTO_INCREMENT=5：主键自增，插入下一条数据id为5`
- `DEFAULT CHARSET=utf8mb3：默认字符集`



## 3、<span style="color:brown">InnoDB：</span>

**3.1、介绍：**

InnoDB是一种兼顾高可靠性和高性能的通用存储引擎，在MySQL5.5之后，InnoDB是默认的MySQL存储引擎。

**3.2、特点：**

- <u>*DML（增删改表中数据）*</u>操作遵循**ACID摸型**，支持<span style="color:green">**事务**</span>
  - ACID，表示事务的四大特性：原子性、一致性、隔离性、持久性

- <span style="color:green">**行级锁**</span>（`row-level locking`），提高并发访问性能

- 支持<span style="color:green">**外键约束**</span>（`foreign key`），保证数据的完整性和正确性

**3.3、文件：**

> 在MySQL8.0中，**参数innodb_file_per_table**，表示<u>每一张表都对应一个表空间文件</u>

`xxx.ibd`：xxx代表的是表名，innoDB引擎的每张表都会对应这样一个**表空间文件**，存储该表的<u>*表结构（frm、sdi) 、数据和索引*</u>。

| `show variables like "innodb_file_per_table"` | 查看MySQL系统中的innodb_file_per_table变量 |
| --------------------------------------------- | ------------------------------------------ |

---

ibd文件存储在<u>*安装的mysql目录下data文件中对应的数据库*</u>，如果需要查看其内容，则在当前路径下CMD，执行指令：

```scss
ibd2sdi xxx.ibd
```

**3.4、逻辑存储结构：**

Extend（*1M*）和Page（*16K*）都是由<u>磁盘</u>进行操作，且大小固定。因此，一个区可以容纳64个页！！！

- Trx id：最后一次操作事务的id
- Roll pointer：指针
- col：字段

![image-20230304163628744](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro02.png)



## 4、<span style="color:brown">MyISAM</span>&<span style="color:brown">Memory</span>

**4.1、MyISAM：**

MyISAM是MySQL早期的默认存储引擎，特点为：<u>不支持*事务和外键*</u>、<u>支持*表锁*，不支持行锁</u>、<u>访问速度快</u>。存储的文件类别有：

- `xxx.sdi`：存储表结构信息
- `xxx.MYD`：存储数据
- `xxx.MYI`：存储索引

**4.2、Memory：**

> Memory锁机制缺陷：对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性

<span style="color:green">Memory引擎的表数据是存储在**内存**中的</span>，由于受到硬件问题、或断电问题的影响，只能将<u>*这些表作为临时表或缓存使用*</u>。

- 特点：访问速度快、默认支持哈希索引
- 存储文件类别为：`xxx.sdi`
- 锁机制为：表锁

**4.3、InnocentDB与MyISAM的区别？**🎶🎶🎶

1. InnoDB支持事务，而MyISAM不支持
2. InnoDB的锁机制为：行级锁，而MyISAM是表级锁
3. InnoDB支持外键，MyISAM不支持

