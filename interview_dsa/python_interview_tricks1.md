

https://docs.python.org/zh-cn/3/tutorial/datastructures.html

#### 1 list  

```python
list = []
list.append('a') #添加
list.insert(2,'d') #插入
list.pop()   #删除默认最后一个元素 -1
list.extend(L) #扩展list
list.count(1)
list.index('c') #返回第一个值为'c'的索引
list.copy() #浅拷贝，同a[:]
l3 = l1 + l2 #两个list合并

#sort
intvs.sort(key=lambda x: x[1])

#sorted 可以对所有可迭代的对象进行排序操作。
intvs = sorted(intvs,key = lambda x: (-x[0],x[1]))
intvs = sorted(intvs, key=lambda x: x[1], reverse=True)  

#反转链表
list.reverse()
res[::-1]  
reverse(res)

#list的拷贝
ans.append(list(nums)); #新建拷贝
ans.append(list(nums[:])); #切片拷贝

#代替stack
stack = [3, 4, 5]
stack.append(6)
stack.pop()

#列表推导式
[str(round(pi, i)) for i in range(1, 6)] #嵌入函数
[[row[i] for row in matrix] for i in range(4)] #嵌套

#del 按索引不是值移除元素
del list[2:4]
```

**数组创建与循环**

```python
array = [0 for i in range(3)] #创建一维
array = [0] * 3 #创建一维
two_array = [[0 for i in range(3)] for i in range(3)] # 二维数组 只能这样创建
#简单方法： [[0]*(n+1)]*(m+1)  (这种有问题，因为浅拷贝，数组一行的0指向了同一个位置)

g = [[] for _ in range(n)]  ##创建二维空数组

#for-in循环：
for i in range(len(x)) :        #range(start, stop[, step])
for i in range(len(a)-1,-1,-1): #反向循环

seq = ['one', 'two', 'three']
for element in seq:      #循环数据

for f in sorted(set(basket)): #按顺序，循环遍历唯一元素

#while循环
while (count < 9):
   count = count + 1

#enumerate: 可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标。    
for i, element in enumerate(seq):
	print i, element

#zip: 将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表。
a = [1,2,3]
b = [4,5,6] 
list(zip(a,b))      # 打包为元组的list
->[(1, 4), (2, 5), (3, 6)]
dict(zip(names, scores))     # 打包为元组的dict
->{'John': 98, 'Amy': 100, 'Jack': 85}

for q, a in zip(questions, answers):#同时迭代两个list
for (k1, v1), (k2, v2) in zip(dict_one.items(), dict_two.items()): #同时迭代两个dict
for temp in zip(*strs):  #迭代string的list数组，逐个取前缀

#判空
if not vis[y]:

```    
[Python zip()用法](https://blog.csdn.net/PaulZhn/article/details/104391756)

**SortedList**
```python
from sortedcontainers import SortedList
sl = SortedList(['e', 'a', 'c', 'd', 'b'])
```


#### 2  dict字典 
```python
map = {}    
map = dict()
map = dict([('sape', 4139), ('guido', 4127)])
map = {'a': 1,'b': 2}

map['a'] = 1 

print(map.keys())
print(map.values())
print(map.items())

map.pop('a') #删除
if 'b' in map: #查找

#dict.get(key, default=None) 获取指定key对应的值
map[sum_i] = map.get(sum_i,0) +1  

{x: x**2 for x in (2, 4, 6)}  #字典推导式
for key,values in  dict.items():#循环 
...

# 邻接表结构 
map = collections.defaultdict(list)
for i in range(len(strs)):
	char_str = ''.join(sorted(strs[i]))
	map[char_str].append(strs[i])
return list(map.values())
```
**map排序**

[python天天进步(3)--字典排序](https://www.cnblogs.com/vivilisa/archive/2009/03/01/1400972.html)

python map是无序的，可以进行排序。
```python
def sortedDictValues1(adict):
    items = adict.items()
    items.sort()
    return [value for key, value in items]
   
def sortedDictValues1(adict):
    keys = adict.keys()
    keys.sort()
    return [adict[key] for key in keys]
    
def sortedDictValues1(adict):
    keys = adict.keys()
    keys.sort()
    return map(adict.get,keys）#映射
    
sorted(d.items, key=lambda d:d[0]) #按键排序，返回tuple
sorted(d.items, key=lambda d:d[1]) #按值排序
```

SortedDict
```python
from sortedcontainers import SortedDict
sd = SortedDict({'c': 3, 'a': 1, 'b': 2})
```


#### 3 string

```python
str = 'Hello World!'
str[2:3]  #切片
str + str #连接
if "H" in a  #查找成员
if "M" not in a

for st in str:  #遍历字符

#字符串内建函数
list1 = str1.split(".") #切分
strs = ['ate','eat']
char_str = ''.join(sorted(strs[i])) #组合成str

str1 = "12345"
list1 = list(str1)     #str->list
str4 = "".join(list1)  # list->str

#str.find(str, beg=0, end=len(string))
str1.find(str2, 5)

#str.replace(old, new[, max])
str.replace("is", "was", 3)

#isalnum()、isalpha()
str.lstrip('8')#str.lstrip([chars]) 截掉字符串左边字符

#字符串反转
str1 = str[::-1] 
str1 = ''.join(reversed(str))
```

**字符数字类型**

python没有char类型，一个字符也是字符串

```python
int('12')
float('12.5')
str(123.45)
ord('A') # 65      字符转ascii：
chr(65)  # 'A'     ascii转char
bin(12)  # '0b1100'   转二进制 
```
**字典序输出**

1. 字符串比较：Python的大于、等于、小于等运算符可直接用于比较两个字符串（基于字典序），例如 "apple" < "banana", "9" > "10"。

2. 字符串序列排序：Python库函数sort可用于多个字符串的排序，其背后逻辑就是利用字符串比较运算符（字典序的），默认为字典序升序排序。

降序的实现方式有：sort + reverse参数，sort + 比较函数 
```python
from functools import cmp_to_key

arr = ["apple", "banana", "9", "10"]
arr.sort(reverse=True) #降序
def cmp(x, y):
    if x < y: return 1
    elif x == y: return 0
    else: return -1
arr = ["apple", "banana", "9", "10"]
arr.sort(key=cmp_to_key(cmp))
``` 
#### 4 tuple 

小括号创建元组,元组中的元素值是不允许修改,可以通过连接创建新元组,可以使用del语句来删除整个元组

```python
tuple = ('physics', 'chemistry', 1997, 2000)
tuple[-2] #倒数2个元素
del tuple 
l1 = list(tuple) #转list
arr = tuple(l1)  #转tuple
```
#### 5 deque
```python
from collections import deque
d = deque()
queue = deque(["a", "b", "c"])
d.append('a')   #添加 
d.pop()         #弹出 
d.appendleft('b') #左添加 
d.popleft()       #左弹出
```

#### 6 堆heapq(优先队列)  

heapq默认的是小顶堆，heapq.heappop， heapq.heappush

```python
import heapq
A = []
heapq.heappush(A,num)
heapq.heappop(A)
heapq.heapreplace(heap, item) #返回并删除堆中的最小item，同时添加一个新item
heapq.heapify(A)  #将list x 转换成堆
```

#### 7 set(集合)

set用来给数据去重,可以使用大括号{ }或set()函数创建集合，

创建空集合必须用set()而不是{ }，因为{ }是用来创建一个空字典

```python
res = set()
res.add(path)  #添加
res.remove('H')
x in res 
x not in res

vis = set([(0, 0)]) #一对数
vis.add((i, j))
list(res)  #转list
{x for x in 'abracadabra' if x not in 'abc'} #集合推导式

setx & sety #交集
setx | sety # 并集  
setx - sety # 差集 
s.union(t) 
s.intersection(t)
s.difference(t)  
```
```python
from sortedcontainers import SortedSet
ss = SortedSet('abracadabra')
```

#### 8 Collections

**Counter**
```python
A=['a','b','b','c','d','b','a']
count = collections.Counter(A)
count.items()
count.keys()
count.values()
```
**OrderedDict**

有序字典会记住其插入顺序
```python
d = {'banana': 3, 'apple': 4, 'pear': 1, 'orange': 2}
OrderedDict(sorted(d.items(), key=lambda t: t[0])) # 按照key排序的字典
OrderedDict(sorted(d.items(), key=lambda t: t[1])) # 按照value排序的字典
```
#### 9 bisect_left

升序序列 

a） bisect_left (同lower_bound)：返回插入位置的索引i，使得a[:i]的所有元素都 < 目标值，a[i:]的所有元素 >=目标值；

b） bisect_right (同upper_bound)：返回插入位置的索引i，使得a[:i]的所有元素都 <= 目标值，a[i:]的所有元素 >目标值。

降序序列：不支持, 可以先反转之后再使用上述方法模拟实现。

```python
import bisect
point_left = bisect.bisect_left(num_list, 7)
point_right = bisect.bisect_right(num_list, 8)
```

#### 10 内置函数 

**classmethod**

classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。

**staticmethod**
```python
class C(object):
    @staticmethod
    def f():
        print('runoob');
 
C.f();          # 静态方法无需实例化
cobj = C()
cobj.f()        # 也可以实例化后调用
```

**setattr**

setattr() 函数对应函数 getattr()，用于设置属性值，该属性不一定是存在的。

setattr(object, name, value)

getattr(object, name[, default])

```python
class A(object):
     bar = 1
a = A()
getattr(a, 'bar')        # 获取属性 bar 值
```

#### 11 类与对象

**装饰器**

@property

@lru_cache

**嵌套函数**

闭包传值
```python
def f1():
    global MIN # 使用global关键字 修改全局变量
    a = 5  # 局部变量
	b = 1
    def f2():
		global MIN # 访问并修改全局变量
		nonlocal a # 使用nonlocal关键字 修改外部函数的变量
		a += 1
		b = 2 # 内部函数变量与外部函数变量同名，覆盖
```
**自定义类**

自定义对象的排序，堆维护，优先级队列维护的问题,数据结构中的元素可比较大小,heapq或者list.sort()都只使用小于<比较，所以只需要定义类的__lt__()方法即可

```python
class Node:
    def __init__(self, x):
        self.val = x
        # 0.left;1.right
        self.flag  = -1
        self.father = None

    def __lt__(self, other):
        return self.val < other.val

```

#### 12 值与引用

[Python值传递还是引用传递](https://blog.csdn.net/hohaizx/article/details/78427406)

Python中一切事物皆对象，变量是对对象在内存中的存储和地址的抽象。

可变对象(list,dict,set等)和不可变对象(number(int float),string,tuple等),都在heap中分配

可变对象传引用，不可变对象传引用，实际等同于传递值(不可变对象无法修改)

浅拷贝(copy()):只拷贝原对象的第一层

深拷贝(deepcopy()):完全复制原变量的所有层的所有数据

#### 13 数学运算

**三目运算**

```python
b = 100
a = 10 if b>0 else 5 #a = 10
a = 100 if b<0 else 5 # a = 5
```

**swap交换**

a,b = b,a

a,b = b,a+b

**多行变一行**

a = 1 ; b = 2 ; c = 3

**极大极小值**

```python
float('-inf')、float('inf')
import sys
max_num = sys.maxsize
min_num = -(sys.maxsize-1)
```

**除法**

/ 除以，带小数

//整除

**位运算与二进制**

```python
print("{0:b}".format(a))
print(bin(a)[2:])
```

**format**

```python
print("hello {0}, this is {1}.".format("world", "python"))            # 根据位置下标进行填充
print("hello {obj}, this is {name}.".format(obj = obj, name = name))  # key填充
print("{:.2f}".format(3.1415926))    #小数点后两位输出
print('{:b}'.format(11))             #二进制输出
```

#### 14 随机数

```python
num = random.randint(1,50)  #随机整数
num = random.randrange(0, 101, 2)  #随机选取0到100间的偶数 
num = random.random()  #随机浮点数 
num = random.uniform(1, 10) # [x, y]范围内下一个随机数
num = random.choice('abcdefghijklmnopqrstuvwxyz!@#$%^&*()') #随机字符 
num = random.sample('zyxwvutsrqponmlkjihgfedcba',5)  #多个字符中生成指定数量的随机字符
ran_str = ''.join(random.sample(string.ascii_letters + string.digits, 8)) # 从a-z A-Z 0-9生成指定数量的随机字符
```

#### 15 类型注解

typing 用来对变量和函数的参数返回值类型做注解

```python
from typing import List, Tuple, Dict

names: List[str] = ['lily', 'tom']
version: Tuple[int, int, int] = (6, 6, 6)
operations: Dict[str, bool] = {'sad': False, 'happy': True}

def plus(num1: int, num2: int) -> int:
    return num1 + num2
```
#### 16 递归深度

Python的最大递归层数是可以设置的,默认的在window上的最大递归层数是 998。

可以通过sys.setrecursionlimit()进行设置,但是一般默认不会超过3925-3929这个范围。

