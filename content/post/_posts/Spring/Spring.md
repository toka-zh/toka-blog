---
draft: true # 是否为草稿
title: Spring学习
date: 2020-07-27 15:16:29
---

# *IoC*容器

> *控制反转是一种思想,一种模式,而Spring的IOC容器是实现了这种思想这种模式的一个载体。*



> **控制反转**（Inversion of Control，缩写为**IoC**），是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的***耦合度***。其中最常见的方式叫做**依赖注入**（Dependency Injection，简称**DI**），还有一种方式叫“依赖查找”（Dependency Lookup）。
>
> 通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体，将其所依赖的对象的引用传递(注入)给它。

控制反转：将对象交给Spring来创建，管理，装配。

## 元数据配置

XML配置模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>
</beans>
```





## 基于注解的容器配置

依赖包:`org.springframework:spring-aop`

xml配置文件

## 基于XML的容器配置

### 别名

```xml
<alias name="fromName" alias="toName"/>
```

``` xml
<bean id="exampleBean" class="examples.ExampleBean" name="firstName,SecondName ThirdName"/>
```

### 导入多个配置

```xml
<import resource="beans.xml"/>
```



### 容器对象的创建使用

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");//使用XML配置实例化容器
context.getBean("BeanName");//使用容器
```





## 依赖注入(DI)

[官方文档](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-collaborators)

### 注入方式

#### 构造函数注入

在XML的bean中使用`constructor-arg`标签注入数据

##### 参数类型匹配

通过`type`属性来匹配对应的参数

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```



##### 参数索引

通过`index`属性来注入具体位置的参数

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

##### 参数名称

通过构造函数的参数名称来竹图对应参数

``` xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

#### Setter注入

```
bean | ref | idref | list | set | map | props | value | null
```

###### 

```XML
<bean id="exampleBean" class="examples.ExampleBean">
    <property name="examplename">
    	<array>
        	<value></value>
        </array>
    </property>
    <property name="examplename2">
    	<list>
        	<value></value>
        </list>
    </property>
    <property name="examplename3">
    	<map>
        	<entry key="" value="">
        </map>
    </property>
    <property name="examplename4">
        <null/>
    </property>
    <property name="examplename5">
    	<props>
        	<prop key=""> </prop>
        </props>
    </property>
</bean>
```



#### 扩展方式注入

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

p命名空间注入用于注入属性值

c命名空间注入通过构造方法`construct-args`注入



### Bean的作用域

使用：在`bean标签`中添加`scope`属性

| 范围                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [singleton](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-custom) | (默认)为每个 Spring IoC 容器的单个 object 实例定义单个 bean 定义。 |
| [prototype](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-singleton) | 为任意数量的 object 实例定义单个 bean 定义。                 |
| [request](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-prototype) | 将单个 bean 定义范围限定为单个 HTTP 请求的生命周期。也就是说，每个 HTTP 请求都有自己的 bean 实例，该实例是在单个 bean 定义的后面创建的。仅在 web-aware Spring `ApplicationContext`的 context 中有效。 |
| [session](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-request) | 将单个 bean 定义范围限定为 HTTP `Session`的生命周期。仅在 web-aware Spring `ApplicationContext`的 context 中有效。 |
| [application](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-session) | 将单个 bean 定义范围限定为`ServletContext`的生命周期。仅在 web-aware Spring `ApplicationContext`的 context 中有效。 |
| [WebSocket](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#beans-factory-scopes-application) | 将单个 bean 定义范围限定为`WebSocket`的生命周期。仅在 web-aware Spring `ApplicationContext`的 context 中有效。 |



## 自动装配Bean

* 自动装配是满足Bean以来的一种方式



`bean`中的`autowire`属性



byname：会在容器上下文中查找，和自己对象set方法对应的beanid（需要保证所有bean的id唯一，并且bean需要和自动注入属性的set方法一致）

bytype：会在容器上下文中查找，和自己对象类型对应的beanid（需要保证所有bean的类型唯一，并且bean需要和自动注入属性的类型一致）



使用注解实现自动装配



通过在`xmlns`中注册来使用注解装配功能

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

`<context:annotation-config/>`为启用注解支持

idea中只用` <context:annotation-config/>`标签就可以实现自动注册

`@Autowired` (ByType)使用类型注入，而多个bean引用一个实体类时，需要使用`@Qualifier`按照id名注入

@Qualifier(value="name")



@Autowire = ByType   @Autowired+@Qualifier  =  byType || byName

@Resource 类似于@Autowired：先使用byName 找不到再使用byType

