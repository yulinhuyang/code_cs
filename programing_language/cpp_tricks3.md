- 宏的问题：没有类型检查，运算符结合的时候，存在结合优先级

 

**头文件：**

头文件 ————> include了就是全局

 

 自引(号)他尖
项目组内部的头文件使用引号形式，对于标准库头文件、第三方库头文件、公司内部开发的平台组件或独立组件的头文件，使用尖括号形式

```
#include "Foo/Foo.h"             // cpp对应的头文件

#include <cstdlib>                // 标准库头文件
#include <string>

#include <linux/list.h>          // 系统库头文件
#include <linux/time.h>
#include <some_library/libxxx.h> // 其他库头文件

#include "project/public/Log.h"  // 本项目内其他头文件
```

**头文件应该自包含，不能重复包含：**

自包含就是任意一个头文件均可独立编译。如果在使用一个头文件时，还要包含另外一个头文件才能工作的话，会给使用这个头文件的用户增添不必要的负担。

另外，**还需要防止头文件重复包含**，团队可以选择使用**宏**或**#****pragma once**防止头文件重复包含。

```C++
// Foo.h
#ifndef TEST_FOO_H  // 防止重复包含
#define TEST_FOO_H
#include <iostream> // 满足自包含
inline void Foo()  
{
    ...
    std::cout << msg; // std::cout依赖iostream头文件
}
#endif
// test.cpp
#include "Foo.h" // 只需要包含Foo.h
int main()
{
    Foo();
    return 0;
}
```

 

使用命名空间管理源代码：
在namespace中定义全局变量、常量、函数和类型
• .cpp文件中不需要导出的符号，建议通过匿名namespace封装（也可以用static修饰），防止不必要的定义被暴露出去
• 不要在头文件中使用匿名namespace或static定义非外部可见符号
• 不要在头文件中或#include之前，使用using向全局命名空间导入符号，否则会影响后续代码，易造成符号冲突。



#### 2.8 标准库 

 优先使用 C++标准库

- 使用特殊成员函数或重载操作符替代memset/memset_s、memcpy/memcpy_s     、memcmp等操作类对象
- 使用C++提供的字符串库，替代C风格的字符串
- 使用C++提供的std::vector、std::array、std::span等替代原生数组
- 使用C++提供的std::format()、std::ostringstream等方法进行格式化，替代sprintf、snprintf/snprintf_s等C函数
- 使用C++提供的std::ifstream、std::ofstream、std::fstream完成文件IO操作，替代C语言FILE结构和相关函数
- 使用C++提供的多线程、原子变量、锁、条件变量等，替代POSIX     线程库
- 使用C++提供的数值库，替代C语言数值库

 

应使用新的cxxx形式的头文件。对于一些形式为xxx.h的C标准库头文件，在C++标准库中同时提供了同名的头文件和功能相同的新的cxxx形式的头文件

#include <cstdlib> // 符合：使用 <cstdlib> 而不是 <stdlib.h> 来包含C标准库的头文件



C风格字符串: \0结束符

 

**使用**std::string表示字符串**

**优点：**

- 不用考虑结尾的'\0'
- 可以直接使用+、=、<、>等操作符，并且提供丰富的字符串操作函数
- 不需要考虑内存分配操作，避免了显式的new/delete，以及由此导致的错误

**问题：**

- 将空指针传递给std::string构造函数，会解引用空指针，从而导致程序产生未定义行为
- std::string类型的**c_str**和**data**成员函数返回的指针，可能会随着修改对象而失效，在使用时需要确保指针的有效性

 

**使用**有效的迭代器

- 使用有效的迭代器和指向容器元素的指针与引用。如：std::**find**() 等算法可能返回无效位置；调用**erase**()、**insert**() 等方法修改容器时，已有的**迭代器也会失效**。
- 外部数据用于容器索引或迭代器时必须确保在有效范围内，需要进行合法性校验。

 

**erase-remove**惯用法

Erase-Remove 惯用法 : https://www.cnblogs.com/tianshihao/p/14378812.html

vector<int> v;

v.erase(remove(v.begin(), v.end(), 99), v.end());

**remove() 并不真正删除东西，因为它做不到**。remove() 移动指定区间中的所有元素直到所有“不删除的”元素在区间的开头（相对位置和原来的一样）。它返回一个指向最后一个“不删除的”元素的下一个元素的迭代器。返回值是区间的“新逻辑终点”。

 

**正确使用格式化输入/输出函数**

调用格式化输入/输出函数时，使用有效的格式字符串。并且格式串必须与对应的实参类型、数量做严格匹配。
Eg:

使用%ld这里用来打印int64_t类型，


调用格式化输入/输出函数时，禁止format参数受外部数据控制。应使用常量字符串作为格式串。

 

**防止命令注入问题：**

system()、popen()等函数会使用命令解析器去执行命令，如果这些函数的参数是外部可控的数据，会导致命令注入漏洞。

命令解析器支持命令分隔符, 管道符用于连续执行多个命令或程序。利用这个特性，当命令参数中含有外部精心构造的数据时，可以注入任意命令。

 

**禁止外部可控数据作为进程启动函数的参数或者作为dlopen/LoadLibrary等模块加载函数的参数**

- 可以使用POSIX的**exec**系列函数直接启动进程，但是不能使用shell启动。
- 若使用**system**()函数，则应使用硬编码的函数入参或对外部输入中的命令分隔符进行白名单过滤/转义。

 



**线程不安全函数/异步不安全函数：**

- C库中的线程不安全函数：**strtok**, **ctime**，**putenv**，…
- C标准库函数中只有**abort**()， _**Exit**()，**quick**_exit()和signal()函数可以在信号处理程序中安全的调用；

- 在信号处理程序中不能含有new或delete操作，不能抛出异常，不能捕获异常，…

 

**2.9** **继承自C语言的编程规范条款**

- 关于**宏定义**：**完备的括号**，**不以分号**结尾，**多条语句**的函数式宏放在 **do-while(0)** 中包装，**不依赖**宏外部的**局部变量**名

 

**变量**

- 避免使用大量栈分配，对于 x86 和 x64     计算机，默认堆栈大小为 1 MB。

- 当局部变量大小超过**1k****时**，要审视合理性；较大的类型作为函数参数时，应使用**引用或指针**

```
constexpr int MAX_BUFF 0x1000000

//过大，单位换算 **byte** --> **KB** --> **MB**

char buff[MAX_BUFF] = {0};

 
```

**控制语句**

- 循环必须安全退出，在退出前正确释放相关资源，避免资源泄漏。

**整数**

- 确保枚举常量映射到唯一值；如果： 类型的多个成员确实需要分配相同的值，应当提供**显式的整数赋值**。

```
enum class ErrorCode {

  SYS_ARG_ERR = 1,

  SYS_SET_ERR = 2,

  SYS_OCCUPIED_ERR = 3, 

  SYS_REQFREE_ERR = 4,

  

  // 显示指定与 SYS_OCCUPIED_ERR的值相同  

  SYS_NO_REC_ERR = 3,

  

  // 显示指定 SYS_REQFREE_ERR的值相同

  SYS_ACTIVED_ERR = 4, 

  SYS_NO_FILE_ERR = 5

};
```

**断言**

断言主要用于调试期间，在发布版本中应将其关闭。因此，断言应该用于防止不正确的程序员假设，而不能用在发布版本上检查程序运行过程中发生的错误。

**断言永远不应用于验证是否存在**运行时（与逻辑相对）错误，包括但不限于：

**函数使用**

禁止使用**realloc**()函数

禁止使用**alloca**()函数申请栈上内存

必须**检查**安全函数**返回值**，并进行正确的处理

正确设置安全函数中的**destMax**参数

禁止封装安全函数

禁止用**宏重命**名安全函数

只能使用华为安全函数库中的安全函数或经华为认可的其他安全函数

禁止直接使用外部数据拼接SQL命令

禁止使用内存操作类不安全函数

 

安全函数有： memcpy_s、memset_s、strcpy_s、sprintf_s、strcat_s、

都有destMax参数

errno_t memcpy_s(void *dest, size_t **destMax**, const void *src, size_t **count**);

**destMax**是dest的实际大小；

**count**是实际要拷贝内存的大小；

如果count>destMax会返回失败；

返回值：

 

150 – src为空

162 – count>destMax

 

**destMax**

安全函数拦截缓冲区写溢出的前提是正确设置destMax，否则达不到预期效果。

**dest为数组的情况：**

• destMax必须设置为 sizeof(dest) 或BUFF_SIZE*sizeof(elementType)。(elementType是数组元素的类型)

• 当数组元素为字节类型(如char, signed char, unsigned char)时，sizeof(elementType)为1，可以省略。

**dest为动态分配的堆内存的情况：**

• destMax设置为当初分配的内存大小，或剩余内存的大小。

**dest为变长struct结构中的变长成员变量：**

• destMax必须设置为该成员变量的实际大小，结构体中必须有描述变长部分长度的成员变量



```
#define BUFF_SIZE 100

unsigned char dest[BUFF_SIZE];

…

ret = memcpy_s(dest, sizeof(dest), src, srcLen);

…

ret = memcpy_s(dest, BUFF_SIZE, src, srcLen);

```


- 内存中的敏感信息使用完毕后立即清0


其他：

不要在信号处理函数中访问共享对象

禁用rand函数产生用于安全用途的伪随机数

禁止在发布版本中输出对象或函数的地址

禁止代码中包含公网地址

 

## 第二部分：C++笔记题库 

除子类，都会调用父类的析构函数

 

**类对象大小计算：**

12. 计算一个类对象的大小时的规律： 

1)空类、单一继承的空类、多重继承的空类所占空间大小为：1（字节，下同）；

2)一个类中，**虚函数本身、成员函数（包括静态与非静态）和静态数据成员都是不占用类对象的存储空间的**；

3)因此一个对象的大小 ≥ 所有**非 静态成员 大小的总和**；

4)当类中**声明了虚函数**（不管是 1 个还是多个），那么在实例化对象时，编译器会自动在对象里安 插一个指针 **vPtr** 指向虚函数表 **VTable**；

5)虚承继的情况：由于涉及到虚函数表和虚基表，会同时增加一个（多重虚继承下对应多个） **vfPtr** 指针指向**虚函数表 vfTable** 和一个 **vbPtr** 指针指向虚基表 **vbTable**，这两者所占的空间大小 为：**8**（或 **8 乘以多继承时父类的个数**）； 

6)在考虑以上内容所占空间的大小时，还要注意编译器下的“**补齐**”padding 的影响，即编译器会插 入多余的字节补齐； 

7)类对象的大小 = **各非静态数据成员（**包括父类的非静态数据成员但都不包括所有的成员函数）的总和+ **vfptr** 指针(多继承下可能不止一个) + **vbptr** 指针(多继承下可能不止一个) + 编译器额外增加的字节。

 

C++多态虚函数表详解(多重继承、多继承情况)： https://blog.csdn.net/qq_36359022/article/details/81870219

一个类只能有一个**虚函数表**，所有该类的**实例化对象**中都会有一个虚函数表指针去指向该类的虚函数表。

class ClassC : public ClassA1, public ClassA2



**多态构造与析构：**

**delete P**：析构父类，如果父类为虚，则先子类，再父类，否则只有父类。

p->Print():  print 非虚，仍然调用父类，print为虚，且被子类重写了，调用子类。本质和delete P一样的，非虚调父类，虚调子类。

C++ 多态的实现及原理： https://www.cnblogs.com/cxq0017/p/6074247.html

 

**虚函数**

多态一句话：在基类的函数前加上virtual关键字，在派生类中重写该函数，运行时将会根据对象的实际类型来调用相应的函数。如果对象类型是派生类，就调用派生类的函数；如果对象类型是基类，就调用基类的函数


多态用虚函数来实现，结合动态绑定. 

纯虚函数是虚函数再加上** **= 0**； 

抽象类是指包括至少一个纯虚函数的类。

纯虚函数:virtual void fun()=0;即抽象类，必须在**子类实现这个函数**，即先有名称，没有内容，在派生类实现内容。


Son son;

Father *pFather=&son; // 隐式类型转换

Print 

虚函数的时候： 

在构造函数中进行虚表的创建和虚表指针的初始化，在构造子类对象时，要先调用父类的构造函数，此时编译器只“看到了”父类，并不知道后面是否还有继承者，它初始化父类对象的虚表指针，该虚表指针指向父类的虚表，当执行子类的构造函数时，**子类对象的虚表指针被初始化**，指向自身的虚表。

pFather的虚表指针(即**vptr**)实际指向的对象类型是**Son**，因此vptr指向的Son类的vtable，当调用pFather->Son()时，根据虚表中的函数地址找到的就是Son类的Say()函数.



在构造函数中调用（纯）虚函数，不会有虚函数的效应

 

**继承**

子类可以访问父类的保护成员，子类的友元类可以通过子类对象去访问父类的保护成员

如果基类当中是虚函数，那么在派生类中就算不加 virtual 关键字修饰，派生类当中重写的函数依然是虚函数

C++ 多继承和虚继承的内存布局: https://blog.csdn.net/yockie/article/details/50603236

 

虚继承（虚基类） -——> 是解决菱形继承的问题

 

class Left : virtual public Top

{

  public: int b;

};

 

**内存布局**

浅析C++类的内存布局: https://zhuanlan.zhihu.com/p/380147337

A --> B --> C

public 继承/private继承/protected继承详解及区别： https://blog.csdn.net/bzhxuexi/article/details/17026149

public继承：可以访问public和protected的成员变量和成员函数 

友元： 是一个函数或者一个类。

类的友元函数： 定义在类外部，但有权访问类的所有私有（**private**）成员和保护（**protected**）成员，不是成员函数

 

**函数重载**

指的是参数不同但是返回值要相同，名字相同，如果从父类继承下来的话，无论返回值还是参数有何不同，只要同名，父类的这个函数就会被隐藏掉，这样就不算重载了，只有同一个域下面写重载 函数才会实现重载。

注意继承是重写不是重载。

重载： 参数不同但是返回值要相同

重载函数如果没有找到最为匹配的会优先调用精度高于它的，但是如果精度比它高的有多于一个重载函数，那么会报二义性错误，这样就需要我们主动声明是要调用哪个函数。

 

精确匹配-->提升匹配-->使用标准转换匹配

编译系统生成的默认拷贝构造函数对内部数据做的都是浅拷贝（非常值成员变量），如果要拷贝要重新写拷贝构造函数进行深拷贝 ，需要新申请new内存。

 

**派生类**

**拷贝构造函数：**

如果一个派生类要用拷贝构造函数去拷贝（不止是新增的类成员，还要拷贝父类的成员）要用如下的拷贝方式：

 

1 D::D(const D &other) :  B(other) // **父类的成员变量拷贝只能用父类的拷贝构造函数** 

2 { 

 // 在这拷贝类 **D 新增的成员** 

4 } 



**构造函数成员变量初始化顺序**：

C++成员变量的初始化顺序问题： https://blog.csdn.net/zhaojinjia/article/details/8785912

1 初始化列表的初始化顺序是按照成员变量的**声明顺序**进行的，而不是按照书写顺序进行的，

2 如果要在派生类初始化同时给基类**传初始化参数**，那么基类的声明是**在任何派生类声明的变量之前**的，如果有传参依赖，要按照这个顺序执行，

3 变量的初始化顺序就应该是： **基类**的静态变量或全局变量 --->  **派生类**的静态变量或全局变量 --> 基类的成员变量 --> 派生类的成员变量

 

当派生类中的成员变量和基类中同名，那么与函数同名一样，基类中的同名变量会被隐藏。也就是通过派生类对象无法访问基类的同名变量。

全局类对象的构造函数会在 main() 函数之前被执行

如果一个类没有声明**任何**构造函数，则编译器会自动生成一个缺省构造函数 //只有在没有任何构造 函数时才会自动生成

纯虚函数-->抽象类，不能定义具体对象，其纯虚函数的实现由派生类给出。

三种继承的方法: public继承/private继承/protected继承详解及区别： https://blog.csdn.net/bzhxuexi/article/details/17026149

 

**构造析构函数不被继承**

在整个层次中的所有的构造函数和析构函数都必须被调用，也就是说，构造函数和析构函数不能被继承。

子类的构造函数会显示的调用父类的构造函数或隐式的调用父类的默认的构造函数进行父类部分的初始化。

析构函数也一样。它们都是每个类都有的东西，如果能被继承，那就没有办法初始化了。

 

B继承A的public成员函数，可以重写继承到private中



只有虚函数才可能是动态联编的，对虚函数的调用不一定是动态联编。

静态联编，也称为编译时多态性，动态联编：在程序运行时才能确定调用哪个函数，也称为运行时多态性

 
如果你编写需要被实现为内联函数的函数模板，你仍然应该使用inline修饰符

 

**using指令**(directive, using namespace std) 和 **using声明(**using std::cout)

using声明的作用域更小，推荐使用

**using定义类型的别名**

using uint = unsigned int; 

typedef unsigned int UINT; 

 

静态成员变量调用方法只能是 **类名::变量名** 或者 **类对象.变量名**

静态成员变量初始化一般来说要在对应的 .cpp 文件下用 **类名::变量名** 方式初始化

对静态成员函数来说，只能访问**静态成员变量**和静态**成员**函数。如果一定要访问类内的成员变量，可以将类指针作为参数传入

相比类成员函数来说，静态成员函数少一个 **this** **指针参数**

 

全局变量的使用h中**extern**声明，**cpp**中定义

res.h

extern char g_szBuffer[]; *// 环形缓冲区*

 

res.cpp

char g_szBuffer[g_nBufferSize];

 

Const: 从右向左读

顶层const:指针本身是个常量

底层const: 指针所指对象是一个常量


右值引用：只能绑定一个将销毁的对象，&&

变量是左值


**new**:  1  operator new分配内存    2  编译器运行相应的构造函数以构造这些对象   3  对象被分配了空间并构造完成，返回一个指向该对象的指针

**delete**: 1  对sp所指对象或者arr所指的对象数组中的元素执行对应的析构操作    2  编译器调用operator delete的标准库函数释放内存空间



**函数指针与数组指针**

c语言的函数指针和void *指向函数： https://blog.csdn.net/HES_C/article/details/81944100

函数指针： 调用函数和做函数的参数



int (***fp**)(int a);      // 一个返回值为 int 类型的函数指针 fp 

void (*fun_1)(void);  // 函数指针，参数为空

 

函数指针可以用 **0** 来初始化或者赋值，以表示该**指针不指向任何**函数

区别： int * fp(int a )  //函数返回值是int *

 

数组指针

int a[5],*pa;

则p=**a**，或p=**&a[0]**都可以

 

利用=运算符来把字符串拷贝到已定义的字符数组中是不可能的：


Sizeof 数组，整个数组大小。

Sizeof 指针，指针本身所占空间。

数组传参数时候，地址传递，相当于指针了。



现代C++语言（C++11/14/17）特性总结和使用建议： https://blog.csdn.net/qq_36631379/article/details/110728643

 


**override和**final成员函数

重载：是参数不同但是返回值要相同；参数相同，但是基类的函数是const的，派生类的函数却不是

 
override，表示函数应当重写**基类**中的虚函数。

final，表示**派生类**不应当重写这个虚函数。



**追踪返回类型**

template <typename T1, typename T2>
 auto Sum(T1 & t1, T2 & t2) -> decltype(t1 + t2) {
   return t1 + t2;
 }

auto占位符 + ->return_type也就是构成追踪返回类型函数的2个基本元素

 

lambda表达式:  [capture list] (parameter list) -> return type { function body }
vector<int> iv{5, 4, 3, 2, 1};  
int a = 2, b = 1;  
for_each(iv.begin(), iv.end(), [b](int &x){cout << (x + b) << endl;}); // (1)  
for_each(iv.begin(), iv.end(), [=](int &x){x *= (a + b);});     // (2)  
for_each(iv.begin(), iv.end(), [=](int &x)->int{return x * (a + b);});// (3) 

 

[]中传入=的话，即是可以取得所有的外部变量

**成员函数控制：=delete和=default**

=default，从而显式的指示编译器生成该函数的默认版本，为了POD类型，自定义后仍然要是生成默认的版本的

=delete会指示编译器不生成函数的缺省版本。如禁止使用者使用拷贝构造函数

POD 是Plain Old Data 的

 



**右值引用(&&)*、移动构造、移动赋值和完美转发**

 

左值就是一个有名字的对象，而右值则是一个无名对象（临时对象）

右值 + move ，移动构造move constructor，移动赋值move assignment operator

 
完美转发（perfect forwarding），指的是在函数模板中，完全依照模板的参数的类型，将**参数**传递给函数模板中调用的**另外一个函数**



当int和unsigned in相加时，要将int转化为unsigned int

unsigned int与int相加的问题 https://blog.csdn.net/thefutureisour/article/details/8147277  

 

unique_ptr是C++11之后才出现的

**uintptr_t** ：将指针转换为整数时, 可以使用 () 类型存放转换后的指针值


打印it迭代器所处的位置： std::cout << (it - myStr.begin());


关于二维数组访问效率问题： 

在C/C++语言中，数组在内存中是**按行存储**的，按行遍历时可以由指向数组第一个数的指针一直向后遍历，由于二维数组的内存地址是连续的，当前行的尾与下一行的头相邻

**行遍历快**： a[i][j] += a[i][j];

 

**默认初始化**： 

如果不明确指出初始化列表，那么基本类型是不会被初始化的（除全局变量和静态变量外），所有的内存都是“脏的”；而类类型则会为每个元素调用默认构造函数进行初始化。

全局, 静态变量,一般默认为0; 而局部变量处于堆栈区，其数值是随机的，即当时内存中的值。

 

宏定义： 简单的文本替换，(M+1)*M/2  -> NUM=(N+1+1)* **N+1** /2 = 8


**shared_ptr的use_count** 

shared_ptr的使用和陷阱  https://blog.csdn.net/qq_33266987/article/details/78784852


多个共享指针指向同一个空间，它们的关系可能是关联（我们所期望的正常关系）或是独立的（一种错误状态）

只有用一个shared_ptr为另一个shared_ptr赋值时，才将这连个共享指针关联起来，直接使用地址值会导致各个shared_ptr独立。

 
1. shared_ptr<int> sp1(new int(10));
2. shared_ptr<int> sp2(sp1), sp3;
3. sp3 = sp1;


1. *//一个典型的错误用法*

1. shared_ptr<int> sp4(sp1.get()); 
 

sp1,sp2,sp3是相互关联的共享指针，共同控制所指内存的生存期，sp4是独立的

 



**inline仅是一个对编译器的建议，写了编译器不一定展开，不写inline编译器也会根据实际情况来决定是否展开。**

C++中inline用法详解 https://segmentfault.com/a/1190000019767044

 

**auto**

auto一般会忽略掉顶层const（指针本身是个常量），同时底层const会保留下来 

顶层const：对象本身是个常量；

底层const：指针所指的对象是个常量、用于声明引用的const都是底层const；



**线程同步**

std::**promise** 对象禁止拷贝

std::**shared**_**future** 对象可以拷贝



**GDB**：

set args 可指定运行时参数。（如：set args 10 20 30 40 50） show args 命令可以查看设置好的运行参数。

start命令在main函数入口设置一个临时断点


Watch <expr> 设置一个观察点。一量表达式值有变化时，马上停住程序


**数组指针别名**

typedef int (*Array)[10];

using Array = int (*)[10];



**命名空间**

在.h文件自定义命名空间使用using声明类型

在.cpp文件中，所有有文件include完成后，使用using声明类型



**range-based-for**

 C++11新特性之基本范围的For循环（range-based-for）：https://blog.csdn.net/hailong0715/article/details/54172848

 基于范围的FOR循环的遍历是**只读的遍历**，除非将变量变量的类型声明为引用类型。

 

大多数运算符结合性是从左到右，只有三个优先级是从右至左结合，单目运算符（算术、逻辑）、条件运算符、赋值运算符



**std::move**

vec.push_back(**str1**);  // 传统方法，copy 

vec.push_back(std::**move**(str1)); // 调用移动语义的push_back方法，避免拷贝，str1会失去原有值，变成空字符串 

 

**匿名namespace**

对于cpp文件中不需要导出的变量、常量或函数，应使用匿名namespace封装或者用static修饰，推荐使用匿名namespace

 

noexcept声明

delete操作符、移动构造函数、移动赋值操作符、swap函数应该有noexcept声明

 

对比：

sizeof(vector) 获取数据类型的,vector::size()和vector::capacity()

 

需要用**左值引用**的方法捕获异常

try… catch(std::exception**& e**)…



重载可以让 ‘>>’ 将数据输入自定义类的成员，cin >> obj

friend istream& operator >> (istream& id, MyClass& a)

 

传参为&时才会触发虚函数继承

 

父类虚函数声明了**noexcept**,子类重写的函数不默认就是noexcept，需要显式加上noexcept

有C++11限制，选make_shared，没有选make_unique(C++ 14）

int (*p[10])(int*) 声明了 p 是一个元素为**函数指针**的数组，数组元素都是 **int** (***p**)(**int** *)的函数指针

int *p[10]   ---> 数组，元素是**int \***， []的优先级高于*

int (*p)[10]  ---> 指向一个 10 列数组的指针，元素是int

 

Lambda引用捕获会根据值作变化更新   https://blog.csdn.net/m0_48990191/article/details/114519589



**Atomic 函数**
std::atomic::store(val） ，用val替换包含的值
std::atomic::load( )，返回包含值

 

禁止用断言检测程序在运行期间可能导致的错误，可能发生的错误要用错误处理代码来处理

赋值（ = ）、索引（ [] ）、调用（ () ）、成员访问箭头（ -> ）操作符必须是成员函数



重载结构体： 

Func(**Type2Type**<**int**>())

 

std::tie会将变量的引用整合成一个tuple



const char *p ：指向的值不能变

char *const p ： P不能变 

 

cout打印\0的问题

cout<<**"alpha****\0****beta"**<<endl;
