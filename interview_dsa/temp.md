##### Leetcode 66 加一

高精度加法模板

```C++
class Solution {
public:
    vector<int> plusOne(vector<int> &digits) {
        int m = digits.size();
        vector<int> res;
        int carry = 1, num = 0;
        for (int i = m - 1; i >= 0; i--) {
            num = (digits[i] + carry) % 10;
            carry = (digits[i] + carry) / 10;
            res.emplace_back(num);
        }
        if (carry) res.emplace_back(1);
        reverse(res.begin(), res.end());
        return res;
    }
};

```
##### Leetcode 73 矩阵置零

```C++
class Solution {
public:
    void setZeroes(vector<vector<int>> &matrix) {
        //标记变量+自标记法
        int m = matrix.size(), n = matrix[0].size();
        int row0 = 0, col0 = 0;
        for (int i = 0; i < m; i++) {
            if (!matrix[i][0]) col0 = 1;
        }
        for (int j = 0; j < n; j++) {
            if (!matrix[0][j]) row0 = 1;
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (!matrix[i][j]) {
                    matrix[i][0] = 0, matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (!matrix[i][0] || !matrix[0][j]) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (row0) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
        if (col0) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }

    }
};

```

#####  Leetcode 80 删除有序数组中的重复项 II

模拟：跳重范式：while(i + 1 < nums.size() && nums[i] == nums[i+1]) i++;

```C++
//class Solution {
public:
    int removeDuplicates(vector<int> &nums) {

        int j = 0;
        for (int i = 0; i < nums.size();) {
            if (i + 1 < nums.size() && nums[i] == nums[i + 1]) {
                nums[j++] = nums[i++];
                nums[j++] = nums[i++];
                while (i < nums.size() && nums[j - 1] == nums[i]) i++;
            } else {
                nums[j++] = nums[i++];
            }
        }
        return j;
    }
};

//极简
class Solution {
public:
    int removeDuplicates(vector<int> &nums) {
        int k = 0;
        for (auto &x:nums) {
            if (k < 2 || nums[k - 1] != x || nums[k - 2] != x) {
                nums[k++] = x;
            }
        }
        return k;
    }
};
```

##### Leetcode 81 搜索旋转排序数组 II

跳结尾重复 ————> 二分右边界搜索寻找旋转点————> 二分左边界搜索

二分查找：寻找满足check条件的左边界或者右边界。


```C++
class Solution {
public:
    bool search(vector<int> &nums, int target) {
        int m = nums.size() - 1, n = 0;
        //跳过重复
        while (m >= 0 && nums[m] == nums[0]) m--;
        //剪枝
        if (m < 0) return nums[0] == target;
        //寻找旋转点
        int l = 0, r = m;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            //满足条件的右边界
            if (nums[mid] >= nums[0]) l = mid;
            else r = mid - 1;
        }
        if (nums[l] == target) return true;
        if (target >= nums[0]) {
            r = l, l = 0;
        } else {
            r = m, l++;
        }
        //l和r可能相等
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return nums[r] == target;
    }
};

```

##### Leetcode 82  删除排序链表中的重复元素 II

把链表当数组看到，特殊处理(提前取)取索引的方式。

```C++
class Solution {
public:
    ListNode *deleteDuplicates(ListNode *head) {
        if (!head) return head;
        auto dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *cur = dummy;
        while (cur->next && cur->next->next) {
            if (cur->next->val == cur->next->next->val) {
                int val = cur->next->val;
                while (cur->next && cur->next->val == val) {
                    cur->next = cur->next->next;
                }
            } else {
                cur = cur->next;
            }
        }
        return dummy->next;
    }
};

```

##### Leetcode 86  分隔链表

大小链表存储，再链接，截断

```C++
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        auto small = new ListNode(-1);
        auto large = new ListNode(-1);
        auto sh = small,lh = large;
        for(auto p = head;p;p = p->next){
            if(p->val < x) {
                small->next = p;
                small = small->next;
            }
            else {
                large->next = p;
                large = large->next;
            }
        }
        small->next = lh->next;
        //链表截断
        large->next = nullptr;
        return sh->next;
    }
};
```
##### Leetcode 89 格雷编码

```C++
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> res(1, 0);
        while (n--) {
            for (int i = res.size() - 1; i >= 0; i--) {
                res[i] <<= 1;
                res.emplace_back(res[i] + 1);
            }
        }
        return res;
    }
};
```
##### Leetcode 97 交错字符串

字符串dp

```C++
class Solution {

public:
    bool isInterleave(string s1, string s2, string s3) {
        if (s1.size() + s2.size() != s3.size()) {
            return false;
        }
        int m = s1.size(), n = s2.size();
        vector<vector<int>> f(m + 1, vector<int>(n + 1, 0));
        s1 = " " + s1, s2 = " " + s2, s3 = " " + s3;
        f[0][0] = true;
        for (int i = 1; i < m + 1; i++) {
            if (s1[i] == s3[i]) f[i][0] = f[i][0] || f[i - 1][0];
        }
        for (int j = 1; j < n + 1; j++) {
            if (s2[j] == s3[j]) f[0][j] = f[0][j] || f[0][j - 1];
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (s1[i] == s3[i + j]) f[i][j] = f[i][j] || f[i - 1][j];
                if (s2[j] == s3[i + j]) f[i][j] = f[i][j] || f[i][j - 1];
            }
        }
        return f[m][n];
    }
};

```
##### Leetcode  99 恢复二叉搜索树

迭代中序遍历，默认升序 -> 找逆序元素

```C++
class Solution {
public:
    void recoverTree(TreeNode *root) {
        TreeNode *x = nullptr;
        TreeNode *y = nullptr;
        TreeNode *pred = nullptr;
        stack<TreeNode *> stk;
        while (!stk.empty() || root != nullptr) {
            while (root) {
                stk.emplace(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            if (pred != nullptr && root->val < pred->val) {
                y = root;
                if (x == nullptr) {
                    x = pred;
                } else break;
            }

            pred = root;
            root = root->right;
        }
        swap(x->val, y->val);
    }

};

```

