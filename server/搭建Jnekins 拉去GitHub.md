# 搭建Jnekins 拉去GitHub



## 安装Jnekins过程



```shell
yum install -y java-1.8.0-openjdk  # jenkins 依赖JDK

wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo // wget -O 下载文件并以指定的文件名保存

cat /etc/yum.repos.d/jenkins.repo
​```
[jenkins]
name=Jenkins
baseurl=http://pkg.jenkins.io/redhat
gpgcheck=1 //这里会检测key
​``` 

rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key # 安装Jnekins Key

yum install -y jenkins # 安装JneKins 

systemctl start jenkins # 启动Jnekins

ps aux |grep jenkins # 查看Jnekins是否启动成功

cat /var/log/jenkins/jenkins.log # 查看Jnekins 启动日志

​```
Jenkins initial setup is required. An admin user has been created and a password generated

Please use the following password to proceed to installation:

77faa20f2ad544f7bcb6593b1cf1436b        //admin密码，初始化安装时会用到

This may also be found at: /var/lib/jenkins/secrets/initialAdminPassword        //admin密码也可以在这里查到
​```
```



## 访问Jnekins

### Jenkins 访问地址:  http://IP:8080/

刚开始加载时间比较长, 需要等待一下

