##### Leetcode 116 填充每个节点的下一个右侧节点指针

使用已建立的next 指针，位于第N层时为第N+1层建立next 指针

局部变量不会初始化的，是随机值；全局变量才会初始化成默认的，比如指针类型会初始化为空

```C++
class Solution {
public:
    Node *connect(Node *root) {
        if (!root) return nullptr;

        auto res = root;
        while (root->left) {
            //遍历一层
            for (auto p = root; p; p = p->next) {
                p->left->next = p->right;
                if(p->next) p->right->next = p->next->left;
            }
            //挪到下一层最左
            root = root->left;
        }
        return res;
    }
};

```

#### Leetcode 117  填充每个节点的下一个右侧节点指针 II

位于第N层时为第N+1层建立next 指针

```C++
class Solution {
public:
    Node *connect(Node *root) {
        if (!root) return nullptr;
        auto cur = root;
        while (cur) {
            Node *head = new Node(-1);
            auto tail = head;
            //遍历一层,建立下一层
            for (auto p = cur; p; p = p->next) {
                if (p->left) tail = tail->next = p->left;
                if (p->right) tail = tail->next = p->right;
            }
            //挪到下一层最左
            cur = head->next;
        }
        return root;
    }
};
```

##### 118  杨辉三角 I

```C+++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> res;
        for (int i = 0; i < numRows; i++) {
            vector<int> line(i + 1, 0);
            line[0] = line[i] = 1;
            for (int j = 1; j < i; j++) {
                line[j] = res[i - 1][j - 1] + res[i - 1][j];
            }
            res.emplace_back(line);
        }
        return res;
    }
};

```

##### 119  杨辉三角 II

```C++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<vector<int>> f(2, vector<int>(rowIndex + 1, 0));
        for (int i = 0; i <= rowIndex; i++) {
            f[i & 1][0] = f[i & 1][i] = 1;
            for (int j = 1; j < i; j++) {
                f[i & 1][j] = f[i - 1 & 1][j - 1] + f[i - 1 & 1][j];
            }
        }
        return f[rowIndex & 1];
    }
};

```
##### 125 验证回文串

```C++
class Solution {
    bool check(char c) {
        return c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z' || c >= '0' && c <= '9';
    }

public:
    bool isPalindrome(string s) {
        int n = s.size();
        for (int i = 0, j = n - 1; i < j; i++, j--) {
            while (i < j && !check(s[i])) i++;
            while (i < j && !check(s[j])) j--;
            if (i < j && tolower(s[i]) != tolower(s[j])) return false;
        }
        return true;
    }
};
```

##### Leetcode 129  求根节点到叶节点数字之和

前判空好于后判空

```C++
class Solution {
    int ans;
public:
    int sumNumbers(TreeNode *root) {
        if (root) dfs(root, 0);
        return ans;
    }

    void dfs(TreeNode *root, int num) {
        num = num * 10 + root->val;
        if (!root->left && !root->right) ans += num;
        if (root->left) dfs(root->left, num);
        if (root->right) dfs(root->right, num);
    }
};

```


