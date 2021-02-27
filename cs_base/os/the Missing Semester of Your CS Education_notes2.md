
## 命令行环境

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



