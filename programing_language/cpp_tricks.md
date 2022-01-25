### 1 tricks 

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


### 2  使用struct关键字和class关键字定义类以及在类的继承方面有啥区别？

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

### 3 多线程 

https://blog.csdn.net/xibeichengf/article/details/71173543

#### 创建线程

linux C++:

```cpp
//join: 当thread::join()函数被调用后，调用它的线程会被block，直到线程的执行被完成
//detach: 当thread::detach()函数被调用后，执行的线程从线程对象中被分离，已不再被一个线程对象所表达--这是两个独立的事情。
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
windows C++(函数不同一样)

[C语言多线程编程(一)](https://zhuanlan.zhihu.com/p/97418361)

```cpp
//pthread_create : 创建线程。
int pthread_create(pthread_t *tidp,const pthread_attr_t *attr, (void*)(*start_rtn)(void*),void *arg);
pthread_exit: 调用这个函数可以显示得退出线程

void  pthread_exit（void  *retval）;

pthread_join: 用来等待一个线程的结束,使一个线程等待另一个线程结束，主要于线程间同步的操作。不使用的话，该线程结束后并不会释放其内存空间，这会导致该线程变成了“僵尸线程”。

pthread_detach: 主线程与子线程分离，子线程结束后，资源自动回收。pthread_join()函数的替代函数.
```

####  Mutex（互斥锁） 

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

https://blog.csdn.net/fengbingchun/article/details/78638138

std::unique_lock对象以独占所有权的方式(unique owership)管理mutex对象的上锁和解锁操作，即在unique_lock对象的声明周期内，它所管理的锁对象会一直保持上锁状态；

而unique_lock的生命周期结束之后，它所管理的锁对象会被解锁。

std::unique_lock还支持同时锁定多个mutex，这避免了多道加锁时的资源”死锁”问题。

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

#### 条件变量 

条件变量（Condition Variable）的一般用法是：线程 A 等待某个条件并挂起，直到线程 B 设置了这个条件，并通知条件变量，然后线程 A 被唤醒。经典的「生产者-消费者」问题就可以用条件变量来解决。

这里等待的线程可以是多个，通知线程可以选择一次通知一个（notify_one）或一次通知所有（notify_all）。

https://blog.csdn.net/li1615882553/article/details/86179781

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

与条件变量搭配使用的「锁」，必须是 unique_lock，不能用 lock_guard。这个前面文章中已有说明。

等待前先加锁。等待时，如果条件不满足，wait 会原子性地解锁并把线程挂起。

条件变量被通知后，挂起的线程就被唤醒，但是唤醒也有可能是假唤醒，或者是因为超时等异常情况，所以被唤醒的线程仍要检查条件是否满足，所以 wait 是放在条件循环里面。

cv.wait(lock, [] { return ready; }); 相当于：while (!ready) { cv.wait(lock); }。

被声明为explicit的构造函数通常比其 non-explicit 兄弟更受欢迎, 因为它们禁止编译器执行非预期 (往往也不被期望) 的类型转换。

#### 线程池 

线程池就是首先创建一些线程，它们的集合称为线程池。使用线程池可以很好地提高性能，线程池在系统启动时即创建大量空闲的线程，程序将一个任务传给线程池，线程池就会启动一条线程来执行这个任务，执行结束以后，该线程并不会死亡，而是再次返回线程池中成为空闲状态，等待执行下一个任务。


基于 Asio 的线程池

```cpp
#include <functional>
#include <iostream>
#include <mutex>
#include <thread>
#include <vector>

#define BOOST_ASIO_NO_DEPRECATED
#include "boost/asio.hpp"


class ThreadPool {
public:
  explicit ThreadPool(std::size_t size)
      : work_guard_(boost::asio::make_work_guard(io_context_)) {
    workers_.reserve(size);
    for (std::size_t i = 0; i < size; ++i) {
      workers_.emplace_back(&boost::asio::io_context::run, &io_context_);
    }
  }

  ~ThreadPool() {
    io_context_.stop();

    for (auto& w : workers_) {
      w.join();
    }
  }

  // Add new work item to the pool.
  template<class F>
  void Enqueue(F f) {
    boost::asio::post(io_context_, f);
  }

private:
  std::vector<std::thread> workers_;
  boost::asio::io_context io_context_;

  typedef boost::asio::io_context::executor_type ExecutorType;
  boost::asio::executor_work_guard<ExecutorType> work_guard_;
};



class ThreadPool {
public:
  explicit ThreadPool(std::size_t size)
      : work_guard_(boost::asio::make_work_guard(io_context_)) {
    workers_.reserve(size);
    for (std::size_t i = 0; i < size; ++i) {
      workers_.emplace_back(&boost::asio::io_context::run, &io_context_);
    }
  }

  ~ThreadPool() {
    io_context_.stop();

    for (auto& w : workers_) {
      w.join();
    }
  }

  // Add new work item to the pool.
  template<class F>
  void Enqueue(F f) {
    boost::asio::post(io_context_, f);
  }

private:
  std::vector<std::thread> workers_;
  boost::asio::io_context io_context_;

  typedef boost::asio::io_context::executor_type ExecutorType;
  boost::asio::executor_work_guard<ExecutorType> work_guard_;
};

成员变量 work_guard_ 的作用是，让 io_context 即使在没有异步任务可执行时也保持运行（即 io_context::run 不返回）

```


线程池结构体


typedef struct threadpool_t {
	pthread_mutex_t lock;               /* 用于锁住本结构体 ，和条件变量一起使用 */    
	pthread_mutex_t thread_counter;     /* 记录忙状态线程个数的锁 -- busy_thr_num */
	pthread_cond_t queue_not_full;      /* 当任务队列满时，添加任务的线程阻塞，等待此条件变量 */
	pthread_cond_t queue_not_empty;     /* 任务队列里不为空时，通知线程池中等待任务的线程 */
 
	pthread_t *threads;                 /* 存放线程池中每个线程的tid。数组 */
	pthread_t adjust_tid;               /* 存管理线程tid */
	threadpool_task_t *task_queue;      /* 任务队列 */
 
	int min_thr_num;                    /* 线程池最小线程数 */
	int max_thr_num;                    /* 线程池最大线程数 */
	int live_thr_num;                   /* 当前存活线程个数 */
	int busy_thr_num;                   /* 忙状态线程个数 */
	int wait_exit_thr_num;              /* 要销毁的线程个数 */
 
	int queue_front;                    /* task_queue队头下标 */
	int queue_rear;                     /* task_queue队尾下标 */
	int queue_size;                     /* task_queue队中实际任务数 */
	int queue_max_size;                 /* task_queue队列可容纳任务数上限 */
 
	int shutdown;                       /* 标志位，线程池使用状态，true或false */
} threadpool_t;



####  生产者和消费者 

生产者 - 消费者（Producer-Consumer），也叫有限缓冲（Bounded-Buffer）

https://github.com/sprinfall/cpp-thread-study/blob/master/doc/CppConcurrency05.ProducerConsumer.md

```cpp

有限缓冲类

class BoundedBuffer {
public:
  BoundedBuffer(const BoundedBuffer& rhs) = delete;
  BoundedBuffer& operator=(const BoundedBuffer& rhs) = delete;

  BoundedBuffer(std::size_t size)
      : begin_(0), end_(0), buffered_(0), circular_buffer_(size) {
  }

  void Produce(int n) {
    {
      std::unique_lock<std::mutex> lock(mutex_);
      // 等待缓冲不为满。
      not_full_cv_.wait(lock, [=] { return buffered_ < circular_buffer_.size(); });

      // 插入新的元素，更新下标。
      circular_buffer_[end_] = n;
      end_ = (end_ + 1) % circular_buffer_.size();

      ++buffered_;
    }  // 通知前，自动解锁。

    // 通知消费者。
    not_empty_cv_.notify_one();
  }

  int Consume() {
    std::unique_lock<std::mutex> lock(mutex_);
    // 等待缓冲不为空。
    not_empty_cv_.wait(lock, [=] { return buffered_ > 0; });

    // 移除一个元素。
    int n = circular_buffer_[begin_];
    begin_ = (begin_ + 1) % circular_buffer_.size();

    --buffered_;

    // 通知前，手动解锁。
    lock.unlock();
    // 通知生产者。
    not_full_cv_.notify_one();
    return n;
  }

private:
  std::size_t begin_;
  std::size_t end_;
  std::size_t buffered_;
  std::vector<int> circular_buffer_;
  std::condition_variable not_full_cv_;
  std::condition_variable not_empty_cv_;
  std::mutex mutex_;
};
```

生产者与消费者线程共享的缓冲。g_io_mutex 是用来同步输出的。

BoundedBuffer g_buffer(2);

boost::mutex g_io_mutex;

生产者

生产 100000 个元素，每 10000 个打印一次。

```cpp
void Producer() {
  int n = 0;
  while (n < 100000) {
    g_buffer.Produce(n);
    if ((n % 10000) == 0) {
      std::unique_lock<std::mutex> lock(g_io_mutex);
      std::cout << "Produce: " << n << std::endl;
    }
    ++n;
  }

  g_buffer.Produce(-1);
}

```

消费者

每消费到 10000 的倍数，打印一次。

```cpp

void Consumer() {
  std::thread::id thread_id = std::this_thread::get_id();

  int n = 0;
  do {
    n = g_buffer.Consume();
    if ((n % 10000) == 0) {
      std::unique_lock<std::mutex> lock(g_io_mutex);
      std::cout << "Consume: " << n << " (" << thread_id << ")" << std::endl;
    }
  } while (n != -1);  // -1 表示缓冲已达末尾。

  // 往缓冲里再放一个 -1，这样其他消费者才能结束。
  g_buffer.Produce(-1);
}

```

主程序

一个生产者线程，三个消费者线程。

```cpp
int main() {
  std::vector<std::thread> threads;

  threads.push_back(std::thread(&Producer));
  threads.push_back(std::thread(&Consumer));
  threads.push_back(std::thread(&Consumer));
  threads.push_back(std::thread(&Consumer));

  for (auto& t : threads) {
    t.join();
  }

  return 0;
}
```


#### 信号量（Semaphore） 

C++11 和 Boost.Thread 都没有提供信号量, 就是信号量太容易出错了（too error prone），通过组合互斥锁（mutex）和条件变量（condition variable）可以达到相同的效果，且更加安全。实现如下：


```cpp
class Semaphore {
public:
  explicit Semaphore(int count = 0) : count_(count) {
  }

  void Signal() {
    std::unique_lock<std::mutex> lock(mutex_);
    ++count_;
    cv_.notify_one();
  }

  void Wait() {
    std::unique_lock<std::mutex> lock(mutex_);
    cv_.wait(lock, [=] { return count_ > 0; });
    --count_;
  }

private:
  std::mutex mutex_;
  std::condition_variable cv_;
  int count_;
};
```


[C语言多线程编程(三)——信号量](https://zhuanlan.zhihu.com/p/98717838)

信号量是在多线程环境下使用的一种设施，是可以用来保证两个或多个关键代码段不被并发调用。

类似计数器，常用在多线程同步任务上，信号量可以在当前线程某个任务完成后，通知别的线程，再进行别的任务。

分类:

二值信号量：信号量的值只有0和1，这和互斥量很类似，若资源被锁住，信号量的值为0，若资源可用，则信号量的值为1；

计数信号量：信号量的值在0到一个大于1的限制值之间，该计数表示可用的资源的个数。

信号量在创建时需要设置一个初始值，表示同时可以有几个任务可以访问该信号量保护的共享资源，初始值为1就变成互斥锁Mutex，即同时只能有一个任务可以访问信号量保护的共享资源

函数使用：

sem_init

创建信号量

int sem_init(sem_t *sem, int pshared, unsigned int value);

sem：指向的信号对象

pshared：控制信号量的类型，如果其值为0，就表示信号量是当前进程的局部信号量，否则信号量就可以在多个进程间共享

value：信号量sem的初始值

sem_post

int sem_post(sem_t *sem);

信号量的值加1

sem_wait

int sem_wait(sem_t *sem);

信号量的值加-1

sem_destroy

int sem_destroy(sem_t *sem);

用完记得销毁 

说明：你可以进行三个下载任务，但是最多选择同时执行二个（创建两个线程）。直接看main函数即可，信号量的逻辑都在里面，在实际代码中最好，所有的线程和信号量的创建、释放都要进行校验，这里为了方便阅读，减少代码行数，就不进行校验了。


```cpp
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>
#include <windows.h>

#define MAXNUM 2
sem_t semDownload;
pthread_t a_thread, b_thread, c_thread;
int g_phreadNum = 1;

void InputInfo(void)
{
	printf("****************************************\n");
	printf("*** which task you want to download? ***\n");
	printf("*** you can enter [1-3],[0] is done  ***\n");
	printf("****************************************\n");
}
void *func1(void *arg)
{
	//等待信号量的值>0
	sem_wait(&semDownload);
	printf("==============  Downloading Task 1  ============== \n");
	Sleep(5000);
	printf("==============    Finished Task 1   ============== \n");
	g_phreadNum--;
	//等待线程结束 
	pthread_join(a_thread, NULL);
}

void *func2(void *arg)
{
	sem_wait(&semDownload);
	printf("==============  Downloading Task 2  ============== \n");
	Sleep(3000);
	printf("==============    Finished Task 2   ============== \n");
	g_phreadNum--;
	pthread_join(b_thread, NULL);
}

void *func3(void *arg)
{
	sem_wait(&semDownload);
	printf("==============  Downloading Task 3  ============== \n");
	Sleep(1000);
	printf("==============    Finished Task 3   ============== \n");
	g_phreadNum--;
	pthread_join(c_thread, NULL);
}

int main()
{
	int taskNum;
	InputInfo();

	while (scanf("%d", &taskNum) != EOF) {
		//输入0,判断是否正常退出
		if (taskNum == 0 && g_phreadNum <= 1) {
			break;
		}
		if (taskNum == 0){
			printf("Can not quit, casue count of threads is [%d]\n", g_phreadNum - 1);
		}
                //初始化信号量
		sem_init(&semDownload, 0, 0);
		printf("your choose Downloading Task [%d]\n", taskNum);
		//线程数超过2个则不下载
		if (g_phreadNum > MAXNUM) {
			printf("!!! You've reached a limit on the number of threads !!!\n");
			continue;
		}
		//用户选择下载Task
		switch (taskNum)
		{
		case 1:
			//创建线程1
			pthread_create(&a_thread, NULL, func1, NULL);
			//信号量+1，进而触发fun1的任务
			sem_post(&semDownload);
			//总线程数+1
			g_phreadNum++;
			break;
		case 2:
			pthread_create(&b_thread, NULL, func2, NULL);
			sem_post(&semDownload);
			g_phreadNum++;
			break;
		case 3:
			pthread_create(&c_thread, NULL, func3, NULL);
			sem_post(&semDownload);
			g_phreadNum++;
			break;
		default:
			printf("!!! eroor task [%d]  !!!\n", taskNum);
			break;
		}

	}

	//销毁信号量
	sem_destroy(&semDownload);
	return 0;
}
```

#### 读写者问题 读写锁（Read-Write Lock） 

STL 和 Boost 都提供了 shared_mutex 来解决「读者-写者」问题。shared_mutex 这个名字并不十分贴切，不如 pthread 直呼「读写锁」。

所谓「读写锁」，就是同时可以被多个读者拥有，但是只能被一个写者拥有的锁。而所谓「多个读者、单个写者」，并非指程序中只有一个写者（线程），而是说不能有多个写者同时去写。

下面看一个计数器的例子。

```cpp
class Counter {
public:
  Counter() : value_(0) {
  }

  // Multiple threads/readers can read the counter's value at the same time.
  std::size_t Get() const {
    std::shared_lock<std::shared_mutex> lock(mutex_);
    return value_;
  }

  // Only one thread/writer can increment/write the counter's value.
  void Increase() {
    // You can also use lock_guard here.
    std::unique_lock<std::shared_mutex> lock(mutex_);
    value_++;
  }

  // Only one thread/writer can reset/write the counter's value.
  void Reset() {
    std::unique_lock<std::shared_mutex> lock(mutex_);
    value_ = 0;
  }

private:
  mutable std::shared_mutex mutex_;
  std::size_t value_;
};
```

shared_mutex 比一般的 mutex 多了函数 lock_shared() / unlock_shared()，允许多个（读者）线程同时加锁、解锁，而 shared_lock 则相当于共享版的 lock_guard。

对 shared_mutex 使用 lock_guard 或 unique_lock 就达到了写者独占的目的。


```cpp
std::mutex g_io_mutex;

void Worker(Counter& counter) {
  for (int i = 0; i < 3; ++i) {
    counter.Increase();
    std::size_t value = counter.Get();

    std::lock_guard<std::mutex> lock(g_io_mutex);
    std::cout << std::this_thread::get_id() << ' ' << value << std::endl;
  }
}

int main() {
  const std::size_t SIZE = 2;

  Counter counter;

  std::vector<std::thread> v;
  v.reserve(SIZE);

  v.emplace_back(&Worker, std::ref(counter));
  v.emplace_back(&Worker, std::ref(counter));

  for (std::thread& t : v) {
    t.join();
  }

  return 0;
}
```

当然，对于计数器来说，原子类型 std::atomic<> 也许是更好的选择。

假如一个线程，先作为读者用 shared_lock 加锁，读完后突然又想变成写者，该怎么办？

方法一：先解读者锁，再加写者锁。这种做法的问题是，一解一加之间，其他写者说不定已经介入并修改了数据，那么当前线程作为读者时所持有的状态（比如指针、迭代器）也就不再有效。

方法二：用 upgrade_lock（仅限 Boost，STL 未提供），可以当做 shared_lock 用，但是必要时可以直接从读者「升级」为写者。



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








