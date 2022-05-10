



##### 剑指 Offer II 092. 翻转字符

状态机模型 + 滚动数组优化

```C++
class Solution {
public:
    int minCost(vector<vector<int>> &costs) {
        int m = costs.size();
        vector<vector<int>> f(2, vector<int>(3, 0));
        f[0][0] = costs[0][0];
        f[0][1] = costs[0][1];
        f[0][2] = costs[0][2];
        for (int i = 1; i < m; i++) {
            f[i & 1][0] = min(f[(i - 1) & 1][1], f[(i - 1) & 1][2]) + costs[i][0];
            f[i & 1][1] = min(f[(i - 1) & 1][0], f[(i - 1) & 1][2]) + costs[i][1];
            f[i & 1][2] = min(f[(i - 1) & 1][1], f[(i - 1) & 1][0]) + costs[i][2];
        }
        return min(min(f[(m - 1) & 1][0], f[(m - 1) & 1][1]), f[(m - 1) & 1][2]);
    }
};
```

##### Leetcode 416 分割等和子集

0-1背包模板题

	for 物品：   
		for 体积：  
			(for)决策：
```C++
//二维数组模板
class Solution {
public:
    bool canPartition(vector<int> &nums) {
        int sum = 0;
        int m = nums.size();
        for (auto &x:nums) sum += x;
        if (sum & 1) return false;
        sum /= 2;
        vector<vector<int>> f(m + 1, vector<int>(sum + 1, 0));
        for (int i = 0; i < m + 1; i++) {
            f[i][0] = true;
        }

        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < sum + 1; j++) { //j从0或1开始都可以，但不能从nums[i-1]开始
                f[i][j] = f[i - 1][j];
                if (j >= nums[i - 1]) {
                    f[i][j] |= f[i - 1][j - nums[i - 1]];
                }
            }
        }
        return f[m][sum];
    }
};
```

```C++
//一维简化，j逆序循环
class Solution {
public:
    bool canPartition(vector<int> &nums) {
        int sum = 0;
        int m = nums.size();
        for (auto &x:nums) sum += x;
        if (sum & 1) return false;
        sum /= 2;
        vector<int> f(sum + 1, 0);
        f[0] = 1;
        for (int i = 0; i < m; i++) {
            for (int j = sum; j >= nums[i]; j--) {
                f[j] |= f[j - nums[i]];
            }
        }
        return f[sum];
    }
};
```

##### Leetcode 494  目标和
```C++
//0-1背包  二维数组模板
class Solution {
public:
    int findTargetSumWays(vector<int> &nums, int target) {
        int sum = 0;
        for (auto &num:nums) sum += num;
        int diff = sum - target;
        if (diff < 0 || diff & 1) return false;
        int m = nums.size();
        diff /= 2;
        vector<vector<int>> f(m + 1, vector<int>(diff + 1));
        f[0][0] = 1;
        for (int i = 1; i < m + 1; i++) {
            for (int j = 0; j < diff + 1; j++) {//j从0或1开始都可以，但不能从nums[i-1]开始
                f[i][j] = f[i - 1][j];
                if (j >= nums[i - 1]) f[i][j] += f[i - 1][j - nums[i - 1]];
            }
        }
        return f[m][diff];
    }
};
```

```C++
//0-1背包  一维模板
class Solution {
public:
    int findTargetSumWays(vector<int> &nums, int target) {
        int sum = 0;
        for (auto &num:nums) sum += num;
        int diff = sum - target;
        if (diff < 0 || diff & 1) return false;
        int m = nums.size();
        diff /= 2;
        vector<int> f(diff + 1, 0);
        f[0] = 1;
        for (int i = 1; i < m + 1; i++) {
            for (int j = diff; j >= nums[i - 1]; j--) {
                f[j] = f[j] + f[j - nums[i - 1]];
            }
        }
        return f[diff];
    }
};

```
##### 377  组合总和 Ⅳ

AcWing 3. 完全背包问题:https://www.acwing.com/solution/content/5345/     
【宫水三叶】本题与完全背包的区别: https://leetcode.cn/problems/combination-sum-iv/solution/gong-shui-san-xie-yu-wan-quan-bei-bao-we-x0kn/     


```C++
//原始模板
class Solution {
public:
    int combinationSum4(vector<int> &nums, int target) {
        int m = nums.size();
        int len = target;
        //定义f[i][j]为组合长度为i，凑成总和为j的方案数是多少
        vector<vector<unsigned long long>> f(len + 1, vector<unsigned long long>(target + 1, 0));
        f[0][0] = 1;
        int res = 0;
        //len最大选择的长度
        for (int i = 1; i < len + 1; i++) {
            for (int j = 0; j < target + 1; j++) {
                for (auto x:nums) {
                    if (j >= x) f[i][j] = f[i][j] + f[i - 1][j - x];
                }
            }
            res += f[i][target];
        }

        return res;
    }
};

//一维数组优化
class Solution {
public:
    int combinationSum4(vector<int> &nums, int target) {
        int m = nums.size();
        int len = target;
        vector<unsigned long long> f(target + 1, 0);
        f[0] = 1;
        for (int i = 1; i < target + 1; i++) {
            for (auto x : nums) {
                if (i >= x) f[i] = f[i] + f[i - x];
            }
        }

        return f[target];
    }
};
```

背包模板总结：

//加入template

闫氏dp分析法：https://www.cnblogs.com/IzayoiMiku/p/13635809.html   

https://blog.csdn.net/weixin_44289697/article/details/105125613   

0-1 背包:  

```C++
//二循环二维
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= m; j++) {
        // 当前背包容量装不进第i个物品，则价值等于前i-1个物品
        if (j < v[i]) {
            f[i][j] = f[i - 1][j];
        }
         // 能装，需进行决策是否选择第i个物品
        else {
            f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]);
        }
    }
}
//二循环一维优化
for (int i = 1; i <= n; i++) {
    for (int j = m; j >= v[i]; j--) {
        f[j] = max(f[j], f[j - v[i]] + w[i]);
    }
}
```

完全背包： 

完全背包优化：三循环二维数组--> 二循环二维数组--> 二循环一维数组
  
```C++
//三循环二维数组
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= m; j++) {
        for (int k = 0; k * v[i] <= j; k++) {
            f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
        }
    }
}
//二循环二维数组
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= m; j++) {
        f[i][j] = f[i - 1][j]; //不选
        if (j - v[i] >= 0)
            f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);
    }
}

//一维优化
for (int i = 1; i <= n; i++) {
    for (int j = v[i]; j <= m; j++) {
        f[j] = max(f[j], f[j - v[i]] + w[i]);
    }

}
```

分组背包：

```C++
f[N][N]; //只从前i组物品中选，当前体积小于等于j的最大值,每个组只能选一个。
//三循环二维,
for (int i = 1; i <= n; i++) {
	for (int j = 0; j <= m; j++) {
		f[i][j] = f[i - 1][j];  //不选
		for (int k = 0; k < s[i]; k++) {  //
			if (j >= v[i][k]) f[i][j] = max(f[i][j], f[i - 1][j - v[i][k]] + w[i][k]);
		}
	}
}
```

多重背包:   
 
第i个物品选0~s[i]个

```C++
//三重循环二维
// f[i][j]表示从前i个物品中选,总体积不超过j的方案的最大价值
// 集合划分:第i个物品选0~s[i]个
for (int i = 1; i <= n; i ++ )
    for (int j = 0; j <= m; j ++ )
        for (int k = 0; k <= s[i] && k * v[i] <= j; k ++ ) //比完全背包的三重循环二维数组版本多了k <= s[i]的限制
            f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
 

//三循环一维
for (int i = 1; i <= n; i ++ ) //循环各组
    for (int j = m; j >= 0; j -- )
        for (int k = 0; k < s[i]; k ++ ) //组内循环
            if (v[i][k] <= j)
                f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);

```

混合背包：01背包(1类物品用1次) + 完全背包(2类物品无限用) + 多重背包(3类物品用si次)
```C++
for (int i = 0; i < n; i++) {
	if (!s[i]) {
		for (int j = v[i]; j <= m; j++) //完全背包
			f[j] = max(f[j], f[j - v[i]] + w[i]);
	} else {
		if (s[i] == -1) s[i] = 1;//0-1背包
		for (int k = 1; k <= s[i]; k *= 2) {
			for (int j = m; j >= k * v[i]; j--)
				f[j] = max(f[j], f[j - k * v[i]] + k * w[i]);
			s[i] -= k;
		}
		if (s[i])//多重背包
		{
			for (int j = m; j >= s[i] * v[i]; j--)
				f[j] = max(f[j], f[j - s[i] * v[i]] + s[i] * w[i]);
		}
	}
}
```

二维费用背包

```C++
//f[i][j][k] 表示考虑前i个物品，且容量不超过j，总重量不超过k的集合下能获得的最大价值
//f[j][k],01背包问题的一维数组的二维化，一维是体积，二维是质量
for (int i = 0; i < n; i++) {
	for (int j = V; j >= v[i]; j--) {
		for (int k = M; k >= m[i]; k--) {
			f[j][k] = max(f[j][k], f[j - v[i]][k - m[i]] + w[i]);
		}
	}
}
```


对比模板：

二重循环二维数组对比：


f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]); //01背包
f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]); //完全背包问题
```

三循环二维数组对比：

```C++
//完全背包
for (int k = 0; k * v[i] <= j; k++) { 
	f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
}

//分组背包
f[i][j] = f[i - 1][j];  //不选
for (int k = 0; k < s[i]; k++) {   
	if (j >= v[i][k]) f[i][j] = max(f[i][j], f[i - 1][j - v[i][k]] + w[i][k]);
}

//多重背包
for (int k = 0; k <= s[i] && k * v[i] <= j; k ++ ) //比完全背包的三重循环二维数组版本多了k <= s[i]的限制
	f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
```



True、False问题：dp[i] = dp[i] or dp[i-num]     
最大最小问题：dp[i] = min(dp[i], dp[i-num]+1)或者dp[i] = max(dp[i], dp[i-num]+1)     
组合问题公式：dp[i] += dp[i-num]     


AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】:https://www.acwing.com/solution/content/56348/

 567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
