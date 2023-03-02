## 1、<span style="color:brown">MySQL体系结构：</span>

**1.1、图示：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro01.png" alt="image-20230302193335957" style="zoom:67%;" />

**1.2、详解：**

- 连接层

  最上层是一些客户端和链接服务，主要完成一些类似于连接处理、授权认证、及相关的安全方案。同时，服务器也会为安全接入的每个客户端验证它所具有的操作权限。

- 服务层

  第二层架构主要完成大多数的核心服务功能，如：SQL接口，并完成缓存的查询，SQL的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。

- 引擎层

  **存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API和存储引擎进行通信**。不同的存储引擎具有不同的功能，这样就可以根据自己的需要，来选取合适的存储引擎。

- 存储层

  主要是将数据存储在文件系统之上，并完成与存储引擎的交互。



## 2、<span style="color:brown">存储引擎：</span>

存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可被称为表类型。

CREATE TABLE `account` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(20) DEFAULT NULL,
  `balance` double DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb3