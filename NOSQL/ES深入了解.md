# ES 深入学习

## 疑惑(1)

![image text](https://raw.githubusercontent.com/huxuekuo/note/master/images/es_doubt_1.jpg)

这里发现`name` 的type是`text`但是学习的时候了解到text类型在Kibana使用`_search`精确匹配搜索时不进行模糊匹配的, 但是如果使用`keyword`是会进行精确匹配的,

需要解释的是, 这些字段是我直接使用`PUT`命令进行创建文档时`系统自动给定的类型`, 我觉得有必要将为创建索引时指定类型的图片贴到这里

![image text](https://raw.githubusercontent.com/huxuekuo/note/master/images/es_doubt_1_1.jpg)

这张图片是我自己创建`type`类型时指定的`type`发现没有下面这句话,并且在我使用`match`进行的时候不进行模糊匹配, 使用系统自动创建的`type`是可以进行模糊匹配的

```json
  "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256
              }
            }
```



### 解答:



