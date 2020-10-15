# MySQL

> 什么是数据库

数据库 ( **DataBase** , 简称**DB** )

**概念** : 长期存放在计算机内,有组织,可共享的大量数据的集合,是一个数据 "仓库"

**作用** : 保存,并能安全管理数据(如:增删改查等),减少冗余...

**数据库总览 :**

- 关系型数据库 ( SQL)

- - MySQL, Oracle , SQL Server , SQLite , DB2 , ...
  - 关系型数据库通过外键关联来建立表与表之间的关系

- 非关系型数据库 ( NOSQL )

- - Redis , MongoDB , ...
  - 非关系型数据库通常指数据以对象的形式存储在数据库中，而对象之间的关系通过每个对象自身的属性来决定



> 什么是DBMS

数据库管理系统 ( **D**ata**B**ase **M**anagement **S**ystem )

数据库管理软件 , 科学组织和存储数据 , 高效地获取和维护数据



![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182455)

> 结构化查询语句分类

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182501)

## 数据库操作

### 命令行操作数据库

创建数据库 :  create database  [if not exists]  数据库名;

删除数据库 : drop database  [if exists] 数据库名;

查看数据库 : show databases;

使用数据库 : use 数据库名;

> 对比工具操作数据库

**学习方法：**

- 对照SQLyog工具自动生成的语句学习
- 固定语法中的单词需要记忆
- ![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182505)



### 创建数据表

属于DDL的一种，语法 :

```mysql
create table [if not exists] `表名`(
   '字段名1' 列类型 [属性][索引][注释],
   '字段名2' 列类型 [属性][索引][注释],
  ...
   '字段名n' 列类型 [属性][索引][注释]
)[表类型][表字符集][注释];
```

**说明 :** 反引号用于区别MySQL保留字与普通字符而引入的 (键盘esc下面的键).



### 数据值和列类型

列类型 : 规定数据库中该列存放的数据类型

> 数值类型

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182510)

> 字符串类型

|    类型    | 说明                                                         | 大小                                              |
| :--------: | ------------------------------------------------------------ | ------------------------------------------------- |
|  char[M]   | 字符固定大小的字符串,M代表字符数 ，  char（5）如果字符串为“abc” 则剩余的两个字符填补为空格 | 0~255字节       能容纳多少字符看编码，varchar同理 |
| varchar[M] | 可变长度的字符串,M代表字符数                                 | 0~65535字节                                       |
|  tinytext  | 微型文本                                                     | 2^8^-1字节                                        |
|    text    | 文本串                                                       | 2^16^-1字节                                       |

举两个例说明一下实际长度的计算。

a)  若一个表只有一个varchar类型，如定义为

create table t4(c varchar(N)) charset=gbk;

则此处N的最大值为(65535-1-2)/2= 32766。

==减1的原因是实际行存储从第二个字节开始;==

==减2的原因是varchar头部的2个字节表示长度;==

除2的原因是字符编码是gbk。

 

b) 若一个表定义为

create table t4(c int, c2 char(30), c3 varchar(N)) charset=utf8;

则此处N的最大值为 (65535-1-2-4-30*3)/3=21812

减1和减2与上例相同;

减4的原因是int类型的c占4个字节;

减30*3的原因是char(30)占用90个字节，编码是utf8。

==如果被varchar超过上述的b规则，被强转成text类型，则每个字段占用定义长度为11字节，当然这已经不是varchar了。==

> 日期和时间型数值类型

|   类型    |        格式        | 说明                           |
| :-------: | :----------------: | ------------------------------ |
|   DATE    |     YYYY-MM-DD     | 日期格式                       |
|   TIME    |      HH:mm:ss      | 时间格式                       |
| DATETIME  | YY-MM-DD  HH:mm:ss | 最常用的时间格式               |
| TIMESTAMP |   YYYYMMDDHHmmss   | 时间戳，1970.1.1到现在的毫秒数 |
|   year    |        YYYY        | 1901~2155                      |

==插入数据时，用‘’单引号==

timestamp 有两个属性，分别是 CURRENT_TIMESTAMP 和 ON UPDATE CURRENT_TIMESTAMP两种，使用情况分别如下：

1.==CURRENT_TIMESTAMP==

当要向数据库执行 insert 操作时，如果有个timestamp字段属性设为 CURRENT_TIMESTAMP，则无论这个字段有木有 set 值都插入当前系统时间 

2.==ON UPDATE CURRENT_TIMESTAMP==

当执行 update 操作时，并且字段有ON UPDATE CURRENT_TIMESTAMP属性。则字段无论值有没有变化，他的值也会跟着更新为当前UPDATE操作时的时间。

> NULL值

- 理解为 "没有值" 或 "未知值"
- ==不要用NULL进行算术运算 , 结果仍为NULL==



### 数据字段属性

**UnSigned**

- 无符号的
- 声明该数据列不允许负数 .

**ZEROFILL**

- 0填充的
- 不足位数的用0来填充 , 如int(3),5则为005

**Auto_InCrement**

- 自动增长的 , 每添加一条数据 , 自动在上一个记录数上加 1(默认)

- 通常用于设置**主键** , 且为整数类型

- 可定义起始值和步长

- - 当前表设置步长(AUTO_INCREMENT=100) : 只影响当前表
  - SET @@auto_increment_increment=5 ; 影响所有使用自增的表(全局)

**NULL 和 NOT NULL**

- 默认为NULL , 即没有插入该列的数值
- 如果设置为NOT NULL , 则该列必须有值

**DEFAULT**

- 默认的
- 用于设置默认值
- 例如,性别字段,默认为"男" , 否则为 "女" ; 若无指定该列的值 , 则默认值为"男"的值

```mysql
-- 目标 : 创建一个school数据库
-- 创建学生表(列,字段)
-- 学号int 登录密码varchar(20) 姓名,性别varchar(2),出生日期(datetime),家庭住址,email
-- 创建表之前 , 一定要先选择数据库
-- COMMENT  注释 ，英文逗号隔开语句，最后一句不用逗号
CREATE TABLE IF NOT EXISTS `student` (
`id` int(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
`name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`pwd` varchar(20) NOT NULL DEFAULT '123456' COMMENT '密码',
`sex` varchar(2) NOT NULL DEFAULT '男' COMMENT '性别',
`birthday` datetime DEFAULT NULL COMMENT '生日',
`address` varchar(100) DEFAULT NULL COMMENT '地址',
`email` varchar(50) DEFAULT NULL COMMENT '邮箱',
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

-- 查看->创建数据库的定义SQL语句
SHOW CREATE DATABASE school;
-- 查看->创建数据表的定义SQL语句
SHOW CREATE TABLE student;
-- 显示表结构
DESC student;  -- 设置严格检查模式(不能容错了)SET sql_mode='STRICT_TRANS_TABLES';
```



### 数据表的类型

> 设置数据表的类型

```mysql
CREATE TABLE 表名(
   -- 省略一些代码
   -- Mysql注释
   -- 1. # 单行注释
   -- 2. /*...*/ 多行注释
)ENGINE = MyISAM (or InnoDB)

-- 查看mysql所支持的引擎类型 (表类型)
SHOW ENGINES;
```

MySQL的数据表的类型 : **MyISAM** , **InnoDB** , HEAP , BOB , CSV等...

常见的 MyISAM 与 InnoDB 类型：

| 名称       | MyISAM                         | InnoDB               |
| :--------- | ------------------------------ | -------------------- |
| 事务处理   | 不支持                         | 支持                 |
| 数据行锁定 | 不支持（表级锁，是整个表锁定） | 支持                 |
| 外键约束   | 不支持                         | 支持                 |
| 全文索引   | 支持                           | 不支持 (5.6以上支持) |
| 表空间大小 | 较小                           | 较大，约两倍         |

经验 ( 适用场合 )  :

- 适用 MyISAM : 节约空间及相应速度

- 适用 InnoDB : 安全性 , 事务处理及多用户操作数据表

  

> 数据表的存储位置

- MySQL数据表以文件方式存放在磁盘中

- - 包括表文件 , 数据文件 , 以及数据库的选项文件
  - 位置 : Mysql安装目录\data\下存放数据表 . 目录名对应数据库名 , 该目录下文件名对应数据表 .

- 注意 :

- - \* . frm -- 表结构定义文件

  - \* . MYD -- 数据文件 ( data )

  - \* . MYI -- 索引文件 ( index )

  - InnoDB类型数据表只有一个 *.frm文件 , 以及上一级目录的ibdata1文件

  - MyISAM类型数据表对应三个文件 :

    ![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182522)

> 设置数据表字符集

我们可为数据库,数据表,数据列设定不同的字符集，设定方法 :

- 创建时通过命令来设置 , 如 : CREATE TABLE 表名( .... )CHARSET = utf8;
- 如无设定 , 则根据MySQL数据库配置文件 my.ini 中的参数设定，这样子导入其他电脑的SQL文件会出现乱码



### 修改数据库

> 修改表 ( ALTER TABLE )

修改表名 :ALTER  TABLE  旧表名  RENAME  AS  新表名

添加字段 : ALTER  TABLE  表名  ADD  字段名  列属性[属性]

修改字段 :

- ALTER  TABLE  表名  MODIFY  字段名  列类型[属性]        ==修改属性约束==
- ALTER  TABLE  表名  CHANGE  旧字段名  新字段名  列属性[属性]     ==字段重命名==和修改约束

删除字段 :  ALTER  TABLE  表名  DROP  字段名

> 删除数据表

语法：DROP  TABLE  [IF EXISTS]  表名

- IF EXISTS为可选 , 判断是否存在该数据表
- 如删除不存在的数据表会抛出错误

> 其他

- 可用反引号（`）为标识符（库名、表名、字段名、索引、别名）包裹，以==避免与关键字重名！中文也可以作为标识符！==
2. 每个库目录存在一个保存当前数据库的选项文件db.opt。

3. 注释：
  单行注释 # 注释内容
  多行注释 /* 注释内容 */
  单行注释 -- 注释内容       (标准SQL注释风格，要求双破折号后加一空格符（空格、TAB、换行等）)
  
4. 模式通配符：
  _   任意单个字符
  %   任意多个字符，甚至包括零字符
  单引号需要进行转义  \ \'
  
5. CMD命令行内的语句结束符可以为 ";", "\G", "\g"，仅影响显示结果。其他地方还是用分号结束。delimiter 可修改当前对话的语句结束符。

6. SQL对大小写不敏感 （关键字）

7. 清除已有语句：\c

## 外键

> 外键概念

如果公共关键字在一个关系中是主关键字，那么这个公共关键字被称为另一个关系的外键。由此可见，外键表示了两个关系之间的相关联系。以另一个关系的外键作主关键字的表被称为**主表**，具有此外键的表被称为主表的**从表**。

在实际操作中，将一个表的值放入第二个表来表示关联，所使用的值是第一个表的主键值(在必要时可包括复合主键值)。此时，第二个表中保存这些值的属性称为外键(**foreign key**)。

**外键作用**

保持数据**一致性**，**完整性**，主要目的是控制存储在外键表中的数据,**约束**。使两张表形成关联，外键只能引用外表中的列的值或使用空值。

> 创建外键

建表时指定外键约束

```mysql
-- 创建外键的方式一 : 创建子表同时创建外键

-- 年级表 (id\年级名称)
CREATE TABLE `grade` (
`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级ID',
`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
PRIMARY KEY (`gradeid`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

-- 学生信息表 (学号,姓名,性别,年级,手机,地址,出生日期,邮箱,身份证号)
CREATE TABLE `student` (
`studentno` INT(4) NOT NULL COMMENT '学号',
`studentname` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`sex` TINYINT(1) DEFAULT '1' COMMENT '性别',
`gradeid` INT(10) DEFAULT NULL COMMENT '年级',
`phoneNum` VARCHAR(50) NOT NULL COMMENT '手机',
`address` VARCHAR(255) DEFAULT NULL COMMENT '地址',
`borndate` DATETIME DEFAULT NULL COMMENT '生日',
`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
`idCard` VARCHAR(18) DEFAULT NULL COMMENT '身份证号',
PRIMARY KEY (`studentno`),
KEY `FK_gradeid` (`gradeid`),
CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
```

建表后修改

```mysql
-- 创建外键方式二 : 创建子表完毕后,修改子表添加外键
ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`);
```

> 删除外键

操作：删除 grade 表，发现报错

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200719182531" alt="img" style="zoom:67%;" />

**注意** : 删除具有主外键关系的表时 , 要先删（依赖）子表 , 后删（被依赖）主表

```mysql
-- 删除外键
ALTER TABLE student DROP FOREIGN KEY FK_gradeid;
-- 发现执行完上面的,索引还在,所以还要删除索引
-- 注:这个索引是建立外键的时候默认生成的
ALTER TABLE student DROP INDEX FK_gradeid;
```

以上操作都是物理外键，数据库级别的外键，不建议使用（避免数据库过多造成困扰）

==最佳实践==

- 数据库就是单纯的表，只用来存储数据，只有行（数据）和列（字段）

- 我们要使用多张表的数据，想用外键（程序去实现）

  

## DML语言

**数据库意义** ： 数据存储、数据管理

**管理数据库数据方法：**

- 通过SQLyog等管理工具管理数据库数据
- 通过**DML语句**管理数据库数据

**DML语言**  ：数据操作语言

- 用于操作数据库对象中所包含的数据

- 包括 :

- - INSERT (添加数据语句)
  - UPDATE (更新数据语句)
  - DELETE (删除数据语句)



### 添加数据

> INSERT命令

**语法：**

```sql
INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1','值2','值3')
```

**注意 :** 

- 字段或值之间用英文逗号隔开 .
- ' 字段1,字段2...' 该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一致 .
- 可同时插入多条数据 , values 后用英文逗号隔开 .

```mysql
-- 使用语句如何增加语句?
-- 语法 : INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1','值2','值3')
INSERT INTO grade(gradename) VALUES ('大一');

-- 主键自增,那能否省略呢?
INSERT INTO grade VALUES ('大二');

-- 查询:INSERT INTO grade VALUE ('大二')错误代码：1136
Column count doesn`t match value count at row 1

-- 结论:'字段1,字段2...'该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一致，一一对应。

-- 一次插入多条数据
INSERT INTO grade(gradename) VALUES ('大三'),('大四');
```

==如果有外键，被依赖的表必须先有数据，依赖的表才可以插入数据，比如依赖的表要插入grade-id = 1，被依赖的表必须要有 grade-id = 1的数据。==



### 修改数据

> update命令

语法：

```mysql
UPDATE 表名 SET column_name = value [,column_name2=value2,...] [WHERE condition];
```

**注意 :** 

- column_name 为要更改的数据列，多个用英文逗号隔开

- value 为修改后的数据 , 可以为变量 , 具体值 , 表达式或者嵌套的SELECT结果

  ~~~mysql
  update `student` set `birth`= current_time() where `id`>1 and `id`<8;
  ~~~

  ==current_time()为变量==

- condition 为筛选条件 , 如不指定则修改该表的所有列数据

  > 关闭和开启安全模式

~~~mysql
SET SQL_SAFE_UPDATES = 0;  -- 关闭安全模式

SET SQL_SAFE_UPDATES = 1;  -- 开启安全模式
~~~

> where条件子句

可以简单的理解为 : 有条件地从表中筛选数据

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182548)测试：

```mysql
-- 修改年级信息
UPDATE grade SET gradename = '高中' WHERE gradeid = 1;
```



### 删除数据

> DELETE命令

语法：

```mysql
DELETE FROM 表名 [WHERE condition];
```

注意：condition为筛选条件 , 如不指定则删除该表的所有列数据

```mysql
-- 删除某一个数据
DELETE FROM grade WHERE gradeid = ？
```



> TRUNCATE命令

作用：用于完全清空表数据 , 但表结构 , 索引 , 约束等不变 ;

语法：

```mysql
TRUNCATE [TABLE] table_name;

-- 清空年级表
TRUNCATE grade
```

**注意：区别于DELETE命令**

- 相同 : 都能删除数据 , 不删除表结构 , 但TRUNCATE速度更快

- ==不同== :

- - 使用TRUNCATE TABLE 重新设置AUTO_INCREMENT计数器
  - 使用TRUNCATE TABLE不会对事务有影响 （事务后面会说）

测试：

```mysql
-- 创建一个测试表
CREATE TABLE `test` (
`id` INT(4) NOT NULL AUTO_INCREMENT,
`coll` VARCHAR(20) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

-- 插入几个测试数据
INSERT INTO test(coll) VALUES('row1'),('row2'),('row3');

-- 删除表数据(不带where条件的delete)
DELETE FROM test;
-- 结论:如不指定Where则删除该表的所有列数据,自增当前值依然从原来基础上进行,会记录日志.

-- 删除表数据(truncate)
TRUNCATE TABLE test;
-- 结论:truncate删除数据,自增当前值会恢复到初始值重新开始;不会记录日志.

-- 同样使用DELETE清空不同引擎的数据库表数据.重启数据库服务后
-- InnoDB : 自增列从初始值重新开始 (因为是存储在内存中,断电即失)
-- MyISAM : 自增列依然从上一个自增数据基础上开始 (存在文件中,不会丢失)
```

了解即可，==delete 删除后，重启数据库==，现象

- InnoDB     自增列会重新从1开始  （存在内存当中，断电即失）

- MyISAM    继续从上一个自增量开始（存在文件中，不会丢失）

  

## DQL语言

**DQL( Data Query Language 数据查询语言 )**

- 查询数据库数据 , 如**SELECT**语句
- 简单的单表查询或多表的复杂查询和嵌套查询
- 是数据库语言中最核心,最重要的语句
- 使用频率最高的语句

> SELECT语法

```mysql
SELECT [ALL | DISTINCT]
{ * | table.* | [table.field1 [as 别名] [,table.field2 [as 别名]] [,...]] }
FROM table_name [as 别名]
  [left | right | inner join table_name2  on....]  -- 联合查询
  [WHERE ...]  -- 指定结果需满足的条件
  [GROUP BY ...]  -- 指定结果按照哪几个字段来分组
  [HAVING]  -- 过滤分组的记录必须满足的次要条件
  [ORDER BY ...]  -- 指定查询记录按一个或多个条件排序
  [LIMIT {[offset,]row_count | row_countOFFSET offset}];
  -- offset 起始数据行  row_count 行数
   -- 指定查询的记录从哪条至哪条
```

**注意 : [ ] 括号代表可选 , { }括号代表必选，条件限制 where 和 having 等语句顺序只能按照上面的顺序写**



### 指定查询字段

```mysql
-- 查询表中所有的数据列结果 , 采用 **" \* "** 符号; 但是效率低，不推荐 .

-- 查询所有学生信息
SELECT * FROM student;

-- 查询指定列(学号 , 姓名)
SELECT studentno,studentname FROM student;
```

> AS 子句作为别名

作用：

- 可给数据列取一个新别名
- 可给表取一个新别名
- 可把经计算或总结的结果用另一个新名称来代替

```mysql
-- 这里是为列取别名(当然as关键词可以省略)
SELECT studentno AS 学号,studentname AS 姓名 FROM student;

-- 使用as也可以为表取别名
SELECT studentno AS 学号,studentname AS 姓名 FROM student AS s;

-- 使用as,为查询结果取一个新名字
-- CONCAT()函数拼接字符串
SELECT CONCAT('姓名:',studentname) AS 新姓名 FROM student;
```

> DISTINCT关键字的使用

作用 : 去掉SELECT查询返回的记录结果中重复的记录 ( 返回所有列的值都相同 ) , 只返回一条

```mysql
-- # 查看哪些同学参加了考试(学号) 去除重复项
SELECT * FROM result; -- 查看考试成绩
SELECT studentno FROM result; -- 查看哪些同学参加了考试
SELECT DISTINCT studentno FROM result; -- 了解:DISTINCT 去除重复项 , (默认是ALL)
```

> 使用表达式的列

数据库中的表达式 : 一般由文本值 , 列值 , NULL , 函数和操作符等组成

应用场景 :

- SELECT语句返回结果列中使用

- SELECT语句中的ORDER BY , HAVING等子句中使用

- DML语句中的 where 条件语句中使用表达式

  ```mysql
  -- selcet查询中可以使用表达式
  SELECT @@auto_increment_increment; -- 查询自增步长
  SELECT VERSION(); -- 查询版本号
  SELECT 100*3-1 AS 计算结果; -- 表达式
  
  -- 学员考试成绩集体提分一分查看
  SELECT studentno,StudentResult+1 AS '提分后' FROM result;
  ```

- 避免SQL返回结果中包含 ' . ' , ' * ' 和括号等干扰开发语言程序.



### where条件语句

作用：用于检索数据表中 符合条件 的记录

搜索条件可由一个或多个逻辑表达式组成 , 结果一般为真或假.

> 逻辑操作符

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182555)

测试

```mysql
-- 满足条件的查询(where)
SELECT Studentno,StudentResult FROM result;

-- 查询考试成绩在95-100之间的
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult>=95 AND StudentResult<=100;

-- AND也可以写成 &&
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult>=95 && StudentResult<=100;

-- 模糊查询(对应的词:精确查询)
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult BETWEEN 95 AND 100;

-- 除了1000号同学,要其他同学的成绩
SELECT studentno,studentresult
FROM result
WHERE studentno!=1000;

-- 使用NOT
SELECT studentno,studentresult
FROM result
WHERE NOT studentno=1000;
```

> 模糊查询 ：比较操作符

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182559)

注意：

- 数值数据类型的记录之间才能进行算术运算 ;
- 相同数据类型的数据之间才能进行比较 ;

测试：

```mysql
-- 模糊查询 between and \ like \ in \ null

-- =============================================
-- LIKE
-- =============================================
-- 查询姓刘的同学的学号及姓名
-- like结合使用的通配符 : % (代表0到任意个字符) _ (一个字符)
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘%';

-- 查询姓刘的同学,后面只有一个字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘_';

-- 查询姓刘的同学,后面只有两个字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘__';

-- 查询姓名中含有 嘉 字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '%嘉%';

-- 查询姓名中含有特殊字符的需要使用转义符号 '\'
-- 自定义转义符关键字: ESCAPE ':'

-- =============================================
-- IN
-- =============================================
-- 查询学号为1000,1001,1002的学生姓名
SELECT studentno,studentname FROM student
WHERE studentno IN (1000,1001,1002);

-- 查询地址在北京,南京,河南洛阳的学生
SELECT studentno,studentname,address FROM student
WHERE address IN ('北京','南京','河南洛阳');

-- =============================================
-- NULL 空
-- =============================================
-- 查询出生日期没有填写的同学
-- 不能直接写=NULL , 这是代表错误的 , 用 is null
SELECT studentname FROM student
WHERE BornDate IS NULL;

-- 查询出生日期填写的同学
SELECT studentname FROM student
WHERE BornDate IS NOT NULL;

-- 查询没有写家庭住址的同学(空字符串不等于null)
SELECT studentname FROM student
WHERE Address='' OR Address IS NULL;
```

==没有填写数据为null，查询这样的数据时不能直接写=NULL , 这是代表错误的 , 用 is null==

### 连接查询

> JOIN 对比

| 操作符名称  | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| INNER JOIN  | 如果表中有至少一个匹配，则返回匹配的数据                     |
| LEFT   JOIN | （以左表为基准）如果有的数据右表中没有匹配的数据，则以null填充，左表返回所有的行 |
| RIGHT JOIN  | （以右表为基准）如果有的数据左表中没有匹配的数据，则以null填充，右表返回所有的行 |

七种Join：

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200719182604" alt="img" style="zoom:150%;" />

测试

```mysql
/*
连接查询
   如需要多张数据表的数据进行查询,则可通过连接运算符实现多个查询
内连接 inner join
   查询两个表中的结果集中的交集
外连接 outer join
   左外连接 left join
       (以左表作为基准,右边表来一一匹配,匹配不上的,返回左表的记录,右表以NULL填充)
   右外连接 right join
       (以右表作为基准,左边表来一一匹配,匹配不上的,返回右表的记录,左表以NULL填充)
       
等值连接和非等值连接

自连接
*/

-- 查询参加了考试的同学信息(学号,学生姓名,科目编号,分数)
SELECT * FROM student;
SELECT * FROM result;

/*思路:
(1):分析需求,确定查询的列来源于两个类,student result,连接查询
(2):确定使用哪种连接查询?(内连接)
*/
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno

-- 右连接(也可实现)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
RIGHT JOIN result r
ON r.studentno = s.studentno

-- 等值连接
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s , result r
WHERE r.studentno = s.studentno

-- 左连接 (查询了所有同学,不考试的也会查出来)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno

-- 查一下缺考的同学(左连接应用场景)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno
WHERE StudentResult IS NULL

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r 
-- 此处用 LEFT JOIN  RIGHT JOIN 都行，因为都是要与 subject 表进行连接的，结果都一样
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno
```

> 自连接

```mysql
/*
自连接
   数据表与自身进行连接

需求:从一个包含栏目ID , 栏目名称和父栏目ID的表中
    查询父栏目名称和其他子栏目名称
*/

-- 创建一个表
CREATE TABLE `category` (
`categoryid` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主题id',
`pid` INT(10) NOT NULL COMMENT '父id',
`categoryName` VARCHAR(50) NOT NULL COMMENT '主题名字',
PRIMARY KEY (`categoryid`)
) ENGINE=INNODB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8

-- 插入数据
INSERT INTO `category` (`categoryid`, `pid`, `categoryName`)
VALUES('2','1','信息技术'),
('3','1','软件开发'),
('4','3','数据库'),
('5','1','美术设计'),
('6','3','web开发'),
('7','5','ps技术'),
('8','2','办公信息');

-- 编写SQL语句,将栏目的父子关系呈现出来 (父栏目名称,子栏目名称)
-- 核心思想:把一张表看成两张一模一样的表,然后将这两张表连接查询(自连接)
SELECT a.categoryName AS '父栏目',b.categoryName AS '子栏目'
FROM category AS a,category AS b
WHERE a.`categoryid`=b.`pid`

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno

-- 查询学员及所属的年级(学号,学生姓名,年级名)
SELECT studentno AS 学号,studentname AS 学生姓名,gradename AS 年级名称
FROM student s
INNER JOIN grade g
ON s.`GradeId` = g.`GradeID`

-- 查询科目及所属的年级(科目名称,年级名称)
SELECT subjectname AS 科目名称,gradename AS 年级名称
FROM SUBJECT sub
INNER JOIN grade g
ON sub.gradeid = g.gradeid

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
```



### 排序和分页

测试

```mysql
/*============== 排序 ================
语法 : ORDER BY
   ORDER BY 语句用于根据指定的列对结果集进行排序。
   ORDER BY 语句默认按照ASC升序对记录进行排序。
   如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。
   
*/

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
-- 按成绩降序排序
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC

/*============== 分页 ================
语法 : SELECT * FROM table LIMIT [offset,] rows | rows OFFSET offset
好处 : (用户体验,网络传输,查询压力)

推导:
   第一页 : limit 0,5
   第二页 : limit 5,5
   第三页 : limit 10,5
   ......
   第N页 : limit (pageNo-1)*pageSzie,pageSzie
   [pageNo:页码,pageSize:单页面显示条数]
   
*/

-- 每页显示5条数据
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC 
LIMIT 0,5

-- 查询 JAVA第一学年 课程成绩前10名并且分数大于80的学生信息(学号,姓名,课程名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='JAVA第一学年'
ORDER BY StudentResult DESC
LIMIT 0,10
```



### 子查询

```mysql
/*============== 子查询 ================
什么是子查询?
   在查询语句中的WHERE条件子句中,又嵌套了另一个查询语句
   嵌套查询可由多个子查询组成,求解的方式是由里及外;
   子查询返回的结果一般都是集合,故而建议使用IN关键字;
*/

-- 查询 数据库结构-1 的所有考试结果(学号,科目编号,成绩),并且成绩降序排列
-- 方法一:使用连接查询
SELECT studentno,r.subjectno,StudentResult
FROM result r
INNER JOIN `subject` sub
ON r.`SubjectNo`=sub.`SubjectNo`
WHERE subjectname = '数据库结构-1'
ORDER BY studentresult DESC;

-- 方法二:使用子查询(执行顺序:由里及外)
SELECT studentno,subjectno,StudentResult
FROM result
WHERE subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '数据库结构-1'
)
ORDER BY studentresult DESC;

-- 查询课程为 高等数学-2 且分数不小于80分的学生的学号和姓名
-- 方法一:使用连接查询
SELECT s.studentno,studentname
FROM student s
INNER JOIN result r
ON s.`StudentNo` = r.`StudentNo`
INNER JOIN `subject` sub
ON sub.`SubjectNo` = r.`SubjectNo`
WHERE subjectname = '高等数学-2' AND StudentResult>=80

-- 方法二:使用连接查询+子查询
-- 分数不小于80分的学生的学号和姓名
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80

-- 在上面SQL基础上,添加需求:课程为 高等数学-2
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80 AND subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '高等数学-2'
)

-- 方法三:使用子查询
-- 分步写简单sql语句,然后将其嵌套起来
SELECT studentno,studentname FROM student WHERE studentno IN(
   SELECT studentno FROM result WHERE StudentResult>=80 AND subjectno=(
       SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'
  )
)
```

### 分组和过滤

~~~mysql
 -- 查询不同课程的平均分,最高分,最低分
 -- 前提:根据不同的课程进行分组
 
 SELECT subjectname,AVG(studentresult) AS 平均分,MAX(StudentResult) AS 最高分,MIN(StudentResult) AS 最低分
 FROM result AS r
 INNER JOIN `subject` AS s
 ON r.subjectno = s.subjectno
 GROUP BY r.subjectno  -- 这里也可以用 sub.subjectno 
 HAVING 平均分>80;
 
 /*
 where写在group by前面.
分组后面的筛选，要使用HAVING..
 因为having是从前面筛选的字段再筛选，而where是从数据表中的>字段直接进行筛选的
 */
~~~



## 常用函数

**数据函数**

```mysql
 SELECT ABS(-8);  /*绝对值*/
 SELECT CEILING(9.4); /*向上取整*/
 SELECT FLOOR(9.4);   /*向下取整*/
 SELECT RAND();  /*随机数,返回一个0-1之间的随机数，包含0，不包含1*/
 SELECT SIGN(0); /*符号函数: 负数返回-1,正数返回1,0返回0*/
```

**字符串函数**

```mysql
 SELECT CHAR_LENGTH('狂神说坚持就能成功'); /*返回字符串包含的字符数*/
 SELECT CONCAT('我','爱','程序');  /*合并字符串,参数可以有多个*/
 SELECT INSERT('我爱编程helloworld',1,2,'超级热爱');  /*这里的1是第一个字符，替换字符串,从某个位置开始替换某个长度*/
 SELECT LOWER('KuangShen'); /*小写*/
 SELECT UPPER('KuangShen'); /*大写*/
 SELECT LEFT('hello,world',5);   /*从左边第一个字符截取，长度*/
 SELECT RIGHT('hello,world',5);  /*从右边第一个字符截取，长度*/
 SELECT REPLACE('狂神说坚持就能成功','坚持','努力');  /*替换字符串*/
 SELECT SUBSTR('狂神说坚持就能成功',4,6); /*截取字符串,开始和长度*/
 SELECT REVERSE('狂神说坚持就能成功'); /*反转
 
 -- 查询姓周的同学,改成邹
 SELECT REPLACE(studentname,'周','邹') AS 新名字
 FROM student WHERE studentname LIKE '周%';
```

**日期和时间函数**

```mysql
 SELECT CURRENT_DATE();   /*获取当前日期*/
 SELECT CURDATE();   /*获取当前日期*/
 SELECT NOW();   /*获取当前日期和时间*/
 SELECT LOCALTIME();   /*获取当前日期和时间*/
 SELECT SYSDATE();   /*获取当前日期和时间*/
 
 -- 获取年月日,时分秒
 SELECT YEAR(NOW());
 SELECT MONTH(NOW());
 SELECT DAY(NOW());
 SELECT HOUR(NOW());
 SELECT MINUTE(NOW());
 SELECT SECOND(NOW());
```

**系统信息函数**

```mysql
 SELECT VERSION();  /*版本*/
 SELECT USER();     /*用户*/
```



### 聚合函数（常用）

| 函数名称 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| COUNT()  | 返回满足Select条件的记录总和数，如 select count(*) 【不建议使用 *，效率低】 |
| SUM()    | 返回数字字段或表达式列作统计，返回一列的总和。               |
| AVG()    | 通常为数值字段或表达列作统计，返回一列的平均值               |
| MAX()    | 可以为数值字段，字符字段或表达式列作统计，返回最大的值。     |
| MIN()    | 可以为数值字段，字符字段或表达式列作统计，返回最小的值。     |

```mysql
 -- 聚合函数
 /*COUNT:非空的*/
 SELECT COUNT(studentname) FROM student;
 SELECT COUNT(*) FROM student;
 SELECT COUNT(1) FROM student;  /*推荐*/
 
 -- 从含义上讲，count(1) 与 count(*) 都表示对全部数据行的查询。
 -- count(字段) 会统计该字段在表中出现的次数，忽略字段为null 的情况。即不统计字段为null 的记录。
 -- count(*) 包括了所有的列，相当于行数，在统计结果的时候，包含字段为null 的记录；
 -- count(1) 用1代表代码行，在统计结果的时候，包含字段为null 的记录 。
 /*
 很多人认为count(1)执行的效率会比count(*)高，原因是count(*)会存在全表扫描，而count(1)可以针对一个字段进行查询。其实不然，count(1)和count(*)都会对全表进行扫描，统计所有记录的条数，包括那些为null的记录，因此，它们的效率可以说是相差无几。而count(字段)则与前两者不同，它会统计该字段不为null的记录条数。
 
 下面它们之间的一些对比：
 
 1）列名不为主键时，count(1) 比 count(列名)快
 2）有主键时，主键作为计算条件，count(主键) 效率最高；
 3）表有多列并且没有主键，cout（1）比 count（*）效率高
 4）若表格只有一个字段，则 count(*) 效率较高。
 */
 
 SELECT SUM(StudentResult) AS 总和 FROM result;
 SELECT AVG(StudentResult) AS 平均分 FROM result;
 SELECT MAX(StudentResult) AS 最高分 FROM result;
 SELECT MIN(StudentResult) AS 最低分 FROM result;
```

### MD5 加密

**一、MD5简介**

MD5即Message-Digest Algorithm 5（信息-摘要算法5），用于确保信息传输完整一致。是计算机广泛使用的杂凑算法之一（又译摘要算法、哈希算法），主流编程语言普遍已有MD5实现。将数据（如汉字）运算为另一固定长度值，是杂凑算法的基础原理，MD5的前身有MD2、MD3和MD4。

**二、实现数据加密**

新建一个表 testmd5

```mysql
 CREATE TABLE `testmd5` (
  `id` INT(4) NOT NULL,
  `name` VARCHAR(20) NOT NULL,
  `pwd` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`id`)
 ) ENGINE=INNODB DEFAULT CHARSET=utf8
```

插入一些数据

```sql
 INSERT INTO testmd5 VALUES(1,'kuangshen','123456'),(2,'qinjiang','456789')
```

如果我们要对pwd这一列数据进行加密，语法是：

```mysql
 update testmd5 set pwd = md5(pwd);
```

如果单独对某个用户(如kuangshen)的密码加密：

```sql
 INSERT INTO testmd5 VALUES(3,'kuangshen','123456')
 update testmd5 set pwd = md5(pwd) where name = 'kuangshen';
```

插入新的数据自动加密

```sql
 INSERT INTO testmd5 VALUES(4,'kuangshen3',md5('123456'));
```

查询登录用户信息（md5对比使用，查看用户输入加密后的密码进行比对）

```sql
 SELECT * FROM testmd5 WHERE `name`='kuangshen' AND pwd=MD5('123456');
```

每次加密后值都会变，可以在加密的基础上再次加密，如果同一个密码，一个加密一次 ，一个加密两次，这两个的加密值都是不一样的，数据校验（查询） 校验的是加密后的值是否相同。

### 小结

```mysql
 -- ================ 内置函数 ================
 -- 数值函数
 abs(x)            -- 绝对值 abs(-10.9) = 10
 format(x, d)    -- 格式化千分位数值 format(1234567.456, 2) = 1,234,567.46
 ceil(x)            -- 向上取整 ceil(10.1) = 11
 floor(x)        -- 向下取整 floor (10.1) = 10
 round(x)        -- 四舍五入去整
 mod(m, n)        -- m%n m mod n 求余 10%3=1
 pi()            -- 获得圆周率
 pow(m, n)        -- m^n
 sqrt(x)            -- 算术平方根
 rand()            -- 随机数
 truncate(x, d)    -- 截取d位小数
 
 -- 时间日期函数
 now(), current_timestamp();     -- 当前日期时间
 current_date();                    -- 当前日期
 current_time();                    -- 当前时间
 date('yyyy-mm-dd hh:ii:ss');    -- 获取日期部分
 time('yyyy-mm-dd hh:ii:ss');    -- 获取时间部分
 date_format('yyyy-mm-dd hh:ii:ss', '%d %y %a %d %m %b %j');    -- 格式化时间
 unix_timestamp();                -- 获得unix时间戳
 from_unixtime();                -- 从时间戳获得时间
 
 -- 字符串函数
 length(string)            -- string长度，字节
 char_length(string)        -- string的字符个数
 substring(str, position [,length])        -- 从str的position开始,取length个字符
 replace(str ,search_str ,replace_str)    -- 在str中用replace_str替换search_str
 instr(string ,substring)    -- 返回substring首次在string中出现的位置
 concat(string [,...])    -- 连接字串
 charset(str)            -- 返回字串字符集
 lcase(string)            -- 转换成小写
 left(string, length)    -- 从string2中的左边起取length个字符
 load_file(file_name)    -- 从文件读取内容
 locate(substring, string [,start_position])    -- 同instr,但可指定开始位置
 lpad(string, length, pad)    -- 重复用pad加在string开头,直到字串长度为length
 ltrim(string)            -- 去除前端空格
 repeat(string, count)    -- 重复count次
 rpad(string, length, pad)    --在str后用pad补充,直到长度为length
 rtrim(string)            -- 去除后端空格
 strcmp(string1 ,string2)    -- 逐字符比较两字串大小
 
 -- 聚合函数
 count()
 sum();
 max();
 min();
 avg();
 group_concat()
 
 -- 其他常用函数
 md5();
 default();
```

## 视图

**什么是视图：**
   视图是一个虚拟表，其内容由查询定义。同真实的表一样，视图包含一系列带有名称的列和行数据。但是，==视图并不在数据库中以存储的数据值集形式存在==。行和列数据来自定义视图的查询所引用的表，并且在引用视图时动态生成。
   ==视图具有表结构文件，但不存在数据文件。==
   对其中所引用的基础表来说，视图的作用类似于筛选。定义视图的筛选可以来自当前或其它数据库的一个或多个表，或者其它视图。通过视图进行查询没有任何限制，通过它们进行数据修改时的限制也很少。
   视图是存储在数据库中的查询的sql语句，它主要出于两种原因：安全原因，视图可以隐藏一些数据，如：社会保险基金表，可以用视图只显示姓名，地址，而不显示社会保险号和工资数等，另一原因是可使复杂的查询易于理解和使用。

~~~mysql
-- 创建视图
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}] VIEW view_name [(column_list)] AS select_statement
   \- 视图名必须唯一，同时不能与表重名。
   \- 视图可以使用 select 语句查询到的列名，也可以自己指定相应的列名。
   \- 可以指定视图执行的算法，通过 ALGORITHM 指定。
   \- column_list 如果存在，则数目必须等于SELECT语句检索的列数
-- 查看结构
   SHOW CREATE VIEW view_name
-- 删除视图
   \- 删除视图后，数据依然存在。
   \- 可同时删除多个视图。
   DROP VIEW [IF EXISTS] view_name ...
-- 修改视图结构
   \- 一般不修改视图，因为不是所有的更新视图都会映射到表上。
   ALTER VIEW view_name [(column_list)] AS select_statement
-- 视图作用
   \1. 简化业务逻辑
   \2. 对客户端隐藏真实的表结构
-- 视图算法(ALGORITHM)
   MERGE    合并
     将视图的查询语句，与外部查询需要先合并再执行！
   TEMPTABLE  临时表
     将视图执行完毕后，形成临时表，再做外层查询！
   UNDEFINED  未定义(默认)，指的是MySQL自主去选择相应的算法。
~~~



## 事务

> 什么是事务

- 事务就是将一组SQL语句放在同一批次内去执行
- 如果一个SQL语句出错,则该批次内的所有SQL都将被取消执行
- MySQL事务处理只支持InnoDB和BDB数据表类型

> 事务的ACID原则  百度 ACID

**原子性(Atomic)**

- 整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执行过程中发生错误，会被回滚（ROLLBACK）到事务开始前的状态，就像这个事务从来没有执行过一样。

**一致性(Consist)**

- 一个事务可以封装状态改变（除非它是一个只读的）。事务必须始终保持系统处于一致的状态，不管在任何给定的时间并发事务有多少。也就是说：如果事务是并发多个，系统也必须如同串行事务一样操作。其主要特征是保护性和不变性(Preserving an Invariant)，以转账案例为例，假设有五个账户，每个账户余额是100元，那么五个账户总额是500元，如果在这个5个账户之间同时发生多个转账，无论并发多少个，比如在A与B账户之间转账5元，在C与D账户之间转账10元，在B与E之间转账15元，五个账户总额也应该还是500元，这就是保护性和不变性。

**隔离性(Isolated)**

- 隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相同的时间内，执行相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系统。这种属性有时称为串行化，为了防止事务操作间的混淆，必须串行化或序列化请求，使得在同一时间仅有一个请求用于同一数据。

**持久性(Durable)**

- 在事务完成以后，该事务对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

  

> 并发事务带来的几个问题：更新丢失，脏读，不可重复读，幻读。

- 更新丢失：==多个事务对同一数据进行修改，最后只保留了最后修改的值，其他事务修改的值都被覆盖了==。

- 脏读：指一个事务读取了另外一个事务==已修改但未提交==的数据。

- 不可重复读：在一个事务内读取表中的某一行数据，多次读取结果不同。==读取到了另一个事务提交的修改数据，不符合隔离性==（这个不一定是错误，只是某些场合不对）

- 虚读(幻读)：是指在一个事务内==读取到了别的事务新增插入的数据==，导致前后读取不一致。
  （一般是行影响，多了一行）

  

事务隔离级别：未提交读(Read uncommitted)，已提交读(Read committed)，可重复读(Repeatable read)，可序列化(Serializable)

```mysql
select @@global.transaction_isolation,@@transaction_isolation; -- 查询隔离级别
-- @@global.transaction_isolation 全局事务隔离级别
-- @@transaction_isolation 当前表的事务隔离级别
```

四种隔离级别的比较

| 读数据一致性及并发副作用 隔离级别 | 读数据一致性                   | 脏读 | 不可重复读 | 幻读 |
| --------------------------------- | ------------------------------ | ---- | ---------- | ---- |
| 未提交读(read uncommitted)        | 最低级别，不读物理上顺环的数据 | 是   | 是         | 是   |
| 已提交读(read committed)          | 语句级                         | 否   | 是         | 是   |
| 可重复读(Repeatable red)          | 事务级                         | 否   | 否         | 是   |
| 可序列化(Serializable)            | 最高级别，事务级               | 否   | 否         | 否   |

==MySQL InnoDB的事务隔离级别为 可重复读(Repeatable red)  ，而MySQL 8.0 以上已经可以解决幻读问题，但是隔离级别还是 可重复读(Repeatable red) 。==

> **基本语法**

```mysql
-- 使用set语句来改变自动提交模式
SET autocommit = 0;   /*关闭*/
SET autocommit = 1;   /*开启*/

-- 注意:
--- 1.MySQL中默认是自动提交
--- 2.使用事务时应先关闭自动提交

-- 开始一个事务,标记事务的起始点
START TRANSACTION  

-- 提交一个事务给数据库，commit 一次就提交一次事务
COMMIT

-- 将事务回滚,数据回到本次事务的初始状态
ROLLBACK

-- 还原MySQL数据库的自动提交
SET autocommit =1;

-- 保存点
SAVEPOINT 保存点名称 -- 设置一个事务保存点
ROLLBACK TO SAVEPOINT 保存点名称 -- 回滚到保存点
RELEASE SAVEPOINT 保存点名称 -- 删除保存点
```

> 测试

```mysql
/*
课堂测试题目

A在线买一款价格为500元商品,网上银行转账.
A的银行卡余额为2000,然后给商家B支付500.
商家B一开始的银行卡余额为10000

创建数据库shop和创建表account并插入2条数据
*/

CREATE DATABASE `shop`CHARACTER SET utf8 COLLATE utf8_general_ci;
USE `shop`;

CREATE TABLE `account` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`name` VARCHAR(32) NOT NULL,
`cash` DECIMAL(9,2) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO account (`name`,`cash`)
VALUES('A',2000.00),('B',10000.00)

-- 转账实现
SET autocommit = 0; -- 关闭自动提交
START TRANSACTION;  -- 开始一个事务,标记事务的起始点
UPDATE account SET cash=cash-500 WHERE `name`='A';
UPDATE account SET cash=cash+500 WHERE `name`='B';
COMMIT; -- 提交事务
# rollback;
SET autocommit = 1; -- 恢复自动提交
```



## 索引

了解索引结构（参考博客）： https://blog.codinglabs.org/articles/theory-of-mysql-index.html

索引就是排好序的快速查找数据结构 。

在数据库之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构，就是索引。

> 索引的优势

- ==提高查询速度==
- 确保数据的唯一性
- 可以加速表和表之间的连接 , 实现表与表之间的参照完整性
- 使用分组和排序子句进行数据检索时 , 可以显著==减少分组和排序的时间==
- 全文检索字段进行搜索优化.

> 索引的劣势

- 实际上索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录，所以索引列也是要占用空间的
- 虽然索引大大提高了查询的速度，同时也会降低更新表的速度，如 insert， update，delete 。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件每次更新添加了索引列的字段，都会调整因为更新所带来的键值变化后的索引信息。

> 索引准则

- 索引不是越多越好，一般建议5个
- 不要对经常变动的数据加索引
- 小数据量的表建议不要加索引
- 如果某个数据列包含许多重复内容，为它建立索引没有太大的实际效果
- 高并发下推荐使用复合索引
- 索引一般应加在频繁作为查找条件的字段
- 查询中与其他表关联的字段
- where 条件里用不到的字段不建立索引
- 排序字段建立索引，大大提高排序速度
- 查询中统计或者分组的字段

> 索引的数据结构

```mysql
-- 我们可以在创建上述索引的时候，为其指定索引类型，分两类
hash类型的索引：查询单条快，范围查询慢
btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它）

-- 不同的存储引擎支持的索引类型也不一样
InnoDB 支持事务，支持行级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
MyISAM 不支持事务，支持表级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
Memory 不支持事务，支持表级别锁定，支持 B-tree、Hash 等索引，不支持 Full-text 索引；
NDB 支持事务，支持行级别锁定，支持 Hash 索引，不支持 B-tree、Full-text 等索引；
Archive 不支持事务，支持表级别锁定，不支持 B-tree、Hash、Full-text 等索引；
```

> 分类

- 主键索引 (Primary Key)
- 唯一索引 (Unique)
- 常规索引 (Index)
- 全文索引 (FullText)

> 主键索引

主键 : 某一个属性组能唯一标识一条记录

特点 :

- 最常见的索引类型，是特殊的唯一索引
- 确保数据记录的唯一性
- 确定特定数据记录在数据库中的位置
- 不能为空

> 唯一索引

作用 : 避免同一个表中某数据列中的值重复

与主键索引的区别

- ==主键索引只能有一个，唯一索引可能有多个==
- ==主键索引不能为空值，唯一索引可以为空值==

```mysql
CREATE TABLE `Grade`(
  `GradeID` INT(11) AUTO_INCREMENT PRIMARYKEY,
  `GradeName` VARCHAR(32) NOT NULL UNIQUE
   -- 或 UNIQUE KEY `GradeID` (`GradeID`)
)
```

> 常规索引

作用 : 快速定位特定数据

注意 :

- index 和 key 关键字都可以设置常规索引
- 应加在查询找条件的字段
- ==不宜添加太多常规索引,影响数据的插入,删除和修改操作==

```mysql
CREATE TABLE `result`(
   -- 省略一些代码
  INDEX/KEY `ind` (`studentNo`,`subjectNo`) -- 创建表时添加
)
-- 创建后添加
ALTER TABLE `result` ADD INDEX `ind`(`studentNo`,`subjectNo`);
```

> 全文索引

作用场景：当查询条件为where column like ‘%xxx%’时，会让索引失效，此时全文索引便派上用场了 

作用 : 快速定位特定数据

注意 :

- ==只能用于MyISAM类型的数据表（5.6以上后 InnoDB 也支持）==
- ==只能用于CHAR , VARCHAR , TEXT数据列类型==
- 数据表越大，全文索引效果好，小表返回结果可能不理想 
- 对于较大的数据集，将数据输入一个没有FULLTEXT索引的表中，然后创建索引，其速度比把数据输入现有FULLTEXT索引的速度更为快 
- 生成全文索引是一个非常耗时且非常耗硬盘空间的做法 
- 全文索引创建速度慢，而且对有全文索引的各种数据修改操作也慢
- 少于3个字符的单词不会被包含在全文索引里，可以通过修改my.cnf的ft_min_word_len选项进行设置 

```mysql
/*
#方法一：创建表时
  　　CREATE TABLE 表名 (
               字段名1 数据类型 [完整性约束条件…],
               字段名2 数据类型 [完整性约束条件…],
               [UNIQUE | FULLTEXT | SPATIAL ]   INDEX | KEY
               [索引名] (字段名[(长度)] [ASC |DESC])
               );


#方法二：CREATE在已存在的表上创建索引
       CREATE [UNIQUE | FULLTEXT | SPATIAL ] INDEX 索引名
                    ON 表名 (字段名[(长度)] [ASC |DESC]) ;


#方法三：ALTER TABLE在已存在的表上创建索引
       ALTER TABLE 表名 ADD [UNIQUE | FULLTEXT | SPATIAL ] INDEX
                            索引名 (字段名[(长度)] [ASC |DESC]) ;
                           
                           
#删除索引：DROP INDEX 索引名 ON 表名字;
#删除主键索引: ALTER TABLE 表名 DROP PRIMARY KEY;


#显示索引信息: SHOW INDEX FROM student;
*/

/*增加全文索引*/
ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname` (`StudentName`);

/*EXPLAIN : 分析SQL语句执行性能*/
EXPLAIN SELECT * FROM student WHERE studentno='1000';

/*使用全文索引*/
-- 全文搜索通过 MATCH() 函数完成。
-- 搜索字符串作为 against() 的参数被给定。搜索以忽略字母大小写的方式执行。对于表中的每个记录行，MATCH() 返回一个相关性值。即，在搜索字符串与记录行在 MATCH() 列表中指定的列的文本之间的相似性尺度。
EXPLAIN SELECT *FROM student WHERE MATCH(studentname) AGAINST('love');

/*
开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况

MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。
测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。
*/
```

### 拓展：测试索引

**建表app_user：**

```mysql
CREATE TABLE `app_user` (
`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
`name` varchar(50) DEFAULT '' COMMENT '用户昵称',
`email` varchar(50) NOT NULL COMMENT '用户邮箱',
`phone` varchar(20) DEFAULT '' COMMENT '手机号',
`gender` tinyint(4) unsigned DEFAULT '0' COMMENT '性别（0:男；1：女）',
`password` varchar(100) NOT NULL COMMENT '密码',
`age` tinyint(4) DEFAULT '0' COMMENT '年龄',
`create_time` datetime DEFAULT CURRENT_TIMESTAMP, -- 数据插入时更新值
`update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, -- 数据插入和更新时，更新值
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表'
```

**批量插入数据：100w**

```mysql
DROP FUNCTION IF EXISTS mock_data;
DELIMITER $$  -- 要写函数必须写，设置成以 $$ 为语句结束符
CREATE FUNCTION mock_data()
RETURNS INT
BEGIN
DECLARE num INT DEFAULT 1000000;
DECLARE i INT DEFAULT 0;
WHILE i < num DO
  INSERT INTO app_user(`name`, `email`, `phone`, `gender`, `password`, `age`)
   VALUES(CONCAT('用户', i), '24736743@qq.com', CONCAT('18', FLOOR(RAND()*(999999999-100000000)+100000000)),FLOOR(RAND()*2),UUID(), FLOOR(RAND()*100));
  SET i = i + 1;
END WHILE;
RETURN i;
END $$;
SELECT mock_data(); -- 执行函数
```

不能写自定义函数，被限制了，因为我们开启了 bin-log 二进制日志, 我们就必须为我们的function指定一个参数，而我们没有。 这时执行语句   ==set global log_bin_trust_function_creators=TRUE;==就行。

**索引效率测试**

无索引

```mysql
SELECT * FROM app_user WHERE name = '用户9999'; -- 查看耗时
SELECT * FROM app_user WHERE name = '用户9999';
SELECT * FROM app_user WHERE name = '用户9999';

mysql> EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'\G
*************************** 1. row ***************************
          id: 1
select_type: SIMPLE
       table: app_user
  partitions: NULL
        type: ALL
possible_keys: NULL
        key: NULL
    key_len: NULL
        ref: NULL
        rows: 992759
    filtered: 10.00
      Extra: Using where
1 row in set, 1 warning (0.00 sec)
```

创建索引

```mysql
CREATE INDEX idx_app_user_name ON app_user(name);
```

测试普通索引

```mysql
mysql> EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'\G
*************************** 1. row ***************************
          id: 1
select_type: SIMPLE
       table: app_user
  partitions: NULL
        type: ref
possible_keys: idx_app_user_name
        key: idx_app_user_name
    key_len: 203
        ref: const
        rows: 1
    filtered: 100.00
      Extra: NULL
1 row in set, 1 warning (0.00 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.00 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.00 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.00 sec)
```

### 复合索引（联合索引）

> 复合索引

　索引可以覆盖多个数据列，如像INDEX（columnA，columnB）索引。这种索引的特点是 MySQL可以有选择地使用一个这样的索引。如果查询操作只需要用到 columnA 数据列上的一个索引，就可以使用复合索引 INDEX（columnA,columnB）。不过，这种用法仅适用于在复合索引中==排列在前的数据列组合==。比如说，INDEX（A，B，C）可以当做A或（A, B）的索引来使用，但不能当做B、C或（B，C）的索引来使用。（这里的字段组合判断只跟select的 ==where条件、group by 、order by== 字段有关）

> ==特点==

创建复合索引时，应该仔细考虑列的顺序。对索引中的所有列执行搜索或仅对前几列执行搜索时，复合索引非常有用；仅对后面的任意列执行搜索时，复合索引则没有用处。

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200719182640.png" alt="img"  />

> 复合索引的建立原则

如果您很可能仅对一个列多次执行搜索，则该列应该是复合索引中的第一列。==如果您很可能对一个两列索引中的两个列执行单独的搜索，则应该创建另一个仅包含第二列的索引。==
如上图所示，如果查询中需要对年龄和性别做查询，则应当再新建一个包含年龄和性别的复合索引。
==包含多个列的主键始终会自动以复合索引的形式创建索引，其列的顺序是它们在表定义中出现的顺序，而不是在主键定义中指定的顺序。==在考虑将来通过主键执行的搜索，确定哪一列应该排在最前面。
请注意，创建复合索引应当包含少数几个列，并且这些列经常在select查询里使用。在复合索引里包含太多的列不仅不会给带来太多好处。而且由于使用相当多的内存来存储复合索引的列的值，其后果是内存溢出和性能降低。

> 查询优化了解

复合索引对排序的优化：

复合索引只对和索引中排序相同或相反的order by 语句优化。
在创建复合索引时，每一列都定义了升序或者是降序。如定义一个复合索引：

~~~mysql
 CREATE INDEX idx_example  
 ON table1 (col1 ASC, col2 DESC, col3 ASC) 
~~~

其中 有三列分别是：col1 升序，col2 降序， col3 升序。现在如果我们执行两个查询
      1：Select col1, col2, col3 from table1 order by col1 ASC, col2 DESC, col3 ASC
       和索引顺序相同
      2：Select col1, col2, col3 from table1 order by col1 DESC, col2 ASC, col3 DESC 
       和索引顺序相反
==查询1，2 都可以被复合索引优化。==
如果查询为：
           Select col1, col2, col3 from table1 order by col1 ASC, col2 ASC, col3 ASC
==排序结果和索引完全不同时，此时的查询不会被复合索引优化。==

~~~mysql
index（a，b，c）
select * from myTest  where a=3 and b>7 and c=3;   -- b范围值，断点，阻塞了c的索引
-- a用到了，b也用到了，c没有用到，这个地方b是范围值，也算断点，只不过自身用到了索引
select * from myTest  where a>4 and b=7 and c=9;
-- a用到了  b没有使用，c没有使用
~~~

### 覆盖索引 

在了解覆盖索引之前我们先大概了解一下什么是聚集索引(主键索引)和辅助索引(二级索引)

​      **聚集索引（主键索引）**：

​      聚集索引就是按照每张表的主键构造一颗B+树，同时叶子节点中存放的即为整张表的记录数据。

​      聚集索引的叶子节点称为数据页，聚集索引的这个特性决定了索引组织表中的数据也是索引的一部分。

​      **辅助索引（二级索引）**：

​      非主键索引，叶子节点=键值+书签。Innodb 存储引擎的书签就是相应行数据的主键索引值。

​     再来看看什么是覆盖索引，有下面三种理解：（==覆盖索引中的字段顺序无需与查询 select后的字段和条件的字段顺序一致==）

- 解释一： 就是select的数据列只用从（==一个或多个==）索引中就能够取得，不必从数据表中读取，换句话说查询列要被所使用的索引覆盖。
- 解释二： 索引是高效找到行的一个方法，当能通过检索索引就可以读取想要的数据，那就不需要再到数据表中读取行了。如果一个索引==包含==了（==或覆盖==了）满足查询语句中字段与条件的数据就叫做覆盖索引。
- 解释三：是非聚集组合索引的一种形式，它包括在查询里的Select、Join和Where子句用到的所有列（即建立索引的字段正好是覆盖查询语句[select子句]与查询条件[Where子句]中所涉及的字段，也即，索引包含了查询正在查找的所有数据）。

　　不是所有类型的索引都可以成为覆盖索引。==覆盖索引必须要存储索引的列（因为SQL查询结果和查询条件的都涉及到具体的值，而覆盖索引需要覆盖查询结果和查询条件），而哈希索引、空间索引和全文索引等都不存储索引列的值==，所以==MySQL只能使用B-Tree索引做覆盖索引==，另外，不同的存储引擎实现覆盖索引的方式也不同，而且不是所有的引擎都支持覆盖索引。

覆盖索引：建索引的列和要查询的列相同，例如==索引列是c1，c2，select 查询的也是c1，c2==； 
定值是常量，范围之后必失效，最终看排序，一般order by 是给个范围； 
group by 分组，分组之前必排序，顺序不对会有临时表产生；

　　当发起一个被索引覆盖的查询(也叫作索引覆盖查询)时，在 EXPLAIN 的 Extra 列可以看到“==Using index==”的信息

> 使用覆盖索引注意事项

**1，当只查询表的主键字段时，是索引覆盖查询，如下为索引覆盖查询**

~~~MYSQL
SELECT SQL_NO_CACHE id FROM user_group;
~~~

**2，当where和select查询的列分别创建单独的索引时，不是索引覆盖查询**

**3，当无where时，select查询的列为主键和另==一个==索引时，是索引覆盖查询**

​	**有where时，则where条件必须是另==一个==非主键索引，才是索引覆盖查询**

**4，select查询的列为==两个及两个以上==单独加了索引(非主键)的字段时（或包含非索引字段），不是索引覆盖查询**

如id为主键，分别为uid和group_id创建索引，如下查询不是索引覆盖查询

```mysql
SELECT SQL_NO_CACHE uid,group_id FROM user_group;
```

因为此时不会使用到索引，而是会做全表查询。

**5，如果select查询的列只要都同时属于同一个组合索引中的全部或部分列时，都是索引覆盖查询**

​	**带上where条件查询时，如果where后面要是包含除了该联合索引以外的字段（或不符合索引的前缀匹配原则）时，都不是索引覆盖查询（联合索引和主键同时作为查询条件也不行）。**

​	 **如果where后面的查询条件都为联合索引中的字段且是符合前缀匹配原则时，是索引覆盖查询，如下是索引覆盖查询：**

存在组合索引 group_id_uid( group_id,  uid )

~~~mysql
SELECT SQL_NO_CACHE id, group_id FROM user_group where uid = 2 and group_id = 123
~~~

**6, where后面的查询条件包含非索引字段 或 ==两个及两个以上的所属不同索引字段== 时，都不是索引覆盖查询。**

如group_id为联合索引的前缀，name为一个单独的索引，则如下不是索引覆盖查询

```mysql
SELECT SQL_NO_CACHE group_id, name FROM user_group where group_id = 123 and name = "test" ;
```

分别为uid和group_id创建独立索引, 即使只查询其中一个索引字段的值或id的值，也不是索引覆盖查询。

```mysql
SELECT SQL_NO_CACHE uid FROM user_group where uid = 2 and group_id = 123;
```

**7，where条件中包含like的操作可能也是索引覆盖查询**

如果id为主键，name为索引

7.1 执行如下，关键词前缀匹配查询，只范围查询具有对应前缀的数据值

```mysql
explain SELECT name FROM `user_group` where name like "test%";
```



#### 总结：覆盖索引的优化及限制

 覆盖索引是一种非常强大的工具，能大大提高查询性能，只需要读取索引而不需要读取数据，有以下优点：

 1、索引项通常比记录要小，所以MySQL访问更少的数据。

 2、索引都按值得大小存储，相对于随机访问记录，需要更少的I/O。

 3、数据引擎能更好的缓存索引，比如MyISAM只缓存索引。

 4、覆盖索引对InnoDB尤其有用，因为InnoDB使用聚集索引组织数据，如果二级索引包含查询所需的数据，就不再需要在聚集索引中查找了。

 **限制：**

 1、覆盖索引也并不适用于任意的索引类型，索引必须存储列的值。

 2、Hash和full-text索引不存储值，因此MySQL只能使用BTree。

 3、不同的存储引擎实现覆盖索引都是不同的，并不是所有的存储引擎都支持覆盖索引。

 4、如果要使用覆盖索引，一定要注意SELECT列表值取出需要的列，==不可以SELECT *==，因为如果将所有字段一起做索引会导致索引文件过大，查询性能下降。



### 创建索引（索引优化）

**Join连接索引分析 **
单表分析 
例：select id from A where c1 = 1 and c2 > 1 order by v1 desc limit 1; 
建立联合索引（c1,c2,v1），但explain时候发现 type 是range，extra中使用using filesort，这需要优化； 
产生原因：按照BTree索引工作原理，先排序c1，如果c1相同，排序c2，c2相同再排序v1，当c2字段在联合索引中处于中间位置，因为c2 > 1条件是一个范围值（range），MySQL无法利用索引在对后面的v1部分进行索引。所以建立（c1，v1）或建立（c1，c2）和（v1）解决这个问题。

双表分析 
==左连接索引加在右表，右连接索引加在左表；== 
Left Join 条件用于确定如何从右表搜索行，左边数据一定有，所以右边数据一定要建索引。

三表分析 
建索引和双表的原理相同，依次在连接表上添加索引；

**总结** 
尽可能减少 Join 语句的 NestedLoop 的循环总次数，永远用小结果集驱动大的结果集； 
优先优化 NestedLoop 的内层循环； 
保证 Join 语句中被驱动表上 On 条件字段已经被索引；

**永远小表驱动大表** 
in 和 exists 选择：

~~~mysql
-- 工作原理，先查B表数据，然后查A的 id 
select * from A where id in (select id from B)
-- 工作原理，先查A表的id，然后查B表的id
select * from A a where exists (select 1 from B b where a.id = b.id )
-- 结论：当B表的数据小于A表时候用 in；当A表数据小于B表时候用 exists
-- 注意：A 表与 B 表的 id 字段应该有索引
~~~

- exists 

  select.....from table where exists (subquery )

  该语法可以理解为：将主查询的数据，放到子查询中做条件验证，根据验证结果（true 或者 false）来决定主查询的数据结果是否得以保留。

- 提示：exists (subquery )只返回 TRUE 或者 FALSE，因此子查询的 select * 可以是 select 1或 select ‘x’ 等任意，官方说法是实际执行时会忽略 select 清单，因此没有区别。

    

**Order By 排序** 
MySQL支持两种排序，index 和 fileSort，index效率高，它指 MySQL扫描索引本身完成排序。 
Order By 满足两种情况使用 index： 
1、Order By 语句使用索引最左前列 
2、使用 ==where子句与Order By子句条件组合==满足索引最左前列 ，==where 的字段和 order by 的字段组合后是索引的最左前缀。==
如果不满足上面情况，会产生临时表，fileSort 有两种算法，4.1版本之前==双路排序，进行两次IO（磁盘扫描），把全部数据都存在缓存，包括不查询的数据==；之后==单路排序，进行一次IO，把要查询的数据放在缓存里，如果重新查询的数据不包括在缓存里，则需要重新再次 IO ，这样效率会比之前的双路排序还要低==； 
==Order By 时不要select *==，只查询所需要的字段；当两种算法的数据超出 sort_buffer 的容量会创建tmp 文件进行合并运算，导致多次 IO，所以需要尝试提高 sort_buffer_size 和max_length_for_sort_ size 来提高 mysql 查询排序效率。

为排序使用索引，MySQL能为排序与查询使用相同的索引

~~~mysql
-- 如果 where 使用索引的最左前缀定义为常量，则 order by 能使用索引 
index（a，b，c）
where a= const order by b,c;
where a= const and b> const order by b,c;

-- 不能使用索引进行排序
order by a ASC,b DESC,c DESC; -- 排序不一致
where g = const order by b,c; -- 缺失a索引
where a = const order by c; -- 缺失b索引
where a = const order by a，d; -- d不是索引的一部分
where a in（....） order by b，c; -- 对于排序来说，多个相等条件也是范围查询
# group by 跟 order by 差不多
~~~



### 查询优化（索引失效）

查询优化器在where查询中的作用：

如果一个多列索引存在于 列 Col1 和 Col2 上，则以下语句：Select  * from table where  col1 = val1 AND col2 = val2 查询优化器会试图通过决定哪个索引将找到更少的行。之后用得到的索引去取值。
1． 如果存在一个多列索引，任何最左面的索引前缀能被优化器使用。所以联合索引的顺序不同，影响索引的选择，==尽量将值少的放在前面==。
如：==一个多列索引为 (col1 ，col2， col3)
  那么在索引在列 (col1) 、(col1 col2) 、(col1 col2 col3) 的搜索会有作用==。

~~~mysql
SELECT * FROM tb WHERE col1 = val1 
SELECT * FROM tb WHERE col1 = val1 and col2 = val2 
SELECT * FROM tb WHERE col1 = val1 and col2 = val2 AND col3 = val3 
----------------------------------------------------------------------------
SELECT * FROM tb WHERE col3 = val3 and col2 = val2 AND col1 = val1
SELECT * FROM tb WHERE col1 = val1 and col3 = val3 AND col2 = val2
-- 上面这两例也会用到复合索引，因为是定值常量，查询优化器会进行优化排序，一般按索引字段顺序来，防止查询优化器重排出错。如果不是定值常量就会索引失效。
~~~

2． 如果列不构成索引的最左面前缀，则建立的索引将不起作用。
如：

~~~mysql
SELECT * FROM tb WHERE col3 = val3 
SELECT * FROM tb WHERE col2 = val2 
SELECT * FROM tb WHERE col2 = val2 and col3=val3 
---------------------------------------------------------------------------------
index（a，b，c）
select * from myTest  where a=3 and b>7 and c=3;    -- b范围值，断点，阻塞了c的索引
-- a用到了，b也用到了，c没有用到，这个地方b是范围值，也算断点，只不过自身用到了索引
select * from myTest  where a>4 and b=7 and c=9;
-- a用到了  b没有使用，c没有使用
select * from myTest  where a=4 and c>7 and b=9;
-- a用到了，b也用到了，c也用到，查询优化器会重排	
~~~

3． 如果一个 Like 语句的查询条件不以通配符起始则使用索引。
如：==%车 或 %车%  不使用索引==。
       ==车%       使用索引==。
索引的缺点：
   \1.    占用磁盘空间。
   \2.    增加了插入和删除的操作时间。一个表拥有的索引越多，插入和删除的速度越慢。如 要求快         			速录入的系统不宜建过多索引。

如果一定要 like ‘%车%’ 这样查询的话怎么办？因为 like ‘车%’ 可能会没有数据

解决方法：==使用覆盖索引== ，使用 select 的查询 select 字段组成覆盖索引

> 下面是一些常见的索引限制问题

1、使用不等于操作符(==<>, !=======)
下面这种情况，即使在列 dept_id 有一个索引，==查询语句仍然执行一次全表扫描==
        select * from dept where dept_id <> 1000;
但是开发中的确需要这样的查询，难道没有解决问题的办法了吗？
有！
通过把==用 or 语法替代不等号进行查询，就可以使用索引，以避免全表扫描==：上面的语句改成下面这样的，就可以使用索引了。

​       select * from dept where dept_id< 1000 ==or== dept_id > 1000; 

==如果 or 连接的是两个不同的字段的话就会索引失效，如 where id = 1 or name = ‘xxx’ 。==

2、==使用 is null 或 is not null==
使用 is null 或 is nuo null 也会限制索引的使用，因为数据库并没有定义null值。==如果被索引的列中有很多null，就不会使用这个索引==（除非索引是一个位图索引，关于位图索引，会在以后的blog文章里做详细解释）。在sql语句中使用null会造成很多麻烦。
解决这个问题的办法就是：==建表时把需要索引的列定义为非空(not null)==

3、==使用函数==
==如果没有使用基于函数的索引，那么where子句中对存在索引的列使用函数时，会使优化器忽略掉这些索引==。下面的查询就不会使用索引：

​       select * from staff where trunc(birthdate) = '01-MAY-82'; 

但是==把函数应用在条件上，索引是可以生效的==，把上面的语句改成下面的语句，就可以通过索引进行查找。

​      select * from staff where birthdate < (to_date('01-MAY-82') + 0.9999); 

4、==比较不匹配的数据类型==
比较不匹配的数据类型也是难于发现的性能问题之一。
下面的例子中，dept_id 是一个 varchar 型的字段，在这个字段上有索引，但是下面的语句会执行全表扫描，==类型输入不匹配==。

​      select * from dept where dept_id = 900198; 

这是因为 oracle 会自动把 where 子句转换成 to_number ( dept_id )=900198，就是3所说的情况，这样就限制了索引的使用。
把SQL语句改为如下形式就可以使用索引

​     select * from dept where dept_id = '900198';  

**总结：**

1、最好全值匹配（按照索引的顺序，查询里的字段是索引顺序的字段，并且全部使用）； 
2、最左前缀法则：如果索引了多列，查询从索引的最左前列开始，且不能跳过索引中的列； 
3、不在==索引列==(索引的字段)上做任何操作（计算，函数，（自动或手动）类型转换），会导致索引	  时校而转向全表扫描； 
4、存储引擎不能使用索引中范围条件右边的列，即范围之后全失效（==范围的字段会用到索引==）； 
5、尽量使用覆盖索引，只访问索引的查询（索引列和查询列一致），减少selec *； 
6、MySQL在使用不等于（<> 和 !=）的时候无法使用索引会导致全表扫描； 
7、is null，is not null 也无法使用索引； 
8、like 以通配符开头（’%aa‘）索引会失效，变成全表扫描，==不以通配符开头就不会失效==； 

​       如果一定要 like ‘%车%’ 这样查询的话怎么办？因为 like ‘车%’ 可能会没有数据

​       解决方法：==使用覆盖索引== ，使用 select 的查询 select 字段组成覆盖索引

9、字符串不加单引号，索引失效； 
10、少用 or，用它来连接时候会索引失效，如 where id = 1 or name = ‘xxx’ 。

### 易错点

~~~mysql
index（col1，col2，col3）
SELECT * FROM tb WHERE col3 = val3 and col2 = val2 AND col1 = val1
SELECT * FROM tb WHERE col1 = val1 and col3 = val3 AND col2 = val2
-- 上面这两例也会用到复合索引，因为是定值常量，查询优化器会进行优化排序，一般按索引字段顺序来，防止查询优化器重排出错。如果不是定值常量就会索引失效。


index（a，b，c）
select * from myTest  where a=3 and b>7 and c=3;    -- b范围值，断点，阻塞了c的索引
-- a用到了，b也用到了，c没有用到，这个地方b是范围值，也算断点，只不过自身用到了索引
select * from myTest  where a>4 and b=7 and c=9;
-- a用到了  b没有使用，c没有使用
select * from myTest  where a=4 and c>7 and b=9;
-- a用到了，b也用到了，c也用到，查询优化器会重排	


index（a，b，c，d）
select * from myTest  where a=4 and b=7 and d=9 order by c;
select * from myTest  where a=4 and b=7 order by c;
-- 上面两个 explain 显示都是用到了a 和 b，但实际上还用到了c，用于排序
select * from myTest  where a=4 and b=7 order by d;
-- 用到了 a 和 b，mysql自己外部排序(不好)
select * from myTest  where a=4 and e=7 order by b,c;
-- explain 显示只用到了a ，但实际上还用到了b 和 c，用于排序
select * from myTest  where a=4 and e=7 order by c,b;
-- 只用到了 a，mysql自己外部排序
select * from myTest  where a=4 and b=7 and e=7 order by c,b;
-- 用到了 a 和 b，b ，c用于排序（这里b是常量，即使用于排序也是无用）
-- 相当于 select * from myTest  where a=4 and b=7 and e=7 order by c;
select * from myTest  where a=4 and b=7 order by b,c;
select * from myTest  where a=4 and b=7 and e=9 order by b,c;
-- 用到了 a 和 b，b 和 c用于排序
-- 相当于 select * from myTest  where a=4 and b=7 and e=9 order by c;


-- group by 和 order by 差不多
-- group by 基本上都要进行排序，会有临时表的产生
select * from myTest  where a=4 and d=7 group by b,c;
-- 只用到了 a， b 和 c 用于排序
select * from myTest  where a=4 and d=7 group by c,b;
-- 只用到了 a，产生临时表，mysql自己外部排序（最坏）
~~~



## explain 执行计划

> explain 作用

**使用 explain 关键字可以模拟优化器执行 SQL 查询语句，从而知道 MySQL是如何处理你的SQL语句的，分析你的查询语句或是表结构的性能瓶颈。**

> explain执行计划包含的信息

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182658)

其中最重要的字段为：==id、type、key、rows、Extra==

### 各字段详解

#### **id**

select查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序 

三种情况： 
1、id相同：执行顺序由上至下 

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182701)

2、id不同：如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行 

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182704)

3、id相同又不同（两种情况同时存在）：id如果相同，可以认为是一组，从上往下顺序执行；在所有组中，id值越大，优先级越高，越先执行 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182708)

#### select_type

查询的类型，主要是用于区分普通查询、联合查询、子查询等复杂的查询

1、SIMPLE：简单的 select 查询，查询中不包含子查询或者 union 
2、PRIMARY：查询中包含任何复杂的子部分，最外层查询则被标记为 primary 
3、SUBQUERY：在select 或 where 列表中包含了子查询 
4、DERIVED：在 from 列表中包含的子查询被标记为 derived（衍生），mysql 或递归执行这些子查询，把结果放在临时表里 
5、UNION：若第二个 select 出现在 union 之后，则被标记为 union；若 union 包含在 from 子句的子查询中，外层 select 将被标记为 derived 
6、UNION RESULT：从 union 表获取结果的 select 

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182711)

#### type

访问类型，sql查询优化中一个很重要的指标，结果值从好到坏依次是：

**system** > **const **> **eq_ref **> **ref **> fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > **range** > **index** > **ALL**

一般来说，好的sql查询至少达到range级别，最好能达到ref

1、system：表只有一行记录（等于系统表），这是const类型的特例，平时不会出现，可以忽略不计

2、const：表示通过索引一次就找到了，const用于比较primary key 或者 unique 索引。因为只需匹配一行数据，所以很快。如果将主键置于 where 列表中，mysql 就能将该查询转换为一个const 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182715)

3、eq_ref：唯一性索引扫描，对于每个索引键，表中==只有一条记录==与之匹配。常见于主键 或 唯一索引扫描。 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182718)

注意：ALL全表扫描的表记录最少的表如 t1表

4、ref：非唯一性索引扫描，返回匹配某个单独值的所有行。本质是也是一种索引访问，它返回所有匹配某个单独值的行，然而他==可能会找到多个符合条件的行，所以它应该属于查找和扫描的混合体==

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182722)

5、range：只检索给定范围的行，使用一个索引来选择行。key列显示使用了那个索引。一般就是在where语句中出现了bettween、<、>、in等的查询。这种索引列上的范围扫描比全索引扫描要好。只需要开始于某个点，结束于另一个点，不用扫描全部索引 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182725)

6、index：Full Index Scan，index与ALL区别为 index 类型==只遍历索引树==。这通常比 ALL快，因为索引文件通常比数据文件小。（Index 与 ALL 虽然都是读全表，但 index 是从索引中读取，而 ALL 是从硬盘读取） 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182727)

7、ALL：Full Table Scan，遍历全表以找到匹配的行 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182729)

#### possible_keys

查询涉及到的字段上存在索引，则该索引将被列出，但==不一定被查询实际使用==

#### key

==实际使用的索引==，如果为NULL，则没有使用索引。 
查询中如果使用了==覆盖索引，则该索引仅出现在key列表中==

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182735)

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182744)

#### key_len

表示索引中使用的字节数，查询中使用的索引的长度（最大可能长度），==并非实际使用长度==，理论上长度越短越好。==key_len是根据表定义计算而得的，不是通过表内检索出的==

#### ref

显示索引的哪一列被使用了，如果可能，==可能是一个常量const==。

#### rows

根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数

#### Extra

不适合在其他字段中显示，但是十分重要的额外信息

1、Using filesort （九死一生）： 
mysql 对数据使用一个外部的索引==排序==，而不是按照表内的索引进行排序读取。也就是说 mysql 无法利用索引完成的排序操作成为“文件排序” 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182747)

由于索引是先 email 排序、再按 address 排序，所以查询时如果直接按 address 排序，索引就不能满足要求了，mysql 内部必须再实现一次“文件排序”（虽然有覆盖索引，但没有实现复合索引）

2、Using temporary（十死无生）： 
使用临时表保存中间结果，也就是说mysql在对查询结果排序时使用了临时表，常见于order by 和 group by 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182751)

3、Using index（好的）： 
表示相应的 select 操作中使用了覆盖索引（Covering Index），避免了访问表的数据行，效率高 
如果同时出现Using where，表明索引被用来执行索引键值的查找（参考上图） 
如果没用同时出现Using where，表明索引用来读取数据而非执行查找动作 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182754)

覆盖索引（Covering Index）：也叫索引覆盖。就是select列表中的字段，只用从索引中就能获取，不必根据索引再次读取数据文件，换句话说查询列要被所建的索引覆盖。 
注意： 
a、==如需使用覆盖索引，select列表中的字段只取出需要的列，不要使用select *==
b、如果将所有字段都建索引会导致索引文件过大，反而降低crud性能

4、Using where ： 
使用了where过滤

5、Using join buffer ： 
使用了链接缓存

6、Impossible WHERE： 
where子句的值总是false，不能用来获取任何元组

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182757)

7、select tables optimized away： 
在没有group by子句的情况下，基于索引优化 MIN/MAX 操作或者对于 MyISAM 存储引擎优化COUNT（*）操作，不必等到执行阶段再进行计算，查询执行计划生成的阶段即可完成优化

8、distinct： 
优化distinct操作，在找到第一个匹配的元组后即停止找同样值得动作

### 综合Case

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182804)

**执行顺序** 
1（id = 4）、【select id, name from t2】：select_type 为union，说明id=4的select是union里面的第二个select。

2（id = 3）、【select id, name from t1 where address = ‘11’】：因为是在from语句中包含的子查询所以被标记为DERIVED（衍生），where address = ‘11’ 通过复合索引idx_name_email_address的==索引覆盖==就能检索到，所以type为index。

3（id = 2）、【select id from t3】：因为是在select中包含的子查询所以被标记为SUBQUERY。

4（id = 1）、【select d1.name, … d2 from … d1】：select_type为PRIMARY表示该查询为最外层查询，table列被标记为 “derived3”表示查询结果来自于一个衍生表（id = 3 的select结果）。

5（id = NULL）、【 … union … 】：代表从union的临时表中读取行的阶段，table列的 ==“union 1, 4”表示用id=1 和 id=4 的select结果进行union操作。==



## 查询截取分析

1、观察，查看慢SQL情况； 
2、开启查询日志，设置阀值，规定多少秒为慢SQL； 
3、explain 分析； 
4、show profile 查看执行细节和生命周期情况 
5、dba 进行参数调优

**慢查询日志** 
响应时间超过long_query_tine的SQL，被记录到慢查询日志中。

~~~mysql
 SHOW VARIABLES LIKE '%slow_query_log%' ; -- 查看是否开启，默认没开启
 set global slow_quary_log = 1; -- 开启，仅本数据库有效，重启MySQL之后失效。

 show variables like '%long_query_time%'; -- 查看当前多少秒算慢
 set global long_query_time = 3;
 -- 设置慢的阙值时间，得重新开会话或重新连接才会看到值改变

 show global status like '%Slow_queries%'; -- 查看当前数据库有多少条慢SQL
~~~

**Show Profile** 
是MySQL提供可以用来分析当前会话中语句执行的资源情况，可以用于SQL的调优的测量。 
默认关闭，并保存最近15次结果；

~~~mysql
 set profiling = on ; -- 开启
 show profiles; -- 查看执行过的sql
 SHOW PROFILE cpu,block io FOR QUERY 87; 
 -- 查看编号为87的执行sql（根据 show profiles 获得的表查看编号）的生命周期相关信息。
~~~

在生命周期相关信息表里面出现下面信息是不行的：

- converting HEAP to MyISAM ： 查询结果太大，内存不够用了往磁盘上搬； 
- Creating tmp table ： 创建临时表，拷贝数据到临时表，用完再删除； 
- Copying to tmp table on disk ： 把内存中临时表复制到磁盘，危险！ 
- locked

**全局查询日志** 
==严禁在生产环境中开启这个功能。==

~~~mysql
 set global general_log = 1; 
 set global log_output = 'TABLE';
-- 此后，你所有编写的sql语句都会记录到mysql库中的general_log表；
~~~

## MySQL锁机制

分类可以分为读锁（共享锁）和写锁（排它锁）；表锁和行锁；

**表锁**：==偏向 MyISAM== 存储引擎，开销小，加锁快，并发度低； 
加锁：lock table tablename read，tablename2 write；解锁：unlock tables; 
查看哪些表被锁了 show open tables; 

**加读锁** 

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182812)

**加写锁** 

![这里写图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719182816)==MyISAM 在执行查询语句前，会自动给涉及的所有表加读锁，在执行增删改前，会自动给涉及的表加写锁==。 
1、对 MyISAM 表的操作（加读锁），不会阻塞其他进程对同一表的读请求，但会阻塞对同一表的写请求，只有当读锁释放后，才会执行其他进程的写操作。 
2、对MyISAM表的操作（加写锁），会阻塞其他进程对同一表的读和写操作，只有当写锁释放后，才会执行其他进程的读写操作。

**分析表锁定** 
show status likes ‘table%’;   显示一个分析表

参数：

Table_locks_immediate：产生表级锁定的次数； 
Table_locks_waited：出现表级锁定争用而发生等待的次数，不能立即获取锁的次数，每等待一次锁值加1；
MyISAM 的读写锁调度是写优先，所以不适合做写为主的表的引擎。

**行锁**：==偏向InnoDB==存储引擎，开销大，会出现死锁，锁的粒度最小，发生锁冲突的概率最低，并发度高。 

**InnoDB与MyISAM最大不同是InnoDB支持事务，并且采用行级锁；**

当我们使用范围条件而不是相等条件检索数据，并请求共享排他锁时，InnoDB会给符合条件的已有数据记录的索引项加锁；==对于键值在条件范围内但不存在的记录，叫做间隙；InnoDB也会对这个间隙加锁，锁叫做间隙锁。==如 where a>7 and a<11,如果表没有 a = 10 的数据，a =10这个位置也会被锁上，其他会话不能进行操作。



## 用户管理

> 基本命令

```mysql
/* 用户和权限管理 */ ------------------
用户信息表：mysql.user

-- 刷新权限
FLUSH PRIVILEGES

-- 增加用户 CREATE USER kuangshen IDENTIFIED BY '123456'
CREATE USER 用户名 IDENTIFIED BY [PASSWORD] 密码(字符串)
  -- 必须拥有mysql数据库的全局CREATE USER权限，或拥有INSERT权限。
  -- 只能创建用户，不能赋予权限。
  -- 用户名，注意引号：如 'user_name'@'192.168.1.1'或者'user_name'@'localhost'
  -- 密码也需引号，纯数字密码也要加引号
  -- 要在纯文本中指定密码，需忽略PASSWORD关键词。要把密码指定为由PASSWORD()函数返回的混编值，需包含关键字PASSWORD

-- 重命名用户 RENAME USER kuangshen TO kuangshen2
RENAME USER old_user TO new_user

-- 设置密码
SET PASSWORD = PASSWORD('密码')    -- 为当前用户设置密码
SET PASSWORD FOR 用户名 = PASSWORD('密码')    -- 为指定用户设置密码

-- 8.0版本以上不支持上面命令，得下面命令
USE MYSQL;
ALTER  USER 'root'@'localhost' IDENTIFIED WITH MYSQL_NATIVE_PASSWORD  BY '123456';
FLUSH PRIVILEGES;

-- 删除用户 DROP USER kuangshen2
DROP USER 用户名

-- 分配权限/添加用户
GRANT 权限列表 ON 表名 TO 用户名 [IDENTIFIED BY [PASSWORD] 'password']
  - all privileges 表示所有权限，但不包含授权给他人权限
  - *.* 表示所有库的所有表
  - 库名.表名 表示某库下面的某表

-- 查看权限   SHOW GRANTS FOR root@localhost;
SHOW GRANTS FOR 用户名
   -- 查看当前用户权限
  SHOW GRANTS; 或 SHOW GRANTS FOR CURRENT_USER; 或 SHOW GRANTS FOR CURRENT_USER();

-- 撤消权限
REVOKE 权限列表 ON 表名 FROM 用户名
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 用户名    -- 撤销所有权限
```

==mysql 5.7.9以后废弃了password字段和password()函数；authentication_string:字段表示用户密码。==

> 权限解释

```mysql
-- 权限列表
ALL [PRIVILEGES]    -- 设置除GRANT OPTION之外的所有简单权限
ALTER    -- 允许使用ALTER TABLE
ALTER ROUTINE    -- 更改或取消已存储的子程序
CREATE    -- 允许使用CREATE TABLE
CREATE ROUTINE    -- 创建已存储的子程序
CREATE TEMPORARY TABLES        -- 允许使用CREATE TEMPORARY TABLE
CREATE USER        -- 允许使用CREATE USER, DROP USER, RENAME USER和REVOKE ALL PRIVILEGES。
CREATE VIEW        -- 允许使用CREATE VIEW
DELETE    -- 允许使用DELETE
DROP    -- 允许使用DROP TABLE
EXECUTE        -- 允许用户运行已存储的子程序
FILE    -- 允许使用SELECT...INTO OUTFILE和LOAD DATA INFILE
INDEX     -- 允许使用CREATE INDEX和DROP INDEX
INSERT    -- 允许使用INSERT
LOCK TABLES        -- 允许对您拥有SELECT权限的表使用LOCK TABLES
PROCESS     -- 允许使用SHOW FULL PROCESSLIST
REFERENCES    -- 未被实施
RELOAD    -- 允许使用FLUSH
REPLICATION CLIENT    -- 允许用户询问从属服务器或主服务器的地址
REPLICATION SLAVE    -- 用于复制型从属服务器（从主服务器中读取二进制日志事件）
SELECT    -- 允许使用SELECT
SHOW DATABASES    -- 显示所有数据库
SHOW VIEW    -- 允许使用SHOW CREATE VIEW
SHUTDOWN    -- 允许使用mysqladmin shutdown
SUPER    -- 允许使用CHANGE MASTER, KILL, PURGE MASTER LOGS和SET GLOBAL语句，mysqladmin debug命令；允许您连接（一次），即使已达到max_connections。
UPDATE    -- 允许使用UPDATE
USAGE    -- “无权限”的同义词
GRANT OPTION    -- 允许授予权限


/* 表维护 */

-- 分析和存储表的关键字分布
ANALYZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE 表名 ...
-- 检查一个或多个表是否有错误
CHECK TABLE tbl_name [, tbl_name] ... [option] ...
option = {QUICK | FAST | MEDIUM | EXTENDED | CHANGED}
-- 整理数据文件的碎片
OPTIMIZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE tbl_name [, tbl_name] ...
```



### MySQL备份

数据库备份必要性

- 保证重要数据不丢失
- 数据转移

MySQL数据库备份方法

- mysqldump备份工具
- 数据库管理工具,如SQLyog
- 直接拷贝数据库文件和相关配置文件

**mysqldump客户端**

作用 :

- 转储数据库
- 搜集数据库进行备份
- 将数据转移到另一个SQL服务器,不一定是MySQL服务器

```mysql
-- 导出
1. 导出一张表 -- mysqldump [-h主机名] -uroot -p123456 school student >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表名 > 文件名(D:/a.sql)
2. 导出多张表 -- mysqldump [-h主机名] -uroot -p123456 school student result >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表1 表2 表3 > 文件名(D:/a.sql)
3. 导出所有表 -- mysqldump [-h主机名] -uroot -p123456 school >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 > 文件名(D:/a.sql)
4. 导出一个库 -- mysqldump [-h主机名] -uroot -p123456 -B school >D:/a.sql
　　mysqldump -u用户名 -p密码 -B 库名 > 文件名(D:/a.sql)

可以-w携带备份条件

-- 导入
1. 在登录mysql的情况下：-- source D:/a.sql
　　source 备份文件  -- 导入表的话需要指定导入的数据库，先 use 数据库。
2. 在不登录的情况下
　　mysql -u用户名 -p密码 库名 < 备份文件
```



## 规范化数据库设计

### 为什么需要数据库设计

**当数据库比较复杂时我们需要设计数据库**

**糟糕的数据库设计 :** 

- 数据冗余,存储空间浪费
- 数据更新和插入的异常
- 程序性能差

**良好的数据库设计 :** 

- 节省数据的存储空间
- 能够保证数据的完整性
- 方便进行数据库应用系统的开发

 **软件项目开发周期中数据库设计 :**

- 需求分析阶段: 分析客户的业务和数据处理需求
- 概要设计阶段:设计数据库的E-R模型图 , 确认需求信息的正确和完整.

**设计数据库步骤**

- 收集信息

- - 与该系统有关人员进行交流 , 座谈 , 充分了解用户需求 , 理解数据库需要完成的任务.

- 标识实体[Entity]

- - 标识数据库要管理的关键对象或实体,实体一般是名词
  - 标识每个实体需要存储的详细信息[Attribute]
  - 标识实体之间的关系[Relationship]



### 三大范式

**问题 : 为什么需要数据规范化?**

不合规范的表设计会导致的问题：

- 信息重复

- 更新异常

- 插入异常

- - 无法正确表示信息

- 删除异常

- - 丢失有效信息

> 三大范式

**第一范式 (1st NF)**

第一范式的目标是确保每列的原子性,如果每列都是不可再分的最小数据单元,则满足第一范式

**第二范式(2nd NF)**

第二范式（2NF）是在第一范式（1NF）的基础上建立起来的，即满足第二范式（2NF）必须先满足第一范式（1NF）。

第二范式要求每个表只描述一件事情

**第三范式(3rd NF)**

如果一个关系满足第二范式,并且除了主键以外的其他列都不传递依赖于主键列,则满足第三范式.

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。



**规范化和性能的关系**

关联查询的表不得超过三张表

- 为满足某种商业目标 , 数据库性能比规范化数据库更重要

- 在数据规范化的同时 , 要综合考虑数据库的性能

- 通过在给定的表中添加额外的字段,以大量减少需要从中搜索信息所需的时间（从多表查询变为单表查询）

- 通过在给定的表中插入计算列,以方便查询通过在给定的表中插入计算列,以方便查询（从大数据量降低为小数据量的查询：索引）

  

## JDBC

**数据库驱动**

应用程序通过数据库驱动，和数据库打交道，开发人员通过调用数据库驱动JDBC接口进行数据库和应用程序连接。

Sun公司为了简化开发人员的（对数据库的统一）操作，提供了一个规范（java操作数据库的规范），俗称 JDBC。这些规范的实现由具体的厂商去做，对于开发人员来说，我们只需要掌握接口的实现即可。

### JDBC程序

步骤：

1、加载驱动，通过 Class.forname（）加载

2、获取数据库连接对象 Connection ，通过 DriverManager 的 getConnection 方法获取

3、获得执行SQL的对象 Statement，通过连接对象 Connection 的createStatement 获取

4、获得返回的结果集 Resultset，通过 Statement 对象 executeQuery 或 executeUpdate 返回获得

5、关闭所有连接对象，Resultset、Statement、Connection 的关闭。

> URL

~~~java
String url = "jdbc:mysql://localhost:3306/数据库名?useUnicode=true&characterEncoding=gbk&useSSL=true" ;    
//"jdbc:oracle:thin:@localhost:1521:表名"  Oracle 默认端口是1521
~~~

Statement对象的一些常用方法

- execute() ，返回的结果boolean，boolean表示是否有结果集返回（除select外为false），有为true，其他情况都为false 
- executeUpdate() ，返回的结果int，int表是对数据库影响的行计数 
- executeQuery()， 返回的结果resultSet，一般情况存放的是select查询的结果集

### PreparedStatement对象

Statemetent 对象执行的是静态SQL语句，而 PreparedStatement 对象执行的是预编译SQL语句，Statement 对象执行 executeUpdate ( String sql ) 和 executeQuery ( String sql )，而PreparedStatement 对象执行的是无参的 executeUpdate ( )和 executeQuery ( )，从这两个方法可以看出这两个对象的特点，正因为如此，==PreparedStatement 可以预防SQL语句注入，更安全，当然它的效率也更高一些==。

> SQL注入

SQL注入攻击指的是通过构建特殊的输入作为参数传入 Web 应用程序，而这些输入大都是 SQL 语法里的一些组合，通过执行 SQL语句 进而执行攻击者所要的操作，其主要原因是==程序没有细致地过滤用户输入的数据==，致使非法数据侵入系统。

根据相关技术原理，SQL注入可以分为==平台层注入==和==代码层注入==。==前者由不安全的数据库配置或数据库平台的漏洞所致；后者主要是由于程序员对输入未进行细致地过滤，从而执行了非法的数据查询==。基于此，SQL注入的产生原因通常表现在以下几方面：①不当的类型处理；②不安全的数据库配置；③不合理的查询集处理；④不当的错误处理；⑤转义字符处理不合适；⑥多个提交处理不当。

> PreparedStatement 防止 SQL 注入的原理

PreparedStatement 把传递过来的参数当做字符，假设其中存在转义字符，比如说  `  会被直接转义

> 示例

~~~java
//STEP 1. Import required packages
import java.sql.*;

public class JDBCExample {
    // JDBC driver name and database URL
    static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
    static final String DB_URL = "jdbc:mysql://localhost/Test?serverTimezone=UTC";

    // Database credentials
    static final String USER = "root";
    static final String PASS = "root";

    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement stmt = null;
        try {
            // STEP 2: Register JDBC driver
            Class.forName("com.mysql.jdbc.Driver");

            // STEP 3: Open a connection
            System.out.println("Connecting to database...");
            conn = DriverManager.getConnection(DB_URL, USER, PASS);

            // STEP 4: Execute a query
            System.out.println("Creating statement...");
            String sql = "UPDATE Employees set age=? WHERE id=?";
            stmt = conn.prepareStatement(sql);

            // Bind values into the parameters.
            stmt.setInt(1, 35); // 设置参数
            stmt.setInt(2, 102); //设置参数

            // Let us update age of the record with ID = 102;
            int rows = stmt.executeUpdate();
            System.out.println("Rows impacted : " + rows);

            // Let us select all the records and display them.
            sql = "SELECT id, first, last, age FROM Employees";
            ResultSet rs = stmt.executeQuery(sql);

            // STEP 5: Extract data from result set
            while (rs.next()) {
                // Retrieve by column name
                int id = rs.getInt("id");
                int age = rs.getInt("age");
                String first = rs.getString("first");
                String last = rs.getString("last");

                // Display values
                System.out.print("ID: " + id);
                System.out.print(", Age: " + age);
                System.out.print(", First: " + first);
                System.out.println(", Last: " + last);
            }
            // STEP 6: Clean-up environment
            rs.close();
            stmt.close();
            conn.close();
        } catch (SQLException se) {
            // Handle errors for JDBC
            se.printStackTrace();
        } catch (Exception e) {
            // Handle errors for Class.forName
            e.printStackTrace();
        } finally {
            // finally block used to close resources
            try {
                if (stmt != null)
                    stmt.close();
            } catch (SQLException se2) {
            } // nothing we can do
            try {
                if (conn != null)
                    conn.close();
            } catch (SQLException se) {
                se.printStackTrace();
            } // end finally try
        } // end try
        System.out.println("Goodbye!");
    }// end main
}// end JDBCExample
~~~

### 在java业务中实现事务

~~~java
package JDBC;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class test {
    public static void main(String[] args) throws ClassNotFoundException {
        Class.forName("com.mysql.cj.jdbc.Driver");  
        String url = "jdbc:mysql://localhost:3306/account?useUnicode=true&characterEncoding=utf8&useSSL=true&serverTimezone=UTC";
        String user = "root";
        String password = "admin";
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
             connection = DriverManager.getConnection(url, user, password);
           //关闭自动提交，就会自动开启事务，开启事务前必须先关闭自动提交
            connection.setAutoCommit(false);
            String sql1 = "update shop set money = money - 200 where name = '张三'";
            String sql2 = "update shop set money = money + 200 where name = '李四'";

            preparedStatement = connection.prepareStatement(sql1);
            preparedStatement.executeUpdate();
            preparedStatement = connection.prepareStatement(sql2);
            preparedStatement.executeUpdate();
            connection.commit();
            System.out.println("成功");

        } catch (Exception e) {
            //有异常默认会回滚事务
              //  connection.rollback();
            e.printStackTrace();
        }finally {
            if (preparedStatement != null) {
                try {
                    preparedStatement.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }

    }
}

~~~

1、开启事务   ==connection.setAutoCommit(false);==

2、一组业务执行完毕，提交事务  connection.commit();

3、可以在 catch 中显示定义回滚语句 connection.rollback();，但是默认失败就会回滚



## 课外了解资料

### MySQL索引实现

在MySQL中，索引属于存储引擎级别的概念，不同存储引擎对索引的实现方式是不同的，本文主要讨论 MyISAM 和 InnoDB 两个存储引擎的索引实现方式。

#### MyISAM索引实现

MyISAM 引擎使用B+Tree作为索引结构，叶节点的==data域存放的是数据记录的地址==。下图是MyISAM 索引的原理图：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182833.png)

​                                                                   图8

这里设表一共有三列，假设我们以Col1为主键，则图8是一个 MyISAM 表的主索引（Primary key）示意。可以看出 ==MyISAM 的索引文件仅仅保存数据记录的地址==。在 MyISAM 中，主索引和辅助索引（Secondary key）在结构上没有任何区别，只是主索引要求key是唯一的，而辅助索引的key可以重复。如果我们在Col2上建立一个辅助索引，则此索引的结构如下图所示：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182849.png)

​                                                                      图9

同样也是一颗B+Tree，data域保存数据记录的地址。因此，MyISAM 中索引检索的算法为首先按照B+Tree搜索算法搜索索引，==如果指定的Key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录。==

==MyISAM 的索引方式也叫做“非聚集”的==，之所以这么称呼是为了与 InnoDB 的聚集索引区分。

#### InnoDB索引实现

虽然 InnoDB 也使用B+Tree作为索引结构，但具体实现方式却与 MyISAM 截然不同。

第一个重大区别是 ==InnoDB 的数据文件本身就是索引文件==。从上文知道，==MyISAM 索引文件和数据文件是分离的，索引文件仅保存数据记录的地址==。而在 InnoDB 中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点==data域保存了完整的数据记录==。这个索引的key是数据表的主键，因此 ==InnoDB 表数据文件本身就是主索引==。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182905.png)

​                                                                         图10

图10是 InnoDB 主索引（同时也是数据文件）的示意图，可以看到==叶节点包含了完整的数据记录。这种索引叫做聚集索引==。因为 InnoDB 的数据文件本身要按主键聚集，所以 ==InnoDB 要求表必须有主键（MyISAM可以没有）==，如果没有显式指定，则MySQL系统会自动选择一个可以唯一标识数据记录的列作为主键，如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整形。

第二个==与MyISAM索引的不同是InnoDB的辅助索引data域存储相应记录主键的值而不是地址==。换句话说，InnoDB 的所有辅助索引都==引用主键作为data域==。例如，图11为定义在Col3上的一个辅助索引：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182914.png)

​                                                                          图11

这里以英文字符的 ASCII 码作为比较准则。聚集索引这种实现方式使得按主键的搜索十分高效，但是==辅助索引搜索需要检索两遍索引：首先检索辅助索引获得主键，然后用主键到主索引中检索获得记录。==

了解不同存储引擎的索引实现方式对于正确使用和优化索引都非常有帮助，例如知道了 InnoDB 的索引实现后，就很容易明白为什么不建议使用过长的字段作为主键，==因为所有辅助索引都引用主索引，过长的主索引会令辅助索引变得过大==。再例如，用非单调的字段作为主键在 InnoDB 中不是个好主意，因为 InnoDB 数据文件本身是一颗B+Tree，==非单调的主键会造成在插入新记录时数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效==，而使用自增字段作为主键则是一个很好的选择。



### 前缀索引（特殊建立索引）

所谓索引的选择性（Selectivity），是指不重复的索引值（也叫基数，Cardinality）与表记录数（#T）的比值：

Index Selectivity = Cardinality / #T

显然选择性的取值范围为(0, 1]，选择性越高的索引价值越大，这是由B+Tree的性质决定的。

如果频繁按名字搜索员工，这样显然效率很低，因此我们可以考虑建索引。有两种选择，建<first_name>或<first_name, last_name>，看下两个索引的选择性：

```mysql
SELECT count(DISTINCT(first_name))/count(*) AS Selectivity FROM employees.employees;
+-------------+
| Selectivity |
+-------------+
|      0.0042 |
+-------------+
SELECT count(DISTINCT(concat(first_name, last_name)))/count(*) AS Selectivity FROM employees.employees;
+-------------+
| Selectivity |
+-------------+
|      0.9313 |
+-------------+
```

<first_name>显然选择性太低，<first_name, last_name>选择性很好，但是first_name和last_name加起来长度为30，有没有兼顾长度和选择性的办法？可以考虑用first_name和last_name的前几个字符建立索引，例如<first_name, left(last_name, 3)>，看看其选择性：

```mysql
SELECT count(DISTINCT(concat(first_name, left(last_name, 3))))/count(*) AS Selectivity FROM employees.employees;
+-------------+
| Selectivity |
+-------------+
|      0.7879 |
+-------------+
```

选择性还不错，但离0.9313还是有点距离，那么把last_name前缀加到4：

```mysql
SELECT count(DISTINCT(concat(first_name, left(last_name, 4))))/count(*) AS Selectivity FROM employees.employees;
+-------------+
| Selectivity |
+-------------+
|      0.9007 |
+-------------+
```

这时选择性已经很理想了，而这个索引的长度只有18，比<first_name, last_name>短了接近一半，我们把这个前缀索引 建上：

```mysql
ALTER TABLE employees.employeesADD INDEX `first_name_last_name4` (first_name, last_name(4));
```

此时再执行一遍按名字查询，比较分析一下与建索引前的结果：

```
SHOW PROFILES;
+----------+------------+---------------------------------------------------------| Query_ID | Duration   | Query         |
+----------+------------+---------------------------------------------------------|       87 | 0.11941700 | SELECT * FROM employees.employees WHERE first_name='Eric' AND last_name='Anido' |
|       90 | 0.00092400 | SELECT * FROM employees.employees WHERE first_name='Eric' AND last_name='Anido' |
+----------+------------+---------------------------------------------------------
```

性能的提升是显著的，查询速度提高了120多倍。

==前缀索引兼顾索引大小和查询速度，但是其缺点是不能用于ORDER BY和GROUP BY操作，也不能用于Covering index 覆盖索引（即当索引本身包含查询所需全部数据时，不再访问数据文件本身）==。



### InnoDB的主键选择与插入优化

==在使用InnoDB存储引擎时，如果没有特别的需要，请永远使用一个与业务无关的自增字段作为主键。==

经常看到有帖子或博客讨论主键选择问题，有人建议使用业务无关的自增主键，有人觉得没有必要，完全可以使用如学号或身份证号这种唯一字段作为主键。不论支持哪种论点，大多数论据都是业务层面的。如果从数据库索引优化角度看，使用InnoDB引擎而不使用自增主键绝对是一个糟糕的主意。

上文讨论过InnoDB的索引实现，InnoDB使用聚集索引，数据记录本身被存于主索引（一颗B+Tree）的叶子节点上。这就要求同一个叶子节点内（大小为一个内存页或磁盘页）的各条数据记录按主键顺序存放，因此每当有一条新的记录插入时，MySQL会根据其主键将其插入适当的节点和位置，如果页面达到装载因子（InnoDB默认为15/16），则开辟一个新的页（节点）。

如果==表使用自增主键，那么每次插入新的记录，记录就会顺序添加到当前索引节点的后续位置，当一页写满，就会自动开辟一个新的页==。如下图所示：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182936.png)

​                                                                         图13

这样就会形成一个紧凑的索引结构，近似顺序填满。由于==每次插入时也不需要移动已有数据，因此效率很高，也不会增加很多开销在维护索引上==。

如果==使用非自增主键（如果身份证号或学号等），由于每次插入主键的值近似于随机，因此每次新纪录都要被插到现有索引页得中间某个位置==：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719182957.png)

​                                                                          图14

此时==MySQL不得不为了将新记录插到合适位置而移动数据，甚至目标页面可能已经被回写到磁盘上而从缓存中清掉，此时又要从磁盘上读回来，这增加了很多开销，同时频繁的移动、分页操作造成了大量的碎片，得到了不够紧凑的索引结构==，后续不得不通过OPTIMIZE TABLE来重建表并优化填充页面。

因此，只要可以，==请尽量在InnoDB上采用自增字段做主键==。