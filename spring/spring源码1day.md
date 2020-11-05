

环境搭建完毕以后已三行代码为起点, 我们走入Spring的世界

```java
public class MyTestApplication {

	public static void main(String[] args) {
		XmlBeanFactory beanFactory = new XmlBeanFactory(new ClassPathResource("services.xml"));
		Object bean = beanFactory.getBean("userTest");
		System.out.println(bean);
	}
}
```



当代码运行起来以后我们看到会先加载`new ClassPathResource("services.xml")`, 获取到配置文件, 具体如何获取的我们留下一个问题!!??. 最后解答.

当获取到配置文件以后, 初始化XmlBeanFactory类, 

```java
public class XmlBeanFactory extends DefaultListableBeanFactory {

	private final XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(this);


	/**
	 * Create a new XmlBeanFactory with the given resource,
	 * which must be parsable using DOM.
	 * @param resource XML resource to load bean definitions from
	 * @throws BeansException in case of loading or parsing errors
	 */
	public XmlBeanFactory(Resource resource) throws BeansException {
		// 调用 XmlBeanFactory(Resource resource, BeanFactory parentBeanFactory) 构造方法
		this(resource, null);
	}

	/**
	 * Create a new XmlBeanFactory with the given input stream,
	 * which must be parsable using DOM.
	 * @param resource XML resource to load bean definitions from
	 * @param parentBeanFactory parent bean factory
	 * @throws BeansException in case of loading or parsing errors
	 */
	public XmlBeanFactory(Resource resource, BeanFactory parentBeanFactory) throws BeansException {
		super(parentBeanFactory);
		this.reader.loadBeanDefinitions(resource);
	}

}
```



第一个代码调用了`public XmlBeanFactory(Resource resource)` 方法, 传递了resource对象, 在 `XmlBeanFactory(Resource resource)`方法中调用了`XmlBeanFactory(Resource resource, BeanFactory parentBeanFactory)` 方法, `XmlBeanFactory(Resource resource, BeanFactory parentBeanFactory)` 方法的第一行调用了父类的构造方法, 主要的代码

```java
	public AbstractAutowireCapableBeanFactory() {
		super();
		// 忽略指定接口的自动装配功能
		ignoreDependencyInterface(BeanNameAware.class);
		ignoreDependencyInterface(BeanFactoryAware.class);
		ignoreDependencyInterface(BeanClassLoaderAware.class);
		// 当A中有属性B, 那么当Spring在获取A的Bean的是如果其属性B没有初始, 那么Spring将自动初始化B,
		// 如果B没有自动初始化, 那么有可能被忽略了, 或者实现了被忽略的接口
	}
```

忽略了指定接口的自动封装,我们留下第二个问题!!?? 为什么忽略这些接口...



我们继续向下走看`this.reader.loadBeanDefinitions(resource)` 方法调用

```java
	@Override
	public int loadBeanDefinitions(Resource resource) throws BeanDefinitionStoreException {
		return loadBeanDefinitions(new EncodedResource(resource));
	}
```

我们看到了resource封装了一层EncodedResource,对资源文件编码进行处理, 我们继续向下看`loadBeanDefinitions(new EncodedResource(resource))`的核心代码

```java
// 通过属性判断已经加载的资源
		Set<EncodedResource> currentResources = this.resourcesCurrentlyBeingLoaded.get();
		if (currentResources == null) {
			currentResources = new HashSet<>(4);
			this.resourcesCurrentlyBeingLoaded.set(currentResources);
		}
		if (!currentResources.add(encodedResource)) {
			throw new BeanDefinitionStoreException(
					"Detected cyclic loading of " + encodedResource + " - check your import definitions!");
		}
		try {
			// 从encodedResource中获取resource对象, 并且获取InputStream对象
			InputStream inputStream = encodedResource.getResource().getInputStream();
			try {
				//
				InputSource inputSource = new InputSource(inputStream);
				if (encodedResource.getEncoding() != null) {
					inputSource.setEncoding(encodedResource.getEncoding());
				}
				// 进入核心逻辑部门
				return doLoadBeanDefinitions(inputSource, encodedResource.getResource());
			} finally {
				// 关闭流
				inputStream.close();
			}
```



代码中已经加入了注解就不进行多余的陈述, 我们直接进入核心逻辑方法`doLoadBeanDefinitions(inputSource, encodedResource.getResource())`

```java
	protected int doLoadBeanDefinitions(InputSource inputSource, Resource resource)
			throws BeanDefinitionStoreException {
			// 1.在doLoadDocument方法中验证xml,保证XML正确性
			// 2.加载xml文件封装为Document对象
			Document doc = doLoadDocument(inputSource, resource);
			// 3.根据Document注册Bean信息
			return registerBeanDefinitions(doc, resource);
```

在 `doLoadDocument` 方法中对XML格式进行了验证, 

```java
	protected Document doLoadDocument(InputSource inputSource, Resource resource) throws Exception {
		// 封装Document对象
		return this.documentLoader.loadDocument(inputSource, getEntityResolver(), this.errorHandler,
				// 验证XML格式,
				getValidationModeForResource(resource), isNamespaceAware());
	}
```



`getValidationModeForResource` 验证XML格式主要包含判断当前xml文件生命的验证方式是DTD还是XSD,  我们简单说一下DTD与XSD的区别



DTD : Document type definition 文档类型定义, 是一种XML约束语言, 是XML文件的验证机制, 可以通过XML文档与DTD文档来看文档是否符合规范, 元素和标签是否正确, 一个DTD文档包括 : 元素的定义规则, 元素与元素之间的关系. 元素可使用的属性, 可以使用的实体或符号规则

要想使用DTD必须在XML文件头部声明, 以下是在spring中使用DTD声明方式

```XML
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" " http://www.springframework.org/dtd/spring-beans.dtd ">  
```



XSD: XML Schema 语言就是XSD(XML Schema Difinition).XML Schema  描述了XML的结构,可以使用一个指定的XML Schema 来验证某个XML, 来检查XML文件是否符合要求,文档设计者可以通过XML Schema 来指定XML的所允许的结构与内容

要向在Spring中声明XSD方式

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
```





今天的学习就到这里了, 明天学习封装Document与注册解析BeanDifinitions!!!!



















## new ClassPathResource("services.xml") 如何获取的配置文件 ??



## 为什么忽略BeanNameAware/BeanFactoryAware/BeanClassLoaderAware 接口?



