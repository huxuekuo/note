# Mysql储存过程





[TOC]



## 简介

储存过程是一个在数据库中创建保存的.**<u>他可以有SQL语句和特殊的控制结构组成,当希望在不同的应用程序后者平台执行相同的函数或者封装特定功能时,储存过程非常有用的.数据库中储存过程可以看做编程中面相对象方法的模拟</u>**

## 优点

- <u>储存过程能实现较快的执行速度</u>
  - 如果某一操作包含大量的Transaction-SQL代码或者分别被多次执行,那么储存过程要比批处理的执行速度快很多,因为储存过程是预编译的,在首次运行一个储存过程查询时,优化器对其进行分析优化,并且给出最终被储存在系统表中的执行计划,而批处理的Transaction-SQL在每次运行时都要进行编译和优化,速度相对慢一些
    - 储存过程将第一次Transaction-SQL语句的执行计划储存在表中
    - 而批处理每次运行需要进行编译和优化
    - 编译优化,快
- 储存过程能够运行标准组件就是编程
  - 储存过程被创建以后,可以在程序中多次调用,而不必重新编写改储存过程的SQL语句,而且数据库专业人员可以随时对储存过程进行修改,对应用程序源代码毫无影响.
    - 封装与抽象,简单调用
- 可以使用流程控制语句
  - 可以完成复杂的判断和较复杂的运算
- 储存过程可以作为一种安全机制来充分利用
  - 系统管理员通过执行某一储存过程的权限进行限制,能够实现对相应的访问权限进行限制,避免了非授权用户对数据的访问,保证数据的安全
- 存储过程能过减少网络流量
  - 针对同一个数据库对象的操作(如查询,修改),如果这一操作涉及到Transaction-SQL语句被组织储存过程,那么当在客户计算机上调用该储存过程时,网络中传输的只是该调用语句,从而大大增加了网络流量并降低了网络负载

## Mysql储存过程学习



### 一  初识创建语法

~~~ mysql
#创建一个储存过程
delimiter // 
create procedure test() #创建储存过程名
begin
	# 储存过程的方法体
end
//
~~~



#### 需要注意的是:

​			在创建储存过程名时,比如test() 就算没有参数此括号也是不能省略的

​			上面语法有时会因为语法报错,可以加入 delimiter// 储存过程 //

#### 二 使用存储过程语法

~~~ mysql
CALL procedureName;
~~~



#### 三 查询全部储存过程

~~~ mysql
show procedure status;
~~~



### 完整的例子

~~~mysql
DELIMITER $$  
DROP PROCEDURE IF EXISTS `dbcall`.`get_page`$$  
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_page`(  
/**//*Table name*/  
tableName varchar(100),  
/**//*Fileds to display*/  
fieldsNames varchar(100),  
/**//*Page index*/  
pageIndex int,  
/**//*Page Size*/  
pageSize int,   
/**//*Field to sort*/  
sortName varchar(500),  
/**//*Condition*/  
strWhere varchar(500)  
)  
BEGIN   
DECLARE fieldlist varchar(200);   
if fieldsNames=''||fieldsNames=null THEN  
set fieldlist='*';  
else  
set fieldlist=fieldsNames;   
end if;  
if strWhere=''||strWhere=null then  
if sortName=''||sortName=null then   
set @strSQL=concat('SELECT ',fieldlist,' FROM ',tableName,' LIMIT ',(pageIndex-1)*pageSize,',',pageSize);  
else  
set @strSQL=concat('SELECT ',fieldlist,' FROM ',tableName,' ORDER BY ',sortName,' LIMIT ',(pageIndex-1)*pageSize,',',pageSize);   
end if;  
else  
if sortName=''||sortName=null then  
set @strSQL=concat('SELECT ',fieldlist,' FROM ',tableName,' WHERE ',strWhere,' LIMIT ',(pageIndex-1)*pageSize,',',pageSize);  
else  
set @strSQL=concat('SELECT ',fieldlist,' FROM ',tableName,' WHERE ',strWhere,' 
ORDER BY ',sortName,' LIMIT ',(pageIndex-1)*pageSize,',',pageSize);   
end if;  
end if;   
PREPARE stmt1 FROM @strSQL;   
EXECUTE stmt1;  
DEALLOCATE PREPARE stmt1;  
END$$  
DELIMITER ;  
~~~

