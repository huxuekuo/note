# consul



## Consul 介绍

------



**Consul 使用GO 语言编写,**



### Consul 所需端口

------



| Use                                                          | Default Ports    |
| :----------------------------------------------------------- | :--------------- |
| DNS: The DNS server (TCP and UDP)                            | 8600             |
| HTTP: The HTTP API (TCP Only)                                | 8500             |
| HTTPS: The HTTPs API                                         | disabled (8501)* |
| gRPC: The gRPC API                                           | disabled (8502)* |
| LAN Serf: The Serf LAN port (TCP and UDP)                    | 8301             |
| Wan Serf: The Serf WAN port (TCP and UDP)                    | 8302             |
| server: Server RPC address (TCP Only)                        | 8300             |
| Sidecar Proxy Min: Inclusive min port number to use for automatically assigned sidecar service registrations. | 21000            |
| Sidecar Proxy Max: Inclusive max port number to use for automatically assigned sidecar service registrations. | 21255            |



**`DNS Interface`** Used to resolve DNS queries. (用于解析DNS查询)



**`HTTP API`** This is used by clients to talk to the HTTP API.(客户端使用它与httpapi进行通信)



**`HTTPS API`** (Optional) Is off by default, but port 8501 is a convention used by various tools as the default.(（可选）默认为off，但端口8501是各种工具默认使用的约定)



**`gRPC API`** (Optional). Currently gRPC is only used to expose the xDS API to Envoy proxies. It is off by default, but port 8502 is a convention used by various tools as the default. Defaults to 8502 in `-dev` mode. （可选）。目前gRPC仅用于向特使代理公开xdsapi。它在默认情况下是关闭的，但是端口8502是一个被各种工具用作默认值的约定。在“-dev”模式下默认为8502



**`Serf LAN`** This is used to handle gossip in the LAN. Required by all agents. 这是用来处理局域网中的流言。所有代理人都要求。



**`Serf WAN`** This is used by servers to gossip over the WAN, to other servers. As of Consul 0.8 the WAN join flooding feature requires the Serf WAN port (TCP/UDP) to be listening on both WAN and LAN interfaces. See also: [Consul 0.8.0 CHANGELOG](https://github.com/hashicorp/consul/blob/master/CHANGELOG.md#080-april-5-2017) and [GH-3058](https://github.com/hashicorp/consul/issues/3058) 这被服务器用来通过广域网与其他服务器闲聊。从consul0.8开始，WAN连接泛洪功能要求Serf WAN端口（TCP/UDP）同时监听WAN和LAN接口



**`Server RPC`** This is used by servers to handle incoming requests from other agents. 服务器使用它来处理来自其他代理的传入请求。



## Consul 常用

------



## 命令介绍



## Consul 特性



## Consul 角色





## Consul 工作原理



## 入门案例



## Consul 集群