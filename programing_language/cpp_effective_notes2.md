
## 1 Effective_C++

### 1  让自己习惯C++(Accustoming Yourself to C++) 

#### Item  1 视C++为一个语言联邦(View C++ as a federation of languages)   

名变换：C++支持重载，C不支持。要禁止名变换，使用C++的extern “C”。不要将extern “C”看作是声明这个函数是用C语言写的，应该看作是声明这个函数应该被当作好像C写的一样而进行调用。

静态初始化：在main执行前和执行后都有大量代码被执行。尤其是，静态的类对象和定义在全局的、命名空间中的或文件体中的类对象的构造函数通常在main被执行前就被调用。同样，通过静态初始化产生的对象也要在静态析构过程中调用其析构函数，这个过程通常发生在main结束运行之后。

动态内存分配：C++部分使用new和delete，C部分使用malloc(或其变形)和free。

同一程序下混合C++与C编程，记住下面的指导原则：

(1).确保C++和C编译器产生兼容的obj文件；

(2).将在两种语言下都使用的函数声明为extern “C”；

(3).只要可能，用C++写main()；

(4).总用delete释放new分配的内存；总用free释放malloc分配的内存；

(5).将在两种语言间传递的东西限制在用C编译的数据结构的范围内；这些结构的C++版本可以包含非虚成员函数

#### Item  2 尽量以const, enum, inline替换#define(Prefer consts, enums, and inlines to #defines)   

宁可以编译器替换预处理器

(1). 对于单纯常量，最好以const对象或enum替换#define；(2). 对于形式函数的宏(macro)，最好改用inline函数替换#define。

prefer:

```cpp
class GamePlayer {

private:
  static const int NumTurns = 5; // 常量声明式
  int scores[NumTurns]; // 使用该常量

};

int test_item_2()
{
	#define ASPECT_RATIO 1.653 // 不推荐
	const double AspectRatio = 1.653; // 推荐

	{const char* const authorName = "Scott Meyers";} // 推荐
	const std::string autorName("Scott Meyers"); // 更推荐

	return 0;
}
```

第一种是定义常量指针(constant pointers)，由于常量定义式通常被放在头文件内(以便被不同的源码含入)，因此有必要将指针(而不只是指针所指之物)声明为const。string对象通常比其前辈char*-base合宜。   
第二种是class专属长量。让它成为class的一个成员(member),让它成为一个static成员.    

#### Item  3 尽可能使用const(Use const whenever possible)   

如果关键字const出现在星号左边，表示被指物是常量；如果出现在星号右边，表示指针自身是常量；如果出现在星号两边，表示被指物和指针两者都是常量。

STL迭代器系以指针为根据塑模出来，所以迭代器的作用就像个T*指针。声明迭代器为const就像声明指针为const一样(即声明一个T* const指针)，表示这个迭代器不得指向不同的东西，但它所指的东西的值是可以改动
的。如果你希望迭代器所指的东西不可被改动(即希望STL模拟一个const T*指针)，你需要的是const_iterator。

请记住：(1).将某些东西声明为const可帮助编译器侦测出错误用法。const可被施加于任何作用域内的对象、函数参数、函数返回类型、成员函数本体。(2).编译器强制实施bitwise constness。(3).当const和non-const成员函数有着实质等价的实现时，令non-const版本调用const版本可避免代码重复

#### Item  4 确定对象被使用前已先被初始化(Make sure that objects are initialized before they’re used)   
  
### 2  构造/析构/赋值运算(Constructors, Destructors, and Assignment Operators) 
  
#### Item  5 了解C++默默编写并调用哪些函数(Know what functions C++ silently writes and calls)      
#### Item  6 若不想使用编译器自动生成的函数，就该明确拒绝(Explicitly disallow the use of compiler-generated functions you do not want)   
#### Item  7 为多态基类声明virtual析构函数(Declare destructors virtual in polymorphic base classes)   
#### Item  8 别让异常逃离析构函数(Prevent exceptions from leaving destructors)   
#### Item  9 绝不在构造和析构过程中调用virtual函数(Never call virtual functions during construction or destruction)   
#### Item  10 令operator=返回一个reference to *this(Have assignment operators return a reference to *this)  

```cpp
Widget& operator+= (const Widget& rhs) // 这个协议适用于+=、-=、*=等等
{
	return *this;
}
```
 
#### Item  11 在operator=中处理”自我赋值”(Handle assignment to self in operator=)   

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

确保当对象自我赋值时operator=有良好行为。其中技术包括比较”来源对象”和”目标对象”的地址、精心周到的语句顺序、以及copy-and-swap
```

**不要重载”&&”, “||”,或”,”**

**通过重载避免隐式类型转换**
 
 每一个重载的operator必须带有一个用户定义类型(user-defined type)的参数,利用重载避免临时对象的方法不只是用在operator函数上。

**考虑用运算符的赋值形式(op=)取代其单独形式(op)**

确保operator的赋值形式(assignment version)(例如operator+=)与一个operator的单独形式(stand-alone)(例如operator+)之间存在正常的关系，一种好方法是后者(指operator+)根据前者(指operator+=)来实现。


#### Item  12 复制对象时勿忘其每一个成分(Copy all parts of an object)    

### 3  资源管理(Resource Management) 
 
#### Item  13 以对象管理资源(Use objects to manage resources)   

在C++11中auto_ptr已经被废弃，用unique_ptr替代

unique_ptr 独占所指向的对象,shared_ptr(make_shared)允许多个指针指向同一个对象

(1).为防止资源泄漏，请使用RAII(Resource Acquisition Is Initialization,资源取得时机便是初始化时机)对象，它们在构造函数中获得资源并在析构函数中释放资源。(2).两个常被使用的RAII classes分别是stdn::shared_ptr和auto_ptr。前者通常是较佳选择，因为其copy行为比较直观。若选择auto_ptr，复制动作会使它(被复制物)指向null

以独立语句将newed对象置入智能指针：
```cpp
int test_item_17()
{
	// 执行new Widget17; 调用priority; 调用std::shared_ptr构造函数，它们的执行顺序不确定
	processWidget(std::shared_ptr<Widget17>(new Widget17), priority()); // 可能泄露资源

	std::shared_ptr<Widget17> pw(new Widget17); // 在单独语句内以智能指针存储newed所得对象
	processWidget(pw, priority()); // 这个调用动作绝不至于造成泄露

	return 0;
}
```
#### Item  14 在资源管理类中小心copying行为(Think carefully about copying behavior in resource-managing classes)   
#### Item  15 在资源管理类中提供对原始资源的访问(Provide access to raw resources in resource-managing classes)   
#### Item  16 成对使用new和delete时要采用相同形式(Use the same form in corresponding uses of new and delete)   

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

new(也就是通过new动态生成一个对象)，有两件事发生：第一，内存被分配出来(通过名为operator new的函数)；第二，针对此内存会有一个(或更多)构造函数被调用。

delete，也有两件事发生：针对此内存会有一个(或更多)析构函数被调用，然后内存才被释放(通过名为operator delete的函数)。

指针使用delete: 唯一能够让delete知道内存中是否存在一个”数组大小记录”的办法就是：由你来告诉它。如果你使用delete时加上中括号(方括号)，delete便认定指针指向一个数组，否则它便认定指针指向单一对象。

new表达式中使用[]，必须在相应的delete表达式中也使用[]。如果你在new表达式中不使用[]，一定不要在相应的delete表达式中使用[]。

#### Item  17 以独立语句将newed对象置入智能指针(Store newed objects in smart pointers in standalone statements)   
 
### 4  设计与声明(Designs and Declarations) 

#### Item  18 让接口容易被正确使用，不易被误用(Make interface easy to use correctly and hard to use incorrectly)   

请记住：(1).好的接口很容易被正确使用，不容易被误用。你应该在你的所有接口中努力达成这些性质。

(2).”促进正确使用”的办法包括接口的一致性，以及与内置类型的行为兼容。

(3).”阻止误用”的办法包括建立新类型、限制类型上的操作，束缚对象值，以及消除客户的资源管理责任。

(4).std::shared_ptr支持定制型删除器(custom deleter)。这可防范DLL问题，可被用来自动解除互斥锁(mutex)等等。

#### Item  19 设计class犹如设计type(Treat class design as type design)   
#### Item  20 宁以pass-by-reference-to-const替换pass-by-value(Prefer pass-by-reference-to-const to pass-by-value) 

pass-by-value：内置类型，以及STL的迭代器和函数对象

pass-by-reference-to-const：尽量以pass-by-reference-to-const替换pass-by-value。前者通常比较高效，并可避免切割问题(slicing problem)。
  
#### Item  21 必须返回对象时，别妄想返回其reference(Don’t try to return a reference when you must return an object) 

绝不要返回pointer或reference指向一个local stack对象，或返回reference指向一个heap-allocated对象，或返回pointer或reference指向一个local static对象而有可能同时需要多个这样的对象。
  
#### Item  22 将成员变量声明为private(Declare data members private)   
#### Item  23 宁以non-member、non-friend替换member函数(Prefer non-member non-friend functions to member functions)   
#### Item  24 若所有参数皆需类型转换，请为此采用non-member函数(Declare non-member functions when type conversions should apply to all parameters)   
#### Item  25 考虑写出一个不抛异常的swap函数(Consider support for a non-throwing swap)   

### 5 实现(Implementations) 
 
#### Item  26 尽可能延后变量定义式的出现时间(Postpone variable definitions as long as possible) 

尽可能延后变量定义式的出现。这样做可增加程序的清晰度并改善程序效率。
  
#### Item  27 尽量少做转型动作(Minimize casting)   

const_cast: 通常被用来将对象的常量性移除(cast away the constness)。它也是唯一有此能力的C++-style转型操作符

dynamic_cast: 主要用来执行”安全向下转型”(safe downcasting)，也就是用来决定某对象是否归属继承体系中的某个类型，它不能被用来缺乏虚函数的类型上

reinterpret_cast:意图执行低级转型，实际动作(及结果)可能取决于编译器

static_cast: 用来强迫隐式转换(implicit conversions)，例如将non-const对象转为const对象，或将int转为double等等

如果可以，尽量避免转型，特别是在注重效率的代码中避免dynamic_cast。如果有个设计需要转型动作，试着发展无须转型的替代设计

#### Item  28 避免返回handles指向对象内部成分(Avoid returning “handles” to object internals)   

reference、指针和迭代器统统都是所谓的handles(号码牌，用来取得某个对象)，而返回一个”代表对象内部数据”的handle，随之而来的便是”降低对象封装性”的风险。

请记住：避免返回handles(包括reference、指针、迭代器)指向对象内部。遵守这个条款可增加封装性，帮助const成员函数的行为像个const，并将发生”虚吊号码牌”(dangling handles)的可能性降至最低。

#### Item  29 为”异常安全”而努力是值得的(Strive for exception-safe code)   
#### Item  30 透彻了解inlining的里里外外(Understand the ins and outs of inlining)  

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

inline函数：将”对此函数的每一个调用”都以函数本体替换之

inlining在大多数C++程序中是编译期行为。

(1).将大多数inlining限制在小型、被频繁调用的函数身上。这可使日后的调试过程和二进制升级(binary upgradability)更容易，也可使潜在的代码膨胀问题最小化，使程序的速度提升机会最大化。

(2).不要只因为function templates出现在头文件，就将它们声明为inline。

#### Item  31 将文件间的编译依存关系降至最低(Minimize compilation dependencies between files)   

如果使用object references或object pointers可以完成任务，就不要使用objects。

以class声明式替换class定义式。

Handle classes：pointer to implementation，成员函数必须通过implementation pointer取得对象数据。

interface classes：特殊的abstract base class(抽象基类），由于每个函数都是virtual，所以你必须为每次函数调用付出一个间接跳跃(indirect jump)成本

(1). 支持”编译依存性最小化”的一般构想是：相依于声明式，不要相依于定义式。基于此构想的两个手段是handle classes和interface classes。

(2). 程序库头文件应该以”完全且仅有声明式”(full and declaration-only forms)的形式存在。这种做法不论是否涉及templates都适用
  

### 6  继承与面向对象设计(Inheritance and Object-Oriented Design) 
 
#### Item  32 确定你的public继承塑模出is-a关系(Make sure public inheritance models “is-a”)   

public inheritance(公开继承)意味”is-a”(是一种)的关系

”public继承”意味is-a。适用于base classes身上的每一件事情一定也适用于derived classes身上，因为每一个derived class对象也都是一个base class对象

#### Item  33 避免遮挡继承而来的名称(Avoid hiding inherited names)   

derived classes内的名称会遮掩base classes内的名称。在public继承下从来没有人希望如此。(2).为了让被遮掩的名称再见天日，可使用using声明式或转交函数(forwarding functions)。

#### Item  34 区分接口继承和实现继承(Differentiate between inheritance of interface and inheritance of implementation)   

pure virtual函数：为了让derived classes只继承函数接口。

简朴的(非纯)impure virtual函数：让derived classes继承该函数的接口和缺省实现。

non-virtual函数的：为了令derived classes继承函数的接口及一份强制性实现

(1).接口继承和实现继承不同。在public继承之下，derived classes总是继承base class的接口。(2). pure virtual函数只具体指定接口继承。(3). 简朴的(非纯)impure virtual函数具体指定接口继承及缺省实现继承。(4). non-virtual函数具体指定接口继承以及强制性实现继承。

#### Item  35 考虑virtual函数以外的其它选择(Consider alternatives to virtual functions)   

(1). virtual函数的替代方案包括non-virtual interface(NVI)手法及Strategy设计模式的多种形式。NVI手法自身是一个特殊形式的Template Method设计模式。   
(2). 将机能从成员函数移到class外部函数，带来的一个缺点是，非成员函数无法访问class的non-public成员。   
(3). std::function对象的行为就像一般函数指针。这样的对象可接纳”与给定之目标签名式(target signature)兼容”的所有可调用物(callable entities)。  

#### Item  36 绝不重新定义继承而来的non-virtual函数(Never redefine an inherited non-virtual function)   
#### Item  37 绝不重新定义继承而来的缺省参数值(Never redefine a function’s inherited default parameter value)   

virtual函数系动态绑定(dynamically bound)，而缺省参数值确是静态绑定(statically bound)。静态绑定又名前期绑定，early binding；动态绑定又名后期绑定，late binding
     
#### Item  38 通过复合塑模出has-a或”根据某物实现出”(Model “has-a” or “is-implemented-in-terms-of” through composition)     

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

(1). 复合(composition)的意义和public继承完全不同。    
(2). 在应用域(application domain)，复合意味has-a(有一个)。在实现域(implementation domain)，复合意味is-implemented-in-terms-of(根据某物实现出)。   
  
#### Item  39 明智而审慎地使用private继承(Use private inheritance judiciously)    

尽可能使用复合，必要时才使用private继承。

(1). private继承意味is-implemented-in-terms-of(根据某物实现出)。它通常比复合(composition)的级别低。但是当derived class需要访问protected base class的成员，或需要重新定义继承而来的virtual函数时，这么设计是合理的。

(2).和复合(composition)不同，private继承可以造成empty base最优化。这对致力于”对象尺寸最小化”的程序库开发者而言，可能很重要。
   
#### Item  40 明智而审慎地使用多重继承(Use multiple inheritance judiciously)       


### 7  模板与泛型编程(Templates and Generic Programming) 

#### Item  41 了解隐式接口和编译期多态(Understand implicit interfaces and compile-time polymorphism)   
#### Item  42 了解typename的双重意义(Understand the two meanings of typename)   

typename必须作为嵌套从属类型名称的前缀词

(1).声明template参数时，前缀关键字class和typename可互换。   
(2).请使用关键字typename标识嵌套从属类型名称；但不得在base class lists(基类列)或member initialization list(成员初值列)内以它作为base class修饰符。  

#### Item  43 学习处理模板化基类内的名称(Know how to access names in templatized base classes)   
#### Item  44 将与参数无关的代码抽离templates(Factor parameter-independent code out of templates)   
#### Item  45 运用成员函数模板接受所有兼容类型(Use member function templates to accept “all compatible types”)   
#### Item  46 需要类型转换时请为模板定义非成员函数(Define non-member functions inside templates when type conversions are desired)   
#### Item  47 请使用traits classes表现类型信息(Use traits classes for information about types)   
#### Item  48 认识template元编程(Be aware of template metaprogramming)       

### 8  定制new和delete(Customizing new and delete) 

#### Item  49 了解new-handler的行为(Understand the behavior of the new-handler)   
#### Item  50 了解new和delete的合理替换时机(Understand when it makes sense to replace new and delete)   
#### Item  51 编写new和delete时需固守常规(Adhere to convention when writing new and delete)   
#### Item  52 写了placement new也要写placement delete(Write placement delete if you write placement new)   

### 9 杂项讨论(Miscellany) 

#### Item  53 不要轻忽编译器的警告(Pay attention to compiler warnings)   
#### Item  54 让自己熟悉包括RT1在内的标准程序库(Familiarize yourself with the standard library, including TR1)   
#### Item  55 让自己熟悉Boost(Familiarize yourself with Boost)      


## 2 more effective C++

### 1 基础议题（Basics）   

#### Item  1 指针与引用的区别   

在任何情况下都不能使用指向空值的引用。一个引用必须总是指向某些对象。在C++里，引用应被初始化。不存在指向空值的引用这个事实意味着使用引用的代码效率比使用指针的要高。

#### Item  2 尽量使用C++风格的类型转换   
#### Item  3 不要对数组使用多态   
#### Item  4 避免无用的缺省构造函数     

###  2 操作符（Operators）  

#### Item  5 谨慎定义类型转换函数   
#### Item  6 自增(increment)、自减(decrement)操作符前缀形式与后缀形式的区别   
#### Item  7 不要重载”&&”, “||”,或”,”   
#### Item  8 理解各种不同含义的new和delete 

new操作符(new operator)和new操作(operator new)的区别：    
new操作符：1 分配足够的内存以便容纳所需类型的对象，2调用构造函数初始化内存中的对象       
函数operator new：仅分配内存，不会调用构造函数    
函数operator delete与delete操作符的关系与operator new与new操作符的关系一样   

### 3 异常（Exceptions）  

#### Item  9 使用析构函数防止资源泄漏   
#### Item  10 在构造函数中防止资源泄漏   
#### Item  11 禁止异常信息(exceptions)传递到析构函数外   
#### Item  12 理解”抛出一个异常”与”传递一个参数”或”调用一个虚函数”间的差异   

调用函数时，程序的控制权最终还会返回到函数的调用处，但是当你抛出一个异常时，控制权永远不会回到抛出异常的地方。

catch子句匹配顺序总是取决于它们在程序中出现的顺序。因此一个派生类异常可能被处理其基类异常的catch子句捕获。

#### Item  13 通过引用(reference)捕获异常   
#### Item  14 审慎使用异常规格(exception specifications)   
#### Item  15 了解异常处理的系统开销   


### 4 效率（Efficiency）  

#### Item  16 牢记80-20准则(80-20 rule)   

大约20%的代码使用了80%的程序资源；大约20%的代码耗用了大约80%的运行时间；大约20%的代码使用了80%的内存。

#### Item  17 考虑使用lazy evaluation(懒惰计算法)   
#### Item  18 分期摊还期望的计算   

over-eager evaluation(过度热情计算法)：如果你认为一个计算需要频繁进行，你就可以设计一个数据结构高效地处理这些计算需求，这样可以降低每次计算需求时的开销。

lazy evaluation：必须支持某些操作而不总需要其结果，以提高程序效率。

over-eager：必须支持某些操作而其结果几乎总是被需要或不止一次地需要时，以提高程序效率。

#### Item  19 理解临时对象的来源   

未命名的对象通常在两种条件下产生：为了使函数成功调用而进行隐式类型转换和函数返回对象时。

未来时态的考虑增加了你的代码的可重用性、可维护性、健壮性，以及在环境发生改变时易于修改。

#### Item  20 协助完成返回值优化   
#### Item  21 通过重载避免隐式类型转换   
#### Item  22 考虑用运算符的赋值形式(op=)取代其单独形式(op)   
#### Item  23 考虑变更程序库   
#### Item  24 理解虚拟函数、多继承、虚基类和RTTI所需的代码   

virtual table和virtual table pointers，通常被分别地称为vtbl和vptr。

虚函数表：一个vtbl通常是一个函数指针数组，每个类应该只有一个virtual table，类的vtbl的大小与类中声明的虚函数的数量成正比。类中vtbl的项目是指向虚函数实现体的指针。

虚函数是不能内联的。这是因为”内联”是指”在编译期间用被调用的函数体本身来代替函数调用的指令”，但是虚函数的”虚”是指”直到运行时才能知道要调用的是哪一个函数”。

RTTI(运行时类型识别): 在运行时找到对象和类的有关信息，所以肯定有某个地方存储了这些信息让我们查询。这些信息被存储在类型为type_info的对象里，你能通过使用typeid操作符访问一个类的type_info对象。
   

### 5 技术（Techniques， Idioms， Patterns）  

#### Item  25 将构造函数和非成员函数虚拟化   
#### Item  26 限制某个类所能产生的对象数量   
#### Item  27 要求或禁止在堆中产生对象   
#### Item  28 灵巧(smart)指针   
#### Item  29 引用计数  

引用计数是这样一个技巧，它允许多个有相同值的对象共享这个值的实现。

作用：第一个是简化跟踪堆中的对象的过程。节省内存，而且可以使得程序运行更快。
 
#### Item  30 代理类 

Proxy类可以完成一些其它方法很难甚至可不能实现的行为：多维数组，左值/右值的区分，限制隐式类型转换。
  
#### Item  31 让函数根据一个以上的对象来决定怎么虚拟       

### 6 杂项讨论（Miscellany） 
 
#### Item  32 在未来时态下开发程序   
#### Item  33 将非尾端类设计为抽象类   
#### Item  34 如何在同一程序中混合使用C++和C   
#### Item  35 让自己习惯使用标准C++语言   


## 3 effective STL

### 1 容器 

#### Item  1: 慎重选择容器类型   

标准STL序列容器：vector、string、deque、list、forward_list(C++11)、array(C++11)。    
标准STL关联容器：set、multiset、map、multimap、unordered_set(C++11)、unordered_multiset(C++11)、unordered_map(C++11)、unordered_multimap(C++11)。   
标准的非STL容器，包括：bitset(include <bitset>)、valarray(include <valarray>)。其它STL容器：stack(include <stack>)、queue(include <queue>)和priority_queue((include <queue>))。    
连续内存容器：vector、string、deque    
链表的容器：比如list、forward_list   
事务语义:只有list对多个元素的插入操作提供了事务语义   

#### Item  2: 不要试图编写独立于容器类型的代码   

STL是以泛化(generalization)原则为基础的：数组被泛化为”以其包含的对象的类型为参数”的容器，函数被泛化为”以其使用的迭代器的类型为参数”的算法，指针被泛化为”以其指向的对象的类型为参数”的迭代器

迭代器： 前向迭代器、双向迭代器

序列容器：支持push_front和/或push _back操作

关联容器: 提供了对数时间的lower_bound、upper_bound和equal_range成员函数

标准的连续内存容器: 提供了随机访问迭代器

标准的基于节点的容器: 提供了双向迭代器

#### Item  3: 确保容器中的对象拷贝正确而高效 

STL的工作方式（拷贝）：进去的是拷贝（insert或push_back），出来的也是拷贝（front或back）(copy in, copy out)。进一步被拷贝，插入或删除操作时，现有元素的位置通常会被移动(拷贝)；排序算法，next_permutation或previous_permutation, remove、unique或类似的操作，rotate或reverse,那么对象将会被移动(拷贝)

”剥离”问题意味着向基类对象的容器中插入派生类对象几乎总是错误的。使拷贝动作高效、正确，并防止剥离问题发生的一个简单办法是使容器包含指针而不是对象。
  
#### Item  4: 调用empty而不是检查size()是否为0 

empty对所有的标准容器都是常数时间操作，而对一些list实现，size耗费线性时间。
  
#### Item 5: 区间成员函数优先于与之对应的单元素成员函数

```cpp
std::vector<Widget5> v1, v2;
v1.assign(v2.begin() + v2.size() / 2, v2.end()); // 推荐

v1.clear();
for (std::vector<Widget5>::const_iterator ci = v2.begin() + v2.size() / 2; ci != v2.end(); ++ci) // 不推荐
	v1.push_back(*ci);

v1.clear();
std::copy(v2.begin() + v2.size() / 2, v2.end(), std::back_inserter(v1)); // 效率不如assign

v1.clear();
v1.insert(v1.end(), v2.begin() + v2.size() / 2, v2.end()); // 对copy的调用可以被替换为利用区间的insert版

std::vector<int> v;
v.insert(v.begin(), data, data + numValues); // 推荐，使用区间成员函数insert

std::vector<int>::iterator insertLoc(v.begin());
for (int i = 0; i < numValues; ++i) {
	insertLoc = v.insert(insertLoc, data[i]); // 不推荐，使用单元素成员函数
	++insertLoc;
}
```

区间成员函数（assign、insert）: 它们像STL算法一样，使用两个迭代器参数来确定该成员操作所执行的区间。如果不使用区间成员函数就得写一个显示的循环。

所有通过利用插入迭代器(insert iterator)的方式(即利用inserter、back_inserter或front_inserter)来限定目标区间的copy调用，其实都可以(也应该)被替换为对区间成员函数的调用。

优先选择区间成员函数而不是其对应的单元素成员函数：区间成员函数写起来更容易，更能清楚地表达你的意图，而且它们表现出了更高的效率。

 
#### Item  6: 当心C++编译器最烦人的分析机制  
#### Item  7: 如果容器中包含了通过new操作创建的指针，切记在容器对象析构前将指针delete掉   

**如果容器中包含了通过new操作创建的指针，切记在容器对象析构前将指针delete掉**

使用指针的容器：用引用计数形式的智能指针对象(比如std::shared_ptr)代替指针，或者当容器被析构时手工删除其中的每个指针。

			      

#### Item  8: 切勿创建包含auto_ptr的容器对象   
#### Item  9: 慎重选择删除元素的方法  

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
	
(1).要删除容器中有特定值的所有对象：容器是vector, string或deque，使用erase-remove；容器是list，使用list::remove；容器是一个标准关联容器，使用erase成员函数。

(2).要删除容器中满足特定判别式(条件)的所有对象：容器是vector, string或deque，使用erase-remove_if习惯用法；容器是list，使用list::remove_if；容器是一个标准关联容器，使用remove_copy_if和swap，或者写一个循环来遍历容器中的元素，记住当把迭代器传给erase时，要对它进行后缀递增。

(3).要在循环内做某些(除了删除对象之外的)操作：容器是一个标准序列容器，则写一个循环来遍历容器中的元素，记住每次调用erase时，要用它的返回值更新迭代器；容器是一个标准关联容器，则写一个循环来遍历容器中的元素，记住当把迭代器传给erase时，要对迭代器做后缀递增。

 
#### Item  10: 了解分配子(allocator)的约定和限制   
#### Item  11: 理解自定义分配子的合理用法   
#### Item  12: 切勿对STL容器的线程安全性有不切实际的依赖   
 
### 2 vector和string 

#### Item  13: vector和string优先于动态分配的数组  

许多string实现在背后使用了引用计数技术，这种策略可以消除不必要的内存分配和不必要的字符拷贝；vector的实现不允许使用引用计数
 
#### Item  14: 使用reserve来避免不必要的重新分配  

reserve成员函数：能使你把重新分配的次数减少到最低限度，避免了重新分配和指针/迭代器/引用失效带来的开销。
 
#### Item  15: 注意string实现的多样性   

(1).string的值可能会被引用计数，默认情况下会使用引用计数，通常也提供了通过预处理宏关闭默认。

(2).string对象大小的范围可以是一个char*指针大小的1倍到7倍。

(3).创建一个新的字符串值可能需要零次、一次或两次动态分配内存。

(4).string对象可能共享，也可能不共享其大小和容量信息。

#### Item  16: 了解如何把vector和string数据传给旧的API   
#### Item  17: 使用”swap技巧”除去多余的容量   

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

shrink_to_fit：自适应宽度是指当未明白设定容器的宽度（或外边距设为auto）时，在特定的情况下容器的宽度会依据情况自行设定。而设定的结果往往并非我们想要的。   
C++11中增加了shrink_to_fit成员函数。  
swap技巧：vector或string进行shrink-to-fit操作；也可以用来清除一个容器，并使其容量变为该实现下的最下值。  

#### Item  18: 避免使用vector<bool>  

替代方案：deque<bool>，deque几乎提供了vector所提供的一切(没有reserve和capacity)；bitset，大小在编译时就确定了，不支持插入和删除元素。 
 
### 3 关联容器 

#### Item  19: 理解相等(equality)和等价(equivalence)的区别   

相等的概念是基于operator==的。

等价关系是以”在已排序的区间中对象值的相对顺序”为基础的。

标准关联容器是基于等价而不是相等。标准关联容器的使用者要为所使用的每个容器指定一个比较函数。

#### Item  20: 为包含指针的关联容器指定比较类型   
```cpp
struct DereferenceLess {
	template<typename PtrType>
	bool operator()(PtrType pT1, PtrType pT2) const
	{
		return *pT1 < *pT2;
	}
};
```

std::set<std::string*, DereferenceLess> ssp; 

包含指针的关联容器时，容器将会按照指针的值进行排序。几乎肯定要创建自己的函数子类作为该容器的比较类型(comparison type)。

包含智能指针或迭代器的容器，也要考虑为它指定一个比较类型

#### Item  21: 总是让比较函数在等值情况下返回false   

等的值从来不会有前后顺序关系，对于相等的值，比较函数应当始终返回false。对set和map确实是这样，对multiset和multimap也是这样。

关联容器排序的比较函数：它们所比较的对象定义一个”严格的弱序化”(strict weak ordering)，定义了”严格的弱序化”的函数必须对相同值的两个拷贝返回false。

#### Item  22: 切勿直接修改set或multiset中的键   
#### Item  23: 考虑用排序的vector替代关联容器   

查找操作几乎从不跟插入和删除操作混在一起”时，再考虑使用排序的vector而不是关联容器才是合理的

#### Item  24: 当效率很关键时尽量用map::insert代替map::operator   

```cpp
int test_item_24()
{
	std::map<int, std::string> m;
	m[1] = "xxx"; // m[1]是m.operator[](1)的缩写形式
	m.operator[](1) = "xxx";

	// m[1] = "xxx"; 在功能上等同于
	typedef std::map<int, std::string> IntStrMap;
	std::pair<IntStrMap::iterator, bool> result = m.insert(IntStrMap::value_type(1, std::string()));
```

map::operator[]的设计目的与众不同, 是为了提供”添加和更新”(add or update)的功能。map::operator[]返回一个引用。

当向映射表中添加元素时，要优先选用insert，而不是operator[]；当更新已经在映射表中的元素的值时，要优先选择operator[]。

#### Item  25: 熟悉非标准的哈希容器   

C++11中新增了四种关联容器，使用哈希函数组织的，无序的，即unordered_map（效率好）、unordered_multimap、unordered_set、unordered_multiset。

### 4 迭代器 

#### Item  26: 尽量使用iterator代替const_iterator，reverse_iterator和const_reverse_iterator   

对容器类container<T>而言，iterator类型的功效相当于T*，而const_iterator则相当于const T*。对一个iterator或者const_iterator进行递增则可以移动到容器中的下一个元素

#### Item  27: 使用distance和advance将容器的const_iterator转换成iterator  

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

std::distance用以取得两个迭代器(它们指向同一个容器)之间的距离；   
std::advance则用于将一个迭代器移动指定的距离。   

#### Item  28: 正确理解由reverse_iterator的base()成员函数所产生的iterator的用法  

```cpp
std::vector<int> v;
v.reserve(5);

for (int i = 1; i <= 5; ++i) v.push_back(i);

std::vector<int>::reverse_iterator ri = std::find(v.rbegin(), v.rend(), 3); // 使ri指向3
std::vector<int>::iterator i(ri.base());
fprintf(stdout, "%d\n", (*i)); // 4

for (int i = 1; i <= 5; ++i) v.push_back(i);
ri = std::find(v.rbegin(), v.rend(), 3);
v.erase((++ri).base());
```

reverse_iterator ri指定的位置上插入新元素，则只需在ri.base()位置处插入元素即可。对于插入操作而言，ri和ri.base()是等价的。   
reverse_iterator ri指定的位置上删除一个元素，则需要在ri.base()前面的位置上执行删除操作。对于删除操作而言，ri和ri.base()是不等价的。   


#### Item  29: 对于逐个字符的输入请考虑使用istreambuf_iterator  

### 5 算法 

#### Item  30: 确保目标区间足够大   
#### Item  31: 了解各种与排序有关的选择    

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
	
std::sort：对给定区间所有元素进行排序。   
std::stable_sort：对给定区间所有元素进行稳定排序，稳定排序算法能够维持相等元素的原有顺序。   
std::partial_sort：对给定区间所有元素进行部分排序。   
std::nth_element：用于排序一个区间，它使得位置n上的元素正好是全排序情况下的第n个元素   
std::partition：可以把所有满足某个特定条件的元素放在区间的前部   

#### Item  32: 如果确实需要删除元素，则需要在remove这一类算法之后调用erase   

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

注意这里是std的标准库

remove类算法：std::remove、remove_if和unique，不是真正意义上的删除，需要后面erase,因为从容器中删除元素的唯一方法是调用该容器的成员函数.    
std::list的remove成员函数:STL中唯一一个名为remove并且确实删除了容器中元素的函数,std::list::unique也会真正删除元素。  

#### Item  33: 对包含指针的容器使用remove这一类算法时要特别小心   

指向动态分配的对象的指针，避免使用remove和类似的算法。具有引用计数功能的智能指针，可以使用。

#### Item  34: 了解哪些算法要求使用排序的区间作为参数  

在include<algorithm>中

要求排序区间的STL算法：binaray_search、lower_bound、upper_bound、equal_range、set_union、set_intersection、set_difference、set_symmetric_difference、merge、inplace_merge、includes。

merge和inplace_merge：实现了合并和排序的联合操作：它们读入两个排序的区间，然后合并成一个新的排序区间

includes： 判断一个区间中的所有对象是否都在另一个区间中

#### Item  35: 通过mismatch或lexicographical_compare实现简单的忽略大小写字符串比较   
#### Item  36: 理解copy_if算法的正确实现 

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

#### Item  37: 使用accumulate或者for_each进行区间统计 

```cpp
// 计算一个区间中数值的乘积
std::vector<float> vf{ 1.f, 2.f, 3.f, 1.5f };
float product = std::accumulate(vf.cbegin(), vf.cend(), 1.f, std::multiplies<float>());
fprintf(stdout, "product: %f\n", product); // 9.000000

// 计算出一个区间中所有点的平均值
std::list<Point> lp{ { 1, 2 }, { 2, 3 }, { 3, 4 }, { 4, 5 } };
Point avg = std::for_each(lp.cbegin(), lp.cend(), PointAverage()).result();
```

std::accumulate两种形式：有两个迭代器和一个初始值，带一个初始值和一个任意的统计函数。直接返回所要的统计结果

std::for_each两个参数：一个是区间，另一个是函数,对区间中的每个元素都要调用这个函数。返回一个函数对象。
  
 
### 6 仿函数，仿函数类，函数等等 

#### Item  38: 遵循按值传递的原则来设计函数子类   
#### Item  39: 确保判别式是”纯函数”   
#### Item  40: 若一个类是函数子，则应使它可配接  
#### Item  41: 理解ptr_fun、men_fun和mem_fun_ref的来由 
#### Item  42: 确保less与operator<具有相同的语义   
 
### 7 用STL编程 

#### Item  43: 算法调用优先于手写的循环   
#### Item  44: 容器的成员函数优先于同名的算法   

标准的关联容器，选择成员函数而不选择对应的同名算法：获得对数时间的性能，使用map和multimap的时候，将很自然地只考虑元素的键部分

list容器：std调用了remove、remove_if或者unique算法之后，必须紧接着再调用erase；list量身定做的成员函数则无需任何对象拷贝，且实实在在地删除了元素。

std::sort算法与list的sort函数:list的迭代器是双向迭代器，而sort算法要求随机访问迭代器。

#### Item  45: 正确区分count、find、binary_search、lower_bound、upper_bound和equal_range   

区间是排序的：通过binary_search、lower_bound、upper_bound和equal_range，你可以获得更快的查找速度。

区间未排序： 局限于count、count_if、find以及find_if，线性时间的效率。

#### Item  46: 考虑使用函数对象而不是函数作为STL算法的参数   
#### Item  47: 避免产生”直写型”(write-only)的代码   
#### Item  48: 总是包含(#include)正确的头文件   

1 所有的标准STL容器都被声明在与之同名的头文件中：vector被声明在<vector>中，list被声明在<list>中，等等。但是<set>和<map>是个例外，<set>中声明了set和multiset，<map>中声明了map和multimap。
	
2 所有的算法都被声明在<algorithm>，4个算法：accumulate、inner_product、adjacent_difference和partial_sum，它们被声明在头文件<numeric>

3 特殊类型的迭代器，包括istream_iterator和istreambuf_iterator，被声明在<iterator>
	
4 标准的函数子(比如less<T>)和函数子配接器(比如not1、bind2nd)被声明在头文件<functional>	


#### Item  49: 学会分析与STL相关的编译器诊断信息   
#### Item  50: 熟悉与STL相关的Web站点   


## 4 Effective Modern C++

### 1 型别推导 

#### Item  1 理解模板型别推导(Understand template type deduction)   
#### Item  2 理解auto型别推导(Understand auto type deduction)  

```cpp
// 当某变量采用auto来声明时，auto就扮演了模板中的T这个角色，而变量的型别饰词则扮演的是ParamType的角色
auto x = 27; // x的型别饰词(type specifier)就是auto自身, x既非指针也非引用
const auto cx = x; // 型别饰词成了const auto, cx既非指针也非引用
const auto& rx = x; // 型别饰词又成了const auto&, rx是个引用，但不是万能引用

auto&& uref1 = x; // x的型别是int,且是左值，所以uref1的型别是int&
auto&& uref2 = cx; // cx的型别是const int, 且是左值，所以uref2的型别是const int&
auto&& uref3 = 27; // 27的型别是int,且是右值，所以uref3的型别是int&&
```

(1).在一般情况下，auto型别推导和模板型别推导是一模一样的，但是auto型别推导会假定用大括号括起的初始化表达式代表一个std::initializer_list，但模板型别推导却不会。

(2).在函数返回值或lambda式的形参中使用auto，意思是使用模板型别推导而非auto型别推导。


C++11提供的新类型，定义在<initializer_list>头文件中。有了initializer_list后，就可以直接像初始化数组一样：

```cpp
template< class T >
class initializer_list;

class Test {

private:
	static std::map<string, string> const nameToBirthday = {
	{"lisi", "18841011"},
	{"zhangsan", "18850123"},
	{"wangwu", "18870908"},
	{"zhaoliu", "18810316"},
	};
}
```	
	
#### Item  3 理解decltype(Understand decltype)模板   

decltype的主要用途大概就在于声明那些返回值型别依赖于形参型别的函数模板

1.绝大多数情况下，decltype会得出变量或表达式的型别而不作任何修改。  
2.对于型别为T的左值表达式，除非该表达式仅有一个名字，否则decltype总是得出型别T&(For lvalue expressions of type T other than names, decltype always reports a type of T&)。   
3.C++14支持decltype(auto)，和auto一样，它会从其初始化表达式出发来推导型别，但是它的型别推导使用的是decltype的规则。  

#### Item  4 掌握查看型别推导结果的方法(Know how to view deduced types)   

### 2 auto 

#### Item  5 优先选用auto，而非显式型别声明(Prefer auto to explicit type declarations)   

```cpp
std::vector<int> v{1, 2, 3};
unsigned sz1 = v.size(); // 不推荐,32位和64位windows上,unsigned均是32位，而在64位windows上，std::vector<int>::size_type则是64位
auto sz2 = v.size(); // 推荐，sz2's type is std::vector<int>::size_type
```

类模版std::function：是一种通用、多态的函数封装。std::function的实例可以对任何可以调用的目标实体进行存储、复制、和调用操作，这些目标实体包括普通函数、Lambda表达式、函数指针、以及其它函数对象等。

1.auto变量必须初始化，基本上对会导致兼容性和效率问题的型别不匹配现象免疫，还可以简化重构流程，通常也比显式指定型别要少打一些字
	
#### Item  6 当auto推导的型别不符合要求时，使用带显式型别的初始化物习惯用法(Use the explicitly typed initializer idiom when auto deduces undesired types)   

```cpp
float ep = calcEpsilon(); // 隐式转换 double-->float,这种写法难以表明"我故意降低了函数的返回值精度"
auto ep2 = static_cast<float>(calcEpsilon()); // 推荐
	
(1).”隐形”的代理型别可以导致auto根据初始化表达式推导出”错误的”型别。

(2).带显式型别的初始化物习惯用法强制auto推导出你想要的型别。

```


### 3 转向现代C++ 

#### Item  7 在创建对象时注意区分()和{}(Distinguish between()and{}when creating objects)   
#### Item  8 优先选用nullptr，而非0或NULL(Prefer nullptr to 0 and NULL) 

```cpp
f8(0); // calls f8(int), not f8(void*)
//f8(NULL); // might not compile, but typically calls f8(int), never calls f8(void*)
f8(nullptr); // calls f(void*) overload
```

nullptr的优点在于，不具备整型型别，也不具备指针型别(pointer type)。

nullptr的实际型别是std::nullptr_t，std::nullptr_t的定义被指定为nullptr的型别。型别std::nullptr_t可以隐式转换到所有的裸指针型别(raw pointer type)，这就是为何nullptr可以扮演所有型别指针的原因。

(1).相对于0或NULL，优先选用nullptr。(2).避免在整型和指针型别之间重载。
  
#### Item  9 优先选用别名声明using，而非typedef(Prefer alias declarations to typedefs)   

```cpp
typedef void (*FP1)(int, const std::string&);
using FP2 = void (*)(int, const std::string&);
template<typename T>
using MyAllocList1 = std::list<T/*, MyAlloc<T>*/>; 
```

(1).typedef不支持模板化，但别名声明支持。(2).别名模板可以让人免写”::type”后缀，并且在模板内，对于内嵌typedef的引用经常要求加上typename前缀。

#### Item  10 优先选用限定作用域的枚举型别，而非不限作用域的枚举型别(Prefer scoped enums to unscoped enums)   

```cpp
enum Color1 { black, white, red }; // 不限范围的(unscoped)枚举型别：black, white, red所在作用域和Color1相同
//auto white = false; // error, white already declared in this scope
Color1 c1 = black;

enum class Color2 { black2, white2, red2 }; // C++11, 限定作用域的(scoped)枚举型别:black2, white2, red2所在作用域被限定在Color2内
auto white2 = false; // 没问题，范围内并无其它"white2"
```

(1).C++98风格的枚举型别，现在称为不限范围的枚举型别。   
(2).限定作用域的枚举型别仅在枚举型别内可见。它们只能通过强制型别转换以转换至其它型别。   
(3).限定作用域的枚举型别和不限范围的枚举型别都支持底层型别指定。限定作用域的枚举型别的默认底层型别是int，而不限范围的枚举型别没有默认底层型别。   
(4).限定作用域的枚举型别总是可以进行前置声明，而不限范围的枚举型别却只有在指定了默认底层型别的前提下才可以进行前置声明。

#### Item  11 优先选用删除函数，而非private未定义函数(Prefer deleted functions to private undefined ones)   

```cpp
class Widget11 {
public:
	Widget11(const Widget11&) = delete;
	Widget11& operator=(const Widget11&) = delete;

	template<typename T>
	void processPointer(T* ptr) {}
};
```

使用”=delete。删除函数无法通过任何方法使用；删除函数会被声明为public，而非private。任何函数都能成为删除函数(any function may be deleted)

(1).优先选用删除函数(deleted function)，而非private未定义函数。(2).任何函数都可以deleted，包括非成员函数和模板具现(template instantiation)

#### Item  12 为意在改写的函数添加override声明(Declare overriding functions override)   

```cpp
class Base {
public:
	virtual void doWork() {} // 基类中的虚函数
};

class Derived : public Base {
public:
	virtual void doWork() override {} // 改写(override)了Base:doWork(“virtual”在这可写可不写)
};
```

override：显式地标明派生类中的函数是为了改写(override)基类版本：为其加上override声明

1.为意在改写的函数添加override声明。

2.成员函数引用饰词使得对于左值和右值对象(*this)的处理能够区分开来

#### Item  13 优先选用const_iterator，而非iterator(Prefer const_iterators to iterators)   

任何时候只要你需要一个迭代器而其指涉到的内容没有修改必要，你就应该使用const_iterator。

(1).优先选用const_iterator，而非iterator。(2).在最通用的代码中，优先选用非成员函数版本的begin、end和rbegin等，而非其成员函数版本。

#### Item  14 只要函数不会发射异常，就为其加上noexcept声明(Declare functions noexcept if they won’t emit exceptions)   
#### Item  15 只要有可能使用constexpr，就使用它(Use constexpr whenever possible)   

```cpp
constexpr Point15(double xVal = 0, double yVal = 0) noexcept : x(xVal), y(yVal) {}
constexpr double xValue() const noexcept { return x; }

constexpr Point15 midpoint(const Point15& p1, const Point15& p2) noexcept
{
	return { (p1.xValue() + p2.xValue()) / 2, (p1.yValue() + p2.yValue()) / 2}; // call constexpr member function
}
```

当constexpr应用于对象时，其实就是一个加强版的const。但应用于函数时，你既不能断定它是const，也不能假定其值在编译阶段就已知。

(1).constexpr对象都具备const属性，并由编译期已知的值完成初始化。    
(2).constexpr函数在调用时若传入的实参值是编译期已知的，则会产生编译期结果。   
(3).比起非constexpr对象或非constexpr函数而言，constexpr对象或constexpr函数可以用在一个作用域更广的语境中

#### Item  16 保证const成员函数的线程安全性(Make const member functions thread safe)   

```cpp
std::lock_guard<std::mutex> guard(m); // lock m
if (cacheValid) return cachedValue;
```

对于单个要求同步的变量或内存区域,使用std::atomic就足够了。但是如果有两个或更多个变量或内存区域需要作为一整个单位进行操作时，就要动用互斥量了。

std::mutex、std::atomic是个只移型别    
(1).保证const成员函数的线程安全性，除非可以确信它们不会用在并发语境中。    
(2).运用std::atomic型别的变量会比运用互斥量提供更好的性能，但前者仅适用对单个变量或内存区域的操作。   

#### Item  17 理解特种成员函数的生成机制(Understand special member function generation)   

(1).特种成员函数是指那些C++会自行生成的成员函数：默认构造函数、析构函数、拷贝操作、以及移动操作。   
(2).移动操作仅当类中未包含用户显式声明的拷贝操作、移动操作和析构函数时才生成。   
(3).拷贝构造函数仅当类中不包含用户显式声明的拷贝构造函数时才生成，如果该类声明了移动操作则拷贝构造函数将被删除。拷贝赋值运算符仅当类中不包含用户显式声明的拷贝赋值运算符才生成，如果该类声明了移动操作则拷贝赋值运算符将被删除。在已经存在显式声明的析构函数的条件下，生成拷贝操作已经成为了被废弃的行为。   
(4).成员函数模板在任何情况下都不会抑制特种成员函数的生成。   

### 4 智能指针 

#### Item  18 使用std::unique_ptr管理具备专属所有权的资源(Use std::unique_ptr for exclusive-ownership resource management)   

(1).std::unique_ptr是小巧、高速的、具备只移型别的智能指针，对托管资源实施专属所有权语义。(2).默认地，资源析构采用delete运算符来实现，但可以指定自定义删除器。有状态的删除器和采用函数指针实现的删除器会增加std::unique_ptr型别的对象尺寸。(3).将std::unique_ptr转换成std::shared_ptr是容易实现的。

#### Item  19 使用std::shared_ptr管理具备共享所有权的资源(Use std::shared_ptr for shared-ownership resource management)   

```cpp
std::unique_ptr<Widget19, decltype(loggingDel)> upw(new Widget19, loggingDel); // 析构器型别是智能指针型别的一部分
std::shared_ptr<Widget19> spw(new Widget19, loggingDel); // 析构器型别不是智能指针型别的一部分
```

(1).std::shared_ptr提供方便的手段，实现了任意资源在共享所有权语义下进行生命周期管理的垃圾回收。(2).与std::unique_ptr相比，std::shared_ptr的尺寸通常是裸指针尺寸的两倍，它还会带来控制块的开销，并要求原子化的引用计数操作。(3).默认的资源析构通过delete运算符进行，但同时也支持定制删除器。删除器的型别对std::shared_ptr的型别没有影响。(4).避免使用裸指针型别的变量来创建std::shared_ptr指针。

#### Item  20 对于类似std::shared_ptr但有可能空悬的指针使用std::weak_ptr(Use std::weak_ptr for std::shared_ptr-like pointers than can dangle)   
#### Item  21 优先选用std::make_unique和std::make_shared，而非直接使用new(Prefer std::make_unique and std::make_shared to direct use of new)   

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

make系列函数会把一个任意实参集合完美转发(perfect-forward)给动态分配内存的对象的构造函数，并返回一个指涉到该对象的智能指针。

(1).相比于直接使用new表达式，make系列函数消除了重复代码、改进了异常安全性，并且对于std::make_shared和std::allocate_shared而言，生成的目标代码会尺寸更小、速度更快。    
(2).不适用使用make系列函数的场景包括需要定制删除器，以及期望直接传递大括号初始化物。   
(3).对于std::shared_ptr，不建议使用make系列函数的额外场景包括：自定义内存管理的类；内存紧张的系统、非常大的对象、以及存在比指涉到相同对象的std::shared_ptr生存期更久的std::weak_ptr。  

#### Item  22 使用Pimpl习惯用法时，将特殊成员函数的定义放到实现文件中(When using the Pimpl Idiom, define special member functions in the implementation file)   

### 5 右值引用、移动语义和完美转发 

#### Item  23 理解std::move和std::forward(Understand std::move and std::forward)   

1.std::move实施的是无条件的向右值型别的强制型别转换。就其本身而言，它不会执行移动操作。   
2.仅当传入的实参被绑定到右值时，std::forward才针对该实参实施向右值型别的强制型别转换。   
3.在运行期，std::move和std::forward都不会做任何操作。   

#### Item  24 区分万能引用和右值引用(Distinguish universal references from rvalue references)   
#### Item  25 针对右值引用实施std::move，针对万能引用实施std::forward(Use std::move on rvalue references, std::forward on universal references)   
#### Item  26 避免依万能引用型别进行重载(Avoid overloading on universal references)   
#### Item  27 熟悉依万能引用型别进行重载的替代方案(Familiarize yourself with alternatives to overloading on universal references)   
#### Item  28 理解引用折叠(Understand reference collapsing)   
#### Item  29 假定移动操作不存在、成本高、未使用(Assume that move operations are not present, not cheap, and not used)   
#### Item  30 熟悉完美转发的失败情形(Familiarize yourself with prefect forwarding failure cases)   

### 6 lambda表达式 

#### Item  31 避免默认捕获模式(Avoid default capture modes)   
#### Item  32 使用初始化捕获将对象移入闭包(Use init capture to move objects into closures)   
#### Item  33 对auto&&型别的形参使用decltype，以std::forward之(Use decltype on auto&& parameters to std::forward them)   
#### Item  34 优先选用lambda式，而非std::bind(Prefer lambdas to std::bind)   

### 7 并发API 

#### Item  35 优先选用基于任务而非基于线程的程序设计(Prefer task-based programming to thread-based)   
#### Item  36 如果异步是必要的，则指定std::launch::async(Specify std::launch::async if asynchronicity is essential)   
#### Item  37 使std::thread型别对象在所有路径皆不可联结(Make std::threads unjoinable on all paths)   
#### Item  38 对变化多端的线程句柄析构函数行为保持关注(Be aware of varying thread handle destructor behavior)   
#### Item  39 考虑针对一次性事件通信使用以void为模板型别实参的期值(Consider void futures for one-shot event communication)   
#### Item  40 对并发使用std::atomic，对特种内存使用volatile(Use std::atomic for concurrency, volatile for special memory)   

std::atomic模板的实例(例如，std::atomic<int>, std::atomic<bool>和std::atomic<Widget*>等)提供的操作可以保证被其它线程视为原子的。一旦构造了一个std::atomic型别对象，针对它的操作就好像这些操作处于受互斥量保护的临界区域内一样，但是实际上这些操作通常会使用特殊的机器指令来实现，这些指令比使用互斥量来得更加高效。   
volatile的用处就是告诉编译器，正在处理的是特种内存(special memory)，它的意思是通知编译器”不要对在此内存上的操作做任何优化”。可能最常见的特种内存是用于内存映射I/O的内存。这种内存的位置实际上是用于与外部设备(例如，外部传感器、显示器、打印机和网络端口等)通信，而非用于读取或写入常规内存(即RAM)。编译器可以消除std::atomic型别上的冗余操作。std::atomic对于并发程序设计有用，但不能用于访问特种内存。volatile对于访问特种内存有用，但不能用于并发程序设计。由于std::atomic和volatile是用于不同目的，它们甚至可以一起使用。访问std::atomic型别对象通常比访问非std::atomic型别对象慢得多。   

要点速记：(1).std::atomic用于多线程访问的数据，且不用互斥量。它是编写并发软件的工具。(2).volatile用于读写操作不可以被优化掉的内存。它是在面对特种内存时使用的工具。   

### 8 微调 

#### Item 41 针对可复制的形参，在移动成本低并且一定会被复制的前提下，才考虑将其按值传递(Consider pass by value for copyable parameters that are cheap to move and always copied)    
#### Item 42 考虑置入而非插入(Consider emplacement instead of insertion)   

emplace_back可用于任何支持push_back的标准容器。相似地，所有支持push_front的标准容器也支持emplace_front。还有，任何支持插入(insert)操作(即除了std::forward_list和std::array以外的所有标准容器)都支持置入操作(emplace)。置入函数可避免临时对象的创建和析构，但插入函数就无法避免。   
(1).从原理上说，置入函数(emplacement function)应该有时比对应的插入函数高效，而且不应该有更低效的可能。   
(2).从实践上说，置入函数在以下几个前提成立时，极有可能会运行得更快：待添加的值是以构造而非赋值方式加入容器；传递的实参型别与容器保存的参数型别不同；容器不会因为重复值而拒绝待添加的值。   
(3).置入函数可能会执行类型转换，而插入函数会拒绝这些类型转换。   


			      
			      

