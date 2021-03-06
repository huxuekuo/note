# Mysql聚集索引和非聚集索引,覆盖索引



<u>**「索引就像书的目录， 通过书的目录就准确的定位到了书籍具体的内容」**</u>



[TOC]



## 聚集索引

​			事实上一个表创建了主键就不能被称之为表了,一个没有加主键的表,他的数据将无序的存放在磁盘存储器上,一行一行的排列的很整齐,**如果加了索引,那么表在磁盘上的数据结构就由整齐的排列结构转换为了树状结构,也就是上面说的平衡树结构,换句话说整个表就变成一个索引,整个表变成一个索引,为什么一个表只能有一个聚集索引"因为主键的作用就是将表结构转换为平衡树结构"**

我们已经知道为什么这么查询会变快,那么他为什么会慢呢?他的是用树进行排序的,当添删改都会改变平衡二叉树的索引内容,破坏树结构,每次修改数据以后都需要重新梳理树的位置,这也就是为什么索引除了查询以为带来的副作用的原因



### 流程图

![img](https://img-blog.csdnimg.cn/20190116112435995.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0Z3VhbmdpdA==,size_16,color_FFFFFF,t_70)

## 非聚簇索引

​		非聚集索引与聚集索引基本一致,每一个索引都会根据索引字段建立一个平衡树的索引结构

​			<u>一个表中多个非聚集索引,之间不会干扰,毫无关系</u>



每个表添加一个非聚集索引都会给表中的数据复制一份,用于生成表的索引,因此给表添加索引,会添加表的体积,占用磁盘体积

### 流程图

![img](https://img-blog.csdnimg.cn/2019011611260686.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0Z3VhbmdpdA==,size_16,color_FFFFFF,t_70)

## 聚集索引与非聚集索引的区别



**区别在于通过聚集索引可以查到数据,但是通过非聚集索引可以找到对应的主键值,在使主键值通过聚集索引找到数据**



不管以什么方式查询数据都是通过聚合索引找到数据,只有聚合索引还是唯一途径



但是有一种例外可以不使用聚合索引,(复合索引,组合索引,覆盖索引)🤢名字真多都是一个玩意





## 覆盖索引(就名字多就觉得恶心🤮)



例如有一条SQL语句

~~~ mysql
CREATE INDEX index_name ADD user(name) # 在name字段创建索引
SELECT age FROM user WHERE name = "12"; # 根据name值查询数据
~~~



这个查询语句的执行流程大致

1. 通过name值找到对应的数据的主键
2. 通过主键找到真实数据的位置
3. 然后在从真实数据中获取age的返回



如果使用覆盖索引可以这样

~~~ mysql
CREATE INDEX index_name_age ADD user(user,age) #(复合索引,组合索引,覆盖索引)
SELECT age FROM user WHERE name = "12"; # 根据name值查询数据
~~~



这个流程大致就变成了

	1. 通过name值找到索引节点位置,
 	2. 但是叶子节点除了有name值以外还有age值(因此不需要用主键去找真实数据,可以直接返回)



### 流程图

![img](https://img-blog.csdnimg.cn/2019011611260686.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0Z3VhbmdpdA==,size_16,color_FFFFFF,t_70)

