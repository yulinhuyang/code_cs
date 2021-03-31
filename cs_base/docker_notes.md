
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













