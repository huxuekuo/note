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



## Hello_word



```sh
docker run dockerinaction/hello_world
```

再次之后Docker会被激活, 它会开始下载组件, 并且最终打印`hello word`,当再一次使用这个命令, 他将只会打印`hello word`,  我看在下图中介绍一下docker run 是如何工作的 

![image-20201225141213984](/Users/huxuekuo/Library/Application Support/typora-user-images/image-20201225141213984.png)







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





## Docker File



### DockerFile指令

---



| 指令名称   | 含义                                                         | 使用方式                            |
| ---------- | ------------------------------------------------------------ | ----------------------------------- |
| FROM       | 基础镜像, 当前新镜像基于那个镜像                             |                                     |
| MAINTAINER | 镜像维护者的姓名和邮箱                                       | MAINTAINER huxuekuo 21044903@qq.com |
| RUN        | 容器构建是需要运行的命令                                     |                                     |
| EXPOSE     | 当前容器对外暴露的端口                                       |                                     |
| WORKDIR    | 终端进入容器后的一个落脚点                                   |                                     |
| ENV        | 构建容器过程中设置环境变量                                   |                                     |
| ADD        | 将宿主机下的文件拷贝进镜像,ADD命令会自动处理URL与解压tar压缩包 |                                     |
| COPAY      | 将宿主机下的文件拷贝进镜像                                   | `COPY src dest`                     |
| VOLUME     | 容器数据卷, 用于数据保存和持久化操作                         |                                     |
| CMD        | 指定一个容器启动时要运行的命令, Dockerfile中可以指定多个CMD命令, 但只有最后一个会生效, CMD会被docker run 之后的命令代替 |                                     |
| ENTRYPOINT | 指定一个容器启动时要运行的命令,                              |                                     |
| ONBUILD    | 当构建一个被继承的dockerfile时运行的命令.父镜像再被子镜像继承后父镜像会运行的指令 |                                     |





### 案例一(centos)

---



