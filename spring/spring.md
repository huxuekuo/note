# spring

### spring的执行流程

### spring的优点

### spring的缺点

### spring可以做什么

### spring事务的传播



spring的传播行为有7中

1.required 如果当前存在事务则在当前事务运行,如果当前事务不存在则新建事务

2.required_new 新建事务

3. Supports 支持事务,如果当前事务存在则在当前事务运行,如果当前事务不存在则以非事务运行
4. mandatory 如果当前事务存在则在当前事务运行,如果当前事务不存在抛出异常
5. not supports 以非事务运行,如果当前存在事务,将事务挂起
6. Never  以非事务运行,如果当前存在事务抛出异常
7. Nester 如果当前事务存在则嵌套在当前事务运行,如果当前事务不存在则与required类型

### spring的隔离级别

spring中有5中隔离级别,分别对应数据库的ACID 还有一个就是使用数据库的隔离级别