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


## 0x51 线性DP

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
