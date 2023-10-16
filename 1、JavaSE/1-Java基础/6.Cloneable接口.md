## 1、<span style="color:brown">**引言：**</span>

**1.1、概述：**

Cloneable接口是Java开发中常用的一个接口， 它的作用是**使一个类的实例能够将自身拷贝到另一个新的实例**中，<u>注意，这里所说的“拷贝”拷的是对象实例，而不是类的定义</u>，进一步说，**拷贝的是一个类的实例中各字段的值。**

**1.2、原因：**

在开发过程中，拷贝实例是常见的一种操作，如果一个类中的字段较多，而我们又采用在客户端中逐字段复制的方法进行拷贝操作的话，将不可避免的造成客户端代码繁杂冗长，而且也无法对类中的私有成员进行复制。

而<u>*如果让需要具备拷贝功能的类实现Cloneable接口，并重写clone()方法，就可以通过调用clone()方法的方式简洁地实现实例拷贝功能*</u>。

**1.3、实现对象克隆的方式：**

- 实现Cloneable接口，并重写Object类中的clone()方法

- 实现 Serializable 接口，通过对象的序列化和反序列化实现克隆



## 2、<span style="color:brown">**源码分析：**</span>

**2.1、Coloneable接口源码：**

```java
/*
JDK1.8的Cloneable接口源代码
*/
public interface Cloneable {
}
```

**2.2、Cloneable接口没有定义任何接口方法的原因？**

 在Java中，**Object类已经将clone()方法定义为所有类都应该具有的基本功能**，只是将该方法声明为了protected类型。

<u>该方法定义了*逐字段拷贝实例*的操作！！！</u>

```java
/**
 *	 Object类中clone()是一个native本地方法, 因此没有实现体
 *	 而且在拷贝字段时, 除了Object类的字段外, 其子类的新字段也将被拷贝到新的实例中
*/
protected native Object clone() throws CloneNotSupportedException;
```

**2.2、 为什么还是需要让想具备实力拷贝功能的类实现Cloneable接口呢？**

在Object类中虽然已经有了一个定义实例拷贝操作的方法，但<span style="color:red">**实现Cloneable接口是为了起到一种标识的作用**</span>，<u>表明实现它的类具备了实例拷贝功能</u>。

**2.3、如果不实现Cloneable接口会出现什么情况？**

在Cloneable接口的官方javadoc文档中有这样一段话：

```java
   "Invoking Object's clone method on an instance that does not implement the Cloneable interface results in the exception CloneNotSupportedException being thrown. JDK1.8"
   
   "在不实现可克隆接口的实例上调用对象的克隆方法会导致引发异常CloneNotSupportedException。JDK1.8 "
```

也就是说，**要想使一个类具备拷贝实例的功能，那么除了要重写Object类的clone()方法外，还必须要实现Cloneable接口**。