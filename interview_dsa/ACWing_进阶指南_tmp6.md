##### AcWing177   噩梦
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 810;

int n, m;
char g[N][N];
int st[N][N];
PII ghost[2];

bool check(int x, int y, int step)
{
    if (x < 0 || x >= n || y < 0 || y >= m || g[x][y] == 'X') return false;

    for (int i = 0; i < 2; i ++ )
        if (abs(x - ghost[i].first) + abs(y - ghost[i].second) <= step * 2)
            return false;

    return true;
}

int bfs()
{
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    memset(st, 0, sizeof st);

    int cnt = 0;
    PII boy, girl;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == 'M') boy = {i, j};
            else if (g[i][j] == 'G') girl = {i, j};
            else if (g[i][j] == 'Z') ghost[cnt ++ ] = {i, j};

    queue<PII> qb, qg;
    qb.push(boy);
    qg.push(girl);

    int step = 0;
    while (qb.size() || qg.size())
    {
        step ++ ;
        for (int i = 0; i < 3; i ++ )
            for (int j = 0, len = qb.size(); j < len; j ++ )
            {
                auto t = qb.front();
                qb.pop();
                int x = t.first, y = t.second;
                if (!check(x, y, step)) continue;
                for (int k = 0; k < 4; k ++ )
                {
                    int a = x + dx[k], b = y + dy[k];
                    if (check(a, b, step))
                    {
                        if (st[a][b] == 2)
                        {
                            return step;
                        }
                        if (!st[a][b])
                        {
                            st[a][b] = 1;
                            qb.push({a, b});
                        }
                    }
                }
            }

        for (int i = 0; i < 1; i ++ )
            for (int j = 0, len = qg.size(); j < len; j ++ )
            {
                auto t = qg.front();
                qg.pop();

                int x = t.first, y = t.second;
                if (!check(x, y, step)) continue;
                for (int k = 0; k < 4; k ++ )
                {
                    int a = x + dx[k], b = y + dy[k];
                    if (check(a, b, step))
                    {
                        if (st[a][b] == 1)
                        {
                            return step;
                        }
                        if (!st[a][b])
                        {
                            st[a][b] = 2;
                            qg.push({a, b});
                        }
                    }
                }
            }
    }

    return -1;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d%d", &n, &m);
        for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);

        printf("%d\n", bfs());
    }

    return 0;
}
```

## 0x27 A*

##### AcWing178   第K短路
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;
typedef pair<int, PII> PIII;

const int N = 1010, M = 200010;

int n, m, S, T, K;
int h[N], rh[N], e[M], w[M], ne[M], idx;
int dist[N], cnt[N];
bool st[N];

void add(int h[], int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void dijkstra()
{
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, T});

    memset(dist, 0x3f, sizeof dist);
    dist[T] = 0;

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.y;
        if (st[ver]) continue;
        st[ver] = true;

        for (int i = rh[ver]; ~i; i = ne[i])
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

int astar()
{
    priority_queue<PIII, vector<PIII>, greater<PIII>> heap;
    heap.push({dist[S], {0, S}});

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.y.y, distance = t.y.x;
        cnt[ver] ++ ;
        if (cnt[T] == K) return distance;

        for (int i = h[ver]; ~i; i = ne[i])
        {
            int j = e[i];
            if (cnt[j] < K)
                heap.push({distance + w[i] + dist[j], {distance + w[i], j}});
        }
    }

    return -1;
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    memset(rh, -1, sizeof rh);

    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(h, a, b, c);
        add(rh, b, a, c);
    }
    scanf("%d%d%d", &S, &T, &K);
    if (S == T) K ++ ;

    dijkstra();
    printf("%d\n", astar());

    return 0;
}
```
##### AcWing179   八数码  
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <queue>

#define x first
#define y second

using namespace std;

typedef pair<int, string> PIS;

int f(string state)
{
    int res = 0;
    for (int i = 0; i < 9; i ++ )
        if (state[i] != 'x')
        {
            int v = state[i] - '1';
            res += abs(v / 3 - i / 3) + abs(v % 3 - i % 3);
        }

    return res;
}

string bfs(string start)
{
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    char op[5] = "urdl";

    string end = "12345678x";
    unordered_map<string, int> dist;
    unordered_map<string, pair<char, string>> pre;
    priority_queue<PIS, vector<PIS>, greater<PIS>> heap;

    heap.push({f(start), start});
    dist[start] = 0;

    while(heap.size())
    {
        auto t = heap.top();
        heap.pop();

        string state = t.y;
        if (state == end) break;

        int x, y;
        for (int i = 0; i < 9; i ++ )
            if (state[i] == 'x')
            {
                x = i / 3, y = i % 3;
                break;
            }

        string source = state;
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a >= 3 || b < 0 || b >= 3) continue;
            state = source;
            swap(state[x * 3 + y], state[a * 3 + b]);
            if (dist.count(state) == 0 || dist[state] > dist[source] + 1)
            {
                dist[state] = dist[source] + 1;
                pre[state] = {op[i], source};
                heap.push({dist[state] + f(state), state});
            }
        }
    }

    string res;
    while (end != start)
    {
        res += pre[end].x;
        end = pre[end].y;
    }
    reverse(res.begin(), res.end());

    return res;
}

int main()
{
    string start, seq;
    char c;
    while (cin >> c)
    {
        start += c;
        if (c != 'x') seq += c;
    }

    int cnt = 0;
    for (int i = 0; i < 8; i ++ )
        for (int j = i + 1; j < 8; j ++ )
            if (seq[i] > seq[j])
                cnt ++ ;

    if (cnt % 2) puts("unsolvable");
    else cout << bfs(start) << endl;

    return 0;
}
```


## 0x28 IDA*

##### AcWing180   排书
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 15;

int n;
int q[N], w[5][N];

int f()
{
    int res = 0;
    for (int i = 0; i + 1 < n; i ++ )
        if (q[i + 1] != q[i] + 1)
            res ++ ;
    return (res + 2) / 3;
}

bool check()
{
    for (int i = 0; i < n; i ++ )
        if (q[i] != i + 1)
            return false;
    return true;
}

bool dfs(int depth, int max_depth)
{
    if (depth + f() > max_depth) return false;
    if (check()) return true;

    for (int l = 0; l < n; l ++ )
        for (int r = l; r < n; r ++ )
            for (int k = r + 1; k < n; k ++ )
            {
                memcpy(w[depth], q, sizeof q);
                int x, y;
                for (x = r + 1, y = l; x <= k; x ++, y ++ ) q[y] = w[depth][x];
                for (x = l; x <= r; x ++, y ++ ) q[y] = w[depth][x];
                if (dfs(depth + 1, max_depth)) return true;
                memcpy(q, w[depth], sizeof q);
            }
    return false;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d", &n);
        for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);

        int depth = 0;
        while (depth < 5 && !dfs(0, depth)) depth ++ ;
        if (depth >= 5) puts("5 or more");
        else printf("%d\n", depth);
    }

    return 0;
}
```
##### AcWing181   回转游戏
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


#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 24;

int q[N];
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
int center[8] = {6, 7, 8, 11, 12, 15, 16, 17};
int opposite[8] = {5, 4, 7, 6, 1, 0, 3, 2};

int path[100];

int f()
{
    static int sum[4];
    memset(sum, 0, sizeof sum);
    for (int i = 0; i < 8; i ++ ) sum[q[center[i]]] ++ ;

    int s = 0;
    for (int i = 1; i <= 3; i ++ ) s = max(s, sum[i]);
    return 8 - s;
}

bool check()
{
    for (int i = 1; i < 8; i ++ )
        if (q[center[i]] != q[center[0]])
            return false;
    return true;
}

void operation(int x)
{
    int t = q[op[x][0]];
    for (int i = 0; i < 6; i ++ ) q[op[x][i]] = q[op[x][i + 1]];
    q[op[x][6]] = t;
}

bool dfs(int depth, int max_depth, int last)
{
    if (depth + f() > max_depth) return false;
    if (check()) return true;

    for (int i = 0; i < 8; i ++ )
    {
        if (opposite[i] == last) continue;
        operation(i);
        path[depth] = i;
        if (dfs(depth + 1, max_depth, i)) return true;
        operation(opposite[i]);
    }

    return false;
}

int main()
{
    while (scanf("%d", &q[0]), q[0])
    {
        for (int i = 1; i < N; i ++ ) scanf("%d", &q[i]);
        int depth = 0;
        while (!dfs(0, depth, -1))
        {
            depth ++ ;
        }
        if (!depth) printf("No moves needed");
        for (int i = 0; i < depth; i ++ ) printf("%c", 'A' + path[i]);
        printf("\n%d\n", q[6]);
    }

    return 0;
}
```
##### AcWing182   破坏正方形
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 61;

int n, m;
bool st[N];
vector<int> square[N];

bool check(vector<int> &sq)
{
    for (auto x : sq)
        if (st[x])
            return false;
    return true;
}

int f()
{
    static bool state[N];
    memcpy(state, st, sizeof st);

    int res = 0;
    for (int i = 0; i < m; i ++ )
    {
        auto &sq = square[i];
        if (check(sq))
        {
            res ++ ;
            for (auto x : sq) st[x] = true;
        }
    }

    memcpy(st, state, sizeof st);

    return res;
}

bool dfs(int depth)
{
    if (f() > depth) return false;

    for (int i = 0; i < m; i ++ )
    {
        auto &sq = square[i];
        if (check(sq))
        {
            for (auto x : sq)
            {
                st[x] = true;
                if (dfs(depth - 1)) return true;
                st[x] = false;
            }
            return false;
        }
    }

    return true;
}

main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d", &n);

        m = 0;
        for (int i = 1; i <= n; i ++ )
            for (int a = 1; a + i - 1 <= n; a ++ )
                for (int b = 1; b + i - 1 <= n; b ++ )
                {
                    square[m].clear();
                    for (int j = 0; j < i; j ++ )
                    {
                        int d = 2 * n + 1;
                        square[m].push_back((a - 1) * d + b + j);
                        square[m].push_back((a - 1 + i) * d + b + j);
                        square[m].push_back(n + 1 + (a - 1) * d + b - 1 + j * d);
                        square[m].push_back(n + 1 + (a - 1) * d + b - 1 + j * d + i);
                    }
                    m ++ ;
                }

        memset(st, 0, sizeof st);
        int k;
        scanf("%d", &k);
        for (int i = 0, t; i < k; i ++ )
        {
            scanf("%d", &t);
            st[t] = true;
        }

        int depth = 0;
        while (!dfs(depth)) depth ++ ;

        printf("%d\n", depth);
    }

    return 0;
}
```

## 0x29 总结与练习

##### AcWing183   靶形数独
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 9, M = 1 << N;

int ones[M], map[M];
int row[N], col[N], cell[3][3];
int g[N][N];
int ans = -1;

inline int lowbit(int x)
{
    return x & -x;
}

void init()
{
    for (int i = 0; i < N; i++) map[1 << i] = i;
    for (int i = 0; i < M; i++)
        for (int j = i; j; j -= lowbit(j))
            ones[i] ++;

    for (int i = 0; i < 9; i++) row[i] = col[i] = cell[i / 3][i % 3] = M - 1;
}

inline int get_score(int x, int y, int t)
{
    return (min(min(x, 8 - x), min(y, 8 - y)) + 6) * t;
}

inline void draw(int x, int y, int t)
{
    int s = 1;
    if (t > 0) g[x][y] = t;
    else
    {
        s = -1;
        t = -t;
        g[x][y] = 0;
    }

    t--;
    row[x] -= (1 << t) * s;
    col[y] -= (1 << t) * s;
    cell[x / 3][y / 3] -= (1 << t) * s;
}

inline int get(int x, int y)
{
    return row[x] & col[y] & cell[x / 3][y / 3];
}

void dfs(int cnt, int score)
{
    if (!cnt)
    {
        ans = max(ans, score);
        return;
    }

    int minv = 10;
    int x, y;
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            if (!g[i][j])
            {
                int t = ones[get(i, j)];
                if (t < minv)
                {
                    minv = ones[get(i, j)];
                    x = i, y = j;
                }
            }

    for (int i = get(x, y); i; i -= lowbit(i))
    {
        int t = map[lowbit(i)] + 1;
        draw(x, y, t);
        dfs(cnt - 1, score + get_score(x, y, t));
        draw(x, y, -t);
    }
}

int main()
{
    init();

    int cnt = 0, score = 0;
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j++)
        {
            int x;
            cin >> x;
            if (x)
            {
                draw(i, j, x);
                score += get_score(i, j, x);
            }
            else cnt++;
        }

    dfs(cnt, score);

    cout << ans << endl;

    return 0;
}
```
  
##### AcWing184   虫食算
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 30;

int n;
char e[3][N];
int q[N], path[N];
bool st[N];     // state

bool check()
{
    for (int i = n - 1, t = 0; i >= 0; i -- )
    {
        int a = e[0][i] - 'A', b = e[1][i] - 'A', c = e[2][i] - 'A';
        if (path[a] != -1 && path[b] != -1 && path[c] != -1)
        {
            a = path[a], b = path[b], c = path[c];
            if (t != -1)
            {
                if ((a + b + t) % n != c) return false;
                if (!i && a + b + t >= n) return false;
                t = (a + b + t) / n;
            }
            else
            {
                if ((a + b + 0) % n != c && (a + b + 1) % n != c) return false;
                if (!i && a + b >= n) return false;
            }
        }
        else t = -1;
    }

    return true;
}

bool dfs(int u)
{
    if (u == n) return true;

    for (int i = 0; i < n; i ++ )
        if (!st[i])
        {
            st[i] = true;
            path[q[u]] = i;
            if (check() && dfs(u + 1)) return true;
            st[i] = false;
            path[q[u]] = -1;
        }

    return false;
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < 3; i ++ ) scanf("%s", e[i]);
    for (int i = n - 1, k = 0; i >= 0; i -- )
        for (int j = 0; j < 3; j ++ )
        {
            int t = e[j][i] - 'A';
            if (!st[t])
            {
                st[t] = true;
                q[k ++ ] = t;
            }
        }

    memset(st, 0, sizeof st);
    memset(path, -1, sizeof path);
    dfs(0);

    for (int i = 0; i < n; i ++ ) printf("%d ", path[i]);

    return 0;
}

```
##### AcWing185   玛雅游戏
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

int n;
int g[5][7], bg[5][5][7];
int cnt[11], bcnt[5][11];
bool st[5][7];

struct Path
{
    int x, y, d;
}path[5];

void move(int a, int b, int c)
{
    swap(g[a][b], g[c][b]);

    while (true)
    {
        bool flag = true;
        // 处理悬空方格
        for (int x = 0; x < 5; x ++ )
        {
            int z = 0;
            for (int y = 0; y < 7; y ++ )
                if (g[x][y])
                    g[x][z ++ ] = g[x][y];
            while (z < 7) g[x][z ++ ] = 0;
        }

        memset(st, 0, sizeof st);
        for (int x = 0; x < 5; x ++ )
            for (int y = 0; y < 7; y ++ )
                if (g[x][y])
                {
                    int l = x, r = x;
                    while (l - 1 >= 0 && g[l - 1][y] == g[x][y]) l -- ;
                    while (r + 1 < 5 && g[r + 1][y] == g[x][y]) r ++ ;
                    if (r - l + 1 >= 3)
                    {
                        st[x][y] = true;
                        flag = false;
                    }
                    else
                    {
                        l = r = y;
                        while (l - 1 >= 0 && g[x][l - 1] == g[x][y]) l -- ;
                        while (r + 1 < 7 && g[x][r + 1] == g[x][y]) r ++ ;

                        if (r - l + 1 >= 3)
                        {
                            st[x][y] = true;
                            flag = false;
                        }
                    }
                }

        if (flag) break;
        for (int x = 0; x < 5; x ++ )
            for (int y = 0; y < 7; y ++ )
                if (st[x][y])
                {
                    cnt[0] -- ;
                    cnt[g[x][y]] -- ;
                    g[x][y] = 0;
                }
    }
}

bool dfs(int u)
{
    if (u == n) return !cnt[0];

    for (int i = 1; i <= 10; i ++ )
        if (cnt[i] == 1 || cnt[i] == 2)
            return false;

    // 枚举所有操作
    memcpy(bg[u], g, sizeof g);
    memcpy(bcnt[u], cnt, sizeof cnt);
    for (int x = 0; x < 5; x ++ )
        for (int y = 0; y < 7; y ++ )
            if (g[x][y])
            {
                int nx = x + 1;
                if (nx < 5)
                {
                    path[u] = {x, y, 1};
                    move(x, y, nx);
                    if (dfs(u + 1)) return true;
                    memcpy(g, bg[u], sizeof g);
                    memcpy(cnt, bcnt[u], sizeof cnt);
                }

                nx = x - 1;
                if (nx >= 0 && !g[nx][y])
                {
                    path[u] = {x, y, -1};
                    move(x, y, nx);
                    if (dfs(u + 1)) return true;
                    memcpy(g, bg[u], sizeof g);
                    memcpy(cnt, bcnt[u], sizeof cnt);
                }
            }


    return false;
}

int main()
{
    scanf("%d", &n);
    for (int x = 0; x < 5; x ++ )
    {
        int t, y = 0;
        while (scanf("%d", &t), t)
        {
            cnt[0] ++ ;
            cnt[t] ++ ;
            g[x][y ++ ] = t;
        }
    }

    if (dfs(0))
    {
        for (int i = 0; i < n; i ++ ) printf("%d %d %d\n", path[i].x, path[i].y, path[i].d);
    }
    else puts("-1");

    return 0;
}
```
