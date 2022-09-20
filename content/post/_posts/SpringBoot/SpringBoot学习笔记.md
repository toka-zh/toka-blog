---
draft: true # 是否为草稿
title: SpringBoot学习笔记   #文章名
date: 2019‎-12‎-30 ‏‎15:32:21
status: public          #公开
tags: SpringBoot      #tag
categlories: SpringBoot           #文章分类
---

# 1. Spring

-----

## 1.1 简介

* Spring:：春天——给软件行业带来了春天
* 2002年，首次退出了Spring框架的雏形：interface21框架
* Spring框架以interface21框架为基础，结果程序设计并不断丰富其内涵，于2004年3月24日发布了1.0正式版。
* Rod Johnson：Spring Framework创始人，音乐学博士
* 理念：使现有的技术更加容易使用，本身是个大杂烩，整合了现有的技术框架
* SSH：Struct2+Spring+Hibernate
* SSM：SpringMVC+Spring+Mybatis

官网 https://spring.io/projects/spring-framework

官方下载地址 https://repo.spring.io/release/org/springframework/spring/

GitHub地址 https://github.com/spring-projects/spring-framework

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.2.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.2.RELEASE</version>
</dependency>

```

## 1.2 优点

* Spring是一个开源的免费的框架（容器）
* Spring是一个**轻量级，非入侵式**的框架
* **控制反转（IOC），面向切面编程（AOP）**
* 支持事务的处理，对框架整合的支持

> Spring就是轻量级的控制反转（IOC）和面向切面编程（AOP）的框架！

## 1.3 组成

7大模块

## 1.4 拓展

构建一切——协调一切——连接一切

现代化开发就是基于Spring的开发

Spring Boot

* 一个快速开发的脚手架
* 基于SpringBoot可以快速开发单个微服务
* **约定大于配置**

Spring Cloud

* SpringCloud是基于SpringBoot实现的

大多数公司都在使用SpringBoot开发，学习SpringBoot的前提，需要五千掌握Spring及SpringMVC！ **承上启下**的作用！



弊端：发展了太久，违背了原来的理念。配置十分繁琐，人称“配置地狱”

# 2.IOC本质

-----

控制反转是一种通过描述（XML或注解）斌通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IOC容器，实现方法是依赖注入（Dependency Injection，DI）



# 3.HelloSpring

# 4.IOC创建对象的方式

1. 默认使用无参构造创建对象

2. 假设要用有参构造参数对象

   ```java
   public User(String name) {
   }
   ```

    * 参数下标

      ```xml
      <bean id="user" class="com.spring.pojo.User">
          <constructor-arg index="0" value=" "/>
      </bean>
      ```

    * 参数类型

      ```xml
      <bean id="user" class="com.spring.pojo.User">
          <constructor-arg type="java.lang.String" value=" "/>
      </bean>
      ```

    * 参数名

      ```xml
      <bean id="user" class="com.spring.pojo.User">
          <property name="name" value=" "/>
      </bean>
      ```

# 5.Spring配置

-----

## 5.1 别名

```xml
<alias name="user" alias="useruser"/>
```

## 5.2 Bean的配置

id：bean的唯一标识符，相当于对象名

class：bean对象所对应的全限定名

name：也是别名，而且name可以同时去多个别名，通过逗号（,）空格（ ）分号（;）来分隔

```xml
<bean id="user" class="com.spring.pojo.User" name="user2,u2 u3;u4">
    <property name="name" value="hhh我擦"/>
</bean>
```

## 5.3 import

它可以将多个配置文件导入总配置，从而合并为一个，使用时直接使用总配置 applicationContext.xml 即可

通常用于团队开发



```xml
<import resourse="bean1.xml">
<import resourse="bean2.xml">
```

# 6.依赖注入

## 6.1 构造器注入

## 6.2 Set注入

* 依赖注入：Set注入
  * 依赖：bean对象的创建依赖于容器
  * 注入：bean对象中的所有属性，由容器来注入

【环境搭建】

1. 复杂类型

   ```java
   public class Address {
       private String address;
   }
   ```

2. 真实测试对象

   ```java
   public class Student {
       private String name;
       private Address address;
       private String[] books;
       private List<String> hobbys;
       private Map<String,String> card;
       private Set<String> games;
       private Properties info;
       private String wife;
   }
   ```

3. application.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="student" class="com.spring.pojo.Student">
           <property name="name" value="Its name"/>
       </bean>
   </beans>
   ```

完善注入信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <bean id="address" class="com.spring.pojo.Address">
        <property name="address" value="福建"/>
    </bean>
    <bean id="student" class="com.spring.pojo.Student">
        <property name="name" value="Its name"/>
        <property name="address" ref="address"/>
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>
        <property name="hobbys">
            <list>
                <value>唱</value>
                <value>跳</value>
                <value>Rap</value>
                <value>篮球</value>
            </list>
        </property>
        <property name="card">
            <map>
                <entry key="身份证" value="444555677777534456"/>
                <entry key="学生证" value="123123123"/>
            </map>
        </property>
        <property name="games">
            <set>
                <value>LOL</value>
                <value>WOW</value>
            </set>
        </property>
        <property name="wife">
            <null/>
        </property>

        <property name="info">
            <props>
                <prop key="学号">123123123</prop>
                <prop key="性别">男</prop>
                <prop key="driver"></prop>
                <prop key="url"></prop>
            </props>
        </property>
    </bean>

</beans>
```

## 6.3 拓展方式注入

可以使用p命名课件和c命名空间进行注入



p命名声明 xml约束

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

c命名 xml约束

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

注意点：p命名和c命名使用需要导入xml约束

使用：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:C="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--P命名空间注入，可以直接注入属性的值-->
    <bean id="user" class="com.spring.pojo.User" p:name="111" p:age="18"/>
    <bean id="user2" class="com.spring.pojo.User" C:age="18" C:name="蜥蜴"/>
</beans>
```

# 6.4 JavaBean的作用域

1. 单例模式（Spring默认机制）：只会iui商城一个对象

   ```xml
   <bean id="user2" class="con.spring.pojo.User" c:age="18" c:name="小子" scope="singleton"/>
   ```

   

2. 原型模式（prototype）：每次从容器中get的时候都会产生一个新对象

   ```xml
   <bean id="user2" class="con.spring.pojo.User" scope="prototype"/>
   ```

3. 其余的request、session、application这些只能在web开发中使用到



# 7.Bean的自动装配

* 自动装配是Spring满足bean依赖的一周方式
* Spring会在上下文中自动寻找，并再主动给bean装配属性

在Spring中由三种装配的方式

1. 在xml中显示的配置
2. 在java中显示配置
3. **隐式的自动装配bean**

## 7.1 byName

byName：会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid

```xml
<bean id="cat" class="com.spring.pojo.Cat"/>
<bean id="dog" class="com.spring.pojo.Dog"/>
<bean id="people" class="com.spring.pojo.People" autowire="byName">
    <property name="name" value="路过的人"/>
</bean>
```

## 7.2 byType

byType：会自动在容器上下文中查找，和自己对象属性类型系统的bean

```xml
<bean class="com.spring.pojo.Cat"/>
<bean class="com.spring.pojo.Dog"/>
<bean id="people" class="com.spring.pojo.People" autowire="byType">
    <property name="name" value="路过的人"/>
</bean>
```

小结：

* byName时，要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
* byName时，要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致

## 7.3使用注解实现自动装配

（@autowired）

使用注解须知：

1. 导入约束 context约束

2. 配置注解的支持

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:annotation-config/>
   
   </beans>
   ```

**@Autowaired**

直接在属性上使用，也可以在set方法上使用

使用Autowired，可以不用编写set方法，前提是这个自动装配的属性在IOC（Spring）容器中存在，且符合名字byname



@Nullable

@Autowired(required = false)

如果显示定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空

如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解完成时，我们可以使用@Qualifier(value="xxx")去配置@Autowired的使用，指定一个唯一的bean对象注入

```java
public class People {
    private String name;
    @Autowired
    @Qualifier(value="dog22")
    private Cat cat;
    @Autowired
    private Dog dog;
}/**省略了Get方法**/
```



@Resource注解

```java
public class People {
    private String name;
    @Resource(name="cat2")
    private Cat cat;
    @Resource
    private Dog dog;
}/**省略了Get方法**/
```

小结：

@resource和@Autowired的区别：

* 都是用来自动装配的，都可以放在属性字段上
* @Autowired通过byType的方式实现，而且必须要求这个对象存在
* @Resource默认通过byName的方式实现，如果找不到名字，则通过NbyType实现，如果两个都找不到的情况下，就报错
* 执行顺序不同：@Autowired通过byType的方式实现，@Resource默认通过byName的方式实现

# 8.使用注解开发

在Spring4后，要使用在注解开发，必须要导入aop依赖，使用注解需要导入context约束，增加注解的支持

1. bean

2. 属性如何注入

   ```java
   package com.spring.pojo;
   
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.stereotype.Component;
   
   //@Component
   @Component
   public class User {
       //相当于<property name="name" value="嚣张的张"/>
       @Value("嚣张的张")
       public String name;
   }
   ```

3. 衍生的注解

   @Component有几个衍生注解，我们在Web开发中，会按照mvc三层架构分层

   * dao  【@Repository】

   * service 【@Service】

   * controller 【@Controller】

     四个注解都是一样的，都是代表将某个类注册到Spring中，转载Bean

4. 自动装配置

   ```java
   - @Autowired：自动装配通过类型
   - @Nullable： 字段标记了这个注解，说明这个字段可以为NULL
   - @Resource: 自动装配通过名字，类型
   ```

5. 作用域

   单例@Scope("singleton") prototype

6. 小结

   xml 与注解

   * xml 更加万能，适用于任何场合，维护简单方便

   * 注解 不是自己的类使用不了，维护相对复杂

     最佳时间

     * xml用来管理bean

     * 注解只负责完成属性注入

     * 要使注解生效，就必须开始注解支持

       ```xml
       <context:annotation-config/>
       ```

# 9 使用Java的方式配置Spring

我们现在要完全不适用Spring的XML配置，全权交给Java来做。

JavaConfig是Spring的一个子项目，都是在Spring4之后成为了核心功能

实体类

```java
package com.spring.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    @Value("xiaozhang")
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

配置文件

```java
package com.spring.config;

import com.spring.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

//这个也会被Spring托管。注册到容器中，因为他本来就是一个@Component
//@Configuration代表这是一个配置类，等同于beans.xml
@Configuration
@ComponentScan("com.spring.pojo")
public class SpringConfig {

    /*
    注册一个bean，相当于xml中的bean标签
    方法的名字相当于bean标签的id属性
    返回值相当于bean标签中的class属性
     */
    @Bean
    public User getUser(){
        return new User();//返回要注入到bean的对象
    }
}
```



测试类

```java
import com.spring.config.SpringConfig;
import com.spring.pojo.User;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        //如果完全使用了配置类方式去做，我们就只能通过AnnotationConfig 上下文来获取容器，通过配置类的class对象加载
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        //使用的时候取用方法名
        User user = context.getBean("getUser", User.class);
        System.out.println(user);
    }
}
```

这种纯Java的配置方式，在SpringBoot中随处可见



# 10 代理模式

## 10.1 静态代理

角色分析：

* 抽象角色：一般使用接口或者抽象类来解决
* 真实角色：被代理的角色
* 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
* 客户：访问代理对象的人



代码步骤

1. 接口

   ```java
   public interface Rent {
       void rent();
   }
   ```

2. 真实角色

   ```java
   public class Landlord {
       public void landlord(){
           System.out.println("房东要出租房子");
       }
   }
   ```

3. 代理角色

   ```java
   public class Proxy implements Rent{
       private Landlord landlord;
   
       public Proxy(Landlord landlord) {
           this.landlord=landlord;
       }
   
       public void fare(){
           System.out.println("收中介费");
       }
       public void seeHouse(){
           System.out.println("看房");
       }
       public void Sign(){
           System.out.println("签合同");
       }
       @Override
       public String toString() {
           return "Proxy{" +
                   "landlord=" + landlord +
                   '}';
       }
   
       public Landlord getLandlord() {
           return landlord;
       }
   
       public void setLandlord(Landlord landlord) {
           this.landlord = landlord;
       }
   
       public  void rent(){
           landlord.landlord();
           seeHouse();
           Sign();
           fare();
       }
   }
   ```

4. 客户端访问代理角色

   ```java
   public class Client {
       public static void main(String[] args) {
   //        Landlord landlord = new Landlord();
   ////        landlord.landlord();
   
           Landlord landlord = new Landlord();
           Proxy proxy = new Proxy(landlord);
           proxy.rent();
       }
   }
   ```

代理模式的好处

* 可以使真实角色的操作更加纯粹，不用去关注一些公共业务
* 公共业务交给代理角色，实现了业务的分工
* 公共业务发生扩展时，方便集中管理

缺点

* 一个真实角色产生一个代理角色，代码量翻倍，开发效率会变低

## 10.2 动态代理

* 动态代理和静态代理角色一样
* 动态代理的代理类是动态生成的，不是我们直接写好的
* 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  * 基于接口 JDK动态代理
  * 基于类 cglib
  * java字节码实现 Javassist



需要了解两个类：InvocationHandler，Proxy



优点：

* 一个动态代理的是一个接口，一般对应一个类
* 一个动态代理类可以代理多个**实现同一个接口**的类



# 11 AOP

AOP（Aspect Oriented Programming）面向切面编程：通过预编译方式和运行期动态代理实现程序功能统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生泛型。利用AOP可以对业务罗技的各部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可复用性，同时提高开发效率。



Maven依赖

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.5</version>
    <scope>runtime</scope>
</dependency>

```



方式一：使用Spring AOI的接口



方式二：使用自定义类来实现AOP



方式三：使用注解实现

@Aspect

