### SSM框架整合

Spring + Spring MVC + MyBatis

Spring MVC 负责实现 MVC 设计模式，MyBatis 负责数据持久层，Spring 负责管理 Spring MVC 和 MyBatis 相关对象的创建和依赖注入。

#### 创建 Maven 工程，pom.xml ==(把建立项目就存在的 junit 单元测试依赖删掉)==

```xml
<dependencies>
  <!-- SpringMVC -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.0.11.RELEASE</version>
  </dependency>

  <!-- Spring JDBC -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.0.11.RELEASE</version>
  </dependency>

  <!-- Spring AOP -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.0.11.RELEASE</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.0.11.RELEASE</version>
  </dependency>

  <!-- MyBatis -->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.5</version>
  </dependency>

  <!-- MyBatis整合Spring -->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.1</version>
  </dependency>

  <!-- MySQL驱动 -->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.11</version>
  </dependency>

  <!-- C3P0 -->
  <dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.1.2</version>
  </dependency>

  <!-- JSTL -->
  <dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
  </dependency>

  <!-- ServletAPI -->
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
  </dependency>
  <!-- lombok -->
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.6</version>
    <scope>provided</scope>
  </dependency>
</dependencies>

<build>
    <!-- idea才需要写，eclipse不用 -->
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.xml</include>
        </includes>
      </resource>
      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>*.xml</include>
          <include>*.properties</include>
        </includes>
      </resource>
    </resources>
</build>
```

#### web.xml 中配置 SpringMVC、Spring、字符编码过滤器、加载静态资源。

==因为 mybatis 是应用与数据库的链接部件（之前学习 mybatis 时写的程序都是纯 java 应用程序），不属于 web 部分，所以不用在 web.xml 中配置==

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <!-- 启动Spring  管理全局，所以用<context-param>标签-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring.xml</param-value>
  </context-param>
  <listener><!-- 监听器，管理全局组件 mybatis 和 springMVC 等 -->
    <listener-class>
        org.springframework.web.context.ContextLoaderListener
      </listener-class>
  </listener>

  <!-- Spring MVC 管理MVC，所以配置在 <servlet> 中 -->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <!-- 局部框架组件被管理，所以用 <init-param>标签 -->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
  </servlet>
    
  <!-- 拦截所有请求 -->
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <!-- 字符编码过滤器 -->
  <filter>
    <filter-name>characterEncodingFilter</filter-name><!--可自定义-->
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name><!--上面自定义的名字相同-->
    <url-pattern>/*</url-pattern>
  </filter-mapping>
  
  <!-- 加载静态资源 -->
  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.js</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.css</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.jpg</url-pattern>
  </servlet-mapping>
</web-app>
```

#### 在 spring.xml 中配置 MyBatis 和 Spring 的整合。

==<bean>标签中如果 bean 需要被其他 bean 用到（会被注入到其他组件），就需要写上 id 属性。==

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
">

    <!-- 整合MyBatis ，数据库连接池的 bean -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test?useUnicode=true&amp;characterEncoding=UTF-8"></property>
        <property name="driverClass" value="com.mysql.cj.jdbc.Driver"></property>
        <!-- 初始化链接数 -->
        <property name="initialPoolSize" value="5"></property>
        <!-- 最大链接数 -->
        <property name="maxPoolSize" value="10"></property>
    </bean>

    <!-- 配置MyBatis SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--加入数据库连接池的 bean-->
        <property name="dataSource" ref="dataSource"></property>
        <property name="mapperLocations"              value="classpath:com/southwind/repository/*.xml"></property>
        <!-- 这样就不用去 config.xml 中去扫描 ~~~RepositoryMapper.xml -->
        <property name="configLocation" value="classpath:config.xml"></property>
    </bean>

 <!-- 扫描自定义的Mapper接口 ，然后创建各自接口的动态代理类,这样 repository 接口就不用写 @repository 注解-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.southwind.repository"></property>
    </bean>

</beans>
```

#### config.xml 配置一些 MyBatis 辅助信息，比如打印 SQL 等。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!-- 打印SQL-->
        <setting name="logImpl" value="STDOUT_LOGGING" />
    </settings>

    <typeAliases>
        <!-- 指定一个包名，MyBatis会在包名下搜索需要的JavaBean-->
        <!-- UserRepository.xml 中 resultType 和 paramType 就不用写完整的包名，直              接写类名就行 -->
        <package name="com.southwind.entity"/>
    </typeAliases>

</configuration>
```

#### 配置 springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd">

    <!-- 启动注解驱动 -->
    <mvc:annotation-driven></mvc:annotation-driven>

    <!-- 扫描业务代码 -->
    <context:component-scan base-package="com.southwind"></context:component-scan>

    <!-- 配置视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"></property><!-- "/"表示根目录 -->
        <property name="suffix" value=".jsp"></property>
    </bean>

</beans>
```

#### 实体类

```java
package com.southwind.entity;

import lombok.Data;

@Data
public class User {
    private long id;
    private String name;
    private String password;
    private double score;
}
```

#### UserRepository

```java
package com.southwind.repository;

import com.southwind.entity.User;

import java.util.List;
//本来实现这个接口的实现类应该加 @repository 注解，但是它已经交给IOC管理，会自动注入
public interface UserRepository {
    public List<User> findAll();
}
```

#### UserRepository.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.repository.UserRepository">
    <select id="findAll" resultType="User">
        select * from user
    </select>
</mapper>
```

#### UserService

```java
package com.southwind.service;

import com.southwind.entity.User;

import java.util.List;
//因为在视图添加对象的时候必须调用的是接口，所以才建立这个接口
public interface UserService {
    public List<User> findAll();
}
```

#### UserServiceImpl

```java
package com.southwind.service.impl;

import com.southwind.entity.User;
import com.southwind.repository.UserRepository;
import com.southwind.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserServiceImpl implements UserService {

  @Autowired
  private UserRepository userRepository;//多态，向上转型，这里实际是一个代理实现类

  @Override
  public List<User> findAll() {
        return userRepository.findAll();
    }
}
```

#### UserHandler

```java
package com.southwind.controller;

import com.southwind.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/user")
public class UserHandler {
    @Autowired
    private UserService userService;//多态，向上转型，

    @GetMapping("/findAll")
    public ModelAndView findAll(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("index");
        modelAndView.addObject("list",userService.findAll());
        //这里必须调用接口，所以才会建立 userService 接口
        return modelAndView;
    }
}
```

#### index.jsp

~~~jsp
<%--
  Created by IntelliJ IDEA.
  User: southwind
  Date: 2019-03-21
  Time: 09:55
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
    <c:forEach items="${list}" var="user">
        ${user.id}--${user.name}--${user.score}<br/>
    </c:forEach>
</body>
</html>

~~~

==@Service和@Repository放到实现类上面而不是接口类上面，接口只是一个规范，需要各种实现类去实现这个接口，我们要用的就是这些实类的方法。==

@Resource 的作用相当于 @Autowired，只不过 @Autowired 按 ==byType== 自动注入，而 @Resource默认按 ==byName== 自动注入罢了。

@Resource 有两个属性是比较重要的，分别是 name 和 type，spring 将 @Resource 注解的 name属性解析为 bean 的名字，而 type 属性则解析为 bean 的类型。所以如果使用 name 属性，则使用byName 的自动注入策略，而使用 type 属性时则使用 byType 自动注入策略。如果既不指定 name也不指定 type 属性，这时将通过反射机制使用 byName 自动注入策略。

@Resource装配顺序

如果同时指定了name 和 type，则从 Spring上下文 中找到唯一匹配的 bean 进行装配，找不到则抛出异常。
如果指定了name，则从上下文中查找名称（id 匹配的 bean 进行装配，找不到则抛出异常。)
如果指定了 type，则从上下文中找到类型匹配的唯一 bean 进行装配，找不到或者找到多个，都会抛出异常。
如果既没有指定 name，又没有指定 type，则自动按照 byName 方式进行装配，如果没有匹配，则回退为一个原始类型（UserDao）进行匹配，如果匹配则自动装配。


当指定了@service 的 name 值时， 在 @Resource 中要么不指示，如果指示的话，则要与之相对应。

当没有指定 @service 的 name 值时，在 @Resource 中随意。但是前提是，实现该接口的只有这一个类。

所以，建议是最好在 @service 和 @Resoure 中同时指定名称，并且做到一一对应。

如果采用 @Autowired 来注解，则同样无需指定 name 属性，若是实现该接口有多个类，则需要通过 @Qualifier 来做区分

例：UserService、UserService2是实现 IuserService 的两个实现类

类中 @Service 的注解分别是

~~~java
@Service("userService1")
public class UserService  implements IuserService {}

@Service("userService2")
public class UserService2 implements IuserService {}

~~~

那么在 TestMethod 中测试方法，使用接口 IuserService 时，使用的 @Autowired 来标注时，需要使用注解 @Qualifier 来做区分

~~~java
@Autowired
@Qualifier("userService2")
private IuserService userService;
~~~

两个的区别：  

~~~java
@Resource(name="loginService") 
     private LoginService loginService;
~~~



```java
 @Autowired(required=false)
 @Qualifier("loginService") 
 private LoginService loginService;
```

(1)@Autowired 与 @Resource 都可以用来装配 bean. 都可以写在字段上,或写在 setter 方法上;

(2)@Autowired 默认按类型装配，默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的 required 属性为 false，如：@Autowired(required=false)；如果我们想使用名称装配可以结合 @Qualifier注解进行使用;

(3)@Resource（这个注解属于J2EE的），默认安装名称进行装配，名称可以通过 name 属性进行指定，如果没有指定 name 属性，当注解写在字段上时，默认取字段名进行安装名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果 name 属性一旦指定，就只会按照名称进行装配。