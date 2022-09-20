---
draft: true # 是否为草稿
title: MyBatis之分页处理   #文章名
date: 2019‎-12‎-28 ‏‎20:05:10
status: public          #公开
tags: MyBatis      #tag
categories: SSM           #文章分类
---

### 分页

* 减少数据处理量

### 使用limit分页 

```sql
#SELECT *from user limit starIndex,pageSize
SELECT *from user limit 3;
```

### 使用MyBatis分页

1. 接口

   ```java
   List<User> getUserByLimit(Map<String,Integer> map);
   ```

2. Mapper.xml

   ```xml
   <select id="getUserByLimit" parameterType="map" resultType="User">
       select * from mybatis.user limit #{startIndex},#{pageSize}
   </select>
   ```

3. 测试

   ```java
   @Test
   public void getUserByLimit(){
       SqlSession sqlSession = MybatisUtils.getSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       HashMap<String, Integer> map = new HashMap<>();
       map.put("startIndex",0);
       map.put("pageSize",2);
   
       List<User> list = mapper.getUserByLimit(map);
       for (User user : list) {
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```