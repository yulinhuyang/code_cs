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

##### 437. 路径总和 III

dfs：返回值，全局变量

两个点：visited数组、pop撤销

TreeNode * t1 = new TreeNode(10);

```cpp
class Solution {
    unordered_map<long long, int> prefix;
    int path = 0;

    void dfs(TreeNode *root, long long curSum, int targetSum) {
        if (!root) {
            return;
        }
        curSum += root->val;
        if (prefix.find(curSum - targetSum) != prefix.end()) {
            path += prefix[curSum - targetSum];
        }
        prefix[curSum]++;
        dfs(root->left, curSum, targetSum);
        dfs(root->right, curSum, targetSum);
        prefix[curSum]--;
        return;
    }

public:
    int pathSum(TreeNode *root, int target) {
        prefix[0] = 1; //必须有
        dfs(root, 0, target);
        return path;
    }
};
```

##### 3. 无重复字符的最长子串

cpp多用++，少+=1

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map <char,int> dict;
        int left = 0;
        int right = 0;
        int maxlen = 0;
        while(right < s.size()){
            char sright = s[right];
            right++;;
            dict[sright]++;;

            while(dict[sright] > 1){
                char sleft = s[left];
                left++;
                dict[sleft]--;
            }
            maxlen = max(maxlen,right - left);
        }
        return maxlen;
    }
};
```



