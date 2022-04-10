
牛蛙点点提高课题解：https://www.acwing.com/user/myspace/activity/52559/

wzc1995: https://www.acwing.com/user/myspace/index/21/

仅存老实人题解：https://www.acwing.com/user/myspace/solution/index/39715/

#### flyod

适用：多源最短路，传递闭包、找最小环(总和最小的)、恰好经过K条边的最短路(倍增)
原理：d[i][j] = inf，d[i][i] = 0;三重循环，基于dp的思想。
```C++
for (int k = 0; k < n; k ++ )
	for (int i = 0; i < n; i ++ )
		for (int j = 0; j < n; j ++ )
			d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
```

d[i][j][k]:从i到j,经过的编号不超过k。   
d[k][i][j] = min(d[k-1][i][j], d[k-1][i][k] + d[k-1][k][j]) 降低一维。

- AcWing 1125 牛的旅行：牧场是一个连通块，情况1 取>=所有连通块之间的最大值，情况2 经过新边的最长路径。    
方法：1 flyod求出任意两点之间的最短路径 2 求maxd[i]表示和i连通的且距离i最远的点的距离 3 情况1 求maxd[i]的最大值 情况2 枚举在哪两个点之间连边，需要满足条件d[i][j]=inf,maxd[i]+dist[i][j] + maxd[j].     

- AcWing 343 排序：传递闭包，所有间接能到达的点都新连一条直接的边，flyod时间复杂度是O(n3)，求原图的一个传递闭包。    
人为规定所有边的权值为1，三重循环判断传递，d[i][j] |= d[i][k] && d[k][j];           
三种情况：1矛盾 d[i][i] = 1；2 能唯一确定 i!=j,dij和dji必有一个是1；3 顺序不唯一。        
如何排序：每次找到没有标记的最小值输出，再标记一下。              
优化掉一维：a和b连接一条边，如果x和a可以连通，y和b可以连通，则x和y可以连通。         
flyod求最小环，最优化问题，从集合角度考虑，不容易丢东西。   
环上节点的编号最大的是k，环的最短距离：d[i][j] + w[i][k] + w[k][j],k是从i到j的路径中编号最大的点。   
```C++
if ((long long)d[i][j] + g[j][k] + g[k][i] < res)
{
	res = d[i][j] + g[j][k] + g[k][i];
	cnt = 0;
	//更新路径
}
```
- AcWing 345 牛站：恰好经过N条边的最短路。d[i][j]表示从i到j,恰好经过编号不超过k的最短路径。    
flyod变形为经过不超过k条边，在k上求倍增。d[a+b][i][j] = min(d[a][i][j] + d[b][k][j]),k = 1~n。    
d[n][s][e] 从s到e最多经过n条边，可以处理负环。    
n段路径：路径之间独立，可以使用结合律，矩阵乘法，快速幂，倍增方法拼接路径。复杂度n3/*logn    

```C++
void qmi()
{
    memset(res, 0x3f, sizeof res);
    for (int i = 1; i <= n; i ++ ) res[i][i] = 0;

    while (k)
    {
        if (k & 1) mul(res, res, g);    // res = res * g
        mul(g, g, g);   // g = g * g
        k >>= 1;
    }
}
```

#### SPFA求负环

Bellmen-Ford判定负环: 若经过n轮迭代，算法仍未结束（仍有能产生更新的边），则图中存在负环;若n - 1轮迭代之内，算法结束（所有边满足三角不等式），则图中无负环

SPFA判定负环:
- 1 设cnt[x]表示从1到x的最短路径包含的边数，cnt[1] = 0。当执行更新dist[y] = dist[x] + z 时，并更新cnt[y] = cnt[x] + 1。若此时发现cnt[y] >= n，即最短路径中包含的边数 >= n, 则图中有负环，若算法正常结束则不存在负环。
- 2 记录每个点的入队次数，如果某个点入队n次，则说明有负环。

比较：一般情况下第二种方法的效率不如第一种的高，例如在n个点构成一个负环的图中，第一种的判定方法只要绕行一次，就能发现负环，而第二种方法要绕环n次.

求负环和差分约束都是SPFA的拓展应用。

SPFA求负环代码理解：原图上把所有点加入队列，等价于在新图上新建超级虚拟源点，把虚拟源点加入队列;dist[i]都是0，求负环，初值无所谓，每次减去有限值，最终都更新为-inf。

spfa O(m)
当所有点的入队次数超过2n时，就认为图中很大可能存在负环。
- AcWing 904 虫洞：虫洞相当于负权边，spfa求负环的模板题， cnt[j] = cnt[t] + 1; if (cnt[j] >= n) return true;
- AcWing 361 观光奶牛：点权之和/边权之和最大，0-1分数规划，二分0~1000之间。
sum点权/sum边权 > mid 点权放在边上 -> 求和(fi - mid*ti) > 0 -> 图中是否存在正环 
```C++
if (dist[j] < dist[t] + wf[t] - mid * wt[i])
{
	dist[j] = dist[t] + wf[t] - mid * wt[i];
	cnt[j] = cnt[t] + 1;
```
-AcWing 1165 单词环：环串的平均长度最大。sum(wi-m/*1) > 0 等价于图中存在正环 -> 二分m(0~1000),浮点数二分模板。
优化方法：统计所有点的更新的总次数，如果>2n，则存在正环，return true。
```C++
if (dist[j] < dist[t] + w[i] - mid)
{
	dist[j] = dist[t] + w[i] - mid;
	cnt[j] = cnt[t] + 1;
	if ( ++ count > 10000) return true; // 经验上的trick
```

#### LCA
1 向上标记法 O(n)
2 倍增：
fa[i,j]表示从i开始，向上走2^j步所能走到的节点，0 <=j <=logn,depth[i]表示深度。   
步骤 1 先将两个点跳到同一层 2 让两个点同时往上跳，一直跳到他们的最近公共祖先的下一层。   
二进制拼凑的思想， j=0,f[i][i]的父节点；j>0,f[i][j] = f[f[i][j]][j-1]     
Lca 可以加速最小生成树的效率。     
预处理 O(logn)   查询O(logn)     
3 Tarjan-离线求LCA O(n+m)              
在深度优先遍历时，将所有点分成三大类：a 已经遍历过，且回溯过的点 b 正在搜索的分支 c  还未搜索到的点。  

- AcWing 1172 祖孙询问：LCA模板题。
```C++
void bfs(int root)
{
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0, depth[root] = 1;
    int hh = 0, tt = 0;
    q[0] = root;
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (depth[j] > depth[t] + 1)
            {
                depth[j] = depth[t] + 1;
                q[ ++ tt] = j;
                fa[j][0] = t;
                for (int k = 1; k <= 15; k ++ )
                    fa[j][k] = fa[fa[j][k - 1]][k - 1];
            }
        }
    }
}

int lca(int a, int b)
{
    if (depth[a] < depth[b]) swap(a, b);
    for (int k = 15; k >= 0; k -- )
        if (depth[fa[a][k]] >= depth[b])
            a = fa[a][k];
    if (a == b) return a;
    for (int k = 15; k >= 0; k -- )
        if (fa[a][k] != fa[b][k])
        {
            a = fa[a][k];
            b = fa[b][k];
        }
    return fa[a][0];
}
```
- AcWing 1171 距离：
AcWing 1171 距离(tarjan求lca)： https://www.acwing.com/solution/content/24569/
离线做法，tarjan利用并查集合并节点到各自的根节点，根节点就是代表元素，也是LCA,然后离线处理某个点时，相关查询是否已经算出来。并查集的时间复杂度O(1)。
dfs计算depth + tarjan +并查集 
```C++
int anc = find(y);
res[id] = dist[u] + dist[y] - dist[anc] * 2;
```

基于RMQ的做法：dfs遍历区间最小值问题。
- AcWing 356 次小生成树：kruskal建图，然后枚举所有边，kruskal + BFS + LCA。
AcWing 356 次小生成树(kruskal+lca+证明)：https://www.acwing.com/solution/content/24609/
定理：对于一张无向图，如果存在最小生成树和（严格）次小生成树，那么对于任何一棵最小生成树，都存在一棵(严格)次小生成树，使得这两棵树只有一条边不同。
ans = 最小生成树边权之和。
生成候选生成树：   
将多余边替换掉最大权值边：  W = ans + z − Val1，即W = 最小生成边权之和 + 加上多余边 − 最大权值边    
将多余边强行替换掉次大权值边:   W = ans + z − Val2，即W = 最小生成树之和 + 加入多余边 − 次大权值边   
最大边数组和严格次大边数组： 
Lca倍增数组：f[x][k]=f[fa[x][k−1]][k−1]

- AcWing 352 暗之连锁：
AcWing 352  闇の連鎖(lca倍增+树上差分)： https://www.acwing.com/solution/content/24653/  
只能删除一条主要边,一条附加边,一种边叫做主要边,一种边叫做附加边。   
附加边(x,y):就是将树上x,y之间的路径上的每条主要边,都覆盖了一次。当(x,y)路径上的任意一条主要边消失后,他都可以成为主要边,去维护连通性.   
问题转化：给定一个n−1条边的树,求每一条树边(主要边),被非树边(附加边)覆盖了多少次。   
每一条附加边,使得(x,y)节点的路径上,每一个节点的权值+1.   
情况：
c = 0，+m，删除主要边后，可以随意删除任一条附加边。         
c = 1，+1， 删除主要边后，只能删除主要边的附加边。         
c > 1, +0，没有可以去的。      

```C++
d[a] ++, d[b] ++, d[p] -= 2;
//切边逻辑
int s = dfs(j, u);
if (s == 0) ans += m;
else if (s == 1) ans ++ ;
res += s;
```
树上差分：    
对树上的一段路径进行操作，并询问某个点或某条边被经过的次数。可以在o(1)的时间给某个路径上的所有的边加上一个权值，再用o(n)时间求出来每个点/边的值最终是多少。    
d[x] += c,d[y] += c,d[p] -=2c，x,y求LCA公共祖先的点P，树上差分时需要求LCA。     

点差分：两点u,v之间路径上的所有点权增加x，o=LCA(u,v),o的父亲节点为p，diff[u]+=x,diff[v]+=x,diff[o]-=x,diff[p]-=x;    
边差分：将两点u,v之间路径上的所有边权增加x，o=LCA(u,v)，diff[u]+=x,diff[v]+=x,diff[o]-=2/*x;    

#### 有向图的强连通分量

60 分钟搞定图论中的 Tarjan 算法: https://zhuanlan.zhihu.com/p/101923309     
Tarjan 关于图的连通性 & 连通分量: https://www.acwing.com/blog/content/159/   
有向图求强联通分量算法模板: https://www.acwing.com/blog/content/42/

模板级补全——唤醒词列表： https://www.acwing.com/blog/content/5224/    


连通分量:有向图中，对于分量中任意两点u、v，必然可以从u走到v,且从v走到u。
强连通分量scc：极大连通分量(加上任意一些点之后，都不是一个连通分量了)。
scc作用：通过缩点将有向图转化为拓扑图。
有向图-->缩点(将所有连通分量缩成一个点)-->有向无环图(DAG,拓扑图)。   
有向无环图：可以按照拓扑序去求最短/最长路，递推。
先去求图中的所有强连通分量，然后将所有的强连通分量缩成一个点，再按照拓扑序去处理这个问题。
dfs过程中，边分类：1 树状边(x,y) 2 前向边(x,y,y是x的父亲) 3 后向边(x,y,x反向y) 4 横叉边(左右其他分支搜)

情况1：存在一条后向边，指向祖先节点。2：先走到了横叉边，横叉边再走到祖先节点。 

Tarjan求强连通分量(SCC):
- 时间戳：dfs[u]表示遍历到u的时间戳，low[u]从u开始走，所能遍历到的最小时间戳。
- u是其所在强连通分量的最高点(深度最小的点)，等价于dfn[u] = low[u]
tarjan - 有向图强连通分量:模板，借助栈实现，时间复杂度o(m+n)。
```C++

// Tarjan算法求强连通分量
void tarjan(int u) {
    dfn[u] = low[u] = ++timestamp;      //初始化u节点的遍历序号(连通块号),从u开始走所能遍历到的最小时间戳
    stk[++top] = u;                     // u入栈,栈的意义一会TODO
    in_stk[u] = true;                   //标识u在栈中
    for (int i = h[u]; ~i; i = ne[i]) { //枚举u的每一条出边
        int j = e[i];
        if (!dfn[j]) {                    //如果j没有被访问过
            tarjan(j);                    // dfs继续探索
            low[u] = min(low[u], low[j]); //我孩子能到达的最小遍历序号，我也可以用
        } else if (in_stk[j])             //如果访问过，而且这个点出现在栈中，表示是后向边，回头路
            low[u] = min(low[u], low[j]); //看看是不是可以被回头路上的low[j]改的更小
    }
    if (dfn[u] == low[u]) { // 找到该连通分量的最高点
        ++cnt;              //强连通分量的序号
        int x;              //临时变量x,用于枚举栈中当前强连通分量中每个节点
        do {
            x = stk[top--];    //弹出节点
            in_stk[x] = false; //标识不在栈中了
            id[x] = cnt;       //记录id号
            sz[cnt]++;         //这个强连通分量中节点的个数+1
        } while (x != u);      //将此强连通分量中的所有节点处理完，都出栈
    }
}
```

缩点：
```C++
for 每个顶点u
	for u的每个邻点v
		if u和v不在同一个强连通分量中
			添加一条从id[u]到id[v]的有向边。
```

拓扑序:做完Tarjan算法后，进行缩点之后的强连通分量已经按照其编号递减的顺序完成拓扑序。因为深度优先搜索把一个强连通分量中所有点找出之后，会给它们设置同一个编号。并且当前编号的强连通分量不会有其它还未搜索到的后继强连通分量，也就是说它的后继都已经处理完成。因为在Tarjan算法中，后继结点后入栈，所以会先被弹出栈进行处理。

- AcWing 1174 最受欢迎的牛：求强连通分量scc + 缩点 + 拓扑序。    
参考：AcWing 1174 受欢迎的牛(tarjan dfs序时间戳+缩环成点)：https://www.acwing.com/solution/content/20678/

1. 如果在这个拓扑图中至少存在两个点的出度为0（终点无后继），那么就不存在被除自己之外的所有牛认为是受欢迎的牛，至少有另外一个终点没有认为它受欢迎。
2.如果只存在一个点其出度为0，那么这个点所代表的强联通分量中的所有牛都是被除自己之外的所有牛认为是受欢迎的牛。因为在该强连通分量内部，所有点都是连通的；在该强连通分量外部，这种关系是具有传递性，最终会沿着拓扑序走到该强连通分量。
```C++
//缩点
//遍历所有点，计算强连通分量的出度
for(int u = 1; u <= n; u++)
	for(int i = h[u]; ~i; i = ne[i])
	{
		int v = e[i];
		//u和v不属于同一个强连通分量
		if(id[u] != id[v]) dout[id[u]]++;
	}

```
- AcWing 367 学校网络 
AcWing 367 学校网络(tarjan+加边缩点证明):https://www.acwing.com/file_system/file/content/whole/index/content/1437715/ 
p个起点(入度=0) q个终点(出度=0)，则初始起码每个起点都要发(因为起点不能被其他点传递到)>=p
最少加max(p,q)个边得到强连通分量
- AcWing 1175 最大半连通子图：
AcWing 1175 最大半连通子图(tarjan缩点拓扑序dp+解释为什么tarjan是逆dfs序) :https://www.acwing.com/solution/content/24796/
半连通 u -> v 或 v -> u，强连通分量必然是半连通。
先把所有强连通求出来并缩点后得到拓扑图：
1 tarjan求scc 
2 缩点 建图 给边判重。对于选中的两点,他们之间的边要么全选,要么一条都不选。建图时要给边判重。
3 按拓扑序递推：求最大半连通子图  <=>  求最长无分叉链(有分叉时scc3不能走到scc6 scc6也不能走到scc3)   
链上权重是连通分量里节点数量
4 统计最长链方案数：统计最长链方案数  <=> 拓扑图求最长路(权重是结点数,权重越大,结点数越多)

```C++
权重最大值         f[i]
让f[i]最大的方案数 g[i]
强连通分量i结点数  s[i]
j ↘
o  → i  if f[j]+s[i] > f[i]:
o ↗         更新路径权重f[i]=f[j]+s[i]
             更新方案数g[i]=g[j]
        if f[j]+s[i] == f[i]:
            不用更新路径权重
            只更新方案数g[i]+=g[j]
			
unordered_set<LL> S;// 边是(u,v) hash后 u*1000000+v
// 边判重
LL hash = a*1000000ll+b;
// 如果a和b不在一个强连通分量 且 边(a,b)没被加过
if(a!=b && !S.count(hash))
```
- AcWing368 银河：
AcWing 368 银河(tarjan+topo图 做差分约束问题)： https://www.acwing.com/solution/content/24811/
N 颗恒星的亮度值总和至少有多大;求最小->求所有下界的最大->最长路 √;求最大->求所有上界的最小->最短路

```C++
spfa最长路 - 做完后每个点的距离就是最小值    
1 边是正的 - 存在正环 => 无解       
2 有解:必须有绝对值;超级源点(能到所有边);x[i]≥x[0]+1
不同于糖果用栈保证spfa的时间复杂度O(n),这里用强连通分量保证时间复杂度 

1 首先用tarjan求scc,一个正环一定是某一个scc当中的,对于一个scc中的所有边,只要一个边的权重是严格>0,
如 u + w → v,w>0   
又u和v 在一个scc中,则v也一定能到u(且w[v][u]>=0(因为我们的不等式约束得到的)), 即只要scc中有一个边>=0 就必然存在正环
则 scc中无正环 <=> scc中的边==0 <=> scc中所有点相同(由不等式知双向边==0时 A==B )  <=> 可近似看成一个点
那么当没有正环时,经过tarjan后的图就是topo图
x[i]最小 <=> 求 x[i]下界最大 <=> 求最长路dist[i]
1 tarjan   
2 缩点+建图    
3 topo序dp最长路   

// 结果 = 新图里每个scc的距离 * scc里的点数 = dist[scc] * cnt[scc] 
for(int i=1;i<=scc_cnt;i++) res+=(LL)dist[i]*scc_size[i];
```


#### 无向图的强连通分量

AcWing 395 冗余路径(tarjan无向图边双连通分量 桥)： https://www.acwing.com/solution/content/20697/

