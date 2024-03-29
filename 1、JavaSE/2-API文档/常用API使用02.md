## 1、<span style='color:brown'>Random类：</span>java.util

**1.1、格式：**

> <font color="#0099ff">Random类用来生成随机数</font>

<u>`Random r = new Random()`</u>

**1.2、演示：**

1. 获取一个随机数int数字（范围是int的所有范围，有正负两种）：`int num = r.nextInt( );`

   ```java
   public class Demo01 {
       public static void main(String[] args) {
           Random r = new Random();
           int num = r.nextInt();
           System.out.println("随机数是:"+num);
       }
   }
   ```

2. 获取一个随机数int数字（参数代表了范围，左闭右开区间）：`int num = r.nextInt(3);`

   > 实际含义是：[0,3)也就是0~2

   ```java
   public class Demo02 {
       public static void main(String[] args) {
           Random r = new Random();
           for (int i = 0; i < 100; i++) {
               int num = r.nextInt(10);
               System.out.print(num+" ");
           }
   
       }
   } 
   ```

3. 根据变量n，获取范围在[1,n]的随机数

   > int result = r.nextInt(n)----->[0,n)即0到n-1，而int result = <font color="#0099ff">r.nextInt(n)+1</font>就变成了[1,n+1)

   ```java
   public class Demo03 {
       public static void main(String[] args) {
           Random r = new Random();
           int n=5;
           for (int i = 0; i < 50; i++) {//循环次数
               int num = r.nextInt(n)+1;//由[0,n)到[1,n]
               System.out.println(num);
               
           }
       }
   }
   ```

**1.3、猜数字游戏：**

```java
public class Demo03 {
    public static void main(String[] args) {
        Random r = new Random();
        //设置随机数变化范围
        int randomNum = r.nextInt(100)+1;
        //输入参数操作
        Scanner sc = new Scanner(System.in);
        int count=1;
        for (int i = 0; i < 10; i++) {
            System.out.println("第"+count+"次测试!!");
            System.out.println("请输入数字:");
            int gussNum = sc.nextInt();
            if(gussNum>randomNum){
                System.out.println("So big,please again!!");
            }else if(gussNum<randomNum){
                System.out.println("So small,please again!!");
            }else{
                System.out.println("Good!!");
                break;
            }
            count++;
        }
        System.out.println("Game Over!!!");
    }
}
```



## 2、<span style='color:brown'>ArrayList集合：</span>java.util

**2.1、引例：**定义一个数组，存储3个Person对象

```java
public class Person {
    private String name;
    private int age;
    // constructor、getter and setter
}
```

```java
public class Demo01 {
    public static void main(String[] args) {
        //定义一个Person类型的数组
        Person[] array = new Person[3];
        //定义三个Person对象
        Person one = new Person("Java", 120);
        Person two = new Person("Python", 100);
        Person three = new Person("C++", 90);
        //将三个对象的地址值赋值给数组元素
        array[0]=one;
        array[1]=two;
        array[2]=three;
        //因为赋值是地址值，所以打印数组元素，这些元素表示为地址值
        System.out.println(array[0]); //package07.Person@1b6d3586
        System.out.println(array[1]); //package07.Person@4554617c
        System.out.println(array[2]); //package07.Person@74a14482
		//调取数组中的数据
        System.out.println(array[0].getName()); //Java
        System.out.println(array[1].getName()); //Python
        System.out.println(array[2].getName()); //C++
        System.out.println(array[0].getAge()); //120
        System.out.println(array[1].getAge()); //100
        System.out.println(array[2].getAge()); //90
    }
}
```

**2.2、集合的概述：**

> <span style='color:orange'>**数组一旦定义之后，长度不能改变；集合的长度在集合之后能够进一步修改**</span>

- <span style='color:green'>**API文档查询：class ArrayList<E>，对于ArrayList来说，有一个<E>表示泛型**</span>；

- <span style='color:red'>**泛型：储存在集合中的元素，应该统一的数据类型**</span>；

- <span style='color:blue'>泛型只能是**引用类型/包装类**，不能是<u>基本数据类型</u></span>；

- <span style='color:red'>**从jdk1.7开始，右侧<>中可以不写内容，但<>本身要写**</span>；

```java
ArrayList<String > list = new ArrayList<String>();
或者
ArrayList<String > list = new ArrayList<>();
```

- <span style='color:red'>**对于ArrayList集合来说，直接打印得到的不是地址值，而是内容。如果内容为空，则得到[  ]**</span>;

```java
public class Demo02 {
    public static void main(String [] args){
        ArrayList<String > list = new ArrayList<String>();
        System.out.println(list);
    }
}
```

**2.3、集合常用方法：**

> ***注意：索引是从0开始***。

- `bolean add(E e)`：
  - 向集合中添加一些数据,参数类型与泛型一致,它的返回值表示添加是否成功；
- `void add(int index, E e)`：
  - 在<u>列表指定位置</u>添加元素；
- `E set(int index, E e)`：
  - 将指定位置的值，修改为指定值，返回被修改前的元素值；
- `E get(int index)`：
- 根据索引从集合中读取元素；
  - 返回值为：对应索引的元素；
- `E remove(int index)`：
  - 根据索引从集合中删除元素；
  - 返回值为被删除的元素；
- `int size( )`：
  - 获取集合的尺寸长度；
  - 返回值是集合中包含元素的个数；
  

**2.4、遍历集合：**

> 注意：<span style='color:red'>**System.out.println(list)虽然可以直接把整个集合打印出来，并不是遍历集合**</span>。

`方法一：`使用foreach方式（推荐）

```java
public class Demo04 {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        list.add(10);
        list.add(20);
        list.add(30);
        list.add(40);
        System.out.println(list);//[10, 20, 30, 40]
        for (Integer num:list) {
            System.out.print(num+" ");//10 20 30 40 
        }
        System.out.println();
    }
}
```

`方法二：`使用ArrayList集合中的size()与get()方法

```java
public class Demo04 {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        list.add(12);
        list.add(15);
        list.add(56);
        //直接打印list
        System.out.println(list);//[12, 15, 56]
        //使用索引查找，遍历数组
        for (int i = 0; i < list.size(); i++) {
            System.out.print(list.get(i)+" ");//12 15 56 
        }
        System.out.println();
    }
}
```

`方法三：`Iterator<E>迭代器方式遍历

> Iterator<E>迭代器是一个用于遍历集合的接口

```java
public class Demo02 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Java");
        list.add("PHP");
        list.add("C#");
        list.add("C++");
        list.add("Python");
        Iterator<String> it = list.iterator();
        while(it.hasNext()){
            String next = it.next();
            System.out.print(next+" ");
        }
        System.out.println();
    }
}
```

