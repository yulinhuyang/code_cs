
## 0x08 总结与练习

##### AcWing116   飞行员兄弟
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef pair<int,int> PII;
const int N = 4, INF = 100;

int change[N][N];

int get(int x, int y)
{
    return x * N + y;
}

int main()
{
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
        {
            for (int k = 0; k < N; k ++ ) change[i][j] += (1 << get(i, k)) + (1 << get(k, j));
            change[i][j] -= 1 << get(i, j);
        }

    int state = 0;
    for (int i = 0; i < N; i ++ )
    {
        string line;
        cin >> line;
        for (int j = 0; j < N; j ++ )
            if (line[j] == '+')
                state += 1 << get(i, j);
    }

    vector<PII> path, temp;
    for (int i = 0; i < 1 << 16; i ++ )
    {
        int now = state;
        temp.clear();
        for (int j = 0; j < 16; j ++ )
            if (i >> j & 1)
            {
                int x = j / 4, y = j % 4;
                now ^= change[x][y];
                temp.push_back({x, y});
            }
        if (!now && (path.empty() || path.size() > temp.size())) path = temp;
    }

    cout << path.size() << endl;
    for (auto &p : path)
        cout << p.first + 1 << ' ' << p.second + 1 << endl;

    return 0;
}
```
##### AcWing117   占卜DIY
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

vector<int> closed[14];
int open[14];

int main()
{
    for (int i = 1; i <= 13; i ++ )
    {
        for (int j = 0; j < 4; j ++ )
        {
            int x;
            char s[2];
            cin >> s;
            if (*s >= '2' && *s <= '9') x = *s - '0';
            else if (*s == '0') x = 10;
            else if (*s == 'A') x = 1;
            else if (*s == 'J') x = 11;
            else if (*s == 'Q') x = 12;
            else x = 13;
            closed[i].push_back(x);
        }
    }

    for (int i = 0; i < 4; i ++ )
    {
        int t = closed[13][i];
        while (t != 13)
        {
            open[t] ++ ;
            int r = t;
            t = closed[r].back();
            closed[r].pop_back();
        }
    }

    int res = 0;
    for (int i = 1; i <= 12; i ++ ) res += open[i] >= 4;
    cout << res << endl;

    return 0;
}
```  
##### AcWing118   分形
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

char g[N][N];

void dfs(int n)
{
    if (n == 1)
    {
        g[0][0] = 'X';
        return;
    }

    dfs(n - 1);

    int len = 1;
    for (int i = 0; i < n - 2; i ++ ) len *= 3;
    int sx[4] = {0, 1, 2, 2}, sy[4] = {2, 1, 0, 2};
    for (int k = 0; k < 4; k ++ )
        for (int i = 0; i < len; i ++ )
            for (int j = 0; j < len; j ++ )
                g[sx[k] * len + i][sy[k] * len + j] = g[i][j];
}

int main()
{
    dfs(7);
    int n;
    while (cin >> n, n != -1)
    {
        int k = 1;
        while ( -- n) k *= 3;

        for (int i = 0; i < k; i ++ )
        {
            for (int j = 0; j < k; j ++ )
                if (g[i][j]) cout << g[i][j];
                else cout << ' ';
            cout << endl;
        }
        cout << '-' << endl;
    }
    return 0;
}
```  
##### AcWing119   袭击
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>

using namespace std;

const int N = 200010, INF = 1e10;

struct Point
{
    double x, y;
    bool type;

    bool operator< (const Point &W)const
    {
        return x < W.x;
    }
};

Point points[N], temp[N];

double dist(Point a, Point b)
{
    if (a.type == b.type) return INF;
    double dx = a.x - b.x, dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

double dfs(int l, int r)
{
    if (l >= r) return INF;

    int mid = l + r >> 1;
    double mid_x = points[mid].x;
    double res = min(dfs(l, mid), dfs(mid + 1, r));

    {
        int k = 0, i = l, j = mid + 1;
        while (i <= mid && j <= r)
            if (points[i].y < points[j].y) temp[k ++ ] = points[i ++ ];
            else temp[k ++ ] = points[j ++ ];
        while (i <= mid) temp[k ++ ] = points[i ++ ];
        while (j <= r) temp[k ++ ] = points[j ++ ];

        for (i = 0, j = l; i < k; i ++, j ++ ) points[j] = temp[i];
    }

    int k = 0;
    for (int i = l; i <= r; i ++ )
        if (points[i].x >= mid_x - res && points[i].x <= mid_x + res)
            temp[k ++ ] = points[i];

    for (int i = 0; i < k; i ++ )
        for (int j = i - 1; j >= 0 && temp[i].y - temp[j].y < res; j -- )
            res = min(res, dist(temp[i], temp[j]));

    return res;
}

int main()
{
    int T, n;
    cin >> T;

    while (T -- )
    {
        scanf("%d", &n);
        for (int i = 0; i < n; i ++ )
        {
            scanf("%lf%lf", &points[i].x, &points[i].y);
            points[i].type = 0;
        }
        for (int i = n; i < 2 * n; i ++ )
        {
            scanf("%lf%lf", &points[i].x, &points[i].y);
            points[i].type = 1;
        }

        sort(points, points + n * 2);

        printf("%.3lf\n", dfs(0, n * 2 - 1));
    }

    return 0;
}
```  
##### AcWing120   防线
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;
const int N = 200010;

struct Seq
{
    int s, e, d;
}seqs[N];

int n;

LL get_sum(int x)
{
    LL res = 0;
    for (int i = 0; i < n; i ++ )
        if (seqs[i].s <= x)
            res += (min(seqs[i].e, x) - seqs[i].s) / seqs[i].d + 1;
    return res;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        int l = 0, r = 0;
        scanf("%d", &n);
        for (int i = 0; i < n; i ++ )
        {
            int s, e, d;
            scanf("%d%d%d", &s, &e, &d);
            seqs[i] = {s, e, d};
            r = max(r, e);
        }

        while (l < r)
        {
            int mid = (LL)l + r >> 1;
            if (get_sum(mid) & 1) r = mid;
            else l = mid + 1;
        }

        auto sum = get_sum(r) - get_sum(r - 1);
        if (sum % 2) printf("%d %lld\n", r, sum);
        else puts("There's no weakness.");
    }

    return 0;
}
```  
##### AcWing121   赶牛入圈
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef pair<int,int> PII;
const int N = 1010;

int n, m;
PII points[N];
int sum[N][N];
vector<int> numbers;

bool check(int len)
{
    for (int x1 = 0, x2 = 1; x2 < numbers.size(); x2 ++ )
    {
        while (numbers[x2] - numbers[x1 + 1] + 1 > len) x1 ++ ;
        for (int y1 = 0, y2 = 1; y2 < numbers.size(); y2 ++ )
        {
            while (numbers[y2] - numbers[y1 + 1] + 1 > len) y1 ++ ;
            if (sum[x2][y2] - sum[x1][y2] - sum[x2][y1] + sum[x1][y1] >= m)
                return true;
        }
    }
    return false;
}

int get(int x)
{
    int l = 0, r = numbers.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (numbers[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r;
}

int main()
{
    cin >> m >> n;
    numbers.push_back(0);
    for (int i = 0; i < n; i ++ )
    {
        int x, y;
        cin >> x >> y;
        numbers.push_back(x);
        numbers.push_back(y);
        points[i] = {x, y};
    }
    sort(numbers.begin(), numbers.end());
    numbers.erase(unique(numbers.begin(), numbers.end()), numbers.end());

    for (int i = 0; i < n; i ++ )
    {
        int x = get(points[i].first), y = get(points[i].second);
        sum[x][y] ++ ;
    }

    for (int i = 1; i < numbers.size(); i ++ )
        for (int j = 1; j < numbers.size(); j ++ )
            sum[i][j] += sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1];

    int l = 1, r = 10000;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }

    cout << r << endl;

    return 0;
}
```  
##### AcWing122   糖果传递
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;
const int N = 1000010;

int n;
LL a[N];

int main()
{
    scanf("%d", &n);
    LL sum = 0;
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%lld", &a[i]);
        sum += a[i];
    }
    sum /= n;
    for (int i = n; i > 1; i -- )
    {
        a[i] = a[i] - sum + a[i + 1];
    }
    a[1] = 0;
    sort(a + 1, a + n + 1);

    LL res = 0;
    for (int i = 1; i <= n; i ++ ) res += abs(a[i] - a[(n + 1) / 2]);
    cout << res << endl;

    return 0;
}
```  
##### AcWing123   士兵
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010;

int n;
int x[N], y[N];

int work(int *q)
{
    sort(q, q + n);
    int res = 0;
    for (int i = 0; i < n; i ++ ) res += abs(q[i] - q[n / 2]);
    return res;
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> x[i] >> y[i];
    sort (x, x + n);
    for (int i = 0; i < n; i ++ ) x[i] -= i;

    cout << work(x) + work(y) << endl;

    return 0;
}
```  
##### AcWing124   数的进制转换
```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        int a, b;
        string line;
        cin >> a >> b >> line;
        vector<int> number;
        for (auto c : line)
        {
            if (c <= '9') number.push_back(c - '0');
            else if (c <= 'Z') number.push_back(c - 'A' + 10);
            else number.push_back(c - 'a' + 36);
        }
        reverse(number.begin(), number.end());
        vector<int> res;
        while (number.size())
        {
            int t = 0;
            for (int i = number.size() - 1; i >= 0; i -- )
            {
                number[i] += t * a;
                t = number[i] % b;
                number[i] /= b;
            }
            res.push_back(t);
            while (number.size() && !number.back()) number.pop_back();
        }
        reverse(res.begin(), res.end());
        string b_line;
        for (auto x : res)
        {
            if (x <= 9) b_line += char('0' + x);
            else if (x <= 35) b_line += char('A' + x - 10);
            else b_line += char('a' + x - 36);
        }
        cout << a << ' ' << line << endl;
        cout << b << ' ' << b_line << endl;
        cout << endl;
    }
    return 0;
}
```  
##### AcWing125   耍杂技的牛
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;
typedef pair<LL, LL> PLL;

const int N = 50010;

int n;
PLL cows[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
    {
        int w, s;
        cin >> w >> s;
        cows[i] = {w + s, w};
    }
    sort(cows, cows + n);

    LL sum = 0, res = -1e18;
    for (int i = 0; i < n; i ++ )
    {
        res = max(res, sum - (cows[i].first - cows[i].second));
        sum += cows[i].second;
    }
    cout << res << endl;
    return 0;
}
```  
##### AcWing126   最大的和
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 110;

int n;
int g[N][N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
        {
            cin >> g[i][j];
            g[i][j] += g[i - 1][j];
        }

    int res = INT_MIN;
    for (int i = 1; i <= n; i ++ )
        for (int j = i; j <= n; j ++ )
        {
            int last = 0;
            for (int k = 1; k <= n; k ++ )
            {
                last = max(last, 0) + g[j][k] - g[i - 1][k];
                res = max(res, last);
            }
        }

    cout << res << endl;
    return 0;
}
```  
##### AcWing127   任务
```cpp
#include <iostream>
#include <algorithm>
#include <set>

using namespace std;

typedef long long LL;
typedef pair<int, int> PII;

const int N = 100010;

int n, m;
PII mchs[N], tasks[N];

int main()
{
    while (cin >> n >> m)
    {
        for (int i = 0; i < n; i ++ ) cin >> mchs[i].first >> mchs[i].second;
        for (int i = 0; i < m; i ++ ) cin >> tasks[i].first >> tasks[i].second;
        sort(mchs, mchs + n);
        sort(tasks, tasks + m);
        multiset<int> ys;
        LL cnt = 0, res = 0;
        for (int i = m - 1, j = n - 1; i >= 0; i -- )
        {
            while (j >= 0 && mchs[j].first >= tasks[i].first) ys.insert(mchs[j -- ].second);
            auto it = ys.lower_bound(tasks[i].second);
            if (it != ys.end())
            {
                cnt ++ ;
                res += 500 * tasks[i].first + 2 * tasks[i].second;
                ys.erase(it);
            }
        }
        cout << cnt << ' ' << res << endl;
    }
    return 0;
}
```  


# 0x10 基本数据结构完成情况(37)

包括栈、队列、链表与邻接表、Hash、字符串、Trie、二叉堆等内容。

## 0x11 栈/单调栈

##### AcWing41   包含min函数的栈
```cpp
class MinStack {
public:
    /** initialize your data structure here. */

    stack<int> stk, stk_min;

    MinStack() {

    }

    void push(int x) {
        stk.push(x);
        if (stk_min.size()) x = min(x, stk_min.top());
        stk_min.push(x);
    }

    void pop() {
        stk.pop();
        stk_min.pop();
    }

    int top() {
        return stk.top();
    }

    int getMin() {
        return stk_min.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```
##### AcWing128   编辑器
```cpp
#include <iostream>
#include <limits.h>

using namespace std;

const int N = 1000010;

int stkl[N], stkr[N], topl, topr;
int f[N], sum[N];

void add(int x)
{
    stkl[ ++ topl] = x;
    sum[topl] = sum[topl - 1] + x;
    f[topl] = max(f[topl - 1], sum[topl]);
}

int main()
{
    int n;
    scanf("%d", &n);
    char ops[2];
    f[0] = INT_MIN;
    while (n -- )
    {
        int x;
        scanf("%s", ops);
        if (*ops == 'I')
        {
            scanf("%d", &x);
            add(x);
        }
        else if (*ops == 'D')
        {
            if (topl) topl -- ;
        }
        else if (*ops == 'L')
        {
            if (topl) stkr[ ++ topr] = stkl[topl -- ];
        }
        else if (*ops == 'R')
        {
            if (topr) add(stkr[topr -- ]);
        }
        else
        {
            scanf("%d", &x);
            printf("%d\n", f[x]);
        }
    }
    return 0;
}
```  
##### AcWing129   火车进栈
```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <stack>

using namespace std;

int n, remain = 20;
vector<int> path;
stack<int> stk;

void dfs(int u)
{
    if (!remain) return;
    if (path.size() == n)
    {
        remain -- ;
        for (auto x : path) cout << x;
        cout << endl;
        return;
    }
    if (stk.size())
    {
        path.push_back(stk.top());
        stk.pop();
        dfs(u);
        stk.push(path.back());
        path.pop_back();
    }
    if (u <= n)
    {
        stk.push(u);
        dfs(u + 1);
        stk.pop();
    }
}

int main()
{
    cin >> n;
    dfs(1);
    return 0;
}

```  
##### AcWing130   火车进出栈问题
```cpp
#include <iostream>
#include <vector>

using namespace std;

typedef long long LL;
const int N = 6000010, M = 120010;


LL res[N], tt;
int q[M];
bool st[M];

void multi(int b)
{
    LL t = 0;
    for (int i = 0; i <= tt; i ++ )
    {
        res[i] = res[i] * b + t;
        t = res[i] / 1000000000;
        res[i] %= 1000000000;
    }
    while (t)
    {
        res[++tt] = t % 1000000000;
        t /= 1000000000;
    }
}

void out()
{
    printf("%lld", res[tt]);
    for (int i = tt - 1; i >= 0; i -- ) printf("%09lld", res[i]);
    cout << endl;
}

int get(int n, int p)
{
    int s = 0;
    while (n) s += n / p, n /= p;
    return s;
}

int main()
{
    int n;
    cin >> n;
    for (int i = 2; i <= 2 * n; i ++ )
        for (int j = i + i; j <= 2 * n; j += i)
            st[j] = true;

    for (int i = 2; i <= n * 2; i ++ )
        if (!st[i])
        {
            q[i] = get(n * 2, i) - get(n * 2 - n, i) * 2;
        }

    int k = n + 1;
    for (int i = 2; i <= k; i ++ )
        while (k % i == 0)
        {
            k /= i;
            q[i] -- ;
        }

    res[0] = 1;

    for (int i = 2; i <= n * 2; i ++ )
        while (q[i] -- )
            multi(i);

    out();

    return 0;
}
```  
##### AcWing131   直方图中最大的矩形
```cpp
#include <iostream>
#include <algorithm>
#include <stack>
#include <limits.h>

using namespace std;

typedef long long LL;
const int N = 100010;

int n;
int h[N], l[N], r[N];

void get(int *last)
{
    stack<int> stk;
    h[0] = -1;
    stk.push(0);
    for (int i = 1; i <= n; i ++ )
    {
        while (h[stk.top()] >= h[i]) stk.pop();
        last[i] = stk.top() + 1;
        stk.push(i);
    }
}

int main()
{
    while (cin >> n, n)
    {
        for (int i = 1; i <= n; i ++ ) scanf("%d", &h[i]);

        get(l);
        reverse(h + 1, h + 1 + n);
        get(r);

        LL res = 0;
        for (int i = 1, j = n; i <= n; i ++, j -- )
            res = max(res, (LL)h[i] * (n - l[j] + 1 - r[i] + 1));
        printf("%lld\n", res);
    }
    return 0;
}
```  

## 0x12 队列/单调队列

##### AcWing132   小组队列 
```cpp
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 1010, M = 1000010;

int teamid[M];

int main()
{
    int n, C = 1;
    while (cin >> n, n)
    {
        queue<int> team;
        queue<int> person[N];
        printf("Scenario #%d\n", C ++ );
        for (int i = 0; i < n; i ++ )
        {
            int cnt;
            cin >> cnt;
            while (cnt -- )
            {
                int x;
                cin >> x;
                teamid[x] = i;
            }
        }

        string command;
        while (cin >> command, command != "STOP")
        {
            if (command == "ENQUEUE")
            {
                int x;
                cin >> x;
                int tid = teamid[x];
                if (person[tid].empty()) team.push(tid);
                person[tid].push(x);
            }
            else
            {
                int tid = team.front();
                auto &q = person[tid];
                cout << q.front() << endl;
                q.pop();
                if (q.empty()) team.pop();
            }
        }
        cout << endl;
    }

    return 0;
}
``` 
##### AcWing133   蚯蚓
```cpp
#include <iostream>
#include <algorithm>
#include <limits.h>

using namespace std;

const int N = 100010, M = 7000010;

int n, m, q, u, v, t;
int q1[N], q2[M], q3[M];
int delta;
int hh1, hh2, hh3, tt1, tt2 = -1, tt3 = -1;

int get_max()
{
    int x = INT_MIN;
    if (hh1 <= tt1) x = max(x, q1[hh1]);
    if (hh2 <= tt2) x = max(x, q2[hh2]);
    if (hh3 <= tt3) x = max(x, q3[hh3]);
    if (hh1 <= tt1 && x == q1[hh1]) hh1 ++ ;
    else if (hh2 <= tt2 && x == q2[hh2]) hh2 ++ ;
    else hh3 ++ ;
    return x;
}

int main()
{
    scanf("%d%d%d%d%d%d", &n, &m, &q, &u, &v, &t);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q1[i]);
    sort(q1, q1 + n);
    reverse(q1, q1 + n);
    tt1 = n - 1;
    for (int i = 1; i <= m; i ++ )
    {
        int x = get_max();
        x += delta;
        if (i % t == 0) printf("%d ", x);
        int left = x * 1ll * u / v;
        int right = x - left;
        delta += q;
        left -= delta, right -= delta;
        q2[ ++ tt2] = left, q3[ ++ tt3] = right;
    }
    puts("");

    for (int i = 1; i <= n + m; i ++ )
    {
        int x = get_max();
        if (i % t== 0) printf("%d ", x + delta);
    }
    puts("");
    return 0;
}
``` 
##### AcWing134   双端队列
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;
const int N = 200010;

int n;
PII a[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
    {
        cin >> a[i].first;
        a[i].second = i;
    }
    sort(a, a + n);
    int res = 1;
    for (int i = 0, last = n + 1, dir = -1; i < n; )
    {
        int j = i;
        while (j < n && a[j].first == a[i].first) j ++ ;
        int minx = a[i].second, maxx = a[j - 1].second;
        if (dir == -1)
        {
            if (last > maxx) last = minx;
            else dir = 1, last = maxx;
        }
        else
        {
            if (last < minx) last = maxx;
            else
            {
                res ++ ;
                last = minx;
                dir = -1;
            }
        }
        i = j;
    }

    cout << res << endl;

    return 0;
}
```  
##### AcWing135   最大子序和
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 300010, INF = 0x3f3f3f3f;

int n, m;
int s[N], q[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d", &s[i]);
        s[i] += s[i - 1];
    }

    int res = -INF;
    int hh = 0, tt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        if (q[hh] < i - m) hh ++ ;
        res = max(res, s[i] - s[q[hh]]);
        while (hh <= tt && s[q[tt]] >= s[i]) tt -- ;
        q[ ++ tt] = i;
    }

    printf("%d\n", res);

    return 0;
}
```  

## 0x13 链表与邻接表

##### AcWing136   邻值查找
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;
typedef pair<LL, int> PII;
const int N = 100010;

int n;
int l[N], r[N];
int p[N];
PII a[N], ans[N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> a[i].first;
        a[i].second = i;
    }
    sort(a + 1, a + 1 + n);

    a[0].first = -3e9, a[n + 1].first = 3e9;
    for (int i = 1; i <= n; i ++ )
    {
        l[i] = i - 1, r[i] = i + 1;
        p[a[i].second] = i;
    }

    for (int i = n; i > 1; i -- )
    {
        int j = p[i], left = l[j], right = r[j];
        LL left_value = abs(a[left].first - a[j].first);
        LL right_value = abs(a[right].first - a[j].first);
        if (left_value <= right_value) ans[i] = {left_value, a[left].second};
        else ans[i] = {right_value, a[right].second};
        l[right] = left, r[left] = right;
    }

    for (int i = 2; i <= n; i ++ ) cout << ans[i].first << ' ' << ans[i].second << endl;

    return 0;
}
```  
