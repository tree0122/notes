1. 服务器CPU数量及线程池线程数量的关系（13m）
1. 多线程之间如何通信的？（21m）
    1. 多进程之间通信：信号量、信号、套接字、有名管道、无名管道、共享内存、mq
    1. 多线程间通信，多线程之间共享类进程的地址空间，由于并发引起互斥操作，需要同步机制：volatile、sync、Lock
1. 描述一下synchronized底层实现，以及和lock的区别（31m）
1. synchroned关键字加在静态方法和实例方法的区别？（32m）
    1. 锁实例对象
    1. 锁class对象
    1. 某class对象的加锁，同时对其实例对象加锁，不冲突
1. CountDownLatch的用法？（41m）
1. 线程池问题（44m）
    1. executors的创建
    1. 线程池参数：
    1. 拒绝策略
    1. 任务放置的执行过程
    1. 任务结束后会不会回收线程
    1. 未使用的线程放在哪里：HashSet<Worker> workers
    1. 线程放在哪里
    1. cache线程池需要销毁核心线程
1. Java线程池的几种状态及线程各个状态之间如何切换？（63m）
    1. running
    1. shutdown
    1. stop
    1. tiding
    1. terminated   
1. 如何在方法栈中进行数据传递（67m）
1. 描述一下ThreadLocal的底层实现形式及实现的数据结构？
1. Synchronized不是公平锁（80m）
1. 锁的四种状态及升级过程？（80m）
1. CMS和G1的异同？（81m）
1. G1什么时候引发Full GC？（83m）
    G1的垃圾回收速度，赶不上应用线程的分配速度
1. 除了CAS、原子类、syn、Lock，还有什么线程安全的方式？（84m）
1. hashMap和HashTable的异同？
1. CAS的ABA
1. AQS
1. volatile的原理
1. volatile的可见性和禁止指令重排序怎么实现的？（86m）
    内存屏障
1. ConcurrnentHashMap底层原理（86m）
