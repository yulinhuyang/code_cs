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














