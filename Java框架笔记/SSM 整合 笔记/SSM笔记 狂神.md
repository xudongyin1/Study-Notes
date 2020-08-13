## 整合SSM

### 数据库环境

创建一个存放书籍数据的数据库表

```mysql
CREATE DATABASE `ssmbuild`;

USE `ssmbuild`;

DROP TABLE IF EXISTS `books`;

CREATE TABLE `books` (
`bookId` INT(10) NOT NULL AUTO_INCREMENT COMMENT '书id',
`bookName` VARCHAR(100) NOT NULL COMMENT '书名',
`bookCounts` INT(11) NOT NULL COMMENT '数量',
`detail` VARCHAR(200) NOT NULL COMMENT '描述',
KEY `bookId` (`bookId`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT  INTO `books`(`bookId`,`bookName`,`bookCounts`,`detail`)VALUES 
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢');
```



### 基本环境搭建

1、新建一Maven项目！ssmbuild ， 添加web的支持

2、导入相关的pom依赖！

```xml
<dependencies>
   <!--Junit-->
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
   </dependency>
   <!--数据库驱动-->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.47</version>
   </dependency>
   <!-- 数据库连接池 -->
   <dependency>
       <groupId>com.mchange</groupId>
       <artifactId>c3p0</artifactId>
       <version>0.9.5.2</version>
   </dependency>

   <!--Servlet - JSP -->
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

   <!--Mybatis-->
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.5.2</version>
   </dependency>
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis-spring</artifactId>
       <version>2.0.2</version>
   </dependency>

   <!--Spring-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.1.9.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-jdbc</artifactId>
       <version>5.1.9.RELEASE</version>
   </dependency>
    <!-- aop切入 -->
     <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.13</version>
        </dependency>
</dependencies>
```

3、Maven资源过滤设置

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

4、建立基本结构和配置框架！

- com.kuang.pojo

- com.kuang.dao

- com.kuang.service

- com.kuang.controller

- mybatis-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
         PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
         "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  
  </configuration>
  ```

- applicationContext.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  </beans>
  ```



### Mybatis层编写

1、数据库配置文件 **database.properties**

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=false&useUnicode=true&characterEncoding=utf8&serverTimezone=GMT
jdbc.username=root
jdbc.password=admin
```

2、IDEA关联数据库

3、编写MyBatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
   <settings>
      <setting name="logImpl" value="STDOUT_LOGGING"/>
   </settings>

   <typeAliases>
      <package name="com.xu.pojo"/>
   </typeAliases>

   <mappers>
      <mapper class="com.xu.mapper.BookMapper"/>
   </mappers>
</configuration>
```

4、编写数据库对应的实体类 com.kuang.pojo.Books

使用lombok插件！

```java
package com.kuang.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Books {
   
   private int bookID;
   private String bookName;
   private int bookCounts;
   private String detail;
   
}
```

5、编写Dao层的 Mapper接口！

```java
package com.xu.mapper;

import com.xu.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface BookMapper {
    //增加一本书
    int addBook(Books book);

    //根据id删除一本书
    int deleteBook(@Param("id") int bookId);

    //更新一本书
    int updateBook(Books book);

    //根据id查询书
    Books findBookById(@Param("id") int bookId);

    //查询所有书
    List<Books> findAllBook();
    
    //按书籍名称查询
    Books queryBookByName(@Param("bookName") String bookName);
}
```

6、编写接口对应的 Mapper.xml 文件。需要导入MyBatis的包；

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.xu.mapper.BookMapper">
    <insert id="addBook" parameterType="Books">
          insert into books (bookId,bookName,bookCounts,detail)
          values (#{bookId},#{bookName},#{bookCounts},#{detail});
    </insert>

    <delete id="deleteBook" parameterType="int">
        delete from books where bookId=#{id}
    </delete>

    <update id="updateBook" parameterType="Books">
        update books
        set bookName = #{bookName},bookCounts = #{bookCounts},detail = #{detail}
        where bookId = #{bookId}
    </update>

    <select id="findBookById" resultType="Books">
        select * from ssmbuild.books
      where bookId = #{id}
    </select>

    <select id="findAllBook" resultType="Books">
        SELECT * from ssmbuild.books
    </select>

    <select id="queryBookByName" resultType="Books">
        select * from books where bookName=#{bookName};
    </select>
</mapper>
```

7、编写Service层的接口和实现类

接口：

```java
package com.kuang.service;

import com.kuang.pojo.Books;

import java.util.List;

//BookService:底下需要去实现,调用dao层
public interface BookService {
  //增加一本书
    int addBook(Books book);

    //根据id删除一本书
    int deleteBook(int bookId);

    //更新一本书
    int updateBook(Books book);

    //根据id查询书
    Books findBookById(int bookId);

    //查询所有书
    List<Books> findAllBook();

    //按书籍名称查询
    Books queryBookByName( String bookName);
}
```

实现类：

```java
package com.kuang.service;

import com.kuang.dao.BookMapper;
import com.kuang.pojo.Books;
import java.util.List;

public class BookServiceImpl implements BookService {

   //调用dao层的操作，设置一个set接口，方便Spring管理
   private BookMapper bookMapper;
   public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }  
   @Override
    public int addBook(Books book) {
        return bookMapper.addBook(book);
    }

    @Override
    public int deleteBook(int bookId) {
        return bookMapper.deleteBook(bookId);
    }

    @Override
    public int updateBook(Books book) {
        return bookMapper.updateBook(book);
    }

    @Override
    public Books findBookById(int bookId) {
        return bookMapper.findBookById(bookId);
    }

    @Override
    public List<Books> findAllBook() {
        return bookMapper.findAllBook();
    }

    @Override
    public Books queryBookByName(String bookName) {
        return bookMapper.queryBookByName(bookName);
    }
}
```

**OK，到此，底层需求操作编写完毕！**



### Spring层

1、配置**Spring整合MyBatis**，我们这里数据源使用c3p0连接池；

2、我们去编写Spring整合Mybatis的相关的配置文件；spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">

   <!-- 配置整合mybatis -->
   <!-- 1.关联数据库文件 -->
   <context:property-placeholder location="classpath:database.properties"/>

   <!-- 2.数据库连接池 -->
   <!--数据库连接池
       dbcp 半自动化操作 不能自动连接
       c3p0 自动化操作（自动的加载配置文件 并且设置到对象里面）
   -->
   <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
       <!-- 配置连接池属性 -->
       <property name="driverClass" value="${jdbc.driver}"/>
       <property name="jdbcUrl" value="${jdbc.url}"/>
       <property name="user" value="${jdbc.username}"/>
       <property name="password" value="${jdbc.password}"/>

       <!-- c3p0连接池的私有属性 -->
       <property name="maxPoolSize" value="30"/>
       <property name="minPoolSize" value="10"/>
       <!-- 关闭连接后不自动commit -->
       <property name="autoCommitOnClose" value="false"/>
       <!-- 获取连接超时时间 -->
       <property name="checkoutTimeout" value="10000"/>
       <!-- 当获取连接失败重试次数 -->
       <property name="acquireRetryAttempts" value="2"/>
   </bean>

   <!-- 3.配置SqlSessionFactory对象 -->
   <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
       <!-- 注入数据库连接池 -->
       <property name="dataSource" ref="dataSource"/>
       <!-- 配置MyBaties全局配置文件:mybatis-config.xml -->
       <property name="configLocation" value="classpath:mybatis-config.xml"/>
   </bean>

   <!-- 4.配置扫描Dao接口包，动态实现Dao接口注入到spring容器中 -->
   <!--解释 ：https://www.cnblogs.com/jpfss/p/7799806.html-->
   <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
       <!-- 注入sqlSessionFactory -->
       <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
       <!-- 给出需要扫描Dao接口包 -->
       <property name="basePackage" value="com.xu.mapper"/>
   </bean>

</beans>
```

3、**Spring整合service层**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

   <!-- 扫描service相关的bean -->
   <context:component-scan base-package="com.xu.service" />

   <!--BookServiceImpl注入到IOC容器中-->
   <bean id="BookServiceImpl" class="com.xu.service.BookServiceImpl">
       <property name="bookMapper" ref="bookMapper"/>
   </bean>

   <!-- 配置事务管理器 -->
   <bean id="transactionManager"class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <!-- 注入数据库连接池 -->
       <property name="dataSource" ref="dataSource" />
   </bean>
    
    <!-- aop事务织入-->
    <!-- 配置事务通知 -->
    <tx:advice id="interceptor" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!-- 配置事务切入 -->
    <aop:config>
         <aop:pointcut id="pointcut" expression="execution(* com.xu.mapper.*.*(..))"/>
        <aop:advisor advice-ref="interceptor" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
```

Spring层搞定！再次理解一下，Spring就是一个大杂烩，一个容器！对吧！



### SpringMVC层

1、**web.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
        version="4.0">

   <!--DispatcherServlet-->
   <servlet>
       <servlet-name>DispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <!--一定要注意:我们这里加载的是总的配置文件，之前被这里坑了！-->  
           <param-value>classpath:applicationContext.xml</param-value>
       </init-param>
       <load-on-startup>1</load-on-startup>
   </servlet>
   <servlet-mapping>
       <servlet-name>DispatcherServlet</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>

   <!--encodingFilter-->
   <filter>
       <filter-name>encodingFilter</filter-name>
       <filter-class>
          org.springframework.web.filter.CharacterEncodingFilter
       </filter-class>
       <init-param>
           <param-name>encoding</param-name>
           <param-value>utf-8</param-value>
       </init-param>
   </filter>
   <filter-mapping>
       <filter-name>encodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   
   <!--Session过期时间-->
   <session-config>
       <session-timeout>15</session-timeout>
   </session-config>
   
</web-app>
```

2、**spring-mvc.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd
   http://www.springframework.org/schema/mvc
   https://www.springframework.org/schema/mvc/spring-mvc.xsd">

   <!-- 配置SpringMVC -->
   <!-- 1.开启SpringMVC注解驱动 -->
   <mvc:annotation-driven />
   <!-- 2.静态资源默认servlet配置-->
   <mvc:default-servlet-handler/>

   <!-- 3.配置jsp 显示ViewResolver视图解析器 -->
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
       <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
       <property name="prefix" value="/WEB-INF/jsp/" />
       <property name="suffix" value=".jsp" />
   </bean>

   <!-- 4.扫描web相关的bean -->
   <context:component-scan base-package="com.xu.controller" />

</beans>
```

3、**Spring配置整合文件，applicationContext.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

   <import resource="spring-dao.xml"/>
   <import resource="spring-service.xml"/>
   <import resource="spring-mvc.xml"/>
   
</beans>
```



### Controller 和 视图层编写

1、BookController 类编写 ， 方法一：查询全部书籍

```java
@Controller
@RequestMapping("/book")
public class BookController {

   @Autowired
    @Qualifier("BookServiceImpl")
    private Bookservice bookservice;  //向上转型

    @RequestMapping("/findAll")
    public String findAllBook(Model model) {
        List<Books> booksList = bookservice.findAllBook();
        model.addAttribute("booksList", booksList);
        return "findAllBook";
    }
}
```

2、编写首页 **index.jsp**

```jsp
<%--
  User: win10
  Date: 2020/6/24
  Time: 15:51
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
    <style type="text/css">
      h3 {
        width: 150px;
        height: 40px;
        margin: 100px auto;
        text-align: center;
        text-decoration: none;
        background-color: burlywood;
        border-radius: 8px;
        line-height: 40px;

      }
      a {
        text-decoration: none;
        color: black;
        font-size: 20px;
      }
    </style>
  </head>
  <body>
  <h3>
    <a href="${pageContext.request.contextPath}/book/findAll">查找所有书籍</a>
  </h3>
  </body>
</html>

```

3、书籍列表页面 **findAllBook.jsp**

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%--
  User: win10
  Date: 2020/6/24
  Time: 16:52
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>书籍列表</title>
    <!-- 引入 Bootstrap -->
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
  <div class="container">
      <div class="row clearfix">
          <div class="col-md-12 column">
              <div class="page-header">
                  <h1>
                      <small>书籍列表 —— 显示所有书籍</small>
                  </h1>
              </div>
          </div>
      </div>

      <div class="row">
          <div class="col-md-4 column">
              <a class="btn btn-primary"
                 href="${pageContext.request.contextPath}/book/toAddBook">新增书籍</a>
              <a class="btn btn-primary"
                 href="${pageContext.request.contextPath}/book/findAll">查询所有书籍</a>
          </div>
          <div class="col-md-8 column">
              <form action="${pageContext.request.contextPath}/book/queryBookByName" method="post" style="float: right" class="form-inline">
                  <span style="color: red;font-weight: bold" >${msg}</span>
                  <div class="form-group">
                      <input type="text" class="form-control" name="bookName"  placeholder="输入书籍名称查询">
                  </div>
                  <button type="submit" class="btn btn-primary">查询</button>
              </form>
          </div>
      </div>

      <div class="row clearfix">
          <div class="col-md-12 column">
              <table class="table table-hover table-striped">
                  <thead>
                  <tr>
                      <th>书籍编号</th>
                      <th>书籍名字</th>
                      <th>书籍数量</th>
                      <th>书籍详情</th>
                      <th>操作</th>
                  </tr>
                  </thead>
                  <tbody>
                  <c:forEach var="book" items="${booksList}">
                      <tr>
                          <th>${book.bookId}</th>
                          <th>${book.bookName}</th>
                          <th>${book.bookCounts}</th>
                          <th>${book.detail}</th>
                          <th><a href="${pageContext.request.contextPath}/book/toUpdatePage?bookId=${book.bookId}">修改</a>
                              | <a href="${pageContext.request.contextPath}/book/deleteBook/${book.bookId}">删除</a>  </th>
                      </tr>
                  </c:forEach>
                  </tbody>
              </table>
          </div>
      </div>
  </div>

</body>
</html>

```

4、BookController 类编写 ， 方法二：添加书籍

```java
@RequestMapping("/toAddBook")
public String toAddBookPage() {
    return "addBook";
}

@RequestMapping("/addBook")
public String addBook(Books book) {
    System.out.println("------addBook------");
    System.out.println(book);
    bookservice.addBook(book);
    return "redirect:/book/findAll";
}
```

5、添加书籍页面：**addBook.jsp**

```jsp
<%--
  User: win10
  Date: 2020/6/24
  Time: 22:33
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>添加书籍</title>
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>增加书籍</small>
                </h1>
            </div>
        </div>
    </div>
</div>
<form action="/book/addBook" method="post">
    <div class="form-group">
        <label>书籍名称：</label>
        <input type="text" name="bookName" class="form-control" required>
    </div>
    <div class="form-group">
        <label>书籍数量：</label>
        <input type="text" name="bookCounts" class="form-control" required>
    </div>
    <div class="form-group">
        <label>书籍详情：</label>
        <input type="text" name="detail" class="form-control" required>
    </div>
    <div class="form-group">
        <input type="submit" value="添加" class="form-control">
    </div>
</form>

</body>
</html>
```

6、BookController 类编写 ， 方法三：修改书籍

```java
@RequestMapping("/toUpdatePage")
public String toUpdatePage(int bookId, Model model) {
    Books book = bookservice.findBookById(bookId);
    model.addAttribute("QBook", book);
    return "updateBook";
}

@RequestMapping("/updateBook")
public String updateBook(Books book) {
    System.out.println("updatebook=>" + book);
    int i = bookservice.updateBook(book);
    if (i > 0) {
        System.out.println("updateBook 成功" + book);
    }
    return "redirect:/book/findAll";
}
```

7、修改书籍页面  **updateBook.jsp**

```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%--
  User: win10
  Date: 2020/6/25
  Time: 9:22
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>修改书籍</title>
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>修改书籍</small>
                </h1>
            </div>
        </div>
    </div>
</div>
<form action="${pageContext.request.contextPath}/book/updateBook" method="post">
    <input type="hidden" name="bookId" value="${QBook.bookId}">
    <div class="form-group">
        <label>书籍名称：</label>
        <input type="text" name="bookName" class="form-control" required value="${QBook.bookName}">
    </div>
    <div class="form-group">
        <label>书籍数量：</label>
        <input type="text" name="bookCounts" class="form-control" required value="${QBook.bookCounts}">
    </div>
    <div class="form-group">
        <label>书籍详情：</label>
        <input type="text" name="detail" class="form-control" required value="${QBook.detail}">
    </div>
    <div class="form-group">
        <input type="submit" value="修改" class="form-control">
    </div>
</form>
</body>
</html>

```

8、BookController 类编写 ， 方法四：删除书籍

```java
@RequestMapping("/deleteBook/{bookId}")
public String deleteBook(@PathVariable("bookId") int bookId) {
    bookservice.deleteBook(bookId);
    return "redirect:/book/findAll";
}
```

9、BookController 类编写 ， 方法五：按书籍名称查询书籍（mapper 和 service 层都要进行添加方法）

~~~java
 @RequestMapping("/queryBookByName")
    public String queryBookByName(String bookName, Model model) {
        Books book = bookservice.queryBookByName(bookName);
        System.err.println(book);
        List<Books> list = new ArrayList<Books>();
        list.add(book);
        if (book == null) {
            list = bookservice.findAllBook();
            model.addAttribute("msg", "未找到匹配书籍");
        }
        model.addAttribute("booksList", list);
        return "findAllBook";
    }
~~~

**配置Tomcat，进行运行！**

到目前为止，这个SSM项目整合已经完全的OK了，可以直接运行进行测试！这个练习十分的重要，大家需要保证，不看任何东西，自己也可以完整的实现出来！

**项目结构图** 

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200719182228" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200719182236" alt="img" style="zoom:67%;" />