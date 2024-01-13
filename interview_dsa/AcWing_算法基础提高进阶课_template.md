参考：

Acwing 技术分享： https://www.acwing.com/blog/      

模板级补全——唤醒词列表：https://www.acwing.com/blog/content/5224/ 

算法基础课：https://www.acwing.com/activity/content/activity_person/content/4102/1/    

算法进阶指南：https://www.acwing.com/activity/content/activity_person/content/1349/9/   

算法提高课：https://www.acwing.com/activity/content/activity_person/content/9226/1/     

# 目录

0x00 基本算法
- int lowbit - 找末尾1
- bitMap
- add - 高精度加法
- sub - 高精度减法
- mul - 高精度乘低精度
- div - 高精度除以低精度
- 整数二分/浮点数二分
- 快速排序/快选
- void merge_sort - 归并排序
- 计数排序/桶排序
- 区间合并
- RMQ 区间最值
- void manacher - 马拉车算法

0x10 基本数据结构
- void insert - Trie 插入

0x20 搜索
- 深度优先遍历 (DFS)
- 宽度优先遍历 (BFS)
- Astar：图遍历 + BFS
- simulate_anneal - 模拟退火

0x30 数学
- bool is_prime - 判定质数
- void get_primes - 线性筛质数
- 朴素筛法求素数
- int phi - 欧拉函数
- get_eulers - 线性筛欧拉函数
- int qmi - 快速幂
- int quick_power - 快速幂
- int gcd - 欧几里得算法
- LL gcd - 欧几里得算法
- int exgcd - 扩展欧几里得算法
- gauss - 高斯消元（浮点值）
- gauss - 高斯消元（浮点值）
- gauss - 高斯消元（布尔值）
- gauss - 高斯消元（布尔值）
- int lucas - Lucas定理
- 分解质因数法求组合数
- int bsgs - BSGS
- void fft - FFT

0x40 数据结构进阶
- int find - 并查集
- int lowbit - 树状数组
- void build - 线段树
- void build - AC自动机
- void splay - splay
- void splay - 动态树
- bool dfs - DLX重复覆盖(Dancing Links)
- bool dfs - DLX精确覆盖
- int merge - 左偏树
- void get_sa - 后缀数组
- void extend - 后缀自动机

0x50 动态规划  
- LCS/LIS 模型
- 背包DP
- 状态机模型
- 状态压缩DP
- 区间DP
- 树形DP
- 单调队列优化DP

0x60 图论
- void add - 加边，不带权
- void add - 加边，带权
- int h[N] - 邻接表不带权
- int h[N] - 邻接表dijkstra
- 朴素dijkstra算法
- 堆优化版dijkstra
- Bellman-Ford算法
- int h[N]-邻接表spfa
- int spfa - 最短路
- void spfa - 最短路
- bool spfa - 判负环
- floyd算法 
- 朴素版prim算法
- Kruskal算法
- int lca - 倍增求LCA
- int h[N] - 邻接表tarjan-scc
- tarjan - 有向图强连通分量
- int h[N] - 邻接表tarjan-v-dcc
- tarjan - v-无向图双连通分量
- int h[N] - 邻接表tarjan-e-dcc
- tarjan - e-无向图双连通分量
- 染色法判别二分图
- bool find - 匈牙利算法
- void topsort - 拓扑排序
- bool topsort - 拓扑排序
- void add - 加边，最大流
- int h[N]-邻接表最大流
- int dinic - 最大流
- void add - 加边，费用流
- int h[N]-邻接表费用流
- void cost_flow - 费用流
- int cost_flow - 费用流



0x70 计算几何
- int sign - 计算几何常用函数
- void andrew - 求凸包
- half- - 求半平面交
- double simpson - 辛普森积分


# 模板 

## 0x00 基本算法

#### 位运算 

模板题 AcWing 801. 二进制中1的个数

求n的第k位数字: n >> k & 1

返回n的最后一位1：lowbit(n) = n & -n

**bitmap 模板**

```cpp
#include <limits.h>    /* for CHAR_BIT */
#include <stdint.h>   /* for uint32_t */

typedef uint32_t word_t;
enum { BITS_PER_WORD = sizeof(word_t) * CHAR_BIT };
#define WORD_OFFSET(b) ((b) / BITS_PER_WORD)
#define BIT_OFFSET(b)  ((b) % BITS_PER_WORD)

void set_bit(word_t *words, int n) { 
    words[WORD_OFFSET(n)] |= (1 << BIT_OFFSET(n));
}

void clear_bit(word_t *words, int n) {
    words[WORD_OFFSET(n)] &= ~(1 << BIT_OFFSET(n)); 
}

int get_bit(word_t *words, int n) {
    word_t bit = words[WORD_OFFSET(n)] & (1 << BIT_OFFSET(n));
    return bit != 0; 
}
```

#### 高精度运算

**高精度加法**

模板题 AcWing 791. 高精度加法

```cpp
// C = A + B, A >= 0, B >= 0
vector<int> add(vector<int> &A, vector<int> &B)
{
    if (A.size() < B.size()) return add(B, A);

    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i ++ )
    {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    
    if (t) C.push_back(t);
    return C;
}
```

**高精度减法** 

模板题 AcWing 792. 高精度减法

```cpp
// C = A - B, 满足A >= B, A >= 0, B >= 0
vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ )
    {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

```

**高精度乘低精度**

模板题 AcWing 793. 高精度乘法
```cpp
// C = A * b, A >= 0, b >= 0
vector<int> mul(vector<int> &A, int b)
{
    vector<int> C;

    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ )
    {
        if (i < A.size()) t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }
    
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    
    return C;
}
```

**高精度除以低精度**

模板题 AcWing 794. 高精度除法
```cpp
// A / b = C - r, A >= 0, b > 0
vector<int> div(vector<int> &A, int b, int &r)
{
    vector<int> C;
    r = 0;
    for (int i = A.size() - 1; i >= 0; i -- )
    {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
```

**vector 比较大小模板**

```cpp
vector<int> max_vec(vector<int> a, vector<int> b) {
    if (a.size() > b.size()) return a;
    if (a.size() < b.size()) return b;
    if (vector<int>(a.rbegin(), a.rend()) > vector<int>(b.rbegin(), b.rend())) return a;
    return b;
}
```

#### 前缀和与差分 

一维前缀和 —— 模板题 AcWing 795. 前缀和

S[i] = a[1] + a[2] + - a[i]

a[l] + - + a[r] = S[r] - S[l - 1]

二维前缀和 —— 模板题 AcWing 796. 子矩阵的和

S[i, j] = 第i行j列格子左上部分所有元素的和
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]

一维差分 —— 模板题 AcWing 797. 差分

给区间[l, r]中的每个数加上c：B[l] += c, B[r + 1] -= c

二维差分 —— 模板题 AcWing 798. 差分矩阵

给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：

S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c

构建差分数组 -> 记录操作变化 -> 恢复变化后的数组

差分和前缀和互为逆运算，有效索引需要从1开始


```cpp
//AcWing797 差分
//b是a的差分，对a所有操作包括读入，都直接施加在b上。
int a[N], b[N];

void insert(int l, int r, int c) {
    b[l] += c;
    b[r + 1] -= c;
}

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        insert(i, i, a[i]);
    }

    int l, r, c;
    while (m--) {
        cin >> l >> r >> c;
        insert(l, r, c);
    }
    for (int i = 1; i <= n; i++) {
        a[i] = a[i - 1] + b[i];
        cout << a[i] << " ";
    }
}
```


列方向压缩前缀和：AcWing126 最大的和  

```cpp
for (int i = 1; i <= n; i ++ )
    for (int j = 1; j <= n; j ++ )
    {
        cin >> g[i][j];
        g[i][j] += g[i - 1][j];
    }

```

#### 二分法

**整数二分算法模板** 

模板题 AcWing 789. 数的范围

```cpp
bool check(int x) {/* - */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

**浮点数二分算法模板**

模板题 AcWing 790. 数的三次方根

```cpp
bool check(double x) {/* - */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

#### 双指针算法 

模板题 AcWIng 799. 最长连续不重复子序列

```cpp
//AcWing 799 最长连续不重复子序列
for (int i = 0, j = 0; i < n; i++) {
	cin >> a[i];
	s[a[i]]++;
	while (j < i && s[a[i]] > 1) {
		s[a[j]]--;
		j++;
	}
	ans = max(ans, i - j + 1);
}
```

AcWing 800. 数组元素的目标和
```cpp
for (int i = 0, j = 0; i < n; i ++ )
{
	while (j < i && check(i, j)) j ++ ;

	// 具体问题的逻辑
}
```
常见问题分类：
    (1) 对于一个序列，用两个指针维护一段区间
    (2) 对于两个序列，维护某种次序，比如归并排序中合并两个有序序列的操作


#### 排序

快排是先合后分(递归)，归并是先分(递归)后合

**快速排序(快选)算法模板** 

模板题 AcWing 785. 快速排序

```cpp
void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}
```

```C++
//快选模板
int quick_select(int q[], int l, int r, int k) {
    // 当数组为空时，quick_sort(q, 0, len(q) - 1)中l = 0, r = -1, 会出现 l > r的情况
    // 除了 一开始 需要判断 l > r, 以后 只需要 判断 l == r 即可
    // 因为 快速选择 传进来的 序列 最少有一个元素, 所以 一般 len(q) >= 1, 不用判断 l > r 也可以
    // 除了 len(q) == 0 会造成 一开始 l > r 之外，以后return 的时候 肯定有 l == r
    if (l >= r) return q[l];

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j) // i,j 两指针相遇后，只有两种情况，(1) i == j; (2) i > j
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }

    if (j - l + 1 >= k) return quick_select(q, l, j, k);
    else return quick_select(q, j + 1, r, k - (j - l + 1));
}
```


**归并排序(逆序对)算法模板**

模板题 AcWing 787. 归并排序
```cpp
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int mid = l + r >> 1;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);
    
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];
    
    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];
    
    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```

AcWing 107 超快速排序

```cpp
while (i <= mid && j <= r) {
	if (a[i] < a[j]) tmp[k++] = a[i++];
	else {
		tmp[k++] = a[j++];
		ans += mid - i + 1; //归并变形，计算逆序对数量
	}
}
```

**计数排序模板**

```cpp
void CountSort(int a[], int n){
    //遍历数组求得最大值和最小值
    int minval = a[0], maxval = a[0];
    for(int i = 0; i < n; i ++){ 
        minval = min(minval, a[i]);
        maxval = max(maxval, a[i]);
    }

    int d = maxval - minval + 1;// 计数数组的实际长度 
    int cnt[d] = {0};//根据最大值开辟新数组空间 

    //统计原数组中元素出现的次数
    for(int i = 0; i < n; i ++) cnt[a[i] - minval] ++;//将元素映射到a[0...d-1] 

    int sum = 0;
    for(int i = 0; i < d; i ++){//本质为前缀和数组,用于求位次 
        sum += cnt[i];//此处的cnt既为元素又为之前的元素和,即cnt[i] = cnt[i] + cnt[i - 1] 
        cnt[i] = sum;// 比如, cnt[5] = 3,表示分数95, 排名第 3 
    } 

    int sortArray[d];//sortArray[]存元素真实序列 
    for(int i = n - 1; i >= 0; i --){//将原数组元素从后往前遍历 
        sortArray[cnt[a[i] - minval] - 1] = a[i];
        cnt[a[i] - minval] --;
    }

    //将排序后的序列赋给原数组
    for(int i = 0, k = 0; i < d; i ++){
        data[k ++] = sortArray[i];
    }
}
```

**桶排序模板**
```cpp
void BucketSort(int a[], int n){
    int minval = a[0], maxval = a[0];
    for(int i = 0; i < n; i ++){//寻找原序列数组元素的最大值和最小值 
        minval = min(minval, a[i]);
        maxval = max(maxval, a[i]);
    }

    int bnum = 10;//桶中元素个数 
    int m = (maxval - minval) / bnum + 1;//桶的个数 
    vector< vector<int> > bucket(m);

    //收集,将元素入相应的桶中. 减偏移量是为了将元素映射到更小的区间内,省内存 
    for(int i = 0; i < n; i ++) bucket[(a[i] - minval) / bnum].push_back(a[i]);

    //将桶内元素排序 
    for(int i = 0; i < m; i ++) sort(bucket.begin(), bucket.end());

    //收集, 将各个桶中的元素收集到一起 
    for(int i = 0, k = 0; i < m; i ++){
        for(int j = 0; j < bucket[i].size(); j ++){
            data[k ++] = bucket[i][j];
        }
    }
}

```


#### 中位数

AcWing 104 货仓选址：排序+中位数
AcWing 122 糖果传递：

均分纸牌问题: ∑ i=1~n−1 |Si|，其中 Si 为 Ai 的前缀和，即Ai = ci - T/M，Si是第i个人要给第i+1个人的牌的数量,ci减去均值的前缀和。   
环形均分纸牌问题：破环成链，一定存在一个最优解方案，环上有相邻的两个人之间没有发生交换。   
可以直接枚举断点K的位置，再做一遍线性纸牌均分。断点Sk可以直接取排序后的中位数即可，然后使用货仓选择模型计算最小距离 ∑i=1 n |Si−Sk|。  
减去均值的前缀和 + 货仓中位数计算距离  

```cpp
LL sum = 0;
for (int i = 1; i <= n; i ++ )
{
    scanf("%lld", &a[i]);
    sum += a[i];
}
sum /= n;
for (int i = n; i > 1; i -- )
{
    a[i] = a[i] - sum + a[i + 1];
}
a[1] = 0;
sort(a + 1, a + n + 1);

LL res = 0;
for (int i = 1; i <= n; i ++ ) res += abs(a[i] - a[(n + 1) / 2]);
```


#### 离散化 

模板题 AcWing 802. 区间和

```cpp
vector<int> alls; // 存储所有待离散化的值
sort(alls.begin(), alls.end()); // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

// 二分求出x对应的离散化的值
int find(int x) // 找到第一个大于等于x的位置
{
    int l = 0, r = alls.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1; // 映射到1, 2, -n
}
//stl find
int find(int x) {
    return lower_bound(cinema + 1, cinema + 1 + k, x) - (cinema + 1);
}
```

#### 倍增模板

AcWing 109 天才ACM 

```cpp
while (start <= n) {
	int step = 1;
	while (step) {
		//start到end是已经排好序的
		if (end + step <= n && check(start, end, end + step)) {
			end += step;
			step *= 2;
			if (end > n) break;
			//已经校验完成的区间拷贝回去
			for (int i = start; i <= end; i++) {
				b[i] = t[i - start];
			}
		} else {
			step /= 2;
		}
	}
	start = end + 1;
	ans++;
}
```


#### 贪心模板

贪心问题一般都需要排序，邻项交换技巧，需要制定排序规则，根据区间起点、终点、两数乘积、两数(字符串)之和等

（1） 区间合并

模板题 AcWing 803. 区间合并

```cpp
// 将所有存在交集的区间合并
typedef pair<int,int> PII;
void merge(vector<PII> &segs)
{
    vector<PII> res;

    sort(segs.begin(), segs.end());
    
    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);
    
    if (st != -2e9) res.push_back({st, ed});
    
    segs = res;
}
```

(2) 区间选点模板：AcWing 905. 区间选点

```cpp
struct Range {
    int l, r;
    bool operator<(const Range &w) const {
        return r < w.r;
    }
} range[N];

//r递增排序
sort(range, range + n);
int res = 0, ed = -2e9;
for (int i = 0; i < n; i++) {
	if (ed < range[i].l) {
		ed = range[i].r;
		res++;
	}
}
```

(3) 区间覆盖模板: AcWing 907. 区间覆盖

```cpp
struct Range {
    int l, r;

    bool operator<(const Range &w) const {
        return l < w.l;
    }
} range[N];


//l升序
sort(range, range + n);
int res = 0;
bool success = false;
for (int i = 0; i < n; i++) {
	int j = i, r = -2e9;
	//贪心：寻找最靠后的符合条件的区间
	while (j < n && range[j].l <= st) {
		r = max(r, range[j].r);
		j++;
	}
	if (r < st) {
		res = -1;
		break;
	}
	res++;
	if (r >= ed) {
		success = true;
		break;
	}
	st = r;
	i = j - 1;
}
```

(4) 排序不等式 AcWing 913. 排队打水

排序 + 邻项交换

```cpp
// a1(n-1) + a2(n-2) ..最小，等价于 a1*1 +a2*2最大,则ai降序排列
cin >> n;
for (int i = 0; i < n; i++) {
	cin >> a[i];
}
sort(a, a + n);
reverse(a, a + n);
int res = 0;
for (int i = 0; i < n; i++) res += i * a[i];
```

```cpp
//国王不参与排序
sort(nums + 1, nums + n + 1, [](PII & a, PII & b) {
	return a.first * a.second < b.first * b.second;
});
```

#### RMQ区间最值

AcWing 1273. 天才的记忆

```cpp
#include <iostream>
#include <cstring>
#include <cmath>

using namespace std;

//RMQ 区间最大值
//st表、跳表、倍增动态规划
//f[i,j]表示从i开始，长度为2^j的区间中的最大值是多少
//f[i,j] = max{f[i, j-1],f[i + 2^(j-1), j-1]}

const int N = 200010, M = 18;
int n,m;
int w[N];
int f[N][M];

//预处理 f[i][j]
void init(){
    for (int j = 0; j < M; j++){
        for (int i = 1; i + (1 << j) - 1 <= n; i++){
            if (!j) f[i][j] = w[i];
            else f[i][j] = max(f[i][j - 1], f[i + (1 <<j - 1)][j - 1]);
        }
    }
}

int query(int l,int r){
    int len = r - l + 1;
    int k = log(len) / log(2);
    return max(f[l][k], f[r - (1 << k) + 1][k]);
}

int main()
{
    cin >> n;
    for(int i = 1;i <= n;i++){
        cin >> w[i];
    }
    
    //预处理
    init();
    cin >> m;
    while (m--)
    {
        int l,r;
        cin >> l >> r;
        cout << query(l,r) << endl;
    }
    
    return 0;
}
```


#### 马拉车算法

```C++
void init()  // a[]为原串，b[]为插入'#'后的新串
{
    int k = 0;
    b[k ++ ] = '$', b[k ++ ] = '#';
    for (int i = 0; i < n; i ++ ) b[k ++ ] = a[i], b[k ++ ] = '#';
    b[k ++ ] = '^';
    n = k;
}

void manacher()  // 马拉车算法，b[]为插入'#'后的新串
{
    int mr = 0, mid;
    for (int i = 1; i < n; i ++ )
    {
        if (i < mr) p[i] = min(p[mid * 2 - i], mr - i);
        else p[i] = 1;
        while (b[i - p[i]] == b[i + p[i]]) p[i] ++ ;
        if (i + p[i] > mr)
        {
            mr = i + p[i];
            mid = i;
        }
    }
}
```


## 0x10 基本数据结构

#### 链表

**单链表**

模板题 AcWing 826. 单链表

```cpp
// head存储链表头，e[]存储节点的值，ne[]存储节点的next指针，idx表示当前用到了哪个节点
int head, e[N], ne[N], idx;

// 初始化
void init()
{
    head = -1;
    idx = 0;
}

// 在链表头插入一个数a
void insert(int a)
{
    e[idx] = a, ne[idx] = head, head = idx ++ ;
}

// 将头结点删除，需要保证头结点存在
void remove()
{
    head = ne[head];
}
```

**双链表**

模板题 AcWing 827. 双链表

```cpp
// e[]表示节点的值，l[]表示节点的左指针，r[]表示节点的右指针，idx表示当前用到了哪个节点
int e[N], l[N], r[N], idx;

// 初始化
void init()
{
    //0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;
}

// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}

// 删除节点a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}
```


#### 栈

模板题 AcWing 828. 模拟栈

```cpp
// tt表示栈顶
int stk[N], tt = 0;

// 向栈顶插入一个数
stk[ ++ tt] = x;

// 从栈顶弹出一个数
tt -- ;

// 栈顶的值
stk[tt];

// 判断栈是否为空
if (tt > 0)
{

}
```

**单调栈**

模板题 AcWing 830. 单调栈

常见模型：找出每个数左边离它最近的比它大/小的数
```cpp
int tt = 0;
for (int i = 1; i <= n; i ++ )
{
    while (tt && check(stk[tt], i)) tt -- ;
    stk[ ++ tt] = i;
}
```
```cpp
//stl
left.emplace(0);
h[0] = h[n + 1] = -1; //哨兵
//单调上升栈，寻找左右边界
for (int i = 1; i <= n; i++) {
    while (left.size() && h[left.top()] >= h[i]) left.pop();
    l[i] = left.top();
    left.emplace(i);
}

right.emplace(n + 1);
for (int i = n; i >= 1; i--) {
    while (right.size() && h[right.top()] >= h[i]) right.pop();
    r[i] = right.top();
    right.emplace(i);
}

```

#### 队列

模板题 AcWing 829. 模拟队列

**1. 普通队列：**

```cpp
// hh 表示队头，tt表示队尾
int q[N], hh = 0, tt = -1;

// 向队尾插入一个数
q[ ++ tt] = x;

// 从队头弹出一个数
hh ++ ;

// 队头的值
q[hh];

// 判断队列是否为空
if (hh <= tt)
{

}
```
**2. 循环队列**

```cpp
// hh 表示队头，tt表示队尾的后一个位置
int q[N], hh = 0, tt = 0;

// 向队尾插入一个数
q[tt ++ ] = x;
if (tt == N) tt = 0;

// 从队头弹出一个数
hh ++ ;
if (hh == N) hh = 0;

// 队头的值
q[hh];

// 判断队列是否为空
if (hh != tt)
{

}
```


**单调队列**

模板题 AcWing 154. 滑动窗口

常见模型：找出滑动窗口中的最大值/最小值

```cpp
int hh = 0, tt = -1;
for (int i = 0; i < n; i ++ )
{
    while (hh <= tt && check_out(q[hh])) hh ++ ;  // 判断队头是否滑出窗口
    while (hh <= tt && check(q[tt], i)) tt -- ;
    q[ ++ tt] = i;
}
```
```cpp
//stl
deque<int> q;
for (int i = 0; i < n; i++) {
    if (q.size() && i - q.front() >= k) q.pop_front();
    //最小值，单调上升栈
    while (q.size() && a[q.back()] >= a[i]) q.pop_back();
    q.push_back(i);
    if (i >= k - 1) cout << a[q.front()] << " ";
}
```

#### 哈希 hash

**一般哈希** 

模板题 AcWing 840. 模拟散列表

(1) 拉链法

```cpp
    int h[N], e[N], ne[N], idx;

    // 向哈希表中插入一个数
    void insert(int x)
    {
        int k = (x % N + N) % N;
        e[idx] = x;
        ne[idx] = h[k];
        h[k] = idx ++ ;
    }

    // 在哈希表中查询某个数是否存在
    bool find(int x)
    {
        int k = (x % N + N) % N;
        for (int i = h[k]; i != -1; i = ne[i])
            if (e[i] == x)
                return true;

        return false;
    }
```

(2) 开放寻址法

```cpp
    int h[N];

    // 如果x在哈希表中，返回x的下标；如果x不在哈希表中，返回x应该插入的位置
    int find(int x)
    {
        int t = (x % N + N) % N;
        while (h[t] != null && h[t] != x)
        {
            t ++ ;
            if (t == N) t = 0;
        }
        return t;
    }
```

**字符串哈希**

模板题 AcWing 841. 字符串哈希

核心思想：将字符串看成P进制数，P的经验值是131或13331，取这两个值的冲突概率低

小技巧：取模的数用2^64，这样直接用unsigned long long存储，溢出的结果就是取模的结果

```cpp
typedef unsigned long long ULL;
ULL h[N], p[N]; // h[k]存储字符串前k个字母的哈希值, p[k]存储 P^k mod 2^64

// 初始化
p[0] = 1;
for (int i = 1; i <= n; i ++ )
{
    h[i] = h[i - 1] * P + str[i];
    p[i] = p[i - 1] * P;
}

// 计算子串 str[l ~ r] 的哈希值
ULL get(int l, int r)
{
    return h[r] - h[l - 1] * p[r - l + 1];
}

```

**字符串最小表示**

```cpp
int get_min(char str[])
{
    int i = 0, j = 1;
    while (i < n && j < n)
    {
        int k = 0;
        while (k < n && str[i + k] == str[j + k]) k ++ ;
        if (k == n) break;
        if (str[i + k] > str[j + k]) i += k + 1;
        else j += k + 1;
        if (i == j) i ++ ;
    }
    int res = min(i, j);
    str[res + n] = 0;
    return res;
}
```

#### KMP 

模板题 AcWing 831. KMP字符串

Next[i]表示模式串P中以i（下标从 1 开始）结尾的真后缀能匹配 P 的前缀的最大长度。

```cpp
// s[]是长文本，p[]是模式串，n是s的长度，m是p的长度
求模式串的Next数组：
for (int i = 2, j = 0; i <= m; i ++ )
{
    while (j && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j ++ ;
    ne[i] = j;
}

// 匹配
for (int i = 1, j = 0; i <= n; i ++ )
{
    while (j && s[i] != p[j + 1]) j = ne[j];
    if (s[i] == p[j + 1]) j ++ ;
    if (j == m)
    {
        j = ne[j];
        // 匹配成功后的逻辑
    }
}
```

**计算循环节**

对于某一个字符串S[1 ~ i], 在它众多的next[i]的候选中，如果存在一个next[i],使得i%(i-next[i]) == 0,那么S[1~(i-next[i])]可以为S[1~i]的循环元，而i/(i-next[i])是它的循环次数K。

```cpp
void get_next()
{
    for (int i = 2, j = 0; i <= n; i ++ )
    {
        while (j && str[i] != str[j + 1]) j = nxt[j];
        if (str[i] == str[j + 1]) j ++ ;
        nxt[i] = j;
    }
}

int main()
{
    int T = 1;
    while (scanf("%d", &n), n)
    {
        scanf("%s", str + 1);

        get_next();

        printf("Test case #%d\n", T ++ );
        for (int i = 2; i <= n; i ++ )
        {
            int t = i - nxt[i];
            if (i > t && i % t == 0) printf("%d %d\n", i, i / t);
        }
        puts("");
    }

    return 0;
}		   
```
 

#### Trie树 

变形10进制、二进制

模板题 AcWing 835. Trie字符串统计

```cpp
int son[N][26], cnt[N], idx;
// 0号点既是根节点，又是空节点
// son[][]存储树中每个节点的子节点
// cnt[]存储以每个节点结尾的单词数量

// 插入一个字符串
void insert(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
    cnt[p] ++ ;
}

// 查询字符串出现的次数
int query(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}
```
#### 堆 

模板题 AcWing 838. 堆排序, AcWing 839. 模拟堆

```cpp
// h[N]存储堆中的值, h[1]是堆顶，x的左儿子是2x, 右儿子是2x + 1
// ph[k]存储第k个插入的点在堆中的位置
// hp[k]存储堆中下标是k的点是第几个插入的
int h[N], ph[N], hp[N], size;

// 交换两个点，及其映射关系
void heap_swap(int a, int b)
{
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}

void down(int u)
{
    int t = u;
    if (u * 2 <= size && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t)
    {
        heap_swap(u, t);
        down(t);
    }
}

void up(int u)
{
    while (u / 2 && h[u] < h[u / 2])
    {
        heap_swap(u, u / 2);
        u >>= 1;
    }
}

// O(n)建堆
for (int i = n / 2; i; i -- ) down(i);
```

**n个最小值问题，礼物问题**

AcWing 146 序列: m个序列的组合的n个最小值问题，礼物问题

```cpp
//合并a和b
priority_queue<PII, vector<PII>, greater<PII>> heap;
for (int i = 0; i < n; i++) {
	heap.push({a[0] + b[i], 0});
}
for (int i = 0; i < n; i++) {
	auto t = heap.top();
	heap.pop();
	c[i] = t.first;
	heap.push({t.first+ a[t.second + 1]  - a[t.second] , t.second + 1});
}
memcpy(a, c, 4 * n);

```
**数据备份模板**

```cpp
d[p] = d[left] + d[right] - d[p];
S.insert({d[p], p});
```

**对顶堆模板**

相同点：down.top是第i小的数/中位数

对比动态中位数：优先加入大根堆down，然后根据数量关系(max_heap.size() > min_heap.size() + 1)和平衡关系(min_heap.top() < max_heap.top())进行调整。

```cpp
priority_queue<int> max_heap;
priority_queue<int, vector<int>, greater<int>> min_heap;
max_heap.push(t);
if (min_heap.size() && min_heap.top() < max_heap.top())
{
	auto a = min_heap.top(), b = max_heap.top();
	min_heap.pop(), max_heap.pop();
	min_heap.push(b), max_heap.push(a);
}
if (max_heap.size() > min_heap.size() + 1)
{
	min_heap.push(max_heap.top());
	max_heap.pop();
}
```

**huffman 树模板**

AcWing149 荷马史诗: 

```cpp
//填充0
while ((n - 1) % (m - 1)) {
	heap.push({0ll, 0});
	n++;
}

LL sum = 0;
int depth = 0;
for (int i = 0; i < m; i ++ )
{
	sum += heap.top().first;
	depth = max(depth, heap.top().second);//优先考虑深度最小的，靠近根节点进行合并
	heap.pop();
}
res += sum;
heap.push({sum, depth + 1});
```

#### C++ STL简介

vector, 变长数组，倍增的思想

    size()  返回元素个数
    empty()  返回是否为空
    clear()  清空
    front()/back()
    push_back()/pop_back()
    begin()/end()
    []
    支持比较运算，按字典序

pair<int, int>

    first, 第一个元素
    second, 第二个元素
    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）

string，字符串

    size()/length()  返回字符串长度
    empty()
    clear()
    substr(起始下标，(子串长度))  返回子串
    c_str()  返回字符串所在字符数组的起始地址

queue, 队列

    size()
    empty()
    push()  向队尾插入一个元素
    front()  返回队头元素
    back()  返回队尾元素
    pop()  弹出队头元素

priority_queue, 优先队列，默认是大根堆

    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;

stack, 栈

    size()
    empty()
    push()  向栈顶插入一个元素
    top()  返回栈顶元素
    pop()  弹出栈顶元素

deque, 双端队列

    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []

set, map, multiset, multimap, 基于平衡二叉树（红黑树），动态维护有序序列

    size()
    empty()
    clear()
    begin()/end()
    ++, -- 返回前驱和后继，时间复杂度 O(logn)
    
    set/multiset
    
        insert()  插入一个数
        find()  查找一个数
        count()  返回某一个数的个数
        erase()
            (1) 输入是一个数x，删除所有x   O(k + logn)
            (2) 输入一个迭代器，删除这个迭代器
        lower_bound()/upper_bound()
            lower_bound(x)  返回大于等于x的最小的数的迭代器
            upper_bound(x)  返回大于x的最小的数的迭代器
    
    map/multimap
    
        insert()  插入的数是一个pair
        erase()  输入的参数是pair或者迭代器
        find()
        []  注意multimap不支持此操作。 时间复杂度是 O(logn)
        lower_bound()/upper_bound()

unordered_set, unordered_map, unordered_multiset, unordered_multimap, 哈希表

    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--

bitset, 圧位

    bitset<10000> s;
    ~, &, |, ^
    >>, <<
    ==, !=
    []
    
    count()  返回有多少个1
    
    any()  判断是否至少有一个1
    none()  判断是否全为0
    
    set()  把所有位置成1
    set(k, v)  将第k位变成v
    reset()  把所有位变成0
    flip()  等价于~
    flip(k) 把第k位取反


## 0x20 搜索

#### 树与图的存储

树是一种特殊的图，与图的存储方式相同。
对于无向图中的边ab，存储两条有向边a->b, b->a。
因此我们可以只考虑有向图的存储。

(1) 邻接矩阵：g[a][b] 存储边a->b

(2) 邻接表：

```cpp
// 对于每个点k，开一个单链表，存储k所有可以走到的点。h[k]存储这个单链表的头结点
int h[N], e[N], ne[N], idx;

// 添加一条边a->b
void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

// 初始化
idx = 0;
memset(h, -1, sizeof h);
```

#### 树与图的遍历 

时间复杂度 O(n+m)O(n+m), nn 表示点数，mm 表示边数

#####  深度优先遍历DFS

(0) 模板题 AcWing 846. 树的重心

```cpp
int dfs(int u)
{
    st[u] = true; // st[u] 表示点u已经被遍历过

    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j]) dfs(j);
    }
}
```

(1) DFS 连通性模型

AcWing 1112. 迷宫

```cpp
//dfs 可行性
bool dfs(int x, int y) {
    //截止条件
    if(g[x][y] == '#') return false;  //入口可能是不能通行的，如果入参数据进行判断可能也存在问题，则只能作为截止判断条件，不能作为循环判断条件
    if (x == xb && y == yb) return true;
    st[x][y] = true;

    for (int i = 0; i < 4; i++) {
        int a = x + dx[i], b = y + dy[i];
        //循环条件
        if (a < 0 || a >= n || b < 0 || b >= n) continue;
        if (st[a][b]) continue;
        if (dfs(a, b)) return true;
    }

    return false;
}
```

(2) memo 存储

记忆化dfs,memo,既当visit又存储值；在DFS搜索回溯之后，得到返回的结果

```cpp
//AcWing 901. 滑雪
int dfs(int i, int j) {
    //memo 既当visit,又存储值
    if (memo[i][j] != -1) return memo[i][j];
    //最小是1
    memo[i][j] = 1;
    for (int k = 0; k < 4; k++) {
        int newX = i + dx[k], newY = j + dy[k];
        if (newX >= 1 && newX <= n && newY >= 1 && newY <= m && g[newX][newY] < g[i][j]) {
            memo[i][j] = max(memo[i][j], dfs(newX, newY) + 1);
        }
    }
    return memo[i][j];
}

int dfs(int i, int j) {
    //memo 既当visit,又存储值
    if (memo[i][j] != -1) return memo[i][j];
    //最小是1
    memo[i][j] = 1;
    for (int k = 0; k < 4; k++) {
        int newX = i + dx[k], newY = j + dy[k];
        if (newX >= 1 && newX <= n && newY >= 1 && newY <= m && g[newX][newY] < g[i][j]) {
            memo[i][j] = max(memo[i][j], dfs(newX, newY) + 1);
        }
    }
    return memo[i][j];
}
```

(3) 回溯标记法

```cpp
//AcWing 843. n-皇后问题
char g[N][N];
bool col[N], dg[N * 2], udg[N * 2]; // dfs 行，列和主副对角线的标记法
int n;

void dfs(int u) {
    if (u == n) {
        for (int i = 0; i < n; i++) cout << g[i] << endl;
        cout << endl;
    }

    for (int i = 0; i < n; i++) {
        if (!col[i] && !dg[u + i] && !udg[n + u - i]) {
            col[i] = dg[u + i] = udg[n + u - i] = true;
            g[u][i] = 'Q';
            dfs(u + 1);
            g[u][i] = '.';
            col[i] = dg[u + i] = udg[n + u - i] = false;
        }
    }

}
```

(4) 剪枝

优化搜索顺序、排除等效冗余、可行性剪枝（上下界）、最优性剪枝、记忆化(v memo)

```cpp
//AcWing167 木棒

bool dfs(int u, int cur, int start)
{
    if (u * length == sum) return true; //如果总长度到达了,返回true
    if (cur == length) return dfs(u + 1, 0, 0);

    for (int i = start; i < n; i ++ )
    {
        if (st[i] || cur + w[i] > length) continue;

        st[i] = true;
        if (dfs(u, cur + w[i], i + 1)) return true;
        st[i] = false;

        if (!cur || cur + w[i] == length) return false; //如果第一根失败了或者最后一根失败了,就一定失败

        int j = i;
        while (j < n && w[j] == w[i]) j ++ ; //如果i失败了,那么长度跟i一样的棍子也一定失败
        i = j - 1;
    }

    return false; //枚举完了还没有成功,就返回失败
}
```




##### 宽度优先遍历BFS

1  Flood Fill (BFS)： 四连通、八连通遍历

2  最短路BFS: 问题只计最少步数，等价于在边权为1的图上求最短路。 使用普通的BFS,时间复杂度是O(n),每个状态只访问(入队)一次。第一次入队时即为该状态的最少步数。

3  多源BFS: 0-1矩阵距离问题, 超级虚拟源点，第一次访问即最短

4  双端队列广搜：问题每次扩展的代价是0或1，等价于在边权只有0和1的图上求最短路, 时间复杂度是O(N), 如果这条分支边权为0，则从队首入队，否则从队尾入队。每个状态被更新(入队)多次，只扩展一次，第一次出队时即为该状态的最小代价。

5  双向广搜：密码锁问题、8数码问题、单词接龙


(0)  模板题 AcWing 847. 图中点的层次

```cpp
queue<int> q;
st[1] = true; // 表示1号点已经被遍历过
q.push(1);

while (q.size())
{
    int t = q.front();
    q.pop();

    for (int i = h[t]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true; // 表示点j已经被遍历过
            q.push(j);
        }
    }
}
```	

(1) Flood Fill (BFS)

AcWing 1097. 池塘计数: 8连通 遍历

```cpp
void bfs(int sx,int sy)
{
    int hh = 0, tt = 0;
    q[0] = {sx,sy};
    st[sx][sy] = true;

    while (hh <= tt) {
        auto t = q[hh++];

        for (int i = t.x - 1; i <= t.x + 1; i++) {
            for (int j = t.y - 1; j <= t.y + 1; j++) {
                if (i == t.x && j == t.y) continue;
                if (i < 0 || i >= n || j < 0 || j >= m) continue;
                if (g[i][j] == '.' || st[i][j]) continue;
                q[++tt] = {i, j};
                st[i][j] = true;
            }
        }
    }
}
```

(2) 普通bfs(最短路bfs): 

bfs最短路模型，首次访问即为最短距离

dist 距离数组 st 判断访问数组（最短路情况下可以dist代替）

AcWing 844 走迷宫

```cpp
int bfs() {
    queue<PII> q;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    memset(d, -1, sizeof(d));
    q.push({0, 0});
    d[0][0] = 0;
    while (q.size()) {
        auto t = q.front();
        q.pop();

        //普通bfs,这里不走回头路
        for (int i = 0; i < 4; i++) {
            int x = t.first + dx[i], y = t.second + dy[i];
            if (x >= 0 && x < n && y >= 0 && y < m && g[x][y] == 0 && d[x][y] == -1) {
                d[x][y] = d[t.first][t.second] + 1;
                q.push({x, y});
            }
        }
    }

    return d[n - 1][m - 1];
}
```

AcWing 1076. 迷宫问题

```cpp
//反向bfs最短路 + 正向求方案
int g[N][N];
PII q[M];
PII pre[N][N];
int n;

void bfs(int sx, int sy) {
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    memset(pre, -1, sizeof(pre));
    pre[sx][sy] = {0, 0};

    while (hh <= tt) {
        auto t = q[hh++];
        for (int i = 0; i < 4; i++) {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= n) continue;
            if (g[a][b]) continue;
            if (pre[a][b].x != -1) continue;

            q[++tt] = {a, b};
            pre[a][b] = t;
        }
    }
}
```


(3) 多源bfs: 

leetcode 542 01 矩阵

```cpp
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<int>> dist(n, vector<int>(m, -1));
        queue<pair<int,int>> q;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!matrix[i][j])
                {
                    dist[i][j] = 0;
                    q.push(make_pair(i, j));
                }
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        while (q.size())
        {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < 4; i ++ )
            {
                int x = t.first + dx[i], y = t.second + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m && dist[x][y] == -1)
                {
                    dist[x][y] = dist[t.first][t.second] + 1;
                    q.push(make_pair(x, y));
                }
            }
        }
        return dist;
    }
};
```

(4) 双向bfs: AcWing190 字串变换

```cpp

int extend(queue<string>& q, unordered_map<string, int>&da, unordered_map<string, int>& db, 
    string a[N], string b[N])
{
    int d = da[q.front()];
    while (q.size() && da[q.front()] == d)
    {
        auto t = q.front();
        q.pop();
		//每次每边扩展完整一层
        for (int i = 0; i < n; i ++ ) 
            for (int j = 0; j < t.size(); j ++ )
                if (t.substr(j, a[i].size()) == a[i])
                {
                    string r = t.substr(0, j) + b[i] + t.substr(j + a[i].size());
                    if (db.count(r)) return da[t] + db[r] + 1;
                    if (da.count(r)) continue;
                    da[r] = da[t] + 1;
                    q.push(r);
                }
    }

    return 11;
}

int bfs()
{
    if (A == B) return 0;
    queue<string> qa, qb;
    unordered_map<string, int> da, db;

    qa.push(A), qb.push(B);
    da[A] = db[B] = 0;

    int step = 0;
    while (qa.size() && qb.size())
    {
        int t;
        if (qa.size() < qb.size()) t = extend(qa, da, db, a, b);
        else t = extend(qb, db, da, b, a);

        if (t <= 10) return t;
        if ( ++ step == 10) return -1;
    }

    return -1;
}
```

#### Astar：图遍历 + BFS

AcWing 178. 第K短路

```cpp
#include <iostream>
#include <cstring>
#include <queue>

using namespace std;

#define x first
#define y second

typedef pair<int,int> PII;
typedef pair<int,pair<int,int>> PIII;

const int N = 1010,M = 200010;
int h[N],rh[N],e[M],w[M],ne[M],idx;
int dist[N],cnt[N];
bool st[N];
int n,m,S,T,K;

void add(int h[],int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

void dijkstra() {
    priority_queue<PII, vector<PII>, greater<PII>> heap; //小顶堆

    memset(dist, 0x3f, sizeof(dist));
    heap.push({0, T});
    dist[T] = 0;

    while (heap.size()) {
        auto t = heap.top();
        heap.pop();

        int ver = t.y;
        if (st[ver]) continue;
        st[ver] = true;
        
        for (int i = rh[ver]; ~i; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[ver] + w[i]) {
                dist[j] = dist[ver] + w[i];
                heap.push({dist[j], j});
            }
        }
    }
}

int astar() {
    priority_queue<PIII, vector<PIII>, greater<PIII>> heap;
    //g(x) 到S距离, 真实值; h(x) 到T距离，估计值（参考最短距离估计）
    heap.push({dist[S], {0, S}});  //{ 到S距离 + 到T距离，{到S距离，顶点}}

    while (heap.size()) {
        auto t = heap.top();
        heap.pop();

        int ver = t.y.y;
        int distance = t.y.x;

        cnt[ver]++;
        //第k次弹出T,则为第k小
        if (cnt[T] == K) return distance;

        for (int i = h[ver]; ~i; i = ne[i]) {
            int j = e[i];
            if (cnt[j] < K) {
                // 按真实值+估计值 = d[j]+f[j] = dist[S->t] + w[t->j] + dist[j->T] 堆排序
                // 真实值 dist[S->t] = distance+w[i]
                heap.push({distance + w[i] + dist[j], {distance + w[i], j}});
            }
        }
    }
    return -1;
}

int main() {
    cin >> n >> m;

    memset(h,-1, sizeof(h));
    memset(rh,-1, sizeof(rh));
    for(int i = 0;i < m;i++){
        int a,b,c;
        cin >> a >> b >> c;
        add(h, a, b, c);
        add(rh, b, a, c);
    }
    cin >> S >> T >> K;
    if(S == T) K++;

    dijkstra(); //求到终点的最短距离

    cout << astar() << endl;

    return 0;
}
```

#### 模拟退火

```C++
double calc()
{
    // TODO: 计算当前方案的值
    
    ans = min(ans, res);  // 更新全局答案
    return res;
}

void simulate_anneal()  // 模拟退火
{
    for (double t = 1e6; t > 1e-6; t *= 0.95)  // 逐渐降温
    {
        double x = calc();  // 原方案的值
        // TODO: 随机一个新方案
        double y = calc();  // 新方案的值
        double delta = y - x;
        
        // 新方案更好，则必选新方案；否则以一定概率选新方案 
        if (exp(-delta / t) > (double)rand() / RAND_MAX)
        {
            // TODO: 换成新方案
        }
    }
}
```
## 0x30  数学知识

**试除法判定质数——模板题 AcWing 866. 试除法判定质数**
```cpp
bool is_prime(int x)
{
    if (x < 2) return false;
    for (int i = 2; i <= x / i; i ++ )
        if (x % i == 0)
            return false;
    return true;
}
```
**试除法分解质因数——模板题 AcWing 867. 分解质因数**
```cpp
void divide(int x)
{
    for (int i = 2; i <= x / i; i ++ )
        if (x % i == 0)
        {
            int s = 0;
            while (x % i == 0) x /= i, s ++ ;
            cout << i << ' ' << s << endl;
        }
    if (x > 1) cout << x << ' ' << 1 << endl;
    cout << endl;
}
```
**朴素筛法求素数——模板题 AcWing 868. 筛质数**
```cpp
int primes[N], cnt;     // primes[]存储所有素数
bool st[N];         // st[x]存储x是否被筛掉

void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (st[i]) continue;
        primes[cnt ++ ] = i;
        for (int j = i + i; j <= n; j += i)
            st[j] = true;
    }
}
```

**线性筛法求素数——模板题 AcWing 868. 筛质数**

```cpp
int primes[N], cnt;     // primes[]存储所有素数
bool st[N];         // st[x]存储x是否被筛掉

void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}
```

**试除法求所有约数—— 模板题 AcWing 869. 试除法求约数**
```cpp
vector<int> get_divisors(int x)
{
    vector<int> res;
    for (int i = 1; i <= x / i; i ++ )
        if (x % i == 0)
        {
            res.push_back(i);
            if (i != x / i) res.push_back(x / i);
        }
    sort(res.begin(), res.end());
    return res;
}
```
**约数个数和约数之和 —— 模板题 AcWing 870. 约数个数, AcWing 871. 约数之和**

如果 N = p1^c1 * p2^c2 * - *pk^ck
约数个数： (c1 + 1) * (c2 + 1) * - * (ck + 1)
约数之和： (p1^0 + p1^1 + - + p1^c1) * - * (pk^0 + pk^1 + - + pk^ck)

**欧几里得算法 —— 模板题 AcWing 872. 最大公约数**
int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}
**求欧拉函数 —— 模板题 AcWing 873. 欧拉函数**
```cpp
int phi(int x)
{
    int res = x;
    for (int i = 2; i <= x / i; i ++ )
        if (x % i == 0)
        {
            res = res / i * (i - 1);
            while (x % i == 0) x /= i;
        }
    if (x > 1) res = res / x * (x - 1);

    return res;
}
```
**筛法求欧拉函数 —— 模板题 AcWing 874. 筛法求欧拉函数**
```cpp
int primes[N], cnt;     // primes[]存储所有素数
int euler[N];           // 存储每个数的欧拉函数
bool st[N];         // st[x]存储x是否被筛掉


void get_eulers(int n)
{
    euler[1] = 1;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            euler[i] = i - 1;
        }
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0)
            {
                euler[t] = euler[i] * primes[j];
                break;
            }
            euler[t] = euler[i] * (primes[j] - 1);
        }
    }
}
```
**快速幂 —— 模板题 AcWing 875. 快速幂**

```cpp
求 m^k mod p，时间复杂度 O(logk)。
int qmi(int m, int k, int p)
{
    int res = 1 % p, t = m;
    while (k)
    {
        if (k&1) res = res * t % p;
        t = t * t % p;
        k >>= 1;
    }
    return res;
}
```
**扩展欧几里得算法 —— 模板题 AcWing 877. 扩展欧几里得算法**
```cpp
// 求x, y，使得ax + by = gcd(a, b)
int exgcd(int a, int b, int &x, int &y)
{
    if (!b)
    {
        x = 1; y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= (a/b) * x;
    return d;
}
```
**高斯消元 —— 模板题 AcWing 883. 高斯消元解线性方程组**
```cpp
// a[N][N]是增广矩阵
int gauss()
{
    int c, r;
    for (c = 0, r = 0; c < n; c ++ )
    {
        int t = r;
        for (int i = r; i < n; i ++ )   // 找到绝对值最大的行
            if (fabs(a[i][c]) > fabs(a[t][c]))
                t = i;

        if (fabs(a[t][c]) < eps) continue;

        for (int i = c; i <= n; i ++ ) swap(a[t][i], a[r][i]);      // 将绝对值最大的行换到最顶端
        for (int i = n; i >= c; i -- ) a[r][i] /= a[r][c];      // 将当前行的首位变成1
        for (int i = r + 1; i < n; i ++ )       // 用当前行将下面所有的列消成0
            if (fabs(a[i][c]) > eps)
                for (int j = n; j >= c; j -- )
                    a[i][j] -= a[r][j] * a[i][c];

        r ++ ;
    }

    if (r < n)
    {
        for (int i = r; i < n; i ++ )
            if (fabs(a[i][n]) > eps)
                return 2; // 无解
        return 1; // 有无穷多组解
    }

    for (int i = n - 1; i >= 0; i -- )
        for (int j = i + 1; j < n; j ++ )
            a[i][n] -= a[i][j] * a[j][n];

    return 0; // 有唯一解
}
```

**递归法求组合数 —— 模板题 AcWing 885. 求组合数 I**
```cpp
// c[a][b] 表示从a个苹果中选b个的方案数
for (int i = 0; i < N; i ++ )
    for (int j = 0; j <= i; j ++ )
        if (!j) c[i][j] = 1;
        else c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
```

**通过预处理逆元的方式求组合数 —— 模板题 AcWing 886. 求组合数 II**
首先预处理出所有阶乘取模的余数fact[N]，以及所有阶乘取模的逆元infact[N]
如果取模的数是质数，可以用费马小定理求逆元
```cpp
int qmi(int a, int k, int p)    // 快速幂模板
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

// 预处理阶乘的余数和阶乘逆元的余数
fact[0] = infact[0] = 1;
for (int i = 1; i < N; i ++ )
{
    fact[i] = (LL)fact[i - 1] * i % mod;
    infact[i] = (LL)infact[i - 1] * qmi(i, mod - 2, mod) % mod;
}
```

**Lucas定理 —— 模板题 AcWing 887. 求组合数 III**

```cpp
若p是质数，则对于任意整数 1 <= m <= n，有：
    C(n, m) = C(n % p, m % p) * C(n / p, m / p) (mod p)

int qmi(int a, int k, int p)  // 快速幂模板
{
    int res = 1 % p;
    while (k)
    {
        if (k & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

int C(int a, int b, int p)  // 通过定理求组合数C(a, b)
{
    if (a < b) return 0;

    LL x = 1, y = 1;  // x是分子，y是分母
    for (int i = a, j = 1; j <= b; i --, j ++ )
    {
        x = (LL)x * i % p;
        y = (LL) y * j % p;
    }

    return x * (LL)qmi(y, p - 2, p) % p;
}

int lucas(LL a, LL b, int p)
{
    if (a < p && b < p) return C(a, b, p);
    return (LL)C(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
}
```
**分解质因数法求组合数 —— 模板题 AcWing 888. 求组合数 IV**
```cpp
当我们需要求出组合数的真实值，而非对某个数的余数时，分解质因数的方式比较好用：
    1. 筛法求出范围内的所有质数
    2. 通过 C(a, b) = a! / b! / (a - b)! 这个公式求出每个质因子的次数。 n! 中p的次数是 n / p + n / p^2 + n / p^3 + -
    3. 用高精度乘法将所有质因子相乘

int primes[N], cnt;     // 存储所有质数
int sum[N];     // 存储每个质数的次数
bool st[N];     // 存储每个数是否已被筛掉


void get_primes(int n)      // 线性筛法求素数
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}


int get(int n, int p)       // 求n！中的次数
{
    int res = 0;
    while (n)
    {
        res += n / p;
        n /= p;
    }
    return res;
}


vector<int> mul(vector<int> a, int b)       // 高精度乘低精度模板
{
    vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size(); i ++ )
    {
        t += a[i] * b;
        c.push_back(t % 10);
        t /= 10;
    }

    while (t)
    {
        c.push_back(t % 10);
        t /= 10;
    }

    return c;
}

get_primes(a);  // 预处理范围内的所有质数

for (int i = 0; i < cnt; i ++ )     // 求每个质因数的次数
{
    int p = primes[i];
    sum[i] = get(a, p) - get(b, p) - get(a - b, p);
}

vector<int> res;
res.push_back(1);

for (int i = 0; i < cnt; i ++ )     // 用高精度乘法将所有质因子相乘
    for (int j = 0; j < sum[i]; j ++ )
        res = mul(res, primes[i]);
```

**卡特兰数 —— 模板题 AcWing 889. 满足条件的01序列**

给定n个0和n个1，它们按照某种顺序排成长度为2n的序列，满足任意前缀中0的个数都不少于1的个数的序列的数量为： Cat(n) = C(2n, n) / (n + 1)

**NIM游戏 —— 模板题 AcWing 891. Nim游戏**
给定N堆物品，第i堆物品有Ai个。两名玩家轮流行动，每次可以任选一堆，取走任意多个物品，可把一堆取光，但不能不取。取走最后一件物品者获胜。两人都采取最优策略，问先手是否必胜。

我们把这种游戏称为NIM博弈。把游戏过程中面临的状态称为局面。整局游戏第一个行动的称为先手，第二个行动的称为后手。若在某一局面下无论采取何种行动，都会输掉游戏，则称该局面必败。
所谓采取最优策略是指，若在某一局面下存在某种行动，使得行动后对面面临必败局面，则优先采取该行动。同时，这样的局面被称为必胜。我们讨论的博弈问题一般都只考虑理想情况，即两人均无失误，都采取最优策略行动时游戏的结果。
NIM博弈不存在平局，只有先手必胜和先手必败两种情况。

定理： NIM博弈先手必胜，当且仅当 A1 ^ A2 ^ … ^ An != 0

**公平组合游戏ICG**

若一个游戏满足：

	由两名玩家交替行动；
	在游戏进程的任意时刻，可以执行的合法行动与轮到哪名玩家无关；
	不能行动的玩家判负；

则称该游戏为一个公平组合游戏。
NIM博弈属于公平组合游戏，但城建的棋类游戏，比如围棋，就不是公平组合游戏。因为围棋交战双方分别只能落黑子和白子，胜负判定也比较复杂，不满足条件2和条件3。

**有向图游戏**
给定一个有向无环图，图中有一个唯一的起点，在起点上放有一枚棋子。两名玩家交替地把这枚棋子沿有向边进行移动，每次可以移动一步，无法移动者判负。该游戏被称为有向图游戏。
任何一个公平组合游戏都可以转化为有向图游戏。具体方法是，把每个局面看成图中的一个节点，并且从每个局面向沿着合法行动能够到达的下一个局面连有向边。

**Mex运算**
设S表示一个非负整数集合。定义mex(S)为求出不属于集合S的最小非负整数的运算，即：
mex(S) = min{x}, x属于自然数，且x不属于S

**SG函数**
在有向图游戏中，对于每个节点x，设从x出发共有k条有向边，分别到达节点y1, y2, …, yk，定义SG(x)为x的后继节点y1, y2, …, yk 的SG函数值构成的集合再执行mex(S)运算的结果，即：
SG(x) = mex({SG(y1), SG(y2), …, SG(yk)})
特别地，整个有向图游戏G的SG函数值被定义为有向图游戏起点s的SG函数值，即SG(G) = SG(s)。

**有向图游戏的和 —— 模板题 AcWing 893. 集合-Nim游戏**

设G1, G2, …, Gm 是m个有向图游戏。定义有向图游戏G，它的行动规则是任选某个有向图游戏Gi，并在Gi上行动一步。G被称为有向图游戏G1, G2, …, Gm的和。
有向图游戏的和的SG函数值等于它包含的各个子游戏SG函数值的异或和，即：
SG(G) = SG(G1) ^ SG(G2) ^ … ^ SG(Gm)

定理
有向图游戏的某个局面必胜，当且仅当该局面对应节点的SG函数值大于0。
有向图游戏的某个局面必败，当且仅当该局面对应节点的SG函数值等于0。


#### BSGS

```C++
int bsgs(int a, int b, int p)  // a ^ x ≡ b (mod p) 的最小非负整数解
{
    if (b == 1) return 0;
    int k = sqrt(p) + 1;
    unordered_map<int, int> hash;
    for (int i = 0, j = b; i < k; i ++ )
    {
        hash[j] = i;
        j = (LL)j * a % p;
    }
    int ak = 1;
    for (int i = 0; i < k; i ++ ) ak = (LL)ak * a % p;
    for (int i = 1, j = ak; i <= k; i ++ )
    {
        if (hash.count(j) && (LL)i * k >= hash[j])
            return (LL)i * k - hash[j];
        j = (LL)j * ak % p;
    }
    return -1;
}
```
#### FFT
```C++
struct Complex
{
    double x, y;
    Complex operator+ (const Complex& t) const
    {
        return {x + t.x, y + t.y};
    }
    Complex operator- (const Complex& t) const
    {
        return {x - t.x, y - t.y};
    }
    Complex operator* (const Complex& t) const
    {
        return {x * t.x - y * t.y, x * t.y + y * t.x};
    }
};
int rev[N], bit, tot;  // tot = 1 << bit

void fft(Complex a[], int inv)
{
    for (int i = 0; i < tot; i ++ )
        rev[i] = (rev[i >> 1] >> 1) | ((i & 1) << (bit - 1));
    for (int i = 0; i < tot; i ++ )
        if (i < rev[i])
            swap(a[i], a[rev[i]]);
    for (int mid = 1; mid < tot; mid <<= 1)
    {
        auto w1 = Complex({cos(PI / mid), inv * sin(PI / mid)});
        for (int i = 0; i < tot; i += mid * 2)
        {
            auto wk = Complex({1, 0});
            for (int j = 0; j < mid; j ++, wk = wk * w1)
            {
                auto x = a[i + j], y = wk * a[i + j + mid];
                a[i + j] = x + y, a[i + j + mid] = x - y;
            }
        }
    }
}

```

## 0x40 数据结构进阶

#### 并查集 

模板题 AcWing 836. 合并集合, AcWing 837. 连通块中点的数量

(1)朴素并查集：
```cpp
    int p[N]; //存储每个点的祖宗节点

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
```

(2)维护size的并查集：
```cpp
    int p[N], size[N];
    //p[]存储每个点的祖宗节点, size[]只有祖宗节点的有意义，表示祖宗节点所在集合中的点的数量

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        size[i] = 1;
    }

    // 合并a和b所在的两个集合：
    size[find(b)] += size[find(a)];
    p[find(a)] = find(b);
```

(3)维护到祖宗节点距离的并查集：
```cpp
    int p[N], d[N];
    //p[]存储每个点的祖宗节点, d[x]存储x到p[x]的距离

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x)
        {
            int u = find(p[x]);
            d[x] += d[p[x]];
            p[x] = u;
        }
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        d[i] = 0;
    }

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
    d[find(a)] = distance; // 根据具体问题，初始化find(a)的偏移量
```

#### 树状数组 

```C++
int lowbit(int x)
{
    return x & -x;
}

void update(int x, int c)  // 位置x加c
{
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
}

int query(int x)  // 返回前x个数的和
{
    int res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}
```
####  线段树
```C++
struct Node
{
    int l, r;
    // TODO: 需要维护的信息和懒标记
}tr[N * 4];

void pushup(int u)
{
    // TODO: 利用左右儿子信息维护当前节点的信息
}

void pushdown(int u)
{
    // TODO: 将懒标记下传
}

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r};
    else
    {
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void update(int u, int l, int r, int d)
{
    if (tr[u].l >= l && tr[u].r <= r)
    {
        // TODO: 修改区间
    }
    else
    {
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) update(u << 1, l, r, d);
        if (r > mid) update(u << 1 | 1, l, r, d);
        pushup(u);
    }
}

int query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r)
    {
        return ;  // TODO 需要补充返回值
    }
    else
    {
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        int res = 0;
        if (l <= mid ) res = query(u << 1, l, r);
        if (r > mid) res += query(u << 1 | 1, l, r);
        return res;
    }
}

```

#### AC自动机

```C++
void insert(char str[])  // 将str插入Trie中
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!tr[p][u]) tr[p][u] = ++ idx;
        p = tr[p][u];
    }
    cnt[p] ++ ;  // 记录单词出现次数
}

void build()  // 创建AC自动机
{
    int hh = 0, tt = -1;
    for (int i = 0; i < 26; i ++ )
        if (tr[0][i])
            q[ ++ tt] = tr[0][i];
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = 0; i < 26; i ++ )
        {
            int p = tr[t][i];
            if (!p) tr[t][i] = tr[ne[t]][i];
            else
            {
                ne[p] = tr[ne[t]][i];
                cnt[p] += cnt[ne[p]];
                q[ ++ tt] = p;
            }
        }
    }
}

```

#### splay
```C++
struct Node
{
    int s[2], p, v;
    int size;
    
    void init(int _v, int _p)
    {
        v = _v, p = _p;
        size = 1;
    }
}tr[N];
int root, idx;

void pushup(int x)
{
    // TODO: 利用子节点信息维护当前节点信息
}

void pushdown(int x)
{
    // TODO: 将懒标记下传
}

void rotate(int x)  // 旋转
{
    int y = tr[x].p, z = tr[y].p;
    int k = tr[y].s[1] == x;
    tr[z].s[tr[z].s[1] == y] = x, tr[x].p = z;
    tr[y].s[k] = tr[x].s[k ^ 1], tr[tr[x].s[k ^ 1]].p = y;
    tr[x].s[k ^ 1] = y, tr[y].p = x;
    pushup(y), pushup(x);
}

void splay(int x, int k)  // splay操作
{
    while (tr[x].p != k)
    {
        int y = tr[x].p, z = tr[y].p;
        if (z != k)
            if ((tr[y].s[1] == x) ^ (tr[z].s[1] == y)) rotate(x);
            else rotate(y);
        rotate(x);
    }
    if (!k) root = x;
}

```

#### 动态树
```C++
struct Node
{
    int s[2], p, v;
    int rev;
    // TODO: 定义需要维护的信息和懒标记
}tr[N];
int stk[N];  // 栈

void pushrev(int x)
{
    swap(tr[x].s[0], tr[x].s[1]);
    tr[x].rev ^= 1;
}

void pushup(int x)
{
    // TODO: 利用子节点信息来维护当前节点的信息
}

void pushdown(int x)
{
    if (tr[x].rev)
    {
        pushrev(tr[x].s[0]), pushrev(tr[x].s[1]);
        tr[x].rev = 0;
    }
    // TODO: 将当前节点的懒标记下传
}

bool isroot(int x)  // 判断x是否为原树的根节点
{
    return tr[tr[x].p].s[0] != x && tr[tr[x].p].s[1] != x;
}

void rotate(int x)  // splay的旋转操作
{
    int y = tr[x].p, z = tr[y].p;
    int k = tr[y].s[1] == x;
    if (!isroot(y)) tr[z].s[tr[z].s[1] == y] = x;
    tr[x].p = z;
    tr[y].s[k] = tr[x].s[k ^ 1], tr[tr[x].s[k ^ 1]].p = y;
    tr[x].s[k ^ 1] = y, tr[y].p = x;
    pushup(y), pushup(x);
}

void splay(int x)  // splay操作
{
    int top = 0, r = x;
    stk[ ++ top] = r;
    while (!isroot(r)) stk[ ++ top] = r = tr[r].p;
    while (top) pushdown(stk[top -- ]);
    while (!isroot(x))
    {
        int y = tr[x].p, z = tr[y].p;
        if (!isroot(y))
            if ((tr[y].s[1] == x) ^ (tr[z].s[1] == y)) rotate(x);
            else rotate(y);
        rotate(x);
    }
}

void access(int x)  // 建立一条从根到x的路径，同时将x变成splay的根节点
{
    int z = x;
    for (int y = 0; x; y = x, x = tr[x].p)
    {
        splay(x);
        tr[x].s[1] = y, pushup(x);
    }
    splay(z);
}

void makeroot(int x)  // 将x变成原树的根节点
{
    access(x);
    pushrev(x);
}

int findroot(int x)  // 找到x所在原树的根节点, 再将原树的根节点旋转到splay的根节点
{
    access(x);
    while (tr[x].s[0]) pushdown(x), x = tr[x].s[0];
    splay(x);
    return x;
}

void split(int x, int y)  // 给x和y之间的路径建立一个splay，其根节点是y
{
    makeroot(x);
    access(y);
}

void link(int x, int y)  // 如果x和y不连通，则加入一条x和y之间的边
{
    makeroot(x);
    if (findroot(y) != x) tr[x].p = y;
}

void cut(int x, int y)  // 如果x和y之间存在边，则删除该边
{
    makeroot(x);
    if (findroot(y) == x && tr[y].p == x && !tr[y].s[0])
    {
        tr[x].s[1] = tr[y].p = 0;
        pushup(x);
    }
}
```

####  DLX重复覆盖
```C++
int l[N], r[N], u[N], d[N], col[N], row[N], s[N], idx;
int ans[N], top;  // 记录选择了哪些行

void init()  // 初始化十字链表
{
    for (int i = 0; i <= m; i ++ )
    {
        l[i] = i - 1, r[i] = i + 1;
        u[i] = d[i] = i;
    }
    l[0] = m, r[m] = 0;
    idx = m + 1;
}

void add(int& hh, int& tt, int x, int y)  // 在十字链表中添加节点
{
    row[idx] = x, col[idx] = y, s[y] ++ ;
    u[idx] = y, d[idx] = d[y], u[d[y]] = idx, d[y] = idx;
    r[hh] = l[tt] = idx, r[idx] = tt, l[idx] = hh;
    tt = idx ++ ;
}

void remove(int p)
{
    r[l[p]] = r[p], l[r[p]] = l[p];
    for (int i = d[p]; i != p; i = d[i])
        for (int j = r[i]; j != i; j = r[j])
        {
            s[col[j]] -- ;
            d[u[j]] = d[j], u[d[j]] = u[j];
        }
}

void resume(int p)
{
    for (int i = d[p]; i != p; i = d[i])
        for (int j = r[i]; j != i; j = r[j])
        {
            s[col[j]] ++ ;
            d[u[j]] = j, u[d[j]] = j;
        }
    r[l[p]] = p, l[r[p]] = p;
}

bool dfs()
{
    if (!r[0]) return true;
    int p = r[0];
    for (int i = r[0]; i; i = r[i])
        if (s[i] < s[p])
            p = i;
    if (!s[p]) return false;
    remove(p);
    for (int i = d[p]; i != p; i = d[i])
    {
        ans[ ++ top] = row[i];
        for (int j = r[i]; j != i; j = r[j]) remove(col[j]);
        if (dfs()) return true;
        for (int j = r[i]; j != i; j = r[j]) resume(col[j]);
        top -- ;
    }
    resume(p);
    return false;
}

```

#### DLX精确覆盖
```C++
int l[N], r[N], u[N], d[N], col[N], row[N], s[N], idx;
int ans[N], top;  // 记录选择了哪些行

void init()  // 初始化十字链表
{
    for (int i = 0; i <= m; i ++ )
    {
        l[i] = i - 1, r[i] = i + 1;
        u[i] = d[i] = i;
    }
    l[0] = m, r[m] = 0;
    idx = m + 1;
}

void add(int& hh, int& tt, int x, int y)  // 在十字链表中添加节点
{
    row[idx] = x, col[idx] = y, s[y] ++ ;
    u[idx] = y, d[idx] = d[y], u[d[y]] = idx, d[y] = idx;
    r[hh] = l[tt] = idx, r[idx] = tt, l[idx] = hh;
    tt = idx ++ ;
}

void remove(int p)
{
    r[l[p]] = r[p], l[r[p]] = l[p];
    for (int i = d[p]; i != p; i = d[i])
        for (int j = r[i]; j != i; j = r[j])
        {
            s[col[j]] -- ;
            d[u[j]] = d[j], u[d[j]] = u[j];
        }
}

void resume(int p)
{
    for (int i = d[p]; i != p; i = d[i])
        for (int j = r[i]; j != i; j = r[j])
        {
            s[col[j]] ++ ;
            d[u[j]] = j, u[d[j]] = j;
        }
    r[l[p]] = p, l[r[p]] = p;
}

bool dfs()
{
    if (!r[0]) return true;
    int p = r[0];
    for (int i = r[0]; i; i = r[i])
        if (s[i] < s[p])
            p = i;
    if (!s[p]) return false;
    remove(p);
    for (int i = d[p]; i != p; i = d[i])
    {
        ans[ ++ top] = row[i];
        for (int j = r[i]; j != i; j = r[j]) remove(col[j]);
        if (dfs()) return true;
        for (int j = r[i]; j != i; j = r[j]) resume(col[j]);
        top -- ;
    }
    resume(p);
    return false;
}
```

#### 左偏树
```C++
int int v[N], dist[N], l[N], r[N], idx;

bool cmp(int x, int y)  // 比较两个节点的权值大小
{
    if (v[x] != v[y]) return v[x] < v[y];
    return x < y;
}

int merge(int x, int y)  // 合并两棵左偏树
{
    if (!x || !y) return x + y;
    if (cmp(y, x)) swap(x, y);
    r[x] = merge(r[x], y);
    if (dist[r[x]] > dist[l[x]]) swap(l[x], r[x]);
    dist[x] = dist[r[x]] + 1;
    return x;
}
```

#### 后缀数组

```C++
char s[N];  // 存储原字符串
int sa[N], rk[N], x[N], y[N], height[N], c[N];

void get_sa()  // 创建后缀数组
{
    for (int i = 1; i <= n; i ++ ) c[x[i] = s[i]] ++ ;
    for (int i = 2; i <= m; i ++ ) c[i] += c[i - 1];
    for (int i = n; i; i -- ) sa[c[x[i]] -- ] = i;
    for (int k = 1; k <= n; k <<= 1)
    {
        int num = 0;
        for (int i = n - k + 1; i <= n; i ++ ) y[ ++ num] = i;
        for (int i = 1; i <= n; i ++ )
            if (sa[i] > k)
                y[ ++ num] = sa[i] - k;
        for (int i = 1; i <= m; i ++ ) c[i] = 0;
        for (int i = 1; i <= n; i ++ ) c[x[i]] ++ ;
        for (int i = 2; i <= m; i ++ ) c[i] += c[i - 1];
        for (int i = n; i; i -- ) sa[c[x[y[i]]] -- ] = y[i], y[i] = 0;
        swap(x, y);
        x[sa[1]] = 1, num = 1;
        for (int i = 2; i <= n; i ++ )
            x[sa[i]] = (y[sa[i]] == y[sa[i - 1]] && y[sa[i] + k] == y[sa[i - 1] + k]) ? num : ++ num;
        if (num == n) break;
        m = num;
    }
}

void get_height()  // 预处理height[]数组
{
    int k = 0;
    for (int i = 1; i <= n; i ++ ) rk[sa[i]] = i;
    for (int i = 1; i <= n; i ++ )
    {
        if (rk[i] == 1) continue;
        if (k) k -- ;
        int j = sa[rk[i] - 1];
        while (i + k <= n && j + k <= n && s[i + k] == s[j + k]) k ++ ;
        height[rk[i]] = k;
    }
}
```

#### 后缀自动机

```C++
int tot = 1, last = 1;
struct Node
{
    int len, fa;
    int ch[26];
}node[N];

void extend(char c)
{
    int p = last, np = last = ++ tot;
    node[np].len = node[p].len + 1;
    for (; p && !node[p].ch[c]; p = node[p].fa) node[p].ch[c] = np;
    if (!p) node[np].fa = 1;
    else
    {
        int q = node[p].ch[c];
        if (node[q].len == node[p].len + 1) node[np].fa = q;
        else
        {
            int nq = ++ tot;
            node[nq] = node[q], node[nq].len = node[p].len + 1;
            node[q].fa = node[np].fa = nq;
            for (; p && node[p].ch[c] == q; p = node[p].fa) node[p].ch[c] = nq;
        }
    }
}

```

## 0x50 动态规划

#### 线性DP

(1) LIS 普通模板：Acwing 895. 最长上升子序列

```cpp
int n;
for (int i = 1; i <= n; i++) {
	f[i] = 1;
	for (int j = 1; j < i; j++) {
		if (a[i] > a[j]) {
			f[i] = max(f[i], f[j] + 1);
		}
	}
}

int res = 0;
for (int i = 1; i <= n; i++) res = max(res, f[i]);
```

(2) LIS二分模板：Acwing 896. 最长上升子序列 II

```cpp
int a[N], q[N];
int n;

int len = 0;
for (int i = 1; i <= n; i++) {
	int l = 0, r = len;
	//寻找严格右边界
	while (l < r) {
		int mid = l + r + 1 >> 1;
		if (q[mid] < a[i]) l = mid;
		else r = mid - 1;
	}
	len = max(len, r + 1);
	q[r + 1] = a[i];
}
```

(3) LCS模板:AcWing 897. 最长公共子序列， 变形，编辑距离等

```cpp
#include <iostream>

using namespace std;

const int N = 10010, M = 10010;
char a[N], b[M];
int f[N][M];
int n, m;

int main() {
    cin >> n >> m;
    cin >> a + 1;
    cin >> b + 1;
    //LCS模板
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            f[i][j] = max(f[i - 1][j], f[i][j - 1]);
            if (a[i] == b[j]) {
                f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1);
            }
        }
    }

    cout << f[n][m];

    return 0;
}
```

#### 背包DP

(1) 0-1 背包:  

AcWing 2 01背包问题

```C++
//二循环二维
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= m; j++) {
        // 当前背包容量装不进第i个物品，则价值等于前i-1个物品
        if (j < v[i]) {
            f[i][j] = f[i - 1][j];
        }
         // 能装，需进行决策是否选择第i个物品
        else {
            f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]);
        }
    }
}
//二循环一维优化
for (int i = 1; i <= n; i++) {
    for (int j = m; j >= v[i]; j--) {
        f[j] = max(f[j], f[j - v[i]] + w[i]);
    }
}
```

(2) 完全背包： 

完全背包优化：三循环二维数组--> 二循环二维数组--> 二循环一维数组
  
```C++
//三循环二维数组
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= m; j++) {
        for (int k = 0; k * v[i] <= j; k++) {
            f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
        }
    }
}
//二循环二维数组
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= m; j++) {
        f[i][j] = f[i - 1][j]; //不选
        if (j - v[i] >= 0)
            f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);
    }
}

//一维优化
for (int i = 1; i <= n; i++) {
    for (int j = v[i]; j <= m; j++) {
        f[j] = max(f[j], f[j - v[i]] + w[i]);
    }

}
```

(3) 分组背包：

 AcWing 9 分组背包问题

```C++
//三循环二维
f[N][N]; //只从前i组物品中选，当前体积小于等于j的最大值,每个组只能选一个。
//物品
for (int i = 1; i <= n; i++) {
	//体积
	for (int j = 0; j <= m; j++) {
		f[i][j] = f[i - 1][j];
		//决策
		for (int k = 0; k < s[i]; k++) {
			if (j >= v[i][k]) {
				f[i][j] = max(f[i][j], f[i-1][j - v[i][k]] + w[i][k]);
			}
		}
	}
}
cout << f[n][m] << endl;
```

```cpp
//三循环一维
//物品
for (int i = 1; i <= n; i++) {
	//体积
	//f[i][j]需要从f[i - 1][j]计算来的，去掉一维需要反向
	for (int j = m; j >= 0; j--) {
		//决策
		for (int k = 0; k < s[i]; k++) {
			if (j >= v[i][k]) {
				f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);
			}
		}
	}
}
cout << f[m] << endl;
```

(4) 多重背包:   
 
第i个物品选0~s[i]个

```C++
//三重循环二维
// f[i][j]表示从前i个物品中选,总体积不超过j的方案的最大价值
// 集合划分:第i个物品选0~s[i]个
for (int i = 1; i <= n; i ++ )
    for (int j = 0; j <= m; j ++ )
        for (int k = 0; k <= s[i] && k * v[i] <= j; k ++ ) //比完全背包的三重循环二维数组版本多了k <= s[i]的限制
            f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
 		
//三循环一维
for (int i = 1; i <= n; i ++ ) //循环各组
    for (int j = m; j >= 0; j -- )
        for (int k = 0; k <= s[i] && v[i]*k <= j; k ++ ) //组内循环 
            f[j] = max(f[j], f[j - v[i]*k] + w[i]*k);
```

(5) 混合背包：

01背包(1类物品用1次) + 完全背包(2类物品无限用) + 多重背包(3类物品用si次)

```C++
for (int i = 0; i < n; i++) {
	if (!s[i]) {
		for (int j = v[i]; j <= m; j++) //完全背包
			f[j] = max(f[j], f[j - v[i]] + w[i]);
	} else {
		if (s[i] == -1) s[i] = 1;//0-1背包
		for (int k = 1; k <= s[i]; k *= 2) {
			for (int j = m; j >= k * v[i]; j--)
				f[j] = max(f[j], f[j - k * v[i]] + k * w[i]);
			s[i] -= k;
		}
		if (s[i])//多重背包
		{
			for (int j = m; j >= s[i] * v[i]; j--)
				f[j] = max(f[j], f[j - s[i] * v[i]] + s[i] * w[i]);
		}
	}
}
```

(6) 二维费用背包

```C++
//f[i][j][k] 表示考虑前i个物品，且容量不超过j，总重量不超过k的集合下能获得的最大价值
//f[j][k],01背包问题的一维数组的二维化，一维是体积，二维是质量
for (int i = 0; i < n; i++) {
	for (int j = V; j >= v[i]; j--) {
		for (int k = M; k >= m[i]; k--) {
			f[j][k] = max(f[j][k], f[j - v[i]][k - m[i]] + w[i]);
		}
	}
}
```


(7)  对比模板：

二重循环二维数组对比：

```C++
f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]); //01背包
f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]); //完全背包问题
```

三循环二维数组对比：

```C++
//完全背包
for (int k = 0; k * v[i] <= j; k++) { 
	f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
}

//分组背包
f[i][j] = f[i - 1][j];  //不选
for (int k = 0; k < s[i]; k++) {   
	if (j >= v[i][k]) f[i][j] = max(f[i][j], f[i - 1][j - v[i][k]] + w[i][k]);
}

//多重背包
for (int k = 0; k <= s[i] && k * v[i] <= j; k ++ ) //比完全背包的三重循环二维数组版本多了k <= s[i]的限制
	f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
```

(8)  背包问题求方案数

AcWing 1021 货币系统

```cpp
    cin >> n >> m;
    f[0] = 1;
    for (int i = 0; i < n; i++) {
        int v;
        cin >> v;
        for (int j = v; j <= m; j++) {
            f[j] += f[j - v]; //背包问题求方案数
        }
    }
    cout << f[m] << endl;
```

(9)  背包问题求具体方案

```cpp
#include <iostream>
#include <vector>

using namespace std;
const int N = 1010;
int f[N][N],v[N],w[N];
int n, m;

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    for (int i = n; i >= 1; i--) {
        for (int j = 0; j <= m; j++) {
            f[i][j] = f[i + 1][j];
            if (j >= v[i]) f[i][j] = max(f[i][j], f[i + 1][j - v[i]] + w[i]);
        }
    }

    int j = m;
    //追溯
    for (int i = 1; i <= n; i++) {
        if (j >= v[i] && f[i][j] == f[i + 1][j - v[i]] + w[i]) {
            cout << i << ' ';
            j -= v[i];
        }
    }
    return 0;
}
```

#### 状态机模板

画状态转换图

AcWing 1057. 股票买卖 IV

```cpp
const int N = 100010, M = 110;
int f[N][M][2], w[N];//买入作为一次交易的开始，会画状态转换图，分析状态机模型
//f[i][j][0] i天，最多j次交易(也可以小于j次)，0表示没有，1表示持有
int n, m;

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> w[i];
    }

    memset(f, -0x3f, sizeof(f));
    for (int i = 0; i <= n; i++) {
        f[i][0][0] = 0;
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            f[i][j][0] = max(f[i - 1][j][0], f[i - 1][j][1] + w[i]);
            f[i][j][1] = max(f[i - 1][j][1], f[i - 1][j - 1][0] - w[i]);
        }
    }

    int res = 0;
    for (int i = 1; i <= m; i++) {
        res = max(res, f[n][i][0]);
    }
    cout << res << endl;
}
```


#### 状态压缩DP

棋盘式

```C++
vector<int> state;   //所有合法状态
vector<int> head[M]; // vector + 数组用法，状态为state[i]时的,所有合法的转移状态
int cnt[M];  //状态M中1的数量
int n,m;
LL f[N][K][M]; //f[i][j][k]:  ixi的棋盘上，放置j个小国王,并且放置状态是state[k]

bool check(int state) {
    //有两个连续1则无效
    for(int i = 0;i < n;i++){
        if((state >> i & 1) && (state >> i + 1 & 1)){
            return false;
        }
    }
    return true;
}

int count(int state) {
    int res = 0;
    for(int i = 0;i < n;i++){
        if (state >> i & 1) {
            res++;
        }
    }
    return res;
}


int main() {
    cin >> n >> m;
    //预计算所有合法状态
    for (int i = 0; i < 1 << n; i++) {
        if (check(i)) {
            state.emplace_back(i);
            cnt[i] = count(i);
        }
    }

    //预处理前后合法状态
    for (int i = 0; i < state.size(); i++) {
        for (int j = 0; j < state.size(); j++) {
            int a = state[i], b = state[j];
            if ((a & b) == 0 && check(a | b)) {
                head[i].emplace_back(j); // i和j不矛盾，合法
            }
        }
    }

    f[0][0][0] = 1;
    for (int i = 1; i <= n + 1; i++) {
        for (int j = 0; j <= m; j++) {
            //遍历所有可能的状态a
            for (int a = 0; a < state.size(); a++) {
                //所有合法的前一个状态
                for (int b:head[a]) {
                    int c = cnt[state[a]];
                    if (j >= c) {
                        f[i][j][a] += f[i - 1][j - c][b];
                    }
                }
            }
        }
    }

    //前n行合法，第n+1没有放置
    cout << f[n + 1][m][0] << endl;

    return 0;
}
```


集合式: AcWing 524. 愤怒的小鸟


#### 区间DP 

```cpp
//模板1  
for (int len = 2; len <= n; len ++ )         //枚举长度 
    for (int l = 1; l + len - 1 <= n; l ++ ) //枚举起点
    {
        int r = l + len - 1;
        f[l][r] = INF;
        for (int k = l; k < r; k ++ )        //枚举分割点
            f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
    }
	

//模板2	
memset(f, 0x3f3f3f, sizeof(f));
for(int i = 0;i <= n;i++){
	f[i][i] = 0;
}

for (int len = 2; len <= n; len++) {
	for (int l = 1; l + len - 1 <= n; l++) {
		int r = l + len - 1;
		for (int k = l; k < r; k++) {
			f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
		}
	}
}	
```

```cpp
//模板3
//记忆化搜索lyd解法
int solve(int l, int r){
	if(l == r) return 1;
	if(l > r) return 0;
	if(f[l][r] != -1) return f[l][r];
	f[l][r] = 0;
	for(int k = l + 2;k <= r;++k)      //枚举第一棵子树在s中划分的结束位置
		if(s[k] == s[l])
			f[l][r] = (f[l][r] + (ll)solve(l+1, k-1)*solve(k, r)) % mod;
	return f[l][r];
}
```


#### 树形DP

(1)  树形DP + 状态机

AcWing 285. 没有上司的舞会

DP的阶段：一般以节点从深到浅(子树从小的到大)的顺序作为DP的阶段  

DP的状态计算：第一维是节点编号(代表该节点为根的子树),第二位是状态机的状态;递归的方式实现树形DP,先递归在它的每个子节点上进行DP,回溯时，从子节点向节点x进行状态转移。     

```cpp
void add(int a,int b){
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void dfs(int u){
    f[u][1] = w[u];
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        dfs(j);
        f[u][0] += max(f[j][0], f[j][1]);
        f[u][1] += f[j][0];
    }
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> w[i];
    }
    memset(h,-1,sizeof(h));
    for (int i = 0; i < n - 1; i++) {
        int a, b;
        cin >> a >> b;
        add(b, a);
        st[a] = true;
    }

    //寻找root
    int root = 1;
    while (st[root]) root++;

    //dfs从根开始
    dfs(root);

    cout << max(f[root][0], f[root][1]);
    return 0;
}
```

(2) 最长路+次长路模板

AcWing 1072. 树的最长路径

```cpp
void add(int a,int b,int c){
    //有一条b指向b,它的权值是c,它插在了h[a]的头部,h[a]指向了它，自加。
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

int dfs(int u, int father) {
    int dist = 0; //从当前点向下走的最大长度
    int d1 = 0, d2 = 0;
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (j == father) continue;
        int d = dfs(j, u) + w[i];
        dist = max(dist, d);
        if (d >= d1) d2 = d1, d1 = d;
        else if (d > d2) d2 = d;
    }
    res = max(res, d1 + d2);
    return dist;
}
```



#### 数位DP简化模板

AcWing 338 计数问题

```cpp
#include <iostream>
#include <cmath>

using namespace std;

int dgt(int n) {
    int res = 0;
    while (n) {
        res++;
        n /= 10;
    }
    return res;
}

int count(int n,int i){
    int res = 0, d = dgt(n);
    for (int j = 1; j <= d; j++) {
        // % 取低位， / 取高位
        // 取某一位，先截断取高位，再 % 取低位。
        int p = pow(10, j - 1), l = n / p / 10, r = n % p, dj = n / p % 10;
        if (i) res += l * p;
        if (!i && l) res += (l - 1) * p; //首位不能全零，l - 1

        if (dj > i && (i || l)) res += p;
        if (dj == i) res += r + 1; //如果dj = i = 0,此时肯定不是高位。
    }
    return res;
}


int main() {
    int a, b;
    while (cin >> a >> b, a) {
        if (a > b) swap(a, b);
        for (int i = 0; i <= 9; i++) {
            cout << count(b, i) - count(a - 1, i) << " ";
        }
        cout << endl;
    }

    return 0;
}
```


#### 单调队列优化DP 

（1）求max用单调递减队列(可以取队头)，求min用单调递增队列。  

```C++
for (int i = 1; i <= n; i ++ )
{
	if (q[hh] < i - m) hh ++ ;
	res = max(res, s[i] - s[q[hh]]);
	while (hh <= tt && s[q[tt]] >= s[i]) tt -- ;
	q[ ++ tt] = i;
}
```

（2） 环形DP + 单调队列
```cpp
deque<int> q;
int w[N];
int n;

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> w[i];
        w[i + n] = w[i];
    }

    int len = n / 2;
    int res = 0;
    q.push_back(1);
    for (int i = 2; i <= 2 * n; i++) {
        if (q.size() && len < i - q.front()) q.pop_front();
        res = max(res, w[i] + i + w[q.front()] - q.front()); //Aj - j的最大值
        while (q.size() && w[q.back()] - q.back() <= w[i] - i) q.pop_back();
        q.push_back(i);
    }
    
    printf("%d\n",res);
    return 0;
}
```

(3) 典型一维问题

AcWing 135. 最大子序和

```cpp
for(int i = 1;i <= n;i++){
    cin >> s[i];
    s[i] += s[i - 1];
}

int hh = 0, tt = 0; //初始时队列不为空，包含一个0,相当于哨兵0
int res = -1e9;
for (int i = 1; i <= n; i++) {
    if (q[hh] < i - m) hh++;
    res = max(res, s[i] - s[q[hh]]);
    //取负队头，单调上升序列
    while (hh <= tt && s[q[tt]] >= s[i]) tt--;
    q[++tt] = i;
}
cout << res << endl;
```

AcWing 1089. 烽火传递

```cpp
int hh = 0, tt = 0; //初始的0在
for (int i = 1; i <= n; i++) {
    if (q[hh] < i - m) hh++;
    f[i] = f[q[hh]] + w[i];
    //队头取min,单调上升
    while (hh <= tt && f[q[tt]] >= f[i]) tt--;
    q[++tt] = i;
}

//f[i]表示前1~i座烽火台满足条件，且第i座烽火台点燃的方案集合
int res = 1e9;
for(int i = n - m + 1;i <= n;i++) res = min(res,f[i]);
cout << res << endl;
```

(4) 二维滑窗模板

```cpp
void get_min(int a[], int b[], int tot) {
    int hh = 0, tt = -1;
    for (int i = 1; i <= tot; i++) {
        if (hh <= tt && q[hh] <= i - k) hh++;
        //队头最小值，单调上升队列
        while (hh <= tt && a[q[tt]] >= a[i]) tt--;
        q[++tt] = i;
        b[i] = a[q[hh]];
    }
}

void get_max(int a[], int b[], int tot) {
    int hh = 0, tt = -1;
    for (int i = 1; i <= tot; i++) {
        if (hh <= tt && q[hh] <= i - k) hh++;
        //队头最大值，单调下降队列
        while (hh <= tt && a[q[tt]] <= a[i]) tt--;
        q[++tt] = i;
        b[i] = a[q[hh]];
    }
}

for (int i = 1; i <= n; i++) {
    get_min(w[i], row_min[i], m);
    get_max(w[i], row_max[i], m);
}

int res = INF;
int a[N], b[N], c[N];
for (int j = k; j <= m; j++) {
    for (int i = 1; i <= n; i++) {
        a[i] = row_min[i][j];
    }
    get_min(a, b, n);

    for (int i = 1; i <= n; i++) {
        a[i] = row_max[i][j];
    }
    get_max(a, c, n);

    for (int i = k; i <= n; i++) {
        res = min(res, c[i] - b[i]);
    }
}
```


## 0x60 图论

#### 朴素dijkstra算法

模板题 AcWing 849. Dijkstra求最短路 I

时间复杂是 O(n2+m), n 表示点数，m 表示边数

```cpp
int g[N][N];  // 存储每条边
int dist[N];  // 存储1号点到每个点的最短距离
bool st[N];   // 存储每个点的最短路是否已经确定

// 求1号点到n号点的最短路，如果不存在则返回-1
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    for (int i = 0; i < n - 1; i ++ )
    {
        int t = -1;     // 在还未确定最短路的点中，寻找距离最小的点
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        // 用t更新其他点的距离
        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);

        st[t] = true;
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```

#### 堆优化版dijkstra

模板题 AcWing 850. Dijkstra求最短路 II

时间复杂度 O(mlogn), n 表示点数，m 表示边数

```cpp
typedef pair<int, int> PII;

int n;      // 点的数量
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储所有点到1号点的距离
bool st[N];     // 存储每个点的最短距离是否已确定

// 求1号点到n号点的最短距离，如果不存在，则返回-1
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});      // first存储距离，second存储节点编号

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.second, distance = t.first;

        if (st[ver]) continue;
        st[ver] = true;

        for (int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > distance + w[i])
            {
                dist[j] = distance + w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```

#### Bellman-Ford算法 

模板题 AcWing 853. 有边数限制的最短路

时间复杂度 O(nm)O(nm), nn 表示点数，mm 表示边数

```cpp
注意在模板题中需要对下面的模板稍作修改，加上备份数组，详情见模板题。

int n, m;       // n表示点数，m表示边数
int dist[N];        // dist[x]存储1到x的最短路距离

struct Edge     // 边，a表示出点，b表示入点，w表示边的权重
{
    int a, b, w;
}edges[M];

// 求1到n的最短路距离，如果无法从1走到n，则返回-1。
int bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    // 如果第n次迭代仍然会松弛三角不等式，就说明存在一条长度是n+1的最短路径，由抽屉原理，路径中至少存在两个相同的点，说明图中存在负权回路。
    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0; j < m; j ++ )
        {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            if (dist[b] > dist[a] + w)
                dist[b] = dist[a] + w;
        }
    }

    if (dist[n] > 0x3f3f3f3f / 2) return -1;
    return dist[n];
}
```

#### spfa 算法（队列优化的Bellman-Ford算法）**

模板题 AcWing 851. spfa求最短路

时间复杂度 平均情况下 O(m)O(m)，最坏情况下 O(nm)O(nm), nn 表示点数，mm 表示边数

```cpp
int n;      // 总点数
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储每个点到1号点的最短距离
bool st[N];     // 存储每个点是否在队列中

// 求1号点到n号点的最短路距离，如果从1号点无法走到n号点则返回-1
int spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    queue<int> q;
    q.push(1);
    st[1] = true;

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                if (!st[j])     // 如果队列中已存在j，则不需要将j重复插入
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```

#### spfa判断图中是否存在负环

模板题 AcWing 852. spfa判断负环

时间复杂度是 O(nm), n 表示点数，m 表示边数

Bellman-Ford 算法判断负环: 若经过n 轮迭代，算法仍未结束（仍有能力产生更新的边），则图中存在负环.

1 设cnt[N]表示从1到 x 的最短路径包含的边数，cnt[1]=0 。当执行更新 dist[y]=dist[x]+z 时，同样更新cnt[y]=cnt[x]+1 。若发现cnt[y]≥n ，则图中有负环。若算法正常结束，则图中没有负环。 

2 统计每个点入队的次数，如果某个点入队 n 次，则存在负环。

```cpp
int n;      // 总点数
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N], cnt[N];        // dist[x]存储1号点到x的最短距离，cnt[x]存储1到x的最短路中经过的点数
bool st[N];     // 存储每个点是否在队列中

// 如果存在负环，则返回true，否则返回false。
bool spfa()
{
    // 不需要初始化dist数组
    // 原理：如果某条最短路径上有n个点（除了自己），那么加上自己之后一共有n+1个点，由抽屉原理一定有两个点相同，所以存在环。

    queue<int> q;
    for (int i = 1; i <= n; i ++ )
    {
        q.push(i);
        st[i] = true;
    }

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true;       // 如果从1号点到x的最短路中包含至少n个点（不包括自己），则说明存在环
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }

    return false;
}
```

#### floyd算法 

模板题 AcWing 854. Floyd求最短路

时间复杂度是 O(n3), n 表示点数

```cpp

初始化：
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            if (i == j) d[i][j] = 0;
            else d[i][j] = INF;

// 算法结束后，d[a][b]表示a到b的最短距离
void floyd()
{
    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
```

AcWing343 排序:传递闭包

```cpp
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                d[i][j] |= d[i][k] && d[k][j];
            }
        }
    }
```

#### 差分约束

差分约束可以求什么:1 求不等式组的可行解，2 求最大值或者最小值

(1)如何求不等式组的可行解

形如:xi <= xj + ck的不等式，其中xi、xj为自变量，ck为常量，可以看成j-->i(c)的边，对于每一条边都会满足dist[i] <= dist[j] + c

满足条件: 从源点出发,一定可以走到所有的边。

1 先将每个不等式xi <= xj + Ck的一条边转换成一条从xj走到xi，长度为Ck的一条边。
2 找一个超级源点,使得该源点一定可以遍历到所有边。
3 从源点求一遍单源最短路。
若存在负环则存在矛盾,
结果1:如果存在负环,则原不等式组一定无解
结果2:如果没有负环,则dist[i]就是原不等式组的一个可行解
(2)如何求最大值或者最小值(每个变量的最值)

结论：如果求的是最小值,所有下界里面找最大的，则应该求最长路;如果求的是最大值,所有上界里面找最小的，则应该求最短路。

问题:如何转化xi<=c,其中c是一个常数,这类不等式：

方法：建立一个超级源点0，xi<=c 转换为xi<=x0+c，然后建立0-->i，长度为c的边即可。

以求xi最大值为例:从所有xi出发,构成不等式链xi<=xj+c1<xk+c1+c2+c3+...所计算出的上界,最终xi的最大值等于所有上界的最小值。

每个链式不等式都是从0出发走到i的一条路径。


AcWing 1169. 糖果

```cpp
A=B　　->　A>=B, B>= A
A < B　->　B >= A + 1
A>=B -> A >= B
A>B　　->　A >= B + 1
A<=B　 ->　B >= A
x>=1 -> x1>=x0+1

添加0号点，虚拟源点;

从0号点求一遍单源最长路

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

bool spfa()
{
    //求最长边
    memset(dist, -0x3f, sizeof(dist));

    int hh = 0, tt = 0;
    dist[0] = 0;
    q[tt++] = 0;
    st[0] = true;

    while (hh != tt)
    {
        auto t = q[--tt]; //求负环超时，将队列改成栈
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            auto j = e[i];
            if (dist[j] < dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] > n + 1)
                    return false;
                if (!st[j])
                {
                    q[tt++] = j;
                    st[j] = true;
                }
            }
        }
    }

    return true;
}

int main()
{
    memset(h, -1, sizeof(h));
    cin >> n >> m;
    for (int i = 0; i < m; i++)
    {
        int x,a,b;
        cin >> x >> a >> b;
        if(x == 1) add(a,b,0), add(b,a,0);
        else if(x == 2) add(a,b,1);
        else if(x == 3) add(b,a,0);
        else if(x == 4) add(b,a,1);
        else add(a,b,0);
    }

    for(int i = 1;i <= n;i++) add(0,i,1);

    if (!spfa())
        cout << "-1" << endl;
    else{
        LL res = 0;
        for(int i = 1;i <= n;i++){
            res += dist[i];
        }
        cout << res << endl;
    }

    return 0;
}
```

#### 倍增求LCA

1 向上标记法 O(n)

2 倍增：

fa[i,j]表示从i开始，向上走2^j步所能走到的节点，0 <=j <=logn,depth[i]表示深度。

哨兵：如果从i开始跳^j步，会跳过根节点，则fa[i,j] = 0,depth[0] = 0

步骤: 1 先将两个点跳到同一层 2 让两个点同时往上跳，一直跳到他们的最近公共祖先的下一层。

二进制拼凑的思想， j=0,f[i][i]的父节点；j>0,f[i][j] = f[f[i][j]][j-1]

Lca 可以加速最小生成树的效率。

预处理 O(logn) 

查询O(logn)

3 Tarjan-离线求LCA O(n+m)

在深度优先遍历时，将所有点分成三大类：a 已经遍历过，且回溯过的点 b 正在搜索的分支 c 还未搜索到的点。

AcWing 1172. 祖孙询问

```C++
void bfs(int root)  // 预处理倍增数组
{
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0, depth[root] = 1;  // depth存储节点所在层数
    int hh = 0, tt = 0;
    q[0] = root;
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (depth[j] > depth[t] + 1)
            {
                depth[j] = depth[t] + 1;
                q[ ++ tt] = j;
                fa[j][0] = t;  // j的第二次幂个父节点
                for (int k = 1; k <= 15; k ++ )
                    fa[j][k] = fa[fa[j][k - 1]][k - 1];
            }
        }
    }
}

int lca(int a, int b)  // 返回a和b的最近公共祖先
{
    if (depth[a] < depth[b]) swap(a, b);
    for (int k = 15; k >= 0; k -- )
        if (depth[fa[a][k]] >= depth[b])
            a = fa[a][k];
    if (a == b) return a;
    for (int k = 15; k >= 0; k -- )
        if (fa[a][k] != fa[b][k])
        {
            a = fa[a][k];
            b = fa[b][k];
        }
    return fa[a][0];
}

```

#### 邻接表tarjan-scc
```C++
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;  // 时间戳
int stk[N], top;
bool in_stk[N];
int id[N], scc_cnt;  // 每个点所属分量编号

```

#### tarjan - 有向图强连通分量

```C++
void tarjan(int u)
{
    dfn[u] = low[u] = ++ timestamp;
    stk[ ++ top] = u, in_stk[u] = true;
    
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (!dfn[j])
        {
            tarjan(j);
            low[u] = min(low[u], low[j]);
        }
        else if (in_stk[j])
            low[u] = min(low[u], dfn[j]);
    }
    
    if (dfn[u] == low[u])
    {
        ++ scc_cnt;
        int y;
        do {
            y = stk[top -- ];
            in_stk[y] = false;
            id[y] = scc_cnt;
        } while (y != u);
    }
}
```

#### 邻接表tarjan-v-dcc
```C++
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;  // 时间戳
int stk[N], top;
int dcc_cnt;
vector<int> dcc[N];  // 每个分量有哪些点
bool cut[N];  // 是否为割点
int root;
```


#### tarjan - v-无向图双连通分量

```C++
void tarjan(int u)
{
    dfn[u] = low[u] = ++ timestamp;
    stk[ ++ top] = u;
    
    if (u == root && h[u] == -1)
    {
        dcc_cnt ++ ;
        dcc[dcc_cnt].push_back(u);
        return;
    }
    
    int cnt = 0;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (!dfn[j])
        {
            tarjan(j);
            low[u] = min(low[u], low[j]);
            if (dfn[u] <= low[j])
            {
                cnt ++ ;
                if (u != root || cnt > 1) cut[u] = true;
                ++ dcc_cnt;
                int y;
                do {
                    y = stk[top -- ];
                    dcc[dcc_cnt].push_back(y);
                } while (y != j);
                dcc[dcc_cnt].push_back(u);
            }
        }
        else low[u] = min(low[u], dfn[j]);
    }
}
```

#### 邻接表tarjan-e-dcc
```C++
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;  // 时间戳
int stk[N], top;
int id[N], dcc_cnt;  // 每个点所属分量编号
bool is_bridge[M];

```

####  tarjan - e-无向图双连通分量	
```C++
void tarjan(int u, int from)
{
    dfn[u] = low[u] = ++ timestamp;
    stk[ ++ top] = u;
    
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (!dfn[j])
        {
            tarjan(j, i);
            low[u] = min(low[u], low[j]);
            if (dfn[u] < low[j])
                is_bridge[i] = is_bridge[i ^ 1] = true;
        }
        else if (i != (from ^ 1))
            low[u] = min(low[u], dfn[j]);
    }
    
    if (dfn[u] == low[u])
    {
        ++ dcc_cnt;
        int y;
        do {
            y = stk[top -- ];
            id[y] = dcc_cnt;
        } while (y != u);
    }
}

```


#### 朴素版prim算法 

模板题 AcWing 858. Prim算法求最小生成树

时间复杂度是 O(n2+m), n 表示点数，m 表示边数

朴素Prim求解最小生成树的方法与Dijkstra算法很相似，可以参考学习。

朴素Prim算法通过n次遍历来确定n个点，首先将寻找到的距离集合最近的点，将这个点加入集合，再拿这个点来更新不在集合中的点离集合距离。每次遍历是先找点，再更新其他点的距离，这与Dijkstra算法相同。

堆优化版的Prim算法和堆优化的Dijkstra算法也很相似，时间复杂度也相同。所以一般在求解稀疏图时，不用堆优化的Prim,而是用Kruskal算法，Kruskal算法的时间复杂度为O(mlogm)与O(mlogn)差不多，所以在求解稀疏图时用Kruskal算法。

```cpp
int n;      // n表示点数
int g[N][N];        // 邻接矩阵，存储所有边
int dist[N];        // 存储其他点到当前最小生成树的距离
bool st[N];     // 存储每个点是否已经在生成树中


// 如果图不连通，则返回INF(值是0x3f3f3f3f), 否则返回最小生成树的树边权重之和
int prim()
{
    memset(dist, 0x3f, sizeof dist);

    int res = 0;
    for (int i = 0; i < n; i ++ )
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        if (i && dist[t] == INF) return INF;

        if (i) res += dist[t];
        st[t] = true;

        for (int j = 1; j <= n; j ++ ) dist[j] = min(dist[j], g[t][j]);
    }

    return res;
}
```

#### Kruskal算法

模板题 AcWing 859. Kruskal算法求最小生成树

时间复杂度是 O(mlogm), n 表示点数，m 表示边数

```cpp
int n, m;       // n是点数，m是边数
int p[N];       // 并查集的父节点数组

struct Edge     // 存储边
{
    int a, b, w;

    bool operator< (const Edge &W)const
    {
        return w < W.w;
    }
}edges[M];

int find(int x)     // 并查集核心操作
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int kruskal()
{
    sort(edges, edges + m);

    for (int i = 1; i <= n; i ++ ) p[i] = i;    // 初始化并查集

    int res = 0, cnt = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = edges[i].a, b = edges[i].b, w = edges[i].w;

        a = find(a), b = find(b);
        if (a != b)     // 如果两个连通块不连通，则将这两个连通块合并
        {
            p[a] = b;
            res += w;
            cnt ++ ;
        }
    }

    if (cnt < n - 1) return INF;
    return res;
}
```

#### 染色法判别二分图 

模板题 AcWing 860. 染色法判定二分图

时间复杂度是 O(n+m), n 表示点数，m 表示边数

```cpp
int n;      // n表示点数
int h[N], e[M], ne[M], idx;     // 邻接表存储图
int color[N];       // 表示每个点的颜色，-1表示未染色，0表示白色，1表示黑色

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

//u节点,c颜色
//0未图色，1白色，2黑色
bool dfs(int u, int c) {
    color[u] = c;
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (!color[j]) {
            if (!dfs(j, 3 - c)) return false;
        } else if (color[j] == c) {
            return false;
        }
    }
    return true;
}

int main() {

    cin >> n >> m;
    memset(h, -1, sizeof(h));
    while (m--) {
        int a, b;
        cin >> a >> b;
        add(a, b);
    }

    bool flag = true;
    for (int i = 1; i <= n; i++) {
        if (!color[i]) {
            if (!dfs(i, 1)) {
                flag = false;
                break;
            }
        }
    }
    if (flag) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```

#### 匈牙利算法

模板题 AcWing 861. 二分图的最大匹配

时间复杂度是 O(nm), n 表示点数，m 表示边数

```cpp
int n1, n2;     // n1表示第一个集合中的点数，n2表示第二个集合中的点数
int h[N], e[M], ne[M], idx;     // 邻接表存储所有边，匈牙利算法中只会用到从第一个集合指向第二个集合的边，所以这里只用存一个方向的边
int match[N];       // 存储第二个集合中的每个点当前匹配的第一个集合中的点是哪个
bool st[N];     // 表示第二个集合中的每个点是否已经被遍历过

bool find(int x)
{
    for (int i = h[x]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true;
            if (match[j] == 0 || find(match[j]))
            {
                match[j] = x;
                return true;
            }
        }
    }

    return false;
}

// 求最大匹配数，依次枚举第一个集合中的每个点能否匹配第二个集合中的点
int res = 0;
for (int i = 1; i <= n1; i ++ )
{
    memset(st, false, sizeof st);
    if (find(i)) res ++ ;
}
```

#### 拓扑排序 

模板题 AcWing 848. 有向图的拓扑序列

时间复杂度 O(n+m), n 表示点数，m 表示边数

```cpp
bool topsort()
{
    int hh = 0, tt = -1;

    // d[i] 存储点i的入度
    for (int i = 1; i <= n; i ++ )
        if (!d[i])
            q[ ++ tt] = i;

    while (hh <= tt)
    {
        int t = q[hh ++ ];

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (-- d[j] == 0)
                q[ ++ tt] = j;
        }
    }

    // 如果所有点都入队了，说明存在拓扑序列；否则不存在拓扑序列。
    return tt == n - 1;
}
```

#### 加边，最大流
```C++
void add(int a, int b, int c)  // 添加一条边a->b，容量为c；同时添加一条反向边
{
    e[idx] = b, f[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
    e[idx] = a, f[idx] = 0, ne[idx] = h[b], h[b] = idx ++ ;
}

```

#### 邻接表最大流
```C++
int h[N], e[M], f[M], ne[M], idx;
int q[N], d[N], pre[N];
bool st[N];
```

#### 最大流
```C++
bool bfs()  // 创建分层图
{
    int hh = 0, tt = 0;
    memset(d, -1, sizeof d);
    q[0] = S, d[S] = 0, cur[S] = h[S];
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int ver = e[i];
            if (d[ver] == -1 && f[i])
            {
                d[ver] = d[t] + 1;
                cur[ver] = h[ver];
                if (ver == T) return true;
                q[ ++ tt] = ver;
            }
        }
    }
    return false;  // 没有增广路
}

int find(int u, int limit)  // 在残留网络中增广
{
    if (u == T) return limit;
    int flow = 0;
    for (int i = cur[u]; ~i && flow < limit; i = ne[i])
    {
        cur[u] = i;  // 当前弧优化
        int ver = e[i];
        if (d[ver] == d[u] + 1 && f[i])
        {
            int t = find(ver, min(f[i], limit - flow));
            if (!t) d[ver] = -1;
            f[i] -= t, f[i ^ 1] += t, flow += t;
        }
    }
    return flow;
}

int dinic()
{
    int r = 0, flow;
    while (bfs()) while (flow = find(S, INF)) r += flow;
    return r;
}

```

#### 加边，费用流
```C++
void add(int a, int b, int c, int d)  // 添加一条边a->b，容量为c，费用为d；同时增加一条反向边
{
    e[idx] = b, f[idx] = c, w[idx] = d, ne[idx] = h[a], h[a] = idx ++ ;
    e[idx] = a, f[idx] = 0, w[idx] = -d, ne[idx] = h[b], h[b] = idx ++ ;
}
```

#### 邻接表费用流
```C++
int h[N], e[M], f[M], w[M], ne[M], idx;
int q[N], d[N], pre[N], incf[N];
bool st[N];

```

#### 费用流
```C++
bool spfa()  // 找距离最短的增广路
{
    int hh = 0, tt = 1;
    memset(d, 0x3f, sizeof d);
    q[0] = S, d[S] = 0, incf[S] = INF;
    while (hh != tt)
    {
        int t = q[hh ++ ];
        if (hh == N) hh = 0;
        st[t] = false;
        for (int i = h[t]; ~i; i = ne[i])
        {
            int ver = e[i];
            if (f[i] && d[ver] > d[t] + w[i])
            {
                d[ver] = d[t] + w[i];
                pre[ver] = i;
                incf[ver] = min(f[i], incf[t]);
                if (!st[ver])
                {
                    q[tt ++ ] = ver;
                    if (tt == N) tt = 0;
                    st[ver] = true;
                }
            }
        }
    }
    return d[T] != INF;
}

void cost_flow(int& flow, int& cost)
{
    flow = cost = 0;  // flow存储流量，cost存储费用
    while (spfa())
    {
        int t = incf[T];
        flow += t, cost += t * d[T];
        for (int i = T; i != S; i = e[pre[i] ^ 1])
        {
            f[pre[i]] -= t;
            f[pre[i] ^ 1] += t;
        }
    }
}
```
#### 费用流
```C++
bool spfa()  // 找距离最短的增广路
{
    int hh = 0, tt = 1;
    memset(d, 0x3f, sizeof d);
    q[0] = S, d[S] = 0, incf[S] = INF;
    while (hh != tt)
    {
        int t = q[hh ++ ];
        if (hh == N) hh = 0;
        st[t] = false;
        for (int i = h[t]; ~i; i = ne[i])
        {
            int ver = e[i];
            if (f[i] && d[ver] > d[t] + w[i])
            {
                d[ver] = d[t] + w[i];
                pre[ver] = i;
                incf[ver] = min(f[i], incf[t]);
                if (!st[ver])
                {
                    q[tt ++ ] = ver;
                    if (tt == N) tt = 0;
                    st[ver] = true;
                }
            }
        }
    }
    return d[T] != INF;
}

int cost_flow()
{
    int cost = 0;  // 存储费用
    while (spfa())
    {
        int t = incf[T];
        cost += t * d[T];
        for (int i = T; i != S; i = e[pre[i] ^ 1])
        {
            f[pre[i]] -= t;
            f[pre[i] ^ 1] += t;
        }
    }
    return cost;
}
```



## 0x70 计算几何

#### 计算几何常用函数
```C++
int sign(double x)  // 符号函数
{
    if (fabs(x) < eps) return 0;  // x为0，则返回0
    if (x < 0) return -1;  // x为负数，则返回-1
    return 1;  // x为正数，则返回1
}

int dcmp(double x, double y)  // 比较两数大小
{
    if (fabs(x - y) < eps) return 0;  // x == y, 返回0
    if (x < y) return -1;  // x < y, 返回-1
    return 1;  // x > y, 返回1
}

PDD operator+ (PDD a, PDD b)  // 向量加法
{
    return {a.x + b.x, a.y + b.y};
}

PDD operator- (PDD a, PDD b)  //  向量减法
{
    return {a.x - b.x, a.y - b.y};
}

PDD operator* (PDD a, double t)  // 向量数乘
{
    return {a.x * t, a.y * t};
}

PDD operator/ (PDD a, double t)  // 向量除以常数
{
    return {a.x / t, a.y / t};
}

double operator* (PDD a, PDD b)  // 外积、叉积
{
    return a.x * b.y - a.y * b.x;
}

double operator& (PDD a, PDD b)  // 内积、点积
{
    return a.x * b.x + a.y * b.y;
}

double area(PDD a, PDD b, PDD c)  // 以a, b, c为顶点的有向三角形面积
{
    return (b - a) * (c - a);
}

double get_len(PDD a)  // 求向量长度
{
    return sqrt(a & a);
}

double get_dist(PDD a, PDD b)  // 求两个点之间的距离
{
    return get_len(b - a);
}

double project(PDD a, PDD b, PDD c)  // 求向量ac在向量ab上的投影
{
    return ((c - a) & (b - a)) / get_len(b - a);
}

PDD rotate(PDD a, double b)  // 向量a逆时针旋转角度b
{
    return {a.x * cos(b) + a.y * sin(b), -a.x * sin(b) + a.y * cos(b)};
}

PDD norm(PDD a)  // 矩阵标准化（将长度变成1）
{
    return a / get_len(a);
}

bool on_segment(PDD p, PDD a, PDD b)  // 点p是否在线段ab上（包含端点a、b）
{
    return !sign((p - a) * (p - b)) && sign((p - a) & (p - b)) <= 0;
}

PDD get_line_intersection(PDD p, PDD v, PDD q, PDD w)  // 求两直线交点：p + vt, q + wt
{
    auto u = p - q;
    auto t = w * u / (v * w);
    return p + v * t;
}

```

#### 求凸包
```C++
int stk[N], top;
PDD q[N];
bool used[N];

PDD operator- (PDD a, PDD b)  // 向量减法
{
    return {a.x - b.x, a.y - b.y};
}

double operator* (PDD a, PDD b)  // 叉积、外积
{
    return a.x * b.y - a.y * b.x;
}

double operator& (PDD a, PDD b)  // 内积、点积
{
    return a.x * b.x + a.y * b.y;
}

double area(PDD a, PDD b, PDD c)  // 以a, b, c为顶点的有向三角形面积
{
    return (b - a) * (c - a);
}

double get_len(PDD a)  // 求向量长度
{
    return sqrt(a & a);
}

double get_dist(PDD a, PDD b)  // 求两个点之间的距离
{
    return get_len(b - a);
}

void andrew()  // Andrew算法, 凸包节点编号逆时针存于stk中，下标从0开始
{
    sort(q, q + n);
    for (int i = 0; i < n; i ++ )
    {
        while (top >= 2 && area(q[stk[top - 2]], q[stk[top - 1]], q[i]) <= 0)
        {
            if (area(q[stk[top - 2]], q[stk[top - 1]], q[i]) < 0)
                used[stk[ -- top]] = false;
            else
                top -- ;
        }
        stk[top ++ ] = i;
        used[i] = true;
    }
    used[0] = false;
    for (int i = n - 1; i >= 0; i -- )
    {
        if (used[i]) continue;
        while (top >= 2 && area(q[stk[top - 2]], q[stk[top - 1]], q[i]) <= 0)
            top -- ;
        stk[top ++ ] = i;
    }
    top -- ;  // 起点重复添加了一次，将其去掉
}

```

#### 求半平面交

```C++
struct Line  // 直线
{
    PDD st, ed;  // 直线上的两个点
}line[N];
int q[N];  // 双端队列

int sign(double x)  // 符号函数
{
    if (fabs(x) < eps) return 0;  // x为0，则返回0
    if (x < 0) return -1;  // x为负数，则返回-1
    return 1;  // x为正数，则返回1
}

int dcmp(double x, double y)  // 比较两数大小
{
    if (fabs(x - y) < eps) return 0;  // x == y, 返回0
    if (x < y) return -1;  // x < y, 返回-1
    return 1;  // x > y, 返回1
}

PDD operator+ (PDD a, PDD b)  // 向量加法
{
    return {a.x + b.x, a.y + b.y};
}

PDD operator-(PDD a, PDD b)  // 向量减法
{
    return {a.x - b.x, a.y - b.y};
}

double operator* (PDD a, PDD b)  // 外积、叉积
{
    return a.x * b.y - a.y * b.x;
}

PDD operator* (PDD a, double t)  // 向量数乘
{
    return {a.x * t, a.y * t};
}

double area(PDD a, PDD b, PDD c)  // 以a, b, c为顶点的有向三角形面积
{
    return (b - a) * (c - a);
}

PDD get_line_intersection(PDD p, PDD v, PDD q, PDD w)  // 求两直线交点：p + vt, q + wt
{
    auto u = p - q;
    auto t = w * u / (v * w);
    return p + v * t;
}

PDD get_line_intersection(Line a, Line b)  // 求两直线交点
{
    return get_line_intersection(a.st, a.ed - a.st, b.st, b.ed - b.st);
}

bool on_right(Line& a, Line& b, Line& c) // bc的交点是否在a的右侧
{
    auto o = get_line_intersection(b, c);
    return sign(area(a.st, a.ed, o)) <= 0;
}

double get_angle(const Line& a)  // 求直线的极角大小
{
    return atan2(a.ed.y - a.st.y, a.ed.x - a.st.x);
}

bool cmp(const Line& a, const Line& b)  // 将所有直线按极角排序
{
    double A = get_angle(a), B = get_angle(b);
    if (!dcmp(A, B)) return area(a.st, a.ed, b.ed) < 0;
    return A < B;
}

void half_plane_intersection()  // 半平面交，交集的边逆时针顺序存于q[]中
{
    sort(line, line + cnt, cmp);
    int hh = 0, tt = -1;
    for (int i = 0; i < cnt; i ++ )
    {
        if (i && !dcmp(get_angle(line[i]), get_angle(line[i - 1]))) continue;
        while (hh + 1 <= tt && on_right(line[i], line[q[tt - 1]], line[q[tt]])) tt -- ;
        while (hh + 1 <= tt && on_right(line[i], line[q[hh]], line[q[hh + 1]])) hh ++ ;
        q[ ++ tt] = i;
    }
    while (hh + 1 <= tt && on_right(line[q[hh]], line[q[tt - 1]], line[q[tt]])) tt -- ;
    while (hh + 1 <= tt && on_right(line[q[tt]], line[q[hh]], line[q[hh + 1]])) hh ++ ;
    
    q[ ++ tt] = q[hh];
    // 交集的边逆时针顺序存于q[]中
    
    // TODO: 求出半平面交后，根据题目要求求答案
}
```


#### 辛普森积分

```C++
double f(double x)
{
    // TODO: 实现所求的函数
}

double simpson(double l, double r)  // 辛普森积分公式
{
    auto mid = (l + r) / 2;
    return (r - l) * (f(l) + 4 * f(mid) + f(r)) / 6;
}

double asr(double l, double r, double s)  // 自适应
{
    auto mid = (l + r) / 2;
    auto left = simpson(l, mid), right = simpson(mid, r);
    if (fabs(left + right - s) < eps) return left + right;
    return asr(l, mid, left) + asr(mid, r, right);
}
```
