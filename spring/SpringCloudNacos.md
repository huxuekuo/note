# Nacos

## 安装及启动Nacos

### 安装Nacos

下载/文档地址 : https://nacos.io/zh-cn/docs/quick-start.html



有两种方式可以安装Nacos

- 从GitHub 下载源码编译构建
- 下载编译后压缩包方式



我使用的事下载压缩包方式, 下载地址 : https://github.com/alibaba/nacos/releases



我使用的是Nacos `2.2.1版本`



### 启动Nacos

#### Linux/Unix/Mac

启动命令(standalone代表着单机模式运行，非集群模式):

```
sh startup.sh -m standalone
```

如果您使用的是ubuntu系统，或者运行脚本报错提示[[符号找不到，可尝试如下运行：

```
bash startup.sh -m standalone
```

#### Windows

启动命令(standalone代表着单机模式运行，非集群模式):

```
startup.cmd -m standalone
```



其余问题查询官方文档....





## Nacos 配置管理



对于Nacos配置管理, 通过namespace, group , DataId能够定位一个配置集

<img src="/Users/edz/Library/Application Support/typora-user-images/image-20210518164051335.png" alt="image-20210518164051335" style="zoom:50%;" />

### 基础应用



