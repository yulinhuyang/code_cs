##### LeetCode554 砖墙 
```C++
class Solution {
public:
    int leastBricks(vector<vector<int>> &wall) {
        unordered_map<int, int> hash;
        int res = 0;
        for (auto brick:wall) {
            int s = 0;
            for (int i = 0; i < brick.size() - 1; i++) {
                s += brick[i];
                res = max(res, ++hash[s]);
            }
        }

        return wall.size() - res;
    }
};
```

##### LeetCode 149 直线上最多的点数	
```C++
class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        if(points.empty()) return 0;
        int res = 1;
        for(int i = 0;i < points.size();i++){
            int duplicates = 0,vertical = 1;
            unordered_map<long double, int> hash;
            for (int j = i + 1; j < points.size(); j++) {
                if (points[i][0] == points[j][0]) {
                    vertical++;
                }
            }
            for (int j = i + 1; j < points.size(); j++) {
                if (points[i][0] != points[j][0]) {
                    long double slope = (long double) (points[i][1] - points[j][1]) / (points[i][0] - points[j][0]);
                    if (hash[slope] == 0) hash[slope] = 2;
                    else hash[slope]++;
                    res = max(res, hash[slope]);
                }
            }
            res = max(res,vertical);
        }

        return res;
    }
};

```

##### LeetCode 290 单词规律 
```C++
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        stringstream raw(s);
        vector<string> words;
        string word;
        while (getline(raw, word, ' ')) {
            words.emplace_back(word);
        }
        if (words.size() != pattern.size()) {
            return false;
        }
        unordered_map<string, char> SP;
        unordered_map<char, string> PS;
        for (int i = 0; i < words.size(); i++) {
            if (!SP.count(words[i])) SP[words[i]] = pattern[i];
            if (!PS.count(pattern[i])) PS[pattern[i]] = words[i];
            if (SP[words[i]] != pattern[i]) return false;
            if (PS[pattern[i]] != words[i]) return false;
        }
        return true;
    }
};
```
##### LeetCode 350 两个数组的交集 
```C++
class Solution {
public:
    vector<int> intersect(vector<int> &nums1, vector<int> &nums2) {
        multiset<int> mulSet;
        vector<int> res;
        for (auto num:nums1) mulSet.insert(num);
        for (auto num:nums2) {
            if (mulSet.count(num)) {
                res.emplace_back(num);
                mulSet.erase(mulSet.find(num));
            }
        }
        return res;
    }
};
```

##### LeetCode 525 连续数组	 
```C++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int res = 0,cnt = 0;
        unordered_map<int, int> hash;
        hash[0] = -1;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i]) {
                cnt++;
            } else {
                cnt--;
            }
            if (hash.count(cnt)) {
                res = max(res, i - hash[cnt]);
            } else {
                hash[cnt] = i;
            }
        }
        return res;
    }
};
```

##### LeetCode 454 四数相加 II	 
```C++
class Solution {
public:
    int fourSumCount(vector<int> &nums1, vector<int> &nums2, vector<int> &nums3, vector<int> &nums4) {
        unordered_map<int, int> hash;
        for (auto a:nums1) {
            for (auto b:nums2) {
                hash[a + b]++;
            }
        }
        int res = 0;
        for (auto c:nums3) {
            for (auto d:nums4) {
                if (hash.count(-c - d)) {
                    res += hash[-c - d];
                }
            }
        }
        return res;
    }
};
```

##### LeetCode 496 下一个更大元素 I	
```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int> &nums1, vector<int> &nums2) {
        unordered_map<int, int> hash;
        stack<int> stk;
        vector<int> res;
        for (int i = nums2.size() - 1; i >= 0; i--) {
            while (stk.size() && stk.top() < nums2[i]) stk.pop();
            hash[nums2[i]] = stk.size() ? stk.top() : -1;
            stk.push(nums2[i]);
        }
        for (auto num: nums1) res.emplace_back(hash[num]);
        return res;
    }
};

```
##### LeetCode 503 下一个更大元素 II
```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int> &nums) {
        int m = nums.size();
        stack<int> stk;
        vector<int> res(m, -1);
        for (int i = 0; i < 2 *m; i++) {
            while (stk.size() && nums[stk.top()] < nums[i % m]) {
                res[stk.top()] = nums[i % m];
                stk.pop();
            }
            stk.push(i % m);
        }
        return res;
    }
};

```

##### Leetcode 131. 分割回文串

```C++
class Solution {
    vector<vector<string>> ans;
    vector<string> path;
    vector<vector<int>> f;
public:
    vector<vector<string>> partition(string s) {
        int n = s.size();
        f = vector<vector<int>>(n, vector<int>(n, 1));
        for (int i = n - 1; i >= 0; i--) { //注意循环顺序
            for (int j = i; j < n; j++) {
                if (i == j) f[i][j] = 1;
                else f[i][j] = (s[i] == s[j]) && f[i + 1][j - 1];
            }
        }
        dfs(s, 0);
        return ans;
    }

    void dfs(string s, int u) {
        if (u == s.size()) {
            ans.emplace_back(path);
            return;
        }

        for (int i = u; i < s.size(); i++) {
            if (f[u][i]) {
                path.emplace_back(s.substr(u, i - u + 1));
                dfs(s, i + 1);
                path.pop_back();
            }
        }
    }
};

```

##### LeetCode 214 最短回文串

```cpp
//RK字符串hash  哈希检索算法（Robin-Karp，简称 RK 算法）
class Solution {
public:
    string shortestPalindrome(string s) {
        int n = s.size();
        int base = 131, mod = 1000000007;
        int left = 0, right = 0, mul = 1;
        int best = -1;
        for (int i = 0; i < n; i++) {
            left = ((long long) left * base + s[i]) % mod;
            right = (right + (long long) mul * s[i]) % mod;
            if (left == right) {
                best = i;
            }
            mul = ((long long)mul * base)% mod;
        }
        string add = best == n - 1 ? "" : s.substr(best + 1, n);
        reverse(add.begin(),add.end());
        return add + s;
    }
};

//kmp法
class Solution {
public:
    string shortestPalindrome(string s) {
        int n = s.size();
        string t(s.rbegin(), s.rend());
        s = ' ' + s + "#" + t;
        vector<int> ne(2 * n + 2);
        for (int i = 2, j = 0; i < 2 * n + 2; i++) {
            while (j && s[i] != s[j + 1]) j = ne[j];
            if (s[i] == s[j + 1]) j++;
            ne[i] = j;
        }
        int len = ne[2 * n + 1];
        string left = s.substr(1, len);
        string right = s.substr(1 + len, n - len);
        return string(right.rbegin(), right.rend()) + left + right;
    }
};

```

股票问题：贪心法、dp状态机法(carl一维数组-->buy(0) + sell(1)简化，carl二维数组--> buy + sell数组简化)

买入股票的状态不代表当天一定买入股票，是一种可能的状态

LeetCode买卖股票问题——模版总结(简化dp)：https://www.acwing.com/blog/content/526/

代码随想录股票问题(二维dp)：https://programmercarl.com


##### LeetCode 121. 买卖股票的最佳时机

```C++
//贪心
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int res = 0, minPrice = INT_MAX;
        for (auto &price:prices) {
            res = max(res, price - minPrice);
            minPrice = min(price, minPrice);
        }
        return res;
    }
};
//一维dp -->简化buy sell
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int n = prices.size();
        int buy = -prices[0], sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            buy = max(buy, -prices[i]);
            sell = max(sell, buy + prices[i]);
        }
        return sell;
    }
};
```

##### LeetCode 122. 买卖股票的最佳时机 II

```C++
//贪心
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int res = 0;
        for (int i = 0; i < prices.size() - 1; i++) {
            if (prices[i + 1] > prices[i]) {
                res += prices[i + 1] - prices[i];
            }
        }
        return res;
    }
};
//dp
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int n = prices.size();
        int buy = -prices[0], sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            buy = max(buy, sell - prices[i]);
            sell = max(sell, buy + prices[i]);
        }
        return sell;
    }
};
```

##### LeetCode 123. 买卖股票的最佳时机 III

一天一共就有五个状态，

没有操作、第一次买入、第一次卖出、第二次买入、第二次卖出

```C++
//dp
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int buy1 = -prices[0], sell1 = 0;
        int buy2 = -prices[0], sell2 = 0;
        //先买才有卖
        for (int i = 1; i < prices.size(); i++) {
            buy1 = max(buy1, -prices[i]);
            sell1 = max(sell1, buy1 + prices[i]);
            buy2 = max(buy2, sell1 - prices[i]);
            sell2 = max(sell2, buy2 + prices[i]);
        }
        return sell2;
    }
};
```

##### LeetCode 188. Best Time to Buy and Sell Stock IV

vector<vector<int>> dp(prices.size(), vector<int>(2 * k + 1, 0)); 二维压缩成一维---> dp[2*k + 1]

```C++
class Solution {

public:
    int maxProfit(int k, vector<int> &prices) {
        if(prices.empty()) return 0;
        if (k > prices.size()) {
            int res;
            //无限交易
            for (int i = 0; i < prices.size() - 1; i++) {
                if (prices[i] < prices[i + 1]) {
                    res += prices[i + 1] - prices[i];
                }
            }
            return res;
        }

        vector<int> buy(k + 1, INT_MIN);
        vector<int> sell(k + 1, 0);
        //状态机模型
        //先遍历价格，再遍历决策
        for (int i = 0; i < prices.size(); i++) {
            for (int j = 1; j < k + 1; j++) {
                buy[j] = max(buy[j], sell[j - 1] - prices[i]);
                sell[j] = max(sell[j], buy[j] + prices[i]);
            }
        }
        return sell[k];
    }
};
```
##### LeetCode 309. 最佳买卖股票时机含冷冻期

```C++
//dp
class Solution {

public:
    int maxProfit(vector<int> &prices) {
        if (prices.empty()) return 0;
        int n = prices.size();
        vector<vector<int>> f(n, vector<int>(3, 0));
        //0 持有，1不持有且冷冻，2 不持有且不冷冻
        f[0][0] = -prices[0];
        for (int i = 1; i < prices.size(); i++) {
            f[i][0] = max(f[i - 1][0], f[i - 1][2] - prices[i]);
            f[i][1] = f[i - 1][0] + prices[i];
            f[i][2] = max(f[i - 1][2], f[i - 1][1]);
        }
        return max(f[n - 1][1], f[n - 1][2]);
    }
};
//简化
class Solution {

public:
    int maxProfit(vector<int> &prices) {
        if (prices.empty()) return 0;
        int n = prices.size();
        //0 持有，1不持有且冷冻，2 不持有且不冷冻
        int buy = -prices[0], cool = 0, sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            int new_buy = max(buy, sell - prices[i]);
            int new_cool = buy + prices[i];
            int new_sell = max(sell, cool);
            buy = new_buy;
            cool = new_cool;
            sell = new_sell;
        }
        return max(sell, cool);
    }
};
```

##### 714. 买卖股票的最佳时机含手续费

```C++
class Solution {
public:
    int maxProfit(vector<int> &prices, int fee) {
        if (prices.empty()) return 0;
        int buy = -prices[0], sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            buy = max(buy, sell - prices[i]);
            sell = max(sell, buy + prices[i] - fee);
        }
        return sell;
    }
};
```

##### 213. 打家劫舍 II
                                          
```C++
1. 选头不选尾 2. 选尾不选头 3. 头尾都不选(包含在1、2中), 双向dp (left->right, right->left)

//双向(left、right)dp:头尾问题

class Solution {
public:
    int rob(vector<int> &nums) {
        int n = nums.size();
        if (!n) return 0;
        if (n == 1) return nums[0];
        //f抢，g不抢
        vector<int> f(n + 1, 0), g(n + 1, 0);

        //选头不选尾
        f[1] = nums[0],g[1] = INT_MIN;
        for (int i = 2; i < n + 1; i++) {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        int res = g[n];

        //选尾不选头
        f = vector<int>(n + 1, 0);
        g = vector<int>(n + 1, 0);
        for (int i = 2; i < n + 1; i++) {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        res = max(f[n],max(res, g[n]));

        return res;
    }
};                                          
                                          
```                                          
##### LeetCode 312. 戳气球
                             
经典区间dp                           
                             
```C++
class Solution {
public:
    int maxCoins(vector<int> &nums) {
        int n = nums.size();
        vector<int> p(n + 2, 0);
        p[0] = 1, p[n + 1] = 1;
        for (int i = 1; i < n + 1; i++) {
            p[i] = nums[i - 1];
        }
        //区间dp
        vector<vector<int>> f(n + 2, vector<int>(n + 2, 0));
        for (int len = 3; len <= n + 2; len++) {
            for (int l = 0; l + len - 1 < n + 2; l++) {
                int r = l + len - 1;
                for (int k = l + 1; k < r; k++) {
                    f[l][r] = max(f[l][r], f[l][k] + f[k][r] + p[l] * p[k] * p[r]);
                }
            }
        }
        return f[0][n + 1];
    }
};

```                             
                             
##### leetcode 475. 供暖器

```C++
class Solution {
    bool check(int mid, vector<int> &houses, vector<int> &heaters) {
        for (int i = 0, j = 0; i < houses.size(); i++) {
            while (j < heaters.size() && abs(houses[i] - heaters[j]) > mid) {
                j++;
            }
            if (j >= heaters.size()) return false;
        }
        return true;
    }

public:
    int findRadius(vector<int> &houses, vector<int> &heaters) {
        sort(houses.begin(), houses.end());
        sort(heaters.begin(), heaters.end());
        int l = 0, r = INT_MAX;
        while (l < r) {
            int mid = (long long)l + r >> 1;
            if (check(mid, houses, heaters)) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
```
##### LeetCode 4. 寻找两个正序数组的中位数
    
```C++    
class Solution {
    int findKthNumbers(vector<int> &nums1, int i, vector<int> &nums2, int j, int k) {
        //边界条件处理，根据数组大小换顺序
        if (nums1.size() - i > nums2.size() - j) {
            return findKthNumbers(nums2, j, nums1, i, k);
        }
        if (nums1.size() == i) return nums2[j + k - 1];
        if (k == 1) return min(nums1[i], nums2[j]);
        int m = nums1.size();
        int s1 = min(m, i + k / 2);
        int s2 = j + k / 2;
        if (nums1[s1 - 1] > nums2[s2 - 1]) {
            return findKthNumbers(nums1, i, nums2, j + k / 2, k - k / 2);
        } else {
            return findKthNumbers(nums1, s1, nums2, j, k - (s1 - i));
        }
    }

public:
    double findMedianSortedArrays(vector<int> &nums1, vector<int> &nums2) {
        int total = nums1.size() + nums2.size();
        if (total % 2 == 0) {
            int left = findKthNumbers(nums1, 0, nums2, 0, total / 2);
            int right = findKthNumbers(nums1, 0, nums2, 0, total / 2 + 1);
            return (left + right) / 2.0;
        } else {
            return findKthNumbers(nums1, 0, nums2, 0, total / 2 + 1);
        }
    }
};
 
```    

##### Leetcode 456. 132 模式
    
```C++
class Solution {
public:
    bool find132pattern(vector<int> &nums) {
        stack<int> stk;
        int s3 = INT_MIN;
        //反向遍历单调栈
        for (int i = nums.size() - 1; i >= 0; i--) {
            if (s3 > nums[i]) return true;
            while (stk.size() && stk.top() < nums[i]) {
                s3 = stk.top();
                stk.pop();
            }
            stk.push(nums[i]);
        }
        return false;
    }
}; 
```    
    
 ##### 300. 最长递增子序列

```cpp
//双循环dp
class Solution {
public:
    int lengthOfLIS(vector<int> &nums) {
        int n = nums.size();
        vector<int> f(n,0);
        int res = 0;
        for(int i = 0;i < n;i++){
            f[i] = 1;
            for(int j = 0;j < i;j++){
                if(nums[i] > nums[j]){
                    f[i] = max(f[i],f[j] + 1);
                }
            }
            res = max(res,f[i]);
        }
        return res;
    }
};

//贪心+ 二分查找
class Solution {
public:
    int lengthOfLIS(vector<int> &nums) {
        int n = nums.size();
        vector<int> f = {nums[0]};
        int res = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] > f.back()) {
                f.emplace_back(nums[i]);
            } else {
                int l = 0, r = f.size();
                while (l < r) {
                    int mid = l + r >> 1;
                    if (f[mid] >= nums[i]) r = mid;
                    else l = mid + 1;
                }
                f[l] = nums[i];
            }
        }
        return f.size();
    }
};

```

##### 354. 俄罗斯套娃信封问题

二维排序 + 一维双循环LIS/二分查找LIS

```cpp
//二维排序，一维双循环dp,超时
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        if (a[0] < b[0]) return true;
        else if (a[0] == b[0] && a[1] > b[1]) return true;
        return false;
    }

public:
    int maxEnvelopes(vector<vector<int>> &envelopes) {
        sort(envelopes.begin(), envelopes.end(), cmp);
        int n = envelopes.size();
        vector<int> f(n, 1);
        int res = 0;
        for (int i = 0; i < envelopes.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (envelopes[i][1] > envelopes[j][1]) {
                    f[i] = max(f[i], f[j] + 1);
                }
                res = max(res, f[i]);
            }
        }
        return res;
    }
};

//贪心+二分查找(LIS)
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        if (a[0] < b[0]) return true;
        else if (a[0] == b[0] && a[1] > b[1]) return true;
        return false;
    }

public:
    int maxEnvelopes(vector<vector<int>> &envelopes) {
        sort(envelopes.begin(), envelopes.end(), cmp);
        int n = envelopes.size();
        vector<int> f = {envelopes[0][1]};
        for (int i = 1; i < envelopes.size(); i++) {
            if (envelopes[i][1] > f.back()) {
                f.emplace_back(envelopes[i][1]);
            } else {
                auto it = lower_bound(f.begin(), f.end(), envelopes[i][1]);
                *it = envelopes[i][1];
            }
        }
        return f.size();
    }
};

```   
    
##### LeetCode 338. 比特位计数

Brian Kernighan 算法: Brian Kernighan 算法的原理是：对于任意整数x，令 x=x & (x−1)该运算将 x 的二进制表示的最后一个 1 变成 0。   
位操作符的总结 Brian Kernighan算法: https://blog.csdn.net/weixin_43688483/article/details/106349982

```cpp
//BK算法
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> res;
        for (int i = 0; i <= n; i++) {
            int sum = 0,num = i;
            while (num) {
                num = num & (num - 1);
                sum += 1;
            }
            res.emplace_back(sum);
        }
        return res;
    }
};

//dp算法
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> f(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            f[i] = f[i >> 1] + (i & 1);
        }
        return f;
    }
};
```    
                             
##### Leetcode 329. 矩阵中的最长递增路径

滑雪：记忆化dfs,memo,既当visit又存储值

```cpp
class Solution {
    int m, n;
    vector<vector<int>> memo;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
public:
    int longestIncreasingPath(vector<vector<int>> &matrix) {
        m = matrix.size(), n = matrix[0].size();
        memo = vector<vector<int>>(m, vector<int>(n, -1));
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                res = max(res, dp(i, j, matrix));
            }
        }
        return res;
    }

    int dp(int i, int j, vector<vector<int>> &matrix) {
        //记忆化dfs,memo,既当visit又存储值
        if (memo[i][j] != -1) return memo[i][j];
        memo[i][j] = 1;
        for (int k = 0; k < 4; k++) {
            int newI = i + dx[k], newJ = j + dy[k];
            if (newI >= 0 && newI < m && newJ >= 0 && newJ < n && matrix[newI][newJ] < matrix[i][j]) {
                memo[i][j] = max(memo[i][j], dp(newI, newJ, matrix) + 1);
            }
        }
        return memo[i][j];
    }
}; 
```    
    
##### LeetCode 264. Ugly Number II

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> num;
        num.emplace_back(1);
        int a = 0, b = 0, c = 0;
        for (int i = 2; i < n + 1; i++) {
            int two = num[a] * 2, three = num[b] * 3, five = num[c] * 5;
            int next = min(two, min(three, five));
            if (next == two) a++;
            if (next == three) b++;
            if (next == five) c++;
            num.emplace_back(next);
        }
        return num.back();
    }
};
```

##### LeetCode 115. 不同的子序列

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int m = s.size(), n = t.size();
        if(m < n) return 0;
        vector<vector<unsigned long long>> f(m + 1, vector<unsigned long long>(n + 1, 0));
        for (int i = 0; i < m + 1; i++) {
            f[i][0] = 1;
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                f[i][j] = f[i - 1][j];
                if (s[i - 1] == t[j - 1]) {
                    f[i][j] += f[i - 1][j - 1];
                }
            }
        }

        return f[m][n];
    }
};
```    
##### 132. 分割回文串 II

 回文dp + LIS DP
                                 
```cpp    
class Solution {
public:
    int minCut(string s) {
        int m = s.size();
        s = " " + s;
        vector<vector<int>> g(m + 1,vector<int>(m + 1, false));
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j <= i; j++) {
                if (i == j) g[j][i] = true;
                else if (s[i] == s[j]) {
                    //j + 1 > i - 1 表示i和j之间间隔一个字母
                    if (j + 1 > i - 1 || g[j + 1][i - 1]) {
                        g[j][i] = true;
                    }
                }
            }
        }

        vector<int> f(m + 1,INT_MAX);
        f[0] = 0;
        //LIS结构
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j <= i; j++) {
                if (g[j][i]) {
                    f[i] = min(f[i],f[j - 1] + 1);
                }
            }
        }
        return f[m] - 1;
    }
};    
```    
    
##### LeetCode 576. 出界的路径数

三维dp    
static constexpr int MOD = 1e9 + 7;
 
```cpp
class Solution {
public:
    int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        if (!maxMove) return 0;
        static constexpr int MOD = 1e9 + 7;
        //f[i][j][k]:移动k次到达i和j的路径总数
        vector<vector<vector<int>>> f(m, vector<vector<int>>(n, vector<int>(maxMove + 1, 0)));

        f[startRow][startColumn][0] = 1;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        int res = 0;
        for (int k = 0; k < maxMove; k++) {
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    int count = f[i][j][k];
                    if (count > 0) {
                        for (int d = 0; d < 4; d++) {
                            int newX = i + dx[d], newY = j + dy[d];
                            if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                                f[newX][newY][k + 1] = (f[newX][newY][k + 1] + count) % MOD;
                            } else {
                                res = (res + count) % MOD;
                            }
                        }
                    }
                }
            }
        }
        return res;
    }
};
    
    
```    

##### 526. 优美的排列

状态压缩dp,f[i]代表选法是i的时候的方案数，i的每一位代表是否选择当前的数。    
__builtin_popcount 计算32位的无符号整数有多少个1。
                                                                              
```c++
class Solution {
public:
    int countArrangement(int n) {
        vector<int> f(1 << n, 0);
        f[0] = 1;
        //状态压缩dp,f[i]代表选法是i的时候的方案数，i的每一位代表是否选择当前的数
        for (int mask = 0; mask < 1 << n; mask++) {
            int num = 1;
            for (int j = 0; j < n; j++) {
                if (mask & (1 << j)) num++;
            }
            for (int k = 0; k < n; k++) {
                if (!(mask & (1 << k)) && ((num % (k + 1) == 0) || ((k + 1) % num == 0))) {
                    f[mask | (1 << k)] += f[mask];
                }
            }
        }

        return f[(1 << n) - 1];
    }
};                                                                            
```                                                                               
                                                                             
##### 486 预测赢家
                        
区间dp
                        
```C++
class Solution {
public:
    bool PredictTheWinner(vector<int> &nums) {
        int m = nums.size();
        vector<vector<int>> f(m, vector<int>(m, 0));
        for (int len = 1; len <= m; len++) {
            for (int l = 0; l + len - 1 < m; l++) {
                int r = l + len - 1;
                if (len == 1) f[l][r] = nums[l];
                else {
                    f[l][r] = max(nums[l] - f[l + 1][r], nums[r] - f[l][r - 1]);
                }
            }
        }
        return f[0][m - 1] >= 0;
    }
};                        
```                        
##### 279 完全平方数

完全背包
    
```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> f(n + 1, INT_MAX);
        f[0] = 0;
        for (int i = 0; i < n + 1; i++) {
            for (int j = 1; j * j <= i; j++) {
                f[i] = min(f[i], f[i - j * j] + 1);
            }
        }
        return f[n];
    }
};
```    
    
##### Leetcode 130 被围绕的区域
    
四边dfs + 涂色法
    
```C++
class Solution {
    int m,n;
public:
    void solve(vector<vector<char>> &board) {
        m = board.size(),n = board[0].size();
        for (int i = 0; i < m; i++) {
            dfs(i, 0, board);
            dfs(i, n - 1, board);
        }
        for (int j = 0; j < n; j++) {
            dfs(0, j, board);
            dfs(m - 1, j, board);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'A') {
                    board[i][j] = 'O';
                } else if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                }
            }
        }
    }

    void dfs(int i, int j, vector<vector<char>> &board) {
        if (i < 0 || i > m - 1 || j < 0 || j > n - 1 || board[i][j] != 'O')
            return;
        board[i][j] = 'A';
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        for (int k = 0; k < 4; k++) {
            int newI = i + dx[k], newJ = j + dy[k];
            dfs(newI, newJ, board);
        }
    }
};    
```    

##### 127 单词接龙

双向BFS理解每次各自扩展一层的含义

26 字母扩展方法

```C++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string> &wordList) {
        unordered_set<string> Sets;
        for (auto &word:wordList) {
            Sets.emplace(word);
        }
        if(!Sets.count(endWord))
            return 0;
        queue<string> qBegin;
        qBegin.emplace(beginWord);
        unordered_map<string, int> distBegin;
        distBegin[beginWord] = 0;

        queue<string> qEnd;
        qEnd.emplace(endWord);
        unordered_map<string, int> distEnd;
        distEnd[endWord] = 0;

        while (qBegin.size() && qEnd.size()) {
            int len1 = qBegin.size();
            for (int k = 0; k < len1; k++) {
                auto word = qBegin.front();
                qBegin.pop();
                if (distEnd.count(word)) {
                    return distBegin[word] + distEnd[word] + 1;
                }
                for (int i = 0; i < word.size(); i++) {
                    auto tmp = word;
                    for (int j = 'a'; j <= 'z'; j++) {
                        tmp[i] = j;
                        if (Sets.count(tmp) && !distBegin.count(tmp)) {
                            distBegin[tmp] = distBegin[word] + 1;
                            qBegin.emplace(tmp);
                        }
                    }
                }
            }

            int len2 = qEnd.size();
            for (int k = 0; k < len2; k++) {
                auto wordEnd = qEnd.front();
                qEnd.pop();
                if (distBegin.count(wordEnd)) {
                    return distBegin[wordEnd] + distEnd[wordEnd] + 1;
                }
                for (int i = 0; i < wordEnd.size(); i++) {
                    auto tmp = wordEnd;
                    for (int j = 'a'; j <= 'z'; j++) {
                        tmp[i] = j;
                        if (Sets.count(tmp) && !distEnd.count(tmp)) {
                            distEnd[tmp] = distEnd[wordEnd] + 1;
                            qEnd.emplace(tmp);
                        }
                    }
                }
            }

        }
        return 0;
    }
};
```

##### 126 单词接龙 II

BFS（26扩展方法） + DFS 反向搜索

```
class Solution {
    string targetWord;
    vector<string> path;
    vector<vector<string>> ans;
    unordered_map<string, int> dist;

public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string> &wordList) {
        unordered_set<string> Sets;
        for (auto &word:wordList) {
            Sets.emplace(word);
        }
        if(!Sets.count(endWord))
            return ans;
        queue<string> q;
        q.emplace(beginWord);
        dist[beginWord] = 0;

        bool found = false;
        while (q.size()) {
            int len = q.size();
            for (int k = 0; k < len; k++) {
                auto word = q.front();
                q.pop();
                if (word == endWord) {
                    found = true;
                    break;
                }
                for (int i = 0; i < word.size(); i++) {
                    auto tmp = word;
                    for (int j = 'a'; j <= 'z'; j++) {
                        tmp[i] = j;
                        if (Sets.count(tmp) && !dist.count(tmp)) {
                            dist[tmp] = dist[word] + 1;
                            q.emplace(tmp);
                        }
                    }
                }
            }

            if(found) break;
        }

        targetWord = beginWord;
        if(found){
            path.push_back(endWord);//反向搜索
            dfs(endWord);
        }

        return ans;
    }

    void dfs(string endWord)
    {
        if(endWord == targetWord){
            reverse(path.begin(),path.end());
            ans.emplace_back(path);
            reverse(path.begin(),path.end());
        }else{
            for (int i = 0; i < endWord.size(); i++) {
                auto tmp = endWord;
                for (int j = 'a'; j <= 'z'; j++) {
                    tmp[i] = j;
                    if (dist.count(tmp) && dist[endWord] == dist[tmp] + 1) {
                        path.emplace_back(tmp);
                        dfs(tmp);
                        path.pop_back();
                    }
                }
            }
        }
    }
};

```

##### Leetcode 207 课程表

拓扑排序

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>> &prerequisites) {
        //inDegree表 + 邻接表(边)
        vector<vector<int>> edges(numCourses);
        vector<int> inDeg(numCourses, 0);
        for (auto &edge:prerequisites) {
            edges[edge[1]].emplace_back(edge[0]);
            inDeg[edge[0]]++;
        }

        queue<int> q;
        for (int i = 0; i < numCourses; i++) {
            if (inDeg[i] == 0) q.emplace(i);
        }
        int cnt = 0;
        while (q.size()) {
            auto top = q.front();
            q.pop();
            cnt++;
            for (auto ver:edges[top]) {
                inDeg[ver]--;
                if (inDeg[ver] == 0) q.emplace(ver);
            }
        }
        return cnt == numCourses;
    }
};
```

#####  Leetcode 210 课程表2

拓扑排序求序列

```C++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>> &prerequisites) {
        vector<int> res;
        vector<int> inDeg(numCourses, 0);
        vector<vector<int>> edges(numCourses);
        for (auto &edge: prerequisites) {
            edges[edge[1]].emplace_back(edge[0]);
            inDeg[edge[0]]++;
        }
        queue<int> q;
        for (int i = 0; i < numCourses; i++) {
            if (inDeg[i] == 0) q.emplace(i);
        }
        while (q.size()) {
            int top = q.front();
            q.pop();
            res.emplace_back(top);
            for (auto &ver:edges[top]) {
                inDeg[ver]--;
                if (inDeg[ver] == 0) q.emplace(ver);
            }
        }

        if (res.size() < numCourses) return {};
        return res;
    }
};

```

##### LeetCode 733. Flood Fill 图像渲染

```c++
class Solution {
    int m, n;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
public:
    vector<vector<int>> floodFill(vector<vector<int>> &image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) return image;
        m = image.size(), n = image[0].size();
        int color = image[sr][sc];
        image[sr][sc] = newColor;
        for (int k = 0; k < 4; k++) {
            int newI = sr + dx[k], newJ = sc + dy[k];
            if (0 <= newI && newI < m && 0 <= newJ && newJ < n && (image[newI][newJ] == color)) {
                floodFill(image, newI, newJ, newColor);
            }
        }
        return image;
    }
};
```

#####  LeetCode 542. 01 Matrix

矩形多源头的bfs，类比入度为0的拓扑排序.

```C++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>> &mat) {
        int m = mat.size(), n = mat[0].size();
        queue<pair<int, int>> q;
        vector<vector<int>> dist(m, vector<int>(n, -1));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 0) {
                    dist[i][j] = 0;
                    q.emplace(make_pair(i, j));
                }
            }
        }

        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        while (q.size()) {
            auto top = q.front();
            q.pop();
            for (int k = 0; k < 4; k++) {
                int x = top.first + dx[k], y = top.second + dy[k];
                if (0 <= x && x < m && 0 <= y && y < n && dist[x][y] == -1) {
                    dist[x][y] = dist[top.first][top.second] + 1;
                    q.emplace(make_pair(x,y));
                }
            }
        }
        return dist;
    }
};


```


##### LeetCode 231. 2的幂

```C++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && (n & -n) == n;
    }
};

class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }
};
```

##### 762. 二进制表示中质数个计算置位

```C++
class Solution {
public:
    int countPrimeSetBits(int left, int right) {
        vector<int> nums = {2, 3, 5, 7, 11, 13, 17, 19};
        unordered_set<int> primes;
        for (auto &num:nums) {
            primes.emplace(num);
        }
        int ans = 0;
        for (int i = left; i <= right; i++) {
            int sum = 0;
            for (int k = i; k;) {
                sum += k & 1;
                k >>= 1;
            }
            if (primes.count(sum)) ans++;
        }
        return ans;
    }
};
```

##### 137. 只出现一次的数字 II

```C++
class Solution {
public:
    int singleNumber(vector<int> &nums) {
        int ans = 0;
        for (int i = 0; i < 32; i++) {
            int sum = 0;
            for (auto &num: nums) {
                sum += (num >> i) & 1;
            }
            if (sum % 3) ans |= 1 << i;
        }
        return ans;
    }
};
```

##### 260. 只出现一次的数字 II

```C++
class Solution {
public:
    vector<int> singleNumber(vector<int> &nums) {
        int n = 0;
        for (auto &num:nums) {
            n ^= num;
        }

        //lowbit 运算
        int k = n == INT_MIN ? INT_MIN : n & (-n);
        int res1 = 0, res2 = 0;
        for (auto &num:nums) {
            if (num & k) {
                res1 ^= num;
            } else {
                res2 ^= num;
            }
        }
        return {res1, res2};
    }
};

```

##### Leetcode 371 两整数之和

```
class Solution {
public:
    int getSum(int a, int b) {
        while (b) {
            auto carry = (unsigned int) (a & b) << 1;
            a = a ^b;
            b = carry;
        }
        return a;
    }
};
```

##### LeetCode 201. 数字范围按位与

寻找公共前缀

```C++
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int shift = 0;
        while (m < n) {
            m >>= 1;
            n >>= 1;
            shift++;
        }
        return m << shift;
    }
};
```
##### Leetcode 477 汉明距离总和

```C++
class Solution {
public:
    int totalHammingDistance(vector<int> &nums) {
        int ans = 0, n = nums.size();
        for (int i = 0; i < 32; i++) {
            int total = 0;
            for (auto &num:nums) {
                total += (num >> i) & 1;
            }
            ans += total * (n - total);
        }
        return ans;
    }
};
```

##### 860. 柠檬水找零

贪心，优先用10块的。 

```C++
class Solution {
public:
    bool lemonadeChange(vector<int> &bills) {
        int fives = 0, tens = 0;
        for (auto bill:bills) {
            if (bill == 5) {
                fives++;
            } else if (bill == 10) {
                if (!fives) return false;
                fives--;
                tens++;
            } else {
                if (tens && fives) {
                    fives--;
                    tens--;
                } else if (fives >= 3) {
                    fives -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
};
```

##### 392 判断子序列

双指针单for 取代双循环的技巧

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if (s.empty()) return true;
        for (int i = 0, j = 0; i < t.size(); i++) {
            if (t[i] == s[j]) j++;
            if (j == s.size()) return true;
        }
        return false;
    }
};
```

##### Leetcode 455. 分发饼干

贪心排序

```C++
class Solution {
public:
    int findContentChildren(vector<int> &g, vector<int> &s) {
        sort(g.begin(), g.end()); //小孩需要
        sort(s.begin(), s.end());
        int i = 0, j = 0;
        while (i < g.size() && j < s.size()) {
            while (j < s.size() && s[j] < g[i]) j++;
            if (j < s.size()) {
                i++;
                j++;
            }
        }
        return i;
    }
};

```

##### Leetcode 55. 跳跃游戏

```C++
class Solution {
public:
    bool canJump(vector<int> &nums) {
        int res = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i > res) {
                return false;
            }
            res = max(res, i + nums[i]);
            if (res >= nums.size() - 1) {
                return true;
            }
        }
        return false;
    }
};
```

##### Leetcode 45. 跳跃游戏 II

贪心区间问题

```C++
class Solution {
public:
    int jump(vector<int> &nums) {
        int step = 0;
        int n = nums.size();
        int end = 0, maxR = 0;
        for (int i = 0; i < n - 1; i++) {
            maxR = max(maxR, i + nums[i]);
            if (i == end) {
                end = maxR;
                step++;
            }
        }

        return step;
    }
};

```

##### LeetCode 376. 摆动序列
 
可以先nums.erase(unique(nums.begin(), nums.end()), nums.end()); 预处理下。

```C++ 
class Solution {
public:
    int wiggleMaxLength(vector<int> &nums) {
        if (nums.size() < 2)
            return nums.size();
        int preDiff = nums[1] - nums[0];
        int res = preDiff != 0 ? 2 : 1;
        for (int i = 2; i < nums.size(); i++) {
            int diff = nums[i] - nums[i - 1];
            if (preDiff >= 0 && diff < 0 || preDiff <= 0 && diff > 0) {
                res++;
                preDiff = diff;
            }
        }
        return res;
    }
};
```

##### LeetCode 406 根据身高重建队列

类信封问题，vector两维度排序

```C++
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[0] > b[0] || a[0] == b[0] && a[1] < b[1];
    }

public:
    vector<vector<int>> reconstructQueue(vector<vector<int>> &people) {
        sort(people.begin(), people.end(), cmp);
        vector<vector<int>> res;
        for (auto &p:people) {
            res.insert(res.begin() + p[1], p);
        }
        return res;
    }
};
```
##### 452 用最少数量的箭引爆气球

贪心区间问题，模板题，区间问题，按起点或终点排序。

```C++
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[1] < b[1];
    }

public:
    int findMinArrowShots(vector<vector<int>> &points) {
        sort(points.begin(), points.end(), cmp);
        int end = points[0][1], res = 1;
        for (auto &point:points) {
            if (point[0] > end) {
                end = point[1];
                res++;
            }
        }
        return res;
    }
};

```


##### LeetCode 402. 移掉 K 位数字

```C++
class Solution {
public:
    string removeKdigits(string num, int k) {
        stack<char> stk;
        for (auto &n:num) {
            while (stk.size() && stk.top() > n && k) {
                stk.pop();
                k--;
            }
            stk.push(n);
        }

        while (k--) stk.pop();
        string res;
        while (stk.size()) {
            res += stk.top();
            stk.pop();
        }
        reverse(res.begin(), res.end());
        int i = 0;
        while (i < res.size() && res[i] == '0') i++;
        if (i == res.size()) return "0";
        return res.substr(i);
    }
};
```

##### LeetCode 134  加油站

```C++
class Solution {
public:
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
        int m = gas.size();
        for (int i = 0, j = 0; i < m;) {
            int gas_left = 0;
            for (j = 0; j < m; j++) {
                int k = (i + j) % m;
                gas_left += gas[k] - cost[k];
                if (gas_left < 0) break;
            }
            if (j == m) {
                return i;
            } else {
                i += j + 1;
            }
        }
        return -1;
    }
};
```




