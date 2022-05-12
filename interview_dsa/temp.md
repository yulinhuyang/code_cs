



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

##### Leetcode 785  判断二分图

染色法判断二分图 模板题

```C++
class Solution {
    vector<int> color;

public:
    bool isBipartite(vector<vector<int>> &graph) {
        int m = graph.size();
        color = vector<int>(m, -1);
        bool flag = true;
        for (int i = 0; i < m; i++) {
            if (color[i] == -1) {
                if (!dfs(graph, i, 0)) {
                    flag = false;
                    break;
                }
            }
        }
        return flag;
    }

    bool dfs(vector<vector<int>> &graph, int u, int c) {
        color[u] = c;
        for (auto cur : graph[u]) {
            if (color[cur] == -1) {
                if (!dfs(graph, cur, !c))
                    return false;
            } else if (color[cur] == c)
                return false;
        }
        return true;
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
        int m = q.size(); //出一层
        while (m--) {
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

##### Leetcode 127 单词接龙

【宫水三叶】如何使用「双向 BFS」解决搜索空间爆炸问题（附 A* 算法）: https://leetcode.cn/problems/word-ladder/solution/gong-shui-san-xie-ru-he-shi-yong-shuang-magjd/

```C++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string> &wordList) {
        unordered_set<string> sets(wordList.begin(), wordList.end());
        if (!sets.count(endWord)) {
            return 0;
        }
        queue<string> q1, q2;
        q1.emplace(beginWord);
        q2.emplace(endWord);
        unordered_map<string, int> d1, d2;
        d1[beginWord] = 0;
        d2[endWord] = 0;
        while (!q1.empty() && !q2.empty()) {
            int t = -1;
            if (q1.size() <= q2.size()) {
                t = bfs(q1, d1, d2, sets);
            } else {
                t = bfs(q2, d2, d1, sets);
            }
            if (t != -1) return t;
        }
        return 0;
    }

    int bfs(queue<string> &q, unordered_map<string, int> &d1, unordered_map<string, int> &d2, unordered_set<string> &sets) {
        int m = q.size();//出一层
        while (m--) {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < t.size(); i++) {
                auto t1 = t;
                for (int j = 'a'; j <= 'z'; j++) {
                    t1[i] = j;
                    if (d2.count(t1)) return d1[t] + d2[t1] + 1 + 1;
                    if (!sets.count(t1)) continue;
                    if (d1.count(t1) && d1[t1] <= d1[t] + 1) continue;
                    d1[t1] = d1[t] + 1;
                    q.emplace(t1);
                }
            }
        }
        return -1;
    }
};
```
##### Leetcode 797 所有可能的路径

```C++
class Solution {
    vector<vector<int>> ans;
    vector<int> path;
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>> &graph) {
        int n = graph.size();
        path.emplace_back(0);
        dfs(graph, 0, n - 1);
        return ans;
    }

    void dfs(vector<vector<int>> &graph, int start, int end) {
        if (start == end) {
            ans.emplace_back(path);
            return;
        }
        for (auto &y:graph[start]) {
            path.emplace_back(y);
            dfs(graph, y, end);
            path.pop_back();
        }
    }
};
```
#####  剑指 Offer II 114 外星文字典

Leetcode 269 外星文字典

```C++
class Solution {
public:
    string alienOrder(vector<string> &words) {
        unordered_map<char, unordered_set<char>> graph;
        unordered_map<char, int> inDegree;//graph + indegree 双hash
        for (auto &word:words) {
            for (auto &c:word) {
                if (!graph.count(c)) graph[c] = {};
                if (!inDegree.count(c)) inDegree[c] = 0;
            }
        }
        for (int i = 1; i < words.size(); i++) {
            int j = 0;
            int len = min(words[i - 1].size(), words[i].size());
            for (; j < len; j++) {
                auto pre = words[i - 1][j];
                auto cur = words[i][j];
                if (pre != cur) {
                    if (!graph[pre].count(cur)) {
                        graph[pre].emplace(cur);
                        inDegree[cur]++;

                    }
                    break;
                }
            }
            if (j == len && words[i - 1].size() > words[i].size()) {
                return "";
            }
        }

        queue<char> q;
        //拓扑排序与多源bfs的一致性
        for (auto &v:inDegree) {
            if (!v.second) q.push(v.first);
        }

        string alienDict;
        while (!q.empty()) {
            int size = q.size();//bfs的层序性
            for (int i = 0; i < size; i++) {
                auto t = q.front();
                q.pop();
                alienDict.push_back(t);
                for (auto &y:graph[t]) {
                    inDegree[y]--;
                    if (!inDegree[y]) q.emplace(y);
                }
            }
        }
        return alienDict.size() == inDegree.size() ? alienDict : "";

    }
};

```


AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】:https://www.acwing.com/solution/content/56348/

 567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
