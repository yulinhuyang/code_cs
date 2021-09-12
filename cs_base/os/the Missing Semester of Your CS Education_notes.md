## prefix

1 Course overview + the shell

2 Shell Tools and Scripting

3 Editors (Vim)

4 Data Wrangling

5 Command-line Environment

6 Version Control (Git)

7 Debugging and Profiling

8 Metaprogramming

9 Security and Cryptography

10 Potpourri

11 Q&A


## 1 Course overview + the shell

### 使用 shell

date: 日期时间

echo：将该参数打印出来

shell： 一个编程环境，所以它具备变量、条件、循环和函数。当你在 shell 中执行命令时，您实际上是在执行一段 shell 可以解释执行的简短代码

echo $PATH  查看环境变量

which ： 确定某个程序名代表的是哪个具体的程序
 
 
 ### 在shell中导航
 
/ 代表的是系统的根目录

pwd  当前目录  

cd  切换目录，. 表示的是当前目录，而 .. 表示上级目录

ls -l 查看
 
 mv（用于重命名或移动文件）、 cp（拷贝文件）以及 mkdir（新建文件夹）
 
### 在程序间创建连接
 
流重定向： < file 和 > file 

    cat hello.txt
    cat < hello.txt > hello2.txt
    
>> 来向一个文件追加内容

管道： | 操作符 将一个程序的输出和另外一个程序的输入连接起来
  
    ls -l / | tail -n1

    echo 3 | sudo tee brightness

## Shell 工具和脚本

### Shell 脚本

shell脚本针对shell所从事的相关工作进行来优化

bash中为变量赋值的语法是foo=bar，访问变量中存储的数值，其语法为 $foo

Bash中的字符串通过' 和 "分隔符来定义，以'定义的字符串为原义字符串，其中的变量不会被转义，而 "定义的字符串会将变量值进行替换

    foo=bar
    echo "$foo"
    # 打印 bar
    echo '$foo'
    # 打印 $foo

bash也支持if, case, while 和 for 这些控制流关键字, bash 也支持函数

bash使用了很多特殊的变量来表示参数、错误代码和相关变量

    $0 - 脚本名
    $1 到 $9 - 脚本的参数。 $1 是第一个参数，依此类推。
    $@ - 所有参数
    $# - 参数个数
    $? - 前一个命令的返回值

退出码可以搭配&& (与操作符) 和 || (或操作符)使用，用来进行条件判断，决定是否执行其他程序。同一行的多个命令可以用 

$( CMD ) ：这样的方式来执行CMD 这个命令时，然后它的输出结果会替换掉 $( CMD )

    #!/bin/bash

    echo "Starting program at $(date)" # date会被替换成日期和时间

    echo "Running program $0 with $# arguments with pid $$"

    for file in $@; do
        grep foobar $file > /dev/null 2> /dev/null
        # 如果模式没有找到，则grep退出状态为 1
        # 我们将标准输出流和标准错误流重定向到Null，因为我们并不关心这些信息
        if [[ $? -ne 0 ]]; then
            echo "File $file does not have any foobar, adding one"
            echo "# foobar" >> "$file"
        fi
    done
    
**通配符：**

? 和 * 来匹配一个或任意个字符

花括号{} - 当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令    
    
      convert image.{png,jpg}
      # 会展开为
      convert image.png image.jpg

      cp /path/to/project/{foo,bar,baz}.sh /newpath
      # 会展开为
      cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

      # 也可以结合通配使用
      mv *{.py,.sh} folder
      # 会移动所有 *.py 和 *.sh 文件

      mkdir foo bar

      # 下面命令会创建foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h这些文件
      touch {foo,bar}/{a..h}
      touch foo/x bar/y
      # 显示foo和bar文件的不同
      diff <(ls foo) <(ls bar)
      # 输出
      # < x
      # ---
      # > y

### Shell 工具

man 命令：提供了命令的用户手册

TLDR pages ：搜索命令手册

tar 、 ffmpeg： https://tldr.ostera.io/ffmpeg

**查找文件：**

find命令会递归地搜索符合条件的文件

    # 查找所有名称为src的文件夹
    find . -name src -type d
    # 查找所有文件夹路径中包含test的python文件
    find . -path '**/test/**/*.py' -type f
    # 查找前一天修改的所有文件
    find . -mtime -1
    # 查找所有大小在500k至10M的tar.gz文件
    find . -size +500k -size -10M -name '*.tar.gz'

fd: 作为find的替代品

https://github.com/sharkdp/fd

locate: 只能通过文件名。

对比： https://unix.stackexchange.com/questions/60205/locate-vs-find-usage-pros-and-cons-of-each-other

**查找代码**

grep

-C ：获取查找结果的上下文（Context）；

-v 将对结果进行反选（Invert），也就是输出不匹配的结果

-R 会递归地进入子目录并搜索所有的文本文件

替代品：

ack

ag： https://github.com/ggreer/the_silver_searcher

ripgrep (rg)：https://github.com/BurntSushi/ripgrep

    # 查找所有使用了 requests 库的文件
    rg -t py 'import requests'
    # 查找所有没有写 shebang 的文件（包含隐藏文件）
    rg -u --files-without-match "^#!"
    # 查找所有的foo字符串，并打印其之后的5行
    rg foo -A 5
    # 打印匹配的统计信息（匹配的行和文件的数量）
    rg --stats PATTERN
    
**查找 shell 命令**

history 命令允许您以程序员的方式来访问shell中输入的历史命令

     history | grep find 会打印包含find子串的命令

Ctrl+R ： 对命令历史记录进行回溯搜索。敲 Ctrl+R 后您可以输入子串来进行匹配
    
zsh: https://github.com/zsh-users/zsh-history-substring-search

fzf: 通用对模糊查找工具，https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings#ctrl-r  可以对历史命令进行模糊查找并将结果以赏心悦目的格式输出

基于历史的自动补全： zsh： https://github.com/zsh-users/zsh-autosuggestions

您在命令的开头加上一个空格，它就不会被加进shell记录中，通过编辑。bash_history或 .zhistory移除那一项

**文件夹导航**

目录切换： 设置alias，使用 ln -s创建符号连接

fasd: https://github.com/clvv/fasd, 查找最常用和/或最近使用的文件和目录。Fasd 基于 frecency对文件和文件排序，也就是说它会同时针对频率（frequency ）和时效（ recency）进行排序

自动跳转 （autojump）:对于经常访问的目录，在目录名子串前加入一个命令 z 就可以快速切换命令到该目录

其他复杂工具：  tree, broot 或更加完整的文件管理器，例如 nnn 或 ranger


### 数据整理

日志处理：

ssh myserver journalctl | grep sshd

改进： ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' | less

我们先在远端机器上过滤文本内容，然后再将结果传输到本机。 less 为我们创建来一个文件分页器，使我们可以通过翻页的方式浏览较长的文本

甚至可以将当前过滤出的日志保存到文件中

   $ ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' > ssh.log
   $ less ssh.log

**sed**

sed  基于文本编辑器ed构建的”流编辑器”

可以利用一些简短的命令来修改文件，而不是直接操作文件的内容，最常用的是 s，即替换命令

    ssh myserver journalctl
     | grep sshd
     | grep "Disconnected from"
     | sed 's/.*Disconnected from //'

s 命令的语法：s/REGEX/SUBSTITUTION/, 其中 REGEX 部分是我们需要使用的正则表达式，而 SUBSTITUTION 是用于替换匹配结果的文本

**正则表达式**

常见的模式有：

    . 除空格之外的”任意单个字符”
    * 匹配前面字符零次或多次
    + 匹配前面字符一次或多次
    [abc] 匹配 a, b 和 c 中的任意一个
    (RX1|RX2) 任何能够匹配RX1 或 RX2的结果
    ^ 行首
    $ 行尾

* 和 + 在默认情况下是贪婪模式，会尽可能多的匹配文本。可以给 * 或 + 增加一个? 后缀使其变成非贪婪模式。

perl 的命令行模式：

    perl -pe 's/.*?Disconnected from //'


正则表达式在线调试工具regex debugger： https://regex101.com/r/qqbZqh/2

    | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'

捕获组（capture groups）： 被圆括号内的正则表达式匹配到的文本，都会被存入一系列以编号区分的捕获组中。捕获组的内容可以在替换字符串时使用

**回到数据整理**

sed  文本注入：(使用 i 命令)，打印特定的行 (使用 p命令)

sort 会对其输入数据进行排序。sort -n 会按照数字顺序对输入进行排序.  -k1,1 则表示“仅基于以空格分割的第一列进行排序”。,n 部分表示“仅排序到第n个部分”

uniq -c 会把连续出现的行折叠为一行并使用出现次数作为前缀

登陆次数最少的用户: 可以使用 head 来代替tail。或者使用sort -r来进行倒序排序

    ssh myserver journalctl
     | grep sshd
     | grep "Disconnected from"
     | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
     | sort | uniq -c
     | sort -nk1,1 | tail -n10
     | awk '{print $2}' | paste -sd,

paste命令来合并行(-s)，并指定一个分隔符进行分割 (-d)

**awk – 另外一种编辑器**

awk 程序接受一个模式串（可选），以及一个代码块，指定当模式匹配时应该做何种操作. 在代码块中，$0 表示整行的内容，$1 到 $n 为一行中的 n 个区域，区域的分割基于 awk 的域分隔符.

    | awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l

第一部分需要等于1, 其第二部分必须满足给定的一个正则表达式, 代码块中的内容则表示打印用户名。然后我们使用 wc -l 统计输出结果的行数

    BEGIN { rows = 0 }
    $1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
    END { print rows }

**分析数据**

  gnuplot: 绘制一些简单的图表

  xargs:  给命令传递参数的一个过滤器. 利用数据整理技术从一长串列表里找出你所需要安装或移除的东西
  
    rustup toolchain list | grep nightly | grep -vE "nightly-x86" | sed 's/-x86.*//' | xargs rustup toolchain uninstall

 整理二进制数据: 用 ffmpeg 从相机中捕获一张图片，将其转换成灰度图后通过SSH将压缩后的文件发送到远端服务器，并在那里解压、存档并显示。
 
    ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 -
    | convert - -colorspace gray -
    | gzip
    | ssh mymachine 'gzip -d | tee copy.jpg | env DISPLAY=:0 feh -'
 
 


## 5 Command-line Environment 命令行环境

### 任务控制

Ctrl-C :来停止命令的执行，shell 会发送一个SIGINT 信号到进程

Ctrl-\ :使用SIGQUIT 信号

      #!/usr/bin/env python
      import signal, time

      def handler(signum, time):
          print("\nI got a SIGINT, but I am not stopping")

      signal.signal(signal.SIGINT, handler)
      i = 0
      while True:
          time.sleep(.1)
          print("\r{}".format(i), end="")
          i += 1

SIGINT 和 SIGQUIT 都常常用来发出和终止程序相关的请求。

SIGTERM 则是一个更加通用的、也更加优雅地退出信号。为了发出这个信号我们需要使用 kill 命令, 它的语法是： kill -TERM <PID>

Ctrl-Z : 让 shell 发送 SIGTSTP 信号,让进程暂停

fg 或 bg 命令恢复暂停的工作。它们分别表示在前台继续或在后台继续

jobs 命令会列出当前终端会话中尚未完成的全部任务,使用 pid 引用这些任务（也可以用 pgrep 找出 pid）,可以使用百分号 + 任务编号（jobs 会打印任务编号）来选取该任务

已经在运行的进程转到后台运行: 可以键入Ctrl-Z ，然后紧接着再输入bg,

关闭终端（会发送另外一个信号SIGHUP），这些后台的进程也会终止。为了防止这种情况发生，您可以使用 nohup (一个用来忽略 SIGHUP 的封装) 来运行程序

      $ sleep 1000
      ^Z
      [1]  + 18653 suspended  sleep 1000

      $ nohup sleep 2000 &
      [2] 18745
      appending output to nohup.out

      $ jobs
      [1]  + suspended  sleep 1000
      [2]  - running    nohup sleep 2000

      $ bg %1
      [1]  - 18653 continued  sleep 1000

      $ jobs
      [1]  - running    sleep 1000
      [2]  + running    nohup sleep 2000

      $ kill -STOP %1
      [1]  + 18653 suspended (signal)  sleep 1000

SIGKILL 是一个特殊的信号，它不能被进程捕获并且它会马上结束该进程。不过这样做会有一些副作用，例如留下孤儿进程

### 终端多路复用 

 tmux:
 
 允许我们基于面板和标签分割出多个终端窗口，这样您便可以同时与多个 shell 会话进行交互,以分离当前终端会话并在将来重新连接.
 
         会话 - 每个会话都是一个独立的工作区，其中包含一个或多个窗口
                tmux 开始一个新的会话
                tmux new -s NAME 以指定名称开始一个新的会话
                tmux ls 列出当前所有会话
                在 tmux 中输入 <C-b> d ，将当前会话分离
                tmux a 重新连接最后一个会话。您也可以通过 -t 来指定具体的会话
        窗口 - 相当于编辑器或是浏览器中的标签页，从视觉上将一个会话分割为多个部分
                <C-b> c 创建一个新的窗口，使用 <C-d>关闭
                <C-b> N 跳转到第 N 个窗口，注意每个窗口都是有编号的
                <C-b> p 切换到前一个窗口
                <C-b> n 切换到下一个窗口
                <C-b> , 重命名当前窗口
                <C-b> w 列出当前所有窗口
        面板 - 像 vim 中的分屏一样，面板使我们可以在一个屏幕里显示多个 shell
                <C-b> " 水平分割
                <C-b> % 垂直分割
                <C-b> <方向> 切换到指定方向的面板，<方向> 指的是键盘上的方向键
                <C-b> z 切换当前面板的缩放
                <C-b> [ 开始往回卷动屏幕。您可以按下空格键来开始选择，回车键复制选中的部分
                <C-b> <空格> 在不同的面板排布间切换

https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/

screen 命令

**别名**

shell 的别名相当于一个长命令的缩写，shell 会自动将其替换成原本的命令

       alias alias_name="command_to_alias arg1 arg2"

        # 创建常用命令的缩写
        alias ll="ls -lh"

        # 能够少输入很多
        alias gs="git status"
        alias gc="git commit"
        alias v="vim"

        # 手误打错命令也没关系
        alias sl=ls

        # 重新定义一些命令行的默认行为
        alias mv="mv -i"           # -i prompts before overwrite
        alias mkdir="mkdir -p"     # -p make parent dirs as needed
        alias df="df -h"           # -h prints human readable format

        # 别名可以组合使用
        alias la="ls -A"
        alias lla="la -l"

        # 在忽略某个别名
        \ls
        # 或者禁用别名
        unalias la

        # 获取别名的定义
        alias ll
        # 会打印 ll='ls -lh'

### 配置文件（Dotfiles） 

 点文件的配置文件,默认是隐藏文件
 
 bash: .bashrc 或 .bash_profile 来进行配置,可以添加需要在启动时执行的命令,或者环境变量
 
 很多程序都要求您在 shell 的配置文件中包含一行类似 export PATH="$PATH:/path/to/program/bin" 
 
 其他的工具也可以通过点文件进行配置：
 
      bash - ~/.bashrc, ~/.bash_profile
      git - ~/.gitconfig
      vim - ~/.vimrc 和 ~/.vim 目录
      ssh - ~/.ssh/config
      tmux - ~/.tmux.conf
 
管理这些配置文件：

它们应该在它们的文件夹下，并使用版本控制系统进行管理，然后通过脚本将其 符号链接 到需要的地方

      安装简单: 如果您登录了一台新的设备，在这台设备上应用您的配置只需要几分钟的时间；
      可以执行: 您的工具在任何地方都以相同的配置工作
      同步: 在一处更新配置文件，可以同步到其他所有地方
      变更追踪: 您可能要在整个程序员生涯中持续维护这些配置文件，而对于长期项目而言，版本历史是非常重要的
      
配置文件中需要放些什么： 在线文档和帮助手册了解所使用工具的设置项；直接浏览其他人的配置文件：您可以在这里找到无数的dotfiles 仓库 

参考： https://github.com/mathiasbynens/dotfiles

https://github.com/anishathalye/dotfiles

https://github.com/jonhoo/configs

**可移植性**

如果配置文件 if 语句，则您可以借助它针对不同的设备编写不同的配置

      if [[ "$(uname)" == "Linux" ]]; then {do_something}; fi

      # 使用和 shell 相关的配置时先检查当前 shell 类型
      if [[ "$SHELL" == "zsh" ]]; then {do_something}; fi

      # 您也可以针对特定的设备进行配置
      if [[ "$(hostname)" == "myServer" ]]; then {do_something}; fi

配置文件支持 include 功能，您也可以多加利用，在每天设备上创建配置文件 ~/.gitconfig_local 来包含与该设备相关的特定配置

      ~/.gitconfig 可以这样编写：

      [include]
          path = ~/.gitconfig_local

不同的程序之间共享某些配置： 要在 bash 和 zsh 中同时启用一些别名，您可以把它们写在 .aliases 里，然后在这两个 shell 里应用

      # Test if ~/.aliases exists and source it
      if [ -f ~/.aliases ]; then
          source ~/.aliases
      fi

### 远端设备 

以用户名 foo 登录服务器 bar.mit.edu

      ssh foo@bar.mit.edu
      
直接远程执行命令：ssh foobar@server ls | grep PATTERN

**SSH 密钥**

私钥(通常是 ~/.ssh/id_rsa 或者 ~/.ssh/id_ed25519) 等效于您的密码，避免每次登录都输入密码

密钥生成：

使用 ssh-keygen 命令可以生成一对密钥

      ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519

可以密钥设置密码，防止有人持有您的私钥并使用它访问您的服务器。您可以使用 ssh-agent 或 gpg-agent ，这样就不需要每次都输入该密码

基于密钥的认证机制： 

ssh 会查询 .ssh/authorized_keys 来确认那些用户可以被允许登录。

可以通过下面的命令将一个公钥拷贝到这里

      cat .ssh/id_ed25519.pub | ssh foobar@remote 'cat >> ~/.ssh/authorized_keys'
      
       或 ssh-copy-id -i .ssh/id_ed25519.pub foobar@remote
    
**通过 SSH 复制文件**

ssh+tee, 最简单的方法是执行 ssh 命令，然后通过这样的方法利用标准输入实现 cat localfile | ssh remote_server tee serverfile。回忆一下，tee 命令会将标准输出写入到一个文件；

scp ：当需要拷贝大量的文件或目录时，使用scp 命令则更加方便，因为它可以方便的遍历相关路径。语法如下：scp path/to/local_file remote_host:path/to/remote_file；

rsync 对 scp 进行来改进，它可以检测本地和远端的文件以防止重复拷贝。它还可以提供一些诸如符号连接、权限管理等精心打磨的功能。甚至还可以基于 --partial标记实现断点续传。rsync 的语法和scp类似

**端口转发**

本地端口转发；

即远端设备上的服务监听一个端口，而您希望在本地设备上的一个端口建立连接并转发到远程端口

远端服务器上运行 Jupyter notebook 并监听 8888 端口。 然后，建立从本地端口 9999 的转发，使用 ssh -L 9999:localhost:8888 foobar@remote_server

远程端口转发： ssh -R

**SSH 配置**

使用 ~/.ssh/config：

      Host vm
          User foobar
          HostName 172.16.174.141
          Port 2222
          IdentityFile ~/.ssh/id_ed25519
          LocalForward 9999 localhost:8888

      # 在配置文件中也可以使用通配符
      Host *.mit.edu
          User foobaz

服务器侧的配置通常放在 /etc/ssh/sshd_config。您可以在这里配置免密认证、修改 shh 端口、开启 X11 转发

Mosh（即 mobile shell ）：对 ssh 进行了改进，它允许连接漫游、间歇连接及智能本地回显

sshfs： 远端文件夹挂载到本地会比较方便， sshfs 可以将远端服务器上的一个文件夹挂载到本地，然后您就可以使用本地的编辑器      
      
### Shell & 框架

bash shell，因为它是目前最通用的 shell，大多数的系统都将其作为默认 shell。但是，它并不是唯一的选项

例如，zsh shell 是 bash 的超集并提供了一些方便的功能：

      智能替换, **
      行内替换/通配符扩展
      拼写纠错
      更好的 tab 补全和选择
      路径展开 (cd /u/lo/b 会被展开为 /usr/local/bin)

**框架**
 
也可以改进您的 shell。比较流行的通用框架包括prezto 或 oh-my-zsh。还有一些更精简的框架，它们往往专注于某一个特定功能，例如zsh 语法高亮 或 zsh 历史子串查询。 像 fish 这样的 shell 包含了很多用户友好的功能，其中一些特性包括：

      向右对齐
      命令语法高亮
      历史子串查询
      基于手册页面的选项补全
      更智能的自动补全
      提示符主题

终端模拟器：

对比：

[A look at terminal emulators, part 1](https://anarc.at/blog/2018-04-12-terminal-emulators-1/)

https://bbs.huaweicloud.com/blogs/157009

      可以从下面这些方面来配置您的终端：

      字体选择
      彩色主题
      快捷键
      标签页/面板支持
      回退配置
      性能（像 Alacritty 或者 kitty 这种比较新的终端，它们支持GPU加速）。


某个进程结束后再开始另外一个进程： sleep 60 & 作为先执行的程序。一种方法是使用 wait 命令

使用 pgrep 来查找 pid 并使用 pkill 结束进程而不需要手动输入pid




## 6 Version Control (Git) 版本控制(Git)

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


## 7 Debugging and Profiling 调试及性能分析

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

    
## 8 Metaprogramming 元编程

### 构建系统

您需要定义依赖、目标和规则。您必须告诉构建系统您具体的构建目标，系统的任务则是找到构建这些目标所需要的依赖，并根据规则构建所需的中间产物，直到最终目标被构建出来

make 是最常用的构建系统之一, 当您执行 make 时，它会去参考当前目录下名为 Makefile 的文件。所有构建目标、相关依赖和规则都需要在该文件中定义.

    paper.pdf: paper.tex plot-data.png
      pdflatex paper.tex

    plot-%.png: %.dat plot.py
      ./plot.py -i $*.dat -o $@

冒号左侧的是构建目标，冒号右侧的是构建它所需的依赖。缩进的部分是从依赖构建目标时需要用到的一段程序

在 make 中，第一条指令还指明了构建的目的，如果您使用不带参数的 make，这便是我们最终的构建结果

规则中的 % 是一种模式，它会匹配其左右两侧相同的字符串

再次执行 make ,make回去检查之前的构建是因其依赖改变而需要被更新.

makefiles 都提供了 一个名为 clean 的构建目标，可以使用它清理文件，让 make 重新构建

### 依赖管理

语义版本号： 主版本号.次版本号.补丁号。相关规则有：

如果新的版本没有改变 API，请将补丁号递增；

如果您添加了 API 并且该改动是向后兼容的，请将次版本号递增；

如果您修改了 API 但是它并不向后兼容，请将主版本号递增。

**锁文件（lock files）**

锁文件列出了您当前每个依赖所对应的具体版本号

极端的依赖锁定叫做 vendoring，它会把您的依赖中的所有代码直接拷贝到您的项目中

### 持续集成系统

持续集成，或者叫做 CI 是一种雨伞术语（umbrella term）。比较大的有 Travis CI、Azure Pipelines 和 GitHub Actions。

工作原理都是类似的：您需要在代码仓库中添加一个文件，描述当前仓库发生任何修改时，应该如何应对

最常见的规则是：如果有人提交代码，执行测试套。当这个事件被触发时，CI 提供方会启动一个（或多个）虚拟机，执行您制定的规则，并且通常会记录下相关的执行结果。CI 会自动帮我们处理后续的事情。

**测试简介**

测试套：所有测试的统称

单元测试：一个“微型测试”，用于对某个封装的特性进行测试

集成测试：: 一个“宏观测试”，针对系统的某一大部分进行，测试其不同的特性或组件是否能协同工作。

回归测试：用于保证之前引起问题的 bug 不会再次出现

模拟（Mocking）: 使用一个假的实现来替换函数、模块或类型，屏蔽那些和测试不相关的内容。例如，您可能会“模拟网络连接” 或 “模拟硬盘”


## 大杂烩

###  修改键位映射

当某一个按键被按下，软件截获键盘发出的按键事件（keypress event）并使用另外一个事件取代。

比如：

    将 Caps Lock 映射为 Ctrl 或者 Escape：Caps Lock 使用了键盘上一个非常方便的位置而它的功能却很少被用到，所以我们（讲师）非常推荐这个修改；
    将 PrtSc 映射为播放/暂停：大部分操作系统支持播放/暂停键；
    交换 Ctrl 和 Meta 键（Windows 的徽标键或者 Mac 的 Command 键）。

也可以将键位映射为任意常用的指令。软件监听到特定的按键组合后会运行设定的脚本。

        打开一个新的终端或者浏览器窗口；
        输出特定的字符串，比如：一个超长邮件地址或者 MIT ID；
        使计算机或者显示器进入睡眠模式。
        
 甚至更复杂的修改也可以通过软件实现：

        映射按键顺序，比如：按 Shift 键五下切换大小写锁定；
        区别映射单点和长按，比如：单点 Caps Lock 映射为 Escape，而长按 Caps Lock 映射为 Ctrl；
        对不同的键盘或软件保存专用的映射配置。
        
修改键位映射的软件:

    macOS - karabiner-elements, skhd 或者 BetterTouchTool
    Linux - xmodmap 或者 Autokey
    Windows - 控制面板，AutoHotkey 或者 SharpKeys
    QMK - 如果你的键盘支持定制固件，QMK 可以直接在键盘的硬件上修改键位映射。保留在键盘里的映射免除了在别的机器上的重复配置。
    
### 守护进程

守护进程,在后台保持运行。以守护进程运行的程序名一般以 d 结尾，比如 SSH 服务端 sshd，用来监听传入的 SSH 连接请求并对用户进行鉴权。

Linux 中的 systemd（the system daemon）是最常用的配置和运行守护进程的方法。运行 systemctl status 命令可以看到正在运行的所有守护进程。

用户使用 systemctl 命令和 systemd 交互来enable（启用）、disable（禁用）、start（启动）、stop（停止）、restart（重启）、或者status（检查）配置好的守护进程及系统服务

systemd 提供了一个很方便的界面用于配置和启用新的守护进程或系统服务。   
   
    # /etc/systemd/system/myapp.service
    [Unit]
    # 配置文件描述
    Description=My Custom App
    # 在网络服务启动后启动该进程
    After=network.target

    [Service]
    # 运行该进程的用户
    User=foo
    # 运行该进程的用户组
    Group=foo
    # 运行该进程的根目录
    WorkingDirectory=/home/foo/projects/mydaemon
    # 开始该进程的命令
    ExecStart=/usr/bin/local/python3.7 app.py
    # 在出现错误时重启该进程
    Restart=on-failure

    [Install]
    # 相当于Windows的开机启动。即使GUI没有启动，该进程也会加载并运行
    WantedBy=multi-user.target
    # 如果该进程仅需要在GUI活动时运行，这里应写作：
    # WantedBy=graphical.target
    # graphical.target在multi-user.target的基础上运行和GUI相关的服务   

### FUSE

UNIX 文件系统在传统上是以内核模块的形式实现，导致只有内核可以进行文件系统相关的调用

FUSE（用户空间文件系统）允许运行在用户空间上的程序实现文件系统调用，并将这些调用与内核接口联系起来

FUSE 可以用于实现如：一个将所有文件系统操作都使用 SSH 转发到远程主机，由远程主机处理后返回结果到本地计算机的虚拟文件系统。sshfs就是一个实现了这种功能的 FUSE 文件系统

一些有趣的 FUSE 文件系统包括：

    sshfs：使用 SSH 连接在本地打开远程主机上的文件
    rclone：将 Dropbox、Google Drive、Amazon S3、或者 Google Cloud Storage 一类的云存储服务挂载为本地文件系统
    gocryptfs：覆盖在加密文件上的文件系统。文件以加密形式保存在磁盘里，但该文件系统挂载后用户可以直接从挂载点访问文件的明文
    kbfs：分布式端到端加密文件系统。在这个文件系统里有私密（private），共享（shared），以及公开（public）三种类型的文件夹
    borgbackup：方便用户浏览删除重复数据后的压缩加密备份

### 备份

有效备份方案的几个核心特性是：版本控制，删除重复数据，以及安全性。

更多： https://missing-semester-cn.github.io/2019/backups/

### API（应用程序接口）

大多数线上服务提供的 API（应用程序接口）让你可以通过编程方式来访问这些服务的数据。比如，美国国家气象局就提供了一个可以从 shell 中获取天气预报的 API.

API 大多具有类似的格式。它们的结构化 URL 通常使用 api.service.com 作为根路径.
 
 以美国天气数据为例，为了获得某个地点的天气数据，你可以发送一个 GET 请求（比如使用curl）到https://api.weather.gov/points/42.3604,-71.094。返回中会包括一系列用于获取特定信息（比如小时预报、气象观察站信息等）的 URL。通常这些返回都是JSON格式，你可以使用jq等工具来选取需要的部分.

有些需要认证的 API 通常要求用户在请求中加入某种私密令牌（secret token）,大多数 API 都会使用 OAuth，OAuth 通过向用户提供一系列仅可用于该 API 特定功能的私密令牌进行校验

IFTTT 这个网站可以将很多 API 整合在一起，让某 API 发生的特定事件触发在其他 API 上执行的任务

### 常见命令行标志参数及模式

    --help 或者类似的标志参数（flag）来显示它们的简略用法
    --version 或者 -V 标志参数可以让工具显示它的版本信息
    --verbose 或者 -v 标志参数来输出详细的运行信息，-vvv，可以让工具输出更详细的信息
    --quiet 标志参数来抑制除错误提示之外的其他输出
    - 代替输入或者输出文件名意味着工具将从标准输入（standard input）获取所需内容，或者向标准输出（standard output）输出结果
    - 会造成破坏性结果的工具一般默认进行非递归的操作，但是支持使用“递归”（recursive）标志函数（通常是 -r）
    - 有的时候你可能需要向工具传入一个 看上去 像标志参数的普通参数，可以使用特殊参数 -- 让某个程序 停止处理 -- 后面出现的标志参数以及选项
     eg: rm -- -r 会让 rm 将 -r 当作文件名
     
### 其他

**窗口管理器**

堆叠式（floating/stacking）

平铺式（tiling）管理器,把不同的窗口像贴瓷砖一样平铺在一起而不和其他窗口重叠

**VPN**

 WireGuard 以及 Algo
   
**Markdown**

Markdown 致力于将人们编写纯文本时的一些习惯标准化。

    用*包围的文字表示强调（斜体），或者用**表示特别强调（粗体）；
    以#开头的行是标题，#的数量表示标题的级别，比如：##二级标题；
    以-开头代表一个无序列表的元素。一个数字加.（比如1.）代表一个有序列表元素；
    反引号 `（backtick）包围的文字会以代码字体显示。如果要显示一段代码，可以在每一行前加四个空格缩进，或者使用三个反引号包围整个代码片段：

        就像这样
        
    如果要添加超链接，将 需要显示 的文字用方括号包围，并在后面紧接着用圆括号包围链接：[显示文字](指向的链接)。

**Hammerspoon (macOS 桌面自动化)**

Hammerspoon 是面向 macOS 的一个桌面自动化框架。它允许用户编写和操作系统功能挂钩的 Lua 脚本，从而与键盘、鼠标、窗口、文件系统等交互。示例应用:

    绑定移动窗口到的特定位置的快捷键
    创建可以自动将窗口整理成特定布局的菜单栏按钮
    在你到实验室以后，通过检测所连接的 WiFi 网络自动静音扬声器
    在你不小心拿了朋友的充电器时弹出警告

**开机引导以及 Live USB**

BIOS 或者 UEFI 会在加载操作系统之前对硬件系统进行初始化，这被称为引导（booting）。在 BIOS 菜单中你可以对硬件相关的设置进行更改，也可以在引导菜单中选择从硬盘以外的其他设备加载操作系统——比如 Live USB。

Live USB 是包含了完整操作系统的闪存盘：

    作为安装操作系统的启动盘；
    在不将操作系统安装到硬盘的情况下，直接运行 Live USB 上的操作系统；
    对硬盘上的相同操作系统进行修复；
    恢复硬盘上的数据。

Live USB 通过在闪存盘上 写入 操作系统的镜像制作，而写入不是单纯的往闪存盘上复制 .iso 文件。你可以使用 UNetbootin 、Rufus 等 Live USB 写入工具制作。

**Docker, Vagrant, VMs, Cloud, OpenStack**

Vagrant 是一个构建和配置虚拟开发环境的工具。它支持用户在配置文件中写入比如操作系统、系统服务、需要安装的软件包等描述，然后使用 vagrant up 命令在各种环境（VirtualBox，KVM，Hyper-V等）中启动一个虚拟机。Docker 是一个使用容器化概念的类似工具。

**软件包和工具存储**

https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard

    /bin - 基本命令二进制文件
    /sbin - 基本的系统二进制文件，通常是root运行的
    /dev - 设备文件，通常是硬件设备接口文件
    /etc - 主机特定的系统配置文件
    /home - 系统用户的家目录
    /lib - 系统软件通用库
    /opt - 可选的应用软件
    /sys - 包含系统的信息和配置(第一堂课介绍的)
    /tmp - 临时文件( /var/tmp ) 通常在重启之间删除
    /usr/ - 只读的用户数据
    /usr/bin - 非必须的命令二进制文件
    /usr/sbin - 非必须的系统二进制文件，通常是由root运行的
    /usr/local/bin - 用户编译程序的二进制文件
    /var -变量文件 像日志或缓存








