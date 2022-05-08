



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


AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】:https://www.acwing.com/solution/content/56348/

 567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
