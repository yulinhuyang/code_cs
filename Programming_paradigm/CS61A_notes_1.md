# 1 Function

# Computer Science

研究的对象是：
- 能用计算解决的问题
- 如何解决这类问题
- 如何高效解决这类问题

该类问题的最大敌人是：
- 复杂度

如何管理复杂度
- 抽象
- Programming paradigms(编程范式或程式设计法)

## expressions 的形式
- 所有形式都可以由 `f(x)` 表示
- 调用表达式
  - add(2, 3)
  - mul(2, 3)
```python
add(2,3)
# add: Operator
# 2,3: Operand （操作数）
```
## expression tree

![表达式树](https://wizardforcel.gitbooks.io/sicp-py/content/img/expression_tree.png)



# 2 绑定

```python
def <name>(<formal parameters>): # 函数签名
    return <return expression>
```
def 执行的步骤
- 创建一个 function，其签名为 ```<name>(<formal parameters>)```
- 设置函数的主体（第一行后的所有内容），但并未执行函数主体
- 把 ```<name>``` 和当前 frame 的 function 绑定
  
## summary
- envrionment 是一系列的 frames
- [局部环境帧与全局环境帧](https://wizardforcel.gitbooks.io/sicp-py/content/1.3.html)

![sum_squares](https://wizardforcel.gitbooks.io/sicp-py/content/img/evaluate_sum_squares_3.png)

- 名称绑定到值上面，它延伸到许多局部帧中；
- 局部帧在唯一的全局帧之上；
- 全局帧包含共享名称；
- 表达式为树形结构；
- 以及每次子表达式（包含用户定义函数）调用时，环境必须被扩展


# 3 实践指南：函数的艺术

## 如何编写良好的函数
 - 每个函数都应只做**一个任务**
  - **不要重复劳动**是软件工程的中心法则
  - 函数应**泛型**，最好在任何情况下可以使用

##  文档字符串
描述这个函数的文档，叫做文档字符串
- 使用 `"""`，描述函数的任务和参数
- [文档字符串准则](https://www.python.org/dev/peps/pep-0257/)

```python
def pressure(v, t, n):
    """Compute the pressure in pascals of an ideal gas.

    Applies the ideal gas law: http://en.wikipedia.org/wiki/Ideal_gas_law

    v -- volume of gas, in cubic meters
    t -- absolute temperature in degrees kelvin
    n -- particles of gas
    """
    k = 1.38e-23  # Boltzmann's constant
    return n * k * t / v

help(pressure) # 查看文档字符串
```

## 参数默认值
```python
k_b=1.38e-23  # Boltzmann's constant
def pressure(v, t, n=6.022e23):
    return n * k_b * t / v
```
- 函数体内的常见数据，应表示为具名参数的默认值 （`n=6.022e23`）
- 永远不会改变的数据，应该定义在全局帧中 （基本常数`k_b=1.38e-23`）
  
## python 可以有多个返回值
```python
def divide_exact(n,d):
    return n // d, n % d
quotient, remainder = divide_exact(2013, 10)
>>> quotient
201
>>> remainder 
3
```
- 实际上返回的是一个元组 tuple

# 4 Control

```python
<header>:
    <statement>
    <statement>
    ...
<separating header>:
    <statement>
    <statement>
    ...
...
```
![statement](../pics/statement.png)

![statement Demo](../pics/statementDemo.png)

头部特化的求值规则 (`<header>`)
  - 指导了组内的语句什么时候被执行、是否会被执行
  
多行语句序列的的执行方法
- 要想执行语句序列，先要执行第一条语句 `<header>`。如果这个语句不是重定向控制，执行语句序列的剩余部分。
  
一个语句序列可以划分为它的第一个元素和其余元素，语句序列的剩余部分也是一个语句序列。我们可以递归应用这个执行规则。

## 定义函数 II：局部赋值
赋值语句的效果是在当前环境的第一个帧上，将名字绑定到值上
- 函数体内的赋值语句不会影响全局帧
- 局部赋值也可以将名称赋为间接量

### 环境赋予 name 实际意义
- 每个表达式的值由环境的内容所求得
- A name evaluates to the value(名称的计算结果) bound to that name(绑定到这个名字) in the earliest frame of the current environment(在当前环境的最早框架中) in which that name is found.

![NameInFrame](../pics/NameInFrame.png)

## 条件语句
```python 
if <expression>: # 布尔上下文
    <suite>
elif <expression>:
    <suite>
else:
    <suite>
```

布尔上下文 `<expression>`
  - 其值的真假对控制流很重要
  - 其值永远不会被赋值或返回

Python 包含了多种假值: `0、None、False`


### 简单语句的短路
`True or False` 输出为 `True`

## 迭代
为了防止 `while` 子句的语句组无限执行，在每次通过 `while` 时修改环境的状态。

按下```<Control>-C```可以强制让 Python 停止循环。

## 实践指南：测试
测试是验证函数的行为是否符合预期的操作
- 通常写为另一个函数
  - 这个函数包含一个或多个被测函数的样例调用
  - 测试涉及到挑选**特殊的参数值**
  - 测试也可作为文档

### 断言
断言是使用 ```assert``` 语句来验证预期，例如测试函数的输出。
  - ```assert``` 语句在布尔上下文中只有一个表达式，后面是带引号的一行文本（单引号或双引号都可以）
  - 如果表达式求值为假，表达式就会显示。
``` python
assert fib(8) == 13, 'The 8th Fibonacci number should be 13'
```
为真时，没有任何效果；当它是假时，会造成执行中断

```python
>>> def fib_test():
        assert fib(2) == 1, 'The 2nd Fibonacci number should be 1'
        assert fib(3) == 1, 'The 3nd Fibonacci number should be 1'
        assert fib(50) == 7778742049, 'Error at the 50th Fibonacci number'
```

测试可以写在同一个文件，或者后缀为 `_test.py` 的相邻文件中

### Doctest
Python 提供了一个便利的方法，将简单的测试直接写到函数的文档字符串内。
  - 第一行应该包含单行的函数描述
  - 后面是一个空行
  - 接着是参数和行为

例如：

```python
>>> def sum_naturals(n):
        """Return the sum of the first n natural numbers

        >>> sum_naturals(10)
        55
        >>> sum_naturals(100)
        5050
        """
        total, k = 0, 1
        while k <= n:
          total, k = total + k, k + 1
        return total
```

可以使用 doctest 模块来验证
```python
>>> from doctest import run_docstring_examples
>>> run_docstring_examples(sum_naturals, globals()) # globals函数返回全局变量的表示，解释器需要它来求解表达式。
```
- 可以通过以下面的命令行选项启动 Python 来运行一个文档中的所有 doctest。
```bash
python3 -m doctest <python_source_file>
```
高效测试的关键是在实现新的函数之后（甚至是之前）**立即编写（以及执行）测试**。

只调用一个函数的测试叫做**单元测试**，详尽的单元测试是良好程序设计的标志。


# 5 高阶函数 Higher-OrderFunction
高阶函数是：
- 接受**其他函数作为参数**的函数
- 或将**函数作为返回值**的函数

## 作为参数的函数
- 优势
  - 更强大的抽象类型：
    - 一些函数表达了计算的**一般方法**，独立于它们调用的特定函数
    - 命名和函数允许我们抽象而远离大量的复杂性   
    - 每个通用的概念或方程都能映射为自己的小型函数
- 缺陷
  - 全局帧会被小型函数弄乱
    - 解决方法： 嵌套函数

## 作为返回值的函数

```python
def make_adder(n):  # 用来定义这个 function
    """
    >>> add_three = make_adder(3)
    >>> add_three(4)
    7
    """
    def adder(k):  # 将要返回的 function , parent 不是 global [parent = f1] 
        return k+n
    
    return adder  # 返回的是 function

make_adder(4)(3) # 7
```

![returnFunctionExpressionTree](../pics/returnFunctionExpressionTree.png)

![environmentDiagramFroNestedDefStatements](../pics/environmentDiagramFroNestedDefStatements.png)
- 注意 parent 的指向
- n 在 f2 找没找到，在 f1 找找到了
- f1 一直存在

## Lambda 表达式
- 使用 Lambda 表达式创建函数，它会求值为匿名函数
- 函数体是具有单个返回表达式的函数

  
```python
square = lambda x: x * x # lambda x: x * x 求出了一个 function
square(4) # 16
(lambda x: x * x)(3) # 9


>>> def compose1(f,g):
        return lambda x: f(g(x))

#      lambda            x            :          f(g(x))
# "A function that    takes x    and returns     f(g(x))"

# Lambda expressions can be used as an operator
# or operand
negate = lambda f, x: -f(x)
negate(lambda x: x * x, 3)  # -9
```

Q:
- lambda 表达式做什么?
- 对 lambda 表达式求值时发生了什么？
- 在什么情况下 lambda 表达式是有用的?

A:
  - lambda 表达式创建函数。
  - 当对一个 lambda 表达式求值时，它会生成一个函数。
  - 对于那些我们不需要用很久的函数,我们经常使用 lambda 来创建简短的匿名函数。

# Iteration
函数可以自己调用自己，自己调用自己的函数是递归。

```python
def print_all(x):
    print(x)
    return print_all

print_all(1)(3)(5)   # 1 3 5

def print_sum(x):
    print(x)
    def sum(y):
        return print_sum(x+y)
    return sum

print_sum(1)(3)(5)   # 1 4 9

```

## if
![ifStatements](../pics/ifStatements.png)

为什么 if 不是右边的结构？
- 因为调用的求值规则，右边的话括号内部会先被全部求值

## 逻辑操作
短路：
- `A or B`
  - A 真 返回 A
    - `1 or 2 # 1`
  - A 假 返回 B
    - `0 or 2 # 2`
- `A and B`
  - A 真 返回 B 
    - `1 and 2 # 2`
  - A 假 返回 A
    - `0 and 2 # 0`

条件语句
  - ```<consequent> if <predicate> else <alternative>```

## 函数装饰器
将高阶函数用作执行 `def` 语句的一部分，叫做装饰器
  
```python
def trace1(fn):   # 接受函数 f
    def wrapped(x):
        print('-> ', fn, '(', x, ')')    # 打印地址
        return fn(x)  # 返回 f(x)
    return wrapped    # 返回 f(x)

def triple(x):
    return 3 * x

triple # tripe 方法 <function triple at 0x000001458C2EEB80/>

trace1(triple)(4)
# ->  <function triple at 0x000001458C2EEB80> ( 4 )
# 12

@trace1   # 装饰器
def another_triple(x):
    return 3 * x

another_triple # trace1 中的返回函数的方法 <function trace1.<locals>.wrapped at 0x000001458C2EED30>


another_triple(4)
# ->  <function another_triple at 0x000001458C2EEA60> ( 4 )
# 12
```
`@trace1` 影响 `def` 的执行规则
- `triple` 并没有绑定到这个函数上，而是绑定到了在新定义的函数 `triple` 上调用 `trace1` 的返回函数值上。

等价关系：
```python
@trace1   # 装饰器
def another_triple(x):
    return 3 * x

# ==>

def another_triple(x):
    return 3 * x
another_triple = trace1(another_triple)
```

# 6 递归 RecursiveFunction

函数可以自己调用自己。因为
**function 在被调用时， return 前都不会离开这个 function**。

递归的设计步骤：
- 计算 `base cases` (不用递归)
- 计算 `recursive cases` (用递归)

## 环境图中的递归
- 相同的 `function` 被多次调用
- 不同的 `frames` 每次跟踪不同的 `arguments`
- `arguments` 的值取决于当前的`frames`

每次调用解决一个比之前调用时的更简单的问题

## 循环 vs 递归
- 循环是递归的特殊形式
- 运用的计算公式有区别

## 解决递归问题的方法
- 检验 base case
- 把递归的方法视为一个方法上的抽象
- 假设递归 f(n-1) 是正确的，检验 f(n) 是否正确

## 相互递归 (mutual recursion)
function A 调用 function B，function B 调用 function A

应用：
- 信用卡校验和
- 检验一个数奇数还是偶数
- 两个人互相玩游戏

## 把递归转化为迭代
- 会很 tricky
  - 因为迭代是递归的特殊形式

要知道迭代需要保持什么状态

## 把迭代转化为递归
- 更加直接，更泛用化
  - 因为迭代是递归的特殊形式

迭代时的状态可以被作为 `arguments` 传递至递归方法

## 树递归
树递归函数视为探索各种可能性的方法


# 7 List

## list 的特性
- 可以使用 + 将两个 list 相加
- in 判断元素是否在 list
- 成对出现的 list 可以用 for 循环提取
```python
>>> [41,42,43,44]
[41,42,43,44]
>>> [41,42,43,44] + [41,42,43,44]
[41,42,43,44,41,42,43,44]
>>> len([41,42,43,44])
4
>>> 41 in [41,42,43,44]
True
>>> 41 not in [41,42,43,44]
False
>>> [41,42] in [41,42,43,44]
False
>>> for i in [41,42,43,44]:
        print(i)
41
42
43
44
>>> for i,j in [[41,42],[43,44]]:  # 适用于成对出现的序列
        print(i,j)
41 42
43 44

```

## range
range 是连续数字的序列
- 用_表示不关心序列里的值
```python
>>> list(range(-2,2))
[-2, -1, 0, 1]
>>> list(range(2))
[0, 1]
>>> for _ in range(3):   # 用_表示不关心序列里的值
        print("Go bear!")
Go bear!
Go bear!
Go bear!
```

## 列表推导 (List Comprehensions)
- `[function(x) for x in list if boolean(x)]`
```python
>>> odds = [1,3,5]
>>> [x+1 for x in odds]
[2,4,6]
>>> [x for x in odds if 25 % x == 0]
[1,5]
>>> [x+1 for x in odds if 25 % x == 0]
[2,6]
```

## 字符串 (Strings)
字符串本身是字符的不可改变的 array
- 可以用 `len(string)` 求的字符串数量
- 可以用 `string[i]` 提取单个字符
- 可以用 in 判断子字符是否在字符中

```python
>>> city = 'Berkeley'
>>> len(city)
8
>>> city[3]
'k'
>>> 'here' in "Where's"
True
```

## 切片 (Slice)
slice 是创建出了一个新的 list

```python
>>> odds = [1,3,5,7,9]
>>> odds[1:3]
[3, 5]
>>> odds[:3]
[1, 3, 5]
>>> odds[1:]
[3, 5, 7, 9]
>>> odds[:]
[1, 3, 5, 7, 9]
```

## 序列函数
### sum
`sum(iterable, start=0)` -> value
- 返回的 value 是：`start` (默认值:0) 加上一个可迭代的数字，当可迭代为空时，返回开始值。

```python
>>> sum([[2,3],[4]],[])  # 相当于 [] + [2,3] + [4]
[2, 3, 4]
```

### max
- `max(iterable, key=func)` -> value
- `max(arg1, arg2, *args, *[, key=func])` -> value

```python
>>> max(range(10),key = lambda x: 7-(x-4)*(x-2))  # y = 7-(x-4)*(x-2) 当 y 取最大值时， x是？
3
```

### all & any
`all(iterable, /)`
- 如果 `bool(x)` 迭代中的**所有**x都为真，则返回真。
- 如果可迭代为空，则返回**True**。
`any(iterable, /)`
- 如果 `\bool(x)` 迭代中的**一个**x为真，则返回真。
- 如果可迭代为空，则返回**False**。

```python
>>> all([2,4])
True
>>> all([2,4,0])
False
>>> all([2,4,[]])
False
>>> all([2,4,[1]])
True
```


# 8 data abstraction
复合数据 可以看做 单个概念单元

解决两个问题
  - 如何表示数据
  - 如何处理数据

## 分数是用 一对数字 来表示的经典例子

```python
>>> pair = [1,2]
>>> pair
[1,2]

>>> x,y = pair # unpacking a list
>>> x
1
>>> y
2
```

## 复合数据体现的抽象原则
抽象屏障 (Abstraction Barriers)
  - 选择适合的抽象层方法，不要用不属于自己抽象层的方法
  
优点：
  - 使得程序更容易被更改
  - 可以改变 data representation 无需重写整个程序

例如：
```python
def rational_mul(x,y):
    return rational(numer(x) * numer(y), denom(x) * denom(y))

#---------用列表表示-------------------

def rational(x,y):
    return [x,y]

def numer(x):
    return x[0]

def denom(x):
    return x[1]

#---------用HOF表示-------------------

def rational(x,y):
    def select(keyword):
        if keyword == "n":
            return x
        if keyword == "d":
            return y
    return select

def numer(x):
    return x("n")

def denom(x):
    return x("d")
```

## dict
特点
- 无顺序
- 键值对
- 两个 key 不能相同
- key 不能是列表


```python
>>> numberals = {'I':1,'V':5,'X':10}    # dict 没有顺序
>>> numberals
{'I': 1, 'V': 5, 'X': 10}

#----- keys() --------
>>> numberals.keys()
dict_keys(['I', 'V', 'X'])

#----- values() ------
>>> numberals.values()
dict_values([1, 5, 10])

#------ items() ------
>>> numberals.items()
dict_items([('I', 1), ('V', 5), ('X', 10)])

#---- list -> dict ------
>>> items = [('I', 1), ('V', 5), ('X', 10)]
>>> dict(items)
{'I': 1, 'V': 5, 'X': 10}

#--- 根据 key 获取 value ------
>>> dict(items)['X']
10

#--------- in -----------
>>> 'X' in numberals
True
>>> 'X-ray' in numberals
False

#---- get(key,default) ---------
>>> numberals.get('X',0)
10
>>> numberals.get('X-ray',0)
0

#------ dict 推导式 ------------
>>> {x:x*x for x in range(5)}
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# ---- 两个 key 不能相同 --------
>>> {1:2,1:3}
{1: 3}

# ----- value 能是列表 --------
>>> {1:[2,3]}
{1: [2, 3]}

# ----- key 不能是列表 --------
>>> {[1]:2}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

## 如何知道违反了数据抽象原则
简而言之，违反了数据抽象是指绕过 ADT 的构造函数和选择器，直接使用其在其余代码中的实现方式，并假设其实现不会更改。
- 我们不能假设我们知道 ADT 是如何构造的，只能使用构造函数。
- 我们不能假设我们知道如何访问 ADT 的详细信息，除非使用选择器。 
- 细节应该由构造函数和选择器抽象出来。 

如果我们绕过构造函数和选择器，直接访问细节，那么对 ADT 实现的任何微小更改都可能破坏整个程序。

## 为什么要保持数据抽象
通过正确实现这些数据类型，可以使代码更具可读性，因为：
- 可以使构造函数和选择器的名称更具信息性
- 让团队协作更轻松
- 其他程序员不必担心实现细节
- 防止错误的传播
  - 可以在单个函数中修复错误，而不是在整个程序中修复错误


# 9 Objects
对象代表着信息
- 它包含数据和行为，捆绑在一起以创建抽象
- 对象可以代表事物，但也可以代表属性、交互和过程
- 对象的类型称为类;类是 Python 中的头等值

面向对象的程序设计
  - 组织大型程序的比喻
  - 可以改进程序组成的特殊语法

Python 中的每个值都是对象
  - 所有对象都有属性
  - 许多数据的处理是通过对象方法
  - 函数只做一件事情，但对象做多个事情(做每个事情都是一个函数)

## string 是 对象
string 有属性
  
```python
s = "Hello"
s.upper()    # 'HELLO'
s.lower()    # 'hello'
s.swapcase() # 'hELLO'

a = 'A'
ord(a)       # 65  对应的 ACSII 数
hex(ord(a))  # '0x41' 是 65 对应的 16进制
```

## Object can change

所有引用同一对象的名称都会受到变化
  - list & dictionaries
  
## tuples
不可改变的序列
  
```python
>>> (3,4) + (5,6)
(3,4,5,6)
>>> {(1,2):3}
{(1,2) : 3}
>>> {[1,2]:3}
error
```

他能保护 values 不被改变
  
```python
>>> turtle = (1, 2, 3)
>>> ooze()
>>> turtle
(1, 2, 3)

>>> list = [1, 2, 3]
>>> ooze()
>>> list
['Anything could be inside']
```
如果 tuple 内有可改变的序列，它的元素也有可能发生改变
  
```python
>>> s = ([1, 2], 3)
>>> s[0] = 4
ERROR
>>> s[0][0] = 4
([4, 2], 3)
```

## 相同与改变
- 只要我们不修改对象，一个复合对象就是它各个部分的总和
- 除了组成复合数据对象的各个部分之外，复合数据对象本身还有一个“标识”
- 即使我们改变了它的内容，复合数据对象这个“标识”还是未改变
- 两个 list 恰好有相同的内容，但他们是不同的。（复合数据对象这个“标识”不一样）

A is B  
- 对应相同的 object 则相同 (类似 java 的 == )

A == B
- 对应相同的 value 则相同 (类似 java 的 equals )

```python
>>> [1,2] == [1,2]
True
>>> [1,2] is [1,2]
False
```
## 一些陷阱
默认实参值是函数的一部分，不是由调用生成的
  
```python
def f(s=[]):
    s.append(5)
    return len(s)

f()     # 1
f()     # 2
f([1])  # 2
```

![defaultArgs](../pics/defaultArgs.png)


# 10 Non-Local Assignment
在函数内部创建的变量，只在自己的 `Frame` 里使用。  
使用高阶函数时，内部函数想要无法引用/改变 `parent frame` 的变量。

解决方法：使用 `nonlocal` 声明变量使内部函数的变量绑定为 `parent frame` 的变量。

```python
def make_withdraw(balance):
    "Return a withdraw function with a starting balance."
    def withdraw(amount):
        nonlocal balance # declare the name "balance" nonlocal
        if amount > balance:
            return "Insufficient funds"
        balance -= amount
        return balance
    return withdraw

withdraw = make_withdraw(100)
withdraw(25)
withdraw(25)
wtihdraw(60)

```
## Non-Local 细节

对经过 `nonlocal` 声明过的 `name` 重新赋值，改变的是 `name` 的第一个非本地框架 (**first non-local framework**) 中预先存在的值
- 该 `name` 要已经声明过
- 该 `name` 不能与 `local scope` 的 `name` 冲突


|状态(x=2)|影响|
|:---:|:---:|
|没有nonlocal,x 没有在本地绑定|在当前环境的第一个 frame 中,创建一个 x 到对象 2 的新绑定|
|没有nonlocal,x 已经在本地绑定过|在当前环境的第一个 frame 中,将 x 重新绑定到对象 2 |
|有nonlocal x,x 已经在 non-local frame 绑定过|在当前环境的第一个 non-local frame 中,将 x 重新绑定到对象 2 |
|有nonlocal x,x 没有在 non-local frame 绑定过|语法错误|
|有nonlocal x,x 已经在 non-local frame 绑定过, x也在本地绑定过|语法错误|


## Python particulars(细节)

python 在执行方法前，先求出 `name` 属于的 `frame`

- 在 `function` 内部，单一 `name` 的所有实例必须指向同一个框架

  
```python
def make_withdraw(balance):
    """Return a withdraw function with a starting balance."""
    def withdraw(amount):
        if amount > balance:
            return 'Insufficient funds'
        balance = balance - amount  # 执行前计算，得知 balance 是本地变量
        return balance
    return withdraw
```

```python
def make_withdraw(balance):
    """Return a withdraw function with a starting balance."""
    def withdraw(amount):
        if amount > balance:
            return 'Insufficient funds'
        # balance = balance - amount # 如果去掉，可以正常执行，因为这个 name 指向前一个 frame
        return balance
    return withdraw

```

其他可选的方法：  
用可变值，如列表。

```python
def make_withdraw_list(balance):
    b = [balance]
    def withdraw(amount):
        if amount > b[0]:
            return 'Insufficient funds'
        b[0] = b[0] - amount
        return b[0]
    return withdraw
```


## 突变会导致 函数的引用透明性 (referential transparency) 丢失
返回值只依赖于输入值，这种特性称为引用透明性。  
`Mutation operations` 违反引用透明度，因为它们改变了环境。

# 11 Iterators
容器可以提供迭代器(按顺序访问其元素)
- ```iter(iterable)```: 在可迭代值的元素上返回一个迭代器
- ```next(iterator)```: 返回迭代器中的下一个元素

这个功能可以由 带 `nonlocal index `的高阶函数实现。

## 字典的迭代器

特点：
- 字典的 `key`, `value`, `item`(输出的是 `tuple`) 都是可迭代的
- 字典的 `size` 在迭代器中不能改变

## `iter` vs `range`

特点：
- `iter` 中已经输出的值不会再出现
- `range` 每次都能输出想要的值
  
```python

# ------ range --------
>>> for i in range(3,6):
>>>     print(i)
3
4
5

>>> for i in range(3,6):
>>>     print(i)
3
4
5

# ------ iter --------
>>> ri = iter(range(3,6))
>>> for i in ri:
    print(i)
3
4
5

>>> for i in ri:
>>>     print(i)

>>> 
```

## `python` 内建的迭代器功能
许多内建的 Python 序列操作都会返回迭代器，这些迭代器会 lazily 地计算结果
  - `map(func, iterable): `: Iterate over func(x) for x in iterable 
  - `filter(func, iterable):`: Iterate over x in iterable if func(x)
  - `zip(first_iter, second_iter):` : Iterate over co-indexed (x, y) pairs
  - `reversed(sequence):` : Iterate over x in a sequence in reverse order

要查看迭代器的内容，将生成的元素放入一个容器中:
  - `list(iterable)`: Create a list containing all x in iterable
  - `tuple(iterable)`: Create a tuple containing all x in iterable
  - `sorted(iterable)`: Create a sorted list containing x in iterable

```python
>>> bcd = ['b', 'c', 'd']
>>> [x.upper() for x in bcd]  # 这是直接生成的情况
['B', 'C', 'D']

>>> caps = map(lambda x: x.upper(), bcd)  # 这是返回的迭代器 
>>> next(caps)
'B'
>>> next(caps)
'C'
```

## 生成器(`generator`)
生成器是特殊的迭代器，使用生成器函数来获得。

```python
def plus_minus(x):
"""Yield x and -x."""
    yield x
    yield -x

>>> t = plus_minus(3)
>>> next(t)
3
>>> next(t)
-3
>>> list(plus_minus(5))
[5, -5]
>>> list(map(abs, plus_minus(7)))
[7, 7]

```

特点：
- 生成函数是一个产生值 (`yields values`) 而不是返回值的函数。
- 一个普通函数只 `return` 一次，一个生成函数可以 `yield` 多次。
- 生成器是通过调用生成器函数自动创建的迭代器
- 当调用生成器函数时，它返回一个生成器，该生成器对其 `yields` 进行迭代。
- 生成函数和 迭代器 类似，都是 `lazily` 地运算的。因此 `yield` 后在那个框架会暂停

## yield from
生成器有时候也要输出迭代器, `yield from` 语句产生一个 迭代器 或 所有可迭代的值。
  
```python
def a_then_b_for(a, b):
    """The elements of a followed by those of b.

    >>> list(a_then_b_for([3, 4], [5, 6]))
    [3, 4, 5, 6]
    """
    for x in a:
        yield x
    for x in b:
        yield x

def a_then_b(a, b):
    """The elements of a followed by those of b.

    >>> list(a_then_b([3, 4], [5, 6]))
    [3, 4, 5, 6]
    """
    yield from a
    yield from b

def countdown(k):
    """Count down to zero.

    >>> list(countdown(5))
    [5, 4, 3, 2, 1]
    """
    if k > 0:
        yield k
        yield from countdown(k-1)
```

# 12 OOP
面向对象编程是组织模块化编程的方法，其将信息和相关行为捆绑在一起，是一种对使用**分布式状态**的隐喻。
- 每个对象都有自己的本地状态
- 每个对象也知道如何管理自己的本地状态(基于方法调用)

OOP 的方法调用 是 对象之间传递 的 信息，OOP 的几个对象可能都是一个共同类型的实例。

## Classes
类是实例的模板。当类语句执行时， suite 被执行。  

```python
class <name>:
    <suite> # 当类语句执行时，该 suite 被执行。
```

类声明创建了一个新的类，并将该类绑定到当前环境的第一个框架中的`<name>`。  
`<suite>` 中的 `Assignment` 和 `def` 语句创建了类的属性。
```python

class Clown:
    """An illustration of a class statement. This class is not useful.

    >>> Clown.nose # 被立即执行了
    'big and red'
    >>> Clown.dance()
    'No thanks'
    """
    nose = 'big and red'
    def dance():
        return 'No thanks'
```

## Object Construction

当一个 `class` 被调用
- 该 `class` 的一个新实例被创建 (`self` 绑定为这个 `instance`)
- 类的 `__init__` 方法被调用，新对象作为它的第一个参数(命名为 `self`)，以及在调用表达式中提供的任何附加参数。

```python
class Account:
    """An account has a balance and a holder.
    All accounts share a common interest rate.

    >>> a = Account('John')
    >>> a.holder
    'John'
    >>> a.balance
    10
    >>> a.interest
    0.02
    >>> Account.interest = 0.04
    >>> a.interest
    0.04
    """

    interest = 0.02  # A class attribute

    def __init__(self, account_holder):  # self 绑定 实例
        self.holder = account_holder     # 实例里有 holder
        self.balance = 0                 # 实例里有 balance
```

## method
所有被调用的方法都可以通过 `self` 参数访问对象，因此它们都可以访问和操作对象的状态。
- `.` 符号自动提供方法的第一个参数。

```python
class Account:
    """
    >>> a = Account('John')
    >>> a.deposit(100) 
    100
    >>> a.withdraw(90)
    10
    >>> a.withdraw(90)
    'Insufficient funds'
    """

    interest = 0.02  # A class attribute

    def __init__(self, account_holder):
        self.holder = account_holder
        self.balance = 0

    def deposit(self, amount):
        """Add amount to balance."""
        self.balance = self.balance + amount
        return self.balance
```

## Attributes(属性)
- 使用 `getattr(<instance>,<attribute(string)>)` 获得对应属性的值
- 使用 `hasattr(<instance>,<attribute(string)>)` 判断对应属性是否存在

在一个对象中查找一个属性名可能会返回：  
对象的一个实例属性或一个类属性。

## Methods 和 Functions
绑定方法(Bound methods)，它将函数和将被调用的对象结合在一起。

  `Object + Function = Bound Method`
  
```python
>>> type(Account.deposit)
<class 'function'>
>>> type(tom_account.deposit)
<class 'method'>
```

## Class Attributes
类的特性：
- 类的属性在类的所有实例中是 “共享” 的，因为它们是类的属性，而不是实例的属性。
- 类的属性没有 copy 到实例中
- 它改变了所有实例的类属性都改变了

# 13 Inheritance (继承)

`Python` 是对象系统
- `Functions` 是 对象
- `Bonds methods` 也是对象 (第一个参数 `self` 已经绑定给实例的方法)
- 点式表达式求类属性的 `Bonds methods` 是 `functions`。
    
    
点式表达式 `<instance>.<method_name>`
- 左边生成一个 `object`
- 右边在 `instance` 属性内部找 `name`
- 如果没有，找类属性
- 该值将返回，除非它是一个函数(会返回绑定方法)
  
## 继承
继承是一种将类联系在一起的技术。子类可以具有与父类类相同的属性，以及一些特殊情况下的行为。 
- 两个子类可以在*定制化*的程度上有差异。

```python
class <Name>(<Base Class>):
    <suite>
```

利用继承，我们通过指定子类与父类的不同之处来实现子类。

## 在 classes 中寻找属性
步骤：
在子类中查看该属性，有则返回这个 attribute value。如果没有则去父类中找。

```python
class CheckingAccount(Account):
    """A bank account that charges for withdrawals."""
    withdraw_fee = 1
    interest = 0.01
    def withdraw(self, amount):
        return Account.withdraw(self, amount + self.withdraw_fee)

>>> ch = CheckingAccount('Tom') # Calls Account.__init__
>>> ch.interest                 # Found in CheckingAccount
0.01
>>> ch.deposit(20)              # Found in Account
20
>>> ch.withdraw(5)              # Found in CheckingAccount
14


```

## 面向对象的设计
原则：
- 不要重复自己，使用已经存在的实现方式
- 尽可能在实例上查找属性

## 继承和构成
继承最好用在 is-a 的关系上。例如，支票账户是一种特殊的账户类型。

构成最适合表示 has-a 关系。例如，一家银行管理着一系列的银行账户。
    
## 多重继承
子类可以有多个基类
```class SubclassName(BaseClass1, BaseClass2, BaseClass3, ...):```
- 先调用 subclass ，在调用 baseclass

多重继承会使得程序变得很复杂，因此应该极少使用

# 14 Representations
探索一些用于组合和操作**不同类型对象**的方式。

## String Representations
所有对象有两个 `String representation`：
- `str` 设计为人类易读，结果是用 `print` 调用得出的结果
- `repr` 易于 `python` 解释器，`eval(repr(object))`打印的和 `iteractive` 模式一样的内容

## 多态函数
对多种数据类型的均适用的函数。例子： `repr` 和 `str`。


`repr`
- `repr` 调用一个 0 参数的方法 `__repr__`
- 只调用类的 `__repr__`，实例无 `__repr__`

`repr` 源码：    
```python
def repr(x):
    return type(x).__repr__(x)
```
  
`str`  
- `str` 调用一个 0 参数的方法 `__str__`
- 只调用类的 `__str__`，实例无 `__str__`
- 如果没有`__str__`, 则使用 `repr`

str 源码：
```python
def str(x):
    t = type(x)
    if hasattr(t,'__str__'):
        return t.__str__(x)
    else:
        return repr(x)
```
      
## 接口
消息传递并不仅仅提供用于组装行为和数据的方式。它也允许不同的数据类型以不同方式响应相同消息。来自不同对象，产生相似行为的共享消息是抽象的有力手段。  
接口是共享消息的集合，带有它们含义的规定
- 信息传递。对象之间通过互相查找各自的属性进行交互
- 属性查找规则允许对不同的数据类型响应相同的消息
- 一个共享的消息（属性名），可以从不同的对象中引出类似的行为，这是一种强大的抽象方法
- 一个接口是一组共享的消息，以及它们的含义的规范。
  - 实现`__repr__`和`__str__`方法的类，返回 `Python-interpretable` 和 `human-readable` 字符串，实现了一个用于生成字符串表示的接口。

接口的优点：
- 用于每个表示的类可以独立开发；
- 它们只需要遵循它们所共享的属性名称。


## Special Method Names
某些名称是特殊的，因为它们有内置的方法，这些名称总是以两个下划线开始和结束。
- `__init__` : 构造对象时自动调用的方法
- `__repr__` : 被调用的方法，以Python表达式的形式显示一个对象。
- `__add__` : 调用方法将一个对象添加到另一个对象中
  - `>>> Ratio(1, 3) + Ratio(1, 6) # Ratio(1, 2)`
  - `Python` 会检查表达式的左操作数`__add__`和右操作数`__radd__`上的特殊方法  
- `__bool__` : 被调用的方法，用于将一个对象转换为 `True` 或 `False`。
- `__float__` : 调用方法将对象转换为float（实数）。

利用 `isinstance` 方法和类型转换来进行对象间的交互

## 泛用函数
如何使用相同的概念，定义不同种类、并且不共享通用结构的参数上的泛用操作？(如将复数与有理数相加)


### 类型分发
一种处理跨类型操作的方式是**为每种可能的类型组合设计不同的函数**，操作可用于这种类型。
- 编写一个函数，首先检测接受到的参数类型，之后执行适用于这种类型的代码
- 可以通过以字典实现类型分发
- 这个基于字典的分发方式是递增的，可扩展。任何新的数值类型可以将自己“安装”到现存的系统中，通过向这些字典添加新的条目。  
### 数据导向编程
将**任意运算符作用于任意类型**，并且使用字典来储存多种组合的实现。

优点：
- 管理了跨类型运算符的复杂性

缺点：
- 十分麻烦
- 引入新类型的开销不仅仅是为类型编写方法，还有实现跨类型操作的函数的构造和安装
      
```python
# 这是字典 -> ('计算方法', ('数据类型1', '数据类型2')): 对应具体函数
>>> apply.implementations = {('mul', ('com', 'com')): mul_complex,
                             ('mul', ('com', 'rat')): mul_complex_and_rational,
                             ('mul', ('rat', 'com')): mul_rational_and_complex,
                             ('mul', ('rat', 'rat')): mul_rational}
        
# 泛用函数，支持各种数据类型 [key]->('计算方法', ('数据类型1', '数据类型2'))
# apply.implementations -> 字典
# apply.implementations[key] -> 函数

>>> def apply(operator_name, x, y):
        tags = (type_tag(x), type_tag(y))
        key = (operator_name, tags)
        return apply.implementations[key](x, y)
        
# 求值
>>> apply('add', ComplexRI(1.5, 0), Rational(3, 2))
ComplexRI(3.0, 0)
```

### 强制转换。
将参数强制转换为**相同类型**的值，之后仅仅调用运算符(仅需要相同类型的运算符的字典)  
需要设计强制转换函数，可以定义强制转换函数的字典。

优点：
- 仅需要为每对类型编写一个函数，而不是为每个类型组合和每个泛用方法编写不同的函数
- 可以让两个不同类型强制转换为第三个来降低了程序所需的转换函数总数

缺点：
- 会丢失信息
  
  
## Property Method
对无参数的函数使用点式表达式进行调用。  
`@property` 装饰器允许函数不使用标准调用表达式语法来调用。

```python
class s:
    # a getter function 
    @property
    def magnitude(self):
        return ...

    # a setter function 
    @magnitude.setter
    def magnitude(self,value):
      ... = value
  
s.magnitude      # 可以这么调用 getter function
s.magnitude = .. # 可以这么调用 setter function
```

