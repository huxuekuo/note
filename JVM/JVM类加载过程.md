# JVM 类加载过程



## 代码是如何运行起来的



>  将我们编写的`*.java`的文件打包成`jar包或war包` 然后通过 **tomcat或者Java -jar ** 命令运行



这里面就涉及到编译文件, 打包以后, `jar包与war包中包含的都是class文件`,  我们分别介绍一下jar包的运行 与 war运行的步骤



- jar包
  - 在打包为jar包时需要标识main方法入口全限定名, 在运行时jvm会以main方法为入口开始加载
- war包
  - war 包通过**tomcat**进行加载, **tomcat**中存在<u>Bootstrap</u>类, 以<u>Bootstrap</u>类的main方法为入口开始加载

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210421112358.png" style="zoom:50%;" />



### 类加载子系统



**是什么将class文件转化为二进制流将类加载到内存中的?**

> 答 : classloader, 因为classloader 篇幅太长, 我这里在另一篇文章中重点叙述了
>
> ​	  要想将*.class 文件加载进 JVM 需要ClassLoader, 

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210421113209.png" style="zoom:40%;" />



### 字节码执行引擎



> JVM就会基于自己的**字节码执行引擎**，来执行加载到内存里的我们写好的那些类了
>
> 比如你的代码中有一个“main()”方法，那么JVM就会从这个“main()”方法开始执行里面的代码。
>
> 需要哪个类的时候，就会使用类加载器来加载对应的类，反正对应的类就在“.class”文件中



## 类加载过程



