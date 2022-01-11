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
