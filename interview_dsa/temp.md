



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
#####  Offer II 114 外星文字典

Leetcode 269 外星文字典

map(set)(graph) + map(inDegree)

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
                    if (!graph[pre].count(cur)) { //加入graph要去重，防止inDegree多加了
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

#####  Offer II 115 重建序列

Leetcode 444 重建序列

map(set)(graph) + map(inDegree)

BFS 遍历size保证层序遍历性(最短路模型) ;加入graph要去重，防止inDegree多加了

```C++
class Solution {
public:
    bool sequenceReconstruction(vector<int> &org, vector<vector<int>> &seqs) {
        //map(set)(graph) + map(inDegree)
        unordered_map<int, unordered_set<int>> graph;
        unordered_map<int, int> inDegree;
        for (auto &seq:seqs) {
            for (auto &c:seq) {
                inDegree[c] = 0;
            }
        }
        if (org.size() != inDegree.size()) return false;
        for (auto &seq:seqs) {
            for (int i = 1; i < seq.size(); i++) {
                //判重，防止inDegree多加
                if (!graph[seq[i - 1]].count(seq[i])) {
                    graph[seq[i - 1]].emplace(seq[i]);
                    inDegree[seq[i]]++;
                }
            }
        }
        queue<int> q;
        for (auto &node:inDegree) {
            if (!node.second) q.emplace(node.first);
        }

        //入度为0的元素不唯一，则无法确定唯一序列
        if (q.size() != 1) return false;
        vector<int> topology;
        while (!q.empty()) {
            int size = q.size();
            if (q.size() != 1) return false;
            for (int i = 0; i < size; i++) {
                auto t = q.front();
                q.pop();
                topology.emplace_back(t);
                for (auto &y:graph[t]) {
                    inDegree[y]--;
                    if (!inDegree[y]) q.emplace(y);
                }
            }
        }
        if (topology.size() != inDegree.size()) return false;
        for (int i = 0; i < topology.size(); i++) {
            if (topology[i] != org[i]) return false;
        }
        return true;
    }
};
```

##### Leetcode 839 相似字符串组

并查集模板题

```C++
class Solution {
    vector<int> p;
public:
    int numSimilarGroups(vector<string> &strs) {
        int m = strs.size();
        p = vector<int>(m, -1);
        for (int i = 0; i < m; i++) {
            p[i] = i;
        }
        for (int i = 0; i < m - 1; i++) {
            for (int j = i + 1; j < m; j++) {
                int pa = find(i);
                int pb = find(j);
                if (pa == pb) continue;
                if (check(strs[i], strs[j])) {
                    p[pb] = pa;
                }
            }
        }
        int res = 0;
        for (int i = 0; i < m; i++) {
            cout << p[i] << endl;
            if (p[i] == i) res++;
        }
        return res;
    }

    int find(int i) {
        if (p[i] != i) p[i] = find(p[i]);
        return p[i];
    }

    bool check(string &a, string &b) {
        int num = 0;
        for (int i = 0; i < a.size(); i++) {
            if (a[i] != b[i]) {
                num++;
                if (num > 2) return false;
            }
        }
        return num == 0 || num == 2;
    }
};
```

##### Leetcode 128 最长连续序列

```C++
class Solution {
public:
    int longestConsecutive(vector<int> &nums) {
        unordered_set<int> set;
        for (auto &num:nums) {
            set.emplace(num);
        }
        int res = 0;
        for (auto &num:nums) {
            if (set.count(num) && !set.count(num - 1)) {
                set.erase(num);
                int end = num;
                while (set.count(end + 1)) {
                    end++;
                    set.erase(end);
                }
                res = max(res, end - num + 1);
            }
        }
        return res;
    }
};

```





AcWing190 字串变换

双向BFS模板题

```C++
int extend(queue<string>& q, unordered_map<string, int>&da, unordered_map<string, int>& db, 
    string a[N], string b[N])
{
    int d = da[q.front()];
    while (q.size() && da[q.front()] == d)
    {
        auto t = q.front();
        q.pop();
		//每次每边扩展完整一层
        for (int i = 0; i < n; i ++ ) 
            for (int j = 0; j < t.size(); j ++ )
                if (t.substr(j, a[i].size()) == a[i])
                {
                    string r = t.substr(0, j) + b[i] + t.substr(j + a[i].size());
                    if (db.count(r)) return da[t] + db[r] + 1;
                    if (da.count(r)) continue;
                    da[r] = da[t] + 1;
                    q.push(r);
                }
    }

    return 11;
}

int bfs()
{
    if (A == B) return 0;
    queue<string> qa, qb;
    unordered_map<string, int> da, db;

    qa.push(A), qb.push(B);
    da[A] = db[B] = 0;

    int step = 0;
    while (qa.size() && qb.size())
    {
        int t;
        if (qa.size() < qb.size()) t = extend(qa, da, db, a, b);
        else t = extend(qb, db, da, b, a);

        if (t <= 10) return t;
        if ( ++ step == 10) return -1;
    }

    return -1;
}


```

AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】:https://www.acwing.com/solution/content/56348/


