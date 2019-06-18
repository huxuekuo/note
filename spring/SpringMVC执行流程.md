# SpringMVC执行流程

![image-20190617235817291](/Users/hu/Library/Application Support/typora-user-images/image-20190617235817291.png)



1. DispatchServlet拦截用户发来的请求,
2. DispatchServlet拿着请求找到HandlerMapping处理映射器,返回一个处理器对象给DispatchServlet对象
3. DispatchServlet拿着这个Controller对象找到处理适配器,
4. 处理适配器找到对象处理器,处理器返回一个ModelAndView给HandlerAdapter,
5. HandlerAdapter直接返回ModelAndView给前端控制器
6. DispatchServlet拿着ModelAndView找到视图解析器,视图解析器返回一个渲染好的页面给前端控制器,
7. 前端控制器直接返回给用户