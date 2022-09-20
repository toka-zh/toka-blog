---
draft: true # 是否为草稿
title: MyBatis-配置解析   #文章名
date: 2019‎-12‎-28 ‏‎13:44:23
status: public          #公开B
tags: MyBatis      #tag
categories: SSM           #文章分类
---

# MyBatis配置解析

---

**核心配置文件：mybatis-config.xml**

包含以下内容

```xml
configuration（配置）
	properties（属性）
	settings（设置）
	typeAliases（类型别名）
	typeHandlers（类型处理器）
	objectFactory（对象工厂）
	plugins（插件）
	environments（环境配置）
		environment（环境变量）
			transactionManager（事务管理器）
			dataSource（数据源）
	databaseIdProvider（数据库厂商标识）
	mappers（映射器）
```

## 属性（properties）

通过properties属性引用配置文件

编写外部配置文件db.properties

```properties
driver = com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true
username=root
password=123456
```

在核心配置文件中使用properties引入外部配置文件，可以在其标签对中添加属性，

```xml
<properties resource="db.properties"></properties>
```

如果添加的属性与外部配置文件重复，系统优先使用外部配置文件

## 类型别名（typeAliases）

 类型别名是为 Java 类型设置一个短的名字。 它只和 XML 配置有关，存在的意义仅在于用来减少类完全限定名的冗余。

1. 直接给实体类起别名

```xml
<typeAliases>
    <typeAlias type="com.rnplus.pojo.User" alias="User"/>
</typeAliases>
```

2. 指定一个包，mybatis会扫描包路径下的实体类，默认别名就是类的小写类名（大写也可以）

```xml
<typeAliases>
    <package name="com.rnplus.pojo"/>
</typeAliases>
```

3. 使用注解起别名（优先级最高）

```java
@Alias("user")
public class User(){
}
```

## 设置（settings）

比较常用的就下面这三了。。。

| 设置名             | 描述                                                         | 有效值                                                       | 默认值 |
| :----------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----- |
| cacheEnabled       | 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存。   | true \| false                                                | true   |
| lazyLoadingEnabled | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false                                                | false  |
| logImpl            | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。        | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置 |

## 映射器（mapper）

***MapperRegister：注册和绑定mapper文件***

1.使用资源路径，通过xml注册（推荐使用）

```xml
<mappers>
    <mapper resource="com/rnplus/dao/UserMapper.xml"/>
</mappers>
```

2.使用绝对路径（垃圾）

3.使用class文件绑定注册

* 接口和对应的Mapper配置文件必须同名

* 接口和配置文件必须在同一包下

```xml
<mappers>
    <mapper resource="com.rnplus.dao.UserMapper"/>
</mappers>
```

4.注册包内所有映射

* 接口和对应的Mapper配置文件必须同名

* 接口和配置文件必须在同一包下

```xml
<mappers>
    <package name="com.rnplus.dao"/>
</mappers>
```

## 插件

常用插件	mybatis-plus

​				mybatis-generator-core

## 生命周期和作用域

错误使用会导致严重的并发问题

### SqlSessionFactoryBuilder

* 在创建SqlSessionFactory后，就不再需要

* 局部变量

### SqlSessionFactory

* 一旦创建就应该一直存在，*** 没有任何理由丢弃或重新创建***
* 最佳作用域是应用作用域
* 使用单例模式或者静态单例模式

### SqlSession

* 连接到连接池的一个请求

* 用完之后及时关闭，避免资源占用