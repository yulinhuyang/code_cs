
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

| -------- | ---------------------- |

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

