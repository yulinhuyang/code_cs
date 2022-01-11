##### AcWing339   圆形数字  

# 0x60 图论完成情况(73)

包括最短路、最小生成树、树的直径与最近公共祖先、基环树、负环与差分约束、Tarjan算法与无向图连通性、Tarjan算法与有向图连通性、二分图的匹配、二分图的覆盖与独立集、网络流初步

## 0x61 最短路

     
  
##### AcWing340   通信线路  
##### AcWing341   最优贸易  
##### AcWing342   道路与航线  
##### AcWing343   排序  
##### AcWing344   观光之旅  
##### AcWing345   牛站  

## 0x62 最小生成树

     
  
##### AcWing346   走廊泼水节
```cpp
#include <cstdio>
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
        for (int i = 0; i < n - 1; i ++ ) cin >> e[i].a >> e[i].b >> e[i].w;
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

  
##### AcWing347   野餐规划  
##### AcWing348   沙漠之王  
##### AcWing349   黑暗城堡  

## 0x63 树的直径与最近公共祖先

     
  
##### AcWing350   巡逻  
##### AcWing351   树网的核  
##### AcWing352   闇の連鎖  
##### AcWing353   雨天的尾巴  
##### AcWing354   天天爱跑步  
##### AcWing355   异象石  
##### AcWing356   次小生成树  
##### AcWing357   疫情控制  

## 0x64 基环树

     
  
##### AcWing358   岛屿  
##### AcWing359   创世纪  
##### AcWing360   Freda的传呼机  

## 0x65  负环与差分约束

##### AcWing361   观光奶牛
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 1010, M = 5010;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int f[N], cnt[N];
double dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool spfa(double mid)
{
    for (int i = 1; i <= n; i ++ )
    {
        dist[i] = 0;
        cnt[i] = 0;
        st[i] = false;
    }
    queue<int> q;

    for (int i = 1; i <= n; i ++ )
    {
        q.push(i);
        st[i] = true;
    }

    while (q.size())
    {
        int t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i] * mid - f[j])
            {
                dist[j] = dist[t] + w[i] * mid - f[j];
                cnt[j] = cnt[t] + 1;

                if (cnt[j] >= n) return true;

                if (!st[j])
                {
                    q.push(j);
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

    for (int i = 1; i <= n; i ++ ) cin >> f[i];
    memset(h, -1, sizeof h);

    while (m -- )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }

    double l = 0, r = 1e9;
    while (r - l > 1e-4)
    {
        double mid = (l + r) / 2;
        if (spfa(mid)) l = mid;
        else r = mid;
    }

    printf("%.2lf\n", r);

    return 0;
}
```
##### AcWing362   区间
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 50010, M = N * 3;

int n;
int h[N], w[M], e[M], ne[M], idx;
int dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void spfa()
{
    queue<int> q;

    for (int i = 0; i <= 50001; i ++ )
    {
        q.push(i);
        st[i] = true;
    }

    while (q.size())
    {
        int t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] < dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                if (!st[j])
                {
                    q.push(j);
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
    while (n -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        a ++, b ++;
        add(a - 1, b, c);
    }

    for (int i = 1; i <= 50001; i ++ )
    {
        add(i - 1, i, 0);
        add(i, i - 1, -1);
    }

    spfa();

    printf("%d\n", dist[50001]);

    return 0;
}
```

## 0x66 Tarjan算法与无向图连通性

     
  
##### AcWing363   B城  
##### AcWing364   网络  
##### AcWing365   圆桌骑士  
##### AcWing366   看牛       
     
## 0x67 Tarjan算法与有向图连通性

##### AcWing367   学校网络
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = 10010;

int n, m;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], cnt;
int stk[N], tt, id[N], num;
int din[N], dout[N];
bool in_stk[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void tarjan(int u)
{
    dfn[u] = low[u] = ++ cnt;
    stk[ ++ tt] = u, in_stk[u] = true;
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
        int y;
        ++ num;
        do
        {
            y = stk[tt -- ];
            in_stk[y] = false;
            id[y] = num;
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
        for (int j = h[i]; ~j; j = ne[j])
        {
            int k = e[j];
            if (id[i] != id[k])
                dout[id[i]] ++ , din[id[k]] ++ ;
        }

    int a = 0, b = 0;
    for (int i = 1; i <= num; i ++ )
    {
        if (!din[i]) a ++ ;
        if (!dout[i]) b ++ ;
    }

    cout << a << endl;
    if (num == 1) cout << 0 << endl;
    else cout << max(a, b) << endl;

    return 0;
}

```

##### AcWing368   银河
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010, M = 600010;

int n, m;
int h[N], hs[N], e[M], w[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
bool in_stk[N];
int id[N], size[N], cnt;
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
        int y;
        ++ cnt;
        do {
            y = stk[top -- ];
            in_stk[y] = false;
            id[y] = cnt;
            size[cnt] ++ ;
        } while (y != u);
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
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
    memset(hs, -1, sizeof hs);
    for (int i = 0; i <= n; i ++ )
    {
        for (int j = h[i]; ~j; j = ne[j])
        {
            int k = e[j];
            if (id[i] == id[k])
            {
                if (w[j] > 0)
                {
                    success = false;
                    break;
                }
            }
            else
            {
                add(hs, id[i], id[k], w[j]);
            }
        }
        if (!success) break;
    }

    if (!success) puts("-1");
    else
    {
        for (int i = cnt; i; i -- )
            for (int j = hs[i]; ~j; j = ne[j])
            {
                int k = e[j];
                dist[k] = max(dist[k], dist[i] + w[j]);
            }

        LL res = 0;
        for (int i = 1; i <= cnt; i ++ ) res += (LL)dist[i] * size[i];

        printf("%lld\n", res);
    }

    return 0;
}

```
##### AcWing369   北大ACM队的远足  
##### AcWing370   卡图难题  
##### AcWing371   牧师约翰最忙碌的一天  

## 0x68 二分图的匹配

##### AcWing372   棋盘覆盖
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

const int N = 110;

int n, m;
int g[N][N];
PII ret[N][N];
bool st[N][N];

int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

bool find(PII t)
{
    for (int i = 0; i < 4; i ++ )
    {
        int a = t.first + dx[i], b = t.second + dy[i];

        if (!a || a > n || !b || b > n || g[a][b] || st[a][b]) continue;

        st[a][b] = true;

        if (ret[a][b].first == 0 || find(ret[a][b]))
        {
            ret[a][b] = t;
            return true;
        }
    }

    return false;
}

int main()
{
    cin >> n >> m;

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        cin >> a >> b;
        g[a][b] = 1;
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
        {
            if ((i + j) % 2 == 0 || g[i][j]) continue;
            memset(st, false, sizeof st);

            if (find({i, j})) res ++ ;
        }

    cout << res << endl;

    return 0;
}
```
##### AcWing373   車的放置
```cpp


```  
##### AcWing374   导弹防御塔
```cpp


```  
##### AcWing375   蚂蚁           

## 0x69 二分图的覆盖与独立集

     
  
##### AcWing376   机器任务
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

  
##### AcWing377   泥泞的区域  
##### AcWing378   骑士放置
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
##### AcWing379   捉迷藏
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 210;

int n, m;
int match[N];
bool d[N][N], st[N];

bool find(int x)
{
    for (int i = 1; i <= n; i ++ )
        if (!st[i] && d[x][i])
        {
            st[i] = true;
            if (match[i] == 0 || find(match[i]))
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

    cout << n - res << endl;

    return 0;
}
```
       

## 0x6A 网络流初步
  
##### AcWing380   舞动的夜晚
  
##### AcWing381   有线电视网络
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110, M = 5210, INF = 1e8;

int n, m, S, T;
int h[N], e[M], f[M], ne[M], idx;
int q[N], d[N], cur[N];
bool g[N][N];

void add(int a, int b, int c)
{
    e[idx] = b, f[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
    e[idx] = a, f[idx] = 0, ne[idx] = h[b], h[b] = idx ++ ;
}

void build()
{
    memset(h, -1, sizeof h);
    idx = 0;
    for (int i = 1; i <= n; i ++ ) add(i, n + i, 1);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j < i; j ++ )
            if (g[i][j])
            {
                add(n + i, j, INF);
                add(n + j, i, INF);
            }
}

bool bfs()
{
    int hh = 0, tt = 0;
    memset(d, -1, sizeof d);
    q[0] = S, d[S] = 0, cur[S] = h[S];
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int ver = e[i];
            if (d[ver] == -1 && f[i])
            {
                d[ver] = d[t] + 1;
                cur[ver] = h[ver];
                if (ver == T) return true;
                q[ ++ tt] = ver;
            }
        }
    }
    return false;
}

int find(int u, int limit)
{
    if (u == T) return limit;
    int flow = 0;
    for (int i = cur[u]; ~i && flow < limit; i = ne[i])
    {
        cur[u] = i;
        int ver = e[i];
        if (d[ver] == d[u] + 1 && f[i])
        {
            int t = find(ver, min(f[i], limit - flow));
            if (!t) d[ver] = -1;
            f[i] -= t, f[i ^ 1] += t, flow += t;
        }
    }
    return flow;
}

int dinic()
{
    int r = 0, flow;
    while (bfs()) while (flow = find(S, INF)) r += flow;
    return r;
}

int main()
{
    while (cin >> n >> m)
    {
        memset(g, 0, sizeof g);
        for (int i = 0; i < m; i ++ )
        {
            int a, b;
            scanf(" (%d,%d)", &a, &b);
            a ++, b ++ ;
            g[a][b] = g[b][a] = true;
        }

        int res = n;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j < i; j ++ )
                if (!g[i][j])
                {
                    build();
                    S = n + i, T = j;
                    res = min(res, dinic());
                }

        printf("%d\n", res);
    }
    return 0;
}
```
  
##### AcWing382   K取方格数  

## 0x6B 总结与练习

##### AcWing383   观光
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
##### AcWing384   升降梯上  
##### AcWing385   GF和猫咪的玩具  
##### AcWing386   社交网络  
##### AcWing387   北极网络  
##### AcWing388   四叶草魔杖  
##### AcWing389   直径  
##### AcWing390   逃学的小孩  
##### AcWing391   聚会  
##### AcWing392   会合  
##### AcWing393   雇佣收银员  
##### AcWing394   最优高铁环  
##### AcWing395   冗余路径
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;


const int N = 5010, M = 20010;

int n, m;
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;
int stk[N], top;
int id[N], scc_cnt;
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
        ++ scc_cnt;
        int y;
        do {
            y = stk[top -- ];
            id[y] = scc_cnt;
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
        add(a, b), add(b, a);
    }

    tarjan(1, -1);

    for (int i = 0; i < idx; i += 2)
        if (is_bridge[i])
        {
            d[id[e[i]]] ++ ;
            d[id[e[i ^ 1]]] ++ ;
        }

    int res = 0;
    for (int i = 1; i <= scc_cnt; i ++ )
        if (d[i] == 1)
            res ++ ;

    printf("%d\n", (res + 1) / 2);

    return 0;
}
```

