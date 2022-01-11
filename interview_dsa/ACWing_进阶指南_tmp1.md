# 0x00 基本算法(40)

包括位运算、递推、递归、二分、排序、倍增、贪心等算法。

## 0x01位运算

   
##### AcWing89   a^b

```cpp
#include <iostream>
using namespace std;

int main()
{
    int a, b, p;
    cin >> a >> b >> p;

    int res = 1 % p;
    while (b)
    {
        if (b & 1) res = (long long)res * a % p;
        a = (long long)a * a % p;
        b >>= 1;
    }

    cout << res << endl;
    return 0;
}
```

##### AcWing90   64位整数乘法

```cpp
#include <iostream>
using namespace std;

typedef unsigned long long ULL;

int main() {
    ULL a, b, p;
    cin >> a >> b >> p;
    ULL res = 0;
    while (a) {
        if (a & 1) res = (res + b) % p;
        b = (b + b) % p;
        a >>= 1;
    }
    cout << res << endl;
    return 0;
}
```

##### AcWing91   最短Hamilton路径
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 20, M = 1 << 20;

int n;
int f[M][N], weight[N][N];

int main() {
    cin >> n;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            cin >> weight[i][j];

    memset(f, 0x3f, sizeof f);
    f[1][0] = 0;
    for (int i = 0; i < 1 << n; i ++ )
        for (int j = 0; j < n; j ++ )
            if (i >> j & 1) {
                for (int k = 0; k < n; k ++ )
                    if (i - (1 << j) >> k & 1) {
                        f[i][j] = min(f[i][j], f[i - (1 << j)][k] + weight[k][j]);
                    }
            }

    cout << f[(1 << n) - 1][n - 1] << endl;
    return 0;
}
```

##### AcWing998   起床困难综合症  


 

## 0x02递推与递归

##### AcWing92   递归实现指数型枚举
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

int n;

void dfs(int u, int state)
{
    if (u == n)
    {
        for (int i = 0; i < n; i ++ )
            if (state >> i & 1)
                cout << i + 1 << ' ';
        cout << endl;
        return;
    }

    dfs(u + 1, state);
    dfs(u + 1, state + (1 << u));
}

int main()
{
    cin >> n;
    dfs(0, 0);
    return 0;
}
```

##### AcWing93   递归实现组合型枚举
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

int n, m;

void dfs(int u, int s, int state)
{
    if (s == m)
    {
        for (int i = 0; i < n; i ++ )
            if (state >> i & 1)
                cout << i + 1 << ' ';
        cout << endl;
        return;
    }
    if (u == n) return;

    for (int i = u; i < n; i ++ )
    {
        dfs(i + 1, s + 1, state + (1 << i));
    }
}

int main()
{
    cin >> n >> m;
    dfs(0, 0, 0);
    return 0;
}
```

##### AcWing94   递归实现排列型枚举
```cpp
#include <cstring>
#include <vector>
#include <iostream>
#include <algorithm>

using namespace std;

int n;
vector<int> path;

void dfs(int u, int state)
{
    if (u == n)
    {
        for (auto x : path) cout << x << ' ';
        cout << endl;
        return;
    }

    for (int i = 0; i < n; i ++ )
        if (!(state >> i & 1))
        {
            path.push_back(i + 1);
            dfs(u + 1, state + (1 << i));
            path.pop_back();
        }
}

int main()
{
    cin >> n;
    dfs(0, 0);
    return 0;
}
```

##### AcWing95   费解的开关 
```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int INF = 100000;

char g[10][10];
int dx[5] = {0, -1, 0, 1, 0}, dy[5] = {0, 0, 1, 0, -1};

void turn(int x, int y)
{
    for (int i = 0; i < 5; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        if (a >= 0 && a < 5 && b >= 0 && b < 5)
        {
            g[a][b] = '0' + !(g[a][b] - '0');
        }
    }
}

int work()
{
    int ans = INF;
    for (int k = 0; k < 1 << 5; k ++ )
    {
        int res = 0;
        char backup[10][10];
        memcpy(backup, g, sizeof g);

        for (int j = 0; j < 5; j ++ )
        {
            if (k >> j & 1)
            {
                res ++ ;
                turn(0, j);
            }
        }

        for (int i = 0; i < 4; i ++ )
            for (int j = 0; j < 5; j ++ )
                if (g[i][j] == '0')
                {
                    res ++ ;
                    turn(i + 1, j);
                }

        bool is_successful = true;
        for (int j = 0; j < 5; j ++ )
            if (g[4][j] == '0')
            {
                is_successful = false;
                break;
            }

        if (is_successful) ans = min(ans, res);

        memcpy(g, backup, sizeof g);
    }

    if (ans > 6) return -1;
    return ans;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        for (int i = 0; i < 5; i ++ ) cin >> g[i];
        cout << work() << endl;
    }
    return 0;
}
```
##### AcWing96   奇怪的汉诺塔
```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 15;

int main()
{
    int d[N], f[N];

    memset(f, 0x3f, sizeof f);
    f[0] = 0;

    for (int i = 1; i <= 12; i ++ ) d[i] = (1 << i) - 1;

    for (int i = 1; i <= 12; i ++ )
        for (int j = 0; j < i; j ++ )
            f[i] = min(f[i], f[j] * 2 + d[i - j]);

    for (int i = 1; i <= 12; i ++ ) cout << f[i] << endl;
    return 0;
}
```
##### AcWing97   约数之和
```cpp
#include <iostream>

using namespace std;

const int mod = 9901;

int qmi(int a, int k)
{
    a %= mod;
    int res = 1;
    while (k)
    {
        if (k & 1) res = res * a % mod;
        a = a * a % mod;
        k >>= 1;
    }
    return res;
}

int sum(int p, int k)
{
    if (k == 0) return 1;
    if (k % 2 == 0) return (p % mod * sum(p, k - 1) % mod + 1) % mod;
    return sum(p, k / 2) % mod * (1 + qmi(p, k / 2 + 1)) % mod;
}

int main()
{
    int A, B;
    cin >> A >> B;

    int res = 1;
    for (int i = 2; i <= A; i ++ )
    {
        int s = 0;
        while (A % i == 0)
        {
            s ++ ;
            A /= i;
        }

        if (s) res = res * sum(i, s * B) % mod;
    }

    if (!A) res = 0;
    cout << res << endl;
    return 0;
}
```
##### AcWing98   分形之城  
```cpp
#include <iostream>
#include <cmath>

using namespace std;

typedef long long LL;
typedef pair<LL, LL> PLL;


PLL calc(LL n, LL m)
{
    if (!n) return {0, 0};
    LL len = 1ll << (n - 1), cnt = 1ll << (2 * n - 2);
    auto pos = calc(n - 1, m % cnt);
    auto x = pos.first, y = pos.second;
    auto z = m / cnt;
    if (z == 0) return {y, x};
    if (z == 1) return {x, y + len};
    if (z == 2) return {x + len, y + len};
    return {len * 2 - 1 - y, len - x - 1};
}


int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        LL N, A, B;
        cin >> N >> A >> B;
        auto ac = calc(N, A - 1);
        auto bc = calc(N, B - 1);
        double x = ac.first - bc.first, y = ac.second - bc.second;
        printf("%.0lf\n", sqrt(x * x + y * y) * 10);
    }
    return 0;
}
```
## 0x03 前缀和与差分

##### AcWing99   激光炸弹
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 5010;

int n, m;
int s[N][N];

int main()
{
    int cnt, R;
    cin >> cnt >> R;
    R = min(5001, R);

    n = m = 5001;
    while (cnt -- )
    {
        int x, y, w;
        cin >> x >> y >> w;
        x ++, y ++ ;
        s[x][y] += w;
    }

    // 预处理前缀和
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            s[i][j] += s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];

    int res = 0;

    // 枚举所有边长是R的矩形，枚举(i, j)为右下角
    for (int i = R; i <= n; i ++ )
        for (int j = R; j <= m; j ++ )
            res = max(res, s[i][j] - s[i - R][j] - s[i][j - R] + s[i - R][j - R]);

    cout << res << endl;

    return 0;
}
```
 
##### AcWing100   IncDec序列
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 500010;

int n;
int a[N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ ) cin >> a[i];
    for (int i = n; i; i -- ) a[i] -= a[i - 1];

    LL pos = 0, neg = 0;
    for (int i = 2; i <= n; i ++ )
        if (a[i] > 0) pos += a[i];
        else neg -= a[i];

    cout << min(pos, neg) + abs(pos - neg) << endl;
    cout << abs(pos - neg) + 1 << endl;

    return 0;
}
```
   
##### AcWing101   最高的牛
```cpp
#include <iostream>
#include <algorithm>
#include <set>

using namespace std;

const int N = 10010;

int d[N];

int main()
{
    int n, p, h, m;
    set<pair<int, int>> existed;
    cin >> n >> p >> h >> m;
    d[1] = h;
    for (int i = 0, a, b; i < m; i ++ )
    {
        cin >> a >> b;
        if (a > b) swap(a, b);
        if (!existed.count({a, b}))
        {
            existed.insert({a, b});
            d[a + 1] --, d[b] ++ ;
        }
    }

    for (int i = 1; i <= n; i ++ )
    {
        d[i] += d[i - 1];
        cout << d[i] << endl;
    }
    return 0;
}
```
## 0x04  二分和三分
 
##### AcWing102   最佳牛围栏
```cpp
#include <iostream>

using namespace std;

const int N = 100010;

int n, m;
int cows[N];
double sum[N];

bool check(double avg)
{
    for (int i = 1; i <= n; i ++ )
        sum[i] = sum[i - 1] + cows[i] - avg;

    double mins = 0;
    for (int i = m, j = 0; i <= n; i ++, j ++ )
    {
        mins = min(mins, sum[j]);
        if (sum[i] - mins >= 0) return true;
    }

    return false;
}

int main()
{
    cin >> n >> m;
    double l = 0, r = 0;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> cows[i];
        r = max(r, (double)cows[i]);
    }

    while (r - l > 1e-5)
    {
        double mid = (l + r) / 2;
        if (check(mid)) l = mid;
        else r = mid;
    }

    printf("%d\n", (int)(r * 1000));

    return 0;
}
```
   
##### AcWing113   特殊排序
```cpp
// Forward declaration of compare API.
// bool compare(int a, int b);
// return bool means whether a is less than b.

class Solution {
public:
    vector<int> specialSort(int n) {
        vector<int> res;
        res.push_back(1);
        for (int i = 2; i <= n; i ++ )
        {
            int l = 0, r = res.size() - 1;
            while (l < r)
            {
                int mid = l + r + 1 >> 1;
                if (compare(res[mid], i)) l = mid;
                else r = mid - 1;
            }
            res.push_back(i);
            for (int j = res.size() - 2; j > r; j -- ) swap (res[j], res[j + 1]);
            if (!compare(res[r], i)) swap(res[r], res[r + 1]);
        }
        return res;
    }
};
```
 

## 0x05 双指针与排序


##### AcWing103   电影
```cpp


```
 
##### AcWing104   货仓选址
```cpp


```
   
##### AcWing105   七夕祭
```cpp


```
  
##### AcWing106   动态中位数
```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <cstring>

using namespace std;

typedef pair<int,int> PII;

const int N = 10010;

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        int m, n;
        cin >> m >> n;
        priority_queue<int> max_heap;
        priority_queue<int, vector<int>, greater<int>> min_heap;

        printf("%d %d\n", m, (n + 1) / 2);
        int cnt = 0;
        for (int i = 0; i < n; i ++ )
        {
            int t;
            scanf("%d", &t);
            max_heap.push(t);
            if (min_heap.size() && min_heap.top() < max_heap.top())
            {
                auto a = min_heap.top(), b = max_heap.top();
                min_heap.pop(), max_heap.pop();
                min_heap.push(b), max_heap.push(a);
            }

            if (max_heap.size() > min_heap.size() + 1)
            {
                min_heap.push(max_heap.top());
                max_heap.pop();
            }

            if (!(i & 1))
            {
                printf("%d ", max_heap.top());
                if ( ++ cnt % 10 == 0) puts("");
            }
        }

        if (cnt % 10) puts("");
    }

    return 0;
}
```
   
##### AcWing107   超快速排序
```cpp


```
   
##### AcWing108   奇数码问题
```cpp


```
   

## 0x06 倍增

##### AcWing109   天才ACM
```cpp


```
  

## 0x07 贪心

     
##### AcWing110   防晒
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <map>

using namespace std;

typedef pair<int,int> PII;
const int N = 2510;

int n, m;
PII cows[N];

int main()
{
    cin >> n >> m;
    map<int, int> spfs;
    for (int i = 0; i < n; i ++ ) cin >> cows[i].first >> cows[i].second;
    for (int i = 0; i < m; i ++ )
    {
        int spf, cover;
        cin >> spf >> cover;
        spfs[spf] += cover;
    }
    sort(cows, cows + n);
    int res = 0;
    spfs[0] = spfs[1001] = n;
    for (int i = n - 1; i >= 0; i -- )
    {
        auto spf = spfs.upper_bound(cows[i].second);
        spf --;
        if (spf->first >= cows[i].first && spf->first <= cows[i].second)
        {
            res ++ ;
            if (--spf->second == 0)
                spfs.erase(spf);
        }
    }
    cout << res << endl;
    return 0;
}
```
   
##### AcWing111   畜栏预定
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

typedef pair<int,int> PII;
const int N = 50010;

int n;
int id[N];
pair<PII, int> cows[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
    {
        cin >> cows[i].first.first >> cows[i].first.second;
        cows[i].second = i;
    }

    sort(cows, cows + n);

    priority_queue<PII, vector<PII>, greater<PII>> heap;
    for (int i = 0; i < n; i ++ )
    {
        if (heap.empty() || heap.top().first >= cows[i].first.first)
        {
            PII stall = {cows[i].first.second, heap.size()};
            id[cows[i].second] = stall.second;
            heap.push(stall);
        }
        else
        {
            auto stall = heap.top();
            heap.pop();
            stall.first = cows[i].first.second;
            id[cows[i].second] = stall.second;
            heap.push(stall);
        }
    }

    cout << heap.size() << endl;
    for (int i = 0; i < n; i ++ ) cout << id[i] + 1 << endl;
    return 0;
}
```
   
##### AcWing112   雷达设备
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

typedef pair<double, double> PDD;
const int N = 1010;
const double eps = 1e-6, INF = 1e10;

int n, d;
PDD seg[N];

int main()
{
    cin >> n >> d;

    bool success = true;

    for (int i = 0; i < n; i ++ )
    {
        int x, y;
        cin >> x >> y;
        if (y > d)
        {
            success = false;
            break;
        }
        auto len = sqrt(d * d - y * y);
        seg[i] = {x + len, x - len};
    }

    if (!success) puts("-1");
    else
    {
        sort(seg, seg + n);
        int res = 0;
        double last = -INF;
        for (int i = 0; i < n; i ++ )
        {
            if (seg[i].second > last + eps)
            {
                res ++ ;
                last = seg[i].first;
            }
        }
        cout << res << endl;
    }
    return 0;
}
```
   
##### AcWing114   国王游戏
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef pair<int, int> PII;
const int N = 1010;

int n;
PII p[N];

vector<int> mul(vector<int>a, int b)
{
    vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size(); i ++ )
    {
        t += a[i] * b;
        c.push_back(t % 10);
        t /= 10;
    }
    while (t)
    {
        c.push_back(t % 10);
        t /= 10;
    }
    return c;
}

vector<int> div(vector<int>a, int b)
{
    vector<int> c;
    bool is_first = true;
    for (int i = a.size() - 1, t = 0; i >= 0; i -- )
    {
        t = t * 10 + a[i];
        int x = t / b;
        if (!is_first || x)
        {
            is_first = false;
            c.push_back(x);
        }
        t %= b;
    }
    reverse(c.begin(), c.end());
    return c;
}

vector<int> max_vec(vector<int> a, vector<int> b)
{
    if (a.size() > b.size()) return a;
    if (a.size() < b.size()) return b;
    if (vector<int>(a.rbegin(), a.rend()) > vector<int>(b.rbegin(), b.rend())) return a;
    return b;
}

int main()
{
    cin >> n;
    for (int i = 0; i <= n; i ++ )
    {
        int a, b;
        cin >> a >> b;
        p[i] = {a * b, a};
    }
    sort(p + 1, p + n + 1);

    vector<int> product(1, 1);

    vector<int> res(1, 0);
    for (int i = 0; i <= n; i ++ )
    {
        if (i) res = max_vec(res, div(product, p[i].first / p[i].second));
        product = mul(product, p[i].second);
    }

    for (int i = res.size() - 1; i >= 0; i -- ) cout << res[i];
    cout << endl;

    return 0;
}
```
   
##### AcWing115   给树染色
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace  std;

const int N = 1010;

int n, root;
struct Node
{
    int p, s, v;
    double avg;
}nodes[N];

int find()
{
    double avg = 0;
    int res = -1;
    for (int i = 1; i <= n; i ++ )
        if (i != root && nodes[i].avg > avg)
        {
            avg = nodes[i].avg;
            res = i;
        }
    return res;
}

int main()
{
    cin >> n >> root;
    int ans = 0;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> nodes[i].v;
        nodes[i].avg = nodes[i].v;
        nodes[i].s = 1;
        ans += nodes[i].v;
    }
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b;
        cin >> a >> b;
        nodes[b].p = a;
    }

    for (int i = 0; i < n - 1; i ++ )
    {
        int p = find();
        int father = nodes[p].p;
        ans += nodes[p].v * nodes[father].s;
        nodes[p].avg = -1;
        for (int j = 1; j <= n; j ++ )
            if (nodes[j].p == p)
                nodes[j].p = father;
        nodes[father].v += nodes[p].v;
        nodes[father].s += nodes[p].s;
        nodes[father].avg = (double)nodes[father].v / nodes[father].s;
    }

    cout << ans << endl;
    return 0;
}
```
