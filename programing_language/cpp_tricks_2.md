
**C\C++中函数后面加const**

https://blog.csdn.net/SMF0504/article/details/52311207

1 前面使用const 表示返回值为const, 后面加 const表示函数不可以修改class的成员。  
2 非静态成员函数后面加const（加到非成员函数或静态成员后面会产生编译错误）。
3 加了const的成员函数可以被非const对象和const对象调用, 但不加const的成员函数只能被非const对象调用。

**make_shared用法**

```cpp
#include <memory>
#include <iostream>

using namespace std;

int main() {

    std::shared_ptr<int> a = std::make_shared<int>(666);
    std::shared_ptr<int> b = a;
    std::shared_ptr<int> c = a;
    std::shared_ptr<int> d = a;
    cout << a.use_count() << endl;  // 4
    a.reset();
    cout << d.use_count() << endl; // 3 
}
```

size_t 反向循环，不能i >= 0,int的可以

explicit关键字用来修饰类的构造函数，被修饰的构造函数的类，不能发生相应的隐式类型转换，只能以显示的方式进行类型转换。

boost生成和解析json: https://blog.csdn.net/baidu_41388533/article/details/119714125

C++ tuple元组的基本用法(总结)：https://blog.csdn.net/sevenjoin/article/details/88420885

C++17 filesystem 文件系统: https://blog.csdn.net/qq_40946921/article/details/91394589

C++json解析库：   https://github.com/nlohmann/json

json库nlohmann简单使用教程: https://blog.csdn.net/u011341856/article/details/108797920

KD-Tree原理详解: https://zhuanlan.zhihu.com/p/112246942

【C++】关于 std::set_intersection( ) 函数用法: https://blog.csdn.net/Sim0Hayha/article/details/80043558

C++ 保存vector到文件： https://blog.csdn.net/BlackCarDriver/article/details/103690469

**为什么 STL 中的容器和算法都是用的左闭右开区间**

https://www.zhihu.com/question/61054439


**STL容器自定义哈希函数和比较函数**

C++ STL无序容器自定义哈希函数和比较规则：https://www.xinbaoku.com/archive/1Bu2FBHA.html

c++ unordered_set，unordered_map中自定义哈希函数： https://blog.csdn.net/qq_34525916/article/details/115896842

unordered_set 需要所选对象具有哈希函数，而 set 要求所选对象的 key 有比较大小的函数

```cpp
//无序容器
//将自定义类型作为unordered_map的键值，需如下两个步骤：
//1.定义自定义key的哈希函数的函数对象，告知此容器如何生成hash的值；
//2.定义等比函数的函数对象或者在自定义类里重载operator==()， 告知容器当出现hash冲突的时候，如何区分hash值相同的不同对象

auto hash_function = [](const PII& o) {return hash<int>()(o.first) ^ hash<int>()(o.second);};
unordered_set<PII, decltype(hash_function)> seen(0, hash_function);

//有序容器
struct myComp
{
	bool operator() (const int &a, const int &b)
	{
		return a > b;	//从大到小排序
		//return a < b;	//从小到大排序
	}
};
set<int, myComp> s1;
```



