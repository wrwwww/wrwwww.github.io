### MyBatis

###### 创建maven项目导入jar包

```xml
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.28</version>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.13.2</version>
          <scope>test</scope>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.9</version>
      </dependency>
```

#### 编写工具类MybatisUtils

通过该类的静态方法得到sqlsession工厂,通过工厂生产session，我们对数据库的操作通过改session来实现

```java
public class MyBatisUtils {
   static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    public static SqlSession getMybatisUtils() {
        return getMybatisUtils(true);
    }
    public static SqlSession getMybatisUtils(boolean bool) {
        return sqlSessionFactory.openSession(bool);
    }
}
```



###### 在idea中连接数据库

###### 创建封装连接数据库的工具类

```java
String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
//通过sqlsessionfactory连接到数据库
```



###### 创建mybatis-config.xml的配置文件,在其中配置数据库的基本信息,以及实体类xml的映射

```xml
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis_study?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/manager/dao/userMapper.xml"/>
    </mappers>
</configuration>
```

或者通过dp.properties来保存数据库的基本配置,不需要转义&字符

``` properties
username=root
password=123
url=jdbc:mysql://localhost:3306/shop?useSSL=true&useUnicode=true&characterEncoding=UTF-8
driver=com.mysql.jdbc.Driver
```

mybatis-config.xml

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="dp.properties"/>
    <settings>
        <!--        标准日志输出-->
        <!--        <setting name="logImpl" value="STDOUT_LOGGING"/>-->
        <!--        logj4-->
        <setting name="logImpl" value="LOG4J"/>
    </settings>


    <!--    这个标签可以把实体类的名称作为类型别名  方便在映射中使用-->
    <typeAliases>
        <package name="com.manager.pojo"/>
    </typeAliases>

    <!--环境标签-->
    <environments default="development">
        <!--可以配置多个环境-->
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>

        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--    映射持久层接口-->
    <mappers>
        <!--        <mapper resource="com/manager/dao/userMapper.xml"/>-->
        <package name="com.manager.dao"/>
    </mappers>
</configuration>
```





###### 创建实体类(pojo)



###### 创建dao层

dao层的作用为访问数据库，向数据库发送[sql语句](https://so.csdn.net/so/search?q=sql语句&spm=1001.2101.3001.7020)，完成数据的增删改查任务,为上面的实体类创建dao层接口,和映射xml文件

```java
public interface userDao {
    List<user> getUserList();
}
```

创建该接口的xml的映射文件

之后的数据库查询语句写xml映射文件,不需要再去实现接口类

```xml
<mapper namespace="com.manager.dao.userDao">
    <select id="getUserList" resultType="com.manager.pojo.user">
            select * from mybatis_study.user;
    </select>
</mapper>
```

#### xml基本属性

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.manager.dao.userMapper">
</mapper>
```





##### 过滤资源

用来反行xml等资源

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



##### 测试

最后在测试类中执行sql语句

```java
public class userDaoText {
    @Test
    public void test() {
        SqlSession sqlSession = MyBatisUtils.getMybatisUtils();
        userDao mapper = sqlSession.getMapper(userDao.class);
        List<user> list = mapper.getUserList();
        list.forEach(x-> System.out.println(x.toString()));
        sqlSession.close();
    }
}
```



##### 关于dao接口的映射文件可以放在resource中的与接口同名的包中

### 结果集映射

> 对数据中的类型起别名来匹配实体类的属性

```xml
<select id="selectUsers" resultType="User">
  select
    user_id             as "id",
    user_name           as "userName",
    hashed_password     as "hashedPassword"
  from some_table
  where id = #{id}
</select>
```

> 使用map映射实体类中的属性,对与名称一样的属性可以省略

```xml
<!--结果集映射-->
<resultMap id="UserMap" type="User">
    <!--column数据库中的字段，property实体类中的属性-->
    <result column="id" property="id"></result>
    <result column="name" property="name"></result>
    <result column="pwd" property="password"></result>
</resultMap>

<select id="getUserList" resultMap="UserMap">
    select * from USER
</select>
```

```xml
<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```

> 复杂的映射

```xml
<!-- 非常复杂的结果映射 -->
<resultMap id="detailedBlogResultMap" type="Blog">
  <constructor>
      <!--constructor用于创建实例时注入构造器-->
    <idArg column="blog_id" javaType="int"/>
  </constructor>
    
  <result property="title" column="blog_title"/>
<!--    association – 一个复杂类型的关联；许多结果将包装成这种类型-->
<!--这里将association中的属性创建author的实例-->
    <--!id对一个实例中的属性进行标记,标记出作为 ID 的结果可以帮助提高整体性能 -->
    <association property="author" javaType="Author">
    <id property="id" column="author_id"/>
    <result property="username" column="author_username"/>
    <result property="password" column="author_password"/>
    <result property="email" column="author_email"/>
    <result property="bio" column="author_bio"/>
    <result property="favouriteSection" column="author_favourite_section"/>
  </association>
    
  <collection property="posts" ofType="Post">
    <id property="id" column="post_id"/>
    <result property="subject" column="post_subject"/>
    <association property="author" javaType="Author"/>
    <collection property="comments" ofType="Comment">
      <id property="id" column="comment_id"/>
    </collection>
    <collection property="tags" ofType="Tag" >
      <id property="id" column="tag_id"/>
    </collection>
    <discriminator javaType="int" column="draft">
      <case value="1" resultType="DraftPost"/>
    </discriminator>
  </collection>
</resultMap>
```

```html
结果映射（resultMap）
constructor - 用于在实例化类时，注入结果到构造方法中
idArg - ID 参数；标记出作为 ID 的结果可以帮助提高整体性能
arg - 将被注入到构造方法的一个普通结果
id – 一个 ID 结果；标记出作为 ID 的结果可以帮助提高整体性能
result – 注入到字段或 JavaBean 属性的普通结果
association – 一个复杂类型的关联；许多结果将包装成这种类型
嵌套结果映射 – 关联可以是 resultMap 元素，或是对其它结果映射的引用
collection – 一个复杂类型的集合
嵌套结果映射 – 集合可以是 resultMap 元素，或是对其它结果映射的引用
discriminator – 使用结果值来决定使用哪个 resultMap
case – 基于某些值的结果映射
嵌套结果映射 – case 也是一个结果映射，因此具有相同的结构和元素；或者引用其它的结果映射
```

#### log4j使用

1.在要使用Log4j的类中，导入包 import org.apache.log4j.Logger;

```xml
     <!-- https://mvnrepository.com/artifact/log4j/log4j -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
```



2.日志对象，参数为当前类的class对象

```java
Logger logger = Logger.getLogger(UserDaoTest.class);
```

3.日志级别

```java
logger.info("info: 测试log4j");
logger.debug("debug: 测试log4j");
logger.error("error:测试log4j");
```

log4j需要配置才可以使用

log4j.properties的配置

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/kuang.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

