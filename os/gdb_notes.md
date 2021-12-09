
[100个gdb小技巧](https://github.com/hellogcc/100-gdb-tips)


GDB调试方法：

方式一：**启动程序并调试**  #gdb [program]

1.该模式针对启动失败问题非常有效，一般需要设置好so加载路径export LD_LIBRARY_PATH=xx，然后逐步跟踪程序运行到问题点。一般自己调试以及实验室环境操作。

方式二：**调试core文件**  #gdb [program] [core]

1.在这种模式下，GDB调试的其实是core文件，所以是可以直接看到core文件产生时的信息，如使用bt查看调用栈，以及寄存器现场信息。一般现网问题、自己调试以及实验室环境操作。

方式三：**调试已在运行的文件（通过pid ）**#gdb [program] [pid]

在产品环境下，很多时候当需要调试一个业务进程时，这个程序可能已经在运行了，这时候就需要使用GDB的attach功能。 

1. gdb [program] [pid]会自动识别第二个参数为pid(如果是十进制的数字的话)。

2. 运行gdb之后，在gdb中使用指令(gdb) attach [pid], 该方式最常见。

3. 如果不使用源文件调试，可以直接#gdb –p [pid]来指定pid进入attach状态。



断点命令：**break**，缩写b, 

   break <source_file_name:line_num> 文件名:行号, 
   break <function_name> 函数名
   
删除断点命令：**delete** <break_point>，缩写d, 

   delete 1  删除1号断点
   
查看堆栈命令：backtrace,缩写**bt** （打印最内n层：bt n, 最外n层： bt -n）

查看所有线程堆栈命令：thread apply all bt

查看变量命令:p

   p <para_name> 变量名 （可使用p直接修改变量值：p <para_name> =xx）
   
查看所有的局部变量: info local

转到线程令：thread <thread_number> 

转到帧命令：frame <frame_number> 

执行下一步命令：next，缩写n

继续运行直到达到下一个断点命令：continue,缩写c

进入子函数命令：step，缩写s

结束子函数返回到调用函数命令：finish，缩写f

结束循环命令：until，缩写u

查看内存地址：**examine**，缩写x, 

   x/<n/f/u> <addr>, 
      输出格式
	   x 按十六进制格式显示变量。
      d 按十进制格式显示变量。
      c 按字符格式显示变量。
      f 按浮点数格式显示变量。
      s 按字符串格式显示变量。
      举例：x/10wx addr  查看10个4字节按16进制输出
       x/10i  addr  查看该地址的10条指令
	   
**查看数据：print，缩写,** 

         print /f exp  exp是表达式，/f指定打印时的格式
         打印格式和x输出格式一致

**信号 signals,** 

   x/<n/f/u> <addr>, 
   info signals 
       打印信号种类表及GDB对他们相应的处理。
   handle signal keyword
       改变GDB对signal信号的处理。
   stop/nostop
   print/noprint
   pass/nopass
   一般常用： handle SIG35 nostop noprint (产品一般会自定义该信号的处理，因此调试过程先过滤掉)

**watch 内存**, 

         **watch**  expr：GDB在expr被程序写及其值改变时停止；
         rwatch expr：GDB在expr被程序读时停止；
         awatch expr：GDB在expr被程序读和写时停止；
         使用较多的是watch一个地址内存是否有变化，watch *(int*)addr
		 
**info register** 查看寄存器，

    i r（缩写）: 查看当前栈的所有寄存器信息，定位问题的时候经常用到；

gdb 不阻塞进程运行, 

	 一般业务进程都会有心跳机制，我们使用gdb调试的时候一般需要先关闭心跳功能，避免长时间断住进程而导致复位。
	 以下两种方式也可以做到gdb执行命令后快速退出：
	 1. gdb -batch -ex "command" -p pid （如gdb -batch -ex "call test_show() " -p 23467）；
	 2. 使用GDB的command命令，自定义操作，该使用在问题复现场景较多；

**GDB多线程调试常用命令**：

info thread                                     --显示所有thread线程（*号代表gdb的当前线程）

thread thread_num                      --gdb现场切换到对应thread

thread apply ID1 ID2 command      --让一个或者多个线程执行GDB命令command

thread apply all command         --让所有被调试线程执行GDB命令command

b xxxx thread thread_num        --只给thread_num这个线程的xxxx函数打断点

set scheduler-locking off|on|step 

off 不锁定任何线程，也就是所有线程都执行，这是默认值；

on 只有当前被调试程序会执行；

step                        --当用" step "命令调试线程时，其它线程不会执行，但是用其它命令（比如" next "）调试 线程时，其它线程也许会执行；
thread apply all bt        --查看所有线程调用栈（常用）





