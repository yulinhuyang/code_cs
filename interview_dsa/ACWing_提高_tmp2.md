##### AcWing11 背包问题求方案数
```cpp
#include <cstring>
#include <iostream>

using namespace std;

const int N = 1010, mod = 1e9 + 7;

int n, m;
int f[N], g[N];

int main()
{
    cin >> n >> m;

    memset(f, -0x3f, sizeof f);
    f[0] = 0;
    g[0] = 1;

    for (int i = 0; i < n; i ++ )
    {
        int v, w;
        cin >> v >> w;
        for (int j = m; j >= v; j -- )
        {
            int maxv = max(f[j], f[j - v] + w);
            int s = 0;
            if (f[j] == maxv) s = g[j];
            if (f[j - v] + w == maxv) s = (s + g[j - v]) % mod;
            f[j] = maxv, g[j] = s;
        }
    }

    int res = 0;
    for (int i = 1; i <= m; i ++ )
        if (f[i] > f[res])
            res = i;

    int sum = 0;
    for (int i = 0; i <= m; i ++ )
        if (f[i] == f[res])
            sum = (sum + g[i]) % mod;

    cout << sum << endl;

    return 0;
}
```
##### AcWing734 能量石
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = 10010;

int n;
struct Stone
{
    int s, e, l;
}stones[N];

bool cmp(Stone a, Stone b)
{
    return a.s * b.l < b.s * a.l;
}

int f[N][M];

int main()
{
    int T;
    cin >> T;
    for (int C = 1; C <= T; C ++ )
    {
        cin >> n;
        int m = 0;
        for (int i = 1; i <= n; i ++ )
        {
            int s, e, l;
            cin >> s >> e >> l;
            stones[i] = {s, e, l};
            m += s;
        }

        sort(stones + 1, stones + 1 + n, cmp);

        for (int i = 1; i <= n; i ++ )
            for (int j = 0; j <= m; j ++ )
            {
                f[i][j] = f[i - 1][j];
                if (j >= stones[i].s)
                {
                    int s = stones[i].s, e = stones[i].e, l = stones[i].l;
                    f[i][j] = max(f[i][j], f[i - 1][j - s] + max(0, e - l * (j - s)));
                }
            }

        int res = 0;
        for (int i = 0; i <= m; i ++ ) res = max(res, f[n][i]);

        printf("Case #%d: %d\n", C, res);
    }

    return 0;
}
```
##### AcWing1049 大盗阿福
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n;
int w[N], f[N][2];

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d", &n);
        for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

        for (int i = 1; i <= n; i ++ )
        {
            f[i][0] = max(f[i - 1][0], f[i - 1][1]);
            f[i][1] = f[i - 1][0] + w[i];
        }

        printf("%d\n", max(f[n][0], f[n][1]));
    }

    return 0;
}
```
##### AcWing1057 股票买卖IV
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 110, INF = 0x3f3f3f3f;

int n, m;
int w[N];
int f[N][M][2];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    memset(f, -0x3f, sizeof f);
    for (int i = 0; i <= n; i ++ ) f[i][0][0] = 0;

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            f[i][j][0] = max(f[i - 1][j][0], f[i - 1][j][1] + w[i]);
            f[i][j][1] = max(f[i - 1][j][1], f[i - 1][j - 1][0] - w[i]);
        }

    int res = 0;
    for (int i = 0; i <= m; i ++ ) res = max(res, f[n][i][0]);

    printf("%d\n", res);

    return 0;
}
```
##### AcWing1058 股票买卖V
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, INF = 0x3f3f3f3f;

int n;
int w[N];
int f[N][3];

int main()
{
    scanf("%d", &n);

    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    f[0][0] = f[0][1] = -INF, f[0][2] = 0;
    for (int i = 1; i <= n; i ++ )
    {
        f[i][0] = max(f[i - 1][0], f[i - 1][2] - w[i]);
        f[i][1] = f[i - 1][0] + w[i];
        f[i][2] = max(f[i - 1][2], f[i - 1][1]);
    }

    printf("%d\n", max(f[n][1], f[n][2]));

    return 0;
}
```
##### AcWing1052 设计密码
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 55, mod = 1e9 + 7;

int n, m;
char str[N];
int nxt[N];
int f[N][N];

int main()
{
    cin >> n >> str + 1;

    m = strlen(str + 1);

    for (int i = 2, j = 0; i <= m; i ++ )
    {
        while (j && str[i] != str[j + 1]) j = nxt[j];
        if (str[i] == str[j + 1]) j ++ ;
        nxt[i] = j;
    }

    f[0][0] = 1;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            for (char k = 'a'; k <= 'z'; k ++ )
            {
                int u = j;
                while (u && k != str[u + 1]) u = nxt[u];
                if (k == str[u + 1]) u ++ ;
                if (u < m) f[i + 1][u] = (f[i + 1][u] + f[i][j]) % mod;
            }

    int res = 0;
    for (int i = 0; i < m; i ++ ) res = (res + f[n][i]) % mod;

    cout << res << endl;

    return 0;
}
```
##### AcWing1064 小国王
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long LL;

const int N = 12, M = 1 << 10, K = 110;

int n, m;
vector<int> state;
int cnt[M];
vector<int> head[M];
LL f[N][K][M];

bool check(int state)
{
    for (int i = 0; i < n; i ++ )
        if ((state >> i & 1) && (state >> i + 1 & 1))
            return false;
    return true;
}

int count(int state)
{
    int res = 0;
    for (int i = 0; i < n; i ++ ) res += state >> i & 1;
    return res;
}

int main()
{
    cin >> n >> m;

    for (int i = 0; i < 1 << n; i ++ )
        if (check(i))
        {
            state.push_back(i);
            cnt[i] = count(i);
        }

    for (int i = 0; i < state.size(); i ++ )
        for (int j = 0; j < state.size(); j ++ )
        {
            int a = state[i], b = state[j];
            if ((a & b) == 0 && check(a | b))
                head[i].push_back(j);
        }

    f[0][0][0] = 1;
    for (int i = 1; i <= n + 1; i ++ )
        for (int j = 0; j <= m; j ++ )
            for (int a = 0; a < state.size(); a ++ )
                for (int b : head[a])
                {
                    int c = cnt[state[a]];
                    if (j >= c)
                        f[i][j][a] += f[i - 1][j - c][b];
                }

    cout << f[n + 1][m][0] << endl;

    return 0;
}
```
##### AcWing327 玉米田
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 14, M = 1 << 12, mod = 1e8;

int n, m;
int w[N];
vector<int> state;
vector<int> head[M];
int f[N][M];

bool check(int state)
{
    for (int i = 0; i + 1 < m; i ++ )
        if ((state >> i & 1) && (state >> i + 1 & 1))
            return false;
    return true;
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j < m; j ++ )
        {
            int t;
            cin >> t;
            w[i] += !t * (1 << j);
        }

    for (int i = 0; i < 1 << m; i ++ )
        if (check(i))
            state.push_back(i);

    for (int i = 0; i < state.size(); i ++ )
        for (int j = 0; j < state.size(); j ++ )
        {
            int a = state[i], b = state[j];
            if (!(a & b))
                head[i].push_back(j);
        }

    f[0][0] = 1;
    for (int i = 1; i <= n + 1; i ++ )
        for (int j = 0; j < state.size(); j ++ )
            if (!(state[j] & w[i]))
                for (int k : head[j])
                    f[i][j] = (f[i][j] + f[i - 1][k]) % mod;

    cout << f[n + 1][0] << endl;

    return 0;
}
```
##### AcWing1053 修复DNA
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int tr[N][4], dar[N], idx;
int q[N], ne[N];
char str[N];

int f[N][N];

int get(char c)
{
    if (c == 'A') return 0;
    if (c == 'T') return 1;
    if (c == 'G') return 2;
    return 3;
}

void insert()
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int t = get(str[i]);
        if (tr[p][t] == 0) tr[p][t] = ++ idx;
        p = tr[p][t];
    }
    dar[p] = 1;
}

void build()
{
    int hh = 0, tt = -1;
    for (int i = 0; i < 4; i ++ )
        if (tr[0][i])
            q[ ++ tt] = tr[0][i];

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = 0; i < 4; i ++ )
        {
            int p = tr[t][i];
            if (!p) tr[t][i] = tr[ne[t]][i];
            else
            {
                ne[p] = tr[ne[t]][i];
                q[ ++ tt] = p;
                dar[p] |= dar[ne[p]];
            }
        }
    }
}

int main()
{
    int T = 1;
    while (scanf("%d", &n), n)
    {
        memset(tr, 0, sizeof tr);
        memset(dar, 0, sizeof dar);
        memset(ne, 0, sizeof ne);
        idx = 0;

        for (int i = 0; i < n; i ++ )
        {
            scanf("%s", str);
            insert();
        }

        build();

        scanf("%s", str + 1);
        m = strlen(str + 1);

        memset(f, 0x3f, sizeof f);
        f[0][0] = 0;
        for (int i = 0; i < m; i ++ )
            for (int j = 0; j <= idx; j ++ )
                for (int k = 0; k < 4; k ++ )
                {
                    int t = get(str[i + 1]) != k;
                    int p = tr[j][k];
                    if (!dar[p]) f[i + 1][p] = min(f[i + 1][p], f[i][j] + t);
                }

        int res = 0x3f3f3f3f;
        for (int i = 0; i <= idx; i ++ ) res = min(res, f[m][i]);

        if (res == 0x3f3f3f3f) res = -1;
        printf("Case %d: %d\n", T ++, res);
    }

    return 0;
}
```
##### AcWing292 炮兵阵地
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 10, M = 1 << 10;

int n, m;
int g[1010];
int f[2][M][M];
vector<int> state;
int cnt[M];

bool check(int state)
{
    for (int i = 0; i < m; i ++ )
        if ((state >> i & 1) && ((state >> i + 1 & 1) || (state >> i + 2 & 1)))
            return false;
    return true;
}

int count(int state)
{
    int res = 0;
    for (int i = 0; i < m; i ++ )
        if (state >> i & 1)
            res ++ ;
    return res;
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j < m; j ++ )
        {
            char c;
            cin >> c;
            g[i] += (c == 'H') << j;
        }

    for (int i = 0; i < 1 << m; i ++ )
        if (check(i))
        {
            state.push_back(i);
            cnt[i] = count(i);
        }

    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j < state.size(); j ++ )
            for (int k = 0; k < state.size(); k ++ )
                for (int u = 0; u < state.size(); u ++ )
                {
                    int a = state[j], b = state[k], c = state[u];
                    if (a & b | a & c | b & c) continue;
                    if (g[i] & b | g[i - 1] & a) continue;
                    f[i & 1][j][k] = max(f[i & 1][j][k], f[i - 1 & 1][u][j] + cnt[b]);
                }

    int res = 0;
    for (int i = 0; i < state.size(); i ++ )
        for (int j = 0; j < state.size(); j ++ )
            res = max(res, f[n & 1][i][j]);

    cout << res << endl;

    return 0;
}
```
##### AcWing524 愤怒的小鸟
```cpp
状态压缩做法，AC
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

#define x first
#define y second

using namespace std;

typedef pair<double, double> PDD;

const int N = 18, M = 1 << 18;
const double eps = 1e-8;

int n, m;
PDD q[N];
int path[N][N];
int f[M];

int cmp(double x, double y)
{
    if (fabs(x - y) < eps) return 0;
    if (x < y) return -1;
    return 1;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        cin >> n >> m;
        for (int i = 0; i < n; i ++ ) cin >> q[i].x >> q[i].y;

        memset(path, 0, sizeof path);
        for (int i = 0; i < n; i ++ )
        {
            path[i][i] = 1 << i;
            for (int j = 0; j < n; j ++ )
            {
                double x1 = q[i].x, y1 = q[i].y;
                double x2 = q[j].x, y2 = q[j].y;
                if (!cmp(x1, x2)) continue;
                double a = (y1 / x1 - y2 / x2) / (x1 - x2);
                double b = y1 / x1 - a * x1;

                if (cmp(a, 0) >= 0) continue;
                int state = 0;
                for (int k = 0; k < n; k ++ )
                {
                    double x = q[k].x, y = q[k].y;
                    if (!cmp(a * x * x + b * x, y)) state += 1 << k;
                }
                path[i][j] = state;
            }
        }

        memset(f, 0x3f, sizeof f);
        f[0] = 0;
        for (int i = 0; i + 1 < 1 << n; i ++ )
        {
            int x = 0;
            for (int j = 0; j < n; j ++ )
                if (!(i >> j & 1))
                {
                    x = j;
                    break;
                }

            for (int j = 0; j < n; j ++ )
                f[i | path[x][j]] = min(f[i | path[x][j]], f[i] + 1);
        }

        cout << f[(1 << n) - 1] << endl;
    }

    return 0;
}
Dancing Links，TLE，能过(20/21)
#include <iostream>
#include <cstring>
#include <algorithm>
#include <set>
#include <vector>

#define x first
#define y second

using namespace std;

typedef pair<double, double> PDD;
const int N = 10000;
const double eps = 1e-8;

int n, m;
int l[N], r[N], u[N], d[N], col[N], row[N], s[N], idx;
bool st[20];
int path[20][20];
PDD q[20];

int cmp(double x)
{
    if (abs(x) < eps) return 0;
    if (x > 0) return 1;
    return -1;
}

void init()
{
    for (int i = 0; i <= m; i ++ )
    {
        l[i] = i - 1, r[i] = i + 1;
        col[i] = u[i] = d[i] = i;
        s[i] = 0;
    }
    l[0] = m, r[m] = 0;
    idx = m + 1;
}

void add(int& hh, int& tt, int x, int y)
{
    row[idx] = x, col[idx] = y, s[y] ++ ;
    u[idx] = y, d[idx] = d[y], u[d[y]] = idx, d[y] = idx;
    r[hh] = l[tt] = idx, r[idx] = tt, l[idx] = hh;
    tt = idx ++ ;
}

int h()
{
    int res = 0;
    memset(st, 0, sizeof st);
    for (int i = r[0]; i; i = r[i])
    {
        if (st[i]) continue;
        st[i] = true;
        res ++ ;
        for (int j = d[i]; j != i; j = d[j])
            for (int k = r[j]; k != j; k = r[k])
                st[col[k]] = true;
    }
    return res;
}

void remove(int p)
{
    for (int i = d[p]; i != p; i = d[i])
    {
        r[l[i]] = r[i];
        l[r[i]] = l[i];
    }
}

void resume(int p)
{
    for (int i = u[p]; i != p; i = u[i])
    {
        r[l[i]] = i;
        l[r[i]] = i;
    }
}

bool dfs(int k, int depth)
{
    if (k + h() > depth) return false;
    if (!r[0]) return true;
    int p = r[0];
    for (int i = r[0]; i; i = r[i])
        if (s[p] > s[i])
            p = i;

    for (int i = d[p]; i != p; i = d[i])
    {
        remove(i);
        for (int j = r[i]; j != i; j = r[j]) remove(j);
        if (dfs(k + 1, depth)) return true;
        for (int j = l[i]; j != i; j = l[j]) resume(j);
        resume(i);
    }
    return false;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d%*d", &n);
        for (int i = 0; i < n; i ++ ) scanf("%lf%lf", &q[i].x, &q[i].y);
        memset(path, 0, sizeof path);
        for (int i = 0; i < n; i ++ )
        {
            path[i][i] = 1 << i;
            for (int j = i + 1; j < n; j ++ )
            {
                double x1 = q[i].x, y1 = q[i].y;
                double x2 = q[j].x, y2 = q[j].y;
                if (!cmp(x1 - x2)) continue;
                double a = (y1 / x1 - y2 / x2) / (x1 - x2);
                if (cmp(a) >= 0) continue;
                double b = y1 / x1 - a * x1;
                for (int k = 0; k < n; k ++ )
                {
                    double x = q[k].x, y = q[k].y;
                    if (!cmp(a * x * x + b * x - y))
                        path[i][j] += 1 << k;
                }
            }
        }

        m = n;
        init();

        vector<int> S;
        for (int i = 0; i < n; i ++ )
            for (int j = i; j < n; j ++ )
                S.push_back(path[i][j]);

        for (int i = 0; i < S.size(); i ++ )
            for (int j = 0; j < S.size(); j ++ )
                if (i != j && S[i] != -1 && S[j] != -1 && (S[i] & S[j]) == S[i])
                {
                    S[i] = -1;
                    break;
                }

        int k = 0;
        for (auto x: S)
            if (x != -1)
                S[k ++ ] = x;
        S.erase(S.begin() + k, S.end());

        sort(S.begin(), S.end());

        for (auto x: S)
        {
            int hh = idx, tt = idx;
            for (int i = 0; i < n; i ++ )
                if (x >> i & 1)
                    add(hh, tt, 0, i + 1);
        }

        int depth = 0;
        while (!dfs(0, depth)) depth ++ ;
        printf("%d\n", depth);
    }

    return 0;
}
```
 
 
##### AcWing529 宝藏
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 12, M = 1 << 12, INF = 0x3f3f3f3f;

int n, m;
int d[N][N];
int f[M][N], g[M];

int main()
{
    scanf("%d%d", &n, &m);

    memset(d, 0x3f, sizeof d);
    for (int i = 0; i < n; i ++ ) d[i][i] = 0;

    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        a --, b --;
        d[a][b] = d[b][a] = min(d[a][b], c);
    }

    for (int i = 1; i < 1 << n; i ++ )
        for (int j = 0; j < n; j ++ )
            if (i >> j & 1)
            {
                for (int k = 0; k < n; k ++ )
                    if (d[j][k] != INF)
                        g[i] |= 1 << k;
            }

    memset(f, 0x3f, sizeof f);
    for (int i = 0; i < n; i ++ ) f[1 << i][0] = 0;

    for (int i = 1; i < 1 << n; i ++ )
        for (int j = (i - 1); j; j = (j - 1) & i)
            if ((g[j] & i) == i)
            {
                int remain = i ^ j;
                int cost = 0;
                for (int k = 0; k < n; k ++ )
                    if (remain >> k & 1)
                    {
                        int t = INF;
                        for (int u = 0; u < n; u ++ )
                            if (j >> u & 1)
                                t = min(t, d[k][u]);
                        cost += t;
                    }

                for (int k = 1; k < n; k ++ ) f[i][k] = min(f[i][k], f[j][k - 1] + cost * k);
            }

    int res = INF;
    for (int i = 0; i < n; i ++ ) res = min(res, f[(1 << n) - 1][i]);

    printf("%d\n", res);
    return 0;
}
```
##### AcWing321 棋盘分割
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

const int N = 15, M = 9;
const double INF = 1e9;

int n, m = 8;
int s[M][M];
double f[M][M][M][M][N];
double X;

int get_sum(int x1, int y1, int x2, int y2)
{
    return s[x2][y2] - s[x2][y1 - 1] - s[x1 - 1][y2] + s[x1 - 1][y1 - 1];
}

double get(int x1, int y1, int x2, int y2)
{
    double sum = get_sum(x1, y1, x2, y2) - X;
    return (double)sum * sum / n;
}

double dp(int x1, int y1, int x2, int y2, int k)
{
    double &v = f[x1][y1][x2][y2][k];
    if (v >= 0) return v;
    if (k == 1) return v = get(x1, y1, x2, y2);

    v = INF;
    for (int i = x1; i < x2; i ++ )
    {
        v = min(v, get(x1, y1, i, y2) + dp(i + 1, y1, x2, y2, k - 1));
        v = min(v, get(i + 1, y1, x2, y2) + dp(x1, y1, i, y2, k - 1));
    }

    for (int i = y1; i < y2; i ++ )
    {
        v = min(v, get(x1, y1, x2, i) + dp(x1, i + 1, x2, y2, k - 1));
        v = min(v, get(x1, i + 1, x2, y2) + dp(x1, y1, x2, i, k - 1));
    }

    return v;
}

int main()
{
    cin >> n;
    for (int i = 1; i <= m; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            cin >> s[i][j];
            s[i][j] += s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];
        }

    X = (double)s[m][m] / n;
    memset(f, -1, sizeof f);
    printf("%.3lf\n", sqrt(dp(1, 1, 8, 8, n)));

    return 0;
}
```
