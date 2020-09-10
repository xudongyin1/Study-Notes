# 1、SpringBoot：Hello,World！

## 1.1、回顾什么是Spring

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson 。

**Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**



## 1.2、Spring是如何简化Java开发的

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1、基于POJO的轻量级和最小侵入性编程，所有东西都是bean；

2、通过IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；



## 1.3、什么是SpringBoot

学过javaweb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat, 跑出一个Hello Wolrld程序，是要经历特别多的步骤；后来就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；你们有经历过框架不断的演进，然后自己开发项目所有的技术也在不断的变化、改造吗？建议都可以去经历一遍；

言归正传，什么是SpringBoot呢，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开发，约定大于配置， you can "just run"，能迅速的开发web应用，几行代码开发一个http接口。

所有的技术框架的发展似乎都遵循了一条主线规律：从一个复杂应用场景 衍生 一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了优点；发展到一定程度之后，人们根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”，进而衍生出一些一站式的解决方案。

是的这就是Java企业级应用->J2EE->spring->springboot的过程。

随着 Spring 不断的发展，涉及的领域越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么易用简单，违背了最初的理念，甚至人称配置地狱。Spring Boot 正是在这样的一个背景下被抽象出来的开发框架，目的为了让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；

Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。

简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。

Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求



## 1.4、准备工作

我们将学习如何快速的创建一个Spring Boot应用，并且实现一个简单的Http请求处理。通过这个例子对Spring Boot有一个初步的了解，并体验其结构简单、开发快速的特性。

我的环境准备：

- java version "1.8.0_181"
- Maven-3.6.1
- SpringBoot 2.x 最新版

开发工具：

- IDEA

## 1.5、创建基础项目说明

Spring官方提供了非常方便的工具让我们快速构建应用

Spring Initializr：https://start.spring.io/

**项目创建方式一：**使用Spring Initializr 的 Web页面创建项目

1、打开 https://start.spring.io/

2、填写项目信息

3、点击”Generate Project“按钮生成项目；下载此项目

4、解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。

**项目创建方式二：**使用 IDEA 直接创建项目

1、创建一个新项目

2、选择spring initalizr ， 可以看到默认就是去官网的快速构建工具那里实现

3、填写项目信息

4、选择初始化的组件（初学勾选 Web 即可）

5、填写项目路径

6、等待项目构建成功

**项目结构分析：**

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类

2、一个 application.properties 配置文件

3、一个 测试类

4、一个 pom.xml

## 1.6、pom.xml 分析

打开pom.xml，看看Spring Boot项目的依赖：

```xml
<!-- 父依赖 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/>
</parent>

<dependencies>
    <!-- web场景启动器 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- springboot单元测试 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <!-- 剔除依赖 -->
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- 打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

## 1.7、编写一个http接口

1、在主程序的同级目录下，新建一个controller包，一定要在同级目录下，否则识别不到

2、在包中新建一个HelloController类

```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello() {
        return "Hello World";
    }
    
}
```

3、编写完毕后，从主程序启动项目，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412220856954-754704117.png)

简单几步，就完成了一个web接口的开发，SpringBoot就是这么简单。所以我们常用它来建立我们的微服务项目！

## 1.8、将项目打成jar包，点击 maven的 package

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412220931385-1725690944.png)

如果遇到以上错误，可以配置打包时 跳过项目运行测试用例

```xml
<!--
    在工作中,很多情况下我们打包是不想执行测试用例的
    可能是测试用例不完事,或是测试用例会影响数据库数据
    跳过测试用例执行
    -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <!--跳过项目运行测试用例-->
        <skipTests>true</skipTests>
    </configuration>
</plugin>
```



**彩蛋**

如何更改启动时显示的字符拼成的字母，SpringBoot呢？也就是 banner 图案；

只需一步：到项目下的 resources 目录下新建一个banner.txt 即可。

图案可以到：https://www.bootschool.net/ascii 这个网站生成，然后拷贝到文件中即可！

# 2、SpringBoot：运行原理初探

我们之前写的HelloSpringBoot，到底是怎么运行的呢，Maven项目，我们一般从pom.xml文件探究起；

> **pom.xml**

## 2.1、父依赖

其中它主要是依赖一个父项目，主要是管理项目的资源过滤及插件！

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

点进去，发现还有一个父依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

这里才是真正管理SpringBoot应用里面所有依赖版本的地方，SpringBoot的版本控制中心；

**以后我们导入依赖默认是不需要写版本；但是如果导入的包没有在依赖中管理着就需要手动配置版本了；**

## 2.2、启动器 spring-boot-starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**springboot-boot-starter-xxx**：就是spring-boot的场景启动器

**spring-boot-starter-web**：帮我们导入了web模块正常运行所依赖的组件；

SpringBoot将所有的功能场景都抽取出来，做成一个个的starter （启动器），只需要在项目中引入这些starter即可，所有相关的依赖都会导入进来 ， 我们要用什么功能就导入什么样的场景启动器即可 ；我们未来也可以自己自定义 starter；

> **主启动类**

分析完了 pom.xml 来看看这个启动类

## 2.3、默认的主启动类

```java
//@SpringBootApplication 来标注一个主程序类
//说明这是一个Spring Boot应用
@SpringBootApplication
public class SpringbootApplication {

   public static void main(String[] args) {
     //以为是启动了一个方法，没想到启动了一个服务
      SpringApplication.run(SpringbootApplication.class, args);
   }

}
```

但是**一个简单的启动类并不简单！**我们来分析一下这些注解都干了什么

## 2.4、@SpringBootApplication

作用：标注在某个类上说明这个类是SpringBoot的主配置类 ， SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

进入这个注解：可以看到上面还有很多其他注解！

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    // ......
}
```

## 2.5、@ComponentScan

这个注解在Spring中很重要 ,它对应XML配置中的元素。

作用：自动扫描并加载符合条件的组件或者bean ， 将这个bean定义加载到IOC容器中

## 2.6、@SpringBootConfiguration

作用：SpringBoot的配置类 ，标注在某个类上 ， 表示这是一个SpringBoot的配置类；

我们继续进去这个注解查看

```java
// 点@Configuration进去得到下面的 @Component
@Configuration
public @interface SpringBootConfiguration {}

@Component
public @interface Configuration {}
```

这里的 @Configuration，说明这是一个配置类 ，配置类就是对应Spring的xml 配置文件；

里面的 @Component 这就说明，启动类本身也是Spring中的一个组件而已，负责启动应用！

我们回到 SpringBootApplication 注解中继续看。

## 2.7、@EnableAutoConfiguration

**@EnableAutoConfiguration ：开启自动配置功能**

以前我们需要自己配置的东西，而现在SpringBoot可以自动帮我们配置 ；@EnableAutoConfiguration告诉SpringBoot开启自动配置功能，这样自动配置才能生效；

点进注解接续查看：

**@AutoConfigurationPackage ：自动配置包**

```java
@Import({Registrar.class})
public @interface AutoConfigurationPackage {
}
```

**@import** ：Spring底层注解@import ， 给容器中导入一个组件

Registrar.class 作用：将主启动类的所在包及包下面所有子包里面的所有组件扫描到Spring容器 ；

这个分析完了，退到上一步，继续看

**@Import({AutoConfigurationImportSelector.class}) ：给容器导入组件 ；**

AutoConfigurationImportSelector ：自动配置导入选择器，那么它会导入哪些组件的选择器呢？我们点击去这个类看源码：

1、这个类中有一个这样的方法

```java
// 获得候选的配置
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
    //这里的getSpringFactoriesLoaderFactoryClass（）方法
    //返回的就是我们最开始看的启动自动导入配置文件的注解类；EnableAutoConfiguration
    List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
    Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
    return configurations;
}
```

2、这个方法又调用了 SpringFactoriesLoader 类的静态方法！我们进入SpringFactoriesLoader类loadFactoryNames() 方法

```java
public static List<String> loadFactoryNames(Class<?> factoryClass, @Nullable ClassLoader classLoader) {
    String factoryClassName = factoryClass.getName();
    //这里它又调用了 loadSpringFactories 方法
    return (List)loadSpringFactories(classLoader).getOrDefault(factoryClassName, Collections.emptyList());
}
```

3、我们继续点击查看 loadSpringFactories 方法

```java
private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
    //获得classLoader ， 我们返回可以看到这里得到的就是EnableAutoConfiguration标注的类本身
    MultiValueMap<String, String> result = (MultiValueMap)cache.get(classLoader);
    if (result != null) {
        return result;
    } else {
        try {
            //去获取一个资源 "META-INF/spring.factories"
            Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
            LinkedMultiValueMap result = new LinkedMultiValueMap();

            //将读取到的资源遍历，封装成为一个Properties
            while(urls.hasMoreElements()) {
                URL url = (URL)urls.nextElement();
                UrlResource resource = new UrlResource(url);
                Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                Iterator var6 = properties.entrySet().iterator();

                while(var6.hasNext()) {
                    Entry<?, ?> entry = (Entry)var6.next();
                    String factoryClassName = ((String)entry.getKey()).trim();
                    String[] var9 = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
                    int var10 = var9.length;

                    for(int var11 = 0; var11 < var10; ++var11) {
                        String factoryName = var9[var11];
                        result.add(factoryClassName, factoryName.trim());
                    }
                }
            }

            cache.put(classLoader, result);
            return result;
        } catch (IOException var13) {
            throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var13);
        }
    }
}
```

4、发现一个多次出现的文件：spring.factories，全局搜索它

## 2.8、spring.factories

我们根据源头打开spring.factories ， 看到了很多自动配置的文件；这就是自动配置根源所在！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221052594-126548247.png)

**WebMvcAutoConfiguration**

我们在上面的自动配置类随便找一个打开看看，比如 ：WebMvcAutoConfiguration

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221108489-300000735.png)

可以看到这些一个个的都是JavaConfig配置类，而且都注入了一些Bean，可以找一些自己认识的类，看着熟悉一下！

所以，自动配置真正实现是从classpath中搜寻所有的META-INF/spring.factories配置文件 ，并将其中对应的 org.springframework.boot.autoconfigure. 包下的配置项，通过反射实例化为对应标注了 @Configuration的JavaConfig形式的IOC容器配置类 ， 然后将这些都汇总成为一个实例并加载到IOC容器中。

**结论：**

1. SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值
2. 将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；
3. 整个J2EE的整体解决方案和自动配置都在springboot-autoconfigure的jar包中；
4. 它会给容器中导入非常多的自动配置类 （xxxAutoConfiguration）, 就是给容器中导入这个场景需要的所有组件 ， 并配置好这些组件 ；
5. 有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；

**现在大家应该大概的了解了下，SpringBoot的运行原理，后面我们还会深化一次！**

> **SpringApplication**

## 2.9、不简单的方法

我最初以为就是运行了一个main方法，没想到却开启了一个服务；

```java
@SpringBootApplication
public class SpringbootApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootApplication.class, args);
    }
}
```

**SpringApplication.run分析**

分析该方法主要分两部分，一部分是SpringApplication的实例化，二是run方法的执行；

## 2.10、SpringApplication

**这个类主要做了以下四件事情：**

1、推断应用的类型是普通的项目还是Web项目

2、查找并加载所有可用初始化器 ， 设置到initializers属性中

3、找出所有的应用程序监听器，设置到listeners属性中

4、推断并设置main方法的定义类，找到运行的主类

查看构造器：

```java
public SpringApplication(ResourceLoader resourceLoader, Class... primarySources) {
    // ......
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    this.setInitializers(this.getSpringFactoriesInstances();
    this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = this.deduceMainApplicationClass();
}
```

## 2.11、run方法流程分析

![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20200309184347408-1065424525.png)
跟着源码和这幅图就可以一探究竟了！

# 3、SpringBoot：yaml配置注入

yaml语法学习

## 3.1、配置文件

SpringBoot使用一个全局的配置文件 ， 配置文件名称是固定的

- application.properties
- - 语法结构 ：key=value
- application.yml
- - 语法结构 ：key：空格 value

**配置文件的作用 ：**修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给我们自动配置好了；

比如我们可以在配置文件中修改Tomcat 默认启动的端口号！测试一下！

```properties
server.port=8081
```

## 3.2、yaml概述

YAML是 "YAML Ain't a Markup Language" （YAML不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）

**这种语言以数据\**\*\*作\*\**\*为中心，而不是以标记语言为重点！**

以前的配置文件，大多数都是使用xml来配置；比如一个简单的端口配置，我们来对比下yaml和xml

传统xml配置：

```xml
<server>
    <port>8081<port>
</server>
```

yaml配置：

```yml
server：
  prot: 8080
```

## 3.3、yaml基础语法

说明：语法要求严格！

1、空格不能省略

2、以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的。

3、属性和值的**大小写都是十分敏感**的。

**字面量：普通的值 [ 数字，布尔值，字符串 ]**

字面量直接写在后面就可以 ， **字符串默认不用加上双引号或者单引号**；

```yml
k: v
```

注意：

- “ ” **双引号，不会转义字符串里面的特殊字符** ， 特殊字符会作为本身想表示的意思；

  比如 ：name: "kuang \n shen" 输出 ：kuang 换行 shen

- '' **单引号，会转义特殊字符** ， 特殊字符最终会变成和普通字符一样输出

  比如 ：name: ‘kuang \n shen’ 输出 ：kuang \n shen

**对象、Map（键值对）**

```yml
#对象、Map格式
map:
 key1: value1
 key2: value2
```

在下一行来写对象的属性和值得关系，注意缩进；比如：

```yml
student:
    name: qinjiang
    age: 3
```

行内写法

```yml
student: {name: qinjiang,age: 3}
```

**数组（ List、set ）**

用 - 值表示数组中的一个元素,比如：

```yml
pets:
 - cat
 - dog
 - pig
```

行内写法

```yml
pets: [cat,dog,pig]
```

**修改SpringBoot的默认端口号**

配置文件中添加，端口号的参数，就可以切换端口；

```yml
server:  
  port: 8082
```

注入配置文件

yaml文件更强大的地方在于，他可以给我们的实体类直接注入匹配值！

## 3.4、yaml注入配置文件

1、在springboot项目中的resources目录下新建一个文件 application.yml

2、编写一个实体类 Dog；

```java
package com.kuang.springboot.pojo;

@Component  //注册bean到容器中
public class Dog {
    private String name;
    private Integer age;
    
    //有参无参构造、get、set方法、toString()方法  
}
```

3、思考，我们原来是如何给bean注入属性值的！@Value，给狗狗类测试一下：

```java
@Component //注册bean
public class Dog {
    @Value("阿黄")
    private String name;
    @Value("18")
    private Integer age;
}
```

4、在SpringBoot的测试类下注入狗狗输出一下；

```java
@SpringBootTest
class DemoApplicationTests {

    @Autowired //将狗狗自动注入进来
    Dog dog;

    @Test
    public void contextLoads() {
        System.out.println(dog); //打印看下狗狗对象
    }

}
```

结果成功输出，@Value注入成功，这是我们原来的办法对吧。

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221242434-877025455.png)

5、我们在编写一个复杂一点的实体类：Person 类

```java
@Component //注册bean到容器中
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
    
    //有参无参构造、get、set方法、toString()方法  
}
```

6、我们来使用yaml配置的方式进行注入，大家写的时候注意区别和优势，我们编写一个yaml配置！

```yml
person:
  name: qinjiang
  age: 3
  happy: false
  birth: 2000/01/01
  maps: {k1: v1,k2: v2}
  lists:
   - code
   - girl
   - music
  dog:
    name: 旺财
    age: 1
```

7、我们刚才已经把person这个对象的所有值都写好了，我们现在来注入到我们的类中！

```java
/*
@ConfigurationProperties作用：
将配置文件中配置的每一个属性的值，映射到这个组件中；
告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
参数 prefix = “person” : 将配置文件中的person下面的所有属性一一对应
*/
@Component //注册bean
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

8、IDEA 提示，springboot配置注解处理器没有找到，让我们看文档，我们可以查看文档，找到一个依赖！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221317864-79632654.png)

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221337189-113941879.png)

```xml
<!-- 导入配置文件处理器，配置文件进行绑定就会有提示，需要重启 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

9、确认以上配置都OK之后，我们去测试类中测试一下：

```java
@SpringBootTest
class DemoApplicationTests {

    @Autowired
    Person person; //将person自动注入进来

    @Test
    public void contextLoads() {
        System.out.println(person); //打印person信息
    }

}
```

结果：所有值全部注入成功！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221356464-178925188.png)

**yaml配置注入到实体类完全OK！**

## 3.5、加载指定的配置文件

**@PropertySource ：**加载指定的配置文件；

**@configurationProperties**：默认从全局配置文件中获取值；

1、我们去在resources目录下新建一个**person.properties**文件

```properties
name=kuangshen
```

2、然后在我们的代码中指定加载person.properties文件

```java
@PropertySource(value = "classpath:person.properties")
@Component //注册bean
public class Person {

    @Value("${name}")
    private String name;

    ......  
}
```

3、再次输出测试一下：指定配置文件绑定成功！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221412474-1138637819.png)

## 3.6、配置文件占位符

配置文件还可以编写占位符生成随机数

```yml
person:
    name: qinjiang${random.uuid} # 随机uuid
    age: ${random.int}  # 随机int
    happy: false
    birth: 2000/01/01
    maps: {k1: v1,k2: v2}
    lists:
      - code
      - girl
      - music
    dog:
      name: ${person.hello:other}_旺财 #如果person有hello属性，则前缀为hello的值，否则前缀为other
      age: 1
```

## 3.7、回顾properties配置

我们上面采用的yaml方法都是最简单的方式，开发中最常用的；也是springboot所推荐的！那我们来唠唠其他的实现方式，道理都是相同的；写还是那样写；配置文件除了yml还有我们之前常用的properties ， 我们没有讲，我们来唠唠！

【注意】properties配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8；

settings-->FileEncodings 中配置；

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221443954-1928760896.png)

**测试步骤：**

1、新建一个实体类User

```java
@Component //注册bean
public class User {
    private String name;
    private int age;
    private String sex;
}
```

2、编辑配置文件 user.properties

```properties
user1.name=kuangshen
user1.age=18
user1.sex=男
```

3、我们在User类上使用@Value来进行注入！

```java
@Component //注册bean
@PropertySource(value = "classpath:user.properties")
public class User {
    //直接使用@value
    @Value("${user.name}") //从配置文件中取值
    private String name;
    @Value("#{9*2}")  // #{SPEL} Spring表达式
    private int age;
    @Value("男")  // 字面量
    private String sex;
}
```

4、Springboot测试

```
user1.name=kuangshen
user1.age=18
user1.sex=男
```

结果正常输出：

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221511674-573127843.png)

## 3.8、对比小结

@Value这个使用起来并不友好！我们需要为每个属性单独注解赋值，比较麻烦；我们来看个功能对比图

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221528114-1258238964.png)

1、@ConfigurationProperties只需要写一次即可 ， @Value则需要每个字段都添加

2、松散绑定：这个什么意思呢? 比如我的yml中写的last-name，这个和lastName是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下

3、JSR303数据校验 ， 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性

4、复杂类型封装，yml中可以封装对象 ， 使用value就不支持

**结论：**

配置yml和配置properties都可以获取到值 ， **强烈推荐 yml**；

如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；

如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！

# 4、SpringBoot：JSR303数据校验及多环境切换

JSR303数据校验

## 4.1、先看看如何使用

Springboot中可以用@validated来校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处理。我们这里来写个注解让我们的name只能支持Email格式；

```java
@Component //注册bean
@ConfigurationProperties(prefix = "person")
@Validated  //数据校验
public class Person {

    @Email(message="邮箱格式错误") //这样子name必须是邮箱格式
    private String name;
}
```

运行结果 ：default message [不是一个合法的电子邮件地址];

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221556237-391670975.png)

**使用数据校验，可以保证数据的正确性；**

## 4.2、常见参数

```java
@NotNull(message="名字不能为空")
private String userName;
@Max(value=120,message="年龄最大不能查过120")
private int age;
@Email(message="邮箱格式错误")
private String email;

空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.
    
Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false  
    
长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) 验证字符串长度是否在给定范围内

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等
除此以外，我们还可以自定义一些数据校验规则
```

多环境切换

profile是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

## 4.3、多配置文件

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml , 用来指定多个环境版本；

**例如：**

application-test.properties 代表测试环境配置

application-dev.properties 代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**；

我们需要通过一个配置来选择需要激活的环境：

```properties
#比如在配置文件中指定使用dev环境，我们可以通过设置不同的端口号进行测试；
#我们启动SpringBoot，就可以看到已经切换到dev下的配置了；
spring.profiles.active=dev
```

## 4.4、yaml的多文档块

和properties配置文件中一样，但是使用yml去实现不需要创建多个配置文件，更加方便了 !

```yml
server:
  port: 8081
#选择要激活那个环境块
spring:
  profiles:
    active: prod

---
server:
  port: 8083
spring:
  profiles: dev #配置环境的名称


---

server:
  port: 8084
spring:
  profiles: prod  #配置环境的名称
```

**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！**

## 4.5、配置文件加载位置

**外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！**

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件：

```xml
优先级1：项目路径下的config文件夹配置文件
优先级2：项目路径下配置文件
优先级3：resources资源路径下的config文件夹配置文件
优先级4：resources资源路径下配置文件
```

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

**SpringBoot会从这四个位置全部加载主配置文件；互补配置；**

我们在最低级的配置文件中设置一个项目访问路径的配置来测试互补问题；

```properties
#配置项目的访问路径
server.servlet.context-path=/kuang
```

## 4.6、拓展，运维小技巧

指定位置加载配置文件

我们还可以通过spring.config.location来改变默认的配置文件位置

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；这种情况，一般是后期运维做的多，**相同配置，外部指定的配置文件优先级最高**

# 5、SpringBoot：自动配置原理

自动配置原理

配置文件到底能写什么？怎么写？

SpringBoot官方文档中有大量的配置，我们无法全部记住

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221646159-343420531.png)

## 5.1、分析自动配置原理

我们以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；

```java
//表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
@Configuration 

//启动指定类的ConfigurationProperties功能；
  //进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
  //并把HttpProperties加入到ioc容器中
@EnableConfigurationProperties({HttpProperties.class}) 

//Spring底层@Conditional注解
  //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
  //这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass({CharacterEncodingFilter.class})

//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
  //如果不存在，判断也是成立的
  //即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)

public class HttpEncodingAutoConfiguration {
    //他已经和SpringBoot的配置文件映射了
    private final Encoding properties;
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }
    
    //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @Bean
    @ConditionalOnMissingBean //判断容器有没有这个组件，没有则执行下面方法
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }
    //。。。。。。。
}
```

**一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！**

- 一但这个配置类生效；这个配置类就会给容器中添加各种组件；
- 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
- 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；
- 配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(prefix = "spring.http") 
public class HttpProperties {
    // .....
}
```

我们去配置文件里面试试前缀，看提示！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221709469-1927678332.png)

**这就是自动装配的原理！**

## 5.2、精髓

1、SpringBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**

## 5.3、了解：@Conditional

了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置里面的所有内容才生效；

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221747954-591282661.png)

**那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是所有的都生效了。**

我们怎么知道哪些自动配置类生效？

**我们可以通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；**

```properties
#开启springboot的调试类
debug=true
```

**Positive matches:（自动配置类启用的：正匹配）**

**Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）**

**Unconditional classes: （没有条件的类）**

【演示：查看输出的日志】

# 6、SpringBoot：自定义starter

> **为什么要自定义starter**

　　在我们的日常开发工作中，经常会有一些独立于业务之外的配置模块，我们经常将其放到一个特定的包下，然后如果另一个工程需要复用这块功能的时候，需要将代码硬拷贝到另一个工程，重新集成一遍，麻烦至极。如果我们将这些可独立于业务代码之外的功配置模块封装成一个个starter，复用的时候只需要将其在pom中引用依赖即可，SpringBoot为我们完成自动装配，简直不要太爽。

## 6.1、说明

启动器模块是一个 空 jar（**就是我们下面创建的maven项目，相当于一个索引，这样子maven可以引入其他模块功能，如果只创建springboot项目的话就不太好扩展功能**） 文件，仅提供辅助性依赖管理，这些依赖可能用于自动装配或者其他类库；

**命名归约：**

官方命名：

- 前缀：spring-boot-starter-xxx
- 比如：spring-boot-starter-web....

自定义命名：

- xxx-spring-boot-starter
- 比如：mybatis-spring-boot-starter

## 6.2、编写启动器

1、在IDEA中新建一个空项目 spring-boot-starter-diy

2、新建一个**普通Maven模块**：kuang-spring-boot-starter

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221856069-1152361375.png)

3、新建一个**Springboot模块**：kuang-spring-boot-starter-autoconfigure

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412221930344-363603356.png)

4、点击apply即可，基本结构

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412222010544-802039453.png)

5、在我们的 starter 中 导入 autoconfigure 的依赖！

```xml
<!-- 启动器 -->
<dependencies>
    <!--  引入自动配置模块 -->
    <dependency>
        <groupId>com.kuang</groupId>
        <artifactId>kuang-spring-boot-starter-autoconfigure</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </dependency>
</dependencies>
```

6、将 autoconfigure 项目下多余的文件都删掉（test文件也删掉），Pom中只留下一个 starter（build也去掉），这是所有的启动器基本配置！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412222056275-508024261.png)

```xml
<!-- 导入消除注解警告提示 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

7、在 autoconfigure 项目下，我们编写一个自己的服务

```java
package com.kuang;

public class HelloService {

    HelloProperties helloProperties;

    public HelloProperties getHelloProperties() {
        return helloProperties;
    }

    public void setHelloProperties(HelloProperties helloProperties) {
        this.helloProperties = helloProperties;
    }

    public String sayHello(String name){
        return helloProperties.getPrefix() + name + helloProperties.getSuffix();
    }

}
```

8、在 autoconfigure 项目下，编写HelloProperties 配置类

```java
package com.kuang;

import org.springframework.boot.context.properties.ConfigurationProperties;

// 前缀 kuang.hello
@ConfigurationProperties(prefix = "kuang.hello")
public class HelloProperties {

    private String prefix;
    private String suffix;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getSuffix() {
        return suffix;
    }

    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}
```

9、在 autoconfigure 项目下，编写我们的自动配置类并注入bean，测试！

```java
package com.kuang;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConditionalOnWebApplication //web应用生效
@EnableConfigurationProperties(HelloProperties.class)
public class HelloServiceAutoConfiguration {

    @Autowired
    HelloProperties helloProperties;

    @Bean
    public HelloService helloService(){
        HelloService service = new HelloService();
        service.setHelloProperties(helloProperties);
        return service;
    }

}
```

10、在 autoconfigure 项目下，在resources编写一个自己的 META-INF\spring.factories

```xml
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.kuang.HelloServiceAutoConfiguration
```

11、编写完成后，可以安装到maven仓库中！**先安装autoconfigure 项目，再安装starter项目**

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412223705466-546919127.png)

我们也可以只建一个springboot项目，然后跟上面一样pom配置，删掉一些冗余的东西，编写好业务代码，然后直接maven安装jar包到本地仓库，新建springboot项目导入该jar包依赖（**注意版本快照号**），进行starter设置，进行测试成功。

## 6.3、新建项目测试我们自己写的启动器

1、新建一个SpringBoot 项目

2、导入我们自己写的启动器

```xml
<dependency>
    <groupId>com.kuang</groupId>
    <artifactId>kuang-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

3、编写一个 HelloController 进行测试我们自己的写的接口！

```java
package com.kuang.controller;

@RestController
public class HelloController {

    @Autowired
    HelloService helloService;

    @RequestMapping("/hello")
    public String hello(){
        return helloService.sayHello("zxc");
    }

}
```

4、编写配置文件 application.properties

```java
kuang.hello.prefix="ppp"
kuang.hello.suffix="sss"
```

5、启动项目进行测试，结果成功 !

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412222355347-1685568990.png)

# 7、SpringBoot整合JDBC

## 7.1、SpringData简介

对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring Data 的方式进行统一处理。

Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring Boot、Spring Cloud 等齐名的知名项目。

Sping Data 官网：https://spring.io/projects/spring-data

数据库相关的启动器 ：可以参考官方文档：

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

整合JDBC

## 7.2、创建测试项目测试数据源

1、我去新建一个项目测试：springboot-data-jdbc ; 引入相应的模块！基础模块

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412223600851-874349014.png)

2、项目建好之后，发现自动帮我们导入了如下的启动器

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

3、编写yaml配置文件连接数据库；

```yml
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
```

4、配置完这一些东西后，我们就可以直接去使用了，因为SpringBoot已经默认帮我们进行了自动配置；去测试类测试一下

```java
@SpringBootTest
class SpringbootDataJdbcApplicationTests {

    //DI注入数据源
    @Autowired
    DataSource dataSource;

    @Test
    public void contextLoads() throws SQLException {
        //看一下默认数据源
        System.out.println(dataSource.getClass());
        //获得连接
        Connection connection =   dataSource.getConnection();
        System.out.println(connection);
        //关闭连接
        connection.close();
    }
}
```

结果：我们可以看到他默认给我们配置的数据源为 : class com.zaxxer.hikari.HikariDataSource ， 我们并没有手动配置

我们来全局搜索一下，找到数据源的所有自动配置都在 ：DataSourceAutoConfiguration文件：

```java
@Import(
    {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class}
)
protected static class PooledDataSourceConfiguration {
    protected PooledDataSourceConfiguration() {
    }
}
```

这里导入的类都在 DataSourceConfiguration 配置类下，可以看出 Spring Boot 2.2.5 默认使用HikariDataSource 数据源，而以前版本，如 Spring Boot 1.5 默认使用 org.apache.tomcat.jdbc.pool.DataSource 作为数据源；

**HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀；**

**可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。**

关于数据源我们并不做介绍，有了数据库连接，显然就可以 CRUD 操作数据库了。但是我们需要先了解一个对象 JdbcTemplate

## 7.3、JDBCTemplate

1、有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；

2、即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。

3、数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。

4、Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用

5、JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类

**JdbcTemplate主要提供以下几类方法：**

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
- query方法及queryForXXX方法：用于执行查询相关语句；
- call方法：用于执行存储过程、函数相关语句。

## 7.4、测试

编写一个Controller，注入 jdbcTemplate，编写测试方法进行访问测试；

```java
package com.kuang.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;
import java.util.List;
import java.util.Map;

@RestController
@RequestMapping("/jdbc")
public class JdbcController {

    /**
     * Spring Boot 默认提供了数据源，默认提供了 org.springframework.jdbc.core.JdbcTemplate
     * JdbcTemplate 中会自己注入数据源，用于简化 JDBC操作
     * 还能避免一些常见的错误,使用起来也不用再自己来关闭数据库连接
     */
    @Autowired
    JdbcTemplate jdbcTemplate;

    //查询employee表中所有数据
    //List 中的1个 Map 对应数据库的 1行数据
    //Map 中的 key 对应数据库的字段名，value 对应数据库的字段值
    @GetMapping("/list")
    public List<Map<String, Object>> userList(){
        String sql = "select * from employee";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }
    
    //新增一个用户
    @GetMapping("/add")
    public String addUser(){
        //插入语句，注意时间问题
        String sql = "insert into employee(last_name, email,gender,department,birth)" +
                " values ('狂神说','24736743@qq.com',1,101,'"+ new Date().toLocaleString() +"')";
        jdbcTemplate.update(sql);
        //查询
        return "addOk";
    }

    //修改用户信息
    @GetMapping("/update/{id}")
    public String updateUser(@PathVariable("id") int id){
        //插入语句
        String sql = "update employee set last_name=?,email=? where id="+id;
        //数据
        Object[] objects = new Object[2];
        objects[0] = "秦疆";
        objects[1] = "24736743@sina.com";
        jdbcTemplate.update(sql,objects);
        //查询
        return "updateOk";
    }

    //删除用户
    @GetMapping("/delete/{id}")
    public String delUser(@PathVariable("id") int id){
        //插入语句
        String sql = "delete from employee where id=?";
        jdbcTemplate.update(sql,id);
        //查询
        return "deleteOk";
    }
    
}
```

测试请求，结果正常；

到此，CURD的基本操作，使用 JDBC 就搞定了。https://docs.spring.io/spring-boot/docs/2.0.4.RELEASE/reference/htmlsingle/#using-boot-starter)



## 7.5、原理探究 ：

org.springframework.boot.autoconfigure.jdbc.DataSourceConfiguration 数据源配置类作用 ：根据逻辑判断之后，添加数据源；

**SpringBoot默认支持以下数据源：**

**com.zaxxer.hikari.HikariDataSource （Spring Boot 2.0 以上，默认使用此数据源）**

org.apache.tomcat.jdbc.pool.DataSource

org.apache.commons.dbcp2.BasicDataSource

**可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。默认情况下，它是从类路径自动检测的。**

```java
@Configuration
@ConditionalOnMissingBean({DataSource.class})
@ConditionalOnProperty(
    name = {"spring.datasource.type"}
)
static class Generic {
    Generic() {
    }

    @Bean
    public DataSource dataSource(DataSourceProperties properties) {
        return properties.initializeDataSourceBuilder().build();
    }
}
```

# 8、SpringBoot整合Druid

## 8.1、Druid简介

Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。

Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。

Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。

Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。

Spring Boot 2.0 以上默认使用 Hikari 数据源，可以说 Hikari 与 Driud 都是当前 Java Web 上最优秀的数据源，我们来重点介绍 Spring Boot 如何集成 Druid 数据源，如何实现数据库监控。

Github地址：https://github.com/alibaba/druid/

**com.alibaba.druid.pool.DruidDataSource 基本配置参数如下：**

| 配置                          | 缺省值             | 说明                                                         |
| ----------------------------- | ------------------ | ------------------------------------------------------------ |
| name                          |                    | 配置这个属性的意义在于，如果存在多个数据源，监控的时候可以通过名字来区分开来。 如果没有配置，将会生成一个名字，格式是："DataSource-" + System.identityHashCode(this). 另外配置此属性至少在1.0.5版本中是不起作用的，强行设置name会出错 。 |
| url                           |                    | 连接数据库的url，不同数据库不一样。例如： mysql : jdbc:mysql://10.20.153.104:3306/druid2 oracle : jdbc:oracle:thin:@10.20.149.85:1521:ocnauto |
| username                      |                    | 连接数据库的用户名                                           |
| password                      |                    | 连接数据库的密码。如果你不希望密码直接写在配置文件中，可以使用ConfigFilter。详细看这里：https://github.com/alibaba/druid/wiki/使用ConfigFilter |
| driverClassName               | 根据url自动识别    | 这一项可配可不配，如果不配置druid会根据url自动识别dbType，然后选择相应的driverClassName |
| initialSize                   | 0                  | 初始化时建立物理连接的个数。初始化发生在显示调用init方法，或者第一次getConnection时 |
| maxActive                     | 8                  | 最大连接池数量                                               |
| maxIdle                       | 8                  | 已经不再使用，配置了也没效果                                 |
| minIdle                       |                    | 最小连接池数量                                               |
| maxWait                       |                    | 获取连接时最大等待时间，单位毫秒。配置了maxWait之后，缺省启用公平锁，并发效率会有所下降，如果需要可以通过配置useUnfairLock属性为true使用非公平锁。 |
| poolPreparedStatements        | false              | 是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。在mysql下建议关闭。 |
| maxOpenPreparedStatements     | -1                 | 要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100 |
| validationQuery               |                    | 用来检测连接是否有效的sql，要求是一个查询语句。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会其作用。 |
| validationQueryTimeout        |                    | 单位：秒，检测连接是否有效的超时时间。底层调用jdbc Statement对象的void setQueryTimeout(int seconds)方法 |
| testOnBorrow                  | true               | 申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。 |
| testOnReturn                  | false              | 归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能 |
| testWhileIdle                 | false              | 建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。 |
| timeBetweenEvictionRunsMillis | 1分钟（1.0.14）    | 有两个含义： 1) Destroy线程会检测连接的间隔时间，如果连接空闲时间大于等于minEvictableIdleTimeMillis则关闭物理连接 2) testWhileIdle的判断依据，详细看testWhileIdle属性的说明 |
| numTestsPerEvictionRun        |                    | 不再使用，一个DruidDataSource只支持一个EvictionRun           |
| minEvictableIdleTimeMillis    | 30分钟（1.0.14）   | 连接保持空闲而不被驱逐的最长时间                             |
| connectionInitSqls            |                    | 物理连接初始化的时候执行的sql                                |
| exceptionSorter               | 根据dbType自动识别 | 当数据库抛出一些不可恢复的异常时，抛弃连接                   |
| filters                       |                    | 属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有： 监控统计用的filter:stat 日志用的filter:log4j 防御sql注入的filter:wall |
| proxyFilters                  |                    | 类型是List<com.alibaba.druid.filter.Filter>，如果同时配置了filters和proxyFilters，是组合关系，并非替换关系 |

## 8.2、配置数据源

1、添加上 Druid 数据源依赖。

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.23</version>
</dependency>
```

2、切换数据源；之前已经说过 Spring Boot 2.0 以上默认使用 com.zaxxer.hikari.HikariDataSource 数据源，但可以 通过 spring.datasource.type 指定数据源。

```yml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源
```

3、数据源切换之后，在测试类中注入 DataSource，然后获取到它，输出一看便知是否成功切换；

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412223942091-598892546.png)

4、切换成功！既然切换成功，就可以设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；可以查看源码

```yml
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

5、导入Log4j 的依赖

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

6、现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 Spring Boot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；

```java
package com.kuang.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class DruidConfig {

    /*
       将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
       绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
       @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局配置文件中
       前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中
     */
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource() {
        return new DruidDataSource();
    }

}
```

7、去测试类中测试一下；看是否成功！

```java
@SpringBootTest
class SpringbootDataJdbcApplicationTests {

    //DI注入数据源
    @Autowired
    DataSource dataSource;

    @Test
    public void contextLoads() throws SQLException {
        //看一下默认数据源
        System.out.println(dataSource.getClass());
        //获得连接
        Connection connection =   dataSource.getConnection();
        System.out.println(connection);

        DruidDataSource druidDataSource = (DruidDataSource) dataSource;
        System.out.println("druidDataSource 数据源最大连接数：" + druidDataSource.getMaxActive());
        System.out.println("druidDataSource 数据源初始化连接数：" + druidDataSource.getInitialSize());

        //关闭连接
        connection.close();
    }
}
```

输出结果 ：可见配置参数已经生效！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224035706-1280927557.png)

## 8.3、配置Druid数据源监控

Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看，类似安装 路由器 时，人家也提供了一个默认的 web 页面。

所以第一步需要设置 Druid 的后台管理页面，比如 登录账号、密码 等；配置后台管理；

```java
//配置 Druid 监控管理后台的Servlet；
//内置 Servlet 容器时没有web.xml文件，所以使用 Spring Boot 的注册 Servlet 方式
@Bean
public ServletRegistrationBean statViewServlet() {
    ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");

    // 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet 
    // 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到
    Map<String, String> initParams = new HashMap<>();
    initParams.put("loginUsername", "admin"); //后台管理界面的登录账号
    initParams.put("loginPassword", "123456"); //后台管理界面的登录密码

    //后台允许谁可以访问
    //initParams.put("allow", "localhost")：表示只有本机可以访问
    //initParams.put("allow", "")：为空或者为null时，表示允许所有访问
    initParams.put("allow", "");
    //deny：Druid 后台拒绝谁访问
    //initParams.put("kuangshen", "192.168.1.20");表示禁止此ip访问

    //设置初始化参数
    bean.setInitParameters(initParams);
    return bean;
}
```

配置完毕后，我们可以选择访问 ：http://localhost:8080/druid/login.html

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224112471-646829041.png)

进入之后

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224131010-12963631.png)

**配置 Druid web 监控 filter 过滤器**

```java
//配置 Druid 监控 之  web 监控的 filter
//WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
@Bean
public FilterRegistrationBean webStatFilter() {
    FilterRegistrationBean bean = new FilterRegistrationBean();
    bean.setFilter(new WebStatFilter());

    //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
    Map<String, String> initParams = new HashMap<>();
    initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
    bean.setInitParameters(initParams);

    //"/*" 表示过滤所有请求
    bean.setUrlPatterns(Arrays.asList("/*"));
    return bean;
}
```

平时在工作中，按需求进行配置即可，主要用作监控

# 9、SpringBoot 整合mybatis

## **9.1、导入mybatis所需要的依赖**

```xml
<!-- 引入 myBatis，这是 MyBatis官方提供的适配 Spring Boot 的，而不是Spring Boot自己的-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.0</version>
</dependency>
```

## **9.2、配置数据库连接信息**

```properties
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

## 9.3、我们这里就是用默认的数据源了；先去测试一下连接是否成功！

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringbootDemoMybatisApplicationTests {

    @Autowired
    DataSource dataSource;

    @Test
    public void contextLoads() throws SQLException {

        System.out.println("数据源>>>>>>" + dataSource.getClass());
        Connection connection = dataSource.getConnection();
        System.out.println("连接>>>>>>>>>" + connection);
        System.out.println("连接地址>>>>>" + connection.getMetaData().getURL());
        connection.close();

    }

}
```

**查看输出结果，数据库配置OK！**

## **9.4、创建实体类**

```java
package com.kuang.mybatis.pojo;

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

## 9.5、配置Mapper接口类

```java
package com.kuang.mybatis.pojo.mapper;

import com.kuang.mybatis.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;

//@Mapper : 表示本类是一个 MyBatis 的 Mapper，等价于以前 Spring 整合 MyBatis 时的 Mapper 接口
@Mapper
@Repository
public interface UserMapper {

    //选择全部用户
    List<User> selectUser();
    //根据id选择用户
    User selectUserById(int id);
    //添加一个用户
    int addUser(User user);
    //修改一个用户
    int updateUser(User user);
    //根据id删除用户
    int deleteUser(int id);

}
```

## 9.6、对应Mapper映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.kuang.mybatis.pojo.mapper.UserMapper">

    <select id="selectUser" resultType="User">
        select * from user
    </select>

    <select id="selectUserById" resultType="User">
        select * from user where id = #{id}
    </select>

    <insert id="addUser" parameterType="User">
        insert into user (id,name,pwd) values (#{id},#{name},#{pwd})
    </insert>

    <update id="updateUser" parameterType="User">
        update user set name=#{name},pwd=#{pwd} where id = #{id}
    </update>

    <delete id="deleteUser" parameterType="int">
        delete from user where id = #{id}
    </delete>
</mapper>
```

## 9.7、maven配置资源过滤问题

```xml
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.xml</include>
        </includes>
        <filtering>true</filtering>
    </resource>
</resources>
```

## 9.8、SpringBoot 整合！

以前 MyBatis 未与 spring 整合时，配置数据源、事务、连接数据库的账号、密码等都是在 myBatis 核心配置文件中进行的myBatis 与 spring 整合后，配置数据源、事务、连接数据库的账号、密码等就交由 spring 管理。因此，在这里我们即使不使用mybatis配置文件也完全ok！
**既然已经提供了 myBatis 的映射配置文件，自然要告诉 spring boot 这些文件的位置**

```properties
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.driver-class-name=com.mysql.jdbc.Driver

#指定myBatis的核心配置文件与Mapper映射文件
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
# 注意：对应实体类的路径
mybatis.type-aliases-package=com.kuang.mybatis.pojo
```

已经说过 spring boot 官方并没有提供 myBaits 的启动器，是 myBatis 官方提供的开发包来适配的 spring boot，从 pom.xml 文件中的依赖包名也能看出来，并非是以 spring-boot 开头的；

同理上面全局配置文件中的这两行配置也是以 mybatis 开头 而非 spring 开头也充分说明这些都是 myBatis 官方提供的

可以从 org.mybatis.spring.boot.autoconfigure.MybatisProperties 中查看所有配置项

```java
@ConfigurationProperties(
    prefix = "mybatis"
)
public class MybatisProperties {
    public static final String MYBATIS_PREFIX = "mybatis";
    private static final ResourcePatternResolver resourceResolver = new PathMatchingResourcePatternResolver();
    private String configLocation;
    private String[] mapperLocations;
    private String typeAliasesPackage;
    private Class<?> typeAliasesSuperType;
    private String typeHandlersPackage;
    private boolean checkConfigLocation = false;
    private ExecutorType executorType;
    private Class<? extends LanguageDriver> defaultScriptingLanguageDriver;
    private Properties configurationProperties;
    @NestedConfigurationProperty
    private Configuration configuration;
```

也可以直接去查看 [官方文档](http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)

## 9.9、编写controller

```java
package com.kuang.mybatis.controller;

import com.kuang.mybatis.pojo.User;
import com.kuang.mybatis.pojo.mapper.UserMapper;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class UserController {

    @Autowired
    private UserMapper userMapper;

    //选择全部用户
    @GetMapping("/selectUser")
    public String selectUser(){
        List<User> users = userMapper.selectUser();
        for (User user : users) {
            System.out.println(user);
        }

        return "ok";
    }
    //根据id选择用户
    @GetMapping("/selectUserById")
    public String selectUserById(){
        User user = userMapper.selectUserById(1);
        System.out.println(user);
        return "ok";
    }
    //添加一个用户
    @GetMapping("/addUser")
    public String addUser(){
        userMapper.addUser(new User(5,"阿毛","456789"));
        return "ok";
    }
    //修改一个用户
    @GetMapping("/updateUser")
    public String updateUser(){
        userMapper.updateUser(new User(5,"阿毛","421319"));
        return "ok";
    }
    //根据id删除用户
    @GetMapping("/deleteUser")
    public String deleteUser(){
        userMapper.deleteUser(5);
        return "ok";
    }

}
```

## **9.10、启动项目访问进行测试！**

**步骤：**

Mybatis整合包

mybatis-spring-boot-starter

1.导入包

2.配置文件

3.mybatis配置

4.编写sql

5.service层调用dao层

6.controller调用service层

## **注：配置数据库连接信息（不变）**

```yml
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址： https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

# 10、SpringBoot：Web开发静态资源处理

Web开发探究

## 10.1、简介

其实SpringBoot的东西用起来非常简单，因为SpringBoot最大的特点就是自动装配。

**使用SpringBoot的步骤：**

1、创建一个SpringBoot应用，选择我们需要的模块，SpringBoot就会默认将我们的需要的模块自动配置好

2、手动在配置文件中配置部分配置项目就可以运行起来了

3、专注编写业务代码，不需要考虑以前那样一大堆的配置了。

要熟悉掌握开发，之前学习的自动配置的原理一定要搞明白！

比如SpringBoot到底帮我们配置了什么？我们能不能修改？我们能修改哪些配置？我们能不能扩展？

- 向容器中自动配置组件 ：*** Autoconfiguration
- 自动配置类，封装配置文件的内容：***Properties

没事就找找类，看看自动装配原理！

静态资源处理

## 10.2、静态资源映射规则

**首先，我们搭建一个普通的SpringBoot项目，回顾一下HelloWorld程序！**

写请求非常简单，那我们要引入我们前端资源，我们项目中有许多的静态资源，比如css，js等文件，这个SpringBoot怎么处理呢？

如果我们是一个web应用，我们的main下会有一个webapp，我们以前都是将所有的页面导在这里面的，对吧！但是我们现在的pom呢，打包方式是为jar的方式，那么这种方式SpringBoot能不能来给我们写页面呢？当然是可以的，但是SpringBoot对于静态资源放置的位置，是有规定的！

**我们先来聊聊这个静态资源映射规则：**

SpringBoot中，SpringMVC的web配置都在 WebMvcAutoConfiguration 这个配置类里面；

我们可以去看看 WebMvcAutoConfigurationAdapter 中有很多配置方法；

有一个方法：addResourceHandlers 添加资源处理

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    //如果自己在properties中定义了静态资源扫描路径
    if (!this.resourceProperties.isAddMappings()) {
        // 禁用默认资源处理
        logger.debug("Default resource handling disabled");
        return;
    }
    // 缓存控制
    Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
    CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
    // webjars 配置，需要导入webjars的依赖
    if (!registry.hasMappingForPattern("/webjars/**")) {
        customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
                                             .addResourceLocations("classpath:/META-INF/resources/webjars/")
                                             .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
    // 静态资源配置
    String staticPathPattern = this.mvcProperties.getStaticPathPattern();
    if (!registry.hasMappingForPattern(staticPathPattern)) {
        customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
                                             .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
                                             .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
}
```

读一下源代码：比如所有的 /webjars/** ， 都需要去 classpath:/META-INF/resources/webjars/ 找对应的资源；

## 10.3、什么是webjars 呢？

Webjars本质就是以jar包的方式引入我们的静态资源 ， 我们以前要导入一个静态资源文件，直接导入即可。

使用SpringBoot需要使用Webjars，我们可以去搜索一下：

网站：[https://www.webjars.org](https://www.webjars.org/)

要使用jQuery，我们只要要引入jQuery对应版本的pom依赖即可！

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.1</version>
</dependency>
```

导入完毕，查看webjars目录结构，并访问Jquery.js文件！

<img src="https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224229426-796441519.png" alt="img" style="zoom:50%;" />

访问：只要是静态资源，SpringBoot就会去对应的路径寻找资源，我们这里访问：http://localhost:8080/webjars/jquery/3.4.1/jquery.js

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224306836-344577320.png)

## 10.4、第二种静态资源映射规则

那我们项目中要是使用自己的静态资源该怎么导入呢？我们看下一行代码；

我们去找staticPathPattern发现第二种映射规则 ：/** , 访问当前的项目任意资源，它会去找 resourceProperties 这个类，我们可以点进去看一下分析：

```java
// 进入方法
public String[] getStaticLocations() {
    return this.staticLocations;
}
// 找到对应的值
private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
// 找到路径
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { 
    "classpath:/META-INF/resources/",
  "classpath:/resources/", 
    "classpath:/static/", 
    "classpath:/public/" 
};
```

ResourceProperties 可以设置和我们静态资源有关的参数；这里面指向了它会去寻找资源的文件夹，即上面数组的内容。

所以得出结论，以下四个目录存放的静态资源可以被我们识别：

```java
"classpath:/META-INF/resources/"  //根目录resources
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
 // 优先级：resources>static>public
```

我们可以在resources根目录下新建对应的文件夹，都可以存放我们的静态文件；

比如我们访问 http://localhost:8080/1.js , 他就会去这些文件夹中寻找对应的静态资源文件；

## 10.5、自定义静态资源路径

我们也可以自己通过配置文件来指定一下，哪些文件夹是需要我们放静态资源文件的，在application.properties中配置；

```properties
spring.resources.static-locations=classpath:/coding/,classpath:/kuang/
```

一旦自己定义了静态文件夹的路径，**原来的自动路径配置就都会失效了**！

首页处理

静态资源文件夹说完后，我们继续向下看源码！可以看到一个欢迎页的映射，就是我们的首页！

```java
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext, FormattingConversionService mvcConversionService,ResourceUrlProvider mvcResourceUrlProvider) {
    WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
        new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(), // getWelcomePage 获得欢迎页
        this.mvcProperties.getStaticPathPattern());
    welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
    return welcomePageHandlerMapping;
}
```

点进去继续看

```java
private Optional<Resource> getWelcomePage() {
    String[] locations = getResourceLocations(this.resourceProperties.getStaticLocations());
    // ::是java8 中新引入的运算符
    // Class::function的时候function是属于Class的，应该是静态方法。
    // this::function的funtion是属于这个对象的。
    // 简而言之，就是一种语法糖而已，是一种简写
    return Arrays.stream(locations).map(this::getIndexHtml).filter(this::isReadable).findFirst();
}
// 欢迎页就是一个location下的的 index.html 而已
private Resource getIndexHtml(String location) {
    return this.resourceLoader.getResource(location + "index.html");
}
```

欢迎页，**静态资源文件夹**下的所有 index.html 页面；被 /** 映射。

~~~java
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
~~~

比如我访问 http://localhost:8080/ ，就会找静态资源文件夹下的 index.html

新建一个 index.html ，在我们上面的3个目录中任意一个；然后访问测试 http://localhost:8080/ 看结果！

**关于网站图标说明（高版本不支持，高版本直接在静态资源下放`favicon.ico`就行）**：

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224327971-166301362.png)

与其他静态资源一样，Spring Boot在配置的静态内容位置中查找 favicon.ico。如果存在这样的文件，它将自动用作应用程序的favicon。

1、关闭SpringBoot默认图标

```properties
#关闭默认图标
spring.mvc.favicon.enabled=false
```

2、自己放一个图标在**静态资源目录**下，可以放在 public 目录下

3、清除浏览器缓存！刷新网页，发现图标已经变成自己的了！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224343431-2144081992.png)

# 11、SpringBoot：Thymeleaf模板引擎

## 11.1、模板引擎

前端交给我们的页面，是html页面。如果是我们以前开发，我们需要把他们转成jsp页面，jsp好处就是当我们查出一些数据转发到JSP页面以后，我们可以用jsp轻松实现数据的显示，及交互等。

jsp支持非常强大的功能，包括能写Java代码，但是呢，我们现在的这种情况，SpringBoot这个项目首先是以jar的方式，不是war，像第二，我们用的还是嵌入式的Tomcat，所以呢，**他现在默认是不支持jsp的**。

那不支持jsp，如果我们直接用纯静态页面的方式，那给我们开发会带来非常大的麻烦，那怎么办呢？

**SpringBoot推荐你可以来使用模板引擎：**

模板引擎，我们其实大家听到很多，其实jsp就是一个模板引擎，还有用的比较多的freemarker，包括SpringBoot给我们推荐的Thymeleaf，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的，什么样一个思想呢我们来看一下这张图：

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224715664-994768786.png)

模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个模板引擎，不管是jsp还是其他模板引擎，都是这个思想。只不过呢，就是说不同模板引擎之间，他们可能这个语法有点不一样。其他的我就不介绍了，我主要来介绍一下SpringBoot给我们推荐的Thymeleaf模板引擎，这模板引擎呢，是一个高级语言的模板引擎，他的这个语法更简单。而且呢，功能更强大。

我们呢，就来看一下这个模板引擎，那既然要看这个模板引擎。首先，我们来看SpringBoot里边怎么用。

## 11.2、引入Thymeleaf

怎么引入呢，对于springboot来说，什么事情不都是一个start的事情嘛，我们去在项目中引入一下。给大家三个网址：

Thymeleaf 官网：https://www.thymeleaf.org/

Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

Spring官方文档：找到我们对应的版本

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

找到对应的pom依赖：可以适当点进源码看下本来的包！

```xml
<!--thymeleaf-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

Maven会自动下载jar包，我们可以去看下下载的东西；

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224749784-1143746028.png)

## 11.3、Thymeleaf分析

前面呢，我们已经引入了Thymeleaf，那这个要怎么使用呢？

我们首先得按照SpringBoot的自动配置原理看一下我们这个Thymeleaf的自动配置规则，在按照那个规则，我们进行使用。

我们去找一下Thymeleaf的自动配置类：ThymeleafPropert

```java
@ConfigurationProperties(
    prefix = "spring.thymeleaf"
)
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
    private boolean checkTemplate = true;
    private boolean checkTemplateLocation = true;
    private String prefix = "classpath:/templates/";
    private String suffix = ".html";
    private String mode = "HTML";
    private Charset encoding;
}
```

我们可以在其中看到默认的前缀和后缀！

我们只需要把我们的html页面放在类路径下的templates下，thymeleaf就可以帮我们自动渲染了。

使用thymeleaf什么都不需要配置，只需要将他放在指定的文件夹下即可！

**测试**

1、编写一个TestController

```java
@Controller
public class TestController {
    
    @RequestMapping("/t1")
    public String test1(){
        //classpath:/templates/test.html
        return "test";
    }
    
}
```

2、编写一个测试页面 test.html 放在 templates 目录下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>测试页面</h1>

</body>
</html>
```

3、启动项目请求测试

## 11.4、Thymeleaf 语法学习

要学习语法，还是参考官网文档最为准确，我们找到对应的版本看一下；

Thymeleaf 官网：https://www.thymeleaf.org/ ， 简单看一下官网！我们去下载Thymeleaf的官方文档！

**我们做个最简单的练习 ：我们需要查出一些数据，在页面中展示**

1、修改测试请求，增加数据传输；

```java
@RequestMapping("/t1")
public String test1(Model model){
    //存入数据
    model.addAttribute("msg","Hello,Thymeleaf");
    //classpath:/templates/test.html
    return "test";
}
```

2、我们要使用thymeleaf，需要在html文件中导入命名空间的约束，方便提示。

我们可以去官方文档的#3中看一下命名空间拿来过来：

```html
 xmlns:th="http://www.thymeleaf.org"
```

3、我们去编写下前端页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>狂神说</title>
</head>
<body>
<h1>测试页面</h1>

<!--th:text就是将div中的内容设置为它指定的值，和之前学习的Vue一样-->
<div th:text="${msg}"></div>
</body>
</html>
```

4、启动测试！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224807885-1463858775.png)

**OK，入门搞定，我们来认真研习一下Thymeleaf的使用语法！**

**1、我们可以使用任意的 th:attr 来替换Html中原生属性的值！**

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224828773-2028075377.png)

**2、我们能写哪些表达式呢？**

```java
Simple expressions:（表达式语法）
Variable Expressions: ${...}：获取变量值；OGNL；
    1）、获取对象的属性、调用方法
    2）、使用内置的基本对象：#18
         #ctx : the context object.
         #vars: the context variables.
         #locale : the context locale.
         #request : (only in Web Contexts) the HttpServletRequest object.
         #response : (only in Web Contexts) the HttpServletResponse object.
         #session : (only in Web Contexts) the HttpSession object.
         #servletContext : (only in Web Contexts) the ServletContext object.

    3）、内置的一些工具对象：
　　　　　　#execInfo : information about the template being processed.
　　　　　　#uris : methods for escaping parts of URLs/URIs
　　　　　　#conversions : methods for executing the configured conversion service (if any).
　　　　　　#dates : methods for java.util.Date objects: formatting, component extraction, etc.
　　　　　　#calendars : analogous to #dates , but for java.util.Calendar objects.
　　　　　　#numbers : methods for formatting numeric objects.
　　　　　　#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
　　　　　　#objects : methods for objects in general.
　　　　　　#bools : methods for boolean evaluation.
　　　　　　#arrays : methods for arrays.
　　　　　　#lists : methods for lists.
　　　　　　#sets : methods for sets.
　　　　　　#maps : methods for maps.
　　　　　　#aggregates : methods for creating aggregates on arrays or collections.
==================================================================================

  Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
  Message Expressions: #{...}：获取国际化内容
  Link URL Expressions: @{...}：定义URL；
  Fragment Expressions: ~{...}：片段引用表达式

Literals（字面量）
      Text literals: 'one text' , 'Another one!' ,…
      Number literals: 0 , 34 , 3.0 , 12.3 ,…
      Boolean literals: true , false
      Null literal: null
      Literal tokens: one , sometext , main ,…
      
Text operations:（文本操作）
    String concatenation: +
    Literal substitutions: |The name is ${name}|
    
Arithmetic operations:（数学运算）
    Binary operators: + , - , * , / , %
    Minus sign (unary operator): -
    
Boolean operations:（布尔运算）
    Binary operators: and , or
    Boolean negation (unary operator): ! , not
    
Comparisons and equality:（比较运算）
    Comparators: > , < , >= , <= ( gt , lt , ge , le )
    Equality operators: == , != ( eq , ne )
    
Conditional operators:条件运算（三元运算符）
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
    
Special tokens:
    No-Operation: _
```

**练习测试：**

1、 我们编写一个Controller，放一些数据

```java
@RequestMapping("/t2")
public String test2(Map<String,Object> map){
    //存入数据
    map.put("msg","<h1>Hello</h1>");
    map.put("users", Arrays.asList("qinjiang","kuangshen"));
    //classpath:/templates/test.html
    return "test";
}
```

2、测试页面取出数据

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>狂神说</title>
</head>
<body>
<h1>测试页面</h1>

<div th:text="${msg}"></div>
<!--不转义-->
<div th:utext="${msg}"></div>

<!--遍历数据-->
<!--th:each每次遍历都会生成当前这个标签-->
<h4 th:each="user :${users}" th:text="${user}"></h4>

<h4>
    <!--行内写法,不推荐-->
    <span th:each="user:${users}">[[${user}]]</span>
</h4>

</body>
</html>
```

3、启动项目测试！

**我们看完语法，很多样式，我们即使现在学习了，也会忘记，所以我们在学习过程中，需要使用什么，根据官方文档来查询，才是最重要的，要熟练使用官方文档！**

# 12、SpringBoot：MVC自动配置原理

## 12.1、官网阅读

在进行项目编写前，我们还需要知道一个东西，就是SpringBoot对我们的SpringMVC还做了哪些配置，包括如何扩展，如何定制。

只有把这些都搞清楚了，我们在之后使用才会更加得心应手。途径一：源码分析，途径二：官方文档！

地址 ：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration

```java
Spring MVC Auto-configuration
// Spring Boot为Spring MVC提供了自动配置，它可以很好地与大多数应用程序一起工作。
Spring Boot provides auto-configuration for Spring MVC that works well with most applications.
// 自动配置在Spring默认设置的基础上添加了以下功能：
The auto-configuration adds the following features on top of Spring’s defaults:
// 包含视图解析器
Inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans.
// 支持静态资源文件夹的路径，以及webjars
Support for serving static resources, including support for WebJars 
// 自动注册了Converter：
// 转换器，这就是我们网页提交数据到后台自动封装成为对象的东西，比如把"1"字符串自动转换为int类型
// Formatter：【格式化器，比如页面给我们了一个2019-8-10，它会给我们自动格式化为Date对象】
Automatic registration of Converter, GenericConverter, and Formatter beans.
// HttpMessageConverters
// SpringMVC用来转换Http请求和响应的的，比如我们要把一个User对象转换为JSON字符串，可以去看官网文档解释；
Support for HttpMessageConverters (covered later in this document).
// 定义错误代码生成规则的
Automatic registration of MessageCodesResolver (covered later in this document).
// 首页定制
Static index.html support.
// 图标定制
Custom Favicon support (covered later in this document).
// 初始化数据绑定器：帮我们把请求数据绑定到JavaBean中！
Automatic use of a ConfigurableWebBindingInitializer bean (covered later in this document).

/*
如果您希望保留Spring Boot MVC功能，并且希望添加其他MVC配置（拦截器、格式化程序、视图控制器和其他功能），则可以添加自己的@configuration类，类型为webmvcconfiguer，但不添加@EnableWebMvc。如果希望提供RequestMappingHandlerMapping、RequestMappingHandlerAdapter或ExceptionHandlerExceptionResolver的自定义实例，则可以声明WebMVCregistrationAdapter实例来提供此类组件。
*/
If you want to keep Spring Boot MVC features and you want to add additional MVC configuration 
(interceptors, formatters, view controllers, and other features), you can add your own 
@Configuration class of type WebMvcConfigurer but without @EnableWebMvc. If you wish to provide 
custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or 
ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

// 如果您想完全控制Spring MVC，可以添加自己的@Configuration，并用@EnableWebMvc进行注释。
If you want to take complete control of Spring MVC, you can add your own @Configuration annotated with @EnableWebMvc.
```

我们来仔细对照，看一下它怎么实现的，它告诉我们SpringBoot已经帮我们自动配置好了SpringMVC，然后自动配置了哪些东西呢？

## **12.2、ContentNegotiatingViewResolver 内容协商视图解析器**

自动配置了ViewResolver，就是我们之前学习的SpringMVC的视图解析器；

即根据方法的返回值取得视图对象（View），然后由视图对象决定如何渲染（转发，重定向）。

我们去看看这里的源码：我们找到 WebMvcAutoConfiguration ， 然后搜索ContentNegotiatingViewResolver。找到如下方法！

```java
@Bean
@ConditionalOnBean(ViewResolver.class)
@ConditionalOnMissingBean(name = "viewResolver", value = ContentNegotiatingViewResolver.class)
public ContentNegotiatingViewResolver viewResolver(BeanFactory beanFactory) {
    ContentNegotiatingViewResolver resolver = new ContentNegotiatingViewResolver();
    resolver.setContentNegotiationManager(beanFactory.getBean(ContentNegotiationManager.class));
    // ContentNegotiatingViewResolver使用所有其他视图解析器来定位视图，因此它应该具有较高的优先级
    resolver.setOrder(Ordered.HIGHEST_PRECEDENCE);
    return resolver;
}
```

我们可以点进这类看看！找到对应的解析视图的代码；

```java
@Nullable // 注解说明：@Nullable 即参数可为null
public View resolveViewName(String viewName, Locale locale) throws Exception {
    RequestAttributes attrs = RequestContextHolder.getRequestAttributes();
    Assert.state(attrs instanceof ServletRequestAttributes, "No current ServletRequestAttributes");
    List<MediaType> requestedMediaTypes = this.getMediaTypes(((ServletRequestAttributes)attrs).getRequest());
    if (requestedMediaTypes != null) {
        // 获取候选的视图对象
        List<View> candidateViews = this.getCandidateViews(viewName, locale, requestedMediaTypes);
        // 选择一个最适合的视图对象，然后把这个对象返回
        View bestView = this.getBestView(candidateViews, requestedMediaTypes, attrs);
        if (bestView != null) {
            return bestView;
        }
    }
    // .....
}
```

我们继续点进去看，他是怎么获得候选的视图的呢？

getCandidateViews中看到他是把所有的视图解析器拿来，进行while循环，挨个解析！

```java
Iterator var5 = this.viewResolvers.iterator();
```

所以得出结论：**ContentNegotiatingViewResolver 这个视图解析器就是用来组合所有的视图解析器的**

我们再去研究下他的组合逻辑，看到有个属性viewResolvers，看看它是在哪里进行赋值的！

```java
protected void initServletContext(ServletContext servletContext) {
    // 这里它是从beanFactory工具中获取容器中的所有视图解析器
    // ViewRescolver.class 把所有的视图解析器来组合的
    Collection<ViewResolver> matchingBeans = BeanFactoryUtils.beansOfTypeIncludingAncestors(this.obtainApplicationContext(), ViewResolver.class).values();
    ViewResolver viewResolver;
    if (this.viewResolvers == null) {
        this.viewResolvers = new ArrayList(matchingBeans.size());
    }
    // ...............
}
```

既然它是在容器中去找视图解析器，我们是否可以猜想，我们就可以去实现一个视图解析器了呢？

我们可以自己给容器中去添加一个视图解析器；这个类就会帮我们自动的将它组合进来；**我们去实现一下**

1、我们在我们的主程序中去写一个视图解析器来试试；

```java
@Bean //放到bean中
public ViewResolver myViewResolver(){
    return new MyViewResolver();
}

//我们写一个静态内部类，视图解析器就需要实现ViewResolver接口
private static class MyViewResolver implements ViewResolver{
    @Override
    public View resolveViewName(String s, Locale locale) throws Exception {
        return null;
    }
}
```

2、怎么看我们自己写的视图解析器有没有起作用呢？

我们给 DispatcherServlet 中的 doDispatch方法 加个断点进行调试一下，因为所有的请求都会走到这个方法中

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224922244-774346297.png)

3、我们启动我们的项目，然后随便访问一个页面，看一下Debug信息；

找到this

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224940354-738491944.png)

找到视图解析器，我们看到我们自己定义的就在这里了；

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412224955594-693958833.png)

所以说，我们如果想要使用自己定制化的东西，我们只需要给容器中添加这个组件就好了！剩下的事情SpringBoot就会帮我们做了！

## 12.3、转换器和格式化器

找到格式化转换器：

```java
@Bean
@Override
public FormattingConversionService mvcConversionService() {
    // 拿到配置文件中的格式化规则
    WebConversionService conversionService = 
        new WebConversionService(this.mvcProperties.getDateFormat());
    addFormatters(conversionService);
    return conversionService;
}
```

点击去：

```java
public String getDateFormat() {
    return this.dateFormat;
}

/**
* Date format to use. For instance, `dd/MM/yyyy`. 默认的
 */
private String dateFormat;
```

可以看到在我们的Properties文件中，我们可以进行自动配置它！

如果配置了自己的格式化方式，就会注册到Bean中生效，我们可以在配置文件中配置日期格式化的规则：

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225016479-244888602.png)

其余的就不一一举例了，大家可以下去多研究探讨即可！

## 12.4、修改SpringBoot的默认配置

这么多的自动配置，原理都是一样的，通过这个WebMVC的自动配置原理分析，我们要学会一种学习方式，通过源码探究，得出结论；这个结论一定是属于自己的，而且一通百通。

SpringBoot的底层，大量用到了这些设计细节思想，所以，没事需要多阅读源码！得出结论；

SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（如果用户自己配置@bean），如果有就用用户配置的，如果没有就用自动配置的；

如果有些组件可以存在多个，比如我们的视图解析器，就将用户配置的和自己默认的组合起来！

**扩展使用SpringMVC** 官方文档如下：

If you want to keep Spring Boot MVC features and you want to add additional MVC configuration (interceptors, formatters, view controllers, and other features), you can add your own @Configuration class of type WebMvcConfigurer but **without** @EnableWebMvc. If you wish to provide custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

我们要做的就是编写一个@Configuration注解类，并且类型要为WebMvcConfigurer，还不能标注@EnableWebMvc注解；我们去自己写一个；我们新建一个包叫config，写一个类MyMvcConfig；

```java
//应为类型要求为WebMvcConfigurer，所以我们实现其接口
//可以使用自定义类扩展MVC的功能
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 浏览器发送/test ， 就会跳转到test页面；
        registry.addViewController("/test").setViewName("test");
    }
}
```

我们去浏览器访问一下：

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225034989-1426302926.png)

**确实也跳转过来了！所以说，我们要扩展SpringMVC，官方就推荐我们这么去使用，既保SpringBoot留所有的自动配置，也能用我们扩展的配置！**

我们可以去分析一下原理：

1、WebMvcAutoConfiguration 是 SpringMVC的自动配置类，里面有一个类WebMvcAutoConfigurationAdapter

2、这个类上有一个注解，在做其他自动配置时会导入：@Import(EnableWebMvcConfiguration.class)

3、我们点进EnableWebMvcConfiguration这个类看一下，它继承了一个父类：DelegatingWebMvcConfiguration

这个父类中有这样一段代码：

```java
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
    private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
    
  // 从容器中获取所有的webmvcConfigurer
    @Autowired(required = false)
    public void setConfigurers(List<WebMvcConfigurer> configurers) {
        if (!CollectionUtils.isEmpty(configurers)) {
            this.configurers.addWebMvcConfigurers(configurers);
        }
    }
}
```

4、我们可以在这个类中去寻找一个我们刚才设置的viewController当做参考，发现它调用了一个

```java
protected void addViewControllers(ViewControllerRegistry registry) {
    this.configurers.addViewControllers(registry);
}
```

5、我们点进去看一下

```java
public void addViewControllers(ViewControllerRegistry registry) {
    Iterator var2 = this.delegates.iterator();

    while(var2.hasNext()) {
        // 将所有的WebMvcConfigurer相关配置来一起调用！包括我们自己配置的和Spring给我们配置的
        WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
        delegate.addViewControllers(registry);
    }

}
```

所以得出结论：所有的WebMvcConfiguration都会被作用，不止Spring自己的配置类，我们自己的配置类当然也会被调用；

## 12.5、全面接管SpringMVC

官方文档：

```java
If you want to take complete control of Spring MVC
you can add your own @Configuration annotated with @EnableWebMvc.
```

全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！

只需在我们的配置类中要加一个@EnableWebMvc。

我们看下如果我们全面接管了SpringMVC了，我们之前SpringBoot给我们配置的静态资源映射一定会无效，我们可以去测试一下；

不加注解之前，访问首页：

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225051724-1570792880.png)

给配置类加上注解：@EnableWebMvc

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225103839-1070501209.png)

我们发现所有的SpringMVC自动配置都失效了！回归到了最初的样子；

**当然，我们开发中，不推荐使用全面接管SpringMVC**

思考问题？为什么加了一个注解，自动配置就失效了！我们看下源码：

1、这里发现它是导入了一个类，我们可以继续进去看

```java
@Import({DelegatingWebMvcConfiguration.class})public @interface EnableWebMvc {}
```

2、它继承了一个父类 WebMvcConfigurationSupport

```java
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {  // ......}
```

3、我们来回顾一下Webmvc自动配置类

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
// 这个注解的意思就是：容器中没有这个组件的时候，这个自动配置类才生效
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
    ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
    
}
```

总结一句话：@EnableWebMvc将WebMvcConfigurationSupport组件导入进来了；

而导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能！

**在SpringBoot中会有非常多的扩展配置，只要看见了这个，我们就应该多留心注意~**

# 13、SpringBoot：Web开发

## 13.1、准备工作

我们现在可以来导入我们的所有提供的资源！

pojo 及 dao 放到项目对应的路径下：

**pojo实体类**

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) Department.java

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) Employee.java

 

**dao层**

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) DepartmentDao

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) EmployeeDao

 

导入完毕这些之后，我们还需要导入我们的前端页面，及静态资源文件！

- css，js等放在static文件夹下
- html放在templates文件夹下

准备工作：OK！！！

**要求一：默认访问首页**

方式一：写一个controller实现！

```java
//会解析到templates目录下的index.html页面
@RequestMapping({"/","/index.html"})
public String index(){
    return "index";
}
```

 方式二：自己编写MVC的扩展配置

```java
package com.kuang.myproject.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
    }
}
```

解决了这个问题，我们还需要解决一个资源导入的问题；

我们可以将我们项目的启动名改掉

```properties
server.servlet.context-path=/kuang
```

 现在你访问localhost：8080 就不行了，需要访问localhost：8080/kuang

为了保证资源导入稳定，我们建议在所有资源导入时候使用 th:去替换原有的资源路径！

```html
<link href="/css/bootstrap.min.css" rel="stylesheet">
<link th:href="@{/css/bootstrap.min.css}" rel="stylesheet">
```

有的时候，我们的网站会去涉及中英文甚至多语言的切换，这时候我们就需要学习**国际化**了！

先在IDEA中统一设置properties的编码问题！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225128194-996554004.png)

编写国际化配置文件，抽取页面需要显示的国际化页面消息。我们可以去登录页面查看一下，哪些内容我们需要编写国际化的配置！

## 13.2、配置文件编写

1、我们在resources资源文件下新建一个i18n目录，存放国际化配置文件

2、建立一个login.properties文件，还有一个login_zh_CN.properties；发现IDEA自动识别了我们要做国际化操作；文件夹变了！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225141876-1307608137.png)

3、我们可以在这上面去新建一个文件；

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225154889-1524653449.png)

弹出如下页面：我们再添加一个英文的；

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225207604-1238451568.png)

这样就快捷多了！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225221065-887398655.png)

**4、接下来，我们就来编写配置，我们可以看到idea下面有另外一个视图；**

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225234441-258590952.png)

这个视图我们点击 + 号就可以直接添加属性了；我们新建一个login.tip，可以看到边上有三个文件框可以输入

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225247649-1746815344.png)

我们添加一下首页的内容！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225259474-1140791172.png)

然后依次添加其他页面内容即可！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225311999-716532013.png)

然后去查看我们的配置文件；

login.properties ：默认

```properties
login.btn=登录
login.password=密码
login.remember=记住我
login.tip=请登录
login.username=用户名
```

英文：

```properties
login.btn=Sign in
login.password=Password
login.remember=Remember me
login.tip=Please sign in
login.username=Username
```

中文：

```properties
login.btn=登录
login.password=密码
login.remember=记住我
login.tip=请登录
login.username=用户名
```

OK，配置文件步骤搞定！

## 13.3、配置文件生效探究

我们去看一下SpringBoot对国际化的自动配置！这里又涉及到一个类：MessageSourceAutoConfiguration

里面有一个方法，这里发现SpringBoot已经自动配置好了管理我们国际化资源文件的组件 ResourceBundleMessageSource；

```java
// 获取 properties 传递过来的值进行判断
@Bean
public MessageSource messageSource(MessageSourceProperties properties) {
    ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
    if (StringUtils.hasText(properties.getBasename())) {
        // 设置国际化文件的基础名（去掉语言国家代码的）
        messageSource.setBasenames(
            StringUtils.commaDelimitedListToStringArray(
                                       StringUtils.trimAllWhitespace(properties.getBasename())));
    }
    if (properties.getEncoding() != null) {
        messageSource.setDefaultEncoding(properties.getEncoding().name());
    }
    messageSource.setFallbackToSystemLocale(properties.isFallbackToSystemLocale());
    Duration cacheDuration = properties.getCacheDuration();
    if (cacheDuration != null) {
        messageSource.setCacheMillis(cacheDuration.toMillis());
    }
    messageSource.setAlwaysUseMessageFormat(properties.isAlwaysUseMessageFormat());
    messageSource.setUseCodeAsDefaultMessage(properties.isUseCodeAsDefaultMessage());
    return messageSource;
}
```

我们真实 的情况是放在了i18n目录下，所以我们要去配置这个messages的路径；

```properties
spring.messages.basename=i18n.login
```

## 13.4、配置页面国际化值

去页面获取国际化的值，查看Thymeleaf的文档，找到message取值操作为：#{...}。我们去页面测试下：

IDEA还有提示，非常智能的！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225332289-780138786.png)

我们可以去启动项目，访问一下，发现已经自动识别为中文的了！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225346425-1869149052.png)

**但是我们想要更好！可以根据按钮自动切换中文英文！**

## 13.5、配置国际化解析

在Spring中有一个国际化的Locale （区域信息对象）；里面有一个叫做LocaleResolver （获取区域信息对象）的解析器！

我们去我们webmvc自动配置文件，寻找一下！看到SpringBoot默认配置：

```java
@Bean
@ConditionalOnMissingBean
@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
public LocaleResolver localeResolver() {
    // 容器中没有就自己配，有的话就用用户配置的
    if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
        return new FixedLocaleResolver(this.mvcProperties.getLocale());
    }
    // 浏览器消息接收头国际化分解
    AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
    localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
    return localeResolver;
}
```

AcceptHeaderLocaleResolver 这个类中有一个方法

```java
public Locale resolveLocale(HttpServletRequest request) {
    Locale defaultLocale = this.getDefaultLocale();
    // 默认的就是根据请求头带来的区域信息获取Locale进行国际化
    if (defaultLocale != null && request.getHeader("Accept-Language") == null) {
        return defaultLocale;
    } else {
        Locale requestLocale = request.getLocale();
        List<Locale> supportedLocales = this.getSupportedLocales();
        if (!supportedLocales.isEmpty() && !supportedLocales.contains(requestLocale)) {
            Locale supportedLocale = this.findSupportedLocale(request, supportedLocales);
            if (supportedLocale != null) {
                return supportedLocale;
            } else {
                return defaultLocale != null ? defaultLocale : requestLocale;
            }
        } else {
            return requestLocale;
        }
    }
}
```

那假如我们现在想点击链接让我们的国际化资源生效，就需要让我们自己的Locale生效！

我们去自己写一个自己的LocaleResolver，可以在链接上携带区域信息！

修改一下前端页面的跳转连接：

```html
<!-- 这里传入参数不需要使用 ？使用 （key=value）-->
<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
```

我们去写一个处理的组件类！

```java
package com.kuang.component;

import org.springframework.util.StringUtils;
import org.springframework.web.servlet.LocaleResolver;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;

//可以在链接上携带区域信息
public class MyLocaleResolver implements LocaleResolver {

    //解析请求
    @Override
    public Locale resolveLocale(HttpServletRequest request) {

        String language = request.getParameter("l");
        Locale locale = Locale.getDefault(); // 如果没有获取到就使用系统默认的
        //如果请求链接不为空
        if (!StringUtils.isEmpty(language)){
            //分割请求参数
            String[] split = language.split("_");
            //国家，地区
            locale = new Locale(split[0],split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}
```

为了让我们的区域化信息能够生效，我们需要再配置一下这个组件！在我们自己的MyMvcConfig下添加bean；

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
    }
    @Bean
    public LocaleResolver localeResolver() {
        return new MyLocaleResolver();
    }
}
```

**我们重启项目，来访问一下，发现点击按钮可以实现成功切换！搞定收工！**

## 13.6、登录 + 拦截器

我们这里就先不连接数据库了，输入任意用户名都可以登录成功！

声明一个之前没有提到的问题：

templates下的页面只能通过Controller跳转实现，而static下的页面是能直接被外界访问的，就能正常访问了。

我们把登录页面的表单提交地址写一个controller！

```html
<form class="form-signin" action="dashboard.html" th:action="@{/user/login}" method="post">  //这里面的所有表单标签都需要加上一个name属性,请求传递参数值
</form>
```

去编写对应的controller

```java
package com.kuang.myproject.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.util.StringUtils;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class LoginController {

    //@RequestMapping(value = "/user/login",method = RequestMethod.POST)
    @PostMapping("/user/login")
    public String login(@RequestParam("username") String username,
                        @RequestParam("password") String password,
                        Model model){

        if (!StringUtils.isEmpty(username) && "123456".equals(password)){
            //登录成功！
            return "dashboard";
        }else {
            //登录失败！存放错误信息
            model.addAttribute("msg","用户名密码错误");
            return "index";
        }

    }

}
```

页面存在缓存，所以我们需要禁用模板引擎的缓存

```properties
#禁用模板缓存
spring.thymeleaf.cache=false
```

 模板引擎修改后，想要实时生效！页面修改完毕后，IDEA小技巧 ： Ctrl + F9 重新编译！

登录失败的话，我们需要将后台信息输出到前台，可以在首页标题下面加上判断！

```html
<!--判断是否显示，使用if, ${}可以使用工具类，可以看thymeleaf的中文文档-->
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

刷新测试 ：

[![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190808170326449-623734820.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190808170326449-623734820.png)

**优化，登录成功后，由于是转发，链接不变，我们可以重定向到内容页！**

我们再添加一个视图控制映射，在我们的自己的**MyMvcConfig**中：

```java
registry.addViewController("/main.html").setViewName("dashboard");
```

将 **Controller** 的代码改为重定向；

```java
//登录成功！防止表单重复提交，我们重定向
return "redirect:/main.html";
```

但是又发现新的问题，我们可以直接登录到后台主页，不用登录也可以实现！

怎么处理这个问题呢？我们可以使用拦截器机制，实现登录检查！

**Controller**里添加session，传递loginUser

```java
session.setAttribute("loginUser", username);
```

我们先自定义一个拦截器

```java
package com.kuang.myproject.component;

import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class LoginHandlerInterceptor implements HandlerInterceptor {
    //目标方法执行之前,进行拦截
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object user = request.getSession().getAttribute("loginUser");
        if (user == null){//未登录，返回登录页面
            request.setAttribute("msg","没有权限，请先登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else {
            //登录，放行
            return true;
        }
    }
}
```

然后将拦截器注册到我们的SpringMVC配置类**MyMvcConfig**当中！

```java
//注册拦截器
@Override
public void addInterceptors(InterceptorRegistry registry) {
    //注册拦截器，及拦截请求和要剔除哪些请求!
    //我们还需要过滤静态资源文件，否则样式显示不出来
    registry.addInterceptor(new LoginHandlerInterceptor()) 
        .addPathPatterns("/**")
        //不拦截的链接
        .excludePathPatterns("/index.html","/","/user/login",
                             "/css/**"，"/js/**"，"/img/**");
}
```

我们然后在后台主页，获取用户登录的信息

```html
<!--后台主页显示登录用户的信息-->
<a class="----" href="#">[[${session.loginUser}]]</a>
<a class="----" href="#" th:text="${{session.loginUser}}"></a>
<!--${{xxx}}表示把表达式里的值转换为string类型-->
```

**然后我们登录测试！完美！**

## 13.7、员工列表功能

**要求 ： 我们需要使用 Restful风格实现我们的crud操作！**

[![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190808230541727-277301972.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190808230541727-277301972.png)

看看一些具体的要求，就是我们小实验的架构；

[![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190808230627472-237067240.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190808230627472-237067240.png)

**我们根据这些要求，来完成第一个功能，就是我们的员工列表功能！**

我们在主页点击Customers，就显示列表页面；我们去修改下

1.将首页的侧边栏Customers改为员工管理

2.a链接添加请求

```html
<a class="nav-link" href="#" th:href="@{/emps}">员工管理</a>
```

 3.将list放在emp文件夹下

[![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190808232442673-1080707018.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190808232442673-1080707018.png)

 4.编写处理请求的controller

```java
@Controller
public class EmployeeController {

    @Autowired
    EmployeeDao employeeDao;

    //查询所有员工，返回列表页面
    @GetMapping("/emps")
    public String list(Model model){
        Collection<Employee> employees = employeeDao.getAll();
        //将结果放在请求中
        model.addAttribute("emps",employees);
        return "emp/list";
    }

}
```

我们启动项目，测试一下看是否能够跳转，测试OK！我们只需要将数据渲染进去即可！

但是发现一个问题，侧边栏和顶部都相同，我们是不是应该将它抽取出来呢？

**Thymeleaf 公共页面元素抽取**

1.抽取公共片段 th:fragment 定义模板名

2.引入公共片段 th:insert 插入模板名

我们来抽取一下，使用list列表做演示！我们要抽取头部，nav标签

我们在dashboard中将nav（顶部栏）部分定义一个模板名

```html
<nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0" th:fragment="topbar" >
    <!--后台主页显示登录用户的信息-->
    <a class="navbar-brand col-sm-3 col-md-2 mr-0" 		 				    		   href="http://getbootstrap.com/docs/4.0/examples/dashboard/#"> 						[[${session.loginUser}]]</a>
    <input class="form-control form-control-dark w-100" type="text" placeholder="Search" 
           aria-label="Search">
    <ul class="navbar-nav px-3">
        <li class="nav-item text-nowrap">
            <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                Sign out</a>
        </li>
    </ul>
</nav>
```

然后我们在list页面中去引入，可以删掉原来的nav

```html
<!--引入抽取的topbar-->
<!--模板名 ： 会使用thymeleaf的前后缀配置规则进行解析
使用~{模板::标签名}-->
<div th:insert="~{dashboard::topbar}"></div>
```

 效果：可以看到已经成功加载过来了！

[![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190808234359418-2014232536.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190808234359418-2014232536.png)

 **除了使用insert插入，还可以使用replace替换，或者include包含，三种方式会有一些小区别，可以见名知义；**

**我们使用replace替换，可以解决div多余的问题，可以查看thymeleaf的文档学习**

侧边栏也是同理，当做练手，可以也同步一下！

保证这一步做完！

我们发现一个小问题，侧边栏激活的问题，它总是激活第一个；按理来说，这应该是动态的才对！

为了重用更清晰，我们建立一个commons文件夹，专门存放公共页面html；

~~~html
<body>
<!--底部栏模板-->
<nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0" th:fragment="topbar">
    -----------
</nav>
<!--侧边栏模板-->
<nav class="col-md-2 d-none d-md-block bg-light sidebar" th:fragment="sidebar" >
    --------------
</nav>
</body>
~~~

我们去页面中引入一下

```html
<div th:replace="~{commons/bar::topbar}"></div>
<div th:replace="~{commons/bar::sidebar}"></div>
```

 我们先测试一下，保证所有的页面没有出问题！ok！

我们来解决我们侧边栏选项高亮问题！

1.将main.html首页的超链接地址改到项目中

```html
<a class="nav-link active" th:href="@{/main.html}">
    首页 <span class="sr-only">(current)</span>
</a>
```

2.我们在a标签中加一个判断，使用class改变标签的值；

```html
<!--模板页面 --> 
<a th:class="${activeUrl=='main.html'?'nav-link active':'nav-link'}" th:href="@{/main.html}"> 首页 </a>
其余同理，判断请求携带的参数，进行激活测试
 
<a th:class="${active=='list.xml'?'nav-link active':'nav-link'}" href="/emps"> 
    员工管理</a>
```

 3.修改页面模板插入链接

```html
<!--dashboard页面  activeUrl是自定义参数名，值也是自定义的，只要与模板class判断时参数对应就行-->
<div th:replace="~{commons/bar::sidebar(activeUrl='main.html')}"></div>
<!--list页面 -->
<div th:replace="~{commons/bar::sidebar(activeUrl='emps')}"></div>其余要用都是同理。
```

 我们刷新页面，去测试一下，OK，动态激活搞定！

现在我们来遍历我们的员工信息！顺便美化一些页面，增加添加，修改，删除的按钮

```html
<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
    <!--添加员工按钮-->
    <h2> <button class="btn btn-sm btn-success">添加员工</button></h2>
    <div class="table-responsive">
        <table class="table table-striped table-sm">
            <thead>
                <tr>
                    <th>id</th>
                    <th>lastName</th>
                    <th>email</th>
                    <th>gender</th>
                    <th>department</th>
                    <th>birth</th>
                    <!--我们还可以在显示的时候带一些操作按钮-->
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr th:each="emp:${emps}">
                    <td th:text="${emp.id}"></td>
                    <td>[[${emp.lastName}]]</td>
                    <td th:text="${emp.email}"></td>
                    <td th:text="${emp.gender==0?'女':'男'}"></td>
                    <td th:text="${emp.department.departmentName}"></td>
                    <!--<td th:text="${emp.birth}"></td>-->
                    <!--使用时间格式化工具-->
                    <td th:text="${#dates.format(emp.birth,'yyyy-MM-dd HH:mm')}"></td>

                    <!--操作-->
                    <td>
                        <a class="btn btn-sm btn-primary">编辑</a>
                        <a class="btn btn-sm btn-danger">删除</a>
                    </td>
                </tr>
            </tr>
        </tbody>
    </table>
</div>
</main>
```

**OK，显示全部员工OK！**

### **添加员工信息**

1.将添加员工信息改为超链接

```html
<!--添加员工按钮-->
<h2> <a class="btn btn-sm btn-success" th:href="@{/emp}">添加员工</a></h2>
```

 2.编写对应的controller

```java
//to员工添加页面
@GetMapping("/emp")
public String toAddPage(){
    return "emp/add";
}
```

 3.添加前端页面；复制 list页面，修改即可

bootstrap官网文档 ： https://v4.bootcss.com/docs/4.0/components/forms/ ， 我们去可以里面找自己喜欢的样式！

我这里给大家提供了编辑好的，**注意该form表单必须包含在main标签下，不让显示会错乱**

```html
<form th:action="@{/emp}" method="post">
    <div class="form-group">
        <label>LastName</label>
        <input type="text" name="lastName" class="form-control" placeholder="emlpoyeeName">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input type="email" name="email" class="form-control" placeholder="email">
    </div>
    <div class="form-group">
        <label>Gender</label><br/>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="1">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="0">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <!-- 注意这里是获取了department的id，不是department对象，所以要用name属性-->
        <select class="form-control" name="department.id">
            <option th:each="dep:${deps}" th:text="${dep.departmentName}"
                    th:value="${dep.id}"> <!-- 注意这里是获取了department的id，不是department对象-->
            </option>

        </select>
    </div>
    <div class="form-group">
        <label>Birth</label>
        <input type="text" name="birth" class="form-control" placeholder="2000-1-1">
    </div>
    <button type="submit" class="btn btn-primary" >添加</button>
</form>
```

 4.部门信息下拉框应该选择的是我们提供的数据，所以我们要修改一下前端和后端

controller

```java
//to员工添加页面
@GetMapping("/emp")
public String toAddPage(Model model){
    //查出所有的部门，提供选择
    Collection<Department> departments = departmentDao.getDepartments();
    model.addAttribute("departments",departments);
    return "emp/add";
}
```

前端

```html
<div class="form-group">
    <label>department</label>
    <!--提交的是部门的ID-->
    <select class="form-control">
        <option th:each="dept:${departments}" th:text="${dept.departmentName}" th:value="${dept.id}"></option>
    </select>
</div>
```

**OK，修改了controller，重启项目测试！**

 我们来具体实现添加功能；

1.修改add页面form表单提交地址和方式

```html
<form th:action="@{/emp}" method="post">
```

 2.编写controller；

```java
//员工添加功能，使用post接收
@PostMapping("/emp")
public String addEmp(){

    //回到员工列表页面，可以使用redirect或者forward，就不会被视图解析器解析
    return "redirect:/emps";
}
```

回忆：重定向和转发 以及 /的问题？

原理探究 ： ThymeleafViewResolver

```java
public static final String REDIRECT_URL_PREFIX = "redirect:";
public static final String FORWARD_URL_PREFIX = "forward:";

protected View createView(String viewName, Locale locale) throws Exception {
    if (!this.alwaysProcessRedirectAndForward && !this.canHandle(viewName, locale)) {
        vrlogger.trace("[THYMELEAF] View \"{}\" cannot be handled by ThymeleafViewResolver. Passing on to the next resolver in the chain.", viewName);
        return null;
    } else {
        String forwardUrl;
        if (viewName.startsWith("redirect:")) {
            vrlogger.trace("[THYMELEAF] View \"{}\" is a redirect, and will not be handled directly by ThymeleafViewResolver.", viewName);
            forwardUrl = viewName.substring("redirect:".length(), viewName.length());
            RedirectView view = new RedirectView(forwardUrl, this.isRedirectContextRelative(), this.isRedirectHttp10Compatible());
            return (View)this.getApplicationContext().getAutowireCapableBeanFactory().initializeBean(view, viewName);
        } else if (viewName.startsWith("forward:")) {
            vrlogger.trace("[THYMELEAF] View \"{}\" is a forward, and will not be handled directly by ThymeleafViewResolver.", viewName);
            forwardUrl = viewName.substring("forward:".length(), viewName.length());
            return new InternalResourceView(forwardUrl);
        } else if (this.alwaysProcessRedirectAndForward && !this.canHandle(viewName, locale)) {
            vrlogger.trace("[THYMELEAF] View \"{}\" cannot be handled by ThymeleafViewResolver. Passing on to the next resolver in the chain.", viewName);
            return null;
        } else {
            vrlogger.trace("[THYMELEAF] View {} will be handled by ThymeleafViewResolver and a {} instance will be created for it", viewName, this.getViewClass().getSimpleName());
            return this.loadView(viewName, locale);
        }
    }
}
```

OK，看完源码，我们继续编写代码！

我们要接收前端传过来的属性，将它封装成为对象！首先需要将前端页面空间的name属性编写完毕！【操作】

编写controller接收调试打印【操作】

```java
//员工添加功能
//接收前端传递的参数，自动封装成为对象[要求前端传递的参数名，和属性名一致]
@PostMapping("/emp")
public String addEmp(Employee employee){
    System.out.println(employee);
    employeeDao.save(employee); //保存员工信息
    //回到员工列表页面，可以使用redirect或者forward
    return "redirect:/emps";
}
```

前端填写数据，注意时间问题

[![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190809224030384-257602303.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190809224030384-257602303.png)

**点击提交，后台输出正常！页面跳转及数据显示正常！OK！**

**那我们将时间换一个格式提交**

 [![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190809224146677-1143842054.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190809224146677-1143842054.png)

提交发现页面出现了400错误！

[![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190809224215683-2028790328.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190809224215683-2028790328.png)

生日我们提交的是一个日期 ， 我们第一次使用的 / 正常提交成功了，后面使用 - 就错误了，所以这里面应该存在一个日期格式化的问题；

SpringMVC会将页面提交的值转换为指定的类型，默认日期是按照 / 的方式提交 ； 比如将2019/01/01 转换为一个date对象。

那思考一个问题？我们能不能修改这个默认的格式呢？

我们去看webmvc的自动配置文件；找到一个日期格式化的方法，我们可以看一下

```java
@Bean
public FormattingConversionService mvcConversionService() {
    WebConversionService conversionService = new WebConversionService(this.mvcProperties.getDateFormat());
    this.addFormatters(conversionService);
    return conversionService;
}
```

调用了 getDateFormat 方法；

```java
public String getDateFormat() {
    return this.dateFormat;
}
```

 这个在配置类中，所以我们可以自定义的去修改这个时间格式化问题，我们在我们的配置文件中修改一下；

```properties
spring.mvc.date-format=yyyy-MM-dd
```

 **这样的话，我们现在就支持 - 的格式了，但是又不支持 / 了 **

测试OK！

### 员工修改功能

我们要实现员工修改功能，需要实现两步；

1.点击修改按钮，去到编辑页面，我们可以直接使用添加员工的页面实现

2.显示原数据，修改完毕后跳回列表页面！

我们去实现一下：首先修改跳转链接的位置；

```HTML
<a class="btn btn-sm btn-primary" th:href="@{/emp/}+${emp.id}">编辑</a>
```

 编写对应的controller

```java
//to员工修改页面
@GetMapping("/emp/{id}")
public String toUpdateEmp(@PathVariable("id") Integer id,Model model){
    //根据id查出来员工
    Employee employee = employeeDao.get(id);
    //将员工信息返回页面
    model.addAttribute("emp",employee);
    //查出所有的部门，提供修改选择
    Collection<Department> departments = departmentDao.getDepartments();
    model.addAttribute("departments",departments);

    return "emp/update";
}
```

我们需要在这里将add页面复制一份，改为update页面；需要修改页面，将我们后台查询数据回显

```html
<form th:action="@{/emp}" method="post">
    <div class="form-group">
        <label>LastName</label>
        <input name="lastName" type="text" class="form-control" th:value="${emp.lastName}">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input name="email" type="email" class="form-control" th:value="${emp.email}">
    </div>
    <div class="form-group">
        <label>Gender</label><br/>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="1"
                   th:checked="${emp.gender==1}">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="0"
                   th:checked="${emp.gender==0}">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <!--提交的是部门的ID-->
        <select class="form-control" name="department.id">
            <option th:selected="${dept.id == emp.department.id}" th:each="dept:${departments}" th:text="${dept.departmentName}" th:value="${dept.id}">
            </option>
        </select>
    </div>
    <div class="form-group">
        <label>Birth</label>
        <input name="birth" type="text" class="form-control" th:value="${emp.birth}">
    </div>
    <button type="submit" class="btn btn-primary">修改</button>
</form>
```

测试OK！

发现我们的日期显示不完美，可以使用日期工具，进行日期的格式化！

```html
<input name="birth" type="text" class="form-control" th:value="${#dates.format(emp.birth,'yyyy-MM-dd HH:mm')}">
```

 数据回显OK，我们继续完成数据修改问题！

修改表单提交的地址：

```html
<form th:action="@{/updateEmp}" method="post">
```

 编写对应的controller

```java
@PostMapping("/updateEmp")
public String updateEmp(Employee employee){
    employeeDao.save(employee);
    //回到员工列表页面
    return "redirect:/emps";
}
```

发现页面提交的没有id；我们在前端加一个隐藏域，提交id；

```html
 <input name="id" type="hidden" class="form-control" th:value="${emp.id}">
```

 重启，修改信息测试OK！

### 删除员工

list页面，编写提交地址

```html
<a class="btn btn-sm btn-danger" th:href="@{/delEmp/}+${emp.id}">删除</a>
```

 编写Controller

```java
@GetMapping("/delEmp/{id}")
public String delEmp(@PathVariable("id") Integer id){
    employeeDao.delete(id);
    return "redirect:/emps";
}
```

 测试OK！

### 定制错误页面

我们只需要在模板目录下添加一个error文件夹，文件夹中存放我们相应的错误页面，比如404.html 或者 4xx.html 等等，SpringBoot就会帮我们自动使用了！

[![img](https://gitee.com/xudongyin/img/raw/master/img/1418974-20190809235201843-1047725196.png)](https://img2018.cnblogs.com/blog/1418974/201908/1418974-20190809235201843-1047725196.png)

###  注销功能

```html
<a class="nav-link" href="#" th:href="@{/user/loginOut}">Sign out</a>
```

 对应的controller

```java
@GetMapping("/user/loginOut")
public String loginOut(HttpSession session){
    session.invalidate();
    return "redirect:/index.html";
}
```

学到这里，SpringBoot的基本开发就以及没有问题了，我们后面去学习一下SpringBoot如何操作数据库以及配置Mybatis；

# 14、SpringBoot：集成Swagger终极版

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225407880-1259118410.png)

**学习目标：**

- 了解Swagger的概念及作用
- 掌握在项目中集成Swagger自动生成API文档

### 14.1、Swagger简介

**前后端分离**

- 前端 -> 前端控制层、视图层
- 后端 -> 后端控制层、服务层、数据访问层
- 前后端通过API进行交互
- 前后端相对独立且松耦合

**产生的问题**

- 前后端集成，前端或者后端无法做到“及时协商，尽早解决”，最终导致问题集中爆发

**解决方案**

- 首先定义schema [ 计划的提纲 ]，并实时跟踪最新的API，降低集成风险

**Swagger**

- 号称世界上最流行的API框架
- Restful Api 文档在线自动生成器 => **API 文档 与API 定义同步更新**
- 直接运行，在线测试API
- 支持多种语言 （如：Java，PHP等）
- 官网：https://swagger.io/

### 14.2、SpringBoot集成Swagger

**SpringBoot集成Swagger** => **springfox**，两个jar包

- **Springfox-swagger2**
- swagger-springmvc

**使用Swagger**

要求：jdk 1.8 + 否则swagger2无法运行

步骤：

1、新建一个SpringBoot-web项目

2、添加Maven依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.10.5</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.10.5</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-spring-webmvc</artifactId>
    <version>2.10.5</version>
</dependency>
```

3、编写HelloController，测试确保运行成功！

4、要使用Swagger，我们需要在config文件夹下编写一个配置类-SwaggerConfig来配置 Swagger

```java
@Configuration //配置类
@EnableSwagger2WebMvc// 开启Swagger2的自动配置
public class SwaggerConfig {  
}
```

5、访问测试 ：http://localhost:8080/swagger-ui.html ，可以看到swagger的界面；

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225423594-1396861605.png)

### 14.3、配置Swagger

1、Swagger实例Bean是Docket，所以通过配置Docket实例来配置Swaggger。

```java
@Bean //配置docket以配置Swagger具体参数
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2);
}
```

2、可以通过apiInfo()属性配置文档信息

```java
//配置文档信息
private ApiInfo apiInfo() {
   Contact contact = new Contact("联系人名字", "http://xxx.xxx.com/联系人访问链接", "联系人邮箱");
   return new ApiInfo(
           "Swagger学习", // 标题
           "学习演示如何配置Swagger", // 描述
           "v1.0", // 版本
           "http://terms.service.url/组织链接", // 组织链接
           contact, // 联系人信息
           "Apach 2.0 许可", // 许可
           "许可链接", // 许可连接
           new ArrayList<>()// 扩展
  );
}
```

3、Docket 实例关联上 apiInfo()

```java
@Bean
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
}
```

4、重启项目，访问测试 http://localhost:8080/swagger-ui.html 看下效果；

### 14.4、配置扫描接口

1、构建Docket时通过select()方法配置怎么扫描接口。

```java
@Bean
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
      .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
       // 配置如何通过path过滤,即这里只扫描请求以/kuang开头的接口
      .paths(PathSelectors.ant("/kuang/**"))
      .build();
}
```

2、重启项目测试，由于我们配置根据包的路径扫描接口，所以我们只能看到一个类

3、除了通过包路径配置扫描接口外，还可以通过配置其他方式扫描接口，这里注释一下**RequestHandlerSelectors**所有的配置方式：

```java
any() // 扫描所有，项目中的所有接口都会被扫描到
none() // 不扫描接口
// 通过方法上的注解扫描，如withMethodAnnotation(GetMapping.class)只扫描get请求
withMethodAnnotation(final Class<? extends Annotation> annotation)
// 通过类上的注解扫描，如.withClassAnnotation(Controller.class)只扫描有controller注解的类中的接口
withClassAnnotation(final Class<? extends Annotation> annotation)
basePackage(final String basePackage) // 根据包路径扫描接口
```

4、除此之外，我们还可以配置接口扫描过滤，**PathSelectors**配置如何过滤：

```java
@Bean
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
      .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
       // 配置如何通过path过滤,即这里只扫描请求以/kuang开头的接口
      .paths(PathSelectors.ant("/kuang/**"))
      .build();
}
```

5、**PathSelectors**这里的可选值还有

```java
any() // 任何请求都扫描
none() // 任何请求都不扫描
regex(final String pathRegex) // 通过正则表达式控制
ant(final String antPattern) // 通过ant()控制
```

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225444394-644044214.png)

### 14.5、配置Swagger开关

1、通过enable()方法配置是否启用swagger，如果是false，swagger将不能在浏览器中访问了

```java
@Bean
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .enable(false) //配置是否启用Swagger，如果是false，在浏览器将无法访问
      .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
      .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
       // 配置如何通过path过滤,即这里只扫描请求以/kuang开头的接口
      .paths(PathSelectors.ant("/kuang/**"))
      .build();
}
```

2、如何动态配置当项目处于test、dev环境时显示swagger，处于prod时不显示？

```java
@Bean
public Docket docket(Environment environment) {
   // 设置要显示swagger的环境
   Profiles of = Profiles.of("dev", "test");
   // 判断当前是否处于该环境
   // 通过 enable() 接收此参数判断是否要显示
   boolean flag = environment.acceptsProfiles(of);
   
   return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .enable(flag) //配置是否启用Swagger，如果是false，在浏览器将无法访问
      .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
      .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
       // 配置如何通过path过滤,即这里只扫描请求以/kuang开头的接口
      .paths(PathSelectors.ant("/kuang/**"))
      .build();
}
```

3、可以在项目中增加一个dev的配置文件查看效果！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225500625-350010165.png)

### 14.6、配置API分组

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225514634-1503893280.png)

1、如果没有配置分组，默认是default。通过groupName()方法即可配置分组：

```java
@Bean
public Docket docket(Environment environment) {
   return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
      .groupName("hello") // 配置分组
       // 省略配置....
}
```

2、重启项目查看分组

3、如何配置多个分组？配置多个分组只需要配置多个docket即可：

```java
@Bean
public Docket docket1(){
   return new Docket(DocumentationType.SWAGGER_2).groupName("group1");
}
@Bean
public Docket docket2(){
   return new Docket(DocumentationType.SWAGGER_2).groupName("group2");
}
@Bean
public Docket docket3(){
   return new Docket(DocumentationType.SWAGGER_2).groupName("group3");
}
```

4、重启项目查看即可

### 14.7、实体配置

1、新建一个实体类

```java
@ApiModel("用户实体")
public class User {
   @ApiModelProperty("用户名")
   public String username;
   @ApiModelProperty("密码")
   public String password;
}
```

2、只要这个实体在**请求接口**的返回值上（即使是泛型），都能映射到实体项中：

```java
@RequestMapping("/getUser")
public User getUser(){   
	return new User();
}
```

3、重启查看测试

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225529234-1972244754.png)

注：并不是因为@ApiModel这个注解让实体显示在这里了，而是只要出现在接口方法的返回值上的实体都会显示在这里，而@ApiModel和@ApiModelProperty这两个注解只是为实体添加注释的。

@ApiModel为类添加注释

@ApiModelProperty为类属性添加注释

### 14.8、常用注解

Swagger的所有注解定义在io.swagger.annotations包下

下面列一些经常用到的，未列举出来的可以另行查阅说明：

| Swagger注解                                            | 简单说明                                             |
| ------------------------------------------------------ | ---------------------------------------------------- |
| @Api(tags = "xxx模块说明")                             | 作用在模块类上                                       |
| @ApiOperation("xxx接口说明")                           | 作用在接口方法上                                     |
| @ApiModel("xxxPOJO说明")                               | 作用在模型类上：如VO、BO                             |
| @ApiModelProperty(value = "xxx属性说明",hidden = true) | 作用在类方法和属性上，hidden设置为true可以隐藏该属性 |
| @ApiParam("xxx参数说明")                               | 作用在参数、方法和字段上，类似@ApiModelProperty      |

我们也可以给请求的接口配置一些注释

```java
@ApiOperation("狂神的接口")
@PostMapping("/kuang")
@ResponseBody
public String kuang(@ApiParam("这个名字会被返回")String username){
   return username;
}
```

这样的话，可以给一些比较难理解的属性或者接口，增加一些配置信息，让人更容易阅读！

相较于传统的Postman或Curl方式测试接口，使用swagger简直就是傻瓜式操作，不需要额外说明文档(写得好本身就是文档)而且更不容易出错，只需要录入数据然后点击Execute，如果再配合自动化框架，可以说基本就不需要人为操作了。

Swagger是个优秀的工具，现在国内已经有很多的中小型互联网公司都在使用它，相较于传统的要先出Word接口文档再测试的方式，显然这样也更符合现在的快速迭代开发行情。当然了，提醒下大家在正式环境要记得关闭Swagger，一来出于安全考虑二来也可以节省运行时内存。

### 14.9、拓展：其他皮肤

我们可以导入不同的包实现不同的皮肤定义：

1、默认的 **访问 http://localhost:8080/swagger-ui.html**

```xml
<dependency> 
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger-ui</artifactId>
   <version>2.9.2</version>
</dependency>
```

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225545785-2143727145.png)

2、bootstrap-ui **访问 http://localhost:8080/doc.html**

```xml
<!-- 引入swagger-bootstrap-ui包 /doc.html-->
<dependency>
   <groupId>com.github.xiaoymin</groupId>
   <artifactId>swagger-bootstrap-ui</artifactId>
   <version>1.9.1</version>
</dependency>
```

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225558425-1483910.png)

3、Layui-ui **访问 http://localhost:8080/docs.html**

```xml
<!-- 引入swagger-ui-layer包 /docs.html-->
<dependency>
   <groupId>com.github.caspar-chen</groupId>
   <artifactId>swagger-ui-layer</artifactId>
   <version>1.1.3</version>
</dependency>
```

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225610315-997693293.png)

4、mg-ui **访问 http://localhost:8080/document.html**

```xml
<!-- 引入swagger-ui-layer包 /document.html-->
<dependency>
   <groupId>com.zyplayer</groupId>
   <artifactId>swagger-mg-ui</artifactId>
   <version>1.0.6</version>
</dependency>
```

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225623225-1419329566.png)

# 15、SpringBoot：异步、定时、邮件任务

前言

> 在我们的工作中，常常会用到异步处理任务，比如我们在网站上发送邮件，后台会去发送邮件，此时前台会造成响应不动，直到邮件发送完毕，响应才会成功，所以我们一般会采用多线程的方式去处理这些任务。还有一些定时任务，比如需要在每天凌晨的时候，分析一次前一天的日志信息。还有就是邮件的发送，微信的前身也是邮件服务呢？这些东西都是怎么实现的呢？其实SpringBoot都给我们提供了对应的支持，我们上手使用十分的简单，只需要开启一些注解支持，配置一些配置文件即可！那我们来看看吧~
>

## 15.1、异步任务

1、创建一个service包

2、创建一个类AsyncService

异步处理还是非常常用的，比如我们在网站上发送邮件，后台会去发送邮件，此时前台会造成响应不动，直到邮件发送完毕，响应才会成功，所以我们一般会采用多线程的方式去处理这些任务。

编写方法，假装正在处理数据，使用线程设置一些延时，模拟同步等待的情况；

```java
@Service
public class AsyncService {

   public void hello(){
       try {
           Thread.sleep(3000);
      } catch (InterruptedException e) {
           e.printStackTrace();
      }
       System.out.println("业务进行中....");
  }
}
```

3、创建controller包

4、编写AsyncController类

我们去写一个Controller测试一下

```java
@RestController
public class AsyncController {

   @Autowired
   AsyncService asyncService;

   @GetMapping("/hello")
   public String hello(){
       asyncService.hello();
       return "success";
  }

}
```

5、访问http://localhost:8080/hello进行测试，3秒后出现success，这是同步等待的情况。

问题：我们如果想让用户直接得到消息，就在后台使用多线程的方式进行处理即可，但是每次都需要自己手动去编写多线程的实现的话，太麻烦了，我们只需要用一个简单的办法，在我们的方法上加一个简单的注解即可，如下：

6、给hello方法添加@Async注解；

```java
//告诉Spring这是一个异步方法
@Async
public void hello(){
   try {
       Thread.sleep(3000);
  } catch (InterruptedException e) {
       e.printStackTrace();
  }
   System.out.println("业务进行中....");
}
```

SpringBoot就会自己开一个线程池，进行调用！但是要让这个注解生效，我们还需要在主程序上添加一个注解@EnableAsync ，开启异步注解功能；

```java
@EnableAsync //开启异步注解功能
@SpringBootApplication
public class SpringbootTaskApplication {

   public static void main(String[] args) {
       SpringApplication.run(SpringbootTaskApplication.class, args);
  }

}
```

7、重启测试，网页瞬间响应，后台代码依旧执行！

## 15.2、定时任务

项目开发中经常需要执行一些定时任务，比如需要在每天凌晨的时候，分析一次前一天的日志信息，Spring为我们提供了异步执行任务调度的方式，提供了两个接口。

- TaskExecutor接口
- TaskScheduler接口

两个注解：

- @EnableScheduling
- @Scheduled

**cron表达式：**

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225641164-1934062088.png)

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225651289-251704533.png)

**测试步骤：**

1、创建一个ScheduledService

我们里面存在一个hello方法，他需要定时执行，怎么处理呢？

```java
@Service
public class ScheduledService {
   
   //秒   分   时     日   月   周几
   //0 * * * * MON-FRI
   //注意cron表达式的用法；
   @Scheduled(cron = "0 * * * * 0-7")
   public void hello(){
       System.out.println("hello.....");
  }
}
```

2、这里写完定时任务之后，我们需要在主程序上增加@EnableScheduling 开启定时任务功能

```java
@EnableAsync //开启异步注解功能
@EnableScheduling //开启基于注解的定时任务
@SpringBootApplication
public class SpringbootTaskApplication {

   public static void main(String[] args) {
       SpringApplication.run(SpringbootTaskApplication.class, args);
  }

}
```

3、我们来详细了解下cron表达式；

http://www.bejson.com/othertools/cron/

4、常用的表达式

```
 (1）0/2 * * * * ?   表示每2秒 执行任务
（1）0 0/2 * * * ?   表示每2分钟 执行任务
（1）0 0 2 1 * ?   表示在每月的1日的凌晨2点调整任务
（2）0 15 10 ? * MON-FRI   表示周一到周五每天上午10:15执行作业
（3）0 15 10 ? 6L 2002-2006   表示2002-2006年的每个月的最后一个星期五上午10:15执行作
（4）0 0 10,14,16 * * ?   每天上午10点，下午2点，4点
（5）0 0/30 9-17 * * ?   朝九晚五工作时间内每半小时
（6）0 0 12 ? * WED   表示每个星期三中午12点
（7）0 0 12 * * ?   每天中午12点触发
（8）0 15 10 ? * *   每天上午10:15触发
（9）0 15 10 * * ?     每天上午10:15触发
（10）0 15 10 * * ?   每天上午10:15触发
（11）0 15 10 * * ? 2005   2005年的每天上午10:15触发
（12）0 * 14 * * ?     在每天下午2点到下午2:59期间的每1分钟触发
（13）0 0/5 14 * * ?   在每天下午2点到下午2:55期间的每5分钟触发
（14）0 0/5 14,18 * * ?     在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发
（15）0 0-5 14 * * ?   在每天下午2点到下午2:05期间的每1分钟触发
（16）0 10,44 14 ? 3 WED   每年三月的星期三的下午2:10和2:44触发
（17）0 15 10 ? * MON-FRI   周一至周五的上午10:15触发
（18）0 15 10 15 * ?   每月15日上午10:15触发
（19）0 15 10 L * ?   每月最后一日的上午10:15触发
（20）0 15 10 ? * 6L   每月的最后一个星期五上午10:15触发
（21）0 15 10 ? * 6L 2002-2005   2002年至2005年的每月的最后一个星期五上午10:15触发
（22）0 15 10 ? * 6#3   每月的第三个星期五上午10:15触发
```

## 15.3、邮件任务

邮件发送，在我们的日常开发中，也非常的多，Springboot也帮我们做了支持

- 邮件发送需要引入spring-boot-start-mail
- SpringBoot 自动配置MailSenderAutoConfiguration
- 定义MailProperties内容，配置在application.yml中
- 自动装配JavaMailSender
- 测试邮件发送

**测试：**

1、引入pom依赖

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

看它引入的依赖，可以看到 jakarta.mail

```xml
<dependency>
   <groupId>com.sun.mail</groupId>
   <artifactId>jakarta.mail</artifactId>
   <version>1.6.4</version>
   <scope>compile</scope>
</dependency>
```

2、查看自动配置类：MailSenderAutoConfiguration

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225708744-252995174.png)

这个类中存在bean，JavaMailSenderImpl

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225721465-1009548816.png)

然后我们去看下配置文件

```java
@ConfigurationProperties(
   prefix = "spring.mail"
)
public class MailProperties {
   private static final Charset DEFAULT_CHARSET;
   private String host;
   private Integer port;
   private String username;
   private String password;
   private String protocol = "smtp";
   private Charset defaultEncoding;
   private Map<String, String> properties;
   private String jndiName;
}
```

3、配置文件：

```properties
spring.mail.username=24736743@qq.com
spring.mail.password=你的qq授权码
spring.mail.host=smtp.qq.com    #qq需要配置ssl
spring.mail.properties.mail.smtp.ssl.enable=true
```

获取授权码：在QQ邮箱中的设置->账户->开启pop3和smtp服务

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225736355-1251829128.png)

4、Spring单元测试

```java
@Autowired
JavaMailSenderImpl mailSender;

@Test
public void contextLoads() {
   //邮件设置1：一个简单的邮件
   SimpleMailMessage message = new SimpleMailMessage();
   message.setSubject("通知-明天来狂神这听课");
   message.setText("今晚7:30开会");

   message.setTo("24736743@qq.com");
   message.setFrom("24736743@qq.com");
   mailSender.send(message);
}

@Test
public void contextLoads2() throws MessagingException {
   //邮件设置2：一个复杂的邮件
   MimeMessage mimeMessage = mailSender.createMimeMessage();
    //要发送附件得带上 true 参数
   MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);

   helper.setSubject("通知-明天来狂神这听课");
   helper.setText("<b style='color:red'>今天 7:30来开会</b>",true);

   //发送附件
   helper.addAttachment("1.jpg",new File(""));
   helper.addAttachment("2.jpg",new File(""));

   helper.setTo("24736743@qq.com");
   helper.setFrom("24736743@qq.com");

   mailSender.send(mimeMessage);
}
```

查看邮箱，邮件接收成功！

我们只需要使用Thymeleaf进行前后端结合即可开发自己网站邮件收发功能了！

# 16、SpringBoot：富文本编辑器

### 16.1、简介

思考：我们平时在博客园，或者CSDN等平台进行写作的时候，有同学思考过他们的编辑器是怎么实现的吗？

在博客园后台的选项设置中，可以看到一个文本编辑器的选项：

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225751589-577736769.png)

其实这个就是富文本编辑器，市面上有许多非常成熟的富文本编辑器，比如：

- **Editor.md**——功能非常丰富的编辑器，左端编辑，右端预览，非常方便，完全免费
- - 官网：https://pandao.github.io/editor.md/
- **wangEditor**——基于javascript和css开发的 Web富文本编辑器， 轻量、简洁、界面美观、易用、开源免费。
- - 官网：http://www.wangeditor.com/
- **TinyMCE**——TinyMCE是一个轻量级的基于浏览器的所见即所得编辑器，由JavaScript写成。它对IE6+和Firefox1.5+都有着非常良好的支持。功能齐全，界面美观，就是文档是英文的，对开发人员英文水平有一定要求。
- - 官网：https://www.tiny.cloud/docs/demo/full-featured/
  - 博客园
- **百度ueditor**——UEditor是由百度web前端研发部开发所见即所得富文本web编辑器，具有轻量，功能齐全，可定制，注重用户体验等特点，开源基于MIT协议，允许自由使用和修改代码，缺点是已经没有更新了
- - 官网：https://ueditor.baidu.com/website/onlinedemo.html
- **kindeditor**——界面经典。
- - 官网：http://kindeditor.net/demo.php
- **Textbox**——Textbox是一款极简但功能强大的在线文本编辑器，支持桌面设备和移动设备。主要功能包含内置的图像处理和存储、文件拖放、拼写检查和自动更正。此外，该工具还实现了屏幕阅读器等辅助技术，并符合WAI-ARIA可访问性标准。
- - 官网：https://textbox.io/
- **CKEditor**——国外的，界面美观。
- - 官网：https://ckeditor.com/ckeditor-5/demo/
- **quill**——功能强大，还可以编辑公式等
- - 官网：https://quilljs.com/
- **simditor**——界面美观，功能较全。
- - 官网：https://simditor.tower.im/
- **summernote**——UI好看，精美
- - 官网：https://summernote.org/
- **jodit**——功能齐全
- - 官网：https://xdsoft.net/jodit/
- **froala Editor**——界面非常好看，功能非常强大，非常好用（非免费）
- - 官网：https://www.froala.com/wysiwyg-editor

总之，目前可用的富文本编辑器有很多......这只是其中的一部分

### 16.2、Editor.md

我这里使用的就是Editor.md，作为一个资深码农，Mardown必然是我们程序猿最喜欢的格式，看下面，就爱上了！

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225807875-1956792074.png)

我们可以在官网下载它：https://pandao.github.io/editor.md/ ， 得到它的压缩包！

解压以后，在examples目录下面，可以看到他的很多案例使用！学习，其实就是看人家怎么写的，然后进行模仿就好了！

我们可以将整个解压的文件倒入我们的项目，将一些无用的测试和案例删掉即可！

### 16.3、基础工程搭建

> 数据库设计

article：文章表

| 字段    |          | 备注         |
| ------- | -------- | ------------ |
| id      | int      | 文章的唯一ID |
| author  | varchar  | 作者         |
| title   | varchar  | 标题         |
| content | longtext | 文章的内容   |

建表SQL：

```sql
CREATE TABLE `article` (
`id` int(10) NOT NULL AUTO_INCREMENT COMMENT 'int文章的唯一ID',
`author` varchar(50) NOT NULL COMMENT '作者',
`title` varchar(100) NOT NULL COMMENT '标题',
`content` longtext NOT NULL COMMENT '文章的内容',
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

> 基础项目搭建

1、建一个SpringBoot项目配置

```yml
spring:
datasource:
  username: root
  password: 123456
  #?serverTimezone=UTC解决时区的报错
  url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
  driver-class-name: com.mysql.cj.jdbc.Driver
<resources>
   <resource>
       <directory>src/main/java</directory>
       <includes>
           <include>**/*.xml</include>
       </includes>
       <filtering>true</filtering>
   </resource>
</resources>
```

2、实体类：

```java
//文章类
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Article implements Serializable {

   private int id; //文章的唯一ID
   private String author; //作者名
   private String title; //标题
   private String content; //文章的内容

}
```

3、mapper接口：

```java
@Mapper
@Repository
public interface ArticleMapper {
   //查询所有的文章
   List<Article> queryArticles();

   //新增一个文章
   int addArticle(Article article);

   //根据文章id查询文章
   Article getArticleById(int id);

   //根据文章id删除文章
   int deleteArticleById(int id);

}
```

**既然已经提供了 myBatis 的映射配置文件，自然要告诉 spring boot 这些文件的位置**

```yml
mybatis:
 mapper-locations: classpath:com/kuang/mapper/*.xml
 type-aliases-package: com.kuang.pojo
```

编写一个Controller测试下，是否ok；

### 16.4、文章编辑整合（重点）

1、导入 editor.md 资源 ，删除多余文件

2、编辑文章页面 editor.html、需要引入 jQuery；

```html
<!DOCTYPE html>
<html class="x-admin-sm" lang="zh" xmlns:th="http://www.thymeleaf.org">

    <head>
        <meta charset="UTF-8">
        <title>秦疆'Blog</title>
        <meta name="renderer" content="webkit">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <meta name="viewport" content="width=device-width,user-scalable=yes, minimum-scale=0.4, initial-scale=0.8,target-densitydpi=low-dpi" />
        <!--Editor.md-->
        <link rel="stylesheet" th:href="@{/editormd/css/editormd.css}"/>
        <link rel="shortcut icon" href="https://pandao.github.io/editor.md/favicon.ico" type="image/x-icon" />
    </head>

    <body>

        <div class="layui-fluid">
            <div class="layui-row layui-col-space15">
                <div class="layui-col-md12">
                    <!--博客表单-->
                    <form name="mdEditorForm">
                        <div>
                            标题：<input type="text" name="title">
                        </div>
                        <div>
                            作者：<input type="text" name="author">
                        </div>
                        <div id="article-content">
                            <textarea name="content" id="content" style="display:none;"> </textarea>
                        </div>
                    </form>

                </div>
            </div>
        </div>
    </body>

    <!--editormd-->
    <script th:src="@{/editormd/lib/jquery.min.js}"></script>
    <script th:src="@{/editormd/editormd.js}"></script>

    <script type="text/javascript">
        var testEditor;

        //window.onload = function(){ }
        $(function() {
            testEditor = editormd("article-content", {
                width : "95%",
                height : 400,
                syncScrolling : "single",
                path : "../editormd/lib/",
                saveHTMLToTextarea : true,    // 保存 HTML 到 Textarea
                emoji: true,
                theme: "dark",//工具栏主题
                previewTheme: "dark",//预览主题
                editorTheme: "pastel-on-dark",//编辑主题
                tex : true,                   // 开启科学公式TeX语言支持，默认关闭
                flowChart : true,             // 开启流程图支持，默认关闭
                sequenceDiagram : true,       // 开启时序/序列图支持，默认关闭,
                //图片上传
                imageUpload : true,
                imageFormats : ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
                imageUploadURL : "/article/file/upload",
                onload : function() {
                    console.log('onload', this);
                },
                /*指定需要显示的功能按钮*/
                toolbarIcons : function() {
                    return ["undo","redo","|",
                            "bold","del","italic","quote","ucwords","uppercase","lowercase","|",
                            "h1","h2","h3","h4","h5","h6","|",
                            "list-ul","list-ol","hr","|",
                            "link","reference-link","image","code","preformatted-text",
                            "code-block","table","datetime","emoji","html-entities","pagebreak","|",
                            "goto-line","watch","preview","fullscreen","clear","search","|",
                            "help","info","releaseIcon", "index"]
                },

                /*自定义功能按钮，下面我自定义了2个，一个是发布，一个是返回首页*/
                toolbarIconTexts : {
                    releaseIcon : "<span bgcolor=\"gray\">发布</span>",
                    index : "<span bgcolor=\"red\">返回首页</span>",
                },

                /*给自定义按钮指定回调函数*/
                toolbarHandlers:{
                    releaseIcon : function(cm, icon, cursor, selection) {
                        //表单提交
                        mdEditorForm.method = "post";
                        mdEditorForm.action = "/article/addArticle";//提交至服务器的路径
                        mdEditorForm.submit();
                    },
                    index : function(){
                        window.location.href = '/';
                    },
                }
            });
        });
    </script>

</html>
```

3、编写Controller，进行跳转，以及保存文章

```java
@Controller
@RequestMapping("/article")
public class ArticleController {

   @GetMapping("/toEditor")
   public String toEditor(){
       return "editor";
  }
   
   @PostMapping("/addArticle")
   public String addArticle(Article article){
       articleMapper.addArticle(article);
       return "editor";
  }
   
}
```

> 图片上传问题

1、前端js中添加配置

```http
//图片上传
imageUpload : true,
imageFormats : ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
imageUploadURL : "/article/file/upload", // //这个是上传图片时的访问地址
```

2、后端请求，接收保存这个图片, 需要导入 FastJson 的依赖！

```java
//博客图片上传问题
@RequestMapping("/file/upload")
@ResponseBody
public JSONObject fileUpload(@RequestParam(value = "editormd-image-file", required = true) MultipartFile file, HttpServletRequest request) throws IOException {
   //上传路径保存设置

   //获得SpringBoot当前项目的路径：System.getProperty("user.dir")
   String path = System.getProperty("user.dir")+"/upload/";

   //按照月份进行分类：
   Calendar instance = Calendar.getInstance();
   String month = (instance.get(Calendar.MONTH) + 1)+"月";
   path = path+month;

   File realPath = new File(path);
   if (!realPath.exists()){
       realPath.mkdir();
  }

   //上传文件地址
   System.out.println("上传文件保存地址："+realPath);

   //解决文件名字问题：我们使用uuid;
   String filename = "ks-"+UUID.randomUUID().toString().replaceAll("-", "");
   //通过CommonsMultipartFile的方法直接写文件（注意这个时候）
   file.transferTo(new File(realPath +"/"+ filename));

   //给editormd进行回调
   JSONObject res = new JSONObject();
   res.put("url","/upload/"+month+"/"+ filename);
   res.put("success", 1);
   res.put("message", "upload success!");

   return res;
}
```

3、解决文件回显显示的问题，设置虚拟目录映射！在我们自己拓展的MvcConfig中进行配置即可！

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

   // 文件保存在真实目录/upload/下，
   // 访问的时候使用虚路径/upload，比如文件名为1.png，就直接/upload/1.png就ok了。
   @Override
   public void addResourceHandlers(ResourceHandlerRegistry registry) {
       registry.addResourceHandler("/upload/**")
          .addResourceLocations("file:"+System.getProperty("user.dir")+"/upload/");
  }

}
```

> 表情包问题

自己手动下载，emoji 表情包，放到图片路径下：

修改editormd.js文件

```js
// Emoji graphics files url path
editormd.emoji     = {
   path : "../editormd/plugins/emoji-dialog/emoji/",
   ext   : ".png"
};
```

### 16.5、文章展示

1、Controller 中增加方法

```java
@GetMapping("/{id}")
public String show(@PathVariable("id") int id,Model model){
   Article article = articleMapper.getArticleById(id);
   model.addAttribute("article",article);
   return "article";
}
```

2、编写页面 article.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
        <title th:text="${article.title}"></title>
    </head>
    <body>

        <div>
            <!--文章头部信息：标题，作者，最后更新日期，导航-->
            <h2 style="margin: auto 0" th:text="${article.title}"></h2>
            作者：<span style="float: left" th:text="${article.author}"></span>
            <!--文章主体内容-->
            <div id="doc-content">
                <textarea style="display:none;" placeholder="markdown" th:text="${article.content}"></textarea>
            </div>

        </div>

        <link rel="stylesheet" th:href="@{/editormd/css/editormd.preview.css}" />
        <script th:src="@{/editormd/lib/jquery.min.js}"></script>
        <script th:src="@{/editormd/lib/marked.min.js}"></script>
        <script th:src="@{/editormd/lib/prettify.min.js}"></script>
        <script th:src="@{/editormd/lib/raphael.min.js}"></script>
        <script th:src="@{/editormd/lib/underscore.min.js}"></script>
        <script th:src="@{/editormd/lib/sequence-diagram.min.js}"></script>
        <script th:src="@{/editormd/lib/flowchart.min.js}"></script>
        <script th:src="@{/editormd/lib/jquery.flowchart.min.js}"></script>
        <script th:src="@{/editormd/editormd.js}"></script>

        <script type="text/javascript">
            var testEditor;
            $(function () {
                testEditor = editormd.markdownToHTML("doc-content", {//注意：这里是上面DIV的id
                    htmlDecode: "style,script,iframe",
                    emoji: true,
                    taskList: true,
                    tocm: true,
                    tex: true, // 默认不解析
                    flowChart: true, // 默认不解析
                    sequenceDiagram: true, // 默认不解析
                    codeFold: true
                });});
        </script>
    </body>
</html>
```

重启项目，访问进行测试！大功告成！

小结：

有了富文本编辑器，我们网站的功能就会又多一项，大家到了这里完全可以有时间写一个属于自己的博客网站了，根据所学的知识是完全没有任何问题的！

# 17、SpringBoot：Dubbo和Zookeeper集成

分布式理论

## **17.1、什么是分布式系统？**

在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；

分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是**利用更多的机器，处理更多的数据**。

分布式系统（distributed system）是建立在网络之上的软件系统。

首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。。。

## **17.2、Dubbo文档**

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，急需**一个治理系统**确保架构有条不紊的演进。

在Dubbo的官网文档有这样一张图

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225836241-1744130417.png)

> **单一应用架构**

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225848354-475809714.png)

适用于小型网站，小型管理系统，将所有功能都部署到一个功能里，简单易用。

**缺点：**

1、性能扩展比较难

2、协同开发问题

3、不利于升级维护

> **垂直应用架构**

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412225901199-858224740.png)

通过切分业务来实现各个模块独立部署，降低了维护和部署的难度，团队各司其职更易管理，性能扩展也更方便，更有针对性。

缺点：公用模块无法重复利用，开发性的浪费

> **分布式服务架构**

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的**分布式服务框架(RPC)**是关键。

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225913839-69515130.png)

> **流动计算架构**

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于**提高机器利用率的资源调度和治理中心**(SOA)[ Service Oriented Architecture]是关键。

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225925894-2054203963.png)

**什么是RPC**

RPC【Remote Procedure Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；

推荐阅读文章：https://www.jianshu.com/p/2accc2840a1b

**RPC基本原理**

<img src="https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225942424-634751471.png" alt="img" style="zoom:80%;" />

**步骤解析：**

<img src="https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412225954994-2069522644.png" alt="img" style="zoom:80%;" />

RPC两个核心模块：**通讯，序列化**。

测试环境搭建

### **Dubbo**

Apache Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

dubbo官网 http://dubbo.apache.org/zh-cn/index.html

1.了解Dubbo的特性

2.查看官方文档

> **dubbo基本概念**

<img src="https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230011744-1175379704.png" alt="img" style="zoom:67%;" />

**服务提供者**（Provider）：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。

**服务消费者**（Consumer）：调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

**注册中心**（Registry）：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

**监控中心**（Monitor）：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

**调用关系说明**

l 服务容器负责启动，加载，运行服务提供者。

l 服务提供者在启动时，向注册中心注册自己提供的服务。

l 服务消费者在启动时，向注册中心订阅自己所需的服务。

l 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。

l 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

l 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

> **Dubbo环境搭建**

点进dubbo官方文档，推荐我们使用Zookeeper 注册中心

什么是zookeeper呢？可以查看官方文档

**Window下安装zookeeper**

1、下载zookeeper ：[地址](https://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.14/)， 我们下载3.4.14 ， 最新版！解压zookeeper

2、运行/bin/zkServer.cmd ，初次运行会报错，没有zoo.cfg配置文件；

可能遇到问题：闪退 !

解决方案：编辑zkServer.cmd文件末尾添加pause 。这样运行出错就不会退出，会提示错误信息，方便找到原因。

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230026479-947348663.png)

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230037329-740930228.png)

3、修改zoo.cfg配置文件

将conf文件夹下面的zoo_sample.cfg复制一份改名为zoo.cfg即可。

注意几个重要位置：

dataDir=./ 临时数据存储的目录（可写相对路径）

clientPort=2181 zookeeper的端口号

修改完成后再次启动zookeeper

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230050035-1907960449.png)

4、使用zkCli.cmd测试

ls /：列出zookeeper根下保存的所有节点

```bash
[zk: 127.0.0.1:2181(CONNECTED) 4] ls /
[zookeeper]
```

create –e /kuangshen 123：创建一个kuangshen节点，值为123

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230103589-1318102017.png)

get /kuangshen：获取/kuangshen节点的值

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230116159-2103649071.png)

我们再来查看一下节点

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230133804-1513007496.png)

**window下安装dubbo-admin**

dubbo本身并不是一个服务软件。它其实就是一个jar包，能够帮你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。

但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序dubbo-admin，不过这个监控即使不装也不影响使用。

我们这里来安装一下：

**1、下载dubbo-admin**

地址 ：https://github.com/apache/dubbo-admin/tree/master

**2、解压进入目录**

修改 dubbo-admin\src\main\resources \application.properties 指定zookeeper地址

```properties
server.port=7001
spring.velocity.cache=false
spring.velocity.charset=UTF-8
spring.velocity.layout-url=/templates/default.vm
spring.messages.fallback-to-system-locale=false
spring.messages.basename=i18n/message
spring.root.password=root
spring.guest.password=guest
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3、在项目根目录下**打包dubbo-admin

```shell
mvn clean package -Dmaven.test.skip=true
```

**第一次打包的过程有点慢，需要耐心等待！直到成功！**

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230149119-363508332.png)

4、执行 dubbo-admin\target 下的dubbo-admin-0.0.1-SNAPSHOT.jar

```shell
java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
```

【注意：zookeeper的服务一定要打开！】

执行完毕，我们去访问一下 http://localhost:7001/ ， 这时候我们需要输入登录账户和密码，我们都是默认的**root-root**；

登录成功后，查看界面

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230202910-800109291.png)

安装完成！

## 17.3、SpringBoot + Dubbo + zookeeper

**框架搭建**

**1. 启动zookeeper ！**

**2. IDEA创建一个空项目；**

**3.创建一个模块，实现服务提供者：provider-server ， 选择web依赖即可**

**4.项目创建完毕，我们写一个服务，比如卖票的服务；**

编写接口

```java
package com.kuang.provider.service;

public interface TicketService {
   public String getTicket();
}
```

编写实现类

```java
package com.kuang.provider.service;

public class TicketServiceImpl implements TicketService {
   @Override
   public String getTicket() {
       return "《狂神说Java》";
  }
}
```

**5.创建一个模块，实现服务消费者：consumer-server ， 选择web依赖即可**

**6.项目创建完毕，我们写一个服务，比如用户的服务；**

编写service

```java
package com.kuang.consumer.service;

public class UserService {
   //我们需要去拿去注册中心的服务
}
```

**需求：现在我们的用户想使用买票的服务，这要怎么弄呢 ？**

**服务提供者**

**1、将服务提供者注册到注册中心，我们需要整合Dubbo和zookeeper，所以需要导包**

**我们从dubbo官网进入github，看下方的帮助文档，找到dubbo-springboot，找到依赖包**

```xml
<!-- Dubbo Spring Boot Starter -->
<dependency>
   <groupId>org.apache.dubbo</groupId>
   <artifactId>dubbo-spring-boot-starter</artifactId>
   <version>2.7.3</version>
</dependency>    
```

**zookeeper的包我们去maven仓库下载，zkclient；**

```xml
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
   <groupId>com.github.sgroschupf</groupId>
   <artifactId>zkclient</artifactId>
   <version>0.1</version>
</dependency>
```

**【新版的坑】zookeeper及其依赖包，解决日志冲突，还需要剔除日志依赖；**

```xml
<!-- 引入zookeeper -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>2.12.0</version>
</dependency>
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>2.12.0</version>
</dependency>
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.14</version>
    <!--排除这个slf4j-log4j12-->
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

**2、在springboot配置文件中配置dubbo相关属性！**

```properties
#当前应用名字，对应项目模块名称
dubbo.application.name=provider-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
#扫描指定包下服务
dubbo.scan.base-packages=com.kuang.provider.service
```

**3、在service的实现类中配置服务注解，发布服务！注意导包问题**

```java
import org.apache.dubbo.config.annotation.Service;
import org.springframework.stereotype.Component;

@Service //将服务发布出去，这个service注解不是spring的service注解
@Component //放在容器中
public class TicketServiceImpl implements TicketService {
   @Override
   public String getTicket() {
       return "《狂神说Java》";
  }
}
```

**逻辑理解 ：应用启动起来，dubbo就会扫描指定的包下带有@component注解的服务，将它发布在指定的注册中心中！**

**服务消费者**

**1、导入依赖，和之前的依赖一样；**

```xml
<!--dubbo-->
<!-- Dubbo Spring Boot Starter -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.3</version>
</dependency>
<!--zookeeper-->
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
</dependency>
<!-- 引入zookeeper -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>2.12.0</version>
</dependency>
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>2.12.0</version>
</dependency>
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.14</version>
    <!--排除这个slf4j-log4j12-->
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

2、**配置参数**

```properties
#当前应用名字
dubbo.application.name=consumer-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3. 本来正常步骤是需要将服务提供者的接口打包，然后用pom文件导入，我们这里使用简单的方式，直接将服务的接口拿过来，路径必须保证正确，即和服务提供者相同；**

<img src="https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230223249-22793107.png" alt="img" style="zoom: 67%;" />

**4. 完善消费者的服务类**

```java
package com.kuang.consumer.service;

import com.kuang.provider.service.TicketService;
import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Service;

@Service //注入到容器中
public class UserService {

   @Reference //远程引用指定的服务，他会按照全类名进行匹配，看谁给注册中心注册了这个全类名
   TicketService ticketService;

   public void bugTicket(){
       String ticket = ticketService.getTicket();
       System.out.println("在注册中心买到"+ticket);
  }

}
```

**5. 测试类编写；**

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ConsumerServerApplicationTests {

   @Autowired
   UserService userService;

   @Test
   public void contextLoads() {

       userService.bugTicket();

  }

}
```

**启动测试**

**1. 开启zookeeper**

**2. 打开dubbo-admin实现监控【可以不用做】**

**3. 开启服务者**

**4. 消费者消费测试，结果：**

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230239279-1442230316.png)

**监控中心 ：**

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230254269-261185375.png)

**ok , 这就是SpingBoot + dubbo + zookeeper实现分布式开发的应用，其实就是一个服务拆分的思想；**

# 18、SpringBoot：集成SpringSecurity

## 18.1、安全简介

在 Web 开发中，安全一直是非常重要的一个方面。安全虽然属于应用的非功能性需求，但是应该在应用开发的初期就考虑进来。如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：一方面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；另一方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而需要更多的开发时间，影响应用的发布进程。因此，从应用开发的第一天就应该把安全相关的因素考虑进来，并在整个应用的开发过程中。

市面上存在比较有名的：Shiro，Spring Security ！

这里需要阐述一下的是，每一个框架的出现都是为了解决某一问题而产生了，那么Spring Security框架的出现是为了解决什么问题呢？

首先我们看下它的官网介绍：Spring Security官网地址

Spring Security is a powerful and highly customizable authentication and access-control framework. It is the de-facto standard for securing Spring-based applications.

Spring Security is a framework that focuses on providing both authentication and authorization to Java applications. Like all Spring projects, the real power of Spring Security is found in how easily it can be extended to meet custom requirements

Spring Security是一个功能强大且高度可定制的身份验证和访问控制框架。它实际上是保护基于spring的应用程序的标准。

Spring Security是一个框架，侧重于为Java应用程序提供身份验证和授权。与所有Spring项目一样，Spring安全性的真正强大之处在于它可以轻松地扩展以满足定制需求

从官网的介绍中可以知道这是一个权限框架。想我们之前做项目是没有使用框架是怎么控制权限的？对于权限 一般会细分为功能权限，访问权限，和菜单权限。代码会写的非常的繁琐，冗余。

怎么解决之前写权限代码繁琐，冗余的问题，一些主流框架就应运而生而Spring Scecurity就是其中的一种。

Spring 是一个非常流行和成功的 Java 应用开发框架。Spring Security 基于 Spring 框架，提供了一套 Web 应用安全性的完整解决方案。一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分。用户认证指的是验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。

对于上面提到的两种应用情景，Spring Security 框架都有很好的支持。在用户认证方面，Spring Security 框架支持主流的认证方式，包括 HTTP 基本认证、HTTP 表单验证、HTTP 摘要认证、OpenID 和 LDAP 等。在用户授权方面，Spring Security 提供了基于角色的访问控制和访问控制列表（Access Control List，ACL），可以对应用中的领域对象进行细粒度的控制。

## 18.2、实战测试

### 18.2.1、实验环境搭建

1、新建一个初始的springboot项目web模块，thymeleaf模块

2、导入静态资源

```
|views
   |level1
     1.html
     2.html
     3.html
   |level2
	1.html
	2.html
	3.html
   |level3
	1.html
	2.html
	3.html
   |login.html
index.html
```

3、controller跳转！

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class RouterController {

   @RequestMapping({"/","/index"})
   public String index(){
       return "index";
  }

   @RequestMapping("/toLogin")
   public String toLogin(){
       return "views/login";
  }

   @RequestMapping("/level1/{id}")
   public String level1(@PathVariable("id") int id){
       return "views/level1/"+id;
  }

   @RequestMapping("/level2/{id}")
   public String level2(@PathVariable("id") int id){
       return "views/level2/"+id;
  }

   @RequestMapping("/level3/{id}")
   public String level3(@PathVariable("id") int id){
       return "views/level3/"+id;
  }

}
```

4、测试实验环境是否OK！

### 18.2.2、认识SpringSecurity

Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式

Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。

**“认证”（Authentication）**

身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。

身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

**“授权” （Authorization）**

授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。

这个概念是通用的，而不是只在Spring Security 中存在。

### 18.2.3、认证和授权

目前，我们的测试环境，是谁都可以访问的，我们使用 Spring Security 增加上认证和授权的功能

1、引入 Spring Security 模块

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

2、编写 Spring Security 配置类

参考官网：https://spring.io/projects/spring-security

查看我们自己项目中的版本，找到对应的帮助文档：

https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5

3、编写基础配置类

```java
package com.kuang.config;

import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@EnableWebSecurity // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {

   @Override
   protected void configure(HttpSecurity http) throws Exception {
       
  }
}
```

4、定制请求的授权规则

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
   // 定制请求的授权规则
   // 首页所有人可以访问
   http.authorizeRequests().antMatchers("/").permitAll()
  .antMatchers("/level1/**").hasRole("vip1")
  .antMatchers("/level2/**").hasRole("vip2")
  .antMatchers("/level3/**").hasRole("vip3");
}
```

5、测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！

6、在configure()方法中加入以下配置，开启自动配置的登录功能！

```java
// 开启自动配置的登录功能
// /login 请求来到登录页
// /login?error 重定向到这里表示登录失败
http.formLogin();
```

7、测试一下：发现，没有权限的时候，会跳转到登录的页面！

8、查看刚才登录页的注释信息；

我们可以定义认证规则，重写configure(AuthenticationManagerBuilder auth)方法

```java
//定义认证规则
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   
   //在内存中定义，也可以在jdbc中去拿....
   auth.inMemoryAuthentication()
          .withUser("kuangshen").password("123456").roles("vip2","vip3")
          .and()
          .withUser("root").password("123456").roles("vip1","vip2","vip3")
          .and()
          .withUser("guest").password("123456").roles("vip1","vip2");
}
```

9、测试，我们可以使用这些账号登录进行测试！发现会报错！

There is no PasswordEncoder mapped for the id “null”

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230347555-1367252514.png)

10、原因，我们要将前端传过来的密码进行某种方式加密，否则就无法登录，修改代码

```java
//定义认证规则
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   //在内存中定义，也可以在jdbc中去拿....
   //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
   //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
   //spring security 官方推荐的是使用bcrypt加密方式。
   
   auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
          .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
          .and()
          .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
          .and()
          .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
}
```

11、测试，发现，登录成功，并且每个角色只能访问自己认证下的规则！搞定

### 18.2.4、权限控制和注销

1、开启自动配置的注销的功能

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
   //....
   //开启自动配置的注销的功能
      // /logout 注销请求
   http.logout();
}
```

2、我们在前端，增加一个注销的按钮，index.html 导航栏中

```html
<a class="item" th:href="@{/logout}">
   <i class="sign out icon"></i> 注销
</a>
```

3、我们可以去测试一下，登录成功后点击注销，发现注销完毕会跳转到登录页面！

4、但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？

```java
// .logoutSuccessUrl("/"); 注销成功来到首页
http.logout().logoutSuccessUrl("/");
```

5、测试，注销完毕后，发现跳转到首页OK

6、我们现在又来一个需求：用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！还有就是，比如kuangshen这个用户，它只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！这个就是真实的网站情况了！该如何做呢？

**我们需要结合thymeleaf中的一些功能**

**sec：authorize="isAuthenticated()"**:是否认证登录！来显示不同的页面

Maven依赖：

```xml
<!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity4 -->
<dependency>
   <groupId>org.thymeleaf.extras</groupId>
   <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   <version>3.0.4.RELEASE</version>
</dependency>
```

7、修改我们的 前端页面

1. 导入命名空间

   ```html
   xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5"
   ```

2. 修改导航栏，增加认证判断

   ```html
   <!--登录注销-->
   <div class="right menu">
   
      <!--如果未登录-->
      <div sec:authorize="!isAuthenticated()">
          <a class="item" th:href="@{/login}">
              <i class="address card icon"></i> 登录
          </a>
      </div>
   
      <!--如果已登录-->
      <div sec:authorize="isAuthenticated()">
          <a class="item">
              <i class="address card icon"></i>
             用户名：<span sec:authentication="principal.username"></span>
             角色：<span sec:authentication="principal.authorities"></span>
          </a>
      </div>
   
      <div sec:authorize="isAuthenticated()">
          <a class="item" th:href="@{/logout}">
              <i class="sign out icon"></i> 注销
          </a>
      </div>
   </div>
   ```


8、重启测试，我们可以登录试试看，登录成功后确实，显示了我们想要的页面；

9、如果注销404了，就是因为它默认防止csrf 跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在spring security中关闭csrf功能；我们试试：在 配置中增加 `http.csrf().disable();`

```java
http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
http.logout().logoutSuccessUrl("/");
```

10、我们继续将下面的角色功能块认证完成！

```html
<!-- sec:authorize="hasRole('vip1')" -->
<!-- 用户含有vip1规则就展示 -->
<div class="column" sec:authorize="hasRole('vip1')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 1</h5>
               <hr>
               <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
               <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
               <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
           </div>
       </div>
   </div>
</div>
<!-- 用户含有vip2规则就展示 -->
<div class="column" sec:authorize="hasRole('vip2')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 2</h5>
               <hr>
               <div><a th:href="@{/level2/1}"><i class="bullhorn icon"></i> Level-2-1</a></div>
               <div><a th:href="@{/level2/2}"><i class="bullhorn icon"></i> Level-2-2</a></div>
               <div><a th:href="@{/level2/3}"><i class="bullhorn icon"></i> Level-2-3</a></div>
           </div>
       </div>
   </div>
</div>
<!-- 用户含有vip3规则就展示 -->
<div class="column" sec:authorize="hasRole('vip3')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 3</h5>
               <hr>
               <div><a th:href="@{/level3/1}"><i class="bullhorn icon"></i> Level-3-1</a></div>
               <div><a th:href="@{/level3/2}"><i class="bullhorn icon"></i> Level-3-2</a></div>
               <div><a th:href="@{/level3/3}"><i class="bullhorn icon"></i> Level-3-3</a></div>
           </div>
       </div>
   </div>
</div>
```

11、测试一下！

12、权限控制和注销搞定！

### 18.2.5、记住我

现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况，就是有一个记住密码的功能，这个该如何实现呢？很简单

1、开启记住我功能

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
//。。。。。。。。。。。
   //记住我
   http.rememberMe();
}
```

2、我们再次启动项目测试一下，发现登录页多了一个记住我功能，我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！

思考：如何实现的呢？其实非常简单

我们可以查看浏览器的cookie

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230410090-275194565.png)

3、我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230424964-1382261300.png)
4、结论：登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie，具体的原理我们在JavaWeb阶段都讲过了，这里就不在多说了！

### 18.2.6、定制登录页

现在这个登录页面都是spring security 默认的，怎么样可以使用我们自己写的Login界面呢？

1、在刚才的登录页配置后面指定 loginpage

```java
http.formLogin().loginPage("/toLogin");
```

2、然后前端也需要指向我们自己定义的 login请求

```html
<a class="item" th:href="@{/toLogin}">
   <i class="address card icon"></i> 登录
</a>
```

3、我们登录，需要将这些信息发送到哪里，我们也需要配置，login.html 配置提交请求及方式，方式必须为post:

在 loginPage()源码中的注释上有写明：

![img](https://gitee.com/xudongyin/img/raw/master/img/1905053-20200412230440929-230804638.png)

```html
<form th:action="@{/login}" method="post">
   <div class="field">
       <label>Username</label>
       <div class="ui left icon input">
           <input type="text" placeholder="Username" name="name">
           <i class="user icon"></i>
       </div>
   </div>
   <div class="field">
       <label>Password</label>
       <div class="ui left icon input">
           <input type="password" name="pwd">
           <i class="lock icon"></i>
       </div>
   </div>
   <input type="submit" class="ui blue submit button"/>
</form>
```

4、这个请求提交上来，我们还需要验证处理，怎么做呢？我们可以查看formLogin()方法的源码！我们配置接收登录的用户名和密码的参数！

```java
http.formLogin()
  .usernameParameter("name")//默认参数名是username，要与前端表单提交的属性名一样
  .passwordParameter("pwd")//默认参数名是password，，要与前端表单提交的属性名一样
  .loginPage("/toLogin")
  .loginProcessingUrl("/login"); // 登陆表单提交请求
```

5、在登录页增加记住我的多选框

```html
<input type="checkbox" name="remember"> 记住我
```

6、后端验证处理！

```java
//定制记住我的参数！
http.rememberMe().rememberMeParameter("remember");
```

7、测试，OK

## 18.3、完整配置代码

```java
package com.kuang.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {

       http.authorizeRequests().antMatchers("/").permitAll()
      .antMatchers("/level1/**").hasRole("vip1")
      .antMatchers("/level2/**").hasRole("vip2")
      .antMatchers("/level3/**").hasRole("vip3");


       //开启自动配置的登录功能：如果没有权限，就会跳转到登录页面！
           // /login 请求来到登录页
           // /login?error 重定向到这里表示登录失败
       http.formLogin()
          .usernameParameter("username")
          .passwordParameter("password")
          .loginPage("/toLogin")
          .loginProcessingUrl("/login"); // 登陆表单提交请求

       //开启自动配置的注销的功能
           // /logout 注销请求
           // .logoutSuccessUrl("/"); 注销成功来到首页

       http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
       http.logout().logoutSuccessUrl("/");

       //记住我
       http.rememberMe().rememberMeParameter("remember");
  }

   //定义认证规则
   @Override
   protected void configure(AuthenticationManagerBuilder auth) throws Exception {
       //在内存中定义，也可以在jdbc中去拿....
       //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
       //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
       //spring security 官方推荐的是使用bcrypt加密方式。

       auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
              .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
              .and()
              .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
              .and()
              .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
  }
}
```

去进行一波源码分析！

# 19、SpringBoot：整合Shiro

## 19.1、Shiro介绍

**1. Shiro是什么**

　　Apache Shiro是一个强大灵活的开源安全框架，可以**完全处理**身份验证，授权，企业会话管理和加密。
　　Apache Shiro的首要目标是易于使用和理解。 安全有时可能非常复杂，甚至痛苦，但使用Shiro后就不一定是这样了。 框架应该在可能的情况下掩盖复杂性，并展示一个干净，直观的API，这简化了开发人员工作，并使应用程序更安全。

这里有一些**可以使用Apache Shiro完全的应用场景**：

- **验证用户以及验证其身份。**
- 对用户执行访问控制，例如：在任何环境中使用会话API，即使没有Web或EJB容器。
  - 确定用户是否分配了某个安全角色。
  - 确定用户是否被允许执行操作。
- **在身份验证，访问控制或会话有效期内对事件做出反应。**
- **聚合`1`个或多个用户安全数据的数据源，并将此全部显示为单个复合用户的“视图”。**
- **启用单点登录(SSO)功能。**
- **为用户关联启用“记住我”服务，无需登录。**
  **… 和更多 - 所有都集成到一个内聚易于使用的API。**

　　Shiro尝试为所有应用程序环境实现这些目标 - 从最简单的命令行应用程序到最大的企业应用程序，而不强制依赖于其他第三方框架，容器或应用程序服务器。 当然，该项目(Shiro)旨在尽可能地集成到这个Shiro环境中，从而可以在任何环境中想用即用。

**2. Shiro体系结构**

　　　　　　　　　　　　![img](https://gitee.com/xudongyin/img/raw/master/img/1.png)

**Shiro目标是什么？**
Shiro开发团队称为“**应用程序安全的四个基石**” - 即：**认证**，**授权**，**会话管理**和**加密**：

**Authentication**：身份认证 / 登录，验证用户是不是拥有相应的身份；

**Authorization**：授权，即权限验证，验证某个已认证的用户是否拥有某个权限；即判断用户是否能做事情，常见的如：验证某个用户是否拥有某个角色。或者细粒度的验证某个用户对某个资源是否具有某个权限；

**Session Manager**：会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中；会话可以是普通 JavaSE 环境的，也可以是如 Web 环境的；

**Cryptography**：加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；

**在不同的应用环境中还有其他特性支持和强化这些问题，特别是：**

**Web Support**：Web 支持，可以非常容易的集成到 Web 环境；

**Caching**：缓存，比如用户登录后，其用户信息、拥有的角色 / 权限不必每次去查，这样可以提高效率；

**Concurrency**：并发性，shiro 支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能把权限自动传播过去；

**Testing**：测试，存在测试支持，可帮助您编写单元测试和集成测试，并确保代码按预期得到保障；

**Run As**：运行方式，允许用户承担另一个用户的身份(如果允许)的功能，有时在管理方案中很有用；

**Remember Me**：记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了。

**记住一点，Shiro 不会去维护用户、维护权限；这些需要我们自己去设计 / 提供；然后通过相应的接口注入给 Shiro 即可。**

**3. 内外分析Shiro架构**

   从外部和内部来看看 Shiro 的架构，对于一个好的框架，从外部来看应该具有非常简单易于使用的 API，且 API 契约明确；从内部来看的话，其应该有一个可扩展的架构，即非常容易插入用户自定义实现，因为任何框架都不能满足所有需求。

**3.1 从外部来看 Shiro（从应用程序角度的来观察如何使用 Shiro 完成工作）**

　　　　　　　　　　![image-20200719180731240](https://gitee.com/xudongyin/img/raw/master/img/image-20200719180731240.png)

可以看到：应用代码直接交互的对象是 Subject，也就是说 Shiro 的对外 API 核心就是 Subject。

其每个 API 的含义：

**Subject**：主体，代表了当前 “用户”，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是 Subject，如网络爬虫，机器人等；即一个抽象概念；所有 Subject 都绑定到 SecurityManager，与 Subject 的所有交互都会委托给 SecurityManager；可以把 Subject 认为是一个门面；SecurityManager 才是实际的执行者；

**SecurityManager**：安全管理器；即所有与安全有关的操作都会与 SecurityManager 交互；且它管理着所有 Subject；可以看出它是 Shiro 的核心，它负责与后边介绍的其他组件进行交互，如果学习过 SpringMVC，你可以把它看成 DispatcherServlet 前端控制器；

**Realm**：域，Shiro 从从 Realm 获取安全数据（如用户、角色、权限），就是说 SecurityManager 要验证用户身份，那么它需要从 Realm 获取相应的用户进行比较以确定用户身份是否合法；也需要从 Realm 得到用户相应的角色 / 权限进行验证用户是否能进行操作；可以把 Realm 看成 DataSource，即安全数据源。

也就是说对于我们而言，最简单的一个 Shiro 应用：

1. 应用代码通过 Subject 来进行认证和授权，而 Subject 又委托给 SecurityManager；
2. 我们需要给 Shiro 的 SecurityManager 注入 Realm，从而让 SecurityManager 能得到合法的用户及其权限进行判断。

**从以上也可以看出，Shiro 不提供维护用户 / 权限，而是通过 Realm 让开发人员自己注入。**

**3.2 从内部来看 Shiro 的架构**

<img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200719180916234.png" alt="image-20200719180916234"  />

其每个 API 的含义：

**Subject**：主体，可以看到主体可以是任何可以与应用交互的 “用户”；

**SecurityManager**：相当于 SpringMVC 中的 DispatcherServlet 或者 Struts2 中的 FilterDispatcher；是 Shiro 的心脏；所有具体的交互都通过 SecurityManager 进行控制；它管理着所有 Subject、且负责进行认证和授权、及会话、缓存的管理。

**Authenticator**：认证器，负责主体认证的，这是一个扩展点，如果用户觉得 Shiro 默认的不好，可以自定义实现；其需要认证策略（Authentication Strategy），即什么情况下算用户认证通过了；

**Authorizer**：授权器，或者访问控制器，用来决定主体是否有权限进行相应的操作；即控制着用户能访问应用中的哪些功能；

**Realm**：可以有 1 个或多个 Realm，可以认为是安全实体数据源，即用于获取安全实体的；可以是 JDBC 实现，也可以是 LDAP 实现，或者内存实现等等；由用户提供；注意：Shiro 不知道你的用户 / 权限存储在哪及以何种格式存储；所以我们一般在应用中都需要实现自己的 Realm；

**SessionManager**：如果写过 Servlet 就应该知道 Session 的概念，Session 需要有人去管理它的生命周期，这个组件就是 SessionManager；而 Shiro 并不仅仅可以用在 Web 环境，也可以用在如普通的 JavaSE 环境、EJB 等环境；所以，Shiro 就抽象了一个自己的 Session 来管理主体与应用之间交互的数据；这样的话，比如我们在 Web 环境用，刚开始是一台 Web 服务器；接着又上了台 EJB 服务器；这时想把两台服务器的会话数据放到一个地方，这个时候就可以实现自己的分布式会话（如把数据放到 Memcached 服务器）；

**SessionDAO**：DAO 大家都用过，数据访问对象，用于会话的 CRUD，比如我们想把 Session 保存到数据库，那么可以实现自己的 SessionDAO，通过如 JDBC 写到数据库；比如想把 Session 放到 Memcached 中，可以实现自己的 Memcached SessionDAO；另外 SessionDAO 中可以使用 Cache 进行缓存，以提高性能；

**CacheManager**：缓存控制器，来管理如用户、角色、权限等的缓存的；因为这些数据基本上很少去改变，放到缓存中后可以提高访问的性能

**Cryptography**：密码模块，Shiro 提高了一些常见的加密组件用于如密码加密 / 解密的。

## 19.2、快速入门

**maven项目**目录：

![img](https://gitee.com/xudongyin/img/raw/master/img/1732557-20200525002144029-1646351177.png)

1.shiro快速入门需要的依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
        <version>1.4.1</version>
    </dependency>

    <!-- configure logging -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.7.21</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.21</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
</dependencies>
```

2.shiro.ini文件

```ini
[users]
# user 'root' with password 'secret' and the 'admin' role
root = secret, admin
# user 'guest' with the password 'guest' and the 'guest' role
guest = guest, guest
# user 'presidentskroob' with password '12345' ("That's the same combination on my luggage!!!" ;)), and role 'president'
presidentskroob = 12345, president
# user 'darkhelmet' with password 'ludicrousspeed' and roles 'darklord' and 'schwartz'
darkhelmet = ludicrousspeed, darklord, schwartz
# user 'lonestarr' with password 'vespa' and roles 'goodguy' and 'schwartz'
lonestarr = vespa, goodguy, schwartz

# -----------------------------------------------------------------------------
# Roles with assigned permissions
# 
# Each line conforms to the format defined in the
# org.apache.shiro.realm.text.TextConfigurationRealm#setRoleDefinitions JavaDoc
# -----------------------------------------------------------------------------
[roles]
# 'admin' role has all permissions, indicated by the wildcard '*'
admin = *
# The 'schwartz' role can do anything (*) with any lightsaber:
schwartz = lightsaber:*
# The 'goodguy' role is allowed to 'drive' (action) the winnebago (type) with
# license plate 'eagle5' (instance specific id)
goodguy = winnebago:drive:eagle5
```

**[users]下面包含了 （用户名）=（密码），（角色名）** 

**[roles]下面包含了 角色名、对应的权限**

3.Quickstart.java

```java
public class Quickstart {

    private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);

    public static void main(String[] args) {

        //创建具有配置的领域，用户，角色和权限的Shiro SecurityManager的最简单方法是使用简单的INI配置。
        //我们将通过使用可以提取.ini文件并返回SecurityManager实例的工厂来做到这一点：

        //在类路径的根目录下使用shiro.ini文件 ，使用工厂生成一个securityManager实例，将提前定义好的shiro.ini文件内容加载进去
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        SecurityManager securityManager = factory.getInstance();

        // 对于这个简单的示例快速入门，使SecurityManager作为JVM单例进行访问。大多数应用程序不这样做，而是依靠其容器配置或web.xml来
        // webapps。这超出了此简单快速入门的范围，因此我们将只做最少的工作，以便您可以继续对事物有所了解。
        SecurityUtils.setSecurityManager(securityManager);

        //获取当前用户对象, 1.这里的currentUser对象说白了就是一个用户容器，可以用来载入前端输入的真实用户信息
        Subject currentUser = SecurityUtils.getSubject();

        //通过当前用户获取session
        Session session = currentUser.getSession();
        session.setAttribute("someKey", "aValue");
        String value = (String) session.getAttribute("someKey");
        if (value.equals("aValue")) {
            log.info("Retrieved the correct value! [" + value + "]");
        }

        // 判断当前用户是否被认证， 2.这时currentUser还没有载入用户信息，执行isAuthenticated()实现与之前ini定义好的内容进行检索比较，肯定匹配不成功
        if (!currentUser.isAuthenticated()) {

            //Token:令牌  3.把输入的真实用户信息封装到token对象中
            UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
            token.setRememberMe(true); //记住我

            try {　　　　　　　　　　// 4.把token注入currentUser对象的.login()方法中执行登录操作,这里的.login(XXX)方法会将token中的用户信息　　　　　　　　　　// 和shiro.ini定义的用户信息进行匹配，若匹配成功则登录成功，且currentUser也会成为一个真正意义上的用户对象，而不是一个空的用户容器，　　　　　　　　　　// 若匹配失败，则会catch 以下种类的异常信息
                currentUser.login(token); //执行登录操作

            } catch (UnknownAccountException uae) { //4.1 用户名不存在的异常
                log.info("There is no user with username of " + token.getPrincipal());

            } catch (IncorrectCredentialsException ice) { //4.2 密码不对的异常
                log.info("Password for account " + token.getPrincipal() + " was incorrect!");

            } catch (LockedAccountException lae) { //4.3 用户被锁定的异常
                log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                         "Please contact your administrator to unlock it.");
            }
            // ... catch more exceptions here (maybe custom ones specific to your application?
            catch (AuthenticationException ae) { // 认证异常，上面的异常都是其子类
                //unexpected condition?  error?
            }
        }

        //say who they are:
        //打印其标识主体（在这种情况下为用户名）
        log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");

        //test a role:
        //判断当前用户是否拥有某种角色
        if (currentUser.hasRole("schwartz")) {
            log.info("May the Schwartz be with you!");
        } else {
            log.info("Hello, mere mortal.");
        }

        //测试用户是否具备某一个行为权限，调用subject的isPermitted()方法（不是实例级别）
        //粗粒度
        if (currentUser.isPermitted("lightsaber:wield")) {
            log.info("You may use a lightsaber ring.  Use it wisely.");
        } else {
            log.info("Sorry, lightsaber rings are for schwartz masters only.");
        }

        //（非常强大的）实例级别权限：
        //细粒度
        if (currentUser.isPermitted("winnebago:drive:eagle5")) {
            log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                     "Here are the keys - have fun!");
        } else {
            log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
        }
        //下面三行代码，第二行用于执行用户的退出操作，第一行和第三行用于检验退出操作是否成功
        System.out.println("---->" + currentUser.isAuthenticated());
        // 5.currentUser.logout()方法会清空currentUser中session的所有用户信息
        currentUser.logout();
        System.out.println("---->" + currentUser.isAuthenticated());
        System.exit(0); 
    }
}
```

以上就是最简单的shiro快速入门，其中shiro.ini用来模拟数据库中存储的用户相关信息，代码中标黄放大的注释表达方式可能比较口语化且意思不一定十分准确，但主要还是为了便于理解，希望对你有所帮助。

## 19.3、SpringBoot 整合

目录：

<img src="https://gitee.com/xudongyin/img/raw/master/img/1732557-20200525224837113-1455173795.png" alt="img" style="zoom:67%;" />

 

### 1.该整合项目所需的依赖

```xml
<dependencies>
    <!--
        subject 用户
        securityManager 管理所有用户
        realm  连接数据
        -->

    <!--连接数据库的依赖-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.20</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.23</version>
    </dependency>

    <!--引入mybatis,这是mybatis官方提供的适配springboot的，而不是springboot自己的-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.1.2</version>
    </dependency>

    <!--不想书写setter、getter方法，导入此依赖-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.16.22</version>
    </dependency>

    <!--shiro整合spring的包-->
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-spring</artifactId>
        <version>1.4.2</version>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <!--导入thymeleaf依赖-->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.11.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.thymeleaf.extras</groupId>
        <artifactId>thymeleaf-extras-java8time</artifactId>
        <version>3.0.4.RELEASE</version>
    </dependency>

    <!--shiro-thymeleaf整合-->
    <dependency>
        <groupId>com.github.theborakompanioni</groupId>
        <artifactId>thymeleaf-extras-shiro</artifactId>
        <version>2.0.0</version>
    </dependency>
</dependencies>

<repositories>
    <repository>
        <id>aliyun-repos</id>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
<pluginRepositories>
    <pluginRepository>
        <id>aliyun-plugin</id>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </pluginRepository>
</pluginRepositories>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>

</project>
```

### 2.application.yml设置连接数据库的相关配置

```yml
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

### 3.index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml?"
      xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro" >
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>首页</h1>

    <div th:if="${session.loginUser==null}">
        <a th:href="@{/toLogin}">登录</a>
    </div>

    <p th:text="${msg}"></p>

    <hr>
    <div shiro:hasPermission="user:add">
        <a th:href="@{/user/add}">add</a>
    </div>
    <div shiro:hasPermission="user:update">
        <a th:href="@{/user/update}">update</a>
    </div>

    <a th:href="@{/logout}">注销</a>

</body>
</html>
```

### 4.add.html和update.html

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <h1>add</h1>
    </body>
</html>

-------------------------------------------------

<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <h1>update</h1>
    </body>
</html>
```

### 5.login.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml?">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <h1>登录</h1>
        <hr>
        <p th:text="${msg}" style="color:red;"></p>
        <form th:action="@{/login}">
            用户名：<input type="text" name="username"><br>
            密码：<input type="password" name="password"><br>
            
            <input type="submit" name="提交">
        </form>
    </body>
</html>
```

### 6.MyController.java

```java
@Controller
public class MyController {

    @RequestMapping({"/","/index"})
    public String toIndex(Model model){
        model.addAttribute("msg","hello,shiro!");
        return "index";
    }

    @RequestMapping("/user/add")
    public String add(){
        return "user/add";
    }

    @RequestMapping("/user/update")
    public String update(){
        return "user/update";
    }

    @RequestMapping("/toLogin")
    public String toLogin(){
        return "login";
    }

    @RequestMapping("/login")
    public String login(String username,String password,Model model){
        //获取当前用户
        Subject subject = SecurityUtils.getSubject();
        //封装用户的登录数据
        UsernamePasswordToken token = new UsernamePasswordToken(username, password);

        try{
            subject.login(token); //执行登录的方法，如果没有异常就说明ok了
            return "index";
        }catch (UnknownAccountException e){ //用户名不存在
            model.addAttribute("msg","用户名不存在！");
            return "login";
        }catch (IncorrectCredentialsException e){
            model.addAttribute("msg","密码错误！");
            return "login";
        }
    }

    @RequestMapping("/noauth")
    @ResponseBody
    public String unauthorized(){
        return "未授权无法访问此页面";
    }

    @RequestMapping("/logout")
    public String logout(){
        //获取当前用户
        Subject subject = SecurityUtils.getSubject();
        System.out.println(subject.getSession().getAttribute("loginUser"));
        subject.logout(); // session 会销毁，在SessionListener监听session销毁，清理权限缓存
        System.out.println(subject.getSession().getAttribute("loginUser"));
        System.out.println("执行了退出");
        return "login";
    }
}
```

### 7.ShiroConfig.java

```java
@Configuration
public class ShiroConfig {

    //ShiroFilterFactoryBean （第三步:连接到前端）
    @Bean
    public ShiroFilterFactoryBean getShiroFilterBean(@Qualifier("securityManager") DefaultWebSecurityManager defaultWebSecurityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        //设置安全管理器
        bean.setSecurityManager(defaultWebSecurityManager);

        //添加shiro的内置过滤器
        /*
        anon: 无需认证即可访问
        authc: 必须认证才能用
        user: 必须拥有 “记住我” 功能才能用
        perms: 拥有对某个资源的权限才能用
        role: 拥有某个角色权限才能访问
        */

        Map<String,String> filterMap = new LinkedHashMap<>();

        //授权，正常情况下没有授权会跳转到未授权页面
        filterMap.put("/user/add","perms[user:add]");
        filterMap.put("/user/update","perms[user:update]");
        
        //拦截
        filterMap.put("/user/add","authc");
        filterMap.put("/user/update","authc");
        //也可使用通配符*
        //filterMap.put("/user/*","authc"); 这样写就必须得在权限验证链接下面，不然无法正常进行权限验证授权

        bean.setFilterChainDefinitionMap(filterMap);

        //若访问时用户未认证，则跳转至登录页面
        bean.setLoginUrl("/toLogin");
        //若访问时用户未被授权，则跳转至未授权页面
        bean.setUnauthorizedUrl("/noauth");

        return bean;
    }

    //DefaultWebSecurityManager （第二步：管理realm对象）
    @Bean(name="securityManager") //@Bean注解后便被spring托管,不加name属性，默认name值为方法名,这里就加一下吧
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        //关联UserRealm
        securityManager.setRealm(userRealm);
        return securityManager;

    }

    //创建realm对象，需要自定义类 (第一步:创建realm对象)
    @Bean(name="userRealm")  //@Bean注解后便被spring托管,不加name属性，默认name值为方法名,这里就加一下吧
    public UserRealm userRealm(){
        return new UserRealm();
    }

    //整合ShiroDialect:用来整合shiro thymeleaf
    @Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }
}
```

注意：**`filterMap.put("/user/*","authc");`必须写在 `filterMap.put("/user/add","perms[user:add]");`下面**

### 8.UserRealm.java

```java
//自定义UserRealm extends AuthorizingRealm
public class UserRealm extends AuthorizingRealm {

    @Autowired
    UserService userService;

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        System.out.println("执行了授权");

        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
        //info.addStringPermission("user:add");

        //拿到当前登录的对象
        Subject subject = SecurityUtils.getSubject();
        User currentUser = (User) subject.getPrincipal(); //拿到user对象

        info.addStringPermission(currentUser.getPerms());

        return info;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("执行了认证");
        //令牌
        UsernamePasswordToken userToken = (UsernamePasswordToken) token;

        /*
        //用户名、密码  模拟从数据库中获取
        String name = "root";
        String password = "1111";

        if (!userToken.getUsername().equals(name)){
            return null; //抛出异常 UnknownAccountException
        }

        //密码认证，shiro做~
        return new SimpleAuthenticationInfo("",password,"");
        */

        //连接真实数据库
        User user = userService.queryUserByName(userToken.getUsername());
        if (user==null){ //没有这个人
            return null; //抛出异常 UnknownAccountException
        }
        Subject currentSubject = SecurityUtils.getSubject();
        Session session = currentSubject.getSession();
        session.setAttribute("loginUser",user);

        //可以加密： MD5加密   MD5盐值加密
        //密码认证，shiro做~
        return new SimpleAuthenticationInfo(user,user.getPwd(),"");

    }
}
```

###  9.UserMapper.java

```java
@Repository
@Mapper
public interface UserMapper {

    public User queryUserByName(String name);

}
```

### 10.UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ztx.mapper.UserMapper">
    <select id="queryUserByName" parameterType="String" resultType="User">
        select * from mybatis.user where name=#{name}
    </select>
</mapper>
```

### 11.User.java

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {

    private int id;
    private String name;
    private String pwd;
    private String perms;
}
```

### 12.UserService.java和UserServiceImpl.java

```java
public interface UserService {

    public User queryUserByName(String name);

}

-----------------------------------------------------

@Service
public class UserServiceImpl implements UserService{

    @Autowired
    UserMapper userMapper;

    @Override
    public User queryUserByName(String name) {
        return userMapper.queryUserByName(name);
    }
}
```

###  13.数据库中的user表

**（perms前缀user：并无作用，可以去掉，修饰作用而已，相应位置使用到也要跟着去掉前缀，如果要多权限，只要在数据库perms列逗号再加加上权限参数就行）**

![img](https://gitee.com/xudongyin/img/raw/master/img/1732557-20200525232923150-1456279780.png)

![image-20200714154919593](https://gitee.com/xudongyin/img/raw/master/img/image-20200714154919593.png)

 

个人对该项目执行流程进行了梳理，如有错误，请指正：

　　1.ShiroConfig.java中设置好拦截器，规定哪些页面需要用户具备何种要求才可访问，同时还设置当用户不满足要求时应该跳转至什么页面。对应本项目的代码：

　　![img](https://gitee.com/xudongyin/img/raw/master/img/1732557-20200528102317787-186241790.png)

　　2.用户在未认证下，会跳转到登陆页面login.html。输入用户名密码，提交执行controller中的login()方法，该方法内将用户信息封装成token对象，传入subject.login()方法内进行登录验证。对应项目代码：

　　![img](https://gitee.com/xudongyin/img/raw/master/img/1732557-20200528102447393-2136646425.png) 

　　3.执行登录验证时会跳转到UserRealm类，首先执行认证(doGetAuthenticationInfo)：连接数据库判断是否有此人，若没有则返回结果null，若有，则将数据库中的用户信息存储在subject的session中。接着进行密码验证，若验证失败，则将登录不通过返回失败认证信息，反之，返回成功的认证信息，并执行用户授权(doGetAuthorizationInfo)操作，返回授权信息。对应项目代码：

　　认证：

 　![img](https://gitee.com/xudongyin/img/raw/master/img/1732557-20200528102519608-999242032.png)

　　授权：

　　 ![img](https://gitee.com/xudongyin/img/raw/master/img/1732557-20200528102540179-466571560.png)

　　4.本项目中登录成功后跳转到index.html时依旧会再次执行授权操作，因为前端页面需要一个判断展示功能，代码如下：

　　 ![img](https://gitee.com/xudongyin/img/raw/master/img/1732557-20200528102558235-1957418305.png)

　　故控制台信息会是这样：（出现两次授权）

　　 ![img](https://gitee.com/xudongyin/img/raw/master/img/1732557-20200528102613001-1241215544.png)

　　5.之后进入add.html或update.html时，因为会被拦截，所以都要进行授权验证，而此时认证将不会在执行。

# 20、SpringBoot：整合 Redis

SpringBoot 操作数据：spring-data jpa jdbc mongodb redis！

SpringData 也是和 SpringBoot 齐名的项目！

说明： 在 SpringBoot2.x 之后，原来使用的jedis 被替换为了 lettuce?

jedis : 采用的直连，多个线程操作的话，是不安全的，如果想要避免不安全的，使用 jedis pool 连接
 池！ 更像 BIO 模式

lettuce : 采用netty，实例可以在多个线程中进行共享，不存在线程不安全的情况！可以减少线程数据
 了，更像 NIO 模式

源码分析：

```java
@Bean
// 我们可以自己定义一个redisTemplate来替换这个默认的！
@ConditionalOnMissingBean(name = "redisTemplate") 
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory     													isConnectionFactory)
    throws UnknownHostException {
    // 默认的 RedisTemplate 没有过多的设置，redis 对象都是需要序列化！
    // 两个泛型都是 Object, Object 的类型，我们后使用需要强制转换 <String, Object>
    RedisTemplate<Object, Object> template = new RedisTemplate<>();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}
@Bean
@ConditionalOnMissingBean 
// 由于 String 是redis中最常使用的类型，所以说单独提出来了一个bean！
public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory
                                               redisConnectionFactory)
    throws UnknownHostException {
    StringRedisTemplate template = new StringRedisTemplate();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}
```

整合测试一下

1 、导入依赖

~~~xml
<!-- 操作redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

2 、配置连接

~~~properties
# 配置redis
spring.redis.host=127.0.0.1
spring.redis.port= 6379
~~~

3 、测试！

```java
@SpringBootTest
class Redis02SpringbootApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;
    @Test
    void contextLoads() {
        // redisTemplate 操作不同的数据类型，api和我们的指令是一样的
        // opsForValue 操作字符串 类似String ，位图在这个里面
        // opsForList 操作List 类似List
        // opsForSet
        // opsForHash
        // opsForZSet
        // opsForGeo
        // opsForHyperLogLog
        // 除了进本的操作，我们常用的方法都可以直接通过redisTemplate操作，比如事务，和基本的CRUD
        // 获取redis的连接对象
        // RedisConnection connection =  redisTemplate.getConnectionFactory().getConnection();
        // connection.flushDb();
        // connection.flushAll();
        redisTemplate.opsForValue().set("mykey","关注狂神说公众号");
        System.out.println(redisTemplate.opsForValue().get("mykey"));
    }
}
```

![image-20200727094055958](https://gitee.com/xudongyin/img/raw/master/img/20200727094058.png)

![image-20200727094147381](https://gitee.com/xudongyin/img/raw/master/img/20200727094148.png)

关于对象的保存：

![image-20200727095307582](https://gitee.com/xudongyin/img/raw/master/img/20200727095358.png)

我们来编写一个自己的 RedisTemplete

```java
package com.kuang.config;
import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;
@Configuration
public class RedisConfig {
    // 自己定义了一个 RedisTemplate
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory
                                                       factory) {
        // 固定模板，在企业中，拿去就可以直接使用！
        // 我们为了自己开发方便，一般直接使用 <String, Object>
        RedisTemplate<String, Object> template = new RedisTemplate<String,
        Object>();
        template.setConnectionFactory(factory);
        
        // Json序列化配置
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new
            Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // String 的序列化
        StringRedisSerializer stringRedisSerializer = new
            StringRedisSerializer();
        
        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();
        return template;
    }
```

所有的redis操作，其实对于java开发人员来说，十分的简单，更重要是要去理解redis的思想和每一种数据结构的用处和作用场景！

# 21、总结

~~~java
## 聊聊现在和未来

三层架构 + MVC
	架构   --->   解耦

开发框架
		Spring
			IOC		AOP
​            IOC ：控制反转
​                  约泡：
​					泡温泉，泡茶.....泡友
​					附近的人，打招呼。加微信，聊天，天天聊，--->约泡

​				 浴场(容器) :温泉， 茶庄，泡友
​					直接进温泉，就有人和你一起了!

​			原来我们都是自己一步步操作，现在交给容器了!我们需要什么就去拿就可以了
​			 AOP :切面(本质，动态代理)
​			      为了解决什么?不影响业务本来的情况下，实现动态增加功能，大量应用在日志，事务...等等方面

​				Spring是一个轻量级的Java开源框架，容器
​				目的:解决企业开发的复杂性问题
​				Spring是春天，觉得他是春天，也十分复杂，配置文件!

​		SpringBoot
​				SpringBoot并不是新东西，就是Spri ng的升级版!
 				新一代JavaEE的开发标准，开箱即用! ->拿过来就可以用! 它自动帮我们配置了非常多的东西，我们拿来即用!
​				特性:约定大于配置!

随着公司体系越来越大，用户越来越多!

微服务架构--->新架构
​		模块化，功能化!
​		用户，支付，签到，娱乐，..... ;
​		人多余多:一台服务器解决不了;在增加服务器 !  横向
​		假设A服务器占用98%资源，B服务器只占用了10% 。一-负载均衡;

​		将原来的整体项目，分成模块化，用户就是一个单独的项目，签到也是一个单独的项目， 项目和项目之前需要通信，如何通信?

​		用户非常多，而签到十分少!	给用户多一点服务器，给签到少一点服务器!

微服务架构问题?

  分布式架构会遇到的四个核心问题?

1. 这么多服务，客户端该如何去访问?
2. 这么多服务，服务之间如何进行通信?
3. 这么多服务，如何治理呢?
4. 服务挂了，怎么办?

解决方案:
​	SpringCloud,是一套生态，就是来解决以上分布式架构的4个问题

​	想使用SpringCloud,必须要掌握SpringBoot,因为SpringCloud是基 于SpringBoot;


     	1. Spring Cloud NetFlix ，出来了一套解决方案! - -站式解决方案。我们都可以直接去这里拿?
   ​	Api网关，zuu1组件
   ​	Feign --> HttpClient ---> HTTP的通信方式，同步并阻塞
   ​	服务注册与发现，Eureka
   ​	熔断机制，Hystrix
   ​	
   ​	2018年年底，NetFlix宣布无限期停止维护。生态不再维护，就会脱节。

  
	    2. Apache Dubbo zookeeper，第二套解决系统

   ​	API ：	没有! 	要么找第三方组件，要么自己实现
   ​	Dubbo是一个高性能的基于Java实现的RPC通信框架！ 	2.6.x
   ​	服务注册与发现，zookeeper: 动物园管理者(Hadoop ，Hive)
   ​	没有：	借助了Hystrix
   ​	
   ​	不完善，Dubbo .
   

	    3. SpringCloud Alibaba-站式解决方案 !


目前，又提出了一种方案:
​			服务网格：下一代微服务标准，Service Mesh
​			代表解决方案: istio (你们未来可能需要掌握! )


万变不离其宗，一通百通!

	1. API网关，服务路由
 	2. HTTP，RPC框架，异步调用
 	3. 服务注册与发现，高可用
 	4. 熔断机制，服务降级

为什么要解决这个问题？ 本质：网络是不可靠的！
~~~

