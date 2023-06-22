## <span style="color:purple">**自定义枚举类的上层父类是Java.lang.Object**</span>，<span style="color:violet">**而enum关键词的使用的上层父类是Java.lang.Enum**</span>！！！！！！

## 1、自定义枚举类：

```java
public class Season {
      private  String seasonName;
      private  String seasonDesc;
      //构造器私有化
      private Season(String seasonName, String seasonDesc) {
          this.seasonName = seasonName;
          this.seasonDesc = seasonDesc;
      }
      //创建对象
      public static final Season spring = new Season("春天","春暖花开");
      public static final Season summer = new Season("夏天","夏日炎炎");
      public static final Season autumn = new Season("秋天","硕果累累");
      public static final Season winter = new Season("冬天","白雪皑皑");
      //额外因素
      public String getSeasonName() {
          return seasonName;
      }
      public String getSeasonDesc() {
          return seasonDesc;
      }
      //元素输出
      //调用的是Object的toString（）方法
      @Override
      public String toString() {
          return "Season{" +
                  "seasonName='" + seasonName + '\'' +
                  ", seasonDesc='" + seasonDesc + '\'' +
                  '}';
      }
  }
```

```java
public class Demo01 {
    public static void main(String[] args) {
        Season one = Season.spring;
        System.out.println(one);
        System.out.println(one.getSeasonDesc());
        System.out.println(one.getSeasonName());
    }
}
```



## 2、枚举类：

**2.1、步骤：**

1. <span style="color:red">**将class用enum取代**</span>；
2. <span style="color:red">**enum枚举类要求对象（常量）必须放在最开始的位置**</span>；
3. 多个常量连接之间是使用**逗号**连接，最后用**分号**结束；

```java
public enum Season {
    SPRING ("春天","春暖花开"),
    SUMMER ("夏天","夏日炎炎"),
    AUTUMN ("秋天","硕果累累"),
    WINTER ("冬天","白雪皑皑");
    private final String seasonName;
    private final String seasonDesc;
    //构造器私有化
    private Season(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }
    //额外因素
    public String getSeasonName() {
        return seasonName;
    }
    public String getSeasonDesc() {
        return seasonDesc;
    }
    //元素输出
    //此处的toString方法可以写出，但是本身父类Enum类中就具备了toString（）方法
    /**
    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDesc='" + seasonDesc + '\'' +
                '}';
    }*/
}
```

```java
public class Demo01 {
    public static void main(String[] args) {
        Season spring = Season.SPRING;
        System.out.println(spring);
        System.out.println(spring.getSeasonName());
        System.out.println(spring.getSeasonDesc());
    }
}
```

**2.2、enum类的简化使用：**

<span style="color:orange">**源码中经常存在此类代码解析，因为这个枚举类底层没有属性，构造器，toString( )，get方法都删除不写了**</span>！！！

```java
public enum Season{
    SPRING,
    SUMMER,
    AUTUMN,
    WINTER;
}
```

**2.3、Enum类中的方法：**(<span style="color:orange">类名直接调用方法</span>)

1. `String toString()`
- <span style="color:red">**获取的是对象的名字**</span>；
2. `T[] values()`
   - <span style="color:red">**返回一个枚举类对象数组**</span>；
   - <span style="color:red">**主要作用是将枚举类中所有内容装入一个枚举数组中**</span>；
3. `T valuesOf(String name)` 
   - <span style="color:red">**通过对象名获取这个枚举对象**</span>；
   - <span style="color:red">**主要作用是将枚举对象装入一个新的对象**</span>；
     - <span style="color:blue">**对象的名字必须传正确，否则会抛出异常**</span>；

```java
public class Demo02 {
    public static void main(String[] args) {
        System.out.println("--------toString方法---------");
        Season autumn = Season.AUTUMN;
        System.out.println(autumn/*.toString()*/);
        System.out.println("--------values方法---------");
        Season[] values = Season.values();
        for(Season s:values){
            System.out.println(s);
        }
        System.out.println("--------valuesOf方法---------");
        Season autumn1 = Season.valueOf("AUTUMN");
        System.out.println(autumn1);
    }
}
```

```java
public enum Season {
         SPRING ("春天","春暖花开"),
         SUMMER ("夏天","夏日炎炎"),
         AUTUMN ("秋天","硕果累累"),
         WINTER ("冬天","白雪皑皑");
         private final String seasonName;
         private final String seasonDesc;
         //构造器私有化
         private Season(String seasonName, String seasonDesc) {
             this.seasonName = seasonName;
             this.seasonDesc = seasonDesc;
         }
         //额外因素
         public String getSeasonName() {
             return seasonName;
         }
         public String getSeasonDesc() {
             return seasonDesc;
         }
     }
```

**2.4、enum枚举类的接口实现：**

1. <span style="color:orange">**枚举类实现接口，要重写对象中的show方法**</span>；

2. <span style="color:red">**所有枚举对象调用show方法的结果是一致的，改进如下**</span>：

   **范例1**：

   ```java
   public interface TestInterface {
       void show();
   }
   ```
   
   ```java
   public enum Seasons implements TestInterface{
       SPRING,
       SUMMER,
       AUTUMN,
       WINTER;
       @Override
       public void show() {
           System.out.println("这是Seasons。。。。。。");
       }
   }
   ```
   
   ```java
   public class Demo03 {
       public static void main(String[] args) {
           Seasons spring = Seasons.SPRING;
           spring.show();
           Seasons winter = Seasons.WINTER;
           winter.show();
           Seasons summer = Seasons.SUMMER;
           summer.show();
           Seasons autumn = Seasons.AUTUMN;
           autumn.show();
       }
   }
   ```

   **范例2**：

   ```java
   public class Demo03 {
       public static void main(String[] args) {
           Seasons spring = Seasons.SPRING;
           spring.show();
           Seasons winter = Seasons.WINTER;
           winter.show();
           Seasons summer = Seasons.SUMMER;
           summer.show();
           Seasons autumn = Seasons.AUTUMN;
           autumn.show();
       }
   }
   ```

   ```java
   public enum Seasons implements TestInterface{
       SPRING{
           @Override
           public void show() {
               System.out.println("这是春天！！！");
           }
       },
       SUMMER{
           @Override
           public void show() {
               System.out.println("这是夏天！！！");
           }
       },
       AUTUMN{
           @Override
           public void show() {
               System.out.println("这是秋天！！！");
           }
       },
       WINTER{
           @Override
           public void show() {
               System.out.println("这是冬天！！！");
           }
       };
   }
   ```

   ```java
   public interface TestInterface {
       void show();
   }
   ```

