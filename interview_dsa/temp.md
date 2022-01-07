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
##### 438. 找到字符串中所有字母异位词

滑窗1：先right,后缩left，最小覆盖子串，最长无重复子串

滑窗2：先right,left同步前进，字符串排列，找所有字母异位词

简化滑窗：找所有字母异位词，统计 vector<int> sCount(26), vector<int> 可以直接比较相等关系

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        unordered_map<char,int> need;
        for(auto ch:p){
            need[ch]++;
        }

        vector<int> res;
        unordered_map<char,int> window;
        int left = 0;
        int right = 0;
        int validNum = 0;
        while(right < s.size()){
            char ch_right = s[right];
            right++;
            if(need.count(ch_right)){
                window[ch_right]++;
                if(window[ch_right] == need[ch_right]){
                    validNum++;
                }
            }

            while(right - left >= p.size()){
                if(validNum == need.size()) {
                    res.emplace_back(left);
                }
                char ch_left = s[left];
                left++;
                if(need.count(ch_left))
                {
                    if(window[ch_left] == need[ch_left]){
                        validNum--;
                    }
                    window[ch_left]--;
                }
            }
        }
        return res;
    }
};
```
简单滑窗

vector<int> 可以直接比较相等关系

vector<int> sCount(26,0); 

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int pLen = p.size();
        if(s.size() < pLen){
            return {};
        }
        vector<int> sCount(26,0);
        vector<int> pCount(26,0);
        for(int i = 0;i < p.size();i++){
            sCount[s[i] - 'a']++;
            pCount[p[i] - 'a']++;
        }
        vector<int> res;
        if(sCount == pCount){
            res.emplace_back(0);
        }
        for(int i = 0;i < s.size() - pLen;i++){
            --sCount[s[i] - 'a'];
            ++sCount[s[i + pLen] - 'a'];
            if(sCount == pCount){
                res.emplace_back(i + 1);
            }
        }
        return res;
    }
};
```
### 448. 找到所有数组中消失的数字

hash: map ->vector 简化->原地修改简化为自己

```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n = nums.size();
        for(auto num:nums){
            int index = (num - 1) % n;
            nums[index] += n;
        }
        vector<int> res;
        for(int i = 0;i < nums.size();i++){
            if(nums[i] <= n){
                res.emplace_back(i + 1);
            }
        }
        return res;
    }
};
```
