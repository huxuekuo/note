# CAS看AtomicInteger



## 概述



CAS 全称 Compare and Swap(比较交换),CAS是一种无锁算法, CAS有3个操作数, 内存值V,旧的预期值A,要修改的新值B, 当预期值A与内存V相等,修改内存值V,