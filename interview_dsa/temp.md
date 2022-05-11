



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

##### Leetcode 752  打开转盘锁
双向BFS模板题

```C++
class Solution {
    char PlusOne(char s) {
        return s == '9' ? '0' : s + 1;
    }

    char MinusOne(char s) {
        return s == '0' ? '9' : s - 1;
    }

    int extend(queue<string> &q, unordered_set<string> &sets, unordered_map<string, int> &dist1,
               unordered_map<string, int> &dist2) {
        auto t = q.front();
        q.pop();
        for (int i = 0; i < t.size(); i++) {
            for (int j = -1; j <= 1; j++) {
                if (j == 0) continue;
                string t1 = t;
                t1[i] = j == -1 ? MinusOne(t[i]) : PlusOne(t[i]);
                if (dist1.count(t1) || sets.count(t1)) continue;
                if (dist2.count(t1)) return dist1[t] + dist2[t1] + 1;
                dist1[t1] = dist1[t] + 1;
                q.emplace(t1);
            }
        }
        return -1;
    }

public:
    int openLock(vector<string> &deadends, string target) {
        string start = "0000";
        if(start == target) return  0;
        unordered_set<string> sets(deadends.begin(), deadends.end());
        if (sets.count(target) || sets.count(start)) return -1;
        queue<string> q1, q2;
        unordered_map<string, int> d1, d2;
        q1.emplace(start);
        q2.emplace(target);
        d1[start] = 0;
        d2[target] = 0;
        while (!q1.empty() && !q2.empty()) {
            int t;
            if (q1.size() <= q2.size()) {
                t = extend(q1, sets, d1, d2);
            } else {
                t = extend(q2, sets, d2, d1);
            }
            if (t != -1) return t;
        }
        return -1;
    }
};
```


AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】:https://www.acwing.com/solution/content/56348/

 567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
