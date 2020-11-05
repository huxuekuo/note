# Nacos



## Nacos 与 OpenJdk配合



### 1.修改/etc/profile文件JAVA_HOME

```shell
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64/jre
```



### 2.保存/etc/profile文件后需要使他生效

```shell
sorce /etc/profile
```



### 3.修改启动文件 JAVA_HOME配置信息

```shell
vim /home/nacos/bin/startup.sh 
​```
[ ! -e "$JAVA_HOME/bin/java" ] && JAVA_HOME=$JAVA_HOME
#[ ! -e "$JAVA_HOME/bin/java" ] && JAVA_HOME=/usr/java
#[ ! -e "$JAVA_HOME/bin/java" ] && JAVA_HOME=/opt/taobao/java
[ ! -e "$JAVA_HOME/bin/java" ] && unset JAVA_HOME
​```
```



