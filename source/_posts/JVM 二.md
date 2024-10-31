---
title: 双亲委派机制
date: 2024-06-01 01:21:46
---
#### 双亲委派机制

由于java虚拟机中有多个类加载器，双亲委派机制的核心就是解决一个类到底由谁加载的问题

##### 一、双亲委派机制的作用
1. 保证类加载的安全性：通过双亲委派机制避免恶意代码替换JDK中的核心类库，比如java.lang.String
2. 避免重复加载：双亲委派机制可以避免同一个类被多次加载

双亲委派机制指的是：当一个类加载器接受到加载类的任务时，会自底向上查找是否加载过，再由顶向下进行加载。

先向上查找，（顺序为 应用程序类加载器 扩展类加载器 启动类加载器）如果该类已经加载过，就直接返回Class对象，加载过程结束

如果所有的父类加载器都无法加载该类，则由当前类加载器自己尝试加载，所以看上去是自顶向下尝试加载。

因此可以推断出， 如果自己定义一个java.lang.String 类，并不会被类加载器加载，因为启动类加载器会加载底层的 java.lang.String, 加载之后就会结束过程
<font color="#ffff00">也就是说 ，双亲委派机制会防止核心类被自定义类覆盖</font>

在Java中如何使用代码的方式去主动加载一个类呢？
方式一：用Class.forName方法，使用当前类的类加载器去加载指定的类
方式二：获取到类加载器，通过类加载器的loadClass方法指定某个类加载器的加载

因此 双亲委派机制是：
1. 当一个类加载器去加载某个类的时候，会自底向上查找是否加载过，如果加载过就直接返回，如果一直到最顶层的类加载器都没有加载，再由顶向下进行加载
2. 应用类加载器的父类加载器是扩展类加载器，扩展类加载器的父类加载器是启动类加载器
3. 有两个优点，第一是避免恶意代码替换JDK中的核心类库，确保核心类库的完整性和安全性。第二是避免一个类重复地被加载


##### 二、打破双亲委派机制
###### 1 自定义类加载器

一个Tomcat程序中是可以运行多个Web应用的，如果这两个应用中出现了相同限定名的类，比如Servlet类，Tomcat要保证这两个类都能加载并且他们应该是不同的类
如果不打破双亲委派机制，当应用类加载器加载Web应用1中的MyServlet之后，Web应用2中相同限定名的MyServlet类就无法被加载了

ClassLoader包含了四个核心方法

``` java
public Class<?> loadClass(String name) 是类加载的入口，提供了双亲委派机制，内部会调用findClass
```

```java
protected Class<?> findClass(String name) 由类加载器子类实现，获取二进制数据调用defineClass ，比如URLClassLoader会根据文件路径去获取类文件中的二进制数据
```

``` java
protected final Class<?> defineClass(String name, byte[] b, int off, int len) 做一些类名的校验，然后调用虚拟机底层的方法将字节码信息加载到虚拟机内存中
```

``` java 
// 双亲委派机制核心代码
// parent 等于null 说明父类加载器是启动类加载器，直接调用findBootstrapClassorNull
// 否则调用父类加载器的加载方法
if (parent != null) {  
c = parent.loadClass(name, false);  
} else {  
c = findBootstrapClassOrNull(name);  
}

// 如果发现父类加载器无法加载对应的类，只能自己去加载
if (c == null) {  
// If still not found, then invoke findClass in order  
// to find the class.  
c = findClass(name);  
}

```

<font color="#c0504d">问题一 自定义类加载器父类为什么是AppClassLoader</font>
答：
``` java
// 在构造时，可以通过parent参数选择父类加载器
private ClassLoader(Void unused, ClassLoader parent) {  
this.parent = parent;  
if (ParallelLoaders.isRegistered(this.getClass())) {  
parallelLockMap = new ConcurrentHashMap<>();  
package2certs = new ConcurrentHashMap<>();  
assertionLock = new Object();  
} else {  
// no finer-grained lock; lock on the classloader instance  
parallelLockMap = null;  
package2certs = new Hashtable<>();  
assertionLock = this;  
}  
}
```

``` java
protected ClassLoader() {  
this(checkCreateClassLoader(), getSystemClassLoader());  
}
```

getSystemClassLoader() 默认的父类加载器是AppClassLoader
这个方法调用了Launcher()构造器，这个构造器会返回 AppClassLoader

<font color="#c0504d">问题二 两个自定义类加载器加载相同限定的类名，会冲突吗？</font>

答：**不会冲突**，在同一个java虚拟机中，只有 <font color="#fdeada">相同类加载器 + 相同的类限定名</font> 才会被认为是同一个类
###### 2 线程上下文类加载器

举例：JDBC
JDBC中使用了DriverManager来管理项目中引入的不同数据库的驱动，比如mysql驱动，oracle驱动
DriverManager 属于 rt.jar， 是启动类加载器加载的。而用户jar包中的驱动需要由应用类加载器加载，这就违反了双亲委派机制

<font color="#d83931">问题一 DriverManager怎么知道jar包中要加载的驱动在哪？</font>
答：SPI机制
SPI的工作原理：
1 在ClassPath路径下的META-INF/services 文件夹中，以接口的全限定名来命名文件名，对应的文件里面写该接口的实现。 比如Mysql里就实现了java.sql.Driver， 并且将Driver对象 注册到DriverManager中
2 ServiceLoader加载实现类 
``` java
// 将要加载的接口进行加载，获取Driver对象
ServiceLoader<java.sql.Driver> loadedDrivers = ServiceLoader.load(java.sql.Driver.class)

```

<font color="#d83931">问题二 SPI中是如何获取到应用程序类加载器的？</font>
SPI中使用了线程上下文保存的类加载器进行类加载，这个类加载器一般是应用程序类加载器

[[JDBC线程上下文类加载器加载驱动类 2]]

<font color="#d83931">问题三 该案例真的打破了双亲委派机制吗？</font>

周志明 《深入理解Java虚拟机》中表示 是打破了双亲委派机制。
但是分析来看 JDBC只是在DriverManager加载完成后，通过初始化阶段触发了驱动类的加载，该加载仍然是遵循了双亲委派机制

###### 3 Osgi框架的类加载器

OSGi还使用类加载器实现了热部署的功能
热部署指的是在服务部停止的情况下，动态地更新字节码文件到内存中

如果代码上线后，发现有小bug，但是用户着急使用，如果新打包在发布需要一个多小时的时间，可以用arthas去解决该类问题
arthas热加载步骤
1. <font color="#548dd4">在出问题的服务器上部署arthas 并且启动</font>
2. <font color="#548dd4">jad --source-only 类全限定名 > 目录/文件名.java</font>
3. <font color="#548dd4">mc -c 类加载器的hashcode 目录/文件名.java -d 输出目录 （通过sc -d 全类名 去找到hashcode）</font>
4. <font color="#548dd4">retransform class文件所在目录/xxx.class</font>
<font color="#d83931">注意事项</font>
<font color="#d83931">1 程序重启之后，字节码文件会回复， 除非将class文件放入jar包中进行更新</font>
<font color="#d83931">2 使用retransform 不能添加方法或者字段，也不能更新正在执行中的方法</font>

