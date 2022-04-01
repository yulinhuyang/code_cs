
##### LeetCode 36. 有效的数独

lt里面全局变量会被初始化，局部变量不会被初始化

```C++
class Solution {
    int rows[9][9],cols[9][9],grids[9][9][9];
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        for(int i = 0;i < 9;i++){
            for(int j = 0;j < 9;j++){
                if (board[i][j] != '.') {
                    int t = board[i][j] - '1';
                    if (rows[i][t] || cols[j][t] || grids[i /3][j /3][t]) {
                        return false;
                    }
                    rows[i][t] = cols[j][t] =  1;
                    grids[i / 3][j / 3][t] = 1;
                }
            }
        }
        return  true;
    }
};
```

#####  LeetCode 39. 组合总和

dfs + 排除等效冗余+ 可行性剪枝

```C++
class Solution {
    vector<vector<int>> res;
    vector<int> path;
public:
    vector<vector<int>> combinationSum(vector<int> &candidates, int target) {
        dfs(candidates, 0, target);
        return res;
    }

    void dfs(vector<int> &can, int u, int target) {
        if (target == 0) {
            res.emplace_back(path);
            return;
        }
        if (u == can.size()) return;
        for (int i = 0; can[u] * i <= target; i++) {
            dfs(can, u + 1, target - can[u] * i);
            path.emplace_back(can[u]);
        }
        for (int i = 0; can[u] * i <= target; i++) {
            path.pop_back();
        }
    }
};
```

##### LeetCode 40  组合总和 II

```C++
class Solution {
    vector<vector<int>> res;
    vector<int> path;
public:
    vector<vector<int>> combinationSum2(vector<int> &candidates, int target) {
        sort(candidates.begin(), candidates.end());
        dfs(candidates, 0, target);
        return res;
    }

    void dfs(vector<int> &can, int u, int target) {
        if (target == 0) {
            res.emplace_back(path);
            return;
        }
        if (u == can.size()) return;
        int k = u + 1;
        while (k < can.size() && can[k] == can[u]) k++;
        for (int i = 0; can[u] * i <= target && u + i <= k; i++) {
            dfs(can, k, target - can[u] * i);
            path.emplace_back(can[u]);
        }
        for (int i = 0; can[u] * i <= target && u + i <= k; i++) {
            path.pop_back();
        }
    }
};

```

##### Leetcode 41  缺失的第一个正数

hash空间简化，数组索引记录做hash，可以对比448题

```C++
class Solution {
public:
    int firstMissingPositive(vector<int> &nums) {
        int n = nums.size();
        //剔除负数和0
        for (int i = 0; i < n; i++) {
            if (nums[i] <= 0) nums[i] = n + 1;
        }
        for (int i = 0; i < n; i++) {
            int num = abs(nums[i]);
            if (num <= n) {
                nums[num - 1] = -abs(nums[num - 1]);
            }
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) return i + 1;
        }
        return n + 1;
    }
};
```

##### Leetcode 50. Pow(x, n)

```C++
class Solution {
public:
    double myPow(double x, int n) {
        double res = 1.0;
        long long t = n;
        if (t < 0) {
            t = -t;
        }
        while (t) {
            if (t & 1) res *= x;
            t >>= 1;
            x *= x;
        }
        return n < 0 ? 1 / res : res;
    }
};
```

##### Leetcode 44  通配符匹配

判* 当前

dp[i][j] = dp[i - 1][j] | dp[i][j - 1];

```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        s = " " + s;
        p = " " + p;
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        dp[0][0] = 1;
        for (int i = 1; i < n + 1; i++) {
            if (p[i] == '*') dp[0][i] |= dp[0][i - 1];
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (s[i] == p[j] || p[j] == '?') {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (p[j] == '*') {
                    dp[i][j] = dp[i - 1][j] | dp[i][j - 1];
                }
            }
        }

        return dp[m][n];
    }
};
```
##### Leetcode 57  插入区间

方法1：模拟，三段式：寻左边界 + 处理交集 + 右边界

方法2：模拟，三种情况讨论 + flag

```C++
//模拟，三段式：寻左边界 + 处理交集 + 右边界
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>> &intervals, vector<int> &newInterval) {
        int k = 0;
        vector<vector<int>> res;
        int start = newInterval[0], end = newInterval[1];
        //寻左边界
        while (k < intervals.size() && intervals[k][1] < start) res.emplace_back(intervals[k++]);

        //处理交集
        if (k < intervals.size()) {
            start = min(start, intervals[k][0]);
            while (k < intervals.size() && intervals[k][0] <= end) end = max(end, intervals[k++][1]);
        }
        res.emplace_back(vector<int>{start, end});

        //处理右边界
        while (k < intervals.size()) res.emplace_back(intervals[k++]);
        return res;
    }
};

//模拟，三种情况讨论 + flag
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>> &intervals, vector<int> &newInterval) {
        int k = 0;
        vector<vector<int>> res;
        int start = newInterval[0], end = newInterval[1];
        bool flag = false;
        for (auto &interval:intervals) {
            if (interval[1] < start) {
                res.emplace_back(interval);
            } else if (interval[0] > end) {
                if (!flag) {
                    res.push_back({start, end});
                    flag = true;
                }
                res.emplace_back(interval);
            } else {
                start = min(start, interval[0]);
                end = max(end, interval[1]);
            }
        }
        if (!flag) {
            res.push_back({start, end});//push_back可以直接{}构造vector
        }
        return res;
    }
};
```

##### Leetcode 58 最后一个单词的长度

```C++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int n = s.size();
        int k = n - 1, res = 0;
        while (k >= 0 && s[k] == ' ') k--;
        while (k >= 0 && s[k] != ' ') {
            k--;
            res++;
        }
        return res;
    }
};

```
#####  Leetcode 59 螺旋矩阵 II

四方向遍历

```C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0));
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        int x = 0, y = 0, d = 0;
        for (int k = 1; k <= n * n; k++) {
            res[x][y] = k;
            int a = x + dx[d], b = y + dy[d];
            if (a < 0 || a >= n || b < 0 || b >= n || res[a][b]) {
                d = (d + 1) % 4;
                a = x + dx[d], b = y + dy[d];
            }
            x = a, y = b;
        }
        return res;
    }
};
```

##### 60. 排列序列

康托展开是一个全排列到一个自然数的双射，常用于构建哈希表时的空间压缩。 康托展开的实质是计算当前排列在所有由小到大全排列中的顺序，因此是可逆的。  
康托展开的逆运算：可以通过康托展开值求出原排列，即可以求出n的全排列中第x大排列。 

X=a[n]\*(n-1)!+a[n-1]\*(n-2)!+...+a[i]\*(i-1)!+...+a[1]\*0!

康托展开和逆康托展开： https://blog.csdn.net/wbin233/article/details/72998375

对于n个数来说，每个组有(n-1)!个全排列，n-1个数，每个组就有(n-2)!个全排列。   
对于特定数字，k/(n-1)!可以得到当前数字的下标，剩下的n-1个数，则用k%(n-1)来计算，只到n个0，得到了第k个。  

```C++
class Solution {
public:
    string getPermutation(int n, int k) {
        string res;
        string tmp = "";
        for (int i = 1; i <= n; i++) {
            tmp += to_string(i);
        }
        vector<int> fac(n, 0);
        fac[0] = 1;
        for (int i = 1; i < n; i++) {
            fac[i] = fac[i - 1] * i;
        }
        k--;

        for (int i = n; i > 0; i--) {
            int idx = k / fac[i - 1];
            k %= fac[i - 1];
            res.push_back(tmp[idx]);
            tmp.erase(tmp.begin() + idx);
        }
        return res;
    }
};

```


