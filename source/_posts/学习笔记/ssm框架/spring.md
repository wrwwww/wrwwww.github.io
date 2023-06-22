---
title: spring
date: 2023-05-05 21:30:42
tags:  
---
# spring框架

### 概述

Spring的英文翻译为春天，可以说是给Java程序员带来了春天，因为它极大的简化了开发。我得出一个公式：Spring = 春天 = Java程序员的春天 = 简化开发。最后的简化开发正是Spring框架带来的最大好处。

Spring是一个开放源代码的设计层面框架，它是于2003 年兴起的一个轻量级的Java 开发框架。由Rod Johnson创建，其前身为Interface21框架，后改为了Spring并且正式发布。

### 核心

Spring 的理念：不去重新发明轮子。其核心是控制反转（IOC）和面向切面（AOP）

### spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>
```

通过set方式注入

```xml
<!-- 通过空的构造方法创建类，然后通过set方法的方式注入-->
<bean id="add" class="com.manager.pojo.Address">
    <!--set注入 -->
      <property name="address" value="北京"/>
	<!--构造方法注入-->
    <constructor-arg index="0" value="太原"/>

</bean>
```

##### 复杂类型的注入

```xml
  <bean id="s1" class="com.manager.pojo.Student">
<!--        普通类型-->
        <property name="id" value="01"/>
<!--        对于引用类型-->
        <property name="address" ref="add"/>
        <property name="name" value="李沧海"/>
<!--        list-->
        <property name="hobby">
            <list>
                <value>看书</value>
            </list>
        </property>
<!--map-->
        <property name="book">
            <map>
                <entry value="且听风吟" key="村上春树"/>
            </map>
        </property>
<!--数组-->
        <property name="score">
            <array>
                <value>12</value>
            </array>
        </property>
     <!--空值--> 
<property name="email" value=""/>
       <null/>
    </bean>
```

p name space 以使用另外的set注入标签

```xml
xmlns:p="http://www.springframework.org/schema/p"
<!-- 普通的set注入-->
<bean name="classic" class="com.example.ExampleBean">
        <property name="email" value="someone@somewhere.com"/>
    </bean>
<!-- p- name space set注入-->
    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="someone@somewhere.com"/>
```

c name space 

```xml
```

#### scope 作用域

```xml
<!--
    scope 作用域
    Singletons 单例模式 从容器中得到的对象是同一个对象
    prototype  原型模式 从容器中得到的对象是不同的对象
-->

@Scope("prototype")
<bean id="s1" class="com.manager.pojo.Student" scope="prototype">
```

#### 自动装箱

- 使用@Autowired关键字 spring会自动注入依赖类型

![image-20220515194628908](C:\Users\wrw\AppData\Roaming\Typora\typora-user-images\image-20220515194628908.png)

```xml
<!--    这是手动装箱-->
    <bean id="people" class="com.manager.pojo.People">
        <property name="name" value="风吹麦浪"/>
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
    </bean>
<!--    自动装箱-->
    <bean id="people" class="com.manager.pojo.People" autowire="byName">
        <property name="name" value="风吹麦浪"/>
    </bean>
```

- 通过id名称匹配，通过类型匹配

```xml
autowire="byName"
autowire="byType"
```

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

可以为属性为没有配置的bean为null

```java
@Autowired(required = false)

@Autowired
public void setMovieFinder(@Nullable MovieFinder movieFinder) {
        ...
}
```

##### @Qualifier(value = "dog12")

##### 配合@autowired实现按名称匹配

```xml
<bean id="dog2" class="com.manager.pojo.Dog">
        <qualifier value="12"/>
</bean>
<bean id="people" class="com.manager.pojo.People" autowire="byName">
    <!-- a-->
    <property name="name" value="风吹麦浪"/>
</bean>
```

### 使用java配置工具类进行配置

创建配置工具类Myconfig使用@Configuration注解标记工具类

```java
@Configuration
@Import(MyConfig2.class)  //导入合并其他配置类，类似于配置文件中的 inculde 标签
public class MyUtils1 {

    @Bean
    public User user(){
        return new User();
    }

}
```

```java
ApplicationContext context = new AnnotationConfigApplicationContext(MyUtils1.class);
User user = context.getBean("user", User.class);
System.out.println(user.name);
```

### spring整合mybatis

1. ##### 之前的mybatis使用

   maven导入mybatis依赖

   创建mybatis的xml配置

   idea连接mysql

   创建实体类

   写操作数据库持久层接口,写接口同名的映射xml文件,在xml文件中写sql

   将xml文件在mybatis中注册绑定

   写mybatis工具类,通过SqlSessionFactoryBuilder().build(inputStream);创建sessionFastory工厂,通过工厂便可以生产sqlsession

   ```java
   SqlSessionFactory sqlSessionFactory;
   String url="mybatis-config.xml";
   InputStream inputStream = Resources.getResourceAsStream(url);
   sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
   ```

   sqlsession便可以通过getMapper(AccountMapper.class)方法得到mapper去执行sql

   2. #### 整合后创建使用mybatis

      一种通过spring配置文件创建mybatis的方法

      导入spring-mybatis的依赖

      在bean上下文中配置mybatis的bean

      1. 这是数据源,即数据库的基本信息,驱动密码

      ```xml
        <!--配置数据源：数据源有非常多，可以使用第三方的，也可使使用Spring的-->
          <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
              <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
              <property name="url" value="jdbc:mysql://localhost:3306/shop?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
              <property name="username" value="root"/>
              <property name="password" value="123"/>
          </bean>
      ```

      2. 这是sqlsessionfactorybean类似mybatis中的sqlsessionfactory用来创建sqlsession,它的参数分别是数据源,mybatis的配置,mapper文件

      ```xml
      <!--配置SqlSessionFactory-->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <property name="dataSource" ref="dataSource"/>
          <!--关联Mybatis-->
          <property name="configLocation" value="classpath:mybatis-config.xml"/>
          <property name="mapperLocations" value="classpath*:com/manager/**/*.xml" />
      </bean>
      ```

   3. 使用线程安全的SqlSessionTemplate的类创建session,他的构建器需要sqlsessionfactory来创建

      ```xml
      <!--注册sqlSessionTemplate , 关联sqlSessionFactory-->
      <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
          <!--利用构造器注入-->
          <constructor-arg index="0" ref="sqlSessionFactory"/>
      </bean>
      ```

这样通过spring就把mybatis配置写好了,接下来需要实体类,需要dao接口,接口映射xml文件和接口实现类,在接口实现类中绑定一个sqlsession来调用接口方法实现,通过容器创建

```xml
   <bean id="accountDao" class="com.manager.dao.AccountMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
```

