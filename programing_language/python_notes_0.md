
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
	

