##### Offer 13. 找出数组中重复的数字
```C++
class Solution {
public:
    int duplicateInArray(vector<int>& nums) {
        int n = nums.size();
        for (auto x : nums)
            if (x < 0 || x >= n)
                return -1;

        for (int i = 0; i < nums.size(); i ++ ) {
            while (nums[i] != i) {
                if (nums[i] == nums[nums[i]])
                    return nums[i];
                swap(nums[i], nums[nums[i]]);
            }
        }

        return -1;
    }
};

```
##### Offer 15. 二维数组中的查找
```C++
class Solution {
public:
    bool searchArray(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;
        int i = 0, j = matrix[0].size() - 1;
        while (i < matrix.size() && j >= 0)
        {
            int t = matrix[i][j];
            if (t == target) return true;
            if (t > target) j -- ;
            else i ++ ;
        }
        return false;
    }
};
```
##### Offer 16. 替换空格
```C++
class Solution {
public:
    string replaceSpaces(string &str) {
        string res;
        for (auto c : str)
            if (c == ' ')
                res += "%20";
            else
                res += c;
        return res;
    }
};
```
##### Offer 17. 从尾到头打印链表
```C++
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
    vector<int> printListReversingly(ListNode* head) {
        vector<int> res;
        while (head) {
            res.push_back(head->val);
            head = head->next;
        }
        return vector<int>(res.rbegin(), res.rend());
    }
};
```
##### Offer 18. 重建二叉树
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
    unordered_map<int,int> pos;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        for (int i = 0; i < n; i ++ )
            pos[inorder[i]] = i;
        return dfs(preorder, inorder, 0, n - 1, 0, n - 1);
    }

    TreeNode* dfs(vector<int>&pre, vector<int>&in, int pl, int pr, int il, int ir)
    {
        if (pl > pr) return NULL;
        int k = pos[pre[pl]] - il;
        TreeNode* root = new TreeNode(pre[pl]);
        root->left = dfs(pre, in, pl + 1, pl + k, il, il + k - 1);
        root->right = dfs(pre, in, pl + k + 1, pr, il + k + 1, ir);
        return root;
    }
};
```
##### Offer 19. 二叉树的下一个节点
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode *father;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL), father(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode *p) {
        if (!p) return NULL;
        if (p->right) {
            p = p->right;
            while (p->left) p = p->left;
            return p;
        }

        while (p->father && p->father->right == p) {
            p = p->father;
        }

        if (p->father) return p->father;
        return NULL;
    }
};
```
##### Offer 20. 用两个栈实现队列
```C++
class MyQueue {
public:
    /** Initialize your data structure here. */
    stack<int>sta, cache;
    MyQueue() {

    }

    /** Push element x to the back of queue. */
    void push(int x) {
        sta.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        while (!sta.empty()) cache.push(sta.top()), sta.pop();
        int x = cache.top();
        cache.pop();
        while (!cache.empty()) sta.push(cache.top()), cache.pop();
        return x;
    }

    /** Get the front element. */
    int peek() {
        while (!sta.empty()) cache.push(sta.top()), sta.pop();
        int x = cache.top();
        while (!cache.empty()) sta.push(cache.top()), cache.pop();
        return x;
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        return sta.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * bool param_4 = obj.empty();
 */
```
##### Offer 21. 斐波那契数列
```C++
class Solution {
public:
    int Fibonacci(int n) {
        int a = 0, b = 1;
        while (n -- ) {
            int c = a + b;
            a = b, b = c;
        }
        return a;
    }
};
```
##### Offer 14. 不修改数组找出重复的数字
```C++
class Solution {
public:
    int duplicateInArray(vector<int>& nums) {
        int l = 1, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            int s = 0;
            for (auto x : nums) s += x >= l && x <= mid;
            if (s > mid - l + 1) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
##### Offer 22. 旋转数组的最小数字
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size() - 1;
        if (n < 0) return -1;
        while (n > 0 && nums[n] == nums[0]) n -- ;
        if (nums[n] >= nums[0]) return nums[0];
        int l = 0, r = n;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] < nums[0]) r = mid;
            else l = mid + 1;
        }
        return nums[r];
    }
};
```
##### Offer 23. 矩阵中的路径
```C++
class Solution {
public:
    bool hasPath(vector<vector<char>>& matrix, string str) {
        for (int i = 0; i < matrix.size(); i ++ )
            for (int j = 0; j < matrix[i].size(); j ++ )
                if (dfs(matrix, str, 0, i, j))
                    return true;
        return false;
    }

    bool dfs(vector<vector<char>> &matrix, string &str, int u, int x, int y) {
        if (matrix[x][y] != str[u]) return false;
        if (u == str.size() - 1) return true;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        char t = matrix[x][y];
        matrix[x][y] = '*';
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < matrix.size() && b >= 0 && b < matrix[a].size()) {
                if (dfs(matrix, str, u + 1, a, b)) return true;
            }
        }
        matrix[x][y] = t;
        return false;
    }
};
```
##### Offer 48. 复杂链表的复刻
```C++
/**
 * Definition for singly-linked list with a random pointer.
 * struct ListNode {
 *     int val;
 *     ListNode *next, *random;
 *     ListNode(int x) : val(x), next(NULL), random(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *copyRandomList(ListNode *head) {
        for (auto p = head; p;)
        {
            auto np = new ListNode(p->val);
            auto next = p->next;
            p->next = np;
            np->next = next;
            p = next;
        }

        for (auto p = head; p; p = p->next->next)
        {
            if (p->random)
                p->next->random = p->random->next;
        }

        auto dummy = new ListNode(-1);
        auto cur = dummy;
        for (auto p = head; p; p = p->next)
        {
            cur->next = p->next;
            cur = cur->next;
            p->next = p->next->next;
        }

        return dummy->next;
    }
};
```
##### Offer 32. 调整数组顺序使奇数位于偶数前面
```C++
class Solution {
public:
    void reOrderArray(vector<int> &array) {
         int l = 0, r = array.size() - 1;
         while (l <= r) {
             while (array[l] % 2 == 1) l ++ ;
             while (array[r] % 2 == 0) r -- ;
             if (l < r) swap(array[l], array[r]);
         }
    }
};
```
##### Offer 24. 机器人的运动范围
```C++
class Solution {
public:

    int get_sum(pair<int, int> p) {
        int s = 0;
        while (p.first) {
            s += p.first % 10;
            p.first /= 10;
        }
        while (p.second) {
            s += p.second % 10;
            p.second /= 10;
        }
        return s;
    }

    int movingCount(int threshold, int rows, int cols)
    {
        if (!rows || !cols) return 0;
        queue<pair<int,int>> q;
        vector<vector<bool>> st(rows, vector<bool>(cols, false));

        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

        int res = 0;
        q.push({0, 0});
        while (q.size()) {
            auto t = q.front();
            q.pop();
            if (st[t.first][t.second] || get_sum(t) > threshold) continue;
            res ++ ;
            st[t.first][t.second] = true;
            for (int i = 0; i < 4; i ++ ) {
                int x = t.first + dx[i], y = t.second + dy[i];
                if (x >= 0 && x < rows && y >= 0 && y < cols) q.push({x, y});
            }
        }

        return res;
    }
};
```
##### Offer 25. 剪绳子
```C++
class Solution {
public:
    int maxProductAfterCutting(int n) {
        if (n <= 3) return 1 * (n - 1);
        int res = 1;
        if (n % 3 == 1) n -= 4, res *= 4;
        if (n % 3 == 2) n -= 2, res *= 2;
        while (n) n -= 3, res *= 3;
        return res;
    }
};
```
##### Offer 26. 二进制中1的个数
```C++
class Solution {
public:
    int NumberOf1(int n) {
        int res = 0;
        unsigned int un = n; 
        while (un) res += un & 1, un >>= 1;
        return res;
    }
};
```
##### Offer 27. 数值的整数次方
```C++
class Solution {
public:
    double Power(double x, int n) {
        typedef long long LL;
        bool is_minus = n < 0;
        long double res = 1;
        for (LL k = abs(LL(n)); k; k >>= 1) {
            if (k & 1) res *= x;
            x *= x;
        }
        if (is_minus) res = 1 / res;
        return res;
    }
};
```
##### Offer 28. 在O(1)时间删除链表结点
```C++
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
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next=node->next->next;
    }
};
```
##### Offer 29. 删除链表中重复的节点
```C++
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
    ListNode* deleteDuplication(ListNode* head) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* p = dummy;
        while (p->next)
        {
            ListNode* q = p->next;
            while (q && q->val == p->next->val)
                q = q->next;
            if (p->next->next == q) p = p->next;
            else p->next = q;
        }
        return dummy->next;
    }
};
```
##### Offer 30. 正则表达式匹配
```C++
class Solution {
public:
    vector<vector<int>>f;
    int n, m;
    bool isMatch(string s, string p) {
        n = s.size();
        m = p.size();
        f = vector<vector<int>>(n + 1, vector<int>(m + 1, -1));
        f[n][m] = 1;
        dp(0, 0, s, p);
        return dp(0, 0, s, p);
    }

    int dp(int x, int y, string &s, string &p)
    {
        if (f[x][y] != -1) return f[x][y];
        if (y == m)
            return f[x][y] = x == n;
        bool first_match = x < n && (s[x] == p[y] || p[y] == '.');
        bool ans;
        if (y + 1 < m && p[y + 1] == '*')
        {
            ans = dp(x, y + 2, s, p) || first_match && dp(x + 1, y, s, p);
        }
        else
            ans = first_match && dp(x + 1, y + 1, s, p);
        return f[x][y] = ans;
    }
};
```
##### Offer 31. 表示数值的字符串
```C++
class Solution {
public:
    bool isNumber(string s) {
        int i = 0;
        while (i < s.size() && s[i] == ' ') i ++ ;
        int j = s.size() - 1;
        while (j >= 0 && s[j] == ' ') j -- ;
        if (i > j) return false;
        s = s.substr(i, j - i + 1);

        if (s[0] == '-' || s[0] == '+') s = s.substr(1);
        if (s.empty() || s[0] == '.' && s.size() == 1) return false;

        int dot = 0, e = 0;
        for (int i = 0; i < s.size(); i ++ )
        {
            if (s[i] >= '0' && s[i] <= '9');
            else if (s[i] == '.')
            {
                dot ++ ;
                if (e || dot > 1) return false;
            }
            else if (s[i] == 'e' || s[i] == 'E')
            {
                e ++ ;
                if (i + 1 == s.size() || !i || e > 1 || i == 1 && s[0] == '.') return false;
                if (s[i + 1] == '+' || s[i + 1] == '-')
                {
                    if (i + 2 == s.size()) return false;
                    i ++ ;
                }
            }
            else return false;
        }
        return true;
    }
};
```
##### Offer 33. 链表中倒数第k个节点
```C++
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
    ListNode* findKthToTail(ListNode* head, int k) {
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;
        if (n < k) return nullptr;
        auto p = head;
        for (int i = 0; i < n - k; i ++ ) p = p->next;
        return p;
    }
};
```
##### Offer 34. 链表中环的入口结点
```C++
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
    ListNode *entryNodeOfLoop(ListNode *head) {
        if (!head || !head->next) return 0;
        ListNode *first = head, *second = head;

        while (first && second)
        {
            first = first->next;
            second = second->next;
            if (second) second = second->next;
            else return 0;

            if (first == second)
            {
                first = head;
                while (first != second)
                {
                    first = first->next;
                    second = second->next;
                }
                return first;
            }
        }

        return 0;
    }
};
```
##### Offer 35. 反转链表
```C++
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
    ListNode* reverseList(ListNode* head) {
        ListNode *prev = nullptr;
        ListNode *cur = head;
        while (cur)
        {
            ListNode *next = cur->next;
            cur->next = prev;
            prev = cur, cur = next;
        }
        return prev;
    }
};
```
##### Offer 36. 合并两个排序的链表
```C++
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
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode *dummy = new ListNode(0);
        ListNode *cur = dummy;
        while (l1 != NULL && l2 != NULL) {
            if (l1 -> val < l2 -> val) {
                cur -> next = l1;
                l1 = l1 -> next;
            }
            else {
                cur -> next = l2;
                l2 = l2 -> next;
            }
            cur = cur -> next;
        }
        cur -> next = (l1 != NULL ? l1 : l2);
        return dummy -> next;
    }
};
```
##### Offer 37. 树的子结构
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
    bool hasSubtree(TreeNode* pRoot1, TreeNode* pRoot2) {
        if (!pRoot1 || !pRoot2) return false;
        if (isSame(pRoot1, pRoot2)) return true;
        return hasSubtree(pRoot1->left, pRoot2) || hasSubtree(pRoot1->right, pRoot2);
    }

    bool isSame(TreeNode* pRoot1, TreeNode* pRoot2) {
        if (!pRoot2) return true;
        if (!pRoot1 || pRoot1->val != pRoot2->val) return false;
        return isSame(pRoot1->left, pRoot2->left) && isSame(pRoot1->right, pRoot2->right);
    }
};
```
##### Offer 38. 二叉树的镜像
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
    void mirror(TreeNode* root) {
        if (!root) return;
        swap(root->left, root->right);
        mirror(root->left);
        mirror(root->right);
    }
};
```
##### Offer 39. 对称的二叉树
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
    bool isSymmetric(TreeNode* root) {
        return !root || dfs(root->left, root->right);
    }

    bool dfs(TreeNode*p, TreeNode*q)
    {
        if (!p || !q) return !p && !q;
        return p->val == q->val && dfs(p->left, q->right) && dfs(p->right, q->left);
    }
};
```
##### Offer 40. 顺时针打印矩阵
```C++
class Solution {
public:
    vector<int> printMatrix(vector<vector<int>>& matrix) {
        vector<int> res;
        if (matrix.empty()) return res;
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<bool>> st(n, vector<bool>(m, false));
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        int x = 0, y = 0, d = 1;
        for (int k = 0; k < n * m; k ++ )
        {
            res.push_back(matrix[x][y]);
            st[x][y] = true;

            int a = x + dx[d], b = y + dy[d];
            if (a < 0 || a >= n || b < 0 || b >= m || st[a][b])
            {
                d = (d + 1) % 4;
                a = x + dx[d], b = y + dy[d];
            }
            x = a, y = b;
        }
        return res;
    }
};
```
##### Offer 41. 包含min函数的栈
```C++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> stackValue;
    stack<int> stackMin;
    MinStack() {

    }

    void push(int x) {
        stackValue.push(x);
        if (stackMin.empty() || stackMin.top() >= x)
            stackMin.push(x);
    }

    void pop() {
        if (stackMin.top() == stackValue.top()) stackMin.pop();
        stackValue.pop();
    }

    int top() {
        return stackValue.top();
    }

    int getMin() {
        return stackMin.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```
##### Offer 42. 栈的压入、弹出序列
```C++
class Solution {
public:
    bool isPopOrder(vector<int> pushV,vector<int> popV) {
        if(popV.size()!=pushV.size()) return false;
        stack<int> s;
        int index=0;
        for(int i=0;i<pushV.size();i++){
            s.push(pushV[i]);
            while(!s.empty() && s.top()==popV[index]){
                s.pop();
                index++;
            }
        }
        if(s.empty())
            return true;
        return false;
    }
};
```
##### Offer 43. 不分行从上往下打印二叉树
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
    vector<int> printFromTopToBottom(TreeNode* root) {
        vector<int> res;
        if (!root) return res;
        queue<TreeNode*> q;
        q.push(root);

        while (q.size()) {
            auto t = q.front();
            q.pop();
            res.push_back(t->val);
            if (t->left) q.push(t->left);
            if (t->right) q.push(t->right);
        }

        return res;
    }
};
```
##### Offer 45. 之字形打印二叉树
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
    vector<int> get_val(vector<TreeNode*> level)
    {
        vector<int> res;
        for (auto &u : level)
            res.push_back(u->val);
        return res;
    }

    vector<vector<int>> printFromTopToBottom(TreeNode* root) {
        vector<vector<int>>res;
        if (!root) return res;
        vector<TreeNode*>level;
        level.push_back(root);
        res.push_back(get_val(level));
        bool zigzag = true;
        while (true)
        {
            vector<TreeNode*> newLevel;
            for (auto &u : level)
            {
                if (u->left) newLevel.push_back(u->left);
                if (u->right) newLevel.push_back(u->right);
            }
            if (newLevel.size())
            {
                vector<int>temp = get_val(newLevel);
                if (zigzag)
                    reverse(temp.begin(), temp.end());
                res.push_back(temp);
                level = newLevel;
            }
            else break;
            zigzag = !zigzag;
        }
        return res;
    }
};
```
##### Offer 44. 分行从上往下打印二叉树
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
    vector<int> get_val(vector<TreeNode*> level)
    {
        vector<int> res;
        for (auto &u : level)
            res.push_back(u->val);
        return res;
    }

    vector<vector<int>> printFromTopToBottom(TreeNode* root) {
        vector<vector<int>>res;
        if (!root) return res;
        vector<TreeNode*>level;
        level.push_back(root);
        res.push_back(get_val(level));
        while (true)
        {
            vector<TreeNode*> newLevel;
            for (auto &u : level)
            {
                if (u->left) newLevel.push_back(u->left);
                if (u->right) newLevel.push_back(u->right);
            }
            if (newLevel.size())
            {
                res.push_back(get_val(newLevel));
                level = newLevel;
            }
            else break;
        }
        return res;
    }
};
```
##### Offer 46. 二叉搜索树的后序遍历序列
```C++
class Solution {
public:

    vector<int> seq;

    bool verifySequenceOfBST(vector<int> sequence) {
        seq = sequence;
        if (seq.empty()) return true;
        return dfs(0, seq.size() - 1);
    }

    bool dfs(int l, int r) {
        if (l >= r) return true;
        int root = seq[r];
        int k = l;
        while (k < r && seq[k] < root) k ++ ;
        for (int i = k; i < r; i ++ )
            if (seq[i] < root)
                return false;
        return dfs(l, k - 1) && dfs(k, r - 1);
    }
};

```
##### Offer 47. 二叉树中和为某一值的路径
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

    vector<vector<int>> ans;

    vector<vector<int>> findPath(TreeNode* root, int sum) {
        vector<int> path;
        dfs(root, sum, path);
        return ans;
    }

    void dfs(TreeNode *root, int sum, vector<int> &path)
    {
        if (!root) return;
        path.push_back(root->val);
        sum -= root->val;

        if (!root->left && !root->right && !sum) ans.push_back(path);
        dfs(root->left, sum, path);
        dfs(root->right, sum, path);

        path.pop_back();
        sum += root->val;
    }
};
```


##### Offer 49. 二叉搜索树与双向链表
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
    TreeNode* convert(TreeNode* root) {
        if (!root) return NULL;
        auto sides = dfs(root);
        return sides.first;
    }

    pair<TreeNode*, TreeNode*> dfs(TreeNode* root)
    {
        if (!root->left && !root->right) return {root, root};
        if (root->left && root->right)
        {
            auto lside = dfs(root->left), rside = dfs(root->right);
            lside.second->right = root, root->left = lside.second;
            root->right = rside.first, rside.first->left = root;
            return {lside.first, rside.second};
        }
        if (root->left)
        {
            auto lside = dfs(root->left);
            lside.second->right = root, root->left = lside.second;
            return {lside.first, root};
        }
        if (root->right)
        {
            auto rside = dfs(root->right);
            rside.first->left = root, root->right = rside.first;
            return {root, rside.second};
        }
    }
};
```
##### Offer 50. 序列化二叉树
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

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res;
        dfs_s(root, res);
        return res;
    }

    void dfs_s(TreeNode *root, string &res)
    {
        if (!root) {
            res += "null ";
            return;
        }
        res += to_string(root->val) + ' ';
        dfs_s(root->left, res);
        dfs_s(root->right, res);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        int u = 0;
        return dfs_d(data, u);
    }

    TreeNode* dfs_d(string data, int &u)
    {
        if (u == data.size()) return NULL;
        int k = u;
        while (data[k] != ' ') k ++ ;
        if (data[u] == 'n') {
            u = k + 1;
            return NULL;
        }
        int val = 0, sign = 1;
        if (u < k && data[u] == '-') sign = -1, u ++ ;
        for (int i = u; i < k; i ++ ) val = val * 10 + data[i] - '0';
        val *= sign;
        u = k + 1;
        auto root = new TreeNode(val);
        root->left = dfs_d(data, u);
        root->right = dfs_d(data, u);
        return root;
    }
};
```
##### Offer 51. 数字排列
```C++
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> permutation(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        path.resize(nums.size());
        dfs(nums, 0, 0, 0);
        return ans;
    }

    void dfs(vector<int> &nums, int u, int start, int state)
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return;
        }

        if (!u || nums[u] != nums[u - 1]) start = 0;

        for (int i = start; i < nums.size(); i ++ )
            if (!(state >> i & 1))
            {
                path[i] = nums[u];
                dfs(nums, u + 1, i + 1, state + (1 << i));
            }
    }
};
```
##### Offer 52. 数组中出现次数超过一半的数字
```C++
class Solution {
public:
    int moreThanHalfNum_Solution(vector<int>& nums) {
        int cnt = 0, val = -1;
        for (auto x : nums)
            if (x == val)
                cnt ++ ;
            else
            {
                if (cnt) cnt -- ;
                else
                {
                    cnt = 1;
                    val = x;
                }
            }
        return val;
    }
};
```
##### Offer 53. 最小的k个数
```C++
class Solution {
public:
    vector<int> getLeastNumbers_Solution(vector<int> input, int k) {
        priority_queue<int> heap;
        for (auto x : input)
        {
            if (heap.size() < k || heap.top() > x) heap.push(x);
            if (heap.size() > k) heap.pop();
        }
        vector<int> res;
        while (heap.size()) res.push_back(heap.top()), heap.pop();
        reverse(res.begin(), res.end());
        return res;
    }
};
```
##### Offer 54. 数据流中的中位数
```C++
class Solution {
public:

    priority_queue<int> max_heap;
    priority_queue<int, vector<int>, greater<int>> min_heap;

    void insert(int num){
        max_heap.push(num);
        while (min_heap.size() && min_heap.top() < max_heap.top())
        {
            auto minv = min_heap.top(), maxv = max_heap.top();
            min_heap.pop(), max_heap.pop();
            max_heap.push(minv), min_heap.push(maxv);
        }
        if (max_heap.size() > min_heap.size() + 1)
        {
            min_heap.push(max_heap.top());
            max_heap.pop();
        }
    }

    double getMedian(){
        if (max_heap.size() + min_heap.size() & 1) return max_heap.top();
        return (max_heap.top() + min_heap.top()) / 2.0;
    }
};
```
##### Offer 55. 连续子数组的最大和
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = INT_MIN, s = 0;
        for (auto x : nums)
        {
            if (s < 0) s = 0;
            s += x;
            res = max(res, s);
        }
        return res;
    }
};
```
##### Offer 56. 从1到n整数中1出现的次数
```C++
class Solution {
public:
    int numberOf1Between1AndN_Solution(int n) {
        if (!n) return 0;
        vector<int> number;
        while (n) number.push_back(n % 10), n /= 10;
        long long res = 0;
        for (int i = number.size() - 1; i >= 0; i -- )
        {
            int left = 0, right = 0, t = 1;
            for (int j = number.size() - 1; j > i; j -- ) left = left * 10 + number[j];
            for (int j = i - 1; j >= 0; j -- ) right = right * 10 + number[j], t *= 10;
            res += left * t;
            if (number[i] == 1) res += right + 1;
            else if (number[i] > 1) res += t;
        }
        return res;
    }
};
```
##### Offer 57. 数字序列中某一位的数字
```C++
class Solution {
public:
    int digitAtIndex(int n) {
        long long i = 1, num = 9, base = 1;
        while (n > i * num) {
            n -= i * num;
            i ++;
            num *= 10;
            base *= 10;
        }

        int number = base + (n + i - 1) / i - 1;
        int r = n % i ? n % i : i;
        for (int j = 0; j < i - r; j ++ ) number /= 10;
        return number % 10;
    }
};
```
##### Offer 58. 把数组排成最小的数
```C++
class Solution {
public:

    static bool cmp(string a, string b)
    {
        return a + b < b + a;
    }

    string printMinNumber(vector<int>& nums) {
        vector<string> num_strs;
        for (auto num : nums) num_strs.push_back(to_string(num));
        sort(num_strs.begin(), num_strs.end(), cmp);
        string res;
        for (auto num : num_strs) res += num;
        return res;
    }
};
```
##### Offer 59. 把数字翻译成字符串
```C++
class Solution {
public:
    int getTranslationCount(string s) {
        int n = s.size();
        vector<int> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = f[i - 1];
            int t = (i >= 2) * (s[i - 2] - '0') * 10 + s[i - 1] - '0';
            if (t >= 10 && t <= 25) f[i] += f[i - 2];
        }
        return f[n];
    }
};
```
##### Offer 60. 礼物的最大价值
```C++
class Solution {
public:
    int getMaxValue(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<int>> f(n + 1, vector<int>(m + 1, 0));

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                f[i][j] = max(f[i - 1][j], f[i][j - 1]) + grid[i - 1][j - 1];

        return f[n][m];
    }
};
```
##### Offer 61. 最长不含重复字符的子字符串
```C++
class Solution {
public:
    int longestSubstringWithoutDuplication(string s) {
        unordered_map<char,int> hash;
        int res = 0;
        for (int i = 0, j = 0; j < s.size(); j ++ )
        {
            if ( ++ hash[s[j]] > 1)
            {
                while (i < j)
                {
                    hash[s[i]]--;
                    i ++ ;
                    if (hash[s[j]] == 1) break;
                }
            }
            res = max(res, j - i + 1);
        }
        return res;
    }
};
```
##### Offer 62. 丑数
```C++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> q;
        q.push_back(1);
        int i = 0, j = 0, k = 0;
        while (--n)
        {
            int t = min(q[i] * 2, min(q[j] * 3, q[k] * 5));
            q.push_back(t);
            if (q[i] * 2 == t) i ++ ;
            if (q[j] * 3 == t) j ++ ;
            if (q[k] * 5 == t) k ++ ;
        }
        return q.back();
    }
};
```
##### Offer 63. 字符串中第一个只出现一次的字符
```C++
class Solution {
public:
    char firstNotRepeatingChar(string s) {
        unordered_map<char, int> hash;
        for (auto c : s) hash[c] ++ ;
        char res = '#';
        for (auto c : s)
            if (hash[c] == 1)
            {
                res = c;
                break;
            }
        return res;
    }
};
```
##### Offer 64. 字符流中第一个只出现一次的字符
```C++
class Solution{
public:

    unordered_map<char, int> count;
    queue<char> q;

    //Insert one char from stringstream
    void insert(char ch){
        q.push(ch);
        if (++ count[ch] > 1)
        {
            while (q.size() && count[q.front()] > 1)
            {
                q.pop();
            }
        }
    }
    //return the first appearence once char in current stringstream
    char firstAppearingOnce(){
        if (q.empty()) return '#';
        return q.front();
    }
};
```
##### Offer 65. 数组中的逆序对
```C++
class Solution {
public:

    int merge(vector<int>& nums, int l, int r) {
        if (l >= r) return 0;

        int mid = l + r >> 1;
        int res = merge(nums, l, mid) + merge(nums, mid + 1, r);

        vector<int> temp;
        int i = l, j = mid + 1;
        while (i <= mid && j <= r)
            if (nums[i] <= nums[j]) temp.push_back(nums[i ++ ]);
            else {
                temp.push_back(nums[j ++ ]);
                res += mid - i + 1;
            }
        while (i <= mid) temp.push_back(nums[i ++ ]);
        while (j <= r) temp.push_back(nums[j ++ ]);

        int k = l;
        for (auto x : temp) nums[k ++ ] = x;

        return res;
    }

    int inversePairs(vector<int>& nums) {
        return merge(nums, 0, nums.size() - 1);
    }
};
```
##### Offer 66. 两个链表的第一个公共结点
```C++
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
    ListNode *findFirstCommonNode(ListNode *headA, ListNode *headB) {
        auto p1 = headA, p2 = headB;
        while (p1 != p2)
        {
            if (p1) p1 = p1->next;
            else p1 = headB;
            if (p2) p2 = p2->next;
            else p2 = headA;
        }
        return p1;
    }
};
```
##### Offer 67. 数字在排序数组中出现的次数
```C++
class Solution {
public:
    int getNumberOfK(vector<int>& nums , int k) {
        if (nums.empty()) return 0;
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= k) r = mid;
            else l = mid + 1;
        }
        if (nums[r] != k) return 0;
        int left = l;
        l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] <= k) l = mid;
            else r = mid - 1;
        }
        return r - left + 1;
    }
};
```
##### Offer 68. 0到n-1中缺失的数字
```C++
class Solution {
public:
    int getMissingNumber(vector<int>& nums) {
        int s = nums.size() * (nums.size() + 1) / 2;
        for (auto x : nums) s -= x;
        return s;
    }
};
```
##### Offer 69. 数组中数值和下标相等的元素
```C++
class Solution {
public:
    int getNumberSameAsIndex(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] - mid >= 0) r = mid;
            else l = mid + 1;
        }
        if (nums[r] != r) r = -1;
        return r;
    }
};
```
##### Offer 70. 二叉搜索树的第k个结点
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

    TreeNode *ans;

    TreeNode* kthNode(TreeNode* root, int k) {
        dfs(root, k);
        return ans;
    }

    void dfs(TreeNode *root, int &k)
    {
        if (!k || !root) return;
        dfs(root->left, k);
        --k;
        if (!k) ans = root;
        else dfs(root->right, k);
    }
};
```
##### Offer 71. 二叉树的深度
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
    int treeDepth(TreeNode* root) {
        if (!root) return 0;
        return max(treeDepth(root->left), treeDepth(root->right)) + 1;
    }
};
```
##### Offer 72. 平衡二叉树
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

    bool ans = true;

    bool isBalanced(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode *root)
    {
        if (!root) return 0;
        int left = dfs(root->left), right = dfs(root->right);
        if (abs(left - right) > 1) ans = false;
        return max(left, right) + 1;
    }
};
```
##### Offer 73. 数组中只出现一次的两个数字
```C++
class Solution {
public:
    vector<int> findNumsAppearOnce(vector<int>& nums) {
        int sum = 0;
        for (auto x : nums) sum ^= x;
        int k = 0;
        while (!(sum >> k & 1)) k ++ ;
        int first = 0;
        for (auto x : nums)
            if (x >> k & 1)
                first ^= x;
        return vector<int>({first, sum ^ first});
    }
};

```
##### Offer 74. 数组中唯一只出现一次的数字
```C++
class Solution {
public:
    int findNumberAppearingOnce(vector<int>& nums) {
        int ones = 0, twos = 0;
        for (auto x : nums)
        {
            ones = (ones ^ x) & ~twos;
            twos = (twos ^ x) & ~ones;
        }
        return ones;
    }
};
```
##### Offer 75. 和为S的两个数字
```C++
class Solution {
public:
    vector<int> findNumbersWithSum(vector<int>& nums, int target) {
        unordered_set<int> hash;
        for (auto x : nums)
        {
            if (hash.count(target - x)) return vector<int>{target - x, x};
            hash.insert(x);
        }
        return vector<int>();
    }
};
```
##### Offer 76. 和为S的连续正数序列
```C++
class Solution {
public:
    vector<vector<int> > findContinuousSequence(int sum) {
        vector<vector<int>> res;
        for (int i = 1, j = 1, s = 1; i <= sum; i ++ )
        {
            while (s < sum) j ++, s += j;
            if (s == sum && j > i)
            {
                vector<int> line;
                for (int k = i; k <= j; k ++ ) line.push_back(k);
                res.push_back(line);
            }
            s -= i;
        }
        return res;
    }
};
```
##### Offer 77. 翻转单词顺序
```C++
class Solution {
public:
    string reverseWords(string s) {
        for (int i = 0; i < s.size(); i ++ )
        {
            int j = i;
            while (j < s.size() && s[j] != ' ') j ++ ;
            reverse(s.begin() + i, s.begin() + j);
            i = j;
        }
        reverse(s.begin(), s.end());
        return s;
    }
}
```
##### Offer 78. 左旋转字符串
```C++
class Solution {
public:
    string leftRotateString(string str, int n) {
        reverse(str.begin(), str.end());
        reverse(str.begin(), str.begin() + str.size() - n);
        reverse(str.begin() + str.size() - n, str.end());
        return str;
    }
};
```
##### Offer 79. 滑动窗口的最大值
```C++
class Solution {
public:
    vector<int> maxInWindows(vector<int>& nums, int k) {
        vector<int> res;
        deque<int> q;

        for (int i = 0; i < nums.size(); i ++ ) {
            while (q.size() && q.front() <= i - k) q.pop_front();
            while (q.size() && nums[q.back()] <= nums[i]) q.pop_back();
            q.push_back(i);
            if (i >= k - 1) res.push_back(nums[q.front()]);
        }

        return res;
    }
};
```
##### Offer 80. 骰子的点数
```C++
class Solution {
public:
    vector<int> numberOfDice(int n) {
        vector<vector<int>> f(n + 1, vector<int>(6 * n + 1, 0));
        f[0][0] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= i * 6; j ++ )
                for (int k = 1; k <= 6; k ++ )
                    if (j >= k)
                        f[i][j] += f[i - 1][j - k];
        return vector<int>(f[n].begin() + n, f[n].end());
    }
};
```
##### Offer 81. 扑克牌的顺子
```C++
class Solution {
public:
    bool isContinuous( vector<int> nums) {
        sort(nums.begin(), nums.end());
        for (int i = 1; i < nums.size(); i ++ )
            if (nums[i] && nums[i] == nums[i - 1])
                return false;
        for (auto x : nums)
            if (x)
            {
                return nums.back() - x <= 4;
            }
    }
};
```
##### Offer 82. 圆圈中最后剩下的数字
```C++
class Solution {
public:
    int lastRemaining(int n, int m){
        if (n == 1) return 0;
        return (lastRemaining(n - 1, m) + m) % n;
    }
};
```
##### Offer 83. 股票的最大利润
```C++
class Solution {
public:
    int maxDiff(vector<int>& nums) {
        int res = 0;
        for (int i = 0, j = INT_MAX; i < nums.size(); i ++ ) {
            if (j < nums[i])
                res = max(res, nums[i] - j);
            j = min(j, nums[i]);
        }
        return res;
    }
};
```
##### Offer 84. 求1+2+…+n
```C++
class Solution {
public:
    int getSum(int n) {
        int res = n;
        n > 0 && (res += getSum(n - 1));
        return res;
    }
};
```
##### Offer 85. 不用加减乘除做加法
```C++
class Solution {
public:
    int add(int num1, int num2){
        while (num2) {
            int sum = num1 ^ num2;
            int carray = (num1 & num2) << 1;
            num1 = sum;
            num2 = carray;
        }
        return num1;
    }
};
```
##### Offer 86. 构建乘积数组
```C++
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        if (A.empty()) return vector<int>();
        int n = A.size();
        vector<int> res(n);
        res[0] = 1;
        for (int i = 1, p = A[0]; i < n; i ++ ) {
            res[i] = p;
            p *= A[i];
        }
        for (int i = n - 2, p = A[n - 1]; i >= 0; i -- ) {
            res[i] *= p;
            p *= A[i];
        }
        return res;
    }
};
```
##### Offer 87. 把字符串转换成整数
```C++
class Solution {
public:
    int strToInt(string str) {
        int k = 0;
        while (k < str.size() && str[k] == ' ') k ++ ;
        long long res = 0;

        int minus = 1;
        if (k < str.size())
        {
            if (str[k] == '-') minus = -1, k ++ ;
            else if (str[k] == '+') k ++ ;
        }
        while (k < str.size() && str[k] >= '0' && str[k] <= '9')
        {
            res = res * 10 + str[k] - '0';
            if (res > 1e11) break;
            k ++ ;
        }

        res *= minus;
        if (res > INT_MAX) res = INT_MAX;
        if (res < INT_MIN) res = INT_MIN;

        return res;
    }
};

```
##### Offer 88. 树中两个结点的最低公共祖先
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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) return NULL;
        if (root == p || root == q) return root;
        auto left = lowestCommonAncestor(root->left, p, q);
        auto right = lowestCommonAncestor(root->right, p, q);
        if (left && right) return root;
        if (left) return left;
        return right;
    }
};
```


