##### AcWing186   巴士
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef pair<int, int> PII;

const int N = 2000, M = 60;

int n;
int bus[M];
vector<pair<int, PII>> routes;

bool check(int a, int d)
{
    for (int i = a; i < 60; i += d)
        if (!bus[i])
            return false;
    return true;
}

bool dfs(int depth, int sum, int start)
{
    if (!depth) return sum == n;

    // 枚举选哪个路线
    for (int i = start; i < routes.size(); i ++ )
    {
        auto r = routes[i];
        int a = r.second.first, d = r.second.second;
        if (r.first * depth + sum < n) continue;
        if (!check(a, d)) continue;
        for (int j = a; j < 60; j += d) bus[j] -- ;
        if (dfs(depth - 1, sum + r.first, i)) return true;
        for (int j = a; j < 60; j += d) bus[j] ++ ;
    }


    return false;
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
    {
        int t;
        scanf("%d", &t);
        bus[t] ++ ;
    }

    for (int a = 0; a < 60; a ++ )
        for (int d = a + 1; a + d < 60; d ++ )
            if (check(a, d))
                routes.push_back({(59 - a) / d + 1, {a, d}});

    sort(routes.begin(), routes.end(), greater<pair<int, PII>>());

    int depth = 0;
    while (!dfs(depth, 0, 0)) depth ++ ;

    printf("%d\n", depth);

    return 0;
}
```

 
##### AcWing187   导弹防御系统
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 60;

int n;
int h[N];
int up[N], down[N];

bool dfs(int depth, int u, int su, int sd)
{
    if (su + sd > depth) return false;
    if (u == n) return true;

    // 枚举放到上升子序列中的情况
    bool flag = false;
    for (int i = 1; i <= su; i ++ )
        if (up[i] < h[u])
        {
            int t = up[i];
            up[i] = h[u];
            if (dfs(depth, u + 1, su, sd)) return true;
            up[i] = t;
            flag = true;
            break;
        }
    if (!flag)
    {
        up[su + 1] = h[u];
        if (dfs(depth, u + 1, su + 1, sd)) return true;
    }

    // 枚举放到下降子序列中的情况
    flag = false;
    for (int i = 1; i <= sd; i ++ )
        if (down[i] > h[u])
        {
            int t = down[i];
            down[i] = h[u];
            if (dfs(depth, u + 1, su, sd)) return true;
            down[i] = t;
            flag = true;
            break;
        }
    if (!flag)
    {
        down[sd + 1] = h[u];
        if (dfs(depth, u + 1, su, sd + 1)) return true;
    }

    return false;
}

int main()
{
    while (cin >> n, n)
    {
        for (int i = 0; i < n; i ++ ) cin >> h[i];

        int depth = 0;
        while (!dfs(depth, 0, 0, 0)) depth ++ ;

        cout << depth << endl;
    }

    return 0;
}
```
  
##### AcWing188   武士风度的牛
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 155;

int n, m;
char g[N][N];
int dist[N][N];

int bfs(PII start, PII end)
{
    queue<PII> q;
    memset(dist, -1, sizeof dist);
    dist[start.x][start.y] = 0;

    q.push(start);

    int dx[] = {-2, -1, 1, 2, 2, 1, -1, -2};
    int dy[] = {1, 2, 2, 1, -1, -2, -2, -1};

    while (q.size())
    {
        auto t = q.front();
        q.pop();
        for (int i = 0; i < 8; i ++ )
        {
            int x = t.x + dx[i], y = t.y + dy[i];
            if (x < 0 || x >= n || y < 0 || y >= m) continue;
            if (g[x][y] == '*') continue;
            if (dist[x][y] != -1) continue;
            dist[x][y] = dist[t.x][t.y] + 1;

            if (make_pair(x, y) == end) return dist[x][y];

            q.push({x, y});
        }
    }

    return -1;
}

int main()
{
    cin >> m >> n;
    for (int i = 0; i < n; i ++ ) cin >> g[i];

    PII start, end;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == 'K') start = {i, j};
            else if (g[i][j] == 'H') end = {i, j};

    cout << bfs(start, end) << endl;

    return 0;
}
```  
##### AcWing189   乳草的入侵
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 110;

int n, m;
PII start;
char g[N][N];
int dist[N][N];

int bfs()
{
    memset(dist, -1, sizeof dist);
    queue<PII> q;
    q.push(start);
    dist[start.x][start.y] = 0;

    int res = 0;
    while (q.size())
    {
        auto t = q.front();
        q.pop();

        for (int x = t.x - 1; x <= t.x + 1; x ++ )
            for (int y = t.y - 1; y <= t.y + 1; y ++ )
                if (x != t.x || y != t.y)
                {
                    if (x < 1 || x > n || y < 1 || y > m) continue;
                    if (g[x][y] == '*' || dist[x][y] != -1) continue;
                    dist[x][y] = dist[t.x][t.y] + 1;
                    res = max(res, dist[x][y]);
                    q.push({x, y});
                }
    }

    return res;
}

int main()
{
    cin >> m >> n >> start.y >> start.x;
    start.x = n + 1 - start.x;

    for (int i = 1; i <= n; i ++ ) cin >> g[i] + 1;

    cout << bfs() << endl;

    return 0;
}

```  
##### AcWing190   字串变换

```cpp
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
##### AcWing191   天气预报
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 366;

int n;
bool st[N][3][3][7][7][7][7];
struct Node
{
    int day, x, y, s0, s1, s2, s3;
};
int state[N][4][4];

int bfs()
{
    if (state[1][1][1] || state[1][1][2] || state[1][2][1] || state[1][2][2]) return 0;

    queue<Node> q;
    memset(st, 0, sizeof st);
    q.push({1, 1, 1, 1, 1, 1, 1});
    st[1][1][1][1][1][1][1] = true;

    int dx[] = {-1, 0, 1, 0, 0}, dy[] = {0, 1, 0, -1, 0};

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        if (t.day == n) return 1;

        for (int i = 0; i < 5; i ++ )
            for (int j = 1; j <= 2; j ++ )
            {
                int x = t.x + dx[i] * j, y = t.y + dy[i] * j;
                if (x < 0 || x >= 3 || y < 0 || y >= 3) continue;

                auto& s = state[t.day + 1];
                if (s[x][y] || s[x][y + 1] || s[x + 1][y] || s[x + 1][y + 1]) continue;

                int s0 = t.s0, s1 = t.s1, s2 = t.s2, s3 = t.s3;
                if (!x && !y) s0 = 0;
                else if ( ++ s0 == 7) continue;
                if (!x && y == 2) s1 = 0;
                else if ( ++ s1 == 7) continue;
                if (x == 2 && !y) s2 = 0;
                else if ( ++ s2 == 7) continue;
                if (x == 2 && y == 2) s3 = 0;
                else if ( ++ s3 == 7) continue;

                if (st[t.day + 1][x][y][s0][s1][s2][s3]) continue;

                st[t.day + 1][x][y][s0][s1][s2][s3] = true;
                q.push({t.day + 1, x, y, s0, s1, s2, s3});
            }
    }

    return 0;
}

int main()
{
    while (cin >> n, n)
    {
        for (int i = 1; i <= n; i ++ )
            for (int j = 0; j < 4; j ++ )
                for (int k = 0; k < 4; k ++ )
                    cin >> state[i][j][k];

        cout << bfs() << endl;
    }

    return 0;
}
```  
##### AcWing192   立体推箱子
```cpp
 
```  
##### AcWing193   算乘方的牛
```cpp
 
```  
##### AcWing194   涂满它
```cpp
 
```  
##### AcWing195   骑士精神
```cpp
 
```  



# 0x30 数学知识(41)

包括质数、约数、同余、矩阵乘法、高斯消元与线性空间、组合计数、容斥原理与Mobius函数、概率与数学期望、0/1分数规划、博弈论之SG函数等内容。

## 0x31 质数

##### AcWing196   质数距离
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int primes[N], cnt;
bool st[N];

void get_primes(int n)
{
    memset(st, false, sizeof st);
    cnt = 0;
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
    long long l, r;
    while (cin >> l >> r)
    {
        get_primes(50000);

        memset(st, false, sizeof st);
        for (int i = 0; i < cnt; i ++ )
        {
            int p = primes[i];

            // 把[l, r]中所有p的倍数筛掉
            for (long long j = max((l + p - 1) / p * p, 2ll * p); j <= r; j += p)
                st[j - l] = true;
        }

        cnt = 0;
        for (int i = 0; i <= r - l; i ++ )
            if (!st[i] && i + l > 1)
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

            printf("%d,%d are closest, %d,%d are most distant.\n", primes[minp], primes[minp + 1], primes[maxp], primes[maxp + 1]);
        }
    }
    return 0;
}
```

##### AcWing197   阶乘分解  
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int primes[N], cnt;
bool st[N];

void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; j < cnt && i * primes[j] <= n; j ++ )
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
    get_primes(n);

    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        int s = 0;
        for (int j = p; j <= n; j *= p)
        {
            s += n / j;
            if (j > n / p) break;
        }
        cout << p << ' ' << s << endl;
    }

    return 0;
}
```

## 0x32 约数

##### AcWing198   反素数
```cpp
#include <iostream>

using namespace std;

typedef long long LL;

int ps[] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};

int n;
int sum = 0, minx;

void dfs(int u, int last, int p, int s)
{
    if (s > sum || s == sum && p < minx)
    {
        sum = s;
        minx = p;
    }

    for (int i = 1; i <= last; i ++ )
    {
        if ((LL)p * ps[u] > n) break;
        p *= ps[u];
        dfs(u + 1, i, p, s * (i + 1));
    }
}

int main()
{
    cin >> n;

    dfs(0, 30, 1, 1);

    cout << minx << endl;

    return 0;
}
```

##### AcWing199   余数之和
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int main()
{
    LL n, k;
    cin >> n >> k;
    LL res = n * k;
    for (LL x = 1, gx; x <= n; x = gx + 1)
    {
        gx = k / x ? min(n, k / (k / x)) : n;
        res -= (k / x) * (x + gx) * (gx - x + 1) / 2;
    }

    cout << res << endl;

    return 0;
}
```
  
##### AcWing200   Hankson的趣味题
```cpp
算法1
暴力枚举 b1b1 的所有约数

#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

int lcm(int a, int b)
{
    return a * 1ll * b / gcd(a, b);
}

int main()
{
    int n;
    scanf("%d", &n);

    while (n -- )
    {
        int a, b, c, d;
        scanf("%d%d%d%d", &a, &b, &c, &d);
        int s = 0;
        for (int i = 1; i * i <= d; i ++ )
        {
            if (d % i) continue;
            if (gcd(a, i) == b && lcm(c, i) == d)
                s ++ ;
            if (i != d / i && gcd(a, d / i) == b && lcm(c, d / i) == d)
                s ++ ;
        }
        printf("%d\n", s);
    }

    return 0;
}


算法2
先筛出 sqrt(b1)内的所有质数，再用试除法分解质因数，然后sqrt(b1)的所有约数，效率比暴力做法快了8倍左右。

#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long LL;
typedef pair<int, int> PII;

const int N = 45000, M = 50;

int primes[N], cnt;
bool st[N];

PII factor[M];
int cntf;

int divider[N], cntd;

void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

void dfs(int u, int p)
{
    if (u > cntf)
    {
        divider[cntd ++ ] = p;
        return;
    }

    for (int i = 0; i <= factor[u].second; i ++ )
    {
        dfs(u + 1, p);
        p *= factor[u].first;
    }
}

int main()
{
    get_primes(N);

    int n;
    scanf("%d", &n);
    while (n -- )
    {
        int a0, a1, b0, b1;
        scanf("%d%d%d%d", &a0, &a1, &b0, &b1);

        int d = b1;
        cntf = 0;
        for (int i = 0; primes[i] <= d / primes[i]; i ++ )
        {
            int p = primes[i];
            if (d % p == 0)
            {
                int s = 0;
                while (d % p == 0) s ++, d /= p;
                factor[ ++ cntf] = {p, s};
            }
        }
        if (d > 1) factor[ ++ cntf] = {d, 1};

        cntd = 0;
        dfs(1, 1);

        int res = 0;
        for (int i = 0; i < cntd; i ++ )
        {
            int x = divider[i];
            if (gcd(x, a0) == a1 && (LL)x * b0 / gcd(x, b0) == b1)
            {

                res ++ ;
            }
        }

        printf("%d\n", res);
    }

    return 0;
}
```
 
##### AcWing201   可见的点
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1010;

int primes[N], euler[N], cnt;
bool st[N];

// 质数存在primes[]中，euler[i] 表示
// i的欧拉函数
void get_eulers(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            euler[i] = i - 1;
        }
        for (int j = 0; j < cnt && i * primes[j] <= n; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0)
            {
                euler[i * primes[j]] = euler[i] * primes[j];
                break;
            }
            euler[i * primes[j]] = euler[i] * (primes[j] - 1);
        }
    }
}

int main()
{
    int T, n;

    get_eulers(N - 1);

    cin >> T;
    for (int k = 1; k <= T; k ++ )
    {
        cin >> n;
        int res = 0;
        for (int i = 2; i <= n; i ++ ) res += euler[i];
        printf("%d %d %d\n", k, n, res * 2 + 3);
    }

    return 0;
}
```
    

## 0x33 同余

     
  
##### AcWing202   最幸运的数字
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

LL mul(LL a, LL b, LL p)
{
    LL res = 0, t = a % p;
    while (b)
    {
        if (b & 1) res = (res + t) % p;
        t = (t + t) % p;
        b >>= 1;
    }

    return res;
}

LL qmi(LL m, LL k, LL p)
{
    LL res = 1, t = m;
    while (k)
    {
        if (k&1) res = mul(res, t, p);
        t = mul(t, t, p);
        k >>= 1;
    }
    return res;
}

int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

LL get_euler(LL x)
{
    auto res = x;
    for (int i = 2; i * i <= x; i ++ )
        if (x % i == 0)
        {
            res = res / i * (i - 1);
            while (x % i == 0) x /= i;
        }
    if (x > 1) res = res / x * (x - 1);
    return res;
}

int main()
{
    LL l;
    for (int T = 1; ; T ++ )
    {
        cin >> l;
        if (!l) break;
        l = l * 9 / gcd(l, 8);
        auto euler = get_euler(l);

        LL res = 1e18;
        for (LL i = 1; i * i <= euler; i ++ )
            if (euler % i == 0)
            {
                if (qmi(10, i, l) == 1) res = min(res, i);
                if (qmi(10, euler / i, l) == 1) res = min(res, euler / i);
            }

        if (res > 1e16) res = 0;

        printf("Case %d: ", T);
        cout << res << endl;
    }

    return 0;
}
```
