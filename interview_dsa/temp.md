##### Leetcode 139 单词拆分

单词问题：双指针dp

双指针正向dp解法

双指针逆向dp解法

```C++
class Solution {
    unordered_set<string> hash;
    vector<int> f;
    int n;
public:
    bool wordBreak(string s, vector<string> &wordDict) {
        for (auto &word:wordDict) {
            hash.insert(word);
        }
        n = s.size();
        //f[i]对应s的i-1
        f.resize(n + 1, 0);
        f[n] = true;
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (f[j + 1] && hash.count(s.substr(i, j - i + 1))) {
                    f[i] = 1;
                }
            }
        }

        return f[0];
    }
};
```
#### Leetcode 140 单词拆分 II

dp方向和dfs方向相反，反向递推方案

```C++
class Solution {
    unordered_set<string> hash;
    vector<string> ans;
    vector<int> f;
    int n;
public:
    vector<string> wordBreak(string s, vector<string> &wordDict) {
        for (auto &word:wordDict) {
            hash.insert(word);
        }
        n = s.size();
        //f[i]对应s的i-1
        f.resize(n + 1, 0);
        f[n] = true;
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (f[j + 1] && hash.count(s.substr(i, j - i + 1))) {
                    f[i] = 1;
                }
            }
        }

        //dfs方向与dp相反，反向递推
        if (f[0]) dfs(s, 0, "");
        return ans;
    }

    //u是len,i是索引
    void dfs(string s, int u, string path) {
        if (u == n) {
            path.pop_back();
            ans.emplace_back(path);
        }
        for (int i = u; i < n; i++) {
            if (f[i + 1] && hash.count(s.substr(u, i - u + 1))) {
                dfs(s, i + 1, path + s.substr(u, i - u + 1) + " ");
            }
        }
    }
};

```

##### Leetcode 150 逆波兰表达式求值

逆波兰记法,后缀表示法

```C++
class Solution {
public:
    int evalRPN(vector<string> &tokens) {
        stack<int> stk;
        for (auto &token:tokens) {
            if (token == "+" || token == "-" || token == "*" || token == "/") {
                auto b = stk.top();
                stk.pop();
                auto a = stk.top();
                stk.pop();
                if (token == "+") a += b;
                else if (token == "-") a -= b;
                else if (token == "*") a *= b;
                else a /= b;
                stk.push(a);
            } else {
                stk.push(stoi(token));
            }
        }
        return stk.top();
    }
};
```

##### 135 分发糖果

双向遍历

```C++
class Solution {
public:
    int candy(vector<int> &ratings) {
        int res = 0;
        int n = ratings.size();
        vector<int> left(n, 1);
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                left[i] = left[i - 1] + 1;
            }
        }

        int right = 0;
        for (int i = n - 1; i >= 0; i--) {
            if (i < n - 1 && ratings[i] > ratings[i + 1]) {
                right++; //记递增序列
            } else {
                right = 1;
            }
            res += max(left[i], right);
        }

        return res;
    }
};
```
-1 取反是0，反向循环的时候，~-1 可以作为截止条件



##### Leetcode 143  重排链表

模板题：寻找中点+ 逆序右半段 + 合并

```C++
class Solution {
public:
    void reorderList(ListNode *head) {
        if (!head) return;
        ListNode *mid = middle(head);
        ListNode *L1 = head;
        ListNode *L2 = mid->next;
        mid->next = nullptr;
        L2 = reverseList(L2);
        mergeList(L1, L2);
    }

    ListNode *middle(ListNode *head) {
        auto fast = head;
        auto slow = head;
        while (fast->next && fast->next->next) {
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }

    ListNode *reverseList(ListNode *head) {
        ListNode *prev = nullptr;
        ListNode *cur = head;
        while (cur) {
            auto nextTmp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = nextTmp;
        }
        return prev;
    }

    void mergeList(ListNode *L1, ListNode *L2) {
        ListNode *L1_next_tmp;
        ListNode *L2_next_tmp;
        while (L1 && L2) {
            L1_next_tmp = L1->next;
            L2_next_tmp = L2->next;

            L1->next = L2;
            L1 = L1_next_tmp;

            L2->next = L1_next_tmp;
            L2 = L2_next_tmp;
        }
    }
};

```

##### 136 只出现一次的数字

```C++
class Solution {
public:
    int singleNumber(vector<int> &nums) {
        //要自己初始化，否则one的值随机的
        int one = 0;
        for(auto num:nums) {
            one ^= num;
        }
        return one;
    }
};
```

 25 43 51  75  77 100 114   






