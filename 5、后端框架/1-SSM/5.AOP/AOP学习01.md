# AOP学习01

## 1、<span style="color:brown">AOP简介：</span>

**1.1、什么是AOP？**

![image-20220915192102837](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/AOP%E7%9A%84%E6%A6%82%E5%BF%B5.png)

**1.2、AOP的优势及其作用：**

- 作用：

  ```apl
  在程序运行期间, 在不修改源码的情况下对方法进行'功能增强'
  ```

- 优势：

  ```apl
  1. 减少重复代码;
  2. 提高开发效率;
  3.便于维护;
  ```

**1.3、AOP的底层实现：**

实际上，AOP的底层是通过**Spring提供的动态代理技术实现的**。

在运行期间，Spring通过动态代理技术`动态的生成代理对象`。

*代理对象方法*执行时，<u>进行增强功能的介入，再去调用目标对象的方法</u>，从而完成功能的增强。

---

<span style="color:green">**常用的动态代理技术：**</span>

```apl
1. cglib代理对象是'目标对象的子类';
2. jdk代理对象和目标对象都'实现了目标接口';
3. jdk代理对象还继承了Proxy;
```

- JDK代理：基于接口的动态代理技术；
- cglib代理：基于父类的动态代理技术；

![image-20220915200013469](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/AOP%E7%9A%84%E5%8A%A8%E6%80%81%E7%94%9F%E6%88%90%E6%8A%80%E6%9C%AF.png)



## 2、<span style="color:brown">动态代理技术底层实现原理：</span>

**2.1、JDK动态代理技术：**

- 接口：

  ```java
  public interface Target {
      public abstract void save();
  }
  ```

- 目标类：

  ```java
  public class TargetImpl implements Target{
      @Override
      public void save() {
          System.out.println("saveRunning...");
      }
  }
  ```

- 功能增强类：

  ```java
  public class Advice {
      public void beforeRunning(){
          System.out.println("前置增强...");
      }
      public void afterRunning(){
          System.out.println("后置增强...");
      }
  }
  ```

- 测试类：

  ```java
  public class ProxyText {
      public static void main(String []args){
          //目标对象
          final TargetImpl target = new TargetImpl();
          //增强对象
          Advice advice = new Advice();
          //返回值: 动态生成的代理对象
          Target proxy = (Target) Proxy.newProxyInstance(
                  target.getClass().getClassLoader(),//目标对象的类加载器
                  target.getClass().getInterfaces(),//目标对象相同的接口字节码对象数组
                  new InvocationHandler(){
                      //调用目标对象的任何方法, 实质: 都是执行invoke方法
                      @Override
                      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                          advice.beforeRunning();//前置增强
                          Object invoke = method.invoke(target, args);//执行目标方法
                          advice.afterRunning();//后置增强
                          return invoke;
                      }
                  });
          //调用代理对象的方法
          proxy.save();
      }
  }
  ```
  
- 测试结果：

  ```apl
  前置增强...
  saveRunning...
  后置增强...
  ```

**2.2、cglib动态代理技术：**

- 目标类：

  ```java
  public class TargetImpl {
      public void save() {
          System.out.println("saveRunning...");
      }
  }
  
  ```

- 功能增强类：

  ```java
  public class Advice {
      public void beforeRunning(){
          System.out.println("前置增强...");
      }
      public void afterRunning(){
          System.out.println("后置增强...");
      }
  }
  ```

- 测试类：

  ```java
  public class ProxyText {
      public static void main(String []args){
          //目标对象
          final TargetImpl target = new TargetImpl();
          //增强对象
          Advice advice = new Advice();
  
          /*返回值: 动态生成的代理对象*/
          //1. 设置增强器
          Enhancer enhancer = new Enhancer();
          //2. 设置父类(目标)
          enhancer.setSuperclass(target.getClass());
          //3. 设置回调
          enhancer.setCallback(new MethodInterceptor() {
              @Override
              public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                  advice.beforeRunning();//执行前置
                  Object invoke = method.invoke(target, args);//执行目标
                  advice.afterRunning();//执行后置
                  return invoke;
              }
          });
          //4. 创建代理对象
          TargetImpl proxy = (TargetImpl) enhancer.create();
          proxy.save();
      }
  }
  ```