# 第一章 动态规划
 
##### AcWing1015 摘花生
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m;
int w[N][N];
int f[N][N];

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d%d", &n, &m);
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                scanf("%d", &w[i][j]);

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                f[i][j] = max(f[i - 1][j], f[i][j - 1]) + w[i][j];

        printf("%d\n", f[n][m]);
    }

    return 0;
}

```
##### AcWing1018 最低通行费
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, INF = 1e9;

int n;
int w[N][N];
int f[N][N];

int main()
{
    scanf("%d", &n);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            scanf("%d", &w[i][j]);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            if (i == 1 && j == 1) f[i][j] = w[i][j];    // 特判左上角
            else
            {
                f[i][j] = INF;
                if (i > 1) f[i][j] = min(f[i][j], f[i - 1][j] + w[i][j]);   // 只有不在第一行的时候，才可以从上面过来
                if (j > 1) f[i][j] = min(f[i][j], f[i][j - 1] + w[i][j]);   // 只有不在第一列的时候，才可以从左边过来
            }

    printf("%d\n", f[n][n]);

    return 0;
}
```
##### AcWing1027 方格取数
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 15;

int n;
int w[N][N];
int f[N * 2][N][N];

int main()
{
    scanf("%d", &n);

    int a, b, c;
    while (cin >> a >> b >> c, a || b || c) w[a][b] = c;

    for (int k = 2; k <= n + n; k ++ )
        for (int i1 = 1; i1 <= n; i1 ++ )
            for (int i2 = 1; i2 <= n; i2 ++ )
            {
                int j1 = k - i1, j2 = k - i2;
                if (j1 >= 1 && j1 <= n && j2 >= 1 && j2 <= n)
                {
                    int t = w[i1][j1];
                    if (i1 != i2) t += w[i2][j2];
                    int &x = f[k][i1][i2];
                    x = max(x, f[k - 1][i1 - 1][i2 - 1] + t);
                    x = max(x, f[k - 1][i1 - 1][i2] + t);
                    x = max(x, f[k - 1][i1][i2 - 1] + t);
                    x = max(x, f[k - 1][i1][i2] + t);
                }
            }

    printf("%d\n", f[n + n][n][n]);
    return 0;
}
```
##### AcWing1017 怪盗基德的滑翔翼
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n;
int h[N];
int f[N];

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d", &n);
        for (int i = 0; i < n; i ++ ) scanf("%d", &h[i]);

        int res = 0;
        for (int i = 0; i < n; i ++ )
        {
            f[i] = 1;
            for (int j = 0; j < i; j ++ )
                if (h[i] < h[j])
                    f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }

        memset(f, 0, sizeof f);
        for (int i = n - 1; i >= 0; i -- )
        {
            f[i] = 1;
            for (int j = n - 1; j > i; j -- )
                if (h[i] < h[j])
                    f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }

        printf("%d\n", res);
    }

    return 0;
}
```
##### AcWing1014 登山
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n;
int h[N];
int f[N], g[N];

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &h[i]);

    for (int i = 0; i < n; i ++ )
    {
        f[i] = 1;
        for (int j = 0; j < i; j ++ )
            if (h[i] > h[j])
                f[i] = max(f[i], f[j] + 1);
    }

    for (int i = n - 1; i >= 0; i -- )
    {
        g[i] = 1;
        for (int j = n - 1; j > i; j -- )
            if (h[i] > h[j])
                g[i] = max(g[i], g[j] + 1);
    }

    int res = 0;
    for (int i = 0; i < n; i ++ ) res = max(res, f[i] + g[i] - 1);

    printf("%d\n", res);

    return 0;
}
```
##### AcWing482 合唱队形
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n;
int h[N];
int f[N], g[N];

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &h[i]);

    for (int i = 0; i < n; i ++ )
    {
        f[i] = 1;
        for (int j = 0; j < i; j ++ )
            if (h[i] > h[j])
                f[i] = max(f[i], f[j] + 1);
    }

    for (int i = n - 1; i >= 0; i -- )
    {
        g[i] = 1;
        for (int j = n - 1; j > i; j -- )
            if (h[i] > h[j])
                g[i] = max(g[i], g[j] + 1);
    }

    int res = 0;
    for (int i = 0; i < n; i ++ ) res = max(res, f[i] + g[i] - 1);

    printf("%d\n", n - res);

    return 0;
}
```
##### AcWing1012 友好城市
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

const int N = 5010;

int n;
PII city[N];
int f[N];

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d%d", &city[i].first, &city[i].second);
    sort(city, city + n);

    int res = 0;
    for (int i = 0; i < n; i ++ )
    {
        f[i] = 1;
        for (int j = 0; j < i; j ++ )
            if (city[i].second > city[j].second)
                f[i] = max(f[i], f[j] + 1);
        res = max(res, f[i]);
    }

    printf("%d\n", res);

    return 0;
}
```
##### AcWing1016 最大上升子序列和
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n;
int w[N];
int f[N];

int main()
{
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ ) scanf("%d", &w[i]);

    int res = 0;
    for (int i = 0; i < n; i ++ )
    {
        f[i] = w[i];
        for (int j = 0; j < i; j ++ )
            if (w[i] > w[j])
                f[i] = max(f[i], f[j] + w[i]);
        res = max(res, f[i]);
    }

    printf("%d\n", res);

    return 0;
}
```
##### AcWing1010 拦截导弹
```cpp
#include <sstream>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n;
int h[N], f[N], q[N];

int main()
{
    string line;
    getline(cin, line);
    stringstream ssin(line);
    while (ssin >> h[n]) n ++ ;

    int res = 0, cnt = 0;
    for (int i = 0; i < n; i ++ )
    {
        f[i] = 1;
        for (int j = 0; j < i; j ++ )
            if (h[i] <= h[j])
                f[i] = max(f[i], f[j] + 1);
        res = max(res, f[i]);

        int k = 0;
        while (k < cnt && q[k] < h[i]) k ++ ;
        if (k == cnt) q[cnt ++ ] = h[i];
        else q[k] = h[i];
    }

    printf("%d\n", res);
    printf("%d\n", cnt);
    return 0;
}

```
##### AcWing187 导弹防御系统
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 55;

int n;
int h[N];
int up[N], down[N];
int ans;

void dfs(int u, int su, int sd)
{
    if (su + sd >= ans) return;
    if (u == n)
    {
        ans = min(ans, su + sd);
        return;
    }

    int k = 0;
    while (k < su && up[k] >= h[u]) k ++ ;
    if (k < su)
    {
        int t = up[k];
        up[k] = h[u];
        dfs(u + 1, su, sd);
        up[k] = t;
    }
    else
    {
        up[k] = h[u];
        dfs(u + 1, su + 1, sd);
    }

    k = 0;
    while (k < sd && down[k] <= h[u]) k ++ ;
    if (k < sd)
    {
        int t = down[k];
        down[k] = h[u];
        dfs(u + 1, su, sd);
        down[k] = t;
    }
    else
    {
        down[k] = h[u];
        dfs(u + 1, su, sd + 1);
    }
}

int main()
{
    while (cin >> n, n)
    {
        for (int i = 0; i < n; i ++ ) cin >> h[i];

        ans = n;
        dfs(0, 0, 0);

        cout << ans << endl;
    }

    return 0;
}
```
##### AcWing272 最长公共上升子序列
```cpp
#include <iostream>

using namespace std;

const int N = 3010;

int n;
int a[N], b[N];
int f[N][N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ ) cin >> a[i];
    for (int i = 1; i <= n; i ++ ) cin >> b[i];

    for (int i = 1; i <= n; i ++ )
    {
        int maxv = 1;
        for (int j = 1; j <= n; j ++ )
        {
            f[i][j] = f[i - 1][j];
            if (a[i] == b[j]) f[i][j] = max(f[i][j], maxv);

            if (b[j] < a[i])
                maxv = max(maxv, f[i - 1][j] + 1);
        }
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, f[n][i]);
    cout << res << endl;

    return 0;
}
```
##### AcWing423 采药
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int f[N];

int main()
{
    cin >> m >> n;
    for (int i = 1; i <= n; i ++ )
    {
        int v, w;
        cin >> v >> w;
        for (int j = m; j >= v; j -- )
            f[j] = max(f[j], f[j - v] + w);
    }

    cout << f[m] << endl;

    return 0;
}

```
##### AcWing1024 装箱问题
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int M = 20010;

int n, m;
int f[M];

int main()
{
    cin >> m >> n;
    for (int i = 0; i < n; i ++ )
    {
        int v;
        cin >> v;
        for (int j = m; j >= v; j -- )
            f[j] = max(f[j], f[j - v] + v);
    }

    cout << m - f[m] << endl;

    return 0;
}

```
##### AcWing1022 宠物小精灵之收服
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, M = 510;

int n, V1, V2;
int f[N][M];

int main()
{
    cin >> V1 >> V2 >> n;
    for (int i = 0; i < n; i ++ )
    {
        int v1, v2;
        cin >> v1 >> v2;
        for (int j = V1; j >= v1; j -- )
            for (int k = V2 - 1; k >= v2; k -- )
                f[j][k] = max(f[j][k], f[j - v1][k - v2] + 1);
    }

    cout << f[V1][V2 - 1] << ' ';
    int k = V2 - 1;
    while (k > 0 && f[V1][k - 1] == f[V1][V2 - 1]) k -- ;
    cout << V2 - k << endl;

    return 0;
}
```
##### AcWing278 数字组合
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010;

int n, m;
int f[N];

int main()
{
    cin >> n >> m;

    f[0] = 1;
    for (int i = 0; i < n; i ++ )
    {
        int v;
        cin >> v;
        for (int j = m; j >= v; j -- )
            f[j] += f[j - v];
    }

    cout << f[m] << endl;

    return 0;
}
```
##### AcWing1023 买书
```cpp
#include <iostream>

using namespace std;

const int N = 1010;

int n;
int v[4] = {10, 20, 50, 100};
int f[N];

int main()
{
    cin >> n;

    f[0] = 1;
    for (int i = 0; i < 4; i ++ )
        for (int j = v[i]; j <= n; j ++ )
            f[j] += f[j - v[i]];

    cout << f[n] << endl;

    return 0;
}
```
##### AcWing1021 货币系统
```cpp
#include <iostream>

using namespace std;

typedef long long LL;

const int M = 3010;

int n, m;
LL f[M];

int main()
{
    cin >> n >> m;

    f[0] = 1;
    for (int i = 0; i < n; i ++ )
    {
        int v;
        cin >> v;
        for (int j = v; j <= m; j ++ )
            f[j] += f[j - v];
    }

    cout << f[m] << endl;

    return 0;
}
```
##### AcWing6 多重背包问题III
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 20010;

int n, m;
int f[N], g[N], q[N];

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
    {
        int v, w, s;
        cin >> v >> w >> s;
        memcpy(g, f, sizeof f);
        for (int j = 0; j < v; j ++ )
        {
            int hh = 0, tt = -1;
            for (int k = j; k <= m; k += v)
            {
                if (hh <= tt && q[hh] < k - s * v) hh ++ ;
                while (hh <= tt && g[q[tt]] - (q[tt] - j) / v * w <= g[k] - (k - j) / v * w) tt -- ;
                q[ ++ tt] = k;
                f[k] = g[q[hh]] + (k - q[hh]) / v * w;
            }
        }
    }

    cout << f[m] << endl;

    return 0;
}
```
##### AcWing1019 庆功会
```cpp
#include <iostream>

using namespace std;

const int N = 6010;

int n, m;
int f[N];

int main()
{
    cin >> n >> m;

    for (int i = 0; i < n; i ++ )
    {
        int v, w, s;
        cin >> v >> w >> s;
        for (int j = m; j >= 0; j -- )
            for (int k = 0; k <= s && k * v <= j; k ++ )
                f[j] = max(f[j], f[j - k * v] + k * w);
    }

    cout << f[m] << endl;

    return 0;
}
```
##### AcWing7 混合背包问题
```cpp
#include <iostream>

using namespace std;

const int N = 1010;

int n, m;
int f[N];

int main()
{
    cin >> n >> m;

    for (int i = 0; i < n; i ++ )
    {
        int v, w, s;
        cin >> v >> w >> s;
        if (!s)
        {
            for (int j = v; j <= m; j ++ )
                f[j] = max(f[j], f[j - v] + w);
        }
        else
        {
            if (s == -1) s = 1;
            for (int k = 1; k <= s; k *= 2)
            {
                for (int j = m; j >= k * v; j -- )
                    f[j] = max(f[j], f[j - k * v] + k * w);
                s -= k;
            }
            if (s)
            {
                for (int j = m; j >= s * v; j -- )
                    f[j] = max(f[j], f[j - s * v] + s * w);
            }
        }
    }

    cout << f[m] << endl;

    return 0;
}
```
 

##### AcWing1020 潜水员
```cpp
#include <cstring>
#include <iostream>

using namespace std;

const int N = 22, M = 80;

int n, m, K;
int f[N][M];

int main()
{
    cin >> n >> m >> K;

    memset(f, 0x3f, sizeof f);
    f[0][0] = 0;

    while (K -- )
    {
        int v1, v2, w;
        cin >> v1 >> v2 >> w;
        for (int i = n; i >= 0; i -- )
            for (int j = m; j >= 0; j -- )
                f[i][j] = min(f[i][j], f[max(0, i - v1)][max(0, j - v2)] + w);
    }


    cout << f[n][m] << endl;

    return 0;
}
```
##### AcWing8 二维费用的背包问题
```cpp
#include <iostream>

using namespace std;

const int N = 110;

int n, V, M;
int f[N][N];

int main()
{
    cin >> n >> V >> M;

    for (int i = 0; i < n; i ++ )
    {
        int v, m, w;
        cin >> v >> m >> w;
        for (int j = V; j >= v; j -- )
            for (int k = M; k >= m; k -- )
                f[j][k] = max(f[j][k], f[j - v][k - m] + w);
    }

    cout << f[V][M] << endl;

    return 0;
}
```
##### AcWing1013 机器分配
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 11, M = 16;

int n, m;
int w[N][M];
int f[N][M];
int way[N];

int main()
{
    cin >> n >> m;

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            cin >> w[i][j];

    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j <= m; j ++ )
            for (int k = 0; k <= j; k ++ )
                f[i][j] = max(f[i][j], f[i - 1][j - k] + w[i][k]);

    cout << f[n][m] << endl;

    int j = m;
    for (int i = n; i; i -- )
        for (int k = 0; k <= j; k ++ )
            if (f[i][j] == f[i - 1][j - k] + w[i][k])
            {
                way[i] = k;
                j -= k;
                break;
            }

    for (int i = 1; i <= n; i ++ ) cout << i << ' ' << way[i] << endl;

    return 0;
}
```
##### AcWing426 开心的金明
```cpp
#include <iostream>

using namespace std;

const int N = 30010;

int n, m;
int f[N];

int main()
{
    cin >> m >> n;

    for (int i = 1; i <= n; i ++ )
    {
        int v, w;
        cin >> v >> w;
        for (int j = m; j >= v; j -- )
            f[j] = max(f[j], f[j - v] + v * w);
    }

    cout << f[m] << endl;

    return 0;
}
```
##### AcWing10 有依赖的背包问题
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m;
int v[N], w[N];
int h[N], e[N], ne[N], idx;
int f[N][N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    for (int i = h[u]; ~i; i = ne[i])   // 循环物品组
    {
        int son = e[i];
        dfs(e[i]);

        // 分组背包
        for (int j = m - v[u]; j >= 0; j -- )  // 循环体积
            for (int k = 0; k <= j; k ++ )  // 循环决策
                f[u][j] = max(f[u][j], f[u][j - k] + f[son][k]);
    }

    // 将物品u加进去
    for (int i = m; i >= v[u]; i -- ) f[u][i] = f[u][i - v[u]] + w[u];
    for (int i = 0; i < v[u]; i ++ ) f[u][i] = 0;
}

int main()
{
    cin >> n >> m;

    memset(h, -1, sizeof h);
    int root;
    for (int i = 1; i <= n; i ++ )
    {
        int p;
        cin >> v[i] >> w[i] >> p;
        if (p == -1) root = i;
        else add(p, i);
    }

    dfs(root);

    cout << f[root][m] << endl;

    return 0;
}
```
##### AcWing487 金明的预算方案
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

#define v first
#define w second

using namespace std;

typedef pair<int, int> PII;

const int N = 60, M = 32010;

int n, m;
PII master[N];
vector<PII> servent[N];
int f[M];

int main()
{
    cin >> m >> n;

    for (int i = 1; i <= n; i ++ )
    {
        int v, p, q;
        cin >> v >> p >> q;
        p *= v;
        if (!q) master[i] = {v, p};
        else servent[q].push_back({v, p});
    }

    for (int i = 1; i <= n; i ++ )
        for (int u = m; u >= 0; u -- )
        {
            for (int j = 0; j < 1 << servent[i].size(); j ++ )
            {
                int v = master[i].v, w = master[i].w;
                for (int k = 0; k < servent[i].size(); k ++ )
                    if (j >> k & 1)
                    {
                        v += servent[i][k].v;
                        w += servent[i][k].w;
                    }
                if (u >= v) f[u] = max(f[u], f[u - v] + w);
            }
    }

    cout << f[m] << endl;

    return 0;
}
```
##### AcWing12 背包问题求具体方案
```cpp
#include <iostream>

using namespace std;

const int N = 1010;

int n, m;
int v[N], w[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ) cin >> v[i] >> w[i];

    for (int i = n; i >= 1; i -- )
        for (int j = 0; j <= m; j ++ )
        {
            f[i][j] = f[i + 1][j];
            if (j >= v[i]) f[i][j] = max(f[i][j], f[i + 1][j - v[i]] + w[i]);
        }

    int j = m;
    for (int i = 1; i <= n; i ++ )
        if (j >= v[i] && f[i][j] == f[i + 1][j - v[i]] + w[i])
        {
            cout << i << ' ';
            j -= v[i];
        }

    return 0;
}
```
##### AcWing275 传纸条
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 55;

int n, m;
int g[N][N];
int f[N * 2][N][N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &g[i][j]);

    for (int k = 2; k <= n + m; k ++ )
        for (int i = max(1, k - m); i <= n && i < k; i ++ )
            for (int j = max(1, k - m); j <= n && j < k; j ++ )
                for (int a = 0; a <= 1; a ++ )
                    for (int b = 0; b <= 1; b ++ )
                    {
                        int t = g[i][k - i];
                        if (i != j || k == 2 || k == n + m) // 除了起点和终点之外，其余每个格子只能走一次
                        {
                            t += g[j][k - j];
                            f[k][i][j] = max(f[k][i][j], f[k - 1][i - a][j - b] + t);
                        }
                    }

    printf("%d\n", f[n + m][n][n]);

    return 0;
}
```
##### AcWing532 货币系统
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 25010;

int n;
int a[N];
bool f[N];

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        cin >> n;
        for (int i = 0; i < n; i ++ ) cin >> a[i];
        sort(a, a + n);

        int m = a[n - 1];
        memset(f, 0, sizeof f);
        f[0] = true;

        int k = 0;
        for (int i = 0; i < n; i ++ )
        {
            if (!f[a[i]]) k ++ ;
            for (int j = a[i]; j <= m; j ++ )
                f[j] |= f[j - a[i]];
        }

        cout << k << endl;
    }

    return 0;
}
```
