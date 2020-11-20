# Docker概述

**Docker为什么出现？**

一款产品： 开发–上线 两套环境！应用环境，应用配置！

开发 — 运维。 问题：我在我的电脑上可以运行！版本更新，导致服务不可用！对于运维来说考验十分大！环境配置是十分的麻烦，每一个及其都要部署环境(集群Redis、ES、Hadoop…) !费事费力。

发布一个项目( jar + (Redis MySQL JDK ES) ),项目能不能带上环境安装打包！

之前在服务器配置一个应用的环境 Redis MySQL JDK ES Hadoop 配置超麻烦了，不能够跨平台。
开发环境Windows，最后发布到Linux！

传统：开发jar，运维来做！

现在：开发打包部署上线，一套流程做完！

安卓流程：java — apk —发布（应用商店）一 张三使用apk一安装即可用！

docker流程： java-jar（环境） — 打包项目帯上环境（镜像） — ( Docker仓库：商店）— 安装即可用

Docker给以上的问题，提出了解决方案！
![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201110105758)
Docker的思想就来自于集装箱！

JRE – 多个应用(端口冲突) – 原来都是交叉的！
隔离：Docker核心思想！打包装箱！每个箱子是互相隔离的。

Docker 通过隔离机制，可以将服务器利用到极致！

本质：所有的技术都是因为出现了一些问题，我们需要去解决，才去学习！

## Docker历史

2010年，几个的年轻人，就在美国成立了一家公司 `dotcloud` 做一些pass的云计算服务！LXC（Linux Container容器）有关的容器技术！

Linux Container容器是一种内核虚拟化技术，可以提供轻量级的虚拟化，以便隔离进程和资源。

他们将自己的技术（容器化技术）命名为 Docker
Docker 刚刚延生的时候，没有引起行业的注意！dotCloud，就活不下去！

> 开源

2013年，Docker开源！越来越多的人发现docker的优点！火了。Docker每个月都会更新一个版本！

2014年4月9日，Docker1.0发布！docker为什么这么火？十分的轻巧！

在容器技术出来之前，我们都是使用虚拟机技术！

虚拟机：在window中装一个VMware，通过这个软件我们可以虚拟出来一台或者多台电脑！笨重！

虚拟机也属于虚拟化技术，Docker 容器技术，也是一种虚拟化技术！

```shell
VMware : linux centos 原生镜像（一个电脑！） 隔离、需要开启多个虚拟机！ 几个G 几分钟

docker: 隔离，镜像（最核心的环境 4m + jdk + mysql）十分的小巧，运行镜像就可以了！小巧！ 几个M 秒级启动！
```

> 聊聊Docker

Docker基于Go语言开发的！开源项目！

docker官网：https://www.docker.com/

文档：https://docs.docker.com/     Docker的文档是超级详细的！

仓库：https://hub.docker.com/

**Docker能干嘛**

> 之前的虚拟机技术

![image-20200515153852954](https://gitee.com/xudongyin/img/raw/master/img/20201110110130)

**虚拟机技术缺点**：

1. 资源占用十分多
2. 冗余步骤多
3. 启动很慢！

> 容器化技术

容器化技术不是模拟一个完整的操作系统

![image-20200515094336846](https://gitee.com/xudongyin/img/raw/master/img/20201110110136)

比较Docker和虚拟机技术的不同：

- 传统虚拟机，虚拟出一套硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了
- 每个容器间是互相隔离，每个容器内都有一个属于自己的文件系统，互不影响

> DevOps（开发、运维）

**应用更快速的交付和部署**

传统：一对帮助文档，安装程序。

Docker：打包镜像发布测试一键运行。

**更便捷的升级和扩缩容**

使用了 Docker之后，我们部署应用就和搭积木一样
项目打包为一个镜像，扩展服务器A！服务器B

**更简单的系统运维**

在容器化之后，我们的开发，测试环境都是高度一致的

**更高效的计算资源利用**

Docker是内核级别的虚拟化，可以在一个物理机上可以运行很多的容器实例！服务器的性能可以被压榨到极致。

# Docker安装

## Docker的基本组成

![image-20200514195805400](https://gitee.com/xudongyin/img/raw/master/img/20201110111419)

**镜像（image)：**

docker镜像就好比是一个目标，可以通过这个目标来创建容器服务，tomcat镜像=>run=>容器（提供服务），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

**容器(container)：**

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的.
启动，停止，删除，基本命令
目前就可以把这个容器理解为就是一个简易的 Linux系统。

**仓库(repository)：**

仓库就是存放镜像的地方！
仓库分为公有仓库和私有仓库。(很类似git)
Docker Hub是国外的。
阿里云…都有容器服务器(配置镜像加速!)

## 安装Docker

> 环境准备

1.Linux要求内核3.0以上

2.CentOS 7

```shell
#查看Linux版本号
[root@iz2zeak7sgj6i7hrb2g862z ~]# uname -r
3.10.0-514.26.2.el7.x86_64	# 要求3.0以上

#查看系统信息
[root@iz2zeak7sgj6i7hrb2g862z ~]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

> 安装

帮助文档：https://docs.docker.com/engine/install/
卸载与安装

```shell
#1.卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
#2.需要的安装包
yum install -y yum-utils

#3.设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo                         #上述方法默认是从国外的，不推荐

#推荐使用国内的
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
#更新yum软件包索引
yum makecache fast

#4.安装docker相关的 docker-ce 社区版 而ee是企业版
yum install docker-ce docker-ce-cli containerd.io # 这里我们使用社区版即可

#5.启动docker
systemctl start docker

#6. 使用docker version查看是否按照成功
docker version

#7. 测试
docker run hello-world

#8.查看已经下载的镜像(从这里可以查看已有镜像的id)
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker images
REPOSITORY         TAG                 IMAGE ID            CREATED             SIZE
hello-world        latest              bf756fb1ae65        4 months ago      13.3kB
```

卸载docker

```shell
#1. 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
#2. 删除资源
rm -rf /var/lib/docker
# /var/lib/docker 是docker的默认工作路径！
```

## 阿里云镜像加速

#### 1、登录阿里云找到容器服务,找到镜像加速器

![1604993277](https://gitee.com/xudongyin/img/raw/master/img/20201110152850.jpg)

#### 2、配置使用

```shell
#1.创建一个目录
sudo mkdir -p /etc/docker

#2.编写配置文件
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://t2wwyxhb.mirror.aliyuncs.com"]
}
EOF

#3.重启服务
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 回顾HelloWorld流程

**docker run 流程图**

![](https://gitee.com/xudongyin/img/raw/master/img/20201115092124.jpg)

#### 底层原理

Docker**是怎么工作的**？

Docker是一个Client-Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问！

Docker-Server接收到Docker-Client的指令，就会执行这个命令！

![1604994051(https://gitee.com/xudongyin/img/raw/master/img/20201115092143.jpg)](https://gitee.com/xudongyin/img/raw/master/img/20201110154111.jpg)

**为什么Docker比Vm快**
1、docker有着比虚拟机更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。
2、docker利用的是宿主机的内核,而不需要Guest OS。

```shell
GuestOS： VM（虚拟机）里的的系统（OS）

HostOS：物理机里的系统（OS）
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201110154604.jpg)

因此,当新建一个容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。因而避免**引导、加载操作系统内核整个比较费时费资源的过程**，当新建一个虚拟机时,虚拟机软件**需要加载GuestOS**，整个新建过程是**分钟级别**的。而docker由于**直接利用宿主机的操作系统**，则省略了这个复杂的过程,因此新建一个docker容器只需要**几秒钟**。

# Docker的常用命令

### 1.帮助命令

```shell
docker version      #显示docker的版本信息。
docker info         #显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help  #帮助命令
```

帮助文档的地址：https://docs.docker.com/engine/reference/commandline/build/

### 2.镜像命令

```shell
docker images #查看所有本地主机上的镜像 可以使用docker image ls代替

docker search #搜索镜像

docker pull #下载镜像 docker image pull

docker rmi #删除镜像 docker image rm
```

##### 查看镜像

```shell
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker images
REPOSITORY          TAG               IMAGE ID            CREATED          SIZE
hello-world         latest            bf756fb1ae65        4 months ago     13.3kB
mysql               5.7               b84d68d0a7db        6 days ago       448MB

# 解释
#REPOSITORY			# 镜像的仓库源
#TAG			    # 镜像的标签(版本)		---lastest 表示最新版本
#IMAGE ID			# 镜像的id
#CREATED			# 镜像的创建时间
#SIZE				# 镜像的大小

# 可选项
Options:
  -a, --all       Show all images (default hides intermediate images) #列出所有镜像
  -q, --quiet     Only show numeric IDs # 只显示镜像的id
  
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker images -a  #列出所有镜像详细信息
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker images -aq #列出所有镜像的id
d5f28a0bb0d0
f19c56ce92a8
1b6b1fe7261e
```

##### 搜索镜像

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201110161935)

```shell
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker search mysql

# --filter=STARS=3000 #过滤，搜索出来的镜像收藏STARS数量大于3000的
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
      
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker search mysql --filter=STARS=3000
NAME        DESCRIPTION         STARS            OFFICIAL        AUTOMATED
mysql       MySQL IS ...        9520             [OK]                
mariadb     MariaDB IS ...      3456             [OK]   
```

##### 下载镜像

```shell
# 下载镜像 docker pull 镜像名[:tag]
[root@iZwz97ikj14iruzlnq4vr3Z ~]# docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql  #如果不写tag，默认就是latest，最新版本
bb79b6b2107f: Pull complete   #分层下载  docker image 的核心 联合文件系统
49e22f6fb9f7: Pull complete 
842b1255668c: Pull complete 
9f48d1f43000: Pull complete 
c693f0615bce: Pull complete 
8a621b9dbed2: Pull complete 
0807d32aef13: Pull complete 
a56aca0feb17: Pull complete 
de9d45fd0f07: Pull complete 
1d68a49161cc: Pull complete 
d16d318b774e: Pull complete 
49e112c55976: Pull complete 
Digest: sha256:8c17271df53ee3b843d6e16d46cff13f22c9c04d6982eb15a9a47bd5c9ac7e2d #防伪签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest  #真实的地址


#等价于
docker pull mysql
docker pull docker.io/library/mysql:latest 

#下载mysql 5.7 
[root@iZwz97ikj14iruzlnq4vr3Z ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
bb79b6b2107f: Already exists   #因为有些底层文件跟mysql最新版本一样，所以就共用了不用下载
49e22f6fb9f7: Already exists 
842b1255668c: Already exists 
9f48d1f43000: Already exists 
c693f0615bce: Already exists 
8a621b9dbed2: Already exists 
0807d32aef13: Already exists 
f15d42f48bd9: Pull complete 
098ceecc0c8d: Pull complete 
b6fead9737bc: Pull complete 
351d223d3d76: Pull complete 
Digest: sha256:4d2b34e99c14edb99cdd95ddad4d9aa7ea3f2c4405ff0c3509a29dc40bcb10ef
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

##### 删除镜像

```shell
docker rmi -f 镜像id  #删除指定id的镜像
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker rmi -f f19c56ce92a8

docker rmi -f 镜像id  镜像id  镜像id  #删除多个指定id的镜像
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker rmi -f f19c56ce92a8 a20c56ce92a6

docker rmi -f $(docker images -aq) #删除全部的镜像
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker rmi -f $(docker images -aq)
```

### 3.容器命令

说明：我们有了镜像才可以创建容器，Linux，下载centos镜像来学习

##### 镜像下载

```shell
#docker中下载centos
docker pull centos
```

##### 新建容器并启动

```shell
docker run [可选参数] image | docker container run [可选参数] image 
#参书说明
--name="Name"		 #起容器名字 tomcat01 tomcat02 用来区分容器
-d					#后台方式运行
-it 				#使用交互方式运行，进入容器查看内容
-p					#指定容器的端口 -p 8080(宿主机):8080(容器)
		-p ip:主机端口:容器端口
		-p 主机端口:容器端口(常用)
		-p 容器端口
		容器端口
-P(大写) 				随机指定端口

# 测试、启动并进入容器
[root@iz2zeak7sgj6i7hrb2g
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -it centos /bin/bash
[root@241b5abce65e /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@241b5abce65e /]# exit   #停止容器运行退回主机
exit
```

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201110165535)

##### 列出运行的容器

```shell
docker ps 命令  		#列出当前正在运行的容器
  -a, --all     	 #列出当前正在运行的容器 + 带出历史运行过的容器
  -n=?, --last int   #列出最近创建的?个容器 ?为1则只列出最近创建的一个容器,为2则列出2个
  -q, --quiet        #只列出容器的编号
```

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201110165941)
![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201110165947)

##### 退出容器

```shell
exit 		 #容器直接退出
ctrl +P +Q   #容器不停止退出 	---注意：这个很有用的操作
```

##### 删除容器

```shell
docker rm 容器id   			  #删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -rf
docker rm -f $(docker ps -aq)  	 #删除所有的容器
docker ps -a -q|xargs docker rm  #删除所有的容器
```

##### 启动和停止容器

```shell
docker start 容器id	#启动容器
docker restart 容器id	#重启容器
docker stop 容器id	#停止当前正在运行的容器
docker kill 容器id	#强制停止当前容器
```

### 4.常用其他命令

##### 后台启动命令

```shell
# 命令 docker run -d 镜像名
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -d centos
a8f922c255859622ac45ce3a535b7a0e8253329be4756ed6e32265d2dd2fac6c

[root@iz2zeak7sgj6i7hrb2g862z ~]# docker ps    
CONTAINER ID      IMAGE       COMMAND    CREATED     STATUS   PORTS    NAMES
# 问题docker ps. 发现centos 停止了
# 常见的坑，docker容器使用后台运行，就必须要有要一个前台进程，docker发现没有应用，就会自动停止
# nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了
```

##### 查看日志

```shell
docker logs --help
Options:
      --details        Show extra details provided to logs 
*  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
*      --tail string    Number of lines to show from the end of the logs (default "all")
*  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
 #写一个脚本  模拟日志打印   -c 表示运行脚本   
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker run -d centos /bin/sh -c "while true;do echo xu;sleep 1;done"
559341c73531fcc6608c2ff0438516afd0a52e2131e80238b478f77789ab7440
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED           
559341c73531        centos              "/bin/sh -c 'while t…"   2 seconds ago     
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker logs -tf --tail 10 559341c73531
2020-11-10T11:32:19.175389018Z xu
2020-11-10T11:32:20.177734043Z xu
2020-11-10T11:32:21.179991429Z xu
2020-11-10T11:32:22.182378671Z xu
2020-11-10T11:32:23.185062917Z xu
2020-11-10T11:32:24.187506010Z xu
2020-11-10T11:32:25.190021447Z xu
2020-11-10T11:32:26.192300226Z xu
2020-11-10T11:32:27.194908898Z xu
2020-11-10T11:32:28.197536665Z xu
2020-11-10T11:32:29.199825363Z xu
2020-11-10T11:32:30.202154866Z xu
^C
[root@iZwz97ikj14iruzlnq4vr3Z /]# 
    
#显示日志
-tf		#显示日志信息（一直更新）
--tail number   #需要显示日志条数（不带日期时间）
docker logs -t --tail n 容器id   #查看n行日志（带日期时间）
docker logs -ft 容器id    #跟着日志
```

##### 查看容器中进程信息ps

```shell
# 命令 docker top 容器id
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker top 559341c73531
```

![image-20200515132913888](https://gitee.com/xudongyin/img/raw/master/img/20201110194609)

##### 查看镜像的元数据

```shell
# 命令
docker inspect 容器id

#测试
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker inspect 559341c73531
[
    {
        "Id": "559341c73531fcc6608c2ff0438516afd0a52e2131e80238b478f77789ab7440",
        "Created": "2020-11-10T11:31:59.812311196Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo xu;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 12248,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-11-10T11:32:00.132022147Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/559341c73531fcc6608c2ff0438516afd0a52e2131e80238b478f77789ab7440/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/559341c73531fcc6608c2ff0438516afd0a52e2131e80238b478f77789ab7440/hostname",
        "HostsPath": "/var/lib/docker/containers/559341c73531fcc6608c2ff0438516afd0a52e2131e80238b478f77789ab7440/hosts",
        "LogPath": "/var/lib/docker/containers/559341c73531fcc6608c2ff0438516afd0a52e2131e80238b478f77789ab7440/559341c73531fcc6608c2ff0438516afd0a52e2131e80238b478f77789ab7440-json.log",
        "Name": "/pedantic_bhaskara",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/29a9053abbfd5e11ac8cc35f5f0e6973d419fc6998277b2e2b325a3118dbd3d0-init/diff:/var/lib/docker/overlay2/268dd219c7917e92d851d24339219959f593fd43a8a855d9ebeb1ffb81a5e360/diff",
                "MergedDir": "/var/lib/docker/overlay2/29a9053abbfd5e11ac8cc35f5f0e6973d419fc6998277b2e2b325a3118dbd3d0/merged",
                "UpperDir": "/var/lib/docker/overlay2/29a9053abbfd5e11ac8cc35f5f0e6973d419fc6998277b2e2b325a3118dbd3d0/diff",
                "WorkDir": "/var/lib/docker/overlay2/29a9053abbfd5e11ac8cc35f5f0e6973d419fc6998277b2e2b325a3118dbd3d0/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "559341c73531",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo xu;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "7dfa71c6c37841502acb4ab7b08f048cdb504aacbb2ec23295ff7a091e3f25db",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/7dfa71c6c378",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "1aadcb05d0e9b5e2e106a3383b51374bfe9fd31042a441f5b8d769b7f024b86e",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "80c4ffaf3459890ad888d3b9dbf32e375640d35c3b118f698f3c82eae4f2d8b0",
                    "EndpointID": "1aadcb05d0e9b5e2e106a3383b51374bfe9fd31042a441f5b8d769b7f024b86e",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

##### 进入当前正在运行的容器

~~~SHELL
docker exec -it 容器id

#测试
[root@iZwz97ikj14iruzlnq4vr3Z ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED            
559341c73531        centos              "/bin/sh -c 'while t…"   12 minutes ago     
[root@iZwz97ikj14iruzlnq4vr3Z ~]# docker exec -it 559341c73531 /bin/bash
#注意，下面已经进入容器
[root@559341c73531 /]# 
~~~


![image-20200515133433372](https://gitee.com/xudongyin/img/raw/master/img/20201110194559)

```shell
# 方式二
docker attach 容器id
#测试
docker attach 55321bcae33d    #进入正在执行当前的代码...
#区别
#docker exec #进入当前容器后开启一个新的终端，可以在里面操作。（常用）
#docker attach # 进入容器正在执行的终端
```

![image-20200515133907134](https://gitee.com/xudongyin/img/raw/master/img/20201110194929)

**从容器内拷贝到主机上**

```shell
docker cp 容器id:容器内路径  主机目的路径

[root@iz2zeak7sgj6i7hrb2g862z ~]# docker ps
CONTAINER ID     IMAGE    COMMAND     CREATED         STATUS       PORTS      NAMES
56a5583b25b4     centos   "/bin/bash" 7seconds ago    Up 6 seconds      

#1. 进入docker容器内部
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker exec -it 56a5583b25b4 /bin/bash
[root@55321bcae33d /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

#新建一个文件
[root@55321bcae33d /]# echo "hello" > java.java
[root@55321bcae33d /]# cat hello.java 
hello
[root@55321bcae33d /]# exit
exit

#hello.java拷贝到home文件加下
[root@iz2zeak7sgj6i7hrb2g862z /]# docker cp 56a5583b25b4:/hello.java /home 
[root@iz2zeak7sgj6i7hrb2g862z /]# cd /home
[root@iz2zeak7sgj6i7hrb2g862z home]# ls -l	#可以看见java.java存在
total 8
-rw-r--r-- 1 root root    0 May 19 22:09 haust.java
-rw-r--r-- 1 root root    6 May 22 11:12 java.java
drwx------ 3 www  www  4096 May  8 12:14 www
```

**小结：**
![image-20200514214313962](https://gitee.com/xudongyin/img/raw/master/img/20201110195645)
**命令大全**

```shell
  attach      Attach local standard input, output, and error streams to a running container
  #当前shell下 attach连接指定运行的镜像
  build       Build an image from a Dockerfile # 通过Dockerfile定制镜像
  commit      Create a new image from a container's changes #提交当前容器为新的镜像
  cp          Copy files/folders between a container and the local filesystem #拷贝文件
  create      Create a new container #创建一个新的容器
  diff        Inspect changes to files or directories on a container's filesystem #查看docker容器的变化
  events      Get real time events from the server # 从服务获取容器实时时间
  exec        Run a command in a running container # 在运行中的容器上运行命令
  export      Export a container's filesystem as a tar archive #导出容器文件系统作为一个tar归档文件[对应import]
  history     Show the history of an image # 展示一个镜像形成历史
  images      List images #列出系统当前的镜像
  import      Import the contents from a tarball to create a filesystem image #从tar包中导入内容创建一个文件系统镜像
  info        Display system-wide information # 显示全系统信息
  inspect     Return low-level information on Docker objects #查看容器详细信息
  kill        Kill one or more running containers # kill指定docker容器
  load        Load an image from a tar archive or STDIN #从一个tar包或标准输入中加载一个镜像[对应save]
  login       Log in to a Docker registry #
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```

### 作业练习

> 作业一：Docker 安装Nginx

```shell
#1. 搜索镜像 search 建议大家去docker搜索，可以看到帮助文档
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker search nginx

#2. 拉取下载镜像 pull
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker pull nginx

#3. 查看是否下载成功镜像
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker images

#3. 运行测试
# -d 后台运行
# --name 给容器命名
# -p 宿主机端口：容器内部端口
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -d --name nginx01 -p 3344:80 nginx
aa664b0c8ed98f532453ce1c599be823bcc1f3c9209e5078615af416ccb454c2

#4. 查看正在启动的镜像
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED           
75943663c116        nginx               "nginx -g 'daemon of…"   41 seconds ago     

#5. 进入容器
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker exec -it nginx01 /bin/bash #进入
root@aa664b0c8ed9:/# whereis nginx	#找到nginx位置
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@aa664b0c8ed9:/# cd /etc/nginx/
root@aa664b0c8ed9:/etc/nginx# ls
conf.d	fastcgi_params	koi-utf  koi-win  mime.types  modules  nginx.conf  scgi_params	uwsgi_params  win-utf

#6. 退出容器
root@aa664b0c8ed9:/etc/nginx# exit
exit

#7. 停止容器
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED           
aa664b0c8ed9        nginx               "nginx -g 'daemon of…"   10 minutes ago     
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker stop aa664b0c8ed9
```

**宿主机端口** 和 **容器内部端口** 以及端口暴露：

![img](https://gitee.com/xudongyin/img/raw/master/img/20201110232824)
**问题：**我们每次改动nginx配置文件，都需要进入容器内部？十分麻烦，我要是可以在容器外部提供一个映射路径，达到在容器外部修改文件名，容器内部就可以自动修改？-v 数据卷 技术！

> 作业二：用docker 来装一个tomcat

```shell
# 下载 tomcat9.0
# 之前的启动都是后台，停止了容器，容器还是可以查到， docker run -it --rm 镜像名 一般是用来测试，用完就删除
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -it --rm tomcat:9.0

--rm       Automatically remove the container when it exits 用完即删

#下载 最新版
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker pull tomcat

#查看下载的镜像
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker images

#以后台方式，暴露端口方式，启动运行
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -d -p 8080:8080 --name tomcat01 tomcat

#测试访问有没有问题
curl localhost:8080

#根据容器id进入tomcat容器
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker exec -it 645596565d3f /bin/bash
root@645596565d3f:/usr/local/tomcat# 
#查看tomcat容器内部内容：
root@645596565d3f:/usr/local/tomcat# ls -l
total 152
-rw-r--r-- 1 root root 18982 May  5 20:40 BUILDING.txt
-rw-r--r-- 1 root root  5409 May  5 20:40 CONTRIBUTING.md
-rw-r--r-- 1 root root 57092 May  5 20:40 LICENSE
-rw-r--r-- 1 root root  2333 May  5 20:40 NOTICE
-rw-r--r-- 1 root root  3255 May  5 20:40 README.md
-rw-r--r-- 1 root root  6898 May  5 20:40 RELEASE-NOTES
-rw-r--r-- 1 root root 16262 May  5 20:40 RUNNING.txt
drwxr-xr-x 2 root root  4096 May 16 12:05 bin
drwxr-xr-x 1 root root  4096 May 21 11:04 conf
drwxr-xr-x 2 root root  4096 May 16 12:05 lib
drwxrwxrwx 1 root root  4096 May 21 11:04 logs
drwxr-xr-x 2 root root  4096 May 16 12:05 native-jni-lib
drwxrwxrwx 2 root root  4096 May 16 12:05 temp
drwxr-xr-x 2 root root  4096 May 16 12:05 webapps
drwxr-xr-x 7 root root  4096 May  5 20:37 webapps.dist
drwxrwxrwx 2 root root  4096 May  5 20:36 work
root@645596565d3f:/usr/local/tomcat# 
#进入webapps目录
root@645596565d3f:/usr/local/tomcat# cd webapps
root@645596565d3f:/usr/local/tomcat/webapps# ls
root@645596565d3f:/usr/local/tomcat/webapps# 
# 发现问题：1、linux命令少了。 2.webapps目录为空 
# 原因：阿里云镜像的原因，阿里云默认是最小的镜像，所以不必要的都剔除掉
# 保证最小可运行的环境！
# 解决方案：
# 将webapps.dist下的文件都拷贝到webapps下即可
root@645596565d3f:/usr/local/tomcat# ls 找到webapps.dist
BUILDING.txt	 LICENSE  README.md	 RUNNING.txt  conf  logs  temp     webapps.dist
CONTRIBUTING.md  NOTICE   RELEASE-NOTES  bin   lib   native-jni-lib  webapps  work

root@645596565d3f:/usr/local/tomcat# cd webapps.dist/ # 进入webapps.dist 
root@645596565d3f:/usr/local/tomcat/webapps.dist# ls # 查看内容
ROOT  docs  examples  host-manager  manager

root@645596565d3f:/usr/local/tomcat/webapps.dist# cd ..
root@645596565d3f:/usr/local/tomcat# cp -r webapps.dist/* webapps # 拷贝webapps.dist 内容给webapps
root@645596565d3f:/usr/local/tomcat# cd webapps #进入webapps
root@645596565d3f:/usr/local/tomcat/webapps# ls #查看拷贝结果
ROOT  docs  examples  host-manager  manager
```

这样docker部署tomcat就可以访问了
![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201110232820)
**问题**:我们以后要部署项目，如果每次都要进入容器是不是十分麻烦？要是可以在容器外部提供一个映射路径，比如webapps，我们在外部放置项目，就自动同步内部就好了！

> 作业三：部署elasticsearch+kibana

```shell
# es 暴露的端口很多！
# es 十分耗内存
# es 的数据一般需要放置到安全目录！挂载
# --net somenetwork ? 网络配置

# 启动elasticsearch
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2

# 测试一下es是否成功启动
[root@iZwz97ikj14iruzlnq4vr3Z home]# curl localhost:9200
{
  "name" : "2a071dce5e61",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "ivWRQ4SPSPSIyFRks3D-dw",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}


#测试成功就关掉elasticSearch，防止耗内存
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker stop d834ce2bd306
d834ce2bd306

[root@iz2zeak7sgj6i7hrb2g862z ~]# docker stats  # 查看docker容器使用内存情况
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201110232804)

```shell
#测试成功就关掉elasticSearch，可以添加内存的限制，修改配置文件 -e 环境配置修改
[root@iZwz97ikj14iruzlnq4vr3Z home]# docker rm -f d73ad2f22dd3            # stop命令也行                               
[root@iZwz97ikj14iruzlnq4vr3Z home]# docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201110233855)

> 作业三：使用kibana连接es (elasticSearch)？思考网络如何才能连接
>
> ![img](https://gitee.com/xudongyin/img/raw/master/img/20201110233909)

### Portainer 可视化面板安装

- portainer(先用这个)

```shell
docker run -d -p 3344:9000 \  
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

- Rancher(CI/CD再用)

**什么是portainer？**

Docker图形化界面管理工具！提供一个后台面板供我们操作！

```shell
# 安装命令
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -d -p 3344:9000 \
> --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer

Unable to find image 'portainer/portainer:latest' locally
latest: Pulling from portainer/portainer
d1e017099d17: Pull complete 
a7dca5b5a9e8: Pull complete 
Digest: sha256:4ae7f14330b56ffc8728e63d355bc4bc7381417fa45ba0597e5dd32682901080
Status: Downloaded newer image for portainer/portainer:latest
81753869c4fd438cec0e31659cbed0d112ad22bbcfcb9605483b126ee8ff306d
```

测试访问： 外网：8080 ：http://ip:3344/
![img](https://gitee.com/xudongyin/img/raw/master/img/20201111153950)
进入之后的面板

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111153957)

# Docker镜像讲解

### 镜像原理之联合文件系统

#### **镜像是什么**

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，他包含运行某个软件所需的所有内容，包括代码、运行时库、环境变量和配置文件。

所有应用，直接打包docker镜像，就可以直接跑起来！

**如何得到镜像**

- 从远程仓库下载
- 别人拷贝给你
- 自己制作一个镜像 DockerFile

#### **Docker镜像加载原理**

> UnionFs （联合文件系统）

UnionFs（联合文件系统）：Union文件系统（UnionFs）是一种分层、轻量级并且高性能的文件系统，他支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（ unite several directories into a single virtual filesystem)。Union文件系统是 Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像
特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

> Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。
boots(boot file system）主要包含 bootloader和 Kernel, bootloader主要是引导加 kernel, Linux刚启动时会加bootfs文件系统，在 Docker镜像的最底层是 boots。这一层与我们典型的Linux/Unix系统是一样的，包含boot加載器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由 bootfs转交给内核，此时系统也会卸载bootfs。
rootfs（root file system),在 bootfs之上。包含的就是典型 Linux系统中的/dev,/proc,/bin,/etc等标准目录和文件。 rootfs就是各种不同的操作系统发行版，比如 Ubuntu, Centos等等。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111153939)

平时我们安装进虚拟机的CentOS都是好几个G，为什么Docker这里才200M？

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111153944)

对于个精简的OS,rootfs可以很小，只需要包合最基本的命令，工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的Linux发行版， boots基本是一致的， rootfs会有差別，因此不同的发行版可以公用bootfs.

虚拟机是分钟级别，容器是秒级！

#### 分层理解

> 分层的镜像

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层层的在下载

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111155727)

**思考：为什么Docker镜像要采用这种分层的结构呢？**

最大的好处，我觉得莫过于资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。

查看镜像分层的方式可以通过docker image inspect 命令

```shell
[root@iZwz97ikj14iruzlnq4vr3Z ~]#  docker image inspect redis          
[
    {
        "Id": "sha256:f9b9909726890b00d2098081642edf32e5211b7ab53563929a47f250bcdc1d7c",
        "RepoTags": [
            "redis:latest"
        ],
        "RepoDigests": [
            "redis@sha256:399a9b17b8522e24fbe2fd3b42474d4bb668d3994153c4b5d38c3dafd5903e32"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2020-05-02T01:40:19.112130797Z",
        "Container": "d30c0bcea88561bc5139821227d2199bb027eeba9083f90c701891b4affce3bc",
        "ContainerConfig": {
            "Hostname": "d30c0bcea885",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.0.1",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.0.1.tar.gz",
                "REDIS_DOWNLOAD_SHA=b8756e430479edc162ba9c44dc89ac394316cd482f2dc6b91bcd5fe12593f273"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"redis-server\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:704c602fa36f41a6d2d08e49bd2319ccd6915418f545c838416318b3c29811e0",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "18.09.7",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.0.1",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.0.1.tar.gz",
                "REDIS_DOWNLOAD_SHA=b8756e430479edc162ba9c44dc89ac394316cd482f2dc6b91bcd5fe12593f273"
            ],
            "Cmd": [
                "redis-server"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:704c602fa36f41a6d2d08e49bd2319ccd6915418f545c838416318b3c29811e0",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 104101893,
        "VirtualSize": 104101893,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/adea96bbe6518657dc2d4c6331a807eea70567144abda686588ef6c3bb0d778a/diff:/var/lib/docker/overlay2/66abd822d34dc6446e6bebe73721dfd1dc497c2c8063c43ffb8cf8140e2caeb6/diff:/var/lib/docker/overlay2/d19d24fb6a24801c5fa639c1d979d19f3f17196b3c6dde96d3b69cd2ad07ba8a/diff:/var/lib/docker/overlay2/a1e95aae5e09ca6df4f71b542c86c677b884f5280c1d3e3a1111b13644b221f9/diff:/var/lib/docker/overlay2/cd90f7a9cd0227c1db29ea992e889e4e6af057d9ab2835dd18a67a019c18bab4/diff",
                "MergedDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/merged",
                "UpperDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/diff",
                "WorkDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:c2adabaecedbda0af72b153c6499a0555f3a769d52370469d8f6bd6328af9b13",
                "sha256:744315296a49be711c312dfa1b3a80516116f78c437367ff0bc678da1123e990",
                "sha256:379ef5d5cb402a5538413d7285b21aa58a560882d15f1f553f7868dc4b66afa8",
                "sha256:d00fd460effb7b066760f97447c071492d471c5176d05b8af1751806a1f905f8",
                "sha256:4d0c196331523cfed7bf5bafd616ecb3855256838d850b6f3d5fba911f6c4123",
                "sha256:98b4a6242af2536383425ba2d6de033a510e049d9ca07ff501b95052da76e894"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

**理解：**

所有的 Docker镜像都起始于一个基础镜像层，当进行修改或培加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创健第三个镜像层该像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点.

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111155958)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111160004)

上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件
下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111160006)

这种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中，Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统

Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的文件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。

Docker在 Windows上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW 。

下图展示了与系统显示相同的三层镜像。所有镜像层堆并合井，对外提供统一的视图。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111160022)

> 特点

Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201111160009)

## commit镜像

```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[版本TAG]
```

实战测试

```shell
# 1、启动一个默认的tomcat
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -d -p 8080:8080 tomcat
de57d0ace5716d27d0e3a7341503d07ed4695ffc266aef78e0a855b270c4064e

# 2、发现这个默认的tomcat 是没有webapps应用，官方的镜像默认webapps下面是没有文件的！
#docker exec -it 容器id /bin/bash
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker exec -it de57d0ace571 /bin/bash
root@de57d0ace571:/usr/local/tomcat# 

# 3、从webapps.dist拷贝文件进去webapp
root@de57d0ace571:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@de57d0ace571:/usr/local/tomcat# cd webapps
root@de57d0ace571:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager

# 4、将操作过的容器通过commit调教为一个镜像！我们以后就使用我们修改过的镜像即可，而不需要每次都重新拷贝webapps.dist下的文件到webapps了，这就是我们自己的一个修改的镜像。
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[TAG]
docker commit -a="kuangshen" -m="add webapps app" 容器id tomcat02:1.0

[root@iz2zeak7sgj6i7hrb2g862z ~]# docker commit -a="csp提交的" -m="add webapps app" de57d0ace571 tomcat02:1.0
sha256:d5f28a0bb0d0b6522fdcb56f100d11298377b2b7c51b9a9e621379b01cf1487e

[root@iz2zeak7sgj6i7hrb2g862z ~]# docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
tomcat02              1.0                 d5f28a0bb0d0        14 seconds ago      652MB
tomcat                latest              1b6b1fe7261e        5 days ago          647MB
nginx                 latest              9beeba249f3e        5 days ago          127MB
mysql                 5.7                 b84d68d0a7db        5 days ago          448MB
elasticsearch         7.6.2               f29a1ee41030        8 weeks ago         791MB
portainer/portainer   latest              2869fc110bf7        2 months ago        78.6MB
centos                latest              470671670cac        4 months ago        237MB
hello-world           latest              bf756fb1ae65        4 months ago        13.3kB
```

如果你想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像，就好比我们我们使用虚拟机的快照。

# 容器数据卷

## 什么是容器数据卷

将应用和环境打包成一个镜像！

数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！需求：**数据可以持久化**

MySQL，容器删除了，删库跑路！需求：**MySQL数据可以存储在本地！**

容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地！

这就是卷技术！目录的挂载，将我们容器内的目录，挂载到Linux上面！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115092857)

**总结一句话：容器的持久化和同步操作！容器间也是可以数据共享的！**

## 使用数据卷

> 方式一 ：直接使用命令挂载 -v

```shell
-v, --volume list                    Bind mount a volume

docker run -it -v 主机目录:容器内目录  -p 主机端口:容器内端口
# /home/ceshi：主机home目录下的ceshi文件夹  映射：centos容器中的/home
[root@iz2zeak7 home]# docker run -it -v /home/ceshi:/home centos /bin/bash
#这时候主机的/home/ceshi文件夹就和容器的/home文件夹关联了,二者可以实现文件或数据同步了

#通过 docker inspect 容器id 查看
[root@iz2zeak7sgj6i7hrb2g862z home]# docker inspect 6064c490c371
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115101102)

测试文件的同步

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115101111)

再来测试！

1、停止容器

2、宿主机修改文件

3、启动容器

4、容器内的数据依旧是同步的

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115101234)

好处：我们以后修改只需要在本地修改即可，容器内会自动同步！

## 实战：安装MySQL

**思考：MySQL的数据持久化的问题**

```shell
# 获取mysql镜像
[root@iz2zeak7sgj6i7hrb2g862z home]# docker pull mysql:5.7

# 运行容器,需要做数据挂载 #安装启动mysql，需要配置密码的，这是要注意点！
# 参考官网hub 
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

#启动我们得
-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置
-- name 容器名字
$ docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql03 mysql:5.7

# 启动成功之后，我们在本地使用sqlyog来测试一下
# sqlyog-连接到服务器的3306--和容器内的3306映射 

# 在本地测试创建一个数据库，查看一下我们映射的路径是否ok！
```

**测试连接**：注意3310端口要在阿里云服务器的安全组中打开，否则无法连接。

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201115102321)

当我们在本地用SQLyog新建名称为test的数据库时候，容器容器也会创建

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201115102318)

假设我们将包含mysql的容器删除时，

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115102329)

发现，**我们挂载到本地的数据卷依旧没有丢失，这就实现了容器数据持久化功能**。

## 具名和匿名挂载

```shell
# 匿名挂载     -v 容器内路径!
$ docker run -d -P --name nginx01 -v /etc/nginx nginx

# 查看所有的volume(卷)的情况
$ docker volume ls    
DRIVER           VOLUME NAME # 容器内的卷名(匿名卷挂载)
local            21159a8518abd468728cdbe8594a75b204a10c26be6c36090cde1ee88965f0d0
local            b17f52d38f528893dd5720899f555caf22b31bf50b0680e7c6d5431dbda2802c
         
# 这里发现，这种就是匿名挂载，我们在 -v只写了容器内的路径，没有写容器外的路径！

# 具名挂载 -P:表示随机映射端口
$ docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
9663cfcb1e5a9a1548867481bfddab9fd7824a6dc4c778bf438a040fe891f0ee

# 查看所有的volume(卷)的情况
$ docker volume ls                  
DRIVER            VOLUME NAME
local             21159a8518abd468728cdbe8594a75b204a10c26be6c36090cde1ee88965f0d0
local             b17f52d38f528893dd5720899f555caf22b31bf50b0680e7c6d5431dbda2802c
local             juming-nginx #多了一个名字


# 通过 -v 卷名：查看容器内路径
# 查看一下这个卷
$ docker volume inspect juming-nginx
[
    {
        "CreatedAt": "2020-05-23T13:55:34+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data", #默认目录
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115103353)

所有的docker容器内的卷，没有指定目录的情况下都是在**/var/lib/docker/volumes/自定义的卷名/_data**下，
**如果指定了目录，docker volume ls 是查看不到的**。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115103541)

**区分三种挂载方式**

```shell
# 三种挂载： 匿名挂载、具名挂载、指定路径挂载
-v 容器内路径			 #匿名挂载
-v 卷名：容器内路径		   #具名挂载
-v /宿主机路径：容器内路径  #指定路径挂载 docker volume ls 是查看不到的
```

拓展：

```shell
# 通过 -v 容器内路径： ro rw 改变读写权限
ro #readonly 只读
rw #readwrite 可读可写
$ docker run -d -P --name nginx05 -v juming:/etc/nginx:ro nginx
$ docker run -d -P --name nginx05 -v juming:/etc/nginx:rw nginx

# ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内部是无法操作！
```

## 初始Dockerfile

**Dockerfile 就是用来构建docker镜像的构建文件**！命令脚本！先体验一下！

通过这个**脚本可以生成镜像**，镜像是一层一层的，脚本是一个个的命令，每个命令都是一层！

```shell
# 创建一个dockerfile文件，名字可以随便 建议Dockerfile
# 文件中的内容： 指令(大写) + 参数
$ vim dockerfile1
    FROM centos 					# 当前这个镜像是以centos为基础的

    VOLUME ["volume01","volume02"] 	  # 挂载卷的卷目录列表(多个目录)

    CMD echo "-----end-----"		 # 输出一下用于测试
    CMD /bin/bash					# 默认走bash控制台

# 这里的每个命令，就是镜像的一层！
# 构建出这个镜像 
-f dockerfile1 			# f代表file，指这个当前文件的地址(这里是当前目录下的dockerfile1)
-t caoshipeng/centos 	# t就代表target，指目标目录(注意caoshipeng镜像名前不能加斜杠‘/’，后面可以加版本号  镜像名:版本号 )
. 						# 表示生成在当前目录下
$ docker build -f dockerfile1 -t caoshipeng/centos .
Sending build context to Docker daemon   2.56kB
Step 1/4 : FROM centos
latest: Pulling from library/centos
8a29a15cefae: Already exists 
Digest: sha256:fe8d824220415eed5477b63addf40fb06c3b049404242b31982106ac204f6700
Status: Downloaded newer image for centos:latest
 ---> 470671670cac
Step 2/4 : VOLUME ["volume01","volume02"] 			# 卷名列表
 ---> Running in c18eefc2c233
Removing intermediate container c18eefc2c233
 ---> 623ae1d40fb8
Step 3/4 : CMD echo "-----end-----"					# 输出 脚本命令
 ---> Running in 70e403669f3c
Removing intermediate container 70e403669f3c
 ---> 0eba1989c4e6
Step 4/4 : CMD /bin/bash
 ---> Running in 4342feb3a05b
Removing intermediate container 4342feb3a05b
 ---> f4a6b0d4d948
Successfully built f4a6b0d4d948
Successfully tagged caoshipeng/centos:latest

# 查看自己构建的镜像
$ docker images
REPOSITORY          TAG          IMAGE ID            CREATED              SIZE
caoshipeng/centos   latest       f4a6b0d4d948        About a minute ago   237MB
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115152916)

> 启动自己写的容器镜像

```shell
$ docker run -it f4a6b0d4d948 /bin/bash	# 运行自己写的镜像
$ ls -l 								# 查看目录
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115152947)

这个卷和外部一定有一个同步的目录

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115152955)

查看一下卷挂载

```shell
# docker inspect 容器id
$ docker inspect ca3b45913df5
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115153000)

测试一下刚才的文件是否同步出去了！

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201115153018.png)

这种方式使用的十分多，因为我们通常会构建自己的镜像！

假设构建镜像时候没有挂载卷，要手动镜像挂载 -v 卷名：容器内路径！

## 数据卷容器

**多个MySQL同步数据**！

命名的容器挂载数据卷！

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201115155744)

```shell
# 测试 启动3个容器，通过刚才自己写的镜像启动
# 创建docker01：因为我本机是最新版，故这里用latest，狂神老师用的是1.0如下图
$ docker run -it --name docker01 caoshipeng/centos:latest

# 查看容器docekr01内容
$ ls
bin  home   lost+found	opt   run   sys  var
dev  lib    media	proc  sbin  tmp  volume01
etc  lib64  mnt		root  srv   usr  volume02

# 不关闭该容器退出
CTRL + Q + P  

# 创建docker02: 并且让docker02 继承 docker01
$ docker run -it --name docker02 --volumes-from docker01 caoshipeng/centos:latest

# 查看容器docker02内容
$ ls
bin  home   lost+found	opt   run   sys  var
dev  lib    media	proc  sbin  tmp  volume01
etc  lib64  mnt		root  srv   usr  volume02
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155741)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201115155737)

```shell
# 再新建一个docker03同样继承docker01
$ docker run -it --name docker03 --volumes-from docker01 caoshipeng/centos:latest
$ cd volume01	#进入volume01 查看是否也同步docker01的数据
$ ls 
docker01.txt

# 测试：可以删除docker01，查看一下docker02和docker03是否可以访问这个文件
# 测试发现：数据依旧保留在docker02和docker03中没有被删除
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155734)

**多个mysql实现数据共享**

```shell
$ docker run -d -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

$ docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01  mysql:5.7

# 这个时候，可以实现两个容器数据同步！
```

结论：

**容器之间的配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止**。

**但是一旦你持久化到了本地，这个时候，本地的数据是不会删除的**！

# DockerFile

## DockerFile介绍

`dockerfile`是用来构建docker镜像的文件！命令参数脚本！

构建步骤：

1、 编写一个dockerfile文件

2、 docker build 构建称为一个镜像

3、 docker run运行镜像

4、 docker push发布镜像（DockerHub 、阿里云仓库)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155756)

点击后跳到一个Dockerfile

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155803)

很多官方镜像都是基础包，很多功能没有，我们通常会自己搭建自己的镜像！

## DockerFile构建过程

**基础知识**：

1、每个保留关键字(指令）都是必须是大写字母

2、执行从上到下顺序

3、#表示注释

4、每一个指令都会创建提交一个新的镜像层，并提交！

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155819)

Dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单！

Docker镜像逐渐成企业交付的标准，必须要掌握！

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages：通过DockerFile构建生成的镜像，最终发布和运行产品。

Docker容器：容器就是镜像运行起来提供服务。

## DockerFile的指令

```shell
FROM		# 基础镜像，一切从这里开始构建
MAINTAINER   # 镜像是谁写的， 姓名+邮箱
RUN			# 镜像构建的时候需要运行的命令
ADD			# 步骤，tomcat镜像，这个tomcat压缩包！添加内容 添加同目录
WORKDIR		# 镜像的工作目录
VOLUME		# 挂载的目录
EXPOSE		# 保留端口配置
CMD			# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT	 # 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD		# 当构建一个被继承 DockerFile 这个时候就会运行onbuild的指令，触发指令
COPY		# 类似ADD，将我们文件拷贝到镜像中
ENV	 		# 构建的时候设置环境变量！
```

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201115155822)

## 实战测试

scratch 镜像

```shell
FROM scratch
ADD centos-7-x86_64-docker.tar.xz /

LABEL \
    org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20200504" \
    org.opencontainers.image.title="CentOS Base Image" \
    org.opencontainers.image.vendor="CentOS" \
    org.opencontainers.image.licenses="GPL-2.0-only" \
    org.opencontainers.image.created="2020-05-04 00:00:00+01:00"

CMD ["/bin/bash"]
```

**Docker Hub 中 99%的镜像都是从这个基础镜像过来的 FROM scratch**，然后配置需要的软件和配置来进行构建。

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201115155836)

> 创建一个自己的centos

```shell
# 1./home下新建dockerfile目录
$ mkdir dockerfile

# 2. dockerfile目录下新建mydockerfile-centos文件
$ vim mydockerfile-centos

# 3.编写Dockerfile配置文件
FROM centos							# 基础镜像是官方原生的centos
MAINTAINER cao<1165680007@qq.com> 	  # 作者

ENV MYPATH /usr/local				# 配置环境变量的目录 
WORKDIR $MYPATH						# 将工作目录设置为 MYPATH

RUN yum -y install vim				# 给官方原生的centos 增加 vim指令
RUN yum -y install net-tools		# 给官方原生的centos 增加 ifconfig命令

EXPOSE 80							# 暴露端口号为80

CMD echo $MYPATH					# 输出下 MYPATH 路径
CMD echo "-----end----"				
CMD /bin/bash						# 启动后进入 /bin/bash

# 4.通过这个文件构建镜像
# 命令： docker build -f 文件路径 -t 镜像名:[tag] .
$ docker build -f mydockerfile-centos -t mycentos:0.1 .

# 5.出现下图后则构建成功
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155840)

```shell
$ docker images
REPOSITORY        TAG             IMAGE ID          CREATED           SIZE
mycentos          0.1             cbf5110a646d      2 minutes ago     311MB

# 6.测试运行
$ docker run -it mycentos:0.1 		# 注意带上版本号，否则每次都回去找最新版latest

$ pwd	
/usr/local					# 与Dockerfile文件中 WORKDIR 设置的 MYPATH 一致
$ vim					    # vim 指令可以使用
$ ifconfig     			     # ifconfig 指令可以使用

# docker history 镜像id 查看镜像构建历史步骤
$ docker history 镜像id
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155843)

我们可以列出本地进行的变更历史

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155847)

我们平时拿到一个镜像，可以用 “docker history 镜像id” 研究一下是什么做的

> CMD 和 ENTRYPOINT区别

```shell
CMD					# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代。
ENTRYPOINT			# 指定这个容器启动的时候要运行的命令，可以追加命令
```

**测试cmd**

```shell
# 编写dockerfile文件
$ vim dockerfile-test-cmd
FROM centos
CMD ["ls","-a"]					# 启动后执行 ls -a 命令

# 构建镜像
$ docker build  -f dockerfile-test-cmd -t cmd-test:0.1 .

# 运行镜像
$ docker run cmd-test:0.1		# 由结果可得，运行后就执行了 ls -a 命令
.
..
.dockerenv
bin
dev
etc
home

# 想追加一个命令  -l 成为ls -al：展示列表详细数据
$ docker run cmd-test:0.1 -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"-l\":
executable file not found in $PATH": unknown.
ERRO[0000] error waiting for container: context canceled 

# cmd的情况下 -l 替换了CMD["ls","-l"] 而 -l  不是命令所以报错
```

**测试ENTRYPOINT**

```shell
# 编写dockerfile文件
$ vim dockerfile-test-entrypoint
FROM centos
ENTRYPOINT ["ls","-a"]

# 构建镜像
$ docker build  -f dockerfile-test-entrypoint -t cmd-test:0.1 .

# 运行镜像
$ docker run entrypoint-test:0.1
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found ...

# 我们的命令，是直接拼接在我们得ENTRYPOINT命令后面的
$ docker run entrypoint-test:0.1 -l
total 56
drwxr-xr-x   1 root root 4096 May 16 06:32 .
drwxr-xr-x   1 root root 4096 May 16 06:32 ..
-rwxr-xr-x   1 root root    0 May 16 06:32 .dockerenv
lrwxrwxrwx   1 root root    7 May 11  2019 bin -> usr/bin
drwxr-xr-x   5 root root  340 May 16 06:32 dev
drwxr-xr-x   1 root root 4096 May 16 06:32 etc
drwxr-xr-x   2 root root 4096 May 11  2019 home
lrwxrwxrwx   1 root root    7 May 11  2019 lib -> usr/lib
lrwxrwxrwx   1 root root    9 May 11  2019 lib64 -> usr/lib64 ....
```

Dockerfile中很多命令都十分的相似，我们需要了解它们的区别，我们最好的学习就是对比他们然后测试效果！

## 实战：Tomcat镜像

##### 1、准备镜像文件

```
准备tomcat 和 jdk 到当前目录，编写好README
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155852)

##### 2、编写dokerfile

```shell
$ vim Dockerfile
FROM centos 								   # 基础镜像centos
MAINTAINER cao<1165680007@qq.com>				# 作者
COPY README /usr/local/README 					# 复制README文件
ADD jdk-8u231-linux-x64.tar.gz /usr/local/ 		 # 添加jdk，ADD 命令会自动解压
ADD apache-tomcat-9.0.35.tar.gz /usr/local/ 	 # 添加tomcat，ADD 命令会自动解压
RUN yum -y install vim								# 安装 vim 命令
ENV MYPATH /usr/local 								# 环境变量设置 工作目录
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_231 				# 环境变量： JAVA_HOME环境变量
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.35 	# 环境变量： tomcat环境变量
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.35

# 设置环境变量 分隔符是：
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin 	

EXPOSE 8080 										# 设置暴露的端口

CMD /usr/local/apache-tomcat-9.0.35/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.35/logs/catalina.out 					# 设置默认命令
```

##### 3、构建镜像

```shell
# 因为dockerfile命名使用默认命名 因此不用使用-f 指定文件
$ docker build -t mytomcat:0.1 .
```

##### 4、run镜像

```shell
# -d:后台运行 -p:暴露端口 --name:别名 -v:绑定路径 
$ docker run -d -p 8080:8080 --name tomcat01 
-v /home/kuangshen/build/tomcat/test:/usr/local/apache-tomcat-9.0.35/webapps/test 
-v /home/kuangshen/build/tomcat/tomcatlogs/:/usr/local/apache-tomcat-9.0.35/logs mytomcat:0.1
```

##### 5、访问测试

```shell
$ docker exec -it 自定义容器的id /bin/bash

$ curl localhost:8080
```

##### 6、发布项目

(由于做了卷挂载，我们直接在本地编写项目就可以发布了！)

发现：项目部署成功，可以直接访问！

我们以后开发的步骤：需要掌握Dockerfile的编写！我们之后的一切都是使用docker镜像来发布运行！

## 发布自己的镜像

> 发布到 Docker Hub

1、地址 https://hub.docker.com/

2、确定这个账号可以登录

3、登录

```shell
$ docker login --help
Usage:  docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username

$ docker login -u 你的用户名 -p 你的密码
```

4、提交 push镜像

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155858)

```shell
# 会发现push不上去，因为如果没有前缀的话默认是push到 官方的library
# 解决方法：
# 第一种 build的时候添加你的dockerhub用户名，然后再push就可以放到自己的仓库了
$ docker build -t kuangshen/mytomcat:0.1 .

# 第二种 使用docker tag  然后再次push
$ docker tag 容器id kuangshen/mytomcat:1.0  #然后再次push
$ docker push kuangshen/mytomcat:1.0
```

> 发布到 阿里云镜像服务上

看官网 很详细https://cr.console.aliyun.com/repository/

1.创建命名空间

![image-20201117102854055](https://gitee.com/xudongyin/img/raw/master/img/20201117102856.png)

2.创建镜像仓库

![image-20201117103112303](https://gitee.com/xudongyin/img/raw/master/img/20201117103114.png)

3.点击镜像仓库，查看基本信息和镜像操作步骤

![image-20201117103300163](https://gitee.com/xudongyin/img/raw/master/img/20201117103301.png)

```shell
$ sudo docker login --username=起名字是个问题阿 registry.cn-shenzhen.aliyuncs.com #登录
$ sudo docker tag [ImageId] registry.cn-shenzhen.aliyuncs.com/xudongyin/test:[镜像版本号]   #镜像版本号自己定义
$ sudo docker push registry.cn-shenzhen.aliyuncs.com/xudongyin/test:[镜像版本号]

# 根据id 修改镜像命名
$sudo docker tag a5ef1f32aaae registry.cn-shenzhen.aliyuncs.com/xudongyin/test:1.0
# 上传镜像
$ sudo docker push registry.cn-shenzhen.aliyuncs.com/xudongyin/test:1.0
```

## 小结

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155902)

# Docker 网络

## 理解Docker 0

学习之前**清空下前面的docker 镜像、容器**

```shell
# 删除全部容器
$ docker rm -f $(docker ps -aq)

# 删除全部镜像
$ docker rmi -f $(docker images -aq)
```

> 测试

![image-20201117110247278](https://gitee.com/xudongyin/img/raw/master/img/20201117110249.png)

**三个网络**

> 问题： docker 是如果处理容器网络访问的？

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155921)

```shell
# 测试  运行一个tomcat
$ docker run -d -P --name tomcat01 tomcat

# 查看容器内部网络地址
$ docker exec -it 容器id ip addr
# 发现容器启动的时候会得到一个 eth0@if91 ip地址，docker分配！
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
261: eth0@if91: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:12:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.18.0.2/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever

       
# 思考？ linux能不能ping通容器内部！ 可以 容器内部可以ping通外界吗？ 可以！
$ ping 172.18.0.2
PING 172.18.0.2 (172.18.0.2) 56(84) bytes of data.
64 bytes from 172.18.0.2: icmp_seq=1 ttl=64 time=0.069 ms
64 bytes from 172.18.0.2: icmp_seq=2 ttl=64 time=0.074 ms
```

> 原理

1、我们每启动一个docker容器，docker就会给docker容器分配一个ip，我们只要安装了docker，就会有一个docker0桥接模式，使用的技术是veth-pair技术！

讲解veth-pair 技术的博客    https://www.cnblogs.com/bakari/p/10613710.html

再次测试 ip addr

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155927)

2 、再启动一个容器测试，发现又多了一对网络

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155932)

```shell
# 我们发现这个容器带来网卡，都是一对对的
# veth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一端连着协议，一端彼此相连
# 正因为有这个特性 veth-pair 充当一个桥梁，连接各种虚拟网络设备的
# OpenStac,Docker容器之间的连接，OVS的连接，都是使用evth-pair技术
```

3、我们来测试下tomcat01和tomcat02是否可以ping通

```shell
# 获取tomcat01的ip 172.17.0.2
$ docker-tomcat docker exec -it tomcat01 ip addr  
550: eth0@if551: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
       
# 让tomcat02 ping tomcat01       
$ docker-tomcat docker exec -it tomcat02 ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.098 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.071 ms

# 结论：容器和容器之间是可以互相ping通
```

**网络模型图**

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155937)

结论：tomcat01和tomcat02共用一个路由器，docker0。

所有的容器不指定网络的情况下，都是docker0路由的，docker会给我们的容器分配一个默认的可用ip。

> 小结

Docker使用的是Linux的桥接，宿主机是一个Docker容器的网桥 docker0

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155943)

Docker中所有网络接口都是虚拟的，虚拟的转发效率高（内网传递文件）

只要容器删除，对应的网桥一对就没了！

**思考一个场景：我们编写了一个微服务，database url=ip: 项目不重启，数据ip换了，我们希望可以处理这个问题，可以通过名字来进行访问容器**？

##### –-link

```shell
$ docker exec -it tomcat02 ping tomca01   # ping不通
ping: tomca01: Name or service not known

# 运行一个tomcat03 --link tomcat02 
$ docker run -d -P --name tomcat03 --link tomcat02 tomcat
5f9331566980a9e92bc54681caaac14e9fc993f14ad13d98534026c08c0a9aef

# 3连接2
# 用tomcat03 ping tomcat02 可以ping通
$ docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.115 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.080 ms

# 2连接3
$ docker exec -it tomcat02 ping tomca03   # ping不通
ping: tomcat03: Name or service not known
```

**探究：**

~~~shell
#查看所有网络指令
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker network --help
#查看所有网段
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker network ls  
NETWORK ID          NAME                DRIVER              SCOPE
80c4ffaf3459        bridge              bridge              local
8fcb7145beb0        host                host                local
853163a1e0db        none                null                local
#查看指定网络的信息
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker network  inspect 80c4ffaf3459
~~~

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155949)

~~~shell
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker inspect tomcat03
~~~

![image-20201117160914107](https://gitee.com/xudongyin/img/raw/master/img/20201117160915.png)

~~~shell
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker inspect tomcat02
~~~

![image-20201117160839454](https://gitee.com/xudongyin/img/raw/master/img/20201117160841.png)

查看tomcat03里面的/etc/hosts发现有tomcat02的配置

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115155955)

**–link 本质就是在hosts配置中添加映射**

现在使用Docker已经**不建议使用–link**了！

自定义网络，不适用docker0！

docker0问题：**不支持容器名连接访问**！

## 自定义网络

```shell
docker network
connect     -- Connect a container to a network
create      -- Creates a new network with a name specified by the
disconnect  -- Disconnects a container from a network
inspect     -- Displays detailed information on a network
ls          -- Lists all the networks created by the user
prune       -- Remove all unused networks
rm          -- Deletes one or more networks
```

> 查看所有的docker网络

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160003)

**网络模式**

bridge ：桥接 docker（默认，自己创建也是用bridge模式）

none ：不配置网络，一般不用

host ：和宿主机共享网络

container ：容器网络连通（用得少！局限很大）

测试

```shell
# 我们直接启动的命令 --net bridge,而这个就是我们得docker0
# bridge就是docker0
$ docker run -d -P --name tomcat01 tomcat
等价于 => docker run -d -P --name tomcat01 --net bridge tomcat

# docker0，特点：默认，域名不能访问。 --link可以打通连接，但是很麻烦！
# 我们可以 自定义一个网络
$ docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160008)

```shell
$ docker network inspect mynet;
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160011)

启动两个tomcat,再次查看网络情况

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160015)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160018)

在自定义的网络下，服务可以互相ping通，不用使用–link

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160026)

我们自定义的网络docker当我们维护好了对应的关系，推荐我们平时这样使用网络！

好处：

redis -不同的集群使用不同的网络，保证集群是安全和健康的

mysql-不同的集群使用不同的网络，保证集群是安全和健康的

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160030)

## 网络连通

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160033)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160037)

```shell
# 测试两个不同的网络连通  再启动两个tomcat 使用默认网络，即docker0
$ docker run -d -P --name tomcat01 tomcat
$ docker run -d -P --name tomcat02 tomcat
# 此时ping不通
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160045)

```shell
# 要将tomcat01 连通 tomcat—net-01 ，连通就是将 tomcat01加到 mynet网络
# 一个容器两个ip（tomcat01）
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker network connect  mynet tomcat01
[root@iZwz97ikj14iruzlnq4vr3Z /]# docker exec -it tomcat01 ping tomcat-mynet01
PING tomcat-mynet01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat-mynet01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.122 ms
64 bytes from tomcat-mynet01.mynet (192.168.0.2): icmp_seq=2 ttl=64 time=0.107 ms
^C
--- tomcat-mynet01 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1ms
rtt min/avg/max/mdev = 0.107/0.114/0.122/0.013 ms
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160049)

```shell
# 01连通 ，加入后此时，已经可以tomcat01 和 tomcat-01-net ping通了
# 02是依旧不通的
```

结论：假设要跨网络操作别人，就需要使用docker network connect 连通！

## 实战：部署Redis集群

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160053)

```shell
# 创建网卡
docker network create redis --subnet 172.38.0.0/16
# 通过脚本创建六个redis配置
for port in $(seq 1 6);\
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >> /mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done

# 通过脚本运行六个redis
for port in $(seq 1 6);\
do
docker run -p 637${port}:6379 -p 1667${port}:16379 --name redis-${port} \
-v /mydata/redis/node-${port}/data:/data \
-v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf 
done

#进入redis
docker exec -it redis-1 /bin/sh #redis默认没有bash

#进行集群开启配置
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379  --cluster-replicas 1
```

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160056)

docker搭建redis集群完成！

~~~shell
/data # redis-cli -c  #进入 redis 客户端
127.0.0.1:6379> cluster nodes
789f0f51aa46656d9754e9785ab4aa2c5962fd0b 172.38.0.11:6379@16379 myself,master - 0 1605607079000 1 connected 0-5460
60c6c6ce439d8aa19e6485c5fc6df80e074037f5 172.38.0.12:6379@16379 master - 0 1605607079408 2 connected 5461-10922
71d85b690af2c616fc95a9d664dbe6d025d50936 172.38.0.16:6379@16379 slave 60c6c6ce439d8aa19e6485c5fc6df80e074037f5 0 1605607079507 6 connected
650e4b3d95035fa72295ab3a36d2b3783f3b6864 172.38.0.13:6379@16379 master - 0 1605607079000 3 connected 10923-16383
087c5c3db81521d01ebe004e075e318e89f417fe 172.38.0.14:6379@16379 slave 650e4b3d95035fa72295ab3a36d2b3783f3b6864 0 1605607078405 4 connected
11ab2149e88c3a99a8d6937b789c705436ac1e32 172.38.0.15:6379@16379 slave 789f0f51aa46656d9754e9785ab4aa2c5962fd0b 0 1605607078000 5 connected
~~~

![img](https://gitee.com/xudongyin/img/raw/master/img/20201115160100)

~~~shell
127.0.0.1:6379> set a b
-> Redirected to slot [15495] located at 172.38.0.13:6379
OK
#此时关闭 redis-1 ，进入 redis-2
172.38.0.13:6379> [root@iZwz97ikj14iruzlnq4vr3Z xu]# docker exec -it redis-2 /bin/sh
/data # redis-cli -c
127.0.0.1:6379> get a
-> Redirected to slot [15495] located at 172.38.0.13:6379
"b"
#退出 redis-2
172.38.0.13:6379> exit
/data # exit
#进入 redis-5
[root@iZwz97ikj14iruzlnq4vr3Z xu]# docker exec -it redis-5 /bin/sh
/data # redis-cli -c
127.0.0.1:6379> get a
-> Redirected to slot [15495] located at 172.38.0.13:6379
"b"

#此时集群状态
172.38.0.13:6379> cluster nodes
087c5c3db81521d01ebe004e075e318e89f417fe 172.38.0.14:6379@16379 slave 650e4b3d95035fa72295ab3a36d2b3783f3b6864 0 1605607380897 4 connected
71d85b690af2c616fc95a9d664dbe6d025d50936 172.38.0.16:6379@16379 slave 60c6c6ce439d8aa19e6485c5fc6df80e074037f5 0 1605607379391 6 connected
789f0f51aa46656d9754e9785ab4aa2c5962fd0b 172.38.0.11:6379@16379 master,fail - 1605607149672 1605607148000 1 connected  # redis-1 失效
60c6c6ce439d8aa19e6485c5fc6df80e074037f5 172.38.0.12:6379@16379 master - 0 1605607380000 2 connected 5461-10922
11ab2149e88c3a99a8d6937b789c705436ac1e32 172.38.0.15:6379@16379 master - 0 1605607380395 7 connected 0-5460
650e4b3d95035fa72295ab3a36d2b3783f3b6864 172.38.0.13:6379@16379 myself,master - 0 1605607380000 3 connected 10923-16383
~~~

我们使用docker之后，所有的技术都会慢慢变得简单起来！

## SpringBoot微服务打包Docker镜像

1、构建SpringBoot项目

2、打包运行

```
mvn package
```

3、编写dockerfile

```shell
FROM java:8
COPY *.jar /app.jar
CMD ["--server.port=8080"]
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
```

4、构建镜像

```shell
# 1.复制jar和DockerFIle到服务器
# 2.构建镜像
$ docker build -t xxxxx:xx  .
```

5、发布运行

以后我们使用了Docker之后，给别人交付就是一个镜像即可！

# Docker Compose

## 简介

原来Docker根据Dockerfile来创建运行镜像都是手动操作，并且操作的是单个容器，如果要运行多个容器做微服务的话这样是很不方便的。

Docker Compose 可以定义运行多个容器，轻松高效的管理容器。

> 官方介绍

定义、运行多个容器。
YAML file配置文件。
single command。命令 有哪些?

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration. To learn more about all the features of Compose, see the list of features.

所有的环境都可以使用 Compose

Compose works in all environments: production, staging, development, testing, as well as CI workflows. You can learn more about each case in Common Use Cases.

**三步骤：**

Using Compose is basically a three-step process:

1.  Define your app's environment withal **Dockerfile** so it can be reproduced anywhere.
   - Dockerfile保证我们的项目在任何地方都可以运行	
2. Define the services that make up your app in **docker-compose. yml** so they can be run together in an isolated environment.
  - services 什么是服务
  - docker-compose.yml 这个文件怎么写
3. Run **docker-compose up** and Compose starts and runs your entire app.
  - 启动项目

作用：批量容器编排

> Compose 理解

Compose是Docker官方的开源项目。需要安装!
Dockerfile 让程序在任何地方运行。 web服务。 redis、 mysql、nginx... 多个容器。run
Compose

~~~yml
version: "3.8"
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
~~~

docker-compose up  100 个服务。

Compose :重要的概念。

- 服务services， 容器。 应用。比如 web、 redis、 mysl....
- 项目project。 一组关联的容器。比如博客系统，由 web、mysql、wp组成

## 安装

1、下载

~~~shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
#这个可能快点!
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
~~~

![image-20201117225306726](https://gitee.com/xudongyin/img/raw/master/img/20201117225309.png)

2、授权

~~~shell
sudo chmod +x /usr/local/bin/docker-compose
~~~

![image-20201117225523768](https://gitee.com/xudongyin/img/raw/master/img/20201117225525.png)

## 体验

地址: https://docs.docker.com/compose/gettingstarted/
 一个python应用。计数器。使用了redis!

1. 准备工作

   1. ~~~shell
      $ mkdir composetest  #创建项目目录
      $ cd composetest   #进入目录
      ~~~

   2. 创建应用 app.py

      ~~~shell
      import time
      
      import redis
      from flask import Flask
      
      app = Flask(__name__)
      cache = redis.Redis(host='redis', port=6379)
      
      def get_hit_count():
          retries = 5
          while True:
              try:
                  return cache.incr('hits')
              except redis.exceptions.ConnectionError as exc:
                  if retries == 0:
                      raise exc
                  retries -= 1
                  time.sleep(0.5)
      
      @app.route('/')
      def hello():
          count = get_hit_count()
          return 'Hello World! I have been seen {} times.\n'.format(count)
          
      import time
      
      import redis
      from flask import Flask
      
      app = Flask(__name__)
      cache = redis.Redis(host='redis', port=6379)
      
      def get_hit_count():
          retries = 5
          while True:
              try:
                  return cache.incr('hits')
              except redis.exceptions.ConnectionError as exc:
                  if retries == 0:
                      raise exc
                  retries -= 1
                  time.sleep(0.5)
                  
      @app.route('/')
      def hello():
          count = get_hit_count()
          return 'Hello World! I have been seen {} times.\n'.format(count)
      
      if __name__ == "__main__":
      	app.run(host="0.0.0.0",debug=True)
      ~~~

   3. 创建 requirements.txt 

      ~~~shell
      flask
      redis
      ~~~

2. 创建 Dockerfile ，应用打包为镜像

   ~~~shell
   #用官方这个 Dockerfile 会使用到 flask 框架，下载极慢
   FROM python:3.7-alpine
   WORKDIR /code
   ENV FLASK_APP=app.py
   ENV FLASK_RUN_HOST=0.0.0.0
   RUN apk add --no-cache gcc musl-dev linux-headers
   COPY requirements.txt requirements.txt
   RUN pip install -r requirements.txt
   EXPOSE 5000
   COPY . .
   CMD ["flask", "run"]
   
   #将官方的 Dockerfile 修改为下面
   FROM python:3.6-alpine
   ADD . /code
   WORKDIR /code
   RUN pip install -r requirements.txt
   CMD ["python", "app.py"]
   #这告诉 Docker:
   #从 Python3.6 映像开始构建映像。
   #将当前目录添加 . 到 /code 映像中的路径中。
   #将工作目录设置为 /code.
   #安装 Python 依赖项。
   #将容器的默认命令设置为 python app.py。
   ~~~

3. 创建 docker-compose.yml 文件 (定义整个服务, 需要的环境。web、 redis) 完整的上线服务!

   ~~~yml
   version: "3.8"
   services:
     web:
       build: .
       ports:
         - "5000:5000"
       volumes:
         - .:/code
     redis:
       image: "redis:alpine"
   ~~~

4. 运行项目  `docker-compose up`  

   项目运行成功

   ![image-20201118000221062](https://gitee.com/xudongyin/img/raw/master/img/20201118000222.png)

   如果项目运行不起来，就先执行  `docker-compose build` ,然后再执行  `docker-compose up`  

   ![image-20201118000048822](https://gitee.com/xudongyin/img/raw/master/img/20201118000050.png)

流程:
1、创建网络
2、执行Docker-compose yaml
3、启动服务。
Starting composetest_web_1   ... done
Starting composetest_redis_1 ... done

![image-20201118102432421](https://gitee.com/xudongyin/img/raw/master/img/20201118102438.png)

![image-20201118103339149](https://gitee.com/xudongyin/img/raw/master/img/20201118103341.png)

只执行一句 `docker-compose up` 就自动把需要的镜像都下载了

~~~shell
[root@iZwz97ikj14iruzlnq4vr3Z ~]# docker service ls
Error response from daemon: This node is not a swarm manager. Use "docker swarm init" or "docker swarm join" to connect this node to swarm and try again.
~~~

默认的服务名       文件名_ 服务名 _ num
多个服务器。集群A 、  B。      num就是副本数量
服务redis服务=> 4个副本。
集群状态。服务都不可能只有一个运行实例。弹性、10  HA 高并发。
kubectl  service  负载均衡。

> 网络规则

![image-20201118104222408](https://gitee.com/xudongyin/img/raw/master/img/20201118104224.png)

启动项目，会自动创建一个网络，项目里面的**所有服务容器都是在同一个网络**下，并且**支持域名访问**

![image-20201118131001342](https://gitee.com/xudongyin/img/raw/master/img/20201118131003.png)

Ctrl+c  停止运行

![image-20201118134008733](https://gitee.com/xudongyin/img/raw/master/img/20201118134010.png)

docker-compose
以前都是单个docker run启动容器。
Docker Compose 通过docker-compose编写yaml配置文件、可以通过 compose 一键启动所有服务以及停止。

## yaml 规则

文档   https://docs.docker.com/compose/compose-file/

~~~shell
# 3层
version: "" # 版本
services:   # 服务
  redis:
    # 服务配置
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
# 其他配置：网络、数据卷、全局规则
networks:
volumes:
configs:
~~~

![image-20201118141717864](https://gitee.com/xudongyin/img/raw/master/img/20201118141720.png)

> 一个开源博客项目

 文档   https://docs.docker.com/compose/wordpress/

1、创建项目目录，进入目录

~~~shell
mkdir  my_wordpress
cd  my_wordpress/
~~~

2、编写 `docker-compose.yml` 文件

~~~yml
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}
~~~

3、运行项目

 `docker-compose up -d`  后台运行   用 `docker-compose down` 关闭

 `docker-compose up`  前台运行  用 `Ctrl+c` 关闭

## 实战

1、编写项目微服务

2、编写 Dockerfile

~~~shell
FROM java:8

COPY *.jar /app.jar

CMD ["--server.port=8080"]

EXPOSE 8080

ENTRYPOINT ["java","-jar","/app.jar"]
~~~

3、编写 docker-compose.yml 

~~~yml
version: "3.8"

services:
  xuapp:
    build: .
    image: xuapp
    depends_on:
      - redis
    ports:
    - "3344:8080"
  redis:
    image: redis
~~~

4、丢到服务器， `docker-compose up`运行	 

![image-20201118164008599](https://gitee.com/xudongyin/img/raw/master/img/20201118164010.png)

如果项目想重新部署构建，执行`docker-compose up --bulid`

~~~shell
docker-compose up --bulid  # 项目重新部署构建
~~~

**小结**

项目根据 docker-compose.yml 文件进行启动编排容器，未来项目只要 docker-compose up 直接运行。

**工程、服务、容器**

项目compose：三层

- 工程Porject
- 服务服务
- 容器运行实例！  docker    k8s   容器 . .

# Docker Swarm

## 准备工作

### 购买服务器

购买4台服务器

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118195840)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118195851)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118195935)![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118195908)

![image-20201118200019649](https://gitee.com/xudongyin/img/raw/master/img/20201118200022.png)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201118200047)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118200112)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118200246)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118200256)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118200325)

到此，服务器购买完毕，1主 3从！

### 4台服务器都安装Docker

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201118200553)

## 工作模式

![image-20201118200638550](https://gitee.com/xudongyin/img/raw/master/img/20201118200640.png)

## 搭建集群

![image-20201118201720720](https://gitee.com/xudongyin/img/raw/master/img/20201118201722.png)

![image-20201118201815193](https://gitee.com/xudongyin/img/raw/master/img/20201118201816.png)

![image-20201118201901744](https://gitee.com/xudongyin/img/raw/master/img/20201118201903.png)

这里 `docker swarm init --advertise-addr  172.24.82.149`  添加的 ip 是服务器私网，用私网不用流量也就是不用钱，用公网就要流量也就是要花钱，使用  `ip addr` 就可以查看服务器的 私网 ip

![image-20201118202059878](https://gitee.com/xudongyin/img/raw/master/img/20201118202101.png)

初始化主节点  `docker swarm init`

`docker swarm join` 加入到一个节点！

~~~shell
# 获取令牌
docker swarm join-token manager  #会生成一段命令字符串，别的服务器上运行这个命令就成为了当前主机的 manager 节点 
docker swarm join-token worker   #会生成一段命令字符串，别的服务器上运行这个命令就成为了当前主机的 worker 节点
~~~

![image-20201118202507661](https://gitee.com/xudongyin/img/raw/master/img/20201118202508.png)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201119094337)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201119094420.png)

![image-20201118203634484](https://gitee.com/xudongyin/img/raw/master/img/20201118203636.png)

![image-20201118203827984](https://gitee.com/xudongyin/img/raw/master/img/20201118203829.png)

1、生成主节点  init

2、加入（管理者，worker）

目标：双主双从，Leader 和 manager 节点都是主节点

## Raft协议

双主双从：假设一个节点挂了，其他节点是否可用？

Raft 协议：保证**主节点存活过半（大于总结点数/2的整数值）**才可以用。

实验：

1、将docker1机器停止，宕机！双主下，另一个主节点也不能使用了！因为 1 不大于 2/2，只是等于

![image-20201119095950984](https://gitee.com/xudongyin/img/raw/master/img/20201119100243.png)

2、再重新启动 docker1，此时 docker1 已不再是 Leader，而是 manager 节点

![image-20201119100347613](https://gitee.com/xudongyin/img/raw/master/img/20201119100349.png)

3、如果节点离开集群，STATUS 字段会变成 Down

~~~shell
docker swarm leave    # 节点离开集群
~~~

![image-20201119100857404](https://gitee.com/xudongyin/img/raw/master/img/20201119100859.png)

![image-20201119100628413](https://gitee.com/xudongyin/img/raw/master/img/20201119100631.png)

4、worker节点是工作的，manager节点是来操作管理节点的

## 体会

弹性、扩缩容 !  集群 !
以后告别  docker run !
docker-compose up  ! 启动一个项目  。单机  !
集群: swarm
docker serivce
容器=>服务! =>副本!
redis服务=> 10个副本! (同时开启10个redis容器)
体验：创建服务、动态扩展服务、动态更新服务

![image-20201119144621928](https://gitee.com/xudongyin/img/raw/master/img/20201119144623.png)

灰度发布：金丝雀发布！

![image-20201119153655371](https://gitee.com/xudongyin/img/raw/master/img/20201119153656.png)

~~~shell
docker run   容器启动，不具有扩缩容器
docker service  服务启动，具有扩缩容器，滚动更新！
~~~

![image-20201119155229044](https://gitee.com/xudongyin/img/raw/master/img/20201119155230.png)

动态扩缩容

![image-20201119155417319](https://gitee.com/xudongyin/img/raw/master/img/20201119155419.png)

服务,集群中任意的节点都可以访问。服务可以有多个副本动态扩缩容实现高可用!

![image-20201119194915819](https://gitee.com/xudongyin/img/raw/master/img/20201119194917.png)

worker 节点不能进行任何管理操作

~~~shell
docker service create -p 8888:80 --name my-nginx nginx  # 集群创建服务，并启动一个随机分布在任何节点的 nginx 服务
docker service ls   # 列出集群上所有服务
docker service ps my-nginx   # 通过自定义服务名查询服务进程
docker service inspect my-nginx  # 通过自定义服务名查看服务信息
docker service update --replicas 3  my-nginx  # 启动3个随机节点分布的 my-nginx 服务，集群里的任何一个节点的ip都可以访问这3个服务
# 注意 docker service update --replicas 10  my-nginx 即使数量超过了服务器数量还是可以启动运行，因为使用的是 docker 容器虚拟化技术，可以一个大的容器开启多个小的容器，可以重复使用这个命令来更改服务启动数 
docker service scale my-nginx=5  # 跟 update --replicas 作用一样
docker service rm my-nginx  # 集群移除服务
~~~

## 概念总结

swarm
集群的管理和编号。docker可以初始化一 个swarm集群，其他节点可以加入。(管理、 工作者)

Node
就是一个docker节点。多个节点就组成了一个网络集群。(管理、 工作者)

Service
任务,可以在管理节点或者工作节点来运行。核心。用户访问!

Task
容器内的命令,细节任务!

![image-20201120103528976](https://gitee.com/xudongyin/img/raw/master/img/20201120103533.png)

> Service

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201120103746)

![img](https://gitee.com/xudongyin/img/raw/master/img/20201120104020)

命令 -> 管理 ->  api -> 调度 -> 工作节点 ( 创建Task容器维护创建! )

> 服务副本与全局服务

![image-20201120104335623](https://gitee.com/xudongyin/img/raw/master/img/20201120104337.png)

**黄色的是全局服务，灰色的是副本服务只在工作节点跑**

调整 service 以什么方式运行

~~~shell
docker service create --mode replicated --name mytom tomcat  # 以副本服务启动
docker service create --mode global --name mytom tomcat # 以全局服务启动
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201120110007)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201120110018)

拓展 : 网络模式:  "PublishMode": "ingress"
Swarm:
Overlay : 几台不同的机器通过连接到一个 overlay 网络可以进行通信
ingress : 特殊的Overlay网络 ! 具有负载均衡的功能 !
虽然docker在4台机器上，实际网络是同一个 ingress 网络，是一个特殊的 overlay 网络

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201120110140.png)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201120110156.png)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20201120110209)

# Docker Stack

docker-compose 单机部署项目！

Docker Stack 部署，集群部署！

![image-20201120141649879](https://gitee.com/xudongyin/img/raw/master/img/20201120141652.png)

~~~shell
# 单机
docker-compose up -d wordpress.yml
# 集群
docker stack deploy wordpress.yml

# docker-compose yml 文件
version: "3"

services:
  nginx:
    image: nginx:alpine
    ports:
      - 80:80
    deploy:    # 集群下载
      mode: replicated
      replicas: 4   # 副本数量

  visualizer:
    image: dockersamples/visualizer
    ports:
      - "9001:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      replicas: 1
      placement:
        constraints: [node.role == manager]

  portainer:
    image: portainer/portainer
    ports:
      - "9000:9000"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      replicas: 1
      placement:
        constraints: [node.role == manager]
~~~

# Docker Secret

安全，配置密码，证书！

![image-20201120141756093](https://gitee.com/xudongyin/img/raw/master/img/20201120141757.png)

# Docker Config

配置

![image-20201120141910671](https://gitee.com/xudongyin/img/raw/master/img/20201120141911.png)