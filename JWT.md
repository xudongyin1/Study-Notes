## 1.什么是JWT？

> JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the **HMAC** algorithm) or a public/private key pair using **RSA** or **ECDSA**.
>
> Although JWTs can be encrypted to also provide secrecy between parties, we will focus on *signed* tokens. Signed tokens can verify the *integrity* of the claims contained within it, while encrypted tokens *hide* those claims from other parties. When tokens are signed using public/private key pairs, the signature also certifies that only the party holding the private key is the one that signed it.--摘自官网

```
JSON Web令牌（JWT）是一个开放标准（RFC 7519），它定义了一种紧凑而独立的方法，用于在各方之间安全地将信息作为JSON对象传输。由于此信息是经过数字签名的，因此可以被验证和信任。可以使用秘密（使用HMAC算法）或使用RSA或ECDSA的公用/专用密钥对对JWT进行签名。
尽管可以对JWT进行加密以提供双方之间的保密性，但我们将重点关注已签名的令牌。签名的令牌可以验证其中包含的声明的完整性，而加密的令牌则将这些声明隐藏在其他方的面前。当使用公钥/私钥对对令牌进行签名时，签名还证明只有持有私钥的一方才是对其进行签名的一方。
```

## 2.JWT能做什么？

```
# 1.授权
-这是使用JWT的最常见方案。一旦用户登录，每个后续请求将包括JWT,从而允许用户访问该令牌允许的路由，服务和资源。单点登录是今广泛使用JWT的一项功能。因为它的开销很小并且可以在不同的域中轻松使用

# 2.信息交换
JSON Web Token是在各方之间安全地传输信息的好方法。因为可以对JWT进行签名（例如，使用公钥/私钥对），所以您可以确保发件人是他们所说的人。此外，由于签名是使用标头和有效负载计算的，因此您还可以验证内容是否遭到篡改。
```

## 3.传统Session认证

**传统的Session认证：**

```
我们知道，http协议本身是一种无状态的协议，而这就意味着如果用户向我们的应用提供了用户名和密码来进行用户认证，那么下一次请求时，用户还要再一次进行用户认证才行，因为根据http协议，我们并不能知道是哪个用户发出的请求，所以为了让我们的应用能识别是哪个用户发出的请求，我们只能在服务器存储一份用户登录的信息，这份登录信息会在响应时传递给浏览器，告诉其保存为cookie,以便下次请求时发送给我们的应用，这样我们的应用就能识别请求来自哪个用户了,这就是传统的基于session认证。

但是这种基于session的认证使应用本身很难得到扩展，随着不同客户端用户的增加，独立的服务器已无法承载更多的用户，而这时候基于session认证应用的问题就会暴露出来.
```

**基于Session认证所显露的问题:**

```
Session: 每个用户经过我们的应用认证之后，我们的应用都要在服务端做一次记录，以方便用户下次请求的鉴别，通常而言session都是保存在内存中，而随着认证用户的增多，服务端的开销会明显增大。

扩展性: 用户认证之后，服务端做认证记录，如果认证的记录被保存在内存中的话，这意味着用户下次请求还必须要请求在这台服务器上,这样才能拿到授权的资源，这样在分布式的应用上，相应的限制了负载均衡器的能力。这也意味着限制了应用的扩展能力。

CSRF: 因为是基于cookie来进行用户识别的, cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。
```

## 4.JWT的认证流程

```
# 1.认证流程
- 首先，前端通过Web表单将自己的用户名和密码发送到后端的接口。这一过程一般是一个HTTP POST请求。建议的方式是通过SSL加密的传输（https协议），从而避免敏感信息被嗅探。
- 后端核对用户名和密码成功后，将用户的id等其他信息作为JWT Payload（负载），将其与头部分别进行Base64编码拼接后签名，形成一个JWT(Token)。形成的JWT就是一个形同lll.zzz.xxx的字符串。 token head.payload.singurater

- 后端将JWT字符串作为登录成功的返回结果返回给前端。前端可以将返回的结果保存在localStorage或sessionStorage上，退出登录时前端删除保存的JWT即可。

- 前端在每次请求时将JWT放入HTTP Header中的Authorization位。(解决XSS和XSRF问题) HEADER

- 后端检查是否存在，如存在验证JWT的有效性。例如，检查签名是否正确；检查Token是否过期；检查Token的接收方是否是自己（可选）。
- 验证通过后后端使用JWT中包含的用户信息进行其他逻辑操作，返回相应结果。

# 2.jwt优势

- 简洁(Compact): 可以通过URL，POST参数或者在HTTP header发送，因为数据量小，传输速度也很快

- 自包含(Self-contained)：负载中包含了所有用户所需要的信息，避免了多次查询数据库

- 因为Token是以JSON加密的形式保存在客户端的，所以JWT是跨语言的，原则上任何web形式都支持。

- 不需要在服务端保存会话信息，特别适用于分布式微服务。
```

## 5.JWT的结构

```java
token  String --------> header.payload.signature

# 1.令牌组成
- 1.标头(Header)
- 2.有效载荷(Payload)
- 3.签名(Signature)
# 2.Header
- 标头通常由两部分组成：令牌的类型(即JWT)和所使用的签名算法，例如HMAC SHA256(默认)或RSA。它会使用Base64编码组成JWT结构的第一部分。

- 注意：Base64是一种编码，也就是说，它是可以被翻译回原来的样子的，它并不是一种加密过程。

{
  "alg": "HS256",	//加密算法
  "typ": "JWT"		//类型
}

# 3.Payload
- 令牌的第二部分是有效负载，其中包含声明。声明是有关实体（通常是用户）和其他数据的声明。同样的，它会使用 Base64 编码组成 JWT 结构的第二部分
- 标准中注册的声明(建议但是不强制使用)
	* iss: jwt签发者
	* sub: jwt所面向的用户
	* aud: 接收jwt的一方
	* exp: jwt的过期时间，这个过期时间必须要大于签发时间
	* nbf: 定义在什么时间之前，该jwt都是不可用的.
	* iat: jwt的签发时间 
	* jti: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。
	
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true		
}

# 4.Signature
- 前面两部分都是使用 Base64 进行编码的，即前端可以解开知道里面的信息。Signature 需要使用编码后的 header 和 payload 以及我们提供的一个密钥，然后使用 header 中指定的签名算法（HS256）进行签名。签名的作用是保证 JWT 没有被篡改过
- 如:
	HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload),secret);

# 签名目的
- 最后一步签名的过程，实际上是对头部以及负载内容进行签名，防止内容被窜改。如果有人对头部以及负载的内容解码之后进行修改，再进行编码，最后加上之前的签名组合形成新的JWT的话，那么服务器端会判断出新的头部和负载形成的签名和JWT附带上的签名是不一样的。如果要对新的头部和负载进行签名，在不知道服务器加密时用的密钥的话，得出来的签名也是不一样的。

# 信息安全问题
- 在这里大家一定会问一个问题：Base64是一种编码，是可逆的，那么我的信息不就被暴露了吗？

- 是的。所以，在JWT中，不应该在负载里面加入任何敏感的数据。在上面的例子中，我们传输的是用户的User ID。这个值实际上不是什么敏	感内容，一般情况下被知道也是安全的。但是像密码这样的内容就不能被放在JWT中了。如果将用户的密码放在了JWT中，那么怀有恶意的第三方通过Base64解码就能很快地知道你的密码了。因此JWT适合用于向Web应用传递一些非敏感信息。JWT还经常用于设计用户认证和授权系统，甚至实现Web应用的单点登录。
```

![image-20201020094714641](https://gitee.com/xudongyin/img/raw/master/img/20201020094716.png)

```
# 5.放在一起
- 输出是三个由点分隔的Base64-URL字符串，可以在HTML和HTTP环境中轻松传递这些字符串，与基于XML的标准（例如SAML）相比，它更紧凑。
- 简洁(Compact)
	可以通过URL, POST 参数或者在 HTTP header 发送，因为数据量小，传输速度快
- 自包含(Self-contained)
	负载中包含了所有用户所需要的信息，避免了多次查询数据库
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020092800.png)

## 6.使用JWT

### 6.1引入依赖

```xml
<!--引入jwt-->
<dependency>
  <groupId>com.auth0</groupId>
  <artifactId>java-jwt</artifactId>
  <version>3.4.0</version>
</dependency>
```

### 6.2生成token

```java
@Test
void contextLoads() {
    Calendar calendar = Calendar.getInstance();
    calendar.add(Calendar.SECOND,60);
    String token = JWT.create()
        .withClaim("username", "gtt")    //设置自定义用户名
        .withExpiresAt(calendar.getTime())    //设置过期时间
        .sign(Algorithm.HMAC256("gttt!@#$%^&*"));//设置签名 gttt!@#$%^&*为签名的第三部分secret
    System.out.println(token);
}
```

![image-20200922191350985](https://gitee.com/xudongyin/img/raw/master/img/20201020092808.png)

### 6.3根据令牌和签名解析数据

```java
@Test
void  test(){
    JWTVerifier verifier = JWT.require(Algorithm.HMAC256("gttt!@#$%^&*")).build();
    DecodedJWT decodedJWT = 			             verifier.verify("eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2MDA3NzMyNDgsInVzZXJuYW1lIjoiZ3R0In0.p8WJrU56oCaxOS21w-z4gzQUgo1u8I52oaHuub9M5HE");
    System.out.println("用户名："+decodedJWT.getClaim("username"));
    System.out.println("过期时间："+decodedJWT.getExpiresAt());
}
```

![image-20200922192035754](https://gitee.com/xudongyin/img/raw/master/img/20201020092823.png)

这是因为我们**签名失效设置的时间到了**，所以我们需要重新生成token并验证。

![image-20200922192345932](https://gitee.com/xudongyin/img/raw/master/img/20201020092830.png)

## 7.工具类jwt

```java
public class JWTUtils {
    private static String TOKEN = "gttt@W3e4r";
    /**
     * 生成token
     * @param map  //传入payload
     * @return 返回token
     */
    public static String getToken(Map<String,String> map){
        JWTCreator.Builder builder = JWT.create();
        map.forEach((k,v)->{
            builder.withClaim(k,v);
        });
        Calendar instance = Calendar.getInstance();
        instance.add(Calendar.HOUR,7*24);//设置过期时间
        builder.withExpiresAt(instance.getTime());
        return builder.sign(Algorithm.HMAC256(TOKEN)).toString();
    }
    /**
     * 验证token
     * @param token
     * @return
     */
    public static void verify(String token){
        JWT.require(Algorithm.HMAC256(TOKEN)).build().verify(token);
    }
    /**
     * 获取token中payload
     * @param token
     * @return
     */
    public static DecodedJWT getToken(String token){
        return JWT.require(Algorithm.HMAC256(TOKEN)).build().verify(token);
    }
}
```

## 8.整合SpringBoot

### 8.1导入依赖

```xml
        <!--引入jwt-->
        <dependency>
            <groupId>com.auth0</groupId>
            <artifactId>java-jwt</artifactId>
            <version>3.4.0</version>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
```

### 8.2 SQL语句

```sql
CREATE DATABASE IF NOT EXISTS jwt;
USE jwt;

CREATE TABLE USER(
	id INT PRIMARY KEY AUTO_INCREMENT,
	username VARCHAR(255) NOT NULL,
	PASSWORD VARCHAR(255) NOT NULL
)ENGINE=INNODB;
INSERT INTO USER VALUES(NULL,'gya','12345');
```

### 8.3编写application.properties配置文件

```properties
# 数据源设置
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.url=jdbc:mysql://localhost:3306/jwt?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/shanghai
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# mybatis设置,因为当时Mybatis官网挂了，所以用的MP，不过都一样
mybatis-plus.mapper-locations=classpath*:mapper/*.xml
mybatis-plus.type-aliases-package=com.gt.springbootjwt.entity

# 日志设置
logging.level.com.gt.springbootjwt.dao=debug
```

### 8.4dao层

```java
@Mapper
public interface UserDao {
    User login(User user);
}



<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gt.springbootjwt.dao.UserDao">
    <select id="login" resultType="com.gt.springbootjwt.entity.User" parameterType="com.gt.springbootjwt.entity.User">
        select id,username,password from user where username=#{username} and password=#{password}
    </select>
</mapper>
```

### 8.5service层

```java
public interface UserService {
    User login(User user);
}



@Service
public class UserServiceImpl implements UserService {
    @Autowired
    UserDao userDao;
    @Transactional(propagation = Propagation.SUPPORTS)
    @Override
    public User login(User user) {
        User loginUser = userDao.login(user);
        if(loginUser!=null){
            return loginUser;
        }
        throw new RuntimeException("用户不存在");
    }
}
```

### 8.6编写Controller

```java
@RestController
@Slf4j
public class UserController {
    @Autowired
    private UserService userService;
    @GetMapping("/user/login")
    public Map<String,Object> login(User user){
        Map<String,Object> result = new HashMap<>();
        log.info("用户名:[{}]",user.getUsername());
        log.info("密码:[{}]",user.getPassword());

        try {
            //登录
            User loginUser = userService.login(user);
            //判断login是否为空
            //添加payload载荷
            Map<String, String> map = new HashMap<>();
            map.put("username",loginUser.getUsername());
            map.put("password",loginUser.getPassword());
            //生成token
            String token = JWTUtils.getToken(map);
            //向前端返回code状态码msg信息和token
            result.put("code",200);
            result.put("msg","登陆成功!");
            result.put("token",token);
        } catch (Exception e) {
            e.printStackTrace();
            result.put("code",502);
            result.put("msg",e.getMessage());
        }
        return result;
    }
}
```

### 8.7测试

使用Postman软件进行测试，但输入错误的用户名和密码时，没有获取到token。

![image-20200922205801978](https://gitee.com/xudongyin/img/raw/master/img/20201020093009.png)

输入正确的用户名和密码后，生成token并返回。

![image-20200922205738392](https://gitee.com/xudongyin/img/raw/master/img/20201020093014.png)

### 8.8编写测试接口并测试

```java
 @PostMapping("/user/test")
    public Map<String, Object> test(String token) {
        Map<String, Object> map = new HashMap<>();
        try {
            JWTUtils.verify(token);
            map.put("msg", "验证通过~~~");
            map.put("state", true);
        } catch (TokenExpiredException e) {
            map.put("state", false);
            map.put("msg", "Token已经过期!!!");
        } catch (SignatureVerificationException e){
            map.put("state", false);
            map.put("msg", "签名错误!!!");
        } catch (AlgorithmMismatchException e){
            map.put("state", false);
            map.put("msg", "加密算法不匹配!!!");
        } catch (Exception e) {
            e.printStackTrace();
            map.put("state", false);
            map.put("msg", "无效token~~");
        }
        return map;
    }
```

我们不携带token去测试

![image-20200922210508589](https://gitee.com/xudongyin/img/raw/master/img/20201020093032.png)

我们携带token去测试，测试通过。

![image-20200922210822507](https://gitee.com/xudongyin/img/raw/master/img/20201020093036.png)

### 8.9问题

- 使用上述方式每次都要传递token数据,每个方法都需要验证token代码冗余,不够灵活? 如何优化
- 使用拦截器进行优化

```java
//拦截器
@Component
public class JWTInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String token = request.getHeader("token");
        Map<String,Object> map = new HashMap<>();
        try {
            JWTUtils.verify(token);
            return true;
        } catch (TokenExpiredException e) {
            map.put("state", false);
            map.put("msg", "Token已经过期!!!");
        } catch (SignatureVerificationException e){
            map.put("state", false);
            map.put("msg", "签名错误!!!");
        } catch (AlgorithmMismatchException e){
            map.put("state", false);
            map.put("msg", "加密算法不匹配!!!");
        } catch (Exception e) {
            e.printStackTrace();
            map.put("state", false);
            map.put("msg", "无效token~~");
        }
        //用自带的jackson转化为json数据
        String json = new ObjectMapper().writeValueAsString(map);
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().println(json);
        return false;
    }
}



@Configuration
public class InterceptorConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new JWTInterceptor())
                .addPathPatterns("/**") //添加拦截路径
                .excludePathPatterns("/user/login");//跳过哪个路径，以实际项目为准
    }
}
```