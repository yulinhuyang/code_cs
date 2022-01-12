 
##### AcWing1089 烽火传递
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 2e5 + 10, INF = 1e9;

int n, m;
int w[N], q[N];
int f[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    int hh = 0, tt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        if (q[hh] < i - m) hh ++ ;
        f[i] = f[q[hh]] + w[i];
        while (hh <= tt && f[q[tt]] >= f[i]) tt -- ;
        q[ ++ tt] = i;
    }

    int res = INF;
    for (int i = n - m + 1; i <= n; i ++ ) res = min(res, f[i]);

    printf("%d\n", res);

    return 0;
}

```
##### AcWing1090 绿色通道
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 50010, INF = 1e9;

int n, m;
int w[N];
int f[N], q[N];

bool check(int k)
{
    f[0] = 0;
    int hh = 0, tt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        if (hh <= tt && q[hh] < i - k - 1) hh ++ ;
        f[i] = f[q[hh]] + w[i];
        while (hh <= tt && f[q[tt]] >= f[i]) tt -- ;
        q[ ++ tt] = i;
    }

    int res = INF;
    for (int i = n - k; i <= n; i ++ ) res = min(res, f[i]);

    return res <= m;
}

int main()
{
    scanf("%d%d", &n, &m);

    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    int l = 0, r = n;
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
##### AcWing1087 修剪草坪
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1e5 + 10;

int n, m;
LL s[N];
LL f[N];
int q[N];

LL g(int i)
{
    if (!i) return 0;
    return f[i - 1] - s[i];
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%lld", &s[i]);
        s[i] += s[i - 1];
    }

    int hh = 0, tt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        if (q[hh] < i - m) hh ++ ;
        f[i] = max(f[i - 1], g(q[hh]) + s[i]);
        while (hh <= tt && g(q[tt]) <= g(i)) tt -- ;
        q[ ++ tt] = i;
    }

    printf("%lld\n", f[n]);

    return 0;
}
```
##### AcWing1091 理想的正方形
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, INF = 1e9;

int n, m, k;
int w[N][N];
int row_min[N][N], row_max[N][N];
int q[N];

void get_min(int a[], int b[], int tot)
{
    int hh = 0, tt = -1;
    for (int i = 1; i <= tot; i ++ )
    {
        if (hh <= tt && q[hh] <= i - k) hh ++ ;
        while (hh <= tt && a[q[tt]] >= a[i]) tt -- ;
        q[ ++ tt] = i;
        b[i] = a[q[hh]];
    }
}

void get_max(int a[], int b[], int tot)
{
    int hh = 0, tt = -1;
    for (int i = 1; i <= tot; i ++ )
    {
        if (hh <= tt && q[hh] <= i - k) hh ++ ;
        while (hh <= tt && a[q[tt]] <= a[i]) tt -- ;
        q[ ++ tt] = i;
        b[i] = a[q[hh]];
    }
}



int main()
{
    scanf("%d%d%d", &n, &m, &k);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &w[i][j]);

    for (int i = 1; i <= n; i ++ )
    {
        get_min(w[i], row_min[i], m);
        get_max(w[i], row_max[i], m);
    }

    int res = INF;
    int a[N], b[N], c[N];
    for (int i = k; i <= m; i ++ )
    {
        for (int j = 1; j <= n; j ++ ) a[j] = row_min[j][i];
        get_min(a, b, n);

        for (int j = 1; j <= n; j ++ ) a[j] = row_max[j][i];
        get_max(a, c, n);

        for (int j = k; j <= n; j ++ ) res = min(res, c[j] - b[j]);
    }

    printf("%d\n", res);

    return 0;
}
```
##### AcWing303 运输小猫
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
##### AcWing300 任务安排1
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 5010;

int n, s;
int sc[N], st[N];
LL f[N];

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
            f[i] = min(f[i], f[j] + (sc[i] - sc[j]) * (LL)st[i] + (LL)s * (sc[n] - sc[j]));

    printf("%lld\n", f[n]);

    return 0;
}
```
##### AcWing301 任务安排2
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 300010;

int n, s;
LL c[N], t[N];
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
        while (hh < tt && (f[q[hh + 1]] - f[q[hh]]) <= (t[i] + s) * (c[q[hh + 1]] - c[q[hh]])) hh ++ ;
        int j = q[hh];
        f[i] = f[j] - (t[i] + s) * c[j] + t[i] * c[i] + s * c[n];
        while (hh < tt && (__int128)(f[q[tt]] - f[q[tt - 1]]) * (c[i] - c[q[tt - 1]]) >= (__int128)(f[i] - f[q[tt - 1]]) * (c[q[tt]] - c[q[tt - 1]])) tt -- ;
        q[ ++ tt] = i;
    }

    printf("%lld\n", f[n]);

    return 0;
}
```
##### AcWing302 任务安排3
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
            if (f[q[mid + 1]] - f[q[mid]] > (t[i] + s) * (c[q[mid + 1]] - c[q[mid]])) r = mid;
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
 
第二章 搜索
 
##### AcWing1097 池塘计数
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n, m;
char g[N][N];
PII q[M];
bool st[N][N];

void bfs(int sx, int sy)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[sx][sy] = true;

    while (hh <= tt)
    {
        PII t = q[hh ++ ];

        for (int i = t.x - 1; i <= t.x + 1; i ++ )
            for (int j = t.y - 1; j <= t.y + 1; j ++ )
            {
                if (i == t.x && j == t.y) continue;
                if (i < 0 || i >= n || j < 0 || j >= m) continue;
                if (g[i][j] == '.' || st[i][j]) continue;

                q[ ++ tt] = {i, j};
                st[i][j] = true;
            }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);

    int cnt = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == 'W' && !st[i][j])
            {
                bfs(i, j);
                cnt ++ ;
            }

    printf("%d\n", cnt);

    return 0;
}
```
##### AcWing1098 城堡问题
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 55, M = N * N;

int n, m;
int g[N][N];
PII q[M];
bool st[N][N];

int bfs(int sx, int sy)
{
    int dx[4] = {0, -1, 0, 1}, dy[4] = {-1, 0, 1, 0};

    int hh = 0, tt = 0;
    int area = 0;

    q[0] = {sx, sy};
    st[sx][sy] = true;

    while (hh <= tt)
    {
        PII t = q[hh ++ ];
        area ++ ;

        for (int i = 0; i < 4; i ++ )
        {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m) continue;
            if (st[a][b]) continue;
            if (g[t.x][t.y] >> i & 1) continue;

            q[ ++ tt] = {a, b};
            st[a][b] = true;
        }
    }

    return area;
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            cin >> g[i][j];

    int cnt = 0, area = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (!st[i][j])
            {
                area = max(area, bfs(i, j));
                cnt ++ ;
            }

    cout << cnt << endl;
    cout << area << endl;

    return 0;
}
```
##### AcWing1106 山峰和山谷
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n;
int h[N][N];
PII q[M];
bool st[N][N];

void bfs(int sx, int sy, bool& has_higher, bool& has_lower)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[sx][sy] = true;

    while (hh <= tt)
    {
        PII t = q[hh ++ ];

        for (int i = t.x - 1; i <= t.x + 1; i ++ )
            for (int j = t.y - 1; j <= t.y + 1; j ++ )
            {
                if (i == t.x && j == t.y) continue;
                if (i < 0 || i >= n || j < 0 || j >= n) continue;
                if (h[i][j] != h[t.x][t.y]) // 山脉的边界
                {
                    if (h[i][j] > h[t.x][t.y]) has_higher  = true;
                    else has_lower = true;
                }
                else if (!st[i][j])
                {
                    q[ ++ tt] = {i, j};
                    st[i][j] = true;
                }
            }
    }
}

int main()
{
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            scanf("%d", &h[i][j]);

    int peak = 0, valley = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            if (!st[i][j])
            {
                bool has_higher = false, has_lower = false;
                bfs(i, j, has_higher, has_lower);
                if (!has_higher) peak ++ ;
                if (!has_lower) valley ++ ;
            }

    printf("%d %d\n", peak, valley);

    return 0;
}
```
##### AcWing1100 抓住那头牛
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int n, k;
int q[N];
int dist[N];

int bfs()
{
    memset(dist, -1, sizeof dist);
    dist[n] = 0;
    q[0] = n;

    int hh = 0, tt = 0;

    while (hh <= tt)
    {
        int t = q[hh ++ ];

        if (t == k) return dist[k];

        if (t + 1 < N && dist[t + 1] == -1)
        {
            dist[t + 1] = dist[t] + 1;
            q[ ++ tt] = t + 1;
        }
        if (t - 1 >= 0 && dist[t - 1] == -1)
        {
            dist[t - 1] = dist[t] + 1;
            q[ ++ tt] = t - 1;
        }
        if (t * 2 < N && dist[t * 2] == -1)
        {
            dist[t * 2] = dist[t] + 1;
            q[ ++ tt] = t * 2;
        }
    }

    return -1;
}

int main()
{
    cin >> n >> k;

    cout << bfs() << endl;

    return 0;
}
```
##### AcWing1076 迷宫问题
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n;
int g[N][N];
PII q[M];
PII pre[N][N];

void bfs(int sx, int sy)
{
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int hh = 0, tt = 0;
    q[0] = {sx, sy};

    memset(pre, -1, sizeof pre);
    pre[sx][sy] = {0, 0};
    while (hh <= tt)
    {
        PII t = q[hh ++ ];

        for (int i = 0; i < 4; i ++ )
        {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= n) continue;
            if (g[a][b]) continue;
            if (pre[a][b].x != -1) continue;

            q[ ++ tt] = {a, b};
            pre[a][b] = t;
        }
    }
}

int main()
{
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            scanf("%d", &g[i][j]);

    bfs(n - 1, n - 1);

    PII end(0, 0);

    while (true)
    {
        printf("%d %d\n", end.x, end.y);
        if (end.x == n - 1 && end.y == n - 1) break;
        end = pre[end.x][end.y];
    }

    return 0;
}
```
##### AcWing188 武士风度的牛
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 155, M = N * N;

int n, m;
char g[N][N];
PII q[M];
int dist[N][N];

int bfs()
{
    int dx[] = {-2, -1, 1, 2, 2, 1, -1, -2};
    int dy[] = {1, 2, 2, 1, -1, -2, -2, -1};

    int sx, sy;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == 'K')
                sx = i, sy = j;

    int hh = 0, tt = 0;
    q[0] = {sx, sy};

    memset(dist, -1, sizeof dist);
    dist[sx][sy] = 0;

    while (hh <= tt)
    {
        auto t = q[hh ++ ];

        for (int i = 0; i < 8; i ++ )
        {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m) continue;
            if (g[a][b] == '*') continue;
            if (dist[a][b] != -1) continue;
            if (g[a][b] == 'H') return dist[t.x][t.y] + 1;

            dist[a][b] = dist[t.x][t.y] + 1;
            q[ ++ tt] = {a, b};
        }
    }

    return -1;
}

int main()
{
    cin >> m >> n;

    for (int i = 0; i < n; i ++ ) cin >> g[i];

    cout << bfs() << endl;

    return 0;
}
```
##### AcWing173 矩阵距离
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n, m;
char g[N][N];
PII q[M];
int dist[N][N];

void bfs()
{
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    memset(dist, -1, sizeof dist);

    int hh = 0, tt = -1;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            if (g[i][j] == '1')
            {
                dist[i][j] = 0;
                q[ ++ tt] = {i, j};
            }

    while (hh <= tt)
    {
        auto t = q[hh ++ ];

        for (int i = 0; i < 4; i ++ )
        {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 1 || a > n || b < 1 || b > m) continue;
            if (dist[a][b] != -1) continue;

            dist[a][b] = dist[t.x][t.y] + 1;
            q[ ++ tt] = {a, b};
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%s", g[i] + 1);

    bfs();

    for (int i = 1; i <= n; i ++ )
    {
        for (int j = 1; j <= m; j ++ ) printf("%d ", dist[i][j]);
        puts("");
    }

    return 0;
}
```
##### AcWing1107 魔板
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <queue>

using namespace std;

char g[2][4];
unordered_map<string, pair<char, string>> pre;
unordered_map<string, int> dist;

void set(string state)
{
    for (int i = 0; i < 4; i ++ ) g[0][i] = state[i];
    for (int i = 7, j = 0; j < 4; i --, j ++ ) g[1][j] = state[i];
}

string get()
{
    string res;
    for (int i = 0; i < 4; i ++ ) res += g[0][i];
    for (int i = 3; i >= 0; i -- ) res += g[1][i];
    return res;
}

string move0(string state)
{
    set(state);
    for (int i = 0; i < 4; i ++ ) swap(g[0][i], g[1][i]);
    return get();
}

string move1(string state)
{
    set(state);
    int v0 = g[0][3], v1 = g[1][3];
    for (int i = 3; i >= 0; i -- )
    {
        g[0][i] = g[0][i - 1];
        g[1][i] = g[1][i - 1];
    }
    g[0][0] = v0, g[1][0] = v1;
    return get();
}

string move2(string state)
{
    set(state);
    int v = g[0][1];
    g[0][1] = g[1][1];
    g[1][1] = g[1][2];
    g[1][2] = g[0][2];
    g[0][2] = v;
    return get();
}

int bfs(string start, string end)
{
    if (start == end) return 0;

    queue<string> q;
    q.push(start);
    dist[start] = 0;

    while (!q.empty())
    {
        auto t = q.front();
        q.pop();

        string m[3];
        m[0] = move0(t);
        m[1] = move1(t);
        m[2] = move2(t);

        for (int i = 0; i < 3; i ++ )
            if (!dist.count(m[i]))
            {
                dist[m[i]] = dist[t] + 1;
                pre[m[i]] = {'A' + i, t};
                q.push(m[i]);
                if (m[i] == end) return dist[end];
            }
    }

    return -1;
}

int main()
{
    int x;
    string start, end;
    for (int i = 0; i < 8; i ++ )
    {
        cin >> x;
        end += char(x + '0');
    }

    for (int i = 1; i <= 8; i ++ ) start += char('0' + i);

    int step = bfs(start, end);

    cout << step << endl;

    string res;
    while (end != start)
    {
        res += pre[end].first;
        end = pre[end].second;
    }

    reverse(res.begin(), res.end());

    if (step > 0) cout << res << endl;

    return 0;
}
```
