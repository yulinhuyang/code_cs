

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


## 71 简化路径





