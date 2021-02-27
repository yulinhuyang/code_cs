
## 版本控制(Git)

### Git 的数据模型

Git 将顶级目录中的文件和文件夹作为集合，并通过一系列快照来管理其历史记录

文件被称作Blob对象（数据对象），也就是一组数据。

目录则被称之为“树”，它将名字与Blob对象或树对象进行映射（使得目录中可以包含其他目录）。

快照则是被追踪的最顶层的树

    <root> (tree)
    |
    +- foo (tree)
    |  |
    |  + bar.txt (blob, contents = "hello world")
    |
    +- baz.txt (blob, contents = "git is wonderful")
    
**历史记录建模：关联快照**

在 Git 中，历史记录是一个由快照组成的有向无环图，快照具有多个“父辈”而非一个，因为某个快照可能由多个父辈而来。在 Git 中，这些快照被称为“提交”。通过可视化的方式来表示这些历史提交记录时。

    o <-- o <-- o <-- o
                ^  
                 \
                  --- o <-- o
              
    上面是一个 ASCII 码构成的简图，其中的 o 表示一次提交（快照），箭头指向了当前提交的父辈，这是一种“在。。。之前”，而不是“在。。。之后”的关系。

开发两个不同的特性，它们之间是相互独立的。开发完成后，这些分支可能会被合并并创建一个新的提交，这个新的提交会同时包含这些特性

**数据模型及其伪代码表示**

    // 文件就是一组数据
    type blob = array<byte>

    // 一个包含文件和目录的目录
    type tree = map<string, tree | file>

    // 每个提交都包含一个父辈，元数据和顶层树
    type commit = struct {
        parent: array<commit>
        author: string
        message: string
        snapshot: tree
    }
    
 **对象和内存寻址**
 
 Git 中的对象可以是 blob、树或提交：

    type object = blob | tree | commit
    
Git 在储存数据时，所有的对象都会基于它们的SHA-1 hash进行寻址。

    objects = map<string, object>

    def store(object):
        id = sha1(object)
        objects[id] = object

    def load(id):
        return objects[id]
        
Blobs、树和提交都一样，它们都是对象。当它们引用其他对象时，它们并没有真正的在硬盘上保存这些对象，而是仅仅保存了它们的哈希值作为引用
 
例如，above例子中的树（可以通过git cat-file -p 698281bc680d1995c5f4caaf3359721a5a58d48d 来进行可视化），看上去是这样的：    
    
    100644 blob 4448adbf7ecd394f42ae135bbeed9676e894af85    baz.txt
    040000 tree c68d233a33c5c06e0340e4c224f0afca87c8ce87    foo

树本身会包含一些指向其他内容的指针，例如baz.txt (blob) 和 foo (树)。如果我们用git cat-file -p 4448adbf7ecd394f42ae135bbeed9676e894af85，即通过哈希值查看 baz.txte 的内容，会得到以下信息：

    git is wonderful

**引用**

快照都可以通过它们的 SHA-1 哈希值来标记,不好记忆。 Git 的解决方法是给这些哈希值赋予人类可读的名字，也就是引用（references）。引用是指向提交的指针。

它是可变的（引用可以被更新，指向新的提交）。例如，master 引用通常会指向主分支的最新一次提交.

    references = map<string, string>

    def update_reference(name, id):
        references[name] = id

    def read_reference(name):
        return references[name]

    def load_reference(name_or_id):
        if name_or_id in references:
            return load(references[name_or_id])
        else:
            return load(name_or_id)

Git 就可以使用诸如 “master” 这样人类刻度的名称来表示历史记录中某个特定的提交，而不需要在使用一长串十六进制字符了

在 Git 中，我们当前的位置有一个特殊的索引，它就是 HEAD

**仓库**

 Git 仓库的定义了：对象 和 引用。
 
 在硬盘上，Git 仅存储对象和引用：因为其数据模型仅包含这些东西。所有的 git 命令都对应着对提交树的操作，例如增加对象，增加或删除引用。
 
 输入某个指令时，请思考一下这条命令是如何对底层的图数据结构进行操作的。

如果您希望修改提交树，例如“丢弃未提交的修改和将 ‘master’ 引用指向提交5d83f9e 时，有什么命令可以完成该操作（针对这个具体问题，您可以使用git checkout master; git reset --hard 5d83f9e）

**暂存区**

Git 处理这些场景的方法是使用一种叫做 “暂存区（staging area）”的机制，它允许您指定下次快照中要包括那些改动

推荐：Pro Git 中文版  https://git-scm.com/book/zh/v2

Git 的命令行接口

              基础：
              
              git help <command>: 获取 git 命令的帮助信息
              git init: 创建一个新的 git 仓库，其数据会存放在一个名为 .git 的目录下
              git status: 显示当前的仓库状态
              git add <filename>: 添加文件到暂存区
              git commit: 创建一个新的提交
  
              如何编写 良好的提交信息!： https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
              
              git log: 显示历史日志
              git log --all --graph --decorate: 可视化历史记录（有向无环图）
              git diff <filename>: 显示与上一次提交之间的差异
              git diff <revision> <filename>: 显示某个文件两个版本之间的差异
              git checkout <revision>: 更新 HEAD 和目前的分支
              
              分支和合并：
              
              git branch: 显示分支
              git branch <name>: 创建分支
              git checkout -b <name>: 创建分支并切换到该分支
              相当于 git branch <name>; git checkout <name>
              git merge <revision>: 合并到当前分支
              git mergetool: 使用工具来处理合并冲突
              git rebase: 将一系列补丁变基（rebase）为新的基线
              
              远端操作：
              
              git remote: 列出远端
              git remote add <name> <url>: 添加一个远端
              git push <remote> <local branch>:<remote branch>: 将对象传送至远端并更新远端引用
              git branch --set-upstream-to=<remote>/<remote branch>: 创建本地和远端分支的关联关系
              git fetch: 从远端获取对象/索引
              git pull: 相当于 git fetch; git merge
              git clone: 从远端下载仓库
              
              撤销：
              git commit --amend: 编辑提交的内容或信息
              git reset HEAD <file>: 恢复暂存的文件
              git checkout -- <file>: 丢弃修改
              
              Git 高级操作：
              git config: Git 是一个 高度可定制的 工具
              git clone --shallow: 克隆仓库，但是不包括版本历史信息
              git add -p: 交互式暂存
              git rebase -i: 交互式变基
              git blame: 查看最后修改某行的人
              git stash: 暂时移除工作目录下的修改内容
              git bisect: 通过二分查找搜索历史记录
              .gitignore: 指定 故意不追踪的文件

### 杂项

Shell 集成: 将 Git 状态集成到您的 shell 中会非常方便。(zsh,bash)。Oh My Zsh这样的框架中一般以及集成了这一功能

编辑器集成: 和上面一条类似，将 Git 集成到编辑器中好处多多。[fugitive.vim](https://github.com/tpope/vim-fugitive) 是 Vim 中集成 GIt 的常用插件

工作流:我们已经讲解了数据模型与一些基础命令，但还没讨论到进行大型项目时的一些惯例 ( 有很多 不同的 处理方法)

Pro Git 

Oh Shit, Git!?! ，简短的介绍了如何从 Git 错误中恢复；

Git for Computer Scientists ，简短的介绍了 Git 的数据模型，与本文相比包含较少量的伪代码以及大量的精美图片；

Git from the Bottom Up详细的介绍了 Git 的实现细节，而不仅仅局限于数据模型。好奇的同学可以看看


## 调试及性能分析

### 调试代码

日志的可读性: 日志可以支持严重等级（例如 INFO, DEBUG, WARN, ERROR等)，这使您可以根据需要过滤日志

ls 和 grep 这样的程序会使用 ANSI escape codes，它是一系列的特殊字符，可以使您的 shell 改变输出结果的颜色

如何在终端中打印多种颜色。

#!/usr/bin/env bash
for R in $(seq 0 20 255); do
    for G in $(seq 0 20 255); do
        for B in $(seq 0 20 255); do
            printf "\e[38;2;${R};${G};${B}m█\e[0m";
        done
    done
done

**第三方日志系统**

UNIX 系统来说，程序的日志通常存放在 /var/log.使用dmesg 命令来读取内核的日志

Linux 系统都会使用 systemd，这是一个系统守护进程, systemd 会将日志以某种特殊格式存放于/var/log/journal，您可以使用 journalctl 命令显示这些消息

macOS 系统中是 /var/log/system.log，内容可以使用 log show。

将日志加入到系统日志中，您可以使用 logger 这个 shell 程序。下面这个例子显示了如何使用 logger并且如何找到能够将其存入系统日志的条目

    logger "Hello Logs"
    # On macOS
    log show --last 1m | grep Hello
    # On Linux
    journalctl --since "1m ago" | grep Hello

您需要对 journalctl 和 log show 的结果进行大量的过滤，那么此时可以考虑使用它们自带的选项对其结果先过滤一遍再输出。还有一些像 lnav 这样的工具

**调试器**

Python 的调试器是pdb，ipdb 是一种增强型的 pdb

    l(ist) - 显示当前行附近的11行或继续执行之前的显示；
    s(tep) - 执行当前行，并在第一个可能的地方停止；
    n(ext) - 继续执行直到当前函数的下一条语句或者 return 语句；
    b(reak) - 设置断点（基于传入的参数）；
    p(rint) - 在当前上下文对表达式求值并打印结果。还有一个命令是pp ，它使用 pprint 打印；
    r(eturn) - 继续执行直到当前函数返回；
    q(uit) - 退出调试器。

gdb ( 以及它的改进版 pwndbg) 和 lldb

**专门工具**

程序需要执行一些只有操作系统内核才能完成的操作时，它需要使用系统调用。 在 Linux 中可以使用strace ，在 macOS 和 BSD 中可以使用 dtrace。

dtrace 用起来可能有些别扭，因为它使用的是它自有的 D 语言，但是我们可以使用一个叫做 dtruss 的封装使其具有和 strace类似的接口

使用 strace 或 dtruss 来显示ls 执行时，对stat 系统调用进行追踪对结果：

    # On Linux
    sudo strace -e lstat ls -l > /dev/null
    4
    # On macOS
    sudo dtruss -t lstat64_extended ls -l > /dev/null

网络数据包才能定位问题： 像 tcpdump 和 Wireshark 这样的网络数据包分析工具

**静态分析**

静态分析会将程序的源码作为输入然后基于编码规则对其进行分析并对代码的正确性进行推理

pyflakes: 分析代码

mypy：对代码进行类型检查

code linting：大多数的编辑器和 IDE 都支持在编辑界面显示这些工具的分析结果、高亮有警告和错误的位置

vim ：有 ale 或 syntastic 可以帮助您做同样的事情。 
 
Python ： pylint 和 pep8 是两种用于进行风格检查的工具，而 bandit 工具则用于检查安全相关的问题。

静态分析工具可以参考这个列表：

Awesome Static Analysis： https://github.com/analysis-tools-dev/static-analysis

Awesome Linters： https://github.com/caramelomartins/awesome-linters

风格检查和代码格式化：

工具可以作为补充： Python 的 black、用于 Go 语言的 gofmt、用于 Rust 的 rustfmt 或是用于 JavaScript, HTML 和 CSS 的 prettier

### 性能分析

**计时**

    import time, random
    n = random.randint(1, 10) * 100

    # 获取当前时间 
    start = time.time()

    # 执行一些操作
    print("Sleeping for {} ms".format(n))
    time.sleep(n/1000)

    # 比较当前时间和起始时间
    print(time.time() - start)

    # Output
    # Sleeping for 500 ms
    # 0.5713930130004883

执行时间（wall clock time）也可能会误导您，因为您的电脑可能也在同时运行其他进程，也可能在此期间发生了等待。 对于工具来说，需要区分真实时间、用户时间和系统时间。通常来说，用户时间+系统时间代表了您的进程所消耗的实际 CPU 

参考： https://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1

    真实时间 - 从程序开始到结束流失掉的真实时间，包括其他进程的执行时间以及阻塞消耗的时间（例如等待 I/O或网络）；
    User - CPU 执行用户代码所花费的时间；
    Sys - CPU 执行系统内核代码所花费的时间。

curl 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端（client）的 URL 工具的意思。

    $ time curl https://missing.csail.mit.edu &> /dev/null`
    real    0m2.561s
    user    0m0.015s
    sys     0m0.012s
    
### 性能分析工具（profilers）

 CPU 性能分析工具有两种： 追踪分析器（tracing）及采样分析器（sampling）。 追踪分析器 会记录程序的每一次函数调用，而采样分析器则只会周期性的监测（通常为每毫秒）您的程序并记录程序堆栈。

分析工具列表： https://jvns.ca/blog/2017/12/17/how-do-ruby---python-profilers-work-/

在 Python 中，我们使用 cProfile 模块来分析每次函数调用所消耗的时间。

    #!/usr/bin/env python

    import sys, re

    def grep(pattern, file):
        with open(file, 'r') as f:
            print(file)
            for i, line in enumerate(f.readlines()):
                pattern = re.compile(pattern)
                match = pattern.search(line)
                if match is not None:
                    print("{}: {}".format(i, line), end="")

    if __name__ == '__main__':
        times = int(sys.argv[1])
        pattern = sys.argv[2]
        for i in range(times):
            for file in sys.argv[3:]:
                grep(pattern, file)

执行： python -m cProfile -s tottime grep.py 1000 '^(import|\s*def)[^,]*$' *.py

分析结果： IO 消耗了大量的时间，编译正则表达式也比较耗费时间。因为正则表达式只需要编译一次

cProfile 分析器（以及其他一些类似的分析器）： 需要注意的是它显示的是每次函数调用的时间。

line_profiler 行分析器： 包括每行代码的执行时间， https://github.com/rkern/line_profiler

#### 内存 

 Valgrind：像 C 或者 C++ 这样的语言，内存泄漏会导致您的程序在使用完内存后不去释放它，来检查内存泄漏问题。
 
 Python 这类具有垃圾回收机制的语言，内存分析器也是很有用的，因为对于某个对象来说，只要有指针还指向它，那它就不会被回收

**memory-profiler**
 
 
   @profile
    def my_func():
        a = [1] * (10 ** 6)
        b = [2] * (2 * 10 ** 7)
        del b
        return a

    if __name__ == '__main__':
        my_func()


    $ python -m memory_profiler example.py
    Line #    Mem usage  Increment   Line Contents
    ==============================================
         3                           @profile
         4      5.97 MB    0.00 MB   def my_func():
         5     13.61 MB    7.64 MB       a = [1] * (10 ** 6)
         6    166.20 MB  152.59 MB       b = [2] * (2 * 10 ** 7)
         7     13.61 MB -152.59 MB       del b
         8     13.61 MB    0.00 MB       return a

####  事件分析 

在我们使用strace调试代码的时候，您可能会希望忽略一些特殊的代码并希望在分析时将其当作黑盒处理。

perf 命令将 CPU 的区别进行了抽象，它不会报告时间和内存的消耗，而是报告与您的程序相关的系统事件

例如，perf 可以报告不佳的缓存局部性（poor cache locality）、大量的页错误（page faults）或活锁（livelocks）。下面是关于常见命令的简介：

    perf list - 列出可以被 pref 追踪的事件；
    perf stat COMMAND ARG1 ARG2 - 收集与某个进程或指令相关的事件；
    perf record COMMAND ARG1 ARG2 - 记录命令执行的采样信息并将统计数据储存在perf.data中；
    perf report - 格式化并打印 perf.data 中的数据。

#### 可视化 

火焰图： CPU 分析数据的形式是 ，火焰图会在 Y 轴显示函数调用关系，并在 X 轴显示其耗时的比例

http://www.brendangregg.com/flamegraphs.html

火焰图同时还是可交互的，您可以深入程序的某一具体部分，并查看其栈追踪。调用图和控制流图可以显示子程序之间的关系，它将函数作为节点并把函数调用作为边。将它们和分析器的信息（例如调用次数、耗时等）放在一起使用时，调用图会变得非常有用，它可以帮助我们分析程序的流程。

Python 中您可以使用 pycallgraph 来生成这些图片

**pycallgraph：** 

函数代码转流程图 ，  pycallgraph graphviz -- ./fib.py

#### 资源监控 

**通用监控:**

最流行的工具要数 htop,了，它是 top的改进版。htop 可以显示当前运行进程的多种统计信息。htop 有很多选项和快捷键，常见的有：<F6> 进程排序、 t 显示树状结构和 h 打开或折叠线程。 

还可以留意一下 glances ，它的实现类似但是用户界面更好。

如果需要合并测量全部的进程， dstat 是也是一个非常好用的工具，它可以实时地计算不同子系统资源的度量数据，例如 I/O、网络、 CPU 利用率、上下文切换等等；

**I/O 操作 :**

iotop 可以显示实时 I/O 占用信息而且可以非常方便地检查某个进程是否正在执行大量的磁盘读写操作；
  
**磁盘使用:**

df 可以显示每个分区的信息，而 du 则可以显示当前目录下每个文件的磁盘使用情况（ disk usage）。-h 选项可以使命令以对人类（human）更加友好的格式显示数据；ncdu是一个交互性更好的 du ，它可以让您在不同目录下导航、删除文件和文件夹；

**内存使用:**

free 可以显示系统当前空闲的内存。内存也可以使用 htop 这样的工具来显示；

**打开文件:**

lsof 可以列出被进程打开的文件信息。 当我们需要查看某个文件是被哪个进程打开的时候，这个命令非常有用；

lsof | grep LISTEN 打印出所有监听端口的进程及相应的端口

**网络连接和配置:**

ss 能帮助我们监控网络包的收发情况以及网络接口的显示信息。ss 常见的一个使用场景是找到端口被进程占用的信息。如果要显示路由、网络设备和接口信息，您可以使用 ip 命令。注意，netstat 和 ifconfig 这两个命令已经被前面那些工具所代替了。

**网络使用:**

nethogs 和 iftop 是非常好的用于对网络占用进行监控的交互式命令行工具。

如果您希望测试一下这些工具，您可以使用 stress 命令来为系统人为地增加负载。


#### 专用工具

hyperfine :这样的命令行可以帮您快速进行基准测试， 

https://github.com/sharkdp/hyperfine

hyperfine来比较一下它们,fd 来代替 find

      $ hyperfine --warmup 3 'fd -e jpg' 'find . -iname "*.jpg"'
      Benchmark #1: fd -e jpg
        Time (mean ± σ):      51.4 ms ±   2.9 ms    [User: 121.0 ms, System: 160.5 ms]
        Range (min … max):    44.2 ms …  60.1 ms    56 runs

      Benchmark #2: find . -iname "*.jpg"
        Time (mean ± σ):      1.126 s ±  0.101 s    [User: 141.1 ms, System: 956.1 ms]
        Range (min … max):    0.975 s …  1.287 s    10 runs

      Summary
        'fd -e jpg' ran
         21.89 ± 2.33 times faster than 'find . -iname "*.jpg"'

    
