---
title: knowledge
date: 2022-11-09 09:23:53
---
#  Spring

Ioc 是 控制反转 是一种 基于容器的对象管理机制。而DI是 依赖注入 是ioc的表现方式。依赖注入指的是 在对象创建时，由容器自动将依赖对象注入到需要依赖的对象中。（想象老韩的di依赖注入，将monsterDao的bean注入到MonsterService中的monsterDao属性）

将bean的配置都会写在xml文件中，spring就会根据 反射机制 ，将xml的配置获取出来 ，再根据xml内的具体配置，将对象注入到ioc中。

bean就是由ioc容器实例化，装配和管理的对象。

bean生命周期
https://chaycao.github.io/2020/02/15/%E5%A6%82%E4%BD%95%E8%AE%B0%E5%BF%86Spring-Bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.html
1创建实例
2属性注入（setter 构造器等）
3初始化 （BeanPostProcessor 有两个方法 before after，可以对初始化前后进行操作）
4使用
5销毁


bean是不是线程安全的
如果bean的状态是prototype 即每次获取都会创建一个新的bean实例，getBean()  两次就会得到不同的bean实例

bean的状态是singleton的话 如果bean是没有成员变量的话 那么就是线程安全的 如果由成员变量，那么在多线程的情况下，如果没有sychronized这种关键字的修饰，那么就是线程不安全的

BeanFactory 和 FactoryBean
BeanFactory 就是IOC的底层容器，负责管理和配置应用中的Bean。 其中一个重要的特性就是延迟初始化，它只会在Bean首次请求后才会实例化改Bean，而不是在容器中启动时就立刻创建所有的Bean



@Autowired 和 @Resource

@Autowired是Spring提供的注解 注入方式是byType 
@Resource是JDK提供的注解 注入方式是byName 但也可以通过name进行配置，并且如果不配置，也会现根据byName去根据属性名去找到对应的bean，byName没找到就会根据ByType去根据属性类型找到对应的bean




三级缓存解决循环依赖
https://mp.weixin.qq.com/s/dSRQBSG42MYNa992PvtnJA




spring有两个id相同的bean会报错吗
分情况
1 再同一个xml文件里是会报错的
2 如果是不同的xml文件，默认会把多个相同id的bean覆盖（后面的覆盖前面的），Spring3.x之后 @Bean声明多个相同名字的bean默认只会注册第一个


事务

1 编程式事务



2 声明式事务


# SpringBoot

约定优于配置

指的是，如果你所期望的配置和默认约定的配置是一致的，那就可以不用做任何配置。约定不符合期待，会用配置的值去替换约定的值。也就是说期望的配置的优先级还是高的。

版本仲裁
根据依赖就近原则：如果在配置文件里自己引用版本，就以自己引用的为准，如果自己没有引用，就以最近的父项目引用的为准

自动配置
只需要项目添加以来，就可以自动创建应用所需要的Bean，比如会自动配置一个servlet容器（如Tomcat），还有相关的Servlet Filter Listener等

自动配置原理：
SPI机制
首先通过@SpringBootApplication注解， 里面由三个注解@ComponentScan @EnableAutoConfiguration @SpringBootConfiguration
@EnableAutoConfiguration 这个注解会通过@Import(AutoConfigurationImportSelector.class) ， 去加载所有的jar包的 META-INFO下面的 spring-factories配置文件（这里用到了SPI机制），也是通过全类名，经过反射 来实例化。

Springboot的启动流程
1 创建Spring应用：先判断是什么项目：web项目还是非web项目
2 加载初始化器和事件监听器：根据spring.factories 文件中的全限定名加载初始化器(SPI)
3 获取启动器

启动Spring应用
4 加载运行监听器
5 初始化环境变量 ，加载命令行启动参数与配置文件，并且初始化一个Environment 以提供后续使用（联想在写项目是根据在yml文件中配置的各个接口的地址，通过Environment获取）
6 初始化上下文（很重要的一步，通过容器初始化器初始化Context， 获取了ConfigurationApplicationContext，在refresh这一步创建了beanFactory并且将bean注入，也创建了webServer并启动）


``` java
public ConfigurableApplicationContext run(String... args) {
   long startTime = System.nanoTime();
   //通过BootstrapRegistryInitializer来initialize默认的DefaultBootstrapContext
   DefaultBootstrapContext bootstrapContext = createBootstrapContext();
   ConfigurableApplicationContext context = null;
   //配置java.awt.headless属性
   configureHeadlessProperty();
   //获取SpringApplicationRunListeners监听器
   SpringApplicationRunListeners listeners = getRunListeners(args);
   //启动SpringApplicationRunListeners监听，表示SpringApplication启动（触发ApplicationStartingEvent事件）
   listeners.starting(bootstrapContext, this.mainApplicationClass);
   try {
      //创建ApplicationArguments对象，封装了args参数
      ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
      //做相关环境准备，绑定到SpringApplication,返回可配置环境对象ConfigurableEnvironment 
      ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);
      //配置spring.beaninfo.ignore，设置为true.即跳过搜索Bean信息
      configureIgnoreBeanInfo(environment);
      //控制台打印SpringBoot的Banner（横幅）标志
      Banner printedBanner = printBanner(environment);
      //根据WebApplicationType从ApplicationContextFactory工厂创建ConfigurableApplicationContext
      context = createApplicationContext();
      //设置ConfigurableApplicationContext中的ApplicationStartup为DefaultApplicationStartup
      context.setApplicationStartup(this.applicationStartup);
      //应用所有的ApplicationContextInitializer容器初始化器初始化context,触发ApplicationContextInitializedEvent事件监听，打印启动日志信息，启动Profile日志信息。
      //ConfigurableListableBeanFactory中注册单例Bean（springApplicationArguments）,并为该BeanFactory中的部分属性赋值。
      //加载所有的source.并将Bean加载到ConfigurableApplicationContext，触发ApplicationPreparedEvent事件监听
      prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
      //刷新容器（在方法中集成了Web容器具体请看 https://editor.csdn.net/md/?articleId=123136262）
      refreshContext(context);
      //刷新容器的后置处理（空方法）
      afterRefresh(context, applicationArguments);
      //启动花费的时间
      Duration timeTakenToStartup = Duration.ofNanos(System.nanoTime() - startTime);
      if (this.logStartupInfo) {
         //打印日志Started xxx in xxx seconds (JVM running for xxxx)
         new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), timeTakenToStartup);
      }
      //触发ApplicationStartedEvent事件监听。上下文已刷新，应用程序已启动。
      listeners.started(context, timeTakenToStartup);
      //调用ApplicationRunner和CommandLineRunner
      callRunners(context, applicationArguments);
   }
   //处理运行时发生的异常，触发ApplicationFailedEvent事件监听
   catch (Throwable ex) {
      handleRunFailure(context, ex, listeners);
      throw new IllegalStateException(ex);
   }
   try {
      //启动准备消耗的时间
      Duration timeTakenToReady = Duration.ofNanos(System.nanoTime() - startTime);
      //在run方法完成前立即触发ApplicationReadyEvent事件监听,表示应用上下文已刷新，并且CommandLineRunners和ApplicationRunners已被调用。
      listeners.ready(context, timeTakenToReady);
   }
   catch (Throwable ex) {
      handleRunFailure(context, ex, null);
      throw new IllegalStateException(ex);
   }
   return context;
}

```


读取配置文件方式
1 @Value
``` java
    @Value("${myapp.name}")
    private String appName;

    @Value("${myapp.version}")
    private String appVersion;
```

2 @ConfigurationProperties
``` java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "myapp.datasource")
public class DataSourceProperties {
    private String url;
    private String username;
    private String password;

    // Getters and setters...
}
```

``` yaml
myapp.datasource.url=jdbc:mysql://localhost:3306/mydatabase
myapp.datasource.username=myuser
myapp.datasource.password=mypassword
```
3  Environment
4 @PropertySource



@Bean 是将方法返回的对象作为bean注入到容器中
如何理解starter
starter是一个组件或者框架的依赖，通过引入starter，就可以简化开发人员的工作量
比如springboot的starter 就会引入这个springboot版本下适配的依赖


抽象工厂模式
有一个抽象工厂类（是一个接口），有两个工厂类去实现这个接口去创建，由工厂类生成产品对象，这些产品对象又继承产品类
https://blog.csdn.net/hm973046/article/details/132508052

可以想象一下 如果在星铁里 仪器中外圈有四个部位的组件，可以将这四个部位视为不同的产品簇，每个具体的组件就是对应产品簇的不同产品。此时，可以采用抽象工厂模式去构建不同的工厂类，不同的工厂类创建不同的组件，每个组件又能组成一套

``` java
// 抽象工厂 创建一起  
interface AbsFactory {  
Arm createArm();  
  
Helmet createHelmet();  
  
Shoes createShoes();  
  
Clothes createClothes();  
  
}  
  
// 根据不同的字段创建该字段对应的一套仪器  
class GuokeFactory implements AbsFactory {  
  
@Override  
public Arm createArm() {  
return new GuokeArm();  
}  
  
@Override  
public Helmet createHelmet() {  
return new GuokeHelmet();  
}  
  
@Override  
public Shoes createShoes() {  
return new GuokeShoes();  
}  
  
@Override  
public Clothes createClothes() {  
return new GuokeClothes();  
}  
}  
  
// 产品簇 这里就是不同的组件  
interface Arm {  
}  
  
class GuokeArm implements Arm {  
}  
  
class TiejiArm implements Arm {  
  
}  
  
  
interface Helmet {  
}  
class GuokeHelmet implements Helmet {}  
class TiejiHelmet implements Helmet {}  
  
  
interface Shoes {  
}  
  
class GuokeShoes implements Shoes {  
}  
  
class TiejiShoes implements Shoes {}  
  
interface Clothes {  
}  
  
class GuokeClothes implements Clothes {  
}  
  
class TiejiClothes implements Clothes {}
```

由上述的例子可以看出 
优点：
1 封装对象创建的过程
2 实现了产品簇的切换，每个字段都分为了不同的工厂去创建该字段的对象
3 保持代码一致性 即所有由同意工厂创建的产品相互关联
缺点：
1 扩展困难 比如我要增加一个叫项链的组件，那么就会违反开闭原则
2 增加系统复杂度

抽象工厂模式的使用场景包括但不限于以下情况：

- 一个系统要独立于它的产品的创建、组合和表示时。
- 一个系统要由多个产品系列中的一个来配置时。
- 需要强调一系列相关的产品对象的设计以便进行联合使用时。
- 提供了一个产品类库，而只想显示它们的接口而不是实现时。



# JVM 
如果用的是final修饰变量，外部访问这个变量时，是不会进行类的初始化阶段
直接访问父类的静态变量，不会触发子类的初始化
子类的初始化clinit调用之前，会先调用父类的clinit初始化

- 假如这个类还没有被加载和连接，则程序先加载并连接该类
- 假如该类的直接父类还没有被初始化，则先初始化其直接父类
- 假如类中有初始化语句，则系统依次执行这些初始化语句

**类初始化时机**: 只有当对类的主动使用的时候才会导致类的初始化，类的主动使用包括以下六种:

- 创建类的实例，也就是new的方式
- 访问某个类或接口的静态变量，或者对该静态变量赋值
- 调用类的静态方法
- 反射(如Class.forName("com.pdai.jvm.Test"))
- 初始化某个类的子类，则其父类也会被初始化
- Java虚拟机启动时被标明为启动类的类(Java Test)，直接使用java.exe命令来运行某个主类

打破双亲委派机制 自定义类加载器
通过重写loadClass方法，可以去实现自定义类加载器


在类加载的时候，将类加载到内存时，会用到方法区和堆区

java内存分为几个部分？
程序计数器，用来保存接下来要执行的指令的地址（在多线程线程切换时，通过程序计数器来继续执行）

栈 
栈帧里存放了
1 局部变量表：实例方法的this对象，方法中的参数，方法中声明的局部变量等。最基本的存储单元是槽，32位以内的类型只占1个slot并且为了节省空间，局部变量表中的槽是可以复用的。一旦某个局部变量不再生效，当前槽就可以被再次利用
2 操作数栈 主要用于保存计算过程的中间结果，同时作为计算过程中变量临时的存储空间
3 帧数据 动态链接，就是保存了符号引用到内存地址的关系。方法出口，就是存放着上一个栈帧中的下一个栈帧的下一条指令的地址

<font color="#ff0000">**栈是运行时的单位，而堆是存储的单位**。</font>
<font color="#ff0000">栈解决程序的运行问题，即程序如何执行，或者说如何处理数据。堆解决的是数据存储的问题，即数据怎么放、放在哪</font>

堆
堆空间三个值 used total max
当total不足时 虚拟机会分配max给total 

对象实例：所有通过new关键字创建的对象都会存储在堆中。
数组：数组也是对象，它们同样存储在堆中。
类的静态变量：虽然静态变量属于类而不是实例，但它们也存储在堆中，因为它们是类的一部分，而类本身是对象。
字符串常量池：在Java中，字符串常量会被存储在一个特殊的区域，称为字符串常量池。这个区域也位于堆中



























