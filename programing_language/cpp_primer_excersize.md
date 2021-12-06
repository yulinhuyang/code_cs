# 第一章 开始

## 练习1.1
查阅你使用的编译器的文档，确定它所使用的文件名约定。编译并运行第2页的main程序。

解：
- ``g++ --std=c++11 ch1.cpp -o main``
- ``./main``

## 练习1.2
改写程序，让它返回-1。返回值-1通常被当做程序错误的标识。重新编译并运行你的程序，观察你的系统如何处理main返回的错误标识。

解：
- 在ubuntu下，使用g++，返回-1，``./main``没有发现任何异常。
- ``echo $?``，返回255。

## 练习1.3
编写程序，在标准输出上打印Hello, World。

解：
```cpp
#include <iostream>

int main()
{
	std::cout << "Hello, World" << std::endl;
	return 0;
}
```

## 练习1.4
我们的程序使用加法运算符`+`来将两个数相加。编写程序使用乘法运算符`*`，来打印两个数的积。

解：
```cpp
#include <iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
    int v1 = 0, v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The product of " << v1 << " and " << v2
              << " is " << v1 * v2 << std::endl;
}        
```

## 练习1.5
我们将所有的输出操作放在一条很长的语句中，重写程序，将每个运算对象的打印操作放在一条独立的语句中。

解：
```cpp
#include <iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
        int v1 = 0, v2 = 0;
        std::cin >> v1 >> v2;
        std::cout << "The product of ";
        std::cout << v1;
        std::cout << " and ";
        std::cout << v2;
        std::cout << " is ";
        std::cout << v1 * v2;
        std::cout << std::endl;
}    
```

## 练习1.6
解释下面程序片段是否合法。
```cpp
std::cout << "The sum of " << v1;
          << " and " << v2;
          << " is " << v1 + v2 << std::endl;
```
如果程序是合法的，它的输出是什么？如果程序不合法，原因何在？应该如何修正？

解：

程序不合法，有多余的分号，修改如下：
```cpp
std::cout << "The sum of " << v1
          << " and " << v2
          << " is " << v1 + v2 << std::endl;
```

## 练习1.7
编译一个包含不正确的嵌套注释的程序，观察编译器返回的错误信息。

解：
```cpp
/* 正常注释 /* 嵌套注释 */ 正常注释*/
```
错误信息：
```
  /* 正常注释 /* 嵌套注释 */ 正常注释*/
                                     ^
ch1.cpp:97:37: error: stray ‘\255’ in program
ch1.cpp:97:37: error: stray ‘\243’ in program
ch1.cpp:97:37: error: stray ‘\345’ in program
ch1.cpp:97:37: error: stray ‘\270’ in program
ch1.cpp:97:37: error: stray ‘\270’ in program
ch1.cpp:97:37: error: stray ‘\346’ in program
ch1.cpp:97:37: error: stray ‘\263’ in program
ch1.cpp:97:37: error: stray ‘\250’ in program
ch1.cpp:97:37: error: stray ‘\351’ in program
ch1.cpp:97:37: error: stray ‘\207’ in program
ch1.cpp:97:37: error: stray ‘\212’ in program
ch1.cpp: In function ‘int main()’:
ch1.cpp:97:50: error: expected primary-expression before ‘/’ token
  /* 正常注释 /* 嵌套注释 */ 正常注释*/
                                                  ^
ch1.cpp:98:5: error: expected primary-expression before ‘return’
     return 0;
     ^
```

## 练习1.8
指出下列哪些输出语句是合法的（如果有的话）：
```cpp
std::cout << "/*";
std::cout << "*/";
std::cout << /* "*/" */;
std::cout << /* "*/" /* "/*" */;
```
预测编译这些语句会产生什么样的结果，实际编译这些语句来验证你的答案(编写一个小程序，每次将上述一条语句作为其主体)，改正每个编译错误。

解：

只有第三句编译出错，改成如下即可：
```cpp
std::cout << /* "*/" */";
```
第四句等价于输出 `" /* "`。

## 练习1.9

编写程序，使用`while`循环将50到100整数相加。

解：
```cpp
#include <iostream>

int main()
{
    int sum = 0, val = 50;
    while (val <= 100){
        sum += val;
        val += 1;
    }
    std::cout << "Sum of 50 to 100 inclusive is "
              << sum << std::endl;
}    
```

## 练习1.10
除了`++`运算符将运算对象的值增加1之外，还有一个递减运算符`--`实现将值减少1.编写程序与，使用递减运算符在循环中按递减顺序打印出10到0之间的整数。

解：
```cpp
#include <iostream>

int main()
{
    int val = 10;
    while (val >= 0){
        std::cout << val << " ";
        val -= 1;
    }
    std::cout << std::endl;
}  
```

## 练习1.11
编写程序，提示用户输入两个整数，打印出这两个整数所指定的范围内的所有整数。

解：
```cpp
#include <iostream>

int main()
{
    int start = 0, end = 0;
    std::cout << "Please input two num: ";
    std::cin >> start >> end;
    if (start <= end) {
        while (start <= end){
            std::cout << start << " ";
            ++start;
        }
        std::cout << std::endl;
    }
    else{
        std::cout << "start should be smaller than end !!!";
    }
}  
```

## 练习1.12

下面的for循环完成了什么功能？sum的终值是多少？
```cpp
int sum = 0;
for (int i = -100; i <= 100; ++i)
	sum += i;
```

解：

从-100加到100，sum的终值是0。

## 练习1.13
使用for循环重做1.4.1节中的所有练习（练习1.9到1.11）。

解：

### 练习1.9

```cpp
#include <iostream>

int main()
{
    int sum = 0;
    for (int val = 50; val <= 100; ++val){
        sum += val;
    }
    std::cout << "Sum of 50 to 100 inclusive is "
              << sum << std::endl;
}    
```

### 练习1.10

```cpp
#include <iostream>

int main()
{
    for (int val = 10; val >=0; --val){
        std::cout << val << " ";
    }
    std::cout << std::endl;
}  
```

### 练习1.11

```cpp
#include <iostream>

int main()
{
    int start = 0, end = 0;
    std::cout << "Please input two num: ";
    std::cin >> start >> end;
    if (start <= end) {
        for (; start <= end; ++start){
            std::cout << start << " ";
        }
        std::cout << std::endl;
    }
    else{
        std::cout << "start should be smaller than end !!!";
    }
}  
```

## 练习1.14
对比for循环和while循环，两种形式的优缺点各是什么？

解：
```
The main difference between the `for`'s and the `while`'s is a matter of pragmatics: 
we usually use `for` when there is a known number of iterations, 
and use `while` constructs when the number of iterations in not known in advance. 
The `while` vs `do ... while` issue is also of pragmatics, 
the second executes the instructions once at start, 
and afterwards it behaves just like the simple `while`.
```

## 练习1.15
编写程序，包含第14页“再探编译”中讨论的常见错误。熟悉编译器生成的错误信息。
   
解：

编译器可以检查出的错误有：
- 语法错误
- 类型错误 
- 声明错误

## 练习1.16
编写程序，从cin读取一组数，输出其和。

解：

```cpp
#include <iostream>

int main()
{
    int sum = 0;
    for (int value = 0; std::cin >> value; )
        sum += value;
    std::cout << sum << std::endl;
    return 0;
}
```

## 练习1.17
如果输入的所有值都是相等的，本节的程序会输出什么？如果没有重复值，输出又会是怎样的？

## 练习1.18
编译并运行本节的程序，给它输入全都相等的值。再次运行程序，输入没有重复的值。

解：

全部重复：
```
1 1 1 1 1 
1 occurs 5 times 
```

没有重复：
```
1 2 3 4 5
1 occurs 1 times 
2 occurs 1 times 
3 occurs 1 times 
4 occurs 1 times 
5 occurs 1 times 
```

## 练习1.19
修改你为1.4.1节练习1.11（第11页）所编写的程序（打印一个范围内的数），使其能处理用户输入的第一个数比第二个数小的情况。

解：

```cpp
#include <iostream>

int main()
{
    int start = 0, end = 0;
    std::cout << "Please input two num: ";
    std::cin >> start >> end;
    if (start <= end) {
        while (start <= end){
            std::cout << start << " ";
            ++start;
        }
        std::cout << std::endl;
    }
    else{
        std::cout << "start should be smaller than end !!!";
    }
}  
```

## 练习1.20
在网站http://www.informit.com/title/032174113 上，第1章的代码目录包含了头文件 Sales_item.h。将它拷贝到你自己的工作目录中。用它编写一个程序，读取一组书籍销售记录，将每条记录打印到标准输出上。

解：

```cpp
#include <iostream>
#include "Sales_item.h"

int main()
{
	for (Sales_item item; std::cin >> item; std::cout << item << std::endl);
	return 0;
}
```

命令：
```
./main < data/add_item
```

输出：
```
0-201-78345-X 3 60 20
0-201-78345-X 2 50 25
```

## 练习1.21
编写程序，读取两个 ISBN 相同的 Sales_item 对象，输出他们的和。

解：

```cpp
#include <iostream>
#include "Sales_item.h"

int main()
{
    Sales_item item_1;
    Sales_item item_2;
    std::cin >> item_1;
    std::cout << item_1 << std::endl;
    std::cin >> item_2;
    std::cout << item_2 << std::endl;
    std::cout << "sum of sale items: " << item_1 + item_2 << std::endl;
	return 0;
}
```

命令：
```
./main < data/add_item
```

输出：
```
0-201-78345-X 3 60 20
0-201-78345-X 2 50 25
sum of sale items: 0-201-78345-X 5 110 22
```

## 练习1.22
编写程序，读取多个具有相同 ISBN 的销售记录，输出所有记录的和。

解：

```cpp
#include <iostream>
#include "Sales_item.h"

int main()
{
    Sales_item sum_item;
    std::cin >> sum_item;
    std::cout << sum_item << std::endl;
    for (Sales_item item; std::cin >> item; std::cout << item << std::endl){
        sum_item += item;
    }
    std::cout << "sum of sale items: " << sum_item << std::endl;
	return 0;
}
```

命令：
```
./main < data/add_item
```

输出：
```
0-201-78345-X 3 60 20
0-201-78345-X 2 50 25
sum of sale items: 0-201-78345-X 5 110 22
```

## 练习1.23
编写程序，读取多条销售记录，并统计每个 ISBN（每本书）有几条销售记录。

## 练习1.24
输入表示多个 ISBN 的多条销售记录来测试上一个程序，每个 ISBN 的记录应该聚在一起。

解：

```cpp
#include <iostream>
#include "Sales_item.h"

int main()
{
    Sales_item total;
    if (std::cin >> total){
        Sales_item trans;
        while (std::cin >> trans){
            if (total.isbn() == trans.isbn()) {
                total += trans;
            }
            else {
                std::cout << total << std::endl;
                total = trans;
            }
        }
        std::cout << total << std::endl;
    }
    else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

命令：
```
./main < data/book_sales
```

输出：
```
0-201-70353-X 4 99.96 24.99
0-201-82470-1 4 181.56 45.39
0-201-88954-4 16 198 12.375
0-399-82477-1 5 226.95 45.39
0-201-78345-X 5 110 22
```
## 练习1.25
借助网站上的`Sales_item.h`头文件，编译并运行本节给出的书店程序。


# 第二章 变量和基本类型 

## 练习2.1
类型 int、long、long long 和 short 的区别是什么？无符号类型和带符号类型的区别是什么？float 和 double的区别是什么？

解：

C++ 规定 short 和 int 至少16位，long 至少32位，long long 至少64位。 带符号类型能够表示正数、负数和 0 ，而无符号类型只能够表示 0 和正整数。

## 练习2.2
计算按揭贷款时，对于利率、本金和付款分别应选择何种数据类型？说明你的理由。

解：

使用`double`。需要进行浮点计算。

## 练习2.3
读程序写结果。
```cpp
unsigned u = 10, u2 = 42;
std::cout << u2 - u << std::endl;
std::cout << u - u2 << std::endl;
int i = 10, i2 = 42;
std::cout << i2 - i << std::endl;
std::cout << i - i2 << std::endl;
std::cout << i - u << std::endl;
std::cout << u - i << std::endl;
```

解：

输出：
```
32
4294967264
32
-32
0
0
```

## 练习2.4
编写程序检查你的估计是否正确，如果不正确，请仔细研读本节直到弄明白问题所在。

## 练习2.5
指出下述字面值的数据类型并说明每一组内几种字面值的区别：
```
(a) 'a', L'a', "a", L"a"
(b) 10, 10u, 10L, 10uL, 012, 0xC
(c) 3.14, 3.14f, 3.14L
(d) 10, 10u, 10., 10e-2
```

解：
- (a): 字符字面值，宽字符字面值，字符串字面值，宽字符串字面值。
- (b): 十进制整型，十进制无符号整型，十进制长整型，十进制无符号长整型， 八进制整型，十六进制整型。
- (c): double, float, long double
- (d): 十进制整型，十进制无符号整型，double, double

## 练习2.6
下面两组定义是否有区别，如果有，请叙述之：
```cpp
int month = 9, day = 7;
int month = 09, day = 07;
```
解：

第一行定义的是十进制的整型，第二行定义的是八进制的整型。但是month变量有误，八进制不能直接写9。

## 练习2.7
下述字面值表示何种含义？它们各自的数据类型是什么？
```cpp
(a) "Who goes with F\145rgus?\012"
(b) 3.14e1L
(c) 1024f
(d) 3.14L
```

解：
- (a) Who goes with Fergus?(换行)，string 类型
- (b) long double
- (c) 无效，因为后缀`f`只能用于浮点字面量，而1024是整型。
- (d) long double

## 练习2.8
请利用转义序列编写一段程序，要求先输出 2M，然后转到新一行。修改程序使其先输出 2，然后输出制表符，再输出 M，最后转到新一行。

解：
```cpp
#include <iostream>
int main()
{
   std::cout << 2 << "\115\012";
   std::cout << 2 << "\t\115\012";
   return 0;
}
```   

## 练习2.9
解释下列定义的含义，对于非法的定义，请说明错在何处并将其改正。

- (a) std::cin >> int input_value;
- (b) int i = { 3.14 };
- (c) double salary = wage = 9999.99;
- (d) int i = 3.14;

解： 

(a): 应该先定义再使用。
```cpp
int input_value = 0;
std::cin >> input_value;
```

(b): 用列表初始化内置类型的变量时，如果存在丢失信息的风险，则编译器将报错。
```cpp
double i = { 3.14 };
```

(c): 在这里`wage`是未定义的，应该在此之前将其定义。
```cpp
double wage;
double salary = wage = 9999.99;
```

(d): 不报错，但是小数部分会被截断。
```cpp
double i = 3.14;
```

## 练习2.10
下列变量的初值分别是什么？
```cpp
std::string global_str;
int global_int;
int main()
{
    int local_int;
    std::string local_str;
}
```

解：

`global_str`和`global_int`是全局变量，所以初值分别为空字符串和0。
`local_int`是局部变量并且没有初始化，它的初值是未定义的。 
`local_str` 是 `string` 类的对象，它的值由类确定，为空字符串。

## 练习2.11
指出下面的语句是声明还是定义：

- (a) extern int ix = 1024;
- (b) int iy;
- (c) extern int iz;

解：

(a): 定义
(b): 定义
(c): 声明

## 练习2.12
请指出下面的名字中哪些是非法的？

- (a) int double = 3.14;
- (b) int _;
- (c) int catch-22;
- (d) int 1_or_2 = 1;
- (e) double Double = 3.14;

解：

(a), (c), (d) 非法。

## 练习2.13
下面程序中`j`的值是多少？

```cpp
int i = 42;
int main()
{
    int i = 100;
    int j = i;
}
```

解：

`j`的值是100，局部变量`i`覆盖了全局变量`i`。

## 练习2.14
下面的程序合法吗？如果合法，它将输出什么？
```cpp
int i = 100, sum = 0;
for (int i = 0; i != 10; ++i)
    sum += i;
std::cout << i << " " << sum << std::endl;
```

解：

合法。输出是 100 45 。


## 练习2.15
下面的哪个定义是不合法的？为什么？

- (a) int ival = 1.01;
- (b) int &rval1 = 1.01;
- (c) int &rval2 = ival;
- (d) int &rval3;

解：

(b)和(d)不合法，(b)引用必须绑定在对象上，(d)引用必须初始化。

## 练习2.16
考察下面的所有赋值然后回答：哪些赋值是不合法的？为什么？哪些赋值是合法的？它们执行了哪些操作？

```cpp
int i = 0, &r1 = i; 
double d = 0, &r2 = d;
```
- (a) r2 = 3.14159;
- (b) r2 = r1;
- (c) i = r2;
- (d) r1 = d;

解：

- (a): 合法。给 d 赋值为 3.14159。
- (b): 合法。会执行自动转换（int->double）。
- (c): 合法。会发生小数截取。
- (d): 合法。会发生小数截取。

## 练习2.17
执行下面的代码段将输出什么结果？
```cpp
int i, &ri = i;
i = 5; ri = 10;
std::cout << i << " " << ri << std::endl;
```

解：

输出：10, 10

## 练习2.18
编写代码分别改变指针的值以及指针所指对象的值。

解：
```cpp
int a = 0, b = 1;
int *p1 = &a, *p2 = p1;

// change the value of a pointer.
p1 = &b;
// change the value to which the pointer points
*p2 = b;
```

## 练习2.19
说明指针和引用的主要区别

解：

引用是另一个对象的别名，而指针本身就是一个对象。
引用必须初始化，并且一旦定义了引用就无法再绑定到其他对象。而指针无须在定义时赋初值，也可以重新赋值让其指向其他对象。

## 练习2.20
请叙述下面这段代码的作用。

```cpp
int i = 42;
int *p1 = &i; 
*p1 = *p1 * *p1;
```

解：

让指针 pi 指向 i，然后将 i 的值重新赋值为 42 * 42 (1764)。

## 练习2.21
请解释下述定义。在这些定义中有非法的吗？如果有，为什么？

`int i = 0;`
- (a) double* dp = &i;
- (b) int *ip = i;
- (c) int *p = &i;

解：

- (a): 非法。不能将一个指向 `double` 的指针指向 `int` 。
- (b): 非法。不能将 `int` 变量赋给指针。
- (c): 合法。

## 练习2.22
假设 p 是一个 int 型指针，请说明下述代码的含义。

```cpp
if (p) // ...
if (*p) // ...
```

解：

第一句判断 p 是不是一个空指针, 
第二句判断 p 所指向的对象的值是不是为0


## 练习2.23
给定指针 p，你能知道它是否指向了一个合法的对象吗？如果能，叙述判断的思路；如果不能，也请说明原因。

解：

不能，因为首先要确定这个指针是不是合法的，才能判断它所指向的对象是不是合法的。

## 练习2.24
在下面这段代码中为什么 p 合法而 lp 非法？
```cpp
int i = 42;
void *p = &i;
long *lp = &i;
```

解：

`void *`是从C语言那里继承过来的，可以指向任何类型的对象。
而其他指针类型必须要与所指对象严格匹配。

## 练习2.25
说明下列变量的类型和值。
```cpp
(a) int* ip, i, &r = i;
(b) int i, *ip = 0;
(c) int* ip, ip2;
```

解：
- (a): ip 是一个指向 int 的指针, i 是一个 int, r 是 i 的引用。
- (b): i 是 int , ip 是一个空指针。
- (c): ip 是一个指向 int 的指针, ip2 是一个 int。

## 练习2.26
下面哪些语句是合法的？如果不合法，请说明为什么？

解：
```cpp
const int buf;      // 不合法, const 对象必须初始化
int cnt = 0;        // 合法
const int sz = cnt; // 合法
++cnt; ++sz;        // 不合法, const 对象不能被改变
```

## 练习2.27
下面的哪些初始化是合法的？请说明原因。

解：

```cpp
int i = -1, &r = 0;         // 不合法, r 必须引用一个对象
int *const p2 = &i2;        // 合法，常量指针
const int i = -1, &r = 0;   // 合法
const int *const p3 = &i2;  // 合法
const int *p1 = &i2;        // 合法
const int &const r2;        // 不合法, r2 是引用，引用没有顶层 const
const int i2 = i, &r = i;   // 合法
```

## 练习2.28
说明下面的这些定义是什么意思，挑出其中不合法的。

解：
```cpp
int i, *const cp;       // 不合法, const 指针必须初始化
int *p1, *const p2;     // 不合法, const 指针必须初始化
const int ic, &r = ic;  // 不合法, const int 必须初始化
const int *const p3;    // 不合法, const 指针必须初始化
const int *p;           // 合法. 一个指针，指向 const int
```

## 练习2.29
假设已有上一个练习中定义的那些变量，则下面的哪些语句是合法的？请说明原因。

解：
```cpp
i = ic;     // 合法, 常量赋值给普通变量
p1 = p3;    // 不合法, p3 是const指针不能赋值给普通指针
p1 = &ic;   // 不合法, 普通指针不能指向常量
p3 = &ic;   // 合法, p3 是常量指针且指向常量
p2 = p1;    // 合法, 可以将普通指针赋值给常量指针
ic = *p3;   // 合法, 对 p3 取值后是一个 int 然后赋值给 ic
```

## 练习2.30
对于下面的这些语句，请说明对象被声明成了顶层const还是底层const？

```cpp
const int v2 = 0; int v1 = v2;
int *p1 = &v1, &r1 = v1;
const int *p2 = &v2, *const p3 = &i, &r2 = v2;
```

解：

v2 是顶层const，p2 是底层const，p3 既是顶层const又是底层const，r2 是底层const。

## 练习2.31
假设已有上一个练习中所做的那些声明，则下面的哪些语句是合法的？请说明顶层const和底层const在每个例子中有何体现。

解：

```cpp
r1 = v2; // 合法, 顶层const在拷贝时不受影响
p1 = p2; // 不合法, p2 是底层const，如果要拷贝必须要求 p1 也是底层const
p2 = p1; // 合法, int* 可以转换成const int*
p1 = p3; // 不合法, p3 是一个底层const，p1 不是
p2 = p3; // 合法, p2 和 p3 都是底层const，拷贝时忽略掉顶层const
```

## 练习2.32
下面的代码是否合法？如果非法，请设法将其修改正确。
```cpp
int null = 0, *p = null;
```
解：

合法。指针可以初始化为 0 表示为空指针。

## 练习2.33
利用本节定义的变量，判断下列语句的运行结果。

解：

```cpp
a=42; // a 是 int
b=42; // b 是一个 int,(ci的顶层const在拷贝时被忽略掉了)
c=42; // c 也是一个int
d=42; // d 是一个 int *,所以语句非法
e=42; // e 是一个 const int *, 所以语句非法
g=42; // g 是一个 const int 的引用，引用都是底层const，所以不能被赋值
```

## 练习2.34
基于上一个练习中的变量和语句编写一段程序，输出赋值前后变量的内容，你刚才的推断正确吗？如果不对，请反复研读本节的示例直到你明白错在何处为止。

## 练习2.35
判断下列定义推断出的类型是什么，然后编写程序进行验证。

```cpp
const int i = 42;
auto j = i; const auto &k = i; auto *p = &i; 
const auto j2 = i, &k2 = i;
```

解：

j 是 int，k 是 const int的引用，p 是const int *，j2 是const int，k2 是 const int 的引用。

## 练习2.36
关于下面的代码，请指出每一个变量的类型以及程序结束时它们各自的值。

```cpp
int a = 3, b = 4;
decltype(a) c = a;
decltype((b)) d = a;
++c;
++d;
```

解：

c 是 int 类型，值为 4。d 是 int & 类型，绑定到 a，a 的值为 4 。

## 练习2.37
赋值是会产生引用的一类典型表达式，引用的类型就是左值的类型。也就是说，如果 i 是 int，则表达式 i=x 的类型是 int&。根据这一特点，请指出下面的代码中每一个变量的类型和值。

```cpp
int a = 3, b = 4;
decltype(a) c = a;
decltype(a = b) d = a;
```

解：

c 是 int 类型，值为 3。d 是 int& 类型，绑定到 a。

## 练习2.38
说明由decltype 指定类型和由auto指定类型有何区别。请举一个例子，decltype指定的类型与auto指定的类型一样；再举一个例子，decltype指定的类型与auto指定的类型不一样。

解：

decltype 处理顶层const和引用的方式与 auto不同，decltype会将顶层const和引用保留起来。
```cpp
int i = 0, &r = i;
//相同
auto a = i;
decltype(i) b = i;

//不同 d 是一个 int&
auto c = r;
decltype(r) d = r;
```

## 练习2.39
编译下面的程序观察其运行结果，注意，如果忘记写类定义体后面的分号会发生什么情况？记录下相关的信息，以后可能会有用。
```cpp
struct Foo { /* 此处为空  */ } // 注意：没有分号
int main()
{
    return 0;
}。
```
解：

提示应输入分号。

## 练习2.40
根据自己的理解写出 Sales_data 类，最好与书中的例子有所区别。

```cpp
struct Sale_data
{
    std::string bookNo;
    std::string bookName;
    unsigned units_sold = 0;
    double revenue = 0.0;
    double price = 0.0;
    //...
}
```

## 练习2.41
使用你自己的Sale_data类重写1.5.1节（第20页）、1.5.2节（第21页）和1.6节（第22页）的练习。眼下先把Sales_data类的定义和main函数放在一个文件里。

```cpp
// 1.5.1

#include <iostream>
#include <string>

struct Sale_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main()
{
    Sale_data book;
    double price;
    std::cin >> book.bookNo >> book.units_sold >> price;
    book.revenue = book.units_sold * price;
    std::cout << book.bookNo << " " << book.units_sold << " " << book.revenue << " " << price;

    return 0;
}

```

```cpp
// 1.5.2

#include <iostream>
#include <string>

struct Sale_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main()
{
    Sale_data book1, book2;
    double price1, price2;
    std::cin >> book1.bookNo >> book1.units_sold >> price1;
    std::cin >> book2.bookNo >> book2.units_sold >> price2;
    book1.revenue = book1.units_sold * price1;
    book2.revenue = book2.units_sold * price2;

    if (book1.bookNo == book2.bookNo)
    {
        unsigned totalCnt = book1.units_sold + book2.units_sold;
        double totalRevenue = book1.revenue + book2.revenue;
        std::cout << book1.bookNo << " " << totalCnt << " " << totalRevenue << " ";
        if (totalCnt != 0)
            std::cout << totalRevenue / totalCnt << std::endl;
        else
            std::cout << "(no sales)" << std::endl;
        return 0;
    }
    else
    {
        std::cerr << "Data must refer to same ISBN" << std::endl;
        return -1;  // indicate failure
    }
}

```

```cpp
// 1.6

#include <iostream>
#include <string>

struct Sale_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main()
{
    Sale_data total;
    double totalPrice;
    if (std::cin >> total.bookNo >> total.units_sold >> totalPrice)
    {
        total.revenue = total.units_sold * totalPrice;

        Sale_data trans;
        double transPrice;
        while (std::cin >> trans.bookNo >> trans.units_sold >> transPrice)
        {
            trans.revenue = trans.units_sold * transPrice;

            if (total.bookNo == trans.bookNo)
            {
                total.units_sold += trans.units_sold;
                total.revenue += trans.revenue;
            }
            else
            {
                std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";
                if (total.units_sold != 0)
                    std::cout << total.revenue / total.units_sold << std::endl;
                else
                    std::cout << "(no sales)" << std::endl;

                total.bookNo = trans.bookNo;
                total.units_sold = trans.units_sold;
                total.revenue = trans.revenue;
            }
        }

        std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";
        if (total.units_sold != 0)
            std::cout << total.revenue / total.units_sold << std::endl;
        else
            std::cout << "(no sales)" << std::endl;

        return 0;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;  // indicate failure
    }
}
```

## 练习2.42
根据你自己的理解重写一个Sales_data.h头文件，并以此为基础重做2.6.2节（第67页）的练习。



# 第三章 字符串、向量和数组

## 练习3.1

使用恰当的using 声明重做 1.4.1节和2.6.2节的练习。

解：

1.4.1

```cpp
#include <iostream>

using std::cin;
using std::cout;
using std::endl;

int main()
{
	int sum = 0;
	for (int val = 1; val <= 10; ++val) sum += val;

	cout << "Sum of 1 to 10 inclusive is " << sum << endl;
	
	return 0;
}
```

2.6.2 类似

## 练习3.2
编写一段程序从标准输入中一次读入一行，然后修改该程序使其一次读入一个词。

解：

一次读入一行：

```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;
using std::getline;

int main()
{
	string s;
	while (getline(cin,s))
	{
		cout << s << endl;
	}
	return 0;
}
```

一次读入一个词

```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;
using std::getline;

int main()
{
	string s;
	while (cin >> s)
	{
		cout << s << endl;
	}
	return 0;
}
```


## 练习3.3
请说明string类的输入运算符和getline函数分别是如何处理空白字符的。

解：

- 类似`is >> s`的读取：string对象会忽略开头的空白并从第一个真正的字符开始，直到遇见下一**空白**为止。
- 类似`getline(is, s)`的读取：string对象会从输入流中读取字符，直到遇见**换行符**为止。


## 练习3.4
编写一段程序读取两个字符串，比较其是否相等并输出结果。如果不相等，输出比较大的那个字符串。改写上述程序，比较输入的两个字符串是否等长，如果不等长，输出长度较大的那个字符串。

解：

比较大的

```cpp
#include <iostream>
#include <string>
using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string str1, str2;
	while (cin >> str1 >> str2)
	{
		if (str1 == str2)
			cout << "The two strings are equal." << endl;
		else
			cout << "The larger string is " << ((str1 > str2) ? str1 : str2);
	}

	return 0;
}
```

长度大的

```cpp
#include <iostream>
#include <string>
using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string str1, str2;
	while (cin >> str1 >> str2)
	{
		if (str1.size() == str2.size())
			cout << "The two strings have the same length." << endl;
		else
			cout << "The longer string is " << ((str1.size() > str2.size()) ? str1 : str2) << endl;
	}

	return 0;
}
```

## 练习3.5
编写一段程序从标准输入中读入多个字符串并将他们连接起来，输出连接成的大字符串。然后修改上述程序，用空格把输入的多个字符串分割开来。

解：

未隔开的：
```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string result, s;
	while (cin >> s)
	{
		result += s;
	}
	cout << result << endl;

	return 0;
}
```

隔开的：
```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string result, s;
	while (cin >> s)
	{
		result += s + " ";
	}
	cout << result << endl;

	return 0;
}
```

## 练习3.6
编写一段程序，使用范围for语句将字符串内所有字符用X代替。

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string s = "this is a string";

	for (auto &x : s)
	{
		x = 'X';
	}

	cout << s << endl;
	return 0;
}
```

## 练习3.7
就上一题完成的程序而言，如果将循环控制的变量设置为char将发生什么？先估计一下结果，然后实际编程进行验证。

解：

如果设置为char，那么原来的字符串不会发生改变。

## 练习3.8
分别用while循环和传统for循环重写第一题的程序，你觉得哪种形式更好呢？为什么？

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string s = "this is a string";

	decltype(s.size()) i = 0;
	while (i != s.size())
	{
		s[i] = 'X';
		++i;
	}
	cout << s << endl;
	for (i = 0; i != s.size(); ++i)
	{
		s[i] = 'Y';
	}
	cout << s << endl;
	return 0;
}
```

范围for语句更好，不直接操作索引，更简洁。

## 练习3.9
下面的程序有何作用？它合法吗？如果不合法？为什么？
```cpp
string s;
cout << s[0] << endl;
```

解：

不合法。使用下标访问空字符串是非法的行为。

## 练习3.10
编写一段程序，读入一个包含标点符号的字符串，将标点符号去除后输出字符串剩余的部分。

解：
```cpp
#include <iostream>
#include <string>
#include <cctype>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string s = "this, is. a :string!";
	string result;

	for (auto x : s)
	{
		if (!ispunct(x))
		{
			result += x;
		}
	}
	
	cout << result << endl;
	return 0;
}
```

## 练习3.11
下面的范围for语句合法吗？如果合法，c的类型是什么？

```cpp
const string s = "Keep out!";
for(auto &c : s){ /* ... */ }
```

解：

要根据for循环中的代码来看是否合法，c是string 对象中字符的引用，s是常量。因此如果for循环中的代码重新给c赋值就会非法，如果不改变c的值，那么合法。

## 练习3.12
下列vector对象的定义有不正确的吗？如果有，请指出来。对于正确的，描述其执行结果；对于不正确的，说明其错误的原因。

```cpp
vector<vector<int>> ivec;         // 在C++11当中合法
vector<string> svec = ivec;       // 不合法，类型不一样
vector<string> svec(10, "null");  // 合法
```

## 练习3.13
下列的vector对象各包含多少个元素？这些元素的值分别是多少？

```cpp
vector<int> v1;         // size:0,  no values.
vector<int> v2(10);     // size:10, value:0
vector<int> v3(10, 42); // size:10, value:42
vector<int> v4{ 10 };     // size:1,  value:10
vector<int> v5{ 10, 42 }; // size:2,  value:10, 42
vector<string> v6{ 10 };  // size:10, value:""
vector<string> v7{ 10, "hi" };  // size:10, value:"hi"
```

## 练习3.14
编写一段程序，用cin读入一组整数并把它们存入一个vector对象。

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;

int main()
{
	vector<int> v;
	int i;
	while (cin >> i)
	{
		v.push_back(i);
	}
	return 0;
}
```

## 练习3.15
改写上题程序，不过这次读入的是字符串。

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<string> v;
	string i;
	while (cin >> i)
	{
		v.push_back(i);
	}
	return 0;
}
```

## 练习3.16
编写一段程序，把练习3.13中vector对象的容量和具体内容输出出来

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<int> v1;         // size:0,  no values.
	vector<int> v2(10);     // size:10, value:0
	vector<int> v3(10, 42); // size:10, value:42
	vector<int> v4{ 10 };     // size:1,  value:10
	vector<int> v5{ 10, 42 }; // size:2,  value:10, 42
	vector<string> v6{ 10 };  // size:10, value:""
	vector<string> v7{ 10, "hi" };  // size:10, value:"hi"

	cout << "v1 size :" << v1.size() << endl;
	cout << "v2 size :" << v2.size() << endl;
	cout << "v3 size :" << v3.size() << endl;
	cout << "v4 size :" << v4.size() << endl;
	cout << "v5 size :" << v5.size() << endl;
	cout << "v6 size :" << v6.size() << endl;
	cout << "v7 size :" << v7.size() << endl;

	cout << "v1 content: ";
	for (auto i : v1)
	{
		cout << i << " , ";
	}
	cout << endl;

	cout << "v2 content: ";
	for (auto i : v2)
	{
		cout << i << " , ";
	}
	cout << endl;

	cout << "v3 content: ";
	for (auto i : v3)
	{
		cout << i << " , ";
	}
	cout << endl;

	cout << "v4 content: ";
	for (auto i : v4)
	{
		cout << i << " , ";
	}
	cout << endl;

	cout << "v5 content: ";
	for (auto i : v5)
	{
		cout << i << " , ";
	}
	cout << endl;

	cout << "v6 content: ";
	for (auto i : v6)
	{
		cout << i << " , ";
	}
	cout << endl;

	cout << "v7 content: ";
	for (auto i : v7)
	{
		cout << i << " , ";
	}
	cout << endl;
	return 0;
}
```

## 练习3.17
从cin读入一组词并把它们存入一个vector对象，然后设法把所有词都改为大写形式。输出改变后的结果，每个词占一行。

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<string> v;
	string s;

	while (cin >> s)
	{
		v.push_back(s);
	}

	for (auto &str : v)
	{
		for (auto &c : str)
		{
			c = toupper(c);
		}
	}

	for (auto i : v)
	{
		cout << i << endl;
	}
	return 0;
}
```

## 练习3.18
下面的程序合法吗？如果不合法，你准备如何修改？

```cpp
vector<int> ivec;
ivec[0] = 42;
```

解：

不合法。应改为：
```cpp
ivec.push_back(42);
```

## 练习3.19
如果想定义一个含有10个元素的vector对象，所有元素的值都是42，请例举三种不同的实现方法，哪种方式更好呢？

如下三种：

```cpp
vector<int> ivec1(10, 42);
vector<int> ivec2{ 42, 42, 42, 42, 42, 42, 42, 42, 42, 42 };
vector<int> ivec3;
for (int i = 0; i < 10; ++i)
	ivec3.push_back(42);
```
第一种方式最好。

## 练习3.20
读入一组整数并把他们存入一个vector对象，将每对相邻整数的和输出出来。改写你的程序，这次要求先输出第一个和最后一个元素的和，接着输出第二个和倒数第二个元素的和，以此类推。

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<int> ivec;
	int i;
	while (cin >> i)
	{
		ivec.push_back(i);
	}

	for (int i = 0; i < ivec.size() - 1; ++i)
	{
		cout << ivec[i] + ivec[i + 1] << endl;
	}
	
	//---------------------------------
	cout << "---------------------------------" << endl;
	
	int m = 0;
	int n = ivec.size() - 1;
	while (m < n)
	{
		cout << ivec[m] + ivec[n] << endl;
		++m;
		--n;
	}
	return 0;
}
```

## 练习3.21
请使用迭代器重做3.3.3节的第一个练习。

解：

```cpp
#include <vector>
#include <iterator>
#include <string>
#include <iostream>

using std::vector;
using std::string;
using std::cout;
using std::endl;

void check_and_print(const vector<int>& vec)
{
	cout << "size: " << vec.size() << "  content: [";
	for (auto it = vec.begin(); it != vec.end(); ++it)
		cout << *it << (it != vec.end() - 1 ? "," : "");
	cout << "]\n" << endl;
}

void check_and_print(const vector<string>& vec)
{

	cout << "size: " << vec.size() << "  content: [";
	for (auto it = vec.begin(); it != vec.end(); ++it)
		cout << *it << (it != vec.end() - 1 ? "," : "");
	cout << "]\n" << endl;
}

int main()
{
	vector<int> v1;
	vector<int> v2(10);
	vector<int> v3(10, 42);
	vector<int> v4{ 10 };
	vector<int> v5{ 10, 42 };
	vector<string> v6{ 10 };
	vector<string> v7{ 10, "hi" };

	check_and_print(v1);
	check_and_print(v2);
	check_and_print(v3);
	check_and_print(v4);
	check_and_print(v5);
	check_and_print(v6);
	check_and_print(v7);

	return 0;
}
```
   
## 练习3.22
修改之前那个输出text第一段的程序，首先把text的第一段全部改成大写形式，然后输出它。
   
解： 略   

## 练习3.23
编写一段程序，创建一个含有10个整数的vector对象，然后使用迭代器将所有元素的值都变成原来的两倍。输出vector对象的内容，检验程序是否正确。

解：

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
	vector<int> v(10, 1);
    for (auto it=v.begin(); it!=v.end(); it++){
        *it *= 2;
    }
    for (auto one : v){
        cout << one <<endl;
    }
	return 0;
}
```

## 练习3.24
请使用迭代器重做3.3.3节的最后一个练习。

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<int> ivec;
	int i;
	while (cin >> i)
	{
		ivec.push_back(i);
	}

	for (auto it = ivec.begin(); it != ivec.end() - 1; ++it)
	{
		cout << *it + *(it + 1) << endl;
	}

	//---------------------------------
	cout << "---------------------------------" << endl;

	auto it1 = ivec.begin();
	auto it2 = ivec.end() - 1;
	while (it1 < it2)
	{
		cout << *it1 + *it2 << endl;
		++it1;
		--it2;
	}
	return 0;
}
```

## 练习3.25
3.3.3节划分分数段的程序是使用下标运算符实现的，请利用迭代器改写该程序实现完全相同的功能。

解：

```cpp
#include <vector>
#include <iostream>

using std::vector; using std::cout; using std::cin; using std::endl;

int main()
{
	vector<unsigned> scores(11, 0);
	unsigned grade;
	while (cin >> grade)
	{
		if (grade <= 100)
			++*(scores.begin() + grade / 10);
	}

	for (auto s : scores)
		cout << s << " ";
	cout << endl;

	return 0;
}
```

## 练习3.26
在100页的二分搜索程序中，为什么用的是 `mid = beg + (end - beg) / 2`, 而非 `mid = (beg + end) / 2 ;` ?

解：

因为两个迭代器相互之间支持的运算只有 `-` ，而没有 `+` 。
但是迭代器和迭代器差值（整数值）之间支持 `+`。

## 练习3.27
假设`txt_size`是一个无参函数，它的返回值是`int`。请回答下列哪个定义是非法的，为什么？
```cpp
unsigned buf_size = 1024;
(a) int ia[buf_size];
(b) int ia[4 * 7 - 14];
(c) int ia[txt_size()];
(d) char st[11] = "fundamental";
```

解：
- (a) 非法。维度必须是一个常量表达式。
- (b) 合法。
- (c) 非法。txt_size() 的值必须要到运行时才能得到。
- (d) 非法。数组的大小应该是12。

## 练习3.28
下列数组中元素的值是什么？
```cpp
string sa[10];
int ia[10];
int main() {
	string sa2[10];
	int ia2[10];
}
```

解：

数组的元素会被默认初始化。
`sa`的元素值全部为空字符串，`ia` 的元素值全部为0。
`sa2`的元素值全部为空字符串，`ia2`的元素值全部未定义。

## 练习3.29
相比于vector 来说，数组有哪些缺点，请例举一些。

解：

- 数组的大小是确定的。
- 不能随意增加元素。
- 不允许拷贝和赋值。

## 练习3.30
指出下面代码中的索引错误。

```cpp
constexpr size_t array_size = 10;
int ia[array_size];
for (size_t ix = 1; ix <= array_size; ++ix)
	ia[ix] = ix;
```

解：

当`ix`增长到 10 的时候，`ia[ix]`的下标越界。

## 练习3.31
编写一段程序，定义一个含有10个int的数组，令每个元素的值就是其下标值。

```cpp
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int arr[10];
    for (auto i = 0; i < 10; ++i) arr[i] = i;
    for (auto i : arr) cout << i << " ";
    cout << endl;

    return 0;
}
```

## 练习3.32
将上一题刚刚创建的数组拷贝给另一数组。利用vector重写程序，实现类似的功能。

```cpp
#include <iostream>
#include <vector>
using std::cout; using std::endl; using std::vector;

int main()
{
    // array
    int arr[10];
    for (int i = 0; i < 10; ++i) arr[i] = i;
    int arr2[10];
    for (int i = 0; i < 10; ++i) arr2[i] = arr[i];

    // vector
    vector<int> v(10);
    for (int i = 0; i != 10; ++i) v[i] = arr[i];
    vector<int> v2(v);
    for (auto i : v2) cout << i << " ";
    cout << endl;

    return 0;
}
```

## 练习3.33
对于104页的程序来说，如果不初始化scores将会发生什么？

解：

数组中所有元素的值将会未定义。

## 练习3.34
假定`p1` 和 `p2` 都指向同一个数组中的元素，则下面程序的功能是什么？什么情况下该程序是非法的？
```cpp
p1 += p2 - p1;
```

解：

将 `p1` 移动到 `p2` 的位置。任何情况下都合法。

## 练习3.35
编写一段程序，利用指针将数组中的元素置为0。

解：

```cpp
#include <iostream>
using std::cout; using std::endl;

int main()
{
    const int size = 10;
    int arr[size];
    for (auto ptr = arr; ptr != arr + size; ++ptr) *ptr = 0;

    for (auto i : arr) cout << i << " ";
    cout << endl;

    return 0;
}
```

## 练习3.36
编写一段程序，比较两个数组是否相等。再写一段程序，比较两个vector对象是否相等。

解：

```cpp
#include <iostream>
#include <vector>
#include <iterator>

using std::begin; using std::end; using std::cout; using std::endl; using std::vector;

// pb point to begin of the array, pe point to end of the array.
bool compare(int* const pb1, int* const pe1, int* const pb2, int* const pe2)
{
    if ((pe1 - pb1) != (pe2 - pb2)) // have different size.
        return false;
    else
    {
        for (int* i = pb1, *j = pb2; (i != pe1) && (j != pe2); ++i, ++j)
            if (*i != *j) return false;
    }

    return true;
}

int main()
{
    int arr1[3] = { 0, 1, 2 };
    int arr2[3] = { 0, 2, 4 };

    if (compare(begin(arr1), end(arr1), begin(arr2), end(arr2)))
        cout << "The two arrays are equal." << endl;
    else
        cout << "The two arrays are not equal." << endl;

    cout << "==========" << endl;

    vector<int> vec1 = { 0, 1, 2 };
    vector<int> vec2 = { 0, 1, 2 };

    if (vec1 == vec2)
        cout << "The two vectors are equal." << endl;
    else
        cout << "The two vectors are not equal." << endl;

    return 0;
}
```

## 练习3.37
下面的程序是何含义，程序的输出结果是什么？
```cpp
const char ca[] = { 'h', 'e', 'l', 'l', 'o' };
const char *cp = ca;
while (*cp) {
    cout << *cp << endl;
    ++cp;
}
```

解：

会将ca 字符数组中的元素打印出来。但是因为没有空字符的存在，程序不会退出循环。

## 练习3.38
在本节中我们提到，将两个指针相加不但是非法的，而且也没有什么意义。请问为什么两个指针相加没有意义？

解：

将两个指针相减可以表示两个指针(在同一数组中)相距的距离，将指针加上一个整数也可以表示移动这个指针到某一位置。但是两个指针相加并没有逻辑上的意义，因此两个指针不能相加。

## 练习3.39
编写一段程序，比较两个 `string` 对象。再编写一段程序，比较两个C风格字符串的内容。

解：

```cpp
#include <iostream>
#include <string>
#include <cstring>
using std::cout; using std::endl; using std::string;

int main()
{
    // use string.
    string s1("Mooophy"), s2("Pezy");
    if (s1 == s2)
        cout << "same string." << endl;
    else if (s1 > s2)
        cout << "Mooophy > Pezy" << endl;
    else
        cout << "Mooophy < Pezy" << endl;

    cout << "=========" << endl;

    // use C-Style character strings.
    const char* cs1 = "Wangyue";
    const char* cs2 = "Pezy";
    auto result = strcmp(cs1, cs2);
    if (result == 0)
        cout << "same string." << endl;
    else if (result < 0)
        cout << "Wangyue < Pezy" << endl;
    else
        cout << "Wangyue > Pezy" << endl;

    return 0;
}
```

## 练习3.40
编写一段程序，定义两个字符数组并用字符串字面值初始化它们；接着再定义一个字符数组存放前面两个数组连接后的结果。使用`strcpy`和`strcat`把前两个数组的内容拷贝到第三个数组当中。

解：

```cpp
#include <iostream>
#include <cstring>

const char cstr1[]="Hello";
const char cstr2[]="world!";

int main()
{
    constexpr size_t new_size = strlen(cstr1) + strlen(" ") + strlen(cstr2) +1;
    char cstr3[new_size];
    
    strcpy(cstr3, cstr1);
    strcat(cstr3, " ");
    strcat(cstr3, cstr2);
    
    std::cout << cstr3 << std::endl;
}
```

## 练习3.41
编写一段程序，用整型数组初始化一个vector对象。

```cpp
#include <iostream>
#include <vector>
using std::vector; using std::cout; using std::endl; using std::begin; using std::end;

int main()
{
    int arr[] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    vector<int> v(begin(arr), end(arr));

    for (auto i : v) cout << i << " ";
    cout << endl;

    return 0;
}
```

## 练习3.42
编写一段程序，将含有整数元素的 `vector` 对象拷贝给一个整型数组。

解：

```cpp
#include <iostream>
#include <vector>
using std::vector; using std::cout; using std::endl; using std::begin; using std::end;

int main()
{
    vector<int> v{ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    int arr[10];
    for (int i = 0; i != v.size(); ++i) arr[i] = v[i];

    for (auto i : arr) cout << i << " ";
    cout << endl;

    return 0;
}
```

## 练习3.43
编写3个不同版本的程序，令其均能输出`ia`的元素。
版本1使用范围`for`语句管理迭代过程；版本2和版本3都使用普通`for`语句，其中版本2要求使用下标运算符，版本3要求使用指针。
此外，在所有3个版本的程序中都要直接写出数据类型，而不能使用类型别名、`auto`关键字和`decltype`关键字。

解：

```cpp
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int arr[3][4] = 
    { 
        { 0, 1, 2, 3 },
        { 4, 5, 6, 7 },
        { 8, 9, 10, 11 }
    };

    // range for
    for (const int(&row)[4] : arr)
        for (int col : row) cout << col << " ";
    cout << endl;

    // for loop
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j) cout << arr[i][j] << " ";
    cout << endl;

    // using pointers.
    for (int(*row)[4] = arr; row != arr + 3; ++row)
        for (int *col = *row; col != *row + 4; ++col) cout << *col << " ";
    cout << endl;

    return 0;
}
```

## 练习3.44
改写上一个练习中的程序，使用类型别名来代替循环控制变量的类型。

解：

```cpp
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int ia[3][4] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 };

    // a range for to manage the iteration
    // use type alias
    using int_array = int[4];
    for (int_array& p : ia)
        for (int q : p)
            cout << q << " ";
    cout << endl;

    // ordinary for loop using subscripts
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j)
            cout << ia[i][j] << " ";
    cout << endl;

    // using pointers.
    // use type alias
    for (int_array* p = ia; p != ia + 3; ++p)
        for (int *q = *p; q != *p + 4; ++q)
            cout << *q << " ";
    cout << endl;

    return 0;
}
```

## 练习3.45
再一次改写程序，这次使用 `auto` 关键字。

解：

```cpp
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int ia[3][4] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 };

    // a range for to manage the iteration
    for (auto& p : ia)
        for (int q : p)
            cout << q << " ";
    cout << endl;

    // ordinary for loop using subscripts
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j)
            cout << ia[i][j] << " ";
    cout << endl;

    // using pointers.
    for (auto p = ia; p != ia + 3; ++p)
        for (int *q = *p; q != *p + 4; ++q)
            cout << *q << " ";
    cout << endl;

    return 0;
}
```

# 第四章 表达式

## 练习4.1

表达式`5 + 10 * 20 / 2`的求值结果是多少？

解：

等价于`5 + ((10 * 20) / 2) = 105`

## 练习4.2

根据4.12节中的表，在下述表达式的合理位置添加括号，使得添加括号后运算对象的组合顺序与添加括号前一致。
(a) `*vec.begin()`
(b) `*vec.begin() + 1`

解：

```cpp
*(vec.begin())
(*(vec.begin())) + 1
```

## 练习4.3

C++语言没有明确规定大多数二元运算符的求值顺序，给编译器优化留下了余地。这种策略实际上是在代码生成效率和程序潜在缺陷之间进行了权衡，你认为这可以接受吗？请说出你的理由。

解：

可以接受。C++的设计思想是尽可能地“相信”程序员，将效率最大化。然而这种思想却有着潜在的危害，就是无法控制程序员自身引发的错误。因此 Java 的诞生也是必然，Java的思想就是尽可能地“不相信”程序员。

## 练习4.4
在下面的表达式中添加括号，说明其求值过程及最终结果。编写程序编译该（不加括号的）表达式并输出结果验证之前的推断。

`12 / 3 * 4 + 5 * 15 + 24 % 4 / 2`

解：

`((12 / 3) * 4) + (5 * 15) + ((24 % 4) / 2) = 16 + 75 + 0 = 91`

## 练习4.5

写出下列表达式的求值结果。
```cpp
-30 * 3 + 21 / 5  // -90+4 = -86
-30 + 3 * 21 / 5  // -30+63/5 = -30+12 = -18
30 / 3 * 21 % 5   // 10*21%5 = 210%5 = 0
-30 / 3 * 21 % 4  // -10*21%4 = -210%4 = -2
```

## 练习4.6

写出一条表达式用于确定一个整数是奇数还是偶数。

解：

```cpp
if (i % 2 == 0) /* ... */
```
或者
```cpp
if (i & 0x1) /* ... */
```

## 练习4.7

溢出是何含义？写出三条将导致溢出的表达式。

解：

当计算的结果超出该类型所能表示的范围时就会产生溢出。

```cpp
short svalue = 32767; ++svalue; // -32768
unsigned uivalue = 0; --uivalue;  // 4294967295
unsigned short usvalue = 65535; ++usvalue;  // 0
```

## 练习4.8

说明在逻辑与、逻辑或及相等性运算符中运算对象的求值顺序。

解：

- 逻辑与运算符和逻辑或运算符都是先求左侧运算对象的值再求右侧运算对象的值，当且仅当左侧运算对象无法确定表达式的结果时才会计算右侧运算对象的值。这种策略称为 **短路求值**。
- 相等性运算符未定义求值顺序。

## 练习4.9

解释在下面的`if`语句中条件部分的判断过程。
```cpp
const char *cp = "Hello World";
if (cp && *cp)
```

解：

首先判断`cp`，`cp` 不是一个空指针，因此`cp`为真。然后判断`*cp`，`*cp` 的值是字符`'H'`，非0。因此最后的结果为真。

## 练习4.10

为`while`循环写一个条件，使其从标准输入中读取整数，遇到`42`时停止。

解：

```cpp
int i;
while(cin >> i && i != 42)
```

## 练习4.11

书写一条表达式用于测试4个值a、b、c、d的关系，确保a大于b、b大于c、c大于d。

解：

```cpp
a>b && b>c && c>d
```

## 练习4.12

假设`i`、`j`和`k`是三个整数，说明表达式`i != j < k`的含义。

解：

这个表达式等于`i != (j < k)`。首先得到`j < k`的结果为`true`或`false`，转换为整数值是`1`或`0`，然后判断`i`不等于`1`或`0` ，最终的结果为`bool`值。

## 练习4.13

在下述语句中，当赋值完成后 i 和 d 的值分别是多少？

```cpp
int i;   double d;
d = i = 3.5; // i = 3, d = 3.0
i = d = 3.5; // d = 3.5, i = 3
```

## 练习4.14

执行下述 if 语句后将发生什么情况？

```cpp
if (42 = i)   // 编译错误。赋值运算符左侧必须是一个可修改的左值。而字面值是右值。
if (i = 42)   // true.
```

## 练习4.15

下面的赋值是非法的，为什么？应该如何修改？

```cpp
double dval; int ival; int *pi;
dval = ival = pi = 0;
```

解：
`p`是指针，不能赋值给`int`，应该改为：

```cpp
dval = ival = 0;
pi = 0;
```

## 练习4.16

尽管下面的语句合法，但它们实际执行的行为可能和预期并不一样，为什么？应该如何修改？

```cpp
if (p = getPtr() != 0)
if (i = 1024)
```

解：

```cpp
if ((p=getPtr()) != 0)
if (i == 1024)
```

## 练习4.17

说明前置递增运算符和后置递增运算符的区别。

解：

前置递增运算符将对象本身作为左值返回，而后置递增运算符将对象原始值的副本作为右值返回。

## 练习4.18

如果132页那个输出`vector`对象元素的`while`循环使用前置递增运算符，将得到什么结果？

解：

将会从第二个元素开始取值，并且最后对`v.end()`进行取值，结果是未定义的。

## 练习4.19

假设`ptr`的类型是指向`int`的指针、`vec`的类型是`vector`、`ival`的类型是`int`，说明下面的表达式是何含义？如果有表达式不正确，为什么？应该如何修改？

```cpp
(a) ptr != 0 && *ptr++  
(b) ival++ && ival
(c) vec[ival++] <= vec[ival] 
```

解：

- (a) 判断`ptr`不是一个空指针，并且`ptr`当前指向的元素的值也为真，然后将`ptr`指向下一个元素
- (b) 判断`ival`的值为真，并且`(ival + 1)`的值也为真
- (c) 表达式有误。C++并没有规定`<=`运算符两边的求值顺序，应该改为`vec[ival] <= vec[ival+1]`

## 练习4.20

假设`iter`的类型是`vector::iterator`, 说明下面的表达式是否合法。如果合法，表达式的含义是什么？如果不合法，错在何处？

```cpp
(a) *iter++;
(b) (*iter)++;
(c) *iter.empty();
(d) iter->empty();
(e) ++*iter;
(f) iter++->empty();
```

解：

- (a)合法。返回迭代器所指向的元素，然后迭代器递增。
- (b)不合法。因为`vector`元素类型是`string`，没有`++`操作。
- (c)不合法。这里应该加括号。
- (d)合法。判断迭代器当前的元素是否为空。
- (e)不合法。`string`类型没有`++`操作。
- (f)合法。判断迭代器当前元素是否为空，然后迭代器递增。

## 练习4.21

编写一段程序，使用条件运算符从`vector`中找到哪些元素的值是奇数，然后将这些奇数值翻倍。

解：

```cpp
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
	vector<int> ivec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };

	for (auto &i : ivec)
	{
		cout << ((i & 0x1) ? i * 2 : i) << " ";
	}
	cout << endl;

	return 0;
}
```

## 练习4.22

本节的示例程序将成绩划分为`high pass`、`pass` 和 `fail` 三种，扩展该程序使其进一步将 60 分到 75 分之间的成绩设定为`low pass`。要求程序包含两个版本：一个版本只使用条件运算符；另一个版本使用1个或多个`if`语句。哪个版本的程序更容易理解呢？为什么？

解：

```cpp
#include <iostream>
using std::cout; using std::cin; using std::endl;

int main()
{
	for (unsigned g; cin >> g;)
	{
		auto result = g > 90 ? "high pass" : g < 60 ? "fail" : g < 75 ? "low pass" : "pass";
		cout << result << endl;

		// -------------------------
		if (g > 90)         cout << "high pass";
		else if (g < 60)    cout << "fail";
		else if (g < 75)    cout << "low pass";
		else                cout << "pass";
		cout << endl;
	}

	return 0;
}
```
第二个版本容易理解。当条件运算符嵌套层数变多之后，代码的可读性急剧下降。而`if else`的逻辑很清晰。

## 练习4.23

因为运算符的优先级问题，下面这条表达式无法通过编译。根据4.12节中的表指出它的问题在哪里？应该如何修改？

```cpp
string s = "word";
string pl = s + s[s.size() - 1] == 's' ? "" : "s" ;
```

解：

加法运算符的优先级高于条件运算符。因此要改为：

```cpp
string pl = s + (s[s.size() - 1] == 's' ? "" : "s") ;
```

## 练习4.24

本节的示例程序将成绩划分为`high pass`、`pass`、和`fail`三种，它的依据是条件运算符满足右结合律。假如条件运算符满足的是左结合律，求值的过程将是怎样的？

解：

如果条件运算符满足的是左结合律。那么

`finalgrade = (grade > 90) ? "high pass" : (grade < 60) ? "fail" : "pass";`
等同于
`finalgrade = ((grade > 90) ? "high pass" : (grade < 60)) ? "fail" : "pass";`
假如此时 `grade > 90` ，第一个条件表达式的结果是 `"high pass"` ，而字符串字面值的类型是 `const char *`，非空所以为真。因此第二个条件表达式的结果是 `"fail"`。这样就出现了自相矛盾的逻辑。

## 练习4.25

如果一台机器上`int`占32位、`char`占8位，用的是`Latin-1`字符集，其中字符`'q'` 的二进制形式是`01110001`，那么表达式`~'q' << 6`的值是什么？

解：

首先将`char`类型提升为`int`类型，即`00000000 00000000 00000000 01110001`，然后取反，再左移6位，结果是-7296。

## 练习4.26

在本节关于测验成绩的例子中，如果使用`unsigned int` 作为`quiz1` 的类型会发生什么情况？

解：

在有的机器上，`unsigned int` 类型可能只有 16 位，因此结果是未定义的。

## 练习4.27

下列表达式的结果是什么？
```cpp
unsigned long ul1 = 3, ul2 = 7;
(a) ul1 & ul2 
(b) ul1 | ul2 
(c) ul1 && ul2
(d) ul1 || ul2 
```

解：

- (a) 3
- (b) 7
- (c) true
- (d) ture

## 练习4.28

编写一段程序，输出每一种内置类型所占空间的大小。

解：

```cpp
#include <iostream> 

using namespace std;

int main()
{
	cout << "bool:\t\t" << sizeof(bool) << " bytes" << endl << endl;

	cout << "char:\t\t" << sizeof(char) << " bytes" << endl;
	cout << "wchar_t:\t" << sizeof(wchar_t) << " bytes" << endl;
	cout << "char16_t:\t" << sizeof(char16_t) << " bytes" << endl;
	cout << "char32_t:\t" << sizeof(char32_t) << " bytes" << endl << endl;

	cout << "short:\t\t" << sizeof(short) << " bytes" << endl;
	cout << "int:\t\t" << sizeof(int) << " bytes" << endl;
	cout << "long:\t\t" << sizeof(long) << " bytes" << endl;
	cout << "long long:\t" << sizeof(long long) << " bytes" << endl << endl;

	cout << "float:\t\t" << sizeof(float) << " bytes" << endl;
	cout << "double:\t\t" << sizeof(double) << " bytes" << endl;
	cout << "long double:\t" << sizeof(long double) << " bytes" << endl << endl;

	return 0;
}
```

输出：

```
bool:           1 bytes

char:           1 bytes
wchar_t:        4 bytes
char16_t:       2 bytes
char32_t:       4 bytes

short:          2 bytes
int:            4 bytes
long:           8 bytes
long long:      8 bytes

float:          4 bytes
double:         8 bytes
long double:    16 bytes
```

## 练习4.29

推断下面代码的输出结果并说明理由。实际运行这段程序，结果和你想象的一样吗？如不一样，为什么？

```cpp
int x[10];   int *p = x;
cout << sizeof(x)/sizeof(*x) << endl;
cout << sizeof(p)/sizeof(*p) << endl;
```

解：

第一个输出结果是 10。第二个结果1,此处用法不合理不是未定义，参考https://www.geeksforgeeks.org/using-sizof-operator-with-array-paratmeters/。

## 练习4.30

根据4.12节中的表，在下述表达式的适当位置加上括号，使得加上括号之后的表达式的含义与原来的含义相同。

```cpp
(a) sizeof x + y      
(b) sizeof p->mem[i]  
(c) sizeof a < b     
(d) sizeof f() 
```

解：

```cpp
(a) (sizeof x) + y
(b) sizeof(p->mem[i])
(c) sizeof(a) < b
(d) sizeof(f())
```

## 练习4.31

本节的程序使用了前置版本的递增运算符和递减运算符，解释为什么要用前置版本而不用后置版本。要想使用后置版本的递增递减运算符需要做哪些改动？使用后置版本重写本节的程序。

解：

在4.5节（132页）已经说过了，除非必须，否则不用递增递减运算符的后置版本。在这里要使用后者版本的递增递减运算符不需要任何改动。

## 练习4.32
解释下面这个循环的含义。

```cpp
constexpr int size = 5;
int ia[size] = { 1, 2, 3, 4, 5 };
for (int *ptr = ia, ix = 0;
    ix != size && ptr != ia+size;
    ++ix, ++ptr) { /* ... */ }
```

解：

这个循环在遍历数组`ia`，指针`ptr`和整型`ix`都是起到一个循环计数的功能。

## 练习4.33

根据4.12节中的表说明下面这条表达式的含义。

```cpp
someValue ? ++x, ++y : --x, --y
```

解：

逗号表达式的优先级是最低的。因此这条表达式也等于：
```cpp
(someValue ? ++x, ++y : --x), --y
```
如果`someValue`的值为真，`x` 和 `y` 的值都自增并返回 `y` 值，然后丢弃`y`值，`y`递减并返回`y`值。如果`someValue`的值为假，`x` 递减并返回`x` 值，然后丢弃`x`值，`y`递减并返回`y`值。



## 练习4.34

根据本节给出的变量定义，说明在下面的表达式中将发生什么样的类型转换：

```cpp
(a) if (fval)
(b) dval = fval + ival;
(c) dval + ival * cval;
```

需要注意每种运算符遵循的是左结合律还是右结合律。

解：

```cpp
(a) fval 转换为 bool 类型
(b) ival 转换为 float ，相加的结果转换为 double
(c) cval 转换为 int，然后相乘的结果转换为 double
```

## 练习4.35

假设有如下的定义：

```cpp
char cval;
int ival;
unsigned int ui;
float fval;
double dval;
```

请回答在下面的表达式中发生了隐式类型转换吗？如果有，指出来。

```cpp
(a) cval = 'a' + 3;
(b) fval = ui - ival * 1.0;
(c) dval = ui * fval;
(d) cval = ival + fval + dval;
```

解：

- (a) `'a'` 转换为 `int` ，然后与 `3` 相加的结果转换为 `char`
- (b) `ival` 转换为 `double`，`ui` 转换为 `double`，结果转换为 `float`
- (c) `ui` 转换为 `float`，结果转换为 `double`
- (d) `ival` 转换为 `float`，与`fval`相加后的结果转换为 `double`，最后的结果转换为`char`

## 练习4.36

假设 `i` 是`int`类型，`d` 是`double`类型，书写表达式 `i*=d` 使其执行整数类型的乘法而非浮点类型的乘法。

解：

```cpp
i *= static_cast<int>(d);
```

## 练习4.37

练习4.37
用命名的强制类型转换改写下列旧式的转换语句。

```cpp
int i; double d; const string *ps; char *pc; void *pv;
(a) pv = (void*)ps;
(b) i = int(*pc);
(c) pv = &d;
(d) pc = (char*)pv;
```

解：

```cpp
(a) pv = static_cast<void*>(const_cast<string*>(ps));
(b) i = static_cast<int>(*pc);
(c) pv = static_cast<void*>(&d);
(d) pc = static_cast<char*>(pv);
```

## 练习4.38
说明下面这条表达式的含义。

```cpp
double slope = static_cast<double>(j/i);
```

解：

将`j/i`的结果值转换为`double`，然后赋值给`slope`。




# 第五章 语句

## 练习5.1

什么是空语句？什么时候会用到空语句？

解：

只含义一个单独的分号的语句是空语句。如：`;`。

如果在程序的某个地方，语法上需要一条语句但是逻辑上不需要，此时应该使用空语句。
```cpp
while (cin >> s && s != sought)
	;
```

## 练习5.2

什么是块？什么时候会用到块？

解：

用花括号括起来的语句和声明的序列就是块。
```cpp
{
	// ...
}
```
如果在程序的某个地方，语法上需要一条语句，而逻辑上需要多条语句，此时应该使用块
```cpp
while (val <= 10) {
	sum += val;
	++val;
}
```

## 练习5.3
使用逗号运算符重写1.4.1节的`while`循环，使它不再需要块，观察改写之后的代码可读性提高了还是降低了。

```cpp
while (val <= 10)
    sum += val, ++val;
```
代码的可读性反而降低了。

## 练习5.4

说明下列例子的含义，如果存在问题，试着修改它。

```cpp
(a) while (string::iterator iter != s.end()) { /* . . . */ }

(b) while (bool status = find(word)) { /* . . . */ }
		if (!status) { /* . . . */ }
```

解：

- (a) 这个循环试图用迭代器遍历`string`，但是变量的定义应该放在循环的外面，目前每次循环都会重新定义一个变量，明显是错误的。
- (b) 这个循环的`while`和`if`是两个独立的语句，`if`语句中无法访问`status`变量，正确的做法是应该将`if`语句包含在`while`里面。

## 练习5.5
写一段自己的程序，使用`if else`语句实现把数字转换为字母成绩的要求。

```cpp
#include <iostream>
#include <vector>
#include <string>
using std::vector; using std::string; using std::cout; using std::endl; using std::cin;

int main()
{
    vector<string> scores = { "F", "D", "C", "B", "A", "A++" };
    for (int g; cin >> g;)
    {
        string letter;
        if (g < 60)
        {
            letter = scores[0];
        }
        else
        {
            letter = scores[(g - 50) / 10];
            if (g != 100)
                letter += g % 10 > 7 ? "+" : g % 10 < 3 ? "-" : "";
        }
        cout << letter << endl;
    }

    return 0;
}
```

## 练习5.6

改写上一题的程序，使用条件运算符代替`if else`语句。

```cpp
#include <iostream>
#include <vector>
#include <string>
using std::vector; using std::string; using std::cout; using std::endl; using std::cin;

int main()
{
    vector<string> scores = { "F", "D", "C", "B", "A", "A++" };

    int grade = 0;
    while (cin >> grade)
    {
        string lettergrade = grade < 60 ? scores[0] : scores[(grade - 50) / 10];
        lettergrade += (grade == 100 || grade < 60) ? "" : (grade % 10 > 7) ? "+" : (grade % 10 < 3) ? "-" : "";
        cout << lettergrade << endl;
    }

    return 0;
}
```

## 练习5.7
改写下列代码段中的错误。

```cpp
(a) if (ival1 != ival2) 
		ival1 = ival2
    else 
    	ival1 = ival2 = 0;
(b) if (ival < minval) 
		minval = ival;
    	occurs = 1;
(c) if (int ival = get_value())
    	cout << "ival = " << ival << endl;
    if (!ival)
    	cout << "ival = 0\n";
(d) if (ival = 0)
    	ival = get_value();
```

解：

- (a) `ival1 = ival2` 后面少了分号。
- (b) 应该用花括号括起来。
- (c) `if (!ival)` 应该改为 `else`。
- (d) `if (ival = 0)` 应该改为 `if (ival == 0)`。

## 练习5.8
什么是“悬垂else”？C++语言是如何处理else子句的？

解：

用来描述在嵌套的`if else`语句中，如果`if`比`else`多时如何处理的问题。C++使用的方法是`else`匹配最近没有配对的`if`。

## 练习5.9
编写一段程序，使用一系列`if`语句统计从`cin`读入的文本中有多少元音字母。

解：

```cpp
#include <iostream>

using std::cout; using std::endl; using std::cin;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;
	char ch;
	while (cin >> ch)
	{
		if (ch == 'a') ++aCnt;
		else if (ch == 'e') ++eCnt;
		else if (ch == 'i') ++iCnt;
		else if (ch == 'o') ++oCnt;
		else if (ch == 'u') ++uCnt;
	}
	cout << "Number of vowel a: \t" << aCnt << '\n'
		<< "Number of vowel e: \t" << eCnt << '\n'
		<< "Number of vowel i: \t" << iCnt << '\n'
		<< "Number of vowel o: \t" << oCnt << '\n'
		<< "Number of vowel u: \t" << uCnt << endl;

	return 0;
}
```

## 练习5.10
我们之前实现的统计元音字母的程序存在一个问题：如果元音字母以大写形式出现，不会被统计在内。编写一段程序，既统计元音字母的小写形式，也统计元音字母的大写形式，也就是说，新程序遇到'a'和'A'都应该递增`aCnt`的值，以此类推。

解：

```cpp
#include <iostream>
using std::cin; using std::cout; using std::endl;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;
	char ch;
	while (cin >> ch)
		switch (ch)
	{
		case 'a':
		case 'A':
			++aCnt;
			break;
		case 'e':
		case 'E':
			++eCnt;
			break;
		case 'i':
		case 'I':
			++iCnt;
			break;
		case 'o':
		case 'O':
			++oCnt;
			break;
		case 'u':
		case 'U':
			++uCnt;
			break;
	}

	cout << "Number of vowel a(A): \t" << aCnt << '\n'
		<< "Number of vowel e(E): \t" << eCnt << '\n'
		<< "Number of vowel i(I): \t" << iCnt << '\n'
		<< "Number of vowel o(O): \t" << oCnt << '\n'
		<< "Number of vowel u(U): \t" << uCnt << endl;

	return 0;
}
```

## 练习5.11
修改统计元音字母的程序，使其也能统计空格、制表符、和换行符的数量。

解：

```cpp
#include <iostream>

using std::cin; using std::cout; using std::endl;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, spaceCnt = 0, tabCnt = 0, newLineCnt = 0;
	char ch;
	while (cin >> std::noskipws >> ch)  //noskipws(no skip whitespce)
		switch (ch)
	{
		case 'a':
		case 'A':
			++aCnt;
			break;
		case 'e':
		case 'E':
			++eCnt;
			break;
		case 'i':
		case 'I':
			++iCnt;
			break;
		case 'o':
		case 'O':
			++oCnt;
			break;
		case 'u':
		case 'U':
			++uCnt;
			break;
		case ' ':
			++spaceCnt;
			break;
		case '\t':
			++tabCnt;
			break;
		case '\n':
			++newLineCnt;
			break;
	}

	cout << "Number of vowel a(A): \t" << aCnt << '\n'
		<< "Number of vowel e(E): \t" << eCnt << '\n'
		<< "Number of vowel i(I): \t" << iCnt << '\n'
		<< "Number of vowel o(O): \t" << oCnt << '\n'
		<< "Number of vowel u(U): \t" << uCnt << '\n'
		<< "Number of space: \t" << spaceCnt << '\n'
		<< "Number of tab char: \t" << tabCnt << '\n'
		<< "Number of new line: \t" << newLineCnt << endl;

	return 0;
}
```

其中，使用 `std::noskipws`可以保留默认跳过的空格。

## 练习5.12
修改统计元音字母的程序，使其能统计含以下两个字符的字符序列的数量：`ff`、`fl`和`fi`。

解：

```cpp
#include <iostream>

using std::cin; using std::cout; using std::endl;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, spaceCnt = 0, tabCnt = 0, newLineCnt = 0, ffCnt = 0, flCnt = 0, fiCnt = 0;
	char ch, prech = '\0';
	while (cin >> std::noskipws >> ch)
	{
		switch (ch)
		{
		case 'a':
		case 'A':
			++aCnt;
			break;
		case 'e':
		case 'E':
			++eCnt;
			break;
		case 'i':
			if (prech == 'f') ++fiCnt;
		case 'I':
			++iCnt;
			break;
		case 'o':
		case 'O':
			++oCnt;
			break;
		case 'u':
		case 'U':
			++uCnt;
			break;
		case ' ':
			++spaceCnt;
			break;
		case '\t':
			++tabCnt;
			break;
		case '\n':
			++newLineCnt;
			break;
		case 'f':
			if (prech == 'f') ++ffCnt;
			break;
		case 'l':
			if (prech == 'f') ++flCnt;
			break;
		}
		prech = ch;
	}

	cout << "Number of vowel a(A): \t" << aCnt << '\n'
		<< "Number of vowel e(E): \t" << eCnt << '\n'
		<< "Number of vowel i(I): \t" << iCnt << '\n'
		<< "Number of vowel o(O): \t" << oCnt << '\n'
		<< "Number of vowel u(U): \t" << uCnt << '\n'
		<< "Number of space: \t" << spaceCnt << '\n'
		<< "Number of tab char: \t" << tabCnt << '\n'
		<< "Number of new line: \t" << newLineCnt << '\n'
		<< "Number of ff: \t" << ffCnt << '\n'
		<< "Number of fl: \t" << flCnt << '\n'
		<< "Number of fi: \t" << fiCnt << endl;

	return 0;
}

```

## 练习5.13
下面显示的每个程序都含有一个常见的编码错误，指出错误在哪里，然后修改它们。
```cpp
(a) unsigned aCnt = 0, eCnt = 0, iouCnt = 0;
    char ch = next_text();
    switch (ch) {
        case 'a': aCnt++;
        case 'e': eCnt++;
        default: iouCnt++;
    }
(b) unsigned index = some_value();
    switch (index) {
        case 1:
            int ix = get_value();
            ivec[ ix ] = index;
            break;
        default:
            ix = ivec.size()-1;
            ivec[ ix ] = index;
    }
(c) unsigned evenCnt = 0, oddCnt = 0;
    int digit = get_num() % 10;
    switch (digit) {
        case 1, 3, 5, 7, 9:
            oddcnt++;
            break;
        case 2, 4, 6, 8, 10:
            evencnt++;
            break;
    }
(d) unsigned ival=512, jval=1024, kval=4096;
    unsigned bufsize;
    unsigned swt = get_bufCnt();
    switch(swt) {
        case ival:
            bufsize = ival * sizeof(int);
            break;
        case jval:
            bufsize = jval * sizeof(int);
            break;
        case kval:
            bufsize = kval * sizeof(int);
            break;
    }
```

解：

(a) 少了`break`语句。应该为：
```cpp
unsigned aCnt = 0, eCnt = 0, iouCnt = 0;
    char ch = next_text();
    switch (ch) {
    	case 'a': aCnt++; break;
    	case 'e': eCnt++; break;
    	default: iouCnt++; break;
    }
```
	
(b) 在`default`分支当中，`ix`未定义。应该在外部定义`ix`。
```cpp
unsigned index = some_value();
    int ix;
    switch (index) {
        case 1:
            ix = get_value();
            ivec[ ix ] = index;
            break;
        default:
            ix = static_cast<int>(ivec.size())-1;
            ivec[ ix ] = index;
    }
```
    
(c) `case`后面应该用冒号而不是逗号。
```cpp
unsigned evenCnt = 0, oddCnt = 0;
    int digit = get_num() % 10;
    switch (digit) {
        case 1: case 3: case 5: case 7: case 9:
            oddcnt++;
            break;
        case 2: case 4: case 6: case 8: case 0:
            evencnt++;
            break;
    }
```
    
(d) `case`标签必须是整型常量表达式。
```cpp
const unsigned ival=512, jval=1024, kval=4096;
    unsigned bufsize;
    unsigned swt = get_bufCnt();
    switch(swt) {
        case ival:
            bufsize = ival * sizeof(int);
            break;
        case jval:
            bufsize = jval * sizeof(int);
            break;
        case kval:
            bufsize = kval * sizeof(int);
            break;
    }
```
    
## 练习5.14
编写一段程序，从标准输入中读取若干`string`对象并查找连续重复出现的单词，所谓连续重复出现的意思是：一个单词后面紧跟着这个单词本身。要求记录连续重复出现的最大次数以及对应的单词。如果这样的单词存在，输出重复出现的最大次数；如果不存在，输出一条信息说明任何单词都没有连续出现过。
例如：如果输入是：
```
how now now now brown cow cow
```
那么输出应该表明单词now连续出现了3次。

解：

```cpp
#include <iostream>
#include <string>

using std::cout; using std::cin; using std::endl; using std::string; using std::pair;

int main()
{ 
    pair<string, int> max_duplicated;
    int count = 0;
    for (string str, prestr; cin >> str; prestr = str)
    {
        if (str == prestr) ++count;
        else count = 0; 
        if (count > max_duplicated.second) max_duplicated = { prestr, count };
    }
    
    if (max_duplicated.first.empty()) cout << "There's no duplicated string." << endl;
    else cout << "the word " << max_duplicated.first << " occurred " << max_duplicated.second + 1 << " times. " << endl;
    
    return 0;
}
```

## 练习5.15
说明下列循环的含义并改正其中的错误。

```cpp
(a) for (int ix = 0; ix != sz; ++ix) { /* ... */ }
    if (ix != sz)
    	// . . .
(b) int ix;
    for (ix != sz; ++ix) { /* ... */ }
(c) for (int ix = 0; ix != sz; ++ix, ++sz) { /*...*/ }
```

解：

应该改为下面这样：

```cpp
(a) int ix;
    for (ix = 0; ix != sz; ++ix)  { /* ... */ }
    if (ix != sz)
    // . . .
(b) int ix;
    for (; ix != sz; ++ix) { /* ... */ }
(c) for (int ix = 0; ix != sz; ++ix) { /*...*/ }
```

## 练习5.16

`while`循环特别适用于那种条件不变、反复执行操作的情况，例如，当未达到文件末尾时不断读取下一个值。
`for`循环更像是在按步骤迭代，它的索引值在某个范围内一次变化。根据每种循环的习惯各自编写一段程序，然后分别用另一种循环改写。
如果只能使用一种循环，你倾向于哪种？为什么？

解：

```cpp
int i;
while ( cin >> i )
    // ...


for (int i = 0; cin >> i;)
    // ...


for (int i = 0; i != size; ++i)
    // ...


int i = 0;
while (i != size)
{
    // ...
    ++i;
}
```
如果只能用一种循环，我会更倾向使用`while`，因为`while`显得简洁，代码可读性强。

## 练习5.17

假设有两个包含整数的`vector`对象，编写一段程序，检验其中一个`vector`对象是否是另一个的前缀。
为了实现这一目标，对于两个不等长的`vector`对象，只需挑出长度较短的那个，把它的所有元素和另一个`vector`对象比较即可。
例如，如果两个`vector`对象的元素分别是0、1、1、2 和 0、1、1、2、3、5、8，则程序的返回结果为真。

解：

```cpp
#include <iostream>
#include <vector>

using std::cout; using std::vector;

bool is_prefix(vector<int> const& lhs, vector<int> const& rhs)
{
    if(lhs.size() > rhs.size())
        return is_prefix(rhs, lhs);
    for(unsigned i = 0; i != lhs.size(); ++i)
        if(lhs[i] != rhs[i]) return false;
    return true;
}

int main()
{
    vector<int> l{ 0, 1, 1, 2 };
    vector<int> r{ 0, 1, 1, 2, 3, 5, 8 };
    cout << (is_prefix(r, l) ? "yes\n" : "no\n");

    return 0;
}
```

## 练习5.18
说明下列循环的含义并改正其中的错误。

```cpp
(a) do { // 应该添加花括号
        int v1, v2;
        cout << "Please enter two numbers to sum:" ;
        if (cin >> v1 >> v2)
            cout << "Sum is: " << v1 + v2 << endl;
    }while (cin);
(b) int ival;
    do {
        // . . .
    } while (ival = get_response()); // 应该将ival 定义在循环外
(c) int ival = get_response();
    do {
        ival = get_response();
    } while (ival); // 应该将ival 定义在循环外
```

## 练习5.19
编写一段程序，使用`do while`循环重复地执行下述任务：
首先提示用户输入两个`string`对象，然后挑出较短的那个并输出它。

解：

```cpp
#include <iostream>
#include <string>

using std::cout; using std::cin; using std::endl; using std::string;

int main()
{
    string rsp;
    do {
        cout << "Input two strings: ";
        string str1, str2;
        cin >> str1 >> str2;
        cout << (str1 <= str2 ? str1 : str2) 
             << " is less than the other. " << "\n\n"
             << "More? Enter yes or no: ";
        cin >> rsp;
    } while (!rsp.empty() && tolower(rsp[0]) == 'y');
    return 0;
}
```

## 练习5.20
编写一段程序，从标准输入中读取`string`对象的序列直到连续出现两个相同的单词或者所有的单词都读完为止。
使用`while`循环一次读取一个单词，当一个单词连续出现两次时使用`break`语句终止循环。
输出连续重复出现的单词，或者输出一个消息说明没有任何单词是连续重复出现的。

解：

```cpp
#include <iostream>
#include <string>
using std::cout; using std::cin; using std::endl; using std::string;

int main()
{
    string read, tmp;
    while (cin >> read)
        if (read == tmp) break; else tmp = read;

    if (cin.eof())  cout << "no word was repeated." << endl; //eof(end of file)判断输入是否结束,或者文件结束符,等同于 CTRL+Z
    else            cout << read << " occurs twice in succession." << endl;

    return 0;
}
```

## 练习5.21
修改5.5.1节练习题的程序，使其找到的重复单词必须以大写字母开头。

解：

```cpp
#include <iostream>
using std::cin; using std::cout; using std::endl;
#include <string>
using std::string;

int main()
{
    string curr, prev;
    bool no_twice = true;
    while (cin >> curr) 
    {
        if (isupper(curr[0]) && prev == curr)
        {
            cout << curr << ": occurs twice in succession." << endl;
            no_twice = false;
            break;
        }
        prev = curr;
    }

    if (no_twice)
        cout << "no word was repeated." << endl;

    return 0;
}
```

## 练习5.22
本节的最后一个例子跳回到`begin`，其实使用循环能更好的完成该任务，重写这段代码，注意不再使用`goto`语句。

```cpp
// 向后跳过一个带初始化的变量定义是合法的
begin:
    int sz = get_size();
    if (sz <= 0) {
        goto begin;
    }
```

解：

用 for 循环修改的话就是这样
```cpp
for (int sz = get_size(); sz <=0; sz = get_size())
    ;
```

## 练习5.23
编写一段程序，从标准输入读取两个整数，输出第一个数除以第二个数的结果。

解：

```cpp
#include <iostream>
using std::cin;
using std::cout;
using std::endl;

int main() 
{
    int i, j; 
    cin >> i >> j;
    cout << i / j << endl;
 
    return 0;
}
```

## 练习5.24
修改你的程序，使得当第二个数是0时抛出异常。先不要设定`catch`子句，运行程序并真的为除数输入0，看看会发生什么？

解：

```cpp
#include <iostream>
#include <stdexcept>

int main(void)
{
    int i, j;
    std::cin >> i >> j;
    if (j == 0)
        throw std::runtime_error("divisor is 0");
    std::cout << i / j << std::endl;

    return 0;
}
```

## 练习5.25
修改上一题的程序，使用`try`语句块去捕获异常。`catch`子句应该为用户输出一条提示信息，询问其是否输入新数并重新执行`try`语句块的内容。

解：

```cpp
#include <iostream>
#include <stdexcept>
using std::cin; using std::cout; using std::endl; using std::runtime_error;

int main(void)
{
    for (int i, j; cout << "Input two integers:\n", cin >> i >> j; )
    {
        try 
        {
            if (j == 0) 
                throw runtime_error("divisor is 0");
            cout << i / j << endl;
        }
        catch (runtime_error err) 
        {
            cout << err.what() << "\nTry again? Enter y or n" << endl;
            char c;
            cin >> c;
            if (!cin || c == 'n')
                break;
        }
    }

    return 0;
}
```

# 第六章 函数

## 练习6.1
实参和形参的区别的什么？

解：

实参是函数调用的实际值，是形参的初始值。

## 练习6.2
请指出下列函数哪个有错误，为什么？应该如何修改这些错误呢？

```cpp
(a) int f() {
         string s;
         // ...
         return s;
   }
(b) f2(int i) { /* ... */ }
(c) int calc(int v1, int v1) { /* ... */ }
(d) double square (double x)  return x * x; 
```

解：

应该改为下面这样：

```cpp
(a) string f() {
         string s;
         // ...
         return s;
   }
(b) void f2(int i) { /* ... */ }
(c) int calc(int v1, int v2) { /* ... */ return ; }
(d) double square (double x) { return x * x; }
```

## 练习6.3
编写你自己的`fact`函数，上机检查是否正确。注：阶乘。

解：

```cpp
#include <iostream>

int fact(int i)
{
    if(i<0)
    {
        std::runtime_error err("Input cannot be a negative number");
        std::cout << err.what() << std::endl;
    }
    return i > 1 ? i * fact( i - 1 ) : 1;
}

int main()
{
    std::cout << std::boolalpha << (120 == fact(5)) << std::endl;
    return 0;
}
```

启用`std::boolalpha`，可以输出 `"true"`或者 `"false"`。

## 练习6.4
编写一个与用户交互的函数，要求用户输入一个数字，计算生成该数字的阶乘。在main函数中调用该函数。

```cpp
#include <iostream>
#include <string>

int fact(int i)
{
    return i > 1 ? i * fact(i - 1) : 1;
}

void interactive_fact()
{
    std::string const prompt = "Enter a number within [1, 13) :\n";
    std::string const out_of_range = "Out of range, please try again.\n";
    for (int i; std::cout << prompt, std::cin >> i; )
    {
        if (i < 1 || i > 12)
        {
            std::cout << out_of_range; 
            continue;
        }
        std::cout << fact(i) << std::endl;
    }
}

int main()
{
    interactive_fact();
    return 0;
}
```

## 练习6.5
编写一个函数输出其实参的绝对值。

```cpp
#include <iostream>

int abs(int i)
{
   return i > 0 ? i : -i;
}

int main()
{
   std::cout << abs(-5) << std::endl;
   return 0;
}
```

## 练习6.6
说明形参、局部变量以及局部静态变量的区别。编写一个函数，同时达到这三种形式。

解：

形参定义在函数形参列表里面；局部变量定义在代码块里面；
局部静态变量在程序的执行路径第一次经过对象定义语句时初始化，并且直到程序终止时才被销毁。

```cpp
// 例子
int count_add(int n)       // n是形参
{
    static int ctr = 0;    // ctr 是局部静态变量
    ctr += n;
    return ctr;
}

int main()
{
    for (int i = 0; i != 10; ++i)  // i 是局部变量
      cout << count_add(i) << endl;

    return 0;
}
```

## 练习6.7
编写一个函数，当它第一次被调用时返回0，以后每次被调用返回值加1。

解：

```cpp
int generate()
{
    static int ctr = 0;
    return ctr++;
}
```

## 练习6.8
编写一个名为Chapter6.h 的头文件，令其包含6.1节练习中的函数声明。

解：

```cpp

int fact(int val);
int func();

template <typename T> //参考：https://blog.csdn.net/fightingforcv/article/details/51472586

T abs(T i)
{
    return i >= 0 ? i : -i;
}
```

## 练习6.9 : fact.cc | factMain.cc
编写你自己的fact.cc 和factMain.cc ，这两个文件都应该包含上一小节的练习中编写的 Chapter6.h 头文件。通过这些文件，理解你的编译器是如何支持分离式编译的。

解：

fact.cc：

```cpp
#include "Chapter6.h"
#include <iostream>

int fact(int val)
{
    if (val == 0 || val == 1) return 1;
    else return val * fact(val-1);
}

int func()
{
    int n, ret = 1;
    std::cout << "input a number: ";
    std::cin >> n;
    while (n > 1) ret *= n--;
    return ret;
}

```

factMain.cc：

```cpp
#include "Chapter6.h"
#include <iostream>

int main()
{
    std::cout << "5! is " << fact(5) << std::endl; 
    std::cout << func() << std::endl; 
    std::cout << abs(-9.78) << std::endl;
}
```

编译： `g++ factMain.cpp fact.cpp -o main`

## 练习6.10
编写一个函数，使用指针形参交换两个整数的值。
在代码中调用该函数并输出交换后的结果，以此验证函数的正确性。

解：

```cpp
#include <iostream>
#include <string>

void swap(int* lhs, int* rhs)
{
	int tmp;
	tmp = *lhs;
	*lhs = *rhs;
	*rhs = tmp;
}

int main()
{
	for (int lft, rht; std::cout << "Please Enter:\n", std::cin >> lft >> rht;)
	{
		swap(&lft, &rht);
		std::cout << lft << " " << rht << std::endl;
	}

	return 0;
}
```

## 练习6.11
编写并验证你自己的reset函数，使其作用于引用类型的参数。注：reset即置0。

解：

```cpp
#include <iostream>

void reset(int &i)
{
    i = 0;
}

int main()
{
    int i = 42;
    reset(i);
    std::cout << i  << std::endl;
    return 0;
}
```

## 练习6.12
改写6.2.1节练习中的程序，使其引用而非指针交换两个整数的值。你觉得哪种方法更易于使用呢？为什么？

```cpp
#include <iostream>
#include <string>


void swap(int& lhs, int& rhs)
{
    int temp = lhs;
    lhs = rhs;
    rhs = temp;
}

int main()
{
    for (int left, right; std::cout << "Please Enter:\n", std::cin >> left >> right; )
    {
        swap(left, right);
        std::cout << left << " " << right << std::endl;
    }

    return 0;
}
```
很明显引用更好用。

## 练习6.13
假设`T`是某种类型的名字，说明以下两个函数声明的区别：
一个是`void f(T)`, 另一个是`void f(&T)`。

解：

`void f(T)`的参数通过值传递，在函数中`T`是实参的副本，改变`T`不会影响到原来的实参。
`void f(&T)`的参数通过引用传递，在函数中的`T`是实参的引用，`T`的改变也就是实参的改变。

## 练习6.14
举一个形参应该是引用类型的例子，再举一个形参不能是引用类型的例子。

解：

例如交换两个整数的函数，形参应该是引用

```cpp
void swap(int& lhs, int& rhs)
{
	int temp = lhs;
	lhs = rhs;
	rhs = temp;
}
```

当实参的值是右值时，形参不能为引用类型

```cpp
int add(int a, int b)
{
	return a + b;
}

int main()
{
	int i = add(1,2);
	return 0;
}
```

## 练习6.15
说明`find_char`函数中的三个形参为什么是现在的类型，特别说明为什么`s`是常量引用而`occurs`是普通引用？
为什么`s`和`occurs`是引用类型而`c`不是？
如果令`s`是普通引用会发生什么情况？
如果令`occurs`是常量引用会发生什么情况？

解：

- 因为字符串可能很长，因此使用引用避免拷贝；
- 而在函数中我们不希望改变`s`的内容，所以令`s`为常量。
- `occurs`是要传到函数外部的变量，所以使用引用，`occurs`的值会改变，所以是普通引用。
- 因为我们只需要`c`的值，这个实参可能是右值(右值实参无法用于引用形参)，所以`c`不能用引用类型。
- 如果`s`是普通引用，也可能会意外改变原来字符串的内容。
- `occurs`如果是常量引用，那么意味着不能改变它的值，那也就失去意义了。

## 练习6.16
下面的这个函数虽然合法，但是不算特别有用。指出它的局限性并设法改善。
```cpp
bool is_empty(string& s) { return s.empty(); }
```

解：

局限性在于常量字符串和字符串字面值无法作为该函数的实参，如果下面这样调用是非法的：

```cpp
const string str;
bool flag = is_empty(str); //非法
bool flag = is_empty("hello"); //非法
```

所以要将这个函数的形参定义为常量引用：

```cpp
bool is_empty(const string& s) { return s.empty(); }
```

## 练习6.17
编写一个函数，判断`string`对象中是否含有大写字母。
编写另一个函数，把`string`对象全部改写成小写形式。
在这两个函数中你使用的形参类型相同吗？为什么？

解：

两个函数的形参不一样。第一个函数使用常量引用，第二个函数使用普通引用。

## 练习6.18
为下面的函数编写函数声明，从给定的名字中推测函数具备的功能。

- (a) 名为`compare`的函数，返回布尔值，两个参数都是`matrix`类的引用。
- (b) 名为`change_val`的函数，返回`vector`的迭代器，有两个参数：一个是`int`，另一个是`vector`的迭代器。

解：

```cpp
(a) bool compare(matrix &m1, matrix &m2);
(b) vector<int>::iterator change_val(int, vector<int>::iterator);
```

## 练习6.19
假定有如下声明，判断哪个调用合法、哪个调用不合法。对于不合法的函数调用，说明原因。

```cpp
double calc(double);
int count(const string &, char);
int sum(vector<int>::iterator, vector<int>::iterator, int);
vector<int> vec(10);
(a) calc(23.4, 55.1);
(b) count("abcda",'a');
(c) calc(66);
(d) sum(vec.begin(), vec.end(), 3.8);
```

解：

- (a) 不合法。`calc`只有一个参数。
- (b) 合法。
- (c) 合法。
- (d) 合法。

## 练习6.20
引用形参什么时候应该是常量引用？如果形参应该是常量引用，而我们将其设为了普通引用，会发生什么情况？

解：

应该尽量将引用形参设为常量引用，除非有明确的目的是为了改变这个引用变量。
如果形参应该是常量引用，而我们将其设为了普通引用，那么常量实参将无法作用于普通引用形参。

## 练习6.21
编写一个函数，令其接受两个参数：一个是`int`型的数，另一个是`int`指针。
函数比较`int`的值和指针所指的值，返回较大的那个。
在该函数中指针的类型应该是什么？

解：

```cpp
#include <iostream>
using std::cout;

int larger_one(const int i, const int *const p)
{
    return (i > *p) ? i : *p;
}

int main()
{
    int i = 6;
    cout << larger_one(7, &i);

    return 0;
}
```

应该是`const int *`类型。

## 练习6.22
编写一个函数，令其交换两个`int`指针。

解：

```cpp
#include <iostream>
#include <string>

void swap(int*& lft, int*& rht)
{
    auto tmp = lft;
    lft = rht;
    rht = tmp;
}

int main()
{
    int i = 42, j = 99;
    auto lft = &i;
    auto rht = &j;
    swap(lft, rht);
    std::cout << *lft << " " << *rht << std::endl;

    return 0;
}
```

## 练习6.23
参考本节介绍的几个`print`函数，根据理解编写你自己的版本。
依次调用每个函数使其输入下面定义的`i`和`j`:

```cpp
int i = 0, j[2] = { 0, 1 };
```

解：

```cpp
#include <iostream>
using std::cout; using std::endl; using std::begin; using std::end;

void print(const int *pi)
{
    if(pi)
        cout << *pi << endl;
}

void print(const char *p)
{
    if (p)
        while (*p) cout << *p++;
    cout << endl;
}

void print(const int *beg, const int *end)
{
    while (beg != end)
        cout << *beg++ << endl;
}

void print(const int ia[], size_t size)
{
    for (size_t i = 0; i != size; ++i) {
        cout << ia[i] << endl;
    }
}

void print(int (&arr)[2])
{
    for (auto i : arr)
        cout << i << endl;
}

int main()
{
    int i = 0, j[2] = { 0, 1 };
    char ch[5] = "pezy";
    
    print(ch);
    print(begin(j), end(j));
    print(&i);
    print(j, end(j)-begin(j));
    print(j);
    
    return 0;
}
```

## 练习6.24
描述下面这个函数的行为。如果代码中存在问题，请指出并改正。

```cpp
void print(const int ia[10])
{
	for (size_t i = 0; i != 10; ++i)
		cout << ia[i] << endl;
}
```

解：

当数组作为实参的时候，会被自动转换为指向首元素的指针。
因此函数形参接受的是一个指针。
如果要让这个代码成功运行(不更改也可以运行），可以将形参改为数组的引用。

```cpp
void print(const int (&ia)[10])
{
	for (size_t i = 0; i != 10; ++i)
		cout << ia[i] << endl;
}
```

## 练习6.25
编写一个`main`函数，令其接受两个实参。把实参的内容连接成一个`string`对象并输出出来。

## 练习6.26
编写一个程序，使其接受本节所示的选项；输出传递给`main`函数的实参内容。

解：

包括6.25

```cpp
#include <iostream>
#include <string>

int main(int argc, char **argv)
{
    std::string str;
    for (int i = 1; i != argc; ++i)
        str += std::string(argv[i]) + " ";

    std::cout << str << std::endl;
    return 0;
}
```

## 练习6.27
编写一个函数，它的参数是`initializer_list`类型的对象，函数的功能是计算列表中所有元素的和。

解：

```cpp
#include <iostream>
#include <initializer_list>

int sum(std::initializer_list<int> const& il)
{
    int sum = 0;
    for (auto i : il) sum += i;
    return sum;
}

int main(void)
{
    auto il = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    std::cout << sum(il) << std::endl;

    return 0;
}
```

## 练习6.28
在`error_msg`函数的第二个版本中包含`ErrCode`类型的参数，其中循环内的`elem`是什么类型？

解：

`elem`是`const string &`类型。

## 练习6.29
在范围`for`循环中使用`initializer_list`对象时，应该将循环控制变量声明成引用类型吗？为什么？

解：

应该使用常量引用类型。`initializer_list`对象中的元素都是常量，我们无法修改`initializer_list`对象中的元素的值。

## 练习6.30
编译第200页的`str_subrange`函数，看看你的编译器是如何处理函数中的错误的。

解：

编译器信息：
```
g++ (Ubuntu 5.4.0-6ubuntu1~16.04.10) 5.4.0 20160609
```

编译错误信息：

```
ch6.cpp:38:9: error: return-statement with no value, in function returning ‘bool’ [-fpermissive]
```

## 练习6.31
什么情况下返回的引用无效？什么情况下返回常量的引用无效？

解：

当返回的引用的对象是局部变量时，返回的引用无效；当我们希望返回的对象被修改时，返回常量的引用无效。

## 练习6.32
下面的函数合法吗？如果合法，说明其功能；如果不合法，修改其中的错误并解释原因。

```cpp
int &get(int *array, int index) { return array[index]; }
int main()
{
    int ia[10];
    for (int i = 0; i != 10; ++i)
        get(ia, i) = i;
}
```

解：

合法。`get`函数根据索引取得数组中的元素的引用。

## 练习6.33
编写一个递归函数，输出`vector`对象的内容。

解：

```cpp
#include <iostream>
#include <vector>
using std::vector; using std::cout;
using Iter = vector<int>::const_iterator;

void print(Iter first, Iter last)
{
    if (first != last)
    {
        cout << *first << " ";
        print(++first, last);
    }
}

int main()
{
    vector<int> vec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    print(vec.cbegin(), vec.cend());

    return 0;
}
```

## 练习6.34
如果`factorial`函数的停止条件如下所示，将发生什么？

```cpp
if (val != 0)
```

解：
如果`val`为正数，从结果上来说没有区别（多乘了个1）; 
如果`val`为负数，那么递归永远不会结束。

## 练习6.35
在调用`factorial`函数时，为什么我们传入的值是`val-1`而非`val--`？

解：

如果传入的值是`val--`，那么将会永远传入相同的值来调用该函数，递归将永远不会结束。

## 练习6.36
编写一个函数声明，使其返回数组的引用并且该数组包含10个`string`对象。
不用使用尾置返回类型、`decltype`或者类型别名。

解：

```cpp
string (&fun())[10];
```

## 练习6.37
为上一题的函数再写三个声明，一个使用类型别名，另一个使用尾置返回类型，最后一个使用`decltype`关键字。
你觉得哪种形式最好？为什么？

解：

```cpp
typedef string str_arr[10];
str_arr& fun();

auto fun()->string(&)[10];

string s[10];
decltype(s)& fun();
```

我觉得尾置返回类型最好，就一行代码。

## 练习6.38
修改`arrPtr`函数，使其返回数组的引用。

解：

```cpp
decltype(odd)& arrPtr(int i)
{
    return (i % 2) ? odd : even;
}
```

## 练习6.39
说明在下面的每组声明中第二条语句是何含义。
如果有非法的声明，请指出来。

```cpp
(a) int calc(int, int);
	int calc(const int, const int);
(b) int get();
	double get();
(c) int *reset(int *);
	double *reset(double *);
```
	
解：

- (a) 非法。因为顶层const不影响传入函数的对象，所以第二个声明无法与第一个声明区分开来。
- (b) 非法。对于重载的函数来说，它们应该只有形参的数量和形参的类型不同。返回值与重载无关。
- (c) 合法。

## 练习6.40
下面的哪个声明是错误的？为什么？

```cpp
(a) int ff(int a, int b = 0, int c = 0);
(b) char *init(int ht = 24, int wd, char bckgrnd);	
```

解：
	
(a) 正确。
(b) 错误。因为一旦某个形参被赋予了默认值，那么它之后的形参都必须要有默认值。

## 练习6.41
下面的哪个调用是非法的？为什么？哪个调用虽然合法但显然与程序员的初衷不符？为什么？

```cpp
char *init(int ht, int wd = 80, char bckgrnd = ' ');
(a) init();
(b) init(24,10);
(c) init(14,'*');
```

解：

- (a) 非法。第一个参数不是默认参数，最少需要一个实参。
- (b) 合法。
- (c) 合法，但与初衷不符。字符`*`被解释成`int`传入到了第二个参数。而初衷是要传给第三个参数。

## 练习6.42
给`make_plural`函数的第二个形参赋予默认实参's', 利用新版本的函数输出单词success和failure的单数和复数形式。

解：

```cpp
#include <iostream>
#include <string>

using std::string;
using std::cout;
using std::endl;

string make_plural(size_t ctr, const string& word, const string& ending = "s")
{
	return (ctr > 1) ? word + ending : word;
}

int main()
{
	cout << "single: " << make_plural(1, "success", "es") << " "
		<< make_plural(1, "failure") << endl;
	cout << "plural : " << make_plural(2, "success", "es") << " "
		<< make_plural(2, "failure") << endl;

	return 0;
}
```

## 练习6.43
你会把下面的哪个声明和定义放在头文件中？哪个放在源文件中？为什么？

```cpp
(a) inline bool eq(const BigInt&, const BigInt&) {...}
(b) void putValues(int *arr, int size);
```

解：

全部都放进头文件。(a) 是内联函数，(b) 是声明。

## 练习6.44
将6.2.2节的`isShorter`函数改写成内联函数。

解：

```cpp
inline bool is_shorter(const string &lft, const string &rht) 
{
    return lft.size() < rht.size();
}
```

## 练习6.45
回顾在前面的练习中你编写的那些函数，它们应该是内联函数吗？
如果是，将它们改写成内联函数；如果不是，说明原因。

解：

一般来说，内联机制用于优化规模小、流程直接、频繁调用的函数。

## 练习6.46
能把`isShorter`函数定义成`constexpr`函数吗？
如果能，将它改写成`constxpre`函数；如果不能，说明原因。

解：

不能。`constexpr`函数的返回值类型及所有形参都得是字面值类型。

## 练习6.47
改写6.3.2节练习中使用递归输出`vector`内容的程序，使其有条件地输出与执行过程有关的信息。
例如，每次调用时输出`vector`对象的大小。
分别在打开和关闭调试器的情况下编译并执行这个程序。

解：

```cpp
#include <iostream>
#include <vector>
using std::vector; using std::cout; using std::endl;

void printVec(vector<int> &vec)
{
#ifndef NDEBUG
    cout << "vector size: " << vec.size() << endl;
#endif
    if (!vec.empty())
    {
        auto tmp = vec.back();
        vec.pop_back();
        printVec(vec);
        cout << tmp << " ";
    }
}

int main()
{
    vector<int> vec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    printVec(vec);
    cout << endl;

    return 0;
}
```

## 练习6.48
说明下面这个循环的含义，它对assert的使用合理吗？

```cpp
string s;
while (cin >> s && s != sought) { } //空函数体
assert(cin);
```

解：

不合理。从这个程序的意图来看，应该用

```cpp
assert(s == sought);
```

## 练习6.49
什么是候选函数？什么是可行函数？

解：

候选函数：与被调用函数同名，并且其声明在调用点可见。
可行函数：形参与实参的数量相等，并且每个实参类型与对应的形参类型相同或者能转换成形参的类型。

## 练习6.50
已知有第217页对函数`f`的声明，对于下面的每一个调用列出可行函数。
其中哪个函数是最佳匹配？
如果调用不合法，是因为没有可匹配的函数还是因为调用具有二义性？

```cpp
(a) f(2.56, 42)
(b) f(42)
(c) f(42, 0)
(d) f(2.56, 3.14)
```

解：

- (a) `void f(int, int);`和`void f(double, double = 3.14);`是可行函数。
该调用具有二义性而不合法。
- (b) `void f(int);` 是可行函数。调用合法。
- (c) `void f(int, int);`和`void f(double, double = 3.14);`是可行函数。
`void f(int, int);`是最佳匹配。
- (d) `void f(int, int);`和`void f(double, double = 3.14);`是可行函数。
`void f(double, double = 3.14);`是最佳匹配。

## 练习6.51
编写函数`f`的4版本，令其各输出一条可以区分的消息。
验证上一个练习的答案，如果你的回答错了，反复研究本节内容直到你弄清自己错在何处。

解：

```cpp
#include <iostream>
using std::cout; using std::endl;

void f()
{
    cout << "f()" << endl;
}

void f(int)
{
    cout << "f(int)" << endl;
}

void f(int, int)
{
    cout << "f(int, int)" << endl;
}

void f(double, double)
{
    cout << "f(double, double)" << endl;
}

int main()
{
    //f(2.56, 42); // error: 'f' is ambiguous.
    f(42);
    f(42, 0);
    f(2.56, 3.14);
    
    return 0;
}

```

## 练习6.52
已知有如下声明：
```cpp
void manip(int ,int);
double dobj;
```
请指出下列调用中每个类型转换的等级。

```cpp
(a) manip('a', 'z');
(b) manip(55.4, dobj);
```

解：

- (a) 第3级。类型提升实现的匹配。
- (b) 第4级。算术类型转换实现的匹配。

## 练习6.53
说明下列每组声明中的第二条语句会产生什么影响，并指出哪些不合法（如果有的话）。


```cpp
(a) int calc(int&, int&); 
	int calc(const int&, const int&); 
(b) int calc(char*, char*);
	int calc(const char*, const char*);
(c) int calc(char*, char*);
	int calc(char* const, char* const);
```
	
解：

(c) 不合法。顶层const不影响传入函数的对象。

## 练习6.54
编写函数的声明，令其接受两个`int`形参并返回类型也是`int`；然后声明一个`vector`对象，令其元素是指向该函数的指针。

解：

```cpp
int func(int, int);
vector<decltype(func)*> v;
```

## 练习6.55
编写4个函数，分别对两个`int`值执行加、减、乘、除运算；在上一题创建的`vector`对象中保存指向这些函数的指针。

解：

```cpp
int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }
int divide(int a, int b) { return b != 0 ? a / b : 0; }

v.push_back(add);
v.push_back(subtract);
v.push_back(multiply);
v.push_back(divide);
```

## 练习6.56
调用上述`vector`对象中的每个元素并输出结果。

解：

```cpp
std::vector<decltype(func) *> vec{ add, subtract, multiply, divide };
for (auto f : vec)
          std::cout << f(2, 2) << std::endl;
```

# 第七章 类

## 练习7.1

使用2.6.1节定义的`Sales_data`类为1.6节的交易处理程序编写一个新版本。

解：

```cpp
#include <iostream>
#include <string>
using std::cin; using std::cout; using std::endl; using std::string;

struct Sales_data
{
    string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main()
{
    Sales_data total;
    if (cin >> total.bookNo >> total.units_sold >> total.revenue)
    {
        Sales_data trans;
        while (cin >> trans.bookNo >> trans.units_sold >> trans.revenue) 
        {
            if (total.bookNo == trans.bookNo) 
            {
                total.units_sold += trans.units_sold;
                total.revenue += trans.revenue;
            }
            else
            {
                cout << total.bookNo << " " << total.units_sold << " " << total.revenue << endl;
                total = trans;
            }
        }
        cout << total.bookNo << " " << total.units_sold << " " << total.revenue << endl;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

## 练习7.2

曾在2.6.2节的练习中编写了一个`Sales_data`类，请向这个类添加`combine`函数和`isbn`成员。

解：

```cpp
#include <string>

struct Sales_data {
    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);
    
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}
```

## 练习7.3

修改7.1.1节的交易处理程序，令其使用这些成员。

解：

```cpp
#include <iostream>
using std::cin; using std::cout; using std::endl;

int main()
{
    Sales_data total;
    if (cin >> total.bookNo >> total.units_sold >> total.revenue)
    {
        Sales_data trans;
        while (cin >> trans.bookNo >> trans.units_sold >> trans.revenue) {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                cout << total.bookNo << " " << total.units_sold << " " << total.revenue << endl;
                total = trans;
            }
        }
        cout << total.bookNo << " " << total.units_sold << " " << total.revenue << endl;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

## 练习7.4

编写一个名为`Person`的类，使其表示人员的姓名和地址。使用`string`对象存放这些元素，接下来的练习将不断充实这个类的其他特征。

解：

```cpp
#include <string>

class Person {
    std::string name;
    std::string address;
};
```

## 练习7.5

在你的`Person`类中提供一些操作使其能够返回姓名和地址。
这些函数是否应该是`const`的呢？解释原因。

解：

```cpp
#include <string>

class Person 
{
    std::string name;
    std::string address;
public:
    auto get_name() const -> std::string const& { return name; }
    auto get_addr() const -> std::string const& { return address; }
};
```
应该是`const`的。因为常量的`Person`对象也需要使用这些函数操作。

## 练习7.6

对于函数`add`、`read`和`print`，定义你自己的版本。

解：

```cpp
#include <string>
#include <iostream>

struct Sales_data {
    std::string const& isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// member functions.
Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

// nonmember functions
std::istream &read(std::istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}
```

## 练习7.7

使用这些新函数重写7.1.2节练习中的程序。

```cpp
int main()
{
    Sales_data total;
    if (read(std::cin, total))
    {
        Sales_data trans;
        while (read(std::cin, trans)) {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(std::cout, total) << std::endl;
                total = trans;
            }
        }
        print(std::cout, total) << std::endl;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    
    return 0;
}
```

## 练习7.8

为什么`read`函数将其`Sales_data`参数定义成普通的引用，而`print`函数将其参数定义成常量引用？

解：

因为`read`函数会改变对象的内容，而`print`函数不会。

## 练习7.9

对于7.1.2节练习中代码，添加读取和打印`Person`对象的操作。

解：

```cpp
#include <string>
#include <iostream>

struct Person 
{
    std::string const& getName()    const { return name; }
    std::string const& getAddress() const { return address; }
    
    std::string name;
    std::string address;
};

std::istream &read(std::istream &is, Person &person)
{
    return is >> person.name >> person.address;
}

std::ostream &print(std::ostream &os, const Person &person)
{
    return os << person.name << " " << person.address;
}
```

## 练习7.10

在下面这条`if`语句中，条件部分的作用是什么？

```cpp
if (read(read(cin, data1), data2)) //等价read(std::cin, data1);read(std::cin, data2);
```

解：

`read`函数的返回值是`istream`对象，
`if`语句中条件部分的作用是从输入流中读取数据给两个`data`对象。

## 练习7.11 : 

在你的`Sales_data`类中添加构造函数，
然后编写一段程序令其用到每个构造函数。

解：

头文件：

```cpp
#include <string>
#include <iostream>

struct Sales_data {
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data(std::istream &is);
    
    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);
    
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// nonmember functions
std::istream &read(std::istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}

// member functions.
Sales_data::Sales_data(std::istream &is)
{
    read(is, *this);
}

Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}
```

主函数：

```cpp
int main()
{
    Sales_data item1;
    print(std::cout, item1) << std::endl;
    
    Sales_data item2("0-201-78345-X");
    print(std::cout, item2) << std::endl;
    
    Sales_data item3("0-201-78345-X", 3, 20.00);
    print(std::cout, item3) << std::endl;
    
    Sales_data item4(std::cin);
    print(std::cout, item4) << std::endl;
    
    return 0;
}
```

## 练习7.12

把只接受一个`istream`作为参数的构造函数移到类的内部。

解：

```cpp
#include <string>
#include <iostream>

struct Sales_data;
std::istream &read(std::istream&, Sales_data&);

struct Sales_data {
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data(std::istream &is) { read(is, *this); }
    
    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);
    
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// member functions.
Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

// nonmember functions
std::istream &read(std::istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}
```

## 练习7.13
使用`istream`构造函数重写第229页的程序。

解：

```cpp
int main()
{
    Sales_data total(std::cin);
    if (!total.isbn().empty())
    {
        std::istream &is = std::cin;
        while (is) {
            Sales_data trans(is);
            if (!is) break;
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(std::cout, total) << std::endl;
                total = trans;
            }
        }
        print(std::cout, total) << std::endl;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    
    return 0;
}
```

## 练习7.14
编写一个构造函数，令其用我们提供的类内初始值显式地初始化成员。

```cpp
Sales_data() : units_sold(0) , revenue(0) { }
```

## 练习7.15
为你的`Person`类添加正确的构造函数。

解：

```cpp
#include <string>
#include <iostream>

struct Person;
std::istream &read(std::istream&, Person&);

struct Person
{
	Person() = default;
	Person(const std::string& sname, const std::string& saddr) :name(sname), address(saddr) {}
	Person(std::istream &is) { read(is, *this); }

	std::string getName() const { return name; }
	std::string getAddress() const { return address; }

	std::string name;
	std::string address;
};

std::istream &read(std::istream &is, Person &person)
{
	is >> person.name >> person.address;
	return is;
}

std::ostream &print(std::ostream &os, const Person &person)
{
	os << person.name << " " << person.address;
	return os;
}
```

## 练习7.16
在类的定义中对于访问说明符出现的位置和次数有限定吗？
如果有，是什么？什么样的成员应该定义在`public`说明符之后？
什么样的成员应该定义在`private`说明符之后？

解：

在类的定义中对于访问说明符出现的位置和次数**没有限定**。

每个访问说明符指定了接下来的成员的访问级别，其有效范围直到出现下一个访问说明符或者达到类的结尾处为止。

如果某个成员能够在整个程序内都被访问，那么它应该定义为`public`; 
如果某个成员只能在类内部访问，那么它应该定义为`private`。

## 练习7.17
使用`class`和`struct`时有区别吗？如果有，是什么？

解：

`class`和`struct`的唯一区别是默认的访问级别不同。

## 练习7.18
封装是何含义？它有什么用处？

解：

将类内部分成员设置为外部不可见，而提供部分接口给外面，这样的行为叫做封装。

用处：

- 1.确保用户的代码不会无意间破坏封装对象的状态。
- 2.被封装的类的具体实现细节可以随时改变，而无需调整用户级别的代码。

## 练习7.19
在你的`Person`类中，你将把哪些成员声明成`public`的？
哪些声明成`private`的？
解释你这样做的原因。

构造函数、`getName()`、`getAddress()`函数将设为`public`。
`name`和 `address` 将设为`private`。
函数是暴露给外部的接口，因此要设为`public`；
而数据则应该隐藏让外部不可见。

## 练习7.20
友元在什么时候有用？请分别举出使用友元的利弊。

解：

当其他类或者函数想要访问当前类的私有变量时，这个时候应该用友元。

利：

与当前类有关的接口函数能直接访问类的私有变量。

弊：

牺牲了封装性与可维护性。

## 练习7.21
修改你的`Sales_data`类使其隐藏实现的细节。
你之前编写的关于`Sales_data`操作的程序应该继续使用，借助类的新定义重新编译该程序，确保其正常工作。

解：

```cpp
#include <string>
#include <iostream>

class Sales_data {
    friend std::istream &read(std::istream &is, Sales_data &item);
    friend std::ostream &print(std::ostream &os, const Sales_data &item);
    friend Sales_data add(const Sales_data &lhs, const Sales_data &rhs);

public:
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data(std::istream &is) { read(is, *this); }

    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);

private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// member functions.
Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

// friend functions
std::istream &read(std::istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}
```

## 练习7.22
修改你的`Person`类使其隐藏实现的细节。

解：

```cpp
#include <string>
#include <iostream>

class Person {
    friend std::istream &read(std::istream &is, Person &person);
    friend std::ostream &print(std::ostream &os, const Person &person);

public:
    Person() = default;
    Person(const std::string sname, const std::string saddr):name(sname), address(saddr){ }
    Person(std::istream &is){ read(is, *this); }

    std::string getName() const { return name; }
    std::string getAddress() const { return address; }
private:
    std::string name;
    std::string address;
};

std::istream &read(std::istream &is, Person &person)
{
    is >> person.name >> person.address;
    return is;
}

std::ostream &print(std::ostream &os, const Person &person)
{
    os << person.name << " " << person.address;
    return os;
}
```

## 练习7.23
编写你自己的`Screen`类型。

解：

```cpp
#include <string>

class Screen {
    public:
        using pos = std::string::size_type;

        Screen() = default;
        Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(ht*wd, c){ }

        char get() const { return contents[cursor]; }
        char get(pos r, pos c) const { return contents[r*width+c]; }

    private:
        pos cursor = 0;
        pos height = 0, width = 0;
        std::string contents;
};
```

# 练习7.24
给你的`Screen`类添加三个构造函数：一个默认构造函数；另一个构造函数接受宽和高的值，然后将`contents`初始化成给定数量的空白；第三个构造函数接受宽和高的值以及一个字符，该字符作为初始化后屏幕的内容。

解：

```cpp
#include <string>

class Screen {
    public:
        using pos = std::string::size_type;

        Screen() = default; // 1
        Screen(pos ht, pos wd):height(ht), width(wd), contents(ht*wd, ' '){ } // 2
        Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(ht*wd, c){ } // 3

        char get() const { return contents[cursor]; }
        char get(pos r, pos c) const { return contents[r*width+c]; }

    private:
        pos cursor = 0;
        pos height = 0, width = 0;
        std::string contents;
};
```

## 练习7.25
`Screen`能安全地依赖于拷贝和赋值操作的默认版本吗？
如果能，为什么？如果不能？为什么？

解：

能。 `Screen`的成员只有内置类型和`string`，因此能安全地依赖于拷贝和赋值操作的默认版本。

管理动态内存的类则不能依赖于拷贝和赋值操作的默认版本，而且也应该尽量使用`string`和`vector`来避免动态管理内存的复杂性。

## 练习7.26
将`Sales_data::avg_price`定义成内联函数。

解：

在头文件中加入：

```cpp
inline double Sales_data::avg_price() const
{
    return units_sold ? revenue/units_sold : 0;
}
```

## 练习7.27 
给你自己的`Screen`类添加`move`、`set` 和`display`函数，通过执行下面的代码检验你的类是否正确。

```cpp
Screen myScreen(5, 5, 'X');
myScreen.move(4, 0).set('#').display(cout);
cout << "\n";
myScreen.display(cout);
cout << "\n";
```

解：

增加代码：

```cpp
#include <string>
#include <iostream>

class Screen {
public:
    ... ...

    inline Screen& move(pos r, pos c);
    inline Screen& set(char c);
    inline Screen& set(pos r, pos c, char ch);

    const Screen& display(std::ostream &os) const { do_display(os); return *this; }
    Screen& display(std::ostream &os) { do_display(os); return *this; }

private:
    void do_display(std::ostream &os) const { os << contents; }

    ... ...
};

inline Screen& Screen::move(pos r, pos c)
{
    cursor = r*width + c;
    return *this;
}

inline Screen& Screen::set(char c)
{
    contents[cursor] = c;
    return *this;
}

inline Screen& Screen::set(pos r, pos c, char ch)
{
    contents[r*width+c] = ch;
    return *this;
}
```

测试代码：

```cpp
int main()
{
    Screen myScreen(5, 5, 'X');
    myScreen.move(4, 0).set('#').display(std::cout);
    std::cout << "\n";
    myScreen.display(std::cout);
    std::cout << "\n";

    return 0;
}
```

## 练习7.28
如果`move`、`set`和`display`函数的返回类型不是`Screen&` 而是`Screen`，则在上一个练习中将会发生什么？

解：

如果返回类型是`Screen`，那么`move`返回的是`*this`的一个副本，因此`set`函数只能改变临时副本而不能改变`myScreen`的值。

## 练习7.29
修改你的`Screen`类，令`move`、`set`和`display`函数返回`Screen`并检查程序的运行结果，在上一个练习中你的推测正确吗？

解：

推测正确。

```
#with '&'
XXXXXXXXXXXXXXXXXXXX#XXXX
XXXXXXXXXXXXXXXXXXXX#XXXX
                    ^
# without '&'
XXXXXXXXXXXXXXXXXXXX#XXXX
XXXXXXXXXXXXXXXXXXXXXXXXX
                    ^
```

## 练习7.30
通过`this`指针使用成员的做法虽然合法，但是有点多余。讨论显示使用指针访问成员的优缺点。

解：

优点：

程序的意图更明确

函数的参数可以与成员同名，如

```cpp
  void setAddr(const std::string &addr) { this->addr = addr; }
```

缺点：

有时候显得有点多余，如
```cpp
std::string getAddr() const { return this->addr; }
```

## 练习7.31
定义一对类`X`和`Y`，其中`X`包含一个指向`Y`的指针，而`Y`包含一个类型为`X`的对象。

解：

```cpp
class Y;

class X{
    Y* y = nullptr;	
};

class Y{
    X x;
};
```

## 练习7.32
定义你自己的`Screen`和`Window_mgr`，其中`clear`是`Window_mgr`的成员，是`Screen`的友元。

解：

```cpp
#include <vector>
#include <iostream>
#include <string>

class Screen;

class Window_mgr
{
public:
	using ScreenIndex = std::vector<Screen>::size_type;
	inline void clear(ScreenIndex);

private:
	std::vector<Screen> screens;
};

class Screen
{
	friend void Window_mgr::clear(ScreenIndex);

public:
	using pos = std::string::size_type;

	Screen() = default;
	Screen(pos ht, pos wd) :height(ht), width(wd), contents(ht*wd,' ') {}
	Screen(pos ht, pos wd, char c) :height(ht), width(wd), contents(ht*wd, c) {}

	char get() const { return contents[cursor]; }
	char get(pos r, pos c) const { return contents[r*width + c]; }
	inline Screen& move(pos r, pos c);
	inline Screen& set(char c);
	inline Screen& set(pos r, pos c, char ch);

	const Screen& display(std::ostream& os) const { do_display(os); return *this; }
	Screen& display(std::ostream& os) { do_display(os); return *this; }
	
private:
	void do_display(std::ostream &os) const { os << contents; }

private:
	pos cursor = 0;
	pos width = 0, height = 0;
	std::string contents;
};


inline void Window_mgr::clear(ScreenIndex i)
{
	Screen& s = screens[i];
	s.contents = std::string(s.height*s.width,' ');
}

inline Screen& Screen::move(pos r, pos c)
{
	cursor = r*width + c;
	return *this;
}

inline Screen& Screen::set(char c)
{
	contents[cursor] = c;
	return *this;
}

inline Screen& Screen::set(pos r, pos c, char ch)
{
	contents[r*width + c] = ch;
	return *this;
}
```

## 练习7.33
如果我们给`Screen`添加一个如下所示的`size`成员将发生什么情况？如果出现了问题，请尝试修改它。

```cpp
pos Screen::size() const
{
    return height * width;
}
```

解：

纠正：错误为 error: extra qualification 'Screen::' on member 'size' [-fpermissive]
则应该去掉Screen::,改为
 ```cpp
 pos size() const{
            return height * width;
        }
```
##  练习7.34
如果我们把第256页`Screen`类的`pos`的`typedef`放在类的最后一行会发生什么情况？

解：

在 dummy_fcn(pos height) 函数中会出现 未定义的标识符pos。

类型名的定义通常出现在类的开始处，这样就能确保所有使用该类型的成员都出现在类名的定义之后。

## 练习7.35
解释下面代码的含义，说明其中的`Type`和`initVal`分别使用了哪个定义。如果代码存在错误，尝试修改它。

```cpp
typedef string Type;
Type initVal(); 
class Exercise {
public:
    typedef double Type;
    Type setVal(Type);
    Type initVal(); 
private:
    int val;
};
Type Exercise::setVal(Type parm) { 
    val = parm + initVal();     
    return val;
}
```
解：

书上255页中说：

```
然而在类中，如果成员使用了外层作用域中的某个名字，而该名字代表一种类型，则类不能在之后重新定义该名字。
```

因此重复定义`Type`是错误的行为。

虽然重复定义类型名字是错误的行为，但是编译器并不为此负责。所以我们要人为地遵守一些原则，在这里有一些讨论。

## 练习7.36
下面的初始值是错误的，请找出问题所在并尝试修改它。

```cpp
struct X {
	X (int i, int j): base(i), rem(base % j) {}
	int rem, base;
};
```

解：

应该改为：

```cpp
struct X {
	X (int i, int j): base(i), rem(base % j) {}
	int base, rem;
};
```

## 练习7.37
使用本节提供的`Sales_data`类，确定初始化下面的变量时分别使用了哪个构造函数，然后罗列出每个对象所有的数据成员的值。

解：
```cpp
Sales_data first_item(cin); // 使用 Sales_data(std::istream &is) ; 各成员值从输入流中读取
int main() {
    // 使用默认构造函数  bookNo = "", cnt = 0, revenue = 0.0
    Sales_data next;

    // 使用 Sales_data(std::string s = "");   bookNo = "9-999-99999-9", cnt = 0, revenue = 0.0
    Sales_data last("9-999-99999-9"); 
}
```

## 练习7.38
有些情况下我们希望提供`cin`作为接受`istream&`参数的构造函数的默认实参，请声明这样的构造函数。

解：

```cpp
Sales_data(std::istream &is = std::cin) { read(is, *this); }
```

## 练习7.39
如果接受`string`的构造函数和接受`istream&`的构造函数都使用默认实参，这种行为合法吗？如果不，为什么？

解：

不合法。当你调用`Sales_data()`构造函数时，无法区分是哪个重载。

## 练习7.40
从下面的抽象概念中选择一个（或者你自己指定一个），思考这样的类需要哪些数据成员，提供一组合理的构造函数并阐明这样做的原因。

```
(a) Book
(b) Data
(c) Employee
(d) Vehicle
(e) Object
(f) Tree
```

解：

(a) Book.

```cpp
class Book 
{
public:
    Book(unsigned isbn, std::string const& name, std::string const& author, std::string const& pubdate)
        :isbn_(isbn), name_(name), author_(author), pubdate_(pubdate)
    { }

    explicit Book(std::istream &in) 
    { 
        in >> isbn_ >> name_ >> author_ >> pubdate_;
    }

private:
    unsigned isbn_;
    std::string name_;
    std::string author_;
    std::string pubdate_;
};
```

## 练习7.41
使用委托构造函数重新编写你的`Sales_data`类，给每个构造函数体添加一条语句，令其一旦执行就打印一条信息。用各种可能的方式分别创建`Sales_data`对象，认真研究每次输出的信息直到你确实理解了委托构造函数的执行顺序。

解：

- [头文件](https://github.com/applenob/Cpp_Primer_Practice/tree/master/cpp_source/ch07/ex_7_41.h)
- [源文件](https://github.com/applenob/Cpp_Primer_Practice/tree/master/cpp_source/ch07/ex_7_41.cpp)
- [主函数](https://github.com/applenob/Cpp_Primer_Practice/tree/master/cpp_source/ch07/ex_7_41_main.cpp)

总结：使用委托构造函数，调用顺序是：
- 1.实际的构造函数的函数体。
- 2.委托构造函数的函数体。

## 练习7.42
对于你在练习7.40中编写的类，确定哪些构造函数可以使用委托。如果可以的话，编写委托构造函数。如果不可以，从抽象概念列表中重新选择一个你认为可以使用委托构造函数的，为挑选出的这个概念编写类定义。

解：

```cpp
class Book 
{
public:
    Book(unsigned isbn, std::string const& name, std::string const& author, std::string const& pubdate)
        :isbn_(isbn), name_(name), author_(author), pubdate_(pubdate)
    { }

    Book(unsigned isbn) : Book(isbn, "", "", "") {}

    explicit Book(std::istream &in) 
    { 
        in >> isbn_ >> name_ >> author_ >> pubdate_;
    }

private:
    unsigned isbn_;
    std::string name_;
    std::string author_;
    std::string pubdate_;
};
```

## 练习7.43
假定有一个名为`NoDefault`的类，它有一个接受`int`的构造函数，但是没有默认构造函数。定义类`C`，`C`有一个 `NoDefault`类型的成员，定义`C`的默认构造函数。
```cpp
class NoDefault {
public:
    NoDefault(int i) { }
};

class C {
public:
    C() : def(0) { } 
private:
    NoDefault def;
};
```

## 练习7.44
下面这条声明合法吗？如果不，为什么？

```cpp
vector<NoDefault> vec(10);//vec初始化有10个元素
```

解：

不合法。因为`NoDefault`没有默认构造函数。

## 练习7.45
如果在上一个练习中定义的vector的元素类型是C，则声明合法吗？为什么？

合法。因为`C`有默认构造函数。

## 练习7.46
下面哪些论断是不正确的？为什么？

- (a) 一个类必须至少提供一个构造函数。
- (b) 默认构造函数是参数列表为空的构造函数。
- (c) 如果对于类来说不存在有意义的默认值，则类不应该提供默认构造函数。
- (d) 如果类没有定义默认构造函数，则编译器将为其生成一个并把每个数据成员初始化成相应类型的默认值。

解：

- (a) 不正确。如果我们的类没有显式地定义构造函数，那么编译器就会为我们隐式地定义一个默认构造函数，并称之为合成的默认构造函数。
- (b) 不完全正确。为每个参数都提供了默认值的构造函数也是默认构造函数。
- (c) 不正确。哪怕没有意义的值也需要初始化。
- (d) 不正确。只有当一个类没有定义**任何构造函数**的时候，编译器才会生成一个默认构造函数。

## 练习7.47
说明接受一个`string`参数的`Sales_data`构造函数是否应该是`explicit`的，并解释这样做的优缺点。

解：

是否需要从`string`到`Sales_data`的转换依赖于我们对用户使用该转换的看法。在此例中，这种转换可能是对的。`null_book`中的`string`可能表示了一个不存在的`ISBN`编号。

优点：

可以抑制构造函数定义的隐式转换

缺点：

为了转换要显式地使用构造函数

## 练习7.48
假定`Sales_data`的构造函数不是`explicit`的，则下述定义将执行什么样的操作？

解：

```cpp
string null_isbn("9-999-9999-9");
Sales_data item1(null_isbn);
Sales_data item2("9-999-99999-9");
```
这些定义和是不是`explicit`的无关。

## 练习7.49
对于`combine`函数的三种不同声明，当我们调用`i.combine(s)`时分别发生什么情况？其中`i`是一个`Sales_data`，而` s`是一个`string`对象。

解：

```cpp
(a) Sales_data &combine(Sales_data); // ok
(b) Sales_data &combine(Sales_data&); // error C2664: 无法将参数 1 从“std::string”转换为“Sales_data &”	因为隐式转换只有一次
(c) Sales_data &combine(const Sales_data&) const; // 该成员函数是const 的，意味着不能改变对象。而 combine函数的本意就是要改变对象
```

## 练习7.50
确定在你的`Person`类中是否有一些构造函数应该是`explicit` 的。

解：

```cpp
explicit Person(std::istream &is){ read(is, *this); }
```

## 练习7.51
`vector`将其单参数的构造函数定义成`explicit`的，而`string`则不是，你觉得原因何在？

假如我们有一个这样的函数：
```cpp
int getSize(const std::vector<int>&);
```
如果`vector`没有将单参数构造函数定义成`explicit`的，我们就可以这样调用：

```cpp
getSize(34);
```
很明显这样调用会让人困惑，函数实际上会初始化一个拥有34个元素的`vecto`r的临时量，然后返回34。但是这样没有任何意义。而`string`则不同，`string`的单参数构造函数的参数是`const char *`，因此凡是在需要用到`string`的地方都可以用` const char *`来代替（字面值就是`const char *`）。如：

```cpp
void print(std::string);
print("hello world");
```

## 练习7.52
使用2.6.1节的 `Sales_data` 类，解释下面的初始化过程。如果存在问题，尝试修改它。
```cpp
Sales_data item = {"987-0590353403", 25, 15.99};
```

解：

`Sales_data` 类不是聚合类，应该修改成如下：
```cpp
struct Sales_data {
    std::string bookNo;
    unsigned units_sold;
    double revenue;
};
```

## 练习7.53
定义你自己的`Debug`。

解：

```cpp
class Debug {
public:
    constexpr Debug(bool b = true) : hw(b), io(b), other(b) { }
    constexpr Debug(bool h, bool i, bool o) : hw(r), io(i), other(0) { }

    constexpr bool any() { return hw || io || other; }
    void set_hw(bool b) { hw = b; }
    void set_io(bool b) { io = b; }
    void set_other(bool b) { other = b; }
    
private:
    bool hw;        // runtime error
    bool io;        // I/O error
    bool other;     // the others
};
```

## 练习7.54
`Debug`中以 `set_` 开头的成员应该被声明成`constexpr` 吗？如果不，为什么？

解：

不能。`constexpr`函数必须包含一个返回语句。

## 练习7.55
7.5.5节的`Data`类是字面值常量类吗？请解释原因。

解：

不是。因为`std::string`不是字面值类型。

## 练习7.56
什么是类的静态成员？它有何优点？静态成员与普通成员有何区别？

解：

与类本身相关，而不是与类的各个对象相关的成员是静态成员。静态成员能用于某些场景，而普通成员不能。

## 练习7.57
编写你自己的`Account`类。

解：

```cpp
class Account {
public:
    void calculate() { amount += amount * interestRate; }
    static double rate() { return interestRate; }
    static void rate(double newRate) { interestRate = newRate; }
    
private:
    std::string owner;
    double amount;
    static double interestRate;
    static constexpr double todayRate = 42.42;
    static double initRate() { return todayRate; }
};

double Account::interestRate = initRate();
```

## 练习7.58
下面的静态数据成员的声明和定义有错误吗？请解释原因。

```cpp
//example.h
class Example {
public:
	static double rate = 6.5;
	static const int vecSize = 20;
	static vector<double> vec(vecSize);
};

//example.c
#include "example.h"
double Example::rate;
vector<double> Example::vec;
```

解：

`rate`应该是一个**常量表达式**。而类内只能初始化整型类型的静态常量，所以不能在类内初始化`vec`。修改后如下：

```cpp
// example.h
class Example {
public:
    static constexpr double rate = 6.5;
    static const int vecSize = 20;
    static vector<double> vec;
};

// example.C
#include "example.h"
constexpr double Example::rate;
vector<double> Example::vec(Example::vecSize);
```

# 第八章 IO库

## 练习8.1
> 编写函数，接受一个`istream&`参数，返回值类型也是`istream&`。此函数须从给定流中读取数据，直至遇到文件结束标识时停止。它将读取的数据打印在标准输出上。完成这些操作后，在返回流之前，对流进行复位，使其处于有效状态。

解：

```cpp
std::istream& func(std::istream &is)
{
    std::string buf;
    while (is >> buf)
        std::cout << buf << std::endl;
    is.clear();
    return is;
}
```

## 练习8.2
> 测试函数，调用参数为`cin`。

解：

```cpp
#include <iostream>
using std::istream;

istream& func(istream &is)
{
    std::string buf;
    while (is >> buf)
        std::cout << buf << std::endl;
    is.clear();
    return is;
}

int main()
{
    istream& is = func(std::cin);
    std::cout << is.rdstate() << std::endl;
    return 0;
}
```

## 练习8.3
> 什么情况下，下面的`while`循环会终止？

```cpp
while (cin >> i) /*  ...    */
```

解：

如`badbit`、`failbit`、`eofbit` 的任一个被置位，那么检测流状态的条件会失败。

## 练习8.4
> 编写函数，以读模式打开一个文件，将其内容读入到一个`string`的`vector`中，将每一行作为一个独立的元素存于`vector`中。

解：

```cpp
void ReadFileToVec(const string& fileName, vector<string>& vec)
{
    ifstream ifs(fileName);
    if (ifs)
    {
        string buf;
        while (getline(ifs, buf))
            vec.push_back(buf);
    }
}
```

## 练习8.5
> 重写上面的程序，将每个单词作为一个独立的元素进行存储。
解：

```cpp
void ReadFileToVec(const string& fileName, vector<string>& vec)
{
    ifstream ifs(fileName);
    if (ifs)
    {
        string buf;
        while (ifs >> buf)
            vec.push_back(buf);
    }
}
```

## 练习8.6
> 重写7.1.1节的书店程序，从一个文件中读取交易记录。将文件名作为一个参数传递给`main`。

解：

```cpp
#include <fstream>
#include <iostream>

#include "../ch07/ex7_26.h"
using std::ifstream; using std::cout; using std::endl; using std::cerr;

int main(int argc, char **argv)
{
    ifstream input(argv[1]);
    
    Sales_data total;
    if (read(input, total))
    {
        Sales_data trans;
        while (read(input, trans))
        {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else
            {
                print(cout, total) << endl;
                total = trans;
            }
        }
        print(cout, total) << endl;
    }
    else
    {
        cerr << "No data?!" << endl;
    }
    
    return 0;
}
```

## 练习8.7
> 修改上一节的书店程序，将结果保存到一个文件中。将输出文件名作为第二个参数传递给`main`函数。

解：

```cpp
#include <fstream>
#include <iostream>

#include "../ch07/ex7_26.h"
using std::ifstream; using std::ofstream; using std::endl; using std::cerr;

int main(int argc, char **argv)
{
    ifstream input(argv[1]);
    ofstream output(argv[2]);
    
    Sales_data total;
    if (read(input, total))
    {
        Sales_data trans;
        while (read(input, trans))
        {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else
            {
                print(output, total) << endl;
                total = trans;
            }
        }
        print(output, total) << endl;
    }
    else
    {
        cerr << "No data?!" << endl;
    }
    
    return 0;
}
```

## 练习8.8
> 修改上一题的程序，将结果追加到给定的文件末尾。对同一个输出文件，运行程序至少两次，检验数据是否得以保留。

解：

```cpp
#include <fstream>
#include <iostream>

#include "../ch07/ex7_26.h"
using std::ifstream; using std::ofstream; using std::endl; using std::cerr;

int main(int argc, char **argv)
{
    ifstream input(argv[1]);
    ofstream output(argv[2], ofstream::app);
    
    Sales_data total;
    if (read(input, total))
    {
        Sales_data trans;
        while (read(input, trans))
        {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else
            {
                print(output, total) << endl;
                total = trans;
            }
        }
        print(output, total) << endl;
    }
    else
    {
        cerr << "No data?!" << endl;
    }
    
    return 0;
}
```


## 练习8.9
> 使用你为8.1.2节第一个练习所编写的函数打印一个`istringstream`对象的内容。

解：

```cpp
#include <iostream>
#include <sstream>
using std::istream;

istream& func(istream &is)
{
    std::string buf;
    while (is >> buf)
        std::cout << buf << std::endl;
    is.clear();
    return is;
}

int main()
{
    std::istringstream iss("hello");
    func(iss);
    return 0;
}

```

## 练习8.10
> 编写程序，将来自一个文件中的行保存在一个`vector`中。然后使用一个`istringstream`从`vector`读取数据元素，每次读取一个单词。

解：

```cpp
#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>
#include <string>

using std::vector; using std::string; using std::ifstream; using std::istringstream; using std::cout; using std::endl; using std::cerr;

int main()
{
    ifstream ifs("../data/book.txt");
    if (!ifs)
    {
        cerr << "No data?" << endl;
        return -1;
    }
    
    vector<string> vecLine;
    string line;
    while (getline(ifs, line))
        vecLine.push_back(line);
    
    for (auto &s : vecLine)
    {
        istringstream iss(s);
        string word;
        while (iss >> word)
            cout << word << endl;
    }
    
    return 0;
}
```

## 练习8.11
> 本节的程序在外层`while`循环中定义了`istringstream`对象。如果`record`对象定义在循环之外，你需要对程序进行怎样的修改？重写程序，将`record`的定义移到`while`循环之外，验证你设想的修改方法是否正确。

解：

```cpp
#include <iostream>
#include <sstream>
#include <string>
#include <vector>
using std::vector; using std::string; using std::cin; using std::istringstream;

struct PersonInfo {
    string name;
    vector<string> phones;
};

int main()
{
    string line, word;
    vector<PersonInfo> people;
    istringstream record;
    while (getline(cin, line))
    {
        PersonInfo info;
        record.clear();
        record.str(line);
        record >> info.name;
        while (record >> word)
            info.phones.push_back(word);
        people.push_back(info);
    }
    
    for (auto &p : people)
    {
        std::cout << p.name << " ";
        for (auto &s : p.phones)
            std::cout << s << " ";
        std::cout << std::endl;
    }
    
    return 0;
}
```

## 练习8.12
> 我们为什么没有在`PersonInfo`中使用类内初始化？

解：

因为这里只需要聚合类就够了，所以没有必要在`PersionInfo`中使用类内初始化。

## 练习8.13
> 重写本节的电话号码程序，从一个命名文件而非`cin`读取数据。

解：

```cpp
#include <iostream>
#include <sstream>
#include <fstream>
#include <string>
#include <vector>

using std::vector; using std::string; using std::cin; using std::istringstream;
using std::ostringstream; using std::ifstream; using std::cerr; using std::cout; using std::endl;
using std::isdigit;

struct PersonInfo {
    string name;
    vector<string> phones;
};

bool valid(const string& str)
{
    return isdigit(str[0]);
}

string format(const string& str)
{
    return str.substr(0,3) + "-" + str.substr(3,3) + "-" + str.substr(6);
}

int main()
{
    ifstream ifs("../data/phonenumbers.txt");
    if (!ifs)
    {
        cerr << "no phone numbers?" << endl;
        return -1;
    }

    string line, word;
    vector<PersonInfo> people;
    istringstream record;
    while (getline(ifs, line))
    {
        PersonInfo info;
        record.clear();
        record.str(line);
        record >> info.name;
        while (record >> word)
            info.phones.push_back(word);
        people.push_back(info);
    }

    for (const auto &entry : people)
    {
        ostringstream formatted, badNums;
        for (const auto &nums : entry.phones)
            if (!valid(nums)) badNums << " " << nums;
            else formatted << " " << format(nums);
        if (badNums.str().empty())
            cout << entry.name << " " << formatted.str() << endl;
        else
            cerr << "input error: " << entry.name
                 << " invalid number(s) " << badNums.str() << endl;
    }

    return 0;
}
```

## 练习8.14
> 我们为什么将`entry`和`nums`定义为`const auto&`？

解：

它们都是类类型，因此使用引用避免拷贝。
在循环当中不会改变它们的值，因此用`const`。


## 练习9.1

> 对于下面的程序任务，`vector`、`deque`和`list`哪种容器最为适合？解释你的选择的理由。如果没有哪一种容器优于其他容器，也请解释理由。
* (a) 读取固定数量的单词，将它们按字典序插入到容器中。我们将在下一章中看到，关联容器更适合这个问题。
* (b) 读取未知数量的单词，总是将单词插入到末尾。删除操作在头部进行。
* (c) 从一个文件读取未知数量的整数。将这些数排序，然后将它们打印到标准输出。

解：

* (a) `list` ，因为需要频繁的插入操作。
* (b) `deque` ，总是在头尾进行插入、删除操作。
* (c) `vector` ，不需要进行插入删除操作。

## 练习9.2

> 定义一个`list`对象，其元素类型是`int`的`deque`。

解：


```cpp
std::list<std::deque<int>> l;
```

## 练习9.3

> 构成迭代器范围的迭代器有何限制？

解：

两个迭代器 `begin` 和 `end`需满足以下条件：
* 它们指向同一个容器中的元素，或者是容器最后一个元素之后的位置。
* 我们可以通过反复递增`begin`来到达`end`。换句话说，`end` 不在`begin`之前。

## 练习9.4

> 编写函数，接受一对指向`vector<int>`的迭代器和一个`int`值。在两个迭代器指定的范围中查找给定的值，返回一个布尔值来指出是否找到。

解：

```cpp
bool find(vector<int>::const_iterator begin, vector<int>::const_iterator end, int i)
{
	while (begin++ != end)
	{
		if (*begin == i) 
			return true;
    }	
    return false;
}
```

## 练习9.5

> 重写上一题的函数，返回一个迭代器指向找到的元素。注意，程序必须处理未找到给定值的情况。

解：

```cpp
vector<int>::const_iterator find(vector<int>::const_iterator begin, vector<int>::const_iterator end, int i)
{
	while (begin != end)
	{
		if (*begin == i) 
			return begin;
		++begin;
    }	
    return end;
}
```

## 练习9.6

> 下面的程序有何错误？你应该如何修改它？

```cpp
list<int> lst1;
list<int>::iterator iter1 = lst1.begin(),
					iter2 = lst1.end();
while (iter1 < iter2) /* ... */
```

解:

修改成如下：
```cpp
while (iter1 != iter2)
```

## 练习9.7

> 为了索引`int`的`vector`中的元素，应该使用什么类型？

解:

```cpp
vector<int>::size_type
```

## 练习9.8

> 为了读取`string`的`list`中的元素，应该使用什么类型？如果写入`list`，又应该使用什么类型？

解:

```cpp
list<string>::const_iterator // 读
list<string>::iterator // 写
```

## 练习9.9

> `begin`和`cbegin`两个函数有什么不同？

解:

`begin` 返回的是普通迭代器，`cbegin` 返回的是常量迭代器。

## 练习9.10

> 下面4个对象分别是什么类型？
```cpp
vector<int> v1;
const vector<int> v2;
auto it1 = v1.begin(), it2 = v2.begin();
auto it3 = v1.cbegin(), it4 = v2.cbegin();
```

解:

`it1` 是 `vector<int>::iterator`

`it2`，`it3` 和 `it4` 是 `vector<int>::const_iterator`


## 练习9.11

> 对6种创建和初始化`vector`对象的方法，每一种都给出一个实例。解释每个`vector`包含什么值。

解：

```cpp
vector<int> vec;    // 0
vector<int> vec(10);    // 10个0
vector<int> vec(10, 1);  // 10个1
vector<int> vec{ 1, 2, 3, 4, 5 }; // 1, 2, 3, 4, 5
vector<int> vec(other_vec); // 拷贝 other_vec 的元素
vector<int> vec(other_vec.begin(), other_vec.end()); // 拷贝 other_vec 的元素
```

## 练习9.12

> 对于接受一个容器创建其拷贝的构造函数，和接受两个迭代器创建拷贝的构造函数，解释它们的不同。

解：

* 接受一个容器创建其拷贝的构造函数，必须容器类型和元素类型都相同。
* 接受两个迭代器创建拷贝的构造函数，只需要元素的类型能够相互转换，容器类型和元素类型可以不同。

## 练习9.13

> 如何从一个`list<int>`初始化一个`vector<double>`？从一个`vector<int>`又该如何创建？编写代码验证你的答案。

解：

```cpp
list<int> ilst(5, 4);
vector<int> ivc(5, 5);

vector<double> dvc(ilst.begin(), ilst.end());
vector<double> dvc2(ivc.begin(), ivc.end());
```

## 练习9.14

> 编写程序，将一个`list`中的`char *`指针元素赋值给一个`vector`中的`string`。

解：

```cpp
    std::list<const char*> l{ "hello", "world" };
    std::vector<std::string> v;
    v.assign(l.cbegin(), l.cend());
```

## 练习9.15

> 编写程序，判定两个`vector<int>`是否相等。

解：

```cpp
    std::vector<int> vec1{ 1, 2, 3, 4, 5 };
    std::vector<int> vec2{ 1, 2, 3, 4, 5 };
    std::vector<int> vec3{ 1, 2, 3, 4 };

    std::cout << (vec1 == vec2 ? "true" : "false") << std::endl;
    std::cout << (vec1 == vec3 ? "true" : "false") << std::endl;
```

## 练习9.16

> 重写上一题的程序，比较一个list<int>中的元素和一个vector<int>中的元素。

解：

```cpp
    std::list<int>      li{ 1, 2, 3, 4, 5 };
    std::vector<int>    vec2{ 1, 2, 3, 4, 5 };
    std::vector<int>    vec3{ 1, 2, 3, 4 };

    std::cout << (std::vector<int>(li.begin(), li.end()) == vec2 ? "true" : "false") << std::endl;
    std::cout << (std::vector<int>(li.begin(), li.end()) == vec3 ? "true" : "false") << std::endl;
```

## 练习9.17

> 假定`c1`和`c2`是两个容器，下面的比较操作有何限制？

解：

```cpp
	if (c1 < c2)
```

* `c1`和`c2`必须是相同类型的容器并且保存相同类型的元素
* 元素类型要支持关系运算符

## 练习9.18

> 编写程序，从标准输入读取`string`序列，存入一个`deque`中。编写一个循环，用迭代器打印`deque`中的元素。

解：

```cpp
#include <iostream>
#include <string>
#include <deque>

using std::string; using std::deque; using std::cout; using std::cin; using std::endl;

int main()
{
    deque<string> input;
    for (string str; cin >> str; input.push_back(str));
    for (auto iter = input.cbegin(); iter != input.cend(); ++iter)
        cout << *iter << endl;

    return 0;
}
```

## 练习9.19

> 重写上一题的程序，用`list`替代`deque`。列出程序要做出哪些改变。

解：

只需要在声明上做出改变即可，其他都不变。
```cpp
deque<string> input; 
//改为
list<string> input;
```

## 练习9.20

> 编写程序，从一个`list<int>`拷贝元素到两个`deque`中。值为偶数的所有元素都拷贝到一个`deque`中，而奇数值元素都拷贝到另一个`deque`中。

解：

```cpp
#include <iostream>
#include <deque>
#include <list>
using std::deque; using std::list; using std::cout; using std::cin; using std::endl;

int main()
{
    list<int> l{ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
    deque<int> odd, even;
    for (auto i : l)
        (i & 0x1 ? odd : even).push_back(i);

    for (auto i : odd) cout << i << " ";
    cout << endl;
    for (auto i : even)cout << i << " ";
    cout << endl;

    return 0;
}
```

## 练习9.21

> 如果我们将第308页中使用`insert`返回值将元素添加到`list`中的循环程序改写为将元素插入到`vector`中，分析循环将如何工作。

解：

一样的。如书上所说：
> 第一次调用 `insert` 会将我们刚刚读入的 `string` 插入到 `iter` 所指向的元素之前的位置。`insert` 返回的迭代器恰好指向这个新元素。我们将此迭代器赋予 `iter` 并重复循环，读取下一个单词。只要继续有单词读入，每步 while 循环就会将一个新元素插入到 `iter` 之前，并将 `iter` 改变为新加入元素的尾置。此元素为（新的）首元素。因此，每步循环将一个元素插入到 `list` 首元素之前的位置。

## 练习9.22

> 假定`iv`是一个`int`的`vector`，下面的程序存在什么错误？你将如何修改？

解：

```cpp
vector<int>::iterator iter = iv.begin(),
					  mid = iv.begin() + iv.size() / 2;
while (iter != mid)
	if (*iter == some_val)
		iv.insert(iter, 2 * some_val);
```

解：

* 循环不会结束
* 迭代器可能会失效

要改为下面这样：
```cpp
while (iter != mid)
{
	if (*iter == some_val)
	{
		iter = v.insert(iter, 2 * some_val);
		++iter;
    }
	++iter;
}
```

## 练习9.23

> 在本节第一个程序中，若`c.size()` 为1，则`val`、`val2`、`val3`和`val4`的值会是什么？

解：

都会是同一个值（容器中仅有的那个）。

## 练习9.24

> 编写程序，分别使用`at`、下标运算符、`front` 和 `begin` 提取一个`vector`中的第一个元素。在一个空`vector`上测试你的程序。

解：

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v;
    std::cout << v.at(0);       // terminating with uncaught exception of type std::out_of_range
    std::cout << v[0];          // Segmentation fault: 11
    std::cout << v.front();     // Segmentation fault: 11
    std::cout << *v.begin();    // Segmentation fault: 11
    return 0;
}
```

## 练习9.25

> 对于第312页中删除一个范围内的元素的程序，如果 `elem1` 与 `elem2` 相等会发生什么？如果 `elem2` 是尾后迭代器，或者 `elem1` 和 `elem2` 皆为尾后迭代器，又会发生什么？

解：

* 如果 `elem1` 和 `elem2` 相等，那么不会发生任何操作。
* `如果elem2` 是尾后迭代器，那么删除从 `elem1` 到最后的元素。
* 如果两者皆为尾后迭代器，也什么都不会发生。

## 练习9.26

> 使用下面代码定义的`ia`，将`ia`拷贝到一个`vector`和一个`list`中。是用单迭代器版本的`erase`从`list`中删除奇数元素，从`vector`中删除偶数元素。

```cpp
int ia[] = { 0, 1, 1, 2, 3, 5, 8, 13, 21, 55, 89 };
```

解：

```cpp
vector<int> vec(ia, end(ia));
list<int> lst(vec.begin(), vec.end());

for (auto it = lst.begin(); it != lst.end(); )
	if (*it & 0x1)
		it = lst.erase(it);
	else 
		++it;

for (auto it = vec.begin(); it != vec.end(); )
	if (!(*it & 0x1))
		it = vec.erase(it);
	else
		++it;			
```

## 练习9.27

> 编写程序，查找并删除`forward_list<int>`中的奇数元素。

解：

```cpp
#include <iostream>
#include <forward_list>

using std::forward_list;
using std::cout;

auto remove_odds(forward_list<int>& flist)
{
    auto is_odd = [] (int i) { return i & 0x1; };
    flist.remove_if(is_odd);
}

int main()
{
    forward_list<int> data = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    remove_odds(data);
    for (auto i : data) 
        cout << i << " ";

    return 0;
}
```

## 练习9.28

> 编写函数，接受一个`forward_list<string>`和两个`string`共三个参数。函数应在链表中查找第一个`string`，并将第二个`string`插入到紧接着第一个`string`之后的位置。若第一个`string`未在链表中，则将第二个`string`插入到链表末尾。

```cpp
void find_and_insert(forward_list<string>& flst, const string& s1, const string& s2)
{
	auto prev = flst.before_begin();
	auto curr = flst.begin();
	while (curr != flst.end())
	{
		if (*curr == s1)
		{
			flst.insert_after(curr, s2);
			return;
	    }
	    prev = curr;
	    ++curr;
    }
    flst.insert_after(prev, s2);
}
```

## 练习9.29

> 假定`vec`包含25个元素，那么`vec.resize(100)`会做什么？如果接下来调用`vec.resize(10)`会做什么？

解：

* 将75个值为0的元素添加到`vec`的末尾
* 从`vec`的末尾删除90个元素

## 练习9.30

> 接受单个参数的`resize`版本对元素类型有什么限制（如果有的话）？

解：

元素类型必须提供一个默认构造函数。

## 练习9.31

> 第316页中删除偶数值元素并复制奇数值元素的程序不能用于`list`或`forward_list`。为什么？修改程序，使之也能用于这些类型。

解：

```cpp
iter += 2;
```

因为复合赋值语句只能用于`string`、`vector`、`deque`、`array`，所以要改为：

```cpp
++iter;
++iter;
```

如果是`forward_list`的话，要增加一个首先迭代器`prev`：

```cpp
auto prev = flst.before_begin();
//...
curr == flst.insert_after(prev, *curr);
++curr; ++curr;
++prev; ++prev;
```

## 练习9.32

> 在第316页的程序中，向下面语句这样调用`insert`是否合法？如果不合法，为什么？

```cpp
iter = vi.insert(iter, *iter++);
```

解：

不合法。因为参数的求值顺序是未指定的。

## 练习9.33

> 在本节最后一个例子中，如果不将`insert`的结果赋予`begin`，将会发生什么？编写程序，去掉此赋值语句，验证你的答案。

解：

`begin`将会失效。

```cpp
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> data { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    for(auto cur = data.begin(); cur != data.end(); ++cur)
        if(*cur & 0x1)
            cur = data.insert(cur, *cur), ++cur;
    
    for (auto i : data)
        cout << i << " ";

    return 0;
}
```

## 练习9.34

> 假定`vi`是一个保存`int`的容器，其中有偶数值也有奇数值，分析下面循环的行为，然后编写程序验证你的分析是否正确。

```cpp
iter = vi.begin();
while (iter != vi.end())
	if (*iter % 2)
		iter = vi.insert(iter, *iter);
	++iter;
```

解：

循环永远不会结束。

## 练习9.35

> 解释一个`vector`的`capacity`和`size`有何区别。

解：

* `capacity`的值表明，在不重新分配内存空间的情况下，容器可以保存多少元素
* 而`size`的值是指容器已经保存的元素的数量

## 练习9.36

> 一个容器的`capacity`可能小于它的`size`吗？

解：

不可能。

## 练习9.37

> 为什么`list`或`array`没有`capacity`成员函数？

解：

因为`list`是链表，而`array`不允许改变容器大小。

## 练习9.38

> 编写程序，探究在你的标准实现中，`vector`是如何增长的。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

int main()
{
	vector<int> v;

	for (int i = 0; i < 100; i++)
	{
		cout << "capacity: " << v.capacity() << "  size: " << v.size() << endl;
		v.push_back(i);
	}
	return 0;
}
```

输出：

```
capacity: 0  size: 0
capacity: 1  size: 1
capacity: 2  size: 2
capacity: 3  size: 3
capacity: 4  size: 4
capacity: 6  size: 5
capacity: 6  size: 6
capacity: 9  size: 7
capacity: 9  size: 8
capacity: 9  size: 9
capacity: 13  size: 10
capacity: 13  size: 11
capacity: 13  size: 12
capacity: 13  size: 13
capacity: 19  size: 14
capacity: 19  size: 15
capacity: 19  size: 16
capacity: 19  size: 17
capacity: 19  size: 18
capacity: 19  size: 19
capacity: 28  size: 20
capacity: 28  size: 21
capacity: 28  size: 22
capacity: 28  size: 23
capacity: 28  size: 24
capacity: 28  size: 25
capacity: 28  size: 26
capacity: 28  size: 27
capacity: 28  size: 28
capacity: 42  size: 29
capacity: 42  size: 30
capacity: 42  size: 31
capacity: 42  size: 32
capacity: 42  size: 33
capacity: 42  size: 34
capacity: 42  size: 35
capacity: 42  size: 36
capacity: 42  size: 37
capacity: 42  size: 38
capacity: 42  size: 39
capacity: 42  size: 40
capacity: 42  size: 41
capacity: 42  size: 42
capacity: 63  size: 43
capacity: 63  size: 44
capacity: 63  size: 45
capacity: 63  size: 46
capacity: 63  size: 47
capacity: 63  size: 48
capacity: 63  size: 49
capacity: 63  size: 50
capacity: 63  size: 51
capacity: 63  size: 52
capacity: 63  size: 53
capacity: 63  size: 54
capacity: 63  size: 55
capacity: 63  size: 56
capacity: 63  size: 57
capacity: 63  size: 58
capacity: 63  size: 59
capacity: 63  size: 60
capacity: 63  size: 61
capacity: 63  size: 62
capacity: 63  size: 63
capacity: 94  size: 64
capacity: 94  size: 65
capacity: 94  size: 66
capacity: 94  size: 67
capacity: 94  size: 68
capacity: 94  size: 69
capacity: 94  size: 70
capacity: 94  size: 71
capacity: 94  size: 72
capacity: 94  size: 73
capacity: 94  size: 74
capacity: 94  size: 75
capacity: 94  size: 76
capacity: 94  size: 77
capacity: 94  size: 78
capacity: 94  size: 79
capacity: 94  size: 80
capacity: 94  size: 81
capacity: 94  size: 82
capacity: 94  size: 83
capacity: 94  size: 84
capacity: 94  size: 85
capacity: 94  size: 86
capacity: 94  size: 87
capacity: 94  size: 88
capacity: 94  size: 89
capacity: 94  size: 90
capacity: 94  size: 91
capacity: 94  size: 92
capacity: 94  size: 93
capacity: 94  size: 94
capacity: 141  size: 95
capacity: 141  size: 96
capacity: 141  size: 97
capacity: 141  size: 98
capacity: 141  size: 99
```

## 练习9.39

> 解释下面程序片段做了什么：

```cpp
vector<string> svec;
svec.reserve(1024);
string word;
while (cin >> word)
	svec.push_back(word);
svec.resize(svec.size() + svec.size() / 2);
```

解：

定义一个`vector`，为它分配1024个元素的空间。然后通过一个循环从标准输入中读取字符串并添加到`vector`当中。循环结束后，改变`vector`的容器大小（元素数量）为原来的1.5倍，使用元素的默认初始化值填充。如果容器的大小超过1024，`vector`也会重新分配空间以容纳新增的元素。

## 练习9.40

> 如果上一题的程序读入了256个词，在`resize`之后容器的`capacity`可能是多少？如果读入了512个、1000个、或1048个呢？

解：

* 如果读入了256个词，`capacity` 仍然是 1024
* 如果读入了512个词，`capacity` 仍然是 1024
* 如果读入了1000或1048个词，`capacity` 取决于具体实现。

## 练习9.41

> 编写程序，从一个`vector<char>`初始化一个`string`。

解：

```cpp
    vector<char> v{ 'h', 'e', 'l', 'l', 'o' };
    string str(v.cbegin(), v.cend());
```

## 练习9.42

> 假定你希望每次读取一个字符存入一个`string`中，而且知道最少需要读取100个字符，应该如何提高程序的性能？

解：

使用 `reserve(100)` 函数预先分配100个元素的空间。

## 练习9.43

> 编写一个函数，接受三个`string`参数是`s`、`oldVal` 和`newVal`。使用迭代器及`insert`和`erase`函数将`s`中所有`oldVal`替换为`newVal`。测试你的程序，用它替换通用的简写形式，如，将"tho"替换为"though",将"thru"替换为"through"。

解：

```cpp
#include <iterator>
#include <iostream>
#include <string>
#include <cstddef>

using std::cout; 
using std::endl; 
using std::string;

auto replace_with(string &s, string const& oldVal, string const& newVal)
{
    for (auto cur = s.begin(); cur <= s.end() - oldVal.size(); )
        if (oldVal == string{ cur, cur + oldVal.size() })
            cur = s.erase(cur, cur + oldVal.size()),
            cur = s.insert(cur, newVal.begin(), newVal.end()),
            cur += newVal.size();
        else  
            ++cur;
}

int main()
{
    string s{ "To drive straight thru is a foolish, tho courageous act." };
    replace_with(s, "tho", "though");
    replace_with(s, "thru", "through");
    cout << s << endl;

    return 0;
}
```

## 练习9.44

> 重写上一题的函数，这次使用一个下标和`replace`。

解：

```cpp
#include <iostream>
#include <string>

using std::cout; 
using std::endl;
using std::string;

auto replace_with(string &s, string const& oldVal, string const& newVal)
{
    for (size_t pos = 0; pos <= s.size() - oldVal.size();)
        if (s[pos] == oldVal[0] && s.substr(pos, oldVal.size()) == oldVal)
            s.replace(pos, oldVal.size(), newVal),
            pos += newVal.size();
        else
            ++pos;
}

int main()
{
    string str{ "To drive straight thru is a foolish, tho courageous act." };
    replace_with(str, "tho", "though");
    replace_with(str, "thru", "through");
    cout << str << endl;
    return 0;
}
```

## 练习9.45

> 编写一个函数，接受一个表示名字的`string`参数和两个分别表示前缀（如"Mr."或"Mrs."）和后缀（如"Jr."或"III"）的字符串。使用迭代器及`insert`和`append`函数将前缀和后缀添加到给定的名字中，将生成的新`string`返回。

解：

```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;

// Exercise 9.45
auto add_pre_and_suffix(string name, string const& pre, string const& su)
{
    name.insert(name.begin(), pre.cbegin(), pre.cend());
    return name.append(su);
}

int main()
{
    string name("Wang");
    cout << add_pre_and_suffix(name, "Mr.", ", Jr.") << endl;
    return 0;
}
```

## 练习9.46

> 重写上一题的函数，这次使用位置和长度来管理`string`，并只使用`insert`。

解：

```cpp
#include <iostream>
#include <string>

auto add_pre_and_suffix(std::string name, std::string const& pre, std::string const& su)
{
    name.insert(0, pre);
    name.insert(name.size(), su);
    return name;
}

int main()
{
    std::string name("alan");
    std::cout << add_pre_and_suffix(name, "Mr.", ",Jr.");
    return 0;
}
```

## 练习9.47

> 编写程序，首先查找`string`"ab2c3d7R4E6"中每个数字字符，然后查找其中每个字母字符。编写两个版本的程序，第一个要使用`find_first_of`，第二个要使用`find_first_not_of`。

解：

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
	string numbers("0123456789");
	string s("ab2c3d7R4E6");

	cout << "numeric characters: ";
	for (int pos = 0; (pos = s.find_first_of(numbers, pos)) != string::npos; ++pos)
	{
		cout << s[pos] << " ";
	}

	cout << "\nalphabetic characters: ";
	for (int pos = 0; (pos = s.find_first_not_of(numbers, pos)) != string::npos; ++pos)
	{
		cout << s[pos] << " ";
	}

	return 0;
}
```

## 练习9.48

> 假定`name`和`numbers`的定义如325页所示，`numbers.find(name)`返回什么？

解：

返回 `string::npos`

## 练习9.49

> 如果一个字母延伸到中线之上，如d或f，则称其有上出头部分（`ascender`）。如果一个字母延伸到中线之下，如p或g，则称其有下出头部分（`descender`）。编写程序，读入一个单词文件，输出最长的既不包含上出头部分，也不包含下出头部分的单词。

解：

```cpp
#include <string>
#include <fstream>
#include <iostream>

using std::string; using std::cout; using std::endl; using std::ifstream;

int main()
{
    ifstream ifs("../data/letter.txt");
    if (!ifs) return -1;

    string longest;
    auto update_with = [&longest](string const& curr)
    {
        if (string::npos == curr.find_first_not_of("aceimnorsuvwxz"))
            longest = longest.size() < curr.size() ? curr : longest;
    };
    for (string curr; ifs >> curr; update_with(curr));
    cout << longest << endl;

    return 0;
}
```

## 练习9.50

> 编写程序处理一个`vector<string>`，其元素都表示整型值。计算`vector`中所有元素之和。修改程序，使之计算表示浮点值的`string`之和。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>

auto sum_for_int(std::vector<std::string> const& v)
{
    int sum = 0;
    for (auto const& s : v)
        sum += std::stoi(s);
    return sum;
}

auto sum_for_float(std::vector<std::string> const& v)
{
    float sum = 0.0;
    for (auto const& s : v)
        sum += std::stof(s);
    return sum;
}

int main()
{
    std::vector<std::string> v = { "1", "2", "3", "4.5" };
    std::cout << sum_for_int(v) << std::endl;
    std::cout << sum_for_float(v) << std::endl;

    return 0;
}
```

## 练习9.51

> 设计一个类，它有三个`unsigned`成员，分别表示年、月和日。为其编写构造函数，接受一个表示日期的`string`参数。你的构造函数应该能处理不同的数据格式，如January 1,1900、1/1/1990、Jan 1 1900 等。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;
class My_date{
private:
    unsigned year, month, day;
public:
    My_date(const string &s){

        unsigned tag;
        unsigned format;
        format = tag = 0;

        // 1/1/1900
        if(s.find_first_of("/")!= string :: npos)
        {
            format = 0x01;
        }

        // January 1, 1900 or Jan 1, 1900
        if((s.find_first_of(',') >= 4) && s.find_first_of(',')!= string :: npos){
            format = 0x10;
        }
        else{ // Jan 1 1900
            if(s.find_first_of(' ') >= 3
                && s.find_first_of(' ')!= string :: npos){
                format = 0x10;
                tag = 1;
            }
        }

        switch(format){

        case 0x01:
            day = stoi(s.substr(0, s.find_first_of("/")));
            month = stoi(s.substr(s.find_first_of("/") + 1, s.find_last_of("/")- s.find_first_of("/")));
            year = stoi(s.substr(s.find_last_of("/") + 1, 4));

        break;

        case 0x10:
            if( s.find("Jan") < s.size() )  month = 1;
            if( s.find("Feb") < s.size() )  month = 2;
            if( s.find("Mar") < s.size() )  month = 3;
            if( s.find("Apr") < s.size() )  month = 4;
            if( s.find("May") < s.size() )  month = 5;
            if( s.find("Jun") < s.size() )  month = 6;
            if( s.find("Jul") < s.size() )  month = 7;
            if( s.find("Aug") < s.size() )  month = 8;
            if( s.find("Sep") < s.size() )  month = 9;
            if( s.find("Oct") < s.size() )  month =10;
            if( s.find("Nov") < s.size() )  month =11;
            if( s.find("Dec") < s.size() )  month =12;

            char chr = ',';
            if(tag == 1){
                chr = ' ';
            }
            day = stoi(s.substr(s.find_first_of("123456789"), s.find_first_of(chr) - s.find_first_of("123456789")));

            year = stoi(s.substr(s.find_last_of(' ') + 1, 4));
            break;
        }
    }

    void print(){
        cout << "day:" << day << " " << "month: " << month << " " << "year: " << year;
    }
};
int main()
{
    My_date d("Jan 1 1900");
    d.print();
    return 0;
}
```

## 练习9.52

> 使用`stack`处理括号化的表达式。当你看到一个左括号，将其记录下来。当你在一个左括号之后看到一个右括号，从`stack`中`pop`对象，直至遇到左括号，将左括号也一起弹出栈。然后将一个值（括号内的运算结果）`push`到栈中，表示一个括号化的（子）表达式已经处理完毕，被其运算结果所替代。

解：

这道题可以延伸为逆波兰求值，以及中缀转后缀表达式。

```cpp
#include <stack>
#include <string>
#include <iostream>

using std::string; using std::cout; using std::endl; using std::stack;

int main()
{
    string expression{ "This is (pezy)." };
    bool bSeen = false;
    stack<char> stk;
    for (const auto &s : expression)
    {
        if (s == '(') { bSeen = true; continue; }
        else if (s == ')') bSeen = false;
        
        if (bSeen) stk.push(s);
    }
    
    string repstr;
    while (!stk.empty())
    {
        repstr += stk.top();
        stk.pop();
    }
    
    expression.replace(expression.find("(")+1, repstr.size(), repstr);
    
    cout << expression << endl;
    
    return 0;
}
```


# 第十章 泛型算法

## 练习10.1

> 头文件`algorithm`中定义了一个名为`count`的函数，它类似`find`， 接受一对迭代器和一个值作为参数。`count`返回给定值在序列中出现的次数。编写程序，读取`int`序列存入`vector`中，打印有多少个元素的值等于给定值。

解：

见下题

## 练习10.2

> 重做上一题，但读取 `string` 序列存入 `list` 中。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <list>


int main()
{
    // 10.1
    std::vector<int> v = { 1, 2, 3, 4, 5, 6, 6, 6, 2 };
    std::cout << "ex 10.01: " << std::count(v.cbegin(), v.cend(), 6) << std::endl;

    // 10.2
    std::list<std::string> l = { "aa", "aaa", "aa", "cc" };
    std::cout << "ex 10.02: " << std::count(l.cbegin(), l.cend(), "aa") << std::endl;

    return 0;
}
```

## 练习10.3

> 用 `accumulate`求一个 `vector<int>` 中元素之和。

解：

见下题。

## 练习10.4

> 假定 `v` 是一个`vector<double>`，那么调用 `accumulate(v.cbegin(),v.cend(),0)` 有何错误（如果存在的话）？

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <numeric>

int main()
{
    // Exercise 10.3
    std::vector<int> v = { 1, 2, 3, 4 };
    std::cout << "ex 10.03: " << std::accumulate(v.cbegin(), v.cend(), 0) << std::endl;

    // Exercise 10.4
    std::vector<double> vd = { 1.1, 0.5, 3.3 };
    std::cout   << "ex 10.04: "
                << std::accumulate(vd.cbegin(), vd.cend(), 0)
                << std::endl;                        //   ^<-- note here.
    // @attention
    //
    // The ouput is 4 rather than 4.9 as expected.
    // The reason is std::accumulate is a template function. The third parameter is _Tp __init
    // When "0" , an integer, had been specified here, the compiler deduced _Tp as
    // interger.As a result , when the following statments were being excuted :
    //  for (; __first != __last; ++__first)
    //	__init = __init + *__first;
    //  return __init;
    // all calculation would be converted to integer.

    return 0;
}
```

结果会是 `int` 类型。

## 练习10.5

> 在本节对名册（`roster`）调用`equal`的例子中，如果两个名册中保存的都是C风格字符串而不是`string`，会发生什么？

C风格字符串是用指向字符的指针表示的，因此会比较两个指针的值（地址），而不会比较这两个字符串的内容。

## 练习10.6

> 编写程序，使用 `fill_n` 将一个序列中的 `int` 值都设置为0。

解：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using std::vector; using std::cout; using std::endl; using std::fill_n;

int main()
{
    vector<int> vec{ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    fill_n(vec.begin(), vec.size(), 0);

    for (auto i : vec)
        cout << i << " ";
    cout << endl;
}
```

## 练习10.7

> 下面程序是否有错误？如果有，请改正：

```cpp
(a) vector<int> vec; list<int> lst; int i;
	while (cin >> i)
		lst.push_back(i);
	copy(lst.cbegin(), lst.cend(), vec.begin());
(b) vector<int> vec;
	vec.reserve(10);
	fill_n(vec.begin(), 10, 0);
```

解：

* (a) 应该加一条语句 `vec.resize(lst.size())` 。`copy`时必须保证目标目的序列至少要包含与输入序列一样多的元素。
* (b) reserve分配了足够的空间，但是不会创建新元素。算法可能改变容器中保存的元素的值，也可能在容器内移动元素，永远不会直接添加和删除元素(c++ priemr中文版第五版 P338)，所以此处应该改为resize(10)。

## 练习10.8

> 本节提到过，标准库算法不会改变它们所操作的容器的大小。为什么使用 `back_inserter`不会使这一断言失效？

`back_inserter` 是插入迭代器，在 `iterator.h` 头文件中，不是标准库的算法。

## 练习10.9

> 实现你自己的`elimDups`。分别在读取输入后、调用`unique`后以及调用`erase`后打印`vector`的内容。

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

// print containers like vector, deque, list, etc.
template<typename Sequence>
auto println(Sequence const& seq) -> std::ostream&
{
    for (auto const& elem : seq) 
        std::cout << elem << " ";
    return std::cout << std::endl;
}

auto eliminate_duplicates(std::vector<std::string> &vs) -> std::vector<std::string>&
{
    std::sort(vs.begin(), vs.end());
    println(vs);

    auto new_end = std::unique(vs.begin(), vs.end());
    println(vs);

    vs.erase(new_end, vs.end());
    return vs;
}

int main()
{
    std::vector<std::string> vs{ "a", "v", "a", "s", "v", "a", "a" };
    println(vs);
    println(eliminate_duplicates(vs));

    return 0;
}
```

## 练习10.10

> 你认为算法不改变容器大小的原因是什么？

解：

* 将算法和容器的成员函数区分开。
* 算法的参数是迭代器，不是容器本身。

## 练习10.11

> 编写程序，使用 `stable_sort` 和 `isShorter` 将传递给你的 `elimDups` 版本的 `vector` 排序。打印 `vector`的内容，验证你的程序的正确性。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <numeric>
#include <list>

// print a container like vector, deque, list, etc.
template<typename Sequence>
inline std::ostream& println(Sequence const& seq)
{
    for(auto const& elem : seq) std::cout << elem << " ";
    std::cout << std::endl;

    return std::cout;
}


inline bool
is_shorter(std::string const& lhs, std::string const& rhs)
{
    return  lhs.size() < rhs.size();
}


void elimdups(std::vector<std::string> &vs)
{
    std::sort(vs.begin(), vs.end());
    auto new_end = std::unique(vs.begin(), vs.end());
    vs.erase(new_end, vs.end());
}


int main()
{
    std::vector<std::string> v{
        "1234", "1234", "1234", "Hi", "alan", "wang"
    };
    elimdups(v);
    std::stable_sort(v.begin(), v.end(), is_shorter);
    std::cout << "ex10.11 :\n";
    println(v);

    return 0;
}
```

## 练习10.12

> 编写名为 `compareIsbn` 的函数，比较两个 `Sales_data` 对象的`isbn()` 成员。使用这个函数排序一个保存 `Sales_data` 对象的 `vector`。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include "../ch07/ex7_26.h"     // Sales_data class.

inline bool compareIsbn(const Sales_data &sd1, const Sales_data &sd2)
{
    return sd1.isbn().size() < sd2.isbn().size();
}

int main()
{
    Sales_data d1("aa"), d2("aaaa"), d3("aaa"), d4("z"), d5("aaaaz");
    std::vector<Sales_data> v{ d1, d2, d3, d4, d5 };

    // @note   the elements the iterators pointing to
    //         must match the parameters of the predicate.
    std::sort(v.begin(), v.end(), compareIsbn);

    for(const auto &element : v)
        std::cout << element.isbn() << " ";
    std::cout << std::endl;

    return 0;
}
```

## 练习10.13

> 标准库定义了名为 `partition` 的算法，它接受一个谓词，对容器内容进行划分，使得谓词为`true` 的值会排在容器的前半部分，而使得谓词为 `false` 的值会排在后半部分。算法返回一个迭代器，指向最后一个使谓词为 `true` 的元素之后的位置。编写函数，接受一个 `string`，返回一个 `bool` 值，指出 `string` 是否有5个或更多字符。使用此函数划分 `words`。打印出长度大于等于5的元素。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

bool predicate(const std::string &s) 
{ 
    return s.size() >= 5; 
}

int main()
{
    auto v = std::vector<std::string>{ "a", "as", "aasss", "aaaaassaa", "aaaaaabba", "aaa" };
    auto pivot = std::partition(v.begin(), v.end(), predicate);
    
    for (auto it = v.cbegin(); it != pivot; ++it)
        std::cout << *it << " ";
    std::cout << std::endl;

    return 0;
}
```

## 练习10.14

> 编写一个`lambda`，接受两个`int`，返回它们的和。

```cpp
auto f = [](int i, int j) { return i + j; };
```

## 练习10.15

> 编写一个 `lambda` ，捕获它所在函数的 `int`，并接受一个 `int`参数。`lambda` 应该返回捕获的 `int` 和 `int` 参数的和。

解：

```cpp
int x = 10;
auto f = [x](int i) { i + x; };
```

## 练习10.16

> 使用 `lambda` 编写你自己版本的 `biggies`。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

// from ex 10.9
void elimdups(std::vector<std::string> &vs)
{
    std::sort(vs.begin(), vs.end());
    auto new_end = std::unique(vs.begin(), vs.end());
    vs.erase(new_end, vs.end());
}

void biggies(std::vector<std::string> &vs, std::size_t sz)
{
    using std::string;

    elimdups(vs);

    // sort by size, but maintain alphabetical order for same size.
    std::stable_sort(vs.begin(), vs.end(), [](string const& lhs, string const& rhs){
        return lhs.size() < rhs.size();
    });

    // get an iterator to the first one whose size() is >= sz
    auto wc = std::find_if(vs.begin(), vs.end(), [sz](string const& s){
            return s.size() >= sz;
    });
        
    // print the biggies
    std::for_each(wc, vs.end(), [](const string &s){
        std::cout << s << " ";
    }); 
}

int main()
{
    // ex10.16
    std::vector<std::string> v
    {
        "1234","1234","1234","hi~", "alan", "alan", "cp"
    };
    std::cout << "ex10.16: ";
    biggies(v, 3);
    std::cout << std::endl;

    return 0;
}
```

## 练习10.17

> 重写10.3.1节练习10.12的程序，在对`sort`的调用中使用 `lambda` 来代替函数 `compareIsbn`。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include "../ch07/ex7_26.h"     // Sales_data class.

int main()
{
    Sales_data d1("aa"), d2("aaaa"), d3("aaa"), d4("z"), d5("aaaaz");
    std::vector<Sales_data> v{ d1, d2, d3, d4, d5 };

    std::sort(v.begin(), v.end(), [](const Sales_data &sd1, const Sales_data &sd2){
        return sd1.isbn().size() < sd2.isbn().size();
    });

    for(const auto &element : v)
        std::cout << element.isbn() << " ";
    std::cout << std::endl;

    return 0;
}
```

## 练习10.18

> 重写 `biggies`，用 `partition` 代替 `find_if`。我们在10.3.1节练习10.13中介绍了 `partition` 算法。

解：

见下题

## 练习10.19

> 用 `stable_partition` 重写前一题的程序，与 `stable_sort` 类似，在划分后的序列中维持原有元素的顺序。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

// from ex 10.9
void elimdups(std::vector<std::string> &vs)
{
    std::sort(vs.begin(), vs.end());
    auto new_end = std::unique(vs.begin(), vs.end());
    vs.erase(new_end, vs.end());
}


// ex10.18
void biggies_partition(std::vector<std::string> &vs, std::size_t sz)
{
    elimdups(vs);
    
    auto pivot = partition(vs.begin(), vs.end(), [sz](const std::string &s){
        return s.size() >= sz;}
    );

    for(auto it = vs.cbegin(); it != pivot; ++it)
        std::cout << *it << " ";
}


// ex10.19
void biggies_stable_partition(std::vector<std::string> &vs, std::size_t sz)
{
    elimdups(vs);
    
    auto pivot = stable_partition(vs.begin(), vs.end(), [sz](const std::string& s){
        return s.size() >= sz;
    });

    for(auto it = vs.cbegin(); it != pivot; ++it)
        std::cout << *it << " ";
}


int main()
{
    // ex10.18
    std::vector<std::string> v{
        "the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"
    };
    
    std::cout << "ex10.18: ";
    std::vector<std::string> v1(v);
    biggies_partition(v1, 4);
    std::cout << std::endl;

    // ex10.19
    std::cout << "ex10.19: ";
    std::vector<std::string> v2(v);
    biggies_stable_partition(v2, 4);
    std::cout << std::endl;

    return 0;
}
```

## 练习10.20

> 标准库定义了一个名为 `count_if` 的算法。类似 `find_if`，此函数接受一对迭代器，表示一个输入范围，还接受一个谓词，会对输入范围中每个元素执行。`count_if`返回一个计数值，表示谓词有多少次为真。使用`count_if`重写我们程序中统计有多少单词长度超过6的部分。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>


using std::vector;
using std::count_if;
using std::string;


// Exercise 10.20
std::size_t bigerThan6(vector<string> const& v)
{
    return count_if(v.cbegin(), v.cend(), [](string const& s){
        return s.size() > 6;
    });
}


int main()
{
    // ex10.20
    vector<string> v{
        "alan","moophy","1234567","1234567","1234567","1234567"
    };
    std::cout << "ex10.20: " << bigerThan6(v) << std::endl;

    // ex10.21
    int i = 7;
    auto check_and_decrement = [&i]() { return i > 0 ? !--i : !i; };
    std::cout << "ex10.21: ";
    while(!check_and_decrement())
        std::cout << i << " ";
    std::cout << i << std::endl;

    return 0;
}
```

## 练习10.21

> 编写一个 `lambda`，捕获一个局部 `int` 变量，并递减变量值，直至它变为0。一旦变量变为0，再调用`lambda`应该不再递减变量。`lambda`应该返回一个`bool`值，指出捕获的变量是否为0。

解：

```cpp
	int i = 10;
	auto f = [&i]() -> bool { return (i == 0 ? true : !(i--)); };
	while (!f()) cout << i << endl;
```

## 练习10.22

> 重写统计长度小于等于6的单词数量的程序，使用函数代替`lambda`。

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <functional>

using std::string;
using namespace std::placeholders;

bool isLesserThanOrEqualTo6(const string &s, string::size_type sz)
{
    return s.size() <= sz;
}

int main()
{
    std::vector<string> authors{ "Mooophy", "pezy", "Queequeg90", "shbling", "evan617" };
    std::cout << count_if(authors.cbegin(), authors.cend(), bind(isLesserThanOrEqualTo6, _1, 6));
}
```

## 练习10.23

> `bind` 接受几个参数？

假设被绑定的函数接受 `n` 个参数，那么`bind` 接受`n + 1`个参数。

## 练习10.24
> 给定一个`string`，使用 `bind` 和 `check_size` 在一个 `int` 的`vector` 中查找第一个大于`string`长度的值。

解：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>


using std::cout;
using std::endl;
using std::string;
using std::vector;
using std::find_if;
using std::bind;
using std::size_t;

auto check_size(string const& str, size_t sz)
{
    return str.size() < sz;
}

int main()
{
    vector<int> vec{ 0, 1, 2, 3, 4, 5, 6, 7 };
    string str("123456");
    auto result = find_if(vec.begin(), vec.end(), bind(check_size, str, std::placeholders::_1));
    if (result != vec.end())
        cout << *result << endl;
    else
        cout << "Not found" << endl;

    return 0;
}
```

## 练习10.25

> 在10.3.2节的练习中，编写了一个使用`partition` 的`biggies`版本。使用 `check_size` 和 `bind` 重写此函数。

解：

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <functional>

using std::string; using std::vector;
using namespace std::placeholders;

void elimdups(vector<string> &vs)
{
    std::sort(vs.begin(), vs.end());
    vs.erase(unique(vs.begin(), vs.end()), vs.end());
}

bool check_size(const string &s, string::size_type sz)
{
    return s.size() >= sz;
}

void biggies(vector<string> &words, vector<string>::size_type sz)
{
    elimdups(words);
    auto iter = std::stable_partition(words.begin(), words.end(), bind(check_size, _1, sz));
    for_each(words.begin(), iter, [](const string &s){ std::cout << s << " "; });
}

int main()
{
    std::vector<std::string> v{
        "the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"
    };
    biggies(v, 4);
}
```

## 练习10.26

> 解释三种插入迭代器的不同之处。

解：

* `back_inserter` 使用 `push_back` 。
* `front_inserter` 使用 `push_front` 。
* `inserter` 使用 `insert`，此函数接受第二个参数，这个参数必须是一个指向给定容器的迭代器。元素将被插入到给定迭代器所表示的元素之前。

## 练习10.27

> 除了 `unique` 之外，标准库还定义了名为 `unique_copy` 的函数，它接受第三个迭代器，表示拷贝不重复元素的目的位置。编写一个程序，使用 `unique_copy`将一个`vector`中不重复的元素拷贝到一个初始化为空的`list`中。

解：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <list>
#include <iterator>

int main()
{
    std::vector<int> vec{ 1, 1, 3, 3, 5, 5, 7, 7, 9 };
    std::list<int> lst;
    
    std::unique_copy(vec.begin(), vec.end(), back_inserter(lst));
    for (auto i : lst)
        std::cout << i << " ";
    std::cout << std::endl;
}
```

## 练习10.28

> 一个`vector` 中保存 1 到 9，将其拷贝到三个其他容器中。分别使用`inserter`、`back_inserter` 和 `front_inserter` 将元素添加到三个容器中。对每种 `inserter`，估计输出序列是怎样的，运行程序验证你的估计是否正确。

解：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <list>
#include <iterator>
using std::list; using std::copy; using std::cout; using std::endl;

template<typename Sequence>
void print(Sequence const& seq)
{
    for (const auto& i : seq)
        std::cout << i << " ";
    std::cout << std::endl;
}

int main()
{
    std::vector<int> vec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    
    // uses inserter
    list<int> lst1;
    copy(vec.cbegin(), vec.cend(), inserter(lst1, lst1.begin()));
    print(lst1);
    
    // uses back_inserter
    list<int> lit2;
    copy(vec.cbegin(), vec.cend(), back_inserter(lit2));
    print(lit2);
    
    // uses front_inserter
    list<int> lst3;
    copy(vec.cbegin(), vec.cend(), front_inserter(lst3));
    print(lst3);
}
```

前两种为正序，第三种为逆序，输出和预计一样。

## 练习10.29

> 编写程序，使用流迭代器读取一个文本文件，存入一个`vector`中的`string`里。

解：

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <iterator>

using std::string;

int main()
{
    std::ifstream ifs("../data/book.txt");
    std::istream_iterator<string> in(ifs), eof;
    std::vector<string> vec;
    std::copy(in, eof, back_inserter(vec));
    
    // output
    std::copy(vec.cbegin(), vec.cend(), std::ostream_iterator<string>(std::cout, "\n"));
}
```

## 练习10.30

> 使用流迭代器、`sort` 和 `copy` 从标准输入读取一个整数序列，将其排序，并将结果写到标准输出。

解：

```cpp

#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

using namespace std;

int main()
{
	vector<int> v;
	istream_iterator<int> int_it(cin), int_eof;

	copy(int_it, int_eof, back_inserter(v));
	sort(v.begin(), v.end());

	copy(v.begin(), v.end(), ostream_iterator<int>(cout," "));
	cout << endl;
	return 0;
}
```

## 练习10.31

> 修改前一题的程序，使其只打印不重复的元素。你的程序应该使用 `unique_copy`。

解：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

using namespace std;

int main()
{
	vector<int> v;
	istream_iterator<int> int_it(cin), int_eof;
	
	copy(int_it, int_eof, back_inserter(v));
	sort(v.begin(), v.end());
	unique(v.begin(), v.end());
	copy(v.begin(), v.end(), ostream_iterator<int>(cout, " "));
	cout << endl;
	return 0;
}
```

## 练习10.32

> 重写1.6节中的书店程序，使用一个`vector`保存交易记录，使用不同算法完成处理。使用 `sort` 和10.3.1节中的 `compareIsbn` 函数来排序交易记录，然后使用 `find` 和 `accumulate` 求和。

解：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>
#include <numeric>
#include "../include/Sales_item.h"

int main()
{
    std::istream_iterator<Sales_item> in_iter(std::cin), in_eof;
    std::vector<Sales_item> vec;
    
    while (in_iter != in_eof)
        vec.push_back(*in_iter++);
    sort(vec.begin(), vec.end(), compareIsbn);
    for (auto beg = vec.cbegin(), end = beg; beg != vec.cend(); beg = end) {
        end = find_if(beg, vec.cend(), [beg](const Sales_item &item){ return item.isbn() != beg->isbn(); });
        std::cout << std::accumulate(beg, end, Sales_item(beg->isbn())) << std::endl;
    }
}
```

## 练习10.33

> 编写程序，接受三个参数：一个输入文件和两个输出文件的文件名。输入文件保存的应该是整数。使用 `istream_iterator` 读取输入文件。使用 `ostream_iterator` 将奇数写入第一个输入文件，每个值后面都跟一个空格。将偶数写入第二个输出文件，每个值都独占一行。

解：

```cpp
#include <fstream>
#include <iterator>
#include <algorithm>

int main(int argc, char **argv)
{
	if (argc != 4) return -1;

	std::ifstream ifs(argv[1]);
	std::ofstream ofs_odd(argv[2]), ofs_even(argv[3]);

	std::istream_iterator<int> in(ifs), in_eof;
	std::ostream_iterator<int> out_odd(ofs_odd, " "), out_even(ofs_even, "\n");

	std::for_each(in, in_eof, [&out_odd, &out_even](const int i)
	{
		*(i & 0x1 ? out_odd : out_even)++ = i;
	});

	return 0;
}
```

## 练习10.34

> 使用 `reverse_iterator` 逆序打印一个`vector`。

解：

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
	vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	for (auto it = v.crbegin(); it != v.crend(); ++it)
	{
		cout << *it << endl;
	}

	return 0;
}
```

## 练习10.35

> 使用普通迭代器逆序打印一个`vector`。

解：

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
	vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	for (auto it = std::prev(v.cend()); true; --it)
	{
		std::cout << *it << " ";
		if (it == v.cbegin()) break;
	}
	std::cout << std::endl;

	return 0;
}
```

## 练习10.36

> 使用 `find` 在一个 `int` 的`list` 中查找最后一个值为0的元素。

解：

```cpp
#include <iostream>
#include <list>
#include <algorithm>

using namespace std;

int main()
{
	list<int> l = { 1, 2, 0, 4, 5, 6, 7, 0, 9 };
	auto it = find(l.crbegin(), l.crend(), 0);

	cout << distance(it, l.crend()) << endl;
	return 0;
}
```

## 练习10.37

> 给定一个包含10 个元素的`vector`，将位置3到7之间的元素按逆序拷贝到一个`list`中。

解：

```cpp
#include <iostream>
#include <list>
#include <algorithm>
#include <vector>
#include <iterator>

using namespace std;

int main()
{
	vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	list<int> l;

	copy(v.crbegin() + 3, v.crbegin() + 8, back_inserter(l));

	for (auto i : l) std::cout << i << " ";
	cout << endl;
	return 0;
}
```

## 练习10.38

> 列出5个迭代器类别，以及每类迭代器所支持的操作。

* 输入迭代器 : `==`,`!=`,`++`,`*`,`->`
* 输出迭代器 : `++`,`*`
* 前向迭代器 : `==`,`!=`,`++`,`*`,`->`
* 双向迭代器 : `==`,`!=`,`++`,`--`,`*`,`->`
* 随机访问迭代器 : `==`,`!=`,`<`,`<=`,`>`,`>=`,`++`,`--`,`+`,`+=`,`-`,`-=`,`*`,`->`,`iter[n]`==`*(iter[n])`

## 练习10.39

> `list` 上的迭代器属于哪类？`vector`呢？

* `list` 上的迭代器是 **双向迭代器**
* `vector` 上的迭代器是 **随机访问迭代器**

## 练习10.40

> 你认为 `copy` 要求哪类迭代器？`reverse` 和 `unique` 呢？

* `copy` 需要两个**输入迭代器**，一个**输出迭代器**
* `reverse` 需要**双向迭代器**
* `unique`需要**随机访问迭代器**

## 练习10.41

> 仅根据算法和参数的名字，描述下面每个标准库算法执行什么操作：

```cpp
replace(beg, end, old_val, new_val);
replace_if(beg, end, pred, new_val);
replace_copy(beg, end, dest, old_val, new_val);
replace_copy_if(beg, end, dest, pred, new_val);
```

解：

* `replace` 在迭代器范围内用新值替换所有原来的旧值。
* `replace_if` 在迭代器范围内，满足谓词条件的元素用新值替换。
* `replace_copy` 复制迭代器范围内的元素到目标迭代器位置，如果元素等于某个旧值，则用新值替换
* `replace_copy_if` 复制迭代器范围内的元素到目标迭代器位置，满足谓词条件的元素用新值替换

## 练习10.42

> 使用 `list` 代替 `vector` 重新实现10.2.3节中的去除重复单词的程序。

解：

```cpp
#include <iostream>
#include <string>
#include <list>

using std::string; using std::list;

void elimDups(list<string> &words)
{
	words.sort();
	words.unique();
}

int main()
{
	list<string> l = { "aa", "aa", "aa", "aa", "aasss", "aa" };
	elimDups(l);
	for (const auto& e : l)
		std::cout << e << " ";
	std::cout << std::endl;
}
```

# 第十一章 关联容器

## 练习11.1

> 描述`map`和`vector`的不同。

解：

`map` 是关联容器， `vector` 是顺序容器。

## 练习11.2

> 分别给出最适合使用`list`、`vector`、`deque`、`map`以及`set`的例子。

解：

* `list`：双向链表，适合频繁插入删除元素的场景。
* `vector`：适合频繁访问元素的场景。
* `deque`：双端队列，适合频繁在头尾插入删除元素的场景。
* `map`：字典。
* `set`：适合有序不重复的元素的场景。

## 练习11.3

> 编写你自己的单词计数程序。

解：

```cpp
#include <string>
#include <map>
#include <iostream>

using namespace std;

int main(){
    map<string, int> word_count;
    string tmp;
    while (cin >> tmp){
        word_count[tmp] += 1;
    }
    for (const auto& elem : word_count)
		std::cout << elem.first << " : " << elem.second << endl;
	return 0;
}
```

## 练习11.4

> 扩展你的程序，忽略大小写和标点。例如，"example."、"example,"和"Example"应该递增相同的计数器。

解：

```cpp
#include <iostream>
#include <map>
#include <string>
#include <algorithm>
#include <cctype>

void word_count_pro(std::map<std::string, int>& m)
{
	std::string word;
	while (std::cin >> word)
	{
		for (auto& ch : word) 
			ch = tolower(ch);
		
		word.erase(std::remove_if(word.begin(), word.end(), ispunct),
			word.end());
		++m[word];
	}
	for (const auto& e : m) std::cout << e.first << " : " << e.second << "\n";
}

int main()
{
	std::map<std::string, int> m;
	word_count_pro(m);

	return 0;
}
```

## 练习11.5

> 解释`map`和`set`的区别。你如何选择使用哪个？

解：

`map` 是键值对，而 `set` 只有键没有值。当我需要存储键值对的时候使用 `map`，而只需要键的时候使用 `set`。

## 练习11.6

> 解释`set`和`list`的区别。你如何选择使用哪个？

`set` 是有序不重复集合，底层实现是红黑树，而 `list` 是无序可重复集合，底层实现是链表。

## 练习11.7

> 定义一个`map`，关键字是家庭的姓，值是一个`vector`，保存家中孩子（们）的名。编写代码，实现添加新的家庭以及向已有家庭中添加新的孩子。

解：

```cpp
map<string, vector<string>> m;
for (string ln; cout << "Last name:\n", cin >> ln && ln != "@q";)
	for (string cn; cout << "|-Children's names:\n", cin >> cn && cn != "@q";)
		m[ln].push_back(cn);
```

## 练习11.8

> 编写一个程序，在一个`vector`而不是一个`set`中保存不重复的单词。使用`set`的优点是什么？

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

int main()
{
	std::vector<std::string> exclude = { "aa", "bb", "cc", "dd", "ee", "ff" };
	for (std::string word; std::cout << "Enter plz:\n", std::cin >> word;)
	{
		auto is_excluded = std::binary_search(exclude.cbegin(), exclude.cend(), word);
		auto reply = is_excluded ? "excluded" : "not excluded";
		std::cout << reply << std::endl;
	}
	return 0;
}
```

`set`的优点是集合本身的元素就是不重复。

## 练习11.9

> 定义一个`map`，将单词与一个行号的`list`关联，`list`中保存的是单词所出现的行号。

解：

```cpp
std::map<std::string, std::list<std::size_t>> m;
```

## 练习11.10

> 可以定义一个`vector<int>::iterator` 到 `int` 的`map`吗？`list<int>::iterator` 到 `int` 的`map`呢？对于两种情况，如果不能，解释为什么。

解：

可以定义 `vector<int>::iterator` 到 `int` 的`map`，但是不能定义 `list<int>::iterator` 到 `int` 的`map`。因为`map`的键必须实现 `<` 操作，`list` 的迭代器不支持比较运算。

## 练习11.11

> 不使用`decltype` 重新定义 `bookstore`。

解：

```cpp
using Less = bool (*)(Sales_data const&, Sales_data const&);
std::multiset<Sales_data, Less> bookstore(less);
```

## 练习11.12

> 编写程序，读入`string`和`int`的序列，将每个`string`和`int`存入一个`pair` 中，`pair`保存在一个`vector`中。

解：

```cpp
#include <vector>
#include <utility>
#include <string>
#include <iostream>

int main()
{
	std::vector<std::pair<std::string, int>> vec;
	std::string str;
	int i;
	while (std::cin >> str >> i)
		vec.push_back(std::pair<std::string, int>(str, i));

	for (const auto &p : vec)
		std::cout << p.first << ":" << p.second << std::endl;
}
```

## 练习11.13

> 在上一题的程序中，至少有三种创建`pair`的方法。编写此程序的三个版本，分别采用不同的方法创建`pair`。解释你认为哪种形式最易于编写和理解，为什么？

解：

```cpp
vec.push_back(std::make_pair(str, i));
vec.push_back({ str, i });
vec.push_back(std::pair<string, int>(str, i)); 
```

使用花括号的初始化器最易于理解和编写。

## 练习11.14

> 扩展你在11.2.1节练习中编写的孩子姓达到名的`map`，添加一个`pair`的`vector`，保存孩子的名和生日。

解：

```cpp
#include <iostream>
#include <map>
#include <string>
#include <vector>

using std::ostream;
using std::cout;
using std::cin;
using std::endl;
using std::string;
using std::make_pair;
using std::pair;
using std::vector;
using std::map;

class Families
{
public:
	using Child = pair<string, string>;
	using Children = vector<Child>;
	using Data = map<string, Children>;

	void add(string const& last_name, string const& first_name, string birthday)
	{
		auto child = make_pair(first_name, birthday);
		_data[last_name].push_back(child);
	}

	void print() const
	{
		for (auto const& pair : _data)
		{
			cout << pair.first << ":\n";
			for (auto const& child : pair.second)
				cout << child.first << " " << child.second << endl;
			cout << endl;
		}
	}

private:
	Data _data;
};

int main()
{
	Families families;
	auto msg = "Please enter last name, first name and birthday:\n";
	for (string l, f, b; cout << msg, cin >> l >> f >> b; families.add(l, f, b));
	families.print();

	return 0;
}
```

## 练习11.15

> 对一个`int`到`vector<int>的map`，其`mapped_type`、`key_type`和 `value_type`分别是什么？

解：

* `mapped_type` : `vector<int>`
* `key_type` : `int`
* `value_type` : `std::pair<const int,vector >`

## 练习11.16

> 使用一个`map`迭代器编写一个表达式，将一个值赋予一个元素。

解：

```cpp
std::map<int, string>::iterator it = m.begin();
it->second = "hello";
```

## 练习11.17

> 假定`c`是一个`string`的`multiset`，`v` 是一个`string` 的`vector`，解释下面的调用。指出每个调用是否合法：

```cpp
copy(v.begin(), v.end(), inserter(c, c.end()));
copy(v.begin(), v.end(), back_inserter(c));
copy(c.begin(), c.end(), inserter(v, v.end()));
copy(c.begin(), c.end(), back_inserter(v));
```

解：

第二个调用不合法，因为 `multiset` 没有 `push_back` 方法。其他调用都合法。

## 练习11.18

> 写出第382页循环中`map_it` 的类型，不要使用`auto` 或 `decltype`。

解：

```cpp
map<string, size_t>::const_iterator map_it = word_count.cbegin();
```

## 练习11.19

> 定义一个变量，通过对11.2.2节中的名为 `bookstore` 的`multiset` 调用`begin()`来初始化这个变量。写出变量的类型，不要使用`auto` 或 `decltype`。

解：

```cpp
using compareType = bool (*)(const Sales_data &lhs, const Sales_data &rhs);
std::multiset<Sales_data, compareType> bookstore(compareIsbn);
std::multiset<Sales_data, compareType>::iterator c_it = bookstore.begin();
```

## 练习11.20

> 重写11.1节练习的单词计数程序，使用`insert`代替下标操作。你认为哪个程序更容易编写和阅读？解释原因。

解：

```cpp
#include <iostream>
#include <map>
#include <string>

using std::string;
using std::map;
using std::cin;
using std::cout;

int main()
{
	map<string, size_t> counts;
	for (string word; cin >> word;)
	{
		auto result = counts.insert({ word, 1 });
		if (!result.second)
			++result.first->second;
	}
	for (auto const& count : counts)
		cout << count.first << " " << count.second << ((count.second > 1) ? " times\n" : " time\n");

	return 0;
}
```

使用`insert`更容易阅读和编写。`insert`有返回值，可以明确的体现出插入操作的结果。

## 练习11.21

> 假定`word_count`是一个`string`到`size_t`的`map`，`word`是一个`string`，解释下面循环的作用：

```cpp
while (cin >> word)
	++word_count.insert({word, 0}).first->second;
```

解：

这条语句等价于：

```cpp
while (cin >> word)
{
	auto result = word_count.insert({word, 0});
	++(result.first->second);
}
```

若`insert`成功：先添加一个元素，然后返回一个 `pair`，`pair` 的 `first`元素是一个迭代器。这个迭代器指向刚刚添加的元素，这个元素是`pair`，然后递增`pair`的`second`成员。
若`insert`失败：递增已有指定关键字的元素的 `second`成员。

## 练习11.22

> 给定一个`map<string, vector<int>>`，对此容器的插入一个元素的`insert`版本，写出其参数类型和返回类型。

```cpp
std::pair<std::string, std::vector<int>>    // 参数类型
std::pair<std::map<std::string, std::vector<int>>::iterator, bool> // 返回类型
```

## 练习11.23

> 11.2.1节练习中的`map` 以孩子的姓为关键字，保存他们的名的`vector`，用`multimap` 重写此`map`。

解：

```cpp
#include <map>
#include <string>
#include <iostream>

using std::string;
using std::multimap;
using std::cin;
using std::endl;

int main()
{
    multimap<string, string> families;
    for (string lname, cname; cin >> cname >> lname; families.emplace(lname, cname));
    for (auto const& family : families)
        std::cout << family.second << " " << family.first << endl;
}
```

## 练习11.24

> 下面的程序完成什么功能？
> 
```cpp
map<int, int> m;
m[0] = 1;
```

解：

添加一个元素到`map`中，如果该键存在，则重新赋值。

## 练习11.25

> 对比下面的程序与上一题程序

```cpp
vector<int> v;
v[0] = 1;
```

解：

未定义行为，`vector` 的下标越界访问。

## 练习11.26

> 可以用什么类型来对一个`map`进行下标操作？下标运算符返回的类型是什么？请给出一个具体例子——即，定义一个`map`，然后写出一个可以用来对`map`进行下标操作的类型以及下标运算符将会返会的类型。

解：

```cpp
std::map<int, std::string> m = { { 1,"ss" },{ 2,"sz" } };
using KeyType = std::map<int, std::string>::key_type;	
using ReturnType = std::map<int, std::string>::mapped_type;
```

## 练习11.27

> 对于什么问题你会使用`count`来解决？什么时候你又会选择`find`呢？

解：

对于允许重复关键字的容器，应该用 `count` ; 对于不允许重复关键字的容器，应该用 `find` 。

## 练习11.28

> 对一个`string`到`int`的`vector`的`map`，定义并初始化一个变量来保存在其上调用`find`所返回的结果。

解：

```cpp
map<string, vector<int>> m;
map<string, vector<int>>::iterator it = m.find("key");
```

## 练习11.29

> 如果给定的关键字不在容器中，`upper_bound`、`lower_bound` 和 `equal_range` 分别会返回什么？

解：

如果给定的关键字不在容器中，则 `lower_bound`和 `upper_bound` 会返回相等的迭代器，指向一个不影响排序的关键字插入位置。而`equal_range` 会返回一个 `pair`，`pair` 中的两个迭代器都指向关键字可以插入的位置。

## 练习11.30

> 对于本节最后一个程序中的输出表达式，解释运算对象`pos.first->second`的含义。

解：

`pos` 是一个`pair`，`pos.first` 是一个迭代器，指向匹配关键字的元素，该元素是一个 `pair`，访问该元素的第二个成员。

## 练习11.31

> 编写程序，定义一个作者及其作品的`multimap`。使用`find`在`multimap`中查找一个元素并用`erase`删除它。确保你的程序在元素不在`map` 中时也能正常运行。

解：

```cpp
#include <map>
#include <string>
#include <iostream>

using std::string;

int main()
{
	std::multimap<string, string> authors{
		{ "alan", "DMA" },
		{ "pezy", "LeetCode" },
		{ "alan", "CLRS" },
		{ "wang", "FTP" },
		{ "pezy", "CP5" },
		{ "wang", "CPP-Concurrency" } };

	string author = "pezy";
	string work = "CP5";

	auto found = authors.find(author);
	auto count = authors.count(author);
	while (count)
	{
		if (found->second == work)
		{
			authors.erase(found);
			break;
		}
		++found;
		--count;
	}

	for (const auto &author : authors)
		std::cout << author.first << " " << author.second << std::endl;

	return 0;
}
```

## 练习11.32

> 使用上一题定义的`multimap`编写一个程序，按字典序打印作者列表和他们的作品。

解：

```cpp
#include <map>
#include <set>
#include <string>
#include <iostream>

using std::string;

int main()
{
	std::multimap<string, string> authors{
		{ "alan", "DMA" },
		{ "pezy", "LeetCode" },
		{ "alan", "CLRS" },
		{ "wang", "FTP" },
		{ "pezy", "CP5" },
		{ "wang", "CPP-Concurrency" } };
	std::map<string, std::multiset<string>> order_authors;

	for (const auto &author : authors)
		order_authors[author.first].insert(author.second);

	for (const auto &author : order_authors)
	{
		std::cout << author.first << ": ";
		for (const auto &work : author.second)
			std::cout << work << " ";
		std::cout << std::endl;
	}

	return 0;
}
```

## 练习11.33

> 实现你自己版本的单词转换程序。

解：

```cpp
#include <map>
#include <string>
#include <fstream> 
#include <iostream>
#include <sstream>

using std::string; using std::ifstream;

std::map<string, string> buildMap(ifstream &map_file)
{
    std::map<string, string> trans_map;
    for (string key, value; map_file >> key && getline(map_file, value); )
        if (value.size() > 1) trans_map[key] = value.substr(1).substr(0, value.find_last_not_of(' '));
    return trans_map;
}

const string & transform(const string &s, const std::map<string, string> &m)
{
    auto map_it = m.find(s);
    return map_it == m.cend() ? s : map_it->second;
}

void word_transform(ifstream &map, ifstream &input)
{
    auto trans_map = buildMap(map);
    for (string text; getline(input, text); ) {
        std::istringstream iss(text);
        for (string word; iss >> word; )
            std::cout << transform(word, trans_map) << " ";
        std::cout << std::endl;
    }
}

int main()
{
    ifstream ifs_map("../data/word_transformation_bad.txt"), ifs_content("../data/given_to_transform.txt");
    if (ifs_map && ifs_content) word_transform(ifs_map, ifs_content);
    else std::cerr << "can't find the documents." << std::endl;
}
```

## 练习11.34

> 如果你将`transform` 函数中的`find`替换为下标运算符，会发生什么情况？

解：

如果使用下标运算符，当关键字未在容器中时，会往容器中添加一个新元素。

## 练习11.35

> 在`buildMap`中，如果进行如下改写，会有什么效果？

```cpp
trans_map[key] = value.substr(1);
//改为
trans_map.insert({key, value.substr(1)});
```

解：

当一个转换规则的关键字多次出现的时候，使用下标运算符会保留最后一次添加的规则，而用insert则保留第一次添加的规则。

## 练习11.36

> 我们的程序并没检查输入文件的合法性。特别是，它假定转换规则文件中的规则都是有意义的。如果文件中的某一行包含一个关键字、一个空格，然后就结束了，会发生什么？预测程序的行为并进行验证，再与你的程序进行比较。

解：

如果关键字没有对应的规则，那么程序会抛出一个 `runtime_error`。

## 练习11.37

> 一个无序容器与其有序版本相比有何优势？有序版本有何优势？

无序容器拥有更好的性能，有序容器使得元素始终有序。

## 练习11.38

> 用 `unordered_map` 重写单词计数程序和单词转换程序。

解：

```cpp
#include <unordered_map>
#include <set>
#include <string>
#include <iostream>
#include <fstream>
#include <sstream>

using std::string;

void wordCounting()
{
    std::unordered_map<string, size_t> word_count;
    for (string word; std::cin >> word; ++word_count[word]);
    for (const auto &w : word_count)
        std::cout << w.first << " occurs " << w.second << (w.second > 1 ? "times" : "time") << std::endl;
}

void wordTransformation()
{
    std::ifstream ifs_map("../data/word_transformation.txt"), ifs_content("../data/given_to_transform.txt");
    if (!ifs_map || !ifs_content) {
        std::cerr << "can't find the documents." << std::endl;
        return;
    }
    
    std::unordered_map<string, string> trans_map;
    for (string key, value; ifs_map >> key && getline(ifs_map, value); )
        if (value.size() > 1) trans_map[key] = value.substr(1).substr(0, value.find_last_not_of(' '));
    
    for (string text, word; getline(ifs_content, text); std::cout << std::endl)
        for (std::istringstream iss(text); iss >> word; ) {
            auto map_it = trans_map.find(word);
            std::cout << (map_it == trans_map.cend() ? word : map_it->second) << " ";
        }
}

int main()
{
    //wordCounting();
    wordTransformation();
}
```


# 第十二章 动态内存

## 练习12.1

> 在此代码的结尾，`b1` 和 `b2` 各包含多少个元素？

```cpp
StrBlob b1;
{
	StrBlob b2 = {"a", "an", "the"};
	b1 = b2;
	b2.push_back("about");
}
```

解：

它们实际操作的是同一个`vector`，都包含4个元素。在代码的结尾，`b2` 被析构了，不影响 `b1` 的元素。

## 练习12.2

> 编写你自己的`StrBlob` 类，包含`const` 版本的 `front` 和 `back`。

解：

头文件：

```cpp
#include <vector>
#include <string>
#include <initializer_list>
#include <memory>
#include <exception>

using std::vector; using std::string;

class StrBlob {
public:
    using size_type = vector<string>::size_type;

    StrBlob():data(std::make_shared<vector<string>>()) { }
    StrBlob(std::initializer_list<string> il):data(std::make_shared<vector<string>>(il)) { }

    size_type size() const { return data->size(); }
    bool empty() const { return data->empty(); }

    void push_back(const string &t) { data->push_back(t); }
    void pop_back() {
        check(0, "pop_back on empty StrBlob");
        data->pop_back();
    }

    std::string& front() {
        check(0, "front on empty StrBlob");
        return data->front();
    }

    std::string& back() {
        check(0, "back on empty StrBlob");
        return data->back();
    }

    const std::string& front() const {
        check(0, "front on empty StrBlob");
        return data->front();
    }
    const std::string& back() const {
        check(0, "back on empty StrBlob");
        return data->back();
    }

private:
    void check(size_type i, const string &msg) const {
        if (i >= data->size()) throw std::out_of_range(msg);
    }

private:
    std::shared_ptr<vector<string>> data;
};
```

主函数：

```cpp

#include "ex12_02.h"
#include <iostream>

int main()
{
    const StrBlob csb{ "hello", "world", "pezy" };
    StrBlob sb{ "hello", "world", "Mooophy" };

    std::cout << csb.front() << " " << csb.back() << std::endl;
    sb.back() = "pezy";
    std::cout << sb.front() << " " << sb.back() << std::endl;
}
```

## 练习12.3

> `StrBlob` 需要`const` 版本的`push_back` 和 `pop_back`吗？如需要，添加进去。否则，解释为什么不需要。

解：

不需要。`push_back` 和 `pop_back` 会改变对象的内容。而 `const` 对象是只读的，因此不需要。

## 练习12.4

> 在我们的 `check` 函数中，没有检查 `i` 是否大于0。为什么可以忽略这个检查？

解：

因为 `size_type` 是一个无符号整型，当传递给 `check` 的参数小于 0 的时候，参数值会转换成一个正整数。

## 练习12.5

> 我们未编写接受一个 `initializer_list explicit` 参数的构造函数。讨论这个设计策略的优点和缺点。

解：

构造函数不是 `explicit` 的，意味着可以从 `initializer_list` 隐式转换为 `StrBlob`。在 `StrBlob` 对象中，只有一个数据成员 `data`，而 `StrBlob` 对象本身的含义，也是一个**管理字符串的序列**。因此，从 `initializer_list` 到 `StrBlob` 的转换，在逻辑上是可行的。而这个设计策略的缺点，可能在某些地方我们确实需要 `initializer_list`，而编译器仍会将之转换为 `StrBlob`。

## 练习12.6

> 编写函数，返回一个动态分配的 `int` 的`vector`。将此`vector` 传递给另一个函数，这个函数读取标准输入，将读入的值保存在 `vector` 元素中。再将`vector`传递给另一个函数，打印读入的值。记得在恰当的时刻`delete vector`。

解：

```cpp
#include <iostream>
#include <vector>

using std::vector;

vector<int>* alloc_vector()
{
	return new vector<int>();
}

void assign_vector(vector<int>* p)
{
	int i;
	while (std::cin >> i)
	{
		p->push_back(i);
	}
}

void print_vector(vector<int>* p)
{
	for (auto i : *p)
	{
		std::cout << i << std::endl;
	}
}

int main()
{
	auto p = alloc_vector();
	assign_vector(p);
	print_vector(p);
	delete p;
	return 0;
}
```

## 练习12.7

> 重做上一题，这次使用 `shared_ptr` 而不是内置指针。

解：

```cpp
#include <iostream>
#include <vector>
#include <memory>

using std::vector;

std::shared_ptr<vector<int>> alloc_vector()
{
	return std::make_shared<vector<int>>();
}

void assign_vector(std::shared_ptr<vector<int>> p)
{
	int i;
	while (std::cin >> i)
	{
		p->push_back(i);
	}
}

void print_vector(std::shared_ptr<vector<int>> p)
{
	for (auto i : *p)
	{
		std::cout << i << std::endl;
	}
}

int main()
{
	auto p = alloc_vector();
	assign_vector(p);
	print_vector(p);
	return 0;
}
```

## 练习12.8

> 下面的函数是否有错误？如果有，解释错误原因。

```cpp
bool b() {
	int* p = new int;
	// ...
	return p;
}
```

解：

有错误。`p`会被强制转换成`bool`，继而没有释放指针 `p` 指向的对象。

## 练习12.9

> 解释下面代码执行的结果。

```cpp
int *q = new int(42), *r = new int(100);
r = q;
auto q2 = make_shared<int>(42), r2 = make_shared<int>(100);
r2 = q2;
```

解：

`r` 和 `q` 指向 42，而之前 `r` 指向的 100 的内存空间并没有被释放，因此会发生内存泄漏。`r2` 和 `q2` 都是智能指针，当对象空间不被引用的时候会自动释放。

## 练习12.10

> 下面的代码调用了第413页中定义的`process` 函数，解释此调用是否正确。如果不正确，应如何修改？

```cpp
shared_ptr<int> p(new int(42));
process(shared_ptr<int>(p));
```

解：

正确。`shared_ptr<int>(p)` 会创建一个临时的智能指针，这个智能指针与 `p` 引用同一个对象，此时引用计数为 2。当表达式结束时，临时的智能指针被销毁，此时引用计数为 1。

## 练习12.11

> 如果我们像下面这样调用 `process`，会发生什么？

```cpp
process(shared_ptr<int>(p.get()));
```

解：

这样会创建一个新的智能指针，它的引用计数为 1，这个智能指针所指向的空间与 `p` 相同。在表达式结束后，这个临时智能指针会被销毁，引用计数为 0，所指向的内存空间也会被释放。而导致 `p` 所指向的空间被释放，使得 p` 成为一个空悬指针。

## 练习12.12

> `p` 和 `sp` 的定义如下，对于接下来的对 `process` 的每个调用，如果合法，解释它做了什么，如果不合法，解释错误原因：

```cpp
auto p = new int();
auto sp = make_shared<int>();
(a) process(sp);
(b) process(new int());
(c) process(p);
(d) process(shared_ptr<int>(p));
```

解：

* (a) 合法。将`sp` 拷贝给 `process`函数的形参，在函数里面引用计数为 2，函数结束后引用计数为 1。
* (b) 不合法。不能从内置指针隐式转换为智能指针。
* (c) 不合法。不能从内置指针隐式转换为智能指针。
* (d) 合法。但是智能指针和内置指针一起使用可能会出现问题，在表达式结束后智能指针会被销毁，它所指向的对象也被释放。而此时内置指针 `p` 依旧指向该内存空间。之后对内置指针 `p` 的操作可能会引发错误。

## 练习12.13

> 如果执行下面的代码，会发生什么？

```cpp
auto sp = make_shared<int>();
auto p = sp.get();
delete p;
```

解：

智能指针 `sp` 所指向空间已经被释放，再对 `sp` 进行操作会出现错误。

## 练习12.14

> 编写你自己版本的用 `shared_ptr` 管理 `connection` 的函数。

解：

```cpp
#include <iostream>
#include <memory>
#include <string>

struct connection
{
	std::string ip;
	int port;
	connection(std::string i, int p) : ip(i), port(p) {}
};

struct destination
{
	std::string ip;
	int port;
	destination(std::string i, int p) : ip(i), port(p) {}
};

connection connect(destination* pDest)
{
	std::shared_ptr<connection> pConn(new connection(pDest->ip, pDest->port));
	std::cout << "creating connection(" << pConn.use_count() << ")" << std::endl;
	return *pConn;
}

void disconnect(connection pConn)
{
	std::cout << "connection close(" << pConn.ip << ":" << pConn.port << ")" << std::endl;	
}

void end_connection(connection* pConn)
{
	disconnect(*pConn);
}

void f(destination &d)
{
	connection conn = connect(&d);
	std::shared_ptr<connection> p(&conn, end_connection);
	std::cout << "connecting now(" << p.use_count() << ")" << std::endl;
}

int main()
{
	destination dest("220.181.111.111", 10086);
	f(dest);

	return 0;
}
```

## 练习12.15

> 重写上一题的程序，用 `lambda` 代替`end_connection` 函数。

解：

```cpp
#include <iostream>
#include <memory>
#include <string>

struct connection
{
	std::string ip;
	int port;
	connection(std::string i, int p) : ip(i), port(p) {}
};

struct destination
{
	std::string ip;
	int port;
	destination(std::string i, int p) : ip(i), port(p) {}
};

connection connect(destination* pDest)
{
	std::shared_ptr<connection> pConn(new connection(pDest->ip, pDest->port));
	std::cout << "creating connection(" << pConn.use_count() << ")" << std::endl;
	return *pConn;
}

void disconnect(connection pConn)
{
	std::cout << "connection close(" << pConn.ip << ":" << pConn.port << ")" << std::endl;
}

void f(destination &d)
{
	connection conn = connect(&d);
	std::shared_ptr<connection> p(&conn, [] (connection* p){ disconnect(*p); });
	std::cout << "connecting now(" << p.use_count() << ")" << std::endl;
}

int main()
{
	destination dest("220.181.111.111", 10086);
	f(dest);

	return 0;
}
```

## 练习12.16

> 如果你试图拷贝或赋值 `unique_ptr`，编译器并不总是能给出易于理解的错误信息。编写包含这种错误的程序，观察编译器如何诊断这种错误。

解：

```cpp
#include <iostream>
#include <string>
#include <memory>

using std::string; using std::unique_ptr;

int main()
{
    unique_ptr<string> p1(new string("pezy"));
    // unique_ptr<string> p2(p1); // copy
    //                      ^
    // Error: Call to implicitly-deleted copy constructor of 'unique_ptr<string>'
    //
    // unique_ptr<string> p3 = p1; // assign
    //                      ^
    // Error: Call to implicitly-deleted copy constructor of 'unique_ptr<string>'
    std::cout << *p1 << std::endl;
    p1.reset(nullptr);
}
```

## 练习12.17

> 下面的 `unique_ptr` 声明中，哪些是合法的，哪些可能导致后续的程序错误？解释每个错误的问题在哪里。

```cpp
int ix = 1024, *pi = &ix, *pi2 = new int(2048);
typedef unique_ptr<int> IntP;
(a) IntP p0(ix);
(b) IntP p1(pi);
(c) IntP p2(pi2);
(d) IntP p3(&ix);
(e) IntP p4(new int(2048));
(f) IntP p5(p2.get());
```

解：

* (a) 不合法。在定义一个 `unique_ptr` 时，需要将其绑定到一个`new` 返回的指针上。
* (b) 不合法。理由同上。
* (c) 合法。但是也可能会使得 `pi2` 成为空悬指针。
* (d) 不合法。当 `p3` 被销毁时，它试图释放一个栈空间的对象。
* (e) 合法。
* (f) 不合法。`p5` 和 `p2` 指向同一个对象，当 `p5` 和 `p2` 被销毁时，会使得同一个指针被释放两次。

## 练习12.18

> `shared_ptr` 为什么没有 `release` 成员？

`release` 成员的作用是放弃控制权并返回指针，因为在某一时刻只能有一个 `unique_ptr` 指向某个对象，`unique_ptr` 不能被赋值，所以要使用 `release` 成员将一个 `unique_ptr` 的指针的所有权传递给另一个 `unique_ptr`。而 `shared_ptr` 允许有多个 `shared_ptr` 指向同一个对象，因此不需要 `release` 成员。

## 练习12.19

> 定义你自己版本的 `StrBlobPtr`，更新 `StrBlob` 类，加入恰当的 `friend` 声明以及 `begin` 和 `end` 成员。

解：

```cpp
#include <string>
#include <vector>
#include <initializer_list>
#include <memory>
#include <stdexcept>

using std::vector; using std::string;

class StrBlobPtr;

class StrBlob
{
public:
	using size_type = vector<string>::size_type;
	friend class StrBlobPtr;

	StrBlobPtr begin();
	StrBlobPtr end();

	StrBlob() : data(std::make_shared<vector<string>>()) {}
	StrBlob(std::initializer_list<string> il) : data(std::make_shared<vector<string>>(il)) {}

	size_type size() const { return data->size(); }
	bool empty() const { return data->empty(); }

	void push_back(const string& s) { data->push_back(s); }
	void pop_back()
	{
		check(0, "pop_back on empty StrBlob");
		data->pop_back();
	}

	std::string& front()
	{
		check(0, "front on empty StrBlob");
		return data->front();
	}

	std::string& back()
	{
		check(0, "back on empty StrBlob");
		return data->back();
	}

	const std::string& front() const
	{
		check(0, "front on empty StrBlob");
		return data->front();
	}
	const std::string& back() const
	{
		check(0, "back on empty StrBlob");
		return data->back();
	}

private:
	void check(size_type i, const string& msg) const
	{
		if (i >= data->size())
			throw std::out_of_range(msg);
	}

private:
	std::shared_ptr<vector<string>> data;
};

class StrBlobPtr
{
public:
	StrBlobPtr() :curr(0) {}
	StrBlobPtr(StrBlob &a, size_t sz = 0) :wptr(a.data), curr(sz) {}
	bool operator!=(const StrBlobPtr& p) { return p.curr != curr; }
	string& deref() const
	{
		auto p = check(curr, "dereference past end");
		return (*p)[curr];
	}
	StrBlobPtr& incr()
	{
		check(curr, "increment past end of StrBlobPtr");
		++curr;
		return *this;
	}

private:
	std::shared_ptr<vector<string>> check(size_t i, const string &msg) const
	{
		auto ret = wptr.lock();
		if (!ret) throw std::runtime_error("unbound StrBlobPtr");
		if (i >= ret->size()) throw std::out_of_range(msg);
		return ret;
	}
	std::weak_ptr<vector<string>> wptr;
	size_t curr;
};

StrBlobPtr StrBlob::begin()
{
	return StrBlobPtr(*this);
}
StrBlobPtr StrBlob::end()
{
	return StrBlobPtr(*this, data->size());
}
```

## 练习12.20

> 编写程序，逐行读入一个输入文件，将内容存入一个 `StrBlob` 中，用一个 `StrBlobPtr` 打印出 `StrBlob` 中的每个元素。

解：

```cpp
#include <iostream>
#include <fstream>
#include "exercise12_19.h"

using namespace std;

int main()
{
	ifstream ifs("books.txt");
	StrBlob sb;
	string s;
	while (getline(ifs, s))
	{
		sb.push_back(s);
	}
	for (StrBlobPtr sbp = sb.begin(); sbp != sb.end(); sbp.incr())
	{
		cout << sbp.deref() << endl;
	}

	return 0;
}
```

## 练习12.21

> 也可以这样编写 `StrBlobPtr` 的 `deref` 成员：
```cpp
std::string& deref() const {
	return (*check(curr, "dereference past end"))[curr];
}
```
你认为哪个版本更好？为什么？

解：

原来的版本更好，可读性更高。

## 练习12.22

> 为了能让 `StrBlobPtr` 使用 `const StrBlob`，你觉得应该如何修改？定义一个名为`ConstStrBlobPtr` 的类，使其能够指向 `const StrBlob`。

解：

构造函数改为接受 `const Strblob &` , 然后给 `Strblob` 类添加两个 `const` 成员函数 `cbegin` 和 `cend`，返回 `ConstStrBlobPtr`。

## 练习12.23

> 编写一个程序，连接两个字符串字面常量，将结果保存在一个动态分配的`char`数组中。重写这个程序，连接两个标准库`string`对象。

解:

```cpp
#include <iostream>
#include <string>
#include <cstring>
#include <memory>

int main() {
	const char *c1 = "Hello ";
	const char *c2 = "World";
	unsigned len = strlen(c1) + strlen(c2) + 1;
	char *r = new char[len]();
	strcat_s(r, len, c1);
	strcat_s(r, len, c2);
	std::cout << r << std::endl;

	std::string s1 = "Hello ";
	std::string s2 = "World";
	strcpy_s(r, len, (s1 + s2).c_str());
	std::cout << r << std::endl;

	delete[] r;

	return 0;
}
```

## 练习12.24

> 编写一个程序，从标准输入读取一个字符串，存入一个动态分配的字符数组中。描述你的程序如何处理变长输入。测试你的程序，输入一个超出你分配的数组长度的字符串。

解：

```cpp
#include <iostream>

int main()
{
	std::cout << "How long do you want the string? ";
	int size{ 0 };
	std::cin >> size;
	char *input = new char[size + 1]();
	std::cin.ignore();
	std::cout << "input the string: ";
	std::cin.get(input, size + 1);
	std::cout << input;
	delete[] input;

	return 0;
}
```

## 练习12.25

> 给定下面的`new`表达式，你应该如何释放`pa`？
```cpp
int *pa = new int[10];
```

解：

```cpp
delete [] pa;
```

## 练习12.26

> 用 `allocator` 重写第427页中的程序。

```cpp
#include <iostream>
#include <string>
#include <memory>

using namespace std;

int main()
{
	int n = 5;
	allocator<string> alloc;
	auto p = alloc.allocate(n);
	string s;
	auto q = p;
	while (cin >> s && q != p + n)
	{
		alloc.construct(q++, s);
	}
	while (q != p)
	{
		std::cout << *--q << " ";
		alloc.destroy(q);
	}
	alloc.deallocate(p, n);

	return 0;
}
```

## 练习12.27

> `TextQuery` 和 `QueryResult` 类只使用了我们已经介绍过的语言和标准库特性。不要提前看后续章节内容，只用已经学到的知识对这两个类编写你自己的版本。

解：

头文件：

```cpp
#ifndef EX12_27_H
#define EX12_27_H

#include <fstream>
#include <memory>
#include <vector>
#include <string>
#include <map>
#include <set>

class QueryResult;

class TextQuery
{
public:
	using line_no = std::vector<std::string>::size_type;
	TextQuery(std::ifstream&);
	QueryResult query(const std::string& s) const;

private:
	std::shared_ptr<std::vector<std::string>> file;
	std::map<std::string, std::shared_ptr<std::set<line_no>>> wm;
};

class QueryResult
{
public:
	friend std::ostream& print(std::ostream&, const QueryResult&);
	QueryResult(std::string s,
				std::shared_ptr<std::set<TextQuery::line_no>> p,
				std::shared_ptr<std::vector<std::string>> f) :
		sought(s), lines(p), file(f) 
	{}

private:
	std::string sought;
	std::shared_ptr<std::set<TextQuery::line_no>> lines;
	std::shared_ptr<std::vector<std::string>> file;
};

std::ostream& print(std::ostream&, const QueryResult&);

#endif
```

实现：

```cpp
#include "ex_12_27.h"
#include <sstream>
#include <fstream>
#include <vector>
#include <string>

using namespace std;

TextQuery::TextQuery(ifstream& ifs) : file(new vector<string>)
{
	string text;
	while (getline(ifs, text))
	{
		file->push_back(text);
		int n = file->size() - 1;
		istringstream line(text);
		string word;
		while (line >> word)
		{
			auto &lines = wm[word];
			if (!lines)
				lines.reset(new set<line_no>);
			lines->insert(n);
		}
	}
}

QueryResult TextQuery::query(const string& s) const
{
	static shared_ptr<set<line_no>> nodata(new set<line_no>);
	auto loc = wm.find(s);
	if (loc == wm.end())
		return QueryResult(s, nodata, file);
	else
		return QueryResult(s, loc->second, file);
}

std::ostream& print(std::ostream& os, const QueryResult& qr)
{
	os << qr.sought << " occurs " << qr.lines->size() << " "
		<< "time" << (qr.lines->size() > 1 ? "s" : "") << endl;
	for (auto num : *qr.lines)
		os << "\t(line " << num + 1 << ") " << *(qr.file->begin() + num) << endl;
	return os;
}
```

主函数：

```cpp
#include <iostream>
#include <string>
#include <fstream>
#include "ex_12_27.h"

using namespace std;

void runQueries(ifstream& infile)
{
	TextQuery tq(infile);
	while (true)
	{
		cout << "enter word to look for, or q to quit: ";
		string s;
		if (!(cin >> s) || s == "q") break;
		print(cout, tq.query(s)) << endl;
	}
}

int main()
{
	ifstream ifs("storyDataFile.txt");
	runQueries(ifs);
	return 0;
}
```

## 练习12.28

> 编写程序实现文本查询，不要定义类来管理数据。你的程序应该接受一个文件，并与用户交互来查询单词。使用`vector`、`map` 和 `set` 容器来保存来自文件的数据并生成查询结果。

解：

```cpp
#include <string>
using std::string;

#include <vector>
using std::vector;

#include <memory>
using std::shared_ptr;

#include <iostream>
#include <fstream>
#include <sstream>
#include <map>
#include <set>
#include <algorithm>

int main()
{
	std::ifstream file("H:/code/C++/Cpp_Primer_Answers/data/storyDataFile.txt");
	vector<string> input;
	std::map<string, std::set<decltype(input.size())>> dictionary;
	decltype(input.size()) lineNo{ 0 };

	for (string line; std::getline(file, line); ++lineNo)
	{
		input.push_back(line);
		std::istringstream line_stream(line);
		for (string text, word; line_stream >> text; word.clear())
		{
			std::remove_copy_if(text.begin(), text.end(), std::back_inserter(word), ispunct);
			dictionary[word].insert(lineNo);
		}
	}

	while (true)
	{
		std::cout << "enter word to look for, or q to quit: ";
		string s;
		if (!(std::cin >> s) || s == "q") break;
		auto found = dictionary.find(s);
		if (found != dictionary.end())
		{
			std::cout << s << " occurs " << found->second.size() << (found->second.size() > 1 ? " times" : " time") << std::endl;
			for (auto i : found->second)
				std::cout << "\t(line " << i + 1 << ") " << input.at(i) << std::endl;
		}
		else std::cout << s << " occurs 0 time" << std::endl;
	}
}
```

## 练习12.29

> 我们曾经用`do while` 循环来编写管理用户交互的循环。用`do while` 重写本节程序，解释你倾向于哪个版本，为什么？

解：

```cpp
do {
    std::cout << "enter word to look for, or q to quit: ";
    string s;
    if (!(std::cin >> s) || s == "q") break;
    print(std::cout, tq.query(s)) << std::endl;
} while ( true );
```
我更喜欢 `while`，这可能是习惯的问题。

## 练习12.30

> 定义你自己版本的 `TextQuery` 和 `QueryResult` 类，并执行12.3.1节中的`runQueries` 函数。

解：

同12.27。

## 练习12.31

> 如果用`vector` 代替 `set` 保存行号，会有什么差别？哪个方法更好？为什么？

如果用 `vector` 则会有单词重复的情况出现。而这里保存的是行号，不需要重复元素，所以 `set` 更好。

## 练习12.32

> 重写 `TextQuery` 和 `QueryResult`类，用`StrBlob` 代替 `vector<string>` 保存输入文件。

解：

`TextQuery` 和 `QueryResult` 类中的 `file` 成员，改为 指向 `StrBlob` 的智能指针。在访问 `StrBlob` 时，要使用 `StrBlobPtr`。

## 练习12.33

> 在第15章中我们将扩展查询系统，在 `QueryResult` 类中将会需要一些额外的成员。添加名为 `begin` 和 `end` 的成员，返回一个迭代器，指向一个给定查询返回的行号的 `set` 中的位置。再添加一个名为 `get_file` 的成员，返回一个 `shared_ptr`，指向 `QueryResult` 对象中的文件。

解：

```cpp
class QueryResult{
public:
	using Iter = std::set<line_no>::iterator;	
	// ...
	Iter begin() const { return lines->begin(); }
	Iter end() const { return lines->end(); }
	shared_ptr<std::vector<std::string>> get_file() const 
	{ 
		return std::make_shared<std::vector<std::string>>(file); 
	}
private:
	// ...
};
```


# 第十三章 拷贝控制

## 练习13.1

> 拷贝构造函数是什么？什么时候使用它？

解：

如果一个构造函数的第一个参数是自身类类型的引用，且任何额外参数都有默认值，则此构造函数是拷贝构造函数。当使用**拷贝初始化**时，我们会用到拷贝构造函数。

## 练习13.2

> 解释为什么下面的声明是非法的：
```cpp
Sales_data::Sales_data(Sales_data rhs);
```

解：

参数类型应该是引用类型。

## 练习13.3

> 当我们拷贝一个`StrBlob`时，会发生什么？拷贝一个`StrBlobPtr`呢？

解：

当我们拷贝`StrBlob`时，会使 `shared_ptr` 的引用计数加1。当我们拷贝 `StrBlobPtr` 时，引用计数不会变化。

## 练习13.4

> 假定 `Point` 是一个类类型，它有一个`public`的拷贝构造函数，指出下面程序片段中哪些地方使用了拷贝构造函数：


```cpp
Point global;
Point foo_bar(Point arg) // 1
{
	Point local = arg, *heap = new Point(global); // 2: Point local = arg,  3: Point *heap = new Point(global) 
	*heap = local; 
	Point pa[4] = { local, *heap }; // 4, 5
	return *heap;  // 6
}
```

解：

上面有6处地方使用了拷贝构造函数。


## 练习13.5

> 给定下面的类框架，编写一个拷贝构造函数，拷贝所有成员。你的构造函数应该动态分配一个新的`string`，并将对象拷贝到`ps`所指向的位置，而不是拷贝ps本身：

```cpp
class HasPtr {
public:
	HasPtr(const std::string& s = std::string()):
		ps(new std::string(s)), i(0) { }
private:
	std::string *ps;
	int i;
}
```

解：

```cpp
#include <string>

class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr& hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
private:
    std::string *ps;
    int i;
};
```

## 练习13.6

> 拷贝赋值运算符是什么？什么时候使用它？合成拷贝赋值运算符完成什么工作？什么时候会生成合成拷贝赋值运算符？

解：

拷贝赋值运算符是一个名为 `operator=` 的函数。当赋值运算发生时就会用到它。合成拷贝赋值运算符可以用来禁止该类型对象的赋值。如果一个类未定义自己的拷贝赋值运算符，编译器会为它生成一个合成拷贝赋值运算符。

## 练习13.7

> 当我们将一个 `StrBlob` 赋值给另一个 `StrBlob` 时，会发生什么？赋值 `StrBlobPtr` 呢？

解：

会发生浅层复制。

## 练习13.8

> 为13.1.1节练习13.5中的 `HasPtr` 类编写赋值运算符。类似拷贝构造函数，你的赋值运算符应该将对象拷贝到`ps`指向的位置。

解：

```cpp
#include <string>

class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
    HasPtr& operator=(const HasPtr &rhs_hp) {
        if(this != &rhs_hp){
            std::string *temp_ps = new std::string(*rhs_hp.ps);
            delete ps;
            ps = temp_ps;
            i = rhs_hp.i;
        }
        return *this;
    }
private:
    std::string *ps;
    int i;
};
```

## 练习13.9

> 析构函数是什么？合成析构函数完成什么工作？什么时候会生成合成析构函数？

解：

析构函数是类的一个成员函数，名字由波浪号接类名构成。它没有返回值，也不接受参数。合成析构函数可被用来阻止该类型的对象被销毁。当一个类未定义自己的析构函数时，编译器会为它生成一个合成析构函数。

## 练习13.10

> 当一个 `StrBlob` 对象销毁时会发生什么？一个 `StrBlobPtr` 对象销毁时呢？

解：

当一个 `StrBlob` 对象被销毁时，`shared_ptr` 的引用计数会减少。当 `StrBlobPtr` 对象被销毁时，不影响引用计数。

## 练习13.11

> 为前面练习中的 `HasPtr` 类添加一个析构函数。

解：

```cpp
#include <string>

class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
    HasPtr& operator=(const HasPtr &hp) {
        std::string *new_ps = new std::string(*hp.ps);
        delete ps;
        ps = new_ps;
        i = hp.i;
        return *this;
    }
    ~HasPtr() {
        delete ps;
    }
private:
    std::string *ps;
    int i;
};
```

## 练习13.12

> 在下面的代码片段中会发生几次析构函数调用？
```cpp
bool fcn(const Sales_data *trans, Sales_data accum)
{
	Sales_data item1(*trans), item2(accum);
	return item1.isbn() != item2.isbn();
}
```

解：

三次，分别是 `accum`、`item1`和`item2`。

## 练习13.13

> 理解拷贝控制成员和构造函数的一个好方法的定义一个简单的类，为该类定义这些成员，每个成员都打印出自己的名字：

```cpp
struct X {
	X() {std::cout << "X()" << std::endl;}
	X(const X&) {std::cout << "X(const X&)" << std::endl;}
}
```

给 `X` 添加拷贝赋值运算符和析构函数，并编写一个程序以不同的方式使用 `X` 的对象：将它们作为非引用参数传递；动态分配它们；将它们存放于容器中；诸如此类。观察程序的输出，直到你确认理解了什么时候会使用拷贝控制成员，以及为什么会使用它们。当你观察程序输出时，记住编译器可以略过对拷贝构造函数的调用。

解：

```cpp
#include <iostream>
#include <vector>
#include <initializer_list>

struct X {
    X() { std::cout << "X()" << std::endl; }
    X(const X&) { std::cout << "X(const X&)" << std::endl; }
    X& operator=(const X&) { std::cout << "X& operator=(const X&)" << std::endl; return *this; }
    ~X() { std::cout << "~X()" << std::endl; }
};

void f(const X &rx, X x)
{
    std::vector<X> vec;
    vec.reserve(2);
    vec.push_back(rx);
    vec.push_back(x);
}

int main()
{
    X *px = new X;
    f(*px, *px);
    delete px;

    return 0;
}
```

## 练习13.14

> 假定 `numbered` 是一个类，它有一个默认构造函数，能为每个对象生成一个唯一的序号，保存在名为 `mysn` 的数据成员中。假定 `numbered` 使用合成的拷贝控制成员，并给定如下函数：

```cpp
void f (numbered s) { cout << s.mysn < endl; }
```

则下面代码输出什么内容？

```cpp
numbered a, b = a, c = b;
f(a); f(b); f(c);
```

解：

输出3个完全一样的数。

## 练习13.15

> 假定`numbered` 定义了一个拷贝构造函数，能生成一个新的序列号。这会改变上一题中调用的输出结果吗？如果会改变，为什么？新的输出结果是什么？

解：

会输出3个不同的数。并且这3个数并不是a、b、c当中的数。

## 练习13.16

> 如果 `f` 中的参数是 `const numbered&`，将会怎样？这会改变输出结果吗？如果会改变，为什么？新的输出结果是什么？

解：

会输出 a、b、c的数。

## 练习13.17

> 分别编写前三题中所描述的 `numbered` 和 `f`，验证你是否正确预测了输出结果。

解：

13.14：

```cpp

#include <iostream>

class numbered
{
public:
	numbered()
	{
		mysn = unique++;
	}

	int mysn;
	static int unique;
};

int numbered::unique = 10;

void f(numbered s)
{
	std::cout << s.mysn << std::endl;
}

int main()
{
	numbered a, b = a, c = b;
	f(a);
	f(b);
	f(c);
}
```

13.15：

```cpp
#include <iostream>

class numbered {
public:
    numbered() {
        mysn = unique++;
    }
	numbered(const numbered& n)
	{
		mysn = unique++;
	}

    int mysn;
    static int unique;
};

int numbered::unique = 10;

void f(numbered s) {
    std::cout << s.mysn << std::endl;
}

int main()
{
    numbered a, b = a, c = b;
    f(a);
    f(b);
    f(c);
}
```

13.16：

```cpp
#include <iostream>

class numbered
{
public:
	numbered()
	{
		mysn = unique++;
	}
	numbered(const numbered& n)
	{
		mysn = unique++;
	}

	int mysn;
	static int unique;
};

int numbered::unique = 10;

void f(const numbered& s)
{
	std::cout << s.mysn << std::endl;
}

int main()
{
	numbered a, b = a, c = b;
	f(a);
	f(b);
	f(c);
}
```

## 练习13.18

> 定义一个 `Employee` 类，它包含雇员的姓名和唯一的雇员证号。为这个类定义默认构造函数，以及接受一个表示雇员姓名的 `string` 的构造函数。每个构造函数应该通过递增一个 `static` 数据成员来生成一个唯一的证号。

解：

```cpp
#include <string>
using std::string;

class Employee
{
public:
	Employee();
	Employee(const string& name);

	const int id() const { return id_; }

private:
	string name_;
	int id_;
	static int s_increment;
};

int Employee::s_increment = 0;
Employee::Employee()
{
	id_ = s_increment++;
}

Employee::Employee(const string& name)
{
	id_ = s_increment++;
	name_ = name;
}

```

## 练习13.19

> 你的 `Employee` 类需要定义它自己的拷贝控制成员吗？如果需要，为什么？如果不需要，为什么？实现你认为 `Employee` 需要的拷贝控制成员。

解：

可以显式地阻止拷贝。

```cpp
#include <string>
using std::string;

class Employee {
public:
    Employee();
    Employee(const string &name);
    Employee(const Employee&) = delete;
    Employee& operator=(const Employee&) = delete;

    const int id() const { return id_; }

private:
    string name_;
    int id_;
    static int s_increment;
};
```

## 练习13.20

> 解释当我们拷贝、赋值或销毁 `TextQuery` 和 `QueryResult` 类对象时会发生什么？

解：

成员会被复制。

## 练习13.21

> 你认为 `TextQuery` 和 `QueryResult` 类需要定义它们自己版本的拷贝控制成员吗？如果需要，为什么？实现你认为这两个类需要的拷贝控制操作。

解：

合成的版本满足所有的需求。因此不需要自定义拷贝控制成员。

## 练习13.22

> 假定我们希望 `HasPtr` 的行为像一个值。即，对于对象所指向的 `string` 成员，每个对象都有一份自己的拷贝。我们将在下一节介绍拷贝控制成员的定义。但是，你已经学习了定义这些成员所需的所有知识。在继续学习下一节之前，为 `HasPtr` 编写拷贝构造函数和拷贝赋值运算符。

解：

```cpp
#include <string>

class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
    HasPtr& operator=(const HasPtr &hp) {
        auto new_p = new std::string(*hp.ps);
        delete ps;
        ps = new_p;
        i = hp.i;
        return *this;
    }
    ~HasPtr() {
        delete ps;
    } 
private:
    std::string *ps;
    int i;
};
```

## 练习13.23

> 比较上一节练习中你编写的拷贝控制成员和这一节中的代码。确定你理解了你的代码和我们的代码之间的差异。

解：

查看13.22代码。

## 练习13.24

> 如果本节的 `HasPtr` 版本未定义析构函数，将会发生什么？如果未定义拷贝构造函数，将会发生什么？

解：

如果未定义析构函数，将会发生内存泄漏。如果未定义拷贝构造函数，将会拷贝指针的值，指向同一个地址。

## 练习13.25

> 假定希望定义 `StrBlob` 的类值版本，而且希望继续使用 `shared_ptr`，这样我们的 `StrBlobPtr` 类就仍能使用指向`vector`的 `weak_ptr` 了。你修改后的类将需要一个拷贝的构造函数和一个拷贝赋值运算符，但不需要析构函数。解释拷贝构造函数和拷贝赋值运算符必须要做什么。解释为什么不需要析构函数。

解：

拷贝构造函数和拷贝赋值运算符要重新动态分配内存。因为 `StrBlob` 使用的是智能指针，当引用计数为0时会自动释放对象，因此不需要析构函数。

## 练习13.26

> 对上一题中描述的 `strBlob` 类，编写你自己的版本。

解：

头文件：

```cpp
#include <vector>
#include <string>
#include <initializer_list>
#include <memory>
#include <exception>

using std::vector; using std::string;

class ConstStrBlobPtr;

class StrBlob {
public:
    using size_type = vector<string>::size_type;
    friend class ConstStrBlobPtr;

    ConstStrBlobPtr begin() const;
    ConstStrBlobPtr end() const;

    StrBlob():data(std::make_shared<vector<string>>()) { }
    StrBlob(std::initializer_list<string> il):data(std::make_shared<vector<string>>(il)) { }

    // copy constructor
    StrBlob(const StrBlob& sb) : data(std::make_shared<vector<string>>(*sb.data)) { }
    // copy-assignment operators
    StrBlob& operator=(const StrBlob& sb);

    size_type size() const { return data->size(); }
    bool empty() const { return data->empty(); }

    void push_back(const string &t) { data->push_back(t); }
    void pop_back() {
        check(0, "pop_back on empty StrBlob");
        data->pop_back();
    }

    string& front() {
        check(0, "front on empty StrBlob");
        return data->front();
    }

    string& back() {
        check(0, "back on empty StrBlob");
        return data->back();
    }

    const string& front() const {
        check(0, "front on empty StrBlob");
        return data->front();
    }
    const string& back() const {
        check(0, "back on empty StrBlob");
        return data->back();
    }

private:
    void check(size_type i, const string &msg) const {
        if (i >= data->size()) throw std::out_of_range(msg);
    }

private:
    std::shared_ptr<vector<string>> data;
};

class ConstStrBlobPtr {
public:
    ConstStrBlobPtr():curr(0) { }
    ConstStrBlobPtr(const StrBlob &a, size_t sz = 0):wptr(a.data), curr(sz) { } // should add const
    bool operator!=(ConstStrBlobPtr& p) { return p.curr != curr; }
    const string& deref() const { // return value should add const
        auto p = check(curr, "dereference past end");
        return (*p)[curr];
    }
    ConstStrBlobPtr& incr() {
        check(curr, "increment past end of StrBlobPtr");
        ++curr;
        return *this;
    }

private:
    std::shared_ptr<vector<string>> check(size_t i, const string &msg) const {
        auto ret = wptr.lock();
        if (!ret) throw std::runtime_error("unbound StrBlobPtr");
        if (i >= ret->size()) throw std::out_of_range(msg);
        return ret;
    }
    std::weak_ptr<vector<string>> wptr;
    size_t curr;
};
```

主函数：

```cpp
#include "ex_13_26.h"

ConstStrBlobPtr StrBlob::begin() const // should add const
{
    return ConstStrBlobPtr(*this);
}
ConstStrBlobPtr StrBlob::end() const // should add const
{
    return ConstStrBlobPtr(*this, data->size());
}

StrBlob& StrBlob::operator=(const StrBlob& sb)
{
    data = std::make_shared<vector<string>>(*sb.data);
    return *this;
}

int main()
{
    return 0;
}
```

## 练习13.27

> 定义你自己的使用引用计数版本的 `HasPtr`。

解：

```cpp
#include <string>

class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0), use(new size_t(1)) { }
    HasPtr(const HasPtr &hp) : ps(hp.ps), i(hp.i), use(hp.use) { ++*use; }
    HasPtr& operator=(const HasPtr &rhs) {
        ++*rhs.use;
        if (--*use == 0) {
            delete ps;
            delete use;
        }
        ps = rhs.ps;
        i = rhs.i;
        use = rhs.use;
        return *this;
    }
    ~HasPtr() {
        if (--*use == 0) {
            delete ps;
            delete use;
        }
    } 
private:
    std::string *ps;
    int i;
    size_t *use;
};
```

## 练习13.28

> 给定下面的类，为其实现一个默认构造函数和必要的拷贝控制成员。

```cpp
(a) 
class TreeNode {
pravite:
	std::string value;
	int count;
	TreeNode *left;
	TreeNode *right;	
};
(b)
class BinStrTree{
pravite:
	TreeNode *root;	
};
```

解：

头文件：

```cpp
#include <string>
using std::string;

class TreeNode {
public:
    TreeNode() : value(string()), count(new int(1)), left(nullptr), right(nullptr) { }
    TreeNode(const TreeNode &rhs) : value(rhs.value), count(rhs.count), left(rhs.left), right(rhs.right) { ++*count; }
    TreeNode& operator=(const TreeNode &rhs);
    ~TreeNode() {
        if (--*count == 0) {
            delete left;
            delete right;
            delete count;
        }
    }

private:
    std::string value;
    int         *count;
    TreeNode    *left;
    TreeNode    *right;
};

class BinStrTree {
public:
    BinStrTree() : root(new TreeNode()) { }
    BinStrTree(const BinStrTree &bst) : root(new TreeNode(*bst.root)) { }
    BinStrTree& operator=(const BinStrTree &bst);
    ~BinStrTree() { delete root; }

private:
    TreeNode *root;
};
```

实现和主函数：

```cpp
#include "ex_13_28.h"

TreeNode& TreeNode::operator=(const TreeNode &rhs)
{
    ++*rhs.count;
    if (--*count == 0) {
        delete left;
        delete right;
        delete count;
    }
    value = rhs.value;
    left = rhs.left;
    right = rhs.right;
    count = rhs.count;
    return *this;
}

BinStrTree& BinStrTree::operator=(const BinStrTree &bst)
{
    TreeNode *new_root = new TreeNode(*bst.root);
    delete root;
    root = new_root;
    return *this;
}

int main()
{
    return 0;
}
```

## 练习13.29

> 解释 `swap(HasPtr&, HasPtr&)`中对 `swap` 的调用不会导致递归循环。

解：

这其实是3个不同的函数，参数类型不一样，所以不会导致递归循环。

## 练习13.30

> 为你的类值版本的 `HasPtr` 编写 `swap` 函数，并测试它。为你的 `swap` 函数添加一个打印语句，指出函数什么时候执行。

解：

```cpp
#include <string>
#include <iostream>

class HasPtr {
public:
    friend void swap(HasPtr&, HasPtr&);
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
    HasPtr& operator=(const HasPtr &hp) {
        auto new_p = new std::string(*hp.ps);
        delete ps;
        ps = new_p;
        i = hp.i;
        return *this;
    }
    ~HasPtr() {
        delete ps;
    } 
    
    void show() { std::cout << *ps << std::endl; }
private:
    std::string *ps;
    int i;
};

inline
void swap(HasPtr& lhs, HasPtr& rhs)
{
    using std::swap;
    swap(lhs.ps, rhs.ps);
    swap(lhs.i, rhs.i);
    std::cout << "call swap(HasPtr& lhs, HasPtr& rhs)" << std::endl;
}
```

## 练习13.31

> 为你的 `HasPtr` 类定义一个 `<` 运算符，并定义一个 `HasPtr` 的 `vector`。为这个 `vector` 添加一些元素，并对它执行 `sort`。注意何时会调用 `swap`。

解：

```cpp
#include <string>
#include <iostream>

class HasPtr 
{
public:
    friend void swap(HasPtr&, HasPtr&);
    friend bool operator<(const HasPtr &lhs, const HasPtr &rhs);

    HasPtr(const std::string &s = std::string()) 
        : ps(new std::string(s)), i(0) 
    { }

    HasPtr(const HasPtr &hp) 
        : ps(new std::string(*hp.ps)), i(hp.i) 
    { }

    HasPtr& operator=(HasPtr tmp) 
    {
        this->swap(tmp);
        return *this;
    }

    ~HasPtr() 
    {
        delete ps;
    }

    void swap(HasPtr &rhs) 
    {
        using std::swap;
        swap(ps, rhs.ps);
        swap(i, rhs.i);
        std::cout << "call swap(HasPtr &rhs)" << std::endl;
    }

    void show() const
    { 
        std::cout << *ps << std::endl; 
    }
private:
    std::string *ps;
    int i;
};

void swap(HasPtr& lhs, HasPtr& rhs)
{
    lhs.swap(rhs);
}

bool operator<(const HasPtr &lhs, const HasPtr &rhs)
{
    return *lhs.ps < *rhs.ps;
}
```

## 练习13.32

> 类指针的 `HasPtr` 版本会从 `swap` 函数收益吗？如果会，得到了什么益处？如果不是，为什么？

解：

不会。类值的版本利用`swap`交换指针不用进行内存分配，因此得到了性能上的提升。类指针的版本本来就不用进行内存分配，所以不会得到性能提升。

## 练习13.33

> 为什么`Message`的成员`save`和`remove`的参数是一个 `Folder&`？为什么我们不能将参数定义为 `Folder` 或是 `const Folder`？

解：

因为 `save` 和 `remove` 操作需要更新指定 `Folder`。

## 练习13.34

> 编写本节所描述的 `Message`。

解：

头文件：

```cpp
#include <string>
#include <set>

class Folder;

class Message {
    friend void swap(Message &, Message &);
    friend class Folder;
public:
    explicit Message(const std::string &str = ""):contents(str) { }
    Message(const Message&);
    Message& operator=(const Message&);
    ~Message();
    void save(Folder&);
    void remove(Folder&);

    void print_debug();

private:
    std::string contents;
    std::set<Folder*> folders;

    void add_to_Folders(const Message&);
    void remove_from_Folders();

    void addFldr(Folder *f) { folders.insert(f); }
    void remFldr(Folder *f) { folders.erase(f); }
};

void swap(Message&, Message&);

class Folder {
    friend void swap(Folder &, Folder &);
    friend class Message;
public:
    Folder() = default;
    Folder(const Folder &);
    Folder& operator=(const Folder &);
    ~Folder();

    void print_debug();

private:
    std::set<Message*> msgs;

    void add_to_Message(const Folder&);
    void remove_from_Message();

    void addMsg(Message *m) { msgs.insert(m); }
    void remMsg(Message *m) { msgs.erase(m); }
};

void swap(Folder &, Folder &);
```

实现和主函数：

```cpp
#include "ex13_34_36_37.h"
#include <iostream>

void swap(Message &lhs, Message &rhs) 
{
    using std::swap;
    lhs.remove_from_Folders(); // Use existing member function to avoid duplicate code.
    rhs.remove_from_Folders(); // Use existing member function to avoid duplicate code.
    
    swap(lhs.folders, rhs.folders);
    swap(lhs.contents, rhs.contents);
    
    lhs.add_to_Folders(lhs); // Use existing member function to avoid duplicate code.
    rhs.add_to_Folders(rhs); // Use existing member function to avoid duplicate code.
}

// Message Implementation

void Message::save(Folder &f) 
{
    addFldr(&f); // Use existing member function to avoid duplicate code.
    f.addMsg(this);
}

void Message::remove(Folder &f) 
{
    remFldr(&f); // Use existing member function to avoid duplicate code.
    f.remMsg(this);
}

void Message::add_to_Folders(const Message &m) 
{
    for (auto f : m.folders)
        f->addMsg(this);
}

Message::Message(const Message &m) 
    : contents(m.contents), folders(m.folders) 
{
    add_to_Folders(m);
}

void Message::remove_from_Folders() 
{
    for (auto f : folders)
        f->remMsg(this);
    // The book added one line here: folders.clear(); but I think it is redundant and more importantly, it will cause a bug:
    // - In Message::operator=, in the case of self-assignment, it first calls remove_from_Folders() and its folders.clear() 
    //   clears the data member of lhs(rhs), and there is no way we can assign it back to lhs.
    //   Refer to: http://stackoverflow.com/questions/29308115/protection-again-self-assignment
    // - Why is it redundant? As its analogous function Message::add_to_Folders(), Message::remove_from_Folders() should ONLY
    //   take care of the bookkeeping in Folders but not touch the Message's own data members - makes it much clearer and easier
    //   to use. As you can see in the 2 places where we call Message::remove_from_Folders(): in Message::operator=, folders.clear()
    //   introduces a bug as illustrated above; in the destructor ~Message(), the member "folders" will be destroyed anyways, why do
    //   we need to clear it first?
}

Message::~Message() 
{ 
    remove_from_Folders(); 
}

Message &Message::operator=(const Message &rhs) 
{
    remove_from_Folders();
    contents = rhs.contents;
    folders = rhs.folders;
    add_to_Folders(rhs);
    return *this;
}

void Message::print_debug() 
{ 
    std::cout << contents << std::endl; 
}

// Folder Implementation

void swap(Folder &lhs, Folder &rhs) 
{
    using std::swap;
    lhs.remove_from_Message();
    rhs.remove_from_Message();

    swap(lhs.msgs, rhs.msgs);
    
    lhs.add_to_Message(lhs);
    rhs.add_to_Message(rhs);
}

void Folder::add_to_Message(const Folder &f) 
{
    for (auto m : f.msgs)
        m->addFldr(this);
}

Folder::Folder(const Folder &f) 
    : msgs(f.msgs) 
{ 
    add_to_Message(f); 
}

void Folder::remove_from_Message() 
{
    for (auto m : msgs)
        m->remFldr(this);
}

Folder::~Folder() 
{ 
    remove_from_Message(); 
}

Folder &Folder::operator=(const Folder &rhs) 
{
    remove_from_Message();
    msgs = rhs.msgs;
    add_to_Message(rhs);
    return *this;
}

void Folder::print_debug() 
{
    for (auto m : msgs)
        std::cout << m->contents << " ";
    std::cout << std::endl;
}

int main() 
{ 
    return 0;
}
```

## 练习13.35

> 如果`Message` 使用合成的拷贝控制成员，将会发生什么？

在赋值后一些已存在的 `Folders` 将会与 `Message` 不同步。

## 练习13.36

> 设计并实现对应的 `Folder` 类。此类应该保存一个指向 `Folder` 中包含  `Message` 的 `set`。

解：

参考13.34。

## 练习13.37

> 为 `Message` 类添加成员，实现向 `folders` 添加和删除一个给定的 `Folder*`。这两个成员类似`Folder` 类的 `addMsg` 和 `remMsg` 操作。

解：

参考13.34。

## 练习13.38

> 我们并未使用拷贝交换方式来设计 `Message` 的赋值运算符。你认为其原因是什么？

对于动态分配内存的例子来说，拷贝交换方式是一种简洁的设计。而这里的 `Message` 类并不需要动态分配内存，用拷贝交换方式只会增加实现的复杂度。

## 练习13.39

> 编写你自己版本的 `StrVec`，包括自己版本的 `reserve`、`capacity` 和`resize`。

解：

头文件：

```cpp
#include <memory>
#include <string>

// 类vector类内存分配策略的简化实现
class StrVec
{
public:
    StrVec() : elements(nullptr), first_free(nullptr), cap(nullptr) { }
    StrVec(const StrVec&);  // 拷贝构造函数
    StrVec& operator=(const StrVec&);  // 拷贝赋值运算符
    ~StrVec();  // 析构函数

    void push_back(const std::string&);  // 添加元素时拷贝元素
    size_t size() const { return first_free - elements; }
    size_t capacity() const { return cap - elements; }
    std::string *begin() const { return elements; }
    std::string *end() const { return first_free; }

    void reserve(size_t new_cap);
    void resize(size_t count);
    void resize(size_t count, const std::string&);

private:
    // 工具函数，被拷贝构造函数、赋值运算符和析构函数所使用
    std::pair<std::string*, std::string*> alloc_n_copy(const std::string*, const std::string*);
    // 销毁元素并释放内存
	void free();
	// 工具函数，被添加元素的函数使用
    void chk_n_alloc() { if (size() == capacity()) reallocate(); }
	//获得更多内存并拷贝已有元素
    void reallocate();
    void alloc_n_move(size_t new_cap);

private:
    std::string *elements;  // 指向数组首元素的指针
    std::string *first_free;  // 指向数组第一个空闲元素的指针
    std::string *cap;  // 指向数组第一个空闲元素的指针
    std::allocator<std::string> alloc;  // 分配元素
};
```

实现和主函数：

```cpp
#include "ex_13_39.h"

void StrVec::push_back(const std::string &s)
{
    chk_n_alloc();
    alloc.construct(first_free++, s);
}

// 分配足够的内存来保存给定范围的元素，并将这些元素拷贝到新分配的内存中
std::pair<std::string*, std::string*>
StrVec::alloc_n_copy(const std::string *b, const std::string *e)
{
    // 分配空间保存给定范围中的元素
	auto data = alloc.allocate(e - b);
	// 初始化并返回一个pair，该pair由data和uninitialized_copy的返回值构成
    return{ data, std::uninitialized_copy(b, e, data) };
}

void StrVec::free()
{
    // 不能传递给deallocate一个空指针，如果elements为0，函数什么也不做
	if (elements) {
		// 逆序销毁元素
        for (auto p = first_free; p != elements;)
            alloc.destroy(--p);
        alloc.deallocate(elements, cap - elements);
    }
}

StrVec::StrVec(const StrVec &rhs)
{
    // 调用alloc_n_copy分配空间以容纳与rhs中一样多的元素
	auto newdata = alloc_n_copy(rhs.begin(), rhs.end());
    elements = newdata.first;
    first_free = cap = newdata.second;
}

StrVec::~StrVec()
{
    free();
}

StrVec& StrVec::operator = (const StrVec &rhs)
{
    // 调用alloc_n_copy分配空间以容纳与rhs中一样多的元素
	auto data = alloc_n_copy(rhs.begin(), rhs.end());
    free();
    elements = data.first;
    first_free = cap = data.second;
    return *this;
}

void StrVec::alloc_n_move(size_t new_cap)
{
    auto newdata = alloc.allocate(new_cap);
    auto dest = newdata;
    auto elem = elements;
    for (size_t i = 0; i != size(); ++i)
        alloc.construct(dest++, std::move(*elem++));
    free();
    elements = newdata;
    first_free = dest;
    cap = elements + new_cap;
}

void StrVec::reallocate()
{
    auto newcapacity = size() ? 2 * size() : 1;
    alloc_n_move(newcapacity);
}

void StrVec::reserve(size_t new_cap)
{
    if (new_cap <= capacity()) return;
    alloc_n_move(new_cap);
}

void StrVec::resize(size_t count)
{
    resize(count, std::string());
}

void StrVec::resize(size_t count, const std::string &s)
{
    if (count > size()) {
        if (count > capacity()) reserve(count * 2);
        for (size_t i = size(); i != count; ++i)
            alloc.construct(first_free++, s);
    }
    else if (count < size()) {
        while (first_free != elements + count)
            alloc.destroy(--first_free);
    }
}

int main()
{
    return 0;
}
```

## 练习13.40

> 为你的 `StrVec` 类添加一个构造函数，它接受一个 `initializer_list<string>` 参数。

解：

头文件：

```cpp
StrVec(std::initializer_list<std::string>);
```

实现：

```cpp

void StrVec::range_initialize(const std::string *first, const std::string *last)
{
    auto newdata = alloc_n_copy(first, last);
    elements = newdata.first;
    first_free = cap = newdata.second;
}

StrVec::StrVec(std::initializer_list<std::string> il)
{
    range_initialize(il.begin(), il.end());
}
```

## 练习13.41

> 在 `push_back` 中，我们为什么在 `construct` 调用中使用后置递增运算？如果使用前置递增运算的话，会发生什么？

解：

会出现 `unconstructed`。

## 练习13.42

> 在你的 `TextQuery` 和 `QueryResult` 类中用你的 `StrVec` 类代替`vector<string>`，以此来测试你的 `StrVec` 类。

解：

略

## 练习13.43

> 重写 `free` 成员，用 `for_each` 和 `lambda` 来代替 `for` 循环 `destroy` 元素。你更倾向于哪种实现，为什么？

解：

**重写**：

```cpp
for_each(elements, first_free, [this](std::string &rhs){ alloc.destroy(&rhs); });
```

更倾向于函数式写法。

## 练习13.44

> 编写标准库 `string` 类的简化版本，命名为 `String`。你的类应该至少有一个默认构造函数和一个接受 C 风格字符串指针参数的构造函数。使用 `allocator` 为你的 `String`类分配所需内存。

解：

头文件：

```cpp
#include <memory>

class String
{
public:
    String() : String("") { }
    String(const char *);
    String(const String&);
    String& operator=(const String&);
    ~String();

    const char *c_str() const { return elements; }
    size_t size() const { return end - elements; }
    size_t length() const { return end - elements - 1; }

private:
    std::pair<char*, char*> alloc_n_copy(const char*, const char*);
    void range_initializer(const char*, const char*);
    void free();

private:
    char *elements;
    char *end;
    std::allocator<char> alloc;
};
```

实现：

```cpp
#include "ex_13_44_47.h"
#include <algorithm>
#include <iostream>

std::pair<char*, char*>
String::alloc_n_copy(const char *b, const char *e)
{
    auto str = alloc.allocate(e - b);
    return{ str, std::uninitialized_copy(b, e, str) };
}

void String::range_initializer(const char *first, const char *last)
{
    auto newstr = alloc_n_copy(first, last);
    elements = newstr.first;
    end = newstr.second;
}

String::String(const char *s)
{
    char *sl = const_cast<char*>(s);
    while (*sl)
        ++sl;
    range_initializer(s, ++sl);
}

String::String(const String& rhs)
{
    range_initializer(rhs.elements, rhs.end);
    std::cout << "copy constructor" << std::endl;
}

void String::free()
{
    if (elements) {
        std::for_each(elements, end, [this](char &c){ alloc.destroy(&c); });
        alloc.deallocate(elements, end - elements);
    }
}

String::~String()
{
    free();
}

String& String::operator = (const String &rhs)
{
    auto newstr = alloc_n_copy(rhs.elements, rhs.end);
    free();
    elements = newstr.first;
    end = newstr.second;
    std::cout << "copy-assignment" << std::endl;
    return *this;
}
```

测试：

```cpp
#include "ex13_44_47.h"
#include <vector>
#include <iostream>

// Test reference to http://coolshell.cn/articles/10478.html

void foo(String x)
{
    std::cout << x.c_str() << std::endl;
}

void bar(const String& x)
{
    std::cout << x.c_str() << std::endl;
}

String baz()
{
    String ret("world");
    return ret;
}

int main()
{
    char text[] = "world";

    String s0;
    String s1("hello");
    String s2(s0);
    String s3 = s1;
    String s4(text);
    s2 = s1;

    foo(s1);
    bar(s1);
    foo("temporary");
    bar("temporary");
    String s5 = baz();

    std::vector<String> svec;
    svec.reserve(8);
    svec.push_back(s0);
    svec.push_back(s1);
    svec.push_back(s2);
    svec.push_back(s3);
    svec.push_back(s4);
    svec.push_back(s5);
    svec.push_back(baz());
    svec.push_back("good job");

    for (const auto &s : svec) {
        std::cout << s.c_str() << std::endl;
    }
}
```

参考：[A trivial String class that designed for write-on-paper in an interview](https://github.com/chenshuo/recipes/blob/fcf9486f5155117fb8c36b6b0944c5486c71c421/string/StringTrivial.h)

## 练习13.45

> 解释左值引用和右值引用的区别？

解：

定义：

* 常规引用被称为左值引用
* 绑定到右值的引用被称为右值引用。

## 练习13.46

> 什么类型的引用可以绑定到下面的初始化器上？

```cpp
int f();
vector<int> vi(100);
int? r1 = f();
int? r2 = vi[0];
int? r3 = r1;
int? r4 = vi[0] * f();
```

解：

```cpp
int f();
vector<int> vi(100);
int&& r1 = f();
int& r2 = vi[0];
int& r3 = r1;
int&& r4 = vi[0] * f();
```

## 练习13.47

> 对你在练习13.44中定义的 `String`类，为它的拷贝构造函数和拷贝赋值运算符添加一条语句，在每次函数执行时打印一条信息。

解：

参考13.44。

## 练习13.48

> 定义一个`vector<String>` 并在其上多次调用 `push_back`。运行你的程序，并观察 `String` 被拷贝了多少次。

解：

```cpp
#include "ex_13_44_47.h"
#include <vector>
#include <iostream>

// Test reference to http://coolshell.cn/articles/10478.html

void foo(String x)
{
    std::cout << x.c_str() << std::endl;
}

void bar(const String& x)
{
    std::cout << x.c_str() << std::endl;
}

String baz()
{
    String ret("world");
    return ret;
}

int main()
{
    char text[] = "world";

    String s0;
    String s1("hello");
    String s2(s0);
    String s3 = s1;
    String s4(text);
    s2 = s1;

    foo(s1);
    bar(s1);
    foo("temporary");
    bar("temporary");
    String s5 = baz();

    std::vector<String> svec;
    svec.reserve(8);
    svec.push_back(s0);
    svec.push_back(s1);
    svec.push_back(s2);
    svec.push_back(s3);
    svec.push_back(s4);
    svec.push_back(s5);
    svec.push_back(baz());
    svec.push_back("good job");

    for (const auto &s : svec) {
        std::cout << s.c_str() << std::endl;
    }
}
```

## 练习13.49

> 为你的 `StrVec`、`String` 和 `Message` 类添加一个移动构造函数和一个移动赋值运算符。

解：

略

## 练习13.50

> 在你的 `String` 类的移动操作中添加打印语句，并重新运行13.6.1节的练习13.48中的程序，它使用了一个`vector<String>`，观察什么时候会避免拷贝。

解：

```cpp
String baz()
{
    String ret("world");
    return ret; // first avoided
}

String s5 = baz(); // second avoided
```

## 练习13.51

> 虽然 `unique_ptr` 不能拷贝，但我们在12.1.5节中编写了一个 `clone` 函数，它以值的方式返回一个 `unique_ptr`。解释为什么函数是合法的，以及为什么它能正确工作。

解：

在这里是移动的操作而不是拷贝操作，因此是合法的。

## 练习13.52

> 详细解释第478页中的 `HasPtr` 对象的赋值发生了什么？特别是，一步一步描述 `hp`、`hp2` 以及 `HasPtr` 的赋值运算符中的参数 `rhs` 的值发生了什么变化。

解：

左值被拷贝，右值被移动。

## 练习13.53

> 从底层效率的角度看，`HasPtr` 的赋值运算符并不理想，解释为什么？为 `HasPtr` 实现一个拷贝赋值运算符和一个移动赋值运算符，并比较你的新的移动赋值运算符中执行的操作和拷贝并交换版本中的执行的操作。

解：

参考：https://stackoverflow.com/questions/21010371/why-is-it-not-efficient-to-use-a-single-assignment-operator-handling-both-copy-a

## 练习13.54

> 如果我们为 `HasPtr` 定义了移动赋值运算符，但未改变拷贝并交换运算符，会发生什么？编写代码验证你的答案。

解：

```cpp
error: ambiguous overload for 'operator=' (operand types are 'HasPtr' and 'std::remove_reference<HasPtr&>::type { aka HasPtr }')
hp1 = std::move(*pH);
^
```

## 练习13.55

> 为你的 `StrBlob` 添加一个右值引用版本的 `push_back`。

解：

```cpp
void push_back(string &&s) { data->push_back(std::move(s)); }
```

## 练习13.56

> 如果 `sorted` 定义如下，会发生什么？

```cpp
Foo Foo::sorted() const & {
	Foo ret(*this);
	return ret.sorted();
}
```

解：

会产生递归并且最终溢出。

## 练习13.57

> 如果 `sorted`定义如下，会发生什么：

```cpp
Foo Foo::sorted() const & { return Foo(*this).sorted(); }
```

解：

没问题。会调用移动版本。

## 练习13.58

> 编写新版本的 `Foo` 类，其 `sorted` 函数中有打印语句，测试这个类，来验证你对前两题的答案是否正确。

解：

```cpp
#include <vector>
#include <iostream>
#include <algorithm>

using std::vector; using std::sort;

class Foo {
public:
    Foo sorted() &&;
    Foo sorted() const &;
private:
    vector<int> data;
};

Foo Foo::sorted() && {
    sort(data.begin(), data.end());
    std::cout << "&&" << std::endl; // debug
    return *this;
}

Foo Foo::sorted() const & {
//    Foo ret(*this);
//    sort(ret.data.begin(), ret.data.end());
//    return ret;

    std::cout << "const &" << std::endl; // debug

//    Foo ret(*this);
//    ret.sorted();     // Exercise 13.56
//    return ret;

    return Foo(*this).sorted(); // Exercise 13.57
}

int main()
{
    Foo().sorted(); // call "&&"
    Foo f;
    f.sorted(); // call "const &"
}
```





# 第十四章 重载运算与类型转换

## 练习14.1

> 在什么情况下重载的运算符与内置运算符有所区别？在什么情况下重载的运算符又与内置运算符一样？

解：

我们可以直接调用重载运算符函数。重置运算符与内置运算符有一样的优先级与结合性。

## 练习14.2

> 为 `Sales_data` 编写重载的输入、输出、加法和复合赋值运算符。

解：

头文件：

```cpp
#include <string>
#include <iostream>

class Sales_data {
    friend std::istream& operator>>(std::istream&, Sales_data&); // input
    friend std::ostream& operator<<(std::ostream&, const Sales_data&); // output
    friend Sales_data operator+(const Sales_data&, const Sales_data&); // addition

public:
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data() : Sales_data("", 0, 0.0f){ }
    Sales_data(const std::string &s) : Sales_data(s, 0, 0.0f){ }
    Sales_data(std::istream &is);

    Sales_data& operator+=(const Sales_data&); // compound-assignment
    std::string isbn() const { return bookNo; }

private:
    inline double avg_price() const;

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

std::istream& operator>>(std::istream&, Sales_data&);
std::ostream& operator<<(std::ostream&, const Sales_data&);
Sales_data operator+(const Sales_data&, const Sales_data&);

inline double Sales_data::avg_price() const
{
    return units_sold ? revenue/units_sold : 0;
}
```

主函数：

```cpp
#include "ex_14_02.h"

Sales_data::Sales_data(std::istream &is) : Sales_data()
{
    is >> *this;
}

Sales_data& Sales_data::operator+=(const Sales_data &rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

std::istream& operator>>(std::istream &is, Sales_data &item)
{
    double price = 0.0;
    is >> item.bookNo >> item.units_sold >> price;
    if (is)
        item.revenue = price * item.units_sold;
    else
        item = Sales_data();
    return is;
}

std::ostream& operator<<(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
    return os;
}

Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum += rhs;
    return sum;
}
```

## 练习14.3

> `string` 和 `vector` 都定义了重载的`==`以比较各自的对象，假设 `svec1` 和 `svec2` 是存放 `string` 的 `vector`，确定在下面的表达式中分别使用了哪个版本的`==`？

```cpp
(a) "cobble" == "stone"
(b) svec1[0] == svec2[0]
(c) svec1 == svec2
(d) svec1[0] == "stone"
```

解：

* (a) 都不是。
* (b) `string` 
* (c) `vector` 
* (d) `string`

## 练习14.4

> 如何确定下列运算符是否应该是类的成员？

```cpp
(a) %
(b) %=
(c) ++
(d) ->
(e) <<
(f) &&
(g) ==
(h) ()
```

解：

* (a) 不需要是成员。
* (b) 是成员。
* (c) 是成员。
* (d) 必须是成员。
* (e) 不需要是成员。
* (f) 不需要是成员。
* (g) 不需要是成员。
* (h) 必须是成员。

## 练习14.5

> 在7.5.1节中的练习7.40中，编写了下列类中某一个的框架，请问在这个类中应该定义重载的运算符吗？如果是，请写出来。

```cpp
(a) Book
(b) Date
(c) Employee
(d) Vehicle
(e) Object
(f) Tree
```

解：

`Book`，应该重载。

头文件：

```cpp
#include <iostream>
#include <string>

class Book
{
	friend std::istream& operator>>(std::istream&, Book&);
	friend std::ostream& operator<<(std::ostream&, const Book&);
	friend bool operator==(const Book&, const Book&);
	friend bool operator!=(const Book&, const Book&);

public:
	Book() = default;
	Book(unsigned no, std::string name, std::string author, std::string pubdate) :no_(no), name_(name), author_(author), pubdate_(pubdate) {}
	Book(std::istream &in) { in >> *this; }

private:
	unsigned no_;
	std::string name_;
	std::string author_;
	std::string pubdate_;
};

std::istream& operator>>(std::istream&, Book&);
std::ostream& operator<<(std::ostream&, const Book&);
bool operator==(const Book&, const Book&);
bool operator!=(const Book&, const Book&);
```

实现：

```cpp
#include "ex_14_5.h"

std::istream& operator>>(std::istream &in, Book &book)
{
	in >> book.no_ >> book.name_ >> book.author_ >> book.pubdate_;
	if (!in)
		book = Book();
	return in;
}

std::ostream& operator<<(std::ostream &out, const Book &book)
{
	out << book.no_ << " " << book.name_ << " " << book.author_ << " " << book.pubdate_;
	return out;
}

bool operator==(const Book &lhs, const Book &rhs)
{
	return lhs.no_ == rhs.no_;
}

bool operator!=(const Book &lhs, const Book &rhs)
{
	return !(lhs == rhs);
}
```

测试：

```cpp
#include "ex_14_5.h"

int main()
{
	Book book1(123, "CP5", "Lippman", "2012");
	Book book2(123, "CP5", "Lippman", "2012");

	if (book1 == book2)
		std::cout << book1 << std::endl;
}
```

## 练习14.6

> 为你的 `Sales_data` 类定义输出运算符。

解：

参考14.2。

## 练习14.7

> 你在13.5节的练习中曾经编写了一个`String`类，为它定义一个输出运算符。

解：

头文件：

```cpp
#include <memory>
#include <iostream>

class String
{
	friend std::ostream& operator<<(std::ostream&, const String&);
public:
	String() : String("") {}
	String(const char *);
	String(const String&);
	String& operator=(const String&);
	~String();

	const char *c_str() const { return elements; }
	size_t size() const { return end - elements; }
	size_t length() const { return end - elements - 1; }

private:
	std::pair<char*, char*> alloc_n_copy(const char*, const char*);
	void range_initializer(const char*, const char*);
	void free();

private:
	char *elements;
	char *end;
	std::allocator<char> alloc;
};

std::ostream& operator<<(std::ostream&, const String&);
```

实现：

```cpp
#include "ex_14_7.h"
#include <algorithm>
#include <iostream>

std::pair<char*, char*>
String::alloc_n_copy(const char *b, const char *e)
{
	auto str = alloc.allocate(e - b);
	return{ str, std::uninitialized_copy(b, e, str) };
}

void String::range_initializer(const char *first, const char *last)
{
	auto newstr = alloc_n_copy(first, last);
	elements = newstr.first;
	end = newstr.second;
}

String::String(const char *s)
{
	char *sl = const_cast<char*>(s);
	while (*sl)
		++sl;
	range_initializer(s, ++sl);
}

String::String(const String& rhs)
{
	range_initializer(rhs.elements, rhs.end);
	std::cout << "copy constructor" << std::endl;
}

void String::free()
{
	if (elements)
	{
		std::for_each(elements, end, [this](char &c) { alloc.destroy(&c); });
		alloc.deallocate(elements, end - elements);
	}
}

String::~String()
{
	free();
}

String& String::operator = (const String &rhs)
{
	auto newstr = alloc_n_copy(rhs.elements, rhs.end);
	free();
	elements = newstr.first;
	end = newstr.second;
	std::cout << "copy-assignment" << std::endl;
	return *this;
}

std::ostream& operator<<(std::ostream &os, const String &s)
{
	char *c = const_cast<char*>(s.c_str());
	while (*c)
		os << *c++;
	return os;
}
```

测试：

```cpp
#include "ex_14_7.h"

int main()
{
	String str("Hello World");
	std::cout << str << std::endl;
}
```

## 练习14.8

> 你在7.5.1节中的练习中曾经选择并编写了一个类，为它定义一个输出运算符。

解：

参考14.5。

## 练习14.9

> 为你的 `Sales_data` 类定义输入运算符。

解：

参考14.2。

## 练习14.10

> 对于 `Sales_data` 的输入运算符来说如果给定了下面的输入将发生什么情况？

```cpp
(a) 0-201-99999-9 10 24.95
(b) 10 24.95 0-210-99999-9
```

解：

* (a) 格式正确。
* (b) 不合法的输入。因为程序试图将 `0-210-99999-9` 转换为 `float`。

## 练习14.11

> 下面的 `Sales_data` 输入运算符存在错误吗？如果有，请指出来。对于这个输入运算符如果仍然给定上个练习的输入将会发生什么情况？

```cpp
istream& operator>>(istream& in, Sales_data& s)
{
	double price;
	in >> s.bookNo >> s.units_sold >> price;
	s.revence = s.units_sold >> price;
	return in;
}
```

解：

没有输入检查，什么也不会发生。

## 练习14.12

> 你在7.5.1节的练习中曾经选择并编写了一个类，为它定义一个输入运算符并确保该运算符可以处理输入错误。

解：

参考14.5。

## 练习14.13

> 你认为 `Sales_data` 类还应该支持哪些其他算术运算符？如果有的话，请给出它们的定义。

解：

没有其他了。

## 练习14.14

> 你觉得为什么调用 `operator+=` 来定义`operator+` 比其他方法更有效？

解：

因为用 `operator+=` 会避免使用一个临时对象，而使得更有效。

## 练习14.15

> 你在7.5.1节的练习7.40中曾经选择并编写了一个类，你认为它应该含有其他算术运算符吗？如果是，请实现它们；如果不是，解释原因。

解：

头文件：

```cpp
#include <iostream>
#include <string>

class Book
{
	friend std::istream& operator>>(std::istream&, Book&);
	friend std::ostream& operator<<(std::ostream&, const Book&);
	friend bool operator==(const Book&, const Book&);
	friend bool operator!=(const Book&, const Book&);
	friend bool operator<(const Book&, const Book&);
	friend bool operator>(const Book&, const Book&);
	friend Book operator+(const Book&, const Book&);

public:
	Book() = default;
	Book(unsigned no, std::string name, std::string author, std::string pubdate, unsigned number) :no_(no), name_(name), author_(author), pubdate_(pubdate), number_(number) {}
	Book(std::istream &in) { in >> *this; }

	Book& operator+=(const Book&);

private:
	unsigned no_;
	std::string name_;
	std::string author_;
	std::string pubdate_;
	unsigned number_;
};

std::istream& operator>>(std::istream&, Book&);
std::ostream& operator<<(std::ostream&, const Book&);
bool operator==(const Book&, const Book&);
bool operator!=(const Book&, const Book&);
bool operator<(const Book&, const Book&);
bool operator>(const Book&, const Book&);
Book operator+(const Book&, const Book&);
```

实现：

```cpp
#include "ex_14_15.h"

std::istream& operator>>(std::istream &in, Book &book)
{
	in >> book.no_ >> book.name_ >> book.author_ >> book.pubdate_ >> book.number_;
	return in;
}

std::ostream& operator<<(std::ostream &out, const Book &book)
{
	out << book.no_ << " " << book.name_ << " " << book.author_ << " " << book.pubdate_ << " " << book.number_ << std::endl;
	return out;
}

bool operator==(const Book &lhs, const Book &rhs)
{
	return lhs.no_ == rhs.no_;
}

bool operator!=(const Book &lhs, const Book &rhs)
{
	return !(lhs == rhs);
}

bool operator<(const Book &lhs, const Book &rhs)
{
	return lhs.no_ < rhs.no_;
}

bool operator>(const Book &lhs, const Book &rhs)
{
	return rhs < lhs;
}

Book& Book::operator+=(const Book &rhs)
{
	if (rhs == *this)
		this->number_ += rhs.number_;

	return *this;
}

Book operator+(const Book &lhs, const Book &rhs)
{
	Book book = lhs;
	book += rhs;
	return book;
}
```

测试：

```cpp
#include "ex_14_15.h"

int main()
{
	Book cp5_1(12345, "CP5", "Lippmen", "2012", 2);
	Book cp5_2(12345, "CP5", "Lippmen", "2012", 4);

	std::cout << cp5_1 + cp5_2 << std::endl;
}
```

## 练习14.16

> 为你的 `StrBlob` 类、`StrBlobPtr` 类、`StrVec` 类和 `String` 类分别定义相等运算符和不相等运算符。

解：

略

## 练习14.17

> 你在7.5.1节中的练习7.40中曾经选择并编写了一个类，你认为它应该含有相等运算符吗？如果是，请实现它；如果不是，解释原因。

解：

参考14.15。

## 练习14.18

> 为你的 `StrBlob` 类、`StrBlobPtr` 类、`StrVec` 类和 `String` 类分别定义关系运算符。

解：

略

## 练习14.19

> 你在7.5.1节的练习7.40中曾经选择并编写了一个类，你认为它应该含有关系运算符吗？如果是，请实现它；如果不是，解释原因。

解：

参考14.15。

## 练习14.20

> 为你的 `Sales_data` 类定义加法和复合赋值运算符。

解：

参考14.2。

## 练习14.21

> 编写 `Sales_data` 类的`+` 和`+=` 运算符，使得 `+` 执行实际的加法操作而 `+=` 调用`+`。相比14.3节和14.4节对这两个运算符的定义，本题的定义有何缺点？试讨论之。

解：

缺点：使用了一个 `Sales_data` 的临时对象，但它并不是必须的。

## 练习14.22

> 定义赋值运算符的一个新版本，使得我们能把一个表示 `ISBN` 的 `string` 赋给一个 `Sales_data` 对象。

解：

头文件：

```cpp
#include <string>
#include <iostream>

class Sales_data
{
	friend std::istream& operator>>(std::istream&, Sales_data&);
	friend std::ostream& operator<<(std::ostream&, const Sales_data&);
	friend Sales_data operator+(const Sales_data&, const Sales_data&);

public:
	Sales_data(const std::string &s, unsigned n, double p) :bookNo(s), units_sold(n), revenue(n*p) {}
	Sales_data() : Sales_data("", 0, 0.0f) {}
	Sales_data(const std::string &s) : Sales_data(s, 0, 0.0f) {}
	Sales_data(std::istream &is);

	Sales_data& operator=(const std::string&);

	Sales_data& operator+=(const Sales_data&);
	std::string isbn() const { return bookNo; }

private:
	inline double avg_price() const;

	std::string bookNo;
	unsigned units_sold = 0;
	double revenue = 0.0;
};

std::istream& operator>>(std::istream&, Sales_data&);
std::ostream& operator<<(std::ostream&, const Sales_data&);
Sales_data operator+(const Sales_data&, const Sales_data&);

inline double Sales_data::avg_price() const
{
	return units_sold ? revenue / units_sold : 0;
}
```

实现：

```cpp
#include "ex_14_22.h"

Sales_data::Sales_data(std::istream &is) : Sales_data()
{
	is >> *this;
}

Sales_data& Sales_data::operator+=(const Sales_data &rhs)
{
	units_sold += rhs.units_sold;
	revenue += rhs.revenue;
	return *this;
}

std::istream& operator>>(std::istream &is, Sales_data &item)
{
	double price = 0.0;
	is >> item.bookNo >> item.units_sold >> price;
	if (is)
		item.revenue = price * item.units_sold;
	else
		item = Sales_data();
	return is;
}

std::ostream& operator<<(std::ostream &os, const Sales_data &item)
{
	os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
	return os;
}

Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs)
{
	Sales_data sum = lhs;
	sum += rhs;
	return sum;
}

Sales_data& Sales_data::operator=(const std::string &isbn)
{
	*this = Sales_data(isbn);
	return *this;
}
```

测试：

```cpp
#include "ex_4_22.h"

int main()
{
	std::string strCp5("C++ Primer 5th");
	Sales_data cp5 = strCp5;
	std::cout << cp5 << std::endl;
}
```

## 练习14.23

> 为你的`StrVec` 类定义一个 `initializer_list` 赋值运算符。

解：

头文件：

```cpp
#include <memory>
#include <string>
#include <initializer_list>

#ifndef _MSC_VER
#define NOEXCEPT noexcept
#else
#define NOEXCEPT
#endif

class StrVec
{
	friend bool operator==(const StrVec&, const StrVec&);
	friend bool operator!=(const StrVec&, const StrVec&);
	friend bool operator< (const StrVec&, const StrVec&);
	friend bool operator> (const StrVec&, const StrVec&);
	friend bool operator<=(const StrVec&, const StrVec&);
	friend bool operator>=(const StrVec&, const StrVec&);

public:
	StrVec() : elements(nullptr), first_free(nullptr), cap(nullptr) {}
	StrVec(std::initializer_list<std::string>);
	StrVec(const StrVec&);
	StrVec& operator=(const StrVec&);
	StrVec(StrVec&&) NOEXCEPT;
	StrVec& operator=(StrVec&&)NOEXCEPT;
	~StrVec();

	StrVec& operator=(std::initializer_list<std::string>);

	void push_back(const std::string&);
	size_t size() const { return first_free - elements; }
	size_t capacity() const { return cap - elements; }
	std::string *begin() const { return elements; }
	std::string *end() const { return first_free; }

	std::string& at(size_t pos) { return *(elements + pos); }
	const std::string& at(size_t pos) const { return *(elements + pos); }

	void reserve(size_t new_cap);
	void resize(size_t count);
	void resize(size_t count, const std::string&);

private:
	std::pair<std::string*, std::string*> alloc_n_copy(const std::string*, const std::string*);
	void free();
	void chk_n_alloc() { if (size() == capacity()) reallocate(); }
	void reallocate();
	void alloc_n_move(size_t new_cap);
	void range_initialize(const std::string*, const std::string*);

private:
	std::string *elements;
	std::string *first_free;
	std::string *cap;
	std::allocator<std::string> alloc;
};

bool operator==(const StrVec&, const StrVec&);
bool operator!=(const StrVec&, const StrVec&);
bool operator< (const StrVec&, const StrVec&);
bool operator> (const StrVec&, const StrVec&);
bool operator<=(const StrVec&, const StrVec&);
bool operator>=(const StrVec&, const StrVec&);
```

实现：

```cpp
#include "ex_14_23.h"
#include <algorithm>

void StrVec::push_back(const std::string &s)
{
	chk_n_alloc();
	alloc.construct(first_free++, s);
}

std::pair<std::string*, std::string*>
StrVec::alloc_n_copy(const std::string *b, const std::string *e)
{
	auto data = alloc.allocate(e - b);
	return{ data, std::uninitialized_copy(b, e, data) };
}

void StrVec::free()
{
	if (elements)
	{
		for_each(elements, first_free, [this](std::string &rhs) { alloc.destroy(&rhs); });
		alloc.deallocate(elements, cap - elements);
	}
}

void StrVec::range_initialize(const std::string *first, const std::string *last)
{
	auto newdata = alloc_n_copy(first, last);
	elements = newdata.first;
	first_free = cap = newdata.second;
}

StrVec::StrVec(const StrVec &rhs)
{
	range_initialize(rhs.begin(), rhs.end());
}

StrVec::StrVec(std::initializer_list<std::string> il)
{
	range_initialize(il.begin(), il.end());
}

StrVec::~StrVec()
{
	free();
}

StrVec& StrVec::operator = (const StrVec &rhs)
{
	auto data = alloc_n_copy(rhs.begin(), rhs.end());
	free();
	elements = data.first;
	first_free = cap = data.second;
	return *this;
}

void StrVec::alloc_n_move(size_t new_cap)
{
	auto newdata = alloc.allocate(new_cap);
	auto dest = newdata;
	auto elem = elements;
	for (size_t i = 0; i != size(); ++i)
		alloc.construct(dest++, std::move(*elem++));
	free();
	elements = newdata;
	first_free = dest;
	cap = elements + new_cap;
}

void StrVec::reallocate()
{
	auto newcapacity = size() ? 2 * size() : 1;
	alloc_n_move(newcapacity);
}

void StrVec::reserve(size_t new_cap)
{
	if (new_cap <= capacity()) return;
	alloc_n_move(new_cap);
}

void StrVec::resize(size_t count)
{
	resize(count, std::string());
}

void StrVec::resize(size_t count, const std::string &s)
{
	if (count > size())
	{
		if (count > capacity()) reserve(count * 2);
		for (size_t i = size(); i != count; ++i)
			alloc.construct(first_free++, s);
	}
	else if (count < size())
	{
		while (first_free != elements + count)
			alloc.destroy(--first_free);
	}
}

StrVec::StrVec(StrVec &&s) NOEXCEPT : elements(s.elements), first_free(s.first_free), cap(s.cap)
{
	// leave s in a state in which it is safe to run the destructor.
	s.elements = s.first_free = s.cap = nullptr;
}

StrVec& StrVec::operator = (StrVec &&rhs) NOEXCEPT
{
	if (this != &rhs)
	{
		free();
		elements = rhs.elements;
		first_free = rhs.first_free;
		cap = rhs.cap;
		rhs.elements = rhs.first_free = rhs.cap = nullptr;
	}
	return *this;
}

bool operator==(const StrVec &lhs, const StrVec &rhs)
{
	return (lhs.size() == rhs.size() && std::equal(lhs.begin(), lhs.end(), rhs.begin()));
}

bool operator!=(const StrVec &lhs, const StrVec &rhs)
{
	return !(lhs == rhs);
}

bool operator<(const StrVec &lhs, const StrVec &rhs)
{
	return std::lexicographical_compare(lhs.begin(), lhs.end(), rhs.begin(), rhs.end());
}

bool operator>(const StrVec &lhs, const StrVec &rhs)
{
	return rhs < lhs;
}

bool operator<=(const StrVec &lhs, const StrVec &rhs)
{
	return !(rhs < lhs);
}

bool operator>=(const StrVec &lhs, const StrVec &rhs)
{
	return !(lhs < rhs);
}

StrVec& StrVec::operator=(std::initializer_list<std::string> il)
{
	*this = StrVec(il);
	return *this;
}
```

测试：

```cpp
#include "ex_14_23.h"
#include <vector>
#include <iostream>

int main()
{
	StrVec vec;
	vec.reserve(6);
	std::cout << "capacity(reserve to 6): " << vec.capacity() << std::endl;

	vec.reserve(4);
	std::cout << "capacity(reserve to 4): " << vec.capacity() << std::endl;

	vec.push_back("hello");
	vec.push_back("world");

	vec.resize(4);

	for (auto i = vec.begin(); i != vec.end(); ++i)
		std::cout << *i << std::endl;
	std::cout << "-EOF-" << std::endl;

	vec.resize(1);

	for (auto i = vec.begin(); i != vec.end(); ++i)
		std::cout << *i << std::endl;
	std::cout << "-EOF-" << std::endl;

	StrVec vec_list{ "hello", "world", "pezy" };

	for (auto i = vec_list.begin(); i != vec_list.end(); ++i)
		std::cout << *i << " ";
	std::cout << std::endl;

	// Test operator==

	const StrVec const_vec_list = { "hello", "world", "pezy" };
	if (vec_list == const_vec_list)
	for (const auto &str : const_vec_list)
		std::cout << str << " ";
	std::cout << std::endl;

	// Test operator<
	const StrVec const_vec_list_small = { "hello", "pezy", "ok" };
	std::cout << (const_vec_list_small < const_vec_list) << std::endl;
}
```

## 练习14.24

> 你在7.5.1节的练习7.40中曾经选择并编写了一个类，你认为它应该含有拷贝赋值和移动赋值运算符吗？如果是，请实现它们。

解：

头文件：

```cpp
#ifndef DATE_H
#define DATE_H

#ifndef _MSC_VER
#define NOEXCEPT noexcept
#else
#define NOEXCEPT
#endif

#include <iostream>
#include <vector>

class Date
{
	friend  bool            operator ==(const Date& lhs, const Date& rhs);
	friend  bool            operator < (const Date &lhs, const Date &rhs);
	friend  bool            check(const Date &d);
	friend  std::ostream&   operator <<(std::ostream& os, const Date& d);
public:
	typedef std::size_t Size;

	// default constructor
	Date() = default;
	// constructor taking Size as days
	explicit Date(Size days);
	// constructor taking three Size
	Date(Size d, Size m, Size y) : day(d), month(m), year(y) {}
	// constructor taking iostream
	Date(std::istream &is, std::ostream &os);

	// copy constructor
	Date(const Date& d);
	// move constructor
	Date(Date&& d) NOEXCEPT;

	// copy operator=
	Date& operator= (const Date& d);
	// move operator=
	Date& operator= (Date&& rhs) NOEXCEPT;

	// destructor  --  in this case, user-defined destructor is not nessary.
	~Date() { std::cout << "destroying\n"; }

	// members
	Size toDays() const;  //not implemented yet.
	Date& operator +=(Size offset);
	Date& operator -=(Size offset);


private:
	Size    day = 1;
	Size    month = 1;
	Size    year = 0;
};

static const Date::Size YtoD_400 = 146097;    //365*400 + 400/4 -3 == 146097
static const Date::Size YtoD_100 = 36524;    //365*100 + 100/4 -1 ==  36524
static const Date::Size YtoD_4 = 1461;    //365*4 + 1          ==   1461
static const Date::Size YtoD_1 = 365;    //365

// normal year
static const std::vector<Date::Size> monthsVec_n =
{ 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };

// leap year
static const std::vector<Date::Size> monthsVec_l =
{ 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };

// non-member operators:  <<  >>  -   ==  !=  <   <=  >   >=
//
std::ostream&
operator <<(std::ostream& os, const Date& d);
std::istream&
operator >>(std::istream& is, Date& d);
int
operator - (const Date& lhs, const Date& rhs);
bool
operator ==(const Date& lhs, const Date& rhs);
bool
operator !=(const Date& lhs, const Date& rhs);
bool
operator < (const Date& lhs, const Date& rhs);
bool
operator <=(const Date& lhs, const Date& rhs);
bool
operator  >(const Date& lhs, const Date& rhs);
bool
operator >=(const Date& lhs, const Date& rhs);
Date
operator - (const Date& lhs, Date::Size  rhs);
Date
operator  +(const Date& lhs, Date::Size  rhs);



//  utillities:
bool check(const Date &d);
inline bool
isLeapYear(Date::Size y);




// check if the date object passed in is valid
inline bool
check(const Date &d)
{
	if (d.month == 0 || d.month >12)
		return false;
	else
	{
		//    month == 1 3 5 7 8 10 12
		if (d.month == 1 || d.month == 3 || d.month == 5 || d.month == 7 ||
			d.month == 8 || d.month == 10 || d.month == 12)
		{
			if (d.day == 0 || d.day > 31) return false;
			else
				return true;
		}
		else
		{
			//    month == 4 6 9 11
			if (d.month == 4 || d.month == 6 || d.month == 9 || d.month == 11)
			{
				if (d.day == 0 || d.day > 30) return false;
				else
					return true;
			}
			else
			{
				//    month == 2
				if (isLeapYear(d.year))
				{
					if (d.day == 0 || d.day >29)  return false;
					else
						return true;
				}
				else
				{
					if (d.day == 0 || d.day >28)  return false;
					else
						return true;
				}
			}
		}
	}
}

inline bool
isLeapYear(Date::Size y)
{
	if (!(y % 400))
	{
		return true;
	}
	else
	{
		if (!(y % 100))
		{
			return false;
		}
		else
			return !(y % 4);
	}
}
#endif // DATE_H
```

实现：

```cpp
#include "ex_14_24.h"
#include <algorithm>

// constructor taking Size as days
// the argument must be within (0, 2^32)
Date::Date(Size days)
{
	// calculate the year
	Size y400 = days / YtoD_400;
	Size y100 = (days - y400*YtoD_400) / YtoD_100;
	Size y4 = (days - y400*YtoD_400 - y100*YtoD_100) / YtoD_4;
	Size y = (days - y400*YtoD_400 - y100*YtoD_100 - y4*YtoD_4) / 365;
	Size d = days - y400*YtoD_400 - y100*YtoD_100 - y4*YtoD_4 - y * 365;
	this->year = y400 * 400 + y100 * 100 + y4 * 4 + y;

	// check if leap and choose the months vector accordingly
	std::vector<Size>currYear
		= isLeapYear(this->year) ? monthsVec_l : monthsVec_n;

	// calculate day and month using find_if + lambda
	Size D_accumu = 0, M_accumu = 0;
	// @bug    fixed:  the variabbles above hade been declared inside the find_if as static
	//                 which caused the bug. It works fine now after being move outside.

	std::find_if(currYear.cbegin(), currYear.cend(), [&](Size m)
	{

		D_accumu += m;
		M_accumu++;

		if (d < D_accumu)
		{
			this->month = M_accumu;
			this->day = d + m - D_accumu;

			return true;
		}
		else
			return false;
	});
}

// construcotr taking iostream
Date::Date(std::istream &is, std::ostream &os)
{
	is >> day >> month >> year;

	if (is)
	{
		if (check(*this)) return;
		else
		{
			os << "Invalid input! Object is default initialized.";
			*this = Date();
		}
	}
	else
	{
		os << "Invalid input! Object is default initialized.";
		*this = Date();
	}

}

// copy constructor
Date::Date(const Date &d) :
day(d.day), month(d.month), year(d.year)
{}

// move constructor
Date::Date(Date&& d) NOEXCEPT :
day(d.day), month(d.month), year(d.year)
{ std::cout << "copy moving"; }

// copy operator=
Date &Date::operator= (const Date &d)
{
	this->day = d.day;
	this->month = d.month;
	this->year = d.year;

	return *this;
}

// move operator=
Date &Date::operator =(Date&& rhs) NOEXCEPT
{
	if (this != &rhs)
	{
		this->day = rhs.day;
		this->month = rhs.month;
		this->year = rhs.year;
	}
	std::cout << "moving =";

	return *this;
}

// conver to days
Date::Size Date::toDays() const
{
	Size result = this->day;

	// check if leap and choose the months vector accordingly
	std::vector<Size>currYear
		= isLeapYear(this->year) ? monthsVec_l : monthsVec_n;

	// calculate result + days by months
	for (auto it = currYear.cbegin(); it != currYear.cbegin() + this->month - 1; ++it)
		result += *it;

	// calculate result + days by years
	result += (this->year / 400)      * YtoD_400;
	result += (this->year % 400 / 100)  * YtoD_100;
	result += (this->year % 100 / 4)    * YtoD_4;
	result += (this->year % 4)        * YtoD_1;

	return result;
}

// member operators:   +=  -=

Date &Date::operator +=(Date::Size offset)
{
	*this = Date(this->toDays() + offset);
	return *this;
}

Date &Date::operator -=(Date::Size offset)
{
	if (this->toDays() > offset)
		*this = Date(this->toDays() - offset);
	else
		*this = Date();

	return *this;
}

// non-member operators:  <<  >>  -   ==  !=  <   <=  >   >=

std::ostream&
operator <<(std::ostream& os, const Date& d)
{
	os << d.day << " " << d.month << " " << d.year;
	return os;
}

std::istream&
operator >>(std::istream& is, Date& d)
{
	if (is)
	{
		Date input = Date(is, std::cout);
		if (check(input))    d = input;
	}
	return is;
}


int operator -(const Date &lhs, const Date &rhs)
{
	return lhs.toDays() - rhs.toDays();
}


bool operator ==(const Date &lhs, const Date &rhs)
{
	return (lhs.day == rhs.day) &&
		(lhs.month == rhs.month) &&
		(lhs.year == rhs.year);
}


bool operator !=(const Date &lhs, const Date &rhs)
{
	return !(lhs == rhs);
}


bool operator < (const Date &lhs, const Date &rhs)
{
	return lhs.toDays() < rhs.toDays();
}


bool operator <=(const Date &lhs, const Date &rhs)
{
	return (lhs < rhs) || (lhs == rhs);
}


bool operator >(const Date &lhs, const Date &rhs)
{
	return !(lhs <= rhs);
}


bool operator >=(const Date &lhs, const Date &rhs)
{
	return !(lhs < rhs);
}


Date operator - (const Date &lhs, Date::Size rhs)
{                                       //  ^^^ rhs must not be larger than 2^32-1
	// copy lhs
	Date result(lhs);
	result -= rhs;

	return result;
}


Date operator + (const Date &lhs, Date::Size rhs)
{                                       //  ^^^ rhs must not be larger than 2^32-1
	// copy lhs
	Date result(lhs);
	result += rhs;

	return result;
}
```

测试：

```cpp
#include "ex_14_24.h"
#include <iostream>

int main()
{

	Date lhs(9999999), rhs(1);

	std::cout << (lhs -= 12000) << "\n";

	return 0;
}
```

## 练习14.25

> 上题的这个类还需要定义其他赋值运算符吗？如果是，请实现它们；同时说明运算对象应该是什么类型并解释原因。

解：

是。如上题。

## 练习14.26

> 为你的 `StrBlob` 类、`StrBlobPtr` 类、`StrVec` 类和 `String` 类定义下标运算符。

解：

略

## 练习14.27

> 为你的 `StrBlobPtr` 类添加递增和递减运算符。

解：

只显示添加的代码：

```cpp
class StrBlobPtr {
public:
    string& deref() const;
    StrBlobPtr& operator++();
    StrBlobPtr& operator--();
    StrBlobPtr operator++(int);
    StrBlobPtr operator--(int);
    StrBlobPtr& operator+=(size_t);
    StrBlobPtr& operator-=(size_t);
    StrBlobPtr operator+(size_t) const;
    StrBlobPtr operator-(size_t) const;
};

inline StrBlobPtr& StrBlobPtr::operator++()
{
    check(curr, "increment past end of StrBlobPtr");
    ++curr;
    return *this;
}

inline StrBlobPtr& StrBlobPtr::operator--()
{
    check(--curr, "decrement past begin of StrBlobPtr");
    return *this;
}

inline StrBlobPtr StrBlobPtr::operator++(int)
{
    StrBlobPtr ret = *this;
    ++*this;
    return ret;
}

inline StrBlobPtr StrBlobPtr::operator--(int)
{
    StrBlobPtr ret = *this;
    --*this;
    return ret;
}

inline StrBlobPtr& StrBlobPtr::operator+=(size_t n)
{
    curr += n;
    check(curr, "increment past end of StrBlobPtr");
    return *this;
}

inline StrBlobPtr& StrBlobPtr::operator-=(size_t n)
{
    curr -= n;
    check(curr, "increment past end of StrBlobPtr");
    return *this;
}

inline StrBlobPtr StrBlobPtr::operator+(size_t n) const
{
    StrBlobPtr ret = *this;
    ret += n;
    return ret;
}

inline StrBlobPtr StrBlobPtr::operator-(size_t n) const
{
    StrBlobPtr ret = *this;
    ret -= n;
    return ret;
}
```

## 练习14.28

> 为你的 `StrBlobPtr` 类添加加法和减法运算符，使其可以实现指针的算术运算。

解：

参考14.27。

## 练习14.29

> 为什么不定义`const` 版本的递增和递减运算符？

解：

因为递增和递减会改变对象本身，所以定义 `const` 版本的毫无意义。

## 练习14.30

> 为你的 `StrBlobPtr` 类和在12.1.6节练习12.22中定义的 `ConstStrBlobPtr` 的类分别添加解引用运算符和箭头运算符。注意：因为 `ConstStrBlobPtr` 的数据成员指向`const vector`，所以`ConstStrBlobPtr` 中的运算符必须返回常量引用。

解：

略。

## 练习14.31

> 我们的 `StrBlobPtr` 类没有定义拷贝构造函数、赋值运算符以及析构函数，为什么？

解：

因为使用合成的足够了。

## 练习14.32

> 定义一个类令其含有指向 `StrBlobPtr` 对象的指针，为这个类定义重载的箭头运算符。

解：

头文件：

```cpp
class StrBlobPtr;

class StrBlobPtr_pointer
{
public:
    StrBlobPtr_pointer() = default;
    StrBlobPtr_pointer(StrBlobPtr* p) : pointer(p) { }

    StrBlobPtr& operator *() const;
    StrBlobPtr* operator->() const;

private:
    StrBlobPtr* pointer = nullptr;
};
```

实现：

```cpp
#include "ex_14_32.h"
#include "ex_14_30_StrBlob.h"
#include <iostream>

StrBlobPtr&
StrBlobPtr_pointer::operator *() const
{
    return *pointer;
}

StrBlobPtr*
StrBlobPtr_pointer::operator ->() const
{
    return pointer;
}
```

## 练习14.33

> 一个重载的函数调用运算符应该接受几个运算对象？

解：

一个重载的函数调用运算符接受的运算对象应该和该运算符拥有的操作数一样多。

## 练习14.34

> 定义一个函数对象类，令其执行`if-then-else` 的操作：该类的调用运算符接受三个形参，它首先检查第一个形参，如果成功返回第二个形参值；如果不成功返回第三个形参的值。

解：

```cpp
struct Test
{
    int operator()(bool b, int iA, int iB) 
    {
        return b ? iA : iB;
    }
};
```

## 练习14.35

> 编写一个类似于 `PrintString` 的类，令其从 `istream` 中读取一行输入，然后返回一个表示我们所读内容的`string`。如果读取失败，返回空`string`。

解：

```cpp
#include <iostream>
#include <string>

class GetInput
{
public:
	GetInput(std::istream &i = std::cin) : is(i) {}
	std::string operator()() const
	{
		std::string str;
		std::getline(is, str);
		return is ? str : std::string();
	}

private:
	std::istream &is;
};

int main()
{
	GetInput getInput;
	std::cout << getInput() << std::endl;
}
```

## 练习14.36

> 使用前一个练习定义的类读取标准输入，将每一行保存为 `vector` 的一个元素。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>

class GetInput
{
public:
	GetInput(std::istream &i = std::cin) : is(i) {}
	std::string operator()() const
	{
		std::string str;
		std::getline(is, str);
		return is ? str : std::string();
	}

private:
	std::istream &is;
};

int main()
{
	GetInput getInput;
	std::vector<std::string> vec;
	for (std::string tmp; !(tmp = getInput()).empty();) vec.push_back(tmp);
	for (const auto &str : vec) std::cout << str << " ";
	std::cout << std::endl;
}
```

## 练习14.37

> 编写一个类令其检查两个值是否相等。使用该对象及标准库算法编写程序，令其替换某个序列中具有给定值的所有实例。

解：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

class IsEqual
{
	int value;
public:
	IsEqual(int v) : value(v) {}
	bool operator()(int elem)
	{
		return elem == value;
	}
};

int main()
{
	std::vector<int> vec = { 3, 2, 1, 4, 3, 7, 8, 6 };
	std::replace_if(vec.begin(), vec.end(), IsEqual(3), 5);
	for (int i : vec) std::cout << i << " ";
	std::cout << std::endl;
}
```

## 练习14.38

> 编写一个类令其检查某个给定的 `string` 对象的长度是否与一个阈值相等。使用该对象编写程序，统计并报告在输入的文件中长度为1的单词有多少个，长度为2的单词有多少个、......、长度为10的单词有多少个。

解：

```cpp
#include <iostream>
#include <string>
#include <fstream>
#include <vector>
#include <map>

struct IsInRange
{
	IsInRange(std::size_t lower, std::size_t upper)
	:_lower(lower), _upper(upper)
	{}

	bool operator()(std::string const& str) const
	{
		return str.size() >= _lower && str.size() <= _upper;
	}

	std::size_t lower_limit() const
	{
		return _lower;
	}

	std::size_t upper_limit() const
	{
		return _upper;
	}
private:
	std::size_t _lower;
	std::size_t _upper;
};

int main()
{
	//create predicates with various upper limits.
	std::size_t lower = 1;
	auto uppers = { 3u, 4u, 5u, 6u, 7u, 8u, 9u, 10u, 11u, 12u, 13u, 14u };
	std::vector<IsInRange> predicates;
	for (auto upper : uppers)
		predicates.push_back(IsInRange{ lower, upper });

	//create count_table to store counts 
	std::map<std::size_t, std::size_t> count_table;
	for (auto upper : uppers)
		count_table[upper] = 0;

	//read file and count
	std::ifstream fin("storyDataFile.txt");
	for (std::string word; fin >> word; /* */)
	for (auto is_size_in_range : predicates)
	if (is_size_in_range(word))
		++count_table[is_size_in_range.upper_limit()];

	//print
	for (auto pair : count_table)
		std::cout << "count in range [1, " << pair.first << "] : " << pair.second << std::endl;

	return 0;
}
```

## 练习14.39

> 修改上一题的程序令其报告长度在1到9之间的单词有多少个、长度在10以上的单词有多少个。

解：

参考14.38。

## 练习14.40

> 重新编写10.3.2节的`biggies` 函数，使用函数对象替换其中的 `lambda` 表达式。

解：

```cpp
#include <vector>
#include <string>
#include <iostream>
#include <algorithm>

using namespace std;

class ShorterString
{
public:
	bool operator()(string const& s1, string const& s2) const { return s1.size() < s2.size(); }
};

class BiggerEqual
{
	size_t sz_;
public:
	BiggerEqual(size_t sz) : sz_(sz) {}
	bool operator()(string const& s) { return s.size() >= sz_; }
};

class Print
{
public:
	void operator()(string const& s) { cout << s << " "; }
};

string make_plural(size_t ctr, string const& word, string const& ending)
{
	return (ctr > 1) ? word + ending : word;
}

void elimDups(vector<string> &words)
{
	sort(words.begin(), words.end());
	auto end_unique = unique(words.begin(), words.end());
	words.erase(end_unique, words.end());
}

void biggies(vector<string> &words, vector<string>::size_type sz)
{
	elimDups(words);
	stable_sort(words.begin(), words.end(), ShorterString());
	auto wc = find_if(words.begin(), words.end(), BiggerEqual(sz));
	auto count = words.end() - wc;
	cout << count << " " << make_plural(count, "word", "s") << " of length " << sz << " or longer" << endl;
	for_each(wc, words.end(), Print());
	cout << endl;
}

int main()
{
	vector<string> vec{ "fox", "jumps", "over", "quick", "red", "red", "slow", "the", "turtle" };
	biggies(vec, 4);
}
```

## 练习14.41

> 你认为 C++ 11 标准为什么要增加 `lambda`？对于你自己来说，什么情况下会使用 `lambda`，什么情况下会使用类？

解：

使用 `lambda` 是非常方便的，当需要使用一个函数，而这个函数不常使用并且简单时，使用`lambda` 是比较方便的选择。

## 练习14.42

> 使用标准库函数对象及适配器定义一条表达式，令其

```
(a) 统计大于1024的值有多少个。 
(b) 找到第一个不等于pooh的字符串。
(c) 将所有的值乘以2。
```

解：

```cpp
std::count_if(ivec.cbegin(), ivec.cend(), std::bind(std::greater<int>(), _1, 1024));
std::find_if(svec.cbegin(), svec.cend(), std::bind(std::not_equal_to<std::string>(), _1, "pooh"));
std::transform(ivec.begin(), ivec.end(), ivec.begin(), std::bind(std::multiplies<int>(), _1, 2));
```

## 练习14.43

> 使用标准库函数对象判断一个给定的`int`值是否能被 `int` 容器中的所有元素整除。

解：

```cpp
#include <iostream>
#include <string>
#include <functional>
#include <algorithm>

int main()
{
	auto data = { 2, 3, 4, 5 };
	int input;
	std::cin >> input;
	std::modulus<int> mod;
	auto predicator = [&](int i) { return 0 == mod(input, i); };
	auto is_divisible = std::any_of(data.begin(), data.end(), predicator);
	std::cout << (is_divisible ? "Yes!" : "No!") << std::endl;

	return 0;
}
```

## 练习14.44

> 编写一个简单的桌面计算器使其能处理二元运算。

解：

```cpp
#include <iostream>
#include <string>
#include <map> 
#include <functional> 

int add(int i, int j) { return i + j; }
auto mod = [](int i, int j) { return i % j; };
struct Div { int operator ()(int i, int j) const { return i / j; } };

auto binops = std::map<std::string, std::function<int(int, int)>>
{
	{ "+", add },                               // function pointer 
	{ "-", std::minus<int>() },                 // library functor 
	{ "/", Div() },                             // user-defined functor 
	{ "*", [](int i, int j) { return i*j; } },  // unnamed lambda 
	{ "%", mod }                                // named lambda object 
};


int main()
{
	while (std::cout << "Pls enter as: num operator num :\n", true)
	{
		int lhs, rhs; std::string op;
		std::cin >> lhs >> op >> rhs;
		std::cout << binops[op](lhs, rhs) << std::endl;
	}

	return 0;
}
```

## 练习14.45

> 编写类型转换运算符将一个 `Sales_data` 对象分别转换成 `string` 和 `double`，你认为这些运算符的返回值应该是什么？

解：

头文件：

```cpp
#include <string>
#include <iostream>

class Sales_data
{
	friend std::istream& operator>>(std::istream&, Sales_data&);
	friend std::ostream& operator<<(std::ostream&, const Sales_data&);
	friend Sales_data operator+(const Sales_data&, const Sales_data&);

public:
	Sales_data(const std::string &s, unsigned n, double p) :bookNo(s), units_sold(n), revenue(n*p) {}
	Sales_data() : Sales_data("", 0, 0.0f) {}
	Sales_data(const std::string &s) : Sales_data(s, 0, 0.0f) {}
	Sales_data(std::istream &is);

	Sales_data& operator=(const std::string&);
	Sales_data& operator+=(const Sales_data&);
	explicit operator std::string() const { return bookNo; }
	explicit operator double() const { return avg_price(); }

	std::string isbn() const { return bookNo; }

private:
	inline double avg_price() const;

	std::string bookNo;
	unsigned units_sold = 0;
	double revenue = 0.0;
};

std::istream& operator>>(std::istream&, Sales_data&);
std::ostream& operator<<(std::ostream&, const Sales_data&);
Sales_data operator+(const Sales_data&, const Sales_data&);

inline double Sales_data::avg_price() const
{
	return units_sold ? revenue / units_sold : 0;
}
```

## 练习14.46

> 你认为应该为 `Sales_data` 类定义上面两种类型转换运算符吗？应该把它们声明成 `explicit` 的吗？为什么？

解：

上面的两种类型转换有歧义，应该声明成 `explicit` 的。

## 练习14.47

> 说明下面这两个类型转换运算符的区别。

```cpp
struct Integral {
	operator const int();
	operator int() const;
}
```

解：

第一个无意义，会被编译器忽略。第二个合法。

## 练习14.48

> 你在7.5.1节的练习7.40中曾经选择并编写了一个类，你认为它应该含有向 `bool` 的类型转换运算符吗？如果是，解释原因并说明该运算符是否应该是 `explicit`的；如果不是，也请解释原因。

解：

`Date` 类应该含有向 `bool` 的类型转换运算符，并且应该声明为 `explicit` 的。

## 练习14.49

> 为上一题提到的类定义一个转换目标是 `bool` 的类型转换运算符，先不用在意这么做是否应该。

解：

```cpp
 explicit operator bool() { return (year<4000) ? true : false; }
```

## 练习14.50

> 在初始化 `ex1` 和 `ex2` 的过程中，可能用到哪些类类型的转换序列呢？说明初始化是否正确并解释原因。

```cpp
struct LongDouble {
	LongDouble(double = 0.0);
	operator double();
	operator float();
};
LongDouble ldObj;
int ex1 = ldObj;
float ex2 = ldObj;
```

解：

`ex1` 转换不合法，没有定义从 `LongDouble` 到 `int` 的转换，从`double`转换还是`float`转换存在二义性。`ex2` 合法。

## 练习14.51

> 在调用 `calc` 的过程中，可能用到哪些类型转换序列呢？说明最佳可行函数是如何被选出来的。

```cpp
void calc(int);
void calc(LongDouble);

double dval;
calc(dval);  // 调用了哪个calc？
```

解：

最佳可行函数是 `void calc(int)`。

转换的优先级如下：

1. 精确匹配
2. `const` 转换。
3. 类型提升
4. 算术转换
5. 类类型转换

## 练习14.52

> 在下面的加法表达式中分别选用了哪个`operator+`？列出候选函数、可行函数及为每个可行函数的实参执行的类型转换：

```cpp
struct Longdouble {
	//用于演示的成员operator+;在通常情况下是个非成员
	LongDouble operator+(const SmallInt&);
	//其他成员与14.9.2节一致
};
LongDouble operator+(LongDouble&, double);
SmallInt si;
LongDouble ld;
ld = si + ld;
ld = ld + si;
```

解：

`ld = si + ld;` 不合法。`ld = ld + si` 两个都可以调用，但是第一个调用更精确一些。

## 练习14.53

> 假设我们已经定义了如第522页所示的`SmallInt`，判断下面的加法表达式是否合法。如果合法，使用了哪个加法运算符？如果不合法，应该怎样修改代码才能使其合法？

```cpp
SmallInt si;
double d = si + 3.14;
```

解：

不合法，存在二义性。

应该该为：

```cpp
SmallInt s1;
double d = s1 + SmallInt(3.14);
```

# 第十五章 面向对象程序设计

## 练习15.1

> 什么是虚成员？

解：

对于某些函数，基类希望它的派生类各自定义适合自身的版本，此时基类就将这些函数声明成虚函数。

## 练习15.2

> `protected` 访问说明符与 `private` 有何区别？

解：

* `protected` ： 基类和和其派生类还有友元可以访问。
* `private` ： 只有基类本身和友元可以访问。

## 练习15.3

> 定义你自己的 `Quote` 类和 `print_total` 函数。

解：

`Quote`:

```cpp
#include <string>

class Quote
{
public:
	Quote() = default;
	Quote(const std::string &b, double p) :
		bookNo(b), price(p){}
	std::string isbn() const { return bookNo; }
	virtual double net_price(std::size_t n) const { return n * price; }

	virtual ~Quote() = default;

private:
	std::string bookNo;

protected:
	double  price = 0.0;

};
```

主函数：

```cpp
#include "ex_15_3.h"
#include <iostream>
#include <string>
#include <map>
#include <functional>

double print_total(std::ostream& os, const Quote& item, size_t n);

int main()
{
	return 0;
}


double print_total(std::ostream &os, const Quote &item, size_t n)
{
	double ret = item.net_price(n);

	os << "ISBN:" << item.isbn()
		<< "# sold: " << n << " total due: " << ret << std::endl;

	return ret;
}

```

## 练习15.4

> 下面哪条声明语句是不正确的？请解释原因。

```cpp
class Base { ... };
(a) class Derived : public Derived { ... };
(b) class Derived : private Base { ... };
(c) class Derived : public Base;
```

解：

* (a) 不正确。类不能派生自身。
* (b) 不正确。这是定义而非声明。
* (c) 不正确。派生列表不能出现在这。

## 练习15.5

> 定义你自己的 `Bulk_quote` 类。

解：

```cpp
#include "ex_15_3.h"

class Bulk_quote : public Quote
{
public:
	Bulk_quote() = default;
	Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :
		Quote(b, p), min_qty(q), discount(disc) {}

	double net_price(std::size_t n) const override;

private:
	std::size_t min_qty = 0;
	double      discount = 0.0;
};
```

## 练习15.6

> 将 `Quote` 和 `Bulk_quote` 的对象传给15.2.1节练习中的 `print_total` 函数，检查该函数是否正确。

解：

```cpp
#include "ex_15_3.h"
#include "ex_15_5.h"

#include <iostream>
#include <string>

double print_total(std::ostream& os, const Quote& item, size_t n);

int main()
{
	// ex15.6
	Quote q("textbook", 10.60);
	Bulk_quote bq("textbook", 10.60, 10, 0.3);

	print_total(std::cout, q, 12);
	print_total(std::cout, bq, 12);

	return 0;
}

double print_total(std::ostream &os, const Quote &item, size_t n)
{
	double ret = item.net_price(n);

	os << "ISBN:" << item.isbn()
		<< "# sold: " << n << " total due: " << ret << std::endl;

	return ret;
}
```

## 练习15.7

> 定义一个类使其实现一种数量受限的折扣策略，具体策略是：当购买书籍的数量不超过一个给定的限量时享受折扣，如果购买量一旦超过了限量，则超出的部分将以原价销售。

解：

```cpp
#include "ex_15_5.h"

class Limit_quote : public Quote
{
public:
	Limit_quote();
	Limit_quote(const std::string& b, double p, std::size_t max, double disc) :
		Quote(b, p), max_qty(max), discount(disc)
	{}

	double net_price(std::size_t n) const override;

private:
	std::size_t max_qty = 0;
	double      discount = 0.0;
};

double Limit_quote::net_price(std::size_t n) const
{
	if (n > max_qty)
		return max_qty * price * discount + (n - max_qty) * price;
	else
		return n * discount *price;
}
```

## 练习15.8

> 给出静态类型和动态类型的定义。

解：

表达式的静态类型在编译时总是已知的，它是变量声明时的类型或表达式生成的类型。动态类型则是变量或表达式表示的内存中的对象的类型。动态类型直到运行时才可知。

## 练习15.9

> 在什么情况下表达式的静态类型可能与动态类型不同？请给出三个静态类型与动态类型不同的例子。

解：

基类的指针或引用的静态类型可能与其动态类型不一致。

## 练习15.10

> 回忆我们在8.1节进行的讨论，解释第284页中将 `ifstream` 传递给 `Sales_data` 的`read` 函数的程序是如何工作的。

解：

`std::ifstream` 是 `std::istream` 的派生基类，因此 `read` 函数能够正常工作。

## 练习15.11

> 为你的 `Quote` 类体系添加一个名为 `debug` 的虚函数，令其分别显示每个类的数据成员。

解：

```cpp
void Quote::debug() const
{
    std::cout << "data members of this class:\n"
              << "bookNo= " <<this->bookNo << " "
              << "price= " <<this->price<< " ";
}
```

## 练习15.12

> 有必要将一个成员函数同时声明成 `override` 和 `final` 吗？为什么？

解：

有必要。`override` 的含义是重写基类中相同名称的虚函数，`final` 是阻止它的派生类重写当前虚函数。

## 练习15.13

> 给定下面的类，解释每个 `print` 函数的机理：

```cpp
class base {
public:
	string name() { return basename;}
	virtual void print(ostream &os) { os << basename; }
private:
	string basename;
};
class derived : public base {
public:
	void print(ostream &os) { print(os); os << " " << i; }
private:
	int i;
};
```
在上述代码中存在问题吗？如果有，你该如何修改它？

解：

有问题。应该改为：

```cpp
	void print(ostream &os) override { base::print(os); os << " derived\n " << i; }
```

## 练习15.14

> 给定上一题中的类以及下面这些对象，说明在运行时调用哪个函数：

```cpp
base bobj; 		base *bp1 = &bobj; 	base &br1 = bobj;
derived dobj; 	base *bp2 = &dobj; 	base &br2 = dobj;
(a) bobj.print();	(b)dobj.print();	(c)bp1->name();
(d)bp2->name();		(e)br1.print();		(f)br2.print();
```

解：

* (a) 编译时。
* (b) 编译时。
* (c) 编译时。
* (d) 编译时。
* (e) 运行时。`base::print()`
* (f) 运行时。`derived::print()`

## 练习15.15

> 定义你自己的 `Disc_quote` 和 `Bulk_quote`。

解：

`Disc_quote`:

```cpp
#include "quote.h"
class Disc_quote : public Quote
{
public:
    Disc_quote();
    Disc_quote(const std::string& b, double p, std::size_t q, double d) :
        Quote(b, p), quantity(q), discount(d)   { }

    virtual double net_price(std::size_t n) const override = 0;

protected:
    std::size_t quantity;
    double      discount;
};

```

`Bulk_quote`:

```cpp
#include "disc_quote.h"

class Bulk_quote : public Disc_quote
{
public:
    Bulk_quote() = default;
    Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :
        Disc_quote(b, p, q, disc) {   }

    double net_price(std::size_t n) const override;
    void  debug() const override;
};
```

## 练习15.16

> 改写你在15.2.2节练习中编写的数量受限的折扣策略，令其继承 `Disc_quote`。

解：

`Limit_quote`：

```cpp
#include "disc_quote.h"

class Limit_quote : public Disc_quote
{
public:
    Limit_quote() = default;
    Limit_quote(const std::string& b, double p, std::size_t max, double disc):
        Disc_quote(b, p, max, disc)  {   }

    double net_price(std::size_t n) const override
    { return n * price * (n < quantity ? 1 - discount : 1 ); }

    void debug() const override;
};
```

## 练习15.17

> 尝试定义一个 `Disc_quote` 的对象，看看编译器给出的错误信息是什么？

解：

`error: cannot declare variable 'd' to be of abstract type 'Disc_quote': Disc_quote d;`

## 练习15.18

> 假设给定了第543页和第544页的类，同时已知每个对象的类型如注释所示，判断下面的哪些赋值语句是合法的。解释那些不合法的语句为什么不被允许：

```cpp
Base *p = &d1;  //d1 的类型是 Pub_Derv
p = &d2;		//d2 的类型是 Priv_Derv
p = &d3;		//d3 的类型是 Prot_Derv
p = &dd1;		//dd1 的类型是 Derived_from_Public	
p = &dd2;		//dd2 的类型是 Derived_from_Private
p = &dd3;		//dd3 的类型是 Derived_from_Protected
```

解：

```cpp
Base *p = &d1; 合法
p = &d2; 不合法
p = &d3; 不合法
p = &dd1; 合法
p = &dd2; 不合法
p = &dd3; 不合法
```

只有在派生类是使用`public`的方式继承基类时，用户代码才可以使用派生类到基类（`derived-to-base`）的转换。

## 练习15.19

> 假设543页和544页的每个类都有如下形式的成员函数：

```cpp
void memfcn(Base &b) { b = *this; }
```

对于每个类，分别判断上面的函数是否合法。

解：

合法：
* Pub_Derv
* Priv_Derv
* Prot_Derv
* Derived_from_Public
* Derived_from_Protected
不合法：
* Derived_from_Private

这段代码是在成员函数中使用`Base`。`Priv_Drev`中的`Base`部分虽然是`private`的，但其成员函数依然可以访问；`Derived_from_Private`继承自`Priv_Drev`，不能访问`Priv_Drev`中的`private`成员，因此不合法。

## 练习15.20

> 编写代码检验你对前面两题的回答是否正确。

解：

```cpp
#include <iostream>
#include <string>

#include "exercise15_5.h"
#include "bulk_quote.h"
#include "limit_quote.h"
#include "disc_quote.h"

class Base
{
public:
	void pub_mem();   // public member
protected:
	int prot_mem;     // protected member
private:
	char priv_mem;    // private member
};

struct Pub_Derv : public    Base
{
	void memfcn(Base &b) { b = *this; }
};
struct Priv_Derv : private   Base
{
	void memfcn(Base &b) { b = *this; }
};
struct Prot_Derv : protected Base
{
	void memfcn(Base &b) { b = *this; }
};

struct Derived_from_Public : public Pub_Derv
{
	void memfcn(Base &b) { b = *this; }
};
struct Derived_from_Private : public Priv_Derv
{
	//void memfcn(Base &b) { b = *this; }
};
struct Derived_from_Protected : public Prot_Derv
{
	void memfcn(Base &b) { b = *this; }
};

int main()
{
	Pub_Derv d1;
	Base *p = &d1;

	Priv_Derv d2;
	//p = &d2;

	Prot_Derv d3;
	//p = &d3;

	Derived_from_Public dd1;
	p = &dd1;

	Derived_from_Private dd2;
	//p =& dd2;

	Derived_from_Protected dd3;
	//p = &dd3;


	return 0;
}
```

## 练习15.21

> 从下面这些一般性抽象概念中任选一个（或者选一个你自己的），将其对应的一组类型组织成一个继承体系：

```cpp
(a) 图形文件格式（如gif、tiff、jpeg、bmp）
(b) 图形基元（如方格、圆、球、圆锥）
(c) C++语言中的类型（如类、函数、成员函数）
```

解：

```cpp
#include <iostream>
#include <string>

#include "quote.h"
#include "bulk_quote.h"
#include "limit_quote.h"
#include "disc_quote.h"

// just for 2D shape
class Shape
{
public:
    typedef std::pair<double, double>    Coordinate;

    Shape() = default;
    Shape(const std::string& n) :
        name(n) { }

    virtual double area()       const = 0;
    virtual double perimeter()  const = 0;

    virtual ~Shape() = default;
private:
    std::string name;
};

class Rectangle : public Shape
{
public:
    Rectangle() = default;
    Rectangle(const std::string& n,
              const Coordinate& a,
              const Coordinate& b,
              const Coordinate& c,
              const Coordinate& d) :
        Shape(n), a(a), b(b), c(c), d(d) { }

    ~Rectangle() = default;

protected:
    Coordinate  a;
    Coordinate  b;
    Coordinate  c;
    Coordinate  d;
};

class Square : public Rectangle
{
public:
    Square() = default;
    Square(const std::string& n,
           const Coordinate& a,
           const Coordinate& b,
           const Coordinate& c,
           const Coordinate& d) :
        Rectangle(n, a, b, c, d) { }

    ~Square() = default;
};


int main()
{
    return 0;
}
```

## 练习15.22

> 对于你在上一题中选择的类，为其添加函数的虚函数及公有成员和受保护的成员。

解：

参考15.21。

## 练习15.23

> 假设第550页的 `D1` 类需要覆盖它继承而来的 `fcn` 函数，你应该如何对其进行修改？如果你修改之后 `fcn` 匹配了 `Base` 中的定义，则该节的那些调用语句将如何解析？

解：

移除 `int` 参数。

## 练习15.24

> 哪种类需要虚析构函数？虚析构函数必须执行什么样的操作？

解：

基类通常应该定义一个虚析构函数。

## 练习15.25

> 我们为什么为 `Disc_quote` 定义一个默认构造函数？如果去掉该构造函数的话会对 `Bulk_quote` 的行为产生什么影响？

解：

因为`Disc_quote`的默认构造函数会运行`Quote`的默认构造函数，而`Quote`默认构造函数会完成成员的初始化工作。
如果去除掉该构造函数的话，`Bulk_quote`的默认构造函数而无法完成`Disc_quote`的初始化工作。

## 练习15.26

> 定义 `Quote` 和 `Bulk_quote` 的拷贝控制成员，令其与合成的版本行为一致。为这些成员以及其他构造函数添加打印状态的语句，使得我们能够知道正在运行哪个程序。使用这些类编写程序，预测程序将创建和销毁哪些对象。重复实验，不断比较你的预测和实际输出结果是否相同，直到预测完全准确再结束。

解：

`Quote`:

```cpp
#include <string>
#include <iostream>

class Quote
{
	friend bool operator !=(const Quote& lhs, const Quote& rhs);
public:
	Quote() { std::cout << "default constructing Quote\n"; }
	Quote(const std::string &b, double p) :
		bookNo(b), price(p)
	{
		std::cout << "Quote : constructor taking 2 parameters\n";
	}

	// copy constructor
	Quote(const Quote& q) : bookNo(q.bookNo), price(q.price)
	{
		std::cout << "Quote: copy constructing\n";
	}

	// move constructor
	Quote(Quote&& q) noexcept : bookNo(std::move(q.bookNo)), price(std::move(q.price))
	{ std::cout << "Quote: move constructing\n"; }

	// copy =
	Quote& operator =(const Quote& rhs)
	{
		if (*this != rhs)
		{
			bookNo = rhs.bookNo;
			price = rhs.price;
		}
		std::cout << "Quote: copy =() \n";

		return *this;
	}

	// move =
	Quote& operator =(Quote&& rhs)  noexcept
	{
		if (*this != rhs)
		{
			bookNo = std::move(rhs.bookNo);
			price = std::move(rhs.price);
		}
		std::cout << "Quote: move =!!!!!!!!! \n";

		return *this;
	}

	std::string     isbn() const { return bookNo; }
	virtual double  net_price(std::size_t n) const { return n * price; }
	virtual void    debug() const;

	virtual ~Quote()
	{
		std::cout << "destructing Quote\n";
	}

private:
	std::string bookNo;

protected:
	double  price = 10.0;
};

bool inline
operator !=(const Quote& lhs, const Quote& rhs)
{
	return lhs.bookNo != rhs.bookNo
		&&
		lhs.price != rhs.price;
}
```

`Bulk_quote`:

```cpp
#include "Disc_quote.h"
#include <iostream>

class Bulk_quote : public Disc_quote
{

public:
	Bulk_quote() { std::cout << "default constructing Bulk_quote\n"; }
	Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :
		Disc_quote(b, p, q, disc)
	{
		std::cout << "Bulk_quote : constructor taking 4 parameters\n";
	}

	// copy constructor
	Bulk_quote(const Bulk_quote& bq) : Disc_quote(bq)
	{
		std::cout << "Bulk_quote : copy constructor\n";
	}

	// move constructor
	Bulk_quote(Bulk_quote&& bq) : Disc_quote(std::move(bq)) noexcept
	{
		std::cout << "Bulk_quote : move constructor\n";
	}

	// copy =()
	Bulk_quote& operator =(const Bulk_quote& rhs)
	{
		Disc_quote::operator =(rhs);
		std::cout << "Bulk_quote : copy =()\n";

		return *this;
	}


	// move =()
	Bulk_quote& operator =(Bulk_quote&& rhs) noexcept
	{
		Disc_quote::operator =(std::move(rhs));
		std::cout << "Bulk_quote : move =()\n";

		return *this;
	}

	double net_price(std::size_t n) const override;
	void  debug() const override;

	~Bulk_quote() override
	{
		std::cout << "destructing Bulk_quote\n";
	}
};
```

程序输出结果：

```
default constructing Quote
default constructing Disc_quote
default constructing Bulk_quote
Quote : constructor taking 2 parameters
Disc_quote : constructor taking 4 parameters.
Bulk_quote : constructor taking 4 parameters
Quote: copy constructing
Quote: copy constructing
destructing Quote
destructing Quote
Disc_quote : move =()
Bulk_quote : move =()
destructing Bulk_quote
destructing Dis_quote
destructing Quote
destructing Bulk_quote
destructing Dis_quote
destructing Quote
```

## 练习15.27

> 重新定义你的 `Bulk_quote` 类，令其继承构造函数。

解：

```cpp
#include "disc_quote.h"
#include <iostream>

class Bulk_quote : public Disc_quote
{

public:
	Bulk_quote() { std::cout << "default constructing Bulk_quote\n"; }

	// changed the below to the inherited constructor for ex15.27.
	// rules:  1. only inherit from the direct base class.
	//         2. default, copy and move constructors can not inherit.
	//         3. any data members of its own are default initialized.
	//         4. the rest details are in the section section 15.7.4.
	/*
	Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :
	Disc_quote(b, p, q, disc) { std::cout << "Bulk_quote : constructor taking 4 parameters\n"; }
	*/
	using Disc_quote::Disc_quote;

	// copy constructor
	Bulk_quote(const Bulk_quote& bq) : Disc_quote(bq)
	{
		std::cout << "Bulk_quote : copy constructor\n";
	}

	// move constructor
	Bulk_quote(Bulk_quote&& bq) : Disc_quote(std::move(bq))
	{
		std::cout << "Bulk_quote : move constructor\n";
	}

	// copy =()
	Bulk_quote& operator =(const Bulk_quote& rhs)
	{
		Disc_quote::operator =(rhs);
		std::cout << "Bulk_quote : copy =()\n";

		return *this;
	}


	// move =()
	Bulk_quote& operator =(Bulk_quote&& rhs)
	{
		Disc_quote::operator =(std::move(rhs));
		std::cout << "Bulk_quote : move =()\n";

		return *this;
	}

	double net_price(std::size_t n) const override;
	void  debug() const override;

	~Bulk_quote() override
	{
		std::cout << "destructing Bulk_quote\n";
	}
};
```

## 练习15.28

> 定义一个存放 `Quote` 对象的 `vector`，将 `Bulk_quote` 对象传入其中。计算 `vector` 中所有元素总的 `net_price`。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <memory>

#include "quote.h"
#include "bulk_quote.h"
#include "limit_quote.h"
#include "disc_quote.h"


int main()
{
	/**
	* @brief ex15.28   outcome == 9090
	*/
	std::vector<Quote> v;
	for (unsigned i = 1; i != 10; ++i)
		v.push_back(Bulk_quote("sss", i * 10.1, 10, 0.3));

	double total = 0;
	for (const auto& b : v)
	{
		total += b.net_price(20);
	}
	std::cout << total << std::endl;

	std::cout << "======================\n\n";

	/**
	* @brief ex15.29   outccome == 6363
	*/
	std::vector<std::shared_ptr<Quote>> pv;

	for (unsigned i = 1; i != 10; ++i)
		pv.push_back(std::make_shared<Bulk_quote>(Bulk_quote("sss", i * 10.1, 10, 0.3)));

	double total_p = 0;
	for (auto p : pv)
	{
		total_p += p->net_price(20);
	}
	std::cout << total_p << std::endl;

	return 0;

}
```

## 练习15.29

> 再运行一次你的程序，这次传入 `Quote` 对象的 `shared_ptr` 。如果这次计算出的总额与之前的不一致，解释为什么;如果一直，也请说明原因。

解：

因为智能指针导致了多态性的产生，所以这次计算的总额不一致。

## 练习15.30

> 编写你自己的 `Basket` 类，用它计算上一个练习中交易记录的总价格。

解：

`Basket h`:

```cpp
#include "quote.h"
#include <set>
#include <memory>

// 购物篮
// a basket of objects from Quote hierachy, using smart pointers.
class Basket
{
public:
	// Basket使用合成的默认构造函数和拷贝控制成员
	// copy verison
	void add_item(const Quote& sale)
	{
		items.insert(std::shared_ptr<Quote>(sale.clone()));
	}
	// move version
	void add_item(Quote&& sale)
	{
		items.insert(std::shared_ptr<Quote>(std::move(sale).clone()));
	}

	// 打印每本书的总价和购物篮中所有书的总价
	double total_receipt(std::ostream& os) const;

private:

	// function to compare needed by the multiset member
	static bool compare(const std::shared_ptr<Quote>& lhs,
		const std::shared_ptr<Quote>& rhs)
	{
		return lhs->isbn() < rhs->isbn();
	}

	// hold multiple quotes, ordered by the compare member
	std::multiset<std::shared_ptr<Quote>, decltype(compare)*>
		items{ compare };
};
```

`Basket cpp`:

```cpp
#include "basket.h"
double Basket::total_receipt(std::ostream &os) const
{
	double sum = 0.0;  // 保存实时计算出的总价格

	// iter指向ISBN相同的一批元素中的第一个
	// upper_bound返回一个迭代器，该迭代器指向这批元素的尾后位置
	for (auto iter = items.cbegin(); iter != items.cend();
		iter = items.upper_bound(*iter))
		//  ^^^^^^^^^^^^^^^^^^^^^^^^^^^
		// @note   this increment moves iter to the first element with key
		//         greater than  *iter.

	{
		sum += print_total(os, **iter, items.count(*iter));
	}                                   // ^^^^^^^^^^^^^ using count to fetch
	// the number of the same book.
	os << "Total Sale: " << sum << std::endl;
	return sum;
}
```

`main`:

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <memory>
#include <fstream>

#include "quote.h"
#include "bulk_quote.h"
#include "limit_quote.h"
#include "disc_quote.h"
#include "basket.h"


int main()
{
	Basket basket;

	for (unsigned i = 0; i != 10; ++i)
		basket.add_item(Bulk_quote("Bible", 20.6, 20, 0.3));

	for (unsigned i = 0; i != 10; ++i)
		basket.add_item(Bulk_quote("C++Primer", 30.9, 5, 0.4));

	for (unsigned i = 0; i != 10; ++i)
		basket.add_item(Quote("CLRS", 40.1));

	std::ofstream log("log.txt", std::ios_base::app | std::ios_base::out);

	basket.total_receipt(log);
	return 0;
}
```

## 练习15.31

> 已知 `s1`、`s2`、`s3` 和 `s4` 都是 `string`，判断下面的表达式分别创建了什么样的对象：

```cpp
(a) Query(s1) | Query(s2) & ~Query(s3);
(b) Query(s1) | (Query(s2) & ~Query(s3));
(c) (Query(s1) & (Query(s2)) | (Query(s3) & Query(s4)));
```

解：

```cpp
(a) OrQuery, AndQuery, NotQuery, WordQuery
(b) OrQuery, AndQuery, NotQuery, WordQuery
(c) OrQuery, AndQuery, WordQuery
```

## 练习15.32

> 当一个 `Query` 类型的对象被拷贝、移动、赋值或销毁时，将分别发生什么？

解：

* **拷贝**：当被拷贝时，合成的拷贝构造函数被调用。它将拷贝两个数据成员至新的对象。而在这种情况下，数据成员是一个智能指针，当拷贝时，相应的智能指针指向相同的地址，计数器增加1.
* **移动**：当移动时，合成的移动构造函数被调用。它将移动数据成员至新的对象。这时新对象的智能指针将会指向原对象的地址，而原对象的智能指针为 `nullptr`，新对象的智能指针的引用计数为 1。
* **赋值**：合成的赋值运算符被调用，结果和拷贝的相同的。
* **销毁**：合成的析构函数被调用。对象的智能指针的引用计数递减，当引用计数为 0 时，对象被销毁。

## 练习15.33

> 当一个 `Query_base` 类型的对象被拷贝、移动赋值或销毁时，将分别发生什么？

解：

由合成的版本来控制。然而 `Query_base` 是一个抽象类，它的对象实际上是它的派生类对象。

## 练习15.34

> 针对图15.3构建的表达式：

```cpp
(a) 例举出在处理表达式的过程中执行的所有构造函数。
(b) 例举出 cout << q 所调用的 rep。
(c) 例举出 q.eval() 所调用的 eval。
```

解：

* **a**： `Query q = Query("fiery") & Query("bird") | Query("wind");`

1. `Query::Query(const std::string& s)` where s == "fiery","bird" and "wind"
2. `WordQuery::WordQuery(const std::string& s)` where s == "fiery","bird" and "wind"
3. `AndQuery::AndQuery(const Query& left, const Query& right);`
4. `BinaryQuery(const Query&l, const Query& r, std::string s);`
5. `Query::Query(std::shared_ptr<Query_base> query)` 2times
6. `OrQuery::OrQuery(const Query& left, const Query& right);`
7. `BinaryQuery(const Query&l, const Query& r, std::string s);`
8. `Query::Query(std::shared_ptr<Query_base> query)` 2times


* **b**：

1. `query.rep()` inside the operator <<().
2. `q->rep()` inside the member function rep().
3. `OrQuery::rep()` which is inherited from `BinaryQuery`.
4. `Query::rep()` for `lhs` and `rhs`:
for `rhs` which is a `WordQuery` : `WordQuery::rep()` where `query_word("wind")` is returned.For `lhs` which is an `AndQuery`.
5. `AndQuery::rep()` which is inherited from `BinaryQuery`.
6. `BinaryQuer::rep()`: for `rhs: WordQuery::rep()`   where query_word("fiery") is returned. For `lhs: WordQuery::rep()` where query_word("bird" ) is returned.


* **c**：

1. `q.eval()`
2. `q->rep()`: where q is a pointer to `OrQuary`.
3. `QueryResult eval(const TextQuery& )const override`: is called but this one has not been defined yet.

## 练习15.35

> 实现 `Query` 类和 `Query_base` 类，其中需要定义`rep` 而无须定义 `eval`。

解：

`Query`:

```cpp
#ifndef QUERY_H
#define QUERY_H

#include <iostream>
#include <string>
#include <memory>
#include "query_base.h"
#include "queryresult.h"
#include "textquery.h"
#include "wordquery.h"



/**
* @brief interface class to manage the Query_base inheritance hierachy
*/
class Query
{
	friend Query operator~(const Query&);
	friend Query operator|(const Query&, const Query&);
	friend Query operator&(const Query&, const Query&);
public:
	// build a new WordQuery
	Query(const std::string& s) : q(new WordQuery(s))
	{
		std::cout << "Query::Query(const std::string& s) where s=" + s + "\n";
	}

	// interface functions: call the corresponding Query_base operatopns
	QueryResult eval(const TextQuery& t) const
	{
		return q->eval(t);
	}
	std::string rep() const
	{
		std::cout << "Query::rep() \n";
		return q->rep();
	}

private:
	// constructor only for friends
	Query(std::shared_ptr<Query_base> query) :
		q(query)
	{
		std::cout << "Query::Query(std::shared_ptr<Query_base> query)\n";
	}
	std::shared_ptr<Query_base> q;
};

inline std::ostream&
operator << (std::ostream& os, const Query& query)
{
	// make a virtual call through its Query_base pointer to rep();
	return os << query.rep();
}

#endif // QUERY_H
```

`Query_base`:

```cpp
#ifndef QUERY_BASE_H
#define QUERY_BASE_H
#include "textquery.h"
#include "queryresult.h"

/**
* @brief abstract class acts as a base class for all concrete query types
*        all members are private.
*/
class Query_base
{
	friend class Query;
protected:
	using line_no = TextQuery::line_no; //  used in the eval function
	virtual ~Query_base() = default;

private:
	// returns QueryResult that matches this query
	virtual QueryResult eval(const TextQuery&) const = 0;

	// a string representation of this query
	virtual std::string rep() const = 0;
};

#endif // QUERY_BASE_H
```

## 练习15.36

> 在构造函数和 `rep` 成员中添加打印语句，运行你的代码以检验你对本节第一个练习中(a)、(b)两小题的回答是否正确。

解：

```cpp
Query q = Query("fiery") & Query("bird") | Query("wind");

WordQuery::WordQuery(wind)
Query::Query(const std::string& s) where s=wind
WordQuery::WordQuery(bird)
Query::Query(const std::string& s) where s=bird
WordQuery::WordQuery(fiery)
Query::Query(const std::string& s) where s=fiery
BinaryQuery::BinaryQuery()  where s=&
AndQuery::AndQuery()
Query::Query(std::shared_ptr<Query_base> query)
BinaryQuery::BinaryQuery()  where s=|
OrQuery::OrQuery
Query::Query(std::shared_ptr<Query_base> query)
Press <RETURN> to close this window...
```

```cpp
std::cout << q <<std::endl;

Query::rep()
BinaryQuery::rep()
Query::rep()
WodQuery::rep()
Query::rep()
BinaryQuery::rep()
Query::rep()
WodQuery::rep()
Query::rep()
WodQuery::rep()
((fiery & bird) | wind)
Press <RETURN> to close this window...
```

## 练习15.37

> 如果在派生类中含有 `shared_ptr<Query_base>` 类型的成员而非 `Query` 类型的成员，则你的类需要做出怎样的改变？

解：

参考15.35。

## 练习15.38

> 下面的声明合法吗？如果不合法，请解释原因;如果合法，请指出该声明的含义。

```cpp
BinaryQuery a = Query("fiery") & Query("bird");
AndQuery b = Query("fiery") & Query("bird");
OrQuery c = Query("fiery") & Query("bird");
```

解：

1. 不合法。因为 `BinaryQuery` 是抽象类。
2. 不合法。`&` 操作返回的是一个 `Query` 对象。
3. 不合法。`&` 操作返回的是一个 `Query` 对象。

## 练习15.39

> 实现 `Query` 类和　`Query_base` 类，求图15.3中表达式的值并打印相关信息，验证你的程序是否正确。

## 练习15.40

> 在 `OrQuery` 的 `eval` 函数中，如果 `rhs` 成员返回的是空集将发生什么？

解：

不会发生什么。代码如下：
```cpp
std::shared_ptr<std::set<line_no>> ret_lines =
       std::make_shared<std::set<line_no>>(left.begin(), left.end());
```
如果 `rhs` 成员返回的是空集，在 `set` 当中不会添加什么。

## 练习15.41

> 重新实现你的类，这次使用指向 `Query_base` 的内置指针而非 `shared_ptr`。请注意，做出上述改动后你的类将不能再使用合成的拷贝控制成员。

解：

略

## 练习15.42

> 从下面的几种改进中选择一种，设计并实现它:

```cpp
(a) 按句子查询并打印单词，而不再是按行打印。
(b) 引入一个历史系统，用户可以按编号查阅之前的某个查询，并可以在其中添加内容或者将其余其他查询组合。
(c) 允许用户对结果做出限制，比如从给定范围的行中跳出匹配的进行显示。
```

解：

略

## TextQuery最终项目

见 cpp_source/cha5/text_query

# 第16章 : 模版与泛型编程

## 练习16.1

> 给出实例化的定义。

解：

当编译器实例化一个模版时，它使用实际的模版参数代替对应的模版参数来创建出模版的一个新“实例”。

## 练习16.2

> 编写并测试你自己版本的 `compare` 函数。

解：

```cpp
template<typename T>
int compare(const T& lhs, const T& rhs)
{
	if (lhs < rhs) return -1;
	if (rhs < lhs) return 1;
	return 0;
}
```

## 练习16.3

> 对两个 `Sales_data` 对象调用你的 `compare` 函数，观察编译器在实例化过程中如何处理错误。

解：

`error: no match for 'operator<' `

## 练习16.4

> 编写行为类似标准库 `find` 算法的模版。函数需要两个模版类型参数，一个表示函数的迭代器参数，另一个表示值的类型。使用你的函数在一个 `vector<int>` 和一个`list<string>`中查找给定值。


解：

```cpp
template<typename Iterator, typename Value>
Iterator find(Iterator first, Iterator last, const Value& v)
{
	for ( ; first != last && *first != value; ++first);
	return first;
}
```

## 练习16.5

> 为6.2.4节中的`print`函数编写模版版本，它接受一个数组的引用，能处理任意大小、任意元素类型的数组。

解：

```cpp
template<typename Array>
void print(const Array& arr)
{
	for (const auto& elem : arr)
		std::cout << elem << std::endl;
}
```

## 练习16.6

> 你认为接受一个数组实参的标准库函数 `begin` 和 `end` 是如何工作的？定义你自己版本的 `begin` 和 `end`。

解：

```cpp
template<typename T, unsigned N>
T* begin(const T (&arr)[N])
{
	return arr;
}

template<typename T, unsigned N>
T* end(const T (&arr)[N])
{
	return arr + N;
}
```

## 练习16.7

> 编写一个 `constexpr` 模版，返回给定数组的大小。

解：

```cpp
template<typename T, typename N> constexpr
unsigned size(const T (&arr)[N])
{
	return N;
}
```

## 练习16.8

> 在第97页的“关键概念”中，我们注意到，C++程序员喜欢使用 `!=` 而不喜欢 `<` 。解释这个习惯的原因。

解：

因为大多数类只定义了 `!=` 操作而没有定义 `<` 操作，使用 `!=` 可以降低对要处理的类型的要求。

## 练习16.9

> 什么是函数模版，什么是类模版？

解：

一个函数模版就是一个公式，可用来生成针对特定类型的函数版本。类模版是用来生成类的蓝图的，与函数模版的不同之处是，编译器不能为类模版推断模版参数类型。如果我们已经多次看到，为了使用类模版，我们必须在模版名后的尖括号中提供额外信息。

## 练习16.10

> 当一个类模版被实例化时，会发生什么？

解：

一个类模版的每个实例都形成一个独立的类。

## 练习16.11

> 下面 `List` 的定义是错误的。应如何修改它？

```cpp
template <typename elemType> class ListItem;
template <typename elemType> class List {
public:
	List<elemType>();
	List<elemType>(const List<elemType> &);
	List<elemType>& operator=(const List<elemType> &);
	~List();
	void insert(ListItem *ptr, elemType value);
private:
	ListItem *front, *end;
};
```

解：

模版需要模版参数，应该修改为如下：
```cpp
template <typename elemType> class ListItem;  
template <typename elemType> class List{  
public:  
  	List<elemType>();  
  	List<elemType>(const List<elemType> &);  
  	List<elemType>& operator=(const List<elemType> &);  
  	~List();  
  	void insert(ListItem<elemType> *ptr, elemType value);  
private:  
  	ListItem<elemType> *front, *end;  
};
```	

## 练习16.12

> 编写你自己版本的 `Blob` 和 `BlobPtr` 模版，包含书中未定义的多个`const`成员。

解：

Blob：

```cpp
#include <memory>
#include <vector>

template<typename T> class Blob
{
public:
	typedef T value_type;
	typedef typename std::vector<T>::size_type size_type;

	// constructors
	Blob();
	Blob(std::initializer_list<T> il);

	// number of elements in the Blob
	size_type size() const { return data->size(); }
	bool      empty() const { return data->empty(); }

	void push_back(const T& t) { data->push_back(t); }
	void push_back(T&& t) { data->push_back(std::move(t)); }
	void pop_back();

	// element access
	T& back();
	T& operator[](size_type i);

	const T& back() const;
	const T& operator [](size_type i) const;

private:
	std::shared_ptr<std::vector<T>> data;
	// throw msg if data[i] isn't valid
	void check(size_type i, const std::string &msg) const;
};

// constructors
template<typename T>
Blob<T>::Blob() : data(std::make_shared<std::vector<T>>())
{}

template<typename T>
Blob<T>::Blob(std::initializer_list<T> il) :
data(std::make_shared<std::vector<T>>(il))
{}

template<typename T>
void Blob<T>::check(size_type i, const std::string &msg) const
{
	if (i >= data->size())
		throw std::out_of_range(msg);
}

template<typename T>
T& Blob<T>::back()
{
	check(0, "back on empty Blob");
	return data->back();
}

template<typename T>
const T& Blob<T>::back() const
{
	check(0, "back on empty Blob");
	return data->back();
}


template<typename T>
T& Blob<T>::operator [](size_type i)
{
	// if i is too big, check function will throw, preventing access to a nonexistent element
	check(i, "subscript out of range");
	return (*data)[i];
}


template<typename T>
const T& Blob<T>::operator [](size_type i) const
{
	// if i is too big, check function will throw, preventing access to a nonexistent element
	check(i, "subscript out of range");
	return (*data)[i];
}

template<typename T>
void Blob<T>::pop_back()
{
	check(0, "pop_back on empty Blob");
	data->pop_back();
}

```

BlobPtr：

```cpp
#include "Blob.h"
#include <memory>
#include <vector>

template <typename> class BlobPtr;

template <typename T>
bool operator ==(const BlobPtr<T>& lhs, const BlobPtr<T>& rhs);

template <typename T>
bool operator < (const BlobPtr<T>& lhs, const BlobPtr<T>& rhs);


template<typename T> class BlobPtr
{
	friend bool operator ==<T>
	(const BlobPtr<T>& lhs, const BlobPtr<T>& rhs);

	friend bool operator < <T>
		(const BlobPtr<T>& lhs, const BlobPtr<T>& rhs);

public:
	BlobPtr() : curr(0) {}
	BlobPtr(Blob<T>& a, std::size_t sz = 0) :
		wptr(a.data), curr(sz)
	{}

	T& operator*() const
	{
		auto p = check(curr, "dereference past end");
		return (*p)[curr];
	}

	// prefix
	BlobPtr& operator++();
	BlobPtr& operator--();

	// postfix
	BlobPtr operator ++(int);
	BlobPtr operator --(int);

private:
	// returns  a shared_ptr to the vector if the check succeeds
	std::shared_ptr<std::vector<T>>
		check(std::size_t, const std::string&) const;

	std::weak_ptr<std::vector<T>> wptr;
	std::size_t curr;

};

// prefix ++
template<typename T>
BlobPtr<T>& BlobPtr<T>::operator ++()
{
	// if curr already points past the end of the container, can't increment it
	check(curr, "increment past end of StrBlob");
	++curr;
	return *this;
}

// prefix --
template<typename T>
BlobPtr<T>& BlobPtr<T>::operator --()
{
	--curr;
	check(curr, "decrement past begin of BlobPtr");

	return *this;
}


// postfix ++
template<typename T>
BlobPtr<T> BlobPtr<T>::operator ++(int)
{
	BlobPtr ret = *this;
	++*this;

	return ret;
}

// postfix --
template<typename T>
BlobPtr<T> BlobPtr<T>::operator --(int)
{
	BlobPtr ret = *this;
	--*this;

	return ret;
}

template<typename T> bool operator==(const BlobPtr<T> &lhs, const BlobPtr<T> &rhs)
{
	if (lhs.wptr.lock() != rhs.wptr.lock())
	{
		throw runtime_error("ptrs to different Blobs!");
	}
	return lhs.i == rhs.i;
}

template<typename T> bool operator< (const BlobPtr<T> &lhs, const BlobPtr<T> &rhs)
{
	if (lhs.wptr.lock() != rhs.wptr.lock())
	{
		throw runtime_error("ptrs to different Blobs!");
	}
	return lhs.i < rhs.i;
}
```

## 练习16.13

> 解释你为 `BlobPtr` 的相等和关系运算符选择哪种类型的友好关系？

解：

这里需要与类型一一对应，所以就选择一对一友好关系。

## 练习16.14

> 编写 `Screen` 类模版，用非类型参数定义 `Screen` 的高和宽。


解：

Screen

```cpp
#include <string>
#include <iostream>

template<unsigned H, unsigned W>
class Screen
{
public:
	typedef std::string::size_type pos;
	Screen() = default; // needed because Screen has another constructor
	// cursor initialized to 0 by its in-class initializer
	Screen(char c) :contents(H * W, c) {}
	char get() const              // get the character at the cursor
	{
		return contents[cursor];
	}       // implicitly inline
	Screen &move(pos r, pos c);      // can be made inline later

	friend std::ostream & operator<< (std::ostream &os, const Screen<H, W> & c)
	{
		unsigned int i, j;
		for (i = 0; i<c.height; i++)
		{
			os << c.contents.substr(0, W) << std::endl;
		}
		return os;
	}

	friend std::istream & operator>> (std::istream &is, Screen &  c)
	{
		char a;
		is >> a;
		std::string temp(H*W, a);
		c.contents = temp;
		return is;
	}
private:
	pos cursor = 0;
	pos height = H, width = W;
	std::string contents;
};

template<unsigned H, unsigned W>
inline Screen<H, W>& Screen<H, W>::move(pos r, pos c)
{
	pos row = r * width;
	cursor = row + c;
	return *this;
}
```

## 练习16.15

> 为你的 `Screen` 模版实现输入和输出运算符。`Screen` 类需要哪些友元（如果需要的话）来令输入和输出运算符正确工作？解释每个友元声明（如果有的话）为什么是必要的。

解：

类的 `operator<<` 和 `operator>>` 应该是类的友元。

## 练习16.16

> 将 `StrVec` 类重写为模版，命名为 `Vec`。

解：

Vec:

```cpp
#include <memory>

/**
*  @brief a vector like class
*/
template<typename T>
class Vec
{
public:
	Vec() :element(nullptr), first_free(nullptr), cap(nullptr) {}
	Vec(std::initializer_list<T> l);
	Vec(const Vec& v);

	Vec& operator =(const Vec& rhs);

	~Vec();

	// memmbers
	void push_back(const T& t);

	std::size_t size() const { return first_free - element; }
	std::size_t capacity()const { return cap - element; }

	T* begin() const { return element; }
	T* end()   const { return first_free; }

	void reserve(std::size_t n);

	void resize(std::size_t n);
	void resize(std::size_t n, const T& t);

private:
	// data members
	T* element;
	T* first_free;
	T* cap;

	std::allocator<T> alloc;

	// utillities
	void reallocate();
	void chk_n_alloc() { if (size() == capacity()) reallocate(); }
	void free();

	void wy_alloc_n_move(std::size_t n);

	std::pair<T*, T*> alloc_n_copy(T* b, T* e);
};


// copy constructor
template<typename T>
Vec<T>::Vec(const Vec &v)
{
	/**
	* @brief newData is a pair of pointers pointing to newly allocated and copied
	*        from range : [b, e)
	*/
	std::pair<T*, T*> newData = alloc_n_copy(v.begin(), v.end());

	element = newData.first;
	first_free = cap = newData.second;
}


// constructor that takes initializer_list<T>
template<typename T>
Vec<T>::Vec(std::initializer_list<T> l)
{
	// allocate memory as large as l.size()
	T* const newData = alloc.allocate(l.size());

	// copy elements from l to the address allocated
	T* p = newData;
	for (const auto &t : l)
		alloc.construct(p++, t);

	// build data structure
	element = newData;
	first_free = cap = element + l.size();
}


// operator =
template<typename T>
Vec<T>& Vec<T>::operator =(const Vec& rhs)
{
	// allocate and copy first to protect against self_assignment
	std::pair<T*, T*> newData = alloc_n_copy(rhs.begin(), rhs.end());

	// destroy and deallocate
	free();

	// update data structure
	element = newData.first;
	first_free = cap = newData.second;

	return *this;
}


// destructor
template<typename T>
Vec<T>::~Vec()
{
	free();
}



/**
* @brief   allocate new memeory if nessary and push back the new T
* @param t new T
*/
template<typename T>
void Vec<T>::push_back(const T &t)
{
	chk_n_alloc();
	alloc.construct(first_free++, t);
}



/**
* @brief   preallocate enough memory for specified number of elements
* @param n number of elements required
*/
template<typename T>
void Vec<T>::reserve(std::size_t n)
{
	// if n too small, just return without doing anything
	if (n <= capacity()) return;

	// allocate new memory and move data from old address to the new one
	wy_alloc_n_move(n);
}


/**
*  @brief  Resizes to the specified number of elements.
*  @param  n  Number of elements the %vector should contain.
*
*  This function will resize it to the specified
*  number of elements.  If the number is smaller than the
*  current size it is truncated, otherwise
*  default constructed elements are appended.
*/
template<typename T>
void Vec<T>::resize(std::size_t n)
{
	resize(n, T());
}



/**
*  @brief  Resizes it to the specified number of elements.
*  @param  __new_size  Number of elements it should contain.
*  @param  __x  Data with which new elements should be populated.
*
*  This function will resize it to the specified
*  number of elements.  If the number is smaller than the
*  current size the it is truncated, otherwise
*  the it is extended and new elements are populated with
*  given data.
*/
template<typename T>
void Vec<T>::resize(std::size_t n, const T &t)
{
	if (n < size())
	{
		// destroy the range [element+n, first_free) using destructor
		for (auto p = element + n; p != first_free;)
			alloc.destroy(p++);
		// update first_free to point to the new address
		first_free = element + n;
	}
	else if (n > size())
	{
		for (auto i = size(); i != n; ++i)
			push_back(t);
	}
}


/**
* @brief   allocate new space for the given range and copy them into it
* @param b
* @param e
* @return  a pair of pointers pointing to [first element , one past the last) in the new space
*/
template<typename T>
std::pair<T*, T*>
Vec<T>::alloc_n_copy(T *b, T *e)
{
	// calculate the size needed and allocate space accordingly
	T* data = alloc.allocate(e - b);
	return{ data, std::uninitialized_copy(b, e, data) };
	//            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	// which copies the range[first, last) to the space to which
	// the starting address data is pointing.
	// This function returns a pointer to one past the last element
}


/**
* @brief   destroy the elements and deallocate the space previously allocated.
*/
template<typename T>
void Vec<T>::free()
{
	// if not nullptr
	if (element)
	{
		// destroy it in reverse order.
		for (auto p = first_free; p != element;)
			alloc.destroy(--p);

		alloc.deallocate(element, capacity());
	}
}


/**
* @brief   allocate memory for spicified number of elements
* @param n
* @note    it's user's responsibility to ensure that @param n is greater than
*          the current capacity.
*/
template<typename T>
void Vec<T>::wy_alloc_n_move(std::size_t n)
{
	// allocate as required.
	std::size_t newCapacity = n;
	T* newData = alloc.allocate(newCapacity);

	// move the data from old place to the new one
	T* dest = newData;
	T* old = element;
	for (std::size_t i = 0; i != size(); ++i)
		alloc.construct(dest++, std::move(*old++));

	free();

	// update data structure
	element = newData;
	first_free = dest;
	cap = element + newCapacity;
}


/**
* @brief   Double the capacity and using std::move move the original data to the newly
*          allocated memory
*/
template<typename T>
void Vec<T>::reallocate()
{
	// calculate the new capacity required
	std::size_t newCapacity = size() ? 2 * size() : 1;

	// allocate and move old data to the new space
	wy_alloc_n_move(newCapacity);
}
```

## 练习16.17

> 声明为 `typename` 的类型参数和声明为 `class` 的类型参数有什么不同（如果有的话）？什么时候必须使用`typename`？

解：

没有什么不同。当我们希望通知编译器一个名字表示类型时，必须使用关键字 `typename`，而不能使用 `class`。

## 练习16.18

> 解释下面每个函数模版声明并指出它们是否非法。更正你发现的每个错误。

```cpp
(a) template <typename T, U, typename V> void f1(T, U, V);
(b) template <typename T> T f2(int &T);
(c) inline template <typename T> T foo(T, unsigned int *);
(d) template <typename T> f4(T, T);
(e) typedef char Ctype;
	template <typename Ctype> Ctype f5(Ctype a);
```

解：

* (a) 非法。应该为 `template <typename T, typename U, typename V> void f1(T, U, V);`。
* (b) 非法。应该为 `template <typename T> T f2(int &t);`
* (c) 非法。应该为 `template <typename T> inline T foo(T, unsigned int*);`
* (d) 非法。应该为 `template <typename T> T f4(T, T);`
* (e) 非法。`Ctype` 被隐藏了。

## 练习16.19

> 编写函数，接受一个容器的引用，打印容器中的元素。使用容器的 `size_type` 和 `size`成员来控制打印元素的循环。

解：

```cpp
template<typename Container>
void print(const Container& c)
{
	for (typename Container::size_type i = 0; i != c.size(); ++i)
		std::cout << c[i] << " ";
}
```

## 练习16.20

> 重写上一题的函数，使用`begin` 和 `end` 返回的迭代器来控制循环。

解：

```cpp
template<typename Container>
void print(const Container& c)
{
	for (auto it = c.begin(); it != c.end(); ++it)
		std::cout << *it << " ";
}
```

## 练习16.21

> 编写你自己的 `DebugDelete` 版本。

解：

DebugDelete

```cpp
#include <iostream>

class DebugDelete
{
public:
	DebugDelete(std::ostream& s = std::cerr) : os(s) {}
	template<typename T>
	void operator() (T* p) const
	{
		os << "deleting unique_ptr" << std::endl;
		delete p;
	}

private:
	std::ostream& os;
};
```

## 练习16.22

> 修改12.3节中你的 `TextQuery` 程序，令 `shared_ptr` 成员使用 `DebugDelete` 作为它们的删除器。

解：

略

## 练习16.23

> 预测在你的查询主程序中何时会执行调用运算符。如果你的预测和实际不符，确认你理解了原因。

解：

略

## 练习16.24

> 为你的 `Blob` 模版添加一个构造函数，它接受两个迭代器。

解：

```cpp
template<typename T>    //for class
template<typename It>   //for this member
Blob<T>::Blob(It b, It e) :
    data(std::make_shared<std::vector<T>>(b, e))
{ }
```

## 练习16.25

> 解释下面这些声明的含义。
```cpp
extern template class vector<string>;
template class vector<Sales_data>;
```

解：

前者是模版声明，后者是实例化定义。

## 练习16.26

> 假设 `NoDefault` 是一个没有默认构造函数的类，我们可以显式实例化 `vector<NoDefualt>`吗？如果不可以，解释为什么。

解：

不可以。如

```cpp
std::vector<NoDefault> vec(10);
```

会使用 `NoDefault` 的默认构造函数，而 `NoDefault` 没有默认构造函数，因此是不可以的。

## 练习16.27

> 对下面每条带标签的语句，解释发生了什么样的实例化（如果有的话）。如果一个模版被实例化，解释为什么;如果未实例化，解释为什么没有。
> 
```cpp
template <typename T> class Stack {	};
void f1(Stack<char>); 		//(a)
class Exercise {
	Stack<double> &rds;		//(b)
	Stack<int> si;			//(c)
};
int main() {
	Stack<char> *sc;		//(d)
	f1(*sc);				//(e)
	int iObj = sizeof(Stack<string>);	//(f)
}
```

解：

(a)、(b)、(c)、(f) 都发生了实例化，(d)、(e) 没有实例化。

## 练习16.28

> 编写你自己版本的 `shared_ptr` 和 `unique_ptr`。

解：

shared_ptr

```cpp
#pragma once
#include <functional>
#include "delete.h"

namespace cp5
{
	template<typename T>
	class SharedPointer;

	template<typename T>
	auto swap(SharedPointer<T>& lhs, SharedPointer<T>& rhs)
	{
		using std::swap;
		swap(lhs.ptr, rhs.ptr);
		swap(lhs.ref_count, rhs.ref_count);
		swap(lhs.deleter, rhs.deleter);
	}

	template<typename T>
	class SharedPointer
	{
	public:
		//
		//  Default Ctor
		//
		SharedPointer()
			: ptr{ nullptr }, ref_count{ new std::size_t(1) }, deleter{ cp5::Delete{} }
		{}
		//
		//  Ctor that takes raw pointer
		//
		explicit SharedPointer(T* raw_ptr)
			: ptr{ raw_ptr }, ref_count{ new std::size_t(1) }, deleter{ cp5::Delete{} }
		{}
		//
		//  Copy Ctor
		//
		SharedPointer(SharedPointer const& other)
			: ptr{ other.ptr }, ref_count{ other.ref_count }, deleter{ other.deleter }
		{
			++*ref_count;
		}
		//
		//  Move Ctor
		//
		SharedPointer(SharedPointer && other) noexcept
			: ptr{ other.ptr }, ref_count{ other.ref_count }, deleter{ std::move(other.deleter) }
		{
			other.ptr = nullptr;
			other.ref_count = nullptr;
		}
		//
		//  Copy assignment
		//
		SharedPointer& operator=(SharedPointer const& rhs)
		{
			//increment first to ensure safty for self-assignment
			++*rhs.ref_count;
			decrement_and_destroy();
			ptr = rhs.ptr, ref_count = rhs.ref_count, deleter = rhs.deleter;
			return *this;
		}
		//
		//  Move assignment
		//
		SharedPointer& operator=(SharedPointer && rhs) noexcept
		{
			cp5::swap(*this, rhs);
			rhs.decrement_and_destroy();
			return *this;
		}
		//
		//  Conversion operator
		//
		operator bool() const
		{
			return ptr ? true : false;
		}
		//
		//  Dereference
		//
		T& operator* () const
		{
			return *ptr;
		}
		//
		//  Arrow
		//
		T* operator->() const
		{
			return &*ptr;
		}
		//
		//  Use count
		//
		auto use_count() const
		{
			return *ref_count;
		}
		//
		//  Get underlying pointer
		//
		auto get() const
		{
			return ptr;
		}
		//
		//  Check if the unique user
		//
		auto unique() const
		{
			return 1 == *refCount;
		}
		//
		//  Swap
		//
		auto swap(SharedPointer& rhs)
		{
			::swap(*this, rhs);
		}
		//
		// Free the object pointed to, if unique
		//
		auto reset()
		{
			decrement_and_destroy();
		}
		//
		// Reset with the new raw pointer
		//
		auto reset(T* pointer)
		{
			if (ptr != pointer)
			{
				decrement_n_destroy();
				ptr = pointer;
				ref_count = new std::size_t(1);
			}
		}
		//
		//  Reset with raw pointer and deleter
		//
		auto reset(T *pointer, const std::function<void(T*)>& d)
		{
			reset(pointer);
			deleter = d;
		}
		//
		//  Dtor
		//
		~SharedPointer()
		{
			decrement_and_destroy();
		}
	private:
		T* ptr;
		std::size_t* ref_count;
		std::function<void(T*)> deleter;

		auto decrement_and_destroy()
		{
			if (ptr && 0 == --*ref_count)
				delete ref_count,
				deleter(ptr);
			else if (!ptr)
				delete ref_count;
			ref_count = nullptr;
			ptr = nullptr;
		}
	};
}//namespace
```

unique_ptr:

```cpp
#include "debugDelete.h"

// forward declarations for friendship

template<typename, typename> class unique_pointer;
template<typename T, typename D> void
swap(unique_pointer<T, D>& lhs, unique_pointer<T, D>& rhs);

/**
*  @brief  std::unique_ptr like class template.
*/
template <typename T, typename D = DebugDelete>
class unique_pointer
{
	friend void swap<T, D>(unique_pointer<T, D>& lhs, unique_pointer<T, D>& rhs);

public:
	// preventing copy and assignment
	unique_pointer(const unique_pointer&) = delete;
	unique_pointer& operator = (const unique_pointer&) = delete;

	// default constructor and one taking T*
	unique_pointer() = default;
	explicit unique_pointer(T* up) : ptr(up) {}

	// move constructor
	unique_pointer(unique_pointer&& up) noexcept
		: ptr(up.ptr) { up.ptr = nullptr; }

	// move assignment
	unique_pointer& operator =(unique_pointer&& rhs) noexcept;

	// std::nullptr_t assignment
	unique_pointer& operator =(std::nullptr_t n) noexcept;



	// operator overloaded :  *  ->  bool
	T& operator  *() const { return *ptr; }
	T* operator ->() const { return &this->operator *(); }
	operator bool() const { return ptr ? true : false; }

	// return the underlying pointer
	T* get() const noexcept{ return ptr; }

	// swap member using swap friend
	void swap(unique_pointer<T, D> &rhs) { ::swap(*this, rhs); }

	// free and make it point to nullptr or to p's pointee.
	void reset()     noexcept{ deleter(ptr); ptr = nullptr; }
	void reset(T* p) noexcept{ deleter(ptr); ptr = p; }

	// return ptr and make ptr point to nullptr.
	T* release();


	~unique_pointer()
	{
		deleter(ptr);
	}
private:
	T* ptr = nullptr;
	D  deleter = D();
};


// swap
template<typename T, typename D>
inline void
swap(unique_pointer<T, D>& lhs, unique_pointer<T, D>& rhs)
{
	using std::swap;
	swap(lhs.ptr, rhs.ptr);
	swap(lhs.deleter, rhs.deleter);
}

// move assignment
template<typename T, typename D>
inline unique_pointer<T, D>&
unique_pointer<T, D>::operator =(unique_pointer&& rhs) noexcept
{
	// prevent self-assignment
	if (this->ptr != rhs.ptr)
	{
		deleter(ptr);
		ptr = nullptr;
		::swap(*this, rhs);
	}
	return *this;
}


// std::nullptr_t assignment
template<typename T, typename D>
inline unique_pointer<T, D>&
unique_pointer<T, D>::operator =(std::nullptr_t n) noexcept
{
	if (n == nullptr)
	{
		deleter(ptr);   ptr = nullptr;
	}
	return *this;
}

// relinquish contrul by returnning ptr and making ptr point to nullptr.
template<typename T, typename D>
inline T*
unique_pointer<T, D>::release()
{
	T* ret = ptr;
	ptr = nullptr;
	return ret;
}
```

## 练习16.29

> 修改你的 `Blob` 类，用你自己的 `shared_ptr` 代替标准库中的版本。

解：

略

## 练习16.30

> 重新运行你的一些程序，验证你的 `shared_ptr` 类和修改后的 `Blob` 类。（注意：实现 `weak_ptr` 类型超出了本书范围，因此你不能将`BlobPtr`类与你修改后的`Blob`一起使用。）

解：

略

## 练习16.31

> 如果我们将 `DebugDelete` 与 `unique_ptr` 一起使用，解释编译器将删除器处理为内联形式的可能方式。

解：

略

## 练习16.32

> 在模版实参推断过程中发生了什么？

解：

在模版实参推断过程中，编译器使用函数调用中的实参类型来寻找模版实参，用这些模版实参生成的函数版本与给定的函数调用最为匹配。

## 练习16.33

> 指出在模版实参推断过程中允许对函数实参进行的两种类型转换。

解：

* `const` 转换：可以将一个非 `const` 对象的引用（或指针）传递给一个 `const` 的引用（或指针）形参。
* 数组或函数指针转换：如果函数形参不是引用类型，则可以对数组或函数类型的实参应用正常的指针转换。一个数组实参可以转换为一个指向其首元素的指针。类似的，一个函数实参可以转换为一个该函数类型的指针。

## 练习16.34

> 对下面的代码解释每个调用是否合法。如果合法，`T` 的类型是什么？如果不合法，为什么？

```cpp
template <class T> int compare(const T&, const T&);
(a) compare("hi", "world");
(b) compare("bye", "dad");
```

解：

* (a) 不合法。`compare(const char [3], const char [6])`, 两个实参类型不一致。
* (b) 合法。`compare(const char [4], const char [4])`.

## 练习16.35

> 下面调用中哪些是错误的（如果有的话）？如果调用合法，`T` 的类型是什么？如果调用不合法，问题何在？

```cpp
template <typename T> T calc(T, int);
tempalte <typename T> T fcn(T, T);
double d; float f; char c;
(a) calc(c, 'c'); 
(b) calc(d, f);
(c) fcn(c, 'c');
(d) fcn(d, f);
```

解：

* (a) 合法，类型为`char`
* (b) 合法，类型为`double`
* (c) 合法，类型为`char`
* (d) 不合法，这里无法确定T的类型是`float`还是`double`

## 练习16.36

> 进行下面的调用会发生什么：

```cpp
template <typename T> f1(T, T);
template <typename T1, typename T2) f2(T1, T2);
int i = 0, j = 42, *p1 = &i, *p2 = &j;
const int *cp1 = &i, *cp2 = &j;
(a) f1(p1, p2);
(b) f2(p1, p2);
(c) f1(cp1, cp2);
(d) f2(cp1, cp2);
(e) f1(p1, cp1);
(f) f2(p1, cp1);
```

解：

```cpp
(a) f1(int*, int*);
(b) f2(int*, int*);
(c) f1(const int*, const int*);
(d) f2(const int*, const int*);
(e) f1(int*, const int*); 这个使用就不合法
(f) f2(int*, const int*);
```

## 练习16.37

> 标准库 `max` 函数有两个参数，它返回实参中的较大者。此函数有一个模版类型参数。你能在调用 `max` 时传递给它一个 `int` 和一个 `double` 吗？如果可以，如何做？如果不可以，为什么？

解：

可以。提供显式的模版实参：

```cpp
int a = 1;
double b = 2;
std::max<double>(a, b);
```

## 练习16.38

> 当我们调用 `make_shared` 时，必须提供一个显示模版实参。解释为什么需要显式模版实参以及它是如果使用的。

解：

如果不显示提供模版实参，那么 `make_shared` 无法推断要分配多大内存空间。

## 练习16.39

> 对16.1.1节 中的原始版本的 `compare` 函数，使用一个显式模版实参，使得可以向函数传递两个字符串字面量。

解： 

```cpp
compare<std::string>("hello", "world")  
```

## 练习16.40

> 下面的函数是否合法？如果不合法，为什么？如果合法，对可以传递的实参类型有什么限制（如果有的话）？返回类型是什么？

```cpp
template <typename It>
auto fcn3(It beg, It end) -> decltype(*beg + 0)
{
	//处理序列
	return *beg;
}
```

解：

合法。该类型需要支持 `+` 操作。

## 练习16.41

> 编写一个新的 `sum` 版本，它返回类型保证足够大，足以容纳加法结果。

解：

```cpp
template<typename T>
auto sum(T lhs, T rhs) -> decltype( lhs + rhs)
{
    return lhs + rhs;
}

```

## 练习16.42

> 对下面每个调用，确定 `T` 和 `val` 的类型：
```cpp
template <typename T> void g(T&& val);
int i = 0; const int ci = i;
(a) g(i);
(b) g(ci);
(c) g(i * ci);
```

解：

```cpp
(a) int&
(b) const int&
(c) int&&
```

## 练习16.43

> 使用上一题定义的函数，如果我们调用`g(i = ci)`,`g` 的模版参数将是什么？

解：

`i = ci` 返回的是左值，因此 `g` 的模版参数是 `int&`

## 练习16.44

> 使用与第一题中相同的三个调用，如果 `g` 的函数参数声明为 `T`（而不是`T&&`），确定T的类型。如果`g`的函数参数是 `const T&`呢？

解：

当声明为`T`的时候，`T`的类型为`int&`。
当声明为`const T&`的时候，T的类型为`int&`。

## 练习16.45

> 如果下面的模版，如果我们对一个像42这样的字面常量调用`g`，解释会发生什么？如果我们对一个`int` 类型的变量调用`g` 呢？

```cpp
template <typename T> void g(T&& val) { vector<T> v; }
```

解：

当使用字面常量，`T`将为`int`。
当使用`int`变量，`T`将为`int&`。编译的时候将会报错，因为没有办法对这种类型进行内存分配，无法创建`vector<int&>`。

## 练习16.46

> 解释下面的循环，它来自13.5节中的 `StrVec::reallocate`:

```cpp
for (size_t i = 0; i != size(); ++i)
	alloc.construct(dest++, std::move(*elem++));
```

解：

在每个循环中，对 `elem` 的解引用操作 `*` 当中，会返回一个左值，`std::move` 函数将该左值转换为右值，提供给 `construct` 函数。

## 练习16.47

> 编写你自己版本的翻转函数，通过调用接受左值和右值引用参数的函数来测试它。

解：

```cpp
template<typename F, typename T1, typename T2>
void flip(F f, T1&& t1, T2&& t2)
{
    f(std::forward<T2>(t2), std::forward<T1>(t1));
}
```

## 练习16.48

> 编写你自己版本的 `debug_rep` 函数。

解：

```cpp
template<typename T> std::string debug_rep(const T& t)
{
    std::ostringstream ret;
    ret << t;
    return ret.str();
}

template<typename T> std::string debug_rep(T* p)
{
    std::ostringstream ret;
    ret << "pointer: " << p;

    if(p)
        ret << " " << debug_rep(*p);
    else
        ret << " null pointer";

    return ret.str();
}
```

## 练习16.49

> 解释下面每个调用会发生什么：

```cpp
template <typename T> void f(T);
template <typename T> void f(const T*);
template <typename T> void g(T);
template <typename T> void g(T*);
int i = 42, *p = &i;
const int ci = 0, *p2 = &ci;
g(42); g(p); g(ci); g(p2);
f(42); f(p); f(ci); f(p2);
```

解：

```cpp
    g(42);    	//g(T )
    g(p);     	//g(T*)
    g(ci);      //g(T)   
    g(p2);      //g(T*)    
    f(42);    	//f(T)
    f(p);     	//f(T)
    f(ci);    	//f(T)
    f(p2);      //f(const T*)
```

## 练习16.50

> 定义上一个练习中的函数，令它们打印一条身份信息。运行该练习中的代码。如果函数调用的行为与你预期不符，确定你理解了原因。

解：

略

## 练习16.51

> 调用本节中的每个 `foo`，确定 `sizeof...(Args)` 和 `sizeof...(rest)`分别返回什么。

解：

```cpp
#include <iostream>

using namespace std;

template <typename T, typename ... Args>
void foo(const T &t, const Args& ... rest){
    cout << "sizeof...(Args): " << sizeof...(Args) << endl;
    cout << "sizeof...(rest): " << sizeof...(rest) << endl;
};

void test_param_packet(){
    int i = 0;
    double d = 3.14;
    string s = "how now brown cow";

    foo(i, s, 42, d);
    foo(s, 42, "hi");
    foo(d, s);
    foo("hi");
}

int main(){
    test_param_packet();
    return 0;
}
```

结果：

```
sizeof...(Args): 3
sizeof...(rest): 3
sizeof...(Args): 2
sizeof...(rest): 2
sizeof...(Args): 1
sizeof...(rest): 1
sizeof...(Args): 0
sizeof...(rest): 0
```

## 练习16.52

> 编写一个程序验证上一题的答案。

解：

参考16.51。

## 练习16.53

> 编写你自己版本的 `print` 函数，并打印一个、两个及五个实参来测试它，要打印的每个实参都应有不同的类型。 

解：

```cpp
template<typename Printable>
std::ostream& print(std::ostream& os, Printable const& printable)
{
    return os << printable;
}
// recursion
template<typename Printable, typename... Args>
std::ostream& print(std::ostream& os, Printable const& printable, Args const&... rest)
{
    return print(os << printable << ", ", rest...);
}
```

## 练习16.54

> 如果我们对一个没 `<<` 运算符的类型调用 `print`，会发生什么？

解：

无法通过编译。

## 练习16.55

> 如果我们的可变参数版本 `print` 的定义之后声明非可变参数版本，解释可变参数的版本会如何执行。

解：

`error: no matching function for call to 'print(std::ostream&)'`

## 练习16.56

> 编写并测试可变参数版本的 `errorMsg`。

解：

```cpp
template<typename... Args>
std::ostream& errorMsg(std::ostream& os, const Args... rest)
{
    return print(os, debug_rep(rest)...);
}
```

## 练习16.57

> 比较你的可变参数版本的 `errorMsg` 和6.2.6节中的 `error_msg`函数。两种方法的优点和缺点各是什么？

解：

可变参数版本有更好的灵活性。

## 练习16.58

> 为你的 `StrVec` 类及你为16.1.2节练习中编写的 `Vec` 类添加 `emplace_back` 函数。

解：

```cpp
template<typename T>        //for the class  template
template<typename... Args>  //for the member template
inline void
Vec<T>::emplace_back(Args&&...args)
{
    chk_n_alloc();
    alloc.construct(first_free++, std::forward<Args>(args)...);
}
```

## 练习16.59

> 假定 `s` 是一个 `string`，解释调用 `svec.emplace_back(s)`会发生什么。

解：

会在 `construst` 函数中转发扩展包。

## 练习16.60

> 解释 `make_shared` 是如何工作的。

解：

`make_shared` 是一个可变模版函数，它将参数包转发然后构造一个对象，再然后一个指向该对象的智能指针。

## 练习16.61

> 定义你自己版本的 `make_shared`。

解：

```cpp
template <typename T, typename ... Args>
auto make_shared(Args&&... args) -> std::shared_ptr<T>
{
	return std::shared_ptr<T>(new T(std::forward<Args>(args)...));
}
```

## 练习16.62

> 定义你自己版本的 `hash<Sales_data>`, 并定义一个 `Sales_data` 对象的 `unorder_multise`。将多条交易记录保存到容器中，并打印其内容。

解：

略

## 练习16.63

> 定义一个函数模版，统计一个给定值在一个`vecor`中出现的次数。测试你的函数，分别传递给它一个`double`的`vector`，一个`int`的`vector`以及一个`string`的`vector`。

解：

```cpp
#include <iostream>
#include <vector>
#include <cstring>

// template
template<typename T>
std::size_t  count(std::vector<T> const& vec, T value) 
{
    auto count = 0u;
    for(auto const& elem  : vec)
        if(value == elem) ++count;
    return count;
}

// template specialization
template<>
std::size_t count (std::vector<const char*> const& vec, const char* value)
{
    auto count = 0u;
    for(auto const& elem : vec)
        if(0 == strcmp(value, elem)) ++count;
    return count;
}
int main()
{
    // for ex16.63
    std::vector<double> vd = { 1.1, 1.1, 2.3, 4 };
    std::cout << count(vd, 1.1) << std::endl;
    
    // for ex16.64
    std::vector<const char*> vcc = { "alan", "alan", "alan", "alan", "moophy" };
    std::cout << count(vcc, "alan") << std::endl;

    return 0;
}
```

## 练习16.64

> 为上一题的模版编写特例化版本来处理`vector<const char*>`。编写程序使用这个特例化版本。

解：

参考16.64。

## 练习16.65

> 在16.3节中我们定义了两个重载的 `debug_rep` 版本，一个接受 `const char*` 参数，另一个接受 `char *` 参数。将这两个函数重写为特例化版本。

解：

```cpp
#include <iostream>
#include <vector>
#include <cstring>
#include <sstream>

// template
template <typename T>
std::string debug_rep(T* t);

// template specialization T=const char*  ,  char*  respectively.
template<>
std::string debug_rep(const char* str);
template<>
std::string debug_rep(      char *str);

int main()
{
    char p[] = "alan";
    std::cout << debug_rep(p) << "\n";
    return 0;
}


template <typename T>
std::string debug_rep(T* t)
{
    std::ostringstream ret;
    ret << t;
    return ret.str();
}

// template specialization
// T = const char*
template<>
std::string debug_rep(const char* str)
{
    std::string ret(str);
    return str;
}

// template specialization
// T =       char*
template<>
std::string debug_rep(      char *str)
{
    std::string ret(str);
    return ret;
}
```

## 练习16.66

> 重载`debug_rep` 函数与特例化它相比，有何优点和缺点？

解：

重载函数会改变函数匹配。

## 练习16.67

> 定义特例化版本会影响 `debug_rep` 的函数匹配吗？如果不影响，为什么？

解：

不影响，特例化是模板的一个实例，并没有重载函数。

# 第17章 : 标准库与特殊设施

## 练习17.1

> 定义一个保存三个`int`值的 `tuple`，并将其成员分别初始化为10、20和30。

解：

```cpp
auto t = tuple<int, int, int>{10, 20, 30};
```

## 练习17.2

> 定义一个 `tuple`，保存一个 `string`、一个`vector<string>` 和一个 `pair<string, int>`。

解：

```cpp
auto t = tuple<string, vector<string>, pair<string, int> >
```

## 练习17.3

> 重写12.3节中的 `TextQuery` 程序，使用 `tuple` 代替 `QueryResult` 类。你认为哪种设计更好？为什么？

解：

程序略。

我认为`tuple`更方便。

## 练习17.4

> 编写并测试你自己版本的 `findBook` 函数。


解：

```cpp
#include <iostream>
#include <tuple>
#include <string>
#include <vector>
#include <algorithm>
#include <utility>
#include <numeric>

#include "ex_17_4_SalesData.h"

using namespace std;

// matches有三个成员：1.一个书店的索引。2.指向书店中元素的迭代器。3.指向书店中元素的迭代器。
typedef tuple<vector<Sales_data>::size_type,
              vector<Sales_data>::const_iterator,
              vector<Sales_data>::const_iterator>
    matches;

// files保存每家书店的销售记录
// findBook返回一个vector，每家销售了给定书籍的书店在其中都有一项
vector<matches> findBook(const vector<vector<Sales_data>> &files,
                         const string &book)
{
    vector<matches> ret; //初始化为空vector
    // 对每家书店，查找给定书籍匹配的记录范围
    for (auto it = files.cbegin; it != files.cend(); ++it)
    {
        // 查找具有相同ISBN的Sales_data范围，found是一个迭代器pair
        auto found = equal_range(it->cbegin(), it->cend(), book, compareIsbn);
        if (found.first != found.second)  // 此书店销售了给定书籍
            // 记住此书店的索引及匹配的范围
            ret.push_back(make_tuple(it - files.cbegin(), found.first, found.second));
    }
    return ret; //如果未找到匹配记录，ret为空
}

void reportResults(istream &in, ostream &os,
                       const vector<vector<Sales_data> > &files){
    string s;  //要查找的书
    while (in >> s){
        auto trans = findBook(files, s);
        if (trans.empty()){
            cout << s << " not found in any stores" << endl;
            continue;  // 获得下一本要查找的书
        }
        for (const auto &store : trans)  // 对每家销售了给定书籍的书店
            // get<n>返回store中tuple的指定的成员
            os << "store " << get<0>(store) << " sales: "
               << accumulate(get<1>(store), get<2>(store), Sales_data(s))
               << endl;
    }
}

int main(){
    return 0;
}
```

## 练习17.5

> 重写 `findBook`，令其返回一个 `pair`，包含一个索引和一个迭代器pair。

解：

```cpp
typedef std::pair<std::vector<Sales_data>::size_type,
                  std::pair<std::vector<Sales_data>::const_iterator,
                            std::vector<Sales_data>::const_iterator>>
                                                                      matches_pair;

std::vector<matches_pair>
findBook_pair(const std::vector<std::vector<Sales_data> > &files,
              const std::string &book)
{
    std::vector<matches_pair> ret;
    for(auto it = files.cbegin(); it != files.cend(); ++it)
    {
        auto found = std::equal_range(it->cbegin(), it->cend(), book, compareIsbn);
        if(found.first != found.second)
            ret.push_back(std::make_pair(it - files.cbegin(),
                                         std::make_pair(found.first, found.second)));
    }
    return ret;
}
```

## 练习17.6

> 重写 `findBook`，不使用`tuple`和`pair`。

解：

```cpp
struct matches_struct
{
    std::vector<Sales_data>::size_type st;
    std::vector<Sales_data>::const_iterator first;
    std::vector<Sales_data>::const_iterator last;
    matches_struct(std::vector<Sales_data>::size_type s,
                   std::vector<Sales_data>::const_iterator f,
                   std::vector<Sales_data>::const_iterator l) : st(s), first(f), last(l) { }
} ;

std::vector<matches_struct>
findBook_struct(const std::vector<std::vector<Sales_data> > &files,
                const std::string &book)
{
    std::vector<matches_struct> ret;
    for(auto it = files.cbegin(); it != files.cend(); ++it)
    {
        auto found = std::equal_range(it->cbegin(), it->cend(), book, compareIsbn);
        if(found.first != found.second)
            ret.push_back(matches_struct(it - files.cbegin(), found.first, found.second));
    }
    return ret;
}
```

## 练习17.7

> 解释你更倾向于哪个版本的`findBook`，为什么。

解：

使用`tuple`的版本。很明显更加灵活方便。

## 练习17.8

> 在本节最后一段代码中，如果我们将`Sales_data()`作为第三个参数传递给`accumulate`，会发生什么？

解：

结果是0，以为`Sales_data`是默认初始化的。

## 练习17.9

> 解释下列每个`bitset` 对象所包含的位模式：

```cpp
(a) bitset<64> bitvec(32);
// 0000000000000000000000000000000000000000000000000000000000100000
(b) bitset<32> bv(1010101);
// 00000000000011110110100110110101
(c) string bstr; cin >> bstr; bitset<8> bv(bstr);
// 根据输入的str转换成bitset
```

## 练习17.10

> 使用序列1、2、3、5、8、13、21初始化一个`bitset`，将这些位置置位。对另一个`bitset`进行默认初始化，并编写一小段程序将其恰当的位置位。

解：

```cpp
#include <iostream>
#include <bitset>
#include <vector>

int main()
{
    std::vector<int> v = { 1, 2, 3, 5, 8, 13, 21 };
    std::bitset<32> bset;

    for (auto i : v)    bset.set(i);

    std::bitset<32> bset2;
    for (unsigned i = 0; i != 32; ++i)
        bset2[i] = bset[i];

    std::cout <<bset <<std::endl;
    std::cout <<bset2<<std::endl;
}
```

## 练习17.11

> 定义一个数据结构，包含一个整型对象，记录一个包含10个问题的真/假测验的解答。如果测验包含100道题，你需要对数据结构做出什么改变（如果需要的话）？

解：

```cpp
#include <iostream>
#include <bitset>
#include <utility>
#include <string>
#include <iostream>

//class Quiz
template<std::size_t N>
class Quiz
{
public:
    //constructors
    Quiz() = default;
    Quiz(std::string& s) :bitquiz(s){ }

    //generate grade
    template<std::size_t M>
    friend std::size_t grade(Quiz<M> const&, Quiz<M> const&);

    //print
    template<std::size_t M>
    friend std::ostream& operator<<(std::ostream&, Quiz<M> const&);

    //update bitset
    void update(std::pair<std::size_t, bool>);
private:
    std::bitset<N> bitquiz;
};
#endif

template<std::size_t N>
void Quiz<N>::update(std::pair<std::size_t, bool> pair)
{
    bitquiz.set(pair.first, pair.second);
}

template<std::size_t M>
std::ostream& operator<<(std::ostream& os, Quiz<M> const& quiz)
{
    os << quiz.bitquiz;
    return os;
}

template<std::size_t M>
std::size_t grade(Quiz<M> const& corAns, Quiz<M> const& stuAns)
{
    auto result = stuAns.bitquiz ^ corAns.bitquiz;
    result.flip();
    return result.count();
}


int main()
{
    //Ex17_11
    std::string s = "1010101";
    Quiz<10> quiz(s);
    std::cout << quiz << std::endl;

    //EX17_12
    quiz.update(std::make_pair(1, true));
    std::cout << quiz << std::endl;

    //Ex17_13
    std::string answer = "10011";
    std::string stu_answer = "11001";
    Quiz<5> ans(answer), stu_ans(stu_answer);
    std::cout << grade(ans, stu_ans) << std::endl;

    return 0;
}
```

## 练习17.12

> 使用前一题中的数据结构，编写一个函数，它接受一个问题编号和一个表示真/假解答的值，函数根据这两个参数更新测验的解答。

解：

参考17.11。

## 练习17.13

> 编写一个整型对象，包含真/假测验的正确答案。使用它来为前两题中的数据结构生成测验成绩。

解：

参考17.11。

## 练习17.14

> 编写几个正则表达式，分别触发不同错误。运行你的程序，观察编译器对每个错误的输出。

解：

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std::endl;

#include <string>
using std::string;

#include <regex>
using std::regex;
using std::regex_error;

int main()
{
    // for ex17.14
    // error_brack
    try{
        regex r("[[:alnum:]+\\.(cpp|cxx|cc)$", regex::icase);
    }
    catch(regex_error e)
    {
        cout << e.what() << " code: " << e.code() << endl;
    }

    // for ex17.15
    regex r("[[:alpha:]]*[^c]ei[[:alpha:]]*", regex::icase);
    string s;
    cout << "Please input a word! Input 'q' to quit!" << endl;
    while(cin >> s && s != "q")
    {
        if(std::regex_match(s, r))
            cout << "Input word " << s << " is okay!" << endl;
        else
            cout << "Input word " << s << " is not okay!" <<endl;

        cout << "Please input a word! Input 'q' to quit!" << endl;
    }

    cout << endl;

    // for ex17.16
    r.assign("[^c]ei", regex::icase);
    cout << "Please input a word! Input 'q' to quit!" << endl;
    while(cin >> s && s != "q")
    {
        if(std::regex_match(s, r))
            cout << "Input word " << s << " is okay!" << endl;
        else
            cout << "Input word " << s << " is not okay!" <<endl;

        cout << "Please input a word! Input 'q' to quit!" << endl;
    }

    return 0;
}
```

## 练习17.15

> 编写程序，使用模式查找违反“i在e之前，除非在c之后”规则的单词。你的程序应该提示用户输入一个单词，然后指出此单词是否符号要求。用一些违反和未违反规则的单词测试你的程序。

解：

参考17.14。

## 练习17.16

> 如果前一题程序中的`regex`对象用`"[^c]ei"`进行初始化，将会发生什么？用此模式测试你的程序，检查你的答案是否正确。

解：

参考17.14。

## 练习17.17

> 更新你的程序，令它查找输入序列中所有违反"ei"语法规则的单词。

解：

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std::endl;

#include <string>
using std::string;

#include <regex>
using std::regex;
using std::sregex_iterator;

int main()
{
	string s;
	cout << "Please input a sequence of words:" << endl;
	getline(cin, s);
	cout << endl;
	cout << "Word(s) that violiate the \"ei\" grammar rule:" << endl;
	string pattern("[^c]ei");
	pattern = "[[:alpha:]]*" + pattern + "[[:alpha:]]*";
	regex r(pattern, regex::icase);
	for (sregex_iterator it(s.begin(), s.end(), r), end_it; it != end_it; ++it)
		cout << it->str() << endl;

	return 0;
}
```

## 练习17.18

> 修改你的程序，忽略包含“ei`但并非拼写错误的单词，如“albeit”和“neighbor”。

解：

参考17.17。

## 练习17.19

> 为什么可以不先检查`m[4]`是否匹配了就直接调用`m[4].str()`？

解：

如果不匹配，则`m[4].str()`返回空字符串。

## 练习17.20

> 编写你自己版本的验证电话号码的程序。

解：

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std::endl;

#include <string>
using std::string;

#include <regex>
using std::regex;
using std::sregex_iterator;
using std::smatch;

bool valid(const smatch& m);

int main()
{
	string phone = "(\\()?(\\d{ 3 })(\\))?([-. ])?(\\d{ 3 })([-. ]?)(\\d{ 4 })";
	regex r(phone);
	smatch m;
	string s;
	bool valid_record;
	// read each record from the input file
	while (getline(cin, s))
	{
		valid_record = false;
		// for each matching phone number
		for (sregex_iterator it(s.begin(), s.end(), r), end_it; it != end_it; ++it)
		{
			valid_record = true;
			// check whether the number's formatting is valid
			if (valid(*it))
				cout << "valid phone number: " << it->str() << endl;
			else
				cout << "invalid phone number: " << it->str() << endl;
		}

		if (!valid_record)
			cout << "invalid record!" << endl;
	}
	return 0;
}

bool valid(const smatch& m)
{
	// if there is an open parenthesis before the area code
	if (m[1].matched)
		// the area code must be followed by a close parenthesis
		// and followed immediately by the rest of the number or a space
		return m[3].matched && (m[4].matched == 0 || m[4].str() == " ");
	else
		// then there can't be a close after the area code
		// the delimiters between the other two components must match
		return !m[3].matched && m[4].str() == m[6].str();
}
```

## 练习17.21

> 使用本节定义的`valid` 函数重写8.3.2节中的电话号码程序。

解：

```cpp
#include <iostream>
using std::cerr;
using std::cout;
using std::cin;
using std::endl;
using std::istream;
using std::ostream;

#include <fstream>
using std::ifstream;
using std::ofstream;

#include <sstream>
using std::istringstream;
using std::ostringstream;

#include <string>
using std::string;

#include <vector>
using std::vector;

#include <regex>
using std::regex;
using std::sregex_iterator;
using std::smatch;

struct PersonInfo
{
    string name;
    vector<string> phones;
};

bool valid(const smatch& m);
bool read_record(istream& is, vector<PersonInfo>& people);
void format_record(ostream& os, const vector<PersonInfo>& people);

// fake function that makes the program compile
string format(const string &num) { return num; }

int main()
{
    vector<PersonInfo> people;

    string filename;
    cout << "Please input a record file name: ";
    cin >> filename;
    cout << endl;
    ifstream fin(filename);

    if (read_record(fin, people))
    {
        ofstream fout("data\\result.txt", ofstream::trunc);
        format_record(fout, people);
    }
    else
    {
        cout << "Fail to open file " << filename << endl;
    }

    return 0;
}

bool valid(const smatch& m)
{
    // if there is an open parenthesis before the area code
    if (m[1].matched)
        // the area code must be followed by a close parenthesis
        // and followed immediately by the rest of the number or a space
        return m[3].matched && (m[4].matched == 0 || m[4].str() == " ");
    else
        // then there can't be a close after the area code
        // the delimiters between the other two components must match
        return !m[3].matched && m[4].str() == m[6].str();
}

bool read_record(istream& is, vector<PersonInfo>& people)
{
    if (is)
    {
        string line, word; // will hold a line and word from input, respectively
                           // read the input a line at a time until cin hits end-of-file (or another error)
        while (getline(is, line))
        {
            PersonInfo info; // create an object to hold this record's data
            istringstream record(line); // bind record to the line we just read
            record >> info.name; // read the name
            while (record >> word) // read the phone numbers
                info.phones.push_back(word); // and store them
            people.push_back(info); // append this record to people
        }
        return true;
    }
    else
        return false;
}

void format_record(ostream& os, const vector<PersonInfo>& people)
{
    string phone = "(\\()?(\\d{ 3 })(\\))?([-. ])?(\\d{ 3 })([-. ]?)(\\d{ 4 })";
    regex r(phone);
    smatch m;

    for (const auto &entry : people)
    {
        // for each entry in people
        ostringstream formatted, badNums; // objects created on each loop
        for (const auto &nums : entry.phones)
        {
            for (sregex_iterator it(nums.begin(), nums.end(), r), end_it; it != end_it; ++it)
            {
                // for each number
                // check whether the number's formatting is valid
                if (!valid(*it))
                    // string in badNums
                    badNums << " " << nums;
                else
                    // "writes" to formatted's string
                    formatted << " " << format(nums);
            }
        }

        if (badNums.str().empty()) // there were no bad numbers
            os << entry.name << " " // print the name
            << formatted.str() << endl; // and reformatted numbers
        else // otherwise, print the name and bad numbers
            cerr << "input error: " << entry.name
            << " invalid number(s) " << badNums.str() << endl;
    }
}
```

## 练习17.22

> 重写你的电话号码程序，使之允许在号码的三个部分之间放置任意多个空白符。


解：

参考17.21。

## 练习17.23

> 编写查找邮政编码的正则表达式。一个美国邮政编码可以由五位或九位数字组成。前五位数字和后四位数字之间可以用一个短横线分隔。

解：

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std::endl;

#include<string>
using std::string;

#include <regex>
using std::regex;
using std::sregex_iterator;
using std::smatch;

bool valid(const smatch& m);

int main()
{
	string zipcode =
		"(\\d{5})([-])?(\\d{4})?\\b";
	regex r(zipcode);
	smatch m;
	string s;
	
	while (getline(cin, s))
	{

		//! for each matching zipcode number
		for (sregex_iterator it(s.begin(), s.end(), r), end_it;
			it != end_it; ++it)
		{
			//! check whether the number's formatting is valid
			if (valid(*it))
				cout << "valid zipcode number: " << it->str() << endl;
			else
				cout << "invalid zipcode number: " << s << endl;
		}

	}
	return 0;
}

bool valid(const smatch& m)
{
	
	if ((m[2].matched)&&(!m[3].matched))
		return false;
	else
		return true;
}
```

## 练习17.24

> 编写你自己版本的重拍电话号码格式的程序。

解：

```cpp
#include <iostream>
#include <regex>
#include <string>

using namespace std;


string pattern = "(\\()?(\\d{3})(\\))?([-. ])?(\\d{3})([-. ])?(\\d{4})";
string format = "$2.$5.$7";
regex r(pattern);
string s;

int main()
{
    while(getline(cin,s))
    {
        cout<<regex_replace(s,r,format)<<endl;
    }

    return 0;
}
```

## 练习17.25

> 重写你的电话号码程序，使之只输出每个人的第一个电话号码。

解：

```cpp
#include <iostream>
#include <regex>
#include <string>

using namespace std;

string pattern = "(\\()?(\\d{3})(\\))?([-. ])?(\\d{3})([-. ])?(\\d{4})";
string fmt = "$2.$5.$7";
regex r(pattern);
string s;

int main()
{
    while(getline(cin,s))
    {
        smatch result;
        regex_search(s,result,r);
        if(!result.empty())
        {
        cout<<result.prefix()<<result.format(fmt)<<endl;
        }
        else
        {
            cout<<"Sorry, No match."<<endl;
        }
    }

    return 0;
}
```

## 练习17.26

> 重写你的电话号码程序，使之对多于一个电话号码的人只输出第二个和后续号码。

解：

略

## 练习17.27

> 编写程序，将九位数字邮政编码的格式转换为 `ddddd-dddd`。

解：

```cpp
#include <iostream>
#include <regex>
#include <string>

using namespace std;

string pattern = "(\\d{5})([.- ])?(\\d{4})";
string fmt = "$1-$3";

regex r(pattern);
string s;


int main()
{
    while(getline(cin,s))
    {
        smatch result;
        regex_search(s,result, r);

        if(!result.empty())
        {
            cout<<result.format(fmt)<<endl;
        }
        else
        {
            cout<<"Sorry, No match."<<endl;
        }

    }
    return 0;
}
```

## 练习17.28

> 编写函数，每次调用生成并返回一个均匀分布的随机`unsigned int`。

解：

```cpp
#include <iostream>
#include <random>
#include<string>

// default version
unsigned random_gen();
// with seed spicified
unsigned random_gen(unsigned seed);
// with seed and range spicified
unsigned random_gen(unsigned seed, unsigned min, unsigned max);
int main()
{
    std::string temp;
    while(std::cin >> temp)
    std::cout << std::hex << random_gen(19, 1, 10) << std::endl;
    return 0;
}

unsigned random_gen()
{
    static std::default_random_engine e;
    static std::uniform_int_distribution<unsigned> ud;
    return ud(e);
}

unsigned random_gen(unsigned seed)
{
    static std::default_random_engine e(seed);
    static std::uniform_int_distribution<unsigned> ud;
    return ud(e);
}

unsigned random_gen(unsigned seed, unsigned min, unsigned max)
{
    static std::default_random_engine e(seed);
    static std::uniform_int_distribution<unsigned> ud(min, max);
    return ud(e);
}
```

## 练习17.29

> 修改上一题中编写的函数，允许用户提供一个种子作为可选参数。

解：

参考17.28。

## 练习17.30

> 再次修改你的程序，此次增加两个参数，表示函数允许返回的最小值和最大值。

解：

参考17.28。

## 练习17.31

> 对于本节中的游戏程序，如果在`do`循环内定义`b`和`e`，会发生什么？

解：

由于引擎返回相同的随机数序列，因此眉不循环都会创建新的引擎，眉不循环都会生成相同的值。

## 练习17.32

> 如果我们在循环内定义`resp`，会发生什么？

解：

会报错，`while`条件中用到了`resp`。

## 练习17.33

> 修改11.3.6节中的单词转换程序，允许对一个给定单词有多种转换方式，每次随机选择一种进行实际转换。

解：

```cpp
#include <iostream>
using std::cout;
using std::endl;

#include <fstream>
using std::ifstream;

#include <string>
using std::string;

#include <vector>
using std::vector;

#include <random>
using std::default_random_engine;
using std::uniform_int_distribution;

#include <ctime>
using std::time;

#include <algorithm>
using std::sort;
using std::find_if;

#include <utility>
using std::pair;


int main() {
	typedef pair<string, string> ps;
	ifstream i("d.txt");
	vector<ps> dict;
	string str1, str2;
	// read wirds from dictionary
	while (i >> str1 >> str2) {
		dict.emplace_back(str1, str2);
	}
	i.close();
	// sort words in vector
	sort(dict.begin(), dict.end(), [](const ps &_ps1, const ps &_ps2){ return _ps1.first < _ps2.first; });
	i.open("i.txt");
	default_random_engine e(unsigned int(time(0)));
	// read words from text
	while (i >> str1) {
	  // find word in dictionary
		vector<ps>::const_iterator it = find_if(dict.cbegin(), dict.cend(),
		  [&str1](const ps &_ps){ return _ps.first == str1; });
		// if word doesn't exist in dictionary
		if (it == dict.cend()) {
		  // write it itself
			cout << str1 << ' ';
		}
		else {
		  // get random meaning of word 
			uniform_int_distribution<unsigned> u (0, find_if(dict.cbegin(), dict.cend(),
			 [&str1](const ps &_ps){ return _ps.first > str1; }) - it - 1);
			// write random meaning
			cout << (it + u(e))->second << ' ';
		}
	}

	return 0;
}
```

## 练习17.34

> 编写一个程序，展示如何使用表17.17和表17.18中的每个操作符。

解：

略

## 练习17.35

> 修改第670页中的程序，打印2的平方根，但这次打印十六进制数字的大写形式。

解：

```cpp

#include <iostream>
#include<iomanip>
#include <math.h>
using namespace std;

int main()
{
	cout <<"default format: " << 100 * sqrt(2.0) << '\n'
		<< "scientific: " << scientific << 100 * sqrt(2.0) << '\n'
		<< "fixed decimal: " << fixed << 100 * sqrt(2.0) << '\n'
		<< "hexidecimal: " << uppercase << hexfloat << 100 * sqrt(2.0) << '\n'
		<< "use defaults: " << defaultfloat << 100 * sqrt(2.0)
		<< "\n\n";
}

//17.36
//Modify the program from the previous exercise to print the various floating-point values so that they line up in a column.
#include <iostream>
#include<iomanip>
#include <math.h>
using namespace std;

int main()
{
	cout <<left<<setw(15) << "default format:" <<setw(25)<< right<< 100 * sqrt(2.0) << '\n'
	<< left << setw(15) << "scientific:" << scientific << setw(25) << right << 100 * sqrt(2.0) << '\n'
	<< left << setw(15) << "fixed decimal:" << setw(25) << fixed << right << 100 * sqrt(2.0) << '\n'
	<< left << setw(15) << "hexidecimal:" << setw(25) << uppercase << hexfloat << right << 100 * sqrt(2.0) << '\n'
	<< left << setw(15) << "use defaults:" << setw(25) << defaultfloat << right << 100 * sqrt(2.0)
	<< "\n\n";
}
```

## 练习17.36

> 修改上一题中的程序，打印不同的浮点数，使它们排成一列。

解：

参考17.36。

## 练习17.37

> 用未格式化版本的`getline` 逐行读取一个文件。测试你的程序，给定一个文件，既包含空行又包含长度超过你传递给`geiline`的字符数组大小的行。

解：

```cpp
//17.37
//Use the unformatted version of getline to read a file a line at a time.
//Test your program by giving it a file that contains empty lines as well as lines that are
//longer than the character array that you pass to getline.

#include <iostream>
#include <fstream>
#include <iomanip>

using namespace std;

//int main () {
//  ifstream myfile("F:\\Git\\Cpp-Primer\\ch17\\17_37_38\\test.txt");
//  if (myfile) cout << 1 << endl;
//  char sink [250];
//
//  while(myfile.getline(sink,250))
//  {
//    cout << sink << endl;
//  }
//  return 0;
//}

//17.38
//Extend your program from the previous exercise to print each word you read onto its own line.

//#include <iostream>
//#include <fstream>
//#include <iomanip>
//
//using namespace std;
//
//int main () {
//  ifstream myfile ("F:\\Git\\Cpp-Primer\\ch17\\17_37_38\\test.txt");
//  char sink [250];
//
//  while(myfile.getline(sink,250,' '))
//  {
//    cout << sink << endl;
//  }
//  return 0;
//}


int main()
{
	std::cout << "Standard Output!\n";
	std::cerr << "Standard Error!\n";
	std::clog << "Standard Log??\n";
}
```

## 练习17.38

> 扩展上一题中你的程序，将读入的每个单词打印到它所在的行。

解：

参考17.37。

## 练习17.39

> 对本节给出的 `seek`程序，编写你自己的版本。

解：


略

# 第18章 : 用于大型程序的工具

## 练习18.1

> 在下列 `throw` 语句中异常对象的类型是什么？
```cpp
(a) range_error r("error");
	throw r;
(b) exception *p = &r;
	throw *p;
```

解：

- (a): `range_error`
- (b): `exception`


## 练习18.2

> 当在指定的位置发生了异常时将出现什么情况？
```cpp
void exercise(int *b, int *e)
{
	vector<int> v(b, e);
	int *p = new int[v.size()];
	ifstream in("ints");
	//此处发生异常
}
```

解：

指针`p`指向的内容不会被释放，将造成内存泄漏。

## 练习18.3

> 要想让上面的代码在发生异常时能正常工作，有两种解决方案。请描述这两种方法并实现它们。

解：

方法一：不使用指针，使用对象：

```cpp
struct intArray
{
    intArray() : p(nullptr) { }
    explicit    intArray(std::size_t s):
        p(new int[s])       { }


    ~intArray()
    {
        delete[] p;
    }

    // data meber
    int *p;
};

intArray p(v.size());
```

方法二：使用智能指针：

```cpp
std::shared_ptr<int> p(new int[v.size()], [](int *p) { delete[] p; });
```

## 练习18.4

> 查看图18.1所示的继承体系，说明下面的 `try` 块有何错误并修改它。
```cpp
try {
	// 使用 C++ 标准库
} catch (exception) {
	// ...
} catch (const runtime_error &re) {
	// ...
} catch (overflow_error eobj) { /* ... */ }
```

解：

细化的异常类型应该写在前面：

```cpp
try {
	// 使用 C++ 标准库
} catch (overflow_error eobj) {
	// ...
} catch (const runtime_error &re) {
	// ...
} catch (exception) { /* ... */ }
```

## 练习18.5

> 修改下面的`main`函数，使其能捕获图18.1所示的任何异常类型：
```cpp
int main(){
	// 使用 C++标准库
}
```
处理代码应该首先打印异常相关的错误信息，然后调用 `abort` 终止函数。

解：

略

## 练习18.6

> 已知下面的异常类型和 `catch` 语句，书写一个 `throw` 表达式使其创建的异常对象能被这些 `catch` 语句捕获：
```cpp
(a) class exceptionType { };
	catch(exceptionType *pet) { }
(b) catch(...) { }
(c) typedef int EXCPTYPE;
	catch(EXCPTYPE) { }
```

解：

```cpp
(a): throw exceptionType();
(b): throw expection();
(c): EXCPTYPE e = 1; throw e;
```

## 练习18.7

> 根据第16章的介绍定义你自己的 `Blob` 和 `BlobPtr`，注意将构造函数写成函数`try`语句块。

解：

略

## 练习18.8

> 回顾你之前编写的各个类，为它们的构造函数和析构函数添加正确的异常说明。如果你认为某个析构函数可能抛出异常，尝试修改代码使得该析构函数不会抛出异常。

解：

略

## 练习18.9

> 定义本节描述的书店程序异常类，然后为 `Sales_data` 类重新编写一个复合赋值运算符并令其抛出一个异常。

## 练习18.10

> 编写程序令其对两个 `ISBN` 编号不相同的对象执行 `Sales_data` 的加法运算。为该程序编写两个不同的版本：一个处理异常，另一个不处理异常。观察并比较这两个程序的行为，用心体会当出现了一个未被捕获的异常时程序会发生什么情况。

解：

略

## 练习18.11

> 为什么 `what` 函数不应该抛出异常？

解：

略

## 练习18.12

> 将你为之前各章练习编写的程序放置在各自的命名空间中。也就是说，命名空间chapter15包含`Query`程序的代码，命名空间chapter10包含`TextQuery`的代码；使用这种结构重新编译`Query`代码实例。

解：

略

## 练习18.13

> 什么时候应该使用未命名的命名空间？

解：

需要定义一系列静态的变量的时候。

参考：https://stackoverflow.com/questions/154469/unnamed-anonymous-namespaces-vs-static-functions

## 练习18.14

> 假设下面的 `operator*` 声明的是嵌套的命名空间 `mathLib::MatrixLib` 的一个成员：
```cpp
namespace mathLib {
	namespace MatrixLib {
		class matrix { /* ... */ };
		matrix operator* (const matrix &, const matrix &);
		// ...
	}
}
```
请问你应该如何在全局作用域中声明该运算符？

解：

```cpp
mathLib::MatrixLib::matrix mathLib::MatrixLib::operator* (const mathLib::MatrixLib::matrix &, const mathLib::MatrixLib::matrix &); 
```

## 练习18.15

> 说明 `using` 指示与 `using` 声明的区别。

解：

- 一条`using`声明语句一次只引入命名空间的一个成员。
- `using` 指示使得某个特定的命名空间中所有的名字都可见。

有点像python中的`import`:

```python
from lib import func
from lib import *
```

## 练习18.16

> 假定在下面的代码中标记为“位置1”的地方是对命名空间 Exercise 中所有成员的`using`声明，请解释代码的含义。如果这些`using`声明出现在“位置2”又会怎样呢？将`using`声明变为`using`指示，重新回答之前的问题。
```cpp
namespace Exercise {
	int ivar = 0;
	double dvar = 0;
	const int limit = 1000;
}
int ivar = 0;
//位置1
void main() {
	//位置2
	double dvar = 3.1416;
	int iobj = limit + 1;
	++ivar;
	++::ivar;
}
```

解：

略

## 练习18.17

> 实际编写代码检验你对上一题的回答是否正确。

解：

略

## 练习18.18

> 已知有下面的 `swap` 的典型定义，当 `mem1` 是一个 `string` 时程序使用 `swap` 的哪个版本？如果 `mem1` 是 `int` 呢？说明在这两种情况下名字查找的过程。
```cpp
void swap(T v1, T v2)
{
	using std::swap;
	swap(v1.mem1, v2.mem1);
	//交换类型的其他成员
}
```

解：

`std::swap`是一个模板函数，如果是`string`会找到`string`版本；反之如果是`int`会找到`int`版本。

## 练习18.19

> 如果对 `swap` 的调用形如 `std::swap(v1.mem1, v2.mem1)` 将会发生什么情况？

解：

会直接调用`std`版的`swap`，但对后面的调用无影响。

## 练习18.20

> 在下面的代码中，确定哪个函数与`compute`调用匹配。列出所有候选函数和可行函数，对于每个可行函数的实参与形参的匹配过程来说，发生了哪种类型转换？
```cpp
namespace primerLib {
	void compute();
	void compute(const void *);
}
using primerLib::compute;
void compute(int);
void compute(double, double = 3.4);
void compute(char*, char* = 0);
void f()
{
	compute(0);
}
```

解：

略

## 练习18.21

> 解释下列声明的含义，在它们当作存在错误吗？如果有，请指出来并说明错误的原因。

```cpp
(a) class CADVehicle : public CAD, Vehicle { ... };
(b) class DbiList : public List, public List { ... };
(c) class iostream : public istream, public ostream { ... };
```

## 练习18.22

> 已知存在如下所示的类的继承体系，其中每个类都定义了一个默认构造函数：

```cpp
class A { ... };
class B : public A { ... };
class C : public B { ... };
class X { ... };
class Y { ... };
class Z : public X, public Y { ... };
class MI : public C, public Z { ... };
```
对于下面的定义来说，构造函数的执行顺序是怎样的？
```cpp
MI mi;
```

## 练习18.23

> 使用练习18.22的继承体系以及下面定义的类 `D`，同时假定每个类都定义了默认构造函数，请问下面的哪些类型转换是不被允许的？
```cpp
class D : public X, public C { ... };
p *pd = new D;
(a) X *px = pd;
(b) A *pa = pd;
(c) B *pb = pd;
(d) C *pc = pd;
```

## 练习18.24

> 在第714页，我们使用一个指向 `Panda` 对象的 `Bear` 指针进行了一系列调用，假设我们使用的是一个指向 `Panda` 对象的 `ZooAnimal` 指针将会发生什么情况，请对这些调用语句逐一进行说明。

## 练习18.25

> 假设我们有两个基类 `Base1` 和 `Base2` ，它们各自定义了一个名为 `print` 的虚成员和一个虚析构函数。从这两个基类中文名派生出下面的类，它们都重新定义了 `print` 函数：
```cpp
class D1 : public Base1 { /* ... */};
class D2 : public Base2 { /* ... */};
class MI : public D1, public D2 { /* ... */};
```
通过下面的指针，指出在每个调用中分别使用了哪个函数：
```cpp
Base1 *pb1 = new MI;
Base2 *pb2 = new MI;
D1 *pd1 = new MI;
D2 *pd2 = new MI;
(a) pb1->print();
(b) pd1->print();
(c) pd2->print();
(d) delete pb2;
(e) delete pd1;
(f) delete pd2;
```

```cpp
struct Base1 {
	void print(int) const;
protected:
	int ival;
	double dval;
	char cval;
private:
	int *id;
};
struct Base2 {
	void print(double) const;
protected:
	double fval;
private:
	double dval;
};
struct Derived : public Base1 {
	void print(std::string) const;
protected:
	std::string sval;
	double dval;
};
struct MI : public Derived, public Base2 {
	void print(std::vector<double>);
protected:
	int *ival;
	std::vector<double> dvec;
};
```

## 练习18.26

> 已知如上所示的继承体系，下面对`print`的调用为什么是错误的？适当修改`MI`，令其对`print`的调用可以编译通过并正确执行。
```cpp
MI mi;
mi.print(42);
```

## 练习18.27

> 已知如上所示的继承体系，同时假定为MI添加了一个名为`foo`的函数：
```cpp
int ival;
double dval;
void MI::foo(double cval)
{
	int dval;
	//练习中的问题发生在此处
}
(a) 列出在MI::foo中可见的所有名字。
(b) 是否存在某个可见的名字是继承自多个基类的？
(c) 将Base1的dval成员与Derived 的dval 成员求和后赋给dval的局部实例。
(d) 将MI::dvec的最后一个元素的值赋给Base2::fval。
(e) 将从Base1继承的cval赋给从Derived继承的sval的第一个字符。
```

## 练习18.28

> 已知存在如下的继承体系，在 `VMI` 类的内部哪些继承而来的成员无须前缀限定符就能直接访问？哪些必须有限定符才能访问？说明你的原因。
```cpp
struct Base {
	void bar(int);
protected:
	int ival;
};
struct Derived1 : virtual public Base {
	void bar(char);
	void foo(char);
protected:
	char cval;
};
struct Derived2 : virtual public Base {
	void foo(int);
protected:
	int ival;
	char cval;
};
class VMI : public Derived1, public Derived2 { };
```

## 练习18.29

> 已知有如下所示的类继承关系：
```cpp
class Class { ... };
class Base : public Class { ... };
class D1 : virtual public Base { ... };
class D2 : virtual public Base { ... };
class MI : public D1, public D2 { ... };
class Final : public MI, public Class { ... };
(a) 当作用于一个Final对象时，构造函数和析构函数的执行次序分别是什么？
(b) 在一个Final对象中有几个Base部分？几个Class部分？
(c) 下面的哪些赋值运算符将造成编译错误？
Base *pb; Class *pc; MI *pmi; D2 *pd2;
(a) pb = new Class;
(b) pc = new Final;
(c) pmi = pb;
(d) pd2 = pmi;
```

## 练习18.30

> 在`Base`中定义一个默认构造函数、一个拷贝构造函数和一个接受`int`形参的构造函数。在每个派生类中分别定义这三种构造函数，每个构造函数应该使用它的形参初始化其`Base`部分。




## 练习19.1

> 使用 malloc 编写你自己的 operator new(sizt_t)函数，使用 free 编写operator delete(void *)函数。

## 练习19.2

> 默认情况下，allocator 类使用 operator new 获取存储空间，然后使用 operator delete 释放它。利用上一题中的两个函数重新编译并运行你的 StrVec 程序。

## 练习19.3

> 已知存在如下的类继承体系，其中每个类分别定义了一个公有的默认构造函数和一个析构函数：
```cpp
class A { /* ... */};
class B : public A { /* ... */};
class C : public B { /* ... */};
class D : public B, public A { /* ... */};
```
下面哪个 dynamic_cast 将失败？
```cpp
(a) A *pa = new C;
	B *pb = dynamic_cast<B*>(pa);
(b) B *pb = new B;
	C *pc = dynamic_cast<C*>(pb);
(c) A *pa = new D;
	B *pb = dynamic_cast<B*>(pa);
```

## 练习19.4

> 使用上一个练习定义的类改写下面的代码，将表达式*pa 转换成类型C&：
```cpp
if (C *pc = dynamic_cast<C*>(pa))
{
	//使用C的成员
} else {
	//使用A的成员
}
```

## 练习19.5

> 在什么情况下你应该用 dynamic_cast 替代虚函数？

## 练习19.6

> 编写一条表达式将 Query_base 指针动态转换为 AndQuery 指针。分别使用 AndQuery 的对象以及其他类型的对象测试转换是否有效。打印一条表示类型转换是否成功的信息，确保实际输出的结果与期望的一致。

## 练习19.7

> 编写与上一个练习类似的转换，这一次将 Query_base 对象转换为 AndQuery 的引用。重复上面的测试过程，确保转换能正常工作。

## 练习19.8

> 编写一条 typeid 表达式检查两个 Query_base 对象是否指向同一种类型。再检查该类型是否是 AndQuery。

## 练习19.9

> 编写与本节最后一个程序类似的代码，令其打印你的编译器为一些常见类型所起的名字。如果你得到的输出结果与本书类似，尝试编写一个函数将这些字符串翻译成人们更容易读懂的形式。

## 练习19.10

> 已知存在如下的类继承体系，其中每个类定义了一个默认公有的构造函数和一个虚析构函数。下面的语句将打印哪些类型名字？
```cpp
class A { /* ... */ };
class B : public A { /* ... */ };
class C : public B { /*...*/ };
(a) A *pa = new C;
	cout << typeid(pa).name() << endl;
(b) C cobj;
	A& ra = cobj;
	cout << typeid(&ra).name() << endl;
(c) B *px = new B;
	A& ra = *px;
	cout << typeid(ra).name() << endl;
```

## 练习19.11

> 普通的数据指针和指向数据成员的指针有何区别？

## 练习19.12

> 定义一个成员指针，令其可以指向 Screen 类的 cursor 成员。通过该指针获得 Screen::cursor 的值。

## 练习19.13

> 定义一个类型，使其可以表示指向 Sales_data 类的 bookNo 成员的指针。

## 练习19.14

> 下面的代码合法吗？如果合法，代码的含义是什么？如果不合法，解释原因。
```cpp
auto pmf = &Screen::get_cursor;
pmf = &Screen::get;
```

## 练习19.15

> 普通函数指针和指向成员函数的指针有何区别？

## 练习19.16

> 声明一个类型别名，令其作为指向 Sales_data 的 avg_price 成员的指针的同义词。

## 练习19.17

> 为 Screen 的所有成员函数类型各定义一个类型别名。

## 练习19.18

> 编写一个函数，使用 count_if 统计在给定的 vector 中有多少个空 string。

## 练习19.19

> 编写一个函数，令其接受vector<Sales_data>并查找平均价格高于某个值的第一个元素。

## 练习19.20

> 将你的 QueryResult 类嵌套在 TextQuery 中，然后重新运行12.3.2节中使用了 TextQuery 的程序。

## 练习19.21

> 编写你自己的 Token 类。

## 练习19.22

> 为你的 Token 类添加一个 Sales_data 类型的成员。

## 练习19.23

> 为你的 Token 类添加移动构造函数和移动赋值运算符。

## 练习19.24

> 如果我们将一个 Token 对象付给它自己将发生什么情况？

## 练习19.25

> 编写一系列赋值运算符，令其分别接收 union 中各种类型的值。

## 练习19.26

> 说明下列声明语句的含义并判断它们是否合法：
```cpp
extern "C" int compute(int *, int);
extern "C" double compute(double *, double);
```