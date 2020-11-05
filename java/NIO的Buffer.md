# Buffer

## position



表示当前位置, 初始化为0,最大值为 -1. 写入一个字节数据时,position就会向下一位, 读取数据时就会向上移动一位



## limit



读写的最大限制, 在写模式与读模式限制是不一样的, 在写模式时最大限制就是capacity的位置,切换为读模式时limit的最大限制就切换为position再读模式时的位置



## capacity

作为一个内存块，Buffer有固定的大小值，也叫作“capacity”，只能往其中写入capacity个byte、long、char等类型。一旦Buffer满了，需要将其清空（通过读数据或者清楚数据）才能继续写数据。



![image-20200713225711488](/Users/huxuekuo/Library/Application Support/typora-user-images/image-20200713225711488.png)