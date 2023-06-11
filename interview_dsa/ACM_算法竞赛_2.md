
### 第6章 数论和线性代数


#### 6.1 模运算

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll mul(ll a,ll b,ll m){     //乘法取模：a*b mod m
    a = a%m;                //先取模，非常重要，能在一定程度上防止第10行a+a溢出
    b = b%m;
    ll res = 0;
    while(b>0){             //递归思想
        if(b&1) res=(res+a)% m;  //用b&1判断b的奇偶
        a=(a+a)% m;              //需要保证a+a=2a不能溢出，否则答案错误
        b>>=1;
    }
    return res;
}
int main(){
    ll a = 0x7877665544332211;
    ll b = 0x7988776655443322;
    ll m =  0x998776655443322;    //把m改成比a大的0x7977665544332211，mul()也会出错
    cout << (a%m)*(b%m)%m <<endl; //输出 145407782617487436，错误
    cout << mul(a,b,m);           //输出 411509877096934416，正确
}
```

#### 6.2 快速幂

```cpp
int fastPow(int a, int n){     //计算an
    int ans = 1;               //用ans返回结果
    while(n) {                 //把n看成二进制，逐个处理它的最后一位
        if(n & 1)   ans *= a;  //如果n的最后一位是1，表示这个地方需要乘
        a *= a;                //递推：a2 --> a4 --> a8--> a16...
        n >>= 1;               //n右移一位，把刚处理过的n的最后一位去掉
    }
    return ans;
}

typedef long long ll;                      //变量改用较大的long long
ll fastPow(ll a, ll n, ll mod){
    ll ans = 1;
    a %= mod;                              //有一定作用，能在一定程度上防止下面的a*a越界
    while(n) {
        if(n & 1)   ans = (ans*a) % mod;   //取模 
        a = (a*a) % mod;                   //取模
        n >>= 1;
    }
    return ans;
}
```

#### 6.3 矩阵快速幂与加速递推

**6.3.2 矩阵快速幂**

```cpp
struct matrix{ int m[N][N]; };     //定义矩阵，常数N是矩阵的行数和列数
matrix operator * (const matrix& a, const matrix& b){   //重载*为矩阵乘法。注意const
    matrix c;   
    memset(c.m, 0, sizeof(c.m));  //清零
    for(int i=0; i<N; i++)
        for(int j=0; j<N; j++)
            for(int k = 0; k<N; k++)
              //c.m[i][j] += a.m[i][k] * b.m[k][j];                   //不取模
                c.m[i][j] = (c.m[i][j] + a.m[i][k] * b.m[k][j]) % mod;//取模
    return c;
}
matrix pow_matrix(matrix a, int n){  //矩阵快速幂，代码和普通快速幂几乎一样
    matrix ans;   
    memset(ans.m,0,sizeof(ans.m));
    for(int i=0;i<N;i++)  ans.m[i][i] = 1; //初始化为单位矩阵，类似普通快速幂的ans=1
    while(n) {
        if(n&1) ans = ans * a;       //不能简写为ans *= a，这里的*重载了
        a = a * a;
        n>>=1;
    }
    return ans;
}
```


**6.3.4 矩阵乘法与路径问题**

例6-3.  Cow Relays  poj 3613

```cpp
#include<cstdio>
#include<algorithm>
#include<cstring>
const int INF=0x3f;
const int N=120;
int Hash[1005],cnt=0;                //用于离散化
struct matrix{int m[N][N]; };        //定义矩阵
matrix operator *(const matrix& a, const matrix& b){   //定义广义矩阵乘法
    matrix c;
    memset(c.m,INF,sizeof c.m);
    for(int i=1;i<=cnt;i++)          //i、j、k可以颠倒，因为对c来说都一样
        for(int j=1;j<=cnt;j++)
            for(int k=1;k<=cnt;k++)
               c.m[i][j] = std::min(c.m[i][j], a.m[i][k] + b.m[k][j]);
    return c;
}
matrix pow_matrix(matrix a, int n){  //矩阵快速幂，几乎就是标准的快速幂写法
    matrix ans = a;                  //矩阵初值ans = M^1
    n--;                             //上一行ans= M^1多了一次
    while(n) {                       //矩阵乘法：M^n
        if(n&1) ans = ans * a;
        a = a * a;
        n>>=1;
    }
    return ans;
}
int main(){
    int n,t,s,e;    scanf("%d%d%d%d",&n,&t,&s,&e);
    matrix a;                                //用矩阵存图
    memset(a.m,INF,sizeof a.m);
    while(t--){
        int u,v,w;     scanf("%d%d%d",&w,&u,&v);
        if(!Hash[u])   Hash[u] = ++cnt;      //对点离散化.  cnt就是新的点编号
        if(!Hash[v])   Hash[v] = ++cnt;
        a.m[Hash[u]][Hash[v]] = a.m[Hash[v]][Hash[u]] = w;
    }
    matrix ans = pow_matrix(a,n);
    printf("%d",ans.m[Hash[s]][Hash[e]]);
    return 0;
}
```

#### 6.4 高斯消元

**6.4.2 高斯-约当消元法**

例6-4. 高斯消元法  洛谷P3389

```cpp
//改写自：https://www.luogu.com.cn/blog/tbr-blog/solution-p3389
#include<bits/stdc++.h>
using namespace std;
double a[105][105];
double eps = 1e-7;
int main(){
	int n; scanf("%d",&n);
	for(int i=1;i<=n;++i)
		for(int j=1;j<=n+1;++j) 	scanf("%lf",&a[i][j]);
	for(int i=1;i<=n;++i){            //枚举列
		int max=i;
		for(int j=i+1;j<=n;++j)      //选出该列最大系数，真实目的是选一个非0系数
			if(fabs(a[j][i])>fabs(a[max][i]))   max=j;
		for(int j=1;j<=n+1;++j) swap(a[i][j],a[max][j]); //移到前面
		if(fabs(a[i][i]) < eps){     //对角线上的主元系数等于0,说明没有唯一解
			puts("No Solution");
			return 0;
		}
		for(int j=n+1;j>=1;j--)	  a[i][j]= a[i][j]/a[i][i]; //把这一行的主元系数变为1
		for(int j=1;j<=n;++j){       //消去主元所在列的其他行的主元
			if(j!=i)	{
				double temp=a[j][i]/a[i][i];
				for(int k=1;k<=n+1;++k)  a[j][k] -= a[i][k]*temp;
			}
		}
	}
	for(int i=1;i<=n;++i)	printf("%.2f\n",a[i][n+1]); //最后得到简化阶梯矩阵
	return 0;
}
```

#### 6.5 异或空间的线性基

**6.5.3 线性基的应用**

```cpp
//不用高斯消元求线性基
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int M=63;
ll p[M];  //线性基
bool zero;
void Insert(ll x){
    for(int i=M;i>=0;i--)
        if(x>>i == 1)                      //x的最高位
           if(p[i]==0){ p[i]=x; return; }  //P[i]还没有，直接让P[i] = x
        else x^=p[i];                      // P[i]已经有了，逐个异或
    zero = true;                           //A有异或和为0的组合
}
ll qmax(){
    ll ans = 0;
    for( int i=M;i>=0;i--)   ans = max(ans,ans^p[i]);
    return ans;
}
int main(){
    ll x; int n; scanf("%d",&n);
    for(int i=1;i<=n;i++)  scanf("%lld",&x), Insert(x);
    printf("%lld\n",qmax());
    return 0;
}
```

例6-8. XOR  Hdu 3949

```cpp
//用高斯消元求线性基 
#include<bits/stdc++.h>
#define N 10100
using namespace std;
typedef long long ll;
int n;
bool zero;
//消元后是否产生全0的行
ll a[N];
void Gauss() {
	//高斯消元求线性基
	int i,k=1;
	//k标记当前第几行
	ll j = (ll)1<<62;
	//注意不是63
	for (;j;j>>=1) {
		for (i=k;i<=n;i++)
		     if(a[i]&j)  break;
			//找到第j位是1的a[]
			if(i > n) 	continue;
			//没有第j位是1的a[]
			swap(a[i],a[k]);
		//把这一行换到上面
		for (i=1;i<=n;i++)            //生成简化阶梯矩阵
			if(i != k && a[i]&j)     a[i]^=a[k];
		k++;
	}
	k--;
	if(k!=n)  zero = true; else      zero = false;
	n = k;
	//线性基中元素的个数
}
ll Query(ll k) {
	//第k小异或和
	ll ans=0;
	if(zero) k--;
	if(!k)   return 0;
	for (int i=n;i;i--) {
		if(k&1) ans^=a[i];
		k >>= 1;
	}
	if(k) return -1;
	return ans;
}
int main() {
	int cnt=0;
	int T;
	cin>>T;
	while(T--) {
		printf("Case #%d:\n",++cnt);
		cin>>n;
		for (int i=1;i<=n;i++) 	scanf("%lld",&a[i]);
		Gauss();
		int q;
		cin>>q;
		while(q--) {
			ll k;
			scanf("%lld",&k);
			printf("%lld\n", Query(k) );
		}
	}
}
```

#### 6.6 0/1分数规划

**6.6.1 二分法与0/1分数规划**

例6-9.  Dropping tests   poj 2976

代码：

```cpp
#include <stdio.h>
#include <algorithm>
using namespace std;
struct Pair{ int a, b;  double y;} p[1005];
bool cmp(Pair a, Pair b){ return a.y > b.y; }
int n, k;
bool check(double x) {
    for(int i=0; i<n; i++)   p[i].y = p[i].a * 1.0 - x * p[i].b;  //计算y=a-xb
    sort(p, p + n, cmp);                       //按y值排序，非常重要
    double f = 0;
    for (int i=0; i<k; i++)  f += p[i].y;      //对前k个直线的y值求和
    return f < 0;                              // f < 0：竖线在M的右侧
}
int main() {
    while (scanf("%d%d", &n, &k) == 2 && n + k) {
        k = n - k;               //改为选出k对
        for (int i = 0; i < n; i++)  scanf("%d", &p[i].a);
        for (int i = 0; i < n; i++)  scanf("%d", &p[i].b);
        double L = 0, R = 0;
        for (int i = 0; i < n; i++)  R += p[i].a;  //R的初值
        for (int i = 0; i < 50; i++) {       //二分50次，本题足够了
            double mid = L+(R-L)/2;
            if (check(mid))   R = mid;        //竖线在M的右侧，需要左移
            else              L = mid;        //竖线在M的左侧，需要右移
        }
        printf("%d\n", (int)(100 * (L + 0.005)));  //四舍五入
    }
    return 0;
}
```


**6.6.2 应用场景**

例6-10. Talent Show G洛谷 P4377

```cpp
//改写自: www.luogu.com.cn/blog/yjlakioi/post-18-open-g-t3-post
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f3f,N=255,WW=1005;
int n,W;
struct{int w, t; double y;}cow[N];
double  dp[WW]; //dp[i],背包容量为i时最大的价值(y值之和)
bool check(double x){         // 0/1背包
	int i,j;
	for(i=1;i<=n;i++) cow[i].y=(double)cow[i].t-x*cow[i].w;
	for(i=1;i<=W;i++) dp[i]=-INF; //初始化为负无穷小
	dp[0] = 0;               //背包容量为0时价值为0
	for(i=1;i<=n;i++)
	    for(j=W;j>=0;j--){   // 滚动数组
	        if(j+cow[i].w>=W)  dp[W]=max(dp[W],dp[j]+cow[i].y); //大于W时按W算
		   else               dp[j+cow[i].w]=max(dp[j+cow[i].w],dp[j]+cow[i].y);
		}
	return dp[W]<0;          // dp[W] < 0，x大了；= dp[W] ≥ 0，x小了
}
int main(){
    cin>>n>>W;
    for(int i=1;i<=n;i++)   cin>>cow[i].w>>cow[i].t;
    double L=0,R=0;
    for (int i = 1; i <= n; i++)  R += cow[i].t;  //R的初值
    for(int i=0;i<50;i++){
        double mid = L+(R-L)/2;
        if(check(mid)) 	R = mid;  //缩小
        else 			L = mid;  //放大
    }
    cout<<(int)(L*1000)<<endl;
    return 0;
}
```

#### 6.8 线性丢番图方程

**6.8.2 扩展欧几里得算法与二元线性丢番图方程的解**

例6-17.  青蛙的约会 洛谷 P1516

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
ll extend_gcd(ll a,ll b,ll &x,ll &y){
    if(b == 0){ x=1; y=0; return a;}
    ll d = extend_gcd(b,a%b,y,x);
    y -= a/b * x;
    return d;
}
int main(){
    ll n,m,x,y,L;     cin>>x>>y>>m>>n>>L;
    ll a=n-m,c=x-y;
    if(a<0){ a=-a; c=-c;}                //处理负数
    ll d = extend_gcd(a,L,x,y);
    if(c%d != 0)  cout<<"Impossible";    //判断方程有无解。
    else          cout<<((x*(c/d))%(L/d)+(L/d))%(L/d);    //x的最小整数解
}
```

#### 6.9 同余

**6.9.3 逆**

例6-19. 同余方程（求逆） 洛谷 P1082

```cpp
long long mod_inverse(long long a, long long m){    //求逆
long long x,y;
    extend_gcd(a,m,x,y);
    return  (x%m + m) % m;                          //保证返回最小正整数
}
int main(){
    long long a,m;  cin >> a >>m; 
    cout << mod_inverse(a,m);
    return 0;
}
```
	
例6-23.  Detachment  hdu 5976 

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long 
const int N = 1e5;                  //分解的数不会超过50000个，请自己分析
const int mod = 1e9 + 7; 
ll sum[N], mul[N];                  //前缀和、连续积
ll fast_pow(ll x,ll y,int m){       //快速幂取模：x^y mod m
    ll res = 1;
    while(y) {
        if(y&1) res*=x, res%=m;
        x = (x*x) % m;
        y>>=1;
    }
    return res;
}
long long mod_inverse(long long a,long long mod){   //费马小定理求逆
   return fast_pow(a,mod - 2,mod);           
}
void init(){       //预计算前缀和、连续积
    sum[1] = 0;  mul[1] = 1;
    for(int i=2; i<=N; i++){
        sum[i] = sum[i-1] + i;        //计算前缀和
        mul[i] = (i*mul[i-1]) % mod;  //计算连续积
    }
} 
int main(){
    init();
    int T; scanf("%d",&T);
    while(T--){
        int x; scanf("%d",&x);
        if( x == 1) {puts("1"); continue;}            //特殊情况
        int k = upper_bound(sum+1,sum+1+N,x)-sum-1;   //分解成k个数
        int m = x - sum[k];                           //余数
        ll ans;
        if(k==m) ans = mul[k]   * mod_inverse(2,mod) %mod * (k+2) % mod; //第2种情况           
        else     ans = mul[k+1] * mod_inverse(k-m+1,mod) %mod % mod;     //第1种情况
        printf("%lld\n",ans);
    }
    return 0;
}
```


**6.9.4 同余方程组**

例6-24.  扩展中国剩余定理 洛谷P4777

```cpp
//改写自 https://www.luogu.com.cn/problem/solution/P4777
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 100010;
int n;
ll ai[N], mi[N];
ll mul(ll a,ll b,ll m){                 //乘法取模：a*b % m
    ll res=0;
    while(b>0){
        if(b&1) res=(res+a) % m;
        a=(a+a) % m;
        b>>=1;
    }
    return res;
}
ll extend_gcd(ll a,ll b,ll &x,ll &y){   //扩展欧几里得
    if(b == 0){ x=1; y=0; return a;}
    ll d = extend_gcd(b,a%b,y,x);
    y -= a/b * x;
    return d;
}
ll excrt(){                              //求解同余方程组，返回最小正整数解
    ll x,y;
    ll m1 = mi[1], a1 = ai[1];           //第1个等式
    ll ans = 0;
    for(int i=2;i<=n;i++){               //合并每2个等式       
        ll a2 = ai[i], m2 = mi[i];       // 第2个等式 
        //合并为：aX + bY = c      
        ll a = m1, b = m2, c = (a2 - a1%m2 + m2) % m2;
        //下面求解 aX + bY = c
        ll d = extend_gcd(a,b,x,y);  //用扩展欧几里得求x0
        if(c%d != 0) return -1;      //无解
        x = mul(x,c/d,b/d);          //aX + bY = c 的特解t，最小值           
        ans = a1 + x* m1;            //代回原第1个等式，求得特解x'
        m1 = m2/d*m1;                //先除再乘，避免越界。合并后的新m1
        ans = (ans%m1 + m1) % m1;    //最小正整数解
        a1 = ans;                    //合并后的新a1
    }
    return ans;
}
int main(){
    scanf("%d", &n);
    for(int i=1;i<=n;++i)  scanf("%lld%lld",&mi[i],&ai[i]);
    printf("%lld",excrt());
    return 0;
}
```

#### 6.10 素数（质数）

**6.10.2 大素数的判定**

例6-25.  How many prime numbers   hdu 2138

```cpp
#include <bits/stdc++.h>
typedef long long LL;
LL fast_pow(LL x,LL y,int m){       //快速幂取模：x^y mod m
    LL res = 1;
    x %= m;
    while(y) {
        if(y&1) res = (res*x) % m;
        x = (x*x) % m;
        y>>=1;
    }
    return res;
}

bool witness(LL a, LL n){           // Miller-Rabin素性测试。返回true表示n是合数
        LL u = n-1;                 //注意，u的意义是：n-1的二进制去掉末尾0
        int t = 0;                  // n-1的二进制，是奇数u的二进制，后面加t个零
        while(u&1 == 0)  u = u>>1, t++;    // 整数n-1末尾0的个数，就是t
        LL x1, x2;
        x1 = fast_pow(a,u,n);              // 先计算  a^u mod n        
        for(int i=1; i<=t; i++) {          // 做t次平方取模
            x2 = fast_pow(x1,2,n);         // x1^2 mod n
            if(x2 == 1 && x1 != 1 && x1 != n-1) return true;  //用推论判断
            x1 = x2;
        }
        if(x1 != 1) return true;  //用费马测试判断是否为合数：an-1≡1(mod n)不成立，是合数            
        return false;
}
int miller_rabin(LL n,int s){             //对n做s次测试
    if(n<2)  return 0;  
    if(n==2) return 1;                    //2是素数
    if(n % 2 == 0 ) return 0;             //偶数
    for(int i = 0;i < s && i < n;i++){    //做s次测试
		LL a = rand() % (n - 1) + 1;     //基值a是随机数
 		if(witness(a,n))  return 0;      //n是合数，返回0           	              
	}
 	return 1;                             //n是素数，返回1
}
int main(){
	int m;                   
	while(scanf("%d",&m) != EOF){
	     int cnt = 0;
 	     for(int i = 0; i < m; i++){
            LL n; scanf("%lld",&n);   
            int s = 50;                  //做s次测试
            cnt += miller_rabin(n,s);
		} 
		printf("%d\n",cnt);
	} 
	return 0;
}
```
	
**6.10.3 素数筛**

```cpp
const int N = 1e7;                       //定义空间大小，1e7约10M
int prime[N+1];                          //存放素数，它记录visit[i] = false的项
bool visit[N+1];                         //true表示被筛掉，不是素数
int E_sieve(int n)  {                    //埃氏筛法，计算[2, n]内的素数
    int k=0;                             //统计素数个数
    for(int i=0; i<=n; i++)  visit[i]= false;  //初始化
    for(int i=2; i<=n; i++) {             //从第一个素数2开始。可优化（1）
        if(!visit[i]) {
            prime[k++] = i;               //i是素数，存储到prime[]中
            for(int j=2*i; j<=n; j+=i)    //i的倍数，都不是素数。可优化（2）
                visit[j] = true;          //标记为非素数，筛掉
        }
    }
    return k;                              //返回素数个数
}
```


```cpp
int E_sieve(int n) {
    for(int i = 0; i <= n; i++)  visit[i]= false;
    for(int i = 2; i*i <= n; i++)          //筛掉非素数。改为i<=sqrt(n)，计算更快
        if(!visit[i])
            for(int j=i*i; j<=n; j+=i)  visit[j] = true;         //标记为非素数
//下面记录素数
    int  k=0;                              //统计素数个数
    for(int i = 2; i <= n; i++)
        if(!visit[i])   prime[k++] = i;    //存储素数
    return k;
}
```


```cpp
int prime[N];                           //保存质数，为节约空间，可以适当减小
bool vis[N];                            //记录是否被筛
int euler_sieve(int n){                 //欧拉筛。返回质数的个数。
	int cnt = 0;                       //记录质数个数
	memset(vis,0,sizeof(vis));
	memset(prime,0,sizeof(prime));
	for(int i=2;i<=n;i++){              //检查每个数，筛去其中的合数
	    if(!vis[i]) prime[cnt++]=i;     //如果没有筛过，是质数，记录。第一个质数是2
	    for(int j=0; j<cnt; j++){       //用已经得到的质数去筛后面的数
	        if(i*prime[j] >n)  break;   //只筛小于等于n的数
	        vis[i*prime[j]]=1;          //关键1。用x的最小质因数筛去x
	        if(i%prime[j]==0)  break;   //关键2。如果不是这个数的最小质因子，结束
		}
	}
	return cnt;                         //返回小于等于n的质数的个数
}
```

**6.10.4 质因数分解**


```cpp
int prime[N];      //记录质数
int vis[N];        //记录最小质因子
int euler_sieve(int n){     
	int cnt=0;
	memset(vis,0,sizeof(vis));
	memset(prime,0,sizeof(prime));
	for(int i=2;i<=n;i++){               
	    if(!vis[i]){ vis[i]=i; prime[cnt++]=i;}    //vis[]记录最小质因子
	    for(int j=0; j<cnt; j++){       
	        if(i*prime[j] >n)  break;       
	        vis[i*prime[j]] = prime[j];            //vis[]记录最小质因子
	        if(i%prime[j]==0)  break;                  
		}
	}
	return cnt;
}
```

```cpp
//代码改写自《算法竞赛进阶指南》河南电子音像出版社，李煜东，137页
int p[20];  //p[]记录因子，p[1]是最小因子。一个int数的质因子最多有十几个
int c[40];  //c[i]记录第i个因子的个数。一个因子的个数最多有三十几个
int factor(int n){
    int m = 0;
    for(int i = 2; i <= sqrt(n); i++)
        if(n%i == 0){
           p[++m] = i, c[m] = 0;
           while(n%i == 0)  n/=i, c[m]++;     //把n中重复的因子去掉              
        }
    if(n>1)  p[++m] = n, c[m] = 1;            //没有被除尽，是素数       
    return m;                                 //共m个
}
```


```cpp
//poj 1811部分代码：输入一个整数n，2<=N<2^54，判断它是否为素数，如果不是，输出最小质因子。
typedef long long ll;
ll Gcd (ll a,ll b){return b? Gcd(b, a%b):a;}
ll mult_mod (ll a,ll b,ll n){       //返回(a*b) mod n
	a %= n,  b %= n;
	ll ret=0;
	while (b){
		if (b&1){
			ret += a;
			if (ret >= n) ret -= n;
		}
		a<<=1;
		if (a>=n) a -= n;
		b>>=1;
	}
	return ret;
}
ll pollard_rho (ll n){                //返回一个因子，不一定是质因子
    ll i=1, k=2;
	ll c = rand()%(n-1)+1;
    ll x = rand()%n;
    ll y = x;
    while (true){
        i++;
        x = (mult_mod(x,x,n)+c) % n;   //(x*x) mod n
        ll d = Gcd(y>x?y-x:x-y, n);    //重要：保证gcd的参数大于等于0
        if (d!=1 && d!=n) return d;
        if (y==x) return n;            //已经出现过，直接返回
        if (i==k) { y=x; k=k<<1;}
    }
}
void findfac (ll n){                  //找所有的素因子
    if (miller_rabin(n)) {            //用miller_rabin判断是否为素数
        factor[tol++] = n;            //存素因子
        return;
    }
    ll p = n;
    while (p>=n) p=pollard_rho(p);    //找到一个因子
    findfac(p);                       //继续寻找更小的因子
    findfac(n/p);
}
```

#### 6.13 欧拉函数

**6.13.2 求欧拉函数的通解公式**

```cpp
int euler(int n){
    int ans = n;
    for(int p = 2; p*p <= n; ++ p){ //试除法：检查从2到sqrt(n)的每个数
        if(n%p == 0){               //能整除，p是一个因子，而且是质因子，请思考
            ans = ans/p*(p-1);      //求欧拉函数的通式
            while(n%p == 0)         //去掉这个因子的幂，并使得下一个p是质因子
                n /= p;             //减小了n
        }
    }
    if(n != 1)  ans = ans/n*(n-1);  //情况(1)：n是一个质数，没有执行上面的分解        
    return ans;
}
```


**6.13.3 线性筛（欧拉筛）求1～n内所有的欧拉函数**

例6-29. 仪仗队 洛谷P2158 

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 50000;
int vis[N];         //记录是否被筛；或者用于记录最小质因子
int prime[N];       //记录质数
int phi[N];         //记录欧拉函数
int sum[N];         //计算欧拉函数的和
void get_phi(){     //模板：求1～N范围内的欧拉函数
    phi[1]=1;
    int cnt=0;
    for(int i=2;i<N;i++) {
        if(!vis[i]) {
            vis[i]=i; //vis[i]=1; //二选一：前者记录最小质因子，后者记录是否被筛
            prime[cnt++]=i;       //记录质数
            phi[i]=i-1;           //情况(1)：i是质数，它欧拉函数值=i-1
        }
        for(int j=0;j<cnt;j++) {
            if(i*prime[j] > N)  break;
            vis[i*prime[j]] = prime[j]; //vis[i*prime[j]]=1; 
                                  //二选一：前者记录最小质因子，后者记录是否被筛
            if(i%prime[j]==0){    //prime[j]是最小质因子
                phi[i*prime[j]]=phi[i]*prime[j]; //情况(2)：i是prime[j]的k次方                                  
                break;
            }
            phi[i*prime[j]]=phi[i]*phi[prime[j]];//情况(3)：i和prime[j]互素，递推出i*prime[j]
        }
    }
}
int main(){
    get_phi();                    //计算所有的欧拉函数
    sum[1]=1;
    for(int i=2;i<=N;i++)  sum[i]=sum[i-1]+phi[i];   //打表计算欧拉函数的和        
    int n;   scanf("%d",&n);
    if(n==1) printf("0\n");
    else     printf("%d\n",2*sum[n-1]+1);
    return 0;
}
```


#### 6.14 整除分块（数论分块）

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
    long long n,L,R,ans=0;
    cin >> n;
    for(L=1;L<=n;L=R+1){
        R = n/(n/L);                               //计算R，让分块右移
        ans += (R-L+1)*(n/L);                      //分块求和
        cout << L <<"-"<< R <<": "<< n/R << endl;  //打印分块的情况
    }
    cout << ans;               //打印和
}
```	

#### 6.16 莫比乌斯函数和莫比乌斯反演

```cpp
bool vis[N];
int prime[N];
int Mob[N];
void Mobius_sieve(){
     int cnt = 0;
     vis[1] = 1;
     Mob[1] = 1;
     for(int i = 2; i <= N; i++){
         if(!vis[i])  prime[cnt++] = i, Mob[i] = - 1;
         for(int j = 0; j < cnt && 1LL * prime[j] * i <= N; j++){
             vis[prime[j] * i] = 1;
             Mob[i * prime[j]] = (i % prime[j] ? -Mob[i]: 0);
             if(i % prime[j] == 0)  break;
        }
    }
}
```

#### 6.17 杜教筛

**6.17.4 杜教筛模板代码**

下面给出洛谷P4213的代码


```cpp
//代码改写自：https://blog.csdn.net/KIKO_caoyue/article/details/100061406
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 5e6+7;         //超过n^(2/3)，够大了
int prime[N];                //记录质数
bool vis[N];                 //记录是否被筛；
int mu[N];                   //莫比乌斯函数值
ll phi[N];                   //欧拉函数值
unordered_map<int,int> summu;   //莫比乌斯函数前缀和
unordered_map<int,ll> sumphi;   //欧拉函数前缀和
void init(){                    //线性筛预计算一部分答案
     int cnt = 0;
     vis[0] = vis[1] = 1;
     mu[1] = phi[1] = 1;
     for(int i=2;i<N;i++){
         if(!vis[i]){ 
            prime[cnt++] = i;
            mu[i] = -1;
            phi[i] = i-1;
         }
         for(int j=0;j<cnt && i*prime[j]<N;j++){
             vis[i*prime[j]] = 1; 
             if(i%prime[j]){ 
                mu[i*prime[j]] = -mu[i];
                phi[i*prime[j]] = phi[i]*phi[prime[j]];
             } 
             else{
                mu[i*prime[j]] = 0;
                phi[i*prime[j]] = phi[i]*prime[j]; 
                break;
			}
		}
	}
	for(int i=1;i<N;i++){       //最后，mu[]和phi[]改为记录1~N的前缀和。
         mu[i] += mu[i-1];
         phi[i] += phi[i-1];
    }
}
int gsum(int x){                    // g(i)的前缀和
	return x;
}
ll getsmu(int x){
    if(x < N) return mu[x];      //预计算
    if(summu[x]) return summu[x];   //记忆化  
    ll ans = 1;                     //杜教筛公式中的 1
    for(ll l=2,r;l<=x;l=r+1){       //用整除分块计算杜教筛公式
        r = x/(x/l); 
        ans -= (gsum(r)-gsum(l-1))*getsmu(x/l);
	}
    return summu[x] = ans/gsum(1);
}
ll getsphi(int x){
    if(x < N) return phi[x];
    if(sumphi[x]) return sumphi[x];  //记忆化，每个sumphi[x]只用算一次
    ll ans = x*((ll)x+1)/2;          //杜教筛公式中的 n(n+1)/2
    for(ll l=2,r;l<=x;l=r+1){        //用整除分块计算杜教筛公式，这里算 sqrt(x)次
        r = x/(x/l);
        ans -= (gsum(r)-gsum(l-1))*getsphi(x/l);
    } 
    return sumphi[x] = ans/gsum(1);
}
int main(){  
    init();                          //用线性筛预计算一部分
    int t;	scanf("%d",&t);
    while(t--){ 
       int n;  scanf("%d",&n);
       printf("%lld %lld\n",getsphi(n),getsmu(n));
    }
    return 0;
}
```

### 第7章 组合数学

#### 7.3 二项式定理和杨辉三角

例7-2. 计算杨辉三角

```cpp
#include<bits/stdc++.h>
using namespace std;
int a[21][21];
int main(){
    int n; cin >> n;
    for(int i=1;i<=n;i++) 	a[i][1] = a[i][i] = 1;   //赋初值
    for(int i=1;i<=n;i++)
    for(int j=2;j<i;j++)
    a[i][j] = a[i-1][j]+a[i-1][j-1];        //递推求二项式系数
    for(int i=1;i<=n;i++){
        for(int j=1;j<=i;j++)
        cout << a[i][j]<<" ";
                cout << endl;
        }
}
```

下面的例题，用两种方法算二项式系数：递推公式、逆。

例7-3. 计算系数 洛谷P1313

（1）用递推公式计算二项式系数（DFS写法）

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1005;
#define mod 10007
int c[N][N];                   //存二项式系数
int fastPow(int a, int n){     //标准快速幂
    int ans = 1;
    a %= mod;                  //防止下面的ans*a越界
    while(n) {
        if(n & 1)   ans = (ans*a) % mod;
        a = (a*a) % mod;
        n >>= 1;
    }
    return ans;
}
int dfs(int n,int m){          //用递推公式求二项式系数
    if(!m)        return c[n][m]=true;
    if(m==1)      return c[n][m]=n;
    if(c[n][m])   return c[n][m];     //记忆化
    if(n-m<m)     m=n-m;
    return c[n][m]=(dfs(n-1,m)+dfs(n-1,m-1))%mod;     
}
int main(){
    int a,b,k,n,m;  cin >>a>>b>>k>>n>>m;
    c[1][0] = c[1][1] = 1;
    int ans = 1;
    ans *= (fastPow(a,n)*fastPow(b,m))%mod;
    ans *= dfs(k,n)%mod;
    ans %= mod;
    cout << ans;
    return 0;
}
```

（2）用逆直接计算二项式公式

```cpp
#include<bits/stdc++.h>
using namespace std;
#define mod 10007
int fac[10001];              //预计算阶乘
int inv[10001];              //预计算逆
int fastPow(int a, int n){}  //和上一个代码完全一样
int C(int n,int m){          //算组合数，用到除法取模的逆
    return (fac[n]*inv[m]%mod*inv[n-m]%mod)%mod;
}
int main(){
    int a,b,n,m,k,ans;  cin >>a>>b>>k>>n>>m;
    fac[0] = 1;
    for(int i=1;i<=n+m;i++){
        fac[i] = (fac[i-1]*i)%mod;        //预计算阶乘，要取模
        inv[i] = fastPow(fac[i],mod-2);   //用费马小定理预计算逆
    }
    ans = (fastPow(a,n)%mod*fastPow(b,m)%mod*C(k,n)%mod)%mod;
    cout << ans;
    return 0;
}
```

#### 7.4 卢卡斯定理

例7-5.  卢卡斯定理  洛谷P3807

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 100010;
typedef long long ll;
ll fac[N];                        //预计算阶乘，取模
ll fastPow(ll a, ll n, ll m){     //标准快速幂
    ll ans = 1;
    a %= m;                       //防止下面的ans*a越界
    while(n) {
        if(n & 1)   ans = (ans*a) % m;
        a = (a*a) % m;
        n >>= 1;
    }
    return ans;
}
ll inverse(ll a,int m){ return fastPow(fac[a],m-2,m); } //用费马小定理计算逆
ll C(ll n,ll r,int m){          //用逆计算C(n mod m, r mod m) mod m
    if(r>n)return 0;
    return ((fac[n] * inverse(r,m))%m * inverse(n-r,m)%m);
}
ll Lucas(ll n,ll r,int m){      //用递归计算C(n, r) mod m
    if(r==0) return 1;
    return C(n%m,r%m,m) * Lucas(n/m,r/m,m)%m;           //分两部分计算
}
int main(){
    int T; cin >> T;
    while(T--){
        int a,b,m; cin >>a>>b>>m;
        fac[0] = 1;
        for(int i=1;i<=m;i++)   fac[i]=(fac[i-1]*i)%m;  //预计算阶乘，取模
        cout << Lucas(a+b,a,m) << endl;
    }
}
```

#### 7.5 容斥原理

例7-7.  硬币购物 洛谷P1450

```cpp
#include<cstdio>
const int N=100009;
long long dp[N];
int main(){
	int c[4],d[4];
	for(int i=0;i<4;i++)  scanf("%d",&c[i]);
	dp[0]=1;
	for(int i=0;i<4;i++)              //完全背包，预处理
	    for(int j=c[i];j<N;j++)
	        dp[j] += dp[j-c[i]];
    int T; scanf("%d",&T);
	while(T--){
	     for(int i=0;i<4;i++) scanf("%d",&d[i]);
	     int s; scanf("%d",&s);
	     long long ans = dp[s];       //容斥定理公式的第1项
	     for(int i=1;i<=15;i++){      //i：0001~1111，二进制数枚举集合
	         int now = s;
	         int tmp = i;
	         int ov = 0;              //用ov判断奇偶
	         for(int j=0;tmp;j++){    //容斥
	             if(tmp&1)
	                ov^=1, now -= (d[j]+1)*c[j];
	             tmp = tmp >>1;        //tmp找i中的1
		    }
		    if(now<0) continue;
		    if(ov) ans -= dp[now];   //奇数，减去
             else   ans += dp[now];   //偶数，加上
		}
		printf("%lld\n",ans);
	}
	return 0;
}
```

#### 7.6 Catalan数和Stirling数

**7.6.2 Stirling数**

例7-10.  小朋友的球  洛谷 P1655

```python
N = 105
S = [[0] * N for i in range(N)]  
for i in range(1, N):
	S[i][i] = S[i][1] = 1
	for j in range(2, i):  S[i][j] = S[i - 1][j - 1] + j * S[i - 1][j]
while True:		# 多组数据
    try:              n, k = map(int,input().split());  print(S[n][k])
    except EOFError:  break
```

#### 7.7 Burnside定理和Pólya计数

**7.7.3 Pólya计数**

例7-12.  Necklace of Beads  poj 1286

```cpp
#include <cstdio>
#include <cmath>
#include <algorithm>
using namespace std;
#define ll long long
int main(){
    ll n;
    while(scanf("%lld", &n) && n != -1 ) {
        if(n==0 ){ printf("0\n"); continue;}
        ll ans = 0;                                         //答案很大，用long long 
        for(ll i=0; i<n; i++) ans += (ll)pow(3,__gcd(n,i)); //旋转。3指三种颜色
        if(n%2) ans += n*(ll)pow(3,n/2+1);                  //翻转：n是奇数
        else{                                               //翻转：n是偶数
            ans += n/2*(ll)pow(3,n/2);
            ans += n/2*(ll)pow(3,n/2+1);
        }
        printf("%lld\n", ans/(n*2));
    }
    return 0 ;
}
```

#### 7.8 母函数

**7.8.1 普通型母函数**

例7-13.  整数划分  hdu 1028

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 200;
int dp[N+1][N+1];            //dp[n][m]: 将n划分成最大数不超过m的划分数
void part() {                //预计算dp[n][m]，求出所有n的划分
    for(int n=1; n<=N; n++)
        for(int m=1; m<=N; m++){
            if((n==1)||(m==1))   dp[n][m] = 1;
            else if(n < m)       dp[n][m] = dp[n][n];
            else if(n == m)      dp[n][n] = dp[n][n-1]+1;
            else                 dp[n][m] = dp[n-m][m] + dp[n][m-1] ;
        }                
}
int main(){
    int n;
    part();
    while(cin >> n)  cout << dp[n][n] << endl;
    return 0;
}

```

```cpp
//用母函数求整数划分（hdu 1028）
#include<bits/stdc++.h>
using namespace std;
const int N=200;
int c1[N+1], c2[N+1];
void part() {
    int i, j, k;
    for(i=0; i<=N; i++){      //初始化，第一部分(1 + x + x2 + ...)的系数，都是1
        c1[i]=1;  c2[i]=0;
    }
    for(k=2; k<=N; k++){      //从第2部分(1 + x2 + x4 + ...)开始展开
        for(i=0; i<=N; i++)   //k=2时，i循环第一部分(1+x+x2+...)，j循环第二部分(1+x2+x4+...)
for(j=0; j+i <= N; j+=k)
     	       c2[i+j] += c1[i];
        for(i=0; i <= N; i++) { c1[i] = c2[i];  c2[i] = 0; }  //更新本次展开的结果
    }
}
int main(){
    int n;
    part();
    while(cin >> n)   cout << c1[n] << endl;
    return 0;
}

```

#### 7.9 公平组合游戏（博弈论）

**7.9.2 尼姆游戏**

例7-18.  Being a Good Boy in Spring Festival  hdu 1850  
主要代码如下：

```cpp
int sum=0, ans=0;                  //sum是Nim-sum，ans是第一步可行方案数
for(int i=0; i<n; i++)   sum ^= a[i];      //异或计算，求Nim-sum
if(sum==0)   cout<<0<<endl;        //开始局面是P-position，先手必败
else{                              //开始局面是N-position，先手胜
for(int i=0; i<n; i++)
    if((sum^a[i]) <= a[i])     //计算第一步所有可能方案
       ans++;
cout<<ans<<endl;
}
```

**7.9.3 图游戏与Sprague-Grundy函数**

hdu 1846为

```cpp
#include<bits/stdc++.h>
using namespace std;
const int MAX = 1001;
int n, m, sg[MAX], s[MAX];
void getSG(){
    memset(sg, 0, sizeof(sg));
    for (int i=1; i<=n; i++){
        memset(s, 0, sizeof(s));
        for (int j=1; j<=m && i-j>=0; j++) s[sg[i-j]] = 1;  //把i的后继点放到集合s中
        for (int j=0; j<=n; j++)      //计算sg[i]
            if(!s[j]){ sg[i]=j; break;}
    }
}
int main(){
    int c;  cin>>c;
    while (c--){
        cin>>n>>m;
        getSG();
        if (sg[n])  cout<<"first\n";     //sg != 0，先手胜
        else         cout<<"second\n";    //sg == 0，后手胜
    }
    return 0;
}
```

例7-19.  Fibonacci again and again  hdu 1848  

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1001;
int sg[N], s[N];
int fibo[15]={1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987};
void getSG(){                          //计算每一堆的SG值
     for(int i=0;i<=N;i++){
        sg[i]=i;
        memset(s, 0, sizeof(s));
        for(int j=0; j<15 && fibo[j]<=i; j++){
            s[sg[i-fibo[j]]] = 1;
            for(int j=0; j<=i; j++)
                if(!s[j]) { sg[i] = j; break;}
        }
     }
}
int main(){
    getSG();                //预计算sg值
    int n,m,p;
    while(cin>>n>>m>>p && n+m+p){
        if(sg[n]^sg[m]^sg[p])      cout << "Fibo" <<endl;
        else                       cout << "Nacci"<<endl;
    }
    return 0;
}
```

**7.9.4 威佐夫游戏**

例7-20.  取石子游戏 洛谷 P2252

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
    int n, m;
    double gold = (1 + sqrt(5))/2;             //黄金分割=1.618033988749894…精确到小数15位
    while(cin >> n >> m){
        int a = min(n, m), b = max(n, m);
        double k = (double)(b – a);
        int test = (int)(k*gold);              //乘以黄金分割数，然后取整
        if(test == a)    cout << 0 << endl;    //先手败
        else             cout << 1 << endl;    //先手胜
    }
    return 0;
}
```


### 第8章 计算几何

```cpp
const double pi = acos(-1.0);          //圆周率，精确到15位小数：3.141592653589793
const double eps = 1e-8;               //偏差值，有时用1e-10，但是要注意精度
int sgn(double x){                     //判断x的大小
    if(fabs(x) < eps)  return 0;       //x==0，返回0
    else return x<0?-1:1;              //x<0返回-1，x>0返回1
}
int dcmp(double x, double y){          //比较两个浮点数 
    if(fabs(x - y) < eps)   return 0;  //x==y，返回0
    else return x<y ?-1:1;             //x<y返回-1，x>y返回1
}
```

#### 8.1 二维几何

**8.1.1 点和向量**

1. 点

二维平面中的点，用坐标(x, y)来表示。

```cpp
struct Point{  
    double x,y;
    Point(){}
    Point(double x,double y):x(x),y(y){}
};
```

2. 两点之间的距离

把两点看成直角三角形的两个顶点，斜边就是两点的距离。

（1）用库函数hypot()计算直角三角形的斜边长。

```cpp
double Distance(Point A, Point B){ return hypot(A.x-B.x,A.y-B.y); }
```

（2）或者用sqrt()函数计算。

```cpp
double Dist(Point A,Point B){ return sqrt((A.x-B.x)*(A.x-B.x) + (A.y-B.y)*(A.y-B.y)); }
```

3. 向量

```cpp
typedef Point Vector;
```

提示：向量并不是一个有向线段，只是表示方向和大小。所以向量平移后仍然不变。

4. 向量的运算

```cpp
（1）加：点与点的加法运算没有意义；点与向量相加得到另一个点；向量与向量相加得到另外一个向量。如图8.2所示。
Point operator + (Point B){return Point(x+B.x,y+B.y);}
（2）减：两个点的差是一个向量；向量A减B，得到由B指向A的向量。	
Point operator - (Point B){return Point(x-B.x,y-B.y);}
（3）乘：向量与实数相乘得到等比例放大的向量。	
Point operator * (double k){return Point(x*k,y*k);}
（4）除：向量与实数相除得到等比例缩小的向量。	
Point operator / (double k){return Point(x/k,y/k);}
（5）等于：	
bool operator == (Point B){return sgn(x-B.x)==0 && sgn(y-B.y)==0;}
```

**8.1.2 点积和叉积**

向量的基本运算是点积和叉积，它们定义了向量的大小、方向的基本关系，计算几何的各种操作几乎都基于这两种运算。

1. 点积（Dot product）	

```cpp
double Dot(Vector A,Vector B){ return A.x*B.x + A.y*B.y; }
```

2. 点积的应用

（2）求向量A的长度	
```cpp
double Len(Vector A){return sqrt(Dot(A,A));}
```

由于开方运算可能导致小数，可以改为求长度的平方，避免开方运算：

```cpp
double Len2(Vector A){return Dot(A,A);}
```

（3）求向量A与B的夹角大小

```cpp	
double Angle(Vector A,Vector B){return acos(Dot(A,B)/Len(A)/Len(B));}
```

3. 叉积（Cross product）

计算叉积A×B时，也不需要用到夹角θ，而是用下面的简单代码计算：	 

```cpp
double Cross(Vector A,Vector B){return A.x*B.y – A.y*B.x;}
```

A×B与B×A相反。

叉积有正负，这个性质使得叉积能用于很多重要的场合。	

4. 叉积的基本应用

2）计算两向量构成的平行四边形有向面积

三个点A、B、C，以A为公共点，得到2个向量B-A和C-A，它们构成的平行四边形，面积是：

```cpp
double Area2(Point A,Point B,Point C){ return Cross(B-A, C-A);}
```

4）向量旋转	

```cpp
Vector Rotate(Vector A, double rad){  
	return Vector(A.x*cos(rad)-A.y*sin(rad), A.x*sin(rad)+A.y*cos(rad));
}
```

特殊情况是旋转90度：

逆时针旋转90度：Rotate(A, pi/2)，返回Vector(-A.y, A.x)；

顺时针旋转90度：Rotate(A, -pi/2)，返回Vector(A.y, - A.x)。

有时需要求单位法向量，即逆时针转90度，然后取单位值。	

```cpp
Vector Normal(Vector A){return Vector(-A.y/Len(A), A.x/Len(A));}
```

5）用叉积检查两个向量是否平行或重合

```cpp
bool Parallel(Vector A, Vector B){return sgn(Cross(A,B)) == 0;}  //返回true表示平行或重合
```

**8.1.3 点和线**

1. 直线的表示
	
```cpp
struct Line{
    Point p1,p2;                  //（1）线上的两个点
    Line(){}
    Line(Point p1,Point p2):p1(p1),p2(p2){}
    Line(Point p,double angle){    //（4）根据一个点和倾斜角 angle 确定直线,0<=angle<pi
        p1 = p;
        if(sgn(angle – pi/2) == 0){p2 = (p1 + Point(0,1));}
        else{p2 = (p1 + Point(1,tan(angle)));}
    }
    Line(double a,double b,double c){     //（2）ax+by+c=0
        if(sgn(a) == 0){
            p1 = Point(0,-c/b);
            p2 = Point(1,-c/b);
        }
        else if(sgn(b) == 0){
            p1 = Point(-c/a,0);
            p2 = Point(-c/a,1);
        }
        else{
            p1 = Point(0,-c/b);
            p2 = Point(1,(-c-a)/b);
        }
    }
};
```

2. 线段的表示	

```cpp
typedef Line Segment;
```

3. 点和直线的位置关系	

```cpp
int Point_line_relation(Point p, Line v){
    int c = sgn(Cross(p-v.p1,v.p2-v.p1));
    if(c < 0)return 1;              //1：p在v的左边
        if(c > 0)return 2;              //2：p在v的右边
        return 0;                       //0：p在v上
}
```

4. 点和线段的位置关系

```cpp
bool Point_on_seg(Point p, Line v){ //点和线段：0 点不在线段v上；1 点在线段v上
   	return sgn(Cross(p-v.p1, v.p2-v.p1)) == 0 && sgn(Dot(p – v.p1,p- v.p2)) <= 0;
}
```

5. 点到直线的距离

```cpp
double Dis_point_line(Point p, Line v){
    return fabs(Cross(p-v.p1,v.p2-v.p1))/Distance(v.p1,v.p2);
}
```

6. 点在直线上的投影

```cpp
Point Point_line_proj(Point p, Line v){
    double k = Dot(v.p2-v.p1,p-v.p1)/Len2(v.p2-v.p1);
    return v.p1+(v.p2-v.p1)*k;
}
```

7. 点关于直线的对称点

```cpp
Point Point_line_symmetry(Point p, Line v){
    Point q = Point_line_proj(p,v);
    return Point(2*q.x-p.x,2*q.y-p.y);
}
```

8. 点到线段的距离	

```cpp
double Dis_point_seg(Point p, Segment v){
    if(sgn(Dot(p- v.p1,v.p2-v.p1))<0 || sgn(Dot(p- v.p2,v.p1-v.p2))<0)
        return min(Distance(p,v.p1),Distance(p,v.p2));
    return Dis_point_line(p,v);           //点的投影在线段上
}
```

9. 两条直线的位置关系

```cpp
int Line_relation(Line v1, Line v2){
    if(sgn(Cross(v1.p2-v1.p1,v2.p2-v2.p1)) == 0){
    if(Point_line_relation(v1.p1,v2)==0) return 1;  //1 重合
    else return 0;                                  //0 平行
    }
    return 2;                                           //2 相交
}
```

10. 两条直线的交点	

```cpp
Point Cross_point(Point a,Point b,Point c,Point d){  //Line1:ab,  Line2:cd
    double s1 = Cross(b-a,c-a);
    double s2 = Cross(b-a,d-a);                    //叉积有正负
    return Point(c.x*s2-d.x*s1,c.y*s2-d.y*s1)/(s2-s1);
}
```

11. 两个线段是否相交

```cpp
bool Cross_segment(Point a,Point b,Point c,Point d){    //Line1:ab,  Line2:cd
	double c1 = Cross(b-a,c-a),c2=Cross(b-a,d-a);
	double d1 = Cross(d-c,a-c),d2=Cross(d-c,b-c);
	return sgn(c1)*sgn(c2) < 0 && sgn(d1)*sgn(d2) < 0;  //1相交；0不相交
}
```

例8-1. 神秘大三角  洛谷P1355
	
```cpp
#include <bits/stdc++.h>
using namespace std;
struct Point{
    int x,y;
    Point (){}
    Point (int x,int y):x(x),y(y){}
    Point operator + (Point B){return Point(x+B.x,y+B.y);}
    Point operator - (Point B){return Point(x-B.x,y-B.y);}
}v[3],p;                                                   //v:三角形， p:点
typedef Point Vector;                                      //定义向量
double Cross(Vector A,Vector B){return A.x*B.y - A.y*B.x;} //叉积
int main(){
    int left=0,right=0;
    for(int i=0;i<3;i++) scanf(" (%d,%d)",&v[i].x,&v[i].y);
    scanf(" (%d,%d)",&p.x,&p.y);
    for(int i=0;i<3;i++) {
        int relation = Cross(p-v[i],p-v[(i+1)%3]);
        if(relation > 0) right++;              //p在直线v右边
        if(relation < 0) left++;               //p在直线v左边
    }
    if(right==3 || left==3)    puts("1");      //在三角内
    else if(right>0 && left>0) puts("2");      //在三角外
    else if(right+left==1)     puts("4");      //在顶点上
    else puts("3");                            //在边界上，不含顶点
    return 0;
}
```

**8.1.4 多边形**

1. 点和多边形的关系

```cpp
int Point_in_polygon(Point pt,Point *p,int n){  //点pt，多边形Point *p
    for(int i = 0;i < n;i++){                   //3：点在多边形的顶点上
    if(p[i] == pt)  return 3;
}
for(int i = 0;i < n;i++){                   //2：点在多边形的边上
    Line v=Line(p[i],p[(i+1)%n]);
    if(Point_on_seg(pt,v)) return 2;
}
int num = 0;
for(int i = 0;i < n;i++){
       int j = (i+1)% n;
       int c = sgn(Cross(pt-p[j],p[i]-p[j]));
       int u = sgn(p[i].y – pt.y);
       int v = sgn(p[j].y – pt.y);
       if(c > 0 && u < 0 && v >=0) num++;
       if(c < 0 && u >=0 && v < 0) num--;
  }
    return num != 0;                            //1：点在内部; 0：点在外部
}
```

2. 多边形的面积

```cpp
double Polygon_area(Point *p, int n){    //Point *p表示多边形
    double area = 0;
    for(int i = 0;i < n;i++)
        area += Cross(p[i],p[(i+1)%n]);
    return area/2;                    //面积有正负，返回时不能简单地取绝对值
}
```

3. 多边形的重心

例8-2.  Lifting the Stone  poj 1385

下面代码中的函数Polygon_center()返回多边形的重心。

```CPP
#include <stdio.h>
struct Point{
    double x,y;
    Point(double X=0,double Y=0){x=X,y=Y;}
    Point operator + (Point B){return Point (x+B.x,y+B.y);}
    Point operator - (Point B){return Point (x-B.x,y-B.y);}
    Point operator * (double k){return Point (x*k,y*k);}
    Point operator / (double k){return Point (x/k,y/k);}
};
typedef Point Vector;
double Cross(Vector A,Vector B){return A.x*B.y - A.y*B.x;}
double Polygon_area(Point *p, int n){         //求多边形面积
    double area = 0;
    for(int i = 0;i < n;i++)  area += Cross(p[i],p[(i+1)%n]);
    return area/2;                            //面积有正负，不能取绝对值
}
Point Polygon_center(Point *p, int n){        //求多边形重心
    Point ans(0,0);
    if(Polygon_area(p,n)==0) return ans;
    for(int i = 0;i < n;i++)
        ans = ans+(p[i]+p[(i+1)%n])*Cross(p[i],p[(i+1)%n]);
    return ans/Polygon_area(p,n)/6;
}
Point p[100000];
int main(){
    int t; scanf("%d",&t);
    while(t--){
        int n; scanf("%d",&n);
        for(int i=0;i<n;i++) scanf("%lf%lf",&p[i].x,&p[i].y);
        Point c = Polygon_center(p,n);   //重心坐标
        printf("%.2f %.2f\n",c.x,c.y);   //注意这里输出用%f，不是%lf
    }
    return 0;
}
```

**8.1.5 凸包**	

例8-3. 圈奶牛  洛谷 P2742

```CPP
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5+1;
const double eps = 1e-6;
int sgn(double x){        //判断x是否等于0
    if(fabs(x) < eps)  return 0;
    else return x<0?-1:1;
}
struct Point{
    double x,y;
    Point(){}
    Point(double x, double y):x(x),y(y){}
    Point operator + (Point B){return Point(x+B.x,y+B.y);}
    Point operator - (Point B){return Point(x-B.x,y-B.y);}
    bool operator == (Point B){return sgn(x-B.x) == 0 && sgn(y-B.y) == 0;}
    bool operator < (Point B){                  //用于sort()排序，先按x排序，再按y排序
        return sgn(x-B.x)<0 || (sgn(x-B.x)==0 && sgn(y-B.y)<0);}
};
typedef Point Vector;
double Cross(Vector A,Vector B){return A.x*B.y - A.y*B.x;}      //叉积
double Distance(Point A,Point B){return hypot(A.x-B.x,A.y-B.y);}
//Convex_hull()求凸包。凸包顶点放在ch中，返回值是凸包的顶点数
int Convex_hull(Point *p,int n,Point *ch){
n = unique(p,p+n)-p;    //去除重复点    
sort(p,p+n);            //对点排序：按x从小到大排序，如果x相同，按y排序    
    int v=0;
	//求下凸包。如果p[i]是右拐弯的，这个点不在凸包上，往回退
    for(int i=0;i<n;i++){
        while(v>1 && sgn(Cross(ch[v-1]-ch[v-2],p[i]-ch[v-1]))<=0) //把后面ch[v-1]改成ch[v-2]也行
			v--;
        ch[v++]=p[i];
    }
    int j=v;
	//求上凸包
    for(int i=n-2;i>=0;i--){
        while(v>j && sgn(Cross(ch[v-1]-ch[v-2],p[i]-ch[v-1]))<=0) //把后面ch[v-1]改成ch[v-2]也行
			v--;
        ch[v++]=p[i];
    }
    if(n>1) v--;
    return v;                      //返回值v是凸包的顶点数
}
Point p[N],ch[N];                  //输入点是p[]，计算得到的凸包顶点放在ch[]中
int main(){
    int n;   cin >> n;
    for(int i=0;i<n;i++)  scanf("%lf%lf",&p[i].x,&p[i].y);
    int v = Convex_hull(p,n,ch);    //返回凸包的顶点数v
    double ans=0;
    for(int i=0;i<v;i++)  ans += Distance(ch[i],ch[(i+1)%v]);  //计算凸包周长
    printf("%.2f\n",ans);
    return 0;
}
```

**8.1.6 最近点对**

例8-4. 平面上的最接近点对 洛谷 P1257

```CPP
#include <bits/stdc++.h>
using namespace std;
const double eps = 1e-8;
const int N = 100010;
const double INF = 1e20;
int sgn(double x){
    if(fabs(x) < eps)  return 0;
    else return x<0?-1:1;
}
struct Point{double x,y;};
double Distance(Point A, Point B){return hypot(A.x-B.x,A.y-B.y);}
bool cmpxy(Point A,Point B){                  //排序：先对横坐标x排序，再对y排序
	return sgn(A.x-B.x)<0 || (sgn(A.x-B.x)==0 && sgn(A.y-B.y)<0);
}
bool cmpy (Point A,Point B){return sgn(A.y-B.y)<0;} //只对y坐标排序
Point p[N],tmp_p[N];
double Closest_Pair(int left,int right){
    double dis = INF;
    if(left == right) return dis;            //只剩1个点
    if(left + 1 == right) return Distance(p[left], p[right]);//只剩2个点
    int mid = (left+right)/2;                //分治
    double d1 = Closest_Pair(left,mid);      //求s1内的最近点对
    double d2 = Closest_Pair(mid+1,right);   //求s2内的最近点对
    dis = min(d1,d2);
    int k = 0;
    for(int i=left;i<=right;i++)             //在s1和s2中间附近找可能的最小点对
        if(fabs(p[mid].x - p[i].x) <= dis)   //按x坐标来找
            tmp_p[k++] = p[i];
	sort(tmp_p,tmp_p+k,cmpy);         //按y坐标排序，用于剪枝。这里不能按x坐标排序
    for(int i=0;i<k;i++)
        for(int j=i+1;j<k;j++){
            if(tmp_p[j].y - tmp_p[i].y >= dis)  break;    //剪枝
            dis = min(dis,Distance(tmp_p[i],tmp_p[j]));
        }
	return dis;  //返回最小距离
}
int main(){
    int n;  cin >>n;
    for(int i=0;i<n;i++) scanf("%lf%lf",&p[i].x,&p[i].y);
    sort(p,p+n,cmpxy);                          //先排序
    printf("%.4f\n",Closest_Pair(0,n-1));       //输出最短距离
    return 0;
}
```

**8.1.8 半平面交**

1. 半平面的表示	

```CPP
struct Line{
	Point p;      //直线上一个点
	Vector v;     //方向向量，它的左边是半平面
	double ang;   //极角，从x正半轴旋转到v的角度
	Line(){};
	Line(Point p, Vector v):p(p),v(v){ang = atan2(v.y, v.x);}
	bool operator < (Line &L){return ang < L.ang;}     //用于排序
};
```

2. 半平面交算法
	
例8-6.  Run  hdu 2297

```CPP
#include <bits/stdc++.h>
using namespace std;
const double INF = 1e12;
const double pi = acos(-1.0);    //圆周率，精确到15位小数：3.141592653589793
const double eps = 1e-8;
int sgn(double x){
    if(fabs(x) < eps)  return 0;
    else return x<0?-1:1;
}
struct Point{
    double x,y;
    Point(){}
    Point(double x,double y):x(x),y(y){}
    Point operator + (Point B){return Point(x+B.x,y+B.y);}
    Point operator – (Point B){return Point(x-B.x,y-B.y);}
    Point operator * (double k){return Point(x*k,y*k);}
};
typedef Point Vector;
double Cross(Vector A,Vector B){return A.x*B.y – A.y*B.x;} //叉积
struct Line{
	Point p;
	Vector v;
	double ang;
	Line(){};
	Line(Point p,Vector v):p(p),v(v){ang=atan2(v.y,v.x);}
	bool operator < (Line &L){return ang<L.ang;}     //用于极角排序
};
//点p在线L左边，即点p在线L在外面：
bool OnLeft(Line L,Point p){return sgn(Cross(L.v,p-L.p))>0;} 
Point Cross_point(Line a,Line b){    //两直线交点
    Vector u=a.p-b.p;
	double t=Cross(b.v,u)/Cross(a.v,b.v);
	return a.p+a.v*t;
}
vector<Point> HPI(vector<Line> L){     //求半平面交，返回凸多边形
	int n=L.size();
	sort(L.begin(),L.end());           //将所有半平面按照极角排序。
	int first,last;                    //指向双端队列的第一个和最后一个元素
	vector<Point> p(n);                //两个相邻半平面的交点
	vector<Line> q(n);                 //双端队列
	vector<Point> ans;                 //半平面交形成的凸包
	q[first=last=0]=L[0];
	for(int i=1;i<n;i++){
		//情况1：删除尾部的半平面
		while(first<last && !OnLeft(L[i], p[last-1])) last--; 
		//情况2：删除首部的半平面：
		while(first<last && !OnLeft(L[i], p[first]))  first++; 
		q[++last]=L[i];     //将当前的半平面加入双端队列尾部
		//极角相同的两个半平面，保留左边：
		if(fabs(Cross(q[last].v,q[last-1].v)) < eps){ 
		last--;
			if(OnLeft(q[last],L[i].p)) q[last]=L[i];
		}
		//计算队列尾部半平面交点：
		if(first<last) p[last-1]=Cross_point(q[last-1],q[last]);
	}
	//情况3：删除队列尾部的无用半平面
	while(first<last && !OnLeft(q[first],p[last-1])) last--;
	if(last-first<=1) return ans;   //空集
	p[last]=Cross_point(q[last],q[first]);  //计算队列首尾部的交点。
	for(int i=first;i<=last;i++)  ans.push_back(p[i]);   //复制。
	return ans;               //返回凸多边形
}
int main(){
    int T,n; cin>>T;
    while(T--){
    cin>>n;
    vector<Line> L;	   
            L.push_back(Line(Point(0,0),Vector(0,-1)));  //加一个半平面F:反向y轴 		
        L.push_back(Line(Point(0,INF),Vector(-1,0)));   //加一个半平面E:y极大的向左的直线
        while(n--){
                double a,b;	scanf(“%lf%lf”,&a,&b);
                L.push_back(Line(Point(0,a),Vector(1,b)));
        }
        vector<Point> ans=HPI(L);         //得到凸多边形
        printf(“%d\n”,ans.size()-2);    //去掉人为加的两个点
    }
    return 0;
}
```

#### 8.2 圆

**8.2.1 基本的定义和计算**

1. 圆的定义

用圆心和半径表示圆。

```CPP
struct Circle{
    Point c;      //圆心
    double r;     //半径
    Circle(){}
    Circle(Point c,double r):c(c),r(r){}
    Circle(double x,double y,double _r){c=Point(x,y);r = _r;}
};
```

2. 点和圆的关系

点和圆的关系，根据点到圆心的距离判断。

```CPP
int Point_circle_relation(Point p, Circle C){
    double dst = Distance(p,C.c);
    if(sgn(dst – C.r) < 0) return 0;       //0 点在圆内
    if(sgn(dst – C.r) ==0) return 1;       //1 圆上
    return 2;                               //2 圆外
}
```

3. 直线和圆的关系

直线和圆的关系，根据圆心到直线的距离判断。

```CPPS
int Line_circle_relation(Line v,Circle C){
    double dst = Dis_point_line(C.c,v);
    if(sgn(dst-C.r) < 0) return 0;     //0 直线和圆相交
    if(sgn(dst-C.r) ==0) return 1;     //1 直线和圆相切
    return 2;                               //2 直线在圆外
}
```

4. 线段和圆的关系

线段和圆的关系，根据圆心到线段的距离判断。

```CPP
int Seg_circle_relation(Segment v,Circle C){
    double dst = Dis_point_seg(C.c,v);
    if(sgn(dst-C.r) < 0) return 0;      //0线段在圆内
    if(sgn(dst-C.r) ==0) return 1;      //1线段和圆相切
    return 2;                           //2线段在圆外
}
```

5. 直线和圆的交点

```CPP
//pa, pb是交点。返回值是交点个数
int Line_cross_circle(Line v,Circle C,Point &pa,Point &pb){
  	if(Line_circle_relation(v, C)==2)  return 0;//无交点
	Point q = Point_line_proj(C.c,v);          //圆心在直线上的投影点
   	double d = Dis_point_line(C.c,v);          //圆心到直线的距离
	double k = sqrt(C.r*C.r-d*d);    
    if(sgn(k) == 0){                            //1个交点，直线和圆相切
   	    pa = q;	pb = q;	return 1;
    }
    Point n=(v.p2-v.p1)/ Len(v.p2-v.p1);       //单位向量
    pa = q + n*k;  pb = q - n*k;
    return 2;                                  //2个交点
}
```

**8.2.2 最小圆覆盖**

例8-7. 最小圆覆盖  洛谷P1742

```CPP
#include <bits/stdc++.h>
using namespace std;
#define eps 1e-8
const int N = 1e5+1;
int sgn(double x){
    if(fabs(x) < eps)  return 0;
    else return x<0?-1:1;
}
struct Point{ double x, y; };
double Distance(Point A, Point B){return hypot(A.x-B.x,A.y-B.y);}
Point circle_center(const Point a, const Point b, const Point c){
    Point center;
    double a1=b.x-a.x, b1=b.y-a.y, c1=(a1*a1+b1*b1)/2;
    double a2=c.x-a.x, b2=c.y-a.y, c2=(a2*a2+b2*b2)/2;
    double d =a1*b2-a2*b1;
    center.x =a.x+(c1*b2-c2*b1)/d;
    center.y =a.y+(a1*c2-a2*c1)/d;
    return center;
}
void min_cover_circle(Point *p, int n, Point &c, double &r){
    random_shuffle(p, p + n);             //随机函数，打乱所有点。这一步很重要
    c=p[0]; r=0;                          //从第1个点p0开始。圆心为p0，半径为0
    for(int i=1;i<n;i++)                  //扩展所有点
        if(sgn(Distance(p[i],c)-r)>0){    //点pi在圆外部
            c=p[i]; r=0;                  //重新设置圆心为pi，半径为0
            for(int j=0;j<i;j++)          //重新检查前面所有的点。
                if(sgn(Distance(p[j],c)-r)>0){   //两点定圆
                    c.x=(p[i].x + p[j].x)/2;
                    c.y=(p[i].y + p[j].y)/2;
                    r=Distance(p[j],c);
                    for(int k=0;k<j;k++)
                        if (sgn(Distance(p[k],c)-r)>0){   //两点不能定圆，就三点定圆
                            c=circle_center(p[i],p[j],p[k]);
                            r=Distance(p[i], c);
                        }
                }
        }
}
Point p[N];  
int main(){
    int n; cin >> n;
    for(int i=0;i<n;i++) scanf("%lf%lf",&p[i].x,&p[i].y);
    Point c; double r;                  //最小覆盖圆的圆心和半径
    min_cover_circle(p,n,c,r);
    printf("%.10f\n%.10f %.10f\n",r,c.x,c.y);
    return 0;
}
```

#### 8.3 三维几何

**8.3.1 三维点和线**

1. 点和向量

```CPP
struct Point3{             //三维点
    double x,y,z;
    Point3(){}
    Point3(double x,double y,double z):x(x),y(y),z(z){}
    Point3 operator + (Point3 B){return Point3(x+B.x,y+B.y,z+B.z);}
    Point3 operator – (Point3 B){return Point3(x-B.x,y-B.y,z-B.z);}
    Point3 operator * (double k){return Point3(x*k,y*k,z*k);}
    Point3 operator / (double k){return Point3(x/k,y/k,z/k);}
    bool operator == (Point3 B){	return sgn(x-B.x)==0 && sgn(y-B.y)==0 && sgn(z-B.z)==0;}
};
typedef Point3 Vector3;    //三维向量
```

两点之间的距离：

```CPP
double Distance(Vector3 A,Vector3 B){
	return sqrt((A.x-B.x)*(A.x-B.x)+(A.y-B.y)*(A.y-B.y)+ (A.z-B.z)*(A.z-B.z));    
}
```

2. 线和线段

```CPP
struct Line3{
    Point3 p1,p2;
    Line3(){}
    Line3(Point3 p1,Point3 p2):p1(p1),p2(p2){}
};
typedef Line3 Segment3;       //定义线段，两端点是Point p1,p2
```

**8.3.2 三维点积**

1. 三维点积的定义

求向量A、B点积的代码：

```CPP
double Dot(Vector3 A,Vector3 B){return A.x*B.x+A.y*B.y+A.z*B.z;}
```

2. 三维点积的基本应用

2）求向量A的长度

```CPP
double Len(Vector3 A){ return sqrt(Dot(A, A));}
	或者是求长度的平方，避免开方运算：
double Len2(Vector3 A){ return Dot(A, A);}
```

3）求向量A与B的夹角

```CPP
double Angle(Vector3 A,Vector3 B){return acos(Dot(A,B)/Len(A)/Len(B));}
```

**8.3.3 三维叉积**

1. 三维叉积的定义

```CPP
Vector3 Cross(Vector3 A,Vector3 B){
	return Point3(A.y*B.z-A.z*B.y, A.z*B.x-A.x*B.z, A.x*B.y-A.y*B.x);
}
```

2. 三维叉积的基本应用

1） 求三角形面积

```CPP
//三角形面积的2倍
double Area2(Point3 A,Point3 B,Point3 C){return Len(Cross(B-A, C-A));}
```

2）点和线的有关问题

```CPP
//三维：点在直线上
bool Point_line_relation(Point3 p,Line3 v){
    return sgn( Len(Cross(v.p1-p,v.p2-p))) == 0 && sgn(Dot(v.p1-p,v.p2-p))== 0;
}
//三维：点到线段距离。
double Dis_point_seg(Point3 p, Segment3 v){
    if(sgn(Dot(p- v.p1,v.p2-v.p1)) < 0 || sgn(Dot(p- v.p2,v.p1-v.p2)) < 0)
        return min(Distance(p,v.p1),Distance(p,v.p2));
    return Dis_point_line(p,v);
}
//三维：点 p 在直线上的投影
Point3 Point_line_proj(Point3 p, Line3 v){
    double k=Dot(v.p2-v.p1,p-v.p1)/Len2(v.p2-v.p1);
    return v.p1+(v.p2-v.p1)*k;
}
```

3）平面

```CPP
struct Plane{
    Point3 p1,p2,p3;     //平面上的三个点
    Plane(){}
    Plane(Point3 p1,Point3 p2,Point3 p3):p1(p1),p2(p2),p3(p3){}
};
```

4）平面法向量

```CPP
Point3 Pvec(Point3 A, Point3 B, Point3 C){return Cross(B-A,C-A);}
或者：
Point3 Pvec(Plane f){return Cross(f.p2-f.p1,f.p3-f.p1);}
```

5）平面的有关问题

```CPP
//四点共平面
bool Point_on_plane(Point3 A,Point3 B,Point3 C,Point3 D){
    return sgn(Dot(Pvec(A,B,C),D-A)) == 0;
}
//两平面平行
int Parallel(Plane f1, Plane f2){ return Len(Cross(Pvec(f1),Pvec(f2))) < eps; }
//两平面垂直
int Vertical (Plane f1, Plane f2){ return sgn(Dot(Pvec(f1),Pvec(f2)))==0; }
```

6）直线和平面的交点

```CPP
int Line_cross_plane(Line3 u,Plane f,Point3 &p){
    Point3 v = Pvec(f);                           //平面的法向量
    double x = Dot(v, u.p2-f.p1);
    double y = Dot(v, u.p1-f.p1);
    double d = x-y;
    if(sgn(x) == 0 && sgn(y) == 0) return -1;    //-1：v在f上
    if(sgn(d) == 0) return 0;                    //0：v与f平行
    p = ((u.p1 * x)-(u.p2 * y))/d;               //v与f相交
    return 1;
}
```

7）四面体的有向体积

```CPP
//四面体有向体积*6
double volume4(Point3 a,Point3 b,Point3 c,Point3 d){
    return Dot(Cross(b-a,c-a),d-a); }
```	

**8.3.4 最小球覆盖**

例8-8. super star  poj 2069

下面是最小球覆盖的模拟退火代码

```cpp
#include<algorithm>
#include<cmath>
using namespace std;
const double eps = 1e-7;
struct Point3{double x,y,z;} p[35];
int n;
double Distance(Point3 A,Point3 B){
	return sqrt((A.x-B.x)*(A.x-B.x)+(A.y-B.y)*(A.y-B.y)+ (A.z-B.z)*(A.z-B.z));
}
double solve(){
    double T=100.0;                    //初始温度
    double delta = 0.98;               //降温系数
    Point3 c = p[0];                   //球心
    int pos;
    double r;                          //半径
    while(T>eps)    {                  //eps是终止温度
        pos = 0; r = 0;                //初始：p[0]是球心，半径是0
        for(int i=0; i<n; i++)         //迭代：找距离球心最远的点
            if(Distance(c,p[i])>r){
                r = Distance(c,p[i]);  //距离球心最远的点肯定在球周上
                pos = i;
            }
        c.x += (p[pos].x-c.x)/r*T;     //逼近最后的解
        c.y += (p[pos].y-c.y)/r*T;
        c.z += (p[pos].z-c.z)/r*T;
        T *= delta;                    //降温
    }
    return r;
}
int main(){
    double ans;
    while(~scanf("%d",&n),n) {
        for(int i=0;i<n;i++) scanf("%lf%lf%lf",&p[i].x,&p[i].y,&p[i].z);
        ans = solve();
        printf("%.5f\n",ans);
    }
    return 0;
}
```

**8.3.6 三维几何例题**

1. 化球为圆

例8-9.  Ghost Busters  poj 2177

```cpp
#include <cmath>
#include <vector>
#include <algorithm>
using namespace std;
const int N=105;
const double eps = 1e-7;
int sgn(double x){                        //判断x是否等于0
    if(fabs(x)<eps) return 0;
    else            return x<0?-1:1;
}
struct Point3{
    double x,y,z;
    Point3(){}
    Point3(double x,double y,double z):x(x),y(y),z(z){}
    Point3 operator + (Point3 B){return Point3(x+B.x,y+B.y,z+B.z);}
    Point3 operator - (Point3 B){return Point3(x-B.x,y-B.y,z-B.z);}
    Point3 operator * (double k){return Point3(x*k,y*k,z*k);}
    Point3 operator / (double k){return Point3(x/k,y/k,z/k);}
    Point3 adjust (double L){             //调整长度
        double len=sqrt(x*x+y*y+z*z);
        L/=len;
        return Point3(x*L,y*L,z*L);
    }
};
typedef Point3 Vector3;
double Dot(Vector3 A,Vector3 B){return A.x*B.x+A.y*B.y+A.z*B.z;}   //点积
Vector3 Cross(Vector3 A,Vector3 B)
{ return Point3(A.y*B.z-A.z*B.y,A.z*B.x-A.x*B.z,A.x*B.y-A.y*B.x);} //叉积
double Len(Vector3 A){return sqrt(Dot(A,A));}                      //向量的长度
double Distance(Point3 A,Point3 B)                                 //两点的距离
{ return sqrt((A.x-B.x)*(A.x-B.x)+(A.y-B.y)*(A.y-B.y)+(A.z-B.z)*(A.z-B.z)); }
struct Line3{                                                      //三维：线
    Point3 p1,p2;
    Line3(){}
    Line3(Point3 p1,Point3 p2):p1(p1),p2(p2){}
};
double Dis_point_line(Point3 p, Line3 v)           //三维：点到直线距离
{  return Len(Cross(v.p2-v.p1,p-v.p1))/Distance(v.p1,v.p2); }
vector<Point3> P;                                  //储存球的交点
void intersect(Point3 c1,double r1,Point3 c2,double r2){      //计算球的交点
    double d1=Len(c1),d2=Len(c2);                  //球心到原点距离
    c2=c2/d2*d1;                                   //调整球2，让两个球心与原点等距
    r2=r2/d2*d1;
    double d=Len(c1-c2);  //连心线长度
    if(sgn(d-r1-r2)==0){P.push_back(c1+(c2-c1)/d*r1);return;}      //(1)相切，存相切的点
    if(sgn(d-r1-r2)>0)        return;               //(2)相离，没有交点
    if(sgn(d-fabs(r1-r2))<=0) return;               //(3)包含，没有交点
    //(4)下面处理两个圆相交，有两个交点
    double b=(r1*r1+d*d-r2*r2)/(2*d);               //余弦定理
    double h =sqrt(r1*r1-b*b);
    Point3 M=c1+(c2-c1)/d*b;                        //两交点中点所在位置
    Point3 v=Cross(c1,M);                           //叉积求得两交点所在直线的向量
    v=v.adjust(h)+M;
    P.push_back(v);
    P.push_back(M*2-v);                              
}
int check(Point3 p,Point3 c,double r){ //检查交点p是否在球内或球面上
    Line3 v(Point3(0,0,0),p);
    double x = Dis_point_line(c,v);
    return sgn(x-r) <= 0;
}
Point3 c[N];     //球心
double r[N];     //球半径
int main (){
    int n; scanf("%d",&n);
    for(int i=1;i<=n;i++) {
        scanf("%lf%lf%lf",&(c[i].x),&(c[i].y),&(c[i].z));
        scanf("%lf",&r[i]);
        P.push_back(c[i]);
    }
    for(int i=1;i<=n;i++)              //求任意两个球的交点，记录在vector P中
        for(int j=i+1;j<=n;j++)
            intersect(c[i],r[i],c[j],r[j]);
    int ans=0,temp,w;                  //w记录选中的交点
    for(int i=0;i<P.size();i++) {      //检查每条射线，记录穿过最多球体的数量
        temp=0;
        for(int j=1;j<=n;j++) temp += check(P[i],c[j],r[j]);
        if(temp>ans)          ans=temp, w=i;
    }
    printf("%d\n",ans);
    for(int i=1;i<=n && ans;i++)    //枚举所有球，判断与选定的最佳射线是否相交
        if(check(P[w],c[i],r[i])) {
            ans--;
            printf(ans!=0?"%d ":"%d\n",i);
        }
    return 0;
}
```


2. 仿射变换

例8-10. A Letter to Programmers    https://vjudge.net/problem/UVALive-5719

```cpp
//改写自：www.cppblog.com/hanfei19910905/archive/2012/06/24/180035.html
#include<bits/stdc++.h>
using namespace std;
const double eps = 1e-6;
const double pi = acos(-1.0);            //圆周率，精确到15位小数：3.141592653589793
const int N = 4;
struct matrix {
    double num[N][N];
    matrix (double a){                           //单位矩阵乘以a
       memset(num,0,sizeof(num));
       for(int i=0;i<N;i++)  num[i][i] = a;
    }
    matrix(double x,double y,double z){          //平移变换
        memset(num,0,sizeof(num));
        for(int i=0;i<N;i++)  num[i][i] = 1;     //单位矩阵
        num[3][0] = x;   num[3][1] = y;   num[3][2] = z;
    }
    matrix(double x,double y,double z, int X){   //缩放变换
        memset(num,0,sizeof(num));
        for(int i=0;i<N;i++)  num[i][i] = 1;     //单位矩阵
        num[0][0] = x;   num[1][1] = y;   num[2][2] = z;
    }
    matrix(double P[3],double ang){              //旋转变换
        memset(num,0,sizeof(num));
        for(int i=0;i<N;i++)  num[i][i] = 1;     //单位矩阵
        double flag [3][3] = {0,1,-1, -1,0,1, 1,-1,0};
        double sum = P[0] + P[1] + P[2];
        for(int i=0;i<3;i++)
          for(int j=0;j<3;j++)
             if(i==j) num[i][j] = P[i]*P[i] + (1-P[i]*P[i]) * cos(ang);
             else     num[i][j] = P[i]*P[j]*(1-cos(ang))+(sum-P[i]-P[j])*sin(ang)*flag[i][j];
    }
};
matrix operator * (const matrix& a, const matrix& b){  //普通矩阵乘法。注意const
    matrix c(0);
    for(int i=0; i<N; i++)
        for(int j=0; j<N; j++)
            for(int k = 0; k<N; k++)
                c.num[i][j] += a.num[i][k] * b.num[k][j];
    return c;
}
matrix pow_matrix(matrix a, int n){                    //普通矩阵快速幂
    matrix ans(1);
    while(n) {
        if(n&1) ans = ans * a;
        a = a * a;
        n>>=1;
    }
    return ans;
}
matrix dfs(){
    matrix ans(1);
    while(1){
        string cmd; cin >> cmd;
        if(cmd=="end")    return ans;
        if(cmd=="repeat"){
            int k; scanf("%d",&k);
            matrix temp = dfs();
            ans = ans * pow_matrix(temp, k);
        }
        else {
            double x,y,z; scanf("%lf%lf%lf",&x,&y,&z);
            if(cmd == "translate") {matrix temp(x,y,z);   ans=ans*temp;}
            else if(cmd == "scale"){matrix temp(x,y,z,0); ans=ans*temp;}
            else if(cmd == "rotate"){
                double a; scanf("%lf",&a);
                a = a/180*pi;
                double sum = sqrt(x*x + y*y +z*z);
                double p[3] = {x/sum, y/sum, z/sum};
                matrix temp(p,a);
                ans = ans * temp;
            }
        }
    }
}
int main(){
    int n;
    while(~scanf("%d",&n) && n){
        matrix t = dfs();
        while(n--){
            double x,y,z,px,py,pz;   scanf("%lf%lf%lf",&x,&y,&z);
            px = x*t.num[0][0] + y*t.num[1][0] + z*t.num[2][0] + t.num[3][0];
            py = x*t.num[0][1] + y*t.num[1][1] + z*t.num[2][1] + t.num[3][1];
            pz = x*t.num[0][2] + y*t.num[1][2] + z*t.num[2][2] + t.num[3][2];
            printf("%.2f %.2f %.2f\n",px+eps,py+eps,pz+eps);
        }
        puts("");
    }
}
```


### 第9章 字符串

#### 9.1 进制哈希

**9.1.1哈希函数BKDRHash**

例9-1. 【模板】字符串哈希  洛谷 P3370

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ull unsigned long long
ll a[10010];
char s[10010];
ull BKDRHash(char *s){             //哈希函数
    ull P = 131, H = 0;            //P是进制，H是哈希值
    int n = strlen(s);
    for(int i=0;i<n;i++)
        H = H * P + s[i]-'a'+1;    //H = H * P + s[i];  //两种方法
 //上面三行可以简写为一行：
 // while(*s)   H = H*P + (*s++);
    return H;                      //隐含了取模操作，等价于 H % 264
}
int main(){
    int n; scanf("%d",&n);
    for(int i=0;i<n;i++) {
        scanf("%s",s);
        a[i] = BKDRHash(s);   //把字符串s的hash值记录在a[i]中
    }
    int ans = 0;
    sort(a,a+n);
    for(int i=0;i<n;i++)    //统计有多少个不同的hash值
        if(a[i]!=a[i+1])
            ans++;
    printf("%d",ans);
}
```

**9.1.2进制哈希的应用**

例9-2. ANT-Antisymmetry   洛谷P3501

```cpp
//改写自：https://www.luogu.com.cn/blog/xrx/solution-p3501
#include<bits/stdc++.h>
using namespace std;
#define ull unsigned long long
const int N = 5e5+10;
char s[N],t[N];
int n,PP =131;
long long ans; 
ull P[N],f[N],g[N];         //P：计算PP的i次方，f：s的前缀哈希值，g：t的前缀哈希值
void bin_search(int x){     //用二分法寻找以s[x]为中心的回文串
     int L=0,R=min(x,n-x);    
     while(L<R){
         int mid = (L+R+1)>>1;
         if((ull)(f[x]-f[x-mid]*P[mid])==(ull)(g[x+1]-g[x+1+mid]*P[mid]))  L = mid;
         else R = mid-1;
	}
	ans += L;              //最长回文串的长度是L，它内部有L个回文串
}
int main(){
    scanf("%d",&n);   scanf("%s",s+1);
    P[0] = 1;
    for(int i=1;i<=n;i++)  s[i]=='1'? t[i]='0':t[i]='1'; //t是反串
    for(int i=1;i<=n;i++)  P[i] = P[i-1]*PP;             //P[i]=PP的i次方
    for(int i=1;i<=n;i++)  f[i] = f[i-1]*PP + s[i];      //求s所有前缀的哈希值
    for(int i=n;i>=1;i--)  g[i] = g[i+1]*PP + t[i);      //求t所有前缀的哈希值
    for(int i=1;i<n;i++)   bin_search(i); 
    printf("%lld\n",ans);
    return 0;
}
```

例9-3. 最短循环节问题 洛谷P4391

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ull unsigned long long
const int N = 1e6+100;
ull PP = 131;
char s[N];
ull P[N],H[N],n;
ull get_hash(ull L,ull R){return H[R]-H[L-1]*P[R-L+1];}     //区间[L,R]的哈希值
int main(){
    P[0]=1;
    for(int i=1; i<=N-1; i++)   P[i] = P[i-1]*PP;  //预处理PP的i次方
    cin>>n;  scanf("%s",s+1);                      //s[0]不用
    for(ull i=1; i<=n; i++)  H[i] = H[i-1]*PP + s[i]; //预处理所有前缀的hash值
    for(ull i=1; i<=n; i++) {
        ull flag = 1;
        ull last = get_hash(1,i);                 //暴力验证区间[1,i]是否为循环节
        for(int j=1; (j+1)*i<=n; j++) {           //一个区间一个区间地判断
            if(get_hash(j*i+1,(j+1)*i)!=last){    //这一区间是否与第一区间相同
               flag=0;
               break;
            }
        }
        if(n*I != 0){                             //末位多了一小截，单独判断
			ull len = n%i;
			if(get_hash(1,len) != get_hash(n-len+1,n))   flag=0;
		}
		if(flag){ printf("%d\n",(int)i); break;}    //找到了答案，输出然后退出
	}
	return 0;
}
```

#### 9.2 Manacher 

**9.2.3 模板代码**

例9-4.  manacher算法 洛谷P3805

下面是代码。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=11000002;
int n,P[N<<1];        //P[i]: 以s[i]为中心的回文半径
char a[N],s[N<<1];
void change(){        //变换
    n = strlen(a);
    int k = 0;  s[k++]='$'; s[k++]='#';
    for(int i=0;i<n;i++){s[k++]=a[i]; s[k++]='#';}  //在每个字符后面插一个#
    s[k++]='&';                       //首尾不一样，保证第18行的while循环不越界
    n = k;
}
void manacher(){
    int R = 0, C;
    for(int i=1;i<n;i++){
        if(i < R)  P[i] = min(P[(C<<1)-i],P[C]+C-i); //合并处理两种情况
        else       P[i] = 1;
        while(s[i+P[i]] == s[i-P[i]])   P[i]++;      //暴力：中心扩展法            
        if(P[i]+i > R){
            R = P[i]+i;        //更新最大R
            C = i;
        }
    }
}
int main(){
    scanf("%s",a);   change();
    manacher();
    int ans=1;
    for(int i=0;i<n;i++)   ans=max(ans,P[i]);
    printf("%d",ans-1);
    return 0;
}
```

#### 9.3 字典树

**9.3.2 模板代码**

用下面的例题给出字典树的代码。

例9-5. 于是他错误的点名开始了  洛谷 P2580

这一题有两种方法：STL map、字典树。

```cpp
//洛谷 P2580的map实现
#include <bits/stdc++.h>
using namespace std;
int main(){
    map<string,int>student;    string name;
    int n;  cin>>n;
    while(n--){ cin>>name;  student[name]=1; }  //直接把名字当成下标处理
    int m;  cin>>m;
    while(m--){
        cin>>name;
        if(student[name]==1){      puts("OK"); student[name]=2;}
        else if(student[name]==2)  puts("REPEAT");
        else                       puts("WRONG");
    }
    return 0;
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 800000;
struct node{
    bool repeat;    //这个前缀是否重复
    int son[26];    //26个字母
    int num;        //这个前缀出现的次数
}t[N];              //trie
int cnt = 1;        //当前新分配的存储位置。把cnt=0留给根结点
void Insert(char *s){
    int now = 0;
    for(int i=0;s[i];i++){
        int ch=s[i]-'a';
        if(t[now].son[ch]==0)          //如果这个字符还没有存过
            t[now].son[ch] = cnt++;    //把cnt位置分配给这个字符
        now = t[now].son[ch];          //沿着字典树往下走
        t[now].num++;                  //统计这个前缀出现过多少次
    }
}
int Find(char *s){
    int now = 0;
    for(int i=0;s[i];i++){
        int ch = s[i]-'a';
        if(t[now].son[ch]==0) return 3; //第一个字符就找不到
        now = t[now].son[ch];
    }
    if(t[now].num == 0) return 3;       //这个前缀没有出现过
    if(t[now].repeat == false){         //第一次被点名
        t[now].repeat = true;
        return 1;
    }
    return 2;
 // return t[p].num;                    //若有需要，返回以s为前缀的单词的数量
}
int main(){
    char s[51];
    int n;cin>>n;
    while(n--){ scanf("%s",s); Insert(s); }
    int m; scanf("%d",&m);
    while(m--) {
        scanf("%s",s);
        int r = Find(s);
        if(r == 1)   puts("OK");
        if(r == 2)   puts("REPEAT");
        if(r == 3)   puts("WRONG");
    }
    return 0;
}
```

#### 9.4 回文树

**9.4.2 模板代码**

例9-6. Victor and String  hdu 5421

输出：对操作3和操作4，输出结果。

```cpp
//改写自 https://blog.csdn.net/qq_40858062/article/details/103956999
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 3e5+8;
int s[N];
struct node{
     int len,fail,son[26],siz;
     void init(int _len){
        memset(son,0,sizeof(son));
        fail = siz = 0;
        len = _len;
    }
}tree[N];
ll num,last[2],ans,L,R;      //L:在S的头部加字符；R:在S的尾部加字符
void init() {                //初始化一个结点
    last[0]=last[1]=0;       //从0号点开始
    ans=0;   num=1;
    L=1e5+8, R=1e5+7;
    tree[0].init(0);         //小技巧，置0号点len = 0
    memset(s,-1,sizeof(s));
    tree[1].init(-1);        //小技巧，置1号点len = -1
    tree[0].fail=1;          //0指向1，1不必指向0
}
int getfail(int p,int d){    //后缀链跳跃。复杂度可以看成O(1)
    if(d)                    //新字符在尾部
        while(s[R-tree[p].len-1] != s[R])
            p = tree[p].fail;
    else                     //新字符在头部
        while(s[L+tree[p].len+1] != s[L])
            p = tree[p].fail;
    return p;                //返回结点p
}
void Insert(int x,int d){    //往回文树上插入新结点，这个结点表示一个新回文串
    if(d) s[++R] = x;        //新字符x插到S的尾部
    else  s[--L] = x;        //新字符x插到S的头部
    int father = getfail(last[d],d);    //插到一个后缀的子结点上
    int now = tree[father].son[x];
    if(!now){                 //字典树上还没有这个字符，新建一个
       now = ++num;
       tree[now].init(tree[father].len+2);
       tree[now].fail = tree[getfail(tree[father].fail,d)].son[x];
       tree[now].siz = tree[tree[now].fail].siz+1;
       tree[father].son[x] = now;
    }
    last[d] = now;
    if(R-L+1 == tree[now].len)   last[d^1]=now;
    ans += tree[now].siz;
//char ch = x +'a';           //在这里打印回文树，帮助理解
//cout<<" fa="<<father<<",me="<<now<<",char="<<ch;
//cout<<",fail="<<tree[now].fail<<",len="<<tree[now].len<<endl;
}
int main(){
    int op,n;
    while(scanf("%d",&n)!=EOF){
        init();
        while(n--) {
            char c;   scanf("%d",&op);
            if(op==1) scanf(" %c",&c), Insert(c-'a',0);
            if(op==2) scanf(" %c",&c), Insert(c-'a',1);
            if(op==3) printf("%lld\n",num-1);
            if(op==4) printf("%lld\n",ans);
        }
    }
    return 0;
}
```

#### 9.5 KMP

**9.5.3 模板代码和例题**

例9-7.  剪花布条 hdu 2087
	
```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1005;
char str[N], pattern[N];
int Next[N];
int cnt;
void getNext(char *p, int plen){ //计算Next[1]~Next[plen]
    Next[0]=0; Next[1]=0;
    for(int i=1; i < plen; i++){  //把i的增加看成后缀的逐步扩展
        int j = Next[i];          //j的后移：j指向前缀阴影w的后一个字符
        while(j && p[i] != p[j])  //阴影的后一个字符不相同
            j = Next[j];          //更新j
        if(p[i]==p[j])   Next[i+1] = j+1;
        else             Next[i+1] = 0;
    }
}
void kmp(char *s, char *p) {         //在s中找p
    int last = -1;
    int slen=strlen(s), plen=strlen(p);
    getNext(p, plen);               //预计算Next[]数组
    int j=0;
    for(int i=0; i<slen; i++) {     //匹配S和P的每个字符
        while(j && s[i]!=p[j])      //失配了。注意j==0是情况(1)
             j=Next[j];             //j滑动到Next[j]位置
        if(s[i]==p[j])  j++;        //当前位置的字符匹配，继续
        if(j == plen) {             //j到了P的末尾，找到了一个匹配
            //这个匹配，在S中的起点是i+1-plen，末尾是i。如有需要可以打印：
            // printf("at location=%d, %s\n", i+1-plen,&s[i+1-plen]);
            //-------------------30--33行是本题相关
            if( i-last >= plen) {   //判断新的匹配和上一个匹配是否能分开
                cnt++;
                last=i;              //last指向上一次匹配的末尾位置
            }
            //-------------------
        }
    }
}
int main(){
    while(~scanf("%s", str)){      //读串
        if(str[0] == '#')  break;
        scanf("%s", pattern);      //读模式串
        cnt = 0;
        kmp(str, pattern);
        printf("%d\n", cnt);
    }
    return 0;
}
```

#### 9.6 AC自动机


**9.6.2 模板代码**

例9-11.  Keywords Search  hdu 2222

```cpp
//改写自：https://blog.csdn.net/u011815404/article/details/88245190
#include <bits/stdc++.h>
using namespace std;
const int N=1000005;
struct node{
    int son[26];         //26个字母
    int end;             //字符串结尾标记
    int fail;            //失配指针
}t[N];                   //trie[]，字典树
int cnt;                 //当前新分配的存储位置
void Insert(char *s){    //在字典树上插入单词s
    int now = 0;         //字典树上当前匹配到的结点，从root=0开始
    for(int i=0;s[i];i++){     //逐一在字典树上查找s[]的每个字符
        int ch=s[i]-'a';
        if(t[now].son[ch]==0)       //如果这个字符还没有存过
            t[now].son[ch]=cnt++;   //把cnt位置分配给这个字符
        now = t[now].son[ch];       //沿着字典树往下走
    }
    t[now].end++;        //end>0，它是字符串的结尾。end=0不是结尾
}
void getFail(){                 //用BFS构建每个结点的fail指针
    queue<int>q;
    for(int i=0;i<26;i++)       //把第一层入队，即root的子结点
        if(t[0].son[i])         //这个位置有字符
            q.push(t[0].son[i]);
    while(!q.empty()){
        int now = q.front();    //队首的fail指针已求得，下面求它孩子的fail指针
        q.pop();
        for(int i=0;i<26;i++){  //遍历now的所有孩子
            if(t[now].son[i]){  //若这个位置有字符
                t[t[now].son[i]].fail=t[t[now].fail].son[i];
         //这个孩子的Fail=“父结点的Fail指针所指向的结点的与x同字符的子结点”
                q.push(t[now].son[i]); //这个孩子入队，后面再处理它的孩子
            }
            else                //若这个位置无字符
                t[now].son[i]=t[t[now].fail].son[i];//虚拟结点，用于底层的Fail计算
        }
    }
}
int query(char *s){            //在文本串s中找有多少个模式串P
    int ans=0;
    int now=0;                 //从root=0开始找
    for(int i=0;s[i];i++){     //对文本串进行遍历
        int ch = s[i]-'a';
        now = t[now].son[ch];
        int tmp = now;
        while(tmp && t[tmp].end!=-1){  //利用fail指针找出所有匹配的模式串
            ans+=t[tmp].end;    //累加到答案中。若这不是模式串的结尾，end=0
            t[tmp].end = -1;    //以这个字符为结尾的模式串已经统计，后面不再统计
            tmp = t[tmp].fail;  //fail指针跳转
            cout << "tmp="<<tmp<<"  "<<t[tmp].son;
        }
    }
    return ans;
}
char str[N];
int main(){
    int k;
    scanf("%d",&k);
    while(k--){
        memset(t,0,sizeof(t));   //清空，准备一个测试
        cnt = 1;                 //把cnt=0留给root
        int n;    scanf("%d",&n);
        while(n--){scanf("%s",str);Insert(str);} //输入模式串, 插入字典树中
        getFail();              //计算字典树上每个结点的失配指针
        scanf("%s",str);        //输入文本串
        printf("%d\n",query(str));
    }
    return 0;
}
```

#### 9.7 后缀树和后缀数组

**9.7.1 后缀树和后缀数组的概念**

```cpp
#include<bits/stdc++.h>
using namespace std;
int find(string S, string T, int *sa){   //在S中查找子串T；sa是S的后缀数组
    int i=0, j=S.length();
    while(j-i > 1) {
        int k = (i+j)/2;                     //二分法，操作O(logn)次
        if(S.compare(sa[k], T.length(), T)<0)   //匹配一次，复杂度是O(m)
            i = k;
        else j = k;
    }
    if(S.compare(sa[j], T.length(), T) == 0)    //找到了，返回t在s中的位置
        return sa[j];
    if(S.compare(sa[i], T.length(), T) == 0)
        return sa[i];
    return -1;                         //没找到
}
int main(){
    string s="vamamadn", t="ad";       //母串和子串
    int sa[]={5, 3, 1, 6, 4, 2, 7, 0};  //sa是s的后缀数组，假设已经得到了
    int location = find(s, t, sa);
    cout << location <<":"<<&s[location]<< endl<<endl; //打印t在s中的位置
}
```

**9.7.2 倍增法求后缀数组**

```cpp
//计算后缀数组sa[]模板。参考《挑战程序设计竞赛》秋叶拓哉，379页，“4.7.3后缀数组”
#include<bits/stdc++.h>
using namespace std;
const int N = 200005;    // 字符串的长度。
char s[N];               //输入字符串
int sa[N], rk[N], tmp[N+1];
int n, k;
bool comp_sa(int i, int j){    //组合数有两部分，高位是rk[i]，低位是rk[i+k]。
    if(rk[i] != rk[j])         //先比较高位：rk[i]和rk[j]
        return rk[i] < rk[j];
    else{                        //高位相等，再比较低位的rk[i+k]和rk[j+k]
        int ri = i+k <= n? rk[i+k] : -1;
        int rj = j+k <= n? rk[j+k] : -1;
        return ri < rj;
    }
}
void calc_sa( ) {               //计算字符串S的后缀数组
    for(int i = 0; i <= n; i++)    {
        rk[i] = s[i];           //字符串的原始数值
        sa[i] = i;              //后缀数组，在每一步记录当前排序后的结果
    }
    for( k =1; k <= n; k = k*2){   //开始一步步操作，每一步递增2倍进行组合
        sort(sa, sa+n, comp_sa);    //排序，结果记录在sa[]中
        tmp[sa[0]] = 0;
        for(int i = 0; i < n; i++)    //用sa[]倒推组合数，并记录在tmp[]中
           tmp[sa[i+1]] = tmp[sa[i]] + (comp_sa(sa[i],sa[i+1]) ? 1: 0);
        for(int i = 0; i< n; i++)    //把tmp[]拷贝给rk[]，用于下一步操作
            rk[i] = tmp[i];
    }
}
int main(){
    while(scanf("%s",s)!=EOF){        //读字符串
        n=strlen(s);
        calc_sa();                    //求后缀数组sa[]
        for(int i=0;i<n;i++)          //打印后缀数组
            cout<<sa[i]<<" ";
    }
    return 0;
}
```

5. 用基数排序求后缀数组

```cpp
//改编自：《算法竞赛入门经典训练指南》，刘汝佳，陈锋，清华大学出版社，3.4.1节
//main()部分和上面用sort()的版本一样。
char s[N];
int sa[N],cnt[N],t1[N],t2[N],rk[N],height[N];
int n;
void calc_sa() { 
    int m = 127;
    int i,*x=t1,*y=t2;
    for(i=0;i<m;i++)   cnt[i]=0;
    for(i=0;i<n;i++)    cnt[x[i]=s[i]]++;
    for(i=1;i<m;i++)   cnt[i]+=cnt[i-1];
    for(i=n-1;i>=0;i--)  sa[--cnt[x[i]]]=i;
    //sa[]: 从0到n-1
    for(int k=1;k<=n;k=k*2){  //利用对长度为k的排序的结果对长度为2*k的排序
        int p=0;
        //2nd
        for(i=n-k;i<n;i++)  y[p++]=i;
        for(i=0;i<n;i++)    if(sa[i]>=k) y[p++]=sa[i]-k;
        //1st
        for(i=0;i<m;i++)   cnt[i]=0;
        for(i=0;i<n;i++)   cnt[x[y[i]]]++;
        for(i=1;i<m;i++)   cnt[i]+=cnt[i-1];
        for(i=n-1;i>=0;i--)  sa[--cnt[x[y[i]]]]=y[i];
        swap(x,y);
        p=1; x[sa[0]]=0;
        for(i=1;i<n;i++)            
             x[sa[i]] = 
                y[sa[i-1]]==y[sa[i]]&&y[sa[i-1]+k]==y[sa[i]+k]?p-1:p++;
        if(p>=n) break;
        m=p;
    }
}
```

6. 高度数组height[]

```cpp
void getheight(int n){                  //n是字符串长度。
    int i, j, k=0;
    for(i=0 ;i<n; i++)  rk[sa[i]]=i;    //用sa[]推导rk[]
    for(i=0; i<n; i++) {
        if(k)  k--;  
        int j = sa[rk[i]-1];
        while(s[i+k]==s[j+k])  k++;
        height[rk[i]] = k;
    }
}
```

**9.7.3 后缀数组的经典应用**

例9-12.  Longest Common Substring  hdu 1403
	
```cpp
// 省略了calc_sa( )、getheight()函数，已在上一节给出。
int main(){
    int len1, ans;
    while(scanf("%s", s)!=EOF) {      //读第1个字符串
        n = strlen(s);
        len1 = n;
        s[n] = '$';                   //用'$'分隔2个字符串。
        scanf("%s", s+n+1);           //读第2个字符串，与第1个合并。
        n = strlen(s);
        calc_sa();                    //求后缀数组sa[]
        getheight(n);                 //求height[]数组
        ans = 0;
        for(int i = 1; i < n; i++)
//找最大的height[i]，并且它对应的sa[i-1]和sa[i]分别属于前后2个字符串。
            if(height[i]>ans && 
			((sa[i-1]<len1 &&sa[i]>=len1) || (sa[i-1]>=len1&&sa[i]<len1)))
                ans = height[i];
        printf("%d\n",ans);
    }
    return 0;
}
```


#### 9.8 后缀自动机

**9.8.4 模板代码**

例9-13.  Reincarnation  hdu 4622

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 2007;
int sz,last;          //sz: 结点（状态）的编号；last：指向最后被添加的结点
struct node{          //用字典树存结点
    int son[26];      //26个字母
    int father;
    int len;          //这个等价类的最大子串长度
}t[N<<1];                      //后缀自动机的状态数不超过2n个
void newNode(int length){      //建一个新结点。sz=0是根
     t[++sz].len = length;     //这个结点所表示的子串的长度
     t[sz].father = -1;        //它的父结点还未知
     memset(t[sz].son,0,sizeof(t[sz].son));
}
void init(){
     sz = -1; last = 0; //根是0，根指向-1，表示结束
     newNode(0);
}
void Insert(int c){
    newNode(t[last].len+1);
    int p = last, cur = sz;    //p:上个结点的位置，cur:新结点的位置
    while(p!=-1 && !t[p].son[c])
        t[p].son[c] = cur, p = t[p].father;
    if(p==-1)
        t[cur].father = 0;
    else{
        int q = t[p].son[c];
        if(t[q].len == t[p].len + 1)
            t[cur].father = q;
        else{        
           newNode(t[p].len+1);
           int nq = sz;             //复制结点
           memcpy(t[nq].son,t[q].son,sizeof(t[q].son));
           t[nq].father = t[q].father;
           t[cur].father = t[q].father = nq;
           while(p>=0 && t[p].son[c] == q)
               t[p].son[c] = nq,  p = t[p].father;
       }
    }
    last = cur;
/* 打印后缀自动机的所有结点和边
    for(int i=0;i<=sz;i++)
      for(int j=0;j<26;j++)
         if(t[i].son[j]) {  //起点-(边上的字符)-终点
            int start=i,end=t[i].son[j];
            printf("%d-(%c)-%d ",start,j+'a',end);
            printf(" father=%d len=%d\n",t[end].father,t[end].len);
        }
    cout<<endl;
*/
}
char S[N];
int ans[N][N];
int main(){
    int T;    scanf("%d",&T);
    while(T--){
        scanf("%s",S);
        int n = strlen(S);
        for(int i = 0;i < n;i++){
            init();              //每次重新求S[i,j]的后缀自动机
            for(int j = i;j < n;j++){
                Insert(S[j]- 'a');
                ans[i][j] = ans[i][j-1] + t[last].len - t[t[last].father].len;
            }
        }
        int Q, L, R;    scanf("%d",&Q);
        while(Q--){
            scanf("%d%d",&L,&R);
            printf("%d\n",ans[--L][--R]);
        }
    }
    return 0;
}
```

### 第10章 图论

#### 10.1 图的存储

**10.1.3 链式前向星**

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e6+5, M = 2e6+5;           //1百万个点,2百万条边
int head[N],cnt;                          //cnt记录当前存储位置
struct {
    int from, to, next;   //from=边的起点u；to=边的终点v；next = u的下一个邻居
    int w;                //边权，根据题目设定有int，double等类型
}edge[M];                 //存边
void init(){                                    //链式前向星初始化
    for(int i=0; i<N; ++i) head[i] = -1;        //点初始化
    for(int i=0; i<M; ++i) edge[i].next = -1;   //边初始化
    cnt = 0;
}
void addedge(int u, int v, int w){    //前向星存边(u,v)，边的权值为w
   edge[cnt].from = u;                //一般情况下，这一句是多余的。
   edge[cnt].to = v;
   edge[cnt].w = w;
   edge[cnt].next = head[u];
   head[u] = cnt++;
}
int main(){
    init();                            //前向星初始化
    int n, m;  cin>>n>>m;              // 输入n个点，m条边
    for(int i=0;i<m;i++){int u,v,w; cin>>u>>v>>w; addedge(u,v,w);}      //存m条有向边
    for(int i=0;i<=n;i++) printf("h[%d]=%d,",i,head[i]); printf("\n");  //打印head[]
    for(int i=0;i<m;i++) printf("e[%d].to=%d,",i,edge[i].to); printf("\n");    //打印edge[].to
    for(int i=0;i<m;i++) printf("e[%d].nex=%d,",i,edge[i].next); printf("\n"); //打印edge[].next
    for(int i=head[2]; ~i; i=edge[i].next)    //遍历结点2的所有邻居。~i可写为i!=-1
        printf("%d ",edge[i].to);        //printf("%d-%d ",edge[i].from,edge[i].to);
    return 0;
}

```

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e6+5, M = 2e6+5;             //1百万个点,2百万个边
int cnt=0,head[N];                          //cnt等于其他值也行，根据题目要求赋值 
struct {int to, next, w;} edge[M];
void addedge(int u,int v,int w) {
    cnt++;
    edge[cnt].to = v;   
    edge[cnt].w = w;
    edge[cnt].next = head[u];
    head[u] = cnt;
}
int main() {
    int n, m;  cin>>n>>m;    
    for(int i=0;i<m;i++){int u,v,w; cin>>u>>v>>w; addedge(u,v,w);}  
    for(int i=head[2]; i>0; i=edge[i].next)  //遍历结点2的所有邻居
        printf("%d ",edge[i].to);            //输出：5 4 3 1
    return 0;
}
```

#### 10.2 拓扑排序

**10.2.4 输出拓扑排序**

例10-1. Following Orders  Poj 1270
	
```cpp
//改写自 https://blog.csdn.net/iteye_4476/article/details/82335629
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
int n,a[25],dir[30][30];           //dir[i][j]=1表示i、j是先后关系
int topo[25],vis[30],indegree[30]; //topo[]记录拓扑排序，indegree记录度，vis标记是否访问
void dfs(int z,int cnt){
	int i;
	topo[cnt]=z;                  //记录拓扑排序
	if(cnt==n-1) {                //所有点取完了，输出一个拓扑排序
		for(i=0;i<n;i++) printf("%c",topo[i]+'a');
		printf("\n");
		return ;
	}
	vis[z]=1;                     //标记为已访问
	for(i=0;i<n;i++){
		if(!vis[a[i]] && dir[z][a[i]] )
			indegree[a[i]] --;      //把所有下属的度数减一
	}
	for(i=0;i<n;i++)
		if(!indegree[a[i]] && !vis[a[i]] )    //度数为0的继续取。
	     	dfs(a[i],cnt+1);
	for(i=0;i<n;i++){
		if(!vis[a[i]] && dir[z][a[i]] )
			indegree[a[i]] ++;
	}
	vis[z]=0;          //恢复
}
int main(){
	char s[100];
	int len;
	while(gets(s)!=NULL){
		memset(dir,0,sizeof(dir));
		memset(vis,0,sizeof(vis));
		memset(indegree,0,sizeof(indegree));
		len=strlen(s);
		n=0;
		for(int i=0;i<len;i++)     //存字母到a[]
			if(s[i]<='z' && s[i]>= 'a')
				a[n++]= s[i]-'a';
		sort(a,a+n);               //对字母排序，这样就能按字典序输出了
		gets(s);
		len=strlen(s);
		int first=1;               //first = 1表示当前字母是起点
		for(int i=0;i<len;i++) {   //处理先后关系
			int st,ed;
				  if(first && s[i]<='z' && s[i] >='a'){      //起点
					 first=0;
					st=s[i]-'a';         //把字母转化为数字
					continue;
				  }
				  if(!first && s[i]<='z' && s[i] >='a'){     //终点
					first=1;
					ed=s[i]-'a';
					dir[st][ed]=1;        //记录先后关系
					indegree[ed]++;       //记录度数，终点的入度加1
					continue;
				}
		}
		for(int i=0;i<n;i++)
            if(!indegree[a[i]])         //从所有入度为0的点开始
                dfs(a[i],0);
		printf("\n");
	}
	return 0;
}
```

#### 10.3 欧拉路

**10.3.1 欧拉路和欧拉回路的存在性判断**

例10-2. The Necklace  uva 10054  https://vjudge.net/problem/UVA-10054

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 55;
int degree[N];  //记录度
int G[N][N];    //存图
void euler(int u){                   //从u开始DFS
    for(int v = 1; v <= 50; v++)  {  //v是u的邻居
        if(G[u][v]) {
            G[u][v]--;
            G[v][u]--;
            euler(v);
            cout << v << " " << u << endl;    //在euler()后打印，即回溯时打印
        }
    }
}
int main(){
    int t; cin >> t;
    int cnt = 0;
    while (t--) {
        cnt++;
        if(cnt != 1) cout << endl;
        cout << "Case #" << cnt << endl;
        memset(degree, 0, sizeof(degree));
        memset(G, 0, sizeof(G));
        int n;  cin >> n;
        int color;
        for(int i = 0; i < n; i++) {   //输入n条边
            int u, v;  cin>>u>>v;
            color = u;                 //记录一种颜色。测试的时候可能只出现某些颜色
            degree[u]++;
            degree[v]++;               //记录点的度
            G[u][v]++;
            G[v][u]++;                 //存图：=0不连接，=1连接，>1有重边
        }
        int ok = 1;
        for(int i = 1; i <= 50; i++)
            if(degree[i] % 2) {        //存在奇点，无欧拉路
                cout<<"some beads may be lost"<<endl;
                ok = 0;
                break;
        }
        if(ok)  euler(color);         //有欧拉路。随便从某个存在的颜色开始
    }
    return 0;
}
```

**10.3.2 输出一个欧拉回路**

例10-2. Code  poj 1780

```cpp
#include <stdio.h>
const int N = 1e5;
int num[N];                       //num[v]：点v后加的数字，num[v]=0~9
int  st_edge[10*N],     top_s;    //栈，用于存边。top_s指示栈顶
char st_ans [10*N]; int top_a;    //栈，存序列结果。top_a指示栈顶
int m;
void no_dfs(int v){               //模拟递归，递归搜点v的10条边，放进st_edge中
    int edge;                     //边的值
    while(num[v]<10){             //在点v(是一个n-1位序列)后加0~9构成10条边
        edge=10*v + num[v];       //数字edge代表一个边
        num[v]++;                 //点v添的下一个数字。按字典序递增
        st_edge[top_s++] = edge;     //把边存入到栈st_edge中,它是字典序的
            //printf("%02d -> ",v);  //打印边的起点
        v = edge%m;               //更新起点为原来的终点，往下走。点值等于edge的后几位
            //printf("%02d: edge=%03d\n",v,edge); //打印边的终点、边的权值
    }
}
int main(){
    int n, edge;
    while(scanf("%d",&n)&&n!=0){
        top_s = top_a = edge = 0;
        m = 1;
        for(int i=0;i<n-1;++i)  m*=10;     //m是点的数量，共10^(n-1)个点
        for(int i=0;i<m; i++)   num[i]=0;
        no_dfs(0);                         //从起点0开始，递归点0的10条边
        while(top_s){                      //继续走
            edge = st_edge[--top_s];
            st_ans[top_a++] = edge%10+'0'; //只需要存边值的最后一位
            no_dfs(edge/10);               //边值的前n-1位，即上一个点，作用类似DFS的回溯
        }
        for(int i=1;i<n;++i)  printf("0"); //打印第一组数，就是n个0
        while(top_a)  printf("%c",st_ans[--top_a]); //打印其他组数，每组打印1位
        printf("\n");
    }
    return 0;
}
```

#### 10.4 无向图的连通性

例10-3.  network  poj 1144  

```cpp
#include<algorithm>
#include<cstring>
#include<vector>
using namespace std;
const int N=109;
int low[N],num[N],dfn;
bool iscut[N];
vector <int> G[N];
void dfs(int u, int fa){                //u的父结点是fa
	low[u] = num[u] = ++ dfn;           //初始值
	int child = 0;                      //孩子数目
	for(int i = 0;i < G[u].size(); i++)	{     //处理u的所有子结点
		int v = G[u][i];
		if (!num[v]) {                   //v没访问过
			child++;
			dfs(v, u);
			low[u] = min(low[v], low[u]);     //用后代的返回值更新low值
			if (low[v] >= num[u] && u !=1)
			iscut[u] = true;                  //标记割点
		}
		else if(num[v]<num[u] && v!=fa)      //处理回退边
		low[u] = min(low[u], num[v]);
	}
	if (u == 1 && child >= 2)                 //根结点，有两个以上不相连的子树
		iscut[1] = true;
}
int main(){
    int ans,n;
	while(scanf("%d",&n)!=-1){
		if (n==0)  break;
		memset(low,0,sizeof(low));
		memset(num,0,sizeof(num));
		dfn = 0;
		for (int i=0;i<=n;i++) G[i].clear();
		int a,b;
		while (scanf("%d",&a)&&a)
			while (getchar()!='\n'){
				scanf("%d",&b);
				G[a].push_back(b); 	G[b].push_back(a);  //双向边
			}
			memset(iscut,false,sizeof(iscut));
			ans = 0;
			dfs(1,1);
			for (int i=1;i<=n;i++) ans+=iscut[i];
			printf("%d\n",ans);
	}
}
```

**10.4.2 双连通分量**

例10-4.  Road Construction  poj 3352

```cpp
#include<cstring>
#include<vector>
#include<stdio.h>
using namespace std;
const int N = 1005;
int n, m, low[N], dfn;
vector<int> G[N];                  //存图
void dfs(int u,int fa){            //计算每个点的low值
    low[u]= ++dfn;
    for(int i=0;i<G[u].size();i++){
        int v = G[u][i];
        if(v == fa) continue;
        if(!low[v]) dfs(v,u);
        low[u] = min(low[u], low[v]);
    }
}
int tarjan(){
    int degree[N];                  //计算每个缩点的度数
    memset(degree,0,sizeof(degree));
    for(int i=1; i<=n; i++)         //把有相同low值的点看成一个缩点
        for(int j=0; j<G[i].size(); j++)
            if(low[i] != low[G[i][j]])
                degree[low[i]]++;
    int res=0;
    for(int i=1;i<=n;i++)           //统计度数为1的缩点个数
        if(degree[i]==1) res++;
    return res;
}
int main(){
    while(~scanf("%d%d", &n, &m)){
        memset(low, 0, sizeof(low));
        for(int i=0; i<=n; i++)   G[i].clear();
        for(int i=1; i<=m; i++){
            int a, b;  scanf("%d%d", &a, &b);
            G[a].push_back(b);  G[b].push_back(a);
        }
        dfn = 0;
        dfs(1,-1);
        int ans = tarjan();
        printf("%d\n",(ans+1)/2);
    }
    return 0;
}
```

#### 10.5 有向图的连通性

例10-5.  迷宫城堡 hdu 1269

下面给出hdu 1269 的Kosaraju算法代码。

```cpp
//部分代码参考《算法竞赛入门经典（第2版）》，刘汝佳，清华大学出版社，320页。
#include<bits/stdc++.h>
using namespace std;
const int N = 10005;
vector<int> G[N], rG[N];
vector<int> S;                //存第一次dfs1的结果：标记点的先后顺序
int vis[N], sccno[N], cnt;    // cnt：强连通分量的个数
void dfs1(int u) {
    if(vis[u]) return;
    vis[u] = 1;
    for(int i=0; i<G[u].size(); i++)   dfs1(G[u][i]);
    S.push_back(u);           //记录点的先后顺序，标记大的放在S的后面
}
void dfs2(int u) {
    if(sccno[u]) return;
    sccno[u] = cnt;
    for(int i=0; i < rG[u].size(); i++)   dfs2(rG[u][i]);
}
void Kosaraju(int n) {
   cnt = 0;
   S.clear();
   memset(sccno, 0, sizeof(sccno));
   memset(vis, 0, sizeof(vis));
   for(int i = 1; i <= n; i++)  dfs1(i);  //点的编号：1~n。递归所有点
   for(int i = n-1; i >= 0; i--)
       if(!sccno[S[i]]) { cnt++; dfs2(S[i]);}
}
int main(){
    int n, m, u, v;
    while(scanf("%d%d", &n, &m), n != 0 || m != 0) {
        for(int i = 0; i < n; i++) { G[i].clear(); rG[i].clear();}
        for(int i = 0; i < m; i++){
            scanf("%d%d", &u, &v);
            G[u].push_back(v);    //原图
            rG[v].push_back(u);   //反图
        }
        Kosaraju(n);
        printf("%s\n", cnt == 1 ? "Yes" : "No");
    }
    return 0;
}
```

**10.5.2 Tarjan算法**

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 10005;
int cnt;                             // 强连通分量的个数
int low[N], num[N], dfn;
int sccno[N], stack[N], top;         // 用stack[]处理栈，top是栈顶
vector<int> G[N];
void dfs(int u){
    stack[top++] = u;                //u进栈
    low[u]= num[u]= ++dfn;
    for(int i=0; i<G[u].size(); ++i){
        int v = G[u][i];
        if(!num[v]){                 //未访问过的点，继续dfs
            dfs(v);                  //dfs的最底层，是最后一个SCC
            low[u]= min( low[v], low[u] );
        }
        else if(!sccno[v])           //处理回退边
            low[u]= min( low[u], num[v] );
    }
    if(low[u] == num[u]){            //栈底的点是SCC的祖先，它的low = num
        cnt++;
        while(1){
            int v = stack[--top];    //v弹出栈
            sccno[v]= cnt;
            if(u==v) break;          //栈底的点是SCC的祖先
        }
    }
}
void Tarjan(int n){
        cnt = top = dfn = 0;
        memset(sccno,0,sizeof(sccno));
        memset(num,0,sizeof(num));
        memset(low,0,sizeof(low));
        for(int i=1; i<=n; i++)
            if(!num[i])
                dfs(i);
}
int main(){
    int n,m,u,v;
    while(scanf("%d%d", &n, &m), n != 0 || m != 0) {
        for(int i=1; i<=n; i++){ G[i].clear();}
        for(int i=0; i<m; i++){
            scanf("%d%d", &u, &v);
            G[u].push_back(v);
        }
        Tarjan(n);
        printf("%s\n", cnt == 1 ? "Yes" : "No" );
    }
    return 0;
}
```

#### 10.6 基环树

例10-6.  骑士 洛谷P2607 

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e6 + 100;
vector<int> G[N];
int father[N],val[N],mark;
bool vis[N];
ll dp[N][2];
void addedge(int from,int to){
    G[from].push_back(to);          //用邻接表建树
    father[to] = from;              //父子关系
}
void dfs(int u){                    //和洛谷P1352 几乎一样
    dp[u][0] = 0;                   //赋初值：不参加
    dp[u][1] = val[u];              //赋初值：参加
    vis[u] = true;
    for(int v : G[u]){              //遍历u的邻居v
        if(v == mark)  continue;   
        dfs(v);
        dp[u][1] += dp[v][0];       //父结点选择，子结点不选
        dp[u][0] += max(dp[v][0],dp[v][1]);  //父结点不选，子结点可选可不选
    }
}
int check_c(ll u){                  //在基环树中找环上一个点
    vis[u] = true;
    int f = father[u];
    if(vis[f]) return f;            //第2次访问到，是环上一个点
    else       check_c(f);          //继续向父结点方向找
}
ll solve(int u){                    //检查一棵基环树
    ll res = 0;
    mark = check_c(u);              //mark是基环树的环上一个点 
    dfs(mark);                      //做一次dfs
    res = max(res,dp[mark][0]);     //mark不参加
    mark = father[mark];
    dfs(mark);                      //mark的父结点不参加，再做一次dfs
    res = max(res,dp[mark][0]);
    return res;
}
int main(){
    int n; scanf("%d",&n);
    for(int i = 1;i <= n;i++){
        int d;   scanf("%d%d",&val[i],&d);    addedge(d,i);
    }
    ll ans = 0;
    for(int i = 1;i <= n;i++)
        if(!vis[i]) ans += solve(i);  //逐个检查每棵基环树
    printf("%lld\n",ans);
    return 0;
}
```

#### 10.7 2-SAT

例10-7. 2-SAT 问题  洛谷P4782

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e6+10;
int cur, head[N<<1];
struct {int to,next;}edge[N<<2];
void addedge(int u,int v){
	edge[++cur].to = v;
	edge[cur].next = head[u];
	head[u] = cur;
}
int low[N<<1],num[N<<1],st[N<<1],sccno[N<<1],dfn,top,cnt;
int n,m;
void tarjan(int u){
	st[top++] = u;
	low[u] = num[u] = ++dfn;
	for(int i=head[u]; i; i=edge[i].next)	{
		int v=edge[i].to;
		if(!num[v]){
			tarjan(v);
			low[u]=min(low[u],low[v]);
		}else if(!sccno[v])
			low[u]=min(low[u],num[v]);
	}
	if(low[u]==num[u])	{
		cnt++;
		while(1){
            int v=st[--top];
			sccno[v]=cnt;
            if(u==v) break;
		}
	}
}
bool two_SAT(){
	for(int i=1; i<=2*n; i++)
		if(!num[i])
		tarjan(i);                   //tarjan找强连通分量
	for(int i=1; i<=n; i++)
		if(sccno[i]==sccno[i+n])         //a和非a在同一个强连通分量，无解
		return false;
	return true;
}
int main(){
	scanf("%d%d",&n,&m);
	while(m--){
		int a,b,va,vb;	scanf("%d%d%d%d",&a,&va,&b,&vb);
		int nota = va^1, notb = vb^1;     //非a，非b
		addedge(a+nota*n, b+vb*n);        //连边(非a，b)
		addedge(b+notb*n, a+va*n);        //连边(非b，a)
	}
	if(two_SAT()){
		printf("POSSIBLE\n");
		for(int i=1; i<=n; i++) printf("%d ",sccno[i]>sccno[i+n]);
	}
	else printf("IMPOSSIBLE");
	return 0;
}

```

#### 10.8 最短路

**10.8.1 Floyd-Warshall**

用下面的例题给出Floyd的模板代码。

例10-8. 计算最短路  https://www.lanqiao.cn/problems/1121/learning/

```cpp
#include <bits/stdc++.h>
using namespace std;
const long long INF = 0x3f3f3f3f3f3f3f3fLL;  //这样定义的好处是: INF <= INF+x
const int N = 405;
long long dp[N][N];
int n,m,q;
void input(){
// for(int i = 1; i <= n; i++)                    //第1种初始化方法
//     for(int j = 1; j <= n; j++)
//         dp[i][j] = INF;
    memset(dp,0x3f,sizeof(dp));                   //第2种初始化方法
    for(int i = 1; i <= m; i++){
        int u,v; long long w;  cin >> u >> v >> w;
        dp[u][v] = dp[v][u] = min(dp[u][v] , w);  //防止有重边
    }
}
void floyd(){                                //floyd算法
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                dp[i][j] = min(dp[i][j] , dp[i][k] + dp[k][j]);
}
void output(){
    int s, t;
    while(q--){
        cin >> s >>t;
        if(dp[s][t]==INF) cout << "-1" <<endl;
        else if(s==t)     cout << "0"  <<endl;    //floyd()后，dp[i][i]并不等于0
        else              cout << dp[s][t] << endl;
    }
}
int main(){
    cin >> n>> m >> q;
    input();   floyd();   output();
    return 0;
}
```

例10-9. Minimum Transport Cost  hdu 1385

```cpp
#include<bits/stdc++.h>
const int INF = 0x3fffffff;
const int N = 505;
int n, map[N][N], tax[N], path[N][N];
void input(){
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++) {
            scanf("%d", &map[i][j]);
            if(map[i][j] == -1) map[i][j] = INF;
            path[i][j] = j;     //path[i][j]: 此时i、j相邻，或者断开
        }
    for(int i = 1; i <= n; i++)  scanf("%d", &tax[i]);  //交税
}
void floyd(){
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++) {
                int len = map[i][k] + map[k][j] + tax[k];  //计算最短路
                if(map[i][j] > len) {
                    map[i][j] = len;
                    path[i][j] = path[i][k];    //标记到该点的前一个点
                }
                else if(len == map[i][j] && path[i][j] > path[i][k])
                        path[i][j] = path[i][k];     //若距离相同，按字典序
            }
}
void output(){
    int s, t;
    while(scanf("%d %d", &s, &t))    {
        if(s == -1 && t == -1) break;
        printf("From %d to %d :\n", s, t);
        printf("Path: %d", s);
        int k = s;
        while(k != t) {       //输出路径从起点直至终点
            printf("-->%d", path[k][t]);
            k = path[k][t];   //一步一步往终点走
        }
        printf("\n");
        printf("Total cost : %d\n\n", map[s][t]);
    }
}
int main(){
    while(scanf("%d", &n), n){
        input();   floyd();    output();
    }
    return 0;
}
```

例10-11.  跑路  洛谷1613

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 55;
bool p[N][N][34];
int mp[N][N];
int main(){
	memset(mp,0x3f,sizeof(mp));
	int n,m;   cin >> n >> m;
	for( int i = 1;i <= m; i++){
        int u,v;  cin >> u >> v;
		mp[u][v] = 1;
		p[u][v][0] = true;
	}
	for(int t = 1;t <= 32; t++)     //长度为2^t的路径
		for(int k = 1;k <= n; k++)  //Floyd
			for(int i = 1;i <= n; i++)
				for(int j = 1;j <= n; j++)
					if(p[i][k][t - 1] == true && p[k][j][t - 1] == true){
						p[i][j][t] = true;
						mp[i][j] = 1;   //计算得到新图
					}
	for(int k = 1;k <= n; k++)      //求最短路，就用Floyd
		for(int i = 1;i <= n; i++)
			for(int j = 1;j <= n; j++)
				mp[i][j] = min(mp[i][j],mp[i][k] + mp[k][j]);
	cout << mp[1][n] << endl;
	return 0;
}
```

**10.8.2 传递闭包**

2. 用Floyd求解传递闭包

```cpp
//(1)普通写法
for(int k=1; k<=n; k++)         //floyd的3重循环
    for(int i=1; i<=n; i++)
        for(int j=1; j<=n; j++)    
            if(dp[i][k] && dp[k][j];)
               dp[i][j] = 1;   //5-6行可以合并为：dp[i][j] |= dp[i][k] & dp[k][j]; 
//(2)简单优化
for(int k=1; k<=n; k++)         
    for(int i=1; i<=n; i++)
        if(dp[i][k])           //先判断dp[i][k]再进入j循环，计算量略少
           for(int j=1; j<=n; j++)    
               if(dp[k][j])
                  dp[i][j] = 1;  
//(3)用bitset优化，见下面的例题hdu 1704
```

例10-12.  Rank  hdu 1704

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=510;
int n, m;
/* 第二种优化
int d[N][N];
void Floyd(){
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            if(d[i][k])               // 简单优化;
                for(int j = 1; j <= n; j++)
                    if(d[k][j])       // 等价于 if(d[k][j] == 1)  
                       d[i][j] = 1;
}
*/
bitset<N> d[N];   //第三种优化：用bitset加速，能解决N = 1000的问题
void Floyd(){
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            if(d[i][k])
               d[i] |= d[k];           //与11-13行等价
}
int main(){
     int T;    scanf("%d", &T);
     while(T--){
        scanf("%d%d", &n, &m);
        for(int i = 1; i <= n; i++)   //初始化
            for(int j = 1; j <= n; j++)  d[i][j] = (i==j);
        int u, v;
        for(int i = 0; i < m; i++){scanf("%d%d", &u, &v); d[u][v] = 1;}
        Floyd();
        int tot = 0;
        for(int i = 1; i <= n; i++)
            for(int j = i+1; j <= n; j++)
                if(d[i][j] == 0 && d[j][i] == 0) ++tot;
        printf("%d\n", tot);
    }
    return 0;
}
```

**10.8.3 Dijkstra**

（poj 2449）

```cpp
// poj 2449代码
#include <cstdio>
#include <cstring>
#include <queue>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 1005, M = 100005;
struct edge{          //记录边
int to, w;
//vector edge[i]:起点是i;它有很多边,其中一个边的to是边的终点,w是边长
    edge(int a,int b){ to = a, w = b;} //赋值
};
vector <edge>G[M], G2[M];  //G:原图; G2:反图
struct node {      //用于dijkstra。记录点，以及点到起点的路径
    int id, dis;   //id:点；dis：点id到起点的路径长度
    node(int a, int b){ id = a, dis = b;} //赋值
    bool operator < (const node &u) const { return dis > u.dis; }
};
int  dist[N];   //dist[i]: 从s到点i的最短路长度
bool done[N];   //done[i]=ture: 表示到i的最短路已经找到
void dijkstra(int s) {    //标准的dijkstra: 求s到其他所有点的最短路
    for(int i =0;i<N;i++) {dist[i]=INF; done[i]=false;}  //初始化
    dist[s] = 0;          //起点s到自己的距离是0
    priority_queue<node> q;
    q.push(node(s, dist[s]));    //从起点开始处理队列
    while (!q.empty()) {
        node u = q.top();        //pop出距起点s最近的点u
        q.pop();
        if (done[u.id])  continue; //丢弃已经找到最短路的点            
        done[u.id] = true;       //标记：点u到s的最短路已经找到
        for (int i = 0; i< G2[u.id].size(); i++) {  //检查点u的所有邻居
            edge y = G2[u.id][i];
            if (done[y.to])   continue; //丢弃已经找到最短路的邻居                
            if (dist[y.to] > u.dis + y.w) {
                dist[y.to] = u.dis + y.w;
                q.push(node(y.to, dist[y.to]));  //扩展新的邻居，放进优先队列
            }
        }
    }
}
struct point {      //用于 astar
    int v, g, h;    //评估函数 f = g + h, g是从s到i的长度，h是从i到t的长度
    point(int a, int b, int c) { v=a, g=b, h=c; }
    bool operator < (const point & b) const { return g + h > b.g + b.h;}
};
int times[N];     //times[i]: 点i被访问的次数
int astar(int s, int t, int k){
    memset(times, 0, sizeof(times));
    priority_queue<point> q;
    q.push(point(s, 0, 0));
    while (!q.empty()) {
        point p = q.top();   //从优先队列中弹出f = g + h最小的
        q.pop();
        times[p.v]++;
        if (times[p.v] == k && p.v == t)  //从队列中第k次弹出t，就是答案
            return p.g + p.h;
        for (int i = 0; i< G[p.v].size(); i++) {
            edge y = G[p.v][i];
            q.push(point(y.to, p.g + y.w, dist[y.to]));
        }
    }
    return -1;
}
int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    while (m--) {
        int a, b, w;             //读边：起点、终点、边长
        scanf("%d%d%d", &a, &b, &w);  //本题是有向图
         G[a].push_back(edge(b,w));  //原图
        G2[b].push_back(edge(a,w));  //反图
    }
    int s, t, k;
    scanf("%d%d%d", &s, &t, &k);
    if (s == t)  k++;         //一个小陷阱
    dijkstra(t);              //在反图G2上，求终点t到其他点的最短路
    printf("%d\n", astar(s, t, k));  //在原图G上，求第k短路
    return 0;
}
```

**10.8.4 Bellman-ford**
 
例10-14.  最短路  hdu 2544

```cpp
#include<bits/stdc++.h>
using namespace std;
const int INF = 1e6;
const int N = 105;
struct Edge { int u, v, w; } e[10005];       //边：起点u，终点v，权值w
int n, m, cnt;
int pre[N];   //记录前驱结点，用于打印路径。pre[x]=y：在最短路径上，结点x的前一个结点是y
void print_path(int s, int t) {              //打印从s到t的最短路
    if(s==t){ printf("%d ", s); return; }    //打印起点
    print_path(s, pre[t]);                   //先打印前一个点
    printf("%d ", t);                        //后打印当前点。最后打印的是终点t
}
void bellman(){
      int s=1;         //定义起点
      int d[N];        //d[i]记录第i个结点到起点s的最短距离
      for (int i=1; i<=n; i++)  d[i]=INF;    //初始化为无穷大
      d[s]=0;
      for (int k=1; k<=n; k++)               //一共有n轮操作
            for (int i=0; i < cnt; i++){     //检查每条边
                int x = e[i].u,  y = e[i].v;
                if (d[x] > d[y] + e[i].w){   // x通过y到达起点s：如果距离更短，更新
                    d[x] = d[y] + e[i].w;
                    pre[x] = y;              //如果有需要，记录路径
                }
            }
        printf("%d\n", d[n]);
        // print_path(s,n);                  //如果有需要，打印路径
}
int main() {
    while(~scanf("%d%d", &n, &m)) {
        if(n==0 && m==0) return 0;
        cnt = 0;        //记录边的数量。本题的边是双向的，共有cnt=2*m条
        while (m--) {
            int a,b,c;   scanf("%d%d%d",&a,&b,&c);
            e[cnt].u=a;  e[cnt].v=b;  e[cnt].w=c;  cnt++;
            e[cnt].u=b;  e[cnt].v=a;  e[cnt].w=c;  cnt++;
        }
        bellman();
    }
    return 0;
}
```

**10.8.5 SPFA**

仍然用“例10-14 hdu 2544”给出SPFA的模板代码。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 1e6+5, M = 2e6+5;            //1百万个点,2百万个边
int n, m;
int pre[N];                                //记录前驱结点，用于打印路径
void print_path(int s, int t) {            //打印从s到t的最短路
    if(s==t){ printf("%d ", s); return; }  //打印起点
    print_path(s, pre[t]);                 //先打印前一个点
    printf("%d ", t);                      //后打印当前点。最后打印的是终点t
}
int head[N],cnt;                          //链式前向星
struct {int to, next, w;}edge[M];         //存边
void init(){                               //链式前向星初始化
    for(int i = 0; i < N; ++i)   head[i] = -1;        //点初始化
    for(int i = 0; i < M; ++i)   edge[i].next = -1;   //边初始化
    cnt = 0;
}
void addedge(int u, int v, int w){      //前向星存边
   edge[cnt].to=v; edge[cnt].w=w; edge[cnt].next=head[u]; head[u]=cnt++;
}
int dis[N];            //dis[i]，从起点到点i的距离
bool inq[N];           //inq[i] = true 表示点i在队列中
int Neg[N];            //判断负环(Negative loop)
int spfa(int s) {      //返回1表示出现负环
    memset(Neg, 0, sizeof(Neg));
    Neg[s] = 1;
    for(int i=1; i<=n; i++) {dis[i]=INF;  inq[i]=false; }   //初始化
    dis[s] = 0;                    //起点到自己的距离是0
    queue<int> Q;     Q.push(s);   //从s开始，s进队列
    inq[s] = true;                 //起点在队列中
    while(!Q.empty()) {
        int u = Q.front(); Q.pop();     //队头出队
        inq[u] = false;                 //u已经不在队列中
        for(int i=head[u]; ~i; i = edge[i].next) {  //~i也可以写成 i!=-1
            int v = edge[i].to, w = edge[i].w;      //v是u的第i个邻居
            if (dis[u]+w < dis[v]) {    //u的第i个邻居v，它借道u，到s更近
                dis[v] = dis[u]+w;      //更新邻居v到s的距离
                pre[v] = u;             //如果有需要，记录路径
                if(!inq[v]) {           //邻居v更新状态了，但v不在队列中，放进队列
                    inq[v] = true;
                    Q.push(v);
                    Neg[v]++;                   //点v进入队列的次数
                    if(Neg[v] > n) return 1;    //出现负环
                }
            }
        }
    }
    return 0;
}
int main() {
    while(~scanf("%d%d",&n,&m)) {
        init();      //前向星初始化
        if(n==0 && m==0) return 0;
        while(m--){
            int u,v,w; scanf("%d%d%d",&u,&v,&w); 
            addedge(u,v,w); addedge(v,u,w);  //双向边
        }
        spfa(1);  //计算起点1到其他所有点的最短路
        printf("%d\n",dis[n]);  //打印从1到n的最短距离
      //printf("path:"); print_path(1,n); printf("\n"); //如有需要，打印从s到t的路径
    }
    return 0;
}
```

例10-15.  Sightseeing Cows 洛谷P2868，Poj 3621

```cpp
//洛谷P2868，Poj 3621的部分代码
#include<bits/stdc++.h>
using namespace std;
const int N = 1e4+5, M = 5e4+5;
double dis[N]; //距离
//下面的定义与上题hdu 2544的代码几乎一样，唯一的区别是把int w改为double w
//bool inq[N]; int Neg[N]; head[N],cnt; edge[M];init(); addedge(); spfa();
int f[N];                      //乐趣值
int u[M],v[M],w[M];            //记录边
bool check(double x){
    init();                    //前向星初始化
    for(int i=1;i<=m;++i)  addedge(u[i],v[i],x * w[i] - f[u[i]]);  //修改边的权值
    return spfa(1);            //若有负环，从任意一个点出发都会遇到负环，这里选从1出发
}
int main(){
    cin>>n>>m;
    for(int i = 1;i <= n;++i) cin >> f[i];   //注意i从1开始，因为结点编号是1~n
    for(int i = 1;i <= m;++i) cin >> u[i] >> v[i]>>w[i];
    double L = 0,R = 0;
    for (int i = 1; i <= n; i++)  R += f[i];  //R的初值
    for (int i = 0; i < 30; i++) { //实数二分。如果大于30，在poj提交会超时
        double mid = L+(R-L)/2;
        if(check(mid))  L = mid;  //F < 0,放大
        else R = mid;             //F >= 0,缩小
    }
    printf("%.2f", L);
    return 0;
}
```

#### 10.9 最小生成树

**10.9.1 Kruskal算法**

例10-17.  最小生成树 洛谷 P3366

代码：

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 5005,M = 2e5+1;
struct Edge{int u,v,w;}edge[M];       //用最简单且最省空间的结构体数组存边
bool cmp(Edge a, Edge b){ return a.w < b.w;}       //从小到大排序
int s[N];                             //并查集
int find_set(int x){                  //查询并查集，返回x的根
    if(x != s[x])
       s[x] = find_set(s[x]);         //路径压缩
    return s[x];
}
int n,m;                              // n个点，m条边
void kruskal(){
    sort(edge+1, edge+m+1, cmp);      //对边做排序
    for(int i=1; i<=n; i++) s[i]=i;   //并查集初始化
    int ans = 0, cnt=0;               //cnt: 已经加入MST的边数
    for(int i=1; i<=m; i++){          //贪心：逐一加入每条边
        if(cnt == n-1)    break;      //小优化：不要也行
        int e1 = find_set(edge[i].u); //边的前端点u属于哪个集？
        int e2 = find_set(edge[i].v); //边的后端点v属于哪个集？
        if(e1 == e2) continue;        //属于同一个集：产生了圈，丢弃
        else{                         //不属于同一个集
		   ans += edge[i].w;         //计算MST
		   s[e1]= e2;                //合并
		   cnt++;                    //统计MST中的边数
		}
	}
	if(cnt == n-1) cout << ans;       //n-1条边
	else cout << "orz";               //图不是连通的
}
int main(){
	cin >> n >> m;
	for(int i=1; i<=m; i++)  cin >> edge[i].u >> edge[i].v >> edge[i].w;
	kruskal();
	return 0;
}
```

**10.9.2 Prim算法**

下面是“例10-17. 洛谷 P3366”的代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=5005,M = 2e5+1;
struct edge{                              //记录边
int to, w;  
edge(int a,int b){ to = a, w = b;}    //赋值
};
vector <edge>G[M];
struct node {
    int id, dis;   //id:点；dis：边
    node(int a, int b){ id = a, dis = b;}  //赋值
    bool operator < (const node &u) const { return dis > u.dis; }
};
int n, m;
bool done[N];                //done[i]=ture: 表示点i已经在MST中
void  prim() {               //对比dijkstra: 求s到其他所有点的最短路
    int s = 1;               //从任意点开始，例如从1开始
    for(int i =1;i<=N;i++)   done[i]=false;   //初始化
    priority_queue<node> q;
    q.push(node(s, 0));      //从s点开始处理队列
    int ans=0,cnt=0;
    while (!q.empty()) {
        node u = q.top();   q.pop();    //pop出距集合U最近的点u
        if (done[u.id])     continue;   //丢弃已经在MST中的点，有判圈的作用
        done[u.id] = true;              //标记
        ans += u.dis;
        cnt++;                          //统计点数
        for (int i = 0; i< G[u.id].size(); i++) {    //检查点u的所有邻居
            edge y = G[u.id][i];        //一个邻居y
            if (done[y.to])   continue; //丢弃已经在MST中的点
            q.push(node(y.to, y.w));    //扩展新的邻居，放进优先队列
        }
    }
    if(cnt == n) cout << ans;           //cnt=n个点。注意在kruskal代码中cnt是边数
    else cout << "orz";
}
int main() {
    cin>>n>>m;
    for(int i=1; i<=m; i++) {
        int a,b,w;   cin>>a>>b>>w;
        G[a].push_back(edge(b,w));  G[b].push_back(edge(a,w));  //双向边
    }
    prim();
    return 0;
}
```

#### 10.10 最大流

例10-19.  网络最大流 洛谷 P3376

**10.10.2 Edmonds-Karp算法**

下面是洛谷3376的代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int INF = 1e9;
const int N = 250;
#define ll long long
int n, m;
ll graph[N][N], pre[N];                  //graph[][]不仅记录图，还是残留网络
ll flow[N];
ll bfs(int s,int t){                     //一次bfs找一条增广路
    memset(pre,-1,sizeof pre);
    flow[s] = INF;  pre[s] = 0;          //初始化起点
    queue<int> Q;  Q.push(s);            //起点入栈，开始bfs
    while(!Q.empty()){
        int u = Q.front();  Q.pop();
        if(u==t) break;                  //搜到一个路径，这次bfs结束
        for(int i=1; i<=n; i++){         //bfs所有的点
            if(i!=s && graph[u][i]>0 && pre[i]==-1){
                pre[i] = u;  		     //记录路径
                Q.push(i);
                flow[i] = min(flow[u], graph[u][i]); //更新结点流量
            }
        }
    }
    if(pre[t]==-1) return -1;            //没有找到新的增广路
    return flow[t];                      //返回这个增广路的流量
}
ll maxflow(int s, int t){
    ll Maxflow = 0;
    while(1){
        ll flow = bfs(s,t);              //执行一次bfs，找到一条路径，返回路径的流量
        if(flow == -1) break;            //没有找到新的增广路，结束
        int cur = t;                     //更新路径上的残留网络
        while(cur!=s){                   //一直沿路径回溯到起点
            int father = pre[cur];       //pre[]记录路径上的前一个点
            graph[father][cur] -= flow;  //更新残留网络：正向边减
            graph[cur][father] += flow;  //更新残留网络：反向边加
            cur = father;
        }
        Maxflow += flow;
    }
    return Maxflow;
}
int main(){
    int s,t; scanf("%d%d%d%d",&n,&m,&s,&t);
    memset(graph,0,sizeof graph);
    for(int i=0; i<m; i++){
        int u,v,w;  scanf("%d%d%d",&u,&v,&w);
        graph[u][v] += w;                //可能有重边
    }
    printf("%ld\n",maxflow(s,t));
    return 0;
}
```

**10.10.3 Dinic算法**

下面重新用Dinic算法实现洛谷P3376。

```cpp
//代码改写自：www.luogu.com.cn/blog/Eleven-Qian-ssty/solution-p3376
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll INF=1e9;
int n,m,s,t;
const int N=250, M=11000;              //M定义为边的两倍：双向边
int cnt=1,head[N];                     //8-16行：链式前向星。cnt初值不能是0，可以是1、3、5
struct {int to, nex, w;} e[M];
void add(int u,int v,int w) {
    cnt++;                  //cnt初值是1，cnt++后第一个存储位置是cnt=2，是偶数位置
    e[cnt].to = v;
    e[cnt].w = w;
    e[cnt].nex = head[u];
    head[u] = cnt;
}
int now[N],dep[N];                     //dep[]记录点所在的层次(深度)
int bfs() {                            //在残留网络中构造分层图
    for(int i=1;i<=n;i++) dep[i]=INF;
    dep[s] = 0;                        //从起点s开始分层
    now[s] = head[s];                  //当前弧优化。now是head的拷贝
    queue<int>Q;  Q.push(s);
    while(!Q.empty()) {
        int u = Q.front();   Q.pop();
        for(int i=head[u]; i>0;i=e[i].nex) {   //搜点u的所有邻居，邻居是下一层
            int v = e[i].to;
            if(e[i].w>0 && dep[v]==INF) {      // e[i].w>0表示还有容量
                Q.push(v);
                now[v] = head[v];
                dep[v] = dep[u]+1;             //分层：u的邻居v是u的下一层
                if(v==t) return 1;             //搜到了终点，返回1
            }
        }
    }
    return 0;   //如果通过有剩余容量的边无法到达终点t，即t不在残留网络中，返回0
}
int dfs(int u,ll sum) {                        //sum是这条增广路对最大流的贡献
    if(u==t) return sum;
    ll k,flow=0;                               //k是当前最小的剩余容量
    for(int i=now[u]; i>0 && sum>0; i=e[i].nex) {
        now[u]=i;                              //当前弧优化
        int v=e[i].to;
        if(e[i].w>0 && (dep[v]==dep[u]+1)){   //分层：用dep限制只能访问下一层
            k = dfs(v,min(sum,(ll)e[i].w));
            if(k==0) dep[v] = INF;   //剪枝，去掉增广完毕的点。其实把INF写成0也行
            e[i].w -= k;             //更新残留网络：正向减
            e[i^1].w += k;           //更新残留网络：反向加。小技巧：奇偶边
            flow += k;               //flow表示经过该点的所有流量和
            sum -= k;                //sum表示经过该点的剩余流量
        }
    }
    return flow;
}
int main() {
    scanf("%d%d%d%d",&n,&m,&s,&t);
    for(int i=1;i<=m;i++) {
        int u,v,w;   scanf("%d%d%lld",&u,&v,&w);
        add(u,v,w);  add(v,u,0);      //双向边，反向边的容量是0
    }
    ll ans=0;
    while(bfs()) ans += dfs(s,INF);   //先后做BFS和DFS。当t不在残留网络中时退出
    printf("%lld",ans);
    return 0;
}
```

**10.10.4 ISAP算法**

下面用ISAP重新实现洛谷P3376。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll INF=1e9;
int n,m,s,t ;
const int N=250, M=11000;              //M定义为边的两倍：双向边
int cnt=1,head[N];                     //链式前向星
struct{int from, to, nex, w;} e[M];
void add(int u, int v, int w){
     cnt++; 
     e[cnt].from = u;
     e[cnt].to = v;
     e[cnt].w = w;
     e[cnt].nex = head[u];
     head[u] = cnt;
}
int now[M], pre[M]; //pre[]用于记录路径，pre[i]是路径上点i(的存储位置)的前一个点(的存储位置)
int dep[M], gap[M]; //dep[i]: 点i的高度；gap[i]:高度为i的点的数量
void bfs(){                         //用BFS确定各顶点到汇点的距离
     memset(gap,0,sizeof(gap));     //初始化间隙数组
     memset(dep,0,sizeof (dep));    //所有点的高度初始为0
     dep[t] = 1;                    //汇点t的高度是1，其他点的高度都大于1
     queue<int> Q;    Q.push(t); 
     while(!Q.empty()){
         int u = Q.front();    Q.pop();
         gap[dep[u]]++;             //间隙数组：计算高度为dep[u]的结点个数
         for(int i=head[u]; i>0; i=e[i].nex){ 
             int v = e[i].to;       //v是u的邻居点
             if(e[i^1].w && dep[v]==0){   //反向边不用处理；高度不等于0的已经处理过了
			   dep[v] = dep[u]+1; 
			   Q.push(v);
			}
		}
	}
}
ll Augment(){               //沿着增广路径更新残留网络
	ll v = t, flow = INF;
	while(v != s){         //找这个路径的流量
	   int u = pre[v];     //u是v向源点s方向回退的上一个点，相当于DFS的回溯 
	   if(e[u].w < flow)  flow = e[u].w;      //路径上的最小容量就是这个路径的流量
	   v = e[u].from;      //更新v，继续回退
	}
	v = t;
	while(v != s){         //更新残留网络
	    int u=pre[v];      //向源点s方向回退
	    e[u].w -= flow;    //正向边
	    e[u^1].w += flow;  //反向边。用到了奇偶边的技巧
	    v = e[u].from;     //更新v，继续回退
	}
	return flow;           //返回这个路径的流量
}
void ISAP(){     
     bfs();                //用bfs()求每个点到汇点的距离（高度）
     ll flow = 0;          //计算最大流 
     int u = s;	        //从源点s开始找增广路径
     memcpy(now, head, sizeof (head));  //当前弧优化。now是head的拷贝
     while(dep[s] <= n){                //最大距离（高度）是n 
         if(u == t){                    //找到了一条增广路径
			flow += Augment();        //更新残留网络，更新流量
			u = s;                    //回到s，重新开始找一条增广路径
		}
		bool ok = 0;                   //用于判断是否能顺利往下走
		for(int i=now[u]; i; i=e[i].nex){   //在u的邻居中确定路径的下一个点
		    int v = e[i].to;                //v是u的邻居点
		    if(e[i].w && dep[v]+1==dep[u]){ //沿着高度递减的方向找下一个点
				ok = 1;              //顺利找到了路径的下一个点
				pre[v] = i;          //记录路径
				now[u] = i;          //记录当前弧，下一次跳过它
				u = v;               //u更新为路径的下一个点v
				break;               //退出for，回到while循环，继续找路径的下一个点
			}
		}
		if(!ok){        //路径走不下去了，需要更新u的高度重新走
		     if(!--gap[dep[u]]) break;  //u的下一个深度的点没有了，断层了，退出while
		     int mindep = n+10;         //mindep用于计算u的邻居点的最小高度。初值比n大就行
		     for(int i=head[u]; i; i=e[i].nex){   //在u的邻居中找最小的高度
		         int v = e[i].to;
		         if(dep[v]<mindep && e[i].w)
					mindep = dep[v];
			}
			dep[u] = mindep+1; //更新u的高度，改为比v大1，从而能够生成一条路径，继续往后走
			gap[dep[u]]++;            //更新间隙数组：高度为dep[u]的结点个数加1
			now[u] = head[u];         //记录当前弧
			if(u != s) u = e[pre[u]].from;     //回退一步，相当于DFS的回溯
		}
	}
	printf("%ld", flow);               //打印最大流
}
int main(){
	scanf("%d%d%d%d", &n,&m,&s,&t);
	for(int i=1; i<=m; ++i){
		int u,v,w;   scanf("%d%d%d",&u,&v,&w);
		add(u,v,w);  add(v,u,0);         //反向边的初始容量是0
	}
	ISAP();
	return 0;
}

```

#### 10.11 二分图

例10-20.  二分图最大匹配 洛谷P3386（hdu 2063 过山车）

3. 用匈牙利算法求解二分图最大匹配

```cpp
#include<bits/stdc++.h>
using namespace std;
int G[510][510];
int match[510], reserve_boy[510];     //匹配结果在match[]中
int n, m;
bool dfs(int x){                      //找一个增广路径，即给女孩x找一个配对男孩
     for(int i=1; i<=m; i++)
         if(!reserve_boy[i] && G[x][i]){
             reserve_boy[i] = 1;      // 预定男孩i，准备分给女孩x
             if(!match[i] || dfs(match[i])){
    //有两种情况：(1)如果男孩i还没配对，就分给女孩x；
    //            (2)如果男孩i已经配对，尝试用dfs()更换原配女孩，以腾出位置给女孩x
                match[i] = x; //配对成功；如果原来有配对，更换成功。现在男孩i属于女孩x
                return true;
            }
         }
     return false;      //女孩x没有喜欢的男孩，或者更换不成功
}
int main(){
    int e; scanf("%d%d%d",&n,&m,&e);
    while(e--){int a,b; scanf("%d%d",&a,&b); G[a][b]=1;}   //矩阵存图
    int sum=0;
    for(int i=1; i<=n; i++){         //为每个女孩找配对
        memset(reserve_boy,0,sizeof(reserve_boy));
        if(dfs(i))  sum++; //第i个女孩配对成功，这个配对后面可能更换，但是保证她能配对
    }
    printf("%d\n",sum);
    return 0;
}
```

#### 10.13 费用流

例10-21. Farm Tour  poj 2135

```cpp
//poj 2135 , 邻接表存图 + SPFA + 最大流
#include <stdio.h>
#include <algorithm>
#include <cstring>
#include <queue>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 1010;
int dis[N], pre[N], preve[N]; //dis[i]记录起点到i的最短距离。pre和 preve见下面注释
int n, m;
struct edge{
    int to, cost, capacity, rev;        //rev用于记录前驱点
    edge(int to_,int cost_,int c,int rev_){
        to=to_; cost=cost_; capacity=c; rev=rev_;}
};
vector<edge> e[N];           //e[i]：存第i个结点连接的所有的边
void addedge(int from,int to,int cost,int capacity){//把1个有向边再分为2个
    e[from].push_back(edge(to, cost, capacity, e[to].size()));
    e[to].push_back(edge(from, -cost, 0, e[from].size()-1));
}
bool spfa(int s, int t, int cnt){          //套SPFA模板
    bool inq[N];
    memset(pre, -1, sizeof(pre));
    for(int i = 1; i <= cnt; ++i) {dis[i]=INF; inq[i]=false; }
    dis[s] = 0;
    queue <int> Q;
    Q.push(s);
    inq[s] = true;
    while(!Q.empty()){
        int u = Q.front();
        Q.pop();
        inq[u] = false;
        for(int i=0; i < e[u].size(); i++)
            if(e[u][i].capacity > 0){
                int v = e[u][i].to, cost = e[u][i].cost;
                if(dis[u]+cost < dis[v]){
                    dis[v] = dis[u]+cost;
                    pre[v] = u;        //v的前驱点是u
                    preve[v] = i;      // u的第i个边连接v点
                    if(!inq[v]){
                        inq[v] = true;
                        Q.push(v);
                    }
                }
            }
    }
    return dis[t] != INF;             //s到t的最短距离（或者最小费用）是dis[t]
}
int mincost(int s,int t,int cnt){     //基本上是套最大流模板
    int cost = 0;
    while(spfa(s,t,cnt)){
        int v = t, flow = INF;        //每次增加的流量
        while(pre[v] != -1){          //回溯整个路径，计算路径的流
            int u = pre[v], i = preve[v];         //u是v的前驱点，u的第i个边连接v
            flow = min(flow, e[u][i].capacity);   //所有边的最小容量就是这条路的流
            v = u;                                //回溯，直到源点
        }
        v = t;
        while(pre[v] != -1){                      //更新残留网络
            int u = pre[v], i = preve[v];
            e[u][i].capacity -= flow;             //正向减
            e[v][e[u][i].rev].capacity += flow;   //反向加，注意rev的作用
            v = u;                                //回溯，直到源点
        }
        cost += dis[t]*flow;    //费用累加。如果程序需要输出最大流，在这里累加flow
    }
    return cost;                //返回总费用
}
int main(){
    while(~scanf("%d%d", &n, &m)){
        for(int i=0;i<N; i++)  e[i].clear();     //清空 
        for(int i=1;i<=m;i++){
            int u,v,w; scanf("%d%d%d",&u,&v,&w);
            addedge(u,v,w,1); addedge(v,u,w,1);  //把1个无向边分为2个有向边            
        }
        int s = n+1, t = n+2;
        addedge(s,1,0,2);           //添加源点
        addedge(n,t,0,2);           //添加汇点
        printf("%d\n", mincost(s,t,n+2));
    }
    return 0;
}
```
