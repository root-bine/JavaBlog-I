# SSMP整合案例02

## 1、<span style="color:brown">业务层标准开发：</span>基础CRUD、分页

**1.1、数据层与业务层的区别：**

- ***各层作用：***

  ![image-20221014103727363](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP12.png)

- ***区分方法：***

  ![image-20221014103755064](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP11.png)

- 范例：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP10.png" alt="image-20221014103045180" style="zoom:80%;" />

**1.2、功能实现：**

- 业务层接口：

  ```java
  public interface BookService {
      Boolean save(Book book);
      Boolean update(Book book);
      Boolean delete(Integer id);
      Book getById(Integer id);
      List<Book> getAll();
      IPage<Book> getPage(int currentPage, int pageSize);
  }
  ```

- 业务层实现类：

  ```java
  @Service
  public class BookServiceImpl implements BookService {
      @Autowired
      BookDao bookDao;
      @Override
      public Boolean save(Book book) {
          // int insert(T entity), 该方法返回的是影响行的数量
          return bookDao.insert(book) > 0;
      }
      @Override
      public Boolean update(Book book) {
          return bookDao.updateById(book) > 0;
      }
      @Override
      public Boolean delete(Integer id) {
          return bookDao.deleteById(id) > 0;
      }
      @Override
      public Book getById(Integer id) {
          return bookDao.selectById(id);
      }
      @Override
      public List<Book> getAll() {
          return bookDao.selectList(null);
      }
      @Override
      public IPage<Book> getPage(int currentPage, int pageSize) {
          IPage<Book> iPage = new Page<Book>(currentPage,pageSize);
          return bookDao.selectPage(iPage,null);
      }
  }
  ```

- 测试类：

  ```java
  @SpringBootTest
  public class ServiceApplicationTests {
      @Autowired
      BookService bookService;
  
      @Test
      void testById(){
          bookService.getById(2);
      }
      @Test
      void testSave(){
          Book book = new Book();
          book.setType("测试数据");
          book.setName("测试数据789");
          book.setDescription("测试数据789");
          bookService.save(book);
      }
      @Test
      void testUpdate(){
          Book book = new Book();
          book.setId(10);
          book.setType("更新数据");
          book.setName("测试数据789");
          book.setDescription("测试数据789");
          bookService.update(book);
      }
      @Test
      void testDelete(){
          bookService.delete(12);
      }
      @Test
      void testGetAll(){
          bookService.getAll();
      }
      @Test
      void testPage(){
          bookService.getPage(1,2);
      }
  }
  ```



## 2、<span style="color:brown">业务层标准开发：</span>基于Mybatis-Plus构建

**2.1、开发详解：**

![image-20221014204920052](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP13.png)

**2.2、功能实现：**

- 业务层接口：

  ```java
  public interface IBookService extends IService<Book> {
      //IPage<Book> getPage(int current, int size);
      IPage<Book> getPage(int current, int size, Book book);
  }
  ```

- 业务层实现类：

  ```java
  @Service
  public class IBookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {
      // 若业务需求超出以提供方法范畴, 可追加方法, 参照基础CRUD模板
      @Autowired
      BookDao bookDao;
      @Override
      public IPage<Book> getPage(int current, int size, Book book) {
          LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<>();
          lqw.like(Strings.isEmpty(book.getType()),Book::getType,book.getType());
          lqw.like(Strings.isEmpty(book.getName()),Book::getName,book.getName());
          lqw.like(Strings.isEmpty(book.getDescription()),Book::getDescription,book.getDescription());
          IPage<Book> iPage = new Page<>(current,size);
          return bookDao.selectPage(iPage,lqw);
      }
  }
  ```
