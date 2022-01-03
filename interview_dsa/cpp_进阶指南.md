主要参考:

 https://github.com/livrth/algorithm-competition-advanced-guide
 
 https://github.com/820fans/ACWinger-ClockIn
 
 https://github.com/livrth/cp


算法竞赛进阶指南 杂谈 https://blog.nowcoder.net/zhxu98/2671

算法竞赛进阶指南题解笔记  秦淮岸： https://www.acwing.com/blog/content/89/

大雪菜：https://blog.csdn.net/GarfieldEr007



## 0x00 基本算法

### 0x02递推与递归

**递归实现指数型枚举**

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;
int n;
vector<int> chosen; // 被选择的数
void calc(int x) {
	if (x == n + 1) { // 问题边界
		for (int i = 0; i < chosen.size(); i++)
			printf("%d ", chosen[i]);
		puts("");
		return;
	}
    //"不选x"分支
	calc(x + 1); // 求解子问题
	//"选x"分支
	chosen.push_back(x); // 记录x已被选择
	calc(x + 1); // 求解子问题 
	chosen.pop_back(); // 准备回溯到上一问题之前，还原现场
}
int main() {
	cin >> n;
	calc(1);  // 主函数中的调用入口
}
```

yxc解法

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
void dfs(int x,int now)
{
    if (x>n)
        return ;
    if (x==n)
    {
        for (int i=1;i<=n;i++)
            if (now>>(i-1) & 1)
               cout<<i<<" ";
        puts("");
    }
    dfs(x+1,now<<1 | 1);
    dfs(x+1,now<<1);
}
int main()
{
    cin>>n;
    dfs(0,0);
}

```


**组合型枚举**

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;
int n, m;
vector<int> chosen; 
void calc(int x) {
	if (chosen.size() > m || chosen.size() + (n - x + 1) < m) return;
	if (x == n + 1) {
		for (int i = 0; i < chosen.size(); i++)
			printf("%d ", chosen[i]);
		puts("");
		return;
	}
	chosen.push_back(x);
	calc(x + 1);
	chosen.pop_back();
	calc(x + 1);
}
int main() {
	cin >> n >> m;
	calc(1);
}
```
yxc解法
```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
void dfs(int x,int now,int date)
{
    if (x>n || date+(n-x+1)<m)
        return ;
    if (date==m)
    {
        for (int i=1;i<=n;i++)
            if (now>>(i-1) & 1)
               cout<<i<<" ";
        puts("");
        return ;
    }
    dfs(x+1,now+(1<<x),date+1);
    dfs(x+1,now,date);
}
int main()
{
    cin>>n>>m;
    dfs(0,0,0);
}

```


**递归实现排列型枚举**

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;
int n;
int order[20]; // 按顺序依次记录被选择的整数
bool chosen[20]; // 标记被选择的整数
void calc(int k) {
	if (k == n + 1) { // 问题边界
		for (int i = 1; i <= n; i++)
			printf("%d ", order[i]);
		puts("");
		return;
	}
	for (int i = 1; i <= n; i++) { 
		if (chosen[i]) continue;
		order[k] = i;
		chosen[i] = 1;
		calc(k + 1); 
		chosen[i] = 0;
		order[k] = 0; // 这一行可以省略
	}
}
int main() { 
	cin >> n;
	calc(1);  // 主函数中的调用入口
}

```



yxc解法

```cpp
#include <bits/stdc++.h>
using namespace std;
int q[110],n;
void dfs(int x,int now)
{
    if (q[0]==n)
    {
        for (int i=1;i<=q[0];i++)
            cout<<q[i]<<" ";
        puts("");
        return ;
    }
    for (int i=1;i<=n;i++)
        if (!(now>>(i-1) & 1))
        {
            q[++q[0]]=i;
            dfs(i+1,now+(1<<(i-1)));
            q[q[0]--]=0;
        }
    return ;
}
int main()
{
    cin>>n;
    dfs(1,0);
}

```
**费解的开关**

CH0201 费解的开关

```cpp
//Author:XuHt
#include <cstring>
#include <iostream>
using namespace std;
const int N = 6;
int a[N], ans, aa[N];
char s[N];

void dj(int x, int y) {
	aa[x] ^= (1 << y);
	if (x != 1) aa[x-1] ^= (1 << y);
	if (x != 5) aa[x+1] ^= (1 << y);
	if (y != 0) aa[x] ^= (1 << (y - 1));
	if (y != 4) aa[x] ^= (1 << (y + 1));
}

void pd(int p) {
	int k = 0;
	memcpy(aa, a, sizeof(a));
	for (int i = 0; i < 5; i++)
		if (!((p >> i) & 1)) {
			dj(1, i);
			if (++k >= ans) return;
		}
	for (int x = 1; x < 5; x++)
		for (int y = 0; y < 5; y++)
			if (!((aa[x] >> y) & 1)) {
				dj(x + 1, y);
				if (++k >= ans) return;
			}
	if (aa[5] == 31) ans = k;
}

void abc() {
	memset(a, 0, sizeof(a));
	for (int i = 1; i <= 5; i++) {
		cin >> (s + 1);
		for (int j = 1; j <= 5; j++) a[i] = a[i] * 2 + (s[j] - '0');
	}
	ans = 7;
	for (int p = 0; p < (1 << 5); p++) pd(p);
	if (ans == 7) cout << "-1" << endl;
	else cout << ans << endl;
}

int main() {
	int n;
	cin >> n;
	while (n--) abc();
	return 0;
}

```

acwing解法


```cpp


```

**Strange Towers of Hanoi**

POJ1958 Strange Towers of Hanoi

```cpp
//Author:XuHt
#include <cstring>
#include <iostream>
#include <algorithm>
#define ll long long
using namespace std;
const int N = 15;
ll d[N], f[N];

int main() {
	int n = 12;
	memset(f, 0x3f, sizeof(f));
	d[1] = f[1] = 1;
	for (int i = 2; i <= n; i++) d[i] = 2 * d[i-1] + 1;
	for (int i = 2; i <= n; i++)
		for (int j = 1; j < i; j++)
			f[i] = min(f[i], 2 * f[j] + d[i-j]);
	for (int i = 1; i <= n; i++) cout << f[i] << endl;
	return 0;
}
```

acwing解法

```cpp
void hanoi(int n, char a, char b, char c) {
    if(n==1){
        printf("%c -> %c\n", a, c);
    }
    else{
        hanoi(n-1, a, c, b);
        printf("%c -> %c\n", a, c);
        hanoi(n-1, b, a, c);
    }
}

int hanoi_3_count(int n){
    int res = 1;
    for(int i=2;i<=n;i++)
    res = res*2 + 1;
    return res;
}

void hanoi_4_count(){
    vector<int> dp(13, 0);
    // vector<int> f(13, 0x3f);
    vector<int> f(13, INT_MAX);
    // int f[13];
    // memset(f,0x3f,sizeof(f));
    dp[1]=1;
    for(int i=2;i<=12;i++) dp[i]=2*dp[i-1]+1;
    f[0]=0;
    for(int i=1;i<=12;i++) {
        for(int j=0;j<i;j++){
            f[i] = min(f[i], 2*f[j] + dp[i-j]);
        }
    }
    for(int i=1;i<=12;i++) cout<<f[i]<<endl;
}

int main(){
    // hanoi(2, 'a', 'b', 'c');
    hanoi_4_count();
    return 0;
}
```

**Fractal Streets**

POJ3889

```cpp
#include <cmath>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;

pair<long long, long long> calc(int n, long long m) {
    if (n == 0) return make_pair(0, 0);
    long long len = 1ll << (n - 1), cnt = 1ll << (2 * n - 2);
    pair<long long, long long> pos = calc(n - 1, m % cnt); // 递归
    long long x = pos.first, y = pos.second;
    long long z = m / cnt;
    if (z == 0) return make_pair(y, x);
    if (z == 1) return make_pair(x, y + len);
    if (z == 2) return make_pair(x + len, y + len);
    if (z == 3) return make_pair(2 * len - y - 1, len - x - 1);
}

int main () {
    int data; for (scanf("%d", &data); data; --data) {
        int n; long long h, o;
        scanf("%d %I64d %I64d", &n, &h, &o);
        pair<long long, long long> hp = calc(n, h - 1);
        pair<long long, long long> op = calc(n, o - 1);
        long long dx = hp.first - op.first, dy = hp.second - op.second;
        printf("%.0f\n", (double)sqrt(dx * dx + dy * dy) * 10);
    }
    return 0;
}

```

acwing解法
```cpp
pair<ll, ll> calc(int n, ll m){
    if(n==0) return make_pair(0,0);
    
    ll len = 1ll<<(n-1), cnt= 1ll<<(2*n-2);
    pair<ll, ll> pos = calc(n-1, m%cnt);
    ll x=pos.first, y = pos.second;
    
    ll z=m/cnt;
    if(z==0) return make_pair(y, x);
    if(z==1) return make_pair(x, y+len);
    if(z==2) return make_pair(x+len, y+len);
    if(z==3) return make_pair(2*len-y-1, len-x-1);
}

int main(){
    int t;
    cin>>t;
    while(t--){
        ll n, s, d;
        cin>>n>>s>>d;
        pair<ll, ll> sp = calc(n, s-1);
        pair<ll, ll> dp = calc(n, d-1);
        ll dx=sp.first-dp.first,dy=sp.second-dp.second;
        double ans=(sqrt(dx*dx+dy*dy)*10); //每个街区都是边长为 10 米的正方形
        printf("%0.lf\n",ans);
    }
    return 0;
}

```


**非递归_模拟系统栈**
```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;
vector<int> chosen;
int stack[100010], top = 0, address = 0, n, m;
void call(int x, int ret_addr) { // 模拟计算机汇编指令call
	int old_top = top;
	stack[++top] = x; // 参数x
	stack[++top] = ret_addr; // 返回地址标号
	stack[++top] = old_top; // 在栈顶记录以前的top值
}
int ret() { // 模拟计算机汇编指令ret
	int ret_addr = stack[top - 1];
	top = stack[top]; // 恢复以前的top值
	return ret_addr;
}

int main() {
	cin >> n >> m;
	call(1, 0); // calc(1)
	while (top) {
		int x = stack[top - 2]; // 获取参数
		switch (address) {
		case 0:
			if (chosen.size()>m || chosen.size()+(n-x+1)<m) {
				address = ret(); // return
				continue;
			}
			if (x == n + 1) {
				for (int i = 0; i < chosen.size(); i++)
					printf("%d ", chosen[i]);
				puts("");
				address = ret(); // return
				continue;
			}
			chosen.push_back(x);
			call(x+1, 1); // 相当于calc(x+1)，返回后会从case 1继续
			address = 0;
			continue; // 回到while循环开头，相当于开始新的递归
		case 1:
			chosen.pop_back();
			call(x+1, 2); // 相当于calc(x+1)，返回后会从case 2继续
			address = 0;
			continue; // 回到while循环开头，相当于开始新的递归
		case 2:
			address = ret(); // 相当于原calc函数结尾，执行return
		}
	}
}
```


### 0x03前缀和与差分

**激光炸弹**

BZOJ1218 激光炸弹

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 5006;
int n, r, s[N][N];

int main() {
	memset(s, 0, sizeof(s));
	cin >> n >> r;
	while (n--) {
		int x, y, z;
		scanf("%d %d %d", &x, &y, &z);
		s[x][y] += z;
	}
	for (int i = 0; i <= 5000; i++)
		for (int j = 0; j <= 5000; j++)
			if (!i && !j) continue;
			else if (!i) s[i][j] += s[i][j-1];
			else if (!j) s[i][j] += s[i-1][j];
			else s[i][j] += s[i-1][j] + s[i][j-1] - s[i-1][j-1];
	int ans = 0;
	if(r<5001){
		for (int i = r - 1; i <= 5000; i++)
			for (int j = r - 1; j <= 5000; j++)
				if (i == r - 1 && j == r - 1) ans = max(ans, s[i][j]);
				else if (i == r - 1) ans = max(ans, s[i][j] - s[i][j-r]);
				else if (j == r - 1) ans = max(ans, s[i][j] - s[i-r][j]);
				else ans = max(ans, s[i][j] - s[i-r][j] - s[i][j-r] + s[i-r][j-r]);
	}
	else{	//r>=5001，可以覆盖所有区域
		ans=s[5000][5000];
	}
	cout << ans << endl;
	return 0;
}


#include<iostream>
#include<cstring>
#define N 5001
using namespace std;

// int a[N][N]; // 直接申请两个数组超内存
int s[N][N];
int ni,r;

int main(){
    cin>>ni>>r;
    // memset(a, 0, sizeof(a));
    memset(s, 0, sizeof(s));
    int xi,yi,vi,maxn;
    for(int i=0;i<ni;i++){
        cin>>xi>>yi>>vi;
        s[xi][yi] = vi;
        maxn=max(maxn, max(xi, yi));
    }
    int n=maxn+1;//数组大小=最大下标+1
    
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            int v1 = (i>0 && j>0)?s[i-1][j-1]:0;
            int v2 = (i>0)?s[i-1][j]:0;
            int v3 = (j>0)?s[i][j-1]:0;
            // s[i-1][j]
            // s[i][j-1]
            // a[i][j]
            s[i][j] = v2+v3-v1+s[i][j];
        }
    }
    if(r>n){
        cout<<s[n-1][n-1]<<endl;
        return 0;
    }
    int maxv=0;
    for(int i=r-1;i<n;i++){
        for(int j=r-1;j<n;j++){
            int ri=i-r, rj=j-r;
            int v1 = (ri>=0&&rj>=0)?s[ri][rj]:0;
            int v2 = (ri>=0)?s[ri][j]:0;
            int v3 = (rj>=0)?s[i][rj]:0;
            maxv=max(maxv, s[i][j]-v2-v3+v1);
        }
    }
    cout<<maxv<<endl;
    return 0;
}
```

**IncDec Sequence**

CH0304 IncDec Sequence

```cpp

//Author:XuHt
#include <cmath>
#include <cstdio>
#include <iostream>
#define ll long long
using namespace std;
const int N = 100006;
ll a[N], b[N];

int main() {
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++) scanf("%lld", &a[i]);
	b[1] = a[1];
	for (int i = 2; i <= n; i++) b[i] = a[i] - a[i-1];
	ll p = 0, q = 0;
	for (int i = 2; i <= n; i++)
		if (b[i] > 0) p += b[i];
		else if (b[i] < 0) q -= b[i];
	cout << max(p, q) << endl << abs(p - q) + 1 << endl;
	return 0;
}


#include<iostream>
#define N 100001
#define ll long long
using namespace std;

int a[N],b[N];


int main(){
    int n;
    cin>>n;
    for(int i=0;i<n;i++){
        cin>>a[i];
        if(i==0) b[i]=a[i];
        else b[i]=a[i]-a[i-1];
    }
    if(n<=1){
        cout<<0<<endl<<1<<endl;
        return 0;
    }
    ll p=0, q=0;
    for(int i=1;i<n;i++){
        if(b[i]>0) p+=b[i];
        else if(b[i]<0){
            q+=abs(b[i]);
        }
    }
    ll op_num = max(p, q);
    ll res_num = abs(p-q)+1ll;
    cout<<op_num<<endl;
    cout<<res_num<<endl;
    // more than one item
    return 0;
}
```

**Tallest Cow**

POJ3263 Tallest Cow

```cpp
//Author:XuHt
#include <map>
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
map<pair<int, int>, bool> v;
const int N = 10006;
int s[N];

int main() {
    int n, p, h, t;
    cin >> n >> p >> h >> t;
    memset(s, 0, sizeof(s));
    while (t--) {
        int a, b;
        scanf("%d %d", &a, &b);
        if (a > b) swap(a, b);
        if (v[make_pair(a, b)]) continue;
        s[a+1]--;
        s[b]++;
        v[make_pair(a, b)] = 1;
    }
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        ans += s[i];
        printf("%d\n", h + ans);
    }
    return 0;
}



#include<bits/stdc++.h>

#define N 10001
using namespace std;

int a[N], b[N];

int main(){
    int n,p,h,m;
    cin>>n>>p>>h>>m;
    for(int i=0;i<n;i++)
    a[i]=h;
    memset(b, 0, sizeof(b));
    
    int ha, hb;
    set<pair<int, int>> rec;
    for(int i=0;i<m;i++){
        cin>>ha>>hb;
        ha--;hb--;// response to index
        int h1 = min(ha, hb), h2=max(ha, hb);
        if((h2-h1)<=1) continue; //neighbor ox
        
        rec.insert({h1, h2});
    }
    // operate
    for(auto item: rec){
        int h1=item.first, h2=item.second;
        b[h1+1]--;
        b[h2]++;
    }
    
    cout<<a[0]<<endl;
    for(int i=1;i<n;i++){
        a[i] = a[i-1] + b[i];
        cout<<a[i]<<endl;
    }
    return 0;
}
```

### 0x04 二分

**Best Cow Fence**

POJ2018

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
double a[100001], b[100001], sum[100001];
int main() {
	int N, L;
	cin >> N >> L;
	for (int i = 1; i <= N; i++) scanf("%lf", &a[i]);
	double eps = 1e-5;
	double l = -1e6, r = 1e6;
	while (r - l > eps) {
		double mid = (l + r) / 2;
		for (int i = 1; i <= N; i++) b[i] = a[i] - mid;
		for (int i = 1; i <= N; i++)
			sum[i] = (sum[i - 1] + b[i]);
		double ans = -1e10;
		double min_val = 1e10;
		for (int i = L; i <= N; i++) {
			min_val = min(min_val, sum[i - L]);
			ans = max(ans, sum[i] - min_val);
		}
		if (ans >= 0) l = mid; else r = mid;
	}
	cout << int(r * 1000) << endl;
}
```

**Innovative Business**



```cpp
// Forward declaration of specialSort API.
// bool specialSort(int a, int b);
// return bool means whether a is less than b.

class Solution {
public:
    vector<int> specialSort(int N) {
        vector<int> a;
        for(int i=0;i<N;i++){
            a.push_back(i+1);
        }
        for (int i=1;i<N;i++){
            int temp = a[i]; 
            int left = 0;
            int right = i - 1;
            int mid = 0;
            while (left <= right) {
                mid = (left + right) / 2;
                if (compare(temp,a[mid])) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            for (int j = i - 1; j >= left; j--) {
                a[j + 1] = a[j];
            }
            if (left != i) {
                a[left] = temp;
            }
        }
        return a;
    }
};

```

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

## 0x10基本数据结构

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






