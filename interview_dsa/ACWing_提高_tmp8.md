##### AcWing1141 局域网
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = 210;

int n, m;
struct Edge
{
    int a, b, w;
    bool operator< (const Edge &t)const
    {
        return w < t.w;
    }
}e[M];
int p[N];

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    for (int i = 0; i < m; i ++ )
    {
        int a, b, w;
        cin >> a >> b >> w;
        e[i] = {a, b, w};
    }

    sort(e, e + m);

    int res = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = find(e[i].a), b = find(e[i].b), w = e[i].w;
        if (a != b) p[a] = b;
        else res += w;
    }

    cout << res << endl;

    return 0;
}
```
##### AcWing1142 繁忙的都市
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 310, M = 10010;

int n, m;
struct Edge
{
    int a, b, w;
    bool operator< (const Edge &t) const
    {
        return w < t.w;
    }
}e[M];
int p[N];

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ) p[i] = i;
    for (int i = 0; i < m; i ++ )
    {
        int a, b, w;
        cin >> a >> b >> w;
        e[i] = {a, b, w};
    }
    sort(e, e + m);

    int res = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = find(e[i].a), b = find(e[i].b), w = e[i].w;
        if (a != b)
        {
            p[a] = b;
            res = w;
        }
    }

    cout << n - 1 << ' ' << res << endl;

    return 0;
}
```
##### AcWing1143 联络员
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 2010, M = 10010;

int n, m;
struct Edge
{
    int a, b, w;
    bool operator< (const Edge &t) const
    {
        return w < t.w;
    }
}e[M];
int p[N];

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    int res = 0, k = 0;
    for (int i = 0; i < m; i ++ )
    {
        int t, a, b, w;
        cin >> t >> a >> b >> w;
        if (t == 1)
        {
            res += w;
            p[find(a)] = find(b);
        }
        else e[k ++ ] = {a, b, w};
    }

    sort(e, e + k);

    for (int i = 0; i < k; i ++ )
    {
        int a = find(e[i].a), b = find(e[i].b), w = e[i].w;
        if (a != b)
        {
            p[a] = b;
            res += w;
        }
    }

    cout << res << endl;

    return 0;
}
```
##### AcWing1144 连接格点
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, M = N * N, K = 2 * N * N;

int n, m, k;
int ids[N][N];
struct Edge
{
    int a, b, w;
}e[K];
int p[M];

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

void get_edges()
{
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1}, dw[4] = {1, 2, 1, 2};

    for (int z = 0; z < 2; z ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                for (int u = 0; u < 4; u ++ )
                    if (u % 2 == z)
                    {
                        int x = i + dx[u], y = j + dy[u], w = dw[u];
                        if (x && x <= n && y && y <= m)
                        {
                            int a = ids[i][j], b = ids[x][y];
                            if (a < b) e[k ++ ] = {a, b, w};
                        }
                    }
}

int main()
{
    cin >> n >> m;

    for (int i = 1, t = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++, t ++ )
            ids[i][j] = t;

    for (int i = 1; i <= n * m; i ++ ) p[i] = i;

    int x1, y1, x2, y2;
    while (cin >> x1 >> y1 >> x2 >> y2)
    {
        int a = ids[x1][y1], b = ids[x2][y2];
        p[find(a)] = find(b);
    }

    get_edges();

    int res = 0;
    for (int i = 0; i < k; i ++ )
    {
        int a = find(e[i].a), b = find(e[i].b), w = e[i].w;
        if (a != b)
        {
            p[a] = b;
            res += w;
        }
    }

    cout << res << endl;

    return 0;
}
```
##### AcWing1145 北极通讯网络
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 510, M = N * N / 2;

int n, k, m;
struct Edge
{
    int a, b;
    double w;
    bool operator< (const Edge &t) const
    {
        return w < t.w;
    }
}e[M];
PII q[M];
int p[N];

double get_dist(PII a, PII b)
{
    int dx = a.x - b.x;
    int dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> k;
    for (int i = 0; i < n; i ++ ) cin >> q[i].x >> q[i].y;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < i; j ++ )
            e[m ++ ] = {i, j, get_dist(q[i], q[j])};

    sort(e, e + m);
    for (int i = 0; i < n; i ++ ) p[i] = i;

    int cnt = n;
    double res = 0;
    for (int i = 0; i < m; i ++ )
    {
        if (cnt <= k) break;

        int a = find(e[i].a), b = find(e[i].b);
        double w = e[i].w;
        if (a != b)
        {
            p[a] = b;
            cnt -- ;
            res = w;
        }
    }

    printf("%.2lf\n", res);

    return 0;
}
```
##### AcWing1146 新的开始
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 310;

int n;
int w[N][N];
int dist[N];
bool st[N];

int prim()
{
    memset(dist, 0x3f, sizeof dist);
    dist[0] = 0;

    int res = 0;
    for (int i = 0; i < n + 1; i ++ )
    {
        int t = -1;
        for (int j = 0; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
        st[t] = true;
        res += dist[t];

        for (int j = 0; j <= n; j ++ ) dist[j] = min(dist[j], w[t][j]);
    }

    return res;
}

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d", &w[0][i]);
        w[i][0] = w[0][i];
    }

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            scanf("%d", &w[i][j]);

    printf("%d\n", prim());

    return 0;
}
```

##### AcWing346 走廊泼水节
```cpp
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
        for (int i = 0; i < n - 1; i ++ )
        {
            int a, b, w;
            cin >> a >> b >> w;
            e[i] = {a, b, w};
        }

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
 
##### AcWing1148 秘密的牛奶运输
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 510, M = 10010;

int n, m;
struct Edge
{
    int a, b, w;
    bool f;
    bool operator< (const Edge &t) const
    {
        return w < t.w;
    }
}edge[M];
int p[N];
int dist1[N][N], dist2[N][N];
int h[N], e[N * 2], w[N * 2], ne[N * 2], idx;

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

void dfs(int u, int fa, int maxd1, int maxd2, int d1[], int d2[])
{
    d1[u] = maxd1, d2[u] = maxd2;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (j != fa)
        {
            int td1 = maxd1, td2 = maxd2;
            if (w[i] > td1) td2 = td1, td1 = w[i];
            else if (w[i] < td1 && w[i] > td2) td2 = w[i];
            dfs(j, u, td1, td2, d1, d2);
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, w;
        scanf("%d%d%d", &a, &b, &w);
        edge[i] = {a, b, w};
    }

    sort(edge, edge + m);
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    LL sum = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = edge[i].a, b = edge[i].b, w = edge[i].w;
        int pa = find(a), pb = find(b);
        if (pa != pb)
        {
            p[pa] = pb;
            sum += w;
            add(a, b, w), add(b, a, w);
            edge[i].f = true;
        }
    }

    for (int i = 1; i <= n; i ++ ) dfs(i, -1, -1e9, -1e9, dist1[i], dist2[i]);

    LL res = 1e18;
    for (int i = 0; i < m; i ++ )
        if (!edge[i].f)
        {
            int a = edge[i].a, b = edge[i].b, w = edge[i].w;
            LL t;
            if (w > dist1[a][b])
                t = sum + w - dist1[a][b];
            else if (w > dist2[a][b])
                t = sum + w - dist2[a][b];
            res = min(res, t);
        }

    printf("%lld\n", res);

    return 0;
}
```
##### AcWing1165 单词环
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 700, M = 100010;

int n;
int h[N], e[M], w[M], ne[M], idx;
double dist[N];
int q[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool check(double mid)
{
    memset(st, 0, sizeof st);
    memset(cnt, 0, sizeof cnt);

    int hh = 0, tt = 0;
    for (int i = 0; i < 676; i ++ )
    {
        q[tt ++ ] = i;
        st[i] = true;
    }

    int count = 0;
    while (hh != tt)
    {
        int t = q[hh ++ ];
        if (hh == N) hh = 0;
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] < dist[t] + w[i] - mid)
            {
                dist[j] = dist[t] + w[i] - mid;
                cnt[j] = cnt[t] + 1;
                if ( ++ count > 10000) return true; // 经验上的trick
                if (cnt[j] >= N) return true;
                if (!st[j])
                {
                    q[tt ++ ] = j;
                    if (tt == N) tt = 0;
                    st[j] = true;
                }
            }
        }
    }

    return false;
}

int main()
{
    char str[1010];
    while (scanf("%d", &n), n)
    {
        memset(h, -1, sizeof h);
        idx = 0;
        for (int i = 0; i < n; i ++ )
        {
            scanf("%s", str);
            int len = strlen(str);
            if (len >= 2)
            {
                int left = (str[0] - 'a') * 26 + str[1] - 'a';
                int right = (str[len - 2] - 'a') * 26 + str[len - 1] - 'a';
                add(left, right, len);
            }
        }

        if (!check(0)) puts("No solution");
        else
        {
            double l = 0, r = 1000;
            while (r - l > 1e-4)
            {
                double mid = (l + r) / 2;
                if (check(mid)) l = mid;
                else r = mid;
            }

            printf("%lf\n", r);
        }
    }

    return 0;
}
```
##### AcWing904 虫洞
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510, M = 5210;

int n, m1, m2;
int h[N], e[M], w[M], ne[M], idx;
int dist[N];
int q[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool spfa()
{
    memset(dist, 0, sizeof dist);
    memset(cnt, 0, sizeof cnt);
    memset(st, 0, sizeof st);

    int hh = 0, tt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        q[tt ++ ] = i;
        st[i] = true;
    }

    while (hh != tt)
    {
        int t = q[hh ++ ];
        if (hh == N) hh = 0;
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true;
                if (!st[j])
                {
                    q[tt ++ ] = j;
                    if (tt == N) tt = 0;
                    st[j] = true;
                }
            }
        }
    }

    return false;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d%d%d", &n, &m1, &m2);
        memset(h, -1, sizeof h);
        idx = 0;
        for (int i = 0; i < m1; i ++ )
        {
            int a, b, c;
            scanf("%d%d%d", &a, &b, &c);
            add(a, b, c), add(b, a, c);
        }
        for (int i = 0; i < m2; i ++ )
        {
            int a, b, c;
            scanf("%d%d%d", &a, &b, &c);
            add(a, b, -c);
        }

        if (spfa()) puts("YES");
        else puts("NO");
    }

    return 0;
}
```
##### AcWing361 观光奶牛
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, M = 5010;

int n, m;
int wf[N];
int h[N], e[M], wt[M], ne[M], idx;
double dist[N];
int q[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, wt[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool check(double mid)
{
    memset(dist, 0, sizeof dist);
    memset(st, 0, sizeof st);
    memset(cnt, 0, sizeof cnt);

    int hh = 0, tt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        q[tt ++ ] = i;
        st[i] = true;
    }

    while (hh != tt)
    {
        int t = q[hh ++ ];
        if (hh == N) hh = 0;
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] < dist[t] + wf[t] - mid * wt[i])
            {
                dist[j] = dist[t] + wf[t] - mid * wt[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true;
                if (!st[j])
                {
                    q[tt ++ ] = j;
                    if (tt == N) tt = 0;
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
    for (int i = 1; i <= n; i ++ ) cin >> wf[i];

    memset(h, -1, sizeof h);
    for (int j = 0; j < m; j ++ )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }

    double l = 0, r = 1e6;
    while (r - l > 1e-4)
    {
        double mid = (l + r) / 2;
        if (check(mid)) l = mid;
        else r = mid;
    }

    printf("%.2lf\n", l);

    return 0;
}
```
##### AcWing1169 糖果
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010, M = 300010;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
LL dist[N];
int q[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++;
}

bool spfa()
{
    int hh = 0, tt = 1;
    memset(dist, -0x3f, sizeof dist);
    dist[0] = 0;
    q[0] = 0;
    st[0] = true;

    while (hh != tt)
    {
        int t = q[ -- tt];
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] < dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n + 1) return false;
                if (!st[j])
                {
                    q[tt ++ ] = j;
                    st[j] = true;
                }
            }
        }
    }

    return true;
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    while (m -- )
    {
        int x, a, b;
        scanf("%d%d%d", &x, &a, &b);
        if (x == 1) add(b, a, 0), add(a, b, 0);
        else if (x == 2) add(a, b, 1);
        else if (x == 3) add(b, a, 0);
        else if (x == 4) add(b, a, 1);
        else add(a, b, 0);
    }

    for (int i = 1; i <= n; i ++ ) add(0, i, 1);

    if (!spfa()) puts("-1");
    else
    {
        LL res = 0;
        for (int i = 1; i <= n; i ++ ) res += dist[i];
        printf("%lld\n", res);
    }

    return 0;
}
```
##### AcWing362 区间
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 50010, M = 150010;

int n;
int h[N], e[M], w[M], ne[M], idx;
int dist[N];
int q[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void spfa()
{
    memset(dist, -0x3f, sizeof dist);
    dist[0] = 0;
    st[0] = true;
    int hh = 0, tt = 1;
    q[0] = 0;

    while (hh != tt)
    {
        int t = q[hh ++ ];
        if (hh == N) hh = 0;
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] < dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                if (!st[j])
                {
                    q[tt ++ ] = j;
                    if (tt == N) tt = 0;
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
    for (int i = 1; i < N; i ++ )
    {
        add(i - 1, i, 0);
        add(i, i - 1, -1);
    }

    for (int i = 0; i < n; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        a ++, b ++ ;
        add(a - 1, b, c);
    }

    spfa();

    printf("%d\n", dist[50001]);

    return 0;
}
```
