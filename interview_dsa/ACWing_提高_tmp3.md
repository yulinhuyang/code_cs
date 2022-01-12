##### AcWing1069 凸多边形的划分
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 55, M = 35, INF = 1e9;

int n;
int w[N];
LL f[N][N][M];

void add(LL a[], LL b[])
{
    static LL c[M];
    memset(c, 0, sizeof c);
    for (int i = 0, t = 0; i < M; i ++ )
    {
        t += a[i] + b[i];
        c[i] = t % 10;
        t /= 10;
    }
    memcpy(a, c, sizeof c);
}

void mul(LL a[], LL b)
{
    static LL c[M];
    memset(c, 0, sizeof c);
    LL t = 0;
    for (int i = 0; i < M; i ++ )
    {
        t += a[i] * b;
        c[i] = t % 10;
        t /= 10;
    }
    memcpy(a, c, sizeof c);
}

int cmp(LL a[], LL b[])
{
    for (int i = M - 1; i >= 0; i -- )
        if (a[i] > b[i]) return 1;
        else if (a[i] < b[i]) return -1;
    return 0;
}

void print(LL a[])
{
    int k = M - 1;
    while (k && !a[k]) k -- ;
    while (k >= 0) cout << a[k -- ];
    cout << endl;
}

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ ) cin >> w[i];

    LL temp[M];
    for (int len = 3; len <= n; len ++ )
        for (int l = 1; l + len - 1 <= n; l ++ )
        {
            int r = l + len - 1;
            f[l][r][M - 1] = 1;
            for (int k = l + 1; k < r; k ++ )
            {
                memset(temp, 0, sizeof temp);
                temp[0] = w[l];
                mul(temp, w[k]);
                mul(temp, w[r]);
                add(temp, f[l][k]);
                add(temp, f[k][r]);
                if (cmp(f[l][r], temp) > 0)
                    memcpy(f[l][r], temp, sizeof temp);
            }
        }

    print(f[1][n]);

    return 0;
}
```
##### AcWing479 加分二叉树
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 30;

int n;
int w[N];
int f[N][N], g[N][N];

void dfs(int l, int r)
{
    if (l > r) return;
    int k = g[l][r];
    cout << k << ' ';
    dfs(l, k - 1);
    dfs(k + 1, r);
}

int main()
{
    cin >> n;

    for (int i = 1; i <= n; i ++ ) cin >> w[i];

    for (int len = 1; len <= n; len ++ )
        for (int l = 1; l + len - 1 <= n; l ++ )
        {
            int r = l + len - 1;
            if (len == 1) f[l][r] = w[l], g[l][r] = l;
            else
            {
                for (int k = l; k <= r; k ++ )
                {
                    int left = k == l ? 1 : f[l][k - 1];
                    int right = k == r ? 1 : f[k + 1][r];
                    int score = left * right + w[k];
                    if (f[l][r] < score)
                    {
                        f[l][r] = score;
                        g[l][r] = k;
                    }
                }
            }
        }

    cout << f[1][n] << endl;

    dfs(1, n);

    return 0;
}
```
##### AcWing320 能量项链
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 210, INF = 0x3f3f3f3f;

int n;
int w[N];
int f[N][N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> w[i];
        w[i + n] = w[i];
    }

    for (int len = 3; len <= n + 1; len ++ )
        for (int l = 1; l + len - 1 <= n * 2; l ++ )
        {
            int r = l + len - 1;
            for (int k = l + 1; k < r; k ++ )
                f[l][r] = max(f[l][r], f[l][k] + f[k][r] + w[l] * w[k] * w[r]);
        }

    int res = 0;
    for (int l = 1; l <= n; l ++ ) res = max(res, f[l][l + n]);

    cout << res << endl;

    return 0;
}

```
##### AcWing1068 环形石子合并
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 410, INF = 0x3f3f3f3f;

int n;
int w[N], s[N];
int f[N][N], g[N][N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> w[i];
        w[i + n] = w[i];
    }

    for (int i = 1; i <= n * 2; i ++ ) s[i] = s[i - 1] + w[i];

    memset(f, 0x3f, sizeof f);
    memset(g, -0x3f, sizeof g);

    for (int len = 1; len <= n; len ++ )
        for (int l = 1; l + len - 1 <= n * 2; l ++ )
        {
            int r = l + len - 1;
            if (l == r) f[l][r] = g[l][r] = 0;
            else
            {
                for (int k = l; k < r; k ++ )
                {
                    f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
                    g[l][r] = max(g[l][r], g[l][k] + g[k + 1][r] + s[r] - s[l - 1]);
                }
            }
        }

    int minv = INF, maxv = -INF;
    for (int i = 1; i <= n; i ++ )
    {
        minv = min(minv, f[i][i + n - 1]);
        maxv = max(maxv, g[i][i + n - 1]);
    }

    cout << minv << endl << maxv << endl;

    return 0;
}
```
##### AcWing323 战略游戏
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1510;

int n;
int h[N], e[N], ne[N], idx;
int f[N][2];
bool st[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    f[u][0] = 0, f[u][1] = 1;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        dfs(j);
        f[u][0] += f[j][1];
        f[u][1] += min(f[j][0], f[j][1]);
    }
}

int main()
{
    while (cin >> n)
    {
        memset(h, -1, sizeof h);
        idx = 0;

        memset(st, 0, sizeof st);
        for (int i = 0; i < n; i ++ )
        {
            int id, cnt;
            scanf("%d:(%d)", &id, &cnt);
            while (cnt -- )
            {
                int ver;
                cin >> ver;
                add(id, ver);
                st[ver] = true;
            }
        }

        int root = 0;
        while (st[root]) root ++ ;
        dfs(root);

        printf("%d\n", min(f[root][0], f[root][1]));
    }

    return 0;
}
```
##### AcWing1074 二叉苹果树
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110, M = N * 2;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int f[N][N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u, int father)
{
    for (int i = h[u]; ~i; i = ne[i])
    {
        if (e[i] == father) continue;
        dfs(e[i], u);
        for (int j = m; j; j -- )
            for (int k = 0; k + 1 <= j; k ++ )
                f[u][j] = max(f[u][j], f[u][j - k - 1] + f[e[i]][k] + w[i]);
    }
}

int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c), add(b, a, c);
    }

    dfs(1, -1);

    printf("%d\n", f[1][m]);

    return 0;
}
```
##### AcWing1075 数字转换
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 50010, M = N;

int n;
int h[N], e[M], w[M], ne[M], idx;
int sum[N];
bool st[N];
int ans;

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

int dfs(int u)
{
    st[u] = true;

    int dist = 0;
    int d1 = 0, d2 = 0;
    for (int i = h[u]; ~i; i = ne[i])
    {
       int j = e[i];
       if (!st[j])
       {
           int d = dfs(j);
           dist = max(dist, d);
           if (d >= d1) d2 = d1, d1 = d;
           else if (d > d2) d2 = d;
       }
    }

    ans = max(ans, d1 + d2);

    return dist + 1;
}

int main()
{
    cin >> n;
    memset(h, -1, sizeof h);

    for (int i = 1; i <= n; i ++ )
        for (int j = 2; j <= n / i; j ++ )
            sum[i * j] += i;

    for (int i = 2; i <= n; i ++ )
        if (sum[i] < i)
            add(sum[i], i);

    for (int i = 1; i <= n; i ++ )
        if (!st[i])
            dfs(i);

    cout << ans << endl;

    return 0;
}
```
##### AcWing1072 树的最长路径
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010, M = N * 2;

int n;
int h[N], e[M], w[M], ne[M], idx;
int ans;

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int dfs(int u, int father)
{
    int dist = 0; // 表示从当前点往下走的最大长度
    int d1 = 0, d2 = 0;

    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (j == father) continue;
        int d = dfs(j, u) + w[i];
        dist = max(dist, d);

        if (d >= d1) d2 = d1, d1 = d;
        else if (d > d2) d2 = d;
    }

    ans = max(ans, d1 + d2);

    return dist;
}

int main()
{
    cin >> n;

    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c), add(b, a, c);
    }

    dfs(1, -1);

    cout << ans << endl;

    return 0;
}

```
##### AcWing1073 树的中心
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010, M = N * 2, INF = 0x3f3f3f3f;

int n;
int h[N], e[M], w[M], ne[M], idx;
int d1[N], d2[N], p1[N], up[N];
bool is_leaf[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int dfs_d(int u, int father)
{
    d1[u] = d2[u] = -INF;
    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (j == father) continue;
        int d = dfs_d(j, u) + w[i];
        if (d >= d1[u])
        {
            d2[u] = d1[u], d1[u] = d;
            p1[u] = j;
        }
        else if (d > d2[u]) d2[u] = d;
    }

    if (d1[u] == -INF)
    {
        d1[u] = d2[u] = 0;
        is_leaf[u] = true;
    }

    return d1[u];
}

void dfs_u(int u, int father)
{
    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (j == father) continue;

        if (p1[u] == j) up[j] = max(up[u], d2[u]) + w[i];
        else up[j] = max(up[u], d1[u]) + w[i];

        dfs_u(j, u);
    }
}

int main()
{
    cin >> n;
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c), add(b, a, c);
    }

    dfs_d(1, -1);
    dfs_u(1, -1);

    int res = d1[1];
    for (int i = 2; i <= n; i ++ )
        if (is_leaf[i]) res = min(res, up[i]);
        else res = min(res, max(d1[i], up[i]));

    printf("%d\n", res);

    return 0;
}
```
##### AcWing1077 皇宫看守
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1510;

int n;
int h[N], w[N], e[N], ne[N], idx;
int f[N][3];
bool st[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    f[u][2] = w[u];

    int sum = 0;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        dfs(j);
        f[u][0] += min(f[j][1], f[j][2]);
        f[u][2] += min(min(f[j][0], f[j][1]), f[j][2]);
        sum += min(f[j][1], f[j][2]);
    }

    f[u][1] = 1e9;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        f[u][1] = min(f[u][1], sum - min(f[j][1], f[j][2]) + f[j][2]);
    }
}

int main()
{
    cin >> n;

    memset(h, -1, sizeof h);
    for (int i = 1; i <= n; i ++ )
    {
        int id, cost, cnt;
        cin >> id >> cost >> cnt;
        w[id] = cost;
        while (cnt -- )
        {
            int ver;
            cin >> ver;
            add(id, ver);
            st[ver] = true;
        }
    }

    int root = 1;
    while (st[root]) root ++ ;

    dfs(root);

    cout << min(f[root][1], f[root][2]) << endl;

    return 0;
}
```
##### AcWing1086 恨7不成妻
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long LL;

const int N = 20, P = 1e9 + 7;

struct F
{
    int s0, s1, s2;
}f[N][10][7][7];

int power7[N], power9[N];

int mod(LL x, int y)
{
    return (x % y + y) % y;
}

void init()
{
    for (int i = 0; i <= 9; i ++ )
    {
        if (i == 7) continue;
        auto& v = f[1][i][i % 7][i % 7];
        v.s0 ++, v.s1 += i, v.s2 += i * i;
    }

    LL power = 10;
    for (int i = 2; i < N; i ++, power *= 10)
        for (int j = 0; j <= 9; j ++ )
        {
            if (j == 7) continue;
            for (int a = 0; a < 7; a ++ )
                for (int b = 0; b < 7; b ++ )
                    for (int k = 0; k <= 9; k ++ )
                    {
                        if (k == 7) continue;
                        auto &v1 = f[i][j][a][b], &v2 = f[i - 1][k][mod(a - j * power, 7)][mod(b - j, 7)];
                        v1.s0 = mod(v1.s0 + v2.s0, P);
                        v1.s1 = mod(v1.s1 + v2.s1 + j * (power % P) % P * v2.s0, P);
                        v1.s2 = mod(v1.s2 + j * j * (power % P) % P * (power % P) % P * v2.s0 + v2.s2 + 2 * j * power % P * v2.s1, P);
                    }
        }

    power7[0] = 1;
    for (int i = 1; i < N; i ++ ) power7[i] = power7[i - 1] * 10 % 7;

    power9[0] = 1;
    for (int i = 1; i < N; i ++ ) power9[i] = power9[i - 1] * 10ll % P;
}

F get(int i, int j, int a, int b)
{
    int s0 = 0, s1 = 0, s2 = 0;
    for (int x = 0; x < 7; x ++ )
        for (int y = 0; y < 7; y ++ )
            if (x != a && y != b)
            {
                auto v = f[i][j][x][y];
                s0 = (s0 + v.s0) % P;
                s1 = (s1 + v.s1) % P;
                s2 = (s2 + v.s2) % P;
            }
    return {s0, s1, s2};
}

int dp(LL n)
{
    if (!n) return 0;

    LL backup_n = n % P;
    vector<int> nums;
    while (n) nums.push_back(n % 10), n /= 10;

    int res = 0;
    LL last_a = 0, last_b = 0;
    for (int i = nums.size() - 1; i >= 0; i -- )
    {
        int x = nums[i];
        for (int j = 0; j < x; j ++ )
        {
            if (j == 7) continue;
            int a = mod(-last_a * power7[i + 1], 7);
            int b = mod(-last_b, 7);
            auto v = get(i + 1, j, a, b);
            res = mod(
                res + 
                (last_a % P) * (last_a % P) % P * power9[i + 1] % P * power9[i + 1] % P * v.s0 % P + 
                v.s2 + 
                2 * last_a % P * power9[i + 1] % P * v.s1,
            P);
        }

        if (x == 7) break;
        last_a = last_a * 10 + x;
        last_b += x;

        if (!i && last_a % 7 && last_b % 7) res = (res + backup_n * backup_n) % P;
    }

    return res;
}

int main()
{
    int T;
    cin >> T;

    init();

    while (T -- )
    {
        LL l, r;
        cin >> l >> r;
        cout << mod(dp(r) - dp(l - 1), P) << endl;
    }

    return 0;
}
```
##### AcWing1085 不要62
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 35;

int f[N][10];

void init()
{
    for (int i = 0; i <= 9; i ++ )
        if (i != 4)
            f[1][i] = 1;

    for (int i = 1; i < N; i ++ )
        for (int j = 0; j <= 9; j ++ )
        {
            if (j == 4) continue;
            for (int k = 0; k <= 9; k ++ )
            {
                if (k == 4 || j == 6 && k == 2) continue;
                f[i][j] += f[i - 1][k];
            }
        }
}

int dp(int n)
{
    if (!n) return 1;

    vector<int> nums;
    while (n) nums.push_back(n % 10), n /= 10;

    int res = 0;
    int last = 0;
    for (int i = nums.size() - 1; i >= 0; i -- )
    {
        int x = nums[i];
        for (int j = 0; j < x; j ++ )
        {
            if (j == 4 || last == 6 && j == 2) continue;
            res += f[i + 1][j];
        }

        if (x == 4 || last == 6 && x == 2) break;
        last = x;

        if (!i) res ++ ;
    }

    return res;
}

int main()
{
    init();

    int l, r;
    while (cin >> l >> r, l || r)
    {
        cout << dp(r) - dp(l - 1) << endl;
    }

    return 0;
}
```
##### AcWing1084 数字游戏II
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 11, M = 110;

int P;
int f[N][10][M];

int mod(int x, int y)
{
    return (x % y + y) % y;
}

void init()
{
    memset(f, 0, sizeof f);

    for (int i = 0; i <= 9; i ++ ) f[1][i][i % P] ++ ;

    for (int i = 2; i < N; i ++ )
        for (int j = 0; j <= 9; j ++ )
            for (int k = 0; k < P; k ++ )
                for (int x = 0; x <= 9; x ++ )
                    f[i][j][k] += f[i - 1][x][mod(k - j, P)];
}

int dp(int n)
{
    if (!n) return 1;

    vector<int> nums;
    while (n) nums.push_back(n % 10), n /= 10;

    int res = 0;
    int last = 0;
    for (int i = nums.size() - 1; i >= 0; i -- )
    {
        int x = nums[i];
        for (int j = 0; j < x; j ++ )
            res += f[i + 1][j][mod(-last, P)];

        last += x;

        if (!i && last % P == 0) res ++ ;
    }

    return res;
}

int main()
{
    int l, r;
    while (cin >> l >> r >> P)
    {
        init();

        cout << dp(r) - dp(l - 1) << endl;
    }

    return 0;
}
```
##### AcWing1083 Windy数
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 11;

int f[N][10];

void init()
{
    for (int i = 0; i <= 9; i ++ ) f[1][i] = 1;

    for (int i = 2; i < N; i ++ )
        for (int j = 0; j <= 9; j ++ )
            for (int k = 0; k <= 9; k ++ )
                if (abs(j - k) >= 2)
                    f[i][j] += f[i - 1][k];
}

int dp(int n)
{
    if (!n) return 0;

    vector<int> nums;
    while (n) nums.push_back(n % 10), n /= 10;

    int res = 0;
    int last = -2;
    for (int i = nums.size() - 1; i >= 0; i -- )
    {
        int x = nums[i];
        for (int j = i == nums.size() - 1; j < x; j ++ )
            if (abs(j - last) >= 2)
                res += f[i + 1][j];

        if (abs(x - last) >= 2) last = x;
        else break;

        if (!i) res ++ ;
    }

    // 特殊处理有前导零的数
    for (int i = 1; i < nums.size(); i ++ )
        for (int j = 1; j <= 9; j ++ )
            res += f[i][j];

    return res;
}

int main()
{
    init();

    int l, r;
    cin >> l >> r;
    cout << dp(r) - dp(l - 1) << endl;

    return 0;
}
```
##### AcWing1082 数字游戏
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 15;

int f[N][N];    // f[i, j]表示一共有i位，且最高位填j的数的个数

void init()
{
    for (int i = 0; i <= 9; i ++ ) f[1][i] = 1;

    for (int i = 2; i < N; i ++ )
        for (int j = 0; j <= 9; j ++ )
            for (int k = j; k <= 9; k ++ )
                f[i][j] += f[i - 1][k];
}

int dp(int n)
{
    if (!n) return 1;

    vector<int> nums;
    while (n) nums.push_back(n % 10), n /= 10;

    int res = 0;
    int last = 0;
    for (int i = nums.size() - 1; i >= 0; i -- )
    {
        int x = nums[i];
        for (int j = last; j < x; j ++ )
            res += f[i + 1][j];

        if (x < last) break;
        last = x;

        if (!i) res ++ ;
    }

    return res;
}

int main()
{
    init();

    int l, r;
    while (cin >> l >> r) cout << dp(r) - dp(l - 1) << endl;

    return 0;
}
```
##### AcWing1081 度的数量
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 35;

int K, B;
int f[N][N];

void init()
{
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j <= i; j ++ )
            if (!j) f[i][j] = 1;
            else f[i][j] = f[i - 1][j] + f[i - 1][j - 1];
}

int dp(int n)
{
    if (!n) return 0;

    vector<int> nums;
    while (n) nums.push_back(n % B), n /= B;

    int res = 0;
    int last = 0;
    for (int i = nums.size() - 1; i >= 0; i -- )
    {
        int x = nums[i];
        if (x)  // 求左边分支中的数的个数
        {
            res += f[i][K - last];
            if (x > 1)
            {
                if (K - last - 1 >= 0) res += f[i][K - last - 1];
                break;
            }
            else
            {
                last ++ ;
                if (last > K) break;
            }
        }

        if (!i && last == K) res ++ ;   // 最右侧分支上的方案
    }

    return res;
}

int main()
{
    init();

    int l, r;
    cin >> l >> r >> K >> B;

    cout << dp(r) - dp(l - 1) << endl;

    return 0;
}
```
##### AcWing135 最大子序和
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 300010, INF = 1e9;

int n, m;
int s[N];
int q[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &s[i]), s[i] += s[i - 1];

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
##### AcWing1088 旅行问题
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 2e6 + 10;

int n;
int oil[N], dist[N];
LL s[N];
int q[N];
bool ans[N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d%d", &oil[i], &dist[i]);
        s[i] = s[i + n] = oil[i] - dist[i];
    }
    for (int i = 1; i <= n * 2; i ++ ) s[i] += s[i - 1];

    int hh = 0, tt = 0;
    q[0] = n * 2 + 1;
    for (int i = n * 2; i >= 0; i -- )
    {
        if (q[hh] > i + n) hh ++ ;
        if (i < n)
        {
            if (s[i] <= s[q[hh]]) ans[i + 1] = true;
        }
        while (hh <= tt && s[q[tt]] >= s[i]) tt -- ;
        q[ ++ tt] = i;
    }

    dist[0] = dist[n];
    for (int i = 1; i <= n; i ++ ) s[i] = s[i + n] = oil[i] - dist[i - 1];
    for (int i = 1; i <= n * 2; i ++ ) s[i] += s[i - 1];

    hh = 0, tt = 0;
    q[0] = 0;
    for (int i = 1; i <= n * 2; i ++ )
    {
        if (q[hh] < i - n) hh ++ ;
        if (i > n)
        {
            if (s[i] >= s[q[hh]]) ans[i - n] = true;
        }
        while (hh <= tt && s[q[tt]] <= s[i]) tt -- ;
        q[ ++ tt] = i;
    }

    for (int i = 1; i <= n; i ++ )
        if (ans[i]) puts("TAK");
        else puts("NIE");

    return 0;
}
```
