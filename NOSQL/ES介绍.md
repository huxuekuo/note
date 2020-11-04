

# ES 简介



> ES是一个使用java语言编写的并且基于Lucene编写的搜索引擎, 他提供了分布式的全文搜索服务, 还提供了一个RESTful风格的web接口, 官方还对多种语言提供了相应的API
>
> 
>
> Lucene?
>
> Lucene 本身就是一个搜索引擎的底层, 



# ES特点



> 
>
> 分布式: ES主要为了横向扩展能力
>
> 
>
> 全文检索: 将一段词语进行分词, 并且将分出的单个词语统一的放入一个分词库中,在搜索时,根据关键字去分词库中搜索去找到想找到的内容,(倒排索引)
>
> 
>
> RESTful风格web接口: 操作ES非常简单, 只需要发送一个Http请求并且根据请求方式不同和携带参数不同,执行相应的功能, 
>
> 



## 倒排索引



待补充



# 安装ES&kibana

```yml
version: "3.1"
services:
  elasticsearch:
    image: daocloud.io/library/elasticsearch:6.5.4
    restart: always
    container_name: elasticsearch
    environment:
      - "cluster.name=elasticsearch" #设置集群名称为elasticsearch
      - "discovery.type=single-node" #以单一节点模式启动
      - "ES_JAVA_OPTS=-Xms4096m -Xmx4096m" #设置使用jvm内存大小
    ports:
      - 9200:9200
  kibana:
    image: daocloud.io/library/kibana:6.5.4
    restart: always
    container_name: kibane
    ports:
    	- 5601:5601
    depends_on:
      - elasticsearch #kibana在elasticsearch启动之后再启动
    environment:
      - "elasticsearch.hosts=http://127.0.0.1:9200" #设置访问elasticsearch的地址
```



# IK 分词器



## 安装

> 下载地址: https://github.com/medcl/elasticsearch-analysis-ik/releases?after=v6.7.2
>
> 下载需要与ES版本一直否则无法启动ES, 



在 elasticsearch/plugins 下新建IK文件夹, 解压 后 重启ES即可



## IK 介绍

他有两种分词模式 ik_mart()与 ik_max_word



### ik_mart 使用介绍



![image text](https://raw.githubusercontent.com/huxuekuo/note/master/images/IK_mart.jpeg)



### ik_max_word使用介绍

