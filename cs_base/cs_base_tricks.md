##  tricks

**linux使用：**

[Ubuntu下挂载远程主机的目录](https://www.huaweicloud.com/articles/9313c263375fde826d7d5da603c7e682.html)

[Linux统计文件夹下的文件数目](http://noahsnail.com/2017/02/07/2017-02-07-Linux%E7%BB%9F%E8%AE%A1%E6%96%87%E4%BB%B6%E5%A4%B9%E4%B8%8B%E7%9A%84%E6%96%87%E4%BB%B6%E6%95%B0%E7%9B%AE/)

[一文助你打通 tmux](https://zhuanlan.zhihu.com/p/102546608)

[SED 简明教程](https://coolshell.cn/articles/9104.html)

[AWK 简明教程](https://coolshell.cn/articles/9070.html)

[程序员精进之路：性能调优利器--火焰图](https://zhuanlan.zhihu.com/p/147875569)

[pro_git](https://git-scm.com/book/zh/v2）

[pycallgraph 函数代码生成调用图](https://github.com/gak/pycallgraph)

[在Linux下做性能分析3：perf](https://zhuanlan.zhihu.com/p/22194920)

[内存泄漏检测工具valgrind神器](https://zhuanlan.zhihu.com/p/75416381)

[Linux下各种查找命令（find, grep, which, whereis, locate）](https://blog.csdn.net/wzzfeitian/article/details/40985549)

[cuda版本切换](https://blog.csdn.net/yinxingtianxia/article/details/80462892)

[/usr/bin/ld: cannot find -lxxx 的解决办法](https://www.cnblogs.com/zhming26/p/6164131.html)

[通过tar file来安装TensorRT](https://zhuanlan.zhihu.com/p/259848864)

[How to Quickly Resize, Convert & Modify Images from the Linux Terminal](https://www.howtogeek.com/109369/how-to-quickly-resize-convert-modify-images-from-the-linux-terminal/)

[Linux命令必知必会](https://github.com/mylxsw/growing-up/blob/master/doc/Linux%E5%91%BD%E4%BB%A4%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A.md)

[Linux启动顺序、运行级别及开机启动](https://blog.csdn.net/soonfly/article/details/72876001)


**VScode:**

[快捷键大全](https://blog.csdn.net/crper/article/details/54099319)

**markdown技巧**

[Markdown进阶（更改字体、颜色、大小，设置文字背景色，调整图片大小设置居中）](https://blog.csdn.net/heimu24/article/details/81189700)

[Typora画流程图、时序图(顺序图)、甘特图（转）](https://www.jianshu.com/p/7ddbb7dc8fec)

[如何用 Typora 神器从容地写作](https://www.jianshu.com/p/602f6ecf48fd)

**tmux**

[tmux使用教程](https://www.ruanyifeng.com/blog/2019/10/tmux.html)

**apt-get相关**

[add-apt-repository使用](https://blog.csdn.net/l740450789/article/details/50856596)

[etc/profile和~/.bashrc的区别](https://blog.csdn.net/ZoeYen_/article/details/78560905)

[Ubuntu设置代理](https://www.jianshu.com/p/f569a404ee0b)

**ack代码搜索**

[linux下的高效代码搜索工具-ack](https://www.the5fire.com/about-ack-grep-in-linux.html)

**linux信号量同步**

[线程同步之信号量（sem_init,sem_post,sem_wait）](https://www.cnblogs.com/zhengAloha/p/8665719.html)


**jenkins使用**

[Jenkins详细教程](https://www.jianshu.com/p/5f671aca2b5a)

**定位命令**

[lsof命令](https://www.cnblogs.com/peida/archive/2013/02/26/2932972.html)

[net-tools 和 iproute2](https://blog.csdn.net/kevin3101/article/details/52368860)

[用tcpdump抓包](https://www.cnblogs.com/chyingp/p/linux-command-tcpdump.html)

**工具使用**

[ubuntu 16.04 安装VNC远程桌面](https://blog.csdn.net/wuchenlhy/article/details/79865437)


**性能调优**

[arm优化——绑定核](https://zhuanlan.zhihu.com/p/138905432)



**linux/unix编程**

[Linux/UNIX系统编程手册](https://zhuanlan.zhihu.com/p/70973666)


## notes

### linux 命令

convert  example.png  -resize  200x100!  example.png

ps aux最初用到Unix Style中，而ps -ef被用在System V Style中，| grep xxx, 对进程进行监测和控制。

find . -name  "*.java"，指定多个起始目录：  find /usr /home  /tmp -name "*.jar"

find 后，-exec 执行复制命令：

find . -name  "*.png"  -exec cp {}  /data1/visiondection/VPS/vps_problem/segbuilding_dataset/openscene/ \;

tee命令用于将标准输入拷贝到标准输出， echo "hello,world"|tee -a test.txt

第三方库 三板斧：

include_directories(../../../thirdparty/comm/include)

link_libraries(“/home/server/third/lib/libcommon.a”)

target_link_libraries(myProject libcomm.a)

vi常用命令:

:set number

add-apt-repository使用： 

安装software-properties-common，配置代理export代理 或者bashrc ----> sudo apt-add-repository ppa:ubuntu-mozilla-daily/ppa  --->sudo apt-get update  --->sudo apt-get install firefox-4.0

查看符号表：
strings libstdc++.so.6|grep GLIBC

查看so依赖：
ldd  + so

查看当前目录下文件夹大小： du -h --max-depth=1

代码搜索ack：  

当前目录下 ack -r "xxx"  就可以搜索所有

信号量线程同步：

    int sem_init(sem_t *sem, int pshared, unsigned int value);，其中sem是要初始化的信号量，pshared表示此信号量是在进程间共享还是线程间共享，value是信号量的初始值。
    
    int sem_destroy(sem_t *sem);,其中sem是要销毁的信号量。只有用sem_init初始化的信号量才能用sem_destroy销毁。
    
    int sem_wait(sem_t *sem);等待信号量，如果信号量的值大于0,将信号量的值减1,立即返回。如果信号量的值为0,则线程阻塞。相当于P操作。成功返回0,失败返回-1。
    
    int sem_post(sem_t *sem); 释放信号量，让信号量的值加1。相当于V操作。
    
改变文件用户和组： chown  chgrp

查看jobs:  jobs

查看文件修改时间和大小:  ls -lht

**链接**

硬链接(hard link)与软链接(symbolic link)，硬链接的意思是一个档案可以有多个名称，而软链接的方式则是产生一个特殊的档案，该档案的内容是指向另一个档案的位置。

硬链接是存在同一个文件系统中，而软链接却可以跨越不同的文件系统。

eg:

软链接：ln -s [源文件或目录][目标文件或目录]

硬链接：ln  [源文件或目录][目标文件或目录]

apt-get锁定问题： sudo rm -rf /var/cache/apt/archives/lock


### markdown

添加代码说明： ```c++   ```




