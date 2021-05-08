
流畅的python笔记：

主要参考：

https://hellowac.github.io/blog/programing/

https://github.com/StdioA/fluent-python-notes/tree/master/markdown

https://segmentfault.com/a/1190000011568813



# 1  Python 数据类型
> Guido 对语言设计美学的深入理解让人震惊。我认识不少很不错的编程语言设计者，他们设计出来的东西确实很精彩，但是从来都不会有用户。Guido 知道如何在理论上做出一定妥协，设计出来的语言让使用者觉得如沐春风，这真是不可多得。  
> ——Jim Hugunin  
>   Jython 的作者，AspectJ 的作者之一，.NET DLR 架构师

Python 最好的品质之一是**一致性**：你可以轻松理解 Python 语言，并通过 Python 的语言特性在类上定义**规范的接口**，来支持 Python 的核心语言特性，从而写出具有“Python 风格”的对象。  
Python 解释器在碰到特殊的句法时，会使用特殊方法（我们称之为魔术方法）去激活一些基本的对象操作。如 `my_c[key]` 语句执行时，就会调用 `my_c.__getitem__` 函数。这些特殊方法名能让你自己的对象实现和支持一下的语言构架，并与之交互：
* 迭代
* 集合类
* 属性访问
* 运算符重载
* 函数和方法的调用
* 对象的创建和销毁
* 字符串表示形式和格式化
* 管理上下文（即 `with` 块）


```python
# 通过实现魔术方法，来让内置函数支持你的自定义对象
# https://github.com/fluentpython/example-code/blob/master/01-data-model/frenchdeck.py
import collections
import random

Card = collections.namedtuple('Card', ['rank', 'suit'])

class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()

    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits
                                        for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, position):
        return self._cards[position]

deck = FrenchDeck()
# 实现 __length__ 以支持 len
print(len(deck))
# 实现 __getitem__ 以支持下标操作
print(deck[1])
print(deck[5::13])
# 有了这些操作，我们就可以直接对这些对象使用 Python 的自带函数了
print(random.choice(deck))
```

    52
    Card(rank='3', suit='spades') [Card(rank='7', suit='spades'), Card(rank='7', suit='diamonds'), Card(rank='7', suit='clubs'), Card(rank='7', suit='hearts')]
    Card(rank='6', suit='diamonds')


Python 支持的所有魔术方法，可以参见 Python 文档 [Data Model](https://docs.python.org/3/reference/datamodel.html) 部分。

比较重要的一点：不要把 `len`，`str` 等看成一个 Python 普通方法：由于这些操作的频繁程度非常高，所以 Python 对这些方法做了特殊的实现：它可以让 Python 的内置数据结构走后门以提高效率；但对于自定义的数据结构，又可以在对象上使用通用的接口来完成相应工作。但在代码编写者看来，`len(deck)` 和 `len([1,2,3])` 两个实现可能差之千里的操作，在 Python 语法层面上是高度一致的。



# 2 序列构成的数组
> 你可能注意到了，之前提到的几个操作可以无差别地应用于文本、列表和表格上。  
> 我们把文本、列表和表格叫作数据火车……FOR 命令通常能作用于数据火车上。  
> ——Geurts、Meertens 和 Pemberton  
>   *ABC Programmer’s Handbook*

* 容器序列  
    `list`、`tuple` 和 `collections.deque` 这些序列能存放不同类型的数据。
* 扁平序列  
    `str`、`bytes`、`bytearray`、`memoryview` 和 `array.array`，这类序列只能容纳一种类型。
   
容器序列存放的是它们所包含的任意类型的对象的**引用**，而扁平序列里存放的**是值而不是引用**。换句话说，扁平序列其实是一段连续的内存空间。由此可见扁平序列其实更加紧凑，但是它里面只能存放诸如字符、字节和数值这种基础类型。

序列类型还能按照能否被修改来分类。
* 可变序列  
    `list`、`bytearray`、`array.array`、`collections.deque` 和 `memoryview`。
* 不可变序列  
    `tuple`、`str` 和 `bytes`


```python
# 列表推导式和生成器表达式
symbols = "列表推导式"
[ord(symbol) for symbol in symbols]
(ord(symbol) for symbol in symbols)
```


```python
# 因为 pack/unpack 的存在，元组中的元素会凸显出它们的位置信息
first, *others, last = (1, 2, 3, 4, 5)
print(first, others, last)
# 当然后面很多可迭代对象都支持 unpack 了…
```


```python
# namedtuple
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print(p, p.x, p.y)
# _asdict() 会返回 OrderedDict
print(p._asdict())
```


```python
# 为什么切片(slice)不返回最后一个元素
a = list(range(6))
# 使用同一个数即可将列表进行分割
print(a[:2], a[2:])
```


```python
# Ellipsis
def test(first, xxx, last):
    print(xxx)
    print(type(xxx))
    print(xxx == ...)
    print(xxx is ...)
    return first, last

# ... 跟 None 一样，有点神奇
print(test(1, ..., 2))
```

### bisect 二分查找


```python
import bisect
def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
    i = bisect.bisect(breakpoints, score)
    return grades[i]

print([grade(score) for score in [33, 99, 77, 70, 89, 90, 100]])

a = list(range(0, 100, 10))
# 插入并保持有序
bisect.insort(a, 55)
print(a)
```

### Array
> 虽然列表既灵活又简单，但面对各类需求时，我们可能会有更好的选择。比如，要存放 1000 万个浮点数的话，数组（array）的效率要高得多，因为数组在背后存的并不是 float 对象，而是数字的机器翻译，也就是字节表述。这一点就跟 C 语言中的数组一样。再比如说，如果需要频繁对序列做先进先出的操作，deque（双端队列）的速度应该会更快。

`array.tofile` 和 `fromfile` 可以将数组以二进制格式写入文件，速度要比写入文本文件快很多，文件的体积也小。

> 另外一个快速序列化数字类型的方法是使用 pickle（https://docs.python.org/3/library/pickle.html）模块。pickle.dump 处理浮点数组的速度几乎跟array.tofile 一样快。不过前者可以处理几乎所有的内置数字类型，包含复数、嵌套集合，甚至用户自定义的类。前提是这些类没有什么特别复杂的实现。

array 具有 `type code` 来表示数组类型：具体可见 [array 文档](https://docs.python.org/3/library/array.html).

### memoryview
> memoryview.cast 的概念跟数组模块类似，能用不同的方式读写同一块内存数据，而且内容字节不会随意移动。


```python
import array

arr = array.array('h', [1, 2, 3])
memv_arr = memoryview(arr)
# 把 signed short 的内存使用 char 来呈现
memv_char = memv_arr.cast('B') 
print('Short', memv_arr.tolist())
print('Char', memv_char.tolist())
memv_char[1] = 2  # 更改 array 第一个数的高位字节
# 0x1000000001
print(memv_arr.tolist(), arr)
print('-' * 10)
bytestr = b'123'
# bytes 是不允许更改的
try:
    bytestr[1] = '3'
except TypeError as e:
    print(repr(e))
memv_byte = memoryview(bytestr)
print('Memv_byte', memv_byte.tolist())
# 同样这块内存也是只读的
try:
    memv_byte[1] = 1
except TypeError as e:
    print(repr(e))

```

### Deque
`collections.deque` 是比 `list` 效率更高，且**线程安全**的双向队列实现。

除了 collections 以外，以下 Python 标准库也有对队列的实现：
* queue.Queue (可用于线程间通信)
* multiprocessing.Queue (可用于进程间通信)
* asyncio.Queue
* heapq



# 3 字典和集合

> 字典这个数据结构活跃在所有 Python 程序的背后，即便你的源码里并没有直接用到它。  
> ——A. M. Kuchling 

可散列对象需要实现 `__hash__` 和 `__eq__` 函数。  
如果两个可散列对象是相等的，那么它们的散列值一定是一样的。


```python
# 字典提供了很多种构造方法
a = dict(one=1, two=2, three=3)
b = {'one': 1, 'two': 2, 'three': 3} 
c = dict(zip(['one', 'two', 'three'], [1, 2, 3])) 
d = dict([('two', 2), ('one', 1), ('three', 3)]) 
e = dict({'three': 3, 'one': 1, 'two': 2})
a == b == c == d == e
```


```python
# 字典推导式
r = range(5)
d = {n * 2: n for n in r if n < 3}
print(d)
# setdefault
for n in r:
    d.setdefault(n, 0)
print(d)
```


```python
# defaultdcit & __missing__
class mydefaultdict(dict):
    def __init__(self, value, value_factory):
        super().__init__(value)
        self._value_factory = value_factory

    def __missing__(self, key):
        # 要避免循环调用
        # return self[key]
        self[key] = self._value_factory()
        return self[key]

d = mydefaultdict({1:1}, list)
print(d[1])
print(d[2])
d[3].append(1)
print(d)
```

### 字典的变种
* collections.OrderedDict
* collections.ChainMap (容纳多个不同的映射对象，然后在进行键查找操作时会从前到后逐一查找，直到被找到为止)
* collections.Counter
* colllections.UserDict (dict 的 纯 Python 实现)


```python
# UserDict
# 定制化字典时，尽量继承 UserDict 而不是 dict
from collections import UserDict

class mydict(UserDict):
    def __getitem__(self, key):
        print('Getting key', key)
        return super().__getitem__(key)

d = mydict({1:1})
print(d[1], d[2])
```


```python
# MyppingProxyType 用于构建 Mapping 的只读实例
from types import MappingProxyType

d = {1: 1}
d_proxy = MappingProxyType(d)
print(d_proxy[1])
try:
    d_proxy[1] = 1
except Exception as e:
    print(repr(e))

d[1] = 2
print(d_proxy[1])
```


```python
# set 的操作
# 子集 & 真子集
a, b = {1, 2}, {1, 2}
print(a <= b, a < b)

# discard
a = {1, 2, 3}
a.discard(3)
print(a)

# pop
print(a.pop(), a.pop())
try:
    a.pop()
except Exception as e:
    print(repr(e))
```

### 集合字面量
除空集之外，集合的字面量——`{1}`、`{1, 2}`，等等——看起来跟它的数学形式一模一样。**如果是空集，那么必须写成 `set()` 的形式**，否则它会变成一个 `dict`.  
跟 `list` 一样，字面量句法会比 `set` 构造方法要更快且更易读。

### 集合和字典的实现
集合和字典采用散列表来实现：
1. 先计算 key 的 `hash`, 根据 hash 的某几位（取决于散列表的大小）找到元素后，将该元素与 key 进行比较
2. 若两元素相等，则命中
3. 若两元素不等，则发生散列冲突，使用线性探测再散列法进行下一次查询。

这样导致的后果：
1. 可散列对象必须支持 `hash` 函数；
2. 必须支持 `__eq__` 判断相等性；
3. 若 `a == b`, 则必须有 `hash(a) == hash(b)`。

注：所有由用户自定义的对象都是可散列的，因为他们的散列值由 id() 来获取，而且它们都是不相等的。


### 字典的空间开销
由于字典使用散列表实现，所以字典的空间效率低下。使用 `tuple` 代替 `dict` 可以有效降低空间消费。  
不过：内存太便宜了，不到万不得已也不要开始考虑这种优化方式，**因为优化往往是可维护性的对立面**。

往字典中添加键时，如果有散列表扩张的情况发生，则已有键的顺序也会发生改变。所以，**不应该在迭代字典的过程各种对字典进行更改**。


```python
# 字典中就键的顺序取决于添加顺序

keys = [1, 2, 3]
dict_ = {}
for key in keys:
    dict_[key] = None

for key, dict_key in zip(keys, dict_):
    print(key, dict_key)
    assert key == dict_key

# 字典中键的顺序不会影响字典比较
```

# 4 文本和字节序列

> 人类使用文本，计算机使用字节序列  
> —— Esther Nam 和 Travis Fischer  "Character Encoding and Unicode in Python"

Python 3 明确区分了人类可读的文本字符串和原始的字节序列。  
隐式地把字节序列转换成 Unicode 文本（的行为）已成过去。

### 字符与编码
字符的标识，及**码位**，是 0~1114111 的数字，在 Unicode 标准中用 4-6 个十六进制数字表示，如 A 为 U+0041, 高音谱号为 U+1D11E，😂 为 U+1F602.  
字符的具体表述取决于所用的**编码**。编码时在码位与字节序列自减转换时使用的算法。  
把码位转换成字节序列的过程是**编码**，把字节序列转成码位的过程是**解码**。

### 序列类型
Python 内置了两种基本的二进制序列类型：不可变的 `bytes` 和可变的 `bytearray`


```python
# 基本的编码
content = "São Paulo"
for codec in ["utf_8", "utf_16"]:
    print(codec, content.encode(codec))

# UnicodeEncodeError
try:
    content.encode('cp437')
except UnicodeEncodeError as e:
    print(e)

# 忽略无法编码的字符
print(content.encode('cp437', errors='ignore'))
# 把无法编码的字符替换成 ?
print(content.encode('cp437', errors='replace'))
# 把无法编码的字符替换成 xml 实体
print(content.encode('cp437', errors='xmlcharrefreplace'))

# 还可以自己设置错误处理方式
# https://docs.python.org/3/library/codecs.html#codecs.register_error
```

    utf_8 b'S\xc3\xa3o Paulo'
    utf_16 b'\xff\xfeS\x00\xe3\x00o\x00 \x00P\x00a\x00u\x00l\x00o\x00'
    'charmap' codec can't encode character '\xe3' in position 1: character maps to <undefined>
    b'So Paulo'
    b'S?o Paulo'
    b'S&#227;o Paulo'



```python
# 基本的解码
# 处理 UnicodeDecodeError
octets = b'Montr\xe9al'
print(octets.decode('cp1252'))
print(octets.decode('iso8859_7'))
print(octets.decode('koi8_r'))
try:
    print(octets.decode('utf-8'))
except UnicodeDecodeError as e:
    print(e)

# 将错误字符替换成 � (U+FFFD)
octets.decode('utf-8', errors='replace')
```


```python
# Python3 可以使用非 ASCII 名称
São = 'Paulo'
# 但是不能用 Emoji…
```

可以用 `chardet` 检测字符所使用的编码

BOM：字节序标记 (byte-order mark)：  
`\ufffe` 为字节序标记，放在文件开头，UTF-16 用它来表示文本以大端表示(`\xfe\xff`)还是小端表示(`\xff\xfe`)。  
UTF-8 编码并不需要 BOM，但是微软还是给它加了 BOM，非常烦人。

### 处理文本文件
处理文本文件的最佳实践是“三明治”：要尽早地把输入的字节序列解码成字符串，尽量晚地对字符串进行编码输出；在处理逻辑中只处理字符串对象，不应该去编码或解码。  
除非想判断编码，否则不要再二进制模式中打开文本文件；即便如此，也应该使用 `Chardet`，而不是重新发明轮子。  
常规代码只应该使用二进制模式打开二进制文件，比如图像。

### 默认编码
可以使用 `sys.getdefaultincoding()` 获取系统默认编码；  
Linux 的默认编码为 `UTF-8`，Windows 系统中不同语言设置使用的编码也不同，这导致了更多的问题。  
`locale.getpreferredencoding()` 返回的编码是最重要的：这是打开文件的默认编码，也是重定向到文件的 `sys.stdout/stdin/stderr` 的默认编码。不过这个编码在某些系统中是可以改的…  
所以，关于编码默认值的最佳建议是：别依赖默认值。

### Unicode 编码方案
```python
a = 'café'
b = 'cafe\u0301'
print(a, b)                       # café café
print(ascii(a), ascii(b))         # 'caf\xe9' 'cafe\u0301'
print(len(a), len(b), a == b)     # 4 5 False
```

在 Unicode 标准中，é 和 e\u0301 这样的序列叫“标准等价物”，应用程序应将它视为相同的字符。但 Python 看到的是不同的码位序列，因此判断两者不相同。  
我们可以用 `unicodedata.normalize` 将 Unicode 字符串规范化。有四种规范方式：NFC, NFD, NFKC, NFKD

NFC 使用最少的码位构成等价的字符串，而 NFD 会把组合字符分解成基字符和单独的组合字符。  
NFKC 和 NFKD 是出于兼容性考虑，在分解时会将字符替换成“兼容字符”，这种情况下会有格式损失。  
兼容性方案可能会损失或曲解信息（如 "4²" 会被转换成 "42"），但可以为搜索和索引提供便利的中间表述。

> 使用 NFKC 和 NFKC 规范化形式时要小心，而且只能在特殊情况中使用，例如搜索和索引，而不能用户持久存储，因为这两种转换会导致数据损失。


```python
from unicodedata import normalize, name
# Unicode 码位
a = 'café'
b = 'cafe\u0301'
print(a, b)
print(ascii(a), ascii(b))
print(len(a), len(b), a == b)

## NFC 和 NFD
print(len(normalize('NFC', a)), len(normalize('NFC', b)))
print(len(normalize('NFD', a)), len(normalize('NFD', b)))
print(len(normalize('NFC', a)) == len(normalize('NFC', b)))

print('-' * 15)
# NFKC & NFKD
s = '\u00bd'
l = [s, normalize('NFKC', s),  normalize('NFKD', s)]
print(*l)
print(*map(ascii, l))
micro = 'μ'
l = [s, normalize('NFKC', micro)]
print(*l)
print(*map(ascii, l))
print(*map(name, l), sep='; ')
```

### Unicode 数据库
`unicodedata` 库中提供了很多关于 Unicode 的操作及判断功能，比如查看字符名称的 `name`，判断数字大小的 `numric` 等。  
文档见 <https://docs.python.org/3.7/library/unicodedata.html>.


```python
import unicodedata
print(unicodedata.name('½'))
print(unicodedata.numeric('½'), unicodedata.numeric('卅'))
```

    VULGAR FRACTION ONE HALF
    0.5 30.0



```python
# 处理鬼符：按字节序将无法处理的字节序列依序替换成 \udc00 - \udcff 之间的码位
x = 'digits-of-π'
s = x.encode('gb2312')
print(s)                                              # b'digits-of-\xa6\xd0'
ascii_err = s.decode('ascii', 'surrogateescape')
print(ascii_err)                                      # 'digits-of-\udca6\udcd0'
print(ascii_err.encode('ascii', 'surrogateescape'))   # b'digits-of-\xa6\xd0'
```

# 5 一等函数

> 不管别人怎么说或怎么想，我从未觉得 Python 受到来自函数式语言的太多影响。我非常熟悉命令式语言，如 C 和 Algol 68，虽然我把函数定为一等对象，但是我并不把 Python 当作函数式编程语言。  
> —— Guido van Rossum: Python 仁慈的独裁者

在 Python 中，函数是一等对象。  
编程语言理论家把“一等对象”定义为满足下述条件的程序实体：
* 在运行时创建
* 能赋值给变量或数据结构中的元素
* 能作为参数传给函数
* 能作为函数的返回结果


```python
# 高阶函数：有了一等函数（作为一等对象的函数），就可以使用函数式风格编程
fruits = ['strawberry', 'fig', 'apple', 'cherry', 'raspberry', 'banana']
print(list(sorted(fruits, key=len)))  # 函数 len 成为了一个参数

# lambda 函数 & map
fact = lambda x: 1 if x == 0 else x * fact(x-1)
print(list(map(fact, range(6))))

# reduce
from functools import reduce
from operator import add
print(reduce(add, range(101)))

# all & any
x = [0, 1]
print(all(x), any(x))
```

    ['fig', 'apple', 'cherry', 'banana', 'raspberry', 'strawberry']
    [1, 1, 2, 6, 24, 120]
    5050
    False True


Python 的可调用对象
* 用户定义的函数：使用 `def` 或 `lambda` 创建
* 内置函数：如 `len` 或 `time.strfttime`
* 内置方法：如 `dict.get`（没懂这俩有什么区别…是说这个函数作为对象属性出现吗？）
* 类：先调用 `__new__` 创建实例，再对实例运行 `__init__` 方法
* 类的实例：如果类上定义了 `__call__` 方法，则实例可以作为函数调用
* 生成器函数：调用生成器函数会返回生成器对象


```python
# 获取函数中的信息
# 仅限关键词参数
def f(a, *, b):
    print(a, b)
f(1, b=2)

# 获取函数的默认参数
# 原生的方法
def f(a, b=1, *, c, d=3):
    pass

def parse_defaults(func):
    code = func.__code__
    argcount = code.co_argcount  # 2
    varnames = code.co_varnames  # ('a', 'b', 'c', 'd')
    argdefaults = dict(zip(reversed(varnames[:argcount]), func.__defaults__))
    kwargdefaults = func.__kwdefaults__
    return argdefaults, kwargdefaults

print(*parse_defaults(f))
print('-----')
# 看起来很麻烦，可以使用 inspect 模块
from inspect import signature
sig = signature(f)
print(str(sig))
for name, param in sig.parameters.items():
    print(param.kind, ':', name, "=", param.default)
print('-----')
# signature.bind 可以在不真正运行函数的情况下进行参数检查
args = sig.bind(1, b=5, c=4)
print(args)
args.apply_defaults()
print(args)
```

    1 2
    {'b': 1} {'d': 3}
    -----
    (a, b=1, *, c, d=3)
    POSITIONAL_OR_KEYWORD : a = <class 'inspect._empty'>
    POSITIONAL_OR_KEYWORD : b = 1
    KEYWORD_ONLY : c = <class 'inspect._empty'>
    KEYWORD_ONLY : d = 3
    -----
    <BoundArguments (a=1, b=5, c=4)>
    <BoundArguments (a=1, b=5, c=4, d=3)>



```python
# 函数注解
def clip(text: str, max_len: 'int > 0'=80) -> str:
    pass

from inspect import signature
sig = signature(clip)
print(sig.return_annotation)
for param in sig.parameters.values():
    note = repr(param.annotation).ljust(13)
    print("{note:13} : {name} = {default}".format(
        note=repr(param.annotation), name=param.name,
        default=param.default))
```

    <class 'str'>
    <class 'str'> : text = <class 'inspect._empty'>
    'int > 0'     : max_len = 80


#### 支持函数式编程的包
`operator` 里有很多函数，对应着 Python 中的内置运算符，使用它们可以避免编写很多无趣的 `lambda` 函数，如：
* `add`: `lambda a, b: a + b`
* `or_`: `lambda a, b: a or b`
* `itemgetter`: `lambda a, b: a[b]`
* `attrgetter`: `lambda a, b: getattr(a, b)`

`functools` 包中提供了一些高阶函数用于函数式编程，如：`reduce` 和 `partial`。  
此外，`functools.wraps` 可以保留函数的一些元信息，在编写装饰器时经常会用到。



```python
# Bonus: 获取闭包中的内容
def fib_generator():
    i, j = 0, 1
    def f():
        nonlocal i, j
        i, j = j, i + j
        return i
    return f

c = fib_generator()
for _ in range(5):
    print(c(), end=' ')
print()
print(dict(zip(
    c.__code__.co_freevars,
    (x.cell_contents for x in c.__closure__))))
```

    1 1 2 3 5 
    {'i': 5, 'j': 8}

	

# 6 使用一等函数实现设计模式

> 符合模式并不表示做得对。
> ——Ralph Johnson: 经典的《设计模式：可复用面向对象软件的基础》的作者之一

本章将对现有的一些设计模式进行简化，从而减少样板代码。

## 策略模式
实现策略模式，可以依赖 `abc.ABC` 和 `abc.abstractmethod` 来构建抽象基类。  
但为了实现“策略”，并不一定要针对每种策略来编写子类，如果需求简单，编写函数就好了。  
我们可以通过 `globals()` 函数来发现所有策略函数，并遍历并应用策略函数来找出最优策略。

## 命令模式
“命令模式”的目的是解耦操作调用的对象（调用者）和提供实现的对象（接受者）。

在 Python 中，我们可以通过定义 `Command` 基类来规范命令调用协议；通过在类上定义 `__call__` 函数，还可以使对象支持直接调用。

```python
import abc

class BaseCommand(ABC):
    def execute(self, *args, **kwargs):
        raise NotImplemented
```

> 事实证明，在 Gamma 等人合著的那本书中，尽管大部分使用 C++ 代码说明（少数使用 Smalltalk），但是 23 个“经典的”设计模式都能很好地在“经典的”Java 中运用。然而，这并不意味着所有模式都能一成不变地在任何语言中运用。


# 7 函数装饰器与闭包

> 有很多人抱怨，把这个特性命名为“装饰器”不好。主要原因是，这个名称与 GoF 书使用的不一致。**装饰器**这个名称可能更适合在编译器领域使用，因为它会遍历并注解语法书。
> —“PEP 318 — Decorators for Functions and Methods”

本章的最终目标是解释清楚函数装饰器的工作原理，包括最简单的注册装饰器和较复杂的参数化装饰器。  

讨论内容：
* Python 如何计算装饰器语法
* Python 如何判断变量是不是局部的
* 闭包存在的原因和工作原理
* `nonlocal` 能解决什么问题
* 实现行为良好的装饰器
* 标准库中有用的装饰器
* 实现一个参数化的装饰器

装饰器是可调用的对象，其参数是一个函数（被装饰的函数）。装饰器可能会处理被装饰的函数，然后把它返回，或者将其替换成另一个函数或可调用对象。

装饰器两大特性：
1. 能把被装饰的函数替换成其他函数
2. 装饰器在加载模块时立即执行


```python
# 装饰器通常会把函数替换成另一个函数
def decorate(func):
    def wrapped():
        print('Running wrapped()')
    return wrapped

@decorate
def target():
    print('running target()')

target()
# 以上写法等同于
def target():
    print('running target()')

target = decorate(target)
target()
```


```python
# 装饰器在导入时（模块加载时）立即执行
registry = []
def register(func):
    print('running register {}'.format(func))
    registry.append(func)
    return func

@register
def f1():
    print('running f1()')

@register
def f2():
    print('running f2()')


print('registry →', registry)
```

上面的装饰器会原封不动地返回被装饰的函数，而不一定会对函数做修改。  
这种装饰器叫注册装饰器，通过使用它来中心化地注册函数，例如把 URL 模式映射到生成 HTTP 响应的函数上的注册处。

```python
@app.get('/')
def index():
    return "Welcome."
```

可以使用装饰器来实现策略模式，通过它来注册并获取所有的策略。


```python
# 变量作用域规则
b = 1
def f2(a):
    print(a)
    print(b)        # 因为 b 在后面有赋值操作，所以认为 b 为局部变量，所以referenced before assignment
    b = 2

f2(3)
```


```python
# 使用 global 声明 b 为全局变量
b = 1
def f3(a):
    global b
    print(a)
    print(b)
    b = 9

print(b)
f3(2)
print(b)
```


```python
# 闭包
# 涉及嵌套函数时，才会产生闭包问题
def register():
    rrrr = []                # 叫 registry 会跟上面的变量重名掉…
    def wrapped(n):
        print(locals())      # locals() 的作用域延伸到了 wrapped 之外
        rrrr.append(n)
        return rrrr
    return wrapped

# num 为**自由变量**，它未在本地作用域中绑定，但函数可以在其本身的作用域之外引用这个变量
c = register()
print(c(1))
print(c(2))
assert 'rrrr' not in locals()

# 获取函数中的自由变量
print({
    name: cell.cell_contents
    for name, cell in zip(c.__code__.co_freevars, c.__closure__)
})
```

    {'n': 1, 'rrrr': []}
    [1]
    {'n': 2, 'rrrr': [1]}
    [1, 2]
    {'rrrr': [1, 2]}



```python
# 闭包内变量赋值与 nonlocal 声明
def counter():
    n = 0
    def count():
        n += 1      # n = n + 1, 所以将 n 视为局部变量，但未声明，触发 UnboundLocalError
        return n
    return count

def counter():
    n = 0
    def count():
        nonlocal n  # 使用 nonlocal 对 n 进行声明，它可以把 n 标记为局部变量
        n += 1      # 这个 n 和上面的 n 引用的时同一个值，更新这个，上面也会更新
        return n
    return count


c = counter()
print(c(), c())
```

    1 2



```python
# 开始实现装饰器
# 装饰器的典型行为：把被装饰的函数替换成新函数，二者接受相同的参数，而且（通常）返回被装饰的函数本该返回的值，同时还会做些额外操作
import time
from functools import wraps

def clock(func):
    @wraps(func)                         # 用 func 的部分标注属性（如 __doc__, __name__）覆盖新函数的值
    def clocked(*args, **kwargs):
        t0 = time.perf_counter()
        result = func(*args, **kwargs)
        t1 = time.perf_counter()
        print(t1 - t0)
        return result
    return clocked

@clock
def snooze(seconds):
    time.sleep(seconds)

snooze(1)
```

    1.002901900000012


Python 内置的三个装饰器分别为 `property`, `classmethod` 和 `staticmethod`.  
但 Python 内置的库中，有两个装饰器很常用，分别为 `functools.lru_cache` 和 [`functools.singledispatch`](https://docs.python.org/3/library/functools.html#functools.singledispatch).


```python
# lru_cache
# 通过内置的 LRU 缓存来存储函数返回值
# 使用它可以对部分递归函数进行优化（比如递归的阶乘函数）（不过也没什么人会这么写吧）
from functools import lru_cache

@lru_cache()
def func(n):
    print(n, 'called')
    return n

print(func(1))
print(func(1))
print(func(2))
```

    1 called
    1
    1
    2 called
    2



```python
# singledispatch
# 单分派泛函数：将多个函数绑定在一起组成一个泛函数，它可以通过参数类型将调用分派至其他函数上
from functools import singledispatch
import numbers

@singledispatch
def func(obj):
    print('Object', obj)

# 只要可能，注册的专门函数应该处理抽象基类，不要处理具体实现（如 int）
@func.register(numbers.Integral)
def _(n):
    print('Integer', n)

# 可以使用函数标注来进行分派注册
@func.register
def _(s:str):
    print('String', s)
    
func(1)
func('test')
func([])
```

    Integer 1
    Object test
    Object []


### 叠放装饰器
```python
@d1
@d2
def func():
    pass

# 等同于
func = d1(d2(func))
```

### 参数化装饰器
为了方便理解，可以把参数化装饰器看成一个函数：这个函数接受任意参数，返回一个装饰器（参数为 func 的另一个函数）。


```python
# 参数化装饰器
def counter(start=1):
    def decorator(func):
        n = start
        def wrapped(*args, **kwargs):
            nonlocal n
            print(f'{func.__name__} called {n} times.')
            n += 1
            return func(*args, **kwargs)
        return wrapped
    return decorator

def test():
    return

t1 = counter(start=1)(test)
t1()
t1()

@counter(start=2)
def t2():
    return

t2()
t2()
```

    test called 1 times.
    test called 2 times.
    t2 called 2 times.
    t2 called 3 times.



```python
# （可能是）更简洁的装饰器实现方式
# 利用 class.__call__

class counter:
    def __init__(self, func):
        self.n = 1
        self.func = func

    def __call__(self, *args, **kwargs):
        print(f'{self.func.__name__} called {self.n} times.')
        self.n += 1
        return self.func(*args, **kwargs)

@counter
def t3():
    return

t3()
t3()
```

    t3 called 1 times.
    t3 called 2 times.


推荐阅读：[decorator 第三方库](http://decorator.readthedocs.io/en/latest/)


```python
from decorator import decorator

@decorator
def counter(func, *args, **kwargs):
    if not hasattr(func, 'n'):
        func.n = 1
    print(f'{func.__qualname__} called {func.n} times.')
    retval = func(*args, **kwargs)
    func.n += 1
    return retval


@counter
def f(n):
    return n

print(f(2))
print(f(3))
```

    f called 1 times.
    2
    f called 2 times.
    3


	

