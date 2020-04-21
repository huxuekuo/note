# Bigdecimal 大数型



在小数小点计算中如果使用double.float经常会遇到丢失经度,这个时候就要使用Bigdecimal来保证经度



## 首先介绍一下初始化方式

Bigdecimal 有两种创建方式

```java
new BigDecimal()	
Bigdecimal.valueOf()
```



这里建议如果是小数类型使用valueOf()方式,因为如果使用new Bigdecimal在初始化时还是有可能出现精度丢失,但是使用Bigdecimal的valueOf()方法

```java
    public static BigDecimal valueOf(double val) {
        // Reminder: a zero double returns '0.0', so we cannot fastpath
        // to use the constant ZERO.  This might be important enough to
        // justify a factory approach, a cache, or a few private
        // constants, later.
        return new BigDecimal(Double.toString(val));
    }
```



看到这段代码,就知道了如果是String类型的数字,就不需要使用valueOf()直接new BigDecimal()就可以了



## 加减乘除使用方式

### Subtract(减法)

```java
        BigDecimal bigdecimal = new BigDecimal("0");
        bigdecimal.subtract()
```



### add(加分)

```java
        BigDecimal bigdecimal = new BigDecimal("0");
        bigdecimal.add()
```



###divide(除法)

最简单用法

```java
BigDecimal decimal = new BigDecimal
 decimal.divide()
```

当除不尽时会出现错误信息:

> <font color=red> java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result. </font>



这时就需要做省略小数点后几位

```java
BigDecimal decimal = new BigDecimal
 decimal.divide(2,2,BigDecimal.ROUND_HALF_UP)
```

这样的意思就是说

当前的decimal除以2保留两位小数,如果第三位小数>=5就进位



### multiply(乘法)

```java
        BigDecimal bigdecimal = new BigDecimal("9.9");
        bigdecimal.multiply()
```



## 省略模式



### 1.ROUND_DOWN

 ```java
 BigDecimal b = new BigDecimal("2.225667").setScale(2, BigDecimal.ROUND_DOWN);
 System.out.println(b);//2.22 直接去掉多余的位数
 ```



 

 

###  2.ROUND_UP



```java
BigDecimal c = new BigDecimal("2.224667").setScale(2, BigDecimal.ROUND_UP);
System.out.println(c);//2.23 跟上面相反，进位处理
```

  

 

### 3.ROUND_CEILING

 

天花板（向上），正数进位向上，负数舍位向上

 ```java
BigDecimal f = new BigDecimal("2.224667").setScale(2, BigDecimal.ROUND_CEILING);
System.out.println(f);//2.23 如果是正数，相当于BigDecimal.ROUND_UP  
BigDecimal g = new BigDecimal("-2.225667").setScale(2, BigDecimal.ROUND_CEILING);
System.out.println(g);//-2.22 如果是负数，相当于BigDecimal.ROUND_DOWN
 ```

 

 ### 4. ROUND_FLOOR

 

地板（向下），正数舍位向下，负数进位向下

```java
1. BigDecimal h = new BigDecimal("2.225667").setScale(2, BigDecimal.ROUND_FLOOR);
2. System.out.println(h);//2.22 如果是正数，相当于BigDecimal.ROUND_DOWN
3.  
4. BigDecimal i = new BigDecimal("-2.224667").setScale(2, BigDecimal.ROUND_FLOOR);
5. System.out.println(i);//-2.23 如果是负数，相当于BigDecimal.ROUND_HALF_UP
```



 

### 5.ROUND_HALF_UP

```java
 
1. BigDecimal d = new BigDecimal("2.225").setScale(2, BigDecimal.ROUND_HALF_UP);
2. System.out.println("ROUND_HALF_UP"+d); //2.23 四舍五入（若舍弃部分>=.5，就进位）
```



 

### 6. ROUND_HALF_DOWN



```java
1. BigDecimal e = new BigDecimal("2.225").setScale(2, BigDecimal.ROUND_HALF_DOWN);
2. System.out.println("ROUND_HALF_DOWN"+e);//2.22 四舍五入（若舍弃部分>.5,就进位）
```



  

###7. ROUND_HALF_EVEN



```java
BigDecimal j = new BigDecimal("2.225").setScale(2, BigDecimal.ROUND_HALF_EVEN);
System.out.println(j);//2.22 如果舍弃部分左边的数字为偶数，则作 ROUND_HALF_DOWN
BigDecimal k = new BigDecimal("2.215").setScale(2, BigDecimal.ROUND_HALF_EVEN);
System.out.println(k);//2.22 如果舍弃部分左边的数字为奇数，则作 ROUND_HALF_UP
```

