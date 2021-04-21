# SpringCloud Config



> 版本:
>
> springCloud 下 项目 均为 `2.0.0.RELEASE`
>
> springBoot 版本`2.0.6.RELEASE`

## server 端



### POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>wechat-server</artifactId>
        <groupId>com.starrank</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>wechat-config-server</artifactId>
    <version>1.0.0</version>

    <!-- project properties -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.source>1.8</maven.compiler.source>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>

        <!-- config server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <!-- config and eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>com.squareup.okhttp3</groupId>
            <artifactId>okhttp</artifactId>
            <version>3.8.1</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```



### bootstrap.yml

```yml
#config of server port
server:
  port: 26999


#spring-cloud config
spring:
  application:
    name: wechat-config-server
  profiles:
    active: dev
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/panda_soft/wechat-config.git
          username: 21044903@qq.com                                #git 用户名
          password: yiqizou89
      label: ${spring.profiles.active} # label git分支
      profile: ${spring.profiles.active} # spring-profiles 选择

---
spring:
  profiles: dev
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        hostname: localhost
        serviceName: ${spring.application.name}
        instance-id: ${spring.application.name}:${spring.cloud.consul.discovery.hostname}:${server.port}
        healthCheckPath: /health/check
        healthCheckInterval: 10s

---
spring:
  profiles: test
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        hostname: localhost
        serviceName: ${spring.application.name}
        instance-id: ${spring.application.name}:${spring.cloud.consul.discovery.hostname}:${server.port}
        healthCheckPath: /health/check
        healthCheckInterval: 10s

```



### ConfigServerApplication

```java
@EnableConfigServer
@SpringBootApplication
public class ConfigServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(WechatConfigServerApplication.class, args);
    }
}
```





## Client 端



### POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>wechat-server</artifactId>
        <groupId>com.starrank</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>wechat-gateway-server</artifactId>
    <version>1.0.0</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 配置客户端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>
    </dependencies>

</project>
```



### bootstrap.yml

```yml
#config of server port
server:
  port: 26999


#spring-cloud config
spring:
  application:
    name: wechat-config-server
  profiles:
    active: dev
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/panda_soft/wechat-config.git
          username: 21044903@qq.com                                #git 用户名
          password: yiqizou89
      label: ${spring.profiles.active} # label git分支
      profile: ${spring.profiles.active} # spring-profiles 选择

---
spring:
  profiles: dev
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        hostname: localhost
        serviceName: ${spring.application.name}
        instance-id: ${spring.application.name}:${spring.cloud.consul.discovery.hostname}:${server.port}
        healthCheckPath: /health/check
        healthCheckInterval: 10s

---
spring:
  profiles: test
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        hostname: localhost
        serviceName: ${spring.application.name}
        instance-id: ${spring.application.name}:${spring.cloud.consul.discovery.hostname}:${server.port}
        healthCheckPath: /health/check
        healthCheckInterval: 10s

```



### GatewayServerApplication

```java
@EnableDiscoveryClient
@SpringBootApplication
public class WechatGatewayServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(WechatGatewayServerApplication.class, args);
    }
}
```

