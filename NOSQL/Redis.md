; nested exception is java.sql.SQLSyntaxErrorException: Table 'starrank_service.wechat_mp_oauth' doesn't exist
2021-04-22 16:52:05.858  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:05.867  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:05.944  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:05.949  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:05.987  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:05.991  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:05.995  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:05.999  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.002  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.019  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.022  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.043  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.058  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.413  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.418  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.421  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:06.440  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 16:52:10.547  INFO 1 --- [onPool-worker-0] c.y.f.g.t.IstioTraceClientInterceptor    : Missing trace meta for outgoing request.
2021-04-22 18:14:57.247 ERROR 1 --- [nio-8080-exec-2] c.s.s.a.c.GlobalExceptionConfiguration   : GRPC_EXCEPRedis



### Redis是什么?

​		redis是键值对数据库,他将数据存储在内存中,可以提高数据查询的效率,在需要在性能和高并发下面可以使用Redis,性能一些数据,在数据库出现查询慢在查询出来以后几个小时不会变动的数据就可以考虑存放在Redis中,还有在大并发下对数据库进行的压力问题也可以考虑加一层缓存,将一些热点数据放在缓存中,Redis的数据结构对比memcachce是比较丰富的

### redis分布式锁实现方式?有什么问题?

分布式锁一般有三种实现,数据库乐观锁,Redis锁,zookeeper的分布式锁,redis的锁可以使用setnx命令

要想确保分布式锁的可用,必须满足

1. 互斥性,在同一时刻只有一个客户端能持有锁

2. 不会发生死锁,及时一个客户端在持有锁的期间崩溃了,也能保证其他的客户端可以加锁
3. 具有容错性,只要大部分的redis节点正常运行,客户端就可以加锁和解锁
4. 加锁和解锁必须是一个客户端,不用客户端自己的锁让其他客户端结了

```java
 String result = jedis.set(lockKey, requestId, SET_IF_NOT_EXIST, SET_WITH_EXPIRE_TIME, expireTime);
```

使用jedis工具中有一个set方法里面有5个形参

​	locKey 我们使用key来当锁,因为key是惟一的

   第二个是value,请求标识,这个是保证我加锁只有我能解

  第三个填写NX,set IF NOT EXIST 加入Key存在就不添加

第四个写要给这个锁加一个过期时间

第五个跟第四个相呼应,设置时间



​	

### Redis分布式Session



登录时将Session存放在Cookie中一份,在Redis缓存一份,

### Redis雪崩与缓存穿透与缓存击穿

#### 				缓存穿透

​						当用户查询的数据是缓存中与数据中都不存在的值,而用户不断发送请求,

​						这个时候如果就可以在缓存中吧这个key的值设置为null,并把这个值的过期时间设置的要比短一些防止数据更新不过来

#### 				缓存击穿

​					主要体现在热点key失效的时候,访问用户多,给数据库造成的压力很大,

​					1.使用锁的方式,如果没有查询到数据使用synchronized锁,只有进去的第一个线程去查询数据库,并将信息放回缓存,其他线程等待第一个线程结束后去缓存中去取数据

					2. 使用热点key不设置过期时间,	



#### 				雪崩

​				当Redis抵挡了大量的请求时,Redis的大量key过期失效,或者说这个Redis节点宕机了,

​				大量key集中失效的解决办法就是分类进行设置缓存时间,区分热点key与冷门key也可以缓解缓存服务的资源

​				宕机这个问题就可以使用Redis集群了



### Redis缓存不一致性问题



#### 不一致性问题的由来

​	假设一个常见,A需要更新一个数据,他首先将缓存删掉,以后去更改数据库的信息,但是还未更改完成,B读请求来了,查询缓存发现缓存中么有,就去数据库中查询找到以后又将数据放回了缓存,这就造成了不一致问题





分布式环境下无法做到强一致性,采取合适的策略比如更新数据库以后即使更新缓存,缓存失败添加重试机制,例如MQ的消息队列

### Redis与memcachce的区别



1.redis有多样的数据结构比如list,set,hash等存储方式

2.redis支持持久化,而memcahce不支持持久化,如果memcahce挂掉以后数据就没了



如果对缓存数据有持久化需求或者对数据结构和处理有高级要求就是用Redis但是如只是简单存储memcahce可以考虑

### Redis持久化方式

​				AOF日志

AOF记录客户端的每一个删除,写操作,以文本的方式储存	他有三种策略 每秒同步,没修改同步和不同,每秒同步也是异步完成效率也很高,所差的是一旦系统出现宕机现在,那么这一秒的数据就丢失了,而修改通过是同步持久化方式,每次放生数据的变化都会记录到磁盘中,



AOF的保存文件通常比RDB保存的文件大,RDB在恢复大数据时速度快鱼AOF的修复速度要快						![image-20190616092006087](/Users/hu/Library/Application Support/typora-user-images/image-20190616092006087.png)

​				RDB快照,

​						在指定的时间间隔内将内存中的数据集快照写入磁盘,实际上就是fork一个子进程,先将数据集写入临时文件中,写入成功以后在替换之前的文件,用二进制压缩储存

![image-20190616084956644](/Users/hu/Library/Application Support/typora-user-images/image-20190616084956644.png)