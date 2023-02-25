## 1、<span style="color:brown">原始JDBC操作：</span>

**1.1、查询数据：**

![image-20220924194629581](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8E%9F%E5%A7%8BJDBC%EF%BC%9A%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE.png)

---

**1.2、插入数据：**

![image-20220924202912854](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8E%9F%E5%A7%8BJDBC%E6%93%8D%E4%BD%9C%EF%BC%9A%E6%8F%92%E5%85%A5%E6%95%B0%E6%8D%AE.png)



## 2、<span style="color:brown">原始JDBC操作分析：</span>

![image-20220924203938346](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E5%8E%9F%E5%A7%8BJDBC%E6%93%8D%E4%BD%9C%E5%88%86%E6%9E%90.png)



## 3、<span style="color:brown">什么是Mybatis？</span>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/Mybatis%E7%9A%84%E6%A6%82%E8%BF%B0.png" alt="image-20220924204544966" style="zoom:80%;" />



## 4、<span style="color:brown">Mybatis的相关API：</span>

**4.1、SqlSession工厂构造器：SqlSessionFactoryBuilder**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SqlSession%E5%B7%A5%E5%8E%82%E6%9E%84%E9%80%A0%E5%99%A8.png" alt="image-20220926204728805" style="zoom:80%;" />

**4.2、SqlSession工厂对象：SqlSessionFactory**

### <!--使用第二种方式,在后期实现JDBC中不需要编写sqlSession.commit()-->

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%E8%8E%B7%E5%8F%96SqlSession%E5%AF%B9%E8%B1%A1.png" alt="image-20220926205155565" style="zoom:80%;" />

**4.3、SqlSession会话对象：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SqlSession%E4%BC%9A%E8%AF%9D%E5%AF%B9%E8%B1%A1.png" alt="image-20220926210141426" style="zoom:80%;" />
