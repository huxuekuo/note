

# 报错内容



```
2020-05-23 09:09:41.439 ERROR org.apache.juli.logging.DirectJDKLog 182 log -  ulimit -n
	at sun.nio.ch.ServerSocketChannelImpl.accept0(Native Method) ~[?:1.8.0_45]
	at sun.nio.ch.ServerSocketChannelImpl.accept(ServerSocketChannelImpl.java:422) ~[?:1.8.0_45]
	at sun.nio.ch.ServerSocketChannelImpl.accept(ServerSocketChannelImpl.java:250) ~[?:1.8.0_45]
	at org.apache.tomcat.util.net.NioEndpoint$Acceptor.run(NioEndpoint.java:482) [tomcat-embed-core-8.5.34.jar!/:8.5.34]
	at java.lang.Thread.run(Thread.java:745) [?:1.8.0_45]

2020-05-23 09:09:43.039 ERROR org.apache.juli.logging.DirectJDKLog 182 log - Socket accept failed java.io.IOException: Too many open files
	at sun.nio.ch.ServerSocketChannelImpl.accept0(Native Method) ~[?:1.8.0_45]
	at sun.nio.ch.ServerSocketChannelImpl.accept(ServerSocketChannelImpl.java:422) ~[?:1.8.0_45]
	at sun.nio.ch.ServerSocketChannelImpl.accept(ServerSocketChannelImpl.java:250) ~[?:1.8.0_45]
	at org.apache.tomcat.util.net.NioEndpoint$Acceptor.run(NioEndpoint.java:482) [tomcat-embed-core-8.5.34.jar!/:8.5.34]
	at java.lang.Thread.run(Thread.java:745) [?:1.8.0_45]

2020-05-23 09:09:44.640 ERROR org.apache.juli.logging.DirectJDKLog 182 log - Socket accept failed java.io.IOException: Too many open files
	at sun.nio.ch.ServerSocketChannelImpl.accept0(Native Method) ~[?:1.8.0_45]
	at sun.nio.ch.ServerSocketChannelImpl.accept(ServerSocketChannelImpl.java:422) ~[?:1.8.0_45]
	at sun.nio.ch.ServerSocketChannelImpl.accept(ServerSocketChannelImpl.java:250) ~[?:1.8.0_45]
	at org.apache.tomcat.util.net.NioEndpoint$Acceptor.run(NioEndpoint.java:482) [tomcat-embed-core-8.5.34.jar!/:8.5.34]
	at java.lang.Thread.run(Thread.java:745) [?:1.8.0_45]

```



线上突然间报错出现这样的问题,一时间不知道什么原因,最重要的一句话

<font color="read"> Socket accept failed java.io.IOException: Too many open files</font>

