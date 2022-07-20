
**AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】**

https://www.acwing.com/solution/content/56348/

挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优     
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1    

https://www.bilibili.com/video/BV1KE411V79h     




- AcWing291 蒙德里安的梦想-

https://www.acwing.com/solution/content/28088/

DP的过程随着阶段的增长，在各个状态维度不断扩张，在任意时刻，已经求出最优解的状态与尚未求出最优解的状态在各个维度上的分界点组成了DP扩展的轮廓。     
对于某些问题，需要在动态规划的状态中记录一个集合，保存轮廓的详细信息，便于进行状态转移。     
若集合大小不超过N,集合中每个元素都是小于K的自然数，则可以把这个集合看成N位K进制数，以一个[0,k^N-1]之间的十进制整数的形式作为DP一维。     
这种把集合转化为整数记录在DP状态中的一类算法，叫做状态压缩DP。     

F[i][j]表示第i行的形态为j时，前i行分割方案的总数。j是十进制表示的M位二进制数，第k位为1表示第k列是一个竖着的1*2长方形的上面一半，第k位为0是其他情况。    
转移方程：F[i,j] = Σ F[i-1,k], j&k = 0，j|k 有偶数个0。

一般都需要预处理

状态压缩DP模板更新

```cpp
const int N = 12, M = 1 << N;
LL f[N][M];
bool st[M]; //是否有偶数个连续的0,有则为true
vector<int> state[M]; //预存储合法状态

//预处理1 判断i是否有偶数个连续的0
for (int i = 0; i < (1 << n); i++) {
	bool isvalid = true; // 是否有偶数个连续的0，有则有效。
	int cnt = 0;
	for (int j = 0; j < n; j++) {
		if (i >> j & 1) {
			if (cnt & 1) {
				isvalid = false;
				break;
			}
			cnt = 0; //开始下一段
		} else cnt++;
	}
	if (cnt & 1) isvalid = false;
	st[i] = isvalid;
}

//预处理2 判断i-2列和i-1列是否冲突
for (int i = 0; i < 1 << n; i++) {
	state[i].clear();
	for (int k = 0; k < 1 << n; k++) {
		if ((i & k) == 0 && st[i | k]) {
			state[i].emplace_back(k);
		}
	}
}

//dp开始
memset(f, 0, sizeof(f));
f[0][0] = 1;
for (int i = 1; i <= m; i++) {
	for (int j = 0; j < (1 << n); j++) {
		for (auto k: state[j]) {    //遍历合法的转移
			f[i][j] += f[i - 1][k]; //方案数
		}
	}
}

```

vector<int> state[M]：vector和数组嵌套使用。

- AcWing292 炮兵阵地

https://www.acwing.com/solution/content/12392/ 

S集合:相邻两个1的距离不小于3的所有M位二进制数,代表两个炮兵的距离不能小于3。     
count:M位2进制中1的个数。     
valid(i,x):M位2进制数x属于S,且x中的每个1对应地图中的第i行中的位置都是平原,则能摆下炮兵。  
  
压缩存储两层的信息，然后枚举合法的i-2层状态进行转移。  

状态表示：f[i][j][k]:第i层状态是j, i-1层状态是k,该方案前i行能摆下多少个炮兵。     
状态计算：f[i][j][k] = max f[i-1][k][pre] + cnt[j], valid(i,j) valid(i-1,k)，并且j&k = 0。     
pre是枚举的能够与k和j合法存在于三行中的所有状态.      


```cpp
bool check(int s){
    for(int i = 0;i < m;i++){
        if ((s >> i & 1) && ((s >> i + 1 & 1) || (s >> i + 2 & 1))) {
            return false;
        }
    }
    return true;
}

int count(int s) {
    int res = 0;
    while (s) {
        res += s & 1;
        s >>= 1;
    }
    return res;
}


//预处理合法状态,连续三位不能都是1(1表示放一个炮兵)
for (int i = 0; i < 1 << m; i++) {
	if (check(i)) {
		state.emplace_back(i);
		cnt[i] = count(i); // 1的个数，即可以放炮兵的数量
	}
}

//状态计算
// i状态j,i-1状态是k,i-2状态是u.
for (int i = 0; i < n + 2; i++) {
	for (int j = 0; j < state.size(); j++) {
		for (int k = 0; k < state.size(); k++) {
			for (int u = 0; u < state.size(); u++) {
				int a = state[u], b = state[k], c = state[j];
				if (a & b || b & c || a & c) continue;
				if (g[i] & c) continue;
				f[i & 1][j][k] = max(f[i & 1][j][k], f[i - 1 & 1][k][u] + cnt[c]);
			}
		}
	}
}
cout << f[n + 1 & 1][0][0] << endl;
```


- AcWing298 围栏

https://www.acwing.com/solution/content/2130/
				  
单调队列  

单调队列优化适合决策取值范围的上下边界均单调编号，每个决策在候选集合中插入或删除至多一次的情况。     
f[i][j] 表示安排前i个工匠粉刷前j块木板(木板可以空着不刷)，能获得的最大报酬。         
f[i][j] = max {f[i-1][k] + Pi(j-k)} ， j - Li <= k <= Si - 1          
在考虑内层循环j和决策k时，把外层循环变量i看作定值。     
f[i][j] = Pi*j + max{f[i-1][k] - Pi*k} ， j - Li <= k <= Si - 1     

求max用单调递减队列(可以取队头)，求min用单调递增队列。  
维护一个随着决策点k单调递增，f[i-1][k] - Pi*k的递减序列。

```cpp
sort(car + 1,car + 1 + m);
for (int i = 1; i <= m; i++) {
	q.clear();

	//对于每个i填充对应的一段j
	for (int j = 0; j <= n; j++) {
		f[i][j] = f[i - 1][j];
		if (j) f[i][j] = max(f[i][j], f[i][j - 1]);

		int l = car[i].l, s = car[i].s, p = car[i].p;
		//超出元素出队
		if (q.size() && q.front() < j - l) q.pop_front();
		if (j >= s && q.size()) {
			//取队头进行状态转移
			int k = q.front();
			f[i][j] = max(f[i][j], f[i-1][k] + p * (j - k));
		}

		if (j < s) {
			//维护队尾的单调性
			while (q.size() && f[i - 1][q.back()] - p * q.back() <= f[i - 1][j] - p * j) q.pop_back();
			q.push_back(j);
		}
	}
}
```
				  
				  
- AcWing299 裁剪序列

https://www.acwing.com/solution/acwing/content/2176/    
	
1D/1D 动态规划 + 单调队列 + 二叉堆(multiset)	


F[i]把前i个数分成若干段，满足每段中所有数的和不超过M的前提下，各段的最大值之和是多少。             
F[i] = min{F[j] + max{Ak}} , 0 <= j < i, j + 1 <= k <= i, Ak求和 <= M；        
DP转移优化的指定思想是及时排除不可能的决策，保持候选集合的高度有效性和秩序性。     
维护一个决策点j单调递增，数值Aj单调递减的队列。   

二叉堆与单调队列建立映射关系：二叉堆与单调队列保存相同的候选集合，该插入的时候一起插入，该删除的时候一起删除（懒惰删除法）   
单调队列以Aj递减作为比较大小的依据，二叉堆以F[j] + max {Ak} (j+1 <= k <= i)作为比较大小的依据，保证能快速在候选集合中查询最值。

// 单调队列模板更新：求max递减队列，求min递增队列。     
1D/1D 动态规划：F[i] = min {F[j] + val(i,j)}, L(i) <= j <= R(i)         
最优化问题 L(i)和R(i)是关于变量i的一次函数，限制j的取值范围；可以把val(i,j)分成两个部分，第一部分与i有关， 第二部分与j有关。      
val(i,j)的每一项都仅与i和j中的一个有关，是单调队列进行优化的基本条件。        

```cpp
//yxc 							 
multiset<LL> S;

void remove(LL x)
{
	auto it = S.find(x);
	S.erase(it);
}


int hh = 0, tt = 0;
LL sum = 0;
for (int i = 1, j = 0; i <= n; i ++ )    // 双指针 j  i
{
	sum += a[i];
	while (sum > m) sum -= a[ ++ j];     //滑动窗口sum和

	while (hh <= tt && q[hh] <= j)      //j之前的全部队头出栈
	{
		if (hh < tt) remove(f[q[hh]] + a[q[hh + 1]]); //不是第一次
		hh ++ ;
	}
	
	int tail = tt;
	while (hh <= tt && a[q[tt]] <= a[i])  //队尾不符合单调递减序列的，全部出栈
	{
		if (tt != tail) remove(f[q[tt]] + a[q[tt + 1]]); //不是第一次，tt+1存在的情况
		tt -- ;
	}
	
	
	if (hh <= tt && tt != tail) remove(f[q[tt]] + a[q[tt + 1]]); //队列非空，删除队尾 f[j] + a[j+1]，tt!=tail 队尾存在的情况
	
	
	//同入
	q[ ++ tt] = i;     // 队尾入队 
	if (hh < tt) S.insert(f[q[tt - 1]] + a[q[tt]]);   // 队尾入S
	
	//f[i]取最小
	f[i] = f[j] + a[q[hh]];                         //更新f[i]
	if (S.size()) f[i] = min(f[i], *S.begin());    //S是满足队列条件的最小值
}
```

```cpp
//stl版本
#include <iostream>
#include <queue>
#include <algorithm>
#include <set>

using namespace std;

using LL = long long;
const int N = 100010;
int n;
LL m;
int a[N];
deque<int> q;
multiset<LL> S;
LL f[N];

void remove(LL x) {
    auto it = S.find(x);
    S.erase(it);
}

int main() {
    scanf("%d%lld", &n, &m);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        if (a[i] > m) {
            puts("-1");
            return 0;
        }
    }

    LL sum = 0;
    //双指针i j
    for (int i = 1, j = 0; i <= n; i++) {
        //滑窗sum出j
        sum += a[i];
        while (sum > m) sum -= a[++j];

        //删队头超界的
        //单调队列以Aj递减作为比较大小的依据，二叉堆以F[j] + max {Ak} (j+1 <= k <= i)作为比较大小的依据，保证能快速在候选集合中查询最值。
        int frt = INT32_MAX;
        while (q.size() && q.front() <= j) {
            if (q.size() > 1) {
                frt = q.front();
                q.pop_front();
                remove(f[frt] + a[q.front()]);
            } else{
                q.pop_front();
            }
        }

        //删队尾不符合单调递减的队列
        int pre = INT32_MAX;
        while (q.size() && a[q.back()] <= a[i]) {
            if (pre != INT32_MAX) {
                remove(f[q.back()] + a[pre]);
            }
            pre = q.back();
            q.pop_back();
        }
        if (q.size() && pre != INT32_MAX) {
            remove(f[q.back()] + a[pre]);
        }

        //同入队
        if (q.size()) S.insert(f[q.back()] + a[i]);
        q.push_back(i);

        //更新f[i]
        f[i] = f[j] + a[q.front()];
        //f[i][j] = max {f[i-1][k] + Pi(j-k)} ， j - Li <= k <= Si - 1
        if(S.size()) f[i] = min(f[i],*S.begin()); // 满足条件的最小值
    }
    printf("%lld\n", f[n]);

    return 0;
}		    
		    
```


- AcWing312 乌龟棋 

https://www.acwing.com/solution/content/3953/

f[b1][b2][b3][b4]表示所有第i种卡片使用了bi张的走法的最大分值

多维DP

```cpp
for (int A = 0; A <= b[1]; A++) {
	for (int B = 0; B <= b[2]; B++) {
		for (int C = 0; C <= b[3]; C++) {
			for (int D = 0; D <= b[4]; D++) {
				int &v = f[A][B][C][D];
				int t = score[A + 2 * B + 3 * C + 4 * D];
				v = t;
				if (A) v = max(v, f[A - 1][B][C][D] + t);
				if (B) v = max(v, f[A][B - 1][C][D] + t);
				if (C) v = max(v, f[A][B][C - 1][D] + t);
				if (D) v = max(v, f[A][B][C][D - 1] + t);
			}
		}
	}
}

```

						 

- AcWing 313 花店橱窗

f[i][j]:前i种花插入j个花瓶的最大价值

转移方程：
j == i: f[i][j] = f[i - 1][j - 1] + g[i][j];
j > i: f[i][j] = max(f[i][j - 1], f[i - 1][j - 1] + g[i][j])


//更新参考答案
```cpp
#include <iostream>

using namespace std;

const int N = 110, M = 110;
int g[N][M], f[N][M]; //f[i][j] i种花、j个花瓶的最大价值
int m, n;

void print(int i, int j) {
    if (i == 0 || j == 0) return;
    while (f[i][j] == f[i][j - 1]) j--; //花瓶没插入
    print(i - 1, j - 1);
    cout << j << " ";
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> g[i][j];
        }
    }

    for (int i = 1; i <= n; i++) {
        for (int j = i; j <= m; j++) {
            if (j == i) f[i][j] = f[i - 1][j - 1] + g[i][j];
            else f[i][j] = max(f[i][j - 1], f[i - 1][j - 1] + g[i][j]);
        }
    }
    cout << f[n][m] << endl;
    print(n, m);

    return 0;
}
```



- AcWing314 低买

https://www.acwing.com/solution/content/26282/   

最长单调递减子序列(LDS)的长度和方案数，继承思想      

fi = max{fj} + 1 (j < i，aj > ai) ： 让每个位置的元素继承大于其自身的前缀中的最优解。    
aj > ai且 fj + 1 = fi :  则fi的最优解可以利用j转移，则i位置的方案数可以继承j位置的方案数。   
f[i]以i结尾的长度，g[i]以i结尾的方案数            

//LDS模板更新
```cpp
 
const int N = 5010;
int a[N];
int f[N], g[N]; //f[i]以i结尾的子序列长度, g[i]以i结尾的方案数
int n;
 

g[0] = 1;
for (int i = 1; i <= n; i++) {
	for (int j = 0; j < i; j++) {
		//序列长度继承
		if (!j || a[i] < a[j]) f[i] = max(f[i], f[j] + 1);
	}

	for (int j = 1; j < i; j++) {
		//相同值的只考虑最后一个
		if (a[j] == a[i]) {
			f[j] = 0;
		}
	}
	for (int j = 0; j < i; j++) {
		if ((!j || a[i] < a[j]) && f[i] == f[j] + 1) {
			// 方案数继承
			g[i] += g[j];
		}
	}
}
int res = 0;
for (int i = 0; i <= n; i++) {
	res = max(res, f[i]);
}
int cnt = 0;
for (int i = 1; i <= n; i++) {
	if (f[i] == res){
		cnt += g[i];
	};  //回溯方案
}
cout << res << " " << cnt << endl;
```
	
	
	

- AcWing315 旅行

LCS + 26字符dfs

更新LCS模板: 
f[i][j]表示a的前i个字符, b的前j个字符的最长串长度
	
正向方程：       
f[i][j] = f[i−1][j−1]      if a[i] == b[j]         
f[i][j] = max(f[i−1][j],f[i][j−1])         

	
```cpp


//26方向dfs 模板
void dfs(int i, int j, int u, int len) {
    if (u > len) {
        puts(path + 1);
        return;
    }
    if (s1[i] == s2[j]) {
        path[u] = s1[i];
        dfs(i + 1, j + 1, u + 1, len);
    } else {
        //26字母dfs
        for (int k = 0; k <= 25; k++) {
            int a = 0; //s1中下一个字母k出现的位置
            int b = 0;
            for (int x = i; x <= n; x++) {
                if (s1[x] == 'a' + k) {
                    a = x;
                    break;
                }
            }
            for (int x = j; x <= m; x++) {
                if (s2[x] == 'a' + k) {
                    b = x;
                    break;
                }
            }
            if (a && b && f[a][b] == f[i][j]) {
                //f[a][b]和f[i][j]值相同，说明都转移到f[i-1][j-1]
                dfs(a, b, u, len);
            }
        }
    }

}

//LCS模板

int main() {
    cin >> s1 + 1 >> s2 + 1;
    n = strlen(s1 + 1), m = strlen(s2 + 1);

    //反向迭代方便dfs正向求方案
    //注意dfs的遍历方向和公式的关系
    for (int i = n; i; i--) {
        for (int j = m; j; j--) {
            if (s1[i] == s2[j]) {
                f[i][j] = f[i + 1][j + 1] + 1;
            }
            else {
                f[i][j] = max(f[i + 1][j], f[i][j + 1]);
            }
        }
    }

    //反向dfs求方案
    dfs(1, 1, 1, f[1][1]);

    return 0;
}
```
 
 	
- AcWing316  减操作 

https://www.acwing.com/solution/content/2590/   

01背包问题，a[1]一定是加，a[2]一定是减。

解方程组： x + y = sum, x - y = t    
则y = (sum - t) / 2 


只有当i−1位进行cut操作的时候,这个第i位才可以是减 --->  一个数字前面是+号,只有在它这一位进行cut操作。

f[i]第i个数是不是减号

```cpp
#include <iostream>
#include <cstring>

using namespace std;


const int N = 10001, M = 100001;

int n, t;
int flag = 0;
int a[N], f[M], book[N]; //book数组,符号是否为减法


// x体积
// s物品选择
// dfs回溯
void find(int x, int s) {
    if (x == 0) {
        flag = 1;
        return;
    }
    for (int i = s; i >= 3; i++) {
        if (x - a[i] >= 0 && f[x - a[i]]) {
            book[i] = 1;
            find(x - a[i], i - 1);
            if (flag) return;
            book[i] = 0;
        }
    }
};

int main() {
    cin >> n >> t;
    int sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += a[i];
        cin >> a[i];
    }

    int s = (sum - t) >> 1 - a[2];

    //dp背包
    f[0] = 1;
    //物品
    for (int i = 2; i <= n; i++) {
        //体积
        for (int j = s; j >= a[i]; j--) {
            f[i] = f[j] | f[j - a[i]];
        }
    }

    book[2] = 1; //2肯定被选上
    find(s, n);
    int cnt = 0;
    for (int i = 2; i <= n; i++) {
        if (!book[i]) { //加号, cut操作对应的位置是加号
            cout << i - cnt - 1 << endl;
            cnt++;
        }
    }
    for (int i = 2; i <= n; i++) {
        if (book[i]) {
            cout << 1 << endl;
        }
    }

    return 0;
}
```

	
- AcWing318 划分大理石

零钱问题 - 多重背包问题模板简化

https://www.acwing.com/solution/content/12873/      

f[i]体积(重量)为i是否可以拼成   


```cpp
while (true) {
	int sum = 0;
	for (int i = 1; i <= 6; i++) {
		cin >> a[i];
		sum += a[i] * i;
	}

	if (!sum) break;
	if (sum & 1) {
		cout << "Can't" << endl;
	} else {
		sum /= 2;
		memset(f,0, sizeof(f));
		f[0] = true;
		for (int i = 1; i <= 6; i++) {
			int s = a[i], k = 1;
			//简化多重背包
			while (s >= k) {
				for (int j = sum; j >= i * k; j--) {
					f[j] |= f[j - i * k];
				}
				s -= k;
				k *= 2;
			}
			//0-1背包
			if (s > 0) {
				for (int j = sum; j >= i * s; j--) {
					f[j] |= f[j - i * s];
				}
			}
		}
		if (f[sum]) cout << "Can" << endl;
		else cout << "Can't" << endl;
	}
}
```

	
	
- AcWing320 能量项链

https://www.acwing.com/solution/content/60478/


```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 210, INF = 0x3f3f3f3f;

int n;
int w[N];
int f[N][N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> w[i];
        w[i + n] = w[i];
    }

    for (int len = 3; len <= n + 1; len ++ )
        for (int l = 1; l + len - 1 <= n * 2; l ++ )
        {
            int r = l + len - 1;
            for (int k = l + 1; k < r; k ++ )
                f[l][r] = max(f[l][r], f[l][k] + f[k][r] + w[l] * w[k] * w[r]);
        }

    int res = 0;
    for (int l = 1; l <= n; l ++ ) res = max(res, f[l][l + n]);

    cout << res << endl;

    return 0;
}
```

- AcWing321 棋盘分割


// 补充 
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

const int N = 15, M = 9;
const double INF = 1e9;

int n, m = 8;
int s[M][M];
double f[M][M][M][M][N];
double X;

int get_sum(int x1, int y1, int x2, int y2)
{
    return s[x2][y2] - s[x2][y1 - 1] - s[x1 - 1][y2] + s[x1 - 1][y1 - 1];
}

double get(int x1, int y1, int x2, int y2)
{
    double sum = get_sum(x1, y1, x2, y2) - X;
    return (double)sum * sum / n;
}

double dp(int x1, int y1, int x2, int y2, int k)
{
    double &v = f[x1][y1][x2][y2][k];
    if (v >= 0) return v;
    if (k == 1) return v = get(x1, y1, x2, y2);

    v = INF;
    for (int i = x1; i < x2; i ++ )
    {
        v = min(v, get(x1, y1, i, y2) + dp(i + 1, y1, x2, y2, k - 1));
        v = min(v, get(i + 1, y1, x2, y2) + dp(x1, y1, i, y2, k - 1));
    }

    for (int i = y1; i < y2; i ++ )
    {
        v = min(v, get(x1, y1, x2, i) + dp(x1, i + 1, x2, y2, k - 1));
        v = min(v, get(x1, i + 1, x2, y2) + dp(x1, y1, x2, i, k - 1));
    }

    return v;
}

int main()
{
    cin >> n;
    for (int i = 1; i <= m; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            cin >> s[i][j];
            s[i][j] += s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];
        }

    X = (double)s[m][m] / n;
    memset(f, -1, sizeof f);
    printf("%.3lf\n", sqrt(dp(1, 1, 8, 8, n)));

    return 0;
}
```	
	
-AcWing 323 战略游戏

https://www.acwing.com/solution/content/66365/

```cpp


```
