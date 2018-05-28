# Spring

## MVC模型概念

M:Model,指数据操作，模型改变将改变视图
V: View 视图展现
C: Controller 控制器，接收指令并改变模型。

## servlet的基本概念

Servlet运行于支持Java的应用服务器中，一般用于响应http请求。

## servlet的生命周期

1. 创建servlet实例
2. 调用init方法
3. 响应客户端get/post请求
4. 调用destroy方法

## servlet中定制session的过期时间

defaultSessionTimeOut="3600" 

## Servlet中的session工作原理 （禁用cookie如何使用session）

浏览器端cookie中保存会话sessionId，再次请求时就能找到会话对应的session。
禁用cookie的情况，将sessionId放到每次请求的url或请求参数中。

## filter和listener是什么？有什么区别？servlet、filter、listener：http://www.i3geek.com/archives/870

servlet 流程是短的，url传来之后，就对其进行处理，之后返回或转向到某一自己指定的页面
filter流程是线性的,它就像是像一个职责链，url传来之后，进行预处理，然后传递给下一个filter继续执行，而servlet 处理之后，不会继续向下传递。其生命周期：init()->doFilter()->destroy()
监听器Listener，它是实现了javax.servlet.ServletContextListener 接口的服务器端程序，它也是随web应用的启动而启动，只初始化一次，随web应用的停止而销毁。
主要作用是： 做一些初始化的内容添加工作、设置一些基本的内容、比如一些参数或者是一些固定的对象等等。

## JSP和Servlet的区别、共同点（JSP的工作原理）。

JSP的本质就是Servlet，JVM只能识别java的类，不能识别JSP的代码,Web容器将JSP的代码编译成JVM能够识别的java类。
Servlet的应用逻辑是在Java文件中，从Java代码中动态输出HTML，并且完全从表示层中的HTML里分离开来。
而JSP的情况是Java和HTML可以组合成一个扩展名为.jsp的文件。JSP侧重于视图，Servlet主要用于控制逻辑

## Spring AOP和IOC 原理，使用？ 具体场景举例？ 如何优化？

IOC
控制反转：依赖对象的获得被反转了，原本依赖对象需要自身实现，先由IOC容器控制。
在对象生成或初始化时直接将数据注入到对象中，或者将对象引用注入到数据域中来注入对方法调用的依赖。
依赖注入可递归，逐层注入，有序的建立依赖关系，简化依赖关系的管理。

应用场景：需要对Bean的依赖关系和生命周期进行管理。

IOC初始化过程：
1. Resource定位过程。指对BeanDefinition的资源定位，有ResourceLoader同感统一的Resource接口完成。
2. BeanDefinition的载入，将用户定义好的Bean表示成IoC容器内部数据结构。
3. 想IoC容器注册BeanDefinition，使用HashMap保存。
IoC依赖注入过程：
依赖注入是用户第一次向IoC容器索要Bean触发的，主要实现在 BeanFactory 的 getBean() 方法。获取当前Bean的所有依赖Bean，
这样会触发getBean的递归调用，直到取到一个没有任何依赖的Bean。使用CGLIB对Bean进行实例化

AOP面向切面编程，OOP引入封装、继承和多态建立对象的层次结构，允许定义从上到下的关系，AOP则定义从左到右的关系。
AOP核心思想是将业务逻辑同对其提供支持的通用服务进行分离。
使用场景：权限认证、日志记录、事务处理、异常处理、缓存
原理：以jdk动态代理技术为基础，设计出一系列的AOP横切实现。
两个概念：Advice通知 和 Pointcut 切入点
Advice 描述AOP在连接点的切面行为，前置、后置、异常通知。
Pointcut 决定Advice通知作用于哪一个连接点，基于正则匹配。
根据切入点进行扫描，在进行IoC的注册时，生成代理对象，并将拦截器链Advice配置进去。在调用target方法时进行回调通知方法。

## Spring的事务管理 ，Spring bean注入的几种方式

Spring通过AOP模块完成声明式事务处理。
首先完成事务属性的配置和读取，事务对象的抽象。
通过TransactionProxyFactoryBean 生成代理对象，在代理对象中，通过TransactionInterception完成对代理方法的拦截。
对于具体的事务处理实现，如事务的生成、提交、回滚、挂起，Spring设计出了一系列PlatformTransactionManager具体事务处理器来完成。

Spring 7个事务传播属性：
1. 默认REQUIRED ， 如果存在事务，则支持当前事务，否则新创建事务。
2. MANDATORY ， 支持当前事务，若不存在事务，抛出异常。
3. NEVER ， 以非事务方式执行，若存在事务，则抛出异常。
4. NOT_SUPPORT ， 以非事务方式执行，若存在事务，则挂起。
5. REQUIRES_NEW ， 新建事务，若存在当前事务，则挂起。
6. SUPPORTS ， 支持当前事务，若不存在事务，以非事务方式执行。
7. NESTED ， 支持当前事务，新增Savepoint，与当前事务同步提交或回滚。外层事务失败会连同嵌套事务回滚，内层事务失败不会引起外层事务回滚。

事务并发引起的三种情况：

1. 脏读。第一个事务读到了另一个事务未提交的数据。
2. 不可重复读。第一个事务的两次读取之间，另一个事务对数据进行了更新，导致两次读取数据不一致。更新和删除造成。
3. 幻象读。第一个事务进行两次查询，另一个事务对数据插入，导致两次查询结果不同。插入数据造成。

四种事务隔离级别：

1. DEFAULT 使用数据库的隔离级别
2. READ_UNCOMMITTED 读未提交
3. READ_COMMITTED 读已提交
4. REPEATABLE_READ 可重复读
5. SERIALIZABLE 串行化

Spring Bean 注入方式：
1. Setter
2. 构造器
3. 接口注入

## Spring的beanFactory和factoryBean的区别

BeanFactory 如同抽象工厂模式中的工厂，会根据所需生产的Bean的不同生成对应的工厂类FactoryBean，再由这些工厂类来生成相应的Bean。
如ProxyFactoryBean，FactoryBean机制为我们提供了很好的封装机制，比如封装Proxy、RMI、JNDI等。

## 为什么CGlib方式可以对接口实现代理？

CGLIB创建一个继承实现类的子类，采用ASM库动态修改子类的字节码实现。

## RMI与代理模式

RMI是基于tcp/ip的远程方法调用，采用JAVA的序列化方案。
代理模式指通过代理者调用目标方法。
RMI通过代理模式可以完成类似本地方法的调用，具体实现是 RmiProxyFactoryBean生成代理对象，在代理对象中设置RmiInterceptor拦截器。
拦截器在IoC容器载入时对客户端参数进行设置，当调用代理方法时，会进行invoke回调，回调方法中发起远程请求。

## Spring的事务隔离级别，实现原理

四种事务隔离级别：
1. 读未提交，最弱的隔离级别，不加锁。
2. 读已提交，在事务提交后才允许读，使用写锁排斥读锁实现。
3. 可重复读，在读取事务提交后才允许读写，使用读锁排斥写锁实现
4. 序列化，串行化读写操作。

## 对Spring的理解，非单例注入的原理？它的生命周期？循环注入的原理，aop的实现原理，说说aop中的几个术语，它们是怎么相互工作的？

非单例注入即类型为property的Bean，每次请求都会创建一个实例对象。它的后续生命周期将会由调用者管理。

循环注入两种方式：
1. 构造参数注入，会报错。正在创建的Bean放入一个创建池中，当正在创建的Bean发现自己已经在池中，会抛出BeanCurrentlyInCreationException。
2. Setter方式注入，先实例化Bean对象，再设置对象属性。Spring提供了已经实例化但未设置属性的Bean获取方式，不会出来循环注入的问题。

AOP实现原理：
原理：以jdk动态代理技术为基础，设计出一系列的AOP横切实现。
两个概念：Advice通知 和 Pointcut 切入点
Advice 描述AOP在连接点的切面行为，前置、后置、异常通知。
Pointcut 决定Advice通知作用于哪一个连接点，基于正则匹配。
根据切入点进行扫描，在进行IoC的注册时，生成代理对象，并将拦截器链Advice配置进去。在调用target方法时进行回调通知方法。

## Spring MVC框架原理，他们都是怎么做url路由的

web.xml中两项配置，ContextLoaderListener 和 DispatchServlet 提供了在Web容器中对Spring的接口。
这些接口与Web容器的耦合是通过 ServletContext 实现的，在ServletContext宿主环境中，Spring 通过 ContextLoaderListener初始化建立起IoC容器体系。
把 DispatchServlet 作为Spring MVC处理web请求的转发器，完成响应http请求的准备。
使用 HandlerExecutionChain 定义 根据URL映射方式，注册 Handler 和 Interceptor.
DispatchServlet 会完成HandlerMapping初始化，这里的 Mapping 关系作用是为http请求找到相应的 Controller 控制器。
Servlet拦截请求并使用 doGet/doPost方法处理，DispatchServlet 通过 doService 方法响应请求。
根据URL在handlerMapping中获取Handler，通过Handler返回的是 HandlerExecutionChain ， 包含了最终的Controller和拦截器链
调用handler的handRequest方法和视图呈现的前后都会穿插对拦截器链的调用。


## spring的controller是单例还是多例，怎么保证并发的安全

controller是单例。
保证并发安全：
1. 不要在Controller定义成员变量
2. 如果需要定义非静态成员变量时，通过 @Scope 注解将其设置为非单例类型。
3. 通过ThreadLocal设置成员变量。

## BeanFactory 和 ApplicationContext？

BeanFactory为基本的IoC容器，可以获取Bean及相关信息。
ApplicationContext 继承自 ListableBeanFactory , 同样继承了 BeanFactory 在基础的容器上添加Web相关的特性。
如通过继承MessageResource接口支持不同信息源，通过继承Resource接口访问资源，通过继承ApplicationPublisher支持应用事件。

## Spring Bean 的生命周期，如何被管理的？

1. 实例化Bean
2. 是指对象属性，依赖注入
3. 检查Aware相关接口并设置相关依赖
4. BeanPostProcessor 前置处理
5. 检查是否实现 InitializingBean 调用afterPropertiesSet方法
6. 检查是否配置自定义的init-method
7. BeanPostProcessor 后置处理
8. 检查是否实现 DisposableBean接口，是否配置自定义destroy方法

## Spring 中用到了那些设计模式？

单例模式，生产Bean
抽象工厂模式，工厂模式，如BeanFactory
代理模式，如AOP
命令模式，如Spring MVC中获取url对应Handler并处理
模板模式，各种BeanFactory以及ApplicationContext实现
Tomcat中有很多场景都使用到了外观模式，因为Tomcat中有很多不同的组件，每个组件需要相互通信，但又不能将自己内部数据过多地暴露给其他组件。用外观模式隔离数据是个很好的方法
策略模式，如url到handlerMapping有很多策略，默认的是url到Bean的映射。
责任链模式，如Filter何拦截器链

## 如果要你实现Spring AOP，请问怎么实现？

动态代理
通知
切入点

## 如果要你实现Spring IOC，你会注意哪些问题？

容器
Bean的创建
依赖注入
