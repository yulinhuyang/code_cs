
在线PS:
  
https://ps.gaoding.com/#/     
  
https://www.photopea.com/    


挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优     
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1    

https://www.bilibili.com/video/BV1KE411V79h     

						   
- AcWing344 观光之旅

https://www.acwing.com/solution/content/9256/

https://www.acwing.com/solution/content/20140/

floyd不用邻接表，直接

g[i][j]边长关系存储
d[i][j]连通性、最短距离

pos[i][j]：最短路中经过的点是k，且k是此路径中的编号最大的点(除去i、j)。    
性质：i ~ j的最短道路中，一定没有环；i,j之间的最短道路经过k,则i~k、k~j之间无交集。   

无向图的最小环： 状态方程 min(d[i][j] + a[j][k] + a[k][i]), 其中1 <= i < j < k 

```cpp
//floyd模板题   
void get_path(int i, int j) {
    if (pos[i][j] == 0) return;

    int k = pos[i][j];
    get_path(i, k);
    path[cnt++] = k;
    get_path(k, j);
}

for (int k = 1; k <= n; k++) {
	//一轮求最小环
	for (int i = 1; i < k; i++) {
		for (int j = i + 1; j < k; j++) {
			//d[i][j] 是上一轮的floyd的结果，最大包含k-1
			if (res > ((long long)d[i][j] + g[j][k] + g[k][i])) {
				res = d[i][j] + g[j][k] + g[k][i];
				cnt = 0;
				path[cnt++] = k;
				path[cnt++] = i;
				get_path(i, j);
				path[cnt++] = j;
			}
		}
	}

	//一轮求floyd,更新d
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			if (d[i][j] > d[i][k] + d[k][j]) {
				pos[i][j] = k;
				d[i][j] = d[i][k] + d[k][j];
			}
		}
	}
}

```

- AcWing345 牛站

https://www.acwing.com/solution/content/17209/    

限定边数的最短路问题:    
floyd + 倍增： https://www.acwing.com/solution/content/36603/      
Bellman-Ford: https://www.acwing.com/solution/content/6111/    

 
这里使用floyd + 倍增

d[k][i][j]从i走到j正好经过k条路径的最短距离。
d[a+b][i][j] = min(dist[a+b][i][j], dist[a][i][k] + dist[b][k][j])

k条边，将k转为二进制数，倍增拼凑  

答案数组=g数组走过2条边的最短距离 + g数组走过4条边的最短距离 +  g数组走过16条边的最短距离     

```cpp
//类floyd
//用上次的结果来更新一次
void mul(int c[][N],int a[][N], int b[][N]){
    static int temp[N][N];
    memset(temp, INF, sizeof(temp));
    for(int k = 1;k <= n;k++){
        for(int i = 1;i <= n;i++){
            for(int j = 1;j <= n;j++){
                temp[i][j] = min(temp[i][j], a[i][k] + b[k][j]);
            }
        }
    }
 
    memcpy(c,temp,sizeof(temp));
}
 
void qmi(){
     
    memset(res,INF,sizeof(res));
    for(int i = 1;i <= n;i++) res[i][i] = 0;
 
    while (k)
    {
        if(k & 1) mul(res,res,g);
        mul(g,g,g);
        k >>= 1;
    }
 
}
```

- AcWing853 有边数限制的最短路

模板加入更新，back数组

https://www.acwing.com/solution/content/6320/ 

https://www.acwing.com/solution/content/14088/

```cpp
const int N = 510, M = 10010;

struct Edge {
    int a;
    int b;
    int w;
} e[M];//把每个边保存下来即可
int dist[N];
int back[N];//备份数组防止串联
int n, m, k;//k代表最短路径最多包涵k条边

int bellman_ford() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    for (int i = 0; i < k; i++) {//k次循环
        memcpy(back, dist, sizeof dist);
        for (int j = 0; j < m; j++) {//遍历所有边
            int a = e[j].a, b = e[j].b, w = e[j].w;
            dist[b] = min(dist[b], back[a] + w);
            //使用backup:避免给a更新后立马更新b, 这样b一次性最短路径就多了两条边出来
        }
    }
    if (dist[n] > 0x3f3f3f3f / 2) return -1;
    else return dist[n];

}

int main() {
    scanf("%d%d%d", &n, &m, &k);
    for (int i = 0; i < m; i++) {
        int a, b, w;
        scanf("%d%d%d", &a, &b, &w);
        e[i] = {a, b, w};
    }
    int res = bellman_ford();
    if (res == -1) puts("impossible");
    else cout << res;

    return 0;
}

```


- AcWing346 走廊泼水节

Kruskal算法：对n个点的n-1边的权值从小到大排序，依次扫描每条边，结合并查集算法。 
为了保证(x,y)一定在最小生成树中，必须让(x,y)是权值最小的边，新增的边权值取w + 1,Sx和Sy之间会增加|Sx|*|Sy|-1条边，累加(w+1)*(|Sx|*|Sy|-1)即可。      

kruskal模板题  

```cpp
int n;
struct Edge
{
    int a, b, w;
    bool operator< (const Edge &t) const
    {
        return w < t.w;
    }
}e[N]

for (int i = 0; i < n - 1; i++) {
	int pa = find(e[i].a), pb = find(e[i].b), w = e[i].w;
	if (pa != pb) {
		res += (pSize[pa] * pSize[pb] - 1) * (w + 1);
		pSize[pb] += pSize[pa];
		p[pa] = pb;
	}
}
```

AcWing352 闇の連鎖


- AcWing361 观光奶牛

二分 + SPFA求负环 + 0-1分数规划

https://www.acwing.com/solution/content/6472/

mid < Σf[vi]/ Σtime[ei] 

对于每轮二分，建立一张新图，结构与原图相同，但是没有点权，有向边e=(x,y) 的权值是 mid *time[e] - fun[x]，即原本的边权乘上mid再减去入点的权值。    
新图上，如果 Σ mid *time[ei] - fun[vi] < 0,则图中存在负环，用SPFA求负环，mid比答案小，则l = mid;     
若最短路的求解正常结束，则r = mid。   

SPFA判负环方法：

```cpp
//最短路包含的边数不超过n次, 超过n次就说明存在负环.   
cnt[i] = cnt[j] + 1;
if (cnt[i] > n) return false;
```

SPFA会多次出入队列，st[i]表示i是否在队列中

```cpp
void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

//是否存在负环
bool spfa(double mid) {
    for (int i = 1; i <= n; i++) {
        dist[i] = 0;
        cnt[i] = 0;
        st[i] = false;
    }

    queue<int> q;
    //st是否在队列中
    for (int i = 1; i <= n; i++) {
        q.push(i);
        st[i] = true;
    }

    while (q.size()) {
        auto t = q.front();
        q.pop();
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i] * mid - f[j]) {
                dist[j] = dist[t] + w[i] * mid - f[j];
                cnt[j] = cnt[t] + 1;

                if (cnt[j] >= n) return true;
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }

    return false;
}


double l = 0.0, r = 1e9;
while (r - l > 1e-8) {
	double mid = (l + r) / 2.0;
	if (spfa(mid)) l = mid;
	else r = mid;
}
cout << fixed << setprecision(2) << r << endl;

```



差分约束系统：
N个变量X1~XN,M个约束条件，每个约束条件都由两个变量作差构成，如Xi-Xj<=ck。      
变形为Xi <= Xj+Ck,这个与最短路问题中三角形不等式dist[y] <= dist[x] +z相似，可以把每个变量Xi看作有向图中的一个节点i,对于每个约束条件     
Xi-Xj <= Ck，从节点j到节点i连一条长度为Ck的有向边。  
设dist[0] = 0,以0为起点求单源最短路，如果图中存在负环，则给定的差分约束系统无解；否则Xi=dist[i]就是差分约束系统的一组解。  


- AcWing362 区间

从0~50000中选出尽量少的整数，使每个区间[ai,bi]内都有至少ci个数被选。

差分约束  + SPFA求最长路

https://www.acwing.com/solution/content/42920/

s[k]表示0~k之间最少选出多少整数，前缀和思想

约束条件：
s[k]-s[k-1] >= 0  ---> 从每个k-1到k连长度为0的有向边      
s[k-1]-s[k] >= -1 ---> 从每个k到k-1连接长度为-1的有向边。    
s[bi] - s[ai-1] >= ci 从每个ai-1到bi连长度为ci的有向边。       
s[-1] = 0,以-1为起点求单源最长路。 

ci <= bi -ai + 1,图中没有正环，差分约束一定有解

```cpp

//求最长路
void spfa(){
    queue<int> q;

    for(int i = 0;i <= 50001;i++){
        q.push(i);
        st[i] = true;
    }

    while (q.size()) {
        auto t = q.front();
        q.pop();
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i]) {
            int j = e[i];
            if (dist[j] < dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
}


int main() {

    cin >> n;
    memset(h, -1, sizeof(h));
    for (int i = 0; i < n; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        a++, b++;  // 0~50000  -> 1~50001
        add(a - 1, b, c);
    }

    for (int i = 1; i <= 50001; i++) {
        add(i - 1, i, 0);
        add(i, i - 1, -1);
    }

    spfa();
    cout << dist[50001] << endl;

    return 0;
}
```

- AcWing367 学校网络

tarjan求有向图的强连通分量模板题

在追溯值计算过程中，若从x回溯前，有low[x]=dfn(x)成立，则栈中从x到栈顶的所有节点构成一个强连通分量。

tarjan算法求出所有强连通分量，并执行缩点过程，得到有向无环图。
求有向无环图中零入度点的个数 a 
最少加多少边，把图变成强连通图：max(p,q) p个零入度点，q个零出度点。  

```cpp

//tarjan模板
void tarjan(int u) {
    dfn[u] = low[u] = ++cnt;
    stk[++tt] = u, in_stk[u] = true;
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (!dfn[j]) {
            tarjan(j);
            low[u] = min(low[u], low[j]);
        } else if(in_stk[j]) {
            low[u] = min(low[u], low[j]);
        }
    }

    //x到栈顶所有节点是同一个强连通分量
    if (dfn[u] == low[u]) {
        int y;
        ++num; //组号
        do {
            y = stk[tt--];
            in_stk[y] = false;
            id[y] = num;
        } while (y != u);
    }
}


//tarjan缩点
for (int i = 1; i <= n; i++) {
	if (!dfn[i]) {
		tarjan(i);
	}
}

//计算每个组的出度和入度
for (int i = 1; i <= n; i++) {
	for (int j = h[i]; ~j; j = ne[j]) {
		int k = e[j];
		if (id[k] != id[i]) {
			din[id[k]]++;
			dout[id[i]]++;
		}
	}
}

int a = 0, b = 0;
for (int i = 1; i <= num; i++) {
	if (!din[i]) a++;
	if (!dout[i]) b++;
}
```
AcWing368 银河

AcWing372 棋盘覆盖


AcWing376 机器任务

AcWing378 骑士放置



						   
						   
						   
