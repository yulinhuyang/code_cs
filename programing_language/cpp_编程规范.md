
## 1  efficitive C++ 、more efficitive C++

### 混合使用C++和C 

名变换：C++支持重载，C不支持。要禁止名变换，使用C++的extern “C”。不要将extern “C”看作是声明这个函数是用C语言写的，应该看作是声明这个函数应该被当作好像C写的一样而进行调用。

静态初始化：在main执行前和执行后都有大量代码被执行。尤其是，静态的类对象和定义在全局的、命名空间中的或文件体中的类对象的构造函数通常在main被执行前就被调用。同样，通过静态初始化产生的对象也要在静态析构过程中调用其析构函数，这个过程通常发生在main结束运行之后。

动态内存分配：C++部分使用new和delete，C部分使用malloc(或其变形)和free。

同一程序下混合C++与C编程，记住下面的指导原则：

(1).确保C++和C编译器产生兼容的obj文件；

(2).将在两种语言下都使用的函数声明为extern “C”；

(3).只要可能，用C++写main()；

(4).总用delete释放new分配的内存；总用free释放malloc分配的内存；

(5).将在两种语言间传递的东西限制在用C编译的数据结构的范围内；这些结构的C++版本可以包含非虚成员函数


### Const使用 

**尽量以const, enum, inline替换#define**

宁可以编译器替换预处理器

(1). 对于单纯常量，最好以const对象或enum替换#define；(2). 对于形式函数的宏(macro)，最好改用inline函数替换#define。

prefer:

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

第一种是定义常量指针(constant pointers)，由于常量定义式通常被放在头文件内(以便被不同的源码含入)，因此有必要将指针(而不只是指针所指之物)声明为const。string对象通常比其前辈char*-base合宜。

第二种是class专属长量。让它成为class的一个成员(member),让它成为一个static成员.


**尽可能使用const**

如果关键字const出现在星号左边，表示被指物是常量；如果出现在星号右边，表示指针自身是常量；如果出现在星号两边，表示被指物和指针两者都是常量。

STL迭代器系以指针为根据塑模出来，所以迭代器的作用就像个T*指针。声明迭代器为const就像声明指针为const一样(即声明一个T* const指针)，表示这个迭代器不得指向不同的东西，但它所指的东西的值是可以改动
的。如果你希望迭代器所指的东西不可被改动(即希望STL模拟一个const T*指针)，你需要的是const_iterator。

请记住：(1).将某些东西声明为const可帮助编译器侦测出错误用法。const可被施加于任何作用域内的对象、函数参数、函数返回类型、成员函数本体。(2).编译器强制实施bitwise constness。(3).当const和non-const成员函数有着实质等价的实现时，令non-const版本调用const版本可避免代码重复




### 重载 

**令operator=返回一个reference to *this***

	Widget& operator+= (const Widget& rhs) // 这个协议适用于+=、-=、*=等等
	{
		return *this;
	}

**在operator=中处理”自我赋值”**

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


**不要重载”&&”, “||”,或”,”**
 
**通过重载避免隐式类型转换**
 
 每一个重载的operator必须带有一个用户定义类型(user-defined type)的参数,利用重载避免临时对象的方法不只是用在operator函数上。

**考虑用运算符的赋值形式(op=)取代其单独形式(op)**

确保operator的赋值形式(assignment version)(例如operator+=)与一个operator的单独形式(stand-alone)(例如operator+)之间存在正常的关系，一种好方法是后者(指operator+)根据前者(指operator+=)来实现。


### 智能指针 

**以对象管理资源**

在C++11中auto_ptr已经被废弃，用unique_ptr替代

unique_ptr 独占所指向的对象,shared_ptr(make_shared)允许多个指针指向同一个对象

(1).为防止资源泄漏，请使用RAII(Resource Acquisition Is Initialization,资源取得时机便是初始化时机)对象，它们在构造函数中获得资源并在析构函数中释放资源。(2).两个常被使用的RAII classes分别是stdn::shared_ptr和auto_ptr。前者通常是较佳选择，因为其copy行为比较直观。若选择auto_ptr，复制动作会使它(被复制物)指向null


以独立语句将newed对象置入智能指针：

	int test_item_17()
	{
		// 执行new Widget17; 调用priority; 调用std::shared_ptr构造函数，它们的执行顺序不确定
		processWidget(std::shared_ptr<Widget17>(new Widget17), priority()); // 可能泄露资源

		std::shared_ptr<Widget17> pw(new Widget17); // 在单独语句内以智能指针存储newed所得对象
		processWidget(pw, priority()); // 这个调用动作绝不至于造成泄露

		return 0;
	}

### new和delete 

**成对使用new和delete时要采用相同形式**

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


new(也就是通过new动态生成一个对象)，有两件事发生：第一，内存被分配出来(通过名为operator new的函数)；第二，针对此内存会有一个(或更多)构造函数被调用。

delete，也有两件事发生：针对此内存会有一个(或更多)析构函数被调用，然后内存才被释放(通过名为operator delete的函数)。

指针使用delete: 唯一能够让delete知道内存中是否存在一个”数组大小记录”的办法就是：由你来告诉它。如果你使用delete时加上中括号(方括号)，delete便认定指针指向一个数组，否则它便认定指针指向单一对象。

new表达式中使用[]，必须在相应的delete表达式中也使用[]。如果你在new表达式中不使用[]，一定不要在相应的delete表达式中使用[]。


**理解各种不同含义的new和delete**

new操作符(new operator)和new操作(operator new)的区别：

new操作符：1 分配足够的内存以便容纳所需类型的对象，2调用构造函数初始化内存中的对象

函数operator new：仅分配内存，不会调用构造函数

函数operator delete与delete操作符的关系与operator new与new操作符的关系一样

### 接口设计 

**让接口容易被正确使用，不易被误用** 

请记住：(1).好的接口很容易被正确使用，不容易被误用。你应该在你的所有接口中努力达成这些性质。

(2).”促进正确使用”的办法包括接口的一致性，以及与内置类型的行为兼容。

(3).”阻止误用”的办法包括建立新类型、限制类型上的操作，束缚对象值，以及消除客户的资源管理责任。

(4).std::shared_ptr支持定制型删除器(custom deleter)。这可防范DLL问题，可被用来自动解除互斥锁(mutex)等等。




### 值与引用 

**宁以pass-by-reference-to-const替换pass-by-value**
 
pass-by-value：内置类型，以及STL的迭代器和函数对象

pass-by-reference-to-const：尽量以pass-by-reference-to-const替换pass-by-value。前者通常比较高效，并可避免切割问题(slicing problem)。

*必须返回对象时，别妄想返回其reference*

绝不要返回pointer或reference指向一个local stack对象，或返回reference指向一个heap-allocated对象，或返回pointer或reference指向一个local static对象而有可能同时需要多个这样的对象。

### 指针与引用 

在任何情况下都不能使用指向空值的引用。一个引用必须总是指向某些对象。在C++里，引用应被初始化。不存在指向空值的引用这个事实意味着使用引用的代码效率比使用指针的要高。

### 引用计数 

引用计数是这样一个技巧，它允许多个有相同值的对象共享这个值的实现。

作用：第一个是简化跟踪堆中的对象的过程。节省内存，而且可以使得程序运行更快。


### 成员函数 

**宁以non-member、non-friend替换member函数，可以增加封装性、包裹弹性(packaging flexibility)和机能扩充性**

**若所有参数皆需类型转换，请为此采用non-member函数**

**尽可能延后变量定义式的出现时间**

尽可能延后变量定义式的出现。这样做可增加程序的清晰度并改善程序效率。

### casting 

const_cast: 通常被用来将对象的常量性移除(cast away the constness)。它也是唯一有此能力的C++-style转型操作符

dynamic_cast: 主要用来执行”安全向下转型”(safe downcasting)，也就是用来决定某对象是否归属继承体系中的某个类型，它不能被用来缺乏虚函数的类型上

reinterpret_cast:意图执行低级转型，实际动作(及结果)可能取决于编译器

static_cast: 用来强迫隐式转换(implicit conversions)，例如将non-const对象转为const对象，或将int转为double等等

如果可以，尽量避免转型，特别是在注重效率的代码中避免dynamic_cast。如果有个设计需要转型动作，试着发展无须转型的替代设计

### handles 

**避免返回handles指向对象内部成分**

reference、指针和迭代器统统都是所谓的handles(号码牌，用来取得某个对象)，而返回一个”代表对象内部数据”的handle，随之而来的便是”降低对象封装性”的风险。

请记住：避免返回handles(包括reference、指针、迭代器)指向对象内部。遵守这个条款可增加封装性，帮助const成员函数的行为像个const，并将发生”虚吊号码牌”(dangling handles)的可能性降至最低。


### inlining 

**透彻了解inlining的里里外外**
 
 inline void f() {} // 假设编译器有意愿inline“对f的调用”
 
	int test_item_30()
	{
		void (*pf)() = f; // pf指向f

		f(); // 这个调用将被inlined,因为它是一个正常调用
		pf(); // 这个调用或许不被inlined,因为它通过函数指针达成

		return 0;
	}

inline函数：将”对此函数的每一个调用”都以函数本体替换之

inlining在大多数C++程序中是编译期行为。

(1).将大多数inlining限制在小型、被频繁调用的函数身上。这可使日后的调试过程和二进制升级(binary upgradability)更容易，也可使潜在的代码膨胀问题最小化，使程序的速度提升机会最大化。

(2).不要只因为function templates出现在头文件，就将它们声明为inline。

### 文件依赖 

**将文件间的编译依存关系降至最低**

如果使用object references或object pointers可以完成任务，就不要使用objects。

以class声明式替换class定义式。

Handle classes：pointer to implementation，成员函数必须通过implementation pointer取得对象数据。

interface classes：特殊的abstract base class(抽象基类），由于每个函数都是virtual，所以你必须为每次函数调用付出一个间接跳跃(indirect jump)成本

(1). 支持”编译依存性最小化”的一般构想是：相依于声明式，不要相依于定义式。基于此构想的两个手段是handle classes和interface classes。

(2). 程序库头文件应该以”完全且仅有声明式”(full and declaration-only forms)的形式存在。这种做法不论是否涉及templates都适用


### 类与对象 

**确定你的public继承塑模出is-a关系**

public inheritance(公开继承)意味”is-a”(是一种)的关系

”public继承”意味is-a。适用于base classes身上的每一件事情一定也适用于derived classes身上，因为每一个derived class对象也都是一个base class对象

**避免遮挡继承而来的名称**

derived classes内的名称会遮掩base classes内的名称。在public继承下从来没有人希望如此。(2).为了让被遮掩的名称再见天日，可使用using声明式或转交函数(forwarding functions)。

**区分接口继承和实现继承**

pure virtual函数：为了让derived classes只继承函数接口。

简朴的(非纯)impure virtual函数：让derived classes继承该函数的接口和缺省实现。

non-virtual函数的：为了令derived classes继承函数的接口及一份强制性实现

(1).接口继承和实现继承不同。在public继承之下，derived classes总是继承base class的接口。(2). pure virtual函数只具体指定接口继承。(3). 简朴的(非纯)impure virtual函数具体指定接口继承及缺省实现继承。(4). non-virtual函数具体指定接口继承以及强制性实现继承。


**明智而审慎地使用private继承**

尽可能使用复合，必要时才使用private继承。

(1). private继承意味is-implemented-in-terms-of(根据某物实现出)。它通常比复合(composition)的级别低。但是当derived class需要访问protected base class的成员，或需要重新定义继承而来的virtual函数时，这么设计是合理的。

(2).和复合(composition)不同，private继承可以造成empty base最优化。这对致力于”对象尺寸最小化”的程序库开发者而言，可能很重要。


### 构造与析构 

**确定对象被使用前已先被初始化**

**使用析构函数防止资源泄漏**

**在构造函数中防止资源泄漏**

### virtual函数 

**考虑virtual函数以外的其它选择**

(1). virtual函数的替代方案包括non-virtual interface(NVI)手法及Strategy设计模式的多种形式。NVI手法自身是一个特殊形式的Template Method设计模式。

(2). 将机能从成员函数移到class外部函数，带来的一个缺点是，非成员函数无法访问class的non-public成员。

(3). std::function对象的行为就像一般函数指针。这样的对象可接纳”与给定之目标签名式(target signature)兼容”的所有可调用物(callable entities)。

**绝对不要重新定义继承而来的non-virtual函数。**

**绝不重新定义继承而来的缺省参数值**

virtual函数系动态绑定(dynamically bound)，而缺省参数值确是静态绑定(statically bound)。静态绑定又名前期绑定，early binding；动态绑定又名后期绑定，late binding

**通过复合塑模出has-a或”根据某物实现出”**

	class Address {};
	class PhoneNumber {};
	class Person {
	private:
		std::string name;        // 合成成分物(composed object)
		Address address;         // 同上
		PhoneNumber voiceNumber; // 同上
		PhoneNumber faxNumber;   // 同上
	};

(1). 复合(composition)的意义和public继承完全不同。

(2). 在应用域(application domain)，复合意味has-a(有一个)。在实现域(implementation domain)，复合意味is-implemented-in-terms-of(根据某物实现出)。


**理解虚拟函数、多继承、虚基类和RTTI所需的代码**

virtual table和virtual table pointers，通常被分别地称为vtbl和vptr。

虚函数表：一个vtbl通常是一个函数指针数组，每个类应该只有一个virtual table，类的vtbl的大小与类中声明的虚函数的数量成正比。类中vtbl的项目是指向虚函数实现体的指针。

虚函数是不能内联的。这是因为”内联”是指”在编译期间用被调用的函数体本身来代替函数调用的指令”，但是虚函数的”虚”是指”直到运行时才能知道要调用的是哪一个函数”。

RTTI(运行时类型识别): 在运行时找到对象和类的有关信息，所以肯定有某个地方存储了这些信息让我们查询。这些信息被存储在类型为type_info的对象里，你能通过使用typeid操作符访问一个类的type_info对象。


### 代理类 

Proxy类可以完成一些其它方法很难甚至可不能实现的行为：多维数组，左值/右值的区分，限制隐式类型转换。

### 模板编程 

**了解typename的双重意义**

typename必须作为嵌套从属类型名称的前缀词

(1).声明template参数时，前缀关键字class和typename可互换。

(2).请使用关键字typename标识嵌套从属类型名称；但不得在base class lists(基类列)或member initialization list(成员初值列)内以它作为base class修饰符。

**学习处理模板化基类内的名称**

其他模板编程待补充。。。

### 异常 

**理解”抛出一个异常”与”传递一个参数”或”调用一个虚函数”间的差异**

调用函数时，程序的控制权最终还会返回到函数的调用处，但是当你抛出一个异常时，控制权永远不会回到抛出异常的地方。

catch子句匹配顺序总是取决于它们在程序中出现的顺序。因此一个派生类异常可能被处理其基类异常的catch子句捕获。



### 系统优化 

**牢记80-20准则(80-20 rule)**

大约20%的代码使用了80%的程序资源；大约20%的代码耗用了大约80%的运行时间；大约20%的代码使用了80%的内存。

**考虑使用lazy evaluation(懒惰计算法)**

**分期摊还期望的计算**

over-eager evaluation(过度热情计算法)：如果你认为一个计算需要频繁进行，你就可以设计一个数据结构高效地处理这些计算需求，这样可以降低每次计算需求时的开销。

lazy evaluation：必须支持某些操作而不总需要其结果，以提高程序效率。

over-eager：必须支持某些操作而其结果几乎总是被需要或不止一次地需要时，以提高程序效率。

**理解临时对象的来源**

未命名的对象通常在两种条件下产生：为了使函数成功调用而进行隐式类型转换和函数返回对象时。

**考虑变更程序库**

**在未来时态下开发程序**

未来时态的考虑增加了你的代码的可重用性、可维护性、健壮性，以及在环境发生改变时易于修改。

## 2  efficitive  STL

### 容器与迭代器

**慎重选择容器类型**

标准STL序列容器：vector、string、deque、list、forward_list(C++11)、array(C++11)。

标准STL关联容器：set、multiset、map、multimap、unordered_set(C++11)、unordered_multiset(C++11)、unordered_map(C++11)、unordered_multimap(C++11)。

标准的非STL容器，包括：bitset(include <bitset>)、valarray(include <valarray>)。其它STL容器：stack(include <stack>)、queue(include <queue>)和priority_queue((include <queue>))。
	
连续内存容器：vector、string、deque

链表的容器：比如list、forward_list

事务语义:只有list对多个元素的插入操作提供了事务语义

**不要试图编写独立于容器类型的代码**

STL是以泛化(generalization)原则为基础的：数组被泛化为”以其包含的对象的类型为参数”的容器，函数被泛化为”以其使用的迭代器的类型为参数”的算法，指针被泛化为”以其指向的对象的类型为参数”的迭代器

迭代器： 前向迭代器、双向迭代器

序列容器：支持push_front和/或push _back操作

关联容器: 提供了对数时间的lower_bound、upper_bound和equal_range成员函数

标准的连续内存容器: 提供了随机访问迭代器

标准的基于节点的容器: 提供了双向迭代器

**确保容器中的对象拷贝正确而高效**

STL的工作方式（拷贝）：进去的是拷贝（insert或push_back），出来的也是拷贝（front或back）(copy in, copy out)。进一步被拷贝，插入或删除操作时，现有元素的位置通常会被移动(拷贝)；排序算法，next_permutation或previous_permutation, remove、unique或类似的操作，rotate或reverse,那么对象将会被移动(拷贝)

”剥离”问题意味着向基类对象的容器中插入派生类对象几乎总是错误的。使拷贝动作高效、正确，并防止剥离问题发生的一个简单办法是使容器包含指针而不是对象。

**调用empty而不是检查size()是否为0**

empty对所有的标准容器都是常数时间操作，而对一些list实现，size耗费线性时间。

**区间成员函数优先于与之对应的单元素成员函数**

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


区间成员函数（assign、insert）: 它们像STL算法一样，使用两个迭代器参数来确定该成员操作所执行的区间。如果不使用区间成员函数就得写一个显示的循环。

所有通过利用插入迭代器(insert iterator)的方式(即利用inserter、back_inserter或front_inserter)来限定目标区间的copy调用，其实都可以(也应该)被替换为对区间成员函数的调用。

优先选择区间成员函数而不是其对应的单元素成员函数：区间成员函数写起来更容易，更能清楚地表达你的意图，而且它们表现出了更高的效率。

### 容器中的指针

**如果容器中包含了通过new操作创建的指针，切记在容器对象析构前将指针delete掉**

使用指针的容器：用引用计数形式的智能指针对象(比如std::shared_ptr)代替指针，或者当容器被析构时手工删除其中的每个指针。

使用指针的容器：用引用计数形式的智能指针对象(比如std::shared_ptr)代替指针，或者当容器被析构时手工删除其中的每个指针。

**切勿创建包含auto_ptr的容器对象**

### 删除对象

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

(1).要删除容器中有特定值的所有对象：如果容器是vector, string或deque，则使用erase-remove习惯用法；如果容器是list，则使用list::remove；如果容器是一个标准关联容器，则使用它的erase成员函数。

(2).要删除容器中满足特定判别式(条件)的所有对象：如果容器是vector, string或deque，则使用erase-remove_if习惯用法；如果容器是list，则使用list::remove_if；如果容器是一个标准关联容器，则使用remove_copy_if和swap，或者写一个循环来遍历容器中的元素，记住当把迭代器传给erase时，要对它进行后缀递增。

(3).要在循环内做某些(除了删除对象之外的)操作：如果容器是一个标准序列容器，则写一个循环来遍历容器中的元素，记住每次调用erase时，要用它的返回值更新迭代器；如果容器是一个标准关联容器，则写一个循环来遍历容器中的元素，记住当把迭代器传给erase时，要对迭代器做后缀递增。









