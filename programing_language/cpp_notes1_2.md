补 effective modern C++

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
 
