## 1、<span style="color:brown">列表功能：</span>

**1.1、分析：**

需要在页面展示所有数据，而**数据传输格式采用data封装**。因此需要在books.html中找到`<el-table size="small" current-row-key="id" :data="dataList" stripe highlight-current-row>...</el-table>`，其中就是数据展示在页面上的情况。

之后在引入组件库部分的`<script></script>`中，dataList默认为空，所以需要在**methods**中配置相关信息！！！

**1.2、功能实现：**

- 钩子函数，VUE对象初始化完成后自动执行：

  ```javascript
  created() {
      // 调用查询全部数据的操作
      this.getAll()
  }
  ```
  
- 列表功能：

  ```javascript
  getAll() {
  	// 发送一步请求
  	axios.get("/books").then((res)=>{
  		this.dataList = (res.data).data
      })
  }
  ```
  
- 页面展示：

  ![image-20221015215425017](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP24.png)



## 2、<span style="color:brown">添加功能：</span>

**2.1、分析：**

**`思路1：`**

添加数据主要是实现页面中【添加】按钮，而在books.html中可以找到**新增标签弹层**：`<div class="add-form"></div>`。

主要实现两个部分的内容：

```apl
1. <el-dialog title="新增图书" :visible.sync="dialogFormVisible"></el-dialog>
2. 在<div slot="footer" class="dialog-footer"></div>中:
        - <el-button @click="cancel()">取消</el-button>

        - <el-button type="primary" @click="handleAdd()">确定</el-button>
```

其中**dialogFormVisible**默认值为false，需要设置成为true。之后对于【确定】按钮，一方面需要将访问路径发送，另一方面也需要将数据传输上去，但在<el-dialog title="新增图书" :visible.sync="dialogFormVisible"></el-dialog>中，***已经使用formData封装了数据属性***。<u>*因此，可以直接使用formData进行数据传输！！！*</u>

---

**`思路2：`**

在页面中进行添加数据时，会出现如下情况：

![image-20221017144808253](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP25.png)

因此，我们需要在**每一次弹出表单页面**时，进行数据的清理，即：**重置表单**！！！<u>执行方法为：`resetForm(){...}`</u>。

**2.2、功能实现：**

- 弹出添加窗口：

  ```javascript
  handleCreate() {
      this.dialogFormVisible = true
      // 每次打开添加页面,都会执行清理数据的操作
      this.resetForm()
  }
  ```
  
- 添加功能：

  ```javascript
  handleAdd () {
      // 在添加完数据之后, 需要关闭弹层
      axios.post("/books",this.formData).then((res)=>{
          // change(1) 判断当前操作是否成功
          if(res.data.flag){
              // 1.关闭弹层
              this.dialogFormVisible = false
              // 提示添加成功
              this.$message.success("添加成功")
          }else{
              // 提示添加失败
              this.$message.error("添加失败")
          }
      }).finally(()=>{
          // change(2) 无论添加操作是否成功,都需要进行页面刷新
          // 2.刷新数据
          this.getAll()
      })
  }
  ```

- 重置表单：

  ```javascript
  resetForm() {
      // 清理数据
      this.formData = {}
  }
  ```

- 取消按钮：【第一部分】

  ```javascript
  cancel(){
  	// 关闭添加弹层
      this.dialogFormVisible = false
      // 提示操作取消
      this.$message.info("当前操作取消")
  }
  ```



## 3、<span style="color:brown">删除功能：</span>

**3.1、分析：**

在进行删除功能编写时，需要找到：<el-table size="small" current-row-key="id" :data="dataList" stripe highlight-current-row></el-table>，然后找到对应区域：

![image-20221017151322697](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP26.png)

在具体编写中，仅需要**通过axios发送异步消息，然后在内部进行判断操作成功与否，以及操作结果提醒**。

另外，对于用户使用过程删除数据时，需要进行数据删除提示，防止误删数据。结果如下图：

![image-20221017154440095](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP27.png)

**3.2、功能实现：**

- 删除功能：

  ```javascript
  handleDelete(row) {
      // 问题: 用户删除重要数据时, 需要进行提示操作
      this.$confirm("此操作永久删除当前数据，是否执行？","提示",{type:"info"}).then(()=>{
          axios.delete("/books/"+row.id).then((res)=>{
              if(res.data.flag){
                  // 提示删除成功
                  this.$message.success("删除成功")
              }else{
                  // 提示删除失败
                  this.$message.error("数据同步失败，自动刷新")
              }
          }).finally(()=>{
              // 刷新页面
              this.getAll()
          })
      }).catch(()=>{
          // 取消操作提示【取消按钮】
          this.$message.info("取消操作")
      })
  }
  ```



## 4、<span style="color:brown">修改功能：</span>

**4.1、分析：**

**`思路1：`**

进行页面中修改数据，相当于**`列表+添加`**。而不同在于：此处的列表仅读取一条数据，然后对数据进行修改后，再添加到数据库中。

在`books.html`文件中，配置区域在<el-table size="small" current-row-key="id" :data="dataList" stripe highlight-current-row></el-table>，内容如下：

![image-20221017155049974](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP28.png)

---

**`思路2：`**

具体编写功能时，我们需要<u>**在页面弹出编辑窗口**</u>，然后根据**列表功能模块和添加功能模块**，读取出一条数据即可。而在文件中我们可以找到如下内容进行操作参考：

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/SSMP29.png" alt="image-20221017155708408" style="zoom: 67%;" />

---

**`思路3：`**

在完成弹窗的数据读取后，会存在一个问题：此过程是操作成功，那么操作失败怎么处理？因此需要进行一个`if(){...}`判定。

但由于`res.data`中有两个参数【flag、data】，因此我们需要限定条件：

```apl
res.data.flag && res.data.data != null
```

最后对于失败的情况，我们应该予以用户**消息提醒**，而<u>*无论操作成功或者失败，页面刷新都应该执行*</u>！！！

**4.2、功能实现：**

- 弹出编辑窗口：

  ```javascript
  handleUpdate(row) {
      axios.get("/books/"+row.id).then((res)=>{
          if(res.data.flag && res.data.data != null){
              this.dialogFormVisible4Edit = true
              // 先查询res下的data属性, 其中包括：flag、data
              this.formData = (res.data).data
          }else{
              this.$message.error("数据同步失败，自动刷新")
          }
      }).finally(()=>{
          this.getAll()
      })
  }
  ```

- 修改功能：

  ```javascript
  handleEdit() {
      axios.put("/books",this.formData).then((res)=>{
          if(res.data.flag){
              this.dialogFormVisible4Edit = false
              this.$message.success("修改成功")
          }else{
              this.$message.error("修改失败")
          }
      }).finally(()=>{
          this.getAll()
      })
  }
  ```

- 取消按钮：【第二部分】

  ```javascript
  cancel(){
  	// 关闭修改弹层
      this.dialogFormVisible4Edit = false
      // 提示操作取消
      this.$message.info("当前操作取消")
  }
  ```

