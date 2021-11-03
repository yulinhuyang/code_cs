**相关资料**

[只要一小时，零基础入门Docker](https://zhuanlan.zhihu.com/p/23599229)

[Docker 入门教程 阮一峰](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

https://github.com/yeasy/docker_practice

https://github.com/moby/moby

https://www.runoob.com/docker/docker-tutorial.html

http://c.biancheng.net/docker/

第一本docker书

深入浅出docker

[Docker buildx 多架构镜像](https://zhuanlan.zhihu.com/p/299116754)

[二进制安装docker](https://my.oschina.net/daoflow/blog/4700063)


查看所在contaioner的id： cat /proc/self/cgroup | head -1

docker tag : 标记本地镜像，将其归入某一仓库。   docker tag ubuntu:15.10 runoob/ubuntu:v3


## 1  docker 基本命令:

docker 官方库： https://github.com/docker-library/official-images/blob/master/library/ros

[终于有人把 Docker 讲清楚了，万字详解！](https://zhuanlan.zhihu.com/p/89587030)

[深入浅出docker读书笔记](https://www.jianshu.com/nb/41522986)


导入镜像:

cat ubuntu-base-16.04.6-base-amd64.tar.gz | docker import - ubuntu_arm64_1604_6

下载运行：

docker pull forumi0721ubuntuaarch64/ubuntu-aarch64-dev

docker run -it --privileged  -v /usr/bin/qemu-aarch64-static:/usr/bin/qemu-aarch64-static  ubuntu_arm64_1604_6 /bin/bash

--name 中的参数设定运行容器实例的名字，--privileged 设定特权模式（docker 容器实际上是做了从 linux 内核到用户进程的映射，容器中的 root 权限实际上并不是外面物理机的 root 权限，而只是一个普通权限）， -volume  参数选择挂载硬盘， 可以用多个-v 挂载多个路径

**运行某个容器：**

docker container exec -it  + 容器名   + 命令

docker exec -it 2e57cec46995 /bin/bash


**attach和detach对比**

docker run -it IMAGES_NAME会创建前台进程，但是会在输入exit后终止进程。

docker attach DOCKER_ID 会通过连接stdin，连接到容器内输入输出流，会在输入exit后终止进程.

docker exec -it DOCKER_ID /bin/bash 会连接到容器，可以像SSH一样进入容器内部，进行操作，可以通过exit退出容器，不影响容器运行。

以上几种方式均可通过输入Ctrl+P+Q把前台容器放入后台运行，不终止进程

启动容器的时候一定要加上--detach或者-d来保持容器在后台持续运行。

**提交修改**

docker commit + 容器名 + 仓库名：tag

docker ps ： 查看容器

docker image ls ：查看当前的镜像


**容器拷贝到宿主机：**

docker cp mycontainer:/opt/testnew/file.txt /opt/test/

**从宿主机拷贝文件到容器:**

docker cp /data1/3559yjn/docker_qemusudo/source/apt.conf  11c5617bc286:/etc/apt

Docker images导出和导入: https://www.jianshu.com/p/8408e06b7273


**镜像导出和导入:**

docker save 9045（imageID） > tomcat8-apr.tar

或者： docker save -o ubuntu_arm_ros.tar   arm64v8/ros:latest 

docker load < tomcat8-apr.tar

**容器导出和导入：**

docker export b91d9ad83efa(容器名) > tomcat80824.tar

docker import tomcat80824.tar


**容器操作其他：**

docker container stop 停止容器

docker container  rm  杀死容器

docker container  ls -a 列出来所有容器，包括杀死的

**镜像操作其他：**


**普通用户加入docker组**

创建docker用户组：   sudo groupadd docker

应用用户加入docker用户组： sudo usermod -aG docker ${USER}

重启docker服务： sudo systemctl restart docker

切换或者退出当前账户再从新登入：

su root             切换到root用户
su ${USER}          再切换到原来的应用用户以上配置才生效





## 2 docker file 文件

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


## 3 深入浅出docker notes

### 3.5 docker 引擎 

Docker引擎由以下主要的组件构成：Docker客户端（Docker Client）、Docker守护进程（Docker daemon）、containerd以及runc

containerd将Docker镜像转换为OCI bundle，并让runc基于此创建一个新的容器

shim的部分职责如下：

保持所有的STDIN和STDOUT流是开启状态，从而当daemon重启的时候，容器不会因为管道的关闭而终止。

将容器的退出状态反馈给daemon

在Linux系统中，前面谈到的组件由单独的二进制来实现，具体包括docker（docker daemon）、docker-containerd（containerd）、docker-containerd-shim（shim）和docker-runc(runc)

### 3.6 docker 镜像 

在该前提下，镜像可以理解为一种构建时结构，而容器可以理解为一种运行时结构。

docker image rm    删除镜像

Docker service主要用于使用Docker swarm配置主节点，以便docker容器可以在分布式环境中运行，并且可以轻松管理

Docker service： 将一些较大应用程序环境中微服务的镜像。服务可能包括HTTP服务器，数据库或您希望在分布式环境中运行的任何其他类型的可执行程序。

**镜像**

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

### 3.7 docker 容器 

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

**重启策略进行容器的自我修复**

重启策略包括 always、unless-stoped 和 on-failed 三种。

docker container run -it  --restart always ubuntu /bin/bash

--restart always 策略有个比较有趣的特征，就是当daemon重启的时候，停止的容器也会被重启。

always和unless-stopped 的最大区别，就是那些指定了--restart unless-stopped并处于Stopped(Exited)状态的容器，不会在Docker daemon重启的时候被重启。

on-failure 策略会在退出容器并且返回值不是0的时候，重启容器。

docker container rm $(docker container ls -aq) -f   删除所有容器

### 3.8 应用的容器化 

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


**生产环境中的多阶段构建**

每个RUN指令都会新增一个镜像层。因此，通过使用 && 连接多个命令以及使用反斜杠换行的方法，将多个命令包含在一个RUN指令中，这是一种值得提倡的做

建造者模式

Docker的构建过程利用了缓存机制

当要构建镜像的层次很多的时候，我们可以考虑把这些镜像层给合并,在 docker image build 的时候加上参数 --squash

**相关命令**

docker image build 命令会读取 Dockerfile，并将应用程序容器化。使用 -t 参数给镜像打标签，使用 -f 参数指定Dockerfile的路径和名称。构建上下文是指文件存放的位置，可能是本地Docker主机上的一个目录或者远程的Git库。

Dockerfile 中的FROM 指令用于指定要构建的镜像的基础镜像。

Dockerfile 中的RUN指令用于在镜像中执行命令，这会构建新的镜像层。

Dockerfile 中的COPY指令用于将文件作为一个新的层添加到镜像中。

Dockerfile 中的EXPOSE指令用于记录应用所使用的网络端口。

Dockerfile 中的ENTRYPOINT指令用于指定镜像以容器方式启动后默认运行的程序。

其他的Dockerfile指令还有LABEL、ENV、ONBUILD、HEALTHCHECK、CMD等

### 3.9 Docker Compose 

Docker Compose: 一个声明式的配置文件描述整个应用，从而使用一条命令完成部署 

Docker Compose 是一个需要在Docker主机上进行安装的外部Python工具。使用它时，首先编写定义多容器（多服务）应用的YAML文件，然后将其交由给docker-compose命令处理

**安装docker compose** 

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


### 3.10 Docker Swarm

Docker Swarm包含两方面：一个企业级的Docker安全集群，以及一个微服务应用编排引擎。

Docker Swarm 是使Docker规模化的关键方案。Docker Swarm 的核心包含一个安全集群组件和一个编排组件。

安全集群管理组件是一个企业级的安全套件，提供了丰富的安全机制以及HA特性，这些都是自动配置好的，并且非常容易调整。

编排组件允许用户以一种简单的声明式的方式来部署和管理微服务应用。它不仅支持原生的Docker Swarm应用，还支持K8s应用。

Swarm将一个或多个Docker节点组织起来，使得用户能够以集群方式管理它们。Swarm默认内置有加密的分布式集群存储、加密网络、公用TLS、安全集群介入令牌以及一套简化数字证书管理的PKI。用户可以自如地添加或删除节点

一个Swarm由一个或多个Docker节点组成。节点会被配置为管理节点（Manager）或者工作节点（Worker）。管理节点负责集群控制面（Control Plane），进行诸如监控集群状态、分发任务至工作节点等操作。工作节点接收来自管理节点的任务并执行

Swarm中的最小调度单元是服务

当容器被封装在一个服务中时，我们称之为一个任务或者一个副本，服务中增加了诸如扩缩容、滚动升级以及简单回滚等特征。

**搭建安全的Swarm集群**

不包含在任何Swarm中的Docker节点，称为运行于单引擎（Single-Engine）模式。一旦被加入Swarm集群，则切换为Swarm模式。

docker swarm init ：在单引擎模式下的Docker主机上运行docker swarm init 会将其切换到Swarm模式，并创建一个新的Swarm，将自身设置为Swarm的第一个管理节点，然后其他的节点就可以加入进来了，新加入的节点也会切换为swarm模式。

        [pangcm@docker01 ~]$ docker swarm init --advertise-addr 192.168.113.70:2377 --listen-addr 192.168.113.70:2377 
        Swarm initialized: current node (liokj5021pc9ls40fj3tcebxq) is now a manager.

        To add a worker to this swarm, run the following command:

            docker swarm join --token SWMTKN-1-59xcha9v3tw180c64c0zg0ic2418qw69ns6ics4t6cj2t0wl6x-b5mlt9leakzj4gpo9qa9souc1 192.168.113.70:2377

        To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

docker node ls   列出Swarm中的节点

docker swarm join  节点加入swam

##这个命令在我们初始化swarm的时候系统生成的，同理添加管理节点的命令也在初始化的时候给出了
docker swarm join --token SWMTKN-1-59xcha9v3tw180c64c0zg0ic2418qw69ns6 192.168.113.70:2377

每个节点的Docker引擎都被切换到Swarm模式下，并且自动启动了TLS安全。如果你有多个管理节点，那么有的节点会是 Reachable状态，有一个是 Leader状态。Leader那个就是主节点了

**Swarm管理器的高可用（HA）**

Swarm的管理节点内置有对HA的支持。这意味着，即使有一个或者多个节点发生了故障，剩余管理节点也会继续保证Swarm的运转

可能有多个管理节点，但是只有一个处于活动状态。通常处于活动状态的节点被称为主节点，而主节点也是唯一一个会对Swarm发送控制命令的节点。

只有主节点才会变更配置，或发送任务到工作节点。

Swarm使用Raft算法来实现支持管理节点的HA，Raft协议在选举Leader的时候遵循少数服从多数的原则。所以使用奇数个节点的好处就是避免脑裂现象的出现。

**Swarm安全**

Swarm集群内置了众多的安全机制，并提供了开箱即用的合理默认配置——如CA设置、接入Token、公用TLS、加密集群存储、加密网络、加密节点ID。

Docker提供了自动锁机制来锁定Swarm，这会强制要求重启的管理节点在提供一个集群解锁码之后才有权重新接入集群

在创建Swarm集群的时候可以直接加上 --autolock 参数启用锁。前面并没有这么操作，那就需要使用 docker swarm update 命令来启动锁

管理节点上：   

        docker swarm update --autolock=true

这时候重启其他的管理节点，管理节点不会自动重新接入集群。要接入集群那就要先解锁，使用 docker swarm unlock 命令进行解锁

**Swarm服务**

服务是自Docker 1.12后新引进的概念，并且仅适用于Swarm模式

使用服务仍能够配置大多数熟悉的容器，比如容器名、端口映射、接入网络和镜像。此外还增加了额外的特性，比如可以声明应用服务的期望状态，将其告知Docker后，Docker会负责进行服务的部署和管理

docker service create --name web-fe -p 8080:8080 --replicas 5 nigelpoulton/pluralsight-docker-ci

使用 --replicas 参数告诉Docker应该总是有5个此服务的副本

所有的服务都会被Swarm持续监控——Swarm会在后台进行轮询检查，来次序比较服务的实际状态和期望状态是否一致。如果不一致，Swarm会使其一致。

docker service ls 命令可以查看Swarm中的所有运行中的服务。

		docker service ls 
	ID                  NAME                MODE                REPLICAS            IMAGE                                       PORTS
	kiyro8y0wbj7        web-fe              replicated          5/5                 nigelpoulton/pluralsight-docker-ci:latest   *:8080->8080/tcp

docker service ps 可以查看服务副本列表及各副本的状态

docker service inspect 命令查看详细服务信息

		docker service inspect web-fe  --pretty， 使用 --pretty 只显示我们感兴趣的信息
		
副本VS全局：服务的默认复制方式是副本模式，这种模式会部署期望数量的服务副本，并尽可能均匀地将各个副本分布在整个集群中。另外一种模式是全局模式，这种模式下，每个节点仅运行一个副本。

服务的扩缩容：能够方便地进行扩缩容。在业务发展的时候，我们会增加副本的数量。

	docker service scale web-fe=10

删除服务： docker service rm web-fe

滚动升级： docker service update 

	docker service update --image tomcat --update-parallelism 2 --update-delay 20s  web-fe

	--update-parallelism 和 --update-delay 声明名词使用新镜像更新2个副本，期间有20s的延迟。升级的过程可以通过 docker service ps web-fe 去查看

docker service logs 可以查看Swarm的服务日志。然而并非所有的日志驱动都支持该命令。

Docker节点默认的配置是，服务使用json-file日志驱动，其他的驱动还有 journald(Linux特有)、syslog、splunk和gelf。


### 3.11 Docker网络

Docker网络架构源自一种叫做容器的网络模型（CNM）的方案，该方案是开源的并且支持插式连接。Libnetwork式Docker对CNM的一种实现，提供了Docker核心网络架构的全部功能。

Docker封装了一系列本地驱动、覆盖了大部分常见的网络需求。其中包括单机桥接网络、多级覆盖网络，并且支持接入现有的VLAN。

不同的驱动可以通过插拔的方式接入Libnetwork来提供定制化的网络拓扑。

Libnetwork提供了本地服务发现和基础的容器负载均衡的解决方案。

**基础理论**

Docker网络架构由3个主要部分构成：CNM、Libnetwork和驱动

CNM是涉及标准，规定了Docker网络架构的基础组成要素；Libnetwork是CNM的具体实现，并且被Docker采用。Libnetwork通过Go语言编写。

CNM定义了3个基本要素：沙盒（Sandbox）、终端（Endpoint）、和网络（Network）。

沙盒是一个独立的网络栈。其中包括以太网接口、端口、路由表以及DNS配置；终端就是虚拟网络接口，主要职责是负责创建连接。网络就是 802.1d网桥（交换机）的软件实现。

沙盒放置在容器内部，为容器提供给网络连接。

Libnetwork：CNM是设计规范文档，Libnetwork是标准的实现。实现CNM中定义的三个组件，实现了本地服务发现、基于 Ingress的容器负载均衡以及网络控制层。

Docker封装了若干内置驱动，通常被叫做原生驱动或者本地驱动。在Linux中包括Bridge、overlay以及Macvlan。

**单机桥接网络**

单机意味着该网络只能在单个Docker主机上运行，并且只能与所在Docker主机上的容器进行连接。桥接意味着这是 802.1d桥接的一种实现。

Linux Docker创建单机桥接网络采用内置的桥接驱动，而Windows Docker创建时使用内置的NAT驱动。

docker network ls 查看Docker主机的网络情况

		NETWORK ID          NAME                DRIVER              SCOPE
		7cb75625ceeb        bridge              bridge              local

docker network inspect 查看网络的更多内容

Docker网络由Bridge驱动创建，底层是Linux Bridge技术，Bridge是高性能并且非常稳定的。 ip link show 查看网络

		[pangcm@docker01 ~]$ ip link show docker0
		3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default 
			link/ether 02:42:01:12:07:d9 brd ff:ff:ff:ff:ff:ff

在Linux Docker主机之上，默认的 bridge 网络被映射到内核中为 docker0 的Linux网桥，dokcer network inspect  命令去观察
			
docker network create 命令创建一个单机桥接的网络。

		docker network create -d bridge localnet

docker network ls   查看网络

新创建的网络一样在我们Docker 主机上新建一个Linux网桥
	
brctl show 命令即可查看
	
	[pangcm@docker01 ~]$ brctl show 
	bridge name bridge id       STP enabled interfaces
	br-e6bca1080f16     8000.0242126f947e   no      
	docker0     8000.0242011207d9   no  

新建一个容器，并加入到新建的桥接网络中去
	
docker container run -d --name test2 --network localnet alpine sleep 1d

docker network inspect 命令去确认

		docker network inspect localnet format

自定义的网络中除了可以使用IP来进行通信之外，还可以使用容器名称去进行通信,新容器都会注册到指定的Docker DNS 服务中

同一个Docker主机同一个桥接网络之间的容器可以相互通信的。那么不同Docker主机上的桥接网络容器可以通过端口映射进行相互通信。



接入现有网络：Docker内置的Macvlan驱动（Windows上是Transparent）就是为此场景而生。通过为容器提供MAC和IP地址，让容器在物理网络上成为“一等公民”。

服务发现：Docker内置的DNS服务器，和真实的DNS解析过程类似，本地解析器找不到域名的话会向Docker的DNS服务发起一个递归查询。

参数： --dns参数允许读者指定自定义的DNS服务列表，--dns-search 参数指定自定义查询时所使用的域名， 

可以通过容器内部 /et/resolve.conf 文件内部增加条目来实现

		docker container run -it --name test5 --dns=8.8.8.8 \
		--dns-search=dockercerts.com alpine sh

会启动一个新的容器，并添加声名狼藉的 8.8.8.8 Google DNS服务器，同时指定 dockercerts.com 作为域名添加到非完整查询


**Ingress网格**

Swarm支持两种服务发布模式，两种模式均保证服务从集群外可访问。分别是：Ingress模式（默认）和Host模式

通过Ingress模式发布的服务，可以保证从Swarm集群内任一节点（即使没有运行服务的副本）都能访问该服务；以Host模式发布的服务只能通过运行服务副本的节点来访问。

Ingress模式是默认方式，如果要使用Host模式发布服务，读者需要使用 --publish 参数的完整格式，并添加 mode=host

		docker service create -d --name svc1 --publish published=5000,target=80,mode=host nginx

Ingress模式采用名为Service Mesh 或者Swarm Mode Service Mesh 的四层路由网络来实现	

一是使用Ingress模式的服务，访问任意一个节点（即使没有运行对应的容器），Docker都能把流量转到实际运行容器的节点上，；二是并且如果存在多个运行中的副本，流量会均衡到每个副本上

docker network inspect 提供 Docker网络的详细配置信息。

docker network prune 删除Docker主机上全部未使用的网络

docker network rm 删除Docker主机上指定的网络


### 3.12 Docker覆盖网络

覆盖网络：允许创建扁平的、安全的二层网络来连接多个主机，容器可以连接到覆盖网络并且直接相互通信。

Swarm模式下构建并测试Docker覆盖网络：构建Swarm——>创建新的覆盖网络——>将服务连接到覆盖网络——>测试覆盖网络

Docker使用VXLAN隧道技术创建了虚拟二层覆盖网络。在VXLAN的设计中，允许用户基于已经存在的三层网络结构创建虚拟的二层网络。

VXLAN基于现有的三层IP网络创建了隧道

VXLAN隧道两端都是VXLAN隧道终端（VXLAN Tunnel Endpoint,VTEP）。VTEP完成了封装和解压的步骤。

在Sandbox内部创建了一个名为Br0的虚拟交换机。同时Sandbox内部还创建了一个VETP，其中一端接入到名为Br0的虚拟交换机当中，另一端接入主机网络栈（VETP）

不同主机上的两个VTEP通过VXLAN隧道创建了一个新的覆盖网络

docker network create 是创建新网络所使用的命令，-d 参数允许用户指定所用驱动，常见的驱动是 Overlay。

docker network ls 用于列出Docker主机上全部可见的容器网络。Swarm模式下的Docker主机只能看到已经接入运行中的容器的网络。这种方式保证了Gossip开销最小化。

docker network inspect 用于查看特定容器网络的详情。其中包括范围、驱动、IPV6、子网配置、VXLAN网络ID以及加密状态。

docker network rm 删除指定网络。

### 3.14 使用Docker Stack 部署应用

大规模场景下的多服务部署和管理，Docker Stack 通过提供期望状态、滚动升级、简单易用、扩缩容、健康检查等特性简化了应用的管理。这些功能都封装在一个完美的声明式模型。

Stack能够在单个声明文件中定义复杂的多服务应用。Stack还提供了简单的方式来部署应用并管理完整的生命周期：初始化部署—>健康检查—>扩容—>更新—>回滚


步骤： 在Compose文件中定义应用，然后通过 docker stack deploy 命令完成部署和管理。Compose文件中包含了构成应用所需的完整服务栈，此外还包括了卷、网络、安全以及应用所需的其他架构。然后基于该文件使用docker stack deploy 命令来部署应用。

Stack是基于Docker Swarm之上来完成应用的部署。因此诸如安全等高级特性，其实都是来自Swarm。简而言之，Docker适用于开发和测试。Docker Stack则适用于大规模场景和生产环境。


**使用Docker Stack 部署应用**

Stack位于Docker应用层级的最顶端。Stack基于服务进行构建，而服务又基于容器。

docker-stack.yml：version、services、networks、secrets。

secrets定义的是应用用到的密钥。

Stack文件就是Docker Compose文件，要求就是version:一项需要是3.0或者更高的值，


网络：默认情况下，覆盖网络的所有控制层都是加密的。默认情况下网络都是使用overlay驱动，payment网络比较特殊，需要对数据层加密。如果需要加密数据层，有两种选择。

在docker network create命令中指定 -o encrypted 参数。在stack文件中的 driver_opts 之下指定 encrypted:'yes'

密钥： 4个密钥都被定义为external。这意味着在Stack部署之前，这些密钥必须存在。
	
	  secrets:
	  postgres_password:
		external: true
	  staging_token:
		external: true
	  revprox_key:
		external: true
	  revprox_cert:
		external: true

服务：部署中主要的操作都在服务这个环节。每个服务都是一个JSON集合。

	reverse_proxy 服务

		reverse_proxy:
		image: dockersamples/atseasampleshopapp_reverse_proxy
		ports:
		  - "80:80"
		  - "443:443"
		secrets:
		  - source: revprox_cert
			target: revprox_cert
		  - source: revprox_key
			target: revprox_key
		networks:
		  - front-tier
 
	ports关键字，所有端口映射都采用Ingress模式，Host模式需要使用完整的格式去配置。
	
	networks关键字确保服务所有副本都会连接到front-tier网络
	
	文件的名称就是stack文件中定义的target属性的值，其在Linux下的路径为 /run/secrets，Linux将 /run/secrets作为内存文件系统挂载。
	
	database服务
	
		database:
		image: dockersamples/atsea_db
		environment:
		  POSTGRES_USER: gordonuser
		  POSTGRES_DB_PASSWORD_FILE: /run/secrets/postgres_password
		  POSTGRES_DB: atsea
		networks:
		  - back-tier
		secrets:
		  - postgres_password
		deploy:
		  placement:
			constraints:
			  - 'node.role == worker'
	
	环境变量和部署约束
	
	environment关键字允许在服务副本中注入环境变量
	
	deploy关键字下定义了部署约束，部署约束是一种拓扑感知定时任务。
	
	Swarm目前允许通过如下几种方式进行调度： 节点ID、角色、引擎标签、自定义标签
	
	appserver 服务
	
		appserver:
		image: dockersamples/atsea_app
		networks:
		  - front-tier
		  - back-tier
		  - payment
		deploy:
		  replicas: 2
		  update_config:
			parallelism: 2
			failure_action: rollback
		  placement:
			constraints:
			  - 'node.role == worker'
		  restart_policy:
			condition: on-failure
			delay: 5s
			max_attempts: 3
			window: 120s
		secrets:
		  - postgres_password
		  
	appserver 服务使用了一个镜像，连接到3个网络，并且挂载了一个密钥。
	
	replicas: 2 设置了期望服务的副本数量为2。update_config 定义了服务在滚动升级的时候应该如何操作。restart_policy 定义了Swarm针对容器异常退出的重启策略。
	
	visualizer 服务
	
		visualizer:
		image: dockersamples/visualizer:stable
		ports:
		  - "8001:8080"
		stop_grace_period: 1m30s
		volumes:
		  - "/var/run/docker.sock:/var/run/docker.sock"
		deploy:
		  update_config:
			failure_action: rollback
		  placement:
			constraints:
			  - 'node.role == manager'
		
	volumes关键字用于挂载提前创建的卷或者主机目录到某个服务副本中。在这里挂载Docker主机的 /var/run/docker.sock 目录到每个副本的 /var/run/docker.sock 路径。这意味着在服务副本中任何对 /var/run/docker.sock 的读写操作都会指向 Docker 主机的对应目录。
	
	docker.sock文件是Docker提供的套接字，Docker daemon通过该套接字对其他进程暴露其API终端
	
	payment_gateway 服务：
	
**部署应用**	 
	
前置处理:

	Swarm模式：应用将采用Docker Stack 部署，而Stack依赖Swarm模式。
	
	标签： 某个Swarm worker节点需要自定义标签。
	
	密钥： 应用所需的密钥需要在部署前创建完成。	
	
	添加节点标签： docker node update --label-add pcidss=yes docker02
	
	创建密钥：docker secret create revprox_cert domain.crt
    
	docker secret ls 
	
	docker stack ls 和docker stack ps 去查看 stack的更多信息
	 
部署应用： 通过声明式方式修改，即将Stack文件作为配置的唯一声明。再重新部署  docker stack deploy -c docker-stack.yml seastack
	
	
docker stack deploy 用于根据 Stack 文件部署和更新Stack服务的命令。

docker stack ls 会列出 Swarm 集群中全部的Stack，包括每个Stack拥有多少服务。

docker stack ps 列出某个已经部署的Stack相关详情。该命令支持Stack名称作为其主要参数，列举了服务副本在节点的分布情况，以及期望状态和当前状态。

docker stack rm 命令用于从Swarm集群中移除Stack。移除操作执行前并不会进行二次确认。
 
		
### 3.15 Docker安全

安全的本质就是分层！通俗地讲，拥有更多的安全层，就能拥有更多的安全性

Linux通用的安全技术:命名空间、控制组、系统权限、强制访问控制、安全计算。

Docker平台技术：
	
	Docker Swarm 模式：默认是开启安全功能的。
	
	Docker内容信任：允许用户对镜像签名，并且对拉取镜像的完整度和发布者进行验证。
	
	Docker密钥：Docker密钥存储在加密集群存储中，在容器传输过程中实时解密。

**Linux安全技术**

Namespace：内核命名空间将操作系统进行拆分。

	Docker容器是由各种命名空间组合而成的，Docker容器本质就是命名空间的组织集合。
	
	Linux Docker 现在利用了下列内核命名空间：进程ID、网络、文件系统/挂载、进程内通信、用户、UTS等。

Control Group:

	命名空间用于隔离，那么控制组就是用于限额。CGroup允许用户设置一些限制来保证不会存在单一容器占用全部的公共资源。
	
	在Docker的世界中，容器之间是相互隔离的，但却共享OS资源，比如CPU、RAM以及硬盘IO。CGroup允许用户设置限制，这样单个容器就不能占用主机全部的资源。
	
Capability: Docker采用Capability机制来实现用户以root身份运行容器的同时，还能移除非必须的root能力。
	
MAC: Docker采用主流Linux MAC技术，例如AppArmor以及SELinux。对新容器增加了默认的AppArmor配置文件.

Seccomp:使用过滤模式下的Seccomp来限制容器对宿主机内核发起的系统调用。每个新容器都会设置默认的Seccomp配置，文件中设置了合理的默认值。

**Docker平台安全技术**

Swarm模式：支持用户集群化管理多个Docker主机。

安全扫描：Docker安全扫描对Docker镜像进行二进制代码级别的扫描，对其中的软件根据已知缺陷数据库（CVE数据库）进行检查。

Docker内容信任：使得用户很容易确认所下载镜像的完整性以及其发布者。

Docker 密钥：docker secret 来管理密钥。	


###3.13  docker 其他命令

精简docker镜像：

删掉container 里面的core.* 相关的无效文件，然后将container export处理，再import进去，再save成tar包，就可以了
ack --python task_0 --> 搜索指定文件的指定字符

GPU docker 启动： docker run -itd --gpus all --net=host --shm-size=64g -v adas_proj/:/adas_proj/ --name adas_proj_wty 6d3d2b2cd6a0 /bin/bash 


	
	
