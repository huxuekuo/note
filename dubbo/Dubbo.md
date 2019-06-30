# Dubbo



## dubbo简介

​		dubbo是一个RPC框架RPC远程服务调用的,dubbo采用了spring的配置方式,没有任何API侵入

## dubbo的缺省依赖

​		spring context

​		netty 

​		javassist



## dubbo的架构

![20180608113657_461](/Users/hu/Desktop/20180608113657_461.jpg)



dubbo一共分十层

service(服务层),

config(配置层),

proxy(代理层),

registry(注册层),

cluster(集群层)

## dubbo调用过程

​		

![send-request-process](/Users/hu/Downloads/send-request-process.jpg)

### 概括

​	服务的消费者(**client**)根据代理对象(**proxy**)发送远程调用请求,接着通过网络客户端将编码后的请求发给服务端的网络层上也就是(**Server**),Server接到请求后首先进行解码,解完码以后转交给分发器(**dispatcher**),再由分发器即将请求派遣到指定线程池上,最后由线程池调用具体服务,



### dubbo的调用方式

~~~ java
// 获取异步配置
            boolean isAsync = RpcUtils.isAsync(getUrl(), invocation);
            // isOneway 为 true，表示“单向”通信
            boolean isOneway = RpcUtils.isOneway(getUrl(), invocation);
            int timeout = getUrl().getMethodParameter(methodName, Constants.TIMEOUT_KEY, Constants.DEFAULT_TIMEOUT);

            // 异步无返回值
            if (isOneway) {
                boolean isSent = getUrl().getMethodParameter(methodName, Constants.SENT_KEY, false);
                // 发送请求
                currentClient.send(inv, isSent);
                // 设置上下文中的 future 字段为 null
                RpcContext.getContext().setFuture(null);
                // 返回一个空的 RpcResult
                return new RpcResult();
            } 

            // 异步有返回值
            else if (isAsync) {
                // 发送请求，并得到一个 ResponseFuture 实例
                ResponseFuture future = currentClient.request(inv, timeout);
                // 设置 future 到上下文中
                RpcContext.getContext().setFuture(new FutureAdapter<Object>(future));
                // 暂时返回一个空结果
                return new RpcResult();
            } 

            // 同步调用
            else {
                RpcContext.getContext().setFuture(null);
                // 发送请求，得到一个 ResponseFuture 实例，并调用该实例的 get 方法进行等待
                return (Result) currentClient.request(inv, timeout).get();
            }
        } catch (TimeoutException e) {
            throw new RpcException(..., "Invoke remote method timeout....");
        } catch (RemotingException e) {
            throw new RpcException(..., "Failed to invoke remote method: ...");
        }
~~~



dubbo的调用方式有两种

​	异步调用和同步调用

同步调用

  