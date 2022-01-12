##### AcWing245 你能回答这些问题吗
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 500010;

int n, m;
int w[N];
struct Node
{
    int l, r;
    int sum, lmax, rmax, tmax;
}tr[N * 4];

void pushup(Node &u, Node &l, Node &r)
{
    u.sum = l.sum + r.sum;
    u.lmax = max(l.lmax, l.sum + r.lmax);
    u.rmax = max(r.rmax, r.sum + l.rmax);
    u.tmax = max(max(l.tmax, r.tmax), l.rmax + r.lmax);
}

void pushup(int u)
{
    pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
}

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, w[r], w[r], w[r], w[r]};
    else
    {
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int x, int v)
{
    if (tr[u].l == x && tr[u].r == x) tr[u] = {x, x, v, v, v, v};
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (x <= mid) modify(u << 1, x, v);
        else modify(u << 1 | 1, x, v);
        pushup(u);
    }
}

Node query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u];
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (r <= mid) return query(u << 1, l, r);
        else if (l > mid) return query(u << 1 | 1, l, r);
        else
        {
            auto left = query(u << 1, l, r);
            auto right = query(u << 1 | 1, l, r);
            Node res;
            pushup(res, left, right);
            return res;
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    build(1, 1, n);

    int k, x, y;
    while (m -- )
    {
        scanf("%d%d%d", &k, &x, &y);
        if (k == 1)
        {
            if (x > y) swap(x, y);
            printf("%d\n", query(1, x, y).tmax);
        }
        else modify(1, x, y);
    }

    return 0;
}

```
##### AcWing246 区间最大公约数
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 500010;

int n, m;
LL w[N];
struct Node
{
    int l, r;
    LL sum, d;
}tr[N * 4];

LL gcd(LL a, LL b)
{
    return b ? gcd(b, a % b) : a;
}

void pushup(Node &u, Node &l, Node &r)
{
    u.sum = l.sum + r.sum;
    u.d = gcd(l.d, r.d);
}

void pushup(int u)
{
    pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
}

void build(int u, int l, int r)
{
    if (l == r)
    {
        LL b = w[r] - w[r - 1];
        tr[u] = {l, r, b, b};
    }
    else
    {
        tr[u].l = l, tr[u].r = r;
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int x, LL v)
{
    if (tr[u].l == x && tr[u].r == x)
    {
        LL b = tr[u].sum + v;
        tr[u] = {x, x, b, b};
    }
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (x <= mid) modify(u << 1, x, v);
        else modify(u << 1 | 1, x, v);
        pushup(u);
    }
}

Node query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u];
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (r <= mid) return query(u << 1, l, r);
        else if (l > mid) return query(u << 1 | 1, l, r);
        else
        {
            auto left = query(u << 1, l, r);
            auto right = query(u << 1 | 1, l, r);
            Node res;
            pushup(res, left, right);
            return res;
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%lld", &w[i]);
    build(1, 1, n);

    int l, r;
    LL d;
    char op[2];
    while (m -- )
    {
        scanf("%s%d%d", op, &l, &r);
        if (*op == 'Q')
        {
            auto left = query(1, 1, l);
            Node right({0, 0, 0, 0});
            if (l + 1 <= r) right = query(1, l + 1, r);
            printf("%lld\n", abs(gcd(left.sum, right.d)));
        }
        else
        {
            scanf("%lld", &d);
            modify(1, l, d);
            if (r + 1 <= n) modify(1, r + 1, -d);
        }
    }

    return 0;
}
```
##### AcWing243 一个简单的整数问题2
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010;

int n, m;
int w[N];
struct Node
{
    int l, r;
    LL sum, add;
}tr[N * 4];

void pushup(int u)
{
    tr[u].sum = tr[u << 1].sum + tr[u << 1 | 1].sum;
}

void pushdown(int u)
{
    auto &root = tr[u], &left = tr[u << 1], &right = tr[u << 1 | 1];
    if (root.add)
    {
        left.add += root.add, left.sum += (LL)(left.r - left.l + 1) * root.add;
        right.add += root.add, right.sum += (LL)(right.r - right.l + 1) * root.add;
        root.add = 0;
    }
}

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, w[r], 0};
    else
    {
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int l, int r, int d)
{
    if (tr[u].l >= l && tr[u].r <= r)
    {
        tr[u].sum += (LL)(tr[u].r - tr[u].l + 1) * d;
        tr[u].add += d;
    }
    else    // 一定要分裂
    {
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) modify(u << 1, l, r, d);
        if (r > mid) modify(u << 1 | 1, l, r, d);
        pushup(u);
    }
}

LL query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].sum;

    pushdown(u);
    int mid = tr[u].l + tr[u].r >> 1;
    LL sum = 0;
    if (l <= mid) sum = query(u << 1, l, r);
    if (r > mid) sum += query(u << 1 | 1, l, r);
    return sum;
}


int main()
{
    scanf("%d%d", &n, &m);

    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    build(1, 1, n);

    char op[2];
    int l, r, d;

    while (m -- )
    {
        scanf("%s%d%d", op, &l, &r);
        if (*op == 'C')
        {
            scanf("%d", &d);
            modify(1, l, r, d);
        }
        else printf("%lld\n", query(1, l, r));
    }

    return 0;
}
```
##### AcWing247 亚特兰蒂斯
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 100010;

int n;
struct Segment
{
    double x, y1, y2;
    int k;
    bool operator< (const Segment &t)const
    {
        return x < t.x;
    }
}seg[N * 2];
struct Node
{
    int l, r;
    int cnt;
    double len;
}tr[N * 8];

vector<double> ys;

int find(double y)
{
    return lower_bound(ys.begin(), ys.end(), y) - ys.begin();
}

void pushup(int u)
{
    if (tr[u].cnt) tr[u].len = ys[tr[u].r + 1] - ys[tr[u].l];
    else if (tr[u].l != tr[u].r)
    {
        tr[u].len = tr[u << 1].len + tr[u << 1 | 1].len;
    }
    else tr[u].len = 0;
}

void build(int u, int l, int r)
{
    tr[u] = {l, r, 0, 0};
    if (l != r)
    {
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
    }
}

void modify(int u, int l, int r, int k)
{
    if (tr[u].l >= l && tr[u].r <= r)
    {
        tr[u].cnt += k;
        pushup(u);
    }
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) modify(u << 1, l, r, k);
        if (r > mid) modify(u << 1 | 1, l, r, k);
        pushup(u);
    }
}

int main()
{
    int T = 1;
    while (scanf("%d", &n), n)
    {
        ys.clear();
        for (int i = 0, j = 0; i < n; i ++ )
        {
            double x1, y1, x2, y2;
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            seg[j ++ ] = {x1, y1, y2, 1};
            seg[j ++ ] = {x2, y1, y2, -1};
            ys.push_back(y1), ys.push_back(y2);
        }

        sort(ys.begin(), ys.end());
        ys.erase(unique(ys.begin(), ys.end()), ys.end());

        build(1, 0, ys.size() - 2);

        sort(seg, seg + n * 2);

        double res = 0;
        for (int i = 0; i < n * 2; i ++ )
        {
            if (i > 0) res += tr[1].len * (seg[i].x - seg[i - 1].x);
            modify(1, find(seg[i].y1), find(seg[i].y2) - 1, seg[i].k);
        }

        printf("Test case #%d\n", T ++ );
        printf("Total explored area: %.2lf\n\n", res);
    }

    return 0;
}
```
##### AcWing1277 维护序列
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010;

int n, p, m;
int w[N];
struct Node
{
    int l, r;
    int sum, add, mul;
}tr[N * 4];

void pushup(int u)
{
    tr[u].sum = (tr[u << 1].sum + tr[u << 1 | 1].sum) % p;
}

void eval(Node &t, int add, int mul)
{
    t.sum = ((LL)t.sum * mul + (LL)(t.r - t.l + 1) * add) % p;
    t.mul = (LL)t.mul * mul % p;
    t.add = ((LL)t.add * mul + add) % p;
}

void pushdown(int u)
{
    eval(tr[u << 1], tr[u].add, tr[u].mul);
    eval(tr[u << 1 | 1], tr[u].add, tr[u].mul);
    tr[u].add = 0, tr[u].mul = 1;
}

void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, w[r], 0, 1};
    else
    {
        tr[u] = {l, r, 0, 0, 1};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int l, int r, int add, int mul)
{
    if (tr[u].l >= l && tr[u].r <= r) eval(tr[u], add, mul);
    else
    {
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) modify(u << 1, l, r, add, mul);
        if (r > mid) modify(u << 1 | 1, l, r, add, mul);
        pushup(u);
    }
}

int query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].sum;

    pushdown(u);
    int mid = tr[u].l + tr[u].r >> 1;
    int sum = 0;
    if (l <= mid) sum = query(u << 1, l, r);
    if (r > mid) sum = (sum + query(u << 1 | 1, l, r)) % p;
    return sum;
}

int main()
{
    scanf("%d%d", &n, &p);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    build(1, 1, n);

    scanf("%d", &m);
    while (m -- )
    {
        int t, l, r, d;
        scanf("%d%d%d", &t, &l, &r);
        if (t == 1)
        {
            scanf("%d", &d);
            modify(1, l, r, 0, d);
        }
        else if (t == 2)
        {
            scanf("%d", &d);
            modify(1, l, r, d, 1);
        }
        else printf("%d\n", query(1, l, r));
    }

    return 0;
}
```
##### AcWing256 最大异或和
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 600010, M = N * 25;

int n, m;
int s[N];
int tr[M][2], max_id[M];
int root[N], idx;

void insert(int i, int k, int p, int q)
{
    if (k < 0)
    {
        max_id[q] = i;
        return;
    }
    int v = s[i] >> k & 1;
    if (p) tr[q][v ^ 1] = tr[p][v ^ 1];
    tr[q][v] = ++ idx;
    insert(i, k - 1, tr[p][v], tr[q][v]);
    max_id[q] = max(max_id[tr[q][0]], max_id[tr[q][1]]);
}

int query(int root, int C, int L)
{
    int p = root;
    for (int i = 23; i >= 0; i -- )
    {
        int v = C >> i & 1;
        if (max_id[tr[p][v ^ 1]] >= L) p = tr[p][v ^ 1];
        else p = tr[p][v];
    }

    return C ^ s[max_id[p]];
}

int main()
{
    scanf("%d%d", &n, &m);

    max_id[0] = -1;
    root[0] = ++ idx;
    insert(0, 23, 0, root[0]);

    for (int i = 1; i <= n; i ++ )
    {
        int x;
        scanf("%d", &x);
        s[i] = s[i - 1] ^ x;
        root[i] = ++ idx;
        insert(i, 23, root[i - 1], root[i]);
    }

    char op[2];
    int l, r, x;
    while (m -- )
    {
        scanf("%s", op);
        if (*op == 'A')
        {
            scanf("%d", &x);
            n ++ ;
            s[n] = s[n - 1] ^ x;
            root[n] = ++ idx;
            insert(n, 23, root[n - 1], root[n]);
        }
        else
        {
            scanf("%d%d%d", &l, &r, &x);
            printf("%d\n", query(root[r - 1], s[n] ^ x, l - 1));
        }
    }

    return 0;
}
```
##### AcWing255 第K小数
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 100010, M = 10010;

int n, m;
int a[N];
vector<int> nums;

struct Node
{
    int l, r;
    int cnt;
}tr[N * 4 + N * 17];

int root[N], idx;

int find(int x)
{
    return lower_bound(nums.begin(), nums.end(), x) - nums.begin();
}

int build(int l, int r)
{
    int p = ++ idx;
    if (l == r) return p;
    int mid = l + r >> 1;
    tr[p].l = build(l, mid), tr[p].r = build(mid + 1, r);
    return p;
}

int insert(int p, int l, int r, int x)
{
    int q = ++ idx;
    tr[q] = tr[p];
    if (l == r)
    {
        tr[q].cnt ++ ;
        return q;
    }
    int mid = l + r >> 1;
    if (x <= mid) tr[q].l = insert(tr[p].l, l, mid, x);
    else tr[q].r = insert(tr[p].r, mid + 1, r, x);
    tr[q].cnt = tr[tr[q].l].cnt + tr[tr[q].r].cnt;
    return q;
}

int query(int q, int p, int l, int r, int k)
{
    if (l == r) return r;
    int cnt = tr[tr[q].l].cnt - tr[tr[p].l].cnt;
    int mid = l + r >> 1;
    if (k <= cnt) return query(tr[q].l, tr[p].l, l, mid, k);
    else return query(tr[q].r, tr[p].r, mid + 1, r, k - cnt);
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%d", &a[i]);
        nums.push_back(a[i]);
    }

    sort(nums.begin(), nums.end());
    nums.erase(unique(nums.begin(), nums.end()), nums.end());

    root[0] = build(0, nums.size() - 1);

    for (int i = 1; i <= n; i ++ )
        root[i] = insert(root[i - 1], 0, nums.size() - 1, find(a[i]));

    while (m -- )
    {
        int l, r, k;
        scanf("%d%d%d", &l, &r, &k);
        printf("%d\n", nums[query(root[r], root[l - 1], 0, nums.size() - 1, k)]);
    }

    return 0;
}
```
##### AcWing253 普通平衡树
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, INF = 1e8;

int n;
struct Node
{
    int l, r;
    int key, val;
    int cnt, size;
}tr[N];

int root, idx;

void pushup(int p)
{
    tr[p].size = tr[tr[p].l].size + tr[tr[p].r].size + tr[p].cnt;
}

int get_node(int key)
{
    tr[ ++ idx].key = key;
    tr[idx].val = rand();
    tr[idx].cnt = tr[idx].size = 1;
    return idx;
}

void zig(int &p)    // 右旋
{
    int q = tr[p].l;
    tr[p].l = tr[q].r, tr[q].r = p, p = q;
    pushup(tr[p].r), pushup(p);
}

void zag(int &p)    // 左旋
{
    int q = tr[p].r;
    tr[p].r = tr[q].l, tr[q].l = p, p = q;
    pushup(tr[p].l), pushup(p);
}

void build()
{
    get_node(-INF), get_node(INF);
    root = 1, tr[1].r = 2;
    pushup(root);

    if (tr[1].val < tr[2].val) zag(root);
}


void insert(int &p, int key)
{
    if (!p) p = get_node(key);
    else if (tr[p].key == key) tr[p].cnt ++ ;
    else if (tr[p].key > key)
    {
        insert(tr[p].l, key);
        if (tr[tr[p].l].val > tr[p].val) zig(p);
    }
    else
    {
        insert(tr[p].r, key);
        if (tr[tr[p].r].val > tr[p].val) zag(p);
    }
    pushup(p);
}

void remove(int &p, int key)
{
    if (!p) return;
    if (tr[p].key == key)
    {
        if (tr[p].cnt > 1) tr[p].cnt -- ;
        else if (tr[p].l || tr[p].r)
        {
            if (!tr[p].r || tr[tr[p].l].val > tr[tr[p].r].val)
            {
                zig(p);
                remove(tr[p].r, key);
            }
            else
            {
                zag(p);
                remove(tr[p].l, key);
            }
        }
        else p = 0;
    }
    else if (tr[p].key > key) remove(tr[p].l, key);
    else remove(tr[p].r, key);

    pushup(p);
}

int get_rank_by_key(int p, int key)    // 通过数值找排名
{
    if (!p) return 0;   // 本题中不会发生此情况
    if (tr[p].key == key) return tr[tr[p].l].size + 1;
    if (tr[p].key > key) return get_rank_by_key(tr[p].l, key);
    return tr[tr[p].l].size + tr[p].cnt + get_rank_by_key(tr[p].r, key);
}

int get_key_by_rank(int p, int rank)   // 通过排名找数值
{
    if (!p) return INF;     // 本题中不会发生此情况
    if (tr[tr[p].l].size >= rank) return get_key_by_rank(tr[p].l, rank);
    if (tr[tr[p].l].size + tr[p].cnt >= rank) return tr[p].key;
    return get_key_by_rank(tr[p].r, rank - tr[tr[p].l].size - tr[p].cnt);
}

int get_prev(int p, int key)   // 找到严格小于key的最大数
{
    if (!p) return -INF;
    if (tr[p].key >= key) return get_prev(tr[p].l, key);
    return max(tr[p].key, get_prev(tr[p].r, key));
}

int get_next(int p, int key)    // 找到严格大于key的最小数
{
    if (!p) return INF;
    if (tr[p].key <= key) return get_next(tr[p].r, key);
    return min(tr[p].key, get_next(tr[p].l, key));
}

int main()
{
    build();

    scanf("%d", &n);
    while (n -- )
    {
        int opt, x;
        scanf("%d%d", &opt, &x);
        if (opt == 1) insert(root, x);
        else if (opt == 2) remove(root, x);
        else if (opt == 3) printf("%d\n", get_rank_by_key(root, x) - 1);
        else if (opt == 4) printf("%d\n", get_key_by_rank(root, x + 1));
        else if (opt == 5) printf("%d\n", get_prev(root, x));
        else printf("%d\n", get_next(root, x));
    }

    return 0;
}
```
##### AcWing265 营业额统计
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 33010, INF = 1e7;

int n;
struct Node
{
    int l, r;
    int key, val;
}tr[N];

int root, idx;

int get_node(int key)
{
    tr[ ++ idx].key = key;
    tr[idx].val = rand();
    return idx;
}

void build()
{
    get_node(-INF), get_node(INF);
    root = 1, tr[1].r = 2;
}

void zig(int &p)
{
    int q = tr[p].l;
    tr[p].l = tr[q].r, tr[q].r = p, p = q;
}

void zag(int &p)
{
    int q = tr[p].r;
    tr[p].r = tr[q].l, tr[q].l = p, p = q;
}

void insert(int &p, int key)
{
    if (!p) p = get_node(key);
    else if (tr[p].key == key) return;
    else if (tr[p].key > key)
    {
        insert(tr[p].l, key);
        if (tr[tr[p].l].val > tr[p].val) zig(p);
    }
    else
    {
        insert(tr[p].r, key);
        if (tr[tr[p].r].val > tr[p].val) zag(p);
    }
}

int get_prev(int p, int key)    // 找到小于等于key的最大数
{
    if (!p) return -INF;
    if (tr[p].key > key) return get_prev(tr[p].l, key);
    return max(tr[p].key, get_prev(tr[p].r, key));
}

int get_next(int p, int key)    // 找到大于等于key的最小数
{
    if (!p) return INF;
    if (tr[p].key < key) return get_next(tr[p].r, key);
    return min(tr[p].key, get_next(tr[p].l, key));
}

int main()
{
    build();
    scanf("%d", &n);

    LL res = 0;
    for (int i = 1; i <= n; i ++ )
    {
        int x;
        scanf("%d", &x);
        if (i == 1) res += x;
        else res += min(x - get_prev(root, x), get_next(root, x) - x);

        insert(root, x);
    }

    printf("%lld\n", res);

    return 0;
}
```
##### AcWing1282 搜索关键词
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010, S = 55, M = 1000010;

int n;
int tr[N * S][26], cnt[N * S], idx;
char str[M];
int q[N * S], ne[N * S];

void insert()
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int t = str[i] - 'a';
        if (!tr[p][t]) tr[p][t] = ++ idx;
        p = tr[p][t];
    }
    cnt[p] ++ ;
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
            int p = tr[t][i];
            if (!p) tr[t][i] = tr[ne[t]][i];
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
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        memset(tr, 0, sizeof tr);
        memset(cnt, 0, sizeof cnt);
        memset(ne, 0, sizeof ne);
        idx = 0;

        scanf("%d", &n);
        for (int i = 0; i < n; i ++ )
        {
            scanf("%s", str);
            insert();
        }

        build();

        scanf("%s", str);

        int res = 0;
        for (int i = 0, j = 0; str[i]; i ++ )
        {
            int t = str[i] - 'a';
            j = tr[j][t];

            int p = j;
            while (p)
            {
                res += cnt[p];
                cnt[p] = 0;
                p = ne[p];
            }
        }

        printf("%d\n", res);
    }

    return 0;
}
```
