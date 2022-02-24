参考：https://www.acwing.com/activity/content/activity_person/content/6022/1/

LeetCode究极班：https://www.acwing.com/activity/content/activity_person/content/29799/1/

# AcWing 2019 LeetCode暑期打卡活动

### Week1 二分

##### LeetCode 69. Sqrt(x)

```C++
class Solution {
public:
    int mySqrt(int x) {
        int l = 0, r = x;
        while (l < r)
        {
            int mid = l + 1 + (long long)r >> 1;
            if (mid <= x / mid) l = mid;
            else r = mid - 1;
        }
        return r;
    }
```
##### LeetCode 35. Search Insert Position

```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        if (nums.empty() || nums.back() < target) return nums.size();

        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        return r;
    }
};
```
##### LeetCode 34. Find First and Last Position of Element in Sorted Array

```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if (nums.empty()) return {-1, -1};

        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if (nums[r] != target) return {-1, -1};
        int res = l;

        l = 0 , r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if (nums[mid] <= target) l = mid;
            else r = mid - 1;
        }

        return {res, r};
    }
};
```
##### LeetCode 74. Search a 2D Matrix

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;

        int n = matrix.size(), m = matrix[0].size();
        int l = 0, r = n * m - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (matrix[mid / m][mid % m] >= target) r = mid;
            else l = mid + 1;
        }

        return matrix[r / m][r % m] == target;
    }
};
```
##### LeetCode 153. Find Minimum in Rotated Sorted Array

```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] <= nums.back()) r = mid;
            else l = mid + 1;
        }
        return nums[r];
    }
};
```
##### LeetCode 33. Search in Rotated Sorted Array
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty()) return -1;

        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] <= nums.back()) r = mid;
            else l = mid + 1;
        }

        if (target <= nums.back()) r = nums.size() - 1;
        else l = 0, r -- ;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if (nums[l] == target) return l;
        return -1;
    }
};
```
##### LeetCode 278. First Bad Version
```C++
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r)
        {
            int mid = l + (long long)r >> 1;
            if (isBadVersion(mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
##### LeetCode 162. Find Peak Element

```C++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] > nums[mid + 1]) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
##### LeetCode 287. Find the Duplicate Number

```C++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size() - 1;
        int l = 1, r = n;
        while (l < r)
        {
            int mid = l + r >> 1;

            int cnt = 0;
            for (int x : nums) cnt += l <= x && x <= mid;
            if (cnt > mid - l + 1) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```

##### LeetCode 275. H-Index II
```C++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        if (citations.empty() || !citations.back()) return 0;
        int n = citations.size();
        int l = 0, r = n - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (citations[mid] >= n - mid) r = mid;
            else l = mid + 1;
        }
        return n - r;
    }
};
```

### Week2 链表

##### LeetCode 19. Remove Nth Node From End of List

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        auto first = dummy, second = dummy;
        while (n -- ) first = first->next;
        while (first->next) 
        {
            first = first->next;
            second = second->next;
        }

        second->next = second->next->next;

        return dummy->next;
    }
};
```

##### LeetCode 237. Delete Node in a Linked List
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
        *node = *(node->next);
    }
};
```
##### LeetCode 83. Remove Duplicates from Sorted List
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
    ListNode* deleteDuplicates(ListNode* head) {
        auto cur = head;
        while (cur)
        {
            while (cur->next && cur->val == cur->next->val) cur->next = cur->next->next;
            cur = cur->next;
        }
        return head;
    }
};
```

##### LeetCode 61. Rotate List
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
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return NULL;

        int n = 0;
        for (auto cur = head; cur; cur = cur->next) n ++ ;
        k %= n;

        auto first = head, second = head;
        while (k -- ) first = first->next;
        while (first->next)
        {
            first = first->next;
            second = second->next;
        }

        first->next = head;
        head = second->next;
        second->next = NULL;

        return head;
    }
};
```

##### LeetCode 206. Reverse Linked List
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
        if (!head) return NULL;

        auto a = head, b = a->next;
        head->next = NULL;
        while (b)
        {
            auto c = b->next;
            b->next = a;
            a = b, b = c;
        }
        return a;
    }
};
```
##### LeetCode 24. Swap Nodes in Pairs
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
    ListNode* swapPairs(ListNode* head) {
        auto dummy = new ListNode(-1);
        dummy->next = head;

        for (auto p = dummy; p && p->next && p->next->next; p = p->next->next)
        {
            auto a = p->next, b = a->next;
            p->next = b;
            a->next = b->next;
            b->next = a;
        }

        return dummy->next;
    }
};
```
##### LeetCode 92. Reverse Linked List II
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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (m == n) return head;

        auto dummy = new ListNode(-1);
        dummy->next = head;

        auto a = dummy, d = dummy;
        for (int i = 0; i < m - 1; i ++ ) a = a->next;
        for (int i = 0; i < n; i ++ ) d = d->next;
        auto b = a->next, c = d->next;

        for (auto p = b, q = b->next; q != c;)
        {
            auto o = q->next;
            q->next = p;
            p = q, q = o;
        }

        a->next = d;
        b->next = c;
        return dummy->next;
    }
};
```
##### LeetCode 160. Intersection of Two Linked Lists
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
        while (p != q)
        {
            if (p) p = p->next;
            else p = headB;
            if (q) q = q->next;
            else q = headA;
        }
        return p;
    }
};

```
##### LeetCode 142. Linked List Cycle II
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
    ListNode *detectCycle(ListNode *head) {
        auto first = head, second = head;
        while (second)
        {
            first = first->next;
            second = second->next;
            if (second) second = second->next;
            else break;

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
        return NULL;
    }
};
```
##### LeetCode 148. Sort List
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
    ListNode* sortList(ListNode* head) {
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;
        auto dummy = new ListNode(-1);
        dummy->next = head;

        for (int i = 1; i < n; i *= 2)
        {
            auto cur = dummy;
            for (int j = 0; j + i < n; j += i * 2)
            {
                auto first = cur->next, second = first;
                for (int k = 0; k < i; k ++ ) second = second->next;

                int f = 0, s = 0;
                while (f < i && s < i && second)
                    if (first->val <= second->val)
                    {
                        cur->next = first;
                        cur = first;
                        first = first->next;
                        f ++ ;
                    }
                    else
                    {
                        cur->next = second;
                        cur = second;
                        second = second->next;
                        s ++ ;
                    }
                while (f < i)
                {
                    cur->next = first;
                    cur = first;
                    first = first->next;
                    f ++ ;
                }
                while (s < i && second)
                {
                    cur->next = second;
                    cur = second;
                    second = second->next;
                    s ++ ;
                }

                cur->next = second;
            }
        }
        return dummy->next;
    }
};
```
### Week3 树

##### LeetCode 98. Validate Binary Search Tree
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
    bool isValidBST(TreeNode* root) {
        return dfs(root, INT_MIN, INT_MAX);
    }

    bool dfs(TreeNode* root, long long minv, long long maxv)
    {
        if (!root) return true;
        if (root->val < minv || root->val > maxv) return false;

        return dfs(root->left, minv, root->val - 1ll) && dfs(root->right, root->val + 1ll, maxv);
    }
};
```
LeetCode 101. Symmetric Tree
```C++
递归写法
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

    bool dfs(TreeNode *p, TreeNode *q)
    {
        if (!p && !q) return true;
        if (p && q) return p->val == q->val && dfs(p->left, q->right) && dfs(p->right, q->left);
        return false;
    }
};
迭代写法
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

        stack<TreeNode*> left, right;
        auto lc = root->left, rc = root->right;
        while (lc || rc || left.size())
        {
            while (lc && rc)
            {
                left.push(lc), right.push(rc);
                lc = lc->left, rc = rc->right;
            }

            if (lc || rc) return false;
            lc = left.top(), left.pop();
            rc = right.top(), right.pop();

            if (lc->val != rc->val) return false;
            lc = lc->right, rc = rc->left;
        }

        return true;
    }
};
```
##### LeetCode 94. Binary Tree Inorder Traversal
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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        if (!root) return res;
        stack<TreeNode*> stk;
        auto cur = root;

        while (cur || stk.size())
        {
            while (cur)
            {
                stk.push(cur);
                cur = cur->left;
            }

            cur = stk.top(), stk.pop();
            res.push_back(cur->val);
            cur = cur->right;
        }

        return res;
    }
};

```
##### LeetCode 105. Construct Binary Tree from Preorder and Inorder Traversal
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
        int n = preorder.size();
        for (int i = 0; i < n; i ++ ) pos[inorder[i]] = i;

        return dfs(preorder, inorder, 0, n - 1, 0, n - 1);
    }

    TreeNode* dfs(vector<int>& preorder, vector<int>& inorder, int pl, int pr, int il, int ir) {
        if (pl > pr) return NULL;
        int k = pos[preorder[pl]];
        int len = k - il;
        auto root = new TreeNode(preorder[pl]);
        root->left = dfs(preorder, inorder, pl + 1, pl + len, il, k - 1);
        root->right = dfs(preorder, inorder, pl + len + 1, pr, k + 1, ir);
        return root;
    }
};
```

##### LeetCode 102. Binary Tree Level Order Traversal
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
        if (!root) return res;

        queue<TreeNode*> q;

        q.push(root);

        while (q.size())
        {
            int len = q.size();
            vector<int> level;
            for (int i = 0; i < len; i ++ )
            {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }
            if (level.size()) res.push_back(level);
        }

        return res;
    }
};
```
##### LeetCode 236. Lowest Common Ancestor of a Binary Tree
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
        if (!root || root == p || root == q) return root;
        auto left = lowestCommonAncestor(root->left, p, q);
        auto right = lowestCommonAncestor(root->right, p, q);
        return !left ? right : !right ? left : root;
    }
};
```
##### LeetCode 297. Serialize and Deserialize Binary Tree
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
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res;
        dfs1(root, res);
        return res;
    }

    void dfs1(TreeNode* root, string& res)
    {
        if (!root)
        {
            res += "#,";
            return;
        }
        res += to_string(root->val) + ',';
        dfs1(root->left, res);
        dfs1(root->right, res);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        int u = 0;
        return dfs2(data, u);
    }

    TreeNode* dfs2(string &data, int &u)
    {
        if (data[u] == '#')
        {
            u += 2;
            return NULL;
        }

        int t = 0;
        bool is_minus = false;
        while (data[u] != ',')
        {
            if (data[u] == '-') is_minus = true;
            else t = t * 10 + data[u] - '0';
            u ++ ;
        }
        u ++ ;
        if (is_minus) t = -t;

        auto root = new TreeNode(t);
        root->left = dfs2(data, u);
        root->right = dfs2(data, u);

        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```
##### LeetCode 543. Diameter of Binary Tree
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

    int ans = 0;

    int diameterOfBinaryTree(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root)
    {
        if (!root) return 0;
        auto l = dfs(root->left);
        auto r = dfs(root->right);
        ans = max(ans, l + r);
        return max(l, r) + 1;
    }
};
```
##### LeetCode 124. Binary Tree Maximum Path Sum
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

    int ans = INT_MIN;

    int maxPathSum(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root)
    {
        if (!root) return 0;
        int l = dfs(root->left);
        int r = dfs(root->right);
        ans = max(ans, l + r + root->val);
        return max(0, max(l, r) + root->val);    }
};
```
##### LeetCode 173. Binary Search Tree Iterator
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
class BSTIterator {
public:

    stack<TreeNode*> stk;

    BSTIterator(TreeNode* root) {
        while (root)
        {
            stk.push(root);
            root = root->left;
        }
    }

    /** @return the next smallest number */
    int next() {
        auto cur = stk.top();
        stk.pop();
        int t = cur->val;
        cur = cur->right;
        while (cur)
        {
            stk.push(cur);
            cur = cur->left;
        }
        return t;
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return stk.size();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```
### Week4 字符串

##### LeetCode 38. Count and Say
```C++
class Solution {
public:
    string countAndSay(int n) {
        string s = "1";
        for (int i = 0; i < n - 1; i ++ )
        {
            string ns;
            for (int j = 0; j < s.size(); j ++ )
            {
                int k = j;
                while (k < s.size() && s[k] == s[j]) k ++ ;
                ns += to_string(k - j) + s[j];
                j = k - 1;
            }
            s = ns;
        }
        return s;
    }
};
```
##### LeetCode 49. Group Anagrams
```C++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> hash;
        for (auto str : strs)
        {
            string s = str;
            sort(str.begin(), str.end());
            hash[str].push_back(s);
        }
        vector<vector<string>> res;
        for (auto item : hash) res.push_back(item.second);
        return res;
    }
};
```
##### LeetCode 151. Reverse Words in a String
```C++
class Solution {
public:
    string reverseWords(string s) {
        int k = 0;
        for (int i = 0; i < s.size();)
        {
            while (i < s.size() && s[i] == ' ') i ++ ;
            if (i == s.size()) break;
            int j = i;
            while (j < s.size() && s[j] != ' ') j ++ ;
            reverse(s.begin() + i, s.begin() + j);
            if  (k) s[k ++ ] = ' ';
            while (i < j) s[k ++ ] = s[i ++ ];
        }
        s.erase(s.begin() + k, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};
```
##### LeetCode 165. Compare Version Numbers
```C++
class Solution {
public:
    int compareVersion(string s1, string s2) {
        int i = 0, j = 0;
        while (i < s1.size() || j < s2.size())
        {
            int x = i, y = j;
            while (x < s1.size() && s1[x] != '.') x ++ ;
            while (y < s2.size() && s2[y] != '.') y ++ ;
            int a = x == i ? 0 : atoi(s1.substr(i, x - i).c_str());
            int b = y == j ? 0 : atoi(s2.substr(j, y - j).c_str());
            if (a > b) return 1;
            if (a < b) return -1;
            i = x + 1, j = y + 1;
        }
        return 0;
    }
};
```


##### LeetCode 929. Unique Email Addresses
```C++
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> hash;
        for (auto email : emails)
        {
            int at = email.find('@');
            string name;
            for (char c : email.substr(0, at))
                if (c == '+') break;
                else if (c != '.') name += c;
            string domain = email.substr(at + 1);

            hash.insert(name + '@' + domain);
        }

        return hash.size();
    }
};

```
##### LeetCode 5. Longest Palindromic Substring
```C++
class Solution {
public:
    string longestPalindrome(string s) {
        string res;
        for (int i = 0; i < s.size(); i ++ )
        {
            int j, k;
            for (j = i, k = i; j >= 0 && k < s.size() && s[j] == s[k]; j --, k ++ )
                if (res.size() < k - j + 1)
                    res = s.substr(j, k - j + 1);
            for (j = i - 1, k = i; j >= 0 && k < s.size() && s[j] == s[k]; j --, k ++ )
                if (res.size() < k - j + 1)
                    res = s.substr(j, k - j + 1);
        }
        return res;
    }
};
```
##### LeetCode 6. ZigZag Conversion
```C++
class Solution {
public:
    string convert(string s, int n) {
        if (n == 1) return s;
        string res;
        for (int i = 0; i < n; i ++ )
        {
            if (!i || i == n - 1)
            {
                for (int j = i; j < s.size(); j += 2 * (n - 1)) res += s[j];
            }
            else
            {
                for (int j = i, k = 2 * (n - 1) - i; j < s.size() || k < s.size(); j += 2 * (n - 1), k += 2 * (n - 1))
                {
                    if (j < s.size()) res += s[j];
                    if (k < s.size()) res += s[k];
                }
            }
        }
        return res;
    }
};
```
##### LeetCode 3. Longest Substring Without Repeating Characters
```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> hash;
        int res = 0;
        for (int i = 0, j = 0; j < s.size(); j ++ )
        {
            hash[s[j]] ++ ;
            while (hash[s[j]] > 1) hash[s[i ++ ]] -- ;
            res = max(res, j - i + 1);
        }
        return res;
    }
};
```
##### LeetCode 208. Implement Trie (Prefix Tree)
```C++
class Trie {
public:

    struct Node
    {
        bool is_end;
        Node *son[26];

        Node()
        {
            is_end = false;
            for (int i = 0; i < 26; i ++ ) son[i] = NULL;
        }
    } *root;

    /** Initialize your data structure here. */
    Trie() {
        root = new Node();
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        auto *p = root;
        for (auto c : word)
        {
            int u = c - 'a';
            if (p->son[u] == NULL) p->son[u] = new Node();
            p = p->son[u];
        }
        p->is_end = true;
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        auto *p = root;
        for (auto c : word)
        {
            int u = c - 'a';
            if (p->son[u] == NULL) return false;
            p = p->son[u];
        }
        return p->is_end;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        auto *p = root;
        for (auto c : prefix)
        {
            int u = c - 'a';
            if (p->son[u] == NULL) return false;
            p = p->son[u];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```
##### LeetCode 273. Integer to English Words
```C++
class Solution {
public:

    string bit[20] = {"Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine",
                       "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    string decade[10] = {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};

    string numberToWords(int num) {
        if (!num) return bit[0];
        string res;

        string big[4] = {"Billion", "Million", "Thousand", ""};

        for (int i = 1000000000, j = 0; i > 0; i /= 1000, j ++ )
        {
            if (num >= i)
            {
                res += get_part(num / i) + big[j] + ' ';
                num %= i;
            }
        }

        while (res.back() == ' ') res.pop_back();
        return res;
    }

    string get_part(int num)
    {
        string res;
        if (num >= 100)
        {
            res += bit[num / 100] + " Hundred ";
            num %= 100;
        }
        if (!num) return res;
        if (num >= 20)
        {
            res += decade[num / 10] + ' ';
            num %= 10;
            if (num) res += bit[num] + ' ';
            return res;
        }
        return res + bit[num] + ' ';
    }
};
```

##### LeetCode 17. Letter Combinations of a Phone Number
```C++
class Solution {
public:

    vector<string> letterCombinations(string digits) {

        if (digits.empty()) return vector<string>();

        string chars[8] = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

        vector<string> res(1, "");
        for (auto u : digits)
        {
            vector<string> now;
            for (char c : chars[u - '2'])
                for (auto path : res)
                    now.push_back(path + c);
            res = now;
        }
        return res;
    }
};
```

### Week5 DFS+回溯

##### LeetCode 79. Word Search
```C++
class Solution {
public:

    int n, m;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    bool exist(vector<vector<char>>& board, string word) {
        if (board.empty() || board[0].empty()) return false;

        n = board.size(), m = board[0].size();
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (dfs(board, i, j, word, 0))
                    return true;
        return false;
    }

    bool dfs(vector<vector<char>>& board, int x, int y, string &word, int u)
    {
        if (board[x][y] != word[u]) return false;
        if (u == word.size() - 1) return true;

        board[x][y] = '.';
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m)
            {
                if (dfs(board, a, b, word, u + 1)) return true;
            }
        }
        board[x][y] = word[u];
        return false;
    }
};
```
##### LeetCode 46. Permutations
```C++
class Solution {
public:

    vector<vector<int>> ans;

    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> st(nums.size());
        vector<int> path;
        dfs(nums, 0, st, path);
        return ans;
    }

    void dfs(vector<int>& nums, int u, vector<bool>& st, vector<int>& path)
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return;
        }

        for (int i = 0; i < nums.size(); i ++ )
            if (!st[i])
            {
                st[i] = true;
                path.push_back(nums[i]);
                dfs(nums, u + 1, st, path);
                path.pop_back();
                st[i] = false;
            }
    }
};
```
##### LeetCode 47. Permutations II
```C++
class Solution {
public:

    int n;
    vector<vector<int>> ans;
    vector<bool> st;
    vector<int> path;

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        n = nums.size();
        st = vector<bool>(n);

        dfs(nums, 0);

        return ans;
    }

    void dfs(vector<int>& nums, int u)
    {
        if (u == n)
        {
            ans.push_back(path);
            return;
        }

        for (int i = 0; i < n; i ++ )
            if (!st[i])
            {
                st[i] = true;
                path.push_back(nums[i]);
                dfs(nums, u + 1);
                path.pop_back();
                st[i] = false;

                while (i + 1 < n && nums[i + 1] == nums[i]) i ++ ;
            }
    }
};
```
##### LeetCode 78. Subsets
```C++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        int n = nums.size();

        for (int i = 0; i < 1 << n; i ++ )
        {
            vector<int> path;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                    path.push_back(nums[j]);
            ans.push_back(path);
        }

        return ans;
    }
};
```
##### LeetCode 90. Subsets II
```C++
class Solution {
public:

    int n;
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        n = nums.size();

        dfs(nums, 0);

        return ans;
    }

    void dfs(vector<int>& nums, int u)
    {
        if (u == n)
        {
            ans.push_back(path);
            return;
        }

        int k = 0;
        while (u + k < n && nums[u + k] == nums[u]) k ++ ;
        for (int i = 0; i <= k; i ++ )
        {
            dfs(nums, u + k);
            path.push_back(nums[u]);
        }
        for (int i = 0; i <= k; i ++ ) path.pop_back();
    }
};
```
##### LeetCode 216. Combination Sum III
```C++
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(k, n, 1);
        return ans;
    }

    void dfs(int k, int n, int start)
    {
        if (!k)
        {
            if (!n) ans.push_back(path);
            return;
        }

        for (int i = start; i <= 10 - k; i ++ )
        {
            path.push_back(i);
            dfs(k - 1, n - i, i + 1);
            path.pop_back();
        }
    }
};
```
##### LeetCode 52. N-Queens II
```C++
class Solution {
public:

    int n;
    int ans = 0;
    vector<bool> st, d, ud;

    int totalNQueens(int _n) {
        n = _n;
        st = vector<bool>(nlse);
        d = ud = vector<bool>(n * 2);
        dfs(0);
        return ans;
    }

    void dfs(int u)
    {
        if (u == n)
        {
            ans ++ ;
            return;
        }

        for (int i = 0; i < n; i ++ )
        {
            if (!st[i] && !d[i + u] && !ud[i - u + n])
            {
                st[i] = d[i + u] = ud[i - u + n] = true;
                dfs(u + 1);
                st[i] = d[i + u] = ud[i - u + n] = false;
            }
        }
    }
};
```
##### LeetCode 37. Sudoku Solver
```C++
class Solution {
public:

    bool col[9][9] = {0}, row[9][9] = {0}, cell[3][3][9] = {0};

    void solveSudoku(vector<vector<char>>& board) {
        for (int i = 0; i < 9; i ++ )
            for (int j = 0; j < 9; j ++ )
            {
                char c = board[i][j];
                if (c != '.')
                {
                    int t = c - '1';
                    row[i][t] = col[j][t] = cell[i / 3][j / 3][t] = true;
                }
            }
        dfs(board, 0, 0);
    }

    bool dfs(vector<vector<char>>& board, int x, int y)
    {
        if (y == 9) x ++, y = 0;
        if (x == 9) return true;

        if (board[x][y] != '.') return dfs(board, x, y + 1);

        for (int i = 0; i < 9; i ++ )
            if (!row[x][i] && !col[y][i] && !cell[x / 3][y / 3][i])
            {
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = true;
                board[x][y] = '1' + i;
                if (dfs(board, x, y + 1)) return true;
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = false;
                board[x][y] = '.';
            }
        return false;
    }
};
```
##### LeetCode 473. Matchsticks to Square
```C++
运行时间: 4ms

搜索顺序：

依次拼正方形的每条边
剪枝：

从大到小枚举，每次剪枝去掉的分支会更多；
每条边内部的木棒长度规定成从大到小；
如果当前木棒拼接失败，则跳过接下来所有长度相同的木棒；
如果当前木棒拼接失败，且是当前边的第一个，则直接剪掉当前分支；
如果当前木棒拼接失败，且是当前边的最后一个，则直接剪掉当前分支；
class Solution {
public:

    vector<bool> st;

    bool makesquare(vector<int>& nums) {
        int sum = 0;
        for (auto num : nums) sum += num;
        if (!sum || sum % 4) return false;

        sort(nums.begin(), nums.end());
        reverse(nums.begin(), nums.end());

        st = vector<bool>(nums.size());

        return dfs(nums, 0, 0, 0, sum / 4);
    }

    bool dfs(vector<int>& nums, int u, int cur, int start, int length)
    {
        if (u == 4) return true;
        if (cur == length) return dfs(nums, u + 1, 0, 0, length);

        for (int i = start; i < nums.size(); i ++ )
            if (!st[i] && cur + nums[i] <= length)
            {
                st[i] = true;
                if (dfs(nums, u, cur + nums[i], i + 1, length)) return true;
                st[i] = false;

                while (i + 1 < nums.size() && nums[i + 1] == nums[i]) i ++ ;
                if (!cur) return false;
                if (cur + nums[i] == length) return false;
            }

        return false;
    }
};
```
### Week6 滑动窗口、双指针、单调队列、单调栈

##### LeetCode 167. Two Sum II - Input array is sorted
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for (int i = 0, j = numbers.size() - 1; i < numbers.size(); i ++ )
        {
            while (j >= 0 && numbers[i] + numbers[j] > target) j -- ;
            if (numbers[i] + numbers[j] == target) return {i + 1, j + 1};
        }
        return {-1, -1};
    }
};
```
##### LeetCode 88. Merge Sorted Array
```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1, j = n - 1, k = m + n - 1;
        while (i >= 0 && j >= 0)
            if (nums1[i] > nums2[j]) nums1[k -- ] = nums1[i -- ];
            else nums1[k -- ] = nums2[j -- ];
        while (i >= 0) nums1[k -- ] = nums1[i -- ];
        while (j >= 0) nums1[k -- ] = nums2[j -- ];
    }
};
```
##### LeetCode 26. Remove Duplicates from Sorted Array
```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        int k = 1;
        for (int i = 1; i < nums.size(); i ++ )
            if (nums[i] != nums[k - 1])
                nums[k ++ ] = nums[i];
        return k;
    }
};
```
##### LeetCode 76. Minimum Window Substring
```C++
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> hash;
        int cnt = 0;
        for (auto c : t)
        {
            if (!hash[c]) cnt ++ ;
            hash[c] ++ ;
        }

        string res = "";
        for (int i = 0, j = 0, c = 0; i < s.size(); i ++ )
        {
            if (hash[s[i]] == 1) c ++ ;
            hash[s[i]] -- ;
            while (c == cnt && hash[s[j]] < 0) hash[s[j ++ ]] ++ ;
            if (c == cnt)
            {
                if (res.empty() || res.size() > i - j + 1) res = s.substr(j, i - j + 1);
            }
        }

        return res;
    }
};
```
##### LeetCode 32. Longest Valid Parentheses
```C++
class Solution {
public:

    int work(string s)
    {
        int res = 0;
        for (int i = 0, start = 0, cnt = 0; i < s.size(); i ++ )
            if (s[i] == '(') cnt ++ ;
            else
            {
                cnt -- ;
                if (!cnt) res = max(res, i - start + 1);
                else if (cnt < 0)
                {
                    start = i + 1;
                    cnt = 0;
                }
            }
        return res;
    }

    int longestValidParentheses(string s) {
        int res = work(s);
        reverse(s.begin(), s.end());
        for (auto &c : s) c ^= 1;
        return max(res, work(s));
    }
};
```
##### LeetCode 155. Min Stack
```C++
class MinStack {
public:
    /** initialize your data structure here. */

    stack<int> stk, stk_min;

    MinStack() {

    }

    void push(int x) {
        stk.push(x);
        if (stk_min.empty()) stk_min.push(x);
        else stk_min.push(min(stk_min.top(), x));
    }

    void pop() {
        stk.pop();
        stk_min.pop();
    }

    int top() {
        return stk.top();
    }

    int getMin() {
        return stk_min.top();
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
##### LeetCode 42. Trapping Rain Water
```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int res = 0;
        stack<int> stk;
        for (int i = 0; i < height.size(); i ++ )
        {
            int last = 0;
            while (stk.size() && height[stk.top()] <= height[i])
            {
                int t = stk.top(); stk.pop();
                res += (i - t - 1) * (height[t] - last);
                last = height[t];
            }
            if (stk.size()) res += (i - stk.top() - 1) * (height[i] - last);
            stk.push(i);
        }
        return res;
    }
};
```
##### LeetCode 84. Largest Rectangle in Histogram
```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n), right(n);

        stack<int> stk;
        for (int i = 0; i < n; i ++ )
        {
            while (stk.size() && heights[stk.top()] >= heights[i]) stk.pop();
            if (stk.empty()) left[i] = -1;
            else left[i] = stk.top();
            stk.push(i);
        }

        while (stk.size()) stk.pop();
        for (int i = n - 1; i >= 0; i -- )
        {
            while (stk.size() && heights[stk.top()] >= heights[i]) stk.pop();
            if (stk.empty()) right[i] = n;
            else right[i] = stk.top();
            stk.push(i);
        }

        int res = 0;
        for (int i = 0; i < n; i ++ ) res = max(res, heights[i] * (right[i] - left[i] - 1));
        return res;
    }
};
```
##### LeetCode 239. Sliding Window Maximum
```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        deque<int> q;
        for (int i = 0; i < nums.size(); i ++ )
        {
            if (q.size() && i - k >= q.front()) q.pop_front();
            while (q.size() && nums[q.back()] <= nums[i]) q.pop_back();
            q.push_back(i);
            if (i >= k - 1) res.push_back(nums[q.front()]);
        }
        return res;
    }
};
```
##### LeetCode 918. Maximum Sum Circular Subarray
```C++
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& A) {
        int n = A.size();
        for (int i = 0; i < n; i ++ ) A.push_back(A[i]);
        vector<int> S(n * 2 + 1, 0);
        for (int i = 0; i < n * 2; i ++ ) S[i + 1] = S[i] + A[i];
        deque<int> q;

        int res = INT_MIN;
        q.push_back(0);
        for (int i = 1; i <= n * 2; i ++ )
        {
            if (q.size() && i - n > q.front()) q.pop_front();
            res = max(res, S[i] - S[q.front()]);
            while (q.size() && S[q.back()] >= S[i]) q.pop_back();
            q.push_back(i);
        }

        return res;
    }
};
```
### Week7 基本数据结构

##### LeetCode 1. Two Sum
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hash;
        for (int i = 0; i < nums.size(); i ++ )
        {
            if (hash.count(target - nums[i])) return {hash[target - nums[i]], i};
            hash[nums[i]] = i;
        }
        return {-1, -1};
    }
};
```
##### LeetCode 187. Repeated DNA Sequences
```C++
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> res;
        unordered_map<string, int> hash;
        for (int i = 0; i + 10 <= s.size(); i ++ )
        {
            string t = s.substr(i, 10);
            if (hash[t] == 1) res.push_back(t);
            hash[t] ++ ;
        }
        return res;
    }
};
```
##### LeetCode 706. Design HashMap
```C++
class MyHashMap {
public:

    int N = 20011;
    vector<list<pair<int, int>>> hash;

    /** Initialize your data structure here. */
    MyHashMap() {
        hash = vector<list<pair<int, int>>>(N);
    }

    list<pair<int, int>>::iterator find(int key)
    {
        int t = key % N;
        for (auto it = hash[t].begin(); it != hash[t].end(); it ++ )
            if (it->first == key)
                return it;
        return hash[t].end();
    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        auto it = find(key);
        int t = key % N;
        if (it == hash[t].end()) hash[t].push_back({key, value});
        else it->second = value;
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        auto it = find(key);
        int t = key % N;
        if (it == hash[t].end()) return -1;
        return it->second;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        auto it = find(key);
        int t = key % N;
        if (it != hash[t].end()) hash[t].erase(it);
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
##### LeetCode 652. Find Duplicate Subtrees
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

    int cnt = 0;
    unordered_map<string, int> hash;
    unordered_map<int, int> tree;
    vector<TreeNode*> ans;

    string dfs(TreeNode* root)
    {
        if (!root) return to_string(hash["#"]);
        auto left = dfs(root->left);
        auto right = dfs(root->right);
        string t = to_string(root->val) + ',' + left + ',' + right;
        if (!hash.count(t)) hash[t] = ++ cnt;

        if (tree[hash[t]] == 1) ans.push_back(root);
        tree[hash[t]] ++ ;
        return to_string(hash[t]);
    }

    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        hash["#"] = ++ cnt;
        dfs(root);
        return ans;
    }
};
```
##### LeetCode 560. Subarray Sum Equals K
```C++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        hash[0] = 1;

        int res = 0;
        for (int i = 0, sum = 0; i < nums.size(); i ++ )
        {
            sum += nums[i];
            res += hash[sum - k];
            hash[sum] ++ ;
        }

        return res;
    }
};
```
##### LeetCode 547. Friend Circles
```C++
class Solution {
public:

    vector<int> p;

    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int findCircleNum(vector<vector<int>>& M) {
        int n = M.size();
        for (int i = 0; i < n; i ++ ) p.push_back(i);

        int res = n;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < i; j ++ )
            {
                if (!M[i][j]) continue;
                if (find(i) != find(j))
                {
                    res -- ;
                    p[find(i)] = find(j);
                }
            }

        return res;
    }
};
```
##### LeetCode 684. Redundant Connection
```C++
class Solution {
public:

    vector<int> p;

    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();

        for (int i = 0; i <= n; i ++ ) p.push_back(i);

        for (auto e : edges)
        {
            int a = e[0], b = e[1];
            if (find(a) == find(b)) return {a, b};
            p[find(a)] = find(b);
        }

        return {-1, -1};
    }
};
```


##### LeetCode 692. Top K Frequent Words
```C++
class Solution {
public:

    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> hash;

        typedef pair<int, string> PIS;
        priority_queue<PIS> heap;
        for (auto word : words) hash[word] ++ ;

        for (auto item : hash)
        {
            PIS t(-item.second, item.first);
            if (heap.size() == k && t < heap.top()) heap.pop();
            if (heap.size() < k) heap.push(t);
        }

        vector<string> res(k);
        for (int i = k - 1; i >= 0; i -- )
        {
            res[i] = heap.top().second;
            heap.pop();
        }

        return res;
    }
};class Solution {
public:

    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> hash;

        typedef pair<int, string> PIS;
        priority_queue<PIS> heap;
        for (auto word : words) hash[word] ++ ;

        for (auto item : hash)
        {
            PIS t(-item.second, item.first);
            if (heap.size() == k && t < heap.top()) heap.pop();
            if (heap.size() < k) heap.push(t);
        }

        vector<string> res(k);
        for (int i = k - 1; i >= 0; i -- )
        {
            res[i] = heap.top().second;
            heap.pop();
        }

        return res;
    }
};
```
##### LeetCode 295. Find Median from Data Stream
```C++
class MedianFinder {
public:
    /** initialize your data structure here. */

    priority_queue<int> left;
    priority_queue<int, vector<int>, greater<int>> right;

    MedianFinder() {

    }

    void addNum(int num) {
        if (left.empty() || left.top() <= num) right.push(num);
        else
        {
            right.push(left.top());
            left.pop();
            left.push(num);
        }

        if (left.size() + 1 < right.size())
        {
            left.push(right.top());
            right.pop();
        }
    }

    double findMedian() {
        if (left.size() + right.size() & 1) return right.top();
        return (left.top() + right.top()) / 2.0;
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```
##### LeetCode 352. Data Stream as Disjoint Intervals
```C++
class SummaryRanges {
public:
    /** Initialize your data structure here. */

    map<int, int> L, R;

    SummaryRanges() {

    }

    void addNum(int val) {
        if (L.size())
        {
            auto it = L.lower_bound(val);
            if (it != L.end() && it->second <= val) return;
        }

        int left = L.count(val - 1), right = R.count(val + 1);
        if (left && right)
        {
            R[L[val - 1]] = R[val + 1];
            L[R[val + 1]] = L[val - 1];
            L.erase(val - 1), R.erase(val + 1);
        }
        else if (left)
        {
            R[L[val - 1]] = val;
            L[val] = L[val - 1];
            L.erase(val - 1);
        }
        else if (right)
        {
            R[val] = R[val + 1];
            L[R[val + 1]] = val;
            R.erase(val + 1);
        }
        else
        {
            L[val] = R[val] = val;
        }
    }

    vector<vector<int>> getIntervals() {
        vector<vector<int>> res;
        for (auto item : R) res.push_back({item.first, item.second});
        return res;
    }
};

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges* obj = new SummaryRanges();
 * obj->addNum(val);
 * vector<vector<int>> param_2 = obj->getIntervals();
 */
```
##### LeetCode 53. Maximum Subarray
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = nums[0], sum = nums[0];
        for (int i = 1; i < nums.size(); i ++ )
        {
            sum = max(0, sum);
            sum += nums[i];
            res = max(res, sum);
        }
        return res;
    }
};
```
### Week8 动态规划

##### LeetCode 120. Triangle
```C++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<int> f[2];
        f[0] = f[1] = vector<int>(n);
        f[0][0] = triangle[0][0];
        for (int i = 1; i < n; i ++ )
            for (int j = 0; j <= i; j ++ )
            {
                auto &a = f[i & 1], &b = f[i - 1 & 1];
                a[j] = INT_MAX;
                if (j) a[j] = b[j - 1] + triangle[i][j];
                if (j <= i - 1) a[j] = min(a[j], b[j] + triangle[i][j]);
            }

        int res = INT_MAX;
        for (int i = 0; i < n; i ++ ) res = min(res, f[n - 1 & 1][i]);
        return res;
    }
};
```
##### LeetCode 63. Unique Paths II
```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& g) {
        int n = g.size(), m = g[0].size();
        vector<vector<long long>> f(n, vector<long long>(m));
        f[0][0] = !g[0][0];
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
            {
                if (g[i][j]) continue;
                if (i) f[i][j] += f[i - 1][j];
                if (j) f[i][j] += f[i][j - 1];
            }

        return f[n - 1][m - 1];
    }
};
```
##### LeetCode 91. Decode Ways
```C++
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        vector<int> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
        {
            if (s[i - 1] != '0') f[i] += f[i - 1];
            if (i >= 2)
            {
                int t = (s[i - 2] - '0') * 10 + s[i - 1] - '0';
                if (t >= 10 && t <= 26) f[i] += f[i - 2];
            }
        }
        return f[n];
    }
};
```
##### LeetCode 198. House Robber
```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n + 1), g(n + 1);
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = max(f[i - 1], g[i - 1]);
            g[i] = nums[i - 1] + f[i - 1];
        }
        return max(f[n], g[n]);
    }
};

```
##### LeetCode 300. Longest Increasing Subsequence
```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n);

        int res = 0;
        for (int i = 0; i < n; i ++ )
        {
            f[i] = 1;
            for (int j = 0; j < i; j ++ )
                if (nums[j] < nums[i])
                    f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }

        return res;
    }
};
```
##### LeetCode 72. Edit Distance
```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size(), m = word2.size();
        vector<vector<int>> f(n + 1, vector<int>(m + 1));
        for (int i = 0; i <= n; i ++ ) f[i][0] = i;
        for (int i = 0; i <= m; i ++ ) f[0][i] = i;

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
            {
                f[i][j] = min(f[i - 1][j], f[i][j - 1]) + 1;
                f[i][j] = min(f[i][j], f[i - 1][j - 1] + (word1[i - 1] != word2[j - 1]));
            }

        return f[n][m];
    }
};
```
##### LeetCode 518. Coin Change 2
```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> f(amount + 1);
        f[0] = 1;
        for (auto c : coins)
            for (int i = c; i <= amount; i ++ )
                f[i] += f[i - c];
        return f[amount];
    }
};
```
##### LeetCode 664. Strange Printer
```C++
class Solution {
public:
    int strangePrinter(string s) {
        int n = s.size();
        vector<vector<int>> f(n + 2, vector<int>(n + 2));
        for (int len = 1; len <= n; len ++ )
            for (int l = 1; l + len - 1 <= n; l ++ )
            {
                int r = l + len - 1;
                f[l][r] = f[l + 1][r] + 1;
                for (int k = l + 1; k <= r; k ++ )
                    if (s[k - 1] == s[l - 1])
                        f[l][r] = min(f[l][r], f[l][k - 1] + f[k + 1][r]);
            }
        return f[1][n];
    }
};
```
##### LeetCode 10. Regular Expression Matching
```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size(), m = p.size();
        vector<vector<bool>> f(n + 1, vector<bool>(m + 1));
        s = " " + s, p = " " + p;
        for (int i = 0; i <= n; i ++ )
            for (int j = 0; j <= m; j ++ )
                if (!i && !j) f[i][j] = true;
                else
                {
                    if (j + 1 <= m && p[j + 1] == '*') continue;
                    if (p[j] != '*')
                    {
                        if (p[j] == '.' || s[i] == p[j])
                            if (i && j) f[i][j] = f[i - 1][j - 1];
                    }
                    else if (j)
                    {
                        if (j >= 2) f[i][j] = f[i][j - 2];
                        if (p[j - 1] == '.' || s[i] == p[j - 1])
                            if (i && j >= 2 && f[i - 1][j]) f[i][j] = f[i - 1][j];
                    }
                }

        return f[n][m];
    }
};
```

# Leetcode提高班

## 第一期

### Week1 链表

#####  LeetCode 19. Remove Nth Node From End of List
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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *first = dummy, *second = dummy;
        for (int i = 0; i < n; i ++ ) first = first->next;
        while (first -> next)
        {
            first = first->next;
            second = second->next;
        }
        second->next = second->next->next;
        return dummy->next;
    }
};

```
#####  LeetCode 83. Remove Duplicates from Sorted List
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
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return NULL;
        ListNode *first, *second;
        first = second = head;
        while (first)
        {
            if (first->val != second->val)
            {
                second->next = first;
                second = first;
            }
            first = first->next;
        }
        second->next = NULL;
        return head;
    }
};
```
#####  LeetCode 206. Reverse Linked List
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
        if (!head) return NULL;
        if (!head->next) return head;
        ListNode *p = reverseList(head->next);
        head->next->next = head;
        head->next = 0;
        return p;
    }
};
```
#####  LeetCode 92. Reverse Linked List II
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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (m == n) return head;
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *b = dummy;
        for (int i = 0; i < m - 1; i ++ ) b = b->next;
        ListNode *a = b;
        b = b->next;
        ListNode *c = b->next;
        for (int i = 0; i < n - m; i ++ )
        {
            ListNode *t = c->next;
            c->next = b;
            b = c, c = t;
        }
        ListNode *mp = a->next;
        ListNode *np = b;
        a->next = np, mp->next = c;
        return dummy->next;
    }
};
```
#####  LeetCode 61. Rotate List
```C++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return NULL;
        int n = 0;
        ListNode *tail;
        for (ListNode *p = head; p; p = p->next)
        {
            n ++ ;
            tail = p;
        }
        k %= n;
        if (!k) return head;
        ListNode *a = head;
        for (int i = 0; i < n - k - 1; i ++ )
        {
            a = a ->next;
        }
        ListNode *b = a -> next;
        tail->next = head;
        a->next = NULL;
        return b;
    }
};
```
#####  LeetCode 143. Reorder List
```C++
class Solution {
public:
    void reorderList(ListNode* head) {
        int n = 0;
        for (ListNode *p = head; p; p = p->next) n ++ ;
        if (n <= 2) return;
        ListNode *mid = head;
        for (int i = 0; i < (n + 1) / 2 - 1; i ++ ) mid = mid->next;
        ListNode *a = mid->next, *b = a->next;
        mid->next = 0, a->next = 0;
        while (b)
        {
            ListNode *t = b->next;
            b->next = a;
            a = b, b = t;
        }
        b = head;
        while (a)
        {
            ListNode *t = a->next;
            a->next = b->next, b->next = a;
            b = b->next->next;
            a = t;
        }
    }
};
```
#####  LeetCode 21. Merge Two Sorted Lists
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *head = new ListNode(0);
        ListNode *p = head;
        while(l1 && l2)
        {
            if (l1->val < l2->val)
            {
                p->next = l1;
                p = l1;
                l1 = l1->next;
            }
            else
            {
                p->next = l2;
                p = l2;
                l2 = l2->next;
            }
        }
        if (!l1) l1 = l2;
        while(l1)
        {
            p->next = l1;
            p = l1;
            l1 = l1->next;
        }
        return head->next;
    }
};
```
#####  LeetCode 160. Intersection of Two Linked Lists
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
        ListNode *p = headA, *q = headB;
        while (p != q)
        {
            if (!p) p = headB;
            else p = p->next;
            if (!q) q = headA;
            else q = q->next;
        }
        return p;
    }
};

```
#####  LeetCode 141. Linked List Cycle
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
    bool hasCycle(ListNode *head) {
        if (!head || !head->next) return 0;
        ListNode *first = head, *second = first->next;

        while (first && second)
        {
            if (first == second) return true;
            first = first->next;
            second = second->next;
            if (second) second = second->next;
        }

        return false;
    }
};
```

#####  LeetCode 147. Insertion Sort List
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
    ListNode* insertionSortList(ListNode* head) {
        ListNode *dummy = new ListNode(-1);
        for (ListNode *p = head, *q; p; p = q)
        {
            q = p->next;
            ListNode *t = dummy;
            while (t->next && t->next->val <= p->val)
            {
                t = t->next;
            }
            p->next = t->next, t->next = p;
        }
        return dummy->next;
    }
};
```
#####  LeetCode 138. Copy List with Random Pointer
```C++
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if (!head) return 0;
        unordered_map<RandomListNode*, RandomListNode*> hash;
        RandomListNode *root = new RandomListNode(head->label);
        hash[head] = root;
        while (head->next)
        {
            if (hash.count(head->next) == 0)
                hash[head->next] = new RandomListNode(head->next->label);
            hash[head]->next = hash[head->next];

            if (head->random && hash.count(head->random) == 0)
                hash[head->random] = new RandomListNode(head->random->label);
            hash[head]->random = hash[head->random];

            head = head->next;
        }

        if (head->random && hash.count(head->random) == 0)
            hash[head->random] = new RandomListNode(head->random->label);
        hash[head]->random = hash[head->random];

        return root;
    }
};
```
#####  LeetCode 142. Linked List Cycle II
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
    ListNode *detectCycle(ListNode *head) {
        if (!head) return NULL;
        ListNode *first = head, *second = head;
        if (!head->next) return NULL;
        do
        {
            first = first->next;
            second = second->next;
            if (!second) return NULL;
            second = second->next;
            if (!second) return NULL;
        }while (first != second);
        first = head;
        while (first != second)
        {
            first = first->next;
            second = second->next;
        }
        return first;
    }
};
```
#####  LeetCode 148. Sort List
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
    ListNode* sortList(ListNode* head) {
        int n = 0;
        for (ListNode *p = head; p; p = p->next) n ++ ;

        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        for (int i = 1; i < n; i *= 2)
        {
            ListNode *begin = dummy;
            for (int j = 0; j + i < n; j += i * 2)
            {
                ListNode *first = begin->next, *second = first;
                for (int k = 0; k < i; k ++ )
                    second = second->next;
                int f = 0, s = 0;
                while (f < i && s < i && second)
                    if (first->val < second->val)
                    {
                        begin = begin->next = first;
                        first = first->next;
                        f ++ ;
                    }
                    else
                    {
                        begin = begin->next = second;
                        second = second->next;
                        s ++ ;
                    }

                while (f < i)
                {
                    begin = begin->next = first;
                    first = first->next;
                    f ++ ;
                }
                while (s < i && second)
                {
                    begin = begin->next = second;
                    second = second->next;
                    s ++ ;
                }

                begin->next = second;
            }
        }

        return dummy->next;
    }
};
```
#####  LeetCode 146. LRU Cache
```C++
class LRUCache {
public:
    struct Node
    {
        int val, key;
        Node *left, *right;
        Node() : key(0), val(0), left(NULL), right(NULL) {}
    };
    Node *hu, *tu;
    Node *hr, *tr;
    int n;
    unordered_map<int, Node*> hash;

    void delete_node(Node *p)
    {
        p->left->right = p->right, p->right->left = p->left;
    }

    void insert_node(Node *h, Node *p)
    {
        p->right = h->right, h->right = p;
        p->left = h, p->right->left = p;
    }

    LRUCache(int capacity) {
        n = capacity;
        hu = new Node(), tu = new Node();
        hr = new Node(), tr = new Node();
        hu->right = tu, tu->left = hu;
        hr->right = tr, tr->left = hr;

        for (int i = 0; i < n; i ++ )
        {
            Node *p = new Node();
            insert_node(hr, p);
        }
    }

    int get(int key) {
        if (hash[key])
        {
            Node *p = hash[key];
            delete_node(p);
            insert_node(tu->left, p);
            return p->val;
        }
        return -1;
    }

    void put(int key, int value) {
        if (hash[key])
        {
            Node *p = hash[key];
            delete_node(p);
            insert_node(tu->left, p);
            p->val = value;
            return;
        }

        if (!n)
        {
            n ++ ;
            Node *p = hu->right;
            hash[p->key] = 0;
            delete_node(p);
            insert_node(hr, p);
        }

        n -- ;
        Node *p = hr->right;
        p->key = key, p->val = value, hash[key] = p;
        delete_node(p);
        insert_node(tu->left, p);
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

### Week2 DFS+回溯专题

#####  LeetCode 46. Permutations
```C++
class Solution {
public:
    vector<vector<int>> ans;
    vector<bool> st;
    vector<int> path;

    vector<vector<int>> permute(vector<int>& nums) {

        for (int i = 0; i < nums.size(); i ++ ) st.push_back(false);
        dfs(nums, 0);
        return ans;
    }

    void dfs(vector<int> &nums, int u)
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return ;
        }

        for (int i = 0; i < nums.size(); i ++ )
            if (!st[i])
            {
                st[i] = true;
                path.push_back(nums[i]);
                dfs(nums, u + 1);
                st[i] = false;
                path.pop_back();
            }
    }
};
```
#####  LeetCode 40. Combination Sum II
```C++
class Solution {
public:
    vector<vector<int>> ans;

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int>path;
        dfs(path, candidates, 0, target);
        return ans;
    }

    void dfs(vector<int>&path, vector<int>&cands, int u, int sum)
    {
        if (!sum)
        {
            ans.push_back(path);
            return;
        }
        if (u == cands.size()) return;

        int k = u + 1;
        while (k < cands.size() && cands[k] == cands[u]) k ++ ;

        dfs(path, cands, k, sum);
        int value = cands[u];
        for (int i = value, j = u; i <= sum && j < k; i += value, j ++ )
        {
            path.push_back(value);
            dfs(path, cands, k, sum - i);
        }
        while (!path.empty() && path.back() == value) path.erase(path.begin() + path.size() - 1, path.end());
    }
};
```
#####  LeetCode 47. Permutations II
```C++
class Solution {
public:
    vector<bool> st;
    vector<int> path;
    vector<vector<int>> ans;

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        st = vector<bool>(nums.size(), false);
        path = vector<int>(nums.size());
        dfs(nums, 0, 0);
        return ans;
    }

    void dfs(vector<int>& nums, int u, int start)
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i < nums.size(); i ++ )
            if (!st[i])
            {
                st[i] = true;
                path[i] = nums[u];
                if (nums[u + 1] != nums[u]) dfs(nums, u + 1, 0);
                else dfs(nums, u + 1, i + 1);
                st[i] = false;
            }
    }
};
```

#####  LeetCode 78. Subsets
```C++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> res;
        int n = nums.size();
        for (int i = 0; i < (1 << n); i ++ )
        {
            vector<int> temp;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                    temp.push_back(nums[j]);
            res.push_back(temp);
        }
        return res;
    }
};
```


#####  LeetCode 90. Subsets II
```C++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        dfs(0, nums);
        return ans;
    }

    void dfs(int u, vector<int>&nums)
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return;
        }
        int k = u;
        while (k < nums.size() && nums[k] == nums[u]) k ++ ;
        dfs(k, nums);
        for (int i = u; i < k; i ++ )
        {
            path.push_back(nums[i]);
            dfs(k, nums);
        }
        path.erase(path.end() - (k - u), path.end());
    }
};
```
#####  LeetCode 39. Combination Sum
```C++
class Solution {
public:
    vector<vector<int>> ans;

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int>path;
        dfs(path, candidates, 0, target);
        return ans;
    }

    void dfs(vector<int>&path, vector<int>&cands, int u, int sum)
    {
        if (!sum)
        {
            ans.push_back(path);
            return;
        }
        if (u == cands.size()) return;

        dfs(path, cands, u + 1, sum);
        int value = cands[u];
        for (int i = value; i <= sum; i += value)
        {
            path.push_back(value);
            dfs(path, cands, u + 1, sum - i);
        }
        while (!path.empty() && path.back() == value) path.erase(path.begin() + path.size() - 1, path.end());
    }
};
```
#####  LeetCode 216. Combination Sum III
```C++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(k, n, 1);
        return ans;
    }

    void dfs(int k, int n, int start)
    {
        if (!k)
        {
            if (!n) ans.push_back(path);
            return;
        }

        for (int i = start; i <= 9; i ++ )
            if (n >= i)
            {
                path.push_back(i);
                dfs(k - 1, n - i, i + 1);
                path.pop_back();
            }
    }
};
```
#####  LeetCode 22. Generate Parentheses
```C++
class Solution {
public:
    vector<string> ans;

    void dfs(string path, int s, int u, int n)
    {
        if (u == n)
        {
            if (!s)
                ans.push_back(path);
            return;
        }
        if (s)
        {
            dfs(path + ')', s - 1, u + 1, n);
        }
        dfs(path + '(', s + 1, u + 1, n);
    }

    vector<string> generateParenthesis(int n) {
        dfs("", 0, 0, n * 2);
        return ans;
    }
};
```
#####  LeetCode 52. N-Queens II
```C++
class Solution {
public:
    int ans;
    vector<bool> row, col, diag, anti_diag;

    int totalNQueens(int n) {
        row = col = vector<bool>(n, false);
        diag = anti_diag = vector<bool>(2 * n, false);
        ans = 0;
        dfs(0, 0, 0, n);
        return ans;
    }

    void dfs(int x, int y, int s, int n)
    {
        if (y == n) x ++ , y = 0;
        if (x == n)
        {
            if (s == n) ++ ans;
            return ;
        }

        dfs(x, y + 1, s, n);
        if (!row[x] && !col[y] && !diag[x + y] && !anti_diag[n - 1 - x + y])
        {
            row[x] = col[y] = diag[x + y] = anti_diag[n - 1 - x + y] = true;
            dfs(x, y + 1, s + 1, n);
            row[x] = col[y] = diag[x + y] = anti_diag[n - 1 - x + y] = false;
        }
    }
};
```
#####  LeetCode 37. Sudoku Solver
```C++
class Solution {
public:
    bool row[9][9], col[9][9], st[3][3][9];
    void solveSudoku(vector<vector<char>>& board) {
        memset(row, 0, sizeof row);
        memset(col, 0, sizeof col);
        memset(st, 0, sizeof st);

        for (int i = 0; i < 9; i ++ )
            for (int j = 0; j < 9; j ++ )
            {
                char c = board[i][j];
                if (c != '.')
                {
                    int t = c - '1';
                    row[i][t] = col[j][t] = st[i / 3][j / 3][t] = true;
                }
            }

        cout << dfs(board, 0, 0);
    }

    bool dfs(vector<vector<char>>& board, int r, int c)
    {
        if (c == 9) r ++, c = 0;
        if (r == 9)
        {
            return true;
        }
        if (board[r][c] != '.') return dfs(board, r, c + 1);

        for (int i = 0; i < 9; i ++ )
        {
            if (!row[r][i] && !col[c][i] && !st[r / 3][c / 3][i])
            {
                board[r][c] = i + '1';
                row[r][i] = col[c][i] = st[r / 3][c / 3][i] = true;
                if (dfs(board, r, c + 1)) return true;
                row[r][i] = col[c][i] = st[r / 3][c / 3][i] = false;
            }
        }

        board[r][c] = '.';
        return false;
    }
};
```
#####  LeetCode 282. Expression Add Operators
```C++
class Solution {
public:
    long long calculate(string s)
    {
        stack<char> op;
        stack<long long> num;
        s += "+0";
        for (int i = 0; i < s.size(); i ++ )
        {
            if (s[i] == ' ') continue;
            if (s[i] == '*' || s[i] == '/' || s[i] == '+' || s[i] == '-') op.push(s[i]);
            else
            {
                int j = i;
                long long number = 0;
                while (j < s.size() && s[j] >= '0' && s[j] <= '9') number = number * 10 + s[j] - '0', j ++ ;
                num.push(number);
                i = j - 1;
                if (!op.empty())
                {
                    if (op.top() == '*' || op.top() == '/')
                    {
                        long long y = num.top();
                        num.pop();
                        long long x = num.top();
                        num.pop();
                        if (op.top() == '*') num.push(x * y);
                        else num.push(x / y);
                        op.pop();
                    }
                    else if (op.size() >= 2)
                    {
                        long long z = num.top(); num.pop();
                        long long y = num.top(); num.pop();
                        long long x = num.top(); num.pop();
                        char op2 = op.top(); op.pop();
                        char op1 = op.top(); op.pop();
                        if (op1 == '+') num.push(x + y), num.push(z);
                        else num.push(x - y), num.push(z);
                        op.push(op2);
                    }
                }
            }
        }
        num.pop();
        return num.top();
    }

    vector<string> ans;

    vector<string> addOperators(string num, int target) {
        if (num.empty()) return ans;
        string path;
        path += num[0];
        dfs(num, 1, path, target);
        return ans;
    }

    void dfs(string &num, int u, string path, long long target)
    {
        if (u == num.size())
        {
            if (calculate(path) == target)
                ans.push_back(path);
            return;
        }

        int k = path.size() - 1;
        while (k >= 0 && path[k] >= '0' && path[k] <= '9') k -- ;
        if (k > 0)
        {
            long long left = calculate(path.substr(0, k)), right = atoi(num.substr(u - (path.size() - 1 - k)).c_str());
            if (abs(left * right) < abs(target) && abs(left + right) < abs(target) && abs(left - right) < abs(target))return;
        }

        char op[] = {'+', '-', '*'};
        for (int i = 0; i < 3; i ++ )
            dfs(num, u + 1, path + op[i] + num[u], target);
        char first;
        for (int i = path.size() - 1; i >= 0; i -- )
            if (path[i] >= '0' && path[i] <= '9')
                first = path[i];
            else break;
        if (first != '0')
            dfs(num, u + 1, path + num[u], target);
    }
};
```
#####  LeetCode 301. Remove Invalid Parentheses
```C++
class Solution {
public:
    vector<string> ans;
    unordered_set<string> S;

    vector<string> removeInvalidParentheses(string s) {
        dfs(s, 0, 0, false);
        return ans;
    }

    void dfs(string &s, int u, int ps, bool is_r)
    {
        if (u == s.size())
        {
            if (!is_r)
            {
                reverse(s.begin(), s.end());
                for (int i = 0; i < s.size(); i++)
                    if (s[i] == '(')
                        s[i] = ')';
                    else if (s[i] == ')')
                        s[i] = '(';
                dfs(s, 0, 0, true);
                reverse(s.begin(), s.end());
                for (int i = 0; i < s.size(); i++)
                    if (s[i] == '(')
                        s[i] = ')';
                    else if (s[i] == ')')
                        s[i] = '(';
            }
            else
            {
                string temp;
                for (int i = s.size() - 1; i >= 0; i--)
                    if (s[i] == '(') temp += ')';
                    else if (s[i] == ')') temp += '(';
                    else if (s[i] != '-') temp += s[i];
                if (!S.count(temp))
                {
                    ans.push_back(temp);
                    S.insert(temp);
                }
            }
            return;
        }
        if (s[u] != ')') dfs(s, u + 1, ps + (s[u] == '('), is_r);
        else
        {
            int i = u;
            while (i < s.size() && s[i] == ')') i++;
            ps -= i - u;
            if (ps >= 0) dfs(s, i, ps, is_r);
            else wipeoff(s, 0, i, -ps, is_r);
        }
    }

    void wipeoff(string &s, int start, int u, int ps, bool is_r)
    {
        if (start == u)
        {
            if (!ps) dfs(s, u, ps, is_r);
            return;
        }
        if (s[start] != ')') wipeoff(s, start + 1, u, ps, is_r);
        else
        {
            int k = start;
            while (k < u && s[k] == ')') k++;
            wipeoff(s, k, u, ps, is_r);
            for (int i = start, j = ps - 1; i < k && j >= 0; i++, j--)
            {
                s[i] = '-';
                wipeoff(s, k, u, j, is_r);
            }
            for (int i = start, j = ps - 1; i < k && j >= 0; i++, j--) s[i] = ')';
        }
    }
};
```
#####  LeetCode 473. Matchsticks to Square
```C++
bool cmp(int x,int y){
    return x>y;
}
class Solution {
public:

    int side;
    vector<bool> st;

    bool makesquare(vector<int>& nums)
    {
        if (nums.size() == 0) return false;
        int sum = 0;
        for (int i = 0; i < nums.size(); i ++ ) sum += nums[i];
        side = sum / 4;
        if (sum % 4) return false;
        st = vector<bool>(nums.size());
        sort(nums.begin(), nums.end());
        reverse(nums.begin(), nums.end());
        return dfs(nums, side, 0, 0);
    }

    bool dfs(vector<int> &nums, int target, int count, int start)
    {
        if (!target)
        {
            count ++ ;
            if (count == 4) return true;
            return dfs(nums, side, count, 0);
        }
        for (int i = start; i < nums.size(); i ++ )
            if (!st[i] && target >= nums[i])
            {
                st[i] = true;
                if (dfs(nums, target - nums[i], count, i + 1)) return true;
                st[i] = false;
            }
        return false;
    }
};
```
#####  LeetCode 17. Letter Combinations of a Phone Number
```C++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return vector<string>();
        string chs[] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> res;
        res.push_back("");
        for (int i = 0; i < digits.size(); i ++ )
        {
            vector<string> newRes;
            int number = digits[i] - '0';
            for (int j = 0; j < res.size(); j ++ )
                for (int k = 0; k < chs[number].size(); k ++ )
                    newRes.push_back(res[j] + chs[number][k]);
            res = newRes;
        }
        return res;
    }
};
```

### Week3 树

#####  LeetCode 101. Symmetric Tree
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
#####  LeetCode 104. Maximum Depth of Binary Tree
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
        return root ? max(maxDepth(root->left), maxDepth(root->right)) + 1 : 0;
    }
};
```
#####  LeetCode 145. Binary Tree Postorder Traversal
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
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        TreeNode *dummy = new TreeNode(-1);
        dummy->left = root;
        root = dummy;
        while (root)
        {
            if (!root->left) root = root->right;
            else
            {
                TreeNode *pre = root->left;
                while (pre->right && pre->right != root) pre = pre->right;
                if (pre->right)
                {
                    pre->right = 0;
                    int top = res.size();
                    for (TreeNode *p = root->left; p; p = p->right)
                        res.push_back(p->val);
                    reverse(res.begin() + top, res.end());
                    root = root->right;
                }
                else
                {
                    pre->right = root;
                    root = root->left;
                }
            }
        }
        return res;
    }
};
```
#####  LeetCode 105. Construct Binary Tree from Preorder and Inorder Traversal
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

#####  LeetCode 331. Verify Preorder Serialization of a Binary Tree
```C++
class Solution {
public:
    bool ans = true;
    bool isValidSerialization(string preorder) {
        preorder += ',';
        int u = 0;
        dfs(preorder, u);
        return ans && u == preorder.size();
    }

    void dfs(string &preorder, int &u)
    {
        if (u == preorder.size())
        {
            ans = false;
            return;
        }
        if (preorder[u] == '#')
        {
            u += 2;
            return;
        }
        while (preorder[u] != ',') u ++ ; u ++ ;
        dfs(preorder, u);
        if (u == preorder.size())
        {
            ans = false;
            return;
        }
        dfs(preorder, u);
    }
};
```
#####  LeetCode 102. Binary Tree Level Order Traversal
```C++
class Solution {
public:
    bool ans = true;
    bool isValidSerialization(string preorder) {
        preorder += ',';
        int u = 0;
        dfs(preorder, u);
        return ans && u == preorder.size();
    }

    void dfs(string &preorder, int &u)
    {
        if (u == preorder.size())
        {
            ans = false;
            return;
        }
        if (preorder[u] == '#')
        {
            u += 2;
            return;
        }
        while (preorder[u] != ',') u ++ ; u ++ ;
        dfs(preorder, u);
        if (u == preorder.size())
        {
            ans = false;
            return;
        }
        dfs(preorder, u);
    }
};
```
#####  LeetCode 236. Lowest Common Ancestor of a Binary Tree
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
        if (!root || root == p || root == q) return root;
        TreeNode *left = lowestCommonAncestor(root->left, p, q);
        TreeNode *right = lowestCommonAncestor(root->right, p, q);
        return !left ? right : !right ? left : root;
    }
};
```
#####  LeetCode 124. Binary Tree Maximum Path Sum
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
    int ans;
    int maxPathSum(TreeNode* root) {
        ans = INT_MIN;
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root)
    {
        if (!root) return 0;
        int left = max(0, dfs(root->left));
        int right = max(0, dfs(root->right));
        ans = max(ans, left + root->val + right);
        return root->val + max(left, right);
    }
};
```
#####  LeetCode 87. Scramble String
```C++
class Solution {
public:
    bool isScramble(string s1, string s2) {
        if (s1 == s2) return true;
        string ss1 = s1, ss2 = s2;
        sort(ss1.begin(), ss1.end()), sort(ss2.begin(), ss2.end());
        if (ss1 != ss2) return false;
        for (int i = 1; i < s1.size(); i ++ )
        {
            if (isScramble(s1.substr(0, i), s2.substr(0, i)) && isScramble(s1.substr(i), s2.substr(i))) return true;
            if (isScramble(s1.substr(0, i), s2.substr(s2.size() - i)) && isScramble(s1.substr(i), s2.substr(0, s2.size() - i))) return true;
        }
        return false;
    }
};
```
#####  LeetCode 117. Populating Next Right Pointers in Each Node II
```C++
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        while (root)
        {
            TreeLinkNode *dummy = new TreeLinkNode(0), *tail = dummy;
            while (root)
            {
                if (root->left)
                {
                    tail->next = root->left;
                    tail = tail->next;
                }
                if (root->right)
                {
                    tail->next = root->right;
                    tail = tail->next;
                }
                root = root->next;
            }
            tail->next = 0;
            root = dummy->next;
        }
    }
};
```
#####  LeetCode 99. Recover Binary Search Tree
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
    void recoverTree(TreeNode* root) {
        TreeNode *first = NULL, *second, *prep = NULL;
        while (root)
        {
            if (!root->left)
            {
                if (prep && prep->val > root->val)
                {
                    if (!first) first = prep, second = root;
                    else second = root;
                }
                prep = root;
                root = root->right;
            }
            else
            {
                TreeNode *p = root->left;
                while (p->right && p->right != root) p = p->right;
                if (!p->right)
                {
                    p->right = root;
                    root = root->left;
                }
                else
                {
                    p->right = NULL;
                    if (prep && prep->val > root->val)
                    {
                        if (!first) first = prep, second = root;
                        else second = root;
                    }
                    prep = root;
                    root = root->right;
                }
            }
        }
        swap(first->val, second->val);
    }
};
```
#####  LeetCode 337. House Robber III
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
    unordered_map<TreeNode*, unordered_map<int, int>>f;

    int rob(TreeNode* root) {
        dfs(root);
        return max(f[root][0], f[root][1]);
    }

    void dfs(TreeNode *root)
    {
        if (!root) return;
        dfs(root->left);
        dfs(root->right);
        f[root][1] = root->val + f[root->left][0] + f[root->right][0];
        f[root][0] = max(f[root->left][0], f[root->left][1]) + max(f[root->right][0], f[root->right][1]);
    }
};
```
#####  LeetCode 653. Two Sum IV - Input is a BST
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
    vector<int> inorder;

    bool findTarget(TreeNode* root, int k) {
        dfs(root);
        for (int i = 0, j = inorder.size() - 1; i < j; i ++ )
        {
            while (i < j && inorder[i] + inorder[j] > k) j -- ;
            if (i < j && inorder[i] + inorder[j] == k) return true;
        }
        return false;
    }

    void dfs(TreeNode *root)
    {
        if (!root) return;
        dfs(root->left);
        inorder.push_back(root->val);
        dfs(root->right);
    }
};
```
#####  LeetCode 543. Diameter of Binary Tree
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
    int ans = 1;
    int diameterOfBinaryTree(TreeNode* root) {
        dfs(root);
        return ans - 1;
    }
    int dfs(TreeNode *u)
    {
        if (!u) return 0;
        int l = dfs(u->left);
        int r = dfs(u->right);
        ans = max(ans, l + r + 1);
        return max(l, r) + 1;
    }
};
```

### Week4 动态规划(1/2)

#####  LeetCode 53. Maximum Subarray
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = INT_MIN;
        for (int i = 0, s = 0; i < nums.size(); i ++ )
        {
            s += nums[i];
            res = max(res, s);
            if (s < 0) s = 0;
        }
        return res;
    }
};
```
#####  LeetCode 300. Longest Increasing Subsequence
```C++
/*

q[i] 表示长度为i的最长上升子序列，结尾最小是多少。

q[i] 是严格单调递增的。

对于当前数x，在q里二分出第一个大于等于x的q[i]， q[i] = x;

*/

class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int tt = 0;
        vector<int> q(nums.size());
        for (auto x : nums)
        {
            if (!tt || x > q[tt - 1]) q[tt++] = x;
            else
            {
                int l = 0, r = tt - 1;
                while (l < r)
                {
                    int mid = l + r >> 1;
                    if (q[mid] >= x) r = mid;
                    else l = mid + 1;
                }
                q[r] = x;
            }
        }
        return tt;
    }
};
```
#####  LeetCode 72. Edit Distance
```C++
/*

状态表示：f[i][j] 让word1的前i个字母和word2的前j个字母匹配，所需的最少操作次数；

状态转移
    if word1[i] == word2[j]: 
        f[i][j] = f[i - 1][j - 1]
    else:
        f[i][j] = min(f[i][[j], f[i - 1][j - 1] + 1);
        f[i][j] = min(f[i][j], f[i][j - 1] + 1);
        f[i][j] = min(f[i][j], f[i - 1][j] + 1);

边界问题：
    f[i][0] = i;
    f[0][j] = j;
*/

class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size(), m = word2.size();
        vector<vector<int>> f(n + 1, vector<int>(m + 1, 0));
        for (int i = 0; i <= n; i ++ ) f[i][0] = i;
        for (int i = 0; i <= m; i ++ ) f[0][i] = i;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
            {
                f[i][j] = f[i - 1][j - 1] + (word1[i - 1] != word2[j - 1]);
                f[i][j] = min(f[i][j], f[i][j - 1] + 1);
                f[i][j] = min(f[i][j], f[i - 1][j] + 1);
            }
        return f[n][m];
    }
};
```
#####  LeetCode 121. Best Time to Buy and Sell Stock
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0, minv = INT_MAX;
        for (int i = 0; i < prices.size(); i ++ )
        {
            res = max(res, prices[i] - minv);
            minv = min(minv, prices[i]);
        }
        return res;
    }
};
```

#####  LeetCode 122. Best Time to Buy and Sell Stock II
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        for (int i = 0; i + 1 < prices.size(); i ++ )
            if (prices[i] < prices[i + 1])
                res += prices[i + 1] - prices[i];
        return res;
    }
};
```
#####  LeetCode 123. Best Time to Buy and Sell Stock III
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (!n) return 0;
        vector<int> f(n, 0);
        int minv = INT_MAX;
        for (int i = 0; i < n; i ++ )
        {
            if (i) f[i] = f[i - 1];
            if (prices[i] > minv)
                f[i] = max(f[i], prices[i] - minv);
            minv = min(minv, prices[i]);
        }
        int res = f[n - 1];
        int maxv = INT_MIN;
        for (int i = n - 1; i > 0; i -- )
        {
            if (prices[i] < maxv)
                res = max(res, maxv - prices[i] + f[i - 1]);
            maxv = max(maxv, prices[i]);
        }
        return res;
    }
};
```
#####  LeetCode 188. Best Time to Buy and Sell Stock IV
```C++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        if (k >= n / 2)
        {
            int res = 0;
            for (int i = 1; i < n; i ++ ) res += max(0, prices[i] - prices[i - 1]);
            return res;
        }
        int f[k + 1], g[k + 1];
        for (int i = 0; i <= k; i ++ ) f[i] = 0, g[i] = INT_MIN;
        for (int cur : prices)
            for (int i = k; i; i -- )
            {
                f[i] = max(f[i], g[i] + cur);
                g[i] = max(g[i], f[i - 1] - cur);
            }
        return f[k];
    }
};
```
#####  LeetCode 309. Best Time to Buy and Sell Stock with Cooldown
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> f(n + 1, vector<int>(3));
        f[0][0] = INT_MIN, f[0][1] = f[0][2] = 0;
        for (int i = 1; i <= n; i ++ )
        {
            f[i][0] = max(f[i - 1][0], f[i - 1][2] - prices[i - 1]);
            f[i][1] = f[i - 1][0] + prices[i - 1];
            f[i][2] = max(f[i - 1][2], f[i - 1][1]);
        }
        return max(f[n][1], f[n][2]);
    }
};
```
#####  LeetCode 198. House Robber
```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n + 1, 0), g(n + 1, 0);
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        return max(f[n], g[n]);
    }
};
```
#####  LeetCode 213. House Robber II
```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return nums[0];
        vector<int>f(n + 1, 0), g(n + 1, 0);
        for (int i = 2; i <= n; i ++ )
        {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        int res = max(f[n], g[n]);
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        return max(res, g[n]);
    }
};
```

#####  LeetCode 312. Burst Balloons
```C++
class Solution {
public:
    vector<vector<int>>f;
    vector<int> nums;

    int dp(int x, int y)
    {
        if (f[x][y] != -1) return f[x][y];
        f[x][y] = 0;
        for (int i = x + 1; i < y; i ++ )
            f[x][y] = max(f[x][y], dp(x, i) + dp(i, y) + nums[x] * nums[i] * nums[y]);
        return f[x][y];
    }

    int maxCoins(vector<int>& _nums) {
        int n = _nums.size();
        f = vector<vector<int>>(n + 2, vector<int>(n + 2, -1));
        nums = vector<int>(n + 2);
        nums[0] = nums[n + 1] = 1;
        for (int i = 0; i < n; i ++ ) nums[i + 1] = _nums[i];
        return dp(0, n + 1);
    }
};
```
#####  LeetCode 96. Unique Binary Search Trees
```C++
class Solution {
public:
    int numTrees(int n) {
        vector<int>f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = 0;
            for (int j = 1; j <= i; j ++ )
                f[i] += f[j - 1] * f[i - j];
        }
        return f[n];
    }
};
```
#####  LeetCode 140. Word Break II
```C++
class Solution {
public:
    vector<string> ans;
    unordered_map<string, int> dict;

    vector<string> wordBreak(string s, vector<string>& wordDict) {

        for (auto &word : wordDict) dict[word] = 1;
        int n = s.size();
        vector<bool>f(n + 1, true);
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = false;
            for (int j = 0; j < i; j ++ )
                if (dict[s.substr(j, i - j)] && f[j])
                {
                    f[i] = true;
                    break;
                }
        }

        dfs(f, s, "", n);
        return ans;
    }

    void dfs(vector<bool>&f, string &s, string path, int u)
    {
        if (!u)
        {
            ans.push_back(path.substr(0, path.size() - 1));
            return ;
        }

        for (int i = 0; i < u; i ++ )
            if (dict[s.substr(i, u - i)] && f[i])
                dfs(f, s, s.substr(i, u - i) + ' ' + path, i);
    }
};
```
#####  LeetCode 10. Regular Expression Matching
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
#####  LeetCode 214. Shortest Palindrome
```C++
class Solution {
public:
    string shortestPalindrome(string s) {
        string raws = s;
        reverse(s.begin(), s.end());
        s = raws + '#' + s;
        int n = s.size();
        vector<int> next(n + 1, 0);
        for (int i = 2, j = 0; i <= n; i ++ )
        {
            while (j && s[j] != s[i - 1]) j = next[j];
            if (s[j] == s[i - 1]) j ++ ;
            next[i] = j;
        }
        int res = next[n];
        string sup = raws.substr(res);
        reverse(sup.begin(), sup.end());
        return sup + raws;
    }
};
```
#####  LeetCode 30. Substring with Concatenation of All Words
```C++
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> res;
        int n = s.size(), m = words.size(), w = words[0].size();
        unordered_map<string, int> hashmap;
        for (int i = 0; i < m; i++ ) hashmap[words[i]] ++ ;
        for (int i = 0; i < w; i ++ )
        {
            unordered_map<string, int>tdict;
            int sum = 0;
            int l = i;
            for (int j = l; j + w <= n; j += w)
            {
                string word;
                if (j - m * w >= l)
                {
                    word = s.substr(j - m * w, w);
                    if (tdict[word] == hashmap[word]) sum -= hashmap[word];
                    tdict[word] -- ;
                    if (tdict[word] == hashmap[word]) sum += hashmap[word];
                }
                word = s.substr(j, w);
                if (hashmap.count(word) == 0)
                {
                    sum = 0;
                    tdict.clear();
                    l = j + w;
                }
                else
                {
                    if (tdict[word] == hashmap[word]) sum -= hashmap[word];
                    tdict[word] ++ ;
                    if (tdict[word] == hashmap[word]) sum += hashmap[word];
                }

                if (sum == m) res.push_back(j - (m - 1) * w);
            }
        }
        return res;
    }
};
```

### Week5 字符串

#####  LeetCode 38. Count and Say
```C++
class Solution {
public:
    string countAndSay(int n) {
        string res = "1";
        while (-- n)
        {
            string newRes = "";
            for (int i = 0; i < res.size();)
            {
                int j = i;
                while (j < res.size() && res[i] == res[j]) j ++ ;
                newRes += to_string(j - i) + res[i];
                i = j;
            }
            res = newRes;
        }
        return res;
    }
};
```
#####  LeetCode 49. Group Anagrams
```C++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> dict;
        for (auto &str : strs)
        {
            string key = str;
            sort(key.begin(), key.end());
            if (dict.count(key) == 0) dict[key] = vector<string>();
            dict[key].push_back(str);
        }

        vector<vector<string>> res;
        for (auto i = dict.begin(); i != dict.end(); i ++ )
        {
            res.push_back(i -> second);
        }

        return res;
    }
};
```
#####  LeetCode 151. Reverse Words in a String
```C++
class Solution {
public:
    void reverseWords(string &s) {
        int k = 0;
        for (int i = 0; i < s.size();)
        {
            int j = i;
            while (j < s.size() && s[j] == ' ') j ++ ;
            if (j == s.size()) break;
            i = j;
            while (j < s.size() && s[j] != ' ') j ++ ;
            reverse(s.begin() + i, s.begin() + j);
            if (k) s[k ++ ] = ' ';
            while (i < j) s[k ++ ] = s[i ++ ];
        }
        s.erase(s.begin() + k, s.end());
        reverse(s.begin(), s.end());
    }
};
```
#####  LeetCode 165. Compare Version Numbers
```C++
class Solution {
public:
    int compareVersion(string version1, string version2) {
        int i = 0, j = 0;
        while (i < version1.size() || j < version2.size())
        {
            int ki = i, kj = j;
            while (ki < version1.size() && version1[ki] != '.') ki ++ ;
            while (kj < version2.size() && version2[kj] != '.') kj ++ ;
            string si, sj;
            if (i != ki) si = version1.substr(i, ki - i);
            if (j != kj) sj = version2.substr(j, kj - j);
            if (atoi(si.c_str()) < atoi(sj.c_str())) return -1;
            if (atoi(si.c_str()) > atoi(sj.c_str())) return 1;
            if (ki != i) i = ki + 1;
            if (kj != j) j = kj + 1;
        }
        return 0;
    }
};
```
#####  LeetCode 5. Longest Palindromic Substring
```C++
class Solution {
public:
    string longestPalindrome(string s) {
        int res = 0;
        string str;
        for (int i = 0; i < s.size(); i ++ )
        {
            for (int j = 0; i - j >= 0 && i + j < s.size(); j ++ )
                if (s[i - j] == s[i + j])
                {
                    if (j * 2 + 1 > res)
                    {
                        res = j * 2 + 1;
                        str = s.substr(i - j, j * 2 + 1);
                    }
                }
                else break;

            for (int j = i, k = i + 1; j >= 0 && k < s.size(); j -- , k ++ )
                if (s[j] == s[k])
                {
                    if (k - j + 1 > res) 
                    {
                        res = k - j + 1;
                        str = s.substr(j, k - j + 1);
                    }
                }
                else break;
        }
        return str;
    }
};
```
#####  LeetCode 273. Integer to English Words
```C++
class Solution {
public:
    int hundred = 100, thousand = 1000, million = 1000000, billion = 1000000000;
    unordered_map<int, string> numbers;

    string numberToWords(int num) {
        char number20[][30] = {"Zero", "One", "Two", "Three", "Four", "Five", 
                               "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", 
                               "Twelve", "Thirteen", "Fourteen", "Fifteen", 
                               "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
        char number2[][30] = {"Twenty", "Thirty", "Forty", "Fifty", "Sixty", 
                              "Seventy", "Eighty", "Ninety"};
        for (int i = 0; i < 20; i ++ ) numbers[i] = number20[i];
        for (int i = 20, j = 0; i < 100; i += 10, j ++ ) numbers[i] = number2[j];
        numbers[hundred] = "Hundred", numbers[thousand] = "Thousand";
        numbers[million] = "Million", numbers[billion] = "Billion";
        string res;
        for (int k = 1000000000; k >= 100; k /= 1000)
        {
            if (num >= k)
            {
                res += ' ' + get3(num / k) + ' ' + numbers[k];
                num %= k;
            }
        }
        if (num) res += ' ' + get3(num);
        if (res.empty()) res = ' ' + numbers[0];
        return res.substr(1);
    }

    string get3(int num)
    {
        string res;
        if (num >= hundred)
        {
            res += ' ' + numbers[num / hundred] + ' ' + numbers[hundred];
            num %= hundred;
        }
        if (num)
        {
            if (num < 20) res += ' ' + numbers[num];
            else if (num % 10 == 0) res += ' ' + numbers[num];
            else res += ' ' + numbers[num / 10 * 10] + ' ' + numbers[num % 10];
        }
        return res.substr(1);
    }
};
```
#####  LeetCode 6. ZigZag Conversion
```C++
class Solution {
public:
    string convert(string s, int numRows) {
        string res;
        if (numRows == 1) return s;
        for (int j = 0; j < numRows; j ++ )
        {
            if (j == 0 || j == numRows - 1)
            {
                for (int i = j; i < s.size(); i += (numRows - 1) * 2) res += s[i];
            }
            else
            {
                for (int k = j, i = numRows * 2 - 1 - j - 1; i < s.size() || k < s.size(); i += (numRows - 1) * 2, k += (numRows - 1) * 2)
                {
                    if (k < s.size()) res += s[k];
                    if (i < s.size()) res += s[i];
                }
            }
        }
        return res;
    }
};

```
#####  LeetCode 3. Longest Substring Without Repeating Characters
```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
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
#####  LeetCode 166. Fraction to Recurring Decimal
```C++
class Solution {
public:
    string fractionToDecimal(int _n, int _d) {
        long long n = _n, d = _d;
        bool minus = false;
        if (n < 0) minus = !minus, n = -n;
        if (d < 0) minus = !minus, d = -d;
        string res = to_string(n / d);
        n %= d;
        if (!n)
        {
            if (minus && res != "0") return '-' + res;
            return res;
        }

        res += '.';
        unordered_map<long long, int> hash;
        while (n)
        {
            if (hash[n])
            {
                res = res.substr(0, hash[n]) + '(' + res.substr(hash[n]) + ')';
                break;
            }
            else hash[n] = res.size();
            n *= 10;
            res += to_string(n / d);
            n %= d;
        }
        if (minus) res = '-' + res;
        return res;
    }
};
```
#####  LeetCode 208. Implement Trie (Prefix Tree)
```C++
class Trie {
public:
    /** Initialize your data structure here. */
    struct Node
    {
        bool is_end;
        Node *next[26];
        Node()
        {
            is_end = false;
            for (int i = 0; i < 26; i ++ )
                next[i] = 0;
        }
    };

    Node *root;
    Trie() {
        root = new Node();
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        Node *p = root;
        for (char c : word)
        {
            int son = c - 'a';
            if (!p->next[son]) p->next[son] = new Node();
            p = p->next[son];
        }
        p->is_end = true;
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node *p = root;
        for (char c : word)
        {
            if (p->next[c - 'a']) p = p->next[c - 'a'];
            else
                return false;
        }
        return p->is_end;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node *p = root;
        for (char c : prefix)
        {
            if (p->next[c - 'a']) p = p->next[c - 'a'];
            else
                return false;
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * bool param_2 = obj.search(word);
 * bool param_3 = obj.startsWith(prefix);
 */
```
#####  LeetCode 131. Palindrome Partitioning
```C++
class Solution {
public:
    vector<vector<string>> ans;
    vector<string> path;

    vector<vector<string>> partition(string s) {
        dfs("", 0, s);
        return ans;
    }

    bool check(string &now)
    {
        if (now.empty()) return false;
        for (int i = 0, j = now.size() - 1; i < j; i ++, j -- )
            if (now[i] != now[j])
                return false;
        return true;
    }

    void dfs(string now, int u, string &s)
    {
        if (u == s.size())
        {
            if (check(now))
            {
                path.push_back(now);
                ans.push_back(path);
                path.pop_back();
            }
            return;
        }

        if (check(now))
        {
            path.push_back(now);
            dfs("", u, s);
            path.pop_back();
        }

        dfs(now + s[u], u + 1, s);
    }
};
```
#####  LeetCode 227. Basic Calculator II
```C++
class Solution {
public:
    int calculate(string s) {
        stack<char> op;
        stack<int> num;
        s += "+0";
        for (int i = 0; i < s.size(); i ++ )
        {
            if (s[i] == ' ') continue;
            if (s[i] == '*' || s[i] == '/' || s[i] == '+' || s[i] == '-') op.push(s[i]);
            else
            {
                int j = i;
                while (j < s.size() && s[j] >= '0' && s[j] <= '9') j ++ ;
                num.push(atoi(s.substr(i, j - i).c_str()));
                i = j - 1;
                if (!op.empty())
                {
                    if (op.top() == '*' || op.top() == '/')
                    {
                        int y = num.top();
                        num.pop();
                        int x = num.top();
                        num.pop();
                        if (op.top() == '*') num.push(x * y);
                        else num.push(x / y);
                        op.pop();
                    }
                    else if (op.size() >= 2)
                    {
                        int z = num.top(); num.pop();
                        int y = num.top(); num.pop();
                        int x = num.top(); num.pop();
                        char op2 = op.top(); op.pop();
                        char op1 = op.top(); op.pop();
                        if (op1 == '+') num.push(x + y), num.push(z);
                        else num.push(x - y), num.push(z);
                        op.push(op2);
                    }
                }
            }
        }
        num.pop();
        return num.top();
    }
};
```

### Week 6 二分与单调队列/栈

#####  LeetCode 69. Sqrt(x)
```C++
class Solution {
public:
    int mySqrt(int x) {
        int l = 0, r = x;
        while (l < r)
        {
            long long mid = l + 1ll + r >> 1;
            if (mid * mid <= x) l = mid;
            else r = mid - 1;
        }
        return l;
    }
};
```
#####  LeetCode 34. Find First and Last Position of Element in Sorted Array
```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] < target) l = mid + 1;
            else r = mid;
        }
        if (nums.empty() || nums[r] != target)
        {
            vector<int>res;
            res.push_back(-1), res.push_back(-1);
            return res;
        }
        vector<int> res;
        res.push_back(l);

        l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if (nums[mid] > target) r = mid - 1;
            else l = mid;
        }
        res.push_back(l);
        return res;
    }
};
```

#####  LeetCode 74. Search a 2D Matrix
```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;
        int n = matrix.size(), m = matrix[0].size();
        int l = 0, r = n * m - 1;
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if (matrix[mid / m][mid % m] <= target) l = mid;
            else r = mid - 1;
        }
        return matrix[r / m][r % m] == target;
    }
};
```
#####  LeetCode 240. Search a 2D Matrix II
```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty()) return false;
        int row = 0, col = matrix[0].size() - 1;
        while (row < matrix.size() && col >= 0)
        {
            int t = matrix[row][col];
            if (t < target) row ++ ;
            else if (t > target) col -- ;
            else return true;
        }
        return false;
    }
};
```
#####  LeetCode 153. Find Minimum in Rotated Sorted Array
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        if (nums.size() < 5)
        {
            int res = INT_MAX;
            for (int x : nums) res = min(res, x);
            return res;
        }
        int left, right;
        if (nums[0] < nums[1] && nums[1] < nums.back()) return nums[0];
        else
        {
            int l = 0, r = nums.size() - 1;
            while (l < r)
            {
                int mid = (l + r + 1) / 2;
                if (nums[mid] >= nums[0] && nums[mid] > nums.back()) l = mid;
                else r = mid - 1;
            }
            return nums[r + 1];
        }
    }
};
```
#####  LeetCode 162. Find Peak Element
```C++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 1, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if (nums[mid - 1] < nums[mid]) l = mid;
            else r = mid - 1;
        }
        if (nums.size() == 1 || nums[l - 1] > nums[l]) l -- ;
        return l;
    }
};
```
#####  LeetCode 155. Min Stack
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
        if (!stackMin.empty() && stackMin.top() == stackValue.top()) stackMin.pop();
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
#####  LeetCode 496. Next Greater Element I
```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& findNums, vector<int>& nums) {
        stack<int> stk;
        unordered_map<int, int> hash;
        for (int j = nums.size() - 1; j >= 0; j -- )
        {
            while (stk.size() && stk.top() <= nums[j]) stk.pop();
            hash[nums[j]] = stk.size() ? stk.top() : -1;
            stk.push(nums[j]);
        }
        vector<int> res;
        for (auto x : findNums) res.push_back(hash[x]);
        return res;
    }
};
```
#####  LeetCode 42. Trapping Rain Water
```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int res = 0;
        stack<int> sta;
        for (int i = 0; i < height.size(); i ++ )
        {
            int level = 0;
            while (!sta.empty() && height[sta.top()] <= height[i])
            {
                res += (height[sta.top()] - level) * (i - sta.top() - 1);
                level = height[sta.top()];
                sta.pop();
            }
            if (!sta.empty())
                res += (height[i] - level) * (i - sta.top() - 1);
            sta.push(i);
        }
        return res;
    }
```
#####  LeetCode 84. Largest Rectangle in Histogram
```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int res = 0;
        stack<pair<int,int>> sta;
        vector<int> forward(heights.size()), backward(heights.size());
        for (int i = 0; i < heights.size(); i ++ )
        {
            forward[i] = i;
            while (!sta.empty() && sta.top().first >= heights[i])
            {
                forward[i] = sta.top().second;
                sta.pop();
            }
            sta.push(make_pair(heights[i], forward[i]));
        }

        while (!sta.empty()) sta.pop();
        for (int i = heights.size() - 1; i >= 0; i -- )
        {
            backward[i] = i;
            while (!sta.empty() && sta.top().first >= heights[i])
            {
                backward[i] = sta.top().second;
                sta.pop();
            }
            sta.push(make_pair(heights[i], backward[i]));
        }

        for (int i = 0; i < heights.size(); i ++ )
            res = max(res, heights[i] * (backward[i] - forward[i] + 1));
        return res;
    }
};
```
#####  LeetCode 475. Heaters
```C++
class Solution {
public:
    int findRadius(vector<int>& houses, vector<int>& heaters) {
        heaters.push_back(INT_MIN), heaters.push_back(INT_MAX);
        sort(heaters.begin(), heaters.end());
        int res = 0;
        for (auto &x : houses)
        {
            int l = 0, r = heaters.size() - 1;
            while (l < r)
            {
                int mid = l + r >> 1;
                if (heaters[mid] >= x) r = mid;
                else l = mid + 1;
            }
            res = max(res, (int)min(heaters[r] - 0ll - x, x - 0ll - heaters[r - 1]));
        }
        return res;
    }
};
```


#####  LeetCode 4. Median of Two Sorted Arrays
```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int total = nums1.size() + nums2.size();
        if (total % 2 == 0)
        {
            int left = findKthNumber(nums1, 0, nums2, 0, total / 2);
            int right = findKthNumber(nums1, 0, nums2, 0, total / 2 + 1);
            return (left + right) / 2.0;
        }
        else
        {
            return findKthNumber(nums1, 0, nums2, 0, total / 2 + 1);
        }
    }

    int findKthNumber(vector<int> &nums1, int i, vector<int> &nums2, int j, int k)
    {
        if (nums1.size() - i > nums2.size() - j) return findKthNumber(nums2, j, nums1, i, k);
        if (nums1.size() == i) return nums2[j + k - 1];
        if (k == 1) return min(nums1[i], nums2[j]);
        int si = min(i + k / 2, int(nums1.size())), sj = j + k / 2;
        if (nums1[si - 1] > nums2[sj - 1])
        {
            return findKthNumber(nums1, i, nums2, j + k / 2, k - k / 2);
        }
        else
        {
            return findKthNumber(nums1, si, nums2, j, k - (si - i));
        }
    }
};
```
#####  LeetCode 239. Sliding Window Maximum
```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> dq;
        vector<int> res;
        for (int i = 0, j = 0; i < nums.size(); i ++ )
        {
            while (!dq.empty() && nums[dq.front()] <= nums[i]) dq.pop_front();
            dq.push_front(i);
            if (i - j + 1 > k)
            {
                if (dq.back() <= j) dq.pop_back();
                j ++ ;
            }
            if (i - j + 1 == k) res.push_back(nums[dq.back()]);
        }
        return res;
    }
};
```
#####  LeetCode 456. 132 Pattern
```C++
class Solution {
public:
    bool find132pattern(vector<int>& nums) {
        int s3 = INT_MIN;
        stack<int> stk;
        for (int i = nums.size() - 1; i >= 0; i -- )
        {
            if (nums[i] < s3) return true;
            while (stk.size() && stk.top() < nums[i])
            {
                s3 = stk.top();
                stk.pop();
            }
            stk.push(nums[i]);
        }
        return false;
    }
};
```
#####  LeetCode 706. Design HashMap
```C++
// 开放寻址法
class MyHashMap {
public:
    /** Initialize your data structure here. */
    const static int N = 20011;
    int hash_key[N], hash_value[N];

    MyHashMap() {
        memset(hash_key, -1, sizeof hash_key);
    }

    int find(int key, bool is_put)
    {
        int t = key % N;
        while (hash_key[t] != key && hash_key[t] != -1)
        {
            if (is_put && hash_key[t] == -2)
                break;
            if ( ++t == N) t = 0;
        }
        return t;
    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        int t = find(key, true);
        hash_key[t] = key;
        hash_value[t] = value;
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int t = find(key, false);
        if (hash_key[t] == -1) return -1;
        return hash_value[t];
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int t = find(key, false);
        if (hash_key[t] != -1)
            hash_key[t] = -2;
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap obj = new MyHashMap();
 * obj.put(key,value);
 * int param_2 = obj.get(key);
 * obj.remove(key);
 */

```

### Week 7 哈希表

#####  LeetCode 1. Two Sum
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target)
    {
        vector<int> res;
        unordered_map<int,int> hash;
        for (int i = 0; i < nums.size(); i ++ )
        {
            int another = target - nums[i];
            if (hash.count(another))
            {
                res = vector<int>({hash[another], i});
                break;
            }
            hash[nums[i]] = i;
        }
        return res;
    }
};
```
#####  LeetCode 454. 4Sum II
```C++
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> hash;
        for (auto a : A)
            for (auto b : B)
                hash[a + b] ++ ;
        int res = 0;
        for (auto c : C)
            for (auto d : D)
                res += hash[-c - d];
        return res;
    }
};
```
#####  LeetCode 560. Subarray Sum Equals K
```C++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        hash[0] = 1;
        int res = 0, s = 0;
        for (auto x: nums)
        {
            s += x;
            res += hash[s - k];
            hash[s] ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 525. Contiguous Array
```C++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        unordered_map<int, int> hash;
        hash[0] = -1;
        int res = 0, s = 0;
        for (int i = 0; i < nums.size(); i ++ )
        {
            s += nums[i] * 2 - 1;
            if (hash.count(s))
                res = max(res, i - hash[s]);
            if (!hash.count(s))
                hash[s] = i;
        }
        return res;
    }
};
```
#####  LeetCode 187. Repeated DNA Sequences
```C++
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> res;
        unordered_map<string, int> S;
        for (int i = 0; i + 10 <= s.size(); i ++ )
        {
            string str = s.substr(i, 10);
            if (S[str] == 1) res.push_back(str);
            S[str] ++ ;
        }
        sort(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode 347. Top K Frequent Elements
```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> hash;
        vector<int> res;
        for (int x : nums) hash[x] ++;
        int n = nums.size();
        vector<int>s(n + 1, 0);
        for (auto &p : hash) s[p.second] ++ ;
        int i = n, t = 0;
        while (t < k) t += s[i -- ];
        for (auto &p : hash)
            if (p.second > i)
                res.push_back(p.first);
        return res;
    }
};
```
#####  LeetCode 350. Intersection of Two Arrays II
```C++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        unordered_multiset<int> S;
        vector<int> res;
        for (int x : nums1) S.insert(x);
        for (int x : nums2)
            if (S.count(x))
            {
                res.push_back(x);
                S.erase(S.find(x));
            }
        return res;
    }
};
```
#####  LeetCode 290. Word Pattern
```C++
class Solution {
public:
    bool wordPattern(string pattern, string str) {
        stringstream raw(str);
        vector<string> strs;
        string line;
        while (raw >> line) strs.push_back(line);
        if (pattern.size() != strs.size()) return false;
        unordered_map<char, string> PS;
        unordered_map<string, char> SP;
        for (int i = 0; i < pattern.size(); i ++ )
        {
            if (PS.count(pattern[i]) == 0) PS[pattern[i]] = strs[i];
            if (SP.count(strs[i]) == 0) SP[strs[i]] = pattern[i];
            if (PS[pattern[i]] != strs[i]) return false;
            if (SP[strs[i]] != pattern[i]) return false;
        }
        return true;
    }
};
```
#####  LeetCode 149. Max Points on a Line
```C++
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
class Solution {
public:
    int maxPoints(vector<Point>& points) {
        if (points.empty()) return 0;
        int res = 1;
        for (int i = 0; i < points.size(); i ++ )
        {
            unordered_map<long double, int> map;
            int duplicates = 0, verticals = 1;

            for (int j = i + 1; j < points.size(); j ++ )
                if (points[i].x == points[j].x)
                {
                    verticals ++ ;
                    if (points[i].y == points[j].y) duplicates ++ ;
                }

            for (int j = i + 1; j < points.size(); j ++ )
                if (points[i].x != points[j].x)
                {
                    long double slope = (long double)(points[i].y - points[j].y) / (points[i].x - points[j].x);
                    if (map[slope] == 0) map[slope] = 2;
                    else map[slope] ++ ;
                    res = max(res, map[slope] + duplicates);
                }

            res = max(res, verticals);
        }
        return res;
    }
};
```
#####  LeetCode 355. Design Twitter
```C++
class Twitter {
public:
    /** Initialize your data structure here. */
    unordered_map<int,vector<pair<int,int>>> posts;
    unordered_map<int,unordered_set<int>> follows;
    int id = 0;
    Twitter() {
    }

    /** Compose a new tweet. */
    void postTweet(int userId, int tweetId) {
        posts[userId].push_back(make_pair(id ++, tweetId));
    }

    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    vector<int> getNewsFeed(int userId) {
        vector<pair<int,int>> ps;
        for (auto x : posts[userId]) ps.push_back(x);
        for (auto follow : follows[userId])
            for (auto x : posts[follow])
                ps.push_back(x);
        sort(ps.rbegin(), ps.rend());
        vector<int> res;
        for (int i = 0; i < 10 && i < ps.size(); i ++ )
            res.push_back(ps[i].second);
        return res;
    }

    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    void follow(int followerId, int followeeId) {
        if (followerId != followeeId)
            follows[followerId].insert(followeeId);
    }

    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    void unfollow(int followerId, int followeeId) {
        follows[followerId].erase(followeeId);
    }
};

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * vector<int> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```
#####  LeetCode 128. Longest Consecutive Sequence
```C++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int res = 0;
        unordered_map<int, int> tr_left, tr_right;
        for (auto & x : nums)
        {
            int left = tr_right[x - 1], right = tr_left[x + 1];
            tr_left[x - left] = max(tr_left[x - left], left + 1 + right);
            tr_right[x + right] = max(tr_right[x + right], left + 1 + right);
            res = max(res, left + 1 + right);
        }
        return res;
    }
};
```
#####  LeetCode 652. Find Duplicate Subtrees
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
    unordered_map<string, int> hash;
    vector<TreeNode*> ans;

    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        dfs(root);
        return ans;
    }

    string dfs(TreeNode *u)
    {
        if (!u) return ",NULL";
        string l = dfs(u->left), r = dfs(u->right);
        string v = "," + to_string(u->val) + l + r;
        if (++hash[v] == 2) ans.push_back(u);
        return v;
    }
};
```
#####  LeetCode 554. Brick Wall
```C++
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        unordered_map<int, int> hash;
        int res = 0;
        for (auto w : wall)
        {
            int s = 0;
            for (int i = 0; i + 1 < w.size(); i ++ )
            {
                s += w[i];
                res = max(res, ++hash[s]);
            }
        }
        return wall.size() - res;
    }
};
```

### Week8 动态规划(2/2)

#####  LeetCode 120. Triangle
```C++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        if (!n) return 0;
        vector<int> f(n), g(n);
        f[0] = triangle[0][0];
        for (int i = 1; i < n; i ++ )
        {
            for (int j = 0; j <= i; j ++ )
            {
                g[j] = INT_MAX;
                if (j < i) g[j] = min(g[j], f[j] + triangle[i][j]);
                if (j) g[j] = min(g[j], f[j - 1] + triangle[i][j]);
            }
            f = g;
        }
        int res = INT_MAX;
        for (int i = 0; i < n; i ++ ) res = min(res, f[i]);
        return res;
    }
};
```
#####  LeetCode 63. Unique Paths II
```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& nums) {
        int n = nums.size(), m = nums[0].size();
        vector<vector<long long>> f(n, vector<long long>(m));
        if (!nums[0][0]) f[0][0] = 1;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
            {
                if (i || j) f[i][j] = 0;
                if (!nums[i][j])
                {
                    if (i) f[i][j] += f[i - 1][j];
                    if (j) f[i][j] += f[i][j - 1];
                }
            }
        return f[n - 1][m - 1];
    }
};
```
#####  LeetCode 354. Russian Doll Envelopes
```C++
class Solution {
public:
    int maxEnvelopes(vector<pair<int, int>>& envelopes) {
        sort(envelopes.begin(), envelopes.end());
        int n = envelopes.size();
        vector<int> f(n);
        int res = 0;
        for (int i = 0; i < n; i ++ )
        {
            f[i] = 1;
            for (int j = 0; j < i; j ++ )
                if (envelopes[i].first > envelopes[j].first && envelopes[i].second > envelopes[j].second)
                    f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }
        return res;
    }
};
```
#####  LeetCode 338. Counting Bits
```C++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> f(num + 1, 0);
        for (int i = 1; i <= num; i ++ )
            f[i] = f[i >> 1] + (i & 1);
        return f;
    }
};
```
#####  LeetCode 329. Longest Increasing Path in a Matrix
```C++
class Solution {
public:
    int n, m;
    vector<vector<int>> f;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty()) return 0;
        n = matrix.size(), m = matrix[0].size();
        f = vector<vector<int>>(n, vector<int>(m, -1));
        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
            {
                res = max(res, dp(i, j, matrix));
            }
        return res;
    }

    int dp(int x, int y, vector<vector<int>> &matrix)
    {
        if (f[x][y] != -1) return f[x][y];
        f[x][y] = 1;
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && matrix[a][b] < matrix[x][y])
                f[x][y] = max(f[x][y], dp(a, b, matrix) + 1);
        }
        return f[x][y];
    }
};
```
#####  LeetCode 322. Coin Change
```C++
class Solution {
public:

    int INF = 1000000000;

    int coinChange(vector<int>& coins, int amount) {
        vector<int> f(amount + 1, INF);
        f[0] = 0;
        for (int i = 0; i < coins.size(); i ++ )
            for (int j = coins[i]; j <= amount; j ++ )
                f[j] = min(f[j], f[j - coins[i]] + 1);
        if (f[amount] == INF) f[amount] = -1;
        return f[amount];
    }
};
```
#####  LeetCode 221. Maximal Square
```C++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if (matrix.empty()) return 0;
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<int>> f(n + 1, vector<int>(m + 1, 0));
        int res = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                if (matrix[i - 1][j - 1] != '0')
                {
                    f[i][j] = min(f[i - 1][j - 1], min(f[i - 1][j], f[i][j - 1])) + 1;
                    res = max(res, f[i][j]);
                }
        return res * res;
    }
};
```
#####  LeetCode 91. Decode Ways
```C++
class Solution {
public:
    int numDecodings(string s) {
        if (s.empty()) return 0;
        int n = s.size();
        vector<int> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
        {
            if (s[i - 1] < '0' || s[i - 1] > '9') return 0;
            f[i] = 0;
            if (s[i - 1] != '0') f[i] = f[i - 1];
            if (i > 1)
            {
                int t = (s[i - 2] - '0') * 10 + s[i - 1] - '0';
                if (t >= 10 && t <= 26)
                    f[i] += f[i - 2];
            }
        }
        return f[n];
    }
};
```
#####  LeetCode 264. Ugly Number II
```C++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int>q;
        q.push_back(1);
        int a = 0, b = 0, c = 0;
        for (int i = 2; i <= n; i ++ )
        {
            int two = q[a] * 2, three = q[b] * 3, five = q[c] * 5;
            int next = min(two, min(three, five));
            q.push_back(next);
            if (two == next) a ++ ;
            if (three == next) b ++ ;
            if (five == next) c ++ ;
        }
        return q.back();
    }
};
```
#####  LeetCode 115. Distinct Subsequences
```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size(), m = t.size();
        vector<vector<int>>f(n + 1, vector<int>(m + 1, 0));
        for (int i = 0; i <= n; i ++ ) f[i][0] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
            {
                f[i][j] = f[i - 1][j];
                if (s[i - 1] == t[j - 1])
                    f[i][j] += f[i - 1][j - 1];
            }
        return f[n][m];
    }
};
```
#####  LeetCode 132. Palindrome Partitioning II
```C++
class Solution {
public:

    int minCut(string s) {
        int n = s.size();
        vector<int>f(n + 1);
        vector<vector<bool>> st(n, vector<bool>(n, false));

        for (int i = 0; i < n; i ++ )
            for (int j = i; j >= 0; j -- )
                if (i - j <= 1) st[j][i] = s[j] == s[i];
                else st[j][i] = s[j] == s[i] && st[j + 1][i - 1];

        f[0] = 0;
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = INT_MAX;
            for (int j = 0; j < i; j ++ )
                if (st[j][i - 1])
                    f[i] = min(f[i], f[j] + 1);
        }
        return max(0, f[n] - 1);
    }
};
```
#####  LeetCode 576. Out of Boundary Paths
```C++
class Solution {
public:
    vector<vector<vector<int>>> f;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    int mod = 1000000007;

    int findPaths(int m, int n, int N, int i, int j) {
        f = vector<vector<vector<int>>>(m, vector<vector<int>>(n, vector<int>(N + 1, -1)));
        return dp(m, n, N, i, j);
    }

    int dp(int m, int n, int k, int x, int y)
    {
        int &v = f[x][y][k];
        if (v != -1) return v;
        v = 0;
        if (!k) return v;
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a == m || b < 0 || b == n) v ++ ;
            else v += dp(m, n, k - 1, a, b);
            v %= mod;
        }
        return v;
    }
};
```
#####  LeetCode 526. Beautiful Arrangement
```C++
class Solution {
public:
    int countArrangement(int N) {
        vector<int> f(1<<N, 0);
        f[0] = 1;
        for (int i = 0; i < (1<<N); i ++ )
        {
            int s = 1;
            for (int j = 0; j < N; j ++ ) s += i >> j & 1;
            for (int j = 1; j <= N; j ++ )
            {
                if (!(i >> (j - 1) & 1) && (s % j == 0 || j % s == 0))
                    f[i | (1 << (j - 1))] += f[i];
            }
        }
        return f[(1 << N) - 1];
    }
};

```
#####  LeetCode 486. Predict the Winner
```C++
class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> f(n + 1, vector<int>(n + 1, 0));
        if (n % 2)
            for (int i = 1; i <= n; i ++ ) f[i][i] = nums[i - 1];
        for (int i = 2; i <= n; i ++ )
        {
            for (int j = 1; j + i - 1 <= n; j ++ )
            {
                int l = j, r = j + i - 1;
                if ((n - r + l - 1) % 2 == 0)
                    f[l][r] = max(f[l + 1][r] + nums[l - 1], f[l][r - 1] + nums[r - 1]);
                else
                    f[l][r] = min(f[l + 1][r], f[l][r - 1]);
            }
        }

        int sum = 0;
        for (auto x : nums) sum += x;
        return f[1][n] >= sum - f[1][n];
    }
};
```



## 第二期

### Week1 宽(深)度优先遍历专题

#####  LeetCode 111. Minimum Depth of Binary Tree
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
    int minDepth(TreeNode* root) {
        return root ? !root->left && !root->right ? 1 : min(minDepth(root->left) + !root->left * 100000, minDepth(root->right) + !root->right * 100000) + 1 : 0;
    }
};
```
#####  LeetCode 279. Perfect Squares
```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> f(n + 1);
        f[0] = 0;
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = f[i - 1] + 1;
            for (int j = 1; j * j <= i; j ++ )
                f[i] = min(f[i], f[i - j * j] + 1);
        }
        return f[n];
    }
};
```
#####  LeetCode 200. Number of Islands
```C++
class Solution {
public:
    int n, m;
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty()) return 0;
        n = grid.size(), m = grid[0].size();
        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (grid[i][j] == '1')
                {
                    res ++ ;
                    dfs(i, j, grid);
                }
        return res;
    }

    void dfs(int x, int y, vector<vector<char>>& grid)
    {
        grid[x][y] = '0';
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && grid[a][b] == '1')
                dfs(a, b, grid);
        }
    }
};
```
#####  LeetCode 130. Surrounded Regions
```C++
class Solution {
public:
    vector<vector<bool>> st;
    int n, m;

    void solve(vector<vector<char>>& board) {
        if (board.empty()) return;
        n = board.size(), m = board[0].size();
        for (int i = 0; i < n; i ++ )
        {
            vector<bool> temp;
            for (int j = 0; j < m; j ++ )
                temp.push_back(false);
            st.push_back(temp);
        }

        for (int i = 0; i < n; i ++ )
        {
            if (board[i][0] == 'O') dfs(i, 0, board);
            if (board[i][m - 1] == 'O') dfs(i, m - 1, board);
        }
        for (int i = 0; i < m; i ++ )
        {
            if (board[0][i] == 'O') dfs(0, i, board);
            if (board[n - 1][i] == 'O') dfs(n - 1, i, board);
        }

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!st[i][j])
                    board[i][j] = 'X';
    }

    void dfs(int x, int y, vector<vector<char>>&board)
    {
        st[x][y] = true;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && !st[a][b] && board[a][b] == 'O')
                dfs(a, b, board);
        }
    }
};
```
#####  LeetCode 543. Diameter of Binary Tree
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
    int ans = 1;
    int diameterOfBinaryTree(TreeNode* root) {
        dfs(root);
        return ans - 1;
    }
    int dfs(TreeNode *u)
    {
        if (!u) return 0;
        int l = dfs(u->left);
        int r = dfs(u->right);
        ans = max(ans, l + r + 1);
        return max(l, r) + 1;
    }
};
```
#####  LeetCode 127. Word Ladder
```C++
class Solution {
public:
    bool check(string a, string b)
    {
        int res = 0;
        for (int i = 0; i < a.size(); i ++ ) res += a[i] != b[i];
        return res == 1;
    }

    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_map<string, int>dist;
        queue<string> que;
        que.push(beginWord), dist[beginWord] = 1;
        while (!que.empty())
        {
            string t = que.front();
            que.pop();

            if (t == endWord) return dist[t];
            for (auto &word : wordList)
                if (check(t, word) && !dist[word])
                {
                    dist[word] = dist[t] + 1;
                    que.push(word);
                }
        }
        return 0;
    }
};
```
#####  LeetCode 207. Course Schedule
```C++
class Solution {
public:
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        unordered_map<int, int>d;
        queue<int> que;
        unordered_map<int, vector<int>>g;
        for (auto &edge : prerequisites)
        {
            int a = edge.first, b = edge.second;
            d[b] ++ ;
            if (!d.count(a)) d[a] = 0;
            if (!g.count(a)) g[a] = vector<int>();
            g[a].push_back(b);
        }
        for (auto &node : d)
            if (!node.second)
                que.push(node.first);
        while (!que.empty())
        {
            int t = que.front();
            que.pop();
            for (int e : g[t])
                if (-- d[e] == 0)
                    que.push(e);
        }
        for (auto &node : d)
            if (node.second)
                return false;
        return true;
    }
};
```
#####  LeetCode 210. Course Schedule II
```C++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<int> d(numCourses, 0);
        queue<int> que;
        vector<vector<int>> g(numCourses, vector<int>());
        for (auto &edge : prerequisites)
        {
            int a = edge.first, b = edge.second;
            d[b] ++ ;
            g[a].push_back(b);
        }
        vector<int> res;
        for (int i = 0; i < numCourses; i ++ )
            if (!d[i])
            {
                res.push_back(i);
                que.push(i);
            }
        while (!que.empty())
        {
            int t = que.front();
            que.pop();
            for (int e : g[t])
                if (-- d[e] == 0)
                {
                    que.push(e);
                    res.push_back(e);
                }
        }
        if (res.size() != numCourses) res = vector<int>();
        else
            reverse(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode 733. Flood Fill
```C++
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) return image;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        int rawColor = image[sr][sc];
        image[sr][sc] = newColor;
        for (int i = 0; i < 4; i ++ )
        {
            int x = sr + dx[i], y = sc + dy[i];
            if (x >= 0 && x < image.size() && y >= 0 && y < image[0].size() && image[x][y] == rawColor)
                floodFill(image, x, y, newColor);
        }
        return image;
    }
};
```
#####  LeetCode 542. 01 Matrix
```C++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<int>> dist(n, vector<int>(m, -1));
        queue<pair<int,int>> q;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!matrix[i][j])
                {
                    dist[i][j] = 0;
                    q.push(make_pair(i, j));
                }
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        while (q.size())
        {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < 4; i ++ )
            {
                int x = t.first + dx[i], y = t.second + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m && dist[x][y] == -1)
                {
                    dist[x][y] = dist[t.first][t.second] + 1;
                    q.push(make_pair(x, y));
                }
            }
        }
        return dist;
    }
};

```

### Week2 位运算 

#####  LeetCode 231. Power of Two
```C++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && (n & -n) == n;
    }
};
```
#####  LeetCode 762. Prime Number of Set Bits in Binary Representation
```C++
class Solution {
public:
    int countPrimeSetBits(int L, int R) {
        int res = 0;
        unordered_set<int> primes;
        int ps[] = {2, 3, 5, 7, 11, 13, 17, 19};
        for (auto p : ps) primes.insert(p);
        for (int i = L; i <= R; i ++ )
        {
            int s = 0;
            for (int j = i; j; j >>= 1) s += j & 1;
            int k = 2;
            if (primes.count(s)) res ++;
        }
        return res;
    }
};
```
#####  LeetCode 136. Single Number
```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = 0;
        for (auto x : nums) res ^= x;
        return res;
    }
};
```
#####  LeetCode 137. Single Number II
```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
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
#####  LeetCode 260. Single Number III
```C++
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int s = 0;
        for (int x : nums) s ^= x;
        int k = 0;
        while ((s >> k & 1) == 0) k ++ ;
        int s2 = 0;
        for (int x : nums)
            if (x >> k & 1)
                s2 ^= x;
        vector<int> res(2);
        res[0] = s2, res[1] = s ^ s2;
        return res;
    }
};
```
#####  LeetCode 371. Sum of Two Integers
```C++
class Solution {
public:
    int getSum(int _a, int _b) {
        unsigned a = _a, b = _b;
        int res = 0, t = 0, bit = 1;
        while (a || b || t)
        {
            int x = a & 1, y = b & 1;
            res |= (x ^ y ^ t) * bit;
            t = (x & y) | (x & t) | (y & t);
            a >>= 1, b >>= 1, bit <<= 1;
        }
        return res;
    }
};
```
#####  LeetCode 201. Bitwise AND of Numbers Range
```C++
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int res = 0;
        for (int i = 0; (1ll << i) <= m; i ++ )
            if (m >> i & 1)
            {
                if ((m & ~((1 << i) - 1ll)) + (1 << i) > n)
                    res += 1 << i;
            }
        return res;
    }
};
```
#####  LeetCode 421. Maximum XOR of Two Numbers in an Array
```C++
class Solution {
public:
    typedef long long LL;

    int findMaximumXOR(vector<int>& nums) {
        unordered_set<LL> edge;
        int res = 0;
        for (auto &x : nums)
        {
            LL pre = 0, pre_op = 0;
            int xorr = 0;
            for (int i = 30; i >= 0; i--)
            {
                int next = x >> i & 1;
                edge.insert(pre + next * (1LL << 32) + i * (1LL << 33));
                if (edge.count(pre_op + !next * (1LL << 32) + i * (1LL << 33)))
                {
                    xorr = xorr * 2 + 1;
                    pre_op = pre_op * 2 + !next;
                }
                else
                {
                    xorr <<= 1;
                    pre_op = pre_op * 2 + next;
                }
                pre = pre * 2 + next;
            }
            res = max(res, xorr);
        }
        return res;
    }
};
```
#####  LeetCode 476. Number Complement
```C++
class Solution {
public:
    int findComplement(int num) {
        int res = 0, t = 0;
        while (num)
        {
            res += !(num & 1) << t;
            num >>= 1;
            t ++;
        }
        return res;
    }
};
```
#####  LeetCode 477. Total Hamming Distance
```C++
class Solution {
public:
    int totalHammingDistance(vector<int>& nums) {
        int res = 0;
        for (int i = 30; i >= 0; i -- )
        {
            int ones = 0;
            for (auto x : nums)
                if (x >> i & 1)
                    ones ++ ;
            res += (nums.size() - ones) * ones;
        }
        return res;
    }
};
```

### Week3 贪心

#####  LeetCode 860. Lemonade Change
```C++
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int fives = 0, tens = 0;
        for (auto b: bills)
            if (b == 5) fives ++ ;
            else if (b == 10)
            {
                if (fives) fives -- ;
                else return false;
                tens ++ ;
            }
            else
            {
                int t = 15;
                if (tens)
                {
                    t -= 10;
                    tens -- ;
                }
                while (fives && t > 0)
                {
                    fives --;
                    t -= 5;
                }
                if (t) return false;
            }
        return true;
    }
};
```
#####  LeetCode 392. Is Subsequence
```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if (s.empty()) return true;
        for (int i = 0, j = 0; j < t.size(); j ++ )
        {
            if (s[i] == t[j]) i ++ ;
            if (i == s.size()) return true;
        }
        return false;
    }
};
```
#####  LeetCode 455. Assign Cookies
```C++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int res = 0, i = 0, j = 0;
        while (i < g.size() && j < s.size())
        {
            while (j < s.size() && s[j] < g[i]) j ++ ;
            if (j < s.size())
            {
                res ++ ;
                j ++ ;
            }
            i ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 55. Jump Game
```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int dist = 0;
        for (int i = 0; i < nums.size() && dist >= i; i ++ )
            dist = max(dist, i + nums[i]);
        return dist + 1 >= nums.size();
    }
};
```
#####  LeetCode 45. Jump Game II
```C++
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() == 1) return 0;
        int l = 0, r = 0, step = 0;
        while (l <= r)
        {
            int max_r = 0;
            step ++ ;
            for (int i = l; i <= r; i ++ )
                max_r = max(max_r, nums[i] + i);
            l = r + 1, r = max_r;
            if (r >= (int)nums.size() - 1) break;
        }
        return step;
    }
};
```
#####  LeetCode 376. Wiggle Subsequence
```C++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        nums.erase(unique(nums.begin(), nums.end()), nums.end());
        if (nums.size() <= 2) return nums.size();
        int res = 2;
        for (int i = 1; i + 1 < nums.size(); i ++ )
            if (nums[i - 1] < nums[i] && nums[i] > nums[i + 1]
                || nums[i - 1] > nums[i] && nums[i] < nums[i + 1])
                res ++ ;
        return res;
    }
};
```
#####  LeetCode 406. Queue Reconstruction by Height
```C++
bool cmp(pair<int,int> a, pair<int,int> b)
{
    return a.first > b.first || a.first == b.first && a.second < b.second;
}


class Solution {
public:
    vector<pair<int, int>> reconstructQueue(vector<pair<int, int>>& people) {
        sort(people.begin(), people.end(), cmp);

        vector<pair<int,int>> res;
        for (auto p : people) res.insert(res.begin() + p.second, p);
        return res;
    }
};
```
#####  LeetCode 452. Minimum Number of Arrows to Burst Balloons
```C++
bool cmp(pair<int,int> a, pair<int,int> b)
{
    return a.second < b.second;
}

class Solution {
public:
    int findMinArrowShots(vector<pair<int, int>>& points) {
        sort(points.begin(), points.end(), cmp);
        long long res = 0, r = INT_MIN - 1ll;
        for (auto p : points)
            if (p.first > r)
            {
                r = p.second;
                res ++ ;
            }
        return res;
    }
};
```
#####  LeetCode 402. Remove K Digits
```C++
class Solution {
public:
    string removeKdigits(string num, int k) {
        stack<char> stk;
        for (auto x : num)
        {
            while (stk.size() && stk.top() > x && k)
            {
                stk.pop();
                k--;
            }
            stk.push(x);
        }

        while (k -- ) stk.pop();

        string res;
        while (stk.size())
        {
            res += stk.top();
            stk.pop();
        }
        reverse(res.begin(), res.end());
        int i = 0;
        while (i < res.size() && res[i] == '0') i++;
        if (i == res.size()) return "0";
        return res.substr(i);
    }
};
```
#####  LeetCode 134. Gas Station
```C++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        for (int i = 0, j; i < n; i += j + 1)
        {
            int gas_left = 0;
            for (j = 0; j < n; j ++ )
            {
                int k = (i + j) % n;
                gas_left += gas[k] - cost[k];
                if (gas_left < 0)
                    break;
            }
            if (j >= n) return i;
        }
        return -1;
    }
};
```


## 第三期

### Week1 模拟

#####  LeetCode 263. Ugly Number
```C++
class Solution {
public:
    bool isUgly(int num) {
        if (num <= 0) return false;
        while (num % 2 == 0) num /= 2;
        while (num % 3 == 0) num /= 3;
        while (num % 5 == 0) num /= 5;
        return num == 1;
    }
};
```
#####  LeetCode 67. Add Binary
```C++
class Solution {
public:
    string addBinary(string a, string b) {
        if (a.size() < b.size()) swap(a, b);
        reverse(a.begin(), a.end());
        reverse(b.begin(), b.end());
        int t = 0;
        string res;
        int k = 0;
        while (k < b.size())
        {
            t += a[k] - '0' + b[k] - '0';
            res += to_string(t&1);
            t /= 2;
            k ++ ;
        }
        while (k < a.size())
        {
            t += a[k] - '0';
            res += to_string(t&1);
            t /= 2;
            k ++ ;
        }
        if (t) res += '1';
        reverse(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode 504. Base 7
```C++
class Solution {
public:
    string convertToBase7(int num) {
        string res, minus;
        if (num < 0)
        {
            minus = '-';
            num = -num;
        }
        if (!num) res = '0';
        while (num)
        {
            res += to_string(num % 7);
            num /= 7;
        }
        reverse(res.begin(), res.end());
        return minus + res;
    }
};
```
#####  LeetCode 54. Spiral Matrix
```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.empty()) return vector<int>();
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<bool>> st(n, vector<bool>(m, false));
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        int x = 0, y = 0, d = 0;
        vector<int> res;
        for (int i = 0; i < n * m; i ++ )
        {
            int a = x + dx[d], b = y + dy[d];
            if (a < 0 || a >= n || b < 0 || b >= m || st[a][b])
            {
                d = (d + 1) % 4;
                a = x + dx[d], b = y + dy[d];
            }
            res.push_back(matrix[x][y]);
            st[x][y] = true;
            x = a, y = b;
        }
        return res;
    }
};

```
#####  LeetCode 24. Swap Nodes in Pairs
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
    ListNode* swapPairs(ListNode* head) {
        ListNode *dummy = new ListNode(0);
        dummy->next = head;
        ListNode *p = dummy;
        while(p && p->next && p->next->next)
        {
            ListNode *a = p->next;
            ListNode *b = a->next;
            a->next = b->next;
            b->next = a;
            p->next = b;
            p = a;
        }
        return dummy->next;
    }
};
```
#####  LeetCode 299. Bulls and Cows
```C++
class Solution {
public:
    string getHint(string secret, string guess) {
        int a = 0, b = 0, n = secret.size();
        vector<int>ds(10, 0), dg(10, 0);
        for (int i = 0; i < n; i ++ )
        {
            int s = secret[i] - '0', g = guess[i] - '0';
            a += s == g;
            ds[s] ++, dg[g] ++ ;
        }
        for (int i = 0; i < 10; i ++ ) b += min(ds[i], dg[i]);
        b -= a;
        return to_string(a) + 'A' + to_string(b) + 'B';
    }
};
```
#####  LeetCode 481. Magical String
```C++
class Solution {
public:
    int magicalString(int n) {
        string s = "122";
        for (int i = 2, j = 1; s.size() <= n; i ++, j = 3 - j)
        {
            for (int k = 0; k < s[i] - '0'; k ++ ) s += to_string(j);
        }
        int res = 0;
        for (int i = 0; i < n; i ++ ) res += s[i] == '1';
        return res;
    }
};
```
#####  LeetCode 71. Simplify Path
```C++
class Solution {
public:
    string simplifyPath(string path) {
        if (path.back() != '/') path += '/';
        string res, s;
        for (auto &c : path)
        {
            if (res.empty()) res += c;
            else if (c == '/')
            {
                if (s == "..")
                {
                    if (res.size() > 1)
                    {
                        res.pop_back();
                        while (res.back() != '/') res.pop_back();
                    }
                }
                else if (s != "" && s != ".")
                {
                    res += s + '/';
                }
                s = "";
            }
            else
            {
                s += c;
            }
        }
        if (res.size() > 1) res.pop_back();
        return res;
    }
};
```
#####  LeetCode 68. Text Justification
```C++
class Solution {
public:
    string space(int x)
    {
        string res;
        while (x -- ) res += ' ';
        return res;
    }

    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        vector<string> res;
        for (int i = 0; i < words.size();)
        {
            int j = i + 1, s = words[i].size(), rs = words[i].size();
            while (j < words.size() && s + 1 + words[j].size() <= maxWidth)
            {
                s += 1 + words[j].size();
                rs += words[j].size();
                j ++ ;
            }
            rs = maxWidth - rs;
            string line = words[i];
            if (j == words.size())
            {
                for (i ++; i < j; i ++ )
                    line += ' ' + words[i];
                while (line.size() < maxWidth) line += ' ';
            }
            else if (j - i == 1) line += space(rs);
            else
            {
                int base = rs / (j - i - 1);
                int rem = rs % (j - i - 1);
                i ++ ;
                for (int k = 0; i < j; i ++, k ++ )
                    line += space(base + (k < rem)) + words[i];
            }
            i = j;
            res.push_back(line);
        }
        return res;
    }
};
```
#####  LeetCode 12. Integer to Roman
```C++
class Solution {
public:
    string intToRoman(int num) {
        int values[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        string reps[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

        string res;
        for (int i = 0; i < 13; i ++ )
            while(num >= values[i])
            {
                num -= values[i];
                res += reps[i];
            }
        return res;
    }
};
```
#####  LeetCode 77. Combinations
```C++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combine(int n, int k) {
        dfs(0, 1, n, k);
        return ans;
    }

    void dfs(int u, int start, int n, int k)
    {
        if (u == k)
        {
            ans.push_back(path);
            return ;
        }

        for (int i = start; i <= n; i ++ )
        {
            path.push_back(i);
            dfs(u + 1, i + 1, n, k);
            path.pop_back();
        }
    }
};
```
#####  LeetCode 257. Binary Tree Paths
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
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> res;
        if (!root) return res;
        if (root->left)
        {
            for (auto path : binaryTreePaths(root->left)) res.push_back(to_string(root->val) + "->" + path);
        }
        if (root->right)
        {
            for (auto path : binaryTreePaths(root->right)) res.push_back(to_string(root->val) + "->" + path);
        }
        if (!root->left && !root->right) res.push_back(to_string(root->val));
        return res;
    }
};
```
#####  LeetCode 93. Restore IP Addresses
```C++
class Solution {
public:
    vector<string> ans;
    vector<int> path;

    vector<string> restoreIpAddresses(string s) {
        dfs(0, 0, s);
        return ans;
    }

    void dfs(int u, int k, string &s)
    {
        if (u == s.size())
        {
            if (k == 4)
            {
                string ip = to_string(path[0]);
                for (int i = 1; i < 4; i ++ )
                    ip += '.' + to_string(path[i]);
                ans.push_back(ip);
            }
            return;
        }
        if (k > 4) return;

        int t = 0;
        for (int i = u; i < s.size(); i ++ )
        {
            t = t * 10 + s[i] - '0';
            if (t >= 0 && t < 256)
            {
                path.push_back(t);
                dfs(i + 1, k + 1, s);
                path.pop_back();
            }
            if (!t) break;
        }
    }
};
```

#####  LeetCode 95. Unique Binary Search Trees II
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
    vector<TreeNode*> generateTrees(int n) {
        if (!n) return vector<TreeNode*>();
        return dfs(1, n);
    }

    vector<TreeNode*> dfs(int l, int r)
    {
        vector<TreeNode*>res;
        if (l > r)
        {
            res.push_back(NULL);
            return res;
        }
        for (int i = l; i <= r; i ++ )
        {
            vector<TreeNode*>left = dfs(l, i - 1), right = dfs(i + 1, r);
            for (auto &lc : left)
                for (auto &rc : right)
                {
                    TreeNode *root = new TreeNode(i);
                    root->left = lc;
                    root->right = rc;
                    res.push_back(root);
                }
        }
        return res;
    }
};
```
#####  LeetCode 394. Decode String
```C++
class Solution {
public:
    string decodeString(string s) {
        string res;
        for (int i = 0; i < s.size();)
        {
            if (!isdigit(s[i])) res += s[i ++ ];
            else
            {
                int j = i;
                while (isdigit(s[j])) j ++ ;
                int t = atoi(s.substr(i, j - i).c_str());
                int k = j + 1, sum = 0;
                while (sum >= 0)
                {
                    if (s[k] == '[') sum ++ ;
                    if (s[k] == ']') sum -- ;
                    k ++ ;
                }
                string str = decodeString(s.substr(j + 1, k - j - 2));
                while (t -- ) res += str;
                i = k;
            }
        }
        return res;
    }
};
```
#####  LeetCode 341. Flatten Nested List Iterator
```C++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class NestedIterator {
public:
    vector<int> seq;
    int cnt = 0;

    NestedIterator(vector<NestedInteger> &nestedList) {
        dfs(nestedList);
    }

    void dfs(vector<NestedInteger> &nestedList)
    {
        for (auto t : nestedList)
        {
            if (t.isInteger()) seq.push_back(t.getInteger());
            else
                dfs(t.getList());
        }
    }

    int next() {
        return seq[cnt ++ ];
    }

    bool hasNext() {
        return cnt < seq.size();
    }
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```
#####  LeetCode 756. Pyramid Transition Matrix
```C++
class Solution {
public:

    vector<char> allows[30][30];

    bool pyramidTransition(string bottom, vector<string>& allowed) {
        for (string allow : allowed)
        {
            char a = allow[0], b = allow[1], c = allow[2];
            allows[a - 'A'][b - 'A'].push_back(c);
        }
        return dfs(bottom, "");
    }

    bool dfs(string &last, string now)
    {
        if (last.size() == 1) return true;

        if (now.size() + 1 == last.size()) return dfs(now, "");

        char a = last[now.size()], b = last[now.size() + 1];
        for (auto c : allows[a - 'A'][b - 'A'])
        {
            now += c;
            if (dfs(last, now)) return true;
            now.pop_back();
        }

        return false;
    }
};
```
#####  LeetCode 79. Word Search
```C++
class Solution {
public:
    int n, m, step;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    vector<vector<int>> st;

    bool exist(vector<vector<char>>& board, string word) {
        if (word.empty()) return true;
        if (board.empty()) return false;

        n = board.size(), m = board[0].size();
        st = vector<vector<int>>(n, vector<int>(m, 0));

        step = 1;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++, step ++ )
                if (dfs(i, j, board, word, 0))
                    return true;

        return false;
    }

    bool dfs(int x, int y, vector<vector<char>>& board, string &word, int k)
    {
        if (board[x][y] != word[k]) return false;

        if (k + 1 == word.size()) return true;

        st[x][y] = step;
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && st[a][b] != step)
            {
                if (dfs(a, b, board, word, k + 1)) return true;
            }
        }
        st[x][y] = 0;

        return false;
    }
};
```
### Week2 DFS

#####  LeetCode 784. Letter Case Permutation
```C++
class Solution {
public:

    vector<string> ans;

    vector<string> letterCasePermutation(string S) {
        dfs(S, 0);
        return ans;
    }

    void dfs(string &S, int u)
    {
        if (u == S.size())
        {
            ans.push_back(S);
            return;
        }
        dfs(S, u + 1);
        if (S[u] >= 'A')
        {
            S[u] ^= 32;
            dfs(S, u + 1);
        }
    }
};
```
#####  LeetCode 464. Can I Win
```C++
class Solution {
public:

    map<pair<int, int>, bool> S;

    bool canIWin(int max, int des) {
        if (des <= 0) return true;
        if (max * (max + 1) / 2 < des) return false;

        return dfs(0, max, des);
    }

    bool dfs(int state, int max, int des)
    {
        if (des <= 0) return false;
        if (S.count({state, des})) return S[{state, des}];

        for (int i = 0; i < max; i ++ )
            if (!(state >> i & 1))
            {
                if (!dfs(state | (1 << i), max, des - i - 1))
                    return S[{state, des}] = true;
            }
        return false;
    }
};
```

### Week3 数学

#####  LeetCode 268. Missing Number
```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int res = nums.size() * (nums.size() + 1) / 2;
        for (int x : nums) res -= x;
        return res;
    }
};
```
#####  LeetCode 62. Unique Paths
```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> f;
        for (int i = 0; i <= n; i ++ )
        {
            vector<int> temp;
            for (int j = 0; j <= m; j ++ )
                temp.push_back(0);
            f.push_back(temp);
        }
        f[1][1] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                f[i][j] += f[i - 1][j] + f[i][j - 1];

        return f[n][m];
    }
};
```
#####  LeetCode 462. Minimum Moves to Equal Array Elements II
```C++
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        int n = nums.size();
        nth_element(nums.begin(), nums.begin() + n / 2, nums.end());
        int k = nums[n / 2];
        int res = 0;
        for (auto x : nums) res += abs(k - x);
        return res;
    }
};
```
#####  LeetCode 458. Poor Pigs
```C++
class Solution {
public:
    int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int t = minutesToTest / minutesToDie + 1;
        int k = 1, res = 0;
        while (k < buckets) k *= t, res ++ ;
        return res;
    }
};
```
#####  LeetCode 319. Bulb Switcher
```C++
class Solution {
public:
    int bulbSwitch(int n) {
        return sqrt(n);
    }
};
```
#####  LeetCode 343. Integer Break
```C++
class Solution {
public:
    int integerBreak(int n) {
        if (n <= 3) return 1 * (n - 1);
        int res = 1;
        if (n % 3 == 1) res = 4, n -= 4;
        else if (n % 3 == 2) res = 2, n -= 2;
        while (n) res *= 3, n -= 3;
        return res;
    }
};
```
#####  LeetCode 223. Rectangle Area
```C++
class Solution {
public:
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        long long X = min(C, G) + 0ll - max(A, E);
        long long Y = min(D, H) + 0ll - max(B, F);
        return (C - A) * (D - B) + (G - E) * (H - F) - max(0ll, X) * max(0ll, Y);
    }
};
```
#####  LeetCode 650. 2 Keys Keyboard
```C++
class Solution {
public:
    int minSteps(int n) {
        int res = 0;
        for (int i = 2; n > 1; i ++ )
            while (n % i == 0)
            {
                res += i;
                n /= i;
            }
        return res;
    }
};
```
#####  LeetCode 829. Consecutive Numbers Sum
```C++
class Solution {
public:
    int consecutiveNumbersSum(int N) {
        int res = 0;
        for (int b = 1; b * (b - 1) < N * 2; b ++ )
            if (2 * N % b == 0 && (2 * N / b - b + 1) % 2 == 0)
                res ++ ;
        return res;
    }
};
```
#####  LeetCode 891. Sum of Subsequence Widths
```C++
class Solution {
public:

    int mod = 1000000007;

    int sumSubseqWidths(vector<int>& A) {
        sort(A.begin(), A.end());
        long long res = 0, p = 1, sum = 0;
        for (auto x : A)
        {
            res = (res + x * (p - 1) - sum) % mod;
            sum = (sum * 2 + x) % mod;
            p = p * 2 % mod;
        }
        return (res + mod) % mod;
    }
};
```

## 第四期

### Week1 Google面试题

#####  LeetCode 14. Longest Common Prefix
```C++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";
        if (strs.size() == 1) return strs[0];
        string res;
        for (int i = 0;; i ++ )
        {
            bool flag = false;
            for (int j = 1; j < strs.size(); j ++ )
                if (i >= strs[0].size() || i >= strs[j].size() || strs[j][i] != strs[0][i])
                {
                    flag = true;
                    break;
                }
            if (flag) break;
            res += strs[0][i];
        }
        return res;
    }
};
```
#####  LeetCode 20. Valid Parentheses
```C++
class Solution {
public:
    bool isValid(string str) {
        stack<char>sta;
        for (int i = 0; i < str.size(); i ++ )
        {
            char s = str[i];
            if (s == '(' || s == '[' || s == '{') sta.push(s);
            else
            {
                if (sta.empty()) return false;
                if (s == ')' && sta.top() != '(') return false;
                if (s == ']' && sta.top() != '[') return false;
                if (s == '}' && sta.top() != '{') return false;
                sta.pop();
            }
        }
        return sta.empty();
    }
}; 
```
#####  LeetCode 43. Multiply Strings
```C++
class Solution {
public:
    string multiply(string num1, string num2) {
        vector<int> product(num1.size() + num2.size());
        for (auto &v : product) v = 0;
        for (int i = num1.size() - 1; i >= 0; i -- )
            for (int j = num2.size() - 1; j >= 0; j -- )
                product[i + j + 1] += (num1[i] - '0') * (num2[j] - '0');
        for (int i = num1.size() + num2.size() - 1, t = 0; i >= 0; i -- )
        {
            product[i] += t;
            t = product[i] / 10;
            product[i] %= 10;
        }

        int k = 0;
        while (k + 1 < product.size() && product[k] == 0) k ++ ;
        string res;
        while (k < product.size()) res += to_string(product[k ++ ]);
        return res;
    }
};
```
#####  LeetCode 48. Rotate Image
```C++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for (int i = 0; i < n; i ++ )
            for (int j = i + 1; j < n; j ++ )
                swap(matrix[i][j], matrix[j][i]);
        for (int i = 0; i < n; i ++ )
            for (int j = 0, k = n - 1; j < k; j ++, k -- )
                swap(matrix[i][j], matrix[i][k]);
    }
};
```
#####  LeetCode 31. Next Permutation
```C++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        for (int i = nums.size() - 1; i > 0; i -- )
            if (nums[i] > nums[i - 1])
            {
                int t = i;
                while (t + 1 < nums.size() && nums[t + 1] > nums[i - 1]) t ++ ;
                swap(nums[i - 1], nums[t]);
                reverse(nums.begin() + i, nums.end());
                return;
            }
        reverse(nums.begin(), nums.end());
    }
};
```
#####  LeetCode 23. Merge k Sorted Lists
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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode *head = new ListNode(0);
        ListNode *p = head;
        priority_queue<pair<int, ListNode*>> heap;
        for (int i = 0; i < lists.size(); i ++ )
        {
            ListNode* head = lists[i];
            if (head) heap.push(make_pair(-head->val, head));
        }
        while(!heap.empty())
        {
            ListNode* t = heap.top().second;
            heap.pop();
            p->next = t;
            p = t;
            t = t->next;
            if (t)
                heap.push(make_pair(-t->val, t));
        }
        return head->next;
    }
};
```
#####  LeetCode 33. Search in Rotated Sorted Array
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.size() < 5)
        {
            for (int i = 0; i < nums.size(); i ++ )
                if (nums[i] == target)
                    return i;
            return -1;
        }
        int left, right;
        if (nums[0] < nums[1] && nums[1] < nums.back())
        {
            left = 0, right = nums.size() - 1;
        }
        else
        {
            int l = 0, r = nums.size() - 1;
            while (l < r)
            {
                int mid = (l + r + 1) / 2;
                if (nums[mid] >= nums[0] && nums[mid] > nums.back()) l = mid;
                else r = mid - 1;
            }
            if (target >= nums[0]) left = 0, right = l;
            else left = l + 1, right = nums.size() - 1;
        }

        cout << left << ' ' << right << endl;
        while (left < right)
        {
            int mid = (left + right) / 2;
            if (nums[mid] < target) left = mid + 1;
            else right = mid;
        }

        if (nums[right] == target) return right;
        return -1;
    }
};
```
#####  LeetCode 11. Container With Most Water
```C++
class Solution {
public:
     int maxArea(vector<int>& height) {
        int i =0 ,j = height.size()-1;
        int ans;
        int maxans = 0;
        while(i<j)
        {
            int dis = j-i;
            ans = min(height[i],height[j])*dis;
            if(ans > maxans)
                maxans = ans;
            if(height[j]>height[i])
                i++;
            else
                j--;
        }
        return maxans;
    }
};
```
#####  LeetCode 41. First Missing Positive
```C++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i ++ )
            while (nums[i] > 0 && nums[i] <= nums.size() && nums[i] != nums[nums[i] - 1])
                swap(nums[i], nums[nums[i] - 1]);

        for (int i = 0; i < nums.size(); i ++ )
            if (nums[i] != i + 1)
                return i + 1;

        return nums.size() + 1;
    }
};
```
#####  LeetCode 25. Reverse Nodes in k-Group
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
    ListNode* reverseKGroup(ListNode* head, int k) {
        if (k == 1) return head;
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *p = dummy;
        while (p)
        {
            int s = 0;
            for (ListNode *i = p->next; i && s < k; i = i->next)
                s ++ ;
            if (s < k) break;

            s = 1;
            ListNode *a = p->next;
            ListNode *b = a, *c = a->next;
            while (s < k)
            {
                ListNode *d = c->next;
                c->next = b;
                b = c;
                c = d;
                s++;
            }
            p->next = b;
            a->next = c;
            p = a;
        }
        return dummy->next;
    }
};
```
### Week2 Facebook面试题

#####  LeetCode 125. Valid Palindrome
```C++
class Solution {
public:
    bool check(char c)
    {
        return c >= '0' && c <= '9' || c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z';
    }

    bool isPalindrome(string s) {
        int i = 0, j = (int)s.size() - 1;
        while (i < j)
        {
            while (i < j && !check(s[i])) i ++ ;
            while (i < j && !check(s[j])) j -- ;
            if (s[i] != s[j] && s[i] != (s[j]^32)) return false;
            i ++, j --;
        }
        return true;
    }
};
```
#####  LeetCode 88. Merge Sorted Array
```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1, j = n - 1, k = n + m - 1;
        while (i >= 0 && j >= 0)
            if (nums1[i] >= nums2[j])
                nums1[k -- ] = nums1[i -- ];
            else
                nums1[k -- ] = nums2[j -- ];
        while (i >= 0) nums1[k -- ] = nums1[i -- ];
        while (j >= 0) nums1[k -- ] = nums2[j -- ];
    }
};
```
#####  LeetCode 278. First Bad Version
```C++
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r)
        {
            int mid = (l + 0ll + r) / 2;
            if (isBadVersion(mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
#####  LeetCode 98. Validate Binary Search Tree
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
    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        int maxv, minv;
        return dfs(root, maxv, minv);
    }

    bool dfs(TreeNode* root, int &maxv, int &minv)
    {
        maxv = minv = root->val;
        if (root->left)
        {
            int nowMaxv, nowMinv;
            if (!dfs(root->left, nowMaxv, nowMinv)) return false;
            if (nowMaxv >= root->val) return false;
            maxv = max(maxv, nowMaxv);
            minv = min(minv, nowMinv);
        }
        if (root->right)
        {
            int nowMaxv, nowMinv;
            if (!dfs(root->right, nowMaxv, nowMinv)) return false;
            if (nowMinv <= root->val) return false;
            maxv = max(maxv, nowMaxv);
            minv = min(minv, nowMinv);
        }
        return true;
    }
};
```
#####  LeetCode 173. Binary Search Tree Iterator
```C++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class BSTIterator {
public:
    stack<TreeNode*> sta;

    BSTIterator(TreeNode *root) {
        while (root)
        {
            sta.push(root);
            root = root->left;
        }
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !sta.empty();
    }

    /** @return the next smallest number */
    int next() {
        TreeNode *t = sta.top();
        sta.pop();
        for (TreeNode *p = t->right; p; p = p->left)
            sta.push(p);
        return t->val;
    }
};

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = BSTIterator(root);
 * while (i.hasNext()) cout << i.next();
 */
```
#####  LeetCode 238. Product of Array Except Self
```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> res(nums.size());
        for (int i = 0, t = 1; i < nums.size(); i ++ )
        {
            res[i] = t;
            t *= nums[i];
        }
        for (int i = nums.size() - 1, t = 1; i >= 0; i -- )
        {
            res[i] *= t;
            t *= nums[i];
        }
        return res;
    }
};
```
#####  LeetCode 56. Merge Intervals
```C++
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Solution {
public:
    vector<Interval> merge(vector<Interval>& _intervals) {
        vector<pair<int, int>> intervals;
        for (auto &interval : _intervals)
            intervals.push_back(make_pair(interval.start, interval.end));
        sort(intervals.begin(), intervals.end());
        int st = INT_MIN, ed = INT_MIN;
        vector<Interval> res;
        for (int i = 0; i < intervals.size(); i ++ )
        {
            if (intervals[i].first > ed)
            {
                if (st != INT_MIN || ed != INT_MIN)
                    res.push_back(Interval(st, ed));
                st = intervals[i].first, ed = intervals[i].second;
            }
            else ed = max(ed, intervals[i].second);
        }

        if (st != INT_MIN || ed != INT_MIN) res.push_back(Interval(st, ed));
        return res;
    }
};
```
#####  LeetCode 75. Sort Colors
```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int zero = 0, second = nums.size() - 1;
        for (int i = 0; i <= second; i ++ )
        {
            while (nums[i] == 2 && i < second) swap(nums[i], nums[second -- ]);
            if (nums[i] == 0) swap(nums[i], nums[zero ++ ]);
        }
    }
};
```
#####  LeetCode 133. Clone Graph
```C++
/**
 * Definition for undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */
class Solution {
public:
    unordered_map<UndirectedGraphNode*, UndirectedGraphNode*> hash;

    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if (!node) return 0;
        auto root = new UndirectedGraphNode(node->label);
        hash[node] = root;
        dfs(node);
        return root;
    }

    void dfs(UndirectedGraphNode* node)
    {
        for (auto &neighbor : node->neighbors)
        {
            if (!hash.count(neighbor))
            {
                hash[neighbor] = new UndirectedGraphNode(neighbor->label);
                dfs(neighbor);
            }
            hash[node]->neighbors.push_back(hash[neighbor]);
        }
    }
};
```
#####  LeetCode 76. Minimum Window Substring
```C++
class Solution {
public:
    string minWindow(string s, string t) {
        string res;
        unordered_map<char, int> S, T;
        int total = t.size();
        for (auto &c : t)
        {
            T[c] ++ ;
            if (T[c] > 1) total -- ;
        }
        int satisfy = 0;
        for (int i = 0, j = 0; i < s.size(); i ++ )
        {
            S[s[i]] ++ ;
            if (S[s[i]] == T[s[i]]) satisfy ++ ;
            while (j <= i && S[s[j]] > T[s[j]])
            {
                S[s[j]] -- ;
                j ++ ;
            }
            if (satisfy >= total && (res.empty() || i - j + 1 < res.size()))
                res = s.substr(j, i - j + 1);
        }
        return res;
    }
};
```

### Week3 Google面试题

#####  LeetCode 66. Plus One
```C++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int t = 1;
        for (int i = digits.size() - 1; i >= 0; i -- )
        {
            digits[i] += t;
            t = digits[i] / 10;
            digits[i] %= 10;
        }
        if (t)
        {
            digits.push_back(0);
            for (int i = digits.size() - 2; i >= 0; i -- )
                digits[i + 1] = digits[i];
            digits[0] = 1;
        }
        return digits;
    }
};
```
#####  LeetCode 326. Power of Three
```C++
class Solution {
public:
    bool isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
};
```


#####  LeetCode 230. Kth Smallest Element in a BST
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
    int kthSmallest(TreeNode* root, int k) {
        return dfs(root, k);
    }

    int dfs(TreeNode *root, int &k) {
        if (!root) return 0;
        int left = dfs(root->left, k);
        if (!k) return left;
        k -- ;
        if (!k) return root->val;
        return dfs(root->right, k);
    }
};
```
#####  LeetCode 139. Word Break
```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_map<string, int> dict;
        for (auto &word : wordDict) dict[word] = 1;
        int n = s.size();
        vector<bool>f(n + 1, true);
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = false;
            for (int j = 0; j < i; j ++ )
                if (dict[s.substr(j, i - j)] && f[j])
                {
                    f[i] = true;
                    break;
                }
        }
        return f[n];
    }
};
```
#####  LeetCode 930. Binary Subarrays With Sum
```C++
class Solution {
public:
    int numSubarraysWithSum(vector<int>& A, int S) {
        int n = A.size();
        vector<int> sum(n + 1, 0), f(n + 1, 0); // f[i] 记录前面有多少个和为i的前缀
        for (int i = 1; i <= n; i ++ ) sum[i] = sum[i - 1] + A[i - 1];
        f[0] = 1;
        int res = 0;
        for (int i = 1; i <= n; i ++ )
        {
            int s = sum[i];
            if (s >= S) res += f[s - S];
            f[s] ++ ;
        }
        return res;
    }
};
```
#####  LeetCode 228. Summary Ranges
```C++
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> res;
        int st, ed;
        for (int i = 0; i < nums.size(); i ++ )
        {
            int x = nums[i];
            if (!i) st = ed = x;
            else if (x == ed + 1) ed ++ ;
            else
            {
                res.push_back(to_string(st) + (st == ed ? "" : "->" + to_string(ed)));
                st = ed = x;
            }
        }
        if (nums.size()) res.push_back(to_string(st) + (st == ed ? "" : "->" + to_string(ed)));
        return res;
    }
};
```
#####  LeetCode 57. Insert Interval
```C++
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */

class Solution {
public:
    vector<Interval> insert(vector<Interval>& intervals, Interval newInterval) {
        vector<Interval> res;
        bool has_in = false;
        for (auto interval : intervals)
        {
            if (interval.start > newInterval.end)
            {
                if (!has_in)
                {
                    res.push_back(newInterval);
                    has_in = true;
                }
                res.push_back(interval);
            }
            else if (interval.end < newInterval.start)
            {
                res.push_back(interval);
            }
            else
            {
                newInterval.start = min(newInterval.start, interval.start);
                newInterval.end = max(newInterval.end, interval.end);
            }
        }
        if (!has_in) res.push_back(newInterval);
        return res;
    }
};
```
#####  LeetCode 224. Basic Calculator
```C++
class Solution {
public:

    void calc(stack<char> &op, stack<int> &num) {
        int y = num.top();
        num.pop();
        int x = num.top();
        num.pop();
        if (op.top() == '+') num.push(x + y);
        else num.push(x - y);
        op.pop();
    }

    int calculate(string s) {
        stack<char> op;
        stack<int> num;
        for (int i = 0; i < s.size(); i ++ ) {
            char c = s[i];
            if (c == ' ') continue;
            if (c == '+' || c == '-' || c == '(') op.push(c);
            else if (c == ')') {
                op.pop();
                if (op.size() && op.top() != '(') {
                    calc(op, num);
                }
            }
            else {
                int j = i;
                while (j < s.size() && isdigit(s[j])) j ++ ;
                num.push(atoi(s.substr(i, j - i).c_str()));
                i = j - 1;
                if (op.size() && op.top() != '(') {
                    calc(op, num);
                }
            }
        }
        return num.top();
    }
};
```
#####  LeetCode 883. Projection Area of 3D Shapes
```C++
class Solution {
public:
    int projectionArea(vector<vector<int>>& grid) {
        if (grid.empty() || grid[0].empty()) return 0;
        int res = 0;
        int n = grid.size(), m = grid[0].size();
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                if (grid[i][j])
                    res ++ ;
            }

        for (int i = 0; i < n; i ++ ) {
            int h = 0;
            for (int j = 0; j < m; j ++ ) {
                h = max(h, grid[i][j]);
            }
            res += h;
        }

        for (int j = 0; j < m; j ++ ) {
            int h = 0;
            for (int i = 0; i < n; i ++ ) {
                h = max(h, grid[i][j]);
            }
            res += h;
        }

        return res;
    }
};
```
#####  LeetCode 857. Minimum Cost to Hire K Workers
```C++
class Solution {
public:
    double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int K) {
        vector<pair<double, int>> workers;
        int n = quality.size();
        for (int i = 0; i < n; i ++ ) workers.push_back({wage[i] * 1.0 / quality[i], quality[i]});
        sort(workers.begin(), workers.end());
        priority_queue<int> heap;
        int s = 0;
        double res = 1e9;
        for (int i = 0; i < n; i ++ ) {
            int q = workers[i].second;
            s += q;
            heap.push(q);
            if (heap.size() > K) {
                s -= heap.top();
                heap.pop();
            }
            if (heap.size() >= K)
                res = min(res, s * workers[i].first);
        }
        return res;
    }
};

```
