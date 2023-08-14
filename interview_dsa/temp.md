
扫描线与自适应辛普森积分 求多边形面积并: https://blog.csdn.net/qq_45735851/article/details/115309408

**两个区间的关系**

两个区间的关系：https://zhuanlan.zhihu.com/p/301507910

远离、重叠、相交

**空间换时间**

2352. 相等行列对： hash代替循环，空间换取时间

**二分转判定**

2517. 礼盒的最大甜蜜度：二分转判定的时候，注意 l和r如何取。


**双栈**

**defaultdict**

`defaultdict` 的初始化需要传入一个函数作为默认值，这个函数会在访问不存在的键时被调用，返回值作为默认值。常见的用法是传入 `int`、`list`、`set` 等 Python 内置类型作为默认值，这样可以方便地进行计数、分组等操作。

freq: int -> list

895. 最大频率栈

hash = defaultdict(int)

freq = defaultdict(list)


代码在线格式化：https://www.bejson.com/format/c_formater/

519. 随机翻转矩阵

index = self.hash.get(val,val)  #查询不到返回默认值

self.get(key, default=None)


list 去重合： nums = list(set(nums))

532. 数组中的 k-diff 数对: 双指针逻辑

排序部分数组：arr[i + 1:] = sorted(arr[i + 1:])

数组转字符串：ans = "".join(map(str,arr))

翻转字符串：ans[:-1] 或者 ''.join(reversed(str))

LeetCode 566. 重塑矩阵:

ans[k // c][k % c] = mat[k // n][k % n]


**Python f-string**

Python f-string 教程: https://www.freecodecamp.org/chinese/news/python-f-strings-tutorial-how-to-use-f-strings-for-string-formatting/

f"This is an f-string {var_name} and {var_name}."

**enumerate**

对于一个可迭代的（iterable）的对象（list、str），enumerate将其组成一个索引序列，利用它可以同时获得索引和值
```python
for j,s in enumerate(list2):
```

**python deque**

注意：正常队列的先进后出，对应python是：append (右添加) <->  popleft (左弹出)

```python
from collections import deque
d = deque()
queue = deque(["a", "b", "c"])
d.append('a')   #右添加 
d.pop()         #右弹出 
d.appendleft('b') #左添加 
d.popleft()       #左弹出
```

**python  卷积计算多维切片**

```python
padSig[:,  padSize: h + padSize,  padSize: w + padSize] = sig


// conv2d
# H = ( h - k + 2 * p ) / s + 1
# W = ( w - k + 2 * p ) / s + 1

# pooling
# H=(H-K)/S+1
# W=(W-K)/S+1

# matrix multi
num = np.sum(sig[:, i:i + kh, j:j + kw] * kernel)       // 方法1
inputs[i:i+p, j:j+q].flatten().T.dot(kernel.flatten())  // 方法2
```

**numpy 常用API**

```python
#numpy
a = np.array([[1, 2], [3, 4]])  # 2x2
a = np.arange(6).reshape((2, -1))  # [0 1 2 3 4 5 6]
a = np.arange(0,7,1,dtype=np.int16)
a = np.ones((2, 3, 4), dtype=np.int16)
a = np.zeros((2, 3, 4))  # 2x3x4


a[2:5]    # [a,b)
a[: :-1]  # inverse a
a = np.arange(0,20).reshape((4,5))
b = a.transpose()  # a.T

c = a.dot(b)  # mul, np.dot(a, b)
a.sum()
a.sum(axis=1) # axis = 0 col; axis = 1 row
a.min()
a.max(axis=1)
a.mean(axis=1)

np.argmax(a)  # max num index
np.argmix(a)  # min num index
np.maximum(a, 0).flatten().tolist() # relu
```






