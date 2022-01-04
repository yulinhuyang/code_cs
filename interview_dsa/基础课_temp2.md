


## 常用代码模板4——数学知识

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

如果 N = p1^c1 * p2^c2 * ... *pk^ck
约数个数： (c1 + 1) * (c2 + 1) * ... * (ck + 1)
约数之和： (p1^0 + p1^1 + ... + p1^c1) * ... * (pk^0 + pk^1 + ... + pk^ck)

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
    2. 通过 C(a, b) = a! / b! / (a - b)! 这个公式求出每个质因子的次数。 n! 中p的次数是 n / p + n / p^2 + n / p^3 + ...
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



# 常用代码模板5——DFS、动态规划

## 5.1 递归回溯

教程:  [[递归与回溯的理解]](https://cloud.tencent.com/developer/article/1434886)  [[手把手教怎么写递归和回溯]](https://leetcode-cn.com/circle/article/GV6eQ2/)   

递归回溯一般模板：
```cpp
'''
backtracking使用dfs的模板，基本跟dfs的模板一模一样
'''
class Backtracking(object):

    def backtracking(self, input):

        self.res = []

        def dfs(input, temp, [index]):
            # 边界
            if 非法数据：
                return

            # 终止条件
            if len(input) == len(temp)：
                self.res.append(temp[:])
                return

            # for循环
            for i range(len(input)):
                ##1. 修改path
                temp.append(input[i])
                ##2. backtracking
                dfs(input, temp, [index])
                ##3. 退回原来状态，恢复path
                temp.pop()
        # 执行
        dfs(input, [], 0)
        return self.res
```

- [x] (中等)Leetcode 17 电话号码的字母组合[[problem]](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
- [x] (中等)Leetcode 22 括号生成[[problem]](https://leetcode-cn.com/problems/generate-parentheses/)
- [x] (中等)Leetcode 46 全排列[[problem]](https://leetcode-cn.com/problems/permutations/)
- [x] (中等)Leetcode 47 全排列II[[problem]](https://leetcode-cn.com/problems/permutations-ii/)  

## 5.2 DFS

**算法描述**  
DFS的算法具体描述为选择一个起点v作为当前节点，执行如下操作：  
a. 访问当前节点，并且标记当前节点已被访问过，然后跳转到b；  
b. 如果存在一个和**当前节点**相邻并且尚未被访问的节点u，则将节点u设为当前节点，继续执行a；  
c. 如果不存在这样的u，则进行回溯，回溯的过程就是回退**当前节点**；  
上述**当前节点**需要用一个栈来维护，每次访问到得到节点入栈，回溯的时候出栈（也可以用递归来实现，更佳直观和方便）。    

- DFS实现  
```python
def DFS(v):
    vistited[v]=true
    dosomething()
    for u in adjcent_list[v]:
        if visited[v] is false:
            DFS(u) 
//其中dosomething 表示访问时具体要干的事，根据情况而定，并DFS是可以有返回值的；
```

**基础应用**   
- 求N的阶乘；   
```cpp
//f(n)=n*f(n-1), n>0
```
- 求斐波那契数列的第N项  
```cpp
//f(n)=f(n-1)+f(n-2), n>2,f(0)=f(1)=1  
//记忆化搜索
```
- 求n个数的全排列   
```cpp
//建立全连接图
```

**DFS高级应用**    
- 枚举    
数据较小的排列、组合的穷举；  
- 容斥原理  
利用深搜计算一个公式，本质上还是枚举；  	
- 基于状态压缩的动态规划    
一般解决棋盘摆放问题，k进制表示状态，然后利用深搜进行状态转移；  
- 记忆化搜索  
某个状态被计算过了，就将它cache住，下次要用到的时候就不需要再一次计算；  
- 有向图的强连通分量   
经典的Tarjan算法；  
求解2-sat问题的基础；   
- 无向图割边割点和双连通分量  
经典的Tarjan算法；  
- LCA   
最近公共祖先递归求解；  
- 博弈  
利用深搜计算SG值；   
- 二分图最大匹配  
经典的匈牙利算法；  
最小顶点覆盖，最大独立集，最小值支配集向二分图的转化；  
- 欧拉回路  
经典的圈套圈算法；  
- K短路   
依赖数据，数据不卡的话可以采用二分答案+深搜或者广搜+A*;  
- 线段树   
二分经典思想，配合枚举深搜左右子树；  
- 最大团  
极大完全子图的优化算法；  
- 最大流   
EK算法求任意路径中有涉及；  
- 树形DP   
即树形动态规划；父节点的值由各子节点计算得出；  


### 5.2.1 基于DFS的记忆化搜索  

### 5.2.2 基于DFS的剪枝    
好的剪枝可以大大提升程序的运行效率，那么问题来了，如何进行剪枝？我们先来看剪枝需要满足什么原则：  
a. 正确性  
剪掉的子树中如果存在可行解（或最优解），那么在其它的子树中很可能搜不到解导致搜索失败，所以剪枝的前提必须是要正确；  
b. 准确性  
剪枝要“准”。所谓“准”，就是要在保证在正确的前提下，尽可能多得剪枝。  
c. 高效性  
剪枝一般是通过一个函数来判断当前搜索空间是否是一个合法空间，在每个结点都会调用到这个函数，所以这个函数的效率很重要。    

- 可行性剪枝  
可行性剪枝一般是处理可行解的问题，如一个迷宫，问能否从起点到达目标点之类的。


- 最优化剪枝    
最优性剪枝一般是处理最优解的问题。以求两个状态之间的最小步数为例，搜索最小步数的过程：一般情况下，需要保存一个“当前最小步数”，这个最小步数就是当前解的一个下界d。在遍历到搜索树的叶子结点时，得到了一个新解，与保存的下界作比较，如果新解的步数更小，则令它成为新的下界。搜索结束后，所保存的解就是最小步数。而当我们已经搜索了k歩，如果能够通过某种方式估算出当前状态到目标状态的理论最少步数s时，就可以计算出起点到目标点的理论最小步数，即估价函数h = k + s，那么当前情况下存在最优解的必要条件是h < d，否则就可以剪枝了。最优性剪枝是不断优化解空间的过程。     


### 5.2.3 基于DFS的A*算法 (迭代加深IDA*) 
**1) 算法原理**  
迭代加深分两步走：  
- 枚举深度。  
- 根据限定的深度进行DFS，并且利用估价函数进行剪枝。  

### 5.2.4 DFS题集 

**ACM DFS**:    
- [x] [Red and Black](http://poj.org/problem?id=1979)        ★☆☆☆☆   FloodFill

- [ ] [The Game](http://poj.org/problem?id=1970)          ★☆☆☆☆   FloodFill

- [ ] [Frogger](http://poj.org/problem?id=2253)           ★☆☆☆☆   二分枚举答案 + FloodFill  

- [ ] [Nearest Common Ancestors](http://poj.org/problem?id=1330)  ★☆☆☆☆   最近公共祖先 

- [ ] [Robot Motion](http://poj.org/problem?id=1573)        ★☆☆☆☆   递归模拟

- [ ] [Dessert](http://poj.org/problem?id=1950)           ★☆☆☆☆   枚举

- [ ] [Matrix](http://poj.org/problem?id=2078)           ★☆☆☆☆   枚举

- [ ] [Frame Stacking](http://poj.org/problem?id=1128)       ★☆☆☆☆   枚举

- [ ] [Transportation](http://poj.org/problem?id=1040)       ★☆☆☆☆   枚举

- [ ] [Pairs of Integers](http://poj.org/problem?id=1117)          ★★☆☆☆   枚举


- [ ] [Machine Schedule](http://poj.org/problem?id=1325)           ★★★☆☆  二
**IDA\***(确定是迭代加深后就一个套路，枚举深度，然后 暴力搜索+强剪枝)
- [ ] [Addition Chains](http://poj.org/problem?id=2248)            ★★☆☆☆   

- [ ] [DNA sequence](http://acm.hdu.edu.cn/showproblem.php?pid=1560)        ★★☆☆☆   

- [ ] [Booksort](http://poj.org/problem?id=3460)                   ★★★☆☆  

- [ ] [The Rotation Game](http://acm.hdu.edu.cn/showproblem.php?pid=1667)   

## 5.3. BFS
BFS的具体算法描述为选择一个起始点v放入一个先进先出的队列中，执行如下操作：    
a. 如果队列不为空，弹出一个队列首元素，记为当前结点，执行b；否则算法结束；  
b. 将与 当前结点 相邻并且尚未被访问的结点的信息进行更新，并且全部放入队列中，继续执行a；  
维护广搜的数据结构是队列和HASH，队列就是官方所说的open-close表，HASH主要是用来标记状态的，比如某个状态并不是一个整数，可能是一个字符串，就需要用字符串映射到一个整数，可以自己写个散列HASH表，不建议用STL的map，效率奇低。   


**算法实现**  
广搜一般用队列维护状态，写成伪代码如下：  
```python
def BFS(v):
    resetArray(visited,false)
    visited[v] = true
    queue.push(v)
    while not queue.empty():
        v = queue.getfront_and_pop()
        for u in adjcent_list[v]:
            if visited[u] is false:
                dosomething(u)
                queue.push(u)
```

**基础应用**    
-  最短路：bellman-ford最短路的优化算法SPFA，主体是利用BFS实现的。    
绝大部分四向、八向迷宫的最短路问题。     
- 拓扑排序：  
首先找入度为0的点入队，弹出元素执行“减度”操作，继续将减完度后入度为0的点入队，循环操作，直到队列为空，经典BFS操作；   
- FloodFill：  
经典洪水灌溉算法；  


**高级应用**  
- 差分约束：   
数形结合的经典算法，利用SPFA来求解不等式组。  
- 稳定婚姻：   
二分图的稳定匹配问题，试问没有稳定的婚姻，如何有心思学习算法，所以一定要学好BFS啊；                
- AC自动机：   
字典树 + KMP + BFS，在设定失败指针的时候需要用到BFS。     
详细算法参见：http://www.cppblog.com/menjitianya/archive/2014/07/10/207604.html  
- 矩阵二分：   
矩阵乘法的状态转移图的构建可以采用BFS；   
- 基于k进制的状态压缩搜索：   
这里的k一般为2的幂，状态压缩就是将原本多维的状态压缩到一个k进制的整数中，便于存储在一个一维数组中，往往可以大大地节省空间，又由于k为2的幂，所以状态转移可以采用位运算进行加速，HDU1813和HDU3278以及HDU3900都是很好的例子；   

### 5.3.1 基于BFS的A*算法  
在搜索的时候，结点信息要用堆（优先队列）维护大小，即能更快到达目标的结点优先弹出。



### 5.3.2 BFS题集

- [ ] [Pushing Boxes](http://poj.org/problem?id=1475)        ★☆☆☆☆   经典广搜 - 推箱子

- [ ] [Jugs](http://poj.org/problem?id=1606)            ★☆☆☆☆   经典广搜 - 倒水问题

- [ ] [Space Station Shielding](http://poj.org/problem?id=1096)  ★☆☆☆☆   FloodFill

- [ ] [Knight Moves](http://poj.org/problem?id=1915)        ★☆☆☆☆   棋盘搜索

- [ ] [Knight Moves](http://poj.org/problem?id=2243)        ★☆☆☆☆   棋盘搜索    

- [ ] [Eight](http://poj.org/problem?id=1077)            ★★☆☆☆   经典八数码

- [ ] [Currency Exchange](http://poj.org/problem?id=1860)      ★★☆☆☆   SPFA

- [ ] [The Postal Worker Rings](http://poj.org/problem?id=1237)  ★★☆☆☆   SPFA


### 5.3.3 双向广搜  (适用于起始状态都给定的问题，一般一眼就能看出来，固定套路，很难有好的剪枝)

初始状态 和 目标状态 都知道，求初始状态到目标状态的最短距离;    
利用两个队列，初始化时初始状态在1号队列里，目标状态在2号队列里，并且记录这两个状态的层次都为0，然后分别执行如下操作：   
a.若1号队列已空，则结束搜索，否则从1号队列逐个弹出层次为K(K >= 0)的状态；    
i.  如果该状态在2号队列扩展状态时已经扩展到过，那么最短距离为两个队列扩展状态的层次加和，结束搜索；   
ii. 否则和BFS一样扩展状态，放入1号队列，直到队列首元素的层次为K+1时执行b；   
b.若2号队列已空，则结束搜索，否则从2号队列逐个弹出层次为K(K >= 0)的状态；   
i.  如果该状态在1号队列扩展状态时已经扩展到过，那么最短距离为两个队列扩展状态的层次加和，结束搜索；   
ii. 否则和BFS一样扩展状态，放入2号队列，直到队列首元素的层次为K+1时执行a；    


- [ ] [Solitaire](http://poj.org/problem?id=1198)         ★★★☆☆

- [ ] [A Game on the Chessboard](http://poj.org/problem?id=1735)  ★★★☆☆

- [ ] [魔板](http://acm.hdu.edu.cn/showproblem.php?pid=1430)            ★★★★☆
- [ ] [Tobo or not Tobo](http://acm.hdu.edu.cn/showproblem.php?pid=2918)      ★★★★☆

- [ ] [Eight II](http://acm.hdu.edu.cn/showproblem.php?pid=3567)          ★★★★★


## 5.4 动态规划 DP

[夜深人静写算法——动态规划](http://www.cppblog.com/menjitianya/archive/2015/10/23/212084.html)  

[闫氏dp分析法](https://blog.csdn.net/qq_39782006/article/details/105420831)

[【紫书第九章】动态规划(DP)常见模型汇总](https://blog.csdn.net/qq_49688477/article/details/119569267)

### 5.4.1 动态规划初探

#### 1、递推   

- 5.最长回文子串 [[problem]](https://leetcode-cn.com/problems/longest-palindromic-substring/)  
- 62.不同路径 [[problem]](https://leetcode-cn.com/problems/unique-paths/)  
- 63.不同路径 II [[problem]](https://leetcode-cn.com/problems/unique-paths-ii/)  


#### 2、记忆化搜索   
递推说白了就是在知道前i-1项的值的前提下，计算第i项的值，而记忆化搜索则是另外一种思路。它是直接计算第i项，需要用到第 j 项的值( j < i)时去查表，如果表里已经有第 j 项的话，则直接取出来用，否则递归计算第 j 项，并且在计算完毕后把值记录在表中。记忆化搜索在求解多维的情况下比递推更加方便。  

- [x] 87. 扰乱字符串 [[problem]](https://leetcode-cn.com/problems/scramble-string/)     



- [ ] Function Run Fun  [[proble]](http://poj.org/problem?id=1579)                              ★☆☆☆☆          【例题3】

- [ ] FatMouse and Cheese  [[problem]](http://acm.hdu.edu.cn/showproblem.php?pid=1078)                            ★☆☆☆☆          经典迷宫问题

- [ ] Cheapest Palindrome  [[problem]](http://poj.org/problem?id=3280)                           ★★☆☆☆ 

- [ ] A Mini Locomotive    [[problem]](http://poj.org/problem?id=1976)                           ★★☆☆☆

- [ ] Millenium Leapcow    [[problem]](http://poj.org/problem?id=2111)                           ★★☆☆☆

- [ ] Brackets Sequence   [[problem]](http://poj.org/problem?id=1141)                            ★★★☆☆          经典记忆化

- [ ] Chessboard Cutting  [[problem]](http://poj.org/problem?id=1191)                            ★★★☆☆          《算法艺术和信息学竞赛》例题

- [ ] Number Cutting Game  [[problem]](http://acm.hdu.edu.cn/showproblem.php?pid=2848)                         ★★★☆☆

#### 3、状态和状态转移 
#### 4、最优化原理和最优子结构
#### 5、决策和无后效性

### 5.4.2 动态规划的经典模型

#### 1、线性模型   
- [x] 72. 编辑距离  [[problem]](https://leetcode-cn.com/problems/edit-distance/)     
- [x] 85. 最大矩形 [[problem]](https://leetcode-cn.com/problems/maximal-rectangle/)  [[直方图O(N\*N\*M]]  [[DP O(N\*M)]]  
- [x] 91. 解码方法 [[problem]](https://leetcode-cn.com/problems/decode-ways/)



#### 2、区间模型
#### 3、背包模型
#### 5、树状模型

### 5.4.3 动态规划的常用状态转移方程
#### 1、1D/1D
#### 2、2D/0D
#### 3、2D/1D
#### 4、2D/2D

### 5.4.4 动态规划和数据结构结合的常用优化
#### 1、滚动数组
#### 2、最长单调子序列的二分优化
#### 3、矩阵优化
#### 4、斜率优化
#### 5、树状数组优化
#### 6、线段树优化
#### 7、其他优化
