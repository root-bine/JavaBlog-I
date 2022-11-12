# Servlet进阶

## 1、<span style="color:brown">生命周期方法：</span>

**1.1、Servlet接口方法的解析：**

在实现Servlet接口，并重写覆盖全部的抽象方法之后，解析如下：

1. `void init(ServletConfig config)`：初始化方法
   - 在Servlet**被创建时**，执行；
   - <span style="color:green">**仅执行一次**</span>；
2. `void service(ServletRequest servletRequest, ServletResponse servletResponse)`：提供服务方法
   - 每一次Servlet**被访问时**，执行；
   - <span style="color:green">**可以执行多次**</span>；
3. `void destroy()`：销毁方法
   - 在Servlet**被正常关闭时**，执行；
   - <span style="color:green">**仅执行一次**</span>；
4. `ServletConfig getServletConfig()`：获取ServletConfig对象
   - ServletConfig对象，就是**Servlet配置对象**；
5. `String getServletInfo()`：获取Servlet的一些信息，如：作者、版本、作者等等

**1.2、方法演示：**

第一次执行：

```apl
init...
service...
```

刷新页面：

```apl
init...
service...
service...
```

再次刷新页面

```apl
init...
service...
service...
service...
```

关闭Tomcat：

```apl
destroy...
```



## 2、<span style="color:brown">生命周期详解：</span>

**2.1、生命周期的阶段：**

```apl
加载类 —> 实例化(为对象分配空间) —> 初始化(为对象的属性赋值) —> 请求响应(服务阶段) —> 销毁
```

**2.1、生命周期的过程：**

```apl
1. 被创建
2. 被访问，即：提供服务
3. 被销毁
4. 'JVM的垃圾回收器'进行垃圾回收
```

**2.2、Servlet被创建的时机：**

> 标记容器是否在启动的时候就加载这个servlet对象

```xml
<servlet>
	<load-on-startup>......</load-on-startup>
</servlet>
```

- 当值为0或者大于0时，表示容器在应用启动时就加载这个servlet；

- 当是一个负数时或者没有指定时，则指示容器在该servlet被选择时才加载：
  - **正数的值越小，启动该servlet的优先级越高**；

---

init方法，只能执行一次，说明：

```apl
Servlet在内存中, 只存在一个对象, 即: Servlet是单例的
```

但是，当多个用户同时访问Servlet时，可能会出现线程安全的问题。解决方式：

```apl
1. 尽量不要在Serclet接口的实现类中, 定义成员变量;
2. 如果定义了成员变量, 那就只'获取这个变量, 不对其进行数据操作';
```



