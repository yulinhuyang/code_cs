
### 1 tricks 

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


### 2  使用struct关键字和class关键字定义类以及在类的继承方面有啥区别？

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

### 3 RAII   RTTI 

https://zhuanlan.zhihu.com/p/34660259

RAII (Resource Acquisition Is Initialization) 资源获取即初始化

构造函数的写法：
```cpp
person(const std::string name = "", int age = 0) : 
name_(name), age_(age) {
      std::cout << "Init a person!" << std::endl;
}
~person() {
      std::cout << "Destory a person!" << std::endl;
}
```

RTTI（Run-Time Type Identification)-运行时类型识别

[C++ typeid关键字详解](https://blog.csdn.net/gatieme/article/details/50947821)

它使程序能够获取由基指针或引用所指向的对象的实际派生类型，即允许“用指向基类的指针或引用来操作对象”的程序能够获取到“这些指针或引用所指对象”的实际派生类型。

在C++中，为了支持RTTI提供了两个操作符：dynamic_cast和typeid

dynamic_cast允许运行时刻进行类型转换，从而使程序能够在一个类层次结构中安全地转化类型，与之相对应的还有一个非安全的转换操作符static_cast，因为这不是本文的讨论重点，所以这里不再详述，感兴趣的可以自行查阅资料。

typeid是C++的关键字之一，等同于sizeof这类的操作符。typeid操作符的返回结果是名为type_info的标准库类型的对象的引用（在头文件typeinfo中定义，稍后我们看一下vs和gcc库里面的源码），它的表达式有下图两种形式。

### 4 类对象大小计算

计算一个类对象的大小时的规律： 

1)空类、单一继承的空类、多重继承的空类所占空间大小为：1（字节，下同）；

2)一个类中，**虚函数本身、成员函数（包括静态与非静态）和静态数据成员都是不占用类对象的存储空间的**；

3)因此一个对象的大小 ≥ 所有**非 静态成员 大小的总和**；

4)当类中**声明了虚函数**（不管是 1 个还是多个），那么在实例化对象时，编译器会自动在对象里安 插一个指针 **vPtr** 指向虚函数表 **VTable**；

5)虚承继的情况：由于涉及到虚函数表和虚基表，会同时增加一个（多重虚继承下对应多个） **vfPtr** 指针指向**虚函数表 vfTable** 和一个 **vbPtr** 指针指向虚基表 **vbTable**，这两者所占的空间大小 为：**8**（或 **8 乘以多继承时父类的个数**）； 

6)在考虑以上内容所占空间的大小时，还要注意编译器下的“**补齐**”padding 的影响，即编译器会插 入多余的字节补齐； 

7)类对象的大小 = **各非静态数据成员（**包括父类的非静态数据成员但都不包括所有的成员函数）的总和+ **vfptr** 指针(多继承下可能不止一个) + **vbptr** 指针(多继承下可能不止一个) + 编译器额外增加的字节。

### 5 多态与虚函数

**多态构造与析构**

C++ 多态的实现及原理： https://www.cnblogs.com/cxq0017/p/6074247.html

delete P：析构父类，如果父类为虚，则先子类，再父类，否则只有父类。

p->Print():  print 非虚，仍然调用父类，print为虚，且被子类重写了，调用子类。本质和delete P一样的，非虚调父类，虚调子类。

除子类，都会调用父类的析构函数

多态用虚函数来实现，结合动态绑定. 

**虚函数**

C++多态虚函数表详解(多重继承、多继承情况)： https://blog.csdn.net/qq_36359022/article/details/81870219

多态一句话：在基类的函数前加上virtual关键字，在派生类中重写该函数，运行时将会根据对象的实际类型来调用相应的函数。如果对象类型是派生类，就调用派生类的函数；如果对象类型是基类，就调用基类的函数

纯虚函数是虚函数再加上=0； 

抽象类是指包括至少一个纯虚函数的类。

纯虚函数:virtual void fun()=0;即抽象类，必须在子类实现这个函数，即先有名称，没有内容，在派生类实现内容。
```cpp
Son son;
Father *pFather=&son; // 隐式类型转换
Print 
```
虚函数的时候： 

在构造函数中进行虚表的创建和虚表指针的初始化，在构造子类对象时，要先调用父类的构造函数，此时编译器只“看到了”父类，并不知道后面是否还有继承者，它初始化父类对象的虚表指针，该虚表指针指向父类的虚表，当执行子类的构造函数时，子类对象的虚表指针被初始化，指向自身的虚表。

pFather的虚表指针(即vptr)实际指向的对象类型是Son，因此vptr指向的Son类的vtable，当调用pFather->Son()时，根据虚表中的函数地址找到的就是Son类的Say()函数.

在构造函数中调用（纯）虚函数，不会有虚函数的效应

一个类只能有一个虚函数表，所有该类的实例化对象中都会有一个虚函数表指针去指向该类的虚函数表。

class ClassC : public ClassA1, public ClassA2

如果基类当中是虚函数，那么在派生类中就算不加 virtual 关键字修饰，派生类当中重写的函数依然是虚函数

C++ 多继承和虚继承的内存布局: https://blog.csdn.net/yockie/article/details/50603236

虚继承（虚基类） -——> 是解决菱形继承的问题
```cpp
class Left : virtual public Top
{
  public: int b;
};
```
### 6 类与对象

**构造函数成员变量初始化顺序**：

C++成员变量的初始化顺序问题： https://blog.csdn.net/zhaojinjia/article/details/8785912

1 初始化列表的初始化顺序是按照成员变量的**声明顺序**进行的，而不是按照书写顺序进行的，

2 如果要在派生类初始化同时给基类**传初始化参数**，那么基类的声明是**在任何派生类声明的变量之前**的，如果有传参依赖，要按照这个顺序执行，

3 变量的初始化顺序就应该是： **基类的静态变量或全局变量** --->  **派生类**的静态变量或全局变量 --> 基类的成员变量 --> 派生类的成员变量

当派生类中的成员变量和基类中同名，那么与函数同名一样，基类中的同名变量会被隐藏。也就是通过派生类对象无法访问基类的同名变量。

全局类对象的构造函数会在 main() 函数之前被执行

如果一个类没有声明**任何**构造函数，则编译器会自动生成一个缺省构造函数 //只有在没有任何构造 函数时才会自动生成

纯虚函数-->抽象类，不能定义具体对象，其纯虚函数的实现由派生类给出。

三种继承的方法: public继承/private继承/protected继承详解及区别： https://blog.csdn.net/bzhxuexi/article/details/17026149

子类可以访问父类的保护成员，子类的友元类可以通过子类对象去访问父类的保护成员

**内存布局**

浅析C++类的内存布局: https://zhuanlan.zhihu.com/p/380147337

A --> B --> C

public 继承/private继承/protected继承详解及区别： https://blog.csdn.net/bzhxuexi/article/details/17026149

public继承：可以访问public和protected的成员变量和成员函数 

友元： 是一个函数或者一个类。

类的友元函数： 定义在类外部，但有权访问类的所有私有（**private**）成员和保护（**protected**）成员，不是成员函数

**拷贝构造函数：**

如果一个派生类要用拷贝构造函数去拷贝（不止是新增的类成员，还要拷贝父类的成员）要用如下的拷贝方式：

```cpp
D::D(const D &other) :  B(other) //父类的成员变量拷贝只能用父类的拷贝构造函数 
{ 
 // 在这拷贝类D新增的成员 
} 
```
**函数重载**

指的是参数不同但是返回值要相同，名字相同，如果从父类继承下来的话，无论返回值还是参数有何不同，只要同名，父类的这个函数就会被隐藏掉，这样就不算重载了，只有同一个域下面写重载 函数才会实现重载。

注意继承是重写不是重载。

重载： 参数不同但是返回值要相同

重载函数如果没有找到最为匹配的会优先调用精度高于它的，但是如果精度比它高的有多于一个重载函数，那么会报二义性错误，这样就需要我们主动声明是要调用哪个函数。

精确匹配-->提升匹配-->使用标准转换匹配

编译系统生成的默认拷贝构造函数对内部数据做的都是浅拷贝（非常值成员变量），如果要拷贝要重新写拷贝构造函数进行深拷贝 ，需要新申请new内存。

**构造析构函数不被继承**

在整个层次中的所有的构造函数和析构函数都必须被调用，也就是说，构造函数和析构函数不能被继承。

子类的构造函数会显示的调用父类的构造函数或隐式的调用父类的默认的构造函数进行父类部分的初始化。

析构函数也一样。它们都是每个类都有的东西，如果能被继承，那就没有办法初始化了。

B继承A的public成员函数，可以重写继承到private中

只有虚函数才可能是动态联编的，对虚函数的调用不一定是动态联编。

静态联编，也称为编译时多态性，动态联编：在程序运行时才能确定调用哪个函数，也称为运行时多态性

如果你编写需要被实现为内联函数的函数模板，你仍然应该使用inline修饰符

**using**

using指令(directive, using namespace std)和using声明(using std::cout)

using声明的作用域更小，推荐使用

using定义类型的别名；

using uint = unsigned int; 

typedef unsigned int UINT; 

**静态成员变量**

静态成员变量调用方法只能是类名::变量名或者类对象.变量名 

静态成员变量初始化一般来说要在对应的.cpp 文件下用类名::变量名方式初始化

对静态成员函数来说，只能访问静态成员变量和静态成员函数。如果一定要访问类内的成员变量，可以将类指针作为参数传入

相比类成员函数来说，静态成员函数少一个this指针参数



