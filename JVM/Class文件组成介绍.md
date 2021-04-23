# Class文件组成介绍

## 描述符概述

> 每种基本数据类型都有一个大写字母做对应， void也有一个大写字符做对应

| 基本数据类型和void类型 | 类型的对应字符 |
| ---------------------- | -------------- |
| byte                   | B              |
| char                   | C              |
| double                 | D              |
| float                  | F              |
| int                    | I              |
| long                   | J              |
| short                  | S              |
| boolean                | Z              |
| void                   | V              |



**那么引用类型（类和接口，枚举）在描述符中是如何对应的呢?**

>  “L” + 类型的全限定名 + “;”
>
> 比如Object :  Ljava/lang/Object; 
>
> 自定义类型com.example.Person在描述符中的对应字符串是： Lcom/example/Person

**数组类型描述符 :** 

> 若干个“[”  +  数组中元素类型的对应字符串 
>
>  int[]类型的对应字符串是： [I 
>
> int[/]/[]类型的对应字符串是： [[I 
>
> Object[]类型的对应字符串是： [Ljava/lang/Object;
>
> Object[/]/[/]类型的对应字符串是： [[Ljava/lang/Object;

**字段方法描述符**

下面举例说明（此表格来源于《深入Java虚拟机》）。

| 方法描述符 | 方法声明 |
| ---------- | -------- |
|()I	|int getSize()|
|()Ljava/lang/String;|	String toString()|
|([Ljava/lang/String;)V	|void main(String[] args)|
|()V	|void wait()|
|(JI)V	|void wait(long timeout, int nanos)|
|(ZILjava/lang/String;II)Z|	boolean regionMatches(boolean ignoreCase, int toOffset, String other, int ooffset, int len)|
|([BII)I	|int read(byte[] b, int off, int len )|
|()[[Ljava/lang/Object;|	Object[][] getObjectArray()|

 

## Class文件概述

```
ClassFile {
    u4             magic;
    u2             minor_version;
    u2             major_version;
    u2             constant_pool_count;
    cp_info        constant_pool[constant_pool_count-1];
    u2             access_flags;
    u2             this_class;
    u2             super_class;
    u2             interfaces_count;
    u2             interfaces[interfaces_count];
    u2             fields_count;
    field_info     fields[fields_count];
    u2             methods_count;
    method_info    methods[methods_count];
    u2             attributes_count;
    attribute_info attributes[attributes_count];
}
```

### **magic**

>  魔术值提供用于标识类文件格式的魔术数字； 它的值为0xCAFEBABE。

###  **minor_version, major_version**

> `minor_version`和`major_version`项目的值是此类文件的次要版本号和主要版本号。 主版本号和次版本号共同决定了类文件格式的版本。 如果类文件的主版本号为M，次版本号为m，则将其类文件格式的版本表示为M.m。 因此，可以按字典顺序对类文件格式的版本进行排序，例如1.5 <2.0 <2.1。

###  **constant_pool_count**

>  该`constant_pool_count`项目的值 等于表中的条目数`constant_pool`加一。一个 `constant_pool`指数被认为是有效的，如果它比大于零且少`constant_pool_count`，除了类型的常量`long`和`double`

###  **constant_pool[]**

>  `constant_pool`是代表各种串常量，类和接口名，字段名，和本内提到的其他常量, `ClassFile`结构和其子结构。每个`constant_pool`表条目的格式 由其第一个“标签”字节指示。

>  该`constant_pool` 表的索引从1到`constant_pool_count`-1

  - 数据项介绍

     | 常量池中数据项类型 | 	类型标志	| 类型描述 |
     | -------------- | ------ | --------------------------------- |
     | CONSTANT_Utf8	 | 1	 | UTF-8编码的Unicode字符串 |
     | CONSTANT_Integer	 | 3	 | int类型字面值 |
     | CONSTANT_Float	 | 4	 | float类型字面值 |
     | CONSTANT_Long	 | 5	 | long类型字面值 |
     | CONSTANT_Double	 | 6	 | double类型字面值 |
     | CONSTANT_Class	 | 7	 | 对一个类或接口的符号引用 |
     | CONSTANT_String	 | 8	 | String类型字面值 |
     | CONSTANT_Fieldref	 | 9	 | 对一个字段的符号引用 |
     | CONSTANT_Methodref	 | 10	 | 对一个类中声明的方法的符号引用 |
     | CONSTANT_InterfaceMethodref	 | 11	 | 对一个接口中声明的方法的符号引用 |
     | CONSTANT_NameAndType	 | 12	 | 对一个字段或方法的部分符号引用 |

###  **access_flags**

>  该`access_flags`项目的值 是标志的掩码，用于表示对该类或接口的访问权限以及该类或接口的属性

  - | 标志名称       | Value  | 备注                              |
    | -------------- | ------ | --------------------------------- |
    | ACC_PUBLIC     | 0x0001 | 宣布public修饰,可以从外部进行访问 |
    | ACC_FINAL      | 0x0010 | 被final修饰, 不允许子类继承, 重写 |
    | ACC_SUPER      | 0x0020 | 如何调用父类的方法                |
    | ACC_INTERFACE  | 0x0200 | 标识这是一个接口                  |
    | ACC_ABSTRACT   | 0x0400 | 这是一个抽象类, 不能实例化        |
    | ACC_SYNTHETIC  | 0x1000 | 表示合成, 源代码中不存在          |
    | ACC_ANNOTATION | 0x2000 | 声明这是一个注解类型              |
    | ACC_ENUM       | 0x4000 | 声明这是一个枚举类型              |

###  **this_class**

>  该`this_class`项目的值 必须是`constant_pool`表中的有效索引。该`constant_pool`索引处的条目必须是表示此文件定义的类或接口的`CONSTANT_Class_info`结构class

###  **super_class**

>  对于一个类，`super_class`值必须为零或必须是`constant_pool`表中的有效索引。 如果`super_class`的值不为零，则`constant_pool`条目必须为`CONSTANT_Class_info`结构，该结构表示此类文件定义的类的直接超类。 直接超类或其任何超类都不能在其ClassFile结构的access_flags项中设置ACC_FINAL标志。

**如果`super_class`的值为零，则该类文件必须表示类Object，这是没有直接超类的唯一类或接口。**

**对于接口，`super_class`的值必须始终是`constant_pool`表中的有效索引。 `constant_pool`条目必须是代表类Object的`CONSTANT_Class_info`结构。*



### interfaces_count

> interfaces_count的值给出了此类或接口类型的直接超级接口的数量


###  **interfaces[interfaces_count]**
>  `interfaces`数组中的每个值都必须是`constant_pool`表中的有效索引。 `interfaces [i]`的每个值**（其中0≤i <interfaces_count）**的`constant_pool`条目必须是`CONSTANT_Class_info`结构

**注意， 只有当前类直接实现的接口才会被统计， 如果当前类继承了另一个类， 而另一个类又实现了一个接口， 那么这个接口不会统计在当前类的interfaces_count中**



### `fields_count`

> `fields_count`的值给出了**fields**表中`field_info`结构的数量。 `field_info`结构.表示此类或接口类型声明的所有字段，包括类变量和实例变量

###   **fields[fields_count]**

`fields`的每个值都必须是`field_info`结构，以提供对该类或接口中字段的完整描述。 字段表仅包含由此类或接口声明的那些字段。 **它不包括代表从超类或超接口继承的字段**

**注意， 这里包括静态字段， 但不包括从父类继承的字段**





#### `field_info` 介绍

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210422182219.png" style="zoom:33%;" />

- name_index : 这是一个指向常量池的索引， 它描述的是当前字段的字段名
- Descriptor_index : 指向常量池的索引， 它描述的是当前字段的描述符
- Attributes_count/attributes: 对当前字段所具有的属性的描述,属性和源文件中的属性不是同一个概念， 在源文件测层面中， 属性是字段的另一种叫法
  - attributes_count表示这个字段有几个属性
  - 可以出现在filed_info中的属性有三种， 分别是ConstantValue， Deprecated， 和 Synthetic

###   **methods_count**

​		`methods_count`的值给出了`methods`表中`method_info`结构的数量

###  **methods[methods_count]**



​		方法表中的每个值都必须是`method_info`结构，以提供对该类或接口中方法的完整描述。 如果在method_info结构的`access_flags`项中均未设置ACC_NATIVE和ACC_ABSTRACT标志，则还将提供实现该方法的Java虚拟机指令。

`method_info`表示此类或接口类型声明的所有方法，包括实例方法，类方法，实例初始化方法以及任何类或接口初始化的方法。 **方法表不包括表示从超类或超接口继承的方法的项**。



#### method_info 介绍

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210422182219.png" style="zoom:33%;" />

- name_index : 这是一个指向常量池的索引， 它描述的是当前方法的方法名
- Descriptor_index : 指向常量池的索引， 它描述的是当前方法的描述符
- Attributes_count/attributes: 对当前方法所具有的属性的描述,属性和源文件中的属性不是同一个概念， 在源文件测层面中， 属性是字段的另一种叫法
  - attributes_count表示这个字段有几个属性
  - 可以出现在filed_info中的属性有三种，Code， Deprecated， Exceptions 和Synthetic

### **attributes_count**

> `attribute_count`的值提供此类的属性表中的属性数

###  **attributes[attributes_count]**

`attributes` 数据中每一项是一个 `attribute_info` 属性, 

**需要说明的是， 属性会出现在多个地方， 不仅仅出现在顶层的ClassFile中， 也会出现在class文件中的数据项中， 如出现在field_info中， 用来描述特定字段的一些信息， 还可以出现在method_info中， 用来描述特定方法的一些信息**



#### Attribute_info 

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210422183200.png" style="zoom:40%;" />

`attribute_name_index` : attribute_name_index占两个字节， 它是一个指向常量池数据项的索引。 它指向一个CONSTANT_Utf8_info ， 这个CONSTANT_Utf8_info 中存放的是当前属性的名字



`attribute_length`: 它表示当前属性的长度， 这个长度不包括前6个字节， 也就是说只包括属性真实信息（也就是info）的长度。



`Info` : info， 它的长度由上面提到的attribute_length指定， 它存放的是真实的属性数据



#### ClassFile中存在的属性



##### `SourceFile`



该属性出现在顶层的class文件中。 它描述了该类是从哪个源文件中编译来的， 注意， 描述的是源文件， 而不是类， 一个源文件中可以存在多个类。



 attribute_name_index指向常量池中的一个CONSTANT_Utf8_info ， 这个CONSTANT_Utf8_info 中存放的是这个属性的名字字符串， 即“SourceFile” 。 

attribute_length是属性信息的长度， 这里是2， 因为这个属性的info就两个字节

sourcefile_index指向常量池中的一个CONSTANT_Utf8_info ， 这个CONSTANT_Utf8_info 中存放的是生成该类的源文件的文件名， 这里的文件名不包括路径部分



```java
  #17 = Utf8               SourceFile
  #18 = Utf8               PersonSS.java
		....
    ....
SourceFile: "PersonSS.java"
  // 省略部分源文件
```



##### `InnerClasses`



InnerClasses是一个存在于顶层class文件中的属性， 它描述的是内部类和外围类的关系

每个类可能有多个内部类， 而这些内部类中可能还有内部类， 多层嵌套。外围类中的InnerClasses属性必须描述它的所有内部类， 而内部类中的InnerClasses也必须描述它的外围类

**这个属性相对较为复杂， 而对于我们理解class文件又不具有很大的意义， 所以我们只是简单的介绍一下**

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210423103516.png" style="zoom:33%;" />

前两个就不介绍了, number_of_classes 是描述具体内部类个数

classes 可以看做一个数组, 这个数组每一个项是inner_class_info, 

<img src="/Users/edz/Library/Application Support/typora-user-images/image-20210423103809561.png" alt="image-20210423103809561" style="zoom:33%;" />



##### `Synthetic`



`Synthetic`属性可以出现在`filed_info`中， `method_info`中和顶层的`ClassFile`中， 分别表示这个**字段， 方法或类不是有用户代码生成的（即不存在与源文件中）， 而是由编译器自动添加的例如**， 编译器会为内部类增加一个字段， 该字段是对外部类对象的引用； 如果一个不定义构造方法， 那么编译器会自动添加一个无参数的构造方法<init>， 如果定义了静态字段或静态代码块， 还会根据具体情况， 增加静态初始化方法<clinit> 。 此外， 有些机制， 如动态代理， 会在运行时自动生成字节码文件， 由于这些类不是由源文件中编译来的， 所以这些类的class文件中会有一个Synthetic属性。 



<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210423104218.png" style="zoom:33%;" />

##### `ConstantValue`



`ConstantValue`属性出现在`class`文件中的`field_info`中， 也就是说它是一个和字段相关的属性。 每个`field_info`中最多只能出现一个`ConstantValue`属性。 此外， 要注意的是， 必须是静态字段才可以有`ConstantValue`属性。 这个静态字段可以是`final`的， 也可以不是`final`的,只有**基本数据类型或String类型的静态变量**才可以存在`ConstantValue`属性

静态初始化有两种方式:

	1. constantValue 属性
 	2. 静态初始化方法<clinit> 

不同编译器有不同的实现, 但是如果虚拟机决定使用constantValue进行赋值, 那么这个变量的复制动作, 必须位于<clinit> 之前



<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210423111325.png" style="zoom:33%;" />

constantvalue_index ， 这是一个指向常量池中某个数据项的索引。这个常量池数据项中存放的就是当前字段的值



常量池中的数据项，根据field_info描述的字段的不同， 可以是不同类型的数据项

- byte， short， char， boolean : CONSTANT_Integer_info
  -  虽然java语言支持byte， short， char， boolean类型， 但是JVM却不支持这几种类型

- String : CONSTANT_String_info
- long : CONSTANT_Long_info



###### 实例说明

```java
package com.jg.zhang;
 
public class Person {
	
	static final int a = 1;
	
	int age;
 
	int getAge(){
		return age;
	}
}
```



```java
 
Constant pool:
 
   #7 = Utf8               ConstantValue
   #8 = Integer            1
 
 
{
  static final int a;
    flags: ACC_STATIC, ACC_FINAL
    ConstantValue: int 1
    .........
}
```



##### `Deprecated`



Deprecated属性可以存在于filed_info中， method_info中和顶层的ClassFile中， 分别表示这个字段， 方法或类已经过时

@deprecated注解。 也就是说， 如果在源文件中为一个字段， 方法或类标注了@deprecated注解， 那么编译器就会在class文件中为这个字段， 方法或类生成一个Deprecated属性 。

<img src="/Users/edz/Library/Application Support/typora-user-images/image-20210423112046732.png" alt="image-20210423112046732" style="zoom:40%;" />

attribute_length永远为0 ， 因为这个属性只是一个标志信息， 用来表示字段， 方法， 类已经过时， 而不具有任何实质性的属性信息

###### 实例说明

```java
package com.jg.zhang;
 
public class Person {
	
	int age;
 
	@Deprecated
	int getAge(){
		return age;
	}
}
```

```java
  ......
  
Constant pool:
  ......
 
  #18 = Utf8               Deprecated
 
  ......
 
{
 
  ......
 
  int getAge();
    flags:
    Deprecated: true
 
    ......
 
}
```

可以看到， 在getAge方法相关的信息中， 有一行 Deprecated: true ， 这说明编译器在getAge方法的method_info中加入了Deprecated属性。 常量池第18项的CONSTANT_Utf8_info中存放的是Deprecated属性的属性名“Deprecated” 。 


## 常量池数据项介绍



### CONSTANT_Utf8_info



> 一个CONSTANT_Utf8_info是一个CONSTANT_Utf8类型的常量池数据项,,它存储的是一个常量字符串。 常量池中的所有字面量几乎都是通过CONSTANT_Utf8_info描述的

**CONSTANT_Utf8_info可包括的字符串主要以下这些**：



- 程序中的字符串常量

- 常量池所在当前类（包括接口和枚举）的全限定名
- 常量池所在当前类的直接父类的全限定名
- 常量池所在当前类型所实现或继承的所有接口的全限定名
- 常量池所在当前类型中所定义的字段的名称和描述符
- 常量池所在当前类型中所定义的方法的名称和描述符
- 由当前类所引用的类型的全限定名
- 由当前类所引用的其他类中的字段的名称和描述符
- 由当前类所引用的其他类中的方法的名称和描述符
- 与当前class文件中的属性相关的字符串， 如属性名等



**总结这么五类： 程序中的字符串常量， 类型的全限定名， 方法和字段的名称， 方法和字段的描述符， 属性相关字符串。**



**源文件中的几乎所有可见的字符串都存放在CONSTANT_Utf8_info中， 其他类型的常量池项只不过是对CONSTANT_Utf8_info的引用。 其他常量池项， 把引用的CONSTANT_Utf8_info组合起来， 进而可以描述更多的信息**。 下面将要介绍的CONSTANT_NameAndType_info就可以验证这个结论。





### CONSTANT_NameAndType_info



常量池中的一个CONSTANT_NameAndType_info数据项， 可以看做CONSTANT_NameAndType类型的一个实例 。



它的描述可以看出两个信息 , 一个是名称(Name), 一个是类型(Type), **这里的名称可以是方法名称, 也可以是字段名称,**

**Type 是 方法的描述 或者是字段的描述**,



<img src="/Users/edz/Library/Application Support/typora-user-images/image-20210421153644824.png" alt="image-20210421153644824" style="zoom:33%;" />

> `序号23:`
>
> ​		#5:#6 表示 age 字段 int 类型
>
> `序号22:`
>
> ​		#5:#6 初始化方法返回值未void

descriptor



### CONSTANT_INTEGER



> 一个常量池中的 CONSTANT_Integer_info 数据项, 可以看做是CONSTANT_Integer类型的一个实例。它存储的是源文件中出现的int型数据的值。

![image-20210421155045879](/Users/edz/Library/Application Support/typora-user-images/image-20210421155045879.png)



**存储模型:** 

int, float 为4个字节

long, double 为 8个字节

![](https://gitee.com/panda_soft/note_images/raw/master/path/20210421160500.png)



> **其他基础类型以此类推**





### CONSTANT_String_info



>  一个CONSTANT_String_info数据项， 是CONSTANT_String类型的一个实例

 可以把他看做是一个存在于class文件中的字符串对象,它指向一个CONSTANT_Utf8_info， 这个CONSTANT_Utf8_info存放的才是字符串的字面量



存储模型: 

​	<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210421160311.png" style="zoom:80%;" />







### CONSTANT_Class_info



常量池中的一个 `CONSTANT_Class_info`， 可以看做是`CONSTANT_Class`数据类型的一个实例。 **他是对类或者接口的符号引用。 它描述的可以是当前类型的信息， 也可以描述对当前类的引用， 还可以描述对其他类的引用**。 也就是说， **如果访问了一个类字段， 或者调用了一个类的方法， 对这些字段或方法的符号引用， 必须包含它们所在的类型的信息**， `CONSTANT_Class_info`就是对字段或方法符号引用中类型信息的描述。 

`CONSTANT_Class_info`的第一个字节是`tag`， 值为`7`， 也就是说， 当虚拟机访问到一个常量池中的数据项， 如果发现它的tag值为7， 就可以判断这是一个`CONSTANT_Class_info` 。 `tag`下面的两个字节是一个叫做`name_index`的索引值， 它指向一个 `CONSTANT_Utf8_info`， 这个`CONSTANT_Utf8_info`中存储了`CONSTANT_Class_info`要描述的类型的**全限定名** 

> **全限定名** :  
>
> ​			**源代码 : java.lang.Object**
>
> ​          **class文件: java/lang/Object**
>
>   源文件中的全新定名是包名加类名， 包名的各个部分之间，包名和类名之间， 使用点号分割。 如**Object**类， 在源文件中的全限定名是`java.lang.Object` 。 而**class**文件中的全限定名是`将点号替换成“/”` 。 例如， **Object**类在**class**文件中的全限定名是 `java/lang/Object` 。 

此外要说明的是， **java中数组变量也是对象， 那么数组也就有相应的类型**， 并且数组的类型也是使用`CONSTANT_Class_info`描述的， 并且数组类型和普通类型的描述有些区别。 普通类型的`CONSTANT_Class_info`中存储的是全限定名， 而数组类型对应的`CONSTANT_Class_info`中存储的是数组类型相对应的描述符字符串。 举例说明：

> 与Object类型对应的CONSTANT_Class_info中存储的是： java/lang/Object 
> 与Object[]类型对应的CONSTANT_Class_info中存储的是： [Ljava/lang/Object; 

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210421161047.png" style="zoom:50%;" />

储存模型: 

​	<img src="/Users/edz/Library/Application Support/typora-user-images/image-20210421160904253.png" alt="image-20210421160904253" style="zoom:60%;" />



### CONSTANT_Fieldref_info



> 该数据项表示对一个字段的符号引用， 可以是对本类中的字段的符号引用， 也可以是对其他类中的字段的符号引用， 可以是对成员变量字段的符号引用
>
> `“符号引用”`这个名词出现了很多次， 可能有的同学一直不是很明白， 等介绍完CONSTANT_Fieldref_info， 就可以很清晰的了解什么是符号引用

class文件反编译: 

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210421161905.png" style="zoom:50%;" />

 **CONSTANT_Fieldref_info就是对一个字段的符号引用， 这个符号引用包括两部分， 一部分是该字段所在的类， 另一部分是该字段的字段名和描述符。 这就是所谓的 “对字段的符号引用”** 



**存储模型:** 

<img src="https://gitee.com/panda_soft/note_images/raw/master/path/20210421163213.png" style="zoom:67%;" />





### CONSTANT_Methodref_info

>  该数据项表示对一个类中**方法的符号引用**， 可以是对`本类`中的方法的符号引用， 也可以是对`其他类`中的方法的符号引用， 可以是对`成员方法`字段的符号引用， 也可以是对`静态方法`的符号引用,但是不会是对`接口中的方法`的符号引用



```java
  #20 = Methodref          #21.#23        //  com/jg/zhang/Computer.calculate:()V
  #21 = Class              #22            //  com/jg/zhang/Computer
  #22 = Utf8               com/jg/zhang/Computer
  #23 = NameAndType        #24:#12        //  calculate:()V
  #24 = Utf8               calculate
```



储存布局图: **省略**



###  CONSTANT_InterfaceMethodref_info

>  该数据项表示对一个接口方法的符号引用， 不能是对类中的方法的符号引用
>
> CONSTANT_InterfaceMethodref和CONSTANT_Methodref_info很相似。既然是符号“引用”， 那么只有在原文件中调用了一个接口中的方法， 常量池中才有和这个被调用方法的相对应的符号引用， 即存在一个CONSTANT_InterfaceMethodref



```java
  #8 = Utf8               ()V
  #19 = InterfaceMethodref #20.#22        //  com/jg/zhang/IFlyable.fly:()V
  #20 = Class              #21            //  com/jg/zhang/IFlyable
  #21 = Utf8               com/jg/zhang/IFlyable
  #22 = NameAndType        #23:#8         //  fly:()V
  #23 = Utf8               fly
```



储存布局图: **省略**



