## 关系型数据库

### 什么是关系型数据库？

关系型数据库，是指采用了关系模型来组织数据的数据库，其以行和列的形式存储数据，以便于用户理解，关系型数据库这一系列的行和列被称为表，一组表组成了数据库。用户通过查询来检索数据库中的数据，而查询是一个用于限定数据库中某些区域的执行代码。

简单来说，关系模式就是二维表格模型。

### 关系型数据库有什么优势？

关系型数据库的优势：

- 易于理解

  关系型二维表的结构非常贴近现实世界，二维表格，容易理解。

- 支持复杂查询 可以用 SQL 语句方便的在一个表以及多个表之间做非常复杂的数据查询。

- 支持事务 可靠的处理事务并且保持事务的完整性，使得对于安全性能很高的数据访问要求得以实现。
  

## MySQL数据库

### 什么是SQL

结构化查询语言 (Structured Query Language) 简称SQL，是一种特殊目的的编程语言，是一种数据库查询和程序设计语言程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统。

### 什么是MySQL？

MySQL 是一个关系型数据库管理系统，MySQL 是最流行的关系型数据库管理系统之一，常见的关系型数据库还有 Oracle 、SQL Server、Access 等等。

**MySQL在过去由于性能高、成本低、可靠性好，已经成为最流行的开源数据库，广泛地应用在 Internet 上的中小型网站中**。

### MySQL 和 MariaDB 傻傻分不清楚？

MySQL 最初由瑞典 MySQL AB 公司开发，MySQL 的创始人是乌尔夫·米卡埃尔·维德纽斯，常用昵称蒙提（Monty）。

在被甲骨文公司收购后，现在属于甲骨文公司（Oracle） 旗下产品。Oracle 大幅调涨MySQL商业版的售价，因此导致自由软件社区们对于Oracle是否还会持续支持MySQL社区版有所隐忧。

MySQL 的创始人就是之前那个叫 Monty 的大佬以 MySQL为基础成立分支计划 MariaDB。

MariaDB打算保持与MySQL的高度兼容性，确保具有库二进制奇偶校验的直接替换功能，以及与MySQL API 应用程序接口)和命令的精确匹配。而原先一些使用 MySQL 的开源软件逐渐转向 MariaDB 或其它的数据库。

所以如果看到你公司用的是 MariaDB 不用怀疑，其实它骨子里还是 MySQL，学会了MySQL 也就会了 MariaDB。

### 一个彩蛋

MariaDB 是以 Monty 的小女儿Maria命名的，就像MySQL是以他另一个女儿 My 命名的一样，两款鼎鼎大名的数据库分别用两个女儿的名字命名，你大爷还是你大爷，老爷子牛批! 

### 如何查看MySQL当前版本号？

在系统命令行下：`mysql -V`

连接上MySQL命令行输入:

`> status`;

```
Server:   MySQL
Server version:  5.5.45
Protocol version: 10
```

或 `select version();`

```
+------------------------+
| version()              |
+------------------------+
| 5.5.45-xxxxx |
+------------------------+
```

## 基础数据类型

#### MySQL 有哪些数据类型？

MySQL 数据类型非常丰富，常用类型简单介绍如下：

整数类型：`BIT、BOOL、TINY INT、SMALL INT、MEDIUM INT、 INT、 BIG INT`

浮点数类型：`FLOAT、DOUBLE、DECIMAL`

字符串类型：`CHAR、VARCHAR、TINY TEXT、TEXT、MEDIUM TEXT、LONGTEXT、TINY BLOB、BLOB、MEDIUM BLOB、LONG BLOB`

日期类型：`Date、DateTime、TimeStamp、Time、Year`

其他数据类型：`BINARY、VARBINARY、ENUM、SET`...

### CHAR 和 VARCHAR的区别？

**CHAR 是固定长度的字符类型，VARCHAR 则是可变长度的字符类型**，下面讨论基于在 MySQL5.0 以上版本中。

#### 共同点

CHAR(M) 和 VARCHAR(M) 都表示该列能存储 M 个**字符**，**注意不是字节！！**

#### CHAR类型特点

- CHAR 最多可以存储 255 个**字符 (注意不是字节)**，字符有不同的编码集，比如 UTF8 编码 (3字节)、GBK 编码 (2字节) 等。
- 对于 `CHAR(M)` 如果实际存储的数据长度小于M，则 MySQL 会自动会在它的右边用空格字符补足，但是在检索操作中那些填补出来的空格字符会被去掉。

#### VARCHAR类型特点

- VARCHAR 的最大长度为 65535 个**字节**。
- VARCHAR 存储的是实际的字符串加1或2个字节用来记录字符串实际长度，字符串长度小于255字节用1字节记录，超过255就需要2字节记录。

### VARCHAR(50) 能存放几个 UTF8 编码的汉字？

存放的汉字个数与版本相关。

mysql 4.0以下版本，varchar(50) 指的是 50 **字节**，如果存放 UTF8 格式编码的汉字时（每个汉字3字节），只能存放16 个。

mysql 5.0以上版本，varchar(50) 指的是 50 **字符**，无论存放的是数字、字母还是 UTF8 编码的汉字，都可以存放 50 个。

### int(10) 和 bigint(10)能存储的数据大小一样吗？

不一样，具体原因如下：

- int 能存储四字节有符号整数。
- bigint 能存储八字节有符号整数。

所以能存储的数据大小不一样，其中的数字 `10` 代表的只是数据的显示宽度。[^13]

- 显示宽度指明Mysql最大可能显示的数字个数，数值的位数小于指定的宽度时数字左边会用**空格填充**，空格不容易看出。
- 如果插入了大于显示宽度的值，只要该值不超过该类型的取值范围，数值依然可以插入且能够显示出来。
- 建表的时候指定 `zerofill` 选项，则不足显示宽度的部分用 `0` 填充，如果是 1 会显示成 `0000000001`。
- 如果没指定显示宽度， bigint 默认宽度是 20 ，int默认宽度 11。
  

## 存储引擎相关

### MySQL存储引擎类型有哪些？

常用的存储引擎有 InnoDB 存储引擎和 MyISAM 存储引擎，InnoDB 是 MySQL 的默认事务引擎。

查看数据库表当前支持的引擎，可以用下面查询语句查看 ：

```
# 查询结果表中的 Engine 字段指示存储引擎类型。
show table status from 'your_db_name' where name='your_table_name'; 
```

### InnoDB存储引擎应用场景是什么？

InnoDB 是 MySQL的默认「事务引擎」，被设置用来处理大量短期（short-lived）事务，短期事务大部分情况是正常提交的，很少会回滚。

#### InnoDB存储引擎特性有哪些？

采用多版本并发控制（MVCC，MultiVersion Concurrency Control）来支持高并发。并且实现了四个标准的隔离级别，通过间隙锁`next-key locking`策略防止幻读的出现。

引擎的表基于聚簇索引建立，聚簇索引对主键查询有很高的性能。不过它的二级索引`secondary index`非主键索引中必须包含主键列，所以如果主键列很大的话，其他的所有索引都会很大。因此，若表上的索引较多的话，主键应当尽可能的小。另外InnoDB的存储格式是平台独立。

InnoDB做了很多优化，比如：磁盘读取数据方式采用的可预测性预读、自动在内存中创建hash索引以加速读操作的自适应哈希索引（adaptive hash index)，以及能够加速插入操作的插入缓冲区（insert buffer)等。

InnoDB通过一些机制和工具支持真正的热备份，MySQL 的其他存储引擎不支持热备份，要获取一致性视图需要停止对所有表的写入，而在读写混合场景中，停止写入可能也意味着停止读取。

### InnoDB  引擎的四大特性是什么？

#### 插入缓冲（Insert buffer)

Insert Buffer 用于非聚集索引的插入和更新操作。先判断插入的非聚集索引是否在缓存池中，如果在则直接插入，否则插入到 Insert Buffer 对象里。再以一定的频率进行 Insert Buffer 和辅助索引叶子节点的 merge 操作，将多次插入合并到一个操作中，提高对非聚集索引的插入性能。

#### 二次写 (Double write)

Double Write由两部分组成，一部分是内存中的double write buffer，大小为2MB，另一部分是物理磁盘上共享表空间连续的128个页，大小也为 2MB。在对缓冲池的脏页进行刷新时，并不直接写磁盘，而是通过 memcpy 函数将脏页先复制到内存中的该区域，之后通过doublewrite buffer再分两次，每次1MB顺序地写入共享表空间的物理磁盘上，然后马上调用fsync函数，同步磁盘，避免操作系统缓冲写带来的问题。

#### 自适应哈希索引 (Adaptive Hash Index)

InnoDB会根据访问的频率和模式，为热点页建立哈希索引，来提高查询效率。索引通过缓存池的 B+ 树页构造而来，因此建立速度很快，InnoDB存储引擎会监控对表上各个索引页的查询，如果观察到建立哈希索引可以带来速度上的提升，则建立哈希索引，所以叫做自适应哈希索引。

#### 缓存池

为了提高数据库的性能，引入缓存池的概念，通过参数 innodb_buffer_pool_size 可以设置缓存池的大小，参数 innodb_buffer_pool_instances 可以设置缓存池的实例个数。缓存池主要用于存储以下内容：

缓冲池中缓存的数据页类型有：索引页、数据页、undo页、插入缓冲 (insert buffer)、自适应哈希索引(adaptive hash index)、InnoDB存储的锁信息 (lock info)和数据字典信息 (data dictionary)。

### MyISAM存储引擎应用场景有哪些？

MyISAM 是 MySQL 5.1 及之前的版本的默认的存储引擎。MyISAM 提供了大量的特性，包括全文索引、压缩、空间函数（GIS)等，但MyISAM 不「支持事务和行级锁」，对于只读数据，或者表比较小、可以容忍修复操作，依然可以使用它。

### MyISAM存储引擎特性有哪些？

MyISAM「不支持行级锁而是对整张表加锁」。读取时会对需要读到的所有表加共享锁，写入时则对表加排它锁。但在表有读取操作的同时，也可以往表中插入新的记录，这被称为并发插入。

MyISAM 表可以手工或者自动执行检查和修复操作。但是和事务恢复以及崩溃恢复不同，可能导致一些「数据丢失」，而且修复操作是非常慢的。

对于 MyISAM 表，即使是`BLOB`和`TEXT`等长字段，也可以基于其前 500 个字符创建索引，MyISAM 也支持「全文索引」，这是一种基于分词创建的索引，可以支持复杂的查询。

如果指定了`DELAY_KEY_WRITE`选项，在每次修改执行完成时，不会立即将修改的索引数据写入磁盘，而是会写到内存中的键缓冲区，只有在清理键缓冲区或者关闭表的时候才会将对应的索引块写入磁盘。这种方式可以极大的提升写入性能，但是在数据库或者主机崩溃时会造成「索引损坏」，需要执行修复操作。

### MyISAM  与  InnoDB  存储引擎 5 大区别

- InnoDB支持事物，而MyISAM不支持事物
- InnoDB支持行级锁，而MyISAM支持表级锁
- InnoDB支持MVCC, 而MyISAM不支持
- InnoDB支持外键，而MyISAM不支持
- InnoDB不支持全文索引，而MyISAM支持

一张表简单罗列两种引擎的主要区别，如下图：

![img](https://mmbiz.qpic.cn/mmbiz_png/ceNmtYOhbMSIaAmoSYianlwqUjqymUETljvrMbU5PaMZUOzK2hDwdm9oibJhbslSq5icicsgja5th4S3Y5ZlIuibiajA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

mysql引擎对比

### SELECT COUNT(*) 在哪个引擎执行更快？

`SELECT COUNT(*)`  常用于统计表的总行数，**在 MyISAM  存储引擎中执行更快，前提是不能加有任何WHERE条件**。

这是因为 MyISAM 对于表的行数做了优化，内部用一个变量存储了表的行数，如果查询条件没有 WHERE 条件则是查询表中一共有多少条数据，MyISAM 可以迅速返回结果，如果加 WHERE 条件就不行。

InnoDB 的表也有一个存储了表行数的变量，但这个值是一个估计值，所以并没有太大实际意义。


## MySQL 基础知识

### 说一下数据库设计三范式是什么？

1范式：1NF是对属性的原子性约束，要求属性具有原子性，不可再分解；(只要是关系型数据库都满足1NF)

2范式：2NF是对记录的惟一性约束，要求记录有惟一标识，即实体的惟一性；

3范式：3NF是对字段冗余性的约束，即任何字段不能由其他字段派生出来，它要求字段没有冗余。没有冗余的数据库设计可以做到

但是，没有冗余的数据库未必是最好的数据库，有时为了提高运行效率，就必须降低范式标准，适当保留冗余数据，具体做法是：在概念数据模型设计时遵守第三范式，降低范式标准的工作放到物理数据模型设计时考虑，降低范式就是增加字段，允许冗余。

### SQL 语句有哪些分类？

1. DDL：数据定义语言（create alter drop）
2. DML：数据操作语句（insert update delete）
3. DTL：数据事务语句（commit collback savapoint）
4. DCL：数据控制语句（grant revoke）

### 数据库删除操作中的 delete、drop、 truncate 区别在哪？

- 当不再需要该表时可以用 drop 来删除表;
- 当仍要保留该表，但要删除所有记录时， 用 truncate来删除表中记录。
- 当要删除部分记录时（一般来说有 WHERE 子句约束） 用 delete来删除表中部分记录。

### 什么是MySql视图？

视图是虚拟表，并不储存数据，只包含定义时的语句的动态数据。

创建视图语法：

```
CREATE
    [OR REPLACE]
    [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
    [DEFINER = user]
    [SQL SECURITY { DEFINER | INVOKER }]
    VIEW view_name [(column_list)]
    AS select_statement
    [WITH [CASCADED | LOCAL] CHECK OPTION]
```

参数说明：

- OR REPLACE：如果视图存在，则替换已有视图。
- ALGORITHM：视图选择算法，默认算法是 UNDEFINED(未定义的)由 MySQL自动选择要使用的算法。
- DEFINER：指定视图创建者或定义者，如果不指定该选项，则创建视图的用户就是定义者。
- SQL SECURITY：SQL安全性，默认为DEFINER。
- select_statement：创建视图的 SELECT语句，可以从基表或其他视图中选择数据。
- WITH CHECK OPTION：表示视图在更新时保证约束，默认是 CASCADED。

### 使用 MySQL 视图有何优点？

1. 操作简单方便。视图用户完全不需要关心视图对应的表的结构、关联条件和筛选条件，对用户来说已经是过滤好的复合条件的结果集。
2. 数据更加安全。视图用户只能访问视图中的结果集，通过视图可以把对表的访问权限限制在某些行和列上面。
3. 数据隔离。屏蔽了源表结构变化对用户带来的影响，源表结构变化视图结构不变。^1

### MySql服务默认端口号是多少 ？

默认端口是 `3306`

查看端口命令：`> show variables like 'port';`

### 用  DISTINCT  过滤 多列的规则？

DISTINCT 用于对选择的数据去重，单列用法容易理解。比如有如下数据表 `tamb`：

```
   name        number
   Tencent      1
   Alibaba      2
   Bytedance    3
   Meituan      3
```

查询语句：`SELECT DISTINCT name FROM table tamb` 结果如下：

```
   name       
   Tencent   
   Alibaba    
   Bytedance 
   Meituan  
```

如果要求按 `number` 列去重同时显示 `name` ，你可能会写出查询语句：

```
SELECT DISTINCT number, name FROM table tamb
```

**多参数 DISTINCT 去重规则是：把 DISTINCT  之后的所有参数当做一个过滤条件，也就是说会对 `(number, name)`整体去重处理，只有当这个组合不同才会去重**，结果如下：

```
       number   name
         1  Tencent
         2  Alibaba
         3  Bytedance
         3  Meituan
```

从结果来看好像并没有达到我们想要的去重的效果，那要怎么实现「按 `number` 列去重同时显示 `name`」呢？可以用 `Group By` 语句：

`SELECT number, name FROM table tamb GROUP BY number` 输出如下，正是我们想要的效果：

```
       number   name
         1  Tencent
         2  Alibaba
         3  Bytedance
```

### 什么是存储过程？

一条或多条sql语句集合，有以下一些特点：

- 存储过程能实现较快的执行速度。
- 存储过程可以用流程控制语句编写，有很强的灵活性，可以完成复杂的判断和较复杂的运算。
- 存储过程可被作为一种安全机制来充分利用。
- 存储过程能够减少网络流量

```
delimiter 分隔符
create procedure|proc proc_name()
begin
    sql语句
end 分隔符
delimiter ；    --还原分隔符，为了不影响后面的语句的使用
--默认的分隔符是；但是为了能在整个存储过程中重用，因此一般需要自定义分隔符（除\外）

show procedure status like ""; --查询存储过程，可以不适用like进行过滤
drop procedure if exists；--删除存储过程
```

#### 存储过程和函数好像差不多，你说说他们有什么区别?

存储过程和函数是事先经过编译并存储在数据库中的一段 SQL 语句的集合，调用存储过程和函数可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

**相同点**

- 存储过程和函数都是为了可重复的执行操作数据库的 SQL 语句的集合。
- 存储过程和函数都是一次编译后缓存起来，下次使用就直接命中已经编译好的 sql 语句，减少网络交互提高了效率。

**不同点**

- 标识符不同，函数的标识符是 function，存储过程是 procedure。
- 函数返回单个值或者表对象，而存储过程没有返回值，但是可以通过OUT参数返回多个值。
- 函数限制比较多，比如不能用临时表，只能用表变量，一些函数都不可用等，而存储过程的限制相对就比较少。
- 一般来说，存储过程实现的功能要复杂一点，而函数的实现的功能针对性比较强
- 函数的参数只能是 IN 类型，存储过程的参数可以是`IN OUT INOUT`三种类型。
- 存储函数使用 select 调用，存储过程需要使用 call 调用。