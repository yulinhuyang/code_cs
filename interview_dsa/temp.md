LeetCode 算法题讲解视频 Up 主推荐： https://learnku.com/articles/40491

##### 337. 打家劫舍 III

C++ 使用结构体返回多个值

树的递归与选择最大

```C++
struct SubtreeStatus {
    int select;
    int noSelect;
};


class Solution {
public:
    SubtreeStatus dfs(TreeNode *root) {
        if (root == nullptr) {
            return {0, 0};
        }

        auto leftValue = dfs(root->left);
        auto rightValue = dfs(root->right);
        int select = root->val + leftValue.noSelect + rightValue.noSelect;
        int noSelect = max(leftValue.select, leftValue.noSelect) + max(rightValue.select, rightValue.noSelect);

        return {select, noSelect};
    }

    int rob(TreeNode *root) {
        auto res = dfs(root);
        return max(res.noSelect, res.select);
    }
};


```










链表标准形式

括号与栈、栈混洗

一维前缀和：S[i]= S[i-1] +A[i]

二维前缀和： S[i][j] = S[i-1][j] + S[i][j-1] – S[i-1][j-1] + A[i][j]

树上前缀和：从根到某节点的路径上点（或边）的值之和（上到下）；某节点及其所有子节点（或边）的值之和（下到上）



## 71 简化路径





