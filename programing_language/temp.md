
**C\C++中函数后面加const**

https://blog.csdn.net/SMF0504/article/details/52311207

1 前面使用const 表示返回值为const, 后面加 const表示函数不可以修改class的成员。  
2 非静态成员函数后面加const（加到非成员函数或静态成员后面会产生编译错误）。
3 加了const的成员函数可以被非const对象和const对象调用, 但不加const的成员函数只能被非const对象调用。



