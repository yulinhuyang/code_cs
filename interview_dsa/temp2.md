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

##### Leetcode 133 克隆图

```C++
class Solution {
    unordered_map<Node *, Node *> hash;
public:
    Node *cloneGraph(Node *node) {
        dfs(node);

        for (auto[s, d]:hash) {
            for (auto ver:s->neighbors) {
                d->neighbors.push_back(hash[ver]);
            }
        }

        return hash[node];
    }

    void dfs(Node *node) {
        hash[node] = new Node(node->val);

        for (auto ver:node->neighbors) {
            if (!hash.count(ver)) {
                dfs(ver);
            }
        }
    }
};
```
#### Leetcode 138 复制带随机指针的链表

```C++
class Solution {
public:
    Node *copyRandomList(Node *head) {
        for (auto p = head; p; p = p->next->next) {
            Node *node = new Node(p->val);
            node->next = p->next;
            p->next = node;
        }

        for (auto p = head; p; p = p->next->next) {
            if (p->random) p->next->random = p->random->next;
        }

        Node *dummy = new Node(-1);
        auto cur = dummy;
        for (auto p = head; p; p = p->next) {
            cur->next = p->next;
            cur = cur->next;
            p->next = p->next->next;
        }

        return dummy->next;
    }
};

```

##### 94  中序遍历

```C++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> ans;
        if(!root) return ans;

        while(root || stk.size()){
            //循环一侧走
            while(root){
                stk.push(root);
                root = root->left;
            }
            //出栈切方向
            root = stk.top();
            stk.pop();
            ans.emplace_back(root->val);
            root = root->right;
        }

        return ans;
    }
};

```
##### 144 二叉树的前序遍历

循环一侧走，出栈切方向

```C++
//递归
class Solution {
    vector<int> ans;
public:
    vector<int> preorderTraversal(TreeNode *root) {
        if (root) dfs(root);
        return ans;
    }

    void dfs(TreeNode *root) {
        ans.emplace_back(root->val);
        if (root->left) dfs(root->left);
        if (root->right) dfs(root->right);
    }
};

//迭代
class Solution {

public:
    vector<int> preorderTraversal(TreeNode *root) {
        vector<int> ans;
        stack<TreeNode *> stk;
        if (!root) return ans;

        while (root || !stk.empty()) {
            //循环一侧走
            while (root) {
                ans.emplace_back(root->val);
                stk.push(root);
                root = root->left;
            }
            //出栈切方向
            root = stk.top();
            stk.pop();
            root = root->right;
        }

        return ans;
    }
};

```

##### 145 二叉树的后序遍历

迭代：左右根 reverse 根右左。

```C++
//递归
class Solution {
    vector<int> ans;
public:
    vector<int> postorderTraversal(TreeNode *root) {
        if (root) dfs(root);
        return ans;
    }

    void dfs(TreeNode *root) {
        if (root->left) dfs(root->left);
        if (root->right) dfs(root->right);
        ans.emplace_back(root->val);
    }
};


//迭代
class Solution {
public:
    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> ans;
        stack<TreeNode *> stk;
        if(!root) return ans;
        
        while(root || !stk.empty()){
            while(root){
                ans.emplace_back(root->val);
                stk.push(root);
                root = root->right;
            }
            root = stk.top();
            stk.pop();
            root = root->left;
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```



