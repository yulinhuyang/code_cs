
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



