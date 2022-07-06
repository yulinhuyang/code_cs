
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

**AcWing 274 移动服务 **

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
