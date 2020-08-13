# Linux 系统部署 Java 应用

服务器 Linux 

阿里云、华为云、腾讯云

安装虚拟机，虚拟机就是在你的电脑中安装一台虚拟的计算机，内存、CPU、硬盘，Linux 安装到虚拟机中。

CentOS7，企业级 Linux 的发行版，完全开源，完全免费。

安装软件：

- 虚拟机 VMware workstation 15
- CentOS7
- 安装 Java 环境 JDK 8
- 安装 MySQL 8
- 安装 Tomcat
- 安装 Xshell
- 安装 Xftp

![image-20200529202256155](https://gitee.com/xudongyin/img/raw/master/img/20200719182344.png)

# 安装 VM

# 安装 Linux 操作系统 CentOS 7

1、解压 CentOS7

2、将解压之后的文件导入 VM，运行即可

3、虚拟机设置

默认的网络配置是桥接模式，直接接入当前的网络环境，如果网络不稳定，IP 会变。

NAT，相当于在 Windows 系统中独立开辟一块新的网络空间，IP 地址固定不变的，无论是否接入外网，都可以访问 CentOS。

![image-20200529202419635](https://gitee.com/xudongyin/img/raw/master/img/20200719182351.png)

![image-20200529202437020](https://gitee.com/xudongyin/img/raw/master/img/20200719182356.png)

4、启动虚拟机，用 root 权限登录

用户名：root

密码：123456

Linux 查询 IP 地址

```
ifconfig
```

# 安装 JDK

1、删除 CentOS 自带的 OpenJDK

2、通过 Xftp 将安装包拷贝到 CentOS 中

3、通过命令安装 JDK

```
rpm -ivh jdk-8u221-linux-x64.rpm
```

4、配环境变量，先进入环境变量文件    

~~~
 vim /etc/profile
~~~

再按 i 进入编辑模式，在最尾添加下面的信息

```
JAVA_HOME=/usr/java/jdk1.8.0_221-amd64
CLASSPATH=%JAVA_HOME%/lib:%JAVA_HOME%/jre/lib
PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
export PATH CLASSPATH JAVA_HOME
```

然后按 Esc 退出编辑模式，接着按 ：wq  退出保存

5、让配置生效

```
source /etc/profile
```



# Java Web 应用

1、配置 Tomcat

2、解压缩

```
tar -zxvf apache-tomcat-9.0.34.tar.gz
```

3、启动 Tomcat

```
./startup.sh
```

4、CentOS 开放 8080 端口

- 检查防火墙状态

```
firewall-cmd --state
```

running 表示防火墙是开启的，如果你看到的是 not running，防火墙关闭，需要开启

```
systemctl restart firewalld.service
```

- 开放 8080 端口

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
```

- 重启防火墙

```
systemctl restart firewalld.service
```

- 重新载入配置

```
firewall-cmd --reload
```



# 部署 Spring Boot 应用

IDEA 启动 Spring Boot ，将 Spring Boot 应用打成 jar，通过命令行部署。

```
java -jar xxx.jar
```

1、将 Spring Boot 应用打包，jar，Maven

![image-20200529202515254](https://gitee.com/xudongyin/img/raw/master/img/20200719182415.png)

![image-20200529202540224](https://gitee.com/xudongyin/img/raw/master/img/20200719182420.png)

2、通过命令行启动 jar 即可

```
java -jar demo.jar
```



# 安装 MySQL

1、通过 Xftp 将 MySQL 安装包拷贝到 Linux

2、解压缩

```
tar -xvf mysql-8.0.20-1.el7.x86_64.rpm-bundle.tar 
```

3、安装 common、libs、client、server

4、删除自带的 mariadb

```
rpm -e mariadb-libs-5.5.44-2.el7.centos.x86_64 --nodeps
```

5、安装命令行

```
rpm -ivh mysql-community-common-8.0.20-1.el7.x86_64.rpm --nodeps --force
rpm -ivh mysql-community-libs-8.0.20-1.el7.x86_64.rpm --nodeps --force
rpm -ivh mysql-community-client-8.0.20-1.el7.x86_64.rpm --nodeps --force
rpm -ivh mysql-community-server-8.0.20-1.el7.x86_64.rpm --nodeps --force
```

6、初始化 MySQL

```
mysqld --initialize
```

7、授权防火墙

```
chown mysql:mysql /var/lib/mysql -R;
systemctl start mysqld.service;
systemctl enable mysqld;
```

8、查看数据库的初始化密码

```
cat /var/log/mysqld.log | grep password
```

9、登录数据库

```
mysql -uroot -p
```

10、修改密码

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
```

11、使用新密码登录

12、开启远程访问

```
create user 'root'@'%' identified with mysql_native_password by 'root';
grant all privileges on *.* to 'root'@'%' with grant option;
flush privileges;
```

13、开放 3306 端口

```
firewall-cmd --zone=public --add-port=3306/tcp --permanent
systemctl restart firewalld.service
firewall-cmd --reload
```

14、MySQL 安装默认使用美国的时区，北京时间比美国晚 8 小时

```
set global time_zone='+8:00';
```

15、创建数据表

```
create database test character set utf8 collate utf8_general_ci;
use test;
create table user(
    id int primary key auto_increment,
    name varchar(22),
    birthday datetime
);
insert into user(name, birthday) VALUES ('小明','1999-01-01');
insert into user(name, birthday) VALUES ('小红','2000-01-01');
```

war jar

如果你使用的是 SSM 或者 Java WEB（非 Spring Boot）需要通过 war 包进行部署，先把你的应用打成 war 包，丢到外置的 Tomcat 中，启动 Tomcat 服务，进而访问你的应用。

如果你使用的是 Spring Boot，内置了 Tomcat，直接用 jar 部署，使用命令启动即可

```
java -jar demo.jar
```



