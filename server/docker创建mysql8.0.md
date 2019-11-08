

从mysql官网拉取mysql8.0版本

```shell
docker pull mysql:8.0
```



运行并设置初始密码与设置外部访问端口

```shell
docker run -it  --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql 
```



查看全部镜像

```shell
docker ps -a  
```



进入mysql容器

```shell
docker exec -it mysql bash 
```



进入mysql命令行

```shell
mysql -uroot -p123456
```



查看mysql用户信息

```shell
select host,user,plugin,authentication_string from mysql.user;
```



更改用户权限

```shell
ALTER user 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
```



刷新用户权限

```shell
FLUSH PRIVILEGES; 
```



退出mysql命令行

```shell
 exit; 
```

