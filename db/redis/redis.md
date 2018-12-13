## [redis: 内存中的数据结构存储系统](http://redis.cn/)
+ 简介  
+ 数据结构
+ 其他功能
+ 命令
+ 原理  

### 1. 简介
Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 

### 2. 数据结构  
它支持多种类型的数据结构，如： 
>1. [字符串（strings）](http://redis.cn/topics/data-types-intro.html#strings)  
>1. [散列（hashes）](http://redis.cn/topics/data-types-intro.html#hashes) 
>1. [列表（lists）](http://redis.cn/topics/data-types-intro.html#lists) 
>1. [集合（sets）](http://redis.cn/topics/data-types-intro.html#sets) 
>1. [有序集合（sorted sets）](http://redis.cn/topics/data-types-intro.html#sorted-sets) 
>1. [范围查询(bitmaps)](http://redis.cn/topics/data-types-intro.html#bitmaps)
>1. [范围查询(hyperloglogs)](http://redis.cn/topics/data-types-intro.html#hyperloglogs)  
>1. [地理空间（geospatial）](http://redis.cn/topics/data-types-intro.html#geospatial)   

### 3. 其他功能  
>1. [复制（replication）](http://redis.cn/topics/replication.html)  
>1. [LUA脚本（Lua scripting）](http://redis.cn/commands/eval.html)  
>1. [LRU驱动事件（LRU eviction）](http://redis.cn/topics/lru-cache.html)  
>1. [事务（transactions）](http://redis.cn/topics/transactions.html) 
>1. [不同级别的 磁盘持久化（persistence）](http://redis.cn/topics/persistence.html)  
>1. [Redis哨兵（Sentinel）](http://redis.cn/topics/sentinel.html)    
>1. [自动分区（Cluster）](http://redis.cn/topics/cluster-tutorial.html) 

### 4. 命令
+ [redis命令](http://redis.cn/commands.html)

### 5. 原理 
+ [redis的设计与实现](../../resources/pdf/redis设计与实现-v2.pdf)