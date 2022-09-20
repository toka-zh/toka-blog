---
draft: true # 是否为草稿
title: MyBatis-日志管理   #文章名
date: 2019‎-12‎-28 ‏‎19:00:00
status: public          #公开
tags: MyBatis      #tag
categories: SSM           #文章分类
---

### 标准日志

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

LOG4J

先导入log4j的包

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>

```



log4j.properties

```properties
### set log levels ###

log4j.rootLogger = debug,console,file

### 输出到控制台 ###

log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold = DEBUG ## 输出DEBUG级别以上的日志
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern =[%c]-%m%n

### 输出到日志文件 ###
log4j.appender.file = org.apache.log4j.DailyRollingFileAppender
log4j.appender.file.File = ./log/debug.log
log4j.appender.file.MaxFileSize=10mb ## 日志文件的最大大小
log4j.appender.file.Append = true
log4j.appender.file.Threshold = DEBUG ## 输出DEBUG级别以上的日志
log4j.appender.file.layout = org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern =[%p][d{yyyy-MM-dd HH:mm:ss}] [%c]-%m%n

### 日志输出级别 ###
log4j.logger.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

配置log4j为日志的实现

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

简单的使用

1.在要使用log4j的类中导入包

```java
import org.apache.log4j.Logger;
```

2.日志对象，参数为当前类的class

```java
static Logger logger = Logger.getLogger(UserMapper.class);
```

3.日志级别

```java
logger.info("info:进入了testLoh4j");
logger.debug("debug:进入了testLog4j");
logger.error("error:进入了testLog4j");
```

