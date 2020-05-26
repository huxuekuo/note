# 安装Redis 6.0.3

```
[root@master ~]# ls
anaconda-ks.cfg  redis-6.0.1.tar.gz
[root@master ~]# tar xf redis-6.0.1.tar.gz 
[root@master ~]# cd redis-6.0.1/
[root@master ~]# make && make install
```



```
开启服务(守护进程)
1,设置daemonize为yes
```



# 问题

## /bin/sh: cc: command not found

```
[root@master redis-6.0.1]# make
……
make[3]: cc: Command not found
make[3]: *** [net.o] Error 127
make[3]: Leaving directory `/root/redis-6.0.1/deps/hiredis'
make[2]: *** [hiredis] Error 2
make[2]: Leaving directory `/root/redis-6.0.1/deps'
make[1]: [persist-settings] Error 2 (ignored)
    CC adlist.o
/bin/sh: cc: command not found
make[1]: *** [adlist.o] Error 127
make[1]: Leaving directory `/root/redis-6.0.1/src'
make: *** [all] Error 2
```

解决办法：

```
yum -y install gcc
```



## 报错2：server.c:xxxx:xx: error: ‘xxxxxxxx’ has no member named ‘xxxxx’

```
[root@master redis-6.0.1]# make
……
server.c:5101:19: error: ‘struct redisServer’ has no member named ‘sofd’
         if (server.sofd > 0)
                   ^
server.c:5102:94: error: ‘struct redisServer’ has no member named ‘unixsocket’
             serverLog(LL_NOTICE,"The server is now ready to accept connections at %s", server.unixsocket);
                                                                                              ^
server.c:5103:19: error: ‘struct redisServer’ has no member named ‘supervised_mode’
         if (server.supervised_mode == SUPERVISED_SYSTEMD) {
                   ^
server.c:5104:24: error: ‘struct redisServer’ has no member named ‘masterhost’
             if (!server.masterhost) {
                        ^
server.c:5117:15: error: ‘struct redisServer’ has no member named ‘maxmemory’
     if (server.maxmemory > 0 && server.maxmemory < 1024*1024) {
               ^
server.c:5117:39: error: ‘struct redisServer’ has no member named ‘maxmemory’
……
```

解决办法：

```
# 查看gcc版本是否在5.3以上，centos7.6默认安装4.8.5
gcc -v
# 升级gcc到5.3及以上,如下：
升级到gcc 9.3：
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
scl enable devtoolset-9 bash
需要注意的是scl命令启用只是临时的，退出shell或重启就会恢复原系统gcc版本。
如果要长期使用gcc 9.3的话：

echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile
这样退出shell重新打开就是新版的gcc了
以下其他版本同理，修改devtoolset版本号即可。
```



# 其他命令

```
# 编译出错时，清出编译生成的文件
make distclean
# 编译安装到指定目录下
make PREFIX=/usr/local/redis install 
# 卸载
make uninstal
```



# 设置密码

