## 运维架构分层
```
scale up: 向上扩展
scale out: 水平扩展

心得：一立（0-1） 二扩（1-10） 三优化  四架构  五治理

自动化运维平台: ansible, soltstack
持续集成及持续部署平台: jenkins, gitlab
日志收集及展示平台: elk
分布式监控平台: prometheus, zabbix
```


1. 域名

1. 负载均衡层
    ```
    软件实现：lvs(4层), nginx, haproxy
    高可用实现：keepalived, HA
    硬件实现：F5
    ```
1. web缓存层：静态数据

1. web服务器：处理动态请求

1. 数据缓存层：redis等

1. 数据层