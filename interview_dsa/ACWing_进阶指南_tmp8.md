##### AcWing203   同余方程
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

LL exgcd(LL a, LL b, LL &x, LL &y)
{
    if (!b)
    {
        x = 1; y = 0;
        return a;
    }
    LL d = exgcd(b, a % b, y, x);
    y -= (a/b) * x;
    return d;
}

int main()
{
    LL a, b, x, y;
    cin >> a >> b;
    exgcd(a, b, x, y);

    cout << (x % b + b) % b << endl;

    return 0;
}
```
 
##### AcWing204   表达整数的奇怪方式
```cpp


```
  

## 0x34 矩阵乘法

     
  
##### AcWing205   斐波那契  
##### AcWing206   石头游戏       

## 0x35 高斯消元与线性空间

     
  
##### AcWing207   球形空间产生器  
##### AcWing208   开关问题  
##### AcWing209   装备购买  
##### AcWing210   异或运算    
     
## 0x36 组合计数

     
  
##### AcWing211   计算系数  
##### AcWing212   计数交换  
##### AcWing213   古代猪文  

## 0x37 容斥原理与Mobius函数

     
  
##### AcWing214   Devu和鲜花  
##### AcWing215   破译密码  
       
## 0x38 概率与数学期望

     
  
##### AcWing216   Rainbow的信号  
##### AcWing217   绿豆蛙的归宿  
##### AcWing218   扑克牌    
     
## 0x39 0/1分数规划

## 0x3A 博弈论之SG函数

     
  
##### AcWing219   剪纸游戏  
        
## 0x3B 总结与练习

     
##### AcWing220   最大公约数  
##### AcWing221   龙哥的问题  
##### AcWing222   青蛙的约会  
##### AcWing223   阿九大战朱最学  
##### AcWing224   计算器  
##### AcWing225   矩阵幂求和  
##### AcWing226   233矩阵  
##### AcWing227   小部件厂  
##### AcWing228   异或  
##### AcWing229   新NIM游戏
```cpp

```
    
##### AcWing230   排列计数
```cpp

```
    
##### AcWing231   天码
```cpp
#include <iostream>
#include <cstring>

using namespace std;

typedef long long LL;
const int N = 10010;

int n, numbers[N];

LL C(int n, int m)
{
    if (n < m) return 0;

    LL res = 1;
    for (int i = n, j = 0; j < m; j ++, i -- ) res *= i;
    for (int i = 1; i <= m; i ++ ) res /= i;
    return res;
}

int main()
{
    while(cin >> n)
    {
        memset(numbers, 0, sizeof numbers);

        for (int i = 0, t; i < n; i ++ )
        {
            cin >> t;
            numbers[t] ++ ;
        }

        LL res = C(n, 4);

        for (int i = 2; i < N; i ++ )
        {
            int ps = 0, x = i;
            for (int j = 2; j * j <= x; j ++ )
                if (x % j == 0)
                {
                    int s = 0;
                    while (x % j == 0) s ++, x /= j;
                    if (s > 1)
                    {
                        ps = -10;
                        break;
                    }
                    ps ++ ;
                }

            if (x > 1) ps ++ ;

            if (ps > 0)
            {
                int s = 0;
                for (int j = i; j < N; j += i) s += numbers[j];
                if (ps & 1) res -= C(s, 4);
                else res += C(s, 4);
            }
        }

        cout << res << endl;
    }
    return 0;
}
```
  
##### AcWing232   守卫者的挑战
```cpp

```
    
##### AcWing233   换教室  
##### AcWing234   放弃测试  
##### AcWing235   魔法珠  
##### AcWing236   格鲁吉亚和鲍勃      

# 0x40 数据结构进阶(34)

包括并查集、树状数组、线段树、分块、点分治、二叉查找树与平衡树初步、离线分治算法、可持久化数据结构等内容。

## 0x41 并查集

##### AcWing237   程序自动分析
```cpp
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <vector>

using namespace std;

typedef pair<int, int> PII;

const int N = 1000010;

int n, cnt;
vector<PII> eqs, uneqs;
unordered_map<int, int> H;
int p[N * 2];

int mapping(int x)
{
    if (H.count(x)) return H[x];
    return H[x] = cnt ++ ;
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        cin >> n;
        cnt = 0;
        H.clear();
        eqs.clear(), uneqs.clear();
        for (int i = 0; i < n; i ++ )
        {
            int x, y, e;
            scanf("%d%d%d", &x, &y, &e);
            x = mapping(x), y = mapping(y);
            if (e) eqs.push_back({x, y});
            else uneqs.push_back({x, y});
        }

        for (int i = 0; i < cnt; i ++ ) p[i] = i;
        for (auto item : eqs) p[find(item.first)] = find(item.second);

        bool flag = true;
        for (auto item : uneqs)
            if (find(item.first) == find(item.second))
            {
                flag = false;
                break;
            }
        printf("%s\n", flag ? "YES" : "NO");
    }

    return 0;
}
```
##### AcWing238   银河英雄传说
```cpp
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <vector>

using namespace std;

typedef pair<int, int> PII;

const int N = 30010;

int n, m;
int p[N], d[N], size[N];

int find(int x)
{
    if (p[x] != x)
    {
        auto root = find(p[x]);
        d[x] += d[p[x]];
        p[x] = root;
    }
    return p[x];
}

int main()
{
    n = 30000;
    cin >> m;
    for (int i = 1; i <= n; i ++ ) p[i] = i, size[i] = 1;

    while (m -- )
    {
        char s[2];
        int x, y;
        scanf("%s%d%d", s, &x, &y);
        if (*s == 'M')
        {
            x = find(x), y = find(y);
            d[x] = size[y];
            size[y] += size[x];
            p[x] = y;
        }
        else
        {
            auto fax = find(x), fay = find(y);
            if (fax != fay) puts("-1");
            else printf("%d\n", max(0, abs(d[x] - d[y]) - 1));
        }
    }

    return 0;
}
```
##### AcWing239   奇偶游戏  
##### AcWing240   食物链  
        
## 0x42 树状数组

##### AcWing241   楼兰图腾
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 200010;

int n;
int w[N];
int tr[N];
int Greater[N], lower[N];

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

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    for (int i = 1; i <= n; i ++ )
    {
        int y = w[i];
        Greater[i] = sum(n) - sum(y);
        lower[i] = sum(y - 1);
        add(y, 1);
    }

    memset(tr, 0, sizeof tr);

    LL res1 = 0, res2 = 0;
    for (int i = n; i; i -- )
    {
        int y = w[i];
        res1 += (LL)(sum(n) - sum(y)) * Greater[i];
        res2 += (LL)sum(y - 1) * lower[i];
        add(y, 1);
    }

    printf("%lld %lld\n", res1, res2);

    return 0;
}
```
 
##### AcWing242   一个简单的整数问题
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010;

int n, m;
int a[N];
LL tr[N];

int lowbit(int x)
{
    return x & -x;
}

void add(int x, int c)
{
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
}

LL sum(int x)
{
    LL res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

    for (int i = 1; i <= n; i ++ ) add(i, a[i] - a[i - 1]);

    while (m -- )
    {
        char op[2];
        scanf("%s", op);
        if (*op == 'Q')
        {
            int x;
            scanf("%d", &x);
            printf("%lld\n", sum(x));
        }
        else
        {
            int l, r, c;
            scanf("%d%d%d", &l, &r, &c);
            add(l, c), add(r + 1, -c);
        }
    }

    return 0;
}
```
##### AcWing243   一个简单的整数问题
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010;

int n, m;
LL a[N];
LL tr1[N], tr2[N];

int lowbit(int x)
{
    return x & -x;
}

void add(LL tr[], int x, LL c)
{
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
}

LL sum(LL tr[], int x)
{
    LL res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}

LL prefix_sum(int x)
{
    return sum(tr1, x) * (x + 1) - sum(tr2, x);
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%lld", &a[i]);
    for (int i = 1; i <= n; i ++ )
    {
        add(tr1, i, a[i] - a[i - 1]);
        add(tr2, i, i * (a[i] - a[i - 1]));
    }

    while (m -- )
    {
        char op[2];
        int l, r, d;
        scanf("%s%d%d", op, &l, &r);
        if (op[0] == 'Q')
        {
            printf("%lld\n", prefix_sum(r) - prefix_sum(l - 1));
        }
        else
        {
            scanf("%d", &d);
            add(tr1, l, d), add(tr2, l, (LL)l * d);
            add(tr1, r + 1, -d), add(tr2, r + 1, (LL)(r + 1) * -d);
        }
    }

    return 0;
}
```
##### AcWing244   谜一样的牛  
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n;
int h[N], ans[N];
int tr[N];

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

int main()
{
    scanf("%d", &n);
    for (int i = 2; i <= n; i ++ ) scanf("%d", &h[i]);
    for (int i = 1; i <= n; i ++ ) tr[i] = lowbit(i);

    for (int i = n; i; i -- )
    {
        int l = 1, r = n;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (sum(mid) >= h[i] + 1) r = mid;
            else l = mid + 1;
        }

        add(r, -1);
        ans[i] = r;
    }

    for (int i = 1; i <= n; i ++ ) printf("%d\n", ans[i]);

    return 0;
}
``` 

## 0x43 线段树

     
  
##### AcWing245   你能回答这些问题吗
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 500010;

int n, m;
int w[N];
struct Tree
{
    int l, r;
    int sum, lmax, rmax, tmax;
}tr[N * 4];

void pushup(Tree &c, Tree &a, Tree &b)
{
    c.sum = a.sum + b.sum;
    c.lmax = max(a.lmax, a.sum + b.lmax);
    c.rmax = max(b.rmax, b.sum + a.rmax);
    c.tmax = max(max(a.tmax, b.tmax), a.rmax + b.lmax);
}

void pushup(int u)
{
    pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
}

void build(int u, int l, int r)
{
    if (l == r)
    {
        tr[u] = {l, r, w[l], w[l], w[l], w[l]};
        return;
    }
    tr[u].l = l, tr[u].r = r;

    int mid = l + r >> 1;
    build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
    pushup(u);
}

Tree query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u];
    int mid = tr[u].l + tr[u].r >> 1;
    if (r <= mid) return query(u << 1, l, r);
    else if (l > mid) return query(u << 1 | 1, l, r);
    else
    {
        auto a = query(u << 1, l, r);
        auto b = query(u << 1 | 1, l, r);
        Tree c;
        pushup(c, a, b);
        return c;
    }
}

void modify(int u, int x, int y)
{
    if (tr[u].l == x && tr[u].r == x)
    {
        tr[u] = {x, x, y, y, y, y};
        return;
    }
    int mid = tr[u].l + tr[u].r >> 1;
    if (x <= mid) modify(u << 1, x, y);
    else modify(u << 1 | 1, x, y);
    pushup(u);
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    build(1, 1, n);

    while (m -- )
    {
        int k, x, y;
        scanf("%d%d%d", &k, &x, &y);
        if (k == 1)
        {
            if (x > y) swap(x, y);
            printf("%d\n", query(1, x, y).tmax);
        }
        else modify(1, x, y);
    }

    return 0;
}
```

##### AcWing246   区间最大公约数
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 500010;

int n, m;
LL w[N];
LL tr1[N];
struct Tree
{
    int l, r;
    LL v;
}tr[N * 4];

int lowbit(int x)
{
    return x & -x;
}

void add(int x, LL v)
{
    for (int i = x; i <= n; i += lowbit(i)) tr1[i] += v;
}

LL sum(int x)
{
    LL res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr1[i];
    return res;
}

LL gcd(LL a, LL b)
{
    return b ? gcd(b, a % b) : a;
}

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, w[r] - w[r - 1]};
    else
    {
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        tr[u] = {l, r, gcd(tr[u << 1].v, tr[u << 1 | 1].v)};
    }
}

LL query(int u, int l, int r)
{
    if (l > r) return 0;
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].v;
    int mid = tr[u].l + tr[u].r >> 1;
    LL v = 0;
    if (l <= mid) v = query(u << 1, l, r);
    if (r > mid) v = gcd(v, query(u << 1 | 1, l, r));
    return v;
}

void modify(int u, int x, LL v)
{
    if (tr[u].l == x && tr[u].r == x) tr[u].v += v;
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (x <= mid) modify(u << 1, x, v);
        else modify(u << 1 | 1, x, v);
        tr[u].v = gcd(tr[u << 1].v, tr[u << 1 | 1].v);
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%lld", &w[i]);

    for (int i = 1; i <= n; i ++ ) add(i, w[i] - w[i - 1]);
    build(1, 1, n + 1);

    char op[2];
    int l, r;
    LL d;
    while (m -- )
    {
        scanf("%s%d%d", op, &l, &r);
        if (*op == 'Q')
        {
            LL left = sum(l);
            LL right = query(1, l + 1, r);
            printf("%lld\n", abs(gcd(left, right)));
        }
        else
        {
            scanf("%lld", &d);
            add(l, d), add(r + 1, -d);
            modify(1, l, d), modify(1, r + 1, -d);
        }
    }

    return 0;
}
``` 
##### AcWing247   亚特兰蒂斯
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 110;

int n, m;
struct Segment
{
    double x, y1, y2;
    int k;
    bool operator< (const Segment &t)const
    {
        return x < t.x;
    }
}seg[N * 2];
struct Node
{
    int l, r;
    int cnt;
    double len;
}tr[N * 8];

vector<double> ys;

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, 0, 0};
    else
    {
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
    }
}

int find(double y)
{
    return lower_bound(ys.begin(), ys.end(), y) - ys.begin();
}

void pushup(int u)
{
    if (tr[u].cnt) tr[u].len = ys[tr[u].r + 1] - ys[tr[u].l];
    else if (tr[u].l == tr[u].r) tr[u].len = 0;
    else tr[u].len = tr[u << 1].len + tr[u << 1 | 1].len;
}

void modify(int u, int l, int r, int k)
{
    if (tr[u].l >= l && tr[u].r <= r)
    {
        tr[u].cnt += k;
        pushup(u);
    }
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) modify(u << 1, l, r, k);
        if (r > mid) modify(u << 1 | 1, l, r, k);
        pushup(u);
    }
}

int main()
{
    int T = 1;
    while (scanf("%d", &n), n)
    {
        ys.clear();
        for (int i = 0; i < n; i ++ )
        {
            double x1, y1, x2, y2;
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            ys.push_back(y1), ys.push_back(y2);
            seg[i * 2] = {x1, y1, y2, 1};
            seg[i * 2 + 1] = {x2, y1, y2, -1};
        }

        sort(ys.begin(), ys.end());
        ys.erase(unique(ys.begin(), ys.end()), ys.end());

        m = ys.size();
        build(1, 0, m - 2);

        sort(seg, seg + n * 2);

        double res = 0;
        for (int i = 0; i < n * 2;)
        {
            int j = i;
            while (j < n * 2 && seg[j].x == seg[i].x) j ++ ;
            if (i) res += tr[1].len * (seg[i].x - seg[i - 1].x);
            while (i < j)
            {
                modify(1, find(seg[i].y1), find(seg[i].y2) - 1, seg[i].k);
                i ++ ;
            }
        }

        printf("Test case #%d\n", T ++ );
        printf("Total explored area: %.2lf\n\n", res);
    }

    return 0;
}
```  
##### AcWing248   窗内的星星
```cpp

```  
       
## 0x44 分块

     
  
##### AcWing249   蒲公英  
##### AcWing250   磁力块  
##### AcWing251   小Z的袜子  
        
## 0x45 点分治

     
  
##### AcWing252   树  
           
## 0x46 二叉查找树与平衡树初步

     
  
##### AcWing253   普通平衡树  
      
## 0x47 离线分治算法

     
  
##### AcWing254   天使玩偶  
##### AcWing255   第K个vb数  

## 0x48 可持久化数据结构

     
  
##### AcWing256   最大异或和

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 600010, M = N * 24;

int n, m;
int tr[M][2], latest[M], idx;
int root[N];
int s[N];

void insert(int i, int k, int p, int q)
{
    if (k < 0)
    {
        latest[q] = i;
        return;
    }
    int v = s[i] >> k & 1;
    if (p) tr[q][v ^ 1] = tr[p][v ^ 1];
    tr[q][v] = ++ idx;
    insert(i, k - 1, tr[p][v], tr[q][v]);
    latest[q] = max(latest[tr[q][0]], latest[tr[q][1]]);
}

int query(int root, int val, int limit)
{
    int p = root;
    for (int i = 23; i >= 0; i -- )
    {
        int v = val >> i & 1;
        if (latest[tr[p][v ^ 1]] >= limit) p = tr[p][v ^ 1];
        else p = tr[p][v];
    }

    return val ^ s[latest[p]];
}

int main()
{
    scanf("%d%d", &n, &m);

    latest[0] = -1;
    root[0] = ++ idx;
    insert(0, 23, 0, root[0]);

    for (int i = 1; i <= n; i ++ )
    {
        int x;
        scanf("%d", &x);
        s[i] = s[i - 1] ^ x;
        root[i] = ++ idx;
        insert(i, 23, root[i - 1], root[i]);
    }

    char op[2];
    int l, r, x;
    while (m -- )
    {
        scanf("%s", op);
        if (*op == 'A')
        {
            scanf("%d", &x);
            n ++ ;
            s[n] = s[n - 1] ^ x;
            root[n] = ++ idx;
            insert(n, 23, root[n - 1], root[n]);
        }
        else
        {
            scanf("%d%d%d", &l, &r, &x);
            printf("%d\n", query(root[r - 1], x ^ s[n], l - 1));
        }
    }

    return 0;
}
```  
       
## 0x49 总结与练习
