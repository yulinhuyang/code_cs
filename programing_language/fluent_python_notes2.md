
#  第1章  Python 数据类型
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


# 第2章 序列构成的数组
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



# 第3章 字典和集合

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


# 第4章 文本和字节序列

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


# 第5章 一等函数

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



# 第6章 使用一等函数实现设计模式

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


# 第7章 函数装饰器与闭包

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


# 第8章 对象引用、可变性和垃圾回收

~~这章的前言实在是太长了…~~

Python 使用引用式变量：变量和变量名是两个不同的东西。

在 Python 中，变量不是一个存储数据的盒子，而是一个针对盒子的标注。同时，一个盒子上可以有很多标注，也可以一个都没有。


```python
a = [1, 2, 3]
b = a
a.append(4)
print(b)

# 对象会在赋值之前创建
class Test:
    def __init__(self):
        print('ID', id(self))    # 在把 self 分配给 t 之前，self 就已经有了自己的 ID

t = Test()
print('Test', t, id(t))
```

    [1, 2, 3, 4]
    ID 88908560
    Test <__main__.Test object at 0x054CA310> 88908560



```python
# 分清“==”和“is”
# a is b 等同于 id(a) == id(b)
a = {'a': 1}
b = a
assert a is b
assert id(a) == id(b)

c = {'a': 1}
assert a == c
assert not a is c
assert id(a) != id(c)
```

在 CPython 中，id() 返回对象的内存地址，但在其他 Python 解释器中可能是别的值。关键是，ID 一定是唯一的数值标注，而且在对象的生命周期中绝不会变。  
编程中，我们很少会使用 `id()` 函数，标识最常使用 `is` 运算符检查相同性，而不是直接比较 ID.


```python
# 与单例进行比较时，应该使用 is
a = None
assert a is None
b = ...
assert b is ...

# a == b 是语法糖，等同于 a.__eq__(b)
```


```python
# 元组的相对不可变性
t1 = (1, 2, [30, 40])
t2 = (1, 2, [30, 40])
assert t1 == t2

id1 = id(t1[-1])
t1[-1].append(50)
print(t1)

id2 = id(t1[-1])
assert id1 == id2     # t1 的最后一个元素的标识没变，但是值变了
assert t1 != t2
```

    (1, 2, [30, 40, 50])



```python
# 浅复制
# Python 对列表等进行复制时，只会复制容器，而不会复制容器内的内容
a = [1, [2, 3], (4, 5, 6)]
b = list(a)
assert a == b
assert a is not b     # b 是一个新对象
assert a[1] is b[1]   # 但两个列表中的元素是同一个

a[1] += [4, 5]        # a[1] = a[1] + [4, 5], list 就地执行操作后返回对象本身的引用
b[2] += (7, 8)        # b[2] = b[2] + (7, 8), tuple 在执行操作后生成一个新对象并返回它的引用
print(a, b)
```

    [1, [2, 3, 4, 5], (4, 5, 6)] [1, [2, 3, 4, 5], (4, 5, 6, 7, 8)]



```python
# 深复制
from copy import deepcopy
a = [1, [2, 3], (4, 5, 6)]
b = deepcopy(a)
assert a == b
assert a[1] is not b[1]      # 不单复制了容器，还复制了容器中的值

b[1].extend([4, 5])
print(a, b)
assert a != b
```

    [1, [2, 3], (4, 5, 6)] [1, [2, 3, 4, 5], (4, 5, 6)]



```python
# Python 的传参方式：共享传参（call by sharing），只会传递引用
def f(a, b):
    a += b
    return a

a, b = [1, 2], [3, 4]
c = f(a, b)
assert a == c == [1, 2, 3, 4]
assert a is c
```


```python
# 共享传参导致的问题
# 可变参数作为函数默认值 & 跨作用域引用导致共享变量更改
class Container:
    def __init__(self, initial=[]):
        self.values = initial
    
    def add(self, value):
        self.values.append(value)

a = Container()
b = Container()
l = []
c = Container(l)
a.add(1)
b.add(2)
c.add(3)
print(a.values, b.values, c.values)
assert a.values is b.values            # a.values 和 b.values 共享同一个变量（init 函数中的默认值）
print(l)                               # l 和 c.values 共享同一个变量，c.values 更改后，l 也会更改
assert c.values is l
```

    [1, 2] [1, 2] [3]
    [3]


## del 和垃圾回收
> 对象绝不会自行销毁；然而，无法得到对象时，可能会被当做垃圾回收。

在 CPython 中，垃圾回收使用的主要算法时引用计数。当引用计数归零时，对象立即就被销毁。  
销毁时，CPython 会先调用对象的 `__del__` 方法，然后释放分配给对象的内存。  
CPython 2.0 增加了分代垃圾回收机制，用于检测循环引用中涉及的对象组。


```python
# 监测引用计数垃圾回收
import weakref
s1 = s2 = {1}
ender = weakref.finalize(s1, lambda: print('Bye'))
print(ender.alive)
del s1                # 删除 s1 的引用
print(ender.alive)    # 对象 {1} 的引用还在（s2）
s2 = {2}
print(ender.alive)    # 无法引用到 {1} 对象，于是它被垃圾回收
```

    True
    True
    Bye
    False


## 弱引用
使用 `weakref.ref` 可以生成一个对象的引用，但不会增加它的引用计数

`weakref.ref` 类实际上是低层接口，多数程序最好使用 `weakref` 集合和 `finalize`.

`weakref` 提供的弱引用集合有 `WeakKeyDictionary`, `WeakValueDictionary`, `WeakSet`.  
它们的用途可以从名字中得出。

弱引用的局限：并不是所有对象都可以创建弱引用，比如 `list`, `dict`, `str` 实例。  
但是，某些自定义类都可以创建弱引用（比如基于 `list` 生成的子类）。  
`int` 和 `tuple` 实例及它们的子类实例都不能成为弱引用的目标。


```python
import weakref

s1 = {1}
ref = weakref.ref(s1)
print(ref, ref())
del s1
print(ref, ref())
```

    <weakref at 0x00B10BD0; to 'set' at 0x00B35A08> {1}
    <weakref at 0x00B10BD0; dead> None



```python
# WeakValueDictionary 实例
from weakref import WeakValueDictionary

weak_dict = WeakValueDictionary()
s1, s2 = {1}, {2}
weak_dict.update({
    's1': s1,
    's2': s2
})
print(list(weak_dict.items()))
del s2
print(list(weak_dict.items()))
```

    [('s1', {1}), ('s2', {2})]
    [('s1', {1})]


## 一些杂事
如果不可变集合（如 `tuple` 和 `frozenset`）中保存的是可变元素的引用，那么可变元素的值发生变化后，不可变集合也会发生改变。  
这里指的是 `hash` 和 `==`的结果，即使集合中的对象更改，该对象在集合中存储的引用也是不会变的。

`tuple()` 的参数如果是一个元组，则得到的是同一个对象。对元组使用 `[:]` 切片操作也不会生成新对象。  
`str`, `bytes` 和 `frozenset` 实例也有这种行为。  
`fs.copy()` 返回的是它本身（喵喵喵？）。

字符串字面量可能会产生**驻留**现象：两个相等的字符串共享同一个字符串对象。  
`int` 对象中，在 [-5, 256] 之间的整型实例也被提前创建，所有指向这些数字的引用均会共享对象。

自定义对象中，如果没有实现 `__eq__` 方法，则进行 `==` 判断时会比较它们的 ID.  
这种后备机制认为用户定义的类的各个实例是不同的。


```python
a = "hahaha"
b = "hahaha"
assert a is b

a = 66
b = 66
assert a is b
a = 257
b = 257
assert a is not b

class T:
    pass

a = T()
b = a
c = T()
assert a is b
assert a is not c
```

## 延伸阅读
[Python Garbage Collector Implementations: CPython, PyPy and Gas](https://thp.io/2012/python-gc/python_gc_final_2012-01-22.pdf)




# 第9章 符合 Python 风格的对象
> 绝对不要使用两个前导下划线，这是很烦人的自私行为。  
> ——Ian Bicking  
>   pip, virtualenv 和 Paste 等项目的创建者

得益于 Python 数据模型，自定义类型的行为可以像内置类型那样自然。  
实现如此自然的行为，靠的不是继承，而是**鸭子类型**（duck typing）：我们只需按照预定行为实现对象所需的方法即可。

当然，这些方法不是必须要实现的，我们需要哪些功能，再去实现它就好。

书中的 `Vector` 类完整实现可见[官方 Repo](https://github.com/fluentpython/example-code/blob/master/09-pythonic-obj/vector2d_v3.py).

## 对象表现形式
Python 中，有两种获取对象的字符串表示形式的标准方式：
* `repr()`：以便于开发者理解的方式返回对象的字符串表示形式
* `str()`：以便于用户理解的方式返回对象的字符串表示形式

实现 `__repr__` 和 `__str__` 两个特殊方法，可以分别为 `repr` 和 `str` 提供支持。

## classmethod & staticmethod
`classmethod`: 定义操作**类**，而不是操作**实例**的方法。  
`classmethod` 最常见的用途是定义备选构造方法，比如 `datetime.fromordinal` 和 `datetime.fromtimestamp`.

`staticmethod` 也会改变方法的调用方式，但方法的第一个参数不是特殊的值（`self` 或 `cls`）。  
`staticmethod` 可以把一些静态函数定义在类中而不是模块中，但抛开 `staticmethod`，我们也可以用其它方法来实现相同的功能。


```python
class Demo:
    def omethod(*args):
        # 第一个参数是 Demo 对象
        return args
    @classmethod
    def cmethod(*args):
        # 第一个参数是 Demo 类
        return args
    @staticmethod
    def smethod(*args):
        # 第一个参数不是固定的，由调用者传入
        return args

print(Demo.cmethod(1), Demo.smethod(1))
demo = Demo()
print(demo.cmethod(1), demo.smethod(1), demo.omethod(1))
```

    (<class '__main__.Demo'>, 1) (1,)
    (<class '__main__.Demo'>, 1) (1,) (<__main__.Demo object at 0x05E719B0>, 1)


## 字符串模板化
`__format__` 实现了 `format` 方法的接口，它的参数为 `format` 格式声明符，格式声明符的表示法叫做[格式规范微语言](https://docs.python.org/3/library/string.html#formatspec)。  
`str.format` 的声明符表示法和格式规范微语言类似，称作[格式字符串句法](https://docs.python.org/3/library/string.html#formatstrings)。


```python
# format
num = 15
print(format(num, '2d'), format(num, '.2f'), format(num, 'X'))
```

    15 15.00 F


## 对象散列化
只有可散列的对象，才可以作为 `dict` 的 key，或者被加入 `set`.  
要想创建可散列的类型，只需要实现 `__hash__` 和 `__eq__` 即可。  
有一个要求：如果 `a == b`，那么 `hash(a) == hash(b)`.

文中推荐的 hash 实现方法是使用位运算异或各个分量的散列值：
```python
class Vector2d:
    # 其余部分省略
    def __hash__(self):
        return hash(self.x) ^ hash(self.y)
```

而最新的文章中，推荐把各个分量组成一个 `tuple`，然后对其进行散列：
```python
class Vector2d:
    def __hash__(self):
        return hash((self.x, self.y))
```

## Python 的私有属性和“受保护的”属性
Python 的“双下前导”变量：如果以 `__a` 的形式（双前导下划线，尾部最多有一个下划线）命名一个实例属性，Python 会在 `__dict__` 中将该属性进行“名称改写”为 `_Klass__a`，以防止外部对对象内部属性的直接访问。  
这样可以避免意外访问，但**不能防止故意犯错**。

有一种观点认为不应该使用“名称改写”特性，对于私有方法来说，可以使用前导单下划线 `_x` 来标注，而不应使用双下划线。

此外：在 `from mymod import *` 中，任何使用下划线前缀（无论单双）的变量，都不会被导入。


```python
class Vector2d:
    def __init__(self, x, y):
        self.__x = x
        self.__y = y
    
vector = Vector2d(1, 2)
print(vector.__dict__)
print(vector._Vector2d__x)
```

    {'_Vector2d__x': 1, '_Vector2d__y': 2}
    1


## 使用 __slot__ 类属性节省空间
默认情况下，Python 在各个实例的 `__dict__` 字典中存储实例属性。但 Python 字典使用散列表实现，会消耗大量内存。  
如果需要处理很多个属性**有限且相同**的实例，可以通过定义 `__slots__` 类属性，将实例用元组存储，以节省内存。

```python
class Vector2d:
    __slots__ = ('__x', '__y')
```

`__slots__` 的目的是优化内存占用，而不是防止别人在实例中添加属性。_所以一般也没有什么使用的必要。_

使用 `__slots__` 时需要注意的地方：
* 子类不会继承父类的 `__slots_`
* 实例只能拥有 `__slots__` 中所列出的属性
* 如果不把 `"__weakref__"` 加入 `__slots__`，实例就不能作为弱引用的目标。


```python
# 对象二进制化
from array import array
import math

class Vector2d:
    typecode = 'd'

    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return str(tuple(self))

    def __iter__(self):
        yield from (self.x, self.y)

    def __bytes__(self):
        return (bytes([ord(self.typecode)]) +
                bytes(array(self.typecode, self)))
    
    def __eq__(self, other):
        return tuple(self) == tuple(other)

    @classmethod
    def frombytes(cls, octets):
        typecode = chr(octets[0])
        memv = memoryview(octets[1:]).cast(typecode)
        return cls(*memv)

vector = Vector2d(1, 2)
v_bytes = bytes(vector)
vector2 = Vector2d.frombytes(v_bytes)
print(v_bytes)
print(vector, vector2)
assert vector == vector2
```

    b'd\x00\x00\x00\x00\x00\x00\xf0?\x00\x00\x00\x00\x00\x00\x00@'
    (1, 2) (1.0, 2.0)


# 第10章 序列的修改、散列和切片
> 不要检查它**是不是**鸭子、它的**叫声**像不像鸭子、它的**走路姿势**像不像鸭子，等等。具体检查什么取决于你想用语言的哪些行为。(comp.lang.python, 2000 年 7 月 26 日)  
> ——Alex Martelli

本章在 `Vector2d` 基础上进行改进以支持多维向量。不过我不想写那么多 `Vector` 代码了，所以我会尝试对里面讲到的知识进行一些抽象。

当然，如果有兴趣也可以研究一下书中实现的[多维向量代码](https://github.com/fluentpython/example-code/tree/master/10-seq-hacking)。

## Python 序列协议
鸭子类型（duck typing）：“如果一个东西长的像鸭子、叫声像鸭子、走路像鸭子，那我们可以认为它就是鸭子。”

Python 中，如果我们在类上实现了 `__len__` 和 `__getitem__` 接口，我们就可以把它用在任何期待序列的场景中。


```python
# 序列协议中的切片
class SomeSeq:
    def __init__(self, seq):
        self._seq = list(seq)
    
    def __len__(self):
        return len(self._seq)

    def __getitem__(self, index):
        if isinstance(index, slice):
            # 专门对 slice 做一个自己的实现
            start, stop = index.start, index.stop
            step = index.step
            
            if start is None:
                start = 0
            elif start < 0:
                start = len(self) + start
            else:
                start = min(start, len(self))
            if stop is None:
                stop = len(self)
            elif stop < 0:
                stop = len(self) + stop
            else:
                stop = min(stop, len(self))
            if step is None:
                step = 1
            elif step == 0:
                raise ValueError("slice step cannot be zero")

            # 以上的复杂逻辑可以直接使用 slice 的接口
            # start, stop, step = index.indices(len(self))
            index_range = range(start, stop, step)
            return [self._seq[i] for i in index_range]
        else:
            return self._seq[index]

        
seq = SomeSeq([1, 2, 3, 4, 5])
print(seq[2])
print(seq[2:4], seq[:5], seq[:5:2], seq[3:], seq[:200])
print(seq[:-1], seq[-1:-5:-1])
```

    3
    [3, 4] [1, 2, 3, 4, 5] [1, 3, 5] [4, 5] [1, 2, 3, 4, 5]
    [1, 2, 3, 4] [5, 4, 3, 2]



```python
# __getitem__ 的参数不一定是单个值或者 slice，还有可能是 tuple
class SomeSeq:
    def __init__(self, seq):
        self._seq = list(seq)
    
    def __len__(self):
        return len(self._seq)

    def __getitem__(self, item):
        return item

seq = SomeSeq([1, 2, 3, 4, 5])
print(seq[1, 2, 3])
print(seq[:5, 2:5:2, -1:5:3])
```

    (1, 2, 3)
    (slice(None, 5, None), slice(2, 5, 2), slice(-1, 5, 3))



```python
# zip 和 enumerate: 知道这两个方法可以简化一些场景
l1, l2 = [1, 2, 3], [1, 2, 3, 4, 5]
for n1, n2 in zip(l1, l2):
    print(n1, n2)

print('---')
# 不要这么写
# for index in range(len(l1)):
#     print(index, l1[index])
for index, obj in enumerate(l1):
    print(index, obj)

print('---')
# list 分组的快捷操作
# 注意：只用于序列长度是分组的倍数的场景，否则最后一组会丢失
l = [1,2,3,4,5,6,7,8,9]
print(list(zip(*[iter(l)] * 3)))
```

    1 1
    2 2
    3 3
    ---
    0 1
    1 2
    2 3
    ---
    [(1, 2, 3), (4, 5, 6), (7, 8, 9)]


# 第11章 接口：从协议到抽象基类
> 抽象类表示接口。  
> ——Bjarne Stroustrup, C++ 之父

本章讨论的话题是接口：从**鸭子类型**的代表特征动态协议，到使接口更明确、能验证实现是否符合规定的抽象基类（Abstract Base Class, ABC）。

> 接口的定义：对象公开方法的子集，让对象在系统中扮演特定的角色。  
> 协议是接口，但不是正式的（只由文档和约定定义），因此协议不能像正式接口那样施加限制。  
> 允许一个类上只实现部分接口。

## 抽象基类（abc）
抽象基类是一个非常实用的功能，可以使用抽象基类来检测某个类是否实现了某种协议，而这个类并不需要继承自这个抽象类。  
[`collections.abc`](https://docs.python.org/3/library/collections.abc.html) 和 [`numbers`](https://docs.python.org/3/library/numbers.html) 模块中提供了许多常用的抽象基类以用于这种检测。

有了这个功能，我们在自己实现函数时，就不需要非常关心外面传进来的参数的**具体类型**（`isinstance(param, list)`），只需要关注这个参数是否支持我们**需要的协议**（`isinstance(param, abc.Sequence`）以保障正常使用就可以了。

但是注意：从 Python 简洁性考虑，最好不要自己创建新的抽象基类，而应尽量考虑使用现有的抽象基类。


```python
# 抽象基类
from collections import abc


class A:
    pass

class B:
    def __len__(self):
        return 0

assert not isinstance(A(), abc.Sized)
assert isinstance(B(), abc.Sized)
assert abc.Sequence not in list.__bases__    # list 并不是 Sequence 的子类
assert isinstance([], abc.Sequence)          # 但是 list 实例支持序列协议
```


```python
# 在抽象基类上进行自己的实现
from collections import abc

class FailedSized(abc.Sized):
    pass


class NormalSized(abc.Sized):
    def __len__(self):
        return 0


n = NormalSized()
print(len(n))
f = FailedSized()       # 基类的抽象协议未实现，Python 会阻止对象实例化
```

    0



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-6-fad553cddcd6> in <module>()
         13 n = NormalSized()
         14 print(len(n))
    ---> 15 f = FailedSized()       # 协议未实现，Python 会阻止对象实例化
    

    TypeError: Can't instantiate abstract class FailedSized with abstract methods __len__


有一点需要注意：抽象基类上的方法并不都是抽象方法。  
换句话说，想继承自抽象基类，只需要实现它上面**所有的抽象方法**即可，有些方法的实现是可选的。  
比如 [`Sequence.__contains__`](https://github.com/python/cpython/blob/3.7/Lib/_collections_abc.py#L889)，Python 对此有自己的实现（使用 `__iter__` 遍历自身，查找是否有相等的元素）。但如果你在 `Sequence` 之上实现的序列是有序的，则可以使用二分查找来覆盖 `__contains__` 方法，从而提高查找效率。

我们可以使用 `__abstractmethods__` 属性来查看某个抽象基类上的抽象方法。这个抽象基类的子类必须实现这些方法，才可以被正常实例化。


```python
# 自己定义一个抽象基类
import abc

# 使用元类的定义方式是 class SomeABC(metaclass=abc.ABCMeta)
class SomeABC(abc.ABC):
    @abc.abstractmethod
    def some_method(self):
        raise NotImplementedError

        
class IllegalClass(SomeABC):
    pass

class LegalClass(SomeABC):
    def some_method(self):
        print('Legal class OK')

    
l = LegalClass()
l.some_method()
il = IllegalClass()    # Raises
```

    Legal class OK



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-22-e78c72cc5a83> in <module>()
         19 l = LegalClass()
         20 l.some_method()
    ---> 21 il = IllegalClass()    # Raises
    

    TypeError: Can't instantiate abstract class IllegalClass with abstract methods some_method


## 虚拟子类
使用 [`register`](https://docs.python.org/3/library/abc.html#abc.ABCMeta.register) 接口可以将某个类注册为某个 ABC 的“虚拟子类”。支持 `register` 直接调用注册，以及使用 `@register` 装饰器方式注册（其实这俩是一回事）。  
注册后，使用 `isinstance` 以及实例化时，解释器将不会对虚拟子类做任何方法检查。  
注意：虚拟子类不是子类，所以虚拟子类不会继承抽象基类的任何方法。


```python
# 虚拟子类
import abc
import traceback

class SomeABC(abc.ABC):
    @abc.abstractmethod
    def some_method(self):
        raise NotImplementedError
    
    def another_method(self):
        print('Another')
    
    @classmethod
    def __subclasshook__(cls, subcls):
        """
        在 register 或者进行 isinstance 判断时进行子类检测
        https://docs.python.org/3/library/abc.html#abc.ABCMeta.__subclasshook__
        """
        print('Subclass:', subcls)
        return True


class IllegalClass:
    pass

SomeABC.register(IllegalClass)                # 注册
il = IllegalClass()
assert isinstance(il, IllegalClass)
assert SomeABC not in IllegalClass.__mro__    # isinstance 会返回 True，但 IllegalClass 并不是 SomeABC 的子类
try:
    il.some_method()                          # 虚拟子类不是子类，不会从抽象基类上继承任何方法
except Exception as e:
    traceback.print_exc()

try:
    il.another_method()
except Exception as e:
    traceback.print_exc()

```

    Subclass: <class '__main__.IllegalClass'>


    Traceback (most recent call last):
      File "<ipython-input-26-d307e70e7794>", line 31, in <module>
        il.some_method()                          # 虚拟子类不是子类，不会从抽象基类上继承任何方法
    AttributeError: 'IllegalClass' object has no attribute 'some_method'
    Traceback (most recent call last):
      File "<ipython-input-26-d307e70e7794>", line 36, in <module>
        il.another_method()
    AttributeError: 'IllegalClass' object has no attribute 'another_method'


# 第12章 继承的优缺点
> （我们）推出继承的初衷是让新手顺利使用只有专家才能设计出来的框架。  
> ——Alan Klay, "The Early History of Smalltalk"

本章探讨继承和子类化，重点是说明对 Python 而言尤为重要的两个细节：
* 子类化内置类型的缺点
* 多重继承和方法解析顺序（MRO）

## 子类化类型的缺点
基本上，内置类型的方法不会调用子类覆盖的方法，所以尽可能不要去子类化内置类型。  
如果有需要使用 `list`, `dict` 等类，`collections` 模块中提供了用于用户继承的 `UserDict`、`userList` 和 `UserString`，这些类经过特殊设计，因此易于扩展。


```python
# 子类化内置类型的缺点
class DoppelDict(dict):
    def __setitem__(self, key, value):
        super().__setitem__(key, [value] * 2)

# 构造方法和 update 都不会调用子类的 __setitem__
dd = DoppelDict(one=1)
dd['two'] = 2
print(dd)
dd.update(three=3)
print(dd)


from collections import UserDict
class DoppelDict2(UserDict):
    def __setitem__(self, key, value):
        super().__setitem__(key, [value] * 2)

# UserDict 中，__setitem__ 对 update 起了作用，但构造函数依然不会调用 __setitem__
dd = DoppelDict2(one=1)
dd['two'] = 2
print(dd)
dd.update(three=3)
print(dd)
```

    {'one': 1, 'two': [2, 2]}
    {'one': 1, 'two': [2, 2], 'three': 3}
    {'one': [1, 1], 'two': [2, 2]}
    {'one': [1, 1], 'two': [2, 2], 'three': [3, 3]}


## 方法解析顺序（Method Resolution Order）
权威论述：[The Python 2.3 Method Resolution Order](https://www.python.org/download/releases/2.3/mro/)  
> Moreover, unless you make strong use of multiple inheritance and you have non-trivial hierarchies, you don't need to understand the C3 algorithm, and you can easily skip this paper. 

Emmm…

OK，提两句：
1. 如果想查看某个类的方法解析顺序，可以访问该类的 `__mro__` 属性；
2. 如果想绕过 MRO 访问某个父类的方法，可以直接调用父类上的非绑定方法。


```python
class A:
    def f(self):
        print('A')


class B(A):
    def f(self):
        print('B')


class C(A):
    def f(self):
        print('C')


class D(B, C):
    def f(self):
        print('D')

    def b_f(self):
        "D -> B"
        super().f()

    def c_f(self):
        "B -> C"
        super(B, self).f()
        # C.f(self)
    
    def a_f(self):
        "C -> A"
        super(C, self).f()


print(D.__mro__)
d = D()
d.f()
d.b_f()
d.c_f()
d.a_f()
```

    (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
    D
    B
    C
    A


## 处理多重继承
书中列出来了一些处理多重继承的建议，以免做出令人费解和脆弱的继承设计：
1. 把接口继承和实现继承区分开  
    如果继承重用的是代码实现细节，通常可以换用组合和委托模式。
2. 使用抽象基类显式表示接口  
    如果基类的作用是定义接口，那就应该定义抽象基类。
3. 通过混入（Mixin）重用代码  
    如果一个类的作用是为多个不相关的子类提供方法实现，从而实现重用，但不体现实际的“上下级”关系，那就应该明确地将这个类定义为**混入类**（Mixin class）。关于 Mixin（我还是习惯英文名），可以看 Python3-Cookbook 的[《利用Mixins扩展类功能》](https://python3-cookbook.readthedocs.io/zh_CN/latest/c08/p18_extending_classes_with_mixins.html)章节。
4. 在名称中明确指出混入  
    在类名里使用 `XXMixin` 写明这个类是一个 Mixin.
5. 抽象基类可以作为混入，反过来则不成立
6. 不要子类化多个具体类  
    在设计子类时，不要在多个**具体基类**上实现多继承。一个子类最好只继承自一个具体基类，其余基类最好应为 Mixin，用于提供增强功能。
7. 为用户提供聚合类  
    如果抽象基类或 Mixin 的组合对客户代码非常有用，那就替客户实现一个包含多继承的聚合类，这样用户可以直接继承自你的聚合类，而不需要再引入 Mixin.
8. “优先使用对象组合，而不是类继承”  
    组合和委托可以代替混入，把行为提供给不同的类，不过这些方法不能取代接口继承去**定义类型层次结构**。

两个实际例子：  
* 很老的 `tkinter` 称为了反例，那个时候的人们还没有充分认识到多重继承的缺点；
* 现代的 `Django` 很好地利用了继承和 Mixin。它提供了非常多的 `View` 类，鼓励用户去使用这些类以免除大量模板代码。


# 第13章 正确重载运算符
> 有些事情让我不安，比如运算符重载。我决定不支持运算符重载，这完全是个个人选择，因为我见过太多 C++ 程序员滥用它。
> ——James Gosling, Java 之父

本章讨论的内容是：
* Python 如何处理中缀运算符（如 `+` 和 `|`）中不同类型的操作数
* 使用鸭子类型或显式类型检查处理不同类型的操作数
* 众多比较运算符（如 `==`、`>`、`<=` 等等）的特殊行为
* 增量赋值运算符（如 `+=`）的默认处理方式和重载方式

重载运算符，如果使用得当，可以让代码更易于阅读和编写。  
Python 出于灵活性、可用性和安全性方面的平衡考虑，对运算符重载做了一些限制：
* 不能重载内置类型的运算符
* 不能新建运算符
* 计算符 `is`、`and`、`or`、`not` 不能重载

Python 算数运算符对应的魔术方法可以见[这里](https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types)。

一个小知识：二元运算符 `+` 和 `-` 对应的魔术方法是 `__add__` 和 `__sub__`，而一元运算符 `+` 和 `-` 对应的魔术方法是 `__pos__` 和 `__neg__`.

## 反向运算符
> 为了支持涉及不同类型的运算，Python 为中缀运算符特殊方法提供了特殊的分派机制。对表达式 `a + b` 来说，解释器会执行以下几步操作。
> 1. 如果 a 有 `__add__` 方法，而且返回值不是 `NotImplemented`，调用 `a.__add__`，然后返回结果。
> 2. 如果 a 没有 `__add__` 方法，或者调用 `__add__` 方法返回 `NotImplemented`，检查 b 有没有 `__radd__` 方法，如果有，而且没有返回 `NotImplemented`，调用 `b.__radd__`，然后返回结果。
> 3. 如果 b 没有 `__radd__` 方法，或者调用 `__radd__` 方法返回 `NotImplemented`，抛出 `TypeError`， 并在错误消息中指明操作数类型不支持。

这样一来，只要运算符两边的任何一个对象正确实现了运算方法，就可以正常实现二元运算操作。

小知识：  
* `NotImplemented` 是一个特殊的单例值，要 `return`；而 `NotImplementedError` 是一个异常，要 `raise`.
* Python 3.5 新引入了 `@` 运算符，用于点积乘法，对应的魔术方法是 [`__matmul__`](https://docs.python.org/3/reference/datamodel.html#object.__matmul__).
* 进行 `!=` 运算时，如果双方对象都没有实现 `__ne__`，解释器会尝试 `__eq__` 操作，并将得到的结果**取反**。

放在这里有点吓人的小知识：
* Python 在进行 `==` 运算时，如果运算符两边的 `__eq__` 都失效了，解释器会用两个对象的 id 做比较\_(:з」∠)\_。_书中用了“最后一搏”这个词…真的有点吓人。_

## 运算符分派
有的时候，运算符另一边的对象可能会出现多种类型：比如对向量做乘法时，另外一个操作数可能是向量，也可能是一个标量。此时，需要在方法实现中，根据操作数的类型进行分派。  
此时有两种选择：
1. 尝试直接运算，如果有问题，捕获 `TypeError` 异常；
2. 在运算前使用 `isinstance` 进行类型判断，在收到可接受类型时在进行运算。  
判断类型时，应进行鸭子类型的判断。应该使用 `isinstance(other, numbers.Integral)`，而不是用 `isinstance(other, int)`，这是之前的知识点。  

不过，在类上定义方法时，是不能用 `functools.singledispatch` 进行单分派的，因为第一个参数是 `self`，而不是 `o`.


```python
# 一个就地运算符的错误示范
class T:
    def __init__(self, s):
        self.s = s

    def __str__(self):
        return self.s

    def __add__(self, o):
        return self.s + o

    def __iadd__(self, o):
        self.s += o
        # 这里必须要返回一个引用，用于传给 += 左边的引用变量
        # return self


t = T('1')
t1 = t
w = t + '2'
print(w, type(w))
t += '2'             # t = t.__iadd__('2')
print(t, type(t))    # t 被我们搞成了 None
print(t1, type(t1))
```

    12 <class 'str'>
    None <class 'NoneType'>
    12 <class '__main__.T'>



# 第14章 可迭代的对象、迭代器和生成器

> 当我在自己的程序中发现用到了模式，我觉得这就表明某个地方出错了。程序的形式应该仅仅反映它所要解决的问题。代码中其他任何外加的形式都是一个信号，（至少对我来说）表明我对问题的抽象还不够深——这通常意味着自己正在手动完成事情，本应该通过写代码来让宏的扩展自动实现。
> ——Paul Graham, Lisp 黑客和风险投资人

Python 内置了迭代器模式，用于进行**惰性运算**，按需求一次获取一个数据项，避免不必要的提前计算。

迭代器在 Python 中并不是一个具体类型的对象，更多地使指一个具体**协议**。

## 迭代器协议
Python 解释器在迭代一个对象时，会自动调用 `iter(x)`。  
内置的 `iter` 函数会做以下操作：
1. 检查对象是否实现了 `__iter__` 方法（`abc.Iterable`），若实现，且返回的结果是个迭代器（`abc.Iterator`），则调用它，获取迭代器并返回；
2. 若没实现，但实现了 `__getitem__` 方法（`abc.Sequence`），若实现则尝试从 0 开始按顺序获取元素并返回；
3. 以上尝试失败，抛出 `TypeError`，表明对象不可迭代。

判断一个对象是否可迭代，最好的方法不是用 `isinstance` 来判断，而应该直接尝试调用 `iter` 函数。

注：可迭代对象和迭代器不一样。从鸭子类型的角度看，可迭代对象 `Iterable` 要实现 `__iter__`，而迭代器 `Iterator` 要实现 `__next__`. 不过，迭代器上也实现了 `__iter__`，用于[返回自身](https://github.com/python/cpython/blob/3.7/Lib/_collections_abc.py#L268)。

## 迭代器的具体实现
《设计模式：可复用面向对象软件的基础》一书讲解迭代器设计模式时，在“适用性”一 节中说：
迭代器模式可用来：
* 访问一个聚合对象的内容而无需暴露它的内部表示
* 支持对聚合对象的多种遍历
* 为遍历不同的聚合结构提供一个统一的接口（即支持多态迭代）

为了“支持多种遍历”，必须能从同一个可迭代的实例中获取多个**独立**的迭代器，而且各个迭代器要能维护自身的内部状态，因此这一模式正确的实现方式是，每次调用 `iter(my_iterable)` 都新建一个独立的迭代器。这就是为什么这个示例需要定义 `SentenceIterator` 类。

所以，不应该把 Sentence 本身作为一个迭代器，否则每次调用 `iter(sentence)` 时返回的都是自身，就无法进行多次迭代了。


```python
# 通过实现迭代器协议，让一个对象变得可迭代
import re
from collections import abc


class Sentence:
    def  __init__(self, sentence):
        self.sentence = sentence
        self.words = re.findall(r'\w+', sentence)

    def __iter__(self):
        """返回 iter(self) 的结果"""
        return SentenceIterator(self.words)
        

# 推荐的做法是对迭代器对象进行单独实现
class SentenceIterator(abc.Iterator):
    def __init__(self, words):
        self.words = words
        self._index = 0

    def __next__(self):
        """调用时返回下一个对象"""
        try:
            word = self.words[self._index]
        except IndexError:
            raise StopIteration()
        else:
            self._index += 1
        
        return word


    
sentence = Sentence('Return a list of all non-overlapping matches in the string.')
assert isinstance(sentence, abc.Iterable)      # 实现了 __iter__，就支持 Iterable 协议
assert isinstance(iter(sentence), abc.Iterator)
for word in sentence:
    print(word, end='·')
```

    Return·a·list·of·all·non·overlapping·matches·in·the·string·

上面的例子中，我们的 `SentenceIterator` 对象继承自 `abc.Iterator` 通过了迭代器测试。而且 `Iterator` 替我们实现了 `__iter__` 方法。  
但是，如果我们不继承它，我们就需要同时实现 `__next__` 抽象方法和*实际迭代中并不会用到的* `__iter__` 非抽象方法，才能通过 `Iterator` 测试。

## 生成器函数
如果懒得自己写一个迭代器，可以直接用 Python 的生成器函数来在调用 `__iter__` 时生成一个迭代器。

注：在 Python 社区中，大家并没有对“生成器”和“迭代器”两个概念做太多区分，很多人是混着用的。不过无所谓啦。


```python
# 使用生成器函数来帮我们创建迭代器
import re


class Sentence:
    def  __init__(self, sentence):
        self.sentence = sentence
        self.words = re.findall(r'\w+', sentence)

    def __iter__(self):
        for word in self.words:
            yield word
        return

sentence = Sentence('Return a list of all non-overlapping matches in the string.')
for word in sentence:
    print(word, end='·')
```


```python
# 使用 re.finditer 来惰性生成值
# 使用生成器表达式（很久没用过了）
import re


class Sentence:
    def  __init__(self, sentence):
        self.re_word = re.compile(r'\w+')
        self.sentence = sentence

    def __iter__(self):
        return (match.group()
                for match in self.re_word.finditer(self.sentence))

sentence = Sentence('Return a list of all non-overlapping matches in the string.')
for word in sentence:
    print(word, end='·')
```


```python
# 实用模块
import itertools

# takewhile & dropwhile
print(list(itertools.takewhile(lambda x: x < 3, [1, 5, 2, 4, 3])))
print(list(itertools.dropwhile(lambda x: x < 3, [1, 5, 2, 4, 3])))
# zip
print(list(zip(range(5), range(3))))
print(list(itertools.zip_longest(range(5), range(3))))

# itertools.groupby
animals = ['rat', 'bear', 'duck', 'bat', 'eagle', 'shark', 'dolphin', 'lion']
# groupby 需要假定输入的可迭代对象已经按照分组标准进行排序（至少同组的元素要连在一起）
print('----')
for length, animal in itertools.groupby(animals, len):
    print(length, list(animal))
print('----')
animals.sort(key=len)
for length, animal in itertools.groupby(animals, len):
    print(length, list(animal))
print('---')
# tee
g1, g2 = itertools.tee('abc', 2)
print(list(zip(g1, g2)))
```


```python
# 使用 yield from 语句可以在生成器函数中直接迭代一个迭代器
from itertools import chain

def my_itertools_chain(*iterators):
    for iterator in iterators:
        yield from iterator

chain1 = my_itertools_chain([1, 2], [3, 4, 5])
chain2 = chain([1, 2, 3], [4, 5])
print(list(chain1), list(chain2))
```

    [1, 2, 3, 4, 5] [1, 2, 3, 4, 5]


`iter` 函数还有一个鲜为人知的用法：传入两个参数，使用常规的函数或任何可调用的对象创建迭代器。这样使用时，第一个参数必须是可调用的对象，用于不断调用（没有参数），产出各个值；第二个值是哨符，这是个标记值，当可调用的对象返回这个值时，触发迭代器抛出 StopIteration 异常，而不产出哨符。


```python
# iter 的神奇用法
# iter(callable, sentinel)
import random

def rand():
    return random.randint(1, 6)
# 不停调用 rand(), 直到产出一个 5
print(list(iter(rand, 5)))
```



# 第15章 上下文管理和 else 块
> 最终，上下文管理器可能几乎与子程序（subroutine）本身一样重要。目前，我们只了解了上下文管理器的皮毛……Basic 语言有 with 语句，而且很多语言都有。但是，在各种语言中 with 语句的作用不同，而且做的都是简单的事，虽然可以避免不断使 用点号查找属性，但是不会做事前准备和事后清理。不要觉得名字一样，就意味着作用也一样。with 语句是非常了不起的特性。  
> ——Raymond Hettinger, 雄辩的 Python 布道者

本章讨论的特性有：
* with 语句和上下文管理器
* for、while 和 try 语句的 else 语句

## if 语句之外的 else 块
> else 子句的行为如下。
>
> for: 仅当 for 循环运行完毕时（即 for 循环没有被 break 语句中止）才运行 else 块。  
> while: 仅当 while 循环因为条件为**假值**而退出时（即 while 循环没有被 break 语句中止）才运行 else 块。  
> try: 仅当try 块中没有异常抛出时才运行else 块。官方文档（https://docs.python.org/3/ reference/compound_stmts.html）还指出：“else 子句抛出的异常不会由前面的 except 子句处理。”


## try 和 else
为了清晰和准确，`try` 块中应该之抛出预期异常的语句。因此，下面这样写更好：
```python
try：
    dangerous_call()
    # 不要把 after_call() 放在这里
    # 虽然放在这里时的代码运行逻辑是一样的
except OSError:
    log('OSError')
else:
    after_call()
```

## 上下文管理器
上下文管理器可以在 `with` 块中改变程序的上下文，并在块结束后将其还原。


```python
# 上下文管理器与 __enter__ 方法返回对象的区别

class LookingGlass:
    def __enter__(self):
        import sys
        self.original_write = sys.stdout.write
        sys.stdout.write = self.reverse_write
        return 'JABBERWOCKY'

    def reverse_write(self, text):
        self.original_write(text[::-1])
    
    def __exit__(self, exc_type, exc_value, traceback):
        import sys
        sys.stdout.write = self.original_write
        if exc_type is ZeroDivisionError:
            print('Please DO NOT divide by zero!')
            return True

with LookingGlass() as what:    # 这里的 what 是 __enter__ 的返回值
    print('Alice')
    print(what)

print(what)
print('Back to normal')
```

    ecilA
    YKCOWREBBAJ
    JABBERWOCKY
    Back to normal



```python
# contextmanager 的使用

import contextlib

@contextlib.contextmanager
def looking_glass():
    import sys
    original_write = sys.stdout.write
    sys.stdout.write = lambda text: original_write(text[::-1])
    msg = ''
    try:
        # 在当前上下文中抛出的异常，会通过 yield 语句抛出
        yield 'abcdefg'
    except ZeroDivisionError:
        msg = 'Please DO NOT divide by zero!'
    finally:
        # 为了避免上下文内抛出异常导致退出失败
        # 所以退出上下文时一定要使用 finally 语句
        sys.stdout.write = original_write
        if msg:
            print(msg)

# 写完以后感觉这个用法跟 pytest.fixture 好像啊

with looking_glass() as what:
    print('ahhhhh')
    print(what)

print(what)
```

    hhhhha
    gfedcba
    abcdefg


此外，[`contextlib.ExitStack`](https://docs.python.org/3/library/contextlib.html#contextlib.ExitStack) 在某些需要进入未知多个上下文管理器时可以方便管理所有的上下文。具体使用方法可以看文档中的示例。



# 第16章 协程
> 如果 Python 书籍有一定的指导作用，那么（协程就是）文档最匮乏、最鲜为人知的 Python 特性，因此表面上看是最无用的特性。
> ——David Beazley, Python 图书作者

在“生成器”章节中我们认识了 `yield` 语句。但 `yield` 的作用不只是在生成器运行过程中**返回**一个值，还可以从调用方拿回来一个值（`.send(datum)`），甚至一个异常（`.throw(exc)`）。  
由此依赖，`yield` 语句就成为了一种流程控制工具，使用它可以实现协作式多任务：协程可以把控制器让步给中心调度程序，从而激活其它的协程。

从根本上把 yield 视作控制流程的方式，这样就好理解协程了。

本章涵盖以下话题：
* 生成器作为协程使用时的行为和状态
* 使用装饰器自动预激协程
* 调用方如何使用生成器对象的 `.close()` 和 `.throw(...)` 方法控制协程
* 协程终止时如何返回值
* `yield from` 新句法的用途和语义
* 使用案例——使用协程管理仿真系统中的并发活动

协程的四种状态：
* `GEN_CREATED`: 等待开始执行
* `GEN_RUNNING`: 解释器正在执行
* `GEN_SUSPENDED`: 在 `yield` 表达式处暂停
* `GEN_CLOSED`: 执行结束


```python
# 最简单的协程使用演示
from inspect import getgeneratorstate

def simple_coroutine():
    # GEN_RUNNING 状态
    print("Coroutine started")
    x = yield
    print("Couroutine received:", x)

my_coro = simple_coroutine()
print(getgeneratorstate(my_coro)) # GEN_CREATED
next(my_coro)                     # “预激”(prime)协程，使它能够接收来自外部的值
print(getgeneratorstate(my_coro)) # GEN_SUSPENDED
try:
    my_coro.send(42)
except StopIteration as e:
    print('StopIteration')
print(getgeneratorstate(my_coro)) # GEN_CLOSED
```


```python
# 产出多个值的协程
def async_sum(a=0):
    s = a
    while True:
        n = yield s
        s += n

asum = async_sum()
next(asum)
for i in range(1, 11):
    print(i, asum.send(i))
asum.close()                 # 如果协程不会自己关闭，我们还可以手动终止协程
asum.send(11)
```

    1 1
    2 3
    3 6
    4 10
    5 15
    6 21
    7 28
    8 36
    9 45
    10 55



    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-13-0daab1220103> in <module>()
         11     print(i, asum.send(i))
         12 asum.close()                 # 如果协程不会自己关闭，我们还可以手动终止协程
    ---> 13 asum.send(11)
    

    StopIteration: 


协程手动终止的注意事项：  
调用 `gen.closed()` 后，生成器内的 `yield` 语句会抛出 `GeneratorExit` 异常。如果生成器没有处理这个异常，或者抛出了 `StopIteration` 异常（通常是指运行到结尾），调用方不会报错。  
如果收到 `GeneratorExit` 异常，生成器一定不能产出值，否则解释器会抛出 `RuntimeError` 异常。生成器抛出的其他异常会向上冒泡，传给调用方。

协程内异常处理的示例见[官方示例 Repo](https://github.com/fluentpython/example-code/blob/master/16-coroutine/coro_finally_demo.py)。


## yield from
在协程中，`yield from` 语句的主要功能是打开双向通道，把外层的调用方与内层的子生成器连接起来，这样二者可以直接发送和产出值，还可以直接传入异常，而不用在位于中间的协程中添加大量处理异常的样板代码。  
这种夹在中间的生成器，我们称它为“委派生成器”。

子生成器迭代结束后返回（`return`）的值，会交给 `yield from` 函数。

注意：`yield from` 语句会预激生成器，所以与用来预激生成器的装饰器不能放在一起用，否则会出问题。


```python
# 委派生成器
def async_sum(a=0):
    s = a
    while True:
        try:
            n = yield s
        except Exception as e:
            print('Caught exception', e)
            return s
        s += n

def middleware():
    x = yield from async_sum()
    print('Final result:', x)
        
asum = middleware()
next(asum)
for i in range(1, 11):
    print(asum.send(i))

_ = asum.throw(ValueError)
```

    1
    3
    6
    10
    15
    21
    28
    36
    45
    55
    Caught exception 
    Final result: 55



    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-16-27762d9b83ae> in <module>()
         19     print(asum.send(i))
         20 
    ---> 21 _ = asum.throw(ValueError)
    

    StopIteration: 


关于“任务式”协程，书中给出了一个[简单的例子](https://github.com/fluentpython/example-code/blob/master/16-coroutine/taxi_sim.py)，用于执行[离散事件仿真](https://zhuanlan.zhihu.com/p/22689081)，仔细研究一下可以对协程有个简单的认识。



# 第17章 使用期物处理并发
> 抨击线程的往往是系统程序员，他们考虑的使用场景对一般的应用程序员来说，也许一生都不会遇到……应用程序员遇到的使用场景，99% 的情况下只需知道如何派生一堆独立的线程，然后用队列收集结果。  
> Michele Simionato, 深度思考 Python 的人

本章主要讨论 `concurrent.futures` 模块，并介绍“期物”（future）的概念。

我们在进行 IO 密集型并发编程（如批量下载）时，经常会考虑使用多线程场景来替代依序下载的方案，以提高下载效率。  
在 IO 密集型任务中，如果代码写的正确，那么不管使用哪种并发策略（使用线程或 `asyncio` 包），吞吐量都要比依序执行的代码高很多。

## 期物
期物（Future）表示“**将要**执行并返回结果的任务”，这个概念与 JavaScript 的 `Promise` 对象较为相似。

Python 3.4 起，标准库中有两个 Future 类：`concurrent.futures.Future` 和 `asyncio.Future`。这两个类的作用相同：`Future` 类的实例表示可能已经完成或尚未完成的延迟计算。  
通常情况下自己不应该创建期物或改变期物的状态，而只能由并发框架实例化。  
我们将某个任务交给并发框架后，这个任务将会由框架来进行调度，我们无法改变它的状态，也不能控制计算任务何时结束。


```python
# 简单的期物用法
import time
from concurrent import futures


def fake_download(url):
    time.sleep(1)      # 这里用的是多线程，所以可以直接考虑 sleep
    return url


def download_many(url_list):
    with futures.ThreadPoolExecutor(max_workers=2) as executor:
        to_do = []
        for url in url_list:
            future = executor.submit(fake_download, url)
            to_do.append(future)
            print(f"Scheduled for {url}: {future}")       # 因为 worker 数量有限，所以会有一个 future 处于 pending 状态
        results = []
        for future in futures.as_completed(to_do):
            result = future.result()
            print(f'{future} result: {result}')
            results.append(result)
        return results

download_many(["https://www.baidu.com/", "https://www.google.com/",
               "https://twitter.com/"])
```

    Scheduled for https://www.baidu.com/: <Future at 0x10d4c04a8 state=running>
    Scheduled for https://www.google.com/: <Future at 0x10d4a4f98 state=running>
    Scheduled for https://twitter.com/: <Future at 0x10d4c0198 state=pending>
    <Future at 0x10d4c04a8 state=finished returned str> result: https://www.baidu.com/
    <Future at 0x10d4a4f98 state=finished returned str> result: https://www.google.com/
    <Future at 0x10d4c0198 state=finished returned str> result: https://twitter.com/





    ['https://www.baidu.com/', 'https://www.google.com/', 'https://twitter.com/']



`ThreadExecutor` 使用多线程处理并发。在程序被 IO 阻塞时，Python 标准库会释放 GIL，以允许其它线程运行。  
所以，GIL 的存在并不会对 IO 密集型多线程并发造成太大影响。


`concurrent` 包中提供了 `ThreadPoolExecutor` 和 `ProcessPoolExecutor` 类，分别对应多线程和多进程模型。  
关于两种模型的使用及推荐并发数，我们有一个经验：
* CPU 密集型任务，推荐使用多进程模型，以利用 CPU 的多个核心，`max_workers` 应设置为 CPU 核数；
* IO 密集型任务，多核 CPU 不会提高性能，所以推荐使用多线程模型，可以省下多进程带来的资源开销，`max_workers` 可以尽可能设置多一些。


```python
# Executor.map
# 并发运行多个可调用的对象时，可以使用 map 方法

import time
from concurrent import futures


def fake_download(url):
    time.sleep(1)      # 这里用的是多线程，所以可以直接考虑 sleep
    print(f'[{time.strftime("%H:%M:%S")}] Done with {url}\n', end='')
    return url


def download_many(url_list):
    with futures.ThreadPoolExecutor(max_workers=3) as executor:
        results = executor.map(fake_download, url_list)
        return results

results = download_many(list(range(5)))
print('Results:', list(results))
```

    [15:36:08] Done with 0
    [15:36:08] Done with 1
    [15:36:08] Done with 2
    [15:36:09] Done with 3
    [15:36:09] Done with 4
    Results: [0, 1, 2, 3, 4]


`map` 的使用可能更方便一点，但 `futures.as_completed` 则更灵活：支持不同的运算方法及参数，甚至支持来自不同 `Executor` 的 `future`.

## 总结
15 年的时候看过一篇文章叫[《一行 Python 实现并行化》](https://segmentfault.com/a/1190000000414339)，里面讲述了如何利用 `multiprocessing.Pool.map`（或者 `multiprocessing.dummy.Pool.map`）快速实现多进程 / 多线程模型的并发任务处理。

[`concurrent.furures`](https://docs.python.org/3/library/concurrent.futures.html) 模块于 Python 3.2 版本引入，它把线程、进程和队列是做服务的基础设施，无须手动进行管理即可轻松实现并发任务。同时，这个包引入了“期物”的概念，可以对并发任务更加规范地进行注册及管理。


# 第18章 使用 asyncio 包处理并发
> 并发是指一次处理多件事。  
> 并行是指一次做多件事。  
> 二者不同，但是有联系。  
> 一个关于结构，一个关于执行。  
> 并发用于制定方案，用来解决可能（但未必）并行的问题。  
> ——Rob Pike, Go 语言的创造者之一

本章介绍 `asyncio` 包，这个包使用事件循环驱动的协程实现并发。  
`asyncio` 包支持 Python 3.3 及以上版本，并在 Python 3.4 中正式成为标准库。

本章讨论以下话题：
* 对比一个简单的多线程程序和对应的 asyncio 版，说明多线程和异步任务之间的关系
* asyncio.Future 类与 concurrent.futures.Future 类之间的区别
* 第 17 章中下载国旗那些示例的异步版
* 摒弃线程或进程，如何使用异步编程管理网络应用中的高并发
* 在异步编程中，与回调相比，协程显著提升性能的方式
* 如何把阻塞的操作交给线程池处理，从而避免阻塞事件循环
* 使用 asyncio 编写服务器，重新审视 Web 应用对高并发的处理方式
* 为什么 asyncio 已经准备好对 Python 生态系统产生重大影响

由于 Python 3.5 支持了 `async` 和 `await`，所以书中的 ` @asyncio.coroutine` 和 `yield from` 均会被替换成 `async def` 和 `await`.  
同时，[官方 Repo](https://github.com/fluentpython/example-code/tree/master/18b-async-await) 也使用 `async` 和 `await` 重新实现了书中的实例。

## 协程的优点
多线程的代码，很容易在任务执行过程中被挂起；而协程的代码是手动挂起的，只要代码没有运行到 `await`，调度器就不会挂起这个协程。


```python
# 基于 asyncio 的协程并发实现
# https://github.com/fluentpython/example-code/blob/master/18b-async-await/spinner_await.py
# 在 Jupyter notebook 中运行该代码会报错，所以考虑把它复制出去单独运行。
# 见 https://github.com/jupyter/notebook/issues/3397
import asyncio
import itertools
import sys


async def spin(msg):
    write, flush = sys.stdout.write, sys.stdout.flush
    for char in itertools.cycle('|/-\\'):
        status = char + ' ' + msg
        write(status)
        flush()
        write('\x08' * len(status))
        try:
            await asyncio.sleep(.1)
        except asyncio.CancelledError:
            break
    write(' ' * len(status) + '\x08' * len(status))   # 使用空格和 '\r' 清空本行


async def slow_function():
    # pretend waiting a long time for I/O
    await asyncio.sleep(3)
    return 42


async def supervisor():
    spinner = asyncio.ensure_future(spin('thinking!'))    # 执行它，但不需要等待它返回结果
    print('spinner object:', spinner)
    result = await slow_function()                        # 挂起当前协程，等待 slow_function 完成
    spinner.cancel()                                      # 从 await 中抛出 CancelledError，终止协程
    return result


def main():
    loop = asyncio.get_event_loop()
    result = loop.run_until_complete(supervisor())
    loop.close()
    print('Answer:', result)


main()
```

## 基于协程的并发编程用法
定义协程：
1. 使用 `async def` 定义协程任务；
2. 在协程中使用 `await` 挂起当前协程，唤起另一个协程，并等待它返回结果；
3. 处理完毕后，使用 `return` 返回当前协程的结果

运行协程：不要使用 `next` 或 `.send()` 来操控协程，而是把它交给 `event_loop` 去完成。
```python
loop = asyncio.get_event_loop()
result = loop.run_until_complete(supervisor())
```

注：`@asyncio.coroutine` 并**不会预激协程**。

## 使用协程进行下载
[aiohttp](https://aiohttp.readthedocs.io/en/stable/) 库提供了基于协程的 HTTP 请求功能。  
书中提供的并行下载国旗的简单示例可以在[这里](https://github.com/fluentpython/example-code/blob/master/17-futures/countries/flags_await.py)看到。  
完整示例在下载国旗的同时还下载了国家的元信息，并考虑了出错处理及并发数量，可以在[这里](https://github.com/fluentpython/example-code/blob/master/17-futures/countries/flags3_asyncio.py)看到。

`asyncio.Semaphore` 提供了协程层面的[信号量](https://zh.wikipedia.org/zh-cn/%E4%BF%A1%E5%8F%B7%E9%87%8F)服务，我们可以使用这个信号量来[限制并发数](https://github.com/fluentpython/example-code/blob/master/17-futures/countries/flags3_asyncio.py#L61)。

```python
with (await semaphore):                   # semaphore.acquire
    image = await get_flag(base_url, cc)
                                          # semaphore.release
```

## 在协程中避免阻塞任务
文件 IO 是一个非常耗时的操作，但 asyncio 并没有提供基于协程的文件操作，所以我们可以在协程中使用 `run_in_executor` 将任务交给 `Executor` 去执行[异步操作](https://github.com/fluentpython/example-code/blob/master/17-futures/countries/flags3_asyncio.py#L74)。

注：[aiofiles](https://github.com/Tinche/aiofiles) 实现了基于协程的文件操作。

## 使用 aiohttp 编写 Web 服务器
廖雪峰写了个关于 asyncio 的[小教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014320981492785ba33cc96c524223b2ea4e444077708d000)。  
如果需要实现大一点的应用，可以考虑使用 [Sanic](https://sanic.readthedocs.io/en/latest/) 框架。基于这个框架的 Web 应用写法和 Flask  非常相似。

## 使用 asyncio 做很多很多事情
GitHub 的 [aio-libs](https://github.com/aio-libs) 组织提供了很多异步驱动，比如 MySQL, Zipkin 等。


# 第19章 动态属性和特性
> 特性至关重要的地方在于，特性的存在使得开发者可以非常安全并且确定可行地将公共数据属性作为类的公共接口的一部分开放出来。
> ——Alex Martelli, Python 贡献者和图书作者

本章内容：
* 特性（property）
* 动态属性存取（`__getattr__` 和 `__setattr__`）
* 对象的动态创建（`__new__`）

## 特性
Python 特性（property）可以使我们在不改变接口的前提下，使用**存取方法**修改数据属性。


```python
# property
class A:
    def __init__(self):
        self._val = 0
    
    @property
    def val(self):
        return self._val

    @val.setter
    def val(self, val):
        print('Set val', val)
        self._val = val
    
    @val.deleter
    def val(self):
        print('Val deleted!')

a = A()
assert a.val == 0
a.val = 1
assert a.val == 1
del a.val
assert a.val == 1      # del val 只是触发了 deleter 方法，再取值时还会执行 val 的 getter 函数
```

    Set val 1
    Val deleted!


## 动态属性
当访问对象的某个属性时，如果对象没有这个属性，解释器会尝试调用对象的 `__attr__` 方法。  
但是注意，这个属性名必须是一个合法的标识符。

注：`__getattr__` 和 `__getattribute__` 的区别在于，`__getattribute__` 在每次访问对象属性时都会触发，而 `__getattr__` 只在该对象没有这个属性的时候才会触发。


```python
class B:
    a = 1
    def __getattr__(self, attr):
        print('getattr', attr)
        return attr

    def __getattribute__(self, attr):
        print('getattribute', attr)
        return super().__getattribute__(attr)

b = B()
print(b.a, b.b)
```

    getattribute a
    getattribute b
    getattr b
    1 b


## __new__ 方法
`__new__` 方法是类上的一个特殊方法，用于生成一个新对象。  
与 `__init__` 不同，`__new__` 方法必须要返回一个对象，而 `__init__` 则不需要。  
调用 `A.__new__` 时，返回的对象不一定需要是 A 类的实例。


```python
# 对象构造过程示意
class Foo:
    # __new__ 是一个特殊方法，所以不需要 @classmethod 装饰器
    def __new__(cls, arg):
        if arg is None:
            return []
        return super().__new__(cls)   # 用 object.__new__ 生成对象后开始执行 __init__ 函数

    def __init__(self, arg):
        print('arg:', arg)
        self.arg = arg


def object_maker(the_class, some_arg):
    new_object = the_class.__new__(the_class, some_arg)
    if isinstance(new_object, the_class):
        the_class.__init__(new_object, some_arg)
    return new_object 
 
# 下述两个语句的作用基本等效
x = Foo('bar')
y = object_maker(Foo, 'bar')
assert x.arg == y.arg == 'bar'
n = Foo(None)
assert n == []
```

    arg bar
    arg bar


## 杂谈
[shelve](https://docs.python.org/3/library/shelve.html) 是 Python 自带的、类 `dict` 的 KV 数据库，支持持久化存储。

```python
import shelve

d = shelve.open(filename)  # open -- file may get suffix added by low-level
                           # library

d[key] = data              # store data at key (overwrites old data if
                           # using an existing key)
data = d[key]              # retrieve a COPY of data at key (raise KeyError
                           # if no such key)
del d[key]                 # delete data stored at key (raises KeyError
                           # if no such key)

flag = key in d            # true if the key exists
klist = list(d.keys())     # a list of all existing keys (slow!)

# as d was opened WITHOUT writeback=True, beware:
d['xx'] = [0, 1, 2]        # this works as expected, but...
d['xx'].append(3)          # *this doesn't!* -- d['xx'] is STILL [0, 1, 2]!

# having opened d without writeback=True, you need to code carefully:
temp = d['xx']             # extracts the copy
temp.append(5)             # mutates the copy
d['xx'] = temp             # stores the copy right back, to persist it

# or, d=shelve.open(filename,writeback=True) would let you just code
# d['xx'].append(5) and have it work as expected, BUT it would also
# consume more memory and make the d.close() operation slower.

d.close()                  # close it
```
架子（shelve）上放一堆泡菜（pickle）坛子…没毛病。


# 第20章 属性描述符
> 学会描述符之后，不仅有更多的工具集可用，还会对 Python 的运作方式有更深入的理解，并由衷赞叹 Python 设计的优雅。  
> ——Raymond Hettinger, Python 核心开发者和专家

本章的话题是描述符。  
描述符是实现了特定协议的类，这个协议包括 `__get__`、`__set__`、和 `__delete__` 方法。

有了它，我们就可以在类上定义一个托管属性，并把所有对实例中托管属性的读写操作交给描述符类去处理。


```python
# 描述符示例：将一个属性托管给一个描述符类
class CharField:                       # 描述符类
    def __init__(self, field_name):
        self.field_name = field_name

    def __get__(self, instance, storage_cls):
        print('__get__', instance, storage_cls)
        if instance is None:            # 直接在类上访问托管属性时，instance 为 None
            return self
        return instance[self.field_name]

    def __set__(self, instance, value):
        print('__set__', instance, value)
        if not isinstance(value, str):
            raise TypeError('Value should be string')
        instance[self.field_name] = value


class SomeModel:                         # 托管类
    name = CharField('name')             # 描述符实例，也是托管类中的托管属性

    def __init__(self, **kwargs):
        self._dict = kwargs              # 出巡属性，用于存储属性

    def __getitem__(self, item):
        return self._dict[item]

    def __setitem__(self, item, value):
        self._dict[item] = value



print(SomeModel.name)
d = SomeModel(name='some name')
print(d.name)
d.name = 'another name'
print(d.name)
try:
    d.name = 1
except Exception as e:
    print(repr(e))
```

    __get__ None <class '__main__.SomeModel'>
    <__main__.CharField object at 0x063AF1F0>
    __get__ <__main__.SomeModel object at 0x063AF4B0> <class '__main__.SomeModel'>
    some name
    __set__ <__main__.SomeModel object at 0x063AF4B0> another name
    __get__ <__main__.SomeModel object at 0x063AF4B0> <class '__main__.SomeModel'>
    another name
    __set__ <__main__.SomeModel object at 0x063AF4B0> 1
    TypeError('Value should be string')


## 描述符的种类
根据描述符上实现的方法类型，我们可以把描述符分为**覆盖型描述符**和**非覆盖型描述符**。

实现 `__set__` 方法的描述符属于覆盖型描述符，因为虽然描述符是类属性，但是实现 `__set__` 方法的话，会覆盖对实例属性的赋值操作。  
而没有实现 `__set__` 方法的描述符是非覆盖型描述符。对实例的托管属性赋值，则会覆盖掉原有的描述符属性，此后再访问该属性时，将不会触发描述符的 `__get__` 操作。如果想恢复原有的描述符行为，则需要用 `del` 把覆盖掉的属性删除。

具体可以看[官方 Repo 的例子](https://github.com/fluentpython/example-code/blob/master/20-descriptor/descriptorkinds.py)。

## 描述符的用法建议
* 如果只想实现一个只读描述符，可以考虑使用 `property` 而不是自己去实现描述符；
* 只读描述符必须有 `__set__` 方法，在方法内抛出 `AttributeError`，防止属性在写时被覆盖；
* 用于验证的描述符可以只有 `__set__` 方法：通过验证后，可以修改 `self.__dict__[key]` 来将属性写入对象；
* 仅有 `__get__` 方法的描述符可以实现高效缓存；
* 非特殊的方法可以被实例属性覆盖。



# 第21章 类元编程
> （元类）是深奥的知识，99% 的用户都无需关注。如果你想知道是否需要使用元类，我告诉你，不需要（真正需要使用元类的人确信他们需要，无需解释原因）。
> ——Tim Peters, Timsort 算法的发明者，活跃的 Python 贡献者

元类功能强大，但是难以掌握。使用元类可以创建具有某种特质的全新类中，例如我们见过的抽象基类。

本章还会谈及导入时和运行时的区别。

注：除非开发框架，否则不要编写元类。

## 类工厂函数
像 `collections.namedtuple` 一样，类工厂函数的返回值是一个类，这个类的特性（如属性名等）可能由函数参数提供。  
可以参见[官方示例](https://github.com/fluentpython/example-code/blob/master/21-class-metaprog/factories.py)。  
原理：使用 `type` 构造方法，可以构造一个新的类。


```python
help(type)
```

## 元类
元类是一个类，但因为它继承自 `type`，所以可以通过它生成一个类。

在使用元类时，将会调用元类的 `__new__` 和 `__init__`，为类添加更多特性。  
这一步会在**导入**时完成，而不是在类进行实例化时。

元类的作用举例：
* 验证属性
* 一次性把某个/种装饰器依附到多个方法上（记得以前写过一个装饰器来实现这个功能，因为那个类的 `metaclass` 被占了）
* 序列化对象或转换数据
* 对象关系映射
* 基于对象的持久存储
* 动态转换使用其他语言编写的结构

### Python 中各种类的关系
> `object` 类和 `type` 类的关系很独特：`object` 是 `type` 的实例，而 `type` 是 `object` 的子类。
所有类都直接或间接地是 `type` 的实例，不过只有元类同时也是 `type` 的子类，因为**元类从 `type` 类继承了构造类的能力**。

这里面的关系比较复杂，简单理一下
* 实例关系
    * `type` 可以产出类，所以 `type` 的实例是类（`isinstance(int, type)`）；
    * 元类继承自 `type` 类，所以元类也具有生成类实例的**能力**（`isinstance(Sequence, ABCMeta)`)
* 继承关系
    * Python 中一切皆对象，所以所有类都是 `object` 的子类（`object in int.__mro__`）
    * 元类要**继承**自 `type` 对象，所以元类是 `type` 的子类（`type in ABCMeta.__mro__`）


```python
# 构建一个元类
class SomeMeta(type):
    def __init__(cls, name, bases, dic):
        # 这里 __init__ 的是 SomeClass，因为它是个类，所以我们直接用 cls 而不是 self 来命名它
        print('Metaclass __init__')
        # 为我们的类添加一个**类**属性
        cls.a = 1

class SomeClass(metaclass=SomeMeta):
    # 在解释器编译这个类的最后，SomeMeta 的 __init__ 将被调用
    print('Enter SomeClass')
    def __init__(self, val):
        # 这个函数在 SomeClass 实例化时才会被调用
        self.val = val

        
assert SomeClass.a == 1    # 元类为类添加的类属性
sc = SomeClass(2)
assert sc.val == 2
assert sc.a == 1
print(sc.__dict__, SomeClass.__dict__)
```

    Enter SomeClass
    Metaclass __init__
    {'val': 2} {'__module__': '__main__', '__init__': <function SomeClass.__init__ at 0x1113e4510>, '__dict__': <attribute '__dict__' of 'SomeClass' objects>, '__weakref__': <attribute '__weakref__' of 'SomeClass' objects>, '__doc__': None, 'a': 1}


关于类构造过程，可以参见官方 Repo 中的[代码执行练习(evaltime)部分](https://github.com/fluentpython/example-code/tree/master/21-class-metaprog)。


```python
# 用元类构造描述符
from collections import OrderedDict


class Field:
    def __get__(self, instance, cls):
        if instance is None:
            return self
        name = self.__name__
        return instance._value_dict.get(name)

    def __set__(self, instance, val):
        name = self.__name__                    # 通过 _entity_name 属性拿到该字段的名称
        instance._value_dict[name] = val

        
class DesNameMeta(type):
    @classmethod
    def __prepare__(cls, name, bases):
        """
        Python 3 特有的方法，用于返回一个映射对象
        然后传给 __init__ 的 dic 参数
        """
        return OrderedDict()

    def __init__(cls, name, bases, dic):
        field_names = []
        for name, val in dic.items():
            if isinstance(val, Field):
                val.__name__ = name            # 在生成类的时候，将属性名加到了描述符中
                field_names.append(name)
        cls._field_names = field_names
        


class NewDesNameMeta(type):                    # 使用 __new__ 方法构造新类
    def __new__(cls, name, bases, dic):
        for name, val in dic.items():
            if isinstance(val, Field):
                val.__name__ = name
        return super().__new__(cls, name, bases, dic)


class SomeClass(metaclass=DesNameMeta):
    name = Field()
    title = Field()
    
    def __init__(self):
        self._value_dict = {}
    
    def __iter__(self):
        """
        按定义顺序输出属性值
        """
        for field in self._field_names:
            yield getattr(self, field)


assert SomeClass.name.__name__ == 'name'
sc = SomeClass()
sc.name = 'Name'
sc.title = 'Title'
assert sc.name == 'Name'
print(sc._value_dict)
print(list(sc))
```

    {'name': 'Name', 'title': 'Title'}
    ['Name', 'Title']


上面的例子只是演示作用，实际上在设计框架的时候，`SomeClass` 会设计为一个基类（`models.Model`），框架用户只要继承自 `Model` 即可正常使用 `Field` 中的属性名，而无须知道 `DesNameMeta` 的存在。

## 类的一些特殊属性
类上有一些特殊属性，这些属性不会在 `dir()` 中被列出，访问它们可以获得类的一些元信息。  
同时，元类可以更改这些属性，以定制类的某些行为。

* `cls.__bases__`  
    由类的基类组成的元组
* `cls.__qualname__`  
    类或函数的限定名称，即从模块的全局作用域到类的点分路径
* `cls.__subclasses__()`  
    这个方法返回一个列表，包含类的**直接**子类
* `cls.__mro__`  
    类的方法解析顺序，这个属性是只读的，元类无法进行修改
* `cls.mro()`  
    构建类时，如果需要获取储存在类属性 __mro__ 中的超类元组，解释器会调用这个方法。元类可以覆盖这个方法，定制要构建的类解析方法的顺序。

## 延伸阅读
[`types.new_class`](https://docs.python.org/3/library/types.html#types.new_class) 和 `types.prepare_class` 可以辅助我们进行类元编程。

最后附上一个名言警句：

> 此外，不要在生产代码中定义抽象基类(或元类)……如果你很想这样做，我打赌可能是因为你想“找茬”，刚拿到新工具的人都有大干一场的冲动。如果你能避开这些深奥的概念，你(以及未来的代码维护者)的生活将更愉快，因为代码简洁明了。  
> ——Alex Martelli
