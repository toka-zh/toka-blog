---
draft: true # 是否为草稿
title: 我的第一个MyBatis程序   #文章名
date: 2019‎-12‎-27‎ ‏‎16:15:51
status: public          #公开
tags: MyBatis      #tag
categories: SSM           #文章分类
---

# 我的第一个MyBatis程序

-----

* [MyBatis仓库地址](https://github.com/mybatis/mybatis-3/releases)

* [官方中文文档](https://mybatis.org/mybatis-3/zh/index.html)

### 1 . 创建对应数据库

| 名   | 类型    | 长度 | 是否空 | 主键 |
| ---- | ------- | ---- | ------ | ---- |
| id   | int     | 20   | 否     | 1    |
| name | varchar | 30   | 是     |      |
| pwd  | vachar  | 30   | 是     |      |

 ### 2 . 新建maven项目

删除src，使其成为一个父工程

导入相关的maven依赖

```xml
    <dependencies>

        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.3</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.18</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

    </dependencies>
```

在父工程中创建一个子Maven模块,接下来的操作都在子Maven模块中完成。

### 3. 编写核心配置文件

在resource中新建配置文件mybatis-config.xml，内容如下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
</configuration>
```

填入数据库配置

```xml
<property name="driver" value="com.mysql.cj.jdbc.Driver"/>
<property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true"/>
<property name="username" value="root"/>
<property name="password" value="123456"/>
```

### 4.编写mybatis工具类

```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//sqlSessionFactory --> sqlSession
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        //使用Mybatis获取sqlSessionFactory对象
        InputStream inputStream = null;
        try {
            String resource = "mybatis-config.xml";
            inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}

```

### 5. 封装实体类

```java
package com.rn.pojo;

public class User {
    private int id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }


    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}

```

编写Mapper接口

```java
public interface UserMapper {
    List<User> getUserList();
}
```

绑定接口（mapper配置文件）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--绑定一个对应的Mapper接口-->
<mapper namespace="com.rn.dao.UserMapper">
    <select id="getUserList" resultType="com.rn.pojo.User">
        select *from mybatis.user;
    </select>
</mapper>
```

### 6 junit测试，处理错误

在test目录下新建与main系统的目录

编写测试代码

```java
package com.rn.dao;

import com.rn.pojo.User;
import com.rn.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class UserMapperTest {
    @Test

    public void test(){

        //获得sqlsession对象
        SqlSession sqlSession =  MybatisUtils.getSqlSession();
        //执行
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.getUserList();

        for(User user:userList){
            System.out.println(user);
        }

        sqlSession.close();
    }
}

```



运行得到结果报错:无法找到xml配置文件

```java
java.io.IOException: Could not find resource org/mybatis/example/mybatis-config.xml
```



经过检查原因为MybatisUtils工具类中的插件SqlSessionFactoryBuilder的resource中文件目录未更改：

```java
String resource = "org/mybatis/example/mybatis-config.xml";
```



因为mybatis-config.xml文件放在了resource目录下，其实只要写文件名就可以了（resource文件可以算是根目录）

```java
String resource = "mybatis-config.xml";
```



再次编译运行后，依然报错

```java
org.apache.ibatis.binding.BindingException: Type interface com.rn.dao.UserMapper is not known to the MapperRegistry.
```

​		***MapperRegistry：核心配置文件中注册mapper***

报错原因：核心配置文件中没有注册mapper

编译运行 。。。继续报错。。。

```java
### Error building SqlSession.
### The error may exist in com.rn.dao.UserMapper
### Cause: org.apache.ibatis.builder.BuilderException: Error parsing SQL Mapper Configuration. 
```

错误提示找不到xml文件，但是配置文件其实是写了的

由于maven的约定大于配置，我们写的配置文件无法被导出或者生效

所以需要***手动配置资源过滤*** 

解决方法 在poe.xml中添加资源过滤，代码如下

```xml
<build>
	 <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
</build>
```



结果运行又又又报同样的错，找了半天才发现原来是mybatis-config.xml的Mapper的recource写错了

```xml
<mapper resource="com.rn.dao.UserMapper"/>
```



正确写法如下：

```xml
<mapper resource="com/rn/dao/UserMapper.xml"/>
```

接着再再再次运行

![image-20191227153227844](C:\Users\Rorschach\AppData\Roaming\Typora\typora-user-images\image-20191227153227844.png)

终于运行成功了。。。。成功打印数据库数据



