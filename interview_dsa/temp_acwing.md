
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
- AcWing 243 一个简单的整数问题2：
- AcWing 247 亚特兰蒂斯
- AcWing 1277 维护序列

#### 欧几里得算法

```C++
int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}
```
#### 卡特兰数

给定n个0和n个1，它们按照某种顺序排成长度为2n的序列，满足任意前缀中0的个数都不少于1的个数的序列的数量为： Cat(n) = C(2n, n) / (n + 1)


