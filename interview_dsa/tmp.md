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

##### 560. 和为 K 的子数组

前缀和+ hash

```cpp
class Solution {
public:
    int subarraySum(vector<int> &nums, int k) {
        //前缀和简化，连续子数组
        int m = nums.size();
        int count = 0;
        vector<int> preSum(m + 1, 0);
        unordered_map<int, int> preMap;
        preMap[0] = 1;
        for (int i = 1; i < m + 1; i++) {
            preSum[i] = preSum[i - 1] + nums[i - 1];
            if (preMap.find(preSum[i] - k) != preMap.end()) {
                count += preMap[preSum[i] - k];
            }
            preMap[preSum[i]]++;
        }
        
        return count;
    }
};

```

前缀和简化为一个数

```cpp
class Solution {
public:
    int subarraySum(vector<int> &nums, int k) {
        //前缀和简化，连续子数组
        int m = nums.size();
        int count = 0;
        unordered_map<int, int> preMap;
        preMap[0] = 1;
        int pre = 0;
        for (auto num:nums) {
            pre = pre + num;
            if (preMap.find(pre - k) != preMap.end()) {
                count += preMap[pre - k];
            }
            preMap[pre]++;
        }

        return count;
    }
};
```



