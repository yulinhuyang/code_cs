#### KMP 

模板题 AcWing 831. KMP字符串

```cpp
// s[]是长文本，p[]是模式串，n是s的长度，m是p的长度
求模式串的Next数组：
for (int i = 2, j = 0; i <= m; i ++ )
{
    while (j && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j ++ ;
    ne[i] = j;
}

// 匹配
for (int i = 1, j = 0; i <= n; i ++ )
{
    while (j && s[i] != p[j + 1]) j = ne[j];
    if (s[i] == p[j + 1]) j ++ ;
    if (j == m)
    {
        j = ne[j];
        // 匹配成功后的逻辑
    }
}
```


#### Trie树 

模板题 AcWing 835. Trie字符串统计

```cpp
int son[N][26], cnt[N], idx;
// 0号点既是根节点，又是空节点
// son[][]存储树中每个节点的子节点
// cnt[]存储以每个节点结尾的单词数量

// 插入一个字符串
void insert(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
    cnt[p] ++ ;
}

// 查询字符串出现的次数
int query(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}
```

#### 并查集 

模板题 AcWing 836. 合并集合, AcWing 837. 连通块中点的数量

(1)朴素并查集：
```cpp
    int p[N]; //存储每个点的祖宗节点

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
```

(2)维护size的并查集：
```cpp
    int p[N], size[N];
    //p[]存储每个点的祖宗节点, size[]只有祖宗节点的有意义，表示祖宗节点所在集合中的点的数量

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        size[i] = 1;
    }

    // 合并a和b所在的两个集合：
    size[find(b)] += size[find(a)];
    p[find(a)] = find(b);
```

(3)维护到祖宗节点距离的并查集：
```cpp
    int p[N], d[N];
    //p[]存储每个点的祖宗节点, d[x]存储x到p[x]的距离

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x)
        {
            int u = find(p[x]);
            d[x] += d[p[x]];
            p[x] = u;
        }
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        d[i] = 0;
    }

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
    d[find(a)] = distance; // 根据具体问题，初始化find(a)的偏移量
```

#### 堆 

模板题 AcWing 838. 堆排序, AcWing 839. 模拟堆

```cpp
// h[N]存储堆中的值, h[1]是堆顶，x的左儿子是2x, 右儿子是2x + 1
// ph[k]存储第k个插入的点在堆中的位置
// hp[k]存储堆中下标是k的点是第几个插入的
int h[N], ph[N], hp[N], size;

// 交换两个点，及其映射关系
void heap_swap(int a, int b)
{
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}

void down(int u)
{
    int t = u;
    if (u * 2 <= size && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t)
    {
        heap_swap(u, t);
        down(t);
    }
}

void up(int u)
{
    while (u / 2 && h[u] < h[u / 2])
    {
        heap_swap(u, u / 2);
        u >>= 1;
    }
}

// O(n)建堆
for (int i = n / 2; i; i -- ) down(i);
```

**一般哈希** 

模板题 AcWing 840. 模拟散列表

(1) 拉链法

```cpp
    int h[N], e[N], ne[N], idx;

    // 向哈希表中插入一个数
    void insert(int x)
    {
        int k = (x % N + N) % N;
        e[idx] = x;
        ne[idx] = h[k];
        h[k] = idx ++ ;
    }

    // 在哈希表中查询某个数是否存在
    bool find(int x)
    {
        int k = (x % N + N) % N;
        for (int i = h[k]; i != -1; i = ne[i])
            if (e[i] == x)
                return true;

        return false;
    }
```

(2) 开放寻址法

```cpp
    int h[N];

    // 如果x在哈希表中，返回x的下标；如果x不在哈希表中，返回x应该插入的位置
    int find(int x)
    {
        int t = (x % N + N) % N;
        while (h[t] != null && h[t] != x)
        {
            t ++ ;
            if (t == N) t = 0;
        }
        return t;
    }
```

**字符串哈希**

模板题 AcWing 841. 字符串哈希

核心思想：将字符串看成P进制数，P的经验值是131或13331，取这两个值的冲突概率低

小技巧：取模的数用2^64，这样直接用unsigned long long存储，溢出的结果就是取模的结果

```cpp
typedef unsigned long long ULL;
ULL h[N], p[N]; // h[k]存储字符串前k个字母的哈希值, p[k]存储 P^k mod 2^64

// 初始化
p[0] = 1;
for (int i = 1; i <= n; i ++ )
{
    h[i] = h[i - 1] * P + str[i];
    p[i] = p[i - 1] * P;
}

// 计算子串 str[l ~ r] 的哈希值
ULL get(int l, int r)
{
    return h[r] - h[l - 1] * p[r - l + 1];
}

```

#### C++ STL简介

vector, 变长数组，倍增的思想

    size()  返回元素个数
    empty()  返回是否为空
    clear()  清空
    front()/back()
    push_back()/pop_back()
    begin()/end()
    []
    支持比较运算，按字典序

pair<int, int>

    first, 第一个元素
    second, 第二个元素
    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）

string，字符串

    size()/length()  返回字符串长度
    empty()
    clear()
    substr(起始下标，(子串长度))  返回子串
    c_str()  返回字符串所在字符数组的起始地址

queue, 队列

    size()
    empty()
    push()  向队尾插入一个元素
    front()  返回队头元素
    back()  返回队尾元素
    pop()  弹出队头元素

priority_queue, 优先队列，默认是大根堆

    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;

stack, 栈

    size()
    empty()
    push()  向栈顶插入一个元素
    top()  返回栈顶元素
    pop()  弹出栈顶元素

deque, 双端队列

    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []

set, map, multiset, multimap, 基于平衡二叉树（红黑树），动态维护有序序列

    size()
    empty()
    clear()
    begin()/end()
    ++, -- 返回前驱和后继，时间复杂度 O(logn)
    
    set/multiset
    
        insert()  插入一个数
        find()  查找一个数
        count()  返回某一个数的个数
        erase()
            (1) 输入是一个数x，删除所有x   O(k + logn)
            (2) 输入一个迭代器，删除这个迭代器
        lower_bound()/upper_bound()
            lower_bound(x)  返回大于等于x的最小的数的迭代器
            upper_bound(x)  返回大于x的最小的数的迭代器
    
    map/multimap
    
        insert()  插入的数是一个pair
        erase()  输入的参数是pair或者迭代器
        find()
        []  注意multimap不支持此操作。 时间复杂度是 O(logn)
        lower_bound()/upper_bound()

unordered_set, unordered_map, unordered_multiset, unordered_multimap, 哈希表

    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--

bitset, 圧位

    bitset<10000> s;
    ~, &, |, ^
    >>, <<
    ==, !=
    []
    
    count()  返回有多少个1
    
    any()  判断是否至少有一个1
    none()  判断是否全为0
    
    set()  把所有位置成1
    set(k, v)  将第k位变成v
    reset()  把所有位变成0
    flip()  等价于~
    flip(k) 把第k位取反


### 0x11栈

**HDOJ4699 Editor**

```cpp
//Author:XuHt
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 1000006, INF = 0x3f3f3f3f;
int q, st1[N], st2[N], s[N], f[N];

void Editor() {
	int t1 = 0, t2 = 0;
	while (q--) {
		char c[2];
		scanf("%s", c);
		switch (c[0]) {
			case 'I':
				scanf("%d", &st1[++t1]);
				s[t1] = s[t1-1] + st1[t1];
				f[t1] = max(f[t1-1], s[t1]);
				continue;
			case 'D':
				if (t1) t1--;
				continue;
			case 'L':
				if (t1) st2[++t2] = st1[t1--];
				continue;
			case 'R':
				if (!t2) continue;
				st1[++t1] = st2[t2--];
				s[t1] = s[t1-1] + st1[t1];
				f[t1] = max(f[t1-1], s[t1]);
				continue;
			case 'Q':
				int k;
				scanf("%d", &k);
				printf("%d\n", f[k]);
		}
	}
}

int main() {
	s[0] = 0;
	f[0] = -INF;
	while (cin >> q) Editor();
	return 0;
}
```

**火车进出栈问题**

CH1102

方法三


```cpp
//Author:XuHt
#include <iostream>
#define ll long long
using namespace std;
const int N = 1006;
ll f[N][N];

int main() {
	int n;
	cin >> n;
	f[0][0] = 1;
	for (int i = 0; i <= n; i++)
		for (int j = 0; j <= n - i; j++)
			if (i || j) f[i][j] = (i ? f[i-1][j+1] : 0) + (j ? f[i][j-1] : 0);
	cout << f[n][0] << endl;
	return 0;
}

```

方法四

```cpp
//Author:XuHt
#include <cstdio>
#include <cstring>
#include <iostream>
using namespace std;
const int N = 60006;
int ans[1000000], len = 1, p[N*2], c[N*2], tot = 0;
bool v[N*2];

void gj(int x) {
	for (int i = 1; i <= len; i++) ans[i] *= x;
	len += 6;
	for (int i = 1; i <= len; i++) {
		ans[i+1] += ans[i] / 10;
		ans[i] %= 10;
	}
	while (!ans[len]) len--;
}

int main() {
	int n;
	cin >> n;
	ans[0] = 0;
	ans[1] = 1;
	memset(v, 0, sizeof(v));
	for (int i = 2; i <= n * 2; i++)
		if (!v[i]) {
			p[++tot] = i;
			for (int j = i; j <= n * 2; j += i) v[j] = 1;
		}
	memset(c, 0, sizeof(c));
	for (int i = 1; i <= tot; i++) {
		int a = p[i];
		while (a <= n * 2) {
			c[i] += n * 2 / a - n / a - (n + 1) / a;
			a *= p[i];
		}
		while (c[i]--) gj(p[i]);
	}
	for (int i = len; i; i--) printf("%d", ans[i]);
	cout << endl;
	return 0;
}
```


**Largest Rectangle in a Histogram**

```cpp
//Author:XuHt
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#define ll long long
using namespace std;
const int N = 100006;
int n, a[N], s[N], w[N];

void Largest(int n) {
	memset(s, 0, sizeof(s));
	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
	a[n+1] = 0;
	int p = 0;
	ll ans = 0;
	for (int i = 1; i <= n + 1; i++)
		if (a[i] > s[p]) {
			s[++p] = a[i];
			w[p] = 1;
		} else {
			int wid = 0;
			while (s[p] > a[i]) {
				wid += w[p];
				ans = max(ans, (ll)wid * s[p]);
				p--;
			}
			s[++p] = a[i];
			w[p] = wid + 1;
		}
	cout << ans << endl;
}

int main() {
	while (cin >> n && n) Largest(n);
	return 0;
}
```
### 0x12 队列

**Team Queue**

POJ2259 Team Queue


```cpp
//Author:XuHt
#include <queue>
#include <cstdio>
#include <iostream>
using namespace std;
const int N = 1000000, T = 1006;
int t, f[N], id = 0;
char s[10];
queue<int> q[T];

void Team_Queue() {
	q[0] = queue<int>();
	for (int i = 1; i <= t; i++) {
		int n;
		scanf("%d", &n);
		while (n--) {
			int x;
			scanf("%d", &x);
			f[x] = i;
		}
		q[i] = queue<int>();
	}
	cout << "Scenario #" << ++id << endl;
	while (scanf("%s", s) && s[0] != 'S') {
		if (s[0] == 'E') {
			int x;
			scanf("%d", &x);
			if (q[f[x]].empty()) q[0].push(f[x]);
			q[f[x]].push(x);
		} else {
			int x = q[0].front();
			printf("%d\n", q[x].front());
			q[x].pop();
			if (q[x].empty()) q[0].pop();
		}
	}
	cout << endl;
}

int main() {
	while (cin >> t && t) Team_Queue();
	return 0;
}

```

**蚯蚓**

NOIP2016／CH1202 蚯蚓

```cpp

//Author:XuHt
#include <queue>
#include <cstdio>
#include <iostream>
#include <algorithm>
#define ll long long
using namespace std;
const ll INF = 0x3f3f3f3f3f3f3f3f;
int n, m, q, u, v, t, delta = 0;
priority_queue<ll> pq;
queue<ll> q1, q2;

int main() {
	cin >> n >> m >> q >> u >> v >> t;
	for (int i = 1; i <= n; i++) {
		ll a;
		scanf("%lld", &a);
		pq.push(a);
	}
	for (int i = 1; i <= m; i++) {
		ll maxx = -INF;
		int w;
		if (pq.size() && maxx < pq.top()) {
			maxx = pq.top();
			w = 0;
		}
		if (q1.size() && maxx < q1.front()) {
			maxx = q1.front();
			w = 1;
		}
		if (q2.size() && maxx < q2.front()) {
			maxx = q2.front();
			w = 2;
		}
		if (w == 1) q1.pop();
		else if (w == 2) q2.pop();
		else pq.pop();
		maxx += delta;
		q1.push(maxx * u / v - delta - q);
		q2.push(maxx - maxx * u / v - delta - q);
		delta += q;
		if (i % t == 0) printf("%lld ", maxx);
	}
	cout << endl;
	for (int i = 1; i <= n + m; i++) {
		ll maxx = -INF;
		int w;
		if (pq.size() && maxx < pq.top()) {
			maxx = pq.top();
			w = 0;
		}
		if (q1.size() && maxx < q1.front()) {
			maxx = q1.front();
			w = 1;
		}
		if (q2.size() && maxx < q2.front()) {
			maxx = q2.front();
			w = 2;
		}
		if (w == 1) q1.pop();
		else if (w == 2) q2.pop();
		else pq.pop();
		if (i % t == 0) printf("%lld ", maxx + delta);
	}
	cout << endl;
	return 0;
}
```


### 双端队列

BZOJ2457 双端队列

```cpp
//Author:XuHt
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 200006, INF = 0x3f3f3f3f;
pair<int, int> a[N];
int n;
vector<int> p[N];

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) {
		scanf("%d", &a[i].first);
		a[i].second = i;
	}
	sort(a + 1, a + n + 1);
	int t = 0;
	for (int i = 1; i <= n; i++) {
		p[++t].push_back(a[i].second);
		while (a[i].first == a[i+1].first)
			p[t].push_back(a[++i].second);
	}
	for (int i = 1; i <= t; i++) sort(p[i].begin(), p[i].end());
	bool flag = 0;
	int num = INF, ans = 1;
	for (int i = 1; i <= t; i++) {
		int s = p[i].size();
		if (flag) {
			if (num < p[i][0]) num = p[i][s-1];
			else {
				++ans;
				flag = 0;
				num = p[i][0];
			}
		}
		else {
			if (num > p[i][s-1]) num = p[i][0];
			else {
				flag = 1;
				num = p[i][s-1];
			}
		}
	}
	cout << ans << endl;
	return 0;
}

```
### 最大子序和

CH1201 最大子序
```cpp
//Author:XuHt
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 300006, INF = 0x3f3f3f3f;
int n, m, s[N], q[N];

int main() {
	cin >> n >> m;
	s[0] = 0;
	for (int i = 1; i <= n; i++) {
		scanf("%d", &s[i]);
		s[i] += s[i-1];
	}
	int l = 1, r = 1, ans = -INF;
	q[1] = 0;
	for (int i = 1; i <= n; i++) {
		while (l <= r && q[l] < i - m) l++;
		ans = max(ans, s[i] - s[q[l]]);
		while (l <= r && s[q[r]] >= s[i]) r--;
		q[++r] = i;
	}
	cout << ans << endl;
	return 0;
}
```


### 0x13 链表与邻接表

CH1301 邻值查找

解法一

```cpp
//Author:XuHt
#include <set>
#include <cstdio>
#include <iostream>
using namespace std;
const int INF = 0x7f7f7f7f;
set<pair<int, int> > s;

int main() {
	int n, a;
	cin >> n >> a;
	s.insert(make_pair(a, 1));
	for (int i = 2; i <= n; i++) {
		scanf("%d", &a);
		s.insert(make_pair(a, i));
		set<pair<int, int> >::iterator it = s.find(make_pair(a, i));
		pair<int, int> ans;
		ans.first = INF;
		if (++it != s.end())
			ans = make_pair((*it).first - a, (*it).second);
		it = s.find(make_pair(a, i));
		if (it-- != s.begin() && ans.first >= a - (*it).first)
			ans = make_pair(a - (*it).first, (*it).second);
		cout << ans.first << " " << ans.second << endl;
	}
	return 0;
}
```
解法二
```cpp
//Author:XuHt
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 100006;
struct A {
	int a, w, prv, nxt;
	bool operator < (const A x) const {
		return a < x.a;
	}
} a[N];
int n, b[N];
struct ANS {
	int x, k;
} ans[N];

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) {
		scanf("%d", &a[i].a);
		a[i].w = i;
	}
	sort(a + 1, a + n + 1);
	for (int i = 1; i <= n; i++) {
		b[a[i].w] = i;
		a[i].prv = i - 1;
		a[i].nxt = i + 1;
	}
	int l = 1, r = n;
	for (int i = n; i > 1; i--) {
		if (b[i] == r) {
			ans[i].x = a[r].a - a[a[r].prv].a;
			ans[i].k = a[a[r].prv].w;
			r = a[r].prv;
		} else if (b[i] == l) {
			ans[i].x = a[a[l].nxt].a - a[l].a;
			ans[i].k = a[a[l].nxt].w;
			l = a[l].nxt;
		} else {
			ans[i].x = a[a[b[i]].nxt].a - a[b[i]].a;
			ans[i].k = a[a[b[i]].nxt].w;
			if (a[b[i]].a - a[a[b[i]].prv].a <= ans[i].x) {
				ans[i].x = a[b[i]].a - a[a[b[i]].prv].a;
				ans[i].k = a[a[b[i]].prv].w;
			}
		}
		a[a[b[i]].prv].nxt = a[b[i]].nxt;
		a[a[b[i]].nxt].prv = a[b[i]].prv;
	}
	for (int i = 2; i <= n; i++) printf("%d %d\n", ans[i].x, ans[i].k);
	return 0;
}

```

### 0x14 Hash

### 0x15 字符串/Period

### 0x16 Trie

### 0x17 二叉堆

### 0x41 并查集
