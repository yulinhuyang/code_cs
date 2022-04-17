
RMQ/ST表/区间最值查询 ---- 模板： https://www.acwing.com/blog/content/7942/    

算法学习笔记(12): ST表： https://zhuanlan.zhihu.com/p/105439034 

秦淮岸灯火阑珊题解：https://www.acwing.com/user/myspace/solution/index/1130/

####  第一讲 基础算法

##### 快速排序：
- AcWing 786 第k个数：快排划分
```C++
1 找到分界点，q[L],q[(L+R)/2],q[R]
2 左边所有数left <=x，右边所有数right>=x
3 sL = k - L + 1, k <=sL 递归排序left, k > sL 递归排序right,k - SL 
```
**TOP K问题总结**

面试官最喜爱的TopK问题算法详解：https://zhuanlan.zhihu.com/p/76734219      
前端进阶算法10：别再说你不懂topk问题了： https://github.com/sisterAn/JavaScript-Algorithms/issues/73     

第 K 个最大（小）元素： 数组中，快排划分；数据流中，最小堆堆顶。    
最大（小） K 个数：堆     
前 K 个高频元素：堆,pair 频率，lt 347     
中位数：两个数组中位数，归并lt4；数据流，对顶堆    

归并排序：
- AcWing 787 归并排序
- AcWing 788 逆序对的数量

```C++
//归并排序
1 [L,R] ->[L,mid],[mid + 1,R]
2 递归排序[L,mid]和[mid + 1,R]
3 归并，将左右两个有序序列合并成一个序列。 
//逆序对
1 左半边内部逆序对的数量，mergesort(L,mid)
2 右半边内部逆序对的数量，mergesort(mid + 1,R)
3 比归并排序多一句：if(q[i] > q[j]) res += mid - i + 1
```


#####  二分：

AcWing 789 数的范围
AcWing 790 数的三次方根：浮点数二分模板：while(r - l> 1e-8)，边界处理 r = mid，l=mid

##### 前缀和与差分：

- AcWing 795 前缀和
```C++
一维前缀和 Si=a[1]+a[2]+...+a[i]     
sum(L,R) = a[L] + a[L+1] + ...+ a[R]，等于S[R] - S[L - 1] 
```
- AcWing 796 子矩阵的和
```C++
构造：S[i,j] = S[i-1,j] + S[i,j-1] - S[i-1,j-1] + a[i,j]     
计算(x1,y1),(x2,y2)子矩阵中数的和 = S[x2,y2] - S[x1-1,y2] - S[x2,y1-1] + S[x1-1,y1-1] 
```
- AcWing 797 差分
```C++
给定a[1],a[2],...a[n],构造差分数组b,使得a[i] = b[1] + b[2] + ...+ b[i],可以直接insert构造。            
核心差分操作：将a[L~R]全部加上C,等价于b[L] += C,b[R] -= C
```
- AcWing 798 差分矩阵
核心操作：以(x1,y1)为左上角，(x2,y2)为右下角的子矩阵中所有的数a[i][j]加上C   
```C++
S[x1][y1] += C      
S[x1][y2+1] -= C      
S[x2+1][y1] -= C      
S[x2+1][y2+1] += C  
```
通过核心操作构建差分矩阵      

##### 双指针：
- AcWing 799 最长连续不重复子序列
- AcWing 800 数组元素的目标和
双指针的时间复杂度：o(n+m)      
先写暴力算法，再看有没有单调性，如果有可以把时间复杂度降低一维     
```C++
//暴力
for(int i = 0；i < n;i++){
	for(int j=0;j<n;j++{}
}
//优化
for(int i = 0；i < n;i++ )
{
	while(j>=0 && a[i] + b[j] > 0)j--;
}
```

- AcWing 2816 判断子序列

##### 位运算：
- AcWing 801 二进制中1的个数

##### 离散化：
- AcWing 802 区间和
离散化：下标来表示原来的值，unique去重+二分查找find,缩小数的规模的范围。      
离散化，配合前缀和、线段树等。
```C++
sort(alls.begin(), alls.end()); // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

int find(int x)
{
    return lower_bound(q.begin(),q.end(),x)-q.begin();
}
```

##### 区间合并

- AcWing 803 区间合并

#### 第二讲 数据结构

##### 单链表：
- AcWing 826 单链表
头插法,head = -1，idx = 0;

##### 双链表：
- AcWing 827 双链表

##### 栈：
- AcWing 828 模拟栈
- AcWing  3302 表达式求值

##### 队列：
- AcWing 829 模拟队列

##### 单调栈：
- AcWing 830 单调栈
单调栈、单调队列和双指针都是类似的，先从暴力入手，挖掘里面的单调性，双指针利用单调性将枚举的数量从n^2变成O(n),单调栈、单调队列利用单调性将求最值的过程从O(n)变成O(1)，本来扫描一遍，现在直接拿队头就可以了。      

##### 单调队列：
- AcWing 154 滑动窗口
单调栈：左(右)边第一个比它大/小。     
单调队列：滑窗问题，不一定固定长度。     

##### KMP:
- AcWing 831 KMP字符串
KMP下标从1开始。  

##### Trie：
- AcWing 835 Trie字符串统计
- AcWing 143 最大异或对
边插入，边查找；先插入，再查询，避免空集。
```C++
//query
if(son[p][!u]){
	p = son[p][u]
	res = res * 2 + !u;
}
else {
	p = son[p][u];
	res = res * 2 + u;
}
```

##### 并查集：
- AcWing 836 合并集合
- AcWing 837 连通块中点的数量
- AcWing 240 食物链：维护距离信息
mod 2: 余1，可以吃根节点；余2，可以被根节点吃；余0，与根节点同类。 p[x] - P[y] = 1,则x吃Y。
```C++
//路径压缩并查集
int find(int x)
{
    if (p[x] != x)
    {
        int t = find(p[x]);
        d[x] += d[p[x]];
        p[x] = t;
    }
    return p[x];
}
//d[p[x]]用？推导
d[x] + d[px] = d[py]] x和y同类
d[x] + d[px] = d[py] + 1 x吃y
```

##### 堆：
- AcWing 838 堆排序
- AcWing 839 模拟堆:删除时和堆顶交换。

##### 哈希表：
- AcWing 840 模拟散列表
- AcWing 841 字符串哈希

#### 第三讲 搜索与图论

DFS:
- AcWing 842 排列数字
- AcWing 843 n-皇后问题

BFS：
- AcWing 844 走迷宫
- AcWing 845 八数码:BFS 最短路，最小步数，最小生成树，第一次搜到的，一定是最短路过去的。     
状态表示：
```C++
queue<string> q;
unordered_map<string, int> d;
```
状态转移：恢复3x3-->转移-->转成一串
```C++
int x = k / 3, y = k % 3;
for (int i = 0; i < 4; i ++ )
{
	int a = x + dx[i], b = y + dy[i];
	if (a >= 0 && a < 3 && b >= 0 && b < 3)
	{
		swap(t[a * 3 + b], t[k]);
		if (!d.count(t))
		{
			d[t] = distance + 1;
			q.push(t);
		}
		swap(t[a * 3 + b], t[k]);
	}
}
```

树与图的深度优先遍历：
- AcWing 846 树的重心

树与图的广度优先遍历：
- AcWing 847 图中点的层次

拓扑排序：
- AcWing 848 有向图的拓扑序列

Dijkstra:
- AcWing 849 Dijkstra求最短I
- AcWing 850 Dijkstra求最短II


Bellman-ford:
- AcWing 853 有边数限制的最短路

spfa：
- AcWing 851 spfa求最短路
- AcWing 852 spfa判断负环

Floyd:
- AcWing 854 Floyd求最短路

Prim:
- AcWing 858 Prim算法求最小生成树

Kruskal:
- AcWing 859 Kruskal算法求最小生成树

染色法判断二分图：
- AcWing 860 染色法判定二分图

匈牙利算法：
- AcWing 861 二分图的最大匹配

#### 数学知识

- AcWing 204 表达整数的奇怪方式：https://www.acwing.com/solution/content/3539/

- AcWing 896 最长上升子序列II：理解闫氏DP，分类依据，倒数第二个数是哪个数，考虑最后一步操作，据此划分集合。     
f[i] = max(f(k) + 1)         
暴力->优化，数组q[]来存每个长度下的最长上升子序列的最后一个数的最小值是几。         
二分查找接到哪个数后面。          
```C++
int l = 0, r = len;
while (l < r)
{
	int mid = l + r + 1 >> 1;//寻找右边界
	if (q[mid] < a[i]) l = mid;
	else r = mid - 1;
}
len = max(len, r + 1);
q[r + 1] = a[i];
```

- AcWing 899 编辑距离：    
下标从1开始，先初始化0行0列   
```C++
状态表示：
	集合：所有将a[1~i]变成b[1~j]的操作方式。
	属性：min

状态计算：考虑最后一步
	删a[i]:要a[i-1]已经和b[j]匹配了，f[i-1][j] + 1
	a[i]后增:要a[i]已经和b[j-1]匹配了，f[i][j-1] + 1
	改a[i]:要a[i-1]已经和b[j-1]匹配了，f[i-1][j-1] + 1
```

- AcWing 900 整数划分：     
完全背包问题，https://www.acwing.com/activity/content/code/content/62496/
容量n,物品体积1~n。
```C++
状态表示：
	f[i][j]集合：只从1~i中选，选法总和j。
	属性：数量
	
状态计算：
f[i][j] = f[i-1][j] + f[i][j-i]
f[j] = f[j] + f[j - i]
```


