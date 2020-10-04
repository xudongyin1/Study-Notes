# MyBatis

半自动化 ORM 框架: Object Relationship Mapping 对象关系映射

对象指面向对象

关系指关系型数据库

Java 到 MySQL 的映射，开发者可以以面向对象的思想来管理数据库。

> 什么是MyBatis

- MyBatis 是一款优秀的**持久层框架**
- MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程
- MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 实体类 【Plain Old Java Objects 普通老式的 Java对象】映射成数据库中的记录。
- MyBatis 本是apache的一个开源项目 **ibatis**, 2010年这个项目由 apache 迁移到了 google code，并且改名为 MyBatis 。
- 2013年11月迁移到 **Github** .
- Mybatis官方文档 : http://www.mybatis.org/mybatis-3/zh/index.html
- GitHub : https://github.com/mybatis/mybatis-3

> 持久化

**持久化是将程序数据在持久状态和瞬时状态间转换的机制。**

- 即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用是将内存中的对象存储在数据库中，或者存储在磁盘文件中、XML数据文件中等等。
- JDBC就是一种持久化机制。文件IO也是一种持久化机制。
- 在生活中 : 将鲜肉冷藏，吃的时候再解冻的方法也是。将水果做成罐头的方法也是。

**为什么需要持久化服务呢？那是由于内存本身的缺陷引起的**

- 内存断电后数据会丢失，但有一些对象是无论如何都不能丢失的，比如银行账号等，遗憾的是，人们还无法保证内存永不掉电。
- 内存过于昂贵，与硬盘、光盘等外存相比，内存的价格要高2~3个数量级，而且维持成本也高，至少需要一直供电吧。所以即使对象不需要永久保存，也会因为内存的容量限制不能一直呆在内存中，需要持久化来缓存到外存。

> 持久层

**什么是持久层？**

- 完成持久化工作的代码块 .  ---->  dao层 【DAO (Data Access Object)  数据访问对象】
- 大多数情况下特别是企业级应用，数据持久化往往也就意味着将内存中的数据保存到磁盘上加以固化，而持久化的实现过程则大多通过各种**关系数据库**来完成。
- 不过这里有一个字需要特别强调，也就是所谓的“层”。对于应用系统而言，数据持久功能大多是必不可少的组成部分。也就是说，我们的系统中，已经天然的具备了“持久层”概念？也许是，但也许实际情况并非如此。之所以要独立出一个“持久层”的概念,而不是“持久模块”，“持久单元”，也就意味着，我们的系统架构中，应该有一个相对独立的逻辑层面，专注于数据持久化逻辑的实现.
- 与系统其他部分相对而言，这个层面应该具有一个较为清晰和严格的逻辑边界。【说白了就是用来操作数据库存在的！】

## 核心配置文件

- mybatis-config.xml 系统核心配置文件
- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。
- 能配置的内容如下：

```xml
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
<!-- 注意元素节点的顺序！顺序不对会报错 -->
```

我们可以阅读 mybatis-config.xml 上面的dtd的头文件查看元素节点顺序！

### environments元素

```xml
<environments default="development">
 <environment id="development">
   <transactionManager type="JDBC">
     <property name="..." value="..."/>
   </transactionManager>
   <dataSource type="POOLED">
     <property name="driver" value="${driver}"/>
     <property name="url" value="${url}"/>
     <property name="username" value="${username}"/>
     <property name="password" value="${password}"/>
   </dataSource>
 </environment>
</environments>
```

- 配置MyBatis的多套运行环境，将SQL映射到多个不同的数据库上，必须指定其中一个为默认运行环境（**通过default指定**）

- 具体的一套环境，通过设置id进行区别，id保证唯一！

- 子元素节点：**environment**

- - dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。

  - 数据源是必须配置的。

  - 有**三种内建的数据源类型**

    ```JAVA
    type="[UNPOOLED|POOLED|JNDI]"
    ```

  - UNPOOLED：这个数据源的实现只是每次被请求时打开和关闭连接。

  - **POOLED**：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来 , 这是一种使得并发 Web 应用快速响应请求的流行处理方式。

  - JNDI：这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。

  - 数据源也有很多第三方的实现，比如**dbcp，c3p0，druid**等等....

  - **两种事务管理器类型**，这两种事务管理器类型都不需要设置任何属性。

  - 子元素节点：transactionManager - [ 事务管理器 ]

    ```xml
    <!-- 语法 -->
    <transactionManager type="[ JDBC | MANAGED ]"/>
    ```

  - 子元素节点：**数据源（dataSource）**

### mappers元素

**mappers**

- 映射器 : 定义映射SQL语句文件
- 既然 MyBatis 的行为其他元素已经配置完了，我们现在就要定义 SQL 映射语句了。但是首先我们需要告诉 MyBatis 到哪里去找到这些语句。Java 在自动查找这方面没有提供一个很好的方法，所以最佳的方式是告诉 MyBatis 到哪里去找映射文件。你可以使用相对于类路径的资源引用， 或完全限定资源定位符（包括 `file:///` 的 URL），或类名和包名等。映射器是MyBatis中最核心的组件之一，在MyBatis 3之前，只支持xml映射器，即：所有的SQL语句都必须在xml文件中配置。而从MyBatis 3开始，还支持接口映射器，这种映射器方式允许以Java代码的方式注解定义SQL语句，非常简洁。

**引入资源方式**

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
    <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
    <mapper url="file:///var/mappers/AuthorMapper.xml"/>
</mappers>
<!--
使用映射器接口的完全限定类名
需要配置文件名称和接口名称一致，并且位于同一目录下
-->
<mappers>
    <mapper class="org.mybatis.builder.AuthorMapper"/>
     <!-- 不能这样写，用* 会出错-->
     <mapper class="org.mybatis.builder.*"/>
</mappers>
<!--
将包内的映射器接口全部注册为映射器
但是需要配置文件名称和接口名称一致，并且位于同一目录下
-->
<mappers>
    <package name="org.mybatis.builder"/>
</mappers>
```

**Mapper文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.UserMapper">
   <select></select>
</mapper>
```

- namespace中文意思：命名空间，作用如下：

- - namespace的命名必须跟某个接口同名
  - 接口中的方法与映射文件中 sql 语句 id 应该一一对应

- 1. namespace 和子元素的 id 联合保证唯一  , 区别不同的 mapper
  2. 绑定DAO接口
  3. namespace命名规则 : 包名+类名

MyBatis 的真正强大在于它的映射语句，这是它的魔力所在。由于它的异常强大，映射器的 XML 文件就显得相对简单。如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 95% 的代码。MyBatis 为聚焦于 SQL 而构建，以尽可能地为你减少麻烦。

### Properties优化

数据库这些属性都是可外部配置且可动态替换的，既可以在典型的 Java 属性文件中配置，亦可通过 properties 元素的子元素来传递。

第一步 ; 在资源目录下新建一个**db.properties**

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?        		useSSL=true&useUnicode=true&characterEncoding=utf8
username=root
password=123456
```

第二步 : 将文件导入properties 配置文件

```xml
<configuration>
   <!--导入properties文件-->
    <properties resource="db.properties">
     <!-- 优先读取properties文件，properties文件里有的属性，即使property标签下面写           了也只读取properties文件的属性（无论properties文件里属性值是否正确都不会           读取property标签），下面写了相当于没写 -->
        <property name="username" value="root"></property>
        <property name="password" value="root"></property>
    </properties>

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
   <mappers>
       <mapper resource="mapper/UserMapper.xml"/>
   </mappers>
</configuration>
```

- 配置文件优先级问题

  - 如果一个属性在不只一个地方进行了配置，那么，MyBatis 将按照下面的顺序来加载：

    - 首先读取在 properties 元素体内指定的属性。

    - 然后根据 properties 元素中的 resource 属性读取类路径下属性文件，或根据 url 属性指定的路径读取属性文件，并覆盖之前读取过的同名属性。

    - 最后读取作为方法参数传递的属性，并覆盖之前读取过的同名属性。可以在 SqlSessionFactoryBuilder.build() 方法中传入属性值，例如：

      ~~~java
      SqlSessionFactory factory = new     SqlSessionFactoryBuilder().build(reader, props);
      ~~~

  -  **通过方法参数传递的属性具有最高优先级，resource/url 属性中指定的配置文件次之，最低优先级的则是 properties 元素中指定的属性。**

- 新特性：使用占位符

  - 从 MyBatis 3.4.2 开始，你可以为占位符指定一个默认值。例如：

  ```xml
  <dataSource type="POOLED">
    <!-- ... -->
    <property name="username" value="${username:ut_user}"/> <!-- 如果属性 'username' 没有被配置，'username' 属性的值将为 'ut_user' -->
  </dataSource>
  ```

    这个特性默认是关闭的。要启用这个特性，需要添加一个特定的属性来开启这个特性。例如：

  ```xml
  <properties resource="org/mybatis/example/config.properties">
    <!-- ... -->
    <property name="org.apache.ibatis.parsing.PropertyParser.enable-default-value" value="true"/> <!-- 启用默认值特性 -->
  </properties>
  ```

### typeAliases优化

类型别名是为 Java 类型设置一个短的名字。它只和 XML 配置有关，**存在的意义仅在于用来减少类完全限定名的冗余。**

```xml
<!--配置别名,注意顺序-->
<typeAliases>
   <typeAlias type="com.kuang.pojo.User" alias="User"/>
</typeAliases>
```

当这样配置时，**`User`可以用在任何使用`com.kuang.pojo.User`的地方。**

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如:

```xml
<typeAliases>
   <package name="com.kuang.pojo"/>
</typeAliases>
```

每一个在包 `com.kuang.pojo` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。（即包下的实体类 类名首字母小写作为别名），例如：**user** 可以用在任何使用 **com.kuang.pojo.User** 的地方

若有注解，则别名为其注解值。见下面的例子：

```java
@Alias("user")
public class User {
  ...
}
```

### 其他配置浏览

**设置**

- 设置（settings）相关 => 查看帮助文档

- - 懒加载
  - 日志实现
  - 缓存开启关闭

- 一个配置完整的 settings 元素的示例如下：

  ```xml
  <settings>
      <!-- 开启缓存 -->
   <setting name="cacheEnabled" value="true"/>
       <!-- 开启延迟加载 -->
   <setting name="lazyLoadingEnabled" value="true"/>
       <!-- 是否允许单个语句返回多结果集（需要数据库驱动支持） -->
   <setting name="multipleResultSetsEnabled" value="true"/>
       <!-- 使用列标签代替列名 -->
   <setting name="useColumnLabel" value="true"/>
       <!--允许 JDBC 支持自动生成主键，需要数据库驱动支持。如果设置为 true，将强制使用自动生成主键。尽管一些数据库驱动不支持此特性，但仍可正常工作（如 Derby）。 -->
   <setting name="useGeneratedKeys" value="false"/>
   <setting name="autoMappingBehavior" value="PARTIAL"/>
   <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
   <setting name="defaultExecutorType" value="SIMPLE"/>
   <setting name="defaultStatementTimeout" value="25"/>
   <setting name="defaultFetchSize" value="100"/>
   <setting name="safeRowBoundsEnabled" value="false"/>
         <!-- 是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。 -->
   <setting name="mapUnderscoreToCamelCase" value="false"/>
   <setting name="localCacheScope" value="SESSION"/>
   <setting name="jdbcTypeForNull" value="OTHER"/>
   <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
  </settings>
  ```

**类型处理器**

- 无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。
- 你可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。【了解即可】

**对象工厂**

- MyBatis 每次创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成。
- 默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认构造方法，要么在参数映射存在的时候通过有参构造方法来实例化。
- 如果想覆盖对象工厂的默认行为，则可以通过创建自己的对象工厂来实现。【了解即可】

## 如何使用

1，新建 Maven 工程，pom.xml

~~~xml
<dependencies> 
    <dependency> 
        <groupId>org.mybatis</groupId> 
        <artifactId>mybatis</artifactId> 
        <version>3.4.5</version>
    </dependency> 
    <dependency> 
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.11</version>
   </dependency> 
</dependencies>
<!--解决Maven静态资源过滤问题，寻找配置资源文件-->
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
  </resources>
</build>
~~~

2，新建数据表

~~~mysql
use mybatis;
create table t_account(
    id int primary key auto_increment,
    username varchar(11),
    password varchar(11),
    age int
)ENGINE=InnoDB DEFAULT CHARSET=UTF8;
~~~

3，新建数据表对应的实体类 Account

~~~java
package com.southwind.entity;
import lombok.Data;
@Data
public class Account {
    private long id;
    private String username;
    private String password;
    private int age; 
}
~~~

4，在 resources 创建 MyBatis 的配置文件 config.xml，文件名可自定义

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!--下面这两行代码需要自己导入-->
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置MyBatis运行环境 -->
    <environments default="development"> <!--这里的environments的default值为要选用的数据库源id-->
        <environment id="development"><!--environment标签可用多个，即数据源可以有多个-->
            <!-- 配置JDBC事务管理 -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- POOLED配置JDBC数据源连接池 -->
            <dataSource type="POOLED"> 
               <property name="driver" value="com.mysql.cj.jdbc.Driver">
                </property> 
                <property name="url"
                         value="jdbc:mysql://localhost:3306/mybatis?
                                serverTimezone=GMT&amp;useUnicode=true&amp;
                                characterEncoding=utf-8&amp;useSSL=false">                   </property> 
                <property name="username" value="root"></property> 
                <property name="password" value="root"></property>
            </dataSource>
        </environment>
    </environments>
</configuration>
~~~

#### 使用原生接口

1、MyBatis 框架需要开发者自定义 SQL 语句，写在 Mapper.xml 文件中，实际开发中，会为每个实体类创建对应的 Mapper.xml ，定义管理该对象数据的 SQL。

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--下面这两行代码需要自己导入-->
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.mapper.AccoutMapper"> 
    <insert id="save" parameterType="com.southwind.entity.Account">
        insert into t_account(username,password,age) values(#{username},
        #{password},#{age})
        <!--这里的#{username}，#{password}，#{age}对应Account类的属性，
            因为传过来的参数是Account-->
     </insert>
</mapper>
~~~

- **namespace** 通常设置为**文件所在包+文件名**的形式。
- insert 标签表示执行添加操作。
- select 标签表示执行查询操作。
- update 标签表示执行更新操作。
- delete 标签表示执行删除操作。
- **id** 是实际调用**实体类接口方法名**。
- parameterType 是调用对应方法时参数的数据类型。

2、在全局配置文件 config.xml 中注册 AccountMapper.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!--下面这两行代码需要自己导入-->
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置MyBatis运行环境 -->
    <environments default="development"> 
        <environment id="development">
            <!-- 配置JDBC事务管理 -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- POOLED配置JDBC数据源连接池 -->
            <dataSource type="POOLED"> 
               <property name="driver" value="com.mysql.cj.jdbc.Driver">
               </property> 
               <property name="url"
                         value="jdbc:mysql://localhost:3306/mybatis?
                                serverTimezone=GMT&amp;useUnicode=true&amp;
                                characterEncoding=utf-8&amp;useSSL=false">
                </property> 
               <property name="username" value="root"></property> 
               <property name="password" value="root"></property>
            </dataSource>
        </environment>
    </environments>
    <!-- 注册AccountMapper.xml -->
    <mappers> 
        <mapper resource="com/southwind/mapper/AccountMapper.xml"></mapper>
    </mappers>
</configuration> 
~~~

3、调用 MyBatis 的原生接口执行添加操作。

~~~java
public class Test {
    public static void main(String[] args) {
        //加载MyBatis配置文件
        InputStream inputStream =
            Test.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = 
            new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory =
            sqlSessionFactoryBuilder.build(inputStream);
   /*   String resource = "org/mybatis/example/mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new     			SqlSessionFactoryBuilder().build(inputStream);    */
        SqlSession sqlSession = sqlSessionFactory.openSession();
        String statement = "com.southwind.mapper.AccoutMapper.save";
        Account account = new Account(1L,"张三","123123",22);
        sqlSession.insert(statement,account);
        sqlSession.commit();//对数据进行修改添加的操作就需要commit
        sqlSession.close();
    }
}
~~~

> SqlSessionFactoryBuilder

这个类可以被实例化、使用和丢弃，**一旦创建了 SqlSessionFactory，就不再需要它了**。 因此 **SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域（也就是局部方法变量）**。 你可以重用 SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但最好还是不要一直保留着它，以保证所有的 XML 解析资源可以被释放给更重要的事情。

> SqlSessionFactory

**SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在**，没有任何理由丢弃它或重新创建另一个实例。 使用 SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次，多次重建 SqlSessionFactory 被视为一种代码“坏习惯”。因此 **SqlSessionFactory 的最佳作用域是应用作用域**。 有很多方法可以做到，**最简单的就是使用单例模式或者静态单例模式。**

> SqlSession

每个线程都应该有它自己的 SqlSession 实例。**SqlSession 的实例不是线程安全的，因此是不能被共享的**，所以**它的最佳的作用域是请求或方法作用域**。 绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。 也绝不能将 SqlSession 实例的引用放在任何类型的托管作用域中，比如 Servlet 框架中的 HttpSession。 如果你现在正在使用一种 Web 框架，考虑将 SqlSession 放在一个和 HTTP 请求相似的作用域中。 换句话说，**每次收到 HTTP 请求，就可以打开一个 SqlSession，返回一个响应后，就关闭它。 这个关闭操作很重要，为了确保每次都能执行关闭操作，你应该把这个关闭操作放到 finally 块中**。 下面的示例就是一个确保 SqlSession 关闭的标准模式：

```
try (SqlSession session = sqlSessionFactory.openSession()) {
  // 你的应用逻辑代码
}
```

在所有代码中都遵循这种使用模式，可以保证所有数据库资源都能被正确地关闭。

==sqlSession.commit();     //对数据进行修改添加的操作就需要commit，如果只进行查询就不用==

#### 通过 Mapper 代理实现自定义接口

自定义接口，定义相关业务方法。

编写与方法相对应的 Mapper.xml。 

1、自定义接口

~~~java
package com.southwind.repository;
import com.southwind.entity.Account;
import java.util.List;
public interface AccountRepository {
    public int save(Account account);
    public int update(Account account);
    public int deleteById(long id);
    public List<Account> findAll();
    public Account findById(long id);
} 
~~~

2、创建接口对应的 Mapper.xml，定义接口方法对应的 SQL 语句。

statement 标签可根据 SQL 执行的业务选择 insert、delete、update、select。

MyBatis 框架会根据规定自动创建接口实现类的代理对象。

规则：

- Mapper.xml 中 namespace 为接口的全类名。
- Mapper.xml 中 statement 的 id 为接口中对应的方法名。
- Mapper.xml 中 statement 的 parameterType 和接口中对应方法的参数类型一致。
- Mapper.xml 中 statement 的 resultType 和接口中对应方法的返回值类型一致。

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--下面这两行代码需要自己导入-->
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.repository.AccountRepository"> 
    <insert id="save" parameterType="com.southwind.entity.Account">
 <!--增删改操作不用指定resultType，默认是int类型，因为数据库执行成功返回的是行号-->
        insert into t_account(username,password,age) values(#{username},#
        {password},#{age})
    </insert> 
    <update id="update" parameterType="com.southwind.entity.Account">
        update t_account set username = #{username},password = #{password},age
        = #{age} where id = #{id}
    </update> 
    <delete id="deleteById" parameterType="long">
        delete from t_account where id = #{id}
    </delete> 
    <select id="findAll" resultType="com.southwind.entity.Account">
        select * from t_account
    </select> 
    <select id="findById" parameterType="long"
            resultType="com.southwind.entity.Account">
        select * from t_account where id = #{id}
    </select>
</mapper> 
~~~

==增删改操作不用指定resultType，默认是int类型，因为数据库执行成功返回的是行号==

3、在 config.xml 中注册 AccountRepository.xml

~~~xml
<!-- 注册AccountMapper.xml -->
<mappers> 
    <mapper resource="com/southwind/mapper/AccountMapper.xml"></mapper> 
    <mapper resource="com/southwind/repository/AccountRepository.xml"></mapper>
</mappers> 
~~~

4、调用接口的代理对象完成相关的业务操作

~~~java
package com.southwind.test;
import com.southwind.entity.Account;
import com.southwind.repository.AccountRepository;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import java.io.InputStream;
import java.util.List;
public class Test2 {
    public static void main(String[] args) {
        InputStream inputStream =
            Test.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new
            SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory =
            sqlSessionFactoryBuilder.build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //获取实现接口的代理对象
        AccountRepository accountRepository =
            sqlSession.getMapper(AccountRepository.class);
        //添加对象
        // Account account = new Account(3L,"王五","111111",24);
        // int result = accountRepository.save(account);
        // sqlSession.commit();
        //查询全部对象
        // List<Account> list = accountRepository.findAll();
        // for (Account account:list){
        // System.out.println(account);
        // }
        // sqlSession.close();
        //通过id查询对象
        // Account account = accountRepository.findById(3L);
        // System.out.println(account);
        // sqlSession.close();
        //修改对象
        // Account account = accountRepository.findById(3L);
        // account.setUsername("小明");
        // account.setPassword("000");
        // account.setAge(18);
        这样子account的id还是3L，所以修改的是id为3L的数据，如果写上 
        account.setId(10l);数据库里面没有的id会无法修改
        也可以通过new一个Account来修改，但new来的Account的id必须设置成
        数据库有的
        // int result = accountRepository.update(account);
        // sqlSession.commit();
        // System.out.println(result);
        // sqlSession.close();
        //通过id删除对象
        int result = accountRepository.deleteById(3L);
        System.out.println(result);
        sqlSession.commit();
        sqlSession.close();
    }
}
~~~

### **Mapper.xml**

- statement 标签：select、update、delete、insert 分别对应查询、修改、删除、添加操作。

- parameterType：参数数据类型

  1、基本数据类型，通过 id 查询 Account

  ~~~xml
  <select id="findById" parameterType="long" resultType="com.southwind.entity.Account">
      select * from t_account where id = #{id}
  </select> 
  ~~~

  2、String 类型，通过 name 查询 Account 

  ~~~xml
  <select id="findByName" parameterType="java.lang.String" resultType="com.southwind.entity.Account">
      select * from t_account where username = #{username}
  </select> 
  ~~~

  3、包装类，通过 id 查询 Account

  ~~~xml
  <select id="findById2" parameterType="java.lang.Long" resultType="com.southwind.entity.Account">
      select * from t_account where id = #{id}
  </select> 
  ~~~

  4、多个参数，通过 name 和 age 查询 Account

  ~~~xml
  <select id="findByNameAndAge" resultType="com.southwind.entity.Account">
      select * from t_account where username = #{arg0} and age = #{arg1}
  <!--  或者  select * from t_account where username = #{param1} and age = #{param2} -->
  </select> 
  ~~~

  5、Java Bean

  ~~~xml
  <update id="update" parameterType="com.southwind.entity.Account">
      update t_account set username = #{username},password = #{password},
      age =#{age} where id = #{id}
  </update>
  ~~~

- resultType：结果类型

  1、基本数据类型，统计 Account 总数

  ~~~xml
  <select id="count" resultType="int"> 
      select count(id) from t_account
  </select>
  ~~~

  2、包装类，统计 Account 总数	

  ~~~xml
  <select id="count2" resultType="java.lang.Integer">
      select count(id) from t_account
  </select> 
  ~~~

  3、String 类型，通过 id 查询 Account 的 name

  ~~~xml
  <select id="findNameById" resultType="java.lang.String">
      select username from t_account where id = #{id}
  </select> 
  ~~~

  4、Java Bean

  ~~~xml
  <select id="findById" parameterType="long"  resultType="com.southwind.entity.Account">
      select * from t_account where id = #{id}
  </select>
  ~~~

**课堂练习**：根据 密码 和 名字 查询用户

思路一：直接在方法中传递参数

1、在接口方法的参数前加 @Param属性

~~~java
//通过密码和名字查询用户
User selectUserByNP(@Param("username") String username,@Param("pwd") Stringpwd);
/* 或者这样
User FindbyNP3( String name,  String p);  */
~~~

2、Sql语句编写的时候，直接取@Param中设置的值即可，不需要单独设置参数类型

```xml
<select id="selectUserByNP" resultType="com.kuang.pojo.User">
    select * from user where name = #{username} and pwd = #{pwd}
</select>
<!--  对应上面  User FindbyNP3( String name,  String p);
<select id="FindbyNP3" resultType="com.kuangshen.entity.User">
        select * from user where name=#{param1} and pwd=#{param2};
</select>   -->
```

3、使用方法

~~~java
mapper.selectUserByNP("李四", "111")
~~~

思路二：使用万能的Map

1、在接口方法中，参数直接传递Map；

```java
User selectUserByNP2(Map<String,Object> map);
```

2、编写sql语句的时候，需要传递参数类型，参数类型为map

```xml
<select id="selectUserByNP2" parameterType="map" resultType="com.kuang.pojo.User">
    <!--  parameterType="map" 可写可不写-->
select * from user where name = #{username} and pwd = #{pwd}
    <!--  username 对应 map 的 key ，pwd一样-->
</select>
```

3、在使用方法的时候，Map的 key 为 sql中取的值即可，**没有顺序要求**！

```java
Map<String, Object> map = new HashMap<String, Object>();
map.put("username","小明");
map.put("pwd","123456");
User user = mapper.selectUserByNP2(map);
```

总结：如果参数过多，我们可以考虑直接使用Map实现，如果参数比较少，直接传递参数即可

**模糊查询like语句该怎么写?**

第1种：在Java代码中添加sql通配符。

```xml
string wildcardname = “%smi%”;
list<name> names = mapper.selectlike(wildcardname);

<select id=”selectlike” resultType="com.kuangshen.entity.User">
select * from foo where bar like #{value}
</select>
```

第2种：在sql语句中拼接通配符，会引起sql注入 (不推荐)

```xml
string wildcardname = “smi”;
list<name> names = mapper.selectlike(wildcardname);

<select id=”selectlike” resultType="com.kuangshen.entity.User">
    select * from foo where bar like "%"#{value}"%"
</select>
```

## ResultMap

> 查询为null问题

**要解决的问题：属性名和字段名不一致**

1、查看之前的数据库的字段名

user（id，name，pwd）

2、Java中的实体类设计

```java
public class User {
   private int id;  //id
   private String name;   //姓名
   private String password;   //密码和数据库的密码字段名不一样！
}
```

3、接口

```java
//根据id查询用户
User selectUserById(int id);
```

4、mapper映射文件

```xml
<select id="selectUserById" resultType="user">
  select * from user where id = #{id}
</select>
```

5、测试

```java
@Test
public void testSelectUserById() {
   SqlSession session = MybatisUtils.getSession();  //获取SqlSession连接
   UserMapper mapper = session.getMapper(UserMapper.class);
   User user = mapper.selectUserById(1);
   System.out.println(user);
   session.close();
}
```

**结果:**

- User{id=1, name='狂神', password='null'}
- 查询出来发现 password 为空 . 说明出现了问题！

**分析：**

- select * from user where id = #{id} 可以看做

  select  id,name,pwd  from user where id = #{id}

- mybatis会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写) , 去对应的实体类中查找相应列名的get方法取值 , 由于找不到getPwd() , 所以password返回null ; 【自动映射】

> 解决方案

方案一：为列名指定别名 , 别名和java实体类的属性名一致 .

```xml
<select id="selectUserById" resultType="User">
  select id , name , pwd as password from user where id = #{id}
</select>
```

**方案二：使用结果集映射->ResultMap** 【推荐】

```xml
<resultMap id="UserMap" type="User">
   <!-- id为主键 -->
   <id column="id" property="id"/>
   <!-- column 是数据库表的列名 , property 是对应实体类的属性名 -->
   <result column="name" property="name"/>
   <!-- 一般只写不同名字段，同名字段写了也没用，所以上面 id 和 name 都可以不写 -->
   <result column="pwd" property="password"/>
</resultMap>

<select id="selectUserById" resultMap="UserMap">
  select id , name , pwd from user where id = #{id}
</select>
```

> ResultMap

**自动映射**

- `resultMap` 元素是 MyBatis 中最重要最强大的元素。它可以让你从 90% 的 JDBC `ResultSets` 数据提取代码中解放出来。
- 实际上，在为一些比如连接的复杂语句编写映射代码的时候，一份 `resultMap` 能够代替实现同等功能的长达数千行的代码。
- ResultMap 的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的关系就行了。

你已经见过简单映射语句的示例了，但并没有显式指定 `resultMap`。比如：

```xml
<select id="selectUserById" resultType="map">
select id , name , pwd
  from user
  where id = #{id}
</select>
```

上述语句只是简单地将所有的列映射到 `HashMap` 的键上，这由 `resultType` 属性指定。虽然在大部分情况下都够用，但是 HashMap 不是一个很好的模型。你的程序更可能会使用 JavaBean 或 POJO（Plain Old Java Objects，普通老式 Java 对象）作为模型。

`ResultMap` 最优秀的地方在于，虽然你已经对它相当了解了，但是根本就不需要显式地用到他们。

**手动映射**

1、返回值类型为resultMap

```xml
<select id="selectUserById" resultMap="UserMap">
  select id , name , pwd from user where id = #{id}
</select>
```

2、编写resultMap，实现手动映射！

```xml
<resultMap id="UserMap" type="User">
   <!-- id为主键 -->
   <id column="id" property="user_id"/>
   <!-- column是数据库表的列名 , property是对应实体类的属性名 -->
   <result column="name" property="user_name"/>
   <result column="pwd" property="user_password"/>
</resultMap>
```

如果世界总是这么简单就好了。但是肯定不是的，数据库中，存在一对多，多对一的情况，我们之后会使用到一些高级的结果集映射，association，collection这些。

## 日志工厂

思考：我们在测试SQL的时候，要是能够在控制台输出 SQL 的话，是不是就能够有更快的排错效率？

如果一个 数据库相关的操作出现了问题，我们可以根据输出的SQL语句快速排查问题。

对于以往的开发过程，我们会经常使用到debug模式来调节，跟踪我们的代码执行过程。但是现在使用Mybatis是基于接口，配置文件的源代码执行过程。因此，我们必须选择日志工具来作为我们开发，调节程序的工具。

Mybatis内置的日志工厂提供日志功能，具体的日志实现有以下几种工具：

- SLF4J
- Apache Commons Logging
- Log4j 2
- Log4j
- JDK logging

具体选择哪个日志实现工具由MyBatis的内置日志工厂确定。它会使用最先找到的（按上文列举的顺序查找）。如果一个都未找到，日志功能就会被禁用。

**标准日志实现**

指定 MyBatis 应该使用哪个日志记录实现。如果此设置不存在，则会自动发现日志记录实现。

```xml
<settings>
       <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

测试，可以看到控制台有大量的输出！我们可以通过这些输出来判断程序到底哪里出了Bug

### Log4j

**简介：**

- Log4j是Apache的一个开源项目
- 通过使用Log4j，我们可以控制日志信息输送的目的地：控制台，文本，GUI组件....
- 我们也可以控制每一条日志的输出格式；
- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。最令人感兴趣的就是，这些可以通过一个配置文件来灵活地进行配置，而不需要修改应用的代码。

**使用步骤：**

1、导入log4j的包

```xml
<dependency>
   <groupId>log4j</groupId>
   <artifactId>log4j</artifactId>
   <version>1.2.17</version>
</dependency>
```

2、log4j.properties 配置文件编写 (放在 resources 下)

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
log4j.appender.file.File=./log/xu.log
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

3、setting设置日志实现

```xml
<settings>
   <setting name="logImpl" value="LOG4J"/>
</settings>
```

4、在程序中使用Log4j进行输出！

```java
//注意导包：org.apache.log4j.Logger
static Logger logger = Logger.getLogger(MyTest.class);

@Test
public void selectUser() {
   logger.info("info：进入selectUser方法");
   logger.debug("debug：进入selectUser方法");
   logger.error("error: 进入selectUser方法");
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);
   List<User> users = mapper.selectUser();
   for (User user: users){
       System.out.println(user);
  }
   session.close();
}
```

5、测试，看控制台输出！

- 使用Log4j 输出日志
- 可以看到还生成了一个日志的文件 【需要修改 file 的日志级别】

## limit实现分页

**思考：为什么需要分页？**

在学习mybatis等持久层框架的时候，会经常对数据进行增删改查操作，使用最多的是对数据库进行查询操作，如果查询大量数据的时候，我们往往使用分页进行查询，也就是每次处理小部分数据，这样对数据库压力就在可控范围内。

> **使用Limit实现分页**

```mysql
#语法
SELECT * FROM table LIMIT stratIndex，pageSize

SELECT * FROM table LIMIT 5,10; // 检索记录行 6-15  

#为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1： 
#以前支持，现在已经作为bug被修复
SELECT * FROM table LIMIT 95,-1; // 检索记录行 96-last.  

#如果只给定一个参数，它表示返回最大的记录行数目：   
SELECT * FROM table LIMIT 5; //检索前 5 个记录行  

#换句话说，LIMIT n 等价于 LIMIT 0,n。 
```

**步骤：**

1、修改Mapper文件

```xml
<select id="selectUser" parameterType="map" resultType="user">
    select * from user limit #{startIndex},#{pageSize}
</select>
```

2、Mapper接口，参数为map

```jAVA
//选择全部用户实现分页
List<User> selectUser(Map<String,Integer> map);
```

3、在测试类中传入参数测试

- 推断：起始位置 =  （当前页面 - 1 ） * 页面大小

```JAVA
//分页查询 , 两个参数startIndex , pageSize
@Test
public void testSelectUser() {
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   int currentPage = 1;  //第几页
   int pageSize = 2;  //每页显示几个
   Map<String,Integer> map = new HashMap<String,Integer>();
   map.put("startIndex",(currentPage-1)*pageSize);
   map.put("pageSize",pageSize);

   List<User> users = mapper.selectUser(map);
   for (User user: users){
       System.out.println(user);
  }
   session.close();
}
```

> RowBounds分页

我们除了使用Limit在SQL层面实现分页，也可以使用RowBounds在Java代码层面实现分页，当然此种方式作为了解即可。我们来看下如何实现的！

**步骤：**

1、mapper接口

```java
//选择全部用户RowBounds实现分页
List<User> getUserByRowBounds();
```

2、mapper文件

```xml
<select id="getUserByRowBounds" resultType="user">
    select * from user
</select>
```

3、测试类

在这里，我们需要使用RowBounds类

```java
@Test
public void testUserByRowBounds() {
   SqlSession session = MybatisUtils.getSession();

   int currentPage = 2;  //第几页
   int pageSize = 2;  //每页显示几个
   RowBounds rowBounds = new RowBounds((currentPage-1)*pageSize,pageSize);

   //通过session.**方法进行传递rowBounds，[此种方式现在已经不推荐使用了]
   List<User> users =session.selectList("com.kuang.mapper.UserMapper.getUserByRowBounds", null,rowBounds);

   for (User user: users){
       System.out.println(user);
  }
   session.close();
}
```

> PageHelper

分页插件，了解即可



## 使用注解开发

> 面向接口编程

- 大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程
- **根本原因 :  解耦 , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好**
- 在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；
- 而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。

**关于接口的理解**

- 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

- 接口的本身反映了系统设计人员对系统的抽象理解。

- 接口应有两类：

- - 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；
  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

- 一个个体有可能有多个抽象面。抽象体与抽象面是有区别的。

**三个面向区别**

- 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .
- 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .
- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是对系统整体的架构

> 利用注解开发

- **mybatis最初配置信息是基于 XML ,映射语句(SQL)也是定义在 XML 中的。而到MyBatis 3提供了新的基于注解的配置。不幸的是，Java 注解的的表达力和灵活性十分有限。最强大的 MyBatis 映射（级联查询）并不能用注解来构建**

- sql 类型主要分成 :

- - @select ()
  - @update ()
  - @Insert ()
  - @delete ()

**注意：**利用注解开发就不需要mapper.xml映射文件了 .

1、我们在我们的接口中添加注解

```java
//查询全部用户
@Select("select id,name,pwd password from user")
public List<User> getAllUser();
```

2、**在mybatis的核心配置文件中注入**

```xml
<!--使用class绑定接口-->
<mappers>
   <mapper class="com.kuang.mapper.UserMapper"/>
</mappers>
```

3、我们去进行测试

```java
@Test
public void testGetAllUser() {
   SqlSession session = MybatisUtils.getSession();
   //本质上利用了jvm的动态代理机制
   UserMapper mapper = session.getMapper(UserMapper.class);

   List<User> users = mapper.getAllUser();
   for (User user : users){
       System.out.println(user);
  }

   session.close();
}
```

4、利用Debug查看本质

![img](https://gitee.com/xudongyin/img/raw/master/img/640)

5、本质上利用了jvm的动态代理机制

![image-20200719174415599](https://gitee.com/xudongyin/img/raw/master/img/image-20200719174415599.png)

6、Mybatis详细的执行流程

![image-20200719174607890](https://gitee.com/xudongyin/img/raw/master/img/image-20200719174607890.png)

> 注解增删改

改造MybatisUtils工具类的getSession( ) 方法，重载实现。

```java
  //获取SqlSession连接
  public static SqlSession getSession(){
      return getSession(true); //事务自动提交
  }
 
  public static SqlSession getSession(boolean flag){
      return sqlSessionFactory.openSession(flag);
  }
```

【注意】确保实体类和数据库字段对应，不对应的话无法查阅对应字段信息

**查询：**

1、编写接口方法注解

```java
//根据id查询用户
@Select("select * from user where id = #{id}")
User selectUserById(@Param("id") int id);
```

2、测试

```java
@Test
public void testSelectUserById() {
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   User user = mapper.selectUserById(1);
   System.out.println(user);

   session.close();
}
```

**新增：**

1、编写接口方法注解

```java
//添加一个用户
@Insert("insert into user (id,name,pwd) values (#{id},#{name},#{pwd})")
int addUser(User user);
```

2、测试

```java
@Test
public void testAddUser() {
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   User user = new User(6, "秦疆", "123456");
   mapper.addUser(user);

   session.close();
}
```

**修改：**

1、编写接口方法注解

```java
//修改一个用户
@Update("update user set name=#{name},pwd=#{pwd} where id = #{id}")
int updateUser(User user);
```

2、测试

```java
@Test
public void testUpdateUser() {
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   User user = new User(6, "秦疆", "zxcvbn");
   mapper.updateUser(user);

   session.close();
}
```

**删除：**

1、编写接口方法注解

```java
//根据id删除用
@Delete("delete from user where id = #{id}")
int deleteUser(@Param("id")int id);
```

2、测试

```java
@Test
public void testDeleteUser() {
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   mapper.deleteUser(6);
   
   session.close();
}
```

【注意点：增删改一定记得对事务的处理】

> 关于@Param

@Param注解用于给方法参数起一个名字。以下是总结的使用原则：

- 在方法只接受一个参数的情况下，可以不使用@Param。
- 在方法接受多个参数的情况下，建议一定要使用@Param注解给参数命名。
- **如果参数是 JavaBean ， 则不能使用@Param。**
- 不使用@Param注解时，参数只能有一个，并且是Javabean。

> \#与$的区别

- **\#{} 的作用主要是替换预编译语句(PrepareStatement)中的占位符?，很大程度上防止 SQL注入** 【推荐使用】

  ```mysql
  INSERT INTO user (name) VALUES (#{name});
  INSERT INTO user (name) VALUES (?);
  ```

- **${} 的作用是直接进行字符串替换，无法防止 SQL 注入**

  ```mysql
  INSERT INTO user (name) VALUES ('${name}');
  INSERT INTO user (name) VALUES ('kuangshen');
  ```

> 使用注解和配置文件协同开发，才是MyBatis的最佳实践！

## 级联查询

#### **一对多**

Student

~~~java
package com.southwind.entity;
import lombok.Data;
@Data
public class Student {
    private long id;
    private String name;
    private Classes classes;
    //一个学生对应一个班级
}
~~~

Classes

~~~java
package com.southwind.entity;
import lombok.Data;
import java.util.List;
@Data
public class Classes {
    private long id;
    private String name;
    private List<Student> students;
    //一个班级对应多个学生
}
~~~

StudentRepository

~~~java
package com.southwind.repository;
import com.southwind.entity.Student;
public interface StudentRepository {
    public Student findById(long id);
}
~~~

StudentRepository.xml

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.repository.StudentRepository">
    <!--将整个studentMap返回-->
    <select id="findById" parameterType="long" resultMap="studentMap">
        select s.id,s.name,c.id as cid,c.name as cname from student s,classes c
        where s.id = #{id} and s.cid = c.id
        <!--c.id as cid,c.name as cname这里修改字段名称是为了与前面student的
            s.id和s.name做区分，因为查表出来显示的四个字段名称为“id，name，id，               name”，方便操作-->
    </select>
    <resultMap id="studentMap" type="com.southwind.entity.Student">
        <!--主键使用id标签，其他字段属性用result标签-->
        <id column="id" property="id"></id>
        <!-- column 对应数据库字段，property 对应实体类属性-->
        <result column="name" property="name"></result>
        <!--这里对应classes类-->
        <association property="classes" javaType="com.southwind.entity.Classes">
            <!--这里因为 c.id as cid ，所以 column 为 cid-->
            <id column="cid" property="id"></id>
            <result column="cname" property="name"></result>
        </association>
    </resultMap>
</mapper>
~~~

==<association>：对象是类就用这个标签，里面的类型定义是javaType属性定义==

==<collection>：对象是集合就用这个标签，里面的类型定义是ofType属性定义==

> 测试

~~~java
StudentRepository studentRepository =    sqlSession.getMapper(StudentRepository.class);
System.out.println(studentRepository.findByIdLazy(2));
~~~



ClassesRepository

~~~java
package com.southwind.repository;
import com.southwind.entity.Classes;
public interface ClassesRepository {
    public Classes findById(long id);
}
~~~

ClassesRepository.xml

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.repository.ClassesRepository">
    <select id="findById" parameterType="long" resultMap="classesMap">
        select s.id,s.name,c.id as cid,c.name as cname from student s,classes c
        where c.id = #{id} and s.cid = c.id
    </select>
    <resultMap id="classesMap" type="com.southwind.entity.Classes">
         <!-- column 对应数据库字段，property 对应实体类属性-->
        <id column="cid" property="id"></id>
        <result column="cname" property="name"></result>
        <collection property="students" ofType="com.southwind.entity.Student">
            <id column="id" property="id"/>
            <result column="name" property="name"/>
        </collection>
    </resultMap>
</mapper>
~~~

> 测试

~~~java
ClassRepository classRepository = sqlSession.getMapper(ClassRepository.class);
System.out.println(classRepository.findById(2));
~~~



#### **多对多**

Customer

```java
package com.southwind.entity;
import lombok.Data;
import java.util.List;
@Data
public class Customer {
    private long id;
    private String name;
    private List<Goods> goods;
    //顾客对应多种货物
}
```

Goods

```java
package com.southwind.entity;
import lombok.Data;
import java.util.List;
@Data
public class Goods {
    private long id;
    private String name;
    private List<Customer> customers;
    //货物对应多个用户
}
```

CustomerRepository

```java
package com.southwind.repository;
import com.southwind.entity.Customer;
public interface CustomerRepository {
    public Customer findById(long id);
}
```

CustomerRepository.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.repository.CustomerRepository">
    <select id="findById" parameterType="long" resultMap="customerMap">
        select c.id cid,c.name cname,g.id gid,g.name gname from customer c,
        goods g,customer_goods cg where c.id = #{id} and cg.cid = c.id and               cg.gid = g.id
    </select>
     <resultMap id="customerMap" type="com.southwind.entity.Customer">
         <!-- column 对应数据库字段，property 对应实体类属性-->
         <id column="cid" property="id"></id>
        <result column="cname" property="name"></result>
        <collection property="goods" ofType="com.southwind.entity.Goods">
            <id column="gid" property="id"/>
            <result column="gname" property="name"/>
        </collection>
    </resultMap>
</mapper>
```

> 测试

~~~java
CustomerRepository customerRepository =    sqlSession.getMapper(CustomerRepository.class);
System.out.println(customerRepository.findById(2l));   
~~~



GoodsRepository

```java
package com.southwind.repository;
import com.southwind.entity.Goods;
public interface GoodsRepository {
    public Goods findById(long id);
}
```

GoodsRepository.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.repository.GoodsRepository">
    <select id="findById" parameterType="long" resultMap="goodsMap">
        select c.id cid,c.name cname,g.id gid,g.name gname from customer c,
        goods g,customer_goods cg where g.id = #{id} and cg.cid = c.id and               cg.gid = g.id
    </select>
    <resultMap id="goodsMap" type="com.southwind.entity.Goods">
        <!-- column 对应数据库字段，property 对应实体类属性-->
        <id column="gid" property="id"></id>
        <result column="gname" property="name"></result>
        <collection property="customers" ofType="com.southwind.entity.Customer">
            <id column="cid" property="id"/>
            <result column="cname" property="name"/>
        </collection>
    </resultMap>
</mapper>
```

> 测试

```java
GoodsRepository goodsRepository = sqlSession.getMapper(GoodsRepository.class);
System.out.println(goodsRepository.findById(2));
```



## 逆向工程

MyBatis 框架需要：实体类、自定义 Mapper 接口、Mapper.xml

传统的开发中上述的三个组件需要开发者手动创建，逆向工程可以帮助开发者来自动创建三个组件，减轻开发者的工作量，提高工作效率。

### 如何使用

MyBatis Generator，简称 MBG，是一个专门为 MyBatis 框架开发者定制的代码生成器，可自动生成MyBatis 框架所需的实体类、Mapper 接口、Mapper.xml，支持基本的 CRUD 操作，但是一些相对复杂的 SQL 需要开发者自己来完成。

1，新建 Maven 工程，pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId> 
        <version>3.4.5</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.11</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-core</artifactId>
        <version>1.3.2</version>
    </dependency>
</dependencies>
```

2，在 resources 创建 MBG 配置文件 generatorConfig.xml

- jdbcConnection 配置数据库连接信息。
- javaModelGenerator 配置 JavaBean 的生成策略。
- sqlMapGenerator 配置 SQL 映射文件生成策略。
- javaClientGenerator 配置 Mapper 接口的生成策略。
- table 配置目标数据表（tableName：表名，domainObjectName：JavaBean 类名）。

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE generatorConfiguration
     PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
     "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
    <generatorConfiguration>
        <context id="testTables" targetRuntime="MyBatis3">
            <jdbcConnection
                            driverClass="com.mysql.cj.jdbc.Driver"
                            connectionURL="jdbc:mysql://localhost:3306/mybatis?
                                           useUnicode=true&characterEncoding=UTF-8"
                            userId="root"
                            password="root">
            </jdbcConnection>
            <javaModelGenerator targetPackage="com.southwind.entity"
                                targetProject="./src/main/java"> 
                <!--./表示当前文件夹，可省略，resources文件夹就是项目的根目录-->
            </javaModelGenerator>
            <sqlMapGenerator targetPackage="com.southwind.repository"
                             targetProject="./src/main/java">
            </sqlMapGenerator>
            <javaClientGenerator type="XMLMAPPER"
                                 targetPackage="com.southwind.repository" targetProject="./src/main/java">
            </javaClientGenerator>
            <table tableName="t_user" domainObjectName="User"></table>
        </context>
    </generatorConfiguration>
    ```

3，创建 Generator 执行类。

```java
package com.southwind.test;
import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.exception.InvalidConfigurationException;
import org.mybatis.generator.exception.XMLParserException;
import org.mybatis.generator.internal.DefaultShellCallback;
import java.io.File;
import java.io.IOException;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> warings = new ArrayList<String>();
        boolean overwrite = true;
        String genCig = "/generatorConfig.xml";
        File configFile = new File(Main.class.getResource(genCig).getFile());
        ConfigurationParser configurationParser = new
            ConfigurationParser(warings);
        Configuration configuration = null;
        try {
            configuration = configurationParser.parseConfiguration(configFile);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (XMLParserException e) {
            e.printStackTrace();
        }
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = null;
        try {
            myBatisGenerator = new
               MyBatisGenerator(configuration,callback,warings);
        } catch (InvalidConfigurationException e) {
            e.printStackTrace();
        }
        try {
            myBatisGenerator.generate(null);
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace(); }
    }
}
```



## **MyBatis** 延迟加载

什么是延迟加载？

延迟加载也叫懒加载、惰性加载，使用延迟加载可以提高程序的运行效率，针对于数据持久层的操作，在某些特定的情况下去访问特定的数据库，在其他情况下可以不访问某些表，从一定程度上减少了 Java应用与数据库的交互次数。

查询学生和班级时，学生和班级是两张不同的表，如果当前需求只需要获取学生的信息，那么查询学生单表即可，如果需要通过学生获取对应的班级信息，则必须查询两张表。

不同的业务需求，需要查询不同的表，根据具体的业务需求来动态减少数据表查询的动作就是延迟加载。

1,在 config.xml 中开启延迟加载

```xml
<settings>
    <!-- 打印SQL-->
    <setting name="logImpl" value="STDOUT_LOGGING" />
    <!-- 开启延迟加载 -->
    <setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

2,将多表关联查询拆分成多个单表查询

StudentRepository

```java
public Student findByIdLazy(long id);
```

ClassesRepository

```java
public Classes findByIdLazy(long id);
```

ClassesRepository.xml

```xml
<select id="findByIdLazy" parameterType="long"
        resultType="com.southwind.entity.Classes">
    select * from classes where id = #{id}
</select>
```

StudentRepository.xml

```xml
<resultMap id="studentMapLazy" type="com.southwind.entity.Student">
    <id column="id" property="id"></id>
    <result column="name" property="name"></result>
    <association property="classes" javaType="com.southwind.entity.Classes"
                select="com.southwind.repository.ClassesRepository.findByIdLazy"                 column="cid">
    </association>
</resultMap>
<select id="findByIdLazy" parameterType="long" resultMap="studentMapLazy">
    select * from student where id = #{id}
</select>
```

3，运行测试

test.java

```java
StudentRepository studentRepository =              					              sqlSession.getMapper(StudentRepository.class);
Student student = studentRepository.findByIdLazy(2);
System.out.println(student.getName());
```

日志

```verilog
==>  Preparing: select * from student where id=? 
==> Parameters: 2(Long)
<==    Columns: id, name, cid
<==        Row: 2, 张三, 2
<==      Total: 1
张三
```

==只查询一次，不会去查询班级表==

获取学生班级时，才会查询两次

~~~java
System.out.println(student.getClasses());
~~~

日志

~~~verilog
==>  Preparing: select * from student where id=? 
==> Parameters: 2(Long)
<==    Columns: id, name, cid
<==        Row: 2, 张三, 2
<==      Total: 1
==>  Preparing: select * from class where id=?; 
==> Parameters: 2(Long)
<==    Columns: id, name
<==        Row: 2, 二年级
<==      Total: 1
Classes(id=2, name=二年级, students=null)
~~~



## MyBatis 缓存

> 缓存简介

1、什么是缓存 [ Cache ]？

- 存在内存中的临时数据。
- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

2、为什么使用缓存？

- 减少和数据库的交互次数，减少系统开销，提高系统效率。

3、什么样的数据能使用缓存？

- 经常查询并且不经常改变的数据。

什么是 MyBatis 缓存

使用缓存可以减少 Java 应用与数据库的交互次数，从而提升程序的运行效率。比如查询出 id = 1 的对象，第一次查询出之后会自动将该对象保存到缓存中，当下一次查询时，直接从缓存中取出对象即可，无需再次访问数据库。

> MyBatis 缓存分类

1、一级缓存：**SqlSession 级别**，也称为本地缓存，**默认开启**，并且**不能关闭**。

操作数据库时需要创建 SqlSession 对象，在对象中有一个 **HashMap 用于存储缓存数据**，不同的

SqlSession 之间缓存数据区域是互不影响的。

一级缓存的作用域是 SqlSession 范围的，当在**同一个 SqlSession 中执行两次相同的 SQL 语句**时，第一次执行完毕会将结果保存到缓存中，第二次查询时直接从缓存中获取。

**需要注意的是，如果 SqlSession 执行了 DML 操作（insert、update、delete）（即使操作的不是同一条数据），MyBatis 必须将缓存清空以保证数据的准确性。**

2、二级缓存：也叫全局缓存，**Mapper 级别**，默认关闭，可以开启。

- 工作机制

- - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  - 如果**当前会话关闭了，这个会话对应的一级缓存就没了，一级缓存中的数据被保存到二级缓存中；**
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；

使二级级缓存时，多个 SqlSession 使用同一个 Mapper 的 SQL 语句操作数据库，得到的数据会存在二级缓存区，**同样是使用 HashMap 进行数据存储**，相比较于一级缓存，二级缓存的范围更广，**多个SqlSession 可以共用二级缓存**，二级缓存是跨 SqlSession 的。

二级缓存是多个 SqlSession 共享的，其作用域是 Mapper 的同一个 namespace，**不同的 SqlSession两次执行相同的 namespace 下的 SQL 语句，参数也相等，则第一次执行成功之后会将数据保存到二级缓存中，第二次可直接从二级缓存中取出数据。**

3、自定义二级缓存：为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

> 缓存原理图

![img](https://gitee.com/xudongyin/img/raw/master/img/233)

sqlsession 先从二级缓存开始查询有无缓存，如果没有二级缓存再查看有无一级缓存，没有一级缓存的话就只能连接数据库进行查询

> 代码

### 一级缓存

~~~java
package com.southwind.test;
import com.southwind.entity.Account;
import com.southwind.repository.AccountRepository;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;import java.io.InputStream;
public class Test4 {
   public static void main(String[] args) {
        InputStream inputStream =
            Test.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new
            SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory =
            sqlSessionFactoryBuilder.build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        AccountRepository accountRepository =
            sqlSession.getMapper(AccountRepository.class);
        Account account = accountRepository.findById(1L);
        System.out.println(account);
       // Account account1 = accountRepository.findById(1L);
       // System.out.println(account1);
       // 上面这样写就只执行一次SQL查询,sqlSession的一级缓存
        sqlSession.close();
       // 一级缓存会被清空
        sqlSession = sqlSessionFactory.openSession();
        accountRepository = sqlSession.getMapper(AccountRepository.class);
        Account account1 = accountRepository.findById(1L);
       //执行第二次查询
        System.out.println(account1);
    }
}
~~~

### 二级缓存

#### 1、MyBatis 自带的二级缓存

- config.xml 配置开启二级缓存

~~~xml
<settings>
    <!-- 打印SQL-->
    <setting name="logImpl" value="STDOUT_LOGGING" />
    <!-- 开启延迟加载 -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!-- 开启二级缓存 -->
    <setting name="cacheEnabled" value="true"/>
</settings>
~~~

- Mapper.xml 中配置二级缓存

~~~xml
<!-- 一般这样写就行，但这样写因为没有缓存处理策略，所以实体类得实现序列化接口-->
<cache></cache>

<!-- 这样写可以进行参数设置 -->
<cache
 eviction="FIFO"         #缓存处理方式
 flushInterval="60000"   #刷新时间
 size="512"              #缓存数目大小
 readOnly="true"/>       #只读
这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。
~~~

- 实体类实现序列化接口

~~~java
package com.southwind.entity;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;import java.io.Serializable;
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Account implements Serializable {
    private long id;
    private String username;
    private String password;
    private int age;
}
~~~

> 测试代码

~~~java
package com.southwind.test;
import com.southwind.entity.Account;
import com.southwind.repository.AccountRepository;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;import java.io.InputStream;
public class Test4 {
   public static void main(String[] args) {
        InputStream inputStream =
            Test.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new
            SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory =
            sqlSessionFactoryBuilder.build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        AccountRepository accountRepository =
            sqlSession.getMapper(AccountRepository.class);
        Account account = accountRepository.findById(1L);
        System.out.println(account);
        sqlSession.close();
       // 一级缓存会被清空，二级缓存还在
        sqlSession = sqlSessionFactory.openSession();
        accountRepository = sqlSession.getMapper(AccountRepository.class);
        Account account1 = accountRepository.findById(1L);
        System.out.println(account1);
    }
}
~~~

> 结论

- 只要开启了二级缓存，我们在同一个Mapper中的查询，可以在二级缓存中拿到数据
- **查出的数据都会被默认先放在一级缓存中**
- **只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中**

#### 2、第三方 ehcache 二级缓存

- pom.xml 添加相关依赖

~~~xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache-core</artifactId>
    <version>2.4.3</version>
</dependency>
~~~

- 在 resources 中添加 ehcache.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
        updateCheck="false">
   <!--
      diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
      user.home – 用户主目录
      user.dir – 用户当前工作目录
      java.io.tmpdir – 默认临时文件路径
    -->
   <diskStore path="./tmpdir/Tmp_EhCache"/>
   
   <defaultCache
           eternal="false"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           diskPersistent="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="259200"
           memoryStoreEvictionPolicy="LRU"/>

   <cache
           name="cloud_user"
           eternal="false"
           maxElementsInMemory="5000"
           overflowToDisk="false"
           diskPersistent="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="1800"
           memoryStoreEvictionPolicy="LRU"/>
   <!--
      defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
    -->
   <!--
     name:缓存名称。
     maxElementsInMemory:缓存最大数目
     maxElementsOnDisk：硬盘最大缓存个数。
     eternal:对象是否永久有效，一但设置了，timeout将不起作用。
     overflowToDisk:是否保存到磁盘，当系统崩了时
     timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
     timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
     diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
     diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
     diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
     memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
     clearOnFlush：内存数量最大时是否清除。
     memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
     FIFO，first in first out，这个是大家最熟的，先进先出。
     LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
     LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
  -->

</ehcache>
~~~

- config.xml 配置开启二级缓存

~~~xml
<settings>
    <!-- 打印SQL-->
    <setting name="logImpl" value="STDOUT_LOGGING" />
    <!-- 开启延迟加载 -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!-- 开启二级缓存 -->
    <setting name="cacheEnabled" value="true"/>
</settings>
~~~

- Mapper.xml 中配置二级缓存

~~~xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache">
    <!-- 下面这些 property 标签可不写 -->
    <!-- 缓存创建之后，最后一次访问缓存的时间到缓存失效的时间间隔，以秒为单位 -->
    <property name="timeToIdleSeconds" value="3600"/>
    <!-- 缓存自创建时间起到失效的时间间隔 -->
    <property name="timeToLiveSeconds" value="3600"/>
    <!-- 缓存回收策略，LRU表示移除近期使用最少的对象 -->
    <property name="memoryStoreEvictionPolicy" value="LRU"/>
</cache>
~~~

- 实体类不需要实现序列化接口

~~~java
package com.southwind.entity;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Account {
    private long id;
    private String username;
    private String password;
    private int age;
}
~~~



## **MyBatis** 动态 **SQL**

**动态SQL指的是根据不同的查询条件 , 生成不同的Sql语句。**使用动态 SQL 可简化代码的开发，减少开发者的工作量，程序可以自动根据业务参数来决定 SQL 的组成。

AccountRepository . java

~~~java
public Account findByAccount(Account account);
~~~

#### if 标签

~~~xml
<select id="findByAccount" parameterType="com.southwind.entity.Account"
        resultType="com.southwind.entity.Account">
    select * from t_account where 
    <if test="id!=0">
        id = #{id}
    </if>
    <if test="username!=null">
        and username = #{username}
    </if>
    <if test="password!=null">
        and password = #{password}
    </if>
    <if test="age!=0">
        and age = #{age}
    </if>
</select>
~~~

if 标签可以自动根据表达式的结果来决定是否将对应的语句添加到 SQL 中，如果条件不成立则不添加，如果条件成立则添加。==（但是id必须设置，id不设置会查不出或者出异常）==

因为没有设置id，查询语句会变成 ==select  * from t_account  where  and username = #{username}==

> 测试

~~~java
    Account account = new Account();
    account.setId(1l);//必须设置
    account.setUsername("张三");
    System.out.println(accountRepository.findByAccount(account));
~~~

#### where 标签

~~~xml
<select id="findByAccount" parameterType="com.southwind.entity.Account"
        resultType="com.southwind.entity.Account">
    select * from t_account
    <where>
        <if test="id!=0">
            id = #{id}
        </if>
        <if test="username!=null">
            and username = #{username}
        </if>
        <if test="password!=null">
            and password = #{password}
        </if>
        <if test="age!=0">
            and age = #{age}
        </if>
    </where>
</select>
~~~

where 标签可以自动判断是否要删除语句块中的 and 或者 or 关键字，如果检测到 where 直接跟 and 或者 or 拼接，则自动删除 and 或者 or，通常情况下 if 和 where 结合起来使用。

> 测试

~~~java
  Account account = new Account();
//        account.setId(1l);  不用必须设置id了 
        account.setUsername("张三");
//        account.setAge(22);
        account.setPassword("123123");
        System.out.println(accountRepository.findByAccount(account));
~~~

#### choose 、when 标签

~~~xml
<select id="findByAccount" parameterType="com.southwind.entity.Account"
        resultType="com.southwind.entity.Account">
    select * from t_account
      <where>
              <if test="id!=0">
                    id=#{id}
               </if>
            <choose>
                <when test="username!=null">
                    and username=#{username}
                </when>
                <when test="password!=null">
                    and password=#{password}
                </when>
                <when test="age!=0">
                   and age=#{age}
                </when>
            </choose>
    </where>
</select>
~~~

choose相当于switch   ，   when相当于case   ，  otherwise相当于default，这样子的话choose标签就只能一个字段判断，多的其他字段不会进行判断，**只会选择第一个满足的语句块拼接，即使后面的语句块也满足判断条件**

> 测试

~~~java
Account account = new Account();
account.setId(5l);
account.setUsername("王五");
account.setAge(100);
System.out.println(accountRepository.findByAccount(account));
~~~

> 日志

~~~verilog
==>  Preparing: select * from t_account WHERE id=? and username=? 
==> Parameters: 5(Long), 王五(String)
~~~

==年龄 age 没有进行判断==

#### trim 标签

trim 标签中的 prefix 和 suffix 属性会被用于生成实际的 SQL 语句，会和标签内部的语句进行拼接，如果语句前后出现了 prefixOverrides 或者 suffixOverrides 属性中指定的值，MyBatis 框架会自动将其删除。

~~~xml
<select id="findByAccount" parameterType="com.southwind.entity.Account"
        resultType="com.southwind.entity.Account">
    select * from t_account
    <trim prefix="where" prefixOverrides="and|or">
        <if test="id!=0">
            id = #{id}
        </if>
        <if test="username!=null">
            and username = #{username}
        </if>
        <if test="password!=null">
            and password = #{password}
        </if>
        <if test="age!=0">
            and age = #{age}
        </if>
    </trim>
</select>
~~~

#### set 标签

set 标签用于 update 操作，会自动根据参数选择生成 SQL 语句，==生成的SQL只写需要修改的字段==。set 标签会自动去除多余的 “，”。

~~~xml
<update id="update" parameterType="com.southwind.entity.Account">
    update t_account
    <set>
        <if test="username!=null">
            username = #{username}, 
        </if>
        <if test="password!=null">
            password = #{password},
        </if>
        <if test="age!=0">
            age = #{age}
        </if>
    </set>
    where id = #{id}
</update>
~~~

> 测试

~~~java
Account account = new Account();
account.setId(5l);
account.setUsername("666");
account.setAge(100);
~~~

> 日志

~~~verilog
==>  Preparing: update t_account SET username=?, age=? where id=?; 
==> Parameters: 666(String), 100(Integer), 5(Long)
~~~

#### foreach 标签

foreach 标签可以迭代生成一系列值，这个标签主要用于 SQL 的 in 语句。

Account .java

```java
private List<Long> ids;
```

AccountRepository . java

```java
public List<Account> findByIds(Account account);
```

AccountRepositoryMapper .xml

~~~xml
<select id="findByIds" parameterType="com.southwind.entity.Account"
        resultType="com.southwind.entity.Account">
    select * from t_account
    <where>
       <foreach collection="ids" open="id in (" close=")" item="id" separator=",">
             #{id}   <!-- 与item相对应 -->
       </foreach>
    </where>
</select>
~~~

> 测试

~~~java
Account account = new Account();
List<Long> ids = new ArrayList<Long>();
ids.add(1l);
ids.add(2l);
ids.add(4l);
ids.add(5l);
account.setIds(ids);
System.out.println(accountRepository.findByIds(account));
~~~

> 日志

~~~verilog
==>  Preparing: select * from t_account WHERE id in ( ? , ? , ? , ? ) 
==> Parameters: 1(Long), 2(Long), 4(Long), 5(Long)
<==    Columns: id, username, password, age
<==        Row: 1, 张三, 123123, 22
<==        Row: 2, 李四, 123123, 25
<==        Row: 4, 小明, 12345, 11
<==        Row: 5, 王五, 8080, 90
~~~

#### 重用SQL片段

有时候可能某个 sql 语句我们用的特别多，为了增加代码的重用性，简化代码，我们需要将这些代码抽取出来，然后使用时直接调用。

**提取SQL片段：**

```xml
<sql id="if-title-author">
   <if test="title != null">
      title = #{title}
   </if>
   <if test="author != null">
      and author = #{author}
   </if>
</sql>
```

**引用SQL片段：**

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
  select * from blog
   <where>
 <!-- 引用 sql 片段，如果refid 指定的不在本文件中，那么需要在前面加上 namespace -->
       <include refid="if-title-author"></include>
       <!-- 在这里还可以引用其他的 sql 片段 -->
   </where>
</select>
```

注意：

①、最好基于单表操作来定义 sql 片段，提高片段的可重用性

②、在 **sql 片段中不要包括 where**