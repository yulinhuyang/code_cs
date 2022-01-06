最长回文子串，# ，中心展开法

##### 416. 分割等和子集

dp[i][j]含义

i是前i个物品(硬币)代表选择, j是限制容量（目标金额）代表限制条件,dp[i][j]最大价值或方法数

0-1背包: 最大价值, 变形子集背包

完全(无限)背包：零钱问题

```cpp
class Solution {
public:
    bool canPartition(vector<int> &nums) {
        int m = nums.size();
        if (m < 2) {
            return false;
        }
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum & 1) {
            return false;
        }
        int target = sum / 2;
        int maxNum = *max_element(nums.begin(), nums.end());
        if (maxNum > target) {
            return false;
        }
        vector<vector<int>> dp(m + 1, vector<int>(target + 1, 0));
        for (int i = 1; i < m + 1; i++) {
            dp[i][0] = true;
        }
        dp[0][0] = true;
        dp[1][nums[0]] = true;
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < target + 1; j++) {
                if (j < nums[i - 1]) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j] | dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        return dp[m][target];
    }
};
```
##### 1143. 最长公共子序列

128 最长连续序列 LCS ： set中心展开法

300 最长递增子序列 LIS: insertsort dp()

1143 最长公共子序列 LCS ；二维dp

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size();
        int n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j],dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
};

```



