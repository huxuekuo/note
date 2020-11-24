# Redis 淘汰策略



# Redis可选项

## volatile-lru



> 从设置了过期时间中的数据集中, 选择最近最少没有使用的数据,进行释放



## allkeys_lru



> 在全部的数据集中, 选择最近最少没有使用的数据,进行释放



### LRU算法



> LRU是Least Recently Used的缩写，即最近最少使用，是一种常用的[页面置换算法](https://baike.baidu.com/item/页面置换算法/7626091)，选择最近最久未使用的页面予以淘汰。该算法赋予每个[页面](https://baike.baidu.com/item/页面/5544813)一个访问字段，用来记录一个页面自上次被访问以来所经历的时间 t，当须淘汰一个页面时，选择现有页面中其 t 值最大的，即最近最少使用的页面予以淘汰。



百度科普链接 : https://baike.baidu.com/item/LRU/1269842?fr=aladdin

## volatile_random



> 从设置了过期时间的数据当中,随机选择一个数据进行释放



## allkeys_random



> 从全部的数据中,随机随机选择数据进行释放



## votatile_ttl



> 从设置了过期时间的数据中, 找即将要过期的数据进行释放





## noeviction



> 不删除任何数据, 这时如果内存不够时，会直接返回错误。





# Redis配置



> redis 的默认配置`noeviction`,在Redis中使用LRU近似算法, 默认情况下 随机挑选5个键从中获取最近最少使用的数据进行释放,在配置文件中可以通过`maxmemory-samples`的值来设置redis需要检查key的个数,但是检查的越多，耗费的时间也就越久,淘汰的数据越精确



`redis配置文件中默认`



```json
maxmemory-policy noeviction
```



