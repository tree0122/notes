## redis的数据结构与对象
+ 简单动态字符串  
+ 链表
+ 字典
+ 跳跃表
+ 整数集合  
+ 压缩列表  
+ 对象  

### 1. 简单动态字符串

### 2. 链表

### 3. 字典

### 4. 跳跃表

### 5. 整数集合

### 6. 压缩列表（ziplist）  
> ziplist是**列表键**和**哈希键**的底层实现之一。 
> 
> 当一个列表键只包含**少量**列表项， 并且每个列表项要么就是**小整数值**， 要么就是长度比**较短的字符串**， 那么 Redis 就会使用压缩列表来做列表键的底层实现。

### 7. 对象
> Redis基于这些数据结构创建了一个**对象系统**， 这个系统包含**字符串对象**、**列表对象**、**哈希对象**、**集合对象**和**有序集合对象**这五种类型的对象。  

每个对象都由一个 redisObject 结构表示：  

    typedef struct redisObject {
    
        // 类型
        unsigned type:4;
        
        // 编码
        unsigned encoding:4;
        
        // 指向底层实现数据结构的指针
        void *ptr;
        
    } robj;  
    
对象的类型：  

   类型常量	    | 对象的名称 
  --------      | -------- 
REDIS_STRING    |	字符串对象
REDIS_LIST	    |   列表对象
REDIS_HASH	    |   哈希对象
REDIS_SET	    |   集合对象
REDIS_ZSET	    |   有序集合对象

表: 对象的编码  

编码常量	                    |   编码所对应的底层数据结构
------                      |   -----
REDIS_ENCODING_INT          |	long 类型的整数
REDIS_ENCODING_EMBSTR       |	embstr 编码的简单动态字符串
REDIS_ENCODING_RAW          |	简单动态字符串
REDIS_ENCODING_HT           |	字典
REDIS_ENCODING_LINKEDLIST   |	双端链表
REDIS_ENCODING_ZIPLIST	    |   压缩列表
REDIS_ENCODING_INTSET       |	整数集合
REDIS_ENCODING_SKIPLIST     |	跳跃表和字典

