
牛蛙点点提高课题解：https://www.acwing.com/user/myspace/activity/52559/

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



