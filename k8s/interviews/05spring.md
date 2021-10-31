1. 谈谈Spring IOC的理解、原理与实现 （1m）
    总：
    1. 控制反转，
        DI：依赖注入
    1. 容器，map存储，三级缓存，singletonObject，生命周期

    分：
    1. 容器的创建过程（DefaultListableBeanFacotry），向bean工厂设置一些参数（BeanPostProcessor，Aware）
    1. 加载解析Bean对象，准备创建bean的定义对象BeanDefinition
    1. BeanFactoryPostProcessor的处理（扩展点），PlaceHolderConfigSupport， ConfigurationClassPostProcessor
    1. BeanPostProcessor 的注册，对后续bean对象的扩展
    1. 反射的方式对BD对象实例化成具体的bean对象
    1. bean对象的初始化过程（填充属性，aware方法、BPP前置方法、init方法、BPP后置方法）
    1. 生成完成的bean对象，通过getBean()可获取
    1. 销毁过程

1. 谈谈Spring IOC的底层实现（34m）
    对他的理解和了解过的实现过程：
    反射，工厂，设计模式，关键的几个方法

    createBeanFactory, getBean, doGetBean, doCreateBean, createBeanInstance, polulateBean, initializeBean

    1. createBeanFactory创建一个bean工厂（ DefaultListableBeanFacotry ）
    1. 循环创建对象，getBean查找
    1. 通过 createBean, doCreateBean 反射方式创建
    1. 对象的属性填充 polulateBean
    1. 进行其他的初始化操作 initializeBean

1. 描述一下bean的生命周期（43m）
    1. 实例化bean：反射生成
    1. 填充bean的属性： polulateBean， 循环依赖问题（三级缓存）
    1. 调用aware接口相关的方法： invokeAwareMethod （BeanName、BeanFactory、BeanClassLoader）
    1. 调用 BeanPostProcessor 的前置方法 （ApplicationContextPostProcessor，设置 ctx、env等）
    1. 调用initMethod方法: invokeMethod(), initializingBean
    1. 调用 BeanPostProcessor 的后置方法：aop在此次实现， AbstractAutoProxyCreator， 注册Destruction相关回调接口
    1. 获取到完整的对象，可通过getBean方式获取
    1. 销毁流程，实现DispoBean接口，调用destroyMethod方法 

1. Spring是如何解决循环依赖问题（63m）
    三级缓存、提前暴露对象、aop

    总：什么是循环依赖

    分：bean的创建过程：实例化、初始化（填充属性）
    1. 先创建A对象，实例化A对象，此时A对象的b属性为null，填充属性b
    1. 从容器中查找B对象，找不到直接创建B对象
    1. 实例化B对象，此时B对象的a为null，填充属性a
    1. 从容器中查找A对象，找不到，直接创建
    1. 实例化和初始化分开操作

    为啥需要三级缓存？三级缓存的value类型是ObjectFactory

1. 缓存的放置时间和删除时间
    三级缓存：createBeanInstance 之后，addSingltonFactory
    二级缓存：第一次从三级缓存获取对象后放入的，同时删除三级缓存， getSinglton
    一级缓存：完整对象后，放入一级缓存，删除二三级缓存， 之后，addSinglton

1. BeanFactroy 和 FactoryBean 的区别（92m）
    相同点：都是用来创建bean对象的
    不同点：
        BeanFactroy 创建对象时，必须严格遵守生命周期流程
        FactoryBean 简单的自定义某个对象的创建，并交给spring管理
