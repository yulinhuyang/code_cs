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
