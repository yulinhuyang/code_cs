
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
| :----- | ------ | -------------- | :------------------------------------------- -- |
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



