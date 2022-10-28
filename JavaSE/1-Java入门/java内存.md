# java内存

- 栈【Stack】：

  - <font color="red">**存放的都是方法中的局部变量；**</font>

  - <font color="#0099ff">方法一定是要在栈中运行</font>
  - 局部变量：方法中的参数或者方法｛｝内部的变量；
  - 作用域：一旦超出作用域，立刻就会从内存中释放；
- 堆【Heap】：

  - <font color="red">**凡事new出来的东西，都存放在Heap中；**</font>

  - <font color="orange">**字符串常量池也存在Heap中;**</font>
  - 堆内存中的东西都会有一个<font color="#0099">**地址值**</font>:16进制；
  - 堆内存中的数据都有一个默认值，规则：
    - 如果是整数					默认为0；
    - 如果是浮点数				默认为0.0；
    - 如果是字符					默认为'\u0000'；
    - 如果是布尔值				默认为false；
    - 如果是引用类型			默认为null；
- 方法区【Method Area】：
  - <font color="#0099ff">**存储  .class文件相关信息，包含方法的信息；**</font>
  - <font color='red'>**该区存在一个静态区**</font>
- 本地方法区【Native Method Area】：与操作系统有关；
- 寄存器【pc Register】：与CPU相关；
