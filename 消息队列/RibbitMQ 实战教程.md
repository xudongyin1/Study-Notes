# RabbitMQ 实战教程

## 1.MQ引言

### 1.1 什么是MQ

`MQ`(Message Quene) :  翻译为 `消息队列`,通过典型的 `生产者`和`消费者`模型,生产者不断向消息队列中生产消息，消费者不断的从队列中获取消息。因为消息的生产和消费都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入,轻松的实现系统间解耦。别名为 `消息中间件`	通过利用高效可靠的消息传递机制进行平台无关的数据交流，并基于数据通信来进行分布式系统的集成。

### 1.2 MQ有哪些

当今市面上有很多主流的消息中间件，如老牌的`ActiveMQ`、`RabbitMQ`，炙手可热的`Kafka`，阿里巴巴自主开发`RocketMQ`等。

### 1.3 不同MQ特点

```markdown
# 1.ActiveMQ
		ActiveMQ 是Apache出品，最流行的，能力强劲的开源消息总线。它是一个完全支持JMS规范的的消息中间件。丰富的API,多种集群架构模式让ActiveMQ在业界成为老牌的消息中间件,在中小型企业颇受欢迎!

# 2.Kafka
		Kafka是LinkedIn开源的分布式发布-订阅消息系统，目前归属于Apache顶级项目。Kafka主要特点是基于Pull的模式来处理消息消费，追求高吞吐量，一开始的目的就是用于日志收集和传输。0.8版本开始支持复制，不支持事务，对消息的重复、丢失、错误没有严格要求，适合产生大量数据的互联网服务的数据收集业务。

# 3.RocketMQ
		RocketMQ是阿里开源的消息中间件，它是纯Java开发，具有高吞吐量、高可用性、适合大规模分布式系统应用的特点。RocketMQ思路起源于Kafka，但并不是Kafka的一个Copy，它对消息的可靠传输及事务性做了优化，目前在阿里集团被广泛应用于交易、充值、流计算、消息推送、日志流式处理、binglog分发等场景。

# 4.RabbitMQ
		RabbitMQ是使用Erlang语言开发的开源消息队列系统，基于AMQP协议来实现。AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。AMQP协议更多用在企业系统内对数据一致性、稳定性和可靠性要求很高的场景，对性能和吞吐量的要求还在其次。
		
```

> RabbitMQ比Kafka可靠，Kafka更适合IO高吞吐的处理，一般应用在大数据日志处理或对实时性（少量延迟），可靠性（少量丢数据）要求稍低的场景使用，比如ELK日志收集。

---

## 2.RabbitMQ 的引言

### 2.1 RabbitMQ 

> 基于`AMQP`协议，erlang语言开发，是部署最广泛的开源消息中间件,是最受欢迎的开源消息中间件之一。

`官网`: https://www.rabbitmq.com/

`官方教程`: https://www.rabbitmq.com/#getstarted

```markdown
 # AMQP 协议
 		AMQP（advanced message queuing protocol）`在2003年时被提出，最早用于解决金融领域不同平台之间的消息传递交互问题。顾名思义，AMQP是一种协议，更准确的说是一种binary wire-level protocol（链接协议）。这是其和JMS的本质差别，AMQP不从API层进行限定，而是直接定义网络交换的数据格式。这使得实现了AMQP的provider天然性就是跨平台的。以下是AMQP协议模型:
```

AMQP  一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。

AMQP是一个二进制协议，拥有一些现代化特点：多信道、协商式，异步，安全，扩平台，中立，高效。

RabbitMQ是AMQP协议的Erlang的实现。

| 概念           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| 连接Connection | 一个网络连接，比如TCP/IP套接字连接。                         |
| 会话Session    | 端点之间的命名对话。在一个会话上下文中，保证“恰好传递一次”。 |
| 信道Channel    | 多路复用连接中的一条独立的双向数据流通道。为会话提供物理传输介质。 |
| 客户端Client   | AMQP连接或者会话的发起者。AMQP是非对称的，客户端生产和消费消息，服务器存储和路由这些消息。 |
| 服务节点Broker | 消息中间件的服务节点；一般情况下可以将一个RabbitMQ Broker看作一台RabbitMQ 服务器。 |
| 端点           | AMQP对话的任意一方。一个AMQP连接包括两个端点（一个是客户端，一个是服务器）。 |
| 消费者Consumer | 一个从消息队列里请求消息的客户端程序。                       |
| 生产者Producer | 一个向交换机发布消息的客户端应用程序。                       |

![image-20201018092736315](https://gitee.com/xudongyin/img/raw/master/img/20201018092738.png)

### 2.2 RabbitMQ 的安装

#### 2.2.1 下载

`官网下载地址`: https://www.rabbitmq.com/download.html

#### 2.2.2 下载的安装包

> `注意`:这里的安装包是centos7安装的包

#### 2.2.3 安装步骤

```markdown
# 1.将rabbitmq安装包上传到linux系统中
	erlang-22.0.7-1.el7.x86_64.rpm
	rabbitmq-server-3.7.18-1.el7.noarch.rpm

# 2.安装Erlang依赖包
	rpm -ivh erlang-22.0.7-1.el7.x86_64.rpm

# 3.安装RabbitMQ安装包(需要联网)
	yum install -y rabbitmq-server-3.7.18-1.el7.noarch.rpm
		注意:默认安装完成后配置文件模板在:/usr/share/doc/rabbitmq-server-3.7.18/rabbitmq.config.example目录中,需要将配置文件复制到/etc/rabbitmq/目录中,并修改名称为rabbitmq.config
# 4.复制配置文件
	cp /usr/share/doc/rabbitmq-server-3.7.18/rabbitmq.config.example /etc/rabbitmq/rabbitmq.config

# 5.查看配置文件位置
	ls /etc/rabbitmq/rabbitmq.config

# 6.修改配置文件(参见下图:)
	vim /etc/rabbitmq/rabbitmq.config 
```

![image-20201018095258956](https://gitee.com/xudongyin/img/raw/master/img/20201018095300.png)

将上图中配置文件中红色部分去掉`%%`,以及最后的`,`逗号 修改为下图:

![image-20201018095319589](https://gitee.com/xudongyin/img/raw/master/img/20201018095320.png)

```markdown
# 7.执行如下命令,启动rabbitmq中的插件管理
	rabbitmq-plugins enable rabbitmq_management
	
	出现如下说明:
		Enabling plugins on node rabbit@localhost:
    rabbitmq_management
    The following plugins have been configured:
      rabbitmq_management
      rabbitmq_management_agent
      rabbitmq_web_dispatch
    Applying plugin configuration to rabbit@localhost...
    The following plugins have been enabled:
      rabbitmq_management
      rabbitmq_management_agent
      rabbitmq_web_dispatch

    set 3 plugins.
    Offline change; changes will take effect at broker restart.

# 8.启动RabbitMQ的服务
	systemctl start rabbitmq-server
	systemctl restart rabbitmq-server
	systemctl stop rabbitmq-server
	

# 9.查看服务状态(见下图:)
	systemctl status rabbitmq-server
	
	出现如下说明:
  ● rabbitmq-server.service - RabbitMQ broker
     Loaded: loaded (/usr/lib/systemd/system/rabbitmq-server.service; disabled; vendor preset: disabled)
     Active: active (running) since 三 2019-09-25 22:26:35 CST; 7s ago
   Main PID: 2904 (beam.smp)
     Status: "Initialized"
     CGroup: /system.slice/rabbitmq-server.service
             ├─2904 /usr/lib64/erlang/erts-10.4.4/bin/beam.smp -W w -A 64 -MBas ageffcbf -MHas ageffcbf -
             MBlmbcs...
             ├─3220 erl_child_setup 32768
             ├─3243 inet_gethost 4
             └─3244 inet_gethost 4
      .........
```

![image-20201018095543710](https://gitee.com/xudongyin/img/raw/master/img/20201018095545.png)

```markdown
# 10.关闭防火墙服务
	systemctl disable firewalld
    Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
    Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
	systemctl stop firewalld   

# 11.访问web管理界面
	http://10.15.0.8:15672/
```

 ![image-20201018095610937](https://gitee.com/xudongyin/img/raw/master/img/20201018095611.png)

```markdown
# 12.登录管理界面
	username:  guest
	password:  guest
```

![image-20201018095632402](https://gitee.com/xudongyin/img/raw/master/img/20201018095633.png)

----

## 3. RabiitMQ 配置

### 3.1 RabbitMQ 管理命令行

```markdown
# 1.服务启动相关
	systemctl start|restart|stop|status rabbitmq-server

# 2.管理命令行  用来在不使用web管理界面情况下命令操作RabbitMQ
	rabbitmqctl  help  可以查看更多命令

# 3.插件管理命令行
	rabbitmq-plugins enable|list|disable 
```

### 3.2  web管理界面介绍

#### 3.2.1 overview概览

<img src="https://gitee.com/xudongyin/img/raw/master/img/20201018205728.png" alt="image-20191126162026720"  />

- `connections：无论生产者还是消费者，都需要与RabbitMQ建立连接后才可以完成消息的生产和消费，在这里可以查看连接情况`

- `channels：通道，建立连接后，会形成通道，消息的投递获取依赖通道。`

- `Exchanges：交换机，用来实现消息的路由`

- `Queues：队列，即消息队列，消息存放在队列中，等待消费，消费后被移除队列。`

  

#### 3.2.2 Admin用户和虚拟主机管理

##### 1. 添加用户

![image-20191126162617280](https://gitee.com/xudongyin/img/raw/master/img/20201018205728.png)

上面的Tags选项，其实是指定用户的角色，可选的有以下几个：

- `超级管理员(administrator)`

  可登陆管理控制台，可查看所有的信息，并且可以对用户，策略(policy)进行操作。

- `监控者(monitoring)`

  可登陆管理控制台，同时可以查看rabbitmq节点的相关信息(进程数，内存使用情况，磁盘使用情况等)

- `策略制定者(policymaker)`

  可登陆管理控制台, 同时可以对policy进行管理。但无法查看节点的相关信息(上图红框标识的部分)。

- `普通管理者(management)`

  仅可登陆管理控制台，无法看到节点信息，也无法对策略进行管理。

- `其他`

  无法登陆管理控制台，通常就是普通的生产者和消费者。

##### 2. 创建虚拟主机

```markdown
# 虚拟主机
	为了让各个用户可以互不干扰的工作，RabbitMQ添加了虚拟主机（Virtual Hosts）的概念。其实就是一个独立的访问路径，不同用户使用不同路径，各自有自己的队列、交换机，互相不会影响。
```

 ![image-20191126163023153](https://gitee.com/xudongyin/img/raw/master/img/20201018205725.png)

##### 3. 绑定虚拟主机和用户

创建好虚拟主机，我们还要给用户添加访问权限：

点击添加好的虚拟主机：

![image-20191126163506795](https://gitee.com/xudongyin/img/raw/master/img/20201018205722.png)

进入虚拟机设置界面:

![image-20191126163631889](https://gitee.com/xudongyin/img/raw/master/img/20201018205719.png)

### 3.3 RabbitMQ的工作流程介绍

1、建立信息。Publisher定义需要发送消息的结构和内容。

2、建立Conection和Channel。由Publisher和Consumer创建连接，连接到Broker的物理节点上，同时建立Channel。Channel是建立在Connection之上的，一个Connection可以建立多个Channel。Publisher连接Virtual Host 建立Channel，Consumer连接到相应的Queue上建立Channel。

3、声明交换机和队列。声明一个消息交换机（Exchange）和队列（Queue），并设置相关属性。

4、发送消息。由Publisher发送消息到Broker中的Exchange中

5、路由转发。RabbitMQ收到消息后，根据消息指定的Exchange(交换机) 来查找Binding(绑定) 然后根据规则（Routing Key）分发到不同的Queue。这里就是说使用Routing Key在消息交换机（Exchange）和消息队列（Queue）中建立好绑定关系，然后将消息发送到绑定的队列中去。

6、消息接收。Consumer监听相应的Queue，一旦Queue中有可以消费的消息，Queue就将消息发送给Consumer端。

7、消息确认。当Consumer完成某一条消息的处理之后，需要发送一条ACK消息给对应的Queue。

Consumer收到消息时需要显式的向RabbitMQ Broker发送basic.ack消息或者Consumer订阅消息时设置auto_ack参数为true。 
在通信过程中，队列对ACK的处理有以下几种情况：

- 如果Consumer接收了消息，发送ack，RabbitMQ会删除队列中这个消息，发送另一条消息给Consumer。
- 如果Consumer接收了消息, 但在发送ack之前断开Channel，RabbitMQ会认为这条消息没有被deliver（递送）,如果有其他的Channel，会该消息将被发送给另外的Channel。如果没有，当在Consumer再次连接的时候，这条消息会被redeliver（重新递送）。
- 如果consumer接收了消息，但是忘记了ack,RabbitMQ不会重复发送消息。
- 新版RabbitMQ还支持Consumer reject某条（类）消息，可以通过设置requeue参数中的reject为true达到目的，那么Consumer将会把消息发送给下一个注册的Consumer。

8、关闭消息通道（channel）以及和服务器的连接。

-----

## 4.RabbitMQ 的第一个程序

### 4.0 AMQP协议的回顾

![image-20200312140114784](https://gitee.com/xudongyin/img/raw/master/img/20201018205713.png)

### 4.1 RabbitMQ支持的消息模型

![image-20191126165434784](https://gitee.com/xudongyin/img/raw/master/img/20201018205747.png)

![image-20191126165459282](https://gitee.com/xudongyin/img/raw/master/img/20201020092215.png)

### 4.2 引入依赖

```xml
<dependency>
    <groupId>com.rabbitmq</groupId>
    <artifactId>amqp-client</artifactId>
    <version>5.7.2</version>
</dependency>
```

### 4.3 第一种模型(直连)

![image-20191126165840602](https://gitee.com/xudongyin/img/raw/master/img/20201020092216.png)

在上图的模型中，有以下概念：

- P：生产者，也就是要发送消息的程序
- C：消费者：消息的接受者，会一直等待消息到来。
- queue：消息队列，图中红色部分。类似一个邮箱，可以缓存消息；生产者向其中投递消息，消费者从其中取出消息。

##### 1. 开发生产者

```java
  //创建连接工厂
  ConnectionFactory connectionFactory = new ConnectionFactory();
  connectionFactory.setHost("10.15.0.9");
  connectionFactory.setPort(5672);
  connectionFactory.setUsername("ems");
  connectionFactory.setPassword("123");
  connectionFactory.setVirtualHost("/ems");
  Connection connection = connectionFactory.newConnection();
  //创建通道
  Channel channel = connection.createChannel();
  //参数1：队列名称，如果不存在就创建  参数2: 队列是否持久化  参数3:是否独占队列 参数4:队列消费完是否自动删除  参数4:其他属性
  channel.queueDeclare("hello",true,false,false,null);
  //发布消息
  //参数1: 交换机名称 参数2:队列名称  参数3:传递消息额外设置  参数4:消息的具体内容
  channel.basicPublish("","hello", null,"hello rabbitmq".getBytes());
  channel.close();
  connection.close();
```

##### 2. 开发消费者

```java
  //创建连接工厂
  ConnectionFactory connectionFactory = new ConnectionFactory();
  connectionFactory.setHost("10.15.0.9");
  connectionFactory.setPort(5672);
  connectionFactory.setUsername("ems");
  connectionFactory.setPassword("123");
  connectionFactory.setVirtualHost("/ems");
  Connection connection = connectionFactory.newConnection();
  Channel channel = connection.createChannel();
  channel.queueDeclare("hello", true, false, false, null);
  //消费消息
        //参数1: 消费那个队列的消息 队列名称
        //参数2: 开始消息的自动确认机制
        //参数3: 消费时的回调接口
  channel.basicConsume("hello",true,new DefaultConsumer(channel){
    @Override
    public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
      System.out.println(new String(body));
    }
  });
```

##### 3. 参数的说明

```java
  //生产者和消费者的这个API的参数要一致，不然使用不是同一个队列
   channel.queueDeclare("hello",true,false,false,null);
	'参数1':用来声明通道对应的队列
  '参数2':用来指定是否持久化队列，不开启，如果RabbitMQ重启的话会丢失原有的队列，但不会保存消息，只保存队列，要保存消息得在下面的函数里的'第3个参数'设置
  '参数3':用来指定是否独占队列，独占队列只允许一个链接连接
  '参数4':用来指定是否自动删除队列，消费者连接断开后才删除
  '参数5':对队列的额外配置
      
   channel.basicPublish("","hello", null,"hello rabbitmq".getBytes());
   '参数1': 交换机名称
   '参数2':队列名称  
   '参数3':传递消息额外设置,使用 MessageProperties 里的常量进行设置
   '参数4':消息的具体内容
  
```

---

### 4.4 第二种模型(work quene)

`Work queues`，也被称为（`Task queues`），任务模型。当消息处理比较耗时的时候，可能生产消息的速度会远远大于消息的消费速度。长此以往，消息就会堆积越来越多，无法及时处理。此时就可以使用work 模型：**让多个消费者绑定到一个队列，共同消费队列中的消息**。队列中的消息一旦消费，就会消失，因此任务是不会被重复执行的。

![image-20200314221002008](https://gitee.com/xudongyin/img/raw/master/img/20201020092217.png)

角色：

- P：生产者：任务的发布者
- C1：消费者-1，领取任务并且完成任务，假设完成速度较慢
- C2：消费者-2：领取任务并完成任务，假设完成速度快

##### 1. 开发生产者

```java
channel.queueDeclare("hello", true, false, false, null);
for (int i = 0; i < 10; i++) {
  channel.basicPublish("", "hello", null, (i+"====>:我是消息").getBytes());
}
```

##### 2.开发消费者-1

```java
channel.queueDeclare("hello",true,false,false,null);
channel.basicConsume("hello",true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者1: "+new String(body));
  }
});
```

##### 3.开发消费者-2

```java
channel.queueDeclare("hello",true,false,false,null);
channel.basicConsume("hello",true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    try {
      Thread.sleep(1000);   //处理消息比较慢 一秒处理一个消息
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println("消费者2: "+new String(body));  
  }
});
```

##### 4.测试结果

![image-20200314223242058](https://gitee.com/xudongyin/img/raw/master/img/20201020092218.png)

![image-20200314223302207](https://gitee.com/xudongyin/img/raw/master/img/20201020092219.png)

> `总结:默认情况下，RabbitMQ将按顺序将每个消息发送给下一个使用者。平均而言，每个消费者都会收到相同数量的消息。这种分发消息的方式称为循环。`	

##### 5.消息自动确认机制

> Doing a task can take a few seconds. You may wonder what happens if one of the consumers starts a long task and dies with it only partly done. With our current code, once RabbitMQ delivers a message to the consumer it immediately marks it for deletion. In this case, if you kill a worker we will lose the message it was just processing. We'll also lose all the messages that were dispatched to this particular worker but were not yet handled.
>
> But we don't want to lose any tasks. If a worker dies, we'd like the task to be delivered to another worker.
>
> 完成一项任务可能需要几秒钟。你可能会想，如果其中一个消费者开始了一项长期任务，但只完成了一部分就结束了，会发生什么。根据我们当前的代码，一旦RabbitMQ向消费者传递了一条消息，它会立即将其标记为删除。在这种情况下，如果你杀了一个工人，我们将丢失它正在处理的消息。我们还将丢失发送给该特定工作人员但尚未处理的所有消息。
>
> 但是我们不想失去任何任务。如果一个工人死了，我们希望把任务交给另一个工人。
> **只要关闭消息自动确认机制，只要消息没有被手动确认，即使被消费到一半消费者就挂了，这个消息还是会被另外的消费者消费。**

```java
channel.basicQos(1);//一次只接受一条未确认的消息
//参数2:关闭自动确认消息
channel.basicConsume("hello",false,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者1: "+new String(body));
    channel.basicAck(envelope.getDeliveryTag(),false);//手动确认消息
  }
});
```

- 设置通道一次只能消费一个消息

- 关闭消息的自动确认,开启手动确认消息

  ![image-20200314230412178](https://gitee.com/xudongyin/img/raw/master/img/20201020092220.png)

  ![image-20200314230423280](https://gitee.com/xudongyin/img/raw/master/img/20201020092221.png)

### 4.5 RabbitMQ的分发机制

1、Round-robin dispatch（轮询分发）

这个是RabbitMQ默认的消息分发机制，使用任务队列的优点之一就是可以轻易的并行工作。如果我们有很多要分发的消息，可以通过增加工作者（消费者）来解决这种状况，使得系统的伸缩性更加容易扩展。

**在默认情况下，RabbitMQ不会顾虑消息者处理消息的能力，即使其中有的消费者闲置有的消费者高负荷。RabbitMQ会逐个发送消息到在序列中的下一个消费者==(而不考虑每个任务的时长等等，且是提前一次性分配，并非一个一个分配)==。平均每个消费者获得相同数量的消息，这种方式分发消息机制称为Round-Robin（轮询）。**

2、Fair dispatch （公平分发）

**而公平分发，则是根据消费者的处理能力来进行分发处理的。这里主要是==通过设置prefetchCount 参数来实现的==。这样RabbitMQ就会使得每个Consumer在同一个时间点最多处理规定的数量级个数的Message。换句话说，在==接收到该Consumer的ack前，它不会将新的Message分发给它==。 比如prefetchCount=1，则在同一时间下，每个Consumer在同一个时间点最多处理1个Message，同时在收到Consumer的ack前，它不会将新的Message分发给它。**

==注意：使用公平分发，必须关闭自动应答，改为手动应答。==

说完了概念，我们再来思考一下，前面我们的实例。在使用Spring Boot结合RabbitMQ时，我们并没有手动去应答，那么这为啥是采用的公平分发机制？

**这个是因为Spring Boot封装的RabbitMQ方法，默认ACK机制是使用手工应答机制，当@RabbitListener修饰的方法被调用且没有抛出异常时, Spring Boot会为我们自动应答。**

我们可以在@RabbitListener源码的注解里看到，

![RabbitListener](https://gitee.com/xudongyin/img/raw/master/img/20201030160312)

如果没有指定containerFactory，将采用默认的containerFactory。然后我们在RabbitListenerContainerFactory中查看到这个接口与MessageListenerContainer有关， 
![RabbitListenerContainerFactory](https://gitee.com/xudongyin/img/raw/master/img/20201030160823)

接着查看MessageListenerContainer，这是一个接口，我们查看实现了该接口的类，在源码包了提供了一个SimpleMessageListenerContainer的类，在里面我们找到了DEFAULT_PREFETCH_COUNT，这下就清晰明了了。 
![SimpleMessageListenerContainer](https://gitee.com/xudongyin/img/raw/master/img/20201030161005)

默认情况下，Spring Boot中的RabbitMQ采用手动确认机制，只要如果不是程序员编程实现应答，框架就会为我们自动去确认。并且prefetchCount=1，这下就可以解释为啥上面的实例出现的结果了。

---

### 4.6 第三种模型(fanout) 

`fanout 扇出 也称为广播`

![image-20191126213115873](https://gitee.com/xudongyin/img/raw/master/img/20201020092222.png)

在广播模式下，消息发送流程是这样的：

-  可以有多个消费者
-  每个**消费者有自己的queue**（队列）
-  每个**队列都要绑定到Exchange**（交换机）
- **生产者发送的消息，只能发送到交换机**，交换机来决定要发给哪个队列，生产者无法决定。
-  交换机把消息发送给绑定过的所有队列
-  队列的消费者都能拿到消息。实现**一条消息被多个消费者消费**

##### 1. 开发生产者

```java
//声明交换机
channel.exchangeDeclare("logs","fanout");//广播 一条消息多个消费者同时消费
//发布消息
channel.basicPublish("logs","",null,"hello".getBytes());
```

##### 2. 开发消费者-1

```java
//绑定交换机
channel.exchangeDeclare("logs","fanout");
//创建临时队列
String queue = channel.queueDeclare().getQueue();
//将临时队列绑定exchange
channel.queueBind(queue,"logs","");
//处理消息
channel.basicConsume(queue,true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者1: "+new String(body));
  }
});
```

##### 3. 开发消费者-2

```java
//绑定交换机
channel.exchangeDeclare("logs","fanout");
//创建临时队列
String queue = channel.queueDeclare().getQueue();
//将临时队列绑定exchange
channel.queueBind(queue,"logs","");
//处理消息
channel.basicConsume(queue,true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者2: "+new String(body));
  }
});
```

##### 4.开发消费者-3

```java
//绑定交换机
channel.exchangeDeclare("logs","fanout");
//创建临时队列
String queue = channel.queueDeclare().getQueue();
//将临时队列绑定exchange
channel.queueBind(queue,"logs","");
//处理消息
channel.basicConsume(queue,true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者3: "+new String(body));
  }
});
```

##### 5. 测试结果

![image-20200315180653207](https://gitee.com/xudongyin/img/raw/master/img/20201020092223.png)

![image-20200315180708489](https://gitee.com/xudongyin/img/raw/master/img/20201020092224.png)

![image-20200315180728035](https://gitee.com/xudongyin/img/raw/master/img/20201020092225.png)

----

### 4.7 第四种模型(Routing)

#### 4.7.1 Routing 之订阅模型-Direct(直连)

`在Fanout模式中，一条消息，会被所有订阅的队列都消费。但是，在某些场景下，我们希望不同的消息被不同的队列消费。这时就要用到Direct类型的Exchange。`

 在Direct模型下：

- 队列与交换机的绑定，不能是任意绑定了，而是要指定一个`RoutingKey`（路由key）
- 消息的发送方在 向 Exchange发送消息时，也必须指定消息的 `RoutingKey`。
- Exchange不再把消息交给每一个绑定的队列，而是根据消息的`Routing Key`进行判断，只有队列的`Routingkey`与消息的 `Routing key`完全一致，才会接收到消息

流程:

![image-20191126220145375](https://gitee.com/xudongyin/img/raw/master/img/20201020092154.png)

图解：

- P：生产者，向Exchange发送消息，发送消息时，会指定一个routing key。
- X：Exchange（交换机），接收生产者的消息，然后把消息递交给 与routing key完全匹配的队列
- C1：消费者，其所在队列指定了需要routing key 为 error 的消息
- C2：消费者，其所在队列指定了需要routing key 为 info、error、warning 的消息

##### 1. 开发生产者

```java
//声明交换机  参数1:交换机名称 参数2:交换机类型 基于指令的Routing key转发
channel.exchangeDeclare("logs_direct","direct");
String key = "";
//发布消息
channel.basicPublish("logs_direct",key,null,("指定的route key"+key+"的消息").getBytes());
```

##### 2.开发消费者-1

```java
 //声明交换机
channel.exchangeDeclare("logs_direct","direct");
//创建临时队列
String queue = channel.queueDeclare().getQueue();
//绑定队列和交换机
channel.queueBind(queue,"logs_direct","error");
channel.queueBind(queue,"logs_direct","info");
channel.queueBind(queue,"logs_direct","warn");

//消费消息
channel.basicConsume(queue,true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者1: "+new String(body));
  }
});
```

##### 3.开发消费者-2

```java
//声明交换机
channel.exchangeDeclare("logs_direct","direct");
//创建临时队列
String queue = channel.queueDeclare().getQueue();
//绑定队列和交换机
channel.queueBind(queue,"logs_direct","error");
//消费消息
channel.basicConsume(queue,true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者2: "+new String(body));
  }
});
```

##### 4.测试生产者发送Route key为error的消息时

 ![image-20200316102613933](https://gitee.com/xudongyin/img/raw/master/img/20201020092226.png)

 ![image-20200316102627912](https://gitee.com/xudongyin/img/raw/master/img/20201020092227.png)

##### 5.测试生产者发送Route key为info的消息时

 ![image-20200316102925740](https://gitee.com/xudongyin/img/raw/master/img/20201020092228.png)

 ![image-20200316102947326](https://gitee.com/xudongyin/img/raw/master/img/20201020092229.png)

----

#### 4.7.2 Routing 之订阅模型-Topic

`Topic`类型的`Exchange`与`Direct`相比，都是可以根据`RoutingKey`把消息路由到不同的队列。只不过`Topic`类型`Exchange`可以让队列在绑定`Routing key` 的时候使用通配符！这种模型`Routingkey` 一般都是由一个或多个单词组成，多个单词之间以”.”分割，例如： `item.insert`

![image-20191127121900255](https://gitee.com/xudongyin/img/raw/master/img/20201020092230.png)

``` markdown
# 统配符
		* (star) can substitute for exactly one word.    匹配不多不少恰好1个词
		# (hash) can substitute for zero or more words.  匹配一个或多个词
# 如:
		audit.#    匹配 audit.irs.corporate 或者 audit.irs 或者 audit 等
         audit.*   只能匹配 audit.irs
```

##### 1.开发生产者

```java
//生命交换机和交换机类型 topic 使用动态路由(通配符方式)
channel.exchangeDeclare("topics","topic");
String routekey = "user.save";//动态路由key
//发布消息
channel.basicPublish("topics",routekey,null,("这是路由中的动态订阅模型,route key: ["+routekey+"]").getBytes());
```

##### 2.开发消费者-1 

`Routing Key中使用*通配符方式`

```java
 //声明交换机
channel.exchangeDeclare("topics","topic");
//创建临时队列
String queue = channel.queueDeclare().getQueue();
//绑定队列与交换机并设置获取交换机中动态路由
channel.queueBind(queue,"topics","user.*");

//消费消息
channel.basicConsume(queue,true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者1: "+new String(body));
  }
});
```

##### 3.开发消费者-2

`Routing Key中使用#通配符方式`

```java
//声明交换机
channel.exchangeDeclare("topics","topic");
//创建临时队列
String queue = channel.queueDeclare().getQueue();
//绑定队列与交换机并设置获取交换机中动态路由
channel.queueBind(queue,"topics","user.#");

//消费消息
channel.basicConsume(queue,true,new DefaultConsumer(channel){
  @Override
  public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
    System.out.println("消费者2: "+new String(body));
  }
});
```

##### 4.测试结果

 ![image-20200316113935785](https://gitee.com/xudongyin/img/raw/master/img/20201020092231.png)

 ![image-20200316114000459](https://gitee.com/xudongyin/img/raw/master/img/20201020092232.png)







## 5. SpringBoot中使用RabbitMQ

### 5.0 搭建初始环境

##### 	1. 引入依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

##### 	2. 配置配置文件

```yml
spring:
  application:
    name: springboot_rabbitmq
  rabbitmq:
    host: 10.15.0.9
    port: 5672
    username: ems
    password: 123
    virtual-host: /ems
```

`RabbitTemplate`  用来简化操作     使用时候直接在项目中注入即可使用

### 5.1 第一种hello world模型使用

1. ##### 开发生产者

   ```java
   @SpringBootTest(classes = RabbitmqSpringbootApplication.class)
   @RunWith(SpringRunner.class)
   public class TestRabbitMQ {
       @Autowired
       private RabbitTemplate rabbitTemplate;
   
       @Test
       public void testHello(){
           rabbitTemplate.convertAndSend("hello","hello world");
       }
   }
   ```

2. ##### 开发消费者

   ```java
   @Component
   @RabbitListener(queuesToDeclare = @Queue("hello"))
   public class HelloCustomer {
   
       @RabbitHandler
       public void receive1(String message){
           System.out.println("message = " + message);
       }
   }
   ```

### 5.2 第二种work模型使用

1. ##### 开发生产者

   ```java
   @Autowired
   private RabbitTemplate rabbitTemplate;
   
   @Test
   public void testWork(){
     for (int i = 0; i < 10; i++) {
       rabbitTemplate.convertAndSend("work","hello work!");
     }
   }
   ```

2. ##### 开发消费者

   ```java
   @Component
   public class WorkCustomer {
       @RabbitListener(queuesToDeclare = @Queue("work"))
       public void receive1(String message){
           System.out.println("work message1 = " + message);
       }
   
       @RabbitListener(queuesToDeclare = @Queue("work"))
       public void receive2(String message){
           System.out.println("work message2 = " + message);
       }
   }
   ```

   > `说明:默认在Spring AMQP实现中Work这种方式就是公平调度,如果需要实现能者多劳需要额外配置`

### 5.3 Fanout 广播模型

1. ##### 开发生产者

   ```java
   @Autowired
   private RabbitTemplate rabbitTemplate;
   
   @Test
   public void testFanout() throws InterruptedException {
     rabbitTemplate.convertAndSend("logs","","这是日志广播");
   }
   ```

   

2. ##### 开发消费者

   ```java
   @Component
   public class FanoutCustomer {
   
       @RabbitListener(bindings = @QueueBinding(
               value = @Queue,
               exchange = @Exchange(name="logs",type = "fanout")
       ))
       public void receive1(String message){
           System.out.println("message1 = " + message);
       }
   
       @RabbitListener(bindings = @QueueBinding(
               value = @Queue, //创建临时队列
               exchange = @Exchange(name="logs",type = "fanout")  //绑定交换机类型
       ))
       public void receive2(String message){
           System.out.println("message2 = " + message);
       }
   }
   ```

   

### 5.4 Route 路由模型

1. ##### 开发生产者

   ```java
   @Autowired
   private RabbitTemplate rabbitTemplate;
   
   @Test
   public void testDirect(){
     rabbitTemplate.convertAndSend("directs","error","error 的日志信息");
   }
   ```

2. ##### 开发消费者

   ```java
   @Component
   public class DirectCustomer {
   
       @RabbitListener(bindings ={
               @QueueBinding(
                       value = @Queue(),
                       key={"info","error"},
                       exchange = @Exchange(type = "direct",name="directs")
               )})
       public void receive1(String message){
           System.out.println("message1 = " + message);
       }
   
       @RabbitListener(bindings ={
               @QueueBinding(
                       value = @Queue(),
                       key={"error"},
                       exchange = @Exchange(type = "direct",name="directs")
               )})
       public void receive2(String message){
           System.out.println("message2 = " + message);
       }
   }
   
   ```

### 5.5 Topic 订阅模型(动态路由模型)

1. ##### 开发生产者

   ```java
   @Autowired
   private RabbitTemplate rabbitTemplate;
   
   //topic
   @Test
   public void testTopic(){
     rabbitTemplate.convertAndSend("topics","user.save.findAll","user.save.findAll 的消息");
   }
   ```

   

2. ##### 开发消费者

   ```java
   @Component
   public class TopCustomer {
       @RabbitListener(bindings = {
               @QueueBinding(
                       value = @Queue,
                       key = {"user.*"},
                       exchange = @Exchange(type = "topic",name = "topics")
               )
       })
       public void receive1(String message){
           System.out.println("message1 = " + message);
       }
   
       @RabbitListener(bindings = {
               @QueueBinding(
                       value = @Queue,
                       key = {"user.#"},
                       exchange = @Exchange(type = "topic",name = "topics")
               )
       })
       public void receive2(String message){
           System.out.println("message2 = " + message);
       }
   }
   ```

### 5.6 消息确认、消息拒绝与消息回调

在resources中把下面的代码注释打开，采用手动确认消息机制。

```properties
# 采用手动应答
spring.rabbitmq.listener.acknowledge-mode=manual12
```

然后我们在接收消息中，即Receiver中如下设置：

```java
    @RabbitListener(queues = "hello")
    public void process(Message message, Channel channel) throws IOException {
        System.out.println("CheckReceiver: " + new String(message.getBody()));
        try {
            doWork();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 消息的标识，false只确认当前一个消息收到，true确认所有consumer获得的消息
        channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
    }
```

**注意**：采用了手动确认消息后，所有消费者都需要手动确认消息是否收到完毕。不然RabbitMQ会认为消息投递失败，反复投递。

当然除了消息确认外，还有消息拒绝，当我们拒绝某个消息时，让RabbitMQ会把消息传递给它的下一个消费者接受该消息，直到该消息把确认收到为止，也可以让RabbitMQ把消息给删除掉。

这里的使用和消息确认基本一致，传递不同的参数采用不同的操作。

```java
// true 发送给下一个消费者
// false 谁都不接受，从队列中删除
// 拒绝消息
channel.basicReject(message.getMessageProperties().getDeliveryTag(), true);
```

除了上面的消息确认、拒绝外，RabbitMQ还带消息的回调确认，用户是否收到消息，发送者的消息是否成功投递，可以通过Callback中的确认来实现。 
通过在Sender中实现RabbitTemplate.ConfirmCallback接口来实现该操作，如下：

```java
@Component
public class CallBackSender implements RabbitTemplate.ConfirmCallback {
    @Autowired
    private RabbitTemplate rabbitTemplatenew;

    public void send() {

        rabbitTemplatenew.setConfirmCallback(this);
        String msg = "callbackSender : i am callback sender";
        System.out.println(msg);
        CorrelationData correlationData = new CorrelationData(UUID.randomUUID().toString());
        System.out.println("callbackSender UUID: " + correlationData.getId());
        this.rabbitTemplatenew.convertAndSend("exchange", "topic.messages", msg, correlationData);
    }

    public void confirm(CorrelationData correlationData, boolean ack, String cause) {
        System.out.println("callbakck confirm: " + correlationData.getId() + " ACK : " + ack);
    }
}
```

通过ACK的返回值，我们可以确认用户是否消费掉该消息没有，然后做后续的操作。 

----

## 6、消息确认机制(AMQP事务)

我们知道可以通过持久化（交换机、队列和消息持久化）来保障我们在服务器崩溃时，重启服务器消息数据不会丢失。但是我们无法确认当消息的发布者在将消息发送出去之后，消息到底有没有正确到达Broker代理服务器呢？如果不进行特殊配置的话，默认情况下发布操作是不会返回任何信息给生产者的，也就是默认情况下我们的生产者是不知道消息有没有正确到达Broker的。如果在消息到达Broker之前已经丢失的话，持久化操作也解决不了这个问题，因为消息根本就没到达代理服务器，这个是没有办法进行持久化的，那么当我们遇到这个问题又该如何去解决呢？

这里就是我们讲解到的RabbitMQ中的消息确认机制，通过消息确认机制我们可以确保我们的消息可靠送达到我们的用户手中，即使消息丢失掉，我们也可以通过进行重复分发确保用户可靠收到消息。

RabbitMQ消息确认机制，主要包括两个方面，因为RabbitMQ为我们提供了两种方式：

- 通过AMQP事务机制实现，这也是AMQP协议层面提供的解决方案；
- 通过将channel设置成confirm模式来实现；

### 6.1 AMQP事务

#### 1、使用java原生事务

我们知道事务可以保证消息的传递，使得可靠消息最终一致性。接下来我们先来探究一下RabbitMQ的事务机制。

RabbitMQ中与事务有关的主要有三个方法：

- txSelect()
- txCommit()
- txRollback()

txSelect主要用于将当前channel设置成transaction模式，txCommit用于提交事务，txRollback用于回滚事务。

当我们使用txSelect提交开始事务之后，我们就可以发布消息给Broke代理服务器，如果txCommit提交成功了，则消息一定到达了Broker了，如果在txCommit执行之前Broker出现异常崩溃或者由于其他原因抛出异常，这个时候我们便可以捕获异常通过txRollback方法进行回滚事务了。

所以RabbitMQ事务中的主要代码为：

```java
   channel.txSelect();
   channel.basicPublish(exchange,routingKey,MessageProperties.PERSISTENT_TEXT_PLAIN, msg.getBytes());
   channel.txCommit();
```

先进行事务提交，然后开始发送消息，最后提交事务。

还是在原来的demo代码（这些代码本笔记没有在前面记载）基础下，在sender和receiver包下分别新建TransactionSender1.java和TransactionReceiver1.java。分别如下所示：

TransactionSender1.java

```java
package net.anumbrella.rabbitmq.sender;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class TransactionSender1 {

	private final static String QUEUE_NAME = "transition";

	public static void main(String[] args) throws IOException, TimeoutException {
		/**
		 * 创建连接连接到MabbitMQ
		 */
		ConnectionFactory factory = new ConnectionFactory();

		// 设置MabbitMQ所在主机ip或者主机名
		factory.setUsername("guest");
		factory.setPassword("guest");
		factory.setHost("127.0.0.1");
		factory.setVirtualHost("/");
		factory.setPort(5672);

		// 创建一个连接
		Connection connection = factory.newConnection();

		// 创建一个频道
		Channel channel = connection.createChannel();

		// 指定一个队列
		channel.queueDeclare(QUEUE_NAME, false, false, false, null);
		// 发送的消息
		String message = "This is a transaction message！";

		try {
			// 开启事务
			channel.txSelect();
			// 往队列中发出一条消息，使用rabbitmq默认交换机
			channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
			// 提交事务
			channel.txCommit();
		} catch (Exception e) {
			e.printStackTrace();
			// 事务回滚
			channel.txRollback();
		}

		System.out.println(" TransactionSender1 Sent '" + message + "'");
		// 关闭频道和连接
		channel.close();
		connection.close();
	}

}
```

在上面中我们使用try-catch来捕获异常，如果发送失败，就会进行事务回滚。

TransactionReceiver1.java

```java
package net.anumbrella.rabbitmq.receiver;

import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.TimeoutException;

import com.rabbitmq.client.AMQP;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Consumer;
import com.rabbitmq.client.DefaultConsumer;
import com.rabbitmq.client.Envelope;

public class TransactionReceiver1 {
	
	private final static String QUEUE_NAME = "transition";

	public static void main(String[] argv) throws IOException, InterruptedException, TimeoutException {

		ConnectionFactory factory = new ConnectionFactory();

		factory.setUsername("guest");
		factory.setPassword("guest");
		factory.setHost("127.0.0.1");
		factory.setVirtualHost("/");
		factory.setPort(5672);
		// 打开连接和创建频道，与发送端一样

		Connection connection = factory.newConnection();
		Channel channel = connection.createChannel();

		// 声明队列，主要为了防止消息接收者先运行此程序，队列还不存在时创建队列。
		channel.queueDeclare(QUEUE_NAME, false, false, false, null);
		System.out.println("Receiver1 waiting for messages. To exit press CTRL+C");

		// 创建队列消费者
		final Consumer consumer = new DefaultConsumer(channel) {
			@Override
			public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties,
					byte[] body) throws IOException {
				SimpleDateFormat time = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss:SSSS");

				String message = new String(body, "UTF-8");

				System.out.println(" TransactionReceiver1  : " + message);
				System.out.println(" TransactionReceiver1 Done! at " + time.format(new Date()));
			}
		};
		channel.basicConsume(QUEUE_NAME, true, consumer);
	}

}
```

消息的接收者跟原来是一样的，因为**事务主要是保证消息要发送到Broker当中**。
接着我们使用wireshark来监听网络，这里也可以使用Fiddler。由于笔者使用的是MAC系统，没有Fiddler版本。如果读者要使用Fiddler，同时使用windows可以看看这篇文章，后面有对Fiddler的介绍，[JMeter搭配Fiddler的简单使用（一）](https://blog.csdn.net/anumbrella/article/details/78940826)。

启动wireshark，选择好网络，输入amqp过滤我们需要的信息。
然后我们分别启动TransactionReceiver1.java 和 TransactionSender1.java。

![wireshark](https://gitee.com/xudongyin/img/raw/master/img/20201030233858)

从上面我们可以清晰的看见消息的分发过程，与我们前面分析的一致。主要执行了四个步骤：

1. Client发送Tx.Select
2. Broker发送Tx.Select-Ok(在它之后，发送消息)
3. Client发送Tx.Commit
4. Broker发送Tx.Commit-Ok

接下来我们通过抛出异常来模拟发送消息错误，进行事务回滚。更改发送信息代码为：

```java
		try {
			// 开启事务
			channel.txSelect();
			// 往队列中发出一条消息，使用rabbitmq默认交换机
			channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
			// 除以0，模拟异常，使用rabbitmq默认交换机
			int t = 1/0;
			// 提交事务
			channel.txCommit();
		} catch (Exception e) {
			e.printStackTrace();
			// 事务回滚
			channel.txRollback();
		}
```

这里我们通过除以0来模拟抛出异常，接着按同样的顺序运行代码。

![rollback](https://gitee.com/xudongyin/img/raw/master/img/20201030233904)

可以看见事务进行了回滚，同时我们在接收端也没有收到消息。

通过上面我们可以知道**事务确实能够解决消息的发送者和Broker之间消息的确认，只有当消息成功被服务端Broker接收，并且接受时，事务才能提交成功，不然我们便可以在捕获异常进行事务回滚操作同时进行消息重发**。

在上面的情况中，我们使用java原生代码来模拟事务进行发送，而在实际开发中，我们可能需要结合框架来完成。

#### 2、结合Spring Boot来使用事务

我们一般在Spring Boot使用RabbitMQ，主要是通过封装的RabbitTemplate模板来实现消息的发送，这里主要也是分为两种情况，使用RabbitTemplate同步发送，或者异步发送。

注意：**发布确认和事务。(两者不可同时使用)在channel为事务时，不可引入确认模式；同样channel为确认模式下，不可使用事务。**

所以在使用事务时，在application.properties中，需要将确认模式更改为false。

```properties
# 支持发布确认
spring.rabbitmq.publisher-confirms=false
```

##### A、同步

通过设置RabbitTemplate的channelTransacted为true，来设置事务环境，使得可以使用RabbitMQ事务。如下：

```java
template.setChannelTransacted(true);
```

在demo代码里面，主要是在config包下的RabbitConfig.java里的rabbitTemplateNew方法里面配置，如下：

```java
	@Bean
	@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
	public RabbitTemplate rabbitTemplateNew() {
		RabbitTemplate template = new RabbitTemplate(connectionFactory());
		template.setChannelTransacted(true);
		return template;
	}
```

接着在在sender和receiver包，分别建立TransactionSender2.java和TransactionReceiver2.java。分别如下所示：

TransactionSender2.java

```java
package net.anumbrella.rabbitmq.sender;

import java.text.SimpleDateFormat;
import java.util.Date;

import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.transaction.annotation.Transactional;

@Component
public class TransactionSender2 {

	@Autowired
	private AmqpTemplate rabbitTemplate;

	@Transactional(rollbackFor = Exception.class)
	public void send(String msg) {
		SimpleDateFormat time = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		String sendMsg = msg + time.format(new Date()) + " This is a transaction message！ ";
		/**
		 * 这里可以执行数据库操作
		 * 
		 **/
		System.out.println("TransactionSender2 : " + sendMsg);
		this.rabbitTemplate.convertAndSend("transition", sendMsg);
	}

}
```

在上面代码中，我们通过调用者提供外部事务 @Transactional ( rollbackFor = Exception.class)，来现实事务方法。一旦方法中抛出异常，比如执行数据库操作时，就会被捕获到，同时事务将进行回滚，并且向外发送的消息将不会发送出去。

TransactionReceiver2.java

```java
package net.anumbrella.rabbitmq.receiver;

import java.io.IOException;

import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

import com.rabbitmq.client.Channel;

@Component
public class TransactionReceiver2 {

	@RabbitListener(queues = "transition")
	public void process(Message message, Channel channel) throws IOException {
		System.out.println("TransactionReceiver2  : " + new String(message.getBody()));
	}
}
```

添加完消息的发送者和接收者后，还需要在controller包下的RabbitTest.java中添加模拟消息发送的Restful接口方法，添加如下代码：

```java
	@Autowired
	private TransactionSender2 transactionSender;
	
	/**
	 * 事务消息发送测试
	 */
	@GetMapping("/transition")
	public void transition() {
		transactionSender.send("Transition:  ");
	}
```

启动wireshark，选择好网络，输入amqp过滤我们需要的信息。
然后启动Spring Boot项目，访问接口http://localhost:8080/rabbit/transition。

在控制台我们可以得到消息已经发送和收到，

```verilog
TransactionSender2 : Transition:  2018-06-18 23:00:16 This is a transaction message！ 
TransactionReceiver2  : Transition:  2018-06-18 23:00:16 This is a transaction message！ 
```

查看wireshark如下：
![事务](https://gitee.com/xudongyin/img/raw/master/img/20201030233912)

可以看到这里与前面我们讲解的原生事务是一致的，而当发送消息出现异常时，就会响应执行事务回滚。

##### B、异步

刚才我们讲解的是同步的情况，现在我们讲解一下异步的形式。在异步当中，主要使用MessageListener 接口，它是 Spring AMQP 异步消息投递的监听器接口。而MessageListener的实现类SimpleMessageListenerContainer则是作为了整个异步消息投递的核心类存在。

接下来我们开始介绍使用异步的方法，同样表示需要的外部事务，用户需要在容器配置的时候指定PlatformTransactionManager的实现。代码如下：

```java
    @Bean
    public ConnectionFactory connectionFactory() {

        CachingConnectionFactory connectionFactory = new CachingConnectionFactory();
        connectionFactory.setAddresses(addresses + ":" + port);
        connectionFactory.setUsername(username);
        connectionFactory.setPassword(password);
        connectionFactory.setVirtualHost(virtualHost);
        /** 如果要进行消息回调，则这里必须要设置为true */
        connectionFactory.setPublisherConfirms(publisherConfirms);
        return connectionFactory;
    }

    @Bean
	public SimpleMessageListenerContainer messageListenerContainer() {
		SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
		container.setConnectionFactory(connectionFactory());
		container.setTransactionManager(rabbitTransactionManager());//rabbitTransactionManager()方法在下面代码
		container.setChannelTransacted(true);
		// 开启手动确认
		container.setAcknowledgeMode(AcknowledgeMode.MANUAL);
		container.setQueues(transitionQueue());//transitionQueue()方法在下面代码有
		container.setMessageListener(new TransitionConsumer());//TransitionConsumer()也在下面代码有
		return container;
	}
	/**
	 * 声明transition2队列
	 * @return
	 */
	@Bean
	public Queue transitionQueue() {
		return new Queue("transition2");
	}
	
	/**
	 * 事务管理
	 * 
	 * @return
	 */
	@Bean
	public RabbitTransactionManager rabbitTransactionManager() {
		return new RabbitTransactionManager(connectionFactory());
	}

	/**
	 * 自定义消费者
	 */
	public class TransitionConsumer implements ChannelAwareMessageListener {

		@Override
		public void onMessage(Message message, Channel channel) throws Exception {
			byte[] body = message.getBody();
			System.out.println("TransitionConsumer: " + new String(body));
			// 确认消息成功消费
			channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
			// 除以0，模拟异常，进行事务回滚
			// int t = 1 / 0;
		}
	}
```

这段代码我们是添加在config下的RabbitConfig.java下，通过配置事务管理器，将channelTransacted属性被设置为true。

在容器中配置事务时，如果提供了transactionManager，channelTransaction必须为true，使得如果监听器处理失败，并且抛出异常，那么事务将进行回滚，那么消息将返回给消息代理；如果为false，外部的事务仍然可以提供给监听容器，造成的影响是在回滚的业务操作中也会提交消息传输的操作。

通过使用RabbitTransactionManager，这个事务管理器是PlatformTransactionManager接口的实现，它只能在一个RabbitConnectionFactory中使用。

**注意**：这种策略不能够提供XA事务，例如在消息和数据库之间共享事务。

因为我们在container中设置队列为“transition2”，所以我们在TransactionSender2中更改发送的队列为“transition2”，如下：

```java
this.rabbitTemplate.convertAndSend("transition2", sendMsg);
```

接着我们启动wireshark，选择好网络，输入amqp过滤我们需要的信息。
然后启动Spring Boot项目，访问接口http://localhost:8080/rabbit/transition。

我们可以在wireshark中看到有事务的提交，如下：
![事务1](https://gitee.com/xudongyin/img/raw/master/img/20201030233919)

然后我们在TransitionConsumer中把除以0的模拟异常情况打开，然后再执行上面的操作，可得：
![事务2](https://gitee.com/xudongyin/img/raw/master/img/20201030233920)

可以看到先进行了事务提交，后面事务又回滚了。意味着消息没有接收成功，我们在RabbitMQ管理界面也可以查看到消息，如果将consumer关掉，则unacked的msg则会又回到了ready状态。（注意：这里我们模拟的是消费者接收事务，前面是消息生成者发送到Broker的事务）
![unacked](https://gitee.com/xudongyin/img/raw/master/img/20201030233926)

关闭消息者监听后，消息又恢复了ready状态。当重启应用会重新发过它。
![ready](https://gitee.com/xudongyin/img/raw/master/img/20201030165543)

但是**使用事务虽然可以保证消息的准确达到，但是它极大地牺牲了性能**，因此我们为了性能上的要求，可以采用另一种**高效的解决方案——通过使用Confirm模式来保证消息的准确性**。

### 6.2 Confirm模式

这里的Confirm模式可以分为两个方面来讲解，一是消息的生产者(Producer)的Confirm模式，另一个是消息的消费者(Consumer)的Confirm模式。

#### 1、生产者(Producer)的Confirm模式

通过生产者的确认模式我们是要保证消息准确达到Broker端，而与AMQP事务不同的是Confirm是针对一条消息的，而事务是可以针对多条消息的。

发送原理图大致如下：

![Producer-Confirm](https://gitee.com/xudongyin/img/raw/master/img/20201030233938)

为了使用Confirm模式，client会发送confirm.select方法帧。通过是否设置了no-wait属性，来决定Broker端是否会以confirm.select-ok来进行应答。**一旦在channel上使用confirm.select方法，channel就将处于Confirm模式。处于 transactional模式的channel不能再被设置成Confirm模式，反之亦然**。channel一旦设置为Confirm模式就不能为事务模式，为事务模式就不能为Confirm模式。

在生产者将信道设置成Confirm模式，一旦信道进入Confirm模式，所有在该信道上面发布的消息都会被指派一个唯一的ID(以confirm.select为基础从1开始计数)，一旦消息被投递到所有匹配的队列之后，Broker就会发送一个确认给生产者（包含消息的唯一ID）,这就使得生产者知道消息已经正确到达目的队列了，如果消息和队列是可持久化的，那么确认消息会将消息写入磁盘之后发出，**Broker回传给生产者的确认消息中deliver-tag域包含了确认消息的序列号**，此外Broker也可以设置basic.ack的multiple域，表示到这个序列号之前的所有消息都已经得到了处理。

Confirm模式最大的好处在于它是异步的，一旦发布一条消息，生产者应用程序就可以在等信道返回确认的同时继续发送下一条消息，当消息最终得到确认之后，生产者应用便可以通过回调方法来处理该确认消息，如果RabbitMQ因为自身内部错误导致消息丢失，就会发送一条basic.nack来代替basic.ack的消息，在这个情形下，basic.nack中各域值的含义与basic.ack中相应各域含义是相同的，同时requeue域的值应该被忽略。通过nack一条或多条消息， Broker表明自身无法对相应消息完成处理，并拒绝为这些消息的处理负责。在这种情况下，client可以选择将消息re-publish。

在channel 被设置成Confirm模式之后，所有被publish的后续消息都将被Confirm（即 ack）或者被nack一次。但是没有对消息被Confirm的快慢做任何保证，并且同一条消息不会既被Confirm又被nack。

#### 开启confirm模式的方法

生产者通过调用channel的confirmSelect方法将channel设置为Confirm模式，如果没有设置no-wait标志的话，Broker会返回confirm.select-ok表示同意发送者将当前channel信道设置为Confirm模式(从目前RabbitMQ最新版本3.6来看，如果调用了channel.confirmSelect方法，默认情况下是直接将no-wait设置成false的，也就是默认情况下broker是必须回传confirm.select-ok的)。

![confirm](https://gitee.com/xudongyin/img/raw/master/img/20201031101200)

#### 编程模式

对于固定消息体大小和线程数，如果消息持久化，生产者Confirm(或者采用事务机制)，消费者ack那么对性能有很大的影响.

消息持久化的优化没有太好方法，用更好的物理存储（SAS, SSD, RAID卡）总会带来改善。生产者confirm这一环节的优化则主要在于客户端程序的优化之上。归纳起来，客户端实现生产者confirm有三种编程方式：

1. 普通Confirm模式：每发送一条消息后，调用waitForConfirms()方法，等待服务器端Confirm。实际上是一种串行Confirm了，每publish一条消息之后就等待服务端Confirm，如果服务端返回false或者超时时间内未返回，客户端进行消息重传；
2. 批量Confirm模式：批量Confirm模式，每发送一批消息之后，调用waitForConfirms()方法，等待服务端Confirm，这种批量确认的模式极大的提高了Confirm效率，但是如果一旦出现Confirm返回false或者超时的情况，客户端需要将这一批次的消息全部重发，这会带来明显的重复消息，如果这种情况频繁发生的话，效率也会不升反降；
3. 异步Confirm模式：提供一个回调方法，服务端Confirm了一条或者多条消息后Client端会回调这个方法。

##### 1、普通Confirm模式

主要代码为：

```java
channel.basicPublish("", QUEUE_NAME, MessageProperties.PERSISTENT_BASIC, (" Confirm模式， 第" + (i + 1) + "条消息").getBytes());
if (channel.waitForConfirms()) {
   System.out.println("发送成功");
}else{
  //进行消息重发
}
```

普通Confirm模式最简单，publish一条消息后，等待服务器端Confirm，如果服务端返回false或者超时时间内未返回，客户端就进行消息重传。

我们还是结合代码来讲解，下载原来的代码 [rabbitmq-demo](https://github.com/Shuyun123/rabbitmq-demo)，然后在sender和receiver中分别新建代码ConfirmSender1.java和ConfirmReceiver1.java。

ConfirmSender1.java：

```java
package net.anumbrella.rabbitmq.sender;


import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.MessageProperties;
import org.apache.commons.lang.StringUtils;
import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.beans.factory.annotation.Autowired;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * 这是java原生类支持RabbitMQ，直接运行该类
 */
public class ConfirmSender1 {

    private final static String QUEUE_NAME = "confirm";

    public static void main(String[] args) throws IOException, TimeoutException, InterruptedException {
        /**
         * 创建连接连接到RabbitMQ
         */
        ConnectionFactory factory = new ConnectionFactory();

        // 设置RabbitMQ所在主机ip或者主机名
        factory.setUsername("guest");
        factory.setPassword("guest");
        factory.setHost("127.0.0.1");
        factory.setVirtualHost("/");
        factory.setPort(5672);

        // 创建一个连接
        Connection connection = factory.newConnection();

        // 创建一个频道
        Channel channel = connection.createChannel();

        // 指定一个队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 发送的消息
        String message = "This is a confirm message！";

        channel.confirmSelect();
        final long start = System.currentTimeMillis();
        //发送持久化消息
        for (int i = 0; i < 5; i++) {
            //第一个参数是exchangeName(默认情况下代理服务器端是存在一个""名字的exchange的,
            //因此如果不创建exchange的话我们可以直接将该参数设置成"",如果创建了exchange的话
            //我们需要将该参数设置成创建的exchange的名字),第二个参数是路由键
            channel.basicPublish("", QUEUE_NAME, MessageProperties.PERSISTENT_BASIC, (" Confirm模式， 第" + (i + 1) + "条消息").getBytes());
            if (channel.waitForConfirms()) {
                System.out.println("发送成功");
            }else{
                // 进行消息重发
            }
        }
        System.out.println("执行waitForConfirms耗费时间: " + (System.currentTimeMillis() - start) + "ms");
        // 关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

我们在代码中发送了5条消息到Broker端，每条消息发送后都会等待确认。

ConfirmReceiver1.java：

```java
package net.anumbrella.rabbitmq.receiver;


import com.rabbitmq.client.*;

import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.TimeoutException;

/**
 * 这是java原生类支持RabbitMQ，直接运行该类
 */
public class ConfirmReceiver1 {

    private final static String QUEUE_NAME = "confirm";

    public static void main(String[] argv) throws IOException, InterruptedException, TimeoutException {

        ConnectionFactory factory = new ConnectionFactory();

        factory.setUsername("guest");
        factory.setPassword("guest");
        factory.setHost("127.0.0.1");
        factory.setVirtualHost("/");
        factory.setPort(5672);
        // 打开连接和创建频道，与发送端一样

        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();

        // 声明队列，主要为了防止消息接收者先运行此程序，队列还不存在时创建队列。
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        System.out.println("ConfirmReceiver1 waiting for messages. To exit press CTRL+C");

        // 创建队列消费者
        final Consumer consumer = new DefaultConsumer(channel) {
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties,
                                       byte[] body) throws IOException {
                SimpleDateFormat time = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss:SSSS");

                String message = new String(body, "UTF-8");

                System.out.println(" ConfirmReceiver1  : " + message);
                System.out.println(" ConfirmReceiver1 Done! at " + time.format(new Date()));
            }
        };
        channel.basicConsume(QUEUE_NAME, true, consumer);
    }
}
```

我们开启WireShak，监听RabbitMQ消息的发送。然后我们直接运行ConfirmSender1.java类，可以不用运行ConfirmReceiver.java，因为我们主要是测试消息到达Broker端，这主要是涉及到Producer和RabbitMQ的服务端。

在控制台打印出了信息：

```verilog
发送成功
发送成功
发送成功
发送成功
发送成功
执行waitForConfirms耗费时间: 181ms
```

在RabbitMQ管理界面confirm队列里，我们可以查看到我们发送的5条消息数据。
![message](https://gitee.com/xudongyin/img/raw/master/img/20201031103140)

在WireShark中也可以发现开启了Confirm模式，以及我们发送的5条消息。
![WireShark](https://gitee.com/xudongyin/img/raw/master/img/20201031103144)
接着我们启动ConfirmReceiver.java，可以收到我们发送的具体消息：

```verilog
 ConfirmReceiver1 waiting for messages. To exit press CTRL+C
 ConfirmReceiver1  :  Confirm模式， 第1条消息
 ConfirmReceiver1 Done! at 2018-08-04 14:58:27:0014
 ConfirmReceiver1  :  Confirm模式， 第2条消息
 ConfirmReceiver1 Done! at 2018-08-04 14:58:27:0016
 ConfirmReceiver1  :  Confirm模式， 第3条消息
 ConfirmReceiver1 Done! at 2018-08-04 14:58:27:0016
 ConfirmReceiver1  :  Confirm模式， 第4条消息
 ConfirmReceiver1 Done! at 2018-08-04 14:58:27:0017
 ConfirmReceiver1  :  Confirm模式， 第5条消息
 ConfirmReceiver1 Done! at 2018-08-04 14:58:27:0017
```

##### 2、批量Confirm模式

主要代码为：

```java
channel.confirmSelect();
for(int i=0;i<5;i++){
     channel.basicPublish("", QUEUE_NAME, MessageProperties.PERSISTENT_BASIC, (" Confirm模式， 第" + (i + 1) + "条消息").getBytes());
}
if(channel.waitForConfirms()){
    System.out.println("发送成功");
}else{
	// 进行消息重发
}
```

这里主要更改代码为发送批量消息后再进行等待服务器确认，还可以调用`channel.waitForConfirmsOrDie()`方法，该方法会等到最后一条消息得到确认或者得到nack才会结束，也就是说在waitForConfirmsOrDie处会造成当前程序的阻塞。更改代码为批量Confirm模式，运行我们查看控制台：

```verilog
发送成功
执行waitForConfirms耗费时间: 59ms
```

在WireShark查看信息如下：
![批量Confirm](https://gitee.com/xudongyin/img/raw/master/img/20201031103148)

可以发现这里处理的就是在批量发送信息完毕后，再进行ACK确认。同时我们发现这里只有三个Basic.Ack，这是因为Broker对信息进行了批量处理。

![批量处理](https://gitee.com/xudongyin/img/raw/master/img/20201031103151)

我们可以发现multiple的值为true，这与前面我们讲解的一致，true确认所有将比第一个参数指定的 delivery-tag 小的消息都得到确认。

我们也可以发现执行时间比第一种模式缩短了很多，效率极大提高了。

如果我们要对每条消息进行监听处理，可以通过在channel中添加监听器来实现，**代码添加在发布者**

```java
channel.addConfirmListener(new ConfirmListener() {

    @Override
    public void handleNack(long deliveryTag, boolean multiple) throws IOException {
        System.out.println("nack: deliveryTag = " + deliveryTag + " multiple: " + multiple);
    }

    @Override
    public void handleAck(long deliveryTag, boolean multiple) throws IOException {
        System.out.println("ack: deliveryTag = " + deliveryTag + " multiple: " + multiple);
    }
});
```

当收到Broker发送过来的ack消息时就会调用handleAck方法，收到nack时就会调用handleNack方法。

我们可以在控制台看到信息，这次调用了两次Basic.Ack方法。

```verilog
ack: deliveryTag = 4 multiple: true
ack: deliveryTag = 5 multiple: false
发送成功
执行waitForConfirms耗费时间: 50ms
```

##### 3、异步Confirm模式

这里使用的异步Confirm模式，也要用到上面提到的监听，但是这里需要我们自己去维护实现一个waitForConfirms()方法或waitForConfirmsOrDie()，而waitForConfirms()是同步的，因此我们需要自己去实现维护delivery-tag。

![waitForConfirms](https://gitee.com/xudongyin/img/raw/master/img/20201031103155)

我们可以在jar中查看到源码，其实waitForConfirmsOrDie()最终调用的也是waitForConfirms()方法，在waitForConfirms()方法内部维护了一个同步块代码，而unconfirmedSet就是存储delivery-tag标识的。

我们要实现自己异步调用，主要就是为了维护delivery-tag，**在发送者代码里进行修改**，主要实现代码如下：

```java
SortedSet<Long> confirmSet = Collections.synchronizedSortedSet(new TreeSet<Long>());
channel.confirmSelect();
channel.addConfirmListener(new ConfirmListener() {
    public void handleAck(long deliveryTag, boolean multiple) throws IOException {
        if (multiple) {
            confirmSet.headSet(deliveryTag + 1L).clear();
        } else {
            confirmSet.remove(deliveryTag);
        }
    }
    public void handleNack(long deliveryTag, boolean multiple) throws IOException {
        System.out.println("Nack, SeqNo: " + deliveryTag + ", multiple: " + multiple);
        /**     
               消息发送失败，应该进行消息重发
        **/
    }
});
//进行消息发送
for(int i=0;i<5;i++){
    long nextSeqNo = channel.getNextPublishSeqNo();//获取消息id
    channel.basicPublish("", QUEUE_NAME, MessageProperties.PERSISTENT_BASIC, (" Confirm模式， 第" + (i + 1) + "条消息").getBytes());
    confirmSet.add(nextSeqNo);
}
```

维持异步调用要求我们**不能断掉连接**，不能关掉连接还有通道。

##### 4、关于Spring Boot使用Producer的Confirm模式

主要是通过在Sender中实现RabbitTemplate.ConfirmCallback接口来实现该操作。可以参考rabbitmq-demo中的 CallBackSender.java 的实现。

CallBackSender.java

~~~java

package net.anumbrella.rabbitmq.sender;

import java.util.UUID;

import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.amqp.rabbit.support.CorrelationData;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class CallBackSender implements RabbitTemplate.ConfirmCallback {

	@Autowired
	private RabbitTemplate rabbitTemplate;

	public void send() {

		rabbitTemplate.setConfirmCallback(this);
		String msg = "callbackSender : i am callback sender";
		System.out.println(msg);
		CorrelationData correlationData = new CorrelationData(UUID.randomUUID().toString());
		System.out.println("callbackSender UUID: " + correlationData.getId());
		this.rabbitTemplate.convertAndSend("", "hello", msg, correlationData);
	}

	public void confirm(CorrelationData correlationData, boolean ack, String cause) {
		// 这里的ack是Broker对发布者消息达到服务端的确认
		System.out.println("callbakck confirm: " + correlationData.getId() + " ACK : " + ack + " cause : "+ cause);
	}
}
~~~

#### 2、消费者(Consumer)的Confirm模式

##### 手动确认和自动确认

为了保证消息从队列可靠地到达消费者，RabbitMQ提供消息确认机制(message acknowledgment)。消费者在声明队列时，可以指定noAck参数，当noAck=false时，RabbitMQ会等待消费者显式发回ack信号后才从内存(和磁盘，如果是持久化消息的话)中移去消息。否则，RabbitMQ会在队列中消息被消费后立即删除它。

采用消息确认机制后，只要令noAck=false，消费者就有足够的时间处理消息(任务)，不用担心处理消息过程中消费者进程挂掉后消息丢失的问题，因为RabbitMQ会一直持有消息直到消费者显式调用basicAck为止。

在Consumer中Confirm模式中分为手动确认和自动确认。

手动确认主要并使用以下方法：

basic.ack: 用于肯定确认，multiple参数用于多个消息确认。
basic.recover：是路由不成功的消息可以使用recovery重新发送到队列中。
basic.reject：是接收端告诉服务器这个消息我拒绝接收,不处理,可以设置是否放回到队列中还是丢掉，而且只能一次拒绝一个消息,官网中有明确说明不能批量拒绝消息，为解决批量拒绝消息才有了basicNack。
basic.nack：可以一次拒绝N条消息，客户端可以设置basicNack方法的multiple参数为true，服务器会拒绝指定了delivery_tag的所有未确认的消息(tag是一个64位的long值，最大值是9223372036854775807)。

肯定的确认只是指导RabbitMQ将一个消息记录为已投递。basic.reject的否定确认具有相同的效果。 两者的差别在于：肯定的确认假设一个消息已经成功处理，而对立面则表示投递没有被处理，但仍然应该被删除。

同样的Consumer中的Confirm模式也具有同时确认多个投递，通过将确认方法的 multiple “字段设置为true完成的，实现的意义与Producer的一致。

在自动确认模式下，消息在发送后立即被认为是发送成功。 这种模式可以提高吞吐量（只要消费者能够跟上），不过会降低投递和消费者处理的安全性。 这种模式通常被称为“发后即忘”。 与手动确认模式不同，如果消费者的TCP连接或信道在成功投递之前关闭，该消息则会丢失。

使用自动确认模式时需要考虑的另一件事是消费者过载。 手动确认模式通常与有限的信道预取一起使用，限制信道上未完成（“进行中”）传送的数量。 然而，对于自动确认，根据定义没有这样的限制。 因此，消费者可能会被交付速度所压倒，可能积压在内存中，堆积如山，或者被操作系统终止。 某些客户端库将应用TCP反压（直到未处理的交付积压下降超过一定的限制时才停止从套接字读取）。 因此，只建议当消费者可以有效且稳定地处理投递时才使用自动投递方式。

主要实现代码：

```java
// 手动确认消息
channel.basicAck(envelope.getDeliveryTag(), false);

// 关闭自动确认
boolean autoAck = false;
channel.basicConsume(QUEUE_NAME, autoAck, consumer);
```

##### 关于Spring Boot使用Consumer的Confirm模式

CheckReceiver.java

```java
package net.anumbrella.rabbitmq.receiver;

import java.io.IOException;

import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

import com.rabbitmq.client.Channel;

@Component
public class CheckReceiver {

   @RabbitListener(queues = "hello")
   public void process(Message message, Channel channel) throws IOException {
      System.out.println("CheckReceiver: " + new String(message.getBody()));
      try {
         doWork();
      } catch (InterruptedException e) {
         e.printStackTrace();
      }
      // 使用时需要在application.properties开启手动确认设置
      // 消息的标识，false只确认当前一个消息收到，true确认所有将比第一个参数指定的 delivery tag 小的consumer都获得的消息
      channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
   }

   private static void doWork() throws InterruptedException {
      Thread.sleep(1000);
   }
}
```

---

## 7. MQ的应用场景

### 7.1 异步处理

`场景说明：用户注册后，需要发注册邮件和注册短信,传统的做法有两种 1.串行的方式 2.并行的方式`

- `串行方式:` 将注册信息写入数据库后,发送注册邮件,再发送注册短信,以上三个任务全部完成后才返回给客户端。 这有一个问题是,邮件,短信并不是必须的,它只是一个通知,而这种做法让客户端等待没有必要等待的东西. 

 ![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201020092233.png)

- `并行方式: `将注册信息写入数据库后,发送邮件的同时,发送短信,以上三个任务完成后,返回给客户端,并行的方式能提高处理的时间。 

 ![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201020092234.png)

- `消息队列:`假设三个业务节点分别使用50ms,串行方式使用时间150ms,并行使用时间100ms。虽然并行已经提高的处理时间,但是,前面说过,邮件和短信对我正常的使用网站没有任何影响，客户端没有必要等着其发送完成才显示注册成功,应该是写入数据库后就返回.  `消息队列`: 引入消息队列后，把发送邮件,短信不是必须的业务逻辑异步处理 

  ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020092235.jpg)

由此可以看出,引入消息队列后，用户的响应时间就等于写入数据库的时间+写入消息队列的时间(可以忽略不计),引入消息队列后处理后,响应时间是串行的3倍,是并行的2倍。



### 7.2 应用解耦

`场景：双11是购物狂节,用户下单后,订单系统需要通知库存系统,传统的做法就是订单系统调用库存系统的接口. `

 ![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201020092236.png)

这种做法有一个缺点:

当库存系统出现故障时,订单就会失败。 订单系统和库存系统高耦合.  引入消息队列 

 ![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201020092237.png)

- `订单系统:`用户下单后,订单系统完成持久化处理,将消息写入消息队列,返回用户订单下单成功。

- `库存系统:`订阅下单的消息,获取下单消息,进行库操作。  就算库存系统出现故障,消息队列也能保证消息的可靠投递,不会导致消息丢失.

  

### 7.3 流量削峰

 `场景:` 秒杀活动，一般会因为流量过大，导致应用挂掉,为了解决这个问题，一般在应用前端加入消息队列。  

  `作用:` 

​			1.可以控制活动人数，超过此一定阀值的订单直接丢弃(我为什么秒杀一次都没有成功过呢^^) 

​			2.可以缓解短时间的高流量压垮应用(应用程序按自己的最大处理能力获取订单) 

 ![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201020092238.png)

1.用户的请求,服务器收到之后,首先写入消息队列,加入消息队列长度超过最大值,则直接抛弃用户请求或跳转到错误页面.  

2.秒杀业务根据消息队列中的请求信息，再做后续处理.

-----

## 8. RabbitMQ的集群

### 8.1 普通集群(副本集群)

> All data/state required for the operation of a RabbitMQ broker is replicated across all nodes. An exception to this are message queues, which by default reside on one node, though they are visible and reachable from all nodes. To replicate queues across nodes in a cluster   --摘自官网

`默认情况下:RabbitMQ代理操作所需的所有数据/状态都将跨所有节点复制。这方面的一个例外是消息队列，默认情况下，消息队列位于一个节点上，尽管它们可以从所有节点看到和访问`

1. 架构图

![image-20200320094147471](https://gitee.com/xudongyin/img/raw/master/img/20201020092239.png)

​	核心解决问题:  `当集群中某一时刻master节点宕机,可以对Quene中元数据信息进行备份，不会同步存储的消息，消息只会存在于创建该队列的master节点上，其它节点只知道这个队列的元数据信息和一个指向队列的owner node的地址`

2. 集群搭建

   ```markdown
   # 0.集群规划
   	node1: 10.15.0.3  mq1  master 主节点
   	node2: 10.15.0.4  mq2  repl1  副本节点
   	node3: 10.15.0.5  mq3  repl2  副本节点
   
   # 1.克隆三台机器主机名和ip映射
   	vim /etc/hosts加入:
   		 10.15.0.3 mq1
       	10.15.0.4 mq2
       	10.15.0.5 mq3
   	node1: vim /etc/hostname 加入:  mq1
   	node2: vim /etc/hostname 加入:  mq2
   	node3: vim /etc/hostname 加入:  mq3
   
   # 2.三个机器安装rabbitmq,并同步cookie文件,在node1上执行:
   	scp /var/lib/rabbitmq/.erlang.cookie root@mq2:/var/lib/rabbitmq/
   	scp /var/lib/rabbitmq/.erlang.cookie root@mq3:/var/lib/rabbitmq/
   
   # 3.查看cookie是否一致:
   	node1: cat /var/lib/rabbitmq/.erlang.cookie 
   	node2: cat /var/lib/rabbitmq/.erlang.cookie 
   	node3: cat /var/lib/rabbitmq/.erlang.cookie 
   
   # 4.后台启动rabbitmq所有节点执行如下命令,启动成功访问管理界面:
   	rabbitmq-server -detached 
   
   # 5.在node2和node3执行加入集群命令:
   	1.关闭       rabbitmqctl stop_app
   	2.加入集群    rabbitmqctl join_cluster rabbit@mq1
   	3.启动服务    rabbitmqctl start_app
   
   # 6.查看集群状态,任意节点执行:
   	rabbitmqctl cluster_status
   
   # 7.如果出现如下显示,集群搭建成功:
   	Cluster status of node rabbit@mq3 ...
   	[{nodes,[{disc,[rabbit@mq1,rabbit@mq2,rabbit@mq3]}]},
   	{running_nodes,[rabbit@mq1,rabbit@mq2,rabbit@mq3]},
   	{cluster_name,<<"rabbit@mq1">>},
   	{partitions,[]},
   	{alarms,[{rabbit@mq1,[]},{rabbit@mq2,[]},{rabbit@mq3,[]}]}]
   
   # 8.登录管理界面,展示如下状态:
   ```

   ![image-20200320095613586](https://gitee.com/xudongyin/img/raw/master/img/20201020092240.png)

   ```markdown
   # 9.测试集群在node1上,创建队列
   ```

   ![image-20200320095743935](https://gitee.com/xudongyin/img/raw/master/img/20201020092241.png)

   ```markdown
   # 10.查看node2和node3节点:
   ```

   ![image-20200320095827688](https://gitee.com/xudongyin/img/raw/master/img/20201020092242.png)

   ![image-20200320095843370](https://gitee.com/xudongyin/img/raw/master/img/20201020092243.png)

   ```markdown
   # 11.关闭node1节点,执行如下命令,查看node2和node3:
   	rabbitmqctl stop_app
   ```

   ![image-20200320100000347](https://gitee.com/xudongyin/img/raw/master/img/20201020092244.png)

   ![image-20200320100010968](https://gitee.com/xudongyin/img/raw/master/img/20201020092245.png)

   ---

### 8.2 镜像集群

> This guide covers mirroring (queue contents replication) of classic queues  --摘自官网
>
> By default, contents of a queue within a RabbitMQ cluster are located on a single node (the node on which the queue was declared). This is in contrast to exchanges and bindings, which can always be considered to be on all nodes. Queues can optionally be made *mirrored* across multiple nodes. --摘自官网

`镜像队列机制就是将队列在三个节点之间设置主从关系，消息会在三个节点之间进行自动同步，且如果其中一个节点不可用，并不会导致消息丢失或服务不可用的情况，提升MQ集群的整体高可用性。`

1. 集群架构图

   ![image-20200320113423235](https://gitee.com/xudongyin/img/raw/master/img/20201020092246.png)

   

2. 配置集群架构

   ```markdown
   # 0.策略说明
   	rabbitmqctl set_policy [-p <vhost>] [--priority <priority>] [--apply-to <apply-to>] <name>  <pattern>  <definition>
   	-p Vhost： 可选参数，针对指定vhost下的queue进行设置
   	Name:     policy的名称
   	Pattern: queue的匹配模式(正则表达式)
   	Definition：镜像定义，包括三个部分ha-mode, ha-params, ha-sync-mode
              		ha-mode:指明镜像队列的模式，有效值为 all/exactly/nodes
                           all：表示在集群中所有的节点上进行镜像
                           exactly：表示在指定个数的节点上进行镜像，节点的个数由ha-params指定
                           nodes：表示在指定的节点上进行镜像，节点名称通过ha-params指定
               	 ha-params：ha-mode模式需要用到的参数
                   ha-sync-mode：进行队列中消息的同步方式，有效值为automatic和manual
                   priority：可选参数，policy的优先级
                   
                    
   # 1.查看当前策略
   	rabbitmqctl list_policies
   
   # 2.添加策略
   	rabbitmqctl set_policy ha-all '^hello' '{"ha-mode":"all","ha-sync-mode":"automatic"}' 
   	说明:策略正则表达式为 “^” 表示所有匹配所有队列名称  ^hello:匹配hello开头队列
   
   # 3.删除策略
   	rabbitmqctl clear_policy ha-all
   
   # 4.测试集
   ```

## 9. RabbitMQ 应用与面试

### 9.1 消息堆积

当消息生产的速度长时间，远远大于消费的速度时。就会造成消息堆积。

![消息堆积](https://gitee.com/xudongyin/img/raw/master/img/20201031165331.jpg)

- 消息堆积的影响
  - 可能导致新消息无法进入队列
  - 可能导致旧消息无法丢失
  - 消息等待消费的时间过长，超出了业务容忍范围。
- 产生堆积的原因
  - 生产者突然大量发布消息
  - 消费者消费失败
  - 消费者出现性能瓶颈。
  - 消费者挂掉
- 解决办法
  - 排查消费者的消费性能瓶颈
  - 增加消费者的多线程处理
  - 部署增加多个消费者
- 场景介绍

在用户登录成功之后，会向rabbitmq发送一个登录成功的消息。这个消息可以被多类业务订阅。

登录成功，记录登录日志；登录成功，根据规则送积分。其中登录送积分可以模拟成较为耗时的处理

**场景重现**：让消息产生堆积

1. 生产者大量发送消息：使用Jmeter开启多线程，循环发送消息大量进入队列。

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201031195452.jpg)

   模拟堆积10万条数据

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201031195506.jpg)

2. 消费者消费失败：随机抛出异常，模拟消费者消费失败，没有ack（手动ack的时候）。

3. 设置消费者的性能瓶颈：在消费方法中设置休眠时间，模拟性能瓶颈

4. 关闭消费者：停掉消费者，模拟消费者挂掉

5. 消费者端示例核心代码：

```java
public class LoginIntegralComsumer implements MessageListener {
    public void onMessage(Message message) {
        String jsonString = null;
        try {
            jsonString = new String(message.getBody(),"UTF8");
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        if(new Random().nextInt(5)==2){
            //模拟发生异常
            throw new RuntimeException("模拟处理异常");
        }
        try {
            //模拟耗时的处理过程
            TimeUnit.MILLISECONDS.sleep(1000);
            System.out.println(Thread.currentThread().getName()+"处理消息:"+jsonString);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

如果每1秒钟处理一条消息，1小时处理 60*60=3600条，处理完10万条数据 100000/3600=27.7小时  

**问题解决**：消息已经堆积如何解决

 消息队列堆积，想办法把消息转移到一个新的队列，可以增加服务器慢慢来消费这个消息

 生产环境的队列可用状态

1、解决消费者消费异常问题

2、解决消费者的性能瓶颈：改短休眠时间

```
5.4小时。
```

3、增加消费线程，增加多台服务器部署消费者，快速消费。

 增加10个线程，1小时  

```
concurrency="10" prefetch="10"
```

增加一台服务器，0.5小时

### 9.2 消息丢失

在实际的生产环境中有可能出现一条消息因为一些原因丢失，导致消息没有消费成功，从而造成数据不一致等问题，造成严重的影响，比如：在一个商城的下单业务中，需要生成订单信息和扣减库存两个动作，如果使用RabbitMQ来实现该业务，那么在订单服务下单成功后需要发送一条消息到库存服务进行扣减库存，如果在此过程中，一条消息因为某些原因丢失，那么就会出现下单成功但是库存没有扣减，从而导致超卖的情况，也就是库存已经没有了，但是用户还能下单，这个问题对于商城系统来说是致命的。  

消息丢失的场景主要分为：消息在生产者丢失，消息在RabbitMQ丢失，消息在消费者丢失。

#### 9.2.1 消息在生产者丢失

> 场景介绍

​	消息生产者发送消息成功，但是MQ没有收到该消息，消息在从生产者传输到MQ的过程中丢失，一般是由于网络不稳定的原因。

> 解决方案

​	采用RabbitMQ 发送方消息确认机制，当消息成功被MQ接收到时，会给生产者发送一个确认消息，表示接收成功。RabbitMQ 发送方消息确认模式有以下三种：普通确认模式，批量确认模式，异步监听确认模式。spring整合RabbitMQ后只使用了异步监听确认模式。  

> 说明

​	异步监听模式，可以实现边发送消息边进行确认，不影响主线程任务执行。

**步骤**

1. 生产者发送3000条消息

2. 在发送消息前开启开启发送方确认模式

   ```xml
       <rabbit:connection-factory id="connectionFactory" host="${rabbitmq.host}"
                                  port="${rabbitmq.port}"
                                  username="${rabbitmq.username}"
                                  password="${rabbitmq.password}"
                                  virtual-host="${rabbitmq.virtual-host}"
                                  publisher-confirms="true"
        />
   ```

3. 在发送消息前添加异步确认监听器

   ```java
   //添加异步确认监听器
   rabbitTemplate.setConfirmCallback(new RabbitTemplate.ConfirmCallback() {
       public void confirm(CorrelationData correlationData, boolean ack, String cause) {
           if (ack) {
               // 处理ack
               System.out.println("已确认消息，标识：" + correlationData.getId());
           } else {
               // 处理nack, 此时cause包含nack的原因。
               System.out.println("未确认消息，标识：" + correlationData.getId());
               System.out.println("未确认原因：" + cause);
               //重发
           }
       }
   });
   ```

#### 9.2.2 消息在RabbitMQ丢失

> 场景介绍

​	消息成功发送到MQ，消息还没被消费却在MQ中丢失，比如MQ服务器宕机或者重启会出现这种情况

> 解决方案

​	持久化交换机，队列，消息，确保MQ服务器重启时依然能从磁盘恢复对应的交换机，队列和消息。

spring整合后默认开启了交换机，队列，消息的持久化，所以不修改任何设置就可以保证消息不在RabbitMQ丢失。但是为了以防万一，还是可以申明下。

#### 9.2.3 消息在消费者丢失

> 场景介绍

​	   消息费者消费消息时，如果设置为自动回复MQ，消息者端收到消息后会自动回复MQ服务器，MQ则会删除该条消息，如果消息已经在MQ被删除但是消费者的业务处理出现异常或者消费者服务宕机，那么就会导致该消息没有处理成功从而导致该条消息丢失。  

> 解决方案

​	   设置为手动回复MQ服务器，当消费者出现异常或者服务宕机时，MQ服务器不会删除该消息，而是会把消息重发给绑定该队列的消费者，如果该队列只绑定了一个消费者，那么该消息会一直保存在MQ服务器，直到消息者能正常消费为止。本解决方案以一个队列绑定多个消费者为例来说明，一般在生产环境上也会让一个队列绑定多个消费者也就是工作队列模式来减轻压力，提高消息处理效率  

​	MQ重发消息场景：

​	1.消费者未响应ACK，主动关闭频道或者连接

​	2.消费者未响应ACK，消费者服务挂掉

### 9.3 有序消费消息

> 场景介绍

**场景1**

当RabbitMQ采用work Queue模式，此时只会有一个Queue但是会有多个Consumer,同时多个Consumer直接是竞争关系，此时就会出现MQ消息乱序的问题。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201031202345.jpg)

解决方案

![img](https://gitee.com/xudongyin/img/raw/master/img/20201031202643.jpg)

**场景2**

当RabbitMQ采用简单队列模式的时候,如果消费者采用多线程的方式来加速消息的处理,此时也会出现消息乱序的问题。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201031202403.jpg)

解决方案

![img](https://gitee.com/xudongyin/img/raw/master/img/20201031202804.jpg)

### 9.4 重复消费

> 场景介绍

​	为了防止消息在消费者端丢失，会采用手动回复MQ的方式来解决，同时也引出了一个问题，消费者处理消息成功，手动回复MQ时由于网络不稳定，连接断开，导致MQ没有收到消费者回复的消息，那么该条消息还会保存在MQ的消息队列，由于MQ的消息重发机制，会重新把该条消息发给和该队列绑定的消息者处理，这样就会导致消息重复消费。而有些操作是不允许重复消费的，比如下单，减库存，扣款等操作。  

​	MQ重发消息场景：

​	1.消费者未响应ACK，主动关闭频道或者连接

​	2.消费者未响应ACK，消费者服务挂掉

> 解决方案

​	如果消费消息的业务是幂等性操作（同一个操作执行多次，结果不变）就算重复消费也没问题，可以不做处理，如果不支持幂等性操作，如：下单，减库存，扣款等，那么可以在消费者端每次消费成功后将该条消息id保存到数据库，每次消费前查询该消息id，如果该条消息id已经存在那么表示已经消费过就不再消费否则就消费。本方案采用redis存储消息id，因为redis是单线程的，并且性能也非常好，提供了很多原子性的命令，本方案使用setnx命令存储消息id。  

> setnx(key,value):如果key不存在则插入成功且返回1,如果key存在,则不进行任何操作,返回0

 