---
draft: true # 是否为草稿
title: MyBatis之使用注解开发   #文章名
date: 2020‎-1-4‎ ‏‎7:39:51
status: public          #公开
tags: MyBatis      #tag
categories: SSM           #文章分类
---

## 一级缓存（本地缓存）

- 映射语句文件中的所有 select 语句的结果将会被缓存。
- 映射语句文件中的所有 insert、update 和 delete 语句会刷新缓存。
- 缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存。
- 缓存不会定时进行刷新（也就是说，没有刷新间隔）。
- 缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用。
- 缓存会被视为读/写缓存，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改。



小结：默认开启，且无法关闭，只在SqlSession生命周期内有效

相当于一个map

## 二级缓存

开启全局缓存（在Mapper.xml中使用二级缓存）

```xml
<setting name="cacheEnabled" value="true"/>
```



在要使用二级缓存中的Mapper中开启

```xml
<cache/>
```



NotSerializableException：未序列化，目标类序列化即可



小结：

* 只要开启了二级缓存，同一个Mapper下就有效
* 所有数据都会放在一级缓存中
* 当sqlsession关闭后才会提交到二级缓存

