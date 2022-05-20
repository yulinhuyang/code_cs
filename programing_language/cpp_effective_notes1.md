参考：

Scott Meyers大师Effective三部曲：Effective C++、More Effective C++、Effective STL，这三本书出版已很多年，后来又出版了Effective Modern C++。

Effective C++改善程序与设计的55个具体做法笔记： https://blog.csdn.net/fengbingchun/article/details/102761542

More Effective C++的笔记见：https://blog.csdn.net/fengbingchun/article/details/102990753

Effective STL的笔记见：https://blog.csdn.net/fengbingchun/article/details/103223914

Effective Modern C++的笔记见：https://blog.csdn.net/fengbingchun/article/details/104136592

# 目录

参考：

Effective C++改善程序与设计的55个具体做法笔记： https://blog.csdn.net/fengbingchun/article/details/102761542      
More Effective C++35个改善编程与设计的有效方法笔记：https://blog.csdn.net/fengbingchun/article/details/102990753   
Effective STL的笔记： https://blog.csdn.net/fengbingchun/article/details/103223914   
Effective Modern C++42招独家技巧助你改善C++11和C++14的高效用法笔记：https://blog.csdn.net/fengbingchun/article/details/104136592   


# 目录

## 1 Effective_C++

**1  让自己习惯C++(Accustoming Yourself to C++)**

Item 1 视C++为一个语言联邦(View C++ as a federation of languages)   
Item 2 尽量以const, enum, inline替换#define(Prefer consts, enums, and inlines to #defines)   
Item 3 尽可能使用const(Use const whenever possible)   
Item 4 确定对象被使用前已先被初始化(Make sure that objects are initialized before they’re used)   
  
**2  构造/析构/赋值运算(Constructors, Destructors, and Assignment Operators)**
  
Item 5 了解C++默默编写并调用哪些函数(Know what functions C++ silently writes and calls)      
Item 6 若不想使用编译器自动生成的函数，就该明确拒绝(Explicitly disallow the use of compiler-generated functions you do not want)   
Item 7 为多态基类声明virtual析构函数(Declare destructors virtual in polymorphic base classes)   
Item 8 别让异常逃离析构函数(Prevent exceptions from leaving destructors)   
Item 9 绝不在构造和析构过程中调用virtual函数(Never call virtual functions during construction or destruction)   
Item 10 令operator=返回一个reference to *this(Have assignment operators return a reference to *this)   
Item 11 在operator=中处理”自我赋值”(Handle assignment to self in operator=)   
Item 12 复制对象时勿忘其每一个成分(Copy all parts of an object)    

**3  资源管理(Resource Management)**
 
Item 13 以对象管理资源(Use objects to manage resources)   
Item 14 在资源管理类中小心copying行为(Think carefully about copying behavior in resource-managing classes)   
Item 15 在资源管理类中提供对原始资源的访问(Provide access to raw resources in resource-managing classes)   
Item 16 成对使用new和delete时要采用相同形式(Use the same form in corresponding uses of new and delete)   
Item 17 以独立语句将newed对象置入智能指针(Store newed objects in smart pointers in standalone statements)   
 
**4  设计与声明(Designs and Declarations)**

Item 18 让接口容易被正确使用，不易被误用(Make interface easy to use correctly and hard to use incorrectly)   
Item 19 设计class犹如设计type(Treat class design as type design)   
Item 20 宁以pass-by-reference-to-const替换pass-by-value(Prefer pass-by-reference-to-const to pass-by-value)   
Item 21 必须返回对象时，别妄想返回其reference(Don’t try to return a reference when you must return an object)   
Item 22 将成员变量声明为private(Declare data members private)   
Item 23 宁以non-member、non-friend替换member函数(Prefer non-member non-friend functions to member functions)   
Item 24 若所有参数皆需类型转换，请为此采用non-member函数(Declare non-member functions when type conversions should apply to all parameters)   
Item 25 考虑写出一个不抛异常的swap函数(Consider support for a non-throwing swap)   

**5 实现(Implementations)**
 
Item 26 尽可能延后变量定义式的出现时间(Postpone variable definitions as long as possible)   
Item 27 尽量少做转型动作(Minimize casting)   
Item 28 避免返回handles指向对象内部成分(Avoid returning “handles” to object internals)   
Item 29 为”异常安全”而努力是值得的(Strive for exception-safe code)   
Item 30 透彻了解inlining的里里外外(Understand the ins and outs of inlining)   
Item 31 将文件间的编译依存关系降至最低(Minimize compilation dependencies between files)     

**6  继承与面向对象设计(Inheritance and Object-Oriented Design)**
 
Item 32 确定你的public继承塑模出is-a关系(Make sure public inheritance models “is-a”)   
Item 33 避免遮挡继承而来的名称(Avoid hiding inherited names)   
Item 34 区分接口继承和实现继承(Differentiate between inheritance of interface and inheritance of implementation)   
Item 35 考虑virtual函数以外的其它选择(Consider alternatives to virtual functions)   
Item 36 绝不重新定义继承而来的non-virtual函数(Never redefine an inherited non-virtual function)   
Item 37 绝不重新定义继承而来的缺省参数值(Never redefine a function’s inherited default parameter value)       
Item 38 通过复合塑模出has-a或”根据某物实现出”(Model “has-a” or “is-implemented-in-terms-of” through composition)       
Item 39 明智而审慎地使用private继承(Use private inheritance judiciously)       
Item 40 明智而审慎地使用多重继承(Use multiple inheritance judiciously)       


**7  模板与泛型编程(Templates and Generic Programming)** 

Item 41 了解隐式接口和编译期多态(Understand implicit interfaces and compile-time polymorphism)   
Item 42 了解typename的双重意义(Understand the two meanings of typename)   
Item 43 学习处理模板化基类内的名称(Know how to access names in templatized base classes)   
Item 44 将与参数无关的代码抽离templates(Factor parameter-independent code out of templates)   
Item 45 运用成员函数模板接受所有兼容类型(Use member function templates to accept “all compatible types”)   
Item 46 需要类型转换时请为模板定义非成员函数(Define non-member functions inside templates when type conversions are desired)   
Item 47 请使用traits classes表现类型信息(Use traits classes for information about types)   
Item 48 认识template元编程(Be aware of template metaprogramming)       

**8  定制new和delete(Customizing new and delete)**

Item 49 了解new-handler的行为(Understand the behavior of the new-handler)   
Item 50 了解new和delete的合理替换时机(Understand when it makes sense to replace new and delete)   
Item 51 编写new和delete时需固守常规(Adhere to convention when writing new and delete)   
Item 52 写了placement new也要写placement delete(Write placement delete if you write placement new)   

**9 杂项讨论(Miscellany)**

Item 53 不要轻忽编译器的警告(Pay attention to compiler warnings)   
Item 54 让自己熟悉包括RT1在内的标准程序库(Familiarize yourself with the standard library, including TR1)   
Item 55 让自己熟悉Boost(Familiarize yourself with Boost)      


## 2 more effective C++

**1 基础议题（Basics）** 

Item 1 指针与引用的区别   
Item 2 尽量使用C++风格的类型转换   
Item 3 不要对数组使用多态   
Item 4 避免无用的缺省构造函数     

**2 操作符（Operators）** 

Item 5 谨慎定义类型转换函数   
Item 6 自增(increment)、自减(decrement)操作符前缀形式与后缀形式的区别   
Item 7 不要重载”&&”, “||”,或”,”   
Item 8 理解各种不同含义的new和delete       

**3 异常（Exceptions）** 

Item 9 使用析构函数防止资源泄漏   
Item 10 在构造函数中防止资源泄漏   
Item 11 禁止异常信息(exceptions)传递到析构函数外   
Item 12 理解”抛出一个异常”与”传递一个参数”或”调用一个虚函数”间的差异   
Item 13 通过引用(reference)捕获异常   
Item 14 审慎使用异常规格(exception specifications)   
Item 15 了解异常处理的系统开销   


**4 效率（Efficiency）** 

Item 16 牢记80-20准则(80-20 rule)   
Item 17 考虑使用lazy evaluation(懒惰计算法)   
Item 18 分期摊还期望的计算   
Item 19 理解临时对象的来源   
Item 20 协助完成返回值优化   
Item 21 通过重载避免隐式类型转换   
Item 22 考虑用运算符的赋值形式(op=)取代其单独形式(op)   
Item 23 考虑变更程序库   
Item 24 理解虚拟函数、多继承、虚基类和RTTI所需的代码      

**5 技术（Techniques， Idioms， Patterns）** 

Item 25 将构造函数和非成员函数虚拟化   
Item 26 限制某个类所能产生的对象数量   
Item 27 要求或禁止在堆中产生对象   
Item 28 灵巧(smart)指针   
Item 29 引用计数   
Item 30 代理类   
Item 31 让函数根据一个以上的对象来决定怎么虚拟       

**6 杂项讨论（Miscellany）**
 
Item 32 在未来时态下开发程序   
Item 33 将非尾端类设计为抽象类   
Item 34 如何在同一程序中混合使用C++和C   
Item 35 让自己习惯使用标准C++语言   


## 3 effective STL

**1 容器**

Item 1: 仔细选择你要的容器   
Item 2: 小心对“容器无关代码”的幻想   
Item 3: 使容器里对象的拷贝操作轻量而正确   
Item 4: 用empty来代替检查size是否为0   
Item 5: 尽量使用范围成员函数代替他们的单元素兄弟   
Item 6: 警惕C++的及其令人恼怒的分析   
Item 7: 当使用new得指针的容器时，切记在容器销毁前delete那些指针   
Item 8: 千万不要把auto_ptr放入容器中   
Item 9: 小心选择删除选项   
Item 10: 当心allocator的协定和约束   
Item 11: 了解自定义allocator的正统使用法   
Item 12: 对STL容器的线程安全性的期待现实一些   
 
**2 vector和string**

Item 13: 尽量使用vector和string来代替动态申请的数组   
Item 14: 用reserve来避免不必要的内存重新分配   
Item 15: 当心string的实现中的变化   
Item 16: 如何将vector和string的数据传给传统的API   
Item 17: 用“交换技巧”来修正过度的容量   
Item 18: 避免使用vector<bool>   
 
**3 关联容器**

Item 19: 了解相等和等价的区别   
Item 20: 为包含指针的关联容器指定比较类型   
Item 21: 永远让比较函数对相等的值返回false   
Item 22: 避免对set和multiset的键值进行修改   
Item 23: 考虑用排序的vector代替关联容器   
Item 24: 当效率很关键时尽量用map::insert代替map::operator   
Item 25: 让自己熟悉非标准的hash容器   
 
**4 迭代器**

Item 26: 尽量使用iterator代替const_iterator，reverse_iterator和const_reverse_iterator   
Item 27: 使用distance和advance把const_iterators转化成iterators   
Item 28: 了解如何通过reverse_iterator的base得到iterator   
Item 29: 需要一字符一字符输入时请用istreambuf_iterator   

**5 算法**

Item 30: 确保目的范围足够大   
Item 31: 了解你的排序选项   
Item 32: 如果你真的想删除东西的话在remove-like的算法后紧接上erase   
Item 33: 当心在包含指针的容器使用remove-like的算法   
Item 34: 注意哪些算法需要排序过的范围   
Item 35: 通过mismatch或lexicographical_compare实现简单的忽略大小写字符串比较   
Item 36: 用not1和remove_copy_if来表现copy_if   
Item 37: 用accumulate或for_each来统计序列   
 
**6 仿函数，仿函数类，函数等等**

Item 38: 把仿函数类设计成值传递的   
Item 39: 用纯函数做predicate   
Item 40: 增强仿函数类的适应性   
Item 41: 明确ptr_fun, mem_fun和mem_fun_ref的区别   
Item 42: 保证less是operator<的意思   
 
**7 用STL编程**

Item 43: 尽量用算法调用代替手写循环   
Item 44: 尽量用成员函数代替同名的算法   
Item 45: 注意count、find、binary_search、lower_bound、upper_bound和equal_range的区别   
Item 46: 考虑用函数对象代替函数作为算法的参数   
Item 47: 避免产生只写代码   
Item 48: 总是#include适当的头文件   
Item 49: 学会破解STL相关的编译器出错信息   
Item 50: 让自己熟悉STL相关的网站   


## 4 Effective Modern C++

**1 型别推导** 

Item 1 理解模板型别推导(Understand template type deduction)   
Item 2 理解auto型别推导(Understand auto type deduction)   
Item 3 理解decltype(Understand decltype)模板   
Item 4 掌握查看型别推导结果的方法(Know how to view deduced types)   

**2 auto**

Item 5 优先选用auto，而非显式型别声明(Prefer auto to explicit type declarations)   
Item 6 当auto推导的型别不符合要求时，使用带显式型别的初始化物习惯用法(Use the explicitly typed initializer idiom when auto deduces undesired types)   

**3 转向现代C++** 

Item 7 在创建对象时注意区分()和{}(Distinguish between()and{}when creating objects)   
Item 8 优先选用nullptr，而非0或NULL(Prefer nullptr to 0 and NULL)   
Item 9 优先选用别名声明，而非typedef(Prefer alias declarations to typedefs)   
Item 10 优先选用限定作用域的枚举型别，而非不限作用域的枚举型别(Prefer scoped enums to unscoped enums)   
Item 11 优先选用删除函数，而非private未定义函数(Prefer deleted functions to private undefined ones)   
Item 12 为意在改写的函数添加override声明(Declare overriding functions override)   
Item 13 优先选用const_iterator，而非iterator(Prefer const_iterators to iterators)   
Item 14 只要函数不会发射异常，就为其加上noexcept声明(Declare functions noexcept if they won’t emit exceptions)   
Item 15 只要有可能使用constexpr，就使用它(Use constexpr whenever possible)   
Item 16 保证const成员函数的线程安全性(Make const member functions thread safe)   
Item 17 理解特种成员函数的生成机制(Understand special member function generation)   

**4 智能指针** 

Item 18 使用std::unique_ptr管理具备专属所有权的资源(Use std::unique_ptr for exclusive-ownership resource management)   
Item 19 使用std::shared_ptr管理具备共享所有权的资源(Use std::shared_ptr for shared-ownership resource management)   
Item 20 对于类似std::shared_ptr但有可能空悬的指针使用std::weak_ptr(Use std::weak_ptr for std::shared_ptr-like pointers than can dangle)   
Item 21 优先选用std::make_unique和std::make_shared，而非直接使用new(Prefer std::make_unique and std::make_shared to direct use of new)   
Item 22 使用Pimpl习惯用法时，将特殊成员函数的定义放到实现文件中(When using the Pimpl Idiom, define special member functions in the implementation file)   

**5 右值引用、移动语义和完美转发** 

Item 23 理解std::move和std::forward(Understand std::move and std::forward)   
Item 24 区分万能引用和右值引用(Distinguish universal references from rvalue references)   
Item 25 针对右值引用实施std::move，针对万能引用实施std::forward(Use std::move on rvalue references, std::forward on universal references)   
Item 26 避免依万能引用型别进行重载(Avoid overloading on universal references)   
Item 27 熟悉依万能引用型别进行重载的替代方案(Familiarize yourself with alternatives to overloading on universal references)   
Item 28 理解引用折叠(Understand reference collapsing)   
Item 29 假定移动操作不存在、成本高、未使用(Assume that move operations are not present, not cheap, and not used)   
Item 30 熟悉完美转发的失败情形(Familiarize yourself with prefect forwarding failure cases)   

**6 lambda表达式**

Item 31 避免默认捕获模式(Avoid default capture modes)   
Item 32 使用初始化捕获将对象移入闭包(Use init capture to move objects into closures)   
Item 33 对auto&&型别的形参使用decltype，以std::forward之(Use decltype on auto&& parameters to std::forward them)   
Item 34 优先选用lambda式，而非std::bind(Prefer lambdas to std::bind)   

**7 并发API** 

Item 35 优先选用基于任务而非基于线程的程序设计(Prefer task-based programming to thread-based)   
Item 36 如果异步是必要的，则指定std::launch::async(Specify std::launch::async if asynchronicity is essential)   
Item 37 使std::thread型别对象在所有路径皆不可联结(Make std::threads unjoinable on all paths)   
Item 38 对变化多端的线程句柄析构函数行为保持关注(Be aware of varying thread handle destructor behavior)   
Item 39 考虑针对一次性事件通信使用以void为模板型别实参的期值(Consider void futures for one-shot event communication)   
Item 40 对并发使用std::atomic，对特种内存使用volatile(Use std::atomic for concurrency, volatile for special memory)   

**8 微调** 

Item 41 针对可复制的形参，在移动成本低并且一定会被复制的前提下，才考虑将其按值传递(Consider pass by value for copyable parameters that are cheap to move and always copied)   
Item 42 考虑置入而非插入(Consider emplacement instead of insertion)   



# 1 Effective C++的笔记：

## 1  让自己习惯C++(Accustoming Yourself to C++)

### Item 1. 视C++为一个语言联邦(View C++ as a federation of languages)

最简单的方法将C++视为一个由相关语言组成的联邦而非单一语言。在其某个次语言(sublanguage)中，各种守则与通例都倾向于简单、直观易懂、并且容易记住。总共只有4个次语言：

(1). C：说到底C++仍是以C为基础。区块(blocks)、语句(statements)、预处理(preprocessor)、内置数据类型(build-in data types)、数组(arrays)、指针(pointers)等统统来自C。许多时候C++对问题的解法不过就是较高级的C解法。

(2). Object-Oriented C++：classes(包括构造函数和析构函数)，封装(encapsulation)、继承(inheritance)、多态(polymorphism)、virtual函数(动态绑定)…等等。

(3). Tesplate C++：这是C++的泛型编程(generic programming)部分。实际上由于templates威力强大，它们带来崭新的编程范型(programming paradigm)，也就是所谓的template metaprogramming(TMP，模板元编程)。

(4). STL：是个template程序库。它对容器(containers)、迭代器(iterators)、算法(algorithms)以及函数对象(function objects)的规约有极佳的紧密配合与协调。

对内置(也就是C-like)类型而言pass-by-value通常比pass-by-reference高效，但当你从C part of C++移往Object-Oriented C++，由于用户自定义(user-defined)构造函数和析构函数的存在，pass-by-reference-to-const往往更好。运用Template C++时尤其如此，因此此时你甚至不知道所处理的对象的类型。

**请记住：C++高效编程守则视状况而变化，取决于你使用C++的哪一部分。**

### Item 2. 尽量以const, enum, inline替换#define(Prefer consts, enums, and inlines to #defines)

```cpp
class GamePlayer {
private:
	static const int NumTurns = 5; // 常量声明式
	int scores[NumTurns]; // 使用该常量
};

// 注意：由于class常量已在声明时获得初值(设初值为5),因此定义时不可以再设初值
const int GamePlayer::NumTurns; // NumTurns的定义

void f(int) {}
#define CALL_WITH_MAX(a, b) f((a) > (b) ? (a) : (b)) // 不推荐
template<typename T>
inline void callWithMax(const T& a, const T& b) // 推荐
{
	f(a > b ? a : b);
}

int test_item_2()
{
	#define ASPECT_RATIO 1.653 // 不推荐
	const double AspectRatio = 1.653; // 推荐

	{const char* const authorName = "Scott Meyers";} // 推荐
	const std::string autorName("Scott Meyers"); // 更推荐
	 
	return 0;
}
```

这个条款或许改为”宁可以编译器替换预处理器”比较好，因为或许#define不被视为语言的一部分。

对浮点常量(floating point constant)而言，使用常量可能比使用#define导致较小量的码，因为预处理器”盲目地将名称ASPECT_RATIO替换为1.653”可能导致目标码(object code)出现多份1.653，若改用常量AspectRatio绝不会出现相同情况。

以常量替换#define，有两种特殊情况：

第一种是定义常量指针(constant pointers)，由于常量定义式通常被放在头文件内(以便被不同的源码含入)，因此有必要将指针(而不只是指针所指之物)声明为const。例如若要在头文件内定义一个常量的(不变的)char*-based字符串，你必须写const两次。string对象通常比其前辈char*-base合宜。

第二种是class专属长量。为了将常量的作用域(scope)限制于class内，你必须让它成为class的一个成员(member)；而为确保此常量至多只有一份实体，你必须让它成为一个static成员。

我们无法利用#define创建一个class专属常量，因为#define并不重视作用域(scope)。一旦宏被定义，它就在其后的编译过程中有效(除非在某处被#undef)。这意味#define不仅不能够用来定义class专属常量，也不能够提供任何封装性，也就是说没有所谓private #define这样的东西。而当然const成员变量是可以被封装的，NumTurns就是。

另一个常见的#define误用情况是以它实现宏(macro)。宏看起来像函数，但不会招致函数调用(function call)带来的额外开销。无论何时你写成这种宏，你必须记住为宏中的所有实参加上小括号。

**请记住：(1). 对于单纯常量，最好以const对象或enum替换#define；(2). 对于形式函数的宏(macro)，最好改用inline函数替换#define。**

### Item 3. 尽可能使用const(Use const whenever possible)

```cpp
class TextBlock {
public:
	TextBlock(const std::string& str) { text = str; }
	// 两个成员函数如果只是常量性(constness)不同，可以被重载
	const char& operator[] (std::size_t position) const // operator[] for const对象
	{ return text[position]; }
	//char& operator[] (std::size_t position) // operator[] for non-const对象
	//{ return text[position]; }
	// 在const和non-const成员函数中避免重复
	char& operator[] (std::size_t position) // 现在只调用const op[]
	{ return const_cast<char&>( // 将const op[]返回值的const移除
		static_cast<const TextBlock&>(*this) // 为*this加上const，调用const op[]
		[position]); }

private:
	std::string text;
};

void print(const TextBlock& ctb) // ctb是const
{
	std::cout<< ctb[0]; // 调用const TextBlock::operator []
}

int test_item_3()
{
	char greeting[] = "Hello";
	{char* p = greeting;} // non-const pointer, non-const data
	{const char* p = greeting;} // non-const pointer, const data
	{char* const p = greeting;} // const pointer, non-const data
	{const char* const p = greeting;} //const pointer, const data

	std::vector<int> vec(1);
	const std::vector<int>::iterator iter = vec.begin(); // iter的作用像个T* const
	*iter = 10; // 没问题，改变iter所指物
	//++iter; // 错误，iter是const
	 
	std::vector<int>::const_iterator cIter = vec.begin(); // cIter的作用像个const T*
	//*cIter = 10; // 错误，*cIter是const
	++cIter; // 没问题，改变cIter
	 
	TextBlock tb("Hello");
	std::cout<< tb[0]; // 调用non-const TextBlock::operator []
	const TextBlock ctb("World");
	std::cout<< ctb[0]; // 调用const TextBlock::operator []
	print(ctb);
	print(tb);
	 
	return 0;
}
```

你可以用它在classes外边修饰global或namespace作用域中的常量，或修饰文件、函数、或区块作用域(block scope)中被声明为static的对象。你也可以用它修饰classes内部的static和non-static成员变量。面对指针，你也可以指出指针自身、指针所指物，或两者都(或都不)是const。

**如果关键字const出现在星号左边，表示被指物是常量；如果出现在星号右边，表示指针自身是常量；如果出现在星号两边，表示被指物和指针两者都是常量。**

**STL迭代器系以指针为根据塑模出来，所以迭代器的作用就像个T*指针。声明迭代器为const就像声明指针为const一样(即声明一个T* const指针)，表示这个迭代器不得指向不同的东西，但它所指的东西的值是可以改动的。如果你希望迭代器所指的东西不可被改动(即希望STL模拟一个const T*指针)，你需要的是const_iterator**。

两个成员函数如果只是常量性(constness)不同，可以被重载。

真实程序中const对象大多用于passed by pointer-to-const或passed by reference-to-const的传递结果。

const成员函数不可以更改对象内任何non-static成员变量，称为bitwise const。

令函数返回一个常量值，往往可以降低因客户错误而造成的意外，而又不至于放弃安全性和高效性。可以预防那个”没意思的赋值动作”。

mutable(可变的)可释放掉non-static成员变量的bitwise constness约束。

在const和non-const成员函数中避免重复：转型(casting)。只能non-const成员函数调用const成员函数，不能const成员函数调用non-const成员函数。

**请记住：(1).将某些东西声明为const可帮助编译器侦测出错误用法。const可被施加于任何作用域内的对象、函数参数、函数返回类型、成员函数本体。(2).编译器强制实施bitwise constness。(3).当const和non-const成员函数有着实质等价的实现时，令non-const版本调用const版本可避免代码重复。**

### Item 4. 确定对象被使用前已先被初始化(Make sure that objects are initialized before they’re used.)

```cpp
class ABEntry {
public:
	ABEntry(const std::string& name, const std::string& address);
	ABEntry(const std::string& name, const std::string& address, int phones);
	ABEntry();
private:
	std::string theName;
	std::string theAddress;
	int thePhones;
};

ABEntry::ABEntry(const std::string& name, const std::string& address)
{
	// 这些都是赋值(assignments)而非初始化(initializations)
	// 首先调用default构造函数为这些变量设初值，然后立刻再对它们赋予新值
	theName = name;
	theAddress = address;
	thePhones = 2;
}

ABEntry::ABEntry(const std::string& name, const std::string& address, int phones)
	: theName(name), theAddress(address), thePhones(phones) // 这些都是初始化(initializations)
{} // 构造函数本体不必有任何动作

ABEntry::ABEntry() : theName(), theAddress(), thePhones(0) // 调用theName等的default构造函数
{}
```

读取未初始化的值会导致不明确的行为。

永远在使用对象之前先将它初始化。对于无任何成员的内置类型，你必须手工完成此事。

至于内置类型以外的任何其它东西，初始化责任落在构造函数(constructors)身上。规则很简单：确保每一个构造函数都将对象的每一个成员初始化。别混淆了赋值(assignment)和初始化(initialization)。**C++规定，对象的成员变量的初始化动作发生在进入构造函数本体之前。使用所谓的member initialization list(成员初值列)替换赋值动作。**

编译器会为用户自定义类型(user-defined types)之成员变量自动调用default构造函数。规定总是在初值列中列出所有成员变量。

如果成员变量是const或reference，它们就一定需要初值，不能被赋值。

许多classes拥有多个构造函数，每个构造函数有自己的成员初值列。如果这种classes存在许多成员变量和/或base classes。这种情况下可以合理地在初值列中遗漏那些”赋值表现像初始化一样好”的成员变量，改用它们的赋值操作，并将那些赋值操作移往某个函数(通常是private)，供所有构造函数调用。这种做法在”成员变量的初值系由文件或数据库读入”时特别有用。

C++有着十分固定的”成员初始化次序”。次序总是相同：base classes更早于其derived classes 被初始化，而class的成员变量总是以其声明次序被初始化。

所谓static对象，其寿命从被构造出来直到程序结束为止，因此stack和heap-based对象都被排除。这种对象包括global对象、定义于namespace作用域内的对象、在classes内、在函数内、以及在file作用域内被声明为static的对象。函数内的static对象称为local static对象(因为它们对函数而言是local)，其它static对象称为non-local static对象。**程序结束时static对象会被自动销毁，也就是它们的析构函数会在main()结束时被自动调用。**

所谓编译单元(translation unit)是指产出单一目标文件(single object file)的那些源码。基本上它是单一源码文件加上其所含入的头文件(#include files)。C++对”定义于不同编译单元内的non-local static对象”的初始化次序并无明确定义。消除这个问题，唯一需要做的是：将每个non-local static对象搬到自己的专属函数内(该对象在此函数内被声明为static)。这些函数返回一个reference指向它所含的对象。然后用户调用这些函数，而不直接指涉这些对象。换句话说，non-local static对象被local static对象替换了。这是Singleton模式的一个常见实现手法。C++保证，函数内的local static对象会在”该函数被调用期间””首次遇上该对象之定义式”时被初始化。

Singleton模式的介绍参考：https://blog.csdn.net/fengbingchun/article/details/22584107

任何一种non-const static对象，不论它是local或non-local，在多线程环境下”等待某事发生”都会有麻烦。处理这个麻烦的一种做法是：在程序的单线程启动阶段(single-threaded startup portion)手工调用所有reference-returning函数，这个消除与初始化有关的”竞速形势(race conditions)”。

**请记住：(1).为内置型对象进行手工初始化，因为C++不保证初始化它们。(2).构造函数最好使用成员初值列(member initialization list)，而不要在构造函数本体内使用赋值操作(assignment)。初值列列出的成员变量，其排列次序应该和它们在class中的声明次序相同。(3).为免除”跨编译单元之初始化次序”问题，请以local static对象替换non-local static对象。**

## 2  构造/析构/赋值运算(Constructors, Destructors, and Assignment Operators)

### Item 5. 了解C++默默编写并调用哪些函数(Know what functions C++ silently writes and calls)

```cpp
class Empty {};

int test_item_5()
{
	Empty e1; // default构造函数
	Empty e2(e1); // copy构造函数
	e2 = e1; // copy assignment操作符

	return 0;
}
```

什么时候empty class(空类)不再是个empty class呢？当C++处理过它之后。是的，如果自己没声明，编译器就会为它声明(编译器版本的)一个copy构造函数、一个copy assignment操作符和一个析构函数。此外如果你没有声明任何构造函数，编译器也会为你声明一个default构造函数。所有这些函数都是public且inline。**唯有当这些函数被需要(被调用)，它们才会被编译器创建出来**。default构造函数和析构函数主要是给编译器一个地方用来放置”藏身幕后”的代码，像是调用base classes和non-static成员变量的构造函数和析构函数。注意，编译器产出的析构函数是个non-virtual，除非这个class的bass class自身声明有virtual析构函数(这种情况下这个函数的虚属性virtualness，主要来自base class)。至于copy构造函数和copy assignment操作符，编译器创建的版本只是单纯地将来源对象的每一个non-static成员变量拷贝到目标对象。

如果你打算在一个”内含reference成员”的class内支持赋值操作(assignment)，你必须自己定义copy assignment操作符。面对”内含const成员”的classes，编译器的反应也一样。更改const成员是不合法的，所以编译器不知道如何在它自己生成的赋值函数内面对它们。最后还有一种情况：如果某个base classes将copy assignment操作符声明为private，编译器将拒绝为其derived class生成一个copy assignment操作符。

**请记住：编译器可以暗自为class创建default构造函数、copy构造函数、copy assignment操作符，以及析构函数。**

### Item 6. 若不想使用编译器自动生成的函数，就该明确拒绝(Explicitly disallow the use of compiler-generated functions you do not want)

```cpp
class Uncopyable {
protected:
	Uncopyable() {} // 允许derived对象构造和析构
	~Uncopyable() {}
private:
	Uncopyable(const Uncopyable&); // 但阻止copying
	Uncopyable& operator= (const Uncopyable&);
};

class HomeForSale: private Uncopyable {
// class不再声明copy构造函数或copy assign操作符
};

int test_item_6()
{
	HomeForSale h1;
	HomeForSale h2;
	//HomeForSale h3(h1); // 不会通过编译
	//h1 = h2; // 也不该通过编译

	return 0;
}
```

请记住：**为驳回编译器自动(暗自)提供的机能，可将相应的成员函数声明为private并且不予实现。使用像Uncopyable这样的base class也是一种做法**。

### Item 7. 为多态基类声明virtual析构函数(Declare destructors virtual in polymorphic base classes)

```cpp
class AWOV { // AWOV = "Abstract w/o Virtuals"
public:
	virtual ~AWOV() = 0; // 声明pure virtual析构函数
};

AWOV::~AWOV() {} // 必须为这个pure virtual析构函数提供一份定义
```

任何class只要带有virtual函数都几乎确定应该也有一个virtual析构函数。如果class不含virtual函数，通常表示它并不意图被用做一个base class。当class不企图被当作base class，令其析构函数为virtual往往是个馊主意。无端地将所有classes的析构函数声明为virtual，就像从未声明它们为virtual一样，都是错误的。

vptr(virtual table pointer)指向一个由函数指针构成的数组，称为vtbl(virtual table)。每一个带有virtual函数的class都有一个相应的vtbl。当对象调用某一virtual函数，实际被调用的函数取决于该对象的vptr所指的那个vtbl----编译器在其中寻找适当的函数指针。

析构函数的运作方式是，最深层派生(most derived)的那个class其析构函数最先被调用，然后是其每一个base class的析构函数被调用。编译器会在AWOV的derived classes的析构函数中创建一个对~AWOV的调用动作，所以你必须为这个函数提供一份定义。

“给base classes一个virtual析构函数”，这个规则只适用于polymorphic(带多态性质的)base classes身上。这种base classes的设计目的是为了用来”通过base class接口处理derived class对象”。并非所有base classes的设计目的都是为了多态用途。例如标准string和STL容器都不被设计作为base classes使用，更别提多态了。

**请记住：(1).polymorphic(带多态性质的)base classes应该声明一个virtual析构函数。如果class带有任何virtual函数，它就应该拥有一个virtual析构函数。(2).Classes的设计目的如果不是作为base classes使用，或不是为了具备多态性(polymorphically)，就不该声明virtual析构函数。**

### Item 8. 别让异常逃离析构函数(Prevent exceptions from leaving destructors)

C++并不禁止析构函数吐出异常，但它不鼓励你这样做。

**请记住：(1).析构函数绝对不要吐出异常。如果一个被析构函数调用的函数可能抛出异常，析构函数应该捕捉任何异常，然后吞下它们(不传播)或结束程序。(2).如果客户需要对某个操作函数运行期间抛出的异常做出反应，那么class应该提供一个普通函数(而非在析构函数中)执行该操作。**

### Item 9. 绝不在构造和析构过程中调用virtual函数(Never call virtual functions during construction or destruction)

derived class对象内的base class成分会在derived class自身成分被构造之前先构造妥当。由于base class构造函数的执行更早于derived class构造函数，当base class构造函数执行时derived class的成员变量尚未初始化。确定你的构造函数和析构函数都没有(在对象被创建和被销毁期间)调用virtual函数，而它们调用的所有函数也都服从同一约束。

**请记住：在构造和析构期间不要调用virtual函数，因为这类调用从不下降至derived class(比起当前执行构造函数和析构函数的那层)。**

### Item 10. 令operator=返回一个reference to *this(Have assignment operators return a reference to *this)
```cpp
class Widget {
public:
	Widget& operator= (const Widget& rhs) // 返回类型是个reference,指向当前对象
	{
		return *this;
	}

	Widget& operator+= (const Widget& rhs) // 这个协议适用于+=、-=、*=等等
	{
		return *this;
	}
	 
	Widget& operator= (int rhs) // 此函数也适用，即使此一操作符的参数类型不符协定
	{
		return *this;
	}
};
```

**请记住：令赋值(assignment)操作符返回一个reference to *this。**

### Item 11. 在operator=中处理”自我赋值”(Handle assignment to self in operator=)

```cpp
class Widget11 {
public:
	void swap(Widget11& rhs) {} // 交换*this和rhs的数据

	Widget11& operator= (const Widget11& rhs)
	{
		Widget11 temp(rhs); // 为rhs数据制作一份复件(副本)
		swap(temp); // 将*this数据和上述复件的数据交换
		return *this;
	}
};

int test_item_11()
{
	Widget11 w;
	w = w; // 赋值给自己

	return 0;
}
```

在operator=函数内手工排列语句(确保代码不但”异常安全”而且”自我赋值安全”)的一个替代方案是，使用所谓的copy and swap技术。

**请记住：(1).确保当对象自我赋值时operator=有良好行为。其中技术包括比较”来源对象”和”目标对象”的地址、精心周到的语句顺序、以及copy-and-swap。(2).确定任何函数如果操作一个以上的对象，而其中多个对象是同一个对象时，其行为仍然正确。**

### Item 12. 复制对象时勿忘其每一个成分(Copy all parts of an object)

```cpp
void logCall(const std::string& funcName) {} // 制造一个log entry

class Customer {
public:
	Customer(const Customer& rhs)
		: name(rhs.name) // 复制rhs的数据
	{
		logCall("Customer copy constructor");
	}

	Customer& operator= (const Customer& rhs)
	{
		logCall("Customer copy assignment operator");
		name = rhs.name; // 复制rhs的数据
		return *this;
	}

private:
	std::string name;
};

class PriorityCustomer : public Customer {
public:
	PriorityCustomer(const PriorityCustomer& rhs)
		: Customer(rhs), // 调用base class的copy构造函数
		  priority(rhs.priority)
	{
		logCall("PriorityCustormer copy constructor");
	}

	PriorityCustomer& operator= (const PriorityCustomer& rhs)
	{
		logCall("PriorityCustomer copy assignment operator");
		Customer::operator=(rhs); // 对base class成分进行赋值操作
		priority =  rhs.priority;
		return*this;
	}

private:
	int priority;
};
```

如果你为class添加一个成员变量，你必须同时修改copying函数(包括copy构造函数和copy assignment操作符)。(你也需要修改class的所有构造函数以及任何非标准形式的operator=。如果你忘记，编译器不太可能提醒你。)

任何时候只要你承担起”为derived class撰写copying函数”的重责大任，必须很小心地也复制其base class成分。那些成分往往是private，所以你无法直接访问它们，你应该让derived class的copying函数调用相应的base class函数。

令copy assignment操作符调用copy构造函数是不合理的，因为这就像试图构造一个已经存在的对象。令copy构造函数调用copy assignment操作符同样无意义。构造函数用来初始化新对象，而assignment操作符只施行于已初始化对象身上。

如果你发现你的copy构造函数和copy assignment操作符有相近的代码，消除重复代码的做法是，建立一个新的成员函数给两者调用。这样的函数往往是private而且常被命名为init。这个策略可以安全消除copy构造函数和copy assignment操作符之间的代码重复。

**请记住：(1).Copying函数应该确保复制”对象内的所有成员变量”及”所有base class成分”。(2).不要尝试以某个copying函数实现另一个copying函数。应该将共同机能放进第三个函数中，并由两个copying函数共同调用。**

## 3  资源管理(Resource Management)

### Item 13. 以对象管理资源(Use objects to manage resources)

把资源放进对象内，我们便可依赖C++的”析构函数自动调用机制”确保资源被释放。

auto_ptr是个”类指针(pointer-like)对象”，也就是所谓的”智能指针”，其析构函数自动对其所指对象调用delete。一定要注意别让多个auto_ptr同时指向同一对象，如果真是那样，对象会被删除一次以上。注：在C++11中auto_ptr已经被废弃，用unique_ptr替代。

std::unique_ptr用法参考：https://blog.csdn.net/fengbingchun/article/details/52203664

auto_ptr的替代方案是”引用计数型智慧指针”(reference-counting smart pointer, RCSP)。所谓RCSP也是个智能指针，持续追踪共有多少对象指向某笔资源，并在无人指向它时自动删除该资源。类似于C++11中的shared_ptr。

std::shared_ptr用法参考：https://blog.csdn.net/fengbingchun/article/details/52202007

**请记住：(1).为防止资源泄漏，请使用RAII(Resource Acquisition Is Initialization,资源取得时机便是初始化时机)对象，它们在构造函数中获得资源并在析构函数中释放资源。(2).两个常被使用的RAII classes分别是stdn::shared_ptr和auto_ptr。前者通常是较佳选择，因为其copy行为比较直观。若选择auto_ptr，复制动作会使它(被复制物)指向null。**

### Item 14. 在资源管理类中小心copying行为(Think carefully about copying behavior in resource-managing classes)

Copying函数(包括copy构造函数和copy assignment操作符)有可能被编译器自动创建出来，因此除非编译器所生版本做了你想要做的事，否则你得自己编写它们。

**请记住：(1).复制RAII对象必须一并复制它所管理的资源，所以资源的copying行为决定RAII对象的copying行为。(2).普遍而常见的RAII class copying行为是：抑制copying、施行引用计数法(reference counting)。不过其它行为也都可能被实现。**

### Item 15. 在资源管理类中提供对原始资源的访问(Provide access to raw resources in resource-managing classes)

**请记住：(1).APIs往往要求访问原始资源(raw resources)，所以每一个RAII class应该提供一个”取得其所管理之资源”的办法。(2).对原始资源的访问可能经由显示转换或隐式转换。一般而言显示转换比较安全，但隐式转换对客户比较方便。**

### Item 16. 成对使用new和delete时要采用相同形式(Use the same form in corresponding uses of new and delete)

```cpp
int test_item_16()
{
	std::string* stringPtr1 = new std::string;
	std::string* stringPtr2 = new std::string[100];

	delete stringPtr1;    // 删除一个对象
	delete [] stringPtr2; // 删除一个由对象组成的数组
	 
	typedef std::string AddressLines[4]; // 每个人的地址有4行，每行是一个string
	std::string* pal = new AddressLines; // 注意："new AddressLines"返回一个string*,就像"new string[4]"一样
	 
	//delete pal;    // 行为未有定义
	delete [] pal; // 很好
	 
	return 0;
}
```
当你使用new(也就是通过new动态生成一个对象)，有两件事发生：第一，内存被分配出来(通过名为operator new的函数)；第二，针对此内存会有一个(或更多)构造函数被调用。当你使用delete，也有两件事发生：针对此内存会有一个(或更多)析构函数被调用，然后内存才被释放(通过名为operator delete的函数)。

当你对着一个指针使用delete，唯一能够让delete知道内存中是否存在一个”数组大小记录”的办法就是：由你来告诉它。如果你使用delete时加上中括号(方括号)，delete便认定指针指向一个数组，否则它便认定指针指向单一对象。

如果你调用new时使用[]，你必须在对应调用delete时也使用[]。如果你调用new时没有使用[]，那么也不该在对应调用delete时使用[]。

最好尽量不要对数组形式做typedef动作。因为C++标准程序库含有string、vector等template，可将数组的需求降至几乎为零。

**请记住：如果你在new表达式中使用[]，必须在相应的delete表达式中也使用[]。如果你在new表达式中不使用[]，一定不要在相应的delete表达式中使用[]。**

### Item 17. 以独立语句将newed对象置入智能指针(Store newed objects in smart pointers in standalone statements)

```cpp
class Widget17 {};
int priority() { return 0; }
void processWidget(std::shared_ptr<Widget17> pw, int priority) {}

int test_item_17()
{
	// 执行new Widget17; 调用priority; 调用std::shared_ptr构造函数，它们的执行顺序不确定
	processWidget(std::shared_ptr<Widget17>(new Widget17), priority()); // 可能泄露资源
	
	std::shared_ptr<Widget17> pw(new Widget17); // 在单独语句内以智能指针存储newed所得对象
	processWidget(pw, priority()); // 这个调用动作绝不至于造成泄露
	 
	return 0;
}
```

**请记住：以独立语句将newed对象存储于(置入)智能指针内。如果不这样做，一旦异常被抛出，有可能导致难以察觉的资源泄漏。**

## 4  设计与声明(Designs and Declarations)

### Item 18. 让接口容易被正确使用，不易被误用(Make interface easy to use correctly and hard to use incorrectly)

**请记住：(1).好的接口很容易被正确使用，不容易被误用。你应该在你的所有接口中努力达成这些性质。(2).”促进正确使用”的办法包括接口的一致性，以及与内置类型的行为兼容。(3).”阻止误用”的办法包括建立新类型、限制类型上的操作，束缚对象值，以及消除客户的资源管理责任。(4).std::shared_ptr支持定制型删除器(custom deleter)。这可防范DLL问题，可被用来自动解除互斥锁(mutex)等等。**

### Item 19. 设计class犹如设计type(Treat class design as type design)

### Item 20. 宁以pass-by-reference-to-const替换pass-by-value(Prefer pass-by-reference-to-const to pass-by-value)

如果你有个对象属于内置类型(例如int)，pass by value往往比pass by reference的效率高些。

**请记住：(1).尽量以pass-by-reference-to-const替换pass-by-value。前者通常比较高效，并可避免切割问题(slicing problem)。(2).以上规则并不适用于内置类型，以及STL的迭代器和函数对象。对它们而言，pass-by-value往往比较适当。**

### Item 21. 必须返回对象时，别妄想返回其reference(Don’t try to return a reference when you must return an object)

```cpp
class Rational {
public:
	Rational(int numerator = 0, int denominator = 1) : n(numerator), d(denominator)
	{}

private:
	int n, d; // 分子(numerator)和分母(denominator)

// 返回const Rational可以预防"没意思的赋值动作": Rational a, b, c; (a * b) = c;
friend const Rational operator* (const Rational& lhs, const Rational& rhs)
{
	return Rational(lhs.n * rhs.n, lhs.d * rhs.d);
}
};
```

所谓reference只是个名称，代表某个既有对象。任何时候看到一个reference声明式，你都应该立刻问自己，它的另一个名称是什么？因为它一定是某物的另一个名称。任何函数如果返回一个reference指向某个local对象，都将一败涂地。(如果函数返回指针指向一个local对象，也是一样。)

**请记住：绝不要返回pointer或reference指向一个local stack对象，或返回reference指向一个heap-allocated对象，或返回pointer或reference指向一个local static对象而有可能同时需要多个这样的对象。**

### Item 22. 将成员变量声明为private(Declare data members private)

**请记住：(1).切记将成员变量声明为private。这可赋予客户访问数据的一致性、可细微划分访问控制、允诺约束条件获得保证，并提供class作者以充分的实现弹性。(2).protected并不比public更具封装性。**

### Item 23. 宁以non-member、non-friend替换member函数(Prefer non-member non-friend functions to member functions)

**请记住：宁可拿non-member non-friend函数替换member函数。这样做可以增加封装性、包裹弹性(packaging flexibility)和机能扩充性。**

### Item 24. 若所有参数皆需类型转换，请为此采用non-member函数(Declare non-member functions when type conversions should apply to all parameters)

```cpp
class Rational24 {
public:
	Rational24(int numerator = 0, int denominator = 1) {} // 构造函数刻意不为explicit,允许int-to-Rational24隐式转换
	int numerator() const { return 1; } // 分子(numerator)的访问函数
	int denominator() const { return 2; } // 分母(denominator)的访问函数

	/*const Rational24 operator* (const Rational24& rhs) const
	{
		return Rational24(this->n * rhs.numerator(), this->d * rhs.denominator());
	}*/

private:
	int n, d;
};

const Rational24 operator* (const Rational24& lhs, const Rational24& rhs) // non-member函数
{
	return Rational24(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
}

int test_item_24()
{
	Rational24 oneEighth(1, 8);
	Rational24 oneHalf(1, 2);
	Rational24 result = oneHalf * oneEighth; // 很好
	result = result * oneEighth; // 很好

	result = oneHalf * 2; // 很好,隐式类型转换(implicit type conversion)
	result = 2 * oneHalf; // 错误, only non-member function success
	 
	// 以对应的函数形式重写上述两个式子
	//result = oneHalf.operator*(2); // 很好, only member function success
	//result = 2.operator*(oneHalf); // 错误, only non-member function success
	 
	result = operator*(2, oneHalf); // 错误, only non-member function success
	 
	return 0;
}
```

无论何时如果你可以避免friend函数就该避免。

**请记住：如果你需要为某个函数的所有参数(包括被this指针所指的那个隐喻参数)进行类型转换，那么这个函数必须是个non-member。**

### Item 25. 考虑写出一个不抛异常的swap函数(Consider support for a non-throwing swap)

```cpp
class WidgetImpl { // 针对Widget25数据而设计的class
public:
private:
	int a, b, c; // 可能有许多数据，意味复制时间很长
	std::vector<double> v;
};

class Widget25 { // 这个class使用pimpl(pointer to implementation)手法
public:
	Widget25(const Widget25& rhs) {}
	Widget25& operator= (const Widget25& rhs) // 复制Widget25时，令它复制其WidgetImpl对象
	{
		*pImpl = *(rhs.pImpl);

		return *this;
	}
	 
	void swap(Widget25& other)
	{
		using std::swap;
		swap(pImpl, other.pImpl); // 若要置换Widget25就置换其pImpl指针
	}

private:
	WidgetImpl* pImpl; // 指针，所指对象内含Widget25数据
};

// std::swap针对Widget25特化版本
namespace std {
template<>
void swap<effective_cplusplus_::Widget25>(effective_cplusplus_::Widget25& a, effective_cplusplus_::Widget25& b)
{
	a.swap(b); // 若要置换Widget25,调用其swap成员函数
}

}
```
所谓swap(置换)两对象值，意思是将两对象的值彼此赋予对方。缺省情况下swap动作可由标准程序库提供的swap算法完成。

一般而言，重载function template没有问题，但std是个特殊的命名空间，其管理规则也比较特殊。客户可以全特化std内的template，但不可以添加新的template(或class或function或其它任何东西)到std里头。

首先，如果swap的缺省实现码对你的class或class template提供可接受的效率，你不需要额外做任何事。其次，如果swap缺省实现版的效率不足，试着做以下事情：(1). 提供一个public swap成员函数，让它高效地置换你的类型的两个对象值。这个函数不该抛出异常。(2). 在你的class或template所在的命名空间内提供一个non-member swap，并令它调用上述swap成员函数。(3). 如果你正在编写一个class(而非class template)，为你的class特化std::swap。并令它调用你的swap成员函数。 最后，如果你调用swap，请确定包含一个using声明式，以便让std::swap在你的函数内曝光可见，然后不加任何namespace修饰符，赤裸裸地调用swap。

**请记住：(1).当std::swap对你的类型效率不高时，提供一个swap成员函数，并确定这个函数不抛出异常。(2).如果你提供一个member swap，也该提供一个non-member swap用来调用前者。对于classes(而非template)，也请特化std::swap。(3).调用swap时应针对std::swap使用using声明式，然后调用swap并且不带任何”命名空间资格修饰”。(4).为”用户定义类型”进行std template全特化是好的，但千万不要尝试在std内加入某些对std而言全新的东西。**


## 5 实现(Implementations)

### Item 26. 尽可能延后变量定义式的出现时间(Postpone variable definitions as long as possible)

你不只应该延后变量的定义，直到非得使用该变量的前一刻为止，甚至应该尝试延后这份定义直到能够给它初值实参为止。如果这样，不仅能够避免构造(和析构)非必要对象，还可以避免无意义的default构造行为。

**请记住：尽可能延后变量定义式的出现。这样做可增加程序的清晰度并改善程序效率。**

### Item 27. 尽量少做转型动作(Minimize casting)

const_cast通常被用来将对象的常量性移除(cast away the constness)。它也是唯一有此能力的C++-style转型操作符。

dynamic_cast主要用来执行”安全向下转型”(safe downcasting)，也就是用来决定某对象是否归属继承体系中的某个类型。它是唯一无法由旧式语法执行的动作，也是唯一可能耗费重大运行成本的转型动作。

reinterpret_cast意图执行低级转型，实际动作(及结果)可能取决于编译器，这也就表示它不可移植。例如将一个pointer to int转型为一个int。

static_cast用来强迫隐式转换(implicit conversions)，例如将non-const对象转为const对象，或将int转为double等等。它也可以用来执行上述多种转换的反向转换，例如将void*指针转为typed指针，将pointer-to-base转为pointer-to-derived。但它无法将const转为non-const，这个只有const_cast才办得到。

**请记住：(1).如果可以，尽量避免转型，特别是在注重效率的代码中避免dynamic_cast。如果有个设计需要转型动作，试着发展无须转型的替代设计。(2).如果转型是必要的，试着将它隐藏于某个函数背后。客户随后可以调用该函数，而不需将转型放进他们自己的代码内。(3). 宁可使用C++-style(新式)转型，不要使用旧式转型(C风格转型)。前者很容易辨识出来，而且也比较有着分门别类的职掌。**

### Item 28. 避免返回handles指向对象内部成分(Avoid returning “handles” to object internals)

```cpp
class Point { // 这个class用来表述"点"
public:
	Point(int x, int y) {}
	void setX(int newVal) {}
	void setY(int newVal) {}
};

struct RectData { // 这些"点"数据用来表现一个矩形
	Point ulhc; // ulhc = "upper left-hand corner"(左上角)
	Point lrhc; // lrhc = "lower right-hand corner"(右上角)
};

class Rectangle {
public:
	Rectangle(const Point&, const Point&) {}
	Point& upperLeft() const { return pData->ulhc; }
	Point& lowerRight() const { return pData->lrhc; }

	// 有了这样的改变，客户可以读取矩形的Point，但不能涂写它们
	// 但即使如此，也可能导致dangling handles(空悬的号码牌):这种handles所指东西(的所属对象)不复存在
	//const Point& upperLeft() const { return pData->ulhc; }
	//const Point& lowerRight() const { return pData->lrhc; }

private:
	std::shared_ptr<RectData> pData;
};

int test_item_28()
{
	Point coord1(0, 0);
	Point coord2(100, 100);
	const Rectangle rec(coord1, coord2); // rec是个const矩形，从(0,0)到(100,100)

	// upperLeft的调用者能够使用被返回的reference(指向rec内部的Point成员变量)来更改成员，但rec其实应该是不可变的(const)
	rec.upperLeft().setX(50); // 现在rec却变成从(50,0)到(100,100)
	 
	return 0;
}
```

reference、指针和迭代器统统都是所谓的handles(号码牌，用来取得某个对象)，而返回一个”代表对象内部数据”的handle，随之而来的便是”降低对象封装性”的风险。

通常我们认为，对象的”内部”就是指它的成员变量，但其实不被公开使用的成员函数(也就是被声明为protected或private者)也是对象”内部”的一部分。因此也应该留心不要返回它们的handles。这意味你绝对不该令成员函数返回一个指针指向”访问级别较低”的成员函数。

**请记住：避免返回handles(包括reference、指针、迭代器)指向对象内部。遵守这个条款可增加封装性，帮助const成员函数的行为像个const，并将发生”虚吊号码牌”(dangling handles)的可能性降至最低。**

### Item 29. 为”异常安全”而努力是值得的(Strive for exception-safe code)

“异常安全”有两个条件：(1).不泄漏任何资源。(2).不允许数据败坏。

异常安全函数(Exception-safe functions)提供以下三个保证之一：

(1).基本承诺：如果异常被抛出，程序内的任何事物仍然保持在有效状态下。没有任何对象或数据结构会因此而败坏，所有对象都处于一种内部前后一致的状态(例如所有的class约束条件都继续获得满足)。然而程序的现实状态(exact state)恐怕不可预料。

(2).强烈保证：如果异常被抛出，程序状态不改变。调用这样的函数需有这样的认知：如果函数成功，就是完全成功；如果函数失败，程序会回复到”调用函数之前”的状态。

(3).不抛掷(nothrow)保证：承诺绝不抛出异常，因为它们总是能够完成它们原先承诺的功能。作用于内置类型(例如int,指针等等)身上的所有操作都提供nothrow保证。这是异常安全码中一个必不可少的关键基础材料。

异常安全码(Exception-safe code)必须提供上述三种保证之一。如果它不这样做，它就不具备异常安全性。

有个一般化的设计策略很典型地会导致强烈保证，这个策略被称为copy and swap。原则很简单：为你打算修改的对象(原件)做出一份副本，然后在那副本身上做一切必要修改。若有任何修改动作抛出异常，原对象仍保持未改变状态。待所有改变都成功后，再将修改过的那个副本和原对象在一个不抛出异常的操作中置换(swap)。但一般而言它并不保证整个函数有强烈的异常安全性。

**请记住：(1).异常安全函数(Exception-safe function)即使发生异常也不会泄漏资源或允许任何数据结构败坏。这样的函数区分为三种可能的保证：基本型、强烈型、不抛异常型。(2).”强烈保证”往往能够以copy-and-swap实现出来，但”强烈保证”并非对所有函数都可实现或具备现实意义。(3).函数提供的”异常安全保证”通常最高只等于其所调用之各个函数的”异常安全保证”中的最弱者。**

### Item 30. 透彻了解inlining的里里外外(Understand the ins and outs of inlining)

```cpp
inline void f() {} // 假设编译器有意愿inline“对f的调用”

int test_item_30()
{
	void (*pf)() = f; // pf指向f

	f(); // 这个调用将被inlined,因为它是一个正常调用
	pf(); // 这个调用或许不被inlined,因为它通过函数指针达成
	 
	return 0;
}
```

inline函数背后的整体观念是，将”对此函数的每一个调用”都以函数本体替换之。

过度热衷inlining会造成程序体积太大(对可用空间而言)。即使拥有虚内存，inline造成的代码膨胀亦会导致额外的换页行为(paging)，降低指令高速缓存装置的击中率(instruction cache hit rate)，以及伴随这些而来的效率损失。

inline只是对编译器的一个申请，不是强制命令。这项申请可以隐喻提出，也可以明确提出。隐喻方式是将函数定义于class定义式内。这样的函数通常是成员函数。friend函数如果被定义于class内，它们也是被隐喻声明为inline。明确声明inline函数的做法则是在其定义式前加上关键字inline。

inline函数通常一定被置于头文件内，因为大多数建置环境(build environments)在编译过程中进行inlining，而为了将一个”函数调用”替换为”被调用函数的本体”，编译器必须知道那个函数长什么样子。

inlining在大多数C++程序中是编译期行为。

templates通常也被置于头文件内，因为它一旦被使用，编译器为了将它具现化，需要知道它长什么样子。(这其实也不是世界一统的准则。某些建置环境可以在链接期才执行template具现化。只不过编译期完成具现化动作比较常见。)template的具现化与inlining无关。

大部分编译器拒绝将太过复杂(例如带有循环或递归)的函数inlining，而所有对virtual函数的调用(除非是最平淡无奇的)也都会使inlining落空。

**一个表面上看似inline的函数是否真是inline，取决于你的建置环境，主要取决于编译器。**大多数编译器提供了一个诊断级别:如果它们无法将你要求的函数inline化，会给你一个警告信息。为数ining个函数长声明entshit rate

编译器通常不对”通过函数指针而进行的调用”实施inlining，这意味对inline函数的调用有可能被inlined，也可能不被inlined，取决于该调用的实施方式。

实际上构造函数和析构函数往往是inlining的糟糕候选人。

inline函数无法随着程序库的升级而升级。换句话说如果f是程序库内的一个inline函数，客户将”f函数本体”编进其程序中，一旦程序库设计者决定改变f，所有用到f的客户端程序都必须重新编译。

一开始先不要将任何函数声明为inline，或至少将inlining施行范围局限在那些”一定成为inline”或”十分平淡无奇”的函数身上。慎重使用inline便是对日后使用调试器带来帮助。

请记住：(1).将大多数inlining限制在小型、被频繁调用的函数身上。这可使日后的调试过程和二进制升级(binary upgradability)更容易，也可使潜在的代码膨胀问题最小化，使程序的速度提升机会最大化。(2).不要只因为function templates出现在头文件，就将它们声明为inline。

### Item 31. 将文件间的编译依存关系降至最低(Minimize compilation dependencies between files)

标准程序库组件不该被前置声明。

设计策略：

(1). 如果使用object references或object pointers可以完成任务，就不要使用objects。你可以只靠一个类型声明式就定义出指向该类型的references和pointers；但如果定义某类型的objects，就需要用到该类型的定义式。

(2). 如果能够，尽量以class声明式替换class定义式。注意，当你声明一个函数而它用到某个class时，你并不需要该class的定义；纵使函数以by value方式传递该类型的参数(或返回值)亦然。

(3). 为声明式和定义式提供不同的头文件。为了促进严守上述准则，需要两个头文件，一个用于声明式，一个用于定义式。当然，这些文件必须保持一致性，如果有个声明式被改变了，两个文件都得改变。

Handle classes(使用pimpl idiom(pimpl是”pointer to implementation”的缩写))和Interface classes(特殊的abstract base class(抽象基类))解除了接口和实现之间的耦合关系，从而降低文件间的编译依存性(compilation dependencies)。

在Handle classes身上，成员函数必须通过implementation pointer取得对象数据。那会为每一次访问增加一层间接性。而每一个对象消耗的内存数量必须增加implementation pointer的大小。最后，implementation pointer必须初始化(在Handle class构造函数内)，指向一个动态分配得来的implementation object，所以你将蒙受因动态内存分配(及其后的释放动作)而来的额外开销，以及遭遇bad_alloc异常(内存不足)的可能性。

至于interface classes，由于每个函数都是virtual，所以你必须为每次函数调用付出一个间接跳跃(indirect jump)成本。此外interface class派生的对象必须内含一个vptr(virtual table pointer)，这个指针可能会增加存放对象所需的内存数量----实际取决于这个对象除了interface class之外是否还有其它virtual函数来源。

最后，不论handle classes或interface classes，一旦脱离inline函数都无法有太大作为。函数本体为了被inlined必须(很典型地)置于头文件内，但handle classes和interface classes正是被设计用来隐藏实现细节如函数本体。

然而，如果只因为若干额外成本便不考虑handle classes和interface classes，将是严重的错误。

**请记住：(1). 支持”编译依存性最小化”的一般构想是：相依于声明式，不要相依于定义式。基于此构想的两个手段是handle classes和interface classes。(2). 程序库头文件应该以”完全且仅有声明式”(full and declaration-only forms)的形式存在。这种做法不论是否涉及templates都适用。**


## 6  继承与面向对象设计(Inheritance and Object-Oriented Design)

### Item 32. 确定你的public继承塑模出is-a关系(Make sure public inheritance models “is-a”)

public inheritance(公开继承)意味”is-a”(是一种)的关系。

如果你令class D(“Derived”)以public形式继承class B(“Base”)，你便是告诉C++编译器(以及你的代码读者)说，每一个类型为D的对象同时也是一个类型为B的对象，反之不成立。

**请记住：”public继承”意味is-a。适用于base classes身上的每一件事情一定也适用于derived classes身上，因为每一个derived class对象也都是一个base class对象。**

### Item 33. 避免遮挡继承而来的名称(Avoid hiding inherited names)

```cpp
class Base {
private:
	int x;

public:
	virtual void mf1() = 0;
	virtual void mf1(int) {}
	virtual void mf2() {}
	void mf3() {}
	void mf3(double) {}
};

class Derived : public Base {
public:
	virtual void mf1() {}
	void mf3() {}
	void mf4() {}
};

class Derived33 : public Base {
public:
	// 必须为那些原本会被遮掩的每个名称引入一个using声明式，否则某些你希望继承的名称会被遮掩
	using Base::mf1; // 让Base class内名为mf1和mf3的所有东西在Derived作用域内都可见(并且public)
	using Base::mf3;
	virtual void mf1() {}
	void mf3() {}
	void mf4() {}
};

int test_item_33()
{
	Derived d;
	int x = 0;

	d.mf1(); // 没问题，调用Derived::mf1
	//d.mf1(x); // 错误，因为Derived::mf1遮掩了Base::mf1
	d.mf2(); // 没问题，调用Base::mf2
	d.mf3(); // 没问题，调用Derived::mf3
	// Derived内的函数mf3遮掩了一个名为mf3但类型不同的Base函数
	//d.mf3(x); // 错误，因为Derived::mf3遮掩了Base::mf3
	 
	Derived33 d2;
	 
	d2.mf1(); // 仍然没问题，仍然调用Derived::mf1
	d2.mf1(x); // 现在没问题了，调用Base::mf1
	d2.mf2(); // 仍然没问题，调用Base::mf2
	d2.mf3(); // 没问题，调用Derived::mf3
	d2.mf3(x); // 现在没问题了，调用Base::mf3
	 
	return 0;
}
```

**请记住：(1). derived classes内的名称会遮掩base classes内的名称。在public继承下从来没有人希望如此。(2).为了让被遮掩的名称再见天日，可使用using声明式或转交函数(forwarding functions)。**

### Item 34. 区分接口继承和实现继承(Differentiate between inheritance of interface and inheritance of implementation)

成员函数的接口总是会被继承。

声明一个pure virtual函数的目的是为了让derived classes只继承函数接口。

声明简朴的(非纯)impure virtual函数的目的，是让derived classes继承该函数的接口和缺省实现。

声明non-virtual函数的目的是为了令derived classes继承函数的接口及一份强制性实现。

**请记住：(1).接口继承和实现继承不同。在public继承之下，derived classes总是继承base class的接口。(2). pure virtual函数只具体指定接口继承。(3). 简朴的(非纯)impure virtual函数具体指定接口继承及缺省实现继承。(4). non-virtual函数具体指定接口继承以及强制性实现继承。**

### Item 35. 考虑virtual函数以外的其它选择(Consider alternatives to virtual functions)

```cpp
class GameCharacter {
public:
	int healthValue() const // derived classes不重新定义它
	{
		// ... // 做一些事前工作
		int retVal = doHealthValue(); // 做真正的工作
		// ... // 做一些事后工作

		return retVal;
	}

private:
	virtual int doHealthValue() const // derived classes可重新定义它
	{
		return 10;
	}
};


// Strategy设计模式的简单应用
class GameCharacter35; // 前置声明(forward declaration)
int defaultHealthCalc(const GameCharacter35& gc) { return 1; }
class GameCharacter35 {
public:
	typedef int (*HealthCalcFunc)(const GameCharacter35&);
	explicit GameCharacter35(HealthCalcFunc hcf = defaultHealthCalc) : healthFunc(hcf) {}
	int healthValue() const
	{
		return healthFunc(*this); 
	}

private:
	HealthCalcFunc healthFunc;
};

class EvilBadGuy : public GameCharacter35 {
public:
	explicit EvilBadGuy(HealthCalcFunc hcf = defaultHealthCalc) : GameCharacter35(hcf) {}
};

int loseHealthQuickly(const GameCharacter35&) { return 2; } // 健康指数计算函数1
int loseHealthSlowly(const GameCharacter35&) { return 3; } // 健康指数计算函数2

// 藉由std::function完成Strategy模式
class GameCharacter35_1;
int defaultHealthCalc_1(const GameCharacter35_1& gc) { return 11; }
class GameCharacter35_1 {
public:
	// HealthCalcFunc可以是任何"可调用物"(callae entity),可被调用并接受
	// 任何兼容于GameCharacter35_1之物，返回任何兼容于int的东西
	typedef std::function<int (const GameCharacter35_1&)> HealthCalcFunc;
	explicit GameCharacter35_1(HealthCalcFunc hcf = defaultHealthCalc_1) : healthFunc(hcf) {}
	int healthValue() const
	{
		return healthFunc(*this);
	}

private:
	HealthCalcFunc healthFunc;
};

int test_item_35()
{
	EvilBadGuy ebg1(loseHealthQuickly); // 相同类型的人物搭配不同的健康计算方式
	EvilBadGuy ebg2(loseHealthSlowly);

	return 0;
}
```


藉由Non-Virtual Interface手法实现Template Method模式：这一基本设计，也就是”令客户通过public non-virtual成员函数间接调用private virtual函数”，称为non-virtual interface(NVI)手法。它是所谓Template Method设计模式(与C++ templates并无关联)的一个独特表现形式。把这个non-virtual函数称为virtual函数的外覆器(wrapper)。在NVI手法下其实没有必要让virtual函数一定得是private。

Template Method模式介绍参考：https://blog.csdn.net/fengbingchun/article/details/30990475

藉由Function Pointers实现Strategy模式。

Strategy模式介绍参考：https://blog.csdn.net/fengbingchun/article/details/32179281

藉由std::function完成Strategy模式。

std::function介绍参考：https://blog.csdn.net/fengbingchun/article/details/52562918

当你为解决问题而寻找某个设计方法时，不妨考虑virtual函数的替代方案：它们各自有其相对的优点和缺点

(1). 使用**non-virtual interface(NVI)手法**，那是Template Method设计模式的一种特殊形式。它以public non-virtual成员函数包裹较低访问性(private或protected)的virtual函数。

(2). 将virtual函数替换为”函数指针成员变量”，这是Strategy设计模式的一种分解表现形式。

(3). 以std::function成员变量替换virtual函数，因而允许使用任何可调用物(callable entity)搭配一个兼容于需求的签名式。这也是Strategy设计模式的某种形式。

(4). 将继承体系内的virtual函数替换为另一个继承体系内的virtual函数。这是Strategy设计模式的传统实现方法。

**请记住：(1). virtual函数的替代方案包括NVI手法及Strategy设计模式的多种形式。NVI手法自身是一个特殊形式的Template Method设计模式。(2). 将机能从成员函数移到class外部函数，带来的一个缺点是，非成员函数无法访问class的non-public成员。(3). std::function对象的行为就像一般函数指针。这样的对象可接纳”与给定之目标签名式(target signature)兼容”的所有可调用物(callable entities)。**

### Item 36. 绝不重新定义继承而来的non-virtual函数(Never redefine an inherited non-virtual function)

```cpp
class B {
public:
	void mf() { fprintf(stdout, "B::mf\n"); }
};

class D : public B {
public:
	void mf() { fprintf(stdout, "D::mf\n"); } // 遮掩(hides)了B::mf
};

int test_item_36()
{
	D x;
	B* pB = &x;
	// 由于pB被声明为一个pointer-to-B,通过pB调用的non-virtual函数永远是B所定义的版本，即使pB指向一个类型为"B派生之class"的对象
	pB->mf(); // 调用B::mf

	D* pD = &x;
	pD->mf(); // 调用D::mf
	 
	return 0;
}
```

non-virtual函数都是静态绑定(statically bound)，virtual函数确是动态绑定(dynamically bound)。

**请记住：绝对不要重新定义继承而来的non-virtual函数。**

### Item 37. 绝不重新定义继承而来的缺省参数值(Never redefine a function’s inherited default parameter value)

```cpp
class Shape37 {
public:
	enum ShapeColor { Red, Green, Blud };
	// 所有形状都必须提供一个函数，用来绘出自己
	virtual void draw(ShapeColor color = Red) const = 0;
};

class Rectangle37 : public Shape37 {
public:
	// 注意，赋予不同的缺省参数值。这真糟糕!
	virtual void draw(ShapeColor color = Green) const
	{
		fprintf(stdout, "rectangle shape color: %d\n", color);
	}
};

class Circle37 : public Shape37 {
public:
	virtual void draw(ShapeColor color) const
	// 请注意，以上这么写则当客户以对象调用此函数，一定要指定参数值。因为静态绑定下这个函数并
	// 不从其base继承缺省参数值。但若以指针(或reference)调用此函数，可以不指定参数值，因为动态
	// 绑定下这个函数会从其base继承缺省参数值
	{
		fprintf(stdout, "circle shape color: %d\n", color);
	}
};

int test_item_37()
{
	Shape37* ps; // 静态类型为Shape37*；没有动态类型，因为它尚未指向任何对象
	Shape37* pc = new Circle37; // 静态类型为Shape37*,动态类型为Circle37*
	Shape37* pr = new Rectangle37; // 静态类型为Shape37*,动态类型为Rectangle37*

	pc->draw(Shape37::Red); // 调用Circle37::draw(Shape37::Red)
	pr->draw(Shape37::Red); // 调用Rectangle::draw(Shape37::Red)
	 
	// 即使把指针换成references问题仍然存在
	pr->draw(); // 调用Rectangle37::draw(Shape37::Red)!
	 
	return 0;
}
```

virtual函数系动态绑定(dynamically bound)，而缺省参数值确是静态绑定(statically bound)。静态绑定又名前期绑定，early binding；动态绑定又名后期绑定，late binding。

对象的所谓静态类型(static type)，就是它在程序中被声明时所采用的类型。对象的所谓动态类型(dynamic type)则是指”目前所指对象的类型”。也就是说，动态类型可以表现出一个对象将会有什么行为。virtual函数系动态绑定而来，意思是调用一个virtual函数时，究竟调用哪一份函数实现代码，取决于发出调用的那个对象的动态类型。

**请记住：绝对不要重新定义一个继承而来的缺省参数值，因为缺省参数值都是静态绑定，而virtual函数----你唯一应该覆写的东西----确是动态绑定。**

### Item 38. 通过复合塑模出has-a或”根据某物实现出”(Model “has-a” or “is-implemented-in-terms-of” through composition)

```cpp
class Address {};
class PhoneNumber {};
class Person {
private:
	std::string name;        // 合成成分物(composed object)
	Address address;         // 同上
	PhoneNumber voiceNumber; // 同上
	PhoneNumber faxNumber;   // 同上
};
```

复合(composition)是类型之间的一种关系，当某种类型的对象内含它种类型的对象，便是这种关系。

public继承带有is-a(是一种)的意义。复合也有它自己的意义。实际上它有两个意义。复合意味has-a(有一个)或is-implemented-in-terms-of(根据某物实现出)。

**请记住：(1). 复合(composition)的意义和public继承完全不同。(2). 在应用域(application domain)，复合意味has-a(有一个)。在实现域(implementation domain)，复合意味is-implemented-in-terms-of(根据某物实现出)。**

### Item 39. 明智而审慎地使用private继承(Use private inheritance judiciously)
```cpp
class Empty39 {};
class HoldsAnInt {
private:
	int x;
	Empty39 e;
};

class HoldsAnInt39 : private Empty39 { // EBO(empty base optimization)
private:
	int x;
};

int test_item_39()
{
	fprintf(stdout, "sizeof(Empty39): %d\n", sizeof(Empty39)); // 1
	fprintf(stdout, "sizeof(HoldsAnInt): %d\n", sizeof(HoldsAnInt)); // 8 
	fprintf(stdout, "sizeof(HoldsAnInt39): %d\n", sizeof(HoldsAnInt39)); // 4

	return 0;
}
```

如果classes之间的继承关系是private，编译器不会自动将一个derived class对象转换为一个base class对象。这和public继承的情况不同。由private base class继承而来的所有成员，在derived class中都会变成private属性，纵使它们在base class中原本是protected或public属性。private继承意味implemented-in-terms-of(根据某物实现出)。如果你让class D以private形式继承class B，你的用意是为了采用class B内已经备妥的某些特性，不是因为B对象和D对象存在有任何观念上的关系。private继承纯粹只是一种实现技术。private继承意味只有实现部分被继承，接口部分应略去。如果D以private形式继承B，意思是D对象根据B对象实现而得，再没有其它意涵了。

尽可能使用复合，必要时才使用private继承。

EBO(empty base optimization, 空白基类最优化)，EBO一般只在单一继承(而非多重继承)下才可行。

**请记住：(1). private继承意味is-implemented-in-terms-of(根据某物实现出)。它通常比复合(composition)的级别低。但是当derived class需要访问protected base class的成员，或需要重新定义继承而来的virtual函数时，这么设计是合理的。(2).和复合(composition)不同，private继承可以造成empty base最优化。这对致力于”对象尺寸最小化”的程序库开发者而言，可能很重要。**


## 7  模板与泛型编程(Templates and Generic Programming) 

### Item 40. 明智而审慎地使用多重继承(Use multiple inheritance judiciously)

**请记住：(1).多重继承比单一继承复杂。它可能导致新的歧义性，以及对virtual继承的需要。(2).virtual继承会增加大小、速度、初始化(及赋值)复杂度等等成本。如果virtual base classes不带任何数据，将是最具实用价值的情况。(3).多重继承的确有正当用途。其中一个情节涉及”public继承某个Interface class”和”private继承某个协助实现的class”的两相组合。**

### Item 41. 了解隐式接口和编译期多态(Understand implicit interfaces and compile-time polymorphism)

```cpp
// 隐式接口
template<typename T>
void doProcessing(T& w)
{
	T someNastyWidget;
	if (w.size() > 10 && w != someNastyWidget) {
		T temp(w);
		temp.normalize();
		temp.swap(w);
	}
}

// 显示接口
class Widget41 {
public:
	Widget41();
	virtual std::size_t size() const;
	virtual void normalize();
	void swap(Widget41& other);
};
```

“以不同的template参数具现化function templates”会导致调用不同的函数，这便是所谓的编译期多态(compile-time polymorphism)。”哪一个重载函数该被调用”(发生在编译期)和”哪一个virtual函数该被绑定”(发生在运行期)。

通常显式接口由函数的签名式(也就是函数名称、参数类型、返回类型)构成。隐式接口就完全不同了，它并不基于函数签名式，而是由有效表达式(valid expressions)组成。

**请记住：(1).classes和templates都支持接口(interface)和多态(polymorphism)。(2).对classes而言接口是显式的(explicit)，以函数签名为中心。多态则是通过virtual函数发生于运行期。(3).对template参数而言，接口是隐式的(implicit)，奠基于有效表达式。多态则是通过template具现化和函数重载解析(function overloading resolution)发生于编译期。**

### Item 42. 了解typename的双重意义(Understand the two meanings of typename)

```cpp
template<typename C>
void print2nd(const C& container)
{
	if (container.size() >= 2) {
		// 一般性规则很简单：任何时候当你想要在template中指涉一个嵌套从属类型名称，就必须
		// 在紧临它的前一个位置放上关键字typename
		typename C::const_iterator iter(container.begin());
	}
}

template<typename C> //允许使用"typename"(或"class")
void f42(const C& container, // 不允许使用"typename",C并不是嵌套从属类型名称
	typename C::iterator iter); // 一定要使用"typename",C::iterator是个嵌套从属类型名称

template<typename T>
class Base42 {
	class Nested {
		Nested(int x) {}
	};
};

template<typename T>
class Derived42 : public Base42<T>::Nested { // base class list中不允许"typename"
public:
	explicit Derived42(int x) 
	: Base42<T>::Nested(x) // mem.init.list中不允许"typename"
	{
		typename Base42<T>::Nested temp; // 嵌套从属类型名称，既不在base class list中也不在
			// mem.init.list中，作为一个base class修饰符需加上typename
	}
};

template<typename IterT>
void workWithIterator(IterT iter)
{
	// 使用IterT对象所指物的相同类型将temp初始化为iter所指物
	// 如果IterT是vector<int>::iterator,temp的类型就是int
	// value_type被嵌套于iterator_traits<IterT>之内而IterT是个template参数，所以必须在它之前放置typename
	typename std::iterator_traits<IterT>::value_type temp(*iter);
	
	typedef typename std::iterator_traits<IterT>::value_type value_type;
	value_type temp2(*iter);
}
```

当我们声明template类型参数，class和typename的意义完全相同。

template内出现的名称如果相依于某个template参数，称之为从属名称(dependent names)。如果从属名称在class内呈嵌套状，我们称它为嵌套从属名称(nested dependent name)。嵌套从属类型名称(nested dependent type name)，也就是个嵌套从属名称并且指涉某类型。

tepename只被用来验明嵌套从属类型名称，其它名称不该有它存在。

“typename必须作为嵌套从属类型名称的前缀词”这一规则的例外是，typename不可以出现在base classes list内的嵌套从属类型名称之前，也不可在member initialization list(成员初值列)中作为base class修饰符。

**请记住：(1).声明template参数时，前缀关键字class和typename可互换。(2).请使用关键字typename标识嵌套从属类型名称；但不得在base class lists(基类列)或member initialization list(成员初值列)内以它作为base class修饰符。**

### Item 43. 学习处理模板化基类内的名称(Know how to access names in templatized base classes)

```cpp
class CompanyA {
public:
	void sendCleartext(const std::string& msg) {}
	void sendEncrypted(const std::string& msg) {}
};

class CompanyB {
public:
	void sendCleartext(const std::string& msg) {}
	void sendEncrypted(const std::string& msg) {}
};

class CompanyZ {
public:
	void sendEncrypted(const std::string& msg) {}
};

class MsgInfo {};

template<typename Company>
class MsgSender {
public:
	void sendClear(const MsgInfo& info)
	{
		std::string msg;
		Company c;
		c.sendCleartext(msg);
	}

	void sendSecret(const MsgInfo& info)
	{
		std::string msg;
		Company c;
		c.sendEncrypted(msg);
	}
};

// 一个全特化的MsgSender,它和一般template相同，差别只在于它删掉了sendClear
// class定义式最前面的"template<>"语法象征这既不是template也不是标准class,而是个特化版的MsgSender template
template<>
class MsgSender<CompanyZ> {
public:
	void sendSecret(const MsgInfo& info)
	{
		std::string msg;
		CompanyZ c;
		c.sendEncrypted(msg);
	}
};

template<typename Company>
class LoggingMsgSender : public MsgSender<Company> {
public:
	//using MsgSender<Company>::sendClear;

	void sendClearMsg(const MsgInfo& info)
	{
		//sendClear(info); // 调用base class函数，无法通过编译
	 
		//this->sendClear(info); // 成立，假设sendClear将被继承
	 
		//sendClear(info); // using MsgSender<Company>::sendClear, ok,假设sendClear将被继承下来
	 
		MsgSender<Company>::sendClear(info); // 明白指出被调用的函数位于base class内，ok,假设sendClear将被继承下来
	}
};
```


从Object Oriented C++跨进Template C++，继承就不像以前那般畅行无阻了。令C++”不进入templatized base classes观察”的行为失效。有三个办法，第一是在base class函数调用动作之前加上”this->”；第二是使用using声明式；第三是明白指出被调用的函数位于base class内。

面对”指涉base class members”之无效references，编译器的诊断时间可能发生在早期(当解析derived class template的定义式时)，也可能发生在晚期(当那些templates被特定之template实参具现化时)。C++的政策是宁愿较早诊断。

**请记住：可在derived class templates内通过”this->”指涉base class templates内的成员名称，或藉由一个明白写出的”base class资格修饰符”完成。**

### Item 44. 将与参数无关的代码抽离templates(Factor parameter-independent code out of templates)

class templates的成员函数只有在被使用时才被暗中具现化。

如果你不小心，使用templates可能会导致代码膨胀(code bloat)：其二进制码带着重复(或几乎重复)的代码、数据或两者。

**请记住：(1).Templates生成多个classes和多个函数，所以任何template代码都不该与某个造成膨胀的template参数产生相依关系。(2).因非类型模板参数(non-type template parameters)而造成的代码膨胀，往往可消除，做法是以函数参数或class成员变量替换template参数。(3).因类型参数(type parameters)而造成的代码膨胀，往往可降低，做法是让带有完全相同二进制表述(binary representations)的具现类型(instantiation types)共享实现码。**

### Item 45. 运用成员函数模板接受所有兼容类型(Use member function templates to accept “all compatible types”)
```cpp
template<typename T>
class SmartPtr {
public:
	// 泛化copy构造函数并未被声明为explicit,那是蓄意的，因为原始指针类型之间的转换(例如从
	// derived class指针转为base class指针)是隐式转换，无需明白写出转型动作(cast)
	template<typename U>
	SmartPtr(const SmartPtr<U>& other) : heldPtr(other.get()) {}// 以other的heldPtr初始化this的heldPtr

	T* get() const { return heldPtr; }

private:
	T* heldPtr; // 这个SmartPtr持有的内置(原始)指针
};
```

在class内声明泛化copy构造函数(是个member template)并不会阻止编译器生成它们自己的copy构造函数(一个non-template)，所以如果你想要控制copy构造的方方面面，你必须同时声明泛化copy构造函数和”正常的”copy构造函数。相同规则也适用于赋值(assignment)操作。

**请记住：(1).请使用member function template(成员函数模板)生成”可接受所有兼容类型”的函数。(2).如果你声明member templates用于”泛化copy构造”或”泛化assignment操作”，你还需要声明正常的copy构造函数和copy assignment操作符。**

### Item 46. 需要类型转换时请为模板定义非成员函数(Define non-member functions inside templates when type conversions are desired)

```cpp
template<typename T> class Rational46;
template<typename T>
const Rational46<T> doMutiply(const Rational46<T>& lhs, const Rational46<T>& rhs);

template<typename T>
class Rational46 {
public:
	Rational46(const T& numerator = 0, const T& denominator = 1) {}
	const T numerator() const { return (T)0; }
	const T denominator() const { return (T)0; }

	friend const Rational46 operator* (const Rational46& lhs, const Rational46& rhs)
	{
		//return Rational46(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
		
		// 令friend函数调用辅助函数
		return doMultiply(lhs, rhs);
	}
};

template<typename T>
const Rational46<T> doMultiply(const Rational46<T>& lhs, const Rational46<T>& rhs)
{
	return Rational46<T>(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
}

int test_item_46()
{
	Rational46<int> oneHalf(1, 2);
	Rational46<int> result = oneHalf * 2;

	return 0;
}
```

template实参推导过程中从不将隐式类型转换函数纳入考虑。

在一个class template内，template名称可被用来作为”template和其参数”的简略表达方式，所以在Rational<T>内我们可以只写Rational而不必写Rational<T>。

**请记住：当我们编写一个class template，而它所提供之”与此template相关的”函数支持”所有参数之隐式类型转换”时，请将那些函数定义为”class template内部的friend函数”。**

### Item 47. 请使用traits classes表现类型信息(Use traits classes for information about types)

```cpp
// 类型IterT的iterator_category
template<typename IterT>
struct iterator_traits {
	typedef typename IterT::iterator_category iterator_category;
};

// template偏特化，针对内置指针
template<typename IterT>
struct iterator_traits<IterT*> {
	typedef std::random_access_iterator_tag iterator_category;
};

// 实现用于random access迭代器
template<typename IterT, typename DistT>
void doAdvance(IterT& iter, DistT d, std::random_access_iterator_tag)
{
	iter += d;
}

// 实现用于bidirectional迭代器
template<typename IterT, typename DistT>
void doAdvance(IterT& iter, DistT d, std::bidirectional_iterator_tag)
{
	if (d >= 0) { while (d--) ++iter; }
	else { while (d++) --iter; }
}

// 实现用于input迭代器
template<typename IterT, typename DistT>
void doAdvance(IterT& iter, DistT d, std::input_iterator_tag)
{
	if (d < 0) {
		throw std::out_of_range("Negative distance");
	}
	
	while (d--) ++iter;
}

template<typename IterT, typename DistT>
void advance(IterT& iter, DistT d)
{
	doAdvance(iter, d, typename std::iterator_traits<IterT>::iterator_category());
}
```

std::advance用来将某个迭代器移动某个给定距离。STL共有5种迭代器分类，可参考：https://blog.csdn.net/fengbingchun/article/details/77985191

Traits并不是C++关键字或一个预先定义好的构件；它们是一种技术，也是一个C++程序员共同遵守的协议。这个技术的要求之一是，它对内置(built-in)类型和用户自定义(user-defined)类型的表现必须一样好。类型的traits信息必须位于类型自身之外。标准技术是把它放进一个template及其一或多个特化版本中。这样的templates在标准程序库中有若干个，其中针对迭代器者被命名为iterator_traits。

iterator_traits是个struct，习惯上traits总是被实现为structs，但它们却又往往被称为traits classes。iterator_traits的运作方式是，针对每一个类型IterT，在struct iterator_tratis<IterT>内一定声明某个typedef名为iterator_category。这个typedef用来确认IterT的迭代器分类。

设计并实现一个traits class：(1).确认若干你希望将来可取得的类型相关信息。例如对迭代器而言，希望将来可取得其分类(category)。(2).为该信息选择一个名称(例如iterator_category)。(3).提供一个template和一组特化版本(例如iterator_traits)，内含你希望支持的类型相关信息。

如何使用一个traits class：(1).建立一组重载函数(身份像劳工)或函数模板(例如doAdvance)，彼此间的差异只在于各自的traits参数。令每个函数实现码与其接受之traits信息相应和。(2).建立一个控制函数(身份像工头)或函数模板(例如advance)，它调用上述那些”劳工函数”并传递traits class所提供的信息。

Traits广泛用于标准程序库。如iterator_traits，除了供应iterator_category还供应另四份迭代器相关信息(其中最有用的是value_type)。此外还有char_traits用来保存字符类型的相关信息，以及numeric_limits用来保存数值类型的相关信息。

**请记住：(1).Traits classes使得”类型相关信息”在编译期可用。它们以templates和”templates特化”完成实现。(2).整合重载技术(overloading)后，traits classes有可能在编译期对类型进行if…else测试。**

### Item 48. 认识template元编程(Be aware of template metaprogramming)
```cpp
// TMP的阶乘运算, 一般情况:Factorial<n>的值是n乘以Factorial<n-1>的值
template<unsigned n>
struct Factorial {
	enum { value = n * Factorial<n-1>::value };
};

// 特殊情况:Factorial<0>的值是1
template<>
struct Factorial<0> {
	enum { value = 1 };
};

int test_item_48()
{
	fprintf(stdout, "Factorial<5>::value: %d\n", Factorial<5>::value);
	fprintf(stdout, "Factorial<10>::value: %d\n", Factorial<10>::value);

	return 0;
}
```

Template metaprogramming(TMP, 模板元编程)是编写template-based C++程序并执行于编译期的过程。所谓template metaprogram(模板元程序)是以C++写成、执行于C++编译期内的程序。

**请记住：(1).Template metaprogramming(TMP, 模板元编程)可将工作由运行期移往编译期，因而得以实现早期错误侦测和更高的执行效率。(2).TMP可被用来生成”基于政策选择组合”(base on combinations of policy choices)的客户定制代码，也可用来避免生成对某些特殊类型并不适合的代码。**


## 8  定制new和delete(Customizing new and delete)

### Item 49. 了解new-handler的行为(Understand the behavior of the new-handler)

```cpp
// 当operator new无法分配足够内存时，该被调用的函数
void outOfMem()
{
	std::cerr<<"Unable to satisfy request for memory\n";
	std::abort();
}

class NewHandlerHolder {
public:
	explicit NewHandlerHolder(std::new_handler nh) : handler(nh) {} // 取得目前的new-handler
	~NewHandlerHolder() // 释放它
	{ std::set_new_handler(handler); }

private:
	std::new_handler handler; // 记录下来
	NewHandlerHolder(const NewHandlerHolder&); // 阻止copying
	NewHandlerHolder& operator= (const NewHandlerHolder&);
};

// "mixin"风格的base class,用以支持class专属的set_new_handler
template<typename T>
class NewHandlerSupport {
public:
	static std::new_handler set_new_handler(std::new_handler p) throw();
	static void* operator new(std::size_t size) throw(std::bad_alloc);

private:
	static std::new_handler currentHandler;
};

template<typename T>
std::new_handler NewHandlerSupport<T>::set_new_handler(std::new_handler p) throw()
{
	std::new_handler oldHandler = currentHandler;
	currentHandler = p;
	return oldHandler;
}

template<typename T>
void* NewHandlerSupport<T>::operator new(std::size_t size) throw(std::bad_alloc)
{
	NewHandlerHolder h(std::set_new_handler(currentHandler));
	return ::operator new(size);
}

template<typename T>
std::new_handler NewHandlerSupport<T>::currentHandler = NULL;

class Widget49 : public NewHandlerSupport<Widget> {};

int test_item_49()
{
	std::set_new_handler(outOfMem);
	int* pBigDataArray = new int[100000000L];

	Widget49::set_new_handler(outOfMem); // 设定outOfMem为Widget49的new-handling函数
	Widget49* pw1 = new Widget49; // 如果内存分配失败，调用outOfMem
	 
	std::string* ps = new std::string; // 如果内存分配失败，调用global new-handling函数
	 
	Widget49::set_new_handler(NULL); // 设定Widget49专属的new-handling函数为NULL
	Widget49* pw2 = new Widget49; // 如果内存分配失败，立刻抛出异常(clalss Widget49并没有专属的new-handling函数)
	 
	return 0;
}
```

 当operator new无法满足某一内存分配需求时，它会抛出异常。以前它会返回一个null指针。当operator new抛出异常以反映一个未获满足的内存需求之前，它会先调用一个客户指定的错误处理函数，一个所谓的new-handler。为了指定这个”用于处理内存不足”的函数，客户必须调用set_new_handler，那是声明于<new>的标准程序库函数。new-handler是个typedef，定义出一个指针指向函数，该函数没有参数也不返回任何东西。set_new_handler则是”获得一个new-handler并返回一个new-handler”的函数。set_new_handler的参数是个指针，指向operator new无法分配足够内存时该被调用的函数。其返回值也是个指针，指向set_new_handler被调用前正在执行的那个new-handler函数。当operator new无法满足内存申请时，它会不断调用new-handler函数，直到找到足够内存。

设计良好的new-handler函数必须做以下事情：(1).让更多内存可被使用。这便造成operator new内的下一次内存分配动作可能成功。实现此策略的一个做法是，程序一开始执行就分配一大块内存，而后当new-handler第一次被调用，将它们释还给程序使用。(2).安装另一个new-handler。如果目前这个new-handler无法取得更多可用内存，或许它知道另外哪个new-handler有此能力。(3).卸除new-handler，也就是将null指针传给set_new_handler。一旦没有安装任何new-handler, operator new会在内存分配不成功时抛出异常。(4).抛出bad_alloc(或派生自bad_alloc)的异常。这样的异常不会被operator new捕捉，因此会被传播到内存索求处。(5).不返回，通常调用abort或exit。

**请记住：(1).set_new_handler允许客户指定一个函数，在内存分配无法获得满足时被调用。(2).nothow new是一个颇为局限的工具，因为它只适用于内存分配；后继的构造函数调用还是可能抛出异常。**

std::nothrow介绍参考：https://blog.csdn.net/fengbingchun/article/details/63686673

### Item 50. 了解new和delete的合理替换时机(Understand when it makes sense to replace new and delete)

内存对齐介绍参考：https://blog.csdn.net/fengbingchun/article/details/81270326

重载new和delete介绍参考：https://blog.csdn.net/fengbingchun/article/details/78991749

**请记住：有许多理由需要写个自定的new和delete，包括改善效能、对heap运用错误进行调试、收集heap使用信息。**

### Item 51. 编写new和delete时需固守常规(Adhere to convention when writing new and delete)

C++保证”删除null指针永远安全”。

**请记住：(1).operator new应该包含一个无穷循环，并在其中尝试分配内存，如果它无法满足内存需求，就该调用new-handler。它也应该有能力处理0 btyes申请。class专属版本则还应该处理”比正确大小更大的(错误)申请”。(2).operator delete应该在收到null指针时不做任何事。class专属版本则还应该处理”比正确大小更大的(错误)申请”。**

### Item 52. 写了placement new也要写placement delete(Write placement delete if you write placement new)

如果operator new接受的参数除了一定会有的那个size_t之外还有其它，这便是所谓的placement new。”placement new”意味带任意额外参数的new。类似于new的placement版本，operator delete如果接受额外参数，便称为placement delete。

如果一个带额外参数的operator new没有”带相同额外参数”的对应版operator delete，那么当new的内存分配动作需要取消并恢复旧观时就没有任何operator delete会被调用。

**请记住：(1).当你写一个placement operator new，请确定也写出了对应的placement operator delete。如果没有这样做，你的程序可能会发生隐微而时断时续的内存泄漏。(2).当你声明placement new和placement delete，请确定不要无意识(非故意)地遮掩了它们的正常版本。**


## 9 杂项讨论(Miscellany)

### Item 53. 不要轻忽编译器的警告(Pay attention to compiler warnings)

**请记住：(1).严肃对待编译器发出的警告信息。努力在你的编译器的最高(最严苛)警告级别下争取”无任何警告”的荣誉。(2).不要过度依赖编译器的报警能力，因为不同的编译器对待事情的态度并不相同。一旦移植到另一个编译器上，你原本依赖的警告信息有可能消失。**

### Item 54. 让自己熟悉包括RT1在内的标准程序库(Familiarize yourself with the standard library, including TR1)

TR1代表”Technical Report 1”。TR1自身只是一份规范，为获得TR1提供的好处，你需要一份实物，一个好的实物来源是Boost。

std::shared_ptr介绍参考：https://blog.csdn.net/fengbingchun/article/details/52202007

std::weak_ptr介绍参考：https://blog.csdn.net/fengbingchun/article/details/52203825

std::function介绍参考：https://blog.csdn.net/fengbingchun/article/details/52562918

std::bind介绍参考：https://blog.csdn.net/fengbingchun/article/details/52613910

std::unordered_map介绍参考：https://blog.csdn.net/fengbingchun/article/details/52235026

C++11中的正则表达式介绍参考：https://blog.csdn.net/fengbingchun/article/details/54835571

std::tuple介绍参考：https://blog.csdn.net/fengbingchun/article/details/72835446

std::array介绍参考：https://blog.csdn.net/fengbingchun/article/details/72809699

### Item 55. 让自己熟悉Boost(Familiarize yourself with Boost)

**请记住：(1).Boost是一个社群，也是一个网站。致力于免费、源码开放、同僚复审的C++程序库开发。Boost在C++标准化过程中扮演着深具影响力的角色。(2).Boost提供许多TR1组件实现品，以及其它许多程序库。**

GitHub：https://github.com/fengbingchun/Messy_Test





# More Effective C++35个改善编程与设计的有效方法笔记

## 1 基础议题（Basics）

### Item 1. 指针与引用的区别

```cpp
void printDouble(const double& rd)
{
	std::cout<<rd; // 不需要测试rd,它肯定指向一个double值
}
 
void printDouble(const double* pd)
{
	if (pd) { // 检查是否为NULL
		std::cout<<*pd;
	}
}
 
int test_item_1()
{
	char* pc = 0; // 设置指针为空值
	char& rc = *pc; // 让指针指向空值，这是非常有害的，结果将是不确定的
 
	//std::string& rs; // 错误，引用必须被初始化
	std::string s("xyzzy");
	std::string& rs = s; // 正确,rs指向s
	std::string* ps; // 未初始化的指针，合法但危险
 
{
	std::string s1("Nancy");
	std::string s2("Clancy");
	std::string& rs = s1; // rs引用s1
	std::string* ps = &s1; // ps指向s1
	rs = s2; // rs仍旧引用s1,但是s1的值现在是"Clancy"
	ps = &s2; // ps现在指向s2,s1没有改变
}
 
	std::vector<int> v(10);
	v[5] = 10; // 这个被赋值的目标对象就是操作符[]返回的值，如果操作符[]
		   // 返回一个指针，那么后一个语句就得这样写: *v[5] = 10;
 
	return 0;
}
```

指针与引用看上去完全不同(指针用操作符”*”和”->”，引用使用操作符”.”)，但是它们似乎有相同的功能。指针和引用都是让你间接引用其它对象。

**在任何情况下都不能使用指向空值的引用。一个引用必须总是指向某些对象。在C++里，引用应被初始化。**

不存在指向空值的引用这个事实意味着使用引用的代码效率比使用指针的要高。因为在使用引用之前不需要测试它的合法性。

指针与引用的另一个重要的不同是指针可以被重新赋值以指向另一个不同的对象。但是引用则总是指向在初始化时被指定的对象，以后不能改变。

总的来说，在以下情况下你应该使用指针，一是你考虑到存在不指向任何对象的可能(在这种情况下，你能够设置指针为空)，二是你需要能够在不同的时刻指向不同的对象(在这种情况下，你能改变指针的指向)。如果总是指向一个对象并且一旦指向一个对象后就不会改变指向，那么你应该使用引用。

当你知道你必须指向一个对象并且不想改变其指向时，或者在重载操作符并为防止不必要的语义误解时(最普通的例子是操作符[])，你不应该使用指针。而在除此之外的其它情况下，则应使用指针。

关于引用的更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/69820184

### Item 2. 尽量使用C++风格的类型转换

```cpp
class Widget {
public:
	virtual void func() {}
};
 
class SpecialWidget : public Widget {
public:
	virtual void func() {}
};
 
void update(SpecialWidget* psw) {}
void updateViaRef(SpecialWidget& rsw) {}
 
typedef void (*FuncPtr)(); // FuncPtr是一个指向函数的指针
int doSomething() { return 1; };
 
int test_item_2()
{
	int firstNumber = 1, secondNumber = 1;
	double result1 = ((double)firstNumber) / secondNumber; // C风格
	double result2 = static_cast<double>(firstNumber) / secondNumber; // C++风格类型转换
 
	SpecialWidget sw; // sw是一个非const对象
	const SpecialWidget& csw = sw; // csw是sw的一个引用，它是一个const对象
	//update(&csw); // 错误，不能传递一个const SpecialWidget*变量给一个处理SpecialWidget*类型变量的函数
	update(const_cast<SpecialWidget*>(&csw)); // 正确，csw的const显示地转换掉(csw和sw两个变量值在update函数中能被更新)
	update((SpecialWidget*)&csw); // 同上，但用了一个更难识别的C风格的类型转换
 
	Widget* pw = new SpecialWidget;
	//update(pw); // 错误，pw的类型是Widget*，但是update函数处理的是SpecialWidget*类型
	//update(const_cast<SpecialWidget*>(pw)); // 错误，const_cast仅能被用在影响constness or volatileness的地方，不能用在向继承子类进行类型转换
 
	Widget* pw2 = nullptr;
	update(dynamic_cast<SpecialWidget*>(pw2)); // 正确，传递给update函数一个指针是指向变量类型为SpecialWidget的pw2的指针， 如果pw2确实指向一个对象，否则传递过去的将是空指针
 
	Widget* pw3 = new SpecialWidget;
	updateViaRef(dynamic_cast<SpecialWidget&>(*pw3)); // 正确，传递给updateViaRef函数SpecailWidget pw3指针，如果pw3确实指向了某个对象，否则将抛出异常
 
	//double result3 = dynamic_cast<double>(firstNumber) / secondNumber; // 错误，没有继承关系
	const SpecialWidget sw4;
	//update(dynamic_cast<SpecialWidget*>(&sw4)); // 错误，dynamic_cast不能转换掉const
 
	FuncPtr funcPtrArray[10]; // funcPtrArray是一个能容纳10个FuncPtr指针的数组
	//funcPtrArray[0] = &doSomething; // 错误，类型不匹配
	funcPtrArray[0] = reinterpret_cast<FuncPtr>(&doSomething); // 转换函数指针的代码是不可移植的(C++不保证所有的函数指针都被用一样的方法表示)，在一些情况下这样的转换会产生不正确的结果，所以应该避免转换函数指针类型
 
	return 0;
}
```

C++通过引进四个新的类型转换(cast)操作符克服了C风格类型转换的缺点(过于粗鲁，能允许你在任何类型之间进行转换；C风格的类型转换在程序语句中难以识别)，这四个操作符是：static_cast、const_cast、dynamic_cast、reinterpret_cast。

static_cast在功能上基本上与C风格的类型转换一样强大，含义也一样。它也有功能上限制。例如，不能用static_cast像用C 风格的类型转换一样把struct转换成int类型或者把double类型转换成指针类型，另外，static_cast不能从表达式中去除const属性，因为另一个新的类型转换操作符const_cast有这样的功能。

const_cast用于类型转换掉表达式的const或volatileness属性。如果你试图使用const_cast来完成修改constness或者volatileness属性之外的事情，你的类型转换将被拒绝。

**dynamic_cast被用于安全地沿着类的继承关系向下进行类型转换。**这就是说，你能用dynamic_cast把指向基类的指针或引用转换成指向其派生类或其兄弟类的指针或引用，而且你能知道转换是否成功。失败的转换将返回空指针(当对指针进行类型转换时)或者抛出异常(当对引用进行类型转换时)。dynamic_cast在帮助你浏览继承层次上是有限制的，**它不能被用来缺乏虚函数的类型上，也不能用它来转换掉constness**。如你想在没有继承关系的类型中进行转换，你可能想到static_cast。如果是为了去除const，你总得用const_cast。

reinterpret_cast使用这个操作符的类型转换，其转换结果几乎都是执行期定义(implementation-defined)。因此，使用reinterpret_cast的代码很难移植。此操作符最普通的用途就是在函数指针之间进行转换。

关于类型转换更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/51235498

### Item 3. 不要对数组使用多态

```cpp
class BST {
public:
	virtual ~BST() { fprintf(stdout, "BST::~BST\n"); }
private:
	int score;
};
 
class BalancedBST : public BST {
public:
	virtual ~BalancedBST() { fprintf(stdout, "BalancedBST::~BalancedBST\n"); }
private:
	int length;
	int size; // 如果增加此一个int成员，执行test_item_3会segmentation fault，注释掉此变量，运行正常
};
 
int test_item_3()
{
	fprintf(stdout, "BST size: %d\n", sizeof(BST)); // 16
	fprintf(stdout, "BalancedBST size: %d\n", sizeof(BalancedBST)); // 24
 
	BST* p = new BalancedBST[10];
	delete [] p; // 如果sizeof(BST) != sizeof(BalancedBST)，则会segmentation fault
 
	return 0;
}
```
C++允许你通过基类指针和引用来操作派生类数组。不过这根本就不是一个特性，因为这样的代码几乎从不如你所愿地那样运行。数组与多态不能用在一起。值得注意的是如果你不从一个具体类(concrete classes)(例如BST)派生出另一个具体类(例如BalancedBST)，那么你就不太可能犯这种使用多态性数组的错误。

### Item 4. 避免无用的缺省构造函数
```cpp
class EquipmentPiece {
public:
	EquipmentPiece(int IDNumber) {}
};
 
int test_item_4()
{
	//EquipmentPiece bestPieces[10]; // 错误，没有正确调用EquipmentPiece构造函数
	//EquipmentPiece* bestPieces2 = new EquipmentPiece[10]; // 错误，与上面的问题一样
 
	int ID1 = 1, ID2 = 2;
	EquipmentPiece bestPieces3[] = { EquipmentPiece(ID1), EquipmentPiece(ID2) }; // 正确，提供了构造函数的参数
 
	// 利用指针数组来代替一个对象数组
	typedef EquipmentPiece* PEP; // PEP指针指向一个EquipmentPiece对象
	PEP bestPieces4[10]; // 正确，没有调用构造函数
	PEP* bestPieces5 = new PEP[10]; // 也正确
	// 在指针数组里的每一个指针被重新赋值，以指向一个不同的EquipmentPiece对象
	for (int i = 0; i < 10; ++i)
		bestPieces5[i] = new EquipmentPiece(ID1);
 
	// 为数组分配raw memory,可以避免浪费内存，使用placement new方法在内存中构造EquipmentPiece对象
	void* rawMemory = operator new[](10*sizeof(EquipmentPiece));
	// make bestPieces6 point to it so it can be treated as an EquipmentPiece array
	EquipmentPiece* bestPieces6 = static_cast<EquipmentPiece*>(rawMemory);
	// construct the EquipmentPiece objects in the memory使用"placement new"
	for (int i = 0; i < 10; ++i)
		new(&bestPieces6[i]) EquipmentPiece(ID1);
	// ...
	// 以与构造bestPieces6对象相反的顺序解构它
	for (int i = 9; i >= 0; --i)
		bestPieces6[i].~EquipmentPiece(); // 如果使用普通的数组删除方法，程序的运行将是不可预测的
	// deallocate the raw memory
	delete [] rawMemory;
 
	return 0;
}
```
构造函数能初始化对象，而缺省构造函数则可以不利用任何在建立对象时的外部数据就能初始化对象。有时这样的方法是不错的。例如一些行为特性与数字相仿的对象被初始化为空值或不确定的值也是合理的，还有比如链表、哈希表、图等等数据结构也可以被初始化为空容器。但不是所有的对象都属于上述类型，对于很多对象来说，不利用外部数据进行完全的初始化是不合理的。比如一个没有输入姓名的地址薄对象，就没有任何意义。

利用指针数组代替一个对象数组这种方法有两个缺点：第一你必须删除数组里每个指针所指向的对象。如果忘了，就会发生内存泄漏。第二增加了内存分配量，因为正如你需要空间来容纳EquipmentPiece对象一样，你也需要空间来容纳指针。

对于类里没有定义缺省构造函数还会造成它们无法在许多基于模板(template-based)的容器类里使用。因为实例化一个模板时，模板的类型参数应该提供一个缺省构造函数。在多数情况下，通过仔细设计模板可以杜绝对缺省构造函数的需求。


## 2 操作符（Operators） 

### Item 5. 谨慎定义类型转换函数
```cpp
class Name {
public:
	Name(const std::string& s); // 转换string到Name
};
 
class Rational {
public:
	Rational(int numerator = 0, int denominator = 1) // 转换int到有理数类
	{
		n = numerator;
		d = denominator;
	}
 
	operator double() const // 转换Rational类成double类型
	{
		return static_cast<double>(n) / d;
	}
 
	double asDouble() const
	{
		return static_cast<double>(n) / d;
	}
 
private:
	int n, d;
};
 
template<class T>
class Array {
public:
	Array(int lowBound, int highBound) {}
	explicit Array(int size) {}
	T& operator[](int index) { return data[index]; }
 
private:
	T* data;
};
 
bool operator== (const Array<int>& lhs, const Array<int>& rhs)
{ return false; }
 
int test_item_5()
{
	Rational r(1, 2); // r的值是1/2
	double d = 0.5 * r; // 转换r到double,然后做乘法
	fprintf(stdout, "value: %f\n", d);
 
	std::cout<<r<<std::endl; // 应该打印出"1/2",但事与愿违,是一个浮点数，而不是一个有理数,隐式类型转换的缺点
				 // 解决方法是不使用语法关键字的等同的函数来替代转换运算符,如增加asDouble函数，去掉operator double
 
	Array<int> a(10);
	Array<int> b(10);
	for (int i = 0; i < 10; ++i) {
		//if (a == b[i]) {} // 如果构造函数Array(int size)没有explicit关键字，编译器将能通过调用Array<int>构造函数能转换int类型到Array<int>类型，这个构造函数只有一个int类型的参数,加上explicit关键字则可避免隐式转换
 
		if (a == Array<int>(b[i])) {} // 正确，显示从int到Array<int>转换(但是代码的逻辑不合理)
		if (a == static_cast<Array<int>>(b[i]))	 {} // 同样正确，同样不合理
		if (a == (Array<int>)b[i]) {} // C风格的转换也正确，但是逻辑依旧不合理
	}
	return 0;
}
```

C++编译器能够在两种数据类型之间进行隐式转换(implicit conversions)，它继承了C语言的转换方法，例如允许把char隐式转换为int和从short隐式转换为double。你对这些类型转换是无能为力的，因为它们是语言本身的特性。不过当你增加自己的类型时，你就可以有更多的控制力，因为你能选择是否提供函数让编译器进行隐式类型转换。

**有两种函数允许编译器进行这些的转换：单参数构造函数(single-argument constructors)和隐式类型转换运算符。**单参数构造函数是指只用一个参数即可调用的构造函数。该函数可以是只定义了一个参数，也可以是虽定义了多个参数但第一个参数以后的所有参数都有缺省值。

**隐式类型转换运算符只是一个样子奇怪的成员函数：operator关键字，其后跟一个类型符号**。你不用定义函数的返回类型，因为返回类型就是这个函数的名字。

explicit关键字是为了解决隐式类型转换而特别引入的这个特性。**如果构造函数用explicit声明，编译器会拒绝为了隐式类型转换而调用构造函数。显式类型转换依然合法。**

### Item 6. 自增(increment)、自减(decrement)操作符前缀形式与后缀形式的区别

```cpp
class UPInt { // unlimited precision int
public:
	// 注意：前缀与后缀形式返回值类型是不同的，前缀形式返回一个引用，后缀形式返回一个const类型
	UPInt& operator++() // ++前缀
	{
		//*this += 1; // 增加
		i += 1;
		return *this; // 取回值
	}
 
	const UPInt operator++(int) // ++后缀
	{
		// 注意：建立了一个显示的临时对象，这个临时对象必须被构造并在最后被析构，前缀没有这样的临时对象
		UPInt oldValue = *this; // 取回值
		// 后缀应该根据它们的前缀形式来实现
		++(*this); // 增加
		return oldValue; // 返回被取回的值
	}
 
	UPInt& operator--() // --前缀
	{
		i -= 1;
		return *this;
	}
 
	const UPInt operator--(int) // --后缀
	{
		UPInt oldValue = *this;
		--(*this);
		return oldValue;
	}
 
	UPInt& operator+=(int a) // +=操作符，UPInt与int相运算
	{
		i += a;
		return *this;
	}
 
	UPInt& operator-=(int a)
	{
		i -= a;
		return *this;
	}
 
private:
	int i;
}; 
 
int test_item_6()
{
	UPInt i;
	++i; // 调用i.operator++();
	i++; // 调用i.operator++(0);
	--i; // 调用i.operator--();
	i--; // 调用i.operator--(0);
 
	//i++++; // 注意：++后缀返回的是const UPInt
 
	return 0;
}
```
无论是increment或decrement的前缀还是后缀都只有一个参数，为了解决这个语言问题，C++规定后缀形式有一个int类型参数，当函数被调用时，编译器传递一个0作为int参数的值给该函数。

前缀形式有时叫做”增加然后取回”，后缀形式叫做”取回然后增加”。

**当处理用户定义的类型时，尽可能地使用前缀increment，因为它的效率较高。**

### Item 7. 不要重载”&&”, “||”,或”,”
```cpp
int test_item_7()
{
	// if (expression1 && expression2)
	// 如果重载了操作符&&，对于编译器来说，等同于下面代码之一
	// if (expression1.operator&&(expression2)) // when operator&& is a member function
	// if (operator&&(expression1, expression2)) // when operator&& is a global function
 
	return 0;
}
```
与C一样，C++使用布尔表达式短路求值法(short-circuit evaluation)。这表示一旦确定了布尔表达式的真假值，即使还有部分表达式没有被测试，布尔表达式也停止运算。

C++允许根据用户定义的类型，来定制&&和||操作符。方法是重载函数operator&&和operator||，你能在全局重载或每个类里重载。风险：你以函数调用法替代了短路求值法。函数调用法与短路求值法是绝对不同的。首先当函数被调用时，需要运算其所有参数。第二是C++语言规范没有定义函数参数的计算顺序，所以没有办法知道表达式1与表达式2哪一个先计算。完全可能与具有从左参数到右参数计算顺序的短路计算法相反。因此如果你重载&&或||，就没有办法提供给程序员他们所期望和使用的行为特性，所以不要重载&&和||。

同样的理由也适用于逗号操作符。逗号操作符用于组成表达式。一个包含逗号的表达式首先计算逗号左边的表达式，然后计算逗号右边的表达式；整个表达式的结果是逗号右边表达式的值。如果你写一个非成员函数operator，你不能保证左边的表达式先于右边的表达式计算，因为函数(operator)调用时两个表达式作为参数被传递出去。但是你不能控制函数参数的计算顺序。所以非成员函数的方法绝对不行。成员函数operator，你也不能依靠于逗号左边表达式先被计算的行为特性，因为编译器不一定必须按此方法去计算。因此你不能重载逗号操作符，保证它的行为特性与其被料想的一样。重载它是完全轻率的行为。

### Item 8. 理解各种不同含义的new和delete
```cpp
class Widget8 {
public:
	Widget8(int widget8Size) {}
};
 
void* mallocShared(size_t size)
{
	return operator new(size);
}
 
void freeShared(void* memory)
{
	operator delete(memory);
}
 
Widget8* constructWidget8InBuffer(void* buffer, int widget8Size)
{
	return new(buffer) Widget8(widget8Size); // new操作符的一个用法，需要使用一个额外的变量(buffer)，当new操作符隐含调用operator new函数时，把这个变量传递给它
	// 被调用的operator new函数除了待有强制的参数size_t外，还必须接受void*指针参数，指向构造对象占用的内存空间。这个operator new就是placement new,它看上去像这样:
	// void * operator new(size_t, void* location) { return location; }
}
 
int test_item_8()
{
	std::string* ps = new std::string("Memory Management"); // 使用的new是new操作符(new operator)
	//void * operator new(size_t size); // 函数operator new通常声明
	void* rawMemory = operator new(sizeof(std::string)); // 操作符operator new将返回一个指针，指向一块足够容纳一个string类型对象的内存
	operator delete(rawMemory);
 
	delete ps; // ps->~std::string(); operator delete(ps);
 
	void* buffer = operator new(50*sizeof(char)); // 分配足够的内存以容纳50个char，没有调用构造函数
	operator delete(buffer); // 释放内存，没有调用析构函数. 这与在C中调用malloc和free等同OA
 
	void* sharedMemory = mallocShared(sizeof(Widget8));
	Widget8* pw = constructWidget8InBuffer(sharedMemory, 10); // placement new
	//delete pw; // 结果不确定，共享内存来自mallocShared,而不是operator new
	pw->~Widget8(); // 正确，析构pw指向的Widget8,但是没有释放包含Widget8的内存
	freeShared(pw); // 正确，释放pw指向的共享内存，但是没有调用析构函数
 
	return 0;
}
```
**new操作符(new operator)和new操作(operator new)的区别**：

new操作符就像sizeof一样是语言内置的，你不能改变它的含义，它的功能总是一样的。它要完成的功能分成两部分。第一部分是分配足够的内存以便容纳所需类型的对象。第二部分是它调用构造函数初始化内存中的对象。new操作符总是做这两件事情，你不能以任何方式改变它的行为。你所能改变的是如何为对象分配内存。new操作符调用一个函数来完成必须的内存分配，你能够重写或重载这个函数来改变它的行为。new操作符为分配内存所调用函数的名字是operator new。

函数operator new通常声明：返回值类型是void*，因为这个函数返回一个未经处理(raw)的指针，未初始化的内存。参数size_t确定分配多少内存。你能增加额外的参数重载函数operator new，但是第一个参数类型必须是size_t。就像malloc一样，operator new的职责只是分配内存。它对构造函数一无所知。把operator new返回的未经处理的指针传递给一个对象是new操作符的工作。

placement new：特殊的operator new，接受的参数除了size_t外还有其它。

new操作符(new operator)与operator new关系：你想在堆上建立一个对象，应该用new操作符。它既分配内存又为对象调用构造函数。如果你仅仅想分配内存，就应该调用operator new函数，它不会调用构造函数。如果你想定制自己的在堆对象被建立时的内存分配过程，你应该写你自己的operator new函数，然后使用new操作符，new操作符会调用你定制的operator new。如果你想在一块已经获得指针的内存里建立一个对象，应该用placement new。

Deletion and Memory Deallocation：为了避免内存泄漏，每个动态内存分配必须与一个等同相反的deallocation对应。函数operator delete与delete操作符的关系与operator new与new操作符的关系一样。

如果你用placement new在内存中建立对象，你应该避免在该内存中用delete操作符。因为delete操作符调用operator delete来释放内存，但是包含对象的内存最初不是被operator nen分配的，placement new只是返回转到给它的指针。

Arrays：operator new[]、operator delete[]



## 3 异常（Exceptions） 


### Item 9. 使用析构函数防止资源泄漏

用一个对象存储需要被自动释放的资源，然后依靠对象的析构函数来释放资源，这种思想不只是可以运用在指针上，还能用在其它资源的分配和释放上。

资源应该被封装在一个对象里，遵循这个规则，你通常就能够避免在存在异常环境里发生资源泄漏，通过智能指针的方式。

C++确保删除空指针是安全的，所以析构函数在删除指针前不需要检测这些指针是否指向了某些对象。

### Item 10. 在构造函数中防止资源泄漏

C++仅仅能删除被完全构造的对象(fully constructed objects)，只有一个对象的构造函数完全运行完毕，这个对象才被完全地构造。C++拒绝为没有完成构造操作的对象调用析构函数。

在构造函数中可以使用try catch throw捕获所有的异常。更好的解决方法是通过智能指针的方式。

如果你用对应的std::unique_ptr对象替代指针成员变量，就可以防止构造函数在存在异常时发生资源泄漏，你也不用手工在析构函数中释放资源，并且你还能像以前使用非const指针一样使用const指针，给其赋值。

std::unique_ptr的使用参考：https://blog.csdn.net/fengbingchun/article/details/52203664

### Item 11. 禁止异常信息(exceptions)传递到析构函数外

禁止异常传递到析构函数外有两个原因：第一能够在异常传递的堆栈辗转开解(stack-unwinding)的过程中，防止terminate被调用。第二它能帮助确保析构函数总能完成我们希望它做的所有事情。

### Item 12. 理解”抛出一个异常”与”传递一个参数”或”调用一个虚函数”间的差异

你调用函数时，程序的控制权最终还会返回到函数的调用处，但是当你抛出一个异常时，控制权永远不会回到抛出异常的地方。

C++规范要求被作为异常抛出的对象必须被复制。即使被抛出的对象不会被释放，也会进行拷贝操作。抛出异常运行速度比参数传递要慢。

当异常对象被拷贝时，拷贝操作是由对象的拷贝构造函数完成的。该拷贝构造函数是对象的静态类型(static type)所对应类的拷贝构造函数，而不是对象的动态类型(dynamic type)对应类的拷贝构造函数。

catch子句中进行异常匹配时可以进行两种类型转换：第一种是继承类与基类间的转换。一个用来捕获基类的catch子句也可以处理派生类类型的异常。这种派生类与基类(inheritance_based)间的异常类型转换可以作用于数值、引用以及指针上。第二种是允许从一个类型化指针(typed pointer)转变成无类型指针(untyped pointer)，所以带有const void*指针的catch子句能捕获任何类型的指针类型异常。

**catch子句匹配顺序总是取决于它们在程序中出现的顺序。**因此一个派生类异常可能被处理其基类异常的catch子句捕获，即使同时存在有能直接处理该派生类异常的catch子句，与相同的try块相对应。不要把处理基类异常的catch子句放在处理派生类异常的catch子句的前面。

把一个对象传递给函数或一个对象调用虚拟函数与把一个对象作为异常抛出，这之间有三个主要区别：第一，异常对象在传递时总被进行拷贝；当通过传值方式捕获时，异常对象被拷贝了两次。对象作为参数传递给函数时不一定需要被拷贝。第二，对象作为异常被抛出与作为参数传递给函数相比，前者类型转换比后者要少(前者只有两种转换形式)。最后一点，catch子句进行异常类型匹配的顺序是它们在源代码中出现的顺序，第一个类型匹配成功的catch将被用来执行。当一个对象调用一个虚拟函数时，被选择的函数位于与对象类型匹配最佳的类里，即使该类不是在源代码的最前头。

try catch介绍参考：https://blog.csdn.net/fengbingchun/article/details/65939258

### Item 13. 通过引用(reference)捕获异常

通过指针捕获异常不符合C++语言本身的规范。四个标准的异常----bad_alloc(当operator new不能分配足够的内存时被抛出)；bad_cast(当dynamic_cast针对一个引用(reference)操作失败时被抛出)；bad_typeid(当dynamic_cast对空指针进行操作时被抛出)；bad_exception(用于unexpected异常)----都不是指向对象的指针，所以你必须通过值或引用来捕获它们。

std::exception的介绍参考：https://blog.csdn.net/fengbingchun/article/details/78303734

### Item 14. 审慎使用异常规格(exception specifications)

如果一个函数抛出一个不在异常规格范围里的异常，系统在运行时能够检测出这个错误，然后一个特殊函数std::unexpected将被自动地调用(This function is automatically called when a function throws an exception that is not listed in its dynamic-exception-specifier.)。std::unexpected缺省的行为是调用函数std::terminate，而std::terminate缺省的行为是调用函数abort。应避免调用std::unexpected。

避免在带有类型参数的模板内使用异常规格。

C++允许你用其它不同的异常类型替换std::unexpected异常，通过std::set_unexpected。

### Item 15. 了解异常处理的系统开销

采用不支持异常的方法编译的程序一般比支持异常的程序运行速度更快所占空间也更小。

为了减少开销，你应该避免使用无用的try块。如果使用try块，代码的尺寸将增加并且运行速度也会减慢。



## 4 效率（Efficiency）


### Item 16. 牢记80-20准则(80-20 rule)

80-20准则说的是大约20%的代码使用了80%的程序资源；大约20%的代码耗用了大约80%的运行时间；大约20%的代码使用了80%的内存；大约20%的代码执行80%的磁盘访问；80%的维护投入于大约20%的代码上。基本的观点：软件整体的性能取决于代码组成中的一小部分。

### Item 17. 考虑使用lazy evaluation(懒惰计算法)

在某些情况下要求软件进行原来可以避免的计算，这时lazy evaluation才是有用的。

### Item 18. 分期摊还期望的计算

over-eager evaluation(过度热情计算法)：在要求你做某些事情以前就完成它们。隐藏在over-eager evaluation后面的思想是如果你认为一个计算需要频繁进行，你就可以设计一个数据结构高效地处理这些计算需求，这样可以降低每次计算需求时的开销。

当你必须支持某些操作而不总需要其结果时，lazy evaluation是在这种时候使用的用以提高程序效率的技术。当你必须支持某些操作而其结果几乎总是被需要或不止一次地需要时，over-eager是在这种时候使用的用以提高程序效率的一种技术。

### Item 19. 理解临时对象的来源
```cpp
size_t countChar(const std::string& str, char ch)
{
	// 建立一个string类型的临时对象，通过以buffer做为参数调用string的构造函数来初始化这个临时对象,
	// countChar的参数str被绑定在这个临时的string对象上，当countChar返回时，临时对象自动释放
 
	// 将countChar(const std::string& str, char ch)修改为countChar(std::string& str, char ch)则会error
	return 1;
}
 
#define MAX_STRING_LEN 64
 
int test_item_19()
{
	char buffer[MAX_STRING_LEN];
	char c;
 
	std::cin >> c >> std::setw(MAX_STRING_LEN) >> buffer;
	std::cout<<"There are "<<countChar(buffer, c)<<" occurrences of the character "<<c<<" in "<<buffer<<std::endl;
 
	return 0;
}
```
在C++中真正的临时对象是看不见的，它们不出现在你的源代码中。建立一个没有命名的非堆(non-heap)对象会产生临时对象。**这种未命名的对象通常在两种条件下产生：为了使函数成功调用而进行隐式类型转换和函数返回对象时。**

仅当通过传值(by value)方式传递对象或传递常量引用(reference-to-const)参数时，才会发生这些类型转换。当传递一个非常量引用(reference-to-non-const)参数对象，就不会发生。

C++语言禁止为非常量引用(reference-to-non-const)产生临时对象。

临时对象是有开销的，所以你应该尽可能地去除它们。在任何时候只要见到常量引用(reference-to-const)参数，就存在建立临时对象而绑定在参数上的可能性。在任何时候只要见到函数返回对象，就会有一个临时对象被建立(以后被释放)。

### Item 20. 协助完成返回值优化
```cpp
class Rational20 {
public:
	Rational20(int numerator = 0, int denominator = 1) {}
 
	int numerator() const { return 1; }
	int denominator() const { return 2; }
};
 
const Rational20 operator*(const Rational20& lhs, const Rational20& rhs)
{
	// 以某种方法返回对象，能让编译器消除临时对象的开销：这种技巧是返回constructor argument而不是直接返回对象
	return Rational20(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
}
 
int test_item_20()
{
	Rational20 a = 10;
	Rational20 b(1, 2);
	Rational20 c = a * b; 
 
	return 0;
}
```

一些函数(operator*也在其中)必须要返回对象。这就是它们的运行方法。

C++规则允许编译器优化不出现的临时对象(temporary objects out of existence)。

### Item 21. 通过重载避免隐式类型转换

```cpp
class UPInt21 { // unlimited precision integers class
public:
	UPInt21() {}
	UPInt21(int value) {}
};
 
const UPInt21 operator+(const UPInt21& lhs, const UPInt21& rhs) // add UPInt21+UPInt21
{
	return UPInt21(1);
}
 
const UPInt21 operator+(const UPInt21& lhs, int rhs) // add UPInt21+int
{
	return UPInt21(1);
}
 
const UPInt21 operator+(int lhs, const UPInt21& rhs) // add int+UPInt21
{
	return UPInt21(1);
}
 
int test_item_21()
{
	UPInt21 upi1, upi2;
	UPInt21 upi3 = upi1 + upi2; // 正确，没有由upi1或upi2生成临时对象
	upi3 = upi1 + 10; // 正确,没有由upi1或10生成临时对象
	upi3 = 10 + upi2; // 正确，没有由10或upi2生成临时对象
 
	// 注意：注释掉上面的operator+(UPInt21&, int)和operator+(int, UPInt21&)也正确，但是会通过临时对象把10转换为UPInt21
 
	return 0;
}
```

在C++中有一条规则是每一个重载的operator必须带有一个用户定义类型(user-defined type)的参数。

利用重载避免临时对象的方法不只是用在operator函数上。

没有必要实现大量的重载函数，除非你有理由确信程序使用重载函数以后其整体效率会有显著的提高。

### Item 22. 考虑用运算符的赋值形式(op=)取代其单独形式(op)
```cpp
class Rational22 {
public:
	Rational22(int numerator = 0, int denominator = 1) {}
	Rational22& operator+=(const Rational22& rhs) { return *this; }
	Rational22& operator-=(const Rational22& rhs) { return *this; }
};
 
// operator+根据operator+=实现
const Rational22 operator+(const Rational22& lhs, const Rational22& rhs)
{
	return Rational22(lhs) += rhs;
}
 
// operator-根据operator-=实现
const Rational22 operator-(const Rational22& lhs, const Rational22& rhs)
{
	return Rational22(lhs) -= rhs;
}
```

就C++来说，operator+、operator=和operator+=之间没有任何关系，因此如果你想让三个operator同时存在并具有你所期望的关系，就必须自己实现它们。同理，operator-, *, /, 等等也一样。

确保operator的赋值形式(assignment version)(例如operator+=)与一个operator的单独形式(stand-alone)(例如operator+)之间存在正常的关系，一种好方法是后者(指operator+)根据前者(指operator+=)来实现。

### Item 23. 考虑变更程序库

不同的程序库在效率、可扩展性、移植性、类型安全和其它一些领域上蕴含着不同的设计理念，通过变换使用给予性能更多考虑的程序库，你有时可以大幅度地提供软件的效率。

### Item 24. 理解虚拟函数、多继承、虚基类和RTTI所需的代码

当调用一个虚拟函数时，被执行的代码必须与调用函数的对象的动态类型相一致；指向对象的指针或引用的类型是不重要的。大多数编译器是使用virtual table和virtual table pointers，通常被分别地称为vtbl和vptr。

一个vtbl通常是一个函数指针数组。(一些编译器使用链表来代替数组，但是基本方法是一样的)在程序中的每个类只要声明了虚函数或继承了虚函数，它就有自己的vtbl，并且类中vtbl的项目是指向虚函数实现体的指针。

你必须为每个包含虚函数的类的virtual table留出空间。类的vtbl的大小与类中声明的虚函数的数量成正比(包括从基类继承的虚函数)。**每个类应该只有一个virtual table，所以virtual table所需的空间不会太大**，但是如果你有大量的类或者在每个类中有大量的虚函数，你会发现vtbl会占用大量的地址空间。

一些原因导致现在的编译器一般总是忽略虚函数的inline指令。

Virtual table只实现了虚拟函数的一半机制，如果只有这些是没有用的。只有用某种方法指出每个对象对应的vtbl时，它们才能使用。这是virtual table pointer的工作，它来建立这种联系。每个声明了虚函数的对象都带着它，它是一个看不见的数据成员，指向对应类的virtual table。这个看不见的数据成员也称为vptr，被编译器加在对象里，位置只有编译器知道。

关于虚函数表的介绍参考：https://blog.csdn.net/fengbingchun/article/details/79592347

**虚函数是不能内联的**。这是因为”内联”是指”在编译期间用被调用的函数体本身来代替函数调用的指令”，但是虚函数的”虚”是指”直到运行时才能知道要调用的是哪一个函数”。

RTTI(运行时类型识别)能让我们在运行时找到对象和类的有关信息，所以肯定有某个地方存储了这些信息让我们查询。这些信息被存储在类型为type_info的对象里，你能通过使用typeid操作符访问一个类的type_info对象。

关于typeid的使用参考：https://blog.csdn.net/fengbingchun/article/details/51866559

RTTI被设计为在类的vtbl基础上实现。



## 5 技术（Techniques， Idioms， Patterns）


### Item 25. 将构造函数和非成员函数虚拟化

虚拟构造函数是指能够根据输入给它的数据的不同而建立不同类型的对象。虚拟拷贝构造函数能返回一个指针，指向调用该函数的对象的新拷贝。类的虚拟拷贝构造函数只是调用它们真正的拷贝构造函数。**被派生类重定义的虚拟函数不用必须与基类的虚拟函数具有一样的返回类型。**如果函数的返回类型是一个指向基类的指针(或一个引用)，那么派生类的函数可以返回一个指向基类的派生类的指针(或引用)。

### Item 26. 限制某个类所能产生的对象数量

阻止建立某个类的对象，最容易的方法就是把该类的构造函数声明在类的private域。

### Item 27. 要求或禁止在堆中产生对象

```cpp
// 判断一个对象是否在堆中, HeapTracked不能用于内建类型，因为内建类型没有this指针
typedef const void* RawAddress;
class HeapTracked { // 混合类，跟踪
public:
	class MissingAddress {}; // 从operator new返回的ptr异常类
	virtual ~HeapTracked() = 0;
	static void* operator new(size_t size);
	static void operator delete(void* ptr);
	bool isOnHeap() const;
 
private:
	static std::list<RawAddress> addresses;
};
 
std::list<RawAddress> HeapTracked::addresses;
 
HeapTracked::~HeapTracked() {}
 
void* HeapTracked::operator new(size_t size)
{
	void* memPtr = ::operator new(size);
	addresses.push_front(memPtr);
	return memPtr;
}
 
void HeapTracked::operator delete(void* ptr)
{
	std::list<RawAddress>::iterator it = std::find(addresses.begin(), addresses.end(), ptr);
	if (it != addresses.end()) {
		addresses.erase(it);
		::operator delete(ptr);
	} else {
		throw MissingAddress(); // ptr就不是用operator new分配的，所以抛出一个异常
	}
}
 
bool HeapTracked::isOnHeap() const
{
	// 生成的指针将指向"原指针指向对象内存"的开始处
	// 如果HeapTracked::operator new为当前对象分配内存，这个指针就是HeapTracked::operator new返回的指针
	const void* rawAddress = dynamic_cast<const void*>(this);
	std::list<RawAddress>::iterator it = std::find(addresses.begin(), addresses.end(), rawAddress);
	return it != addresses.end();
}
 
class Asset : public HeapTracked {};
 
// 禁止堆对象
class UPNumber27 {
private:
	static void* operator new(size_t size);
	static void operator delete(void* ptr);
};
 
void* UPNumber27::operator new(size_t size)
{
	return ::operator new(size);
}
 
void UPNumber27::operator delete(void* ptr)
{
	::operator delete(ptr);
}
 
class Asset27 {
public:
	Asset27(int initValue) {}
 
private:
	UPNumber27 value;
};
 
int test_item_27()
{
	UPNumber27 n1; // okay
	static UPNumber27 n2; // also okay
	//UPNumber27* p = new UPNumber27; // error, attempt to call private operator new
 
	// UPNumber27的operator new是private这一点 不会对包含UPNumber27成员对象的对象的分配产生任何影响
	Asset27* pa = new Asset27(100); // 正确，调用Asset::operator new或::operator new,不是UPNumber27::operator new
 
	return 0;
}
```


禁止堆对象：禁止用于调用new，利用new操作符总是调用operator new函数这点来达到目的，可以自己声明这个函数，而且你可以把它声明为private。

### Item 28. 灵巧(smart)指针

```cpp
// 大多数灵巧指针模板
template<class T>
class SmartPtr {
public:
	SmartPtr(T* realPtr = 0); // 建立一个灵巧指针指向dumb pointer(内建指针)所指的对象，未初始化的指针，缺省值为0(null)
	SmartPtr(const SmartPtr& rhs); // 拷贝一个灵巧指针
	~SmartPtr(); // 释放灵巧指针
	// make an assignment to a smart ptr
	SmartPtr& operator=(const SmartPtr& rhs);
	T* operator->() const; // dereference一个灵巧指针以访问所指对象的成员
	T& operator*() const; // dereference灵巧指针
 
private:
	T* pointee; // 灵巧指针所指的对象
};
```

灵巧指针是一种外观和行为都被设计成与内建指针相类似的对象，不过它能提供更多的功能。它们有许多应用的领域，包括资源管理和重复代码任务的自动化。

在C++11中auto_ptr已经被废弃，用unique_ptr替代。

std::unique_ptr的使用参考：https://blog.csdn.net/fengbingchun/article/details/52203664

### Item 29. 引用计数
```cpp
class String {
public:
	String(const char* initValue = "");
	String(const String& rhs);
	String& operator=(const String& rhs);
	const char& operator[](int index) const; // for const String
	char& operator[](int index); // for non-const String
	~String();
 
private:
	// StringValue的主要目的是提供一个空间将一个特别的值和共享此值的对象的数目联系起来
	struct StringValue { // holds a reference count and a string value
		int refCount;
		char* data;
		bool shareable; // 标志，以指出它是否为可共享的
		StringValue(const char* initValue);
		~StringValue();
	};
 
	StringValue* value; // value of this String
};
 
String::String(const char* initValue) : value(new StringValue(initValue))
{}
 
String::String(const String& rhs)
{
	if (rhs.value->shareable) {
		value = rhs.value;
		++value->refCount;
	} else {
		value = new StringValue(rhs.value->data);
	}
}
 
String& String::operator=(const String& rhs)
{
	if (value == rhs.value) { // do nothing if the values are already the same
		return *this;
	}
 
	if (value->shareable && --value->refCount == 0) { // destroy *this's value if no one else is using it
		delete value;
	}
 
	if (rhs.value->shareable) {
		value = rhs.value; // have *this share rhs's value
		++value->refCount;
	} else {
		value = new StringValue(rhs.value->data);
	}
 
	return *this;
}
 
const char& String::operator[](int index) const
{
	return value->data[index];
}
 
char& String::operator[](int index)
{
	// if we're sharing a value with other String objects, break off a separate copy of the value fro ourselves
	if (value->refCount > 1) {
		--value->refCount; // decrement current value's refCount, becuase we won't be using that value any more
		value = new StringValue(value->data); // make a copy of the value for ourselves
	}
 
	value->shareable = false;
	// return a reference to a character inside our unshared StringValue object
	return value->data[index];
}
 
String::~String()
{
	if (--value->refCount == 0) {
		delete value;
	}
}
 
String::StringValue::StringValue(const char* initValue) : refCount(1), shareable(true)
{
	data = new char[strlen(initValue) + 1];
	strcpy(data, initValue);
}
 
String::StringValue::~StringValue()
{
	delete[] data;
}
```

```cpp
// 基类，任何需要引用计数的类都必须从它继承
class RCObject {
public:
	void addReference() { ++refCount; }
	void removeReference() { if (--refCount == 0) delete this; } // 必须确保RCObject只能被构建在堆中
	void markUnshareable() { shareable = false; }
	bool isShareable() const { return shareable; }
	bool isShared() const { return refCount > 1; }
 
protected:
	RCObject() : refCount(0), shareable(true) {}
	RCObject(const RCObject& rhs) : refCount(0), shareable(true) {}
	RCObject& operator=(const RCObject& rhs) { return *this; }
	virtual ~RCObject() = 0;
 
private:
	int refCount;
	bool shareable;
 
};
 
RCObject::~RCObject() {} // virtual dtors must always be implemented, even if they are pure virtual and do nothing
 
// template class for smart pointers-to-T objects. T must support the RCObject interface, typically by inheriting from RCObject
template<class T>
class RCPtr {
public:
	RCPtr(T* realPtr = 0) : pointee(realPtr) { init(); }
	RCPtr(const RCPtr& rhs) : pointee(rhs.pointee) { init(); }
	~RCPtr() { if (pointee) pointee->removeReference(); }
 
	RCPtr& operator=(const RCPtr& rhs)
	{
		if (pointee != rhs.pointee) { // skip assignments where the value doesn't change
			if (pointee)
				pointee->removeReference(); // remove reference to current value
 
			pointee = rhs.pointee; // point to new value
			init(); // if possible, share it else make own copy
		}
 
		return *this;
	}
 
	T* operator->() const { return pointee; }
	T& operator*() const { return *pointee; }
 
private:
	T* pointee; // dumb pointer this object is emulating
 
	void init() // common initialization
	{
		if (pointee == 0) // if the dumb pointer is null, so is the smart one
			return;
 
		if (pointee->isShareable() == false) // if the value isn't shareable copy it
			pointee = new T(*pointee);
 
		pointee->addReference(); // note that there is now a new reference to the value
	}
};
 
// 将StringValue修改为是从RCObject继承
// 将引用计数功能移入一个新类(RCObject)，增加了灵巧指针(RCPtr)来自动处理引用计数
class String2 {
public:
	String2(const char* value = "") : value(new StringValue(value)) {}
	const char& operator[](int index) const { return value->data[index]; } // for const String2
	
	char& operator[](int index) // for non-const String2
	{
		if (value->isShared())
			value = new StringValue(value->data);
		value->markUnshareable();
		return value->data[index];
	}
 
private:
	// StringValue的主要目的是提供一个空间将一个特别的值和共享此值的对象的数目联系起来
	struct StringValue : public RCObject { // holds a reference count and a string value
		char* data;
 
		StringValue(const char* initValue) { init(initValue); }
		StringValue(const StringValue& rhs) { init(rhs.data); }
 
		void init(const char* initValue)
		{
			data = new char[strlen(initValue) + 1];
			strcpy(data, initValue);
		}
 
		~StringValue() { delete [] data; }
	};
 
	RCPtr<StringValue> value; // value of this String2
 
};
 
int test_item_29()
{
	String s1("More Effective C++");
	String s2 = s1;
	s1 = s2;
	fprintf(stdout, "char: %c\n", s1[2]);
	String s3 = s1;
	s3[5] = 'x';
 
	return 0;
}
```
引用计数是这样一个技巧，它允许多个有相同值的对象共享这个值的实现。这个技巧有两个常用动机。第一个是简化跟踪堆中的对象的过程。一旦一个对象通过调用new被分配出来，最要紧的就是记录谁拥有这个对象，因为其所有者----并且只有其所有者----负责对这个对象调用delete。但是，所有权可以被从一个对象传递到另外一个对象(例如通过传递指针型参数)。引用计数可以免除跟踪对象所有权的担子，因为当使用引用计数后，对象自己拥有自己。当没人再使用它时，它自己自动销毁自己。因此，引用计数是个简单的垃圾回收体系。第二个动机是由于一个简单的常识。如果很多对象有相同的值，将这个值存储多次是很无聊的。更好的办法是让所有的对象共享这个值的实现。这么做不但节省内存，而且可以使得程序运行更快，因为不需要构造和析构这个值的拷贝。

引用计数介绍参考：https://blog.csdn.net/fengbingchun/article/details/85861776

实现引用计数不是没有代价的。每个被引用的值带一个引用计数，其大部分操作都需要以某种形式检查或操作引用计数。对象的值需要更多的内存，而我们在处理它们时需要执行更多的代码。引用计数是基于对象通常共享相同的值的假设的优化技巧。如果假设不成立的话，引用计数将比通常的方法使用更多的内存和执行更多的代码。另一方面，如果你的对象确实有具有相同值的趋势，那么引用计数将同时节省时间和空间。

### Item 30. 代理类

```cpp
template<class T>
class Array2D { // 使用代理实现二维数组
public:
	Array2D(int i, int j) : i(i), j(j)
	{
		data.reset(new T[i*j]);
	}
 
	class Array1D { // Array1D是一个代理类，它的实例扮演的是一个在概念上不存在的一维数组
	public:
		Array1D(T* data) : data(data) {}
		T& operator[](int index) { return data[index]; }
		const T& operator[](int index) const { return data[index]; }
 
	private:
		T* data;
	};
 
	Array1D operator[](int index) { return Array1D(data.get()+j*index); }
	const Array1D operator[](int index) const { return Array1D(data.get()+j*index); }
 
private:
	std::unique_ptr<T[]> data;
	int i, j;
};
 
// 可以通过代理类帮助区分通过operator[]进行的是读操作还是写操作
class String30 {
public:
	String30(const char* value = "") : value(new StringValue(value)) {}
	
	class CharProxy { // proxies for string chars
	public:
		CharProxy(String30& str, int index) : theString(str), charIndex(index) {}
 
		CharProxy& operator=(const CharProxy& rhs)
		{
			// if the string is haring a value with other String objects,
			// break off a separate copy of the value for this string only
			if (theString.value->isShared())
				theString.value = new StringValue(theString.value->data);
 
			// now make the assignment: assign the value of the char
			// represented by rhs to the char represented by *this
			theString.value->data[charIndex] = rhs.theString.value->data[rhs.charIndex];
			return *this;
		}
		
		CharProxy& operator=(char c)
		{
			if (theString.value->isShared())
				theString.value = new StringValue(theString.value->data);
			theString.value->data[charIndex] = c;
			return *this;
		}
 
		operator char() const { return theString.value->data[charIndex]; }
 
	private:
		String30& theString;
		int charIndex;
	};
 
	const CharProxy operator[](int index) const // for const String30
	{
		return CharProxy(const_cast<String30&>(*this), index);
	}
 
	CharProxy operator[](int index) // for non-const String30
	{
		return CharProxy(*this, index);
	}
 
	//friend class CharProxy;
private:
	// StringValue的主要目的是提供一个空间将一个特别的值和共享此值的对象的数目联系起来
	struct StringValue : public RCObject { // holds a reference count and a string value
		char* data;
 
		StringValue(const char* initValue) { init(initValue); }
		StringValue(const StringValue& rhs) { init(rhs.data); }
 
		void init(const char* initValue)
		{
			data = new char[strlen(initValue) + 1];
			strcpy(data, initValue);
		}
 
		~StringValue() { delete [] data; }
	};
 
	RCPtr<StringValue> value; // value of this String30
 
};
 
int test_item_30()
{
	Array2D<float> data(10, 20);
	fprintf(stdout, "%f\n", data[3][6]);
 
	String30 s1("Effective C++"), s2("More Effective C++"); // reference-counted strings using proxies
	fprintf(stdout, "%c\n", s1[5]); // still legal, still works
	s2[5] = 'x'; // also legal, also works
	s1[3] = s2[8]; // of course it's legal, of course it works
 
	//char* p = &s1[1]; // error, 通常,取proxy对象地址的操作与取实际对象地址的操作得到的指针，其类型是不同的,重载CharProxy类的取地址运算可消除这个不同
 
	return 0;
}
```

可以通过代理类实现二维数组。

可以通过代理类帮助区分通过operator[]进行的是读操作还是写操作。

Proxy类可以完成一些其它方法很难甚至可不能实现的行为。多维数组是一个例子，左值/右值的区分是第二个，限制隐式类型转换是第三个。

同时，proxy类也有缺点。作为函数返回值，proxy对象是临时对象，它们必须被构造和析构。Proxy对象的存在增加了软件的复杂度。从一个处理实际对象的类改换到处理proxy对象的类经常改变了类的语义，因为proxy对象通常表现出的行为与实际对象有些微妙的区别。

### Item 31. 让函数根据一个以上的对象来决定怎么虚拟



## 6 杂项讨论（Miscellany）

### Item 32. 在未来时态下开发程序

未来时态的考虑增加了你的代码的可重用性、可维护性、健壮性，以及在环境发生改变时易于修改。

### Item 33. 将非尾端类设计为抽象类

### Item 34. 如何在同一程序中混合使用C++和C

名变换：就是C++编译器给程序的每个函数换一个独一无二的名字。在C中，这个过程是不需要的，因为没有函数重载，但几乎所有C++程序都有函数重名。要禁止名变换，使用C++的extern “C”。不要将extern “C”看作是声明这个函数是用C语言写的，应该看作是声明这个函数应该被当作好像C写的一样而进行调用。

静态初始化：在main执行前和执行后都有大量代码被执行。尤其是，静态的类对象和定义在全局的、命名空间中的或文件体中的类对象的构造函数通常在main被执行前就被调用。这个过程称为静态初始化。同样，通过静态初始化产生的对象也要在静态析构过程中调用其析构函数，这个过程通常发生在main结束运行之后。

动态内存分配：C++部分使用new和delete，C部分使用malloc(或其变形)和free。

数据结构的兼容性：在C++和C之间这样相互传递数据结构是安全的----在C++和C下提供同样的定义来进行编译。在C++版本中增加非虚成员函数或许不影响兼容性，但几乎其它的改变都将影响兼容。

如果想在同一程序下混合C++与C编程，记住下面的指导原则：(1).确保C++和C编译器产生兼容的obj文件；(2).将在两种语言下都使用的函数声明为extern “C”；(3).只要可能，用C++写main()；(4).总用delete释放new分配的内存；总用free释放malloc分配的内存；(5).将在两种语言间传递的东西限制在用C编译的数据结构的范围内；这些结构的C++版本可以包含非虚成员函数。

### Item 35. 让自己习惯使用标准C++语言






# 3 Effective STL 50条有效使用STL的经验笔记



这里是Effective STL的笔记：

注：(1).书中少许内容已不再适合现代C++的开发，有些算法或函数在C++11中已不推荐使用，在下文涉及到的地方已标注。(2).有些测试代码使用了C++11特性。

STL表示C++标准库中与迭代器一起工作的那部分，其中包括标准容器(包含string)、iostream库的一部分、函数对象和各种算法。STL主要组成部分包括：容器、迭代器、算法、函数对象。

根据迭代器所支持的操作，可以把迭代器分为五类：

(1). 输入迭代器(input iterator)是只读迭代器，在每个被遍历到的位置上只能被读取一次。

(2). 输出迭代器(output iterator)是只写迭代器，在每个被遍历到的位置上只能被写入一次。

输入和输出迭代器的模型分别是建立在针对输入和输出流(例如文件)的读写操作的基础上的。所以不难理解，输入和输出迭代器最常见的表现形式是istream_iterator和ostream_iterator。

(3). 前向迭代器(forward iterator)兼具输入和输出迭代器的能力，但是它可以对同一个位置重复进行读和写。前向迭代器不支持operator--，所以它只能向前移动。所有的标准STL容器都支持比前向迭代器功能更强大的迭代器。

(4). 双向迭代器(bidirectional iterator)很像前向迭代器，只是它们向后移动和向前移动同样容易。标准关联容器都提供了双向迭代器。list也是如此。

(5). 随机访问迭代器(random access iterator)有双向迭代器的所有更能，而且，它还提供了”迭代器算术”，即在一步内向前或向后跳跃的能力。vector、string和deque都提供了随机访问迭代器。指向数组内部的指针对于数组来说也是随机访问迭代器。

**所有重载了函数调用操作符(即operator())的类都是一个函数子类(functor class)。从这些类创建的对象被称为函数对象(function object)或函数子(functor)。在STL中，大多数使用函数对象的地方同样也可以使用实际的函数。**

## 1 容器

### Item 1. 慎重选择容器类型

STL中有迭代器(iterator)、算法(algorithm)和函数对象(function object)。但是对于大多数C++程序员来说，最值得注意的还是容器。容器比数组功能更强大、更灵活。它们可以动态增长(和缩减)，可以自己管理内存，可以记住自己包含了多少对象。它们限定了自己所支持的操作的复杂性。

标准STL序列容器：vector、string、deque、list、forward_list(C++11)、array(C++11)。

标准STL关联容器：set、multiset、map、multimap、unordered_set(C++11)、unordered_multiset(C++11)、unordered_map(C++11)、unordered_multimap(C++11)。

标准的非STL容器，包括：bitset(include <bitset>)、valarray(include <valarray>)。其它STL容器：stack(include <stack>)、queue(include <queue>)和priority_queue((include <queue>))。

vector<char>可以作为string的替代。vector作为标准关联容器的替代。有时vector在运行时间和空间上都要优于标准关联容器。

vector是默认应使用的序列类型，当需要频繁地在序列中间做插入和删除操作时，应使用list；当大部分插入和删除操作发生在序列的头部和尾部时， deque是应考虑的数据结构。

**STL容器的一种分类方法**：连续内存容器(contiguous memory container)和基于节点的容器(node-based container)。

(1).连续内存容器(或称为基于数组的容器，array-based container)把它的元素存放在一块或多块(动态分配的)内存中，每块内存中存有多个元素。当有新元素插入或已有的元素被删除时，同一内存块中的其它元素要向前或向后移动，以便为新元素让出空间，或者填充被删除元素所留下的空隙。这种移动影响到效率和异常安全性。标准的连续内存容器有vector、string、deque。

(2).基于节点的容器在每一个(动态分配的)内存块中只存放一个元素。容器中元素的插入或删除只影响到指向节点的指针，而不影响节点本身的内容，所以当有插入或删除操作时，元素的值不需要移动。表示链表的容器，比如list、forward_list，是基于节点的；所有的标准关联容器也是如此(通常的实现方式是平衡树)。标准的哈希容器使用不同的基于节点的实现。

选择容器时最重要的一些问题：

(1). 你是否需要在容器的任意位置插入新元素？如果需要，就选择序列容器；关联容器是不行的。

(2). 你是否关心容器中的元素是如何排序的？如果不关心，则哈希容器是一个可行的选择方案；否则，你要避免哈希容器。

(3). 你选择的容器必须是标准C++的一部分吗？如果必须是，就排除了slist和rope。

(4). 你需要哪种类型的迭代器？如果它们必须是随机访问迭代器，则对容器的选择就被限定为vector、deque和string。或许你也可以考虑rope。如果要求使用双向迭代器，那么你必须避免slist以及哈希容器的一个常见实现。

(5). 当发生元素的插入或删除操作时，避免移动容器中原来的元素是否很重要？如果是，就要避免连续内存的容器。

(6). 容器中数据的布局是否需要和C兼容？如果需要兼容，就只能选择vector。

(7). 元素的查找速度是否是关键的考虑因素？如果是，就要考虑哈希容器、排序的vector和标准关联容器----或许这就是优先顺序。

(8). 如果容器内部使用了引用计数技术(reference counting)，你是否介意？如果是，就要避免使用string，因为许多string的实现都使用了引用计数。rope也需要避免，因为权威的rope实现是基于引用计数的。当然，你需要某种表示字符串的方法，这时你可以考虑vector<char>。

(9). 对插入和删除操作，你需要事务语义(transactional semantics)吗？也就是说，在插入和删除操作失败时，你需要回滚的能力吗？如果需要，你就要使用基于节点的容器。如果对多个元素的插入操作(即针对一个区间的形式)需要事务语义，则你需要选择list，因为在标准容器中，只有list对多个元素的插入操作提供了事务语义。对那些希望编写异常安全(exception-safe)代码的程序员，事务语义显得尤为重要。(使用连续内存的容器也可以获得事务语义，但是要付出性能上的代价，而且代码也显得不那么直截了当。)

(10). 你需要使迭代器、指针和引用变为无效的次数最少吗？如果是这样，就要使用基于节点的容器，因为对这类容器的插入和删除操作从来不会使迭代器、指针和引用变得无效(除非它们指向了一个你正在删除的元素)。而针对连续内存容器的插入和删除操作一般会使指向该容器的迭代器、指针和引用变得无效。

(11). 如果序列容器的迭代器是随机访问类型，而且只要没有删除操作发生，且插入操作只发生在容器的末尾，则指向数据的指针和引用就不会变为无效，这样的容器是否对你有帮助？这是非常特殊的情形，但如果你面对的情形正是如此，则deque是你所希望的容器。(当插入操作仅在容器末尾发生时，deque的迭代器有可能会变为无效。deque是唯一的、迭代器可能会变为无效而指针和引用不会变为无效的STL标准容器。)

### Item 2. 不要试图编写独立于容器类型的代码
```cpp
int test_item_2()
{
	// 对容器类型和其迭代器类型使用类型定义(typedef)
	typedef std::vector<int> WidgetContainer;
	typedef WidgetContainer::iterator WCIterator;
	WidgetContainer cw;
	int bestWidget;
 
	WCIterator i = std::find(cw.begin(), cw.end(), bestWidget);
 
	return 0;
}
```

**STL是以泛化(generalization)原则为基础的**：数组被泛化为”以其包含的对象的类型为参数”的容器，函数被泛化为”以其使用的迭代器的类型为参数”的算法，指针被泛化为”以其指向的对象的类型为参数”的迭代器。

容器类型被泛化为序列和关联容器，类似的容器被赋予相似的功能。标准的连续内存容器提供了随机访问迭代器，而标准的基于节点的容器提供了双向迭代器。序列容器支持push_front和/或push _back操作，而关联容器则不然。关联容器提供了对数时间的lower_bound、upper_bound和equal_range成员函数，但序列容器却没有提供。

试图编写独立于容器的代码(container independent code)，这类泛化，尽管出发点是好的，却几乎总是误入歧途。试图编写对序列容器和关联容器都适用的代码几乎是毫无意义的。很多成员函数仅当其容器为某一种类型时才存在。不同的容器是不同的，它们有非常明显的优缺点。它们并不是被设计来交换适用的。

类型定义只不过是其它类型的别名，所以它带来的封装纯粹是词法(lexical)上的。类型定义并不能阻止一个客户去做(或依赖)它们原本无法做到(或依赖)的事情。

要想减少在替换容器类型时所需要修改的代码，你可以把容器隐藏到一个类中，并尽量减少那些通过类接口(而使外部)可见的、与容器相关的信息。

### Item 3. 确保容器中的对象拷贝正确而高效
```cpp
class Widget {};
class SpecialWidget : public Widget {};
 
int test_item_3()
{
	std::vector<Widget> vw;
	SpecialWidget sw;
	vw.push_back(sw); // sw作为基类对象被拷贝进vw中，它的派生类特有部分在拷贝时被丢掉了
 
	return 0;
}
```
当(通过如insert或push_back之类的操作)向容器中加入对象时，存入容器的是你所指定的对象的拷贝。当(通过如front或back之类的操作)从容器中取出一个对象时，你所得到的是容器中所保存的对象的拷贝。进去的是拷贝，出来的也是拷贝(copy in, copy out)。这就是STL的工作方式。

一旦一个对象被保存到容器中，它经常会进一步被拷贝。当对vector、string或deque进行元素的插入或删除操作时，现有元素的位置通常会被移动(拷贝)。如果你使用下列任何操作----排序算法，next_permutation或previous_permutation, remove、unique或类似的操作，rotate或reverse,等等----那么对象将会被移动(拷贝)。没错，拷贝对象是STL的工作方式。

利用一个对象的拷贝成员函数就可以很方便地拷贝该对象，特别是对象的拷贝构造函数(copy constructor)和拷贝赋值操作符(copy assignment operator)。

在存在继承关系的情况下，拷贝动作会导致剥离(slicing)。也就是说，如果你创建了一个存放基类对象的容器，却向其中插入派生类的对象，那么在派生类对象(通过基类的拷贝构造函数)被拷贝进容器时，它所特有的部分(即派生类中的信息)将会丢失。**”剥离”问题意味着向基类对象的容器中插入派生类对象几乎总是错误的。使拷贝动作高效、正确，并防止剥离问题发生的一个简单办法是使容器包含指针而不是对象。**

### Item 4. 调用empty而不是检查size()是否为0

empty对所有的标准容器都是常数时间操作，而对一些list实现，size耗费线性时间。

### Item 5. 区间成员函数优先于与之对应的单元素成员函数
```cpp
class Widget5 {};
 
int test_item_5()
{
	std::vector<Widget5> v1, v2;
	v1.assign(v2.begin() + v2.size() / 2, v2.end()); // 推荐
 
	v1.clear();
	for (std::vector<Widget5>::const_iterator ci = v2.begin() + v2.size() / 2; ci != v2.end(); ++ci) // 不推荐
		v1.push_back(*ci);
 
	v1.clear();
	std::copy(v2.begin() + v2.size() / 2, v2.end(), std::back_inserter(v1)); // 效率不如assign
 
	v1.clear();
	v1.insert(v1.end(), v2.begin() + v2.size() / 2, v2.end()); // 对copy的调用可以被替换为利用区间的insert版本
 
	const int numValues = 100;
	int data[numValues];
 
	std::vector<int> v;
	v.insert(v.begin(), data, data + numValues); // 推荐，使用区间成员函数insert
 
	std::vector<int>::iterator insertLoc(v.begin());
	for (int i = 0; i < numValues; ++i) {
		insertLoc = v.insert(insertLoc, data[i]); // 不推荐，使用单元素成员函数
		++insertLoc;
	}
 
	return 0;
}
```
区间成员函数是指这样的一类成员函数，它们像STL算法一样，使用两个迭代器参数来确定该成员操作所执行的区间。如果不使用区间成员函数就得写一个显示的循环。

所有通过利用插入迭代器(insert iterator)的方式(即利用inserter、back_inserter或front_inserter)来限定目标区间的copy调用，其实都可以(也应该)被替换为对区间成员函数的调用。

优先选择区间成员函数而不是其对应的单元素成员函数有三条充分的理由：区间成员函数写起来更容易，更能清楚地表达你的意图，而且它们表现出了更高的效率。

### Item 6. 当心C++编译器最烦人的分析机制
```cpp
// 注意：围绕参数名的括号(比如对f2中d)与独立的括号的区别：围绕参数名的括号被忽略，而独立的括号则表明参数
// 列表的存在：它们说明存在一个函数指针参数
int f1(double d); // 声明了一个带double参数并返回int的函数
int f2(double(d)); // 同上，d两边的括号被忽略,可以给参数名加上圆括号
int f3(double); // 同上，参数名被忽略
 
int g1(double(*pf)()); // 参数是一个指向不带任何参数的函数的指针，该函数返回double值；g1以指向函数的指针为参数
int g2(double pf()); // 同上，pf为隐式指针
int g3(double()); // 同上，省去参数名
 
int test_item_6()
{
	// 把一个存有整数(int)的文件ints.dat拷贝到一个list中
	std::ifstream dataFile("ints.dat");
	std::list<int> data1(std::istream_iterator<int>(dataFile), std::istream_iterator<int>()); // 小心，结果不会是你所想象的那样
 
	std::list<int> data2((std::istream_iterator<int>(dataFile)), std::istream_iterator<int>()); // 正确，注意list构造函数的第一个参数两边的括号
 
	std::istream_iterator<int> dataBegin(dataFile);
	std::istream_iterator<int> dataEnd;
	std::list<int> data3(dataBegin, dataEnd); // 正确
 
	return 0;
}
```

使用命名的迭代器对象与通常的STL程序风格相违背，但你或许觉得为了使代码对所有编译器都没有二义性，并且使维护代码的人理解起来更容易，这一代价是值得的。

### Item 7. 如果容器中包含了通过new操作创建的指针，切记在容器对象析构前将指针delete掉
```cpp
class Widget7 {};
 
struct DeleteObject {
	template<typename T>
	void operator()(const T* ptr) const
	{
		delete ptr;
	}
};
 
int test_item_7()
{
	const int num = 5;
 
	std::vector<Widget7*> vwp1, vwp2;
	for (int i = 0; i < num; ++i) {
		vwp1.push_back(new Widget7); // 如果在后面自己不delete，使用vwp在这里发生了Widget7的泄露
		vwp2.push_back(new Widget7);
	}
 
	for (std::vector<Widget7*>::iterator i = vwp1.begin(); i != vwp1.end(); ++i) {
		delete *i; // 能行，但不是异常安全的
	}
 
	for_each(vwp2.begin(), vwp2.end(), DeleteObject()); // 正确，类型安全，但仍不是异常安全的
 
	typedef std::shared_ptr<Widget7> SPW; // SPW"指向Widget7的shared_ptr"
	std::vector<SPW> vwp3;
	for (int i = 0; i < num; ++i) {
		vwp3.push_back(SPW(new Widget7)); // 从Widget7创建SPW,然后对它进行一次push_back使用vwp3,这里不会有Widget7泄露，即使有异常被抛出
	}
 
	return 0;
}
```
STL容器很智能，但没有智能到知道是否该删除自己所包含的指针的程度。当你使用指针的容器，而其中的指针应该被删除时，为了避免资源泄漏，你必须或者用引用计数形式的智能指针对象(比如std::shared_ptr)代替指针，或者当容器被析构时手工删除其中的每个指针。

std::shared_ptr的使用参考：https://blog.csdn.net/fengbingchun/article/details/52202007


### Item 8. 切勿创建包含auto_ptr的容器对象

auto_ptr的容器(简称COAP)是被禁止的。当你拷贝一个auto_ptr时，它所指向的对象的所有权被移交到拷入的auto_ptr上，而它自身被置为NULL。如果你的目标是包含智能指针的容器，这并不意味着你要倒霉，包含智能指针的容器是没有问题的。

### Item 9. 慎重选择删除元素的方法
```cpp
bool badValue(int) { return true; } // 返回x是否为"坏值"
 
int test_item_9()
{
	// 删除c中所有值为1963的元素
	std::vector<int> c1;
	c1.erase(std::remove(c1.begin(), c1.end(), 1963), c1.end()); // 当c1是vector, string或deque时，erase-remove习惯用法是删除特定值的元素的最好办法
 
	std::list<int> c2;
	c2.remove(1963); // 当c2是list时，remove成员函数是删除特定值的元素的最好办法
 
	std::set<int> c3;
	c3.erase(1963); // 当c3是标准关联容器时，erase成员函数是删除特定值元素的最好办法
 
	// 删除判别式(predicate)返回true的每一个对象
	c1.erase(std::remove_if(c1.begin(), c1.end(), badValue), c1.end()); // 当c1是vector, string或deque时，这是删除使badValue返回true的对象的最好办法
 
	c2.remove_if(badValue); // 当c2是list时，这是删除使badValue返回true的对象的最好办法
 
	for (std::set<int>::iterator i = c3.begin(); i != c3.end();) {
		if (badValue(*i)) c3.erase(i++); // 对坏值，把当前的i传给erase，递增i是副作用
		else ++i;                        // 对好值，则简单的递增i
	}
 
	// 每次元素被删除时，都向一个日志(log)文件中写一条信息
	std::ofstream logFile;
	for (std::set<int>::iterator i = c3.begin(); i != c3.end();) {
		if (badValue(*i)) {
			logFile << "Erasing " << *i << '\n'; // 写日志文件
			c3.erase(i++); // 对坏值，把当前的i传给erase，递增i是副作用
		}
		else ++i;              // 对好值，则简单第递增i
	}
 
	for (std::vector<int>::iterator i = c1.begin(); i != c1.end();) {
		if (badValue(*i)) {
			logFile << "Erasing " << *i << '\n';
			i = c1.erase(i); // 把erase的返回值赋给i，使i的值保持有效
		}
		else ++i;
	}
 
	return 0;
}
```

(1).要删除容器中有特定值的所有对象：如果容器是vector, string或deque，则使用erase-remove习惯用法；如果容器是list，则使用list::remove；如果容器是一个标准关联容器，则使用它的erase成员函数。

(2).要删除容器中满足特定判别式(条件)的所有对象：如果容器是vector, string或deque，则使用erase-remove_if习惯用法；如果容器是list，则使用list::remove_if；如果容器是一个标准关联容器，则使用remove_copy_if和swap，或者写一个循环来遍历容器中的元素，记住当把迭代器传给erase时，要对它进行后缀递增。

(3).要在循环内做某些(除了删除对象之外的)操作：如果容器是一个标准序列容器，则写一个循环来遍历容器中的元素，记住每次调用erase时，要用它的返回值更新迭代器；如果容器是一个标准关联容器，则写一个循环来遍历容器中的元素，记住当把迭代器传给erase时，要对迭代器做后缀递增。

### Item 10. 了解分配子(allocator)的约定和限制

编写自定义的分配子，需要注意：(1).你的分配子是一个模板，模板参数T代表你为它分配内存的对象的类型。(2).提供类型定义pointer和reference，但是始终让pointer为T*，reference为T&。(3).千万别让你的分配子拥有随对象而不同的状态(per-object state)。通常，分配子不应该有非静态的数据成员。(4).记住，传给分配子的allocate成员函数的是那些要求内存的对象的个数，而不是所需的字节数。同时要记住，这些函数返回T*指针(通过pointer类型定义)，即使尚未有T对象被构造出来。(5).一定要提供嵌套的rebind模板，因为标准容器依赖该模板。

std::allocator的使用参考：https://blog.csdn.net/fengbingchun/article/details/78943527

### Item 11. 理解自定义分配子的合理用法
```cpp
void* mallocShared(size_t bytesNeeded)
{
	return malloc(bytesNeeded);
}
 
void freeShared(void* ptr)
{
	free(ptr);
}
 
template<typename T>
class SharedMemoryAllocator { // 把STL容器的内容放到共享内存(即由mallocShared生成的)中去
public:
	typedef T* pointer; // pointer是个类型定义，它实际上总是T*
	typedef size_t size_type; // 通常情况下,size_type是size_t的一个类型定义
	typedef T value_type;
 
	pointer allocate(size_type numObjects, const void* localityHint = 0)
	{
		return static_cast<pointer>(mallocShared(numObjects * sizeof(T)));
	}
 
	void deallocate(pointer ptrToMemory, size_type numObjects)
	{
		freeShared(ptrToMemory);
	}
 
	template<typename U>
	struct rebind {
		typedef std::allocator<U> other;
	};
};
 
int test_item_11()
{
	typedef std::vector<double, SharedMemoryAllocator<double>> SharedDoubleVec;
	// v所分配的用来容纳其元素的内存将来自共享内存
	// 而v自己----包括它所有的数据成员----几乎肯定不会位于共享内存中，v只是普通的基于栈(stack)的对象，所以，像所
	// 有基于栈的对象一样，它将会被运行时系统放在任意可能的位置上。这个位置几乎肯定不是共享内存
	SharedDoubleVec v; // 创建一个vector,其元素位于共享内存中
 
	// 为了把v的内容和v自身都放到共享内存中，需要这样做
	void* pVectorMemory = mallocShared(sizeof(SharedDoubleVec)); // 为SharedDoubleVec对象分配足够的内存
	SharedDoubleVec* pv = new (pVectorMemory)SharedDoubleVec; // 使用"placement new"在内存中创建一个SharedDoubleVec对象
	// ... // 使用对象(通过pv)
	pv->~SharedDoubleVec(); // 析构共享内存中的对象
	freeShared(pVectorMemory); // 释放最初分配的那一块共享内存
 
	return 0;
}
```

遵守同一类型的分配子必须是等价的这一限制要求。

### Item 12. 切勿对STL容器的线程安全性有不切实际的依赖

当涉及到STL容器和线程安全性时，你可以指望一个STL库允许多个线程同时读一个容器，以及多个线程对不同的容器做写入操作。你不能指望STL库会把你从手工同步控制中解脱出来，而且你不能依赖于任何线程支持。

## 2 vector和string

### Item 13. vector和string优先于动态分配的数组

std::vector的使用参考：https://blog.csdn.net/fengbingchun/article/details/51510916

std::string的使用参考：https://blog.csdn.net/fengbingchun/article/details/62417284

**许多string实现在背后使用了引用计数技术**，这种策略可以消除不必要的内存分配和不必要的字符拷贝，从而可以提供很多应用程序的效率。如果你在多线程环境下使用了引用计数的string，那么注意一下因支持线程安全而导致的性能问题。vector的实现不允许使用引用计数，所以不会发生隐藏的多线程性能问题。

### Item  14. 使用reserve来避免不必要的重新分配
```cpp
int test_item_14()
{
	std::vector<int> v;
	v.reserve(1000); // 如果不使用reserve,下面的循环在进行过程中将导致2到10次重新分配;加上reserve，则在循环过程中,将不会再发生重新分配
	for (int i = 1; i <= 1000; ++i) v.push_back(i);
 
	return 0;
}
```
对于vector和string，增长过程是这样来实现的：每当需要更多空间时，就调用与realloc类似的操作。这一类似于realloc的操作分为四部分：(1).分配一块大小为当前容量的某个倍数的新内存。在大多数实现中，vector和string的容量每次以2的倍数增长，即，每当容器需要扩张时，它们的容量即加倍。(2).把容器的所有元素从旧的内存拷贝到新的内存中。(3).析构掉就内存中的对象。(4).释放旧内存。

**reserve成员函数能使你把重新分配的次数减少到最低限度**，从而避免了重新分配和指针/迭代器/引用失效带来的开销。避免重新分配的关键在于，尽早地使用reserve，把容器的容量设为足够大的值，最好是在容器刚被构造出来之后就使用reserve。

大小(size)和容量(capacity)之间的关系使我们能够预知什么时候插入操作会导致vector或string进行重新分配的动作，进而又使我们有可能预知什么时候一个插入操作会使容器中的迭代器、指针和引用失效。

通常有两种方式来使用reserve以避免不必要的重新分配。第一种方式是，若能确切知道或大致预计容器中最终会有多少元素，则此时可以使用reserve。第二种方式是，先预留足够大的空间(根据你的需要而定)，然后，当把所有数据都加入以后，再去除多余的容量。

### Item 15. 注意string实现的多样性
```cpp
int test_item_15()
{
	fprintf(stdout, "string size: %d, char* size: %d\n", sizeof(std::string), sizeof(char*));
 
	return 0;
}
```

(1).string的值可能会被引用计数，也可能不会。很多实现在默认情况下会使用引用计数，但它们通常提供了关闭默认选择的方法，往往是通过预处理宏来做到这一点。(2).string对象大小的范围可以是一个char*指针大小的1倍到7倍。(3).创建一个新的字符串值可能需要零次、一次或两次动态分配内存。(4).string对象可能共享，也可能不共享其大小和容量信息。(5).string可能支持，也可能不支持针对单个对象的分配子。(6).不同的实现对字符内存的最小分配单位有不同的策略。

### Item 16. 了解如何把vector和string数据传给旧的API
```cpp
void doSomething(const int* pInts, size_t numInts) {}
void doSomething(const char* pString) {}
size_t fillArray(double* pArray, size_t arraySize) { return (arraySize - 2); }
size_t fillString(char* pArray, size_t arraySize) { return (arraySize - 2); }
 
int test_item_16()
{
	// vector/string --> C API
	std::vector<int> v{ 1, 2 };
	if (!v.empty()) {
		doSomething(&v[0], v.size());
		doSomething(v.data(), v.size()); // C++11
		// doSomething(v.begin(), v.size()); // 错误的用法
		doSomething(&*v.begin(), v.size()); // 可以，但不易于理解
	}
 
	std::string s("xxx");
	doSomething(s.c_str());
 
	// C API 初始化vector
	const int maxNumDoubles = 100;
	std::vector<double> vd(maxNumDoubles); // 创建大小为maxNumDoubles的vector
	vd.resize(fillArray(&vd[0], vd.size())); // 使用fillArray向vd中写入数据，然后把vd的大小改为fillArray所写入的元素的个数
 
	// C API 初始化string
	const int maxNumChars = 100;
	std::vector<char> vc(maxNumChars); // 创建大小为maxNumChars的vector
	size_t charsWritten = fillString(&vc[0], vc.size()); // 使用fillString向vc中写入数据
	std::string s2(vc.cbegin(), vc.cbegin() + charsWritten); // 通过区间构造函数，把数据从vc拷贝到s2中
 
	// 先让C API把数据写入到一个vector中，然后把数据拷贝到期望最终写入的STL容器中，这一思想总是可行的
	std::deque<double> d(vd.cbegin(), vd.cend()); // 把数据拷贝到deque中
	std::list<double> l(vd.cbegin(), vd.cend()); // 把数据拷贝到list中
	std::set<double> s22(vd.cbegin(), vd.cend()); // 把数据拷贝到set中
 
	// 除了vector和string以外的其它STL容器也能把它们的数据传递给C API，你只需要把每个容器的元素拷贝到一个vector中，然后传给该API
	std::set<int> intSet; // 存储要传给API的数据的set
	std::vector<int> v2(intSet.cbegin(), intSet.cend()); // 把set的数据拷贝到vector
	if (!v2.empty()) doSomething(&v2[0], v2.size()); // 把数据传给API
 
	return 0;
}
```

C++标准要求vector中的元素存储在连续的内存中，就像数组一样。string中的数据不一定存储在连续的内存中，而且string的内部表示不一定是以空字符结尾的。

### Item 17. 使用”swap技巧”除去多余的容量

```cpp
class Contestant {};
 
int test_item_17()
{
	// 从contestants矢量中除去多余的容量
	std::vector<Contestant> contestants;
	// ... // 让contestants变大，然后删除它的大部分元素
	// vector<Contestant>(contestants)创建一个临时矢量，vector的拷贝构造函数只为所拷贝的元素分配所需要的内存
	std::vector<Contestant>(contestants).swap(contestants);
 
	contestants.shrink_to_fit(); // C++11
 
	std::string s;
	// ... // 让s变大，然后删除它的大部分字符
	std::string(s).swap(s);
 
	s.shrink_to_fit(); // C++11
 
	std::vector<Contestant>().swap(contestants); // 清除contestants并把它的容量变为最小
 
	std::string().swap(s); // 清除s并把它的容量变为最小
 
	return 0;
}
```

对vector或string进行shrink-to-fit操作时，考虑”swap”技巧。C++11中增加了**shrink_to_fit**成员函数。

swap技巧的一种变化形式可以用来清除一个容器，并使其容量变为该实现下的最下值。

在做swap的时候，不仅两个容器的内容被交换，同时它们的迭代器、指针和引用也将被交换(string除外)。在swap发生后，原先指向某容器中元素的迭代器、指针和引用依然有效，并指向同样的元素----但是，这些元素已经在另一个容器中了。

### Item 18. 避免使用vector<bool>
```cpp
int test_item_18()
{
	std::vector<bool> v;
	// error: cannot convert 'std::vector<bool>::reference* {aka std::_Bit_reference}' to 'bool*' in initialization
	//bool* pb = &v[0]; // 不能被编译，原因：vector<bool>是一个假的容器，它并不真的储存bool，相反，为了节省空间，它储存的是bool的紧凑表示
 
	return 0;
}
```
作为一个STL容器，vector<bool>只有两点不对。首先，它不是一个STL容器；其次，它并不存储bool。除此以外，一切正常。

在一个典型的实现中，储存在”vector”中的每个”bool”仅占一个二进制位，一个8位的字节可容纳8个”bool”。在内部，vector<bool>使用了与位域(bit field)一样的思想，来表示它所存储的那些bool，实际上它只是假装存储了这些bool。

位域与bool相似，它只能表示两个可能的值，但是在bool和看似bool的位域之间有一个很重要的区别：你可以创建一个指向bool的指针，而指向单个位的指针则是不允许的。指向单个位的引用也是被禁止的。

当你需要vector<bool>时，标准库提供了两种选择，可以满足绝大多数情况下的需求。第一种是deque<bool>。deque几乎提供了vector所提供的一切(没有reserve和capacity)，但deque<bool>是一个STL容器，而且它确实存储bool。当然deque中元素的内存不是连续的，所以你不能把deque<bool>中的数据传递给一个期望bool数组的C API。

第二种可以替代vector<bool>的选择是bitset。bitset不是STL容器，但它是标准C++库的一部分。与STL容器不同的是，它的大小(即元素的个数)在编译时就确定了，所以它不支持插入和删除元素。

## 3 关联容器

### Item 19. 理解相等(equality)和等价(equivalence)的区别

std::set的使用参考：https://blog.csdn.net/fengbingchun/article/details/63268962

std::map的使用参考：https://blog.csdn.net/fengbingchun/article/details/52074286

相等的概念是基于operator==的。等价关系是以”在已排序的区间中对象值的相对顺序”为基础的。标准关联容器是基于等价而不是相等。

标准关联容器总是保持排列顺序的，所以每个容器必须有一个比较函数(默认为less)来决定保持怎样的顺序。等价的定义正是通过该比较函数而确定的，因此，标准关联容器的使用者要为所使用的每个容器指定一个比较函数(用来决定如何排序)。如果该关联容器使用相等来决定两个对象是否有相同的值，那么每个关联容器除了用于排序的比较函数外，还需要另一个比较函数来决定两个值是否相等。(默认情况下，该比较函数应该是equal_to，但equal_to从来没有被用作STL的默认比较函数。当STL中需要相等判断时，一般的惯例是直接调用operator==)。

C++标准对于等价的值(对multiset)或键(对multimap)的相对顺序没有什么限制。

### Item 20. 为包含指针的关联容器指定比较类型
```cpp
struct StringPtrLess : public std::binary_function<const std::string*, const std::string*, bool> { // std::binary_function在C++11中已被废弃
	bool operator()(const std::string* ps1, const std::string* ps2) const
	{
		return *ps1 < *ps2;
	}
};
 
struct DereferenceLess {
	template<typename PtrType>
	bool operator()(PtrType pT1, PtrType pT2) const
	{
		return *pT1 < *pT2;
	}
};
 
int test_item_20()
{
	//std::set<std::string*> ssp; // std::set<std::string*> <==> std::set<std::string*, std::less<std::string*>>, 得不到预期的结果
	//std::set<std::string*, StringPtrLess> ssp;
	std::set<std::string*, DereferenceLess> ssp; // 与std::set<std::string*, StringPtrLess> ssp;的行为相同
	ssp.insert(new std::string("Anteater"));
	ssp.insert(new std::string("Wombat"));
	ssp.insert(new std::string("Lemur"));
	ssp.insert(new std::string("Penguin"));
 
	for (auto it = ssp.cbegin(); it != ssp.cend(); ++it) {
		fprintf(stdout, "%s\n", (**it).c_str());
	}
 
	return 0;
}
```
每当你要创建包含指针的关联容器时，一定要记住，容器将会按照指针的值进行排序。绝大多数情况下，这不会是你所希望的，所以你几乎肯定要创建自己的函数子类作为该容器的比较类型(comparison type)。

如果你有一个包含智能指针或迭代器的容器，那么你也要考虑为它指定一个比较类型。对指针的解决方案同样也适用于那些类似指针的对象。就像DereferenceLess适合作为包含T*的关联容器的比较类型一样，对于容器中包含了指向T对象的迭代器或智能指针的情形，DereferenceLess也同样可用作比较类型。

### Item 21. 总是让比较函数在等值情况下返回false

比较函数的返回值表明的是按照该函数定义的排列顺序，一个值是否在另一个之前。相等的值从来不会有前后顺序关系，所以，对于相等的值，比较函数应当始终返回false。对set和map确实是这样，因为这些容器不能包含重复的值。对multiset和multimap也是这样。从技术上来说，用于对关联容器排序的比较函数必须为它们所比较的对象定义一个”严格的弱序化”(strict weak ordering)。(对于传递给像sort这类算法的比较函数也有同样的限制。)任何一个定义了”严格的弱序化”的函数必须对相同值的两个拷贝返回false。

### Item 22. 切勿直接修改set或multiset中的键

```cpp
int test_item_22()
{
	std::map<int, std::string> m{ { 0, "xxx" } };
	//m.begin()->first = 10; // build error, map的键不能修改
 
	std::multimap<int, std::string> mm{ { 1, "yyy" } };
	//mm.begin()->first = 10; // build error, multimap的键同样不能修改
 
	std::set<int> s{ 1, 2, 3 };
	//*(s.begin()) = 10; // build error, set的键不能修改
	const_cast<int&>(*s.begin()) = 10; // 强制类型转换
 
	std::vector<int> v{ 1, 2, 3 };
	*v.begin() = 10;
 
	return 0;
}
```
像所有的标准关联容器一样，set和multiset按照一定的顺序来存放自己的元素，而这些容器的正确行为也是建立在其元素保持有序的基础之上的。如果你把关联容器中的一个元素的值改变了(比如把10改为1000)，那么，新的值可能不在正确的位置上，这将会打破容器的有序性。

对于map和multimap尤其简单，因为如果有程序试图改变这些容器中的键，它将不能通过编译。这是因为，对于一个map<K, V>或multimap<K, V>类型的对象，其中的元素类型是pair<const K, V>。因为键的类型是const K，所以它不能被修改。(如果利用const_cast，你或许可以修改它。)

对于set<T>或multiset<T>类型的对象，容器中元素的类型是T，而不是const T。因此，~~只要你愿意，你随时可以改变set或multiset中的元素，而无需任何强制类型转换。~~注：不通过强制类型转换并不能改变set或multiset中的元素。

### Item 23. 考虑用排序的vector替代关联容器

对于许多应用，哈希容器可能提供常数时间的查找能力优于set、multiset、map和multimap的确定的对数时间查找能力。

标准关联容器通常被实现为平衡的二叉查找树。

在排序的vector中存储数据可能比在标准关联容器中存储同样的数据要耗费更少的内存，而考虑到页面错误的因素，**通过二分搜索法来查找一个排序的vector可能比查找一个标准关联容器要更快一些**。当然，对于排序的vector，最不利的地方在于它必须保持有序，当一个新的元素被插入时，新元素之后的所有元素都必须向后移动一个元素的位置，尤其是当vector必须重新分配自己的内存时。与此类似，如果一个元素被从vector中删除了，则在它之后的所有元素也都要向前移动。插入和删除操作对于vector来说是昂贵的，但对于关联容器却是廉价的。这就是为什么只有当你知道”对数据结构的使用方式是：查找操作几乎从不跟插入和删除操作混在一起”时，再考虑使用排序的vector而不是关联容器才是合理的。否则，使用排序的vector而不是标准关联容器几乎肯定是在浪费时间。

### Item 24. 当效率至关重要时，请在map::operator[]与map::insert之间谨慎做出选择
```cpp
// 提供高效的添加和更新功能
template<typename MapType, typename KeyArgType, typename ValueArgType>
typename MapType::iterator efficientAddOrUpdate(MapType& m, const KeyArgType& k, const ValueArgType& v)
{
	typename MapType::iterator lb = m.lower_bound(k); // 确定k在什么位置或应在什么位置
 
	if (lb != m.end() && !(m.key_comp()(k, lb->first))) { // 如果lb指向的pair的健与k等价,
		lb->second = v;				      // 则更新pair的值并返回指向该pair的迭代器
		return lb;
	}
	else {
		typedef typename MapType::value_type MVT;
		return m.insert(lb, MVT(k, v)); // 把pair<k,v>添加到m中，并返回一个指向该新元素的迭代器
	}
}
 
int test_item_24()
{
	std::map<int, std::string> m;
	m[1] = "xxx"; // m[1]是m.operator[](1)的缩写形式
	m.operator[](1) = "xxx";
 
	// m[1] = "xxx"; 在功能上等同于
	typedef std::map<int, std::string> IntStrMap;
	std::pair<IntStrMap::iterator, bool> result = m.insert(IntStrMap::value_type(1, std::string()));
	result.first->second = "xxx";
	// 以上方式性能降低的原因：先默认构造了一个string，然后立刻赋给它新的值。如果"直接使用我们所需要的值构造一个
	// string"比"先默认构造一个string再赋值"效率更高，那么，我们最好把对operator[]的使用(包括与之相伴的构造和
	// 赋值)换成对insert的直接调用
	m.insert(IntStrMap::value_type(1, "xxx")); // 它通常会节省三个函数调用：一个用于创建默认构造的临时string对象，
	// 一个用于析构该临时对象，另一个是调用string的赋值操作符
 
	efficientAddOrUpdate(m, 2, "yyy");
 
	return 0;
}
```

map的operator[]函数与众不同。它与vector、deque和string的operator[]函数无关，与用于数组的内置operator[]也没有关系。相反，map::operator[]的设计目的是为了提供”添加和更新”(add or update)的功能。map::operator[]返回一个引用。

对效率的考虑使我们得出结论：当向映射表中添加元素时，要优先选用insert，而不是operator[]；而从效率和美学的观点考虑，结论是：当更新已经在映射表中的元素的值时，要优先选择operator[]。

### Item 25. 熟悉非标准的哈希容器

C++11中新增了四种关联容器，使用哈希函数组织的，无序的，即**unordered_map、unordered_multimap、unordered_set、unordered_multiset**。

std::unordered_map的使用参考：https://blog.csdn.net/fengbingchun/article/details/52235026

## 4 迭代器

### Item 26. iterator优先于const_iterator、reverse_iterator以及const_reverse_iterator

STL中的所有标准容器都提供了4中迭代器类型。对容器类container<T>而言，iterator类型的功效相当于T*，而const_iterator则相当于const T*。对一个iterator或者const_iterator进行递增则可以移动到容器中的下一个元素，通过这种方式可以从容器的头部一直遍历到尾部。reverse_iterator与const_reverse_iterator同样分别对应于T*和const T*，所不同的是，对这两个迭代器进行递增的效果是由容器的尾部反向遍历到容器头部。

注意：vector::insert，对于C++98中，第一个参数均为iterator；而对于C++11中，第一个参数均为const_iterator。vector::erase的情况也是这样。

### Item 27. 使用distance和advance将容器的const_iterator转换成iterator

```cpp
int test_item_27()
{
	typedef std::deque<int> IntDeque;
	typedef IntDeque::iterator Iter;
	typedef IntDeque::const_iterator ConstIter;
 
	IntDeque d(5, 10);
	ConstIter ci;
	ci = d.cbegin() + 1; // 使ci指向d
	Iter i(d.begin());
	std::advance(i, std::distance<ConstIter>(i, ci));
 
	return 0;
}
```
std::distance用以取得两个迭代器(它们指向同一个容器)之间的距离；std::advance则用于将一个迭代器移动指定的距离。

std::distance和std::advance的使用参考：https://blog.csdn.net/fengbingchun/article/details/77985191

### Item 28. 正确理解由reverse_iterator的base()成员函数所产生的iterator的用法

```cpp
int test_item_28()
{
	std::vector<int> v;
	v.reserve(5);
 
	for (int i = 1; i <= 5; ++i) v.push_back(i);
 
	std::vector<int>::reverse_iterator ri = std::find(v.rbegin(), v.rend(), 3); // 使ri指向3
	std::vector<int>::iterator i(ri.base());
	fprintf(stdout, "%d\n", (*i)); // 4
	v.insert(i, 99);
	for (auto it = v.cbegin(); it != v.cend(); ++it) fprintf(stdout, "value: %d\n", *it); // 1 2 3 99 4 5
 
	v.clear(); v.reserve(5);
	for (int i = 1; i <= 5; ++i) v.push_back(i);
	ri = std::find(v.rbegin(), v.rend(), 3);
	v.erase((++ri).base());
	for (auto it = v.cbegin(); it != v.cend(); ++it) fprintf(stdout, "value: %d\n", *it); // 1 2 4 5
 
	return 0;
}
```

如果要在一个reverse_iterator ri指定的位置上插入新元素，则只需在ri.base()位置处插入元素即可。对于插入操作而言，ri和ri.base()是等价的，ri.base()是真正与ri对应的iterator。

如果要在一个reverse_iterator ri指定的位置上删除一个元素，则需要在ri.base()前面的位置上执行删除操作。对于删除操作而言，ri和ri.base()是不等价的，ri.base()不是与ri对应的iterator。

### Item 29. 对于逐个字符的输入请考虑使用istreambuf_iterator

```cpp
int test_item_29()
{
	// 把一个文本文件的内容拷贝到一个string对象中
	std::ifstream inputFile("interestingData.txt");
	inputFile.unsetf(std::ios::skipws); // 禁止忽略inputFile中的空格
	std::string fileData((std::istream_iterator<char>(inputFile)), std::istream_iterator<char>()); // 速度慢
 
	std::string fileData2((std::istreambuf_iterator<char>(inputFile)), std::istreambuf_iterator<char>()); // 速度快
 
	return 0;
}
```
std::istream_iterator<char>对象使用operator>>从输入流中读取单个字符，而std::istreambuf_iterator<char>则直接从流的缓冲区中读取下一个字符。std::istreambuf_iterator不会跳过任何字符，它只是简单地取回流缓冲区中的下一个字符，而不管它们是什么字符，因此用不着清除输入流的skipws标志。

同样地，对于非格式化的逐个字符输出过程，你也应该考虑使用std::ostreambuf_iterator，它可以避免因使用std::ostream_iterator而带来的额外负担。

## 5 算法

### Item 30. 确保目标区间足够大
```cpp
int transmogrify(int x) { return (x + 1); }
 
int test_item_30()
{
	std::vector<int> values{ 1, 2, 3 };
	std::vector<int> results;
	results.reserve(results.size() + values.size()); // 可避免内存的重新分配
	//std::transform(values.cbegin(), values.cend(), results.end(), transmogrify); // 错误，segmentation fault
	std::transform(values.cbegin(), values.cend(), std::back_inserter(results), transmogrify); // 正确
	// 在内部，std::back_inserter返回的迭代器将使得push_back被调用，所以back_inserter可适用于所有提供了push_back方法的容器
 
	std::list<int> results2;
	std::transform(values.cbegin(), values.cend(), std::front_inserter(results2), transmogrify);
	// std::front_inserter在内部利用了push_front，所以front_inserter仅适用于那些提供了push_front成员函数的容器
 
 
	return 0;
}
```

无论何时，如果所使用的算法需要指定一个目标区间，那么必须确保目标区间足够大，或者确保它会随着算法的运行而增大。要在算法执行过程中增大目标区间，请使用插入型迭代器，比如ostream_iterator或者由back_inserter、front_inserter和inserter返回的迭代器。

### Item 31. 了解各种与排序有关的选择
```cpp
bool qualityCompare(const std::string& lhs, const std::string& rhs)
{
	return (lhs < rhs);
}
 
bool hasAcceptableQuality(const std::string& w)
{
	return true; // 判断w的质量值是否为2或者更好
}
 
int test_item_31()
{
	std::vector<std::string> vec(50, "xxx");
	std::partial_sort(vec.begin(), vec.begin() + 20, vec.end(), qualityCompare); // 将质量最好的20个元素顺序放在vec的前20个位置上
 
	std::nth_element(vec.begin(), vec.begin() + 19, vec.end(), qualityCompare); // 将最好的20个元素放在vec的前部，但并不关心它们的具体排列顺序
 
	// std::partia_sort和std::nth_element在效果上唯一不同之处在于：partial_sort对位置1--20中的元素进行了排序，而
	// nth_element没有对它们进行排序。然而，这两个算法都将质量最好的20个vec放到了矢量的前部
 
	std::vector<std::string>::iterator begin(vec.begin());
	std::vector<std::string>::iterator end(vec.end());
	std::vector<std::string>::iterator goalPosition; // 用于定位感兴趣的元素
	// 找到具有中间质量级别的string
	goalPosition = begin + vec.size() / 2; // 如果全排序的话，待查找的string应该位于中间
	std::nth_element(begin, goalPosition, end, qualityCompare); // 找到vec的中间质量值
	// 现在goalPosition所指的元素具有中间质量
 
	// 找到区间中具有75%质量的元素
	std::vector<std::string>::size_type goalOffset = 0.25 * vec.size(); // 找出如果全排序的话，待查找的string离起始处有多远
	std::nth_element(begin, begin + goalOffset, end, qualityCompare); // 找到75%处的质量值	
 
	// 将满足hasAcceptableQuality的所有元素移到前部，然后返回一个迭代器，指向第一个不满足条件的string
	std::vector<std::string>::iterator goodEnd = std::partition(vec.begin(), vec.end(), hasAcceptableQuality);
 
	return 0;
}
```

std::sort/sdk::stable_sort/std::partial_sort使用参考：https://blog.csdn.net/fengbingchun/article/details/71305229

std::nth_element：用于排序一个区间，它使得位置n上的元素正好是全排序情况下的第n个元素。而且，当nth_element返回的时候，所有按全排序规则(即sort的结果)排在位置n之前的元素也都被排在位置n之前，而所有按全排序规则排在位置n之后的元素则都被排在位置n之后。

std::partial_sort和std::nth_element在排列等价元素的时候，有它们自己的做法，你无法控制它们的行为。

std::partial_sort、std::nth_element和std::sort都属于非稳定的排序算法，但是有一个名为std::stable_sort的算法可以提供稳定排序特性。

std::nth_element除了可以用来找到排名在前的n个元素以外，它还可以用来找到一个区间的中间值，或者找到某个特定百分比上的值。

std::partition：可以把所有满足某个特定条件的元素放在区间的前部。

std::nth_element和std::partition使用参考：https://blog.csdn.net/fengbingchun/article/details/78034969

总结排序选择：(1).如果需要对vector、string、deque或者数组中的元素执行一次完全排序，那么可以使用sort或者stable_sort。(2).如果有一个vector、string、deque或者数组，并且只需要对等价性最前面的n个元素进行排序，那么可以使用**partial_sort**。(3).如果有一个vector、string、deque或者数组，并且需要找到第n个位置上的元素，或者，需要找到等价性前面的n个元素但又不必对这n个元素进行排序，那么，**nth_element**正是你所需要的函数。(4).如果需要将一个标准序列容器中的元素按照是否满足某个特定的条件区分开来，那么，partition和stable_partition可能正是你所需要的。(5).如果你的数据在一个list种，那么你仍然可以直接调用partition和stable_partition算法；你可以用list::sort来替代sort和stable_sort算法。但是，如果你需要获得partial_sort或nth_element算法的效果，那么，你可以有一些间接的途径来完成这项任务。

### Item 32. 如果确实需要删除元素，则需要在remove这一类算法之后调用erase
```cpp
int test_item_32()
{
	std::vector<int> v;
	v.reserve(10);
	for (int i = 1; i <= 10; ++i) v.push_back(i);
	fprintf(stdout, "v.size: %d\n", v.size()); // 输出10
	v[3] = v[5] = v[9] = 99;
	std::remove(v.begin(), v.end(), 99); // 删除所有值等于99的元素
	fprintf(stdout, "v.size: %d\n", v.size()); // 仍然输出10, remove不是真正意义上的删除，因为它做不到
	for (auto i : v) fprintf(stdout, "%d\n", i);
 
	v.erase(std::remove(v.begin(), v.end(), 99), v.end()); // 真正删除所有值等于99的元素	
 
	return 0;
}
```

std::remove使用参考：https://blog.csdn.net/fengbingchun/article/details/78034969

std::remove并不接受容器作为参数，所以remove并不知道这些元素被存放在哪个容器中。并且，remove也不可能推断出是什么容器，因为无法从迭代器推知对应的容器类型。因为从容器中删除元素的唯一方法是调用该容器的成员函数，而remove并不知道它操作的元素所在的容器，所以remove不可能从容器中删除元素。

std::list的remove成员函数是STL中唯一一个名为remove并且确实删除了容器中元素的函数。

std::remove并不是唯一一个适用于这种情形的算法，其它还有两个属于”remove类”的算法：remove_if和unique。如同list::remove会真正删除元素(并且比使用erase-remove习惯用法更为高效)一样，std::list::unique也会真正删除元素(而且比使用erase-unique更为高效)。

### Item 33. 对包含指针的容器使用remove这一类算法时要特别小心
```cpp
class Widget33 {
public:
	bool isCertified() const { return true; }
};
 
// 如果*pWidget是一个未被验证的Widget33,则删除该指针，并把它置成空
void delAndNullifyUncertified(Widget33*& pWidget)
{
	if (!pWidget->isCertified()) {
		delete pWidget;
		pWidget;
	}
}
 
int test_item_33()
{
	std::vector<Widget33*> v;
	for (int i = 0; i < 5; ++i) v.push_back(new Widget33);
 
	// 删除那些指向未被验证过的Widget33对象的指针，会资源泄露
	v.erase(std::remove_if(v.begin(), v.end(), std::not1(std::mem_fun(&Widget33::isCertified))), v.end());
 
	// 一种可以消除资源泄露的做法
	// 将所有指向未被验证的Widget33对象的指针删除并置成空
	std::for_each(v.begin(), v.end(), delAndNullifyUncertified);
	// 删除v中的空指针，必须将0转换成一个指针，这样C++才能正确推断出remove的第三个参数类型
	v.erase(std::remove(v.begin(), v.end(), static_cast<Widget33*>(0)), v.end());
 
	// 使用智能指针可防止资源泄露
	std::vector<std::shared_ptr<Widget33>> v2;
	for (int i = 0; i < 5; ++i) v2.push_back(std::make_shared<Widget33>());
	// 下面语句需要编译器必须能够把智能指针类型std::shared<Widget33>隐式转换为对应的内置指针类型Widget33*才能通过编译
	//v2.erase(std::remove_if(v2.begin(), v2.end(), std::not1(std::mem_fun(&Widget33::isCertified))), v2.end());
 
	return 0;
}
```


当容器中存放的是指向动态分配的对象的指针的时候，应该避免使用remove和类似的算法(remove_if和unique)。

如果容器中存放的不是普通指针，而是具有引用计数功能的智能指针，那么就可以直接使用erase-remove的习惯用法。

### Item 34. 了解哪些算法要求使用排序的区间作为参数

并非所有的算法都可以应用于任何区间。举例来说，remove算法要求单向迭代器并且要求可以通过这些迭代器向容器中的对象赋值。所以，它不能用于由输入迭代器指定的区间，也不适用于map或multimap，同样不适用于某些set和multiset的实现。同样地，很多排序算法要求随机访问迭代器，所以对于list的元素不可能调用这些算法。有些算法要求排序的区间，即区间中的值是排过序的。有些算法既可以与排序的区间一起工作，也可以与未排序的区间一起工作，但是当它们作用在排序的区间上时，算法会更加有效。

要求排序区间的STL算法：binaray_search、lower_bound、upper_bound、equal_range、set_union、set_intersection、set_difference、set_symmetric_difference、merge、inplace_merge、includes。

unique、unique_copy并不一定要求排序的区间，但通常情况下会与排序区间一起使用。

用于查找的算法binaray_search、lower_bound、upper_bound、equal_range要求排序的区间，因为它们用二分法查找数据。实际上，这些算法并不一定保证对数时间的查找效率。只有当它们接受了随机访问迭代器的时候，它们才保证有这样的效率。

set_union、set_intersection、set_difference、set_symmetric_difference这4个算法提供了线性时间效率的集合操作。如果它们不满足排序的区域的要求，它们就无法在线性时间内完成工作。

merge和inplace_merge实际上实现了合并和排序的联合操作：它们读入两个排序的区间，然后合并成一个新的排序区间，其中包含了原来两个区间中的所有元素。它们具有线性时间的性能，但如果它们不知道源区间已经排过序的话，它们就不可能在线性时间内完成。

最后一个要求排序源区间的算法是includes，它可用来判断一个区间中的所有对象是否都在另一个区间中。因为includes总是假设这两个区间是排序的，所以它承诺线性时间的效率。如果没有这一前提的话，它通常会运行得更慢。

unique和unique_copy与上述讨论过的算法有所不同，它们即使对于未排序的区间也有很好的行为。unique使用了与remove类似的办法来删除区间中的元素，而并非真正意义上的删除。

**如果你为一个算法提供了一个排序的区间，而这个算法也带一个比较函数作为参数，那么，你一定要保证你传递的比较函数与这个排序区间所用的比较函数有一致的行为。**

以上这些算法在include<algorithm>中，使用参考：https://blog.csdn.net/fengbingchun/article/details/78034969

### Item 35. 通过mismatch或lexicographical_compare实现简单的忽略大小写的字符串比较

```cpp
// 忽略大小写地比较字符c1和c2，如果c1<c2,返回-1；如果c1>c2,返回1；如果c1==c2,返回0
int ciCharCompare(char c1, char c2)
{
	int lc1 = std::tolower(static_cast<unsigned char>(c1));
	int lc2 = std::tolower(static_cast<unsigned char>(c2));
 
	if (lc1 < lc2) return -1;
	if (lc1 > lc2) return 1;
	return 0;
}
 
int ciStringCompareImpl(const std::string& s1, const std::string& s2)
{
	typedef std::pair<std::string::const_iterator, std::string::const_iterator> PSCI;
	PSCI p = std::mismatch(s1.begin(), s1.end(), s2.begin(), std::not2(std::ptr_fun(ciCharCompare)));
 
	if (p.first == s1.end()) { // 如果为true,要么s1和s2相等，或者s1比s2短
		if (p.second == s2.end()) return 0;
		else return -1;
	}
 
	return ciCharCompare(*p.first, *p.second); // 字符串之间的关系和这两个不匹配的字符之间的关系相同
}
 
int ciStringCompare(const std::string& s1, const std::string& s2)
{
	// 把短的字符串作为第一个区间传入
	if (s1.size() <= s2.size()) return ciStringCompareImpl(s1, s2);
	else return -ciStringCompareImpl(s2, s1);
}
 
// 返回在忽略大小写的情况下，c1是否在c2之前
bool ciCharLess(char c1, char c2)
{
	return std::tolower(static_cast<unsigned char>(c1)) <
		std::tolower(static_cast<unsigned char>(c2));
}
 
bool ciStringCompare2(const std::string& s1, const std::string& s2)
{
	return std::lexicographical_compare(s1.begin(), s1.end(), s2.begin(), s2.end(), ciCharLess);
}
 
bool ciStringCompare3(const std::string& s1, const std::string& s2)
{
	// 前提：不考虑国际化支持，也确定字符串中不会包含内嵌的空字符
	return strcmp(s1.c_str(), s2.c_str());
}
 
int test_item_35()
{
	std::string str1{ "xxz" }, str2{ "xxx" };
	fprintf(stdout, "str1:str2: %d\n", ciStringCompare(str1, str2));
	fprintf(stdout, "str1:str2: %d\n", ciStringCompare2(str1, str2));
	fprintf(stdout, "str1:str2: %d\n", ciStringCompare3(str1, str2));
 
	return 0;
}
```

std::lexicographical_compare是strcmp的一个泛化版本。不过，strcmp只能与字符数组一起工作，而lexicographical_compare则可以与任何类型的值的区间一起工作。而且，strcmp总是通过比较两个字符来判断它们的关系相等、小于还是大于，而lexicographical_compare则可以接受一个判别式，由该判别式来决定两个值是否满足一个用户自定义的准则。

strcmp通常是被优化过的，它们在字符串的处理上一般要比通用算法mismatch和lexicographical_compare快。

### Item 36. 理解copy_if算法的正确实现

```cpp
int test_item_36()
{
	std::vector<int> v1{ 1, 2, 3, 4, 5 }, v2(v1.size());
 
	auto it = std::copy_if(v1.begin(), v1.end(), v2.begin(), [](int i) { return (i % 2 == 1); });
	v2.resize(std::distance(v2.begin(), it));
 
	for (const auto& v : v2)
		fprintf(stdout, "%d\n", v); // 1 3 5
 
	return 0;
}
```

C++11中增加了std::copy_if函数。

### Item 37. 使用accumulate或者for_each进行区间统计

```cpp
// 接受当前的长度总和值和新的字符串，然后返回更新之后的总和值
std::string::size_type stringLengthSum(std::string::size_type sumSoFar, const std::string& s)
{
	return sumSoFar + s.size();
}
 
struct Point {
	Point(double initX, double initY) : x(initX), y(initY) {}
	double x, y;
};
 
class PointAverage : public std::unary_function<Point, void> {
public:
	PointAverage() : xSum(0), ySum(0), numPoints(0) {}
 
	void operator()(const Point& p)
	{
		++numPoints;
		xSum += p.x;
		ySum += p.y;
	}
 
	Point result() const
	{
		return Point(xSum / numPoints, ySum / numPoints);
	}
 
private:
	size_t numPoints;
	double xSum, ySum;
};
 
int test_item_37()
{
	std::vector<double> v{ 1.0f, 2.2f, 3.3f, 4.5f };
	double sum = std::accumulate(v.cbegin(), v.cend(), 0.0); // 注意：初始值被指定为0.0,而不是简单的0
	fprintf(stdout, "vaule: %f\n", sum); // 11.000000
 
	fprintf(stdout, "sum of the ints: %d\n", std::accumulate(std::istream_iterator<int>(std::cin), std::istream_iterator<int>(), 0)); // 输入非整数值结束,如字母
 
	std::set<std::string> ss{ "abc", "cde", "xyzw" };
	// 计算一个容器中字符串的长度总和
	std::string::size_type lengthSum = std::accumulate(ss.cbegin(), ss.cend(),
		static_cast<std::string::size_type>(0), stringLengthSum);
	fprintf(stdout, "length sum: %d\n", lengthSum); // 10
 
	// 计算一个区间中数值的乘积
	std::vector<float> vf{ 1.f, 2.f, 3.f, 1.5f };
	float product = std::accumulate(vf.cbegin(), vf.cend(), 1.f, std::multiplies<float>());
	fprintf(stdout, "product: %f\n", product); // 9.000000
 
	// 计算出一个区间中所有点的平均值
	std::list<Point> lp{ { 1, 2 }, { 2, 3 }, { 3, 4 }, { 4, 5 } };
	Point avg = std::for_each(lp.cbegin(), lp.cend(), PointAverage()).result();
 
	return 0;
}
```

std::**accumulate**有两种形式：第一种形式有两个迭代器和一个初始值，它返回该初始值加上由迭代器标识的区间中的值的总和。

std::**accumulate**只要求输入迭代器，所以你可以使用std::istream_iterator和std::istreambuf_iterator。

std::accumulate的第二种形式带一个初始值和一个任意的统计函数。

std::for_each是另一个可被用来统计区间的算法，而且它不受accumulate的那些限制。如同accumulate一样，for_each也带两个参数：一个是区间，另一个是函数(通常是函数对象)----对区间中的每个元素都要调用这个函数，但是，传给for_each的这个函数只接收一个实参(即当前的区间元素)。for_each执行完毕后会返回它的函数。(实际上，它返回的是这个函数的一份拷贝。)重要的是，传给for_each的函数(以及后来返回的函数)可以有副作用。

std::for_each和std::accumulate在两个方面有所不同：首先，名字accumulate暗示着这个算法将会计算出一个区间的统计信息。而for_each听起来就好像是对一个区间的每个元素做一个操作。用for_each来统计一个区间是合法的，但是不如accumulate来得清晰。其次，accumulate直接返回我们所要的统计结果，而for_each却返回一个函数对象，我们必须从这个函数对象中提取出我们所要的统计信息。在C++中，这意味着我们必须在函数子类中 加入一个成员函数，以便获得我们想要的统计信息。


## 6 仿函数，仿函数类，函数等等

### Item 38. 遵循按值传递的原则来设计函数子类

```cpp
class Widget38 {};
template<typename T> class BPFC;
 
template<typename T>
class BPFCImpl : public std::unary_function<T, void> {
private:
	Widget38 w; // 原来BPFC中所有数据现在都放在这里
	int x;
	virtual ~BPFCImpl(); // 多态类需要虚析构函数
	virtual void operator()(const T& val) const;
 
	friend class BPFC<T>; // 允许BPFC访问内部数据
};
 
template<typename T>
class BPFC : public std::unary_function<T, void> { // 新的BPFC类:短小、单态
private:
	BPFCImpl<T>* pImpl; // BPFC唯一的数据成员
public:
	void operator()(const T& val) const // 现在这是一个非虚函数，将调用转到BPFCImpl中
	{
		pImpl->operator()(val);
	}
};
```

无论是C还是C++，都不允许将一个函数作为参数传递给另一个函数，相反，你必须传递函数指针。C和C++的标准库函数都遵循这一规则：**函数指针是按值传递的。**

STL函数对象是函数指针的一种抽象和建模形式，所以，按照惯例，在STL中，函数对象在函数之间来回传递的时候也是按值传递(即被拷贝)的。标准库中一个最好的证明是for_each算法，它需要一个函数对象作为参数，同时其返回值也是一个函数对象，而且都是按值传递的。

由于函数对象往往会按值传递和返回，所以，你必须确保你编写的函数对象在经过了传递之后还能正常工作。这意味着两件事：首先，你的函数对象必须尽可能地小，否则拷贝的开销会非常昂贵；其次，函数对象必须是单态的(不是多态的)，也就是说，它们不得使用虚函数。这是因为，如果参数的类型是基类类型，而实参是派生类对象，那么在传递过程中会产生剥离问题(slicing problem)：在对象拷贝过程中，派生部分可能会被去掉，而仅保留了基类部分。

既允许函数对象可以很大并且/或者保留多态性，又可以与STL所采用的按值传递函数子的习惯保持一致的一种两全其美的办法：将所需的数据和虚函数从函数子类中分离出来，放到一个新的类中；然后在函数子类中包含一个指针，指向这个新类的对象。

### Item 39. 确保判别式是”纯函数”

一个判别式(predicate)是一个返回值为bool类型(或者可以隐式地转换为bool类型)的函数。在STL中，判别式有着广泛的用途。标准关联容器的比较函数就是判别式；对于像find_if以及各种与排序有关的算法，判别式往往也被作为参数来传递。

一个纯函数(pure function)是指返回值仅仅依赖于其参数的函数。在C++中，纯函数所能访问的数据应该仅局限参数以及常量(在函数生命期内不会被改变，自然地，这样的常量数据应该被声明为const)。如果一个纯函数需要访问那些可能在两次调用之间发生变化的数据，那么用相同的参数在不同的时刻调用该函数就有可能会得到不同的结果，这将与纯函数的定义相矛盾。

判别式类(predicate class)是一个函数的子类，它的operator()函数是一个判别式，也就是说，它的operator()返回true或者false。STL中凡是能接受判别式的地方，就既可以接受一个真正的判别式，也可以接受一个判别式类的对象。

### Item 40. 若一个类是函数子，则应使它可配接

```cpp
bool isInteresting(const std::string* pw) { return true; }
 
int test_item_40()
{
	std::list<std::string*> strs{ new std::string("xxx"), new std::string("yyy"), new std::string("zzz") };
	std::list<std::string*>::iterator it = std::find_if(strs.begin(), strs.end(), isInteresting);
	std::list<std::string*>::iterator it2 = std::find_if(strs.begin(), strs.end(), std::not1(std::ptr_fun(isInteresting)));
 
	return 0;
}
```
std::ptr_fun仅完成一些类型定义的工作，这些类型定义是std::not1所必须的。在C++11中已不推荐使用std::ptr_fun。

4个标准的函数配接器(not1、not2、bind1st、bind2nd，后两个，在C++11中已不推荐使用)都要求一些特殊的类型定义，那些非标准的、与STL兼容的配接器通常也是如此(例如，Boost提供的STL中就包含了这样的组件)。提供了这些必要的类型定义的函数对象被称为可配接的(adaptable)函数对象，反之，如果函数对象缺少这些类型定义，则称为不可配接的。可配接的函数对象能够与其它STL组件更为默契地协同工作，它们能够应用于更多的上下文环境中，因此你应当尽可能地使你编写的函数对象可以配接。

“这些特殊的类型定义”是argument_type、first_argument_type、second_argument_type以及result_type。不同种类的函数子类所需提供的类型定义也不尽相同，它们是这些名字的不同子集。提供这些类型定义最简便的办法是让函数子从特定的基类继承，或者更准确地说，从一个基结构继承。如果函数子类的operator()只有一个实参，那么它应该从std::unary_function继承；如果函数子类的operator()有两个实参，那么它应该从std::binary_function继承。C++11中已不推荐使用这两个函数。

STL函数对象是C++函数的一种抽象和建模形式，而每个C++函数只有一组确定的参数类型和一个返回类型。所以，STL总是假设每个函数子类只有一个operator()成员函数，并且其参数和返回类型应该吻合unary_function或binary_function的模板参数。

### Item 41. 理解ptr_fun、men_fun和mem_fun_ref的来由

```cpp
class Widget41 {
public:
	void test() {}
};
 
void test(Widget41& w) {}
 
int test_item_41()
{
	std::vector<Widget41> vw;
 
	std::for_each(vw.begin(), vw.end(), test);
	std::for_each(vw.begin(), vw.end(), std::ptr_fun(test));
	std::for_each(vw.begin(), vw.end(), std::mem_fun_ref(&Widget41::test));
 
	std::list<Widget41*> lpw;
	std::for_each(lpw.begin(), lpw.end(), std::mem_fun(&Widget41::test));
 
	return 0;
}
```

std::ptr_fun：将函数指针转换为函数对象。

std::mem_fun：将成员函数转换为函数对象(指针版本)。

std::mem_fun_ref：将成员函数转换为函数对象(引用版本)。

像mem_fun_t这样的类被称为函数对象配接器(function object adapter)。这些由mem_fun和mem_fun_ref产生的对象不仅使得STL组件可以通过同一种语法形式来调用所有的函数，而且它们还提供了一些重要的类型定义，就像fun_ptr所产生的对象一样。

如果你对什么时候该使用ptr_fun，什么时候不该使用ptr_fun感到困惑，那么你可以在每次将一个函数传递给一个STL组件的时候总是使用它。STL不会在意的，而且这样做也不会带来运行时的性能损失。另一种策略是，只有在迫不得已的时候才使用ptr_fun。如果你省略了那些必要的类型定义，编译器就会提醒你，然后你再回去把ptr_fun加上。

mem_fun和mem_fun_ref的情形则截然不同。每次在将一个成员函数传递给一个STL组件的时候，你就要使用它们。因为它们不仅仅引入了一些类型定义(可能是必要的，也可能不是必要的)，而且它们还转换调用语法的形式来适应算法----它们将针对成员函数的调用语法转换为STL组件所使用的调用语法。

C++11中已不再推荐使用ptr_fun、mem_fun、mem_fun_ref等相关函数。

### Item 42. 确保less<T>与operator<具有相同的语义

应该尽量避免修改less的行为，因为这样做很可能会误导其他的程序员。如果你使用了less，无论是显式地或是隐式地，你都需要确保它与operator<具有相同的意义。如果你希望以一种特殊的方式来排序对象，那么最好创建一个特殊的函数子类，它的名字不能是less。



## 7 用STL编程

### Item 43. 算法调用优先于手写的循环

理由：(1).效率：算法通常比程序员自己写的循环效率更高。(2).正确性：自己写循环比使用算法更容易出错。(3).可维护性：使用算法的代码通常比手写循环的代码更加简洁明了。

如果你要做的工作与一个算法所实现的功能很相近，那么用算法调用更好。但是如果你的循环很简单，而若使用算法来实现的话，却要求混合使用绑定器和配接器或者要求一个单独的函数子类，那么，可能使用手写的循环更好。最后，如果你在循环中要做的工作很多，而且又很复杂，则最好使用算法调用。

### Item 44. 容器的成员函数优先于同名的算法

有些STL容器提供了一些与算法同名的成员函数。比如，关联容器提供了count、find、lower_bound、upper_bound和equal_range，而list则提供了remove、remove_if、unique、sort、merge和reverse。大多数情况下，你应该使用这些成员函数，而不是相应的STL算法。这里有两个理由：第一，成员函数往往速度快；第二，成员函数通常与容器(特别是关联容器)结合得更加紧密，这是算法所不能比的。原因在于，算法和成员函数虽然有同样的名称，但是它们所做的事情往往不完全相同。

对于标准的关联容器，选择成员函数而不选择对应的同名算法，这可以带来几方面的好处：第一，你可以获得对数时间的性能，而不是线性时间的性能。第二，你可以使用等价性来确定两个值是否”相同”，而等价性是关联容器的一个本质定义。第三，你在使用map和multimap的时候，将很自然地只考虑元素的键部分，而不是完整的(key, value)对。

remove、remove_if、unique、sort、merge以及reverse这些算法无一例外地需要拷贝list容器中的对象，而专门为list容器量身定做的成员函数则无需任何对象拷贝，它们只是简单地维护好那些将list节点连接起来的指针。list成员函数的行为不同于与其同名的算法的行为。如果真的要从一个容器中删除对象的话，你在调用了remove、remove_if或者unique算法之后，必须紧接着再调用erase；而list的remove、remove_if和unique成员函数则实实在在地删除了元素，所以你无需再调用erase了。在sort算法与list的sort函数之间有一个很重要的区别是，前者根本不能被应用到list容器上，这是因为，list的迭代器是双向迭代器，而sort算法要求随机访问迭代器。在merge算法和list的merge函数之间也存在行为上的隔阂：merge算法是不允许修改其源区间的，而list::merge则总是在修改它所操作的链表。

### Item 45. 正确区分count、find、binary_search、lower_bound、upper_bound和equal_range

在选择具体的查找策略时,由迭代器指定的区间是否是排序的，这是一个至关重要的决定条件。如果区间是排序的，那么通过binary_search、lower_bound、upper_bound和equal_range，你可以获得更快的查找速度(通常是对数时间的效率)。如果迭代器并没有指定一个排序的区间，那么你的选择范围将局限于count、count_if、find以及find_if，而这些算法仅能提供线性时间的效率。

<center> <img src="https://img-blog.csdnimg.cn/20191124142100772.png" style="zoom:80%" /> </center>    

以上这些函数用法参考：https://blog.csdn.net/fengbingchun/article/details/78034969

### Item 46. 考虑使用函数对象而不是函数作为STL算法的参数

```cpp
struct StringSize : public std::unary_function<std::string, std::string::size_type> {
	std::string::size_type operator()(const std::string& s) const
	{
		return s.size();
	}
};
 
int test_item_46()
{
	std::set<std::string> s{ "abc", "cde", "xyzw" };
	std::transform(s.begin(), s.end(), std::ostream_iterator<std::string::size_type>(std::cout, "\n"), std::mem_fun_ref(&std::string::size)); // 3 3 4，普通函数
 
	std::transform(s.begin(), s.end(), std::ostream_iterator<std::string::size_type>(std::cout, "\n"), StringSize()); // 3 3 4, 函数对象
 
	return 0;
}
```
在C/C++中并不能真正地将一个函数作为参数传递给另一个函数。如果我们试图将一个函数作为参数进行传递，则编译器会隐式地将它转换成一个指向该函数的指针，并将该指针传递过去。函数指针参数抑制了内联机制。

以函数对象作为STL算法的参数，这种做法提供了包括效率在内的多种优势。从代码被编译器接受的程度而言，它们更加稳定可靠。当然，普通函数在C++中也是非常实用的，但是就有效使用STL而言，函数对象通常更加实用一些。

### Item 47. 避免产生”直写型”(write-only)的代码

当你编写代码的时候，它看似非常直接和简捷，因为它是由某些基本想法(比如，erase-remove习惯用法加上在find中使用reverse_interator的概念)自然而形成的。然而，阅读代码的人却很难将最终的语句还原成它所依据的思路，这就是”直写型的代码”叫法的来历：虽然很容易编写，但是难以阅读和理解。一段代码是否是”直写型”取决于其读者的知识水平。

### Item 48. 总是包含(#include)正确的头文件

C++标准与C的标准有所不同，它没有规定标准库中的头文件之间的相互包含关系。

总结每个与STL有关的标准头文件中所包含的内容：

(1). 几乎所有的标准STL容器都被声明在与之同名的头文件中，比如vector被声明在<vector>中，list被声明在<list>中，等等。但是<set>和<map>是个例外，<set>中声明了set和multiset，<map>中声明了map和multimap。

(2). 除了4个STL算法以外，其它所有的算法都被声明在<algorithm>中，这4个算法使accumulate、inner_product、adjacent_difference和partial_sum，它们被声明在头文件<numeric>中。

(3). 特殊类型的迭代器，包括istream_iterator和istreambuf_iterator，被声明在<iterator>中。

(4). 标准的函数子(比如less<T>)和函数子配接器(比如not1、bind2nd)被声明在头文件<functional>中。

任何时候如果你使用了某个头文件中的一个STL组件，那么你就一定要提供对应的#include指令，即使你正在使用的STL平台允许你省略#include指令，你也要将它们包含到你的代码中。当你需要将代码移植到其它平台上的时候，移植的压力就会减轻。

### Item 49. 学会分析与STL相关的编译器诊断信息

在一个被声明为const的成员函数内部，该类的所有非静态数据成员都自动被转变为相应的const类型。

一些技巧：

(1). vector和string的迭代器通常就是指针，所以当错误地使用了iterator的时候，编译器的诊断信息中可能会引用到指针类型。例如，如果源代码中引用了vector<double>::iterator，那么编译器的诊断信息中极有可能就会提及double*指针。

(2). 如果诊断信息中提到了back_insert_iterator、front_insert_iterator或者insert_iterator，则几乎总是意味着你错误地调用了back_inserter、front_inserter或者inserter。如果你并没有直接调用这些函数，则一定是你所调用的某个函数直接或者间接地调用了这些函数。

(3). 类似地，如果诊断信息中提到了binder1st或者binder2nd，那么你可能是错误地使用了bind1st和bind2nd。

(4). 输出迭代器(如ostream_iterator、ostreambuf_iterator以及那些由back_inserter、front_inserter、front_inserter和inserter函数返回的迭代器)在赋值操作符内部完成其输出或者插入工作，所以，如果在使用这些迭代器的时候犯了错误，那么你所看到的错误消息中可能会提到与赋值操作符有关的内容。

(5). 如果你得到的错误消息来源于某一个STL算法的内部实现(例如，引起错误的源代码在<algorithm>中)，那也许是你在调用算法的时候使用了错误的类型。例如，你可能使用了不恰当的迭代器类型。

(6). 如果你正在使用一个很常见的STL组件，比如vector、string或者for_each算法，但是从错误消息来看，编译器好像对此一无所知，那么可能是你没有包含相应的头文件。

### Item 50. 熟悉与STL相关的Web站点

书中介绍了SGI STL(已废弃)、STLport、Boost。

个人强烈推荐：http://www.cplusplus.com/ 尤其是其中的Reference(http://www.cplusplus.com/reference/)，有时cplusplus网站会打不开，此时也可参考cppreference：https://en.cppreference.com/w/cpp








# 4  Effective Modern C++42招独家技巧助你改善C++11和C++14的高效用法笔记


这里是Effective Modern C++的笔记：

注：(1).以下测试代码既可以在Windows下执行也可以在Linux执行。(2).个人感觉中文版有些内容不如直接看英文版理解的更透彻，因此下面有些中文也同时给出了对应的英文。

C++11被最广泛接受的特性可能莫过于移动语义，而移动语义的基础在于区分左值表达式和右值表达式。因为，一个对象是右值意味着能够对其实施移动语义，而左值则一般不然。从概念上说(实践上并不总是成立)，右值对应的是函数返回的临时对象，而左值对应的是可指涉的对象，而指涉的途径则无论通过名字、指针，还是左值引用皆可。

有一种甄别表达式是否是左值的实用方法富有启发性，那就是检查能否取得该表达式的地址。如果可以取得，那么该表达式基本上可以判定是左值。如果不可取得，则其通常是右值。这种方法之所以说富有启发性，是因为它让你记得，表达式的型别(type)与它是左值还是右值没有关系。换言之，给定一型别T，则既有T型别的左值，也有T型别的右值。

在函数调用中，调用方的表达式，称为函数的实参。实参的用处，是初始化函数的形参。实参和形参有着重大的区别，因为形参都是左值，而用来作为其初始化依据的实参，则既可能是右值，也可能是左值。

## 1 型别推导 

### Item 1. 理解模板型别推导(Understand template type deduction)
```cpp
//template<typename T>
//void f(ParamType param);
 
template<typename T>
void f(T& param) {} // param是个引用
 
template<typename T>
void f2(T* param) {} // param现在是个指针
 
template<typename T>
void f3(T&& param) {} // param现在是个万能引用
 
template<typename T>
void f4(T param) {} // param现在是按值传递
 
// 以编译期常量形式返回数组尺寸(该数组形参未起名字，因为我们只关系其含有的元素个数)
template<typename T, std::size_t N>
constexpr std::size_t arraySize(T (&)[N]) noexcept // 将该函数声明为constexpr，能够使得其返回值在编译期就可用。从而就可以在
{						   // 声明一个数组时，指定其尺寸和另一数组相同，而后者的尺寸则从花括号初始化式(braced initializer)计算得出
	return N;
}
 
void someFunc(int, double) {} // someFunc是个函数，其型别为void(int, double)
 
int test_item_1()
{
	//f(expr); // 已某表达式调用f
	// 在编译期，编译器会通过expr推导两个型别：一个是T的型别，另一个是ParamType的型别，这两个型别往往不一样
 
	int x = 27; // x的型别是int
	const int cx = x; // cx的型别是const int
	const int& rx = x; // rx是x的型别为const int的引用
 
	f(x); // T的型别是int, param的型别是int&
	f(cx); // T的型别是const int, param的型别是const int&
	f(rx); // T的型别是const int, param的型别是const int&, 注意：即使rx具有引用型别，T也并未被推导成一个引用，原因在于，rx的引用性(reference-ness)会在型别推导过程中被忽略
 
	const int* px = &x; // px is ptr to x as a const int
	f2(&x); // T is int, param's type is int*
	f2(px); // T is const int, param's type is const int*
 
	f3(x); // x is lvalue, so T is int&, param's type is also int&
	f3(cx); // cx is lvalue, so T is const int&, param's type is also const int&
	f3(rx); // rx is lvalue, so T is const int&, param's type is also const int&
	f3(27); // 27 is rvalue, so T is int, param's type is therefore int&&
 
	// param是个完全独立于cx和rx存在的对象----是cx和rx的一个副本
	f4(x); // T's and param's types are both int
	f4(cx); // T's and param's types are again both int
	f4(rx); // T's and param's types are still both int
 
	const char* const ptr = "Fun with pointers"; // ptr is const pointer to const object
	f4(ptr); // pass arg of type const char* const
 
	const char name[] = "J. P. Briggs"; // name's type is const char[13]
	const char* ptrToName = name; // array decays to pointer
 
	f4(name); // name is array, but T deduced as const char*
	f(name); // pass array to f, T的型别推导结果是const char[13], 而f的形参(该数组的一个引用)型别则被推导为const char (&)[13]
 
	int keyVals[] = {1, 3, 7, 9, 11, 22, 35};
	fprintf(stdout, "array length: %d\n", arraySize(keyVals)); // 7
	int mappedVals[arraySize(keyVals)]; // mappedVals被指定与之相同
	std::array<int, arraySize(keyVals)> mappedVals2; // mappedVals2也指定为7个元素
 
	f4(someFunc); // param被推导为函数指针(ptr-to-func)，具体型别是void (*)(int, double)
	f(someFunc); // param被推导为函数引用(ref-to-func), 具体型别是void (&)(int, double)
 
	return 0;
}
```
T的型别推导结果，不仅仅依赖expr的型别，还依赖ParamType的形式。具体要分三种情况讨论：

(1).ParamType具有指针或引用型别，但不是万能引用(universal reference)：若expr具有引用型别，先将引用部分忽略；然后对expr的型别和ParamType的型别执行模式匹配，来决定T的型别。

(2).ParamType是一个万能引用(universal reference)：此类形参的声明方式类似右值引用(即在函数模板中持有型别形参T时，万能引用的声明型别写作T&&)，但是当传入的实参是左值时，其表现会有所不同。如果expr是个左值，T和ParamType都会被推导为左值引用。这个结果具有双重的奇特之处：首先，这是在模板型别推导中，T被推导为引用型别的唯一情形。其次，尽管在声明时使用的是右值引用语法，它的型别推导结果却是左值引用。如果expr是个右值，则应用”常规”(即情况1中的)规则。当遇到万能引用时，型别推导规则会区分实参是左值还是右值。而非万能引用是从来不会做这样的区分的。

(3).ParamType既非指针也非引用：当ParamType既非指针也非引用时，我们面对的就是所谓按值传递了。一如之前，若expr具有引用型别，则忽略其引用部分。忽略expr的引用性之后，若expr是个const对象，也忽略之。若其是个volatile对象，同样忽略之(volatile对象不常用，它们一般仅用于实现设备驱动程序)。

数组实参：数组型别(array type)有别于指针型别，尽管有时它们看起来可以互换。形成这种假象的主要原因是，在很多语境下，数组会退化成指涉到其首元素的指针。**可以利用声明数组引用这一能力创造出一个模板，用来推导出数组含有的元素个数。**

函数实参：数组并非C++中唯一可以退化为指针之物。函数型别也同样会退化成函数指针，并且我们针对数组型别推导的一切讨论都适用于函数及其向函数指针的退化。

要点速记：**(1).在模板型别推导过程中，具有引用型别的实参会被当成非引用型别来处理。换言之，其引用性会被忽略。(2).对万能引用形态进行推导时，左值实参会进行特殊处理。(3).对按值传递的形参进行推导时，若实参型别中带有const或volatile饰词，则它们还是会被当作不带const或volatile饰词的型别来处理。(4).在模板型别推导过程中，数组或函数型别的实参会退化成对应的指针(arguments that are array or function names decay to pointers)，除非它们被用来初始化引用。**

### Item 2. 理解auto型别推导(Understand auto type deduction)

```cpp
//template<typename T>
//void f(ParamType param);
 
void someFunc2(int, double) {} // someFunc是个函数，其型别为void(int, double)
 
/*auto createInitlist()
{
	return {1, 2, 3}; // error: can't deduce type for {1, 2, 3}
}*/
 
int test_item_2()
{
	//f(expr); // 已某表达式调用f
	// 当某变量采用auto来声明时，auto就扮演了模板中的T这个角色，而变量的型别饰词则扮演的是ParamType的角色
	auto x = 27; // x的型别饰词(type specifier)就是auto自身, x既非指针也非引用
	const auto cx = x; // 型别饰词成了const auto, cx既非指针也非引用
	const auto& rx = x; // 型别饰词又成了const auto&, rx是个引用，但不是万能引用
 
	auto&& uref1 = x; // x的型别是int,且是左值，所以uref1的型别是int&
	auto&& uref2 = cx; // cx的型别是const int, 且是左值，所以uref2的型别是const int&
	auto&& uref3 = 27; // 27的型别是int,且是右值，所以uref3的型别是int&&
 
	const char name[] = "R. N. Briggs"; // name的型别是const char[13]
	auto arr1 = name; // arr1's type is const char*
	auto& arr2 = name; // arr2's type is const char (&)[13]
 
	auto func1 = someFunc2; // func1's type is void(*)(int, double)
	auto& func2 = someFunc2; // func2's type is void(&)(int, double)
 
	// 若要声明一个int，并将其初始化为值27，C++98中有两种可选语法
	int x1 = 27;
	int x2(27);
	// 而C++11为了支持统一初始化(uniform initialization),增加了下面的语法选项
	int x3 = {27};
	int x4{27};
 
	auto x1_1 = 27; // type is int, value is 27
	auto x2_1(27); // type is int, value is 27
	auto x3_1 = {27}; // type is std::initializer_list<int>, value is {27}
	auto x4_1{27}; // type is std::initializer_list<int>, value is {27}
	//auto x5_1 = {1, 2, 3.0}; // error, can't deduce T for std::initializer_list<T> 
 
	std::vector<int> v;
	auto resetV = [&v](const auto& newValue) { v = newValue; }; // C++14
	//resetV({1, 2, 3}); // error, can't deduce type for {1, 2, 3}
 
	return 0;
}
```

除了一个奇妙的例外情况以外，auto型别推导就是模板型别推导。在采用auto进行变量声明中，型别饰词取代了ParamType，所以也存在三种情况：(1).型别饰词是指针或引用，但不是万能引用(universal reference)。(2).型别饰词是万能引用。(3).型别饰词既非指针也非引用。

**当用于auto声明变量的初始化表达式是使用大括号括起时，推导所得的型别就属于std::initializer_list**。这么一来，如果型别推导失败(例如，大括号里的值型别不一)，则代码就通不过编译。对于大括号初始化表达式的处理方式是auto型别推导和模板型别推导的唯一不同之处。当采用auto声明的变量使用大括号初始化表达式进行初始化时，推导所得的型别是std::initializer_list的一个实例型别，但模板型别却不会。

C++14允许使用auto来说明函数返回值需要推导，而且C++14中的lambda式也会在形参声明中用到auto。然而，这些auto用法是在使用模板型别推导而非auto型别推导。所以，带有auto返回值的函数若要返回一个大括号括起来的初始化表达式，是通不过编译的。同样地，用auto来指定C++14中lambda式的形参型别时，也不能使用大括号括起的初始化表达式。

**要点速记：(1).在一般情况下，auto型别推导和模板型别推导是一模一样的，但是auto型别推导会假定用大括号括起的初始化表达式代表一个std::initializer_list，但模板型别推导却不会。(2).在函数返回值或lambda式的形参中使用auto，意思是使用模板型别推导而非auto型别推导。**

auto更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/51834927

### Item 3. 理解decltype(Understand decltype)模板

```cpp
class Widget3 {};
bool f5(const Widget3& w) { return true; } // decltype(w) is const Widget3&; decltype(f5) is bool(const Widgeet3&)
 
template<typename Container, typename Index>
// 这里的auto只为说明这里使用了C++11中的返回值型别尾序语法(trailing return type syntax),即该函数的返回值型别将在形参列表之后(在"->"之后)
// 尾序返回值的好处在于，在指定返回值型别时可以使用函数形参
//auto authAndAccess(Container&& c, Index i) -> decltype(std::forward<Container>(c)[i]) // C++11
decltype(auto) authAndAccess(Container&& c, Index i) // C++14, c is now a universal reference
{
	return std::forward<Container>(c)[i];
}
 
struct Point {
	int x, y; // decltype(Point::x) is int; decltype(Point::y) is int
};
 
decltype(auto) ff3_1()
{
	int x = 0;
	return x; // decltype(x)是int,所以ff3_1返回的是int
	//return (x); // decltype((x))是int&,所以ff3_1返回的是int&
}
 
int test_item_3()
{
	const int i = 0; // decltype(i) is const int
	Widget3 w; // decltype(w) is Widget3
	if (f5(w)) {} // decltype(f5(w)) is bool
	std::vector<int> v; // decltype(v) is vector<int>
 
	const Widget3& cw = w;
	auto myWidget31 = cw; // auto型别推导:myWidget31的型别是Widget3
	decltype(auto) myWidget32 = cw; // decltype型别推导:myWidget32的型别是const Widget3&
 
	return 0;
}
```

对于给定的名字或表达式，decltype能告诉你该名字或表达式的型别。与模板和auto的型别推导过程相反，decltype一般只会鹦鹉学舌，返回给定的名字或表达式的确切型别而已。

C++11中，decltype的主要用途大概就在于声明那些返回值型别依赖于形参型别的函数模板。

C++11允许对单表达式的lambda式的返回值型别实施推导，而C++14则将这个允许范围扩张到了一切lambda式和一切函数，包括那些多表达式。

C++14中的decltype(auto)并不限于在函数返回值型别处使用。在变量声明的场合上，若你也想在初始化表达式处应用decltype型别推导规则，也可以照样便宜行事。

容器的传递方式是对非常量的左值引用(lvalue-reference-to-non-const)。

**要点速记：(1).绝大多数情况下，decltype会得出变量或表达式的型别而不作任何修改。(2).对于型别为T的左值表达式，除非该表达式仅有一个名字，否则decltype总是得出型别T&(For lvalue expressions of type T other than names, decltype always reports a type of T&)。(3).C++14支持decltype(auto)，和auto一样，它会从其初始化表达式出发来推导型别，但是它的型别推导使用的是decltype的规则。**

decltype更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52504519

### Item 4. 掌握查看型别推导结果的方法(Know how to view deduced types)
```cpp
int test_item_4()
{
	const int theAnswer = 42;
	auto x = theAnswer; // int
	auto y = &theAnswer; // const int*
	fprintf(stdout, "%s, %s\n", typeid(x).name(), typeid(y).name());
 
	return 0;
}
```
IDE编辑器：IDE中的代码编辑器通常会在你将鼠标指针悬停至某个程序实体，如变量、形参、函数等时，显示出该实体的型别。IDE显示的型别信息不可靠。

编译器诊断信息：想要让编译器显示其推导出的型别，一条有效的途径是使用该型别导致某些编译错误。而报告错误的消息几乎肯定会提及导致该错误的型别。

运行时输出：使用printf来显示型别信息，这种方法只有到了运行期才能使用，却可以对于型别输出的格式提供完全的控制。std::type_info::name并不可靠。

**要点速记：(1).利用IDE编辑器、编译器错误信息和Boost.TypeIndex库常常能够查看到推导而得的型别。(2).有些工具产生的结果可能会无用，或者不准确。所以理解C++型别推导规则是必要的。**




## 2 auto 


### Item 5. 优先选用auto，而非显式型别声明(Prefer auto to explicit type declarations)
```cpp
class Widget5 {};
bool operator<(const Widget5& lhs, const Widget5& rhs) { return true; }
 
int test_item_5()
{
	int x1; // potentially uninitialized
	//auto x2; // error, initializer required
	auto x3 = 0; // fine, x's value is well-defined
 
	auto derefUPLess = [](const std::unique_ptr<Widget5>& p1, const std::unique_ptr<Widget5>& p2) { return *p1 < *p2; }; // comparison func. for Widget5 pointed to by std::unique_ptrs
	auto derefLess = [](const auto& p1, const auto& p2) { return *p1 < *p2; }; // C++14 comparison function for values pointed to by anything pointer-like
 
	// bool(const std::unique_ptr<Widget5>&, const std::unique_ptr<Widget5>&) // C++11 signature for std::unique_ptr<Widget5> comparison function
	std::function<bool(const std::unique_ptr<Widget5>&, const std::unique_ptr<Widget5>&)> func;
 
	// 在C++11中，不用auto也可以声明derefUPLess
	std::function<bool(const std::unique_ptr<Widget5>&, const std::unique_ptr<Widget5>&)> derefUPLess2 = [](const std::unique_ptr<Widget5>& p1, const std::unique_ptr<Widget5>& p2) { return *p1 < *p2; };
 
	std::vector<int> v{1, 2, 3};
	unsigned sz1 = v.size(); // 不推荐,32位和64位windows上,unsigned均是32位，而在64位windows上，std::vector<int>::size_type则是64位
	auto sz2 = v.size(); // 推荐，sz2's type is std::vector<int>::size_type
	
	std::unordered_map<std::string, int> m;
	for (const std::pair<std::string, int>& p : m) {} // 显式型别声明，不推荐,the key part of a std::unordered_map is const， so the type of std::pair in the hash table is std::<const std::string, int>, 需要进行隐式转换，会产生临时对象
	for (const auto& p : m) {} // 推荐
 
	return 0;
}
```

用auto声明的变量必须初始化。

在C++14中，lambda表达式的形参都可以使用auto。

std::function是C++11标准库中的一个模板。函数指针只能指涉(point)到函数，而std::function却可以指涉(refer to)任何可调用对象，即任何可以像函数一样实施调用之物。正如你若要创建一个函数指针就必须指定欲指涉到的函数的型别(即该指针指涉到的函数的签名)，你若要创建一个std::function对象就必须指定欲指涉的函数的型别。

使用std::function和使用auto有所不同：使用auto声明的、存储着一个闭包(closure)的变量和该闭包是同一型别，从而它要求的内存量也和该闭包一样。而使用std::function声明的、存储着一个闭包的变量是std::function的一个实例，所以不管给定的签名(signature)如何，它都占有固定尺寸的内存，而这个尺寸对于其存储的闭包而言并不一定够用。如果是这样的话，std::function的构造函数就会分配堆上的内存来存储该闭包。从结果上看，std::function对象一般都会比使用auto声明的变量使用更多内存。通过std::function来调用闭包几乎必然会比通过使用auto声明的变量来调用同一闭包要来得慢。

std::function更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52562918

auto也并不完美，每个auto变量的型别都是从它的初始化表达式推导出来的，而有些初始化表达式的型别既不符合期望也不符合要求。

显式的写出型别经常是画蛇添足，带来各种微妙的偏差，有些关乎正确性，有些关乎效率，或是两者都受影响。还有，auto型别可以随着其初始化表达式的型别变化而自动随之改变。

要点速记：**(1).auto变量必须初始化，基本上对会导致兼容性和效率问题的型别不匹配现象免疫，还可以简化重构流程，通常也比显式指定型别要少打一些字。(2).auto型别的变量都有着条款2和条款6中所描述的毛病。**

auto更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/51834927

### Item 6. 当auto推导的型别不符合要求时，使用带显式型别的初始化物习惯用法(Use the explicitly typed initializer idiom when auto deduces undesired types)

```cpp
class Widget6 {};
std::vector<bool> features(const Widget6& w)
{
	return std::vector<bool>{true, true, false, false, true, false};
}
 
void processWidget6(const Widget6& w, bool highPriority) {}
 
double calcEpsilon() { return 1.0; }
 
int test_item_6()
{
	Widget6 w;
	bool highPriority =features(w)[5]; // 正确,显式声明highPriority的型别
	processWidget6(w, highPriority);
 
	// 把highPriority从显示型别改成auto
	auto highPriority2 = features(w)[5]; // highPriority2的型别由推导而得,std::vector<bool>的operator[]的返回值并不是容器中一个元素的引用(单单bool是个例外),返回的是个std::vector<bool>::reference型别的对象,返回一个std::vector<bool>型别的临时对象
	processWidget6(w, highPriority2); // undefined behavior, highPriority2 contains dangling pointer(空悬指针)
 
	auto highPriority3 = static_cast<bool>(features(w)[5]); // 正确
	processWidget6(w, highPriority3);
 
	float ep = calcEpsilon(); // 隐式转换 double-->float,这种写法难以表明"我故意降低了函数的返回值精度"
	auto ep2 = static_cast<float>(calcEpsilon()); // 推荐
 
	return 0;
}
```

std::vector<bool>是vector的特殊版本，用于bool类型的元素并优化空间，存储每个值仅占用一个位而不是一个字节(each value is stored in a single bit)。

std::vector<bool>::reference是个代理类的实例。所谓代理类，就是指为了模拟或增广其它型别的类(a class that exists for the purpose of emulating and augmenting the behavior of some other type)。一个普遍的规律是，”隐形”代理类和auto无法和平共处。问题在于auto没有推导成为你想推导出来的型别。解决方案应该是强制进行另一次型别转换，这种方法称为带显式型别的初始化物习惯用法。

带显式型别的初始化物习惯用法要求使用auto声明变量，但针对初始化表达式进行强制型别转换，转换成你想要auto推导出来的型别。

**要点速记：(1).”隐形”的代理型别可以导致auto根据初始化表达式推导出”错误的”型别。(2).带显式型别的初始化物习惯用法强制auto推导出你想要的型别。**





## 3 转向现代C++

### Item 7. 在创建对象时注意区分()和{}(Distinguish between()and{}when creating objects)
```cpp
class Widget7 {
public:
	Widget7(int i, bool b) {} // constructor not declaring std::initializer_list params
	Widget7(int i, double d) {}
	Widget7(std::initializer_list<long double> il) { fprintf(stdout, "std::initializer_list params\n"); }
	Widget7() = default;
	Widget7(int) {}
 
	operator float() const { return 1.0f; } // 强制转换成float型别, 注意：此函数的作用,下面的w13和w15
 
private:
	int x{0}; // fine, x's default value is 0
	int y = 0; // also fine
	//int z(0); // error
};
 
int test_item_7()
{
	int x(0); // 初始化物在小括号内
	int y = 0; // 初始化物在等号之后
	int z{0}; // 初始化物在大括号内
	int z2 = {0}; // 使用等号和大括号来指定初始化物，一般C++会把它和只有大括号的语法同样处理
 
	Widget7 w1; // call default constructor
	Widget7 w2 = w1; // not an assignment; calls copy constructor
	w1 = w2; // an assignment; calls copy operator=
 
	std::vector<int> v{1, 3, 5}; // v's initial content is 1, 3, 5
 
	std::atomic<int> ai1{0}; // fine
	std::atomic<int> ai2(0); // fine
	//std::atomic<int> ai3 = 0; // error
 
	double a{std::numeric_limits<double>::max()}, b{std::numeric_limits<double>::max()}, c{std::numeric_limits<double>::max()};
	//int sum1{a + b + c}; // error, sum of doubles may not be expressible as int
	int sum2(a + b + c); // okey(value of expression truncated to an int)
	int sum3 = a + b + c; // okey(value of expression truncated to an int)
 
	Widget7 w3(10); // call Widget7 constructor with argument 10
	Widget7 w4(); // most vexing parse! declares a function named w4 that returns a Widget7
	Widget7 w5{}; // call Widget7 constructor with no args
 
	Widget7 w6(10, true); // calls first constructor
	Widget7 w7{10, true}; // alse calls first constructor, 假设没有Widget7(std::initializer_list<long double>)构造函数
	Widget7 w8(10, 5.0); // calls second constructor
	Widget7 w9{10, 5.0}; // also calls second constructor, 假设没有Widget7(std::initializer_list<long double>)构造函数
 
	Widget7 w10{10, true}; // 使用大括号，调用的是带有std::initializer_list型别形参的构造函数(10和true被强制转换为long double)
	Widget7 w11{10, 5.0}; // 使用大括号，调用的是带有std::initializer_list型别形参的构造函数(10和5.0被强制转换为long double) 
 
	Widget7 w12(w11); // 使用小括号，调用的是拷贝构造函数
	Widget7 w13{w11}; // 使用大括号，调用的是带有std::initializer_list型别形参的构造函数(w11的返回值被强制转换成float,随后float又被强制转换成long double)
	Widget7 w14(std::move(w11)); // 使用小括号，调用的是移动构造函数
	Widget7 w15{std::move(w11)}; // 使用大括号，调用的是带有std::initializer_list型别形参的构造函数(和w13的结果理由相同)
	
	Widget7 w16{}; // call Widget7 constructor with no args,调用无参的构造函数，而非调用带有std::initializer_list型别形参的构造函数
	Widget7 w17({}); // 调用带有std::initializer_list型别形参的构造函数，传入一个空的std::initializer_list
	Widget7 w18{{}}; // 调用带有std::initializer_list型别形参的构造函数，传入一个空的std::initializer_list
 
	std::vector<int> v1(10, 20); // 调用了形参中没有任何一个具备std::initializer_list型别的构造函数，结果是：创建了一个含有10个元素的std::vector，所有的元素的值都是20
	std::vector<int> v2{10, 20}; // 调用了形参中含有std::initializer_list型别的构造函数，结果是：创建了一个含有2个元素的std::vector，元素的值分别为10和20
	fprintf(stdout, "v1 length: %d, v2 length: %d\n", v1.size(), v2.size());
 
	return 0;
}
```

指定初始化值的方式包括使用小括号、使用等号，或是使用大括号。

C++11引入了统一初始化(uniform initialization)：单一的、至少从概念上可以用于一切场合、表达一切意思的初始化。它的基础是大括号形式或称为大括号初始化(braced initialization)。

大括号同样可以用来为非静态成员指定默认初始化值，这项能力(在C++11中新加入的能力)也可以使用”=”的初始化语法，却不能使用小括号。

不可复制的对象(如std::atomic型别的对象)可以采用大括号和小括号来进行初始化，却不能使用”=”。

大括号初始化有一项新特性，就是它禁止内建型别之间进行隐式窄化型别转换(narrowing conversion)。如果大括号内的表达式无法保证能够采用进行初始化的对象来表达，则代码不能通过编译。而采用小括号和”=”的初始化则不会进行窄化型别转换检查。

大括号初始化的另一项值得一提的特征是，它对于C++的最令人苦恼之解析语法(most vexing parse)免疫。C++规定：任何能够解析为声明的都要解析为声明，而这会带来副作用。所谓最令人苦恼之解析语法就是说，程序员本来想要以默认方式构造一个对象，结果却一不小心声明了一个函数。这个错误的根本原因在于构造函数调用语法。

**大括号初始化的缺陷在于伴随它有时会出现的意外行为**：这种行为源于大括号初始化物、std::initializer_list以及构造函数重载决议之间的纠结关系。这几者之间的相互作用可以使得代码看起来是要做某一件事，但实际上是在做另一件事。如果使用大括号初始化物来初始化一个使用auto声明的变量，那么推导出来的型别就会成为std::initializer_list，尽管用其它方式使用相同的初始化物来声明变量就能够得出更符合直觉的型别。在构造函数被调用时，只要形参中没有任何一个具备std::initializer_list型别，那么小括号和大括号的意义就没有区别。**如果，有一个或多个构造函数声明了任何一个具备std::initializer_list型别的形参，那么采用了大括号初始化语法的调用语句会强烈地优先选用带有std::initializer_list型别形参的重载版本**。即使是平常会执行复制(copy)或移动的构造函数也可能被带有std::initializer_list型别形参的构造函数劫持。只有在找不到任何办法把大括号初始化物中的实参转换成std::initializer_list模板中的型别时，编译器才会退而去检查普通的重载决议(normal overload resolution)。空大括号对表示的是”没有实参”，而非”空的std::initializer_list”。

要点速记：**(1).大括号初始化可以应用的语境最为宽泛，可以阻止隐式窄化型别转换，还对最令人苦恼之解析语法免疫。(2).在构造函数重载决议期间，只要有任何可能，大括号初始化物就会与带有std::initializer_list型别的形参相匹配，即使其它重载版本有着貌似更加匹配的形参表。(3).使用小括号还是大括号，会造成结果大相径庭的一个例子是：使用两个实参来创建一个std::vector<数值型别>对象。(4).在模板内容进行对象创建时，到底应该使用小括号还是大括号会成为一个棘手问题。**

### Item 8. 优先选用nullptr，而非0或NULL(Prefer nullptr to 0 and NULL)

```cpp
void f8(int) { fprintf(stdout, "f8(int)\n"); }
void f8(bool) { fprintf(stdout, "f8(bool)\n"); }
void f8(void*) { fprintf(stdout, "f8(void*)\n"); }
 
class Widget8 {};
int f8_1(std::shared_ptr<Widget8> spw) { return 0; }
double f8_2(std::unique_ptr<Widget8> upw) { return 1.f; }
bool f8_3(Widget8* pw) { return false; }
 
template<typename FuncType, typename MuxType, typename PtrType>
//auto lockAddCall(FuncType func, MuxType& mutex, PtrType ptr) -> decltype(func(ptr)) // C++11
decltype(auto) lockAndCall(FuncType func, MuxType& mutex, PtrType ptr) // C++14
{
	using MuxGuard = std::lock_guard<std::mutex>; // C++11 typedef
	MuxGuard g(mutex);
	return func(ptr);
}
 
int test_item_8()
{
	f8(0); // calls f8(int), not f8(void*)
	//f8(NULL); // might not compile, but typically calls f8(int), never calls f8(void*)
	f8(nullptr); // calls f(void*) overload
 
	std::mutex f1m, f2m, f3m;
	//auto result1 = lockAndCall(f8_1, f1m, 0); // error, ‘void result1’ has incomplete type
	//auto result2 = lockAndCall(f8_2, f2m, NULL); // error: ‘void result2’ has incomplete type
	auto result3 = lockAndCall(f8_3, f3m, nullptr);
 
	return 0;
}
```

字面常量0的型别是0，而非指针。当C++在只能使用指针的语境中发现了一个0，它也会把它勉强解释为空指针，但说到底这是一个不得已而为之的行为。C++的基本观点还是0的型别是int，而非指针。

nullptr的优点在于，它不具备整型型别。实话实说，它也不具备指针型别(pointer type)。nullptr的实际型别是std::nullptr_t，std::nullptr_t的定义被指定为nullptr的型别。型别std::nullptr_t可以隐式转换到所有的裸指针型别(raw pointer type)，这就是为何nullptr可以扮演所有型别指针的原因。

**要点速记：(1).相对于0或NULL，优先选用nullptr。(2).避免在整型和指针型别之间重载。**

nullptr更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/51793497

### Item 9. 优先选用别名声明，而非typedef(Prefer alias declarations to typedefs)
```cpp
class Widget9 {};
 
typedef void (*FP1)(int, const std::string&);
using FP2 = void (*)(int, const std::string&);
 
template<typename T>
using MyAllocList1 = std::list<T/*, MyAlloc<T>*/>; // C++11,  MyAllocList1<T>是std::list<T, MyAlloc<T>>的同义词
 
template<typename T>
struct MyAllocList2 { // MyAllocList<T>::type 是std::list<T, MyAlloc<T>>的同义词
	typedef std::list<T/*, MyAlloc<T>*/> type;
};
 
template<typename T>
class Widget9_2 { // Widget9_2<T>含一个MyAllocList2<T>型别的数据成员
private:
	typename MyAllocList2<T>::type list; // MyAllocList2<T>::type代表一个依赖于模板型别形参(T)的型别，所以MyAllocList2<T>::type称为带依赖型别，C++中规则之一就是带依赖型别必须前面加个typename
};
 
template<typename T>
class Widget9_1 {
private:
	MyAllocList1<T> list; // 不再有"typename"和"::type"
};
 
int test_item_9()
{
	typedef std::unique_ptr<std::unordered_map<std::string, std::string>> UPtrMapSS1;
	using UPtrMapSS2 = std::unique_ptr<std::unordered_map<std::string, std::string>>; // C++11, alias declaration
 
	MyAllocList1<Widget9> lw1;
	MyAllocList2<Widget9>::type lw2;
 
	typedef const char cc;
	std::remove_const<cc>::type a; // char a
	std::remove_const<const char*>::type b; // const char* b
 
	typedef int&& rval_int;
	typedef std::remove_reference<int>::type A;
 
	//std::remove_const<T>::type // C++11: const T --> T
	//std::remove_const_t<T>     // C++14中的等价物
	//template<class T>
	//using remove_const_t = typename remove_const<T>::type;
 
	//std::remove_reference<T>::type // C++11: T&/T&& --> T
	//std::remove_reference_t<T>     // C++14中的等价物
	//template<class T>
	//using remove_reference_t = typename remove_reference<T>::type;
 
	return 0;
}
```

别名声明可以模板化(这种情况下它们被称为别名模板，alias template)，typedef就不行。

C++11以型别特征(type trait)的形式给了程序员以执行此类变换的工具。型别特征是在头文件<type_traits>给出的一整套模板。该头文件中有几十个型别特征，它们并非都是执行型别变换功能的用途，但其中派此用途的部分则提供了可预测的接口。

每个C++11中的变换std::transformation<T>::type，都有一个C++14中名为std::transformation_t的对应别名模板。

**要点速记：(1).typedef不支持模板化，但别名声明支持。(2).别名模板可以让人免写”::type”后缀，并且在模板内，对于内嵌typedef的引用经常要求加上typename前缀。**

别名声明更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/81259210

### Item 10. 优先选用限定作用域的枚举型别，而非不限作用域的枚举型别(Prefer scoped enums to unscoped enums)

```cpp
std::vector<std::size_t> primeFactors(std::size_t x) { return std::vector<std::size_t>(); }
 
enum class Status; // 前置声明, 默认底层型别(underlying type)是int
enum class Status2: std::uint32_t; // Status2的底层型别是std::uint32_t
enum Color: std::uint8_t; // 不限范围的枚举型别的前置声明，底层型别是std::uint8_t
 
int test_item_10()
{
	enum Color1 { black, white, red }; // 不限范围的(unscoped)枚举型别：black, white, red所在作用域和Color1相同
	//auto white = false; // error, white already declared in this scope
	Color1 c1 = black;
 
	enum class Color2 { black2, white2, red2 }; // C++11, 限定作用域的(scoped)枚举型别:black2, white2, red2所在作用域被限定在Color2内
	auto white2 = false; // 没问题，范围内并无其它"white2"
	//Color2 c1 = black2; // 错误，范围内并无名为"black2"的枚举量
	Color2 c2 = Color2::black2; // fine
	auto c3 = Color2::black2; // also fine
 
	if (c1 < 14.5) // 将Color1型别和double型别值作比较,怪胎
		auto factors = primeFactors(c1);
 
	//if (c2 < 14.5) // 错误，不能将Color型别和double型别值作比较
	//	auto facotrs = primeFactors(c2); // 错误，不能将Color2型别传入要求std::size_t型别形参的函数
 
	return 0;
}
```

一个通用规则，如果在一对大括号里声明一个名字，则该名字的可见性就被限定在括号括起来的作用域内。但这个规则不适用于C++98风格的枚举型别中定义的枚举量。这些枚举量的名字属于包含着这个枚举型别的作用域，这就意味着在此作用域内不能有其它实体取相同的名字。

由于限定作用域的枚举型别是通过”enum class”声明的，所有有时它们也被称为枚举类。

限定作用域的枚举型别带来的名字空间污染降低，已经是”应该优先选择它，而不是不限范围的枚举型别”的足够理由。但是限定作用域的枚举型别还有第二个优势：它的枚举量是更强型别的(strongly typed)。不限范围的枚举型别中的枚举量可以隐式转换到整数型别(并能够从此处进一步转换到浮点型别)。**从限定作用域的枚举型别到任何其它型别都不存在隐式转换路径。**限定作用域的枚举型别可以进行前置声明，C++11中，不限范围的枚举型别也可以进行前置声明，但须得在完成一些额外工作之后。这些额外工作是由以下事实带来的：一切枚举型别在C++里都会由编译器来选择一个整数型别作为其底层型别。

为了节约使用内存，编译器通常会为枚举型别选用足够表示枚举量取值的最小底层型别。在某些情况下，编译器会用空间来换取时间，而在这样的情况下，它们可能会不选择只具备最小可容尺寸的型别，但是它们当然需要具备优化空间的能力。为了使这种设计成为可能，C++98就只提供了枚举型别定义(即列出所有枚举量)的支持，枚举型别声明则不允许。

限定作用域的枚举型别的底层型别(underlying type)是已知的，默认地是int；而对于不限范围的枚举型别，你可以指定这个底层型别。如果要指定不限范围的枚举型别的底层型别，做法和限定作用域的枚举型别一样。这样作了以后，不限范围的枚举型别也能够进行前置声明了。底层型别指定同样也可以在枚举型别定义中进行。

**要点速记：(1).C++98风格的枚举型别，现在称为不限范围的枚举型别。(2).限定作用域的枚举型别仅在枚举型别内可见。它们只能通过强制型别转换以转换至其它型别。(3).限定作用域的枚举型别和不限范围的枚举型别都支持底层型别指定。限定作用域的枚举型别的默认底层型别是int，而不限范围的枚举型别没有默认底层型别。(4).限定作用域的枚举型别总是可以进行前置声明，而不限范围的枚举型别却只有在指定了默认底层型别的前提下才可以进行前置声明。**

enum class更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/78535754

### Item 11. 优先选用删除函数，而非private未定义函数(Prefer deleted functions to private undefined ones)

```cpp
class Widget11 {
public:
	Widget11(const Widget11&) = delete;
	Widget11& operator=(const Widget11&) = delete;
 
	template<typename T>
	void processPointer(T* ptr) {}
};


template<>
void Widget11::processPointer<void>(void*) = delete;

bool isLucky(int number) { return false; } // 原始版本
bool isLucky(char) = delete; // 拒绝char型别
bool isLucky(bool) = delete; // 拒绝bool型别
bool isLucky(double) = delete; // 拒绝double和float型别

template<typename T>
void processPointer(T* ptr) {}
template<>
void processPointer<void>(void*) = delete; // 不可以使用void*来调用processPointer
template<>
void processPointer<char>(char*) = delete; // 不可以使用char*来调用processPointer

int test_item_11()
{
	//if (isLucky('a')) {} // error, call to deleted function

	return 0;
}
```

C++98中为了阻止个别成员函数的使用采取的做法是声明其为private，并且不去定义它们。在C++11中，有更好的途径来达成效果上相同的结果：使用”=delete。删除函数无法通过任何方法使用。习惯上，删除函数会被声明为public，而非private。任何函数都能成为删除函数(any function may be deleted)，但只有成员函数能声明为private。还有一个妙处是删除函数能做到而private成员函数做不到的，那就是阻止那些不应该进行的模板具现(template instantiation)。

指针世界中有两个异类：一个是void*指针，因为无法对其执行提领(dereference)、自增、自减等操作。还有一个是char*指针，因为它们基本上表示的是C风格的字符串，而不是指涉到单个字符的指针。

**要点速记：(1).优先选用删除函数(deleted function)，而非private未定义函数。(2).任何函数都可以deleted，包括非成员函数和模板具现(template instantiation)。**

“= delete;”更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52475108

### Item 12. 为意在改写的函数添加override声明(Declare overriding functions override)
```cpp
class Base {
public:
	virtual void doWork() {} // 基类中的虚函数
};
 
class Derived : public Base {
public:
	virtual void doWork() override {} // 改写(override)了Base:doWork(“virtual”在这可写可不写)
};
 
class Widget12 {
public:
	void doWork() & { fprintf(stdout, "&\n"); } // 这个版本的doWork仅在*this是左值时调用
	void doWork() && { fprintf(stdout, "&&\n"); } // 这个版本的doWork仅在*this是右值时调用
 
	using DataType = std::vector<double>;
	DataType& data() & { fprintf(stdout, "data() &\n"); return values; } // 对于左值Widget12型别，返回左值
	DataType data() && { fprintf(stdout, "data() &&\n"); return std::move(values); } // 对于右值Widget12型别，返回右值
 
private:
	DataType values;
};
 
Widget12 makeWidget() // 工厂函数(返回右值)
{
	Widget12 w;
	return w;
}
 
void doSomething(Widget12& w) {} // 仅接受左值的Widget12型别
void doSomething(Widget12&& w) {} // 仅接受右值的Widget12型别
 
int test_item_12()
{
	std::unique_ptr<Base> upb = std::make_unique<Derived>(); // 创建基类指针，指涉到派生类对象
	upb->doWork(); // 通过基类指针调用doWork,结果是派生类函数被调用
 
	Widget12 w; // 普通对象(左值)
	w.doWork(); // 以左值调用Widget12::doWork(即Widget12::doWork &)
	makeWidget().doWork(); // 以右值调用Widget12::doWork(即Widget12::doWork &&)
 
	auto vals1 = w.data(); // 调用Widget12::data的左值重载版本,vals1拷贝构造完成初始化
	auto vals2 = makeWidget().data(); // 调用Widget12::data的右值重载版本，vals2采用移动构造完成初始化
 
	return 0;
}
```

如果要改写(override)真的发生，有一系列要求必须满足：(1).基类中的函数必须是虚函数。(2).基类和派生类中的函数名字必须完全相同(析构函数例外)。(3).基类和派生类中的函数形参型别必须完全相同。(4).基类和派生类中的函数常量性(constness)必须完全相同。(5).基类和派生类中的函数返回值和异常规格(exception specification)必须兼容。除了C++98给出的这些限制，C++11又加了一条。(6).基类和派生类中的函数引用饰词(reference qualifier)必须完全相同。引用饰词是为了实现限制成员函数仅用于左值或右值。带有引用饰词的成员函数，不必是虚函数。

C++11提供了一种方法来显式地标明派生类中的函数是为了改写(override)基类版本：为其加上override声明。

成员函数引用饰词的作用就是针对发起成员函数调用的对象，即*this，加一些区分度。这和在成员函数声明末尾加一个const的情形一模一样：后者表明发起成员函数调用的对象，即*this，应为const。

**要点速记：(1).为意在改写的函数添加override声明。(2).成员函数引用饰词使得对于左值和右值对象(*this)的处理能够区分开来。**

override更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52304284

### Item 13. 优先选用const_iterator，而非iterator(Prefer const_iterators to iterators)
```cpp
template<class C>
auto cbegin_(const C& container) -> decltype(std::begin(container)) // cbegin的一个实现
{
	return std::begin(container);
}
 
template<class C>
auto cend_(const C& container) -> decltype(std::end(container)) // cend的一个实现
{
	return std::end(container);
}
 
int test_item_13()
{
	std::vector<int> values{1, 10, 1000};
	auto it = std::find(values.cbegin(), values.cend(), 1983); // use cbegin and cend
	values.insert(it, 1998);
 
#ifdef _MSC_VER
	auto it2 = std::find(std::cbegin(values), std::cend(values), 1983); // C++14,非成员函数版本的cbegin, cend, gcc 4.9.4 don't support
	values.insert(it2, 1998);
#endif
 
	auto it3 = std::find(cbegin_(values), cend_(values), 1983);
	values.insert(it3, 1998);
 
	return 0;
}
```
const_iterator是STL中相当于指涉到const的指针的等价物。它们指涉到不可被修改的值。**任何时候只要你需要一个迭代器而其指涉到的内容没有修改必要，你就应该使用const_iterator。**

**要点速记：(1).优先选用const_iterator，而非iterator。(2).在最通用的代码中，优先选用非成员函数版本的begin、end和rbegin等，而非其成员函数版本。**

### Item 14. 只要函数不会发射异常，就为其加上noexcept声明(Declare functions noexcept if they won’t emit exceptions)

```cpp
int f14_1(int x) throw() { return 1; } // f14_1不会发射异常: C++98风格
int f14_2(int x) noexcept { return 2; } // f14_2不会发射异常: C++11风格
 
//RetType function(params) noexcept; // 最优化
//RetType function(params) throw(); // 优化不够
//RetType function(params); // 优化不够
 
int test_item_14()
{
	return 0;
}
```

在C++11中，无条件的noexcept就是为了不会发射异常(emit exception)的函数而准备的。函数是否带有noexcept声明，就和成员函数是否带有const声明是同等重要的信息。当你明明知道一个函数不会发射异常却未给它加上noexcept声明的话，这就是接口规格缺陷。对不会发射异常的函数应用noexcept声明还有一个动机，那就是它可以让编译器生成更好的目标代码。

在带有noexcept声明的函数中，优化器不需要在异常传出函数的前提下，将执行期栈(runtime stack)保持在可开解状态(unwindable state)；也不需要在异常溢出函数的前提下，保证所有其中的对象以其被构造顺序的逆序完成析构。而那些以”throw()”异常规格(exception specification)声明的函数就享受不到这样的优化灵活性，和没有加异常规格声明的函数一样。

**要点速记：(1).noexcept声明是函数接口的组成部分，这意味着调用方可能会对它有依赖。(2).相对于不带noexcept声明的函数，带有noexcept声明的函数有更多的机会得到优化。(3).noexcept性质对于移动操作、swap、内存释放函数和析构函数最有价值。(4).大多数函数都是异常中立的，不具备noexcept性质。**

### Item 15. 只要有可能使用constexpr，就使用它(Use constexpr whenever possible)

```cpp
// pow前面写的那个constexpr并不表明pow要返回一个const值，它表明的是如果base和exp是编译期常量，pow的返回结果
// 就可以当一个编译期常量使用；如果base和exp中有一个不是编译期常量，则pow的返回结果就将在执行期计算
constexpr int pow(int base, int exp) noexcept // pow is a constexpr func that never throws
{
	return (exp == 0 ? 1 : base * pow(base, exp - 1)); // C++11
	//auto result = 1; // C++14
	//for (int i = 0; i < exp; ++i) result *= base;
	//return result;
}
 
auto readFromDB(const std::string& str) { return 1; }
 
class Point15 {
public:
	constexpr Point15(double xVal = 0, double yVal = 0) noexcept : x(xVal), y(yVal) {}
	constexpr double xValue() const noexcept { return x; }
	constexpr double yValue() const noexcept { return y; }
	void setX(double newX) noexcept { x = newX; }
	//constexpr void setX(double newX) noexcept { x = newX; } // C++14
	void setY(double newY) noexcept { y = newY; }
	//constexpr void setY(double newY) noexcept { y = newY; } // C++14
private:
	double x, y;
};
 
constexpr Point15 midpoint(const Point15& p1, const Point15& p2) noexcept
{
	return { (p1.xValue() + p2.xValue()) / 2, (p1.yValue() + p2.yValue()) / 2}; // call constexpr member function
}
 
int test_item_15()
{
	int sz = 0; // non-constexpr variable
	//constexpr auto arraySize1 = sz; // error, sz's value not known at compilation
	//std::array<int, sz> data1; // error, sz's value not known at compilation
	constexpr auto arraySize2 = 10; // fine, 10 is a compile-time constant
	std::array<int, arraySize2> data2; // fine, arraySize2 is constexpr
 
	// 注意：const对象不一定经由编译器已知值来初始化
	const auto arraySize3 = sz; // fine, arraySize3 is const copy of sz,arraySize3是sz的一个const副本
	//std::array<int arraySize3> data3; // error, arraySize3.s value not known at compilation
 
	constexpr auto numConds = 5;
	std::array<int, pow(3, numConds)> results; // results has 3^numConds elements
 
	auto base = readFromDB("base"); // get these values at runtime
	auto exp = readFromDB("exponent");
	auto baseToExp = pow(base, exp); // call pow function at runtime
 
	constexpr Point15 p1(9.4, 27.7); // fine, "runs" constexpr constructor during compilation
	constexpr Point15 p2(28.8, 5.3);
 
	constexpr auto mid = midpoint(p1, p2); // 使用constexpr函数的结果来初始化constexpr对象
 
	return 0;
}
```

当constexpr应用于对象时，其实就是一个加强版的const。但应用于函数时，你既不能断定它是const，也不能假定其值在编译阶段就已知。

所有constexpr对象都是const对象，但并非所有的const对象都是constexpr对象。如果你想让编译器提供保证，让变量拥有一个值，用于要求编译期常量的语境，那么能达到这个目的的工具是constexpr，而非const。

constexpr函数可以用在要求编译期常量的语境中。在这样的语境中，若你传给一个constexpr函数的实参值是在编译期已知的，则结果也会在编译期计算出来。如果任何一个实参值在编译期未知，则你的代码将无法通过编译。在调用constexpr函数时，若传入的值有一个或多个在编译期未知，则它的运作方式和普通函数无异，亦即它也是在运行期执行结果的计算。

**在C++11中，constexpr函数不得包含多于一个可执行语句，即一条return语句。在C++14中没有这样的限制。**

**要点速记：(1).constexpr对象都具备const属性，并由编译期已知的值完成初始化。(2).constexpr函数在调用时若传入的实参值是编译期已知的，则会产生编译期结果。(3).比起非constexpr对象或非constexpr函数而言，constexpr对象或constexpr函数可以用在一个作用域更广的语境中。(4).constexpr是对象或函数接口的一部分。**

### Item 16. 保证const成员函数的线程安全性(Make const member functions thread safe)
```cpp
class Point16 { // 使用std::atomic型别的对象来计算调用次数
public:
	double distanceFromOrigin() const noexcept
	{
		++callCount; // 带原子性的自增操作
		return std::sqrt((x*x) + (y*y));
	}
 
private:
	mutable std::atomic<unsigned> callCount{0};
	double x, y;
};
 
class Widget16 {
public:
	int magicValue() const
	{
		std::lock_guard<std::mutex> guard(m); // lock m
		if (cacheValid) return cachedValue;
		else {
			auto val1 = expensiveComputation1();
			auto val2 = expensiveComputation2();
			cachedValue = val1 + val2;
			cacheValid = true;
			return cachedValue;
		}
	} // unlock m
 
private:
	int expensiveComputation1() const { return 1; }
	int expensiveComputation2() const { return 2; }
 
	mutable std::mutex m;
	mutable int cachedValue; // no longer atomic
	mutable bool cacheValid{false};
};
 
int test_item_16()
{
	return 0;
}
```
对于单个要求同步的变量或内存区域,使用std::atomic就足够了。但是如果有两个或更多个变量或内存区域需要作为一整个单位进行操作时，就要动用互斥量了。

std::mutex是个只移型别(std::mutex is a move-only type)(i.e., a type that can be moved, but not copied)。与std::mutex一样，std::atomic也是只移型别。

**要点速记：(1).保证const成员函数的线程安全性，除非可以确信它们不会用在并发语境中。(2).运用std::atomic型别的变量会比运用互斥量提供更好的性能，但前者仅适用对单个变量或内存区域的操作。**

### Item 17. 理解特种成员函数的生成机制(Understand special member function generation)
```cpp
class Widget17 {
public:
	Widget17(Widget17&& rhs); // move constructor
	Widget17& operator=(Widget17&& rhs); // move assignment operator
 
	Widget17(const Widget17&) = default; // default copy constructor, behavior is OK
	Widget17& operator=(const Widget17&) = default; // default copy assign, behavior is OK
};
 
int test_item_17()
{
	return 0;
}
```


在C++官方用语中，特种成员函数是指那些C++会自行生成的成员函数。C++98有四种特种成员函数：**默认构造函数、析构函数、拷贝构造函数、以及拷贝赋值运算符。这些函数仅在需要时才会生成，**亦即，在某些代码使用了它们，而在类中并未显式声明的场合。仅当一个类没有声明任何构造函数时，才会生成默认构造函数(只要指定了一个要求传参的构造函数，就会阻止编译器生成默认构造函数)。生成的特种成员函数都具有public访问层级且是inline的，而且它们都是非虚的，除非讨论的是一个析构函数，位于一个派生类中，并且基类的析构函数是个虚函数。在那种情况下，编译器为派生类生成的析构函数也是个虚函数。

在C++11中，特种成员函数加入了两位新成员：移动构造函数和移动赋值运算符。移动操作也仅在需要时才生成，而一旦生成，它们执行的也是作用于非静态成员的”按成员移动”操作。意思是，移动构造函数将依照其形参rhs的各个非静态成员对于本类的对应成员执行移动构造，而移动赋值运算符则将依照其形参rhs的各个非静态成员对于本类的对应成员执行移动赋值。移动构造函数同时还会移动构造它的基类部分(如果有的话)，而移动赋值运算符则会移动赋值它的基类部分。不过，当我提到移动操作在某个数据成员或基类部分上执行移动构造或移动赋值的时候，并不能保证移动操作真的会发生。”按成员移动(Memberwise moves)”实际上更像是按成员的移动请求，因为那些不可移动的型别(即那些并未为移动操作提供特殊支持的型别，这包括了大多数C++98中的遗留型别)将通过其拷贝操作实现”移动”。

两种拷贝操作是彼此独立的(the two copy operations are independent)：声明了其中一个，并不会阻止编译器生成另一个。两种移动操作并不彼此独立(the two move operations are not independent)：声明了其中一个，就会阻止编译器生成另一个。此外，一旦显式声明了拷贝操作，这个类也就不再会生成移动操作了。反之亦然，一旦声明了移动操作(无论是移动构造还是移动赋值)，编译器就会废除拷贝操作(废除的方式是删除它们)。

移动操作的生成条件(如果需要生成)仅当以下三者同时成立：该类未声明任何拷贝操作。该类未声明任何移动操作。该类未声明任何析构函数。

C++11中，支配特种成员函数的机制如下：(1).默认构造函数：与C++98的机制相同。仅当类中不包含用户声明的构造函数时才生成。(2).析构函数：与C++98的机制基本相同，唯一的区别在于析构函数默认为noexcept。与C++98的机制相同，仅当基类的析构函数为虚的，派生类的析构函数才是虚的。(3).拷贝构造函数：运行期行为与C++98相同：按成员进行非静态数据成员的拷贝构造。仅当类中不包含用户声明的拷贝构造函数时才生成。如果该类声明了移动操作，则拷贝构造函数将被删除。在已经存在拷贝赋值运算符或析构函数的条件下，仍然生成拷贝构造函数已经成为了被废弃的行为。(4).拷贝赋值运算符：运行期行为与C++98相同：按成员进行非静态数据成员的拷贝赋值。仅当类中不包含用户声明的拷贝赋值运算符时才生成。如果该类声明了移动操作，则拷贝构造函数将被删除。在已经存在拷贝构造函数或析构函数的条件下，仍然生成拷贝赋值运算符已经成为了被废弃的行为。(5).移动构造函数和移动赋值运算符：都按成员进行非静态数据成员的移动操作。仅当类中不包含用户声明的拷贝操作、移动操作和析构函数时才生成。

**要点速记：(1).特种成员函数是指那些C++会自行生成的成员函数：默认构造函数、析构函数、拷贝操作、以及移动操作。(2).移动操作仅当类中未包含用户显式声明的拷贝操作、移动操作和析构函数时才生成。(3).拷贝构造函数仅当类中不包含用户显式声明的拷贝构造函数时才生成，如果该类声明了移动操作则拷贝构造函数将被删除。拷贝赋值运算符仅当类中不包含用户显式声明的拷贝赋值运算符才生成，如果该类声明了移动操作则拷贝赋值运算符将被删除。在已经存在显式声明的析构函数的条件下，生成拷贝操作已经成为了被废弃的行为。(4).成员函数模板在任何情况下都不会抑制特种成员函数的生成。**




## 第4章 智能指针



### Item 18. 使用std::unique_ptr管理具备专属所有权的资源(Use std::unique_ptr for exclusive-ownership resource management)
```cpp
class Investment { // 投资
public:
	virtual ~Investment() {}
};
class Stock : public Investment {}; // 股票
class Bond : public Investment {}; // 债券
class RealEstate : public Investment {}; // 不动产
 
void makeLogEntry(Investment*) {}
 
auto delInvmt1 = [](Investment* pInvestment) { // custom deleter(a lambda expression), 使用无状态lambda表达式作为自定义析构器
			makeLogEntry(pInvestment);
			delete pInvestment;
		};
 
void delInvmt2(Investment* pInvestment) // 使用函数作为自定义析构器
{
	makeLogEntry(pInvestment);
	delete pInvestment;
}
 
template<typename... Ts>
//std::unique_ptr<Investment> makeInvestment(Ts&&... params) // return std::unique_ptr to an object created from the given args
std::unique_ptr<Investment, decltype(delInvmt1)> makeInvestment(Ts&&... params) // 改进的返回类型,返回值尺寸与Investment*相同
//std::unique_ptr<Investment, void(*)(Investment*)> makeInvestment(Ts&&... params) // 返回值尺寸等于Investment*的尺寸+至少函数指针的尺寸
{
	//std::unique_ptr<Investment> pInv(nullptr);
	std::unique_ptr<Investment, decltype(delInvmt1)> pInv(nullptr, delInvmt1); // ptr to be returned
 
	if (nullptr/* a Stoc object should be created*/) {
		pInv.reset(new Stock(std::forward<Ts>(params)...));
	} else if (nullptr/*a Bond object should be created*/) {
		pInv.reset(new Bond(std::forward<Ts>(params)...));
	} else if (nullptr/*a RealEstate object should be created*/) {
		pInv.reset(new RealEstate(std::forward<Ts>(params)...));
	}
 
	return pInv;
}
 
int test_item_18()
{
	//auto pInvestment = makeInvestment(arguments); // pInvestment is of type std::unique_ptr<Investment>
	//std::shared_ptr<Investment> sp = makeInvestment(arguments); // converts std::unique_ptr to std::shared_ptr
 
	return 0;
} // destroy *pInvestment
```
C++11中共有四种智能指针：std::auto_ptr、std::unique_ptr、std::shared_ptr和std::weak_ptr。所有这些智能指针都是为管理动态分配对象的生命期而设计的，换言之，通过保证这样的对象在适当的时机以适当的方式析构(包括发生异常的场合)，来防止资源泄漏。

std::auto_ptr是个从C++98中残留下来的弃用特性，它是一种对智能指针进行标准化的尝试，这种尝试后来成为了C++11中的std::unique_ptr。

std::unique_ptr可以做std::auto_ptr能够做的任何事，并且不止于此。它执行的效率和std::auto_ptr一样高，而且不用扭曲其要表达的本意去复制任何对象。它从任何方面来看都要比std::auto_ptr更好。

每当你需要使用智能指针时，std::unique_ptr基本上应是手头首选。可以认为在默认情况下(使用默认析构器)std::unique_ptr和裸指针(raw pointer)有着相同的尺寸，并且对于大多数的操作(包括提领(including dereferenceing))，它们都是精确地执行了相同的指令。

std::unique_ptr实现的是专属所有权(exclusive ownership)语义。一个非空的std::unique_ptr总是拥有其所指涉到的资源。移动一个std::unique_ptr会将所有权从源指针移至目标指针(源指针被置空)。std::unique_ptr不允许复制(copy)，因为如果复制了一个std::unique_ptr，就会得到两个指涉到同一资源的std::unique_ptr，而这两者都认为自己拥有(因此应当析构)该资源。因而std::unique_ptr是个只移型别(move-only type)。在执行析构操作时，由非空的std::unique_ptr析构其资源。默认地，资源的析构是通过对std::unique_ptr内部的裸指针实施delete完成的。

std::unique_ptr的一个常见用法是在对象继承谱系中作为工厂函数的返回型别。

默认地，析构通过delete运算符实现，但是在析构过程中std::unique_ptr可以被设置为使用自定义析构器(custom delete)：析构资源所调用的任意函数(或函数对象，包括那些由lambda表达式产生的)。

std::unique_ptr以两种形式提供，一种是单个对象(std::unique_ptr<T>)，另一种是数组(std::unique_ptr<T[]>)。单个对象形式不提供索引运算符(operator[])，而数组形式则不提供提领运算符(lack dereferencing operator)(operator*和operator->)。

std::unique_ptr是C++11中表达专属所有权的方式，但它还有一个十分吸引人的特性，就是std::unique_ptr可以方便高效地转换成std::shared_ptr。

**要点速记：(1).std::unique_ptr是小巧、高速的、具备只移型别的智能指针，对托管资源实施专属所有权语义。(2).默认地，资源析构采用delete运算符来实现，但可以指定自定义删除器。有状态的删除器和采用函数指针实现的删除器会增加std::unique_ptr型别的对象尺寸。(3).将std::unique_ptr转换成std::shared_ptr是容易实现的**。

std::unique_ptr更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52203664

### Item 19. 使用std::shared_ptr管理具备共享所有权的资源(Use std::shared_ptr for shared-ownership resource management)
```cpp
class Widget19 {};
void makeLogEntry(Widget19*) {}
auto loggingDel = [](Widget19* pw) { makeLogEntry(pw); delete pw; }; // custom deleter,自定义析构器
 
int test_item_19()
{
	std::unique_ptr<Widget19, decltype(loggingDel)> upw(new Widget19, loggingDel); // 析构器型别是智能指针型别的一部分
	std::shared_ptr<Widget19> spw(new Widget19, loggingDel); // 析构器型别不是智能指针型别的一部分
 
	auto pw = new Widget19; // pw是个裸指针
	//std::shared_ptr<Widget19> spw1(pw, loggingDel); // 为*pw创建一个控制块
	//std::shared_ptr<Widget19> spw2(pw, loggingDel); // 为*pw创建了第二个控制块
	// 以上两行语句会导致*pw被析构两次，第二次析构将会引发未定义行为,不推荐上面的用法
 
	std::shared_ptr<Widget19> spw1(new Widget19, loggingDel); // 直接传递new表达式
	std::shared_ptr<Widget19> spw2(spw1); // spw2使用的是和spw1同一个控制块
 
	return 0;
}
```

std::shared_ptr这种智能指针访问的对象采用共享所有权来管理其生存期。没有哪个特定的std::shared_ptr拥有该对象。取而代之的是，所有指涉到它的std::shared_ptr共同协作，确保在不再需要该对象的时刻将其析构。当最后一个指涉到某对象的std::shared_ptr不再指涉到它时(例如，由于该std::shared_ptr被析构，或使其指涉到另一个不同的对象)，该std::shared_ptr会析构其指涉到的对象。

std::shared_ptr可以通过访问某资源的引用计数来确定是否自己是最后一个指涉到该资源的。引用计数是个与资源关联的值，用来记录跟踪指涉到该资源的std::shared_ptr数量。 std::shared_ptr的构造函数会使该计数递增(通常如此)，而其析构函数会使该计数递减，而拷贝赋值运算符同时执行两种操作(如果sp1和sp2是指涉到不同对象的std::shared_ptr，则赋值运算”sp1=sp2”将修改sp1，使其指涉到sp2所指涉到的对象。该赋值的净效应是：最初sp1所指涉到的对象的引用计数递减，同时sp2所指涉到的对象的引用计数递增)。如果某个std::shared_ptr发现，在实施过一次递减后引用计数变成了零，即不再有std::shared_ptr指涉到该资源，则std::shared_ptr会析构之。

引用计数的存在会带来一些性能影响：(1).std::shared_ptr的尺寸是裸指针的两倍。因为它们内部既包含一个指涉到该资源的裸指针，也包含一个指涉到该资源的引用计数的裸指针。(2).引用计数的内存必须动态分配。std::shared_ptr若是由std::make_ptr创建，可以避免动态分配的成本。然而仍有一些场景下，不可以使用std::make_ptr。但无论是不是使用std::make_ptr，引用计数都会作为动态分配的数据来存储。(3).引用计数的递增和递减必须是原子操作。因为在不同的线程中可能存在并发的读写器。

从一个已有std::shared_ptr移动构造一个新的std::shared_ptr会将源std::shared_ptr置空，这意味着一旦新的std::shared_ptr产生后，原有的std::shared_ptr将不再指涉到其资源，结果是不需要进行任何引用计数操作。因此，移动std::shared_ptr比拷贝它们要快：拷贝要求递增引用计数，而移动则不需要。这一点对于构造和赋值操作同样成立，所以，移动构造函数比拷贝构造函数快，移动赋值比拷贝赋值快。

与std::unique_ptr类似，std::shared_ptr也使用delete运算符作为其默认资源析构机制，但它同样支持自定义析构器。然而这种支持的设计却与std::unique_ptr有所不同。对于std::unique_ptr而言，析构器的型别是智能指针型别的一部分。但对于std::shared_ptr而言，却并非如此。与std::unique_ptr的另一点不同，是自定义析构器不会改变std::shared_ptr的尺寸。无论析构器是怎样的型别，std::shared_ptr对象的尺寸都相当于裸指针的两倍。

每一个由std::shared_ptr管理的对象都有一个控制块。除了包含引用计数之外，如果该自定义析构器被指定的话，该控制块还包含自定义析构器的一个拷贝。如果指定了一个自定义内存分配器，控制块也会包含一份它的拷贝。控制块还有可能包含其它附加数据，如被称为弱计数的次级引用计数。

一个对象的控制块由创建首个指涉到该对象的std::shared_ptr的函数来确定。控制块的创建遵循以下规则：(1).std::make_shared总是创建一个控制块。(2).从具备专属所有权的指针(即std::unique_ptr或std::auto_ptr指针)出发构造一个std::shared_ptr时，会创建一个控制块。专属所有权指针不使用控制块。(3).当std::shared_ptr构造函数使用裸指针作为实参来调用时，它会创建一个控制块。

尽可能避免将裸指针传递给一个std::shared_ptr的构造函数。常用的替代手法，是使用std::make_shared。如果必须将一个裸指针传递给std::shared_ptr的构造函数，就直接传递new运算符的结果，而非传递一个裸指针变量。

当你希望一个托管到std::shared_ptr的类能够安全地由this指针创建一个std::shared_ptr时，可以使用std::enable_shared_from_this。std::enable_shared_from_this是一个基类模板，其型别形参总是其派生类的类名。std::enable_shared_from_this定义了一个成员函数，它会创建一个std::shared_ptr指涉到当前对象，但同时不会重复创建控制块。这个成员函数的名字是shared_from_this，每当你需要一个和this指针指涉到相同对象的std::shared_ptr时，都可以在成员函数中使用它。

std::shared_ptr不能处理数组。std::shared_ptr的API仅被设计用来处理指涉到单个对象的指针，并没有所谓的std::shared_ptr<T[]>。

**要点速记：(1).std::shared_ptr提供方便的手段，实现了任意资源在共享所有权语义下进行生命周期管理的垃圾回收。(2).与std::unique_ptr相比，std::shared_ptr的尺寸通常是裸指针尺寸的两倍，它还会带来控制块的开销，并要求原子化的引用计数操作。(3).默认的资源析构通过delete运算符进行，但同时也支持定制删除器。删除器的型别对std::shared_ptr的型别没有影响。(4).避免使用裸指针型别的变量来创建std::shared_ptr指针。**

std::shared_ptr更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52202007

### Item 20. 对于类似std::shared_ptr但有可能空悬的指针使用std::weak_ptr(Use std::weak_ptr for std::shared_ptr-like pointers than can dangle)
```cpp

class Widget20 {};

int test_item_20()
{
	auto spw = std::make_shared<Widget20>(); // spw构造完成后，指涉到Widget20的引用计数置为1
	std::weak_ptr<Widget20> wpw(spw); // wpw和spw指涉到同一个Widget20，引用计数保持为1
	spw = nullptr; // 引用计数变为0,Widget20对象被析构，wpw空悬(dangle)
	if (wpw.expired()) // 若wpw不再指涉到任何对象
	      fprintf(stdout, "wpw doesn't point to an object\n");

	std::shared_ptr<Widget20> spw1 = wpw.lock(); // 若wpw失效，则spw1为空
	auto spw2 = wpw.lock(); // 使用auto,同上，若wpw失效，则spw2为空
	if (spw2 == nullptr) fprintf(stdout, "wpw expired\n");
	 
	std::shared_ptr<Widget20> spw3(wpw); // 若wpw失效,抛出std::bad_weak_ptr型别的异常
	 
	return 0;
}
```

std::weak_ptr不能提领(dereferenced)，也不能检查是否为空。这是因为std::weak_ptr并不是一种独立的智能指针，而是std::shared_ptr的一种扩充。std::weak_ptr一般是通过std::shared_ptr来创建的。当使用std::shared_ptr完成初始化std::weak_ptr的时刻，两者就指涉到了相同位置，但std::weak_ptr并不影响所指涉到的对象的引用计数。

std::weak_ptr的空悬(dangle)，也被称作失效(expired)。

需要一个原子操作来完成std::weak_ptr是否失效的校验，以及在未失效的条件下提供对所指涉到的对象的访问。这个操作可以通过由std::weak_ptr创建std::shared_ptr来实现。该操作有两种形式。一种形式是std::weak_ptr::lock，它返回一个std::shared_ptr。如果std::weak_ptr已经失效，则std::shared_ptr为空。另一种形式是用std::weak_ptr作为实参来构造std::shared_ptr，这样，如果std::weak_ptr失效的话，抛出异常。

**要点速记：(1).使用std::weak_ptr来代替可能空悬的std::shared_ptr。(2).std::weak_ptr可能的用武之地包括缓存、观察者列表、以及避免std::shared_ptr指针环路。**

std::weak_ptr更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52203825

### Item 21. 优先选用std::make_unique和std::make_shared，而非直接使用new(Prefer std::make_unique and std::make_shared to direct use of new)
```cpp
template<typename T, typename... Ts>
std::unique_ptr<T> make_unique(Ts&&... params) // std::make_unique的一个基础版本，不支持数组和自定义析构器
{
	return std::unique_ptr<T>(new T(std::forward<Ts>(params)...));
}
 
class Widget21 {};
 
int test_item_21()
{
	// 使用了new的版本将被创建对象的型别重复写了两遍，但是make系列函数则没有
	auto upw1(std::make_unique<Widget21>()); // 使用make系列函数
	std::unique_ptr<Widget21> upw2(new Widget21); // 不使用make系列函数
	auto spw1(std::make_shared<Widget21>()); // 使用make系列函数
	std::shared_ptr<Widget21> spw2(new Widget21); // 不使用make系列函数
 
	return 0;
}
```
std::make_shared是C++11的一部分，而std::make_unique是在C++14中才加入标准库的。

std::make_unique和std::make_shared是三个make系列函数中的两个。make系列函数会把一个任意实参集合完美转发(perfect-forward)给动态分配内存的对象的构造函数，并返回一个指涉到该对象的智能指针。make系列函数的第三个是std::allocate_shared。它的行为和std::make_shared一样，只不过它的第一个实参是个用以动态分配内存的分配器对象。

一般使用std::make_shared比直接使用new好处：(1).可编写异常安全的代码；(2).性能的提升。

有一些情景，不能或者不应使用make系列函数。例如，所有的make系列函数都不允许使用自定义析构器，但是std::unique_ptr和std::shared_ptr却都有着允许使用自定义析构器的构造函数。

**要点速记：(1).相比于直接使用new表达式，make系列函数消除了重复代码、改进了异常安全性，并且对于std::make_shared和std::allocate_shared而言，生成的目标代码会尺寸更小、速度更快。(2).不适用使用make系列函数的场景包括需要定制删除器，以及期望直接传递大括号初始化物。(3).对于std::shared_ptr，不建议使用make系列函数的额外场景包括：自定义内存管理的类；内存紧张的系统、非常大的对象、以及存在比指涉到相同对象的std::shared_ptr生存期更久的std::weak_ptr。**

### Item 22. 使用Pimpl习惯用法时，将特殊成员函数的定义放到实现文件中(When using the Pimpl Idiom, define special member functions in the implementation file)
```cpp
// 以下代码假设位于gadget.h文件中
typedef struct Gadget { // Gadget is some userdefined type
	int x, y;
	std::string str;
};
 
// 以下代码假设位于widget.h文件中
class Widget22 {
public:
	Widget22();
	// 注意：这里仅声明，不能定义，定义必须放在widget.cpp文件中，因为Impl是个不完整类型
	~Widget22(); // declaration only
 
	// 添加支持移动操作,注意：这里仅声明，不能定义，定义必须放在widget.cpp文件中,因为Impl是个不完整类型
	Widget22(Widget22&& rhs); // declaration only
	Widget22& operator=(Widget22&& rhs); // declaration only
 
	// 添加拷贝操作
	Widget22(const Widget22& rhs); // declaration only
	Widget22& operator=(const Widget22& rhs); // declaration only
 
private:
	// 原数据成员
	//std::string name;
	//std::vector<double> data;
	//Gadget g1, g2, g3;
 
	struct Impl; // declare implementation struct
	//Impl* pImpl; // and pointer to it
	std::unique_ptr<Impl> pImpl; // 使用智能指针代替裸指针(raw pointer)
	// 如果使用std::shared_ptr而非std::unique_ptr，则无需再有析构函数或移动操作的声明
};
 
// 以下代码假设位于widget.cpp中
//#include "widget.h"
//#include "gadget.h"
//#include <string>
//#include <vector>
 
struct Widget22::Impl { // Widget22::Impl的实现，包括此前在Widget22中的数据成员
	std::string name;
	std::vector<double> data;
	Gadget g1, g2, g3;
};
 
//Widget22::Widget22() : pImpl(new Impl) {} // allocate data members for this Widget22 object
Widget22::Widget22() : pImpl(std::make_unique<Impl>()) {}
//Widget22::~Widget22() { delete pImpl; } // destroy data members for this Widget22 object
Widget22::~Widget22() = default; // ~Widget22 definition
Widget22::Widget22(Widget22&& rhs) = default;
Widget22& Widget22::operator=(Widget22&& rhs) = default;
Widget22::Widget22(const Widget22& rhs) : pImpl(std::make_unique<Impl>(*rhs.pImpl)) {}
Widget22& Widget22::operator=(const Widget22& rhs) { *pImpl = *rhs.pImpl; return *this; }
 
int test_item_22()
{
	Widget22 w1;
	auto w2(std::move(w1));
	Widget22 w3(w2);
	w1 = std::move(w2);
 
	return 0;
}
```
“Pimpl”意为”pointer to implementation”，即指涉到实现的指针。这种技巧就是把某类的数据成员用一个指涉到某实现类(或结构体)的指针替代，尔后把原来在主类中的数据成员放置到实现类中，并通过指针间接访问这些数据成员。

Pimpl习惯用法的第一部分，是声明一个指针型别的数据成员，指涉到一个非完整型别(an incomplete type)。第二部分是动态分配和回收(dynamic allocation and deallocation)持有从前在原始类里的那些数据成员的对象，而分配和回收代码则放在实现文件中。

**Pimpl习惯用法是一种可以在类实现和类使用者之间减少编译依赖性的方法**，但从概念上说，Pimpl习惯用法并不能改变类所代表的事物。

std::unique_ptr和std::shared_ptr这两种智能指针在实现pImpl指针行为时的不同，源自它们对于自定义析构器的支持的不同。对于std::unique_ptr而言，析构器型别是智能指针型别的一部分，这使得编译器会产生更小尺寸的运行期数据结构以及更快速的运行期代码。如此高效带来的后果是，预使用编译器产生的特种函数(例如，析构函数或移动操作)，就要求其指涉到的型别必须是完整型别。而对于std::shared_ptr而言，析构器的型别并非智能指针型别的一部分，这就需要更大尺寸的运行时期数据结构以及更慢一些的目标代码，但在使用编译器生成的特种函数时，其指涉到的型别却并不要求是完整型别。

**要点速记：(1).Pimpl习惯用法通过降低类的客户和类实现者之间的依赖性，减少了构建遍数。(2).对于采用std::unique_ptr来实现的pImpl指针，须在类的头文件中声明特种成员函数，但在实现文件中实现它们。即使默认函数实现有着正确行为，也必须这样做。(3).上述建议仅适用于std::unique_ptr，但并不适用于std::shared_ptr。**




## 第5章 右值引用、移动语义和完美转发 

### Item 23. 理解std::move和std::forward(Understand std::move and std::forward)
```cpp
// 比较接近C++11中std::move的示例实现,它不完全符合标准的所有细节
template<typename T> // in namespace std
typename std::remove_reference<T>::type&& move(T&& param)
{
	using ReturnType = typename std::remove_reference<T>::type&&; // 别名声明
	return static_cast<ReturnType>(param);
}
 
// C++14中比较接近的std::move示例实现
template<typename T> // C++14, still in namespace std
decltype(auto) move(T&& param)
{
	using ReturnType = std::remove_reference_t<T>&&;
	return static_cast<ReturnType>(param);
}
 
class Widget23 {};
 
void process(const Widget23& lvalArg)  { fprintf(stderr, "process lvalues\n"); } // process lvalues
void process(Widget23&& rvalArg) { fprintf(stderr, "process rvalues\n"); } // process rvalues
 
template<typename T>
void logAndProcess(T&& param) // template that passes param to process
{
	process(std::forward<T>(param));
}
 
int test_item_23()
{
	Widget23 w;
	logAndProcess(w); // call with lvalue
	logAndProcess(std::move(w)); // call with rvalue
 
	return 0;
}
```
移动语义(move semantics)：使得编译器得以使用不那么昂贵的移动操作来替换昂贵的拷贝操作(makes it possible for compilers to replace expensive copying operations with less expensive moves)。同拷贝构造函数、拷贝赋值运算符给予人们控制对象拷贝的具体意义的能力一样，移动构造函数和移动赋值运算符也给予人们控制对象移动语义的能力。移动语义也使得创建只移型别对象成为可能，这些型别包括std::unique_ptr、std::future和std::thread等。

完美转发(perfect forwarding)：使得人们可以撰写接受任意实参的函数模板，并将其转发到其它函数，目标函数会接受到与转发函数所接受的完全相同的实参(makes it possible to write function templates that take arbitrary arguments and forward them to other functions such that the target functions receive exactly the same arguments as were passed to the forwarding functions)。

右值引用是将这两个(移动语义和完美转发)风马牛不相及的语言特性胶合起来的底层语言机制，正是它使得移动语义和完美转发成为了可能。

std::move并不进行任何移动，std::forward也不进行任何转发。这两者在运行期都无所作为。它们不会生成任何可执行代码，连一个字节都不会生成。(std::move doesn’t move anything. std::forward doesn’t forward anything. At runtime, neither does anything at all. They generate no executable code. Not a single byte.)

**std::move和std::forward都是仅仅执行强制型别转换的函数(其实是函数模板)**。std::move无条件地将实参强制转换成右值，而std::forward则仅在某个特定条件满足时才执行同一个强制转换。

std::move的形参是指涉到一个对象的引用(准确地说，是万能引用(universal reference))，它返回的是指涉到同一个对象的引用。函数返回值的”&&”部分，暗示着std::move返回的是个右值引用。如果T碰巧是个左值引用的话，那么T&&就成了左值引用。为了避免这种情况发生，它将型别特征std::remove_reference应用于T，从而保证”&&”应用在一个非引用型别之上。这么一来，就可以确保std::move返回的是右值引用，而这一点十分重要，因为从该函数返回的右值引用肯定是右值。综上所述，std::move将实参强制转换成了右值，而这就是该函数全部的所做作为。

右值是可以实施移动的，所以在一个对象上实施了std::move，就是告诉编译器该对象具备可移动的条件。右值也仅在通常情况下能够移动。

如果想取得对某个对象执行移动操作的能力，则不要将其声明为常量，因为针对常量对象执行的移动操作将一声不响地变换成拷贝操作。std::move不仅不实际移动任何东西，甚至不保证经过其强制型别转换后的对象具备可移动的能力。关于针对任意对象实施过std::move的结果，唯一可以确定的是，该结果会是个右值。

std::forward与std::move类似，只是与std::move会无条件地将其实参强制转换为右值型别不同，std::forward仅在特定条件下才实施这样的强制型别转换。换言之，std::forward是一个有条件强制型别转换(std::forward is a conditional cast)：仅当其实参是使用右值完成初始化时，它才会执行向右值型别的强制型别转换。

使用std::move所要传达的意思是无条件地向右值型别的强制型别转换，而使用std::forward则想说明仅仅对绑定到右值的引用实施向右值型别的强制型别转换。这是两个非常不同的行为。前者是典型地为移动操作做铺垫，而后者仅仅是传递(转发)一个对象到另一个函数(just passes----forwards----an object to another function)，而在此过程中无论该对象原始型别具备左值性或右值性(lvalueness or rvalueness)，都保持原样。这两个行为是如此不同，因而最好使用两个不同函数(以及函数名字)来区分这两者。

**要点速记：(1).std::move实施的是无条件的向右值型别的强制型别转换。就其本身而言，它不会执行移动操作。(2).仅当传入的实参被绑定到右值时，std::forward才针对该实参实施向右值型别的强制型别转换。(3).在运行期，std::move和std::forward都不会做任何操作。**

std::move更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52558914

std::forward更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52589454

### Item 24. 区分万能引用和右值引用(Distinguish universal references from rvalue references)
```cpp
class Widget24 {};
 
void f24(Widget24&& param) { fprintf(stdout, "Widget24&&\n"); } // no type deduction(不涉及型别推导), param is an rvalue reference
template<typename T>
void f24_1(std::vector<T>&& param) { fprintf(stdout, "std::vector<T>&&\n"); } // param is an rvalue reference
template<typename T>
void f24(T&& param) { fprintf(stdout, "T&&\n"); } // not rvalue reference, param is a universal reference
template<typename T>
void f24(const T&& param) {} // param is an rvalue reference
 
int test_item_24()
{
	Widget24&& var1 = Widget24(); // no type deduction, var1 is an rvalue reference
	auto&& var2 = var1; // not rvalue reference, var2 is a universal reference
 
	Widget24 w;
	f24(w); // lvalue passed to f, param's type is Widget24&(an lvalue reference)
	f24(std::move(w)); // rvalue passed to f, param's type is Widget24&&(an rvalue reference)
 
	std::vector<int> v;
	//f24_1(v); // error, can't bind lvalue to rvalue reference
	f24_1(std::move(v));
	//f24(v); // will call: void f24(T&& param)
 
	return 0;
}
```

实际上，”T&&”有两种不同的含义。其中一种含义，理所当然，是右值引用。正如期望，它们仅仅会绑定到右值，而其主要的存在理由，在于识别出可移对象。”T&&”的另一种含义，则表示其既可以是右值引用，亦可以是左值引用，二者居一。带有这种含义的引用在代码中形如右值引用(即T&&)，但它们可以像左值引用一样运作(即T&)。这种双重特性使之既可以绑定到右值(如右值引用)，也可以绑定到左值(即左值引用)。犹有进者(furthermore)，它们也可以绑定到const对象或非const对象，以及volatile对象或非volatile对象，甚至绑定到那些既带有const又带有volatile饰词的对象。它们几乎可以绑定到万事万物。这种拥有史无前例的灵活性的引用值得拥有一个独特的名字。我称之为万能引用(universal reference、forwarding reference)。

万能引用会在两种场景下现身。最常见的一种场景是函数模板的形参。第二个场景是auto声明。这两个场景的共同之处，在于它们都涉及型别推导(type deduction)。如果你看到了”T&&”，却没有涉及型别推导，那么，你看到的就是个右值引用。

因为万能引用首先是个引用，所以初始化是必须的。万能引用的初始化物会决定它代表的是个左值引用还是右值引用：如果初始化物是右值，万能引用就会对应到一个右值引用；如果初始化物是左值，万能引用就会对应到一个左值引用。

若要使一个引用成为万能引用，其涉及型别推导是必要条件，但还不是充分条件。引用声明的形式也必须正确无误，并且该形式被限定得很死：**必须得正好形如”T&&”才行**，但没必要一定要取”T”这个名字。声明为auto&&型别的变量都是万能引用，因为它们肯定涉及型别推导并且肯定有正确的形式(“T&&”)。

**要点速记：(1).如果函数模板形参具备T&&型别，并且T的型别系推导而来，或如果对象使用auto&&声明其型别，则该形参或对象就是个万能引用。(2).如果型别声明并不精确地具备type&&的形式，或者型别推导并未发生，则type&&就代表右值引用。(3).若采用右值来初始化万能引用，就会得到一个右值引用。若采用左值来初始化万能引用，就会得到一个左值引用。**

右值引用更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/78619152

### Item 25. 针对右值引用实施std::move，针对万能引用实施std::forward(Use std::move on rvalue references, std::forward on universal references)
```cpp
class Widget25 {
public:
	Widget25(Widget25&& rhs) : name(std::move(rhs.name)), p(std::move(rhs.p)) {} // rhs is rvalue reference
	template<typename T>
	void setName(T&& newName) { name = std::forward<T>(newName); } // newName is universal reference
 
private:
	std::string name;
	typedef struct SomeDataStructure {} SomeDataStructure;
	std::shared_ptr<SomeDataStructure> p;
};
 
class Matrix {
public:
	Matrix& operator+=(const Matrix& rhs) { return *this; }
	void reduce() {}
};
 
Matrix operator+(Matrix&& lhs, const Matrix& rhs) // 按值返回
{
	lhs += rhs;
	return std::move(lhs); // 将lhs移入返回值
}
 
template<typename T>
Matrix reduceAndCopy(T&& mat) // 按值返回，万能引用形参
{
	mat.reduce();
	return std::forward<T>(mat); // 对于右值是移入返回值;对于左值是拷贝入返回值
}
 
int test_item_25()
{
	return 0;
}
```
右值引用仅会绑定到那些可供移动的对象上(Rvalue references bind only to objects that are candidates for moving)。

当转发右值引用给其它函数时，应当对其实施向右值的无条件强制型别转换(通过std::move)，因为它们一定绑定到右值；而当转发万能引用时，应当对其实施向右值的有条件强制型别转换(通过std::forward)，因为它们不一定绑定到右值。应当避免针对右值引用实施std::forward，针对万能引用使用std::move的想法更为糟糕。

**要点速记：(1).针对右值引用的最后一次使用实施std::move，针对万能引用的最后一次使用实施std::forward。(2).作为按值返回的函数的右值引用和万能引用，依上一条所述采取相同行为。(3).若局部对象可能适用于返回值优化(RVO, return value optimization)，则请勿针对其实施std::move或std::forward。**

### Item 26. 避免依万能引用型别进行重载(Avoid overloading on universal references)
```cpp
std::multiset<std::string> names; // global data structure
 
//void logAndAdd(const std::string& name) // 第一种实现方法
template<typename T>
void logAndAdd(T&& name) // universal reference，第二种实现方法
{
	auto now = std::chrono::system_clock::now();
	fprintf(stdout, "time point\n");
	//names.emplace(name);
	names.emplace(std::forward<T>(name));
}
 
std::string nameFromIdx(int idx) // 返回索引对应的名字
{
	return std::string("xxx");
}
 
void logAndAdd(int idx) // 新的重载函数
{
	auto now = std::chrono::system_clock::now();
	fprintf(stdout, "time point2\n");
	names.emplace(nameFromIdx(idx));
}


 
int test_item_26()
{
	std::string petName("Darla");
 
	logAndAdd(petName); // as before, copy lvalue into multiset
	logAndAdd(std::string("Persephone")); // move rvalue instead of copying it
	logAndAdd("Patty Dog"); // create std::string in multiset instead of copying a temporary std::string
 
	logAndAdd(22); // 调用形参型别为int的重载版本
 
	short nameIdx = 100;
	//logAndAdd(nameIdx); // error c2664, 形参型别为T&&的版本可以将T推导为short, 对于short型别的实参来说，万能引用产生了比int更好的匹配
 
	return 0;
}
```

形参为万能引用的函数，是C++中最贪婪的，它们会在实例化(instantiate)过程中，和几乎任何实参型别都会产生精确匹配。这就是为何把重载和万能引用这两者结合起来总是馊主意：一旦万能引用成为重载候选，它就会吸引走大批的实参型别，远比撰写重载代码的程序员期望的要多。

**要点速记：(1).把万能引用作为重载候选型别，几乎总会让该重载版本在始料未及的情况下被调用到。(2).完美转发构造函数的问题尤其严重，因为对于非常量的左值型别而言，它们一般都会形成相对于拷贝构造函数的更佳匹配，并且它们还会劫持派生类中对基类的拷贝和移动构造函数的调用。**

### Item 27. 熟悉依万能引用型别进行重载的替代方案(Familiarize yourself with alternatives to overloading on universal references)
```cpp
std::multiset<std::string> names27; // global data structure
std::string nameFromIdx27(int idx) { return std::string("xxx"); }
 
template<typename T>
void logAndAddImpl(T&& name, std::false_type) // 非整型实参
{
	auto now = std::chrono::system_clock::now();
	fprintf(stdout, "time point: no int\n");
	names27.emplace(std::forward<T>(name));
}
 
void logAndAddImpl(int idx, std::true_type) // 整型实参
{
	auto now = std::chrono::system_clock::now();
	fprintf(stdout, "time point: int\n");
	names.emplace(nameFromIdx(idx));
}
 
template<typename T>
void logAndAdd27(T&& name) // name to data structure
{
	logAndAddImpl(std::forward<T>(name), std::is_integral<typename std::remove_reference<T>::type>());
}
 
class Person {
public:
	//template<typename T, typename = typename std::enable_if<!std::is_same<Person, typename std::decay<T>::type>::value>::type>
	//template<typename T, typename = typename std::enable_if<!std::is_base_of<Person, typename std::decay<T>::type>::value>::type> // 可使继承自Person的类，当调用基类的构造函数时走的是基类的拷贝或移动构造函数
	//template<typename T, typename = std::enable_if_t<!std::is_base_of<Person, std::decay_t<T>>::value>> // C++14
	template<typename T, typename = std::enable_if_t<!std::is_base_of<Person, std::decay_t<T>>::value &&
														!std::is_integral<std::remove_reference_t<T>>::value>> // C++14
	explicit Person(T&& n) // 只有指定的条件满足了才会启用此模板, constructor for string and args convertible to string
		: name(std::forward<T>(n)) 
	{
		// assert that a std::string can be created from a T object
		static_assert(std::is_constructible<std::string, T>::value, "Parameter n can't be used to construct a std::string");
	}
 
	explicit Person(int idx) // constructor for integral args
		: name(nameFromIdx27(idx)) {}
 
private:
	std::string name;
};
 
int test_item_27()
{
	// 注意：test_item_26()与test_item_27()实现的差异
	std::string petName("Darla");
 
	logAndAdd27(petName);
	logAndAdd27(std::string("Persephone"));
	logAndAdd27("Patty Dog");
 
	logAndAdd27(22);
 
	short nameIdx = 100;
	logAndAdd27(nameIdx);
 
	return 0;
}
```
标签分派(tag dispatch)：型别std::false_type和std::true_type就是所谓”标签(tags)”，运用它们的唯一目的在于强制重载决议(force overload resolution)按我们想要的方向推进。值得注意的是，这些形参甚至没有名字，它们在运行期不起任何作用。

std::enable_if可以强制编译器表现出来的行为如同特定的模板不存在一般。这样的模板称为禁用的。默认地，所有的模板都是启用的。可是，实施了std::enable_if的模板只会在满足了std::enable_if指定的条件的前提下才会启用。

完美转发效率更高，因为它出于和形参声明时的型别严格保持一致的目的，会避免创建临时对象。但是完美转发亦有不足，首先是针对某些型别无法实施完美转发，尽管它们可以被传递到接受特定型别的函数。其次是在客户传递了非法形参时，错误信息的可理解性。

**要点速记：(1).使用万能引用和重载组合的替代方案包括使用彼此不同的函数名字、传递const T&型别的形参、传值和标签分派(Alternatives to the combination of universal references and overloading include the use of distinct function names, passing parameters by lvalue-reference-to-const, passing parameters by value, and using tag dispatch)。(2). 通过std::enable_if约束模板允许一起使用万能引用和重载，但是它控制了编译器可以使用万能引用重载的条件。(3).万能引用形参通常在性能方面具备优势，但在易用性方面一般会有劣势。**

### Item 28. 理解引用折叠(Understand reference collapsing)
```cpp
class Widget28 {};
 
template<typename T>
void func(T&& param) {}
 
Widget28 widget28Factory() // function returning rvalue
{
	return Widget28();
}
 
int test_item_28()
{
	Widget28 w; // a variable(an lvalue)
	func(w); // call func with lvalue, T deduced to be Widget28&
 
	func(widget28Factory()); // call func with rvalue, T deduced to be Widget28
 
	auto&& w1 = w; // w1是左值引用
	auto&& w2 = widget28Factory(); // w2是右值引用
 
	return 0;
}
```

你是被禁止声明引用的引用，但编译器却可以在特殊的语境中产生引用的引用，模板实例化就是这样的语境之一。当编译器生成引用的引用时，引用折叠(reference collapsing)机制便支配了接下来发生的事情。有两种引用(左值和右值)，所以就有四种可能的引用--引用的组合(左值--左值，左值--右值，右值--左值，右值--右值)。如果引用的引用出现在允许的语境(例如，在模板实例化过程中)，该双重引用会折叠成单个引用，规则如下：如果任一引用为左值引用，则结果为左值引用。否则(即如果两个引用皆为右值引用)，结果为右值引用。引用折叠是使std::forward得以运作的关键。

引用折叠会出现的语境有四种：第一种，最常见的一种，就是模板实例化。第二种，是auto变量的型别生成。技术细节本质上和模板实例化一模一样，因为auto变量的型别推导和模板的型别推导在本质上就是一模一样的。第三种，是生成和使用typedef和别名声明。第四种，在于decltype的运用中。

万能引用并非一种新的引用型别，其实它就是满足了下面两个条件的语境中的右值引用：(1).型别推导的过程会区别左值和右值。T型别的左值推导结果为T&，而T型别的右值则推导结果为T。(2).会发生引用折叠。

**要点速记：(1).引用折叠会在四种语境中发生：模板实例化、auto型别生成、创建和运用typedef和别名声明，以及decltype。(2).当编译器在引用折叠的语境下生成引用的引用时，结果会变成单个引用。如果原始的引用中有任一引用为左值引用，则结果为左值引用。否则，结果为右值引用。(3).万能引用就是在型别推导的过程会区别左值和右值，以及会发生引用折叠的语境中的右值引用(Universal references are rvalue references in contexts where type deduction distinguishes lvalues from rvalues and where reference collapsing occurs)。**

### Item 29. 假定移动操作不存在、成本高、未使用(Assume that move operations are not present, not cheap, and not used)
```cpp
class Widget29 {};
 
int test_item_29()
{
 
	std::vector<Widget29> vw1;
	// ... // put data into vw1
	// move vw1 into vw2. runs in constant time. only ptrs in vw1 and vw2 are modified
	auto vw2 = std::move(vw1);
 
	std::array<Widget29, 10000> aw1;
	// ... // put data into aw1
	// move aw1 into aw2. runs in linear time. all elements in aw1 are moved into aw2
	auto aw2 = std::move(aw1);
 
	return 0;
}
```

在下面的几个场景中，C++11的移动语义不会给你带来什么好处：

(1).没有移动操作：待移动的对象未能提供移动操作。因此，移动请求就变成了拷贝请求。

(2).移动未能更快：待移动的对象虽然有移动操作，但并不比其拷贝操作更快。

(3).移动不可用：移动本可以发生的语境下，要求移动操作不可发射异常(emit no exception)，但该操作未加上noexcept声明。

(4).源对象是个左值：除了极少数例外，只有右值可以作为移动操作的源。

**要点速记：(1).假定移动操作不存在、成本高、未使用。(2).对于那些型别或对于移动语义的支持情况已知的代码，则无需作以上假定。**

### Item 30. 熟悉完美转发的失败情形(Familiarize yourself with prefect forwarding failure cases)
```cpp
void f30(const std::vector<int>& v) {}
void f30_2(std::size_t val) {}
 
template<typename T>
void fwd(T&& param) // accept any argument
{
	f30(std::forward<T>(param)); // forward it to f30
}
 
template<typename T>
void fwd30_2(T&& param) // accept any argument
{
	f30_2(std::forward<T>(param)); // forward it to f30_2
}
 
class Widget30 {
public:
	static const std::size_t MinVals = 28; // MinVals' declaration
};
//const std::size_t Widget30::MinVals; // no define for MinVals
 
struct IPv4Header {
	std::uint32_t version : 4,
		IHL : 4,
		DSCP : 6,
		ECN : 2,
		totalLength : 16;
};
 
int test_item_30()
{
	f30({ 1, 2, 3 }); // fine, "{1, 2, 3}"implicitly converted to std::vector<int>
	//fwd({ 1, 2, 3 }); // error, 大括号初始化物的运用，就是一种完美转发失败的情形
	auto il = { 1, 2, 3 }; // il's type deduced to be std::initializer_list<int>
	fwd(il); // fine, prefect-forwards il to f
 
	std::vector<int> widget30Data;
	widget30Data.reserve(Widget30::MinVals); // use of MinVals
 
	f30_2(Widget30::MinVals); // fine, treated as "f30_2(28)"
	fwd30_2(Widget30::MinVals); // error, shouldn't link, note: windows and linux can link
 
	IPv4Header h;
	memset(&h, 0, sizeof(IPv4Header));
	f30_2(h.totalLength); // fine
 
	//fwd30_2(h.totalLength); // error
	auto length = static_cast<std::uint16_t>(h.totalLength);
	fwd30_2(length); // forward the copy
 
	return 0;
}
```

“转发(forwarding)”的含义不过是一个函数把自己的形参传递(转发)给另一个函数而已。其目的是为了让第二个函数(转发目的函数)接受第一个函数(转发发起函数)所接受的同一个对象。这就排除了按值传递形参，因为它们只是原始调用者所传递之物的副本。我们想要转发目的函数能够处理原始传入对象。指针形参也只能出局，因为我们不想强迫调用者传递指针。论及一般意义上的转发时，都是在处理形参为引用型别的情形。

完美转发的含义是我们不仅转发对象，还转发其显著特征：型别、是左值还是右值，以及是否带有const或volatile饰词等。

**不能实施完美转发的实参**：(1).大括号初始化物；(2).0和NULL用作空指针：若尝试把0和NULL以空指针之名传递给模板，型别推导就会发生行为扭曲，推导结果会是整型(一般情况下会是int)而非所传递实参的指针型别。结论就是：0和NULL都不能用作空指针以进行完美转发。不过，修正方案也颇简单：传递nullptr，而非0或NULL。(3).仅有声明的整型static const成员变量。(4).重载的函数名字和模板名字。(5).位域：非const引用不得绑定到位域。

**要点速记：(1).完美转发的失败情形，是源于模板型别推导失败，或推导结果是错误的型别。(2).会导致完美转发失败的实参种类有大括号初始化物、以值0或NULL表达的空指针、仅有声明的整型static const成员变量、模板或重载的函数名字，以及位域。**




### 第6章 lambda表达式 

### Item 31. 避免默认捕获模式(Avoid default capture modes)
```cpp
using FilterContainer = std::vector<std::function<bool(int)>>;
FilterContainer filters;
 
class Widget31 {
public:
	void addFilter() const // add an entry to filters
	{
		//filters.emplace_back([=](int value) { return value % divisor == 0; } );
		// 捕获只能针对于在创建lambda式的作用域内可见的非静态局部变量(包括形参)
		//filters.emplace_back([](int value) { return value % divisor == 0; }); // error, divisor not available
		//filters.emplace_back([divisor](int value) { return value % divisor == 0; }); // error, no local divisor to capture
 
		auto currentObjectPtr = this;
		filters.emplace_back([currentObjectPtr](int value) { return value % currentObjectPtr->divisor == 0; });
 
		auto divisorCopy = divisor; // copy data member
		filters.emplace_back([divisorCopy](int value) { return value % divisorCopy == 0; }); // capture the copy use the copy
 
		static int xxx = 2;
		//filters.emplace_back([xxx](int value) { return value % xxx == 0; }); // error
		//filters.emplace_back([=](int value) { return value % xxx == 0; });
		++xxx;
	}
 
private:
	int divisor; // used in Widget31's filter
};
 
int test_item_31()
{
 
	filters.emplace_back(
		[](int value) { return value % 5 == 0; } // 不捕获任何外部变量
	);
	filters.emplace_back(
		[&](int value) { return value % 5 == 0; } // 以引用形式捕获所有外部变量
	);
	filters.emplace_back(
		[=](int value) { return value % 5 == 0; } // 以值的形式捕获所有外部变量
	);
 
	return 0;
}
```
C++11中有两种默认捕获模式：按引用或按值。

按引用捕获会导致闭包(closure)包含指涉到局部变量的引用，或者指涉到定义lambda式的作用域内的形参的引用。一旦由lambda式所创建的闭包越过了该局部变量或形参的生命期，那么闭包的引用就会空悬(dangle)。显示地列出lambda式所依赖的局部变量或形参是更好的软件工程实践。

**捕获只能针对于在创建lambda式的作用域内可见的非静态局部变量(包括形参)。**

**要点速记：(1).按引用的默认捕获会导致空悬指针问题(dangling references)。(2).按值的默认捕获极易受空悬指针影响(尤其是this)，并会误导人们认为lambda式是自洽的(lambdas are self-contained)。**

lambda表达式更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52653313

### Item 32. 使用初始化捕获将对象移入闭包(Use init capture to move objects into closures)
```cpp
class Widget32 {
public:
	bool isValidated() const { return true; }
	bool isProcessed() const { return true; }
	bool isArchived() const { return true; }
 
private:
};
 
class IsValAndArch { // is validated and archived
public:
	using DataType = std::unique_ptr<Widget32>;
 
	explicit IsValAndArch(DataType&& ptr) : pw(std::move(ptr)) {}
 
	bool operator()() const
	{
		return pw->isValidated() && pw->isArchived();
	}
 
private:
	DataType pw;
};
 
std::vector<double> data32; // 欲移入闭包的对象
 
int test_item_32()
{
	auto pw = std::make_unique<Widget32>();
	auto func = [pw = std::move(pw)]{ return pw->isValidated() && pw->isArchived(); }; // C++14, 采用std::move(pw)初始化闭包类的数据成员
	// pw = std::move(pw): 初始化捕获，位于"="左侧的，在你所指定的闭包类中数据成员的名字，而位于"="右侧的则是初始化表达式
	// "="左右两侧处于不同的作用域。左侧作用域就是闭包类的作用域，而右侧的作用域与定义lambda式的作用域相同
	// "pw = std::move(pw)"表达了"在闭包类中创建一个数据成员pw,然后使用针对局部变量pw实施std::move的结果来初始化该数据成员"
	
	auto func2 = [pw = std::make_unique<Widget32>()]{ return pw->isValidated() && pw->isArchived(); }; // C++14, 闭包类数据成员可以由std::make_unique直接初始化
	auto func7 = std::bind([](const std::unique_ptr<Widget32>& pw) {return pw->isValidated() && pw->isArchived(); }, std::make_unique<Widget32>()); // C++11
 
	auto func3 = IsValAndArch(std::make_unique<Widget32>()); // C++11
 
	auto func4 = [data32 = std::move(data32)]{ /*use of data*/ }; // C++14
	auto func5 = std::bind([](const std::vector<double>& data32) { /*use of data*/ }, std::move(data32)); // 初始化捕获的C++11模拟
	auto func6 = std::bind([](std::vector<double>& data32) mutable {/*use of data*/}, std::move(data32)); // 初始化捕获的C++11模拟，for mutable lambda
 
	return 0;
}
```
使用初始化捕获(init capture)(C++14)，可以使你指定：(1).由lambda生成的闭包类中数据成员的名字。(2).一个表达式用于初始化该数据成员。

初始化捕获又被称为广义lambda捕获(generalized lambda capture)。

移动捕获在C++11中可以采用以下方法模拟：(1).把需要捕获的对象移动到由std::bind产生的函数对象中。(2).给到lambda式一个指涉到欲”捕获”的对象的引用。

移动构造(move-construct)一个对象进C++11闭包是不可能的，但是移动构造一个对象进绑定对象(bind object)则是可能的。在C++11中模拟移动捕获(move-capture)包括以下步骤：先移动构造一个对象进一个绑定对象，然后按引用把该移动构造所得的对象传递给lambda式。因为绑定对象的生命期和闭包相同，所以针对绑定对象中的对象和闭包里的对象可以采用同样方法加以处置。

**要点速记：(1).使用C++14的初始化捕获将对象移入闭包。(2).在C++11中，可由手工实现的类或std::bind去模拟初始化捕获。**

### Item 33. 对auto&&型别的形参使用decltype，以std::forward之(Use decltype on auto&& parameters to std::forward them)
```cpp
int test_item_33()
{
	auto f1 = [](auto x) { return func33(normalize(x)); };
	auto f2 = [](auto&& param) { return func33(normalize(std::forward<decltype(param)>(param))); };
	auto f3 = [](auto&&... param) { return func33(normalize(std::forward<decltype(param)>(param)...)); };
 
	return 0;
}
```
在C++14中泛型lambda式(generic lambda)可以在形参规格(parameter specification)中使用auto。

### Item 34. 优先选用lambda式，而非std::bind(Prefer lambdas to std::bind)
```cpp
using Time = std::chrono::steady_clock::time_point; // typedef for a point in time
enum class Sound { Beep, Siren, Whistle };
using Duration = std::chrono::steady_clock::duration; // typedef for a length of time
 
void setAlarm(Time t, Sound s, Duration d) {} // at time t, make sound s for duration d
 
int test_item_34()
{
	// setSoundL("L" for "lambda") is a function object allowing a sound to be specified for a 30-sec alarm to go off an hour after it's set
	auto setSoundL1 = [](Sound s) {
		using namespace std::chrono;
		setAlarm(steady_clock::now() + hours(1), s, seconds(30));
	};
 
	auto setSoundL2 = [](Sound s) {
		using namespace std::chrono;
		using namespace std::literals; // C++14
		setAlarm(steady_clock::now() + 1h, s, 30s); // C++14
	};
 
	setSoundL1(Sound::Siren);
	setSoundL2(Sound::Siren);
 
	using namespace std::chrono;
	using namespace std::literals; // C++14
	using namespace std::placeholders; // needed for use of "_1"
	auto setSoundB1 = std::bind(setAlarm, std::bind(std::plus<>(), steady_clock::now(), 1h), _1, 30s); // C++14
	auto setSoundB2 = std::bind(setAlarm, std::bind(std::plus<steady_clock::time_point>(), steady_clock::now(), hours(1)), _1, seconds(30)); // C++11
 
	setSoundB1(Sound::Siren);
	//setSoundB2(Sound::Siren);
 
	using SetAlarm3ParamType = void(*)(Time t, Sound s, Duration d);
 
	return 0;
}
```
之所以优先选用lambda式，而非std::bind，最主要原因是lambda式具备更高的可读性。比起lambda式，使用std::bind的代码可读性更差、表达力更低，运行效率也可能更糟。在C++14中，根本没有使用std::bind的适当用例。而在C+11中，std::bind仅在两个受限的场合还算有着使用的理由：(1).移动捕获：C++11的lambda式没有提供移动捕获特性，但可以通过结合std::bind和lambda式来模拟移动捕获。(2).多态函数对象：因为绑定对象的函数调用运算符利用了完美转发，它就可以接受任何型别的实参。

**要点速记：(1).lambda式比起std::bind而言，可读性更好、表达力更强，可能运行效率也更高。(2).仅在C++11中，std::bind在实现移动捕获，或是绑定到具备模板化的函数调用运算符的对象的场合中，可能尚有余热可以发挥。**

std::bind更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/52613910




## 第7章 并发API 


### Item 35. 优先选用基于任务而非基于线程的程序设计(Prefer task-based programming to thread-based)
```cpp
int doAsyncWork() { return 1; }
 
int test_item_35()
{
	std::thread t(doAsyncWork);
	t.join();
	
	auto fut = std::async(doAsyncWork);
 
	return 0;
}
```
如果你想以异步方式运行函数doAsyncWork，有两种基本选择：你可以创建一个std::thread，并在其上运行doAsyncWork，因此这是基于线程(thread-based)的方法。或者你可以把doAsyncWork传递给std::async，这是一种基于任务(task-based)的策略。

硬件线程是实际执行计算的线程。现代计算机体系结构会为每个CPU内核提供一个或多个硬件线程。软件线程(又称操作系统线程或系统线程)是操作系统用以实施跨进程的管理，以及进行硬件线程调度的线程。通常，能够创建的软件线程会比硬件线程要多。std::thread是C++进程里的对象，用作底层软件线程的句柄。软件线程是一种有限的资源，如果你试图创建的线程数量多于系统能够提供的数量，就会抛出std::system_error异常。

比起基于线程编程，基于任务的设计能够分担你手工管理线程的艰辛，而且它提供了一种很自然的方式，让你检查异步执行函数的结果(即返回值或异常)。但是仍有几种情况下，直接使用线程会更适合，它们包括：(1).你需要访问底层线程(underlying threading)实现的API：C++并发API通常会采用特定平台的低级API(lower-level platform specific API)来实现，经常使用的有pthread或Windows线程库。它们提供的API比C++提供的更丰富(例如，C++11没有线程优先级的概念)。为了访问底层线程实现的API，std::thread通常会提供native_handle成员函数，而std::future(即std::async的返回型别)则没有该功能的对应物。(2).你需要且有能力为你的应用优化线程用法。(3).你需要实现超越C++并发API的线程技术。

**要点速记：(1).std::thread的API未提供直接获取异步运行函数返回值的途径，而且如果那些函数抛出异常，程序就会终止。(2).基于线程的程序设计要求手动管理线程耗尽(thread exhaustion)、超订(oversubscription)、负载均衡(load balancing)，以及新平台适配。(3). 通过使用默认启动策略的std::async进行基于任务的编程，可以为你解决大多数此类问题(Task-based programming via std::async with the default launch policy handles most of these issues for you)。**

std::async更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/104133494

### Item 36. 如果异步是必要的，则指定std::launch::async(Specify std::launch::async if asynchronicity is essential)
```cpp
void f36()
{
	using namespace std::literals; // for C++14 duration suffixes
	std::this_thread::sleep_for(1s);
	//std::this_thread::sleep_for(std::chrono::seconds(1)); // C++11
}
 
template<typename F, typename... Ts>
inline std::future<typename std::result_of<F(Ts...)>::type> reallyAsync(F&& f, Ts&&... params) // C++11, return future for asynchronous call to f(params...)
{
	return std::async(std::launch::async, std::forward<F>(f), std::forward<Ts>(params)...);
}
 
template<typename F, typename... Ts>
inline auto reallyAsync2(F&& f, Ts&&... params) // C++14
{
	return std::async(std::launch::async, std::forward<F>(f), std::forward<Ts>(params)...);
}
 
int test_item_36()
{
	// 下面两个调用有着完全相同的意义
	auto fut1 = std::async(f36); // run f using default launch policy
	auto fut2 = std::async(std::launch::async | std::launch::deferred, f36); // run f either async or dererred
 
	auto fut = std::async(f36);
	using namespace std::literals;
	if (fut.wait_for(0s) == std::future_status::deferred) { // 如果任务被推迟了
		// use wait or get on fut to call f synchronously
	} else { // task isn't deferred
		while (fut.wait_for(100s) != std::future_status::ready) { // 不可能死循环(前提假设f36会结束)
			// task is neither deferred nor ready, so do concurrent work until it's ready
		}
 
		// fut is ready
	}
 
	auto fut3 = std::async(std::launch::async, f36); // launch f asynchronously
 
	auto fut4 = reallyAsync(f36); // 以异步方式运行f，如果std::async会抛出异常reallyAsync也会抛出异常
	auto fut5 = reallyAsync2(f36);
 
	return 0;
}
```
当调用std::async来执行一个函数(或可调用对象)时，仅仅通过std::async来运行，你实际上要求的并非一定会达成异步运行的结果，你要求的仅仅是让该函数以符合std::async的启动策略(launch policy)来运行。**有两个基本策略，它们都是用限定作用域的枚举型别std::launch中的枚举量来表示的**。假设函数f要传递给std::async执行，则：(1).std::launch::async启动策略意味着函数f必须以异步方式运行，即在不同的线程上执行。(2).std::launch::deferred启动策略意味着函数f只会在std::async所返回的期值(future)的get或wait得到调用时才运行，即调用方会阻塞直至f运行结束为止，如果get或wait都没有得到调用，f是不会运行的。std::async的默认启动策略，也就是你如果不明确指定一个的话，它采用的并非以上两者中的一种。相反地，它采用的是对二者进行或运算的结果。

以默认启动策略对任务使用std::async能正常工作需要满足以下所有条件：(1).任务不需要与调用get或wait的线程并发执行。(2).读或写哪个线程的thread_local变量都没有关系。(3).或者可以给出保证在std::async返回的期值(future)之上可以调用get或wait，或者可以接受任务可能永不执行。(4).使用wait_for或wait_unitil的代码会将任务被推迟的可能性纳入考量(the possibility of deferred status into account)。

你想要确保任务以异步方式执行，实现这一点的方法就是在调用时把std::launch::async作为第一个实参传入。

**要点速记：(1).std::async的默认启动策略既允许任务以异步方式执行，也允许任务以同步方式执行。(2).如此的弹性会导致使用thread_local变量时的不确定性，隐含着任务可能永远不会执行，还会影响运用了基于超时的wait调用的程序逻辑。(3).如果异步是必要的，则指定std::launch::async。**

std::future更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/104115489

### Item 37. 使std::thread型别对象在所有路径皆不可联结(Make std::threads unjoinable on all paths)
```cpp
void f37() {}
 
// 下面这个类允许调用者在销毁ThreadRAII对象(用于std::thread的RAII对象)时是调用join还是detach
class ThreadRAII {
public:
	enum class DtorAction { join, detach };
 
	// 构造函数只接受右值型别的std::thread
	ThreadRAII(std::thread&& t, DtorAction a) : action(a), t(std::move(t)) {}
 
	~ThreadRAII()
	{
		if (t.joinable()) {
			if (action == DtorAction::join) {
				t.join();
			} else {
				t.detach();
			}
		}
	}
 
	// support moving
	ThreadRAII(ThreadRAII&&) = default;
	ThreadRAII& operator=(ThreadRAII&&) = default;
 
	std::thread& get() { return t; }
 
private:
	DtorAction action;
	std::thread t;
};
 
int test_item_37()
{
	ThreadRAII trall(std::thread(f37), ThreadRAII::DtorAction::join);
 
	return 0;
}
```
每个std::thread型别对象皆处于两种状态之一：可联结或不可联结(joinable or unjoinable)。可联结的std::thread对应底层(underlying)以异步方式已运行或可运行的线程。std::thread型别对象对应的底层线程(underlying thread)若处于阻塞或等待调度，则它可联结。std::thread型别对象对应的底层线程如已运行至结束，则亦认为其可联结。

不可联结的std::thread不处于以上可联结的状态。不可联结的std::thread型别对象包括：(1).默认构造的std::thread：此类std::thread没有可以执行的函数，因此也没有对应的执行底层线程。(2).已移动的std::thread。(3).已联结的std::thread。(4).已分离(detached)的std::thread。

**针对可联结的std::thread型别对象实施析构会导致程序终止。**

**要点速记：(1).使std::thread型别对象在所有路径皆不可联结。(2).在析构时调用join可能导致难以调试的性能异常。(3).在析构时调用detach可能导致难以调试的未定义行为。(4).在成员列表的最后声明std::thread型别对象。**

std::thread更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/73393229

### Item 38. 对变化多端的线程句柄析构函数行为保持关注(Be aware of varying thread handle destructor behavior)
```cpp
class Widget38 { // Widget38 objects might block in their destructor
public: 
 
private:
	std::shared_future<double> fut;
};
 
int calcValue() { return 1; }
 
int test_item_38()
{
	// this container might block in its destructor, because one or more contained futures could refer to a shared state for a nondeferred task launched via std::async
	std::vector<std::future<void>> futs;
 
	std::packaged_task<int()> pt(calcValue); // 给calcValue加上包装使之能以异步方式运行
	auto fut = pt.get_future(); // get future for pt
	std::thread t(std::move(pt));
	t.join();
	
	return 0;
}
```


针对可联结的std::thread型别对象实施析构会导致程序终止。而期值(future)的析构函数，有时候行为像是执行了一次隐式join，有时候行为像是执行了一次隐式detach，有时候行为像是二者都没有执行，但它从不会导致程序终止。

std::packaged_task型别对象一经创建，就会运行在线程上(它也可以经由std::async的调用而运行，但是如果你要用std::async运行任务，就没有很好的理由再去创建什么std::packaged_task型别对象，因为std::async能够在调度任务执行之前就做到std::packaged_task能够做到的任何事情)。std::packaged_task不能拷贝。

**要点速记：(1).期值(future)的析构函数在常规情况下，仅会析构期值的成员变量。(2).指涉到经由std::aysnc启动的未推迟(non-deferred)任务的共享状态的最后一个期值会保持阻塞直至该任务结束(The final future referring to a shared state for a non-deferred task launched via std::async blocks until the task completes)。**

std::packaged_task更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/104127352

std::shared_future更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/104118831

### Item 39. 考虑针对一次性事件通信使用以void为模板型别实参的期值(Consider void futures for one-shot event communication)
```cpp
std::promise<void> p;
 
void react() {} // function for reacting task
 
void detect() // function for detecting task, 暂停线程一次
{
	std::thread t([] {p.get_future().wait(); react(); }); // create thread, suspend t until future is set
 
	// ... // here, t is suspended prior to call to react
 
	p.set_value(); // unsuspend t (and thus call react)
 
	// ... // do additional work
 
	t.join(); // make t unjoinable
}
 
void detect_multi() // now for multiple reacting tasks
{
	auto sf = p.get_future().share(); // sf's type is std::shared_future<void>
	std::vector<std::thread> vt; // container for reacting threads
	for (int i = 0; i < /*threadsToRun*/2; ++i) {
		vt.emplace_back([sf] {sf.wait(); react(); }); // wait on local copy of sf
	}
 
	// ... // detect hangs if this "…" code throws
 
	p.set_value(); // unsuspend all threads
 
	// ...
 
	for (auto& t : vt) { // make all threads unjoinable
		t.join();
	}
}
 
int test_item_39()
{
	return 0;
}
```
**要点速记：(1).如果仅为了实现简单事件通信，基于条件变量的设计会要求多余的互斥量，这会给相互关联的检测和反应任务带来约束，并要求反应任务校验事件确已发生。(2).使用标志位的设计可以避免上述问题，但这一设计基于轮询而非阻塞(based on polling, not blocking)。(3).条件变量和标志位可以一起使用，但这样的通信机制设计结果不甚自然(somewhat stilted)。(4).使用std::promise型别对象和期值(future)就可以回避这些问题，但是这个途径为了共享状态需要使用堆内存，而且仅限于一次性通信。**

std::promise更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/104124174

### Item 40. 对并发使用std::atomic，对特种内存使用volatile(Use std::atomic for concurrency, volatile for special memory)
```cpp
int test_item_40()
{
	std::atomic<int> ai(0); // initialize ai to 0
	ai = 10; // atomically set ai to 10
	std::cout << ai << std::endl; // atomically read ai's value
	++ai; // atomically increment ai to 11
	--ai; // atomically decrement ai to 10
 
	volatile int x = 0;
	auto y = x; // read x
	y = x; // read x again(can't be optimized away,不可以被优化掉)
 
	x = 10; // write x(can't be optimized away)
	x = 20; // write x again
 
	std::atomic<int> x2;
	std::atomic<int> y2(x2.load()); // read x2
	y2.store(x2.load()); // read x2 again
 
	register int z2 = x2.load(); // read x2 into register
	std::atomic<int> y2_(z2); // init y2_ with register value
	y2_.store(z2); // store register value into y2_
 
	volatile std::atomic<int> vai; // operations on vai are atomic and can't be optimized away
 
	return 0;
}
```
std::atomic模板的实例(例如，std::atomic<int>, std::atomic<bool>和std::atomic<Widget*>等)提供的操作可以保证被其它线程视为原子的。一旦构造了一个std::atomic型别对象，针对它的操作就好像这些操作处于受互斥量保护的临界区域内一样，但是实际上这些操作通常会使用特殊的机器指令来实现，这些指令比使用互斥量来得更加高效。

volatile的用处就是告诉编译器，正在处理的是特种内存(special memory)，它的意思是通知编译器”不要对在此内存上的操作做任何优化”。可能最常见的特种内存是用于内存映射I/O的内存。这种内存的位置实际上是用于与外部设备(例如，外部传感器、显示器、打印机和网络端口等)通信，而非用于读取或写入常规内存(即RAM)。编译器可以消除std::atomic型别上的冗余操作。std::atomic对于并发程序设计有用，但不能用于访问特种内存。volatile对于访问特种内存有用，但不能用于并发程序设计。由于std::atomic和volatile是用于不同目的，它们甚至可以一起使用。访问std::atomic型别对象通常比访问非std::atomic型别对象慢得多。

**要点速记：(1).std::atomic用于多线程访问的数据，且不用互斥量。它是编写并发软件的工具。(2).volatile用于读写操作不可以被优化掉的内存。它是在面对特种内存时使用的工具。**

std::atomic更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/73436710

volatile更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/104109696





## 第8章 微调  


### Item 41. 针对可复制的形参，在移动成本低并且一定会被复制的前提下，才考虑将其按值传递(Consider pass by value for copyable parameters that are cheap to move and always copied)
```cpp
class Widget41 {
public:
	 method 1: by-reference approaches
	//void addName(const std::string& newName) // 接受左值，对其实施拷贝
	//{
	//	names.push_back(newName);
	//}
 
	//void addName(std::string&& newName) // 接受右值，对其实施移动
	//{
	//	names.push_back(std::move(newName));
	//}
 
	 method2: by-reference approaches
	//template<typename T>
	//void addName(T&& newName) // 万能引用，接受左值也接受右值，对左值实施拷贝，对右值实施移动
	//{
	//	names.push_back(std::forward<T>(newName));
	//}
 
	// method3: by-value approaches
	void addName(std::string newName) // 即接受左值也接受右值，对右值实施移动
	{
		names.push_back(std::move(newName));
	}
 
private:
	std::vector<std::string> names;
};
 
int test_item_41()
{
	Widget41 w;
	std::string name("Bart");
	w.addName(name); // call addName with lvalue
	w.addName(name + "Jenne"); // call addName with rvalue
 
	return 0;
}
```
**要点速记：(1).对于可复制的、在移动成本低廉并且一定会被复制的形参而言，按值传递可能会和按引用传递具备相近的效率，并可能生成更少量的目标代码。(2).构造复制形参的成本可能比赋值复制形参高出很多。(3).按值传递肯定会导致切片问题(slicing problem)，所以基类型别特别不适用于按值传递。**

### Item 42. 考虑置入而非插入(Consider emplacement instead of insertion)
```cpp
int test_item_42()
{
	std::vector<std::string> vs;
	vs.push_back("xyzzy"); // 调用两次构造函数，一次析构函数
	vs.emplace_back("xyzzy"); // 调用一次构造函数，不涉及任何临时对象
 
	vs.emplace_back(50, 'x'); // insert std::string consisting of 50 'x' characters
 
	std::string queenOfDisco("Donna Summer");
	// 以下两条语句效果相同
	vs.push_back(queenOfDisco); // copy-construct queenOfDisco at end of vs
	vs.emplace_back(queenOfDisco); // copy-construct queenOfDisco at end of vs
 
	//std::regex r1 = nullptr; // 拷贝初始化(copy initialization),error, won't compile
	//std::regex r2(nullptr); // 直接初始化(direct initialization),can compile
 
	std::vector<std::regex> regexes;
	//regexes.emplace_back(nullptr); // 能编译，直接初始化允许使用接受指针的、带有explicit声明的std::regex构造函数
	//regexes.push_back(nullptr); // 不能编译，拷贝初始化禁止使用带指针的、explicit声明的std::regex构造函数
 
	return 0;
}
```
emplace_back可用于任何支持push_back的标准容器。相似地，所有支持push_front的标准容器也支持emplace_front。还有，任何支持插入(insert)操作(即除了std::forward_list和std::array以外的所有标准容器)都支持置入操作(emplace)。置入函数可避免临时对象的创建和析构，但插入函数就无法避免。即使在插入函数并不要求创建临时对象的情况下，也可以使用置入函数。在那种情况下，插入函数和置入函数本质上做的是同一件事。

如果下列情况都成立，那么置入将几乎肯定会比插入更高效：(1).欲添加的值是以构造而非赋值方式加入容器。(2).传递的实参型别与容器保存的型别不同。(3).容器不太可能由于重复值而拒绝新值(The container is unlikely to reject the new value as a duplicate)。

在决定是否选用置入函数时，还有其它两个问题值得操心：第一个和资源管理有关。第二个是它们与显式构造函数之间的交互(interaction)。在使用置入函数时，要特别小心保证传递了正确的实参。

**要点速记：(1).从原理上说，置入函数(emplacement function)应该有时比对应的插入函数高效，而且不应该有更低效的可能。(2).从实践上说，置入函数在以下几个前提成立时，极有可能会运行得更快：待添加的值是以构造而非赋值方式加入容器；传递的实参型别与容器保存的参数型别不同；容器不会因为重复值而拒绝待添加的值。(3).置入函数可能会执行类型转换，而插入函数会拒绝这些类型转换。**

emplace更多介绍参考：https://blog.csdn.net/fengbingchun/article/details/78670376

GitHub：https://github.com/fengbingchun/Messy_Test


