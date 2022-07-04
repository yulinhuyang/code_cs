
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
