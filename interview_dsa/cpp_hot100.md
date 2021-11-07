###   1 链表（栈 队列 堆 ）

#### 链表

熟练掌握 链表的反转、合并


##### 2. 两数相加

```C++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        //nullptr类型
        ListNode *head = nullptr;
        ListNode *tail = nullptr;
        int carry = 0;
        while(l1 || l2){
            int l1_val = l1?l1->val:0;
            int l2_val = l2?l2->val:0;

            int sum = l1_val + l2_val + carry;
            carry = sum/10;
            if(!head){
                //理解new的返回类型
                tail = head = new ListNode(sum%10);
            }else{
                tail->next = new ListNode(sum%10);
                tail = tail->next;
            }
            if(l1){
                l1 = l1->next;
            }
            if(l2){
                l2 = l2->next;
            }
        }
        if(carry > 0){
            tail->next = new ListNode(carry);
        }

        return head;
    }
};

```





##### 19 删除链表的倒数第 N 个结点

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {

        //dummy 便于处理一个节点的情况
        ListNode *dummy = new ListNode(0,head);
        ListNode *fast = head;
        ListNode *slow = dummy;
        int index = 0;
        while(index < n){
            fast = fast->next;
            index++;
        }

        while(fast){
            fast = fast->next;
            slow = slow->next;
        }

        slow->next = slow->next->next;
        ListNode *ans = dummy->next;
        delete dummy;
        return ans;

    }
};

```
##### 21 合并两个有序链表

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *dummy = new ListNode(0);
        ListNode *head = dummy;
        while(l1 && l2){
            if(l1->val < l2->val){
                head->next = l1;
                l1 = l1->next;
            } else{
                head->next = l2;
                l2 = l2->next;
            }
            head = head->next;
        }
        if(l1){
            head->next = l1;
        }
        if(l2){
            head->next = l2;
        }
        return  dummy->next;
```
##### 23 合并K个升序链表

```c++
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.size() == 0){
            return nullptr;
        }

        ListNode *head = nullptr;
        for(int i = 0;i < lists.size();i++){
            head = mergeTwoLists(head,lists[i]);
        }
        return head;
    }

    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {

        ListNode *dummy = new ListNode(0);
        ListNode *head = dummy;
        while(l1 && l2){
            if(l1->val < l2->val){
                head->next = l1;
                l1 = l1->next;
                head = head->next;
            }else{
                head->next = l2;
                l2 = l2->next;
                head = head->next;
            }
        }
        if(l1){
            head->next = l1;
        }
        if(l2){
            head->next = l2;
        }
        return dummy->next;
    }
};

```


##### 141 环形链表

```C++

class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr){
            return false;
        }

        ListNode *slow = head;
        ListNode *fast = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
            if(fast==slow){
                return true;
            }
        }

        return false;
    }
};

```
##### 142. 环形链表 II

```C++							  
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head== nullptr || head->next == nullptr){
            return nullptr;
        }
        ListNode *slow = head;
        ListNode *fast = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast){
                break;
            }
        }

        if(slow != fast){
            return nullptr;
        }

        slow = head;
        while(slow != fast){
            fast = fast->next;
            slow = slow->next;
        }
        return fast;
    }
};
```
##### 146. LRU 缓存机制

双向链表(添加删除) + hashMAP（保存key + 地址）

双向链表：时间复杂度是O(1)，删除需要前驱节点。

struct Dnode --> Dnode List定义(伪头伪尾) + LRU定义 -——>Dnode API(delete、delte_tail、add_head)  -> LRU API get put 函数
				
				
```C++

//STL版,看思路，分解替换

class LRUCache {
private:

    int capacity;
    //cachelist 和cache map同步变化
    list<pair<int, int>> cacheList;// pair内为key、value
    unordered_map<int, list<pair<int, int>>::iterator> cacheMap;

public:
    LRUCache(int capacity) {
        this->capacity = capacity;
    }

    int get(int key) {
        if (!cacheMap.count(key)) {
            return -1;
        }
        //*取值迭代器
        auto it = *cacheMap[key];
        //删除迭代器
        cacheList.erase(cacheMap[key]);
        cacheList.push_front(it);
        cacheMap[key] = cacheList.begin();
        return cacheList.front().second;
    }

    void put(int key, int value) {
        //已存在
        if (cacheMap.count(key)) {
            auto it = *cacheMap[key];
            cacheList.erase((cacheMap[key]));
            it.second = value;
            cacheList.push_front(it);
            cacheMap[key] = cacheList.begin();
        } else {
            if (cacheList.size() == capacity) {
                cacheMap.erase(cacheList.back().first);
                cacheList.pop_back();
            }
            cacheList.push_front(make_pair(key, value));
            cacheMap[key] = cacheList.begin();
        }
        return;

    }
};


//Dlist 定义版

申请与释放

DListNode *head = new DListNode();

delete(head):

正常删除节点,都要delete node释放内存，但这里如果后面addhead,则不用释放

delete tail的时候，需要释放内存


struct DListNode {
    int key;
    int val;
    DListNode *prev;
    DListNode *next;

    DListNode() : key(0), val(0), prev(NULL), next(NULL) {};

    DListNode(int key, int val) : key(key), val(val), prev(NULL), next(NULL) {};
};


class LRUCache {
private:
    //Dlist定义
    DListNode *head = new DListNode();
    DListNode *tail = new DListNode();
    int size;
    //类相关定义，list 和cache map同步变化
    unordered_map<int, DListNode *> cacheMap;
    int capacity;

public:
    LRUCache(int capacity) {
        //伪头 伪尾
        head->next = tail;
        tail->prev = head;
        this->size = 0;
        this->capacity = capacity;
    }

    void addHead(DListNode *node) {
        //先插后断
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }

    //正常删除节点,都要delete释放内存，但这里如果后面addhead,则不用释放
    void deleteNode(DListNode *node) {
        node->next->prev = node->prev;
        node->prev->next = node->next;
    }

    //delete tail的时候，先断关系，再释放内存
    DListNode *deleteTail() {
        DListNode *node = tail->prev;
        deleteNode(node);
        return node;
    }

    int get(int key) {
        if (!cacheMap.count(key)) {
            return -1;
        }
        //*取值迭代器
        DListNode *node = cacheMap[key];
        //删除迭代器
        deleteNode(node);
        addHead(node);
        //cacheMap[key] = node;
        return node->val;
    }

    void put(int key, int value) {
        //已存在
        if (cacheMap.count(key)) {
            DListNode *node = cacheMap[key];
            deleteNode(node);
            node->val = value;
            addHead(node);
            //cacheMap[key] = node;
        } else {
            DListNode *node = new DListNode(key, value);
            addHead(node);
            cacheMap[key] = node;
            size++;
            if (size > capacity) {
                DListNode *tail = deleteTail();
                cacheMap.erase(tail->key);
                delete tail;
                size--;
            }
        }
    }
};

```

##### 148. 排序链表

复杂问题，设计拆解

find middle --> sortList 递归 --> merge list

链表的经典模块组装

```C++

class Solution {
public:
    ListNode *sortList(ListNode *head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        ListNode *middle = findMiddle(head);

        ListNode *head2 = middle->next;
        middle->next = nullptr;
        ListNode *L1 = sortList(head);
        ListNode *L2 = sortList(head2);

        return merge(L1, L2);
    }

    ListNode *findMiddle(ListNode *head) {
        if(!head){
            return nullptr;
        }

        ListNode *fast = head->next;
        ListNode *slow = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    ListNode *merge(ListNode *L1, ListNode *L2) {
        ListNode *dummy = new ListNode(0);
        ListNode *head = dummy;
        while (L1 && L2) {
            if (L1->val < L2->val) {
                head->next = L1;
                L1 = L1->next;
            } else {
                head->next = L2;
                L2 = L2->next;
            }
            head = head->next;
        }
        if(L1) {
            head->next = L1;
        } 
        if(L2){
            head->next = L2;
        }

        return dummy->next;
    }
};
```

##### 160. 相交链表


```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        ListNode* nodeA = headA;
        ListNode* nodeB = headB;
        while(nodeA != nodeB){
            if(nodeA){
               nodeA = nodeA->next;
            } else{
                nodeA = headB;
            }
            if(nodeB){
                nodeB = nodeB->next;
            } else{
                nodeB = headA;
            }
        }

        return nodeA;
    }
};

```
##### 206. 反转链表 

递归法

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr){
            return head;
        }

        ListNode* node = reverseList(head->next);
        //反转且避免成环
        head->next->next = head;
        head->next = nullptr;
        return node;
    }
};
```


迭代法：pre cur next

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr){
            return head;
        }
        ListNode* pre = nullptr;
        ListNode* cur = head;
        while(cur){
            //pre cur next的使用
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }

        return pre;
    }
};
```

#### 栈 stack

括号类问题、单调栈问题

##### 20 有效的括号

```c++
class Solution {
public:
    bool isValid(string s) {
        if (s.size() % 2 == 1) {
            return false;
        }
        stack<char> stack_c;
        unordered_map<char, char> pairs = {
                {')', '('},
                {']', '['},
                {'}', '{'}
        };
        for (auto ch:s) {
            if (pairs.count(ch)) {
                if((!stack_c.empty())&&(stack_c.top() == pairs[ch])){
                    stack_c.pop();
                }else{
                    return false;
                }
            } else {
                stack_c.push(ch);
            }
        }
        return stack_c.empty();
    }
};
```


##### 32. 最长有效括号

栈

辅助位置-1；（ 入栈，）出栈

```C++

class Solution {
public:
    int longestValidParentheses(string s) {

        stack<int> stack;
        int ans = 0;
        stack.push(-1);
        for(int i = 0;i < s.size();i++){
            if(s[i] == '('){
                stack.push(i);
            }else{
                stack.pop();
                if(stack.empty()){
                    stack.push(i);
                } else {
                    ans = max(ans, i - stack.top());
                }
            }
        }
        return  ans;
    }
};

```

##### 84 柱状图中最大的矩形

单调栈

ans + stack辅助

stack辅助：使得每次新元素入栈后，栈内的元素都保持有序

stack: 存坐标、存值，单调升、单调降

左右双栈：左一遍、右一遍

```C++

class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n,0);
        vector<int> right(n,0);
        stack<int> left_stack;
        stack<int> right_stack;
        for(int i = 0;i < heights.size();i++){
            while ((!left_stack.empty()) &&(heights[left_stack.top()] >= heights[i])){
                left_stack.pop();
            }
            left[i] = left_stack.empty()? -1:left_stack.top();
            left_stack.push(i);
        }
		
		//可以清空 left_stack = stack<int>()
        for(int j = n - 1;j > -1;j--){
            while ((!right_stack.empty()) &&(heights[right_stack.top()] >= heights[j])){
                right_stack.pop();
            }
            right[j] = right_stack.empty()?n:right_stack.top();
            right_stack.push(j);
        }
        
        int maxArea = 0;
        for(int i = 0;i < n;i++){
            maxArea = max(maxArea,(right[i] - left[i] - 1) *heights[i]);
        }
        return  maxArea;
    }
};

```
##### 85. 最大矩形

矩形面积，列加分解 + 单调栈

```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n,0);
        vector<int> right(n,0);
        stack<int> left_stack;
        stack<int> right_stack;
        for(int i = 0;i < heights.size();i++){
            while ((!left_stack.empty()) &&(heights[left_stack.top()] > heights[i])){
                left_stack.pop();
            }
            left[i] = left_stack.empty()? -1:left_stack.top();
            left_stack.push(i);
        }

        for(int j = n - 1;j > -1;j--){
            while ((!right_stack.empty()) &&(heights[right_stack.top()] > heights[j])){
                right_stack.pop();
            }
            right[j] = right_stack.empty()?n:right_stack.top();
            right_stack.push(j);
        }

        int maxArea = 0;
        for(int i = 0;i < n;i++){
            maxArea = max(maxArea,(right[i] - left[i] - 1) *heights[i]);
        }
        return  maxArea;
    }

    int maximalRectangle(vector<vector<char>> matrix) {

        int m = matrix.size();
        int n = matrix[0].size();
        vector<int> heights(n,0);
        int maxArea;
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                if(matrix[i][j] == '1'){
                    heights[j] += 1;
                }
            }
            int area = largestRectangleArea(heights);
            maxArea = max(maxArea,area);
        }
        return maxArea;
    }
};
```


##### 155. 最小栈

```C++
class MinStack {
    vector<int> commonStack;
    vector<int> minStack;
public:
    MinStack() {
    }

    void push(int val) {
        commonStack.emplace_back(val);
        if(minStack.empty()){
            minStack.emplace_back(val);
        }
        else{
			#存储非升序
            minStack.emplace_back(min(val,minStack.back()));
        }
    }

    void pop() {
        commonStack.pop_back();
        minStack.pop_back();
    }

    int top() {
        return  commonStack.back();
    }

    int getMin() {
        return  minStack.back();
    }
};
```

#### 堆 heap 队列

### 2 树


##### 94. 二叉树的中序遍历

树天然递归

```C++
class Solution {
public:
    void inorder(TreeNode* root,vector<int> &ans){
        if(!root){
            return;
        }
        inorder(root->left,ans);
        ans.emplace_back(root->val);
        inorder(root->right,ans);
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        inorder(root,ans);
        return ans; 
    }
};
```

##### 96. 不同的二叉搜索树

二叉搜索树形状递归，向上比较


```C++

class Solution {
public:
    int numTrees(int n) {
        //G(n) = 求和 F(i,n)
        //F(i,n) = G(i-1)*G(n-i)
        //G(i,n) = 求和 G(i-1)*G(n-i)
        
        vector<int> G(n+1,0);
        G[0] = 1;
        G[1] = 1; 
        for(int i = 2;i < n + 1;i++){
            for(int j = 1;j <= i;j++){
                G[i] += G[j-1]*G[i-j];
            }
        }
        return G[n];
    }
};

```

##### 98 验证二叉搜索树

注意二叉搜索树，左子树所有节点都小于当前节点的值，右子树所有节点都大于当前节点的值。

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if(root == nullptr){
            return true;
        }
        return isValidBST(root,LONG_MIN,LONG_MAX);
    }

    bool isValidBST(TreeNode* root,long long minVal,long long maxVal){
        if(root == nullptr){
            return true;
        }
        if(root->val <= minVal){
            return false;
        }
        if(root->val >= maxVal){
            return false;
        }

        return isValidBST(root->left,minVal,root->val) && isValidBST(root->right,root->val,maxVal);
    }
};

```


##### 101. 对称二叉树

壳 + 递归函数 

```C++
class Solution {
public:
    bool recur(TreeNode* left,TreeNode* right){
        if(left == nullptr && right ==nullptr){
            return true;
        }
        if(left == nullptr || right ==nullptr){
            return false;
        }
        if(left->val != right->val){
            return false;
        }
        return recur(left->right,right->left) && recur(left->left,right->right);
    }
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr){
            return true;
        }
        return recur(root->left,root->right);
    }
};
```

##### 102 二叉树的层序遍历

queue<TreeNode *> 结构

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode *root) {
        vector<vector<int>> ans;
        if (!root) {
            return ans;
        }
        
        queue<TreeNode *> q;
        q.push(root);
        while (!q.empty()) {
            int len = q.size();
            vector<int> oneLevel;
            for (int i = 0; i < len; i++) {
                auto node = q.front();
                q.pop();
                
                oneLevel.emplace_back(node->val);
                if (node->left){
                    q.push(node->left);
                }
                if (node->right) {
                    q.push(node->right);
                }
            }
            ans.emplace_back(oneLevel);
        }

        return ans;
    }
};

```

##### 104. 二叉树的最大深度

```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root){
            return 0;
        }
        return max(maxDepth(root->left),maxDepth(root->right)) + 1;

    }
};
```

##### 105 从前序与中序遍历序列构造二叉树

C++切片：

   auto v2 = vector<int> (v1.begin(),v1.end());
	
   auto v2 = vector<int> (v1.begin(),v1.begin() + 4);

```C++
class Solution {

    //存储元素与索引
    unordered_map<int,int> hashIndex;
public:
    TreeNode* buildTree(vector<int>& preOrder, vector<int>& inOrder,int preLeft,int preRight,int inLeft, int inRight){
        if(preLeft > preRight){
            return nullptr;
        }

        TreeNode *root = new TreeNode(preOrder[preLeft]);
        //root 到left长度
        int size_left = hashIndex[preOrder[preLeft]] - inLeft;
        //注意index计算
        root->left = buildTree(preOrder, inOrder, preLeft + 1, preLeft + size_left, inLeft, inLeft + size_left -1);
        root->right = buildTree(preOrder, inOrder, preLeft + 1 + size_left, preRight, inLeft + 1 + size_left, inRight);
        return  root;
    }

    TreeNode* buildTree(vector<int>& preOrder, vector<int>& inOrder) {

        for(int i = 0;i < inOrder.size();i++){
            hashIndex[inOrder[i]] = i;
        }
        int n = preOrder.size();
        auto root = buildTree(preOrder,inOrder,0,n-1,0,n-1);
        return root;
    }
};
```

##### 114. 二叉树展开为链表

树天然递归

```C++
class Solution {
public:
    void flatten(TreeNode* root) {

        if(!root){
            return;
        }
        //找左子树最右，连接右子树
        flatten(root->left);
        auto right = root->right;
        root->right = root->left;
        //主动置空
        root->left = nullptr;
        while(root->right){
            root = root->right;
        }
        flatten(right);
        root->right = right;

        return;  
    }
};

```

##### 124  二叉树中的最大路径和

壳 + recur

树天然递归

```C++
class Solution {
    int maxnum = INT_MIN;
public:
    int recur(TreeNode* root){
        if(!root){
            return 0;
        }
        int left = max(recur(root->left),0);
        int right = max(recur(root->right),0);
        maxnum = max(maxnum,left + right + root->val);

        return max(left + root->val,right + root->val);
    }

    int maxPathSum(TreeNode* root) {
        (void)recur(root);
        return maxnum;

    }
};

```

### 3 动态规划

重叠子问题、最优子结构

##### 10 正则表达式匹配

vector<vector<int>> 定义bool类型

*忽略p一个（2个）或者忽略s一个。

```C++

class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();
        vector<vector<int>> dp(m +1,vector<int>(n + 1, 0));

        dp[0][0] = true;
        for(int j = 1;j < n + 1;j ++) {
            if (p[j - 1] == '*') {
                dp[0][j] |= dp[0][j - 2];
            }
        }

        for(int i = 1;i < m + 1 ;i++){
            for (int j = 1; j < n + 1; j++) {
                if ((s[i - 1] == p[j - 1]) || p[j - 1] == '.') {
                    dp[i][j] |= dp[i - 1][j - 1];
                }
                if (p[j - 1] == '*') {
                    dp[i][j] |= dp[i][j - 2]; //忽略p一个
                    if (s[i - 1] == p[j - 2]|| p[j - 2] == '.') {
                        dp[i][j] |= dp[i - 1][j]; //忽略s一个
                    }
                }
            }
        }
        return dp[m][n];
    }
};

```
##### 53. 最大子序和

简化动归

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        int dp = 0;
        int maxRes = nums[0];
        for(auto &num:nums){
            dp = max(dp + num,num);
            maxRes = max(maxRes,dp);
        }
        return maxRes;
    }
};

```

##### 70 爬楼梯

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n == 0){
            return 1;
        }
        if(n == 1){
            return 1;
        }
        vector<int> dp(n + 1,0);
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2;i < n+1;i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];

    }
};

```
##### 72 编辑距离

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        //简化操作: A插入、B 插入、A替换
        //dp[i][j]: A的前i和B的前j个
        for (int i = 0; i < m + 1; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j < n + 1; j++) {
            dp[0][j] = j;
        }

        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = min(dp[i - 1][j] + 1, min(dp[i][j - 1] + 1, dp[i - 1][j - 1]));
                } else {
                    dp[i][j] = min(dp[i - 1][j] + 1, min(dp[i][j - 1] + 1, dp[i - 1][j - 1] + 1));
                }
            }
        }
        return dp[m][n];
    }
};


```

#### 路径问题

##### 62. 不同路径

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m,vector<int> (n,0));
        for(int i = 0;i < m;i++){
            dp[i][0] = 1;
        }
        for(int j = 0;j < n;j++){
            dp[0][j] = 1;
        }
        for(int i = 1;i < m ;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = dp[i - 1][j] + dp[i][j-1];
            }
        }
        
        return  dp[m-1][n-1];
    }
};
```

##### 64. 最小路径和

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.size() == 0 || grid[0].size() == 0){
            return 0;
        }
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp(m,vector<int> (n,0));
        dp[0][0] = grid[0][0];
        for(int i = 1;i < m;i++){
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }
        for(int j = 1;j < n;j++){
            dp[0][j] = dp[0][j - 1] + grid[0][j];
        }
        for(int i = 1;i < m ;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = min(dp[i - 1][j],dp[i][j-1]) + grid[i][j];
            }
        }

        return  dp[m-1][n-1];
    }
};
```

#### 最值类问题

##### 152 乘积最大子数组

```C++

class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        vector<int> dpMax(n,0);
        vector<int> dpMin(n,0);
        dpMax[0] = nums[0];
        dpMin[0] = nums[0];
        int ans = nums[0];
        for(int i = 1;i < n;i++){
            dpMax[i] = max(nums[i],max(dpMax[i-1]*nums[i],dpMin[i-1]*nums[i]));
            dpMin[i] = min(nums[i],min(dpMax[i-1]*nums[i],dpMin[i-1]*nums[i]));
            ans = max(ans,dpMax[i]);
        }
        return  ans;
    }
};

```


##### 139 单词拆分

字典类动归

两层迭代：i 外层，j内层/字典层


```c++

class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {

        unordered_set<string> set;
        for (auto word:wordDict) {
            set.emplace(word);
        }

        vector<int> dp(s.size() + 1, 0);
        dp[0] = true;
        for (int i = 0; i < s.size() + 1; i++) {
            //j用set迭代
            for (int j = 0; j < i; j++) {
                if (dp[j] && set.count(s.substr(j, i - j))) {
                    dp[i] = true;
                }
            }
        }

        return dp[s.size()];
    }
};

class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {

        vector<int> dp(s.size() + 1, 0);
        dp[0] = true;
        for (int i = 0; i < s.size() +1; i++) {
            //字典迭代
            for(auto word:wordDict){
                if(word.size() <= i && dp[i-word.size()]){
                    if(s.substr(i-word.size(),word.size()) == word){
                        dp[i] = true;    
                    }
                }
            }
        }

        return dp[s.size()];
    }
};

```


##### 128 最长连续序列

unordered_set 使用

for(auto num:nums) --> for(const int &num:nums) 迭代

```
class Solution {
public:
    int longestConsecutive(vector<int> &nums) {

        unordered_set<int> set;
        int max_len = 0;
        for (const int &num:nums) {
            set.emplace(num);
        }
        for (const int &num:nums) {
            if (set.count(num - 1)) {
                continue;
            }
            int num_len = 1;
            int current_num = num;
            current_num += 1;
            while (set.count(current_num)) {
                current_num += 1;
                num_len += 1;
            }
            max_len = max(max_len, num_len);
        }
        return max_len;
    }
};
```

#### 区间问题

##### 56. 合并区间

```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end());
        vector<vector<int>> merged;
        merged.emplace_back(intervals[0]);

        for(int i = 1;i < intervals.size();i++){
            int L = intervals[i][0];
            int R = intervals[i][1];
            if(L <= merged.back()[1]){
                merged.back()[1] = max(merged.back()[1],R);
            }else{
                merged.emplace_back(intervals[i]);
            }
        }
        
        return merged;
    }
};


//自定义key比较

class Solution {
public:
    static bool cmp(vector<int> &a,vector<int> &b){
        if(a[0] > b[0]){
            return false;
        }

        return true;
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        sort(intervals.begin(),intervals.end(),cmp);
        vector<vector<int>> merged;
        merged.emplace_back(intervals[0]);

        for(int i = 1;i < intervals.size();i++){
            int L = intervals[i][0];
            int R = intervals[i][1];
            if(L <= merged.back()[1]){
                merged.back()[1] = max(merged.back()[1],R);
            }else{
                merged.emplace_back(intervals[i]);
            }
        }
        
        return merged;
    }
};

```

#### 股票问题

##### 121. 买卖股票的最佳时机

```C++

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxAns;
        int dpi_0 = 0;
        int dpi_1 = INT_MIN;
        for(int i = 0;i < prices.size();i++){
            dpi_0 = max(dpi_0,dpi_1 + prices[i]);
            dpi_1 = max(dpi_1 ,-prices[i]);
        }
        return  dpi_0;
    }
};

```
#### 打家劫舍问题
					     
#### 198 打家劫舍

```C++
class Solution {
public:
    int rob(vector<int>& nums) {

        //dp array -->简化
        int dp_i = 0;
        int dp_i_1 = 0;
        int dp_i_2 = 0;
        for(int i = 0;i < nums.size();i++){
            dp_i = max(dp_i_1,dp_i_2 + nums[i]);
            dp_i_2 = dp_i_1;
            dp_i_1 = dp_i;
        }
        return  dp_i;
    }
};

```

#### 背包问题

0-1背包

完全背包

### 4 回溯

子集、排列、组合、floodfill回溯

二叉、多叉

##### 17 电话号码的字母组合

```python

class Solution {
public:
    vector<string> letterCombinations(string digits) {

        vector<string> combines;
        if(digits.empty()){
            return combines;
        }
        //C++ map 直接初始化
        unordered_map<char,string> dict = {
            {'2',"abc"},
            {'3',"def"},
            {'4',"ghi"},
            {'5',"jkl"},
            {'6',"mno"},
            {'7',"pqrs"},
            {'8',"tuv"},
            {'9',"wxyz"},
        };

        //string 部分API的vector化
        string combine;    
        backtrack(combines,dict,digits,0,combine);
        return combines;
            
    }

    void backtrack(vector<string> &combines,unordered_map<char,string> dict,string digits,int index,string &combine){

        if(index == digits.size()){
            combines.emplace_back(combine);
            return;
        }

        char key = digits[index];
        const string letters  = dict[key];
        for(auto letter:letters){
            combine.push_back(letter);
            backtrack(combines,dict,digits,index + 1,combine);
            combine.pop_back();
        }
    }
};

```

#####22. 括号生成
```c++
class Solution {
public:
    void backtrack(int leftcount,int rightcount,string &path,vector<string> &ans,int n){
        if(path.size() == 2*n){
            ans.emplace_back(path);
            return;
        }

        if(leftcount < n){
            path.push_back('(');
            backtrack(leftcount+1,rightcount,path,ans,n);
            path.pop_back();
        }

        if(rightcount < leftcount){
            path.push_back(')');
            backtrack(leftcount,rightcount+1,path,ans,n);
            path.pop_back();
        }
    }

    vector<string> generateParenthesis(int n) {

        vector<string> ans;
        string path;
        backtrack(0,0,path,ans,n);

        return ans;
    }
};
```
##### 39 组合总和

```C++
class Solution {
public:
    void backtrack(vector<vector<int>> &ans,vector<int> &path,vector<int>& candidates,int target,int index){
        if(index == candidates.size()){
            return;
        }
        if(target == 0){
            ans.emplace_back(path);
            return;
        }

        //不选
        backtrack(ans,path,candidates,target,index + 1);
        //选
        if(candidates[index] <= target){
            path.emplace_back(candidates[index]);
            backtrack(ans,path,candidates,target - candidates[index],index);
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        
        vector<vector<int>> ans;
        vector<int> path;
        int index = 0;
        backtrack(ans,path,candidates,target,index);
        return ans;
    }
};
```

##### 46 全排列

1  全局visit[bool] 函数

2  swap动态交换，模仿组合排列过程

```c++
class Solution {
public:
    void backtrack(vector<vector<int>> &ans,vector<int>& nums,int first){
        if(first == nums.size()){
            ans.emplace_back(nums);
            return;
        }

        for(int i = first;i < nums.size();i++){
            swap(nums[i],nums[first]);
            backtrack(ans,nums, first+1);
            swap(nums[i],nums[first]);
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> ans;
        backtrack(ans,nums,0);

        return ans;
    }
};

```

##### 78 子集问题

回溯二叉 与回溯多叉

```
class Solution {
    vector<vector<int>> ans;
    vector<int> path;
public:
    void backtrack(vector<int>& nums,int index){
        if(index == nums.size()){
            ans.emplace_back(path);
            return;
        }

        backtrack(nums,index + 1);
        
        path.emplace_back(nums[index]);
        backtrack(nums,index + 1);
        path.pop_back();

    }
    vector<vector<int>> subsets(vector<int>& nums) {
        backtrack(nums,0);

        return ans;
    }
};

```
##### 79 单词搜索

1  pair 管理方向

2  visited 去重复访问路径

3  flood fill 四方向访问

```
class Solution {
public:
    //flood fill
    bool backtrack(vector<vector<char>> &board, string word, vector<vector<int>> &visited, int i, int j, int index) {
        if (board[i][j] != word[index]) {
            return false;
        }
        if (index == word.size() - 1) {
            return true;
        }
        visited[i][j] = true;
        int m = board.size();
        int n = board[0].size();
        vector<pair<int,int>> direct = {{-1,0},{0,-1},{1,0},{0,1}};
        for(auto dir:direct){
            int newi = i + dir.first;
            int newj = j + dir.second;
            if(newi < 0 || newi >= m || newj < 0 || newj >= n){
                continue;
            }
            if(visited[newi][newj]){
                continue;
            }
            if(backtrack(board,word,visited,newi,newj,index+1))
            {
                return true;
            }
        }
        visited[i][j] = false;

        return false;
    }

    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size();
        int n = board[0].size();
        vector<vector<int>> visited(m,vector<int>(n,0));
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                if(backtrack(board,word,visited,i,j,0)){
                    return true;
                }
            }
        }
        return false;
    }
};

```

	
#### 200 岛屿问题

floodfill回溯,涂色问题
	
```C++
class Solution {
public:
    void dfs(vector<vector<char>> &grid, int row, int col) {
        if (row < 0 || row > grid.size() - 1 || col < 0 || col > grid[0].size() - 1) {
            return;
        }
        if (grid[row][col] != '1') {
            return;
        }
        grid[row][col] = '2';
        dfs(grid, row - 1, col);
        dfs(grid, row, col - 1);
        dfs(grid, row + 1, col);
        dfs(grid, row, col + 1);

    }

    int numIslands(vector<vector<char>> &grid) {

        int m = grid.size();
        int n = grid[0].size();
        int sum = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    sum += 1;
                }
            }
        }
        return sum;
    }
};

```	
	
### 5 BFS

层序遍历

BFS: queue + while + for

### 6 双指针

快慢指针、左右指针、双向遍历、回文问题、归并排序、三指针



#### 双向遍历

#### 回文问题


##### 5  最长回文子串

回文问题

```python
class Solution {
public:
    string palindrome(string s,int l,int r){
        while(l >=0 && r < s.size()){
            if(s[l] == s[r]){
                l -= 1;
                r += 1;
            }else{
                break;
            }
        }
        return s.substr(l+ 1,r - l-1);
    }

    string longestPalindrome(string s) {
        int len = s.size();
        int index = 0;
        char s0 = s[0];
        string res = string(1,s0);
        while(index < len){
            string str1 = palindrome(s,index,index);
            string str2 = palindrome(s,index,index + 1);
            string max_str = str1.size()> str2.size()?str1:str2;
            res = res.size() > max_str.size()?res:max_str;
            index += 1;
        }
        return res;
    }
};


```


##### 11. 盛最多水的容器

双指针

```python

class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0;
        int r = height.size() - 1;
        int maxArea = 0;
        while(l < r){
            int area = min(height[l],height[r])*(r-l);
            maxArea = max(maxArea,area);
            if(height[l] < height[r]){
                l++;
            }else{
                r--;
            }
        }
        return maxArea;
    }
};
```


##### 15 三数之和

三指针

```c++
class Solution {
public:
  vector<vector<int>> threeSum(vector<int>& nums) {
        if(nums.size() < 3){
            return {};
        }

        sort(nums.begin(),nums.end());
        int len = nums.size();
        vector<vector<int>> res;
        for(int i = 0;i < len;i++){
            if(nums[i]>0){
                continue;
            }
            if((i > 0)&&(nums[i] ==nums[i-1])){
                continue;
            }
            int L = i + 1;
            int R = len -1;
            while(L < R){
                if(nums[i]+nums[L]+nums[R] == 0){
                    auto tmp = {nums[i],nums[L],nums[R]};
                    res.emplace_back(tmp); 
                
                    while((L+1 < R) && (nums[L] == nums[L+1])){
                        L++;
                    }
                    while((R-1 > L) && (nums[R] == nums[R-1])){
                        R--;
                    }
                    R--;
                    L++;
                }

                if(nums[i]+nums[L]+nums[R] > 0){
                    R--;
                }else if (nums[i]+nums[L]+nums[R] < 0)
                {
                    L++;
                }
                
            }
        }

        return res;
    }
};
```

##### 31 下一个排列

```C++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        
        //1 寻找左边较小（尽量靠右，数对）
        int n = nums.size();
        int i = n - 2;
        int j = n - 1;
        while((i >= 0)&&(nums[i] >= nums[j])){
            i -= 1;
            j -= 1;
        }
        //2 寻找右边较大
        int k = n -1;
        if(i >= 0){
            while((k >=0)&&(nums[i] >= nums[k])){
                k -= 1;
            }
            swap(nums[i],nums[k]);//交换两个数
        }
        //3 交换重新排升序，缩小幅度
        //int index1 = j;
        //int index2 = n - 1;
        //while(index1 < index2){
        //    swap(nums[index1],nums[index2]);
        //    index1 += 1;
        //    index2 -= 1;
        //}
        reverse(nums.begin() + j,nums.end());//反转

        return;
    }
};
```

##### 75 颜色分类

双指针：右侧循环判，左侧不超过


```c++

class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int p1 = 0;
        int p2 = n - 1;
        int index = 0;
        while(index <= p2){
            //循环右侧判
            while((index <= p2) &&(nums[index] == 2)){
                swap(nums[index],nums[p2]);
                p2--;
            }
            //p1不能跑到index前面
            if(nums[index] == 0){
                swap(nums[index],nums[p1]);
                p1++;
            }
            index ++;
        }
    }
};

```


### 7 二分查找

##### 33  搜索旋转排序数组

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;
        int n = nums.size();
        while(low <= high){
            int mid = low + (high - low)/2;
            if(nums[mid] == target){
                return mid;
            }
            //搜索确定性的、基本有序的部分
            if(nums[0] <= nums[mid]){
                if((nums[0] <= target)&&(target < nums[mid])){
                    high = mid - 1;
                }else{
                    low = mid + 1;
                }
            }else{
                if((nums[mid] < target)&&(target <= nums[n - 1])){
                    low = mid + 1;
                }else{
                    high = mid - 1;
                }
            }
        }
        return -1;
    }
};
```
##### 34 在排序数组中查找元素的第一个和最后一个位置

二分查找左侧边界，收缩右边。

target+ 1灵活收缩

```c++
class Solution {
public:
    int searchLeft(vector<int>& nums, int target){
        int left = 0;
        int right = nums.size() -1;
        while(left <= right){
            int mid = left + (right - left)/2;
            if(nums[mid] == target){
                right = mid -1;
            }else if(nums[mid] < target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }

        return left;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.size() ==0){
            return {-1,-1};
        }
        int left = searchLeft(nums,target);
        if((left < nums.size())&&(nums[left]== target)){
            int right = searchLeft(nums,target + 1);
            return {left,right -1};
        }else{
            return {-1,-1};
        }

    }
};
```


### 8 滑动窗口


##### 76 最小覆盖字串

注意unordered_map 直接访问会自动创建，需要count先判断存在

```C++

class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char,int> need;
        unordered_map<char,int> window;
        for(auto &str:t){
            need[str]++;
        }

        int left = 0;
        int right = 0;
        int n = s.size();
        int valid = 0;
        int minLen = 2*s.size();
        string minString;
        while(right < n){
            char char_right = s[right];
            right++;
            if(need.count(char_right)){
                window[char_right]++;
                if(window[char_right] == need[char_right]){
                    valid++;
                }
            }

            while(valid == need.size()){
                if(right - left < minLen){
                    minLen = right - left;
                    minString = s.substr(left,minLen);
                }
                char char_left = s[left];
                left++;
                if(need.count(char_left)) {
                    if(window[char_left] == need[char_left]) {
                        valid--;
                    }
                    window[char_left]--;
                }
            }
        }
        if(minLen == 2*s.size()){
            return "";
        }else{
            return minString;
        }
    }
};

```

### 9 其他 高频

贪心、位运算、 前缀和 、前缀树（字典树）、哈希、拓扑排序、并查集

#### 贪心

#####   55 跳跃游戏 I

贪心最远

```c++

class Solution {
public:
    bool canJump(vector<int>& nums) {
        int farest = 0;
        int n = nums.size();
        for(int i = 0;i < n;i++){
            if(i <= farest){
                farest = max(farest,i + nums[i]);
                if(farest >= n - 1){
                    return true;
                }
            }
        }

        return false;
    }
};

```

#### 169 多数元素

投票法
	
```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {

        //投票法
        int count = 1;
        int conda = nums[0];
        for(int i = 1;i < nums.size();i++){
            if(count == 0){
                conda = nums[i];
            }
            if(conda == nums[i]){
                count++;
            } else{
                count--;
            }
        }
        return conda;
    }
};

```

#### 哈希

##### 1. 两数之和

```c++
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> dict;
        for(int i = 0;i < nums.size();i++){
            int tmp = target - nums[i];
	    
            //map.find(key) != map.end()
	    //map.count(key) > 0
            if(dict.count(tmp) > 0){
                return {dict[tmp],i}; //返回vector
            }
            dict[nums[i]] = i;
        }
        return  {};
    }
```



##### 49 字母异位词分组

 map --value --list结构

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> dict;
        for(auto &str:strs){
            auto key = str;  //值拷贝
            sort(key.begin(),key.end());
            dict[key].emplace_back(str);
        }

        vector<vector<string>> ans;
        for(auto it = dict.begin();it != dict.end();it ++){
            ans.emplace_back(it->second);
        }
        return ans;
    }
};

```



#### 左右两边遍历


##### 42. 接雨水

左右两边遍历 + 动态规划

```c++
class Solution {
public:
    int trap(vector<int>& height) {

        int n = height.size();
        vector<int> leftMax(n,0);
        vector<int> rightMax(n,0);
        leftMax[0] = height[0];
        for(int i = 1;i < n;i++){
            leftMax[i] = max(leftMax[i - 1],height[i]);
        }
        rightMax[n-1] = height[n-1];
        for(int i = n - 2;i > -1;i--){
            rightMax[i] = max(rightMax[i + 1],height[i]);
        }

        int sum = 0;
        for(int i = 0;i < n ;i++){
            int water = min(leftMax[i],rightMax[i]);
            sum += water - height[i];
        }

        return  sum;
    }
};

```


##### 48 旋转图像

二次旋转

auto matrix_new = matrix;   // C++ 这里的 = 拷贝是值拷贝，会得到一个新的数组

matrix = matrix_new;  //赋值拷贝


```c++

class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for(int i = 0;i < n/2;i++){
            for(int j = 0;j < n;j++){
                swap(matrix[i][j],matrix[n-i -1][j]);
            }
        }
        for(int i = 0;i <n;i++){
            for(int j = i + 1;j< n;j++){
                swap(matrix[i][j],matrix[j][i]);
            }
        }

    }
};
```


#### 位运算


##### 136. 只出现一次的数字

```C++
class Solution {
public:
    int singleNumber(vector<int> &nums) {
        //要自己初始化，否则one的值随机的
        int one = 0;
        for(auto num:nums) {
            one ^= num;
        }
        return one;
    }
};
```

#### 前缀和数组

#### 前缀树（字典树）

#### 并查集

#### 拓扑排序

BFS:degree入度表、adjacency邻接表、queue(deque)遍历
