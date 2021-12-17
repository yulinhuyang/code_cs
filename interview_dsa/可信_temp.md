LeetCode 算法题讲解视频 Up 主推荐： https://learnku.com/articles/40491

# 方法

## 前缀和与差分数组

一维前缀和： S[i]= S[i-1]  + A[i]

二维前缀和： S[i][j] = S[i-1][j] + S[i][j-1] – S[i-1][j-1] + A[i][j]

树上前缀和：从根到某节点的路径上点（或边）的值之和（上到下）；某节点及其所有子节点（或边）的值之和（下到上）


差分数组定义

真实数组a = {a[1]、a[2]、…、a[n]}          // 各点真实数据

差分数组df = {df[1]、df[2]、…、df[n]}      // 各点数据的变更值 

df[i] = a[i] - a[i-1]                      // 差分数组各点数据为真实数据的变更值

a[i] = df[1] + df[2] …+ df[i]              // 差分数组的前缀和即为真实数组

a[i] = a[i-1] + df[i]                      // 真实数据也可以从上一点数据+变更值求出


## 优先队列

优先队列的入和出与元素（数据）进的次序无关，而是由设定的优先级来决定元素的弹出次序，优先级最高的元素最先得到服务，优先级相同的元素按照其在优先队列中的顺序得到服务

实现优先队列的方式有多种，如数组、链表、平衡二叉树（如：AVL树和RB树）、堆等

堆是一种完全二叉树，可以分为小顶堆和大顶堆。小顶堆指的是堆顶元素（即树的根节点）为堆中最小值，且每一个节点的值都必须 小于等于 其孩子节点的值；反之即大顶堆

对堆的操作主要有两个，即插入或删除，且都是在顶端进行。由于堆是一个完全二叉树，删除和添加后都需要保证是完全二叉树，一般情况是先和最后一个节点进行交换，然后再进行删除和添加。

C++	priority_queue	push、top、pop、empty	系统自带	大顶堆
 
Python	heapq	heappush、heappop	系统自带	小顶堆

可以使用可自动排序的map进行替代，也能够达到减少时间复杂度的目的。如  C++(map)、 Python(SortedDict)

【c++】STL里的priority_queue用法总结: https://blog.csdn.net/xiaoquantouer/article/details/52015928


# 题目

## 337. 打家劫舍 III

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

## 71 简化路径

链表标准形式

括号与栈、栈混洗



