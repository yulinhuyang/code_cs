### 平台开源

[力扣](https://leetcode-cn.com/problemset/all/)

[leetcode食用指南](https://github.com/azl397985856/leetcode)

[牛客网](https://www.nowcoder.com/)

[Comet OJ](https://www.cometoj.com/contests)

[HDU](http://acm.hdu.edu.cn/)

[HihoCoder](https://hihocoder.com/)

[洛谷](https://www.luogu.org/)

[LibreOJ(算法竞赛一本通——提高篇)](https://loj.ac)

**OI wiki**

[学习资源](https://oi-wiki.org/intro/resources/)  

[OI wiki](https://oi-wiki.org/)

[OI & ACM 课件分享](https://github.com/hzwer/shareOI)  

Just Code [[github]](https://github.com/YaxeZhang/Just-Code) 
  
写在20年初的校招面试心得与自学CS经验及找工作分享 [github](https://github.com/conanhujinming/tips_for_interview/blob/master/README-zh_CN.md)   

### ACM模板：

https://github.com/Bryce1010/bryce1010-ACM-Template

KuangBin的ACM模板 [[github]](https://github.com/kuangbin/ACM-ICPC)  

f-zyj/ACM 模板 [[github]](https://github.com/f-zyj/ACM)

f-zyj/ACM 在线模板：https://blog.csdn.net/f_zyj/article/details/51594851

https://github.com/soulmachine/acm-cheat-sheet

### 算法基础课：

算法基础课： https://www.acwing.com/activity/content/11/

yxc代码模板合集： https://blog.csdn.net/weixin_45629285/category_10218699.html

代码基础课模板： https://github.com/kszlzj/algorithm

### 进阶指南

[算法竞赛知识合集 目录](https://blog.csdn.net/weixin_45697774/article/details/105603064)

算法竞赛进阶指南 杂谈 https://blog.nowcoder.net/zhxu98/2671

算法竞赛进阶指南题解笔记 秦淮岸： https://www.acwing.com/blog/content/89/

大雪菜：https://blog.csdn.net/GarfieldEr007


**随书代码**

[算法竞赛进阶指南 题目练习](https://ac.nowcoder.com/acm/archive/oi-advance/problem)

https://github.com/livrth/algorithm-competition-advanced-guide

https://github.com/820fans/ACWinger-ClockIn

https://github.com/lydrainbowcat/tedukuri



## 0x00 基本算法

### 基础课模板

#### 排序

**快速排序算法模板** 

模板题 AcWing 785. 快速排序

```cpp
void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}
```

**归并排序算法模板**

模板题 AcWing 787. 归并排序
```cpp
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int mid = l + r >> 1;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);
    
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];
    
    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];
    
    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```

#### 二分法

**整数二分算法模板** 

模板题 AcWing 789. 数的范围

```cpp
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

**浮点数二分算法模板**

模板题 AcWing 790. 数的三次方根

```cpp
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

#### 高精度运算

**高精度加法**

模板题 AcWing 791. 高精度加法

```cpp
// C = A + B, A >= 0, B >= 0
vector<int> add(vector<int> &A, vector<int> &B)
{
    if (A.size() < B.size()) return add(B, A);

    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i ++ )
    {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    
    if (t) C.push_back(t);
    return C;
}
```

**高精度减法** 

模板题 AcWing 792. 高精度减法

```cpp
// C = A - B, 满足A >= B, A >= 0, B >= 0
vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ )
    {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

```

**高精度乘低精度**

模板题 AcWing 793. 高精度乘法
```cpp
// C = A * b, A >= 0, b >= 0
vector<int> mul(vector<int> &A, int b)
{
    vector<int> C;

    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ )
    {
        if (i < A.size()) t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }
    
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    
    return C;
}
```

**高精度除以低精度**

模板题 AcWing 794. 高精度除法
```cpp
// A / b = C ... r, A >= 0, b > 0
vector<int> div(vector<int> &A, int b, int &r)
{
    vector<int> C;
    r = 0;
    for (int i = A.size() - 1; i >= 0; i -- )
    {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
```

#### 前缀和与差分 

一维前缀和 —— 模板题 AcWing 795. 前缀和

S[i] = a[1] + a[2] + ... a[i]

a[l] + ... + a[r] = S[r] - S[l - 1]

二维前缀和 —— 模板题 AcWing 796. 子矩阵的和

S[i, j] = 第i行j列格子左上部分所有元素的和
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]

一维差分 —— 模板题 AcWing 797. 差分

给区间[l, r]中的每个数加上c：B[l] += c, B[r + 1] -= c

二维差分 —— 模板题 AcWing 798. 差分矩阵

给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：

S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c

#### 位运算 

模板题 AcWing 801. 二进制中1的个数

求n的第k位数字: n >> k & 1

返回n的最后一位1：lowbit(n) = n & -n

#### 双指针算法 

模板题 AcWIng 799. 最长连续不重复子序列

AcWing 800. 数组元素的目标和

	for (int i = 0, j = 0; i < n; i ++ )
	{
		while (j < i && check(i, j)) j ++ ;
	
		// 具体问题的逻辑
	}

常见问题分类：
    (1) 对于一个序列，用两个指针维护一段区间
    (2) 对于两个序列，维护某种次序，比如归并排序中合并两个有序序列的操作

#### 离散化 

模板题 AcWing 802. 区间和

```cpp
vector<int> alls; // 存储所有待离散化的值
sort(alls.begin(), alls.end()); // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

// 二分求出x对应的离散化的值
int find(int x) // 找到第一个大于等于x的位置
{
    int l = 0, r = alls.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1; // 映射到1, 2, ...n
}
```

#### 区间合并 

模板题 AcWing 803. 区间合并

```cpp
// 将所有存在交集的区间合并
typedef pair<int,int> PII;
void merge(vector<PII> &segs)
{
    vector<PII> res;

    sort(segs.begin(), segs.end());
    
    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);
    
    if (st != -2e9) res.push_back({st, ed});
    
    segs = res;
}
```


### 0x02递推与递归

**递归回溯**

[[递归与回溯的理解]](https://cloud.tencent.com/developer/article/1434886)  

[[手把手教怎么写递归和回溯]](https://leetcode-cn.com/circle/article/GV6eQ2/)  

递归回溯一般模板：
```cpp
'''
backtracking：使用dfs的模板，基本跟dfs的模板一模一样
'''
class Backtracking(object):

    def backtracking(self, input):

        self.res = []

        def dfs(input, temp, [index]):
            # 边界
            if 非法数据：
                return

            # 终止条件
            if len(input) == len(temp)：
                self.res.append(temp[:])
                return

            # for循环
            for i range(len(input)):
                ##1. 修改path
                temp.append(input[i])
                ##2. backtracking
                dfs(input, temp, [index])
                ##3. 退回原来状态，恢复path
                temp.pop()
        # 执行
        dfs(input, [], 0)
        return self.res
```


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


###  0x03前缀和与差分

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

