**AcWing239 奇偶游戏**

https://www.acwing.com/solution/content/29308/  


```cpp
带边权写法
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_map>

using namespace std;

const int N = 20010;

int n, m;
int p[N], d[N];
unordered_map<int, int> S;

int get(int x)
{
    if (S.count(x) == 0) S[x] = ++ n;
    return S[x];
}

int find(int x)
{
    if (p[x] != x)
    {
        int root = find(p[x]);
        d[x] += d[p[x]];
        p[x] = root;
    }
    return p[x];
}

int main()
{
    cin >> n >> m;
    n = 0;

    for (int i = 0; i < N; i ++ ) p[i] = i;

    int res = m;
    for (int i = 1; i <= m; i ++ )
    {
        int a, b;
        string type;
        cin >> a >> b >> type;
        a = get(a - 1), b = get(b);

        int t = 0;
        if (type == "odd") t = 1;

        int pa = find(a), pb = find(b);
        if (pa == pb)
        {
            if (((d[a] + d[b]) % 2 + 2) % 2 != t)
            {
                res = i - 1;
                break;
            }
        }
        else
        {
            p[pa] = pb;
            d[pa] = d[a] ^ d[b] ^ t;
        }
    }

    cout << res << endl;

    return 0;
}
扩展域写法
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_map>

using namespace std;

const int N = 40010, Base = N / 2;

int n, m;
int p[N];
unordered_map<int, int> S;

int get(int x)
{
    if (S.count(x) == 0) S[x] = ++ n;
    return S[x];
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;
    n = 0;

    for (int i = 0; i < N; i ++ ) p[i] = i;

    int res = m;
    for (int i = 1; i <= m; i ++ )
    {
        int a, b;
        string type;
        cin >> a >> b >> type;
        a = get(a - 1), b = get(b);

        if (type == "even")
        {
            if (find(a + Base) == find(b))
            {
                res = i - 1;
                break;
            }
            p[find(a)] = find(b);
            p[find(a + Base)] = find(b + Base);
        }
        else
        {
            if (find(a) == find(b))
            {
                res = i - 1;
                break;
            }

            p[find(a + Base)] = find(b);
            p[find(a)] = find(b + Base);
        }
    }

    cout << res << endl;

    return 0;
}

```

**AcWing 240 食物链**

数组d的真正含义以及find()函数调用过程：https://www.acwing.com/solution/content/15938/

https://www.acwing.com/solution/content/1357/


**AcWing 257 关押罪犯**

https://www.acwing.com/solution/content/3042/  

