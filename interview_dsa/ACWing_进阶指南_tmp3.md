## 0x14  hash表(字符串hash)

##### AcWing137   雪花雪花雪花
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n;
int snows[N][6], idx[N];

void get_min(int *b)
{
    static int a[12];
    for (int i = 0; i < 12; i ++ ) a[i] = b[i % 6];

    int i = 0, j = 1, k;
    while (i < 6 && j < 6)
    {
        for (k = 0; k < 6 && a[i + k] == a[j + k]; k ++ );
        if (k == 6) break;

        if (a[i + k] > a[j + k])
        {
            i += k + 1;
            if (i == j) i ++ ;
        }
        else
        {
            j += k + 1;
            if (i == j) j ++ ;
        }
    }

    k = min(i, j);

    for (i = 0; i < 6; i ++ ) b[i] = a[i + k];
}

bool cmp(int a, int b)
{
    for (int i = 0; i < 6; i ++ )
        if (snows[a][i] < snows[b][i])
            return true;
        else if (snows[a][i] > snows[b][i])
            return false;
    return false;
}

bool cmp2(int a[], int b[])
{
    for (int i = 0; i < 6; i ++ )
        if (a[i] < b[i])
            return true;
        else if (a[i] > b[i])
            return false;
    return false;
}

int main()
{
    scanf("%d", &n);
    int snow[6], isnow[6];
    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0, k = 5; j < 6; j ++, k -- )
        {
            scanf("%d", &snow[j]);
            isnow[k] = snow[j];
        }
        get_min(snow);
        get_min(isnow);
        if (cmp2(snow, isnow)) memcpy(snows[i], snow, sizeof snow);
        else memcpy(snows[i], isnow, sizeof isnow);

        idx[i] = i;
    }

    sort(idx, idx + n, cmp);

    for (int i = 1; i < n; i ++ )
    {
        if (!cmp(idx[i], idx[i - 1]) && !cmp(idx[i - 1], idx[i]))
        {
            puts("Twin snowflakes found.");
            return 0;
        }
    }

    puts("No two snowflakes are alike.");
    return 0;
}
```  
##### AcWing138   兔子与兔子
```cpp
#include <iostream>
#include <string.h>

using namespace std;

typedef unsigned long long ULL;
const int N = 1000010, p = 131;

char str[N];
ULL h[N], power[N];

ULL get(int l, int r)
{
    return h[r] - h[l - 1] * power[r - l + 1];
}

int main()
{
    scanf("%s", str + 1);
    int n = strlen(str + 1);
    power[0] = 1;
    for (int i = 1; i <= n; i ++ )
    {
        h[i] = h[i - 1] * p + str[i] - 'a' + 1;
        power[i] = power[i - 1] * p;
    }

    int m;
    scanf("%d", &m);
    while (m -- )
    {
        int l1, r1, l2, r2;
        scanf("%d%d%d%d", &l1, &r1, &l2, &r2);
        if (get(l1, r1) == get(l2, r2)) puts("Yes");
        else puts("No");
    }

    return 0;
}
```  
##### AcWing139   回文子串的最大长度
```cpp
#include <iostream>
#include <string.h>

using namespace std;

typedef unsigned long long ULL;
const int N = 2000010, base = 131;

char str[N];
ULL hl[N], hr[N], p[N];

ULL get(ULL h[], int l, int r)
{
    return h[r] - h[l - 1] * p[r - l + 1];
}

int main()
{
    int T = 1;
    while (scanf("%s", str + 1), strcmp(str + 1, "END"))
    {
        int n = strlen(str + 1);
        for (int i = n * 2; i; i -= 2)
        {
            str[i] = str[i / 2];
            str[i - 1] = 'a' + 26;
        }
        n *= 2;
        p[0] = 1;
        for (int i = 1, j = n; i <= n; i ++, j -- )
        {
            hl[i] = hl[i - 1] * base + str[i] - 'a' + 1; 
            hr[i] = hr[i - 1] * base + str[j] - 'a' + 1;
            p[i] = p[i - 1] * base;
        }

        int res = 0;
        for (int i = 1; i <= n; i ++ )
        {
            int l = 0, r = min(i - 1, n - i);
            while (l < r)
            {
                int mid = l + r + 1 >> 1;
                if (get(hl, i - mid, i - 1) != get(hr, n - (i + mid) + 1, n - (i + 1) + 1)) r = mid - 1;
                else l = mid;
            }

            if (str[i - l] <= 'z') res = max(res, l + 1);
            else res = max(res, l);
        }

        printf("Case %d: %d\n", T ++ , res);
    }

    return 0;
}
```  
##### AcWing140   后缀数组
```cpp
#include <iostream>
#include <algorithm>
#include <string.h>
#include <limits.h>

using namespace std;

typedef unsigned long long ULL;
const int N = 300010, base = 131;

int n;
char str[N];
ULL h[N], p[N];
int sa[N];

ULL get(int l, int r)
{
    return h[r] - h[l - 1] * p[r - l + 1];
}

int get_max_common_prefix(int a, int b)
{
    int l = 0, r = min(n - a + 1, n - b + 1);
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (get(a, a + mid - 1) != get(b, b + mid - 1)) r = mid - 1;
        else l = mid;
    }
    return l;
}

bool cmp(int a, int b)
{
    int l = get_max_common_prefix(a, b);
    int av = a + l > n ? INT_MIN : str[a + l];
    int bv = b + l > n ? INT_MIN : str[b + l];
    return av < bv;
}

int main()
{
    scanf("%s", str + 1);
    n = strlen(str + 1);

    p[0] = 1;
    for (int i = 1; i <= n; i ++ )
    {
        h[i] = h[i - 1] * base + str[i] - 'a' + 1;
        p[i] = p[i - 1] * base;
        sa[i] = i;
    }

    sort(sa + 1, sa + 1 + n, cmp);

    for (int i = 1; i <= n; i ++ ) printf("%d ", sa[i] - 1);
    puts("");
    for (int i = 1; i <= n; i ++ )
        if (i == 1) printf("0 ");
        else printf("%d ", get_max_common_prefix(sa[i - 1], sa[i]));
    puts("");

    return 0;
}
```  

## 0x15 字符串(KMP与最小表示法）

##### AcWing141   周期
```cpp
#include <iostream>

using namespace std;

const int N = 1000010;

int n;
char str[N];
int nxt[N];

void get_next()
{
    for (int i = 2, j = 0; i <= n; i ++ )
    {
        while (j && str[i] != str[j + 1]) j = nxt[j];
        if (str[i] == str[j + 1]) j ++ ;
        nxt[i] = j;
    }
}

int main()
{
    int T = 1;
    while (scanf("%d", &n), n)
    {
        scanf("%s", str + 1);

        get_next();

        printf("Test case #%d\n", T ++ );
        for (int i = 2; i <= n; i ++ )
        {
            int t = i - nxt[i];
            if (i > t && i % t == 0) printf("%d %d\n", i, i / t);
        }
        puts("");
    }

    return 0;
}
``` 

## 0x16  Trie树（字典树）

  
##### AcWing142   前缀统计
```cpp
#include <iostream>

using namespace std;

const int N = 500000, M = 1000010;

int n, m;
int son[N][26], cnt[N], idx;
char str[M];

void insert()
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int s = str[i] - 'a';
        if (!son[p][s]) son[p][s] = ++ idx;
        p = son[p][s];
    }
    cnt[p] ++ ;
}

int search()
{
    int p = 0, res = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int s = str[i] - 'a';
        if (!son[p][s]) break;
        p = son[p][s];
        res += cnt[p];
    }
    return res;
}

int main()
{
    scanf("%d%d", &n, &m);
    while (n -- )
    {
        scanf("%s", str);
        insert();
    }

    while (m -- )
    {
        scanf("%s", str);
        printf("%d\n", search());
    }

    return 0;
}
```  
##### AcWing143   最大异或对
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 3100010;

int n;
int a[N], son[M][2], idx;

void insert(int x)
{
    int p = 0;
    for (int i = 30; i >= 0; i -- )
    {
        int &s = son[p][x >> i & 1];
        if (!s) s = ++ idx;
        p = s;
    }
}

int search(int x)
{
    int p = 0, res = 0;
    for (int i = 30; i >= 0; i -- )
    {
        int s = x >> i & 1;
        if (son[p][!s])
        {
            res += 1 << i;
            p = son[p][!s];
        }
        else p = son[p][s];
    }
    return res;
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
    {
        cin >> a[i];
        insert(a[i]);
    }

    int res = 0;
    for (int i = 0; i < n; i ++ ) res = max(res, search(a[i]));

    cout << res << endl;

    return 0;
}
```  
##### AcWing144   最长异或值路径
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 200010;

int n;
int h[N], e[M], c[M], ne[M], cnt;
int a[N], son[3000000][2], idx;

void add(int u, int v, int w)
{
    e[cnt] = v, c[cnt] = w, ne[cnt] = h[u], h[u] = cnt ++ ;
}

void dfs(int u, int father, int sum)
{
    a[u] = sum;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (j != father) dfs(j, u, sum ^ c[i]);
    }
}

void insert(int x)
{
    int p = 0;
    for (int i = 30; i >= 0; i -- )
    {
        int &s = son[p][x >> i & 1];
        if (!s) s = ++ idx;
        p = s;
    }
}

int search(int x)
{
    int p = 0, res = 0;
    for (int i = 30; i >= 0; i -- )
    {
        int s = x >> i & 1;
        if (son[p][!s])
        {
            res += 1 << i;
            p = son[p][!s];
        }
        else p = son[p][s];
    }
    return res;
}

int main()
{
    memset(h, -1, sizeof h);

    cin >> n;
    for (int i = 0; i < n - 1; i ++ )
    {
        int u, v, w;
        cin >> u >> v >> w;
        add(u, v, w);
        add(v, u, w);
    }

    dfs(0, -1, 0);

    for (int i = 0; i < n; i ++ ) insert(a[i]);

    int res = 0;
    for (int i = 0; i < n; i ++ ) res = max(res, search(a[i]));

    cout << res << endl;

    return 0;
}
```  

## 0x17  二叉堆

##### AcWing145   超市
```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

const int N = 10010;

int main()
{
    int n;
    while (cin >> n)
    {
        vector<pair<int, int>> products(n);
        for (int i = 0; i < n; i ++ ) cin >> products[i].second >> products[i].first;
        sort(products.begin(), products.end());
        priority_queue<int, vector<int>, greater<int>> heap;
        for (auto p : products)
        {
            heap.push(p.second);
            if (heap.size() > p.first) heap.pop();
        }
        int res = 0;
        while (heap.size())
        {
            res += heap.top();
            heap.pop();
        }
        cout << res << endl;
    }
    return 0;
}
```
##### AcWing146   序列
```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <cstring>

using namespace std;

typedef pair<int,int> PII;

const int N = 2010;

int n, m;
int a[N], b[N], c[N];

void work()
{
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    for (int i = 0; i < n; i ++ ) heap.push({a[0] + b[i], 0});

    for (int i = 0; i < n; i ++ )
    {
        auto t = heap.top();
        heap.pop();
        c[i] = t.first;
        heap.push({t.first + a[t.second + 1] - a[t.second], t.second + 1});
    }

    memcpy(a, c, 4 * n);
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        cin >> m >> n;
        for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);
        sort(a, a + n);
        for (int i = 1; i < m; i ++ )
        {
            for (int j = 0; j < n; j ++ ) scanf("%d", &b[j]);
            sort(b, b + n);
            work();
        }

        for (int i = 0; i < n; i ++ ) printf("%d ", a[i]);
        puts("");
    }

    return 0;
}
```  
##### AcWing147   数据备份
```cpp
#include <iostream>
#include <set>

using namespace std;

typedef long long LL;
typedef pair<LL, int> PLI;

const int N = 100010;

int n, k;
int l[N], r[N];
LL d[N];

void delete_node(int p)
{
    r[l[p]] = r[p];
    l[r[p]] = l[p];
}

int main()
{
    cin >> n >> k;

    for (int i = 0; i < n; i ++ ) cin >> d[i];
    for (int i = n - 1; i; i -- ) d[i] -= d[i - 1];

    set<PLI> S;
    d[0] = d[n] = 1e15;
    for (int i = 0; i <= n; i ++ )
    {
        l[i] = i - 1;
        r[i] = i + 1;
        S.insert({d[i], i});
    }

    LL res = 0;
    while (k -- )
    {
        auto it = S.begin();
        LL v = it->first;
        int p = it->second, left = l[p], right = r[p];

        S.erase(it);
        S.erase({d[left], left}), S.erase({d[right], right});
        delete_node(left), delete_node(right);
        res += v;

        d[p] = d[left] + d[right] - d[p];
        S.insert({d[p], p});
    }

    cout << res << endl;

    return 0;
}
```  
##### AcWing148   合并果子
```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

const int N = 10010;

int main()
{
    int n;
    cin >> n;
    priority_queue<int, vector<int>, greater<int>> heap;

    while (n -- )
    {
        int v;
        cin >> v;
        heap.push(v);
    }

    int res = 0;
    while (heap.size() > 1)
    {
        auto a = heap.top();
        heap.pop();
        auto b = heap.top();
        heap.pop();
        res += a + b;
        heap.push(a + b);
    }

    cout << res << endl;
    return 0;
}
```  
##### AcWing149   荷马史诗
```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

typedef long long LL;
typedef pair<LL, int> PLI;

const int N = 100010;

int main()
{
    int n, m;
    cin >> n >> m;

    priority_queue<PLI, vector<PLI>, greater<PLI>> heap;
    for (int i = 0; i < n; i ++ )
    {
        LL w;
        cin >> w;
        heap.push({w, 0});
    }

    while ((n - 1) % (m - 1))
    {
        heap.push({0ll, 0});
        n ++ ;
    }

    LL res = 0;
    while (heap.size() > 1)
    {
        LL sum = 0;
        int depth = 0;
        for (int i = 0; i < m; i ++ )
        {
            sum += heap.top().first;
            depth = max(depth, heap.top().second);
            heap.pop();
        }
        res += sum;
        heap.push({sum, depth + 1});
    }

    cout << res << endl << heap.top().second << endl;

    return 0;
}
```  

## 0x18 总结与练习

##### AcWing150   括号画家
```cpp
#include <iostream>
#include <stack>

using namespace std;

const int N = 100010;

int main()
{
    string str;
    cin >> str;
    stack<int> stk;

    int res = 0;
    for (int i = 0; i < str.size(); i ++ )
    {
        char c = str[i];
        if (c == ')' && stk.size() && str[stk.top()] == '(') stk.pop();
        else if (c == ']' && stk.size() && str[stk.top()] == '[') stk.pop();
        else if (c == '}' && stk.size() && str[stk.top()] == '{') stk.pop();
        else stk.push(i);

        if (stk.size()) res = max(res, i - stk.top());
        else res = max(res, i + 1);
    }

    cout << res << endl;
    return 0;
}
```
##### AcWing151   表达式计算
```cpp
#include <iostream>
#include <stack>

using namespace std;

stack<int> nums;
stack<char> ops;

int qmi(int a, int k)
{
    int res = 1;
    while (k -- ) res *= a;
    return res;
}

void cal()
{
    int a = nums.top(); nums.pop();
    int b = nums.top(); nums.pop();
    char c = ops.top(); ops.pop();
    int d;

    if (c == '+') d = b + a;
    else if (c == '-') d = b - a;
    else if (c == '*') d = b * a;
    else if (c == '/') d = b / a;
    else d = qmi(b, a);

    nums.push(d);
}

int main()
{
    string str;
    cin >> str;

    if (str[0] == '-') str = '0' + str;
    string left;
    for (int i = 0; i < str.size(); i ++ ) left += '(';
    str = left + str + ')';

    for (int i = 0; i < str.size(); i ++ )
    {
        if (str[i] >= '0' && str[i] <= '9')
        {
            int j = i, t = 0;
            while (str[j] >= '0' && str[j] <= '9')
            {
                t = t * 10 + str[j] - '0';
                j ++ ;
            }
            nums.push(t);
            i = j - 1;
        }
        else
        {
            char c = str[i];
            if (c == '(') ops.push(c);
            else if (c == '+' || c == '-')
            {
                if (c == '-' && i && !(str[i - 1] >= '0' && str[i - 1] <= '9') && str[i - 1] != ')')
                {
                    if (str[i + 1] == '(')  // 将-(...)变成-1 * (...)
                    {
                        nums.push(-1);
                        ops.push('*');
                    }
                    else
                    {
                        int j = i + 1, t = 0;
                        while (str[j] >= '0' && str[j] <= '9')
                        {
                            t = t * 10 + str[j] - '0';
                            j ++ ;
                        }
                        nums.push(-t);
                        i = j - 1;
                    }
                }
                else
                {
                    while (ops.top() != '(') cal();
                    ops.push(c);
                }
            }
            else if (c == '*' || c == '/')
            {
                while (ops.top() == '*' || ops.top() == '/' || ops.top() == '^') cal();
                ops.push(c);
            }
            else if (c == '^')
            {
                while (ops.top() == '^') cal();
                ops.push(c);
            }
            else if (c == ')')
            {
                while (ops.top() != '(') cal();
                ops.pop();
            }
            else cout << "invalid operator!" << endl;
        }
    }

    cout << nums.top() << endl;
    return 0;
}
```

##### AcWing152   城市游戏
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
char g[N][N];
int h[N][N];
int q[N], l[N], r[N];

void cal(int a[], int l[])
{
    int hh = 0, tt = 0;
    a[0] = -1;
    for (int i = 1; i <= m; i ++ )
    {
        while (a[q[tt]] >= a[i]) tt -- ;
        l[i] = q[tt] + 1;
        q[ ++ tt] = i;
    }
}

int work(int a[])
{
    cal(a, l);
    reverse(a + 1, a + 1 + m);
    cal(a, r);
    reverse(a + 1, a + 1 + m);

    int res = 0;
    for (int i = 1; i <= m; i ++ )
    {
        int left = l[i];
        int right = m + 1 - r[m + 1 - i];
        res = max(res, a[i] * (right - left + 1));
    }

    return res;
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            cin >> g[i][j];
            if (g[i][j] == 'F') h[i][j] = h[i - 1][j] + 1;
            else h[i][j] = 0;
        }

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, work(h[i]));

    cout << res * 3 << endl;
    return 0;
}
```
