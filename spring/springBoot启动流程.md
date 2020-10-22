# springBoot启动流程



我们讲解SpringBoot启动流程以一段代码作为开端进行分析

```java
@SpringBootApplication
public class ReceptionCarApplication {
    public static void main(String[] args) {
        SpringApplication.run(ReceptionCarApplication.class, args);
}
```



启动有两块是需要进行分析解析的一个`@SpringBootApplication` 和 `SpringApplication.run()` 我们先解析run都做了什么,然后解析注解的作用



## Run 方法究竟都做了什么



`Run` 方法追踪下去我们可以看到以下代码

```java
 public static ConfigurableApplicationContext run(Class<?>[] primarySources, String[] args) {
        return (new SpringApplication(primarySources)).run(args);
    }
```

