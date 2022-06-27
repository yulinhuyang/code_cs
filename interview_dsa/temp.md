
**AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】**

https://www.acwing.com/solution/content/56348/

挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优     
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1    

https://www.bilibili.com/video/BV1KE411V79h     



**AcWing 164 可达性统计**

拓扑排序模板题 +状态压缩(bitset) 模板题

拓扑排序:

广度优先遍历队列中的元素关于层次满足两段性和单调性:         
1 在访问完所有第i层的节点后，才会访问第i+1层节点        
2 任意时候，队列中最多有两个层次的节点(i层和i+1层)。           

https://www.acwing.com/solution/content/32117/

从点x出发的点构成的集合是f(x),则f(x) = {x} U ( Uf(y),存在有向边(x,y))，也就是x的各个后续节点y出发能到达点的并集，再加上x自身。   
先求拓扑序，然后按照拓扑序倒序计算。拓扑排序中，对于任意一条边(x,y),x都在y之前。      

bitset: 支持 |、cout()

```cpp
bitset<N> f[N]; //N位的bitset对象 构成的数组

for (int i = n - 1; i >= 0; i--) {
	int j = seq[i];
	f[j][j] = 1;  // j这个点可以到达自己   f[j][j] =表示从 j出发的点，
				  // 能够到的点（1表示可以到， 0表示不能到），j可以到自己，
				  // 因此f[j][j]=1
	for (int k = h[j]; ~k; k = ne[k]) {  //所有能到到达的点
		f[j] |= f[e[k]];  // j这个点可以到达的点的数量= {j} U {y1} U {y2}
						  // ... {yn}
	}
}

for (int i = 1; i <= n; i ++ ) cout << f[i].count() << endl;
```

```cpp
bitset<16> third("1110011");  //将二进制字符串初始化到对象中
cout << third.count() << endl;  //统计bitset里面1的位数
cout << first.to_string() << endl;   //以二进制字符串形式输出，将所有二进制位输出

```

**AcWing 165 小猫爬山** 

https://www.acwing.com/solution/content/32118/

优化搜索顺序：贪心降序排列，先运大的猫     
可行性剪枝： if (cab[i] + cat[now] <= w)  不超过车承重     
最优性剪枝： if (cnt >= ans)  return          	


**AcWing 166 数独**

数独 舞蹈链解法 Dancing links解法：https://www.acwing.com/solution/content/3843/
位运算 + dfs解法：https://www.acwing.com/solution/content/31873/

打表: 查表法是一种在某些条件下简化算法的办法,通过打表技巧获得一个有序表或常量表。  

位为1表示可选，待填充；为0表示已填充。  

```cpp
//标记数组
int row[N], col[N], cell[3][3];/
对每行、列和九宫格，分别用一个9位二进制数保持哪些数字可以填，对应位是1，该数可以填，回溯恢复现场修改。 

//打表
for (int i = 0; i < N; i++) {
	map[1 << i] = i; //打表，快速知道是哪一个数字
}
for (int i = 0; i < 1 << N; i++) {
	int s = 0;
	for (int j = i; j; j -= lowBit(j)) s++;
	ones[i] = s; //记录每个状态有多少个1
}


//位运算的映射与反映射
row[i] = col[i] = (1 << N) - 1;  //初始化为全1
int t = str[k] - '1'; 
row[i] -= 1 << t;    //注意第4位对应1 << 3   
```

优化搜索顺序：dfs优先选一个1的个数最少的，这样的分支数量最少。依次做lowbit操作，选择每个分支。   
可行性剪枝：if (dfs(cnt - 1)) return true;   
排除等效冗余：避免重复遍历若干棵覆盖同一状态空间的等效搜索树。   
		


**AcWing167 木棍**

https://www.acwing.com/solution/content/36030/

优化搜索序列:优先选择较长的木棒
排除等效冗余：
1 先后加入的木棒具有单调性  
2 对于当前木棒，如果拼接失败，不能再尝试和他等长的木棒。
3 第一次尝试拼入木棒就递归失败，那么后面必然失败。
4 如果最后一个木棒失败，则一定失败。   

截止条件：双阈值，u 组成的木棍数量，cur 已经拼接的长度  


```cpp
bool dfs(int u, int cur, int start) {
    if (u * length == sum) return true;
    if (cur == length) return dfs(u + 1, 0, 0);

    for (int i = start; i < n; i++) {
        if(st[i]) continue;
        int l = sticks[i];
        if (cur + l <= length) {
            st[i] = true;
            if (dfs(u, cur + l, i + 1)) return true;
            st[i] = false;
            if (!cur) return false; //第一次尝试失败，后面也会失败
            if (cur + l == length) return false; //最后一根失败了，也会失败
            int j = i;
            while (j < n && sticks[j] == l) j++; //等长的木棍都会失败，跳过
            i = j - 1;
        }
    }
    return false;
}
```

**AcWing 168 生日蛋糕**  

https://www.acwing.com/solution/content/31876/

https://img-blog.csdnimg.cn/20210127003014554.png

总体积n，层数m，表面积S最小

优化搜索顺序(二维,影响性大到小原则)：层间，从下到上;层内，先枚举半径再枚举高(R影响大)，半径由大到小，高度由大到小。   
第u层： 
R:  u <= Ru <= min{Ru+1−1 ，(int)sqrt(n - v)}
H:  u <= Hu <= min{Hu+1−1， (n - v) / r / r)}
推导：根据表面积和体积关系，推导 S + 2(n−V)/Ru+1 >= Sans 剪枝  
最优性剪枝：上面(1~u-1) + 下面(u ~m) < res(面积最优解)


```cpp
void dfs(int u, int v, int s) {
    if (v + minv[u] > n) return;
    if (s + mins[u] >= ans) return;
    if (s + 2 * (n - v) / R[u + 1] >= ans) return;

    if (!u) {
        if (v == n) ans = s;
        return;
    }
    //枚举r ,h取最小u
    for (int r = min(R[u + 1] - 1, (int)sqrt(n - v)); r >= u; r--) {
        for (int h = min(H[u + 1] - 1, (n - v) / r / r); h >= u; h--) {
            int t = 0;
            if (u == m) t = r * r;
            R[u] = r;
            H[u] = h;
            dfs(u - 1, v + r * r * h, s + 2 * r * h + t);
        }
    }
}


```


**AcWing 170 加成序列**

迭代加深:在当前深度限制下搜不到答案，就把深度限制增加，重新进行一次搜索。先搜索浅层，再搜索深层。
     
https://www.acwing.com/solution/content/6859/   

迭代加深+dfs(双指针+剪枝)

```cpp
for (int i = u - 1; i >= 0; i -- )
	for (int j = i; j >= 0; j -- )
	{
		int s = path[i] + path[j];
		if (s > n || s <= path[u - 1] || st[s]) continue;
		st[s] = true;

		path[u] = s;
		if (dfs(u + 1, k)) return true;
	}
```


**AcWing 171 送礼物**

双向搜索：从初态和终态出个各搜索一半的状态，产生两棵深度减半的搜索树，在中间交会、合成最终的答案。 

https://www.acwing.com/solution/content/38250/

dfs1先搜索出前一半礼物(1 ~ N/2 + 2)选出若干，得到 0~W 之间，放入数组A。
dfs2再从后一半礼物(N/2 + 3 ~ N)中选出一些，达到的重量是t,数组A中二分出来一个 <= W - t 最大的，用两者和更新答案。

```cpp
void dfs2(int u, int s) {
    //截止条件
    if (u == n) {
        int l = 0, r = cnt - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (weights[mid] + (long long )s <= m) l = mid;
            else r = mid - 1;
        }
        if (weights[l] + (long long) s <= m) ans = max(ans, weights[l] + s);
        return;
    }
    //不选
    dfs2(u + 1, s);

    //选
    if ((long long) s + g[u] <= m) {
        dfs2(u + 1, s + g[u]);
    }
}
```
			     
**AcWing 172 立体推箱子**

https://www.acwing.com/solution/content/16264/

箱子共有三种状态：立着，记录为0; 竖躺着，记录为 1; 横躺着，记录为 2  

坐标记录：
对于立着的，坐标记为其所立的位置。
对于竖躺着的，坐标记为其靠上的方块的坐标。
对于横躺着的，坐标记为其靠左的方块的坐标。

广搜之前记录下三个状态扩展的偏移量。 

```cpp
dx[3][4] = {       // x 坐标的偏移量
    {-2, 0, 1, 0}, // 0 向上扩展，x - 2，向左 x 不变，向下 x + 1，向右 x 不变
    {-1, 0, 2, 0}, // 1，同上
    {-1, 0, 1, 0}  // 2
}, dy[3][4] = {    // y 坐标的偏移量
    {0, -2, 0, 1},
    {0, -1, 0, 1},
    {0, -1, 0, 2}
}, dl[3][4] = {    // lie 的偏移量
    {1, 2, 1, 2},
    {0, 1, 0, 1},
    {2, 0, 2, 0}
};
```

**AcWing 173 矩阵距离**

多源BFS 模板题

```cpp
memset(d, -1, sizeof(d));
queue<PII> q;
for (int i = 0; i < m; i++) {
	for (int j = 0; j < n; j++) {
		if (g[i][j] == '1') {
			q.push({i, j});
			d[i][j] = 0;
		};
	}
}
```   
		   
**AcWing 175 电路维修** 

https://www.acwing.com/solution/content/21775/

双端队列BFS    

区分格子的坐标和角点坐标    
点和格子双枚举：左上角，右上角，右下角，左下角     

```cpp
//对于点的顺时针枚举
int dx[4] = {-1, -1, 1, 1};
int dy[4] = {-1, 1, 1, -1};

//对于格子的枚举
int ix[4] = {-1, -1, 0, 0};
int iy[4] = {-1, 0, 0, -1};

char cs[] = "\\/\\/";
```

旋转电线，则从当前的点到想去的点边权是1，否则是0。  

每次从队头取出元素，并进行拓展其他元素时         
1 若拓展某一元素的边权是0，则将该元素插入到队头。       
2 若拓展某一元素的边权是1，则将该元素插入到队尾。    
在边权不同一的情况下，更新其他点的距离并不是最短距离，而出队的点的距离才是最短距离。        

```cpp
while (q.size())
{
	auto t = q.front();
	q.pop_front();

	int x = t.first, y = t.second;
	if (st[x][y]) continue;
	st[x][y] = true;  //出队的点的距离才是最短距离

	for (int i = 0; i < 4; i ++ )
	{
		int a = x + dx[i], b = y + dy[i];
		int j = x + ix[i], k = y + iy[i];
		if (a >= 0 && a <= n && b >= 0 && b <= m)
		{
			int w = 0;
			if (g[j][k] != cs[i]) w = 1; //对比格子的状态
			if (d[a][b] > d[x][y] + w)
			{
				d[a][b] = d[x][y] + w;
				if (w) q.push_back({a, b});
				else q.push_front({a, b});
			}
		}
	}
}

//点比格子数多，所以是m、n  
if (d[n][m] == 0x3f3f3f3f) return -1;
return d[n][m];
```
				
**优先队列BFS** 

带权图上求从起点到每个节点的最短路的两种方式：   
1 不能保证每个状态第一次入队就得到最小代价，允许一个状态被多次更新、进出队列，不断搜索直到队列为空。   
2 优先队列广搜。每次取出队列中代价最小的状态进行扩展。一个状态可能多次更新、多次进出队列，一个状态也可能以不同的代价在队列中同时存在。   
不过，每个状态第一次从队列中被取出时，就得到了从起始状态到该状态的最小代价。之后若再被取出，则可以直接忽略，不再扩展。优先队列BFS中每个状态只扩展一次。    


**BFS根据边权情况进行总结：**
- 1 问题只计最少步数，等价于在边权为1的图上求最短路。
使用普通的BFS,时间复杂度是O(n)
每个状态**只访问(入队)一次。第一次入队时**即为该状态的最少步数。
- 2 问题每次扩展的代价是0或1，等价于在边权只有0和1的图上求最短路。   
使用双端队列BFS,时间复杂度是O(N)    
每个状态被**更新(入队)多次，只扩展一次，第一次出队时**即为该状态的最小代价。    
- 3 问题每次扩展的代价是任意数值，等价于一般的最短路问题。    
(1) 使用优先队列BFS,时间复杂度是O(N logN)   
每个状态被**更新(入队)多次，只扩展一次，第一次出队**即为改状态的最小代价。   
(2) 使用迭代思想 + 普通的BFS,时间复杂度 O(N*N)
每个状态被更新(入队)、扩展(出队)多次，最终完成搜索后，记录数组中保存了最小代价。
				
				
**AcWing 176 装满的油箱** 

https://www.acwing.com/solution/content/16438/

堆优化的dijkstra + 双状态维护

重载 < 号，大根堆变成小根堆。         
维护状态(city,fuel) :城市编号u + 剩余油量c，扩展从两维扩展。  

分层图最短路问题,(x,c) --> 扩展 (x,c+1)，边权为px 代表加一单位油需要的花费;扩展邻边
 
```cpp
int dist[N][C];
bool st[N][C];

struct Ver
{
    int d, u, c;
    bool operator< (const Ver &W)const
    {
        return d > W.d;
    }
};

int dijkstra(int start, int end, int cap)
{
    memset(dist, 0x3f, sizeof dist);
    memset(st, false, sizeof st);
    priority_queue<Ver> heap;
    heap.push({0, start, 0});
    dist[start][0] = 0;

    while (heap.size())
    {
        auto t = heap.top(); heap.pop();

        if (t.u == end) return t.d;

        if (st[t.u][t.c]) continue;
        st[t.u][t.c] = true;
		
	//油箱未满，尝试扩展C  
        if (t.c < cap)
        {
            if (dist[t.u][t.c + 1] > t.d + price[t.u])
            {
                dist[t.u][t.c + 1] = t.d + price[t.u];
                heap.push({dist[t.u][t.c + 1], t.u, t.c + 1});
            }
        }
		
	//扩展邻边u
        for (int i = h[t.u]; ~i; i = ne[i])
        {
            int j = e[i];
            if (t.c >= w[i])
            {
                if (dist[j][t.c - w[i]] > t.d)
                {
                    dist[j][t.c - w[i]] = t.d;
                    heap.push({dist[j][t.c - w[i]], j, t.c - w[i]});
                }
            }
        }
    }

    return -1;
}
```
				
**AcWing177 噩梦**
  
https://www.acwing.com/solution/content/16498/   

双向BFS: 普通的求最少步数的双向bfs,需要从起始状态、目标状态分别开始，两边轮流进行，每次扩展一层。当两边各自有一个状态在记录数组
中发生重复时，说明两个搜索过程相遇了，可以合并得出起点到终点的最少步数。**入队的时候，比较和记录状态**。

```cpp
while (qb.size() || qg.size())
{
	step ++ ;
	for (int i = 0; i < 3; i ++ )
		for (int j = 0, len = qb.size(); j < len; j ++ )
		{
			auto t = qb.front();
			qb.pop();
			int x = t.first, y = t.second;
			if (!check(x, y, step)) continue;
			for (int k = 0; k < 4; k ++ )
			{
				int a = x + dx[k], b = y + dy[k];
				if (check(a, b, step))
				{
					if (st[a][b] == 2)// st的不同值对应途经状态
					{
						return step;
					}
					if (!st[a][b])
					{
						st[a][b] = 1;
						qb.push({a, b});
					}
				}
			}
		}

	for (int i = 0; i < 1; i ++ )
		for (int j = 0, len = qg.size(); j < len; j ++ )
		{
			auto t = qg.front();
			qg.pop();

			int x = t.first, y = t.second;
			if (!check(x, y, step)) continue;
			for (int k = 0; k < 4; k ++ )
			{
				int a = x + dx[k], b = y + dy[k];
				if (check(a, b, step))
				{
					if (st[a][b] == 1)
					{
						return step;
					}
					if (!st[a][b])
					{
						st[a][b] = 2;
						qg.push({a, b});
					}
				}
			}
		}
}

```

对比AcWing190 字串变换: 每次每边扩展完整一层



**AcWing 183  靶形数独**  


数独的位运算模板
 
```cpp
inline void draw(int x, int y, int t)
{
    int s = 1;
    if (t > 0) g[x][y] = t;
    else
    {
        s = -1;
        t = -t;
        g[x][y] = 0;
    }

    t--;
    row[x] -= (1 << t) * s;
    col[y] -= (1 << t) * s;
    cell[x / 3][y / 3] -= (1 << t) * s;
}


void dfs(int cnt, int score) {
    if (!cnt) {
        ans = max(ans, score);
        return;
    }
	
    //枚举最少选择的分支
    int minv = 10;
    int x, y;
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            if (!g[i][j]) {
                int t = ones[get(i, j)];
                if (t < minv) {
                    x = i, y = j;
                    minv = ones[get(i, j)];
                }
            }
        }
    }
	
	//依次做lowbit操作，选择每个分支
    for (int j = get(x, y); j; j -= lowbit(j)) {
        int t = map[lowbit(j)] + 1;
        draw(x, y, t);
        dfs(cnt - 1, score + get_score(x, y, t));
        draw(x, y, -t);
    }
}
```
	
**AcWing 184 虫食算**

https://www.acwing.com/solution/content/83532/   

N个字母并不一定顺序地代表0到N−1。   
q[u]来定位字母，path[q[u]]来得到字母的赋值结果,枚举u ；          
path置为-1为判断字母是否被赋值做铺垫；    

提前剪枝：直接用a+b>=n包含了a+b+0>=n和a+b+1>=n两种情况   

```cpp
bool check()
{
    for (int i = n - 1, t = 0; i >= 0; i -- )
    {
        int a = e[0][i] - 'A', b = e[1][i] - 'A', c = e[2][i] - 'A';    //转化 
        if (path[a] != -1 && path[b] != -1 && path[c] != -1)    //剪枝：判断一列的三个字母是否都确定 
        {
            a = path[a], b = path[b], c = path[c];
            if (t != -1)    //上一列字母全部确定 
            {
                if ((a + b + t) % n != c) return false;
                if (!i && a + b + t >= n) return false;     //第一列特判 
                t = (a + b + t) / n; 
            }
            else    //上一列字母中有没有确定的
            {
                if ((a + b + 0) % n != c && (a + b + 1) % n != c) return false;     //剪枝：若进位是0或1的两种情况取膜后均无法得到c则返回false 
                if (!i && a + b >= n) return false;     //第一列特判 
            }
        }
        else t = -1;
    }

    return true;    //历经百般磨难都没有false说明满足题意，成功返回true 
}

bool dfs(int u)
{
    if (u == n) return true;     

    for (int i = 0; i < n; i ++ )
        if (!st[i])
        {
            st[i] = true;   //某字母出现过 
            path[q[u]] = i;     //选择编号q[u](某字母)可能的数字i
            if (check() && dfs(u + 1)) return true;   //每次确定一个字母都进行check判断 
            st[i] = false;      //回溯 
            path[q[u]] = -1;
        }

    return false;   //已经判断过该组合无法满足题意，因此false 
}
```


**AcWing 186 巴士**

https://www.acwing.com/solution/content/4221/

剪枝策略：       
1 枚举组合数，为避免重复在DFS时传入当前枚举的起点。          
2 将所有等差数列按长度排序，优先枚举长度较长的等差数列，这样搜索树的前几层分支少，可以快速回溯。    
3 因为2，当前路线覆盖的点数是最多的，如果当前路线覆盖的点数*剩余可选的路径点数 + 当前已经覆盖的点数 < 总点数，则当前方案一定非法，直接回溯即可。      

```cpp
bool is_route(int a, int d) {
    for (int i = a; i < 60; i += d) {
        if (!bus[i]) {
            return false;
        }
    }
    return true;
}
//depth尝试线路数,u当前线路号，sum 当前累积点数，start可行路线
bool dfs(int depth, int u, int sum, int start) {
    if (depth == u) return sum == n;
    if (routes[start].first * (depth - u) + sum < n) return false;

    for (int i = start; i < routes.size(); i++) {
        auto r = routes[i];
        int a = r.second.first, d = r.second.second;
        if (!is_route(a, d)) continue;
        for (int j = a; j < 60; j += d) bus[j]--;
        if (dfs(depth, u + 1, sum + r.first, i)) return true;
        for (int j = a; j < 60; j += d) bus[j]++;
    }
    return false;
}

for (int i = 0; i < 60; i++) {
	for (int j = i + 1; i + j < 60; j++) {
		if (is_route(i, j)) {
			routes.push_back({(59 - i) / j + 1, {i, j}});//点数、起点、方差
		}
	}
}
```

**AcWing 187 导弹防御系统**

https://www.acwing.com/solution/content/4258/

dfs(LIS) + 迭代加深  

从前往后枚举每颗导弹属于某个上升子序列，还是下降子序列；    
如果属于上升子序列，则枚举属于哪个上升子序列（包括新开一个上升子序列）；如果属于下降子序列，可以类似处理。  
类似最长上升子序列问题 LIS II，分别记录每个上升子序列的末尾数up[]、和下降子序列的末尾数down[]，可以快速判断当前数接在某个序列的后面。  

贪心：当前数接在最大的数后面，一定不会比接在其他数列后面更差。这样up和down是单调的。  


```cpp
int h[N];
int down[N], up[N]; //因为贪心的性质，up和down是单调的
int n;

//su 上升序列数量， sd 下降序列数量
//up需要从1开始，保持数量和索引的一致
bool dfs(int depth, int u, int su, int sd) {
    if (su + sd > depth) return false;
    if (u == n) return true;

    //枚举放在上升子序列后面
    bool flag = false;
    for (int i = 1; i <= su; i++) {
        if (up[i] < h[u]) {
            int t = up[i];
            up[i] = h[u];
            if (dfs(depth, u + 1, su, sd)) return true;

            up[i] = t;
            flag = true;
            break;
        }
    }
    //新开一个
    if (!flag) {
        up[su + 1] = h[u];
        if (dfs(depth, u + 1, su + 1, sd)) return true;
    }

    //枚举放在下降子序列后面
    flag = false;
    for (int i = 1; i <= sd; i++) {
        if (down[i] > h[u]) {
            int t = down[i];
            down[i] = h[u];
            if (dfs(depth, u + 1, su, sd)) return true;
            down[i] = t;
            flag = true;
            break;
        }
    }
    if (!flag) {
        down[sd + 1] = h[u];
        if (dfs(depth, u + 1, su, sd + 1)) return true;
    }

    return false;
}
```

**AcWing188 武士风度的牛**

BFS的日字型遍历    

```cpp
int dx[8] = {-2, -1, 1, 2, 2, 1, -1, -2};
int dy[8] = { 1, 2, 2, 1, -1, -2, -2,-1};
```

**AcWing189 乳草的入侵**

https://www.acwing.com/solution/content/112028/  

8连通BFS遍历   
起点直角坐标转换数组坐标：平移到左下角(x = n - y)，交换x,y, 给出坐标(1,1)开始，所以 x = n + 1 - y    
```cpp
for (int x = t.first - 1; x <= t.first + 1; x++) {
	for (int y = t.second - 1; y <= t.second + 1; y++) {
	
	}
```

**AcWing190 字串变换**

https://www.acwing.com/solution/content/5434/  

双向BFS模板题      
扩展方式：分别枚举在原字符串中使用替换规则的起点，和所使用的的替换规则。  

```cpp
int extend(queue<string> &q, unordered_map<string, int> &da, unordered_map<string, int> &db, string a[N], string b[N]) {
    //出队同一层的元素的判断方法
    int d = da[q.front()];
    while (q.size() && da[q.front()] == d) {
        auto t = q.front();
        q.pop();

        //遍历变换规则
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < t.size(); j++) {
                if (t.substr(j, a[i].size()) == a[i]) {
                    string ne = t.substr(0, j) + b[i] + t.substr(j + a[i].size(), t.size());
                    if (db.count(ne)) return da[t] + db[ne] + 1;
                    if (da.count(ne)) continue;
                    da[ne] = da[t] + 1;
                    q.push(ne);
                }
            }
        }
    }
    return 11;
}

int bfs() {
    if (A == B) return 0;
    queue<string> qa, qb;
    unordered_map<string, int> da, db;
    qa.push(A);
    qb.push(B);
    da[A] = db[B] = 0;

    int step = 0;
    while (qa.size() && qb.size()) {
        int t = 0;
        if (qa.size() < qb.size()) t = extend(qa, da, db, a, b);
        else t = extend(qb, db, da, b, a);
        if (t <= 10) return t;
        if (++step == 10) return -1;
    }

    return -1;
}
```

