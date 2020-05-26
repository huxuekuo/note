# BUG 记录



## [ERROR: Maven JVM terminated unexpectedly with exit code 137](https://www.cnblogs.com/zzb-yp/p/11890976.html)(maven内存不足)



一、问题原因

　　jenkins编译的时候报错：ERROR: Maven JVM terminated unexpectedly with exit code 137，因为maven设置的内存不足，



二、问题解决方法一

　　1、找到linux中maven安装目录

　　[root@]# whereis mvn

　　编辑mvn文件　　[root@]# vim mvn

　　2、在mvn配置文件中添加

　　MAVEN_OPTS="$MAVEN_OPTS -Xms256m -Xmx512m -XX:MaxPermSize=128m -XX:ReservedCodeCacheSize=64m"