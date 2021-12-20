LeetCode 算法题讲解视频 Up 主推荐： https://learnku.com/articles/40491

# 方法

## 前缀和与差分数组

一维前缀和： S[i]= S[i-1]  + A[i]

二维前缀和： S[i][j] = S[i-1][j] + S[i][j-1] – S[i-1][j-1] + A[i][j]

树上前缀和：从根到某节点的路径上点（或边）的值之和（上到下）；某节点及其所有子节点（或边）的值之和（下到上）


差分数组定义

真实数组a = {a[1]、a[2]、…、a[n]}          // 各点真实数据

差分数组df = {df[1]、df[2]、…、df[n]}      // 各点数据的变更值 

df[i] = a[i] - a[i-1]                      // 差分数组各点数据为真实数据的变更值

a[i] = df[1] + df[2] …+ df[i]              // 差分数组的前缀和即为真实数组

a[i] = a[i-1] + df[i]                      // 真实数据也可以从上一点数据+变更值求出


## 优先队列

优先队列的入和出与元素（数据）进的次序无关，而是由设定的优先级来决定元素的弹出次序，优先级最高的元素最先得到服务，优先级相同的元素按照其在优先队列中的顺序得到服务

实现优先队列的方式有多种，如数组、链表、平衡二叉树（如：AVL树和RB树）、堆等

堆是一种完全二叉树，可以分为小顶堆和大顶堆。小顶堆指的是堆顶元素（即树的根节点）为堆中最小值，且每一个节点的值都必须 小于等于 其孩子节点的值；反之即大顶堆

对堆的操作主要有两个，即插入或删除，且都是在顶端进行。由于堆是一个完全二叉树，删除和添加后都需要保证是完全二叉树，一般情况是先和最后一个节点进行交换，然后再进行删除和添加。

C++	priority_queue	push、top、pop、empty	系统自带	大顶堆
 
Python	heapq	heappush、heappop	系统自带	小顶堆

可以使用可自动排序的map进行替代，也能够达到减少时间复杂度的目的。如  C++(map)、 Python(SortedDict)

【c++】STL里的priority_queue用法总结: https://blog.csdn.net/xiaoquantouer/article/details/52015928

priority_queue<Type, Container, Functional>

Type为数据类型， Container为保存数据的容器，Functional为元素比较方式。

如果不写后两个参数，那么容器默认用的是vector，比较方式默认用operator<，也就是优先队列是大顶堆，队头元素最大。


数据流中的K大元素： 

	用最小堆解决heapq:  heapq.heappop， heapq.heappush
	
	取栈顶 heapq[0]
 
 
 优先队列： 优先级高的先出，insert 、del_max 、max 求最大 
		
		用堆实现
		
		Heapify 建立
		
		Heappush  
		
		Heappop
		
		Heapsort 

相关题目：
	
50     ---     求pow(x,n)  
239    ----    滑动窗口最大值（堆实现）
692    ---     前K个高频词，用了heapq 、counter 函数

	return    [heapq.heappop(pq)[1]]    for  _   in   range(k)]
 
常用库积累：

	Bisect 
	Heapq
	@lru_cache
	Collections(deque/defaultdict/Counter)

## 二分法


| 语言   | 支持二分的常用数据结构             | 找值          | lower_bound     | upper_bound  |
| ------ | ---------------------------------- | ------------- | --------------- | ------------ |
| C++    | vector  multiset/set  map/multimap | binary_search | lower_bound     | upper_bound  |
| Python | List                               | bisect_left   | **bisect**_left | bisect_right |

**C++**:

**升序序列**

lower_bound：返回第一个 >= 目标值的迭代器，找不到则返回end()。

upper_bound：返回第一个 > 目标值的迭代器，找不到则返回end()。

**降序序列**

需要重载或者目标比较器，例如greater<int >()

lower_bound：返回第一个 <= 目标值的迭代器，找不到则返回end()。

upper_bound：返回第一个 < 目标值的迭代器，找不到则返回end()。

eg: 
```C++
 // 返回第一个小于等于目标值的迭代器

**lower_bound**(vec.begin(), vec.end(), 8, greater<int>());

// 返回第一个小于目标值的迭代器

**upper_bound**(vec.begin(), vec.end(), 8, greater<int>());

 bool isFind = **binary**_**search**(vec.begin(), vec.end(), 7);

// 返回第一个大于等于目标值的迭代器

 vector<int>::iterator iter1 = **lower**_bound(vec.begin(), vec.end(), 8);

 // 返回第一个大于目标值的迭代器

 vector<int>::iterator iter2 = upper_bound(vec.begin(), vec.end(), 8);
```

**Python** 

**升序序列**

a） bisect_left (同lower_bound)：返回插入位置的索引i，使得a[:i]的所有元素都 < 目标值，a[i:]的所有元素 >=目标值；

b） bisect_right (同upper_bound)：返回插入位置的索引i，使得a[:i]的所有元素都 <= 目标值，a[i:]的所有元素 >目标值。

 降序序列：

不支持。可以先反转之后再使用上述方法模拟实现。

eg：

import bisect

point_left = bisect.bisect_left(num_list, 7)

point_right = bisect.bisect_right(num_list, 8)

 
## 字典序
 
字典序：指按照单词出现在字典的顺序进行排序的方法。先按照第一个字母以 0、1、2 … 9，a、b、c … z 等的ASCII码值顺序排列，如果第一个字母一样，那么比较第二个、第三个乃至后面的字母。如果比到最后两个单词不一样长（比如 sigh 和 sight），那么把短者排在前。

**C++** 

1. 字符串比较：类似于C语言的strcmp函数，C++标准库string类重载了大于、等于、小于等运算符，可直接用于字符串的比较（基于字典序）。例如 "apple" < "banana", "9"> "10" 。

2. 字符串序列排序：同样的，类似于C语言的qsort函数，C++标准库还提供了 sort函数，可用于对字符串序列进行排序。sort 函数默认的排序方式是字典序升序，两个参数就可以了。sort函数原型如下：

基于sort函数，可以采用不同形式的comp参数，来实现按字典序降序排序：

1）lambda表达式（其中调用string类的比较运算符实现）；2）全局函数或者静态函数；3）greater模板类对象；

也可以采用 sort 的默认形式完成升序排序，然后再调用 reverse 反转实现降序排序
 

**Python**

1.  字符串比较：Python的大于、等于、小于等运算符可直接用于比较两个字符串（基于字典序），例如 "apple" < "banana", "9" > "10"。

2.  字符串序列排序：Python库函数 sort 可用于多个字符串的排序，其背后逻辑就是利用字符串比较运算符（字典序的），默认为字典序升序排序。降序的实现方式有：

1）  sort + reverse参数  2）  sort + 比较函数
 

 ```C++
 
//原型
void sort(RandomIt first, RandomIt last);
void sort(RandomIt first, RandomIt last, Compare comp);
 
bool Cmp(const string &a, const string &b)
{
    return a > b;
}
 
sort(arr.begin(), arr.end(), [](string a, string b) { return a > b; });
// sort(arr.begin(), arr.end(), Cmp);
// sort(arr.begin(), arr.end(), greater<string>());

sort(arr.begin(), arr.end());
reverse(arr.begin(), arr.end());	

```

 
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

##   DFS、BFS
	
	BFS:层次划分
	
	DFS:回溯和剪枝
	
BFS： 层次遍历、最短路径问题

DFS： 排列、组合、分割、棋牌
 
BFS     934 最短的桥

 先深度搜索，确度一座岛的边界
 再广度搜索，查找路径

279    完全平方数  最短路径

全排列问题： 回溯

77   组合问题： n个数和K ,  选择出k个数的所有组合

306   累加数： 字符的分割  


##  差分&拓扑排序&编程规范           
             
差分：

Leetcode    1094       标记变化量 -->  求和最大值

1109  航班预订统计

122   股票买卖的最佳时机 II

差分-贪心算法

拓扑排序：
 
	210 课程表   II

	269 火星词典： 
	
	广度优先搜索：走迷宫、最短路径
             
             
## 单调栈、并查集
 
 单调栈：

	739   每日温度    

	503   下一个更多元素 II   循环数组
	 
	84   柱状图中最大的矩形    单调栈

并查集：

	547   省份数量: 无向图的联通域的个数
	
	200   岛屿数量
	
	737 句子相似性：
	
	1135  最低成本联通所有城市

	合并集合、查找集合中的元素

• 合并：把两个不相交的集合按照某种条件合并为一个集合。
• 查询：查询两个元素是否在同一个集合中。
 
 ## 滑动窗口+前缀和 & HASH
 
 滑动窗口：先right,后left


	
前缀和 + 哈希表：

	注意前缀和的0索引位置

	区域和检索--数组不可变

304    二维区域和检索--矩阵不可变

560    和为K的子数组

523   连续的子数组和
 
 
 
 
