##### AcWing1285 单词
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int n;
int tr[N][26], f[N], idx;
int q[N], ne[N];
char str[N];
int id[210];

void insert(int x)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int t = str[i] - 'a';
        if (!tr[p][t]) tr[p][t] = ++ idx;
        p = tr[p][t];
        f[p] ++ ;
    }
    id[x] = p;
}

void build()
{
    int hh = 0, tt = -1;
    for (int i = 0; i < 26; i ++ )
        if (tr[0][i])
            q[ ++ tt] = tr[0][i];

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = 0; i < 26; i ++ )
        {
            int &p = tr[t][i];
            if (!p) p = tr[ne[t]][i];
            else
            {
                ne[p] = tr[ne[t]][i];
                q[ ++ tt] = p;
            }
        }
    }
}

int main()
{
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ )
    {
        scanf("%s", str);
        insert(i);
    }

    build();

    for (int i = idx - 1; i >= 0; i -- ) f[ne[q[i]]] += f[q[i]];

    for (int i = 0; i < n; i ++ ) printf("%d\n", f[id[i]]);

    return 0;
}
```

# 第五章 数学知识

##### AcWing1292 哥德巴赫猜想
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>


using namespace std;

const int N = 1000010;

int primes[N], cnt;
bool st[N];

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

int main()
{
    init(N - 1);

    int n;
    while (cin >> n, n)
    {
        for (int i = 1; ; i ++ )
        {
            int a = primes[i];
            int b = n - a;
            if (!st[b])
            {
                printf("%d = %d + %d\n", n, a, b);
                break;
            }
        }
    }

    return 0;
}

```
##### AcWing1293 夏洛克和他的女朋友
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int primes[N], cnt;
bool st[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int n;
    cin >> n;

    init(n + 1);

    if (n <= 2) puts("1");
    else puts("2");

    for (int i = 2; i <= n + 1; i ++ )
        if (!st[i]) printf("1 ");
        else printf("2 ");

    return 0;
}

```
##### AcWing196	质数距离
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1000010;

int primes[N], cnt;
bool st[N];

void init(int n)
{
    memset(st, 0, sizeof st);
    cnt = 0;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int l, r;
    while (cin >> l >> r)
    {
        init(50000);

        memset(st, 0, sizeof st);
        for (int i = 0; i < cnt; i ++ )
        {
            LL p = primes[i];
            for (LL j = max(p * 2, (l + p - 1) / p * p); j <= r; j += p)
                st[j - l] = true;
        }

        cnt = 0;
        for (int i = 0; i <= r - l; i ++ )
            if (!st[i] && i + l >= 2)
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

            printf("%d,%d are closest, %d,%d are most distant.\n",
                primes[minp], primes[minp + 1],
                primes[maxp], primes[maxp + 1]);
        }
    }

    return 0;
}
```
##### AcWing197	阶乘分解
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int primes[N], cnt;
bool st[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int n;
    cin >> n;
    init(n);

    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        int s = 0;
        for (int j = n; j; j /= p) s += j / p;
        printf("%d %d\n", p, s);
    }

    return 0;
}
```
##### AcWing1289 序列的第k个数
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int mod = 200907;

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

int main()
{
    int n;
    cin >> n;
    while (n -- )
    {
        int a, b, c, k;
        cin >> a >> b >> c >> k;
        if (a + c == b * 2) cout << (a + (b - a) * (LL)(k - 1)) % mod << endl;
        else cout << (LL)a * qmi(b / a, k - 1) % mod << endl;
    }

    return 0;
}
```
##### AcWing1290 越狱
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int mod = 100003;

int qmi(int a, LL k)
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

int main()
{
    int m;
    LL n;
    cin >> m >> n;

    cout << (qmi(m, n) - (LL)m * qmi(m - 1, n - 1) % mod + mod) % mod << endl;

    return 0;
}

```
##### AcWing1291 轻拍牛头
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;

int n;
int a[N], cnt[N], s[N];

int main()
{
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ )
    {
        scanf("%d", &a[i]);
        cnt[a[i]] ++ ;
    }

    for (int i = 1; i < N; i ++ )
        for (int j = i; j < N; j += i)
            s[j] += cnt[i];

    for (int i = 0; i < n; i ++ ) printf("%d\n", s[a[i]] - 1);

    return 0;
}
```
##### AcWing1294 樱花
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1e6 + 10, mod = 1e9 + 7;

int primes[N], cnt;
bool st[N];

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

int main()
{
    int n;
    cin >> n;

    init(n);

    int res = 1;
    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        int s = 0;
        for (int j = n; j; j /= p) s += j / p;
        res = (LL)res * (2 * s + 1) % mod;
    }

    cout << res << endl;


    return 0;
}
```
##### AcWing198	反素数
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int primes[9] = {2, 3, 5, 7, 11, 13, 17, 19, 23};
int maxd, number;
int n;

void dfs(int u, int last, int p, int s)
{
    if (s > maxd || s == maxd && p < number)
    {
        maxd = s;
        number = p;
    }

    if (u == 9) return;

    for (int i = 1; i <= last; i ++ )
    {
        if ((LL)p * primes[u] > n) break;
        p *= primes[u];
        dfs(u + 1, i, p, s * (i + 1));
    }
}

int main()
{
    cin >> n;

    dfs(0, 30, 1, 1);

    cout << number << endl;

    return 0;
}
```
##### AcWing200	Hankson的趣味题
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 50010;

int primes[N], cnt;
bool st[N];
struct Factor
{
    int p, s;
}factor[10];
int fcnt;

int dividor[1601], dcnt;

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

void dfs(int u, int p)
{
    if (u == fcnt)
    {
        dividor[dcnt ++ ] = p;
        return;
    }

    for (int i = 0; i <= factor[u].s; i ++ )
    {
        dfs(u + 1, p);
        p *= factor[u].p;
    }
}

int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

int main()
{
    init(N - 1);

    int n;
    cin >> n;
    while (n -- )
    {
        int a, b, c, d;
        cin >> a >> b >> c >> d;

        fcnt = 0;
        int t = d;
        for (int i = 0; primes[i] <= t / primes[i]; i ++ )
        {
            int p = primes[i];
            if (t % p == 0)
            {
                int s = 0;
                while (t % p == 0) t /= p, s ++ ;
                factor[fcnt ++ ] = {p, s};
            }
        }

        if (t > 1) factor[fcnt ++ ] = {t, 1};

        dcnt = 0;
        dfs(0, 1);

        int res = 0;
        for (int i = 0; i < dcnt; i ++ )
        {
            int x = dividor[i];
            if (gcd(a, x) == b && (LL)c * x / gcd(c, x) == d) res ++ ;
        }

        cout << res << endl;
    }

    return 0;
}

```
##### AcWing201	可见的点
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int primes[N], cnt;
bool st[N];
int phi[N];

void init(int n)
{
    phi[1] = 1;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            phi[i] = i - 1;
        }
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0)
            {
                phi[i * primes[j]] = phi[i] * primes[j];
                break;
            }
            phi[i * primes[j]] = phi[i] * (primes[j] - 1);
        }
    }
}

int main()
{
    init(N - 1);

    int n, m;
    cin >> m;
    for (int T = 1; T <= m; T ++ )
    {
        cin >> n;
        int res = 1;
        for (int i = 1; i <= n; i ++ ) res += phi[i] * 2;
        cout << T << ' ' << n << ' ' << res << endl;
    }

    return 0;
}
```
##### AcWing220	最大公约数
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1e7 + 10;

int primes[N], cnt;
bool st[N];
int phi[N];
LL s[N];

void init(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            phi[i] = i - 1;
        }
        for (int j = 0; primes[j] * i <= n; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0)
            {
                phi[i * primes[j]] = phi[i] * primes[j];
                break;
            }
            phi[i * primes[j]] = phi[i] * (primes[j] - 1);
        }
    }

    for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + phi[i];
}

int main()
{
    int n;
    cin >> n;
    init(n);

    LL res = 0;
    for (int i = 0; i < cnt; i ++ )
    {
        int p = primes[i];
        res += s[n / p] * 2 + 1;
    }

    cout << res << endl;

    return 0;
}
```
##### AcWing203	同余方程
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int exgcd(int a, int b, int &x, int &y)
{
    if (!b)
    {
        x = 1, y = 0;
        return a;
    }

    int d = exgcd(b, a % b, y, x);
    y -= a / b * x;

    return d;
}

int main()
{
    int a, b;
    cin >> a >> b;

    int x, y;
    exgcd(a, b, x, y);

    cout << (x % b + b) % b << endl;

    return 0;
}
```
##### AcWing222	青蛙的约会
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

LL exgcd(LL a, LL b, LL &x, LL &y)
{
    if (!b)
    {
        x = 1, y = 0;
        return a;
    }

    LL d = exgcd(b, a % b, y, x);
    y -= a / b * x;

    return d;
}

int main()
{
    LL a, b, m, n, L;
    cin >> a >> b >> m >> n >> L;

    LL x, y;
    LL d = exgcd(m - n, L, x, y);
    if ((b - a) % d) puts("Impossible");
    else
    {
        x *= (b - a) / d;
        LL t = abs(L / d);
        cout << (x % t + t) % t << endl;
    }

    return 0;
}
```
##### AcWing202	最幸运的数字
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

LL qmul(LL a, LL k, LL b)
{
    LL res = 0;
    while (k)
    {
        if (k & 1) res = (res + a) % b;
        a = (a + a) % b;
        k >>= 1;
    }
    return res;
}

LL qmi(LL a, LL k, LL b)
{
    LL res = 1;
    while (k)
    {
        if (k & 1) res = qmul(res, a, b);
        a = qmul(a, a, b);
        k >>= 1;
    }
    return res;
}

LL get_euler(LL C)
{
    LL res = C;
    for (LL i = 2; i <= C / i; i ++ )
        if (C % i == 0)
        {
            while (C % i == 0) C /= i;
            res = res / i * (i - 1);
        }
    if (C > 1) res = res / C * (C - 1);
    return res;
}

int main()
{
    int T = 1;
    LL L;
    while (cin >> L, L)
    {
        int d = 1;
        while (L % (d * 2) == 0 && d * 2 <= 8) d *= 2;

        LL C = 9 * L / d;

        LL phi = get_euler(C);

        LL res = 1e18;
        if (C % 2 == 0 || C % 5 == 0) res = 0;
        else
        {
            for (LL d = 1; d * d <= phi; d ++ )
                if (phi % d == 0)
                {
                    if (qmi(10, d, C) == 1) res = min(res, d);
                    if (qmi(10, phi / d, C) == 1) res = min(res, phi / d);
                }
        }

        printf("Case %d: %lld\n", T ++, res);
    }

    return 0;
}
```
##### AcWing1298 曹冲养猪
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 10;

int n;
int A[N], B[N];

void exgcd(LL a, LL b, LL &x, LL &y)
{
    if (!b) x = 1, y = 0;
    else
    {
        exgcd(b, a % b, y, x);
        y -= a / b * x;
    }
}

int main()
{
    scanf("%d", &n);

    LL M = 1;
    for (int i = 0; i < n; i ++ )
    {
        scanf("%d%d", &A[i], &B[i]);
        M *= A[i];
    }

    LL res = 0;
    for (int i = 0; i < n; i ++ )
    {
        LL Mi = M / A[i];
        LL ti, x;
        exgcd(Mi, A[i], ti, x);
        res += B[i] * Mi * ti;
    }

    cout << (res % M + M) % M << endl;

    return 0;
}
```
##### AcWing1303 斐波那契前n项和
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 3;

int n, m;

void mul(int c[], int a[], int b[][N])
{
    int temp[N] = {0};
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            temp[i] = (temp[i] + (LL)a[j] * b[j][i]) % m;

    memcpy(c, temp, sizeof temp);
}

void mul(int c[][N], int a[][N], int b[][N])
{
    int temp[N][N] = {0};
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            for (int k = 0; k < N; k ++ )
                temp[i][j] = (temp[i][j] + (LL)a[i][k] * b[k][j]) % m;

    memcpy(c, temp, sizeof temp);
}

int main()
{
    cin >> n >> m;

    int f1[N] = {1, 1, 1};
    int a[N][N] = {
        {0, 1, 0},
        {1, 1, 1},
        {0, 0, 1}
    };

    n -- ;
    while (n)
    {
        if (n & 1) mul(f1, f1, a);  // res = res * a
        mul(a, a, a);  // a = a * a
        n >>= 1;
    }

    cout << f1[2] << endl;

    return 0;
}
```
