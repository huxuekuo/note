# JWT(JSON Web Token)



## Session





## Token

临时且唯一, 保证不能重复,安全性更高, 比传统的`session`灵活性更大,比如`session`是需要存储在服务端的, 如果服务重启了,`session`也就不存在了, 而`token`可以存放在第三方缓存数据库中比如`redis` 



### Token 流程

![image text](https://gitee.com/panda_soft/note_images/raw/master/JWT_Token_01.jpg)

### 为什么token保证安全? 

- 保证唯一性
- 保证参数真实性



## `JWT`



全称 JSON Web Token . `JSON 跨语言,轻量级,可读性高`,`Token`上面已经介绍过了,与token不同的地方是JWT的`payLoad`部分是可以存放数据的, 这样就减少了查询数据库的方式



JWT 一共三个部分.

第一部分, `header部分`, 标记使用什么算法, **使用base64编码后的数据**

第二部分, `payLoad(载荷)`, JWT 存放的数据 **使用base64编码后的数据**

第三部分, `签证信息`,将编码后的header和编码后的payLoad 根据 header中的算法加盐后加密数据

