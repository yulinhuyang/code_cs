1  554 砖墙开始



##### 214 最短回文串

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
                                          
                                          
