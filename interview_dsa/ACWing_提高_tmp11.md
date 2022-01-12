##### AcWing1192 奖金
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010, M = 20010;

int n, m;
int h[N], e[M], ne[M], idx;
int q[N];
int d[N];
int dist[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

bool topsort()
{
    int hh = 0, tt = -1;
    for (int i = 1; i <= n; i ++ )
        if (!d[i])
            q[ ++ tt] = i;

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if ( -- d[j] == 0)
                q[ ++ tt] = j;
        }
    }

    return tt == n - 1;
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(b, a);
        d[a] ++ ;
    }

    if (!topsort()) puts("Poor Xed");
    else
    {
        for (int i = 1; i <= n; i ++ ) dist[i] = 100;
        for (int i = 0; i < n; i ++ )
        {
            int j = q[i];
            for (int k = h[j]; ~k; k = ne[k])
                dist[e[k]] = max(dist[e[k]], dist[j] + 1);
        }

        int res = 0;
        for (int i = 1; i <= n; i ++ ) res += dist[i];

        printf("%d\n", res);
    }

    return 0;
}
```
##### AcWing164 可达性统计
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <bitset>

using namespace std;

const int N = 30010, M = 30010;

int n, m;
int h[N], e[M], ne[M], idx;
int d[N], q[N];
bitset<N> f[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void topsort()
{
    int hh = 0, tt = -1;
    for (int i = 1; i <= n; i ++ )
        if (!d[i])
            q[ ++ tt] = i;

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if ( -- d[j] == 0)
                q[ ++ tt] = j;
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b);
        d[b] ++ ;
    }

    topsort();

    for (int i = n - 1; i >= 0; i -- )
    {
        int j = q[i];
        f[j][j] = 1;
        for (int k = h[j]; ~k; k = ne[k])
            f[j] |= f[e[k]];
    }

    for (int i = 1; i <= n; i ++ ) printf("%d\n", f[i].count());

    return 0;
}
```
##### AcWing456 车站分级
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 2010, M = 1000010;

int n, m;
int h[N], e[M], ne[M], w[M], idx;
int q[N], d[N];
int dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
    d[b] ++ ;
}

void topsort()
{
    int hh = 0, tt = -1;
    for (int i = 1; i <= n + m; i ++ )
        if (!d[i])
            q[ ++ tt] = i;

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if ( -- d[j] == 0)
                q[ ++ tt] = j;
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 1; i <= m; i ++ )
    {
        memset(st, 0, sizeof st);
        int cnt;
        scanf("%d", &cnt);
        int start = n, end = 1;
        while (cnt -- )
        {
            int stop;
            scanf("%d", &stop);
            start = min(start, stop);
            end = max(end, stop);
            st[stop] = true;
        }

        int ver = n + i;
        for (int j = start; j <= end; j ++ )
            if (!st[j]) add(j, ver, 0);
            else add(ver, j, 1);
    }

    topsort();

    for (int i = 1; i <= n; i ++ ) dist[i] = 1;
    for (int i = 0; i < n + m; i ++ )
    {
        int j = q[i];
        for (int k = h[j]; ~k; k = ne[k])
            dist[e[k]] = max(dist[e[k]], dist[j] + w[k]);
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, dist[i]);

    printf("%d\n", res);

    return 0;
}
```
 
第四章 高级数据结构
 
##### AcWing1250 格子游戏
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 40010;

int n, m;
int p[N];

int get(int x, int y)
{
    return x * n + y;
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;

    for (int i = 0; i < n * n; i ++ ) p[i] = i;

    int res = 0;
    for (int i = 1; i <= m; i ++ )
    {
        int x, y;
        char d;
        cin >> x >> y >> d;
        x --, y -- ;
        int a = get(x, y);
        int b;
        if (d == 'D') b = get(x + 1, y);
        else b = get(x, y + 1);

        int pa = find(a), pb = find(b);
        if (pa == pb)
        {
            res = i;
            break;
        }
        p[pa] = pb;
    }

    if (!res) puts("draw");
    else cout << res << endl;

    return 0;
}
```

##### AcWing1252 搭配购买
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010;

int n, m, vol;
int v[N], w[N];
int p[N];
int f[N];

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m >> vol;

    for (int i = 1; i <= n; i ++ ) p[i] = i;
    for (int i = 1; i <= n; i ++ ) cin >> v[i] >> w[i];

    while (m -- )
    {
        int a, b;
        cin >> a >> b;
        int pa = find(a), pb = find(b);
        if (pa != pb)
        {
            v[pb] += v[pa];
            w[pb] += w[pa];
            p[pa] = pb;
        }
    }

    // 01背包
    for (int i = 1; i <= n; i ++ )
        if (p[i] == i)
            for (int j = vol; j >= v[i]; j -- )
                f[j] = max(f[j], f[j - v[i]] + w[i]);

    cout << f[vol] << endl;

    return 0;
}
```
##### AcWing237 程序自动分析
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_map>

using namespace std;

const int N = 2000010;

int n, m;
int p[N];
unordered_map<int, int> S;

struct Query
{
    int x, y, e;
}query[N];

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
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        n = 0;
        S.clear();
        scanf("%d", &m);
        for (int i = 0; i < m; i ++ )
        {
            int x, y, e;
            scanf("%d%d%d", &x, &y, &e);
            query[i] = {get(x), get(y), e};
        }

        for (int i = 1; i <= n; i ++ ) p[i] = i;

        // 合并所有相等约束条件
        for (int i = 0; i < m; i ++ )
            if (query[i].e == 1)
            {
                int pa = find(query[i].x), pb = find(query[i].y);
                p[pa] = pb;
            }

        // 检查所有不等条件
        bool has_conflict = false;
        for (int i = 0; i < m; i ++ )
            if (query[i].e == 0)
            {
                int pa = find(query[i].x), pb = find(query[i].y);
                if (pa == pb)
                {
                    has_conflict = true;
                    break;
                }
            }

        if (has_conflict) puts("NO");
        else puts("YES");
    }

    return 0;
}
```
##### AcWing238 银河英雄传说
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 30010;

int m;
int p[N], size[N], d[N];

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
    scanf("%d", &m);

    for (int i = 1; i < N; i ++ )
    {
        p[i] = i;
        size[i] = 1;
    }

    while (m -- )
    {
        char op[2];
        int a, b;
        scanf("%s%d%d", op, &a, &b);
        if (op[0] == 'M')
        {
            int pa = find(a), pb = find(b);
            d[pa] = size[pb];
            size[pb] += size[pa];
            p[pa] = pb;
        }
        else
        {
            int pa = find(a), pb = find(b);
            if (pa != pb) puts("-1");
            else printf("%d\n", max(0, abs(d[a] - d[b]) - 1));
        }
    }

    return 0;
}
```
##### AcWing239 奇偶游戏
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
##### AcWing241 楼兰图腾
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 200010;

int n;
int a[N];
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

    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

    for (int i = 1; i <= n; i ++ )
    {
        int y = a[i];
        Greater[i] = sum(n) - sum(y);
        lower[i] = sum(y - 1);
        add(y, 1);
    }

    memset(tr, 0, sizeof tr);
    LL res1 = 0, res2 = 0;
    for (int i = n; i; i -- )
    {
        int y = a[i];
        res1 += Greater[i] * (LL)(sum(n) - sum(y));
        res2 += lower[i] * (LL)(sum(y - 1));
        add(y, 1);
    }

    printf("%lld %lld\n", res1, res2);

    return 0;
}
```
##### AcWing242 一个简单的整数问题
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
        int l, r, d;
        scanf("%s%d", op, &l);
        if (*op == 'C')
        {
            scanf("%d%d", &r, &d);
            add(l, d), add(r + 1, -d);
        }
        else
        {
            printf("%lld\n", sum(l));
        }
    }

    return 0;
}
```
##### AcWing243 一个简单的整数问题2
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
LL tr1[N];  // 维护b[i]的前缀和
LL tr2[N];  // 维护b[i] * i的前缀和

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
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);
    for (int i = 1; i <= n; i ++ )
    {
        int b = a[i] - a[i - 1];
        add(tr1, i, b);
        add(tr2, i, (LL)b * i);
    }

    while (m -- )
    {
        char op[2];
        int l, r, d;
        scanf("%s%d%d", op, &l, &r);
        if (*op == 'Q')
        {
            printf("%lld\n", prefix_sum(r) - prefix_sum(l - 1));
        }
        else
        {
            scanf("%d", &d);
            // a[l] += d
            add(tr1, l, d), add(tr2, l, l * d);
            // a[r + 1] -= d
            add(tr1, r + 1, -d), add(tr2, r + 1, (r + 1) * -d);
        }
    }

    return 0;
}
```
 
##### AcWing244 谜一样的牛
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n;
int h[N];
int ans[N];
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
        int k = h[i] + 1;
        int l = 1, r = n;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (sum(mid) >= k) r = mid;
            else l = mid + 1;
        }
        ans[i] = r;
        add(r, -1);
    }

    for (int i = 1; i <= n; i ++ ) printf("%d\n", ans[i]);

    return 0;
}
```
##### AcWing1275 最大数
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 200010;

int m, p;
struct Node
{
    int l, r;
    int v;  // 区间[l, r]中的最大值
}tr[N * 4];

void pushup(int u)  // 由子节点的信息，来计算父节点的信息
{
    tr[u].v = max(tr[u << 1].v, tr[u << 1 | 1].v);
}

void build(int u, int l, int r)
{
    tr[u] = {l, r};
    if (l == r) return;
    int mid = l + r >> 1;
    build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
}

int query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].v;   // 树中节点，已经被完全包含在[l, r]中了

    int mid = tr[u].l + tr[u].r >> 1;
    int v = 0;
    if (l <= mid) v = query(u << 1, l, r);
    if (r > mid) v = max(v, query(u << 1 | 1, l, r));

    return v;
}

void modify(int u, int x, int v)
{
    if (tr[u].l == x && tr[u].r == x) tr[u].v = v;
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (x <= mid) modify(u << 1, x, v);
        else modify(u << 1 | 1, x, v);
        pushup(u);
    }
}


int main()
{
    int n = 0, last = 0;
    scanf("%d%d", &m, &p);
    build(1, 1, m);

    int x;
    char op[2];
    while (m -- )
    {
        scanf("%s%d", op, &x);
        if (*op == 'Q')
        {
            last = query(1, n - x + 1, n);
            printf("%d\n", last);
        }
        else
        {
            modify(1, n + 1, ((LL)last + x) % p);
            n ++ ;
        }
    }

    return 0;
}
```
