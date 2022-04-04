##### Leetcode 116 填充每个节点的下一个右侧节点指针

使用已建立的next 指针，位于第N层时为第N+1层建立next 指针

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

#### Leetcode 117  填充每个节点的下一个右侧节点指针 II

位于第N层时为第N+1层建立next 指针

```C++
class Solution {
public:
    Node *connect(Node *root) {
        if (!root) return nullptr;
        auto cur = root;
        while (cur) {
            Node *head = new Node(-1);
            auto tail = head;
            //遍历一层,建立下一层
            for (auto p = cur; p; p = p->next) {
                if (p->left) tail = tail->next = p->left;
                if (p->right) tail = tail->next = p->right;
            }
            //挪到下一层最左
            cur = head->next;
        }
        return root;
    }
};
```

