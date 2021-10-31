### 基础
1. 网络到分布式（lvs+redis+zk） mashibing.com/pc.html
1. 网络的小课
1. 内存与io，磁盘io、网络io

### 具体问题
1. tcp三次握手（7m）
    1. client 发送 syn （syn-send）
    1. server 收到 client 的包后，发送 syn + ack （sync-rcv）
    1. client 发送 ack （established)
    
    三次握手完成后，内核将socket放入accept队列

    backlog满了（accept队列满了），新客户端直接 connection refused

1. tcp四次挥手 （45m）
    1. client 发送 fin （fin-wait1）
    1. server 发送 fin + ack （server为close-wait，client为fin-wait2）
    1. server 处理完事情后，发送 fin （server为last-ack，client为time-wait）
    1. client 发送 ack （server为closed）

    双方都需要挥手，time-wait是2个返回时间
1. tcp连接状态
1. connection refused（25m）
    accept队列，等待程序接受的连接，有大小限制 backlog（不被程序接收），backlog可内核、程序配置

    listen状态的socket，其sendQ队列表示内核可等待程序接收的连接，rcvQ队列表示内核得到的连接，即accept队列

1. OSI七层参考模型
    1. 应用层
    1. 表示层
    1. 会话层
    1. 传输控制层
    1. 网络层
    1. 链路层
    1. 物理层

1. 什么是长连接和短链接（80m）
    tcp是连接，是否长连接是受应用层协议影响
    连接是不是一个“复用”载体，例子如下：
    1. http1.0/1.1没有开启keepalive保持，连接只负责一次同步阻塞的请求+响应
    1. http1.1开启keepalive保持，同步复用连接：多次请求+响应，长连接
    1. dobbo协议（rpc），打开连接，同步/异步复用连接：多次（请求+响应）、（请求请求）（响应响应），当复用连接的时候，需要消息的id，且client和server都要有这个约定，是有状态的通信，长连接

1. io模型（86m）
    io是程序对着内核的socket-queue的包装
    1. BIO，读取，一直等queue里有才返回，阻塞模型，每连接对应每个线程。server.accept() 阻塞等待返回
    1. NIO，读取，立即返回：两种结果（读到、未读到），程序逻辑要自己维护。server.accept() 立即返回（1，0）
    1. 多路复用器：内核增加select、poll、epoll新增的和数据接收，连接接受实质无关的调用，得到对应socket（listen、r/w）的事件，可以有效的再去accept、r/w
1. 同步阻塞，同步非阻塞（94m）
    BIO、NIO、多路复用器，在IO模型上都是同步的，都是程序自己accept、r/w
1. 有状态、无状态
1. 粘包、拆包（97m）
    有程序，有内核，程序和内核协调工作
    有一些是内核做的事情，如 三次握手，数据发出去，接受进来，tcp 分包

    到我们自己的程序中，即便在一个socket中，也可能收到多个消息在一个字节数组中，我们要自己拆解 

### cmd
nc- l localhost 9999
netstat -natp 
ss -lna
nc localhost 9999
cat /proc/sys/net/core/somaxconn: accept队列，min(somaxconn, 程序设置的backlog)
cat /proc/sys/net/ipv4/tcp_max_syn_backlog：半连接（与DDOS相关）

每人顾好自己，每人处理好对别人的打扰
