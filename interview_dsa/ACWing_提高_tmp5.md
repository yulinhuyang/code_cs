##### AcWing175 电路维修
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <deque>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 510, M = N * N;

int n, m;
char g[N][N];
int dist[N][N];
bool st[N][N];

int bfs()
{
    memset(dist, 0x3f, sizeof dist);
    memset(st, 0, sizeof st);
    dist[0][0] = 0;
    deque<PII> q;
    q.push_back({0, 0});

    char cs[] = "\\/\\/";
    int dx[4] = {-1, -1, 1, 1}, dy[4] = {-1, 1, 1, -1};
    int ix[4] = {-1, -1, 0, 0}, iy[4] = {-1, 0, 0, -1};

    while (q.size())
    {
        PII t = q.front();
        q.pop_front();

        if (st[t.x][t.y]) continue;
        st[t.x][t.y] = true;

        for (int i = 0; i < 4; i ++ )
        {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a > n || b < 0 || b > m) continue;

            int ca = t.x + ix[i], cb = t.y + iy[i];
            int d = dist[t.x][t.y] + (g[ca][cb] != cs[i]);

            if (d < dist[a][b])
            {
                dist[a][b] = d;

                if (g[ca][cb] != cs[i]) q.push_back({a, b});
                else q.push_front({a, b});
            }
        }
    }

    return dist[n][m];
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

        if (t == 0x3f3f3f3f) puts("NO SOLUTION");
        else printf("%d\n", t);
    }

    return 0;
}
```
##### AcWing190 字串变换
```cpp
注意
在本题的视频讲解中，我想当然地每次每一边只扩展一个点了，但这样是不正确的。正确做法应该是每次每边扩展完整一层。

反例如下图所示：

如上图，如果每次不是扩展完整一层，而是只扩展一个点。此时上面该扩展点 aa 了，点 aa 搜到了下半部分的点 cc，此时算出的最短路长度是 x+1+y+1+1=x+y+3x+1+y+1+1=x+y+3。但是最优解可能是后面还没扩展到的点 bb 和点 dd 之间的路径，这条路径的长度是 x+1+y+1=x+y+2x+1+y+1=x+y+2。

#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>
#include <unordered_map>

using namespace std;

const int N = 6;

int n;
string A, B;
string a[N], b[N];

int extend(queue<string>& q, unordered_map<string, int>&da, unordered_map<string, int>& db, 
    string a[N], string b[N])
{
    int d = da[q.front()];
    while (q.size() && da[q.front()] == d)
    {
        auto t = q.front();
        q.pop();

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < t.size(); j ++ )
                if (t.substr(j, a[i].size()) == a[i])
                {
                    string r = t.substr(0, j) + b[i] + t.substr(j + a[i].size());
                    if (db.count(r)) return da[t] + db[r] + 1;
                    if (da.count(r)) continue;
                    da[r] = da[t] + 1;
                    q.push(r);
                }
    }

    return 11;
}

int bfs()
{
    if (A == B) return 0;
    queue<string> qa, qb;
    unordered_map<string, int> da, db;

    qa.push(A), qb.push(B);
    da[A] = db[B] = 0;

    int step = 0;
    while (qa.size() && qb.size())
    {
        int t;
        if (qa.size() < qb.size()) t = extend(qa, da, db, a, b);
        else t = extend(qb, db, da, b, a);

        if (t <= 10) return t;
        if ( ++ step == 10) return -1;
    }

    return -1;
}

int main()
{
    cin >> A >> B;
    while (cin >> a[n] >> b[n]) n ++ ;

    int t = bfs();
    if (t == -1) puts("NO ANSWER!");
    else cout << t << endl;

    return 0;
}
```
##### AcWing178 第K短路
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
##### AcWing179 八数码
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>
#include <unordered_map>

using namespace std;

int f(string state)
{
    int res = 0;
    for (int i = 0; i < state.size(); i ++ )
        if (state[i] != 'x')
        {
            int t = state[i] - '1';
            res += abs(i / 3 - t / 3) + abs(i % 3 - t % 3);
        }
    return res;
}

string bfs(string start)
{
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    char op[4] = {'u', 'r', 'd', 'l'};

    string end = "12345678x";
    unordered_map<string, int> dist;
    unordered_map<string, pair<string, char>> prev;
    priority_queue<pair<int, string>, vector<pair<int, string>>, greater<pair<int, string>>> heap;

    heap.push({f(start), start});
    dist[start] = 0;

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        string state = t.second;

        if (state == end) break;

        int step = dist[state];
        int x, y;
        for (int i = 0; i < state.size(); i ++ )
            if (state[i] == 'x')
            {
                x = i / 3, y = i % 3;
                break;
            }
        string source = state;
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < 3 && b >= 0 && b < 3)
            {
                swap(state[x * 3 + y], state[a * 3 + b]);
                if (!dist.count(state) || dist[state] > step + 1)
                {
                    dist[state] = step + 1;
                    prev[state] = {source, op[i]};
                    heap.push({dist[state] + f(state), state});
                }
                swap(state[x * 3 + y], state[a * 3 + b]);
            }
        }
    }

    string res;
    while (end != start)
    {
        res += prev[end].second;
        end = prev[end].first;
    }
    reverse(res.begin(), res.end());
    return res;
}

int main()
{
    string g, c, seq;
    while (cin >> c)
    {
        g += c;
        if (c != "x") seq += c;
    }

    int t = 0;
    for (int i = 0; i < seq.size(); i ++ )
        for (int j = i + 1; j < seq.size(); j ++ )
            if (seq[i] > seq[j])
                t ++ ;

    if (t % 2) puts("unsolvable");
    else cout << bfs(g) << endl;

    return 0;
}
```
 
##### AcWing1112 迷宫
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n;
char g[N][N];
int xa, ya, xb, yb;
int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
bool st[N][N];

bool dfs(int x, int y)
{
    if (g[x][y] == '#') return false;
    if (x == xb && y == yb) return true;

    st[x][y] = true;

    for (int i = 0; i < 4; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        if (a < 0 || a >= n || b < 0 || b >= n) continue;
        if (st[a][b]) continue;
        if (dfs(a, b)) return true;
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
        for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);
        scanf("%d%d%d%d", &xa, &ya, &xb, &yb);

        memset(st, 0, sizeof st);
        if (dfs(xa, ya)) puts("YES");
        else puts("NO");
    }

    return 0;
}

```
##### AcWing1113 红与黑
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 25;

int n, m;
char g[N][N];
bool st[N][N];

int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

int dfs(int x, int y)
{
    int cnt = 1;

    st[x][y] = true;
    for (int i = 0; i < 4; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        if (a < 0 || a >= n || b < 0 || b >= m) continue;
        if (g[a][b] != '.') continue;
        if (st[a][b]) continue;

        cnt += dfs(a, b);
    }

    return cnt;
}

int main()
{
    while (cin >> m >> n, n || m)
    {
        for (int i = 0; i < n; i ++ ) cin >> g[i];

        int x, y;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == '@')
                {
                    x = i;
                    y = j;
                }

        memset(st, 0, sizeof st);
        cout << dfs(x, y) << endl;
    }

    return 0;
}
```
##### AcWing1116 马走日
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10;

int n, m;
bool st[N][N];
int ans;
int dx[8] = {-2, -1, 1, 2, 2, 1, -1, -2};
int dy[8] = {1, 2, 2, 1, -1, -2, -2, -1};

void dfs(int x, int y, int cnt)
{
    if (cnt == n * m)
    {
        ans ++ ;
        return;
    }
    st[x][y] = true;

    for (int i = 0; i < 8; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        if (a < 0 || a >= n || b < 0 || b >= m) continue;
        if (st[a][b]) continue;
        dfs(a, b, cnt + 1);
    }

    st[x][y] = false;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        int x, y;
        scanf("%d%d%d%d", &n, &m, &x, &y);

        memset(st, 0, sizeof st);
        ans = 0;
        dfs(x, y, 1);

        printf("%d\n", ans);
    }

    return 0;
}
```
##### AcWing1117 单词接龙
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 21;

int n;
string word[N];
int g[N][N];
int used[N];
int ans;

void dfs(string dragon, int last)
{
    ans = max((int)dragon.size(), ans);

    used[last] ++ ;

    for (int i = 0; i < n; i ++ )
        if (g[last][i] && used[i] < 2)
            dfs(dragon + word[i].substr(g[last][i]), i);

    used[last] -- ;
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> word[i];
    char start;
    cin >> start;

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
        {
            string a = word[i], b = word[j];
            for (int k = 1; k < min(a.size(), b.size()); k ++ )
                if (a.substr(a.size() - k, k) == b.substr(0, k))
                {
                    g[i][j] = k;
                    break;
                }
        }

    for (int i = 0; i < n; i ++ )
        if (word[i][0] == start)
            dfs(word[i], i);

    cout << ans << endl;

    return 0;
}
```
##### AcWing1118 分成互质组
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10;

int n;
int p[N];
int group[N][N];
bool st[N];
int ans = N;

int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

bool check(int group[], int gc, int i)
{
    for (int j = 0; j < gc; j ++ )
        if (gcd(p[group[j]], p[i]) > 1)
            return false;
    return true;
}

void dfs(int g, int gc, int tc, int start)
{
    if (g >= ans) return;
    if (tc == n) ans = g;

    bool flag = true;
    for (int i = start; i < n; i ++ )
        if (!st[i] && check(group[g], gc, i))
        {
            st[i] = true;
            group[g][gc] = i;
            dfs(g, gc + 1, tc + 1, i + 1);
            st[i] = false;

            flag = false;
        }

    if (flag) dfs(g + 1, 0, tc, 0);
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> p[i];

    dfs(1, 0, 0, 0);

    cout << ans << endl;

    return 0;
}
```
##### AcWing165 小猫爬山
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 20;

int n, m;
int w[N];
int sum[N];
int ans = N;

void dfs(int u, int k)
{
    // 最优性剪枝
    if (k >= ans) return;
    if (u == n)
    {
        ans = k;
        return;
    }

    for (int i = 0; i < k; i ++ )
        if (sum[i] + w[u] <= m) // 可行性剪枝
        {
            sum[i] += w[u];
            dfs(u + 1, k);
            sum[i] -= w[u]; // 恢复现场
        }

    // 新开一辆车
    sum[k] = w[u];
    dfs(u + 1, k + 1);
    sum[k] = 0; // 恢复现场
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ ) cin >> w[i];

    // 优化搜索顺序
    sort(w, w + n);
    reverse(w, w + n);

    dfs(0, 0);

    cout << ans << endl;

    return 0;
}
```
##### AcWing166 数独
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 9, M = 1 << N;

int ones[M], map[M];
int row[N], col[N], cell[3][3];
char str[100];

void init()
{
    for (int i = 0; i < N; i ++ )
        row[i] = col[i] = (1 << N) - 1;

    for (int i = 0; i < 3; i ++ )
        for (int j = 0; j < 3; j ++ )
            cell[i][j] = (1 << N) - 1;
}

void draw(int x, int y, int t, bool is_set)
{
    if (is_set) str[x * N + y] = '1' + t;
    else str[x * N + y] = '.';

    int v = 1 << t;
    if (!is_set) v = -v;

    row[x] -= v;
    col[y] -= v;
    cell[x / 3][y / 3] -= v;
}

int lowbit(int x)
{
    return x & -x;
}

int get(int x, int y)
{
    return row[x] & col[y] & cell[x / 3][y / 3];
}

bool dfs(int cnt)
{
    if (!cnt) return true;

    int minv = 10;
    int x, y;
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            if (str[i * N + j] == '.')
            {
                int state = get(i, j);
                if (ones[state] < minv)
                {
                    minv = ones[state];
                    x = i, y = j;
                }
            }

    int state = get(x, y);
    for (int i = state; i; i -= lowbit(i))
    {
        int t = map[lowbit(i)];
        draw(x, y, t, true);
        if (dfs(cnt - 1)) return true;
        draw(x, y, t, false);
    }

    return false;
}

int main()
{
    for (int i = 0; i < N; i ++ ) map[1 << i] = i;
    for (int i = 0; i < 1 << N; i ++ )
        for (int j = 0; j < N; j ++ )
            ones[i] += i >> j & 1;

    while (cin >> str, str[0] != 'e')
    {
        init();

        int cnt = 0;
        for (int i = 0, k = 0; i < N; i ++ )
            for (int j = 0; j < N; j ++, k ++ )
                if (str[k] != '.')
                {
                    int t = str[k] - '1';
                    draw(i, j, t, true);
                }
                else cnt ++ ;

        dfs(cnt);

        puts(str);
    }

    return 0;
}
```
##### AcWing167 木棒
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 70;

int n;
int w[N];
int sum, length;
bool st[N];

bool dfs(int u, int cur, int start)
{
    if (u * length == sum) return true;
    if (cur == length) return dfs(u + 1, 0, 0);

    for (int i = start; i < n; i ++ )
    {
        if (st[i] || cur + w[i] > length) continue;

        st[i] = true;
        if (dfs(u, cur + w[i], i + 1)) return true;
        st[i] = false;

        if (!cur || cur + w[i] == length) return false;

        int j = i;
        while (j < n && w[j] == w[i]) j ++ ;
        i = j - 1;
    }

    return false;
}

int main()
{
    while (cin >> n, n)
    {
        memset(st, 0, sizeof st);
        sum = 0;

        for (int i = 0; i < n; i ++ )
        {
            cin >> w[i];
            sum += w[i];
        }

        sort(w, w + n);
        reverse(w, w + n);

        length = 1;
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
##### AcWing168 生日蛋糕
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
