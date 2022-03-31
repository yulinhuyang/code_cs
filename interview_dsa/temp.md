
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

全排列：每次dfs,每个点都要遍历做一次选择(选/不选)。    
组合/子集：每次dfs，只需要在某个或者某些(相同元素剪枝)点做一次选择(选/不选)。   

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









