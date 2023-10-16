## 1、<span style='color:brown'>Scanner类：</span>java.util

**1.1、Scanner格式：**

`Scanner 对象名 = new Scanner(System.in)`

```java
public class Demo01_Scanner {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int num=scanner.nextInt();
        int sum=0;
        for (int i = 0; i < num; i++) {
            sum+=i*2+1;
        }
        scanner.close();
        System.out.println(sum);
    }
}
```

**1.2、Scanner详解：**

> <span style='color:red'>next()不能读取带有空白的单词，而nextLine()则可以</span>

- next()：读取输入的下一个单词；

- hasNext()：检测输入中是否还有其他单词；

- nextLine()：读取下一行内容；

- hasNextLine()：检测是否还有下一个表示字符串的序列；


<span style='color:green'>后续的nextInt()、nextDouble()、nextByte()、hasNextInt()、hasNextDouble()、hasNextByte()等类似于上述作用及功能。</span>

```java
public class Demo02_hasNexts {
    public static void main(String[] args) {
        //创建一个扫描器对象，用于接收键盘数据
        Scanner scanner = new Scanner(System.in);
        //判断是否有数据输入
        if(scanner.hasNextLine()){
            String str=scanner.nextLine();
            System.out.println();
        }
        //结束运行，释放空间
        scanner.close();
    }
}
```

**1.3、处理换行符：**

​	<span style='color:brown'>**在读取完整数或其他类型的输入后，通常会留下一个换行符在输入缓冲区中**</span>，这会造成后续内容输入为null值。为了避免这个问题，可以在读取完整数后使用`nextLine()`来消耗掉换行符！！！

```java
public class Main{
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int num = scanner.nextInt();
        scanner.nextLine();
        int line = scanner.nextInt();
        scanner.nextLine();
        HashMap<Integer, String> map = new HashMap<>();
        for (int i = 0; i < line; i++) {
            map.put(i,scanner.nextLine());
        }
        String[][] str = new String[line][1];
        Set<Map.Entry<Integer, String>> set = map.entrySet();
        for (Map.Entry<Integer, String> entry:set) {
            String value = entry.getValue();
            Integer key = entry.getKey();
            System.out.println(key+value);
        }
    }
}
```

**1.4、关闭Scanner对象：**

<u>`对象名.close()`</u>

```java
//输入三个数，输出其中最大的值
public class Demo03 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int []a = new int[3];
        for (int i = 0; i < a.length; i++) {
            a[i] = sc.nextInt();
        }
        int max=a[0];
        for (int i = 0; i < a.length; i++) {
            if(max<a[i]){
                max=a[i];
            }
        }
        System.out.println(max);
        sc.close();
    }
}
```



## 2、<span style='color:brown'>匿名对象</span>

**2.1、格式：**

> <span style='color:red'>**匿名对象只能使用唯一的一次，下次使用就是另外一个新的对象**</span>

<u>`new 类名称()`</u>

**2.2、演示：**

```java
public class Person {
    String name;
    public void showName(String name){
        System.out.println("你好，"+name+"!我叫"+this.name);
    }
}
```

```java
public class Demo04 {
    public static void main(String[] args) {
        Person one = new Person();
        one.name="java";			//Java
        one.showName("李明");        //你好李明！我叫java
        System.out.println("======匿名对象======");
        new Person().name="Python";		//Python
        new Person().showName("咸鱼");        //你好咸鱼！我叫null
    }
}
```

**2.3、匿名对象作为方法的参数和返回值：**

```java
public class Demo05 {
    public static void main(String[] args) {
        //普通方式
//        Scanner sc = new Scanner(System.in);
//        int num=sc.nextInt();

        //匿名对象方式
//        int num = new Scanner(System.in).nextInt();
//        System.out.println("输入的是:"+num);

        //作为方法参数
//        Scanner sc = new Scanner(System.in);
//        methodParam(sc);

        //使用匿名对象写入参数
        methodParam(new Scanner(System.in));
    }
    public static void methodParam(Scanner sc){
        int num=sc.nextInt();
        System.out.println("输入是:"+num);
    }
}
```

```java
public class Demo06 {
    public static void main(String[] args) {
        Scanner sc = methodParam();
        int num=sc.nextInt();
        System.out.println("输入是:"+num);
    }
    public static Scanner methodParam(){
        return new Scanner(System.in);
    }
}
```



