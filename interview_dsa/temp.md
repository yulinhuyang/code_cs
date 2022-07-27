
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
static int temp[N][N];
    memset(temp,0x3f,sizeof temp);
    for(int k=1;k<=n;k++)
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                temp[i][j]=min(temp[i][j],a[i][k]+b[k][j]);

```




AcWing346 走廊泼水节


AcWing352 闇の連鎖


AcWing361 观光奶牛

AcWing362 区间


AcWing367 学校网络

AcWing368 银河

AcWing372 棋盘覆盖


AcWing376 机器任务

AcWing378 骑士放置



						   
						   
						   
