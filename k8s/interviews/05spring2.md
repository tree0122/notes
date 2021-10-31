1. Spring中的设计模式（1m）
    1. 单例模式：bean默认是单例
    1. 原型模式：指定作用域为prototype
    1. 工厂模式：BeanFactory
    1. 模版方法：postProcessBeanFactory，onRefresh，initPropertyValue
    1. 策略模式： XmlBeanDefinitionReader, PropertiesBeanDefinitionReader
    1. 观察者模式：listener, event, multicast
    1. 适配器模式：
    1. 装饰者模式： BeanWrapper
    1. 责任链模式： 使用aop时的拦截链
    1. 代理模式： aop
    1. 委托者模式： delegate

1. AOP的实现原理（7m）
    动态代理

    aop时ioc的扩展功能，先有ioc，再有aop（ioc流程中的某个扩展点 BeanPostProcessor ）

    总： aop概念，应用场景，动态代理

    分： bean的创建过程中，BeanPostProcessor的后置处理 对bean扩展实现
    1. 代理对象的创建过程 （advice， 切面，切点）
    1. 通过jdk或者cglib的方式生成代理对象
    1. 在执行方法调用的时候，会调用到生成的字节码文件中，直接会找到 DynamicAdvisoredInterceptor 类的方法 intercept
    1. 根据之前定义好的通知来生成拦截器链
    1. 从拦截器链中依次获取每个通知开始执行

1. Spring中的事务如何回滚（25m）
    spring的事务管理是如何实现的？

    总：事务是由aop实现的，现生成代理对象，然后按照aop的流程来执行具体的逻辑。正常情况下要通过通知来完成核心功能，但是在事务通过 TransactionInterceptor 实现的，调用 invoke 来实现具体的逻辑

    分：
    1. 先做准备工作，解析各个方法上事务相关的属性，根据具体的属性来判断是否开启新事务
    1. 当需要开启的时候，获取数据库连接，关闭自动提交功能，开始事务
    1. 执行具体的业务sql
    1. 在操作过程中，如果执行失败了，那么会通过 AfterThrowing 完成回滚操作，具体是 doRollBack 方法实现
    1. 如果执行成功，那么通过 doCommit 完成事务提交操作
    1. 事务完毕后，需要清除相关的事务信息 cleanupTransactionInfo

更细致的话，需要TransactionInfo， TransactionStatus

1. Spring的事务传播（44m）
    传播特性7种
    Required, 
    Required_new
    nested
    support
    not_support
    never
    mandatory

    某个事务嵌套另一个事务时，怎么办？

    总：事务的传播特性指的是，不同方法的嵌套调用过程中，事务该如何处理，是用同一个事务还是不同事务，当出现异常是该如何处理。在日常中，使用比较多的是 required, required_new, nested

    分：
    1. 事务的不同分类，可分为三类：支持当前事务，不支持档期事务，嵌套事务
    1. 如果外层是 required，内层方法 required， required_new, nested
    1. 如果外层是 required_new，内层方法 required， required_new, nested
    1. 如果外层是 nested，内层方法 required， required_new, nested

    核心处理逻辑很简单：(可忽略)
    1. 判断内外方法是否同一个事务，是：异常统一在外层方法处理；不是：内层可能影响到外层方法，但是外层方法不会影响内层方法