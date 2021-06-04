# JVM Eden XX:SurvivorRatio

大家应该都知道新生代是有三块区域的, Eden区域 SurvivorFrom区域 SurvivorTo区域, 我们下面将的`XX:SurvivorRatio` 是对这个三个区域内存比例进行分配内容



首先 `XX:SurvivorRatio` 参数默认是8 , 假设 我们这是` -XX:MaxNewSize=5G` 计算公式为 

```
Eden=(N * Xmn)/(N+2)，survivor(from)=suvivor(to)=(1* Xmn)/(N+2)
Eden = (8 * 5) / (8 + 2) survivor(from)=suvivor(to)=(1 * 5)/ (8 + 2)
```

结果就是 Eden 区域 4G, survivorFrom-To 0.5G 





https://blog.csdn.net/rogerxue12345/article/details/107863762

https://blog.csdn.net/flyfhj/article/details/86630105

-XX:NewSize=3G -XX:MaxNewSize=3G  -XX:SurvivorRatio=3

Eden=(N *Xmn)/(N+2)，survivor(from)=suvivor(to)=(1* Xmn)/(N+2)

