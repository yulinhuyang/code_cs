

基础议题（Basics） 

**条款1：仔细区别 pointers 和 references** 

Distinguish between pointers and references．

**条款2：最好使用 C++ 转型操作符**  

Prefer C++-style casts．

**条款3：绝对不要以多态（polymorphically）方式处理数组**  

Never treat arrays polymorphically．

**条款4：非必要不提供 default constructor**  

Avoid gratuitous default constructors．


操作符（Operators） 

**条款5：对定制的“类型转换函数”保持警觉**  

Be wary of user-defined conversion functions．

**条款6：区别 increment/decrement 操作符的** 

前置（prefix）和后置（postfix）形式 

Distinguish between prefix and postfix forms of increment  and decrement operators．

**条款7：千万不要重载＆＆，||和， 操作符**  

Never overload ＆＆， ||， or ，

**条款8：了解各种不同意义的 new 和 delete**  

Understand the different meanings of new and delete



异常（Exceptions） 

**条款9：利用 destructors 避免泄漏资源**  

Use destructors to prevent resource leaks．

**条款10：在 constructors 内阻止资源泄漏（resource leak）**  

Prevent resource leaks in constructors．

**条款11：禁止异常（exceptions）流出 destructors 之外**  

Prevent exceptions from leaving destructors．

**条款12：了解“抛出一个 exception”与“传递一个参数” 或“调用一个虚函数”之间的差异**  

Understand how throwing an exception differs from passing a parameter or calling a virtual function．

**条款13：以 by reference 方式捕捉 exceptions**  

Catch exceptions by reference．

**条款14：明智运用 exception specifications**  

Use exception specifications judiciously．

**条款15：了解异常处理（exception handling）的成本**  

Understand the costs of exception handling．



效率（Efficiency） 

**条款16：谨记 80-20 法则**  

Remember the 80-20 rule．

**条款17：考虑使用 lazy evaluation（缓式评估）**  

Consider using lazy evaluation．

**条款18：分期摊还预期的计算成本**  

Amortize the cost of expected computations．

**条款19：了解临时对象的来源**  

Understand the origin of temporary objects．

**条款20：协助完成“返回值优化（RVO）”**  

Facilitate the return value optimization．

**条款21：利用重载技术（overload）避免隐式类型转换（implict type conversions）** 

Overload to avoid implicit type conversions．

**条款22：考虑以操作符复合形式（op=）取代其独身形式（op）**  

Consider using op= instead of stand-alone op．



**条款23：考虑使用其他程序库**  

Consider alternative libraries．

**条款24：了解 virtual functions、multiple inheritance、virtual base classes、runtime type identification 的成本**  

Understand the costs of virtual functions， multiple inheritance，virtual base classes， and RTTI．



技术（Techniques， Idioms， Patterns） 

**条款25：将 constructor 和 non-member functions 虚化** 

Virtualizing constructors and non-member functions．

**条款26：限制某个 class 所能产生的对象数量**  

Limiting the number of objects of a class．

**条款27：要求（或禁止）对象产生于 heap 之中**  

Requiring or prohibiting heap-based objects．

**条款28：Smart Pointers（智能指针）**  

**条款29：Reference counting（引用计数）**  

**条款30：Proxy classes（替身类、代理类）**  

**条款31：让函数根据一个以上的对象类型来决定如何虚化**  

Making functions virtual with respect to more than one object．

杂项讨论（Miscellany） 

**条款32：在未来时态下发展程序**  

Program in the future tense．

**条款33：将非尾端类（non-leaf classes）设计为抽象类（abstract classes）**  

Make non-leaf classes abstract．

**条款34：如何在同一个程序中结合 C++ 和 C**  

Understand how to combine C++ and C in the same program．

**条款35：让自己习惯于标准 C++ 语言**  

Familiarize yourself with the language standard．

推荐读物 

auto_ptr 实现代码


