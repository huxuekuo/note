# redis基础类型 



## redisObject

redisObject中包含的属性 值 type(类型),encoding(编码格式),lru(时间),notused(对齐位),refcount(引用计数),ptr(指向对象的值),

<font color="read">`type` 、 `encoding` 和 `ptr` 是最重要的三个属性。</font>

### type

<font color="read">type</font> 记录了对象的值的类型, 它的值有可能为 <font color="read">REDIS_STRING(字符串) , REDIS_HASH(哈希表) , REDIS_LIST(列表) , REDIS_SET(集合)  , REDIS_ZSET(有序集合)  </font> 

### encoding

<font color="read">encoding</font> 记录了对象所保存的值的编码格式 可能为 <font color="read"> RAW(编码为字符串), INT(整数), HT(哈希表), ZIPMAP(zipmap) LINKEDLIST(双端链表) ZIPLIST(压缩列表) INTSET(整数集合), SKIPLIST(跳跃集合) </font>

### prt



## String



## Hash



## List



## zset

