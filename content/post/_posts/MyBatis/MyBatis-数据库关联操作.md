---
draft: true # 是否为草稿
title: MyBatis之数据库关联操作   #文章名
date: 2019‎-12‎-29‎ 15:25:46
status: public          #公开
tags: MyBatis      #tag
categories: SSM           #文章分类
---

```sql
create table teacher (
    id int(10) not null,
    name varchar(30) default null,
    primary key (id)
);

insert into teacher(id,name) values (1,'一老师');

create table student(
    id int(10) not null,
    name varchar(30) default null,
    tid int(10) default null,
    primary key (id),
    key fktid(tid),
    constraint fktid foreign key (tid) references teacher (id)
)engine =innodb;

insert into student(id,name,tid) values (1,'同学A','1');
insert into student(id,name,tid) values (2,'同学B','1');
insert into student(id,name,tid) values (3,'同学C','1');
insert into student(id,name,tid) values (4,'同学D','1');
insert into student(id,name,tid) values (5,'同学E','1');
```

1. 新建实体类

2. 建立Mapper接口

3. 建立Mapper.xml文件

4. 在核心配置中注册Mapper接口或文件

5. 测试查询

   

# 多对一处理

## 通过查询嵌套处理

```xml
<select id="getStudent" resultMap="studentteacher">
    select *from student;
</select>
<resultMap id="studentteacher" type="Student">
    <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
</resultMap>
<select id="getTeacher" resultType="Teacher">
    select * from teacher where id = #{id}
</select>
```

## 通过结果嵌套处理



```xml
<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id sid,s.name sname,t.name tname
    from student s,teacher t
    where s.tid=t.id;
</select>
<resultMap id="StudentTeacher2" type="Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```



回顾MySQL多对一查询方式：

* 子查询
* 联表查询

# 一对多处理

按照

## 通过结果嵌套查询

```xml
<select id="getTeacher" resultMap="TeacherStudent">
    select s.id sid,s.name sname,t.id tid,t.name tname from student s,teacher t where s.tid=t.id and t.id=#{tid}
</select>

<resultMap id="TeacherStudent" type="teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <collection property="students" ofType="student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```



## 通过查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from teacher where id = #{tid}
</select>

<resultMap id="TeacherStudent2" type="teacher">
    <collection property="students" column="id" javaType="ArrayList" ofType="student" select="getStudent"/>
</resultMap>
<select id="getStudent" resultType="student">
    select * from student where tid=#{tid}
</select>
```

# 小结

1. 关联 - association 【多对一】

2. 集合 - collection 【一对多】

3. **javaType & ofType**

   * javaType 用来指定实体类中属性的类型

   * ofType 依赖指定映射到List或者集合中的pojo类型，泛型中的约束类型

注意点：

* 保证SQL的可读性，尽量保证通俗易懂
* 注意一对多和多对一中，属性名和字段的问题
* 如果问题不好排查错误，可以使用日志，如Log4j等



