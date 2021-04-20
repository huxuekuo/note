# CAS看AtomicInteger



## 概述



CAS 全称 Compare and Swap(比较交换),CAS是一种无锁算法, CAS有3个操作数, 内存值V,旧的预期值A,要修改的新值B, 当预期值A与内存V相等,修改内存值V,



## AtomicInteger实现CAS



```java
 public final int getAndAddInt(Object var1, long var2, int var4) {
        int var5;
        do {
            var5 = this.getIntVolatile(var1, var2);
        } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));

        return var5;
    }
```



var1 内存值