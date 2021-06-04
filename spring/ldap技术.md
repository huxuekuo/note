# LDAP 讲解

## 什么是LDAP

`LDAP`（Light Directory Access Portocol）是一款轻量级目录访问协议, 属于`开源账号集中管理的实现`,且支持众多系统版本

`OpenLdap`是LDAP的一种开源实现, OpenLdap是在X.500的基础上进行改进的, 但与`X.500` 也有不同之处，例如`OpenLDAP` 支持TCP/IP 协议等，目前TCP/IP 是Internet 上访问互联网的协议。

`OpenLDAP` 则直接运行在更简单和更通用的TCP/IP 或其他可靠的传输协议层上，避免了在OSI会话层和表示层的开销，使连接的建立和包的处理更简单、更快，对于互联网和企业网应用更理想

**OpenLDAP 目录不支持通用数据库大量的更新操作与所需的复杂的事务管理或者回滚策略**

`OpenLDAP` 默认以**Berkeley DB** 作为后端数据库，**Berkeley DB** 数据库主要以散列的数据类型进行数据存储，如以键值对的方式进行存储。**Berkeley DB** 是一类特殊的数据库，主要用于搜索、浏览、更新查询操作，一般对于一次写入数据、多次查询和搜索有很好的效果。**Berkeley DB** 数据库是面向查询进行优化，面向读取进行优化的数据库。**Berkeley DB 不支持事务型数据库（MySQL、MariDB、Oracle 等）所支持的高并发的吞吐量以及复杂的事务操作**。



### 什么是目录服务? 



目录服务是一个为了 `查询 浏览 搜索` 而优化的数据库, 它是树状组织数据, 类似文件目录一样

目录数据库有关系数据库不同, 它有优异的读性能, 但 `写性能`差, 

属于开源集中账号管理架构的实现，且支持众多系统版本，被广大互联网公司所采用

目录服务在国际中有两个标准, 一个是x.500一个是LDAP, OpenLdap是LDAP的实现, 也是基于x.500的



## LDAP 的实现产品



LDAP 全称: 轻量级目录访问协议, 协议就是一套标准, 那么具体的实现有哪些呢? 



| 厂家       | 产品                       | 介绍                                                         |
| ---------- | -------------------------- | ------------------------------------------------------------ |
| SUN        | SUNONE Directory Server    | 基于文本数据库的存储，速度快                                 |
| IBM        | IBM Directory Server       | 基于DB2 的的数据库，速度一般。                               |
| Novell     | Novell Directory Server    | 基于文本数据库的存储，速度快, 不常用到。                     |
| Microsoft  | Microsoft Active Directory | 基于WINDOWS系统用户，对大数据量处理速度一般，但维护容易，生态圈大，管理相对简单。 |
| Opensource | Opensource                 | OpenLDAP 开源的项目，速度很快，但是非主 流应用。             |





## Spring整合LDAP



### jar包选择

```xml
        <dependency>
            <groupId>org.springframework.ldap</groupId>
            <artifactId>spring-ldap-core</artifactId>
        </dependency>
```

> 部署版本根据Spring的版本进行选择



### Config

```java
@Configuration
public class LdapConfiguration {

    @Bean
    public LdapContextSource contextSource() {
        LdapContextSource contextSource = new LdapContextSource();
        Map<String, Object> config = new HashMap();

        contextSource.setUrl("ldap://IP:PORT");
        contextSource.setUserDn("....");
        contextSource.setPassword("...");

        //  解决 乱码 的关键一句
        config.put("java.naming.ldap.attributes.binary", "objectGUID");

        contextSource.setPooled(true);
        contextSource.setBaseEnvironmentProperties(config);
        return contextSource;
    }

    @Primary
    @Bean("ldapTemplate")
    public LdapTemplate ldapTemplate() {
        return new LdapTemplate(contextSource());
    }

}
```



### 常用方法

```java
ldapTemplate.authenticate(userCn, "(objectclass=person)", pwd); // 根据用户验证账号密码
```