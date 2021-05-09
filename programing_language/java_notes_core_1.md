**同步阻塞**
 
每一个 Java 对象有一个锁。线程可以通过调用同步方法获得锁。还有 另一种机制可以获得锁，通过进入一个同步阻塞。当线程进入如下形式的阻塞： synchronized (obj) 
 
于是它获得Obj的锁。 有时会发现“ 特殊的” 锁，例如：

```java

public class Bank 
{ 

private double[] accounts; 
private Object lock = new Object() ; 
public void transfer(int from, int to, int amount)

 { 
   synchronized (lock) // an ad-hoc lock
 { 
     accounts[from] -= amount; 
     accounts[to] += amount; 
 } 
   System.out.print1n(.. } 
}

```

在此，lock 对象被创建仅仅是用来使用每个 Java 对象持有的锁。 有时程序员使用一个对象的锁来实现额外的原子操作， 实际上称为客户端锁定。如你所见，客户端锁定是非常脆弱 的，通常不推荐使用。

**监视器**

• 监视器是只包含私有域的类。

• 每个监视器类的对象有一个相关的锁。

• 使用该锁对所有的方法进行加锁。

**Volatile域**

volatile关键字为实例域的同步访问提供了一种免锁机制。如果声明一个域为 volatile, 那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。

Volatile 变量不能提供原子性。例如， 方法 public void flipDone() { done = !done; } // not atomic 不能确保翻转域中的值。不能保证读取、翻转和写入不被中断。

	private boolean done;
	public synchronized boolean isDoneO { return done; }
	public synchronized void setDoneO { done = true; }

**final变量**

还有一种情况可以安全地访问一个共享域， 即这个域声明为 final 时

	final Map<String, Double>accounts = new HashKap<>()；

**原子性**

java.util.concurrent.atomic 包中有很多类使用了很高效的机器级指令（而不是使用 锁）来保证其他操作的原子性。

例如，Atomiclnteger 类提供了方法 incrementAndGet 和 decrementAndGet, 它们分别以原子方式将一个整数自增或自减。

```java
	public static AtomicLong nextNumber=new AtomicLong();
	
	long id=nextNumber.incrementAndGet();
```

incrementAndGet方法以原子方式将 AtomicLong 自增， 并返回自增后的值。

```java
	AtomicLong largest=new AtomicLong();
		  do {
			  oldValue=largest.get();
			  newValue=Math.max(oldvalue,observed);
		  }while(!largest.compareAndSet(oldValue, newValue));
```

**死锁**

 有可能会因为每一个线程要等待更多的钱款存人而导致所有线程都被阻塞。这样的状态 称为死锁（deadlock）。

**线程局部变量**

 用空间换时间，不同的线程使用自己的对象。

要避免共享变量， 使用 ThreadLocal 辅助类为各个线程提供各自的实例。

要为每个线程构造一个实例，

	public static final ThreadLocal<SimpleDateFormat> dateformat=
	new ThreadLocal.withInitial(()->new SimpleDateFormat("yyyy-mm-dd"));


String dateStamp = dateFormat.get().format(new Date()); 在一个给定线程中首次调用 get 时， 会调用 initialValue方法。在此之后，get方法会返回属于当前线程的那个实例。

**锁测试与超时**

线程在调用 lock 方法来获得另一个线程所持有的锁的时候，很可能发生阻塞。应该更加 谨慎地申请锁。tryLock方法试图申请一个锁， 在成功获得锁后返回 true, 否则， 立即返回 false, 而且线程可以立即离开去做其他事情：

	if(mylock.tryLock()){
	    try{}
	    finally{mylock.unlock();}
	}


可以调用 tryLock 时，使用超时参数：

	if (myLock.tryLock(100, TineUnit.MILLISECONDS)) ...

在等待一个条件时，也可以提供一个超时：

	myCondition.await(100, TineUniBILLISECONDS))

**读/ 写锁**

允许对读者线程共享访问是合适的。当然，写者线程依然必须是互斥访问的。

下面是使用读 / 写锁的必要步骤：

```java
	1 ) 构造一个 ReentrantReadWriteLock 对象：

	private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock():

	2 ) 抽取读锁和写锁：

	private Lock readLock = rwl.readLock(); private Lock writeLock = rwl.writeLock();

	3 ) 对所有的获取方法加读锁：

	public double getTotalBalance()

	{
	    readLock.lock()； try { . . . } finally { readLock.unlock(); }
	}

	4 ) 对所有的修改方法加写锁：

	public void transfer(. . .)

	{
	    writeLock.lock(); try { . . . } finally { writeLock.unlock(); }
	}
```

**弃用stop和suspend**

stop方法天生就不安全，经验证明 suspend方法会经常导致死锁

#### 阻塞队列

阻塞队列方法分为以下3类， 这取决于当队列满或空时它们的响应方式。

如果将队列当作线程管理工具来使用， 将要用到 put 和 take 方法。

当试图向满的队列中添加或从空的队列中移出元素时，add、 remove 和 element 操作抛出异常。

当然，在一个多线程程序中，队列会在任何时候空或满，因此，一定要使用offer、poll 和 peek方法作为替代。

注： poll和 peek 方法返回空来指示失败。因此，向这些队列中插入 null 值是非法的。

LinkedBlockingQueue 的容量是没有上边界的，但是，也可以选择指定最大容量。

LinkedBlockingDeque 是一个双端的版本。

ArrayBlockingQueue 在构造时需要指定容量，并且有一个可选的参数来指定是否需要公平性。若设置了公平参数， 则那么等待了最长时间的线程会优先得到处理。通常，公平 性会降低性能，只有在确实非常需要时才使用它。

PriorityBlockingQueue是一个带优先级的队列， 而不是先进先出队列。元素按照它们的优先级顺序被移出。

DelayQueue 包含实现Delayed 接口的对象。

#### 线程安全的集合

**高效的映射、集和队列**

Collections.synchronizedMap()、Hashtable并发量小。

HashTable锁住所有数据，ConcurrentHashMap锁住一段。

ConcurrentHashMap 并发高 ConcurrentHashSet

ConcurrentSkipListMap 高并发有序。 ConcurrentSkipListSet

CopyOnWriteArrayList 和 CopyOnWriteArraySet


**映射条目的原子更新**

replace 操作，它会以原子方式用一个新值替换原值

调用 compute方法时可以提供 一个键和一个计算新值的函数。这个函数接收键和相关联的值（如果没有值，则为 mill), 它会计算新值。

首次增加一个键时通常需要做些特殊的处理。利用 merge 方法可以非常方便地做到这一点。这个方法有一个参数表示键不存在时使用的初始值。

 	map.merge(word, 1L, (existingValue, newValue) -> existingValue + newValue);

**对并发散列映射的批操作**

批操作会遍历映射，处理遍历过程中找到的元素。无须冻结当前映射的快照。

• 搜索（search) 为每个键或值提供一个函数，直到函数生成一个非 null 的结果。然后搜 索终止，返回这个函数的结果。

• 归约（reduce) 组合所有键或值， 这里要使用所提供的一个累加函数。

• for Each 为所有键或值提供一个函数

每个操作都有 4 个版本：

• operationKeys: 处理键。

• operatioriValues: 处理值。

• operation: 处理键和值。

• operatioriEntries: 处理 Map.Entry对象

 对于上述各个操作， 需要指定一个参数化阈值。如果映射包含的 元素多于这个阈值， 就会并行完成批操作。如果希望批操作在一个线程中运行，可以使用阈 值 Long.MAX_VALUE。

```java
	String result=map.search(threhold,(k,v)->v>1000?k:null);

	//forEach方法有两种形式。第一个只为各个映射条目提供一个消费者函数， 例如:
	map.forEach(threhold,(k,v)->System.out.println(k+"->"+v));
	//第二种形式还有一个转换器函数，这个函数要先提供，其结果会传递到消费者： 
	map.forEach(threhold,(k,v)->k+"->"+v,System.out::println);

	Long sum=map.reduceValues(threhold,Long::sum);

	Integer maxlength=map.reduceKeys(threshold,String::length,Integer::sum);
 
```

forEach 方法有两种形式。第一个只为各个映射条目提供一个消费者函数；第二种形式还有一个转换器函数，这个函数要先提供，其结果会传递到消费者。

如果映射为空， 或者所有条目都被过滤掉， reduce 操作会返回 null。

对于 int、 long 和 double 输出还有相应的特殊化操作， 分别有后缀 Tolnt、ToLong和 ToDouble.

**并发集视图**

静态newKeySet方法会生成一个Set<K>, 这实际上是 ConcurrentHashMap<K, Boolean〉的一个包装器。（所有映射值都为 Boolean.TRUE）
						     
	Set<String> words=ConcurrentHashMap.<String>newKeySet();

这个集是可变的。 如果删除这个集的元素，这个键（以及相应的值）会从映射中删除。不过，不能向键集增加元素，因为没有相应的值可以增加。

**写数组的拷贝**

CopyOnWriteArrayList 和 CopyOnWriteArraySet 是线程安全的集合，其中所有的修改线程对底层数组进行复制。如果在集合上进行迭代的线程数超过修改线程数，

**并行数组算法**

Arrays类提供了大量并行化操作。静态 Arrays.parallelSort 方法可以对 一个基本类型值或对象的数组排序。

parallelSetAll 方法会用由一个函数计算得到的值填充一个数组。这个函数接收元素索引， 然后计算相应位置上的值。 

Arrays.parallelSort(words);

最后还有一个 parallelPrefix 方法，它会用对应一个给定结合操作的前缀的累加结果替换各个数组元素.

Arrays.parallelPrefix(values, (x, y)-> x * y) 之后，数组将包含： [1,1*2,1*2*3,...]

**较早的线程安全集合**

Vector 和 Hashtable类就提供了线程安全的动态数组和散列表的 实现。现在这些类被弃用了， 取而代之的是 AnayList 和 HashMap类。这些类不是线程安全 的，而集合库中提供了不同的机制。任何集合类都可以通过使用同步包装器（synchronization wrapper) 变成线程安全的： 

	List<E> list=Collections.synchronizedList(new ArrayList<>);
	Map<K,V> synchHashMap=Collections.synchronizeMap(new HashMap<K,V>());

 如果在另一个线程可能进行修改时要对集合进行迭代，仍然需要使用“ 客户端” 锁定：

	synchronized (synchHashMap)

	{ Iterator<K> iter = synchHashMap.keySet().iterator(); while (iter.hasNextO) . . }

#### Collable与Future

Runnable 封装一个异步运行的任务，可以把它想象成为一个没有参数和返回值的异步方法。

Callable 与 Runnable 类似，但是有返回值。Callable 接口是一个参数化的类型， 只有一 个方法 call。类型参数是返回值的类型。例如， Callable<Integer> 表示一个最终返回 Integer 对象的异步计算。

Future 保存异步计算的结果。可以启动一个计算，将 Future 对象交给某个线程，然后忘 掉它。Future 对象的所有者在结果计算好之后就可以获得它。

Future接口的方法：

```java
	public interface Future<V> 
	{ V get() throws .. 
	  V get(long timeout, TimeUnit unit) throws .. 
	  void cancel(boolean maylnterrupt); 
	   boolean isCancelled（）; 
	   boolean isDone(); } 
```

FutureTask 包装器是一种非常便利的机制， 可将 Callable转换成 Future 和 Runnable, 它 同时实现二者的接口.


#### 执行器

Executors操作Executor的一个工具类

Executor接口只有一个execute(Runnable r)函数.执行某一个任务

ExecutorService接口继承Executor接口 。在后台不停的运行，等待被扔任务。

构建一个新的线程是有一定代价的， 因为涉及与操作系统的交互。如果程序中创建了大量的生命期很短的线程，应该使用线程池（thread pool)。一个线程池中包含许多准备运行的 空闲线程。

将Runnable对象交给线程池， 就会有一个线程调用 run方法。当 run 方法退出时，线程不会死亡，而是在池中准备为下一个请求提供服务。

另一个使用线程池的理由是减少并发线程的数目。创建大量线程会大大降低性能甚至使虚拟机崩溃。如果有一个会创建许多线程的算法，应该使用一个线程数“ 固定的” 线程池以限制并发线程的总数。 执行器（Executors) 类有许多静态工厂方法用来构建线程池。

**线程池**

 newScheduledThreadPool

 newCachedThreadPool

 newFixedThreadPool

 newSingleThreadExecutor

 newWorkStealingPool 工作窃取：偷工作的线程池。这个线程main函数结束了，可能线程还在运行，可能看不到输出。每个线程维护自己的队列，当某个任务完成自己的任务之后，去偷别线程的任务自动执行。使用ForkJoin实现。

 ForkJoinPool 分叉合并：任务队列BlokingQueue，维护两个队列，一个等待执行的任务队列，一个已完成的线程队列。

 newCachedThreadPool方法构建了一个线程池， 对于每个任务， 如果有空闲线程可用，立即让它执行 任务，如果没有可用的空闲线程， 则创建一个新线程。newFixedThreadPool 方法构建一个具 有固定大小的线程池。
 

可用下面的方法之一将一个 Runnable 对象或 Callable 对象提交给 ExecutorService:

Future<?> submit(Runnable task)

Future<T> submit(Runnable task, T result)

Future<T> submit(Callable<T> task) 该池会在方便的时候尽早执行提交的任务。调用 submit 时，会得到一个 Future 对象， 可 用来查询该任务的状态。 

使用连接池时应该做的事：

1) 调用 Executors 类中静态的方法 newCachedThreadPool 或 newFixedThreadPool。

2) 调用 submit 提交 Runnable 或 Callable对象。

3 ) 如果想要取消一个任务， 或如果提交 Callable 对象， 那就要保存好返回的 Future 对象。

4 ) 当不再提交任何任务时，调用 shutdown。

**预定执行**

  ScheduledExecutorService 接口具有为预定执行（Scheduled Execution) 或 重复执行任务而设计的方法。它是一种允许使用线程池机制的java.util.Timer 的泛化。

  Executors 类的 newScheduledThreadPool 和 newSingleThreadScheduledExecutor方法将返回实现了 Scheduled-ExecutorService 接口的对象。 可以预定 Runnable 或 Callable 在初始的延迟之后只运行一次。

**控制任务组**

使用执行器有更有实际意义的原因，控制一组相关任务。invokeAny方法提交所有对象到一个Callable对象的集合中，并返回某个已经完成了的任务的结果。

```java

List<Callable<T>>tasks=...;
list<Future<T>> results=executor.invokeAll(tasks);
for(Future<T>:results){
    processFurther(result.get(i));
}

```

用 ExecutorCompletionService 来进行排列。 用常规的方法获得一个执行器。然后，构建一个ExecutorCompletionService， 提交任务给完成服务。

```java

ExecutorCompletionService<T> service = new ExecutorCompletionServiceo(executor)；
for (Callable<T> task : tasks) 
   service,submit(task);
 
for (int i = 0; i < tasks.size()；i++) 
  processFurther(service.take().get())； 
  
```

**Fork-Join框架**

另外一些应用可能对每个处理器内核分别使用一个线程，来完成计算密集型任务，如图像或视频处理。

fork-join poll的任务必须是fork-join task。一般使用fork-join task的子类Recuesive Action（没有返回值）和Recursive Task（有返回值)

要采用框架可用的一种方式完成这种递归计算，需要提供一个扩展RecursiveTask()的类（如果计算会生成一个类型为T 的结果）或者提供一个扩展RecursiveAction() 的类（如果不生成任何结果)。

再覆盖compute 方法来生成并调用子任务，然后合并其结果.

在后台，fork-join 框架使用了一种有效的智能方法来平衡可用线程的工作负载，这种方法称为工作密取（work stealing)。每个工作线程都有一个双端队列 ( deque) 来完成任务。

一个工作线程将子任务压人其双端队列的队头。（只有一个线程可以访问队头，所以不需要加锁。）一个工作线程空闲时，它会从另一个双端队列的队尾“ 密取” 一个任务。由于大的子任务都在队尾，这种密取很少出现。

**可完成Future**


#### 同步器

java.util.concurrent 包包含了几个能帮助人们管理相互合作的线程集的类。这些机制具有为线程之间的共用集结点模式，提供的“ 预置功能” 。如果有一个相互合作的线程集满足这些行为模式之一， 那么应该直接 重用合适的库类而不要试图提供手工的锁与条件的集合。

信号量： 一个信号量管理许多的许可证（permit )。为了通过信号量，线程通过调用acquire 请求许可。

倒计时门栓： 一个倒计时门栓（CountDownLatch) 让一个线程集等待直到计数变为0。倒计时门栓是一次性的。一旦计数为0, 就不能再重用了。

障栅： CyclicBarrier 类实现了一个集结点（rendezvous) 称为障栅（barrier)。当所有部分都准备好时，需要把结果组合在一起。当一个线 程完成了它的那部分任务后， 我们让它运行到障栅处。一旦所有的线程都到达了这个障栅， 障栅就撤销，线程就可以继续运行。

首先，构造一个障栅，并给出参与的线程数：

	CyclicBarrier barrier = new CydicBarrier(nthreads);
	
每一个线程做一些工作，完成后在障栅上调用await :

	public void runO
	{
	doWorkO ;
	bamer.awaitO；

await 方法有一个可选的超时参数：

	barrier.awaitdOO, TineUnit.MILLISECONDS);

可以提供一个可选的障栅动作（barrier action), 当所有线程到达障栅的时候就会执行这 一动作。

	Runnable barrierAction = ..

	CyclicBarrier barrier = new CyclicBarrier(nthreads, barrierAction);
 
障栅被称为是循环的（cyclic), 因为可以在所有等待线程被释放后被重用。

**交换器**

当两个线程在同一个数据缓冲区的两个实例上工作的时候， 就可以使用交换器 ( Exchanger) 典型的情况是， 一个线程向缓冲区填人数据， 另一个线程消耗这些数据。当它们都完成以后，相互交换缓冲区。

**同步队列**

同步队列是一种将生产者与消费者线程配对的机制。当一个线程调用 SynchronousQueue 的 put 方法时，它会阻塞直到另一个线程调用 take方法为止，反之亦然。

与 Exchanger 的情况不同， 数据仅仅沿一个方向传递，从生产者到消费者。 即使 SynchronousQueue 类实现了 BlockingQueue 接口， 
 

