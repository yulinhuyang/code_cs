算法基础课及模板：

算法基础课： https://www.acwing.com/activity/content/11/

yxc代码模板合集： https://blog.csdn.net/weixin_45629285/category_10218699.html

代码基础课模板： https://github.com/kszlzj/algorithm

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




