# Web容器加载流程



## 大概加载流程(💪)



1. 启动一个WEB项目,WEB项目先回去读取他的配置文件web.xml,读取<context-param>和<listener>两个节点.

2. 接着,容器创建一个ServletContext上下文,这个web项目的所有部分都将共享这个上下文

3. 容器将<context-param>转换为键值对,并交给ServletContext.

4. 容器将创建<listener>中的实例,创建监听器.

   

   <u>**Context-param —> listener —> fileter —> servlet** </u>

   

   

## load-on-startup



Load-on-startup 元素在web应用启动的时候创建了servlet被加载的顺序,他的值必须是一个整数,

**如果他的值是一个负整数或者这个元素不存在,那么该servlet会在被调用的时候加载,**

如果他的值是一个正整数或者零,容器在配置的时候就加载并初始化这个servlet,

1. **容器必须保证值小的先被加载**
2. **如果load-on-startup值相等,那么容器自动项目加载**



## 没有先后顺序关系吗?

​		

​		虽然是web容器是有一套自己的加载流程的,但是对于某类而言比如,filter为例,web.xml中可以定义多个filter,与filter相关的配置节点是filter-mapping,这里一定注意,对于拥有filter-name的filter和filter-Mapping配置节点而言,filter-mapping必须在filter以后,是按照 filter 配置节出现的顺序来初始化的，当请求资源匹配多个 filter-mapping 时，filter 拦截资源是按照 filter-mapping 配置节出现的顺序来依次调用 doFilter() 方法的。

servlet 同 filter 类似 ，此处不再赘述。