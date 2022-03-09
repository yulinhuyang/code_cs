##### LeetCode554 砖墙 

##### LeetCode 149 直线上最多的点数	


##### LeetCode 290 单词规律 

##### LeetCode 350 两个数组的交集 


##### LeetCode 525 连续数组	 


##### LeetCode 454 四数相加 II	 


##### LeetCode 496 下一个更大元素 I	

##### LeetCode 503 下一个更大元素 II


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
    
    
                             
