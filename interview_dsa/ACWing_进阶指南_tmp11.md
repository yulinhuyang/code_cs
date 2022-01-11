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
