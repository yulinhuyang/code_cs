## 0x23 剪枝

##### AcWing167   木棒
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 70;

int n;
int sum, length;
int sticks[N];
bool st[N];

bool dfs(int u, int cur, int start)
{
    if (u * length == sum) return true;
    if (cur == length) return dfs(u + 1, 0, 0);

    for (int i = start; i < n; i ++ )
    {
        if (st[i]) continue;
        int l = sticks[i];
        if (cur + l <= length)
        {
            st[i] = true;
            if (dfs(u, cur + l, i + 1)) return true;
            st[i] = false;

            // 剪枝3 如果是第一个木棒失败，则一定失败
            if (!cur) return false;

            // 剪枝4 如果是最后一个木棒失败，则一定失败
            if (cur + l == length) return false;

            // 剪枝2 跳过相同木棒
            int j = i;
            while (j < n && sticks[j] == l) j ++ ;
            i = j - 1;
        }
    }


    return false;
}

int main()
{
    while (cin >> n, n)
    {
        sum = 0, length = 0;
        memset(st, false, sizeof st);

        for (int i = 0; i < n; i ++ )
        {
            cin >> sticks[i];
            if (sticks[i] > 50) continue;
            sum += sticks[i];
            length = max(length, sticks[i]);
        }

        // 剪枝：优化搜索顺序
        sort(sticks, sticks + n);
        reverse(sticks, sticks + n);

        for (int i = 0; i < n; i ++ )
            if (sticks[i] > 50)
                st[i] = true;

        while (true)
        {
            if (sum % length == 0 && dfs(0, 0, 0))
            {
                cout << length << endl;
                break;
            }
            length ++ ;
        }
    }

    return 0;
}
``` 
##### AcWing168   生日蛋糕  
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

const int N = 25, INF = 1e9;

int n, m;
int minv[N], mins[N];
int R[N], H[N];
int ans = INF;

void dfs(int u, int v, int s)
{
    if (v + minv[u] > n) return;
    if (s + mins[u] >= ans) return;
    if (s + 2 * (n - v) / R[u + 1] >= ans) return;

    if (!u)
    {
        if (v == n) ans = s;
        return;
    }

    for (int r = min(R[u + 1] - 1, (int)sqrt(n - v)); r >= u; r -- )
        for (int h = min(H[u + 1] - 1, (n - v) / r / r); h >= u; h -- )
        {
            int t = 0;
            if (u == m) t = r * r;
            R[u] = r, H[u] = h;
            dfs(u - 1, v + r * r * h, s + 2 * r * h + t);
        }
}

int main()
{
    cin >> n >> m;

    for (int i = 1; i <= m; i ++ )
    {
        minv[i] = minv[i - 1] + i * i * i;
        mins[i] = mins[i - 1] + 2 * i * i;
    }

    R[m + 1] = H[m + 1] = INF;

    dfs(m, 0, 0);

    if (ans == INF) ans = 0;
    cout << ans << endl;

    return 0;
}
```
##### AcWing169   数独2  
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 16;


int map[1 << N], ones[1 << N];
int state[N][N];
char str[N][N + 1];

int bstate[N * N + 1][N][N], bstate2[N * N + 1][N][N];
char bstr[N * N + 1][N][N + 1];


inline int lowbit(int x)
{
    return x & -x;
}

void draw(int x, int y, int c)
{
    str[x][y] = 'A' + c;

    for (int i = 0; i < N; i ++ )
    {
        state[x][i] &= ~(1 << c);
        state[i][y] &= ~(1 << c);
    }

    int sx = x / 4 * 4, sy = y / 4 * 4;
    for (int i = 0; i < 4; i ++ )
        for (int j = 0; j < 4; j ++ )
                state[sx + i][sy + j] &= ~(1 << c);

    state[x][y] = 1 << c;
}


bool dfs(int cnt)
{
    if (!cnt) return true;

    int kcnt = cnt;
    memcpy(bstate[kcnt], state, sizeof state);
    memcpy(bstr[kcnt], str, sizeof str);

    // 每个空格，如果不能填则返回false；如果只有一个选项，则直接填上
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            if (str[i][j] == '-')
            {
                if (!state[i][j])
                {
                    memcpy(state, bstate[kcnt], sizeof state);
                    memcpy(str, bstr[kcnt], sizeof str);
                    return false;
                }

                if (ones[state[i][j]] == 1)
                {
                    draw(i, j, map[state[i][j]]);
                    cnt -- ;
                }
            }

    // 每一行，如果某个字母不能填，则返回false；如果某个字母只有一种填法，则直接填
    for (int i = 0; i < N; i ++ )
    {
        int sor = 0, sand = (1 << N) - 1;
        int drawn = 0;
        for (int j = 0; j < N; j ++ )
        {
            int s = state[i][j];
            sand &= ~(sor & s);
            sor |= s;

            if (str[i][j] != '-') drawn |= state[i][j];
        }

        if (sor != (1 << N) - 1)
        {
            memcpy(state, bstate[kcnt], sizeof state);
            memcpy(str, bstr[kcnt], sizeof str);
            return false;
        }

        for (int j = sand; j; j -= lowbit(j))
        {
            int t = lowbit(j);
            if (!(drawn & t))
            {
                for (int k = 0; k < N; k ++ )
                    if (state[i][k] & t)
                    {
                        draw(i, k, map[t]);
                        cnt -- ;
                        break;
                    }
            }
        }
    }

    // 每一列，如果某个字母不能填，则返回false；如果某个字母只有一种填法，则直接填
    for (int i = 0; i < N; i ++ )
    {
        int sor = 0, sand = (1 << N) - 1;
        int drawn = 0;
        for (int j = 0; j < N; j ++ )
        {
            int s = state[j][i];
            sand &= ~(sor & s);
            sor |= s;

            if (str[j][i] != '-') drawn |= state[j][i];
        }

        if (sor != (1 << N) - 1)
        {
            memcpy(state, bstate[kcnt], sizeof state);
            memcpy(str, bstr[kcnt], sizeof str);
            return false;
        }

        for (int j = sand; j; j -= lowbit(j))
        {
            int t = lowbit(j);
            if (!(drawn & t))
            {
                for (int k = 0; k < N; k ++ )
                    if (state[k][i] & t)
                    {
                        draw(k, i, map[t]);
                        cnt -- ;
                        break;
                    }
            }
        }
    }

    // 每个16宫格，如果某个字母不能填，则返回false；如果某个字母只有一种填法，则直接填
    for (int i = 0; i < N; i ++ )
    {
        int sor = 0, sand = (1 << N) - 1;
        int drawn = 0;
        for (int j = 0; j < N; j ++ )
        {
            int sx = i / 4 * 4, sy = i % 4 * 4;
            int dx = j / 4, dy = j % 4;
            int s = state[sx + dx][sy + dy];
            sand &= ~(sor & s);
            sor |= s;

            if (str[sx + dx][sy + dy] != '-') drawn |= state[sx + dx][sy + dy];
        }

        if (sor != (1 << N) - 1)
        {
            memcpy(state, bstate[kcnt], sizeof state);
            memcpy(str, bstr[kcnt], sizeof str);
            return false;
        }
        for (int j = sand; j; j -= lowbit(j))
        {
            int t = lowbit(j);
            if (!(drawn & t))
            {
                for (int k = 0; k < N; k ++ )
                {
                    int sx = i / 4 * 4, sy = i % 4 * 4;
                    int dx = k / 4, dy = k % 4;
                    if (state[sx + dx][sy + dy] & t)
                    {
                        draw(sx + dx, sy + dy, map[t]);
                        cnt -- ;
                        break;
                    }
                }
            }
        }
    }

    if (!cnt) return true;

    int x, y, s = 100;
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            if (str[i][j] == '-' && ones[state[i][j]] < s)
            {
                s = ones[state[i][j]];
                x = i, y = j;
            }

    memcpy(bstate2[kcnt], state, sizeof state);
    for (int i = state[x][y]; i; i -= lowbit(i))
    {
        memcpy(state, bstate2[kcnt], sizeof state);
        draw(x, y, map[lowbit(i)]);
        if (dfs(cnt - 1)) return true;
    }

    memcpy(state, bstate[kcnt], sizeof state);
    memcpy(str, bstr[kcnt], sizeof str);
    return false;
}


int main()
{
    for (int i = 0; i < N; i ++ ) map[1 << i] = i;
    for (int i = 0; i < 1 << N; i ++ )
    {
        for (int j = i; j; j -= lowbit(j))
            ones[i] ++ ;
    }

    while (cin >> str[0])
    {
        for (int i = 1; i < N; i ++ ) cin >> str[i];

        for (int i = 0; i < N; i ++ )
            for (int j = 0; j < N; j ++ )
                state[i][j] = (1 << N) - 1;

        int cnt = 0;
        for (int i = 0; i < N; i ++ )
            for (int j = 0; j < N; j ++ )
                if (str[i][j] != '-')
                    draw(i, j, str[i][j] - 'A');
                else cnt ++ ;

        dfs(cnt);

        for (int i = 0; i < N; i ++ ) cout << str[i] << endl;
        cout << endl;
    }

    return 0;
}
```

## 0x24 迭代加深

##### AcWing170   加成序列
```cpp
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
##### AcWing171   送礼物
```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long LL;

const int N = 1 << 24;

int n, m, k;
int g[50], weights[N];
int cnt = 0;
int ans;


void dfs(int u, int s)
{
    if (u == k)
    {
        weights[cnt ++ ] = s;
        return;
    }

    if ((LL)s + g[u] <= m) dfs(u + 1, s + g[u]);
    dfs(u + 1, s);
}


void dfs2(int u, int s)
{
    if (u == n)
    {
        int l = 0, r = cnt - 1;
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if (weights[mid] + (LL)s <= m) l = mid;
            else r = mid - 1;
        }
        if (weights[l] + (LL)s <= m) ans = max(ans, weights[l] + s);

        return;
    }

    if ((LL)s + g[u] <= m) dfs2(u + 1, s + g[u]);
    dfs2(u + 1, s);
}


int main()
{
    cin >> m >> n;
    for (int i = 0; i < n; i ++ ) cin >> g[i];

    sort(g, g + n);
    reverse(g, g + n);

    k = n / 2 + 2;
    dfs(0, 0);

    sort(weights, weights + cnt);
    int t = 1;
    for (int i = 1; i < cnt; i ++ )
        if (weights[i] != weights[i - 1])
            weights[t ++ ] = weights[i];
    cnt = t;

    dfs2(k, 0);

    cout << ans << endl;

    return 0;
}
```  

## 0x25 广度优先搜索

##### AcWing172   立体推箱子
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 510;


struct State
{
    int x, y, lie;
};


int n, m;
char g[N][N];
int dist[N][N][3];


bool check(int x, int y)
{
    if (x < 0 || x >= n || y < 0 || y >= m) return false;
    return g[x][y] != '#';
}


int bfs(State start, State end)
{
    queue<State> q;
    memset(dist, -1, sizeof dist);
    dist[start.x][start.y][start.lie] = 0;
    q.push(start);

    int d[3][4][3] = {
        {{-2, 0, 2}, {0, 1, 1}, {1, 0, 2}, {0, -2, 1}},
        {{-1, 0, 1}, {0, 2, 0}, {1, 0, 1}, {0, -1, 0}},
        {{-1, 0, 0}, {0, 1, 2}, {2, 0, 0}, {0, -1, 2}}
    };

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        for (int i = 0; i < 4; i ++ )
        {
            State next = {t.x + d[t.lie][i][0], t.y + d[t.lie][i][1], d[t.lie][i][2]};

            int x = next.x, y = next.y;
            if (!check(x, y)) continue;
            if (next.lie == 0)
            {
                if (g[x][y] == 'E') continue;
            }
            else if (next.lie == 1)
            {
                if (!check(x, y + 1)) continue;
            }
            else
            {
                if (!check(x + 1, y)) continue;
            }

            if (dist[next.x][next.y][next.lie] == -1)
            {
                dist[next.x][next.y][next.lie] = dist[t.x][t.y][t.lie] + 1;
                q.push(next);
            }
        }
    }

    return dist[end.x][end.y][end.lie];
}


int main()
{
    while (scanf("%d%d", &n, &m), n || m)
    {
        for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);

        State start = {-1}, end;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == 'X' && start.x == -1)
                {
                    if (g[i + 1][j] == 'X') start = {i, j, 2};
                    else if (g[i][j + 1] == 'X') start = {i, j, 1};
                    else start = {i, j, 0};
                }
                else if (g[i][j] == 'O') end = {i, j, 0};

        int res = bfs(start, end);
        if (res == -1) puts("Impossible");
        else printf("%d\n", res);
    }

    return 0;
}
```
##### AcWing173   矩阵距离
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

const int N = 1010;

int n, m;
int d[N][N];
char g[N][N];
PII q[N * N];

void bfs()
{
    int hh = 0, tt = -1;

    memset(d, -1, sizeof d);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            if (g[i][j] == '1')
            {
                d[i][j] = 0;
                q[ ++ tt] = {i, j};
            }

    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    while (hh <= tt)
    {
        auto t = q[hh ++ ];
        int x = t.first, y = t.second;

        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (!a || a > n || !b || b > m || d[a][b] != -1) continue;

            d[a][b] = d[x][y] + 1;
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
        for (int j = 1; j <= m; j ++ )
            printf("%d ", max(d[i][j], 0));
        puts("");
    }

    return 0;
}
```  
##### AcWing174   推箱子
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 25;

struct Node
{
    int x, y, dir;
};

int n, m;
char g[N][N];   // 存储游戏地图
Node pre[N][N][4];  // 表示箱子在(x, y)，上一个格子在i方向上的状态，即上一个状态是(x + dx[i], y + dy[i])
vector<int> path[N][N][4];  // path[j][k][i] 表示人从推(j,k,i)的上一个状态的位置，走到推(j,k,i)这个状态的位置的行走路径
bool st[N][N][4], used[N][N];   // BFS的判重数组，为了防止BFS遍历相同状态
PII dist[N][N][4];  // dist[j][k][i]是表示从初始状态到达j,k,i状态所需要的箱子最短路程和人行走最短路程
int dx[4] = {1, -1, 0, 0}, dy[4] = {0, 0, 1, -1};  // 依次表示下、上、右、左四个方向
int pre_man[N][N];  // pre_man[x][y]表示人从哪个方向走到(x, y)，如果pre_man[x][y] = i, 那么上一个状态是(x - dx[i], y - dy[i])

bool check(int x, int y)    // 判断(x, y)是否在地图内，且是空地
{
    return x >= 0 && x < n && y >= 0 && y < m && g[x][y] != '#';
}

void output(Node end, PII box)
{
    char ops[] = "nswe";
    string res;
    while (end.dir != -1)
    {
        res += ops[end.dir] - 32;
        for (auto dir : path[end.x][end.y][end.dir]) res += ops[dir];
        end = pre[end.x][end.y][end.dir];
        printf("(%d,%d,%d) ", end.x, end.y, end.dir);
    }
    reverse(res.begin(), res.end());
    cout << res << endl;
}

// 求人从start走到end，中间不经过box的最短路径，行走序列保存在seq中
int bfs_man(PII start, PII end, PII box, vector<int> &seq)
{
    memset(used, false, sizeof used);
    memset(pre_man, -1, sizeof pre_man);

    queue<PII> q;
    q.push(start);
    used[start.first][start.second] = true;
    used[box.first][box.second] = true;

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        if (t == end)
        {
            seq.clear();
            int x = t.first, y = t.second;
            while (pre_man[x][y] != -1)
            {
                int dir = pre_man[x][y] ^ 1;
                seq.push_back(dir);
                x += dx[dir], y += dy[dir];
            }

            return seq.size();
        }

        for (int ii = 0; ii < 4; ii ++ )
        {
            int i = ii ^ 1;
            int x = t.first, y = t.second;
            int a = x + dx[i], b = y + dy[i];
            if (check(a, b) && !used[a][b])
            {
                used[a][b] = true;
                pre_man[a][b] = i;
                q.push({a, b});
            }
        }
    }

    return -1;
}

bool bfs_box(PII man, PII box, Node &end) 
{
    memset(st, false, sizeof st);

    queue<Node> q;
    for (int i = 0; i < 4; i ++ )
    {
        int x = box.first, y = box.second;
        int a = x + dx[i], b = y + dy[i];
        int j = x - dx[i], k = y - dy[i];
        vector<int> seq;

        if (check(a, b) && check(j, k) && bfs_man(man, {a, b}, box, seq) != -1)
        {
            st[j][k][i] = true;
            q.push({j, k, i});
            dist[j][k][i] = {1, seq.size()};
            path[j][k][i] = seq;
            pre[j][k][i] = {x, y, -1};
        }
    }

    bool success = false;
    PII man_d = {1e9, 1e9};

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        if (g[t.x][t.y] == 'T')
        {
            success = true;

            if (dist[t.x][t.y][t.dir] < man_d)
            {
                man_d = dist[t.x][t.y][t.dir];
                end = t;
            }
        }

        for (int i = 0; i < 4; i ++ )
        {
            int a = t.x + dx[i], b = t.y + dy[i];
            int j = t.x - dx[i], k = t.y - dy[i];
            if (check(a, b) && check(j, k))
            {
                vector<int> seq;
                auto &p = dist[j][k][i];
                int distance = bfs_man({t.x + dx[t.dir], t.y + dy[t.dir]}, {a, b}, {t.x, t.y}, seq);
                if (distance != -1)
                {
                    PII td = {dist[t.x][t.y][t.dir].first + 1, dist[t.x][t.y][t.dir].second + distance};
                    if (!st[j][k][i])
                    {
                        st[j][k][i] = true;
                        q.push({j, k, i});
                        path[j][k][i] = seq;
                        pre[j][k][i] = t;
                        p = td;
                    }
                    else if (p > td)
                    {
                        p = td;
                        path[j][k][i] = seq;
                        pre[j][k][i] = t;
                    }
                }
            }
        }
    }

    return success;
}

int main()
{
    int T = 1;
    while (cin >> n >> m, n || m)
    {
        for (int i = 0; i < n; i ++ ) cin >> g[i];

        printf("Maze #%d\n", T ++ );

        PII man, box;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == 'S') man = {i, j};
                else if (g[i][j] == 'B') box = {i, j};

        Node end;

        if (!bfs_box(man, box, end)) puts("Impossible.");
        else
        {
            char ops[] = "nswe";
            string res;
            while (end.dir != -1)
            {
                res += ops[end.dir] - 32;
                for (auto dir : path[end.x][end.y][end.dir]) res += ops[dir];
                end = pre[end.x][end.y][end.dir];
            }
            reverse(res.begin(), res.end());
            cout << res << endl;
        }

        puts("");
    }

    return 0;
}

```  

## 0x26 广搜变形

##### AcWing175   电路维修
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <deque>

using namespace std;

typedef pair<int, int> PII;

const int N = 510;

int n, m;
char g[N][N];
int d[N][N];
bool st[N][N];

int bfs()
{
    memset(st, 0, sizeof st);
    memset(d, 0x3f, sizeof d);

    deque<PII> q;
    q.push_back({0, 0});
    d[0][0] = 0;

    int dx[4] = {-1, -1, 1, 1}, dy[4] = {-1, 1, 1, -1};
    int ix[4] = {-1, -1, 0, 0}, iy[4] = {-1, 0, 0, -1};
    char cs[] = "\\/\\/";

    while (q.size())
    {
        auto t = q.front();
        q.pop_front();

        int x = t.first, y = t.second;
        if (st[x][y]) continue;
        st[x][y] = true;

        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            int j = x + ix[i], k = y + iy[i];
            if (a >= 0 && a <= n && b >= 0 && b <= m)
            {
                int w = 0;
                if (g[j][k] != cs[i]) w = 1;
                if (d[a][b] > d[x][y] + w)
                {
                    d[a][b] = d[x][y] + w;
                    if (w) q.push_back({a, b});
                    else q.push_front({a, b});
                }
            }
        }
    }

    if (d[n][m] == 0x3f3f3f3f) return -1;
    return d[n][m];
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d%d", &n, &m);
        for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);

        int t = bfs();

        if (t == -1) puts("NO SOLUTION");
        else printf("%d\n", t);
    }

    return 0;
}
```
##### AcWing176   装满的油箱
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, C = 110, M = 20010;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int price[N];
int dist[N][C];
bool st[N][C];

struct Ver
{
    int d, u, c;
    bool operator< (const Ver &W)const
    {
        return d > W.d;
    }
};

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int dijkstra(int start, int end, int cap)
{
    memset(dist, 0x3f, sizeof dist);
    memset(st, false, sizeof st);
    priority_queue<Ver> heap;
    heap.push({0, start, 0});
    dist[start][0] = 0;

    while (heap.size())
    {
        auto t = heap.top(); heap.pop();

        if (t.u == end) return t.d;

        if (st[t.u][t.c]) continue;
        st[t.u][t.c] = true;

        if (t.c < cap)
        {
            if (dist[t.u][t.c + 1] > t.d + price[t.u])
            {
                dist[t.u][t.c + 1] = t.d + price[t.u];
                heap.push({dist[t.u][t.c + 1], t.u, t.c + 1});
            }
        }

        for (int i = h[t.u]; ~i; i = ne[i])
        {
            int j = e[i];
            if (t.c >= w[i])
            {
                if (dist[j][t.c - w[i]] > t.d)
                {
                    dist[j][t.c - w[i]] = t.d;
                    heap.push({dist[j][t.c - w[i]], j, t.c - w[i]});
                }
            }
        }
    }

    return -1;
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n; i ++ ) scanf("%d", &price[i]);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c), add(b, a, c);
    }

    int query;
    scanf("%d", &query);
    while (query -- )
    {
        int a, b, c;
        scanf("%d%d%d", &c, &a, &b);
        int t = dijkstra(a, b, c);
        if (t == -1) puts("impossible");
        else printf("%d\n", t);
    }

    return 0;
}

```
