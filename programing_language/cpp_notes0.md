
C++ primer plus 笔记整理

主要参考: 

https://zhuanlan.zhihu.com/p/343271809

https://dinghmcn.github.io/post/c++primerplus/


### 第一章 开始 

#### **1.1 编写一个简单的C++程序**

```cpp
int main()
{
    return 0;
}
```

每个C++程序都包含一个或多个函数，其中一个必须命名为main.

#### **1.2 初识输入输出**

| 对象 | 用途                   |
| ---- | ---------------------- |
| cin  | 标准输入               |
| cout | 标准输出               |
| cerr | 标准错误               |
| clog | 输出运行时的一般性消息 |

#### **1.3 注释简介**

两种：

单行注释： //

界定符：/* 和 */

#### **1.4 控制流**

while;for;if;

### **第二章 变量和基本类型**

P30-P71

数据类型是程序的基础。C++语言支持广泛的数据类型。

#### **基本内置类型**

#### **算术类型**

| 类型 | 最小尺寸 |
| :--- | :--- |
| bool | 未定义 |
| char | 8位 |
| w_char_t | 16位 |
| char16_t | 16位 |
| char32_t | 32位 |
| short | 16位 |
| int | 16位 |
| long | 32位 |
| long long | 64位 |
| float | 6位有效数字 |
| double | 10位有效数字 |
| long double | 10位有效数字 |

#### **类型转换**

不要混用符号类型和无符号类型。

#### **变量**

#### **变量定义**

（1）基本形式：

类型说明符，随后紧跟着一个或者多个变量名组成的列表，其中变量名以逗号分隔，最后以分号结束。

（2）初始值

在C++中，初始化和赋值是2个完全不同的操作。初始化的含义是创建变量的时候赋予一个初始值，而赋值的含义是把对象的当前值擦除，用一个新值来替代。两者区别很小。

（3）列表初始化

用花括号来初始化变量的方式，称为列表初始化。

（4）默认初始化

如果定义变量没有指定初始值，则变量被默认初始化。

::: tip

例外情况：

定义在函数体内部的内置类型变量将不被初始化，其值未定义。

建议初始化每个内置类型的变量。 :::

#### **变量声明和定义的关系**

变量声明：规定了变量的类型和名字。

变量定义：除声明之外，还需要申请存储空间。

如果想声明一个变量，而非定义它，需要使用extern关键词。

```cpp
extern int i;    // 声明i而非定义i
int j;           // 声明并定义j
```

::: tip 变量只能被定义一次，但可以被多次声明。 :::

#### **名字的作用域**

作用域：C++中大多数作用域都用花括号分隔。

作用域中一旦声明了某个名字，它所嵌套的所有作用域都能访问该名字。同时，允许在内层作用域中重新定义外层作用域中有的名字。

::: warning 如果函数有可能用到某全局变量，则不宜再定义一个同名的局部变量。 :::

#### **复合类型**

> 定义: 复合类型是基于其他类型定义的类型。

#### **引用**

引用：为对象起另外一个名字。

::: warning 引用必须被初始化。 引用本身不是对象，所以不能定义引用的引用。 引用要和绑定的对象严格匹配。 引用类型的初始值，必须是一个对象。 :::

#### **指针**

指针：本身就是一个对象。允许对指针赋值和拷贝。指针无须在定义的时候赋值。

（1）利用指针访问对象

如果指针指向了一个对象，则允许使用解引用符（*）来访问该对象。

（2）void* 指针

#### **理解复合类型的声明**

（1）指向指针的指针

** 表示指向指针的指针

*** 表示指向指针的指针的指针

（2）指向指针的引用

不能定义指向引用的指针。但指针是对象，所以存在对指针的引用。

#### **const限定符**

定义：const用于定义一个变量，它的值不能被改变。const对象必须初始化。

::: tip

默认状态下，const对象仅在文件内有效。当多个文件出现了同名的const变量时，等同于在不同文件中分别定义了独立的变量。

如果想让const变量在文件间共享，则使用extern修饰。

:::

（1）const的引用

允许为一个常量引用绑定非常量的对象、字面值，甚至是个一般表达式。

一般，引用的类型必须与其所引用对象的类型一致，特殊情况是表达式。

（2）指针和const

弄清楚类型，可以从右边往左边阅读。

（3）顶层const

top-level const 表示指针本身是个常量

low-level const表示指针所指的对象是一个常量。

（4）constexpr和常量表达式

C++新标准规定，允许将变量声明为constexpr类型以便由编译器来验证变量的值是否是一个常量表达式。

#### **处理类型**

#### **类型别名**

两种方法用于定义类型别名：

（1）使用关键词typedef

```cpp
typedef double wages; //wages是double的同义词
typedef wages *p; // p是double*的同义词
```

（2）别名声明

```cpp
using SI = Sales_item;  // SI是Sales_item的同义词
```

auto类型说明符：让编译器通过初始值来推算变量的类型。

decltype类型指示符：选择并返回操作符的数据类型。只得到类型，不实际计算表达式的值。

#### **自定义数据结构**

（1）类

数据结构是把一组相关的数据元素组织起来，然后使用它们的策略和方法。

类一般不定义在函数体内，为了确保各个文件中类的定义一致，类通常被定义在头文件中，而且类所在头文件的名字应该与类的名字一样。

头文件通常包含那些被定义一次的实体。

（2）预处理器

```cpp
#ifndef SALES_DATA_H
#define SALES_DATA_H
#endif
```

一般把预处理变量的名字全部大写。

#### **术语**

**空指针** ：值为0的指针，空指针合法但是不指向任何对象。nullPtr是表示空指针的字面值常量。

**void\***：可以指向任意非常量的指针类型，不能执行解引用操作。


### **第三章 字符串、向量和数组**

P74-P118

string表示可变长的字符序列，vector存放的是某种给定类型对象的可变长序列。

#### **命名空间的 using 声明**

```cpp
using namespace:name;
```

头文件不应包含using声明。

#### **标准库类型 string**

```text
#include <string>
using namespace std;
```

（1）定义和初始化

```text
string s1;
sting s2(s1);
string s3("value");
string s3 = "value";
string s4(n, 'c');
```

（2）string对象的操作

```text
s.empty();      // 判空
s.size();       // 字符个数
s[n];           // s中第n个字符的引用
s1+s2;          // s1和s2连接
<,<=,>,>=       // 比较
```

::: warning

标准局允许把字面值和字符串字面值转换成string对象。字面值和string是不同的类型。

:::

(3)处理string对象中的字符

::: tip C++程序的头文件应该使用cname，而不应该使用name.h的形式 :::

遍历给定序列中的每个值执行某种操作

```cpp
for (declaration : expression)
    statement
```

#### **标准库类型 vector**

标准库vector表示对象的集合，其中所有对象的类型都相同。

vector是一个类模板，而不是类型。

（1）定义和初始化vector对象

```cpp
vector<T> v1;
vector<T> v2(v1);
vector<T> v2 = v1;
vector<T> v3(n, val);
vector<T> v4(n);
vector<T> v5{a,b,c...}
vecrot<T> v5={a,b,c...}
```

如果用圆括号，那么提供的值是用来构造vector对象的。

如果用花括号，则是使用列表初始化该vector对象。

（2）向vector对象添加元素

先定义一个空的vector对象，在运行的时候使用push_back向其中添加具体指。

（3）其他vector操作

```cpp
v.empty();
v.size();
v.push_back(t);
v[n];
```

::: warning

只能对确认已存在的元素执行下标操作。

:::

#### **迭代器介绍**

迭代器运算符

```cpp
*iter            // 解引用，返回引用
iter->mem        // 等价于  (*iter).mem
++iter
--iter
iter1 == iter2
iter1 != iter2
iter + n
iter - n
iter += n
iter -= n
iter1 - iter2     // 两个迭代器相减的结果是它们之间的距离
>, >=, <, <=      // 位置比较
```

::: warning

凡是使用了迭代器的循环体，都不能向迭代器所属的容器添加元素。

:::

#### **数组**

（1）数组、指针

使用数组下标的时候，通常将其定义为size_t类型。

::: warning

定义数组必须指定数组的类型，不允许用auto推断。

不存在引用的数组。

如果两个指针分别指向不相关的对象，则不能进行对这2个指针进行比较。

:::

#### **多维数组**

多维数组实际上是数组的数组。

```text
size_t cnt = 0;
for(auto &row : a)
	for (auto &col : row){
		col = cnt;
		++cnt;
	}
int *ip[4];    // 整型指针的数组
int (*ip)[4];  // 指向含有4个整数的数组
```

#### **术语**

**begin** string和vector的成员，返回指向第一个元素的迭代器。也是一个标准库函数，输入一个数组，返回指向该数组首元素的指针。

**end** string和vector的成员，返回一个尾后迭代器。也是一个标准库函数，输入一个数组，返回指向该数组尾元素的下一个位置的指针。


### **第四章 表达式**

P120-P151

#### **4.1 基础**

**重载运算符**：为已经存在的运算符赋予了另外一层含义。

**左值、右值**：

当一个对象用作右值得时候，用的是对象的值（内容）。

当对象被用作左值得时候，用的是对象的身份（在内存中的位置）。

#### **4.2 算术运算符**

**%**：参与取余运算的运算对象必须是整数类型。

#### **4.3 逻辑和关系运算符**

| 运算符 |
| :----:|
| ! |
| < |
| <= |
| > |
| >= |
| == |
| != |
| && |
| || |

&& 运算符和 || 运算符都是先求左侧运算对象的值再求右侧运算对象的值。

::: warning

进行比较运算的时候，除非比较的对象是bool类型，否则不要使用布尔字面值true,false作为运算对象。

:::

#### **4.4 赋值运算符**

赋值运算符满足右结合律。

不要混淆相等运算符和赋值运算符

```cpp
if (i = j)

if (i == j)
```

#### **4.5 递增和递减运算符**

递增运算符 ++

递减运算符 --

#### **4.6 成员访问运算符**

点运算符和箭头运算符

```cpp
n = (*p).size();
n = p->size();
```

#### **4.7 条件运算符**

```cpp
condition ? expression1 : expression2;
```

#### **4.8 位运算符**

| 运算符 | 功能 | 用法 | 备注 |
| :----- | ------ | -------------- | :--------------------------------------------- |
| ~ | 位求反 | ~expr | 1置为0，0置为1 |
| << | 左移 | expr << expr2 | 在右侧插入值位0的二进制位 |
| >> | 右移 | expr1 >> expr2 | |
| & | 位与 | expr1 & expr2 | 对应位置都是1，则结果1，否则为0。 |
| ^ | 位异或 | expr1 ^ expr2 | 对应位置有且只有1个为1，则结果是1，否则为0。|
| | | 位或 | expr1 | expr2 | 对应位置至少有1个位1，则结果是1，否则为0。 |

#### **4.9 sizeof运算符**

sizeof运算符返回一条表达式或一个类型名字所占的字节数，其所得值是一个size_t类型，是一个常量表达式。

```cpp
sizeof (type)
sizeof expr
```

#### **4.10 逗号运算符**

逗号运算符含有两个运算对象，按照从左向右的顺序依次求值。

#### **4.11 类型转换**

**隐式转换**

**显式转换**

命名的强制类型转换

```cpp
cast-name<type>(expression)

// cast-name是static_cast,dynamic_cast,const_cast,reinterpret_cast
```

::: tip

由于强制类型转换干扰了正常的类型检查，因此建议避免强制类型转换。

:::

#### **4.12 运算符优先级表**


### **第五章 语句**

P154-P178

#### **5.1 简单语句**

（1）空语句

```cpp
;    // 空语句
```

（2）复合语句

复合语句是指用花括号括起来的（可能为空的）语句和声明的序列，复合语句也被称作块（block）。

```cpp
{}
```

#### **5.2 语句作用域**

定义在控制结构当中的变量只在相应语句的内部可见，一旦语句结束，变量就超出其作用范围。

#### **5.3 条件语句**

（1）if 语句

（2）switch 语句

case关键字和它对应的值一起被称为case标签。

case标签必须是整形常量表达式。

如果某个case标签匹配成功，将从该标签开始往后顺序执行所有case分支，除非程序显示的中断了这一过程。

dedault 标签：如果没有任何一个case标签能匹配上switch表达式的值，程序将执行紧跟在default标签后面的语句。

#### **5.4 迭代语句**

（1）while 语句

```text
while (condition)
			statement
```

（2）传统 for 语句

```cpp
for (initializar; condition; expression)
		statement
```

for 语句中定义的对象只在for循环体内可见。

（3）范围 for 语句

```cpp
for (declaration : expression)
		statement
```

（4）do while 语句

```cpp
do 
	statement
while (condition)
```

#### **5.5 跳转语句**

**break**

break只能出现在迭代语句或者switch语句内部。仅限于终止离它最近的语句，然后从这些语句之后的第一条语句开始执行。

**continue**

continue语句终止最近的循环中的当前迭代并立即开始下一次迭代。

**goto**

goto的作用是从goto语句无条件跳转到同一函数内的另一条语句。

容易造成控制流混乱，应禁止使用。

**return**

#### **5.6 try语句块和异常处理**

C++中异常处理包括：throw表达式、try语句块。

try和catch，将一段可能抛出异常的语句序列括在花括号里构成try语句块。catch子句负责处理代码抛出的异常。

throw表达式语句，终止函数的执行。抛出一个异常，并把控制权转移到能处理该异常的最近的catch字句。

### **第六章 函数**

P182-P225

#### **6.1 函数基础**

（1）形参和实参：

实参的类型必须与对应的形参类型匹配。

函数的调用规定实参数量应与形参数量一致。

（2）局部对象

形参和参数体内部定义的变量统称为**局部变量**，它们对函数而言是"局部"的，仅在函数的作用域内可见，同时局部变量还会隐藏外层作用域中同名的其他变量。

**自动对象**：只存在于块执行期间的对象。

局部静态对象：在程序的执行路径第一次经过对象定义语句时候进行初始化，并且直到程序终止才会被销毁。

```cpp
size_t count_calls()
{
	static size_t ctr = 0;
	return ++ctr;
}
```

（3）函数声明

函数的三要素：（返回类型、函数名、形参类型）。

函数可被声明多次，但只能被定义一次。

（4）分离式编译

分离式编译允许把程序分割到几个文件中去，每个文件独立编译。

编译->链接

#### **6.2 参数传递**

当形参是引用类型，这时它对应的实参被引用传递或者函数被传引用调用。

当实参被拷贝给形参，这样的实参被值传递或者函数被传值调用。

（1）传值参数

（2）被引用传参

（3）const形参和实参

（4）数组形参

为函数传递一个数组时，实际上传递的是指向数组首元素的指针。

```cpp
void print(const int*);
void pring(const int[]);
void print(const int[10]);
// 以上三个函数等价
```

数组引用实参： f(int (&arr)[10])

```cpp
int *matrix[10];   // 10个指针构成的数组
int (*matrix)[10]; // 指向含有10个整数的数组的指针
```

（5）含有可变形参的数组

initializer_list

```cpp
for err_msg(initializer_list<string> li)
```

#### **6.3 返回类型和return语句**

2种：无返回值函数和右返回值函数。

```cpp
return;
return expression;
```

函数完成后，它所占用的存储空间也会随着被释放掉。

::: warning

返回局部对象的引用是错误的；返回局部对象的指针也是错误的。

:::

#### **6.4 函数重载**

重载函数：同一作用域内的几个函数名字相同但形参列表不通，我们称之为重载函数。（overloaded）。

不允许2个函数除了返回类型外其他所有的要素都相同。

**重载与作用域**

如果在内存作用域中声明名字，它将隐藏外层作用域中声明的同名实体。

#### **6.5 特殊用途语言特性**

（1）默认实参

函数调用时，实参按其位置解析，默认实参负责填补函数调用缺少的尾部实参。

```cpp
typedef string::size_type sz;
string screen(sz ht = 24, sz wid = 80, char background = ' ');
```

::: tip

当设计含有默认实参的函数时，需要合理设置形参的顺序。一旦某个形参被赋予了默认值，它后面的所有形参都必须有默认值。

:::

（2）内联函数

使用关键词inline来声明内联函数。

内联用于优化规模较小，流程直接，频繁调用的函数。

（3）constexpr函数

constexpr函数是指能用于常量表达式的函数。

#### **6.6 函数匹配**

Step1:确定候选函数和可选函数。

Step2:寻找最佳匹配。

#### **6.7 函数指针**

函数指针指向的是函数而非对象。

```cpp
void useBigger (const string &s1, const string &s2, bool pf(const string &, const string &));
等价于
void useBigger (const string &s1, const string &s2, bool (*pf)(const string &, const string &));
```


### **第七章 类**

P228-P273

类的基本思想是数据抽象和封装。

抽象是一种依赖于接口和实现分离的编程技术。

封装实现了类的接口和实现的分离。

#### **7.1 定义抽象数据类型**

（1）this

任何对类成员的直接访问都被看作this的隐式引用。

```cpp
std::string isbn() const {return bookNo;}
```

等价于

```cpp
std::string isbn() const {return this->bookNo;}
```

（2）在类的外部定义成员函数

类外部定义的成员的名字必须包含它所属的类名。

```cpp
double Sales_data::avg_price() const {
	if (units_sol)
		return revenue/units_sols;
	else
		return 0;
}
```

（3）构造函数

> 定义：类通过一个或几个特殊的成员函数来控制其对象的初始化过程，这些函数叫做构造函数。
> 构造函数没有返回类型；
> 构造函数的名字和类名相同。

类通过一个特殊的构造函数来控制默认初始化过程，这个函数叫做**默认构造函数**。

编译器创建的构造函数被称为**合成的默认构造函数**。

::: tip

只有当类没有声明任何构造函数的时，编译器才会自动的生成默认构造函数。

一旦我们定义了一些其他的构造函数，除非我们再定义一个默认的构造函数，否则类将没有默认构造函数

:::

#### **7.2 访问控制与封装**

（1）访问控制

| 说明符 | 用途 |
| ------- | ------------------------------------------------------------ |
| public | 使用public定义的成员，在整个程序内可被访问，public成员定义类的接口。 |
| private | 使用private定义的成员可以被类的成员函数访问，但是不能被使用该类的代码访问，private部分封装了类的实现细节。 |

（2）友元

类可以允许其他类或者函数访问它的非公有成员，方法是令其他类或者函数成为它的友元。

以friend关键字标识。

友元不是类的成员，不受访问控制级别的约束。

::: tip

友元的声明仅仅制定了访问的权限，而非通常意义的函数声明。必须在友元之外再专门对函数进行一次声明。

:::

```cpp
// Sales_data.h

class Sales_data {
friend Sales_data add(const Sales_data&, const Sales_data&);
friend std::ostream &print(std::ostream&, const Sales_data&);
friend std::istream &read(std::istream&, Sales_data&);
}

// nonmember Sales_data interface functions
Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, Sales_data&);

//Sales_data.cpp

Sales_data 
add(const Sales_data &lhs, const Sales_data &rhs)
{
	Sales_data sum = lhs;  // copy data members from lhs into sum
	sum.combine(rhs);      // add data members from rhs into sum
	return sum;
}

// transactions contain ISBN, number of copies sold, and sales price
istream&
read(istream &is, Sales_data &item)
{
	double price = 0;
	is >> item.bookNo >> item.units_sold >> price;
	item.revenue = price * item.units_sold;
	return is;
}

ostream&
print(ostream &os, const Sales_data &item)
{
	os << item.isbn() << " " << item.units_sold << " " 
	   << item.revenue << " " << item.avg_price();
	return os;
}
```

#### **7.3 类的其他特性**

（1）重载成员变量

```cpp
Screen myScrren;
char ch = myScreen.get();
ch = myScreen.get(0,0);
```

（2）类数据成员的初始化

类内初始值必须使用=或者{}的初始化形式。

```cpp
class Window_mgr{
private:
    std::vector<Screen> screens{Screen(24, 80, ' ')};
}
```

（3）基于const的重载

```cpp
class Screen {
public:
	// display overloaded on whether the object is const or not
    Screen &display(std::ostream &os) 
                  { do_display(os); return *this; }
    const Screen &display(std::ostream &os) const
                  { do_display(os); return *this; }
}
```

当某个对象调用display的时候，该对象是否是const决定了应该调用display的哪个版本。

（3）类类型

对于一个类来说，在我们创建他的对象之前该类必须被定义过，而不能仅被声明。

（4）友元

*友元类*

如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。

```cpp
class Screen {
	// Window_mgr的成员可以访问Screen类的私有部分
	friend class Window_mgr;
}
```

*令成员函数作为友元*

```text
class Screen {
	// Window_mgr::clear必须在Screen类之前被声明
	friend void Window_mgr::clear(ScreenIndex);
}
```

#### **7.4 类的作用域**

一个类就是一个作用域。

#### **7.5 构造函数再探**

（1）构造函数的初始值有时必不可少

::: tip

如果成员是const、引用，或者属于某种未提供默认构造函数的类类型化。我们必须通过构造函数初始值列表为这些成员提供初值。

:::

```cpp
class ConstRef{
public:
	ConstRef (int i);
private:
	int i;
	const int ci;
	int &ri;
};

ConstRef:ConstRef(int ii) : i(ii), ci(ii), ri(i){  }
```

（2）成员初始化的顺序

成员初始化的顺序与它们在类定义中出现 的顺序一致。P259

（3）委托构造函数

使用它所述类的其他构造函数执行它自己的初始化过程。

（4）如果去抑制构造函数定义的隐式转换？

在类内声明构造函数的时候使用explicit关键字。

#### **7.6 类的静态成员**

（1）声明静态成员

在成员的声明之前加上关键词static。

类的静态成员存在于任何对象之外，对象中不包含任何与静态成员有关的数据。

（2）使用类的静态成员

```cpp
double r;
r = Account::rate();
```

#### **小结**

类有两项基本能力：

一是数据数据抽象，即定义数据成员和函数成员的能力；

二是封装，即保护类的成员不被随意访问的能力。





### **第八章 IO库**

P278-P290

C++语言不直接处理输入输出，而是通过一组定义在标准库中的类型来处理IO。

- iostream处理控制台IO
- fstream处理命名文件IO
- stringstream完成内存string的IO

ifstream和istringstream继承自istream

ofstream和ostringstream继承自ostream

#### **8.1 IO类**

（1）IO对象无拷贝或复制。

进行IO操作的函数通常以引用方式传递和返回流。

（2）刷新输出缓冲区

flush刷新缓冲区，但不输出任何额外的字符；

ends向缓冲区插入一个空字符，然后刷新缓冲区。

#### **8.2 文件输入输出**

| 类 | 作用 |
|  ---- |  ----  |
| ifstream | 从一个给定文件读取数据 |
| ofstream | 从一个给定文件写入数据 |
| fstream | 读写给定文件 |

#### **8.3 string流**

| 类 | 作用 |
| ------------- | -------------------------------------- |
| istringstream | 从string读取数据 |
| ostringstream | 向string写入数据 |
| stringstream | 既可从string读数据也可以向string写数据 |

```cpp
	// will hold a line and word from input, respectively
	string line, word;

	// will hold all the records from the input
	vector<PersonInfo> people;

	// read the input a line at a time until end-of-file (or other error)
	while (getline(is, line)) {       
		PersonInfo info;            // object to hold this record's data
	    istringstream record(line); // bind record to the line we just read
		record >> info.name;        // read the name
	    while (record >> word)      // read the phone numbers 
			info.phones.push_back(word);  // and store them
		people.push_back(info); // append this record to people
	}
```



```cpp
	// for each entry in people
	for (vector<PersonInfo>::const_iterator entry = people.begin();
				entry != people.end(); ++entry) {    
		ostringstream formatted, badNums; // objects created on each loop

		// for each number
		for (vector<string>::const_iterator nums = entry->phones.begin();
				nums != entry->phones.end(); ++nums) {  
			if (!valid(*nums)) {           
				badNums << " " << *nums;  // string in badNums
			} else                        
				// ``writes'' to formatted's string
				formatted << " " << format(*nums); 
		}
		if (badNums.str().empty())      // there were no bad numbers
			os << entry->name << " "    // print the name 
			   << formatted.str() << endl; // and reformatted numbers 
		else                   // otherwise, print the name and bad numbers
			cerr << "input error: " << entry->name 
			     << " invalid number(s) " << badNums.str() << endl;
	}
```


### **第九章 顺序容器**

P292-P332

顺序容器为程序员提供了控制元素存储和访问顺序的能力。

#### **9.1 顺序容器概述**

| 类型 | 作用 |
| ------------ | ------------------------------------------------------------ |
| vector | 可变数组大小。支持快速随机访问。在尾部之外的位置插入或删除元素可能很慢。 |
| deque | 双端队列。支持快速随机访问。在头尾位置插入/删除速度很快。 |
| list | 双向链表。只支持双向顺序访问。在list中任何位置进行插入/删除操作速度都很快。 |
| forward_list | 单向链表。只支持单向顺序访问。在链表任何位置进行插入/删除操作速度都很快。 |
| array | 固定大小数组。支持快速随机访问。不能添加或删除元素。 |
| string | 与vector相似的容器，但专门用于保存字符、随机访问快。在尾部插入/删除速度快。 |

#### **9.2 容器库概述**

一般，每个容器都定义在一个头文件中。

容器均定义为模板类。

| 类型别名 | |
| --------------- | ------------------------------------------------------ |
| iterator | 此容器类型的迭代器类型 |
| const_iterator | 可以读取元素，但不能修改元素的迭代器类型 |
| size_type | 无符号整数类型，足够保存此种容器类型最大可能容器的大小 |
| difference_type | 带符号整数类型，足够保存两个迭代器之间的距离 |
| value_type | 元素类型 |
| reference | 元素的左值诶性：与value_type&含义相同 |
| const_reference | 元素的const左值类型（即，const value_type&） |

| 构造函数 | |
| --------------- | ----------------------------------------------------------- |
| C c; | 默认构造函数，构造空容器 |
| C c1(c2) | 构造c2的拷贝c1 |
| C c(b, e) | 构造c，将迭代器b和e指定的范围内的元素拷贝到c（array不支持） |
| C c{a, b, c...} | 列表初始化c |

| 赋值与swap | |
| ----------------- | --------------------------------------------- |
| c1=c2 | 将c1中的元素替换为c2中元素 |
| c1 = {a, b, c...} | 将c1中的元素替换为列表中元素（不适用于array） |
| a.swap(b) | 交换a和b的元素 |
| swap(a, b) | 与a.swap(b)等价 |

| 大小 | |
| ------------ | --------------------------------------- |
| c.size() | c中元素的数组（不支持forward_list） |
| c.max_size() | c中可保存的最大元素数目 |
| c.empty() | 若c中存储了元素，返回false,否则返回true |

| 添加/删除元素（不适用于array） | |
| ------------------------------ | --------------------------- |
| c.insert(args) | 将args中的元素拷贝进c |
| c.emplace(inits) | 使用inits构造c中的一个元素 |
| c.erase(args) | 删除args指定的元素 |
| c.clear() | 删除c中的所有元素，返回void |

| 关系运算符 | |
| ---------- | ------------------------------- |
| ==, != | 所有容器都支持相等(不等运算符) |
| <,<=,>,>= | 关系运算符(无序关联容器不支持） |

| 获取迭代器 | |
| -------------------- | ----------------------------------------- |
| c.begin(), c.end() | 返回指向c的首元素和尾元素之后位置的迭代器 |
| c.cbengin(),c.cend() | 返回const_iterator |

| 反向容器的额外成员（不支持forward_list） | |
| ---------------------------------------- | ----------------------------------------- |
| reverse_iterator | 按逆序寻址元素的迭代器 |
| const_reverse_iterator | 不能修改元素的逆序迭代器 |
| c.rbegin(), c.rend() | 返回指向c的尾元素和首元素之前位置的迭代器 |
| c.crbegin(), c.crend() | 返回const_reverse_iterator |

（1）迭代器

标准库的迭代器允许我们访问容器中的元素，所有迭代器都是通过解引用运算符来实现这个操作。

一个迭代器返回由一对迭代器表示，两个迭代器分别指向同一个容器中的元素或者是尾元素之后的位置。它们标记了容器中元素的一个范围。

左闭合区间：[begin, end)

```cpp
while (begin !=end){
		*begin = val;
		++begin;
}
```

（2）容器类型成员

见概述

通过别名，可以在不了解容器中元素类型的情况下使用它。

（3）begin和end成员

begin是容器中第一个元素的迭代器

end是容器尾元素之后位置的迭代器

（4）容器定义和初始化

P290

```cpp
C c;            // 默认构造函数
C c1(c2)
C c1=c2
C c{a,b,c...}   // 列表初始化
C c={a,b,c...}
C c(b,e)        // c初始化为迭代器b和e指定范围中的元素的拷贝
// 只有顺序容器（不包括array）的构造函数才能接受大小参数
C seq(n)
C seq(n,t)
```

**将一个容器初始化为另一个容器的拷贝**:

当将一个容器初始化为另一个容器的拷贝时，两个容器的容器类型和元素类型都必须相同。

不过，当传递迭代器参数来拷贝一个范围时，就不要求容器类型相同，只要能将要拷贝的元素转换为要初始化的容器的元素类型即可。

**标注库array具有固定大小**:

不能对内置数组类型进行拷贝或对象赋值操作，但array并无此限制。P301

（5）赋值与swap

arrray类型不允许用花括号包围的值列表进行赋值。

```cpp
array<int, 10> a2={0}; //所有元素均为0
s2={0};  // 错误！
```

seq.assign(b,e) // 将seq中的元素替换为迭代器b和e所表示的范围中的元素。迭代器b和e不能指向seq中的元素。

swap用于交换2个相同类型容器的内容。调用swap之后，两个容器中的元素将交换。

（6）容器大小操作

size 返回容器中元素的数目

empty 当size为0返回布尔值true，否则返回false

max_size 返回一个大于或等于该类型容器所能容纳的最大元素数的值

（7）关系运算符

关系运算符左右两边的元素符对象必须是相同类型的容器。

::: tip

只有当元素类型也定义了相应的比较运算符，才可以使用关系元素安抚来比较两个容器

:::

#### **9.3 顺序容器操作**

（1）向顺序容器添加元素

表格P305

**使用push_back**:追加到容器尾部

**使用push_front**:插入到容器头部

**在容器中的特定位置添加元素**:使用insert

```cpp
vector <string> svec;
svec.insert(svec.begin(), "Hello!");
```

**插入范围内元素**:使用insert

**使用emplace操作**:

emplace_front、emplace和emplace_back分别对应push_front、insert和push_back。

emplace函数直接在容器中构造函数，不是拷贝。

（2）访问元素

P309

注意end是指向的是容器尾元素之后的元素。

| 在顺序容器中访问元素的操作 | |
| -------------------------- | ------------------------------------------------------------ |
| c.back() | 返回c中尾元素的引用。若c为空，函数行为未定义 |
| c.front() | 返回c中首元素的引用。若c为空，哈数行为未定义 |
| c[n] | 返回c中下标为n的元素的引用，n是一个无符号整数。若n>=size(),则函数行为未定义 |
| c.at[n] | 返回下标为n的元素的引用。如果下标越界，则抛出out_of_range异常 |

（3）删除元素

| 顺序容器的删除操作 | |
| ------------------ | ------------------------------------------------------------ |
| c.pop_back() | 删除c中尾元素。若c为空，则函数行为未定义。返回返回void |
| c.pop_front() | 删除c中首元素。若c为空，则函数行为未定义。返回void |
| c.erase(p) | 删除迭代器p所指定的元素，返回一个指向被删除元素之后元素的迭代器，如p指向尾元素，则返回尾后(off-the-end)迭代器。若p是尾后迭代器，则函数行为未定义 |
| c.erase(b, e) | 删除迭代器b和e所指定范围内的元素。返回一个指向最后一个被删除元素之后元素的迭代器。若e本身就是尾后迭代器，则函数也返回尾后迭代器 |
| c.claer() | 删除c中的所有元素。返回void |

（4）特殊的forwar_list操作

P313

befor_begin();cbefore_begin();insert_after;emplace_after;erase_after;

（5）改变容器大小

reseize用于扩大或者缩小容器。

resize操作接受一个可选的元素值参数，用来初始化添加到容器内的元素。

如果容器保存的是类类型元素，且resize向容器中添加新元素，则必须提供初始值，或者元素类型必须提供一个默认构造函数。

#### **9.4 vector对象是如何增长的**

为了避免影性能，标准库采用了可以减少容器空间重新分配次数的策略。当不得不获取新的内存空间时，vector和string通常会分配比新的新的空间需求更大的内存空间。容器预留这些空间作为备用，可以用来保存更多的新元素。

**容器管理的成员函数**:

| 容器大小管理擦作 | |
| ----------------- | ------------------------------------------------------------ |
| c.shrink_to_fit() | 请将capacity()减少为与size()相同大小 |
| c.capacity() | 不重新分配内存空间的话,c可以保存多少元素 |
| c.reverse() | 分配至少能容纳n个元素的内存空间。reverse并不改变容器中元素的数量，它仅影响vector预先分配多大的内存空间。调用reverse永远不减少容器占用的内存空间。 |

**capcacity和size**:

区别：

容器的size是指它已经保存的元素的数目；

capcacity则是在不分配新的内存空间的前提下它最多可以保存多少元素。

注意：只有当迫不得已时才可以分配新的内存空间。

#### **9.5 额外的string操作**

（1）构造string的其他方法

| 构造string的其他方法 | |
| ------------------------- | ------------------------------------------ |
| string s(cp, n) | s是cp指向的数组中前n个字符的拷贝 |
| string s(s2, pos2) | s是string s2从下标pos2开始的字符的拷贝。 |
| string s (s2, pos2, len2) | s是string s2从下标pos2开始len2个字符的拷贝 |

**substr操作**:

substr操作返回一个string，它是原始string的一部分或全部的拷贝。

s.substr(pos, n) 返回一个string，包含s中从pos开始的n个字符的拷贝。pos的默认值为0。n的默认值为s.size() - pos， 即拷贝从pos开始的所有字符

（2）改变string的其他方法

assign 替换赋值，总是替换string中的所有内容

insert 插入

append 末尾插入，总是将新字符追加到string末尾

replace 删除再插入

（3）string搜索操作

| string搜索操作 | |
| ------------------------- | --------------------------------------------- |
| s.find(args) | 查找s中args第一次出现的位置 |
| s.rfind(args) | 查找s中args最后一次出现的位置 |
| s.find_first_of(args) | 在s中查找args中任何一个字符第一次出现的位置 |
| s.find_last_of(args) | 在s中查找args中任何一个字符最后一次出现的位置 |
| s.find_first_not_of(args) | 在s中查找第一个不在args中的字符 |
| s.find_last_not_of(args) | 在s中查找最后一个不在args中的字符 |

（4）compare函数

compare有6个版本，P327

（5）数值转换

P328

tostring

stod

#### **9.6 容器适配器**

顺序容器适配器：

stack; queue; priority_queue;

适配器是一种机制，能使某种事物看起来像另外一种事物。

**定义一个适配器**:

适配器有2个构造函数:

> 1、默认构造函数创建一个空对象
> 2、接受一个容器的构造函数

**栈适配器**:

| 栈的操作 | |
| --------------- | ------------------------------------------------------------ |
| s.pop() | 删除栈顶元素，但不返回该元素值 |
| s.push(item) | 创建一个新元素压入栈顶，该元素通过拷贝或移动item而来，或者由args构造 |
| s.emplace(args) | 由arg构造 |
| s.top() | 返回栈顶元素，但不将元素弹出栈 |

**队列适配器**:

| queue和priority_queue操作 | |
| ----------------------------------- | ------------------------------------------------------------ |
| q.pop() | 返回queue的首元素或priority_queue的最高优先级的元素，但不删除此元素 |
| q.front() q.back() | 返回首元素或尾元素，但不删除此元素。只适用于queue |
| q.top() | 返回最高优先级元素，但不删除该元素。只适用于priority_queue |
| q.push(item) q.empalce(args) | 在queue末尾或priority_queue中恰当的位置创建一个元素，其值为item,或者由args构造 |

#### **术语**

begin容器操作：返回一个指向容器首元素的迭代器，如果容器为空，则返回尾后迭代器。是否返回const迭代器依赖于容器的类型。

cbegin容器操作：返回一个指向容器尾元素之后的const_iterator。


### **第十章 泛型算法**

P336-P371

标准库并未给每个容器添加大量功能，而是提供了一组算法。这些算法是通用的，可以用于不同类型的容器和不同类型的元素。

#### **10.1 概述**

头文件：algorithm、numeric

算法不依赖于容器，但算法依赖于元素类型的操作。

#### **10.2 初识泛型算法**

（1）只读算法

accumulate 求和

equal 是否相等

（2）写容器元素的算法

算法不检查写操作

拷贝算法：copy

重排容器元素的算法：sort

::: tip

标准库函数对迭代器而不是容器进行操作。因此，算法不能直接添加或删除元素

:::

#### **10.3 定制操作**

标准库允许我们提供自己定义的操作来代替默认运算符。

（1）向算法传递函数

**谓词**:

谓词是一个可调用的表达式，其返回结果是一个能用作条件的值。

> 标准库算法的谓词分为两类：
> 1、一元谓词：只接受单一参数。
> 2、二元谓词：接受两个参数。

```cpp
bool isShorter(const string &s1, const string &s2)
{
		retrun s1.size() < s2.size();
}

sort(words.begin(), words.end(), isShorter);
```

**排序算法**:

stable_sort算法维持相等元素的原有顺序。

（2）lambda表达式

**lamba**:

lambda表达式表示一个可调用的代码单元。一个lambda具有一个返回类型、一个参数列表和一个函数体。

```cpp
[capture list](parameter list) -> return type {function body}
// capture list 捕获列表，lambda所在函数中定义的局部变量
// 捕获列表只用于局部非static变量，lambda可以直接使用局部static变量和在它所在函数之外声明的名字
// lambda必须使用尾置返回来指定返回类型
```

（3）lambda捕获和返回

两种：值捕获、引用捕获

::: warnning

当以引用方式捕获一个变量时，必须保证在lambda执行时变量是存在的。

一般的，应该尽量减少捕获的数据量，来避免潜在的问题。

如果可能，避免捕获指针或引用。

:::

**隐式捕获**：

当混合使用隐式捕获和显式捕获时，捕获列表中的第一个元素必须是一个&或=。显式捕获的变量必须使用与隐式捕获不同的方式。

lambda捕获列表 P352

**可变lambda**:

若希望改变一个被捕获的变量的值，必须在参数列表首加上关键字mutable。

**指定lambda返回类型**:

当需要为lambda定义返回类型时，必须使用尾置返回类型。

（4）参数绑定

**标准库bind函数**:

```cpp
auto newCallable = bind(callable, arg_list);
// 调用newCallable时，newCallable会调用callable,并传递给它arg_list中的参数
```

#### **10.4 再探迭代器**

插入迭代器、流迭代器、反向迭代器、移动迭代器

（1）插入迭代器

back_inserter：创建一个使用push_back的迭代器

front_inserter：创建一个使用push_front的迭代器

inserter：创建一个使用inserter的迭代器

（2）iostream迭代器

istream_iterator 读取输入流

ostream_iterator 向一个输出流写数据

**istream_iterator操作**:

| istream-iterator操作 | |
| ---------------------------------- | ------------------------------------------------------------ |
| istream_iterator<T> in(is); | in从输入流is读取类型为T的值 |
| istream_iterator<T> end; | 读取类型为T的值得istream_iterator迭代器，表示尾后位置 |
| in1 == in2 in1 != in2 | in1和in2必须读取相同类型。如果它们都是尾后迭代器，或绑定到相同的输入，则两者相等 |
| *in | 返回从流中读取的值 |
| in->mem | 与(*in).mem含义相同 |
| ++in, in++ | 用>>从输入流读取下一个值 |

**ostream_iterator操作**:

| ostream_iterator操作 | |
| ------------------------------- | ------------------------------------------------------------ |
| ostream_iterator<T> out(os); | out将类型为T的值写到输出流os中 |
| ostream_iterator<T> out(os, d); | out将类型为T的值写到输出流os中，每个值后面都输出一个d。d指向一个空字符串结尾的字符数组 |
| out = val | 用<<将val写入到out所绑定的ostream中 |
| *out, ++out, out++ | |

（3）反向迭代器

反向迭代器就是在容器中从尾元素向首元素反向移动的迭代器。

#### **10.5 泛型算法结构**

| 迭代器类别 | |
| -------------- | ------------------------------------ |
| 输入迭代器 | 只读、不写；单遍扫描，只能递增 |
| 输出迭代器 | 只写，不读；单遍扫描，只能递增 |
| 前向迭代器 | 可读写；多遍扫描，只能递增 |
| 双向迭代器 | 可读写，多遍扫描，可递增递减 |
| 随机访问迭代器 | 可读写，多遍扫描，支持全部迭代器运算 |

#### **10.6 特定容器算法**

对于list、forward_list，应该优先使用成员函数的算法而不是通用算法。

#### **术语**

cref标准库函数：返回一个可拷贝的对象，其中保存了一个指向不可拷贝类型的const对象的引用


### **第十一章 关联容器**

P374-P397

关联容器支持高效的关键字查找和访问。

| 类型 | 备注 |
| ------------------ | --------------------------------- |
| map | 关联数组，保存关键字-值对 |
| set | 值保存关键字的容器 |
| multimap | 关键字可重复出现的map |
| multiset | 关键字可重复出现的set |
| unordered_map | 用哈希函数组织的map |
| unordered_set | 用哈希函数组织的set |
| unordered_multimap | 哈希组织的map；关键字可以重复出现 |
| unordered_multiset | 哈希组织的set；关键字可以重复出现 |

#### **11.1 使用关联容器**

map是关键词-值对的集合。

为了定义一个map，我们必须指定关键字和值的类型。

```cpp
// 统计每个单词在输入中出现的次数
map<string, size_t> word_count;
string word;
while (cin >> word)
    ++word_count[word];
for (const auto &w : word_count)
  	count << w.first << " cccurs " < w.second 
  				<< ((w.second > 1) ? " times" : "time") << endl;
```

set是关键字的简单集合。

为了定义一个set，必须指定其元素类型。

```cpp
// 统计输入中每个单词出现的次数，并忽略常见单词
map<string, size_t> word_count;
set<string> exclude = {"the", "But"};
string word;
while (cin >> word)
		// 只统计不在exclude中的单词
		if (exclude.find(word) == exclude.end())
				++word_count[word]; //获取并递增word的计数器
```

#### **11.2 关联容器概述**

（1）定义关联容器

定义map时，必须指明关键字类型又指明值类型；

定义set时，只需指明关键字类型。

（2）pair类型

pair标准库类型定义在头文件utility中。

一个pair保存两个数据成员。当创建一个pair时，必须提供两个类型名。

```cpp
pair<string, string> anon; // 保存两个string
pair<string, string> author{"James", "Joyce"}; // 也可为每个成员提供初始化器
```

pair的数据类型是public的，两个成员分别命名为first和second。

pair上的操作，见表，P380

#### **11.3 关联容器操作**

| 关联容器额外的类型别名 | |
| ---------------------- | ------------------------------------------------------------ |
| key_type | 此容器类型的关键字类型 |
| mapped_type | 每个关键字关联的类型，只适用于map |
| value_type | 对于set，与key_type相同；对于map，为pair<const key_type, mapped_type> |

（1）关联容器迭代器

set的迭代器是const的

set和map的关键字都是const的

**遍历关联容器**：

map和set都支持begin和end操作。使用beigin、end获取迭代器，然后用迭代器来遍历容器。

（2）添加元素

| 关联容器insert操作 | |
| ------------------ | ------------------------ |
| c.insert(v) | v是value_type类型的对象; |
| c.emplace(args) | args用来构造一个元素 |
| c.insert(b, e) | |
| c.insert(il) | |
| c.insert(p, v) | |
| c.emplace(p ,args) | |

（3）删除元素

| 从关联容器删除元素 | |
| ------------------ | ------------------------------------------------------------ |
| c.erase(k) | 从c中删除每个关键字为k的元素。返回一个size_type值，指出删除的元素的数量 |
| c.erase(p) | 从c中删除迭代器p指定的元素。p必须指向c中一个真实元素，不能等于c.end()。返回一个指向p之后元素的迭代器，若p指向c中的尾元素，则返回.end() |
| c.erase(b, e) | 删除迭代器b和e所表示的范围中的元素。返回e |

（4）map的下标操作

| map和unorder_map的下标操作 | |
| -------------------------- | ------------------------------------------------------------ |
| c[k] | 返回关键字为k的元素；如果k不在c中，添加一个关键字为k的元素，对其进行值初始化 |
| c.at[k] | 访问关键字为k的元素，带参数检查；若k不在c中，抛出一个out_of_range异常 |

::: tip

map进行下标操作，会获得mapped_type对象；当解引用时，会得到value_type对象。

:::

（5）访问元素

```cpp
c.find(k)  // 返回一个迭代器，指向第一个关键字k的元素，如k不在容器中，则返回尾后迭代器
c.count(k)  // 返回关键字等于k的元素的数量。对于不允许重复关键字的容器，返回值永远是0或1
c.lower_bound(k)  // 返回一个迭代器，指向第一个关键字不小于k的元素;不适用于无序容器
c.upper_bound(k)  // 返回一个迭代器，指向第一个关键字大于k的元素；不适用于无序容器
c.equal_bound(k)  // 返回一个迭代器pair，表示关键字等于k的元素的范围。如k不存在，pair的两个成员均等于c.end()
```

#### **11.4 无序容器**

无序容器使用关键字类型的==运算符和一个hash<key_type>类型的对象来组织元素。

无序容器在存储上组织为一组桶，适用一个哈希函数将元素映射到桶。

无序容器管理操作，表格，P395

还可以自定义自己的hash模板 P396

```cpp
using SD_multiset = unordered_multiset<Sales_data, decltype(hasher)*, decltype(eqOp)*>;
SD_multiset bookStore(42, haser, eqOp);
```




