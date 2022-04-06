
牛蛙点点提高课题解：https://www.acwing.com/user/myspace/activity/52559/


#### AC自动机补充

ac自动机：trie + kmp

trie类比数组，每个点有一个next数组，kmp每次匹配一个串，ac自动机每层匹配一堆串。

f[i][j]前i个字母，当前走到了ac自动机的第j个位置的所有操作方案中，最少修改字母的数量。

AcWing 1053 修复DNA： Ac自动机= Trie + KMP

#### 树状数组

树状数组学习笔记：https://www.acwing.com/blog/content/80/

难写难调：线段树、splay 平衡树、块状链表。    
能用树状数组的题：单点更新 + 维护前缀“操作和”(前缀和，前缀积，前缀最大最小)   
树状数组的建立方式，复杂度nlogn     

```C++
int lowbit(int x)
{
    return x & -x;
}

void add(int x, int c)
{
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
}

int sum(int x)
{
    int res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}
```

- AcWing 241 楼兰图腾：区间加，求单点和。
- AcWing 242 一个简单的整数问题1 ：区间加，求单点和，借助差分。
- AcWing 243 一个简单的整数问题2 ：区间加，区间求和，差分b,维护两个前缀和bi、i/*bi。
- AcWing 244 谜一样的牛: 1 从剩余的数中，找到第K小的数。 2 删除某个数，a1~an=1表示当前数可用，等价于找一个最小的x,sum(x)=k，二分查找。

#### 线段树

线段树总结: https://www.acwing.com/blog/content/3369/      
走近ZWK(张昆玮) 线段树（一）：https://zhuanlan.zhihu.com/p/29876526     

树状数组只能维护前缀“操作和”(前缀和，前缀积，前缀最大最小)，而线段树可以维护区间操作和。  
线段树直接维护的就是区间信息，所以一切满足结合律的操作都能维护区间和，并且lazy标记的存在还能使线段树能够支持区间修改。

5个基本操作：build、modify、query、pushUP(子到父)、pushDown(区间修改，父到子)。
分裂最多两条链，每条logn级别，最多4logn。  

```C++
struct Tree{
    int l, r;    //表示该节点区间左右边界
    int val;   // 代表该区间特征的某个值 ，如最大值，区间和等等。。。。
}
int a[N] , 叶子节点的值

void pushup(int u){        // 往上推
     tr[u].val = tr[u << 1].val + tr[u << 1].val; // 用区间和做示范，具体依据情况而定
     // tr[u].val = max(tr[u << 1].val , tr[u << 1].val);   求区间最大值
}
void build(int u,int l,int r){
    if(l == r)tr[u] = {l ,r ，a[i]};
    else
    {
        int mid = l + r >> 1;
        tr[u] = {l , r, 0};
        build(u << 1 , l ,mid),build(u << 1 , mid + 1 , r);   // 递归建立左右子树，从下而上建树
        pushup(u);
    }
}

//单点修改
void change(int u,int x,int d){
    if(tr[u ].l == x && tr[u].r == x)
    {
        tr[u].val += d;
        return ;
    }
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if(l <= mid)change(u << 1 , x , d);
        else change(u << 1 | 1 , x, d);         // 往有交集的区间递归修改
        pushup(u);               // 修改完成后还要往上推
    }
}

//区间查询
int query(int u,int l,int r){
     if(tr[u].l >= l && tr[u].r <= r)return tr[u].val;   // 只有在查询区间的的节点才会返回
     else
     {
         int val = 0;
         int mid = tr[u].l + tr[u].r >> 1;
         if(l <= mid)val = max(val, query(u << 1 , l ,r))   // or val + query(u << 1 , l ,r)
         if(r > mid )val = max(val, query(u << 1 | 1 , l ,r))   // or val + query(u << 1 | 1, l ,r)
         return sum;
     }

}
```

- AcWing 1275 最大数:修改点，查询空间，求某个空间的某种属性。pushUP由于子节点信息计算父节点信息，单点修改不需要lazy,时间logn。
- AcWing 245 你能回答这些问题吗：单点修改 + 查询区间内的最大子段和；横跨两个区间的最大子段和  < 左子区间最大后缀 + 右子区间的最大前缀；query需要pushUP。
- AcWing 246 区间最大公约数：差分b, gcd符合结合律，交换律,辗转相除法。
- AcWing 243 一个简单的整数问题2：pushdown懒标记传递
- AcWing 247 亚特兰蒂斯: 扫描线法，线段树节点，永远只往下看，不往上看。 1 永远只考虑根节点信息，查询的时候不需要pushdown。 2 所有操作成对出现，先加后减，modify不需要pushdown. 
离散化 + 线段树 + 线性扫描
- AcWing 1277 维护序列：先乘再加上。

#### 欧几里得算法

```C++
int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}
```
#### 卡特兰数

给定n个0和n个1，它们按照某种顺序排成长度为2n的序列，满足任意前缀中0的个数都不少于1的个数的序列的数量为： Cat(n) = C(2n, n) / (n + 1)

#### 可持久化

前提：本身拓扑结构在操作过程中不变，可以存下来数据结构的所有历史版本。     
核心：只记录每个版本与前一个版本不一样的地方。       
平衡树不支持可持久化。   

**trie的可持久化**

有变化的点分裂开成一个新的路径。建边指向原来的点。

- Acwing256 最大异或和：trie可持久化。query更好选择tr[p](v异或1)

**线段树的可持久化**

可持久化的线段树：主席树，m次修改，会有m个版本。一个节点当前次修改发生变化，则创建一个新的节点。    
可持久化的线段树难以进行区间修改操作，懒标记难以处理，可以使用永久化代替标记的下传。

-AcWing255 第k小的树:划分树，树套树，可持久化线段树，nlogn。1 离散化 2 在数值上建立线段树，维护每个数值区间中一共有多少个数。           
insert/query:线段树可以在树上做二分。 cnt > k 递归左边，否则递归右边。         

```C++
sort(nums.begin(), nums.end());
nums.erase(unique(nums.begin(), nums.end()), nums.end());

//找离散化的映射值
int find(int x)
{
    return lower_bound(nums.begin(), nums.end(), x) - nums.begin();
}

int build(int l, int r)
{
    int p = ++ idx;
    if (l == r) return p;
    int mid = l + r >> 1;
    tr[p].l = build(l, mid), tr[p].r = build(mid + 1, r);
    return p;
}
int query(int q, int p, int l, int r, int k)
{
    if (l == r) return r;
    int cnt = tr[tr[q].l].cnt - tr[tr[p].l].cnt;
    int mid = l + r >> 1;
    if (k <= cnt) return query(tr[q].l, tr[p].l, l, mid, k);
    else return query(tr[q].r, tr[p].r, mid + 1, r, k - cnt);
}
```
#### 平衡树

平衡树treap：tree + heap(大根堆)   

红黑树、splay(用的多)、sbt、avl

**bst**
二叉搜索树

当前节点的左子树任何节点的值都 < 当前节点的值，右子树任何节点的值都 > 当前节点的值。可以动态维护一个有序序列。

set(红黑树实现)支持的操作：insert、erase、get_prev(前驱，左子树最右节点)、get_next(后继，右子树最左节点)、getMin、getMax。     
set不支持的操作：get_rank_by_value、getKthNum、求比某个数小的最大值、比某个数大的最小值。     

bst会退化成一条链

**treap**

核心：让bst尽量随机。在插入每个新节点时，给节点一个随机生成的额外的权值，然后就像二叉树的插入过程一样。自底向上依次检查，当某个节点不满足大根堆的性质时，将该点与父节点交换。    
treap通过适当的单旋转，在维持节点关键码满足bst性质的同时，还使每个节点上随机生成的额外权值满足大根堆的性质，各种操作时间复杂度都是O(logn)。        

加两个哨兵，-inf,inf。       
作用对象：旋转前处于父节点位置的节点作为左右旋的对象。        
左旋zag，右旋zig不影响中序的遍历顺序。    
左儿子大则右旋，右儿子大则左旋，把大的儿子旋转上去。    

基本函数：get_node、build、zig、zag、insert、get_prev、get_next

AcWing253 普通平衡树：get_prev、get_next、get_rank_by_val、get_val_by_rank,size记录以该节点为根的子树的副本数之和。     
AcWing265 营业额统计：a[1]~a[i-1]中找到与a[i]相差最小的数a[j]。1 插入一个数；2 找到>=a[i]的最小的，get_next，也可以set的low_bound; 3 <=a[i]的最大的，get_prev，也可以用set是upper_bound。类似136临值查找。

```C++
set<int> s; 
s.insert(1); 
s.insert(4);

auto it = s.lower_bound(2); 
auto it = s.upper_bound(2); 
```
 


