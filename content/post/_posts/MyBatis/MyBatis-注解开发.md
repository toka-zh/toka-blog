---
draft: true # 是否为草稿
title: MyBatis之使用注解开发   #文章名
date: 2019‎-12‎-27‎ ‏‎20:53:11
status: public          #公开
tags: MyBatis      #tag
categories: SSM           #文章分类
---

### 使用注解开发

----

面向接口编程

根本原因 **解耦**

## 注释用法

1. 注解在接口上实现

   ```java
   @Select("select * from User")
   List<User> getUsers();
   ```

2. 需要在核心配置文件绑定接口

   ```xml
   <mappers>
       <mapper class="com.rnplus.mapper.UserMapper"/>
   </mappers>
   ```

3. 测试

   

   本质：反射机制实现

   底层：动态代理

## 增删改查CRUD

可以在工具类创建时设置自动提交事务

```java
public static SqlSession getSession(){
    return sqlSessionFactory.openSession(true);
}
```

编写接口,增加注解

*必须将接口绑定到核心配置文件*

```java
public interface UserMapper {

    @Select("select * from User")
    List<User> getUsers();
    
    //方法存在多个参数，所有参数前必须加上注解@Param（“”）
    @Select("select * from user where id = #{id}")
    User getUserById(@Param("id") int id);

    @Insert("insert into user(id,name,pwd) values (#{id},#{name},#{pwd})")
    int addUser(User user);

    @Update("update user set name=#{name},pwd=#{pwd} where id=#{id}")
    int updateUser(User user);

    @Update("delete from user where id=#{uid}")
    int deleteUser(@Param("uid") int id);
}
```



## 关于@Param()注解

* 基本类型的参数或String，需要加上
* 引用类型不需要加
* 如果只有一个基本类型，可以忽略，但是建议加上

***#{} 和 ${}的区别***

#{}可以防止SQL注入，${}无法做到

