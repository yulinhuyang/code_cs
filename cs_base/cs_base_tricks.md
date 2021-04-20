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

**tmux**

[tmux使用教程](https://www.ruanyifeng.com/blog/2019/10/tmux.html)

**apt-get相关**

[add-apt-repository使用](https://blog.csdn.net/l740450789/article/details/50856596)

[etc/profile和~/.bashrc的区别](https://blog.csdn.net/ZoeYen_/article/details/78560905)

[Ubuntu设置代理](https://www.jianshu.com/p/f569a404ee0b)

## notes

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





