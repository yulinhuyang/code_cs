
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

https://www.acwing.com/solution/content/29308/  


```cpp
带边权写法
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_map>

using namespace std;

const int N = 20010;

int n, m;
int p[N], d[N];
unordered_map<int, int> S;

int get(int x)
{
    if (S.count(x) == 0) S[x] = ++ n;
    return S[x];
}

int find(int x)
{
    if (p[x] != x)
    {
        int root = find(p[x]);
        d[x] += d[p[x]];
        p[x] = root;
    }
    return p[x];
}

int main()
{
    cin >> n >> m;
    n = 0;

    for (int i = 0; i < N; i ++ ) p[i] = i;

    int res = m;
    for (int i = 1; i <= m; i ++ )
    {
        int a, b;
        string type;
        cin >> a >> b >> type;
        a = get(a - 1), b = get(b);

        int t = 0;
        if (type == "odd") t = 1;

        int pa = find(a), pb = find(b);
        if (pa == pb)
        {
            if (((d[a] + d[b]) % 2 + 2) % 2 != t)
            {
                res = i - 1;
                break;
            }
        }
        else
        {
            p[pa] = pb;
            d[pa] = d[a] ^ d[b] ^ t;
        }
    }

    cout << res << endl;

    return 0;
}
扩展域写法
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_map>

using namespace std;

const int N = 40010, Base = N / 2;

int n, m;
int p[N];
unordered_map<int, int> S;

int get(int x)
{
    if (S.count(x) == 0) S[x] = ++ n;
    return S[x];
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;
    n = 0;

    for (int i = 0; i < N; i ++ ) p[i] = i;

    int res = m;
    for (int i = 1; i <= m; i ++ )
    {
        int a, b;
        string type;
        cin >> a >> b >> type;
        a = get(a - 1), b = get(b);

        if (type == "even")
        {
            if (find(a + Base) == find(b))
            {
                res = i - 1;
                break;
            }
            p[find(a)] = find(b);
            p[find(a + Base)] = find(b + Base);
        }
        else
        {
            if (find(a) == find(b))
            {
                res = i - 1;
                break;
            }

            p[find(a + Base)] = find(b);
            p[find(a)] = find(b + Base);
        }
    }

    cout << res << endl;

    return 0;
}

```

##### AcWing240   食物链  

数组d的真正含义以及find()函数调用过程：https://www.acwing.com/solution/content/15938/

https://www.acwing.com/solution/content/1357/

```cpp

#include <iostream>

using namespace std;

const int N = 50010;

int n, m;
int p[N], d[N];

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

int main()
{
    scanf("%d%d", &n, &m);

    for (int i = 1; i <= n; i ++ ) p[i] = i;

    int res = 0;
    while (m -- )
    {
        int t, x, y;
        scanf("%d%d%d", &t, &x, &y);

        if (x > n || y > n) res ++ ;
        else
        {
            int px = find(x), py = find(y);
            if (t == 1)
            {
                if (px == py && (d[x] - d[y]) % 3) res ++ ;
                else if (px != py)
                {
                    p[px] = py;
                    d[px] = d[y] - d[x];
                }
            }
            else
            {
                if (px == py && (d[x] - d[y] - 1) % 3) res ++ ;
                else if (px != py)
                {
                    p[px] = py;
                    d[px] = d[y] + 1 - d[x];
                }
            }
        }
    }

    printf("%d\n", res);

    return 0;
}
```   
   
   
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

##### AcWing257   关押罪犯
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 20010, M = 200010;

int n, m;
int h[N], e[M], ne[M], w[M], idx;
int color[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool dfs(int u, int c, int mid)
{
    color[u] = c;

    for (int i = h[u]; ~i; i = ne[i])
        if (w[i] > mid)
        {
            int j = e[i];
            if (color[j] == -1)
            {
                if (!dfs(j, !c, mid)) return false;
            }
            else if (c == color[j]) return false;
        }

    return true;
}

bool check(int mid)
{
    memset(color, -1, sizeof color);

    for (int i = 1; i <= n; i ++ )
        if (color[i] == -1)
            if (!dfs(i, 0, mid))
                return false;

    return true;
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);

    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c), add(b, a, c);
    }

    int l = 0, r = 1e9;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }

    printf("%d\n", r);

    return 0;
}
```
##### AcWing258   石头剪子布
  
##### AcWing259   真正的骗子 
 
##### AcWing260   买票 
 
##### AcWing261   旅馆 
 
##### AcWing262   海报
  
##### AcWing263   作诗
  
##### AcWing264   权值 
 
##### AcWing265   营业额统计
  
##### AcWing266   超级备忘录
  
##### AcWing267   莫基亚
  
##### AcWing268   流星 
 
##### AcWing269   Fotile模拟赛L 
 
##### AcWing270   可持久化并查集加强版  

# 0x50 动态规划(72)

包括线性DP、背包、区间DP、树形DP、环形与后效性处理、状态压缩DP、倍增优化DP、数据结构优化DP、单调队列优化DP、斜率优化、四边形不等式、计数类 DP、数位统计DP等内容。

[夜深人静写算法——动态规划](http://www.cppblog.com/menjitianya/archive/2015/10/23/212084.html)  

[闫氏dp分析法](https://blog.csdn.net/qq_39782006/article/details/105420831)

[【紫书第九章】动态规划(DP)常见模型汇总](https://blog.csdn.net/qq_49688477/article/details/119569267)

递推、记忆化搜索、状态和状态转移、最优化原理和最优子结构、决策和无后效性

常用状态转移方程：1D/1D、2D/0D、2D/1D、2D/2D


## 0x51 线性DP

[72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)     
[85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)   
[91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

##### AcWing271   杨老师的照相排列
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 31;

int n;
LL f[N][N][N][N][N];

int main()
{
    while (cin >> n, n)
    {
        int s[5] = {0};
        for (int i = 0; i < n; i ++ ) cin >> s[i];
        memset(f, 0, sizeof f);
        f[0][0][0][0][0] = 1;

        for (int a = 0; a <= s[0]; a ++ )
            for (int b = 0; b <= min(a, s[1]); b ++ )
                for (int c = 0; c <= min(b, s[2]); c ++ )
                    for (int d = 0; d <= min(c, s[3]); d ++ )
                        for (int e = 0; e <= min(d, s[4]); e ++ )
                        {
                            LL &x = f[a][b][c][d][e];
                            if (a && a - 1 >= b) x += f[a - 1][b][c][d][e];
                            if (b && b - 1 >= c) x += f[a][b - 1][c][d][e];
                            if (c && c - 1 >= d) x += f[a][b][c - 1][d][e];
                            if (d && d - 1 >= e) x += f[a][b][c][d - 1][e];
                            if (e) x += f[a][b][c][d][e - 1];
                        }
        cout << f[s[0]][s[1]][s[2]][s[3]][s[4]] << endl;
    }

    return 0;
}

```
##### AcWing272   最长公共上升子序列
```cpp
#include<iostream>
#include<cstring>
using namespace std;

const int N = 3010;
int a[N],b[N];
int f[N][N]; 
int n;

int main()
{
    cin >> n;
    for(int i = 1;i <= n;i ++) cin >> a[i];
    for(int i = 1;i <= n;i ++) cin >> b[i];
    b[0] = -(1 << 30);

    for(int i = 1;i <= n;i ++)
    {
        int maxs = 0; 
        for(int j = 1;j <= n;j ++)
        {
            f[i][j] = f[i - 1][j];
            if(b[j - 1] < a[i])
                    maxs = max(maxs,f[i - 1][j - 1] + 1);
            if(a[i] == b[j])
            {

                f[i][j] = max(f[i][j] ,maxs);
            }
        }
    }

    int res = 0;
    for(int i = 1;i <= n;i ++)
        res = max(res,f[n][i]);
    cout << res << endl;
    return 0;

}
```
##### AcWing273   分级
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 2010, INF = 0x3f3f3f3f;

int n;
int a[N], b[N];
int f[N][N];

int work()
{
    for (int i = 1; i <= n; i ++ ) b[i] = a[i];
    sort(b + 1, b + n + 1);

    for (int i = 1; i <= n; i ++ )
    {
        int minv = INF;
        for (int j = 1; j <= n; j ++ )
        {
            minv = min(minv, f[i - 1][j]);
            f[i][j] = minv + abs(a[i] - b[j]);
        }
    }

    int res = INF;
    for (int i = 1; i <= n; i ++ ) res = min(res, f[n][i]);

    return res;
}

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

    int res = work();
    reverse(a + 1, a + n + 1);
    res = min(res, work());

    printf("%d\n", res);

    return 0;
}
```
##### AcWing274   移动服务
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 210, M = 1010, INF = 0x3f3f3f3f;

int n, m;
int w[N][N];
int f[M][N][N];
int p[M];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            scanf("%d", &w[i][j]);
    for (int i = 1; i <= m; i ++ ) scanf("%d", &p[i]);

    p[0] = 3;
    memset(f, 0x3f, sizeof f);
    f[0][1][2] = 0;
    for (int i = 0; i < m; i ++ )
        for (int x = 1; x <= n; x ++ )
            for (int y = 1; y <= n; y ++ )
            {
                int z = p[i], v = f[i][x][y];
                if (x == y || x == z || y == z) continue;
                int u = p[i + 1];
                f[i + 1][x][y] = min(f[i + 1][x][y], v + w[z][u]);
                f[i + 1][z][y] = min(f[i + 1][z][y], v + w[x][u]);
                f[i + 1][x][z] = min(f[i + 1][x][z], v + w[y][u]);
            }

    int res = INF;
    for (int x = 1; x <= n; x ++ )
        for (int y = 1; y <= n; y ++ )
        {
            int z = p[m];
            if (x == y || x == z || y == z) continue;
            res = min(res, f[m][x][y]);
        }

    printf("%d\n", res);

    return 0;
}
```  
##### AcWing275   传纸条
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 55;

int n, m;
int w[N][N];
int f[N * 2][N][N];

int main()
{
    scanf("%d%d", &n, &m);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &w[i][j]);

    for (int k = 2; k <= n + m; k ++ )
        for (int i1 = max(1, k - m); i1 <= min(n, k - 1); i1 ++ )
            for (int i2 = max(1, k - m); i2 <= min(n, k - 1); i2 ++ )
            {
                int j1 = k - i1, j2 = k - i2;
                int t = w[i1][j1];
                if (i1 != i2) t += w[i2][j2];

                int &v = f[k][i1][i2];
                v = max(v, f[k - 1][i1][i2] + t);
                v = max(v, f[k - 1][i1][i2 - 1] + t);
                v = max(v, f[k - 1][i1 - 1][i2] + t);
                v = max(v, f[k - 1][i1 - 1][i2 - 1] + t);
            }

    printf("%d\n", f[n + m][n][n]);

    return 0;
}
```  
##### AcWing276   I-区域
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 16;

int n, m, k;
int w[N][N];
int f[N][N * N][N][N][2][2];

struct State
{
    int i, j, l, r, x, y;
}g[N][N * N][N][N][2][2];

int main()
{
    cin >> n >> m >> k;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            cin >> w[i][j];

    memset(f, -0x3f, sizeof f);

    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j <= k; j ++ )
            for (int l = 1; l <= m; l ++ )
                for (int r = l; r <= m; r ++ )
                {
                    if (j < r - l + 1) continue;

                    // 左扩张，右扩张
                    {
                        auto &vf = f[i][j][l][r][1][0];
                        auto &vg = g[i][j][l][r][1][0];
                        if (j == r - l + 1) vf = 0;
                        for (int p = l; p <= r; p ++ )
                            for (int q = p; q <= r; q ++ )
                            {
                                int val = f[i - 1][j - (r - l + 1)][p][q][1][0];
                                if (vf < val)
                                {
                                    vf = val;
                                    vg = {i - 1, j - (r - l + 1), p, q, 1, 0};
                                }
                            }
                        for (int u = l; u <= r; u ++ ) vf += w[i][u];
                    }

                    // 左扩张，右收缩
                    {
                        auto &vf = f[i][j][l][r][1][1];
                        auto &vg = g[i][j][l][r][1][1];
                        for (int p = l; p <= r; p ++ )
                            for (int q = r; q <= m; q ++ )
                                for (int y = 0; y <= 1; y ++ )
                                {
                                    int val = f[i - 1][j - (r - l + 1)][p][q][1][y];
                                    if (vf < val)
                                    {
                                        vf = val;
                                        vg = {i - 1, j - (r - l + 1), p, q, 1, y};
                                    }
                                }
                        for (int u = l; u <= r; u ++ ) vf += w[i][u];
                    }

                    // 左收缩，右扩张
                    {
                        auto &vf = f[i][j][l][r][0][0];
                        auto &vg = g[i][j][l][r][0][0];
                        for (int p = 1; p <= l; p ++ )
                            for (int q = l; q <= r; q ++ )
                                for (int x = 0; x <= 1; x ++ )
                                {
                                    int val = f[i - 1][j - (r - l + 1)][p][q][x][0];
                                    if (vf < val)
                                    {
                                        vf = val;
                                        vg = {i - 1, j - (r - l + 1), p, q, x, 0};
                                    }
                                }
                        for (int u = l; u <= r; u ++ ) vf += w[i][u];
                    }

                    // 左收缩，右收缩
                    {
                        auto &vf = f[i][j][l][r][0][1];
                        auto &vg = g[i][j][l][r][0][1];
                        for (int p = 1; p <= l; p ++ )
                            for (int q = r; q <= m; q ++ )
                                for (int x = 0; x <= 1; x ++ )
                                    for (int y = 0; y <= 1; y ++ )
                                    {
                                        int val = f[i - 1][j - (r - l + 1)][p][q][x][y];
                                        if (vf < val)
                                        {
                                            vf = val;
                                            vg = {i - 1, j - (r - l + 1), p, q, x, y};
                                        }
                                    }
                        for (int u = l; u <= r; u ++ ) vf += w[i][u];
                    }
                }

    int res = 0;
    State state;

    for (int i = 1; i <= n; i ++ )
        for (int l = 1; l <= m; l ++ )
            for (int r = 1; r <= m; r ++ )
                for (int x = 0; x <= 1; x ++ )
                    for (int y = 0; y <= 1; y ++ )
                    {
                        int val = f[i][k][l][r][x][y];
                        if (res < val)
                        {
                            res = val;
                            state = {i, k, l, r, x, y};
                        }
                    }

    printf("Oil : %d\n", res);

    while (state.j)
    {
        for (int i = state.l; i <= state.r; i ++ ) printf("%d %d\n", state.i, i);
        state = g[state.i][state.j][state.l][state.r][state.x][state.y];
    }

    return 0;
}
```  
##### AcWing277   饼干
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

const int N = 31, M = 5010;

int n, m;
PII g[N];
int s[N];
int f[N][M];
int ans[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> g[i].first;
        g[i].second = i;
    }
    sort(g + 1, g + n + 1);
    reverse(g + 1, g + n + 1);

    for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + g[i].first;

    memset(f, 0x3f, sizeof f);
    f[0][0] = 0;

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            if (j >= i) f[i][j] = f[i][j - i];
            for (int k = 1; k <= i && k <= j; k ++ )
                f[i][j] = min(f[i][j], f[i - k][j - k] + (s[i] - s[i - k]) * (i - k));
        }

    cout << f[n][m] << endl;

    int i = n, j = m, h = 0;
    while (i && j)
    {
        if (j >= i && f[i][j] == f[i][j - i]) j -= i, h ++ ;
        else
        {
            for (int k = 1; k <= i && k <= j; k ++ )
                if (f[i][j] == f[i - k][j - k] + (s[i] - s[i - k]) * (i - k))
                {
                    for (int u = i; u > i - k; u -- )
                        ans[g[u].second] = 1 + h;
                    i -= k, j -= k;
                    break;
                }

        }
    }

    for (int i = 1; i <= n; i ++ ) cout << ans[i] << ' ';
    cout << endl;

    return 0;
}
```  

## 0x52 背包

    
##### AcWing278   数字组合
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = 10010;

int n, m;
int v[N];
int f[M];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ) cin >> v[i];
    f[0] = 1;
    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= v[i]; j -- )
            f[j] += f[j - v[i]];

    cout << f[m] << endl;

    return 0;
}
```

##### AcWing279   自然数拆分
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 4010;

int n;
unsigned f[N];

int main()
{
    cin >> n;

    f[0] = 1;
    for (int i = 1; i <= n; i ++ )
        for (int j = i; j <= n; j ++ )
            f[j] += f[j - i];

    cout << (f[n] - 1) % 2147483648u << endl;

    return 0;
}
```

##### AcWing280   陪审团
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 210, M = 810, base = 400;

int n, m;
int p[N], d[N];
int f[N][21][M];
int ans[N];

int main()
{
    int T = 1;
    while (scanf("%d%d", &n, &m), n || m)
    {
        for (int i = 1; i <= n; i ++ ) scanf("%d%d", &p[i], &d[i]);

        memset(f, -0x3f, sizeof f);
        f[0][0][base] = 0;

        for (int i = 1; i <= n; i ++ )
            for (int j = 0; j <= m; j ++ )
                for (int k = 0; k < M; k ++ )
                {
                    f[i][j][k] = f[i - 1][j][k];
                    int t = k - (p[i] - d[i]);
                    if (t < 0 || t >= M) continue;
                    if (j < 1) continue;
                    f[i][j][k] = max(f[i][j][k], f[i - 1][j - 1][t] + p[i] + d[i]);
                }

        int v = 0;
        while (f[n][m][base - v] < 0 && f[n][m][base + v] < 0) v ++ ;

        if (f[n][m][base - v] > f[n][m][base + v]) v = base - v;
        else v = base + v;

        int cnt = 0;
        int i = n, j = m, k = v;
        while (j)
        {
            if (f[i][j][k] == f[i - 1][j][k]) i -- ;
            else
            {
                ans[cnt ++ ] = i;
                k -= (p[i] - d[i]);
                i --, j -- ;
            }
        }

        int sp = 0, sd = 0;
        for (int i = 0; i < cnt; i ++ )
        {
            sp += p[ans[i]];
            sd += d[ans[i]];
        }

        printf("Jury #%d\n", T ++ );
        printf("Best jury has value %d for prosecution and value %d for defence:\n", sp, sd);

        sort(ans, ans + cnt);
        for (int i = 0; i < cnt; i ++ ) printf(" %d", ans[i]);
        puts("\n");
    }

    return 0;
}
``` 
##### AcWing281   硬币
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = 100010;

int n, m;
int f[M], g[M];
int v[N], s[N];

int main()
{
    while (scanf("%d%d", &n, &m), n || m)
    {
        memset(f, 0, sizeof f);
        for (int i = 1; i <= n; i ++ ) scanf("%d", &v[i]);
        for (int i = 1; i <= n; i ++ ) scanf("%d", &s[i]);

        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
        {
            memset(g, 0, sizeof g);
            for (int j = v[i]; j <= m; j ++ )
                if (!f[j] && f[j - v[i]] && g[j - v[i]] < s[i])
                {
                    g[j] = g[j - v[i]] + 1;
                    f[j] = 1;
                }
        }

        int res = 0;
        for (int i = 1; i <= m; i ++ ) res += f[i];

        printf("%d\n", res);
    }

    return 0;
}
``` 

  

## 0x53 区间DP

     
  
##### AcWing282   石子合并
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 310, INF = 0x3f3f3f3f;

int n;
int w[N], s[N];
int f[N][N];

int main()
{
    cin >> n;

    for (int i = 1; i <= n; i ++ ) cin >> w[i];
    for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + w[i];

    for (int len = 2; len <= n; len ++ )
        for (int l = 1; l + len - 1 <= n; l ++ )
        {
            int r = l + len - 1;
            f[l][r] = INF;
            for (int k = l; k < r; k ++ )
                f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
        }

    cout << f[1][n] << endl;

    return 0;
}

```
  
##### AcWing283   多边形
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, INF = 32768;

int n;
int w[N];
char c[N];
int f[N][N], g[N][N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> c[i] >> w[i];
        c[i + n] = c[i];
        w[i + n] = w[i];
    }

    for (int len = 1; len <= n; len ++ )
        for (int l = 1; l + len - 1 <= n * 2; l ++ )
        {
            int r = l + len - 1;
            if (len == 1) f[l][r] = g[l][r] = w[l];
            else
            {
                f[l][r] = -INF, g[l][r] = INF;
                for (int k = l; k < r; k ++ )
                {
                    char op = c[k + 1];
                    int minl = g[l][k], minr = g[k + 1][r];
                    int maxl = f[l][k], maxr = f[k + 1][r];
                    if (op == 't')
                    {
                        f[l][r] = max(f[l][r], maxl + maxr);
                        g[l][r] = min(g[l][r], minl + minr);
                    }
                    else
                    {
                        int x1 = maxl * maxr, x2 = maxl * minr, x3 = minl * maxr, x4 = minl * minr;
                        f[l][r] = max(f[l][r], max(max(x1, x2), max(x3, x4)));
                        g[l][r] = min(g[l][r], min(min(x1, x2), min(x3, x4)));
                    }
                }
            }
        }

    int res = -INF;
    for (int i = 1; i <= n; i ++ ) res = max(res, f[i][i + n - 1]);
    cout << res << endl;

    for (int i = 1; i <= n; i ++ )
        if (res == f[i][i + n - 1])
            cout << i << ' ';

    return 0;
}
```
##### AcWing284   金字塔
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <string.h>

using namespace std;

typedef long long LL;

const int N = 310, mod = 1e9;

char str[N];
int f[N][N];

int main()
{
    cin >> str + 1;
    int n = strlen(str + 1);
    if (n % 2 == 0) cout << 0 << endl;
    else
    {
        for (int len = 1; len <= n; len += 2)
            for (int l = 1; l + len - 1 <= n; l ++ )
            {
                int r = l + len - 1;
                if (len == 1) f[l][r] = 1;
                else if (str[l] == str[r])
                {
                    for (int k = l; k < r; k += 2)
                        if (str[k] == str[r])
                            f[l][r] = (f[l][r] + (LL)f[l][k] * f[k + 1][r - 1]) % mod;
                }
            }

        cout << f[1][n] << endl;
    }

    return 0;
}
```

## 0x54 树形DP

##### AcWing285   没有上司的舞会
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 6010, INF = 0x3f3f3f3f;

int n;
int h[N], e[N], w[N], ne[N], idx;
int f[N][2];
bool st[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    f[u][1] = w[u];

    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        dfs(j);
        f[u][0] += max(f[j][0], f[j][1]);
        f[u][1] += f[j][0];
    }
}

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(b, a);
        st[a] = true;
    }

    int root = 1;
    while (st[root]) root ++ ;

    dfs(root);

    printf("%d\n", max(f[root][0], f[root][1]));

    return 0;
}
```
  
##### AcWing286   选课
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 310;

int n, m;
int h[N], e[N], ne[N], idx;
int w[N];
int f[N][N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    for (int i = h[u]; ~i; i = ne[i])
    {
        int son = e[i];
        dfs(son);

        for (int j = m - 1; j >= 0; j -- )
            for (int k = 1; k <= j; k ++ )
                f[u][j] = max(f[u][j], f[u][j - k] + f[son][k]);
    }

    for (int i = m; i; i -- ) f[u][i] = f[u][i - 1] + w[u];
    f[u][0] = 0;
}

int main()
{
    cin >> n >> m;

    memset(h, -1, sizeof h);
    for (int i = 1; i <= n; i ++ )
    {
        int father;
        cin >> father >> w[i];
        add(father, i);
    }

    m ++ ;
    dfs(0);

    cout << f[0][m] << endl;

    return 0;
}
```

##### AcWing287   积蓄程度
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 200010, M = N * 2, INF = 0x3f3f3f3f;

int n;
int h[N], e[M], w[M], ne[M], idx;
int d[N], f[N], deg[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int dfs_d(int u, int fa)
{
    if (deg[u] == 1)
    {
        d[u] = INF;
        return d[u];
    }

    d[u] = 0;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (j == fa) continue;
        d[u] += min(w[i], dfs_d(j, u));
    }

    return d[u];
}

void dfs_f(int u, int fa)
{
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (j == fa) continue;
        if (deg[j] == 1) f[j] = min(w[i], f[u] - w[i]);
        else
        {
            f[j] = d[j] + min(f[u] - min(d[j], w[i]), w[i]);
            dfs_f(j, u);
        }
    }
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d", &n);
        memset(h, -1, sizeof h);
        idx = 0;
        memset(deg, 0, sizeof deg);

        for (int i = 0; i < n - 1; i ++ )
        {
            int a, b, c;
            scanf("%d%d%d", &a, &b, &c);
            add(a, b, c), add(b, a, c);
            deg[a] ++ , deg[b] ++ ;
        }

        int root = 1;
        while (root <= n && deg[root] == 1) root ++ ;

        if (root > n)
        {
            cout << w[0] << endl;
            continue;
        }

        dfs_d(root, -1);
        f[root] = d[root];
        dfs_f(root, -1);

        int res = 0;
        for (int i = 1; i <= n; i ++ ) res = max(res, f[i]);

        printf("%d\n", res);
    }

    return 0;
}
```
  

## 0x55 环形与后效性处理

     
  
##### AcWing288   休息时间
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 4000, INF = 0x3f3f3f3f;

int n, m;
int f[2][N][2];
int w[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ) cin >> w[i];

    memset(f, -0x3f, sizeof f);
    f[1][0][0] = f[1][1][1] = 0;
    // 第n小时不在睡觉
    for (int i = 2; i <= n; i ++ )
        for (int j = 0; j <= m; j ++ )
        {
            f[i & 1][j][0] = max(f[i - 1 & 1][j][0], f[i - 1 & 1][j][1]);
            f[i & 1][j][1] = -INF;
            if (j) f[i & 1][j][1] = max(f[i - 1 & 1][j - 1][0], f[i - 1 & 1][j - 1][1] + w[i]);
        }

    int res = f[n & 1][m][0];

    // 第n小时在睡觉
    memset(f, -0x3f, sizeof f);
    f[1][1][1] = w[1];
    f[1][0][0] = 0;
    for (int i = 2; i <= n; i ++ )
        for (int j = 0; j <= m; j ++ )
        {
            f[i & 1][j][0] = max(f[i - 1 & 1][j][0], f[i - 1 & 1][j][1]);
            f[i & 1][j][1] = -INF;
            if (j) f[i & 1][j][1] = max(f[i - 1 & 1][j - 1][0], f[i - 1 & 1][j - 1][1] + w[i]);
        }

    res = max(res, f[n & 1][m][1]);

    cout << res << endl;

    return 0;
}
```


##### AcWing289   环路运输
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 2000010;

int n;
int w[N];
int q[N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d", &w[i]);
        w[i + n] = w[i];
    }

    int res = 0;

    int hh = 0, tt = -1;
    int len = n / 2;
    for (int i = 1; i <= n * 2; i ++ )
    {
        if (hh <= tt && q[hh] < i - len) hh ++ ;
        res = max(res, i - q[hh] + w[q[hh]] + w[i]);
        while (hh <= tt && w[q[tt]] - q[tt] <= w[i] - i) tt -- ;
        q[ ++ tt] = i;
    }

    printf("%d\n", res);

    return 0;
}

```  
##### AcWing290   坏掉的机器人
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <iomanip>

using namespace std;

const int N = 1010;

int n, m;
int x, y;
double f[N][N];
double a[N][N];

void gauss()
{
    for (int i = 1; i <= m; i ++ )
    {
        double  t = a[i + 1][i] / a[i][i];
        int d[3] = {i, i + 1, m + 1};
        for (int j = 0; j < 3; j ++ )
            a[i + 1][d[j]] -= t * a[i][d[j]];
        a[i + 1][i] = 0;
    }

    for (int i = m; i; i -- )
    {
        a[i - 1][m + 1] -= a[i - 1][i] / a[i][i] * a[i][m + 1];
        a[i - 1][i] = 0;
    }
}

int main()
{
    cin >> n >> m;
    cin >> x >> y;

    if (m == 1) printf("%.4lf\n", 2.0 * (n - x));
    else
    {
        for (int i = n - 1; i >= x; i -- )
        {
            a[1][1] = 2.0 / 3, a[1][2] = -1.0 / 3, a[1][m + 1] = f[i + 1][1] / 3 + 1;
            a[m][m] = 2.0 / 3, a[m][m - 1] = -1.0 / 3, a[m][m + 1] = f[i + 1][m] / 3 + 1;
            for (int j = 2; j < m; j ++ )
            {
                a[j][j - 1] = -1.0 / 4, a[j][j] = 3.0 / 4, a[j][j + 1] = -1.0 / 4;
                a[j][m + 1] = f[i + 1][j] / 4 + 1;
            }

            gauss();

            for (int j = 1; j <= m; j ++ ) f[i][j] = a[j][m + 1] / a[j][j];
        }

        cout.setf(std::ios::fixed);
        cout << setprecision(4) << f[x][y];
        //cout << f[x][y];
    }

    return 0;
}
```  
  

## 0x56 状态压缩DP

##### AcWing291   蒙德里安的梦想
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long LL;

const int N = 12, M = 1 << N;

int n, m;
LL f[N][M];
vector<int> state[M];
bool st[M];

int main()
{
    while (cin >> n >> m, n || m)
    {
        for (int i = 0; i < 1 << n; i ++ )
        {
            int cnt = 0;
            bool is_valid = true;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                {
                    if (cnt & 1)
                    {
                        is_valid = false;
                        break;
                    }
                    cnt = 0;
                }
                else cnt ++ ;
            if (cnt & 1) is_valid = false;
            st[i] = is_valid;
        }

        for (int i = 0; i < 1 << n; i ++ )
        {
            state[i].clear();
            for (int j = 0; j < 1 << n; j ++ )
                if ((i & j) == 0 && st[i | j])
                    state[i].push_back(j);
        }

        memset(f, 0, sizeof f);
        f[0][0] = 1;
        for (int i = 1; i <= m; i ++ )
            for (int j = 0; j < 1 << n; j ++ )
                for (auto k : state[j])
                    f[i][j] += f[i - 1][k];

        cout << f[m][0] << endl;
    }

    return 0;
}
```
##### AcWing292   炮兵阵地
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 110, M = 10, S = 1 << M;

int n, m;
int g[N];
int f[2][S][S];
vector<int> state;
int cnt[S];

bool check(int s)
{
    for (int i = 0; i < m; i ++ )
        if ((s >> i & 1) && ((s >> i + 1 & 1) || (s >> i + 2 & 1)))
            return false;
    return true;
}

int count(int s)
{
    int res = 0;
    while (s)
    {
        res += s & 1;
        s >>= 1;
    }
    return res;
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0; j < m; j ++ )
        {
            char c;
            cin >> c;
            if (c == 'H') g[i] += 1 << j;
        }
    }

    for (int i = 0; i < 1 << m; i ++ )
        if (check(i))
        {
            state.push_back(i);
            cnt[i] = count(i);
        }

    for (int i = 0; i < n + 2; i ++ )
        for (int j = 0; j < state.size(); j ++ )
            for (int k = 0; k < state.size(); k ++ )
                for (int u = 0; u < state.size(); u ++ )
                {
                    int a = state[u], b = state[j], c = state[k];
                    if ((a & b) || (a & c) || (b & c)) continue;
                    if (g[i] & c) continue;
                    f[i & 1][j][k] = max(f[i & 1][j][k], f[i - 1 & 1][u][j] + cnt[c]);
                }

    cout << f[n + 1 & 1][0][0] << endl;

    return 0;
}
```
##### AcWing529   宝藏
```cpp

```

## 0x57 倍增优化DP

##### AcWing293   开车旅行
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <set>

using namespace std;

typedef long long LL;
typedef pair<LL, int> PLI;

const int N = 100010, M = 17;
const LL INF = 1e12;

int n;
int h[N];
int ga[N], gb[N];
int f[M][N][2];
LL da[M][N][2], db[M][N][2];

void init_g()
{
    set<PLI> S;
    S.insert({INF, 0}), S.insert({INF + 1, 0});
    S.insert({-INF, 0}), S.insert({-INF - 1, 0});

    for (int i = n; i; i -- )
    {
        PLI t(h[i], i);
        auto j = S.lower_bound(t);
        j ++ ;
        vector<PLI> cand;
        for (int k = 0; k < 4; k ++ )
        {
            cand.push_back(*j);
            j -- ;
        }
        LL d1 = INF, d2 = INF;
        int p1 = 0, p2 = 0;
        for (int k = 3; k >= 0; k -- )
        {
            LL d = abs(h[i] - cand[k].first);
            if (d < d1)
            {
                d2 = d1, d1 = d;
                p2 = p1, p1 = cand[k].second;
            }
            else if (d < d2)
            {
                d2 = d;
                p2 = cand[k].second;
            }
        }
        ga[i] = p2, gb[i] = p1;

        S.insert(t);
    }
}

void init_f()
{
    for (int i = 0; i < M; i ++ )
        for (int j = 1; j <= n; j ++ )
        {
            if (!i) f[0][j][0] = ga[j], f[0][j][1] = gb[j];
            else
            {
                for (int k = 0; k < 2; k ++ )
                {
                    if (i == 1) f[1][j][k] = f[0][f[0][j][k]][1 - k];
                    else f[i][j][k] = f[i - 1][f[i - 1][j][k]][k];
                }
            }
        }
}

int get_dist(int a, int b)
{
    return abs(h[a] - h[b]);
}

void init_d()
{
    for (int i = 0; i < M; i ++ )
        for (int j = 1; j <= n; j ++ )
        {
            if (!i)
            {
                da[0][j][0] = get_dist(j, ga[j]), da[0][j][1] = 0;
                db[0][j][1] = get_dist(j, gb[j]), db[0][j][0] = 0;
            }
            else
            {
                for (int k = 0; k < 2; k ++ )
                {
                    if (i == 1)
                    {
                        da[1][j][k] = da[0][j][k] + da[0][f[0][j][k]][1 - k];
                        db[1][j][k] = db[0][j][k] + db[0][f[0][j][k]][1 - k];
                    }
                    else
                    {
                        da[i][j][k] = da[i - 1][j][k] + da[i - 1][f[i - 1][j][k]][k];
                        db[i][j][k] = db[i - 1][j][k] + db[i - 1][f[i - 1][j][k]][k];
                    }
                }
            }
        }
}

void calc(int p, int x, int &la, int &lb)
{
    la = lb = 0;
    for (int i = M - 1; i >= 0; i -- )
        if (f[i][p][0] && la + lb + da[i][p][0] + db[i][p][0] <= x)
        {
            la += da[i][p][0], lb += db[i][p][0];
            p = f[i][p][0];
        }
}

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &h[i]);

    init_g();
    init_f();
    init_d();

    int p, x;
    scanf("%d", &x);
    int res = 0, max_h = 0;
    double min_ratio = INF;
    for (int i = 1; i <= n; i ++ )
    {
        int la, lb;
        calc(i, x, la, lb);
        double ratio = lb ? (double)la / lb : INF;
        if (ratio < min_ratio || ratio == min_ratio && h[i] > max_h)
        {
            min_ratio = ratio;
            max_h = h[i];
            res = i;
        }
    }

    printf("%d\n", res);

    int m;
    scanf("%d", &m);
    while (m -- )
    {
        scanf("%d%d", &p, &x);
        int la, lb;
        calc(p, x, la, lb);
        printf("%d %d\n", la, lb);
    }

    return 0;
}
``` 
##### AcWing294   计算重复  
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 110, M = 31;

int n1, n2;
string s1, s2;
LL f[N][M];

int main()
{
    while (cin >> s2 >> n2 >> s1 >> n1)
    {
        bool fail = false;

        int sz = s1.size();
        for (int i = 0; i < sz; i ++ )
        {
            int p = i;
            f[i][0] = 0;
            for (int j = 0; j < s2.size(); j ++ )
            {
                int cnt = 0;
                while (s1[p] != s2[j])  // 找到下一个s2[j]
                {
                    p = (p + 1) % sz;
                    cnt ++ ;
                    if (cnt >= sz)
                    {
                        fail = true;
                        break;
                    }
                }
                if (fail) break;
                p = (p + 1) % sz;
                f[i][0] += cnt + 1;
            }
            if (fail) break;
        }

        if (fail)
        {
            cout << 0 << endl;
            continue;
        }

        // 预处理
        for (int j = 1; j <= 30; j ++ )
            for (int i = 0; i < sz; i ++ )
                f[i][j] = f[i][j - 1] + f[(i + f[i][j - 1]) % sz][j - 1];

        // 拼凑
        LL res = 0;
        for (int i = 0; i < sz; i ++ )
        {
            LL p = i, t = 0;
            for (int k = 30; k >= 0; k -- )
                if (p + f[p % sz][k] <= sz * n1)
                {
                    p += f[p % sz][k];
                    t += 1 << k;
                }
            res = max(res, t);
        }

        cout << res / n2 << endl;
    }

    return 0;
}
```

## 0x58 数据结构优化DP

     
  
##### AcWing295   清理班次
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 25010, T = 1000010, INF = 1e8;

int n, m;
struct Range
{
    int l, r;
    bool operator< (const Range &t)const
    {
        return r < t.r;
    }
}range[N];

struct Node
{
    int l, r, v;
}tr[T * 4];

void pushup(int u)
{
    tr[u].v = min(tr[u << 1].v, tr[u << 1 | 1].v);
}

void build(int u, int l, int r)
{
    tr[u] = {l, r, INF};
    if (l == r) return;
    int mid = l + r >> 1;
    build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
}

void update(int u, int k, int v)
{
    if (tr[u].l == tr[u].r)
    {
        tr[u].v = min(tr[u].v, v);
        return;
    }
    int mid = tr[u].l + tr[u].r >> 1;
    if (k <= mid) update(u << 1, k, v);
    else update(u << 1 | 1, k, v);
    pushup(u);
}

int query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].v;
    int mid = tr[u].l + tr[u].r >> 1;
    int res = INF;
    if (l <= mid) res = query(u << 1, l, r);
    if (r > mid) res = min(res, query(u << 1 | 1, l, r));
    return res;
}

int main()
{
    scanf("%d%d", &n, &m);
    build(1, 0, m);

    for (int i = 0; i < n; i ++ ) scanf("%d%d", &range[i].l, &range[i].r);
    sort(range, range + n);

    update(1, 0, 0);
    for (int i = 0; i < n; i ++ )
    {
        int l = range[i].l, r = range[i].r;
        int v = query(1, l - 1, r - 1) + 1;  // f[r]
        update(1, r, v);
    }

    int res = query(1, m, m);
    if (res == INF) res = -1;

    printf("%d\n", res);

    return 0;
}
```  
##### AcWing296   清理班次
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 10010, M = 90000;
const LL INF = 1e15;

int n, m, e;
struct Range
{
    int l, r, w;
    bool operator< (const Range &t) const
    {
        return r < t.r;
    }
}range[N];

struct Node
{
    int l, r;
    LL v;
}tr[M * 4];

void pushup(int u)
{
    tr[u].v = min(tr[u << 1].v, tr[u << 1 | 1].v);
}

void build(int u, int l, int r)
{
    tr[u] = {l, r, INF};
    if (l == r) return;
    int mid = l + r >> 1;
    build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
}

void update(int u, int k, LL v)
{
    if (tr[u].l == tr[u].r)
    {
        tr[u].v = min(tr[u].v, v);
        return;
    }

    int mid = tr[u].l + tr[u].r >> 1;
    if (k <= mid) update(u << 1, k, v);
    else update(u << 1 | 1, k, v);
    pushup(u);
}

LL query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].v;

    int mid = tr[u].l + tr[u].r >> 1;
    LL res = INF;
    if (l <= mid) res = query(u << 1, l, r);
    if (r > mid) res = min(res, query(u << 1 | 1, l, r));
    return res;
}

int main()
{
    scanf("%d%d%d", &n, &m, &e);
    build(1, m - 1, e);
    update(1, m - 1, 0);

    for (int i = 0; i < n; i ++ )
    {
        int l, r, w;
        scanf("%d%d%d", &l, &r, &w);
        range[i] = {l, r, w};
    }

    sort(range, range + n);

    for (int i = 0; i < n; i ++ )
    {
        int l = range[i].l, r = range[i].r, w = range[i].w;
        LL v = query(1, l - 1, r - 1) + w;  // f[r]
        update(1, r, v);
    }

    LL res = query(1, e, e);
    if (res == INF) res = -1;

    printf("%lld\n", res);

    return 0;
}
```  
##### AcWing297   赤壁之战
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, mod = 1e9 + 7;

int n, m;
int a[N];
int nums[N], cnt;
int tr[N];
int f[N][N];

int lowbit(int x)
{
    return x & -x;
}

void add(int x, int v)
{
    for (int i = x; i <= cnt ; i += lowbit(i))
        tr[i] = (tr[i] + v) % mod;
}

int sum(int x)
{
    int res = 0;
    for (int i = x; i; i -= lowbit(i))
        res = (res + tr[i]) % mod;
    return res;
}

int main()
{
    int T;
    scanf("%d", &T);
    for (int C = 1; C <= T; C ++ )
    {
        scanf("%d%d", &n, &m);
        cnt = 0;
        for (int i = 1; i <= n; i ++ )
        {
            scanf("%d", &a[i]);
            nums[cnt ++ ] = a[i];
        }
        sort(nums, nums + cnt);
        cnt = unique(nums, nums + cnt) - nums;
        for (int i = 1; i <= n; i ++ ) a[i] = lower_bound(nums, nums + cnt, a[i]) - nums + 1;

        for (int i = 1; i <= n; i ++ ) f[i][1] = 1;
        for (int j = 2; j <= m; j ++ )
        {
            for (int i = 1; i <= cnt; i ++ ) tr[i] = 0;
            for (int i = 1; i <= n; i ++ )
            {
                f[i][j] = sum(a[i] - 1);
                add(a[i], f[i][j - 1]);
            }
        }

        int res = 0;
        for (int i = 1; i <= n; i ++ ) res = (res + f[i][m]) % mod;

        printf("Case #%d: %d\n", C, res);
    }

    return 0;
}
```  

## 0x59 单调队列优化DP

##### AcWing298   围栏
```cpp
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>

using namespace std;

const int N = 16010, M = 110;

int n, m;
int q[N];
int f[M][N];

struct Carpenter
{
    int l, p, s;
    bool operator< (const Carpenter& t) const
    {
        return s < t.s;
    }
}car[M];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= m; i ++ ) cin >> car[i].l >> car[i].p >> car[i].s;
    sort(car + 1, car + m + 1);

    for (int i = 1; i <= m; i ++ )
    {
        int hh = 0, tt = -1;
        for (int j = 0; j <= n; j ++ )
        {
            f[i][j] = f[i - 1][j];
            if (j) f[i][j] = max(f[i][j], f[i][j - 1]);

            int l = car[i].l, p = car[i].p, s = car[i].s;
            if (hh <= tt && q[hh] < j - l) hh ++ ;
            if (j >= s && hh <= tt)
            {
                int k = q[hh];
                f[i][j] = max(f[i][j], f[i - 1][k] + (j - k) * p);
            }

            if (j < s)
            {
                while (hh <= tt && f[i - 1][q[tt]] - q[tt] * p <= f[i - 1][j] - j * p) tt -- ;
                q[ ++ tt] = j;
            }
        }
    }

    cout << f[m][n] << endl;

    return 0;
}
```  
##### AcWing299   裁剪序列
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <set>

using namespace std;

typedef long long LL;

const int N = 100010;

int n;
LL m;
int a[N], q[N];
LL f[N];

multiset<LL> S;

void remove(LL x)
{
    auto it = S.find(x);
    S.erase(it);
}

int main()
{
    scanf("%d%lld", &n, &m);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d", &a[i]);
        if (a[i] > m)
        {
            puts("-1");
            return 0;
        }
    }

    int hh = 0, tt = 0;
    LL sum = 0;
    for (int i = 1, j = 0; i <= n; i ++ )
    {
        sum += a[i];
        while (sum > m) sum -= a[ ++ j];

        while (hh <= tt && q[hh] <= j)
        {
            if (hh < tt) remove(f[q[hh]] + a[q[hh + 1]]);
            hh ++ ;
        }
        int tail = tt;
        while (hh <= tt && a[q[tt]] <= a[i])
        {
            if (tt != tail) remove(f[q[tt]] + a[q[tt + 1]]);
            tt -- ;
        }
        if (hh <= tt && tt != tail) remove(f[q[tt]] + a[q[tt + 1]]);
        q[ ++ tt] = i;
        if (hh < tt) S.insert(f[q[tt - 1]] + a[q[tt]]);

        f[i] = f[j] + a[q[hh]];
        if (S.size()) f[i] = min(f[i], *S.begin());
    }

    printf("%lld\n", f[n]);

    return 0;
}

```  

## 0x5A 斜率优化

##### AcWing300   任务安排1
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 5010;

int n, s;
int sc[N], st[N];
int f[N];

int main()
{
    scanf("%d%d", &n, &s);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d%d", &st[i], &sc[i]);
        st[i] += st[i - 1];
        sc[i] += sc[i - 1];
    }

    memset(f, 0x3f, sizeof f);
    f[0] = 0;

    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j < i; j ++ )
            f[i] = min(f[i], f[j] + (sc[i] - sc[j]) * st[i] + s * (sc[n] - sc[j]));

    printf("%d\n", f[n]);

    return 0;
}
```    
##### AcWing301   任务安排2
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 300010;

int n, s;
LL sc[N], st[N];
LL f[N];
int q[N];

int main()
{
    scanf("%d%d", &n, &s);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%lld%lld", &st[i], &sc[i]);
        st[i] += st[i - 1];
        sc[i] += sc[i - 1];
    }

    int hh = 0, tt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        while (hh < tt && (f[q[hh + 1]] - f[q[hh]]) <= (st[i] + s) * (sc[q[hh + 1]] - sc[q[hh]])) hh ++ ;

        f[i] = f[q[hh]] - (st[i] + s) * sc[q[hh]] + sc[i] * st[i] + s * sc[n];
        while (hh < tt && (f[q[tt]] - f[q[tt - 1]]) * (sc[i] - sc[q[tt]]) >= (f[i] - f[q[tt]]) * (sc[q[tt]] - sc[q[tt - 1]])) tt -- ;
        q[ ++ tt] = i;
    }

    printf("%lld\n", f[n]);

    return 0;
}
```   
##### AcWing302   任务安排3
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 300010;

int n, s;
LL t[N], c[N];
LL f[N];
int q[N];

int main()
{
    scanf("%d%d", &n, &s);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%lld%lld", &t[i], &c[i]);
        t[i] += t[i - 1];
        c[i] += c[i - 1];
    }

    int hh = 0, tt = 0;
    q[0] = 0;

    for (int i = 1; i <= n; i ++ )
    {
        int l = hh, r = tt;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (f[q[mid + 1]] - f[q[mid]] >= (t[i] + s) * (c[q[mid + 1]] - c[q[mid]])) r = mid;
            else l = mid + 1;
        }

        int j = q[r];
        f[i] = f[j] -   (t[i] + s) * c[j] + t[i] * c[i] + s * c[n];
        while (hh < tt && (double)(f[q[tt]] - f[q[tt - 1]]) * (c[i] - c[q[tt - 1]]) >= (double)(f[i] - f[q[tt - 1]]) * (c[q[tt]] - c[q[tt - 1]])) tt -- ;
        q[ ++ tt] = i;
    }

    printf("%lld\n", f[n]);

    return 0;
}
```    
##### AcWing303   运输小猫
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010, M = 100010, P = 110;

int n, m, p;
LL d[N], t[N], a[N], s[N];
LL f[P][M];
int q[M];

LL get_y(int k, int j)
{
    return f[j - 1][k] + s[k];
}

int main()
{
    scanf("%d%d%d", &n, &m, &p);

    for (int i = 2; i <= n; i ++ )
    {
        scanf("%lld", &d[i]);
        d[i] += d[i - 1];
    }

    for (int i = 1; i <= m; i ++ )
    {
        int h;
        scanf("%d%lld", &h, &t[i]);
        a[i] = t[i] - d[h];
    }

    sort(a + 1, a + m + 1);

    for (int i = 1; i <= m; i ++ ) s[i] = s[i - 1] + a[i];

    memset(f, 0x3f, sizeof f);
    for (int i = 0; i <= p; i ++ ) f[i][0] = 0;

    for (int j = 1; j <= p; j ++ )
    {
        int hh = 0, tt = 0;
        q[0] = 0;

        for (int i = 1; i <= m; i ++ )
        {
            while (hh < tt && (get_y(q[hh + 1], j) - get_y(q[hh], j)) <= a[i] * (q[hh + 1] - q[hh])) hh ++ ;
            int k = q[hh];
            f[j][i] = f[j - 1][k] - a[i] * k + s[k] + a[i] * i - s[i];
            while (hh < tt && (get_y(q[tt], j) - get_y(q[tt - 1], j)) * (i - q[tt]) >=
                (get_y(i, j) - get_y(q[tt], j)) * (q[tt] - q[tt - 1])) tt -- ;
            q[ ++ tt] = i;
        }
    }

    printf("%lld\n", f[p][m]);

    return 0;
}
```    

## 0x5B 四边形不等式

     
  
##### AcWing304   诗人小G  
##### AcWing2889   再探石子合并
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 5010, INF = 0x3f3f3f3f;

int n;
int s[N], f[N][N], p[N][N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d", &s[i]);
        s[i] += s[i - 1];
    }

    for (int i = 1; i <= n; i ++ ) p[i][i] = i;
    for (int len = 2; len <= n; len ++ )
        for (int i = 1; i + len - 1 <= n; i ++ )
        {
            int j = i + len - 1;
            f[i][j] = INF;
            for (int k = p[i][j - 1]; k <= p[i + 1][j]; k ++ )
            {
                int t = f[i][k] + f[k + 1][j] + s[j] - s[i - 1];
                if (f[i][j] > t)
                {
                    f[i][j] = t;
                    p[i][j] = k;
                }
            }
        }

    printf("%d\n", f[1][n]);
    return 0;
}
```

 
##### AcWing305   一个古老的石头游戏  

## 0x5C 计数类DP
  
##### AcWing306   杰拉尔德和巨型象棋
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

typedef pair<int, int> PII;
const int N = 200010, M = 2010, mod = 1e9 + 7;

int n, m, k;
PII cells[M];
int jc[N], jcinv[N];
int f[M];

int qmi(int a, int b, int p)
{
    int res = 1;
    while (b)
    {
        if (b & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        b >>= 1;
    }
    return res;
}

int C(int b, int a)
{
    return (LL)jc[a] * jcinv[a - b] % mod * jcinv[b] % mod;
}

int main()
{
    jc[0] = jcinv[0] = 1;
    for (int i = 1; i < N; i ++ )
    {
        jc[i] = jc[i - 1] * (LL)i % mod;
        jcinv[i] = jcinv[i - 1] * (LL)qmi(i, mod - 2, mod) % mod;
    }

    cin >> n >> m >> k;

    for (int i = 1; i <= k; i ++ ) cin >> cells[i].first >> cells[i].second;
    sort(cells + 1, cells + 1 + k);
    cells[k + 1] = {n, m};

    f[0] = 1;
    for (int i = 1; i <= k + 1; i ++ )
    {
        int x = cells[i].first, y = cells[i].second;
        f[i] = C(x - 1, x + y - 2);
        for (int j = 1; j < i; j ++ )
            if (cells[j].first <= x && cells[j].second <= y)
            {
                int a = cells[j].first, b = cells[j].second;
                f[i] = (f[i] - (LL)f[j] * C(x - a, x - a + y - b)) % mod;
            }
    }

    cout << (f[k + 1] + mod) % mod << endl;

    return 0;
}
```

##### AcWing307   连通图
```cpp
import sys


def power(k):
    res = 1
    for i in range(k * (k - 1) // 2):
        res *= 2
    return res


def C(a, b):
    res = 1
    for i in range(1, a + 1): res *= i
    for i in range(1, b + 1): res //= i
    for i in range(1, a - b + 1): res //= i
    return res


for line in sys.stdin:
    n = int(line)
    if n == 0:
        break
    f = [0 for _ in range(n + 1)]
    f[1] = 1
    for i in range(n + 1):
        f[i] = power(i)
        for j in range(1, i):
            f[i] -= f[j] * C(i - 1, i - j) * power(i - j)
    print(f[n])
```

##### AcWing308   它们中的多少个
```cpp

``` 
##### AcWing309   装饰围栏
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;
const int N = 21;

LL f[N][N][2];

void init()
{
    f[1][1][0] = f[1][1][1] = 1;
    for (int i = 2; i < N; i ++ )
        for (int j = 1; j <= i; j ++ )
        {
            for (int k = j; k < i; k ++ ) f[i][j][0] += f[i - 1][k][1];
            for (int k = 1; k < j; k ++ ) f[i][j][1] += f[i - 1][k][0];
        }
}

int main()
{
    init();

    int T;
    cin >> T;
    while (T -- )
    {
        int n;
        bool st[N];
        memset(st, false, sizeof st);

        LL m;
        cin >> n >> m;

        int last, k;
        for (int i = 1; i <= n; i ++ )
        {
            if (f[n][i][1] >= m)
            {
                last = i, k = 1;
                break;
            }
            else m -= f[n][i][1];

            if (f[n][i][0] >= m)
            {
                last = i, k = 0;
                break;
            }
            else m -= f[n][i][0];
        }

        st[last] = true;
        cout << last << ' ';

        for (int i = n - 1; i; i -- )
        {
            int lower = 0;
            for (int j = 1; j < last; j ++ )
                if (!st[j])
                    lower ++ ;
            if (k)
            {
                for (int j = 1; j <= lower; j ++ )
                    if (f[i][j][0] >= m)
                    {
                        last = j, k = 0;
                        break;
                    }
                    else m -= f[i][j][0];
            }
            else
            {
                for (int j = lower + 1; j <= i; j ++ )
                    if (f[i][j][1] >= m)
                    {
                        last = j, k = 1;
                        break;
                    }
                    else m -= f[i][j][1];
            }
            for (int j = 1; j <= n; j ++ )
                if (!st[j])
                {
                    last -- ;
                    if (!last)
                    {
                        last = j;
                        st[j] = true;
                        break;
                    }
                }
            cout << last << ' ';
        }
        cout << endl;
    }
    return 0;
}
```

## 0x5D 数位统计DP

    
##### AcWing310   启示录
```cpp

```  
##### AcWing311   月之谜
```cpp

```  

## 0x5E 总结与练习

##### AcWing312   乌龟棋
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 360, M = 41;

int n, m;
int a[N], b[5];
int f[M][M][M][M];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ) cin >> a[i];
    for (int j = 0; j < m; j ++ )
    {
        int t;
        cin >> t;
        b[t] ++ ;
    }

    f[0][0][0][0] = a[1];
    for (int A = 0; A <= b[1]; A ++ )
        for (int B = 0; B <= b[2]; B ++ )
            for (int C = 0; C <= b[3]; C ++ )
                for (int D = 0; D <= b[4]; D ++ )
                {
                    int i = 1 + A * 1 + B * 2 + C * 3 + D * 4;
                    if (!i) continue;
                    int &x = f[A][B][C][D];
                    if (A) x = max(x, f[A - 1][B][C][D] + a[i]);
                    if (B) x = max(x, f[A][B - 1][C][D] + a[i]);
                    if (C) x = max(x, f[A][B][C - 1][D] + a[i]);
                    if (D) x = max(x, f[A][B][C][D - 1] + a[i]);
                }

    cout << f[b[1]][b[2]][b[3]][b[4]] << endl;
    return 0;
}
```
##### AcWing313   花店橱窗
```cpp
#include <iostream>

using namespace std;

const int N = 110, INF = 1e9;

int n, m;
int g[N][N], f[N][N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            cin >> g[i][j];

    for (int j = 1; j <= m; j ++ ) f[n][j] = g[n][j];

    for (int i = n - 1; i; i -- )
        for (int j = m; j; j -- )
        {
            f[i][j] = -INF;
            for (int k = j + 1; k <= m; k ++ )
                f[i][j] = max(f[i][j], f[i + 1][k] + g[i][j]);
        }

    int j = 1;
    for (int i = 1; i <= m; i ++ )
        if (f[1][i] > f[1][j])
            j = i;

    cout << f[1][j] << endl;

    int i = 1;
    while (i <= n)
    {
        cout << j << ' ';
        for (int k = j + 1; k <= m; k ++ )
            if (f[i][j] == f[i + 1][k] + g[i][j])
            {
                j = k;
                break;
            }
        i ++ ;
    }

    cout << endl;

    return 0;
}
```
  
##### AcWing314   低买
```cpp
#include <iostream>

using namespace std;

const int N = 5010;

int n;
int a[N];
int f[N], g[N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ ) cin >> a[i];

    g[0] = 1;
    for (int i = 1; i <= n; i ++ )
    {
        for (int j = 0; j < i; j ++ )
            if (!j || a[j] > a[i])
                f[i] = max(f[i], f[j] + 1);

        for (int j = 1; j < i; j ++ )
            if (a[j] == a[i])
                f[j] = 0;

        for (int j = 0; j < i; j ++ )
            if ((!j || a[j] > a[i]) && f[i] == f[j] + 1)
                g[i] += g[j];
    }

    int res = 0, cnt = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, f[i]);
    for (int i = 1; i <= n; i ++ )
        if (f[i] == res)
            cnt += g[i];

    cout << res << ' ' << cnt << endl;
    return 0;
}
```
##### AcWing315   旅行
```cpp
#include <iostream>
#include <string.h>

using namespace std;

const int N = 110;

int n, m;
char s1[N], s2[N], path[N];
int f[N][N];

void dfs(int i, int j, int u, int len)
{
    if (u > len)
    {
        puts(path + 1);
        return;
    }

    if (s1[i] == s2[j])
    {
        path[u] = s1[i];
        dfs(i + 1, j + 1, u + 1, len);
    }
    else
    {
        for (int k = 0; k < 26; k ++ )
        {
            int a = 0; // 在s1中，下一个字母k出现在哪里
            int b = 0; // 在s2中，下一个字母k出现在哪里

            for (int x = i; x <= n; x ++ )
                if (s1[x] == 'a' + k)
                {
                    a = x;
                    break;
                }

            for (int x = j; x <= m; x ++ )
                if (s2[x] == 'a' + k)
                {
                    b = x;
                    break;
                }

            if (a && b && f[a][b] == f[i][j])
            {
                dfs(a, b, u, len);
            }
        }
    }
}

int main()
{
    scanf("%s%s", s1 + 1, s2 + 1);
    n = strlen(s1 + 1), m = strlen(s2 + 1);
    for (int i = n; i; i -- )
        for (int j = m; j; j -- )
            if (s1[i] == s2[j])
                f[i][j] = f[i + 1][j + 1] + 1;
            else
                f[i][j] = max(f[i + 1][j], f[i][j + 1]);

    dfs(1, 1, 1, f[1][1]);

    return 0;
}
```
  
##### AcWing316   减操作  
##### AcWing317   陨石的秘密  
##### AcWing318   划分大理石
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 60010;

bool f[N];
int a[10];

int main()
{
    while (true)
    {
        int m = 0;
        for (int i = 1; i <= 6; i ++ )
        {
            cin >> a[i];
            m += a[i] * i;
        }
        if (!m) break;
        if (m & 1) cout << "Can't" << endl;
        else
        {
            m /= 2;
            memset(f, 0, sizeof f);
            f[0] = true;
            for (int i = 1; i <= 6; i ++ )
            {
                int s = a[i], k = 1;
                while (s >= k)
                {
                    // v: i * k
                    for (int j = m; j >= i * k; j -- ) f[j] |= f[j - i * k];
                    s -= k;
                    k *= 2;
                }
                if (s > 0)
                    for (int j = m; j >= i * s; j -- ) f[j] |= f[j - i * s];
            }
            if (f[m]) puts("Can");
            else puts("Can't");
        }
    }

    return 0;
}
```
##### AcWing319   折叠序列  
##### AcWing320   能量项链  
##### AcWing321   棋盘分割  
##### AcWing322   消木块  
##### AcWing323   战略游戏
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1510;

int n;
int h[N], e[N], ne[N], idx;
int f[N][2];
bool st[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    for (int i = h[u]; ~i; i = ne[i]) dfs(e[i]);

    f[u][0] = 0;
    f[u][1] = 1;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        f[u][1] += min(f[j][0], f[j][1]);
        f[u][0] += f[j][1];
    }
}

int main()
{
    while (cin >> n)
    {
        memset(h, -1, sizeof h);
        idx = 0;

        memset(st, false, sizeof st);
        for (int i = 0; i < n; i ++ )
        {
            int a, t;
            scanf("%d:(%d)", &a, &t);

            while (t -- )
            {
                int b;
                cin >> b;
                add(a, b);
                st[b] = true;
            }
        }

        int root;
        while (st[root]) root ++ ;

        dfs(root);

        cout << min(f[root][0], f[root][1]) << endl;
    }

    return 0;
}
```
  
##### AcWing324   贿赂FIPA  
##### AcWing325   计算机  
##### AcWing326   XOR和路径  
##### AcWing1194   岛和桥  
##### AcWing327   玉米田
```cpp
#include <iostream>

using namespace std;

typedef long long LL;

const int N = 12, M = 1 << N;
const int mod = 1000000000;

int n, m;
int f[N + 1][M], g[N][N], gs[N];
bool st[M];

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            cin >> g[i][j];
    for (int i = 0; i < m; i ++ )
        for (int j = 0; j < n; j ++ )
            if (!g[j][i])
                gs[i] += 1 << j;

    for (int i = 0; i < 1 << n; i ++ )
    {
        st[i] = true;
        for (int j = 1; j < n; j ++ )
            if ((i >> j & 1) && (i >> j - 1 & 1))
            {
                st[i] = false;
                break;
            }
    }

    f[0][0] = 1;
    for (int i = 0; i < m; i ++ )
        for (int j = 0; j < 1 << n; j ++ )
            if (st[j])
                for (int k = 0; k < 1 << n; k ++ )
                    if (st[k] && (j & k) == 0 && (k & gs[i]) == 0)
                    {
                        f[i + 1][k] += f[i][j];

                        if (f[i + 1][k] >= mod) f[i + 1][k] -= mod;
                    }

    int res = 0;
    for (int i = 0; i < 1 << n; i ++ ) res = (res + f[m][i]) % mod;

    cout << res << endl;

    return 0;
}
```
  
##### AcWing328   芯片  
##### AcWing329   围栏障碍训练场  
##### AcWing330   估算  
##### AcWing331   干草堆  
##### AcWing332   股票交易  
##### AcWing333   最大子矩阵  
##### AcWing334   K匿名序列  
##### AcWing335   特别行动队  
##### AcWing336   邮局  
##### AcWing337   扑克牌  
##### AcWing338   计数问题
```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 10;

/*

100~abc-1, 999

abc
    1. num[i] < x, 0
    2. num[i] == x, 0~efg
    3. num[i] > x, 0~999

*/

int get(vector<int> num, int l, int r)
{
    int res = 0;
    for (int i = l; i >= r; i -- ) res = res * 10 + num[i];
    return res;
}

int power10(int x)
{
    int res = 1;
    while (x -- ) res *= 10;
    return res;
}

int count(int n, int x)
{
    if (!n) return 0;

    vector<int> num;
    while (n)
    {
        num.push_back(n % 10);
        n /= 10;
    }
    n = num.size();

    int res = 0;
    for (int i = n - 1 - !x; i >= 0; i -- )
    {
        if (i < n - 1)
        {
            res += get(num, n - 1, i + 1) * power10(i);
            if (!x) res -= power10(i);
        }

        if (num[i] == x) res += get(num, i - 1, 0) + 1;
        else if (num[i] > x) res += power10(i);
    }

    return res;
}

int force_count(int n, int x)
{
    int res = 0;
    for (int i = 1; i <= n; i ++ )
    {
        for (int j = i; j; j /= 10)
            if (j % 10 == x)
                res ++ ;
    }

    return res;
}

int main()
{
    int a, b;
    while (cin >> a >> b , a)
    {
        if (a > b) swap(a, b);

        for (int i = 0; i <= 9; i ++ )
            cout << count(b, i) - count(a - 1, i) << ' ';
        cout << endl;
    }

    return 0;
}

```
##### AcWing339   圆形数字  

# 0x60 图论完成情况(73)

包括最短路、最小生成树、树的直径与最近公共祖先、基环树、负环与差分约束、Tarjan算法与无向图连通性、Tarjan算法与有向图连通性、二分图的匹配、二分图的覆盖与独立集、网络流初步

## 0x61 最短路

     
  
##### AcWing340   通信线路  
##### AcWing341   最优贸易  
##### AcWing342   道路与航线  
##### AcWing343   排序  
##### AcWing344   观光之旅  
##### AcWing345   牛站  

## 0x62 最小生成树

  
##### AcWing346   走廊泼水节
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 6010;

int n;
struct Edge
{
    int a, b, w;
    bool operator< (const Edge &t) const
    {
        return w < t.w;
    }
}e[N];
int p[N], size[N];

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
        for (int i = 0; i < n - 1; i ++ ) cin >> e[i].a >> e[i].b >> e[i].w;
        sort(e, e + n - 1);

        for (int i = 1; i <= n; i ++ ) p[i] = i, size[i] = 1;

        int res = 0;
        for (int i = 0; i < n - 1; i ++ )
        {
            int a = find(e[i].a), b = find(e[i].b), w = e[i].w;
            if (a != b)
            {
                res += (size[a] * size[b] - 1) * (w + 1);
                size[b] += size[a];
                p[a] = b;
            }
        }

        cout << res << endl;
    }

    return 0;
}

```

  
##### AcWing347   野餐规划  
##### AcWing348   沙漠之王  
##### AcWing349   黑暗城堡  

## 0x63 树的直径与最近公共祖先

     
  
##### AcWing350   巡逻  
##### AcWing351   树网的核  
##### AcWing352   闇の連鎖  
##### AcWing353   雨天的尾巴  
##### AcWing354   天天爱跑步  
##### AcWing355   异象石  
##### AcWing356   次小生成树  
##### AcWing357   疫情控制  

## 0x64 基环树

     
  
##### AcWing358   岛屿  
##### AcWing359   创世纪  
##### AcWing360   Freda的传呼机  

## 0x65  负环与差分约束

##### AcWing361   观光奶牛
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 1010, M = 5010;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int f[N], cnt[N];
double dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool spfa(double mid)
{
    for (int i = 1; i <= n; i ++ )
    {
        dist[i] = 0;
        cnt[i] = 0;
        st[i] = false;
    }
    queue<int> q;

    for (int i = 1; i <= n; i ++ )
    {
        q.push(i);
        st[i] = true;
    }

    while (q.size())
    {
        int t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i] * mid - f[j])
            {
                dist[j] = dist[t] + w[i] * mid - f[j];
                cnt[j] = cnt[t] + 1;

                if (cnt[j] >= n) return true;

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

int main()
{
    cin >> n >> m;

    for (int i = 1; i <= n; i ++ ) cin >> f[i];
    memset(h, -1, sizeof h);

    while (m -- )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }

    double l = 0, r = 1e9;
    while (r - l > 1e-4)
    {
        double mid = (l + r) / 2;
        if (spfa(mid)) l = mid;
        else r = mid;
    }

    printf("%.2lf\n", r);

    return 0;
}
```
##### AcWing362   区间
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 50010, M = N * 3;

int n;
int h[N], w[M], e[M], ne[M], idx;
int dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void spfa()
{
    queue<int> q;

    for (int i = 0; i <= 50001; i ++ )
    {
        q.push(i);
        st[i] = true;
    }

    while (q.size())
    {
        int t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] < dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
}

int main()
{
    scanf("%d", &n);

    memset(h, -1, sizeof h);
    while (n -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        a ++, b ++;
        add(a - 1, b, c);
    }

    for (int i = 1; i <= 50001; i ++ )
    {
        add(i - 1, i, 0);
        add(i, i - 1, -1);
    }

    spfa();

    printf("%d\n", dist[50001]);

    return 0;
}
```

## 0x66 Tarjan算法与无向图连通性

     
  
##### AcWing363   B城  
##### AcWing364   网络  
##### AcWing365   圆桌骑士  
##### AcWing366   看牛       
     
## 0x67 Tarjan算法与有向图连通性

##### AcWing367   学校网络
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = 10010;

int n, m;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], cnt;
int stk[N], tt, id[N], num;
int din[N], dout[N];
bool in_stk[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void tarjan(int u)
{
    dfn[u] = low[u] = ++ cnt;
    stk[ ++ tt] = u, in_stk[u] = true;
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
        int y;
        ++ num;
        do
        {
            y = stk[tt -- ];
            in_stk[y] = false;
            id[y] = num;
        } while (y != u);
    }
}

int main()
{
    cin >> n;
    memset(h, -1, sizeof h);
    for (int i = 1; i <= n; i ++ )
    {
        int t;
        while (cin >> t, t) add(i, t);
    }

    for (int i = 1; i <= n; i ++ )
        if (!dfn[i])
            tarjan(i);

    for (int i = 1; i <= n; i ++ )
        for (int j = h[i]; ~j; j = ne[j])
        {
            int k = e[j];
            if (id[i] != id[k])
                dout[id[i]] ++ , din[id[k]] ++ ;
        }

    int a = 0, b = 0;
    for (int i = 1; i <= num; i ++ )
    {
        if (!din[i]) a ++ ;
        if (!dout[i]) b ++ ;
    }

    cout << a << endl;
    if (num == 1) cout << 0 << endl;
    else cout << max(a, b) << endl;

    return 0;
}

```

##### AcWing368   银河
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010, M = 600010;

int n, m;
int h[N], hs[N], e[M], w[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
bool in_stk[N];
int id[N], size[N], cnt;
int dist[N];

void add(int h[], int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

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
        else if (in_stk[j]) low[u] = min(low[u], dfn[j]);
    }

    if (dfn[u] == low[u])
    {
        int y;
        ++ cnt;
        do {
            y = stk[top -- ];
            in_stk[y] = false;
            id[y] = cnt;
            size[cnt] ++ ;
        } while (y != u);
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 1; i <= n; i ++ ) add(h, 0, i, 1);
    while (m -- )
    {
        int t, a, b;
        scanf("%d%d%d", &t, &a, &b);
        if (t == 1) add(h, b, a, 0), add(h, a, b, 0);
        else if (t == 2) add(h, a, b, 1);
        else if (t == 3) add(h, b, a, 0);
        else if (t == 4) add(h, b, a, 1);
        else add(h, a, b, 0);
    }

    tarjan(0);

    bool success = true;
    memset(hs, -1, sizeof hs);
    for (int i = 0; i <= n; i ++ )
    {
        for (int j = h[i]; ~j; j = ne[j])
        {
            int k = e[j];
            if (id[i] == id[k])
            {
                if (w[j] > 0)
                {
                    success = false;
                    break;
                }
            }
            else
            {
                add(hs, id[i], id[k], w[j]);
            }
        }
        if (!success) break;
    }

    if (!success) puts("-1");
    else
    {
        for (int i = cnt; i; i -- )
            for (int j = hs[i]; ~j; j = ne[j])
            {
                int k = e[j];
                dist[k] = max(dist[k], dist[i] + w[j]);
            }

        LL res = 0;
        for (int i = 1; i <= cnt; i ++ ) res += (LL)dist[i] * size[i];

        printf("%lld\n", res);
    }

    return 0;
}

```
##### AcWing369   北大ACM队的远足  
##### AcWing370   卡图难题  
##### AcWing371   牧师约翰最忙碌的一天  

## 0x68 二分图的匹配

##### AcWing372   棋盘覆盖
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

const int N = 110;

int n, m;
int g[N][N];
PII ret[N][N];
bool st[N][N];

int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

bool find(PII t)
{
    for (int i = 0; i < 4; i ++ )
    {
        int a = t.first + dx[i], b = t.second + dy[i];

        if (!a || a > n || !b || b > n || g[a][b] || st[a][b]) continue;

        st[a][b] = true;

        if (ret[a][b].first == 0 || find(ret[a][b]))
        {
            ret[a][b] = t;
            return true;
        }
    }

    return false;
}

int main()
{
    cin >> n >> m;

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        cin >> a >> b;
        g[a][b] = 1;
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
        {
            if ((i + j) % 2 == 0 || g[i][j]) continue;
            memset(st, false, sizeof st);

            if (find({i, j})) res ++ ;
        }

    cout << res << endl;

    return 0;
}
```
##### AcWing373   車的放置
```cpp


```  
##### AcWing374   导弹防御塔
```cpp


```  
##### AcWing375   蚂蚁           

## 0x69 二分图的覆盖与独立集

     
  
##### AcWing376   机器任务
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m, k;
int match[N];
bool g[N][N], st[N];

bool find(int x)
{
    for (int i = 0; i < m; i ++ )
        if (!st[i] && g[x][i])
        {
            st[i] = true;
            if (match[i] == -1 || find(match[i]))
            {
                match[i] = x;
                return true;
            }
        }

    return false;
}

int main()
{
    while (cin >> n, n)
    {
        cin >> m >> k;
        memset(g, 0, sizeof g);
        memset(match, -1, sizeof match);

        while (k -- )
        {
            int t, a, b;
            cin >> t >> a >> b;
            if (!a || !b) continue;
            g[a][b] = true;
        }

        int res = 0;
        for (int i = 0; i < n; i ++ )
        {
            memset(st, 0, sizeof st);
            if (find(i)) res ++ ;
        }

        cout << res << endl;
    }

    return 0;
}
```

  
##### AcWing377   泥泞的区域  
##### AcWing378   骑士放置
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 110;

int n, m, k;
PII match[N][N];
bool g[N][N], st[N][N];

int dx[8] = {-2, -1, 1, 2, 2, 1, -1, -2};
int dy[8] = {1, 2, 2, 1, -1, -2, -2, -1};

bool find(int x, int y)
{
    for (int i = 0; i < 8; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        if (a < 1 || a > n || b < 1 || b > m) continue;
        if (g[a][b]) continue;
        if (st[a][b]) continue;

        st[a][b] = true;

        PII t = match[a][b];
        if (t.x == 0 || find(t.x, t.y))
        {
            match[a][b] = {x, y};
            return true;
        }
    }

    return false;
}

int main()
{
    cin >> n >> m >> k;

    for (int i = 0; i < k; i ++ )
    {
        int x, y;
        cin >> x >> y;
        g[x][y] = true;
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            if (g[i][j] || (i + j) % 2) continue;
            memset(st, 0, sizeof st);
            if (find(i, j)) res ++ ;
        }

    cout << n * m - k - res << endl;

    return 0;
}
```    
##### AcWing379   捉迷藏
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 210;

int n, m;
int match[N];
bool d[N][N], st[N];

bool find(int x)
{
    for (int i = 1; i <= n; i ++ )
        if (!st[i] && d[x][i])
        {
            st[i] = true;
            if (match[i] == 0 || find(match[i]))
            {
                match[i] = x;
                return true;
            }
        }
    return false;
}

int main()
{
    scanf("%d%d", &n, &m);
    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        d[a][b] = true;
    }

    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                d[i][j] |= d[i][k] & d[k][j];

    int res = 0;
    for (int i = 1; i <= n; i ++ )
    {
        memset(st, 0, sizeof st);
        if (find(i)) res ++ ;
    }

    cout << n - res << endl;

    return 0;
}
```
       

## 0x6A 网络流初步
  
##### AcWing380   舞动的夜晚
  
##### AcWing381   有线电视网络
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110, M = 5210, INF = 1e8;

int n, m, S, T;
int h[N], e[M], f[M], ne[M], idx;
int q[N], d[N], cur[N];
bool g[N][N];

void add(int a, int b, int c)
{
    e[idx] = b, f[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
    e[idx] = a, f[idx] = 0, ne[idx] = h[b], h[b] = idx ++ ;
}

void build()
{
    memset(h, -1, sizeof h);
    idx = 0;
    for (int i = 1; i <= n; i ++ ) add(i, n + i, 1);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j < i; j ++ )
            if (g[i][j])
            {
                add(n + i, j, INF);
                add(n + j, i, INF);
            }
}

bool bfs()
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
    return false;
}

int find(int u, int limit)
{
    if (u == T) return limit;
    int flow = 0;
    for (int i = cur[u]; ~i && flow < limit; i = ne[i])
    {
        cur[u] = i;
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

int main()
{
    while (cin >> n >> m)
    {
        memset(g, 0, sizeof g);
        for (int i = 0; i < m; i ++ )
        {
            int a, b;
            scanf(" (%d,%d)", &a, &b);
            a ++, b ++ ;
            g[a][b] = g[b][a] = true;
        }

        int res = n;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j < i; j ++ )
                if (!g[i][j])
                {
                    build();
                    S = n + i, T = j;
                    res = min(res, dinic());
                }

        printf("%d\n", res);
    }
    return 0;
}
```
  
##### AcWing382   K取方格数  

## 0x6B 总结与练习

##### AcWing383   观光
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 1010, M = 20010;

struct Ver
{
    int id, type, dist;
    bool operator> (const Ver &W) const
    {
        return dist > W.dist;
    }
};

int n, m, S, T;
int h[N], e[M], w[M], ne[M], idx;
int dist[N][2], cnt[N][2];
bool st[N][2];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int dijkstra()
{
    memset(st, 0, sizeof st);
    memset(dist, 0x3f, sizeof dist);
    memset(cnt, 0, sizeof cnt);

    dist[S][0] = 0, cnt[S][0] = 1;
    priority_queue<Ver, vector<Ver>, greater<Ver>> heap;
    heap.push({S, 0, 0});

    while (heap.size())
    {
        Ver t = heap.top();
        heap.pop();

        int ver = t.id, type = t.type, distance = t.dist, count = cnt[ver][type];
        if (st[ver][type]) continue;
        st[ver][type] = true;

        for (int i = h[ver]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j][0] > distance + w[i])
            {
                dist[j][1] = dist[j][0], cnt[j][1] = cnt[j][0];
                heap.push({j, 1, dist[j][1]});
                dist[j][0] = distance + w[i], cnt[j][0] = count;
                heap.push({j, 0, dist[j][0]});
            }
            else if (dist[j][0] == distance + w[i]) cnt[j][0] += count;
            else if (dist[j][1] > distance + w[i])
            {
                dist[j][1] = distance + w[i], cnt[j][1] = count;
                heap.push({j, 1, dist[j][1]});
            }
            else if (dist[j][1] == distance + w[i]) cnt[j][1] += count;
        }
    }

    int res = cnt[T][0];
    if (dist[T][0] + 1 == dist[T][1]) res += cnt[T][1];

    return res;
}

int main()
{
    int cases;
    scanf("%d", &cases);
    while (cases -- )
    {
        scanf("%d%d", &n, &m);
        memset(h, -1, sizeof h);
        idx = 0;

        while (m -- )
        {
            int a, b, c;
            scanf("%d%d%d", &a, &b, &c);
            add(a, b, c);
        }
        scanf("%d%d", &S, &T);

        printf("%d\n", dijkstra());
    }

    return 0;
}

``` 
##### AcWing384   升降梯上  

##### AcWing385   GF和猫咪的玩具 
 
##### AcWing386   社交网络  

##### AcWing387   北极网络  

##### AcWing388   四叶草魔杖 
 
##### AcWing389   直径  

##### AcWing390   逃学的小孩 
 
##### AcWing391   聚会  

##### AcWing392   会合  

##### AcWing393   雇佣收银员 
 
##### AcWing394   最优高铁环
  
##### AcWing395   冗余路径
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;


const int N = 5010, M = 20010;

int n, m;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
int id[N], scc_cnt;
bool is_bridge[M];
int d[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

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
        ++ scc_cnt;
        int y;
        do {
            y = stk[top -- ];
            id[y] = scc_cnt;
        } while (y != u);
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);

    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b), add(b, a);
    }

    tarjan(1, -1);

    for (int i = 0; i < idx; i += 2)
        if (is_bridge[i])
        {
            d[id[e[i]]] ++ ;
            d[id[e[i ^ 1]]] ++ ;
        }

    int res = 0;
    for (int i = 1; i <= scc_cnt; i ++ )
        if (d[i] == 1)
            res ++ ;

    printf("%d\n", (res + 1) / 2);

    return 0;
}
```

