# Mysql中的锁



## 行锁 表锁



### 简介

​	行锁与表锁在每个引擎中实现是不一致的,在MyISAM中只支持表锁,不支持行锁,在InnoDB中都支持

​	我们本文就通过Innodb与Myisam讲解行锁与表锁

行锁



​	

## 乐观锁 悲观锁



### 悲观锁



#### 简介



​	悲观锁的实现是根据数据库的锁机制实现,对于除当前事务以外的事务都是保守状态

​	实现悲观锁需要使用到 **SELECT … FROM UPDATE**语句,将选中一行的数据加锁,

​	如果其他的事务也想对本行数据进行锁定,就会出现互斥的(需要等待锁被释放以后才可以进行操作)

​	**SELECT … FROM UPDATE** 获取行锁,当前事务结束,行锁也会释放掉,因此必须在事务中使用,

​	但是Mysql会自动提交事务,所以所选将**autocomment设置为0,**变成手动提交事务的基础上进行悲观锁



#### 不同数据的实现



​	在oralce不支持**SELECT … FROM UPDATE**但是支持**SELECT … FROM UPDATE ON WAIT** 如果要加锁的行已经被其他的事务锁住不会等待,会报错,mysql 没有 **ON WAIT**选项,但是mysql中有一个问题SELECT … FROM UPDATE 扫描到的行都会上锁,很容易造成问题,这一点很容易产生问题,在使用一定要确定是否使用的了索引,而不是全表扫描



#### mysql悲观锁执行流程图

​	![image-20190618180107835](/Users/hu/Library/Application Support/typora-user-images/image-20190618180107835.png)



#### 代码实现

~~~ mysql


CREATE DATABASE goods_test; -- 创建数据库
use goods_test; -- 使用表

CREATE TABLE goods( -- 创建表
	id int(11) primary key auto_increment,
    statuc int(11)
);

alter table goods  change statuc `status` int(11);

CREATE TABLE orders(
	id int(11) primary key auto_increment,
    `goods_id` int(11) 
);

set autocommit = 0;  -- 设置手动提交

INSERT INTO goods(id,status)VALUES(null,1);

SELECT * FROM goods;

-- 开始事务
start Transaction;
-- 1.查询出商品信息

select status from goods where status = 1 FOR UPDATE;

 -- 2.根据商品信息生成订单

insert into orders (id,goods_id) values (null,1);

-- 3.修改商品status为2

update goods set status=1;
commit; -- 提交事务
~~~



#### 优缺点

1. 悲观锁不一定适合各种环境,因为悲观锁需要**数据库锁机制实现**,如果锁的时间长了,其他用户长时间无法访问,影响了程序的并发访问性,



### 乐观锁



乐观锁相对悲观锁而言,乐观锁不会真正的加锁,是在表中建立一个字段去分版本,如果在更新数据之前,先