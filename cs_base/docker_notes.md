
### 1  docker 基本命令:

docker 官方库： https://github.com/docker-library/official-images/blob/master/library/ros

[终于有人把 Docker 讲清楚了，万字详解！](https://zhuanlan.zhihu.com/p/89587030)

[深入浅出docker读书笔记](https://www.jianshu.com/nb/41522986)


导入镜像:

cat ubuntu-base-16.04.6-base-amd64.tar.gz | docker import - ubuntu_arm64_1604_6

下载运行：

docker pull forumi0721ubuntuaarch64/ubuntu-aarch64-dev
docker run -it --privileged  -v /usr/bin/qemu-aarch64-static:/usr/bin/qemu-aarch64-static  ubuntu_arm64_1604_6 /bin/bash


**运行某个容器：**

docker container exec -it  + 容器名   + 命令

docker exec -it 2e57cec46995 /bin/bash

提交修改： 

docker commit + 容器名 + 仓库名


docker ps ： 查看容器

docker image ls ：查看当前的镜像


**容器拷贝到宿主机：**

docker cp mycontainer:/opt/testnew/file.txt /opt/test/

**从宿主机拷贝文件到容器:**

docker cp /data1/3559yjn/docker_qemusudo/source/apt.conf  11c5617bc286:/etc/apt

Docker images导出和导入: https://www.jianshu.com/p/8408e06b7273


**镜像导出和导入:**

docker save 9045（imageID） > tomcat8-apr.tar

docker load < tomcat8-apr.tar

**容器导出和导入：**

docker export b91d9ad83efa(容器名) > tomcat80824.tar

docker import tomcat80824.tar


**容器操作其他：**

docker container stop 停止容器

docker container  rm  杀死容器

docker container  ls -a 列出来所有容器，包括杀死的

**镜像操作其他：**



### 2 docker file 文件

[Dockerfile 文件详解](https://blog.csdn.net/xiaojin21cen/article/details/84568656)

docker image build  根据dockerfile 的命令构建镜像

注意：Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大。例如：

    FROM centos
    RUN yum install wget
    RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz"
    RUN tar -xvf redis.tar.gz
    以上执行会创建 3 层镜像。可简化为以下格式：
    FROM centos
    RUN yum install wget \
        && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
        && tar -xvf redis.tar.gz

如上，以 && 符号连接命令，这样执行后，只会创建 1 层镜像。


### 3 深入浅出docker notes

**3.5 docker 引擎**

Docker引擎由以下主要的组件构成：Docker客户端（Docker Client）、Docker守护进程（Docker daemon）、containerd以及runc

containerd将Docker镜像转换为OCI bundle，并让runc基于此创建一个新的容器

shim的部分职责如下：

保持所有的STDIN和STDOUT流是开启状态，从而当daemon重启的时候，容器不会因为管道的关闭而终止。

将容器的退出状态反馈给daemon

在Linux系统中，前面谈到的组件由单独的二进制来实现，具体包括docker（docker daemon）、docker-containerd（containerd）、docker-containerd-shim（shim）和docker-runc(runc)

**3.6 docker 镜像**

在该前提下，镜像可以理解为一种构建时结构，而容器可以理解为一种运行时结构。

docker image rm    删除镜像

Docker service主要用于使用Docker swarm配置主节点，以便docker容器可以在分布式环境中运行，并且可以轻松管理

Docker service： 将一些较大应用程序环境中微服务的镜像。服务可能包括HTTP服务器，数据库或您希望在分布式环境中运行的任何其他类型的可执行程序。

镜像：

默认情况下，我们拉取的是带有标签 "latest"的镜像，如果要拉取不同的镜像，需要指定特有的标签

docker image pull ubuntu:18.04

Docker提供了--filter参数来过滤docker image ls命令返回的镜像列表内容。

docker image pull -a ubuntu   拉取所有标签

docker image ls --filter dangling=true

过滤器:

dangling：可以指定true或者false，仅返回悬虚镜像或者非悬虚镜像。

before： 需要镜像名称或者ID作为参数，返回之前被创建的全部镜像

label：根据标注（label）的名称或者值，对镜像进行过滤。docker image ls 命令中不显示标注内容。

docker search 命令允许通过CLI的方式搜索Docker Hub

docker search ubuntu

docker search ubuntu --filter is-official=true

镜像分层：

所有的Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之上，创建新的镜像层

docker image inspect 命令来查看镜像的分层

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多个镜像层对外展示为一个统一的文件。Linux上可用的存储引擎有AUFS、Overlay2、Device Maooer等等。

镜像摘要：--digests

镜像散列：

每个镜像层同时包含一个分发散列值，这是一个压缩版镜像的散列值。校验传输过程中是否被篡改。

多层架构的镜像：

一个镜像标签下可以支持多个平台和架构，manifest列表和manifest。

选择当前平台和架构所需的正确镜像版本是由docker完成的

删除镜像：

docker image rm 

docker image rm 9b915a241e29(ID)

如果被删除的镜像上存在运行状态的容器，那么该删除操作不会被允许。再次执行删除镜像命令之前，需要停止并删除该镜像相关的全部容器

**3.7 docker 容器**

docker container run <image> <app>中，指定启动所需的镜像以及要运行的应用。docker container run -it ubuntu /bin/bash 则会启动某个ubuntu Linux容器，并允许Bash Shell作为其应用。

docker container stop 命令可以手动停止容器运行 

docker container start再次启动该容器

Hypervisor是硬件虚拟化（Hardware Virtualization）——Hypervisor将硬件物理资源划分为虚拟资源；另外，容器时操作系统虚拟机（OS Virtualization）——容器将系统资源划分为虚拟资源

容器共享一个操作系统/内核。这意味着只有一个操作系统消耗CPU、内存等资源，只有一个系统需要授权，只有一个系统需要升级和打补丁。

容器的启动时间远远小于虚拟机

添加用户到docker unix： usermod -aG docker <user>

检查docker daemon状态： service docker status

容器进程：一旦我们退出了bash shell,该容器也会退出（终止）。原因是容器如果不允许任何进程则无法存在，退出了bash shell也就是等于杀死了容器。我们使用 Crtl-PQ组合键来退出容器但不会终止容器运行，然后我们使用docker container ls命令来观察当前系统中正在运行的容器列表。

容器的生命周期：容器时可以持久化数据的。但是说到持久化，卷（volume）才是容器中存储持久化数据的首选方式

docker container stop 命令向容器内的PID 1进程发送了SIGTERM这样的信号

docker container rm -f 就是直接发出SIGKILL的信号，直接杀掉容器

重启策略进行容器的自我修复:

重启策略包括 always、unless-stoped 和 on-failed 三种。

docker container run -it  --restart always ubuntu /bin/bash

--restart always 策略有个比较有趣的特征，就是当daemon重启的时候，停止的容器也会被重启。

always和unless-stopped 的最大区别，就是那些指定了--restart unless-stopped并处于Stopped(Exited)状态的容器，不会在Docker daemon重启的时候被重启。

on-failure 策略会在退出容器并且返回值不是0的时候，重启容器。

docker container rm $(docker container ls -aq) -f   删除所有容器

**3.8 应用的容器化**

将应用整合到容器中并且运行起来的这个过程，称为“容器化”（Containerizing），有时也叫做“Docker化”（Dockerizing）

应用容器化过程主要分为以下几个步骤：

编写应用代码

创建一个Dockerfile，其中包括当前应用的描述、依赖以及该如何运行这个应用。

对该Dockerfile执行docker image build 命令。

等待Docker 将应用程序构建到Docker镜像中。
 
获取应用代码——>分析Dockerfile——>构建应用镜像——>运行该应用——>测试应用——>容器应用化细节——>生产环境中多阶段构建——>最佳实践

包含应用文件的目录通常被称为构建上下文（Build Context）。通常将Dockerfile放到构建上下文的根目录下

文件开头字母是大写的D

通过ENTRYPOINT 指令来指定当前应用程序的入口程序

docker image tag来打标签


生产环境中的多阶段构建

每个RUN指令都会新增一个镜像层。因此，通过使用 && 连接多个命令以及使用反斜杠换行的方法，将多个命令包含在一个RUN指令中，这是一种值得提倡的做

建造者模式

Docker的构建过程利用了缓存机制

当要构建镜像的层次很多的时候，我们可以考虑把这些镜像层给合并,在 docker image build 的时候加上参数 --squash

相关命令：

docker image build 命令会读取 Dockerfile，并将应用程序容器化。使用 -t 参数给镜像打标签，使用 -f 参数指定Dockerfile的路径和名称。构建上下文是指文件存放的位置，可能是本地Docker主机上的一个目录或者远程的Git库。

Dockerfile 中的FROM 指令用于指定要构建的镜像的基础镜像。

Dockerfile 中的RUN指令用于在镜像中执行命令，这会构建新的镜像层。

Dockerfile 中的COPY指令用于将文件作为一个新的层添加到镜像中。

Dockerfile 中的EXPOSE指令用于记录应用所使用的网络端口。

Dockerfile 中的ENTRYPOINT指令用于指定镜像以容器方式启动后默认运行的程序。

其他的Dockerfile指令还有LABEL、ENV、ONBUILD、HEALTHCHECK、CMD等

**3.9 Docker Compose**

Docker Compose: 一个声明式的配置文件描述整个应用，从而使用一条命令完成部署 

Docker Compose 是一个需要在Docker主机上进行安装的外部Python工具。使用它时，首先编写定义多容器（多服务）应用的YAML文件，然后将其交由给docker-compose命令处理

安装docker compose 

下载二进制包，目前最新版本时1.24.1

        sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

修改文件权限

sudo chmod +x /usr/local/bin/docker-compose

查看docker-compose查看版本信息

docker-compose --version

Dockers Compose默认使用的文件名为 docker-compose.yml

包含了4个一级key：version、services、networks、volumes。

volumes用于指引Docker来创建新的卷。

docker-compose up 命令。它会构建所需的镜像，创建网络和卷，并启动容器。默认情况下，docker-compose up 会查找名为docker-compose.yml或者docker-compose.yaml的Compose文件。-d  后台启动

docker-compose down 来关闭应用

docker-compose ps 观察应用的状态

docker-compose top 命令可以列出各个服务内运行的进程

docker-compose stop 可以停止应用，但是不会删除资源

docker-compose rm命令去删除资源

docker volume ls 

inspect找到我们这个卷所在的位置 

 [pangcm@docker01 counter-app]$ docker volume inspect counter-app_counter-vol |grep Mountpoint
        "Mountpoint": "/var/lib/docker/volumes/counter-app_counter-vol/_data",









