
docker 基本命令:

导入镜像:
cat ubuntu-base-16.04.6-base-amd64.tar.gz | docker import - ubuntu_arm64_1604_6

下载运行：

docker pull forumi0721ubuntuaarch64/ubuntu-aarch64-dev
docker run -it --privileged  -v /usr/bin/qemu-aarch64-static:/usr/bin/qemu-aarch64-static  ubuntu_arm64_1604_6 /bin/bash

运行某个容器： 

docker exec -it 2e57cec46995 /bin/bash

提交修改： 

docker commit + 容器名 + 仓库名


docker ps ： 查看容器

docker image ls ：查看当前的镜像


容器拷贝到宿主机：
docker cp mycontainer:/opt/testnew/file.txt /opt/test/

从宿主机拷贝文件到容器
docker cp /data1/3559yjn/docker_qemusudo/source/apt.conf  11c5617bc286:/etc/apt
