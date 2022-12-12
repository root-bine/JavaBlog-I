## 1、<span style="color:brown">类加载器总结：</span>

**1.1、ClassLoader类别：**

JVM 中内置了三个重要的 ClassLoader：

1. **BootstrapClassLoader(启动类加载器)** ：最顶层的加载类，由 C++实现，负责加载 `%JAVA_HOME%/lib`目录下的 jar 包和类或者被 `-Xbootclasspath`参数指定的路径中的所有类。
2. **ExtensionClassLoader(扩展类加载器)** ：主要负责加载 `%JRE_HOME%/lib/ext` 目录下的 jar 包和类，或被 `java.ext.dirs` 系统变量所指定的路径下的 jar 包。
3. **AppClassLoader(应用程序类加载器)** ：面向我们用户的加载器，负责加载当前应用 classpath 下的所有 jar 包和类。

<span style="color:green">除了 BootstrapClassLoader，其他类加载器均由 Java 实现且全部继承自`java.lang.ClassLoader`</span>！！！

**1.2、ClassLoader的作用：**

<u>一个非数组类的加载阶段（**加载阶段获取类的二进制字节流的动作**）</u>是可控性最强的阶段，这一步可以去自定义类加载器去控制字节流的获取方式。数组类型不通过类加载器创建，它由 Java 虚拟机直接创建。

<span style="color:blue">所有的类都由类加载器加载，加载的作用就是将 `.class`文件加载到内存</span>。

**1.3、类加载机制：**

> **缓存机制**，才是导致<u>为什么更改了class后，需要重启JVM才生效</u>的原因

| 类型                 | 解释                                                         |
| -------------------- | ------------------------------------------------------------ |
| 全盘负责             | 当一个类加载器负责加载某个Class时，该Class所依赖和引用的其他Class也由该类加载器负责载入，除非显示使用另一个类加载器来载入 |
| 父类委托（双亲委派） | 先让父加载器试图加载该Class，只有在父加载器无法加载时该类加载器才会尝试从自己的类路径中加载该类 |
| 缓存机制             | 将已经加载的class缓存起来，当程序中需要使用某个Class时，类加载器先从缓存区中搜寻该Class。只有当缓存中不存在该Class时，系统才会读取该类的二进制数据，并将其转换为Class对象，存入缓存中 |



## 2、<span style="color:brown">双亲委派模型：</span>

**2.1、介绍：**

每一个类都有一个对应它的类加载器，系统中的 ClassLoader 在协同工作的时候会默认使用 **<u>双亲委派模型</u>** ：

```apl
在类加载的时候, 系统会首先判断当前类是否被加载过:
		# 已被加载的类会直接返回
		# 未被加载的类才会尝试加载
```

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/ClassLoader01.png" alt="image-20221212171100923" style="zoom: 67%;" />

加载的时候，首先会把<u>*该请求*</u>委派给**父类加载器的 `loadClass()` 处理**，情况分类：

- 当父类加载器无法处理时，才由自己来处理；
- 当父类加载器为 null 时，会使用启动类加载器 `BootstrapClassLoader` 作为父类加载器；

因此，<span style="color:red">所有的请求最终都会传送到顶层的启动类加载器 `BootstrapClassLoader` 中</span>！！！

**2.2、验证每个类加载都有一个父类加载器？**

[#](#Code)Code

```java
public class Demo {
    public static void main(String[] args) {
        System.out.println("Demo's ClassLoader is " + ClassLoaderDemo.class.getClassLoader());
        System.out.println("The Parent of Demo's ClassLoader is " + ClassLoaderDemo.class.getClassLoader().getParent());
        System.out.println("The GrandParent of Demo's ClassLoader is " + ClassLoaderDemo.class.getClassLoader().getParent().getParent());
    }
}
```

[#](#Output)Output

```scss
Demo's ClassLoader is sun.misc.Launcher$AppClassLoader@18b4aac2
The Parent of Demo's ClassLoader is sun.misc.Launcher$ExtClassLoader@1b6d3586
The GrandParent of Demo's ClassLoader is null
```

[#](#conclusion)conclusion

`AppClassLoader`的父类加载器为`ExtClassLoader`， `ExtClassLoader`的父类加载器为 null，**null 并不代表`ExtClassLoader`没有父类加载器，而是 `BootstrapClassLoader`** 。

[#](#attention)attention

- 这里的双亲更多地表达的是“父母这一辈”的人而已，并不是说真的有一个 Mother ClassLoader 和一个 Father ClassLoader；
- <u>类加载器之间的父子关系</u>也不是通过继承来体现的，是**由优先级来决定**；

**2.3、双亲委派的好处：**

双亲委派模型保证了 ：1、Java 程序的稳定运行；2、避免类的重复加载；3、保证 Java 的核心 API 不被篡改。

在JVM中，<span style="color:red">区分不同类的方式不仅仅根据类名，相同的类文件被不同的类加载器加载产生的是两个不同的类</span>。因而如果没有使用双亲委派模型，而是每个类加载器加载自己的话就会出现一些问题，比如我们编写一个称为 `java.lang.Object` 类的话，那么程序运行的时候，系统就会出现多个不同的 `Object` 类。

**2.4、双亲委派源码分析：**

```java
private final ClassLoader parent;
protected Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
    synchronized (getClassLoadingLock(name)) {
        // 首先，检查请求的类是否已经被加载过
        Class<?> c = findLoadedClass(name);
        if (c == null) {
            long t0 = System.nanoTime();
            try {
                if (parent != null) {//父加载器不为空，调用父加载器loadClass()方法处理
					c = parent.loadClass(name, false);
                } else {//父加载器为空，使用启动类加载器 BootstrapClassLoader 加载
                    c = findBootstrapClassOrNull(name);
                }
            } catch (ClassNotFoundException e) {
                //抛出异常说明父类加载器无法完成加载请求
            }

            if (c == null) {
                long t1 = System.nanoTime();
                //自己尝试加载
                c = findClass(name);

                // this is the defining class loader; record the stats
                sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                sun.misc.PerfCounter.getFindClasses().increment();
            }
        }
        if (resolve) {
            resolveClass(c);
        }
        return c;
    }
}
```

**loadClass加载方法流程：**

1. 判断此类是否已经加载；
2. 如果父加载器不为null，则使用父加载器进行加载；反之，使用`BootstrapClassLoader`（根加载器）进行加载；
3. 如果前面都没加载成功，则使用findClass方法进行加载；



## 3、<span style="color:brown">自定义类加载器：</span>

**3.1、使用场景：**

| 类别         | 解释                                                         |
| :----------- | :----------------------------------------------------------- |
| 扩展类加载源 | 通常情况下我们写的java类文件存放在classpath下，由应用类加载器AppClassLoader加载。<br>我们可以自定义类加载器从数据库、网络等其他地方加载我们我们的类 |
| 隔离类       | 每一个应用自己的类加载器`WebAppClassLoader`负责加载本身的目录下的class文件，<br>加载不到时再交给`CommonClassLoader`加载，这和双亲委派刚好相反 |
| 防止源码泄露 | 某些情况下，源码是商业机密，不能外泄，这种情况下会进行编译加密。<br>那么在类加载的时候，需要进行解密还原，这种情况下就要自定义类加载器了 |

**3.2、实现方式：**

Java提供了抽象类`java.lang.ClassLoader`，所有用户自定义的类加载器都应该继承`ClassLoader`类。

在自定义`ClassLoader`的子类时候，我们常见的会有两种做法:

● 重写loadClass()方法

```java
protected Class loadClass(String name, boolean resolve)
    # name为类名
    # resove如果为true, 则在加载时解析该类
```

● 重写findClass()方法

```java
protected Class findClass(String name) 
    # 根据指定类名来查找类
```

**3.3、实现方法选取：**

| 类别             | 重写方法              | 解释                                                         |
| ---------------- | --------------------- | ------------------------------------------------------------ |
| 打破双亲委派模型 | 重写整个loadClass方法 | `loadClass()`中封装了双亲委派模型的核心逻辑，<br>如果确实有需求，需要打破这样的机制，那么就需要重写`loadClass()`方法 |
| 使用双亲委派模型 | 仅重写findClass方法   | /                                                            |

**3.4、代码演示：**

```java
public class Hello {
   public void test(String str){
       System.out.println(str);
   }
}

public class MyClassloader extends ClassLoader {
    private byte[] getBytes(String fileName) throws IOException {
        File file = new File(fileName);
        long len = file.length();
        byte[] raw = new byte[(int)len];
        try (FileInputStream fin = new FileInputStream(file)) {
            //一次性读取Class文件的全部二进制数据
            int read = fin.read(raw);
            if (read != len) {
                throw new IOException("无法读取全部文件");
            }
            return raw;
        }
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        Class clazz = null;
        //将包路径的(.)替换为斜线(/)
        String fileStub = name.replace(".", "/");
        String classFileName = fileStub + ".class";
        File classFile = new File(classFileName);

        //如果Class文件存在，系统负责将该文件转换为Class对象
        if (classFile.exists()) {
            try {
                //将Class文件的二进制数据读入数组
                byte[] raw = getBytes(classFileName);
                //调用ClassLoader的defineClass方法将二进制数据转换为Class对象
                clazz = defineClass(name, raw, 0, raw.length);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        //如果clazz为null,表明加载失败，抛出异常
        if (null == clazz) {
            throw new ClassNotFoundException(name);
        }
        return clazz;
    }

    public static void main(String[] args) throws Exception {
        String classPath = "loader.Hello";
        MyClassloader myClassloader = new MyClassloader();
        Class<?> aClass = myClassloader.loadClass(classPath);
        Method main = aClass.getMethod("test", String.class);
        System.out.println(main);
        main.invoke(aClass.newInstance(), "Hello World");
    }
}
//输出结果
//Hello World
```

