# SSMP整合案例03

## 1、<span style="color:brown">表现层标准开发：</span>

**1.1、功能实现：**

创建一个**表现层包controller**，代码如下：

```java
@RestController
@RequestMapping("/books")
public class BookController {
    @Autowired
    IBookService iBookService;
    // 查询所有
    @GetMapping
    public List<Book> getAll() {
        List<Book> list = iBookService.list();
        return list;
    }
    // 添加数据
    @PostMapping
    public Boolean save(@RequestBody Book book) {
        return iBookService.save(book);
    }
    // 修改数据
    @PutMapping
    public Boolean update(@RequestBody Book book) {
        return iBookService.updateById(book);
    }
    // 删除数据
    @DeleteMapping("/{id}")
    public Boolean delete(@PathVariable Integer id) {
        return iBookService.removeById(id);
    }
    // 查询单个数据
    @GetMapping("/{id}")
    public Book getById(@PathVariable Integer id) {
        return iBookService.getById(id);
    }
    // 分页操作
    @GetMapping("/{current}/{size}")
    public IPage<Book> getByPage(@PathVariable int current, @PathVariable int size){
        return iBookService.getPage(current,size);
    }
}
```

在上述表现层功能编写完毕之后，启动引导类，开启数据访问！！！

**1.2、PostMan关于数据添加和修改操作：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP14.png" alt="image-20221014220754552" style="zoom:67%;" />

**1.3、注意事项：**

在进行功能封装时，首先需要注意**数据请求方式**：GET、POST、PUT、DELETE，以及是否需要在后面加上追加数据。

然后就是***关于请求方式后有追加数据设置情况***，接收参数的注解：

- @PathVariable
- @RequestBody



## 2、<span style="color:brown">表现层消息一致性处理：</span>

**2.1、问题描述：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP15.png" alt="image-20221014221436436" style="zoom:67%;" />

**2.2、采用data进行封装：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP16.png" alt="image-20221014221904826" style="zoom: 67%;" />

<span style="color:blue">添加**flag设置**，区分操作是否成功执行？</span>

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP17.png" alt="image-20221014222350379" style="zoom:67%;" />

**2.3、具体操作：**

## <!--设计表现层返回结果的模型类，用于后端与前端进行数据格式统一，也称为: "前后端数据协议"-->

- 前后端数据协议类：

  ```java
  @Data
  public class Result {
      private Boolean flag;
      // 此处使用Object, 是因为可能存在集合、IPage等
      private Object data;
      // 用于异常处理信息显示
      private String msg;
      public Result() {
      }
      public Result(Boolean flag) {
          this.flag = flag;
      }
      public Result(Boolean flag, Object data) {
          this.flag = flag;
          this.data = data;
      }
      public Result(Boolean flag, String msg) {
          this.flag = flag;
          this.msg = msg;
      }
      public Result(String msg) {
          this.flag = false;
          this.msg = msg;
      }
  }
  ```

- 表现层：

  ```java
  @RestController
  @RequestMapping("/books")
  public class BookControllerReal {
      @Autowired
      IBookService iBookService;
      // 查询所有
      @GetMapping
      public Result getAll() {
          Result result = new Result(true, iBookService.list());
          return result;
      }
      // 添加数据
      @PostMapping
      public Result save(@RequestBody Book book) {
          /*Result result = new Result();
          boolean flag = iBookService.save(book);
          result.setFlag(flag);*/
          Result result = new Result(iBookService.save(book));
          return result;
      }
      // 修改数据
      @PutMapping
      public Result update(@RequestBody Book book) {
          Result result = new Result(iBookService.updateById(book));
          return result;
      }
      // 删除数据
      @DeleteMapping("/{id}")
      public Result delete(@PathVariable Integer id) {
          Result result = new Result(iBookService.removeById(id));
          return result;
      }
      // 查询单个数据
      @GetMapping("/{id}")
      public Result getById(@PathVariable Integer id) {
          Result result = new Result(true, iBookService.getById(id));
          return result;
      }
      @GetMapping("/{current}/{size}")
      public Result getByPage(@PathVariable int current, @PathVariable int size, Book book){
          IPage<Book> page = iBookService.getPage(current, size, book);
          if(current > page.getPages()){
              // page.getPages()的返回值为long,所以这里需要强转
              page = iBookService.getPage((int)page.getPages(), size, book);
          }
          Result result = new Result(true, page);
          return result;
      }
  }
  ```



## 3、<span style="color:brown">前后端调用：</span>文件导入，及异步消息发送

**3.1、详解：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP18.png" alt="image-20221015210916107" style="zoom:80%;" />

---

![image-20221015215151061](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP23.png)

**3.2、文件导入：**

导入事先准备的css、js、pages、plugins文件，然后在**Maven配置中找到spring-ssmp项目的lifecycle，点击clean加载前端文件**。

之后在页面输入：`http://localhost/pages/books.html`检查是否加载成功？

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP19.png" alt="image-20221015212128557" style="zoom:67%;" />

**3.3、axios发送异步请求：**

- books.html：【部分展示】

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP20.png" alt="image-20221015213820978" style="zoom:67%;" />

- IDEA控制台展示：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP21.png" alt="image-20221015213936478" style="zoom: 67%;" />

- 浏览器控制台展示：

  <img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP22.png" alt="image-20221015214049611" style="zoom: 67%;" />

