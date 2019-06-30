# redis分布式锁



## 简介

常用的分布式锁分为三种DB锁,redis锁和zookeeper锁,我们今天讲解一下redis锁

首先说一下锁,

1.如果实现锁必须保证不能产生死锁,

2.可用性不能redis宕机了加锁失败,

3.高性能加锁解锁快,

4.保证自己的锁自己可以解

## 方式一



### 加锁+解锁步骤

1.在java中引入jedis使用**set(String key, String value, String nxxx, String expx, int time)** 获得锁的同时设置过期时间防止服务宕机后锁不能释放,发生死锁,

2.释放锁,就是把key删掉就可以了,注意一个细节,如果直接使用del命令释放锁,这就代表谁都可以将这个锁释放掉,所以在解锁是需要判断这个锁是不是自己的,就需要那value值做判断.可以使用lua脚本实现,可以将判断value是否有自己的value是否相同如果相同释放锁,原本两个步骤,变成一个原子性的操作.

### 代码

~~~ java
package com.mzc.redis;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisCluster;

import java.util.Collections;
import java.util.concurrent.TimeUnit;

/**
 * @Author: huxuekuo
 * @Date: 2019-06-29 07:46
 * @Description:
 */
public class RedisLock {

    private Jedis jedis = new Jedis("127.0.0.1", 6380);

    private static final String SET_IF_NOT_EXIST = "nx"; // 判断如果key存在则不做任何操作,锁不存在写入
    private static final String SET_WITH_EXPIRE_TIME = "px"; //设置锁的过期时间
    private static final String LOCK_MSG = "OK"; //加锁成功
    private static final String UNLOCK_MSG = "1"; //释放锁成功

    //开启锁
    public boolean setLock(String key, String request) {
        for (; ; ) {
            String set = this.jedis.set(key, request, SET_IF_NOT_EXIST, SET_WITH_EXPIRE_TIME, 10 * 1000);
            if (LOCK_MSG.equals(set)) {
                System.out.println("设置成功");
                return true;
            }
            try {
                Thread.sleep(5 * 1000); //设置等待时长,防止一直消耗CPU
            } catch (InterruptedException e) {
            }
        }
    }

    //释放锁
    public boolean unlock(String key, String request) {
        String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";

        Object result = null;
        if (jedis instanceof Jedis) {
            result = ((Jedis) this.jedis).eval(script, Collections.singletonList(key), Collections.singletonList(request));
        } else {
            return false;
        }

        if (UNLOCK_MSG.equals(result)) {
            return true;
        } else {
            return false;
        }
    }
}

~~~

### 	使用jar包

~~~java
       <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.9.0</version>
        </dependency>
~~~

### 	错误实例

~~~java
public static void wrongGetLock1(Jedis jedis, String lockKey, String requestId, int expireTime) {

    Long result = jedis.setnx(lockKey, requestId); //先说的锁
    if (result == 1) {
        // 若在这里程序突然崩溃，则无法设置过期时间，将发生死锁
        jedis.expire(lockKey, expireTime);
    }

}
~~~



## 方式二



