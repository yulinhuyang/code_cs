du -sm * | sort -nr     排序

ls -a  查看隐藏文件

du -sm * | sort -nr | head    查看最大的

精简docker镜像：
  
    删掉container 里面的core.* 相关的无效文件，然后将container export处理，再import进去，再save成tar包，就可以了


ack --python  task_0  --> 搜索指定文件的指定字符
