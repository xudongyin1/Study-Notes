

# Spring MVC

Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。Spring MVC 是目前主流的实现 MVC 设计模式的企业级开发框架，Spring 框架的一个子模块，无需整合，开发起来更加便捷。

**WEB-INF 下的资源用户不能直接访问，只能通过 servlet 和 controller 访问。**

## 什么是 MVC 设计模式？

将应用程序分为 Controller、Model、View 三层，Controller 接收客户端请求，调用 Model 生成业务数据，传递给 View。

- MVC是模型(Model)、视图(View)、控制器(Controller)的简写，是一种软件设计规范。
- 是将业务逻辑、数据、显示分离的方法来组织代码。
- MVC主要作用是**降低了视图与业务逻辑间的双向偶合**。
- MVC不是一种设计模式，**MVC是一种架构模式**。当然不同的MVC存在差异。

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

![image-20200622115034173](https://gitee.com/xudongyin/img/raw/master/img/image-20200622115034173.png)

## 回顾Servlet

1. 新建一个Maven工程当做父工程！pom依赖！

   ```xml
   <dependencies>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.1.9.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>servlet-api</artifactId>
          <version>2.5</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>jsp-api</artifactId>
          <version>2.2</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>jstl</artifactId>
          <version>1.2</version>
      </dependency>
   </dependencies>
   ```

2. 建立一个Moudle：springmvc-01-servlet ， 添加Web app的支持！

3. 导入servlet 和 jsp 的 jar 依赖

   ```xml
   <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
   </dependency>
   <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.2</version>
   </dependency>
   ```

4. 编写一个Servlet类，用来处理用户的请求

   ```java
   package com.kuang.servlet;
   
   //实现Servlet接口
   public class HelloServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throwsServletException, IOException {
          //取得参数
          String method = req.getParameter("method");
          if (method.equals("add")){
              req.getSession().setAttribute("msg","执行了add方法");
         }
          if (method.equals("delete")){
              req.getSession().setAttribute("msg","执行了delete方法");
         }
          //业务逻辑
          //视图跳转
          req.getRequestDispatcher("/WEB-INF/jsp/hello.jsp").forward(req,resp);
     }
   
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throwsServletException, IOException {
          doGet(req,resp);
     }
   }
   ```

5. 编写Hello.jsp，在WEB-INF目录下新建一个jsp的文件夹，新建hello.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
       <head>
           <title>Kuangshen</title>
       </head>
       <body>
           ${msg}
       </body>
   </html>
   ```

6. 在web.xml中注册Servlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
           version="4.0">
      <servlet>
          <servlet-name>HelloServlet</servlet-name>
          <servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>HelloServlet</servlet-name>
          <!-- 路径拦截映射，/user 路径由该 HelloServlet 处理 -->
          <url-pattern>/user</url-pattern>
      </servlet-mapping>
       <!-- 会话超时设置 -->
       <session-config>
           <session-timeout></session-timeout>
       </session-config>
       <!-- 默认启动页面 -->
       <welcome-file-list>
          <welcome-file>index.jsp</welcome-file>
      </welcome-file-list>
   
   </web-app>
   ```

7. 配置Tomcat，并启动测试

8. - localhost:8080/user?method=add
   - localhost:8080/user?method=delete

**MVC框架要做哪些事情**

1. 将url映射到java类或java类的方法 .
2. 封装用户提交的数据 .
3. 处理请求--调用相关的业务处理--封装响应数据 .
4. 将响应的数据进行渲染 . jsp / html 等表示层数据 .

**说明：**

​	常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端MVC框架：vue、angularjs、react、backbone；由MVC演化出了另外一些模式如：MVP、MVVM 等等....



## Hello，SpringMVC

> 第一个 SpringMVC 程序

### 配置版

1、新建一个Moudle ， springmvc-02-hello ， 添加web的支持！

2、确定导入了SpringMVC 的依赖！

3、配置web.xml  ， 注册DispatcherServlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
        version="4.0">

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

</web-app>
```

4、编写SpringMVC 的 配置文件！名称：springmvc-servlet.xml  : [servletname]-servlet.xml

说明，这里的名称要求是按照官方来的

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

5、添加 处理映射器

```xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
```

6、添加 处理器适配器

```xml
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
```

7、添加 视图解析器

```xml
<!--视图解析器:DispatcherServlet给他的ModelAndView
    1、获取ModelAndView的数据
    2、解析ModelAndView的视图名字
    3、拼接视图名字，找到对应的视图 /WEB-INF/jsp/xx.jsp-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"         id="InternalResourceViewResolver">
   <!--前缀-->
   <property name="prefix" value="/WEB-INF/jsp/"/>
   <!--后缀-->
   <property name="suffix" value=".jsp"/>
</bean>
```

8、编写我们要操作业务Controller ，要么实现Controller接口，要么增加注解；需要返回一个ModelAndView，装数据，封视图；

```java
package com.kuang.controller;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//注意：这里我们先导入Controller接口
public class HelloController implements Controller {

   public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
       //ModelAndView 模型和视图
       ModelAndView mv = new ModelAndView();

       //封装对象，放在ModelAndView中。Model
       mv.addObject("msg","HelloSpringMVC!");
       //封装要跳转的视图，放在ModelAndView中
       mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
       return mv;
  }
   
}
```

9、将自己的类交给SpringIOC容器，注册bean

```xml
<!--Handler，这里加“/”是因为用的处理映射器是-->
<bean id="/hello" class="com.kuang.controller.HelloController"/>
```

10、写要跳转的jsp页面，显示ModelandView存放的数据，以及我们的正常页面；

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Kuangshen</title>
    </head>
    <body>
        ${msg}
    </body>
</html>
```

11、配置Tomcat 启动测试！

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181229)

**可能遇到的问题：访问出现404，排查步骤：**

1. 查看控制台输出，看一下是不是缺少了什么jar包。
2. 如果jar包存在，显示无法输出，就在IDEA的项目发布 Artifacts 中，在 WEB-INF 文件夹下添加lib 文件夹，然后添加 lib 依赖！
3. 重启Tomcat 即可解决！

小结：看这个估计大部分同学都能理解其中的原理了，但是我们实际开发才不会这么写，不然就疯了，还学这个玩意干嘛！我们来看个注解版实现，这才是SpringMVC的精髓，到底有多么简单，看这个图就知道了。

### 注解版

**1、新建一个Moudle，springmvc-03-hello-annotation 。添加web支持！**

2、由于Maven可能存在资源过滤的问题，我们将配置完善

```xml
<build>
   <resources>
       <resource>
           <directory>src/main/java</directory>
           <includes>
               <include>**/*.properties</include>
               <include>**/*.xml</include>
           </includes>
           <filtering>false</filtering>
       </resource>
       <resource>
           <directory>src/main/resources</directory>
           <includes>
               <include>**/*.properties</include>
               <include>**/*.xml</include>
           </includes>
           <filtering>false</filtering>
       </resource>
   </resources>
</build>

```

3、在pom.xml文件引入相关的依赖：主要有Spring框架核心库、Spring MVC、servlet , JSTL等。我们在父依赖中已经引入了！

**4、配置web.xml**

注意点：

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

**/ 和 /\* 的区别：**< url-pattern > / </ url-pattern > 不会匹配到.jsp， 只针对我们编写的请求，即.jsp 不会进入spring的 DispatcherServlet类 ,直接就进入jsp页面。< url-pattern > /* </ url-pattern > 会匹配 *.jsp，会出现视图解析器返回 jsp视图时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报404错。

1. - 注意web.xml版本问题，要最新版！
   - 注册DispatcherServlet
   - 关联SpringMVC的配置文件
   - 启动级别为1
   - 映射路径为 / 【不要用/*，会404】

**5、添加Spring MVC配置文件**

在resource目录下添加springmvc-servlet.xml配置文件，配置的形式与Spring容器配置基本类似，为了支持基于注解的IOC，设置了自动扫描包的功能，具体配置信息如下：

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
   <context:component-scan base-package="com.kuang.controller"/>
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

   <!--视图解析器:DispatcherServlet给他的ModelAndView
    1、获取ModelAndView的数据
    2、解析ModelAndView的视图名字
    3、拼接视图名字，找到对应的视图 /WEB-INF/jsp/xx.jsp-->
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
         id="internalResourceViewResolver">
       <!-- 前缀 -->
       <property name="prefix" value="/WEB-INF/jsp/" />
       <!-- 后缀 -->
       <property name="suffix" value=".jsp" />
   </bean>

</beans>
```

在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。

- 让IOC的注解生效
- 静态资源过滤 ：HTML 、 JS 、 CSS 、 图片 、 视频 .....
- MVC的注解驱动
- 配置视图解析器

**6、创建Controller**

编写一个Java控制类：com.kuang.controller.HelloController , 注意编码规范

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/HelloController")
public class HelloController {

   //真实访问地址 : 项目名/HelloController/hello
   @RequestMapping("/hello")
   public String sayHello(Model model){
       //Spring MVC会自动实例化一个Model对象用于向视图中传值
       //向模型中添加属性msg与值，可以在JSP页面中取出并渲染
       model.addAttribute("msg","hello,SpringMVC");
       //web-inf/jsp/hello.jsp
       return "hello";
  }
}
```

- @Controller是为了让Spring IOC容器初始化时自动扫描到；
- @RequestMapping是为了映射URL请求路径，这里因为类与方法上都有映射所以访问时应该是/HelloController/hello；
- 方法中声明Model类型的参数是为了把Action中的数据带到视图中；
- 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/**hello**.jsp。

7、**创建视图层**

在WEB-INF/ jsp目录中创建hello.jsp ， 视图可以直接取出并展示从Controller带回的信息；

可以通过EL表示取出Model中存放的值，或者对象；

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>SpringMVC</title>
    </head>
    <body>
        ${msg}
    </body>
</html>
~~~

**8、配置Tomcat运行**

配置Tomcat ，  开启服务器 ， 访问 对应的请求路径！

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181239)

**OK，运行成功！**



### 小结

实现步骤其实非常的简单：

1. 新建一个web项目
2. 导入相关jar包
3. 编写web.xml , 注册DispatcherServlet
4. 编写springmvc配置文件
5. 接下来就是去创建对应的控制类 , controller
6. 最后完善前端视图和controller之间的对应
7. 测试运行调试.

使用springMVC必须配置的三大件：

**处理器映射器、处理器适配器、视图解析器**

通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而省去了大段的xml配置



## Spring MVC 的核心组件

- DispatcherServlet：前置控制器，是整个流程控制的核心，控制其他组件的执行，进行统一调度，降低组件之间的耦合性，相当于总指挥。
- Handler：处理器，完成具体的业务逻辑，相当于 Servlet 或 Action。
- HandlerMapping：DispatcherServlet 接收到请求之后，通过 HandlerMapping 将不同的请求映射到不同的 Handler。
- HandlerInterceptor：处理器拦截器，是一个接口，如果需要完成一些拦截处理，可以实现该接口。
- HandlerExecutionChain：处理器执行链，包括两部分内容：Handler 和 HandlerInterceptor（系统会有一个默认的 HandlerInterceptor，如果需要额外设置拦截，可以添加拦截器）。
- HandlerAdapter：处理器适配器，Handler 执行业务方法之前，需要进行一系列的操作，包括表单数据的验证、数据类型的转换、将表单数据封装到 JavaBean 等，这些操作都是由 HandlerApater 来完成，开发者只需将注意力集中业务逻辑的处理上，DispatcherServlet 通过 HandlerAdapter 执行不同的 Handler。
- ModelAndView：装载了模型数据和视图信息，作为 Handler 的处理结果，返回给 DispatcherServlet。
- ViewResolver：视图解析器，DispatcheServlet 通过它将逻辑视图解析为物理视图，最终将渲染结果响应给客户端。

## 中心控制器  DispatcherServlet

​	Spring 的web框架围绕 DispatcherServlet 设计。DispatcherServlet 的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的 @controller 声明方式。

​	Spring MVC框架像许多其他MVC框架一样, **以请求为驱动** , **围绕一个中心Servlet分派请求及提供其他功能**，**DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)**。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181243)

SpringMVC的原理如下图所示：

​	当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者。

![image-20200622175628510](https://gitee.com/xudongyin/img/raw/master/img/20200719181249.png)

### SpringMVC执行原理

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181258.png)

图为SpringMVC的一个较完整的流程图，**实线表示SpringMVC框架提供的技术，不需要开发者实现，虚线表示需要开发者实现**。

**简要分析执行流程**

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。

   我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

   

   **如上url拆分成三部分：**

   http://localhost:8080服务器域名

   SpringMVC部署在服务器上的web站点

   hello表示控制器

   通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

6. Handler让具体的Controller执行。

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12. 最终视图呈现给用户。

## Spring MVC 的工作流程

- 客户端请求被 DisptacherServlet 接收。
- 根据 HandlerMapping 映射到 Handler。
- 生成 Handler 和 HandlerInterceptor。
- Handler 和 HandlerInterceptor 以 HandlerExecutionChain 的形式一并返回给 DisptacherServlet。
- DispatcherServlet 通过 HandlerAdapter 调用 Handler 的方法完成业务逻辑处理。
- Handler 返回一个 ModelAndView 给 DispatcherServlet。
- DispatcherServlet 将获取的 ModelAndView 对象传给 ViewResolver 视图解析器，将逻辑视图解析为物理视图 View。
- ViewResovler 返回一个 View 给 DispatcherServlet。
- DispatcherServlet 根据 View 进行视图渲染（将模型数据 Model 填充到视图 View 中）。
- DispatcherServlet 将渲染后的结果响应给客户端。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181313.jpg)

Spring MVC 流程非常复杂，实际开发中很简单，因为大部分的组件不需要开发者创建、管理，只需要通过配置文件的方式完成配置即可，真正需要开发者进行处理的只有 Handler 、View。

## 控制器Controller

- 控制器复杂提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现。
- 控制器负责解析用户的请求并将其转换为一个模型。
- 在Spring MVC中一个控制器类可以包含多个方法
- 在Spring MVC中，对于Controller的配置方式有很多种

### 实现Controller接口

Controller是一个接口，在org.springframework.web.servlet.mvc包下，接口中只有一个方法；

```java
//实现该接口的类获得控制器功能
public interface Controller {
   //处理请求且返回一个模型与视图对象
   ModelAndView handleRequest(HttpServletRequest var1, HttpServletResponsevar2) throws Exception;
}
```

**测试**

1. 新建一个Moudle，springmvc-04-controller 。将刚才的03 拷贝一份, 我们进行操作！

2. - 删掉HelloController
   - mvc的配置文件只留下 视图解析器！

3. 编写一个Controller类，ControllerTest1

   ```java
   //定义控制器
   //注意点：不要导错包，实现Controller接口，重写方法；
   public class ControllerTest1 implements Controller {
   
      public ModelAndView handleRequest(HttpServletRequest httpServletRequest,HttpServletResponse httpServletResponse) throws Exception {
          //返回一个模型视图对象
          ModelAndView mv = new ModelAndView();
          mv.addObject("msg","Test1Controller");
          mv.setViewName("test");
          return mv;
     }
   }
   ```

4. 编写完毕后，去Spring配置文件中注册请求的bean；name对应请求路径，class对应处理请求的类

   ```xml
   <bean name="/t1" class="com.kuang.controller.ControllerTest1"/>
   ```

5. 编写前端test.jsp，注意在WEB-INF/jsp目录下编写，对应我们的视图解析器

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
       <head>
           <title>Kuangshen</title>
       </head>
       <body>
           ${msg}
       </body>
   </html>
   ```

6. 配置Tomcat运行测试，我这里没有项目发布名配置的就是一个 / ，所以请求不用加项目名，OK！

   

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181320)

**说明：**

- 实现接口Controller定义控制器是较老的办法

- 缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个Controller；定义的方式比较麻烦；

  

### 使用注解@Controller

- @Controller注解类型用于声明Spring类的实例是一个控制器（在讲IOC时还提到了另外3个注解）；

- Spring可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到你的控制器，需要在配置文件中声明组件扫描。

  ```xml
  <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
  <context:component-scan base-package="com.kuang.controller"/>
  ```

- 增加一个ControllerTest2类，使用注解实现；

  ```java
  //@Controller注解的类会自动添加到Spring上下文中
  @Controller
  public class ControllerTest2{
  
     //映射访问路径
     @RequestMapping("/t2")
     public String index(Model model){
         //Spring MVC会自动实例化一个Model对象用于向视图中传值
         model.addAttribute("msg", "ControllerTest2");
         //返回视图位置
         return "test";
    }
  
  }
  ```

- 运行tomcat测试

  ![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181327)

**可以发现，我们的两个请求都可以指向一个视图，但是页面结果的结果是不一样的，从这里可以看出视图是被复用的，而控制器与视图之间是弱偶合关系。**

**注解方式是平时使用的最多的方式！**



### **@RequestMapping**

- @RequestMapping注解用于映射url到控制器类或一个特定的处理程序方法。可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

- 为了测试结论更加准确，我们可以加上一个项目名测试 myweb

- 只注解在方法上面

  ```java
  @Controller
  public class TestController {
     @RequestMapping("/h1")
     public String test(){
         return "test";
    }
  }
  ```

  访问路径：http://localhost:8080 / 项目名 / h1

- 同时注解类与方法

  ```java
  @Controller
  @RequestMapping("/admin")
  public class TestController {
     @RequestMapping("/h1")
     public String test(){
         return "test";
    }
  }
  ```

  访问路径：http://localhost:8080 / 项目名/ admin /h1  , 需要先指定类的路径再指定方法的路径；

**使用method属性指定请求类型**

用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE等

我们来测试一下：

- 增加一个方法

  ```java
  //映射访问路径,必须是POST请求
  @RequestMapping(value = "/hello",method = {RequestMethod.POST})
  public String index2(Model model){
     model.addAttribute("msg", "hello!");
     return "test";
  }
  ```

- 我们使用浏览器地址栏进行访问默认是Get请求，会报错405：

  ![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181331)

- 如果将POST修改为GET则正常了；

  ```java
  //映射访问路径,必须是Get请求
  @RequestMapping(value = "/hello",method = {RequestMethod.GET})
  public String index2(Model model){
     model.addAttribute("msg", "hello!");
     return "test";
  }
  ```

  ![img](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7JOmNdhqNbrRK9XaseXIDsua1f90EJnCRJxuBoeSWiaTmdVfjIDgUo5v6ENY5N307GfoXUp4PxYu0g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**小结：**

Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH。

**所有的地址栏请求默认都会是 HTTP GET 类型的。**

方法级别的注解变体有如下几个：组合注解

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

@GetMapping 是一个组合注解，平时使用的会比较多！

它所扮演的是 @RequestMapping(method =RequestMethod.GET) 的一个快捷方式。

**使用params：指定请求中必须包含某些参数，否则无法调用该方法。**

```java
@RequestMapping(value = "/index",method = RequestMethod.GET,params = {"name","id=10"})
public String index(){
    System.out.println("执行了index...");
    return "index";
}
```

上述代码表示请求中必须包含 name 和 id 两个参数，同时 id 的值必须是 10。

关于参数绑定，在形参列表中通过添加 @RequestParam 注解完成 HTTP 请求参数与业务方法形参的映射。

```java
@RequestMapping(value = "/index",method = RequestMethod.GET,params = {"name","id=10"})
public String index(@RequestParam("name") String str,@RequestParam("id") int age){
    System.out.println(str);
    System.out.println(age);
    System.out.println("执行了index...");
    return "index";
}
```

上述代码表示将请求的参数 name 和 id 分别赋给了形参 str 和 age ，同时自动完成了数据类型转换，将 “10” 转为了 int 类型的 10，再赋给 age，这些工作都是由 HandlerAdapter 来完成的。



### RestFul 风格

**概念**

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

REST：Representational State Transfer，资源表现层状态转换，是目前比较主流的一种互联网软件架构，它结构清晰、标准规范、易于理解、便于扩展。

- 资源（Resource）

网络上的一个实体，或者说网络中存在的一个具体信息，一段文本、一张图片、一首歌曲、一段视频等等，总之就是一个具体的存在。可以用一个 URI（统一资源标识符）指向它，每个资源都有对应的一个特定的 URI，要获取该资源时，只需要访问对应的 URI 即可。

- 表现层（Representation）

资源具体呈现出来的形式，比如文本可以用 txt 格式表示，也可以用 HTML、XML、JSON等格式来表示。

- 状态转换（State Transfer）

客户端如果希望操作服务器中的某个资源，就需要通过某种方式让服务端发生状态转换，而这种转换是建立在表现层之上的，所以叫做"表现层状态转换"。

**特点**

- URL 更加简洁。
- 有利于不同系统之间的资源共享，只需要遵守一定的规范，不需要进行其他配置即可实现资源共享。

**功能**

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

分别对应 添加、 删除、修改、查询。

**传统方式操作资源**  ：通过不同的参数来实现不同的效果！方法单一，post 和 get

​	http://127.0.0.1/item/queryItem.action?id=1 查询,GET

​	http://127.0.0.1/item/saveItem.action 新增,POST

​	http://127.0.0.1/item/updateItem.action 更新,POST

​	http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST

**使用RESTful操作资源** ：可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！

​	http://127.0.0.1/item/1 查询,GET

​	http://127.0.0.1/item 新增,POST

​	http://127.0.0.1/item 更新,PUT（根据请求方法类型来与上面区别）

​	http://127.0.0.1/item/1 删除,DELETE

**学习测试**

1. 在新建一个类 RestFulController

   ```java
   @Controller
   public class RestFulController {
   }
   ```

2. 在Spring MVC中可以使用  @PathVariable 注解，让方法参数的值对应绑定到一个URI模板变量上。

   ```java
   @Controller
   public class RestFulController {
   
      //映射访问路径
      @RequestMapping("/commit/{p1}/{p2}")
      public String index(@PathVariable int p1, @PathVariable int p2, Model model){
          
          int result = p1+p2;
          //Spring MVC会自动实例化一个Model对象用于向视图中传值
          model.addAttribute("msg", "结果："+result);
          //返回视图位置
          return "test";
          
     }
      
   }
   ```

3. 我们来测试请求查看下

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181340)

4. 思考：使用路径变量的好处？

5. - 使路径变得更加简洁；

   - 获得参数更加方便，框架会自动进行类型转换。

   - 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败。

     

     ![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181343)

6. 我们来修改下对应的参数类型，再次测试

   ```java
   //映射访问路径
   @RequestMapping("/commit/{p1}/{p2}")
   public String index(@PathVariable int p1, @PathVariable String p2, Model model){
   
      String result = p1+p2;
      //Spring MVC会自动实例化一个Model对象用于向视图中传值
      model.addAttribute("msg", "结果："+result);
      //返回视图位置
      return "test";
   
   }
   ```

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181348)



## 结果跳转方式

### ModelAndView

设置ModelAndView对象 , 根据view的名称 , 和视图解析器跳到指定的页面 .

页面 : {视图解析器前缀} + viewName +{视图解析器后缀}

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
     id="internalResourceViewResolver">
   <!-- 前缀 -->
   <property name="prefix" value="/WEB-INF/jsp/" />
   <!-- 后缀 -->
   <property name="suffix" value=".jsp" />
</bean>
```

对应的controller类

```java
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest,HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```



### ServletAPI

通过设置ServletAPI , 不需要视图解析器 .

1、通过HttpServletResponse进行输出

2、通过HttpServletResponse实现重定向

3、通过HttpServletResponse实现转发

```java
@Controller
public class ResultGo {

   @RequestMapping("/result/t1")
   public void test1(HttpServletRequest req, HttpServletResponse rsp) throwsIOException {
       rsp.getWriter().println("Hello,Spring BY servlet API");
  }

   @RequestMapping("/result/t2")
   public void test2(HttpServletRequest req, HttpServletResponse rsp) throwsIOException {
       rsp.sendRedirect("/index.jsp");
  }

   @RequestMapping("/result/t3")
   public void test3(HttpServletRequest req, HttpServletResponse rsp) throwsException {
       //转发
       req.setAttribute("msg","/result/t3");
       req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,rsp);
  }

}
```



### SpringMVC

**通过SpringMVC来实现转发和重定向 - 无需视图解析器；**

**测试前，需要将视图解析器注释掉**

```java
@Controller
public class ResultSpringMVC {
   @RequestMapping("/rsm/t1")
   public String test1(){
       //转发
       return "/index.jsp";
  }

   @RequestMapping("/rsm/t2")
   public String test2(){
       //转发二
       return "forward:/index.jsp";
  }

   @RequestMapping("/rsm/t3")
   public String test3(){
       //重定向
       return "redirect:/index.jsp";
  }
}
```



**通过SpringMVC来实现转发和重定向 - 有视图解析器；**

**重定向 , 不需要视图解析器 , 本质就是重新请求一个新地方嘛 , 所以注意路径问题.**

可以重定向到另外一个请求实现 .

```java
@Controller
public class ResultSpringMVC2 {
   @RequestMapping("/rsm2/t1")
   public String test1(){
       //转发
       return "test";
  }

   @RequestMapping("/rsm2/t2")
   public String test2(){
       //重定向
       //return "redirect:/index.jsp";
       return "redirect:/hello"; //hello为另一个请求路径/
  }

}
```



## 数据处理

### 处理提交数据

**1、提交的域名称和处理方法的参数名一致**

提交数据 : http://localhost:8080/hello?name=kuangshen

处理方法 :

```java
@RequestMapping("/hello")
public String hello(String name){
   System.out.println(name);
   return "hello";
}
```

后台输出 : kuangshen

**2、提交的域名称和处理方法的参数名不一致**

提交数据 : http://localhost:8080/hello?username=kuangshen

处理方法 :

```java
//@RequestParam("username") : username提交的域的名称 .
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
   System.out.println(name);
   return "hello";
}
```

后台输出 : kuangshen

**3、提交的是一个对象**

要求提交的表单域和对象的属性名一致  , 参数使用对象即可

1、实体类

```java
public class User {
   private int id;
   private String name;
   private int age;
   //构造
   //get/set
   //tostring()
}
```

2、提交数据 : http://localhost:8080/mvc04/user?name=kuangshen&id=1&age=15

3、处理方法 :

```java
@RequestMapping("/user")
public String user(User user){
   System.out.println(user);
   return "hello";
}
```

后台输出 : User { id=1, name='kuangshen', age=15 }

说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是null。



### 数据显示到前端

**第一种 : 通过ModelAndView**

实现controller接口

```java
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest,HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```

**第二种 : 通过ModelMap**

ModelMap

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

**第三种 : 通过Model**

Model

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



### 对比

就对于新手而言简单来说使用区别就是：

- Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；
- ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；
- ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。

当然更多的以后开发考虑的更多的是性能和优化，就不能单单仅限于此的了解。

**请使用80%的时间打好扎实的基础，剩下18%的时间研究框架，2%的时间去学点英文，框架的官方文档永远是最好的教程。**



### 乱码问题

测试步骤：

1、我们可以在首页编写一个提交的表单

```jsp
<form action="/e/t" method="post">
 <input type="text" name="name">
 <input type="submit">
</form>
```

2、后台编写对应的处理类

```java
@Controller
public class Encoding {
   @RequestMapping("/e/t")
   public String test(Model model,String name){
       model.addAttribute("msg",name); //获取表单提交的值
       return "test"; //跳转到test页面显示输入的值
  }
}
```

3、输入中文测试，发现乱码

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181400)

不得不说，乱码问题是在我们开发中十分常见的问题，也是让我们程序猿比较头大的问题！

以前乱码问题通过过滤器解决 , 而SpringMVC给我们提供了一个过滤器 , 可以在web.xml中配置 .

修改了xml文件需要重启服务器！

```xml
<filter>
    <filter-name>encodingFilter</filter-name><!-- 自定义 -->
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<!-- 对所有请求生效 -->
<filter-mapping>
    <filter-name>encodingFilter</filter-name><!-- 跟上面自定义的名字对应 -->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

但是我们发现 , 有些极端情况下，这个过滤器对get的支持不好 .

处理方法 :

1、修改tomcat配置文件 ：设置编码！

```xml
<Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
          connectionTimeout="20000"
          redirectPort="8443" />
```

2、自定义过滤器

```java
package com.kuang.filter;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.Map;

/**
* 解决get和post请求 全部乱码的过滤器
*/
public class GenericEncodingFilter implements Filter {

   @Override
   public void destroy() {
  }

   @Override
   public void doFilter(ServletRequest request, ServletResponse response, FilterChainchain) throws IOException, ServletException {
       //处理response的字符编码
       HttpServletResponse myResponse=(HttpServletResponse) response;
       myResponse.setContentType("text/html;charset=UTF-8");

       // 转型为与协议相关对象
       HttpServletRequest httpServletRequest = (HttpServletRequest) request;
       // 对request包装增强
       HttpServletRequest myrequest = new MyRequest(httpServletRequest);
       chain.doFilter(myrequest, response);
  }

   @Override
   public void init(FilterConfig filterConfig) throws ServletException {
  }

}

//自定义request对象，HttpServletRequest的包装类
class MyRequest extends HttpServletRequestWrapper {

   private HttpServletRequest request;
   //是否编码的标记
   private boolean hasEncode;
   //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
   public MyRequest(HttpServletRequest request) {
       super(request);// super必须写
       this.request = request;
  }

   // 对需要增强方法 进行覆盖
   @Override
   public Map getParameterMap() {
       // 先获得请求方式
       String method = request.getMethod();
       if (method.equalsIgnoreCase("post")) {
           // post请求
           try {
               // 处理post乱码
               request.setCharacterEncoding("utf-8");
               return request.getParameterMap();
          } catch (UnsupportedEncodingException e) {
               e.printStackTrace();
          }
      } else if (method.equalsIgnoreCase("get")) {
           // get请求
           Map<String, String[]> parameterMap = request.getParameterMap();
           if (!hasEncode) { // 确保get手动编码逻辑只运行一次
               for (String parameterName : parameterMap.keySet()) {
                   String[] values = parameterMap.get(parameterName);
                   if (values != null) {
                       for (int i = 0; i < values.length; i++) {
                           try {
                               // 处理get乱码
                               values[i] = new String(values[i]
                                      .getBytes("ISO-8859-1"), "utf-8");
                          } catch (UnsupportedEncodingException e) {
                               e.printStackTrace();
                          }
                      }
                  }
              }
               hasEncode = true;
          }
           return parameterMap;
      }
       return super.getParameterMap();
  }

   //取一个值
   @Override
   public String getParameter(String name) {
       Map<String, String[]> parameterMap = getParameterMap();
       String[] values = parameterMap.get(name);
       if (values == null) {
           return null;
      }
       return values[0]; // 取回参数的第一个值
  }

   //取所有值
   @Override
   public String[] getParameterValues(String name) {
       Map<String, String[]> parameterMap = getParameterMap();
       String[] values = parameterMap.get(name);
       return values;
  }
}
```

这个也是我在网上找的一些大神写的，一般情况下，SpringMVC默认的乱码处理就已经能够很好的解决了！

**然后在web.xml中配置这个过滤器即可！**

乱码问题，需要平时多注意，在尽可能能设置编码的地方，都设置为统一编码 UTF-8！



## 什么是JSON？

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
- 采用完全独立于编程语言的**文本格式**来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

```
{"name": "QinJiang"}
{"age": "3"}
{"sex": "男"}
```

很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

```html
var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```

### **JSON 和 JavaScript 对象互转**

要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法：

```
var obj = JSON.parse('{"a": "Hello", "b": "World"}');
//结果是 {a: 'Hello', b: 'World'}
```

要实现从JavaScript 对象转换为JSON字符串，使用 JSON.stringify() 方法：

```
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
```



### **代码测试**

1、新建一个module ，springmvc-05-json ， 添加web的支持

2、在web目录下新建一个 json-1.html ， 编写测试内容

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>JSON_秦疆</title>
    </head>
    <body>

        <script type="text/javascript">
            //编写一个js的对象
            var user = {
                name:"秦疆",
                age:3,
                sex:"男"
            };
            //将js对象转换成json字符串，let与var一样
            let s = JSON.stringify(usr);
            console.log(s);

            //将json字符串转换为js对象，let与var一样
            let parse = JSON.parse(s);
            console.log(parse);

        </script>

    </body>
</html>
```

3、在IDEA中使用浏览器打开，查看控制台输出！

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181408)



> Controller返回JSON数据

Jackson应该是目前比较好的json解析工具了

当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等。

我们这里使用Jackson，使用它需要导入它的jar包；

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
   <groupId>com.fasterxml.jackson.core</groupId>
   <artifactId>jackson-databind</artifactId>
   <version>2.9.8</version>
</dependency>
```

配置SpringMVC需要的配置

web.xml

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
       <url-pattern>/</url-pattern>
   </filter-mapping>

</web-app>
```

springmvc-servlet.xml

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

   <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
   <context:component-scan base-package="com.kuang.controller"/>

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

我们随便编写一个User的实体类，然后我们去编写我们的测试Controller；

```java
package com.kuang.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

//需要导入lombok
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {

   private String name;
   private int age;
   private String sex;
   
}
```

这里我们需要两个新东西，一个是@ResponseBody，一个是ObjectMapper对象，我们看下具体的用法

编写一个Controller；

```java
@Controller
public class UserController {

   @RequestMapping("/json1")
   @ResponseBody
   public String json1() throws JsonProcessingException {
       //创建一个jackson的对象映射器，用来解析数据
       ObjectMapper mapper = new ObjectMapper();
       //创建一个对象
       User user = new User("秦疆1号", 3, "男");
       //将我们的对象解析成为json格式
       String str = mapper.writeValueAsString(user);
       //由于@ResponseBody注解，这里会将str转成json格式返回；十分方便
       return str;
  }

}
```

配置Tomcat ， 启动测试一下！

http://localhost:8080/json1

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181414)

发现出现了乱码问题，我们需要设置一下他的编码格式为utf-8，以及它返回的类型；

通过@RequestMapping 的 produces 属性来实现，修改下代码

```java
//produces:指定响应体返回类型和编码
@RequestMapping(value = "/json1",produces = "application/json;charset=utf-8")
```

再次测试， http://localhost:8080/json1 ， 乱码问题OK！

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181418)

【注意：使用json记得处理乱码问题】

> 代码优化

### **乱码统一解决**

上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过Spring配置统一指定，这样就不用每次都去处理了！

我们可以在springmvc的配置文件上添加一段消息StringHttpMessageConverter转换配置！

```xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```



### **返回json字符串统一解决**

在类上直接使用 **@RestController** ，**这样子，里面所有的方法都只会返回 json 字符串了，不会进行视图解析**，不用再每一个都添加@ResponseBody ！我们在前后端分离开发中，一般都使用 @RestController ，十分便捷！

```java
@RestController
public class UserController {

   //produces:指定响应体返回类型和编码
   @RequestMapping(value = "/json1")
   public String json1() throws JsonProcessingException {
       //创建一个jackson的对象映射器，用来解析数据
       ObjectMapper mapper = new ObjectMapper();
       //创建一个对象
       User user = new User("秦疆1号", 3, "男");
       //将我们的对象解析成为json格式
       String str = mapper.writeValueAsString(user);
       //由于@ResponseBody注解，这里会将str转成json格式返回；十分方便
       return str;
  }

}
```

启动tomcat测试，结果都正常输出！

> 测试集合输出

增加一个新的方法

```java
@RequestMapping("/json2")
public String json2() throws JsonProcessingException {

   //创建一个jackson的对象映射器，用来解析数据
   ObjectMapper mapper = new ObjectMapper();
   //创建一个对象
   User user1 = new User("秦疆1号", 3, "男");
   User user2 = new User("秦疆2号", 3, "男");
   User user3 = new User("秦疆3号", 3, "男");
   User user4 = new User("秦疆4号", 3, "男");
   List<User> list = new ArrayList<User>();
   list.add(user1);
   list.add(user2);
   list.add(user3);
   list.add(user4);

   //将我们的对象解析成为json格式
   String str = mapper.writeValueAsString(list);
   return str;
}
```

运行结果 : 十分完美，没有任何问题！

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181423)

> 输出时间对象

增加一个新的方法

```java
@RequestMapping("/json3")
public String json3() throws JsonProcessingException {

   ObjectMapper mapper = new ObjectMapper();

   //创建时间一个对象，java.util.Date
   Date date = new Date();
   //将我们的对象解析成为json格式
   String str = mapper.writeValueAsString(date);
   return str;
}
```

运行结果 :

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181426)

- 默认日期格式会变成一个数字，是1970年1月1日到当前日期的毫秒数！
- Jackson 默认是会把时间转成timestamps形式

**解决方案：取消timestamps形式 ， 自定义时间格式**

```java
@RequestMapping("/json4")
public String json4() throws JsonProcessingException {

   ObjectMapper mapper = new ObjectMapper();

   //不使用时间戳的方式
   mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
   //自定义日期格式对象
   SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
   //指定日期格式
   mapper.setDateFormat(sdf);

   Date date = new Date();
   String str = mapper.writeValueAsString(date);

   return str;
}
```

运行结果 : 成功的输出了时间！

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181429)

> 抽取为工具类

**如果要经常使用的话，这样是比较麻烦的，我们可以将这些代码封装到一个工具类中；我们去编写下**

```java
package com.kuang.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.text.SimpleDateFormat;

public class JsonUtils {
   
   public static String getJson(Object object) {
       return getJson(object,"yyyy-MM-dd HH:mm:ss");
  }

   public static String getJson(Object object,String dateFormat) {
       ObjectMapper mapper = new ObjectMapper();
       //不使用时间差的方式
       mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
       //自定义日期格式对象
       SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
       //指定日期格式
       mapper.setDateFormat(sdf);
       try {
           return mapper.writeValueAsString(object);
      } catch (JsonProcessingException e) {
           e.printStackTrace();
      }
       return null;
  }
}
```

我们使用工具类，代码就更加简洁了！

```java
@RequestMapping("/json5")
public String json5() throws JsonProcessingException {
   Date date = new Date();
   String json = JsonUtils.getJson(date);
   return json;
}
```

大功告成！完美！



### FastJson

fastjson.jar是阿里开发的一款专门用于Java开发的包，可以方便的实现json对象与JavaBean对象的转换，实现JavaBean对象与json字符串的转换，实现json对象与json字符串的转换。实现json的转换方法很多，最后的实现结果都是一样的。

fastjson 的 pom依赖！

```xml
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>fastjson</artifactId>
   <version>1.2.60</version>
</dependency>
```

fastjson 三个主要的类：

**JSONObject  代表 json 对象** 

- JSONObject实现了Map接口, 猜想 JSONObject底层操作是由Map实现的。
- JSONObject对应json对象，通过各种形式的get()方法可以获取json对象中的数据，也可利用诸如size()，isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现Map接口并调用接口中的方法完成的。

**JSONArray  代表 json 对象数组**

- 内部是有List接口中的方法来完成操作的。

**JSON代表 JSONObject和JSONArray的转化**

- JSON类源码分析与使用
- 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间的相互转化。

**代码测试，我们新建一个FastJsonDemo 类**

```java
package com.kuang.controller;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.kuang.pojo.User;

import java.util.ArrayList;
import java.util.List;

public class FastJsonDemo {
   public static void main(String[] args) {
       //创建一个对象
       User user1 = new User("秦疆1号", 3, "男");
       User user2 = new User("秦疆2号", 3, "男");
       User user3 = new User("秦疆3号", 3, "男");
       User user4 = new User("秦疆4号", 3, "男");
       List<User> list = new ArrayList<User>();
       list.add(user1);
       list.add(user2);
       list.add(user3);
       list.add(user4);

       System.out.println("*******Java对象 转 JSON字符串*******");
       String str1 = JSON.toJSONString(list);
       System.out.println("JSON.toJSONString(list)==>"+str1);
       String str2 = JSON.toJSONString(user1);
       System.out.println("JSON.toJSONString(user1)==>"+str2);

       System.out.println("\n****** JSON字符串 转 Java对象*******");
       User jp_user1=JSON.parseObject(str2,User.class);
       System.out.println("JSON.parseObject(str2,User.class)==>"+jp_user1);

       System.out.println("\n****** Java对象 转 JSON对象 ******");
       JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
       System.out.println("(JSONObject) JSON.toJSON(user2)==>"+jsonObject1.getString("name"));

       System.out.println("\n****** JSON对象 转 Java对象 ******");
       User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
       System.out.println("JSON.toJavaObject(jsonObject1, User.class)==>"+to_java_user);
  }
}
```

这种工具类，我们只需要掌握使用就好了，在使用的时候在根据具体的业务去找对应的实现。和以前的commons-io那种工具包一样，拿来用就好了！

## AJAX

### 简介

- **AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。**
- AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。
- **Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。**
- 在 2005 年，Google 通过其 Google Suggest 使 AJAX 变得流行起来。Google Suggest能够自动帮你完成搜索单词。
- Google Suggest 使用 AJAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时，JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。
- 就和国内百度的搜索框一样!

- 传统的网页(即不用ajax技术的网页)，想要更新内容或者提交一个表单，都需要重新加载整个网页。
- 使用ajax技术的网页，通过在后台服务器进行少量的数据交换，就可以实现异步局部更新。
- 使用Ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的Web用户界面。



### 伪造Ajax

我们可以使用前端的一个标签来伪造一个ajax的样子。iframe标签

1、新建一个module ：sspringmvc-06-ajax ， 导入web支持！

2、编写一个 ajax-frame.html 使用 iframe 测试，感受下效果

```html
<!DOCTYPE html>
<html>
<head lang="en">
   <meta charset="UTF-8">
   <title>kuangshen</title>
</head>
<body>

<script type="text/javascript">
   window.onload = function(){
       var myDate = new Date();
       document.getElementById('currentTime').innerText = myDate.getTime();
  };

   function LoadPage(){
       var targetUrl =  document.getElementById('url').value;
       console.log(targetUrl);
       document.getElementById("iframePosition").src = targetUrl;
  }

</script>

<div>
   <p>请输入要加载的地址：<span id="currentTime"></span></p>
   <p>
       <input id="url" type="text" value="https://www.baidu.com/"/>
       <input type="button" value="提交" onclick="LoadPage()">
   </p>
</div>

<div>
   <h3>加载页面位置：</h3>
   <iframe id="iframePosition" style="width: 100%;height: 500px;"></iframe>
</div>

</body>
</html>
```

3、使用IDEA开浏览器测试一下！

**利用AJAX可以做：**

- 注册时，输入用户名自动检测用户是否已经存在。
- 登陆时，提示用户名密码错误
- 删除数据行时，将行ID发送到后台，后台在数据库中删除，数据库删除成功后，在页面DOM中将数据行也删除。
- ....等等



### jQuery.ajax

纯JS原生实现Ajax我们不去讲解这里，直接使用jquery提供的，方便学习和使用，避免重复造轮子，有兴趣的同学可以去了解下JS原生XMLHttpRequest ！

Ajax的核心是XMLHttpRequest对象(XHR)。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

jQuery 提供多个与 AJAX 有关的方法。

通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON – 同时您能够把这些外部数据直接载入网页的被选元素中。

jQuery 不是生产者，而是大自然搬运工。

jQuery Ajax本质就是 XMLHttpRequest，对他进行了封装，方便调用！

```
jQuery.ajax(...)
      部分参数：
            url：请求地址
            type：请求方式，GET、POST（1.9.0之后用method）
        headers：请求头
            data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
          async：是否异步
        timeout：设置请求超时时间（毫秒）
      beforeSend：发送请求前执行的函数(全局)
        complete：完成之后执行的回调函数(全局)
        success：成功之后执行的回调函数(全局)
          error：失败之后执行的回调函数(全局)
        accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
        dataType：将服务器端返回的数据转换成指定类型
          "xml": 将服务器端返回的内容转换成xml格式
          "text": 将服务器端返回的内容转换成普通文本格式
          "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
          "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```

**我们来个简单的测试，使用最原始的HttpServletResponse处理 , .最简单 , 最通用**

1、配置web.xml 和 springmvc的配置文件，复制上面案例的即可 【记得静态资源过滤和注解驱动配置上】

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

   <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
   <context:component-scan base-package="com.kuang.controller"/>
    <!--静态资源过滤-->
   <mvc:default-servlet-handler />
   <mvc:annotation-driven />

   <!-- 视图解析器 -->
   <beanclass="org.springframework.web.servlet.view.InternalResourceViewResolver"
         id="internalResourceViewResolver">
       <!-- 前缀 -->
       <property name="prefix" value="/WEB-INF/jsp/" />
       <!-- 后缀 -->
       <property name="suffix" value=".jsp" />
   </bean>

</beans>
```

2、编写一个AjaxController

```java
@Controller
public class AjaxController {

   @RequestMapping("/a1")
   public void ajax1(String name , HttpServletResponse response) throwsIOException {
       if ("admin".equals(name)){
           response.getWriter().print("true");
      }else{
           response.getWriter().print("false");
      }
  }

}
```

3、导入jquery ， 可以使用在线的CDN ， 也可以下载导入

```html
<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
<script src="${pageContext.request.contextPath}/statics/js/jquery-3.1.1.min.js"></script>
```

4、编写index.jsp测试

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
 <head>
   <title>$Title$</title>
  <%--<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>--%>
   <script src="${pageContext.request.contextPath}/statics/js/jquery-3.1.1.min.js"></script>
   <script>
       function a1(){
           <%-- $ 是 jQuery 的缩写--%>
           $.post({
               url:"${pageContext.request.contextPath}/a1",
               data:{'name':$("#txtName").val()},
               success:function (data,status) {
                    <%-- 这里的data是后台controller返回的数据--%>
                   alert(data);
                   alert(status);
              }
          });
      }
   </script>
 </head>
 <body>

<%--onblur：失去焦点触发事件--%>
用户名:<input type="text" id="txtName" onblur="a1()"/>

 </body>
</html>
```

5、启动tomcat测试！打开浏览器的控制台，当我们鼠标离开输入框的时候，可以看到发出了一个ajax的请求！是后台返回给我们的结果！测试成功！

 

### SpringMVC实现

实体类user

```JAVA
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {

   private String name;
   private int age;
   private String sex;

}
```

我们来获取一个集合对象，展示到前端页面

```JAVA
@RequestMapping("/a2")
public List<User> ajax2(){
   List<User> list = new ArrayList<User>();
   list.add(new User("秦疆1号",3,"男"));
   list.add(new User("秦疆2号",3,"男"));
   list.add(new User("秦疆3号",3,"男"));
   return list; //由于@RestController注解，将list转成json格式返回
}
```

前端页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
   <title>Title</title>
</head>
<body>
<input type="button" id="btn" value="获取数据"/>
<table width="80%" align="center">
   <tr>
       <td>姓名</td>
       <td>年龄</td>
       <td>性别</td>
   </tr>
   <tbody id="content">
   </tbody>
</table>

<script src="${pageContext.request.contextPath}/statics/js/jquery-3.1.1.min.js"></script>
<script>

   $(function () {
       <%-- 没有外面这层function嵌套的话，点击事件不能正常运行，
            点击事件在这里不是一个函数--%>
       $("#btn").click(function () {
           <%-- 缩写形式 $.post(url , param[可以省略], success)--%>
           $.post("${pageContext.request.contextPath}/a2",function (data) {
               console.log(data)
               var html="";
               for (var i = 0; i <data.length ; i++) {
                   html+= "<tr>" +
                       "<td>" + data[i].name + "</td>" +
                       "<td>" + data[i].age + "</td>" +
                       "<td>" + data[i].sex + "</td>" +
                       "</tr>"
              }
               $("#content").html(html);
          });
      })
  })
</script>
</body>
</html>
```

**成功实现了数据回显！可以体会一下Ajax的好处！**



### 注册提示效果

我们再测试一个小Demo，思考一下我们平时注册时候，输入框后面的实时提示怎么做到的；如何优化

我们写一个Controller

```java
@RequestMapping("/a3")
public String ajax3(String name,String pwd){
   String msg = "";
   //模拟数据库中存在数据
   if (name!=null){
       if ("admin".equals(name)){
           msg = "OK";
      }else {
           msg = "用户名输入错误";
      }
  }
   if (pwd!=null){
       if ("123456".equals(pwd)){
           msg = "OK";
      }else {
           msg = "密码输入有误";
      }
  }
   return msg; //由于@RestController注解，将msg转成json格式返回
}
```

前端页面 login.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
   <title>ajax</title>
   <script src="${pageContext.request.contextPath}/statics/js/jquery-3.1.1.min.js"></script>
   <script>

       function a1(){
           $.post({
               url:"${pageContext.request.contextPath}/a3",
               data:{'name':$("#name").val()},
               success:function (data) {
                   if (data.toString()=='OK'){
                       $("#userInfo").css("color","green");
                  }else {
                       $("#userInfo").css("color","red");
                  }
                   $("#userInfo").html(data);
              }
          });
      }
       function a2(){
           $.post({
               url:"${pageContext.request.contextPath}/a3",
               data:{'pwd':$("#pwd").val()},
               success:function (data) {
                   if (data.toString()=='OK'){
                       $("#pwdInfo").css("color","green");
                  }else {
                       $("#pwdInfo").css("color","red");
                  }
                   $("#pwdInfo").html(data);
              }
          });
      }

   </script>
</head>
<body>
<p>
  用户名:<input type="text" id="name" onblur="a1()"/>
   <span id="userInfo"></span>
</p>
<p>
  密码:<input type="text" id="pwd" onblur="a2()"/>
   <span id="pwdInfo"></span>
</p>
</body>
</html>
```

【记得处理json乱码问题】

我们可以在springmvc的配置文件上添加一段消息StringHttpMessageConverter转换配置！

```xml
<mvc:annotation-driven>
   <mvc:message-converters register-defaults="true">
       <bean class="org.springframework.http.converter.StringHttpMessageConverter">
           <constructor-arg value="UTF-8"/>
       </bean>
       <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
           <property name="objectMapper">
               <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                   <property name="failOnEmptyBeans" value="false"/>
               </bean>
           </property>
       </bean>
   </mvc:message-converters>
</mvc:annotation-driven>
```

测试一下效果，动态请求响应，局部刷新，就是如此！

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181447)



### 获取baidu接口Demo

```jsp
<!DOCTYPE HTML>
<html>
<head>
   <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
   <title>JSONP百度搜索</title>
   <style>
       #q{
           width: 500px;
           height: 30px;
           border:1px solid #ddd;
           line-height: 30px;
           display: block;
           margin: 0 auto;
           padding: 0 10px;
           font-size: 14px;
      }
       #ul{
           width: 520px;
           list-style: none;
           margin: 0 auto;
           padding: 0;
           border:1px solid #ddd;
           margin-top: -1px;
           display: none;
      }
       #ul li{
           line-height: 30px;
           padding: 0 10px;
      }
       #ul li:hover{
           background-color: #f60;
           color: #fff;
      }
   </style>
   <script>

       // 2.步骤二
       // 定义demo函数 (分析接口、数据)
       function demo(data){
           var Ul = document.getElementById('ul');
           var html = '';
           // 如果搜索数据存在 把内容添加进去
           if (data.s.length) {
               // 隐藏掉的ul显示出来
               Ul.style.display = 'block';
               // 搜索到的数据循环追加到li里
               for(var i = 0;i<data.s.length;i++){
                   html += '<li>'+data.s[i]+'</li>';
              }
               // 循环的li写入ul
               Ul.innerHTML = html;
          }
      }

       // 1.步骤一
       window.onload = function(){
           // 获取输入框和ul
           var Q = document.getElementById('q');
           var Ul = document.getElementById('ul');

           // 事件鼠标抬起时候
           Q.onkeyup = function(){
               // 如果输入框不等于空
               if (this.value != '') {
                   // ☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆JSONPz重点☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
                   // 创建标签
                   var script = document.createElement('script');
                   //给定要跨域的地址 赋值给src
                   //这里是要请求的跨域的地址 我写的是百度搜索的跨域地址
                   script.src ='https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+this.value+'&cb=demo';
                   // 将组合好的带src的script标签追加到body里
                   document.body.appendChild(script);
              }
          }
      }
   </script>
</head>

<body>
<input type="text" id="q" />
<ul id="ul">

</ul>
</body>
</html>
```

Ajax在我们开发中十分重要，一定要学会使用！

## 拦截器

> 概述

SpringMVC的处理器拦截器类似于Servlet开发中的过滤器Filter,用于对处理器进行预处理和后处理。开发者可以自己定义一些拦截器来实现特定的功能。

**过滤器与拦截器的区别：**拦截器是AOP思想的具体应用。

**过滤器**

- servlet规范中的一部分，任何java web工程都可以使用
- 在url-pattern中配置了/*之后，可以对所有要访问的资源进行拦截

**拦截器** 

- 拦截器是SpringMVC框架自己的，只有使用了SpringMVC框架的工程才能使用
- 拦截器只会拦截访问的控制器方法， 如果访问的是jsp/html/css/image/js是不会进行拦截的

> 自定义拦截器

那如何实现拦截器呢？

想要自定义拦截器，必须实现 HandlerInterceptor 接口。

1、新建一个Moudule ， springmvc-07-Interceptor  ， 添加web支持

2、配置web.xml 和 springmvc-servlet.xml 文件

3、编写一个拦截器

```java
package com.kuang.interceptor;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyInterceptor implements HandlerInterceptor {

   //在请求处理的方法之前执行
   //如果返回true执行下一个拦截器
   //如果返回false就不执行下一个拦截器
   public boolean preHandle(HttpServletRequest httpServletRequest,HttpServletResponse httpServletResponse, Object o) throws Exception {
       System.out.println("------------处理前------------");
       return true;
  }

   //在请求处理方法执行之后执行
   public void postHandle(HttpServletRequest httpServletRequest,HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView)throws Exception {
       System.out.println("------------处理后------------");
  }

   //在dispatcherServlet处理后执行,做清理工作.
   public void afterCompletion(HttpServletRequest httpServletRequest,HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
       System.out.println("------------清理------------");
  }
}
```

4、在springmvc的配置文件中配置拦截器

```xml
<!--关于拦截器的配置-->
<mvc:interceptors>
   <mvc:interceptor>
       <!--/** 包括路径及其子路径-->
       <!--/admin/* 拦截的是/admin/add等等这种 , /admin/add/user不会被拦截-->
       <!--/admin/** 拦截的是/admin/下的所有-->
       <mvc:mapping path="/**"/>
       <!--bean配置的就是拦截器-->
       <bean class="com.kuang.interceptor.MyInterceptor"/>
   </mvc:interceptor>
</mvc:interceptors>
```

5、编写一个Controller，接收请求

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

//测试拦截器的控制器
@Controller
public class InterceptorController {

   @RequestMapping("/interceptor")
   @ResponseBody
   public String testFunction() {
       System.out.println("控制器中的方法执行了");
       return "hello";
  }
}
```

6、前端 index.jsp

```jsp
<a href="${pageContext.request.contextPath}/interceptor">拦截器测试</a>
```

7、启动tomcat 测试一下！



![img](https://gitee.com/xudongyin/img/raw/master/img/20200719181456)

> 验证用户是否登录 (认证用户)

**实现思路**

1、有一个登陆页面，需要写一个controller访问页面。

2、登陆页面有一提交表单的动作。需要在controller中处理。判断用户名密码是否正确。如果正确，向session中写入用户信息。*返回登陆成功。*

3、**拦截用户请求，判断用户是否登陆。如果用户已经登陆。放行， 如果用户未登陆，跳转到登陆页面**

**测试：**

1、编写一个登陆页面  login.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Title</title>
    </head>

    <h1>登录页面</h1>
    <hr>

    <body>
        <form action="${pageContext.request.contextPath}/user/login">
            用户名：<input type="text" name="username"> <br>
            密码：<input type="password" name="pwd"> <br>
            <input type="submit" value="提交">
        </form>
    </body>
</html>
```

2、编写一个Controller处理请求

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpSession;

@Controller
@RequestMapping("/user")
public class UserController {

   //跳转到登陆页面
   @RequestMapping("/jumplogin")
   public String jumpLogin() throws Exception {
       return "login";
  }

   //跳转到成功页面
   @RequestMapping("/jumpSuccess")
   public String jumpSuccess() throws Exception {
       return "success";
  }

   //登陆提交
   @RequestMapping("/login")
   public String login(HttpSession session, String username, String pwd) throwsException {
       // 向session记录用户身份信息
       System.out.println("接收前端==="+username);
       session.setAttribute("user", username);
       return "success";
  }

   //退出登陆
   @RequestMapping("logout")
   public String logout(HttpSession session) throws Exception {
       // session 过期
       session.invalidate();
       //移除节点数据
       //session.removeAttribute("user");
       return "login";
  }
}
```

3、编写一个登陆成功的页面 success.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Title</title>
    </head>
    <body>

        <h1>登录成功页面</h1>
        <hr>

        ${user}
        <a href="${pageContext.request.contextPath}/user/logout">注销</a>
    </body>
</html>
```

4、在 index 页面上测试跳转！启动Tomcat 测试，未登录也可以进入主页！

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>$Title$</title>
    </head>
    <body>
        <h1>首页</h1>
        <hr>
        <%--登录--%>
        <a href="${pageContext.request.contextPath}/user/jumplogin">登录</a>
        <a href="${pageContext.request.contextPath}/user/jumpSuccess">成功页面</a>
    </body>
</html>
```

5、编写用户登录拦截器

```java
package com.kuang.interceptor;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class LoginInterceptor implements HandlerInterceptor {

    public boolean preHandle(HttpServletRequest request, HttpServletResponseresponse, Object handler) throws ServletException, IOException {
        // 如果是登陆页面则放行
        System.out.println("uri: " + request.getRequestURI());
        if (request.getRequestURI().contains("login")) {
            return true;
        }

        HttpSession session = request.getSession();

        // 如果用户已登陆也放行
        if(session.getAttribute("user") != null) {
            return true;
        }

        // 用户没有登陆跳转到登陆页面
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request,response);
        return false;
    }

    public void postHandle(HttpServletRequest httpServletRequest,HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView)throws Exception {

    }

    public void afterCompletion(HttpServletRequest httpServletRequest,HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {

    }
}
```

6、在Springmvc的配置文件中注册拦截器

```xml
<!--关于拦截器的配置-->
<mvc:interceptors>
   <mvc:interceptor>
       <mvc:mapping path="/**"/>
       <bean id="loginInterceptor" class="com.kuang.interceptor.LoginInterceptor"/>
   </mvc:interceptor>
</mvc:interceptors>
```

7、再次重启Tomcat测试！

**OK，测试登录拦截功能无误.**



## Spring MVC 数据绑定

数据绑定：在后端的业务方法中直接获取客户端 HTTP 请求中的参数，将请求参数映射到业务方法的形参中，Spring MVC 中数据绑定的工作是由 HandlerAdapter 来完成的。

- 基本数据类型

```java
@RequestMapping("/baseType")
@ResponseBody
public String baseType(int id){
    return id+"";
}
```

**@ResponseBody 表示 Spring MVC 会直接将业务方法的返回值响应给客户端，如果不加 @ResponseBody 注解，Spring MVC 会将业务方法的放回值传递给 DispatcherServlet，再由 DisptacherServlet 调用 ViewResolver 对返回值进行解析，映射到一个 JSP 资源。（这里没有指定JSP，所以去掉的话就会找不到页面）**

- 包装类

```java
@RequestMapping("/packageType")
@ResponseBody
public String packageType(@RequestParam(value = "num",required = false,defaultValue = "0") Integer id){
    return id+"";
}
```

包装类可以接收 null，当 HTTP 请求没有参数时，使用包装类定义形参的数据类型，程序不会抛出异常。

@RequestParam

value = "num"：将 HTTP 请求中名为 num 的参数赋给形参 id。

requried：设置 num 是否为必填项，true 表示必填，false 表示非必填，可省略。

defaultValue = “0”：如果 HTTP 请求中没有 num 参数，默认值为0.

- 数组

```java
@RestController
@RequestMapping("/data")
public class DataBindHandler {
    @RequestMapping("/array")
    public String array(String[] name){
        String str = Arrays.toString(name);
        return str;
    }
}
```

**@RestController 表示该控制器会直接将业务方法的返回值响应给客户端，不进行视图解析。**

**@Controller 表示该控制器的每一个业务方法的返回值都会交给视图解析器进行解析，如果只需要将数据响应给客户端，而不需要进行视图解析，则需要在对应的业务方法定义处添加 @ResponseBody。**

```java
@RestController
@RequestMapping("/data")
public class DataBindHandler {
    @RequestMapping("/array")
    public String array(String[] name){
        String str = Arrays.toString(name);
        return str;
    }
}
```

等同于

```java
@Controller
@RequestMapping("/data")
public class DataBindHandler {
    @RequestMapping("/array")
    @ResponseBody
    public String array(String[] name){
        String str = Arrays.toString(name);
        return str;
    }
}
```

- List

Spring MVC 不支持 List 类型的直接转换，需要对 List 集合进行包装。

集合封装类

```java
package com.southwind.entity;

import lombok.Data;

import java.util.List;

@Data
public class UserList {
    private List<User> users;
}
```

JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-14
  Time: 09:12
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/data/list" method="post">
        用户1编号：<input type="text" name="users[0].id"/><br/>
        用户1名称：<input type="text" name="users[0].name"/><br/>
        用户2编号：<input type="text" name="users[1].id"/><br/>
        用户2名称：<input type="text" name="users[1].name"/><br/>
        用户3编号：<input type="text" name="users[2].id"/><br/>
        用户3名称：<input type="text" name="users[2].name"/><br/>
        <input type="submit" value="提交"/>
    </form>
</body>
</html>
```

业务方法

```java
@RequestMapping("/list")
public String list(UserList userList){
    StringBuffer str = new StringBuffer();
    for(User user:userList.getUsers()){
        str.append(user);
    }
    return str.toString();
}
```

处理 @ResponseBody 中文乱码，在 springmvc.xml 中配置消息转换器。

~~~xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
~~~

==register-defaults="Boolean"  是否添加其他默认的HttpMessageConverter注册，消息转化器默认总共有七个==

- Map

自定义封装类

```java
package com.southwind.entity;

import lombok.Data;

import java.util.Map;

@Data
public class UserMap {
    private Map<String,User> users;
}
```

业务方法

```java
@RequestMapping("/map")
public String map(UserMap userMap){
    StringBuffer str = new StringBuffer();
    for(String key:userMap.getUsers().keySet()){
        User user = userMap.getUsers().get(key);
        str.append(user);
    }
    return str.toString();
}
```

JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-14
  Time: 09:12
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/data/map" method="post">
        用户1编号：<input type="text" name="users['a'].id"/><br/>
        用户1名称：<input type="text" name="users['a'].name"/><br/>
        用户2编号：<input type="text" name="users['b'].id"/><br/>
        用户2名称：<input type="text" name="users['b'].name"/><br/>
        用户3编号：<input type="text" name="users['c'].id"/><br/>
        用户3名称：<input type="text" name="users['c'].name"/><br/>
        <input type="submit" value="提交"/>
    </form>
</body>
</html>
```

- JSON

客户端发生 JSON 格式的数据，直接通过 Spring MVC 绑定到业务方法的形参中。

处理 Spring MVC 无法加载静态资源的问题(被 ==dispatcherServlet== 拦截)，在 web.xml 中添加配置即可。

```xml
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.js</url-pattern>
</servlet-mapping>
```

JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-14
  Time: 10:35
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="js/jquery-3.3.1.min.js"></script>
    <!--这里引用了jquery文件，需要自己去找下载-->
    <script type="text/javascript">
        $(function(){
           var user = {
               "id":1,
               "name":"张三"
           };
           $.ajax({
               url:"/data/json",  
               data:JSON.stringify(user),   <这里进入到handler里的业务方法>
               type:"POST",
               contentType:"application/json;charset=UTF-8",
               dataType:"JSON",
               success:function(data){
                   alert(data.id+"---"+data.name);
               }
           })
        });
    </script>
</head>
<body>

</body>
</html>
```

业务方法

```java
@RequestMapping("/json")
public User json(@RequestBody User user){
    System.out.println(user);
    user.setId(6);
    user.setName("张六");
    return user;
}
```

Spring MVC 中的 JSON 和 JavaBean 的转换需要借助于 fastjson，pom.xml 引入相关依赖。

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.32</version>
</dependency>
```

springmvc.xml 添加 fastjson 配置。

```xml
<mvc:annotation-driven>
    <!-- 消息转换器 -->
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
        </bean>
        <!-- 配置fastjson -->
        <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter4"></bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```



## Spring MVC 模型数据解析

JSP 四大作用域对应的内置对象：pageContext、request、session、application。

模型数据的绑定是由 ViewResolver 来完成的，实际开发中，我们需要先添加模型数据，再交给 ViewResolver 来绑定。

Spring MVC 提供了以下几种方式添加模型数据：

- Map
- Model
- ModelAndView
- HttpServletRequest
- @SessionAttribute
- @ModelAttribute

> 将模式数据绑定到 request 对象。

1、Map

```java
@RequestMapping("/map")
public String map(Map<String,User> map){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    map.put("a",user);   
    //这里把key值改为“1”的话，jsp调用的时候不能调用，只能是字符串
    return "view";
}
```

JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-14
  Time: 11:36
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>  
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${requestScope.a}     //这里requestScope. 后面只能是字符串，不能是数字
</body>
</html>
```

==<%@ page isELIgnored="false" %>==  不写这行代码，浏览器会将  ${requestScope.user} 作为文本输出，而不会解析

2、Model

   pom.xml

~~~xml
 <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
 </dependency>
~~~

  Handler

```java
@RequestMapping("/model")
public String model(Model model){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    model.addAttribute("user",user);
    return "view";
}
```

3、ModelAndView

```java
@RequestMapping("/modelAndView")
public ModelAndView modelAndView(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("user",user);
    modelAndView.setViewName("view");
    return modelAndView;
}

@RequestMapping("/modelAndView2")
public ModelAndView modelAndView2(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("user",user);
    View view = new InternalResourceView("/view.jsp");
    modelAndView.setView(view);
    return modelAndView;
}

@RequestMapping("/modelAndView3")
public ModelAndView modelAndView3(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    ModelAndView modelAndView = new ModelAndView("view");
    modelAndView.addObject("user",user);
    return modelAndView;
}

@RequestMapping("/modelAndView4")
public ModelAndView modelAndView4(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    View view = new InternalResourceView("/view.jsp");
    ModelAndView modelAndView = new ModelAndView(view);
    modelAndView.addObject("user",user);
    return modelAndView;
}

@RequestMapping("/modelAndView5")
public ModelAndView modelAndView5(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    Map<String,User> map = new HashMap<>();
    map.put("user",user);
    ModelAndView modelAndView = new ModelAndView("view",map);
    return modelAndView;
}

@RequestMapping("/modelAndView6")
public ModelAndView modelAndView6(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    Map<String,User> map = new HashMap<>();
    map.put("user",user);
    View view = new InternalResourceView("/view.jsp");
    ModelAndView modelAndView = new ModelAndView(view,map);
    return modelAndView;
}

@RequestMapping("/modelAndView7")
public ModelAndView modelAndView7(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    ModelAndView modelAndView = new ModelAndView("view","user",user);
    return modelAndView;
}

@RequestMapping("/modelAndView8")
public ModelAndView modelAndView8(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    View view = new InternalResourceView("/view.jsp");
    ModelAndView modelAndView = new ModelAndView(view,"user",user);
    return modelAndView;
}
```

4、HttpServletRequest

```java
@RequestMapping("/request")
public String request(HttpServletRequest request){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    request.setAttribute("user",user);
    return "view";
}
```

5、@ModelAttribute

- 定义一个方法，该方法专门用来返回要填充到模型数据中的对象。

```java
@ModelAttribute
//返回一个user对象，交给框架自己去处理
public User getUser(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    return user;
}
```

```java
@ModelAttribute
//把对象装map里，也就是自己处理，不用返回对象
public void getUser(Map<String,User> map){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    map.put("user",user);
}
```

```java
@ModelAttribute
//把对象装model里，也就是自己处理，不用返回对象
public void getUser(Model model){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    model.addAttribute("user",user);
}
```

- 业务方法中无需再处理模型数据，只需返回视图即可。

```java
@RequestMapping("/modelAttribute")
public String modelAttribute(){
    return "view";
}
```

> 将模型数据绑定到 session 对象

1、直接使用原生的 Servlet API。

```java
@RequestMapping("/session")
public String session(HttpServletRequest request){
    HttpSession session = request.getSession();
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    session.setAttribute("user",user);
    return "view";
}
//或者这个方法也一样
@RequestMapping("/session2")
public String session2(HttpSession session){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    session.setAttribute("user",user);
    return "view";
}
```

2、@SessionAttribute（==慎用==，全局注解，对所有方法都生效，注解在Handler类上）

```java
@SessionAttributes(value = {"user","address"})
public class ViewHandler {
}
```

对于 ViewHandler 中的所有业务方法，只要向 request 中添加了 key = "user"、key = "address" 的对象时，Spring MVC 会自动将该数据添加到 session 中，保存 key 不变。

```java
@SessionAttributes(types = {User.class,Address.class})
public class ViewHandler {
}
```

对于 ViewHandler 中的所有业务方法，只要向 request 中添加了数据类型是 User 、Address 的对象时，Spring MVC 会自动将该数据添加到 session 中，保存 key 不变。

> 将模型数据绑定到 application 对象

```java
@RequestMapping("/application")
public String application(HttpServletRequest request){
    ServletContext application = request.getServletContext();
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    application.setAttribute("user",user);
    return "view";
}
```



## Spring MVC 自定义数据转换器

数据转换器是指将客户端 HTTP 请求中的参数转换为业务方法中定义的形参，自定义表示开发者可以自主设计转换的方式，HandlerApdter 已经提供了通用的转换，String 转 int，String 转 double，表单数据的封装等，但是在特殊的业务场景下，HandlerAdapter 无法进行转换，就需要开发者自定义转换器。

客户端输入 String 类型的数据 "2019-03-03"，自定义转换器将该数据转为 Date 类型的对象。

- 创建 DateConverter 转换器，实现 Conveter 接口。

```java
package com.southwind.converter;

import org.springframework.core.convert.converter.Converter;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateConverter implements Converter<String, Date> {

    private String pattern;

    public DateConverter(String pattern){
        this.pattern = pattern;  //这里pattern是"yyyy-MM-dd"
    }

    @Override
    public Date convert(String s) {
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(this.pattern);
        //创建对应"yyyy-MM-dd"的日期格式
        Date date = null;
        try {
            date = simpleDateFormat.parse(s);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return date;
    }
}
```

- springmvc.xml 配置转换器。

```xml
<!-- 配置自定义转换器 -->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <bean class="com.southwind.converter.DateConverter">
                <constructor-arg type="java.lang.String" value="yyyy-MM-dd"></constructor-arg>
            <!--这里有参构造是给DateConverter的有参构造传"yyyy-MM-dd"的日期格式-->
            </bean>
        </list>
    </property>
</bean>
<!--conversion-service="conversionService" 注册一个转化器 -->
<mvc:annotation-driven conversion-service="conversionService">
    <!-- 消息转换器 -->
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
        </bean>
        <!-- 配置fastjson -->
        <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter4"></bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

- JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-14
  Time: 14:47
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/converter/date" method="post">
        请输入日期:<input type="text" name="date"/>(yyyy-MM-dd)<br/>  
   <!--这里的name="date"与Handler的public String date(Date date)的参数date一致-->
        <input type="submit" value="提交"/>
    </form>
</body>
</html>
```

- Handler

```java
package com.southwind.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;

@RestController
@RequestMapping("/converter")
public class ConverterHandler {

    @RequestMapping("/date")
    public String date(Date date){
        return date.toString();
    }
}
```

String 转 Student

StudentConverter

```java
package com.southwind.converter;

import com.southwind.entity.Student;
import org.springframework.core.convert.converter.Converter;

public class StudentConverter implements Converter<String, Student> {
    @Override
    public Student convert(String s) {
        String[] args = s.split("-");
        Student student = new Student();
        student.setId(Long.parseLong(args[0]));
        student.setName(args[1]);
        student.setAge(Integer.parseInt(args[2]));
        return student;
    }
}
```

springmvc.xml

```xml
<!-- 配置自定义转换器 -->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <bean class="com.southwind.converter.DateConverter">
                <constructor-arg type="java.lang.String" value="yyyy-MM-dd"></constructor-arg>
            </bean>
            <bean class="com.southwind.converter.StudentConverter"></bean>
        </list>
    </property>
</bean>

<mvc:annotation-driven conversion-service="conversionService">
    <!-- 消息转换器 -->
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
        </bean>
        <!-- 配置fastjson -->
        <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter4"></bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-14
  Time: 15:23
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/converter/student" method="post">
        请输入学生信息：<input type="text" name="student"/>(id-name-age)<br/>
        <input type="submit" value="提交"/>
    </form>
</body>
</html>
```

Handler

```java
@RequestMapping("/student")
public String student(Student student){
    return student.toString();
}
```



## Spring MVC 文件上传下载

> 单文件上传

底层是使用 Apache fileupload 组件完成上传，Spring MVC 对这种方式进行了封装。

- pom.xml

```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.5</version>
</dependency>

<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.3</version>
</dependency>
```

- JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-15
  Time: 09:09
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/file/upload" method="post" enctype="multipart/form-data">
        <input type="file" name="img"/>
        <input type="submit" value="上传"/>
    </form>
    <img src="${path}">
</body>
</html>
```

1、input 的 type 设置为 file。

2、form 的 method 设置为 post（get 请求只能将文件名传给服务器）

3、from 的 enctype 设置为 multipart-form-data（如果不设置只能将文件名传给服务器）

- springmvc.xml

```xml
<!-- 配置上传组件 -->
<!--这个bena的id必须为：multipartResolver ，否则上传文件会报400的错误！-->
<bean id="multipartResolver"  class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 下面几个属性可以不配置，会有默认值 -->
   <!-- 请求的编码格式，必须和jSP的pageEncoding属性一致，以便正确读取表单的内容，默认为ISO-8859-1 -->
   <property name="defaultEncoding" value="utf-8"/>
   <!-- 上传文件大小上限，单位为字节（10485760=10M） -->
   <property name="maxUploadSize" value="10485760"/>
   <property name="maxInMemorySize" value="40960"/>
</bean>
```

- Handler

```java
package com.southwind.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.io.IOException;

@Controller
@RequestMapping("/file")
public class FileHandler {

    @PostMapping("/upload")
    public String upload(MultipartFile img, HttpServletRequest request){
        if(img.getSize()>0){
            //获取保存上传文件的file路径
            String path = request.getServletContext().getRealPath("file");
            //这里可以做一个目录存在否判断
       //    File realPath = new File(path);
      //     if (!realPath.exists()){
       //         realPath.mkdir();
         //   }
            //获取上传的文件名
            String name = img.getOriginalFilename(); 
            File file = new File(path,name);
            try {
                img.transferTo(file);
                //保存上传之后的文件路径
                request.setAttribute("path","/file/"+name);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return "upload";
    }
}
```

CommonsMultipartFile 的 常用方法：

- **String getOriginalFilename()：获取上传文件的原名**
- **InputStream getInputStream()：获取文件流**
- **void transferTo(File dest)：将上传文件保存到一个目录文件中**

- web.xml 添加如下配置，否则客户端无法访问 png

```xml
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.png</url-pattern>
</servlet-mapping>
```

> 多文件上传

pom.xml

```xml
<!-- 标准表单标签库依赖 -->
<dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>

<dependency>
    <groupId>taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
</dependency>
```

JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-15
  Time: 09:32
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/file/uploads" method="post" enctype="multipart/form-data">
        file1:<input type="file" name="imgs"/><br/>
        file2:<input type="file" name="imgs"/><br/>
        file3:<input type="file" name="imgs"><br/>
        <input type="submit" value="上传"/>
    </form>
    <c:forEach items="${files}" var="file" >
        <img src="${file}" width="300px">
    </c:forEach>
</body>
</html>
```

Handler

```java
@PostMapping("/uploads")
public String uploads(MultipartFile[] imgs,HttpServletRequest request){
    List<String> files = new ArrayList<>();//用来存储上传文件名
    for (MultipartFile img:imgs){
        if(img.getSize()>0){
            //获取保存上传文件的file路径
            String path = request.getServletContext().getRealPath("file");
            //获取上传的文件名
            String name = img.getOriginalFilename();
            File file = new File(path,name);  
            try {
                img.transferTo(file);
                //保存上传之后的文件路径
                files.add("/file/"+name); 
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    request.setAttribute("files",files);
    return "uploads";
}
```



> 下载

> Rest格式

- JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-15
  Time: 10:36
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <a href="/file/download/1">1.png</a>
    <a href="/file/download/2">2.png</a>
    <a href="/file/download/3">3.png</a>
</body>
</html>
```

- Handler

```java
@GetMapping("/download/{name}")    
//这里是rest格式URL，所以下面需要@PathVariable("name")来映射取值
public void download(@PathVariable("name") String name, HttpServletRequest request, HttpServletResponse response){
    if(name != null){
        name += ".png";
        String path = request.getServletContext().getRealPath("file");
        File file = new File(path,name);
        OutputStream outputStream = null;
        if(file.exists()){
            response.setContentType("application/forc-download");  //设置为下载类型
            response.setHeader("Content-Disposition","attachment;filename="+name);
            //告诉浏览器文件的名字和类型，Content-Disposition中指定的类型是文件的扩展名，并且弹出的下载对话框中的文件类型图片是按照文件的扩展名显示的，点保存后，文件以filename的值命名，保存类型以Content中设置的为准。  注意：在设置Content-Disposition头字段之前，一定要设置Content-Type头字段。
            try {
                outputStream = response.getOutputStream();
                outputStream.write(FileUtils.readFileToByteArray(file));
                outputStream.flush();
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                if(outputStream != null){
                    try {
                        outputStream.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}
```

> 普通方式

- JSP

~~~jsp
<a href="/download">点击下载</a>
~~~

- Handler

~~~java
@RequestMapping(value="/download")
public String downloads(HttpServletResponse response ,HttpServletRequest request) throws Exception{
   //要下载的图片地址
   String  path = request.getServletContext().getRealPath("/upload");
   String  fileName = "基础语法.jpg";

   //1、设置response 响应头
   response.reset(); //设置页面不缓存,清空buffer
   response.setCharacterEncoding("UTF-8"); //字符编码
   response.setContentType("multipart/form-data"); //二进制传输数据
   //设置响应头
   response.setHeader("Content-Disposition",
           "attachment;fileName="+URLEncoder.encode(fileName, "UTF-8"));

   File file = new File(path,fileName);
   //2、 读取文件--输入流
   InputStream input=new FileInputStream(file);
   //3、 写出文件--输出流
   OutputStream out = response.getOutputStream();

   byte[] buff =new byte[1024];
   int index=0;
   //4、执行 写出操作
   while((index= input.read(buff))!= -1){
       out.write(buff, 0, index);
       out.flush();
  }
   out.close();
   input.close();
   return null;
}
~~~





## Spring MVC 表单标签库

- Handler

```java
@GetMapping("/get")
public ModelAndView get(){
    ModelAndView modelAndView = new ModelAndView("tag");
    Student student = new Student(1L,"张三",22);
    modelAndView.addObject("student",student);
    return modelAndView;
}
```

- JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-15
  Time: 10:53
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>学生信息</h1>
    <form:form modelAttribute="student">
       <!--第一个form是<%@ taglib prefix="form" %>的form，第二个form才是form标签-->
        学生ID：<form:input path="id"/><br/>
        学生姓名：<form:input path="name"/><br/>
        学生年龄：<form:input path="age"/><br/>
        <input type="submit" value="提交"/>
    </form:form>
</body>
</html>
```

1、JSP 页面导入 Spring MVC 表单标签库，与导入 JSTL 标签库的语法非常相似，前缀 prefix 可以自定义，通常定义为 from。

```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
```

2、将 form 表单与模型数据进行绑定，通过 modelAttribute 属性完成绑定，将 modelAttribute 的值设置为模型数据对应的 key 值。

```java
Handeler:  modelAndView.addObject("student",student);
JSP:  <form:form modelAttribute="student">
```

3、form 表单完成绑定之后，将模型数据的值取出绑定到不同的标签中，通过设置标签的 path 属性完成，将 path 属性的值设置为模型数据对应的属性名即可。

```jsp
学生ID：<form:input path="id"/><br/>
学生姓名：<form:input path="name"/><br/>
学生年龄：<form:input path="age"/><br/>
```

### 常用的表单标签

- form

```jsp
<form:form modelAttribute="student"/>
```

渲染的是 HTML 中的`<form></from>`，通过 modelAttribute 属性绑定具体的模型数据。

- input

```jsp
<form:input path="name"/>
```

渲染的是 HTML 中的 ==`<input type="text"/>`==(文本类型的input标签)，form 标签绑定的是模型数据，input 标签绑定的是模型数据中的属性值，通过 path 属性可以与模型数据中的属性名对应，并且支持及联操作。

```jsp
<form:input path="address.name"/>
```

- password

```jsp
<form:password path="password"/>
```

渲染的是 HTML 中的 `<input type="password"/>`，通过 path 属性与模型数据的属性值进行绑定，password 标签的值不会在页面显示。

- checkbox

```jsp
<form:checkbox path="hobby" value="读书"/>
```

```java
student.setFlag(false);
```

```jsp
checkbox：<form:checkbox path="flag" value="flag"></form:checkbox><br/> 
```

上面代码行的 ==value="flag"==是设置表单提交后这个CheckBox标签的value值，不关乎这个CheckBox的勾选状态

渲染的是 HTML 中的 `<input type="checkbox"/>`，通过 path 与模型数据的属性值进行绑定，可以绑定 boolean、数组和集合。

如果绑定 boolean 值，若该变量的值为 true，则表示该复选框选中，否则表示不选中。

==如果绑定数组或者集合，数组/集合中的元素等于 checkbox 的 value 值，则选中。==

```java
student.setHobby(Arrays.asList("读书","看电影","玩游戏"));
modelAndView.addObject("student",student);
```

```jsp
爱好：<form:checkbox path="hobby" value="摄影"></form:checkbox>摄影<br/>
<form:checkbox path="hobby" value="读书"></form:checkbox>读书<br/>
<form:checkbox path="hobby" value="听音乐"></form:checkbox>听音乐<br/>
<form:checkbox path="hobby" value="看电影"></form:checkbox>看电影<br/>
<form:checkbox path="hobby" value="旅游"></form:checkbox>旅游<br/>
<form:checkbox path="hobby" value="玩游戏"></form:checkbox>玩游戏<br/>
<input type="submit" value="提交"/>
```

- checkboxes

```jsp
<form:checkboxes items=${student.hobby} path="selecHobby"/>
```

渲染的是 HTML 中的一组 `<input type="checkbox"/>`，是对 `<form:checkbox/>` 的一种简化，需要结合 items 和 path 属性来使用，items 绑定被遍历的集合或数组，path 绑定被选中的集合或数组，可以这样理解，items 为全部可选集合，path 为默认的选中集合 (student 选择的 hobby 的数据)。

```java
student.setHobby(Arrays.asList("摄影","读书","听音乐","看电影","旅游","玩游戏"));
student.setSelectHobby(Arrays.asList("摄影","读书","听音乐"));
modelAndView.addObject("student",student);
```

```jsp
爱好：<form:checkboxes path="selectHobby" items="${student.hobby}"/><br/>
<!-- items="${student.hobby}" 只能这样写，规定-->
```

需要注意的是 path 可以直接绑定模型数据的属性值，items 则需要通过 EL 表达式的形式从域对象中获取数据，不能直接写属性名。==items="${student.hobby}" 只能这样写，规定==

- rad-iobutton

```jsp
<form:radiobutton path="radioId" value="0"/>
```

渲染的是 HTML 中的一个 `<input type="radio"/>`，绑定的数据与标签的 value 值相等则为选中，否则不选中。

```java
student.setRadioId(1);
modelAndView.addObject("student",student);
```

```jsp
radiobutton:<form:radiobutton path="radioId" value="1"/>radiobutton<br/>
```

- radiobuttons

```jsp
<form:radiobuttons itmes="${student.grade}" path="selectGrade"/>
```

渲染的是 HTML 中的一组 `<input type="radio"/>`，这里需要结合 items 和 path 两个属性来使用，items 绑定被遍历的集合或数组，path 绑定被选中的值，items 为全部的可选类型，path 为默认选中的选项，用法与 `<form:checkboxes/>` 一致。

```java
Map<Integer,String> gradeMap = new HashMap<>();
gradeMap.put(1,"一年级");
gradeMap.put(2,"二年级");
gradeMap.put(3,"三年级");
gradeMap.put(4,"四年级");
gradeMap.put(5,"五年级");
gradeMap.put(6,"六年级");
student.setGradeMap(gradeMap);
student.setSelectGrade(3);
modelAndView.addObject("student",student);
```

```jsp
学生年级：<form:radiobuttons items="${student.gradeMap}" path="selectGrade"/><br/>
```

- select

```jsp
<form:select items="${student.citys}" path="selectCity"/>
```

渲染的是 HTML 中的一个 `<select/>` 标签，需要结合 items 和 path 两个属性来使用，items 绑定被遍历的集合或数组，path 绑定被选中的值，用法与 `<from:radiobuttons/>`一致。

```java
Map<Integer,String> cityMap = new HashMap<>();
cityMap.put(1,"北京");
cityMap.put(2,"上海");
cityMap.put(3,"广州");
cityMap.put(4,"深圳");
student.setCityMap(cityMap);
student.setSelectCity(3);
modelAndView.addObject("student",student);
```

```jsp
所在城市：<form:select items="${student.cityMap}" path="selectCity"></form:select><br/>
```

- options

`form:select` 结合 `form:options` 的使用，`from:select` 只定义 path 属性，在 `form:select` 标签内部添加一个子标签 `form:options` ，设置 items 属性，获取被遍历的集合。

```jsp
所在城市：<form:select path="selectCity">
              <form:options items="${student.cityMap}"></form:options>
          </form:select><br/>
```

- option

  `form:select` 结合 `form:option` 的使用，`from:select` 定义 path 属性，给每一个 `form:option` 设置 value 值，path 的值与哪个 value 值相等，该项默认选中。

```jsp
所在城市：<form:select path="selectCity">
            <form:option value="1">杭州</form:option>
            <form:option value="2">成都</form:option>
            <form:option value="3">西安</form:option>
        </form:select><br/>
```

- textarea

渲染的是 HTML 中的一个 `<textarea/>` ，path 绑定模型数据的属性值，作为文本输入域的默认值。

```java
student.setIntroduce("你好，我是...");
modelAndView.addObject("student",student);
```

```jsp
信息：<form:textarea path="introduce"/><br/>
```

- errors

处理错误信息，一般用在数据校验，该标签需要结合 Spring MVC 的验证器结合起来使用。

## Spring MVC 数据校验

Spring MVC 提供了两种数据校验的方式：1、基于 Validator 接口。2、使用 Annotation JSR - 303 标准进行校验。

基于 Validator 接口的方式需要自定义 Validator 验证器，每一条数据的验证规则需要开发者手动完成，使用 Annotation JSR - 303 标准则不需要自定义验证器，通过注解的方式可以直接在实体类中添加每个属性的验证规则，这种方式更加方便，实际开发中推荐使用。

### 基于 Validator 接口

- 实体类 Account

```java
package com.southwind.entity;

import lombok.Data;

@Data
public class Account {
    private String name;
    private String password;
}
```

- 自定义验证器 AccountValidator，实现 Validator 接口。

```java
package com.southwind.validator;

import com.southwind.entity.Account;
import org.springframework.validation.Errors;
import org.springframework.validation.ValidationUtils;
import org.springframework.validation.Validator;

public class AccountValidator implements Validator {
    @Override
    public boolean supports(Class<?> aClass) {
        return Account.class.equals(aClass);
    }

    @Override
    public void validate(Object o, Errors errors) {
        ValidationUtils.rejectIfEmpty(errors,"name",null,"姓名不能为空");
        ValidationUtils.rejectIfEmpty(errors,"password",null,"密码不能为空");
    }
}
```

- 控制器

```java
package com.southwind.controller;

import com.southwind.entity.Account;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/validator")
public class ValidatorHandler {

    @GetMapping("/login")
    public String login(Model model){
        model.addAttribute("account",new Account());
        return "login";
    }

    @PostMapping("/login")
    public String login(@Validated Account account, BindingResult bindingResult){
       if(bindingResult.hasErrors()){
            return "login";
        }
        return "index";
    }
}
```

- springmvc.xml 配置验证器。

```xml
<bean id="accountValidator" class="com.southwind.validator.AccountValidator"></bean>
<mvc:annotation-driven validator="accountValidator"></mvc:annotation-driven>
```

- JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-18
  Time: 10:31
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form:form modelAttribute="account" action="/validator/login" method="post">
        姓名：<form:input path="name"/><form:errors path="name"></form:errors><br/>
        密码：<form:input path="password"/><form:errors path="password"></form:errors><br/>
        <input type="submit" value="登录"/>
    </form:form>
</body>
</html>
```



### Annotation JSR - 303 标准

使用 Annotation JSR - 303 标准进行验证，需要导入支持这种标准的依赖 jar 文件，这里我们使用 Hibernate Validator。

- pom.xml       

```xml
<!-- JSR-303 -->
<dependency>
  <groupId>org.hibernate</groupId>
  <artifactId>hibernate-validator</artifactId>
  <version>5.3.6.Final</version>
</dependency>

<dependency>
  <groupId>javax.validation</groupId>
  <artifactId>validation-api</artifactId>
  <version>2.0.1.Final</version>
</dependency>

<dependency>
  <groupId>org.jboss.logging</groupId>
  <artifactId>jboss-logging</artifactId>
  <version>3.3.2.Final</version>
</dependency>
```

- 通过注解的方式直接在实体类中添加相关的验证规则。

```java
package com.southwind.entity;

import lombok.Data;
import org.hibernate.validator.constraints.Email;
import org.hibernate.validator.constraints.NotEmpty;
import javax.validation.constraints.Pattern;
import javax.validation.constraints.Size;

@Data
public class Person {
    @NotEmpty(message = "用户名不能为空")
    private String username;
    @Size(min = 6,max = 12,message = "密码6-12位")
    private String password;
    @Email(regexp = "^[a-zA-Z0-9_.-]+@[a-zA-Z0-9-]+(\\\\.[a-zA-Z0-9-]+)*\\\\.[a-zA-Z0-9]{2,6}$",message = "请输入正确的邮箱格式")
    private String email;
    @Pattern(regexp = "^((13[0-9])|(14[5|7])|(15([0-3]|[5-9]))|(18[0,5-9]))\\\\\\\\d{8}$",message = "请输入正确的电话")
    private String phone;
}
```

- ValidatorHandler

```java
@GetMapping("/register")
public String register(Model model){
    model.addAttribute("person",new Person());
    return "register";
}

@PostMapping("/register")
public String register(@Valid Person person, BindingResult bindingResult){
    if(bindingResult.hasErrors()){
        return "register";
    }
    return "index";
}
```

- springmvc.xml

```xml
<mvc:annotation-driven />
```

这里的配置必须单独配，即使上面已经配过，不配会不能正常验证。

- JSP

```jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-18
  Time: 11:29
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form:form modelAttribute="person" action="/validator/register2" method="post">
        用户名：<form:input path="username"></form:input><form:errors path="username"/><br/>
        密码：<form:password path="password"></form:password><form:errors path="password"/><br/>
        邮箱：<form:input path="email"></form:input><form:errors path="email"/><br/>
        电话：<form:input path="phone"></form:input><form:errors path="phone"/><br/>
        <input type="submit" value="提交"/>
    </form:form>
</body>
</html>
```

校验规则详解：

@Null					被注解的元素必须为null

@NotNull				  被注解的元素不能为null

@Min(value)			     被注解的元素必须是一个数字，其值必须大于等于指定的最小值

@Max(value)			    被注解的元素必须是一个数字，其值必须小于于等于指定的最大值	

@Email				     被注解的元素必须是电子邮箱地址

@Pattern				  被注解的元素必须符合对应的正则表达式

@Length				   被注解的元素的大小必须在指定的范围内

@NotEmpty			      被注解的字符串的值必须非空

Null 和 Empty 是不同的结果，String str = null，str 是 null，String str = ""，str 不是 null，其值为空。



