## 1、<span style="color:brown">Stack内存溢出：</span>

**1.1、原因：**

- *<u>栈帧过多</u>导致栈内存溢出*；
- *<u>栈帧过多</u>导致栈内存溢出*；

**1.2、JVM参数：**

> <u>可以在`Edit Configurations`中的`VM options`设置设置</u>。

```scss
// 来划分栈内存大小, 如: -Xss1024k
-Xss
```

**1.3、演示：**

```java
public class demo {
    private static int count;
    public static void main(String []args){
        try {
            method();
        } catch (Throwable e) {
            e.printStackTrace();
            System.out.println(count);
        }
    }
    private static void method(){
        count++;
        method();
    }
}
```

上述范例就是没有设置递归界限，导致无限进行递归操作，最终导致栈内存耗尽，抛出：`java.lang.StackOverflowError`异常。

---

```java
public class demo {
    public static void main(String []args) {
        Dept d = new Dept();
        d.setName("Market");

        Emp e1 = new Emp();
        e1.setName("Tom");
        e1.setDept(d);
        Emp e2 = new Emp();
        e2.setName("Marry");
        e2.setDept(d);

        d.setEmp(Arrays.asList(e1,e2));
        //转换成Json数据
        ObjectMapper mapper = new ObjectMapper();
        System.out.println(mapper.writeValueAsString(d));
    }
}
class Emp{
    String name;
    Dept dept;
    // Getter and Setter
}
class Dept{
    String name;
    List<Emp> emp;
    // Getter and Setter
}
```

由于Json转换对象是`Dept d`，所以会把部门名称转换，然后转换其中的`List<Emp> emp`对象。

然后转换集合，又读取到了员工名称和部门信息，而部门信息又包含了部门名称和部门员工信息......

按照上述读取转化步骤，数据格式如下：

```json
{name:"Market",emp:[{name:'Tom',dept:{name:'Market',emp:[{name:'Tom',dept:{name:'',...}}]}}]}
```

从而造成了反复套用、读取数据的情况，从而抛出：`java.lang.StackOverflowError`异常。

<u>可以在员工类的`Dept dept`上使用注解`@JsonIgnore`，让在Json转换时忽略此项数据</u>！！！



## 2、<span style="color:brown">Heap内存溢出：</span>

**2.1、演示：**

```java
public class demo {
    public static void main(String []args) {
        int i = 0;
        List<String> list = new ArrayList<>();
        String str = "Hello";
        try {
            while(true){
                list.add(str);
                str =str + str;
                i++;
            }
        } catch (Throwable e) {
            e.printStackTrace();
            System.out.println(i);
        }
    }
}
```

**2.2、分析：**

运行程序，会抛出：`java.lang.OutOfMemoryError: Java heap space`异常。

**2.3、JVM参数：**

> <u>可以在`Edit Configurations`中的`VM options`设置设置</u>。

```scss
// 来划分栈内存大小, 如: -Xms8m
-Xms
```



## 3、<span style="color:brown">Method Area内存溢出：</span>

**3.1、溢出区域：**

|  JDK版本   |        区域        |
| :--------: | :----------------: |
| 小于JDK1.8 | **永久代**内存溢出 |
| 大于JDK1.8 | **元空间**内存溢出 |

**3.2、抛出异常：**

* 演示永久代内存溢出

  ```scss
  * java.lang.OutOfMemoryError: PermGen space
  * -XX:NaxPermSize=8m
  ```

* 演示元空间内存溢出

  ```scss
  * java.lang.0utOfMemoryError: Metaspace
  * -XX:MaxNetaspaceSize=8m
  ```



## 4、<span style="color:brown">诊断工具：</span>

>  在控制台找到`Terminal按钮`，之后使用<u>*cd命令*</u>进入到out目录下对应程序的包

|      名称      |                     功能                      |         使用         |
| :------------: | :-------------------------------------------: | :------------------: |
|   javap工具    |         查询已编译好的字节码文件信息          | `javap -v xxx.class` |
|    jps工具     |         查询当前系统中有哪些Java进程          |        `jps`         |
|    jmap工具    |           查看**堆内存**的占用情况            |   `jmap -heap pid`   |
|  jconsole工具  | <u>有图形界面、可连续监测</u>的多功能监测工具 |      `jconsole`      |
| jvirsualvm工具 |                 JVM可视化工具                 |     `jvirsualvm`     |