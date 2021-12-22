### 1 主要参考：

刷题顺序： 剑指offer --> labuladong --> hot100 --> 精选TOP 70

剑指offer:

https://github.com/JushuangQiao/Python-Offer

https://zhuanlan.zhihu.com/p/75864673

labuladong:

https://labuladong.gitee.io/algo/2/

leetcode 700:

https://blog.csdn.net/qq_32424059/article/details/88855423

### 2 notes 

#### 2.1 数组与循环

**创建一维、二维数组**

    array = [0 for i in range(3)]

    two_array = [[0 for i in range(3)] for i in range(3)]

//简单方法： [[0]*(n+1)]*(m+1)  (这种有问题，因为浅拷贝，数组一行的0指向了同一个位置)


**循环与迭代**

普通for循环：
    
    for range： 指定 start stop step

    python反向循环：for i in range(len(a)-1,-1,-1):

    while循环，非+1情况

for迭代：

    seq = ['one', 'two', 'three']
    >>> for element in seq:
        ...

enumerate：

    可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标    

    >>> for i, element in enumerate(seq):
    ...     print i, element
    
    

fib 循环法: a,b = b, a+b

    
#### 2.2 list API

```python

list = []

list.append('a')
list.append('b')
list.append('c')
 
list.insert(2,'d')
 
list.pop() # 默认最后一个元素 -1

list.sort()

sort的key的使用:
    
    sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。

    sorted(intvs,key=(lambda x:x[1])) 或者 intvs.sort(key=lambda x: x[1])

    people = sorted(people,key = lambda x: (-x[0],x[1])) # 数对问题，先正序排，后逆序排
    

#反转链表： 
    
    res[::-1]  
    
    reverse(res)

#list的拷贝

    ans.append(list(nums)); #新建拷贝
    
    ans.append(list(nums[:])); #切片拷贝 

```

**stack**
    
    使用list代替


#### 2.3 dict API

```python

map = {}    //对比set

map = dict()

map = {'a': 1,'b': 2}

#添加或者更新

    map['a'] = 1 
    map['b'] = 2
    
    print(map.keys())
    print(map.values())
    print(map.items())


#map的迭代方式：

    for key,values in  dict.items():
    .    ...
    
#删除条目
    print(map.pop('a'))

#判存在
    if 'b' in map:
        print(2)

#更新指定值：

    map[sum_i] = map.get(sum_i,0) +1
    
    dict.get(key, default=None)  #获取指定key对应的值

```

**map —>value->list 结构**

    map = collections.defaultdict(list)
    
    eg：
        #map --> value(list)
    
        map = collections.defaultdict(list)
        for i in range(len(strs)):
            char_str = ''.join(sorted(strs[i]))
            map[char_str].append(strs[i])
        
        return list(map.values())



#### 2.4 str  API 

```python

str:切片   
    
    str[2:3]

    #遍历str
    for st in str:    

string 排序后组合

	strs = ['ate','eat']

    	char_str = ''.join(sorted(strs[i]))
 
#char -- int

ASCII码转换为int：ord('A')    65

int转为ASCII码：chr(65)    'A'

```

##### list 和str互相转换 
  
```python
    str->list
    
    str1 = "12345"
    list1 = list(str1)
    

    list2 = str2.split( ) 或者  list3 = str3.split(".")
    
    list->str
    
    str4 = "".join(list3)  或者 str4 = " ".join(list3)
```



##### 字典序输出

1.  字符串比较：Python的大于、等于、小于等运算符可直接用于比较两个字符串（基于字典序），例如 "apple" < "banana", "9" > "10"。

2.  字符串序列排序：Python库函数 sort 可用于多个字符串的排序，其背后逻辑就是利用字符串比较运算符（字典序的），默认为字典序升序排序。降序的实现方式有：

1）  sort + reverse参数  2）  sort + 比较函数


```python

from functools import cmp_to_key
 
# 字符串序列排序-sort+reverse参数
arr = ["apple", "banana", "9", "10"]

arr.sort(reverse=True)  # 降序

def cmp(x, y):
    if x < y: return 1
    elif x == y: return 0
    else: return -1
arr = ["apple", "banana", "9", "10"]
arr.sort(key=cmp_to_key(cmp))

``` 

#### 2.5 tuple API

tup1 = ('physics', 'chemistry', 1997, 2000)  

小括号创建元组

元组中的元素值是不允许修改,可以通过连接创建新元组

元组中的元素值是不允许删除的，但我们可以使用del语句来删除整个元组



#### 2.6 deque API 

```python

import collections

d = collections.deque()

d.append('a')   #右边添加元素

d.pop()   #右边弹出元素

d.appendleft('b')   #左边添加元素

d.popleft() # 将最左边的元素取出

```

#### 2.7 优先队列、堆 heapq API 

可以使用可自动排序的map进行替代，也能够达到减少时间复杂度的目的。如  C++(map)、 Python(SortedDict)

heapq默认的是小顶堆

用最小堆解决heapq:  heapq.heappop， heapq.heappush

```python

import heapq

空列表 A = []

heapq.heappush(A,num)

heapq.heappop(A)

heapq.heapreplace(heap, item) 返回并删除堆中的最小item，同时添加一个新item

```

#### 2.8  set 

用set来表示一个无序不重复元素的序列。

set的只要作用就是用来给数据去重。 

可以使用大括号 { } 或者 set() 函数创建集合，

注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典

```python
    
    添加一对数：
    
    vis = set([(0, 0)])
    
    vis.add((i, j))
 
    res = set()
    
    res.add(path)
    
    return list(res)   
```

    
#### 2.9  Collections

Collections(deque/defaultdict/Counter)

```python

A=['a','b','b','c','d','b','a']

count=collections.Counter(A)
 
count.items()

count.keys()

count.values()


```    
   
#### 2.10  bisect_left

升序序列 

a） bisect_left (同lower_bound)：返回插入位置的索引i，使得a[:i]的所有元素都 < 目标值，a[i:]的所有元素 >=目标值；

b） bisect_right (同upper_bound)：返回插入位置的索引i，使得a[:i]的所有元素都 <= 目标值，a[i:]的所有元素 >目标值。

降序序列：

不支持。可以先反转之后再使用上述方法模拟实现。

```python

import bisect

point_left = bisect.bisect_left(num_list, 7)

point_right = bisect.bisect_right(num_list, 8)

```


#### 2.11 内置函数

**sorted**

sorted() 函数对所有可迭代的对象进行排序操作。

sorted(iterable, key=None, reverse=False)  

list.sort() 方法只为 list 定义, 而 sorted() 函数可以接收任何的 iterable

```python
array = [{"age":20,"name":"a"},{"age":25,"name":"b"},{"age":10,"name":"c"}]
array = sorted(array,key=lambda x:x["age"])

example_list = [5, 0, 6, 1, 2, 7, 3, 4]
result_list = sorted(example_list, key=lambda x: x*-1)
```
**enumerate**

将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标

```python
seq = ['one', 'two', 'three']
for i, element in enumerate(seq):
    print(i, element)
```

**zip**

zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象

```python
a = [1,2,3]
b = [4,5,6]
zipped = zip(a,b)     # 返回一个对象
```

#### 2.12 面向对象

##### 内置函数 -- 面向对象

**classmethod**

classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。

**staticmethod**

class C(object):
    @staticmethod
    def f(arg1, arg2, ...):
    
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

##### 值与引用

[Python值传递还是引用传递](https://blog.csdn.net/hohaizx/article/details/78427406)

Python中一切事物皆对象，变量是对对象在内存中的存储和地址的抽象。

可变对象(list,dict,set等)和不可变对象(number(int float),string,tuple等),都在heap中分配

	可变对象传引用，不可变对象传引用，实际等同于传递值(不可变对象无法修改)

浅拷贝(copy()):只拷贝原对象的第一层

深拷贝(deepcopy()):完全复制原变量的所有层的所有数据

##### 装饰器

@property

@lru_cache

##### 闭包传值 

self.ans 操作

list 直接引用


#### 2.13 运算与取值

**位运算**

```python

& 按位与

| 按位或

^ 位异或

~ 按位取发

<< 左移

>> 右移

```    

**除法**

/ 除以，带小数

//整除

**极大极小值**

float('-inf')

float('inf')

