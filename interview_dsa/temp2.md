



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

##### AcWing347 野餐规划

```cpp

```

##### AcWing348 沙漠之王

##### AcWing349 黑暗城堡

##### AcWing350 巡逻

