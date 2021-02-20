
1  efficitive C++ 

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




