## 1、<span style="color:brown">异常信息处理：</span>【参考】

**1.1、问题描述：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP30.png" alt="image-20221017171812079" style="zoom:67%;" />

**1.2、编写异常处理类：**

事先需要在Result类中新定义一个属性：String  msg，用于显示异常处理信息。

之后在内部分别定义：flag+msg、msg，两种构造函数；其中只有msg的构造函数，内部还需定义：`this.flag = false`

<!--@ControllerAdvice/@RestControllerAdvice + @ExceptionHandler 全局处理 Controller 层异常-->

```java
// 作为SpringMVC的异常处理器
// @ControllerAdvice
@RestControllerAdvice
public class ProjectExceptionAdvice {
    // 拦截所有的异常信息
    @ExceptionHandler
    public Result doException(Exception ex){
        ex.printStackTrace();
        return new Result("服务器故障，请稍后再试！");
    }
}
```

**1.3、手动编写一个异常：**

表现层controller中，在**添加数据方法`save()`**中定义如下：

```java
@PostMapping
public Result save(@RequestBody Book book) throws IOException {
    if(book.getName().equals("123")) {
        throw new IOException();
    }
    boolean flag = iBookService.save(book);
    return new Result(flag,flag? "添加成功^_^" : "添加失败-_-!");
}
```

**1.4、修改前端添加部分代码：**

```javascript
handleAdd () {
    // 在添加完数据之后, 需要关闭弹层
    axios.post("/books",this.formData).then((res)=>{
        // change(1) 判断当前操作是否成功
        if(res.data.flag){
            // 1.关闭弹层
            this.dialogFormVisible = false
            // 提示添加成功
            this.$message.success((res.data).msg)
        }else{
            // 提示添加失败
            this.$message.error((res.data).msg)
        }
    }).finally(()=>{
        // change(2) 无论添加操作是否成功,都需要进行页面刷新
        // 2.刷新数据
        this.getAll()
    })
}
```



## 2、<span style="color:brown">分页：</span>

**2.1、分析：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP31.png" alt="image-20221018153936742" style="zoom: 67%;" />

根据分页组件、分页数据模型，核心的编写变量为：currentPage、pageSize、total，而这三个变量分别对应了`getCurrent()、getSize()、getTotal()`。

在封装完后需要整合数据显示到页面上，那么就需要将`getRecords()`加载到前端来，以records显示。

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP32.png" alt="image-20221018154051339" style="zoom:80%;" />

**2.2、Bug处理：**分页中，出现的删除BUG

<u>假设当前总页面数为3，且第三页只有一条数据</u>，将该数据删除，**页面仍旧显示当前页面为3，无法跳转到第二页**。

![image-20221018160320837](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP33.png)

因此，需要在表现层controller中的分页功能中，进行判断处理：

- <span style="color:red">**如果当前页面数  >   总页数，那么就重新进行数据查询**</span>

```java
// 分页操作
@GetMapping("/{current}/{size}")
public Result getByPage(@PathVariable int current, @PathVariable int size){
    IPage<Book> page = iBookService.getPage(current, size);
    if(current > page.getPages()){
        // page.getPages()的返回值为long,所以这里需要强转
        page = iBookService.getPage((int)page.getPages(), size);
    }
    Result result = new Result(true, page);
    return result;
}
```

**2.3、功能实现：**

- 分页查询【分页功能与列表功能结合使用】

  ```javascript
  getAll() {
      // 发送一步请求
      axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize).then((res)=>{
          this.pagination.currentPage = (res.data).data.current
          this.pagination.pageSize = (res.data).data.size
          this.pagination.total = (res.data).data.total
          // 这里的records是后端表现层返回的page对象
          // 当page对象转换为json时里面会有一个records保存这些数据
          this.dataList = ((res.data).data).records
      })
  }
  ```

- 切换页码：

  ```javascript
  handleCurrentChange(currentPage) {
      // 1.修改当前页码
      this.pagination.currentPage = currentPage
      // 2.重新查询列表和分页
      this.getAll()
  }
  ```

​	

## 3、<span style="color:brown">条件查询：</span>

**3.1、前端分析：**

​		根据页面中查询部分显示，所需要的是type、name、description三个变量，而在books.html中该部分是根据`getAll()方法`来进行实现。但**在分页部分已经将原始的列表功能进行了组合：分页+列表**，所以只需要在分页数据模型中添加上述三个变量：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP35.png" alt="image-20221018165203426" style="zoom:80%;" />

然后在组件部分添加上数据模型封装：

![image-20221018164926465](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP34.png)

因此，**getAll()方法目前综合了分页、列表、条件查询三部分功能**！！！

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP36.png" alt="image-20221018165328672" style="zoom: 67%;" />

---

**3.2、后端分析：**

由于采用的是修改前端显示，而数据的传输仍然还是后端进行操作，所以需要对表现层Controller的分页功能再一次进行修改：

![image-20221018165651402](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP37.png)

之后需要进一步修改业务层Service：

![image-20221018170034273](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP38.png)

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP39.png" alt="image-20221018174058592" style="zoom:67%;" />
