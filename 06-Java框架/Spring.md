# Spring

## MVC模型概念
## servlet的基本概念
## servlet的生命周期
## servlet中定制session的过期时间
## Servlet中的session工作原理 （禁用cookie如何使用session）
## filter和listener是什么？有什么区别？servlet、filter、listener：http://www.i3geek.com/archives/870
## JSP和Servlet的区别、共同点（JSP的工作原理）。

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

## Spring如何实现AOP和IOC的？

## Spring IoC 中的BeanFactory：http://www.i3geek.com/archives/878


## Spring AOP 原理：http://www.i3geek.com/archives/912


## Spring的事务管理 ，Spring bean注入的几种方式
## Spring的beanFactory和factoryBean的区别
## 为什么CGlib方式可以对接口实现代理？
## RMI与代理模式
## Spring的事务隔离级别，实现原理
## 对Spring的理解，非单例注入的原理？它的生命周期？循环注入的原理，aop的实现原理，说说aop中的几个术语，它们是怎么相互工作的？
## Mybatis的底层实现原理
## MVC框架原理，他们都是怎么做url路由的
## spring boot特性，优势，适用场景等
## quartz和timer对比
## spring的controller是单例还是多例，怎么保证并发的安全
## BeanFactory 和 FactoryBean？
## Spring IOC 的理解，其初始化过程？
## BeanFactory 和 ApplicationContext？
## Spring Bean 的生命周期，如何被管理的？
## Spring Bean 的加载过程是怎样的？
## 如果要你实现Spring AOP，请问怎么实现？
## 如果要你实现Spring IOC，你会注意哪些问题？
## Spring 是如何管理事务的，事务管理机制？
## Spring 的不同事务传播行为有哪些，干什么用的？
## Spring 中用到了那些设计模式？
## Spring MVC 的工作原理？
## Spring 循环注入的原理？
## Spring AOP的理解，各个术语，他们是怎么相互工作的？
## Spring 如何保证 Controller 并发的安全？