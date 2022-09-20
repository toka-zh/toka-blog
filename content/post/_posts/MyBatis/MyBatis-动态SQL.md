---
draft: true # 是否为草稿
title: 动态SQL   #文章名
date: 2019‎-12‎-29‎ 20:49:52
status: public          #公开
tags: MyBatis      #tag
categories: SSM           #文章分类
---

# 动态SQL

环境搭建

```sql
create  table blog(
    id varchar(50) not null comment '博客id',
    title varchar(100) not null comment '博客标题',
    auther varchar(30) not null comment '博客作者',
    create_time datetime not null comment '创建时间',
    views int(30) not null comment '浏览量'
)
```



插件一个基础工程

1. 导包
2. 编写配置文件
3. 编写实体类
4. 编写实体类对应的Mapper和映射文件

## trim（where，set）

### where

如果标签内条件都不成立，自动舍弃where，若语句开头为“AND”或“OR”，where元素会将他们去除

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <if test="title!=null">
            and title = #{title}
        </if>
    </where>
</select>
```

### set 

动态前置SET关键字，同时删除无关逗号

```xml
<update id="updateBlogSet" parameterType="map">
    update mybatis.blog
    <set>
        <if test="title!=null">
            title = #{title},
        </if>

    </set>
    where id = #{id}
</update>
```

### trim  prefix

判定成功加入语句     去除语句前多余的prefixOverrides，去除语句后面多余的suffixOverrides

```xml
<trim prefix="WHERE" prefixOverrides="AND | OR">
    
</trim>
```



```xml
<trim prefix="SET" suffixOverrides=",">
    
</trim>
```





## IF

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <if test="title!=null">
            and title = #{title}
        </if>
    </where>
</select>
```



## choose（when，otherwise）

choose when otherwise类比switch case otherwise

不想写



## foreach

collection：指定一个集合

open separator close：定义开头 分割 结尾 字符

```xml
<select id="queryBlogForeach" parameterType="map" resultType="blog">
        select * from mybatis.blog
        <where>
            <foreach collection="ids" item="id" open="and (" close=")" separator=" or ">
                id=#{id}
            </foreach>
        </where>
    </select>
```



```java
@org.junit.Test
public void queruBlogForeach(){
    SqlSession sqlsession = MybatisUtils.getSqlSession();
    BlogMapper mapper = sqlsession.getMapper(BlogMapper.class);
    Map map = new HashMap<>();
    List<Integer> ids = new ArrayList<>();
    ids.add(1);

    map.put("ids",ids);
    List<Blog> blogs = mapper.queryBlogForeach(map);
    for (Blog blog : blogs) {
        System.out.println(blog);
    }
}
```



## SQL片段

将相同的语句抽取为一个SQL片段（sql标签），在使用时用include标签的refid引用

注意事项：

* 最好基于单表定义
* 片段内不要加入where标签

