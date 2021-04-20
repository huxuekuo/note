# ClassLoader(1.8)



| ClassLoaderName         | 类型                                                         |
| ----------------------- | ------------------------------------------------------------ |
| BootstrapClassLoader    | 负责加载 JVM 运行时核心类，这些类位于 JAVA_HOME/lib/rt.jar 文件中 |
| ExtClassLoader          | 负责加载 JVM 扩展类，比如 swing 系列、内置的 js 引擎、xml 解析器 等等 |
| AppClassLoader          | 接面向我们用户的加载器，它会加载 Classpath 环境变量里定义的路径中的 jar 包和目录 |
| URLClassLoader          | 传递规范的网络路径给构造器，就可以使用 URLClassLoader 来加载远程类库了 |
| `用户自定义ClassLoader` |                                                              |

## 延迟加载



JVM 一次不会加载所有需要的类, 程序在运行中会逐渐遇到很多`不认识的新类, 这时候会调用ClassLoader加载这些类`. 加载完成后Class对象会保存在ClassLoader里面, 下次就不会要加载了



## ClassLoader传递性



上面提到,`不认识的新类, 这时候会调用ClassLoader加载这些类`, 这个未知的类, 他会选择哪一种ClassLoader加载他呢?  

> 虚拟机会选择, 调用这个`未知类`对象的ClassLoader来加载`未知类`



## 双亲委派



上面`Class传递性`提到, 当遇到一个`未知类`, 会使用调用者的`ClassLoader`加载`未知类`, 但是如果Main方法运行的过程中遇到了系统类库怎么办? 我们知道Main方法的`ClassLoader`是**ApplcaitionClassLoader**,这个时候就需要`双亲委派`



**ApplcaitionClassLoader** 在加载一个未知的类名时，它并不是立即去搜寻 Classpath，它会首先将这个类名称交给 **ExtensionClassLoader** 来加载，如果 **ExtensionClassLoader** 可以加载，那么 **ApplcaitionClassLoader** 就不用麻烦了。否则它就会搜索 Classpath 。



> 1. 可以避免类的重复加载, 当父类已经加载了, 子类就不会在加载





## ClassLoader的四个关键方法





### loadClass

```java
protected Class<?> loadClass(String name, boolean resolve)
        throws ClassNotFoundException
    {
        synchronized (getClassLoadingLock(name)) {
            // First, check if the class has already been loaded
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                long t0 = System.nanoTime();
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    long t1 = System.nanoTime();
                    c = findClass(name);

                    // this is the defining class loader; record the stats
                    sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                    sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                    sun.misc.PerfCounter.getFindClasses().increment();
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }
```



> 1. 首先判断自己是否加载了这个类
> 2. 如果没有加载,交给父类, 如果父类为空交个根加载器
> 3. 如果都没有加载到类
> 4. 调用findClass方法查询类
> 5. findClass 方法内部调用了defineClass加载类

### findClass

```java
protected Class<?> findClass(String name) throws ClassNotFoundException {
        throw new ClassNotFoundException(name);
    }
```

> 这是一个空方法, 需要子类自己去实现, 比如`AppClassLoader`的实现, 下面展示一下

```java
protected Class findClass(String name) throws ClassNotFoundException {
        StringBuilder sb = new StringBuilder(name.length() + 6);
        sb.append(name.replace('.', '/')).append(".class");
        InputStream is = this.getResourceAsStream(sb.toString());
        if (is == null) {
            throw new ClassNotFoundException("Class not found" + sb);
        } else {
            Class var19;
            try {
                ByteArrayOutputStream baos = new ByteArrayOutputStream();
                byte[] buf = new byte[1024];

                int len;
                while((len = is.read(buf)) >= 0) {
                    baos.write(buf, 0, len);
                }

                buf = baos.toByteArray();
                int i = name.lastIndexOf(46);
                if (i != -1) {
                    String pkgname = name.substring(0, i);
                    Package pkg = this.getPackage(pkgname);
                    if (pkg == null) {
                        this.definePackage(pkgname, (String)null, (String)null, (String)null, (String)null, (String)null, (String)null, (URL)null);
                    }
                }

               var19 = this.defineClass(name, buf, 0, buf.length);
            } catch (IOException var17) {
                throw new ClassNotFoundException(name, var17);
            } finally {
                try {
                    is.close();
                } catch (IOException var16) {
                }

            }

            return var19;
        }
    }
```



### defineClass

```java
    protected final Class<?> defineClass(String name, byte[] b, int off, int len,
                                         ProtectionDomain protectionDomain)
        throws ClassFormatError
    {
        protectionDomain = preDefineClass(name, protectionDomain);
        String source = defineClassSourceLocation(protectionDomain);
        Class<?> c = defineClass1(name, b, off, len, protectionDomain, source);
        postDefineClass(c, protectionDomain);
        return c;
    }
```

> 加载类

### resolveClass





