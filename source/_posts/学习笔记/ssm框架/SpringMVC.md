### SpringMVC

#### servlet

需要写servlet类来继承HttpServle并且重写doget和dopust方法,在servlet 中对业务层进行调用,最后重定向或者转发

```java
public class helloService extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
System.out.println(req.getContextPath());
resp.sendRedirect(req.getContextPath()+"/templates/home.html");//重定向
//        resp.sendRedirect(req.getContextPath()+"/jsp/hello.jsp");//重定向
}
```

然后再web.xml中注册servlet

```xml
 <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>helloService</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
```

跳转到指定路径先经过web.xml,在其中找到地址所对应的servlet然后找到所对应的servlet,servlet对网页进行重定向或者转发

弊端:当servlet增加的越来越多,去添加会很麻烦,也不直观

#### SpringMVC

![这里写图片描述](http://static.oschina.net/uploads/img/201601/26185156_co9w.jpg)

- M：(Model)  模型  :  应用程序的核心功能，管理这个模块中用的数据和值；

- V(View )视图:  视图提供模型的展示，管理模型如何显示给用户，它是应用程序的外观；

- C(Controller)控制器: 对用户的输入做出反应，管理用户和视图的交互，是连接模型和视图的枢纽。

MVC用于将web（UI）层进行职责解耦

##### 使用xml配置的mvc

首先创建普通的maven项目添加web.xml的框架支持,

![image-20220520200110071](C:\Users\wrw\AppData\Roaming\Typora\typora-user-images\image-20220520200110071.png)

创建目录lib添加jar到其中.在web.xml中注册DispatcherServlet的servlet并且配置参数和启动级别,它是spring提供给我们的.

```xml
<!--1.注册DispatcherServlet-->
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc-servlet.xml</param-value>
    </init-param>
    <!--启动级别-1-->
    <load-on-startup>1</load-on-startup>
</servlet>
            
    <!--/ 匹配所有的请求；（不包括.jsp）-->
    <!--/* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>            
```

创建名为springmvc-servlet.xml的springmvc的上下文注册

SimpleControllerHandlerAdapter控制层适配器

BeanNameUrlHandlerMapping处理servlet映射

InternalResourceViewResolver视图解析器处理dipatcherservlet传递过来的modelandview

```xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

<!--视图解析器:DispatcherServlet给他的ModelAndView-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
    <!--前缀-->
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!--后缀-->
    <property name="suffix" value=".jsp"/>
</bean>
```

###### 注册控制层bean

```xml
<bean id="/hello" class="com.manager.controller.helloController"/>
```

地址栏输入/hello然后找到控制层

```java
public class helloController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中。Model
        mv.addObject("msg","HelloSpringMVC!");
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
        return mv;
    }
}
```

经过控制层处理返回要跳转的视图

![img](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7KwPOPWq00pMJiaK86lF6BjIXW7Wmm9KVEV1FXUfJMD0KzuYZ7ic5UHggsZDAzyYyrd4pLvnBIVM5zA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![img](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7KwPOPWq00pMJiaK86lF6BjIWe8RPcCUeexojBiaPtY7HibQonS3PdCy98oV24F0tYk8IxEUY43N93TA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

SpringMVC的原理如下图所示：

当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者。![img](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7KwPOPWq00pMJiaK86lF6BjIaosVziclWLEJQkzobxHrpHcmtu2yTeVWPmEI4Yq5PaicS52VaJt8dYfQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### SpringMVC执行原理

![img](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7IicxBZbkh0D4dJJiaXSzGEXyzsXDPy7oAJFsBvvBibiaFWpSp75vFIEOCBm7wnt4JKXJCHB9MflUycKw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**简要分析执行流程**

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。

   我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

   **如上url拆分成三部分：**

   [http://localhost:8080](http://localhost:8080/)服务器域名

   SpringMVC部署在服务器上的web站点

   hello表示控制器在springmvc-servlet中注册

   ```xml
   <!--Handler-->
   <bean id="/hello" class="com.manager.controller.helloController"/>
   ```

   通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。

   ```xml
   <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   <!-- 根据url查找处理器,比如从上面的连接中查找到需要的控制器hello-->
   ```

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

6. Handler让具体的Controller执行。

   ```java
   //实现接口controller处理业务及页面跳转重定向
   public class helloController implements Controller {
       @Override
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           ModelAndView mv = new ModelAndView();
   
           //封装对象，放在ModelAndView中。Model
           mv.addObject("msg","HelloSpringMVC!");
           //封装要跳转的视图，放在ModelAndView中
           mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
           return mv;
       }
   }
   ```

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

   ```xml
       <!--视图解析器:DispatcherServlet给他的ModelAndView-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!--前缀-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--后缀-->
           <property name="suffix" value=".jsp"/>
       </bean>
   ```

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12. 最终视图呈现给用户。

##### 使用annotation减少配置提高效率

web.xml的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
        version="4.0">
   <!--1.注册servlet-->
   <servlet>
       <servlet-name>SpringMVC</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:springmvc-servlet.xml</param-value>
       </init-param>
       <!-- 启动顺序，数字越小，启动越早 -->
       <load-on-startup>1</load-on-startup>
   </servlet>
   <!--所有请求都会被springmvc拦截 -->
   <servlet-mapping>
       <servlet-name>SpringMVC</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>
</web-app>
```

1. 1. #### springmvc-servlet.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
   <context:component-scan base-package="com.manager.controller"/>
   <!-- 让Spring MVC不处理静态资源 -->
   <mvc:default-servlet-handler />
   <!--
   支持mvc注解驱动
       在spring中一般采用@RequestMapping注解来完成映射关系
       要想使@RequestMapping注解生效
       必须向上下文中注册DefaultAnnotationHandlerMapping
       和一个AnnotationMethodHandlerAdapter实例
       这两个实例分别在类级别和方法级别处理。
       而annotation-driven配置帮助我们自动完成上述两个实例的注入。
    -->
   <mvc:annotation-driven />
   <!-- 视图解析器 -->
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
         id="internalResourceViewResolver">
       <!-- 前缀 -->
       <property name="prefix" value="/WEB-INF/jsp/" />
       <!-- 后缀 -->
       <property name="suffix" value=".jsp" />
   </bean>
</beans>
```

##### 编写一个Java控制类：

###### 简单的

```java
@Controller
public class helloController {
    @RequestMapping("/hello")
    public String helloAnnotation(){
        return "h1";
    }
}
```

###### 复制一点的

```java
@Controller
@RequestMapping("/HelloController")
public class HelloController {

   //真实访问地址 : 项目名/HelloController/hello
   @RequestMapping("/hello")
    //这个hello为url中的地址,为控制器
   public String sayHello(Model model){
       //向模型中添加属性msg与值，可以在JSP页面中取出并渲染
       model.addAttribute("msg","hello,SpringMVC");
       //web-inf/jsp/hello.jsp
       return "hello";
       //返回的这个字符串为要跳转的视图
  }
}
```

在地址栏后输入/hello或者其他网页跳转到hello就会通过控制器进行一系列操作

@RequestMapping是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该是/HelloController/hello；
方法中声明Model类型的参数是为了把Action中的数据带到视图中；
方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/hello.jsp。

#### 使用springMVC必须配置的三大件：

处理器映射器、处理器适配器、视图解析器
通常，我们只需要手动配置视图解析器，而处理器映射器和处理器适配器只需要开启注解驱动即可，而省去了大段的xml配置

#### 前后端数据的处理

 http://localhost:8080/hello?name=hello1

从链接中得到参数

```java
@RequestMapping("/hello")
public String hello(String name){
   System.out.println(name);
   return "hello";
}
```

输出hello1

http://localhost:8080/hello?username=hello1

如果参数名与链接中的参数名不同

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
   System.out.println(name);
   return "hello";
}
```

输出hello1

 http://localhost:8080/mvc04/user?name=hello&id=1&age=15

提交的额为对象时候名字需要和类的属性名称相同

```java
@RequestMapping("/user")
public String user(User user){
   System.out.println(user);
   return "hello";
}
```

#### 通过ModelAndView

```java
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```

#### 通过ModelMap

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, ModelMap model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("name",name);
   System.out.println(name);
   return "hello";
}
```

通过Model

```java
@RequestMapping("/ct2/hello")
public String hello(@RequestParam("username") String name, Model model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("msg",name);
   System.out.println(name);
   return "test";
}
```

#### 解决网页乱码问题

在javaweb的时候通过添加一层过滤器来给每个请求响应设置统一编码

在SpringMVC中提供了一个过滤器

```xml
<filter>
   <filter-name>encoding</filter-name>
   <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
   <init-param>
       <param-name>encoding</param-name>
       <param-value>utf-8</param-value>
   </init-param>
</filter>
<filter-mapping>
   <filter-name>encoding</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```

有时候可能支持不是那么好

修改tamcat的配置

```xml
<Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
          connectionTimeout="20000"
          redirectPort="8443" />
```

也可以添加自己的过滤器

和javaweb的时候一样添加过滤器类实现filter的接口实现方法doFilter

```java
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=UTF-8");
        chain.doFilter(request, response);
    }
```

最后在web.xml中配置

```xml

 <filter>
        <filter-name>GenericEncodingFilter</filter-name>
        <filter-class>com.manager.filter.GenericEncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>GenericEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

