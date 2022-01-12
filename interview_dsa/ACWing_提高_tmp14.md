##### AcWing1304 佳佳的斐波那契
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 4;

int n, m;

void mul(int c[][N], int a[][N], int b[][N])  // c = a * b
{
    static int t[N][N];
    memset(t, 0, sizeof t);

    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            for (int k = 0; k < N; k ++ )
                t[i][j] = (t[i][j] + (LL)a[i][k] * b[k][j]) % m;

    memcpy(c, t, sizeof t);
}

int main()
{
    cin >> n >> m;

    // {fn, fn+1, sn, pn}
    // pn = n * sn - tn
    int f1[N][N] = {1, 1, 1, 0};
    int a[N][N] = {
        {0, 1, 0, 0},
        {1, 1, 1, 0},
        {0, 0, 1, 1},
        {0, 0, 0, 1},
    };

    int k = n - 1;

    // 快速幂
    while (k)
    {
        if (k & 1) mul(f1, f1, a);  // f1 = f1 * a
        mul(a, a, a);  // a = a * a
        k >>= 1;
    }

    cout << (((LL)n * f1[0][2] - f1[0][3]) % m + m) % m << endl;

    return 0;
}
```
##### AcWing1305 GT考试
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 25;

int n, m, mod;
char str[N];
int ne[N];
int a[N][N];

void mul(int c[][N], int a[][N], int b[][N])  // c = a * b
{
    static int t[N][N];
    memset(t, 0, sizeof t);

    for (int i = 0; i < m; i ++ )
        for (int j = 0; j < m; j ++ )
            for (int k = 0; k < m; k ++ )
                t[i][j] = (t[i][j] + a[i][k] * b[k][j]) % mod;

    memcpy(c, t, sizeof t);
}

int qmi(int k)
{
    int f0[N][N] = {1};
    while (k)
    {
        if (k & 1) mul(f0, f0, a);  // f0 = f0 * a
        mul(a, a, a);  // a = a * a
        k >>= 1;
    }

    int res = 0;
    for (int i = 0; i < m; i ++ ) res = (res + f0[0][i]) % mod;
    return res;
}

int main()
{
    cin >> n >> m >> mod;
    cin >> str + 1;

    // kmp
    for (int i = 2, j = 0; i <= m; i ++ )
    {
        while (j && str[j + 1] != str[i]) j = ne[j];
        if (str[j + 1] == str[i]) j ++ ;
        ne[i] = j;
    }

    // 初始化A[i][j]
    for (int j = 0; j < m; j ++ )
        for (int c = '0'; c <= '9'; c ++ )
        {
            int k = j;
            while (k && str[k + 1] != c) k = ne[k];
            if (str[k + 1] == c) k ++ ;
            if (k < m) a[j][k] ++ ;
        }


    // F[n] = F[0] * A^n
    cout << qmi(n) << endl;

    return 0;
}
```
##### AcWing1307 牡牛和牝牛
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, mod = 5000011;

int n, k;
int f[N], s[N];

int main()
{
    cin >> n >> k;

    f[0] = s[0] = 1;
    for (int i = 1; i <= n; i ++ )
    {
        f[i] = s[max(i - k - 1, 0)];
        s[i] = (s[i - 1] + f[i]) % mod;
    }

    cout << s[n] << endl;

    return 0;
}

```
##### AcWing1308 方程的解
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 150;

int k, x;
int f[1000][100][N];

int qmi(int a, int b, int p)
{
    int res = 1;
    while (b)
    {
        if (b & 1) res = res * a % p;
        a = a * a % p;
        b >>= 1;
    }
    return res;
}

void add(int c[], int a[], int b[])
{
    for (int i = 0, t = 0; i < N; i ++ )
    {
        t += a[i] + b[i];
        c[i] = t % 10;
        t /= 10;
    }
}

int main()
{
    cin >> k >> x;

    int n = qmi(x % 1000, x, 1000);

    // C(n - 1, k - 1)
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j <= i && j < k; j ++ )
            if (!j) f[i][j][0] = 1;
            else add(f[i][j], f[i - 1][j], f[i - 1][j - 1]);  // f[i][j] = f[i - 1][j] + f[i - 1][j - 1];

    int *g = f[n - 1][k - 1];
    int i = N - 1;
    while (!g[i]) i -- ;
    while (i >= 0) cout << g[i -- ];

    return 0;
}
```
##### AcWing1309 车的放置
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 2010, mod = 100003;

int fact[N], infact[N];

int qmi(int a, int k)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % mod;
        a = (LL)a * a % mod;
        k >>= 1;
    }
    return res;
}

int C(int a, int b)
{
    if (a < b) return 0;
    return (LL)fact[a] * infact[a - b] % mod * infact[b] % mod;
}

int P(int a, int b)
{
    if (a < b) return 0;
    return (LL)fact[a] * infact[a - b] % mod;
}

int main()
{
    fact[0] = infact[0] = 1;
    for (int i = 1; i < N; i ++ )
    {
        fact[i] = (LL)fact[i - 1] * i % mod;
        infact[i] = (LL)infact[i - 1] * qmi(i, mod - 2) % mod;
    }

    int a, b, c, d, k;
    cin >> a >> b >> c >> d >> k;

    int res = 0;
    for (int i = 0; i <= k; i ++ )
    {
        res = (res + (LL)C(b, i) * P(a, i) % mod * C(d, k - i) % mod * P(a + c - i, k - i)) % mod;
    }

    cout << res << endl;

    return 0;
}
```
##### AcWing1310 数三角形
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

LL C(int n)
{
    return (LL)n * (n - 1) * (n - 2) / 6;
}

int main()
{
    int n, m;
    cin >> n >> m;

    n ++, m ++ ;

    LL res = C(n * m) - (LL)n * C(m) - (LL)m * C(n);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            res -= 2ll * (gcd(i, j) - 1) * (n - i) * (m - j);

    cout << res << endl;

    return 0;
}

```
##### AcWing1312 序列统计
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int p = 1000003;

int qmi(int a, int k)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

int C(int a, int b)
{
    if (a < b) return 0;

    int down = 1, up = 1;
    for (int i = a, j = 1; j <= b; i --, j ++ )
    {
        up = (LL)up * i % p;
        down = (LL)down * j % p;
    }

    return (LL)up * qmi(down, p - 2) % p;
}

int Lucas(int a, int b)
{
    if (a < p && b < p) return C(a, b);
    return (LL)Lucas(a / p, b / p) * C(a % p, b % p) % p;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        int n, l, r;
        cin >> n >> l >> r;
        cout << (Lucas(r - l + n + 1, r - l + 1) + p - 1) % p << endl;
    }

    return 0;
}

```
##### AcWing1315 网格
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int primes[N], cnt;
bool st[N];
int a[N], b[N];

void init(int n)
{
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

int get(int n, int p)
{
    int s = 0;
    while (n) s += n / p, n /= p;
    return s;
}

void mul(int r[], int &len, int x)
{
    int t = 0;
    for (int i = 0; i < len; i ++ )
    {
        t += r[i] * x;
        r[i] = t % 10;
        t /= 10;
    }
    while (t)
    {
        r[len ++ ] = t % 10;
        t /= 10;
    }
}

void sub(int a[], int al, int b[], int bl)
{
    for (int i = 0, t = 0; i < al; i ++ )
    {
        a[i] -= t + b[i];
        if (a[i] < 0) a[i] += 10, t = 1;
        else t = 0;
    }
}

int C(int x, int y, int r[N])
{
    int len = 1;
    r[0] = 1;

    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        int s = get(x, p) - get(y, p) - get(x - y, p);
        while (s -- ) mul(r, len, p);
    }

    return len;
}

int main()
{
    init(N - 1);

    int n, m;
    cin >> n >> m;
    int al = C(n + m, m, a);
    int bl = C(n + m, n + 1, b);

    sub(a, al, b, bl);

    int k = al - 1;
    while (!a[k] && k > 0) k -- ;
    while (k >= 0) printf("%d", a[k -- ]);

    return 0;
}

```
##### AcWing1316 有趣的数列
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 2000010;

int n, p;
int primes[N], cnt;
bool st[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if  (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int qmi(int a, int k)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

int get(int n, int p)
{
    int s = 0;
    while (n)
    {
        s += n / p;
        n /= p;
    }
    return s;
}

int C(int a, int b)
{
    int res = 1;
    for (int i = 0; i < cnt; i ++ )
    {
        int prime = primes[i];
        int s = get(a, prime) - get(b, prime) - get(a - b, prime);

        res = (LL)res * qmi(prime, s) % p;
    }

    return res;
}

int main()
{
    scanf("%d%d", &n, &p);
    init(n * 2);

    cout << (C(n * 2, n) - C(n * 2, n - 1) + p) % p << endl;

    return 0;
}

```
##### AcWing207	球形空间产生器
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

const int N = 15;

int n;
double a[N][N], b[N][N];

void gauss()
{
    // 转化成上三角矩阵
    for (int r = 1, c = 1; c <= n; c ++, r ++ )
    {
        // 找主元
        int t = r;
        for (int i = r + 1; i <= n; i ++ )
            if (fabs(b[i][c]) > fabs(b[t][c]))
                t = i;

        // 交换
        for (int i = c; i <= n + 1; i ++ ) swap(b[t][i], b[r][i]);
        // 归一化
        for (int i = n + 1; i >= c; i -- ) b[r][i] /= b[r][c];
        // 消
        for (int i = r + 1; i <= n; i ++ )
            for (int j = n + 1; j >= c; j -- )
                b[i][j] -= b[i][c] * b[r][j];
    }

    // 转化成对角矩阵
    for (int i = n; i > 1; i -- )
        for (int j = i - 1; j; j -- )
        {
            b[j][n + 1] -= b[i][n + 1] * b[j][i];
            b[j][i] = 0;
        }
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n + 1; i ++ )
        for (int j = 1; j <= n; j ++ )
            scanf("%lf", &a[i][j]);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
        {
            b[i][j] += 2 * (a[i][j] - a[0][j]);
            b[i][n + 1] += a[i][j] * a[i][j] - a[0][j] * a[0][j];
        }

    gauss();

    for (int i = 1; i <= n; i ++ ) printf("%.3lf ", b[i][n + 1]);

    return 0;
}
```
##### AcWing208	开关问题
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 35;

int n;
int a[N][N];

int gauss()
{
    int r, c;
    for (r = 1, c = 1; c <= n; c ++ )
    {
        // 找主元
        int t = r;
        for (int i = r + 1; i <= n; i ++ )
            if (a[i][c])
                t = i;

        if (!a[t][c]) continue;
        // 交换
        for (int i = c; i <= n + 1; i ++ ) swap(a[t][i], a[r][i]);
        // 消
        for (int i = r + 1; i <= n; i ++ )
            for (int j = n + 1; j >= c; j -- )
                a[i][j] ^= a[i][c] & a[r][j];
        r ++ ;
    }

    int res = 1;
    if (r < n + 1)
    {
        for (int i = r; i <= n; i ++ )
        {
            if (a[i][n + 1]) return -1;  // 出现了 0 == !0，无解
            res *= 2;
        }
    }

    return res;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        memset(a, 0, sizeof a);
        scanf("%d", &n);
        for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i][n + 1]);
        for (int i = 1; i <= n; i ++ )
        {
            int t;
            scanf("%d", &t);
            a[i][n + 1] ^= t;
            a[i][i] = 1;
        }

        int x, y;
        while (scanf("%d%d", &x, &y), x || y) a[y][x] = 1;

        int t = gauss();
        if (t == -1) puts("Oh,it's impossible~!!");
        else printf("%d\n", t);
    }

    return 0;
}
```
##### AcWing214	Devu和鲜花
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 20, mod = 1e9 + 7;

LL A[N];
int down = 1;

int qmi(int a, int k, int p)
{
    int res = 1;
    while (k)
    {
        if (k & 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

int C(LL a, LL b)
{
    if (a < b) return 0;
    int up = 1;
    for (LL i = a; i > a - b; i -- ) up = i % mod * up % mod;

    return (LL)up * down % mod; // 费马小定理
}

int main()
{
    LL n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i ++ ) cin >> A[i];

    for (int j = 1; j <= n - 1; j ++ ) down = (LL)j * down % mod;
    down = qmi(down, mod - 2, mod);

    int res = 0;
    for (int i = 0; i < 1 << n; i ++ )
    {
        LL a = m + n - 1, b = n - 1;
        int sign = 1;
        for (int j = 0; j < n; j ++ )
            if (i >> j & 1)
            {
                sign *= -1;
                a -= A[j] + 1;
            }
        res = (res + C(a, b) * sign) % mod;
    }

    cout << (res + mod) % mod << endl;

    return 0;
}
```
##### AcWing215	破译密码
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 50010;

int primes[N], cnt;
bool st[N];
int mobius[N], sum[N];

// 线性筛法，求莫比乌斯函数
void init(int n)
{
    mobius[1] = 1;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            mobius[i] = -1;
        }
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0)
            {
                mobius[t] = 0;
                break;
            }
            mobius[t] = mobius[i] * -1;
        }
    }

    for (int i = 1; i <= n; i ++ ) sum[i] = sum[i - 1] + mobius[i];
}

int main()
{
    init(N - 1);

    int T;
    scanf("%d", &T);
    while (T -- )
    {
        int a, b, d;
        scanf("%d%d%d", &a, &b, &d);
        a /= d, b /= d;
        int n = min(a, b);
        LL res = 0;
        for (int l = 1, r; l <= n; l = r + 1)
        {
            r = min(n, min(a / (a / l), b / (b / l)));
            res += (sum[r] - sum[l - 1]) * (LL)(a / l) * (b / l);
        }

        printf("%lld\n", res);
    }

    return 0;
}

```
##### AcWing217	绿豆蛙的归宿
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 200010;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int dout[N];
double f[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

double dp(int u)
{
    if (f[u] >= 0) return f[u];
    f[u] = 0;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        f[u] += (w[i] + dp(j)) / dout[u];
    }
    return f[u];
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
        dout[a] ++ ;
    }

    memset(f, -1, sizeof f);

    printf("%.2lf\n", dp(1));

    return 0;
}

```
##### AcWing218	扑克牌
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 14;
const double INF = 1e20;

int A, B, C, D;
double f[N][N][N][N][5][5];

double dp(int a, int b, int c, int d, int x, int y)
{
    double &v = f[a][b][c][d][x][y];
    if (v >= 0) return v;
    int as = a + (x == 0) + (y == 0);
    int bs = b + (x == 1) + (y == 1);
    int cs = c + (x == 2) + (y == 2);
    int ds = d + (x == 3) + (y == 3);
    if (as >= A && bs >= B && cs >= C && ds >= D) return v = 0;

    int sum = a + b + c + d + (x != 4) + (y != 4);
    sum = 54 - sum;
    if (sum <= 0) return v = INF;

    v = 1;
    if (a < 13) v += (13.0 - a) / sum * dp(a + 1, b, c, d, x, y);
    if (b < 13) v += (13.0 - b) / sum * dp(a, b + 1, c, d, x, y);
    if (c < 13) v += (13.0 - c) / sum * dp(a, b, c + 1, d, x, y);
    if (d < 13) v += (13.0 - d) / sum * dp(a, b, c, d + 1, x, y);
    if (x == 4)
    {
        double t = INF;
        for (int i = 0; i < 4; i ++ ) t = min(t, 1.0 / sum * dp(a, b, c, d, i, y));
        v += t;
    }
    if (y == 4)
    {
        double t = INF;
        for (int i = 0; i < 4; i ++ ) t = min(t, 1.0 / sum * dp(a, b, c, d, x, i));
        v += t;
    }

    return v;
}

int main()
{
    cin >> A >> B >> C >> D;
    memset(f, -1, sizeof f);

    double t = dp(0, 0, 0, 0, 4, 4);
    if (t > INF / 2) t = -1;

    printf("%.3lf\n", t);

    return 0;
}
```
##### AcWing1319 移棋子游戏
```cpp
#include <cstdio>
#include <cstring>
#include <set>

using namespace std;

const int N = 2010, M = 6010;

int n, m, k;
int h[N], e[M], ne[M], idx;
int f[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

int sg(int u)
{
    if (f[u] != -1) return f[u];

    set<int> S;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        S.insert(sg(j));
    }

    for (int i = 0; ; i ++ )
        if (S.count(i) == 0)
        {
            f[u] = i;
            break;
        }

    return f[u];
}

int main()
{
    scanf("%d%d%d", &n, &m, &k);
    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b);
    }

    memset(f, -1, sizeof f);

    int res = 0;
    for (int i = 0; i < k; i ++ )
    {
        int u;
        scanf("%d", &u);
        res ^= sg(u);
    }

    if (res) puts("win");
    else puts("lose");

    return 0;
}
```
##### AcWing1321 取石子
```cpp
#include <cstdio>
#include <cstring>

using namespace std;

const int N = 55, M = 50050;

int f[N][M];

int dp(int a, int b)
{
    int &v = f[a][b];
    if (v != -1) return v;
    if (!a) return v = b % 2;
    if (b == 1) return dp(a + 1, 0);

    if (a && !dp(a - 1, b)) return v = 1;
    if (b && !dp(a, b - 1)) return v = 1;
    if (a >= 2 && !dp(a - 2, b + (b ? 3 : 2))) return v = 1;
    if (a && b && !dp(a - 1, b + 1)) return v = 1;

    return v = 0;
}

int main()
{
    memset(f, -1, sizeof f);

    int T;
    scanf("%d", &T);
    while (T -- )
    {
        int n;
        scanf("%d", &n);
        int a = 0, b = 0;
        for (int i = 0; i < n; i ++ )
        {
            int x;
            scanf("%d", &x);
            if (x == 1) a ++ ;
            else b += b ? x + 1 : x;
        }

        if (dp(a, b)) puts("YES");
        else puts("NO");
    }

    return 0;
}

```
##### AcWing1322 取石子游戏
```cpp
#include <cstdio>

using namespace std;

const int N = 1010;

int n;
int a[N];
int l[N][N], r[N][N];

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d", &n);
        for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

        for (int len = 1; len <= n; len ++ )
            for (int i = 1; i + len - 1 <= n; i ++ )
            {
                int j = i + len - 1;
                if (len == 1) l[i][j] = r[i][j] = a[i];
                else
                {
                    int L = l[i][j - 1], R = r[i][j - 1], X = a[j];
                    if (R == X) l[i][j] = 0;
                    else if (X < L && X < R || X > L && X > R) l[i][j] = X;
                    else if (L > R) l[i][j] = X - 1;
                    else l[i][j] = X + 1;

                    L = l[i + 1][j], R = r[i + 1][j], X = a[i];
                    if (L == X) r[i][j] = 0;
                    else if (X < L && X < R || X > L && X > R) r[i][j] = X;
                    else if (R > L) r[i][j] = X - 1;
                    else r[i][j] = X + 1;
                }
            }

        if (n == 1) puts("1");
        else printf("%d\n", l[2][n] != a[1]);
    }

    return 0;
}
```

