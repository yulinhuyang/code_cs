
**vim dos to unix**

https://remarkablemark.org/blog/2020/12/07/vim-convert-file-from-dos-to-unix/#dos-to-unix

**Umask与权限**

在Linux中设置UMASK值： https://www.cnblogs.com/wish123/p/7073114.html

Linux权限详解： https://blog.csdn.net/u013197629/article/details/73608613

即 r=4，w=2，x=1

rwx = 4 + 2 + 1 = 7

rw = 4 + 2 = 6

rx = 4 +1 = 5

umask值用于设置用户在创建文件时的默认权限，当我们在系统中创建目录或文件时，目录或文件所具有的默认权限就是由umask值决定的。

对于root用户，系统默认的umask值是0022；对于普通用户，系统默认的umask值是0002。执行umask命令可以查看当前用户的umask值。

默认情况下，对于目录，用户所能拥有的最大权限是777；对于文件，用户所能拥有的最大权限是目录的最大权限去掉执行权限，即666。因为x执行权限对于目录是必须的，没有执行权限就无法进入目录，而对于文件则不必默认赋予x执行权限。

对于root用户，他的umask值是022。当root用户创建目录时，默认的权限就是用最大权限777去掉相应位置的umask值权限，即对于所有者不必去掉任何权限，对于所属组要去掉w权限，对于其他用户也要去掉w权限，所以目录的默认权限就是755；当root用户创建文件时，默认的权限则是用最大权限666去掉相应位置的umask值，即文件的默认权限是644。    


十位权限表示: owner + group + other

-rwxrwxrwx (777)    所有用户都有读、写、执行权限。

**sh获取当前工作路径**

workdir=$(cd $(dirname $0); pwd)


ag搜索前后几行的数据： ag -C 2 example


**vim的巧妙使用**

vimdiff : https://www.freecodecamp.org/news/compare-two-files-in-linux-using-vim/

vimdiff index.js index.js.bkp  --> 比较A和B文件

**统计回显**

搜索统计回显次数： ag Region | wc -l





