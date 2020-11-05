# Spring在Web中的启动过程



## 宿主环境

​	对于web应用,其部署在web容器中,web容器提供一个全局上下文件环境,这个上下文就是servletContext,其后面的spring IOC容器提供宿环境



## contextLoaderListener

​	在web启动以后会触发容器初始化事件,contextLoaderListener会监听到这个事件,

​	初始化方法被调用,在这个方法中,spring会初始化一个启动上下文,这个上下文就是被称为根上下文,

​	webApplicationContext,这时一个接口类,实际实现类是XMLwebApplicationContext,这个就是Spring的IOC容器

  定义Bean配置由web.xml中的context-param标签指定,在这个ioc容器初始化完毕以后,

~~~ java
WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE = WebApplicationContext.class.getName() + ".ROOT";
~~~

<u>spring 以WebApplicationContext.ROOTWEBAPPLICATIONEXTATTRIBUTE 为属性key</u>

也就是用IOC容器为key存储在ServletContext中,



## 初始化Servlet

<!--下面提到的IOC容器就是固定格式的字符串-->

初始化DispatcherServlet是,在建立自己的上下文时,会向利用**IOC容器**去获取以前的根上下文,IOC容器作为自己的**parent上下文**,有了这个上下文以后再去初始化自己的上下文,初始化完毕以后用servlet的名字作为key存在servletContext之中,这样每个类都有自己的上下文,**每个servlet共享根上文的那些bean**





​	