### [git地址](http://git.mashibing.com/bjmashibing/InternetArchitect)

1. jvm的内存模型，以及他们存放的内容（2m）
    - jvm stack
    - pc register
    - native method stack
    - method area
    - heap
1. 堆内存划分的空间，如何回收这些内存对象，有哪些回收算法（35m）
1. 如何解决线上gc频繁的问题？（47m）
    1. 时间点
    1. 监控的gc频繁
    1. jvm的参数设置
    1. 对可能进行排除
    1. 针对大对象或者长生命周期对象导致的FGC，可jmap定位可疑对象
    1. 通过可疑对象定位到具体代码，再次分析，并结合GC原理和jvm参数，弄清楚可疑对象是否满足进入到老年代的条件，然后才能下结论
1. 描述class初始化过程（52m）
1. 内存溢出的原因，如何排查线上问题（53m）
    1. java heap space...：堆栈溢出，代码问题的可能性大
    1. GC over head limit exceeded：高频的GC状态
    1. PermGen space： 类
    1. Direct buffer memory
    1. StackOverFlowError：Xss太小
    1. unable to create new native thread：堆外内存不足，无法为线程分配内存区域
    1. request {} byte for {} out of swap：地址空间不足
1. jvm的垃圾回收器【7种】，实际中如何选择（61m）
    1. Serial
    1. ParNew
    1. Parallel Scavenge
    1. Serial Old
    1. Parallel Old
    1. CMS
    1. G1
1. 类加载模型（71m）
    1. 启动类加载器
    1. 扩展类加载器
    1. 应用类加载器
    1. 自定义类加载器
1. 为啥要用meta空间，有什么好处(71m)
    原因：
    1. 字符串在永久代，容易出现性能问题和内存溢出
    1. 永久代的大小难确定，太大容易使老年代溢出，太小容易使永久代溢出
    1. 永久代为GC带来不必要的复杂度，且回收效率低

    元空间特点：
    1. 每个类加载器都有专门的存储空间
    1. 不会单独回收某个类
    1. 元空间里的对象的位置使固定的
    1. 如果发现某个类加载器不再存货，会把相关的空间整体回收
1. G1有啥特点（结合第6题内容）（72m）
1. 介绍垃圾回收算法（73m）
    1. 标记清除
    1. 标记整理
    1. 复制
1. Happens-Before规则（76m）
    1. 程序顺序
    1. 锁顺序
    1. volatile变量
    1. 传递性
    1. 线程启动
    1. 线程终止
    1. 线程中断
1. 描述java的类加载和初始化的过程（88m）
    加载、连接（校验、准备、解析）、初始化、使用、卸载
1. JVM线上出现OOM问题如何定位（问题5类似）
1. 吞吐量优先和响应优先的回收器是哪些？
    - 吞吐量优先：Parallel Scavenge + Parallel old
    - 响应优先：ParNew + CMS
1. 阻塞队列的有界和无界，实际中使用？
    - ArrayBlockingQueue：数组构成的有界阻塞队列，一把锁
    - LinkedBlockingQueue：链表构成的无界阻塞队列，两把锁
    - PrioriyBlockingQueue：优先排序的无界队列，可实现精确的定时任务
    - DelayQueue：优先排序的无界队列
    - SynchronousQueue：不存储元素的阻塞队列
    - LinkedTransferQueue：链表构成的无界阻塞队列
    - LinkedBlokingDeque：链表构成的双向无界阻塞队列
1. jvm监控系统是通过jmx做的（99m）
1. 内存屏障的汇编指令是啥？（100m）
1. 怎么提前避免内存泄露？（101m，类似问题5）
