## 1 链表（堆 栈）

### 1.1  base code

链表基本结构：


```c++ 
struct ListNode {
    int val;
    ListNode *next; 
    ListNode() : val(0), next(nullptr) {} 
    ListNode(int x) : val(x), next(nullptr) {} 
    ListNode(int x, ListNode *next) : val(x), next(next) {}
}
```	
	
dummy的使用（注意new与delete）、head tail使用

迭代法：pre cur next使用、递归法：防止成环
 

```c++
        //dummy定义与释放

        //1 删除倒数节点，slow dummy
    	ListNode *dummy = new ListNode(0,head); 
    	ListNode *fast = head;
    	ListNode *slow = dummy; 
    	
    	ListNode *ans = dummy->next;
    	delete dummy;
    
        //另外一种方式
        ListNode dummy(-1);
        ListNode* p = &dummy
        
        //2 链表合并，有效是dummy->next
        ListNode *dummy = new ListNode(0);//过渡指针
        ListNode *head = dummy;
        head->next = l1;
        
        //3 two sum head tail使用
        ListNode *head = nullptr;//便于操作head 
        ListNode *tail = nullptr;
        if(!head){
            //理解new的返回类型
            tail = head = new ListNode(sum%10);
        }else{
            tail->next = new ListNode(sum%10);
            tail = tail->next;
        }

        //4 递归，中间截断
        ListNode *fast = head->next;
        ListNode *slow = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }        
   
```

链表基本操作

```C++


// 206. 反转链表 

递归法

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

迭代法：pre cur next

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


    
    测试用例的链表定义
    
```c++
        ListNode *l1_next_next = new ListNode(4);
        ListNode *l1_next = new ListNode(2,l1_next_next);
        ListNode *l1 = new ListNode(1,l1_next);
```

### 1.2 单调栈

单调栈

ans + stack辅助

stack辅助：使得每次新元素入栈后，栈内的元素都保持有序

stack: 存坐标、存值，单调升、单调降

左右双栈：左一遍、右一遍

```C++

    for(int i = 0;i < heights.size();i++){
        while ((!left_stack.empty()) &&(heights[left_stack.top()] >= heights[i])){
            left_stack.pop();
        }
        left[i] = left_stack.empty()? -1:left_stack.top();
        left_stack.push(i);
    }
    
    for(int j = n - 1;j > -1;j--){
        while ((!right_stack.empty()) &&(heights[right_stack.top()] >= heights[j])){
            right_stack.pop();
        }
        right[j] = right_stack.empty()?n:right_stack.top();
        right_stack.push(j);
    }

```

## 2 树

树天生为了递归左右子树而存在

树的三种遍历：

    两序恢复第三序，C++ 传递索引
    

平衡二叉树:

	空树是平衡二叉树；非空的树，所有子树都满足左子树和右子树高度差不超过1。
	
二叉查找树（Binary Search Tree）：（二叉搜索树，二叉排序树）
		
	空树是二叉查找树;左子树上所有结点的值均小于它的根结点的值,右子树上所有结点的值均大于它的根结点的值。用min,max向下比较。
	
	搜索二叉树按照中序遍历得到的序列一定是从小到大排列的.
	
	红黑树.平衡搜索二叉树(AVL)树,都是搜索二叉树的不同实现
	
完全二叉树： 	
	
	除了最后一层之外,其他每一层的节点数都是满的,如果最后一层满了就是满二叉树。
	
	如果最后一层不满,缺少的节点也全部集中在右边。

壳 + 递归结构

### 2.1 base code

```C++
struct TreeNode {
      int val;
      TreeNode *left;
      TreeNode *right;
      TreeNode() : val(0), left(nullptr), right(nullptr) {}
      TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
      TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

//验证二叉搜索树

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


//索引传递代替切片

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


## 3 动态规划

重叠子问题，最优子结构

### 3.1 base code




## 4 回溯（DFS）

### 4.1 base code

```c++
void backtrack(){

    if 满足结束条件：
        result.add(路径)
        return 
    
    //多叉递归回溯
    for 选择 in 选择列表:
        排除不合法的选择
    
        做选择
        backtrack(路径，选择列表)
        撤销选择
        
}
```

全排列问题： 

    1 swap交换法  

```C++

    for(int i = first;i < nums.size();i++){
        swap(nums[i],nums[first]);
        backtrack(ans,nums, first+1);
        swap(nums[i],nums[first]);
    }
```
    
    2  visit[bool] 法  
    
    3 contain排除 
      // 排除不合法的选择
      if (track.contains(nums[i]))
          continue; 

子集、排列、组合问题： 二叉递归回溯、多叉递归回溯

多叉for 选择回溯

```C++

      []
  |    |    |
  1    2    3
 |||  | |  | 


子集：

class Solution {
    vector<vector<int>> ans;
    vector<int> path;
public:
    void backtrack(vector<int>& nums,int index){
        ans.emplace_back(path);
        
        //多叉选择
        for(int i = index;i < nums.size();i++){
            path.emplace_back(nums[i]);
            backtrack(nums,i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        backtrack(nums,0);

        return ans;
    }
};

```

二叉选择回溯

1 no/yes -- 2 no/yes -- 3 no/yes

```C++

子集：
class Solution {
    vector<vector<int>> ans;
    vector<int> path;
public:
    void backtrack(vector<int>& nums,int index){
        if(index == nums.size()){
            ans.emplace_back(path);
            return;
        }
		
	//不选
        backtrack(nums,index + 1);
        
	//选
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

四方向回溯

pair集合  +  visited去重

visited这里类似涂色了

```C++
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

flood fill问题

```




```

## 5 BFS

### 5.1 base code

两个循环（while + for） + 一个遍历（for adj点）


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

                //类似for遍历
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

## 6 双指针

### 6.1 base code

[冒泡排序的三种改进方法](https://www.cnblogs.com/mistermoney/p/9550590.html)

## 7 二分搜索

### 7.1 base code




## 8 滑动窗口

### 8.1 base code

注意map(unordered_map)访问key 则会自动创建，所以需要count先判断。

```
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

## 9 其他高频

### 9.1 base code
