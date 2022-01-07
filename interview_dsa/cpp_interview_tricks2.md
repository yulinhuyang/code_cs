# 0x00 基本算法

前缀和、二分、双指针(排序、滑窗)

## 0x01 位运算
```cpp
int a = 2;
a >> 1; ---> 1  #除
a << 1; ---> 4  #乘
if(0 == (a & 1)) {
 //偶数
}
while(a){   //二进制1个数
  a = a & (a - 1);  
  count++;  
} 
a ^ b ^ b = a;
```
## 0x03 前缀和与差分

一维前缀和： S[i]= S[i-1]  + A[i]

二维前缀和： S[i][j] = S[i-1][j] + S[i][j-1] – S[i-1][j-1] + A[i][j]

树上前缀和：从根到某节点的路径上点（或边）的值之和（上到下）；某节点及其所有子节点（或边）的值之和（下到上）

差分数组定义：

真实数组a = {a[1]、a[2]、…、a[n]}          // 各点真实数据

差分数组df = {df[1]、df[2]、…、df[n]}      // 各点数据的变更值 

df[i] = a[i] - a[i-1]                      // 差分数组各点数据为真实数据的变更值

a[i] = df[1] + df[2] …+ df[i]              // 差分数组的前缀和即为真实数组

a[i] = a[i-1] + df[i]                      // 真实数据也可以从上一点数据+变更值求出

差分算法题型特征：在一段区间内（例如时间/站点）给出数据变更点（例如上下车/占用释放等），需要感知变更后数据值。

Ÿ   一维差分：主要用于对子数组（或区间）的元素整体加减固定值，特别是子数组较多时，可提高性能。

Ÿ   二维差分：对子矩阵元素整体进行加减处理，在子矩阵较多时，为提高性能，可以考虑用差分数组来处理。
	
## 0x04  二分和三分

### 0x04.1 二分 base code

关键代码

```C++
middle = start + (end — start) / 2

//左边界查找
if (nums[mid] == target) {
    // 收缩右侧边界
    right = mid - 1;
    
//右边界查找
if (nums[mid] == target) {
// 这里改成收缩左侧边界即可
    left = mid + 1;

```

### 0x04.2 stl二分法

| 语言   | 支持二分的常用数据结构             | 找值          | lower_bound     | upper_bound  |
| ------ | ---------------------------------- | ------------- | --------------- | ------------ |
| C++    | vector  multiset/set  map/multimap | binary_search | lower_bound     | upper_bound  |

**升序序列**

lower_bound：返回第一个 >= 目标值的迭代器，找不到则返回end()。

upper_bound：返回第一个 > 目标值的迭代器，找不到则返回end()。

**降序序列**

需要重载或者目标比较器，例如greater<int >()

lower_bound：返回第一个 <= 目标值的迭代器，找不到则返回end()。

upper_bound：返回第一个 < 目标值的迭代器，找不到则返回end()。

eg: 
```C++
 // 返回第一个小于等于目标值的迭代器

 lower_bound(vec.begin(), vec.end(), 8, greater<int>());

// 返回第一个小于目标值的迭代器

 upper_bound(vec.begin(), vec.end(), 8, greater<int>());

 bool isFind = binary_search(vec.begin(), vec.end(), 7);

// 返回第一个大于等于目标值的迭代器

 vector<int>::iterator iter1 = lower_bound(vec.begin(), vec.end(), 8);

 // 返回第一个大于目标值的迭代器

 vector<int>::iterator iter2 = upper_bound(vec.begin(), vec.end(), 8);
```


## 0x05 双指针与排序

左右指针（同向、反向）：排好序，找一些组合满足某种条件

快慢指针：有环的链表和数组问题，如判断链表是否是回文

### 0x05.1 基础排序 base code

**冒泡排序优化版**

```C++
void bubbleSort(vector<int> &nums) {

	int sortBorder = nums.size() - 1;
	int lastExchangeIndex = 0;
	for (int i = 0; i < nums.size(); i++) {
		bool isSorted = true;
		for (int j = 0; j < sortBorder; j++) {
			//j的相邻原始比较向上冒泡
			if (nums[j] > nums[j + 1]) {
				swap(nums[j], nums[j + 1]);
				isSorted = false;
				//设置有序边界,扩大有序区长度
				lastExchangeIndex = j;
			}
		}
		if (isSorted) {
			break;
		}
		sortBorder = lastExchangeIndex;
	}
}
```

**快速排序随机优化版**

```C++
class Solution {
public:
    int partition(vector<int> &nums, int low, int high) {
        //gen random index
        int index = (rand()%(high - low + 1))+1;
        swap(nums[low],nums[index]);

        int pivot = nums[low];
        int i = low;
        int j = high;
        while (i < j) {
            //i在大于基准数的地方停下，j在小于基准数的地方停下，如果i先走，最后停下跟基准数交换时，总是大于基准数的
            //j先走
            while (i < j && nums[j] >= pivot) {
                j--;
            }
            while (i < j && nums[i] <= pivot) {
                i++;
            }
            swap(nums[i], nums[j]);
        }
        swap(nums[low], nums[i]);
        return i;
    }

    void quickSort(vector<int> &nums, int start, int end) {
        if (start >= end) {
            return;
        }
        int index = partition(nums, start, end);
        quickSort(nums, start, index - 1);
        quickSort(nums, index + 1, end);
    }
}
```

**归并排序**

### 0x05.2 滑动窗口 base code
	
滑窗1：先right,后缩left，最小覆盖子串，最长无重复子串

滑窗2：先right,left同步前进，字符串排列，找所有字母异位词

简化滑窗：找所有字母异位词，统计 vector<int> sCount(26), vector<int> 可以直接比较相等关系
	
**滑动窗口，字符串**
	
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

### 0x05.3 双指针+ 双向遍历

双向遍历：柱形面积、接雨水，左边一遍，右边一遍

### 0x05.4 回文问题


### 0x05.5 Cyclic Sort，循环排序

在排好序/翻转过的数组中，寻找丢失的/重复的/最小的元素



## 0x06 贪心

区间合并 6种情况
	
打点标记法、区间合并法
	
会议室安排问题
	
 
# 0x10 基本数据结构

单调栈、单调队列、链表、二叉堆

## 0x11 栈/单调栈

栈处理字符串、括号，逆向处理法

### 0x11.1  单调栈

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


## 0x12 队列/单调队列

单向queue支持push_back、pop_front

双向deque支持push_front、pop_front、push_back、pop_back

**deque + 单调队列** 

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int m = nums.size();

        //存索引，方便出的判断
        deque<int> deque;
        vector<int> res;
	
        //处理前K个,保持升序
        for (int i = 0; i < k; i++) {
            while (!deque.empty() && nums[i] >= nums[deque.back()]) {
                deque.pop_back();
            }
            deque.emplace_back(i);
        }
        res.emplace_back(nums[deque.front()]);

        for (int i = k; i < m; i++) {
            //先出
            if (deque.front() == i - k) {
                deque.pop_front();
            }
            //后入并判断
            while (!deque.empty() &&  nums[i] >= nums[deque.back()]) {
                deque.pop_back();
            }

            deque.emplace_back(i);
            res.emplace_back(nums[deque.front()]);
        }

        return  res;
    }
};

```

## 0x13 链表与邻接表

### 0x13.1 链表 base code

链表基本结构：

```c++ 
struct ListNode {
    int val;
    ListNode *next; 
    ListNode() : val(0), next(nullptr) {} 
    ListNode(int x) : val(x), next(nullptr) {} 
    ListNode(int x, ListNode *next) : val(x), next(next) {}
}
	
ListNode *node = new ListNode(0,head);   #初始化节点
```

	
dummy的使用（注意new与delete）、head tail使用
 
ListNode *new = nullptr和Listnode的默认值不一样，默认值val是0

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

### 0x13.2 链表翻转

迭代法：pre cur next使用，不需要额外空间

递归法：防止成环

```C++
// 206. 反转链表 
//递归法
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

//迭代法：pre cur next
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


## 0x14  hash表(字符串hash)
	
hash: map -> vector简化-> 原地修改简化为自己

**448. 找到所有数组中消失的数字**
	
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n = nums.size();
        for(auto num:nums){
            int index = (num - 1) % n;
            nums[index] += n;
        }
        vector<int> res;
        for(int i = 0;i < nums.size();i++){
            if(nums[i] <= n){
                res.emplace_back(i + 1);
            }
        }
        return res;
    }
};
```
高级结构设计 LRU/LFU

## 0x15 字符串(KMP与最小表示法）


## 0x16  Trie树（字典树）

文件目录问题

## 0x17  二叉堆

### 0x17.1 ToP k问题

优先队列（堆）

优先队列的入和出与元素（数据）进的次序无关，而是由设定的优先级来决定元素的弹出次序，优先级最高的元素最先得到服务，优先级相同的元素按照其在优先队列中的顺序得到服务

实现优先队列的方式有多种，如数组、链表、平衡二叉树（如：AVL树和RB树）、堆等

堆是一种完全二叉树，可以分为小顶堆和大顶堆。小顶堆指的是堆顶元素（即树的根节点）为堆中最小值，且每一个节点的值都必须 小于等于 其孩子节点的值；反之即大顶堆

对堆的操作主要有两个，即插入或删除，且都是在顶端进行。由于堆是一个完全二叉树，删除和添加后都需要保证是完全二叉树，一般情况是先和最后一个节点进行交换，然后再进行删除和添加。

C++	priority_queue	push、top、pop、empty	系统自带	大顶堆
 
Python	heapq	heappush、heappop	系统自带	小顶堆

可以使用可自动排序的map进行替代，也能够达到减少时间复杂度的目的。如  C++(map)、 Python(SortedDict)

【c++】STL里的priority_queue用法总结: https://blog.csdn.net/xiaoquantouer/article/details/52015928

priority_queue<Type, Container, Functional>

Type为数据类型， Container为保存数据的容器，Functional为元素比较方式。

如果不写后两个参数，那么容器默认用的是vector，比较方式默认用operator<，也就是优先队列是大顶堆，队头元素最大。

### 0x17.2 双堆（Two Heaps） 

类似的有双栈

中位数，优先队列计划安排问题（Scheduling）

### 0x17.3 K-way merge，多路归并

Merge K Sorted Lists, Kth Smallest Number in M Sorted Lists

K个排好序的数组，用堆来高效顺序遍历，合并K个list, 和找第K小元素、

把每个数组中的第一个元素都加入最小堆中 --> 取出堆顶元素（全局最小），将该元素放入排好序的结果集合里面 --> 将刚取出的元素所在的数组里面的下一个元素加入堆 --> 重复步骤2，3，直到处理完所有数字




# 0x20  搜索

树遍历、DFS、BFS、动态规划

## 0x21 树与图的遍历

### 0x21.1 树

树天生为了递归左右子树而存在

树的三种遍历：

     两序恢复第三序（C++ 传递索引，代替切片），必须有中序，迭代、递归两个版本
    
平衡二叉树:

	空树是平衡二叉树；非空的树，所有子树都满足左子树和右子树高度差不超过1。
	
二叉查找树（Binary Search Tree）：（二叉搜索树，二叉排序树）
		
	空树是二叉查找树;左子树上所有结点的值均小于它的根结点的值,右子树上所有结点的值均大于它的根结点的值。用min,max向下比较。
	
	搜索二叉树按照中序遍历得到的序列一定是从小到大排列的.
	
	红黑树.平衡搜索二叉树(AVL)树,都是搜索二叉树的不同实现
	
完全二叉树： 	
	
	除了最后一层之外,其他每一层的节点数都是满的,如果最后一层满了就是满二叉树。
	
	如果最后一层不满,缺少的节点也全部集中在右边。

树的 base code

```C++
struct TreeNode {
      int val;
      TreeNode *left;
      TreeNode *right;
      TreeNode() : val(0), left(nullptr), right(nullptr) {}
      TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
      TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

TreeNode * t1 = new TreeNode(10);
```

### 0x21.2 树的DFS（Tree Depth First Search，stack）

三种遍历，递归与迭代版本（stack）

```C++
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
### 0x21.3 树的BFS(Tree Breadth First Search，queue)

层序遍历(queue)： 把根节点加到队列中，不断遍历直到队列为空。每一次循环中，我们都会把队头结点拿出来（remove）


### 0x21.4 图的遍历-拓扑排序 

依赖元素之间的线性顺序

BFS:degree入度表、adjacency邻接表、queue(deque)遍历
	
Tasks Scheduling


## 0x22 DFS(递归、回溯)

递归(自顶向下)：暴力递归---> memo 解法

DFS：深度优先搜索

动归(自底而上)：dp 数组 ---> 状态压缩二维变一维---> pre cur for循环解法

参进阶指南区别递归、DFS、动态规划，适用范围不同。

回溯是DFS的一种，会剪枝和修改后恢复全局变量

### 0x22.1 base code

**DFS**
```C++
//三种状态：unDiscovered、Discovered、visited
template <typename Tv, typename Te> //深度优先搜索DFS算法（全图）
void Graph<Tv, Te>::dfs ( int s ) { //assert: 0 <= s < n
   reset(); int clock = 0; int v = s; //初始化
   do //逐一检查所有顶点
      if ( UNDISCOVERED == status ( v ) ) //一旦遇到尚未发现的顶点
         DFS ( v, clock ); //即从该顶点出发启动一次DFS
   while ( s != ( v = ( ++v % n ) ) ); //按序号检查，故不漏不重
}

template <typename Tv, typename Te> //深度优先搜索DFS算法（单个连通域）
void Graph<Tv, Te>::DFS ( int v, int& clock ) { //assert: 0 <= v < n
   dTime ( v ) = ++clock; status ( v ) = DISCOVERED; //发现当前顶点v
   for ( int u = firstNbr ( v ); -1 < u; u = nextNbr ( v, u ) ) //枚举v的所有邻居u
      switch ( status ( u ) ) { //并视其状态分别处理
         case UNDISCOVERED: //u尚未发现，意味着支撑树可在此拓展
            type ( v, u ) = TREE; parent ( u ) = v; DFS ( u, clock ); break;
         case DISCOVERED: //u已被发现但尚未访问完毕，应属被后代指向的祖先
            type ( v, u ) = BACKWARD; break;
         default: //u已访问完毕（VISITED，有向图），则视承袭关系分为前向边或跨边
            type ( v, u ) = ( dTime ( v ) < dTime ( u ) ) ? FORWARD : CROSS; break;
      }
   status ( v ) = VISITED; fTime ( v ) = ++clock; //至此，当前顶点v方告访问完毕
}
```

**回溯**

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
DFS 防止死循环

1 涂色法。

2 visited数组法, unDiscovered  0，Discovered 1， visited  2 ，表示不同状态，注意是否能兼顾memo功能，有时会有更新循环问题

memo缓存： floodfill变形慎重缓存，visited了部分情况下也需要更新

### 0x22.2 子集问题（排列、组合）

**全排列问题** 

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

**多叉for选择回溯**

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

**二叉选择回溯**

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
###  0x22.3 flood fill问题

四方向回溯

pair集合  +  visited去重

visited代替涂色了

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
直接涂色
```C++
void fill(int[][] image, int x, int y,
        int origColor, int newColor) {
    // 出界：超出数组边界
    if (!inArea(image, x, y)) return;
    // 碰壁：遇到其他颜色，超出 origColor 区域
    if (image[x][y] != origColor) return;
    // 已探索过的 origColor 区域
    if (image[x][y] == -1) return;
    
    // choose：打标记，以免重复
    image[x][y] = -1;
    fill(image, x, y + 1, origColor, newColor);
    fill(image, x, y - 1, origColor, newColor);
    fill(image, x - 1, y, origColor, newColor);
    fill(image, x + 1, y, origColor, newColor);
    // unchoose：将标记替换为 newColor
    image[x][y] = newColor;
}
```
### 0x22.4 省份问题（DFS版） 岛屿问题

```cpp
class Solution {
    void dfs(vector<vector<int>> &isConnected,vector<int> &visited, int index){
        for(int j = 0;j < isConnected.size();j++){
            if(isConnected[index][j] && !visited[j]){
                visited[j] = 1;
                dfs(isConnected,visited,j);
            }
        }
    }

public:
    int findCircleNum(vector<vector<int>> &isConnected) {
        int provinces = isConnected.size();
        int circles = 0;
        vector<int> visited(provinces, 0);
        for (int i = 0; i < provinces; i++) {
            if (!visited[i]) {
                visited[i] = 1;
                dfs(isConnected,visited,i);
                circles++;
            }
        }
        return circles;
    }
};
```

## 0x23 BFS

### 0x23.1 BFS base code

核心思想：

```C++
template <typename Tv, typename Te> //广度优先搜索BFS算法（全图）
void Graph<Tv, Te>::bfs ( int s ) { //assert: 0 <= s < n
   reset(); int clock = 0; int v = s; //初始化
   do //逐一检查所有顶点
      if ( UNDISCOVERED == status ( v ) ) //一旦遇到尚未发现的顶点
         BFS ( v, clock ); //即从该顶点出发启动一次BFS
   while ( s != ( v = ( ++v % n ) ) ); //按序号检查，故不漏不重
}

template <typename Tv, typename Te> //广度优先搜索BFS算法（单个连通域）
void Graph<Tv, Te>::BFS ( int v, int& clock ) { //assert: 0 <= v < n
   Queue<int> Q; //引入辅助队列
   status ( v ) = DISCOVERED; Q.enqueue ( v ); //初始化起点
   while ( !Q.empty() ) { //在Q变空之前，不断
      int v = Q.dequeue(); dTime ( v ) = ++clock; //取出队首顶点v
      for ( int u = firstNbr ( v ); -1 < u; u = nextNbr ( v, u ) ) //枚举v的所有邻居u
         if ( UNDISCOVERED == status ( u ) ) { //若u尚未被发现，则
            status ( u ) = DISCOVERED; Q.enqueue ( u ); //发现该顶点
            type ( v, u ) = TREE; parent ( u ) = v; //引入树边拓展支撑树
         } else { //若u已被发现，或者甚至已访问完毕，则
            type ( v, u ) = CROSS; //将(v, u)归类于跨边
         }
      status ( v ) = VISITED; //至此，当前顶点访问完毕
   }
}
```

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
### 0x23.2 迷宫问题

路径障碍、迷宫问题

一 W 双 F，判空，for size, for选择 + visited数组

```C++
class Solution {
    int get(int x) {
        int res = 0;
        for (; x; x /= 10) {
            res += x % 10;
        }
        return res;
    }

public:
    int movingCount(int m, int n, int k) {
        if(!k){
            return  1;
        }
        queue<pair<int, int>> queue;
        vector<pair<int, int>> dirs = {{-1, 0},
                                       {1,  0},
                                       {0,  -1},
                                       {0,  1}};
        vector<vector<int>> visited(m,vector<int>(n,0));
        int ans = 1;
        queue.emplace(make_pair(0, 0));
        visited[0][0] = 1;
        while (!queue.empty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                auto pos = queue.front();
                queue.pop();
                for (auto dir:dirs) {
                    int newX = pos.first + dir.first;
                    int newY = pos.second + dir.second;
                    if (newX < 0 || newX > m - 1 || newY < 0 || newY > n - 1 || get(newX) + get(newY) > k || visited[newX][newY]) {
                        continue;
                    }
                    queue.emplace(make_pair(newX, newY));
                    visited[newX][newY] = 1;
                    ans += 1;
                }
            }
        }
        return ans;
    }
};
```
### 0x23.3 省份问题（简化BFS）

一个while 一个for

```cpp
简化的bfs
class Solution {
public:
    int findCircleNum(vector<vector<int>> &isConnected) {
        int provinces = isConnected.size();
        int circles = 0;
        vector<int> visited(provinces, 0);
        for (int i = 0; i < provinces; i++) {
            if (!visited[i]) {
                queue<int> queue;
                queue.push(i);
                while (!queue.empty()) {
                    auto top = queue.front();
                    queue.pop();
                    visited[top] = 1;
                    for (int j = 0; j < provinces; j++) {
                        if (visited[j] == 0 && isConnected[top][j]) {
                            queue.emplace(j);
                        }
                    }
                }
                circles++;
            }
        }
        return circles;
    }
};
```

## 0x41  数据结构进阶 - 并查集



## 0x50 动态规划
 
重叠子问题，最优子结构

动归(自底而上)：dp 数组 ---> 状态压缩二维变一维---> pre cur for循环解法

### 0x50.1 base code

**dp**

dp：明确 状态和选择--->base case ---> 明确dp数组含义(m,n)或(m+1,n+1)，状态转移--->返回dp值

```C++
#初始化base case，填充dp[0][0]、0行、0列
dp[0][0][...] = base

#进行状态转移，遍历[1:m][1:n](或[1:m+1][1:n+1])填充dp[i][j] 

for (状态1  in 状态1的所有取值)
	(for 状态2 in 状态2的所有取值)
		for ..
			dp[状态1][状态2][..] = 求最值（选择1，选择2...）

#返回dp[m-1][n-1](或dp[m][n])	

```
**dp遍历顺序**

1、遍历的过程中，所需的状态必须是已经计算出来的。

2、遍历的终点必须是存储结果的那个位置。

3. 常见的遍历方向：正向、反向、斜着

状态转移所依赖的状态必须被提前计算出来，需要根据 base case 和最终状态进行推导，合理安排i,j的遍历顺序

注意戳气球问题

### 0x50.2 背包问题

dp[i][j]含义: i是前i个物品(硬币)代表选择, j是限制容量（目标金额）代表限制条件,dp[i][j]最大价值或方法数

0-1背包: 最大价值, 变形子集背包

完全(无限)背包：零钱问题

背包问题，正常情况下 choice(coins)在外循环，amount(限制)在内循环，根据choice是否有限进行求最值、能否、方法数等，

部分特殊情况，如完全平方数等，choice不使用sqrt可以简化放在内循环。

0-1背包模板(最值)，部分可以简化为一维的
```Cpp
for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
return dp[N][W]
```

子集背包模板(能否)：

 #装入或者不装入
```cpp
 dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]]
```
 
完全背包模板(方法数)

https://labuladong.gitee.io/algo/3/25/81/

```cpp
for i in range(n + 1):
    for j in range(amount + 1):
        if j - coins[i-1] >= 0：
            dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]] #这里表示i可以反复使用
        else:
            dp[i][j] = dp[i - 1][j]
```

零钱问题


### 0x50.3 子序列问题

信封嵌套问题
	
128 最长连续序列 LCS ： set中心展开法

300  最长递增子序列 LIS: insertsort dp()

1143 最长公共子序列 LCS ；二维dp


### 0x50.4 打家劫舍问题



