
### 究极班-Week 25(第 600、605 ~ 606、609、611、617、621 ~ 623、628 题)

##### LeetCode 600. 不含连续1的非负整数
```cpp
class Solution {
public:
    int findIntegers(int n) {
        vector<int> num;
        while (n) num.push_back(n % 2), n /= 2;
        vector<vector<int>> f(num.size() + 1, vector<int>(2));
        f[1][0] = f[1][1] = 1;
        for (int i = 2; i <= num.size(); i ++ ) {
            f[i][0] = f[i - 1][0] + f[i - 1][1];
            f[i][1] = f[i - 1][0];
        }

        int res = 0;
        for (int i = num.size(), last = 0; i; i -- ) {
            int x = num[i - 1];
            if (x) {
                res += f[i][0];
                if (last) return res;
            }
            last = x;
        }
        return res + 1;
    }
};
```
##### LeetCode 605. 种花问题
```cpp
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        if (!n) return true;
        int res = 0;
        for (int i = 0; i < flowerbed.size(); i ++ ) {
            if (flowerbed[i]) continue;
            int j = i;
            while (j < flowerbed.size() && !flowerbed[j]) j ++ ;
            int k = j - i - 1;
            if (!i) k ++ ;
            if (j == flowerbed.size()) k ++ ;
            res += k / 2;
            if (res >= n) return true;
            i = j;
        }
        return false;
    }
};
```
##### LeetCode 606. 根据二叉树创建字符串
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    string ans;

    string tree2str(TreeNode* t) {
        dfs(t);
        return ans;
    }

    void dfs(TreeNode* t) {
        if (!t) return;
        ans += to_string(t->val);
        if (t->left || t->right) {
            ans += '(';
            dfs(t->left);
            ans += ')';
        }
        if (t->right) {
            ans += '(';
            dfs(t->right);
            ans += ')';
        }
    }
};
```
##### LeetCode 609. 在系统中查找重复文件
```cpp
class Solution {
public:
    vector<vector<string>> findDuplicate(vector<string>& paths) {
        unordered_map<string, vector<string>> hash;
        for (auto& path: paths) {
            stringstream ssin(path);
            string p, file, name, content;
            ssin >> p;
            while (ssin >> file) {
                int x = file.find('('), y = file.find(')');
                name = file.substr(0, x), content = file.substr(x + 1, y - x - 1);
                hash[content].push_back(p + '/' + name);
            }
        }
        vector<vector<string>> res;
        for (auto& [k, v]: hash)
            if (v.size() > 1)
                res.push_back(v);
        return res;
    }
};
```
##### LeetCode 611. 有效三角形的个数
```cpp
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int res = 0;
        for (int i = 0; i < nums.size(); i ++ )
            for (int j = i - 1, k = 0; j > 0 && k < j; j -- ) {
                while (k < j && nums[k] <= nums[i] - nums[j]) k ++ ;
                res += j - k;
            }
        return res;
    }
};
```
##### LeetCode 617. 合并二叉树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if (t2) swap(t1, t2);  // 可以保证t1一定不为空
        if (!t1) return NULL;
        if (t2) t1->val += t2->val;
        t1->left = mergeTrees(t1->left, t2 ? t2->left : NULL);
        t1->right = mergeTrees(t1->right, t2 ? t2->right : NULL);
        return t1;
    }
};
```
##### LeetCode 621. 任务调度器
```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char, int> hash;
        for (auto c: tasks) hash[c] ++ ;
        int maxc = 0, cnt = 0;
        for (auto [k, v]: hash) maxc = max(maxc, v);
        for (auto [k, v]: hash)
            if (maxc == v)
                cnt ++ ;
        return max((int)tasks.size(), (maxc - 1) * (n + 1) + cnt);
    }
};
```
##### LeetCode 622. 设计循环队列
```cpp
class MyCircularQueue {
public:
    int hh = 0, tt = 0;
    vector<int> q;

    /** Initialize your data structure here. Set the size of the queue to be k. */
    MyCircularQueue(int k) {
        q.resize(k + 1);
    }

    /** Insert an element into the circular queue. Return true if the operation is successful. */
    bool enQueue(int value) {
        if (isFull()) return false;
        q[tt ++ ] = value;
        if (tt == q.size()) tt = 0;
        return true;
    }

    /** Delete an element from the circular queue. Return true if the operation is successful. */
    bool deQueue() {
        if (isEmpty()) return false;
        hh ++ ;
        if (hh == q.size()) hh = 0;
        return true;
    }

    /** Get the front item from the queue. */
    int Front() {
        if (isEmpty()) return -1;
        return q[hh];
    }

    /** Get the last item from the queue. */
    int Rear() {
        if (isEmpty()) return -1;
        int t = tt - 1;
        if (t < 0) t += q.size();
        return q[t];
    }

    /** Checks whether the circular queue is empty or not. */
    bool isEmpty() {
        return hh == tt;
    }

    /** Checks whether the circular queue is full or not. */
    bool isFull() {
        return (tt + 1) % q.size() == hh;
    }
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */

```
##### LeetCode 623. 在二叉树中增加一行
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        if (d == 1) {
            auto cur = new TreeNode(v);
            cur->left = root;
            return cur;
        }
        queue<TreeNode*> q;
        q.push(root);
        for (int i = 0; i < d - 2; i ++ ) {
            for (int j = q.size(); j; j -- ) {
                auto t = q.front();
                q.pop();
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }
        }

        while (q.size()) {
            auto t = q.front();
            q.pop();
            auto left = new TreeNode(v), right = new TreeNode(v);
            left->left = t->left, right->right = t->right;
            t->left = left, t->right = right;
        }
        return root;
    }
};

```
##### LeetCode 628. 三个数的最大乘积
```cpp
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        return max(nums[n - 1] * nums[n - 2] * nums[n - 3], nums[0] * nums[1] * nums[n - 1]);
    }
};
```



### 究极班—— Week 26(第 629 ~ 630、632 ~ 633、636 ~ 641、643、645 ~ 650、652 ~ 654 题）


#####  LeetCode 629. K个逆序对数组
```cpp
class Solution {
public:
    int kInversePairs(int n, int k) {
        int mod = 1e9 + 7;
        vector<vector<int>> f(n + 1, vector<int>(k + 1));
        f[1][0] = 1;
        for (int i = 2; i <= n; i ++ ) {
            long long s = 0;
            for (int j = 0; j <= k; j ++ ) {
                s += f[i - 1][j];
                if (j - i >= 0) s -= f[i - 1][j - i];
                f[i][j] = s % mod;
            }
        }
        return (f[n][k] + mod) % mod;
    }
};
```
#####  LeetCode 630. 课程表 III
```cpp
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        sort(courses.begin(), courses.end(), [](vector<int>& a, vector<int>& b) {
            return a[1] < b[1];
        });
        priority_queue<int> heap;
        int tot = 0;
        for (auto& c: courses) {
            tot += c[0];
            heap.push(c[0]);
            if (tot > c[1]) {
                tot -= heap.top();
                heap.pop();
            }
        }
        return heap.size();
    }
};

```
#####  LeetCode 632. 最小区间
```cpp
class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        typedef vector<int> VI;
        VI res;
        priority_queue<VI, vector<VI>, greater<VI>> heap;
        int maxv = INT_MIN;
        for (int i = 0; i < nums.size(); i ++ ) {
            heap.push({nums[i][0], i, 0});
            maxv = max(maxv, nums[i][0]);
        }
        while (heap.size()) {
            auto t = heap.top();
            heap.pop();
            int l = t[0], r = maxv;
            if (res.empty() || res[1] - res[0] > r - l)
                res = {l, r};
            int i = t[1], j = t[2] + 1;
            if (j < nums[i].size()) {
                heap.push({nums[i][j], i, j});
                maxv = max(maxv, nums[i][j]);
            } else break;
        }
        return res;
    }
};
```
#####  LeetCode 633. 平方数之和
```cpp
class Solution {
public:
    bool judgeSquareSum(int c) {
        for (long long i = 0; i * i <= c; i ++ ) {
            int j = c - i * i;
            int r = sqrt(j);
            if (r * r == j) return true;
        }
        return false;
    }
};
```
#####  LeetCode 636. 函数的独占时间
```cpp
class Solution {
public:
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        vector<int> res(n);
        stack<int> stk;
        int last;
        for (auto& log: logs) {
            int x = log.find(':'), y = log.substr(x + 1).find(':') + x + 1;
            int id = stoi(log.substr(0, x)), ts = stoi(log.substr(y + 1));
            if (log.substr(x + 1, y - x - 1) == "start") {
                if (stk.size()) res[stk.top()] += ts - last;
                stk.push(id);
                last = ts;
            } else {
                res[stk.top()] += ts - last + 1;
                stk.pop();
                last = ts + 1;
            }
        }
        return res;
    }
};
```
#####  LeetCode 637. 二叉树的层平均值
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> res;
        if (!root) return res;
        queue<TreeNode*> q;
        q.push(root);
        while (q.size()) {
            int sz = q.size();
            double sum = 0;
            for (int i = 0; i < sz; i ++ ) {
                auto t = q.front();
                q.pop();
                sum += t->val;
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }
            res.push_back(sum / sz);
        }
        return res;
    }
};
```
#####  LeetCode 638. 大礼包
```cpp
class Solution {
public:
    int n;
    vector<unordered_map<int, int>> f;
    vector<int> price;
    vector<vector<int>> special;

    int dp(int x, int y) {
        if (f[x].count(y)) return f[x][y];
        if (!x) {
            f[x][y] = 0;
            for (int i = 0; i < n; i ++ ) {
                int c = y >> i * 4 & 15;
                f[x][y] += c * price[i];
            }
            return f[x][y];
        }
        f[x][y] = dp(x - 1, y);
        int state = 0;
        auto s = special[x - 1];
        for (int i = n - 1; i >= 0; i -- ) {
            int c = y >> i * 4 & 15;
            if (c < s[i]) {
                state = -1;
                break;
            }
            state = state * 16 + c - s[i];
        }
        if (state != -1)
            f[x][y] = min(f[x][y], dp(x, state) + s.back());
        return f[x][y];
    }

    int shoppingOffers(vector<int>& _price, vector<vector<int>>& _special, vector<int>& needs) {
        price = _price, special = _special;
        n = price.size();
        f = vector<unordered_map<int, int>>(special.size() + 1, unordered_map<int, int>());
        int state = 0;
        for (int i = needs.size() - 1; i >= 0; i -- )
            state = state * 16 + needs[i];
        return dp(special.size(), state);
    }
};
```
#####  LeetCode 639. 解码方法
```cpp
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size(), MOD = 1e9 + 7;
        vector<long long> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= 26; j ++ ) {
                char a = s[i - 1];
                if (j <= 9) {
                    if (a == '*' || a == j + '0') f[i] += f[i - 1];
                } else if (i >= 2) {
                    char b = s[i - 2];
                    int y = j / 10, x = j % 10;
                    if ((b == y + '0' || b == '*' && y) && (a == x + '0' || a == '*' && x))
                        f[i] += f[i - 2];
                }
                f[i] %= MOD;
            }
        return f[n];
    }
};
```

#####  LeetCode 640. 求解方程
```cpp
class Solution {
public:
    pair<int, int> work(string str) {
        int a = 0, b = 0;
        if (str[0] != '+' && str[0] != '-') str = '+' + str;
        for (int i = 0; i < str.size(); i ++ ) {
            int j = i + 1;
            while (j < str.size() && isdigit(str[j])) j ++ ;
            int c = 1;
            if (i + 1 <= j - 1) c = stoi(str.substr(i + 1, j - 1 - i));
            if (str[i] == '-') c = -c;
            if (j < str.size() && str[j] == 'x') {
                a += c;
                i = j;
            } else {
                b += c;
                i = j - 1;
            }
        }
        return {a, b};
    }

    string solveEquation(string equation) {
        int k = equation.find('=');
        auto left = work(equation.substr(0, k)), right = work(equation.substr(k + 1));
        int a = left.first - right.first, b = right.second - left.second;
        if (!a) {
            if (!b) return "Infinite solutions";
            else return "No solution";
        }
        return "x=" + to_string(b / a);
    }
};
```
#####  LeetCode 641. 设计循环双端队列
```cpp
class MyCircularDeque {
public:
    int hh = 0, tt = 0;
    vector<int> q;

    /** Initialize your data structure here. Set the size of the deque to be k. */
    MyCircularDeque(int k) {
        q.resize(k + 1);
    }

    int get(int x) {
        return (x + q.size()) % q.size();
    }

    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    bool insertFront(int value) {
        if (isFull()) return false;
        hh = get(hh - 1);
        q[hh] = value;
        return true;
    }

    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    bool insertLast(int value) {
        if (isFull()) return false;
        q[tt ++ ] = value;
        tt = get(tt);
        return true;
    }

    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    bool deleteFront() {
        if (isEmpty()) return false;
        hh = get(hh + 1);
        return true;
    }

    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    bool deleteLast() {
        if (isEmpty()) return false;
        tt = get(tt - 1);
        return true;
    }

    /** Get the front item from the deque. */
    int getFront() {
        if (isEmpty()) return -1;
        return q[hh];
    }

    /** Get the last item from the deque. */
    int getRear() {
        if (isEmpty()) return -1;
        return q[get(tt - 1)];
    }

    /** Checks whether the circular deque is empty or not. */
    bool isEmpty() {
        return hh == tt;
    }

    /** Checks whether the circular deque is full or not. */
    bool isFull() {
        return get(hh - 1) == tt;
    }
};

/**
 * Your MyCircularDeque object will be instantiated and called as such:
 * MyCircularDeque* obj = new MyCircularDeque(k);
 * bool param_1 = obj->insertFront(value);
 * bool param_2 = obj->insertLast(value);
 * bool param_3 = obj->deleteFront();
 * bool param_4 = obj->deleteLast();
 * int param_5 = obj->getFront();
 * int param_6 = obj->getRear();
 * bool param_7 = obj->isEmpty();
 * bool param_8 = obj->isFull();
 */
```


#####  LeetCode  643. 子数组最大平均数 I
```C++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double res = -1e5;
        for (int i = 0, j = 0, s = 0; i < nums.size(); i ++ ) {
            s += nums[i];
            if (i - j + 1 > k) s -= nums[j ++ ];
            if (i >= k - 1) res = max(res, s / (double)k);
        }
        return res;
    }
};

```
#####  LeetCode  645. 错误的集合
```C++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        vector<int> res(2);
        for (auto x: nums) {
            int k = abs(x);
            if (nums[k - 1] < 0)
                res[0] = k;
            nums[k - 1] *= -1;
        }

        for (int i = 0; i < nums.size(); i ++ ) {
            if (nums[i] > 0 && i + 1 != res[0]) {
                res[1] = i + 1;
            }
        }

        return res;
    }
};

```
#####  LeetCode  646. 最长数对链
```C++
class Solution {
public:
    int findLongestChain(vector<vector<int>>& pairs) {
        sort(pairs.begin(), pairs.end(), [](vector<int>& a, vector<int>& b){
            return a[1] < b[1];
        });

        int res = 1, ed = pairs[0][1];
        for (auto& p: pairs) {
            if (p[0] > ed) {
                res ++ ;
                ed = p[1];
            }
        }

        return res;
    }
};

```
#####  LeetCode  647. 回文子串
```C++
class Solution {
public:
    int countSubstrings(string s) {
        int res = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            // 枚举长度为奇数的情况
            for (int j = i, k = i; j >= 0 && k < s.size(); j --, k ++ ) {
                if (s[j] != s[k]) break;
                res ++ ;
            }

            // 偶数情况
            for (int j = i, k = i + 1; j >= 0 && k < s.size(); j --, k ++ ) {
                if (s[j] != s[k]) break;
                res ++ ;
            }
        }
        return res;
    }
};
```
#####  LeetCode  648. 单词替换
```C++
class Solution {
public:
    string replaceWords(vector<string>& dictionary, string sentence) {
        typedef unsigned long long ULL;
        const int P = 131;
        unordered_set<ULL> hash;
        for (auto& s: dictionary) {
            ULL h = 0;
            for (auto c: s) h = h * P + c;
            hash.insert(h);
        }

        stringstream ssin(sentence);
        string res, word;
        while (ssin >> word) {
            string s;
            ULL h = 0;
            for (auto c: word) {
                s += c;
                h = h * P + c;
                if (hash.count(h)) break;
            }
            res += s + ' ';
        }
        res.pop_back();
        return res;
    }
};

```
#####  LeetCode  649. Dota2 参议院
```C++
class Solution {
public:
    string predictPartyVictory(string senate) {
        queue<int> r, d;
        for (int i = 0; i < senate.size(); i ++ ) {
            if (senate[i] == 'R') r.push(i);
            else d.push(i);
        }

        int n = senate.size();
        while (r.size() && d.size()) {
            if (r.front() < d.front()) r.push(r.front() + n);
            else d.push(d.front() + n);
            r.pop(), d.pop();
        }

        if (r.size()) return "Radiant";
        return "Dire";
    }
};
```
#####  LeetCode  650. 只有两个键的键盘
```C++
class Solution {
public:
    int minSteps(int n) {
        int res = 0;
        for (int i = 2; i <= n / i; i ++ ) {
            while (n % i == 0) {
                res += i;
                n /= i;
            }
        }
        if (n > 1) res += n;
        return res;
    }
};
```
#####  LeetCode  652. 寻找重复的子树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    unordered_map<string, int> ids;
    int cnt = 0;
    unordered_map<int, int> hash;
    vector<TreeNode*> ans;

    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root) {
        if (!root) return 0;
        int left = dfs(root->left);
        int right = dfs(root->right);
        string key = to_string(root->val) + ' ' + to_string(left) + ' ' + to_string(right);
        if (ids.count(key) == 0) ids[key] = ++ cnt;
        int id = ids[key];
        if (++ hash[id] == 2) ans.push_back(root);
        return id;
    }
};

```
#####  LeetCode  653. 两数之和 IV - 输入 BST
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    unordered_set<int> hash;

    bool findTarget(TreeNode* root, int k) {
        return dfs(root, k);
    }

    bool dfs(TreeNode* root, int k) {
        if (!root) return false;
        if (dfs(root->left, k)) return true;
        int x = root->val;
        if (hash.count(k - x)) return true;
        hash.insert(x);
        return dfs(root->right, k);
    }
};

```
#####  LeetCode  654. 最大二叉树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int n, k;
    vector<vector<int>> f;
    vector<int> nums;

    TreeNode* constructMaximumBinaryTree(vector<int>& _nums) {
        nums = _nums;
        n = nums.size();
        k = log(n) / log(2);
        f = vector<vector<int>>(n, vector<int>(k + 1));
        for (int j = 0; j <= k; j ++ )
            for (int i = 0; i + (1 << j) - 1 < n; i ++ ) {
                if (!j) f[i][j] = i;
                else {
                    int l = f[i][j - 1], r = f[i + (1 << j - 1)][j - 1];
                    if (nums[l] > nums[r]) f[i][j] = l;
                    else f[i][j] = r;
                }
            }

        return build(0, n - 1);
    }

    int query(int l, int r) {
        int len = r - l + 1;
        int k = log(len) / log(2);
        int a = f[l][k], b = f[r - (1 << k) + 1][k];
        if (nums[a] > nums[b]) return a;
        return b;
    }

    TreeNode* build(int l, int r) {
        if (l > r) return NULL;
        int k = query(l, r);
        auto root = new TreeNode(nums[k]);
        root->left = build(l, k - 1);
        root->right = build(k + 1, r);
        return root;
    }
};

```

### 究极班——Week 27(第 655、657 ~ 659、661 ~ 662、664 ~ 665、667 ~ 678 题）

#####  LeetCode  655. 输出二叉树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<string>> ans;

    vector<int> dfs(TreeNode* root) {
        if (!root) return {0, 0};
        auto l = dfs(root->left), r = dfs(root->right);
        return {max(l[0], r[0]) + 1, max(l[1], r[1]) * 2 + 1};;
    }

    void print(TreeNode* root, int h, int l, int r) {
        if (!root) return;
        int mid = (l + r) / 2;
        ans[h][mid] = to_string(root->val);
        print(root->left, h + 1, l, mid - 1);
        print(root->right, h + 1, mid + 1, r);
    }

    vector<vector<string>> printTree(TreeNode* root) {
        auto t = dfs(root);
        int h = t[0], w = t[1];
        ans = vector<vector<string>>(h, vector<string>(w));
        print(root, 0, 0, w - 1);
        return ans;
    }
};
```
#####  LeetCode  657. 机器人能否返回原点
```C++
class Solution {
public:
    bool judgeCircle(string moves) {
        int x = 0, y = 0;
        for (auto c: moves) {
            if (c == 'U') x -- ;
            else if (c == 'R') y ++ ;
            else if (c == 'D') x ++ ;
            else y -- ;
        }
        return !x && !y;
    }
};
```
#####  LeetCode  658. 找到 K 个最接近的元素

```C++

堆，nlogknlogk

class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        priority_queue<pair<int, int>> heap;
        for (auto v: arr) {
            heap.push({abs(x - v), v});
            if (heap.size() > k) heap.pop();
        }
        vector<int> res;
        while (heap.size()) res.push_back(heap.top().second), heap.pop();
        sort(res.begin(), res.end());
        return res;
    }
};

二分 + 双指针，logn+klogn+k

class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int T) {
        int l = 0, r = arr.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (arr[mid] >= T) r = mid;
            else l = mid + 1;
        }
        if (r) {
            int x = arr[r - 1], y = arr[r];
            if (make_pair(abs(x - T), x) < make_pair(abs(y - T), y))
                r -- ;
        }
        int i = r, j = r;
        for (int u = 0; u < k - 1; u ++ ) {
            if (i - 1 < 0) j ++ ;
            else if (j + 1 >= arr.size()) i -- ;
            else {
                int x = arr[i - 1], y = arr[j + 1];
                pair<int, int> a(abs(x - T), x), b(abs(y - T), y);
                if (a < b) i -- ;
                else j ++ ;
            }
        }
        vector<int> res;
        for (int u = i; u <= j; u ++ ) res.push_back(arr[u]);
        return res;
    }
};

```
#####  LeetCode  659. 分割数组为连续子序列
```C++
class Solution {
public:
    bool isPossible(vector<int>& nums) {
        unordered_map<int, int> cnt1, cnt2;
        for (auto x: nums) cnt1[x] ++ ;
        for (auto x: nums) {
            if (!cnt1[x]) continue;
            if (cnt2[x - 1]) {
                cnt2[x - 1] -- ;
                cnt2[x] ++ ;
                cnt1[x] -- ;
            } else if (cnt1[x + 1] && cnt1[x + 2]) {
                cnt2[x + 2] ++ ;
                cnt1[x] --, cnt1[x + 1] --, cnt1[x + 2] -- ;
            } else return false;
        }
        return true;
    }
};

```
#####  LeetCode  661. 图片平滑器
```C++
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& M) {
        int n = M.size(), m = M[0].size();
        vector<vector<int>> res = M;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                int s = 0, c = 0;
                for (int x = i - 1; x <= i + 1; x ++ )
                    for (int y = j - 1; y <= j + 1; y ++ )
                        if (x >= 0 && x < n && y >= 0 && y < m)
                            s += M[x][y], c ++ ;
                res[i][j] = s / c;
            }
        return res;
    }
};

```
#####  LeetCode  662. 二叉树最大宽度
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int widthOfBinaryTree(TreeNode* root) {
        if (!root) return 0;
        queue<pair<TreeNode*, int>> q;
        q.push({root, 1});
        int res = 1;
        while (q.size()) {
            int sz = q.size();
            int l = q.front().second, r;

            for (int i = 0; i < sz; i ++ ) {
                auto t = q.front();
                q.pop();
                auto v = t.first;
                auto p = t.second - l + 1;
                r = t.second;
                if (v->left) q.push({v->left, p * 2ll});
                if (v->right) q.push({v->right, p * 2ll + 1});
            }
            res = max(res, r - l + 1);
        }
        return res;
    }
};

```
#####  LeetCode  664. 奇怪的打印机
```C++
class Solution {
public:
    int strangePrinter(string s) {
        int n = s.size();
        if (!n) return 0;
        vector<vector<int>> f(n + 1, vector<int>(n + 1));
        for (int len = 1; len <= n; len ++ )
            for (int i = 0; i + len - 1 < n; i ++ ) {
                int j = i + len - 1;
                f[i][j] = 1 + f[i + 1][j];
                for (int k = i + 1; k <= j; k ++ )
                    if (s[k] == s[i])
                        f[i][j] = min(f[i][j], f[i][k - 1] + f[k + 1][j]);
            }
        return f[0][n - 1];
    }
};

```
#####  LeetCode  665. 非递减数列
```C++
class Solution {
public:
    bool check(vector<int>& nums) {
        for (int i = 1; i < nums.size(); i ++ )
            if (nums[i] < nums[i - 1])
                return false;
        return true;
    }

    bool checkPossibility(vector<int>& nums) {
        for (int i = 1; i < nums.size(); i ++ )
            if (nums[i] < nums[i - 1]) {
                int a = nums[i - 1], b = nums[i];
                nums[i - 1] = nums[i] = a;
                if (check(nums)) return true;
                nums[i - 1] = nums[i] = b;
                if (check(nums)) return true;
                return false;
            }
        return true;
    }
};

```
#####  LeetCode  667. 优美的排列 II
```C++
class Solution {
public:
    vector<int> constructArray(int n, int k) {
        vector<int> res(n);
        for (int i = 0; i < n - k - 1; i ++ ) res[i] = i + 1;
        int u = n - k - 1;
        int i = n - k, j = n;
        while (u < n) {
            res[u ++ ] = i ++ ;
            if (u < n) res[u ++ ] = j -- ;
        }
        return res;
    }
};

```
#####  LeetCode  668. 乘法表中第k小的数
```C++
class Solution {
public:
    int get(int m, int n, int mid) {
        int res = 0;
        for (int i = 1; i <= n; i ++ )
            res += min(m, mid / i);
        return res;
    }

    int findKthNumber(int m, int n, int k) {
        int l = 1, r = n * m;
        while (l < r) {
            int mid = l + r >> 1;
            if (get(m, n, mid) >= k) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};

```
#####  LeetCode  669. 修剪二叉搜索树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) return nullptr;
        if (root->val < low) return trimBST(root->right, low, high);
        if (root->val > high) return trimBST(root->left, low, high);
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }
};

```
#####  LeetCode  670. 最大交换
```C++
class Solution {
public:
    int maximumSwap(int num) {
        string str = to_string(num);
        for (int i = 0; i + 1 < str.size(); i ++ ) {
            if (str[i] < str[i + 1]) {
                int k = i + 1;
                for (int j = k; j < str.size(); j ++ )
                    if (str[j] >= str[k])
                        k = j;
                for (int j = 0; ; j ++ )
                    if (str[j] < str[k]) {
                        swap(str[j], str[k]);
                        return stoi(str);
                    }
            }
        }
        return num;
    }
};

```
#####  LeetCode  671. 二叉树中第二小的节点
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    long long d1, d2;

    void dfs(TreeNode* root) {
        if (!root) return;
        int x = root->val;
        if (x < d1) d2 = d1, d1 = x;
        else if (x > d1 && x < d2) d2 = x;
        dfs(root->left), dfs(root->right);
    }

    int findSecondMinimumValue(TreeNode* root) {
        d1 = d2 = 1e18;
        dfs(root);
        if (d2 == 1e18) d2 = -1;
        return d2;
    }
};

```
#####  LeetCode  672. 灯泡开关 Ⅱ
```C++
class Solution {
public:
    int state[8][6] = {
        {1, 1, 1, 1, 1, 1},  // 不按
        {0, 0, 0, 0, 0, 0},  // 1
        {1, 0, 1, 0, 1, 0},  // 2
        {0, 1, 0, 1, 0, 1},  // 3
        {0, 1, 1, 0, 1, 1},  // 4
        {1, 0, 0, 1, 0, 0},  // 14
        {0, 0, 1, 1, 1, 0},  // 24
        {1, 1, 0, 0, 0, 1},  // 34
    };

    int work(int n, vector<int> ops) {
        set<int> S;
        for (auto op: ops) {
            int t = 0;
            for (int i = 0; i < n; i ++ )
                t = t * 2 + state[op][i];
            S.insert(t);
        }
        return S.size();
    }

    int flipLights(int n, int m) {
        n = min(n, 6);
        if (m == 0) return work(n, {0});
        else if (m == 1) return work(n, {1, 2, 3, 4});
        else if (m == 2) return work(n, {0, 1, 2, 3, 5, 6, 7});
        else return work(n, {0, 1, 2, 3, 4, 5, 6, 7});
    }
};

```
#####  LeetCode  673. 最长递增子序列的个数
```C++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n), g(n);

        int maxl = 0, cnt = 0;
        for (int i = 0; i < n; i ++ ) {
            f[i] = g[i] = 1;
            for (int j = 0; j < i; j ++ )
                if (nums[j] < nums[i]) {
                    if (f[i] < f[j] + 1) f[i] = f[j] + 1, g[i] = g[j];
                    else if (f[i] == f[j] + 1) g[i] += g[j];
                }
            if (maxl < f[i]) maxl = f[i], cnt = g[i];
            else if (maxl == f[i]) cnt += g[i];
        }
        return cnt;
    }
};
```
#####  LeetCode  674. 最长连续递增序列
```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int res = 0;
        for (int i = 0; i < nums.size(); i ++ ) {
            int j = i + 1;
            while (j < nums.size() && nums[j] > nums[j - 1]) j ++ ;
            res = max(res, j - i);
            i = j - 1;
        }
        return res;
    }
};

```
#####  LeetCode  675. 为高尔夫比赛砍树
```C++
class Solution {
public:
    struct Tree {
        int x, y, h;
        bool operator< (const Tree& t) const {
            return h < t.h;
        }
    };
    int n, m;
    vector<vector<int>> g;

    int bfs(Tree start, Tree end) {
        if (start.x == end.x && start.y == end.y) return 0;
        queue<Tree> q;
        const int INF = 1e8;
        vector<vector<int>> dist(n, vector<int>(m, INF));
        dist[start.x][start.y] = 0;
        q.push({start});
        int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
        while (q.size()) {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < 4; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m && g[x][y]) {
                    if (dist[x][y] > dist[t.x][t.y] + 1) {
                        dist[x][y] = dist[t.x][t.y] + 1;
                        if (x == end.x && y == end.y) return dist[x][y];
                        q.push({x, y});
                    }
                }
            }
        }
        return -1;
    }

    int cutOffTree(vector<vector<int>>& forest) {
        g = forest;
        n = g.size(), m = g[0].size();
        vector<Tree> trs;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] > 1)
                    trs.push_back({i, j, g[i][j]});
        sort(trs.begin(), trs.end());
        Tree last = {0, 0};
        int res = 0;
        for (auto& tr: trs) {
            int t = bfs(last, tr);
            if (t == -1) return -1;
            res += t;
            last = tr;
        }
        return res;
    }
};

```
#####  LeetCode  676. 实现一个魔法字典
```C++
const int N = 10010;

int son[N][26], idx;
bool is_end[N];

class MagicDictionary {
public:
    void insert(string& s) {
        int p = 0;
        for (auto c: s) {
            int u = c - 'a';
            if (!son[p][u]) son[p][u] = ++ idx;
            p = son[p][u];
        }
        is_end[p] = true;
    }

    /** Initialize your data structure here. */
    MagicDictionary() {
        memset(son, 0, sizeof son);
        idx = 0;
        memset(is_end, 0, sizeof is_end);
    }

    void buildDict(vector<string> dictionary) {
        for (auto& s: dictionary) insert(s);
    }

    bool dfs(string& s, int p, int u, int c) {
        if (is_end[p] && u == s.size() && c == 1) return true;
        if (c > 1 || u == s.size()) return false;

        for (int i = 0; i < 26; i ++ ) {
            if (!son[p][i]) continue;
            if (dfs(s, son[p][i], u + 1, c + (s[u] - 'a' != i)))
                return true;
        }
        return false;
    }

    bool search(string searchWord) {
        return dfs(searchWord, 0, 0, 0);
    }
};

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary* obj = new MagicDictionary();
 * obj->buildDict(dictionary);
 * bool param_2 = obj->search(searchWord);
 */

```

#####  LeetCode  677. 键值映射
```C++
const int N = 2510;

int son[N][26], V[N], S[N], idx;

class MapSum {
public:
    void add(string& s, int value, int last) {
        int p = 0;
        for (auto c: s) {
            int u = c - 'a';
            if (!son[p][u]) son[p][u] = ++ idx;
            p = son[p][u];
            S[p] += value - last;
        }
        V[p] = value;
    }

    int query(string& s) {
        int p = 0;
        for (auto c: s) {
            int u = c - 'a';
            if (!son[p][u]) return 0;
            p = son[p][u];
        }
        return p;
    }

    /** Initialize your data structure here. */
    MapSum() {
        memset(son, 0, sizeof son);
        idx = 0;
        memset(V, 0, sizeof V);
        memset(S, 0, sizeof S);
    }

    void insert(string key, int val) {
        add(key, val, V[query(key)]);
    }

    int sum(string prefix) {
        return S[query(prefix)];
    }
};

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum* obj = new MapSum();
 * obj->insert(key,val);
 * int param_2 = obj->sum(prefix);
 */

```
#####  LeetCode  678. 有效的括号字符串
```C++
class Solution {
public:
    bool checkValidString(string s) {
        int low = 0, high = 0;
        for (auto c: s) {
            if (c == '(') low ++, high ++ ;
            else if (c == ')') low -- , high -- ;
            else low --, high ++ ;
            low = max(low, 0);
            if (low > high) return false;
        }
        return !low;
    }
};

```

### 究极班-Week 28（第 679 ~ 680、682、684 ~ 693、695 ~ 701 题）

##### LeetCode 679. 24 点游戏
```cpp
class Solution {
public:
    bool judgePoint24(vector<int>& nums) {
        vector<double> a(nums.begin(), nums.end());
        return dfs(a);
    }

    vector<double> get(vector<double>& nums, int i, int j, double x) {
        vector<double> res;
        for (int k = 0; k < nums.size(); k ++ )
            if (k != i && k != j)
                res.push_back(nums[k]);
        res.push_back(x);
        return res;
    }

    bool dfs(vector<double> nums) {
        if (nums.size() == 1) return fabs(nums[0] - 24) < 1e-8;
        for (int i = 0; i < nums.size(); i ++ )
            for (int j = 0; j < nums.size(); j ++ )
                if (i != j) {
                    double a = nums[i], b = nums[j];
                    if (dfs(get(nums, i, j, a + b))) return true;
                    if (dfs(get(nums, i, j, a - b))) return true;
                    if (dfs(get(nums, i, j, a * b))) return true;
                    if (b && dfs(get(nums, i, j, a / b))) return true;
                }
        return false;
    }
};
```
##### LeetCode 680. 验证回文字符串 Ⅱ
```cpp
class Solution {
public:
    bool check(string& s, int i, int j) {
        while (i < j) {
            if (s[i] != s[j]) return false;
            i ++, j -- ;
        }
        return true;
    }

    bool validPalindrome(string s) {
        for (int i = 0, j = s.size() - 1; i < j; i ++, j -- )
            if (s[i] != s[j]) {
                if (check(s, i + 1, j) || check(s, i, j - 1)) return true;
                return false;
            }
        return true;
    }
};
```
##### LeetCode 682. 棒球比赛
```cpp
class Solution {
public:
    int calPoints(vector<string>& ops) {
        vector<int> stk;
        for (auto& s: ops) {
            int p = stk.size() - 1;
            if (s == "+") stk.push_back(stk[p - 1] + stk[p]);
            else if (s == "D") stk.push_back(stk[p] * 2);
            else if (s == "C") stk.pop_back();
            else stk.push_back(stoi(s));
        }
        return accumulate(stk.begin(), stk.end(), 0);
    }
};
```
##### LeetCode 684. 冗余连接
```cpp
class Solution {
public:
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        p.resize(n + 1);
        for (int i = 1; i <= n; i ++ ) p[i] = i;

        for (auto& e: edges) {
            int a = find(e[0]), b = find(e[1]);
            if (a != b) p[a] = b;
            else return e;
        }
        return {};
    }
};

```
##### LeetCode 685. 冗余连接 II
```cpp
class Solution {
public:
    int n;
    vector<bool> st1, st2, st, in_k, in_c;
    vector<vector<int>> g;
    stack<int> stk;

    bool dfs(int u) {
        st[u] = true;
        stk.push(u), in_k[u] = true;

        for (int x: g[u]) {
            if (!st[x]) {
                if (dfs(x)) return true;
            }
            else if (in_k[x]) {
                while (stk.top() != x) {
                    in_c[stk.top()] = true;
                    stk.pop();
                }
                in_c[x] = true;
                return true;
            }
        }

        stk.pop(), in_k[u] = false;
        return false;
    }

    void work1(vector<vector<int>>& edges) {
        for (auto& e: edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
        }
        for (int i = 1; i <= n; i ++ )
            if (!st[i] && dfs(i))
                break;

        for (int i = 0; i < n; i ++ ) {
            int a = edges[i][0], b = edges[i][1];
            if (in_c[a] && in_c[b])
                st1[i] = true;
        }
    }

    void work2(vector<vector<int>>& edges) {
        vector<int> p(n + 1, -1);
        for (int i = 0; i < n; i ++ ) {
            int a = edges[i][0], b = edges[i][1];
            if (p[b] != -1) {
                st2[p[b]] = st2[i] = true;
                break;
            }
            else p[b] = i;
        }
    }

    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        n = edges.size();
        st1 = st2 = st = in_k = in_c = vector<bool>(n + 1);
        g.resize(n + 1);
        work1(edges);
        work2(edges);

        for (int i = n - 1; i >= 0; i -- )
            if (st1[i] && st2[i])
                return edges[i];
        for (int i = n - 1; i >= 0; i -- )
            if (st1[i] || st2[i])
                return edges[i];
        return {};
    }
};
```
##### LeetCode 686. 重复叠加字符串匹配
```cpp
class Solution {
public:
    int repeatedStringMatch(string a, string p) {
        string s;
        while (s.size() < p.size()) s += a;
        s += a;
        int n = s.size(), m = p.size();
        s = ' ' + s, p = ' ' + p;

        vector<int> next(m + 1);
        for (int i = 2, j = 0; i <= m; i ++ ) {
            while (j && p[i] != p[j + 1]) j = next[j];
            if (p[i] == p[j + 1]) j ++ ;
            next[i] = j;
        }
        for (int i = 1, j = 0; i <= n; i ++ ) {
            while (j && s[i] != p[j + 1]) j = next[j];
            if (s[i] == p[j + 1]) j ++ ;
            if (j == m) return (i + a.size() - 1) / a.size();
        }
        return -1;
    }
};
```
##### LeetCode 687. 最长同值路径
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int ans;

    int longestUnivaluePath(TreeNode* root) {
        ans = 0;
        dfs(root);
        return ans;
    }
    int dfs(TreeNode* root) {
        if (!root) return 0;
        int l = dfs(root->left), r = dfs(root->right);
        if (!root->left || root->left->val != root->val) l = 0;
        if (!root->right || root->right->val != root->val) r = 0;
        ans = max(ans, l + r);
        return max(l, r) + 1;
    }
};
```
##### LeetCode 688. “马”在棋盘上的概率
```cpp
double f[25][25][101];

class Solution {
public:
    double knightProbability(int n, int K, int r, int c) {
        memset(f, 0, sizeof f);
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                f[i][j][K] = 1;

        int dx[] = {-2, -1, 1, 2, 2, 1, -1, -2};
        int dy[] = {1, 2, 2, 1, -1, -2, -2, -1};
        for (int k = K - 1; k >= 0; k -- )
            for (int i = 0; i < n; i ++ )
                for (int j = 0; j < n; j ++ )
                    for (int u = 0; u < 8; u ++ ) {
                        int x = i + dx[u], y = j + dy[u];
                        if (x >= 0 && x < n && y >= 0 && y < n)
                            f[i][j][k] += f[x][y][k + 1] / 8;
                    }
        return f[r][c][0];
    }
};
```
##### LeetCode 689. 三个无重叠子数组的最大和
```cpp
class Solution {
public:
    vector<int> maxSumOfThreeSubarrays(vector<int> nums, int k) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + nums[i - 1];
        vector<vector<int>> f(n + 2, vector<int>(4));

        int x = n + 1, y = 3;
        for (int i = n - k + 1; i; i -- ) {
            for (int j = 1; j <= 3; j ++ )
                f[i][j] = max(f[i + 1][j], f[i + k][j - 1] + s[i + k - 1] - s[i - 1]);
            if (f[x][3] <= f[i][3]) x = i;
        }

        vector<int> res;
        while (y) {
            while (f[x][y] != f[x + k][y - 1] + s[x + k - 1] - s[x - 1]) x ++ ;
            res.push_back(x - 1);
            x += k, y -- ;
        }
        return res;
    }
};
```
##### LeetCode 690. 员工的重要性
```cpp
/*
// Definition for Employee.
class Employee {
public:
    int id;
    int importance;
    vector<int> subordinates;
};
*/

class Solution {
public:
    unordered_map<int, Employee*> hash;

    int getImportance(vector<Employee*> employees, int id) {
        for (auto e: employees) hash[e->id] = e;
        return dfs(id);
    }
    int dfs(int id) {
        auto p = hash[id];
        int res = p->importance;
        for (auto x: p->subordinates)
            res += dfs(x);
        return res;
    }
};
```
##### LeetCode 691. 贴纸拼词
```cpp
const int N = 1 << 15;
int f[N], g[N][26];

class Solution {
public:
    const int INF = 20;
    int n;
    string target;
    vector<string> strs;

    int fill(int state, char c) {
        auto& v = g[state][c - 'a'];
        if (v != -1) return v;
        v = state;
        for (int i = 0; i < n; i ++ )
            if (!(state >> i & 1) && target[i] == c) {
                v += 1 << i;
                break;
            }
        return v;
    }

    int dfs(int state) {
        auto& v = f[state];
        if (v != -1) return v;
        if (state == (1 << n) - 1) return v = 0;
        v = INF;
        for (auto& str: strs) {
            int cur = state;
            for (auto c: str)
                cur = fill(cur, c);
            if (cur != state)
                v = min(v, dfs(cur) + 1);
        }
        return v;
    }

    int minStickers(vector<string>& stickers, string _target) {
        memset(f, -1, sizeof f);
        memset(g, -1, sizeof g);
        target = _target;
        strs = stickers;
        n = target.size();
        int res = dfs(0);
        if (res == INF) res = -1;
        return res;
    }
};
```
##### LeetCode 692. 前K个高频单词
```cpp
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> cnt;
        for (auto& w: words) cnt[w] ++ ;
        typedef pair<int, string> PIS;
        vector<PIS> ws;
        for (auto [k, v]: cnt) ws.push_back({v, k});
        auto cmp = [](PIS a, PIS b) {
            if (a.first != b.first) return a.first < b.first;
            return a.second > b.second;
        };
        make_heap(ws.begin(), ws.end(), cmp);
        vector<string> res;
        while (k -- ) {
            res.push_back(ws[0].second);
            pop_heap(ws.begin(), ws.end(), cmp);
            ws.pop_back();
        }
        return res;
    }
};

```
##### LeetCode 693. 交替位二进制数
```cpp
class Solution {
public:
    bool hasAlternatingBits(int n) {
        for (int i = 1; 1ll << i <= n; i ++ ) {
            int a = n >> i - 1 & 1;
            int b = n >> i & 1;
            if (a == b) return false;
        }
        return true;
    }
};
```
##### LeetCode 695. 岛屿的最大面积
```cpp
class Solution {
public:
    int n, m;
    vector<vector<int>> g;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int dfs(int x, int y) {
        int res = 1;
        g[x][y] = 0;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && g[a][b])
                res += dfs(a, b);
        }
        return res;
    }

    int maxAreaOfIsland(vector<vector<int>>& grid) {
        g = grid;
        n = g.size(), m = g[0].size();
        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j]) {
                    res = max(res, dfs(i, j));
                }
        return res;
    }
};
```
##### LeetCode 696. 计数二进制子串
```cpp
class Solution {
public:
    int countBinarySubstrings(string s) {
        int res = 0, last = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            int j = i + 1;
            while (j < s.size() && s[j] == s[i]) j ++ ;
            int cur = j - i;
            i = j - 1;
            res += min(last, cur);
            last = cur;
        }
        return res;
    }
};

```
##### LeetCode 697. 数组的度
```cpp
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        unordered_map<int, int> cnt, minp, maxp;
        int d = 0;
        for (int i = 0; i < nums.size(); i ++ ) {
            int x = nums[i];
            d = max(d, ++ cnt[x]);
            if (!minp.count(x)) minp[x] = i;
            maxp[x] = i;
        }
        int res = INT_MAX;
        for (auto x: nums)
            if (cnt[x] == d)
                res = min(res, maxp[x] - minp[x] + 1);
        return res;
    }
};
```
##### LeetCode 698. 划分为k个相等的子集
```cpp
class Solution {
public:
    int len;
    vector<int> nums;
    vector<bool> st;

    bool dfs(int start, int cur, int k) {
        if (!k) return true;
        if (cur == len) return dfs(0, 0, k - 1);
        for (int i = 0; i < nums.size(); i ++ ) {
            if (st[i]) continue;
            if (cur + nums[i] <= len) {
                st[i] = true;
                if (dfs(i + 1, cur + nums[i], k)) return true;
                st[i] = false;
            }
            while (i + 1 < nums.size() && nums[i + 1] == nums[i]) i ++ ;
            if (!cur || cur + nums[i] == len) return false;
        }
        return false;
    }

    bool canPartitionKSubsets(vector<int>& _nums, int k) {
        nums = _nums;
        st.resize(nums.size());
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum % k) return false;
        len = sum / k;
        sort(nums.begin(), nums.end(), greater<int>());
        return dfs(0, 0, k);
    }
};

```
##### LeetCode 699. 掉落的方块
```cpp
const int N = 3010;

struct Node {
    int l, r, v, c;
}tr[N << 2];

class Solution {
public:
    vector<int> xs;

    int get(int x) {
        return lower_bound(xs.begin(), xs.end(), x) - xs.begin();
    }

    void pushup(int u) {
        tr[u].v = max(tr[u << 1].v, tr[u << 1 | 1].v);
    }

    void pushdown(int u) {
        int c = tr[u].c;
        if (c) {
            auto &l = tr[u << 1], &r = tr[u << 1 | 1];
            tr[u].c = 0;
            l.v = r.v = c;
            l.c = r.c = c;
        }
    }

    void build(int u, int l, int r) {
        tr[u] = {l, r};
        if (l == r) return;
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
    }

    void update(int u, int l, int r, int c) {
        if (tr[u].l >= l && tr[u].r <= r) {
            tr[u].c = tr[u].v = c;
            return;
        }
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) update(u << 1, l, r, c);
        if (r > mid) update(u << 1 | 1, l, r, c);
        pushup(u);
    }

    int query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) return tr[u].v;
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        int res = 0;
        if (l <= mid) res = query(u << 1, l, r);
        if (r > mid) res = max(res, query(u << 1 | 1, l, r));
        return res;
    }

    vector<int> fallingSquares(vector<vector<int>>& pos) {
        for (auto& p: pos) {
            int a = p[0], b = a + p[1];
            xs.push_back(a * 2), xs.push_back(b * 2), xs.push_back(a + b);
        }
        sort(xs.begin(), xs.end());
        xs.erase(unique(xs.begin(), xs.end()), xs.end());

        build(1, 0, xs.size() - 1);
        vector<int> res;
        for (auto& p: pos) {
            int a = p[0], b = a + p[1];
            a = get(a * 2), b = get(b * 2);
            int h = query(1, a + 1, b - 1);
            update(1, a, b, h + p[1]);
            res.push_back(tr[1].v);
        }
        return res;
    }
};

```
##### LeetCode 700. 二叉搜索树中的搜索
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (!root) return NULL;
        if (root->val == val) return root;
        if (root->val > val) return searchBST(root->left, val);
        else return searchBST(root->right, val);
    }
};
```
##### LeetCode 701. 二叉搜索树中的插入操作

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (!root) return new TreeNode(val);
        if (root->val > val) root->left = insertIntoBST(root->left, val);
        else root->right = insertIntoBST(root->right, val);
        return root;
    }
};
```








### 究极班- Week 29（第 703 ~ 707、709 ~ 710、712 ~ 715、717 ~ 722、724 ~ 726 题）

##### LeetCode 703. 数据流中的第 K 大元素
```cpp
class KthLargest {
public:
    priority_queue<int, vector<int>, greater<int>> heap;
    int k;

    KthLargest(int _k, vector<int>& nums) {
        k = _k;
        for (auto x: nums) {
            heap.push(x);
            if (heap.size() > k) heap.pop();
        }
    }

    int add(int val) {
        heap.push(val);
        if (heap.size() > k) heap.pop();
        return heap.top();
    }
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */

```
##### LeetCode 704. 二分查找
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        if (nums[r] != target) return -1;
        return r;
    }
};

```
##### LeetCode 705. 设计哈希集合
```cpp
const int N = 19997;

class MyHashSet {
public:
    vector<int> h[N];

    /** Initialize your data structure here. */
    MyHashSet() {

    }

    int find(vector<int>& h, int key) {
        for (int i = 0; i < h.size(); i ++ )
            if (h[i] == key)
                return i;
        return -1;
    }

    void add(int key) {
        int t = key % N;
        int k = find(h[t], key);
        if (k == -1) h[t].push_back(key);
    }

    void remove(int key) {
        int t = key % N;
        int k = find(h[t], key);
        if (k != -1) h[t].erase(h[t].begin() + k);
    }

    /** Returns true if this set contains the specified element */
    bool contains(int key) {
        int t = key % N;
        int k = find(h[t], key);
        return k != -1;
    }
};

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */

```
##### LeetCode 706. 设计哈希映射
```cpp
const int N = 19997;
typedef pair<int, int> PII;

class MyHashMap {
public:
    vector<PII> h[N];

    /** Initialize your data structure here. */
    MyHashMap() {

    }

    int find(vector<PII>& h, int key) {
        for (int i = 0; i < h.size(); i ++ )
            if (h[i].first == key)
                return i;
        return -1;
    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        int t = key % N;
        int k = find(h[t], key);
        if (k == -1) h[t].push_back({key, value});
        else h[t][k].second = value;
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int t = key % N;
        int k = find(h[t], key);
        if (k == -1) return -1;
        return h[t][k].second;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int t = key % N;
        int k = find(h[t], key);
        if (k != -1) h[t].erase(h[t].begin() + k);
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
```
##### LeetCode 707. 设计链表
```cpp
class MyLinkedList {
public:
    struct Node {
        int val;
        Node* next;
        Node(int _val): val(_val), next(NULL) {}
    }*head;

    /** Initialize your data structure here. */
    MyLinkedList() {
        head = NULL;
    }

    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) {
        if (index < 0) return -1;
        auto p = head;
        for (int i = 0; i < index && p; i ++ ) p = p->next;
        if (!p) return -1;
        return p->val;
    }

    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    void addAtHead(int val) {
        auto cur = new Node(val);
        cur->next = head;
        head = cur;
    }

    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        if (!head) head = new Node(val);
        else {
            auto p = head;
            while (p->next) p = p->next;
            p->next = new Node(val);
        }
    }

    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    void addAtIndex(int index, int val) {
        if (index <= 0) addAtHead(val);
        else {
            int len = 0;
            for (auto p = head; p; p = p->next) len ++ ;
            if (index == len) addAtTail(val);
            else if (index < len) {
                auto p = head;
                for (int i = 0; i < index - 1; i ++ ) p = p->next;
                auto cur = new Node(val);
                cur->next = p->next;
                p->next = cur;
            }
        }
    }

    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        int len = 0;
        for (auto p = head; p; p = p->next) len ++ ;
        if (index >= 0 && index < len) {
            if (!index) head = head->next;
            else {
                auto p = head;
                for (int i = 0; i < index - 1; i ++ ) p = p->next;
                p->next = p->next->next;
            }
        }
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */

```
##### LeetCode 709. 转换成小写字母
```cpp
class Solution {
public:
    string toLowerCase(string str) {
        string res;
        for (auto c: str) res += tolower(c);
        return res;
    }
};
```
##### LeetCode 710. 黑名单中的随机数
```cpp
class Solution {
public:
    int n, len;
    unordered_map<int, int> hash;

    Solution(int N, vector<int>& blacklist) {
        n = N, len = blacklist.size();
        unordered_set<int> S;
        for (int i = n - len; i < n; i ++ ) S.insert(i);
        for (auto x: blacklist) S.erase(x);
        auto it = S.begin();
        for (auto x: blacklist)
            if (x < n - len)
                hash[x] = *it ++ ;
    }

    int pick() {
        int k = rand() % (n - len);
        if (hash.count(k)) return hash[k];
        return k;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(N, blacklist);
 * int param_1 = obj->pick();
 */
```
##### LeetCode 712. 两个字符串的最小ASCII删除和
```cpp
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int n = s1.size(), m = s2.size(), INF = 1e8;
        s1 = ' ' + s1, s2 = ' ' + s2;
        vector<vector<int>> f(n + 1, vector<int>(m + 1, INF));
        f[0][0] = 0;
        for (int i = 1; i <= n; i ++ ) f[i][0] = f[i - 1][0] + s1[i];
        for (int i = 1; i <= m; i ++ ) f[0][i] = f[0][i - 1] + s2[i];
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                f[i][j] = min(f[i - 1][j] + s1[i], f[i][j - 1] + s2[j]);
                f[i][j] = min(f[i][j], f[i - 1][j - 1] + s1[i] + s2[j]);
                if (s1[i] == s2[j])
                    f[i][j] = min(f[i][j], f[i - 1][j - 1]);
            }
        return f[n][m];
    }
};

```
##### LeetCode 713. 乘积小于K的子数组
```cpp
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int res = 0, p = 1;
        for (int i = 0, j = 0; i < nums.size(); i ++ ) {
            p *= nums[i];
            while (j <= i && p >= k) p /= nums[j ++ ];
            res += i - j + 1;
        }
        return res;
    }
};
```

##### LeetCode 714. 买卖股票的最佳时机含手续费
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size(), INF = 1e8;
        vector<vector<int>> f(n + 1, vector<int>(2, -INF));
        f[0][0] = 0;
        int res = 0;
        for (int i = 1; i <= n; i ++ ) {
            f[i][0] = max(f[i - 1][0], f[i - 1][1] + prices[i - 1]);
            f[i][1] = max(f[i - 1][1], f[i - 1][0] - prices[i - 1] - fee);
            res = max(res, f[i][0]);
        }
        return res;
    }
};
```
##### LeetCode 715. Range 模块
```cpp
typedef pair<int, int> PII;
const int INF = 2e9;

#define x first
#define y second

class RangeModule {
public:
    set<PII> S;

    RangeModule() {
        S.insert({-INF, -INF});
        S.insert({INF, INF});
    }

    void addRange(int left, int right) {
        auto i = S.lower_bound({left, -INF});
        i -- ;
        if (i->y < left) i ++ ;
        if (i->x > right) {
            S.insert({left, right});
        } else {
            auto j = i;
            while (j->x <= right) j ++ ;
            j -- ;
            PII t(min(i->x, left), max(j->y, right));
            while (i != j) {
                auto k = i;
                k ++ ;
                S.erase(i);
                i = k;
            }
            S.erase(i);
            S.insert(t);
        }
    }

    bool queryRange(int left, int right) {
        auto i = S.upper_bound({left, INF});
        i -- ;
        return i->y >= right;
    }

    vector<PII> get(PII a, PII b) {
        vector<PII> res;
        if (a.x < b.x) {
            if (a.y > b.y) {
                res.push_back({a.x, b.x});
                res.push_back({b.y, a.y});
            } else {
                res.push_back({a.x, b.x});
            }
        } else {
            if (a.y > b.y) res.push_back({b.y, a.y});
        }
        return res;
    }

    void removeRange(int left, int right) {
        auto i = S.lower_bound({left, -INF});
        i -- ;
        if (i->y < left) i ++ ;
        if (i->x <= right) {
            auto j = i;
            while (j->x <= right) j ++ ;
            j -- ;

            auto a = get(*i, {left, right});
            auto b = get(*j, {left, right});
            while (i != j) {
                auto k = i;
                k ++ ;
                S.erase(i);
                i = k;
            }
            S.erase(i);
            for (auto t: a) S.insert(t);
            for (auto t: b) S.insert(t);
        }
    }
};

/**
 * Your RangeModule object will be instantiated and called as such:
 * RangeModule* obj = new RangeModule();
 * obj->addRange(left,right);
 * bool param_2 = obj->queryRange(left,right);
 * obj->removeRange(left,right);
 */

```
##### LeetCode 717. 1比特与2比特字符
```cpp
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        for (int i = 0; i < bits.size(); i ++ ) {
            if (i == bits.size() - 1 && !bits[i]) return true;
            if (bits[i]) i ++ ;
        }
        return false;
    }
};
```
##### LeetCode 718. 最长重复子数组
```cpp
typedef unsigned long long ULL;
const int P = 131;

class Solution {
public:
    int n, m;
    vector<ULL> ha, hb, p;

    ULL get(vector<ULL>& h, int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }

    bool check(int mid) {
        unordered_set<ULL> hash;
        for (int i = mid; i <= n; i ++ ) hash.insert(get(ha, i - mid + 1, i));
        for (int i = mid; i <= m; i ++ )
            if (hash.count(get(hb, i - mid + 1, i)))
                return true;
        return false;
    }

    int findLength(vector<int>& A, vector<int>& B) {
        n = A.size(), m = B.size();
        ha.resize(n + 1), hb.resize(m + 1), p.resize(n + 1);
        for (int i = 1; i <= n; i ++ ) ha[i] = ha[i - 1] * P + A[i - 1];
        for (int i = 1; i <= m; i ++ ) hb[i] = hb[i - 1] * P + B[i - 1];
        p[0] = 1;
        for (int i = 1; i <= n; i ++ ) p[i] = p[i - 1] * P;

        int l = 0, r = n;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (check(mid)) l = mid;
            else r = mid - 1;
        }
        return r;
    }
};

```
##### LeetCode 719. 找出第 k 小的距离对
```cpp
class Solution {
public:
    int get(vector<int>& nums, int mid) {
        int res = 0;
        for (int i = 0, j = 0; i < nums.size(); i ++ ) {
            while (nums[i] - nums[j] > mid) j ++ ;
            res += i - j;
        }
        return res;
    }

    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int l = 0, r = 1e6;
        while (l < r) {
            int mid = l + r >> 1;
            if (get(nums, mid) >= k) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
##### LeetCode 720. 词典中最长的单词
```cpp
const int N = 30010;

int son[N][26], idx;
int id[N];

class Solution {
public:
    void insert(string& str, int k) {
        int p = 0;
        for (auto c: str) {
            int u = c - 'a';
            if (!son[p][u]) son[p][u] = ++ idx;
            p = son[p][u];
        }
        id[p] = k;
    }

    vector<int> dfs(int p, int len) {
        vector<int> res{len, id[p]};
        for (int i = 0; i < 26; i ++ ) {
            int j = son[p][i];
            if (j && id[j] != -1) {
                auto t = dfs(j, len + 1);
                if (res[0] < t[0]) res = t;
            }
        }
        return res;
    }

    string longestWord(vector<string>& words) {
        memset(id, -1, sizeof id);
        memset(son, 0, sizeof son);
        idx = 0;
        for (int i = 0; i < words.size(); i ++ ) insert(words[i], i);
        auto t = dfs(0, 0);
        if (t[1] != -1) return words[t[1]];
        return "";
    }
};

```
##### LeetCode 721. 账户合并
```cpp
class Solution {
public:
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        int n = accounts.size();
        for (int i = 0; i < n; i ++ ) p.push_back(i);
        unordered_map<string, vector<int>> hash;
        for (int i = 0; i < n; i ++ ) {
            for (int j = 1; j < accounts[i].size(); j ++ )
                hash[accounts[i][j]].push_back(i);
        }
        for (auto& [k, v]: hash)
            for (int i = 1; i < v.size(); i ++ )
                p[find(v[i])] = find(v[0]);
        vector<set<string>> res(n);
        for (int i = 0; i < n; i ++ )
            for (int j = 1; j < accounts[i].size(); j ++ )
                res[find(i)].insert(accounts[i][j]);
        vector<vector<string>> ans;
        for (int i = 0; i < n; i ++ )
            if (res[i].size()) {
                vector<string> t;
                t.push_back(accounts[i][0]);
                for (auto& e: res[i]) t.push_back(e);
                ans.push_back(t);
            }
        return ans;
    }
};

```
##### LeetCode 722. 删除注释
```cpp
class Solution {
public:
    vector<string> removeComments(vector<string>& source) {
        string str;
        for (auto& s: source) str += s + '\n';
        vector<string> res;
        string line;

        for (int i = 0; i < str.size(); ) {
            if (i + 1 < str.size() && str[i] == '/' && str[i + 1] == '/') {
                while (str[i] != '\n') i ++ ;
            } else if (i + 1 < str.size() && str[i] == '/' && str[i + 1] == '*') {
                i += 2;
                while (str[i] != '*' || str[i + 1] != '/') i ++ ;
                i += 2;
            } else if (str[i] == '\n') {
                if (line.size()) {
                    res.push_back(line);
                    line.clear();
                }
                i ++ ;
            } else {
                line += str[i];
                i ++ ;
            }
        }
        return res;
    }
};

```
##### LeetCode 724. 寻找数组的中心索引
```cpp
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        for (int i = 0, s = 0; i < nums.size(); i ++ ) {
            if (s == sum - s - nums[i]) return i;
            s += nums[i];
        }
        return -1;
    }
};
```
##### LeetCode 725. 分隔链表
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<ListNode*> splitListToParts(ListNode* root, int k) {
        int n = 0;
        for (auto p = root; p; p = p->next) n ++ ;

        vector<ListNode*> res;
        auto p = root;
        for (int i = 0; i < k; i ++ ) {
            int len = n / k;
            if (i + 1 <= n % k) len ++ ;
            res.push_back(p);
            for (int j = 0; j < len - 1; j ++ ) p = p->next;
            if (p) {
                auto q = p->next;
                p->next = NULL;
                p = q;
            }
        }
        return res;
    }
};
```
##### LeetCode 726. 原子的数量
```cpp
typedef map<string, int> MPSI;

class Solution {
public:
    MPSI dfs(string& str, int& u) {
        MPSI res;
        while (u < str.size()) {
            if (str[u] == '(') {
                u ++ ;
                auto t = dfs(str, u);
                u ++ ;
                int cnt = 1, k = u;
                while (k < str.size() && isdigit(str[k])) k ++ ;
                if (k > u) {
                    cnt = stoi(str.substr(u, k - u));
                    u = k;
                }
                for (auto& [x, y]: t) res[x] += y * cnt;
            } else if (str[u] == ')') break;
            else {
                int k = u + 1;
                while (k < str.size() && str[k] >= 'a' && str[k] <= 'z') k ++ ;
                auto key = str.substr(u, k - u);
                u = k;
                int cnt = 1;
                while (k < str.size() && isdigit(str[k])) k ++ ;
                if (k > u) {
                    cnt = stoi(str.substr(u, k - u));
                    u = k;
                }
                res[key] += cnt;
            }
        }
        return res;
    }

    string countOfAtoms(string formula) {
        int k = 0;
        auto t = dfs(formula, k);
        string res;
        for (auto& [x, y]: t) {
            res += x;
            if (y > 1) res += to_string(y);
        }
        return res;
    }
};

```







### 究极班- Week 30（第 728 ~ 733、735 ~ 736、738 ~ 741、743 ~ 749、752 题）

##### LeetCode 728. 自除数
```cpp
class Solution {
public:
    bool check(int x) {
        for (auto c: to_string(x))
            if (c == '0' || x % (c - '0'))
                return false;
        return true;
    }

    vector<int> selfDividingNumbers(int left, int right) {
        vector<int> res;
        for (int i = left; i <= right; i ++ )
            if (check(i))
                res.push_back(i);
        return res;
    }
};
```
##### LeetCode 729. 我的日程安排表 I
```cpp
typedef pair<int, int> PII;
const int INF = 2e9;

class MyCalendar {
public:
    set<PII> S;

    MyCalendar() {
        S.insert({-INF, -INF});
        S.insert({INF, INF});
    }

    bool check(PII a, PII b) {
        if (a.second <= b.first || b.second <= a.first) return false;
        return true;
    }

    bool book(int start, int end) {
        auto i = S.lower_bound({start, -INF});
        auto j = i;
        j -- ;
        PII t(start, end);
        if (check(*i, t) || check(*j, t)) return false;
        S.insert(t);
        return true;
    }
};

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar* obj = new MyCalendar();
 * bool param_1 = obj->book(start,end);
 */
```
##### LeetCode 730. 统计不同回文子序列
```cpp
class Solution {
public:
    int countPalindromicSubsequences(string s) {
        int n = s.size(), MOD = 1e9 + 7;
        vector<vector<int>> f(n + 2, vector<int>(n + 2, 1));
        for (int i = 1; i <= n; i ++ ) f[i][i] ++ ;
        for (int len = 2; len <= n; len ++ ) {
            deque<int> q[4];
            for (int i = 1; i <= n; i ++ ) {
                q[s[i - 1] - 'a'].push_back(i);
                int j = i - len + 1;
                if (j >= 1) {
                    for (int k = 0; k < 4; k ++ ) {
                        while (q[k].size() && q[k].front() < j) q[k].pop_front();
                        if (q[k].size()) {
                            f[j][i] ++ ;
                            int l = q[k].front(), r = q[k].back();
                            if (l < r)
                                f[j][i] = (f[j][i] + f[l + 1][r - 1]) % MOD;
                        }
                    }
                }
            }
        }
        return (f[1][n] + MOD - 1) % MOD;
    }
};
```
##### LeetCode 731. 我的日程安排表 II
```cpp
class MyCalendarTwo {
public:
    map<int, int> S;

    MyCalendarTwo() {

    }

    bool book(int start, int end) {
        S[start] ++ , S[end] -- ;
        int sum = 0;
        for (auto [k, v]: S) {
            sum += v;
            if (sum >= 3) {
                S[start] --, S[end] ++ ;
                return false;
            }
        }
        return true;
    }
};

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo* obj = new MyCalendarTwo();
 * bool param_1 = obj->book(start,end);
 */

```
##### LeetCode 732. 我的日程安排表 III
```cpp
class MyCalendarThree {
public:
    map<int, int> S;

    MyCalendarThree() {

    }

    int book(int start, int end) {
        S[start] ++ , S[end] -- ;
        int sum = 0, res = 0;
        for (auto [k, v]: S) {
            sum += v;
            res = max(res, sum);
        }
        return res;
    }
};

/**
 * Your MyCalendarThree object will be instantiated and called as such:
 * MyCalendarThree* obj = new MyCalendarThree();
 * int param_1 = obj->book(start,end);
 */

```
##### LeetCode 733. 图像渲染
```cpp
class Solution {
public:
    vector<vector<int>> g;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    void dfs(int x, int y, int color, int newColor) {
        g[x][y] = newColor;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < g.size() && b >= 0 && b < g[0].size() && g[a][b] == color)
                dfs(a, b, color, newColor);
        }
    }

    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        g = image;
        int color = g[sr][sc];
        if (color == newColor) return g;
        dfs(sr, sc, color, newColor);
        return g;
    }
};

```
##### LeetCode 735. 行星碰撞
```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        vector<int> res;
        for (auto x: asteroids) {
            if (x > 0) res.push_back(x);
            else {
                while (res.size() && res.back() > 0 && res.back() < -x) res.pop_back();
                if (res.size() && res.back() == -x) res.pop_back();
                else if (res.empty() || res.back() < 0) res.push_back(x);
            }
        }
        return res;
    }
};
```
##### LeetCode 736. Lisp 语法解析
```cpp
typedef unordered_map<string, int> MPSI;

class Solution {
public:
    int evaluate(string expression) {
        int k = 0;
        return dfs(expression, k, MPSI());
    }

    int get_value(string& str, int& k, MPSI vars) {
        int value;
        if (str[k] == '-' || isdigit(str[k])) {
            int i = k + 1;
            while (isdigit(str[i])) i ++ ;
            value = stoi(str.substr(k, i - k));
            k = i;
        } else if (str[k] != '(') {
            string name;
            while (str[k] != ' ' && str[k] != ')') name += str[k ++ ];
            value = vars[name];
        } else {
            value = dfs(str, k, vars);
        }
        return value;
    }

    int dfs(string& str, int& k, MPSI vars) {
        int value;
        k ++ ;  // 跳过 '('
        auto type = str.substr(k, 3);
        if (type == "let") {
            k += 4;  // 跳过 "let "
            while (str[k] != ')') {
                if (str[k] == '(' || str[k] == '-' || isdigit(str[k])) {
                    value = get_value(str, k, vars);
                    break;
                }
                string name;
                while (str[k] != ' ' && str[k] != ')') name += str[k ++ ];
                if (str[k] == ')') {
                    value = vars[name];
                    break;
                }
                k ++ ;  // 跳过 ' '
                vars[name] = get_value(str, k, vars);
                k ++ ;  // 跳过 ' '
            }
        } else if (type == "add") {
            k += 4;  // 跳过 "add "
            int a = get_value(str, k, vars);
            k ++ ;  // 跳过 ' '
            int b = get_value(str, k, vars);
            value = a + b;
        } else {
            k += 5;  // 跳过 "mult "
            int a = get_value(str, k, vars);
            k ++ ;  // 跳过 ' '
            int b = get_value(str, k, vars);
            value = a * b;
        }
        k ++ ;  // 跳过 ')'
        return value;
    }
};
```
##### LeetCode 738. 单调递增的数字
```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int N) {
        auto str = to_string(N);
        int k = 0;
        while (k + 1 < str.size() && str[k] <= str[k + 1]) k ++ ;
        if (k == str.size() - 1) return N;
        while (k && str[k - 1] == str[k]) k -- ;
        str[k] -- ;
        for (int i = k + 1; i < str.size(); i ++ ) str[i] = '9';
        return stoi(str);
    }
};
```
##### LeetCode 739. 每日温度
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        stack<int> stk;
        vector<int> res(T.size());
        for (int i = T.size() - 1; i >= 0; i -- ) {
            while (stk.size() && T[i] >= T[stk.top()]) stk.pop();
            if (stk.size()) res[i] = stk.top() - i;
            stk.push(i);
        }
        return res;
    }
};

```
##### LeetCode 740. 删除与获得点数
```cpp
const int N = 10010;
int cnt[N], f[N][2];

class Solution {
public:
    int deleteAndEarn(vector<int>& nums) {
        memset(cnt, 0, sizeof cnt);
        memset(f, 0, sizeof f);
        for (auto x: nums) cnt[x] ++ ;
        int res = 0;
        for (int i = 1; i < N; i ++ ) {
            f[i][0] = max(f[i - 1][0], f[i - 1][1]);
            f[i][1] = f[i - 1][0] + i * cnt[i];
            res = max(res, max(f[i][0], f[i][1]));
        }
        return res;
    }
};
```
##### LeetCode 741. 摘樱桃
```cpp
const int N = 55;
int f[N][N][N * 2];

class Solution {
public:
    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size();
        memset(f, -0x3f, sizeof f);
        if (grid[0][0] != -1) f[1][1][2] = grid[0][0];
        for (int k = 3; k <= n * 2; k ++ )
            for (int i = max(1, k - n); i <= min(n, k - 1); i ++ )
                for (int j = max(1, k - n); j <= min(n, k - 1); j ++ ) {
                    if (grid[i - 1][k - i - 1] == -1 || grid[j - 1][k - j - 1] == -1) continue;
                    int t = grid[i - 1][k - i - 1];
                    if (i != j) t += grid[j - 1][k - j - 1];
                    for (int a = i - 1; a <= i; a ++ )
                        for (int b = j - 1; b <= j; b ++ )
                            f[i][j][k] = max(f[i][j][k], f[a][b][k - 1] + t);
                }
        return max(0, f[n][n][n * 2]);
    }
};

```
##### LeetCode 743. 网络延迟时间
```cpp
const int N = 110, M = 6010, INF = 0x3f3f3f3f;
int h[N], e[M], w[M], ne[M], idx;
int dist[N];
bool st[N];

class Solution {
public:
    void add(int a, int b, int c) {
        e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
    }

    void spfa(int start) {
        queue<int> q;
        q.push(start);
        memset(dist, 0x3f, sizeof dist);
        dist[start] = 0;
        while (q.size()) {
            int t = q.front();
            q.pop();
            st[t] = false;
            for (int i = h[t]; ~i; i = ne[i]) {
                int j = e[i];
                if (dist[j] > dist[t] + w[i]) {
                    dist[j] = dist[t] + w[i];
                    if (!st[j]) {
                        q.push(j);
                        st[j] = true;
                    }
                }
            }
        }
    }

    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        memset(h, -1, sizeof h);
        idx = 0;
        for (auto& e: times) {
            int a = e[0], b = e[1], c = e[2];
            add(a, b, c);
        }
        spfa(k);
        int res = 1;
        for (int i = 1; i <= n; i ++ ) res = max(res, dist[i]);
        if (res == INF) res = -1;
        return res;
    }
};

```
##### LeetCode 744. 寻找比目标字母大的最小字母
```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int l = 0, r = letters.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (letters[mid] > target) r = mid;
            else l = mid + 1;
        }
        if (letters[r] > target) return letters[r];
        return letters[0];
    }
};
```
##### LeetCode 745. 前缀和后缀搜索
```cpp
const int N = 2000000;
int son[N][27], w[N], idx;

class WordFilter {
public:
    void insert(string& s, int id) {
        int p = 0;
        for (auto c: s) {
            int t = c == '#' ? 26 : c - 'a';
            if (!son[p][t]) son[p][t] = ++ idx;
            p = son[p][t];
            w[p] = id;
        }
    }

    int query(string s) {
        int p = 0;
        for (auto c: s) {
            int t = c == '#' ? 26 : c - 'a';
            if (!son[p][t]) return -1;
            p = son[p][t];
        }
        return w[p];
    }

    WordFilter(vector<string>& words) {
        memset(son, 0, sizeof son);
        idx = 0;
        for (int i = 0; i < words.size(); i ++ ) {
            string s = '#' + words[i];
            insert(s, i);
            for (int j = words[i].size() - 1; j >= 0; j -- ) {
                s = words[i][j] + s;
                insert(s, i);
            }
        }
    }

    int f(string prefix, string suffix) {
        return query(suffix + '#' + prefix);
    }
};

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter* obj = new WordFilter(words);
 * int param_1 = obj->f(prefix,suffix);
 */
```
##### LeetCode 746. 使用最小花费爬楼梯
```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        vector<int> f(n);
        f[0] = cost[0], f[1] = cost[1];
        for (int i = 2; i < n; i ++ )
            f[i] = min(f[i - 1], f[i - 2]) + cost[i];
        return min(f[n - 2], f[n - 1]);
    }
};
```
##### LeetCode 747. 至少是其他数字两倍的最大数
```cpp
class Solution {
public:
    int dominantIndex(vector<int>& nums) {
        int k = 0;
        for (int i = 1; i < nums.size(); i ++ )
            if (nums[i] > nums[k])
                k = i;
        for (int i = 0; i < nums.size(); i ++ )
            if (i != k && nums[k] < nums[i] * 2)
                return -1;
        return k;
    }
};
```
##### LeetCode 748. 最短补全词
```cpp
class Solution {
public:
    bool check(unordered_map<char, int>& a, unordered_map<char, int>& b) {
        for (auto [k, v]: b)
            if (a[k] < v)
                return false;
        return true;
    }

    string shortestCompletingWord(string licensePlate, vector<string>& words) {
        unordered_map<char, int> T;
        for (auto c: licensePlate)
            if (c != ' ' && !isdigit(c))
                T[tolower(c)] ++ ;
        string res;
        for (auto& w: words) {
            unordered_map<char, int> cnt;
            for (auto c: w) cnt[c] ++ ;
            if (check(cnt, T)) {
                if (res.empty() || res.size() > w.size())
                    res = w;
            }
        }
        return res;
    }
};
```


##### LeetCode 749. 隔离病毒
```cpp
typedef pair<int, int> PII;

#define x first
#define y second

class Solution {
public:
    int n, m;
    vector<vector<int>> g;
    vector<vector<bool>> st;
    vector<PII> path;
    set<PII> S;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int dfs(int x, int y) {
        st[x][y] = true;
        path.push_back({x, y});
        int res = 0;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m) {
                if (!g[a][b]) S.insert({a, b}), res ++ ;
                else if (g[a][b] == 1 && !st[a][b])
                    res += dfs(a, b);
            }
        }
        return res;
    }

    int find() {
        st = vector<vector<bool>>(n, vector<bool>(m));
        int cnt = 0, res = 0;
        vector<PII> ps;
        vector<set<PII>> ss;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) 
                if (g[i][j] == 1 && !st[i][j]){
                    path.clear(), S.clear();
                    int t = dfs(i, j);
                    if (S.size() > cnt) {
                        cnt = S.size();
                        res = t;
                        ps = path;
                    }
                    ss.push_back(S);
                }
        for (auto& p: ps) g[p.x][p.y] = -1;
        for (auto& s: ss)
            if (s.size() != cnt)
                for (auto& p: s)
                    g[p.x][p.y] = 1;
        return res;
    }

    int containVirus(vector<vector<int>>& grid) {
        g = grid;
        n = g.size(), m = g[0].size();
        int res = 0;
        while (true) {
            auto cnt = find();
            if (!cnt) break;
            res += cnt;
        }
        return res;
    }
};

```
##### LeetCode 752. 打开转盘锁
```cpp
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        string start = "0000";
        if (start == target) return 0;
        unordered_set<string> S;
        for (auto& s: deadends) S.insert(s);
        if (S.count(start)) return -1;
        queue<string> q;
        q.push(start);
        unordered_map<string, int> dist;
        dist[start] = 0;
        while (q.size()) {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < 4; i ++ )
                for (int j = -1; j <= 1; j += 2) {
                    auto state = t;
                    state[i] = (state[i] - '0' + j + 10) % 10 + '0';
                    if (!dist.count(state) && !S.count(state)) {
                        dist[state] = dist[t] + 1;
                        if (state == target) return dist[state];
                        q.push(state);
                    }
                }
        }
        return -1;
    }
};
```

### 究极班—— Week 31（第 753 ~ 754、756 ~ 757、761 ~ 771、773、775、777 ~ 779 题）

#####  LeetCode  753. 破解保险箱
```C++
class Solution {
public:
    unordered_set<string> S;
    string ans;
    int k;

    void dfs(string u) {
        for (int i = 0; i < k; i ++ ) {
            auto v = u + to_string(i);
            if (!S.count(v)) {
                S.insert(v);
                dfs(v.substr(1));
                ans += to_string(i);
            }
        }
    }

    string crackSafe(int n, int _k) {
        k = _k;
        string start(n - 1, '0');
        dfs(start);
        return ans + start;
    }
};

```
#####  LeetCode  754. 到达终点数字
```C++
class Solution {
public:
    int reachNumber(int target) {
        target = abs(target);
        int n = 0, sum = 0;
        while (sum < target || (sum - target) % 2) {
            n ++ ;
            sum += n;
        }
        return n;
    }
};
```
#####  LeetCode  756. 金字塔转换矩阵
```C++
class Solution {
public:
    vector<char> g[7][7];

    bool pyramidTransition(string bottom, vector<string>& allowed) {
        for (auto& s: allowed) g[s[0] - 'A'][s[1] - 'A'].push_back(s[2]);
        return dfs(bottom, "", 0);
    }

    bool dfs(string bottom, string up, int u) {
        if (bottom.size() == 1) return true;
        if (u == bottom.size() - 1) return dfs(up, "", 0);

        for (auto c: g[bottom[u] - 'A'][bottom[u + 1] - 'A'])
            if (dfs(bottom, up + c, u + 1))
                return true;

        return false;
    }
};

```

#####  LeetCode  757. 设置交集大小至少为2
```C++
class Solution {
public:
    int intersectionSizeTwo(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](vector<int>& a, vector<int>& b) {
            if (a[1] != b[1]) return a[1] < b[1];
            return a[0] > b[0];
        });
        vector<int> q(1, -1);
        int cnt = 0;
        for (auto& r: intervals) {
            if (r[0] > q[cnt]) {
                q.push_back(r[1] - 1);
                q.push_back(r[1]);
                cnt += 2;
            } else if (r[0] > q[cnt - 1]) {
                q.push_back(r[1]);
                cnt ++ ;
            }
        }
        return cnt;
    }
};

```
#####  LeetCode  761. 特殊的二进制序列
```C++
class Solution {
public:
    string makeLargestSpecial(string S) {
        if (S.size() <= 2) return S;
        vector<string> q;
        string s;
        int cnt = 0;
        for (auto c: S) {
            s += c;
            if (c == '1') cnt ++ ;
            else {
                cnt -- ;
                if (cnt == 0) {
                    q.push_back('1' + makeLargestSpecial(s.substr(1, s.size() - 2)) + '0');
                    s.clear();
                }
            }
        }
        sort(q.begin(), q.end(), [](string& a, string& b) {
            return a + b > b + a;
        });
        string res;
        for (auto s: q) res += s;
        return res;
    }
};

```
#####  LeetCode  762. 二进制表示中质数个计算置位
```C++
class Solution {
public:
    int countPrimeSetBits(int L, int R) {
        unordered_set<int> primes{2, 3, 5, 7, 11, 13, 17, 19};
        int res = 0;
        for (int i = L; i <= R; i ++ ) {
            int s = 0;
            for (int j = i; j; j >>= 1) s += j & 1;
            res += primes.count(s);
        }
        return res;
    }
};

```
#####  LeetCode  763. 划分字母区间
```C++
class Solution {
public:
    vector<int> partitionLabels(string S) {
        unordered_map<int, int> last;
        for (int i = 0; i < S.size(); i ++ ) last[S[i]] = i;
        vector<int> res;
        int start = 0, end = 0;
        for (int i = 0; i < S.size(); i ++ ) {
            end = max(end, last[S[i]]);
            if (i == end) {
                res.push_back(end - start + 1);
                start = end = i + 1;
            }
        }
        return res;
    }
};

```
#####  LeetCode  764. 最大加号标志
```C++
const int N = 510;

int f[N][N];
bool g[N][N];

class Solution {
public:
    int orderOfLargestPlusSign(int N, vector<vector<int>>& mines) {
        memset(g, 1, sizeof g);
        for (auto& e: mines) g[e[0]][e[1]] = false;
        for (int i = 0; i < N; i ++ )
            for (int j = 0, s = 0; j < N; j ++ ) {
                if (g[i][j]) s ++ ;
                else s = 0;
                f[i][j] = s;
            }
        for (int i = 0; i < N; i ++ )
            for (int j = N - 1, s = 0; j >= 0; j -- ) {
                if (g[i][j]) s ++ ;
                else s = 0;
                f[i][j] = min(f[i][j], s);
            }
        for (int i = 0; i < N; i ++ )
            for (int j = 0, s = 0; j < N; j ++ ) {
                if (g[j][i]) s ++ ;
                else s = 0;
                f[j][i] = min(f[j][i], s);
            }
        for (int i = 0; i < N; i ++ )
            for (int j = N - 1, s = 0; j >= 0; j -- ) {
                if (g[j][i]) s ++ ;
                else s = 0;
                f[j][i] = min(f[j][i], s);
            }
        int res = 0;
        for (int i = 0; i < N; i ++ )
            for (int j = 0; j < N; j ++ )
                res = max(res, f[i][j]);
        return res;
    }
};

```
#####  LeetCode  765. 情侣牵手
```C++
class Solution {
public:
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int minSwapsCouples(vector<int>& row) {
        int n = row.size() / 2;
        for (int i = 0; i < n; i ++ ) p.push_back(i);

        int cnt = n;
        for (int i = 0; i < n * 2; i += 2) {
            int a = row[i] / 2, b = row[i + 1] / 2;
            if (find(a) != find(b)) {
                p[find(a)] = find(b);
                cnt -- ;
            }
        }
        return n - cnt;
    }
};

```
#####  LeetCode  766. 托普利茨矩阵
```C++
class Solution {
public:
    bool isToeplitzMatrix(vector<vector<int>>& matrix) {
        for (int i = 1; i < matrix.size(); i ++ )
            for (int j = 1; j < matrix[i].size(); j ++ )
                if (matrix[i][j] != matrix[i - 1][j - 1])
                    return false;
        return true;
    }
};

```

#####  LeetCode 767. 重构字符串
```cpp
class Solution {
public:
    string reorganizeString(string S) {
        unordered_map<char, int> cnt;
        int maxc = 0;
        for (auto c: S) {
            cnt[c] ++ ;
            maxc = max(maxc, cnt[c]);
        }
        int n = S.size();
        if (maxc > (n + 1) / 2) return "";
        string res(n, ' ');
        int i = 1, j = 0;
        for (char c = 'a'; c <= 'z'; c ++ ) {
            if (cnt[c] <= n / 2) {
                while (cnt[c] && i < n) {
                    res[i] = c;
                    cnt[c] -- ;
                    i += 2;
                }
            }
            while (cnt[c] && j < n) {
                res[j] = c;
                cnt[c] -- ;
                j += 2;
            }
        }
        return res;
    }
};
```
#####  LeetCode 768. 最多能完成排序的块 II
```cpp
class Solution {
public:
    int maxChunksToSorted(vector<int>& a) {
        auto b = a;
        sort(b.begin(), b.end());
        unordered_map<int, int> cnt;
        int res = 0;
        for (int i = 0, s = 0; i < a.size(); i ++ ) {
            if (cnt[a[i]] == 1) s -- ;
            else if (cnt[a[i]] == 0) s ++ ;
            cnt[a[i]] -- ;
            if (cnt[b[i]] == -1) s -- ;
            else if (cnt[b[i]] == 0) s ++ ;
            cnt[b[i]] ++ ;
            if (!s) res ++ ;
        }
        return res;
    }
};

```
#####  LeetCode 769. 最多能完成排序的块
```cpp
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        int res = 0;
        for (int i = 0, x = 0; i < arr.size(); i ++ ) {
            x = max(x, arr[i]);
            if (x == i) res ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 770. 基本计算器 IV
```cpp
class Solution {
public:
    struct Item {
        int c;  // 系数
        multiset<string> vars;  // 所有自变量
        bool operator< (const Item& t) const {
            if (vars.size() != t.vars.size()) return vars.size() > t.vars.size();
            return vars < t.vars;
        }
        bool operator== (const Item& t) const {
            return vars == t.vars;
        }
        string convert_to_string() {
            string res = to_string(c);
            for (auto& var: vars) res += '*' + var;
            return res;
        }
    };  // 项
    unordered_map<string, int> value;
    stack<vector<Item>> num;
    stack<char> op;

    vector<Item> add(vector<Item> a, vector<Item> b, int sign) {
        vector<Item> res;
        int i = 0, j = 0;
        while (i < a.size() && j < b.size()) {  // 二路归并
            if (a[i] == b[j]) {
                Item t{a[i].c + b[j].c * sign, a[i].vars};
                if (t.c) res.push_back(t);
                i ++, j ++ ;
            } else if (a[i] < b[j]) {
                res.push_back(a[i ++ ]);
            } else {
                res.push_back({b[j].c * sign, b[j].vars}), j ++ ;
            }
        }
        while (i < a.size()) res.push_back(a[i ++ ]);
        while (j < b.size()) res.push_back({b[j].c * sign, b[j].vars}), j ++ ;
        return res;
    }

    vector<Item> mul(vector<Item> a, vector<Item> b) {
        vector<Item> res;
        for (auto& x: a) {
            vector<Item> items;
            for (auto& y: b) {
                Item t{x.c * y.c, x.vars};
                for (auto& v: y.vars) t.vars.insert(v);
                items.push_back(t);
            }
            res = add(res, items, 1);
        }
        return res;
    }

    void eval() {
        auto b = num.top(); num.pop();
        auto a = num.top(); num.pop();
        auto c = op.top(); op.pop();
        vector<Item> x;
        if (c == '+') x = add(a, b, 1);
        else if (c == '-') x = add(a, b, -1);
        else x = mul(a, b);
        num.push(x);
    }

    vector<Item> calc(string& str) {
        unordered_map<char, int> pr{{'+', 1}, {'-', 1}, {'*', 2}};
        for (int i = 0; i < str.size(); i ++ ) {
            if (str[i] == ' ') continue;
            if (str[i] >= 'a' && str[i] <= 'z' || isdigit(str[i])) {
                vector<Item> items;
                if (str[i] >= 'a' && str[i] <= 'z') {
                    string var;
                    int j = i;
                    while (j < str.size() && str[j] >= 'a' && str[j] <= 'z') var += str[j ++ ];
                    i = j - 1;
                    if (value.count(var)) {
                        if (value[var]) items.push_back({value[var], {}});
                    } else {
                        items.push_back({1, {var}});
                    }
                } else {
                    int x = 0, j = i;
                    while (j < str.size() && isdigit(str[j])) x = x * 10 + str[j ++ ] - '0';
                    i = j - 1;
                    if (x) items.push_back({x, {}});
                }
                num.push(items);
            } else if (str[i] == '(') {
                op.push(str[i]);
            } else if (str[i] == ')') {
                while (op.top() != '(') eval();
                op.pop();
            } else {
                while (op.size() && op.top() != '(' && pr[op.top()] >= pr[str[i]]) eval();
                op.push(str[i]);
            }
        }
        while (op.size()) eval();
        return num.top();
    }

    vector<string> basicCalculatorIV(string expression, vector<string>& evalvars, vector<int>& evalints) {
        for (int i = 0; i < evalvars.size(); i ++ ) value[evalvars[i]] = evalints[i];
        auto t = calc(expression);
        vector<string> res;
        for (auto& item: t) res.push_back(item.convert_to_string());
        return res;
    }
};
```
#####  LeetCode 771. 宝石与石头
```cpp
class Solution {
public:
    int numJewelsInStones(string jewels, string stones) {
        unordered_set<char> hash(jewels.begin(), jewels.end());
        int res = 0;
        for (auto c: stones) res += hash.count(c);
        return res;
    }
};
```
#####  LeetCode 773. 滑动谜题
```cpp
class Solution {
public:
    int slidingPuzzle(vector<vector<int>>& board) {
        typedef vector<vector<int>> VII;
        VII target = {{1, 2, 3}, {4, 5, 0}};
        if (board == target) return 0;
        queue<VII> q;
        q.push(board);
        map<VII, int> dist;
        dist[board] = 0;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

        while (q.size()) {
            auto t = q.front();
            q.pop();

            int x, y;
            for (int i = 0; i < 2; i ++ )
                for (int j = 0; j < 3; j ++ )
                    if (t[i][j] == 0)
                        x = i, y = j;
            for (int i = 0; i < 4; i ++ ) {
                int a = x + dx[i], b = y + dy[i];
                if (a >= 0 && a < 2 && b >= 0 && b < 3) {
                    auto r = t;
                    swap(r[x][y], r[a][b]);
                    if (!dist.count(r)) {
                        dist[r] = dist[t] + 1;
                        if (r == target) return dist[r];
                        q.push(r);
                    }
                }
            }
        }
        return -1;
    }
};
```
#####  LeetCode 775. 全局倒置与局部倒置
```cpp
class Solution {
public:
    bool isIdealPermutation(vector<int>& A) {
        for (int i = 0; i < A.size(); i ++ )
            if (abs(A[i] - i) > 1)
                return false;
        return true;
    }
};
```
#####  LeetCode 777. 在LR字符串中交换相邻字符
```cpp
class Solution {
public:
    bool canTransform(string start, string end) {
        string a, b;
        for (auto c: start)
            if (c != 'X') a += c;
        for (auto c: end)
            if (c != 'X') b += c;
        if (a != b) return false;
        for (int i = 0, j = 0; i < start.size(); i ++, j ++ ) {
            while (i < start.size() && start[i] != 'L') i ++ ;
            while (j < end.size() && end[j] != 'L') j ++ ;
            if (i < j) return false;
        }
        for (int i = 0, j = 0; i < start.size(); i ++, j ++ ) {
            while (i < start.size() && start[i] != 'R') i ++ ;
            while (j < end.size() && end[j] != 'R') j ++ ;
            if (i > j) return false;
        }
        return true;
    }
};
```
#####  LeetCode 778. 水位上升的泳池中游泳
```cpp
class Solution {
public:
    int n;
    vector<vector<int>> g;
    vector<vector<bool>> st;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    bool dfs(int x, int y, int mid) {
        if (x == n - 1 && y == n - 1) return true;
        st[x][y] = true;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < n && g[a][b] <= mid && !st[a][b])
                if (dfs(a, b, mid)) return true;
        }
        return false;
    }

    bool check(int mid) {
        if (g[0][0] > mid) return false;
        st = vector<vector<bool>>(n, vector<bool>(n));
        return dfs(0, 0, mid);
    }

    int swimInWater(vector<vector<int>>& grid) {
        g = grid;
        n = g.size();
        int l = 0, r = n * n - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (check(mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
#####  LeetCode 779. 第K个语法符号
```cpp
class Solution {
public:
    int kthGrammar(int N, int K) {
        K -- ;
        int res = 0;
        while (K) res ^= K & 1, K >>= 1;
        return res;
    }
};
```



### 究极班- Week 32 (第 780 ~ 799 题)

##### LeetCode 780. 到达终点
```cpp
class Solution {
public:
    bool reachingPoints(int sx, int sy, int tx, int ty) {
        while (tx >= sx && ty >= sy) {
            if (tx == ty) break;
            if (tx > ty) {
                if (ty > sy) tx %= ty;
                else return (tx - sx) % ty == 0;
            } else {
                if (tx > sx) ty %= tx;
                else return (ty - sy) % tx == 0;
            }
        }

        return sx == tx && sy == ty;
    }
};
```
##### LeetCode 781. 森林中的兔子
```cpp
class Solution {
public:
    int numRabbits(vector<int>& answers) {
        unordered_map<int, int> cnt;
        for (auto x: answers) cnt[x] ++ ;
        int res = 0;
        for (auto [k, v]: cnt)
            res += (v + k) / (k + 1) * (k + 1);
        return res;
    }
};
```
##### LeetCode 782. 变为棋盘
```cpp

```
##### LeetCode 783. 二叉搜索树节点最小距离
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int ans, last;
    bool is_first;

    int minDiffInBST(TreeNode* root) {
        ans = INT_MAX, is_first = true;
        dfs(root);
        return ans;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        if (is_first) {
            last = root->val;
            is_first = false;
        } else {
            ans = min(ans, root->val - last);
            last = root-> val;
        }
        dfs(root->right);
    }
};
```
##### LeetCode 784. 字母大小写全排列
```cpp
class Solution {
public:
    vector<string> ans;

    vector<string> letterCasePermutation(string S) {
        dfs(S, 0);
        return ans;
    }

    void dfs(string& s, int u) {
        if (u == s.size()) ans.push_back(s);
        else {
            dfs(s, u + 1);
            if (!isdigit(s[u])) {
                s[u] ^= 32;
                dfs(s, u + 1);
                s[u] ^= 32;
            }
        }
    }
};

```
##### LeetCode 785. 判断二分图
```cpp
class Solution {
public:
    vector<int> color;
    vector<vector<int>> g;

    bool dfs(int u, int c) {
        color[u] = c;
        for (auto v: g[u]) {
            if (color[v] != -1) {
                if (color[v] == c) return false;
            } else if (!dfs(v, c ^ 1)) return false;
        }
        return true;
    }

    bool isBipartite(vector<vector<int>>& graph) {
        g = graph;
        color = vector<int>(g.size(), -1);

        for (int i = 0; i < g.size(); i ++ )
            if (color[i] == -1)
                if (!dfs(i, 0))
                    return false;
        return true;
    }
};

```
##### LeetCode 786. 第 K 个最小的素数分数
```cpp
class Solution {
public:
    const double eps = 1e-8;
    int A, B;

    int get(vector<int>& arr, double mid) {
        int res = 0;
        for (int i = 0, j = 0; i < arr.size(); i ++ ) {
            while ((double)arr[j + 1] / arr[i] <= mid) j ++ ;
            if ((double)arr[j] / arr[i] <= mid) res += j + 1;
            if (fabs((double)arr[j] / arr[i] - mid) < eps) {
                A = arr[j], B = arr[i];
            }
        }
        return res;
    }

    vector<int> kthSmallestPrimeFraction(vector<int>& arr, int k) {
        double l = 0, r = 1;
        while (r - l > eps) {
            double mid = (l + r) / 2;
            if (get(arr, mid) >= k) r = mid;
            else l = mid;
        }

        get(arr, r);
        return {A, B};
    }
};

```
##### LeetCode 787. K 站中转内最便宜的航班
```cpp
class Solution {
public:
    const int INF = 1e8;

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        vector<int> dist(n, INF);
        dist[src] = 0;
        K ++ ;
        while (K -- ) {
            auto cur = dist;
            for (auto& e: flights) {
                int a = e[0], b = e[1], c = e[2];
                cur[b] = min(cur[b], dist[a] + c);
            }
            dist = cur;
        }
        if (dist[dst] == INF) return -1;
        return dist[dst];
    }
};
```
##### LeetCode 788. 旋转数字
```cpp
class Solution {
public:
    bool check(int x) {
        set<int> s1{0, 1, 8}, s2{2, 5, 6, 9};
        bool is_diff = false;
        for (auto c: to_string(x)) {
            int t = c - '0';
            if (!s1.count(t) && !s2.count(t)) return false;
            if (s2.count(t)) is_diff = true;
        }
        return is_diff;
    }

    int rotatedDigits(int N) {
        int res = 0;
        for (int i = 1; i <= N; i ++ )
            if (check(i))
                res ++ ;
        return res;
    }
};
```
##### LeetCode 789. 逃脱阻碍者
```cpp
class Solution {
public:
    int get_dist(int x1, int y1, int x2, int y2) {
        return abs(x1 - x2) + abs(y1 - y2);
    }

    bool escapeGhosts(vector<vector<int>>& ghosts, vector<int>& target) {
        for (auto& g: ghosts)
            if (get_dist(g[0], g[1], target[0], target[1]) <= abs(target[0]) + abs(target[1]))
                return false;
        return true;
    }
};

```
##### LeetCode 790. 多米诺和托米诺平铺
```cpp
class Solution {
public:
    int numTilings(int n) {
        const int MOD = 1e9 + 7;
        int w[4][4] = {
            {1, 1, 1, 1},
            {0, 0, 1, 1},
            {0, 1, 0, 1},
            {1, 0, 0, 0}
        };
        vector<vector<int>> f(n + 1, vector<int>(4));
        f[0][0] = 1;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < 4; j ++ )
                for (int k = 0; k < 4; k ++ )
                    f[i + 1][k] = (f[i + 1][k] + f[i][j] * w[j][k]) % MOD;
        return f[n][0];
    }
};

```
##### LeetCode 791. 自定义字符串排序
```cpp
class Solution {
public:
    string customSortString(string S, string T) {
        unordered_map<char, int> pos;
        for (int i = 0; i < S.size(); i ++ ) pos[S[i]] = i;
        sort(T.begin(), T.end(), [&](char a, char b) {
            return pos[a] < pos[b];
        });
        return T;
    }
};
```
##### LeetCode 792. 匹配子序列的单词数
```cpp
typedef pair<int, int> PII;

#define x first
#define y second

class Solution {
public:
    int numMatchingSubseq(string s, vector<string>& words) {
        vector<PII> ps[26];
        for (int i = 0; i < words.size(); i ++ )
            ps[words[i][0] - 'a'].push_back({i, 0});

        int res = 0;
        for (auto c: s) {
            vector<PII> buf;
            for (auto& p: ps[c - 'a'])
                if (p.y + 1 == words[p.x].size()) res ++ ;
                else buf.push_back({p.x, p.y + 1});
            ps[c - 'a'].clear();
            for (auto& p: buf)
                ps[words[p.x][p.y] - 'a'].push_back(p);
        }
        return res;
    }
};
```
##### LeetCode 793. 阶乘函数后 K 个零
```cpp
typedef long long LL;

class Solution {
public:
    int preimageSizeFZF(int k) {
        return calc(k) - calc(k - 1);
    }

    LL f(LL x) {
        LL res = 0;
        while (x) res += x / 5, x /= 5;
        return res;
    }

    LL calc(int k) {
        LL l = -1, r = 1e18;
        while (l < r) {
            LL mid = l + r + 1 >> 1;
            if (f(mid) <= k) l = mid;
            else r = mid - 1;
        }
        return r;
    }
};
```
##### LeetCode 794. 有效的井字游戏
```cpp
class Solution {
public:
    vector<string> g;

    int get(char c) {
        int res = 0;
        for (int i = 0; i < 3; i ++ )
            for (int j = 0; j < 3; j ++ )
                if (g[i][j] == c)
                    res ++ ;
        return res;
    }

    bool check(char c) {
        for (int i = 0; i < 3; i ++ ) {
            if (g[i][0] == c && g[i][1] == c && g[i][2] == c) return true;
            if (g[0][i] == c && g[1][i] == c && g[2][i] == c) return true;
        }
        if (g[0][0] == c && g[1][1] == c && g[2][2] == c) return true;
        if (g[0][2] == c && g[1][1] == c && g[2][0] == c) return true;
        return false;
    }

    bool validTicTacToe(vector<string>& board) {
        g = board;
        bool cx = check('X'), co = check('O');
        if (cx && co) return false;
        int sx = get('X'), so = get('O');
        if (cx && sx != so + 1) return false;
        if (co && sx != so) return false;
        if (sx != so && sx != so + 1) return false;
        return true;
    }
};

```
##### LeetCode 795. 区间子数组个数
```cpp
class Solution {
public:
    int calc(vector<int>& A, int k) {
        int res = 0;
        for (int i = 0; i < A.size(); i ++ ) {
            if (A[i] > k) continue;
            int j = i + 1;
            while (j < A.size() && A[j] <= k) j ++ ;
            int k = j - i;
            res += k * (k + 1) / 2;
            i = j;
        }
        return res;
    }

    int numSubarrayBoundedMax(vector<int>& A, int L, int R) {
        return calc(A, R) - calc(A, L - 1);
    }
};
```
##### LeetCode 796. 旋转字符串
```cpp
typedef unsigned long long ULL;

const int N = 210, P = 131;
ULL h[N], p[N];

class Solution {
public:
    ULL get(int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }

    bool rotateString(string A, string B) {
        if (A == B) return true;
        string s = ' ' + A + B;
        int n = s.size() - 1;
        p[0] = 1;
        for (int i = 1; i <= n; i ++ ) {
            p[i] = p[i - 1] * P;
            h[i] = h[i - 1] * P + s[i];
        }

        for (int k = 1; k < A.size(); k ++ )
            if (get(1, k) == get(n - k + 1, n) && get(k + 1, A.size()) == get(A.size() + 1, n - k))
                return true;
        return false;
    }
};

```
##### LeetCode 797. 所有可能的路径
```cpp
class Solution {
public:
    int n;
    vector<vector<int>> g;
    vector<vector<int>> ans;
    vector<int> path;

    void dfs(int u) {
        path.push_back(u);
        if (u == n - 1) ans.push_back(path);
        for (auto v: g[u]) dfs(v);
        path.pop_back();
    }

    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        g = graph;
        n = g.size();
        dfs(0);
        return ans;
    }
};

```
##### LeetCode 798. 得分最高的最小轮调
```cpp
class Solution {
public:
    int bestRotation(vector<int>& A) {
        int n = A.size();
        vector<int> b(n + 1);
        for (int i = 0; i < A.size(); i ++ ) {
            int l = i - A[i] + 1, r = i;
            if (l >= 0) b[l] ++, b[r + 1] -- ;
            else {
                b[0] ++, b[r + 1] -- ;
                b[l + n] ++, b[n] -- ;
            }
        }
        int res = INT_MAX, k = 0;
        for (int i = 0, sum = 0; i < n; i ++ ) {
            sum += b[i];
            if (res > sum) {
                res = sum;
                k = i;
            }
        }
        return k;
    }
};

```
##### LeetCode 799. 香槟塔
```cpp
class Solution {
public:
    double champagneTower(int poured, int query_row, int query_glass) {
        vector<vector<double>> f(query_row + 1, vector<double>(query_row + 1));
        f[0][0] = poured;
        for (int i = 0; i < query_row; i ++ )
            for (int j = 0; j <= i; j ++ )
                if (f[i][j] > 1) {
                    double x = (f[i][j] - 1) / 2;
                    f[i + 1][j] += x, f[i + 1][j + 1] += x;
                }
        return min(1.0, f[query_row][query_glass]);
    }
};
```


### 究极班- Week 33（第 801 ~ 820 题）

##### LeetCode 801. 使序列递增的最小交换次数
```cpp
class Solution {
public:
    int minSwap(vector<int>& A, vector<int>& B) {
        int n = A.size(), INF = 1e8;
        vector<vector<int>> f(n, vector<int>(2, INF));
        f[0][0] = 0, f[0][1] = 1;
        for (int i = 1; i < n; i ++ ) {
            if (A[i] > A[i - 1] && B[i] > B[i - 1]) f[i][0] = min(f[i][0], f[i - 1][0]);
            if (A[i] > B[i - 1] && B[i] > A[i - 1]) f[i][0] = min(f[i][0], f[i - 1][1]);
            if (B[i] > A[i - 1] && A[i] > B[i - 1]) f[i][1] = min(f[i][1], f[i - 1][0] + 1);
            if (B[i] > B[i - 1] && A[i] > A[i - 1]) f[i][1] = min(f[i][1], f[i - 1][1] + 1);
        }
        return min(f[n - 1][0], f[n - 1][1]);
    }
};
```
##### LeetCode 802. 找到最终的安全状态
```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> d(n);
        vector<vector<int>> g(n);
        for (int i = 0; i < n; i ++ )
            for (auto b: graph[i]) {
                int a = i;
                g[b].push_back(a);
                d[a] ++ ;
            }
        queue<int> q;
        for (int i = 0; i < n; i ++ )
            if (!d[i])
                q.push(i);
        while (q.size()) {
            auto t = q.front();
            q.pop();
            for (auto u: g[t])
                if ( -- d[u] == 0)
                    q.push(u);
        }
        vector<int> res;
        for (int i = 0; i < n; i ++ )
            if (!d[i])
                res.push_back(i);
        return res;
    }
};
```
##### LeetCode 803. 打砖块
```cpp
class Solution {
public:
    int n, m;
    vector<int> p, sz;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int get(int x, int y) {
        return x * m + y;
    }

    vector<int> hitBricks(vector<vector<int>>& grid, vector<vector<int>>& hits) {
        n = grid.size(), m = grid[0].size();
        int S = n * m;
        for (int i = 0; i <= S; i ++ ) p.push_back(i), sz.push_back(1);

        vector<bool> st;
        for (auto& p: hits) {
            int x = p[0], y = p[1];
            if (grid[x][y]) {
                grid[x][y] = 0;
                st.push_back(true);
            } else {
                st.push_back(false);
            }
        }

        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (grid[i][j]) {
                    int a = get(i, j);
                    if (!i) {
                        if (find(S) != find(a)) {
                            sz[find(S)] += sz[find(a)];
                            p[find(a)] = find(S);
                        }
                    }
                    for (int k = 0; k < 4; k ++ ) {
                        int x = i + dx[k], y = j + dy[k];
                        if (x >= 0 && x < n && y >= 0 && y < m && grid[x][y]) {
                            int b = get(x, y);
                            if (find(a) != find(b)) {
                                sz[find(b)] += sz[find(a)];
                                p[find(a)] = find(b);
                            }
                        }
                    }
                }

        vector<int> res(hits.size());
        int last = sz[find(S)];
        for (int i = hits.size() - 1; i >= 0; i -- )
            if (st[i]) {
                int x = hits[i][0], y = hits[i][1];
                grid[x][y] = 1;
                int a = get(x, y);
                if (!x) {
                    if (find(S) != find(a)) {
                        sz[find(S)] += sz[find(a)];
                        p[find(a)] = find(S);
                    }
                }
                for (int j = 0; j < 4; j ++ ) {
                    int c = x + dx[j], d = y + dy[j];
                    if (c >= 0 && c < n && d >= 0 && d < m && grid[c][d]) {
                        int b = get(c, d);
                        if (find(a) != find(b)) {
                            sz[find(b)] += sz[find(a)];
                            p[find(a)] = find(b);
                        }
                    }
                }

                res[i] = max(0, sz[find(S)] - last - 1);
                last = sz[find(S)];
            }
        return res;
    }
};

```
##### LeetCode 804. 唯一摩尔斯密码词
```cpp
class Solution {
public:
    int uniqueMorseRepresentations(vector<string>& words) {
        string pos[26] = {
            ".-","-...","-.-.","-..",".",
            "..-.","--.","....","..",".---",
            "-.-",".-..","--","-.","---",
            ".--.","--.-",".-.","...","-",
            "..-","...-",".--","-..-","-.--","--.."
        };
        unordered_set<string> S;
        for (auto& w: words) {
            string s;
            for (auto c: w) s += pos[c - 'a'];
            S.insert(s);
        }
        return S.size();
    }
};
```
##### LeetCode 805. 数组的均值分割
```cpp
class Solution {
public:
    vector<int> nums;
    unordered_multiset<int> S;

    void dfs1(int u, int n, int sum) {
        if (u == n) S.insert(sum);
        else {
            dfs1(u + 1, n, sum);
            dfs1(u + 1, n, sum + nums[u]);
        }
    }

    bool dfs2(int u, int n, int sum, int cnt) {
        if (u == n) {
            if (cnt && cnt < n && S.count(-sum)) return true;
            return false;
        } else {
            if (dfs2(u + 1, n, sum, cnt)) return true;
            if (dfs2(u + 1, n, sum + nums[u], cnt + 1)) return true;
            return false;
        }
    }

    bool splitArraySameAverage(vector<int>& _nums) {
        nums = _nums;
        int n = nums.size();
        if (n == 1) return false;
        int sum = accumulate(nums.begin(), nums.end(), 0);
        for (auto& x: nums) x = x * n - sum;

        int m = n / 2;
        dfs1(m, n, 0);
        int s1 = 0, s2 = accumulate(nums.begin() + m, nums.begin() + n, 0);
        S.erase(S.find(s1));
        if (S.count(0)) return true;
        S.insert(s1), S.erase(S.find(s2));
        if (S.count(-accumulate(nums.begin(), nums.begin() + m, 0))) return true;
        return dfs2(0, m, 0, 0);
    }
};
```
##### LeetCode 806. 写字符串需要的行数
```cpp
class Solution {
public:
    vector<int> numberOfLines(vector<int>& widths, string s) {
        int r = 0, c = 0;
        for (auto x: s) {
            int w = widths[x - 'a'];
            if (c + w > 100) r ++ , c = 0;
            c += w;
        }
        return {r + 1, c};
    }
};
```
##### LeetCode 807. 保持城市天际线
```cpp
class Solution {
public:
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<int> r(n), c(m);
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                r[i] = max(r[i], grid[i][j]);
                c[j] = max(c[j], grid[i][j]);
            }
        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                res += min(r[i], c[j]) - grid[i][j];
        return res;
    }
};
```
##### LeetCode 808. 分汤
```cpp
class Solution {
public:
    int g(int x) {
        return max(0, x);
    }

    double soupServings(int n) {
         n = (n + 24) / 25;
         if (n >= 500) return 1;
         vector<vector<double>> f(n + 1, vector<double>(n + 1));
         for (int i = 0; i <= n; i ++ )
            for (int j = 0; j <= n; j ++ ) {
                if (!i && !j) f[i][j] = 0.5;
                else if (i && !j) f[i][j] = 0;
                else if (!i && j) f[i][j] = 1;
                else {
                    f[i][j] = (f[g(i - 4)][j] + f[g(i - 3)][g(j - 1)]
                        + f[g(i - 2)][g(j - 2)] + f[g(i - 1)][g(j - 3)]) / 4;
                }
            }
        return f[n][n];
    }
};
```
##### LeetCode 809. 情感丰富的文字
```cpp
class Solution {
public:
    int expressiveWords(string S, vector<string>& words) {
        if (S.empty()) {
            int res = 0;
            for (auto& w: words)
                if (w.empty())
                    res ++ ;
            return res;
        }
        vector<pair<char, int>> q;
        for (int i = 0; i < S.size(); i ++ ) {
            int j = i + 1;
            while (j < S.size() && S[i] == S[j]) j ++ ;
            q.push_back({S[i], j - i});
            i = j - 1;
        }
        int res = 0;
        for (auto& w: words) {
            int k = 0;
            for (int i = 0; i < w.size(); i ++ ) {
                if (k == q.size()) {
                    k = -1;
                    break;
                }
                if (q[k].first != w[i]) break;
                int j = i + 1;
                while (j < w.size() && w[i] == w[j]) j ++ ;
                int c1 = q[k].second, c2 = j - i;
                if (c1 < c2) break;
                if (c1 < 3 && c1 != c2) break;
                k ++, i = j - 1;
            }
            if (k == q.size()) res ++ ;
        }
        return res;
    }
};
```
##### LeetCode 810. 黑板异或游戏
```cpp
class Solution {
public:
    bool xorGame(vector<int>& nums) {
        int s = 0;
        for (auto x: nums) s ^= x;
        return !s || nums.size() % 2 == 0;
    }
};
```
##### LeetCode 811. 子域名访问计数
```cpp
class Solution {
public:
    vector<string> subdomainVisits(vector<string>& cpdomains) {
        unordered_map<string, int> cnt;
        for (auto& str: cpdomains) {
            int k = str.find(' ');
            int c = stoi(str.substr(0, k));
            str = str.substr(k + 1);
            while (true) {
                cnt[str] += c;
                k = str.find('.');
                if (k == -1) break;
                str = str.substr(k + 1);
            }
        }
        vector<string> res;
        for (auto& [k, v]: cnt)
            res.push_back(to_string(v) + ' ' + k);
        return res;
    }
};
```
##### LeetCode 812. 最大三角形面积
```cpp
class Solution {
public:
    int cross(int x1, int y1, int x2, int y2) {
        return x1 * y2 - x2 * y1;
    }

    int area(vector<int>& a, vector<int>& b, vector<int>& c) {
        return cross(b[0] - a[0], b[1] - a[1], c[0] - a[0], c[1] - a[1]);
    }

    double largestTriangleArea(vector<vector<int>>& points) {
        int res = 0;
        for (auto& a: points)
            for (auto& b: points)
                for (auto& c: points)
                    res = max(res, abs(area(a, b, c)));
        return res / 2.0;
    }
};
```
##### LeetCode 813. 最大平均值和的分组
```cpp
class Solution {
public:
    double largestSumOfAverages(vector<int>& A, int m) {
        int n = A.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + A[i - 1];
        vector<vector<double>> f(n + 1, vector<double>(m + 1, -1e9));
        f[0][0] = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                for (int k = 0; k < i; k ++ )
                    f[i][j] = max(f[i][j], f[k][j - 1] + (s[i] - s[k]) / (double)(i - k));
        return f[n][m];
    }
};
```
##### LeetCode 814. 二叉树剪枝
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* pruneTree(TreeNode* root) {
        if (!dfs(root)) return NULL;
        return root;
    }

    bool dfs(TreeNode* root) {
        if (!root) return false;
        if (!dfs(root->left)) root->left = NULL;
        if (!dfs(root->right)) root->right = NULL;
        return root->val || root->left || root->right;
    }
};
```
##### LeetCode 815. 公交路线
```cpp
class Solution {
public:
    int numBusesToDestination(vector<vector<int>>& routes, int source, int target) {
        if (source == target) return 0;
        int n = routes.size();
        unordered_map<int, vector<int>> g;
        vector<int> dist(n, 1e8);
        queue<int> q;
        for (int i = 0; i < n; i ++ ) {
            for (int x: routes[i]) {
                if (x == source) {
                    dist[i] = 1;
                    q.push(i);
                }
                g[x].push_back(i);
            }
        }
        while (q.size()) {
            int t = q.front();
            q.pop();

            for (auto x: routes[t]) {
                if (x == target) return dist[t];
                for (auto y: g[x]) {
                    if (dist[y] > dist[t] + 1) {
                        dist[y] = dist[t] + 1;
                        q.push(y);
                    }
                }
                g.erase(x);
            }
        }
        return -1;
    }
};
```
##### LeetCode 816. 模糊坐标
```cpp
class Solution {
public:
    vector<string> get(string s) {
        vector<string> res;
        if (s.size() == 1 || s[0] != '0') res.push_back(s);
        for (int i = 1; i < s.size(); i ++ ) {
            string a = s.substr(0, i), b = s.substr(i);
            if (a.size() > 1 && a[0] == '0') continue;  // 有前导零
            if (b.back() == '0') continue;  // 末尾多余的零
            res.push_back(a + '.' + b);
        }
        return res;
    }

    vector<string> ambiguousCoordinates(string s) {
        vector<string> res;
        s = s.substr(1, s.size() - 2);
        for (int i = 1; i < s.size(); i ++ ) {
            auto l = get(s.substr(0, i)), r = get(s.substr(i));
            for (auto x: l)
                for (auto y: r)
                    res.push_back('(' + x + ", " + y + ')');
        }
        return res;
    }
};

```
##### LeetCode 817. 链表组件
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int numComponents(ListNode* head, vector<int>& G) {
        unordered_set<int> S(G.begin(), G.end());
        int res = 0, s = 0;
        for (auto p = head; p; p = p->next) {
            if (S.count(p->val)) s ++ ;
            else {
                if (s) {
                    s = 0;
                    res ++ ;
                }
            }
        }
        if (s) res ++ ;
        return res;
    }
};
```
##### LeetCode 818. 赛车
```cpp
int dist[20010][15][2];

class Solution {
public:
    struct Node {
        int x, k, d;
    };

    int racecar(int target) {
        memset(dist, 0x3f, sizeof dist);
        queue<Node> q;
        dist[0][0][1] = 0;
        q.push({0, 0, 1});
        while (q.size()) {
            auto t = q.front();
            q.pop();

            int distance = dist[t.x][t.k][t.d];
            int x = t.x + (1 << t.k) * (t.d * 2 - 1);
            if (x >= 0 && x <= target * 2) {
                int k = t.k + 1, d = t.d;
                if (dist[x][k][d] > distance + 1) {
                    dist[x][k][d] = distance + 1;
                    if (x == target) return distance + 1;
                    q.push({x, k, d});
                }
            }

            x = t.x;
            int k = 0, d = t.d ^ 1;
            if (dist[x][k][d] > distance + 1) {
                dist[x][k][d] = distance + 1;
                q.push({x, k, d});
            }
        }
        return -1;
    }
};
```
##### LeetCode 819. 最常见的单词
```cpp
class Solution {
public:
    string mostCommonWord(string paragraph, vector<string>& banned) {
        unordered_set<string> S(banned.begin(), banned.end());
        unordered_map<string, int> cnt;
        string word;
        for (auto c: paragraph) {
            c = tolower(c);
            if (c >= 'a' && c <= 'z') word += c;
            else {
                if (word.size()) {
                    if (!S.count(word)) cnt[word] ++ ;
                    word.clear();
                }
            }
        }
        if (word.size() && !S.count(word))
            cnt[word] ++ ;

        string res;
        for (auto& [k, v]: cnt)
            if (cnt[res] < v)
                res = k;
        return res;
    }
};

```
##### LeetCode 820. 单词的压缩编码
```cpp
const int N = 2000 * 7 + 10;

int son[N][26], cnt[N], len[N], idx;

class Solution {
public:
    void insert(string& s) {
        reverse(s.begin(), s.end());
        int p = 0;
        for (auto c: s) {
            int u = c - 'a';
            if (!son[p][u]) son[p][u] = ++ idx;
            cnt[p] ++ ;
            p = son[p][u];
        }
        len[p] = s.size();
    }

    int minimumLengthEncoding(vector<string>& words) {
        memset(son, 0, sizeof son);
        memset(cnt, 0, sizeof cnt);
        idx = 0;

        for (auto& w: words) insert(w);
        int res = 0;
        for (int i = 1; i <= idx; i ++ )
            if (!cnt[i])
                res += len[i] + 1;
        return res;
    }
};
```


### 究极班- Week 34(第 821 ~ 830 题)

##### LeetCode 821. 字符的最短距离
```cpp
class Solution {
public:
    vector<int> shortestToChar(string s, char c) {
        int n = s.size();
        vector<int> res(n, INT_MAX);
        for (int i = 0, j = -1; i < n; i ++ ) {
            if (s[i] == c) j = i;
            if (j != -1) res[i] = i - j;
        }
        for (int i = n - 1, j = -1; i >= 0; i -- ) {
            if (s[i] == c) j = i;
            if (j != -1) res[i] = min(res[i], j - i);
        }
        return res;
    }
};
```
##### LeetCode 822. 翻转卡片游戏
```cpp
class Solution {
public:
    int flipgame(vector<int>& fronts, vector<int>& backs) {
        unordered_set<int> S;
        for (int i = 0; i < fronts.size(); i ++ )
            if (fronts[i] == backs[i])
                S.insert(fronts[i]);
        int res = INT_MAX;
        for (auto x: fronts)
            if (!S.count(x))
                res = min(res, x);
        for (auto x: backs)
            if (!S.count(x))
                res = min(res, x);
        if (res == INT_MAX) res = 0;
        return res;
    }
};
```
##### LeetCode 823. 带因子的二叉树
```cpp
class Solution {
public:
    int numFactoredBinaryTrees(vector<int>& arr) {
        int n = arr.size();
        sort(arr.begin(), arr.end());
        unordered_map<int, int> hash;
        for (int i = 0; i < n; i ++ ) hash[arr[i]] = i;
        vector<int> f(n);

        int res = 0, MOD = 1e9 + 7;
        for (int i = 0; i < n; i ++ ) {
            f[i] = 1;
            for (int j = 0; j < i; j ++ ) {
                if (arr[i] % arr[j] == 0) {
                    int d = arr[i] / arr[j];
                    if (hash.count(d)) {
                        int k = hash[d];
                        f[i] = (f[i] + (long long)f[j] * f[k]) % MOD;
                    }
                }
            }
            res = (res + f[i]) % MOD;
        }
        return res;
    }
};
```
##### LeetCode 824. 山羊拉丁文
```cpp
class Solution {
public:
    string toGoatLatin(string s) {
        unordered_set<char> S{'a', 'e', 'i', 'o', 'u'};
        string res;
        for (int i = 0, k = 1; i < s.size(); i ++ ) {
            int j = i + 1;
            while (j < s.size() && s[j] != ' ') j ++ ;
            string word = s.substr(i, j - i);
            i = j;
            if (S.count(tolower(word[0]))) word += "ma";
            else {
                word = word.substr(1) + word[0] + "ma";
            }
            word += string(k, 'a');
            k ++ ;
            res += word + ' ';
        }
        res.pop_back();
        return res;
    }
}
```
##### LeetCode 825. 适龄的朋友
```cpp
class Solution {
public:
    int numFriendRequests(vector<int>& ages) {
        int n = ages.size();
        int s[121] = {0};
        for (int i = 0; i < n; i ++ ) s[ages[i]] ++ ;
        int res = n * n;
        for (int i = 1; i <= 120; i ++ )
            for (int j = 1; j <= 120; j ++ )
                if (j <= 0.5 * i + 7 || j > i)
                    res -= s[i] * s[j];
                else if (i == j)
                    res -= s[i];
        return res;
    }
};

```
##### LeetCode 826. 安排工作以达到最大收益
```cpp
class Solution {
public:
    struct Task {
        int d, p;
        bool operator< (const Task& t) const {
            return d < t.d;
        }
    };

    int maxProfitAssignment(vector<int>& difficulty, vector<int>& profit, vector<int>& worker) {
        vector<Task> tasks(difficulty.size());
        for (int i = 0; i < difficulty.size(); i ++ )
            tasks[i] = {difficulty[i], profit[i]};
        sort(tasks.begin(), tasks.end());
        sort(worker.begin(), worker.end());
        int res = 0, p = 0;
        for (int i = 0, j = 0; i < worker.size(); i ++ ) {
            while (j < tasks.size() && tasks[j].d <= worker[i])
                p = max(p, tasks[j ++ ].p);
            res += p;
        }
        return res;
    }
};
```
##### LeetCode 827. 最大人工岛
```cpp
class Solution {
public:
    int n, m;
    vector<int> p, sz;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int get(int x, int y) {
        return x * m + y;
    }

    int largestIsland(vector<vector<int>>& grid) {
        n = grid.size(), m = grid[0].size();
        for (int i = 0; i < n * m; i ++ ) p.push_back(i), sz.push_back(1);

        int res = 1;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (grid[i][j]) {
                    int a = get(i, j);
                    for (int k = 0; k < 4; k ++ ) {
                        int x = i + dx[k], y = j + dy[k];
                        if (x >= 0 && x < n && y >= 0 && y < m && grid[x][y]) {
                            int b = get(x, y);
                            if (find(a) != find(b)) {
                                sz[find(b)] += sz[find(a)];
                                p[find(a)] = find(b);
                            }
                        }
                    }
                    res = max(res, sz[find(a)]);
                }

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!grid[i][j]) {
                    map<int, int> hash;
                    for (int k = 0; k < 4; k ++ ) {
                        int x = i + dx[k], y = j + dy[k];
                        if (x >= 0 && x < n && y >= 0 && y < m && grid[x][y]) {
                            int a = get(x, y);
                            hash[find(a)] = sz[find(a)];
                        }
                    }
                    int s = 1;
                    for (auto [k, v]: hash) s += v;
                    res = max(res, s);
                }
        return res;
    }
};

```
##### LeetCode 828. 统计子串中的唯一字符
```cpp
class Solution {
public:
    int uniqueLetterString(string s) {
        int n = s.size();
        vector<int> l(n), r(n);
        vector<int> p(26, -1);
        for (int i = 0; i < n; i ++ ) {
            l[i] = p[s[i] - 'A'];
            p[s[i] - 'A'] = i;
        }
        p = vector<int>(26, n);
        for (int i = n - 1; i >= 0; i -- ) {
            r[i] = p[s[i] - 'A'];
            p[s[i] - 'A'] = i;
        }
        int res = 0, MOD = 1e9 + 7;
        for (int i = 0; i < n; i ++ )
            res = (res + (long long)(i - l[i]) * (r[i] - i)) % MOD;
        return res;
    }
};

```
##### LeetCode 829. 连续整数求和
```cpp
class Solution {
public:
    int consecutiveNumbersSum(int N) {
        N *= 2;
        int res = 0;
        for (int b = 1; b * b <= N; b ++ ) {
            if (N % b == 0) {
                if ((N / b - b + 1) % 2 == 0)
                    res ++ ;
            }
        }
        return res;
    }
};

```
##### LeetCode 830. 较大分组的位置
```cpp
class Solution {
public:
    vector<vector<int>> largeGroupPositions(string s) {
        vector<vector<int>> res;
        for (int i = 0; i < s.size(); i ++ ) {
            int j = i + 1;
            while (j < s.size() && s[i] == s[j]) j ++ ;
            if (j - i >= 3) res.push_back({i, j - 1});
            i = j - 1;
        }
        return res;
    }
};
```




### 究极班—— Week 35（第 831 ~ 850 题）

#####  LeetCode  831. 隐藏个人信息
```C++
class Solution {
public:
    string work_email(string& t) {
        string s;
        for (auto c: t) s += tolower(c);
        int a = s.find('@'), b = s.find('.');
        string name1 = s.substr(0, a);
        string name2 = s.substr(a + 1, b - a - 1);
        string name3 = s.substr(b + 1);
        return name1[0] + string(5, '*') + name1.back() + '@' + name2 + '.' + name3;
    }

    string work_phone(string t) {
        string s;
        for (auto c: t)
            if (isdigit(c))
                s += c;
        if (s.size() == 10)
            return "***-***-" + s.substr(6);
        return "+" + string(s.size() - 10, '*') + "-***-***-" + s.substr(s.size() - 4);
    }

    string maskPII(string s) {
        if (s.find('@') != -1)
            return work_email(s);
        else
            return work_phone(s);
    }
};

```


#####  LeetCode  832. 翻转图像
```C++
class Solution {
public:
    vector<vector<int>> flipAndInvertImage(vector<vector<int>>& image) {
        int n = image.size(), m = image[0].size();
        for (int i = 0; i < n; i ++ ) {
            reverse(image[i].begin(), image[i].end());
            for (int j = 0; j < m; j ++ )
                image[i][j] ^= 1;
        }
        return image;
    }
};
```
#####  LeetCode  833. 字符串中的查找与替换
```C++
class Solution {
public:
    string findReplaceString(string s, vector<int>& indexes, vector<string>& sources, vector<string>& targets) {
        int n = s.size(), m = indexes.size();
        vector<int> id(m);
        for (int i = 0; i < m; i ++ ) id[i] = i;
        sort(id.begin(), id.end(), [&](int a, int b) {
            return indexes[a] < indexes[b];
        });
        for (int i = m - 1; i >= 0; i -- ) {
            int k = id[i];
            if (s.substr(indexes[k], sources[k].size()) == sources[k])
                s = s.substr(0, indexes[k]) + targets[k] + s.substr(indexes[k] + sources[k].size());
        }
        return s;
    }
};
```
#####  LeetCode  834. 树中距离之和
```C++
const int N = 10010, M = 20010;

int h[N], e[M], ne[M], idx;
int sum[N], cnt[N], up[N];

class Solution {
public:
    int n;

    void add(int a, int b) {
        e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
    }

    void dfs1(int u, int father) {
        sum[u] = 0;
        cnt[u] = 1;
        for (int i = h[u]; ~i; i = ne[i]) {
            int j = e[i];
            if (j == father) continue;
            dfs1(j, u);
            sum[u] += sum[j] + cnt[j];
            cnt[u] += cnt[j];
        }
    }

    void dfs2(int u, int father) {
        for (int i = h[u]; ~i; i = ne[i]) {
            int j = e[i];
            if (j == father) continue;
            up[j] = up[u] + sum[u] - (sum[j] + cnt[j]) + n - cnt[j];
            dfs2(j, u);
        }
    }

    vector<int> sumOfDistancesInTree(int N, vector<vector<int>>& edges) {
        memset(h, -1, sizeof h);
        idx = 0;
        n = N;
        for (auto& e: edges) {
            int a = e[0], b = e[1];
            add(a, b), add(b, a);
        }

        dfs1(0, -1);
        dfs2(0, -1);

        vector<int> res;
        for (int i = 0; i < n; i ++ )
            res.push_back(sum[i] + up[i]);
        return res;
    }
};

```
#####  LeetCode  835. 图像重叠
```C++
class Solution {
public:
    int largestOverlap(vector<vector<int>>& img1, vector<vector<int>>& img2) {
        int res = 0;
        int n = img1.size();
        for (int i = -n; i < n; i ++ )
            for (int j = -n; j < n; j ++ ) {
                int cnt = 0;
                for (int x = max(0, -i); x < min(n, n - i); x ++ )
                    for (int y = max(0, -j); y < min(n, n - j); y ++ )
                        if (img1[i + x][j + y] && img2[x][y])
                            cnt ++ ;
                res = max(res, cnt);
            }
        return res;
    }
};
```
#####  LeetCode  836. 矩形重叠
```C++
class Solution {
public:
    bool check(int a, int b, int c, int d) {
        return a < b && c < d && b > c && d > a;
    }

    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        return check(rec1[0], rec1[2], rec2[0], rec2[2])
            && check(rec1[1], rec1[3], rec2[1], rec2[3]);
    }
};
```
#####  LeetCode  837. 新21点
```C++
const int N = 20010;
double f[N];

class Solution {
public:
    double new21Game(int N, int K, int W) {
        if (!K) return 1;
        memset(f, 0, sizeof f);
        for (int i = K; i <= N && i < K + W; i ++ ) f[i] = 1;
        f[K - 1] = 0;
        for (int i = 1; i <= W; i ++ ) f[K - 1] += f[K - 1 + i] / (double)W;
        for (int i = K - 2; i >= 0; i -- )
            f[i] = f[i + 1] + (f[i + 1] - f[i + W + 1]) / (double)W;
        return f[0];
    }
};
```
#####  LeetCode  838. 推多米诺
```C++
class Solution {
public:
    string pushDominoes(string s) {
        s = 'L' + s + 'R';
        int n = s.size();
        vector<int> l(n), r(n);
        for (int i = 0, j = 0; i < n; i ++ ) {
            if (s[i] != '.') j = i;
            l[i] = j;
        }
        for (int i = n - 1, j = 0; i >= 0; i -- ) {
            if (s[i] != '.') j = i;
            r[i] = j;
        }

        for (int i = 0; i < n; i ++ ) {
            char L = s[l[i]], R = s[r[i]];
            if (L == 'L' && R == 'R') s[i] = '.';
            else if (L == 'L' && R == 'L') s[i] = 'L';
            else if (L == 'R' && R == 'R') s[i] = 'R';
            else {
                if (i - l[i] < r[i] - i) s[i] = 'R';
                else if (r[i] - i < i - l[i]) s[i] = 'L';
                else s[i] = '.';
            }
        }
        return s.substr(1, n - 2);
    }
};

```
#####  LeetCode  839. 相似字符串组
```C++
class Solution {
public:
    int n;
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    bool check(string& a, string& b) {
        if (a == b) return true;
        vector<int> q;
        for (int i = 0; i < a.size(); i ++ )
            if (a[i] != b[i])
                q.push_back(i);

        if (q.size() != 2) return false;
        int x = q[0], y = q[1];
        return a[x] == b[y] && a[y] == b[x];
    }

    int numSimilarGroups(vector<string>& strs) {
        n = strs.size();
        p.resize(n);
        for (int i = 0; i < n; i ++ ) p[i] = i;

        int res = n;
        for (int i = 0; i < n; i ++ )
            for (int j = i + 1; j < n; j ++ )
                if (check(strs[i], strs[j])) {
                    if (find(i) != find(j)) {
                        p[find(i)] = find(j);
                        res -- ;
                    }
                }

        return res;
    }
};
```
#####  LeetCode  840. 矩阵中的幻方
```C++
class Solution {
public:
    int n, m;
    vector<vector<int>> g;

    bool check(int x, int y) {
        bool st[10] = {0};
        for (int i = x; i < x + 3; i ++ )
            for (int j = y; j < y + 3; j ++ ) {
                int t = g[i][j];
                if (t < 1 || t > 9) return false;
                if (st[t]) return false;
                st[t] = true;
            }

        for (int i = 0; i < 3; i ++ ) {
            if (g[x + i][y] + g[x + i][y + 1] + g[x + i][y + 2] != 15) return false;
            if (g[x][y + i] + g[x + 1][y + i] + g[x + 2][y + i] != 15) return false;
        }

        if (g[x][y] + g[x + 1][y + 1] + g[x + 2][y + 2] != 15) return false;
        if (g[x + 2][y] + g[x + 1][y + 1] + g[x][y + 2] != 15) return false;
        return true;
    }

    int numMagicSquaresInside(vector<vector<int>>& grid) {
        g = grid;
        n = g.size(), m = g[0].size();
        int res = 0;
        for (int i = 0; i + 3 <= n; i ++ )
            for (int j = 0; j + 3 <= m; j ++ )
                if (check(i, j))
                    res ++ ;
        return res;
    }
};

```



#####  LeetCode 841. 钥匙和房间
```cpp
class Solution {
public:
    int n;
    vector<vector<int>> g;
    vector<bool> st;

    void dfs(int u) {
        st[u] = true;
        for (auto v: g[u])
            if (!st[v])
                dfs(v);
    }

    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        g = rooms;
        n = g.size();
        st.resize(n);

        dfs(0);

        for (int i = 0; i < n; i ++ )
            if (!st[i])
                return false;
        return true;
    }
};
```
#####  LeetCode 842. 将数组拆分成斐波那契序列
```cpp
typedef long long LL;

class Solution {
public:
    vector<int> get(LL a, LL b, string& s) {
        vector<int> res;
        res = {(int)a, (int)b};
        string t = to_string(a) + to_string(b);
        while (t.size() < s.size()) {
            auto c = a + b;
            if (c > INT_MAX) return {};
            t += to_string(c);
            res.push_back((int)c);
            a = b, b = c;
        }
        if (t != s || res.size() < 3) return {};
        return res;
    }

    vector<int> splitIntoFibonacci(string s) {
        for (int i = 1; i <= 10 && i < s.size(); i ++ )
            for (int j = i + 1; j <= i + 10 && j < s.size(); j ++ ) {
                auto a = stoll(s.substr(0, i)), b = stoll(s.substr(i, j - i));
                auto res = get(a, b, s);
                if (res.size()) return res;
            }
        return {};
    }
};
```
#####  LeetCode 843. 猜猜这个单词
```cpp
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Master {
 *   public:
 *     int guess(string word);
 * };
 */
class Solution {
public:
    int n;
    vector<vector<int>> f;
    vector<bool> st;

    int get(int j, int u) {
        int res = 0;
        for (int i = 0; i < n; i ++ )
            if (!st[i] && f[j][i] == u)
                res ++ ;
        return res;
    }

    void findSecretWord(vector<string>& ws, Master& master) {
        n = ws.size();
        f = vector<vector<int>>(n, vector<int>(n));
        st = vector<bool>(n);
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                for (int k = 0; k < 6; k ++ )
                    if (ws[i][k] == ws[j][k])
                        f[i][j] ++ ;

        for (int i = 0; i < 10; i ++ ) {
            int k = -1, w = INT_MAX;
            for (int j = 0; j < n; j ++ ) {
                if (st[j]) continue;
                int t = 0;
                for (int u = 0; u <= 6; u ++ )
                    t = max(t, get(j, u));
                if (w > t) k = j, w = t;
            }
            int res = master.guess(ws[k]);
            if (res == 6) break;
            st[k] = true;
            for (int j = 0; j < n; j ++ )
                if (f[k][j] != res)
                    st[j] = true;
        }
    }
};
```
#####  LeetCode 844. 比较含退格的字符串
```cpp
class Solution {
public:
    string get(string& s) {
        string res;
        for (auto c: s)
            if (c == '#') {
                if (res.size()) res.pop_back();
            } else {
                res += c;
            }
        return res;
    }

    bool backspaceCompare(string s, string t) {
        return get(s) == get(t);
    }
};
```
#####  LeetCode 845. 数组中的最长山脉
```cpp
class Solution {
public:
    int longestMountain(vector<int>& arr) {
        int n = arr.size();
        vector<int> l(n, 1), r(n, 1);
        for (int i = 1; i < n; i ++ )
            if (arr[i] > arr[i - 1])
                l[i] = l[i - 1] + 1;
        for (int i = n - 2; i >= 0; i -- )
            if (arr[i] > arr[i + 1])
                r[i] = r[i + 1] + 1;

        int res = 0;
        for (int i = 0; i < n; i ++ )
            if (l[i] > 1 && r[i] > 1)
                res = max(res, l[i] + r[i] - 1);
        return res;
    }
};
```
#####  LeetCode 846. 一手顺子
```cpp
class Solution {
public:
    bool isNStraightHand(vector<int>& hand, int W) {
        multiset<int> S;
        for (auto x: hand) S.insert(x);
        while (S.size()) {
            int x = *S.begin();
            for (int i = x; i < x + W; i ++ )
                if (!S.count(i))
                    return false;
                else
                    S.erase(S.find(i));
        }
        return true;
    }
};
```
#####  LeetCode 847. 访问所有节点的最短路径
```cpp
class Solution {
public:
    int shortestPathLength(vector<vector<int>>& graph) {
        int n = graph.size(), INF = 1e8;
        vector<vector<int>> f(1 << n, vector<int>(n, INF));
        typedef pair<int, int> PII;

        #define x first
        #define y second

        queue<PII> q;
        for (int i = 0; i < n; i ++ ) {
            int x = 1 << i, y = i;
            f[x][y] = 0;
            q.push({x, y});
        }

        while (q.size()) {
            auto t = q.front();
            q.pop();

            for (auto z: graph[t.y]) {
                int a = t.x | 1 << z, b = z;
                if (f[a][b] > f[t.x][t.y] + 1) {
                    f[a][b] = f[t.x][t.y] + 1;
                    q.push({a, b});
                }
            }
        }

        int res = INF;
        for (int i = 0; i < n; i ++ )
            res = min(res, f[(1 << n) - 1][i]);
        return res;
    }
};
```
#####  LeetCode 848. 字母移位
```cpp
class Solution {
public:
    string shiftingLetters(string s, vector<int>& shifts) {
        string res;
        for (int i = s.size() - 1, sum = 0; i >= 0; i -- ) {
            sum = (sum + shifts[i]) % 26;
            res += (s[i] - 'a' + sum) % 26 + 'a';
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode 849. 到最近的人的最大距离
```cpp
class Solution {
public:
    int maxDistToClosest(vector<int>& seats) {
        int res = 0;
        for (int i = 0; i < seats.size(); i ++ ) {
            if (seats[i]) continue;
            int j = i + 1;
            while (j < seats.size() && !seats[j]) j ++ ;
            if (!i || j == seats.size()) res = max(res, j - i);
            else res = max(res, (j - i + 1) / 2);
        }
        return res;
    }
};
```
#####  LeetCode 850. 矩形面积 II
```cpp
typedef long long LL;
typedef pair<int, int> PII;

#define x first
#define y second

class Solution {
public:
    LL calc(vector<vector<int>>& rts, int a, int b) {
        vector<PII> q;
        for (auto& r: rts)
            if (r[0] <= a && r[2] >= b)
                q.push_back({r[1], r[3]});
        sort(q.begin(), q.end());
        LL res = 0, st = -1, ed = -1;
        for (auto& r: q)
            if (r.x > ed) {
                res += ed - st;
                st = r.x, ed = r.y;
            } else if (r.y > ed) {
                ed = r.y;
            }
        res += ed - st;
        return res * (b - a);
    }

    int rectangleArea(vector<vector<int>>& rts) {
        vector<int> xs;
        for (auto& r: rts) {
            xs.push_back(r[0]);
            xs.push_back(r[2]);
        }
        sort(xs.begin(), xs.end());

        LL res = 0;
        for (int i = 1; i < xs.size(); i ++ )
            res += calc(rts, xs[i - 1], xs[i]);
        return res % 1000000007;
    }
};

```




### 究极班- Week 36（第 851 ~ 870 题）

##### LeetCode 851. 喧闹和富有
```cpp
class Solution {
public:
    vector<vector<int>> g;
    vector<int> w;
    vector<int> ans;

    void dfs(int u) {
        if (ans[u] != -1) return ;
        ans[u] = u;
        for (auto v: g[u]) {
            dfs(v);
            if (w[ans[u]] > w[ans[v]])
                ans[u] = ans[v];
        }
    }

    vector<int> loudAndRich(vector<vector<int>>& richer, vector<int>& quiet) {
        int n = quiet.size();
        w = quiet;
        g.resize(n);
        ans.resize(n, -1);
        for (auto& e: richer) {
            int a = e[0], b = e[1];
            g[b].push_back(a);
        }
        for (int i = 0; i < n; i ++ ) dfs(i);
        return ans;
    }
};
```
##### LeetCode 852. 山脉数组的峰顶索引
```cpp
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int l = 1, r = arr.size() - 2;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (arr[mid] > arr[mid - 1]) l = mid;
            else r = mid - 1;
        }
        return r;
    }
};
```
##### LeetCode 853. 车队
```cpp
class Solution {
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        int n = position.size();
        vector<int> id(n);
        for (int i = 0; i < n; i ++ ) id[i] = i;
        sort(id.begin(), id.end(), [&](int a, int b) {
            return position[a] < position[b];
        });
        int res = n;
        const double eps = 1e-6;
        double last = 0;
        for (int i = n - 1; i >= 0; i -- ) {
            auto t = (target - position[id[i]]) / (double)speed[id[i]];
            if (t < last + eps) res -- ;
            else last = t;
        }
        return res;
    }
};
```
##### LeetCode 854. 相似度为 K 的字符串
```cpp
class Solution {
public:
    int h(string& s1, string& s2) {
        int res = 0;
        for (int i = 0; i < s1.size(); i ++ )
            if (s1[i] != s2[i])
                res ++ ;
        return (res + 1) / 2;
    }

    bool dfs(string& s1, string& s2, int depth) {
        if (!depth) return s1 == s2;
        if (h(s1, s2) > depth) return false;
        for (int i = 0; i < s1.size(); i ++ )
            if (s1[i] != s2[i]) {
                for (int j = i + 1; j < s1.size(); j ++ )
                    if (s2[i] == s1[j]) {
                        swap(s1[i], s1[j]);
                        if  (dfs(s1, s2, depth - 1)) return true;
                        swap(s1[i], s1[j]);
                    }
                break;
            }
        return false;
    }

    int kSimilarity(string s1, string s2) {
        if (s1 == s2) return 0;
        int depth = 1;
        while (!dfs(s1, s2, depth)) depth ++ ;
        return depth;
    }
};

```
##### LeetCode 855. 考场就座
```cpp
class ExamRoom {
public:
    int n;
    set<int> S;

    ExamRoom(int _n) {
        n = _n;
    }

    int seat() {
        int p = 0;
        if (S.size()) {
            int dist = *S.begin();
            for (auto i = S.begin(); i != S.end(); i ++ ) {
                auto j = i;
                j ++ ;
                if (j != S.end()) {
                    if (dist < (*j - *i) / 2) {
                        dist = (*j - *i) / 2;
                        p = *i + dist;
                    }
                } else {
                    if (dist < n - 1 - *i) {
                        dist = n - 1 - *i;
                        p = n - 1;
                    }
                }
            }
        }
        S.insert(p);
        return p;
    }

    void leave(int p) {
        S.erase(p);
    }
};

/**
 * Your ExamRoom object will be instantiated and called as such:
 * ExamRoom* obj = new ExamRoom(n);
 * int param_1 = obj->seat();
 * obj->leave(p);
 */

```
##### LeetCode 856. 括号的分数
```cpp
class Solution {
public:
    int scoreOfParentheses(string s) {
        stack<int> stk;
        stk.push(0);
        for (auto c: s) {
            if (c == '(') stk.push(0);
            else {
                int t = stk.top();
                stk.pop();
                if (!t) t = 1;
                else t *= 2;
                stk.top() += t;
            }
        }
        return stk.top();
    }
};
```
##### LeetCode 857. 雇佣 K 名工人的最低成本
```cpp
typedef pair<double, int> PDI;

#define x first
#define y second

class Solution {
public:
    double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int k) {
        vector<PDI> q;
        for (int i = 0; i < quality.size(); i ++ )
            q.push_back({(double)wage[i] / quality[i], quality[i]});
        sort(q.begin(), q.end());
        priority_queue<int> heap;
        double res = 1e18, sum = 0;
        for (auto& p: q) {
            heap.push(p.y);
            sum += p.y;
            if (heap.size() > k) {
                sum -= heap.top();
                heap.pop();
            }
            if (heap.size() == k)
                res = min(res, sum * p.x);
        }
        return res;
    }
};

```
##### LeetCode 858. 镜面反射
```cpp
class Solution {
public:
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }

    int mirrorReflection(int p, int q) {
        int Y = p * q / gcd(p, q);
        int x = Y / q, y = Y / p;
        if (x % 2) {
            if (y % 2) return 1;
            return 0;
        }
        return 2;
    }
};

```
##### LeetCode 859. 亲密字符串
```cpp
class Solution {
public:
    bool buddyStrings(string a, string b) {
        if (a.size() != b.size()) return false;
        if (a == b) {
            unordered_map<char, int> cnt;
            for (auto c: a)
                if ( ++ cnt[c] > 1)
                    return true;
            return false;
        }
        vector<int> q;
        for (int i = 0; i < a.size(); i ++ )
            if (a[i] != b[i])
                q.push_back(i);
        if (q.size() != 2) return false;
        int x = q[0], y = q[1];
        if (a[x] == b[y] && a[y] == b[x]) return true;
        return false;
    }
};

```
##### LeetCode 860. 柠檬水找零
```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0;
        for (auto x: bills) {
            if (x == 5) five ++ ;
            else if (x == 10) {
                if (!five) return false;
                five -- ;
                ten ++ ;
            } else {
                if (ten && five) ten --, five -- ;
                else if (five >= 3) five -= 3;
                else return false;
            }
        }
        return true;
    }
};

```
##### LeetCode 861. 翻转矩阵后的得分
```cpp
class Solution {
public:
    int matrixScore(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        for (int i = 0; i < n; i ++ )
            if (!grid[i][0]) {
                for (int j = 0; j < m; j ++ )
                    grid[i][j] ^= 1;
            }

        int res = (1 << m - 1) * n;
        for (int i = 1; i < m; i ++ ) {
            int cnt = 0;
            for (int j = 0; j < n; j ++ )
                if (grid[j][i])
                    cnt ++ ;
            res += (1 << m - 1 - i) * max(cnt, n - cnt);
        }
        return res;
    }
};
```
##### LeetCode 862. 和至少为 K 的最短子数组
```cpp
typedef long long LL;

class Solution {
public:
    int shortestSubarray(vector<int>& nums, int k) {
        int n = nums.size();
        vector<LL> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + nums[i - 1];
        deque<int> q;
        q.push_back(0);
        int res = INT_MAX;
        for (int i = 1; i <= n; i ++ ) {
            while (q.size() && s[q.front()] + k <= s[i]) {
                res = min(res, i - q.front());
                q.pop_front();
            }
            while (q.size() && s[q.back()] >= s[i]) q.pop_back();
            q.push_back(i);
        }
        if (res == INT_MAX) res = -1;
        return res;
    }
};

```
##### LeetCode 863. 二叉树中所有距离为 K 的结点
```cpp

```
##### LeetCode 864. 获取所有钥匙的最短路径
```cpp
int dist[31][31][64];

class Solution {
public:
    struct Node {
        int x, y, s;
    };

    int shortestPathAllKeys(vector<string>& grid) {
        int n = grid.size(), m = grid[0].size(), S = 0;
        memset(dist, 0x3f, sizeof dist);
        queue<Node> q;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (grid[i][j] == '@') {
                    dist[i][j][0] = 0;
                    q.push({i, j, 0});
                } else if (grid[i][j] >= 'A' && grid[i][j] <= 'Z') {
                    S ++ ;
                }

        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        while (q.size()) {
            auto t = q.front();
            q.pop();
            int d = dist[t.x][t.y][t.s];

            for (int i = 0; i < 4; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i], s = t.s;
                if (x < 0 || x >= n || y < 0 || y >= m || grid[x][y] == '#') continue;
                char c = grid[x][y];
                if (c >= 'a' && c <= 'z') {
                    s |= 1 << c - 'a';
                    if (dist[x][y][s] > d + 1) {
                        dist[x][y][s] = d + 1;
                        if (s == (1 << S) - 1) return d + 1;
                        q.push({x, y, s});
                    }
                } else if (c >= 'A' && c <= 'Z') {
                    if (s & (1 << c - 'A')) {
                        if (dist[x][y][s] > d + 1) {
                            dist[x][y][s] = d + 1;
                            q.push({x, y, s});
                        }
                    }
                } else {
                    if (dist[x][y][s] > d + 1) {
                        dist[x][y][s] = d + 1;
                        q.push({x, y, s});
                    }
                }
            }
        }
        return -1;
    }
};

```
##### LeetCode 865. 具有所有最深节点的最小子树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    pair<TreeNode*, int> dfs(TreeNode* root) {
        if (!root) return {NULL, 0};
        auto L = dfs(root->left), R = dfs(root->right);
        if (L.second == R.second) return {root, L.second + 1};
        if (L.second > R.second) return {L.first, L.second + 1};
        return {R.first, R.second + 1};
    }

    TreeNode* subtreeWithAllDeepest(TreeNode* root) {
        return dfs(root).first;
    }
};
```


##### LeetCode 866. 回文素数
```cpp
class Solution {
public:
    int get(int x) {
        string a = to_string(x);
        string b = a;
        reverse(b.begin(), b.end());
        return stoi(a + b.substr(1));
    }

    int is_prime(int x) {
        if (x < 2) return false;
        for (int i = 2; i * i <= x; i ++ )
            if (x % i == 0)
                return false;
        return true;
    }

    int primePalindrome(int n) {
        if (n > 7 && n <= 11) return 11;
        for (int i = 1; ;i ++ ) {
            int x = get(i);
            if (x >= n && is_prime(x))
                return x;
        }
        return -1;
    }
};
```
##### LeetCode 867. 转置矩阵
```cpp
class Solution {
public:
    vector<vector<int>> transpose(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<int>> res(m, vector<int>(n));
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                res[j][i] = matrix[i][j];
        return res;
    }
};

```
##### LeetCode 868. 二进制间距
```cpp
class Solution {
public:
    int binaryGap(int n) {
        int res = 0, last = -1;
        for (int i = 0; i < 30; i ++ )
            if (n >> i & 1) {
                if (last != -1) res = max(res, i - last);
                last = i;
            }
        return res;
    }
};
```
##### LeetCode 869. 重新排序得到 2 的幂
```cpp
class Solution {
public:
    bool check(int a, int b) {
        unordered_multiset<char> s1, s2;
        for (auto c: to_string(a)) s1.insert(c);
        for (auto c: to_string(b)) s2.insert(c);
        return s1 == s2;
    }

    bool reorderedPowerOf2(int n) {
        for (int i = 1; i < 1e9; i *= 2)
            if (check(i, n))
                return true;
        return false;
    }
};
```
##### LeetCode 870. 优势洗牌
```cpp
class Solution {
public:
    vector<int> advantageCount(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        int n = nums2.size();
        vector<int> id(n);
        for (int i = 0; i < n; i ++ ) id[i] = i;
        sort(id.begin(), id.end(), [&](int a, int b) {
            return nums2[a] < nums2[b];
        });
        vector<int> res(n);
        int l = 0, r = n - 1;
        for (auto x: nums1) {
            if (x > nums2[id[l]]) res[id[l ++ ]] = x;
            else res[id[r -- ]] = x;
        }
        return res;
    }
};
```



### 究极班——Week 37(第 871 ~ 890 题）

#####  LeetCode  871. 最低加油次数
```C++
class Solution {
public:
    int minRefuelStops(int target, int sum, vector<vector<int>>& stations) {
        stations.push_back({target, 0});
        int res = 0;
        priority_queue<int> heap;
        for (auto& p: stations) {
            int x = p[0], y = p[1];
            while (heap.size() && sum < x) {
                sum += heap.top();
                heap.pop();
                res ++ ;
            }
            if (sum < x) return -1;
            heap.push(y);
        }
        return res;
    }
};

```
#####  LeetCode  872. 叶子相似的树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, vector<int>& a) {
        if (!root) return;
        if (!root->left && !root->right) a.push_back(root->val);
        dfs(root->left, a);
        dfs(root->right, a);
    }

    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
        vector<int> a, b;
        dfs(root1, a);
        dfs(root2, b);
        return a == b;
    }
};
```
#####  LeetCode  873. 最长的斐波那契子序列的长度
```C++
class Solution {
public:
    int lenLongestFibSubseq(vector<int>& arr) {
        int n = arr.size();
        unordered_map<int, int> pos;
        for (int i = 0; i < n; i ++ ) pos[arr[i]] = i;
        vector<vector<int>> f(n, vector<int>(n));
        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < i; j ++ ) {
                int x = arr[i] - arr[j];
                f[i][j] = 2;
                if (x < arr[j] && pos.count(x)) {
                    int k = pos[x];
                    f[i][j] = max(f[i][j], f[j][k] + 1);
                }
                res = max(res, f[i][j]);
            }
        if (res < 3) res = 0;
        return res;
    }
};

```
#####  LeetCode  874. 模拟行走机器人
```C++
class Solution {
public:
    string get(int x, int y) {
        return to_string(x) + '#' + to_string(y);
    }

    int robotSim(vector<int>& commands, vector<vector<int>>& obstacles) {
        unordered_set<string> S;
        for (auto& p: obstacles) S.insert(get(p[0], p[1]));
        int x = 0, y = 0, d = 0;
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        int res = 0;
        for (auto c: commands) {
            if (c == -2) d = (d + 3) % 4;
            else if (c == -1) d = (d + 1) % 4;
            else {
                for (int i = 0; i < c; i ++ ) {
                    int a = x + dx[d], b = y + dy[d];
                    if (S.count(get(a, b))) break;
                    x = a, y = b;
                    res = max(res, x * x + y * y);
                }
            }
        }
        return res;
    }
};
```
#####  LeetCode  875. 爱吃香蕉的珂珂
```C++
class Solution {
public:
    int get(vector<int>& piles, int mid) {
        int res = 0;
        for (auto x: piles)
            res += (x + mid - 1) / mid;
        return res;
    }

    int minEatingSpeed(vector<int>& piles, int h) {
        int l = 1, r = 1e9;
        while (l < r) {
            int mid = l + r >> 1;
            if (get(piles, mid) <= h) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
#####  LeetCode  876. 链表的中间结点
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        auto p = head, q = head;
        while (q && q->next) {
            p = p->next;
            q = q->next->next;
        }
        return p;
    }
};
```
#####  LeetCode  877. 石子游戏
```C++
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int n = piles.size();
        vector<vector<int>> f(n, vector<int>(n));
        for (int len = 1; len <= n; len ++ )
            for (int i = 0; i + len - 1 < n; i ++ ) {
                int j = i + len - 1;
                if (len == 1) f[i][j] = piles[i];
                else {
                    f[i][j] = max(piles[i] - f[i + 1][j], piles[j] - f[i][j - 1]);
                }
            }
        return f[0][n - 1] > 0;
    }
};
```
#####  LeetCode  878. 第 N 个神奇数字
```C++
typedef long long LL;

class Solution {
public:
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }

    LL get(LL mid, int a, int b) {
        return mid / a + mid / b - mid / (a * b / gcd(a, b));
    }

    int nthMagicalNumber(int n, int a, int b) {
        const int MOD = 1e9 + 7;
        LL l = 1, r = 4e13;
        while (l < r) {
            LL mid = l + r >> 1;
            if (get(mid, a, b) >= n) r = mid;
            else l = mid + 1;
        }
        return r % MOD;
    }
};
```
#####  LeetCode  879. 盈利计划
```C++
class Solution {
public:
    int profitableSchemes(int n, int m, vector<int>& group, vector<int>& profit) {
        const int MOD = 1e9 + 7;
        vector<vector<int>> f(n + 1, vector<int>(m + 1));
        for (int i = 0; i <= n; i ++ ) f[i][0] = 1;
        for (int i = 0; i < group.size(); i ++ ) {
            int g = group[i], p = profit[i];
            for (int j = n; j >= g; j -- )
                for (int k = m; k >= 0; k -- )
                    f[j][k] = (f[j][k] + f[j - g][max(0, k - p)]) % MOD;
        }
        return f[n][m];
    }
};

```
#####  LeetCode  880. 索引处的解码字符串
```C++
class Solution {
public:
    string decodeAtIndex(string s, int k) {
        typedef long long LL;
        LL n = 0;
        for (auto c: s)
            if (isdigit(c)) n *= c - '0';
            else n ++ ;

        for (int i = s.size() - 1; i >= 0; i -- ) {
            char c = s[i];
            if (isdigit(c)) {
                int x = c - '0';
                n /= x;
                k %= n;
                if (!k) k = n;
            } else {
                if (n == k) return string(1, c);
                n -- ;
            }
        }
        return "";
    }
};

```


#####  LeetCode 881. 救生艇
```cpp
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(), people.end());
        int res = 0;
        for (int i = 0, j = people.size() - 1; i <= j; j -- ) {
            if (people[j] + people[i] <= limit) i ++ ;
            res ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 882. 细分图中的可到达结点
```cpp
const int N = 3010, M = 20010, INF = 0x3f3f3f3f;

int h[N], e[M], w[M], ne[M], idx;
int dist[N], q[N];
bool st[N];

class Solution {
public:
    void add(int a, int b, int c) {
        e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
    }

    void spfa() {
        memset(dist, 0x3f, sizeof dist);
        dist[0] = 0;
        int hh = 0, tt = 1;
        q[0] = 0;
        while (hh != tt) {
            int t = q[hh ++ ];
            if (hh == N) hh = 0;
            st[t] = false;

            for (int i = h[t]; ~i; i = ne[i]) {
                int j = e[i];
                if (dist[j] > dist[t] + w[i]) {
                    dist[j] = dist[t] + w[i];
                    if (!st[j]) {
                        q[tt ++ ] = j;
                        if (tt == N) tt = 0;
                        st[j] = true;
                    }
                }
            }
        }
    }

    int reachableNodes(vector<vector<int>>& edges, int maxMoves, int n) {
        memset(h, -1, sizeof h);
        idx = 0;
        for (auto& e: edges) {
            int a = e[0], b = e[1], c = e[2];
            add(a, b, c + 1), add(b, a, c + 1);
        }
        spfa();

        int res = 0;
        for (int i = 0; i < n; i ++ )
            if (dist[i] <= maxMoves)
                res ++ ;

        for (auto& e: edges) {
            int a = e[0], b = e[1], c = e[2];
            int x = max(0, maxMoves - dist[a]), y = max(0, maxMoves - dist[b]);
            res += min(x + y, c);
        }
        return res;
    }
};
```
#####  LeetCode 883. 三维形体投影面积
```cpp
class Solution {
public:
    int projectionArea(vector<vector<int>>& grid) {
        int n = grid.size();
        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            int r = 0, c = 0;
            for (int j = 0; j < n; j ++ ) {
                if (grid[i][j]) res ++ ;
                r = max(r, grid[i][j]);
                c = max(c, grid[j][i]);
            }
            res += r + c;
        }
        return res;
    }
};
```
#####  LeetCode 884. 两句话中的不常见单词
```cpp
class Solution {
public:
    void work(string& s, unordered_map<string, int>& c) {
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == ' ') continue;
            int j = i + 1;
            while (j < s.size() && s[j] != ' ') j ++ ;
            c[s.substr(i, j - i)] ++ ;
            i = j;
        }
    }

    vector<string> uncommonFromSentences(string s1, string s2) {
        unordered_map<string, int> c1, c2;
        work(s1, c1);
        work(s2, c2);
        vector<string> res;
        for (auto& [k, v]: c1)
            if (v == 1 && !c2.count(k))
                res.push_back(k);
        for (auto& [k, v]: c2)
            if (v == 1 && !c1.count(k))
                res.push_back(k);
        return res;
    }
};
```
#####  LeetCode 885. 螺旋矩阵 III
```cpp
class Solution {
public:
    vector<vector<int>> spiralMatrixIII(int rows, int cols, int x, int y) {
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        vector<vector<int>> res;
        res.push_back({x, y});
        int d = 0, tot = rows * cols;
        for (int k = 1; res.size() < tot; k ++ )
            for (int i = 0; i < 2 && res.size() < tot; i ++ ) {
                for (int j = 0; j < k && res.size() < tot; j ++ ) {
                    int a = x + dx[d], b = y + dy[d];
                    if (a >= 0 && a < rows && b >= 0 && b < cols)
                        res.push_back({a, b});
                    x = a, y = b;
                }
                d = (d + 1) % 4;
            }
        return res;
    }
};
```
#####  LeetCode 886. 可能的二分法
```cpp
class Solution {
public:
    vector<vector<int>> g;
    vector<int> color;

    bool dfs(int u, int c) {
        color[u] = c;
        for (int v: g[u])
            if (color[v]) {
                if (color[v] == c) return false;
            } else if (!dfs(v, 3 - c)) {
                return false;
            }
        return true;
    }

    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        g.resize(n);
        color.resize(n);
        for (auto& e: dislikes) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].push_back(b), g[b].push_back(a);
        }
        for (int i = 0; i < n; i ++ )
            if (!color[i] && !dfs(i, 1))
                return false;
        return true;
    }
};
```
#####  LeetCode 887. 鸡蛋掉落
```cpp
int f[10010][110];

class Solution {
public:
    int superEggDrop(int k, int n) {
        for (int i = 1; i <= n; i ++ ) {
            for (int j = 1; j <= k; j ++ )
                f[i][j] = f[i - 1][j - 1] + 1 + f[i - 1][j];
            if (f[i][k] >= n) return i;
        }
        return -1;
    }
};
```
#####  LeetCode 888. 公平的糖果棒交换
```cpp
class Solution {
public:
    vector<int> fairCandySwap(vector<int>& a, vector<int>& b) {
        int s1 = accumulate(a.begin(), a.end(), 0);
        int s2 = accumulate(b.begin(), b.end(), 0);
        unordered_set<int> S;
        for (auto x: b) S.insert(x);
        int t = (s1 - s2) / 2;
        for (auto x: a)
            if (S.count(x - t))
                return {x, x - t};
        return {};
    }
};
```
#####  LeetCode 889. 根据前序和后序遍历构造二叉树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> pos;

    TreeNode* build(vector<int>& pre, vector<int>& post, int a, int b, int x, int y) {
         if (a > b) return NULL;
         auto root = new TreeNode(pre[a]);
         if (a == b) return root;
         int k = pos[pre[a + 1]];
         root->left = build(pre, post, a + 1, a + 1 + k - x, x, k);
         root->right = build(pre, post, a + 1 + k - x + 1, b, k + 1, y - 1);
         return root;
    }

    TreeNode* constructFromPrePost(vector<int>& pre, vector<int>& post) {
        int n = pre.size();
        for (int i = 0; i < n; i ++ ) pos[post[i]] = i;
        return build(pre, post, 0, n - 1, 0, n - 1);
    }
};
```
#####  LeetCode 890. 查找和替换模式
```cpp
class Solution {
public:
    bool check(string& a, string& b) {
        unordered_map<char, char> f, g;
        for (int i = 0; i < a.size(); i ++ ) {
            if (f.count(a[i]) && f[a[i]] != b[i]) return false;
            if (g.count(b[i]) && g[b[i]] != a[i]) return false;
            f[a[i]] = b[i], g[b[i]] = a[i];
        }
        return true;
    }

    vector<string> findAndReplacePattern(vector<string>& words, string pattern) {
        vector<string> res;
        for (auto& w: words)
            if (check(w, pattern))
                res.push_back(w);
        return res;
    }
};
```





### 究极班- Week 38（第 891 ~ 900 题）

##### LeetCode 891. 子序列宽度之和
```cpp
typedef long long LL;
const int N = 20010, MOD = 1e9 + 7;

int p[N];

class Solution {
public:
    int sumSubseqWidths(vector<int>& a) {
        int n = a.size();
        p[0] = 1;
        for (int i = 1; i <= n; i ++ ) p[i] = p[i - 1] * 2 % MOD;
        int res = 0;
        sort(a.begin(), a.end());
        for (int i = 0; i < n; i ++ )
            res = (res + (LL)a[i] * p[i] - (LL)a[i] * p[n - i - 1]) % MOD;
        return res;
    }
};
```
##### LeetCode 892. 三维形体的表面积
```cpp
class Solution {
public:
    int surfaceArea(vector<vector<int>>& grid) {
        int n = grid.size();
        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            for (int j = 1; j < n; j ++ ) {
                res += abs(grid[i][j] - grid[i][j - 1]);
                res += abs(grid[j][i] - grid[j - 1][i]);
                if (grid[i][j]) res += 2;
            }
            res += grid[i][0] + grid[i][n - 1];
            res += grid[0][i] + grid[n - 1][i];
            if (grid[i][0]) res += 2;
        }
        return res;
    }
};
```
##### LeetCode 893. 特殊等价字符串组
```cpp
class Solution {
public:
    int numSpecialEquivGroups(vector<string>& words) {
        unordered_set<string> S;
        for (auto& w: words) {
            string a, b;
            for (int i = 0; i < w.size(); i += 2) {
                a += w[i];
                b += w[i + 1];
            }
            sort(a.begin(), a.end());
            sort(b.begin(), b.end());
            S.insert(a + b);
        }
        return S.size();
    }
};
```
##### LeetCode 894. 所有可能的满二叉树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<TreeNode*> allPossibleFBT(int n) {
        if (n % 2 == 0) return {};
        vector<TreeNode*> res;
        if (n == 1) res.push_back(new TreeNode());
        for (int i = 1; i + 1 < n; i += 2) {
            auto left = allPossibleFBT(i), right = allPossibleFBT(n - i - 1);
            for (auto l: left)
                for (auto r: right)
                    res.push_back(new TreeNode(0, l, r));
        }
        return res;
    }
};
```
##### LeetCode 895. 最大频率栈
```cpp
class FreqStack {
public:
    unordered_map<int, stack<int>> stk;
    unordered_map<int, int> cnt;
    int n = 0;

    FreqStack() {

    }

    void push(int val) {
        stk[ ++ cnt[val]].push(val);
        n = max(n, cnt[val]);
    }

    int pop() {
        int t = stk[n].top();
        stk[n].pop();
        cnt[t] -- ;
        if (stk[n].empty()) n -- ;
        return t;
    }
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(val);
 * int param_2 = obj->pop();
 */
```
##### LeetCode 896. 单调数列
```cpp
class Solution {
public:
    bool isMonotonic(vector<int>& nums) {
        bool x = true, y = true;
        for (int i = 1; i < nums.size(); i ++ ) {
            if (nums[i - 1] > nums[i]) x = false;
            if (nums[i - 1] < nums[i]) y = false;
        }
        return x || y;
    }
};
```
##### LeetCode 897. 递增顺序搜索树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* tail;

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);

        tail = tail->right = root;
        root->left = NULL;

        dfs(root->right);
    }

    TreeNode* increasingBST(TreeNode* root) {
        auto dummy = new TreeNode();
        tail = dummy;
        dfs(root);
        return dummy->right;
    }
};
```
##### LeetCode 898. 子数组按位或操作
```cpp
class Solution {
public:
    int subarrayBitwiseORs(vector<int>& arr) {
        unordered_set<int> res, f, g;
        for (auto x: arr) {
            g.insert(x);
            for (auto y: f) g.insert(x | y);
            for (auto y: g) res.insert(y);
            f = g, g.clear();
        }
        return res.size();
    }
};
```
##### LeetCode 899. 有序队列
```cpp
class Solution {
public:
    string orderlyQueue(string s, int k) {
        if (k == 1) {
            string res = s;
            for (int i = 0; i < s.size(); i ++ ) {
                s = s.substr(1) + s[0];
                res = min(res, s);
            }
            return res;
        }

        sort(s.begin(), s.end());
        return s;
    }
};
```
##### LeetCode 900. RLE 迭代器
```cpp
class RLEIterator {
public:
    int k = 0;
    vector<int> q;

    RLEIterator(vector<int>& encoding) {
        q = encoding;
    }

    int next(int n) {
        while (k < q.size() && n > q[k]) {
            n -= q[k];
            k += 2;
        }
        if (k >= q.size()) return -1;
        q[k] -= n;
        return q[k + 1];
    }
};

/**
 * Your RLEIterator object will be instantiated and called as such:
 * RLEIterator* obj = new RLEIterator(encoding);
 * int param_1 = obj->next(n);
 */
```




### 究极班- Week 39(第 901 ~ 920 题)

##### LeetCode 901. 股票价格跨度
```cpp
class StockSpanner {
public:
    int k = 0;
    stack<int> day, price;

    StockSpanner() {
        day.push(-1);
        price.push(1e6);
    }

    int next(int x) {
        while (price.top() <= x) {
            day.pop();
            price.pop();
        }
        int res = k - day.top();
        day.push(k ++ );
        price.push(x);
        return res;
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```
##### LeetCode 902. 最大为 N 的数字组合
```cpp
class Solution {
public:
    int power(int a, int b) {
        int res = 1;
        while (b -- ) res *= a;
        return res;
    }

    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string num = to_string(n);
        reverse(num.begin(), num.end());

        int res = 0;
        for (int i = 1; i < num.size(); i ++ ) res += power(digits.size(), i);

        bool flag = true;
        for (int i = num.size() - 1; i >= 0; i -- ) {
            int x = num[i] - '0', t = power(digits.size(), i);
            int j;
            for (j = 0; j < digits.size(); j ++ )
                if (digits[j][0] - '0' < x)
                    res += t;
                else break;

            if (j < digits.size() && digits[j][0] - '0' == x) continue;
            flag = false;
            break;
        }

        if (flag) res ++ ;
        return res;
    }
};
```
##### LeetCode 903. DI 序列的有效排列
```cpp
class Solution {
public:
    int numPermsDISequence(string s) {
        int n = s.size(), MOD = 1e9 + 7;
        vector<vector<int>> f(n + 1, vector<int>(n + 1));
        f[0][0] = 1;
        for (int i = 1; i <= n; i ++ ) {
            if (s[i - 1] == 'D') {
                for (int j = i - 1; j >= 0; j -- )
                    f[i][j] = (f[i - 1][j] + f[i][j + 1]) % MOD;
            } else {
                for (int j = 1; j <= i; j ++ )
                    f[i][j] = (f[i - 1][j - 1] + f[i][j - 1]) % MOD;
            }
        }
        int res = 0;
        for (int i = 0; i <= n; i ++ )
            res = (res + f[n][i]) % MOD;
        return res;
    }
};
```


##### LeetCode 904. 水果成篮
```cpp
class Solution {
public:
    int totalFruit(vector<int>& tree) {
        int res = 0;
        unordered_map<int, int> cnt;
        for (int i = 0, j = 0, s = 0; i < tree.size(); i ++ ) {
            if ( ++ cnt[tree[i]] == 1) s ++ ;
            while (s > 2) {
                if ( -- cnt[tree[j]] == 0) s -- ;
                j ++ ;
            }
            res = max(res, i - j + 1);
        }
        return res;
    }
};

```
##### LeetCode 905. 按奇偶排序数组
```cpp
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        int i = 0, j = nums.size() - 1;
        while (i < j) {
            while (i < j && nums[i] % 2 == 0) i ++ ;
            while (i < j && nums[j] % 2 == 1) j -- ;
            if (i < j) swap(nums[i], nums[j]);
        }
        return nums;
    }
};
```
##### LeetCode 906. 超级回文数
```cpp
typedef long long LL;

class Solution {
public:
    bool check(string s, string left, string right) {
        auto x = stoll(s), l = stoll(left), r = stoll(right);
        if (x > INT_MAX) return false;

        x *= x;
        if (x < l || x > r) return false;

        s = to_string(x);
        for (int i = 0, j = s.size() - 1; i < j; i ++, j -- )
            if (s[i] != s[j])
                return false;

        return true;
    }

    int superpalindromesInRange(string left, string right) {
        int res = 0;
        for (int i = 1; i <= 20001; i ++ ) {
            auto a = to_string(i);
            string b(a.rbegin(), a.rend());
            if (check(a + b, left, right)) res ++ ;
            if (check(a + b.substr(1), left, right)) res ++ ;
        }
        return res;
    }
};
```
##### LeetCode 907. 子数组的最小值之和
```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& w) {
        int n = w.size();
        vector<int> l(n), r(n);
        stack<int> stk;

        for (int i = 0; i < n; i ++ ) {
            while (stk.size() && w[stk.top()] > w[i]) stk.pop();
            if (stk.empty()) l[i] = -1;
            else l[i] = stk.top();
            stk.push(i);
        }

        stk = stack<int>();
        for (int i = n - 1; i >= 0; i -- ) {
            while (stk.size() && w[stk.top()] >= w[i]) stk.pop();
            if (stk.empty()) r[i] = n;
            else r[i] = stk.top();
            stk.push(i);
        }

        typedef long long LL;
        const int MOD = 1e9 + 7;
        int res = 0;
        for (int i = 0; i < n; i ++ )
            res = (res + (LL)w[i] * (i - l[i]) * (r[i] - i)) % MOD;
        return res;
    }
};
```
##### LeetCode 908. 最小差值 I
```cpp
class Solution {
public:
    int smallestRangeI(vector<int>& nums, int k) {
        int minw = INT_MAX, maxw = INT_MIN;
        for (auto x: nums) {
            minw = min(minw, x);
            maxw = max(maxw, x);
        }
        return max(0, maxw - minw - k * 2);
    }
};
```
##### LeetCode 909. 蛇梯棋
```cpp
#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    vector<vector<int>> id;
    vector<PII> cor;

    int snakesAndLadders(vector<vector<int>>& board) {
        int n = board.size(), m = board[0].size();
        id = vector<vector<int>>(n, vector<int>(m));
        cor = vector<PII>(n * m + 1);
        for (int i = n - 1, k = 1, s = 0; i >= 0; i --, s ++ ) {
            if (s % 2 == 0) {
                for (int j = 0; j < m; j ++, k ++ ) {
                    id[i][j] = k;
                    cor[k] = {i, j};
                }
            } else {
                for (int j = m - 1; j >= 0; j --, k ++ ) {
                    id[i][j] = k;
                    cor[k] = {i, j};
                }
            }
        }

        queue<PII> q;
        vector<vector<int>> dist(n, vector<int>(m, 1e9));
        q.push({n - 1, 0});
        dist[n - 1][0] = 0;
        while (q.size()) {
            auto t = q.front();
            q.pop();

            int k = id[t.x][t.y];
            if (k == n * m) return dist[t.x][t.y];
            for (int i = k + 1; i <= k + 6 && i <= n * m; i ++ ) {
                int x = cor[i].x, y = cor[i].y;
                if (board[x][y] == -1) {
                    if (dist[x][y] > dist[t.x][t.y] + 1) {
                        dist[x][y] = dist[t.x][t.y] + 1;
                        q.push({x, y});
                    }
                } else {
                    int r = board[x][y];
                    x = cor[r].x, y = cor[r].y;
                    if (dist[x][y] > dist[t.x][t.y] + 1) {
                        dist[x][y] = dist[t.x][t.y] + 1;
                        q.push({x, y});
                    }
                }
            }
        }
        return -1;
    }
};
```
##### LeetCode 910. 最小差值 II
```cpp
class Solution {
public:
    int smallestRangeII(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int res = nums.back() - nums[0];
        for (int i = 0; i + 1 < nums.size(); i ++ ) {
            int maxw = max(nums[i] + k, nums.back() - k);
            int minw = min(nums[0] + k, nums[i + 1] - k);
            res = min(res, maxw - minw);
        }
        return res;
    }
};
```
##### LeetCode 911. 在线选举
```cpp
class TopVotedCandidate {
public:
    vector<int> win;
    vector<int> times;

    TopVotedCandidate(vector<int>& persons, vector<int>& _times) {
        int n = persons.size();
        win.resize(n);
        times = _times;
        vector<int> sum(n + 1);

        int maxc = 0, maxp;
        for (int i = 0; i < n; i ++ ) {
            int p = persons[i];
            if ( ++ sum[p] >= maxc) {
                maxc = sum[p];
                maxp = p;
            }
            win[i] = maxp;
        }
    }

    int q(int t) {
        int k = upper_bound(times.begin(), times.end(), t) - times.begin() - 1;
        return win[k];
    }
};

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate* obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj->q(t);
 */

```
##### LeetCode 912. 排序数组
```cpp
class Solution {
public:
    void quick_sort(vector<int>& q, int l, int r) {
        if (l >= r) return;
        int i = l - 1, j = r + 1, x = q[l + r >> 1];
        while (i < j) {
            do i ++ ; while (q[i] < x);
            do j -- ; while (q[j] > x);
            if (i < j) swap(q[i], q[j]);
        }
        quick_sort(q, l, j);
        quick_sort(q, j + 1, r);
    }

    vector<int> sortArray(vector<int>& nums) {
        quick_sort(nums, 0, nums.size() - 1);
        return nums;
    }
};
```
##### LeetCode 913. 猫和老鼠
```cpp
const int N = 210;

int f[2 * N][N][N];

class Solution {
public:
    int n;
    vector<vector<int>> g;

    int dp(int k, int i, int j) {
        int& v = f[k][i][j];
        if (v != -1) return v;
        if (k > n * 2) return v = 0;
        if (!i) return v = 1;
        if (i == j) return v = 2;

        if (k % 2 == 0) {  // 该老鼠走了
            int draws = 0;
            for (int x: g[i]) {
                int t = dp(k + 1, x, j);
                if (t == 1) return v = 1;
                if (!t) draws ++ ;
            }
            if (draws) return v = 0;
            return v = 2;
        } else {  // 该猫走了
            int draws = 0;
            for (int x: g[j]) {
                if (!x) continue;
                int t = dp(k + 1, i, x);
                if (t == 2) return v = 2;
                if (!t) draws ++ ;
            }
            if (draws) return v = 0;
            return 1;
        }
    }

    int catMouseGame(vector<vector<int>>& graph) {
        g = graph;
        n = g.size();
        memset(f, -1, sizeof f);
        return dp(0, 1, 2);
    }
};
```
##### LeetCode 914. 卡牌分组
```cpp
class Solution {
public:
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }

    bool hasGroupsSizeX(vector<int>& deck) {
        unordered_map<int, int> cnt;
        for (auto x: deck) cnt[x] ++ ;
        int d = 0;
        for (auto [k, v]: cnt) d = gcd(d, v);
        return d >= 2;
    }
};
```
##### LeetCode 915. 分割数组
```cpp
class Solution {
public:
    int partitionDisjoint(vector<int>& nums) {
        int n = nums.size();
        vector<int> r(n, nums.back());
        for (int i = n - 2; i >= 0; i -- )
            r[i] = min(nums[i], r[i + 1]);

        for (int i = 0, l = INT_MIN; i + 1 < n; i ++ ) {
            l = max(l, nums[i]);
            if (l <= r[i + 1]) return i + 1;
        }
        return -1;
    }
};
```
##### LeetCode 916. 单词子集
```cpp
class Solution {
public:
    vector<string> wordSubsets(vector<string>& words1, vector<string>& words2) {
        unordered_map<char, int> max_cnt;
        for (auto& w: words2) {
            unordered_map<char, int> cnt;
            for (auto c: w) cnt[c] ++ ;
            for (auto [k, v]: cnt)
                max_cnt[k] = max(max_cnt[k], v);
        }

        vector<string> res;
        for (auto& w: words1) {
            unordered_map<char, int> cnt;
            for (auto c: w) cnt[c] ++ ;
            bool flag = true;
            for (auto [k, v]: max_cnt)
                if (cnt[k] < v) {
                    flag = false;
                    break;
                }
            if (flag) res.push_back(w);
        }
        return res;
    }
};

```
##### LeetCode 917. 仅仅反转字母
```cpp
class Solution {
public:
    string reverseOnlyLetters(string s) {
        for (int i = 0, j = s.size() - 1; i < j; i ++, j -- ) {
            while (i < j && !isalpha(s[i])) i ++ ;
            while (i < j && !isalpha(s[j])) j -- ;
            if (i < j) swap(s[i], s[j]);
        }
        return s;
    }
};
```
##### LeetCode 918. 环形子数组的最大和
```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int n = nums.size();
        vector<int> s(n * 2 + 1);
        for (int i = 1; i <= n * 2; i ++ )
            s[i] = s[i - 1] + nums[(i - 1) % n];

        deque<int> q;
        q.push_front(0);
        int res = INT_MIN;
        for (int i = 1; i <= n * 2; i ++ ) {
            while (q.size() && i - q.front() > n) q.pop_front();
            res = max(res, s[i] - s[q.front()]);
            while (q.size() && s[q.back()] >= s[i]) q.pop_back();
            q.push_back(i);
        }
        return res;
    }
};
```
##### LeetCode 919. 完全二叉树插入器
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class CBTInserter {
public:
    TreeNode* R;
    vector<TreeNode*> h;

    CBTInserter(TreeNode* root) {
        h.resize(1);
        R = root;
        queue<TreeNode*> q;
        q.push(root);
        while (q.size()) {
            auto t = q.front();
            q.pop();
            h.push_back(t);
            if (t->left) q.push(t->left);
            if (t->right) q.push(t->right);
        }
    }

    int insert(int v) {
        auto t = new TreeNode(v);
        h.push_back(t);
        int k = h.size() - 1;
        int p = k / 2;
        if (p * 2 == k) h[p]->left = t;
        else h[p]->right = t;
        return h[p]->val;
    }

    TreeNode* get_root() {
        return R;
    }
};

/**
 * Your CBTInserter object will be instantiated and called as such:
 * CBTInserter* obj = new CBTInserter(root);
 * int param_1 = obj->insert(v);
 * TreeNode* param_2 = obj->get_root();
 */

```
##### LeetCode 920. 播放列表的数量
```cpp
typedef long long LL;
const int N = 210, MOD = 1e9 + 7;

int f[N][N];

class Solution {
public:
    int numMusicPlaylists(int n, int m, int k) {
        f[0][0] = 1;
        for (int i = 1; i <= m; i ++ )
            for (int j = 1; j <= n && j <= i; j ++ )
                f[i][j] = ((LL)f[i - 1][j - 1] * (n - j + 1)
                    + (LL)f[i - 1][j] * max(j - k, 0)) % MOD;
        return f[m][n];
    }
};
```



### 究极班——Week 40(第921-930题）

#####  LeetCode  921. 使括号有效的最少添加
```C++
class Solution {
public:
    int minAddToMakeValid(string s) {
        int l = 0, r = 0;
        for  (auto c: s) {
            if (c == '(') l ++ ;
            else {
                if (!l) r ++ ;
                else l -- ;
            }
        }
        return l + r;
    }
};
```
#####  LeetCode  922. 按奇偶排序数组 II
```C++
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& nums) {
        for (int i = 0, j = 1; i < nums.size(); i += 2, j += 2) {
            while (i < nums.size() && nums[i] % 2 == 0) i += 2;
            while (j < nums.size() && nums[j] % 2 == 1) j += 2;
            if (i < nums.size()) swap(nums[i], nums[j]);
        }
        return nums;
    }
};
```
#####  LeetCode  923. 三数之和的多种可能
```C++
typedef long long LL;
const int MOD = 1e9 + 7;

class Solution {
public:
    int cnt[310];

    LL C(int a, int b) {
        LL res = 1;
        for (int i = a, j = 1; j <= b; i --, j ++ )
            res = res * i / j;
        return res % MOD;
    }

    int calc(int a, int b, int c) {
        if (a == b && b == c) return C(cnt[a], 3);
        if (a == b) return C(cnt[a], 2) * cnt[c] % MOD;
        if (b == c) return cnt[a] * C(cnt[b], 2) % MOD;
        return (LL)cnt[a] * cnt[b] * cnt[c] % MOD;
    }

    int threeSumMulti(vector<int>& arr, int target) {
        for (auto x: arr) cnt[x] ++ ;
        int res = 0;
        for (int i = 0; i <= target; i ++ )
            for (int j = i; j <= target - i - j; j ++ )
                res = (res + calc(i, j, target - i - j)) % MOD;
        return res;
    }
};

```
#####  LeetCode  924. 尽量减少恶意软件的传播
```C++
class Solution {
public:
    vector<int> p, s, c;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int minMalwareSpread(vector<vector<int>>& graph, vector<int>& initial) {
        int n = graph.size();
        for (int i = 0; i < n; i ++ ) {
            p.push_back(i);
            s.push_back(1);
            c.push_back(0);
        }
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                if (graph[i][j] && find(i) != find(j)) {
                    s[find(i)] += s[find(j)];
                    p[find(j)] = find(i);
                }

        for (auto x: initial) c[find(x)] ++ ;

        int rs = -1, rp = INT_MAX;
        for (auto x: initial) {
            if (rs == -1) rp = min(rp, x);
            if (c[find(x)] == 1) {
                if (rs < s[find(x)]) {
                    rs = s[find(x)];
                    rp = x;
                } else if (rs == s[find(x)]) {
                    rp = min(rp, x);
                }
            }
        }
        return rp;
    }
};
```
#####  LeetCode  925. 长按键入
```C++
class Solution {
public:
    bool isLongPressedName(string name, string typed) {
        int i = 0, j = 0;
        while (i < name.size() && j < typed.size()) {
            if (name[i] != typed[j]) return false;
            int x = i + 1, y = j + 1;
            while (x < name.size() && name[x] == name[i]) x ++ ;
            while (y < typed.size() && typed[y] == typed[j]) y ++ ;
            if (x - i > y - j) return false;
            i = x, j = y;
        }
        return i == name.size() && j == typed.size();
    }
};

```
#####  LeetCode  926. 将字符串翻转到单调递增
```C++
class Solution {
public:
    int minFlipsMonoIncr(string str) {
        int n = str.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ )
            s[i] = s[i - 1] + str[i - 1] - '0';

        int res = n - s[n];
        for (int i = 1; i <= n; i ++ )
            res = min(res, s[i] + n - i - (s[n] - s[i]));
        return res;
    }
};

```
#####  LeetCode  927. 三等分
```C++
class Solution {
public:
    bool check(vector<int>& arr, int a, int b, int c, int d) {
        for (int i = a, j = c; i <= b; i ++, j ++ )
            if (arr[i] != arr[j])
                return false;
        return true;
    }

    vector<int> threeEqualParts(vector<int>& arr) {
        int sum = accumulate(arr.begin(), arr.end(), 0);
        if (!sum) return {0, 2};
        if (sum % 3) return {-1, -1};
        int avg = sum / 3;
        int s[6] = {1, avg, avg + 1, 2 * avg, 2 * avg + 1, 3 * avg};
        int p[6];
        int n = arr.size();
        for (int i = 0, j = 0, c = 0; i < n; i ++ ) {
            if (!arr[i]) continue;
            c ++ ;
            while (j < 6 && s[j] == c) p[j ++ ] = i;
        }

        int zero = n - 1 - p[5];
        if (p[4] - p[3] - 1 < zero || p[2] - p[1] - 1 < zero) return {-1, -1};
        if (!check(arr, p[0], p[1] + zero, p[2], p[3] + zero)) return {-1, -1};
        if (!check(arr, p[2], p[3] + zero, p[4], n - 1)) return {-1, -1};
        return {p[1] + zero, p[3] + zero + 1};
    }
};

```
#####  LeetCode  928. 尽量减少恶意软件的传播 II
```C++
class Solution {
public:
    vector<int> p, s;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int minMalwareSpread(vector<vector<int>>& graph, vector<int>& initial) {
        int n = graph.size();
        p.resize(n), s.resize(n);
        for (int i = 0; i < n; i ++ ) p[i] = i, s[i] = 1;
        unordered_set<int> S(initial.begin(), initial.end());

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                if (!S.count(i) && !S.count(j) && graph[i][j] && find(i) != find(j)) {
                    s[find(i)] += s[find(j)];
                    p[find(j)] = find(i);
                }

        vector<unordered_set<int>> cnt(n);
        for (auto x: initial)
            for (int i = 0; i < n; i ++ )
                if (!S.count(i) && graph[x][i])
                    cnt[find(i)].insert(x);

        vector<int> sum(n);
        unordered_set<int> comp;
        for (int i = 0; i < n; i ++ )
            if (!S.count(i))
                comp.insert(find(i));
        for (auto x: comp)
            if (cnt[x].size() == 1)
                sum[*cnt[x].begin()] += s[x];

        int res = initial[0];
        for (auto x: initial)
            if (sum[x] > sum[res] || sum[x] == sum[res] && x < res)
                res = x;
        return res;
    }
};

```
#####  LeetCode  929. 独特的电子邮件地址
```C++
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> S;
        for (auto& s: emails) {
            int k = s.find('@');
            auto a = s.substr(0, k), b = s.substr(k + 1);
            string c;
            for (auto x: a)
                if (x == '+') break;
                else if (x != '.') c += x;
            S.insert(c + '@' + b);
        }
        return S.size();
    }
};
```
#####  LeetCode  930. 和相同的二元子数组
```C++
class Solution {
public:
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        int n = nums.size();
        unordered_map<int, int> cnt;
        cnt[0] ++ ;

        int res = 0;
        for (int i = 1, s = 0; i <= n; i ++ ) {
            s += nums[i - 1];
            res += cnt[s - goal];
            cnt[s] ++ ;
        }
        return res;
    }
};
```



### 究极班- Week 41(第 931 ~ 950 题)

##### LeetCode 931. 下降路径最小和
```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<int>> f(n, vector<int>(m, INT_MAX));
        for (int i = 0; i < m; i ++ ) f[0][i] = matrix[0][i];
        for (int i = 1; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                for (int k = max(0, j - 1); k <= min(m - 1, j + 1); k ++ )
                    f[i][j] = min(f[i][j], f[i - 1][k] + matrix[i][j]);
        int res = INT_MAX;
        for (int i = 0; i < m; i ++ )
            res = min(res, f[n - 1][i]);
        return res;
    }
};
```

##### LeetCode 932. 漂亮数组
```cpp
class Solution {
public:
    vector<int> beautifulArray(int n) {
        if (n == 1) return {1};
        auto left = beautifulArray((n + 1) / 2);
        auto right = beautifulArray(n / 2);
        vector<int> res;
        for (auto x: left) res.push_back(x * 2 - 1);
        for (auto x: right) res.push_back(x * 2);
        return res;
    }
};
```

##### LeetCode 933. 最近的请求次数
```cpp
class RecentCounter {
public:
    queue<int> q;

    RecentCounter() {

    }

    int ping(int t) {
        q.push(t);
        while (t - q.front() > 3000) q.pop();
        return q.size();
    }
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
 */

```

##### LeetCode 934. 最短的桥
```cpp
#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    int n, m;
    vector<vector<int>> g, dist;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    queue<PII> q;

    void dfs(int x, int y) {
        g[x][y] = 0;
        dist[x][y] = 0;
        q.push({x, y});
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && g[a][b])
                dfs(a, b);
        }
    }

    int bfs() {
        while (q.size()) {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < 4; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m && dist[x][y] > dist[t.x][t.y] + 1) {
                    dist[x][y] = dist[t.x][t.y] + 1;
                    if (g[x][y]) return dist[x][y] - 1;
                    q.push({x, y});
                }
            }
        }
        return -1;
    }

    int shortestBridge(vector<vector<int>>& grid) {
        g = grid;
        n = g.size(), m = g[0].size();
        dist = vector<vector<int>>(n, vector<int>(m, 1e8));

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j]) {
                    dfs(i, j);
                    return bfs();
                }
        return -1;
    }
};
```
##### LeetCode 935. 骑士拨号器
```cpp
class Solution {
public:
    int knightDialer(int n) {
        vector<vector<int>> f(n, vector<int>(10, 0));
        for (int i = 0; i < 10; i ++ ) f[0][i] = 1;
        vector<int> tr[10] = {
            {4, 6},
            {6, 8},
            {7, 9},
            {4, 8},
            {3, 9, 0},
            {},
            {1, 7, 0},
            {2, 6},
            {1, 3},
            {2, 4},
        };
        const int MOD = 1e9 + 7;
        for (int i = 0; i < n - 1; i ++ )
            for (int j = 0; j < 10; j ++ )
                for (int x: tr[j])
                    f[i + 1][x] = (f[i + 1][x] + f[i][j]) % MOD;

        int res = 0;
        for (int i = 0; i < 10; i ++ )
            res = (res + f[n - 1][i]) % MOD;
        return res;
    }
};
```
##### LeetCode 936. 戳印序列
```cpp
class Solution {
public:
    bool work(string& stamp, string& target, int k) {
        if (target.substr(k, stamp.size()) == string(stamp.size(), '?'))
            return false;
        for (int i = 0; i < stamp.size(); i ++ )
            if (target[k + i] != '?' && stamp[i] != target[k + i])
                return false;
        for (int i = 0; i < stamp.size(); i ++ )
            target[k + i] = '?';
        return true;
    }

    vector<int> movesToStamp(string stamp, string target) {
        vector<int> res;
        while (true) {
            bool flag = true;
            for (int i = 0; i + stamp.size() <= target.size(); i ++ ) {
                if (work(stamp, target, i)) {
                    res.push_back(i);
                    flag = false;
                }
            }
            if (flag) break;
        }
        if (target != string(target.size(), '?')) return {};
        reverse(res.begin(), res.end());
        return res;
    }
};
```
##### LeetCode 937. 重新排列日志文件
```cpp
class Solution {
public:
    vector<string> reorderLogFiles(vector<string>& logs) {
        stable_sort(logs.begin(), logs.end(), [](const string& a, const string& b) {
            int x = a.find(' '), y = b.find(' ');
            auto a1 = a.substr(0, x), b1 = b.substr(0, y);
            auto a2 = a.substr(x + 1), b2 = b.substr(y + 1);
            auto a3 = isdigit(a2[0]), b3 = isdigit(b2[0]);
            if (a3 != b3) return a3 < b3;
            if (a3) return false;
            if (a2 != b2) return a2 < b2;
            return a1 < b1;
        });
        return logs;
    }
};
```
##### LeetCode 938. 二叉搜索树的范围和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int rangeSumBST(TreeNode* root, int low, int high) {
        if (!root) return 0;
        int val = root->val;
        if (val >= low && val <= high)
            return val + rangeSumBST(root->left, low, high) + rangeSumBST(root->right, low, high);
        else if (val < low)
            return rangeSumBST(root->right, low, high);
        else
            return rangeSumBST(root->left, low, high);
    }
};
```
##### LeetCode 939. 最小面积矩形
```cpp
class Solution {
public:
    int get(vector<int> p) {
        return p[0] * 50000 + p[1];
    }

    int minAreaRect(vector<vector<int>>& points) {
        unordered_set<int> S;
        for (auto& p: points) S.insert(get(p));
        int res = INT_MAX;
        for (int i = 0; i < points.size(); i ++ )
            for (int j = i + 1; j < points.size(); j ++ ) {
                auto &p = points[i], &q = points[j];
                if (p[0] != q[0] && p[1] != q[1] && S.count(get({p[0], q[1]})) && S.count(get({q[0], p[1]})))
                    res = min(res, abs(p[0] - q[0]) * abs(p[1] - q[1]));
            }
        if (res == INT_MAX) res = 0;
        return res;
    }
};
```
##### LeetCode 940. 不同的子序列 II
```cpp
class Solution {
public:
    int distinctSubseqII(string s) {
        const int MOD = 1e9 + 7;
        int f[26] = {};
        for (auto c: s) {
            int x = c - 'a', s = 1;
            for (int i = 0; i < 26; i ++ )
                s = (s + f[i]) % MOD;
            f[x] = s;
        }
        int res = 0;
        for (int x: f) res = (res + x) % MOD;
        return res;
    }
};
```

##### LeetCode 941. 有效的山脉数组
```cpp
class Solution {
public:
    bool validMountainArray(vector<int>& a) {
        int k = 0;
        while (k + 1 < a.size() && a[k] < a[k + 1]) k ++ ;
        if (!k || k + 1 == a.size()) return false;
        while (k + 1 < a.size() && a[k] > a[k + 1]) k ++ ;
        return k + 1 == a.size();
    }
};
```
##### LeetCode 942. 增减字符串匹配
```cpp
class Solution {
public:
    vector<int> diStringMatch(string s) {
        int n = s.size();
        vector<int> res;
        int l = 0, r = n;
        for (auto c: s)
            if (c == 'I') res.push_back(l ++ );
            else res.push_back(r -- );
        res.push_back(l);
        return res;
    }
};
```
##### LeetCode 943. 最短超级串
```cpp
class Solution {
public:
    int get(string& a, string& b) {
        for (int i = min(a.size(), b.size()); i; i -- )
            if (a.substr(a.size() - i) == b.substr(0, i))
                return i;
        return 0;
    }

    string shortestSuperstring(vector<string>& words) {
        int n = words.size();
        vector<vector<int>> tr(n, vector<int>(n));
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                tr[i][j] = get(words[i], words[j]);

        const int INF = 1e8;
        vector<vector<int>> f(1 << n, vector<int>(n, INF));
        for (int i = 0; i < n; i ++ ) f[1 << i][i] = words[i].size();
        for (int i = 0; i < 1 << n; i ++ )
            for (int j = 0; j < n; j ++ ) {
                if (f[i][j] == INF) continue;
                for (int k = 0; k < n; k ++ )
                    if (!(i >> k & 1))
                        f[i | (1 << k)][k] = min(f[i | (1 << k)][k], f[i][j] + (int)words[k].size() - tr[j][k]);
            }

        int i = (1 << n) - 1, j = 0;
        for (int k = 0; k < n; k ++ )
            if (f[i][k] < f[i][j])
                j = k;

        vector<int> q;
        for (int u = 0; u < n; u ++ ) {
            q.push_back(j);
            for (int k = 0; k < n; k ++ )
                if (f[i - (1 << j)][k] + words[j].size() - tr[k][j] == f[i][j]) {
                    i -= 1 << j;
                    j = k;
                    break;
                }
        }
        reverse(q.begin(), q.end());
        string res = words[q[0]];
        for (int i = 1; i < n; i ++ )
            res += words[q[i]].substr(tr[q[i - 1]][q[i]]);
        return res;
    }
};
```
##### LeetCode 944. 删列造序
```cpp
class Solution {
public:
    int minDeletionSize(vector<string>& strs) {
        int n = strs.size(), m = strs[0].size();
        int res = 0;
        for (int i = 0; i < m; i ++ ) {
            bool flag = true;
            for (int j = 1; j < n; j ++ )
                if (strs[j][i] < strs[j - 1][i])
                    flag = false;
            if (!flag) res ++ ;
        }
        return res;
    }
};
```
##### LeetCode 945. 使数组唯一的最小增量
```cpp
class Solution {
public:
    int minIncrementForUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int res = 0, last = -1;
        for (auto x: nums) {
            int y = max(last + 1, x);
            res += y - x;
            last = y;
        }
        return res;
    }
};
```
##### LeetCode 946. 验证栈序列
```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& a, vector<int>& b) {
        stack<int> stk;
        int k = 0;
        for (int x: a) {
            stk.push(x);
            while (stk.size() && k < b.size() && stk.top() == b[k]) {
                stk.pop();
                k ++ ;
            }
        }
        return k == b.size();
    }
};
```
##### LeetCode 947. 移除最多的同行或同列石头
```cpp
class Solution {
public:
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int removeStones(vector<vector<int>>& stones) {
        int n = stones.size();
        for (int i = 0; i < n; i ++ ) p.push_back(i);
        unordered_map<int, vector<int>> row, col;
        for (int i = 0; i < n; i ++ ) {
            row[stones[i][0]].push_back(i);
            col[stones[i][1]].push_back(i);
        }
        for (auto& [k, v]: row) {
            for (int i = 1; i < v.size(); i ++ )
                p[find(v[0])] = find(v[i]);
        }
        for (auto& [k, v]: col) {
            for (int i = 1; i < v.size(); i ++ )
                p[find(v[0])] = find(v[i]);
        }

        unordered_set<int> S;
        for (int i = 0; i < n; i ++ ) S.insert(find(i));
        return n - S.size();
    }
};
```
##### LeetCode 948. 令牌放置
```cpp
class Solution {
public:
    int bagOfTokensScore(vector<int>& tokens, int power) {
        sort(tokens.begin(), tokens.end());
        int l = 0;
        while (l < tokens.size() && power >= tokens[l]) power -= tokens[l ++ ];
        int res = l;
        for (int r = tokens.size() - 1; r > l; r -- ) {
            if (l < tokens.size() - r) break;
            power += tokens[r];
            while (l < r && power >= tokens[l]) power -= tokens[l ++ ];
            res = max(res, l - ((int)tokens.size() - r));
        }
        return res;
    }
};
```
##### LeetCode 949. 给定数字能组成的最大时间
```cpp
class Solution {
public:
    string largestTimeFromDigits(vector<int>& a) {
        sort(a.begin(), a.end());
        string res;
        do {
            auto h = to_string(a[0]) + to_string(a[1]);
            auto m = to_string(a[2]) + to_string(a[3]);
            auto t = h + ':' + m;
            if (h <= "23" && m <= "59")
                if (res.empty() || res < t) res = t;
        } while (next_permutation(a.begin(), a.end()));
        return res;
    }
};
```
##### LeetCode 950. 按递增顺序显示卡牌
```cpp
class Solution {
public:
    vector<int> deckRevealedIncreasing(vector<int>& deck) {
        sort(deck.begin(), deck.end());
        queue<int> q;
        for (int i = 0; i < deck.size(); i ++ ) q.push(i);
        vector<int> res(deck.size());

        int k = 0;
        while (q.size()) {
            auto t = q.front();
            q.pop();
            res[t] = deck[k ++ ];
            if (q.size()) {
                q.push(q.front());
                q.pop();
            }
        }
        return res;
    }
};
```






### 究极班- Week 42（第 951 ~ 960 题）

##### LeetCode 951. 翻转等价二叉树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
        if (!root1 && !root2) return true;
        if (!root1 || !root2) return false;
        if (root1->val != root2->val) return false;
        return flipEquiv(root1->left, root2->left) && flipEquiv(root1->right, root2->right) ||
            flipEquiv(root1->right, root2->left) && flipEquiv(root1->left, root2->right);
    }
};
```
##### LeetCode 952. 按公因数计算最大组件大小
```cpp
class Solution {
public:
    vector<int> p, s;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int largestComponentSize(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; i ++ ) {
            p.push_back(i);
            s.push_back(1);
        }

        unordered_map<int, vector<int>> q;
        for (int i = 0; i < n; i ++ ) {
            int x = nums[i];
            for (int j = 1; j * j <= x; j ++ )
                if (x % j == 0) {
                    if (j > 1) q[j].push_back(i);
                    q[x / j].push_back(i);
                }
        }

        int res = 1;
        for (auto [k, v]: q) {
            for (int i = 1; i < v.size(); i ++ ) {
                int a = v[0], b = v[i];
                if (find(a) != find(b)) {
                    s[find(a)] += s[find(b)];
                    p[find(b)] = find(a);
                    res = max(res, s[find(a)]);
                }
            }
        }

        return res;
    }
};
```
##### LeetCode 953. 验证外星语词典
```cpp
class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) {
        unordered_map<char, int> p;
        for (int i = 0; i < order.size(); i ++ ) p[order[i]] = i;
        for (int i = 1; i < words.size(); i ++ ) {
            auto &a = words[i - 1], &b = words[i];
            int x = 0, y = 0;
            while (x < a.size() && y < b.size()) {
                if (p[a[x]] > p[b[y]]) return false;
                if (p[a[x]] < p[b[y]]) break;
                x ++, y ++ ;
            }
            if (x < a.size() && y == b.size()) return false;
        }
        return true;
    }
};
```
##### LeetCode 954. 二倍数对数组
```cpp
class Solution {
public:
    bool check(vector<int>& arr, int t) {
        map<int, int> cnt;
        for (auto x: arr)
            if (x * t > 0)
                cnt[x * t] ++ ;
        while (cnt.size()) {
            auto it = cnt.begin();
            int x = it->first, y = it->second;
            if (!cnt[x]) {
                cnt.erase(it);
            } else {
                if (!cnt[x * 2]) return false;
                cnt[x * 2] -- ;
                cnt[x] -- ;
            }
        }
        return true;
    }

    bool canReorderDoubled(vector<int>& arr) {
        return check(arr, 1) && check(arr, -1);
    }
};
```
##### LeetCode 955. 删列造序 II
```cpp
class Solution {
public:
    int minDeletionSize(vector<string>& strs) {
        int n = strs.size(), m = strs[0].size();
        vector<bool> f(n);
        int res = 0;
        for (int i = 0; i < m; i ++ ) {
            bool flag = true;
            for (int j = 1; j < n; j ++ )
                if (!f[j] && strs[j - 1][i] > strs[j][i])
                    flag = false;
            if (!flag) res ++ ;
            else {
                for (int j = 1; j < n; j ++ )
                    if (!f[j] && strs[j - 1][i] < strs[j][i])
                        f[j] = true;
            }
        }
        return res;
    }
};
```
##### LeetCode 956. 最高的广告牌
```cpp
class Solution {
public:
    int tallestBillboard(vector<int>& rods) {
        int n = rods.size(), m = accumulate(rods.begin(), rods.end(), 0);
        vector<vector<int>> f(n + 1, vector<int>(m * 2 + 1, -1e8));
        f[0][m] = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = 0; j <= m * 2; j ++ ) {
                int x = rods[i - 1];
                f[i][j] = f[i - 1][j];
                if (j - x >= 0) f[i][j] = max(f[i][j], f[i - 1][j - x] + x);
                if (j + x <= m * 2) f[i][j] = max(f[i][j], f[i - 1][j + x]);
            }
        return f[n][m];
    }
};
```
##### LeetCode 957. N 天后的牢房
```cpp
class Solution {
public:
    int get(int state) {
        int res = 0;
        for (int i = 7; i >= 0; i -- ) {
            res <<= 1;
            if (i < 7 && i) {
                if ((state >> i - 1 & 1) == (state >> i + 1 & 1))
                    res ++ ;
            }
        }
        return res;
    }

    vector<int> output(int state) {
        vector<int> res;
        for (int i = 7; i >= 0; i -- )
            res.push_back(state >> i & 1);
        return res;
    }

    vector<int> prisonAfterNDays(vector<int>& cells, int n) {
        int state = 0;
        for (auto x: cells) state = state * 2 + x;
        vector<int> f(256, -1);
        f[state] = 0;

        for (int i = 1; n; i ++ ) {
            int next = get(state);
            n -- ;
            if (f[next] != -1) n %= i - f[next];
            else f[next] = i;
            state = next;
        }
        return output(state);
    }
};
```
##### LeetCode 958. 二叉树的完全性检验
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int n = 0, p = 0;

    bool dfs(TreeNode* root, int k) {
        if (!root) return true;
        if (k > 100) return false;
        n ++, p = max(p, k);
        return dfs(root->left, k * 2) && dfs(root->right, k * 2 + 1);
    }

    bool isCompleteTree(TreeNode* root) {
        if (!dfs(root, 1)) return false;
        return n == p;
    }
};
```
##### LeetCode 959. 由斜杠划分区域
```cpp
class Solution {
public:
    int n;
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int get(int i, int j, int k) {
        return (i * n + j) * 4 + k;
    }

    int regionsBySlashes(vector<string>& grid) {
        n = grid.size();
        for (int i = 0; i < n * n * 4; i ++ ) p.push_back(i);
        int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ ) {
                for (int k = 0; k < 4; k ++ ) {
                    int x = i + dx[k], y = j + dy[k];
                    if (x >= 0 && x < n && y >= 0 && y < n)
                        p[find(get(i, j, k))] = find(get(x, y, k ^ 2));
                }
                if (grid[i][j] != '/') {
                    p[find(get(i, j, 0))] = find(get(i, j, 1));
                    p[find(get(i, j, 2))] = find(get(i, j, 3));
                }
                if (grid[i][j] != '\\') {
                    p[find(get(i, j, 0))] = find(get(i, j, 3));
                    p[find(get(i, j, 2))] = find(get(i, j, 1));
                }
            }
        unordered_set<int> S;
        for (int i = 0; i < n * n * 4; i ++ ) S.insert(find(i));
        return S.size();
    }
};

```
##### LeetCode 960. 删列造序 III
```cpp
class Solution {
public:
    bool check(vector<string>& strs, int i, int j) {
        for (int k = 0; k < strs.size(); k ++ )
            if (strs[k][i] > strs[k][j])
                return false;
        return true;
    }

    int minDeletionSize(vector<string>& strs) {
        int n = strs.size(), m = strs[0].size();
        vector<int> f(m);
        int res = m;
        for (int i = 0; i < m; i ++ ) {
            f[i] = i;
            for (int j = 0; j < i; j ++ )
                if (check(strs, j, i))
                    f[i] = min(f[i], f[j] + i - j - 1);
            res = min(res, f[i] + m - 1 - i);
        }
        return res;
    }
};
```


### 究极班— Week 43（第961-980题）


#####  LeetCode 961. 重复 N 次的元素
```cpp
class Solution {
public:
    int repeatedNTimes(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i += 2)
            if (nums[i] == nums[i + 1])
                return nums[i];
        if (nums[0] == nums[2] || nums[0] == nums[3])
            return nums[0];
        return nums[1];
    }
};

```
#####  LeetCode 962. 最大宽度坡
```cpp
class Solution {
public:
    int maxWidthRamp(vector<int>& nums) {
        vector<int> p(nums.size());
        for (int i = 0; i < nums.size(); i ++ ) p[i] = i;
        sort(p.begin(), p.end(), [&](int a, int b) {
            if (nums[a] != nums[b]) return nums[a] < nums[b];
            return a < b;
        });
        int res = 0;
        for (int i = 1, minp = p[0]; i < p.size(); i ++ ) {
            res = max(res, p[i] - minp);
            minp = min(minp, p[i]);
        }
        return res;
    }
};

```
#####  LeetCode 963. 最小面积矩形 II
```cpp
typedef long long LL;

class Solution {
public:
    LL get_dist(vector<int> a, vector<int> b) {
        LL dx = a[0] - b[0];
        LL dy = a[1] - b[1];
        return dx * dx + dy * dy;
    }

    double minAreaFreeRect(vector<vector<int>>& points) {
        int n = points.size();
        for (auto& p: points) p[0] *= 2, p[1] *= 2;
        vector<vector<LL>> q;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < i; j ++ ) {
                int x1 = points[i][0], y1 = points[i][1];
                int x2 = points[j][0], y2 = points[j][1];
                int cx = (x1 + x2) / 2, cy = (y1 + y2) / 2;
                q.push_back({cx, cy, get_dist(points[i], points[j]), i, j});
            }
        sort(q.begin(), q.end());
        double res = 1e20;
        for (int i = 0; i < q.size(); i ++ ) {
            int j = i + 1;
            while (j < q.size() && q[i][0] == q[j][0] && q[i][1] == q[j][1] && q[i][2] == q[j][2]) j ++ ;
            for (int a = i; a < j; a ++ )
                for (int b = i; b < a; b ++ ) {
                    auto &a1 = points[q[a][3]], &a2 = points[q[a][4]];
                    auto &b1 = points[q[b][3]], &b2 = points[q[b][4]];
                    double area = sqrt(get_dist(a1, b1)) * sqrt(get_dist(a1, b2));
                    if (area > 0) res = min(res, area);
                }
            i = j - 1;
        }
        if (res == 1e20) res = 0;
        return res / 4;
    }
};
```
#####  LeetCode 964. 表示数字的最少运算符
```cpp
class Solution {
public:
    unordered_map<string, int> f;

    int dp(int x, int target, int depth) {
        if (!target) return -1;
        auto state = to_string(target) + ' ' + to_string(depth);
        if (f.count(state)) return f[state];
        int cost = depth ? depth : 2;
        if (target == 1) return f[state] = cost - 1;

        int d = target / x, r = target % x;
        if (!r) f[state] = dp(x, d, depth + 1);
        else f[state] = min(dp(x, d, depth + 1) + r * cost, dp(x, d + 1, depth + 1) + (x - r) * cost);
        return f[state];
    }

    int leastOpsExpressTarget(int x, int target) {
        return dp(x, target, 0);
    }
};
```
#####  LeetCode 965. 单值二叉树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isUnivalTree(TreeNode* root) {
        return dfs(root, root->val);
    }

    bool dfs(TreeNode* root, int val) {
        if (!root) return true;
        if (root->val != val) return false;
        return dfs(root->left, val) && dfs(root->right, val);
    }
};
```
#####  LeetCode 966. 元音拼写检查器
```cpp
class Solution {
public:
    string filter1(string s) {
        string res;
        for (auto c: s) res += tolower(c);
        return res;
    }

    string filter2(string s) {
        s = filter1(s);
        string res;
        unordered_set<char> S{'a', 'e', 'i', 'o', 'u'};
        for (auto c: s)
            if (S.count(c)) res += 'a';
            else res += c;
        return res;
    }

    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        unordered_map<string, string> s1, s2, s3;
        for (auto& w: wordlist) {
            if (!s1.count(w)) s1[w] = w;
            if (!s2.count(filter1(w))) s2[filter1(w)] = w;
            if (!s3.count(filter2(w))) s3[filter2(w)] = w;
        }

        vector<string> res;
        for (auto& q: queries) {
            if (s1.count(q)) res.push_back(s1[q]);
            else if (s2.count(filter1(q))) res.push_back(s2[filter1(q)]);
            else if (s3.count(filter2(q))) res.push_back(s3[filter2(q)]);
            else res.push_back("");
        }
        return res;
    }
};

```
#####  LeetCode 967. 连续差相同的数字
```cpp
class Solution {
public:
    int n, k;
    vector<int> ans;

    void dfs(int u, int x) {
        if (u == n) ans.push_back(x);
        else {
            if (x % 10 - k >= 0) dfs(u + 1, x * 10 + x % 10 - k);
            if (k && x % 10 + k < 10) dfs(u + 1, x * 10 + x % 10 + k);
        }
    }

    vector<int> numsSameConsecDiff(int _n, int _k) {
        n = _n, k = _k;
        for (int i = 1; i <= 9; i ++ )
            dfs(1, i);
        return ans;
    }
};
```
#####  LeetCode 968. 监控二叉树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    const int INF = 1e8;

    vector<int> dfs(TreeNode* root) {
        if (!root) return {0, 0, INF};
        auto l = dfs(root->left), r = dfs(root->right);
        return {
            min(l[1], l[2]) + min(r[1], r[2]),
            min(l[2] + min(r[1], r[2]), r[2] + min(l[1], l[2])),
            min(l[0], min(l[1], l[2])) + min(r[0], min(r[1], r[2])) + 1,
        };
    }

    int minCameraCover(TreeNode* root) {
        auto f = dfs(root);
        return min(f[1], f[2]);
    }
};
```
#####  LeetCode 969. 煎饼排序
```cpp
class Solution {
public:
    vector<int> pancakeSort(vector<int>& arr) {
        int n = arr.size();
        vector<int> res;
        for (int i = n - 1; i >= 0; i -- ) {
            int k = 0;
            for (int j = 0; j <= i; j ++ )
                if (arr[j] > arr[k])
                    k = j;
            if (k == i) continue;
            reverse(arr.begin(), arr.begin() + k + 1);
            res.push_back(k + 1);
            reverse(arr.begin(), arr.begin() + i + 1);
            res.push_back(i + 1);
        }
        return res;
    }
};
```
#####  LeetCode 970. 强整数
```cpp
class Solution {
public:
    vector<int> powerfulIntegers(int x, int y, int bound) {
        unordered_set<int> S;
        for (int a = 1; a <= bound; a *= x) {
            for (int b = 1; a + b <= bound; b *= y) {
                S.insert(a + b);
                if (y == 1) break;
            }
            if (x == 1) break;
        }
        return vector<int>(S.begin(), S.end());
    }
};
```

#####  LeetCode  971. 翻转二叉树以匹配先序遍历
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> ans;

    bool dfs(TreeNode* root, vector<int>& voyage, int& k) {
        if (!root) return true;
        if (root->val != voyage[k]) return false;
        k ++ ;
        if (root->left && root->left->val != voyage[k]) {
            ans.push_back(root->val);
            return dfs(root->right, voyage, k) && dfs(root->left, voyage, k);
        } else {
            return dfs(root->left, voyage, k) && dfs(root->right, voyage, k);
        }
    }

    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        int k = 0;
        if (dfs(root, voyage, k)) return ans;
        return {-1};
    }
};
```
#####  LeetCode  972. 相等的有理数
```C++
typedef long long LL;
typedef vector<LL> VL;

class Solution {
public:
    LL gcd(LL a, LL b) {
        return b ? gcd(b, a % b) : a;
    }

    VL add(VL a, VL b) {
        if (!b[1]) return a;
        VL r{a[0] * b[1] + a[1] * b[0], a[1] * b[1]};
        LL d = gcd(r[0], r[1]);
        r[0] /= d, r[1] /= d;
        return r;
    }

    VL get(string s) {
        LL a = 0, b = 1;
        int k = 0;
        while (k < s.size() && s[k] != '.') {
            a = a * 10 + s[k] - '0';
            k ++ ;
        }
        VL r{a, b};
        a = 0, b = 1;
        k ++ ;
        while (k < s.size() && s[k] != '(') {
            a = a * 10 + s[k] - '0';
            b *= 10;
            k ++ ;
        }
        r = add(r, {a, b});
        k ++ ;
        int t = b;
        a = 0, b = 0;
        while (k < s.size() && s[k] != ')') {
            a = a * 10 + s[k] - '0';
            b = b * 10 + 9;
            k ++ ;
        }
        r = add(r, {a, b * t});
        return r;
    }

    bool isRationalEqual(string s, string t) {
        return get(s) == get(t);
    }
};

```
#####  LeetCode  973. 最接近原点的 K 个点
```C++
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        nth_element(points.begin(), points.begin() + k - 1, points.end(), [](vector<int>& a, vector<int>& b) {
            return a[0] * a[0] + a[1] * a[1] < b[0] * b[0] + b[1] * b[1];
        });
        return vector<vector<int>>(points.begin(), points.begin() + k);
    }
};

```
#####  LeetCode  974. 和可被 K 整除的子数组
```C++
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + nums[i - 1];
        unordered_map<int, int> cnt;
        cnt[0] ++ ;
        int res = 0;
        for (int i = 1; i <= n; i ++ ) {
            int r = (s[i] % k + k) % k;
            res += cnt[r];
            cnt[r] ++ ;
        }
        return res;
    }
};
```
#####  LeetCode  975. 奇偶跳
```C++
class Solution {
public:
    int oddEvenJumps(vector<int>& arr) {
        int n = arr.size();
        vector<vector<int>> f(n, vector<int>(2));
        f[n - 1][0] = f[n - 1][1] = 1;
        int res = 1;
        set<vector<int>> S;
        S.insert({arr[n - 1], n - 1});
        for (int i = n - 2; i >= 0; i -- ) {
            auto it = S.lower_bound({arr[i], -1});
            if (it != S.end()) {
                int j = (*it)[1];
                f[i][1] |= f[j][0];
            }
            it = S.lower_bound({arr[i], n});
            if (it != S.begin()) {
                -- it;
                it = S.lower_bound({(*it)[0], -1});
                int j = (*it)[1];
                f[i][0] |= f[j][1];
            }
            S.insert({arr[i], i});
            if (f[i][1]) res ++ ;
        }
        return res;
    }
};

```
#####  LeetCode  976. 三角形的最大周长
```C++
class Solution {
public:
    int largestPerimeter(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for (int i = nums.size() - 1; i >= 2; i -- ) {
            int a = nums[i - 2], b = nums[i - 1], c = nums[i];
            if (a + b > c)
                return a + b + c;
        }
        return 0;
    }
};
```
#####  LeetCode  977. 有序数组的平方
```C++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n);
        for (int i = 0, j = n - 1, k = n - 1; i <= j;) {
            if (nums[i] * nums[i] > nums[j] * nums[j]) {
                res[k] = nums[i] * nums[i];
                i ++, k -- ;
            } else {
                res[k] = nums[j] * nums[j];
                j --, k -- ;
            }
        }
        return res;
    }
};

```
#####  LeetCode  978. 最长湍流子数组
```C++
class Solution {
public:
    int maxTurbulenceSize(vector<int>& arr) {
        int res = 1;
        for (int i = 1, up = 1, down = 1; i < arr.size(); i ++ ) {
            if (arr[i] > arr[i - 1]) {
                up = down + 1;
                down = 1;
            } else if (arr[i] < arr[i - 1]) {
                down = up + 1;
                up = 1;
            } else {
                down = up = 1;
            }
            res = max(res, max(up, down));
        }
        return res;
    }
};
```
#####  LeetCode  980. 不同路径 III
```C++
class Solution {
public:
    int n, m, k;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    vector<vector<int>> g;

    int dfs(int x, int y) {
        if (g[x][y] == 2) {
            if (!k) return 1;
            return 0;
        }
        g[x][y] = -1, k -- ;
        int res = 0;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && g[a][b] != -1)
                res += dfs(a, b);
        }
        g[x][y] = 0, k ++ ;
        return res;
    }

    int uniquePathsIII(vector<vector<int>>& grid) {
        g = grid;
        n = g.size(), m = g[0].size(), k = 0;
        int sx, sy;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == 1) sx = i, sy = j, k ++ ;
                else if (!g[i][j]) k ++ ;
        return dfs(sx, sy);
    }
};

```



### 究极班- Week 44(第 981 ~ 990 题)

##### LeetCode 981. 基于时间的键值存储
```cpp
class TimeMap {
public:
    unordered_map<string, vector<pair<int, string>>> S;

    /** Initialize your data structure here. */
    TimeMap() {

    }

    void set(string key, string value, int timestamp) {
        S[key].push_back({timestamp, value});
    }

    string get(string key, int timestamp) {
        auto& q = S[key];
        pair<int, string> p(timestamp + 1, "");
        auto it = upper_bound(q.begin(), q.end(), p);
        if (it == q.begin()) return "";
        -- it;
        return it->second;
    }
};

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap* obj = new TimeMap();
 * obj->set(key,value,timestamp);
 * string param_2 = obj->get(key,timestamp);
 */
```
##### LeetCode 982. 按位与为零的三元组
```cpp
class Solution {
public:
    int countTriplets(vector<int>& nums) {
        unordered_map<int, int> cnt;
        int n = nums.size();
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                cnt[nums[i] & nums[j]] ++ ;
        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < 1 << 16; j ++ )
                if (!(j & nums[i]))
                    res += cnt[j];
        return res;
    }
};
```
##### LeetCode 983. 最低票价
```cpp
class Solution {
public:
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        int n = days.size();
        vector<int> f(n + 1);
        for (int i = 1, a = 0, b = 0, c = 0; i <= n; i ++ ) {
            while (days[i - 1] - days[a] >= 1) a ++ ;
            while (days[i - 1] - days[b] >= 7) b ++ ;
            while (days[i - 1] - days[c] >= 30) c ++ ;
            f[i] = min(f[a] + costs[0], min(f[b] + costs[1], f[c] + costs[2]));
        }
        return f[n];
    }
};
```
##### LeetCode 984. 不含 AAA 或 BBB 的字符串
```cpp
class Solution {
public:
    string strWithout3a3b(int a, int b) {
        string res;
        if (a >= b) {
            while (a > 0 || b > 0) {
                if (a > b) res += "aab", a -= 2, b -- ;
                else res += "ab", a --, b -- ;
            }
        } else {
            while (a > 0 || b > 0) {
                if (a < b) res += "bba", a --, b -= 2;
                else res += "ba", a --, b -- ;
            }
        }
        for (int i = 0; i < abs(a + b); i ++ )
            res.pop_back();
        return res;
    }
};
```

##### LeetCode 985. 查询后的偶数和
```cpp
class Solution {
public:
    vector<int> sumEvenAfterQueries(vector<int>& nums, vector<vector<int>>& queries) {
        vector<int> res;
        int sum = 0;
        for (auto x: nums)
            if (x % 2 == 0)
                sum += x;
        for (auto& q: queries) {
            int val = q[0], index = q[1];
            if (nums[index] % 2 == 0) sum -= nums[index];
            nums[index] += val;
            if (nums[index] % 2 == 0) sum += nums[index];
            res.push_back(sum);
        }
        return res;
    }
};
```
##### LeetCode 986. 区间列表的交集
```cpp
class Solution {
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& a, vector<vector<int>>& b) {
        vector<vector<int>> res;
        for (int i = 0, j = 0; i < a.size() && j < b.size();) {
            if (b[j][0] <= a[i][1] && a[i][0] <= b[j][1])
                res.push_back({max(a[i][0], b[j][0]), min(a[i][1], b[j][1])});
            if (a[i][1] <= b[j][1]) i ++ ;
            else j ++ ;
        }
        return res;
    }
};
```
##### LeetCode 987. 二叉树的垂序遍历
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    map<int, vector<vector<int>>> S;

    void dfs(TreeNode* root, int x, int y) {
        if (!root) return;
        S[y].push_back({x, root->val});
        dfs(root->left, x + 1, y - 1);
        dfs(root->right, x + 1, y + 1);
    }

    vector<vector<int>> verticalTraversal(TreeNode* root) {
        dfs(root, 0, 0);
        vector<vector<int>> res;
        for (auto& [k, v]: S) {
            sort(v.begin(), v.end());
            vector<int> col;
            for (auto& p: v) col.push_back(p[1]);
            res.push_back(col);
        }
        return res;
    }
};
```
##### LeetCode 988. 从叶结点开始的最小字符串
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    string ans, path;

    void dfs(TreeNode* root) {
        if (!root) return;
        path += root->val + 'a';

        if (!root->left && !root->right) {
            reverse(path.begin(), path.end());
            if (ans.empty() || ans > path) ans = path;
            reverse(path.begin(), path.end());
        } else {
            dfs(root->left);
            dfs(root->right);
        }

        path.pop_back();
    }

    string smallestFromLeaf(TreeNode* root) {
        dfs(root);
        return ans;
    }
};
```
##### LeetCode 989. 数组形式的整数加法
```cpp
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& num, int k) {
        reverse(num.begin(), num.end());

        for (auto& x: num) {
            k += x;
            x = k % 10;
            k /= 10;
        }
        while (k) num.push_back(k % 10), k /= 10;

        reverse(num.begin(), num.end());
        return num;
    }
};
```
##### LeetCode 990. 等式方程的可满足性
```cpp
class Solution {
public:
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    bool equationsPossible(vector<string>& equations) {
        for (int i = 0; i < 26; i ++ ) p.push_back(i);
        for (auto& e: equations) {
            int a = e[0] - 'a', b = e[3] - 'a';
            if (e[1] == '=') p[find(a)] = find(b);
        }
        for (auto& e: equations) {
            int a = e[0] - 'a', b = e[3] - 'a';
            if (e[1] == '!')
                if (find(a) == find(b))
                    return false;
        }
        return true;
    }
};
```




### 究极班- Week 45（第 991 ~ 1000 题）

##### LeetCode 991. 坏了的计算器
```cpp
class Solution {
public:
    int brokenCalc(int x, int y) {
        int res = 0;
        while (y > x) {
            if (y % 2) y ++ ;
            else y /= 2;
            res ++ ;
        }
        return res + x - y;
    }
};
```
##### LeetCode 992. K 个不同整数的子数组
```cpp
class Solution {
public:
    int subarraysWithKDistinct(vector<int>& nums, int k) {
        unordered_map<int, int> S1, S2;
        int res = 0;
        for (int i = 0, j1 = 0, j2 = 0, cnt1 = 0, cnt2 = 0; i < nums.size(); i ++ ) {
            if (!S1[nums[i]]) cnt1 ++ ;
            S1[nums[i]] ++ ;
            while (cnt1 > k) {
                S1[nums[j1]] -- ;
                if (!S1[nums[j1]]) cnt1 -- ;
                j1 ++ ;
            }

            if (!S2[nums[i]]) cnt2 ++ ;
            S2[nums[i]] ++ ;
            while (cnt2 >= k) {
                S2[nums[j2]] -- ;
                if (!S2[nums[j2]]) cnt2 -- ;
                j2 ++ ;
            }

            res += j2 - j1;
        }
        return res;
    }
};
```
##### LeetCode 993. 二叉树的堂兄弟节点
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> dfs(TreeNode* root, int x, int fa, int depth) {
        if (!root) return {0, 0};
        if (root->val == x) return {fa, depth};
        auto l = dfs(root->left, x, root->val, depth + 1);
        auto r = dfs(root->right, x, root->val, depth + 1);
        return {l[0] + r[0], l[1] + r[1]};
    }

    bool isCousins(TreeNode* root, int x, int y) {
        auto a = dfs(root, x, -1, 0);
        auto b = dfs(root, y, -1, 0);
        return a[0] != b[0] && a[1] == b[1];
    }
};
```
##### LeetCode 994. 腐烂的橘子
```cpp
#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    int orangesRotting(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size();
        queue<PII> q;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == 2)
                    q.push({i, j});

        int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};

        int res = 0;
        if (q.size()) res -- ;
        while (q.size()) {
            res ++ ;
            int sz = q.size();
            while (sz -- ) {
                auto t = q.front();
                q.pop();
                for (int i = 0; i < 4; i ++ ) {
                    int x = t.x + dx[i], y = t.y + dy[i];
                    if (x < 0 || x >= n || y < 0 || y >= m || g[x][y] != 1)
                        continue;
                    g[x][y] = 2;
                    q.push({x, y});
                }
            }
        }

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == 1)
                    return -1;
        return res;
    }
};
```
##### LeetCode 995. K 连续位的最小翻转次数
```cpp
class Solution {
public:
    int minKBitFlips(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> s(n + 1);

        for (int i = 1; i <= n; i ++ ) {
            int t = s[i - 1] - s[max(i - k, 0)] & 1;
            nums[i - 1] ^= t;
            if (!nums[i - 1]) {
                if (i >= n - k + 2) return -1;
                s[i] ++ ;
            }
            s[i] += s[i - 1];
        }

        return s[n];
    }
};
```
##### LeetCode 996. 正方形数组的数目
```cpp
class Solution {
public:
    bool is_sqr(int x) {
        int t = sqrt(x);
        return t * t == x;
    }

    int fact(int x) {
        int res = 1;
        for (int i = 1; i <= x; i ++ ) res *= i;
        return res;
    }

    int numSquarefulPerms(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> f(1 << n, vector<int>(n));
        for (int i = 0; i < n; i ++ ) f[1 << i][i] = 1;
        for (int i = 0; i < 1 << n; i ++ )
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                    for (int k = 0; k < n; k ++ )
                        if (!(i >> k & 1) && is_sqr(nums[j] + nums[k]))
                            f[i | 1 << k][k] += f[i][j];

        int res = 0;
        for (int i = 0; i < n; i ++ ) res += f[(1 << n) - 1][i];
        unordered_map<int, int> cnt;
        for (auto x: nums) cnt[x] ++ ;

        for (auto [k, v]: cnt)
            res /= fact(v);

        return res;
    }
};
```
##### LeetCode 997. 找到小镇的法官
```cpp
class Solution {
public:
    int findJudge(int n, vector<vector<int>>& trust) {
        vector<int> din(n + 1), dout(n + 1);
        for (auto p: trust) {
            int a = p[0], b = p[1];
            din[b] ++ ;
            dout[a] ++ ;
        }

        int res = -1;
        for (int i = 1; i <= n; i ++ )
            if (!dout[i] && din[i] == n - 1) {
                if (res != -1) return -1;
                res = i;
            }

        return res;
    }
};
```
##### LeetCode 998. 最大二叉树 II
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* insertIntoMaxTree(TreeNode* root, int val) {
        if (!root || root->val < val) {
            auto p = new TreeNode(val);
            p->left = root;
            return p;
        } else {
            root->right = insertIntoMaxTree(root->right, val);
            return root;
        }
    }
};
```
##### LeetCode 999. 可以被一步捕获的棋子数
```cpp
class Solution {
public:
    int numRookCaptures(vector<vector<char>>& g) {
        int n = g.size(), m = g[0].size();
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        int sx, sy;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == 'R') {
                    sx = i;
                    sy = j;
                }

        int res = 0;
        for (int i = 0; i < 4; i ++ ) {
            int x = sx, y = sy;
            while (true) {
                x += dx[i], y += dy[i];
                if (x < 0 || x >= n || y < 0 || y >= m) break;
                if (g[x][y] == 'B') break;
                if (g[x][y] == 'p') {
                    res ++ ;
                    break;
                }
            }
        }

        return res;
    }
};
```
##### LeetCode 1000. 合并石头的最低成本
```cpp
class Solution {
public:
    int mergeStones(vector<int>& stones, int m) {
        int n = stones.size(), INF = 0x3f3f3f3f;
        if ((n - 1) % (m - 1)) return -1;
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + stones[i - 1];
        vector<vector<int>> f(n, vector<int>(n, INF));
        for (int i = 0; i < n; i ++ ) f[i][i] = 0;

        for (int len = 2; len <= n; len ++ )
            for (int i = 0; i + len - 1 < n; i ++ ) {
                int j = i + len - 1;
                for (int u = j - 1; u >= i; u -= m - 1)
                    f[i][j] = min(f[i][j], f[i][u] + f[u + 1][j]);

                if ((len - 1) % (m - 1) == 0)
                    f[i][j] += s[j + 1] - s[i];
            }

        return f[0][n - 1];
    }
};
```





### 究极班- Week 46(第 1001 ~ 1012 题)

##### LeetCode 1001. 网格照明
```cpp
class Solution {
public:
    vector<int> gridIllumination(int n, vector<vector<int>>& lamps, vector<vector<int>>& queries) {
        unordered_map<int, unordered_set<int>> row, col, dg, udg;
        for (auto& p: lamps) {
            int x = p[0], y = p[1];
            row[x].insert(y);
            col[y].insert(x);
            dg[y - x].insert(x);
            udg[x + y].insert(x);
        }

        vector<int> res;
        for (auto& q: queries) {
            int x = q[0], y = q[1];
            if (row[x].size() || col[y].size() || dg[y - x].size() || udg[x + y].size()) {
                res.push_back(1);
                for (int i = x - 1; i <= x + 1; i ++ )
                    for (int j = y - 1; j <= y + 1; j ++ ) {
                        row[i].erase(j);
                        col[j].erase(i);
                        dg[j - i].erase(i);
                        udg[i + j].erase(i);
                    }
            } else {
                res.push_back(0);
            }
        }

        return res;
    }
};
```
##### LeetCode 1002. 查找共用字符
```cpp
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        vector<int> cnt(26, 1e8);
        for (auto& word: words) {
            unordered_map<int, int> S;
            for (auto c: word) S[c - 'a'] ++ ;
            for (int i = 0; i < 26; i ++ ) cnt[i] = min(cnt[i], S[i]);
        }

        vector<string> res;
        for (int i = 0; i < 26; i ++ )
            for (int j = 0; j < cnt[i]; j ++ )
                res.push_back(string(1, i + 'a'));

        return res;
    }
};

```
##### LeetCode 1003. 检查替换后的词是否有效
```cpp
class Solution {
public:
    bool isValid(string s) {
        string stk;
        for (auto c: s) {
            stk += c;
            if (stk.size() >= 3 && stk.substr(stk.size() - 3) == "abc") {
                for (int i = 0; i < 3; i ++ )
                    stk.pop_back();
            }
        }
        return stk.empty();
    }
};
```
##### LeetCode 1004. 最大连续1的个数 III
```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int res = 0;
        for (int i = 0, j = 0, cnt = 0; i < nums.size(); i ++ ) {
            if (!nums[i]) cnt ++ ;
            while (cnt > k) {
                if (!nums[j]) cnt -- ;
                j ++ ;
            }
            res = max(res, i - j + 1);
        }

        return res;
    }
};
```
##### LeetCode 1005. K 次取反后最大化的数组和
```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;

        int t = 1e8;
        for (auto x: nums) {
            cnt[x] ++ ;
            t = min(t, abs(x));
        }

        for (int i = -100; i < 0 && k > 0; i ++ ) {
            while (cnt[i] > 0 && k > 0) {
                cnt[i] -- ;
                cnt[-i] ++ ;
                k -- ;
            }
        }

        int res = 0;
        for (int i = -100; i <= 100; i ++ ) res += i * cnt[i];

        if (k % 2) res -= 2 * t;

        return res;
    }
};
```
##### LeetCode 1006. 笨阶乘
```cpp
class Solution {
public:
    int clumsy(int N) {
        if (N == 1) return 1;
        if (N == 2) return 2;
        if (N == 3) return 6;
        if (N == 4) return 7;
        if (N % 4 == 0) return N + 1;
        if (N % 4 == 1) return N + 2;
        if (N % 4 == 2) return N + 2;
        return N - 1;
    }
};
```
##### LeetCode 1007. 行相等的最少多米诺旋转
```cpp
class Solution {
public:
    int minDominoRotations(vector<int>& tops, vector<int>& bottoms) {
        const int INF = 0x3f3f3f3f;
        int res = INF, n = tops.size();
        int value[] = {tops[0], bottoms[0]};
        for (auto x: value) {
            int t = 0;
            for (int i = 0; i < n; i ++ )
                if (tops[i] != x) {
                    if (bottoms[i] != x) t = INF;
                    else t ++ ;
                }

            res = min(res, t);
            t = 0;
            for (int i = 0; i < n; i ++ )
                if (bottoms[i] != x) {
                    if (tops[i] != x) t = INF;
                    else t ++ ;
                }
            res = min(res, t);
        }

        if (res == INF) res = -1;

        return res;
    }
};
```
##### LeetCode 1008. 前序遍历构造二叉搜索树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> a, b;
    unordered_map<int, int> pos;

    TreeNode* build(int x, int y, int l, int r) {
        if (x > y) return NULL;
        auto root = new TreeNode(a[x]);
        int k = pos[root->val];
        root->left = build(x + 1, k - l + x, l, k - 1);
        root->right = build(k - l + x + 1, y, k + 1, r);
        return root;
    }

    TreeNode* bstFromPreorder(vector<int>& preorder) {
        a = b = preorder;
        int n = a.size();
        sort(b.begin(), b.end());
        for (int i = 0; i < n; i ++ ) pos[b[i]] = i;
        return build(0, n - 1, 0, n - 1);
    }
};
```
##### LeetCode 1009. 十进制整数的反码
```cpp
class Solution {
public:
    int bitwiseComplement(int n) {
        if (!n) return 1;
        int res = 0;
        for (int i = 0; 1 << i <= n; i ++ )
            res |= !(n >> i & 1) << i;
        return res;
    }
};
```
##### LeetCode 1010. 总持续时间可被 60 整除的歌曲
```cpp
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        int s[60] = {};
        int res = 0;
        for (auto x: time) {
            res += s[(60 - x % 60) % 60];
            s[x % 60] ++ ;
        }
        return res;
    }
};
```
##### LeetCode 1011. 在 D 天内送达包裹的能力
```cpp
class Solution {
public:
    bool check(vector<int>& w, int D, int x) {
        int cnt = 1;
        for (int i = 0, s = 0; i < w.size(); i ++ ) {
            if (w[i] > x) return false;
            if (s + w[i] > x) {
                cnt ++ ;
                s = 0;
            }
            s += w[i];
        }
        return cnt <= D;
    }

    int shipWithinDays(vector<int>& weights, int D) {
        int l = 0, r = 500 * 50000;
        while (l < r) {
            int mid = l + r >> 1;
            if (check(weights, D, mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
##### LeetCode 1012. 至少有 1 位重复的数字
```cpp
class Solution {
public:
    int P(int a, int b) {
        int res = 1;
        for (int i = a, j = 0; j < b; i --, j ++ )
            res *= i;
        return res;
    }

    int numDupDigitsAtMostN(int n) {
        vector<int> nums;
        int res = n;
        while (n) nums.push_back(n % 10), n /= 10;

        vector<bool> st(10);

        for (int i = 1; i < nums.size(); i ++ )
            res -= 9 * P(9, i - 1);

        res -= (nums.back() - 1) * P(9, nums.size() - 1);

        st[nums.back()] = true;
        for (int i = nums.size() - 2; i >= 0; i -- ) {
            int x = nums[i];
            for (int j = 0; j < x; j ++ )
                if (!st[j])
                    res -= P(10 - (nums.size() - i), i);

            if (st[x]) return res;
            st[x] = true;
        }

        return res - 1;
    }
};
```





### 究极班- Week 47（第 1013 ~ 1022 题）

##### LeetCode 1013. 将数组分成和相等的三个部分
```cpp
class Solution {
public:
    bool canThreePartsEqualSum(vector<int>& arr) {
        int sum = accumulate(arr.begin(), arr.end(), 0);
        if (sum % 3) return false;
        sum /= 3;

        int i = 0, j = arr.size() - 1;
        for (int s = 0; i < arr.size(); i ++ ) {
            s += arr[i];
            if (s == sum) break;
        }
        for (int s = 0; j >= 0; j -- ) {
            s += arr[j];
            if (s == sum) break;
        }
        return i + 1 <= j - 1;
    }
};
```
##### LeetCode 1014. 最佳观光组合
```cpp
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& values) {
        int res = 0, maxv = values[0];
        for (int i = 1; i < values.size(); i ++ ) {
            res = max(values[i] - i + maxv, res);
            maxv = max(maxv, values[i] + i);
        }
        return res;
    }
};
```
##### LeetCode 1015. 可被 K 整除的最小整数
```cpp
typedef long long LL;

class Solution {
public:
    int get_euler(int x) {
        int res = x;
        for (int i = 2; i * i <= x; i ++ )
            if (x % i == 0) {
                res = res / i * (i - 1);
                while (x % i == 0) x /= i;
            }
        if (x > 1) res = res / x * (x - 1);
        return res;
    }

    int qmi(int a, int b, int p) {
        int res = 1;
        while (b) {
            if (b & 1) res = (LL)res * a % p;
            a = (LL)a * a % p;
            b >>= 1;
        }
        return res;
    }

    int smallestRepunitDivByK(int k) {
        if (k % 2 == 0 || k % 5 == 0) return -1;

        int phi = get_euler(k * 9);

        int res = phi;
        for (int i = 1; i * i <= phi; i ++ )
            if (phi % i == 0) {
                if (qmi(10, i, k * 9) == 1) return i;
                if (qmi(10, phi / i, k * 9) == 1)
                    res = min(res, phi / i);
            }

        return res;
    }
};

```
##### LeetCode 1016. 子串能表示从 1 到 N 数字的二进制串
```cpp
class Solution {
public:
    bool queryString(string s, int n) {
        unordered_set<int> S;
        for (int i = 0; i < s.size(); i ++ ) {
            int x = 0;
            for (int j = i; j < s.size(); j ++ ) {
                x = x * 2 + s[j] - '0';
                if (x > n) break;
                if (x) S.insert(x);
            }
        }

        return S.size() == n;
    }
};
```
##### LeetCode 1017. 负二进制转换
```cpp
class Solution {
public:
    string baseNeg2(int n) {
        if (!n) return "0";
        string res;
        while (n) {
            int r = n & 1;
            res += to_string(r);
            n = (n - r) / -2;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```
##### LeetCode 1018. 可被 5 整除的二进制前缀
```cpp
class Solution {
public:
    vector<bool> prefixesDivBy5(vector<int>& nums) {
        vector<bool> res;
        for (int i = 0, r = 0; i < nums.size(); i ++ ) {
            r = r * 2 + nums[i];
            r %= 5;
            if (!r) res.push_back(true);
            else res.push_back(false);
        }
        return res;
    }
};
```


##### LeetCode 1019. 链表中的下一个更大节点
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    vector<int> nextLargerNodes(ListNode* head) {
        vector<int> res;
        for (auto p = head; p; p = p->next) res.push_back(p->val);
        stack<int> stk;
        for (int i = res.size() - 1; i >= 0; i -- ) {
            int x = res[i];
            while (stk.size() && x >= stk.top()) stk.pop();
            if (stk.size()) res[i] = stk.top();
            else res[i] = 0;
            stk.push(x);
        }

        return res;
    }
};
```
##### LeetCode 1020. 飞地的数量
```cpp
class Solution {
public:
    vector<vector<int>> g;
    int n, m;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    void dfs(int x, int y) {
        g[x][y] = 0;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && g[a][b])
                dfs(a, b);
        }
    }

    int numEnclaves(vector<vector<int>>& grid) {
        g = grid;
        n = g.size(), m = g[0].size();

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!i || !j || i == n - 1 || j == m - 1)
                    if (g[i][j])
                        dfs(i, j);

        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                res += g[i][j];

        return res;
    }
};

```
##### LeetCode 1021. 删除最外层的括号
```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string res;
        for (int i = 0, cnt = 0, j = 0; i < s.size(); i ++ ) {
            if (s[i] == '(') cnt ++ ;
            else if ( -- cnt == 0) {
                res += s.substr(j + 1, i - j - 1);
                j = i + 1;
            }
        }
        return res;
    }
};

```
##### LeetCode 1022. 从根到叶的二进制数之和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int dfs(TreeNode* root, int x) {
        x = x * 2 + root->val;
        if (!root->left && !root->right) return x;
        int res = 0;
        if (root->left) res += dfs(root->left, x);
        if (root->right) res += dfs(root->right, x);
        return res;
    }

    int sumRootToLeaf(TreeNode* root) {
        return dfs(root, 0);
    }
};
```


### 究极班— Week 48(第1023-1032题)

#####  LeetCode  1023. 驼峰式匹配
```C++
class Solution {
public:
    bool check(string& a, string& b) {
        int j = 0;
        for (auto c: a) {
            if (j < b.size() && c == b[j]) j ++ ;
            else if (c < 'a') return false;
        }
        return j == b.size();
    }

    vector<bool> camelMatch(vector<string>& queries, string pattern) {
        vector<bool> res;
        for (auto& q: queries)
            res.push_back(check(q, pattern));
        return res;
    }
};

```
#####  LeetCode  1024. 视频拼接
```C++
class Solution {
public:
    int videoStitching(vector<vector<int>>& clips, int time) {
        sort(clips.begin(), clips.end(), [&](vector<int>& a, vector<int>& b) {
            return a[0] < b[0];
        });

        int res = 0, last = 0;
        for (int i = 0; i < clips.size();) {
            if (clips[i][0] > last) return -1;

            int r = 0;
            while (i < clips.size() && clips[i][0] <= last) {
                r = max(r, clips[i][1]);
                i ++ ;
            }

            last = r;
            res ++ ;

            if (last >= time) break;
        }

        if (last >= time) return res;
        return -1;
    }
};

```
#####  LeetCode  1025. 除数博弈
```C++
class Solution {
public:
    bool divisorGame(int n) {
        return n % 2 == 0;
    }
};
```
#####  LeetCode  1026. 节点与其祖先之间的最大差值
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int dfs(TreeNode* root, int minv, int maxv) {
        if (!root) return -1;
        int val = root->val;
        int res = max(abs(val - minv), abs(val - maxv));
        minv = min(minv, val);
        maxv = max(maxv, val);
        res = max(res, dfs(root->left, minv, maxv));
        res = max(res, dfs(root->right, minv, maxv));
        return res;
    }

    int maxAncestorDiff(TreeNode* root) {
        int val = root->val;
        return max(dfs(root->left, val, val), dfs(root->right, val, val));
    }
};

```
#####  LeetCode  1027. 最长等差数列
```C++
class Solution {
public:
    int longestArithSeqLength(vector<int>& nums) {
        int n = nums.size();
        vector<unordered_map<int, int>> f(n);

        int res = 2;
        for (int i = 0; i < n; i ++ )
            for (int k = 0; k < i; k ++ ) {
                int j = nums[k] - nums[i];
                f[i][j] = max(f[i][j], 2);
                if (f[k].count(j)) {
                    f[i][j] = max(f[i][j], f[k][j] + 1);
                    res = max(res, f[i][j]);
                }
            }

        return res;
    }
};

```
#####  LeetCode  1028. 从先序遍历还原二叉树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    vector<PII> nodes;
    int k = 0;

    TreeNode* dfs(int d) {
        if (k == nodes.size()) return NULL;
        auto root = new TreeNode(nodes[k].y);
        k ++ ;

        if (k < nodes.size() && nodes[k].x > d)
            root->left = dfs(d + 1);
        if (k < nodes.size() && nodes[k].x > d)
            root->right = dfs(d + 1);

        return root;
    }

    TreeNode* recoverFromPreorder(string str) {
        for (int i = 0; i < str.size();) {
            int dep = 0;
            while (str[i] == '-') dep ++, i ++ ;
            int val = 0;
            while (i < str.size() && str[i] != '-')
                val = val * 10 + str[i ++ ] - '0';
            nodes.push_back({dep, val});
        }

        return dfs(0);
    }
};

```
#####  LeetCode  1029. 两地调度
```C++
class Solution {
public:
    int twoCitySchedCost(vector<vector<int>>& costs) {
        sort(costs.begin(), costs.end(), [&](vector<int>& a, vector<int>& b) {
            return a[0] - a[1] < b[0] - b[1];
        });

        int n = costs.size() / 2;
        int res = 0;
        for (int i = 0; i < n; i ++ ) res += costs[i][0];
        for (int i = n; i < n * 2; i ++ ) res += costs[i][1];
        return res;
    }
};

```
#####  LeetCode  1030. 距离顺序排列矩阵单元格
```C++
class Solution {
public:
    vector<vector<int>> allCellsDistOrder(int n, int m, int r, int c) {
        vector<vector<int>> res(1, {r, c});
        int dx[4] = {1, 1, -1, -1}, dy[4] = {1, -1, -1, 1};

        for (int d = 1; ; d ++ ) {
            int x = r - d, y = c, cnt = 0;
            for (int i = 0; i < 4; i ++ ) {
                for (int j = 0; j < d; j ++ ) {
                    if (x >= 0 && x < n && y >= 0 && y < m) {
                        res.push_back({x, y});
                        cnt ++ ;
                    }
                    x += dx[i], y += dy[i];
                }
            }

            if (!cnt) break;
        }
        return res;
    }
};

```
#####  LeetCode  1031. 两个非重叠子数组的最大和
```C++
class Solution {
public:
    int work(vector<int>& nums, int a, int b) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ )
            s[i] = s[i - 1] + nums[i - 1];

        int res = 0;
        for (int i = a + b, maxa = 0; i <= n; i ++ ) {
            maxa = max(maxa, s[i - b] - s[i - b - a]);
            res = max(res, s[i] - s[i - b] + maxa);
        }

        return res;
    }

    int maxSumTwoNoOverlap(vector<int>& nums, int a, int b) {
        return max(work(nums, a, b), work(nums, b, a));
    }
};
```
#####  LeetCode  1032. 字符流
```C++
AC自动机写法
const int N = 100000;

int tr[N][26], cnt[N], idx;
int q[N], ne[N];

class StreamChecker {
public:

    void insert(string& w) {
        int p = 0;
        for (int i = 0; i < w.size(); i ++ ) {
            int t = w[i] - 'a';
            if (!tr[p][t]) tr[p][t] = ++ idx;
            p = tr[p][t];
        }
        cnt[p] = 1;
    }

    void build() {
        int hh = 0, tt = -1;
        for (int i = 0; i < 26; i ++ )
            if (tr[0][i])
                q[ ++ tt] = tr[0][i];

        while (hh <= tt) {
            int t = q[hh ++ ];
            for (int i = 0; i < 26; i ++ ) {
                int p = tr[t][i];
                if (!p) tr[t][i] = tr[ne[t]][i];
                else {
                    ne[p] = tr[ne[t]][i];
                    cnt[p] |= cnt[ne[p]];
                    q[ ++ tt] = p;
                }
            }
        }
    }

    StreamChecker(vector<string>& words) {
        memset(tr, 0, sizeof tr);
        memset(cnt, 0, sizeof cnt);
        memset(ne, 0, sizeof ne);
        idx = 0;

        for (auto& w: words) insert(w);

        build();
    }

    int cur = 0;
    bool query(char letter) {
        cur = tr[cur][letter - 'a'];
        return cnt[cur];
    }
};

/**
 * Your StreamChecker object will be instantiated and called as such:
 * StreamChecker* obj = new StreamChecker(words);
 * bool param_1 = obj->query(letter);
 */
```



### 究极班- Week 49（第 1033 ~ 1042 题）

##### LeetCode 1033. 移动石子直到连续
```cpp
class Solution {
public:
    vector<int> numMovesStones(int a, int b, int c) {
        if (a > b) swap(a, b);
        if (a > c) swap(a, c);
        if (b > c) swap(b, c);

        vector<int> res(2);
        if (c - a == 2) res[0] = 0;
        else if (b - a <= 2 || c - b <= 2) res[0] = 1;
        else res[0] = 2;

        res[1] = c - a - 2;
        return res;
    }
};
```
##### LeetCode 1034. 边框着色
```cpp
class Solution {
public:
    vector<vector<int>> g;
    vector<vector<int>> st;  // 0未搜过；1：搜过；2：搜过且是边界
    int n, m;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    void dfs(int x, int y) {
        bool is_border = false;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && g[a][b] == g[x][y]) {
                if (!st[a][b]) {
                    st[a][b] = 1;
                    dfs(a, b);
                }
            } else {
                is_border = true;
            }
        }

        if (is_border) st[x][y] = 2;
    }

    vector<vector<int>> colorBorder(vector<vector<int>>& grid, int row, int col, int color) {
        g = grid;
        n = g.size(), m = g[0].size();
        st = vector<vector<int>>(n, vector<int>(m));

        st[row][col] = 1;
        dfs(row, col);

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (st[i][j] == 2)
                    grid[i][j] = color;

        return grid;
    }
};
```
##### LeetCode 1035. 不相交的线
```cpp
class Solution {
public:
    int maxUncrossedLines(vector<int>& a, vector<int>& b) {
        int n = a.size(), m = b.size();
        vector<vector<int>> f(n + 1, vector<int>(m + 1));

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                f[i][j] = max(f[i - 1][j], f[i][j - 1]);
                if (a[i - 1] == b[j - 1])
                    f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1);
            }

        return f[n][m];
    }
};

```
##### LeetCode 1036. 逃离大迷宫
```cpp
class Solution {
public:
    unordered_set<string> S;
    int m, n = 1e6;

    string get(vector<int> p) {
        return to_string(p[0]) + ' ' + to_string(p[1]);
    }

    bool bfs(vector<int>& source, vector<int>& target) {
        auto st = S;

        queue<vector<int>> q;
        q.push(source);
        st.insert(get(source));

        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

        int cnt = 1;
        while (q.size()) {
            auto t = q.front();
            q.pop();

            for (int i = 0; i < 4; i ++ ) {
                int x = t[0] + dx[i], y = t[1] + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < n && !st.count(get({x, y}))) {
                    ++ cnt;
                    if (cnt * 2 > m * (m - 1)) return true;
                    if (target[0] == x && target[1] == y) return true;

                    q.push({x, y});
                    st.insert(get({x, y}));
                }
            }
        }

        return false;
    }

    bool isEscapePossible(vector<vector<int>>& blocked, vector<int>& source, vector<int>& target) {
        for(auto& b: blocked) S.insert(get(b));
        m = blocked.size();
        return bfs(source, target) && bfs(target, source);
    }
};
```
##### LeetCode 1037. 有效的回旋镖
```cpp
class Solution {
public:

    int cross(int x1, int y1, int x2, int y2) {
        return x1 * y2 - x2 * y1;
    }

    bool isBoomerang(vector<vector<int>>& points) {
        auto &a = points[0], &b = points[1], &c = points[2];
        return cross(b[0] - a[0], b[1] - a[1], c[0] - a[0], c[1] - a[1]);
    }
};
```
##### LeetCode 1038. 把二叉搜索树转换为累加树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int sum = 0;

    TreeNode* bstToGst(TreeNode* root) {
        if (!root) return NULL;
        bstToGst(root->right);
        sum += root->val;
        root->val = sum;
        bstToGst(root->left);
        return root;
    }
};
```
##### LeetCode 1039. 多边形三角剖分的最低得分
```cpp
class Solution {
public:
    int minScoreTriangulation(vector<int>& w) {
        int n = w.size();
        vector<vector<int>> f(n, vector<int>(n));

        for (int len = 3; len <= n; len ++ )
            for (int i = 0; i + len - 1 < n; i ++ ) {
                int j = i + len - 1;
                if (len == 3) f[i][j] = w[i] * w[j] * w[i + 1];
                else {
                    f[i][j] = 1e9;
                    for (int k = i + 1; k <= j - 1; k ++ )
                        f[i][j] = min(f[i][j], f[i][k] + f[k][j] + w[i] * w[k] * w[j]);
                }
            }

        return f[0][n - 1];
    }
};
```
##### LeetCode 1040. 移动石子直到连续 II
```cpp
class Solution {
public:
    vector<int> numMovesStonesII(vector<int>& stones) {
        vector<int> res(2);
        int n = stones.size();
        sort(stones.begin(), stones.end());
        int m = stones.back() - stones[0] - (n - 1);
        res[1] = m - min(stones[1] - stones[0] - 1, stones[n - 1] - stones[n - 2] - 1);

        res[0] = n;
        for (int i = 0, j = 0; i < n; i ++ ) {
            while (stones[i] - stones[j] + 1 > n) j ++ ;
            m = i - j + 1;

            int r;
            if (m == n - 1 && stones[i] - stones[j] == i - j) r = 2;
            else r = n - m;

            res[0] = min(res[0], r);
        }

        return res;
    }
};
```
##### LeetCode 1041. 困于环中的机器人
```cpp
class Solution {
public:
    bool isRobotBounded(string instructions) {
        int x = 0, y = 0, d = 0;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

        for (auto c: instructions) {
            if (c == 'G') x += dx[d], y += dy[d];
            else if (c == 'L') d = (d + 3) % 4;
            else d = (d + 1) % 4;
        }

        return !x && !y || d;
    }
};
```
##### LeetCode 1042. 不邻接植花
```cpp
class Solution {
public:
    vector<int> gardenNoAdj(int n, vector<vector<int>>& paths) {
        vector<vector<int>> g(n);
        for (auto& p: paths) {
            int a = p[0] - 1, b = p[1] - 1;
            g[a].push_back(b);
            g[b].push_back(a);
        }

        vector<int> res(n);
        for (int i = 0; i < n; i ++ ) {
            bool st[5] = {};
            for (int j: g[i])
                st[res[j]] = true;

            for (int j = 1; j <= 4; j ++ )
                if (!st[j]) {
                    res[i] = j;
                    break;
                }
        }

        return res;
    }
};
```




### 究极班- Week 50（第 1043、1044、1046 ~ 1049、1051 ~ 1054 题）

##### LeetCode 1043. 分隔数组以得到最大和
```cpp
class Solution {
public:
    int maxSumAfterPartitioning(vector<int>& arr, int k) {
        int n = arr.size();
        vector<int> f(n + 1);
        for (int i = 1; i <= n; i ++ )
            for (int j = 1, v = 0; j <= k && j <= i; j ++ ) {
                v = max(v, arr[i - j]);
                f[i] = max(f[i], f[i - j] + v * j);
            }

        return f[n];
    }
};

```
##### LeetCode 1044. 最长重复子串
```cpp
typedef unsigned long long ULL;

const int N = 100010, P = 131;

ULL p[N], h[N];

class Solution {
public:
    int n, start;

    ULL get(int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }

    bool check(int mid) {
        unordered_set<ULL> S;
        for (int i = 1; i + mid - 1 <= n; i ++ ) {
            ULL v = get(i, i + mid - 1);
            if (S.count(v)) {
                start = i;
                return true;
            }
            S.insert(v);
        }

        return false;
    }

    string longestDupSubstring(string s) {
        n = s.size();
        p[0] = 1;
        for (int i = 1; i <= n; i ++ ) {
            p[i] = p[i - 1] * P;
            h[i] = h[i - 1] * P + s[i - 1];
        }

        int l = 0, r = n;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (check(mid)) l = mid;
            else r = mid - 1;
        }

        check(r);
        return s.substr(start - 1, r);
    }
};
```
##### LeetCode 1046. 最后一块石头的重量
```cpp
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        priority_queue<int> heap(stones.begin(), stones.end());
        while (heap.size() > 1) {
            int a = heap.top(); heap.pop();
            int b = heap.top(); heap.pop();
            heap.push(a - b);
        }
        return heap.top();
    }
};
```
##### LeetCode 1047. 删除字符串中的所有相邻重复项
```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        string res;
        for (auto c: s) {
            if (res.size() && res.back() == c) res.pop_back();
            else res += c;
        }
        return res;
    }
};
```
##### LeetCode 1048. 最长字符串链
```cpp
class Solution {
public:
    bool check(string& a, string &b) {
        if (a.size() + 1 != b.size()) return false;

        int i = 0;
        for (auto c: b) {
            if (i < a.size() && a[i] == c)
                i ++ ;
        }

        return i == a.size();
    }

    int longestStrChain(vector<string>& words) {
        sort(words.begin(), words.end(), [](string& a, string& b) {
            return a.size() < b.size();
        });

        int n = words.size();
        vector<int> f(n);

        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            f[i] = 1;
            for (int j = 0; j < i; j ++ ) {
                if (check(words[j], words[i])) {
                    f[i] = max(f[i], f[j] + 1);
                }
            }
            res = max(res, f[i]);
        }

        return res;
    }
};
```
##### LeetCode 1049. 最后一块石头的重量 II
```cpp
const int N = 1510;

int f[N];

class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum = accumulate(stones.begin(), stones.end(), 0);
        int m = sum / 2;
        memset(f, 0, sizeof f);

        for (auto v: stones) {
            for (int j = m; j >= v; j -- )
                f[j] = max(f[j], f[j - v] + v);
        }

        return sum - f[m] * 2;
    }
};
```
##### LeetCode 1051. 高度检查器
```cpp
class Solution {
public:
    int heightChecker(vector<int>& a) {
        auto b = a;
        sort(b.begin(), b.end());
        int res = 0;
        for (int i = 0; i < a.size(); i ++ )
            if (a[i] != b[i])
                res ++ ;
        return res;
    }
};
```
##### LeetCode 1052. 爱生气的书店老板
```cpp
class Solution {
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) {
        int res = 0;
        int n = customers.size();

        for (int i = 0, s = 0; i < n; i ++ ) {
            s += grumpy[i] * customers[i];
            if (i >= minutes) s -= grumpy[i - minutes] * customers[i - minutes];
            res = max(res, s);
        }

        for (int i = 0; i < n; i ++ )
            res += !grumpy[i] * customers[i];

        return res;
    }
};
```
##### LeetCode 1053. 交换一次的先前排列
```cpp
class Solution {
public:
    vector<int> prevPermOpt1(vector<int>& arr) {
        int n = arr.size();
        for (int i = n - 2; i >= 0; i -- ) {
            if (arr[i] > arr[i + 1]) {
                int j = i + 1;
                while (j + 1 < n && arr[j + 1] < arr[i]) j ++ ;
                while (arr[j - 1] == arr[j]) j -- ;
                swap(arr[i], arr[j]);
                return arr;
            }
        }

        return arr;
    }
};

```


##### LeetCode 1054. 距离相等的条形码
```cpp
#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    vector<int> rearrangeBarcodes(vector<int>& barcodes) {
        int n = barcodes.size();
        vector<int> res(n);

        unordered_map<int, int> cnt;
        for (auto x: barcodes) cnt[x] ++ ;

        vector<PII> q;
        for (auto& [k, v]: cnt) q.push_back({v, k});
        sort(q.begin(), q.end(), greater<PII>());

        int k = 0;
        for (auto& p: q) {
            while (p.x -- ) {
                res[k] = p.y;
                k += 2;
                if (k >= n) k = 1;
            }
        }

        return res;
    }
};
```


### 究极班- Week 51(第 1071 ~ 1074、1078 ~ 1081、1089、1090 题)

#####  LeetCode 1071. 字符串的最大公因子
```cpp
class Solution {
public:
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }

    string gcdOfStrings(string a, string b) {
        if (a + b != b + a) return "";
        return a.substr(0, gcd(a.size(), b.size()));
    }
};
```
#####  LeetCode 1072. 按列翻转得到最大值等行数
```cpp
class Solution {
public:
    int maxEqualRowsAfterFlips(vector<vector<int>>& g) {
        unordered_map<string, int> cnt;

        int res = 0;
        for (int i = 0; i < g.size(); i ++ ) {
            string a, b;
            for (auto x: g[i]) {
                a += to_string(x);
                b += to_string(x ^ 1);
            }

            res = max(res, max( ++ cnt[a], ++ cnt[b]));
        }

        return res;
    }
};
```
#####  LeetCode 1073. 负二进制数相加
```cpp
class Solution {
public:
    vector<int> addNegabinary(vector<int>& A, vector<int>& B) {
        reverse(A.begin(), A.end());
        reverse(B.begin(), B.end());

        vector<int> C;
        for (int i = 0, a = 0, b = 0, c = 0; i < A.size() || i < B.size() || a || b; i ++ ) {
            if (a == 1 && b == 2) a = b = 0;
            c = b, b = a, a = 0;
            if (i < A.size()) c += A[i];
            if (i < B.size()) c += B[i];
            C.push_back(c & 1);
            c >>= 1;
            a += c, b += c;
        }

        while (C.size() > 1 && !C.back()) C.pop_back();

        reverse(C.begin(), C.end());
        return C;
    }
};
```
#####  LeetCode 1074. 元素和为目标值的子矩阵数量
```cpp
class Solution {
public:
    int numSubmatrixSumTarget(vector<vector<int>>& g, int target) {
        int n = g.size(), m = g[0].size();
        vector<vector<int>> s(n + 1, vector<int>(m + 1));
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                s[i][j] = s[i - 1][j] + g[i - 1][j - 1];

        int res = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = i; j <= n; j ++ ) {
                unordered_map<int, int> cnt;
                cnt[0] ++ ;
                for (int k = 1, sum = 0; k <= m; k ++ ) {
                    sum += s[j][k] - s[i - 1][k];
                    res += cnt[sum - target];
                    cnt[sum] ++ ;
                }
            }

        return res;
    }
};
```
#####  LeetCode 1078. Bigram 分词
```cpp
class Solution {
public:
    vector<string> findOcurrences(string text, string first, string second) {
        stringstream ssin(text);
        vector<string> strs;
        string str;
        while (ssin >> str) strs.push_back(str);

        vector<string> res;
        for (int i = 0; i + 2 < strs.size(); i ++ )
            if (strs[i] == first && strs[i + 1] == second)
                res.push_back(strs[i + 2]);

        return res;
    }
};
```
#####  LeetCode 1079. 活字印刷
```cpp
class Solution {
public:
    int numTilePossibilities(string str) {
        sort(str.begin(), str.end());
        return dfs(str, 0) - 1;
    }

    int dfs(string& str, int u) {
        int res = 1;
        if (u == str.size()) return res;

        for (int i = 0; i < str.size(); i ++ ) {
            if (i && str[i] == str[i - 1]) continue;
            if (str[i] == ' ') continue;
            char c = str[i];
            str[i] = ' ';
            res += dfs(str, u + 1);
            str[i] = c;
        }
        return res;
    }
};
```
#####  LeetCode 1080. 根到叶路径上的不足节点
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* sufficientSubset(TreeNode* root, int limit) {
        dfs(root, 0, limit);
        return root;
    }

    void dfs(TreeNode* &root, int sum, int limit) {
        sum += root->val;
        if (!root->left && !root->right) {
            if (sum < limit) root = NULL;
        } else {
            if (root->left) dfs(root->left, sum, limit);
            if (root->right) dfs(root->right, sum, limit);
            if (!root->left && !root->right) root = NULL;
        }
    }
};
```
#####  LeetCode 1081. 不同字符的最小子序列
```cpp
class Solution {
public:
    string smallestSubsequence(string s) {
        string stk;
        unordered_map<char, bool> ins;
        unordered_map<char, int> last;
        for (int i = 0; i < s.size(); i ++ ) last[s[i]] = i;

        for (int i = 0; i < s.size(); i ++ ) {
            if (ins[s[i]]) continue;
            while (stk.size() && stk.back() > s[i] && last[stk.back()] > i) {
                ins[stk.back()] = false;
                stk.pop_back();
            }
            stk += s[i];
            ins[s[i]] = true;
        }

        return stk;
    }
};
```
#####  LeetCode 1089. 复写零
```cpp
class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
        int n = arr.size();
        arr.push_back(0);

        for (int i = 0, cnt = 0; i < n; i ++ ) {
            if (arr[i]) cnt ++ ;
            else cnt += 2;
            if (cnt >= n) {
                for (int j = cnt - 1; i >= 0; i -- ) {
                    if (arr[i]) arr[j -- ] = arr[i];
                    else {
                        arr[j -- ] = 0;
                        arr[j -- ] = 0;
                    }
                }

                break;
            }
        }

        arr.pop_back();
    }
};
```
#####  LeetCode 1090. 受标签影响的最大值
```cpp
#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    int largestValsFromLabels(vector<int>& values, vector<int>& labels, int numWanted, int useLimit) {
        int n = values.size();

        vector<PII> q;
        for (int i = 0; i < n; i ++ ) q.push_back({values[i], labels[i]});

        sort(q.begin(), q.end(), greater<PII>());

        int res = 0;
        unordered_map<int, int> cnt;
        for (int i = 0; i < n && numWanted > 0; i ++ ) {
            if (cnt[q[i].y] < useLimit) {
                res += q[i].x;
                cnt[q[i].y] ++ ;
                numWanted -- ;
            }
        }

        return res;
    }
};
```


### 究极班- Week 52（第 1091 ~ 1096、1103 ~ 1106 题）

#####  LeetCode 1091. 二进制矩阵中的最短路径
```cpp
#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& g) {
        if (g[0][0]) return -1;
        int n = g.size();
        vector<vector<int>> dist(n, vector<int>(n, -1));
        dist[0][0] = 1;
        queue<PII> q;
        q.push({0, 0});

        int dx[] = {-1, -1, -1, 0, 1, 1, 1, 0};
        int dy[] = {-1, 0, 1, 1, 1, 0, -1, -1};

        while (q.size()) {
            auto t = q.front();
            q.pop();

            for (int i = 0; i < 8; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < n && g[x][y] == 0 && dist[x][y] == -1) {
                    dist[x][y] = dist[t.x][t.y] + 1;
                    q.push({x, y});
                }
            }
        }

        return dist[n - 1][n - 1];
    }
};
```
#####  LeetCode 1092. 最短公共超序列
```cpp
class Solution {
public:
    string shortestCommonSupersequence(string a, string b) {
        int n = a.size(), m = b.size(), INF = 1e8;
        vector<vector<int>> f(n + 1, vector<int>(m + 1, INF));
        for (int i = 0; i <= n; i ++ ) f[i][0] = 0;
        for (int i = 1; i <= m; i ++ ) f[0][i] = i;

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                f[i][j] = min(f[i][j - 1] + 1, f[i - 1][j]);
                if (a[i - 1] == b[j - 1])
                    f[i][j] = min(f[i][j], f[i - 1][j - 1]);
            }

        string res;
        for (int i = n, j = m; i >= 0;) {
            if (!i || !j || a[i - 1] != b[j - 1]) {
                if (j && f[i][j] == f[i][j - 1] + 1) {
                    res += b[ -- j];
                } else {
                    if (i) res += a[ -- i];
                    else i -- ;
                }
            } else {
                if (f[i][j] == f[i][j - 1] + 1) {
                    res += b[ -- j];
                } else if (f[i][j] == f[i - 1][j]) {
                    res += a[ -- i];
                } else {
                    res += a[ -- i];
                    j -- ;
                }
            }
        }

        reverse(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode 1093. 大样本统计
```cpp
class Solution {
public:
    int get(vector<int>& count, int cnt) {
        int res = 0;
        for (int i = 0; i < 256; i ++ ) {
            res += count[i];
            if (res >= cnt)
                return i;
        }

        return -1;
    }

    vector<double> sampleStats(vector<int>& count) {
        vector<double> res(5);
        for (int i = 0;; i ++ )
            if (count[i]) {
                res[0] = i;
                break;
            }

        for (int i = 255;; i -- ) {
            if (count[i]) {
                res[1] = i;
                break;
            }
        }

        double sum = 0;
        int cnt = 0;
        for (int i = 0; i < 256; i ++ ) {
            sum += (double)i * count[i];
            cnt += count[i];
        }
        res[2] = sum / cnt;

        int t = 0;
        for (int i = 0; i < 256; i ++ )
            if (count[i] > count[t])
                t = i;
        res[4] = t;

        if (cnt % 2) res[3] = get(count, cnt / 2 + 1);
        else res[3] = (get(count, cnt / 2) + get(count, cnt / 2 + 1)) / 2.0;

        return res;
    }
};
```
#####  LeetCode 1094. 拼车
```cpp
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        vector<int> b(1001);
        for (auto& t: trips) {
            b[t[1]] += t[0];
            b[t[2]] -= t[0];
        }

        int sum = 0;
        for (int i = 0; i <= 1000; i ++ ) {
            sum += b[i];
            if (sum > capacity) return false;
        }

        return true;
    }
};
```
#####  LeetCode 1095. 山脉数组中查找目标值
```cpp
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class MountainArray {
 *   public:
 *     int get(int index);
 *     int length();
 * };
 */

class Solution {
public:
    int findInMountainArray(int target, MountainArray &mountainArr) {
        int n = mountainArr.length();
        int l = 0, r = n - 1;

        while (l < r) {
            int mid = l + r >> 1;
            if (mountainArr.get(mid) > mountainArr.get(mid + 1)) r = mid;
            else l = mid + 1;
        }

        int M = r;
        l = 0, r = M;
        while (l < r) {
            int mid = l + r >> 1;
            if (mountainArr.get(mid) >= target) r = mid;
            else l = mid + 1;
        }
        if (mountainArr.get(r) == target) return r;

        l = M + 1, r = n - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (mountainArr.get(mid) <= target) r = mid;
            else l = mid + 1;
        }

        if (mountainArr.get(r) == target) return r;
        return -1;
    }
};
```
#####  LeetCode 1096. 花括号展开 II
```cpp
typedef set<string> SS;

class Solution {
public:
    string str;
    int k;

    SS add(SS& A, SS& B) {
        SS C(A.begin(), A.end());
        for (auto& x: B)
            if (x.size())
                C.insert(x);
        return C;
    }

    SS mul(SS& A, SS& B) {
        SS C;
        for (auto& x: A)
            for (auto& y: B)
                C.insert(x + y);
        return C;
    }

    SS dfs() {
        SS A, B;
        B.insert("");

        while (k < str.size() && str[k] != '}') {
            if (str[k] == ',') {
                k ++ ;
                continue;
            } else if (str[k] == '{') {
                bool is_add = true;
                if (!k || str[k - 1] != ',') is_add = false;

                k ++ ;  // 跳过 {
                auto C = dfs();
                k ++ ;  // 跳过 }

                if (is_add) {
                    A = add(A, B);
                    B = C;
                } else {
                    B = mul(B, C);
                }
            } else {
                bool is_add = true;
                if (!k || str[k - 1] != ',') is_add = false;

                string s;
                while (k < str.size() && str[k] >= 'a' && str[k] <= 'z')
                    s += str[k ++ ];

                SS C;
                C.insert(s);

                if (is_add) {
                    A = add(A, B);
                    B = C;
                } else {
                    B = mul(B, C);
                }
            }
        }

        return add(A, B);
    }

    vector<string> braceExpansionII(string expression) {
        str = expression;
        k = 0;
        auto res = dfs();
        return vector<string>(res.begin(), res.end());
    }
};
```
#####  LeetCode 1103. 分糖果 II
```cpp
class Solution {
public:
    vector<int> distributeCandies(int m, int n) {
        vector<int> res(n);

        for (int i = 0, j = 1; m; i = (i + 1) % n, j ++ ) {
            int t = min(j, m);
            res[i] += t;
            m -= t;
        }

        return res;
    }
};
```
#####  LeetCode 1104. 二叉树寻路
```cpp
class Solution {
public:
    vector<int> pathInZigZagTree(int label) {
        vector<int> res;
        while (label) res.push_back(label), label >>= 1;

        reverse(res.begin(), res.end());

        for (int i = res.size() % 2; i < res.size(); i += 2) {
            res[i] ^= ((1 << i) - 1);
        }

        return res;
    }
};
```
#####  LeetCode 1105. 填充书架
```cpp
class Solution {
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        int n = books.size();
        vector<int> f(n + 1, 1e8);
        f[0] = 0;

        for (int i = 1; i <= n; i ++ ) {
            int s = 0, h = 0;
            for (int j = i; j; j -- ) {
                s += books[j - 1][0];
                if (s > shelfWidth) break;
                h = max(h, books[j - 1][1]);
                f[i] = min(f[i], f[j - 1] + h);
            }
        }

        return f[n];
    }
};
```
#####  LeetCode 1106. 解析布尔表达式
```cpp
class Solution {
public:
    string str;
    int k;

    bool dfs() {
        if (str[k] == 't') {
            k ++ ;
            return true;
        }
        if (str[k] == 'f') {
            k ++ ;
            return false;
        }

        char op = str[k];
        k += 2;

        bool res = true;
        if (op == '|') res = false;

        while (str[k] != ')') {
            if (str[k] == ',') {
                k ++ ;
            } else {
                auto t = dfs();
                if (op == '|') res |= t;
                else res &= t;
            }
        }

        k ++ ;

        if (op == '!') res = !res;
        return res;
    }

    bool parseBoolExpr(string expression) {
        str = expression;
        k = 0;
        return dfs();
    }
};
```



### 究极班- Week 53 (第 1108 ~ 1111、1122 ~ 1125、1128 ~ 1131、1137 ~ 1140、1143 ~ 1147 题)

#####  LeetCode 1108. IP 地址无效化
```cpp
class Solution {
public:
    string defangIPaddr(string address) {
        string res;
        for (auto c: address)
            if (c == '.') res += "[.]";
            else res += c;
        return res;
    }
};
```
#####  LeetCode 1109. 航班预订统计
```cpp
class Solution {
public:
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
        vector<int> b(n);
        for (auto& p: bookings) {
            int l = p[0] - 1, r = p[1] - 1, c = p[2];
            b[l] += c;
            if (r + 1 < n)
                b[r + 1] -= c;
        }

        for (int i = 1; i < n; i ++ ) b[i] += b[i - 1];
        return b;
    }
};

```
#####  LeetCode 1110. 删点成林
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<TreeNode*> ans;
    unordered_set<int> hash;

    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        for (auto val: to_delete) hash.insert(val);
        dfs(root, false);
        return ans;
    }

    void dfs(TreeNode*& root, bool has_father) {
        if (hash.count(root->val)) {
            if (root->left) dfs(root->left, false);
            if (root->right) dfs(root->right, false);
            root = nullptr;
        } else {
            if (!has_father) ans.push_back(root);
            if (root->left) dfs(root->left, true);
            if (root->right) dfs(root->right, true);
        }
    }
};

```
#####  LeetCode 1111. 有效括号的嵌套深度
```cpp
class Solution {
public:
    vector<int> maxDepthAfterSplit(string seq) {
        vector<int> res;
        int cnt = 0;
        for (auto c: seq) {
            if (c == '(') {
                cnt ++ ;
                res.push_back(cnt % 2);
            } else {
                res.push_back(cnt % 2);
                cnt -- ;
            }
        }
        return res;
    }
};
```
#####  LeetCode 1122. 数组的相对排序
```cpp
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        unordered_map<int, int> hash;
        for (int i = 0; i < arr2.size(); i ++ )
            hash[arr2[i]] = i - arr2.size();

        sort(arr1.begin(), arr1.end(), [&](int a, int b) {
            if (hash[a] == hash[b]) return a < b;
            return hash[a] < hash[b];
        });

        return arr1;
    }
};
```
#####  LeetCode 1123. 最深叶节点的最近公共祖先
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    pair<TreeNode*, int> dfs(TreeNode* root) {
        if (!root) return {NULL, 0};
        auto left = dfs(root->left), right = dfs(root->right);
        if (left.second == right.second) return {root, left.second + 1};
        if (left.second > right.second) return {left.first, left.second + 1};
        return {right.first, right.second + 1};
    }

    TreeNode* lcaDeepestLeaves(TreeNode* root) {
        return dfs(root).first;
    }
};
```
#####  LeetCode 1124. 表现良好的最长时间段
```cpp
class Solution {
public:
    int longestWPI(vector<int>& hours) {
        unordered_map<int, int> hash;
        int res = 0, sum = 0;
        for (int i = 0; i < hours.size(); i ++ ) {
            int t = hours[i];
            if (t > 8) sum ++ ;
            else sum -- ;

            if (sum > 0) res = i + 1;
            else {
                if (hash.count(sum - 1))
                    res = max(res, i - hash[sum - 1]);
                if (!hash.count(sum))
                    hash[sum] = i;
            }
        }

        return res;
    }
};
```
#####  LeetCode 1125. 最小的必要团队
```cpp
class Solution {
public:
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
        int n = req_skills.size(), m = people.size();
        vector<int> f(1 << n, n + 1), g(m);

        vector<pair<int, int>> path(1 << n);

        int id = 0;
        unordered_map<string, int> hash;
        for (int i = 0; i < req_skills.size(); i ++ )
            hash[req_skills[i]] = i;

        for (int i = 0; i < people.size(); i ++ ) {
            for (auto& s: people[i])
                g[i] |= 1 << hash[s];
        }

        f[0] = 0;
        for (int i = 0; i < 1 << n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                int& v = f[i | g[j]];
                if (v > f[i] + 1) {
                    path[i | g[j]] = {i, j};
                    v = f[i] + 1;
                }
            }

        vector<int> res;
        for (int state = (1 << n) - 1; state;) {
            res.push_back({path[state].second});
            state = path[state].first;
        }

        return res;
    }
};
```
#####  LeetCode 1128. 等价多米诺骨牌对的数量
```cpp
class Solution {
public:
    int numEquivDominoPairs(vector<vector<int>>& dominoes) {
        unordered_map<int, int> hash;

        int res = 0;
        for (auto& p: dominoes) {
            int x = p[0], y = p[1];
            if (x > y) swap(x, y);
            int key = x * 10 + y;
            res += hash[key];
            hash[key] ++ ;
        }

        return res;
    }
};
```
#####  LeetCode 1129. 颜色交替的最短路径
```cpp
#define x first
#define y second

class Solution {
public:
    vector<int> shortestAlternatingPaths(int n, vector<vector<int>>& redEdges, vector<vector<int>>& blueEdges) {
        const int INF = 1e8;
        vector<vector<pair<int, int>>> g(n);
        for (auto& p: redEdges) g[p[0]].push_back({p[1], 0});
        for (auto& p: blueEdges) g[p[0]].push_back({p[1], 1});

        vector<vector<int>> dist(n, vector<int>(2, INF));
        dist[0][0] = dist[0][1] = 0;
        queue<pair<int, int>> q;
        q.push({0, 0}), q.push({0, 1});

        while (q.size()) {
            auto t = q.front();
            q.pop();

            for (auto& p: g[t.x]) {
                if (t.y != p.y && dist[p.x][p.y] > dist[t.x][t.y] + 1) {
                    dist[p.x][p.y] = dist[t.x][t.y] + 1;
                    q.push(p);
                }
            }
        }

        vector<int> res;
        for (int i = 0; i < n; i ++ ) {
            res.push_back(min(dist[i][0], dist[i][1]));
            if (res[i] == INF) res[i] = -1;
        }

        return res;
    }
};
```
#####  LeetCode 1130. 叶值的最小代价生成树
```cpp
class Solution {
public:
    int mctFromLeafValues(vector<int>& arr) {
        stack<int> stk;
        int res = 0;
        for (auto x: arr) {
            while (stk.size() && x >= stk.top()) {
                int t = stk.top();
                stk.pop();
                res += t * min(x, stk.empty() ? 100 : stk.top());
            }
            stk.push(x);
        }

        while (stk.size() > 1) {
            int t = stk.top();
            stk.pop();
            res += t * stk.top();
        }
        return res;
    }
};
```
#####  LeetCode 1131. 绝对值表达式的最大值
```cpp
class Solution {
public:
    vector<int> a, b;

    int work(int x, int y) {
        int res = 0, mx = -1e8;
        for (int i = 0; i < a.size(); i ++ ) {
            res = max(res, a[i] * x + b[i] * y + i + mx);
            mx = max(mx, -a[i] * x - b[i] * y - i);
        }
        return res;
    }

    int maxAbsValExpr(vector<int>& arr1, vector<int>& arr2) {
        a = arr1, b = arr2;
        return max({work(1, 1), work(1, -1), work(-1, 1), work(-1, -1)});
    }
};
```
#####  LeetCode 1137. 第 N 个泰波那契数
```cpp
class Solution {
public:
    int tribonacci(int n) {
        int a = 0, b = 1, c = 1;
        while (n -- ) {
            long long d = (long long)a + b + c;
            a = b, b = c, c = d;
        }
        return a;
    }
};
```
#####  LeetCode 1138. 字母板上的路径
```cpp
#define x first
#define y second

using PII = pair<int, int>;

class Solution {
public:
    PII get(char c) {
        int t = c - 'a';
        return {t / 5, t % 5};
    }

    string alphabetBoardPath(string target) {
        string res;
        PII p(0, 0);
        for (auto c: target) {
            auto t = get(c);
            if (c == 'z') {
                if (t.y > p.y) res += string(t.y - p.y, 'R');
                else res += string(p.y - t.y, 'L');
                if (t.x > p.x) res += string(t.x - p.x, 'D');
                else res += string(p.x - t.x, 'U');
            } else {
                if (t.x > p.x) res += string(t.x - p.x, 'D');
                else res += string(p.x - t.x, 'U');
                if (t.y > p.y) res += string(t.y - p.y, 'R');
                else res += string(p.y - t.y, 'L');
            }
            res += '!';
            p = t;
        }
        return res;
    }
};
```
#####  LeetCode 1139. 最大的以 1 为边界的正方形
```cpp
class Solution {
public:
    vector<vector<int>> s;

    int get(int x1, int y1, int x2, int y2) {
        return s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1];
    }

    int largest1BorderedSquare(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size();
        s = vector<vector<int>>(n + 1, vector<int>(m + 1));
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                s[i][j] = s[i][j - 1] + s[i - 1][j] - s[i - 1][j - 1] + g[i - 1][j - 1];

        for (int len = min(n, m); len > 1; len -- ) {
            for (int i = 1; i + len - 1 <= n; i ++ ) {
                for (int j = 1; j + len - 1 <= m; j ++ ) {
                    int a = i, b = j, c = i + len - 1, d = j + len - 1;
                    if (get(a, b, c, d) - get(a + 1, b + 1, c - 1, d - 1) == 4 * (len - 1)) {
                        return len * len;
                    }
                }
            }
        }

        if (s[n][m] > 0) return 1;

        return 0;
    }
};
```
#####  LeetCode 1140. 石子游戏 II
```cpp
class Solution {
public:
    int stoneGameII(vector<int>& piles) {
        int n = piles.size();
        vector<vector<int>> f(n + 2, vector<int>(n + 1));
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ )
            s[i] = s[i - 1] + piles[i - 1];

        for (int i = n; i; i -- ) {
            for (int j = 1; j <= n; j ++ ) {
                for (int k = 1; i + k - 1 <= n && k <= j * 2; k ++ ) {
                    f[i][j] = max(f[i][j], s[n] - s[i - 1] - f[i + k][max(k, j)]);
                }
            }
        }

        return f[1][1];
    }
};
```
#####  LeetCode 1143. 最长公共子序列
```cpp

```
#####  LeetCode 1144. 递减元素使数组呈锯齿状
```cpp
class Solution {
public:
    int work(vector<int>& nums, int start) {
        int res = 0;
        for (int i = start; i < nums.size(); i += 2) {
            int t = nums[i];
            if (i - 1 >= 0) t = min(t, nums[i - 1] - 1);
            if (i + 1 < nums.size()) t = min(t, nums[i + 1] - 1);
            res += nums[i] - t;
        }
        return res;
    }

    int movesToMakeZigzag(vector<int>& nums) {
        return min(work(nums, 0), work(nums, 1));
    }
};
```
#####  LeetCode 1145. 二叉树着色游戏
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int ans = 0;
    int n;

    bool btreeGameWinningMove(TreeNode* root, int _n, int x) {
        n = _n;
        dfs(root, x);
        return ans > n - ans;
    }

    int dfs(TreeNode* root, int x) {
        if (!root) return 0;
        if (root->val == x) {
            int left = dfs(root->left, x);
            int right = dfs(root->right, x);
            ans = max({ans, left, right, n - left - right - 1});
            return left + right + 1;
        } else {
            return dfs(root->left, x) + dfs(root->right, x) + 1;
        }
    }
};
```
#####  LeetCode 1146. 快照数组
```cpp
class SnapshotArray {
public:
    unordered_map<int, vector<pair<int, int>>> hash;
    int id = 0;

    SnapshotArray(int length) {

    }

    void set(int index, int val) {
        if (hash.count(index) && hash[index].back().first == id) {
            hash[index].back().second = val;
        } else {
            hash[index].push_back({id, val});
        }
    }

    int snap() {
        return id ++ ;
    }

    int get(int index, int snap_id) {
        if (!hash.count(index) || hash[index][0].first > snap_id) {
            return 0;
        } else {
            const auto& items = hash[index];
            int l = 0, r = items.size() - 1;
            while (l < r) {
                int mid = l + r + 1 >> 1;
                if (items[mid].first <= snap_id) l = mid;
                else r = mid - 1;
            }
            return items[r].second;
        }
    }
};

/**
 * Your SnapshotArray object will be instantiated and called as such:
 * SnapshotArray* obj = new SnapshotArray(length);
 * obj->set(index,val);
 * int param_2 = obj->snap();
 * int param_3 = obj->get(index,snap_id);
 */

```
#####  LeetCode 1147. 段式回文
```cpp
class Solution {
public:
    int longestDecomposition(string s) {
        if (s.empty()) return 0;
        for (int i = 1; i * 2 <= s.size(); i ++ ) {
            if (s.substr(0, i) == s.substr(s.size() - i)) {
                return 2 + longestDecomposition(s.substr(i, s.size() - i * 2));
            }
        }
        return 1;
    }
};
```




### 究极班- Week 54(第 1154 ~ 1157、1160 ~ 1163、1169 ~ 1172、1175、1177、1178、1184 ~ 1187、1189 题)

#####  LeetCode 1154. 一年中的第几天
```cpp
class Solution {
public:
    int is_leap(int year) {
        if (year % 4 == 0 && year % 100 || year % 400 == 0)
            return 1;
        return 0;
    }

    int dayOfYear(string date) {
        const int days[13] = {
            0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31
        };
        int year, month, day;
        sscanf(date.c_str(), "%d-%d-%d", &year, &month, &day);
        int res = 0;
        for (int i = 1; i < month; i ++ ) {
            res += days[i];
            if (i == 2) res += is_leap(year);
        }
        return res + day;
    }
};
```
#####  LeetCode 1155. 掷骰子的N种方法
```cpp
class Solution {
public:
    int numRollsToTarget(int n, int k, int target) {
        const int MOD = 1e9 + 7;
        vector<int> f(target + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ ) {
            for (int j = target; j >= 0; j -- ) {
                f[j] = 0;
                for (int u = 1; u <= k && u <= j; u ++ ) {
                    f[j] = (f[j] + f[j - u]) % MOD;
                }
            }
        }
        return f[target];
    }
};
```
#####  LeetCode 1156. 单字符重复子串的最大长度
```cpp
class Solution {
public:
    int maxRepOpt1(string s) {
        vector<int> p[26];
        for (int i = 0; i < s.size(); i ++ ) {
            p[s[i] - 'a'].push_back(i);
        }

        int res = 0;
        for (auto q: p) {
            for (int i = 0, j = 0; i < q.size(); i ++ ) {  // 中间有一个空位
                while (q[i] - q[j] > i - j + 1) j ++ ;
                if ((i + 1 < q.size() || j)) {
                    res = max(res, q[i] - q[j] + 1);
                }
            }

            for (int i = 0, j = 0; i < q.size(); i ++ ) {  // 中间有一个空位
                while (q[i] - q[j] > i - j) j ++ ;
                int t = q[i] - q[j] + 1;
                if (i + 1 < q.size() || j) t ++ ;
                res = max(res, t);
            }
        }

        return res;
    }
};

```
#####  LeetCode 1157. 子数组中占绝大多数的元素
```cpp
class MajorityChecker {
public:
    int n, len;
    unordered_map<int, int> cnt;
    unordered_map<int, vector<int>> s;
    vector<int> a;

    MajorityChecker(vector<int>& arr) {
        n = arr.size();
        len = sqrt(n * 2);
        a = arr;

        for (auto x: a) cnt[x] ++ ;
        for (auto& [k, v]: cnt) {
            if (v > len) {
                s[k] = vector<int>(n + 1);
                for (int i = 1; i <= n; i ++ ) {
                    s[k][i] = s[k][i - 1];
                    if (a[i - 1] == k) s[k][i] ++ ;
                }
            }
        }
    }

    int query(int left, int right, int threshold) {
        if (right - left + 1 <= len) {
            cnt.clear();
            for (int i = left; i<= right; i ++ ) {
                if ( ++ cnt[a[i]] >= threshold) {
                    return a[i];
                }
            }
        } else {
            for (auto& [k, v]: s) {
                if (v[right + 1] - v[left] >= threshold) {
                    return k;
                }
            }
        }

        return -1;
    }
};

/**
 * Your MajorityChecker object will be instantiated and called as such:
 * MajorityChecker* obj = new MajorityChecker(arr);
 * int param_1 = obj->query(left,right,threshold);
 */

```
#####  LeetCode 1160. 拼写单词
```cpp
class Solution {
public:
    int countCharacters(vector<string>& words, string chars) {
        unordered_map<char, int> hw, hc;
        for (auto c: chars) hc[c] ++ ;

        int res = 0;
        for (auto& w: words) {
            hw.clear();
            bool flag = true;
            for (auto c: w) {
                if ( ++ hw[c] > hc[c]) {
                    flag = false;
                    break;
                }
            }

            if (flag) res += w.size();
        }

        return res;
    }
};

```
#####  LeetCode 1161. 最大层内元素和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> sum;

    void dfs(TreeNode* root, int depth) {
        if (!root) return;
        if (depth > sum.size()) sum.push_back(0);
        sum[depth - 1] += root->val;
        dfs(root->left, depth + 1);
        dfs(root->right, depth + 1);
    }

    int maxLevelSum(TreeNode* root) {
        dfs(root, 1);

        int mx = -2e9, res = 0;
        for (int i = 0; i < sum.size(); i ++ ) {
            if (mx < sum[i]) {
                mx = sum[i];
                res = i + 1;
            }
        }

        return res;
    }
};

```
#####  LeetCode 1162. 地图分析
```cpp
#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    int maxDistance(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size(), INF = 1e8;
        vector<vector<int>> dist(n, vector<int>(m, INF));
        queue<PII> q;
        for (int i = 0; i < n; i ++ ) 
            for (int j = 0; j < m; j ++ )
                if (g[i][j]) {
                    dist[i][j] = 0;
                    q.push({i, j});
                }

        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        while (q.size()) {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < 4; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i];
                if (x < 0 || x >= n || y < 0 || y >= m) continue;
                if (dist[x][y] >dist[t.x][t.y] + 1) {
                    dist[x][y] = dist[t.x][t.y] + 1;
                    q.push({x, y});
                }
            }
        }

        int res = -1;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!g[i][j])
                    res = max(res, dist[i][j]);

        if (res == INF) res = -1;
        return res;
    }
};

```
#####  LeetCode 1163. 按字典序排在最后的子串
```cpp
typedef unsigned long long ULL;
const int N = 400010, P = 13331;

ULL p[N], h[N];

class Solution {
public:
    int n;

    ULL get(int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }

    bool cmp(int a, int b, string& s) {
        if (a > b) return !cmp(b, a, s);

        int l = 0, r = n - b + 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (get(a, a + mid - 1) == get(b, b + mid - 1)) l = mid;
            else r = mid - 1;
        }

        if (r == n - b + 1) return false;
        return s[a + r - 1] < s[b + r - 1];
    }

    string lastSubstring(string s) {
        n = s.size();
        p[0] = 1;
        for (int i = 1; i <= n; i ++ ) {
            p[i] = p[i - 1] * P;
            h[i] = h[i - 1] * P + s[i - 1];
        }

        int res = 1;
        for (int i = 2; i <= n; i ++ ) {
            if (cmp(res, i, s)) {
                res = i;
            }
        }

        return s.substr(res - 1);
    }
};

```
#####  LeetCode 1169. 查询无效交易
```cpp
struct Trans {
    string name, city, str;
    int time, price;
    bool valid;

    Trans(string _str) {
        str = _str;
        int k = 0;
        while (str[k] != ',') name += str[k ++ ];
        k ++ ;
        string s;
        while (str[k] != ',') s += str[k ++ ];
        k ++ ;
        time = stoi(s);
        s.clear();
        while (str[k] != ',') s += str[k ++ ];
        k ++ ;
        price = stoi(s);
        while (k < str.size()) city += str[k ++ ];
        if (price > 1000) valid = false;
        else valid = true;
    }

    bool operator< (const Trans& t) {
        return time < t.time;
    }
};

class Solution {
public:
    vector<string> invalidTransactions(vector<string>& transactions) {
        unordered_map<string, vector<Trans>> hash;
        for (auto& t: transactions) {
            auto trans = Trans(t);
            hash[trans.name].push_back(trans);
        }

        for (auto& [k, v]: hash) {
            sort(v.begin(), v.end());
            unordered_map<string, int> cnt;
            for (int i = 0, j = 0, tot = 0; i < v.size(); i ++ ) {
                if ( ++ cnt[v[i].city] == 1) tot ++ ;
                while (v[i].time - v[j].time > 60) {
                    if ( -- cnt[v[j].city] == 0) {
                        tot -- ;
                    }
                    j ++ ;
                }
                if (tot > 1) v[i].valid = false;
            }

            cnt.clear();
            reverse(v.begin(), v.end());
            for (int i = 0, j = 0, tot = 0; i < v.size(); i ++ ) {
                if ( ++ cnt[v[i].city] == 1) tot ++ ;
                while (v[j].time - v[i].time > 60) {
                    if ( -- cnt[v[j].city] == 0) {
                        tot -- ;
                    }
                    j ++ ;
                }
                if (tot > 1) v[i].valid = false;
            }
        }

        vector<string> res;
        for (auto& [k, v]: hash) {
            for (auto& t: v) {
                if (!t.valid) {
                    res.push_back(t.str);
                }
            }
        }

        return res;
    }
};

```
#####  LeetCode 1170. 比较字符串最小字母出现频次
```cpp
class Solution {
public:
    int f(string& s) {
        map<char, int> cnt;
        for (auto c: s) cnt[c] ++ ;
        return cnt.begin()->second;
    }

    vector<int> numSmallerByFrequency(vector<string>& queries, vector<string>& words) {
        int s[11] = {0};
        for (auto& w: words) s[f(w)] ++ ;
        for (int i = 1; i <= 10; i ++ ) s[i] += s[i - 1];

        vector<int> res;
        for (auto& q: queries) res.push_back(s[10] - s[f(q)]);

        return res;
    }
};
```
#####  LeetCode 1171. 从链表中删去总和值为零的连续节点
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeZeroSumSublists(ListNode* head) {
        unordered_map<int, ListNode*> hash;
        auto dummy = new ListNode(0, head);
        hash[0] = dummy;
        int sum = 0;
        for (auto p = head; p; p = p->next) {
            sum += p->val;
            if (hash.count(sum)) {
                int cur = sum;
                for (auto q = hash[sum]->next; q != p; q = q->next) {
                    cur += q->val;
                    hash.erase(cur);
                }
                hash[sum]->next = p->next;
            } else {
                hash[sum] = p;
            }
        }
        return dummy->next;
    }
};

```
#####  LeetCode 1172. 餐盘栈
```cpp
class DinnerPlates {
public:
    vector<stack<int>> stks;
    priority_queue<int, vector<int>, greater<int>> heap;
    int last = -1, capacity;

    DinnerPlates(int capacity) {
        this->capacity = capacity;
    }

    void push(int val) {
        if (heap.empty()) {
            int id = stks.size();
            stks.push_back(stack<int>());
            heap.push(id);
        }
        auto& stk = stks[heap.top()];
        stk.push(val);
        last = max(last, heap.top());
        if (stk.size() == capacity) heap.pop();
    }

    int pop() {
        return popAtStack(last);
    }

    int popAtStack(int index) {
        if (index == -1 || index > last) return -1;
        auto& stk = stks[index];
        if (stk.empty()) return -1;
        int res = stk.top();
        stk.pop();
        if (stk.size() == capacity - 1) heap.push(index);
        while (last >= 0 && stks[last].empty()) last -- ;
        return res;
    }
};

/**
 * Your DinnerPlates object will be instantiated and called as such:
 * DinnerPlates* obj = new DinnerPlates(capacity);
 * obj->push(val);
 * int param_2 = obj->pop();
 * int param_3 = obj->popAtStack(index);
 */

```
#####  LeetCode 1175. 质数排列
```cpp
typedef long long LL;

const int MOD = 1e9 + 7;

class Solution {
public:
    LL fact(int n) {
        int res = 1;
        for (int i = 1; i <= n; i ++ ) {
            res = (LL)res * i % MOD;
        }
        return res;
    }

    int numPrimeArrangements(int n) {
        int cnt = 0;
        for (int i = 2; i <= n; i ++ ) {
            bool is_prime = true;
            for (int j = 2; j * j <= i; j ++ ) {
                if (i % j == 0) {
                    is_prime = false;
                    break;
                }
            }
            if (is_prime) cnt ++ ;
        }
        return fact(n - cnt) * fact(cnt) % MOD;
    }
};

```
#####  LeetCode 1177. 构建回文串检测
```cpp
class Solution {
public:
    vector<bool> canMakePaliQueries(string str, vector<vector<int>>& queries) {
        int n = str.size();
        vector<vector<int>> s(26, vector<int>(n + 1));
        for (int i = 0; i < 26; i ++ ) {
            for (int j = 1; j <= n; j ++ ) {
                s[i][j] = s[i][j - 1];
                if (i + 'a' == str[j - 1]) s[i][j] ++ ;
            }
        }

        vector<bool> res;
        for (auto& q: queries) {
            int l = q[0] + 1, r = q[1] + 1, k = q[2];
            int cnt = 0;
            for (int i = 0; i < 26; i ++ ) {
                if ((s[i][r] - s[i][l - 1]) % 2)
                    cnt ++ ;
            }
            res.push_back(cnt / 2 <= k);
        }

        return res;
    }
};

```
#####  LeetCode 1178. 猜字谜
```cpp
class Solution {
public:
    vector<int> findNumOfValidWords(vector<string>& words, vector<string>& puzzles) {
        unordered_map<int, int> hash;
        for (auto& w: words) {
            int state = 0;
            for (auto c: w) {
                state |= 1 << c - 'a';
            }
            hash[state] ++ ;
        }

        vector<int> res;
        for (auto& p: puzzles) {
            int cnt = 0;
            for (int i = 0; i < 1 << 6; i ++ ) {
                int state = 1 << p[0] - 'a';
                for (int j = 0; j < 6; j ++ ) {
                    if (i >> j & 1) {
                        state |= 1 << p[j + 1] - 'a';
                    }
                }
                cnt += hash[state];
            }
            res.push_back(cnt);
        }

        return res;
    }
};

```
#####  LeetCode 1184. 公交站间的距离
```cpp
class Solution {
public:
    int distanceBetweenBusStops(vector<int>& d, int a, int b) {
        if (a > b) swap(a, b);
        int s1 = 0, sum = 0;
        for (auto x: d) sum += x;
        for (int i = a; i < b; i ++ ) s1 += d[i];
        return min(s1, sum - s1);
    }
};
```
#####  LeetCode 1185. 一周中的第几天
```cpp
class Solution {
public:
    const int months[13] = {
        0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31
    };

    int is_leap(int year) {
        if (year % 4 == 0 && year % 100 || year % 400 == 0)
            return 1;
        return 0;
    }

    int get_days(int year, int month) {
        int res = months[month];
        if (month == 2) res += is_leap(year);
        return res;
    }

    string dayOfTheWeek(int day, int month, int year) {
        int res = 4;
        for (int i = 1971; i < year; i ++ )
            res += 365 + is_leap(i);

        for (int i = 1; i < month; i ++ )
            res += get_days(year, i);

        res += day;
        res %= 7;

        string week[7] = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
        return week[res];
    }
};

```
#####  LeetCode 1186. 删除一次得到子数组最大和
```cpp
前后缀分解做法
class Solution {
public:
    int maximumSum(vector<int>& a) {
        int n = a.size();
        vector<int> f(n), g(n);
        int res = a[0];
        f[0] = a[0];
        for (int i = 1; i < n; i ++ ) {
            f[i] = max(f[i - 1], 0) + a[i];
            res = max(res, f[i]);
        }

        g[n - 1] = a[n - 1];
        for (int i = n - 2; i >= 0; i -- )
            g[i] = max(g[i + 1], 0) + a[i];

        for (int i = 1; i < n - 1; i ++ )
            res = max(res, f[i - 1] + g[i + 1]);

        return res;
    }
};
状态机类型DP
class Solution {
public:
    int maximumSum(vector<int>& a) {
        int n = a.size(), INF = 2e9;
        vector<int> f(n), g(n);
        f[0] = a[0];
        g[0] = -INF;
        int res = a[0];
        for (int i = 1; i < n; i ++ ) {
            f[i] = max(f[i - 1], 0) + a[i];
            g[i] = g[i - 1] + a[i];
            if (i >= 2) g[i] = max(g[i], f[i - 2] + a[i]);
            res = max({res, f[i], g[i]});
        }

        return res;
    }
};
```
#####  LeetCode 1187. 使数组严格递增
```cpp
class Solution {
public:
    int makeArrayIncreasing(vector<int>& a, vector<int>& b) {
        sort(b.begin(), b.end());
        int n = a.size(), m = b.size(), INF = 2e9;
        b.push_back(INF);
        vector<vector<int>> f(n, vector<int>(m + 1, INF));
        f[0][0] = a[0];
        for (int i = 1; i <= m; i ++ ) f[0][i] = min(a[0], b[0]);
        for (int i = 1; i < n; i ++ ) {
            for (int j = 0; j <= m; j ++ ) {
                if (f[i - 1][j] < a[i]) f[i][j] = a[i];
                if (j) {
                    int l = 0, r = m;
                    while (l < r) {
                        int mid = l + r >> 1;
                        if (b[mid] > f[i - 1][j - 1]) r = mid;
                        else l = mid + 1;
                    }
                    f[i][j] = min(f[i][j], b[r]);
                }
            }
        }

        for (int i = 0; i <= m; i ++ )
            if (f[n - 1][i] < INF)
                return i;
        return -1;
    }
};
```
#####  LeetCode 1189. “气球” 的最大数量
```cpp
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        unordered_map<char, int> cnt;
        for (auto c: text) cnt[c] ++ ;
        return min({cnt['b'], cnt['a'], cnt['l'] / 2, cnt['o'] / 2, cnt['n']});
    }
};
```


### 究极班- Week 55(第 1190 ~ 1192、1200 ~ 1203、1206 ~ 1208 题)

#####  LeetCode 1190. 反转每对括号间的子串
```cpp
class Solution {
public:
    string reverseParentheses(string s) {
        unordered_map<int, int> hash;
        stack<int> stk;
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == '(') stk.push(i);
            else if (s[i] == ')') {
                hash[i] = stk.top();
                hash[stk.top()] = i;
                stk.pop();
            }
        }

        string res;
        int d = 1;
        for (int i = 0; i < s.size(); i += d) {
            if (s[i] == '(' || s[i] == ')') {
                d = -d;
                i = hash[i];
            } else {
                res += s[i];
            }
        }

        return res;
    }
};

```
#####  LeetCode 1191. K 次串联后最大子数组之和
```cpp
typedef long long LL;

const int MOD = 1e9 + 7;

class Solution {
public:
    int kConcatenationMaxSum(vector<int>& arr, int k) {
        LL mx = 0, l = 0, r = 0, sum = 0, s = 0;
        for (int i = 0; i < arr.size(); i ++ ) {
            sum += arr[i];
            l = max(l, sum);
            s = max(s, 0ll) + arr[i];
            mx = max(mx, s);
            if (i + 1 == arr.size()) r = s;
        }

        if (k == 1) return mx % MOD;
        if (sum < 0) return max(mx, l + r) % MOD;
        return max(sum * (LL)(k - 2) + l + r, mx) % MOD;
    }
};

```
#####  LeetCode 1192. 查找集群内的「关键连接」
```cpp
const int N = 100010, M = N * 2;

int h[N], e[M], ne[M], idx;
int dfn[N], low[N], timestamp;

class Solution {
public:
    vector<vector<int>> ans;
    void add(int a, int b) {
        e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
    }

    void tarjan(int u, int from) {
        dfn[u] = low[u] = ++ timestamp;
        for (int i = h[u]; ~i; i = ne[i]) {
            int j = e[i];
            if (!dfn[j]) {
                tarjan(j, i);
                low[u] = min(low[u], low[j]);
                if (dfn[u] < low[j]) ans.push_back({u, j});
            } else if (i != (from ^ 1)) {
                low[u] = min(low[u], low[j]);
            }
        }
    }

    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        memset(h, -1, n * 4);
        memset(dfn, 0, n * 4);
        idx = timestamp = 0;

        for (auto& p: connections) {
            int a = p[0], b = p[1];
            add(a, b), add(b, a);
        }

        tarjan(0, -1);
        return ans;
    }
};

```
#####  LeetCode 1200. 最小绝对差
```cpp
class Solution {
public:
    vector<vector<int>> minimumAbsDifference(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        int mn = 1e8;
        for (int i = 1; i < arr.size(); i ++ )
            mn = min(mn, arr[i] - arr[i - 1]);

        vector<vector<int>> res;
        for (int i = 1; i < arr.size(); i ++ ) {
            if (arr[i] - arr[i - 1] == mn) {
                res.push_back({arr[i - 1], arr[i]});
            }
        }

        return res;
    }
};

```
#####  LeetCode 1201. 丑数 III
```cpp
typedef long long LL;

class Solution {
public:
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }

    LL lcm(LL a, LL b) {
        return a * b / gcd(a, b);
    }

    int nthUglyNumber(int n, LL a, LL b, LL c) {
        LL ab = lcm(a, b), ac = lcm(a, c), bc = lcm(b, c);
        LL abc = lcm(ab, c);

        LL l = 1, r = 2e9;
        while (l < r) {
            LL mid = l + r >> 1;
            LL cnt = mid / a + mid / b + mid / c - mid / ab - mid / ac - mid / bc + mid / abc;
            if (cnt >= n) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
#####  LeetCode 1202. 交换字符串中的元素
```cpp
class Solution {
public:
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        int n = s.size();
        for (int i = 0; i < n; i ++ ) p.push_back(i);

        for (auto& items: pairs) {
            int a = items[0], b = items[1];
            p[find(a)] = find(b);
        }

        vector<vector<char>> chr(n);
        vector<vector<int>> pos(n);
        for (int i = 0; i < n; i ++ ) {
            int k = find(i);
            chr[k].push_back(s[i]);
            pos[k].push_back(i);
        }

        string res = s;
        for (int i = 0; i < n; i ++ ) {
            sort(chr[i].begin(), chr[i].end());
            for (int j = 0; j < chr[i].size(); j ++ ) {
                res[pos[i][j]] = chr[i][j];
            }
        }

        return res;
    }
};
```
#####  LeetCode 1203. 项目管理
```cpp
class Solution {
public:
    vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
        vector<int> gc(m);
        for (int i = 0; i < n; i ++ ) {
            int id = group[i];
            if (id != -1) gc[id] ++ ;
            else {
                gc.push_back(1);
                group[i] = m ++ ;
            }
        }

        vector<vector<int>> g1(n), g2(m);
        vector<int> d1(n), d2(m);
        for (int i = 0; i < n; i ++ ) {
            for (int j: beforeItems[i]) {
                g1[j].push_back(i);
                d1[i] ++ ;
                int a = group[i], b = group[j];
                if (a != b) {
                    g2[b].push_back(a);
                    d2[a] ++ ;
                }
            }
        }

        queue<int> q;
        for (int i = 0; i < m; i ++ ) {
            if (!d2[i]) {
                q.push(i);
            }
        }
        int cnt = 0, cur = 0;
        vector<int> pos(m);
        while (q.size()) {
            auto t = q.front();
            q.pop();
            cnt ++ ;
            pos[t] = cur;
            cur += gc[t];
            for (auto j: g2[t]) {
                if ( -- d2[j] == 0) {
                    q.push(j);
                }
            }
        }

        if (cnt != m) return {};
        for (int i = 0; i < n; i ++ ) {
            if (!d1[i]) {
                q.push(i);
            }
        }

        cnt = 0;
        vector<int> res(n);
        while (q.size()) {
            auto t = q.front();
            q.pop();
            cnt ++ ;
            int k = group[t];
            res[pos[k] ++ ] = t;
            for (auto j: g1[t]) {
                if ( -- d1[j] == 0) {
                    q.push(j);
                }
            }
        }

        if (cnt != n) return {};
        return res;
    }
};
```
#####  LeetCode 1206. 设计跳表
```cpp
class Skiplist {
public:
    static const int level = 8;

    struct Node {
        int val;
        vector<Node*> next;
        Node(int _val): val(_val) {
            next.resize(level, NULL);
        }
    }*head;

    Skiplist() {
        head = new Node(-1);
    }

    ~Skiplist() {
        delete head;
    }

    void find(int target, vector<Node*>& pre) {
        auto p = head;
        for (int i = level - 1; i >= 0; i -- ) {
            while (p->next[i] && p->next[i]->val < target) p = p->next[i];
            pre[i] = p;
        }
    }

    bool search(int target) {
        vector<Node*> pre(level);
        find(target, pre);
        auto p = pre[0]->next[0];
        return p && p->val == target;
    }

    void add(int num) {
        vector<Node*> pre(level);
        find(num, pre);
        auto p = new Node(num);
        for (int i = 0; i < level; i ++ ) {
            p->next[i] = pre[i]->next[i];
            pre[i]->next[i] = p;
            if (rand() % 2) break;
        }
    }

    bool erase(int num) {
        vector<Node*> pre(level);
        find(num, pre);

        auto p = pre[0]->next[0];
        if (!p || p->val != num) return false;

        for (int i = 0; i < level && pre[i]->next[i] == p; i ++ )
            pre[i]->next[i] = p->next[i];

        delete p;

        return true;
    }
};

/**
 * Your Skiplist object will be instantiated and called as such:
 * Skiplist* obj = new Skiplist();
 * bool param_1 = obj->search(target);
 * obj->add(num);
 * bool param_3 = obj->erase(num);
 */

```
#####  LeetCode 1207. 独一无二的出现次数
```cpp
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        unordered_map<int, int> cnt;
        for (auto x: arr) cnt[x] ++;

        unordered_set<int> hash;
        for (auto& [k, v]: cnt) {
            if (hash.count(v)) return false;
            hash.insert(v);
        }
        return true;
    }
};
```
#####  LeetCode 1208. 尽可能使字符串相等
```cpp
class Solution {
public:
    int equalSubstring(string s, string t, int maxCost) {
        int res = 0;
        for (int i = 0, j = 0, cost = 0; i < s.size(); i ++ ) {
            cost += abs(s[i] - t[i]);
            while (cost > maxCost) {
                cost -= abs(s[j] - t[j]);
                j ++ ;
            }
            res = max(res, i - j + 1);
        }
        return res;
    }
};
```





### 究极班- Week 56(第 1209、1210、1217 ~ 1224、1227、1232 ~ 1235、1237 ~ 1240、1247 题)


#####  LeetCode 1209. 删除字符串中的所有相邻重复项 II
```cpp
#define x first
#define y second

class Solution {
public:
    string removeDuplicates(string s, int k) {
        stack<pair<char, int>> stk;
        for (auto c: s) {
            if (stk.empty() || stk.top().x != c) stk.push({c, 1});
            else stk.top().y ++ ;

            if (stk.top().y >= k) {
                stk.top().y -= k;
                if (stk.top().y == 0) stk.pop();
            }
        }

        string res;
        while (stk.size()) {
            auto t = stk.top();
            stk.pop();
            res += string(t.y, t.x);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode 1210. 穿过迷宫的最少移动次数
```cpp
class Solution {
public:
    struct Node {
        int x, y, d;
    };

    vector<vector<vector<int>>> dist;
    queue<Node> q;

    void update(int x, int y, int d, int distance) {
        if (dist[x][y][d] > distance) {
            dist[x][y][d] = distance;
            q.push({x, y, d});
        }
    }

    int minimumMoves(vector<vector<int>>& g) {
        int n = g.size(), INF = 1e8;
        dist = vector<vector<vector<int>>>(n, vector<vector<int>>(n, vector<int>(2, INF)));
        q.push({0, 0, 0});
        dist[0][0][0] = 0;

        int dx[2] = {0, 1}, dy[2] = {1, 0};

        while (q.size()) {
            auto t = q.front();
            q.pop();
            int distance = dist[t.x][t.y][t.d];
            if (t.x == n - 1 && t.y == n - 2 && t.d == 0)
                return distance;
            Node a{t.x, t.y}, b{t.x + dx[t.d], t.y + dy[t.d]};

            // 向右平移
            if (a.y + 1 < n && !g[a.x][a.y + 1] && b.y + 1 < n && !g[b.x][b.y + 1])
                update(t.x, t.y + 1, t.d, distance + 1);

            // 向下平移
            if (a.x + 1 < n && !g[a.x + 1][a.y] && b.x + 1 < n && !g[b.x + 1][b.y])
                update(t.x + 1, t.y, t.d, distance + 1);

            // 顺时针旋转90度
            if (t.d == 0 && t.x + 1 < n && !g[t.x + 1][t.y] && !g[t.x + 1][t.y + 1])
                update(t.x, t.y, 1, distance + 1);

            // 逆时针旋转90度
            if (t.d == 1 && t.y + 1 < n && !g[t.x][t.y + 1] && !g[t.x + 1][t.y + 1])
                update(t.x, t.y, 0, distance + 1);
        }

        return -1;
    }
};
```
#####  LeetCode 1217. 玩筹码
```cpp
class Solution {
public:
    int minCostToMoveChips(vector<int>& position) {
        int odd = 0, even = 0;
        for (auto x: position)
            if (x % 2) odd ++ ;
            else even ++ ;
        return min(odd, even);
    }
};
```
#####  LeetCode 1218. 最长定差子序列
```cpp
class Solution {
public:
    int longestSubsequence(vector<int>& arr, int d) {
        unordered_map<int, int> f;
        int res = 0;
        for (auto x: arr) {
            if (f.count(x - d)) f[x] = f[x - d] + 1;
            else f[x] = 1;
            res = max(res, f[x]);
        }

        return res;
    }
};

```
#####  LeetCode 1219. 黄金矿工
```cpp
class Solution {
public:
    vector<vector<int>> g;
    int n, m;
    int ans = 0;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    void dfs(int x, int y, int sum) {
        int w = g[x][y];
        sum += w;
        ans = max(ans, sum);
        g[x][y] = 0;

        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && g[a][b])
                dfs(a, b, sum);
        }

        g[x][y] = w;
    }

    int getMaximumGold(vector<vector<int>>& grid) {
        g = grid;
        n = g.size(), m = g[0].size();
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j])
                    dfs(i, j, 0);
        return ans;
    }
};

```
#####  LeetCode 1220. 统计元音字母序列的数目
```cpp
class Solution {
public:
    int countVowelPermutation(int n) {
        const int MOD = 1e9 + 7;

        vector<vector<int>> f(n + 1, vector<int>(5, 0));

        int g[5][5] = {
            {0, 1, 0, 0, 0},
            {1, 0, 1, 0, 0},
            {1, 1, 0, 1, 1},
            {0, 0, 1, 0, 1},
            {1, 0, 0, 0, 0},
        };
        for (int i = 0; i < 5; i ++ ) f[1][i] = 1;
        for (int i = 2; i <= n; i ++ )
            for (int j = 0; j < 5; j ++ )
                for (int k = 0; k < 5; k ++ )
                    if (g[k][j])
                        f[i][j] = (f[i][j] + f[i - 1][k]) % MOD;

        int res = 0;
        for (int i = 0; i < 5; i ++ )
            res = (res + f[n][i]) % MOD;
        return res;
    }
};

```
#####  LeetCode 1221. 分割平衡字符串
```cpp
class Solution {
public:
    int balancedStringSplit(string s) {
        int res = 0, cnt = 0;
        for (auto c: s) {
            if (c == 'L') cnt -- ;
            else cnt ++ ;
            if (!cnt) res ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 1222. 可以攻击国王的皇后
```cpp
class Solution {
public:
    vector<vector<int>> queensAttacktheKing(vector<vector<int>>& queens, vector<int>& king) {
        bool st[8][8] = {0};
        for (auto& q: queens) st[q[0]][q[1]] = true;

        vector<vector<int>> res;

        for (int i = -1; i <= 1; i ++ )
            for (int j = -1; j <= 1; j ++ )
                if (i || j) {
                    int x = king[0], y = king[1];
                    while (true) {
                        x += i, y += j;
                        if (x >= 8 || x < 0 || y >= 8 || y < 0) break;
                        if (st[x][y]) {
                            res.push_back({x, y});
                            break;
                        }
                    }
                }

        return res;
    }
};

```
#####  LeetCode 1223. 掷骰子模拟
```cpp
class Solution {
public:
    int dieSimulator(int n, vector<int>& rollMax) {
        const int MOD = 1e9 + 7;
        vector<vector<vector<int>>> f(n + 1, vector<vector<int>>(6, vector<int>(16)));
        for (int i = 0; i < 6; i ++ ) f[1][i][1] = 1;
        for (int i = 1; i < n; i ++ )
            for (int j = 0; j < 6; j ++ )
                for (int k = 1; k <= rollMax[j]; k ++ )
                    for (int u = 0; u < 6; u ++ ) {
                        int len = 1;
                        if (u == j) {
                            len = k + 1;
                            if (len > rollMax[j]) continue;
                        }
                        f[i + 1][u][len] = (f[i + 1][u][len] + f[i][j][k]) % MOD;
                    }

        int res = 0;
        for (int i = 0; i < 6; i ++ )
            for (int j = 1; j <= rollMax[i]; j ++ )
                res = (res + f[n][i][j]) % MOD;
        return res;
    }
};
```
#####  LeetCode 1224. 最大相等频率
```cpp
class Solution {
public:
    int maxEqualFreq(vector<int>& nums) {
        unordered_map<int, int> cnt, hash;

        int res = 0, len = 0;
        for (auto x: nums) {
            if (cnt.count(x)) {
                hash[cnt[x]] -- ;
                if (!hash[cnt[x]]) hash.erase(cnt[x]);
            }
            cnt[x] ++ ;
            hash[cnt[x]] ++ ;

            len ++ ;
            if (hash.size() == 1) {
                for (auto& [k, v]: hash) {
                    if (v == 1 || k == 1) res = len;
                }
            } if (hash.size() == 2) {
                vector<vector<int>> tmp;
                for (auto& [k, v]: hash) {
                    tmp.push_back({k, v});
                }
                if (tmp[0][0] > tmp[1][0]) swap(tmp[0], tmp[1]);
                if (tmp[0][0] == 1 && tmp[0][1] == 1) res = len;
                else if (tmp[0][0] + 1 == tmp[1][0] && tmp[1][1] == 1) res = len;
            }
        }

        return res;
    }
};
```
#####  LeetCode 1227. 飞机座位分配概率
```cpp
class Solution {
public:
    double nthPersonGetsNthSeat(int n) {
        return n > 1 ? 0.5 : 1;
    }
};
```
#####  LeetCode 1232. 缀点成线
```cpp
class Solution {
public:
    bool checkStraightLine(vector<vector<int>>& c) {
        for (int i = 2; i < c.size(); i ++ ) {
            int x1 = c[1][0] - c[0][0], y1 = c[1][1] - c[0][1];
            int x2 = c[i][0] - c[0][0], y2 = c[i][1] - c[0][1];
            if (x1 * y2 - x2 * y1) return false;
        }
        return true;
    }
};
```
#####  LeetCode 1233. 删除子文件夹
```cpp
class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        sort(folder.begin(), folder.end());

        for (auto& f: folder) f += '/';

        vector<string> res;
        for (auto& f: folder) {
            if (res.empty() || res.back().size() > f.size() || f.substr(0, res.back().size()) != res.back())
                res.push_back(f);
        }

        for (auto& f: res) f.pop_back();
        return res;
    }
};
```
#####  LeetCode 1234. 替换子串得到平衡字符串
```cpp
class Solution {
public:
    int get(char c) {
        if (c == 'Q') return 0;
        if (c == 'W') return 1;
        if (c == 'E') return 2;
        return 3;
    }

    bool check(vector<int>& tot, vector<int>& sum, int target) {
        for (int i = 0; i < 4; i ++ )
            if (tot[i] - sum[i] > target)
                return false;
        return true;
    }

    int balancedString(string s) {
        vector<int> tot(4);
        for (auto c: s) tot[get(c)] ++ ;
        int n = s.size();

        if (tot[0] == n / 4 && tot[1] == n / 4 && tot[2] == n / 4)
            return 0;
        int res = n;
        vector<int> sum(4);
        for (int i = 0, j = 0; i < n; i ++ ) {
            sum[get(s[i])] ++ ;
            while (j <= i && check(tot, sum, n / 4)) {
                res = min(res, i - j + 1);
                sum[get(s[j])] -- ;
                j ++ ;
            }
        }

        return res;
    }
};
```
#####  LeetCode 1235. 规划兼职工作
```cpp
class Solution {
public:
    struct Job {
        int l, r, w;
        bool operator< (const Job& t) const {
            return r < t.r;
        }
    };

    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n = startTime.size();
        vector<Job> jobs;
        for (int i = 0; i < n; i ++ )
            jobs.push_back({startTime[i], endTime[i], profit[i]});

        sort(jobs.begin(), jobs.end());
        vector<int> f(n);
        f[0] = jobs[0].w;
        for (int i = 1; i < n; i ++ ) {
            f[i] = max(f[i - 1], jobs[i].w);
            if (jobs[0].r <= jobs[i].l) {
                int l = 0, r = i - 1;
                while (l < r) {
                    int mid = l + r + 1 >> 1;
                    if (jobs[mid].r <= jobs[i].l) l = mid;
                    else r = mid - 1;
                }
                f[i] = max(f[i], f[r] + jobs[i].w);
            }
        }

        return f[n - 1];
    }
};
```
#####  LeetCode 1237. 找出给定方程的正整数解
```cpp
/*
 * // This is the custom function interface.
 * // You should not implement it, or speculate about its implementation
 * class CustomFunction {
 * public:
 *     // Returns f(x, y) for any given positive integers x and y.
 *     // Note that f(x, y) is increasing with respect to both x and y.
 *     // i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
 *     int f(int x, int y);
 * };
 */

class Solution {
public:
    vector<vector<int>> findSolution(CustomFunction& c, int z) {
        vector<vector<int>> res;
        int x = 1, y = 1000;
        while (x <= 1000 && y >= 1) {
            int t = c.f(x, y);
            if (t > z) y -- ;
            else if (t < z) x ++ ;
            else {
                res.push_back({x, y});
                y --, x ++ ;
            }
        }

        return res;
    }
};
```
#####  LeetCode 1238. 循环码排列
```cpp
class Solution {
public:
    vector<int> circularPermutation(int n, int start) {
        vector<int> a{0, 1};
        for (int i = 1; i < n; i ++ ) {
            vector<int> b = a;
            for (int j = a.size() - 1; j >= 0; j -- )
                b.push_back(a[j] + (1 << i));
            a = b;
        }

        for (int& x: a) x ^= start;
        return a;
    }
};
```
#####  LeetCode 1239. 串联字符串的最大长度
```cpp
class Solution {
public:
    int maxLength(vector<string>& strs) {
        vector<int> state;
        int n = strs.size();
        for (auto& str: strs) {
            int s = 0;
            for (auto c: str) {
                int t = c - 'a';
                if (s >> t & 1) {
                    s = -1;
                    break;
                }
                s |= 1 << t;
            }
            state.push_back(s);
        }

        int res = 0;
        for (int i = 0; i < 1 << n; i ++ ) {
            int s = 0, len = 0;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1) {
                    if (state[j] == -1 || (s & state[j])) {
                        len = -1;
                        break;
                    }
                    s |= state[j];
                    len += strs[j].size();
                }
            res = max(res, len);
        }

        return res;
    }
};
```
#####  LeetCode 1240. 铺瓷砖
```cpp
class Solution {
public:
    vector<vector<bool>> st;
    int ans, n, m;

    bool check(int x, int y, int len) {
        for (int i = x; i < x + len; i ++ )
            for (int j = y; j < y + len; j ++ )
                if (st[i][j])
                    return false;
        return true;
    }

    void fill(int x, int y, int len, bool t) {
        for (int i = x; i < x + len; i ++ )
            for (int j = y; j < y + len; j ++ )
                st[i][j] = t;
    }

    void dfs(int x, int y, int cnt) {
        if (cnt >= ans) return;  // 最优性剪枝
        // 组合数优化
        if (y == m) x ++, y = 0;
        if (x == n) ans = cnt;
        else {
            if (st[x][y]) dfs(x, y + 1, cnt);
            else {
                // 搜索顺序优化
                for (int len = min(n - x, m - y); len; len -- ) {
                    if (check(x, y, len)) {
                        fill(x, y, len, true);
                        dfs(x, y + 1, cnt + 1);
                        fill(x, y, len, false);
                    }
                }
            }
        }
    }

    int tilingRectangle(int n, int m) {
        ans = n * m;
        this->n = n, this->m = m;
        st = vector<vector<bool>>(n, vector<bool>(m));

        dfs(0, 0, 0);

        return ans;
    }
};
```
#####  LeetCode 1247. 交换字符使得字符串相同
```cpp
class Solution {
public:
    int minimumSwap(string s1, string s2) {
        int a = 0, b = 0;
        for (int i = 0; i < s1.size(); i ++ )
            if (s1[i] != s2[i])
                if (s1[i] == 'x') a ++ ;
                else b ++ ;

        if ((a + b) % 2) return -1;
        if (a % 2) return (a + b) / 2 + 1;
        return (a + b) / 2;
    }
};
```




### 究极班- Week 57（第 1248 ~ 1250、1252 ~ 1255、1260 ~ 1262 题）

#####  LeetCode 1248. 统计「优美子数组」
```cpp
class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        unordered_map<int, int> cnt;
        cnt[0] ++ ;
        int res = 0, sum = 0;
        for (auto x: nums) {
            if (x % 2) sum ++ ;
            res += cnt[sum - k];
            cnt[sum] ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 1249. 移除无效的括号
```cpp
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        string res;
        int cnt = 0;
        for (auto c: s) {
            if (c == '(') cnt ++, res += c;
            else if (c == ')') {
                if (cnt) cnt --, res += c;
            } else res += c;
        }

        cnt = 0;
        s = res, res = "";
        reverse(s.begin(), s.end());
        for (auto c: s) {
            if (c == ')') cnt ++, res += c;
            else if (c == '(') {
                if (cnt) cnt --, res += c;
            } else res += c;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};

```
#####  LeetCode 1250. 检查「好数组」
```cpp
class Solution {
public:
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }

    bool isGoodArray(vector<int>& nums) {
        int res = 0;
        for (auto x: nums)
            res = gcd(res, x);
        return res == 1;
    }
};
```



#####  LeetCode 1252. 奇数值单元格的数目
```cpp
class Solution {
public:
    int oddCells(int m, int n, vector<vector<int>>& indices) {
        vector<int> row(m), col(n);
        for (auto& p: indices) {
            row[p[0]] ++ ;
            col[p[1]] ++ ;
        }

        int oddm = 0, oddn = 0;
        for (int x: row) oddm += x % 2;
        for (int x: col) oddn += x % 2;
        return oddm * (n - oddn) + (m - oddm) * oddn;
    }
};
```
#####  LeetCode 1253. 重构 2 行二进制矩阵
```cpp
class Solution {
public:
    vector<vector<int>> reconstructMatrix(int upper, int lower, vector<int>& colsum) {
        int n = colsum.size();
        vector<vector<int>> res(2, vector<int>(n, -1));
        int cnt = 0;
        for (int i = 0; i < n; i ++ ) {
            int c = colsum[i];
            if (c == 0) res[0][i] = res[1][i] = 0;
            else if (c == 2) {
                res[0][i] = res[1][i] = 1;
                upper -- ;
                lower -- ;
            } else cnt ++ ;
        }

        if (upper < 0 || lower < 0 || cnt != upper + lower) return {};
        for (int i = 0; i < n; i ++ )
            if (res[0][i] == -1) {
                if (upper) {
                    res[0][i] = 1;
                    res[1][i] = 0;
                    upper -- ;
                } else {
                    res[0][i] = 0;
                    res[1][i] = 1;
                }
            }

        return res;
    }
};
```
#####  LeetCode 1254. 统计封闭岛屿的数目
```cpp
class Solution {
public:
    int n, m;
    vector<vector<int>> g;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int dfs(int x, int y) {
        int res = 1;
        g[x][y] = 1;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m) {
                res = 0;
                continue;
            }
            if (!g[a][b] && !dfs(a, b))
                res = 0;
        }
        return res;
    }

    int closedIsland(vector<vector<int>>& grid) {
        int res = 0;
        n = grid.size(), m = grid[0].size();
        g = grid;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!g[i][j])
                    res += dfs(i, j);
        return res;
    }
};
```
#####  LeetCode 1255. 得分最高的单词集合
```cpp
class Solution {
public:
    int maxScoreWords(vector<string>& words, vector<char>& letters, vector<int>& score) {
        int tot[26] = {0};
        for (auto c: letters) tot[c - 'a'] ++ ;
        int n = words.size();
        int res = 0;
        for (int i = 0; i < 1 << n; i ++ ) {
            int cnt[26] = {0};
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                    for (auto c: words[j])
                        cnt[c - 'a'] ++ ;

            bool flag = true;
            for (int j = 0; j < 26; j ++ )
                if (cnt[j] > tot[j]) {
                    flag = false;
                    break;
                }

            if (flag) {
                int sum = 0;
                for (int j = 0; j < 26; j ++ )
                    sum += cnt[j] * score[j];
                res = max(res, sum);
            }
        }

        return res;
    }
};
```
#####  LeetCode 1260. 二维网格迁移
```cpp
class Solution {
public:
    int n, m;

    void reverse(vector<vector<int>>& grid, int start, int end) {
        for (int i = start, j = end - 1; i < j; i ++, j -- )
            swap(grid[i / m][i % m], grid[j / m][j % m]);
    }

    vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) {
        n = grid.size(), m = grid[0].size();
        k %= n * m;
        reverse(grid, 0, n * m);
        reverse(grid, 0, k);
        reverse(grid, k, n * m);
        return grid;
    }
};
```
#####  LeetCode 1261. 在受污染的二叉树中查找元素
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class FindElements {
public:
    unordered_set<int> hash;

    void dfs(TreeNode* root, int x) {
        hash.insert(x);
        root->val = x;
        if (root->left) dfs(root->left, x * 2 + 1);
        if (root->right) dfs(root->right, x * 2 + 2);
    }

    FindElements(TreeNode* root) {
        dfs(root, 0);
    }

    bool find(int target) {
        return hash.count(target);
    }
};

/**
 * Your FindElements object will be instantiated and called as such:
 * FindElements* obj = new FindElements(root);
 * bool param_1 = obj->find(target);
 */

```
#####  LeetCode 1262. 可被三整除的最大和
```cpp
class Solution {
public:
    int maxSumDivThree(vector<int>& nums) {
        const int INF = 1e8;
        int sum = 0;
        int f1 = INF, s1 = INF;
        int f2 = INF, s2 = INF;
        for (auto x: nums) {
            sum += x;
            if (x % 3 == 1) {
                if (x <= f1) s1 = f1, f1 = x;
                else if (x < s1) s1 = x;
            } else if (x % 3 == 2) {
                if (x <= f2) s2 = f2, f2 = x;
                else if (x < s2) s2 = x;
            }
        }

        if (sum % 3 == 0) return sum;
        if (sum % 3 == 1)
            return max(sum - f1, sum - f2 - s2);
        return max(sum - f2, sum - f1 - s1);
    }
};
```


### 究极班-Week 58(第 1263、1266 ~ 1269、1275 ~ 1278、1281 题)


#####  LeetCode 1263. 推箱子
```cpp
typedef vector<int> VI;

const int N = 20;

int dist[N][N][N][N];
bool st[N][N][N][N];

class Solution {
public:
    int minPushBox(vector<vector<char>>& g) {
        int n = g.size(), m = g[0].size();
        VI start(4);
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == 'B') start[0] = i, start[1] = j;
                else if (g[i][j] == 'S') start[2] = i, start[3] = j;

        deque<VI> q;
        q.push_back(start);
        memset(dist, 0x3f, sizeof dist);
        memset(st, 0, sizeof st);

        dist[start[0]][start[1]][start[2]][start[3]] = 0;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

        while (q.size()) {
            auto t = q.front();
            q.pop_front();

            if (st[t[0]][t[1]][t[2]][t[3]]) continue;
            st[t[0]][t[1]][t[2]][t[3]] = true;

            int distance = dist[t[0]][t[1]][t[2]][t[3]];
            if (g[t[0]][t[1]] == 'T') return distance;

            for (int i = 0; i < 4; i ++ ) {
                int x = t[2] + dx[i], y = t[3] + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m && g[x][y] != '#'
                    && (x != t[0] || y != t[1]) && dist[t[0]][t[1]][x][y] > distance) {
                        dist[t[0]][t[1]][x][y] = distance;
                        q.push_front({t[0], t[1], x, y});
                    }

                if (t[0] - t[2] == dx[i] && t[1] - t[3] == dy[i]) {
                    x = t[0] + dx[i], y = t[1] + dy[i];
                    if (x >= 0 && x < n && y >= 0 && y < m && g[x][y] != '#'
                        && dist[x][y][t[0]][t[1]] > distance + 1) {
                            dist[x][y][t[0]][t[1]] = distance + 1;
                            q.push_back({x, y, t[0], t[1]});
                        }
                }
            }
        }

        return -1;
    }
};
```
#####  LeetCode 1266. 访问所有点的最小时间
```cpp
class Solution {
public:
    int minTimeToVisitAllPoints(vector<vector<int>>& p) {
        int res = 0;
        for (int i = 1; i < p.size(); i ++ ) {
            int dx = abs(p[i][0] - p[i - 1][0]);
            int dy = abs(p[i][1] - p[i - 1][1]);
            res += max(dx, dy);
        }
        return res;
    }
};

```
#####  LeetCode 1267. 统计参与通信的服务器
```cpp
class Solution {
public:
    int countServers(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size();
        vector<int> row(n), col(m);
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j]) {
                    row[i] ++ ;
                    col[j] ++ ;
                }

        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] && (row[i] >= 2 || col[j] >= 2))
                    res ++ ;
        return res;
    }
};
```
#####  LeetCode 1268. 搜索推荐系统
```cpp
class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& ps, string str) {
        sort(ps.begin(), ps.end());
        vector<vector<string>> ans;
        string cur;
        int k = 0, n = ps.size();
        for (auto c: str) {
            cur += c;
            while (k < n && ps[k] < cur) k ++ ;
            vector<string> res;
            for (int i = k; i < n && i < k + 3; i ++ )
                if (ps[i].substr(0, cur.size()) == cur) {
                    res.push_back(ps[i]);
                } else {
                    break;
                }
            ans.push_back(res);
        }
        return ans;
    }
};
```
#####  LeetCode 1269. 停在原地的方案数
```cpp
class Solution {
public:
    int numWays(int n, int m) {
        const int MOD = 1e9 + 7;
        m = min(m, n);
        vector<vector<int>> f(n + 1, vector<int>(m));
        f[0][0] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                f[i][j] = f[i - 1][j];
                if (j) f[i][j] = (f[i][j] + f[i - 1][j - 1]) % MOD;
                if (j + 1 < m) f[i][j] = (f[i][j] + f[i - 1][j + 1]) % MOD;
            }
        return f[n][0];
    }
};
```
#####  LeetCode 1275. 找出井字棋的获胜者
```cpp
class Solution {
public:
    int g[3][3];

    bool check(int x) {
        for (int i = 0; i < 3; i ++ ) {
            if (g[i][0] == x && g[i][1] == x && g[i][2] == x)
                return true;
            if (g[0][i] == x && g[1][i] == x && g[2][i] == x)
                return true;
        }

        if (g[0][0] == x && g[1][1] == x && g[2][2] == x)
            return true;
        if (g[0][2] == x && g[1][1] == x && g[2][0] == x)
            return true;
        return false;
    }

    string tictactoe(vector<vector<int>>& moves) {
        memset(g, 0, sizeof g);
        int t = 1;
        for (auto& move: moves) {
            g[move[0]][move[1]] = t;
            t = 3 - t;
        }

        if (check(1)) return "A";
        if (check(2)) return "B";
        if (moves.size() == 9) return "Draw";
        return "Pending";
    }
};
```
#####  LeetCode 1276. 不浪费原料的汉堡制作方案
```cpp
class Solution {
public:
    vector<int> numOfBurgers(int a, int b) {
        if (a < b * 2 || (a - 2 * b) % 2) return {};
        int x = (a - b * 2) / 2;
        if (b < x) return {};
        int y = b - x;
        return {x, y};
    }
};
```
#####  LeetCode 1277. 统计全为 1 的正方形子矩阵
```cpp
class Solution {
public:
    int countSquares(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size();
        vector<vector<int>> f(n, vector<int>(m));

        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j]) {
                    if (!i || !j) {
                        f[i][j] = 1;
                        res ++ ;
                    } else {
                        f[i][j] = min({f[i - 1][j], f[i][j - 1], f[i - 1][j - 1]}) + 1;
                        res += f[i][j];
                    }
                }

        return res;
    }
};
```
#####  LeetCode 1278. 分割回文串 III
```cpp
class Solution {
public:
    int palindromePartition(string s, int m) {
        int n = s.size(), INF = 1e8;
        vector<vector<int>> f(n, vector<int>(m + 1, INF));
        vector<vector<int>> cost(n, vector<int>(n));
        for (int i = 0; i < n; i ++ )
            for (int j = i; j < n; j ++ )
                for (int a = i, b = j; a < b; a ++, b -- )
                    if (s[a] != s[b])
                        cost[i][j] ++ ;

        for (int i = 0; i < n; i ++ ) {
            f[i][1] = cost[0][i];
            for (int j = 2; j <= m; j ++ )
                for (int k = i; k > 0; k -- )
                    f[i][j] = min(f[i][j], f[k - 1][j - 1] + cost[k][i]);
        }

        return f[n - 1][m];
    }
};

```
#####  LeetCode 1281. 整数的各位积和之差
```cpp
class Solution {
public:
    int subtractProductAndSum(int n) {
        int p = 1, s = 0;
        while (n) {
            p *= n % 10;
            s += n % 10;
            n /= 10;
        }
        return p - s;
    }
};
```



### 究极班- Week 59(第 1282 ~ 1284、1286 ~ 1292 题)


#####  LeetCode 1282. 用户分组
```cpp
class Solution {
public:
    vector<vector<int>> groupThePeople(vector<int>& groupSizes) {
        vector<vector<int>> res;
        unordered_map<int, vector<int>> hash;
        for (int i = 0; i < groupSizes.size(); i ++ ) {
            int x = groupSizes[i];
            hash[x].push_back(i);
            if (hash[x].size() == x) {
                res.push_back(hash[x]);
                hash[x].clear();
            }
        }
        return res;
    }
};
```
#####  LeetCode 1283. 使结果不超过阈值的最小除数
```cpp
class Solution {
public:
    bool check(vector<int>& nums, int threshold, int mid) {
        int sum = 0;
        for (auto x: nums)
            sum += (x + mid - 1) / mid;
        return sum <= threshold;
    }

    int smallestDivisor(vector<int>& nums, int threshold) {
        int l = 1, r = 1e6 + 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (check(nums, threshold, mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
#####  LeetCode 1284. 转化为全零矩阵的最少反转次数
```cpp
class Solution {
public:
    int minFlips(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size();
        int dx[] = {-1, 0, 1, 0, 0}, dy[] = {0, 1, 0, -1, 0};
        int res = -1;
        for (int i = 0; i < 1 << n * m; i ++ ) {
            auto w = g;
            int cnt = 0;
            for (int j = 0; j < n * m; j ++ )
                if (i >> j & 1) {
                    cnt ++ ;
                    for (int k = 0; k < 5; k ++ ) {
                        int x = j / m + dx[k], y = j % m + dy[k];
                        if (x >= 0 && x < n && y >= 0 && y < m)
                            w[x][y] ^= 1;
                    }
                }

            bool allzero = true;
            for (int j = 0; j < n; j ++ )
                for (int k = 0; k < m; k ++ )
                    if (w[j][k]) {
                        allzero = false;
                        break;
                    }

            if (allzero && (res == -1 || res > cnt)) res = cnt;
        }

        return res;
    }
};
```
#####  LeetCode 1286. 字母组合迭代器
```cpp
class CombinationIterator {
public:
    string str;
    int n, m, cur;

    CombinationIterator(string characters, int combinationLength) {
        str = characters;
        n = str.size(), m = combinationLength, cur = 0;
        for (int i = 0; i < m; i ++ )
            cur |= 1 << i;
    }

    string next() {
        string res;
        for (int i = 0; i < n; i ++ )
            if (cur >> i & 1)
                res += str[i];

        int k = n - 1;
        while (k >= 0 && (cur >> k & 1)) k -- ;
        if (n - k - 1 == m) cur = -1;
        else {
            int cnt = n - k;
            while (k >= 0 && !(cur >> k & 1)) k -- ;
            for (int i = k; i < n; i ++ )
                if (cur >> i & 1)
                    cur -= 1 << i;
            for (int i = k + 1, j = 0; j < cnt; i ++, j ++ )
                cur |= 1 << i;
        }

        return res;
    }

    bool hasNext() {
        return cur != -1;
    }
};

/**
 * Your CombinationIterator object will be instantiated and called as such:
 * CombinationIterator* obj = new CombinationIterator(characters, combinationLength);
 * string param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```
#####  LeetCode 1287. 有序数组中出现次数超过25%的元素
```cpp
class Solution {
public:
    int findSpecialInteger(vector<int>& a) {
        int n = a.size(), m = n / 4 + 1;
        for (int i = 0; i < n; i += m) {
            int x = a[i];
            int l = lower_bound(a.begin(), a.end(), x) - a.begin();
            int r = upper_bound(a.begin(), a.end(), x) - a.begin();
            if (r - l >= m) return x;
        }
        return -1;
    }
};

```
#####  LeetCode 1288. 删除被覆盖区间
```cpp
class Solution {
public:
    int removeCoveredIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b) {
            if (a[0] != b[0]) return a[0] < b[0];
            return a[1] > b[1];
        });
        int res = intervals.size(), r = 0;
        for (auto& q: intervals) {
            if (q[1] <= r) res -- ;
            r = max(r, q[1]);
        }
        return res;
    }
};
```
#####  LeetCode 1289. 下降路径最小和 II
```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size(), INF = 1e8;
        vector<vector<int>> f(n, vector<int>(m));
        for (int i = 0; i < m; i ++ ) f[0][i] = g[0][i];
        for (int i = 1; i < n; i ++ ) {
            int d1 = INF, d2 = INF;
            for (int j = 0; j < m; j ++ ) {
                int x = f[i - 1][j];
                if (x <= d1) d2 = d1, d1 = x;
                else if (x < d2) d2 = x;
            }
            for (int j = 0; j < m; j ++ )
                if (f[i - 1][j] == d1) f[i][j] = d2 + g[i][j];
                else f[i][j] = d1 + g[i][j];
        }

        int res = INF;
        for (int i = 0; i < m; i ++ )
            res = min(res, f[n - 1][i]);
        return res;
    }
};
```
#####  LeetCode 1290. 二进制链表转整数
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    int getDecimalValue(ListNode* head) {
        int res = 0;
        for (auto p = head; p; p = p->next)
            res = res * 2 + p->val;
        return res;
    }
};
```
#####  LeetCode 1291. 顺次数
```cpp
class Solution {
public:
    vector<int> sequentialDigits(int low, int high) {
        vector<int> res;
        for (int i = 1; i <= 9; i ++ ) {
            int x = 0;
            for (int j = i; j <= 9; j ++ ) {
                x = x * 10 + j;
                if (x >= low && x <= high)
                    res.push_back(x);
            }
        }

        sort(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode 1292. 元素和小于等于阈值的正方形的最大边长
```cpp
class Solution {
public:
    vector<vector<int>> s;

    int get(int x1, int y1, int x2, int y2) {
        return s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1];
    }

    int maxSideLength(vector<vector<int>>& g, int threshold) {
        int n = g.size(), m = g[0].size();
        s = vector<vector<int>>(n + 1, vector<int>(m + 1));
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + g[i - 1][j - 1];

        int res = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                for (int k = res + 1; k <= min(i, j); k ++ ) {
                    if (get(i - k + 1, j - k + 1, i, j) > threshold) break;
                    res ++ ;
                }

        return res;
    }
};
```



### 究极班- Week 60(第 1293、1295 ~ 1302、1304 题)

#####  LeetCode 1293. 网格中的最短路径
```cpp
class Solution {
public:
    struct Node {
        int x, y, z;
    };

    int shortestPath(vector<vector<int>>& g, int k) {
        int n = g.size(), m = g[0].size(), INF = 1e8;
        k = min(k, max(0, n + m - 3));
        vector<vector<vector<int>>> dist(n, vector<vector<int>>(m, vector<int>(k + 1, INF)));
        dist[0][0][0] = 0;
        queue<Node> q;
        q.push({0, 0, 0});

        int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};

        while (q.size()) {
            auto t = q.front();
            q.pop();

            int distance = dist[t.x][t.y][t.z];
            if (t.x == n - 1 && t.y == m - 1) return distance;
            for (int i = 0; i < 4; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m) {
                    int z = t.z + g[x][y];
                    if (z <= k && dist[x][y][z] > distance + 1) {
                        dist[x][y][z] = distance + 1;
                        q.push({x, y, z});
                    }
                }
            }
        }

        return -1;
    }
};
```
#####  LeetCode 1295. 统计位数为偶数的数字
```cpp
class Solution {
public:
    int findNumbers(vector<int>& nums) {
        int res = 0;
        for (auto x: nums)
            if (to_string(x).size() % 2 == 0)
                res ++ ;
        return res;
    }
};
```
#####  LeetCode 1296. 划分数组为连续数字的集合
```cpp
class Solution {
public:
    bool isPossibleDivide(vector<int>& nums, int k) {
        if (nums.size() % k) return false;
        multiset<int> S;
        for (auto x: nums) S.insert(x);

        while (S.size()) {
            int x = *S.begin();
            for (int i = x; i < x + k; i ++ ) {
                auto it = S.find(i);
                if (it == S.end()) return false;
                S.erase(it);
            }
        }

        return true;
    }
};
```
#####  LeetCode 1297. 子串的最大出现次数
```cpp
typedef unsigned long long ULL;

const int P = 13331;

class Solution {
public:
    int maxFreq(string s, int maxLetters, int m, int maxSize) {
        int n = s.size();
        ULL p = 1, key = 0;
        for (int i = 0; i < m; i ++ ) p = p * P;

        unordered_map<ULL, int> hash;
        unordered_map<char, int> cnt;
        int res = 0;
        for (int i = 0, tot = 0; i < n; i ++ ) {
            key = key * P + s[i];
            if (!cnt[s[i]]) tot ++ ;
            cnt[s[i]] ++ ;
            if (i >= m) {
                cnt[s[i - m]] -- ;
                if (!cnt[s[i - m]]) tot -- ;
                key -= s[i - m] * p;
            }

            if (i >= m - 1 && tot <= maxLetters)
                res = max(res, ++ hash[key]);
        }
        return res;
    }
};
```
#####  LeetCode 1298. 你能从盒子里获得的最大糖果数
```cpp
class Solution {
public:
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) {
        queue<int> q;
        unordered_set<int> S_closed, S_keys;

        for (auto x: initialBoxes)
            if (status[x]) q.push(x);
            else S_closed.insert(x);

        int res = 0;
        while (q.size()) {
            int t = q.front();
            q.pop();

            res += candies[t];
            for (auto x: keys[t]) {
                if (S_closed.count(x)) {
                    q.push(x);
                    S_closed.erase(x);
                } else {
                    S_keys.insert(x);
                }
            }

            for (auto x: containedBoxes[t]) {
                if (status[x]) q.push(x);
                else if (S_keys.count(x)) {
                    q.push(x);
                    S_keys.erase(x);
                } else {
                    S_closed.insert(x);
                }
            }
        }

        return res;
    }
};
```
#####  LeetCode 1299. 将每个元素替换为右侧最大元素
```cpp
class Solution {
public:
    vector<int> replaceElements(vector<int>& a) {
        int r = -1;
        for (int i = a.size() - 1; i >= 0; i -- ) {
            int t = a[i];
            a[i] = r;
            r = max(r, t);
        }
        return a;
    }
};
```
#####  LeetCode 1300. 转变数组后最接近目标值的数组和
```cpp
class Solution {
public:

    int get(vector<int>& a, int mid) {
        int res = 0;
        for (auto x: a)
            res += min(x, mid);
        return res;
    }

    int findBestValue(vector<int>& a, int target) {
        int sum = 0, mx = 0;
        for (auto x: a) sum += x, mx = max(mx, x);
        if (sum < target) return mx;

        int l = 0, r = 1e5;
        while (l < r) {
            int mid = l + r >> 1;
            if (get(a, mid) >= target) r = mid;
            else l = mid + 1;
        }

        int lower = get(a, r - 1), upper = get(a, r);
        if (target - lower <= upper - target)
            return r - 1;
        return r;
    }
};
```
#####  LeetCode 1301. 最大得分的路径数目
```cpp
class Solution {
public:
    vector<int> pathsWithMaxScore(vector<string>& w) {
        int n = w.size(), m = w[0].size(), INF = 1e8, MOD = 1e9 + 7;
        vector<vector<int>> f(n, vector<int>(m, -INF)), g(n, vector<int>(m));
        f[0][0] = 0, g[0][0] = 1;
        w[n - 1][m - 1] = '0';
        for (int i = 0; i < n; i ++ ) {
            for (int j = 0; j < m; j ++ ) {
                if (!i && !j || w[i][j] == 'X') continue;
                int &t = f[i][j], &r = g[i][j];
                if (i) t = max(t, f[i - 1][j]);
                if (j) t = max(t, f[i][j - 1]);
                if (i && j) t = max(t, f[i - 1][j - 1]);
                if (i && f[i - 1][j] == t)
                    r = (r + g[i - 1][j]) % MOD;
                if (j && f[i][j - 1] == t)
                    r = (r + g[i][j - 1]) % MOD;
                if (i && j && f[i - 1][j - 1] == t)
                    r = (r + g[i - 1][j - 1]) % MOD;
                t += w[i][j] - '0';
            }
        }
        if (f[n - 1][m - 1] < 0)
            return {0, 0};
        return {f[n - 1][m - 1], g[n - 1][m - 1]};
    }
};
```
#####  LeetCode 1302. 层数最深叶子节点的和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int depth = -1, sum = 0;

    void dfs(TreeNode* root, int d) {
        if (!root) return;
        if (d > depth) depth = d, sum = root->val;
        else if (d == depth) sum += root->val;

        dfs(root->left, d + 1);
        dfs(root->right, d + 1);
    }

    int deepestLeavesSum(TreeNode* root) {
        dfs(root, 0);
        return sum;
    }
};
```
#####  LeetCode 1304. 和为零的 N 个不同整数
```cpp
class Solution {
public:
    vector<int> sumZero(int n) {
        vector<int> res;
        for (int i = 1; i <= n / 2; i ++ ) {
            res.push_back(i);
            res.push_back(-i);
        }
        if (n % 2) res.push_back(0);
        return res;
    }
};
```


### 究极班- Week 61（第 1305 ~ 1307、1309 ~ 1315 题）

#####  LeetCode 1305. 两棵二叉搜索树中的所有元素
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, vector<int>& a) {
        if (!root) return;
        dfs(root->left, a);
        a.push_back(root->val);
        dfs(root->right, a);
    }

    vector<int> getAllElements(TreeNode* root1, TreeNode* root2) {
        vector<int> a, b;
        dfs(root1, a), dfs(root2, b);
        vector<int> res;
        int i = 0, j = 0;
        while (i < a.size() && j < b.size()) {
            if (a[i] < b[j]) res.push_back(a[i ++ ]);
            else res.push_back(b[j ++ ]);
        }
        while (i < a.size()) res.push_back(a[i ++ ]);
        while (j < b.size()) res.push_back(b[j ++ ]);
        return res;
    }
};
```
#####  LeetCode 1306. 跳跃游戏 III
```cpp
class Solution {
public:
    bool dfs(vector<int>& a, int k) {
        if (!a[k]) return true;
        int pos[] = {k - a[k], k + a[k]};
        a[k] = -1;
        for (auto x: pos) {
            if (x >= 0 && x < a.size() && a[x] != -1) {
                if (dfs(a, x)) return true;
            }
        }
        return false;
    }

    bool canReach(vector<int>& arr, int start) {
        return dfs(arr, start);
    }
};
```
#####  LeetCode 1307. 口算难题
```cpp
class Solution {
public:
    vector<string> words;
    string result;
    int p[300];
    bool st[10];

    bool dfs(int k, int carry) {
        if (k == result.size()) {
            if (carry) return false;
            for (auto& w: words) {
                if (w.size() > 1 && !p[w.back()]) return false;
            }
            if (result.size() > 1 && !p[result.back()]) return false;
            return true;
        }

        bool flag = true;
        int sum = carry;
        for (auto& w: words) {
            if (k >= w.size()) continue;
            char c = w[k];
            if (p[c] == -1) {
                flag = false;
                for (int i = 0; i < 10; i ++ ) {
                    if (!st[i]) {
                        p[c] = i;
                        st[i] = true;
                        if (dfs(k, carry)) return true;
                        st[i] = false;
                        p[c] = -1;
                    }
                }
            } else {
                sum += p[c];
            }
        }

        if (flag) {
            carry = sum / 10;
            sum %= 10;
            char c = result[k];
            if (p[c] == -1 && !st[sum]) {
                p[c] = sum;
                st[sum] = true;
                if (dfs(k + 1, carry)) return true;
                st[sum] = false;
                p[c] = -1;
            } else if (p[c] == sum) {
                if (dfs(k + 1, carry)) return true;
            }
        }

        return false;
    }

    bool isSolvable(vector<string>& words, string result) {
        memset(p, -1, sizeof p);
        memset(st, 0, sizeof st);

        for (auto& w: words) reverse(w.begin(), w.end());
        reverse(result.begin(), result.end());
        this->words = words;
        this->result = result;

        set<char> hash;
        int len = 0;
        for (auto& w: words) {
            for (auto c: w) {
                hash.insert(c);
            }
            len = max(len, (int)w.size());
        }
        for (auto c: result) hash.insert(c);
        if (hash.size() > 10) return false; 
        if (len > result.size() || result.size() - len > 1) return false;

        return dfs(0, 0);
    }
};
```
#####  LeetCode 1309. 解码字母到整数映射
```cpp
class Solution {
public:
    string freqAlphabets(string s) {
        string res;
        for (int i = 0; i < s.size(); i ++ ) {
            if (i + 2 < s.size() && s[i + 2] == '#') {
                res += 'j' + stoi(s.substr(i, 2)) - 10;
                i += 2;
            } else {
                res += 'a' + s[i] - '1';
            }
        }
        return res;
    }
};
```
#####  LeetCode 1310. 子数组异或查询
```cpp
class Solution {
public:
    vector<int> xorQueries(vector<int>& w, vector<vector<int>>& queries) {
        int n = w.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] ^ w[i - 1];
        vector<int> res;
        for (auto& q: queries) {
            int l = q[0] + 1, r = q[1] + 1;
            res.push_back(s[r] ^ s[l - 1]);
        }
        return res;
    }
};
```
#####  LeetCode 1311. 获取你好友已观看的视频
```cpp
class Solution {
public:
    vector<string> watchedVideosByFriends(vector<vector<string>>& watchedVideos, vector<vector<int>>& friends, int id, int level) {
        int n = friends.size();
        queue<int> q;
        vector<int> dist(n, 1e8);
        dist[id] = 0;
        q.push(id);

        while (q.size()) {
            auto t = q.front();
            q.pop();

            for (auto x: friends[t]) {
                if (dist[x] > dist[t] + 1) {
                    dist[x] = dist[t] + 1;
                    q.push(x);
                }
            }
        }

        unordered_map<string, int> hash;
        for (int i = 0; i < n; i ++ ) {
            if (dist[i] == level) {
                for (auto& v: watchedVideos[i]) {
                    hash[v] ++ ;
                }
            }
        }

        vector<pair<int, string>> items;
        for (auto& [k, v]: hash) items.push_back({v, k});
        sort(items.begin(), items.end());

        vector<string> res;
        for (auto& p: items) res.push_back(p.second);
        return res;
    }
};

```
#####  LeetCode 1312. 让字符串成为回文串的最少插入次数
```cpp
class Solution {
public:
    int minInsertions(string s) {
        int n = s.size();
        vector<vector<int>> f(n, vector<int>(n));

        for (int len = 2; len <= n; len ++ ) {
            for (int i = 0; i + len - 1 < n; i ++ ) {
                int j = i + len - 1;
                f[i][j] = min(f[i + 1][j], f[i][j - 1]) + 1;
                if (s[i] == s[j]) {
                    f[i][j] = min(f[i][j], f[i + 1][j - 1]);
                }
            }
        }

        return f[0][n - 1];
    }
};
```
#####  LeetCode 1313. 解压缩编码列表
```cpp
class Solution {
public:
    vector<int> decompressRLElist(vector<int>& nums) {
        vector<int> res;
        for (int i = 0; i < nums.size(); i += 2) {
            for (int j = 0; j < nums[i]; j ++ ) {
                res.push_back(nums[i + 1]);
            }
        }
        return res;
    }
};

```
#####  LeetCode 1314. 矩阵区域和
```cpp
class Solution {
public:
    vector<vector<int>> s;

    int get(int x1, int y1, int x2, int y2) {
        return s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1];
    }

    vector<vector<int>> matrixBlockSum(vector<vector<int>>& g, int k) {
        int n = g.size(), m = g[0].size();
        s = vector<vector<int>>(n + 1, vector<int>(m + 1));
        for (int i = 1; i <= n; i ++ ) {
            for (int j = 1; j <= m; j ++ ) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + g[i - 1][j - 1];
            }
        }

        vector<vector<int>> res(n, vector<int>(m));
        for (int i = 1; i <= n; i ++ ) {
            for (int j = 1; j <= m; j ++ ) {
                res[i - 1][j - 1] = get(max(i - k, 1), max(j - k, 1), min(n, i + k), min(m, j + k));
            }
        }

        return res;
    }
};
```
#####  LeetCode 1315. 祖父节点值为偶数的节点和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int ans = 0;

    void dfs(TreeNode* root, int p, int pg) {
        if (!root) return;
        if (pg % 2 == 0) ans += root->val;
        dfs(root->left, root->val % 2, p);
        dfs(root->right, root->val % 2, p);
    }

    int sumEvenGrandparent(TreeNode* root) {
        dfs(root, 1, 1);
        return ans;
    }
};

```


### 究极班- Week 62(第 1316 ~ 1320、1323 ~ 1326、1328 题)

#####  LeetCode 1316. 不同的循环子字符串
```cpp
typedef unsigned long long ULL;

const int N = 2010, P = 131;

ULL h[N], p[N];

class Solution {
public:
    ULL get(int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }

    int distinctEchoSubstrings(string text) {
        int n = text.size();
        p[0] = 1;
        for (int i = 1; i <= n; i ++ ) {
            p[i] = p[i - 1] * P;
            h[i] = h[i - 1] * P + text[i - 1];
        }

        unordered_set<ULL> hash;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; i + j * 2 <= n + 1; j ++ ) {
                auto left = get(i, i + j - 1);
                auto right = get(i + j, i + j * 2 - 1);
                if (left == right) hash.insert(left);
            }

        return hash.size();
    }
};

```
#####  LeetCode 1317. 将整数转换为两个无零整数的和
```cpp
class Solution {
public:
    vector<int> getNoZeroIntegers(int n) {
        for (int a = 1; a < n; a ++ ) {
            int b = n - a;
            if (to_string(a).find('0') == -1 && to_string(b).find('0') == -1)
                return {a, b};
        }
        return {};
    }
};
```
#####  LeetCode 1318. 或运算的最小翻转次数
```cpp
class Solution {
public:
    int minFlips(int a, int b, int c) {
        int res = 0;
        for (int i = 0; i < 30; i ++ ) {
            int x = a >> i & 1, y = b >> i & 1, z = c >> i & 1;
            if (!z) res += x + y;
            else if (!x && !y) res ++ ;
        }
        return res;
    }
};

```
#####  LeetCode 1319. 连通网络的操作次数
```cpp
class Solution {
public:
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int makeConnected(int n, vector<vector<int>>& connections) {
        if (connections.size() < n - 1) return -1;
        for (int i = 0; i < n; i ++ ) p.push_back(i);

        int res = n;
        for (auto& e: connections) {
            int a = e[0], b = e[1];
            if (find(a) != find(b)) {
                p[find(a)] = find(b);
                res -- ;
            }
        }
        return res - 1;
    }
};
```
#####  LeetCode 1320. 二指输入的的最小距离
```cpp
class Solution {
public:
    int get_dist(int x, int y) {
        return abs(x / 6 - y / 6) + abs(x % 6 - y % 6);
    }

    int minimumDistance(string word) {
        int n = word.size(), INF = 1e8;
        vector<vector<vector<int>>> f(n + 1, vector<vector<int>>(26, vector<int>(26, INF)));
        for (int i = 0; i < 26; i ++ )
            for (int j = 0; j < 26; j ++ )
                f[0][i][j] = 0;

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < 26; j ++ )
                for (int k = 0; k < 26; k ++ ) {
                    int t = word[i] - 'A';
                    f[i + 1][t][k] = min(f[i + 1][t][k], f[i][j][k] + get_dist(j, t));
                    f[i + 1][j][t] = min(f[i + 1][j][t], f[i][j][k] + get_dist(k, t));
                }

        int res = INF;
        for (int i = 0; i < 26; i ++ )
            for (int j = 0; j < 26; j ++ )
                res = min(res, f[n][i][j]);
        return res;
    }
};
```
#####  LeetCode 1323. 6 和 9 组成的最大数字
```cpp
class Solution {
public:
    int maximum69Number (int num) {
        string str = to_string(num);
        for (auto& c: str)
            if (c == '6') {
                c = '9';
                break;
            }
        return stoi(str);
    }
};
```
#####  LeetCode 1324. 竖直打印单词
```cpp
class Solution {
public:
    vector<string> printVertically(string s) {
        vector<string> res;
        int c = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == ' ') continue;
            int j = i, r = 0;
            while (j < s.size() && s[j] != ' ') {
                while (res.size() <= r) res.push_back("");
                while (res[r].size() <= c) res[r] += ' ';
                res[r][c] = s[j];
                j ++, r ++ ;
            }
            c ++, i = j;
        }
        return res;
    }
};

```
#####  LeetCode 1325. 删除给定值的叶子节点
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* removeLeafNodes(TreeNode* root, int target) {
        if (!root) return NULL;
        root->left = removeLeafNodes(root->left, target);
        root->right = removeLeafNodes(root->right, target);
        if (!root->left && !root->right && root->val == target)
            return NULL;
        return root;
    }
};


```
#####  LeetCode 1326. 灌溉花园的最少水龙头数目
```cpp
#define x first
#define y second

typedef pair<int, int> PII;

class Solution {
public:
    int minTaps(int n, vector<int>& ranges) {
        vector<PII> q;
        for (int i = 0; i < ranges.size(); i ++ ) {
            q.push_back({i - ranges[i], i + ranges[i]});
        }
        sort(q.begin(), q.end());
        int res = 0, r = 0;
        for (int i = 0; i < q.size(); i ++ ) {
            int j = i, mr = -1;
            while (j < q.size() && q[j].x <= r) mr = max(mr, q[j ++ ].y);
            if (mr == -1) return -1;
            res ++ ;
            if (mr >= n) return res;
            r = mr;
            i = j - 1;
        }

        return -1;
    }
};
```
#####  LeetCode 1328. 破坏回文串
```cpp
class Solution {
public:
    string breakPalindrome(string str) {
        if (str.size() == 1) return "";
        for (int i = 0; i < str.size() / 2; i ++ ) {
            if (str[i] != 'a' && (str.size() % 2 == 0 || i != str.size() / 2)) {
                str[i] = 'a';
                return str;
            }
        }

        str.back() = 'b';
        return str;
    }
};

```



### 究极班- Week 63(第 1329 ~ 1335、1337 ~ 1340 题)


#####  LeetCode 1329. 将矩阵按对角线排序
```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size();
        for (int b = -(n - 1); b <= m - 1; b ++ ) {
            vector<int> q;
            for (int x = 0, y = b; x < n && y < m; x ++, y ++ )
                if (y >= 0)
                    q.push_back(g[x][y]);
            sort(q.begin(), q.end());
            for (int x = 0, y = b, k = 0; x < n && y < m; x ++, y ++ )
                if (y >= 0)
                    g[x][y] = q[k ++ ];
        }
        return g;
    }
};
```
#####  LeetCode 1330. 翻转子数组得到最大的数组值
```cpp
class Solution {
public:
    int maxValueAfterReverse(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return 0;

        int sum = 0;
        for (int i = 1; i < n; i ++ )
            sum += abs(nums[i] - nums[i - 1]);
        int res = sum;
        for (int i = 1; i < n - 1; i ++ ) {
            res = max(res, sum - abs(nums[i] - nums[i + 1]) + abs(nums[0] - nums[i + 1]));
            res = max(res, sum - abs(nums[i] - nums[i - 1]) + abs(nums[i - 1] - nums.back()));
        }

        int a = max(nums[0], nums[1]), b = min(nums[0], nums[1]);
        for (int i = 2; i < n; i ++ ) {
            int c = nums[i - 1], d = nums[i];
            if (c < d) swap(c, d);
            if (c < b) res = max(res, sum + 2 * (b - c));
            if (a < d) res = max(res, sum + 2 * (d - a));
            b = max(b, min(c, d));
            a = min(a, max(c, d));
        }

        return res;
    }
};
```
#####  LeetCode 1331. 数组序号转换
```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& a) {
        auto b = a;
        sort(b.begin(), b.end());
        b.erase(unique(b.begin(), b.end()), b.end());
        vector<int> res;
        for (auto x: a)
            res.push_back(lower_bound(b.begin(), b.end(), x) - b.begin() + 1);
        return res;
    }
};
```
#####  LeetCode 1332. 删除回文子序列
```cpp
class Solution {
public:
    int removePalindromeSub(string s) {
        for (int i = 0, j = s.size() - 1; i < j; i ++, j -- )
            if (s[i] != s[j])
                return 2;
        return 1;
    }
};
```
#####  LeetCode 1333. 餐厅过滤器
```cpp
class Solution {
public:
    vector<int> filterRestaurants(vector<vector<int>>& restaurants, int veganFriendly, int maxPrice, int maxDistance) {
        vector<vector<int>> q;
        for (auto& r: restaurants) {
            if (veganFriendly && !r[2] || r[3] > maxPrice || r[4] > maxDistance) continue;
            q.push_back(r);
        }

        sort(q.begin(), q.end(), [](vector<int>& a, vector<int>& b) {
            if (a[1] != b[1]) return a[1] > b[1];
            return a[0] > b[0];
        });

        vector<int> res;
        for (auto& p: q) res.push_back(p[0]);
        return res;
    }
};

```
#####  LeetCode 1334. 阈值距离内邻居最少的城市
```cpp
class Solution {
public:
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<vector<int>> d(n, vector<int>(n, 1e8));
        for (int i = 0; i < n; i ++ ) d[i][i] = 0;
        for (auto& e: edges) {
            int a = e[0], b = e[1], c = e[2];
            d[a][b] = d[b][a] = min(d[a][b], c);
        }
        for (int k = 0; k < n; k ++ )
            for (int i = 0; i < n; i ++ )
                for (int j = 0; j < n; j ++ )
                    d[i][j] = min(d[i][j], d[i][k] + d[k][j]);

        int id = -1, min_cnt = n + 1;
        for (int i = 0; i < n; i ++ ) {
            int cnt = 0;
            for (int j = 0; j < n; j ++ )
                if (i != j && d[i][j] <= distanceThreshold)
                    cnt ++ ;
            if (cnt <= min_cnt) {
                min_cnt = cnt;
                id = i;
            }
        }
        return id;
    }
};

```
#####  LeetCode 1335. 工作计划的最低难度
```cpp
class Solution {
public:
    int minDifficulty(vector<int>& w, int d) {
        int n = w.size(), INF = 1e8;
        vector<vector<int>> f(n + 1, vector<int>(d + 1, INF));
        f[0][0] = 0;
        for (int i = 1; i <= n; i ++ ) {
            for (int j = 1; j <= d; j ++ ) {
                int cost = 0;
                for (int k = 1; k <= i; k ++ ) {
                    cost = max(cost, w[i - k]);
                    f[i][j] = min(f[i][j], f[i - k][j - 1] + cost);
                }
            }
        }
        if (f[n][d] == INF) return -1;
        return f[n][d];
    }
};
```
#####  LeetCode 1337. 矩阵中战斗力最弱的 K 行
```cpp
class Solution {
public:
    vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
        vector<int> cnt;
        for (auto& line: mat) {
            int c = 0;
            for (auto x: line)
                c += x;
            cnt.push_back(c);
        }

        vector<int> res;
        for (int i = 0; i < mat.size(); i ++ )
            res.push_back(i);
        sort(res.begin(), res.end(), [&](int a, int b) {
            if (cnt[a] != cnt[b]) return cnt[a] < cnt[b];
            return a < b;
        });

        res.erase(res.begin() + k, res.end());
        return res;
    }
};

```
#####  LeetCode 1338. 数组大小减半
```cpp
class Solution {
public:
    int minSetSize(vector<int>& arr) {
        unordered_map<int, int> hash;
        for (auto x: arr) hash[x] ++ ;
        vector<int> cnt;
        for (auto& [k, v]: hash) cnt.push_back(v);
        sort(cnt.begin(), cnt.end(), greater<int>());

        for (int i = 0, sum = 0; i < cnt.size(); i ++ ) {
            sum += cnt[i];
            if (sum * 2 >= arr.size())
                return i + 1;
        }
        return 0;
    }
};
```
#####  LeetCode 1339. 分裂二叉树的最大乘积
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
typedef long long LL;

class Solution {
public:
    const int MOD = 1e9 + 7;
    LL tot = 0, ans = 0;

    int dfs(TreeNode* root) {
        if (!root) return 0;
        int sum = root->val;
        sum += dfs(root->left) + dfs(root->right);
        ans = max(ans, sum * (tot - sum));
        return sum;
    }

    int maxProduct(TreeNode* root) {
        tot = dfs(root);
        dfs(root);
        return ans % MOD;
    }
};

```
#####  LeetCode 1340. 跳跃游戏 V

```cpp
class Solution {
public:
    int minJumps(vector<int>& w) {
        unordered_map<int, vector<int>> hash;
        int n = w.size(), INF = 1e9;
        for (int i = 0; i < n; i ++ )
            hash[w[i]].push_back(i);

        vector<int> dist(n, INF);
        queue<int> q;
        dist[0] = 0;
        q.push(0);

        while (q.size()) {
            auto t = q.front();
            q.pop();

            for (int i = t - 1; i <= t + 1; i += 2) {
                if (i >= 0 && i < n && dist[i] > dist[t] + 1) {
                    dist[i] = dist[t] + 1;
                    q.push(i);
                }
            }

            int val = w[t];
            if (hash.count(val)) {
                for (int i: hash[val]) {
                    if (dist[i] > dist[t] + 1) {
                        dist[i] = dist[t] + 1;
                        q.push(i);
                    }
                }
                hash.erase(val);
            }
        }

        return dist[n - 1];
    }
};
```

### 究极班- Week 64(第 1342 ~ 1349、1351 ~ 1354、1356 ~ 1358 题)


#####  LeetCode 1342. 将数字变成 0 的操作次数
```cpp
class Solution {
public:
    int numberOfSteps(int num) {
        int res = 0;
        while (num) {
            if (num & 1) num -- ;
            else num >>= 1;
            res ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 1343. 大小为 K 且平均值大于等于阈值的子数组数目
```cpp
class Solution {
public:
    int numOfSubarrays(vector<int>& a, int k, int threshold) {
        int res = 0;
        for (int i = 0, j = 0, s = 0; i < a.size(); i ++ ) {
            s += a[i];
            if (i - j >= k) s -= a[j ++ ];
            if (i - j + 1 == k && s >= k * threshold) res ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 1344. 时钟指针的夹角
```cpp
class Solution {
public:
    double angleClock(int hour, int minutes) {
        hour %= 12;
        double x = minutes * 6;
        double y = hour * 30 + minutes * 0.5;
        return min(abs(x - y), 360 - abs(x - y));
    }
};

```
#####  LeetCode 1345. 跳跃游戏 IV
```cpp
class Solution {
public:
    int minJumps(vector<int>& w) {
        unordered_map<int, vector<int>> hash;
        int n = w.size(), INF = 1e9;
        for (int i = 0; i < n; i ++ )
            hash[w[i]].push_back(i);

        vector<int> dist(n, INF);
        queue<int> q;
        dist[0] = 0;
        q.push(0);

        while (q.size()) {
            auto t = q.front();
            q.pop();

            for (int i = t - 1; i <= t + 1; i += 2) {
                if (i >= 0 && i < n && dist[i] > dist[t] + 1) {
                    dist[i] = dist[t] + 1;
                    q.push(i);
                }
            }

            int val = w[t];
            if (hash.count(val)) {
                for (int i: hash[val]) {
                    if (dist[i] > dist[t] + 1) {
                        dist[i] = dist[t] + 1;
                        q.push(i);
                    }
                }
                hash.erase(val);
            }
        }

        return dist[n - 1];
    }
};
```
#####  LeetCode 1346. 检查整数及其两倍数是否存在
```cpp
class Solution {
public:
    bool checkIfExist(vector<int>& w) {
        unordered_set<int> hash;
        for (auto x: w) {
            if (hash.count(x * 2) || x % 2 == 0 && hash.count(x / 2))
                return true;
            hash.insert(x);
        }
        return false;
    }
};-

```
