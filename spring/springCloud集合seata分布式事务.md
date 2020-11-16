# Seata(Alibaba分布式事务解决方案)



## 介绍



Seata的设计是对业务无侵入, 因为从业务无侵入2PC方案着手,在`传统2PC`的基础上演进,他把一个分布式事务理解成一个包含若干个分支的事物.全局事务的职责是协调其下管辖的分支事务达成一致,要么一起成功提交,要摸一起失败回滚,  此外,通常分支事物本身就是一个关系数据库的本地事务

![image text](/Users/lhy/Documents/note/images/affair_manager.jpg)



## seata 组成

- TC: 







## seata  执行流程



