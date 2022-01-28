

### 1 代码风格： 命名、注释、格式 

类型、函数大驼峰，其他全部小驼峰

```cpp
class SomeType {        // 类型大驼峰 
public:
  int Fun(int a, int b); // 函数大驼峰，参数小驼峰
  {
	int blockCount;   // 局部变量小驼峰
	...
  }
  ...
private:
  int objCount;      // 类成员变量小驼峰
  ...
};
```

SomeType g_fileCount; // 全局变量g_前缀+小驼峰

**C++文件名和类名保持一致**

如果有一个类叫 DatabaseConnection，那么对应的文件名：

DatabaseConnection.h 、DatabaseConnection.cpp

或者 database_connection.h 、database_connection.cpp

注释: 注释符使用 \* *\或** **//**都可以，对应代码的上方或右边，与代码之间，至少留有1空格

大括号：推荐K&R风格

```c++
void CheckNegative(int x)

{
  if (x < 0) {
	 puts("Negative");
  } else {
	 NonNegative(x);
  }
}
```

### 2 函数、类、常量

**函数**

输入校验原则：同一个模块内校验一次就可以，跨模块调用需要校验

需要校验的边界：so（或者dll）之间、进程与进程之间

函数参数：

- 拷贝代价大的类型传const引用
- 要被移动的参数，应使用右值引用类型，并在函数内使用std::move
- 要被转发的参数使用“转发引用”类型，并在函数内使用std::forward

返回类型：

- 返回多个值时，优先使用返回类型为struct或std::tuple
- 不要使用std::move返回函数局部变量
- 当入参或返回值可能无效时，可以使用**T\*****、****std::****optional**或一个可以代表无效状态的对象


**类**

- 类的成员变量应该声明为private，常量可以声明为public
- 当成员变量可以独立进行变化时，可以使用struct，否则使用class

**构造函数**

构造后的对象应该是完整的、可用的。在允许使用异常的情况下，应当使用异常来报告错误，或者unique抛出异常

**单参数构造函数声明为explicit**

```C++
class Foo {
public:
   explicit Foo(const std::string& fileName)
  {
	file.open(fileName); // RAII会自动关闭文件
	if (file.fail()) {

​      // 抛出**异常**，使得Foo的构造失败
​      throw FooException("Cannot open file");
	 }

	// 使用**智能指针**保证释放
	resource = std::make_unique<Data>();    
	... // 这里的异常会抛出构造函数

  }
  ...
private:
  std::ifstream file;
  std::unique_ptr<Data> resource;

};
```

**类的成员变量必须显式初始化**

- 优先使用初始化列表或者类内初始化来初始化成员

```c++
class Message {

public:
  Message() {} // 所有成员都没有初始化
  Message(int id, int len) : msgID(id), msgLength(len) {} // msgBuffer指针没有初始化
  ...

private:
  unsigned int msgID;
  unsigned int msgLength;
  unsigned char* msgBuffer;
};

int main()
{
  Message a;
  Message b(1, 2);
  ...
  return 0;
}
```

**三之法则（Rule of three）**

若某个类需要用户定义的析构函数、拷贝构造函或拷贝赋值操作符，则它基本同时需要这三个函数。

```c++
class RuleOfThree {
public:
  ...
  RuleOfThree(const RuleOfThree& other);
  RuleOfThree& operator=(const RuleOfThree& other);
  ~RuleOfThree();
  ...

};
```

**五之法则（Rule of five）：**

如果定义了析构函数、拷贝构造函数或拷贝赋值操作符，会阻止移动构造函数和移动赋值操作符的隐式定义，所以任何想要移动语义的类必须声明全部五个特殊成员函数

```c++
class RuleOfFive {

public:

  ...
  RuleOfFive(const RuleOfFive& other);
  RuleOfFive& operator=(const RuleOfFive& other);
  RuleOfFive(RuleOfFive&& other) noexcept;  // **移动构造函数、移动赋值操作符，应该有****noexcept****声明**
  RuleOfFive& operator=(RuleOfFive&& other) noexcept; 
  ~RuleOfFive();
  ...

};
```

**多态基类中的特殊成员函数**

如果多态基类声明了拷贝构造/拷贝赋值操作符，可能会发生切片，所以经常会将基类中的拷贝构造/拷贝赋值操作符显式定义为delete，并将移动构造/移动赋值操作符也显式定义为delete

**重载与虚函数**

在重写虚函数时应明确指定override或final（如果派生类函数与基类函数原型不一致，则产生编译错误）

只有基类析构函数是虚函数时，才能保证通过多态调用的时候调用到派生类的析构函

禁止子类的虚函数定义与基类不同的默认参数

重载逗号操作符、&&操作符和||操作符会影响对象的求值顺序，本规范进展重载这些操作符

**常量**

尽量使用const语义

优先使用constexpr定义在编译时就可以被计算出的

对于实例化为const的对象，也只允许调用其const成员函数

对于指针和引用类型的参数，如果不需要修改其引用的对象，应使用const修饰，明确表明在函数内部不会修改这个参数

### 3 枚举、声明与定义 

**优先使用枚举类而不是普通枚举**
```cpp
// 枚举类会将枚举项包含在其内部作用域中，使用时需要显示指定作用域，如State::DEFAULT
enum class State { DEFAULT, RUNNING, STOP }; 
```

**保持作用域尽量小**

变量被使用时才声明并初始化，如果需要使用全局变量，建议将数据进行封装，使用者只通过接口修改数据。

**遵循单一定义原则**

- 不要在头文件中定义非内联函数和非内联变量
- 只能通过包含头文件的方式使用其他文件提供的接口和变量。对于多个编译单元可能使用到的类型、模板、内联函数、内联变量、const/constexpr变量，将其定义放在头文件中，然后通过#include在多个编译单元中使用，保证定义的一致性


**禁止逐位操作非trivially copyable对象**

- 拷贝不变的（trivially copyable），如果一个类型是拷贝不变的（trivially copyable），使用memcpy这种方式把它的数据从一个地方拷贝出来会得到相同的结果。     
- 满足trivially copyable的类型主要包括bool、char、int、float等算术类型、枚举类型、指针类型以及满足trivially copyable的类（一般来说该类中没有虚函数或虚基类；类的非静态成员变量满足trivially copyable）。

- 非trivially copyable对象中可能含有编译器生成的隐藏数据（如：虚表指针），如果逐位操作这些对象，可能会破坏对象的内部数据，造成严重问题。
- 应当避免逐位操作类对象，会破坏类的信息隐蔽和数据保护的作用

### 4 类型转换、整数运算 

- dynamic_cast：继承体系下行转换，如果这种强制转换不可避免，应优先使用dynamic_cast，而不是使用static_cast，除非转换的不是多态基类
- static_cast：和C风格转换相似可做值的强制转换，或者void到其他指针类型的转换
- reinterpret_cast：用于转换不相关的类型，强制将某个类型对象的内存重新解释成另一种类型，是一种不安全的转换，应尽量少用
- const_cast：用于移除对象的const属性，使对象变得可修改，这样会破坏数据的不变性，只有在明确不会修改对象时，可以使用

```c++
short small = 0;
long big = small;                                // 自动转换
char tiny = static_cast<char>(big);              // 小心使用，窄化转换
const char* imuutable = &tiny;                   // 自动转换
char* canChange = const_cast<char*>(imuutable);  // 危险操作，只应该在为了适配使用外部接口，不会修改被转换的对象时使用
int number = reinterpret_cast<int>(canChange);   // 危险操作，转换后的数值可能不在int范围内

Derived derived;
Base* base = &derived;                           // 自动转换（上行转换）
Derived* local = dynamic_cast<Derived*>(base);  // 下行转换，反应出设计问题
void* nakedPtr = local;                          // 自动转换
Unknown* unknown = static_cast<Unknown*>(nakedPtr); // 危险操作，小心使用
unsigned char* data = ...
Base* tmp = reinterpret_cast<Base*>(data);       // 危险操作，避免使用
```

**禁止使用std::move操作const对象**

- 整数溢出/回绕原理
- 有符号整数溢出
- 无符号整数回绕（部分书籍称之为反转）
- 判断极小也要判断极大

**只对无符号数uint32做位运算：**

**对精度低于int类型的无符号整数做位运算时，会先进行整数提升，再对提升后的整数做位运算，小心出现非预期的结果。**

### 5 表达式、语句 

-比较两个表达式时，左侧倾向于变化，右侧倾向于不变

对于单个bool类型变量的判断，应该避免多余的==或!=
```c++
if (value == MAX) { // 符合
  ...
}

bool isNegative = ...
if (isNegative) {   // 符合
  ...
}
```

**注意表达式中的副作用**
- 一些表达式的操作数在运行期不会被计算，也不会触发该操作数的副作用，如：sizeof、decltype等。

decltype(value++) data = 1;  // 不符合：value++没有发生自增

std::cout << value;

- 注意：Sizeof(array)     返回数组大小

**对于&&和||操作符的右侧操作数不要包含副作用**
```cpp
if (flag > 0 || value-- > 0) { // 不符合：value-- 存在副作用
 ...
}
```
使用static_assert时不生成目标代码，可以在编译期暴露问题

控制表达式的结果必须是布尔（或逻辑表达式）

**switch语句**

每个switch语句中要有default分支，保证在遗漏case标签处理时能够有一个缺省的处理行为。

如果switch语句中少于两个case，则更适合使用if语句。

分支下沉的情况，可以标识出来。

###  6 资源管理

**资源管理**

在传递数组参数时，不应单独传递指针，使用std:array或带len

```c++
// 符合：使用std:array作为参数
void Init(std::array<char, LEN>& array)
{
  // 使用数组大小作为判断条件
  for (size_t i = 0; i < array.size(); i++) {
​   ...
  }
}

// 符合：添加函数参数，传递数组大小
void Init(char array[], size_t len)
{
  // 使用传入的数组大小作为判断条件
  for (size_t i = 0; i < len; i++) {
​  ...
  }
}

```

**RAII**

resource acquisition is initialization。它可以用于避免手工资源管理的复杂性。

资源的获取和释放是成对操作（例如new/delete，fopen/fclose，lock/unlock 等），恰好能对应C++语言对称的构造函数和析构函数。

lock_guard是互斥体的一个包装器，利用RAII技术确保离开作用域时会自动调用unlock()解锁。

```c++
explicit lock_guard(_Mutex& _Mtx) : _MyMutex(_Mtx) { // construct and lock
​    _MyMutex.lock();
  }

  lock_guard(_Mutex& _Mtx, adopt_lock_t) : _MyMutex(_Mtx) {} 
  ~lock_guard() noexcept {
​    _MyMutex.unlock();

  }
```


**不要访问超出生命周期的对象**

禁止将局部变量的地址返回到其作用域以外。如果对象在其生命周期之外被引用，则程序会产生未定义行为。

当lambda会逃逸出函数外面时，禁止按引用捕获局部变量。如果按引用捕获了某个局部变量，并且将lambda传递到了函数的外部，或者传递给了其他线程，则会在生命周期之外使用那个捕获的局部变量。

[capture list] (parameter list) -> return type { function body }


**合理使用智能指针**

- 使用 shared_ptr<T> 来表达共享所有权
- 使用 unique_ptr<T> 来表达独占所有权。如果资源只有一个所有者，应使用unique_ptr<T>     而不是shared_ptr<T>
- 使用 weak_ptr<T> 来解除shared_ptr<T>循环依赖导致引用计数用于不为零的问题
- 使用 shared_ptr 或 unique_ptr     代替 auto_ptr。auto_ptr在C++11中已标识为deprecated，在C++17中已去除


- 使用 unique_ptr<T>     类型作为参数或返回值，代表这个资源的所有权的转移
- 使用 unique_ptr<T>& 类型作为参数，代表这个函数可能重置这个 unique_ptr 的指向
- 使用 shared_ptr<T>     类型作为参数或返回值，代表函数或者调用者是这个资源的其中一个拥有者
- 使用 shared_ptr<T>& 类型作为参数，代表这个函数可能重置这个 shared_ptr 的指向


**优先使用std::make_shared/std:make_unique创建指针指针**

- 使用 std::make_shared创建 std::shared_ptr，会一次性在堆上分配足够容纳控制块和管理对象的内存
- C++14开始增加了std::make _unique，提供与std::make_shared类似的方式构造unique_ptr 

**正确使用new/delete**

- 使用new操作符创建的对象，只能由delete操作符来销毁。
- 使用new[]操作符创建的对象数组，只能由delete[]操作符来销毁。
- 自定义操作符的时候，new和delete要配对定义，new[]和delete[]要配对定义。自定义了new，必须同时自定义对应的delete。
- 调用自定义的new/delete操作符要和调用内置的操作符时表现的行为一致。使用了std::nothrow参数的 new 操作符在内存分配失败时，应该返

回nullptr，而默认的new操作符不返回nullptr 。

```c++
struct SomeType {
  void* operator new(size_t size)
  {
​    ... // 自定义操作
  }

  void operator delete(void* ptr) noexcept
  {
​    ... // 自定义操作
  }

  void operator delete(void* ptr, size_t size) noexcept // C++14
  {
​    ... // 自定义操作
  }
};
```

**合理处理new操作的错误**

​**nothrow + 空指针检查**

默认的new操作符在内存分配失败时，会抛出std::bad_alloc异常，而使用了std::nothrow参数的new操作符在内存分配失败时，会返回nullptr。

```c++
void Fun()
{
  char* p = new(std::nothrow) char[BUFFER_SIZE];
  if (p == nullptr) {                          // 符合： 此处必须检查空指针
​    std::cout << "failed" << std::endl;
  }
  ...
}
```

当new作符以返回nullptr表示错误时，应当同对待malloc函数一样，对返回值做空指针检查。

在new失败抛会出异常的情况下，不要做空指针检查。因为抛出异常时不会执行到if语句以及下面的代码

不使用nothrow, 检查没有意义，执行不到

```c++
void Fun()
{
  char* p = new char[BUFFER_SIZE];    
  if (p == nullptr) {                        
​    std::cout << "failed" << std::endl;
  }
  ...
}
```


### 7 异常处理 

**异常仅用于处理错误**

异常处理的控制流通常比正常返回更慢，不是用于错误处理，不建议使用异常。

不要盲目catch，不能处理的异常要继续传播。

**构造异常的操作本身不应该产生其他异常**

**下面特定的函数不宜抛出异常：**

- 全局变量和线程局部变量的构造函数
- 析构函数
- delete操作符
- 移动构造函数
- 移动赋值操作符
- swap函数

**如果一个函数不应抛出异常，应使用noexcept关键字声明**

如果一个虚函数noexcept声明，那么重写（override）它的函数也必须是noexcept的（除非用=delete删除），因此需要考虑noexcept对接口的影响。

**异常的抛出与捕获**

只使用std::exception的派生类的实例作为异常对象

- 抛异常时，抛对象本身，而不是指向对象的指针。抛出指针，会使回收被抛出对象的责任不明确。

- 捕获异常时，以左值引用的形式捕获异常。

- catch的顺序应该是先捕获派生类（即更特殊的异常类型），后捕获基类（即更通用的异常类型）。

```c++
class BadDataException : public std::exception {
  ...
};

if (SomeFunction()) {
  throw BadDataException{"some reason"}; // 错误 例子： throw new BadDataException{"some reason"}; 
}

try {
  Process(data);
  ...

} catch (const BadDataException & e) {
  ...
} catch (const std::exception& e) {
	...
}
```

### 8 模板与泛型编程、并发与并行 

**留意由于模板而造成的代码膨胀**。

**留意不依赖模板参数的成员是否合理**。

模板类的所有成员（包括内部类）都应该对模板参数有依赖，否则考虑是否移到模板外边。

**并发与并行**

**优先使用C++标准库提供的线程并发、同步机制避免数据竞争**。

注意：volatile关键字对避免数据竞争没有任何作用

```c++
void ThreadFun()
{
  {
​    // 这里使用lock_guard避免修改data时产生数据竞争

​    const std::**lock**_**guard**<std::mutex> lock(dataMutex);
​    ++data;
  }
  Use(data);
}
```

注意对象的生命周期

**不要直接调用mutex的方法（lock_guard/unique_lock + mutex）**

std::mutex以及std::shared_mutex等锁对象提供lock()和unlock()方法来进行加锁和解锁。但是，直接调用这两个方法很容易造成未能正常解锁的问题。应该使用std::lock_guard、std::unique_lock等机制，确保以任何形式（正常执行完、中途break、continue、返回、抛异常等）离开作用域时都会自动调用unlock()解锁。

```c++
Data& GetData()
{
    std::lock_guard<std::mutex> lock(dataMutex);

    if (data != nullptr) {
        return *data;   // lock析构，自动unlock
    }
    // 如果下面抛异常（只要能被catch）也会自动unlock
    data = CreateData();    
    return *data;              // lock析构，自动unlock

}
```

**正确使用条件变量（lock_guard/unique_lock  +  mutex  +  cond变量）**

使用条件变量的wait方法时，必须外加条件判断，并在循环中等待。有两个原因：

- wait和notify发生的顺序不确定：如果一个线程在wait之前，其他线程已经调用了notify方法，那么wait的线程就会一直等待下去。

- 虚假唤醒（spurious wakeup）：指的是在wait的线程，即使在没有任何线程调用notify方法，也可能被随机地唤醒。

```c++
void Thread1()
{
    std::unique_lock<std::mutex> lock(dataMutex);

    // 如果lambda表达式返回true则wait方法直接返回；
    // 如果发生虚假唤醒，且lambda表达式返回false则继续等待
    dataReadyCond.wait(lock, [] { return dataReady; });
    Use(data); 
}

void Thread2()
{
    {
        std::lock_guard<std::mutex> lock(dataMutex);
        data = MakeData();
        dataReady = true;
    }
    dataReadyCond.notify_all();
}
bool dataReady = false;
```

### 9 预处理、头文件与源文件 

#if或#elif预处理指令中的常量表达式的求值应为布尔值，被求值前应确保其使用的标识符是有效的

**避免使用宏**

**在C++中，有许多方式来避免使用宏**：

- 用const或enum定义常量
- 用namespace避免名字冲突
- 用inline函数避免函数调用的开销
- 用template函数来处理多种类型
```cpp
constexpr int MAX_MSISDN_LEN = 20; // 符合：优先使用constexpr
// 符合：使用模板函数、内联函数代替函数式宏
template <typename T>
T Max(T a, T b)
{

  a < b ? b : a;
}
```


- 宏的问题：没有类型检查，运算符结合的时候，存在结合优先级
 
**头文件**

头文件 ————> include了就是全局

自引(号)他尖：项目组内部的头文件使用引号形式，对于标准库头文件、第三方库头文件、公司内部开发的平台组件或独立组件的头文件，使用尖括号形式

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


### 10 标准库 

**优先使用 C++标准库**

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

**使用std::string表示字符串**

优点：

- 不用考虑结尾的'\0'
- 可以直接使用+、=、<、>等操作符，并且提供丰富的字符串操作函数
- 不需要考虑内存分配操作，避免了显式的new/delete，以及由此导致的错误

问题：

- 将空指针传递给std::string构造函数，会解引用空指针，从而导致程序产生未定义行为
- std::string类型的**c_str**和**data**成员函数返回的指针，可能会随着修改对象而失效，在使用时需要确保指针的有效性

**使用有效的迭代器**

- 使用有效的迭代器和指向容器元素的指针与引用。如：std::**find**() 等算法可能返回无效位置；调用**erase**()、**insert**() 等方法修改容器时，已有的**迭代器也会失效**。
- 外部数据用于容器索引或迭代器时必须确保在有效范围内，需要进行合法性校验。

**erase-remove惯用法**

Erase-Remove 惯用法 : https://www.cnblogs.com/tianshihao/p/14378812.html
```cpp
vector<int> v;
v.erase(remove(v.begin(), v.end(), 99), v.end());
```
remove() 并不真正删除东西，因为它做不到。

remove() 移动指定区间中的所有元素直到所有“不删除的”元素在区间的开头（相对位置和原来的一样）。它返回一个指向最后一个“不删除的”元素的下一个元素的迭代器。返回值是区间的“新逻辑终点”。

**正确使用格式化输入/输出函数**

调用格式化输入/输出函数时，使用有效的格式字符串。并且格式串必须与对应的实参类型、数量做严格匹配。

使用%ld这里用来打印int64_t类型，

调用格式化输入/输出函数时，禁止format参数受外部数据控制。应使用常量字符串作为格式串。

**防止命令注入问题**

system()、popen()等函数会使用命令解析器去执行命令，如果这些函数的参数是外部可控的数据，会导致命令注入漏洞。

命令解析器支持命令分隔符, 管道符用于连续执行多个命令或程序。利用这个特性，当命令参数中含有外部精心构造的数据时，可以注入任意命令。

**禁止外部可控数据作为进程启动函数的参数或者作为dlopen/LoadLibrary等模块加载函数的参数**

- 可以使用POSIX的exec系列函数直接启动进程，但是不能使用shell启动。
- 若使用system()函数，则应使用硬编码的函数入参或对外部输入中的命令分隔符进行白名单过滤/转义。


**线程不安全函数/异步不安全函数：**

- C库中的线程不安全函数：strtok, ctime，putenv*，…
- C标准库函数中只有abort()，_Exit()，quick_exit()和signal()函数可以在信号处理程序中安全的调用；
- 在信号处理程序中不能含有new或delete操作，不能抛出异常，不能捕获异常，…

### 11 继承自C语言的编程规范条款

- 关于宏定义：完备的括号，不以分号结尾，多条语句的函数式宏放在do-while(0)中包装，不依赖宏外部的局部变量名


**变量**

- 避免使用大量栈分配，对于x86 和 x64计算机，默认堆栈大小为 1 MB。

- 当局部变量大小超过1k时，要审视合理性；较大的类型作为函数参数时，应使用引用或指针

```
constexpr int MAX_BUFF 0x1000000
//过大，单位换算byte-->KB--> MB
char buff[MAX_BUFF] = {0};
```

**控制语句**

- 循环必须安全退出，在退出前正确释放相关资源，避免资源泄漏。

**整数**

- 确保枚举常量映射到唯一值；如果类型的多个成员确实需要分配相同的值，应当提供显式的整数赋值。

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

断言永远不应用于验证是否存在运行时（与逻辑相对）错误，包括但不限于：

函数使用

**安全函数**

禁止使用realloc()函数

禁止使用alloca()函数申请栈上内存

必检查安全函数返回值，并进行正确的处理

正确设置安全函数中的destMax参数

禁止封装安全函数

禁止用宏重命名安全函数

只能使用华为安全函数库中的安全函数或经华为认可的其他安全函数

禁止直接使用外部数据拼接SQL命令

禁止使用内存操作类不安全函数

安全函数有： memcpy_s、memset_s、strcpy_s、sprintf_s、strcat_s、

都有destMax参数

```cpp
//destMax是dest的实际大小；
//count是实际要拷贝内存的大小；
errno_t memcpy_s(void *dest, size_t **destMax**, const void *src, size_t **count**);

如果count>destMax会返回失败；
```

destMax：安全函数拦截缓冲区写溢出的前提是正确设置destMax，否则达不到预期效果。

dest为数组的情况：

• destMax必须设置为 sizeof(dest) 或BUFF_SIZE*sizeof(elementType)。(elementType是数组元素的类型)

• 当数组元素为字节类型(如char, signed char, unsigned char)时，sizeof(elementType)为1，可以省略。

dest为动态分配的堆内存的情况：

• destMax设置为当初分配的内存大小，或剩余内存的大小。

dest为变长struct结构中的变长成员变量：

• destMax必须设置为该成员变量的实际大小，结构体中必须有描述变长部分长度的成员变量

```
#define BUFF_SIZE 100
unsigned char dest[BUFF_SIZE];
…

ret = memcpy_s(dest, sizeof(dest), src, srcLen);
…
ret = memcpy_s(dest, BUFF_SIZE, src, srcLen);
```

**内存中的敏感信息使用完毕后立即清0**

其他：

不要在信号处理函数中访问共享对象

禁用rand函数产生用于安全用途的伪随机数

禁止在发布版本中输出对象或函数的地址

禁止代码中包含公网地址

 
       
