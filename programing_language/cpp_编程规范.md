
## 1  efficitive C++ 

**Const使用**

*尽量以const, enum, inline替换#define*

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


*尽可能使用const*

如果关键字const出现在星号左边，表示被指物是常量；如果出现在星号右边，表示指针自身是常量；如果出现在星号两边，表示被指物和指针两者都是常量。

STL迭代器系以指针为根据塑模出来，所以迭代器的作用就像个T*指针。声明迭代器为const就像声明指针为const一样(即声明一个T* const指针)，表示这个迭代器不得指向不同的东西，但它所指的东西的值是可以改动
的。如果你希望迭代器所指的东西不可被改动(即希望STL模拟一个const T*指针)，你需要的是const_iterator。

请记住：(1).将某些东西声明为const可帮助编译器侦测出错误用法。const可被施加于任何作用域内的对象、函数参数、函数返回类型、成员函数本体。(2).编译器强制实施bitwise constness。(3).当const和non-const成员函数有着实质等价的实现时，令non-const版本调用const版本可避免代码重复

**构造与析构**

*确定对象被使用前已先被初始化*


**重载**

*令operator=返回一个reference to *this*

	Widget& operator+= (const Widget& rhs) // 这个协议适用于+=、-=、*=等等
	{
		return *this;
	}

*在operator=中处理”自我赋值”*

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


**智能指针**

*以对象管理资源*

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

**new和delete**

*成对使用new和delete时要采用相同形式*

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


**接口设计**

让接口容易被正确使用，不易被误用 

请记住：(1).好的接口很容易被正确使用，不容易被误用。你应该在你的所有接口中努力达成这些性质。(2).”促进正确使用”的办法包括接口的一致性，以及与内置类型的行为兼容。(3).”阻止误用”的办法包括建立新类型、限制类型上的操作，束缚对象值，以及消除客户的资源管理责任。(4).std::shared_ptr支持定制型删除器(custom deleter)。这可防范DLL问题，可被用来自动解除互斥锁(mutex)等等。


**值与引用**

 *宁以pass-by-reference-to-const替换pass-by-value*
 
pass-by-value：内置类型，以及STL的迭代器和函数对象

pass-by-reference-to-const：尽量以pass-by-reference-to-const替换pass-by-value。前者通常比较高效，并可避免切割问题(slicing problem)。

*必须返回对象时，别妄想返回其reference*

绝不要返回pointer或reference指向一个local stack对象，或返回reference指向一个heap-allocated对象，或返回pointer或reference指向一个local static对象而有可能同时需要多个这样的对象。

**成员函数**

*宁以non-member、non-friend替换member函数，可以增加封装性、包裹弹性(packaging flexibility)和机能扩充性*

*若所有参数皆需类型转换，请为此采用non-member函数*

*尽可能延后变量定义式的出现时间*

尽可能延后变量定义式的出现。这样做可增加程序的清晰度并改善程序效率。

**casting**

const_cast: 通常被用来将对象的常量性移除(cast away the constness)。它也是唯一有此能力的C++-style转型操作符

dynamic_cast: 主要用来执行”安全向下转型”(safe downcasting)，也就是用来决定某对象是否归属继承体系中的某个类型

reinterpret_cast:意图执行低级转型，实际动作(及结果)可能取决于编译器

static_cast: 用来强迫隐式转换(implicit conversions)，例如将non-const对象转为const对象，或将int转为double等等

如果可以，尽量避免转型，特别是在注重效率的代码中避免dynamic_cast。如果有个设计需要转型动作，试着发展无须转型的替代设计

**handles**

*避免返回handles指向对象内部成分*

reference、指针和迭代器统统都是所谓的handles(号码牌，用来取得某个对象)，而返回一个”代表对象内部数据”的handle，随之而来的便是”降低对象封装性”的风险。

请记住：避免返回handles(包括reference、指针、迭代器)指向对象内部。遵守这个条款可增加封装性，帮助const成员函数的行为像个const，并将发生”虚吊号码牌”(dangling handles)的可能性降至最低。


**inlining**

 *透彻了解inlining的里里外外*
 
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

**文件依赖**

*将文件间的编译依存关系降至最低*

如果使用object references或object pointers可以完成任务，就不要使用objects。

以class声明式替换class定义式。

Handle classes：pointer to implementation，成员函数必须通过implementation pointer取得对象数据。

interface classes：特殊的abstract base class(抽象基类），由于每个函数都是virtual，所以你必须为每次函数调用付出一个间接跳跃(indirect jump)成本

(1). 支持”编译依存性最小化”的一般构想是：相依于声明式，不要相依于定义式。基于此构想的两个手段是handle classes和interface classes。

(2). 程序库头文件应该以”完全且仅有声明式”(full and declaration-only forms)的形式存在。这种做法不论是否涉及templates都适用


**类与对象*



