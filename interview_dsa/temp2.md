##### Leetcode 116 填充每个节点的下一个右侧节点指针

```C++
class Solution {
public:
    Node *connect(Node *root) {
        if (!root) return nullptr;

        auto res = root;
        while (root->left) {
            //遍历一层
            for (auto p = root; p; p = p->next) {
                p->left->next = p->right;
                if(p->next) p->right->next = p->next->left;
            }
            //挪到下一层最左
            root = root->left;
        }
        return res;
    }
};

```
