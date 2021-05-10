**1 tricks**

[c++ 那些事](https://github.com/Light-City/CPlusPlusThings)

[万字长文把 VSCode 打造成 C++ 开发利器](https://zhuanlan.zhihu.com/p/96819625)

[适合 C++ 新手学习的开源项目](https://zhuanlan.zhihu.com/p/273682109)

[C++的三个阶段](https://www.zhihu.com/people/lin-qi-53-1)

[c++ 之 std::move 原理实现与用法总结](https://blog.csdn.net/p942005405/article/details/84644069)

[微软C++ 参考 ](https://docs.microsoft.com/zh-cn/cpp/cpp/cpp-language-reference?view=msvc-160)

[json for modern c++的使用](https://blog.csdn.net/fengxinlinux/article/details/71037244)

[glog](https://github.com/google/glog)

[cnpy](https://github.com/rogersce/cnpy)

[C++ 多线程教程](https://github.com/sprinfall/cpp-thread-study)

[C++11多线程并发基础入门教程](https://zhuanlan.zhihu.com/p/194198073)


https://github.com/fightingwangzq/cpp-learning

https://github.com/forthespada/InterviewGuide


**2 notes：** 

2.1 使用struct关键字和class关键字定义类以及在类的继承方面有啥区别？

（1）定义类的差别：

C语言中的struct 关键字也可以实现类，用class 关键字和struct 关键字定义类的唯一差别就在于默认访问级别不同：

默认情况下，struct 成员的访问级别为public，而class 成员的访问级别是private 。语法使用方面都是相同，直接将class 换成struct 即可。

（2）类的继承的差别：

使用class 关键字定义的类，它的派生类默认具有private 继承，而使用struct 关键字定义的类，它的派生类默认具有public 继承。其他的方面没有区别。

因此，主要就两个区别：默认的访问级别和默认的继承级别：class 都是private的，struct 都是public的。

（3） 结构体是一种值类型，而类是引用类型。值类型用于存储数据的值，引用类型用于存储对实际数据的引用。那么结构体就是当成值来使用的，类则通过引用来对实际数据操作。

结构体初始化问题：

https://blog.csdn.net/ericbar/article/details/79567108

C语言没有构造函数的概念，如同内建类型的变量：

1.定义全局或静态的struct变量时，其成员会初始化为零。
2.定义局部的struct变量时，其成员为不确定的值。

C语言对struct(以及union和数组)变量使用初始化列表。

[C语言结构体初始化的四种方法](https://blog.csdn.net/ericbar/article/details/79567108)

**3 多线程**




**4 RAII  RTTI**

https://zhuanlan.zhihu.com/p/34660259

RAII  资源获取即初始化

构造函数的写法：

      person(const std::string name = "", int age = 0) : 
      name_(name), age_(age) {
            std::cout << "Init a person!" << std::endl;
      }
      ~person() {
            std::cout << "Destory a person!" << std::endl;
      }


RTTI（Run-Time Type Identification)-运行时类型识别

[C++ typeid关键字详解](https://blog.csdn.net/gatieme/article/details/50947821)

它使程序能够获取由基指针或引用所指向的对象的实际派生类型，即允许“用指向基类的指针或引用来操作对象”的程序能够获取到“这些指针或引用所指对象”的实际派生类型。

在C++中，为了支持RTTI提供了两个操作符：dynamic_cast和typeid

dynamic_cast允许运行时刻进行类型转换，从而使程序能够在一个类层次结构中安全地转化类型，与之相对应的还有一个非安全的转换操作符static_cast，因为这不是本文的讨论重点，所以这里不再详述，感兴趣的可以自行查阅资料。

typeid是C++的关键字之一，等同于sizeof这类的操作符。typeid操作符的返回结果是名为type_info的标准库类型的对象的引用（在头文件typeinfo中定义，稍后我们看一下vs和gcc库里面的源码），它的表达式有下图两种形式。
 









