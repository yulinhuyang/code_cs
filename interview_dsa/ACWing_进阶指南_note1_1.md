### 0x05 排序

**Cinema**

CF670C Cinema

```cpp
//Author:XuHt
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 200006;
int n, m, a[N], x[N], y[N], cinema[N*3], tot = 0, k, ans[N*3];

int find(int f) {
	return lower_bound(cinema + 1, cinema + k + 1, f) - cinema;
}

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) {
		scanf("%d", &a[i]);
		cinema[++tot] = a[i];
	}
	cin >> m;
	for (int i = 1; i <= m; i++) {
		scanf("%d", &x[i]);
		cinema[++tot] = x[i];
	}
	for (int i = 1; i <= m; i++) {
		scanf("%d", &y[i]);
		cinema[++tot] = y[i];
	}
	sort(cinema + 1, cinema + tot + 1);
	k = unique(cinema + 1, cinema + tot + 1) - (cinema + 1);
	memset(ans, 0, sizeof(ans));
	for (int i = 1; i <= n; i++) ans[find(a[i])]++;
	int ans0 = 1, ans1 = 0, ans2 = 0;
	for (int i = 1; i <= m; i++) {
		int ansx = ans[find(x[i])], ansy = ans[find(y[i])];
		if (ansx > ans1 || (ansx == ans1 && ansy > ans2)) {
			ans0 = i;
			ans1 = ansx;
			ans2 = ansy;
		}
	}
	cout << ans0 << endl;
	return 0;
}

```

**货仓选址**

CH0501 货仓选址

```cpp
//Author:XuHt
#include <cstdio>
#include <iostream>
#include <algorithm>
#define ll long long
using namespace std;
const int N = 100006;
int n, a[N];

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
	sort(a + 1, a + n + 1);
	ll ans = 0;
	for (int i = 1; i <= n / 2; i++)
		ans += a[n-i+1] - a[i];
	cout << ans << endl;
	return 0;
}

```
**七夕祭**

BZOJ3032／CH0502 七夕祭

```cpp
//Author:XuHt
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#define ll long long
using namespace std;
const int N = 100006;
int n, m, t, x[N], y[N], a[N], s[N];

int main() {
	cin >> n >> m >> t;
	for (int i = 1; i <= t; i++) scanf("%d %d", &x[i], &y[i]);
	bool row = !(t % n), column = !(t % m);
	if (row) {
		if (column) cout << "both ";
		else cout << "row ";
	} else {
		if (column) cout << "column ";
		else {
			cout << "impossible" << endl;
			return 0;
		}
	}
	ll ans = 0;
	if (row) {
		int num = t / n;
		memset(a, 0, sizeof(a));
		for (int i = 1; i <= t; i++) a[x[i]]++;
		for (int i = 1; i <= n; i++) a[i] -= num;
		s[0] = 0;
		for (int i = 1; i <= n; i++) s[i] = s[i-1] + a[i];
		sort(s + 1, s + n + 1);
		for (int i = 1; i <= n / 2; i++) ans += s[n-i+1] - s[i];
	}
	if (column) {
		int num = t / m;
		memset(a, 0, sizeof(a));
		for (int i = 1; i <= t; i++) a[y[i]]++;
		for (int i = 1; i <= m; i++) a[i] -= num;
		s[0] = 0;
		for (int i = 1; i <= m; i++) s[i] = s[i-1] + a[i];
		sort(s + 1, s + m + 1);
		for (int i = 1; i <= m / 2; i++) ans += s[m-i+1] - s[i];
	}
	cout << ans << endl;
	return 0;
}

```

**Running Median**

POJ3784 Running Median
```cpp
//Author:XuHt
#include <queue>
#include <cstdio>
#include <iostream>
using namespace std;
priority_queue<int> q1, q2;

void Running_Median() {
	while (q1.size()) q1.pop();
	while (q2.size()) q2.pop();
	int num, n;
	cin >> num >> n;
	cout << num << " " << (n + 1) / 2 << endl;
	int a;
	cin >> a;
	cout << a << " ";
	q2.push(-a);
	int cnt = 1;
	for (int i = 2; i <= n; i++) {
		scanf("%d", &a);
		if (a < -q2.top()) q1.push(a);
		else q2.push(-a);
		int s = q1.size();
		if (s > i / 2) {
			q2.push(-q1.top());
			q1.pop();
		}
		if (s < i / 2) {
			q1.push(-q2.top());
			q2.pop();
		}
		if (i % 2) {
			cout<< -q2.top() << " ";
			if (++cnt % 10 == 0) cout << endl;
		}
	}
	cout << endl;
}

int main() {
	int t;
	cin >> t;
	while (t--) Running_Median();
	return 0;
}
```

**Ultra-QuickSort**

POJ2299 Ultra-QuickSort

```cpp
//Author:XuHt
#include <cstdio>
#include <iostream>
#include <algorithm>
#define ll long long
using namespace std;
const int N = 500006;
int n, a[N], b[N];
ll ans;

void merge(int l, int mid, int r) {
	if (l == r) return;
	if (l + 1 == r) {
		if (a[l] > a[r]) {
			ans++;
			swap(a[l], a[r]);
		}
		return;
	}
	merge(l, (l + mid) >> 1, mid);
	merge(mid + 1, (mid + 1 + r) >> 1, r);
	int i = l, j = mid + 1;
	for (int k = l; k <= r; k++)
		if (j > r || (i <= mid && a[i] <= a[j])) b[k] = a[i++];
		else {
			b[k] = a[j++];
			ans += mid - i + 1;
		}
	for (int k = l; k <= r; k++) a[k] = b[k];
}

void Ultra_QuickSort() {
	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
	ans = 0;
	merge(1, (1 + n) >> 1, n);
	cout << ans << endl;
}

int main() {
	while (cin >> n && n) Ultra_QuickSort();
	return 0;
}

```

**奇数码问题**

std
```cpp
using namespace std;
int n, c[1000010];
vector<int> a,b;

int ask(int x)
{
	int y=0;
	for(;x;x-=x&-x) y+=c[x];
	return y;
}

void add(int x)
{
	for(;x<=n*n-1;x+=x&-x) c[x]++;
}

long long calc(vector<int> a)
{
	long long ans=0;
	memset(c,0,sizeof(c));
	for(int i=a.size()-1;i>=0;i--)
	{
		ans+=ask(a[i]);
		add(a[i]);
	}
	return ans;
}

int main()
{
	freopen("data.in","r",stdin);
	freopen("data.out","w",stdout); 
	while(cin>>n)
	{
		a.clear();
		b.clear();
		for(int i=1;i<=n;i++)
			for(int j=1;j<=n;j++)
			{
				int x;
				scanf("%d",&x);
				if(x) a.push_back(x);
			}
		for(int i=1;i<=n;i++)
			for(int j=1;j<=n;j++)
			{
				int x;
				scanf("%d",&x);
				if(x) b.push_back(x);
			}
		puts(calc(a)-calc(b)&1 ? "NIE" : "TAK");
	}
}
```

std2

```cpp
#include<algorithm>
#include<vector>
using namespace std;
int n;
long long ans;
vector<int> a[2];
int c[250010];

void merge(int k, int l, int mid, int r)
{
	int x = l, y = mid + 1;
	for (int i = l; i <= r; i++)
	{
		if (y>r || x <= mid&&a[k][x]<a[k][y])
			c[i] = a[k][x++];
		else ans += mid - x + 1, c[i] = a[k][y++];
	}
	for (int i = l; i <= r; i++) a[k][i] = c[i];
}

void mergesort(int k, int l, int r)
{
	if (l == r) return;
	int mid = (l + r) / 2;
	mergesort(k, l, mid);
	mergesort(k, mid + 1, r);
	merge(k, l, mid, r);
}

long long calc(int k)
{
	ans = 0;
	mergesort(k, 0, n*n - 1);
	return ans;
}

int main()
{
	while (cin >> n)
	{
		a[0].clear();
		a[1].clear();
		for (int i = 1; i <= n; i++)for (int j = 1; j <= n; j++)
		{
			int x; scanf("%d", &x); if (x) a[0].push_back(x);
		}
		for (int i = 1; i <= n; i++)for (int j = 1; j <= n; j++)
		{
			int x; scanf("%d", &x); if (x) a[1].push_back(x);
		}
		puts(a[0].size()&&(calc(1) - calc(0) & 1) ? "NIE" : "TAK");
	}
}

```

### 0x06 倍增

**Genius ACM**

CH0601 Genius ACM

```cpp
//Author:XuHt
#include <cstdio>
#include <iostream>
#include <algorithm>
#define ll long long
using namespace std;
const int N = 500006;
int n, m, w;
ll k, a[N], b[N], c[N];

void gb(int l, int mid, int r) {
	int i = l, j = mid + 1;
	for (int k = l; k <= r; k++)
		if (j > r || (i <= mid && b[i] <= b[j])) c[k] = b[i++];
		else c[k] = b[j++];
}

ll f(int l, int r) {
	if (r > n) r = n;
	int t = min(m, (r - l + 1) >> 1);
	for (int i = w + 1; i <= r; i++) b[i] = a[i];
	sort(b + w + 1, b + r + 1);
	gb(l, w, r);
	ll ans = 0;
	for (int i = 0; i < t; i++)
		ans += (c[r-i] - c[l+i]) * (c[r-i] - c[l+i]);
	return ans;
}

void Genius_ACM() {
	cin >> n >> m;
	cin >> k;
	for (int i = 1; i <= n; i++) scanf("%lld", &a[i]);
	int ans = 0, l = 1, r = 1;
	w = 1;
	b[1] = a[1];
	while (l <= n) {
		int p = 1;
		while (p) {
			ll num = f(l, r + p);
			if (num <= k) {
				w = r = min(r + p, n);
				for (int i = l; i <= r; i++) b[i] = c[i];
				if (r == n) break;
				p <<= 1;
			} else p >>= 1;
		}
		ans++;
		l = r + 1;
	}
	cout << ans << endl;
}

int main() {
	int t;
	cin >> t;
	while (t--) Genius_ACM();
	return 0;
}
```

### 0x07 贪心

**Sunscreen**

POJ3614 Sunscreen

匈牙利算法（简单易懂）：https://blog.csdn.net/sunny_hun/article/details/80627351


```cpp
//Author:XuHt
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 2506;
int n, m;
struct COW {
	int l, r;
	bool operator < (const COW x) const {
		return l > x.l;
	}
} cow[N];
struct SPF {
	int s, c;
	bool operator < (const SPF x) const {
		return s > x.s;
	}
} spf[N];

int main() {
	cin >> n >> m;
	for (int i = 1; i <= n; i++) scanf("%d %d", &cow[i].l, &cow[i].r);
	for (int i = 1; i <= m; i++) scanf("%d %d", &spf[i].s, &spf[i].c);
	sort(cow + 1, cow + n + 1);
	sort(spf + 1, spf + m + 1);
	int ans = 0;
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= m; j++)
			if (spf[j].c && spf[j].s >= cow[i].l && spf[j].s <= cow[i].r) {
				ans++;
				spf[j].c--;
				break;
			}
	cout << ans << endl;
	return 0;
}

```


yxc解法

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
        spfs[spf] += cover; // 注意这里要写 +=，因为数据中存在spf值相同的防晒霜
    }
    sort(cows, cows + n);
    int res = 0;
    spfs[0] = spfs[1001] = n;
    for (int i = n - 1; i >= 0; i -- )
    {
        auto spf = spfs.upper_bound(cows[i].second);
        spf --;
        if (spf->first >= cows[i].first)
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

**Stall Reservation 畜栏预定**

POJ3190 Stall Reservations

```cpp
//Author:XuHt
#include <queue>
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 50006;
int n, ans[N];
struct COW {
	int id, l, r, ans;
	bool operator < (const COW x) const {
		return l < x.l;
	}
} cow[N];
priority_queue<pair<int, int> > s;

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cow[i].id = i;
		scanf("%d %d", &cow[i].l, &cow[i].r);
	}
	sort(cow + 1, cow + n + 1);
	for (int i = 1; i <= n; i++) {
		int num = s.size();
		if (num && -s.top().first < cow[i].l) {
			cow[i].ans = s.top().second;
			s.pop();
			s.push(make_pair(-cow[i].r, cow[i].ans));
			continue;
		}
		cow[i].ans = ++num;
		s.push(make_pair(-cow[i].r, num));
	}
	cout << s.size() << endl;
	for (int i = 1; i <= n; i++) ans[cow[i].id] = cow[i].ans;
	for (int i = 1; i <= n; i++) printf("%d\n", ans[i]);
	return 0;
}
```

yxc解法

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


**Radar Installation**

POJ1328 Radar Installation

```cpp
//Author:XuHt
#include <cmath>
#include <cstdio>
#include <iostream>
#include <algorithm>
#define ld long double
using namespace std;
const int N = 1006;
const double INF = -0x3f3f3f3f, eps = 0.000001;
int n, d, num = 0;
struct P {
	int x, y;
	double l, r;
	bool operator < (const P x) const {
		return l < x.l;
	}
} p[N];

void Radar_Installation() {
	for (int i = 1; i <= n; i++) scanf("%d %d", &p[i].x, &p[i].y);
	bool b = 1;
	for (int i = 1; i <= n; i++)
		if (p[i].y > d) {
			b = 0;
			break;
		}
	if (!b) {
		cout << "Case " << ++num << ": -1" << endl;
		return;
	}
	for (int i = 1; i <= n; i++) {
		ld k = sqrt((ld)d * d - (ld)p[i].y * p[i].y);
		p[i].l = p[i].x - k, p[i].r = p[i].x + k;
	}
	sort(p + 1, p + n + 1);
	int ans = 1;
	double pos = -INF;
	for (int i = 1; i <= n; i++) {
		if (pos + eps < p[i].l) {
			ans++;
			pos = p[i].r;
		} else pos = min(p[i].r, pos);
	}
	cout << "Case " << ++num << ": " << ans << endl;
}

int main() {
	while (cin >> n >> d && n && d) Radar_Installation();
	return 0;
}
```

yxc解法

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

**国王游戏**

NOIP2012／CH0701 国王游戏

```cpp
//Author:XuHt
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 1006;
int n, k[N*4], lk = 1, ans[N*4], la = 1, ans0[N*4], la0 = 1;
struct P {
	int a, b;
	bool operator < (const P x) const {
		return a * b < x.a * x.b;
	}
} p[N];

void gj1(int x) {
	for (int i = 1; i <= lk; i++) k[i] *= x;
	lk += 4;
	for (int i = 1; i <= lk; i++) {
		k[i+1] += k[i] / 10;
		k[i] %= 10;
	}
	while (!k[lk]) lk--;
}

void gj2(int x) {
	int w = 0;
	bool flag = 1;
	for (int i = lk; i; i--) {
		w = w * 10 + k[i];
		ans0[i] = w / x;
		w %= x;
		if (ans0[i] && flag) {
			la0 = i;
			flag = 0;
		}
	}
}

bool pd() {
	if (la != la0) return la < la0;
	for (int i = la; i; i--)
		if (ans[i] != ans0[i]) return ans[i] < ans0[i];
	return 0;
}

int main() {
	cin >> n;
	for (int i = 0; i <= n; i++) scanf("%d %d", &p[i].a, &p[i].b);
	sort(p + 1, p + n + 1);
	memset(k, 0, sizeof(k));
	memset(ans, 0, sizeof(ans));
	memset(ans0, 0, sizeof(ans0));
	k[1] = 1;
	gj1(p[0].a);
	for (int i = 1; i <= n; i++) {
		gj2(p[i].b);
		if (pd()) {
			memcpy(ans, ans0, sizeof(ans));
			la = la0;
		}
		gj1(p[i].a);
	}
	for (int i = la; i; i--) cout << ans[i];
	cout << endl;
	return 0;
}


```

yxc解法

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


**Color a Tree**

POJ2054 Color a Tree

```cpp
//Author:XuHt
#include <cstdio>
#include <vector>
#include <cstring>
#include <iostream>
using namespace std;
const int N = 1006;
int n, r, fa[N], nxt[N], lst[N], num[N];
double c[N], d[N], tot[N];
bool v[N];

void Color_a_Tree() {
    for (int i = 1; i <= n; i++) {
        scanf("%lf", &c[i]);
        nxt[i] = i;
        lst[i] = i;
        num[i] = 1;
        tot[i] = c[i];
    }
    memcpy(d, c, sizeof(d));
    for (int i = 1; i < n; i++) {
        int a, b;
        scanf("%d %d", &a, &b);
        fa[b] = a;
    }
    memset(v, 0, sizeof(v));
    for (int i = 1; i < n; i++) {
        int p;
        double k = 0;
        for (int j = 1; j <= n; j++)
            if (j != r && !v[j] && c[j] > k) {
                k = c[j];
                p = j;
            }
        int f = fa[p];
        while (v[f]) fa[p] = f = fa[f];
        nxt[lst[f]] = p;
        lst[f] = lst[p];
        num[f] += num[p];
        tot[f] += tot[p];
        c[f] = tot[f] / num[f];
        v[p] = 1;
    }
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        ans += i * d[r];
        r = nxt[r];
    }
    cout << ans << endl;
}

int main() {
    while (cin >> n >> r && n && r) Color_a_Tree();
    return 0;
}

```

yxc解法

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




