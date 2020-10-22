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



## SpringApplication 构造过程



`Run` 方法追踪下去我们可以看到以下代码

```java
 public static ConfigurableApplicationContext run(Class<?>[] primarySources, String[] args) {
        return (new SpringApplication(primarySources)).run(args);
    }
```



这里`new SpringApplication(..)` 构造了一个spring应用, 具体看一下构造方法



```java
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
    this.sources = new LinkedHashSet();
    this.bannerMode = Mode.CONSOLE;
    this.logStartupInfo = true;
    this.addCommandLineProperties = true;
    this.headless = true;
    this.registerShutdownHook = true;
    this.additionalProfiles = new HashSet();
    this.isCustomEnvironment = false;
    this.resourceLoader = resourceLoader;
    Assert.notNull(primarySources, "PrimarySources must not be null");
    this.primarySources = new LinkedHashSet(Arrays.asList(primarySources));
  
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    this.setInitializers(this.getSpringFactoriesInstances(ApplicationContextInitializer.class));
    this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = this.deduceMainApplicationClass();
}
```

初始化了很多属性, 比较有意思的

### WebApplicationType.deduceFromClasspath()

` this.webApplicationType = WebApplicationType.deduceFromClasspath();` 创建的是 REACTIVE应用、SERVLET应用、NONE 三种中的某一种

```java
    static WebApplicationType deduceFromClasspath() {
        if (ClassUtils.isPresent("org.springframework.web.reactive.DispatcherHandler", (ClassLoader)null) && !ClassUtils.isPresent("org.springframework.web.servlet.DispatcherServlet", (ClassLoader)null) && !ClassUtils.isPresent("org.glassfish.jersey.servlet.ServletContainer", (ClassLoader)null)) {
            return REACTIVE;
        } else {
            String[] var0 = SERVLET_INDICATOR_CLASSES;
            int var1 = var0.length;

            for(int var2 = 0; var2 < var1; ++var2) {
                String className = var0[var2];
                if (!ClassUtils.isPresent(className, (ClassLoader)null)) {
                    return NONE;
                }
            }

            return SERVLET;
        }
    }
```



### this.setInitializers()

使用 `SpringFactoriesLoader`查找并加载 classpath下 `META-INF/spring.factories`文件中所有可用的 `ApplicationContextInitializer`



这里需要重点介绍一下 `ApplicationContextInitializer`



#### ApplicationContextInitializer



`ApplicationContextInitializer` 是spring容器刷新之前之前执行的一个回调函数是在`ConfigurableApplicationContext#refresh() `之前调用（当spring框架内部执行 `ConfigurableApplicationContext#refresh() `方法的时候或者在SpringBoot的run()执行时）



该接口使用的场景是web应用场景中需要初始化一些 ,注册属性源(property sources)



其中 `DelegatingApplicationContextInitializer` 比较特殊 我们需要详细看一看

??

??

??



### this.setListeners()



使用 `SpringFactoriesLoader`查找并加载 classpath下 `META-INF/spring.factories`文件中的所有可用的 `ApplicationListener`



`ApplicationListener` 是springBoot 提供的一个用于监听应用事件的事件监听器, 它继承自Java标准观察者模式的`EventListener`接口

一个`ApplicationListener`实现可以声明自己所关心的事件类型