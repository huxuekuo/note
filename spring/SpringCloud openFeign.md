

# OpenFeign application 配置说明

| Name                                         | Default                                         | Description                                                  |
| -------------------------------------------- | ----------------------------------------------- | ------------------------------------------------------------ |
| feign.autoconfiguration.jackson.enabled      | `false`                                         | 如果为true，则将提供PageJacksonModule和SortJacksonModule bean用于Jackson页面解码. |
| feign.circuitbreaker.enabled                 | `false`                                         | 如果为true，则将使用Spring Cloud CircuitBreaker断路器包装OpenFeign客户端. |
| feign.client.config                          |                                                 |                                                              |
| feign.client.decode-slash                    | `true`                                          | 伪客户端默认情况下不对斜杠“ /”字符进行编码。 要更改此行为，请将“ decodeSlash”设置为“ false”. |
| feign.client.default-config                  | `default`                                       |                                                              |
| feign.client.default-to-properties           | `true`                                          |                                                              |
| feign.compression.request.enabled            | `false`                                         | 允许压缩Feign发送的请求.                                     |
| feign.compression.request.mime-types         | `[text/xml, application/xml, application/json]` | 支持的mime类型列表.                                          |
| feign.compression.request.min-request-size   | `2048`                                          | 最小阈值内容大小.                                            |
| feign.compression.response.enabled           | `false`                                         | 可以压缩来自Feign的响应.                                     |
| feign.compression.response.useGzipDecoder    | `false`                                         | 启用要使用的默认gzip解码器.                                  |
| feign.encoder.charset-from-content-type      | `false`                                         | 指示字符集是否应从{@code Content-Type}标头派生.              |
| feign.httpclient.connection-timeout          | `2000`                                          |                                                              |
| feign.httpclient.connection-timer-repeat     | `3000`                                          |                                                              |
| feign.httpclient.disable-ssl-validation      | `false`                                         |                                                              |
| feign.httpclient.enabled                     | `true`                                          | 允许通过Feign使用Apache HTTP客户端.                          |
| feign.httpclient.follow-redirects            | `true`                                          |                                                              |
| feign.httpclient.hc5.enabled                 | `false`                                         | 允许通过Feign使用Apache HTTP Client 5.                       |
| feign.httpclient.hc5.pool-concurrency-policy |                                                 | 池并发策略.                                                  |
| feign.httpclient.hc5.pool-reuse-policy       |                                                 | 池连接重用策略.                                              |
| feign.httpclient.hc5.socket-timeout          | `5`                                             | 套接字超时的默认值.                                          |
| feign.httpclient.hc5.socket-timeout-unit     |                                                 | 套接字超时单位的默认值.                                      |
| feign.httpclient.max-connections             | `200`                                           |                                                              |
| feign.httpclient.max-connections-per-route   | `50`                                            |                                                              |
| feign.httpclient.time-to-live                | `900`                                           |                                                              |
| feign.httpclient.time-to-live-unit           |                                                 |                                                              |
| feign.metrics.enabled                        | `true`                                          | 启用伪装的指标功能.                                          |
| feign.okhttp.enabled                         | `false`                                         | 允许通过Feign使用OK HTTP客户端.                              |



# 配置信息



## feign.httpclient.enabled(开启连接池)



需要引入两个依赖jar包后才能开启



```xml
        <dependency>
            <groupId>io.github.openfeign</groupId>
            <artifactId>feign-httpclient</artifactId>
        </dependency>
       <dependency>
				<groupId>org.apache.httpcomponents</groupId>
				<artifactId>httpclient</artifactId>
				<version>4.5.3</version>
			</dependency>

 <!-- 下面的jar包应该暂时不需要 -->
		<dependency>
				<groupId>org.apache.httpcomponents</groupId>
				<artifactId>httpcore</artifactId>
				<version>4.4.8</version>
			</dependency>
			<dependency>
				<groupId>org.apache.httpcomponents</groupId>
				<artifactId>httpmime</artifactId>
				<version>4.5.3</version>
			</dependency>
```





# Feign性能调优



## Gzip压缩

 

> - gzip介绍
>   - gzip 是一种数据格式, 采用deflate算法压缩数据, gzip是一种流行的文件压缩算法, 应用十分广泛, 尤其是Linux品台
> - gzip能力
>   - 当Gzip压缩一个纯文本是, 效果是非常明显的, 大约可以减少70%以上的文件大小
> - gzip作用
>   - 网络数据讲过压缩后实际上降低了网络传输的字节数,最明显的好处就是可以加载网页速度,除了节省流量, 改善用户的浏览体验外, 另一个潜在的好处是Gzip与搜素引擎的抓取工具有着更好的关系



## Feign 请求超时设置



Feign 引入了ribbon依赖, 有两种设置超时时间的方式



- 全局方式

  - 直接设置Ribbon配置

    - ```yml
      #ribbon config
      ribbon:
        # 读取超时时间
        ReadTimeout: 10000
        # 连接超时时间
        ConnectTimeout: 10000
        # 是否对所有请求都进行重试
        OkToRetryOnAllOperations: false
        # 对当前实例重试次数
        MaxAutoRetries: 1
        # 切换实例重试次数
        MaxAutoRetriesNextServer: 2
      ```

- 针对某些服务方式

  - ```yml
    
    serverName:
      #ribbon config
      ribbon:
        # 读取超时时间
        ReadTimeout: 10000
        # 连接超时时间
        ConnectTimeout: 10000
        # 是否对所有请求都进行重试
        OkToRetryOnAllOperations: false
        # 对当前实例重试次数
        MaxAutoRetries: 1
        # 切换实例重试次数
        MaxAutoRetriesNextServer: 2
    ```





1 - 2 - 3 