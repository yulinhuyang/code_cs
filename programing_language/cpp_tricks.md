1  tricks


[万字长文把 VSCode 打造成 C++ 开发利器](https://zhuanlan.zhihu.com/p/96819625)

[适合 C++ 新手学习的开源项目](https://zhuanlan.zhihu.com/p/273682109)

[C++的三个阶段](https://www.zhihu.com/people/lin-qi-53-1)

[c++ 之 std::move 原理实现与用法总结](https://blog.csdn.net/p942005405/article/details/84644069)

[微软C++ 参考 ](https://docs.microsoft.com/zh-cn/cpp/cpp/cpp-language-reference?view=msvc-160)

[json for modern c++的使用](https://blog.csdn.net/fengxinlinux/article/details/71037244)

[glog](https://github.com/google/glog)

[cnpy](https://github.com/rogersce/cnpy)

2 面试notes： 

2.1 使用struct关键字和class关键字定义类以及在类的继承方面有啥区别？

（1）定义类的差别：

C语言中的struct 关键字也可以实现类，用class 关键字和struct 关键字定义类的唯一差别就在于默认访问级别不同：

默认情况下，struct 成员的访问级别为public，而class 成员的访问级别是private 。语法使用方面都是相同，直接将class 换成struct 即可。

（2）类的继承的差别：

使用class 关键字定义的类，它的派生类默认具有private 继承，而使用struct 关键字定义的类，它的派生类默认具有public 继承。其他的方面没有区别。

因此，主要就两个区别：默认的访问级别和默认的继承级别：class 都是private的，struct 都是public的。

（3） 结构体是一种值类型，而类是引用类型。值类型用于存储数据的值，引用类型用于存储对实际数据的引用。那么结构体就是当成值来使用的，类则通过引用来对实际数据操作。
