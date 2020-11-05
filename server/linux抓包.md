

# tcpdump



<font color="read">tcpdump prot 6379 </font> 抓区指定端口的数据



### 来源主机+端口+TCP

监听来自主机`123.207.116.169`在端口`22`上的TCP数据包

<font color="read">tcpdump tcp port 22 and src host 123.207.116.169</font>

### 监听特定主机之间的通信

tcpdump ip host 210.27.48.1 and 210.27.48.2

`210.27.48.1`除了和`210.27.48.2`之外的主机之间的通信

tcpdump ip host 210.27.48.1 and ! 210.27.48.2

### 稍微详细点的例子

(1)tcp: ip icmp arp rarp 和 tcp、udp、icmp这些选项等都要放到第一个参数的位置，用来过滤数据报的类型
(2)-i eth1 : 只抓经过接口eth1的包
(3)-t : 不显示时间戳
(4)-s 0 : 抓取数据包时默认抓取长度为68字节。加上-S 0 后可以抓到完整的数据包
(5)-c 100 : 只抓取100个数据包
(6)dst port ! 22 : 不抓取目标端口是22的数据包
(7)src net 192.168.1.0/24 : 数据包的源网络地址为192.168.1.0/24
(8)-w ./target.cap : 保存成cap文件，方便用ethereal(即wireshark)分析

### 限制抓包的数量

如下，抓到1000个包后，自动退出

```bash
tcpdump -c 1000
```

### 保存到本地

备注：tcpdump默认会将输出写到缓冲区，只有缓冲区内容达到一定的大小，或者tcpdump退出时，才会将输出写到本地磁盘

```bash
tcpdump -n -vvv -c 1000 -w /tmp/tcpdump_save.cap
```

也可以加上`-U`强制立即写到本地磁盘（一般不建议，性能相对较差）

## 实战案例

先看下面一个比较常见的部署方式，在服务器上部署了nodejs server，监听3000端口。nginx反向代理监听80端口，并将请求转发给nodejs server（`127.0.0.1:3000`）。

> 浏览器 -> nginx反向代理 -> nodejs server

问题：假设用户(183.14.132.117)访问浏览器，发现请求没有返回，该怎么排查呢？

步骤一：查看请求是否到达nodejs server -> 可通过日志查看。

步骤二：查看nginx是否将请求转发给nodejs server。

```bash
tcpdump port 8383 
```

这时你会发现没有任何输出，即使nodejs server已经收到了请求。因为nginx转发到的地址是127.0.0.1，用的不是默认的interface，此时需要显示指定interface

```bash
tcpdump port 8383 -i lo
```

备注：配置nginx，让nginx带上请求侧的host，不然nodejs server无法获取 src host，也就是说，下面的监听是无效的，因为此时对于nodejs server来说，src host 都是 127.0.0.1

```bash
tcpdump port 8383 -i lo and src host 183.14.132.117
```

步骤三：查看请求是否达到服务器

```bash
tcpdump -n tcp port 8383 -i lo and src host 183.14.132.117
```



## 相关链接

tcpdump 很详细的
http://blog.chinaunix.net/uid-11242066-id-4084382.html

http://www.cnblogs.com/ggjucheng/archive/2012/01/14/2322659.html
Linux tcpdump命令详解

Tcpdump usage examples（推荐）
http://www.rationallyparanoid.com/articles/tcpdump.html

使用TCPDUMP抓取HTTP状态头信息
http://blog.sina.com.cn/s/blog_7475811f0101f6j5.html