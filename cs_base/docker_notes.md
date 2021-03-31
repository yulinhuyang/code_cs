
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

**docker 引擎**

Docker引擎由以下主要的组件构成：Docker客户端（Docker Client）、Docker守护进程（Docker daemon）、containerd以及runc

containerd将Docker镜像转换为OCI bundle，并让runc基于此创建一个新的容器

shim的部分职责如下：

保持所有的STDIN和STDOUT流是开启状态，从而当daemon重启的时候，容器不会因为管道的关闭而终止。

将容器的退出状态反馈给daemon

在Linux系统中，前面谈到的组件由单独的二进制来实现，具体包括docker（docker daemon）、docker-containerd（containerd）、docker-containerd-shim（shim）和docker-runc(runc)

**docker 镜像**

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











