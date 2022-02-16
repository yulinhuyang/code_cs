当空间优化为1维之后，只有完全背包问题的体积是从小到大循环的。

完全背包：求所有前缀的最大值

多重背包：求滑动窗口内的最大值

多重背包问题3：dp[i][j] = max(dp[i-1][j], dp[i-1][j-v] + w, dp[i-1][j-2*v] + 2*w,..., dp[i-1][j-k*v] + k*w)

多重背包问题3单调队列优化： https://www.acwing.com/solution/content/6500/


for 物品：   
	for 体积：  
		for 决策：  
