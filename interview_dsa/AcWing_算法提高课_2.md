 
# 第三章 图论
 
##### AcWing1129 热浪
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 2510, M = 6200 * 2 + 10;

int n, m, S, T;
int h[N], e[M], w[M], ne[M], idx;
int dist[N], q[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[S] = 0;

    int hh = 0, tt = 1;
    q[0] = S, st[S] = true;

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
    cin >> n >> m >> S >> T;

    memset(h, -1, sizeof h);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c), add(b, a, c);
    }

    spfa();

    cout << dist[T] << endl;

    return 0;
}
```
##### AcWing1128 信使
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, INF = 0x3f3f3f3f;

int n, m;
int d[N][N];

int main()
{
    cin >> n >> m;

    memset(d, 0x3f, sizeof d);
    for (int i = 1; i <= n; i ++ ) d[i][i] = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        cin >> a >> b >> c;
        d[a][b] = d[b][a] = min(d[a][b], c);
    }

    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);

    int res = 0;
    for (int i = 1; i <= n; i ++ )
        if (d[1][i] == INF)
        {
            res = -1;
            break;
        }
        else res = max(res, d[1][i]);

    cout << res << endl;

    return 0;
}
```
##### AcWing1127 香甜的黄油
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 810, M = 3000, INF = 0x3f3f3f3f;

int n, p, m;
int id[N];
int h[N], e[M], w[M], ne[M], idx;
int dist[N], q[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int spfa(int start)
{
    memset(dist, 0x3f, sizeof dist);
    dist[start] = 0;

    int hh = 0, tt = 1;
    q[0] = start, st[start] = true;
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
                if (!st[j])
                {
                    q[tt ++ ] = j;
                    if (tt == N) tt = 0;
                    st[j] = true;
                }
            }
        }
    }

    int res = 0;
    for (int i = 0; i < n; i ++ )
    {
        int j = id[i];
        if (dist[j] == INF) return INF;
        res += dist[j];
    }

    return res;
}

int main()
{
    cin >> n >> p >> m;
    for (int i = 0; i < n; i ++ ) cin >> id[i];

    memset(h, -1, sizeof h);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c), add(b, a, c);
    }

    int res = INF;
    for (int i = 1; i <= p; i ++ ) res = min(res, spfa(i));

    cout << res << endl;

    return 0;
}
```
##### AcWing1126 最小花费
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 2010;

int n, m, S, T;
double g[N][N];
double dist[N];
bool st[N];

void dijkstra()
{
    dist[S] = 1;
    for (int i = 1; i <= n; i ++ )
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] < dist[j]))
                t = j;
        st[t] = true;

        for (int j = 1; j <= n; j ++ )
            dist[j] = max(dist[j], dist[t] * g[t][j]);
    }
}

int main()
{
    scanf("%d%d", &n, &m);

    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        double z = (100.0 - c) / 100;
        g[a][b] = g[b][a] = max(g[a][b], z);
    }

    cin >> S >> T;

    dijkstra();

    printf("%.8lf\n", 100 / dist[T]);

    return 0;
}
```
##### AcWing920 最优乘车
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <sstream>

using namespace std;

const int N = 510;

int m, n;
bool g[N][N];
int dist[N];
int stop[N];
int q[N];

void bfs()
{
    int hh = 0, tt = 0;
    memset(dist, 0x3f, sizeof dist);
    q[0] = 1;
    dist[1] = 0;

    while (hh <= tt)
    {
        int t = q[hh ++ ];

        for (int i = 1; i <= n; i ++ )
            if (g[t][i] && dist[i] > dist[t] + 1)
            {
                dist[i] = dist[t] + 1;
                q[ ++ tt] = i;
            }
    }
}

int main()
{
    cin >> m >> n;

    string line;
    getline(cin, line);
    while (m -- )
    {
        getline(cin, line);
        stringstream ssin(line);
        int cnt = 0, p;
        while (ssin >> p) stop[cnt ++ ] = p;
        for (int j = 0; j < cnt; j ++ )
            for (int k = j + 1; k < cnt; k ++ )
                g[stop[j]][stop[k]] = true;
    }

    bfs();

    if (dist[n] == 0x3f3f3f3f) puts("NO");
    else cout << max(dist[n] - 1, 0) << endl;

    return 0;
}
```
##### AcWing903 昂贵的聘礼
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, INF = 0x3f3f3f3f;

int n, m;
int w[N][N], level[N];
int dist[N];
bool st[N];

int dijkstra(int down, int up)
{
    memset(dist, 0x3f, sizeof dist);
    memset(st, 0, sizeof st);

    dist[0] = 0;
    for (int i = 1; i <= n + 1; i ++ )
    {
        int t = -1;
        for (int j = 0; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                 t = j;

        st[t] = true;
        for (int j = 1; j <= n; j ++ )
            if (level[j] >= down && level[j] <= up)
                dist[j] = min(dist[j], dist[t] + w[t][j]);
    }

    return dist[1];
}

int main()
{
    cin >> m >> n;

    memset(w, 0x3f, sizeof w);
    for (int i = 1; i <= n; i ++ ) w[i][i] = 0;

    for (int i = 1; i <= n; i ++ )
    {
        int price, cnt;
        cin >> price >> level[i] >> cnt;
        w[0][i] = min(price, w[0][i]);
        while (cnt -- )
        {
            int id, cost;
            cin >> id >> cost;
            w[id][i] = min(w[id][i], cost);
        }
    }

    int res = INF;
    for (int i = level[1] - m; i <= level[1]; i ++ ) res = min(res, dijkstra(i, i + m));

    cout << res << endl;

    return 0;
}
```
##### AcWing1135 新年好
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 50010, M = 200010, INF = 0x3f3f3f3f;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int q[N], dist[6][N];
int source[6];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void dijkstra(int start, int dist[])
{
    memset(dist, 0x3f, N * 4);
    dist[start] = 0;
    memset(st, 0, sizeof st);

    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, start});

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.second;
        if (st[ver]) continue;
        st[ver] = true;

        for (int i = h[ver]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[ver] + w[i])
            {
                dist[j] = dist[ver] + w[i];
                heap.push({dist[j], j});
            }
        }
    }
}

int dfs(int u, int start, int distance)
{
    if (u > 5) return distance;

    int res = INF;
    for (int i = 1; i <= 5; i ++ )
        if (!st[i])
        {
            int next = source[i];
            st[i] = true;
            res = min(res, dfs(u + 1, i, distance + dist[start][next]));
            st[i] = false;
        }

    return res;
}

int main()
{
    scanf("%d%d", &n, &m);
    source[0] = 1;
    for (int i = 1; i <= 5; i ++ ) scanf("%d", &source[i]);

    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c), add(b, a, c);
    }

    for (int i = 0; i < 6; i ++ ) dijkstra(source[i], dist[i]);

    memset(st, 0, sizeof st);
    printf("%d\n", dfs(1, 0, 0));

    return 0;
}
```
 
##### AcWing340 通信线路
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <deque>

using namespace std;

const int N = 1010, M = 20010;

int n, m, k;
int h[N], e[M], w[M], ne[M], idx;
int dist[N];
deque<int> q;
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool check(int bound)
{
    memset(dist, 0x3f, sizeof dist);
    memset(st, 0, sizeof st);

    q.push_back(1);
    dist[1] = 0;

    while (q.size())
    {
        int t = q.front();
        q.pop_front();

        if (st[t]) continue;
        st[t] = true;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i], x = w[i] > bound;
            if (dist[j] > dist[t] + x)
            {
                dist[j] = dist[t] + x;
                if (!x) q.push_front(j);
                else q.push_back(j);
            }
        }
    }

    return dist[n] <= k;
}

int main()
{
    cin >> n >> m >> k;
    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c), add(b, a, c);
    }

    int l = 0, r = 1e6 + 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }

    if (r == 1e6 + 1) cout << -1 << endl;
    else cout << r << endl;

    return 0;
}
```
##### AcWing342 道路与航线
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 25010, M = 150010, INF = 0x3f3f3f3f;

int n, mr, mp, S;
int id[N];
int h[N], e[M], w[M], ne[M], idx;
int dist[N], din[N];
vector<int> block[N];
int bcnt;
bool st[N];
queue<int> q;

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u, int bid)
{
    id[u] = bid, block[bid].push_back(u);

    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (!id[j])
            dfs(j, bid);
    }
}

void dijkstra(int bid)
{
    priority_queue<PII, vector<PII>, greater<PII>> heap;

    for (auto u : block[bid])
        heap.push({dist[u], u});

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.y, distance = t.x;
        if (st[ver]) continue;
        st[ver] = true;

        for (int i = h[ver]; ~i; i = ne[i])
        {
            int j = e[i];
            if (id[j] != id[ver] && -- din[id[j]] == 0) q.push(id[j]);
            if (dist[j] > dist[ver] + w[i])
            {
                dist[j] = dist[ver] + w[i];
                if (id[j] == id[ver]) heap.push({dist[j], j});
            }
        }
    }
}

void topsort()
{
    memset(dist, 0x3f, sizeof dist);
    dist[S] = 0;

    for (int i = 1; i <= bcnt; i ++ )
        if (!din[i])
            q.push(i);

    while (q.size())
    {
        int t = q.front();
        q.pop();
        dijkstra(t);
    }
}

int main()
{
    cin >> n >> mr >> mp >> S;
    memset(h, -1, sizeof h);

    while (mr -- )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c), add(b, a, c);
    }

    for (int i = 1; i <= n; i ++ )
        if (!id[i])
        {
            bcnt ++ ;
            dfs(i, bcnt);
        }

    while (mp -- )
    {
        int a, b, c;
        cin >> a >> b >> c;
        din[id[b]] ++ ;
        add(a, b, c);
    }

    topsort();

    for (int i = 1; i <= n; i ++ )
        if (dist[i] > INF / 2) cout << "NO PATH" << endl;
        else cout << dist[i] << endl;

    return 0;
}
```

##### AcWing341 最优贸易
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 2000010;

int n, m;
int w[N];
int hs[N], ht[N], e[M], ne[M], idx;
int dmin[N], dmax[N];
int q[N];
bool st[N];

void add(int h[], int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void spfa(int h[], int dist[], int type)
{
    int hh = 0, tt = 1;
    if (type == 0)
    {
        memset(dist, 0x3f, sizeof dmin);
        dist[1] = w[1];
        q[0] = 1;
    }
    else
    {
        memset(dist, -0x3f, sizeof dmax);
        dist[n] = w[n];
        q[0] = n;
    }

    while (hh != tt)
    {
        int t = q[hh ++ ];
        if (hh == N) hh = 0;

        st[t] = false;
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (type == 0 && dist[j] > min(dist[t], w[j]) || type == 1 && dist[j] < max(dist[t], w[j]))
            {
                if (type == 0) dist[j] = min(dist[t], w[j]);
                else dist[j] = max(dist[t], w[j]);

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
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    memset(hs, -1, sizeof hs);
    memset(ht, -1, sizeof ht);

    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(hs, a, b), add(ht, b, a);
        if (c == 2) add(hs, b, a), add(ht, a, b);
    }

    spfa(hs, dmin, 0);
    spfa(ht, dmax, 1);

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, dmax[i] - dmin[i]);

    printf("%d\n", res);

    return 0;
}

```
##### AcWing383 观光
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
##### AcWing1134 最短路计数
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 400010, mod = 100003;

int n, m;
int h[N], e[M], ne[M], idx;
int dist[N], cnt[N];
int q[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void bfs()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    cnt[1] = 1;

    int hh = 0, tt = 0;
    q[0] = 1;

    while (hh <= tt)
    {
        int t = q[hh ++ ];

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + 1)
            {
                dist[j] = dist[t] + 1;
                cnt[j] = cnt[t];
                q[ ++ tt] = j;
            }
            else if (dist[j] == dist[t] + 1)
            {
                cnt[j] = (cnt[j] + cnt[t]) % mod;
            }
        }
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

    bfs();

    for (int i = 1; i <= n; i ++ ) printf("%d\n", cnt[i]);

    return 0;
}
```
##### AcWing1131 拯救大兵瑞恩
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <deque>
#include <set>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 11, M = 360, P = 1 << 10;

int n, m, k, p;
int h[N * N], e[M], w[M], ne[M], idx;
int g[N][N], key[N * N];
int dist[N * N][P];
bool st[N * N][P];

set<PII> edges;

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void build()
{
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            for (int u = 0; u < 4; u ++ )
            {
                int x = i + dx[u], y = j + dy[u];
                if (!x || x > n || !y || y > m) continue;
                int a = g[i][j], b = g[x][y];
                if (!edges.count({a, b})) add(a, b, 0);
            }
}

int bfs()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1][0] = 0;

    deque<PII> q;
    q.push_back({1, 0});

    while (q.size())
    {
        PII t = q.front();
        q.pop_front();

        if (st[t.x][t.y]) continue;
        st[t.x][t.y] = true;

        if (t.x == n * m) return dist[t.x][t.y];

        if (key[t.x])
        {
            int state = t.y | key[t.x];
            if (dist[t.x][state] > dist[t.x][t.y])
            {
                dist[t.x][state] = dist[t.x][t.y];
                q.push_front({t.x, state});
            }
        }

        for (int i = h[t.x]; ~i; i = ne[i])
        {
            int j = e[i];
            if (w[i] && !(t.y >> w[i] - 1 & 1)) continue;   // 有门并且没有钥匙
            if (dist[j][t.y] > dist[t.x][t.y] + 1)
            {
                dist[j][t.y] = dist[t.x][t.y] + 1;
                q.push_back({j, t.y});
            }
        }
    }

    return -1;
}

int main()
{
    cin >> n >> m >> p >> k;

    for (int i = 1, t = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            g[i][j] = t ++ ;

    memset(h, -1, sizeof h);
    while (k -- )
    {
        int x1, y1, x2, y2, c;
        cin >> x1 >> y1 >> x2 >> y2 >> c;
        int a = g[x1][y1], b = g[x2][y2];

        edges.insert({a, b}), edges.insert({b, a});
        if (c) add(a, b, c), add(b, a, c);
    }

    build();

    int s;
    cin >> s;
    while (s -- )
    {
        int x, y, c;
        cin >> x >> y >> c;
        key[g[x][y]] |= 1 << c - 1;
    }

    cout << bfs() << endl;

    return 0;
}
```
##### AcWing1137 选择最佳线路
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, M = 20010, INF = 0x3f3f3f3f;

int n, m, T;
int h[N], e[M], w[M], ne[M], idx;
int dist[N], q[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void spfa()
{
    int scnt;
    scanf("%d", &scnt);

    memset(dist, 0x3f, sizeof dist);

    int hh = 0, tt = 0;
    while (scnt -- )
    {
        int u;
        scanf("%d", &u);
        dist[u] = 0;
        q[tt ++ ] = u;
        st[u] = true;
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
    while (scanf("%d%d%d", &n, &m, &T) != -1)
    {
        memset(h, -1, sizeof h);
        idx = 0;

        while (m -- )
        {
            int a, b, c;
            scanf("%d%d%d", &a, &b, &c);
            add(a, b, c);
        }

        spfa();

        if (dist[T] == INF) dist[T] = -1;
        printf("%d\n", dist[T]);
    }

    return 0;
}
```
##### AcWing343 排序
```cpp
传递闭包O(mn3)
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 26;

int n, m;
bool g[N][N], d[N][N];
bool st[N];

void floyd()
{
    memcpy(d, g, sizeof d);

    for (int k = 0; k < n; k ++ )
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                d[i][j] |= d[i][k] && d[k][j];
}

int check()
{
    for (int i = 0; i < n; i ++ )
        if (d[i][i])
            return 2;

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < i; j ++ )
            if (!d[i][j] && !d[j][i])
                return 0;

    return 1;
}

char get_min()
{
    for (int i = 0; i < n; i ++ )
        if (!st[i])
        {
            bool flag = true;
            for (int j = 0; j < n; j ++ )
                if (!st[j] && d[j][i])
                {
                    flag = false;
                    break;
                }
            if (flag)
            {
                st[i] = true;
                return 'A' + i;
            }
        }
}

int main()
{
    while (cin >> n >> m, n || m)
    {
        memset(g, 0, sizeof g);
        int type = 0, t;
        for (int i = 1; i <= m; i ++ )
        {
            char str[5];
            cin >> str;
            int a = str[0] - 'A', b = str[2] - 'A';

            if (!type)
            {
                g[a][b] = 1;
                floyd();
                type = check();
                if (type) t = i;
            }
        }

        if (!type) puts("Sorted sequence cannot be determined.");
        else if (type == 2) printf("Inconsistency found after %d relations.\n", t);
        else
        {
            memset(st, 0, sizeof st);
            printf("Sorted sequence determined after %d relations: ", t);
            for (int i = 0; i < n; i ++ ) printf("%c", get_min());
            printf(".\n");
        }
    }

    return 0;
}
增量算法 O(mn2)
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 26;

int n, m;
bool d[N][N];
bool st[N];

int check()
{
    for (int i = 0; i < n; i ++ )
        if (d[i][i])
            return 2;

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < i; j ++ )
            if (!d[i][j] && !d[j][i])
                return 0;

    return 1;
}

char get_min()
{
    for (int i = 0; i < n; i ++ )
        if (!st[i])
        {
            bool flag = true;
            for (int j = 0; j < n; j ++ )
                if (!st[j] && d[j][i])
                {
                    flag = false;
                    break;
                }
            if (flag)
            {
                st[i] = true;
                return 'A' + i;
            }
        }
}

int main()
{
    while (cin >> n >> m, n || m)
    {
        memset(d, 0, sizeof d);

        int type = 0, t;
        for (int i = 1; i <= m; i ++ )
        {
            char str[5];
            cin >> str;
            int a = str[0] - 'A', b = str[2] - 'A';

            if (!type)
            {
                d[a][b] = 1;
                for (int x = 0; x < n; x ++ )
                {
                    if (d[x][a]) d[x][b] = 1;
                    if (d[b][x]) d[a][x] = 1;
                    for (int y = 0; y < n; y ++ )
                        if (d[x][a] && d[b][y])
                            d[x][y] = 1;
                }
                type = check();
                if (type) t = i;
            }
        }

        if (!type) puts("Sorted sequence cannot be determined.");
        else if (type == 2) printf("Inconsistency found after %d relations.\n", t);
        else
        {
            memset(st, 0, sizeof st);
            printf("Sorted sequence determined after %d relations: ", t);
            for (int i = 0; i < n; i ++ ) printf("%c", get_min());
            printf(".\n");
        }
    }

    return 0;
}
```
##### AcWing1125 牛的旅行
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

#define x first
#define y second

using namespace std;

typedef pair<double, double> PDD;

const int N = 155;
const double INF = 1e20;

int n;
PDD q[N];
double d[N][N];
double maxd[N];
char g[N][N];

double get_dist(PDD a, PDD b)
{
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> q[i].x >> q[i].y;
    for (int i = 0; i < n; i ++ ) cin >> g[i];

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            if (i == j) d[i][j] = 0;
            else if (g[i][j] == '1') d[i][j] = get_dist(q[i], q[j]);
            else d[i][j] = INF;

    for (int k = 0; k < n; k ++ )
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);

    double r1 = 0;
    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0; j < n; j ++ )
            if (d[i][j] < INF / 2)
                maxd[i] = max(maxd[i], d[i][j]);
        r1 = max(r1, maxd[i]);
    }

    double r2 = INF;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            if (d[i][j] > INF / 2)
                r2 = min(r2, maxd[i] + maxd[j] + get_dist(q[i], q[j]));

    printf("%.6lf\n", max(r1, r2));

    return 0;
}
```
##### AcWing344 观光之旅
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, INF = 0x3f3f3f3f;

int n, m;
int d[N][N], g[N][N];
int pos[N][N];
int path[N], cnt;

void get_path(int i, int j)
{
    if (pos[i][j] == 0) return;

    int k = pos[i][j];
    get_path(i, k);
    path[cnt ++ ] = k;
    get_path(k, j);
}

int main()
{
    cin >> n >> m;

    memset(g, 0x3f, sizeof g);
    for (int i = 1; i <= n; i ++ ) g[i][i] = 0;

    while (m -- )
    {
        int a, b, c;
        cin >> a >> b >> c;
        g[a][b] = g[b][a] = min(g[a][b], c);
    }

    int res = INF;
    memcpy(d, g, sizeof d);
    for (int k = 1; k <= n; k ++ )
    {
        for (int i = 1; i < k; i ++ )
            for (int j = i + 1; j < k; j ++ )
                if ((long long)d[i][j] + g[j][k] + g[k][i] < res)
                {
                    res = d[i][j] + g[j][k] + g[k][i];
                    cnt = 0;
                    path[cnt ++ ] = k;
                    path[cnt ++ ] = i;
                    get_path(i, j);
                    path[cnt ++ ] = j;
                }

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                if (d[i][j] > d[i][k] + d[k][j])
                {
                    d[i][j] = d[i][k] + d[k][j];
                    pos[i][j] = k;
                }
    }

    if (res == INF) puts("No solution.");
    else
    {
        for (int i = 0; i < cnt; i ++ ) cout << path[i] << ' ';
        cout << endl;
    }

    return 0;
}
```
##### AcWing345 牛站
```cpp

16


3
#include <cstring>
#include <iostream>
#include <algorithm>
#include <map>

using namespace std;

const int N = 210;

int k, n, m, S, E;
int g[N][N];
int res[N][N];

void mul(int c[][N], int a[][N], int b[][N])
{
    static int temp[N][N];
    memset(temp, 0x3f, sizeof temp);
    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                temp[i][j] = min(temp[i][j], a[i][k] + b[k][j]);
    memcpy(c, temp, sizeof temp);
}

void qmi()
{
    memset(res, 0x3f, sizeof res);
    for (int i = 1; i <= n; i ++ ) res[i][i] = 0;

    while (k)
    {
        if (k & 1) mul(res, res, g);    // res = res * g
        mul(g, g, g);   // g = g * g
        k >>= 1;
    }
}

int main()
{
    cin >> k >> m >> S >> E;

    memset(g, 0x3f, sizeof g);
    map<int, int> ids;
    if (!ids.count(S)) ids[S] = ++ n;
    if (!ids.count(E)) ids[E] = ++ n;
    S = ids[S], E = ids[E];

    while (m -- )
    {
        int a, b, c;
        cin >> c >> a >> b;
        if (!ids.count(a)) ids[a] = ++ n;
        if (!ids.count(b)) ids[b] = ++ n;
        a = ids[a], b = ids[b];

        g[a][b] = g[b][a] = min(g[a][b], c);
    }

    qmi();

    cout << res[S][E] << endl;

    return 0;
}
```
##### AcWing1140 最短网络
```cpp

16


3
#include <cstring>
#include <iostream>
#include <algorithm>
#include <map>

using namespace std;

const int N = 210;

int k, n, m, S, E;
int g[N][N];
int res[N][N];

void mul(int c[][N], int a[][N], int b[][N])
{
    static int temp[N][N];
    memset(temp, 0x3f, sizeof temp);
    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                temp[i][j] = min(temp[i][j], a[i][k] + b[k][j]);
    memcpy(c, temp, sizeof temp);
}

void qmi()
{
    memset(res, 0x3f, sizeof res);
    for (int i = 1; i <= n; i ++ ) res[i][i] = 0;

    while (k)
    {
        if (k & 1) mul(res, res, g);    // res = res * g
        mul(g, g, g);   // g = g * g
        k >>= 1;
    }
}

int main()
{
    cin >> k >> m >> S >> E;

    memset(g, 0x3f, sizeof g);
    map<int, int> ids;
    if (!ids.count(S)) ids[S] = ++ n;
    if (!ids.count(E)) ids[E] = ++ n;
    S = ids[S], E = ids[E];

    while (m -- )
    {
        int a, b, c;
        cin >> c >> a >> b;
        if (!ids.count(a)) ids[a] = ++ n;
        if (!ids.count(b)) ids[b] = ++ n;
        a = ids[a], b = ids[b];

        g[a][b] = g[b][a] = min(g[a][b], c);
    }

    qmi();

    cout << res[S][E] << endl;

    return 0;
}
```
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

##### AcWing1170 排队布局
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, M = 10000 + 10000 + 1000 + 10, INF = 0x3f3f3f3f;

int n, m1, m2;
int h[N], e[M], w[M], ne[M], idx;
int dist[N];
int q[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool spfa(int size)
{
    int hh = 0, tt = 0;
    memset(dist, 0x3f, sizeof dist);
    memset(st, 0, sizeof st);
    memset(cnt, 0, sizeof cnt);

    for (int i = 1; i <= size; i ++ )
    {
        q[tt ++ ] = i;
        dist[i] = 0;
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
    scanf("%d%d%d", &n, &m1, &m2);
    memset(h, -1, sizeof h);

    for (int i = 1; i < n; i ++ ) add(i + 1, i, 0);
    while (m1 -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        if (a > b) swap(a, b);
        add(a, b, c);
    }
    while (m2 -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        if (a > b) swap(a, b);
        add(b, a, -c);
    }

    if (spfa(n)) puts("-1");
    else
    {
        spfa(1);
        if (dist[n] == INF) puts("-2");
        else printf("%d\n", dist[n]);
    }

    return 0;
}
```
##### AcWing393 雇佣收银员
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 30, M = 100, INF = 0x3f3f3f3f;

int n;
int h[N], e[M], w[M], ne[M], idx;
int r[N], num[N];
int dist[N];
int q[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void build(int c)
{
    memset(h, -1, sizeof h);
    idx = 0;
    add(0, 24, c), add(24, 0, -c);
    for (int i = 1; i <= 7; i ++ ) add(i + 16, i, r[i] - c);
    for (int i = 8; i <= 24; i ++ ) add(i - 8, i, r[i]);
    for (int i = 1; i <= 24; i ++ )
    {
        add(i, i - 1, -num[i]);
        add(i - 1, i, 0);
    }
}

bool spfa(int c)
{
    build(c);

    memset(dist, -0x3f, sizeof dist);
    memset(cnt, 0, sizeof cnt);
    memset(st, 0, sizeof st);

    int hh = 0, tt = 1;
    dist[0] = 0;
    q[0] = 0;
    st[0] = true;

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
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= 25) return false;
                if (!st[j])
                {
                    q[tt ++ ] = j;
                    if (tt == N) tt = 0;
                    st[j] = true;
                }
            }
        }
    }

    return true;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        for (int i = 1; i <= 24; i ++ ) cin >> r[i];
        cin >> n;
        memset(num, 0, sizeof num);
        for (int i = 0; i < n; i ++ )
        {
            int t;
            cin >> t;
            num[t + 1] ++ ;
        }

        bool success = false;
        for (int i = 0; i <= 1000; i ++ )
            if (spfa(i))
            {
                cout << i << endl;
                success = true;
                break;
            }

        if (!success) puts("No Solution");
    }

    return 0;
}
```
##### AcWing1172 祖孙询问
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 40010, M = N * 2;

int n, m;
int h[N], e[M], ne[M], idx;
int depth[N], fa[N][16];
int q[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void bfs(int root)
{
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0, depth[root] = 1;
    int hh = 0, tt = 0;
    q[0] = root;
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (depth[j] > depth[t] + 1)
            {
                depth[j] = depth[t] + 1;
                q[ ++ tt] = j;
                fa[j][0] = t;
                for (int k = 1; k <= 15; k ++ )
                    fa[j][k] = fa[fa[j][k - 1]][k - 1];
            }
        }
    }
}

int lca(int a, int b)
{
    if (depth[a] < depth[b]) swap(a, b);
    for (int k = 15; k >= 0; k -- )
        if (depth[fa[a][k]] >= depth[b])
            a = fa[a][k];
    if (a == b) return a;
    for (int k = 15; k >= 0; k -- )
        if (fa[a][k] != fa[b][k])
        {
            a = fa[a][k];
            b = fa[b][k];
        }
    return fa[a][0];
}

int main()
{
    scanf("%d", &n);
    int root = 0;
    memset(h, -1, sizeof h);

    for (int i = 0; i < n; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        if (b == -1) root = a;
        else add(a, b), add(b, a);
    }

    bfs(root);

    scanf("%d", &m);
    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        int p = lca(a, b);
        if (p == a) puts("1");
        else if (p == b) puts("2");
        else puts("0");
    }

    return 0;
}
```
##### AcWing1171 距离
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef pair<int, int> PII;

const int N = 10010, M = N * 2;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int dist[N];
int p[N];
int res[M];
int st[N];
vector<PII> query[N];   // first存查询的另外一个点，second存查询编号

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u, int fa)
{
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (j == fa) continue;
        dist[j] = dist[u] + w[i];
        dfs(j, u);
    }
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

void tarjan(int u)
{
    st[u] = 1;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            tarjan(j);
            p[j] = u;
        }
    }

    for (auto item : query[u])
    {
        int y = item.first, id = item.second;
        if (st[y] == 2)
        {
            int anc = find(y);
            res[id] = dist[u] + dist[y] - dist[anc] * 2;
        }
    }

    st[u] = 2;
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c), add(b, a, c);
    }

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        if (a != b)
        {
            query[a].push_back({b, i});
            query[b].push_back({a, i});
        }
    }

    for (int i = 1; i <= n; i ++ ) p[i] = i;

    dfs(1, -1);
    tarjan(1);

    for (int i = 0; i < m; i ++ ) printf("%d\n", res[i]);

    return 0;
}
```
##### AcWing356 次小生成树
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010, M = 300010, INF = 0x3f3f3f3f;

int n, m;
struct Edge
{
    int a, b, w;
    bool used;
    bool operator< (const Edge &t) const
    {
        return w < t.w;
    }
}edge[M];
int p[N];
int h[N], e[M], w[M], ne[M], idx;
int depth[N], fa[N][17], d1[N][17], d2[N][17];
int q[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

LL kruskal()
{
    for (int i = 1; i <= n; i ++ ) p[i] = i;
    sort(edge, edge + m);
    LL res = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = find(edge[i].a), b = find(edge[i].b), w = edge[i].w;
        if (a != b)
        {
            p[a] = b;
            res += w;
            edge[i].used = true;
        }
    }

    return res;
}

void build()
{
    memset(h, -1, sizeof h);
    for (int i = 0; i < m; i ++ )
        if (edge[i].used)
        {
            int a = edge[i].a, b = edge[i].b, w = edge[i].w;
            add(a, b, w), add(b, a, w);
        }
}

void bfs()
{
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0, depth[1] = 1;
    q[0] = 1;
    int hh = 0, tt = 0;
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (depth[j] > depth[t] + 1)
            {
                depth[j] = depth[t] + 1;
                q[ ++ tt] = j;
                fa[j][0] = t;
                d1[j][0] = w[i], d2[j][0] = -INF;
                for (int k = 1; k <= 16; k ++ )
                {
                    int anc = fa[j][k - 1];
                    fa[j][k] = fa[anc][k - 1];
                    int distance[4] = {d1[j][k - 1], d2[j][k - 1], d1[anc][k - 1], d2[anc][k - 1]};
                    d1[j][k] = d2[j][k] = -INF;
                    for (int u = 0; u < 4; u ++ )
                    {
                        int d = distance[u];
                        if (d > d1[j][k]) d2[j][k] = d1[j][k], d1[j][k] = d;
                        else if (d != d1[j][k] && d > d2[j][k]) d2[j][k] = d;
                    }
                }
            }
        }
    }
}

int lca(int a, int b, int w)
{
    static int distance[N * 2];
    int cnt = 0;
    if (depth[a] < depth[b]) swap(a, b);
    for (int k = 16; k >= 0; k -- )
        if (depth[fa[a][k]] >= depth[b])
        {
            distance[cnt ++ ] = d1[a][k];
            distance[cnt ++ ] = d2[a][k];
            a = fa[a][k];
        }
    if (a != b)
    {
        for (int k = 16; k >= 0; k -- )
            if (fa[a][k] != fa[b][k])
            {
                distance[cnt ++ ] = d1[a][k];
                distance[cnt ++ ] = d2[a][k];
                distance[cnt ++ ] = d1[b][k];
                distance[cnt ++ ] = d2[b][k];
                a = fa[a][k], b = fa[b][k];
            }
        distance[cnt ++ ] = d1[a][0];
        distance[cnt ++ ] = d1[b][0];
    }

    int dist1 = -INF, dist2 = -INF;
    for (int i = 0; i < cnt; i ++ )
    {
        int d = distance[i];
        if (d > dist1) dist2 = dist1, dist1 = d;
        else if (d != dist1 && d > dist2) dist2 = d;
    }

    if (w > dist1) return w - dist1;
    if (w > dist2) return w - dist2;
    return INF;
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        edge[i] = {a, b, c};
    }

    LL sum = kruskal();
    build();
    bfs();

    LL res = 1e18;
    for (int i = 0; i < m; i ++ )
        if (!edge[i].used)
        {
            int a = edge[i].a, b = edge[i].b, w = edge[i].w;
            res = min(res, sum + lca(a, b, w));
        }
    printf("%lld\n", res);

    return 0;
}
```
##### AcWing352 闇の連鎖
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = N * 2;

int n, m;
int h[N], e[M], ne[M], idx;
int depth[N], fa[N][17];
int d[N];
int q[N];
int ans;

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void bfs()
{
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0, depth[1] = 1;
    int hh = 0, tt = 0;
    q[0] = 1;

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (depth[j] > depth[t] + 1)
            {
                depth[j] = depth[t] + 1;
                q[ ++ tt] = j;
                fa[j][0] = t;
                for (int k = 1; k <= 16; k ++ )
                    fa[j][k] = fa[fa[j][k - 1]][k - 1];
            }
        }
    }
}

int lca(int a, int b)
{
    if (depth[a] < depth[b]) swap(a, b);
    for (int k = 16; k >= 0; k -- )
        if (depth[fa[a][k]] >= depth[b])
            a = fa[a][k];
    if (a == b) return a;
    for (int k = 16; k >= 0; k -- )
        if (fa[a][k] != fa[b][k])
        {
            a = fa[a][k];
            b = fa[b][k];
        }
    return fa[a][0];
}

int dfs(int u, int father)
{
    int res = d[u];
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (j != father)
        {
            int s = dfs(j, u);
            if (s == 0) ans += m;
            else if (s == 1) ans ++ ;
            res += s;
        }
    }

    return res;
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b), add(b, a);
    }

    bfs();

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        int p = lca(a, b);
        d[a] ++, d[b] ++, d[p] -= 2;
    }
    dfs(1, -1);
    printf("%d\n", ans);

    return 0;
}
```
##### AcWing1174 受欢迎的牛
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010, M = 50010;

int n, m;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
bool in_stk[N];
int id[N], scc_cnt, Size[N];
int dout[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void tarjan(int u)
{
    dfn[u] = low[u] = ++ timestamp;
    stk[ ++ top] = u, in_stk[u] = true;
    for (int i = h[u]; i != -1; i = ne[i])
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
            Size[scc_cnt] ++ ;
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
        add(a, b);
    }

    for (int i = 1; i <= n; i ++ )
        if (!dfn[i])
            tarjan(i);

    for (int i = 1; i <= n; i ++ )
        for (int j = h[i]; ~j; j = ne[j])
        {
            int k = e[j];
            int a = id[i], b = id[k];
            if (a != b) dout[a] ++ ;
        }

    int zeros = 0, sum = 0;
    for (int i = 1; i <= scc_cnt; i ++ )
        if (!dout[i])
        {
            zeros ++ ;
            sum += Size[i];
            if (zeros > 1)
            {
                sum = 0;
                break;
            }
        }

    printf("%d\n", sum);

    return 0;
}
```
##### AcWing367 学校网络
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = 10010;

int n;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
bool in_stk[N];
int id[N], scc_cnt;
int din[N], dout[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
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
        else if (in_stk[j])
            low[u] = min(low[u], dfn[j]);
    }

    if (dfn[u] == low[u])
    {
        ++ scc_cnt;
        int y;
        do {
            y = stk[top -- ];
            in_stk[y] = false;
            id[y] = scc_cnt;
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
        for (int j = h[i]; j != -1; j = ne[j])
        {
            int k = e[j];
            int a = id[i], b = id[k];
            if (a != b)
            {
                dout[a] ++ ;
                din[b] ++ ;
            }
        }

    int a = 0, b = 0;
    for (int i = 1; i <= scc_cnt; i ++ )
    {
        if (!din[i]) a ++ ;
        if (!dout[i]) b ++ ;
    }

    printf("%d\n", a);
    if (scc_cnt == 1) puts("0");
    else printf("%d\n", max(a, b));

    return 0;
}
```
##### AcWing1175 最大半连通子图
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_set>

using namespace std;

typedef long long LL;

const int N = 100010, M = 2000010;

int n, m, mod;
int h[N], hs[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
bool in_stk[N];
int id[N], scc_cnt, scc_size[N];
int f[N], g[N];

void add(int h[], int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
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
            scc_size[scc_cnt] ++ ;
        } while (y != u);
    }
}

int main()
{
    memset(h, -1, sizeof h);
    memset(hs, -1, sizeof hs);

    scanf("%d%d%d", &n, &m, &mod);
    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(h, a, b);
    }

    for (int i = 1; i <= n; i ++ )
        if (!dfn[i])
            tarjan(i);

    unordered_set<LL> S;    // (u, v) -> u * 1000000 + v
    for (int i = 1; i <= n; i ++ )
        for (int j = h[i]; ~j; j = ne[j])
        {
            int k = e[j];
            int a = id[i], b = id[k];
            LL hash = a * 1000000ll + b;
            if (a != b && !S.count(hash))
            {
                add(hs, a, b);
                S.insert(hash);
            }
        }

    for (int i = scc_cnt; i; i -- )
    {
        if (!f[i])
        {
            f[i] = scc_size[i];
            g[i] = 1;
        }
        for (int j = hs[i]; ~j; j = ne[j])
        {
            int k = e[j];
            if (f[k] < f[i] + scc_size[k])
            {
                f[k] = f[i] + scc_size[k];
                g[k] = g[i];
            }
            else if (f[k] == f[i] + scc_size[k])
                g[k] = (g[k] + g[i]) % mod;
        }
    }

    int maxf = 0, sum = 0;
    for (int i = 1; i <= scc_cnt; i ++ )
        if (f[i] > maxf)
        {
            maxf = f[i];
            sum = g[i];
        }
        else if (f[i] == maxf) sum = (sum + g[i]) % mod;

    printf("%d\n", maxf);
    printf("%d\n", sum);

    return 0;
}
```

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
 
# 第四章 高级数据结构
 
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
##### AcWing245 你能回答这些问题吗
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 500010;

int n, m;
int w[N];
struct Node
{
    int l, r;
    int sum, lmax, rmax, tmax;
}tr[N * 4];

void pushup(Node &u, Node &l, Node &r)
{
    u.sum = l.sum + r.sum;
    u.lmax = max(l.lmax, l.sum + r.lmax);
    u.rmax = max(r.rmax, r.sum + l.rmax);
    u.tmax = max(max(l.tmax, r.tmax), l.rmax + r.lmax);
}

void pushup(int u)
{
    pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
}

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, w[r], w[r], w[r], w[r]};
    else
    {
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int x, int v)
{
    if (tr[u].l == x && tr[u].r == x) tr[u] = {x, x, v, v, v, v};
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (x <= mid) modify(u << 1, x, v);
        else modify(u << 1 | 1, x, v);
        pushup(u);
    }
}

Node query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u];
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (r <= mid) return query(u << 1, l, r);
        else if (l > mid) return query(u << 1 | 1, l, r);
        else
        {
            auto left = query(u << 1, l, r);
            auto right = query(u << 1 | 1, l, r);
            Node res;
            pushup(res, left, right);
            return res;
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    build(1, 1, n);

    int k, x, y;
    while (m -- )
    {
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
##### AcWing246 区间最大公约数
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
struct Node
{
    int l, r;
    LL sum, d;
}tr[N * 4];

LL gcd(LL a, LL b)
{
    return b ? gcd(b, a % b) : a;
}

void pushup(Node &u, Node &l, Node &r)
{
    u.sum = l.sum + r.sum;
    u.d = gcd(l.d, r.d);
}

void pushup(int u)
{
    pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
}

void build(int u, int l, int r)
{
    if (l == r)
    {
        LL b = w[r] - w[r - 1];
        tr[u] = {l, r, b, b};
    }
    else
    {
        tr[u].l = l, tr[u].r = r;
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int x, LL v)
{
    if (tr[u].l == x && tr[u].r == x)
    {
        LL b = tr[u].sum + v;
        tr[u] = {x, x, b, b};
    }
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (x <= mid) modify(u << 1, x, v);
        else modify(u << 1 | 1, x, v);
        pushup(u);
    }
}

Node query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u];
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (r <= mid) return query(u << 1, l, r);
        else if (l > mid) return query(u << 1 | 1, l, r);
        else
        {
            auto left = query(u << 1, l, r);
            auto right = query(u << 1 | 1, l, r);
            Node res;
            pushup(res, left, right);
            return res;
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%lld", &w[i]);
    build(1, 1, n);

    int l, r;
    LL d;
    char op[2];
    while (m -- )
    {
        scanf("%s%d%d", op, &l, &r);
        if (*op == 'Q')
        {
            auto left = query(1, 1, l);
            Node right({0, 0, 0, 0});
            if (l + 1 <= r) right = query(1, l + 1, r);
            printf("%lld\n", abs(gcd(left.sum, right.d)));
        }
        else
        {
            scanf("%lld", &d);
            modify(1, l, d);
            if (r + 1 <= n) modify(1, r + 1, -d);
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
int w[N];
struct Node
{
    int l, r;
    LL sum, add;
}tr[N * 4];

void pushup(int u)
{
    tr[u].sum = tr[u << 1].sum + tr[u << 1 | 1].sum;
}

void pushdown(int u)
{
    auto &root = tr[u], &left = tr[u << 1], &right = tr[u << 1 | 1];
    if (root.add)
    {
        left.add += root.add, left.sum += (LL)(left.r - left.l + 1) * root.add;
        right.add += root.add, right.sum += (LL)(right.r - right.l + 1) * root.add;
        root.add = 0;
    }
}

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, w[r], 0};
    else
    {
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int l, int r, int d)
{
    if (tr[u].l >= l && tr[u].r <= r)
    {
        tr[u].sum += (LL)(tr[u].r - tr[u].l + 1) * d;
        tr[u].add += d;
    }
    else    // 一定要分裂
    {
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) modify(u << 1, l, r, d);
        if (r > mid) modify(u << 1 | 1, l, r, d);
        pushup(u);
    }
}

LL query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].sum;

    pushdown(u);
    int mid = tr[u].l + tr[u].r >> 1;
    LL sum = 0;
    if (l <= mid) sum = query(u << 1, l, r);
    if (r > mid) sum += query(u << 1 | 1, l, r);
    return sum;
}


int main()
{
    scanf("%d%d", &n, &m);

    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    build(1, 1, n);

    char op[2];
    int l, r, d;

    while (m -- )
    {
        scanf("%s%d%d", op, &l, &r);
        if (*op == 'C')
        {
            scanf("%d", &d);
            modify(1, l, r, d);
        }
        else printf("%lld\n", query(1, l, r));
    }

    return 0;
}
```
##### AcWing247 亚特兰蒂斯
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 100010;

int n;
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

int find(double y)
{
    return lower_bound(ys.begin(), ys.end(), y) - ys.begin();
}

void pushup(int u)
{
    if (tr[u].cnt) tr[u].len = ys[tr[u].r + 1] - ys[tr[u].l];
    else if (tr[u].l != tr[u].r)
    {
        tr[u].len = tr[u << 1].len + tr[u << 1 | 1].len;
    }
    else tr[u].len = 0;
}

void build(int u, int l, int r)
{
    tr[u] = {l, r, 0, 0};
    if (l != r)
    {
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
    }
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
        for (int i = 0, j = 0; i < n; i ++ )
        {
            double x1, y1, x2, y2;
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            seg[j ++ ] = {x1, y1, y2, 1};
            seg[j ++ ] = {x2, y1, y2, -1};
            ys.push_back(y1), ys.push_back(y2);
        }

        sort(ys.begin(), ys.end());
        ys.erase(unique(ys.begin(), ys.end()), ys.end());

        build(1, 0, ys.size() - 2);

        sort(seg, seg + n * 2);

        double res = 0;
        for (int i = 0; i < n * 2; i ++ )
        {
            if (i > 0) res += tr[1].len * (seg[i].x - seg[i - 1].x);
            modify(1, find(seg[i].y1), find(seg[i].y2) - 1, seg[i].k);
        }

        printf("Test case #%d\n", T ++ );
        printf("Total explored area: %.2lf\n\n", res);
    }

    return 0;
}
```
##### AcWing1277 维护序列
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010;

int n, p, m;
int w[N];
struct Node
{
    int l, r;
    int sum, add, mul;
}tr[N * 4];

void pushup(int u)
{
    tr[u].sum = (tr[u << 1].sum + tr[u << 1 | 1].sum) % p;
}

void eval(Node &t, int add, int mul)
{
    t.sum = ((LL)t.sum * mul + (LL)(t.r - t.l + 1) * add) % p;
    t.mul = (LL)t.mul * mul % p;
    t.add = ((LL)t.add * mul + add) % p;
}

void pushdown(int u)
{
    eval(tr[u << 1], tr[u].add, tr[u].mul);
    eval(tr[u << 1 | 1], tr[u].add, tr[u].mul);
    tr[u].add = 0, tr[u].mul = 1;
}

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, w[r], 0, 1};
    else
    {
        tr[u] = {l, r, 0, 0, 1};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int l, int r, int add, int mul)
{
    if (tr[u].l >= l && tr[u].r <= r) eval(tr[u], add, mul);
    else
    {
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) modify(u << 1, l, r, add, mul);
        if (r > mid) modify(u << 1 | 1, l, r, add, mul);
        pushup(u);
    }
}

int query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].sum;

    pushdown(u);
    int mid = tr[u].l + tr[u].r >> 1;
    int sum = 0;
    if (l <= mid) sum = query(u << 1, l, r);
    if (r > mid) sum = (sum + query(u << 1 | 1, l, r)) % p;
    return sum;
}

int main()
{
    scanf("%d%d", &n, &p);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    build(1, 1, n);

    scanf("%d", &m);
    while (m -- )
    {
        int t, l, r, d;
        scanf("%d%d%d", &t, &l, &r);
        if (t == 1)
        {
            scanf("%d", &d);
            modify(1, l, r, 0, d);
        }
        else if (t == 2)
        {
            scanf("%d", &d);
            modify(1, l, r, d, 1);
        }
        else printf("%d\n", query(1, l, r));
    }

    return 0;
}
```
##### AcWing256 最大异或和
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 600010, M = N * 25;

int n, m;
int s[N];
int tr[M][2], max_id[M];
int root[N], idx;

void insert(int i, int k, int p, int q)
{
    if (k < 0)
    {
        max_id[q] = i;
        return;
    }
    int v = s[i] >> k & 1;
    if (p) tr[q][v ^ 1] = tr[p][v ^ 1];
    tr[q][v] = ++ idx;
    insert(i, k - 1, tr[p][v], tr[q][v]);
    max_id[q] = max(max_id[tr[q][0]], max_id[tr[q][1]]);
}

int query(int root, int C, int L)
{
    int p = root;
    for (int i = 23; i >= 0; i -- )
    {
        int v = C >> i & 1;
        if (max_id[tr[p][v ^ 1]] >= L) p = tr[p][v ^ 1];
        else p = tr[p][v];
    }

    return C ^ s[max_id[p]];
}

int main()
{
    scanf("%d%d", &n, &m);

    max_id[0] = -1;
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
            printf("%d\n", query(root[r - 1], s[n] ^ x, l - 1));
        }
    }

    return 0;
}
```
##### AcWing255 第K小数
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 100010, M = 10010;

int n, m;
int a[N];
vector<int> nums;

struct Node
{
    int l, r;
    int cnt;
}tr[N * 4 + N * 17];

int root[N], idx;

int find(int x)
{
    return lower_bound(nums.begin(), nums.end(), x) - nums.begin();
}

int build(int l, int r)
{
    int p = ++ idx;
    if (l == r) return p;
    int mid = l + r >> 1;
    tr[p].l = build(l, mid), tr[p].r = build(mid + 1, r);
    return p;
}

int insert(int p, int l, int r, int x)
{
    int q = ++ idx;
    tr[q] = tr[p];
    if (l == r)
    {
        tr[q].cnt ++ ;
        return q;
    }
    int mid = l + r >> 1;
    if (x <= mid) tr[q].l = insert(tr[p].l, l, mid, x);
    else tr[q].r = insert(tr[p].r, mid + 1, r, x);
    tr[q].cnt = tr[tr[q].l].cnt + tr[tr[q].r].cnt;
    return q;
}

int query(int q, int p, int l, int r, int k)
{
    if (l == r) return r;
    int cnt = tr[tr[q].l].cnt - tr[tr[p].l].cnt;
    int mid = l + r >> 1;
    if (k <= cnt) return query(tr[q].l, tr[p].l, l, mid, k);
    else return query(tr[q].r, tr[p].r, mid + 1, r, k - cnt);
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d", &a[i]);
        nums.push_back(a[i]);
    }

    sort(nums.begin(), nums.end());
    nums.erase(unique(nums.begin(), nums.end()), nums.end());

    root[0] = build(0, nums.size() - 1);

    for (int i = 1; i <= n; i ++ )
        root[i] = insert(root[i - 1], 0, nums.size() - 1, find(a[i]));

    while (m -- )
    {
        int l, r, k;
        scanf("%d%d%d", &l, &r, &k);
        printf("%d\n", nums[query(root[r], root[l - 1], 0, nums.size() - 1, k)]);
    }

    return 0;
}
```
##### AcWing253 普通平衡树
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, INF = 1e8;

int n;
struct Node
{
    int l, r;
    int key, val;
    int cnt, size;
}tr[N];

int root, idx;

void pushup(int p)
{
    tr[p].size = tr[tr[p].l].size + tr[tr[p].r].size + tr[p].cnt;
}

int get_node(int key)
{
    tr[ ++ idx].key = key;
    tr[idx].val = rand();
    tr[idx].cnt = tr[idx].size = 1;
    return idx;
}

void zig(int &p)    // 右旋
{
    int q = tr[p].l;
    tr[p].l = tr[q].r, tr[q].r = p, p = q;
    pushup(tr[p].r), pushup(p);
}

void zag(int &p)    // 左旋
{
    int q = tr[p].r;
    tr[p].r = tr[q].l, tr[q].l = p, p = q;
    pushup(tr[p].l), pushup(p);
}

void build()
{
    get_node(-INF), get_node(INF);
    root = 1, tr[1].r = 2;
    pushup(root);

    if (tr[1].val < tr[2].val) zag(root);
}


void insert(int &p, int key)
{
    if (!p) p = get_node(key);
    else if (tr[p].key == key) tr[p].cnt ++ ;
    else if (tr[p].key > key)
    {
        insert(tr[p].l, key);
        if (tr[tr[p].l].val > tr[p].val) zig(p);
    }
    else
    {
        insert(tr[p].r, key);
        if (tr[tr[p].r].val > tr[p].val) zag(p);
    }
    pushup(p);
}

void remove(int &p, int key)
{
    if (!p) return;
    if (tr[p].key == key)
    {
        if (tr[p].cnt > 1) tr[p].cnt -- ;
        else if (tr[p].l || tr[p].r)
        {
            if (!tr[p].r || tr[tr[p].l].val > tr[tr[p].r].val)
            {
                zig(p);
                remove(tr[p].r, key);
            }
            else
            {
                zag(p);
                remove(tr[p].l, key);
            }
        }
        else p = 0;
    }
    else if (tr[p].key > key) remove(tr[p].l, key);
    else remove(tr[p].r, key);

    pushup(p);
}

int get_rank_by_key(int p, int key)    // 通过数值找排名
{
    if (!p) return 0;   // 本题中不会发生此情况
    if (tr[p].key == key) return tr[tr[p].l].size + 1;
    if (tr[p].key > key) return get_rank_by_key(tr[p].l, key);
    return tr[tr[p].l].size + tr[p].cnt + get_rank_by_key(tr[p].r, key);
}

int get_key_by_rank(int p, int rank)   // 通过排名找数值
{
    if (!p) return INF;     // 本题中不会发生此情况
    if (tr[tr[p].l].size >= rank) return get_key_by_rank(tr[p].l, rank);
    if (tr[tr[p].l].size + tr[p].cnt >= rank) return tr[p].key;
    return get_key_by_rank(tr[p].r, rank - tr[tr[p].l].size - tr[p].cnt);
}

int get_prev(int p, int key)   // 找到严格小于key的最大数
{
    if (!p) return -INF;
    if (tr[p].key >= key) return get_prev(tr[p].l, key);
    return max(tr[p].key, get_prev(tr[p].r, key));
}

int get_next(int p, int key)    // 找到严格大于key的最小数
{
    if (!p) return INF;
    if (tr[p].key <= key) return get_next(tr[p].r, key);
    return min(tr[p].key, get_next(tr[p].l, key));
}

int main()
{
    build();

    scanf("%d", &n);
    while (n -- )
    {
        int opt, x;
        scanf("%d%d", &opt, &x);
        if (opt == 1) insert(root, x);
        else if (opt == 2) remove(root, x);
        else if (opt == 3) printf("%d\n", get_rank_by_key(root, x) - 1);
        else if (opt == 4) printf("%d\n", get_key_by_rank(root, x + 1));
        else if (opt == 5) printf("%d\n", get_prev(root, x));
        else printf("%d\n", get_next(root, x));
    }

    return 0;
}
```
##### AcWing265 营业额统计
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 33010, INF = 1e7;

int n;
struct Node
{
    int l, r;
    int key, val;
}tr[N];

int root, idx;

int get_node(int key)
{
    tr[ ++ idx].key = key;
    tr[idx].val = rand();
    return idx;
}

void build()
{
    get_node(-INF), get_node(INF);
    root = 1, tr[1].r = 2;
}

void zig(int &p)
{
    int q = tr[p].l;
    tr[p].l = tr[q].r, tr[q].r = p, p = q;
}

void zag(int &p)
{
    int q = tr[p].r;
    tr[p].r = tr[q].l, tr[q].l = p, p = q;
}

void insert(int &p, int key)
{
    if (!p) p = get_node(key);
    else if (tr[p].key == key) return;
    else if (tr[p].key > key)
    {
        insert(tr[p].l, key);
        if (tr[tr[p].l].val > tr[p].val) zig(p);
    }
    else
    {
        insert(tr[p].r, key);
        if (tr[tr[p].r].val > tr[p].val) zag(p);
    }
}

int get_prev(int p, int key)    // 找到小于等于key的最大数
{
    if (!p) return -INF;
    if (tr[p].key > key) return get_prev(tr[p].l, key);
    return max(tr[p].key, get_prev(tr[p].r, key));
}

int get_next(int p, int key)    // 找到大于等于key的最小数
{
    if (!p) return INF;
    if (tr[p].key < key) return get_next(tr[p].r, key);
    return min(tr[p].key, get_next(tr[p].l, key));
}

int main()
{
    build();
    scanf("%d", &n);

    LL res = 0;
    for (int i = 1; i <= n; i ++ )
    {
        int x;
        scanf("%d", &x);
        if (i == 1) res += x;
        else res += min(x - get_prev(root, x), get_next(root, x) - x);

        insert(root, x);
    }

    printf("%lld\n", res);

    return 0;
}
```
##### AcWing1282 搜索关键词
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010, S = 55, M = 1000010;

int n;
int tr[N * S][26], cnt[N * S], idx;
char str[M];
int q[N * S], ne[N * S];

void insert()
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int t = str[i] - 'a';
        if (!tr[p][t]) tr[p][t] = ++ idx;
        p = tr[p][t];
    }
    cnt[p] ++ ;
}

void build()
{
    int hh = 0, tt = -1;
    for (int i = 0; i < 26; i ++ )
        if (tr[0][i])
            q[ ++ tt] = tr[0][i];

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = 0; i < 26; i ++ )
        {
            int p = tr[t][i];
            if (!p) tr[t][i] = tr[ne[t]][i];
            else
            {
                ne[p] = tr[ne[t]][i];
                q[ ++ tt] = p;
            }
        }
    }
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        memset(tr, 0, sizeof tr);
        memset(cnt, 0, sizeof cnt);
        memset(ne, 0, sizeof ne);
        idx = 0;

        scanf("%d", &n);
        for (int i = 0; i < n; i ++ )
        {
            scanf("%s", str);
            insert();
        }

        build();

        scanf("%s", str);

        int res = 0;
        for (int i = 0, j = 0; str[i]; i ++ )
        {
            int t = str[i] - 'a';
            j = tr[j][t];

            int p = j;
            while (p)
            {
                res += cnt[p];
                cnt[p] = 0;
                p = ne[p];
            }
        }

        printf("%d\n", res);
    }

    return 0;
}
```
##### AcWing1285 单词
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int n;
int tr[N][26], f[N], idx;
int q[N], ne[N];
char str[N];
int id[210];

void insert(int x)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int t = str[i] - 'a';
        if (!tr[p][t]) tr[p][t] = ++ idx;
        p = tr[p][t];
        f[p] ++ ;
    }
    id[x] = p;
}

void build()
{
    int hh = 0, tt = -1;
    for (int i = 0; i < 26; i ++ )
        if (tr[0][i])
            q[ ++ tt] = tr[0][i];

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = 0; i < 26; i ++ )
        {
            int &p = tr[t][i];
            if (!p) p = tr[ne[t]][i];
            else
            {
                ne[p] = tr[ne[t]][i];
                q[ ++ tt] = p;
            }
        }
    }
}

int main()
{
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ )
    {
        scanf("%s", str);
        insert(i);
    }

    build();

    for (int i = idx - 1; i >= 0; i -- ) f[ne[q[i]]] += f[q[i]];

    for (int i = 0; i < n; i ++ ) printf("%d\n", f[id[i]]);

    return 0;
}
```

# 第五章 数学知识

##### AcWing1292 哥德巴赫猜想
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>


using namespace std;

const int N = 1000010;

int primes[N], cnt;
bool st[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    init(N - 1);

    int n;
    while (cin >> n, n)
    {
        for (int i = 1; ; i ++ )
        {
            int a = primes[i];
            int b = n - a;
            if (!st[b])
            {
                printf("%d = %d + %d\n", n, a, b);
                break;
            }
        }
    }

    return 0;
}

```
##### AcWing1293 夏洛克和他的女朋友
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int primes[N], cnt;
bool st[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int n;
    cin >> n;

    init(n + 1);

    if (n <= 2) puts("1");
    else puts("2");

    for (int i = 2; i <= n + 1; i ++ )
        if (!st[i]) printf("1 ");
        else printf("2 ");

    return 0;
}

```
##### AcWing196	质数距离
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1000010;

int primes[N], cnt;
bool st[N];

void init(int n)
{
    memset(st, 0, sizeof st);
    cnt = 0;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int l, r;
    while (cin >> l >> r)
    {
        init(50000);

        memset(st, 0, sizeof st);
        for (int i = 0; i < cnt; i ++ )
        {
            LL p = primes[i];
            for (LL j = max(p * 2, (l + p - 1) / p * p); j <= r; j += p)
                st[j - l] = true;
        }

        cnt = 0;
        for (int i = 0; i <= r - l; i ++ )
            if (!st[i] && i + l >= 2)
                primes[cnt ++ ] = i + l;

        if (cnt < 2) puts("There are no adjacent primes.");
        else
        {
            int minp = 0, maxp = 0;
            for (int i = 0; i + 1 < cnt; i ++ )
            {
                int d = primes[i + 1] - primes[i];
                if (d < primes[minp + 1] - primes[minp]) minp = i;
                if (d > primes[maxp + 1] - primes[maxp]) maxp = i;
            }

            printf("%d,%d are closest, %d,%d are most distant.\n",
                primes[minp], primes[minp + 1],
                primes[maxp], primes[maxp + 1]);
        }
    }

    return 0;
}
```
##### AcWing197	阶乘分解
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int primes[N], cnt;
bool st[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int n;
    cin >> n;
    init(n);

    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        int s = 0;
        for (int j = n; j; j /= p) s += j / p;
        printf("%d %d\n", p, s);
    }

    return 0;
}
```
##### AcWing1289 序列的第k个数
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int mod = 200907;

int qmi(int a, int k)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % mod;
        a = (LL)a * a % mod;
        k >>= 1;
    }
    return res;
}

int main()
{
    int n;
    cin >> n;
    while (n -- )
    {
        int a, b, c, k;
        cin >> a >> b >> c >> k;
        if (a + c == b * 2) cout << (a + (b - a) * (LL)(k - 1)) % mod << endl;
        else cout << (LL)a * qmi(b / a, k - 1) % mod << endl;
    }

    return 0;
}
```
##### AcWing1290 越狱
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int mod = 100003;

int qmi(int a, LL k)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % mod;
        a = (LL)a * a % mod;
        k >>= 1;
    }
    return res;
}

int main()
{
    int m;
    LL n;
    cin >> m >> n;

    cout << (qmi(m, n) - (LL)m * qmi(m - 1, n - 1) % mod + mod) % mod << endl;

    return 0;
}

```
##### AcWing1291 轻拍牛头
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int n;
int a[N], cnt[N], s[N];

int main()
{
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ )
    {
        scanf("%d", &a[i]);
        cnt[a[i]] ++ ;
    }

    for (int i = 1; i < N; i ++ )
        for (int j = i; j < N; j += i)
            s[j] += cnt[i];

    for (int i = 0; i < n; i ++ ) printf("%d\n", s[a[i]] - 1);

    return 0;
}
```
##### AcWing1294 樱花
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1e6 + 10, mod = 1e9 + 7;

int primes[N], cnt;
bool st[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int n;
    cin >> n;

    init(n);

    int res = 1;
    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        int s = 0;
        for (int j = n; j; j /= p) s += j / p;
        res = (LL)res * (2 * s + 1) % mod;
    }

    cout << res << endl;


    return 0;
}
```
##### AcWing198	反素数
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int primes[9] = {2, 3, 5, 7, 11, 13, 17, 19, 23};
int maxd, number;
int n;

void dfs(int u, int last, int p, int s)
{
    if (s > maxd || s == maxd && p < number)
    {
        maxd = s;
        number = p;
    }

    if (u == 9) return;

    for (int i = 1; i <= last; i ++ )
    {
        if ((LL)p * primes[u] > n) break;
        p *= primes[u];
        dfs(u + 1, i, p, s * (i + 1));
    }
}

int main()
{
    cin >> n;

    dfs(0, 30, 1, 1);

    cout << number << endl;

    return 0;
}
```
##### AcWing200	Hankson的趣味题
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 50010;

int primes[N], cnt;
bool st[N];
struct Factor
{
    int p, s;
}factor[10];
int fcnt;

int dividor[1601], dcnt;

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

void dfs(int u, int p)
{
    if (u == fcnt)
    {
        dividor[dcnt ++ ] = p;
        return;
    }

    for (int i = 0; i <= factor[u].s; i ++ )
    {
        dfs(u + 1, p);
        p *= factor[u].p;
    }
}

int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

int main()
{
    init(N - 1);

    int n;
    cin >> n;
    while (n -- )
    {
        int a, b, c, d;
        cin >> a >> b >> c >> d;

        fcnt = 0;
        int t = d;
        for (int i = 0; primes[i] <= t / primes[i]; i ++ )
        {
            int p = primes[i];
            if (t % p == 0)
            {
                int s = 0;
                while (t % p == 0) t /= p, s ++ ;
                factor[fcnt ++ ] = {p, s};
            }
        }

        if (t > 1) factor[fcnt ++ ] = {t, 1};

        dcnt = 0;
        dfs(0, 1);

        int res = 0;
        for (int i = 0; i < dcnt; i ++ )
        {
            int x = dividor[i];
            if (gcd(a, x) == b && (LL)c * x / gcd(c, x) == d) res ++ ;
        }

        cout << res << endl;
    }

    return 0;
}

```
##### AcWing201	可见的点
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int primes[N], cnt;
bool st[N];
int phi[N];

void init(int n)
{
    phi[1] = 1;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            phi[i] = i - 1;
        }
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0)
            {
                phi[i * primes[j]] = phi[i] * primes[j];
                break;
            }
            phi[i * primes[j]] = phi[i] * (primes[j] - 1);
        }
    }
}

int main()
{
    init(N - 1);

    int n, m;
    cin >> m;
    for (int T = 1; T <= m; T ++ )
    {
        cin >> n;
        int res = 1;
        for (int i = 1; i <= n; i ++ ) res += phi[i] * 2;
        cout << T << ' ' << n << ' ' << res << endl;
    }

    return 0;
}
```
##### AcWing220	最大公约数
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1e7 + 10;

int primes[N], cnt;
bool st[N];
int phi[N];
LL s[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            phi[i] = i - 1;
        }
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0)
            {
                phi[i * primes[j]] = phi[i] * primes[j];
                break;
            }
            phi[i * primes[j]] = phi[i] * (primes[j] - 1);
        }
    }

    for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + phi[i];
}

int main()
{
    int n;
    cin >> n;
    init(n);

    LL res = 0;
    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        res += s[n / p] * 2 + 1;
    }

    cout << res << endl;

    return 0;
}
```
##### AcWing203	同余方程
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int exgcd(int a, int b, int &x, int &y)
{
    if (!b)
    {
        x = 1, y = 0;
        return a;
    }

    int d = exgcd(b, a % b, y, x);
    y -= a / b * x;

    return d;
}

int main()
{
    int a, b;
    cin >> a >> b;

    int x, y;
    exgcd(a, b, x, y);

    cout << (x % b + b) % b << endl;

    return 0;
}
```
##### AcWing222	青蛙的约会
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

LL exgcd(LL a, LL b, LL &x, LL &y)
{
    if (!b)
    {
        x = 1, y = 0;
        return a;
    }

    LL d = exgcd(b, a % b, y, x);
    y -= a / b * x;

    return d;
}

int main()
{
    LL a, b, m, n, L;
    cin >> a >> b >> m >> n >> L;

    LL x, y;
    LL d = exgcd(m - n, L, x, y);
    if ((b - a) % d) puts("Impossible");
    else
    {
        x *= (b - a) / d;
        LL t = abs(L / d);
        cout << (x % t + t) % t << endl;
    }

    return 0;
}
```
##### AcWing202	最幸运的数字
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

LL qmul(LL a, LL k, LL b)
{
    LL res = 0;
    while (k)
    {
        if (k & 1) res = (res + a) % b;
        a = (a + a) % b;
        k >>= 1;
    }
    return res;
}

LL qmi(LL a, LL k, LL b)
{
    LL res = 1;
    while (k)
    {
        if (k & 1) res = qmul(res, a, b);
        a = qmul(a, a, b);
        k >>= 1;
    }
    return res;
}

LL get_euler(LL C)
{
    LL res = C;
    for (LL i = 2; i <= C / i; i ++ )
        if (C % i == 0)
        {
            while (C % i == 0) C /= i;
            res = res / i * (i - 1);
        }
    if (C > 1) res = res / C * (C - 1);
    return res;
}

int main()
{
    int T = 1;
    LL L;
    while (cin >> L, L)
    {
        int d = 1;
        while (L % (d * 2) == 0 && d * 2 <= 8) d *= 2;

        LL C = 9 * L / d;

        LL phi = get_euler(C);

        LL res = 1e18;
        if (C % 2 == 0 || C % 5 == 0) res = 0;
        else
        {
            for (LL d = 1; d * d <= phi; d ++ )
                if (phi % d == 0)
                {
                    if (qmi(10, d, C) == 1) res = min(res, d);
                    if (qmi(10, phi / d, C) == 1) res = min(res, phi / d);
                }
        }

        printf("Case %d: %lld\n", T ++, res);
    }

    return 0;
}
```
##### AcWing1298 曹冲养猪
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 10;

int n;
int A[N], B[N];

void exgcd(LL a, LL b, LL &x, LL &y)
{
    if (!b) x = 1, y = 0;
    else
    {
        exgcd(b, a % b, y, x);
        y -= a / b * x;
    }
}

int main()
{
    scanf("%d", &n);

    LL M = 1;
    for (int i = 0; i < n; i ++ )
    {
        scanf("%d%d", &A[i], &B[i]);
        M *= A[i];
    }

    LL res = 0;
    for (int i = 0; i < n; i ++ )
    {
        LL Mi = M / A[i];
        LL ti, x;
        exgcd(Mi, A[i], ti, x);
        res += B[i] * Mi * ti;
    }

    cout << (res % M + M) % M << endl;

    return 0;
}
```
##### AcWing1303 斐波那契前n项和
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 3;

int n, m;

void mul(int c[], int a[], int b[][N])
{
    int temp[N] = {0};
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            temp[i] = (temp[i] + (LL)a[j] * b[j][i]) % m;

    memcpy(c, temp, sizeof temp);
}

void mul(int c[][N], int a[][N], int b[][N])
{
    int temp[N][N] = {0};
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            for (int k = 0; k < N; k ++ )
                temp[i][j] = (temp[i][j] + (LL)a[i][k] * b[k][j]) % m;

    memcpy(c, temp, sizeof temp);
}

int main()
{
    cin >> n >> m;

    int f1[N] = {1, 1, 1};
    int a[N][N] = {
        {0, 1, 0},
        {1, 1, 1},
        {0, 0, 1}
    };

    n -- ;
    while (n)
    {
        if (n & 1) mul(f1, f1, a);  // res = res * a
        mul(a, a, a);  // a = a * a
        n >>= 1;
    }

    cout << f1[2] << endl;

    return 0;
}
```
##### AcWing1304 佳佳的斐波那契
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 4;

int n, m;

void mul(int c[][N], int a[][N], int b[][N])  // c = a * b
{
    static int t[N][N];
    memset(t, 0, sizeof t);

    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            for (int k = 0; k < N; k ++ )
                t[i][j] = (t[i][j] + (LL)a[i][k] * b[k][j]) % m;

    memcpy(c, t, sizeof t);
}

int main()
{
    cin >> n >> m;

    // {fn, fn+1, sn, pn}
    // pn = n * sn - tn
    int f1[N][N] = {1, 1, 1, 0};
    int a[N][N] = {
        {0, 1, 0, 0},
        {1, 1, 1, 0},
        {0, 0, 1, 1},
        {0, 0, 0, 1},
    };

    int k = n - 1;

    // 快速幂
    while (k)
    {
        if (k & 1) mul(f1, f1, a);  // f1 = f1 * a
        mul(a, a, a);  // a = a * a
        k >>= 1;
    }

    cout << (((LL)n * f1[0][2] - f1[0][3]) % m + m) % m << endl;

    return 0;
}
```
##### AcWing1305 GT考试
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 25;

int n, m, mod;
char str[N];
int ne[N];
int a[N][N];

void mul(int c[][N], int a[][N], int b[][N])  // c = a * b
{
    static int t[N][N];
    memset(t, 0, sizeof t);

    for (int i = 0; i < m; i ++ )
        for (int j = 0; j < m; j ++ )
            for (int k = 0; k < m; k ++ )
                t[i][j] = (t[i][j] + a[i][k] * b[k][j]) % mod;

    memcpy(c, t, sizeof t);
}

int qmi(int k)
{
    int f0[N][N] = {1};
    while (k)
    {
        if (k & 1) mul(f0, f0, a);  // f0 = f0 * a
        mul(a, a, a);  // a = a * a
        k >>= 1;
    }

    int res = 0;
    for (int i = 0; i < m; i ++ ) res = (res + f0[0][i]) % mod;
    return res;
}

int main()
{
    cin >> n >> m >> mod;
    cin >> str + 1;

    // kmp
    for (int i = 2, j = 0; i <= m; i ++ )
    {
        while (j && str[j + 1] != str[i]) j = ne[j];
        if (str[j + 1] == str[i]) j ++ ;
        ne[i] = j;
    }

    // 初始化A[i][j]
    for (int j = 0; j < m; j ++ )
        for (int c = '0'; c <= '9'; c ++ )
        {
            int k = j;
            while (k && str[k + 1] != c) k = ne[k];
            if (str[k + 1] == c) k ++ ;
            if (k < m) a[j][k] ++ ;
        }


    // F[n] = F[0] * A^n
    cout << qmi(n) << endl;

    return 0;
}
```
##### AcWing1307 牡牛和牝牛
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, mod = 5000011;

int n, k;
int f[N], s[N];

int main()
{
    cin >> n >> k;

    f[0] = s[0] = 1;
    for (int i = 1; i <= n; i ++ )
    {
        f[i] = s[max(i - k - 1, 0)];
        s[i] = (s[i - 1] + f[i]) % mod;
    }

    cout << s[n] << endl;

    return 0;
}

```
##### AcWing1308 方程的解
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 150;

int k, x;
int f[1000][100][N];

int qmi(int a, int b, int p)
{
    int res = 1;
    while (b)
    {
        if (b & 1) res = res * a % p;
        a = a * a % p;
        b >>= 1;
    }
    return res;
}

void add(int c[], int a[], int b[])
{
    for (int i = 0, t = 0; i < N; i ++ )
    {
        t += a[i] + b[i];
        c[i] = t % 10;
        t /= 10;
    }
}

int main()
{
    cin >> k >> x;

    int n = qmi(x % 1000, x, 1000);

    // C(n - 1, k - 1)
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j <= i && j < k; j ++ )
            if (!j) f[i][j][0] = 1;
            else add(f[i][j], f[i - 1][j], f[i - 1][j - 1]);  // f[i][j] = f[i - 1][j] + f[i - 1][j - 1];

    int *g = f[n - 1][k - 1];
    int i = N - 1;
    while (!g[i]) i -- ;
    while (i >= 0) cout << g[i -- ];

    return 0;
}
```
##### AcWing1309 车的放置
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 2010, mod = 100003;

int fact[N], infact[N];

int qmi(int a, int k)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % mod;
        a = (LL)a * a % mod;
        k >>= 1;
    }
    return res;
}

int C(int a, int b)
{
    if (a < b) return 0;
    return (LL)fact[a] * infact[a - b] % mod * infact[b] % mod;
}

int P(int a, int b)
{
    if (a < b) return 0;
    return (LL)fact[a] * infact[a - b] % mod;
}

int main()
{
    fact[0] = infact[0] = 1;
    for (int i = 1; i < N; i ++ )
    {
        fact[i] = (LL)fact[i - 1] * i % mod;
        infact[i] = (LL)infact[i - 1] * qmi(i, mod - 2) % mod;
    }

    int a, b, c, d, k;
    cin >> a >> b >> c >> d >> k;

    int res = 0;
    for (int i = 0; i <= k; i ++ )
    {
        res = (res + (LL)C(b, i) * P(a, i) % mod * C(d, k - i) % mod * P(a + c - i, k - i)) % mod;
    }

    cout << res << endl;

    return 0;
}
```
##### AcWing1310 数三角形
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

LL C(int n)
{
    return (LL)n * (n - 1) * (n - 2) / 6;
}

int main()
{
    int n, m;
    cin >> n >> m;

    n ++, m ++ ;

    LL res = C(n * m) - (LL)n * C(m) - (LL)m * C(n);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            res -= 2ll * (gcd(i, j) - 1) * (n - i) * (m - j);

    cout << res << endl;

    return 0;
}

```
##### AcWing1312 序列统计
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int p = 1000003;

int qmi(int a, int k)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

int C(int a, int b)
{
    if (a < b) return 0;

    int down = 1, up = 1;
    for (int i = a, j = 1; j <= b; i --, j ++ )
    {
        up = (LL)up * i % p;
        down = (LL)down * j % p;
    }

    return (LL)up * qmi(down, p - 2) % p;
}

int Lucas(int a, int b)
{
    if (a < p && b < p) return C(a, b);
    return (LL)Lucas(a / p, b / p) * C(a % p, b % p) % p;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        int n, l, r;
        cin >> n >> l >> r;
        cout << (Lucas(r - l + n + 1, r - l + 1) + p - 1) % p << endl;
    }

    return 0;
}

```
##### AcWing1315 网格
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int primes[N], cnt;
bool st[N];
int a[N], b[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int get(int n, int p)
{
    int s = 0;
    while (n) s += n / p, n /= p;
    return s;
}

void mul(int r[], int &len, int x)
{
    int t = 0;
    for (int i = 0; i < len; i ++ )
    {
        t += r[i] * x;
        r[i] = t % 10;
        t /= 10;
    }
    while (t)
    {
        r[len ++ ] = t % 10;
        t /= 10;
    }
}

void sub(int a[], int al, int b[], int bl)
{
    for (int i = 0, t = 0; i < al; i ++ )
    {
        a[i] -= t + b[i];
        if (a[i] < 0) a[i] += 10, t = 1;
        else t = 0;
    }
}

int C(int x, int y, int r[N])
{
    int len = 1;
    r[0] = 1;

    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        int s = get(x, p) - get(y, p) - get(x - y, p);
        while (s -- ) mul(r, len, p);
    }

    return len;
}

int main()
{
    init(N - 1);

    int n, m;
    cin >> n >> m;
    int al = C(n + m, m, a);
    int bl = C(n + m, n + 1, b);

    sub(a, al, b, bl);

    int k = al - 1;
    while (!a[k] && k > 0) k -- ;
    while (k >= 0) printf("%d", a[k -- ]);

    return 0;
}

```
##### AcWing1316 有趣的数列
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 2000010;

int n, p;
int primes[N], cnt;
bool st[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if  (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int qmi(int a, int k)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

int get(int n, int p)
{
    int s = 0;
    while (n)
    {
        s += n / p;
        n /= p;
    }
    return s;
}

int C(int a, int b)
{
    int res = 1;
    for (int i = 0; i < cnt; i ++ )
    {
        int prime = primes[i];
        int s = get(a, prime) - get(b, prime) - get(a - b, prime);

        res = (LL)res * qmi(prime, s) % p;
    }

    return res;
}

int main()
{
    scanf("%d%d", &n, &p);
    init(n * 2);

    cout << (C(n * 2, n) - C(n * 2, n - 1) + p) % p << endl;

    return 0;
}

```
##### AcWing207	球形空间产生器
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

const int N = 15;

int n;
double a[N][N], b[N][N];

void gauss()
{
    // 转化成上三角矩阵
    for (int r = 1, c = 1; c <= n; c ++, r ++ )
    {
        // 找主元
        int t = r;
        for (int i = r + 1; i <= n; i ++ )
            if (fabs(b[i][c]) > fabs(b[t][c]))
                t = i;

        // 交换
        for (int i = c; i <= n + 1; i ++ ) swap(b[t][i], b[r][i]);
        // 归一化
        for (int i = n + 1; i >= c; i -- ) b[r][i] /= b[r][c];
        // 消
        for (int i = r + 1; i <= n; i ++ )
            for (int j = n + 1; j >= c; j -- )
                b[i][j] -= b[i][c] * b[r][j];
    }

    // 转化成对角矩阵
    for (int i = n; i > 1; i -- )
        for (int j = i - 1; j; j -- )
        {
            b[j][n + 1] -= b[i][n + 1] * b[j][i];
            b[j][i] = 0;
        }
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n + 1; i ++ )
        for (int j = 1; j <= n; j ++ )
            scanf("%lf", &a[i][j]);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
        {
            b[i][j] += 2 * (a[i][j] - a[0][j]);
            b[i][n + 1] += a[i][j] * a[i][j] - a[0][j] * a[0][j];
        }

    gauss();

    for (int i = 1; i <= n; i ++ ) printf("%.3lf ", b[i][n + 1]);

    return 0;
}
```
##### AcWing208	开关问题
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 35;

int n;
int a[N][N];

int gauss()
{
    int r, c;
    for (r = 1, c = 1; c <= n; c ++ )
    {
        // 找主元
        int t = r;
        for (int i = r + 1; i <= n; i ++ )
            if (a[i][c])
                t = i;

        if (!a[t][c]) continue;
        // 交换
        for (int i = c; i <= n + 1; i ++ ) swap(a[t][i], a[r][i]);
        // 消
        for (int i = r + 1; i <= n; i ++ )
            for (int j = n + 1; j >= c; j -- )
                a[i][j] ^= a[i][c] & a[r][j];
        r ++ ;
    }

    int res = 1;
    if (r < n + 1)
    {
        for (int i = r; i <= n; i ++ )
        {
            if (a[i][n + 1]) return -1;  // 出现了 0 == !0，无解
            res *= 2;
        }
    }

    return res;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        memset(a, 0, sizeof a);
        scanf("%d", &n);
        for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i][n + 1]);
        for (int i = 1; i <= n; i ++ )
        {
            int t;
            scanf("%d", &t);
            a[i][n + 1] ^= t;
            a[i][i] = 1;
        }

        int x, y;
        while (scanf("%d%d", &x, &y), x || y) a[y][x] = 1;

        int t = gauss();
        if (t == -1) puts("Oh,it's impossible~!!");
        else printf("%d\n", t);
    }

    return 0;
}
```
##### AcWing214	Devu和鲜花
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 20, mod = 1e9 + 7;

LL A[N];
int down = 1;

int qmi(int a, int k, int p)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

int C(LL a, LL b)
{
    if (a < b) return 0;
    int up = 1;
    for (LL i = a; i > a - b; i -- ) up = i % mod * up % mod;

    return (LL)up * down % mod; // 费马小定理
}

int main()
{
    LL n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i ++ ) cin >> A[i];

    for (int j = 1; j <= n - 1; j ++ ) down = (LL)j * down % mod;
    down = qmi(down, mod - 2, mod);

    int res = 0;
    for (int i = 0; i < 1 << n; i ++ )
    {
        LL a = m + n - 1, b = n - 1;
        int sign = 1;
        for (int j = 0; j < n; j ++ )
            if (i >> j & 1)
            {
                sign *= -1;
                a -= A[j] + 1;
            }
        res = (res + C(a, b) * sign) % mod;
    }

    cout << (res + mod) % mod << endl;

    return 0;
}
```
##### AcWing215	破译密码
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 50010;

int primes[N], cnt;
bool st[N];
int mobius[N], sum[N];

// 线性筛法，求莫比乌斯函数
void init(int n)
{
    mobius[1] = 1;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            mobius[i] = -1;
        }
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0)
            {
                mobius[t] = 0;
                break;
            }
            mobius[t] = mobius[i] * -1;
        }
    }

    for (int i = 1; i <= n; i ++ ) sum[i] = sum[i - 1] + mobius[i];
}

int main()
{
    init(N - 1);

    int T;
    scanf("%d", &T);
    while (T -- )
    {
        int a, b, d;
        scanf("%d%d%d", &a, &b, &d);
        a /= d, b /= d;
        int n = min(a, b);
        LL res = 0;
        for (int l = 1, r; l <= n; l = r + 1)
        {
            r = min(n, min(a / (a / l), b / (b / l)));
            res += (sum[r] - sum[l - 1]) * (LL)(a / l) * (b / l);
        }

        printf("%lld\n", res);
    }

    return 0;
}

```
##### AcWing217	绿豆蛙的归宿
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 200010;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int dout[N];
double f[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

double dp(int u)
{
    if (f[u] >= 0) return f[u];
    f[u] = 0;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        f[u] += (w[i] + dp(j)) / dout[u];
    }
    return f[u];
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
        dout[a] ++ ;
    }

    memset(f, -1, sizeof f);

    printf("%.2lf\n", dp(1));

    return 0;
}

```
##### AcWing218	扑克牌
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 14;
const double INF = 1e20;

int A, B, C, D;
double f[N][N][N][N][5][5];

double dp(int a, int b, int c, int d, int x, int y)
{
    double &v = f[a][b][c][d][x][y];
    if (v >= 0) return v;
    int as = a + (x == 0) + (y == 0);
    int bs = b + (x == 1) + (y == 1);
    int cs = c + (x == 2) + (y == 2);
    int ds = d + (x == 3) + (y == 3);
    if (as >= A && bs >= B && cs >= C && ds >= D) return v = 0;

    int sum = a + b + c + d + (x != 4) + (y != 4);
    sum = 54 - sum;
    if (sum <= 0) return v = INF;

    v = 1;
    if (a < 13) v += (13.0 - a) / sum * dp(a + 1, b, c, d, x, y);
    if (b < 13) v += (13.0 - b) / sum * dp(a, b + 1, c, d, x, y);
    if (c < 13) v += (13.0 - c) / sum * dp(a, b, c + 1, d, x, y);
    if (d < 13) v += (13.0 - d) / sum * dp(a, b, c, d + 1, x, y);
    if (x == 4)
    {
        double t = INF;
        for (int i = 0; i < 4; i ++ ) t = min(t, 1.0 / sum * dp(a, b, c, d, i, y));
        v += t;
    }
    if (y == 4)
    {
        double t = INF;
        for (int i = 0; i < 4; i ++ ) t = min(t, 1.0 / sum * dp(a, b, c, d, x, i));
        v += t;
    }

    return v;
}

int main()
{
    cin >> A >> B >> C >> D;
    memset(f, -1, sizeof f);

    double t = dp(0, 0, 0, 0, 4, 4);
    if (t > INF / 2) t = -1;

    printf("%.3lf\n", t);

    return 0;
}
```
##### AcWing1319 移棋子游戏
```cpp
#include <cstdio>
#include <cstring>
#include <set>

using namespace std;

const int N = 2010, M = 6010;

int n, m, k;
int h[N], e[M], ne[M], idx;
int f[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

int sg(int u)
{
    if (f[u] != -1) return f[u];

    set<int> S;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        S.insert(sg(j));
    }

    for (int i = 0; ; i ++ )
        if (S.count(i) == 0)
        {
            f[u] = i;
            break;
        }

    return f[u];
}

int main()
{
    scanf("%d%d%d", &n, &m, &k);
    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b);
    }

    memset(f, -1, sizeof f);

    int res = 0;
    for (int i = 0; i < k; i ++ )
    {
        int u;
        scanf("%d", &u);
        res ^= sg(u);
    }

    if (res) puts("win");
    else puts("lose");

    return 0;
}
```
##### AcWing1321 取石子
```cpp
#include <cstdio>
#include <cstring>

using namespace std;

const int N = 55, M = 50050;

int f[N][M];

int dp(int a, int b)
{
    int &v = f[a][b];
    if (v != -1) return v;
    if (!a) return v = b % 2;
    if (b == 1) return dp(a + 1, 0);

    if (a && !dp(a - 1, b)) return v = 1;
    if (b && !dp(a, b - 1)) return v = 1;
    if (a >= 2 && !dp(a - 2, b + (b ? 3 : 2))) return v = 1;
    if (a && b && !dp(a - 1, b + 1)) return v = 1;

    return v = 0;
}

int main()
{
    memset(f, -1, sizeof f);

    int T;
    scanf("%d", &T);
    while (T -- )
    {
        int n;
        scanf("%d", &n);
        int a = 0, b = 0;
        for (int i = 0; i < n; i ++ )
        {
            int x;
            scanf("%d", &x);
            if (x == 1) a ++ ;
            else b += b ? x + 1 : x;
        }

        if (dp(a, b)) puts("YES");
        else puts("NO");
    }

    return 0;
}

```
##### AcWing1322 取石子游戏
```cpp
#include <cstdio>

using namespace std;

const int N = 1010;

int n;
int a[N];
int l[N][N], r[N][N];

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d", &n);
        for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

        for (int len = 1; len <= n; len ++ )
            for (int i = 1; i + len - 1 <= n; i ++ )
            {
                int j = i + len - 1;
                if (len == 1) l[i][j] = r[i][j] = a[i];
                else
                {
                    int L = l[i][j - 1], R = r[i][j - 1], X = a[j];
                    if (R == X) l[i][j] = 0;
                    else if (X < L && X < R || X > L && X > R) l[i][j] = X;
                    else if (L > R) l[i][j] = X - 1;
                    else l[i][j] = X + 1;

                    L = l[i + 1][j], R = r[i + 1][j], X = a[i];
                    if (L == X) r[i][j] = 0;
                    else if (X < L && X < R || X > L && X > R) r[i][j] = X;
                    else if (R > L) r[i][j] = X - 1;
                    else r[i][j] = X + 1;
                }
            }

        if (n == 1) puts("1");
        else printf("%d\n", l[2][n] != a[1]);
    }

    return 0;
}
```