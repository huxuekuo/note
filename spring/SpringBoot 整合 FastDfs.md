# SpringBoot 整合 FastDfs



## Docker搭建FastDfs



```shell
# 搜索fastdfs镜像
docker search fastdfs

# 拉去fastdfs镜像
docker pull delron/fastdfs

# 使用docker镜像构建tracker容器（跟踪服务器，起到调度的作用）

docker run -d --network=host --name tracker -v /var/fdfs/tracker:/var/fdfs delron/fastdfs tracker

# 使用docker镜像构建storage容器（存储服务器，提供容量和备份服务) IP 为主机公网IP

docker run -d --network=host --name storage -e TRACKER_SERVER=ip:22122 -v /var/fdfs/storage:/var/fdfs -e GROUP_NAME=group1 delron/fastdfs storage

# 进入 storage 测试是否搭建成功
fdfs_upload_file /etc/fdfs/client.conf cumt.png
```



重启时发现重启不了, 查看docker启动容器日志



```shell
[root@VM-0-9-centos logs]# docker logs --since 30m 0146a9e5235e

>>>>>>> 

try to start the storage node...
tail: cannot open '/var/fdfs/logs/storaged.log' for reading: No such file or directory
tail: no files remaining
ngx_http_fastdfs_set pid=8
try to start the storage node...
tail: cannot open '/var/fdfs/logs/storaged.log' for reading: No such file or directory
tail: no files remaining
```



手动创建 `/var/fdfs/logs/storaged.log`



## springboot 整合 fastdfs





### 配置文件

```java
import com.github.tobato.fastdfs.FdfsClientConfig;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableMBeanExport;
import org.springframework.context.annotation.Import;
import org.springframework.jmx.support.RegistrationPolicy;

/**
 *
 */
@Configuration
@Import(FdfsClientConfig.class) // 导入FastDFS-Client组件
@EnableMBeanExport(registration = RegistrationPolicy.IGNORE_EXISTING) // 解决jmx重复注册bean的问题
public class FdfsConfiguration {
}

```



### 工具类

```java

import com.github.tobato.fastdfs.domain.fdfs.StorePath;
import com.github.tobato.fastdfs.domain.fdfs.ThumbImageConfig;
import com.github.tobato.fastdfs.domain.proto.storage.DownloadCallback;
import com.github.tobato.fastdfs.service.FastFileStorageClient;
import org.apache.commons.io.FilenameUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.io.InputStream;

@Component
public class FastDFSClientUtil {

    @Value("${fdfs.reqHost}")
    private String reqHost;

    @Value("${fdfs.reqPort}")
    private String reqPort;

    @Autowired
    private FastFileStorageClient storageClient;

    @Autowired
    private ThumbImageConfig thumbImageConfig; //创建缩略图   ， 缩略图访问有问题，暂未解决


    public String uploadFile(MultipartFile file) throws IOException {
        StorePath storePath = storageClient.uploadFile((InputStream) file.getInputStream(), file.getSize(), FilenameUtils.getExtension(file.getOriginalFilename()), null);

        String path = thumbImageConfig.getThumbImagePath(storePath.getPath());
        System.out.println("thumbImage :" + path);  //   缩略图访问有问题，暂未解决

        return getResAccessUrl(storePath);
    }

    public void delFile(String filePath) {
        storageClient.deleteFile(filePath);

    }


    public InputStream download(String groupName, String path) {
        InputStream ins = storageClient.downloadFile(groupName, path, new DownloadCallback<InputStream>() {
            @Override
            public InputStream recv(InputStream ins) throws IOException {
                // 将此ins返回给上面的ins
                return ins;
            }
        });
        return ins;
    }

    /**
     * 封装文件完整URL地址
     *
     * @param storePath
     * @return
     */
    private String getResAccessUrl(StorePath storePath) {
        String fileUrl = "http://" + reqHost + ":" + reqPort + "/" + storePath.getFullPath();
        return fileUrl;
    }
```



### bootstrap.yml 配置



```yaml
# 分布式文件系统FDFS配置
fdfs:
  soTimeout: 1500 #socket连接超时时长
  connectTimeout: 600 #连接tracker服务器超时时长
  reqHost: 49.232.61.35   #nginx访问地址
  reqPort: 8888              #nginx访问端口
  thumbImage: #缩略图生成参数，可选
    width: 150
    height: 150
  trackerList: #TrackerList参数,支持多个，我这里只有一个，如果有多个在下方加- x.x.x.x:port
    - 49.232.61.35:22122
```





### jar 引入



```xml

        <dependency>
            <groupId>com.github.tobato</groupId>
            <artifactId>fastdfs-client</artifactId>
            <version>1.26.5</version>
        </dependency>
```

