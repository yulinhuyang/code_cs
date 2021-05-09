
## 第十三章: 正确重载运算符

在python中, 大多数的运算符是可以重载的, 如 `==` 对应了 `__eq__` , `+` 对应 `__add__` .

某些运算符不能重载, 如 `is, and, or, and`.

## 第十四章: 可迭代的对象、迭代器和生成器

迭代是数据处理的基石. 扫描内存中放不下的数据集时, 我们要找到一种惰性获取数据的方式, 即按需一次获取一个数据. 这就是 `迭代器模式` .

python中有 `yield` 关键字, 用于构建 `生成器(generator)`, 其作用用于迭代器一样.

所有的生成器都是迭代器, 因为生成器完全实现了迭代器的接口.

检查对象 x 是否迭代, 最准确的方法是调用 `iter(x)` , 如果不可迭代, 则抛出 `TypeError` 异常. 这个方法比 `isinstance(x, abc.Iterable)` 更准确, 因为它还考虑到遗留的 `__getitem__` 方法.

### 可迭代的对象与迭代器的对比

我们需要对可迭代的对象进行一下定义:

> 使用 iter 内置函数可以获取迭代器的对象。如果对象实现了能返回迭代器的
> **iter** 方法，那么对象就是可迭代的。序列都可以迭代；实现了 **getitem** 方
> 法，而且其参数是从零开始的索引，这种对象也可以迭代。

我们要明确可迭代对象和迭代器之间的关系: 从可迭代的对象中获取迭代器.

标准的迭代器接口有两个方法:

- `__next__`: 返回下一个可用的元素, 如果没有元素了, 抛出 `StopIteration` 异常.
- `__iter__`: 返回 `self` , 以便咋应该使用可迭代对象的地方使用迭代器.

### 典型的迭代器

为了清楚地说明可迭代对象与迭代器之间的重要区别, 我们将两者分开, 写成两个类:

```python
import re
import reprlib

RE_WORD = re.compile('\w+')


class Sentence:

    def __init__(self, text):
        self.text = text
        # 返回一个字符串列表、元素为正则所匹配到的非重叠匹配】
        self.words = RE_WORD.findall(text)

    def __repr__(self):
        # 该函数用于生成大型数据结构的简略字符串的表现形式
        return 'Sentence(%s)' % reprlib.repr(self.text)

    def __iter__(self):
        '''明确表明该类型是可以迭代的'''
        return SentenceIterator(self.words) # 创建一个迭代器


class SentenceIterator:
    def __init__(self, words):
        self.words = words  # 该迭代器实例应用单词列表
        self.index = 0      # 用于定位下一个元素

    def __next__(self):
        try:
            word = self.words[self.index] # 返回当前的元素
        except IndexError:
            raise StopIteration()
        self.index += 1 # 索引+1
        return word # 返回单词

    def __iter__(self):
        return self     # 返回self
```

这个例子主要是为了区分可迭代对象和迭代器, 这种情况工作量一般比较大, 程序员也不愿这样写.

构建可迭代对象和迭代器经常会出现错误, 原因是混淆了二者. 要知道, 可迭代的对象有个 `__iter__` 方法, 每次都实例化一个新的迭代器; 而迭代器是要实现 `__next__` 方法, 返回单个元素, 同时还要提供 `__iter__` 方法返回迭代器本身.

可迭代对象一定不能是自身的迭代器. 也就是说, 可迭代对象必须实现 `__iter__` 方法, 但不能实现 `__next__` 方法.

小结下, 迭代器可以迭代, 但是可迭代对象不是迭代器.

### 生成器函数

实现相同功能, 覆盖python习惯的方式, 就是用生成器代替迭代器 `SentenceIterator` . 将上个例子改成生成器的方式:

```python
import re
import reprlib

RE_WORD = re.compile('\w+')

class Sentence:

    def __init__(self, text):
        self.text = text
        # 返回一个字符串列表、元素为正则所匹配到的非重叠匹配】
        self.words = RE_WORD.findall(text)

    def __repr__(self):
        # 该函数用于生成大型数据结构的简略字符串的表现形式
        return 'Sentence(%s)' % reprlib.repr(self.text)

    def __iter__(self):
        '''生成器版本'''
        for word in self.words:  # 迭代实例的words
            yield word  # 生成单词
        return
```

在这个例子中, 迭代器其实就是生成器对象, 每次调用 `__iter__` 都会自动创建, 因为这里的 `__iter__` 方法是生成器函数.

**生成器函数的工作原理**
只要python函数的定义体中有 `yield` 关键字, 改函数就是生成器函数. 调用生成器函数时, 会返回一个生成器对象. 也就是说, 生成器函数是生成器工厂.

普通函数与生成器函数的唯一区别就是, 生成器函数里面有 `yield` 关键字.

生成器函数会创建一个生成器对象, 包装生成器函数的定义体. 吧生成器传给 `next(...)` 函数时, 生成器函数会向前, 执行函数体中下一个 `yield` 语句, 返回产出的值, 并在函数定义体的当前位置暂停.

### 惰性实现

惰性的方式就是索性把所有数据都产出, 这是区别于 `next(...)` 一次生成一次元素的.

```python
import re
import reprlib

RE_WORD = re.compile('\w+')

class Sentence:

    def __init__(self, text):
        self.text = text
        self.words = RE_WORD.findall(text)

    def __repr__(self):
        return 'Sentence(%s)' % reprlib.repr(self.text)

    def __iter__(self):
        for match in RE_WORD.finditer(self.text):
            yield match.group()
```

### 生成器表达式

生成器表达式可以理解为列表推导的惰性版本: 不会迫切地构建列表, 而是返回一个生成器, 按需惰性生成元素. 也就是, 如果列表推导是产出列表的工厂, 那么生成器表达式就是产出生成器的工厂.

```python
def gen_AB():
    print('start')
    yield 'A'
    print('continue')
    yield 'B'
    print('end.')

res1 = [x*3 for x in gen_AB()]
for i in res1:
    print('-->', i)
```

可以看出, 生成器表达式会产出生成器, 因此可以使用生成器表达式减少代码:

```python
import re
import reprlib

RE_WORD = re.compile('\w+')

class Sentence:

    def __init__(self, text):
        self.text = text
        self.words = RE_WORD.findall(text)

    def __repr__(self):
        return 'Sentence(%s)' % reprlib.repr(self.text)

    def __iter__(self):
        return (match.group() for match in RE_WORD.finditer(self.text))
```

这里的 `__iter__` 不是生成器函数了, 而是使用生成器表达式构建生成器, 最终的效果一样. 调用 `__iter__` 方法会得到一个生成器对象.

生成器表达式是语法糖, 完全可以替换生成器函数.

### 标准库中的生成器函数

标准库提供了很多生成器, 有用于逐行迭代纯文本文件的对象, 还有出色的 `os.walk` 函数. 这个函数在遍历目录树的过程中产出文件名, 因此递归搜索文件系统像 for 循环那样简单.

标准库中的生成器大多在 `itertools` 和 `functools` 中, 表格中不代表所有.

**用于过滤的生成器函数**

| 模块      | 函数                      | 说明                                                         |
| --------- | ------------------------- | ------------------------------------------------------------ |
| itertools | compress(it, selector_it) | 并行处理两个可迭代的对象；如果 selector_it 中的元素是真值，产出 it 中对应的元素 |
| itertools | dropwhile(predicate, it)  | 处理 it，跳过 predicate 的计算结果为真值的元素，然后产出剩下的各个元素（不再进一步检查） |
| （内置）  | filter(predicate, it)     | 把 it 中的各个元素传给 predicate，如果 predicate(item) 返回真值，那么产出对应的元素；如果 predicate 是 None，那么只产出真值元素 |

**用于映射的生成器函数**

| 模块      | 函数                            | 说明                                                         |
| --------- | ------------------------------- | ------------------------------------------------------------ |
| itertools | accumulate(it, [func])          | 产出累积的总和；如果提供了 func，那么把前两个元素传给它，然后把计算结果和下一个元素传给它，以此类推，最后产出结果 |
| （内置）  | enumerate(iterable, start=0)    | 产出由两个元素组成的元组，结构是 (index, item)，其中 index 从 start 开始计数，item 则从 iterable 中获取 |
| （内置）  | map(func, it1, [it2, ..., itN]) | 把 it 中的各个元素传给func，产出结果；如果传入 N 个可迭代的对象，那么 func 必须能接受 N 个参数，而且要并行处理各个可迭代的对象 |

**合并多个可迭代对象的生成器函数**

| 模块      | 函数                    | 说明                                                         |
| --------- | ----------------------- | ------------------------------------------------------------ |
| itertools | chain(it1, ..., itN)    | 先产出 it1 中的所有元素，然后产出 it2 中的所有元素，以此类推，无缝连接在一起 |
| itertools | chain.from_iterable(it) | 产出 it 生成的各个可迭代对象中的元素，一个接一个，无缝连接在一起；it 应该产出可迭代的元素，例如可迭代的对象列表 |
| （内置）  | zip(it1, ..., itN)      | 并行从输入的各个可迭代对象中获取元素，产出由 N 个元素组成的元组，只要有一个可迭代的对象到头了，就默默地停止 |

### 新的句法：yield from

如果生成器函数需要产出另一个生成器生成的值, 传统的方式是嵌套的 for 循环, 例如, 我们要自己实现 `chain` 生成器:

```python
>>> def chain(*iterables):
...     for it in iterables:
...         for i in it:
...             yield i
...
>>> s = 'ABC'
>>> t = tuple(range(3))
>>> list(chain(s, t))
['A', 'B', 'C', 0, 1, 2]
```

`chain` 生成器函数把操作依次交给接收到的可迭代对象处理. 而改用 `yield from` 语句可以简化:

```python
>>> def chain(*iterables):
...     for i in iterables:
...         yield from i
...
>>> list(chain(s, t))
['A', 'B', 'C', 0, 1, 2]
```

可以看出, `yield from i` 取代一个 for 循环. 并且让代码读起来更流畅.

### 可迭代的归约函数

有些函数接受可迭代对象, 但仅返回单个结果, 这类函数叫规约函数.

| 模块      | 函数                        | 说明                                                         |
| --------- | --------------------------- | ------------------------------------------------------------ |
| （内置）  | sum(it, start=0)            | it 中所有元素的总和，如果提供可选的 start，会把它加上（计算浮点数的加法时，可以使用 math.fsum 函数提高精度） |
| （内置）  | all(it)                     | it 中的所有元素都为真值时返回 True，否则返回 False；all([]) 返回 True |
| （内置）  | any(it)                     | 只要 it 中有元素为真值就返回 True，否则返回 False；any([]) 返回 False |
| （内置）  | max(it, [key=,] [default=]) | 返回 it 中值最大的元素；*key 是排序函数，与 sorted 函数中的一样；如果可迭代的对象为空，返回 default |
| functools | reduce(func, it, [initial]) | 把前两个元素传给 func，然后把计算结果和第三个元素传给 func，以此类推，返回最后的结果；如果提供了 initial，把它当作第一个元素传入 |

## 第十五章: 上下文管理器和 else 块

本章讨论的是其他语言不常见的流程控制特性, 正因如此, python新手往往忽视或没有充分使用这些特性. 下面讨论的特性有:

- with 语句和上下文管理器
- for while try 语句的 else 子句

`with` 语句会设置一个临时的上下文, 交给上下文管理器对象控制, 并且负责清理上下文. 这么做能避免错误并减少代码量, 因此API更安全, 而且更易于使用. 除了自动关闭文件之外, with 块还有其他很多用途.

`else` 子句先做这个，选择性再做那个的作用.

### if语句之外的else块

这里的 else 不是在在 if 语句中使用的, 而是在 for while try 语句中使用的.

```python
for i in lst:
    if i > 10:
        break
else:
    print("no num bigger than 10")
```

`else` 子句的行为如下:

- `for` : 仅当 for 循环运行完毕时（即 for 循环没有被 `break` 语句中止）才运行 else 块。
- `while` : 仅当 while 循环因为条件为假值而退出时（即 while 循环没有被 `break` 语句中止）才运行 else 块。
- `try` : 仅当 try 块中没有异常抛出时才运行 else 块。

在所有情况下, 如果异常或者 `return` , `break` 或 `continue` 语句导致控制权跳到了复合语句的住块外, `else` 子句也会被跳过.

这一些情况下, 使用 else 子句通常让代码更便于阅读, 而且能省去一些麻烦, 不用设置控制标志作用的变量和额外的if判断.

### 上下文管理器和with块

上下文管理器对象的目的就是管理 `with` 语句, with 语句的目的是简化 `try/finally` 模式. 这种模式用于保证一段代码运行完毕后执行某项操作, 即便那段代码由于异常, `return` 或者 `sys.exit()` 调用而终止, 也会执行执行的操作. `finally` 子句中的代码通常用于释放重要的资源, 或者还原临时变更的状态.

上下文管理器协议包含 `__enter__` 和 `__exit__` 两个方法. with 语句开始运行时, 会在上下文管理器上调用 `__enter__` 方法, 待 with 语句运行结束后, 再调用 `__exit__` 方法, 以此扮演了 `finally` 子句的角色.

with 最常见的例子就是确保关闭文件对象.

上下文管理器调用 `__enter__` 没有参数, 而调用 `__exit__` 时, 会传入3个参数:

- `exc_type` : 异常类（例如 ZeroDivisionError）
- `exc_value` : 异常实例。有时会有参数传给异常构造方法，例如错误消息，这些参数可以使用 `exc_value.args` 获取
- `traceback` : `traceback` 对象

### contextlib模块中的实用工具

在ptyhon的标准库中, contextlib 模块中还有一些类和其他函数，使用范围更广。

- `closing`: 如果对象提供了 `close()` 方法，但没有实现 `__enter__/__exit__` 协议，那么可以使用这个函数构建上下文管理器。
- `suppress`: 构建临时忽略指定异常的上下文管理器。
- `@contextmanager`: 这个装饰器把简单的生成器函数变成上下文管理器，这样就不用创建类去实现管理器协议了。
- `ContextDecorator`: 这是个基类，用于定义基于类的上下文管理器。这种上下文管理器也能用于装饰函数，在受管理的上下文中运行整个函数。
- `ExitStack`: 这个上下文管理器能进入多个上下文管理器。with 块结束时，ExitStack 按照后进先出的顺序调用栈中各个上下文管理器的 `__exit__` 方法。如果事先不知道 with 块要进入多少个上下文管理器，可以使用这个类。例如，同时打开任意一个文件列表中的所有文件。

显然，在这些实用工具中，使用最广泛的是 `@contextmanager` 装饰器，因此要格外留心。这个装饰器也有迷惑人的一面，因为它与迭代无关，却要使用 yield 语句。

### 使用@contextmanager

@contextmanager 装饰器能减少创建上下文管理器的样板代码量, 因为不用定义 `__enter__` 和 `__exit__` 方法, 只需要实现一个 `yield` 语句的生成器.

```python
import sys
import contextlib
@contextlib.contextmanager
def looking_glass():

    original_write = sys.stdout.write
    def reverse_write(text):
        original_write(text[::-1])
    sys.stdout.write = reverse_write
    yield 'JABBERWOCKY'
    sys.stdout.write = original_write

with looking_glass() as f:
    print(f)        # YKCOWREBBAJ
    print("ABCD")   # DCBA
    
```

`yield` 语句起到了分割的作用, yield 语句前面的所有代码在 with 块开始时（即解释器调用 `__enter__` 方法时）执行， yield 语句后面的代码在 with 块结束时（即调用 `__exit__` 方法时）执行.

## 第十六章: 协程

为了理解协程的概念, 先从 `yield` 来说. `yield item` 会产出一个值, 提供给 `next(...)` 调用方; 此外还会做出让步, 暂停执行生成器, 让调用方继续工作, 直到需要使用另一个值时再调用 `next(...)` 从暂停的地方继续执行.

从句子语法上看, 协程与生成器类似, 都是通过 `yield` 关键字的函数. 可是, 在协程中, `yield` 通常出现在表达式的右边(datum = yield), 可以产出值, 也可以不产出(如果yield后面没有表达式, 那么会产出None). 协程可能会从调用方接收数据, 不过调用方把数据提供给协程使用的是 `.send(datum)` 方法. 而不是 `next(...)` . 通常, 调用方会把值推送给协程.

生成器调用方是一直索要数据, 而协程这是调用方可以想它传入数据, 协程也不一定要产出数据.

不管数据如何流动, `yield` 都是一种流程控制工具, 使用它可以实现写作式多任务: 协程可以把控制器让步给中心调度程序, 从而激活其他的协程.

### 生成器如何进化成协程

协程的底层框架实现后, 生成器API中增加了 `.send(value)` 方法. 生成器的调用方可以使用 `.send(...)` 来发送数据, 发送的数据会变成生成器函数中 `yield` 表达式的值. 因此, 生成器可以作为协程使用. 除了 `.send(...)` 方法, 还添加了 `.throw(...)` 和 `.close()` 方法, 用来让调用方抛出异常和终止生成器.

### 用作协程的生成器的基本行为

```python
>>> def simple_coroutine():
...     print('-> coroutine started')
...     x = yield
...     print('-> coroutine received:', x)
...
>>> my_coro = simple_coroutine()
>>> my_coro
<generator object simple_coroutine at 0x100c2be10>
>>> next(my_coro)
-> coroutine started
>>> my_coro.send(42)
-> coroutine received: 42
Traceback (most recent call last):
...
StopIteration
```

在 `yield` 表达式中, 如果协程只需从调用那接受数据, 那么产出的值是 `None` . 与创建生成器的方式一样, 调用函数得到生成器对象. 协程都要先调用 `next(...)` 函数, 因为生成器还没启动, 没在 yield 出暂定, 所以一开始无法发送数据. 如果控制器流动到协程定义体末尾, 会像迭代器一样抛出 `StopIteration` 异常.

使用协程的好处是不用加锁, 因为所有协程只在一个线程中运行, 他们是非抢占式的. 协程也有一些状态, 可以调用 `inspect.getgeneratorstate(...)` 来获得, 协程都是这4个状态中的一种:

- `'GEN_CREATED'` 等待开始执行。
- `'GEN_RUNNING'` 解释器正在执行。
- `'GEN_SUSPENDED'` 在 yield 表达式处暂停。
- `'GEN_CLOSED'` 执行结束。

只有在多线程应用中才能看到这个状态。此外，生成器对象在自己身上调用 `getgeneratorstate` 函数也行，不过这样做没什么用。

为了更好理解继承的行为, 来看看产生两个值的协程:

```python
>>> from inspect import getgeneratorstate
>>> def simple_coro2(a):
...     print('-> Started: a =', a)
...     b = yield a
...     print('-> Received: b =', b)
...     c = yield a + b
...     print('-> Received: c =', c)
...
>>> my_coro2 = simple_coro2(14)
>>> getgeneratorstate(my_coro2) # 协程处于未启动的状态
'GEN_CREATED'
>>> next(my_coro2)              # 向前执行到yield表达式, 产出值 a, 暂停并等待 b 赋值
-> Started: a = 14
14
>>> getgeneratorstate(my_coro2) # 协程处于暂停状态
'GEN_SUSPENDED'
>>> my_coro2.send(28)           # 数字28发给协程, yield 表达式中 b 得到28, 协程向前执行, 产出 a + b 值
-> Received: b = 28
42
>>> my_coro2.send(99)           # 同理, c 得到 99, 但是由于协程终止, 导致生成器对象抛出 StopIteration 异常
-> Received: c = 99
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
StopIteration
>>> getgeneratorstate(my_coro2) # 协程处于终止状态
'GEN_CLOSED'
```

关键的一点是, 协程在 `yield` 关键字所在的位置暂停执行. 对于 `b = yield a` 这行代码来说, 等到客户端代码再激活协程时才会设定 b 的值. 这种方式要花点时间才能习惯, 理解了这个, 才能弄懂异步编程中 `yield` 的作用. 对于实例的代码中函数 `simple_coro2` 的执行过程可以分为三个阶段:


### 示例：使用协程计算移动平均值

```python
def averager():
    total = 0.0
    count = 0
    average = None
    while True:
        term = yield average
        total += term
        count += 1
        average = total/count
```

这是一个动态计算平均值的协程代码, 这个无限循环表明, 它会一直接收值然后生成结果. 只有当调用方在协程上调用 `.close()` 方法, 或者没有该协程的引用时, 协程才会终止.

协程的好处是, 无需使用实例属性或者闭包, 在多次调用之间都能保持上下文.

### 预激协程的装饰器

如果没有执行 `next(...)` , 协程没什么用. 为了简化协程的用法, 有时会使用一个预激装饰器.

```python
from functools import wraps
def coroutine(func):
    """装饰器：向前执行到第一个`yield`表达式，预激`func`"""
    @wraps(func)
    def primer(*args,**kwargs): # 调用 primer 函数时，返回预激后的生成器
        gen = func(*args,**kwargs) # 调用被装饰的函数，获取生成器对象。
        next(gen)               # 预激生成器
        return gen              # 返回生成器
    return primer
```

### 终止协程和异常处理

协程中未处理的异常会向上冒泡, 传给 `next()` 函数或者 `send()` 的调用方. 如果这个异常没有处理, 会导致协程终止.

```python
>>> coro_avg.send(40)
40.0
>>> coro_avg.send(50)
45.0
>>> coro_avg.send('spam') # 传入会产生异常的值
Traceback (most recent call last):
...
TypeError: unsupported operand type(s) for +=: 'float' and 'str'
>>> coro_avg.send(60)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
StopIteration
```

这要求在协程内部要处理这些异常, 另外, 客户端代码也可以显示的发送异常给协程, 方法是 `throw` 和 `close` :

```
coro_avg.throw(ZeroDivisionError)
```

协程内部如果不能处理这个异常, 就会导致协程终止.

而 `close` 是致使在暂停的 `yield` 表达式处抛出 `GeneratorExit` 异常. 协程内部当然允许处理这个异常, 但收到这个异常时一定不能产出值, 不然解释器会抛出 `RuntimeError` 异常.

### 让协程返回值

```python
def averager():
    total = 0.0
    count = 0
    average = None
    while True:
        term = yield
        if term is None:
            break
        total += term
        count += 1
        average = total/count
        return (count, average)

coro_avg = averager()
next(coro_avg)
coro_avg.send(10)
coro_avg.send(30)
try:
    coro_avg.send(None)         # 发送 None 让协程终止
except StopIteration as exc:
    result = exc.value
```

为了返回值, 协程必须正常终止, 而正常终止的的协程会抛出 `StopIteration` 异常, 因此需要调用方处理这个异常.

### 使用yield from

`yield from` 是全新的语法结构. 它的作用比 `yield` 多很多.

```python
>>> def gen():
...     for c in 'AB':
...         yield c
...     for i in range(1, 3):
...         yield i
...
>>> list(gen())
['A', 'B', 1, 2]
```

可以改写成:

```python
>>> def gen():
...     yield from 'AB'
...     yield from range(1, 3)
...
>>> list(gen())
['A', 'B', 1, 2]
```

在生成器 `gen` 中使用 `yield form subgen()` 时, subgen 会得到控制权, 把产出的值传给 gen 的调用方, 既调用方可以直接调用 subgen. 而此时, gen 会阻塞, 等待 subgen 终止.

`yield from x` 表达式对 `x` 对象所做的第一件事是, 调用 `iter(x)` 获得迭代器. 因此, x 对象可以是任何可迭代对象.

这个语义过于复杂, 来看看作者 `Greg Ewing` 的解释:

> “把迭代器当作生成器使用，相当于把子生成器的定义体内联在 yield from 表达式
> 中。此外，子生成器可以执行 return 语句，返回一个值，而返回的值会成为 yield
> from 表达式的值。”

子生成器是从 `yield from <iterable>` 中获得的生成器. 而后, 如果调用方使用 `send()` 方法, 其实也是直接传给子生成器. 如果发送的是 `None` , 那么会调用子生成器的 `__next__()` 方法. 如果不是 `None` , 那么会调用子生成器的 `send()` 方法. 当子生成器抛出 `StopIteration` 异常, 那么委派生成器恢复运行. 任何其他异常都会向上冒泡, 传给委派生成器.

生成器在 `return expr` 表达式中会触发 `StopIteration` 异常.

## 第十七章: 使用期物处理并发

`"期物"` 是什么概念呢? 期物指一种对象, 表示异步执行的操作. 这个概念是 `concurrent.futures` 模块和 `asyncio` 包的基础.

### 示例：网络下载的三种风格

为了高效处理网络io, 需要使用并发, 因为网络有很高的延迟, 所以为了不浪费 CPU 周期去等待.

以一个下载网络 20 个图片的程序看, 串行下载的耗时 7.18s . 多线程的下载耗时 1.40s, asyncio的耗时 1.35s . 两个并发下载的脚本之间差异不大, 当对于串行的来说, 快了很多.

### 阻塞型I/O和GIL

CPython解释器不是线程安全的, 因此有全局解释锁(GIL), 一次只允许使用一个线程执行 python 字节码, 所以一个python进程不能同时使用多个 CPU 核心.

python程序员编写代码时无法控制 GIL, 然而, 在标准库中所有执行阻塞型I/O操作的函数, 在登台操作系统返回结果时都会释放GIL. 这意味着IO密集型python程序能从中受益.

### 使用concurrent.futures模块启动进程

一个python进程只有一个 GIL. 多个python进程就能绕开GIL, 因此这种方法就能利用所有的 CPU 核心. `concurrent.futures` 模块就实现了真正的并行计算, 因为它使用 `ProcessPoolExecutor` 把工作交个多个python进程处理.

`ProcessPoolExecutor` 和 `ThreadPoolExecutor` 类都实现了通用的 `Executor` 接口, 因此使用 `concurrent.futures` 能很轻松把基于线程的方案转成基于进程的方案.

```python
def download_many(cc_list):
    workers = min(MAX_WORKERS, len(cc_list))
    with futures.ThreadPoolExecutor(workers) as executor:
        res = executor.map(download_one, sorted(cc_list))
```

改成:

```python
def download_many(cc_list):
    with futures.ProcessPoolExecutor() as executor:
        res = executor.map(download_one, sorted(cc_list))
```

`ThreadPoolExecutor.__init__` 方法需要 `max_workers` 参数，指定线程池中线程的数量; 在 `ProcessPoolExecutor` 类中, 这个参数是可选的.

## 第十八章: 使用 asyncio 包处理并发

> 并发是指一次处理多件事。
> 并行是指一次做多件事。
> 二者不同，但是有联系。
> 一个关于结构，一个关于执行。
> 并发用于制定方案，用来解决可能（但未必）并行的问题。—— Rob Pike Go 语言的创造者之一

并行是指两个或者多个事件在同一时刻发生, 而并发是指两个或多个事件在同一时间间隔发生. 真正运行并行需要多个核心, 现在笔记本一般有 4 个 CPU 核心, 但是通常就有超过 100 个进程同时运行. 因此, 实际上大多数进程都是并发处理的, 而不是并行处理. 计算机始终运行着 100 多个进程, 确保每个进程都有机会取得发展, 不过 CPU 本身同时做的事情不会超过四件.

本章介绍 `asyncio` 包, 这个包使用事件循环驱动的协程实现并发. 这个库有龟叔亲自操刀. `asyncio` 大量使用 `yield from` 表达式, 因此不兼容 python3.3 以下的版本.

### 线程与协程对比

一个借由 `threading` 模块使用线程, 一个借由 `asyncio` 包使用协程实现来进行比对.

```python
import threading
import itertools
import time

def spin(msg, done):  # 这个函数会在单独的线程中运行
    for char in itertools.cycle('|/-\\'):  # 这其实是个无限循环，因为 itertools.cycle 函数会从指定的序列中反复不断地生成元素
        status = char + ' ' + msg
        print(status)
        if done.wait(.1):  # 如果进程被通知等待, 那就退出循环
            break

def slow_function():  # 假设这是耗时的计算
    # pretend waiting a long time for I/O
    time.sleep(3)  # 调用 sleep 函数会阻塞主线程，不过一定要这么做，以便释放 GIL，创建从属线程
    return 42

def supervisor():  # 这个函数设置从属线程，显示线程对象，运行耗时的计算，最后杀死线程。
    done = threading.Event()
    spinner = threading.Thread(target=spin,
                               args=('thinking!', done))
    print('spinner object:', spinner)  # 显示从属线程对象。输出类似于 <Thread(Thread-1, initial)>
    spinner.start()  # 启动从属线程
    result = slow_function()  # 运行 slow_function 函数，阻塞主线程。同时，从属线程以动画形式显示旋转指针
    done.set()  # 改变 signal 的状态；这会终止 spin 函数中的那个 for 循环
    spinner.join()  # 等待 spinner 线程结束
    return result

if __name__ == '__main__':
    result = supervisor()
    print('Answer:', result)
```

这是使用 `threading` 的案例, 让子线程在 3 秒内不断打印, 在python中, 没有提供终止线程的API. 若想关闭线程, 必须给线程发送消息.

下面看看使用 `@asyncio.coroutine` 装饰器替代协程, 实现相同的行为:

```python
import asyncio
import itertools

@asyncio.coroutine  # 交给 asyncio 处理的协程要使用 @asyncio.coroutine 装饰
def spin(msg):
    for char in itertools.cycle('|/-\\'):
        status = char + ' ' + msg
        print(status)
        try:
            yield from asyncio.sleep(.1)  # 使用 yield from asyncio.sleep(.1) 代替 time.sleep(.1)，这样的休眠不会阻塞事件循环。
        except asyncio.CancelledError:  # 如果 spin 函数苏醒后抛出 asyncio.CancelledError 异常，其原因是发出了取消请求，因此退出循环。
            break

@asyncio.coroutine
def slow_function():  # slow_function 函数是协程，在用休眠假装进行 I/O 操作时，使用 yield from 继续执行事件循环。
    # pretend waiting a long time for I/O
    yield from asyncio.sleep(3)  # yield from asyncio.sleep(3) 表达式把控制权交给主循环，在休眠结束后恢复这个协程。
    return 42

@asyncio.coroutine
def supervisor():  # supervisor 函数也是协程
    spinner = asyncio.async(spin('thinking!'))  # asyncio.async(...) 函数排定 spin 协程的运行时间，使用一个 Task 对象包装spin 协程，并立即返回。
    print('spinner object:', spinner)
    result = yield from slow_function()  # 驱动 slow_function() 函数。结束后，获取返回值。
                                         # 同时，事件循环继续运行，因为slow_function 函数最后使用 yield from asyncio.sleep(3) 表达式把控制权交回给了主循环。
    spinner.cancel()  # Task 对象可以取消；取消后会在协程当前暂停的 yield 处抛出 asyncio.CancelledError 异常。协程可以捕获这个异常，也可以延迟取消，甚至拒绝取消。
    return result

if __name__ == '__main__':
    loop = asyncio.get_event_loop()  # 获取事件循环的引用
    result = loop.run_until_complete(supervisor())  # 驱动 supervisor 协程，让它运行完毕；这个协程的返回值是这次调用的返回值。
    loop.close()
    print('Answer:', result)
```

`asyncio` 包使用的协程是比较严格的定义, 适合 asyncio API 的协程在定义体中必须使用 `yield from` , 而不是使用 `yield` . 此外, `asyncio` 的协程要由调用方驱动, 例如 `asyncio.async(...)` , 从而驱动协程. 最后由 `@asyncio.coroutine` 装饰器应用在协程上.

这两种 `supervisor` 实现之间的主要区别概述如下:

- `asyncio.Task` 对象差不多与 `threading.Thread` 对象等效。“Task对象像是实现协作式多任务的库（例如 gevent）中的绿色线程（green thread）”。
- `Task` 对象用于驱动协程，`Thread` 对象用于调用可调用的对象。
- `Task` 对象不由自己动手实例化，而是通过把协程传给 `asyncio.async(...)` 函数或 `loop.create_task(...)` 方法获取。
- 获取的 `Task` 对象已经排定了运行时间（例如，由 `asyncio.async` 函数排定）；Thread 实例则必须调用 start 方法，明确告知让它运行。
- 在线程版 `supervisor` 函数中，`slow_function` 函数是普通的函数，直接由线程调用。在异步版 `supervisor` 函数中，`slow_function` 函数是协程，由 `yield from` 驱动。
- 没有 API 能从外部终止线程，因为线程随时可能被中断，导致系统处于无效状态。如果想终止任务，可以使用 `Task.cancel()` 实例方法，在协程内部抛出 `CancelledError` 异常。协程可以在暂停的 `yield` 处捕获这个异常，处理终止请求。
- `supervisor` 协程必须在 `main` 函数中由 `loop.run_until_complete` 方法执行。

多线程编程是比较困难的, 因为调度程序任何时候都能中断线程, 必须记住保留锁, 去保护程序中重要部分, 防止多线程在执行的过程中断.

而协程默认会做好全方位保护, 以防止中断. 我们必须显示产出才能让程序的余下部分运行. 对协程来说, 无需保留锁, 而在多个线程之间同步操作, 协程自身就会同步, 因为在任意时刻, 只有一个协程运行.

### 从期物、任务和协程中产出

在 `asyncio` 包中, 期物和协程关系紧密, 因为可以使用 `yield from` 从 `asyncio.Future` 对象中产出结果. 也就是说, 如果 `foo` 是协程函数, 或者是返回 `Future` 或 `Task` 实例的普通函数, 那么可以用 `res = yield from foo()` .

为了执行这个操作, 必须排定协程的运行时间, 然后使用 `asyncio.Task` 对象包装协程. 对协程来说, 获取 `Task` 对象主要有两种方式:

- `asyncio.async(coro_or_future, *, loop=None)` : 这个函数统一了协程和期物：第一个参数可以是二者中的任何一个。如果是 Future 或 Task 对象，那就原封不动地返回。如果是协程，那么 async 函数会调用 `loop.create_task(...)` 方法创建 Task 对象。loop 关键字参数是可选的，用于传入事件循环；如果没有传入，那么 async 函数会通过调用 `asyncio.get_event_loop()` 函数获取循环对象。
- `BaseEventLoop.create_task(coro)` : 这个方法排定协程的执行时间，返回一个 `asyncio.Task` 对象。如果在自定义的 `BaseEventLoop` 子类上调用，返回的对象可能是外部库（如 Tornado）中与 Task 类兼容的某个类的实例。

`asyncio` 包中有多个函数会自动(使用 `asyncio.async` 函数) 把参数指定的协程包装在 `asyncio.Task` 对象中.

### 使用asyncio和aiohttp包下载

`asyncio` 包只直接支持 TCP 和 UDP. 如果像使用 HTTP 或其他协议, 就需要借助第三方包. 使用的几乎都是 `aiohttp` 包. 以下载图片为例:

```python
import asyncio
import aiohttp
from flags import BASE_URL, save_flag, show, main

@asyncio.coroutine
def get_flag(cc): # 协程应该使用 @asyncio.coroutine 装饰。
    url = '{}/{cc}/{cc}.gif'.format(BASE_URL, cc=cc.lower())
    resp = yield from aiohttp.request('GET', url) # 阻塞的操作通过协程实现
    image = yield from resp.read()                # 读取响应内容是一项单独的异步操作
    return image

@asyncio.coroutine
def download_one(cc): # download_one 函数也必须是协程，因为用到了 yield from
    image = yield from get_flag(cc)
    show(cc)
    save_flag(image, cc.lower() + '.gif')
    return cc
def download_many(cc_list):
    loop = asyncio.get_event_loop() # 获取事件循环底层实现的引用
    to_do = [download_one(cc) for cc in sorted(cc_list)] # 调用 download_one 函数获取各个国旗，然后构建一个生成器对象列表
    wait_coro = asyncio.wait(to_do) # 虽然函数的名称是 wait，但它不是阻塞型函数。wait 是一个协程，等传给它的所有协程运行完毕后结束
    res, _ = loop.run_until_complete(wait_coro) # 执行事件循环，直到 wait_coro 运行结束
    loop.close()  # 关闭事件循环
    return len(res)
if __name__ == '__main__':
    main(download_many)
```

`asyncio.wait(...)` 协程参数是一个由期物或协程构成的可迭代对象, wait 会分别把各个协程装进一个 `Task` 对象. 最终的结果是, wait 处理的所有对象都通过某种方式变成 `Future` 类的实例. wait 是协程函数, 因此返回的是一个协程或生成器对象. 为了驱动协程, 我们把协程传给 `loop.run_until_complete(...)` 方法.

`loop.run_until_complete` 方法的参数是一个期物或协程. 如果是协程, `run_until_complete` 方法与 wait 函数一样, 把协程包装进一个 `Task` 对象中. 因为协程都是由 `yield from` 驱动, 这正是 `run_until_complete` 对 wait 返回返回的 wait_coro 对象所做的事. 运行结束后返回两个元素, 第一个是是结束的期物, 第二个是未结束的期物.

### 避免阻塞型调用

有两种方法能避免阻塞型调用中止整个应用程序的进程:

- 在单独的线程中运行各个阻塞型操作
- 把每个阻塞型操作转换成非阻塞的异步调用使用

多线程是可以的, 但是会消耗比较大的内存. 为了降低内存的消耗, 通常使用回调来实现异步调用. 这是一种底层概念, 类似所有并发机制中最古老最原始的那种--硬件中断. 使用回调时, 我们不等待响应, 而是注册一个函数, 在发生某件事时调用. 这样, 所有的调用都是非阻塞的.

异步应用程序底层的事件循环能依靠基础设置的中断, 线程, 轮询和后台进程等待等, 确保多个并发请求能取得进展并最终完成, 这样才能使用回调. 事件循环获得响应后, 会回过头来调用我们指定的回调. 如果做法正确, 事件循环和应用代码公共的主线程绝不会阻塞.

把生成器当做协程使用是异步编程的另一种方式. 对事件循环来说, 调用回调与在暂停的协程上调用 `.send()` 效果差不多.

### 使用Executor对象，防止阻塞事件循环

访问本地文件会阻塞, 而CPython底层在阻塞型I/O调用时会释放 GIL, 因此另一个线程可以继续.

因为 `asyncio` 事件不是通过多线程来完成, 因此 `save_flag` 用来保存图片的函数阻塞了与 `asyncio` 事件循环共用的唯一线程, 因此保存文件时, 真个应用程序都会冻结. 这个问题的解决办法是, 使用事件循环对象的 `run_in_executor` 方法.

`asyncio` 的事件循环背后维护者一个 `ThreadPoolExecutor` 对象, 我们可以调用 `run_in_executor` 方法, 把可调用的对象发给它执行:

```python
@asyncio.coroutine
    def download_one(cc, base_url, semaphore, verbose):
    try:
        with (yield from semaphore):
            image = yield from get_flag(base_url, cc)
    except web.HTTPNotFound:
        status = HTTPStatus.not_found
        msg = 'not found'
    except Exception as exc:
        raise FetchError(cc) from exc
    else:
        loop = asyncio.get_event_loop() # 获取事件循环对象的引用
        loop.run_in_executor(None, # run_in_executor 方法的第一个参数是 Executor 实例；如果设为 None，使用事件循环的默认 ThreadPoolExecutor 实例。
        save_flag, image, cc.lower() + '.gif') # 余下的参数是可调用的对象，以及可调用对象的位置参数 
        status = HTTPStatus.ok
        msg = 'OK'
    if verbose and msg:
        print(cc, msg)
    return Result(status, cc)
```
