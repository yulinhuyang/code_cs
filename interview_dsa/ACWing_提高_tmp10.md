##### AcWing368 银河
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010, M = 600010;

int n, m;
int h[N], hs[N], e[M], ne[M], w[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
bool in_stk[N];
int id[N], scc_cnt, sz[N];
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
        ++ scc_cnt;
        int y;
        do {
            y = stk[top -- ];
            in_stk[y] = false;
            id[y] = scc_cnt;
            sz[scc_cnt] ++ ;
        } while (y != u);
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    memset(hs, -1, sizeof hs);

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
    for (int i = 0; i <= n; i ++ )
    {
        for (int j = h[i]; ~j; j = ne[j])
        {
            int k = e[j];
            int a = id[i], b = id[k];
            if (a == b)
            {
                if (w[j] > 0)
                {
                    success = false;
                    break;
                }
            }
            else add(hs, a, b, w[j]);
        }
        if (!success) break;
    }

    if (!success) puts("-1");
    else
    {
        for (int i = scc_cnt; i; i -- )
        {
            for (int j = hs[i]; ~j; j = ne[j])
            {
                int k = e[j];
                dist[k] = max(dist[k], dist[i] + w[j]);
            }
        }

        LL res = 0;
        for (int i = 1; i <= scc_cnt; i ++ ) res += (LL)dist[i] * sz[i];

        printf("%lld\n", res);
    }

    return 0;
}
```
##### AcWing395 冗余路径
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 5010, M = 20010;

int n, m;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
int id[N], dcc_cnt;
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
        ++ dcc_cnt;
        int y;
        do {
            y = stk[top -- ];
            id[y] = dcc_cnt;
        } while (y != u);
    }
}

int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b;
        cin >> a >> b;
        add(a, b), add(b, a);
    }

    tarjan(1, -1);

    for (int i = 0; i < idx; i ++ )
        if (is_bridge[i])
            d[id[e[i]]] ++ ;

    int cnt = 0;
    for (int i = 1; i <= dcc_cnt; i ++ )
        if (d[i] == 1)
            cnt ++ ;

    printf("%d\n", (cnt + 1) / 2);

    return 0;
}
```
##### AcWing1183 电力
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010, M = 30010;

int n, m;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int root, ans;

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void tarjan(int u)
{
    dfn[u] = low[u] = ++ timestamp;

    int cnt = 0;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (!dfn[j])
        {
            tarjan(j);
            low[u] = min(low[u], low[j]);
            if (low[j] >= dfn[u]) cnt ++ ;
        }
        else low[u] = min(low[u], dfn[j]);
    }

    if (u != root) cnt ++ ;

    ans = max(ans, cnt);
}

int main()
{
    while (scanf("%d%d", &n, &m), n || m)
    {
        memset(dfn, 0, sizeof dfn);
        memset(h, -1, sizeof h);
        idx = timestamp = 0;

        while (m -- )
        {
            int a, b;
            scanf("%d%d", &a, &b);
            add(a, b), add(b, a);
        }

        ans = 0;
        int cnt = 0;
        for (root = 0; root < n; root ++ )
            if (!dfn[root])
            {
                cnt ++ ;
                tarjan(root);
            }

        printf("%d\n", ans + cnt - 1);
    }

    return 0;
}
```
##### AcWing396 矿场搭建
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef unsigned long long ULL;

const int N = 1010, M = 1010;

int n, m;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
int dcc_cnt;
vector<int> dcc[N];
bool cut[N];
int root;

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void tarjan(int u)
{
    dfn[u] = low[u] = ++ timestamp;
    stk[ ++ top] = u;

    if (u == root && h[u] == -1)
    {
        dcc_cnt ++ ;
        dcc[dcc_cnt].push_back(u);
        return;
    }

    int cnt = 0;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (!dfn[j])
        {
            tarjan(j);
            low[u] = min(low[u], low[j]);
            if (dfn[u] <= low[j])
            {
                cnt ++ ;
                if (u != root || cnt > 1) cut[u] = true;
                ++ dcc_cnt;
                int y;
                do {
                    y = stk[top -- ];
                    dcc[dcc_cnt].push_back(y);
                } while (y != j);
                dcc[dcc_cnt].push_back(u);
            }
        }
        else low[u] = min(low[u], dfn[j]);
    }
}

int main()
{
    int T = 1;
    while (cin >> m, m)
    {
        for (int i = 1; i <= dcc_cnt; i ++ ) dcc[i].clear();
        idx = n = timestamp = top = dcc_cnt = 0;
        memset(h, -1, sizeof h);
        memset(dfn, 0, sizeof dfn);
        memset(cut, 0, sizeof cut);

        while (m -- )
        {
            int a, b;
            cin >> a >> b;
            n = max(n, a), n = max(n, b);
            add(a, b), add(b, a);
        }

        for (root = 1; root <= n; root ++ )
            if (!dfn[root])
                tarjan(root);

        int res = 0;
        ULL num = 1;
        for (int i = 1; i <= dcc_cnt; i ++ )
        {
            int cnt = 0;
            for (int j = 0; j < dcc[i].size(); j ++ )
                if (cut[dcc[i][j]])
                    cnt ++ ;

            if (cnt == 0)
            {
                if (dcc[i].size() > 1) res += 2, num *= dcc[i].size() * (dcc[i].size() - 1) / 2;
                else res ++ ;
            }
            else if (cnt == 1) res ++, num *= dcc[i].size() - 1;
        }

        printf("Case %d: %d %llu\n", T ++, res, num);
    }

    return 0;
}
```
##### AcWing257 关押罪犯
 ```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 20010, M = 200010;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int color[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool dfs(int u, int c, int mid)
{
    color[u] = c;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (w[i] <= mid) continue;
        if (color[j])
        {
            if (color[j] == c) return false;
        }
        else if (!dfs(j, 3 - c, mid)) return false;
    }

    return true;
}

bool check(int mid)
{
    memset(color, 0, sizeof color);
    for (int i = 1; i <= n; i ++ )
        if (!color[i])
            if (!dfs(i, 1, mid))
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
 
##### AcWing372 棋盘覆盖
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 110;

int n, m;
PII match[N][N];
bool g[N][N], st[N][N];
int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

bool find(int x, int y)
{
    for (int i = 0; i < 4; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        if (a && a <= n && b && b <= n && !g[a][b] && !st[a][b])
        {
            st[a][b] = true;
            PII t = match[a][b];
            if (t.x == -1 || find(t.x, t.y))
            {
                match[a][b] = {x, y};
                return true;
            }
        }
    }

    return false;
}

int main()
{
    cin >> n >> m;

    while (m -- )
    {
        int x, y;
        cin >> x >> y;
        g[x][y] = true;
    }

    memset(match, -1, sizeof match);

    int res = 0;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            if ((i + j) % 2 && !g[i][j])
            {
                memset(st, 0, sizeof st);
                if (find(i, j)) res ++ ;
            }

    cout << res << endl;

    return 0;
}
```
##### AcWing376 机器任务
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
##### AcWing378 骑士放置
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
##### AcWing379 捉迷藏
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 210, M = 30010;

int n, m;
bool d[N][N], st[N];
int match[N];

bool find(int x)
{
    for (int i = 1; i <= n; i ++ )
        if (d[x][i] && !st[i])
        {
            st[i] = true;
            int t = match[i];
            if (t == 0 || find(t))
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

    // 传递闭包
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

    printf("%d\n", n - res);

    return 0;
}
```
##### AcWing1123 铲雪车
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

int main()
{
    double x1, y1, x2, y2;
    cin >> x1 >> y1;

    double sum = 0;
    while (cin >> x1 >> y1 >> x2 >> y2)
    {
        double dx = x1 - x2;
        double dy = y1 - y2;
        sum += sqrt(dx * dx + dy * dy) * 2;
    }

    int minutes = round(sum / 1000 / 20 * 60);
    int hours = minutes / 60;
    minutes %= 60;

    printf("%d:%02d\n", hours, minutes);

    return 0;
}
```
##### AcWing1184 欧拉回路
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 400010;

int type;
int n, m;
int h[N], e[M], ne[M], idx;
bool used[M];
int ans[M], cnt;
int din[N], dout[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    for (int &i = h[u]; ~i;)
    {
        if (used[i])
        {
            i = ne[i];
            continue;
        }

        used[i] = true;
        if (type == 1) used[i ^ 1] = true;

        int t;

        if (type == 1)
        {
            t = i / 2 + 1;
            if (i & 1) t = -t;
        }
        else t = i + 1;

        int j = e[i];
        i = ne[i];
        dfs(j);

        ans[ ++ cnt] = t;
    }
}

int main()
{
    scanf("%d", &type);
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b);
        if (type == 1) add(b, a);
        din[b] ++ , dout[a] ++ ;
    }

    if (type == 1)
    {
        for (int i = 1; i <= n; i ++ )
            if (din[i] + dout[i] & 1)
            {
                puts("NO");
                return 0;
            }
    }
    else
    {
        for (int i = 1; i <= n; i ++ )
            if (din[i] != dout[i])
            {
                puts("NO");
                return 0;
            }
    }

    for (int i = 1; i <= n; i ++ )
        if (h[i] != -1)
        {
            dfs(i);
            break;
        }

    if (cnt < m)
    {
        puts("NO");
        return 0;
    }

    puts("YES");
    for (int i = cnt; i; i -- ) printf("%d ", ans[i]);
    puts("");

    return 0;
}
```
##### AcWing1124 骑马修栅栏
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510;

int n = 500, m;
int g[N][N];
int ans[1100], cnt;
int d[N];

void dfs(int u)
{
    for (int i = 1; i <= n; i ++ )
        if (g[u][i])
        {
            g[u][i] --, g[i][u] -- ;
            dfs(i);
        }
    ans[ ++ cnt] = u;
}

int main()
{
    cin >> m;
    while (m -- )
    {
        int a, b;
        cin >> a >> b;
        g[a][b] ++, g[b][a] ++ ;
        d[a] ++, d[b] ++ ;
    }

    int start = 1;
    while (!d[start]) start ++ ;
    for (int i = 1; i <= n; i ++ )
        if (d[i] % 2)
        {
            start = i;
            break;
        }

    dfs(start);

    for (int i = cnt; i; i -- ) printf("%d\n", ans[i]);

    return 0;
}
```
##### AcWing1185 单词游戏
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 30;

int n;
int din[N], dout[N], p[N];
bool st[N];

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    char str[1010];

    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d", &n);
        memset(din, 0, sizeof din);
        memset(dout, 0, sizeof dout);
        memset(st, 0, sizeof st);
        for (int i = 0; i < 26; i ++ ) p[i] = i;

        for (int i = 0; i < n; i ++ )
        {
            scanf("%s", str);
            int len = strlen(str);
            int a = str[0] - 'a', b = str[len - 1] - 'a';
            st[a] = st[b] = true;
            dout[a] ++, din[b] ++ ;
            p[find(a)] = find(b);
        }

        int start = 0, end = 0;
        bool success = true;
        for (int i = 0; i < 26; i ++ )
            if (din[i] != dout[i])
            {
                if (din[i] == dout[i] + 1) end ++ ;
                else if (din[i] + 1 == dout[i]) start ++ ;
                else
                {
                    success = false;
                    break;
                }
            }

        if (success && !(!start && !end || start == 1 && end == 1)) success = false;

        int rep = -1;
        for (int i = 0; i < 26; i ++ )
            if (st[i])
            {
                if (rep == -1) rep = find(i);
                else if (rep != find(i))
                {
                    success = false;
                    break;
                }
            }

        if (success) puts("Ordering is possible.");
        else puts("The door cannot be opened.");
    }

    return 0;
}
```
##### AcWing1191 家谱树
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = N * N / 2;

int n;
int h[N], e[M], ne[M], idx;
int q[N];
int d[N];

void add (int a, int b)
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
    cin >> n;
    memset(h, -1, sizeof h);

    for (int i = 1; i <= n; i ++ )
    {
        int son;
        while (cin >> son, son)
        {
            add(i, son);
            d[son] ++ ;
        }
    }

    topsort();

    for (int i = 0; i < n; i ++ ) printf("%d ", q[i]);

    return 0;
}
```
