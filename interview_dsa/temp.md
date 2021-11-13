##### 221. 最大正方形

```C++
class Solution {
public:
    int maximalSquare(vector<vector<char>> &matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));

        //求边长
        int maxSide = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '1') {
                    //左上边界好填，直接填上
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = min(dp[i - 1][j], min(dp[i][j - 1], dp[i - 1][j - 1])) + 1;
                    }
                }
                maxSide = max(maxSide, dp[i][j]);
            }
        }
        return maxSide * maxSide;
    }
};

```


#####  226. 翻转二叉树

```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr){
            return root;
        }
        TreeNode * right = invertTree(root->right);
        TreeNode * left = invertTree(root->left);

        root->left = right;
        root->right = left;
        return root;
    }
};
```

##### 234. 回文链表

nullptr和默认值不一样，默认的值为0

```C++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == nullptr || head->next == nullptr){
            return true;
        }

        ListNode* fast = head;
        ListNode* slow = head;
        ListNode* pre = head;
        //nullptr和默认值不一样，默认的值为0
        ListNode* prepre = nullptr;

        while(fast && fast->next){
            //边走边反转
            pre = slow;
            fast = fast->next->next;
            slow = slow->next;
            pre->next = prepre;
            prepre = pre;
        }
        if(fast){
            slow = slow->next;          
        }
        while(pre && slow){
            if(pre->val != slow->val){
                return false;
            }
            pre = pre->next;
            slow = slow->next;
        }
        return true;
    }
};
```



