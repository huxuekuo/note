# docker设置固定IP

![image-20190629192125062](/Users/hu/Library/Application Support/typora-user-images/image-20190629192125062.png)

~~~shell
sudo docker network ls
~~~

​	使用上面的命令可以看到docker支持网络类型分别为:bridge,host,none三种类型

## docker网络类型介绍

### bridge(网桥模式)

​	这个模式是daocker默认的方式,所以每次docker容器重启时会按照顺序获取对应ip地址，这就导致容器每次重启，ip都发生变化

### none(无指定网络)

在启动docker的时候,可以通过—network=none,这样docker就不会给容器分配IP



### host(主机网络)



