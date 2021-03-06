# Spring 框架

## 1.什么是Spring

Spring是一种轻量级开发框架，旨在提高开发人员的**开发效率**以及系统的**可维护性**。

#### 官方介绍

Spring makes programming Java quicker, easier, and safer for everybody. 

Spring’s focus on speed, simplicity, and productivity has made it the [world's most popular](https://snyk.io/blog/jvm-ecosystem-report-2018-platform-application/) Java framework.

Spring使Java编程对每个人来说更快、更容易、更安全。

Spring对速度、简单性和生产率的关注使它成为世界上最流行的Java框架。



## 2. 什么是IOC

它是一种设计思想，核心的目的是为了解耦，解除调用者对被调用者的依赖。

#### 什么被反转了？

**获取资源的方式被反转了，从主动获取，变成了被动接受**，对象的创建和配置交给第三方容器。

控制反转的意思就是，把主动创建对象的权利，交给第三方，我只要被动接收就行了，不需要关系对象是如何创建的。



### 2.1 优点

第一：**资源集中管理**，实现资源的可配置和以管理，由Spring容易来配置和管理。

第二：**解耦**，把对象的创建配置与使用进行了解耦，调用者不用对象如何创建和配置的，用就行了。

第三：**可维护，可扩展**：想要增加和更换子类，修改配置文件就行了，实现了对象了**“热插拔”。**



### 2.2 缺点

1. 生成对象的步骤变的复杂了一些，本来只是new一下，现在要写一堆配置文件。

2. 效率损失，因为IOC生成对象是通过反射实现的，所以性能上有一定损耗。

   



### 2.3 为啥这样做

**解耦解耦解耦**

如果一个系统有大量的组件，其生命周期和相互之间的依赖关系如果由组件自身来维护，不但大大增加了系统的复杂度，而且会导致组件之间极为**紧密的耦合**，继而给测试和维护带来了极大的困难。



### 2.4 分析

 软件系统在没有引入IOC容器之前，如图1所示，对象A依赖于对象B，那么对象A在初始化或者运行到某一点的时候，自己必须主动去创建对象B或者使用已经创建的对象B。无论是创建还是使用以及销毁对象B，控制权都在自己手上。

  软件系统在引入IOC容器之后，这种情形就完全改变了，如图3所示，由于IOC容器的加入，对象A与对象B之间失去了直接联系，所以，当对象A运行到需要对象B的时候，IOC容器会主动创建一个对象B注入到对象A需要的地方。

  通过前后的对比，我们不难看出来：**对象A获得依赖对象B的过程,由主动行为变为了被动行为**，**控制权颠倒过来了**，这就是“控制反转”这个名称的由来。

![img](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE634ad3e08a9b4bd69dd796e0f63c3db8/2001)

![img](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCEf78aa835b5a043d58ba295febc42d616/2002)

### 什么是依赖注入

所谓依赖注入，就是由IOC容器在运行期间，动态地将某种依赖关系注入到对象之中。

### 与IOC的区别

IOC是一种解耦思想，依赖注入是一种解耦的实现方式。

依赖注入(DI)和控制反转(IOC)是从不同的角度的描述的同一件事情，就是指通过引入IOC容器，利用依赖关系注入的方式，实现对象之间的解耦。





### IOC的解释

**借助于“第三方”实现具有依赖关系的对象之间的解耦**

具体的是，解除对依赖对象的依赖关系，由主动创建，变为被动接收，从而提高系统的可维护性。

**对象A获得依赖对象B的过程,由主动行为变为了被动行为**，**控制权颠倒过来了**，这就是“控制反转”这个名称的由来。

IoC（Inverse of Control:控制反转）是一种**设计思想**，就是 **将原本在程序中手动创建对象的控制权，交由Spring框架来管理，更准确的是交给IOC容器来控制对象。**

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。

 **IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。** 



比如：ABC都依赖D，那么传统的WEB要在ABC中显式的new DImp（面向接口编程，DImp是D的实现类）

缺点一：本来可以共享DImp的，但是却重复创建对象，管理混乱

缺点二：一旦需求更换，或者迭代升级，Dlmp需要更换成DImp2，要把ABC全都修改一遍，耦合度极高。

而如果我们用了IOC，创建DImp任务，就交给了IOC容器，它来我们创建对象DImp，并注入到我们的ABC中。

优点一：ABC不需要显式的new DImp，只需要声明需要接口就行了，具体的实现类，由IOC容器给你注入进去

优点二：一旦需求更换，或者迭代升级，Dlmp需要更换成DImp2，只需要修改配置文件，而不需要把ABC全都修改一遍，完成了ABC与D的解耦。



## 3. 什么是AOP

AOP(Aspect-Oriented Programming:面向切面编程)，它是一种编程思想。

能够将那些与业务无关，**却为业务模块所共同调用的逻辑（例如事务处理、日志管理、安全检查等）封装起来，然后，以某种自动化的方式，以切面的方式动态的插入到核心逻辑中**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。



简单说就是那些与业务无关，却为业务模块所共同调用的逻辑封装抽离出来，以切面的形式，动态的织入我们的业务逻辑代码中。

**便于减少系统的重复代码，降低模块之间的耦合度，提高代码的可复用性和可维护性。**







## 4. Bean的生命周期

 五个阶段：

1. 实例化Bean

   当客户向容器请求一个尚未初始化的bean时，容器回调用createBean进行实例化

2. 属性赋值

   利用依赖注入完成 Bean 中所有属性值的配置注入。

3. 初始化 Initialization

4. 使用

5. 销毁 Destruction（前提是单例，非单例的bean，Spring就不管了）



## 5.注解

### 5.1 @Component, @Controller , @Service, @Repository

我们一般使用 `@Autowired` 注解让 Spring 容器帮我们自动装配 bean。要想把类标识成可用于 `@Autowired` 注解自动装配的 bean 的类,可以采用以下注解实现：

- `@Component` ：通用的注解，可标注任意类为 `Spring` 组件。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。
- `@Controller` : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 Service 层返回数据给前端页面。
- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
- `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。

@Service, @Controller , @Repository = {@Component + 一些特定的功能}。

- `@Component`是通用注解，其他三个注解是这个注解的拓展，并且具有了特定的功能，他是一个泛化的概念，仅仅表示一个组件 (Bean) ，可以作用在任何层次。
- `@Controller`层是spring-mvc的注解，具有将请求进行转发，重定向的功能。
- `@Service`层是业务逻辑层注解，这个注解只是标注该类处于业务逻辑层。
- `@Repository`注解在持久层中，捕获了平台特定的异常，具有将数据库操作抛出的原生异常转化为spring的持久层异常的功能。



### 5.2 @Resource和@Autowired

@Resource和@Autowired都是用来注入bean的

@Autowired为Spring提供的注解，只按照ByType注入。

@Resource由J2EE提供，默认按照ByName自动注入。

@Resource的作用相当于@Autowired，只不过@Autowired按照byType自动注入。



### 5.3 @Component和@Bean

Spring帮助我们管理Bean分为两个部分

- 一个是**注册Bean。**
- 一个是**装配Bean。**

@Component作用于类之上，表明一个类会作为组件类，并告知Spring要为这个类创建bean。（注册bean）

@Bean作用于方法之上，注解告诉Srping，这个方法会返回一个对象，这个对象要注册为Spring应用上下文中的bean。

两者的作用是相同的，都是注册bean到容器中。



创建bean的区别：

@Component通常通过类路径扫描来自动侦测以及自动装配到Spring容器当中去的。

@Bean注解通常是在这个注解所标注的方法中，定义产生这个bean的逻辑。



补充：

@bean更加灵活，当我们**引用第三方库需要装配到Spring中的时候**，只能通过@Bean来实现。



## 6. Spring中的设计模式

- 监听器：观察者模式
- 拦截器：代理模式、责任链模式
- 过滤器：责任链模式
- bean的创建：工厂模式、单例模式
- JdbcTemplate：模板模式

