# 1    effcitive  python

## 1.1 Pythonic Thinking

### pep8 

### 基础结构

**命名法:** 

函数，变量以及属性应该使用小写，如果有多个单词推荐使用下划线进行连接，如lowercase_underscore

被保护 的属性应该使用 单个 前导下划线来声明。

私有 的属性应该使用 两个 前导下划线来进行声明。

类以及异常信息 应该使用单词 首字母大写 形式，也就是我们经常使用的驼峰命名法，如CapitalizedWord。

模块级 别的常量应该使用 全部大写 的形式, 如ALL_CAPS。

类内部的实例方法的应该将self作为其第一个参数。且self也是对当前类对象的引用。

类方法应该使用cls来作为其第一个参数。且self引用自当前类。

**表达式和语句:** 

如果非要相对的引用，应该使用明确的语法from . import foo。

按照以下规则引入模块：标准库，第三方库，你自己的库。每一个部分内部也应该按照字母顺序来引入


**引用-变化-追随:**

当为列表赋值的时候省去开头和结尾下标的时候，将会用 这个引用 来替换整个列表的内容，而不是创建一个新的列表。同时，引用了这个列表的列表的相关内容，也会跟着发生变化。

**切片：**

避免在分片中同时使用start，end，stride；如果非要使用，考虑两次赋值（一个分片，一个调幅），或者使用内置模块itertoolsde 的 islice方法来进行处理。

**列表表达式for  in  if 结构**

用列表推导来代替 map 和 filter，字典和集合也都支持列表表达式

    chile_rank = {'ghost':1,'habanero':2,'cayenne':3}

    rank_dict = {rank:name for name,rank in child_rank.items()}

    chile_len_set = {len(name) for name in rank_dict.values()}

避免使用超过两个的表达式，可以使用多if代替

b = [x for x in a if x> 4 if x%2 ==0]

c = [x for x in a if x > 4 and if x%2 ==0]


### 迭代器：enumerate、zip  

enumerate 比range更好用:

enumerate提供了简洁的语法，再循环迭代一个迭代器的同时既能获取下标，也能获取当前值。

Prefer：

    for i, flavor in enumerate(flavor_list):

        print(‘%d: %s’ % (i + 1, flavor)) 

用 zip 函数来同时遍历两个迭代器:

内置的zip函数可以并行的对多个迭代器进行处理;如果所处理的迭代器的长度不一致时，zip会默认截断输出.内置模块itertools中的zip_longest函数可以并行地处理多个迭代器，而可以无视长度不一致的问题。

Prefer:

    names = [‘Cecilia’, ‘Lise’, ‘Marie’]

    max_letters = 0

    letters = [len(n) for n in names]

    for name, count in zip(names, letters):

....

### 生成器 

考虑使用生成器而不是返回列表

相较于返回一个列表的情况，替代方案中使用生成器可以使得代码变得更加的清晰。生成器返回的迭代器，是在其生成器内部一个把值传递给了yield变量的集合。

生成器可以处理很大的输出序列就是因为它在处理的时候不会完全的包含所有的数据。

    def index_words_iter(text):
        if text:
            yield 0
        for index, letter in enumerate(text):
            if letter == ' ':
                yield index + 1

    result = list(index_words_iter(address))
    

## 1.2 函数

### 全局与局部变量

**闭包中是怎样使用外围作用域变量**

global 关键字用于使用全局变量，nonlocal 关键字用于使用局部变量(函数内)

Python编译器变量查找域的顺序：

- 当前函数的作用域

- 任何其他的封闭域（比如其他的包含着的函数）。

- 包含该段代码的模块域（也称之为全局域）

- 内置域（包含了像len,str等函数的域）


### 参数 

位置参数： 使用*args定义语句，函数可以接收可变数量的位置参数

    def log(message, *values):
        if not values:
            print(message)
        else:
            values_str = ', '.join(str(x) for x in values)
            print('%s: %s' % (message, values_str))

    log('My numbers are', 1, 2)
    log('Hi there')
    

关键字参数：可选的关键字参数应该优于位置参数

关键字参数的好处:代码可读性的提高，以在定义的时候初始化一个默认值，在前面的调用方式不变的情况下可以很好的拓展函数的参数，不用修改太多的代码

    def flow_rate(weight_diff, time_diff, period=1):
        return (weight_diff / time_diff) * period

默认参数：使用None和文档说明动态的指定默认参数

**使用None作为关键字参数的默认值会有一个动态值**

prefer:

    def log(message, when=None):
        """Log a message with a timestamp.

        Args:
            message: Message to print
            when: datetime of when the message occurred.
                Default to the present time.
        """
        when = datetime.now() if when is None else when
        print("%s: %s" %(when, message))

    # 测试

    log('Hi there!')
    sleep(0.1)
    log('Hi again!')


## 1.3 类和继承

**尽量使用辅助类来维护程序的状态，避免dict嵌套dict或大tuple**

避免字典中嵌套字典，或者长度较大的元组。

在一个整类（类似于前面第一个复杂类那样）之前考虑使用 namedtuple 制作轻量，不易发生变化的容器。

当内部的字典关系变得复杂的时候将代码重构到多个工具类中。

你可以从依赖树的底端开始，将其划分成多个类

prefer:

        import collections

    Grade = collections.namedtuple('Grade', ('score', 'weight'))


    class Subject(object):
        def __init__(self):
            self._grades = []

        def report_grade(self, score, weight):
            self._grades.append(Grade(score, weight))

        def average_grade(self):
            total, total_weight = 0, 0
            for grade in self._grades:
                total += grade.score * grade.weight
                total_weight += grade.weight
            return total / total_weight


    class Student(object):
        def __init__(self):
            self._subjects = {}

        def subject(self, name):
            if name not in self._subjects:
                self._subjects[name] = Subject()
            return self._subjects[name]

        def average_grade(self):
            total, count = 0, 0
            for subject in self._subjects.values():
                total += subject.average_grade()
                count += 1
            return total / count


    class Gradebook(object):
        def __init__(self):
            self._students = {}

        def student(self, name):
            if name not in self._students:
                self._students[name] = Student()
            return self._students[name]

**对于简单接口使用函数而不是类的实例**

对于简单接口组件而言，函数就足够了。Python中引用函数和方法的原因就在于它们是first-class，可以直接的被运用在表达式中。

特殊方法__call__允许你像调用函数一样调用一个对象实例。需要一个函数来维护状态信息，考虑一个定义了__call__方法的状态闭包类。

内置的API都允许你通过向函数传递参数,如

        names = ['Socrates', 'Archimedes', 'Plato', 'Aristotle']
        names.sort(key=lambda x: len(x))

python hooks：函数可以作为钩子来工作，因为Python有first-class函数，函数、方法可以像其他的变量值一样被引用，或者被传递给其他的函数

        current = {'green': 12, 'blue': 3}
        incremetns = [
            ('red', 5),
            ('blue', 17),
            ('orange', 9)
        ]

        class BetterCountMissing(object):

            def __init__(self):
                self.added = 0

            def __call__(self):
                self.added += 1
                return 0

        counter = BetterCountMissing()
        counter()
        assert callable(counter)
        # 这里我使用一个BetterCountMissing实例作为defaultdict函数的默认的hook值来追踪缺省值被添加的次数。
        counter = BetterCountMissing()
        result = defaultdict(counter, current)
        for key, amount in increments:
            result[key] += amount
        assert counter.added == 2

**使用@classmethod多态性构造对象**

Python的每个类只支持单个的构造方法，__init__。

使用@classmethod可以为你的类定义可替代构造方法的方法。

class GenericInputData(object):
    def read(self):
        raise NotImplementedError

    @classmethod
    def generate_inputs(cls, config):
        raise NotImplementedError

class PathInputData(GenericInputData):
    def __init__(self, path):
        super().__init__()
        self.path = path

    def read(self):
        return open(self.path).read()

    @classmethod
    def generate_inputs(cls, config):
        data_dir = config['data_dir']
        for name in os.listdir(data_dir):
            yield cls(os.path.join(data_dir, name))


### 继承

**使用super关键字初始化父类**

Python的解决实例化次序问题的方法MRO解决了菱形继承（ diamond inheritance ）中超类多次被初始化的问题。

总是应该使用super来初始化父类。

python中父类实例化的规则是按照MRO标准来进行的，MRO 的执行顺序是 DFS

    class MyBaseClass(object):
        def __init__(self, value):
            self.value = value

    class Implicit(MyBaseClass):
        def __init__(self, value):
            super().__init__(value * 2)

**只在用编写Max-in组件的工具类的时候使用多继承**

如果可以使用mix-in实现相同的结果输出的话，就不要使用多继承了

当mix-in类需要的时候，在实例级别上使用可插拔的行为可以为每一个自定义的类工作的更好

从简单的行为出发，创建功能更为灵活的mix-in

Mix-in类：一个只定义了几个类必备功能方法的很小的类，不定义以自己的实例属性，也不需要它们的初始化方法__init__被调用

    import json

    class ToDictMixin(object):
        def to_dict(self):
            return self._traverse_dict(self.__dict__)

        def _traverse_dict(self, instance_dict):
            output = {}
            for key, value in instance_dict.items():
                output[key] = self._traverse(key, value)
            return output

    class JsonMixin(object):
        @classmethod
        def from_json(cls, data):
            kwargs = json.loads(data)
            return cls(**kwargs)

        def to_json(self):
            return json.dumps(self.to_dict())

**多使用公共属性，而不是私有属性**

Python 编译器无法严格保证 private 字段的私密性

不要盲目将属性设置为private，而是应该从一开始就做好规划，并允子类更多地访问超类的内部的API

应该多用protected 属性，并且在文档中把这些字段的合理用法告诉子类的开发者，而不要试图用 private 属性来限制子类的访问

    class ClassDef(object):
        def __init__(self):
            # public
            self.name = "class_def"
            # private
            self.__age = 29
            # protected
            self._sex = "man"

        def fun1(self):
            print("call public function")

        def __fun2(self):
            print("call private function")

        def _fun3(self):
            print("call protected function")

默认都是pubic类型，被子类、类内以及类外被访问

protected类型： _xx 以单下划线开头表示的是protected类型的变量或者方法，只能允许其本身与子类进行访问。

private类型：__xx 双下划线表示的是私有类型的变量或者方法, 只能允许类内进行访问。

特列方法:   __xx__定义的是特列方法。用户控制的命名空间内的变量或是属性


**自定义容器类型要从collections.abc来继承**

如果要定制的子类比较简单，那就可以直接从Python的容器类型（如list或dict）中继承

编写自制的容器类型时，可以从collection.abc 模块的抽象类基类中继承，那些基类能确保我们的子类具备适当的接口及行为

collections.abc的 Sequence : 子类只要实现 __getitem__以及 __len__， Sequence 提供了 count() 以及 index()

## 1.4  元类和属性

### 属性

**用纯属性取代 get 和 set 方法**

使用public属性避免set和get方法，@property定义一些特别的行为

@property 方法应该遵循最小惊讶原则，而不应该产生奇怪的副作用

    class VoltageResistance(Resistor):
        def __init__(self, ohms):
            super().__init__(ohms)
            self._voltage = 0

        # 相当于 getter
        @property
        def voltage(self):
            return self._voltage

        # 相当于 setter
        @voltage.setter
        def voltage(self, voltage):
            self._voltage = voltage
            self.current = self._voltage / self.ohms

    r2 = VoltageResistance(1e3)
    print('Before: %5r amps' % r2.current)
    # 会执行 setter 方法
    r2.voltage = 10
    print('After:  %5r amps' % r2.current)
    
**考虑@property来替代属性重构**
 
使用@property给已有属性扩展新需求，可以用 @property 来逐步完善数据模型

当@property太复杂了才考虑重构

**用描述符来改写需要复用的 @property 方法**

如果想复用 @property 方法及其验证机制，那么可以自定义描述符类

WeakKeyDictionary 可以保证描述符类不会泄露内存

通过描述符协议来实现属性的获取和设置操作时，不要纠结于__getatttttribute__ 的方法的具体运作细节
    
not prefer：

    class Exam(object):
        def __init__(self):
            self._writing_grade = 0
            self._math_grade = 0

        @staticmethod
        def _check_grade(value):
            if not (0 <= value <= 100):
                raise ValueError('Grade must be between 0 and 100')

        @property
        def writing_grade(self):
            return self._writing_grade

        @writing_grade.setter
        def writing_grade(self, value):
            self._check_grade(value)
            self._writing_grade = value

        @property
        def math_grade(self):
            return self._math_grade

        @math_grade.setter
        def math_grade(self, value):
            self._check_grade(value)
            self._math_grade = value

prefer:
    from weakref import WeakKeyDictionary

    class Grade(object):
        def __init__(self):
            self._values = WeakKeyDictionary()
        def __get__(self, instance, instance_type):
            if instance is None: return self
            return self._values.get(instance, 0)

        def __set__(self, instance, value):
            if not (0 <= value <= 100):
                raise ValueError('Grade must be between 0 and 100')
            self._values[instance] = value

    class Exam(object):
        math_grade = Grade()
        writing_grade = Grade()
        science_grade = Grade()

    exam = Exam()
    exam.writing_grade = 40


**用 __getattr__, __getattribute__, 和__setattr__ 实现按需生产的属性**

通过__getattr__ 和 __setattr__，可以用惰性的方式来加载并保存对象的属性

要理解 __getattr__ 和 __getattribute__ 的区别：前者只会在待访问的属性缺失时触发，而后者则会在每次访问属性的时候触发

如果要在__getattributte__ 和 __setattr__ 方法中访问实例属性，那么应该直接通过 super() 来做，以避免无限递归

obj.name，getattr和hasattr都会调用getattribute方法，如果name不在obj.dict里面，还会调用getattr方法，如果没有自定义getattr方法会AttributeError异常

只要有赋值操作（=，setattr）都会调用setattr方法（包括a = A())

### 元类

**用元类来验证子类**

通过元类，我们可以在生成子类对象之前，先验证子类的定义是否合乎规范，使用元类对类型对象进行验证

Python 系统把子类的整个 class 语句体处理完毕之后，就会调用其元类的__new__ 方法

元类：用来创建类的“东西”。你创建类就是为了创建类的实例对象，元类就是类的类，可以把元类称为“类工厂”。

MyClass = MetaClass()

MyObject = MyClass()

    >>> age = 35
    >>> age.__class__
    <type 'int'>
    >>> age.__class__.__class__
    <type 'type'>

先定义metaclass，就可以创建类，最后创建实例。

首先，定义元类，我们要继承 type, python 默认会把那些类的 class 语句体中所含的相关内容，发送给元类的 new 方法。

    class Meta(type):
        def __new__(meta, name, bases, class_dict):
            print(meta, name, bases, class_dict)
            return type.__new__(meta, name, bases, class_dict)
            
    class MyClassInPython3(object, metaclass=Meta):
        stuff = 123

        def foo(self):
            pass

**用元类来注解类的属性**

借助元类，我们可以在某个类完全定义好之前，率先修改该类的属性

描述符与元类能够有效的组合起来，以便对某种行为做出修饰，或者在程序运行时探查相关信息

如果把元类与描述符相结合，那就可以在不使用 weakerf 模块的前提下避免内存泄露

## 1.5 并行与并发

**用 subprocess 模块来管理子进程**

使用 subprocess 模块运行子进程管理自己的输入和输出流

subprocess 可以并行执行最大化CPU的使用

communicate 的 timeout 参数避免死锁和被挂起的子进程

### 线程

**可以用线程来执行阻塞时I/O，但不要用它做平行计算**

因为GIL，Python thread并不能并行运行多段代码

Python保留thread的两个原因：1.可以模拟多线程，2.多线程可以处理I/O阻塞的情况

Python thread可以并行执行多个系统调用，这使得程序能够在执行阻塞式I/O操作的同时，执行一些并行计算

**在线程中使用Lock来防止数据竞争**

虽然Python thread不能同时执行，但是Python解释器还是会打断操作数据的两个字节码指令，所以还是需要锁

thread模块的Lock类是Python的互斥锁实现

**用 Queue 来协调各线程之间的工作**

管线是一种优秀的任务处理方式，它可以把处理流程划分为若干阶段，并使用多条Python线程同时执行这些任务

构建并发式的管线时，要注意许多问题，包括：如何防止某个阶段陷入持续等待的状态之中、如何停止工作线程，以及如何防止内存膨胀等

Queue类具备构建健壮并发管道的特性：阻塞操作，缓存大小和连接（join）

    from queue import Queue
    from threading import Thread

    class ClosableQueue(Queue):
        SENTINEL = object()

        def close(self):
            self.put(self.SENTINEL)

        def __iter__(self):
            while True:
                item = self.get()
                try:
                    if item is self.SENTINEL:
                        return  # Cause the thread to exit
                    yield item
                finally:
                    self.task_done()

    class StoppableWorker(Thread):
        def __init__(self, func, in_queue, out_queue):
            super().__init__()
            self.func = func
            self.in_queue = in_queue
            self.out_queue = out_queue

        def run(self):
            for item in self.in_queue:
                result = self.func(item)
                self.out_queue.put(result)
    def download(item):
        return item

    def resize(item):
        return item

    def upload(item):
        return item

    download_queue = ClosableQueue()
    resize_queue = ClosableQueue()
    upload_queue = ClosableQueue()
    done_queue = ClosableQueue()
    threads = [
        StoppableWorker(download, download_queue, resize_queue),
        StoppableWorker(resize, resize_queue, upload_queue),
        StoppableWorker(upload, upload_queue, done_queue),
    ]


    for thread in threads:
        thread.start()
    for _ in range(1000):
        download_queue.put(object())
    download_queue.close()


    download_queue.join()
    resize_queue.close()
    resize_queue.join()
    upload_queue.close()
    upload_queue.join()
    print(done_queue.qsize(), 'items finished')

**考虑用协程来并发地运行多个函数**

线程有三个大问题：需要特定工具去确定安全性，单个线程需要8M的内存，线程启动消耗。

coroutine只有1kb的内存消耗

generator可以通过send方法把值传递给yield


**考虑用 concurrent.futures 来实现真正的并行计算**

CPU瓶颈模块使用C扩展

concurrent.futures的multiprocessing可以并行处理一些任务

使用 concurrent.futures 里面的 ProcessPoolExecutor 可以很简单地平行处理 CPU-bound 的程式，省得用 multiprocessing 自定义。

    from concurrent.futures import ProcessPoolExecutor

    start = time()
    pool = ProcessPoolExecutor(max_workers=2)  # The one change
    results = list(pool.map(gcd, numbers))
    end = time()
    print('Took %.3f seconds' % (end - start))


里面的ThreadPoolExecutor和ProcessPoolExecutor，继承了Executor，分别被用来创建线程池和进程池的代码。

将相应的tasks直接放入线程池/进程池，不需要维护Queue来操心死锁的问题，线程池/进程池会自动帮我们调。

可以使用submit和map提交任务。

    import concurrent.futures
    import urllib.request

    URLS = ['http://www.foxnews.com/',
            'http://www.cnn.com/',
            'http://europe.wsj.com/',
            'http://www.bbc.co.uk/',
            'http://some-made-up-domain.com/']

    # Retrieve a single page and report the URL and contents
    def load_url(url, timeout):
        with urllib.request.urlopen(url, timeout=timeout) as conn:
            return conn.read()

    # We can use a with statement to ensure threads are cleaned up promptly
    with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
        # Start the load operations and mark each future with its URL
        future_to_url = {executor.submit(load_url, url, 60): url for url in URLS}
        for future in concurrent.futures.as_completed(future_to_url):
            url = future_to_url[future]
            try:
                data = future.result()
            except Exception as exc:
                print('%r generated an exception: %s' % (url, exc))
            else:
                print('%r page is %d bytes' % (url, len(data)))


## 1.6 内置模块

**用 functools.wraps 定义函数修饰器**

装饰器可以对函数进行封装，但是会改变函数信息

使用 functools 的 warps 可以解决这个问题

    def trace(func):
      @wraps(func)
      def wrapper(*args, **kwargs):
          # …
      return wrapper
    @trace
    def fibonacci(n):
      # …

**考虑用 contextlib 和with 语句来改写可复用的 try/finally 代码**

使用with语句代替try/finally，增加代码可读性

使用 contextlib 提供的 contextmanager 装饰函数就可以被 with 使用, with 和 yield 返回值使用

实现一个新的上下文管理器的最简单的方法就是使用 contexlib 模块中的 @contextmanager 装饰器


    import time
    from contextlib import contextmanager

    @contextmanager
    def timethis(label):
        start = time.time()
        try:
            yield
        finally:
            end = time.time()
            print('{}: {}'.format(label, end - start))

    # Example use
    with timethis('counting'):
        n = 10000000
        while n > 0:
            n -= 1

在函数 timethis() 中，yield 之前的代码会在上下文管理器中作为 __enter__() 方法执行， 所有在 yield 之后的代码会作为 __exit__() 方法执行。 如果出现了异常，异常会在yield语句那里抛出。

**用 copyreg 实现可靠的 pickle 操作**

pickle 模块只能序列化和反序列化确认没有问题的对象

copyreg的 pickle 支持属性丢失，版本和导入类表信息

使用 copyreg這个内建的 module, 搭配 pickle使用

    def pickle_game_state(game_state):
        kwargs = game_state.__dict__
        kwargs['version'] = 2
        return unpickle_game_state, (kwargs,)

    def unpickle_game_state(kwargs):
        version = kwargs.pop('version', 1)
        if version == 1:
            kwargs.pop('lives')
        return GameState(**kwargs)

    copyreg.pickle(GameState, pickle_game_state)

**用 datetime 替代 time 来处理本地时间**

不要使用time模块在转换不同时区的时间

而用datetime配合 pytz 转换

总数保持UTC时间，最后面再输出本地时间

**使用内置算法与数据结构**

使用 Python 内置的模块来描述各种算法和数据结构

内置算法和数据结构

    collections.deque

    collections.OrderedDict

    collection.defaultdict

    heapq模块操作list（优先队列）：heappush，heappop和nsmallest

        a = []
        heappush(a, 5)
        heappush(a, 3)
        heappush(a, 7)
        heappush(a, 4)
        print(heappop(a), heappop(a), heappop(a), heappop(a))

        # >>>

        # 3 4 5 7

    bisect模块：bisect_left可以对有序列表进行高效二分查找

    itertools模块（Python2不一定支持）：

        连接迭代器：chain，cycle，tee和zip_longest

        过滤：islice，takewhile，dropwhile，filterfalse

        组合不同迭代器：product，permutations和combination




