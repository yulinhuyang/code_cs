Prim 算法，YYDS: https://blog.csdn.net/fdl123456/article/details/122375113

##### 538. 把二叉搜索树转换为累加树

```cpp
class Solution {
    int sum = 0;
    void traverse(TreeNode* root){
        if(!root){
            return;
        }
        traverse(root->right);
        sum += root->val;
        root->val = sum;
        traverse(root->left);
    }
public:
    TreeNode* convertBST(TreeNode* root) {
        traverse(root);
        return root;
    }
};
```
##### 543. 二叉树的直径

```cpp
class Solution {
    int ans = 0;
    int dfs(TreeNode *root) {
        if (!root) {
            return 0;
        }
        int L = dfs(root->left);
        int R = dfs(root->right);
        ans = max(ans, L + R);
        return max(L, R) + 1;
    }

public:
    int diameterOfBinaryTree(TreeNode *root) {
        dfs(root);
        return ans;
    }
};
```
