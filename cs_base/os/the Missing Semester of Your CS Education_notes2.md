
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

**终端多路复用**

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

**配置文件（Dotfiles）**

 
 


