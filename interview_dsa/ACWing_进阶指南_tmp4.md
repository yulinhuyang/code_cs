##### AcWing153   双栈排序
```cpp
#include <iostream>
#include <algorithm>
#include <stack>
#include <cstring>

using namespace std;

const int N = 1010;

int n;
int a[N], f[N];
int color[N];
bool g[N][N];

bool dfs(int u, int c)
{
    color[u] = c;
    for (int i = 1; i <= n; i++)
        if (g[u][i])
        {
            if (color[i] == c) return false;
            if (color[i] == -1 && !dfs(i, !c)) return false;
        }

    return true;
}

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++) cin >> a[i];
    f[n + 1] = n + 1;
    memset(g, false, sizeof g);
    for (int i = n; i; i--) f[i] = min(f[i + 1], a[i]);

    for (int i = 1; i <= n; i++)
        for (int j = i + 1; j <= n; j++)
            if (a[i] < a[j] && f[j + 1] < a[i])
                g[i][j] = g[j][i] = true;

    memset(color, -1, sizeof color);

    bool flag = true;
    for (int i = 1; i <= n; i++)
        if (color[i] == -1 && !dfs(i, 0))
        {
            flag = false;
            break;
        }

    if (!flag)
    {
        cout << 0 << endl;
        return 0;
    }

    stack<int> stk1, stk2;

    int now = 1;
    for (int i = 1; i <= n; i++)
    {
        if (color[i] == 0)
        {
            // 要入第一个栈了，第一个栈该出的现在必须要出掉
            // 为了使字典序最小，第二个栈可以再等等
            while (stk1.size() && stk1.top() == now)
            {
                stk1.pop();
                cout << "b ";
                now++;
            }
            stk1.push(a[i]);
            cout << "a ";
        }
        else
        {
            // 要入第二个栈了，第二个栈该出的现在必须要出掉
            // 然而由于b比c字典序小，第一个栈可以出的也应该出掉
            while (true)
                if (stk1.size() && stk1.top() == now)
                {
                    stk1.pop();
                    cout << "b ";
                    now++;
                }
                else if (stk2.size() && stk2.top() == now)
                {
                    stk2.pop();
                    cout << "d ";
                    now++;
                }
                else break;
            stk2.push(a[i]);
            cout << "c ";
        }

    }
    // 最后出栈剩余的
    while (true)
        if (stk1.size() && stk1.top() == now)
        {
            stk1.pop();
            cout << "b ";
            now++;
        }
        else if (stk2.size() && stk2.top() == now)
        {
            stk2.pop();
            cout << "d ";
            now++;
        }
        else break;
    cout << endl;

    return 0;
}
```
 
##### AcWing154   滑动窗口
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int n, m;
int a[N];
int q[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);

    int hh = 0, tt = -1;
    for (int i = 0; i < n; i ++ )
    {
        if (hh <= tt && q[hh] <= i - m) hh ++ ;
        while (hh <= tt && a[q[tt]] >= a[i]) tt -- ;
        q[ ++ tt] = i;
        if (i >= m - 1) printf("%d ", a[q[hh]]);
    }
    puts("");

    hh = 0, tt = -1;
    for (int i = 0; i < n; i ++ )
    {
        if (hh <= tt && q[hh] <= i - m) hh ++ ;
        while (hh <= tt && a[q[tt]] <= a[i]) tt -- ;
        q[ ++ tt] = i;
        if (i >= m - 1) printf("%d ", a[q[hh]]);
    }
    puts("");

    return 0;
}
```

##### AcWing155   内存分配
```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <set>
#include <vector>

using namespace std;

typedef pair<int, int> PII;

int n;
queue<PII> waits;       // (first: 内存长度，second: 占用时间)
set<PII> runs;        // (first: 起始下标，sercond：长度)
priority_queue<PII, vector<PII>, greater<PII>> endts;       // (first: 释放时间，second: 起始下标)
int tm, cnt;

bool give(int t, int m, int p)
{
    for (auto it = runs.begin(); it != runs.end(); it ++ )
    {
        auto jt = it;
        jt ++ ;
        if (jt != runs.end())
        {
            if (m <= jt->first - (it->first + it->second - 1) - 1)
            {
                int start = it->first + it->second;
                runs.insert({start, m});
                endts.push({t + p, start});
                return true;
            }
        }
    }

    return false;
}

void finish(int t)
{
    while (endts.size() && endts.top().first <= t)
    {
        int f = endts.top().first;
        while (endts.size() && endts.top().first == f)
        {
            auto top = endts.top();
            endts.pop();
            auto it = runs.lower_bound({top.second, 0});
            runs.erase(it);
        }

        tm = f;
        while (waits.size())
        {
            auto front = waits.front();
            if (give(f, front.first, front.second))
            {
                waits.pop();
            }
            else break;
        }
    }
}

int main()
{
    cin >> n;
    int t, m, p;

    runs.insert({-1, 1}), runs.insert({n, 1});

    while (cin >> t >> m >> p, t || m || p)
    {
        finish(t);
        if (!give(t, m, p))
        {
            waits.push({m, p});
            cnt ++ ;
        }
    }

    finish(2e9);

    cout << tm << endl << cnt << endl;

    return 0;
}
```  
##### AcWing156   矩阵
```cpp
#include <iostream>
#include <algorithm>
#include <unordered_set>

using namespace std;

typedef unsigned long long ULL;

const int N = 1010, M = N * N, P = 131;

int n, m, a, b;
ULL hashv[N][N], p[M];
char str[N];

ULL calc(ULL f[], int l, int r)
{
    return f[r] - f[l - 1] * p[r - l + 1];
}

int main()
{
    scanf("%d%d%d%d", &n, &m, &a, &b);

    p[0] = 1;
    for (int i = 1; i <= n * m; i ++ ) p[i] = p[i - 1] * P;

    for (int i = 1; i <= n; i ++ )
    {
        scanf("%s", str + 1);
        for (int j = 1; j <= m; j ++ ) hashv[i][j] = hashv[i][j - 1] * P + str[j] - '0';
    }

    unordered_set<ULL> S;
    for (int i = b; i <= m; i ++ )
    {
        ULL s = 0;
        int l = i - b + 1, r = i;
        for (int j = 1; j <= n; j ++ )
        {
            s = s * p[b] + calc(hashv[j], l, r);
            if (j - a > 0) s -= calc(hashv[j - a], l, r) * p[a * b];
            if (j >= a) S.insert(s);
        }
    }

    int Q;
    scanf("%d", &Q);
    while (Q -- )
    {
        ULL s = 0;
        for (int i = 0; i < a; i ++ )
        {
            scanf("%s", str);
            for (int j = 0; j < b; j ++ ) s = s * P + str[j] - '0';
        }
        if (S.count(s)) puts("1");
        else puts("0");
    }

    return 0;
}
```   
##### AcWing157   树形地铁系统

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

string dfs(string &seq, int &u)
{
    u ++ ;
    vector<string> seqs;
    while (seq[u] == '0') seqs.push_back(dfs(seq, u));
    u ++ ;
    sort(seqs.begin(), seqs.end());
    string res = "0";
    for (auto &s : seqs) res += s;
    res += '1';
    return res;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        string a, b;
        cin >> a >> b;
        a = '0' + a + '1';
        b = '0' + b + '1';
        int ua = 0, ub = 0;
        auto ra = dfs(a, ua), rb = dfs(b, ub);
        if (ra == rb) puts("same");
        else puts("different");
    }
    return 0;
}
```   
##### AcWing158   项链
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <string.h>

using namespace std;

const int N = 2000000;

int n;
char a[N], b[N];

int get_min(char str[])
{
    int i = 0, j = 1;
    while (i < n && j < n)
    {
        int k = 0;
        while (k < n && str[i + k] == str[j + k]) k ++ ;
        if (k == n) break;
        if (str[i + k] > str[j + k]) i += k + 1;
        else j += k + 1;
        if (i == j) i ++ ;
    }
    int res = min(i, j);
    str[res + n] = 0;
    return res;
}

int main()
{
    scanf("%s%s", a, b);
    n = strlen(a);

    memcpy(a + n, a, n);
    memcpy(b + n, b, n);

    int ia = get_min(a), ib = get_min(b);

    if (strcmp(a + ia, b + ib)) puts("No");
    else
    {
        puts("Yes");
        puts(a + ia);
    }

    return 0;
}
```  
##### AcWing159   奶牛矩阵
```cpp
#include <iostream>
#include <algorithm>
#include <string.h>

using namespace std;

const int N = 10010, M = 80;

int n, m;
char str[N][M];
bool st[M];
int ne[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> str[i];
        for (int j = 1; j <= m; j ++ )
        {
            bool is_match = true;
            for (int k = j; k < m; k += j)
            {
                for (int u = 0; u < j && k + u < m; u ++ )
                    if (str[i][u] != str[i][k + u])
                    {
                        is_match = false;
                        break;
                    }
                if (!is_match) break;
            }
            if (!is_match) st[j] = true;
        }
    }

    int width;
    for (int i = 1; i <= m; i ++ )
        if (!st[i])
        {
            width = i;
            break;
        }

    for (int i = 1; i <= n; i ++ ) str[i][width] = 0;

    for (int j = 0, i = 2; i <= n; i ++ )
    {
        while (j && strcmp(str[j + 1], str[i])) j = ne[j];
        if (!strcmp(str[j + 1], str[i])) j ++ ;
        ne[i] = j;
    }

    int height = n - ne[n];

    cout << width * height << endl;

    return 0;
}

``` 
##### AcWing160   匹配统计
```cpp
#include <iostream>

using namespace std;

const int N = 200010;

int n, m, q;
char a[N], b[N];
int nxt[N];
int f[N];

int main()
{
    cin >> n >> m >> q;
    scanf("%s%s", a + 1, b + 1);

    for (int i = 2, j = 0; i <= m; i ++ )
    {
        while (j && b[i] != b[j + 1]) j = nxt[j];
        if (b[i] == b[j + 1]) j ++ ;
        nxt[i] = j;
    }

    for (int i = 1, j = 0; i <= n; i ++ )
    {
        while (j && a[i] != b[j + 1]) j = nxt[j];
        if (a[i] == b[j + 1]) j ++ ;
        f[j] ++ ;
    }

    for (int i = m; i; i -- ) f[nxt[i]] += f[i];

    while (q -- )
    {
        int x;
        cin >> x;
        cout << f[x] - f[x + 1] << endl;
    }

    return 0;
}
```   
##### AcWing161   电话列表
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n;
int son[N][10], idx;
bool f[N];

bool insert(char *str)
{
    int p = 0;
    bool has_new = false;
    bool has_found = false;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - '0';
        if (!son[p][u])
        {
            son[p][u] = ++ idx;
            has_new = true;
        }
        p = son[p][u];
        if (f[p]) has_found = true;
    }

    f[p] = true;

    return has_new && !has_found;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        cin >> n;
        memset(son, 0, sizeof son);
        memset(f, false, sizeof f);
        idx = 0;

        bool res = true;

        char str[20];
        for (int i = 0; i < n; i ++ )
        {
            cin >> str;
            if (!insert(str)) res = false;
        }

        if (res) puts("YES");
        else puts("NO");
    }

    return 0;
}
```   
##### AcWing162   黑盒子
```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

const int N = 30010;

int n, m;
int a[N], b[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ) cin >> a[i];
    for (int i = 1; i <= m; i ++ ) cin >> b[i];

    priority_queue<int> down;
    priority_queue<int, vector<int>, greater<int>> up;

    int k = 0;
    for (int i = 1, j = 1; i <= m; i ++ )
    {
        while (j <= b[i])
        {
            if (down.empty() || a[j] >= down.top()) up.push(a[j]);
            else
            {
                up.push(down.top());
                down.pop();
                down.push(a[j]);
            }
            j ++ ;
        }

        down.push(up.top());
        up.pop();
        cout << down.top() << endl;
    }

    return 0;
}
```   
##### AcWing163   生日礼物
```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

typedef pair<int, int> PII;

const int N = 100010;

int n, m;
int a[N], l[N], r[N];
bool st[N];

void remove(int p)
{
    // 从链表中删去

    l[r[p]] = l[p];
    r[l[p]] = r[p];

    // 从heap里删去
    st[p] = true;
}

int main()
{
    cin >> n >> m;

    int k = 1;
    for (int i = 0; i < n; i ++ )
    {
        int x;
        cin >> x;
        if ((long long)a[k] * x < 0) a[ ++ k] = x;
        else a[k] += x;
    }

    n = k;

    int cnt = 0, res = 0;
    for (int i = 1; i <= n; i ++ )
        if (a[i] > 0)
        {
            cnt ++ ;
            res += a[i];
        }

    priority_queue<PII, vector<PII>, greater<PII>> heap;

    for (int i = 1; i <= n; i ++ )
    {
        l[i] = i - 1;
        r[i] = i + 1;

        heap.push({abs(a[i]), i});
    }

    while (cnt > m)
    {
        while (st[heap.top().second]) heap.pop();

        auto t = heap.top();
        heap.pop();

        int v = t.first, p = t.second;

        if (l[p] != 0 && r[p] != n + 1 || a[p] > 0)
        {
            cnt -- ;
            res -= v;

            int left = l[p], right = r[p];
            a[p] += a[left] + a[right];

            heap.push({abs(a[p]), p});
            remove(left);
            remove(right);
        }
    }

    cout << res << endl;

    return 0;
}
```   


# 0x20 搜索(32)

包括树与图的遍历、深度优先搜索、剪枝、迭代加深、广度优先搜索、广搜变形、A*、IDA*等内容。


## 0x21 树与图的遍历

##### AcWing164   可达性统计
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <bitset>
#include <queue>

using namespace std;

const int N = 30010;

int n, m;
int h[N], e[N], ne[N], idx;
int d[N], seq[N];
bitset<N> f[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void topsort()
{
    queue<int> q;

    for (int i = 1; i <= n; i ++ )
        if (!d[i])
            q.push(i);

    int k = 0;
    while (q.size())
    {
        int t = q.front();
        q.pop();
        seq[k ++ ] = t;

        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (-- d[j] == 0)
                q.push(j);
        }
    }
}

int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        cin >> a >> b;
        add(a, b);
        d[b] ++ ;
    }

    topsort();

    for (int i = n - 1; i >= 0; i -- )
    {
        int j = seq[i];
        f[j][j] = 1;
        for (int k = h[j]; ~k; k = ne[k])
            f[j] |= f[e[k]];
    }

    for (int i = 1; i <= n; i ++ ) cout << f[i].count() << endl;

    return 0;
}
```

## 0x22 深度优先搜索

##### AcWing165   小猫爬山
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 20;

int n, m;
int sum[N], cat[N];
int ans = N;

void dfs(int u, int k)
{
    if (k > ans) return;

    if (u == n)
    {
        ans = k;
        return;
    }

    for (int i = 0; i < k; i ++ )
        if (sum[i] + cat[u] <= m)
        {
            sum[i] += cat[u];
            dfs(u + 1, k);
            sum[i] -= cat[u];
        }

    sum[k] = cat[u];
    dfs(u + 1, k + 1);
    sum[k] = 0;
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ ) cin >> cat[i];

    sort(cat, cat + n);
    reverse(cat, cat + n);

    dfs(0, 0);

    cout << ans << endl;

    return 0;
}
```
##### AcWing166   数独  
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 9;

int ones[1 << N], map[1 << N];
int row[N], col[N], cell[3][3];
char str[100];

inline int lowbit(int x)
{
    return x & -x;
}

void init()
{
    for (int i = 0; i < N; i ++ ) row[i] = col[i] = (1 << N) - 1;
    for (int i = 0; i < 3; i ++ )
        for (int j = 0; j < 3; j ++ )
            cell[i][j] = (1 << N) - 1;
}

// 求可选方案的交集
inline int get(int x, int y)
{
    return row[x] & col[y] & cell[x / 3][y / 3];
}

bool dfs(int cnt)
{
    if (!cnt) return true;

    // 找出可选方案数最少的空格
    int minv = 10;
    int x, y;
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            if (str[i * 9 + j] == '.')
            {
                int t = ones[get(i, j)];
                if (t < minv)
                {
                    minv = t;
                    x = i, y = j;
                }
            }

    for (int i = get(x, y); i; i -= lowbit(i))
    {
        int t = map[lowbit(i)];

        // 修改状态
        row[x] -= 1 << t;
        col[y] -= 1 << t;
        cell[x / 3][y / 3] -= 1 << t;
        str[x * 9 + y] = '1' + t;

        if (dfs(cnt - 1)) return true;

        // 恢复现场
        row[x] += 1 << t;
        col[y] += 1 << t;
        cell[x / 3][y / 3] += 1 << t;
        str[x * 9 + y] = '.';
    }

    return false;
}

int main()
{
    for (int i = 0; i < N; i ++ ) map[1 << i] = i;
    for (int i = 0; i < 1 << N; i ++ )
    {
        int s = 0;
        for (int j = i; j; j -= lowbit(j)) s ++ ;
        ones[i] = s; // i的二进制表示中有s个1
    }

    while (cin >> str, str[0] != 'e')
    {
        init();

        int cnt = 0;
        for (int i = 0, k = 0; i < N; i ++ )
            for (int j = 0; j < N; j ++ , k ++ )
                if (str[k] != '.')
                {
                    int t = str[k] - '1';
                    row[i] -= 1 << t;
                    col[j] -= 1 << t;
                    cell[i / 3][j / 3] -= 1 << t;
                }
                else cnt ++ ;

        dfs(cnt);

        cout << str << endl;
    }

    return 0;
}
```
