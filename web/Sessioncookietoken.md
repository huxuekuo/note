# Session/cookie/token



## cookie

​	cookie是一个非常具体的东西,就是浏览器中可以永久储存的一种数据,仅仅是浏览器存储的一种存储的功能,

​	cookie由服务器生产,发送给浏览器,浏览器以KV的方式存储在某个目录下的文件内,下次请求同一个网站会将cookie发送到服务器,cookie的个数是有限制的,这个限制每个浏览器都不太一样,就常用的google是50个



## session

​	session是基于cookie的,session的一种服务器端会话技术,就好像有在与一个人交谈但是你怎么知道你是在跟李四聊天还是在跟张三聊天,对方肯定有某种特征（长相等）表明他就是张三。

​	就拿登录来说,如果李四登录了系统,就分配一个Session给李四,然后每次发送请求都带有这个session,服务器知道这个请求是谁发来的了,有一个缺陷如果web做了负载均衡,那么下一个操作请求到了另一台服务器的时候session会丢失。



## 负载均衡中Session



## 分布式中Session



## token



