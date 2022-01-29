
流畅的python笔记：

主要参考：

https://hellowac.github.io/blog/programing/

https://github.com/StdioA/fluent-python-notes/tree/master/markdown

https://segmentfault.com/a/1190000011568813


## 第一章: python数据模型

这部分主要介绍了python的魔术方法, 它们经常是两个下划线包围来命名的(比如 `__init__` , `__lt__`, `__len__` ). 这些特殊方法是为了被python解释器调用的, 这些方法会注册到他们的类型中方法集合中, 相当于为cpython提供抄近路. 这些方法的速度也比普通方法要快, 当然在自己不清楚这些魔术方法的用途时, 不要随意添加.

关于字符串的表现形式是两种, `__str__` 与 `__repr__` . python的内置函数 `repr` 就是通过 `__repr__` 这个特殊方法来得到一个对象的字符串表示形式. 这个在交互模式下比较常用, 如果没有实现 `__repr__` , 当控制台打印一个对象时往往是 `<A object at 0x000>` . 而 `__str__` 则是 `str()` 函数时使用的, 或是在 `print` 函数打印一个对象的时候才被调用, 终端用户友好.

两者还有一个区别, 在字符串格式化时, `"%s"` 对应了 `__str__` . 而 `"%r"` 对应了 `__repr__`. `__str__` 和 `__repr__` 在使用上比较推荐的是，前者是给终端用户看，而后者则更方便我们调试和记录日志.

更多的特殊方法: https://docs.python.org/3/reference/datamodel.html

## 第二章: 序列构成的数组

这部分主要是介绍序列, 着重介绍数组和元组的一些高级用法.

序列按照容纳数据的类型可以分为:

- `容器序列`: list、tuple 和 collections.deque 这些序列能存放不同类型的数据
- `扁平序列`: str、bytes、bytearray、memoryview 和 array.array，这类序列只能容纳一种类型.

如果按照是否能被修改可以分为:

- `可变序列`: list、bytearray、array.array、collections.deque 和 memoryview
- `不可变序列`: tuple、str 和 bytes

### 列表推导

列表推导是构建列表的快捷方式, 可读性更好且效率更高.

例如, 把一个字符串变成unicode的码位列表的例子, 一般:

```python
symbols = '$¢£¥€¤'
codes = []
for symbol in symbols:
    codes.append(ord(symbol))
```

使用列表推导:

```python
symbols = '$¢£¥€¤'
codes = [ord(symbol) for symbol in symbols]
```

能用列表推导来创建一个列表, 尽量使用推导, 并且保持它简短.

### 笛卡尔积与生成器表达式

生成器表达式是能逐个产出元素, 节省内存. 例如:

```python
>>> colors = ['black', 'white']
>>> sizes = ['S', 'M', 'L']
>>> for tshirt in ('%s %s' % (c, s) for c in colors for s in sizes):
... print(tshirt)
```

实例中列表元素比较少, 如果换成两个各有1000个元素的列表, 显然这样组合的笛卡尔积是一个含有100万元素的列表, 内存将会占用很大, 而是用生成器表达式就可以帮忙省掉for循环的开销.

### 具名元组

元组经常被作为 `不可变列表` 的代表. 经常只要数字索引获取元素, 但其实它还可以给元素命名:

```python
>>> from collections import namedtuple
>>> City = namedtuple('City', 'name country population coordinates')
>>> tokyo = City('Tokyo', 'JP', 36.933, (35.689722, 139.691667))
>>> tokyo
City(name='Tokyo', country='JP', population=36.933, coordinates=(35.689722,
139.691667))
>>> tokyo.population
36.933
>>> tokyo.coordinates
(35.689722, 139.691667)
>>> tokyo[1]
'JP'
```

### 切片

列表中是以0作为第一个元素的下标, 切片可以根据下标提取某一个片段.

用 `s[a:b:c]` 的形式对 `s` 在 `a` 和 `b` 之间以 `c` 为间隔取值。`c` 的值还可以为负, 负值意味着反向取值.

```python
>>> s = 'bicycle'
>>> s[::3]
'bye'
>>> s[::-1]
'elcycib'
>>> s[::-2]
'eccb'
```

## 第三章: 字典和集合

`dict` 类型不但在各种程序里广泛使用, 它也是 `Python` 语言的基石. 正是因为 `dict` 类型的重要, `Python` 对其的实现做了高度的优化, 其中最重要的原因就是背后的「散列表」 set（集合）和dict一样, 其实现基础也是依赖于散列表.

散列表也叫哈希表, 对于dict类型, 它的key必须是可哈希的数据类型. 什么是可哈希的数据类型呢, 它的官方解释是:

> 如果一个对象是可散列的，那么在这个对象的生命周期中，它的散列值是不变
> 的，而且这个对象需要实现 `__hash__()` 方法。另外可散列对象还要有
> `__qe__()` 方法，这样才能跟其他键做比较。如果两个可散列对象是相等的，那么它们的散列值一定是一样的……

`str`, `bytes`, `frozenset` 和 `数值` 都是可散列类型.

### 字典推导式

```python
DIAL_CODE = [
    (86, 'China'),
    (91, 'India'),
    (7, 'Russia'),
    (81, 'Japan'),
]

### 利用字典推导快速生成字典
country_code = {country: code for code, country in DIAL_CODE}
print(country_code)

'''
OUT:
{'China': 86, 'India': 91, 'Russia': 7, 'Japan': 81}
'''
```

### defaultdict：处理找不到的键的一个选择

当某个键不在映射里, 我们也希望也能得到一个默认值. 这就是 `defaultdict` , 它是 `dict` 的子类, 并实现了 `__missing__` 方法.

```python
import collections
index = collections.defaultdict(list)
for item in nums:
    key = item % 2
    index[key].append(item)
```

### 字典的变种

标准库里 `collections` 模块中，除了 `defaultdict` 之外的不同映射类型:

- **OrderDict**： 这个类型在添加键的时候，会保存顺序，因此键的迭代顺序总是一致的
- **ChainMap**： 该类型可以容纳数个不同的映射对像，在进行键的查找时，这些对象会被当做一个整体逐个查找，直到键被找到为止 `pylookup = ChainMap(locals(), globals())`
- **Counter**： 这个映射类型会给键准备一个整数技术器，每次更行一个键的时候都会增加这个计数器，所以这个类型可以用来给散列表对象计数，或者当成多重集来用.

```python
import collections
ct = collections.Counter('abracadabra')
print(ct)   # Counter({'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1})
ct.update('aaaaazzz')
print(ct)   # Counter({'a': 10, 'z': 3, 'b': 2, 'r': 2, 'c': 1, 'd': 1})
print(ct.most_common(2)) # [('a', 10), ('z', 3)]
```

- **UserDict**: 这个类其实就是把标准 dict 用纯 Python 又实现了一遍

```python
import collections
class StrKeyDict(collections.UserDict):
    def __missing__(self, key):
        if isinstance(key, str):
            raise KeyError(key)
        return self[str(key)]
        
    def __contains__(self, key):
        return str(key) in self.data
        
    def __setitem__(self, key, item):
        self.data[str(key)] = item
```

### 不可变映射类型

说到不可变, 第一想到的肯定是元组, 但是对于字典来说, 要将key和value的对应关系变成不可变, `types` 模块的 `MappingProxyType` 可以做到:

```python
from types import MappingProxyType
d = {1:'A'}
d_proxy = MappingProxyType(d)
d_proxy[1]='B' # TypeError: 'mappingproxy' object does not support item assignment

d[2] = 'B'
print(d_proxy) # mappingproxy({1: 'A', 2: 'B'})
```

`d_proxy` 是动态的, 也就是说对 `d` 所做的任何改动都会反馈到它上面.

### 集合论

集合的本质是许多唯一对象的聚集. 因此, 集合可以用于去重. 集合中的元素必须是可散列的, 但是 `set` 本身是不可散列的, 而 `frozenset` 本身可以散列.

集合具有唯一性, 与此同时, 集合还实现了很多基础的中缀运算符. 给定两个集合 a 和 b, `a | b` 返
回的是它们的合集, `a & b` 得到的是交集, 而 `a - b` 得到的是差集.

合理的利用这些特性, 不仅能减少代码的数量, 更能增加运行效率.

```python
# 集合的创建
s = set([1, 2, 2, 3])

# 空集合
s = set()

# 集合字面量
s = {1, 2}

# 集合推导
s = {chr(i) for i in range(23, 45)}
```

## 第四章: 文本和字节序列

本章讨论了文本字符串和字节序列, 以及一些编码上的转换. 本章讨论的 `str` 指的是python3下的.

### 字符问题

字符串是个比较简单的概念: 一个字符串是一个字符序列. 但是关于 `"字符"` 的定义却五花八门, 其中, `"字符"` 的最佳定义是 `Unicode 字符` . 因此, python3中的 `str` 对象中获得的元素就是 unicode 字符.

把码位转换成字节序列的过程就是 `编码`, 把字节序列转换成码位的过程就是 `解码` :

```python
>>> s = 'café'
>>> len(s)
4 
>>> b = s.encode('utf8') 
>>> b
b'caf\xc3\xa9'
>>> len(b)
5 
>>> b.decode('utf8') #'café
```

码位可以认为是人类可读的文本, 而字符序列则可以认为是对机器更友好. 所以要区分 `.decode()` 和 `.encode()` 也很简单. 从字节序列到人类能理解的文本就是解码(decode). 而把人类能理解的变成人类不好理解的字节序列就是编码(encode).

### 字节概要

python3有两种字节序列, 不可变的 `bytes` 类型和可变的 `bytearray` 类型. 字节序列中的各个元素都是介于 `[0, 255]` 之间的整数.

### 处理编码问题

python自带了超过100中编解码器. 每个编解码器都有一个名称, 甚至有的会有一些别名, 如 `utf_8` 就有 `utf8`, `utf-8`, `U8` 这些别名.

如果字符序列和预期不符, 在进行解码或编码时容易抛出 `Unicode*Error` 的异常. 造成这种错误是因为目标编码中没有定义某个字符(没有定义某个码位对应的字符), 这里说说解决这类问题的方式.

- 使用python3, python3可以避免95%的字符问题.
- 主流编码尝试下: latin1, cp1252, cp437, gb2312, utf-8, utf-16le
- 留意BOM头部 `b'\xff\xfe'` , UTF-16编码的序列开头也会有这几个额外字节.
- 找出序列的编码, 建议使用 `codecs` 模块

### 规范化unicode字符串

```python
s1 = 'café'
s2 = 'caf\u00e9'
```

这两行代码完全等价. 而有一种是要避免的是, 在Unicode标准中 `é` 和 `e\u0301` 这样的序列叫 `"标准等价物"`. 这种情况用NFC使用最少的码位构成等价的字符串:

```python
>>> s1 = 'café'
>>> s2 = 'cafe\u0301'
>>> s1, s2
('café', 'café')
>>> len(s1), len(s2)
(4, 5)
>>> s1 == s2
False
```

改进后:

```python
>>> from unicodedata import normalize
>>> s1 = 'café' # 把"e"和重音符组合在一起
>>> s2 = 'cafe\u0301' # 分解成"e"和重音符
>>> len(s1), len(s2)
(4, 5)
>>> len(normalize('NFC', s1)), len(normalize('NFC', s2))
(4, 4)
>>> len(normalize('NFD', s1)), len(normalize('NFD', s2))
(5, 5)
>>> normalize('NFC', s1) == normalize('NFC', s2)
True
>>> normalize('NFD', s1) == normalize('NFD', s2)
True
```

### unicode文本排序

对于字符串来说, 比较的码位. 所以在非 ascii 字符时, 得到的结果可能会不尽人意.

## 第五章: 一等函数

在python中, 函数是一等对象. 编程语言把 `"一等对象"` 定义为满足下列条件:

- 在运行时创建
- 能赋值给变量或数据结构中的元素
- 能作为参数传给函数
- 能作为函数的返回结果

在python中, 整数, 字符串, 列表, 字典都是一等对象.

### 把函数视作对象

Python即可以函数式编程，也可以面向对象编程. 这里我们创建了一个函数, 然后读取它的 `__doc__` 属性, 并且确定函数对象其实是 `function` 类的实例:

```python
def factorial(n):
    '''
    return n
    '''
    return 1 if n < 2 else n * factorial(n-1)

print(factorial.__doc__)
print(type(factorial))
print(factorial(3))

'''
OUT

    return n

<class 'function'>
6
'''
```

### 高阶函数

高阶函数就是接受函数作为参数, 或者把函数作为返回结果的函数. 如 `map`, `filter` , `reduce` 等.

比如调用 `sorted` 时, 将 `len` 作为参数传递:

```
fruits = ['strawberry', 'fig', 'apple', 'cherry', 'raspberry', 'banana']
sorted(fruits, key=len)
# ['fig', 'apple', 'cherry', 'banana', 'raspberry', 'strawberry']
```

### 匿名函数

`lambda` 关键字是用来创建匿名函数. 匿名函数一些限制, 匿名函数的定义体只能使用纯表达式. 换句话说, `lambda` 函数内不能赋值, 也不能使用while和try等语句.

```python
fruits = ['strawberry', 'fig', 'apple', 'cherry', 'raspberry', 'banana']
sorted(fruits, key=lambda word: word[::-1])
# ['banana', 'apple', 'fig', 'raspberry', 'strawberry', 'cherry']
```

### 可调用对象

除了用户定义的函数, 调用运算符即 `()` 还可以应用到其他对象上. 如果像判断对象能否被调用, 可以使用内置的 `callable()` 函数进行判断. python的数据模型中有7种可是可以被调用的:

- 用户定义的函数: 使用def语句或lambda表达式创建
- 内置函数:如len
- 内置方法:如dict.get
- 方法:在类定义体中的函数
- 类
- 类的实例: 如果类定义了 `__call__` , 那么它的实例可以作为函数调用.
- 生成器函数: 使用 `yield` 关键字的函数或方法.

### 从定位参数到仅限关键字参数

就是可变参数和关键字参数:

```python
def fun(name, age, *args, **kwargs):
    pass
```

其中 `*args` 和 `**kwargs` 都是可迭代对象, 展开后映射到单个参数. args是个元组, kwargs是字典.

## 第六章: 使用一等函数实现设计模式

虽然设计模式与语言无关, 但这并不意味着每一个模式都能在每一个语言中使用. Gamma 等人合著的 `《设计模式：可复用面向对象软件的基础》` 一书中有 `23` 个模式, 其中有 `16` 个在动态语言中"不见了, 或者简化了".

这里不举例设计模式, 因为书里的模式不常用.

## 第七章: 函数装饰器和闭包

> 函数装饰器用于在源码中“标记”函数，以某种方式增强函数的行为。这是一项强大的功
> 能，但是若想掌握，必须理解闭包。

修饰器和闭包经常在一起讨论, 因为修饰器就是闭包的一种形式. 闭包还是回调式异步编程和函数式编程风格的基础.

### 装饰器基础知识

装饰器是可调用的对象, 其参数是另一个函数(被装饰的函数). 装饰器可能会处理被
装饰的函数, 然后把它返回, 或者将其替换成另一个函数或可调用对象.

```python
@decorate
def target():
    print('running target()')
```

这种写法与下面写法完全等价:

```python
def target():
    print('running target()')
target = decorate(target)
```

装饰器是语法糖, 它其实是将函数作为参数让其他函数处理. 装饰器有两大特征:

- 把被装饰的函数替换成其他函数
- 装饰器在加载模块时立即执行

要理解立即执行看下等价的代码就知道了, `target = decorate(target)` 这句调用了函数. 一般情况下装饰函数都会将某个函数作为返回值.

### 变量作用域规则

要理解装饰器中变量的作用域, 应该要理解闭包, 我觉得书里将闭包和作用域的顺序换一下比较好. 在python中, 一个变量的查找顺序是 `LEGB` (L：Local 局部环境，E：Enclosing 闭包，G：Global 全局，B：Built-in 内建).

```python
base = 20
def get_compare():
    base = 10
    def real_compare(value):
        return value > base
    return real_compare
    
compare_10 = get_compare()
print(compare_10(5))
```

在闭包的函数 `real_compare` 中, 使用的变量 `base` 其实是 `base = 10` 的. 因为base这个变量在闭包中就能命中, 而不需要去 `global` 中获取.

### 闭包

闭包其实挺好理解的, 当匿名函数出现的时候, 才使得这部分难以掌握. 简单简短的解释闭包就是:

**名字空间与函数捆绑后的结果被称为一个闭包(closure).**

这个名字空间就是 `LEGB` 中的 `E` . 所以闭包不仅仅是将函数作为返回值. 而是将名字空间和函数捆绑后作为返回值的. 多少人忘了理解这个 `"捆绑"` , 不知道变量最终取的哪和哪啊. 哎.

### 标准库中的装饰器

python内置了三个用于装饰方法的函数: `property` 、 `classmethod` 和 `staticmethod` .
这些是用来丰富类的.

```python
class A(object):
    @property
    def age():
        return 12
```

## 第八章: 对象引用、可变性和垃圾回收

### 变量不是盒子

很多人把变量理解为盒子, 要存什么数据往盒子里扔就行了.

```python
a = [1,2,3]
b = a 
a.append(4)
print(b) # [1, 2, 3, 4]
```

变量 `a` 和 `b` 引用同一个列表, 而不是那个列表的副本. 因此赋值语句应该理解为将变量和值进行引用的关系而已.

### 标识、相等性和别名

要知道变量a和b是否是同一个值的引用, 可以用 `is` 来进行判断:

```python
>>> a = b = [4,5,6]
>>> c = [4,5,6]
>>> a is b
True
>>> x is c
False
```

如果两个变量都是指向同一个对象, 我们通常会说变量是另一个变量的 `别名` .

**在==和is之间选择**
运算符 `==` 是用来判断两个对象值是否相等(注意是对象值). 而 `is` 则是用于判断两个变量是否指向同一个对象, 或者说判断变量是不是两一个的别名, is 并不关心对象的值. 从使用上, `==` 使用比较多, 而 `is` 的执行速度比较快.

### 默认做浅复制

```python
l1 = [3, [55, 44], (7, 8, 9)]

l2 = list(l1) # 通过构造方法进行复制 
l2 = l1[:]  #也可以这样想写
>>> l2 == l1
True
>>> l2 is l1
False
```

尽管 l2 是 l1 的副本, 但是复制的过程是先复制(即复制了最外层容器，副本中的元素是源容器中元素的引用). 因此在操作 l2[1] 时, l1[1] 也会跟着变化. 而如果列表中的所有元素是不可变的, 那么就没有这样的问题, 而且还能节省内存. 但是, 如果有可变元素存在, 就可能造成意想不到的问题.

python标准库中提供了两个工具 `copy` 和 `deepcopy` . 分别用于浅拷贝与深拷贝:

```python
import copy
l1 = [3, [55, 44], (7, 8, 9)]

l2 = copy.copy(l1)
l2 = copy.deepcopy(l1)
```

### 函数的参数做引用时

python中的函数参数都是采用共享传参. 共享传参指函数的各个形式参数获得实参中各个引用的副本. 也就是说, 函数内部的形参
是实参的别名.

这种方案就是当传入参数是可变对象时, 在函数内对参数的修改也就是对外部可变对象进行修改. 但这种参数试图重新赋值为一个新的对象时则无效, 因为这只是相当于把参数作为另一个东西的引用, 原有的对象并不变. 也就是说, 在函数内, 参数是不能把一个对象替换成另一个对象的.

### 不要使用可变类型作为参数的默认值

参数默认值是个很棒的特性. 对于开发者来说, 应该避免使用可变对象作为参数默认值. 因为如果参数默认值是可变对象, 而且修改了它的内容, 那么后续的函数调用上都会收到影响.

### del和垃圾回收

在python中, 当一个对象失去了最后一个引用时, 会当做垃圾, 然后被回收掉. 虽然python提供了 `del` 语句用来删除变量. 但实际上只是删除了变量和对象之间的引用, 并不一定能让对象进行回收, 因为这个对象可能还存在其他引用.

在CPython中, 垃圾回收主要用的是引用计数的算法. 每个对象都会统计有多少引用指向自己. 当引用计数归零时, 意味着这个对象没有在使用, 对象就会被立即销毁.

## 符合Python风格的对象

> 得益于 Python 数据模型，自定义类型的行为可以像内置类型那样自然。实现如此自然的
> 行为，靠的不是继承，而是鸭子类型（duck typing）：我们只需按照预定行为实现对象所
> 需的方法即可。

### 对象表示形式

每门面向对象的语言至少都有一种获取对象的字符串表示形式的标准方式。Python 提供了
两种方式。

- `repr()` : 以便于开发者理解的方式返回对象的字符串表示形式。
- `str()` : 以便于用户理解的方式返回对象的字符串表示形式。

### classmethod 与 staticmethod

这两个都是python内置提供了装饰器, 一般python教程都没有提到这两个装饰器. 这两个都是在类 `class` 定义中使用的, 一般情况下, class 里面定义的函数是与其类的实例进行绑定的. 而这两个装饰器则可以改变这种调用方式.

先来看看 `classmethod` , 这个装饰器不是操作实例的方法, 并且将类本身作为第一个参数. 而 `staticmethod` 装饰器也会改变方法的调用方式, 它就是一个普通的函数,

`classmethod` 与 `staticmethod` 的区别就是 `classmethod` 会把类本身作为第一个参数传入, 其他都一样了.

看看例子:

```python
>>> class Demo:
... @classmethod
... def klassmeth(*args):
...     return args
... @staticmethod
... def statmeth(*args):
...     return args
...
>>> Demo.klassmeth()
(<class '__main__.Demo'>,)
>>> Demo.klassmeth('spam')
(<class '__main__.Demo'>, 'spam')
>>> Demo.statmeth()
()
>>> Demo.statmeth('spam')
('spam',)
```

### 格式化显示

内置的 `format()` 函数和 `str.format()` 方法把各个类型的格式化方式委托给相应的 `.__format__(format_spec)` 方法. `format_spec` 是格式说明符，它是：

- `format(my_obj, format_spec)` 的第二个参数
- `str.format()` 方法的格式字符串，{} 里代换字段中冒号后面的部分

### Python的私有属性和"受保护的"属性

python中对于实例变量没有像 `private` 这样的修饰符来创建私有属性, 在python中, 有一个简单的机制来处理私有属性.

```python
class A:
    def __init__(self):
        self.__x = 1

a = A()
print(a.__x) # AttributeError: 'A' object has no attribute '__x'

print(a.__dict__)
```

如果属性以 `__name` 的 `两个下划线为前缀, 尾部最多一个下划线` 命名的实例属性, python会把它名称前面加一个下划线加类名, 再放入 `__dict__` 中, 以 `__name` 为例, 就会变成 `_A__name` .

名称改写算是一种安全措施, 但是不能保证万无一失, 它能避免意外访问, 但不能阻止故意做坏事.

只要知道私有属性的机制, 任何人都能直接读取和改写私有属性. 因此很多python程序员严格规定: `遵守使用一个下划线标记对象的私有属性` . Python 解释器不会对使用单个下划线的属性名做特殊处理, 由程序员自行控制, 不在类外部访问这些属性. 这种方法也是所推荐的, 两个下划线的那种方式就不要再用了. 引用python大神的话:

> 绝对不要使用两个前导下划线，这是很烦人的自私行为。如果担心名称冲突，应该明
> 确使用一种名称改写方式（如 _MyThing_blahblah）。这其实与使用双下划线一
> 样，不过自己定的规则比双下划线易于理解。

**Python中的把使用一个下划线前缀标记的属性称为"受保护的"属性**

### 使用 **slots** 类属性节省空间

默认情况下, python在各个实例中, 用 `__dict__` 的字典存储实例属性. 因此实例的属性是动态变化的, 可以在运行期间任意添加属性. 而字典是消耗内存比较大的结构. 因此当对象的属性名称确定时, 使用 `__slots__` 可以节约内存.

```python
class Vector2d:
    __slots__ = ('__x', '__y')
    typecode = 'd'
    # 下面是各个方法（因排版需要而省略了）
```

在类中定义` __slots__` 属性的目的是告诉解释器："这个类中的所有实例属性都在这儿
了！" 这样, Python 会在各个实例中使用类似元组的结构存储实例变量, 从而避免使用消
耗内存的 `__dict__` 属性. 如果有数百万个实例同时活动, 这样做能节省大量内存.

## 第十章: 序列的修改、散列和切片

### 协议和鸭子类型

在python中, 序列类型不需要使用继承, 只需要符合序列协议的方法即可. 这里的协议就是实现 `__len__` 和 `__getitem__` 两个方法. 任何类, 只要实现了这两个方法, 它就满足了序列操作, 因为它的行为像序列.

协议是非正式的, 没有强制力, 因此你知道类的具体使用场景, 通常只要实现一个协议的部分. 例如, 为了支持迭代, 只需实现 `__getitem__` 方法, 没必要提供 `__len__` 方法, 这也就解释了 `鸭子类型` :

> 当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，
> 那么这只鸟就可以被称为鸭子

### 可切片的序列

切片(Slice)是用来获取序列某一段范围的元素. 切片操作也是通过 `__getitem__` 来完成的:

```python
class Vector:
    # 省略了很多行
    # ...
    def __len__(self):
        return len(self._components)
    # 省略了很多
    def __getitem__(self, index):
        cls = type(self)  # 获取实例的类型

        if isinstance(index, slice):  # 如果index参数值是切片的对象
            # 调用Vector的构造方法，建立一个新的切片后的Vector类
            return cls(self._components[index])
        elif isinstance(index, numbers.Integral):  # 如果参数是整数类型
            return self._components[index]  # 我们就对数组进行切片
        else:  # 否则我们就抛出异常
            msg = '{cls.__name__} indices must be integers'
            raise TypeError(msg.format(cls=cls))
```

### 动态存取属性

通过访问分量名来获取属性:

```python
shortcut_names = 'xyzt'
def __getattr__(self, name):
        cls = type(self) # 获取类型
        if len(name) == 1: # 判断属性名是否在我们定义的names中
            pos = cls.shortcut_names.find(name)
            if 0 <= pos < len(self._components):
                return self._components[pos]
        msg = '{} objects has no attribute {}'
        raise AttributeError(msg.format(cls, name))
        
test = Vector([3, 4, 5])
print(test.x)
print(test.y)
print(test.z)
print(test.c)
```

### 散列和快速等值测试

实现 `__hash__` 方法。加上现有的 `__eq__` 方法，这会把实例变成可散列的对象.

当序列是多维是时候, 我们有一个效率更高的方法:

```python
def __eq__(self, other):
        if len(self) != len(other): # 首先判断长度是否相等
            return False
            for a, b in zip(self, other): # 接着逐一判断每个元素是否相等 
                if a != b:
                    return False
        return True
        
# 我们也可以写的漂亮点

def __eq__(self, other):
    return (len(self) == len(other)) and all(a == b for a, b in zip(self, other))
```

## 第十一章: 接口：从协议到抽象基类

这些协议定义为非正式的接口, 是让编程语言实现多态的方式. 在python中, 没有 `interface` 关键字, 而且除了抽象基类, 每个类都有接口: 所有类都可以自行实现 `__getitem__` 和 `__add__` .

有写规定则是程序员在开发过程中慢慢总结出来的, 如受保护的属性命名采用单个前导下划线, 还有一些编码规范之类的.

协议是接口, 但不是正式的, 这些规定并不是强制性的, 一个类可能只实现部分接口, 这是允许的.

既然有非正式的协议, 那么有没有正式的协议呢? 有, 抽象基类就是一种强制性的协议.

抽象基类要求其子类需要实现定义的某个接口, 且抽象基类不能实例化.

### Python文化中的接口和协议

引入抽象基类之前, python就已经非常成功了, 即使现在也很少使用抽象基类. 通过鸭子类型和协议, 我们把协议定义为非正式接口, 是让python实现多态的方式.

另一边面, 不要觉得把公开数据属性放入对象的接口中不妥, 如果需要, 总能实现读值和设值方法, 把数据属性变成特性. 对象公开方法的自己, 让对象在系统中扮演特定的角色. 因此, 接口是实现特定角色的方法集合.

序列协议是python最基础的协议之一, 即便对象只实现那个协议最基本的一部分, 解释器也会负责地处理.

### 水禽和抽象基类

鸭子类型在很多情况下十分有用, 但是随着发展, 通常由了更好的方式.

近代, 属和种基本是根据表型系统学分类的, 鸭科属于水禽, 而水禽还包括鹅, 鸿雁等. 水禽是对某一类表现一致进行的分类, 他们有一些统一"描述"部分.

因此, 根据分类的演化, 需要有个水禽类型, 只要 `cls` 是抽象基类, 即 `cls` 的元类是 `abc.ABCMeta` , 就可以使用 `isinstance(obj, cls)` 来进行判断.

与具类相比, 抽象基类有很多理论上的优点, 被注册的类必须满足抽象基类对方法和签名的要求, 更重要的是满足底层语义契约.

### 标准库中的抽象基类

大多数的标准库的抽象基类在 `collections.abc` 模块中定义. 少部分在 `numbers` 和 `io` 包中有一些抽象基类. 标准库中有两个 `abc` 模块, 这里只讨论 `collections.abc` .

这个模块中定义了 16 个抽象基类.

**Iterable、Container 和 Sized**
各个集合应该继承这三个抽象基类，或者至少实现兼容的协议。`Iterable` 通过 `__iter__` 方法支持迭代，Container 通过 `__contains__` 方法支持 in 运算符，Sized
通过 `__len__` 方法支持 len() 函数。

**Sequence、Mapping 和 Set**
　　这三个是主要的不可变集合类型，而且各自都有可变的子类。

**MappingView**
　　在 Python3 中，映射方法 `.items()`、`.keys()` 和 `.values()` 返回的对象分别是
ItemsView、KeysView 和 ValuesView 的实例。前两个类还从 Set 类继承了丰富的接
口。

**Callable 和 Hashable**
　　这两个抽象基类与集合没有太大的关系，只不过因为 `collections.abc` 是标准库中
定义抽象基类的第一个模块，而它们又太重要了，因此才把它们放到 `collections.abc`
模块中。我从未见过 `Callable` 或 `Hashable` 的子类。这两个抽象基类的主要作用是为内
置函数 `isinstance` 提供支持，以一种安全的方式判断对象能不能调用或散列。

**Iterator**
　　注意它是 Iterable 的子类。
　　

## 第十二章: 继承的优缺点

很多人觉得多重继承得不偿失, 那些不支持多继承的编程语言好像也没什么损失.

### 子类化内置类型很麻烦

python2.2 以前, 内置类型(如list, dict)是不能子类化的. 它们是不能被其他类所继承的, 原因是内置类型是C语言实现的, 不会调用用户定义的类覆盖的方法.

至于内置类型的子类覆盖的方法会不会隐式调用, CPython 官方也没有制定规则. 基本上, 内置类型的方法不会调用子类覆盖的方法. 例如, dict 的子类覆盖的 `__getitem__` 方法不会覆盖内置类型的 `get()` 方法调用.

### 多重继承和方法解析顺序

任何实现多重继承的语言都要处理潜在的命名冲突，这种冲突由不相关的祖先类实现同名
方法引起。这种冲突称为“菱形问题”，如图.


Python 会按照特定的顺序遍历继承
图。这个顺序叫方法解析顺序（Method Resolution Order，MRO）。类都有一个名为
**mro** 的属性，它的值是一个元组，按照方法解析顺序列出各个超类，从当前类一直
向上，直到 object 类。

从C.__mro__的值可以看出, Python的方法解析优先级从高到低为:

1. 实例本身(instance)

2. 类(class)

3. super class, 继承关系越近, 越先定义, 优先级越高.
	
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

## 第十九章: 动态属性和特性

在python中, 数据的属性和处理数据的方法都可以称为 `属性` . 除了属性, pythpn 还提供了丰富的 API, 用于控制属性的访问权限, 以及实现动态属性, 如 `obj.attr` 方式和 `__getattr__` 计算属性.

动态创建属性是一种元编程,

### 使用动态属性转换数据

通常, 解析后的 json 数据需要形如 `feed['Schedule']['events'][40]['name']` 形式访问, 必要情况下我们可以将它换成以属性访问方式 `feed.Schedule.events[40].name` 获得那个值.

```python
from collections import abc
class FrozenJSON:
    """一个只读接口，使用属性表示法访问JSON类对象
    """
    def __init__(self, mapping):
        self.__data = dict(mapping)
    def __getattr__(self, name):
        if hasattr(self.__data, name):
            return getattr(self.__data, name)
        else:
            return FrozenJSON.build(self.__data[name]) # 从 self.__data 中获取 name 键对应的元素
    @classmethod
    def build(cls, obj):
        if isinstance(obj, abc.Mapping):
            return cls(obj)
        elif isinstance(obj, abc.MutableSequence):
            return [cls.build(item) for item in obj]
        else: # 如果既不是字典也不是列表，那么原封不动地返回元素
            return obj
```

### 使用 **new** 方法以灵活的方式创建对象

我们通常把 `__init__` 成为构造方法, 这是从其他语言借鉴过来的术语. 其实, 用于构造实例的特殊方法是 `__new__` : 这是个类方法, 必须返回一个实例. 返回的实例将作为以后的 `self` 传给 `__init__` 方法.

## 第二十章: 属性描述符

描述符是实现了特性协议的类, 这个协议包括 `__get__`, `__set__` 和 `__delete__` 方法. 通常, 可以实现部分协议.

### 覆盖型与非覆盖型描述符对比

python存取属性的方式是不对等的. 通过实例读取属性时, 通常返回的是实例中定义的属性, 但是, 如果实例中没有指定的属性, 那么会从获取类属性. 而实例中属性赋值时, 通常会在实例中创建属性, 根本不影响类.

这种不对等的处理方式对描述符也有影响. 根据是否定义 `__set__` 方法, 描述符可分为两大类: 覆盖型描述符和与非覆盖型描述符.

实现 `__set__` 方法的描述符属于覆盖型描述符, 因为虽然描述符是类属性, 但是实现 `__set__` 方法的话, 会覆盖对实例属性的赋值操作. 因此作为类方法的 `__set__` 需要传入一个实例 `instance` . 看个例子:

```python
def print_args(*args): # 打印功能
    print(args)

class Overriding:  # 设置了 __set__ 和 __get__
    def __get__(self, instance, owner):
        print_args('get', self, instance, owner)

    def __set__(self, instance, value):
        print_args('set', self, instance, value)

class OverridingNoGet:  # 没有 __get__ 方法的覆盖型描述符
    def __set__(self, instance, value):
        print_args('set', self, instance, value)

class NonOverriding:  # 没有 __set__ 方法，所以这是非覆盖型描述符
    def __get__(self, instance, owner):
        print_args('get', self, instance, owner)

class Managed:  # 托管类，使用各个描述符类的一个实例
    over = Overriding()
    over_no_get = OverridingNoGet()
    non_over = NonOverriding()
    
    def spam(self):
        print('-> Managed.spam({})'.format(repr(self)))
```

**覆盖型描述符**

```python
obj = Managed()
obj.over        # ('get', <__main__.Overriding object>, <__main__.Managed object>, <class '__main__.Managed'>)
obj.over = 7    # ('set', <__main__.Overriding object>, <__main__.Managed object>, 7)
obj.over        # ('get', <__main__.Overriding object>, <__main__.Managed object>, <class '__main__.Managed'>)
```

名为 `over` 的实例属性, 会覆盖读取和赋值 `obj.over` 的行为.

**没有 `__get__` 方法的覆盖型描述符**

```python
obj = Managed()
obj.over_no_get
obj.over_no_get = 7  # ('set', <__main__.OverridingNoGet object>, <__main__.Managed object>, 7)
obj.over_no_get
```

只有在赋值操作的时候才回覆盖行为.

### 方法是描述符

python的类中定义的函数属于绑定方法, 如果用户定义的函数都有 `__get__` 方法, 所以依附到类上, 就相当于描述符.

`obj.spam` 和 `Managed.spam` 获取的是不同的对象. 前者是 `<class method>` 后者是 `<class function>` .

函数都是非覆盖型描述符. 在函数上调用 `__get__` 方法时传入实例作为 `self` , 得到的是绑定到那个实例的方法. 调用函数的 `__get__` 时传入的 instance 是 `None` , 那么得到的是函数本身. 这就是形参 `self` 的隐式绑定方式.

### 描述符用法建议

**使用特性以保持简单**
内置的 `property` 类创建的是覆盖型描述符, `__set__` 和 `__get__` 都实现了.

**只读描述符必须有 \**set\** 方法**
如果要实现只读属性, `__get__` 和 `__set__` 两个方法必须都定义, 柔则, 实例的同名属性会覆盖描述符.

**用于验证的描述符可以只有 \**set\** 方法**
什么是用于验证的描述符, 比方有个年龄属性, 但它只能被设置为数字, 这时候就可以只定义 `__set__` 来验证值是否合法. 这种情况不需要设置 `__get__` , 因为实例属性直接从 `__dict__` 中获取, 而不用去触发 `__get__` 方法.

## 第二十一章: 类元编程

类元编程是指在运行时创建或定制类的技艺. 在python中, 类是一等对象, 因此任何时候都可以使用函数创建类, 而无需使用 `class` 关键字. 类装饰器也是函数, 不过能够审查, 修改, 甚至把被装饰的类替换成其他类.

元类是类元编程最高级的工具. 什么是元类呢? 比如说 `str` 是创建字符串的类, `int` 是创建整数的类. 那么元类就是创建类的类. 所有的类都由元类创建. 其他 `class` 只是原来的"实例".

本章讨论如何在运行时创建类.

### 类工厂函数

标准库中就有一个例子是类工厂函数--具名元组( `collections.namedtuple` ). 我们把一个类名和几个属性传给这个函数, 它会创建一个 `tuple` 的子类, 其中的元素通过名称获取.

假设我们创建一个 `record_factory` , 与具名元组具有相似的功能:

```python
>>> Dog = record_factory('Dog', 'name weight owner')
>>> rex = Dog('Rex', 30, 'Bob')
>>> rex
Dog(name='Rex', weight=30, owner='Bob')
>>> rex.weight = 32
>>> Dog.__mro__
(<class 'factories.Dog'>, <class 'object'>)
```

我们要做一个在运行时创建类的, 类工厂函数:

```python
def record_factory(cls_name, field_names):
    try:
        field_names = field_names.replace(',', ' ').split()  # 属性拆分
    except AttributeError:  # no .replace or .split
        pass  # assume it's already a sequence of identifiers
    field_names = tuple(field_names)  # 使用属性名构建元组，这将成为新建类的 __slots__ 属性

    def __init__(self, *args, **kwargs):  # 这个函数将成为新建类的 __init__ 方法
        attrs = dict(zip(self.__slots__, args))
        attrs.update(kwargs)
        for name, value in attrs.items():
            setattr(self, name, value)

    def __iter__(self):  # 实现 __iter__ 函数, 变成可迭代对象
        for name in self.__slots__:
            yield getattr(self, name)

    def __repr__(self):  # 生成友好的字符串表示形式
        values = ', '.join('{}={!r}'.format(*i) for i
                           in zip(self.__slots__, self))
        return '{}({})'.format(self.__class__.__name__, values)

    cls_attrs = dict(__slots__ = field_names,  # 组建类属性字典
                     __init__  = __init__,
                     __iter__  = __iter__,
                     __repr__  = __repr__)

    return type(cls_name, (object,), cls_attrs)  # 调用元类 type 构造方法，构建新类，然后将其返回
```

`type` 就是元类, 实例的最后一行会构造一个类, 类名是 `cls_name`, 唯一直接的超类是 `object` .

在python中做元编程时, 最好不要用 `exec` 和 `eval` 函数. 这两个函数会带来严重的安全风险.

### 元类基础知识

元类是制造类的工厂, 不过不是函数, 本身也是类. **元类是用于构建类的类**.

为了避免无限回溯, `type` 是其自身的实例. `object` 类和 `type` 类关系很独特, `object` 是 `type` 的实例, 而 `type` 是 `object` 的子类.

### 元类的特殊方法 **prepare**

`type` 构造方法以及元类的 `__new__` 和 `__init__` 方法都会收到要计算的类的定义体, 形式是名称到属性的映像. 在默认情况下, 这个映射是字典, 属性在类的定义体中顺序会丢失. 这个问题的解决办法是, 使用python3引入的特殊方法 `__prepare__` , 这个方法只在元类中有用, 而且必须声明为类方法(即要使用 `@classmethod` 装饰器定义). 解释器调用元类的 `__new__` 方法之前会先调用 `__prepare__` 方法, 使用类定义体中的属性创建映射.

`__prepare__` 的第一个参数是元类, 随后两个参数分别是要构建类的名称和基类组成的原则, 返回值必须是映射.

```python
class EntityMeta(type):
    """Metaclass for business entities with validated fields"""

    @classmethod
    def __prepare__(cls, name, bases):
        return collections.OrderedDict()  # 返回一个空的 OrderedDict 实例，类属性将存储在里面。

    def __init__(cls, name, bases, attr_dict):
        super().__init__(name, bases, attr_dict)
        cls._field_names = []  # 中创建一个 _field_names 属性
        for key, attr in attr_dict.items():
            if isinstance(attr, Validated):
                type_name = type(attr).__name__
                attr.storage_name = '_{}#{}'.format(type_name, key)
                cls._field_names.append(key)

class Entity(metaclass=EntityMeta):
    """Business entity with validated fields"""

    @classmethod
    def field_names(cls):  # field_names 类方法的作用简单：按照添加字段的顺序产出字段的名称
        for name in cls._field_names:
            yield name
```
