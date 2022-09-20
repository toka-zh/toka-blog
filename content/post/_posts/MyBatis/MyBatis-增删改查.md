---
draft: true # 是否为草稿
title: MyBatis的增删改查   #文章名
date: 2019‎-12‎-27‎ ‏‎‏‎23:28:59
status: public          #公开
tags: MyBatis      #tag
categories: SSM           #文章分类
---

# mybatis的增删改查SIUD

## 1.Select

### 1.1 编写接口

```java
User getUserById(int id);
```



### 1.2 编写对应mapper中的sql语句

```xml
<select id="getUserById" resultType="com.rn.pojo.User">
        select * from mybatis.user where id = #{id};
</select>
```



### 1.3 测试

```java
@Test
public void getUserById(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    User user = mapper.getUserById(1);
    System.out.print(user.toString());
    sqlSession.close();
}
```

## 2.Insert

### 2.1 编写接口

```java
int addUser(User user);
```



### 2.2 编写对应mapper中的sql语句

```xml
<insert id="addUser" parameterType="com.rn.pojo.User">
	insert into mybatis.user (id, name, pwd) value (#{id},#{name},#{pwd});
</insert>
```

### 2.3 测试

```java
@Test
public void addUser(){
    User user = new User(2,"李四","1231435");
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    mapper.addUser(user);
    sqlSession.commit();
    sqlSession.close();
}
```



## 3.Update

### 3.1 编写接口

```java
int updateUser(User user);
```



### 3.2 编写对应mapper中的sql语句

```xml
<update id="updateUser" parameterType="com.rn.pojo.User">
	update mybatis.user set name=#{name},pwd=#{pwd} where id = #{id};
</update>
```

### 3.3测试

```java
@Test
public void updateUser(){
    User user = new User(2,"王五","1435");
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    mapper.updateUser(user);
    sqlSession.commit();
    sqlSession.close();
}
```



## 4.Delete

### 4.1 编写接口

```java
int deleteUser(int id);
```



### 4.2 编写对应mapper中的sql语句

```xml
<delete id="deleteUser" parameterType="int">
	delete from mybatis.user where id=#{id};
</delete>
```

### 4.3 测试

```java
@Test
public void deleteUser(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    sqlSession.commit();
	sqlSession.close();
}
```



注意点：增删改要提交事务

## 5 万能的Map

使用map传递参数可以摆脱数据库表的映射类的束缚

### 5.1 编写接口

```java
int addUser1(Map<String,Object> map);
```



### 5.2 编写对应mapper中的sql语句

```xml
<insert id="addUser1" parameterType="map">
    insert into mybatis.user (id, name, pwd) values (#{userid},#{username},#{password})
</insert>
```

### 5.3测试

```java
@Test
public void addUserofMap(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    Map<String,Object> map = new HashMap<String, Object>();

    map.put("userid",6);
    map.put("username","hello");
    map.put("password","124235");

    mapper.addUser1(map);
    sqlSession.commit();
    sqlSession.close();
}
```

Map传递参数，在sql中取出对应的key parameterType="map"

传递参数，在sql中取出对象的属性  parameterType="Object"

多个参数用Map ，或者***注解***



## 6 模糊查询

代码执行时，传递通配符

```java
List<User> userList = mapper.getUserLike("%张%")：
```

在sql拼接中使用通配符

```sql
select *from mybatis.user where name like "%"#{value}"%"
```

## 7 解决属性名和字段名不一致

resultMap

结果集映射

```xml
<resultMap id="UserMap" type="User">
    <id column="id" property="id"/>
    <result column="name" property="username"/>
    <result column="pwd" property="password"/>
</resultMap>

<select id="getUserById" resultMap="UserMap" parameterType="int">
    select * from mybatis.user where id = #{id};
</select>
```



