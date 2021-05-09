

effective modern C++



## 4 efficitive modern C++

### 型别推导

***理解模板型别推导***

模板待补充

***理解auto型别推导**

	// 当某变量采用auto来声明时，auto就扮演了模板中的T这个角色，而变量的型别饰词则扮演的是ParamType的角色
	auto x = 27; // x的型别饰词(type specifier)就是auto自身, x既非指针也非引用
	const auto cx = x; // 型别饰词成了const auto, cx既非指针也非引用
	const auto& rx = x; // 型别饰词又成了const auto&, rx是个引用，但不是万能引用
 
	auto&& uref1 = x; // x的型别是int,且是左值，所以uref1的型别是int&
	auto&& uref2 = cx; // cx的型别是const int, 且是左值，所以uref2的型别是const int&
	auto&& uref3 = 27; // 27的型别是int,且是右值，所以uref3的型别是int&&


(1).在一般情况下，auto型别推导和模板型别推导是一模一样的，但是auto型别推导会假定用大括号括起的初始化表达式代表一个std::initializer_list，但模板型别推导却不会。

(2).在函数返回值或lambda式的形参中使用auto，意思是使用模板型别推导而非auto型别推导。


C++11提供的新类型，定义在<initializer_list>头文件中。有了initializer_list后，就可以直接像初始化数组一样：

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

**理解decltype(Understand decltype)模板**

decltype的主要用途大概就在于声明那些返回值型别依赖于形参型别的函数模板

1.绝大多数情况下，decltype会得出变量或表达式的型别而不作任何修改。

2.对于型别为T的左值表达式，除非该表达式仅有一个名字，否则decltype总是得出型别T&(For lvalue expressions of type T other than names, decltype always reports a type of T&)。

3.C++14支持decltype(auto)，和auto一样，它会从其初始化表达式出发来推导型别，但是它的型别推导使用的是decltype的规则。

**优先选用auto，而非显式型别声明**

	std::vector<int> v{1, 2, 3};
	unsigned sz1 = v.size(); // 不推荐,32位和64位windows上,unsigned均是32位，而在64位windows上，std::vector<int>::size_type则是64位
	auto sz2 = v.size(); // 推荐，sz2's type is std::vector<int>::size_type
	

类模版std::function：是一种通用、多态的函数封装。std::function的实例可以对任何可以调用的目标实体进行存储、复制、和调用操作，这些目标实体包括普通函数、Lambda表达式、函数指针、以及其它函数对象等。

1.auto变量必须初始化，基本上对会导致兼容性和效率问题的型别不匹配现象免疫，还可以简化重构流程，通常也比显式指定型别要少打一些字

**当auto推导的型别不符合要求时，使用带显式型别的初始化物习惯用法(Use the explicitly typed initializer idiom when auto deduces undesired types)**

	float ep = calcEpsilon(); // 隐式转换 double-->float,这种写法难以表明"我故意降低了函数的返回值精度"
	auto ep2 = static_cast<float>(calcEpsilon()); // 推荐
	
(1).”隐形”的代理型别可以导致auto根据初始化表达式推导出”错误的”型别。

(2).带显式型别的初始化物习惯用法强制auto推导出你想要的型别。

### 枚举、别名

**优先选用nullptr，而非0或NULL(Prefer nullptr to 0 and NULL)**

	f8(0); // calls f8(int), not f8(void*)
	//f8(NULL); // might not compile, but typically calls f8(int), never calls f8(void*)
	f8(nullptr); // calls f(void*) overload
	
nullptr的优点在于，不具备整型型别，也不具备指针型别(pointer type)。

nullptr的实际型别是std::nullptr_t，std::nullptr_t的定义被指定为nullptr的型别。型别std::nullptr_t可以隐式转换到所有的裸指针型别(raw pointer type)，这就是为何nullptr可以扮演所有型别指针的原因。

(1).相对于0或NULL，优先选用nullptr。(2).避免在整型和指针型别之间重载。

 **优先选用别名声明(using)，而非typedef**
 
	typedef void (*FP1)(int, const std::string&);
	using FP2 = void (*)(int, const std::string&);
	template<typename T>
	using MyAllocList1 = std::list<T/*, MyAlloc<T>*/>; 

(1).typedef不支持模板化，但别名声明支持。(2).别名模板可以让人免写”::type”后缀，并且在模板内，对于内嵌typedef的引用经常要求加上typename前缀。

**优先选用限定作用域的枚举型别，而非不限作用域的枚举型别(Prefer scoped enums to unscoped enums)**


	enum Color1 { black, white, red }; // 不限范围的(unscoped)枚举型别：black, white, red所在作用域和Color1相同
	//auto white = false; // error, white already declared in this scope
	Color1 c1 = black;
 
	enum class Color2 { black2, white2, red2 }; // C++11, 限定作用域的(scoped)枚举型别:black2, white2, red2所在作用域被限定在Color2内
	auto white2 = false; // 没问题，范围内并无其它"white2"


(1).C++98风格的枚举型别，现在称为不限范围的枚举型别。

(2).限定作用域的枚举型别仅在枚举型别内可见。它们只能通过强制型别转换以转换至其它型别。

(3).限定作用域的枚举型别和不限范围的枚举型别都支持底层型别指定。限定作用域的枚举型别的默认底层型别是int，而不限范围的枚举型别没有默认底层型别。

(4).限定作用域的枚举型别总是可以进行前置声明，而不限范围的枚举型别却只有在指定了默认底层型别的前提下才可以进行前置声明。

### 对象创建

**优先选用删除函数，而非private未定义函数(Prefer deleted functions to private undefined ones)**

	class Widget11 {
	public:
		Widget11(const Widget11&) = delete;
		Widget11& operator=(const Widget11&) = delete;

		template<typename T>
		void processPointer(T* ptr) {}
	};


使用”=delete。删除函数无法通过任何方法使用；删除函数会被声明为public，而非private。任何函数都能成为删除函数(any function may be deleted)

(1).优先选用删除函数(deleted function)，而非private未定义函数。(2).任何函数都可以deleted，包括非成员函数和模板具现(template instantiation)

**为意在改写的函数添加override声明**

	class Base {
	public:
		virtual void doWork() {} // 基类中的虚函数
	};

	class Derived : public Base {
	public:
		virtual void doWork() override {} // 改写(override)了Base:doWork(“virtual”在这可写可不写)
	};

override：显式地标明派生类中的函数是为了改写(override)基类版本：为其加上override声明

1.为意在改写的函数添加override声明。

2.成员函数引用饰词使得对于左值和右值对象(*this)的处理能够区分开来

### const相关

**优先选用const_iterator，而非iterator(Prefer const_iterators to iterators)**

任何时候只要你需要一个迭代器而其指涉到的内容没有修改必要，你就应该使用const_iterator。

(1).优先选用const_iterator，而非iterator。(2).在最通用的代码中，优先选用非成员函数版本的begin、end和rbegin等，而非其成员函数版本。

**只要有可能使用constexpr，就使用它(Use constexpr whenever possible)**


	constexpr Point15(double xVal = 0, double yVal = 0) noexcept : x(xVal), y(yVal) {}
	constexpr double xValue() const noexcept { return x; }

	constexpr Point15 midpoint(const Point15& p1, const Point15& p2) noexcept
	{
		return { (p1.xValue() + p2.xValue()) / 2, (p1.yValue() + p2.yValue()) / 2}; // call constexpr member function
	}

当constexpr应用于对象时，其实就是一个加强版的const。但应用于函数时，你既不能断定它是const，也不能假定其值在编译阶段就已知。

(1).constexpr对象都具备const属性，并由编译期已知的值完成初始化。

(2).constexpr函数在调用时若传入的实参值是编译期已知的，则会产生编译期结果。

(3).比起非constexpr对象或非constexpr函数而言，constexpr对象或constexpr函数可以用在一个作用域更广的语境中

**保证const成员函数的线程安全性**

		std::lock_guard<std::mutex> guard(m); // lock m
		if (cacheValid) return cachedValue;


对于单个要求同步的变量或内存区域,使用std::atomic就足够了。但是如果有两个或更多个变量或内存区域需要作为一整个单位进行操作时，就要动用互斥量了。

std::mutex、std::atomic是个只移型别

(1).保证const成员函数的线程安全性，除非可以确信它们不会用在并发语境中。

(2).运用std::atomic型别的变量会比运用互斥量提供更好的性能，但前者仅适用对单个变量或内存区域的操作。

### 其他

**理解特种成员函数的生成机制**

(1).特种成员函数是指那些C++会自行生成的成员函数：默认构造函数、析构函数、拷贝操作、以及移动操作。

(2).移动操作仅当类中未包含用户显式声明的拷贝操作、移动操作和析构函数时才生成。

(3).拷贝构造函数仅当类中不包含用户显式声明的拷贝构造函数时才生成，如果该类声明了移动操作则拷贝构造函数将被删除。拷贝赋值运算符仅当类中不包含用户显式声明的拷贝赋值运算符才生成，如果该类声明了移动操作则拷贝赋值运算符将被删除。在已经存在显式声明的析构函数的条件下，生成拷贝操作已经成为了被废弃的行为。

(4).成员函数模板在任何情况下都不会抑制特种成员函数的生成。

### 智能指针

**使用std::unique_ptr管理具备专属所有权的资源**

(1).std::unique_ptr是小巧、高速的、具备只移型别的智能指针，对托管资源实施专属所有权语义。(2).默认地，资源析构采用delete运算符来实现，但可以指定自定义删除器。有状态的删除器和采用函数指针实现的删除器会增加std::unique_ptr型别的对象尺寸。(3).将std::unique_ptr转换成std::shared_ptr是容易实现的。

**使用std::shared_ptr管理具备共享所有权的资源**

	std::unique_ptr<Widget19, decltype(loggingDel)> upw(new Widget19, loggingDel); // 析构器型别是智能指针型别的一部分
	std::shared_ptr<Widget19> spw(new Widget19, loggingDel); // 析构器型别不是智能指针型别的一部分


(1).std::shared_ptr提供方便的手段，实现了任意资源在共享所有权语义下进行生命周期管理的垃圾回收。(2).与std::unique_ptr相比，std::shared_ptr的尺寸通常是裸指针尺寸的两倍，它还会带来控制块的开销，并要求原子化的引用计数操作。(3).默认的资源析构通过delete运算符进行，但同时也支持定制删除器。删除器的型别对std::shared_ptr的型别没有影响。(4).避免使用裸指针型别的变量来创建std::shared_ptr指针。

**优先选用std::make_unique和std::make_shared，而非直接使用new(Prefer std::make_unique and std::make_shared to direct use of new)**

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

make系列函数会把一个任意实参集合完美转发(perfect-forward)给动态分配内存的对象的构造函数，并返回一个指涉到该对象的智能指针。

(1).相比于直接使用new表达式，make系列函数消除了重复代码、改进了异常安全性，并且对于std::make_shared和std::allocate_shared而言，生成的目标代码会尺寸更小、速度更快。

(2).不适用使用make系列函数的场景包括需要定制删除器，以及期望直接传递大括号初始化物。

(3).对于std::shared_ptr，不建议使用make系列函数的额外场景包括：自定义内存管理的类；内存紧张的系统、非常大的对象、以及存在比指涉到相同对象的std::shared_ptr生存期更久的std::weak_ptr。

### 引用与万能引用

**理解std::move和std::forward**

1).std::move实施的是无条件的向右值型别的强制型别转换。就其本身而言，它不会执行移动操作。

2).仅当传入的实参被绑定到右值时，std::forward才针对该实参实施向右值型别的强制型别转换。

(3).在运行期，std::move和std::forward都不会做任何操作。



### 多线程

##### 40 对并发使用std::atomic，对特种内存使用volatile(Use std::atomic for concurrency, volatile for special memory)

std::atomic模板的实例(例如，std::atomic<int>, std::atomic<bool>和std::atomic<Widget*>等)提供的操作可以保证被其它线程视为原子的。一旦构造了一个std::atomic型别对象，针对它的操作就好像这些操作处于受互斥量保护的临界区域内一样，但是实际上这些操作通常会使用特殊的机器指令来实现，这些指令比使用互斥量来得更加高效。

volatile的用处就是告诉编译器，正在处理的是特种内存(special memory)，它的意思是通知编译器”不要对在此内存上的操作做任何优化”。可能最常见的特种内存是用于内存映射I/O的内存。这种内存的位置实际上是用于与外部设备(例如，外部传感器、显示器、打印机和网络端口等)通信，而非用于读取或写入常规内存(即RAM)。编译器可以消除std::atomic型别上的冗余操作。std::atomic对于并发程序设计有用，但不能用于访问特种内存。volatile对于访问特种内存有用，但不能用于并发程序设计。由于std::atomic和volatile是用于不同目的，它们甚至可以一起使用。访问std::atomic型别对象通常比访问非std::atomic型别对象慢得多。

要点速记：(1).std::atomic用于多线程访问的数据，且不用互斥量。它是编写并发软件的工具。(2).volatile用于读写操作不可以被优化掉的内存。它是在面对特种内存时使用的工具。

#### 42 考虑置入而非插入(Consider emplacement instead of insertion)

emplace_back可用于任何支持push_back的标准容器。相似地，所有支持push_front的标准容器也支持emplace_front。还有，任何支持插入(insert)操作(即除了std::forward_list和std::array以外的所有标准容器)都支持置入操作(emplace)。置入函数可避免临时对象的创建和析构，但插入函数就无法避免。

(1).从原理上说，置入函数(emplacement function)应该有时比对应的插入函数高效，而且不应该有更低效的可能。

(2).从实践上说，置入函数在以下几个前提成立时，极有可能会运行得更快：待添加的值是以构造而非赋值方式加入容器；传递的实参型别与容器保存的参数型别不同；容器不会因为重复值而拒绝待添加的值。

(3).置入函数可能会执行类型转换，而插入函数会拒绝这些类型转换。
 
