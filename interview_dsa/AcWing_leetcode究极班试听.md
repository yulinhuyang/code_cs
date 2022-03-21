LeetCode究极班：https://www.acwing.com/activity/content/activity_person/content/29799/1/

### 究极班——试听课第1弹（Week1. 第1-10题）

#####  1. 两数之和
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> heap;
        for (int i = 0; i < nums.size(); i ++ ) {
            int r = target - nums[i];
            if (heap.count(r)) return {heap[r], i};
            heap[nums[i]] = i;
        }

        return {};
    }
};
```
#####  2. 两数相加
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        auto dummy = new ListNode(-1), cur = dummy;
        int t = 0;
        while (l1 || l2 || t) {
            if (l1) t += l1->val, l1 = l1->next;
            if (l2) t += l2->val, l2 = l2->next;
            cur = cur->next = new ListNode(t % 10);
            t /= 10;
        }
        return dummy->next;
    }
};
```
#####  3. 无重复字符的最长子串
```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> heap;
        int res = 0;
        for (int i = 0, j = 0; i < s.size(); i ++ ) {
            heap[s[i]] ++ ;
            while (heap[s[i]] > 1) heap[s[j ++ ]] -- ;
            res = max(res, i - j + 1);
        }
        return res;
    }
};
```
#####  4. 寻找两个正序数组的中位数
```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int tot = nums1.size() + nums2.size();
        if (tot % 2 == 0) {
            int left = find(nums1, 0, nums2, 0, tot / 2);
            int right = find(nums1, 0, nums2, 0, tot / 2 + 1);
            return (left + right) / 2.0;
        } else {
            return find(nums1, 0, nums2, 0, tot / 2 + 1);
        }
    }

    int find(vector<int>& nums1, int i, vector<int>& nums2, int j, int k) {
        if (nums1.size() - i > nums2.size() - j) return find(nums2, j, nums1, i, k);
        if (k == 1) {
            if (nums1.size() == i) return nums2[j];
            else return min(nums1[i], nums2[j]);
        }
        if (nums1.size() == i) return nums2[j + k - 1];
        int si = min((int)nums1.size(), i + k / 2), sj = j + k - k / 2;
        if (nums1[si - 1] > nums2[sj - 1])
            return find(nums1, i, nums2, sj, k - (sj - j));
        else
            return find(nums1, si, nums2, j, k - (si - i));
    }
};
```
#####  5. 最长回文子串
```C++
class Solution {
public:
    string longestPalindrome(string s) {
        string res;
        for (int i = 0; i < s.size(); i ++ ) {
            int l = i - 1, r = i + 1;
            while (l >= 0 && r < s.size() && s[l] == s[r]) l --, r ++ ;
            if (res.size() < r - l - 1) res = s.substr(l + 1, r - l - 1);

            l = i, r = i + 1;
            while (l >= 0 && r < s.size() && s[l] == s[r]) l --, r ++ ;
            if (res.size() < r - l - 1) res = s.substr(l + 1, r - l - 1);
        }

        return res;
    }
};
```
#####  6. Z 字形变换
```C++
class Solution {
public:
    string convert(string s, int n) {
        string res;
        if (n == 1) return s;
        for (int i = 0; i < n; i ++ ) {
            if (i == 0 || i == n - 1) {
                for (int j = i; j < s.size(); j += 2 * n - 2)
                    res += s[j];
            } else {
                for (int j = i, k = 2 * n - 2 - i; j < s.size() || k < s.size(); j += 2 * n - 2, k += 2 * n - 2) {
                    if (j < s.size()) res += s[j];
                    if (k < s.size()) res += s[k];
                }
            }
        }

        return res;
    }
};
```
#####  7. 整数反转
```C++
class Solution {
public:
    int reverse(int x) {
        int r = 0;
        while (x) {
            if (r > 0 && r > (INT_MAX - x % 10) / 10) return 0;
            if (r < 0 && r < (INT_MIN - x % 10) / 10) return 0;
            r = r * 10 + x % 10;
            x /= 10;
        }
        return r;
    }
};
```
#####  8. 字符串转换整数 (atoi)
```C++
class Solution {
public:
    int myAtoi(string str) {
        int k = 0;
        while (k < str.size() && str[k] == ' ') k ++ ;
        if (k == str.size()) return 0;

        int minus = 1;
        if (str[k] == '-') minus = -1, k ++ ;
        else if (str[k] == '+') k ++ ;

        int res = 0;
        while (k < str.size() && str[k] >= '0' && str[k] <= '9') {
            int x = str[k] - '0';
            if (minus > 0 && res > (INT_MAX - x) / 10) return INT_MAX;
            if (minus < 0 && -res < (INT_MIN + x) / 10) return INT_MIN;
            if (-res * 10 - x == INT_MIN) return INT_MIN;
            res = res * 10 + x;
            k ++ ;
            if (res > INT_MAX) break;
        }

        res *= minus;

        return res;
    }
};
```
#####  9. 回文数
```C++
转化成字符串
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return 0;
        string s = to_string(x);
        return s == string(s.rbegin(), s.rend());
    }
};
数值方法
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return 0;
        int y = x;
        long long res = 0;
        while (x) {
            res = res * 10 + x % 10;
            x /= 10;
        }
        return res == y;
    }
};
```
#####  10. 正则表达式匹配
```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size(), m = p.size();
        s = ' ' + s, p = ' ' + p;
        vector<vector<bool>> f(n + 1, vector<bool>(m + 1));
        f[0][0] = true;
        for (int i = 0; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                if (j + 1 <= m && p[j + 1] == '*') continue;
                if (i && p[j] != '*') {
                    f[i][j] = f[i - 1][j - 1] && (s[i] == p[j] || p[j] == '.');
                } else if (p[j] == '*') {
                    f[i][j] = f[i][j - 2] || i && f[i - 1][j] && (s[i] == p[j - 1] || p[j - 1] == '.');
                }
            }

        return f[n][m];
    }
};
```

### 究极班——试听课第2弹（Week3. 第31-40题）

#####  31. 下一个排列
```C++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int k = nums.size() - 1;
        while (k > 0 && nums[k - 1] >= nums[k]) k -- ;
        if (k <= 0) {
            reverse(nums.begin(), nums.end());
        } else {
            int t = k;
            while (t < nums.size() && nums[t] > nums[k - 1]) t ++ ;
            swap(nums[t - 1], nums[k - 1]);
            reverse(nums.begin() + k, nums.end());
        }
    }
};
```
#####  32. 最长有效括号
```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> stk;

        int res = 0;
        for (int i = 0, start = -1; i < s.size(); i ++ ) {
            if (s[i] == '(') stk.push(i);
            else {
                if (stk.size()) {
                    stk.pop();
                    if (stk.size()) {
                        res = max(res, i - stk.top());
                    } else {
                        res = max(res, i - start);
                    }
                } else {
                    start = i;
                }
            }
        }

        return res;
    }
};
```
#####  33. 搜索旋转排序数组
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty()) return -1;
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] >= nums[0]) l = mid;
            else r = mid - 1;
        }

        if (target >= nums[0]) l = 0;
        else l = r + 1, r = nums.size() - 1;

        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if (nums[r] == target) return r;
        return -1;
    }
};
```
#####  34. 在排序数组中查找元素的第一个和最后一个位置
```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if (nums.empty()) return {-1, -1};

        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if (nums[r] != target) return {-1, -1};

        int L = r;
        l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] <= target) l = mid;
            else r = mid - 1;
        }

        return {L, r};
    }
};
```
#####  35. 搜索插入位置
```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l = 0, r = nums.size();

        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        return l;
    }
};
```
#####  36. 有效的数独
```C++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        bool st[9];

        // 判断行
        for (int i = 0; i < 9; i ++ ) {
            memset(st, 0, sizeof st);
            for (int j = 0; j < 9; j ++ ) {
                if (board[i][j] != '.') {
                    int t = board[i][j] - '1';
                    if (st[t]) return false;
                    st[t] = true;
                }
            }
        }

        // 判断列
        for (int i = 0; i < 9; i ++ ) {
            memset(st, 0, sizeof st);
            for (int j = 0; j < 9; j ++ ) {
                if (board[j][i] != '.') {
                    int t = board[j][i] - '1';
                    if (st[t]) return false;
                    st[t] = true;
                }
            }
        }

        // 判断小方格
        for (int i = 0; i < 9; i += 3)
            for (int j = 0; j < 9; j += 3) {
                memset(st, 0, sizeof st);
                for (int x = 0; x < 3; x ++ )
                    for (int y = 0; y < 3; y ++ ) {
                        if (board[i + x][j + y] != '.') {
                            int t = board[i + x][j + y] - '1';
                            if (st[t]) return false;
                            st[t] = true;
                        }
                    }
            }

        return true;
    }
};
```
#####  37. 解数独
```C++
class Solution {
public:

    bool row[9][9], col[9][9], cell[3][3][9];

    void solveSudoku(vector<vector<char>>& board) {
        memset(row, 0, sizeof row);
        memset(col, 0, sizeof col);
        memset(cell, 0, sizeof cell);

        for (int i = 0; i < 9; i ++ )
            for (int j = 0; j < 9; j ++ )
                if (board[i][j] != '.') {
                    int t = board[i][j] - '1';
                    row[i][t] = col[j][t] = cell[i / 3][j / 3][t] = true;
                }

        dfs(board, 0, 0);
    }

    bool dfs(vector<vector<char>>& board, int x, int y) {
        if (y == 9) x ++, y = 0;
        if (x == 9) return true;

        if (board[x][y] != '.') return dfs(board, x, y + 1);
        for (int i = 0; i < 9; i ++ )
            if (!row[x][i] && !col[y][i] && !cell[x / 3][y / 3][i]) {
                board[x][y] = '1' + i;
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = true;
                if (dfs(board, x, y + 1)) return true;
                board[x][y] = '.';
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = false;
            }

        return false;
    }
};
```

#####  38. 外观数列
```C++
class Solution {
public:
    string countAndSay(int n) {
        string s = "1";
        for (int i = 0; i < n - 1; i ++ ) {
            string t;
            for (int j = 0; j < s.size();) {
                int k = j + 1;
                while (k < s.size() && s[k] == s[j]) k ++ ;
                t += to_string(k - j) + s[j];
                j = k;
            }
            s = t;
        }

        return s;
    }
};
```
#####  39. 组合总和
```C++
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combinationSum(vector<int>& c, int target) {
        dfs(c, 0, target);
        return ans;
    }

    void dfs(vector<int>& c, int u, int target) {
        if (target == 0) {
            ans.push_back(path);
            return;
        }
        if (u == c.size()) return;

        for (int i = 0; c[u] * i <= target; i ++ ) {
            dfs(c, u + 1, target - c[u] * i);
            path.push_back(c[u]);
        }

        for (int i = 0; c[u] * i <= target; i ++ ) {
            path.pop_back();
        }
    }
};
```
#####  40. 组合总和 II
```C++
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combinationSum2(vector<int>& c, int target) {
        sort(c.begin(), c.end());
        dfs(c, 0, target);

        return ans;
    }

    void dfs(vector<int>& c, int u, int target) {
        if (target == 0) {
            ans.push_back(path);
            return;
        }
        if (u == c.size()) return;

        int k = u + 1;
        while (k < c.size() && c[k] == c[u]) k ++ ;
        int cnt = k - u;

        for (int i = 0; c[u] * i <= target && i <= cnt; i ++ ) {
            dfs(c, k, target - c[u] * i);
            path.push_back(c[u]);
        }

        for (int i = 0; c[u] * i <= target && i <= cnt; i ++ ) {
            path.pop_back();
        }
    }
};

```

### 究极班——试听课第3弹（Week6. 第101-110题）

#####  101. 对称二叉树
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
        if (!root) return true;
        return dfs(root->left, root->right);
    }

    bool dfs(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (!p || !q || p->val != q->val) return false;
        return dfs(p->left, q->right) && dfs(p->right, q->left);
    }
};
```
#####  102. 二叉树的层序遍历
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if (root) q.push(root);

        while (q.size()) {
            vector<int> level;
            int len = q.size();

            while (len -- ) {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }

            res.push_back(level);
        }

        return res;
    }
};
```
#####  103. 二叉树的锯齿形层次遍历
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if (root) q.push(root);

        int cnt = 0;
        while (q.size()) {
            vector<int> level;
            int len = q.size();
            while (len -- ) {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }

            if ( ++ cnt % 2 == 0) reverse(level.begin(), level.end());
            res.push_back(level);
        }

        return res;
    }
};
```
#####  104. 二叉树的最大深度
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
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```
#####  105. 从前序与中序遍历序列构造二叉树
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
    unordered_map<int, int> pos;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for (int i = 0; i < inorder.size(); i ++ ) pos[inorder[i]] = i;
        return build(preorder, inorder, 0, preorder.size() - 1, 0, inorder.size() - 1);
    }

    TreeNode* build(vector<int>& preorder, vector<int>& inorder, int pl, int pr, int il, int ir) {
        if (pl > pr) return NULL;
        auto root = new TreeNode(preorder[pl]);
        int k = pos[root->val];
        root->left = build(preorder, inorder, pl + 1, pl + 1 + k - 1 - il, il, k - 1);
        root->right = build(preorder, inorder, pl + 1 + k - 1 - il + 1, pr, k + 1, ir);
        return root;
    }
};
```
#####  106. 从中序与后序遍历序列构造二叉树
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
    unordered_map<int, int> pos;

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        for (int i = 0; i < inorder.size(); i ++ ) pos[inorder[i]] = i;
        return build(inorder, postorder, 0, inorder.size() - 1, 0, postorder.size() - 1);
    }

    TreeNode* build(vector<int>& inorder, vector<int>& postorder, int il, int ir, int pl, int pr) {
        if (il > ir) return NULL;
        auto root = new TreeNode(postorder[pr]);
        int k = pos[root->val];
        root->left = build(inorder, postorder, il, k - 1, pl, pl + k - 1 - il);
        root->right = build(inorder, postorder, k + 1, ir, pl + k - 1 - il + 1, pr - 1);
        return root;
    }
};
```
#####  107. 二叉树的层次遍历 II
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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if (root) q.push(root);
        while (q.size()) {
            vector<int> level;
            int len = q.size();
            while (len -- ) {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }
            res.push_back(level);
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```
#####  108. 将有序数组转换为二叉搜索树
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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return build(nums, 0, nums.size() - 1);
    }

    TreeNode* build(vector<int>& nums, int l, int r) {
        if (l > r) return NULL;
        int mid = l + r >> 1;
        auto root = new TreeNode(nums[mid]);
        root->left = build(nums, l, mid - 1);
        root->right = build(nums, mid + 1, r);
        return root;
    }
};

```
#####  109. 有序链表转换二叉搜索树
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
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
    TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return NULL;
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;
        if (n == 1) return new TreeNode(head->val);
        auto cur = head;
        for (int i = 0; i < n / 2 - 1; i ++ ) cur = cur->next;
        auto root = new TreeNode(cur->next->val);
        root->right = sortedListToBST(cur->next->next);
        cur->next = NULL;
        root->left = sortedListToBST(head);
        return root;
    }
};
```
#####  110. 平衡二叉树
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
    bool ans;

    bool isBalanced(TreeNode* root) {
        ans = true;
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root) {
        if (!root) return 0;
        int lh = dfs(root->left), rh = dfs(root->right);
        if (abs(lh - rh) > 1) ans = false;
        return max(lh, rh) + 1;
    }
};
```

### 究极班——试听课第4弹（Week9. 第151-166题）

#####  151. 翻转字符串里的单词
```C++
class Solution {
public:
    string reverseWords(string s) {
        int k = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == ' ') continue;
            int j = i, t = k;
            while (j < s.size() && s[j] != ' ') s[t ++ ] = s[j ++ ];
            reverse(s.begin() + k, s.begin() + t);
            s[t ++ ] = ' ';
            k = t, i = j;
        }
        if (k) k -- ;
        s.erase(s.begin() + k, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};

```
#####  152. 乘积最大子数组
```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int res = nums[0];
        int f = nums[0], g = nums[0];
        for (int i = 1; i < nums.size(); i ++ ) {
            int a = nums[i], fa = f * a, ga = g * a;
            f = max(a, max(fa, ga));
            g = min(a, min(fa, ga));
            res = max(res, f);
        }
        return res;
    }
};
```
#####  153. 寻找旋转排序数组中的最小值
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        if (nums[r] >= nums[l]) return nums[0];
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] < nums[0]) r = mid;
            else l = mid + 1;
        }
        return nums[r];
    }
};

```
#####  154. 寻找旋转排序数组中的最小值 II
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r && nums[r] == nums[0]) r -- ;
        if (nums[l] <= nums[r]) return nums[0];
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] < nums[0]) r = mid;
            else l = mid + 1;
        }
        return nums[r];
    }
};

```
#####  155. 最小栈
```C++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> stk, f;
    MinStack() {

    }

    void push(int x) {
        stk.push(x);
        if (f.empty() || f.top() >= x) f.push(x);
    }

    void pop() {
        if (stk.top() <= f.top()) f.pop();
        stk.pop();
    }

    int top() {
        return stk.top();
    }

    int getMin() {
        return f.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */

```
#####  160. 相交链表
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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        auto p = headA, q = headB;
        while (p != q) {
            p = p ? p->next : headB;
            q = q ? q->next : headA;
        }
        return p;
    }
};
```

#####  162. 寻找峰值
```C++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] > nums[mid + 1]) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```

#####  164. 最大间距
```C++
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        struct Range {
            int min, max;
            bool used;
            Range() : min(INT_MAX), max(INT_MIN), used(false){}
        };
        int n = nums.size();
        int Min = INT_MAX, Max = INT_MIN;
        for (auto x: nums) {
            Min = min(Min, x);
            Max = max(Max, x);
        }
        if (n < 2 || Min == Max) return 0;
        vector<Range> r(n - 1);
        int len = (Max - Min + n - 2) / (n - 1);
        for (auto x: nums) {
            if (x == Min) continue;
            int k = (x - Min - 1) / len;
            r[k].used = true;
            r[k].min = min(r[k].min, x);
            r[k].max = max(r[k].max, x);
        }
        int res = 0;
        for (int i = 0, last = Min; i < n - 1; i ++ )
            if (r[i].used) {
                res = max(res, r[i].min - last);
                last = r[i].max;
            }
        return res;
    }
};

```

#####  165. 比较版本号
```C++
class Solution {
public:
    int compareVersion(string v1, string v2) {
        for (int i = 0, j = 0; i < v1.size() || j < v2.size();) {
            int a = i, b = j;
            while (a < v1.size() && v1[a] != '.') a ++ ;
            while (b < v2.size() && v2[b] != '.') b ++ ;
            int x = a == i ? 0 : stoi(v1.substr(i, a - i));
            int y = b == j ? 0 : stoi(v2.substr(j, b - j));
            if (x > y) return 1;
            if (x < y) return -1;
            i = a + 1, j = b + 1;
        }
        return 0;
    }
};

```
#####  166. 分数到小数
```C++
class Solution {
public:
    string fractionToDecimal(int numerator, int denominator) {
        typedef long long LL;
        LL x = numerator, y = denominator;
        if (x % y == 0) return to_string(x / y);
        string res;
        if ((x < 0) ^ (y < 0)) res += '-';
        x = abs(x), y = abs(y);
        res += to_string(x / y) + '.', x %= y;
        unordered_map<LL, int> hash;
        while (x) {
            hash[x] = res.size();
            x *= 10;
            res += to_string(x / y);
            x %= y;
            if (hash.count(x)) {
                res = res.substr(0, hash[x]) + '(' + res.substr(hash[x]) + ')';
                break;
            }
        }
        return res;
    }
};

```
### 究极班——试听课第5弹（Week 13. 第301-315题）
 
#####  301. 删除无效的括号
```C++
class Solution {
public:
    vector<string> ans;

    vector<string> removeInvalidParentheses(string s) {
        int l = 0, r = 0;
        for (auto x: s)
            if (x == '(') l ++ ;
            else if (x == ')') {
                if (l == 0) r ++ ;
                else l -- ;
            }

        dfs(s, 0, "", 0, l, r);
        return ans;
    }

    void dfs(string& s, int u, string path, int cnt, int l, int r) {
        if (u == s.size()) {
            if (!cnt) ans.push_back(path);
            return;
        }

        if (s[u] != '(' && s[u] != ')') dfs(s, u + 1, path + s[u], cnt, l, r);
        else if (s[u] == '(') {
            int k = u;
            while (k < s.size() && s[k] == '(') k ++ ;
            l -= k - u;
            for (int i = k - u; i >= 0; i -- ) {
                if (l >= 0) dfs(s, k, path, cnt, l, r);
                path += '(';
                cnt ++, l ++ ;
            }
        } else if (s[u] == ')') {
            int k = u;
            while (k < s.size() && s[k] == ')') k ++ ;
            r -= k - u;
            for (int i = k - u; i >= 0; i -- ) {
                if (cnt >= 0 && r >= 0) dfs(s, k, path, cnt, l, r);
                path += ')';
                cnt --, r ++ ;
            }
        }
    }
};

```
#####  303. 区域和检索 - 数组不可变
```C++
class NumArray {
public:
    vector<int> s;

    NumArray(vector<int>& nums) {
        s.resize(nums.size() + 1);
        for (int i = 1; i <= nums.size(); i ++ ) s[i] = s[i - 1] + nums[i - 1];
    }

    int sumRange(int i, int j) {
        ++i, ++j;
        return s[j] - s[i - 1];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */

```
#####  304. 二维区域和检索 - 矩阵不可变
```C++
class NumMatrix {
public:
    vector<vector<int>> s;

    NumMatrix(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return;
        s = vector<vector<int>>(matrix.size() + 1, vector<int>(matrix[0].size() + 1));
        for (int i = 1; i <= matrix.size(); i ++ )
            for (int j = 1; j <= matrix[0].size(); j ++ )
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + matrix[i - 1][j - 1];
    }

    int sumRegion(int x1, int y1, int x2, int y2) {
        ++x1, ++y1, ++x2, ++y2;
        return s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1];
    }
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * int param_1 = obj->sumRegion(row1,col1,row2,col2);
 */
```
#####  306. 累加数
```C++
class Solution {
public:

    string add(string x, string y) {
        vector<int> A, B, C;
        for (int i = x.size() - 1; i >= 0; i -- ) A.push_back(x[i] - '0');
        for (int i = y.size() - 1; i >= 0; i -- ) B.push_back(y[i] - '0');
        for (int i = 0, t = 0; i < A.size() || i < B.size() || t; i ++ ) {
            if (i < A.size()) t += A[i];
            if (i < B.size()) t += B[i];
            C.push_back(t % 10);
            t /= 10;
        }
        string z;
        for (int i = C.size() - 1; i >= 0; i -- ) z += to_string(C[i]);
        return z;
    }

    bool isAdditiveNumber(string num) {
        for (int i = 0; i < num.size(); i ++ )
            for (int j = i + 1; j + 1 < num.size(); j ++ ) {
                int a = -1, b = i, c = j;
                while (true) {
                    if (b - a > 1 && num[a + 1] == '0' || c - b > 1 && num[b + 1] == '0') break;  // 有前导0
                    auto x = num.substr(a + 1, b - a), y = num.substr(b + 1, c - b);
                    auto z = add(x, y);
                    if (num.substr(c + 1, z.size()) != z) break;  // 下一个数不匹配
                    a = b, b = c, c += z.size();
                    if (c + 1 == num.size()) return true;
                }
            }

        return false;
    }
};
```
#####  307. 区域和检索 - 数组可修改
```C++
class NumArray {
public:
    int n;
    vector<int> tr, nums;

    int lowbit(int x) {
        return x & -x;
    }

    int query(int x) {
        int res = 0;
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }

    void add(int x, int v) {
        for (int i = x; i <= n; i += lowbit(i)) tr[i] += v;
    }

    NumArray(vector<int>& _nums) {
        nums = _nums;
        n = nums.size();
        tr.resize(n + 1);

        for (int i = 1; i <= n; i ++ ) {
            tr[i] = nums[i - 1];
            for (int j = i - 1; j > i - lowbit(i); j -= lowbit(j))
                tr[i] += tr[j];
        }
    }

    void update(int i, int val) {
        add(i + 1, val - nums[i]);
        nums[i] = val;
    }

    int sumRange(int i, int j) {
        return query(j + 1) - query(i);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * obj->update(i,val);
 * int param_2 = obj->sumRange(i,j);
 */
 
```
#####  309. 最佳买卖股票时机含冷冻期
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty()) return 0;
        int n = prices.size(), INF = 1e8;
        vector<vector<int>> f(n, vector<int>(3, -INF));
        f[0][1] = -prices[0], f[0][0] = 0;
        for (int i = 1; i < n; i ++ ) {
            f[i][0] = max(f[i - 1][0], f[i - 1][2]);
            f[i][1] = max(f[i - 1][1], f[i - 1][0] - prices[i]);
            f[i][2] = f[i - 1][1] + prices[i];
        }
        return max(f[n - 1][0], max(f[n - 1][1], f[n - 1][2]));
    }
};

```
#####  310. 最小高度树
```C++
class Solution {
public:
    vector<vector<int>> g;
    vector<int> d1, d2, p1, up;

    void dfs1(int u, int father) {
        for (int x: g[u]) {
            if (x == father) continue;
            dfs1(x, u);
            int d = d1[x] + 1;
            if (d >= d1[u]) {
                d2[u] = d1[u], d1[u] = d;
                p1[u] = x;
            } else if (d > d2[u]) {
                d2[u] = d;
            }
        }
    }

    void dfs2(int u, int father) {
        for (int x: g[u]) {
            if (x == father) continue;
            if (p1[u] == x) up[x] = max(up[u], d2[u]) + 1;
            else up[x] = max(up[u], d1[u]) + 1;
            dfs2(x, u);
        }
    }

    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        g.resize(n);
        d1 = d2 = p1 = up = vector<int>(n);
        for (auto& e: edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b), g[b].push_back(a);
        }
        dfs1(0, -1);
        dfs2(0, -1);

        int mind = n + 1;
        for (int i = 0; i < n; i ++ ) mind = min(mind, max(up[i], d1[i]));
        vector<int> res;
        for (int i = 0; i < n; i ++ )
            if (max(up[i], d1[i]) == mind)
                res.push_back(i);
        return res;
    }
};
```
#####  312. 戳气球
```C++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        vector<int> a(n + 2, 1);
        for (int i = 1; i <= n; i ++ ) a[i] = nums[i - 1];
        vector<vector<int>> f(n + 2, vector<int>(n + 2));
        for (int len = 3; len <= n + 2; len ++ )
            for (int i = 0; i + len - 1 <= n + 1; i ++ ) {
                int j = i + len - 1;
                for (int k = i + 1; k < j; k ++ )
                    f[i][j] = max(f[i][j], f[i][k] + f[k][j] + a[i] * a[k] * a[j]);
            }

        return f[0][n + 1];
    }
};

```
#####  313. 超级丑数
```C++
class Solution {
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        typedef pair<int, int> PII;
        priority_queue<PII, vector<PII>, greater<PII>> heap;
        for (int x: primes) heap.push({x, 0});
        vector<int> q(n);
        q[0] = 1;
        for (int i = 1; i < n;) {
            auto t = heap.top(); heap.pop();
            if (t.first != q[i - 1]) q[i ++ ] = t.first;
            int idx = t.second, p = t.first / q[idx];
            heap.push({(long long)p * q[idx + 1], idx + 1});
        }
        return q[n - 1];
    }
};
```
#####  315. 计算右侧小于当前元素的个数
```C++
class Solution {
public:
    int n = 20001;
    vector<int> tr;

    int lowbit(int x) {
        return x & -x;
    }

    int query(int x) {
        int res = 0;
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }

    void add(int x, int v) {
        for (int i = x; i <= n; i += lowbit(i)) tr[i] += v;
    }

    vector<int> countSmaller(vector<int>& nums) {
        tr.resize(n + 1);
        vector<int> res(nums.size());
        for (int i = nums.size() - 1; i >= 0; i -- ) {
            int x = nums[i] + 10001;
            res[i] = query(x - 1);
            add(x, 1);
        }
        return res;
    }
};

```

### 究极班——试听课第6弹（Week 17. 第398-407题）

#####  398. 随机数索引
```C++
class Solution {
public:
    unordered_map<int, vector<int>> hash;

    Solution(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i ++ )
            hash[nums[i]].push_back(i);
    }

    int pick(int target) {
        return hash[target][rand() % hash[target].size()];
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(nums);
 * int param_1 = obj->pick(target);
 */
```
#####  399. 除法求值
```C++
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_set<string> vers;
        unordered_map<string, unordered_map<string, double>> d;
        for (int i = 0; i < equations.size(); i ++ ) {
            auto a = equations[i][0], b = equations[i][1];
            auto c = values[i];
            d[a][b] = c, d[b][a] = 1 / c;
            vers.insert(a), vers.insert(b);
        }

        for (auto k: vers)
            for (auto i: vers)
                for (auto j: vers)
                    if (d[i][k] && d[j][k])
                        d[i][j] = d[i][k] * d[k][j];

        vector<double> res;
        for (auto q: queries) {
            auto a = q[0], b = q[1];
            if (d[a][b]) res.push_back(d[a][b]);
            else res.push_back(-1);
        }

        return res;
    }
};

```
#####  400. 第N个数字
```C++
class Solution {
public:
    int findNthDigit(int n) {
        long long k = 1, t = 9, s = 1;
        while (n > k * t) {
            n -= k * t;
            k ++, t *= 10, s *= 10;
        }
        s += (n + k - 1) / k - 1;
        n = n % k ? n % k : k;
        return to_string(s)[n - 1] - '0';
    }
};
```
#####  401. 二进制手表
```C++
class Solution {
public:
    vector<string> readBinaryWatch(int num) {
        vector<string> res;
        char str[10];
        for (int i = 0; i < 1 << 10; i ++ ) {
            int s = 0;
            for (int j = 0; j < 10; j ++ )
                if (i >> j & 1)
                    s ++ ;
            if (s == num) {
                int a = i >> 6, b = i & 63;
                if (a < 12 && b < 60) {
                    sprintf(str, "%d:%02d", a, b);
                    res.push_back(str);
                }
            }
        }
        return res;
    }
};
```
#####  402. 移掉K位数字
```C++
class Solution {
public:
    string removeKdigits(string num, int k) {
        k = min(k, (int)num.size());
        string res;
        for (auto c: num) {
            while (k && res.size() && res.back() > c) {
                k -- ;
                res.pop_back();
            }
            res += c;
        }
        while (k -- ) res.pop_back();
        k = 0;
        while (k < res.size() && res[k] == '0') k ++ ;
        if (k == res.size()) res += '0';
        return res.substr(k);
    }
};
```
#####  403. 青蛙过河
```C++
const int N = 2010;

int f[N][N];

class Solution {
public:
    unordered_map<int, int> hash;
    vector<int> stones;

    int dp(int x, int y) {
        if (f[x][y] != -1) return f[x][y];
        f[x][y] = 0;
        for (int k = max(1, y - 1); k <= y + 1; k ++ ) {
            int z = stones[x] - k;
            if (hash.count(z)) {
                int p = hash[z];
                if (dp(p, k)) {
                    f[x][y] = 1;
                    break;
                }
            }
        }
        return f[x][y];
    }

    bool canCross(vector<int>& _stones) {
        stones = _stones;
        int n = stones.size();
        for (int i = 0; i < n; i ++ ) hash[stones[i]] = i;
        memset(f, -1, sizeof f);
        f[0][1] = 1;
        for (int i = 0; i < n; i ++ )
            if (dp(n - 1, i))
                return true;
        return false;
    }
};
```
#####  404. 左叶子之和
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
    int res = 0;

    int sumOfLeftLeaves(TreeNode* root) {
        dfs(root);
        return res;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        if (root->left) {
            if (!root->left->left && !root->left->right) {
                res += root->left->val;
            }
        }
        dfs(root->left);
        dfs(root->right);
    }
};
```
#####  405. 数字转换为十六进制数
```C++
class Solution {
public:
    string toHex(unsigned int num) {
        if (!num) return "0";
        string res, nums = "0123456789abcdef";
        while (num) {
            res += nums[num & 0xf];
            num >>= 4;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```
#####  406. 根据身高重建队列
```C++
class Solution {
public:
    int n;
    vector<int> tr;

    int lowbit(int x) {
        return x & -x;
    }

    void add(int x, int v) {
        for (int i = x; i <= n; i += lowbit(i)) tr[i] += v;
    }

    int query(int x) {
        int res = 0;
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }

    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        n = people.size();
        tr.resize(n + 1);

        sort(people.begin(), people.end(), [](vector<int>a, vector<int>b) {
            if (a[0] != b[0]) return a[0] < b[0];
            return a[1] > b[1];
        });

        vector<vector<int>> res(n);
        for (auto p: people) {
            int h = p[0], k = p[1];
            int l = 1, r = n;
            while (l < r) {
                int mid = l + r >> 1;
                if (mid - query(mid) >= k + 1) r = mid;
                else l = mid + 1;
            }
            res[r - 1] = p;
            add(r, 1);
        }
        return res;
    }
};
```
#####  407. 接雨水 II
```C++
class Solution {
public:
    struct Cell {
        int h, x, y;
        bool operator< (const Cell& t) const {
            return h > t.h;
        }
    };

    int trapRainWater(vector<vector<int>>& h) { 
        if (h.empty() || h[0].empty()) return 0;
        int n = h.size(), m = h[0].size();
        priority_queue<Cell> heap;
        vector<vector<bool>> st(n, vector<bool>(m));
        for (int i = 0; i < n; i ++ ) {
            st[i][0] = st[i][m - 1] = true;
            heap.push({h[i][0], i, 0});
            heap.push({h[i][m - 1], i, m - 1});
        }
        for (int i = 1; i + 1 < m; i ++ ) {
            st[0][i] = st[n - 1][i] = true;
            heap.push({h[0][i], 0, i});
            heap.push({h[n - 1][i], n - 1, i});
        }
        int res = 0;
        int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
        while (heap.size()) {
            auto t = heap.top();
            heap.pop();
            res += t.h - h[t.x][t.y];

            for (int i = 0; i < 4; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m && !st[x][y]) {
                    heap.push({max(h[x][y], t.h), x, y});
                    st[x][y] = true;
                }
            }
        }

        return res;
    }
};
```
 
### 究极班——试听课第7弹（Week 20. 第470-480题）

#####  470. 用 Rand7() 实现 Rand10()
```C++
// The rand7() API is already defined for you.
// int rand7();
// @return a random integer in the range 1 to 7

class Solution {
public:
    int rand10() {
        int t = (rand7() - 1) * 7 + rand7();  // 1~49
        if (t > 40) return rand10();
        return (t - 1) % 10 + 1;
    }
};
```
#####  472. 连接词
```C++
class Solution {
public:
    unordered_set<string> hash;

    bool check(string& word) {
        int n = word.size();
        vector<int> f(n + 1, INT_MIN);
        f[0] = 0;
        for (int i = 0; i <= n; i ++ ) {
            if (f[i] < 0) continue;
            for (int j = n - i; j; j -- ) {
                if (hash.count(word.substr(i, j))) {
                    f[i + j] = max(f[i + j], f[i] + 1);
                    if (f[n] > 1) return true;
                }
            }
        }
        return false;
    }

    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        for (auto& word: words) hash.insert(word);
        vector<string> res;
        for (auto& word: words)
            if (check(word))
                res.push_back(word);
        return res;
    }
};

```
#####  473. 火柴拼正方形
```C++
class Solution {
public:
    vector<int> nums;
    vector<bool> st;

    bool dfs(int start, int cur, int length, int cnt) {
        if (cnt == 3) return true;
        if (cur == length) return dfs(0, 0, length, cnt + 1);
        for (int i = start; i < nums.size(); i ++ ) {
            if (st[i]) continue;
            if (cur + nums[i] <= length) {
                st[i] = true;
                if (dfs(i + 1, cur + nums[i], length, cnt)) return true;
                st[i] = false;
            }
            if (!cur || cur + nums[i] == length) return false;
            while (i + 1 < nums.size() && nums[i + 1] == nums[i])
                i ++ ;
        }
        return false;
    }

    bool makesquare(vector<int>& _nums) {
        nums = _nums;
        if (nums.empty()) return false;
        st.resize(nums.size());
        int sum = 0;
        for (auto x: nums) sum += x;
        if (sum % 4) return false;
        sum /= 4;
        sort(nums.begin(), nums.end(), greater<int>());
        return dfs(0, 0, sum, 0);
    }
};

```
#####  474. 一和零
```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        for (auto& str: strs) {
            int a = 0, b = 0;
            for (auto c: str)
                if (c == '0') a ++ ;
                else b ++ ;
            for (int i = m; i >= a; i -- )
                for (int j = n; j >= b; j -- )
                    f[i][j] = max(f[i][j], f[i - a][j - b] + 1);
        }
        return f[m][n];
    }
};
```
#####  475. 供暖器
```C++
class Solution {
public:
    bool check(int mid, vector<int>& houses, vector<int>& heaters) {
        for (int i = 0, j = 0; i < houses.size(); i ++ ) {
            while (j < heaters.size() && abs(heaters[j] - houses[i]) > mid)
                j ++ ;
            if (j >= heaters.size()) return false;
        }
        return true;
    }

    int findRadius(vector<int>& houses, vector<int>& heaters) {
        sort(houses.begin(), houses.end());
        sort(heaters.begin(), heaters.end());
        int l = 0, r = INT_MAX;
        while (l < r) {
            int mid = (long long)l + r >> 1;
            if (check(mid, houses, heaters)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};

```
#####  476. 数字的补数
```C++
class Solution {
public:
    int findComplement(int num) {
        if (!num) return 1;
        int cnt = 0;
        for (int x = num; x; x >>= 1) cnt ++ ;
        return ~num & ((1ll << cnt) - 1);
    }
};
```
#####  477. 汉明距离总和
```C++
class Solution {
public:
    int totalHammingDistance(vector<int>& nums) {
        int res = 0;
        for (int i = 0; i <= 30; i ++ ) {
            int x = 0, y = 0;
            for (auto c: nums)
                if (c >> i & 1) y ++ ;
                else x ++ ;
            res += x * y;
        }
        return res;
    }
};
```
#####  478. 在圆内随机生成点
```C++
class Solution {
public:
    double r, x, y;

    Solution(double radius, double x_center, double y_center) {
        r = radius, x = x_center, y = y_center;
    }

    vector<double> randPoint() {
        double a = (double)rand() / RAND_MAX * 2 - 1;
        double b = (double)rand() / RAND_MAX * 2 - 1;
        if (a * a + b * b > 1) return randPoint();
        return {x + r * a, y + r * b};
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(radius, x_center, y_center);
 * vector<double> param_1 = obj->randPoint();
 */
```
#####  479. 最大回文数乘积
```C++
class Solution {
public:
    int largestPalindrome(int n) {
        typedef long long LL;
        if (n == 1) return 9;
        int maxv = pow(10, n) - 1;
        for (int i = maxv; ;i -- ) {
            auto a = to_string(i);
            auto b = a;
            reverse(b.begin(), b.end());
            auto num = stoll(a + b);
            for (LL j = maxv; j * j >= num; j -- )
                if (num % j == 0)
                    return num % 1337;
        }
        return 0;
    }
};

```
#####  480. 滑动窗口中位数
```C++
class Solution {
public:
    int k;
    multiset<int> left, right;

    double get_medium() {
        if (k % 2) return *right.begin();
        return ((double)*left.rbegin() + *right.begin()) / 2;
    }

    vector<double> medianSlidingWindow(vector<int>& nums, int _k) {
        k = _k;
        for (int i = 0; i < k; i ++ ) right.insert(nums[i]);
        for (int i = 0; i < k / 2; i ++ ) {
            left.insert(*right.begin());
            right.erase(right.begin());
        }
        vector<double> res;
        res.push_back(get_medium());
        for (int i = k; i < nums.size(); i ++ ) {
            int x = nums[i], y = nums[i - k];
            if (x >= *right.begin()) right.insert(x);
            else left.insert(x);
            if (y >= *right.begin()) right.erase(right.find(y));
            else left.erase(left.find(y));

            while (left.size() > right.size()) {
                right.insert(*left.rbegin());
                left.erase(left.find(*left.rbegin()));
            }
            while (right.size() > left.size() + 1) {
                left.insert(*right.begin());
                right.erase(right.begin());
            }
            res.push_back(get_medium());
        }
        return res;
    }
};

```
 
### 究极班——试听课第8弹（Week 22. 第520-530题）

#####  520. 检测大写字母
```C++
class Solution {
public:
    bool check(char c) {
        return c >= 'A' && c <= 'Z';
    }

    bool detectCapitalUse(string word) {
        int s = 0;
        for (auto c: word)
            if (check(c))
                s ++ ;
        return s == word.size() || !s || s == 1 && check(word[0]);
    }
};
```
#####  521. 最长特殊序列 Ⅰ
```C++
class Solution {
public:
    int findLUSlength(string a, string b) {
        if (a == b) return -1;
        return max(a.size(), b.size());
    }
};
```
#####  522. 最长特殊序列 II
```C++
class Solution {
public:
    bool check(string& a, string& b) {
        int k = 0;
        for (auto c: b)
            if (k < a.size() && c == a[k])
                k ++ ;
        return k == a.size();
    }

    int findLUSlength(vector<string>& strs) {
        int res = -1;
        for (int i = 0; i < strs.size(); i ++ ) {
            bool is_sub = false;
            for (int j = 0; j < strs.size(); j ++ )
                if (i != j && check(strs[i], strs[j])) {
                    is_sub = true;
                    break;
                }
            if (!is_sub) res = max(res, (int)strs[i].size());
        }
        return res;
    }
};

```
#####  523. 连续的子数组和
```C++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        if (!k) {
            for (int i = 1; i < nums.size(); i ++ )
                if (!nums[i - 1] && !nums[i])
                    return true;
            return false;
        }
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + nums[i - 1];
        unordered_set<int> hash;
        for (int i = 2; i <= n; i ++ ) {
            hash.insert(s[i - 2] % k);
            if (hash.count(s[i] % k)) return true;
        }
        return false;
    }
};
```
#####  524. 通过删除字母匹配到字典里最长单词
```C++
class Solution {
public:
    bool check(string& a, string& b) {
        int i = 0, j = 0;
        while (i < a.size() && j < b.size()) {
            if (a[i] == b[j]) i ++ ;
            j ++ ;
        }
        return i == a.size();
    }

    string findLongestWord(string s, vector<string>& d) {
        string res;
        for (auto str: d)
            if (check(str, s)) {
                if (res.empty() || res.size() < str.size() || res.size() == str.size() && res > str)
                    res = str;
            }
        return res;
    }
};
```
#####  525. 连续数组
```C++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int, int> hash;
        hash[0] = 0;
        int res = 0;
        for (int i = 1, one = 0, zero = 0; i <= n; i ++ ) {
            int x = nums[i - 1];
            if (x == 0) zero ++ ;
            else one ++ ;
            int s = one - zero;
            if (hash.count(s)) res = max(res, i - hash[s]);
            else hash[s] = i;
        }
        return res;
    }
};

```
#####  526. 优美的排列
```C++
class Solution {
public:
    int countArrangement(int n) {
        vector<int> f(1 << n);
        f[0] = 1;
        for (int i = 0; i < 1 << n; i ++ ) {
            int k = 0;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                    k ++ ;
            for (int j = 0; j < n; j ++ )
                if (!(i >> j & 1)) {
                    if ((k + 1) % (j + 1) == 0 || (j + 1) % (k + 1) == 0)
                        f[i | (1 << j)] += f[i];
                }
        }
        return f[(1 << n) - 1];
    }
};

```
#####  528. 按权重随机选择
```C++
class Solution {
public:
    vector<int> s;

    Solution(vector<int>& w) {
        s = w;
        for (int i = 1; i < s.size(); i ++ ) s[i] += s[i - 1];
    }

    int pickIndex() {
        int x = rand() % s.back() + 1;
        return lower_bound(s.begin(), s.end(), x) - s.begin();
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(w);
 * int param_1 = obj->pickIndex();
 */
```
#####  529. 扫雷游戏
```C++
class Solution {
public:
    int n, m;

    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        n = board.size(), m = board[0].size();
        int x = click[0], y = click[1];
        if (board[x][y] == 'M') {
            board[x][y] = 'X';
            return board;
        }
        dfs(board, x, y);
        return board;
    }

    void dfs(vector<vector<char>>& board, int x, int y) {
        if (board[x][y] != 'E') return;
        int s = 0;
        for (int i = max(x - 1, 0); i <= min(n - 1, x + 1); i ++ )
            for (int j = max(y - 1, 0); j <= min(m - 1, y + 1); j ++ )
                if (i != x || j != y)
                    if (board[i][j] == 'M' || board[i][j] == 'X')
                        s ++ ;
        if (s) {
            board[x][y] = '0' + s;
            return;
        }
        board[x][y] = 'B';
        for (int i = max(x - 1, 0); i <= min(n - 1, x + 1); i ++ )
            for (int j = max(y - 1, 0); j <= min(m - 1, y + 1); j ++ )
                if (i != x || j != y)
                    dfs(board, i, j);
    }
};

```
#####  530. 二叉搜索树的最小绝对差
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
    int ans = INT_MAX, last;
    bool is_first = true;

    int getMinimumDifference(TreeNode* root) {
        dfs(root);
        return ans;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        if (is_first) is_first = false;
        else ans = min(ans, root->val - last);
        last = root->val;
        dfs(root->right);
    }
};

```

### 究极班——试听课第9弹（Week 26. 第643-654题）

#####  643. 子数组最大平均数 I
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
#####  645. 错误的集合
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
#####  646. 最长数对链
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
#####  647. 回文子串
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
#####  648. 单词替换
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
#####  649. Dota2 参议院
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
#####  650. 只有两个键的键盘
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
#####  652. 寻找重复的子树
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
#####  653. 两数之和 IV - 输入 BST
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
#####  654. 最大二叉树
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

### 究极班——试听课第10弹（Week 27. 第655-678题）

#####  655. 输出二叉树
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
#####  657. 机器人能否返回原点
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
#####  658. 找到 K 个最接近的元素

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
#####  659. 分割数组为连续子序列
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
#####  661. 图片平滑器
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
#####  662. 二叉树最大宽度
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
#####  664. 奇怪的打印机
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
#####  665. 非递减数列
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
#####  667. 优美的排列 II
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
#####  668. 乘法表中第k小的数
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
#####  669. 修剪二叉搜索树
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
#####  670. 最大交换
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
#####  671. 二叉树中第二小的节点
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
#####  672. 灯泡开关 Ⅱ
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
#####  673. 最长递增子序列的个数
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
#####  674. 最长连续递增序列
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
#####  675. 为高尔夫比赛砍树
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
#####  676. 实现一个魔法字典
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
#####  677. 键值映射
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
#####  678. 有效的括号字符串
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

### 究极班——试听课第11弹（Week 31. 第753-766题）

#####  753. 破解保险箱
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
#####  754. 到达终点数字
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
#####  756. 金字塔转换矩阵
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

#####  757. 设置交集大小至少为2
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
#####  761. 特殊的二进制序列
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
#####  762. 二进制表示中质数个计算置位
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
#####  763. 划分字母区间
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
#####  764. 最大加号标志
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
#####  765. 情侣牵手
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
#####  766. 托普利茨矩阵
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
 
### 究极班——试听课第12弹（Week 35. 第831-840题）

#####  831. 隐藏个人信息
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
#####  832. 翻转图像
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
#####  833. 字符串中的查找与替换
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
#####  834. 树中距离之和
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
#####  835. 图像重叠
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
#####  836. 矩形重叠
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
#####  837. 新21点
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
#####  838. 推多米诺
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
#####  839. 相似字符串组
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
#####  840. 矩阵中的幻方
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

### 究极班——试听课第13弹（Week 37. 第871-880题）

#####  871. 最低加油次数
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
#####  872. 叶子相似的树
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
#####  873. 最长的斐波那契子序列的长度
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
#####  874. 模拟行走机器人
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
#####  875. 爱吃香蕉的珂珂
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
#####  876. 链表的中间结点
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
#####  877. 石子游戏
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
#####  878. 第 N 个神奇数字
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
#####  879. 盈利计划
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
#####  880. 索引处的解码字符串
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

### 究极班——试听课第14弹（Week 40. 第921-930题）

#####  921. 使括号有效的最少添加
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
#####  922. 按奇偶排序数组 II
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
#####  923. 三数之和的多种可能
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
#####  924. 尽量减少恶意软件的传播
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
#####  925. 长按键入
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
#####  926. 将字符串翻转到单调递增
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
#####  927. 三等分
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
#####  928. 尽量减少恶意软件的传播 II
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
#####  929. 独特的电子邮件地址
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
#####  930. 和相同的二元子数组
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

### 究极班——试听课第15弹（Week 43. 第971-980题）

#####  971. 翻转二叉树以匹配先序遍历
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
#####  972. 相等的有理数
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
#####  973. 最接近原点的 K 个点
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
#####  974. 和可被 K 整除的子数组
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
#####  975. 奇偶跳
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
#####  976. 三角形的最大周长
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
#####  977. 有序数组的平方
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
#####  978. 最长湍流子数组
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
#####  980. 不同路径 III
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


### 究极班——试听课第16弹（Week 48. 第1023-1032题)

#####  1023. 驼峰式匹配
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
#####  1024. 视频拼接
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
#####  1025. 除数博弈
```C++
class Solution {
public:
    bool divisorGame(int n) {
        return n % 2 == 0;
    }
};
```
#####  1026. 节点与其祖先之间的最大差值
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
#####  1027. 最长等差数列
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
#####  1028. 从先序遍历还原二叉树
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
#####  1029. 两地调度
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
#####  1030. 距离顺序排列矩阵单元格
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
#####  1031. 两个非重叠子数组的最大和
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
#####  1032. 字符流
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




