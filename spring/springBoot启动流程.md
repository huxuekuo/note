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

初始化了很多属性, 比较有意思的在下面重点说一下



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



其中 `DelegatingApplicationContextInitializer`  



### this.setListeners()



使用 `SpringFactoriesLoader`查找并加载 classpath下 `META-INF/spring.factories`文件中的所有可用的 `ApplicationListener`



`ApplicationListener` 是springBoot 提供的一个用于监听应用事件的事件监听器, 它继承自Java标准观察者模式的`EventListener`接口

一个`ApplicationListener`实现可以声明自己所关心的事件类型

事件有Spring的内置事件,还可以自定义事件



#### 内置事件

| 内置事件名称              | **描述**                                                     |
| ------------------------- | ------------------------------------------------------------ |
| **ContextRefreshedEvent** | ApplicationContext 被初始化或刷新时，该事件被发布。这也可以在 ConfigurableApplicationContext接口中使用 refresh() 方法来发生。此处的初始化是指：所有的Bean被成功装载，后处理Bean被检测并激活，所有Singleton Bean 被预实例化，ApplicationContext容器已就绪可用 |
| **ContextStartedEvent**   | 当使用 ConfigurableApplicationContext （ApplicationContext子接口）接口中的 start() 方法启动 ApplicationContext 时，该事件被发布。你可以调查你的数据库，或者你可以在接受到这个事件后重启任何停止的应用程序。 |
| **ContextStoppedEvent**   | 当使用 ConfigurableApplicationContext 接口中的 stop() 停止 ApplicationContext 时，发布这个事件。你可以在接受到这个事件后做必要的清理的工作。 |
| **ContextClosedEvent**    | 当使用 ConfigurableApplicationContext 接口中的 close() 方法关闭 ApplicationContext 时，该事件被发布。一个已关闭的上下文到达生命周期末端；它不能被刷新或重启 |
| **RequestHandledEvent**   | 这是一个 web-specific 事件，告诉所有 bean HTTP 请求已经被服务。只能应用于使用DispatcherServlet的Web应用。在使用Spring作为前端的MVC控制器时，当Spring处理用户请求结束后，系统会自动触发该事件。 |
|                           |                                                              |



#### 自定义事件案例



##### 定义事件类型



```java
public class MyApplicationEvent extends ApplicationEvent {

    private JSONObject jsonObject;

    public JSONObject getJsonObject() {
        return jsonObject;
    }

    public MyApplicationEvent(Object source) {
        super(source);
        jsonObject = new JSONObject();
        jsonObject.put("s", "s");
    }
}
```



##### 自定义监听器

```java
@Component
public class MyApplicationListener implements ApplicationListener<MyApplicationEvent> {

    @Override
    public void onApplicationEvent(MyApplicationEvent requestHandledEvent) {
        System.out.println("Event->"+requestHandledEvent.getJsonObject());
    }

}
```



##### 发布自定义事件信息

```java

```



### this.deduceMainApplicationClass()



```java
private Class<?> deduceMainApplicationClass() {
		try {
			StackTraceElement[] stackTrace = new RuntimeException().getStackTrace();
			for (StackTraceElement stackTraceElement : stackTrace) {
				if ("main".equals(stackTraceElement.getMethodName())) {
					return Class.forName(stackTraceElement.getClassName());
				}
			}
		}
		catch (ClassNotFoundException ex) {
			// Swallow and continue
		}
		return null;
	}
```



从上向下, 获取栈信息. 判断并设置main方法的定义类



