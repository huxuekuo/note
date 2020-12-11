# Redis内存模型



## redis内存参数介绍(部分)



我们使用**redis-cli** 进入**redis** 后使用 `info memory`,可以获取到内存相关的信息



![image text](https://gitee.com/panda_soft/note_images/raw/master/redis_info_memory_01.jpg)

我们介绍几个重要的数据, 其他想了解的加21044903@qq.com 可以进行免费解答,

### **used_memory**

**Redis** 分配器分配的内存总量(**单位字节**)



### **used_memory_rss**

**Redis**进程占据操作系统的内存,与**top**及**ps**命令看到的值是一致的；除了分配器分配的内存之外，**used_memory_rss**还包括进程运行本身需要的内存、内存碎片等，但是不包括虚拟内存

> 当我们看到这里第一直觉认为`使用used_memory_rss`减去`used_memory`就是内存碎片和本身所使用到的内存和, 但是 他不包含虚拟内存的数值, 也就是错误的, 



### mem_fragmentation_ratio

内存碎片比率，该值是used_memory_rss / used_memory的比值

> mem_fragmentation_ratio一般大于1，且该值越大，内存碎片比例越大。`mem_fragmentation_ratio<1，说明Redis使用了虚拟内存，由于虚拟内存的媒介是磁盘，比内存速度要慢很多，当这种情况出现时，应该及时排查`，如果内存不足应该及时处理，如增加Redis节点、增加Redis服务器的内存、优化应用等



### mem_allocator

Redis使用的内存分配器，在编译时指定；可以是 libc 、jemalloc或者tcmalloc

> 默认是jemalloc
>
> tcmalloc 是谷歌的内存分配器, 需要自己安装后在make阶段指定
>
> jemalloc 在2.4.4包含及以后新版本中已经包含在redis中, 会默认使用
>
> libc 旧版本如果不现在其他分配器默认使用



