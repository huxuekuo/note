# JVM介绍



## JVM整体结构



![image text](https://gitee.com/panda_soft/note_images/raw/master/JVM_01.jpg)



> 首先我们需要知道`java`中都由什么组成?
>
> 对象,对象引用,类, class文件,成员变量,局部变量,静态变量,方法,基本数据类型(int...) 下面我们介绍这些组成在JVM那一块存储



### 方法区

**方法区存储的内容**:

> 运行时常量池, 字段和方法数据, 以及方法和构造函数的代码, 包括用于类和实例初始化以及接口初始化的特殊方法

#### JDK1.8以前

> - JDK1.8以前, hotspot方法区成为`永久代`, 方法区存在与JVM管理, 
> - 设置永久代参数 -XX:MaxPermSize

>  **这里解释一下为什么1.8以前方法区称为永久代**
>
> ​     因为HotSpot虚拟机的设计团队选择吧GC分代收集扩展到了方法区,   使用`永久代的实现方式实现了方法区`而已 这样Hotspot 的垃圾回收器可以像管理java堆一样管理`方法区`, 可以省去编写方法区的相关工作, 
>
> ​	 这并不是一个好办法, 因为这样更容易出现内存溢出问题

​	

​	java 虚拟机规范对方法区的限制很宽松, 除了和`java堆一样不需要连续的内存和可以固定大小或者可以扩展外`.

​	

​	还可以选择不实现垃圾回收, 相对而言, 垃圾回收的行为在这个区域很少出现, 但并不是跟名字一样, `永久`存在了

​	

​	这个区域内存回收的目标, 主要是针对常量池的回收和对类型卸载, 一般来说, 这个区域的回收成绩很好, 尤其是类型的卸载, 条件相对苛刻, 但这部门的回收是有必要的, 因为在sun公司的BUG列表中, 出现低版本的hotspot 对此区未完全回收出现的内存泄露问题, 



​	根据java虚拟机规范的规定, 当方法区无法满足内存分配需求时, 将抛出OutOfMemoryError异常



![image-20210411132446811](/Users/huxuekuo/Library/Application Support/typora-user-images/image-20210411132446811.png)

#### JDK1.8及以后



> - JDK1.8中hotspot 将方法区的实现更改为`元空间`,方法区也不属于JVM内存的一块了, 移出到了本地内存, 将字符串常量池移动到了`堆`中
> - 设置元空间大小的方式 -XX:MaxMetaspaceSize

![image-20210411132827836](/Users/huxuekuo/Library/Application Support/typora-user-images/image-20210411132827836.png)



#### 运行时常量池

​	

​	运行时常量池也是`方法区`的一部分

​	Class 文件中 除了有 类的版本, 字段, 方法, 接口等描述信息, 还有`常量池, 用于存放编译期生成的各种字面量和符号引用,这部分内容将**类加载**后进入方法区的运行时常量池的存放`

​	运行时常量池还可以吧`字符引用翻译出来的直接引用,存放在运行时常量池中`

​	运行时常量池相对于Class文件常量池的另一个重要的特征是`动态性`, java语言并不要求常量池一定只有编译期才生产, 也就是说**不需要提前进入Class文件的常量池, 也能进入运行时常量池**,运行期也可能将新的常量放到**常量池中**,



jdk1.6 及以前 `字符串常量池`在运行时常量池中

jdk1.8及以后 `字符串常量池`移动到了堆中



### 堆



Java 堆 是java虚拟机所管理的内存中最大的一块, java堆是被所有线程共享的一块内存, 在虚拟机启动时创建, 此内存区域唯一目的就是存放对象实例, 几乎所有的对象都在这个分配内存,

**java虚拟机规范中描述**

> 所有的对象实例以及数组都要在堆上分配内存, 但是随着JIT编译器的发展与逃逸分析技术逐渐成熟, 栈上分配, 标量替换技术将会导致一些微妙的变化, 所有的对象都分配在堆上变得没有那么`绝对`了



java堆是垃圾回收的主要区域, 因为很多时候叫做`GC堆` , 由于现在回收器都是基于分代算法, 所以java堆可以细分为:

 - Eden (年轻代)
   	- From Survivor 空间
   	- To Survivor 空间
- Tenured Gen(老年代)







### 虚拟机栈



![image text](https://gitee.com/panda_soft/note_images/raw/master/JVM_02.jpg)



java虚拟机栈, 每个线程都自己的虚拟机栈, 生命周期与线程一致, 每个方法在执行时都会创建一个**栈帧,用于存储局部变量表,操作数栈, 动态链接, 方法返回地址, 等信息**, 每一个方法调用到方法执行完毕, 对应一个栈帧入栈到出栈的过程`(栈:栈先入后出)`,





#### 局部变量表



是一组变量值储存空间,用于存放`方法参数与编译器可知的基本数据类型`,和方法内部定义的`局部变量`,局部变量表以`变量槽`为最小单位, 一个变量槽可以存放一个32位内的数据类型,boolean、byte、char、short、int、float、reference和returnAddress类型的数据,则遇到一个64位数据类型的变量(如long或double型)，则会连续使用两个连续的Slot来存储,



##### 局部变量表的数据什么时候保存的? 

> 在 Java 程序编译为 Class 文件时，就在方法的 Code 属性的 max_locals 数据项中确定了该方法所需要分配的局部变量表的最大容量





#### 操作数栈



虚拟机栈吧操作数栈当做工作区, 大多数指令都要从这里弹出数据, 执行运行, 然后结果压回操作数栈中,

存储的数据类型,虚拟机在操作数栈中存储数据的方式和在局部变量区中是一样的：如int、long、float、double、reference和returnType的存储。对于byte、short以及char类型的值在压入到操作数栈之前，也会被转换为int

栈中的任何一个元素都是可以任意的Java数据类型。`32bit的类型占用一个栈单位深度,64bit的类型占用两个栈单位深度`





#### 动态链接



每一个栈帧内部都包含一个指向该栈帧在运行时常量池的引用, 包含这个引用的目的就是为了支持当前代码能够实现动态链接



### 本地方法栈



本地方法栈 与 `虚拟机栈`发挥的功能非常相似, **他们之间的区别不过是, `虚拟机栈服务的是java方法`, 而 本地方法栈服务的是native方法**

甚至有的虚拟机`(Sun HotSpot)虚拟机`, 直接将本地方法栈和虚拟机栈合二为一, 与虚拟栈一样, 本地方法栈区域也会抛出 StackOverflowError 和 OutOfMemoryError 异常





### 程序计数器



程序计数器,每个线程都有自己的程序计数器, 主要是指向`下一行要执行的指令`,由`执行引擎`来执行下一条指令, 每个线程独有就解决了在`处理器(CPU)切换上下文`后线程还能恢复到正确的位置, 如果线程执行 Java 方法，这个计数器记录的是正在执行的虚拟机字节码指令的地址；如果执行的是 Native 方法，计数器值为Undefined。







