# docker基本操作



## docker仓库



- daoCloud

  - http://hub.daocloud.io/

- 官方

  - https://hub.docker.com/

- 配置私服

  ```json
  # /etc/docker/daemon.json
  {
      "registry-mirrors": [
      "https://mirror.ccs.tencentyun.com"
    ],
     "insecure-registries": ["IP:port"],
  }
  # IP:port 需要自行配置
  # 需要重启两个服务
  #	systemctl daemon-reload
  #	systemctl restart docker
  ```



## docker数据卷

> 数据卷, 将宿主机的一个目录映射到容器的一个目录
>
> 可以在宿主机改变文件内容, 容器内也会改变



### 命令

-----



#### 创建数据卷

> 不指定路径默认创建在`/var/lib/docker/volumes/`

```shell 
docker volume create `volumeName` # volumeName 是自定义的
```

----



#### 查看数据卷

```shell
docker volume inspect `volumeName` # volumeName 是自定义的
```

----



#### 查看全部数据卷

```shell
docker volume ls
```

---



#### 删除数据卷

```shell
docker volume rm `volumeName` # volumeName 是自定义的
```



#### 数据卷映射

```sh
# 方式一丶使用已经创建好的数据卷进行映射, 容器中的一个目录, 当容器创建完毕会把映射目录中的数据都带出来
docker run -v `volumeName`:`mapper_path` 

# 方式二丶使用路径方式进行映射, 会后一个名称为数据卷名称, 并且不会吧容器的目录映射出来需要自己创建
docker run -v `volume_path`:`mapper_path`
```





## docker 自定义镜像

