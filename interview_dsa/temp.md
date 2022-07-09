
**AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】**

https://www.acwing.com/solution/content/56348/

挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优     
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1    

https://www.bilibili.com/video/BV1KE411V79h     


**AcWing 271 杨老师的照相排列**

https://www.acwing.com/solution/content/4954/

5维DP，闫氏DP以最后一个同学被安排在哪一排作为划分依据。

```cpp
for (int a = 0; a <= s[0]; a++) {
	for (int b = 0; b <= min(a, s[1]); b++) {
		for (int c = 0; c <= min(b, s[2]); c++) {
			for (int d = 0; d <= min(c, s[3]); d++) {
				for (int e = 0; e <= min(d, s[4]); e++) {
					int &x = f[a][b][c][d][e];
					if (a && a - 1 > b) x += f[a - 1][b][c][d][e];
					if (b && b - 1 > c) x += f[a][b - 1][c][d][e];
					if (c && c - 1 > d) x += f[a][b][c - 1][d][e];
					if (d && d - 1 > e) x += f[a][b][c][d - 1][e];
					if (e) x += f[a][b][c][d][e - 1];
				}
			}
		}
	}
}
```

- AcWing 272 最长公共上升子序列 FCIS

https://www.acwing.com/solution/content/4955/

f[i][j]代表所有a[1 ~ i]和b[1 ~ j]中以b[j]结尾的公共上升子序列的集合；   
f[i][j]的值等于该集合的子序列中长度的最大值；    

划分：   
不包含a[i]的子集，最大值是f[i - 1][j]      
包含a[i]的子集，将这个子集继续划分，依据是子序列的倒数第二个元素在b[]中是哪个数    



```cpp
//基础版
for (int i = 1; i <= n; i++) {
	for (int j = 1; j <= n; j++) {
		f[i][j] = f[i - 1][j];
		if (a[i] == b[j]) {
			int maxV = 1;
			for (int k = 1; k < j; k++) {
				if (a[i] > b[k]) {
					maxV = max(maxV, f[i - 1][k] + 1);
				}
			}
			f[i][j] = max(f[i][j], maxV);
		}
	}
}
```


```cpp
//循环优化版
for (int i = 1; i <= n; i++) {
	int maxv = 1;
	for (int j = 1; j <= n; j++) {
		f[i][j] = f[i - 1][j];
		if (a[i] == b[j]) f[i][j] = max(f[i][j], maxv);
		if (a[i] > b[j]) maxv = max(maxv, f[i - 1][j] + 1);
	}
}
```

- AcWing 273 分级 

https://www.acwing.com/solution/content/4956/   

非严格单调，贪心 + 序列型DP, 前缀和思想优化掉一维循环  
    
一定存在一组最优解，使得每个Bi都是原序列中的某个值     

f[i][j] 代表所有给A[1]~ A[i]分配好了值且最后一个B[i] = A'[j]的方案的集合；      
f[i][j] 的值是集合中所有方案的最小值；    

```cpp
    for (int i = 1; i <= n; i++) b[i] = a[i];
    sort(b + 1, b + 1 + n);
    for (int i = 1; i <= n; i++) {
        int minv = INF;
        for (int j = 1; j <= n; j++) {
            minv = min(minv, f[i - 1][j]);
            f[i][j] = minv + abs(a[i] - b[j]);
        }
    }
    int res = INF;
    for (int i = 1; i <= n; i++) {
        res = min(res, f[n][i]);
    }
```

- AcWing 274 移动服务   

https://www.acwing.com/solution/content/4957/   

求DP先确定阶段，如果阶段不足以表示一个状态，把附加信息作为状态的维度。  
确定DP状态时，选择最小的能够覆盖整个状态空间的维度集合。

f[i][x][y]表示已经处理完前i个请求，且三个服务员处于p[i],x,y的所有方案的集合。
f[i][x][y]的值是集合中所有方案的花费的最小值。

三维状态的DP

```cpp
for (int i = 0; i < m; i++) {
	for (int x = 1; x <= n; x++) {
		for (int y = 1; y <= n; y++) {
			int z = p[i], v = f[i][x][y];
			if (x == y || y == z || x == z) continue;
			int u = p[i + 1];
			f[i + 1][x][y] = min(f[i + 1][x][y], v + w[z][u]);
			f[i + 1][x][z] = min(f[i + 1][x][z], v + w[y][u]);
			f[i + 1][z][y] = min(f[i + 1][z][y], v + w[x][u]);
		}
	}
}
```

- AcWing 275  传纸条 

https://www.acwing.com/solution/content/3954/ 

寻找两条路径等效：交集中的格子一定在每条路径的相同步数处，让两个人同时从起点出发，每次同时走一步，这样路径中相交的格子一定在同一步内。

状态表示：f[k, i, j] 表示两个人同时走了k步，第一个人在 (i, k - i) 处，第二个人在 (j, k - j)处的所有走法的最大分值。    
状态计算：按照最后一步两个人的走法分成四种情况：
f[k - 1, i, j] + score(k, i, j)    
f[k - 1, i, j - 1] + score(k, i, j)    
f[k - 1, i - 1, j] + score(k, i, j)    
f[k - 1, i - 1, j - 1] + score(k, i, j)     
两人不能走到相同格子，则i和j不能相等    

```cpp
for (int k = 2; k <= n + m; k++) {
	for (int i = max(1, k - m); i < k && i <= n; i++) {
		for (int j = max(1, k - m); j < k && j <= n; j++) {
			//循环四个各种
			for (int a = 0; a <= 1; a++) {
				for (int b = 0; b <= 1; b++) {
					int t = g[i][k - i];
					if (i != j || k == 2 || k == n + m) {
						t += g[j][k - j];
						f[k][i][j] = max(f[k][i][j], f[k - 1][i - a][j - b] + t);
					}
				}
			}
		}
	}
}
```

- AcWing 277 饼干

https://www.acwing.com/solution/content/5240/    

贪心(邻项交换) + DP + 方案倒推推  

一般的DP问题都是在有限集合内求最值或求个数，极个别的DP问题会在无限集合内求最值，此时先将无限集想办法缩小到有限集。
等效处理：如果第i个孩子获取的饼干数大于1，等价于分配j-i个饼干给前i个孩子，饼干数大小顺序相对不变。

状态表示：f[i][j] 所以将j个饼干分配给前i个小朋友，且分配的饼干数单调下降的分配方案的怨气总和最小值。  
状态计算：划分依据，最后有几个小朋友也都分配1个饼干。   
f[i-k][j-k] + (s[i] - s[i-k])*(i - k)   

```cpp

sort(g + 1, g + 1 + n);
reverse(g + 1, g + 1 + n);
for (int i = 1; i <= n; i++) s[i] = s[i - 1] + g[i].first; //前缀和

memset(f,0x3f3f, sizeof(f));
f[0][0] = 0;
for (int i = 1; i <= n; i++) {
	for (int j = 1; j <= m; j++) {
		if (j >= i) f[i][j] = f[i][j - i];     //糖比人多，每人少分一块，不影响怨气值
		for (int k = 1; k <= i && k <= j; k++) {
			f[i][j] = min(f[i][j], f[i - k][j - k] + (s[i] - s[i - k]) * (i - k));
		}
	}
}

cout << f[n][m] << endl;
int i = n, j = m;
int h = 0;
while (i && j) {
	if (j >= i && f[i][j] == f[i][j - i]) {
		j -= i;
		h++;   //等效处理的还原回来
	} else {
		for (int k = 1; k <= i && k <= j; k++) {
			if (f[i][j] == f[i - k][j - k] + (s[i] - s[i - k]) * (i - k)) {
				//i-k ~ i的每个人都分配一块
				for (int u = i; u > i - k; u--) {
					ans[g[u].second] = 1 + h;
				}
				i -= k;
				j -= k;
				break;
			}
		}
	}
}
```



- AcWing278 数字组合

https://www.acwing.com/solution/content/5241/  

0-1背包求方案数

```cpp
f[0] = 1;
for (int i = 0; i < n; i++) {
	for (int j = m; j >= a[i]; j--) {
		f[j] += f[j - a[i]];
	}
}
```

- AcWing279 自然数拆分

完全背包求方案数   

```cpp
f[0] = 1;
for (int i = 1; i < n; i++) {
	for (int j = i; j <= n; j++) {
		f[j] = (f[j] + f[j - i]) % MOD;
	}
}
```

- AcWing280 陪审团

https://www.acwing.com/solution/content/11079/   

二维体积的0-1背包问题  
F[j,d,p] = F[j,d,p] or F[j - 1,d - a[i],p - b[i]]  
f[i][j][k]:前i个人中选j个人,且总差为k时候的总分

不选，则f[i][j][k]=f[i-1][j][k]
选，f[i][j][k]=f[i-1][j-1][k-(p[i]-d[i])]+d[i]+p[i]

puts()函数用来向标准输出设备（屏幕）写字符串并换行, puts("\n");   

```cpp
memset(f,-0x3f, sizeof(f));
f[0][0][base] = 0;
for (int i = 1; i <= n; i++) {
	for (int j = 0; j <= m; j++) {
		for (int k = 0; k < M; k++) {
			f[i][j][k] = f[i - 1][j][k]; //不选i
			int t = k - (p[i] - d[i]);
			if (t < 0 || t >= M) continue;
			if (j < 1) continue;
			f[i][j][k] = max(f[i][j][k], f[i - 1][j - 1][t] + p[i] + d[i]); //选i
		}
	}
}
int v = 0;
while (f[n][m][base + v] < 0 && f[n][m][base - v] < 0) v++;
if (f[n][m][base - v] > f[n][m][base + v]) {
	v = base - v;
} else {
	v = base + v;
}

//回溯求DP方案
int cnt = 0, i = n, j = m, k = v;
while (j) {
	if (f[i][j][k] == f[i - 1][j][k]) i--;
	else {
		ans[cnt++] = i;
		k -= (p[i] - d[i]);
		i--, j--;
	}
}
```


- AcWing281 硬币

https://www.acwing.com/solution/content/11075/  

多重背包 + 滑动窗口，求可行性

```cpp
//多重背包模板更正
//三循环一维
for (int i = 1; i <= n; i ++ ) //循环各组
    for (int j = m; j >= 0; j -- )
        for (int k = 0; k <= s[i] && v[i]*k <= j; k ++ ) //组内循环 
            f[j] = max(f[j], f[j - v[i]*k] + w[i]*k);
```

多重背包模板会超时，需要进行优化(空间换时间)才行 
```cpp
//超时版
memset(f, false, sizeof(f));
f[0] = 1;
for (int i = 1; i <= n; i++) {
	for (int j = m; j >= 0; j--) {
		for (int k = 0; k <= s[i] && v[i] * k <= j; k++) {
			if (!f[j] && f[j - v[i] * k]) {
				f[j] = 1;
			}
		}
	}
}
```

关注可行性，不是最优性     
设used[j]表示在第i阶段下将f[j]变为true至少需要使用多少枚第i种硬币，去掉一重循环。

1 前i-1种硬币就能拼成面值j，即在第i阶段开始前，f[j]已经成为true。尽量走这种情况
2 使用了第i种硬币，即在第i阶段的递推过程中，发现f[j-a[i]]为true，从而使用一个i硬币，使f[j]变为true

```cpp
//空间换时间版
for (int i = 1; i <= n; i++) {
	memset(used,0, sizeof(used));
	for(int j = v[i];j <= m;j++){
		if(!f[j] && f[j - v[i]] && used[j - v[i]] < s[i])  
		{ 
			f[j] = 1;
			used[j] = used[j - v[i]] + 1;
		}
	}
}
```

- AcWing 282 石子合并  

https://www.acwing.com/solution/content/13945/   

区间DP模板题

```cpp
//区间DP
//模板1  
for (int len = 2; len <= n; len ++ )         //枚举长度 
    for (int l = 1; l + len - 1 <= n; l ++ ) //枚举起点
    {
        int r = l + len - 1;
        f[l][r] = INF;
        for (int k = l; k < r; k ++ )        //枚举分割点
            f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
    }
	

//模板2	
memset(f, 0x3f3f3f, sizeof(f));
for(int i = 0;i <= n;i++){
	f[i][i] = 0;
}

for (int len = 2; len <= n; len++) {
	for (int l = 1; l + len - 1 <= n; l++) {
		int r = l + len - 1;
		for (int k = l; k < r; k++) {
			f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
		}
	}
}	
```


**AcWing283 多边形**   

https://www.acwing.com/solution/content/1119/  

区间DP + 状态机(min、max) + 拆环成链
   
任意选择一个位置断开，复制形成两倍长度的链     

```cpp
for (int len = 1; len <= n * 2; len++) {
	for (int l = 1; l + len - 1 <= n * 2; l++) {
		int r = l + len - 1;
		if (len == 1) {
			f[l][r] = g[l][r] = w[l];
		} else {
			f[l][r] = -INF;
			g[l][r] = INF;
			for (int k = l; k < r; k++) {
				int minl = g[l][k], minr = g[k + 1][r], maxl = f[l][k], maxr = f[k + 1][r];
				if (c[k + 1] == 't') {
					f[l][r] = max(f[l][r], maxl + maxr);
					g[l][r] = min(g[l][r], minl + minr);
				} else {
					int x1 = minl * minr, x2 = minl * maxr, x3 = maxl * maxr, x4 = maxl * minr;
					f[l][r] = max(f[l][r], max(max(x1,x2),max(x3,x4)));
					g[l][r] = min(g[l][r], min(min(x1,x2),min(x3,x4)));
				}
			}
		}
	}
}
int res = 0;
for (int i = 1; i <= n; i++) {
	res = max(res, f[i][i + n - 1]);
}
```

- AcWing 284 金字塔

https://www.acwing.com/solution/content/10533/

区间DP + 记忆化搜索 / 区间DP

f[l][r]表示s[l][r]对应多少种可能的金字塔结构(树形结构)      
方案计数类的DP问题，一个状态的各个决策之间满足加法原理，而每个决策划分的几个子状态之间满足乘法原理。

```cpp
//记忆化搜索lyd解法
int solve(int l, int r){
	if(l == r) return 1;
	if(l > r) return 0;
	if(f[l][r] != -1) return f[l][r];
	f[l][r] = 0;
	for(int k = l + 2;k <= r;++k)      //枚举第一棵子树在s中划分的结束位置
		if(s[k] == s[l])
			f[l][r] = (f[l][r] + (ll)solve(l+1, k-1)*solve(k, r)) % mod;
	return f[l][r];
}
```

```cpp
//yxc解法
for (int len = 1; len <= n; len += 2) {
	for (int l = 1; l + len - 1 <= n; l++) {
		int r = l + len - 1;
		if(len == 1) f[l][r] = 1;
		else if(s[l] == s[r]){
		   for (int k = l; k < r; k += 2) {
				if(s[k] == s[r]){
					f[l][r] = (f[l][r] + (long long) f[l][k] * f[k + 1][r - 1]) % MOD;
				}
			}
		}
	}
}
```

- AcWing 285 没有上司的舞会

树形DP：dfs + 状态机

树形DP模板更新

DP的阶段：一般以节点从深到浅(子树从小的到大)的顺序作为DP的阶段
DP的状态计算：第一维是节点编号(代表该节点为根的子树),第二位是状态机的状态;递归的方式实现树形DP,先递归在它的每个子节点上进行DP,回溯时，从子节点向节点x进行状态转移。  

```cpp
void add(int a,int b){
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void dfs(int u){
    f[u][1] = w[u];
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        dfs(j);
        f[u][0] += max(f[j][0], f[j][1]);
        f[u][1] += f[j][0];
    }
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> w[i];
    }
    memset(h,-1,sizeof(h));
    for (int i = 0; i < n - 1; i++) {
        int a, b;
        cin >> a >> b;
        add(b, a);
        st[a] = true;
    }

    //寻找root
    int root = 1;
    while (st[root]) root++;

    //dfs从根开始
    dfs(root);

    cout << max(f[root][0], f[root][1]);
    return 0;
}
```

- AcWing286 选课

https://www.acwing.com/solution/content/75468/   

树形DP + 分组背包

虚拟课程：0号节点，把包含N个节点的森林转化为包含N+1个节点的树，其中0号节点是根节点。
F[x][t]以x为根的子树中，选t门课能获得的最高学分。
分组背包：有p=|Son(x)|组物品，每组有t-1个，背包总容量t-1，每组选[0,t-1]个。

```cpp
void dfs(int u) {

    //p = |son(x)|组物品，每组 t-1个，背包总容量 t-1,每组选[0,t-1]个
    for (int i = h[u]; ~i; i = ne[i]) {
        int son = e[i];
        dfs(son);
        for (int j = m - 1; j >= 0; j--) {
            for (int k = 1; k <= j; k++) {
                f[u][j] = max(f[u][j], f[u][j - k] + f[son][k]);
            }
        }
    }
    for (int j = m; j >= 0; j--) {
        f[u][j] = f[u][j - 1] + w[u];
    }
    f[u][0] = 0;
}
```
