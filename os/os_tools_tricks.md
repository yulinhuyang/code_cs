###  tricks

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


**jenkins使用**

[Jenkins详细教程](https://www.jianshu.com/p/5f671aca2b5a)

**定位命令**

[lsof命令](https://www.cnblogs.com/peida/archive/2013/02/26/2932972.html)

[net-tools 和 iproute2](https://blog.csdn.net/kevin3101/article/details/52368860)

[用tcpdump抓包](https://www.cnblogs.com/chyingp/p/linux-command-tcpdump.html)

**工具使用**

[ubuntu 16.04 安装VNC远程桌面](https://blog.csdn.net/wuchenlhy/article/details/79865437)

**linux/unix编程**

[Linux/UNIX系统编程手册](https://zhuanlan.zhihu.com/p/70973666)


**bashrc与 profile区别**

[~/.profile  ~/.bashrc和 ~./bash_profile的理解](https://www.jianshu.com/p/b39fd35e2360)

**update-alternatives命令使用**

[python默认版本更换](https://www.jianshu.com/p/929cd16edc2c)


### linux 命令

**高效15工具**

[15 个『牛逼』的Linux工具，提高效率的同时增加乐趣](https://cloud.tencent.com/developer/article/1496938)

终端ctrl-r命令 + fzf : 查看历史

fzf: 命令行下模糊搜索工具

ag：比grep、ack更快的递归搜索文件内容。 

glances：更强大的 htop / top 代替者, htop 代替 top，glances 代替 htop：

tmux：终端复用工具

convert  example.png  -resize  200x100!  example.png

**tmux**

[tmux使用教程](https://www.ruanyifeng.com/blog/2019/10/tmux.html)

tmux new -s <session-name>
	
Ctrl+b d：分离当前会话

**ag命令**

ag --lua search_pattern

ag -G .lua search_pattern

**ps命令**

ps aux最初用到Unix Style中，而ps -ef被用在System V Style中，

| grep xxx, 对进程进行监测和控制。

ps -u +用户
 
**find fd命令**

find path   -option   [-print]   [-exec   -ok   command ]   {} \;

```vim
find . -name "*.c"
find . -name  "*.java"  
find /usr /home  /tmp -name "*.jar"  #指定多个起始目录
find . -name  "*.png"  -exec cp {}  /data1/visiondection/VPS/vps_problem/segbuilding_dataset/openscene/  #find 后，-exec 执行复制命令：
```

fd 命令，更快速，替代find  

https://github.com/sharkdp/fd#how-to-use

**cat命令**

```Bash
cat filename | tail -n 10        #显示后10行
cat filename | head -n 10        #显示前10行
cat filename | head -n 50 | tail -n +10   #显示10行到50行
cat filename1 filename2 | grep xxx    #在filename1 和 filename2中查找xxx关键字
cat aaaa*.log | grep xxx              #模糊匹配aaaa开头的文件并查找xxx关键字
cat aaaa*.log | grep xxx   -c         #模糊匹配aaaa开头的文件并查找xxx关键字统计出现次数
```
tee命令用于将标准输入拷贝到标准输出:

echo "hello,world"|tee -a test.txt

**vim常用命令**

```vim
:set number
光标移动到文件头部：gg
光标移动到文件尾部：G
光标迅速移动到第N行：nG
删除光标所在行：dd（其实是剪切操作）
复制一行：yy
复制N行：nyy
```
	
**代码搜索**
	
grep -rn : grep -rn可以关键词查找符合条件的文件的行, grep进行文件内容查找。

**代码搜索ack：** 

[linux下的高效代码搜索工具-ack](https://www.the5fire.com/about-ack-grep-in-linux.html)

当前目录下 ack -r "xxx"  就可以搜索所有

ack --python XXX 搜索所有python文件

**htop**

[htop使用详解--史上最强](https://cloud.tencent.com/developer/article/1115041)

[linux htop：比top更好用的top](https://www.jianshu.com/p/6e9b0cc7f348)

类似的有ps 、top

**ls du**

```vim
du -sm * | sort -nr   #排序
ls -a      #查看隐藏文件
ls -lh     #查看，用M显示
ls -lht   #查看文件修改时间和大小
ls -l | grep "^-" | wc -l    #统计当前目录下文件的个数（不包括目录）
ls -lR| grep "^-" | wc -l    #统计当前目录下文件的个数（包括子目录）
ls -lR | grep "^d" | wc -l   #查看某目录下文件夹(目录)的个数（包括子目录)
ls -lrt s* ：                #所有名称是 s 开头的文件，越新的排越后面 

ls -lR /bin：    #将 /bin 目录以下所有目录及文件详细资料列出 :
ls -AF：          #列出目前工作目录下所有文件及目录

du -sm * | sort -nr | head   #查看最大的
du -h --max-depth=1          #查看当前目录下文件夹大小： 
```

**tar zip**

zip –q –r xahot.zip /home/wwwroot/xahot

**chown chgrp**

chown [-R] aita test.txt

chgrp [-R] aita test.txt

**链接ln ln s**

硬链接(hard link)与软链接(symbolic link)，硬链接的意思是一个档案可以有多个名称，而软链接的方式则是产生一个特殊的档案，该档案的内容是指向另一个档案的位置。

硬链接是存在同一个文件系统中，而软链接却可以跨越不同的文件系统。

eg:

软链接：ln -s [源文件或目录][目标文件或目录]

硬链接：ln  [源文件或目录][目标文件或目录]

apt-get锁定问题： sudo rm -rf /var/cache/apt/archives/lock

**so相关**

查看动态库so命令：https://blog.csdn.net/syh63053767/article/details/14128373

查看符号表： strings libstdc++.so.6|grep GLIBC

查看so依赖： ldd libhello.so 

查看有哪些函数： nm libhello.so | grep printf

找so:  locate libhello.so

反汇编: objdump -S obj C

**快捷键**
	
Linux中ctrl-c, ctrl-z, ctrl-d 区别: https://blog.csdn.net/mylizh/article/details/38385739

### markdown技巧

[Markdown进阶（更改字体、颜色、大小，设置文字背景色，调整图片大小设置居中）](https://blog.csdn.net/heimu24/article/details/81189700)

[Typora画流程图、时序图(顺序图)、甘特图（转）](https://www.jianshu.com/p/7ddbb7dc8fec)

[如何用 Typora 神器从容地写作](https://www.jianshu.com/p/602f6ecf48fd)

添加代码说明： ```c++   ```

生成表格：https://www.tablesgenerator.com/

typora 画流程图、时序图(顺序图)、甘特图： https://www.runoob.com/note/47651

Typora 完全使用详解： https://sspai.com/post/54912
	
在线markdown1: https://markdown.lovejade.cn/   
	
在线markdown2: https://www.zybuluo.com/mdeditor
	
mermaid: https://github.com/mermaid-js/mermaid

### 绑定核

[arm优化——绑定核](https://zhuanlan.zhihu.com/p/138905432)

[Linux中线程与CPU核的绑定](https://www.cnblogs.com/vanishfan/archive/2012/11/16/2773325.html)

**方法1：**

int pthread_setaffinity_np(pthread_t thread, size_t cpusetsize，const cpu_set_t *cpuset);

int pthread_getaffinity_np(pthread_t thread, size_t cpusetsize, cpu_set_t *cpuset);

cpu_set_t这个结构体了。这个结构体的理解类似于select中的fd_set，可以理解为cpu集，也是通过约定好的宏来进行清除、设置以及判断：

```cpp
   void CPU_ZERO (cpu_set_t *set); 
   //将某个cpu加入cpu集中 
   void CPU_SET (int cpu, cpu_set_t *set); 
   //将某个cpu从cpu集中移出 
   void CPU_CLR (int cpu, cpu_set_t *set); 
   //判断某个cpu是否已在cpu集中设置了 
   int CPU_ISSET (int cpu, const cpu_set_t *set); 
```   
   
cpu集可以认为是一个掩码，每个设置的位都对应一个可以合法调度的 cpu，而未设置的位则对应一个不可调度的 CPU。换而言之，线程都被绑定了，只能在那些对应位被设置了的处理器上运行。通常，掩码中的所有位都被置位了，也就是可以在所有的cpu中调度。 

```cpp

    CPU_ZERO(&mask);
    CPU_SET(i, &mask);
    if (pthread_setaffinity_np(pthread_self(), sizeof(mask), &mask) < 0) {
        fprintf(stderr, "set thread affinity failed\n");
    }
    CPU_ZERO(&get);
    if (pthread_getaffinity_np(pthread_self(), sizeof(get), &get) < 0) {
        fprintf(stderr, "get thread affinity failed\n");
    }
    for (j = 0; j < num; j++) {
        if (CPU_ISSET(j, &get)) {
            printf("thread %d is running in processor %d\n", (int)pthread_self(), j);
        }
    }

```

**方法2**

sched_setaffinity 设置 CPU 亲和力的掩码

头文件 sched.h

sched_setaffinity(pid_t pid, unsigned int cpusetsize, cpu_set_t *mask)

如果pid的值为0,则表示指定的是当前进程,使当前进程运行在mask所设定的那些CPU上.

### 信号量线程同步

[线程同步之信号量（sem_init,sem_post,sem_wait）](https://www.cnblogs.com/zhengAloha/p/8665719.html)

int sem_init(sem_t *sem, int pshared, unsigned int value);，其中sem是要初始化的信号量，pshared表示此信号量是在进程间共享还是线程间共享，value是信号量的初始值。

int sem_destroy(sem_t *sem);,其中sem是要销毁的信号量。只有用sem_init初始化的信号量才能用sem_destroy销毁。

int sem_wait(sem_t *sem);等待信号量，如果信号量的值大于0,将信号量的值减1,立即返回。如果信号量的值为0,则线程阻塞。相当于P操作。成功返回0,失败返回-1。

int sem_post(sem_t *sem); 释放信号量，让信号量的值加1。相当于V操作。

### shell 脚本中if判断方法

```shell
for second in 1  
do
    num=$(ps -ef | grep service_registration | grep -v grep | wc -l)
    echo "$check_results"
	
	if [ "$num" -lt  "1" ];then
		echo "haha"
		break
	else
		./service_registration &
		echo "start rosmaster!" >> $log_path
        break
	fi
done
```


### json 处理

在线查看json www.json.cn  

jq: json文件处理以及格式化显示  

cat demo.json | jq '.id,.name,.status,.attachments'

### apt相关

[add-apt-repository使用](https://blog.csdn.net/l740450789/article/details/50856596)

[etc/profile和~/.bashrc的区别](https://blog.csdn.net/ZoeYen_/article/details/78560905)

[Ubuntu设置代理](https://www.jianshu.com/p/f569a404ee0b)

add-apt-repository使用：

安装software-properties-common，配置代理export代理 或者bashrc ----> sudo apt-add-repository ppa:ubuntu-mozilla-daily/ppa --->sudo apt-get update --->sudo apt-get install firefox-4.0

apt download docker-ce-cli 下载相关的离线安装包

dpkg -i 安装
 
### pip相关

pip install onnxruntime -i https://pypi.doubanio.com/simple/  --trusted-host pypi.doubanio.com

### yum相关

centos使用yum源

[CentOS yum 源的配置与使用](https://www.cnblogs.com/mchina/archive/2013/01/04/2842275.html)
 
### ImageMagick 使用

https://github.com/ohmycloud/ImageMagick

当前目录下批量文件resize: 

mogrify -resize 896x512! -format jpg *

批量改格式

mogrify -format png *.jpg

多张图片转pdf 

convert *.jpg foo.pdf

学习笔记：https://github.com/ohmycloud/ImageMagick/blob/master/ImageMagick%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.md

### NFS挂载

[CentOS 7 安装配置 NFS](https://abelsu7.top/2019/10/17/centos7-install-nfs/)

mount -t nfs centos-2:/mnt/kvm /mnt/kvm

### xcopy Robocopy 

多线程拷贝命令

[永不过时的 XCOPY命令](https://hellokandy.blog.csdn.net/article/details/52367906)

[Robocopy 用法](https://blog.csdn.net/mutougede/article/details/5691765)

XCOPY source [destination] [命令参数]

Xcopy   dirA   dirB   /E/H/C/I   >> D:copy.txt

robocopy     dirA   dirB /E    /MT:90  /copy:dt  /LOG:D:copy2.txt

### SHELL脚本 

[shell之if判断的总结](https://www.huaweicloud.com/articles/41b6cb8516ef474673fef27d038ca804.html)



### VScode技巧

[快捷键大全](https://blog.csdn.net/crper/article/details/54099319)

[github1s.com  VS Code](https://zhuanlan.zhihu.com/p/350330364)

[VS-code SSH 远程连接 Docker 配置](https://segmentfault.com/a/1190000039964495)

### 资源搜索

15个好用的百度网盘搜索引擎: https://zhuanlan.zhihu.com/p/60840594

https://ujuso.com/

### 代理配置的方式

当前 shell 生效:直接export

/etc/apt/apt.conf

/etc/yum.conf

~/.bashrc

/etc/profile: 全局生效

pip代理： pip install -r requirements.txt -i https://pypi.douban.com/simple --trusted-host=pypi.douban.com

或者pip.ini 添加配置

### cuda安装与版本切换
```shell 
ls  /usr/lib/x86_64-linux-gnu/libcuda.so.1 -la    #查看之前的连接
rm -rf /usr/local/cuda                            #删除之前软链接
sudo ln -s /usr/local/cuda-8.0/ /usr/local/cuda/  #新建链接
nvcc --version                                    #查看当前 cuda 版本
```

### PDF 输出 

[新CSDN文章一键打印、输出PDF](https://www.codenong.com/cs106602341)

PDF-XChange Viewer Pro Portable: pdf导出jpg

### 时间测试

使用C++ 11 chrono库处理时间： https://blog.csdn.net/mo4776/article/details/119835806

Linux中时间戳和时间之间的转换: https://blog.csdn.net/Jerry_1126/article/details/81151928

C++11的时间新特性之high_resolution_clock： https://blog.csdn.net/cw_hello1/article/details/66476290

system_clock 是启动自1970的时间，是启动自系统重启的时间，steady_clock、high_resolution_clock 系统启动时间
```
//high_resolution_clock  测试time point
high_resolution_clock::time_point ht = high_resolution_clock::now();
std::cout << ht.time_since_epoch().count() << std::endl;

//测试运行时间
duration<double,std::ratio<1,1000>> duration_ms=duration_cast<duration<double,std::ratio<1,1000>>>(t2-t1);
std::cout<<duration_ms.count()<<" milliseconds"<<std::endl;
```
### Windows软件技巧

Windows有哪些神级软件？： https://www.zhihu.com/question/465494790/answer/1999417175

