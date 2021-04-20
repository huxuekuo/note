

# String_Table



## 常量池与字符串池的关系

我们先准备了一点简单的代码: 

```java
public class StringTable1 {

    public static void main(String[] args) {
        String s1 = "a";
        String s2 = "b";
        String s3 = "ab";
    }
}
```

上面代码运行完成以后生成StringTable1.class对象, 然后通过

```shell
javap -v StringTable1.class 
```



<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210416165638.png" style="zoom:33%;" />



**Constant pool 是什么 ?** 

> .java文件经过编译后生成.class文件，常量池可以理解为class文件的资源仓库
>
> - **常量池存放什么**？
>
> ​    常量池主要存放两大类常量：**字面量**（文本字符串、声明为final的常量值等）和***\*符号引用\**（**有三类：类和接口的全限定名、字段的名称和描述符、方法的名称和描述符）
>
> ​    常量池（类常量池）就是类在编译后的class文件中的一部分



**class文件中的常量池在类加载后进入方法区的运行时常量池存放** , 这个时候 图片中的`a,b,ab` 还只是符号,并没有成为java对象

我们看下面的截图

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210416170433.png" style="zoom:40%;" />

`ldc` 是获取常量池的数据,  

```shell
0:ldc   #2 //String a
```

这一行就是从常量池中加载 序号2的数据 , 这是 a 符号才会变成 `a` 字符串对象 , **以此类推**

Astore_1 是 将变量加载到**本地变量表(上图中`localVariableTable`)**slot为1的位置, 



## 字符串拼接

我们先准备了一点简单的代码: 

```java
public class StringTable1 {

    public static void main(String[] args) {
        String s1 = "a";
        String s2 = "b";
        String s3 = "ab";
        String s4 = s1 + s2;
    }
}
```

下图我们只看被红圈出来的部分

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210416173900.png" style="zoom:33%;" />

> 1. 首选`new` 一个StringBuilder对象
> 2. 调用StringBuilder的init方法
> 3. Aload_1 加载本地变量表 1 的数据
> 4. 调用append方法
> 5. Aload_2 加载本地变量表 1 的数据
> 6. 调用append方法
> 7. 调用toString方法
> 8. 保存到本地变量4的位置

这里需要注意的是 `s3` 的 ab 是在字符串池中的, 但是 s4 是在堆中

``` java
s3 == s4 // false
// 但是如果
String s5 = "a" + "b";  
```

这是s5 在编译期就会确定`s5`的值为`ab` , 所以这个 **s3 == s5 ** 是 true



## intern 方法

- intern 方法 
  - 1.8 可以将字符串对象尝试放入串池, 如果有则不会放入, 如果没有则放入, 会把串池的对象返回
  - 1.6 可以将字符串对象尝试放入串池, 如果有则不会放入, 如果没有会把对象复制一份放入, 会把串池的对象返回
    - 将堆中的对象创建一个新的对象放入串池

```java

1.
	 public static void main(String[] args) {
        String s = new String("a") + new String("b");
        String s5 = s.intern();
        System.out.println(s5 == "ab"); // true
        System.out.println(s == "ab"); // true
    }
		
  // -----------------------

2.
    public static void main(String[] args) {
        String x = "ab";
        String s = new String("a") + new String("b");
        String s5 = s.intern();
        System.out.println(s5 == x); // true
        System.out.println(s == x); // false
    }

```



上面的代码, 

- 序号1 , 创建`s` 的时候, 字符串池中并没有, `ab` , 调用**intern** 会将`ab`加入到字符串常量池, 所以都是true
- 序号2, 首先x = ab 将 `ab`加入串池中了, 
  - s.intern() 方法 就不会将s放入串池中, 只会返回一个串池的结果

