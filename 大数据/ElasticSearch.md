## ElasticSearch概述

**Lucene 和 ElasticSearch 关系：**

ElasticSearch 是基于 Lucene 做了一些封装和增强

**Elaticsearch**，简称为es， es是一个开源的**高扩展的分布式全文检索引擎**，它可以近乎**实时的存储、检索数据**；本身扩展性很好，可以扩展到上百台服务器，处理PB级别（大数据时代）的数据。es也使用 Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的**RESTful API** 来隐藏Lucene的复杂性，从而让全文搜索变得简单。

**使用**

1. 维基百科，类似百度百科，全文检索，高亮，搜索推荐/2 （权重，百度！）
2. The Guardian（国外新闻网站），类似搜狐新闻，用户行为日志（点击，浏览，收藏，评论）+社交网络数据（对某某新闻的相关看法），数据分析，给到每篇新闻文章的作者，让他知道他的文章的公众反馈（好，坏，热门，垃圾，鄙视，崇拜）
3. Stack Overflow（国外的程序异常讨论论坛），IT问题，程序的报错，提交上去，有人会跟你讨论和 回答，全文检索，搜索相关问题和答案，程序报错了，就会将报错信息粘贴到里面去，搜索有没有对应的答案
4. GitHub（开源代码管理），搜索上千亿行代码
5. 电商网站，检索商品
6. 日志数据分析，logstash采集日志，ES进行复杂的数据分析，**ELK技术（ elasticsearch+logstash+kibana）**
7. 商品价格监控网站，用户设定某商品的价格阈值，当低于该阈值的时候，发送通知消息给用户，比如 说订阅牙膏的监控，如果高露洁牙膏的家庭套装低于50块钱，就通知我，我就去买。
8. BI系统，商业智能，Business Intelligence。比如说有个大型商场集团，BI，分析一下某某区域最近 3年的用户消费金额的趋势以及用户群体的组成构成，产出相关的数张报表，**区，最近3年，每年消费 金额呈现100%的增长，而且用户群体85%是高级白领，开一个新商场。ES执行数据分析和挖掘， Kibana进行数据可视化
9. 国内：站内搜索（电商，招聘，门户，等等），IT系统搜索（OA，CRM，ERP，等等），数据分析 （ES热门 的一个使用场景）

## ES和Solr的差别

###  Elasticsearch简介

Elasticsearch是一个实时分布式搜索和分析引擎。它让你以前所未有的速度处理大数据成为可能。
它用于**全文搜索、结构化搜索、分析**以及将这三者混合使用：
维基百科使用Elasticsearch提供全文搜索并高亮关键字，以及输入实时搜索(search-asyou-type)和搜索纠错(did-you-mean)等搜索建议功能。
英国卫报使用Elasticsearch结合用户日志和社交网络数据提供给他们的编辑以实时的反馈，以便及时了解公众对新发表的文章的回应。
StackOverflow结合全文搜索与地理位置查询，以及more-like-this功能来找到相关的问题和答案。
Github使用Elasticsearch检索1300亿行的代码。

但是Elasticsearch不仅用于大型企业，它还让像DataDog以及Klout这样的创业公司将最初的想法变成可扩展的解决方案。
Elasticsearch可以在你的笔记本上运行，也可以在数以百计的服务器上处理PB级别的数据 。

Elasticsearch是一个**基于Apache Lucene(TM)的开源搜索引擎**。无论在开源还是专有领域，Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。

但是，Lucene只是一个库。想要使用它，你必须使用Java来作为开发语言并将其直接集成到你的应用中，更糟糕的是，Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。

Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的**RESTful API**来隐藏Lucene的复杂性，从而让全文搜索变得简单。

### Solr简介

Solr 是Apache下的一个顶级开源项目，采用Java开发，它是基于Lucene的全文搜索服务器。Solr提供了比Lucene更为丰富的查询语言，同时实现了可配置、可扩展，并对索引、搜索性能进行了优化

Solr可以独立运行，运行在Jetty、Tomcat等这些Servlet容器中，Solr 索引的实现方法很简单，**用 POST方法向 Solr 服务器发送一个描述 Field 及其内容的 XML 文档，Solr根据xml文档添加、删除、更新索引**。Solr 搜索只需要发送 HTTP GET 请求，然后对 Solr 返回Xml、json等格式的查询结果进行解析，组织页面布局。Solr不提供构建UI的功能，Solr提供了一个管理界面，通过管理界面可以查询Solr的配置和运行情况。

solr是基于lucene开发企业级搜索服务器，实际上就是封装了lucene。

Solr是一个独立的企业级搜索应用服务器，它对外提供类似于Web-service的API接口。用户可以通过http请求，向搜索引擎服务器提交一定格式的文件，生成索引；也可以通过提出查找请求，并得到返回结果。

### Lucene简介

Lucene是apache软件基金会4 jakarta项目组的一个子项目，是一个开放源代码的全文检索引擎工具 包，但它不是一个完整的全文检索引擎，而是一个全文检索引擎的架构，提供了完整的查询引擎和索引 引擎，部分文本分析引擎（英文与德文两种西方语言）。Lucene的目的是为软件开发人员提供一个简单 易用的工具包，以方便的在目标系统中实现全文检索的功能，或者是以此为基础建立起完整的全文检索 引擎。Lucene是一套用于全文检索和搜寻的开源程式库，由Apache软件基金会支持和提供。Lucene提 供了一个简单却强大的应用程式接口，能够做全文索引和搜寻。**在Java开发环境里Lucene是一个成熟的免费开源工具。就其本身而言，Lucene是当前以及最近几年最受欢迎的免费Java信息检索程序库**。人们 经常提到信息检索程序库，虽然与搜索引擎有关，但不应该将信息检索程序库与搜索引擎相混淆。

Lucene是一个全文检索引擎的架构。那什么是全文搜索引擎？ 全文搜索引擎是名副其实的搜索引擎，国外具代表性的有Google、Fast/AllTheWeb、AltaVista、 Inktomi、Teoma、WiseNut等，国内著名的有百度（Baidu）。它们都是通过从互联网上提取的各个网 站的信息（以网页文字为主）而建立的数据库中，检索与用户查询条件匹配的相关记录，然后按一定的 排列顺序将结果返回给用户，因此他们是真正的搜索引擎。

从搜索结果来源的角度，全文搜索引擎又可细分为两种，一种是拥有自己的检索程序（Indexer），俗称 “蜘蛛”（Spider）程序或“机器人”（Robot）程序，并自建网页数据库，搜索结果直接从自身的数据库中 调用，如上面提到的7家引擎；另一种则是租用其他引擎的数据库，并按自定的格式排列搜索结果，如 Lycos引擎。

### Elasticsearch和Solr比较

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020111532)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020111538)

![img](https://upload-images.jianshu.io/upload_images/18792147-57cda1edbe53165d.png?imageMogr2/auto-orient/strip|imageView2/2/w/802/format/webp)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020111541)

**比较总结**

1、es基本是开箱即用（解压就可以用 ! ），非常简单。Solr安装略微复杂一丢丢！

2、Solr 利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能。

3、Solr 支持更多格式的数据，比如JSON、XML、CSV，而 Elasticsearch **仅支持 json文件格式**。

4、Solr 官方提供的功能更多，而 Elasticsearch 本身更注重于核心功能，高级功能多有第三方插件提 供，例如图形化界面需要kibana友好支撑~!

5、Solr 查询快，但更新索引时慢（即插入删除慢），用于电商等查询多的应用；

- ES建立索引快（即查询慢），即实时性查询快，用于facebook新浪等搜索。
- Solr 是传统搜索应用的有力解决方案，但 Elasticsearch 更适用于新兴的实时搜索应用。

6、Solr比较成熟，有一个更大，更成熟的用户、开发和贡献者社区，而 Elasticsearch相对开发维护者较少，更新太快，学习使用成本较高。（趋势！）

## ElasticSearch安装

声明：JDK1.8 ，最低要求！ ElasticSearch 客户端，界面工具！
 Java开发，ElasticSearch 的版本和我们之后对应的 Java 的核心jar包！ 版本对应！JDK 环境是正常！

> 下载

官网：[https://www.elastic.co/](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.elastic.co%2F)

下载地址：[https://www.elastic.co/cn/downloads/elasticsearch](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.elastic.co%2Fcn%2Fdownloads%2Felasticsearch)

> window 下安装！

 解压就可以使用了！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220810)

 熟悉目录！

```java
bin 启动文件
config 配置文件
    log4j2 日志配置文件
    jvm.options java 虚拟机相关的配置
    elasticsearch.yml  elasticsearch 的配置文件！ 默认 9200 端口！ 跨域！
lib   相关jar包
logs   日志！
modules 功能模块
plugins 插件！
```

 启动，访问9200；

启动不起来在elasticsearch.yml末尾添加

```yaml
xpack.ml.enabled: false
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220827)

 访问测试！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220837)

> 安装可视化界面 es head的插件

需要node.js环境

1. 下载地址：[https://github.com/mobz/elasticsearch-head/](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fmobz%2Felasticsearch-head%2F)

2. 启动

   ```bash
   npm install
   npm run start
   ```

3. 连接测试发现，存在跨域问题：配置 es 的 yml 配置文件

   ```yaml
   http.cors.enabled: true
   http.cors.allow-origin: "*"
   ```

4. 重启es服务器，然后再次连接

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220850)

初学，就把es当做一个数据库！ （可以建立索引（库），文档（库中的数据！））

这个head我们就把它当做数据展示工具！我们后面所有的查询，Kibana！

> 了解 ELK

ELK是Elasticsearch、Logstash、Kibana三大开源框架首字母大写简称。市面上也被成为ElasticStack。其中Elasticsearch是一个基于Lucene、分布式、通过Restful方式进行交互的近实时搜索平台框架。像类似百度、谷歌这种大数据全文搜索引擎的场景都可以使用Elasticsearch作为底层支持框架，可见Elasticsearch提供的搜索能力确实强大,市面上很多时候我们简称Elasticsearch为 es。Logstash是ELK的中央数据流引擎，用于从不同目标（文件/数据存储/MQ）收集的不同格式数据，经过过滤后支持输出到不同目的地（文件/MQ/redis/elasticsearch/kafka等）。Kibana可以将Elasticsearch 的数据通过友好的页面展示出来，提供实时分析的功能。

市面上很多开发只要提到ELK能够一致说出它是一个日志分析架构技术栈总称，但实际上ELK不仅仅适用于日志分析，它还可以支持其它任何数据分析和收集的场景，日志分析和收集只是更具有代表性。并非唯一性。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220859)

> 安装Kibana

Kibana是一个针对Elasticsearch的开源分析及可视化平台，用来搜索、查看交互存储在Elasticsearch索引中的数据。使用Kibana，可以通过各种图表进行高级数据分析及展示。Kibana让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速创建仪表板（dashboard）实时显示Elasticsearch查询动态。设置Kibana非常简单。无需编码或者额外的基础架构，几分钟内就可以完成Kibana安装并启动Elasticsearch索引监测

官网：[https://www.elastic.co/cn/kibana](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.elastic.co%2Fcn%2Fkibana)

Kibana 版本要和 Es 一致！

> 启动测试：

1. 解压后端的目录

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220920)

1. 启动

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220927)

1. 访问测试

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220936)

1. 开发工具！ （Post、curl、head、谷歌浏览器插件测试！）

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020220944)

1. 汉化！自己修改kibana配置即可！ zh-CN！

   kibaba.yml文件末尾添加

   ```yaml
   i18n.locale: "zn-CH"
   ```

## ES核心概念

1. 索引
2. 字段类型（mapping）
3. 文档（documents）

> 概述

在前面的学习中，我们已经掌握了es是什么，同时也把es的服务已经安装启动，那么es是如何去存储数据，数据结构是什么，又是如何实现搜索的呢？我们先来聊聊ElasticSearch的相关概念吧！

**集群，节点，索引，类型，文档，分片，映射是什么？**

> elasticsearch是面向文档，关系型数据库 和 elasticsearch 客观的对比！一切都是JSON！

|  Relational DB   | Elasticsearch |
| :--------------: | :-----------: |
| 数据库(database) | 索引(indices) |
|    表(tables)    |     types     |
|     行(rows)     |   documents   |
|  字段(columns)   |    fields     |

elasticsearch(集群)中可以包含多个索引(数据库)，每个索引中可以包含多个类型(表)，每个类型下又包含多个文档(行)，每个文档中又包含多个字段(列)。

**物理设计：**

elasticsearch 在后台把每个索引划分成多个分片，每个分片可以在集群中的不同服务器间迁移

一个人就是一个集群！默认的集群名称就是 elaticsearh

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221001)

**逻辑设计：**

一个索引类型中，包含多个文档，比如说文档1，文档2。 当我们索引一篇文档时，可以通过这样的顺序找到 它: 索引 ▷ 类型 ▷ 文档ID ，通过这个组合我们就能索引到某个具体的文档。 注意:ID不必是整数，实际上它是个字符串。

> 文档

就是我们的一条条数据

```undefined
user
1  zhangsan  18
2  kuangshen  3
```

之前说elasticsearch是面向文档的，那么就意味着索引和搜索数据的最小单位是文档，elasticsearch中，文档有几个 重要属性 :

- 自我包含，一篇文档同时包含字段和对应的值，也就是同时包含 key:value！
- 可以是层次型的，一个文档中包含自文档，复杂的逻辑实体就是这么来的！ {就是一个json对象！fastjson进行自动转换！}
- 灵活的结构，文档不依赖预先定义的模式，我们知道关系型数据库中，要提前定义字段才能使用，在elasticsearch中，对于字段是非常灵活的，有时候，我们可以忽略该字段，或者动态的添加一个新的字段。

尽管我们可以随意的新增或者忽略某个字段，但是，每个字段的类型非常重要，比如一个年龄字段类型，可以是字符串也可以是整形。因为elasticsearch会保存字段和类型之间的映射及其他的设置。这种映射具体到每个映射的每种类型，这也是为什么在elasticsearch中，类型有时候也称为映射类型。

> 类型

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221013)

类型是文档的逻辑容器，就像关系型数据库一样，表格是行的容器。 类型中对于字段的定义称为映射，比如 name 映射为字符串类型。 我们说文档是无模式的，它们不需要拥有映射中所定义的所有字段，比如新增一个字段，那么elasticsearch是怎么做的呢?elasticsearch会自动的将新字段加入映射，但是这个字段不确定它是什么类型，elasticsearch就开始猜，如果这个值是18，那么elasticsearch会认为它是整形。 但是elasticsearch也可能猜不对， 所以最安全的方式就是提前定义好所需要的映射，这点跟关系型数据库殊途同归了，先定义好字段，然后再使用，别整什么幺蛾子。

> 索引

就是数据库！

索引是映射类型的容器，elasticsearch中的索引是一个非常大的文档集合。索引存储了映射类型的字段和其他设置。 然后它们被存储到了各个分片上了。 我们来研究下分片是如何工作的。

物理设计 ：节点和分片 如何工作

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221023)

一个集群至少有一个节点，而一个节点就是一个elasricsearch进程，节点可以有多个索引默认的，如果你创建索引，那么索引将会有个5个分片 ( primary shard ,又称主分片 ) 构成的，每一个主分片会有一个副本 ( replica shard ,又称复制分片 )

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221032)

上图是一个有3个节点的集群，可以看到主分片和对应的复制分片都不会在同一个节点内，这样有利于某个节点挂掉 了，数据也不至于丢失。 实际上，一个分片是一个Lucene索引，一个包含倒排索引的文件目录，**倒排索引**的结构使得elasticsearch在不扫描全部文档的情况下，就能告诉你哪些文档包含特定的关键字。 不过倒排索引是什么?

> 倒排索引

elasticsearch使用的是一种称为倒排索引的结构，采用Lucene倒排索作为底层。这种结构适用于快速的全文搜索， 一个索引由文档中所有不重复的列表构成，对于每一个词，都有一个包含它的文档列表。 例如，现在有两个文档， 每个文档包含如下内容：

```bash
Study every day, good good up to forever  # 文档1包含的内容
To forever, study every day, good good up # 文档2包含的内容
```

为了创建倒排索引，我们首先要将每个文档拆分成独立的词(或称为词条或者tokens)，然后创建一个包含所有不重复的词条的排序列表，然后列出每个词条出现在哪个文档 :

|  term   | doc_1 | doc_2 |
| :-----: | :---: | :---: |
|  Study  |   √   |   x   |
|   To    |   x   |   x   |
|  every  |   √   |   √   |
| forever |   √   |   √   |
|   day   |   √   |   √   |
|  study  |   x   |   √   |
|  good   |   √   |   √   |
|  every  |   √   |   √   |
|   to    |   √   |   x   |
|   up    |   √   |   √   |

现在，我们试图搜索 to forever，只需要查看包含每个词条的文档 score

|  term   | doc_1 | doc_2 |
| :-----: | :---: | :---: |
|   to    |   √   |   ×   |
| forever |   √   |   √   |
|  total  |   2   |   1   |

两个文档都匹配，但是第一个文档比第二个匹配程度更高。如果没有别的条件，现在，这两个包含关键字的文档都将返回。

再来看一个示例，比如我们通过博客标签来搜索博客文章。那么倒排索引列表就是这样的一个结构 :

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221043)

如果要搜索含有 python 标签的文章，那相对于查找所有原始数据而言，查找倒排索引后的数据将会快的多。只需要 查看标签这一栏，然后获取相关的文章ID即可。完全过滤掉无关的所有数据，提高效率！

elasticsearch的索引和Lucene的索引对比

在elasticsearch中， 索引 （库）这个词被频繁使用，这就是术语的使用。 在elasticsearch中，索引被分为多个分片，每份分片是一个Lucene的索引。所以**一个elasticsearch索引是由多个Lucene索引组成的**。别问为什么，谁让elasticsearch使用Lucene作为底层呢! 如无特指，说起索引都是指elasticsearch的索引。

## IK分词器插件

> 什么是IK分词器？

分词：即把一段中文或者别的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，会把数据库中或者索引库中的数据进行分词，然后进行一个匹配操作，默认的中文分词是将每个字看成一个词，比如 “我爱狂神” 会被分为"我","爱","狂","神"，这显然是不符合要求的，所以我们需要安装中文分词器ik来解决这个问题。

如果要使用中文，建议使用ik分词器！

IK提供了两个分词算法：ik_smart 和 ik_max_word，其中 ik_smart 为最少切分，ik_max_word为最细粒度划分！

> 安装

1. [https://github.com/medcl/elasticsearch-analysis-ik](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fmedcl%2Felasticsearch-analysis-ik)

2. 下载完毕之后，放入到我们的elasticsearch 插件即可！

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221053)

1. 重启观察ES，可以看到ik分词器被加载了！

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221101)

1. elasticsearch-plugin 可以通过这个命令来查看加载进来的插件

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221108)

2. 使用kibana测试！

> 查看不同的分词效果

其中 ik_smart 为最少切分

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221114)

ik_max_word为最细粒度划分！穷尽词库的可能！字典！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221123)

> 我们输入 超级喜欢狂神说Java

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221135)

发现问题：狂神说被拆开了！

这种自己需要的词，需要自己加到我们的分词器的字典中！

> ik 分词器增加自己的配置！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221144)

重启es，看细节！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221152)

再次测试一下狂神说，看下效果！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221157)

##  Rest风格说明

一种软件架构风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

基本Rest命令说明：

| method |                     url地址                     |          描述          |
| :----: | :---------------------------------------------: | :--------------------: |
|  PUT   |     localhost:9200/索引名称/类型名称/文档id     | 创建文档（指定文档id） |
|  POST  |        localhost:9200/索引名称/类型名称         | 创建文档（随机文档id） |
|  POST  | localhost:9200/索引名称/类型名称/文档id/_update |        修改文档        |
| DELETE |     localhost:9200/索引名称/类型名称/文档id     |        删除文档        |
|  GET   |     localhost:9200/索引名称/类型名称/文档id     |   查询文档通过文档id   |
|  POST  |    localhost:9200/索引名称/类型名称/_search     |      查询所有数据      |

## 关于索引的基本操作

#### 创建一个索引

```restructuredtext
PUT /索引名/~类型名~/文档id
{ 
  请求体
}
```

![image-20201020221349185](https://gitee.com/xudongyin/img/raw/master/img/20201020221352.png)

完成了自动增加了索引！数据也成功的添加了，这就是我说大家在初期可以把它当做数据库学习的原因！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221423)

那么 name 这个字段用不用指定类型呢。毕竟我们关系型数据库 是需要指定类型的啊 !

- 字符串类型
   text 、 keyword
- 数值类型
   long, integer, short, byte, double, float, half_float, scaled_float
- 日期类型
   date
- 布尔值类型
   boolean
- 二进制类型
   binary
- 等等.....

#### 指定字段的类型

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221435)

获得这个规则！ 可以通过 GET 请求获取具体的信息！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221442)

#### 查看默认的信息

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221453)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201020221504)

如果自己的文档字段没有指定，那么es 就会给我们默认配置字段类型！

**扩展**： 通过命令 elasticsearch 索引情况！ 通过GET  _cat/  可以获得es的当前的很多信息！

![image-20201020221650750](https://gitee.com/xudongyin/img/raw/master/img/20201020221652.png)

#### 修改索引

曾经是用PUT，如果字段写不全（少写字段就会被丢弃）也就是覆盖原来的数据！

![image-20201020222215867](https://gitee.com/xudongyin/img/raw/master/img/20201020222217.png)

现在的方法用POST，相应请求后面要加 _update ，只会更新提交的字段！

![image-20201020222509311](https://gitee.com/xudongyin/img/raw/master/img/20201020222510.png)

#### 删除索引

通过DELETE 命令实现删除、 根据你的请求来判断是删除索引还是删除文档记录！

使用RESTFUL 风格是ES推荐大家使用的！

## 关于文档的基本操作（重点）

### 基本操作

#### 添加数据

```json
PUT /kuangshen/user/1
{
 "name": "狂神说",
 "age": 23,
 "desc": "一顿操作猛如虎，一看工资2500",
 "tags": ["技术宅","温暖","直男"]
}
```

![image-20201020230840878](https://gitee.com/xudongyin/img/raw/master/img/20201020230841.png)

#### 获取数据 GET

![image-20201020230933347](https://gitee.com/xudongyin/img/raw/master/img/20201020230935.png)

#### 更新数据 PUT

![image-20201020231235347](https://gitee.com/xudongyin/img/raw/master/img/20201020231236.png)

#### Post       _update , 推荐使用这种更新方式！

> 使用 POST  后面没有加 _update

![image-20201020231803533](https://gitee.com/xudongyin/img/raw/master/img/20201020231804.png)

其他没有被提交的字段会被置空，然后新添加了doc.name字段

![image-20201020231837256](https://gitee.com/xudongyin/img/raw/master/img/20201020232422.png)

>使用 POST  后面加 _update

![image-20201020232149815](https://gitee.com/xudongyin/img/raw/master/img/20201020232151.png)

提交的字段值被更新了，没有被提交的字段还是原来的值

![image-20201020232216018](https://gitee.com/xudongyin/img/raw/master/img/20201020232433.png)

#### 简单地搜索

```bash
GET kuangshen/user/1
```

简答的条件查询，可以根据默认的映射规则，产生基本的查询！

![image-20201020232631538](https://gitee.com/xudongyin/img/raw/master/img/20201020232633.png)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201021085552)

### 复杂操作搜索 

select ( 排序，分页，高亮，模糊查询，精准查询！)

![img](https:////upload-images.jianshu.io/upload_images/18792147-b0778a9847806d6d.png?imageMogr2/auto-orient/strip|imageView2/2/w/810/format/webp)

![image-20201021100718247](https://gitee.com/xudongyin/img/raw/master/img/20201021100719.png)

输出结果，不想要那么多！

![image-20201021100943589](https://gitee.com/xudongyin/img/raw/master/img/20201021100944.png)

我们之后使用Java操作es ，所有的方法和对象就是这里面的 key！

> 排序！

![image-20201021101139045](https://gitee.com/xudongyin/img/raw/master/img/20201021101140.png)

> 分页查询！

![image-20201021101314977](https://gitee.com/xudongyin/img/raw/master/img/20201021101317.png)

数据下标还是从0开始的，和学的所有数据结构是一样的！

/search/{current}/{pagesize}

> 布尔值查询

**must （and）**，所有的条件都要符合 where id = 1 and name = xxx

![image-20201021101541899](https://gitee.com/xudongyin/img/raw/master/img/20201021101542.png)

**should（or）**，所有的条件都要符合 where id = 1 or name = xxx

![image-20201021101655893](https://gitee.com/xudongyin/img/raw/master/img/20201021101656.png)

**must_not （not）**

![image-20201021101751545](https://gitee.com/xudongyin/img/raw/master/img/20201021101755.png)

**过滤器 filter**

![image-20201021101934754](https://gitee.com/xudongyin/img/raw/master/img/20201021101936.png)

- gt 大于
- gte 大于等于
- lt 小于
- lte 小于等于！

![image-20201021102049200](https://gitee.com/xudongyin/img/raw/master/img/20201021102050.png)

![image-20201021111209384](https://gitee.com/xudongyin/img/raw/master/img/20201021111211.png)

> 匹配多个条件！

![image-20201021102302317](https://gitee.com/xudongyin/img/raw/master/img/20201021102303.png)

> 精确查询！

term 查询是直接通过倒排索引指定的词条进程精确查找的！

**关于分词：**

- term ，直接查询精确的
- match，会使用分词器解析！（先分析文档，然后在通过分析的文档进行查询！）

**两个类型 `text` `keyword`   (text类型的会被分词，keyword类型的不会被分词)**

![img](https://gitee.com/xudongyin/img/raw/master/img/20201021102846)

![image-20201021102830475](https://gitee.com/xudongyin/img/raw/master/img/20201021102831.png)

![image-20201021103033093](https://gitee.com/xudongyin/img/raw/master/img/20201021103034.png)

> 多个值匹配精确查询

![image-20201021103257151](https://gitee.com/xudongyin/img/raw/master/img/20201021103258.png)

> 高亮查询！

![image-20201021103521178](https://gitee.com/xudongyin/img/raw/master/img/20201021103522.png)

![image-20201021103643339](https://gitee.com/xudongyin/img/raw/master/img/20201021103645.png)

## 集成SpringBoot

> 找官方文档！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201021150002)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201021150011)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201021150023)

1. 找到原生的依赖

   

   ```xml
   <dependency>
     <groupId>org.elasticsearch.client</groupId>
     <artifactId>elasticsearch-rest-high-level-client</artifactId>
     <version>7.6.2</version>
   </dependency>
   ```

2. 找对象

   ![img](https://gitee.com/xudongyin/img/raw/master/img/20201021150032)

1. 分析这个类中的方法即可！

> 配置基本的项目

**问题：一定要保证 我们的导入的依赖和我们的 es 版本一致**

![img](https://gitee.com/xudongyin/img/raw/master/img/20201021150040)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201021150049)

源码中提供对象！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201021150102)

虽然这里导入3个类，静态内部类，核心类就一个！

```java
/**
* Elasticsearch rest client infrastructure configurations.
*
* @author Brian Clozel
* @author Stephane Nicoll
*/
class RestClientConfigurations {
    @Configuration(proxyBeanMethods = false)
    static class RestClientBuilderConfiguration {
        // RestClientBuilder
        @Bean
        @ConditionalOnMissingBean
        RestClientBuilder elasticsearchRestClientBuilder(RestClientProperties
                                                         properties,
                                                         ObjectProvider<RestClientBuilderCustomizer> builderCustomizers) {
            HttpHost[] hosts =
                properties.getUris().stream().map(HttpHost::create).toArray(HttpHost[]::new);
            RestClientBuilder builder = RestClient.builder(hosts);
            PropertyMapper map = PropertyMapper.get();
            map.from(properties::getUsername).whenHasText().to((username) -> {
                CredentialsProvider credentialsProvider = new
                    BasicCredentialsProvider();
                Credentials credentials = new
                    UsernamePasswordCredentials(properties.getUsername(),
                                                properties.getPassword());
                credentialsProvider.setCredentials(AuthScope.ANY, credentials);
                builder.setHttpClientConfigCallback(
                    (httpClientBuilder) ->
                    httpClientBuilder.setDefaultCredentialsProvider(credentialsProvider));
            });
            builder.setRequestConfigCallback((requestConfigBuilder) -> {

                map.from(properties::getConnectionTimeout).whenNonNull().asInt(Duration::toMill
                                                                               is)
                    .to(requestConfigBuilder::setConnectTimeout);

                map.from(properties::getReadTimeout).whenNonNull().asInt(Duration::toMillis)
                    .to(requestConfigBuilder::setSocketTimeout);
                return requestConfigBuilder;
            });
            builderCustomizers.orderedStream().forEach((customizer) ->
                                                       customizer.customize(builder));
            return builder;
        }
    }
    @Configuration(proxyBeanMethods = false)
    @ConditionalOnClass(RestHighLevelClient.class)
    static class RestHighLevelClientConfiguration {
        // RestHighLevelClient 高级客户端，也是我们这里要讲，后面项目会用到的客户端
        @Bean
        @ConditionalOnMissingBean
        RestHighLevelClient elasticsearchRestHighLevelClient(RestClientBuilder
                                                             restClientBuilder) {
            return new RestHighLevelClient(restClientBuilder);
        }
        @Bean
        @ConditionalOnMissingBean
        RestClient elasticsearchRestClient(RestClientBuilder builder,
                                           ObjectProvider<RestHighLevelClient> restHighLevelClient) {
            RestHighLevelClient client = restHighLevelClient.getIfUnique();
            if (client != null) {
                return client.getLowLevelClient();
            }
            return builder.build();
        }
    }
    @Configuration(proxyBeanMethods = false)
    static class RestClientFallbackConfiguration {
        // RestClient 普通的客户端！
        @Bean
        @ConditionalOnMissingBean
        RestClient elasticsearchRestClient(RestClientBuilder builder) {
            return builder.build();
        }
    }
}
```

> 具体的Api测试！

1. 创建索引
2. 判断索引是否存在
3. 删除索引
4. 创建文档
5. crud文档！

配置类

~~~java
@Configuration
public class RestHighLevelClientConfiguration {
    @Bean
    public RestHighLevelClient restHighLevelClient() {
        RestHighLevelClient client = new RestHighLevelClient(
                RestClient.builder(
                        new HttpHost("localhost", 9200, "http")));
        return client;
    }
}
~~~

测试类

```java
@SpringBootTest
class KuangshenEsApiApplicationTests {
    // 面向对象来操作
    @Autowired
    @Qualifier("restHighLevelClient")
    private RestHighLevelClient client;
    // 测试索引的创建 Request PUT kuang_index
    @Test
    void testCreateIndex() throws IOException {
        // 1、创建索引请求
        CreateIndexRequest request = new CreateIndexRequest("kuang_index");
        // 2、客户端执行请求 IndicesClient,请求后获得响应
        CreateIndexResponse createIndexResponse =
            client.indices().create(request, RequestOptions.DEFAULT);
        System.out.println(createIndexResponse);
    }
    // 测试获取索引,判断其是否存在
    @Test
    void testExistIndex() throws IOException {
        GetIndexRequest request = new GetIndexRequest("kuang_index2");
        boolean exists = client.indices().exists(request,                                      RequestOptions.DEFAULT);
        System.out.println(exists);
    }
    // 测试删除索引
    @Test
    void testDeleteIndex() throws IOException {
        DeleteIndexRequest request = new DeleteIndexRequest("kuang_index");
        // 删除
        AcknowledgedResponse delete = client.indices().delete(request,
                                                              RequestOptions.DEFAULT);
        System.out.println(delete.isAcknowledged());
    }
    // 测试添加文档
    @Test
    void testAddDocument() throws IOException {
        // 创建对象
        User user = new User("狂神说", 3);
        // 创建请求
        IndexRequest request = new IndexRequest("kuang_index");
        // 规则 put /kuang_index/_doc/1
        request.id("1");
        request.timeout(TimeValue.timeValueSeconds(1));
        request.timeout("1s");
        // 将我们的数据放入请求 json
        request.source(JSON.toJSONString(user), XContentType.JSON);
        // 客户端发送请求 , 获取响应的结果
        IndexResponse indexResponse = client.index(request,
                                                   RequestOptions.DEFAULT);
        System.out.println(indexResponse.toString()); //
        System.out.println(indexResponse.status()); // 对应我们命令返回的状态CREATED
    }
    // 获取文档，判断是否存在 get /index/doc/1
    @Test
    void testIsExists() throws IOException {
        GetRequest getRequest = new GetRequest("kuang_index", "1");
        // 不获取返回的 _source 的上下文了
        getRequest.fetchSourceContext(new FetchSourceContext(false));
        getRequest.storedFields("_none_");
        boolean exists = client.exists(getRequest, RequestOptions.DEFAULT);
        System.out.println(exists);
    }
    // 获得文档的信息
    @Test
    void testGetDocument() throws IOException {
        GetRequest getRequest = new GetRequest("kuang_index", "1");
        GetResponse getResponse = client.get(getRequest,
                                             RequestOptions.DEFAULT);
        System.out.println(getResponse.getSourceAsString()); // 打印文档的内容
        System.out.println(getResponse); // 返回的全部内容和命令式一样的
    }
    // 更新文档的信息
    @Test
    void testUpdateRequest() throws IOException {
        UpdateRequest updateRequest = new UpdateRequest("kuang_index","1");
        updateRequest.timeout("1s");
        User user = new User("狂神说Java", 18);
        updateRequest.doc(JSON.toJSONString(user),XContentType.JSON);
        UpdateResponse updateResponse = client.update(updateRequest,
                                                      RequestOptions.DEFAULT);
        System.out.println(updateResponse.status());
    }
    // 删除文档记录
    @Test
    void testDeleteRequest() throws IOException {
        DeleteRequest request = new DeleteRequest("kuang_index","1");
        request.timeout("1s");
        DeleteResponse deleteResponse = client.delete(request,
                                                      RequestOptions.DEFAULT);
        System.out.println(deleteResponse.status());
    }
    // 特殊的，真的项目一般都会批量插入数据！
    @Test
    void testBulkRequest() throws IOException {
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("10s");
        ArrayList<User> userList = new ArrayList<>();
        userList.add(new User("kuangshen1",3));
        userList.add(new User("kuangshen2",3));
        userList.add(new User("kuangshen3",3));
        userList.add(new User("qinjiang1",3));
        userList.add(new User("qinjiang1",3));
        userList.add(new User("qinjiang1",3));
        // 批处理请求
        for (int i = 0; i < userList.size() ; i++) {
            // 批量更新和批量删除，就在这里修改对应的请求就可以了
            bulkRequest.add(
                new IndexRequest("kuang_index")
                .id(""+(i+1))
                .source(JSON.toJSONString(userList.get(i)),XContentType.JSON));
        }
        BulkResponse bulkResponse = client.bulk(bulkRequest,
                                                RequestOptions.DEFAULT);
        System.out.println(bulkResponse.hasFailures()); // 是否失败，返回 false 代表成功！
    }
    // 查询
    // SearchRequest 搜索请求
    // SearchSourceBuilder 条件构造
    // HighlightBuilder 构建高亮
    // TermQueryBuilder 精确查询
    // MatchAllQueryBuilder
    // xxx QueryBuilder 对应我们刚才看到的命令！

    @Test
    void testSearch() throws IOException {
        SearchRequest searchRequest = new SearchRequest("kuang_index");
        // 构建搜索条件
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        sourceBuilder.highlighter()
            // 查询条件，我们可以使用 QueryBuilders 工具来实现
            // QueryBuilders.termQuery 精确
            // QueryBuilders.matchAllQuery() 匹配所有
            TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("name",
                                                                        "qinjiang1");
        //    MatchAllQueryBuilder matchAllQueryBuilder = QueryBuilders.matchAllQuery();
        sourceBuilder.query(termQueryBuilder);
        sourceBuilder.timeout(new TimeValue(60,TimeUnit.SECONDS));
        searchRequest.source(sourceBuilder);
        SearchResponse searchResponse = client.search(searchRequest,
                                                      RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(searchResponse.getHits()));
        System.out.println("=================================");
        for (SearchHit documentFields : searchResponse.getHits().getHits()) {
            System.out.println(documentFields.getSourceAsMap());
        }
    }
}
```



## 爬虫

1、导入依赖

~~~xml
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.11.2</version>
</dependency>
~~~

2、编写工具类

~~~java
package com.xu.utils;

import com.xu.pojo.Content;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import org.springframework.stereotype.Component;

import java.net.URL;
import java.util.ArrayList;
@Component
public class JsoupUtils {
    public static void main(String[] args) throws Exception {
       new  JsoupUtils().getContent("心理学").forEach(System.out::println);
    }

    public  ArrayList<Content> getContent(String keyword) throws Exception{
        //请求url https://search.jd.com/Search?keyword=java
        String url = "https://search.jd.com/Search?keyword="+keyword+"&enc=utf-8";
        //返回的就是浏览器document对象
        Document document = Jsoup.parse(new URL(url), 30000);
        //获取一个大的div
        Element element = document.getElementById("J_goodsList");
        //获取div里的所有li
        Elements elements = element.getElementsByTag("li");
//        System.out.println(elements.html());
        ArrayList<Content> list = new ArrayList<>();
        for (Element el : elements) {

            String img = el.getElementsByTag("img").get(0).attr("data-lazy-img");
            String price = el.getElementsByClass("p-price").get(0).text();
            String title = el.getElementsByClass("p-name").get(0).text();
            Content content = new Content(title, price, img);
            list.add(content);
        }
        return list;
    }

}
~~~

3、业务编写

~~~java
package com.xu.service;

import com.alibaba.fastjson.JSON;
import com.xu.pojo.Content;
import com.xu.utils.JsoupUtils;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.common.text.Text;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.TermQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightField;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.concurrent.TimeUnit;

//业务编写
@Service
public class ContentService {

    @Autowired
    private RestHighLevelClient restHighLevelClient;

    //解析数据放入 es 索引中
    public boolean parseContent(String keyword) throws Exception {
        ArrayList<Content> contents = new JsoupUtils().getContent(keyword);
        //把查询的数据放入es中
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout(new TimeValue(60, TimeUnit.SECONDS));
        for (int i = 0; i < contents.size(); i++) {
            bulkRequest.add(new IndexRequest("jd_index").source(JSON.toJSONString(contents.get(i)), XContentType.JSON));
        }
        BulkResponse bulkResponse = restHighLevelClient.bulk(bulkRequest, RequestOptions.DEFAULT);
        return !bulkResponse.hasFailures();
    }

    //获取es里数据实现，实现搜索功能
    public List<Map<String, Object>> searchPage(String keyword, int pageNo, int pageSize) throws IOException {
        if (pageNo<=0){
            pageNo = 1;
        }
        //条件搜索
        SearchRequest searchRequest = new SearchRequest("jd_index");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

        //分页
        sourceBuilder.from((pageNo - 1) * pageSize);
        sourceBuilder.size(pageSize);

        //精确查找
        TermQueryBuilder termQuery = QueryBuilders.termQuery("title", keyword);
        sourceBuilder.query(termQuery);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));

        //高亮
        HighlightBuilder highlightBuilder = new HighlightBuilder();
        highlightBuilder.field("title");
        highlightBuilder.requireFieldMatch(false);//false 多个高亮开启 默认就是false
        highlightBuilder.preTags("<span style='color:red'>");
        highlightBuilder.postTags("</span>");
        sourceBuilder.highlighter(highlightBuilder);

        //执行搜索
        searchRequest.source(sourceBuilder);
        SearchResponse response = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);

        //解析结果
        ArrayList<Map<String, Object>> list = new ArrayList<>();
        for (SearchHit hit : response.getHits().getHits()) {
            Map<String, HighlightField> highlightFields = hit.getHighlightFields();
            HighlightField title = highlightFields.get("title");
            Map<String, Object> sourceAsMap = hit.getSourceAsMap();//原来的数据
            //解析高亮字段,将原来的字段换为高亮字段
            if (title != null) {
                Text[] texts = title.getFragments();
                String newTitle = "";
                for (Text text : texts) {
                    newTitle += text;
                }
                sourceAsMap.put("title", newTitle);
            }
            list.add(sourceAsMap);
        }
        return list;
    }
}
~~~



