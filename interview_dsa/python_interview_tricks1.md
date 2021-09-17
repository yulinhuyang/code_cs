### 1 主要参考：

刷题顺序： 剑指offer --> labuladong --> 头条腾讯 高频面试题

剑指offer:

https://github.com/JushuangQiao/Python-Offer

https://zhuanlan.zhihu.com/p/75864673

leetcode:

https://blog.csdn.net/qq_32424059/article/details/88855423


### 2 notes 

#### python使用基础

**创建一维、二维数组**

    array = [0 for i in range(3)]

    two_array = [[0 for i in range(3)] for i in range(3)]

//简单方法： [[0]*(n+1)]*(m+1)  (这种有问题，因为浅拷贝，数组一行的0指向了同一个位置)


**循环:**

    python反向循环：

    for i in range(len(a)-1,-1,-1):

    while循环，非+1情况


fib 循环法: a,b = b, a+b

**for range**
    
    指定 start stop step

 
 **简单数据结构**
 
     class ListNode:
        def __init__(self, x):
            self.val = x
            self.next = None

    class TreeNode:
        def __init__(self, x):
            self.val = x
            self.left = None
            self.right = None
   
 **注意点**
    
    return None 和return []

    判等 ==

    self 使用

    
**list API**

```python
list = []

list.append('a')
list.append('b')
list.append('c')
print(list)
list.insert(2,'d')
print(list)

list.pop() # 默认最后一个元素 -1
list.sort()

sort的key的使用:
    
    sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。

    sorted(intvs,key=(lambda x:x[1])) 或者 intvs.sort(key=lambda x: x[1])

    people = sorted(people,key = lambda x: (-x[0],x[1])) # 数对问题，先正序排，后逆序排
    


反转链表： res[::-1]  reverse(res)

list 和str互相转换：
  
    str->list
    
    str1 = "12345"
    list1 = list(str1)
    
    
    list2 = str2.split( ) 或者  list3 = str3.split(".")
    
    list->str
    
    str4 = "".join(list3)  或者 str4 = " ".join(list3)

```


**dict API**

```python

map = {}    # map = dict()

#map = {'a': 1,'b': 2}

#添加或者更新
map['a'] = 1 
map['b'] = 2

print(map.keys())
print(map.values())
print(map.items())


map的迭代方式：

for key,values in  dict.items():
....

#删除条目

print(map.pop('a'))

if 'b' in map:
    print(2)
```

更新指定值：

map[sum_i] = map.get(sum_i,0) +1

dict.get(key, default=None)  获取指定key对应的值


map —>value->list 结构
```python
map = collections.defaultdict(list)

eg：
        #map --> value(list)

    map = collections.defaultdict(list)
    for i in range(len(strs)):
        char_str = ''.join(sorted(strs[i]))
        map[char_str].append(strs[i])
    
    return list(map.values())
```


**string  API**

```python

string 排序后组合

strs = ['ate','eat']

    char_str = ''.join(sorted(strs[i]))

```


**tuple API**

	tup1 = ('physics', 'chemistry', 1997, 2000)  

	小括号创建元组

**deque API**

```python

import collections

d = collections.deque()

d.append('a')   #右边添加元素

d.pop()   #右边弹出元素

d.appendleft('b')   #左边添加元素

d.popleft() # 将最左边的元素取出

```

**heapq API**

```python

import heapq

heapq默认的是小顶堆

空列表 A = []

heapq.heappush(A,num)

heapq.heappop(A)

heapq.heapreplace(heap, item) 返回并删除堆中的最小item，同时添加一个新item

```

**str**


str:切片   strp[2:3]

for st in str:    迭代str    
	
**set 集合**

用set来表示一个无序不重复元素的序列。set的只要作用就是用来给数据去重。 

可以使用大括号 { } 或者 set() 函数创建集合

eg: 
    
    添加一对数：
    
    vis = set([(0, 0)])
    
    vis.add((i, j))
 
    res = set()
    
    res.add(path)
    
    return list(res)   

/ 除以，带小数

//整除
    
**counter**

```python

A=['a','b','b','c','d','b','a']

count=collections.Counter(A)
 
count.items()

count.keys()

count.values()
   
```    
    
**位运算**

```python

& 按位与

| 按位或

^ 位异或

~ 按位取发

<< 左移

>> 右移

```    
**闭包传值**

self.ans 操作

list 直接引用
    
