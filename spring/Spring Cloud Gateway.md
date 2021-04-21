# 概述

> springcloudgateway旨在提供一种简单而有效的方法路由到api，并为它们提供跨领域的关注点，例如：安全性、监视/度量和恢复能力。



# Gateway 关键词



- **Route(路由)**
  - 网关的基本构造块。它由一个ID、一个目标URI、一组谓词和一组筛选器定义。如果聚合谓词为true，则匹配路由
- **Predicate(谓语)**
  - 这是一个Java8函数谓词。输入类型是Spring Framework ServerWebExchange。这允许您匹配来自HTTP请求的任何内容，例如头或参数
- **Filter(过滤器)**
  - 这些是用特定工厂构建的Spring框架GatewayFilter的实例。在这里，您可以在发送下游请求之前或之后修改请求和响应。



# Gateway 提供的功能



- 性能API高可用,负载均衡,容错机制
- 安全: 权限身份认证. 脱敏, 流量清洗, 后端签名(保证全链路可信调用), 黑名单
- 日志: 日志记录, 一旦涉及分布式,全链路跟踪必不可少
- 缓存: 数据缓存
- 监控: 记录请求响应数据, API耗时分析,性能监控
- 限流: 流量控制,错峰流控,可以定义多种限流规则
- 灰度: 线上灰度部署,可以减少风险
- 路由:动态路由规则



## 代替方案



- Nginx + lua
  - Nginx适合做门户网关, 是作为整个全全局网关, 对外的处于最外层, 而Gateway属于业务网关, 主要用来对应不同客户端提供服务, 用于聚合业务,职责单一
  - Gateway可以实现熔断,重试等功能, Nginx不具备
- ..zuul 已经死了🐶



## 遇到的问题



```java
org.springframework.cloud.gateway.config.GatewayAutoConfiguration required 
a bean of type 'org.springframework.http.codec.ServerCodecConfigurer' 
that could not be found
```



> 将pom.xml中关于spring-boot-start-web模块的jar依赖去掉。
>
> 错误分析：
>
> 根据上面描述（Description）中信息了解到GatewayAutoConfiguration这个配置中找不到ServerCodecConfig这个Bean。
>
> spring cloud gateway server项目是一个spring boot项目，在启动的时候会去加载它的配置，其中有一个叫做GatewayClassPathWarningAutoConfiguration的配置类中有这么一行代码：
>
> ```
> @Configuration
> 
> @ConditionalOnClass(name = "org.springframework.web.servlet.DispatcherServlet")
> protected static class SpringMvcFoundOnClasspathConfiguration {
> 
> 
>   public SpringMvcFoundOnClasspathConfiguration() {
> 
>    log.warn(BORDER+"Spring MVC found on classpath, which is incompatible with Spring Cloud 
> 
>    Gateway at this time. "+
> 
>    "Please remove spring-boot-starter-web dependency."+BORDER);
> 
>   }
> }
> ```
>
> 在类路径上找到的Spring MVC，此时它与Spring Cloud网关不兼容。请删除spring-boot-start-web依赖项。
>
> 因为spring cloud gateway是基于webflux的，如果非要web支持的话需要导入spring-boot-starter-webflux而不是spring-boot-start-web。

