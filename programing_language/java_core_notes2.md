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






