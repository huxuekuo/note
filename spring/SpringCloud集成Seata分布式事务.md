# SpringCloud集成Seata分布式事务



## Seata 下载地址

> https://github.com/seata/seata/releases

下面案例, 我使用的是 1.4.0 最新版本



## 修改Seata配置信息



### registry.conf修改

`seata/conf/registry.conf` 我这里采用了consul为注册中心和配置中心

```JSON
registry {
  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
  type = "consul"
  loadBalance = "RandomLoadBalance"
  loadBalanceVirtualNodes = 10

  consul {
    cluster = "default"
    serverAddr = "127.0.0.1:8500"
  }
  
}

config {
  # file、nacos 、apollo、zk、consul、etcd3
  type = "consul"
 
  consul {
    serverAddr = "127.0.0.1:8500"
  }
}

```

