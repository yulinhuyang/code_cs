Linux中ctrl-c, ctrl-z, ctrl-d 区别: https://blog.csdn.net/mylizh/article/details/38385739

ll + 目录：查看目录下内容

cp 备份常见命名 .bak

grep -rn : grep -rn可以关键词查找符合条件的文件的行, grep进行文件内容查找。

find进行文件名查找(递归,不适合查找modules)

Windows有哪些神级软件？： https://www.zhihu.com/question/465494790/answer/1999417175

**时间测试**

使用C++ 11 chrono库处理时间： https://blog.csdn.net/mo4776/article/details/119835806

Linux中时间戳和时间之间的转换: https://blog.csdn.net/Jerry_1126/article/details/81151928

C++11的时间新特性之high_resolution_clock： https://blog.csdn.net/cw_hello1/article/details/66476290

system_clock 是启动自1970的时间，是启动自系统重启的时间，steady_clock、high_resolution_clock 系统启动时间
```
//high_resolution_clock
high_resolution_clock::time_point ht = high_resolution_clock::now();
std::cout << ht.time_since_epoch().count() << std::endl;

//测试运行时间
duration<double,std::ratio<1,1000>> duration_ms=duration_cast<duration<double,std::ratio<1,1000>>>(t2-t1);
std::cout<<duration_ms.count()<<" milliseconds"<<std::endl;
```

