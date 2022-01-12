##### AcWing170 加成序列
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n;
int path[N];

bool dfs(int u, int k)
{
    if (u == k) return path[u - 1] == n;

    bool st[N] = {0};
    for (int i = u - 1; i >= 0; i -- )
        for (int j = i; j >= 0; j -- )
        {
            int s = path[i] + path[j];
            if (s > n || s <= path[u - 1] || st[s]) continue;
            st[s] = true;
            path[u] = s;
            if (dfs(u + 1, k)) return true;
        }

    return false;
}

int main()
{
    path[0] = 1;
    while (cin >> n, n)
    {
        int k = 1;
        while (!dfs(1, k)) k ++ ;

        for (int i = 0; i < k; i ++ ) cout << path[i] << ' ';
        cout << endl;
    }

    return 0;
}
```
##### AcWing171 送礼物
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 46;

int n, m, k;
int w[N];
int weights[1 << 25], cnt = 1;
int ans;

void dfs1(int u, int s)
{
    if (u == k)
    {
        weights[cnt ++ ] = s;
        return;
    }

    dfs1(u + 1, s);
    if ((LL)s + w[u] <= m) dfs1(u + 1, s + w[u]);
}

void dfs2(int u, int s)
{
    if (u >= n)
    {
        int l = 0, r = cnt - 1;
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if ((LL)s + weights[mid] <= m) l = mid;
            else r = mid - 1;
        }
        ans = max(ans, s + weights[l]);
        return;
    }

    dfs2(u + 1, s);
    if ((LL)s + w[u] <= m) dfs2(u + 1, s + w[u]);
}

int main()
{
    cin >> m >> n;
    for (int i = 0; i < n; i ++ ) cin >> w[i];

    sort(w, w + n);
    reverse(w, w + n);

    k = n / 2 + 2;
    dfs1(0, 0);

    sort(weights, weights + cnt);
    cnt = unique(weights, weights + cnt) - weights;

    dfs2(k, 0);

    cout << ans << endl;

    return 0;
}
```
##### AcWing180 排书
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 15;

int n;
int q[N];
int w[5][N];

int f()
{
    int cnt = 0;
    for (int i = 0; i + 1 < n; i ++ )
        if (q[i + 1] != q[i] + 1)
            cnt ++ ;
    return (cnt + 2) / 3;
}

bool check()
{
    for (int i = 0; i + 1 < n; i ++ )
        if (q[i + 1] != q[i] + 1)
            return false;
    return true;
}

bool dfs(int depth, int max_depth)
{
    if (depth + f() > max_depth) return false;
    if (check()) return true;

    for (int len = 1; len <= n; len ++ )
        for (int l = 0; l + len - 1 < n; l ++ )
        {
            int r = l + len - 1;
            for (int k = r + 1; k < n; k ++ )
            {
                memcpy(w[depth], q, sizeof q);
                int x, y;
                for (x = r + 1, y = l; x <= k; x ++, y ++ ) q[y] = w[depth][x];
                for (x = l; x <= r; x ++, y ++ ) q[y] = w[depth][x];
                if (dfs(depth + 1, max_depth)) return true;
                memcpy(q, w[depth], sizeof q);
            }
        }

    return false;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        cin >> n;
        for (int i = 0; i < n; i ++ ) cin >> q[i];

        int depth = 0;
        while (depth < 5 && !dfs(0, depth)) depth ++ ;
        if (depth >= 5) puts("5 or more");
        else cout << depth << endl;
    }

    return 0;
}
```
##### AcWing181 回转游戏
```cpp
/*
      0     1
      2     3
4  5  6  7  8  9  10
      11    12
13 14 15 16 17 18 19
      20    21
      22    23
*/

#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 24;

int op[8][7] = {
    {0, 2, 6, 11, 15, 20, 22},
    {1, 3, 8, 12, 17, 21, 23},
    {10, 9, 8, 7, 6, 5, 4},
    {19, 18, 17, 16, 15, 14, 13},
    {23, 21, 17, 12, 8, 3, 1},
    {22, 20, 15, 11, 6, 2, 0},
    {13, 14, 15, 16, 17, 18, 19},
    {4, 5, 6, 7, 8, 9, 10}
};

int oppsite[8] = {5, 4, 7, 6, 1, 0, 3, 2};
int center[8] = {6, 7, 8, 11, 12, 15, 16, 17};

int q[N];
int path[100];

int f()
{
    static int sum[4];
    memset(sum, 0, sizeof sum);
    for (int i = 0; i < 8; i ++ ) sum[q[center[i]]] ++ ;

    int maxv = 0;
    for (int i = 1; i <= 3; i ++ ) maxv = max(maxv, sum[i]);

    return 8 - maxv;
}

void operate(int x)
{
    int t = q[op[x][0]];
    for (int i = 0; i < 6; i ++ ) q[op[x][i]] = q[op[x][i + 1]];
    q[op[x][6]] = t;
}

bool dfs(int depth, int max_depth, int last)
{
    if (depth + f() > max_depth) return false;
    if (f() == 0) return true;

    for (int i = 0; i < 8; i ++ )
        if (last != oppsite[i])
        {
            operate(i);
            path[depth] = i;
            if (dfs(depth + 1, max_depth, i)) return true;
            operate(oppsite[i]);
        }

    return false;
}

int main()
{
    while (cin >> q[0], q[0])
    {
        for (int i = 1; i < 24; i ++ ) cin >> q[i];

        int depth = 0;
        while (!dfs(0, depth, -1)) depth ++ ;

        if (!depth) printf("No moves needed");
        else
        {
            for (int i = 0; i < depth; i ++ ) printf("%c", 'A' + path[i]);
        }
        printf("\n%d\n", q[6]);
    }

    return 0;
}
```
 
第三章 图论
 
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
