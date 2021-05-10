## 1 tricks 

[c++ 那些事](https://github.com/Light-City/CPlusPlusThings)

[万字长文把 VSCode 打造成 C++ 开发利器](https://zhuanlan.zhihu.com/p/96819625)

[适合 C++ 新手学习的开源项目](https://zhuanlan.zhihu.com/p/273682109)

[C++的三个阶段](https://www.zhihu.com/people/lin-qi-53-1)

[c++ 之 std::move 原理实现与用法总结](https://blog.csdn.net/p942005405/article/details/84644069)

[微软C++ 参考 ](https://docs.microsoft.com/zh-cn/cpp/cpp/cpp-language-reference?view=msvc-160)

[json for modern c++的使用](https://blog.csdn.net/fengxinlinux/article/details/71037244)

[glog](https://github.com/google/glog)

[cnpy](https://github.com/rogersce/cnpy)

[C++ 多线程教程](https://github.com/sprinfall/cpp-thread-study)

[C++11多线程并发基础入门教程](https://zhuanlan.zhihu.com/p/194198073)


https://github.com/fightingwangzq/cpp-learning

https://github.com/forthespada/InterviewGuide


## 2 notes：  

### 1 使用struct关键字和class关键字定义类以及在类的继承方面有啥区别？

（1）定义类的差别：

C语言中的struct 关键字也可以实现类，用class 关键字和struct 关键字定义类的唯一差别就在于默认访问级别不同：

默认情况下，struct 成员的访问级别为public，而class 成员的访问级别是private 。语法使用方面都是相同，直接将class 换成struct 即可。

（2）类的继承的差别：

使用class 关键字定义的类，它的派生类默认具有private 继承，而使用struct 关键字定义的类，它的派生类默认具有public 继承。其他的方面没有区别。

因此，主要就两个区别：默认的访问级别和默认的继承级别：class 都是private的，struct 都是public的。

（3） 结构体是一种值类型，而类是引用类型。值类型用于存储数据的值，引用类型用于存储对实际数据的引用。那么结构体就是当成值来使用的，类则通过引用来对实际数据操作。

结构体初始化问题：

https://blog.csdn.net/ericbar/article/details/79567108

C语言没有构造函数的概念，如同内建类型的变量：

1.定义全局或静态的struct变量时，其成员会初始化为零。
2.定义局部的struct变量时，其成员为不确定的值。

C语言对struct(以及union和数组)变量使用初始化列表。

[C语言结构体初始化的四种方法](https://blog.csdn.net/ericbar/article/details/79567108)

### 2 多线程 

https://blog.csdn.net/xibeichengf/article/details/71173543

**创建线程**

join: 当thread::join()函数被调用后，调用它的线程会被block，直到线程的执行被完成

detach: 当thread::detach()函数被调用后，执行的线程从线程对象中被分离，已不再被一个线程对象所表达--这是两个独立的事情。

```c++
      int main() {
        std::thread t1(Counter(3));
        t1.join();

        std::thread t2(Counter(3));
        t2.detach();

        // 等待几秒，不然 t2 根本没机会执行。
        std::this_thread::sleep_for(std::chrono::seconds(4));

        return 0;
      }
```

**Mutex（互斥锁）**

多个线程访问同一资源时

Mutex 1：

直接操作 mutex，即直接调用 mutex 的 lock / unlock 函数。

```c++
std::mutex g_mutex;
int g_count = 0;

void Counter() {
  g_mutex.lock();

  int i = ++g_count;
  std::cout << "count: " << i << std::endl;

  // 前面代码如有异常，unlock 就调不到了。
  g_mutex.unlock();
}

  // 创建一组线程。
  std::vector<std::thread> v;
  v.reserve(SIZE);

  for (std::size_t i = 0; i < SIZE; ++i) {
    v.emplace_back(&Counter);
  }

  // 等待所有线程结束。
  for (std::thread& t : v) {
    t.join();
  }
```

Mutex 2:

使用 lock_guard 自动加锁、解锁。原理是 RAII，和智能指针类似。

```cpp
std::mutex g_mutex;
int g_count = 0;

void Counter() {
  // lock_guard 在构造函数里加锁，在析构函数里解锁。
  std::lock_guard<std::mutex> lock(g_mutex);

  int i = ++g_count;
  std::cout << "count: " << i << std::endl;
}
```

Mutex 3:

使用 unique_lock 自动加锁、解锁。 unique_lock 与 lock_guard 原理相同，但是提供了更多功能（比如可以结合条件变量使用）。 注意：mutex::scoped_lock 其实就是 unique_lock<mutex> 的 typedef。


```cpp
#include <iostream>
#include <mutex>
#include <thread>
#include <vector>

std::mutex g_mutex;
int g_count = 0;

void Counter() {
  std::unique_lock<std::mutex> lock(g_mutex);

  int i = ++g_count;
  std::cout << "count: " << i << std::endl;
}

int main() {
  const std::size_t SIZE = 4;

  std::vector<std::thread> v;
  v.reserve(SIZE);

  for (std::size_t i = 0; i < SIZE; ++i) {
    v.emplace_back(&Counter);
  }

  for (std::thread& t : v) {
    t.join();
  }

  return 0;
}
```

Mutex 4：

为输出流使用单独的 mutex。 这么做是因为 IO 流并不是线程安全的

```cpp
std::mutex g_mutex;
std::mutex g_io_mutex;
int g_count = 0;

void Counter() {
  int i;
  {
    std::unique_lock<std::mutex> lock(g_mutex);
    i = ++g_count;
  }

  {
    std::unique_lock<std::mutex> lock(g_io_mutex);
    std::cout << "count: " << i << std::endl;
  }
}
```

**条件变量**

条件变量（Condition Variable）的一般用法是：线程 A 等待某个条件并挂起，直到线程 B 设置了这个条件，并通知条件变量，然后线程 A 被唤醒。经典的「生产者-消费者」问题就可以用条件变量来解决。

这里等待的线程可以是多个，通知线程可以选择一次通知一个（notify_one）或一次通知所有（notify_all）。

```cpp
#include <thread>
#include <mutex>
#include <condition_variable>
      
两个线程共享的全局变量：

std::mutex mutex;
std::condition_variable cv;
std::string data;
bool ready = false;  // 条件
bool processed = false;  // 条件 

工作线程：

void Worker() {
  std::unique_lock<std::mutex> lock(mutex);

  // 等待主线程发送数据。
  cv.wait(lock, [] { return ready; });

  // 等待后，继续拥有锁。
  std::cout << "工作线程正在处理数据..." << std::endl;
  // 睡眠一秒以模拟数据处理。
  std::this_thread::sleep_for(std::chrono::seconds(1));
  data += " 已处理";

  // 把数据发回主线程。
  processed = true;
  std::cout << "工作线程通知数据已经处理完毕。" << std::endl;

  // 通知前，手动解锁以防正在等待的线程被唤醒后又立即被阻塞。
  lock.unlock();

  cv.notify_one();
}

主线程：


int main() {
  std::thread worker(Worker);

  // 把数据发送给工作线程。
  {
    std::lock_guard<std::mutex> lock(mutex);
    std::cout << "主线程正在准备数据..." << std::endl;
    // 睡眠一秒以模拟数据准备。
    std::this_thread::sleep_for(std::chrono::seconds(1));
    data = "样本数据";
    ready = true;
    std::cout << "主线程通知数据已经准备完毕。" << std::endl;
  }
  cv.notify_one();

  // 等待工作线程处理数据。
  {
    std::unique_lock<std::mutex> lock(mutex);
    cv.wait(lock, [] { return processed; });
  }
  std::cout << "回到主线程，数据 = " << data << std::endl;

  worker.join();

  return 0;
}
```



### 3 RAII   RTTI 

https://zhuanlan.zhihu.com/p/34660259

RAII (Resource Acquisition Is Initialization) 资源获取即初始化

构造函数的写法：

      person(const std::string name = "", int age = 0) : 
      name_(name), age_(age) {
            std::cout << "Init a person!" << std::endl;
      }
      ~person() {
            std::cout << "Destory a person!" << std::endl;
      }


RTTI（Run-Time Type Identification)-运行时类型识别

[C++ typeid关键字详解](https://blog.csdn.net/gatieme/article/details/50947821)

它使程序能够获取由基指针或引用所指向的对象的实际派生类型，即允许“用指向基类的指针或引用来操作对象”的程序能够获取到“这些指针或引用所指对象”的实际派生类型。

在C++中，为了支持RTTI提供了两个操作符：dynamic_cast和typeid

dynamic_cast允许运行时刻进行类型转换，从而使程序能够在一个类层次结构中安全地转化类型，与之相对应的还有一个非安全的转换操作符static_cast，因为这不是本文的讨论重点，所以这里不再详述，感兴趣的可以自行查阅资料。

typeid是C++的关键字之一，等同于sizeof这类的操作符。typeid操作符的返回结果是名为type_info的标准库类型的对象的引用（在头文件typeinfo中定义，稍后我们看一下vs和gcc库里面的源码），它的表达式有下图两种形式。
 
###  








