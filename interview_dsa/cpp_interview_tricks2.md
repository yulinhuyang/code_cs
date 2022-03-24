yxc leetcode全解(究极班)： https://www.acwing.com/activity/content/activity_person/content/29799/44/

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
(n>>k)&1  取n的第k位
n | (1<<k) 第k位赋值1
```
## 0x03 前缀和与差分

前缀和定义：     
A从1开始,S从1开始,for从1开始,S[i] = S[i - 1] + A[i]。A从0开始,S从1开始(n+1),for从0开始,S[i+1] = S[i] + A[i]。默认S[0] = 0，S一定从1开始。  

一维前缀和： S[i]= S[i-1]  + A[i]      
二维前缀和： S[i][j] = S[i-1][j] + S[i][j-1] – S[i-1][j-1] + A[i][j]     
S[i, j] = 第i行j列格子左上部分所有元素的和。          
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：             
S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]         

树上前缀和：从根到某节点的路径上点（或边）的值之和（上到下）；某节点及其所有子节点（或边）的值之和（下到上）  

差分数组定义：     
真实数组a = {a[1]、a[2]、…、a[n]}          //   
差分数组df = {df[1]、df[2]、…、df[n]}      // 各点数据的变更值    
df[i] = a[i] - a[i-1]                      // 差分数组各点数据为真实数据的变更值   
a[i] = df[1] + df[2] …+ df[i]              // 差分数组的前缀和即为真实数组   
a[i] = a[i-1] + df[i]                      // 真实数据也可以从上一点数据+变更值求出

一维差分：给区间[l, r]中的每个数加上c ，B[l] += c, B[r + 1] -= c  

二维差分： 给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：      
S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c   

差分算法题型特征：在一段区间内（例如时间/站点）给出数据变更点（例如上下车/占用释放等），需要感知变更后数据值。   
- 一维差分：主要用于对子数组（或区间）的元素整体加减固定值，特别是子数组较多时，可提高性能。   
-  二维差分：对子矩阵元素整体进行加减处理，在子矩阵较多时，为提高性能，可以考虑用差分数组来处理。

- Leetcode 560. 和为 K 的子数组: 简化的前缀和。
	
## 0x04  二分和三分

### 0x04.1 二分 base code

关键代码

```C++
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用，找左边界：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用，找右边界：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

两段性和单调性，折线法，寻找分解条件

步骤：   
1 确定二分的边界。      
2 编写二分的代码框架。       
3 设计一个check(性质)。    
4 判断区间如何更新。    
5 如果更新方式是l=mid,r = mid -1,则算mid的时候需要加上1。  

模板1: L + r下取整。 找后一段的绿色的开始端点，左边界。     
模板2：L + r + 1上取整,避免L = R - 1的时候，L < R 死循环。 找前一段的红色的结束段点，右边界。   

 <div align="center"> <img src="../pics/efen1.png" width="50%"/> </div><br>

模板题：

- Leetcode  153. 寻找旋转排序数组中的最小值：nums.back()的秒用。   
- Leetcode 33. 搜索旋转排序数组:先找最小值，去掉干扰条件   
- offer 11 旋转数组的最小数字：先去掉干扰条件。   
- Offer 03 数组中重复的数字：反复交换法

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

左右指针（同向、反向）：排好序，找一些组合满足某种条件。
快慢指针：有环的链表和数组问题，如判断链表是否是回文。

### 0x05.1 基础排序 base code

冒泡排序优化版:dsa_notes  
快速排序随机优化版:dsa_notes   

**快速排序普通版**
```cpp
void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}	
```
**归并排序**
```cpp
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int mid = l + r >> 1;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);

    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];

    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];

    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}	  		  
```
Merge K Sorted Lists

- Leetcode 167. 两数之和 II - 输入有序数组： 首尾指针。                        
- Leetcode 88. 合并两个有序数组：首尾指针，双指针从后向前合并。                        
- Leetcode 26. 删除有序数组中的重复项：前后指针。
	
### 0x05.2 滑动窗口 base code
	
滑窗1：先right,后缩left，最小覆盖子串，最长无重复子串   
滑窗2：先right,left同步前进，字符串排列，找所有字母异位词   
简化滑窗1：找所有字母异位词，统计 vector<int> sCount(26), vector<int> 可以直接比较相等关系   
简化滑窗2：单个hash逐加逐减, ij双变量循环代替滑窗。
	
**滑动窗口**
	
注意map(unordered_map)访问key 则会自动创建，所以需要count先判断。  
```
//双hash
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
//单hash
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char,int> hash;
        for(auto ch:t) hash[ch]++;
        int cnt = hash.size();

        string res = "";
        int c = 0;
        for(int i = 0,j = 0;i < s.size();i++){
            if(hash[s[i]] == 1) c++;
            hash[s[i]]--;
            while(c == cnt && hash[s[j]] < 0) hash[s[j++]]++;
            if(c == cnt){
                if(res.empty() || res.size() > i - j + 1 ) res = s.substr(j,i - j  + 1);
            }
        }
        return  res;
    }
};
```
LeetCode 76. 最小覆盖字串:  单个hash + i(hash[key]--)  j(hash[key]++) 双指针  简化滑动窗口问题 

### 0x05.3 双指针+ 双向遍历

双向遍历：柱形面积、接雨水，左边一遍，右边一遍

- Leetcode 42. 接雨水:两遍max, 左边一遍Left_max，右边一遍right_max，min(left_max[i],right_max[i]) - height[i]    
- Leetcode 84. 柱状图中最大的矩形:两遍栈，左边一遍单调上升栈，右边一遍单独上升栈，height[i] * (right[i] - left[i] - 1)     

### 0x05.4 回文问题 
### 0x05.5 Cyclic Sort，循环排序

在排好序/翻转过的数组中，寻找丢失的/重复的/最小的元素

## 0x06 贪心

区间合并 6种情况
	
打点标记法、区间合并法
	
会议室安排问题
	
```C++
// 将所有存在交集的区间合并
void merge(vector<PII> &segs)
{
    vector<PII> res;
    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);

    if (st != -2e9) res.push_back({st, ed});

    segs = res;
}
```
	
# 0x10 基本数据结构

单调栈、单调队列、链表、二叉堆

## 0x11 栈/单调栈

栈处理字符串、括号，逆向处理法

### 0x11.1  单调栈

ans + stack辅助： stack辅助：使得每次新元素入栈后，栈内的元素都保持有序。  
stack: 存坐标、存值，单调升、单调降。   
左右双栈：左一遍、右一遍  

```C++
//单调栈代码段
while (stk.size() && heights[stk.top()] >= heights[i]) stk.pop();
if (stk.empty()) left[i] = -1;
else left[i] = stk.top();
stk.push(i);
```      
- Leetcode 84. 柱状图中最大的矩形:两遍栈，左边一遍单调上升栈，右边一遍单独上升栈，height[i] * (right[i] - left[i] - 1)   
- Leetcode 32. 最长有效括号：起始加-1,栈底元素为当前已经遍历过的元素中「最后一个没有被匹配的右括号的下标]                   

## 0x12 队列/单调队列

单向queue支持push_back、pop_front   
双向deque支持push_front、pop_front、push_back、pop_back
	
```C++
//单调队列代码段
if (q.size() && i - k >= q.front()) q.pop_front();
while (q.size() && nums[q.back()] <= nums[i]) q.pop_back();
q.push_back(i);
if (i >= k - 1) res.push_back(nums[q.front()]);
```  
- Leetcode 239. 滑动窗口最大值:单调下降队列简化     
- leetcode 918: 拆环为链 + 前缀和 +单调上升队列：    
前缀和：A从1开始,S从1开始,for从1开始,S[i] = S[i - 1] + A[i]。A从0开始,S从1开始(n+1),for从0开始,S[i+1] = S[i] + A[i]。默认S[0] = 0，S一定从1开始。  
单调队列：针对前缀和S的,所以i-k >= n 有等号。      

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
	
ListNode *node = new ListNode(0,head);  #初始化 
auto *node = new ListNode(0,head);      #初始化 
```

链表画图看，添加虚拟头节点(涉及到头节点可能会变的情况下)

 <div align="center"> <img src="../pics/lianbiao1.png" width="50%"/> </div><br>
 
涉及到要修改头节点的时候，需要dummy,     
注意new与delete 、head tail使用    
链表Cout调试   
ListNode *new = nullptr和Listnode的默认值不一样，默认值val是0

```c++
//dummy定义与释放

//1 删除倒数节点，slow dummy
auto dummy = new ListNode(-1);
dummy->next = head;
auto fast = dummy,slow = dummy;
	
ListNode *ans = dummy->next;
delete dummy;

//另外一种方式
ListNode dummy(-1);
ListNode* p = &dummy

//2 链表合并，有效是dummy->next
ListNode *dummy = new ListNode(-1);//过渡指针
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
//5  链表循环模式,判空是判断cur还是cur->next
while(cur->next && cur->val == cur->next->val) cur->next = cur->next->next;
	
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
        // 边缘条件判断
        if(head == NULL) return NULL;
        if (head->next == NULL) return head;
        
        // 递归调用，翻转第二个节点开始往后的链表
        ListNode *last = reverseList(head->next);
        // 翻转头节点与第二个节点的指向
        head->next->next = head;
        // 此时的 head 节点为尾节点，next 需要指向 NULL
        head->next = NULL;
        return last;
    }
}; 

//迭代法：pre cur next
class Solution {
//链表反转迭代
 ListNode* reverseList(ListNode* head) {
     ListNode* next; // 保存cur的下一个节点
     ListNode* cur = head;
     ListNode* pre = NULL;
     while(cur) {
         next = cur->next;  // 保存一下 cur的下一个节点，因为接下来要改变cur->next
         cur->next = pre; // 翻转操作
         // 更新pre 和 cur指针
         pre = cur;
         cur = next;
     }
     return pre;
 }
 ```

## 0x14  hash表
	
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
	
- Leetcode 187. 重复的DNA序列: unordered_map(hash) 使用。  
- Leetcode 652. 寻找重复的子树: 中序遍历与后序或者中序遍历与前序遍历都可以唯一确定一棵树。两遍hash,第一遍树变成字符串，第二遍变成数。  

## 0x15 字符串(字符串hash 、KMP与最小表示法）

模式：
```
    while (k < s.size() && s[k] == s[j]) k ++ ;  
    while (k < s.size() && s[k] == ' ') k ++ ;
    unordered_map<string,vector<string>> hash;
```
为什么 STL 中的容器和算法都是用的左闭右开区间？：https://www.zhihu.com/question/61054439
	
相关API:      
reverse、str.find   
string substr (size_t pos = 0, size_t len = npos)  //如果没有n,默认到末尾，长度是len,包含pos + len位置的字符。    
to_string() 转字符串  
stoi或 atoi(s1.substr(i, x - i).c_str())
	
题目：    
- Leetcode 3 无重复字符的最长字串: hash逐加逐减, ij双变量循环代替滑窗。   
- Leetcode 17 电话号码的字母组合：三重简单循环代替dfs

## 0x16  Trie树（字典树）

文件目录问题

## 0x17  优先队列（二叉堆）

### 0x17.1 ToP k问题

优先队列的入和出与元素（数据）进的次序无关，而是由设定的优先级来决定元素的弹出次序，优先级最高的元素最先得到服务，优先级相同的元素按照其在优先队列中的顺序得到服务。   
实现优先队列的方式有多种，如数组、链表、平衡二叉树（如：AVL树和RB树）、堆等。   
堆是一种完全二叉树，可以分为小顶堆和大顶堆。小顶堆指的是堆顶元素（即树的根节点）为堆中最小值，且每一个节点的值都必须 小于等于 其孩子节点的值；反之即大顶堆。  
对堆的操作主要有两个，即插入或删除，且都是在顶端进行。由于堆是一个完全二叉树，删除和添加后都需要保证是完全二叉树，一般情况是先和最后一个节点进行交换，然后再进行删除和添加。

C++	priority_queue	push、top、pop、empty	系统自带	大顶堆
 
Python	heapq	heappush、heappop	系统自带	小顶堆

可以使用可自动排序的map进行替代，也能够达到减少时间复杂度的目的。如  C++(map)、 Python(SortedDict)

priority_queue<Type, Container, Functional>   
Type为数据类型， Container为保存数据的容器，Functional为元素比较方式。  
如果不写后两个参数，那么容器默认用的是vector，比较方式默认用operator<，也就是优先队列是大顶堆，队头元素最大。

- Leetcode 692. 前K个高频单词:小顶堆(大顶key加负号)流过程,保证second的字典序排序，也可以自定义比较函数。    
pair 设置 --> make_pair或者 直接构造 PIS t(-word.second, word.first);    
优先队列默认大的在队首，字符串则为字典序由大到小。int，double，char，string类型都可以这样定义。  
	
### 0x17.2 对顶堆（Two Heaps） 

- LeetCode 295. Find Median from Data Stream：对顶堆，小上大下 --> 小右大左。动态流，图参考：https://blog.csdn.net/jiahonghao2002/article/details/114108760   
1 2 3 4 (大顶) | 5 6 7 8(小顶) : right.size() > left.size(), right 比left至多多一个,添加时优先添加right, 先添加后调整。

Merge K Sorted Lists, Kth Smallest Number in M Sorted Lists  
K个排好序的数组，用堆来高效顺序遍历，合并K个list, 和找第K小元素、

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

三种遍历，递归与迭代版本（stack），stack代替了递归的功能

树的递归问题：枚举最高点 + 左右最大(当前)返回

- Leetcode LCA: 236. 二叉树的最近公共祖先，树天然递归    
- Leetcode 94. 二叉树的中序遍历： 栈的递归、迭代（stack中序）    
- Leetcode 124 二叉树中的最大路径和：枚举最高点 + 左右最大(当前)返回     
- Leetcode 105 从前序与中序遍历序列构造二叉树：hash + 索引传递    

```C++
//中序迭代遍历
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> ans;
        if(!root) return ans;
        auto cur = root;
        while(cur || stk.size()){
            while(cur){
                stk.emplace(cur);
                cur = cur->left;
            }
            cur = stk.top();
            stk.pop();
            ans.emplace_back(cur->val);
            cur = cur->right;
        }

        return ans;
    }
};

```
### 0x21.3 树的BFS(Tree Breadth First Search，queue)

层序遍历(queue)： 把根节点加到队列中，不断遍历直到队列为空。每一次循环中，我们都会把队头结点拿出来（remove）

## 0x22 DFS(递归、回溯)

递归(自顶向下)：暴力递归---> memo 解法   
动归(自底而上)：dp 数组 ---> 状态压缩二维变一维---> pre cur for循环解法   
回溯是DFS的一种，会剪枝和修改后恢复全局变量   
bfs一般求最小步数和最短距离，dfs状态数量非常大，但是解的数量很小。  

### 0x22.1 base code

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
DFS防止死循环：
1 涂色法。   
2 visited数组法, unDiscovered  0，Discovered 1， visited  2 ，表示不同状态，注意是否能兼顾memo功能，有时会有更新循环问题。   
memo缓存： floodfill变形慎重缓存，visited了部分情况下也需要更新

### 0x22.2 组合\子集\排列
	
swap交换法、contain排除、visit[bool]法
 
```C++
for(int i = first;i < nums.size();i++){
	swap(nums[i],nums[first]);
	backtrack(ans,nums, first+1);
	swap(nums[i],nums[first]);
}
// 排除不合法的选择
if (track.contains(nums[i]))
  continue;
```

多叉for选择与二叉选择(1 no/yes -- 2 no/yes -- 3 no/yes)

```cpp
void backtrack(vector<int>& nums,int index){
	ans.emplace_back(path);
	//多叉选择
	for(int i = index;i < nums.size();i++){
		path.emplace_back(nums[i]);
		backtrack(nums,i + 1);
		path.pop_back();
	}
}
void backtrack(vector<int>& nums,int index){
	if(index == nums.size()){
		ans.emplace_back(path);
		return;
	}
	backtrack(nums,index + 1);//不选
	
	path.emplace_back(nums[index]);//选
	backtrack(nums,index + 1);
	path.pop_back();
}
```

- Leetcode 47 全排列II  遍历的时候跳过相同元素，人为规定相同数字的相对顺序不变 
 
```
while(i + 1 < nums.size() && nums[i] == nums[i+1]) i++;
```
                         
- Leetcode 78 子集  位运算+循环,简化子集问题, 递归结构相当于一层循环。  
- Leetcode 90 子集II  回溯去重  
- LeetCode 52. N-Queens II    dfs代替行, col + d + ud 数组标记解法，精确覆盖问题。  
- LeetCode 37. Sudoku Solver  dfs 逐行, col + row + cell 数组标记解法，row[9][9]: 9行,0-9的数有了哪几个。       
- Leetcode 473. 火柴拼正方形   剪枝优化技巧，模板题

###  0x22.3 flood fill问题(DFS)

四方向回溯(pair/dxdy)  +  visited去重/涂色

```C++
//双循环枚举每个点
visited[i][j] = true;			 
visited[i][j] = false;
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

## 0x23 BFS

### 0x23.1 BFS base code

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
### 0x23.2 BFS应用

迷宫问题、双向bfs(单词接龙)、0-1矩阵问题(多源头bfs)、bfs floodfil

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

模板题：
- LeetCode 547 省份数量(Number Of Provinces)	
- LeetCode 127 单词接龙: 双向BFS搜索（26子母扩展）
- LeetCode 542 01 Matrix：矩形多源头的bfs

## 0x30 数学知识
	
n项求和公式、摩尔投票法、卡特兰数
	
## 0x40  数据结构进阶 
	
## 0x41 并查集
	
```C++
    int p[N]; //存储每个点的祖宗节点

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
```
			  
- LeetCode 547. Friend Circles 省份问题 :并查集   
- LeetCode 684. Redundant Connection: 并差集变形，1->n 下标从1开始。   

## 0x42 树状数组
	
```C++
    int lowbit(int x){
        return x & -x;
    }
    int query(int x){
        int res = 0;
        for(int i = x;i;i -= lowbit(i)){
            res += tree[i];
        }
        return res;
    }
    
    void add(int index, int val) {
        for(int i = index;i < n + 1;i += lowbit(i)){
            tree[i] += val;
        }
    }
	
```
			       
树状数组的下标从 1 开始计数
			       
- 307 区域和检索 - 数组可修改：模板题。
- 315 计算右侧小于当前元素的个数：统计元素个数转化为树状数组。		       
	
树状数组学习笔记：https://www.acwing.com/blog/content/80/
			       
## 0x50 动态规划
 
重叠子问题，最优子结构，无后效性 + 状态、决策、转移方程

动归(自底而上)：dp 数组 ---> 状态压缩二维变一维---> pre cur for循环解法

### 0x50.1 线性DP
	
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
3. 常见的遍历方向：正向、反向、斜着。  
状态转移所依赖的状态必须被提前计算出来，需要根据 base case 和最终状态进行推导，合理安排i,j的遍历顺序。  

- LeetCode 63. 不同路径 II：循环内处理好0行0列。  
- LeetCode 10. 正则表达式匹配: sp加' ',*提前判断。    

**数字三角形模型**
	
```cpp
 //滚动数组优化
vector<int> f[2];
f[0] = f[1] = vector<int>(n);

auto &a = f[i & 1], &b = f[i - 1 & 1];
a[j] = INT_MAX;

//i,j的0行0列填充，可以初始填好，也可以循环内进行条件判断
if (j) a[j] = b[j - 1] + triangle[i][j];
if (j <= i - 1) a[j] = min(a[j], b[j] + triangle[i][j]);
```
- LeetCode 120. Triangle：滚动数组优化版 + 普通二维数组版本。 
	 
**子序列模型**
	  
- LeetCode 128 最长连续序列 LCS: set中心展开法。  
- LeetCode 300  最长递增子序列 LIS: insertsort dp()   
- LeetCode 1143 最长公共子序列 LCS :二维dp。 
- LeetCode 354 俄罗斯套娃信封问题: 二维子序列，先排序，一维dp。
	 
**状态机模型**
	 
- LeetCode 121 买卖股票的最佳时机:贪心/状态机	 
- LeetCode 198 打家劫舍

### 0x50.2 背包问题

dp[i][j]含义: i是前i个物品(硬币)代表选择, j是限制容量（目标金额）代表限制条件,dp[i][j]求最大价值或方法数(min max count)

0-1背包: 最大价值, 变形子集背包，目标和。  
完全(无限)背包：零钱问题。  	
背包问题，for物品 ->for体积->for选择

**0-1背包模板(最值)**
	
```Cpp
for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
return dp[N][W]
```
子集背包模板(装入或者不装入)：

```cpp
dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]] //能否  
dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i - 1]]  //方案数，需要将前面各种选择的相加  
选择num[i-1]可以为0时，[i][0]的填充需要dp循环填
```
 
**完全(无限)背包模板(方法数)**
```cpp
for i in range(n + 1):
    for j in range(amount + 1):
        if j - coins[i-1] >= 0：
            dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]] #这里表示i可以反复使用
        else:
            dp[i][j] = dp[i - 1][j]
```
- LeetCode 322. 零钱兑换 I: 完全背包的二维和一维解法。      
- LeetCode 518. 零钱兑换 II: 完全背包的二维和一维解法。  

### 0x50.3 区间dp

区间dp: len -> 左端点 -> 决策

- Leetcode 312 戳气球问题
- LeetCode 664. Strange Printer:s[l-1]=s[k-1],f[l][r] = min(f[l][r], f[l][k - 1] + f[k + 1][r]);  
- Leetcode 1000. 合并石头的最低成本: 石子合并变形。

### 0x50.4 状态压缩dp
	
- Leetcode 464 我能赢吗: 状态压缩dfs + 记忆化搜索。    
- Leetcode 526 优美的排列：状态压缩dp,f[i]代表选法是i的时候的方案数，i的每一位代表是否选择当前的数    	

## 0x60 图论
	
### 图遍历--拓扑排序
	
- Leetcode 207 课程表：拓扑排序(模板)，比较点数和入度为0的点数量，模板题  
	
#### 最短路径
	
【宫水三叶】涵盖所有的「存图方式」与「最短路算法（详尽注释）」：https://leetcode-cn.com/problems/network-delay-time/solution/gong-shui-san-xie-yi-ti-wu-jie-wu-chong-oghpz/

【宫水三叶】运用 Bellman Ford 求解有限制的最短路问题：https://leetcode-cn.com/problems/cheapest-flights-within-k-stops/solution/gong-shui-san-xie-xiang-jie-bellman-ford-dc94/
	
邻接表模板（算法竞赛进阶指南）： https://www.acwing.com/blog/content/4689/
	
边与点的存储方式:      
n 为点数，m 为边数   
- 1 邻接矩阵：flyod和朴素dijkstra   
稠密图：m ≈ nxn  
```C++
// 邻接矩阵数组：w[a][b] = c 代表从 a 到 b 有权重为 c 的边
int w[N][N];
// 加边操作
void add(int a, int b, int c) {
    w[a][b] = c;
}
```
- 2 邻接表：堆优化dijkstra、SPFA       
稀疏图：m ≈ n，链式前向星存图。   
```C++
const int N = 110, M = 6010, INF = 0x3f3f3f3f;
int h[N], e[M], w[M], ne[M], idx;

//e w n h,头插法，head初始化为-1
void add(int a, int b, int c) {
	e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}
```
- 3 结构体:bellman-ford  
Bellman-ford算法是一种动态规划算法，而Dijkstra算法是一种贪心算法。
Bellman-ford解边数限制的问题。  
```C++
struct Edge {
	int a, b, c;
} edges[M];
```         
- 4 vector数组
```C++   
vector<vector> edges;
vector<vector<int>> edges(numCourses);
vector<int> inDeg(numCourses, 0);
```
- 5 map存储
```C++
unordered_map<string, unordered_map<string, double>> edges;
unordered_set<string> vers;
```
Dijkstra基于贪心，只松弛最小权值点的所有出边，复杂度O(NxN)。     
Bellman Ford/SPFA都是基于动态规划，其原始的状态定义为f[i][k]代表从起点到i点，且经过最多k条边的最短路径。这样的状态定义引导我们能够使用 Bellman Ford 来解决有边数限制的最短路问题。bellman是动态规划创始人，bf算法对所有出边进行松弛，dijkstra只对最小权值点的出边进行松弛。bf复杂度O(NXM)。

同样多源汇最短路算法 Floyd也是基于动态规划，其原始的三维状态定义为f[i][j][k] 代表从点i到点j，且经过的所有点编号不会超过k（即可使用点编号范围为[1,k]）的最短路径。这样的状态定义引导我们能够使用 Floyd 求最小环或者求“重心点”（即删除该点后，最短路值会变大）。

Dijkstra：邻接矩阵，贪心算法。          
堆优化Dijkstra：邻接表，小顶堆。            
Bellman-ford：邻接矩阵，动态规划算法，n次松弛操作，先backup备份数组，然后直接对边集合进行遍历。         
SPFA: 邻接表，队列。        
Floyd：多源最短路，任意起点到任意终点的最短距离。三层循环:枚举中转点-->枚举起点-->枚举终点-->松弛操作。      

inf用0x3f3f3f3f或者INT_MAX/2;        
idx 从0开始，停止条件i != -1;idx 从1开始，停止条件~i。          
迭代顶点： for (auto & k:vers) 

```C++
//flyod
unordered_set<string> vers;
unordered_map<string, unordered_map<string, double>> edges;
for (int i = 0; i < equations.size(); i++) {
    string a = equations[i][0], b = equations[i][1];
    double c = values[i];
    vers.insert(a);
    vers.insert(b);
    edges[a][b] = c;
    edges[b][a] = 1/c;
}
for (auto & k:vers) {
    for (auto & i:vers) {
	for (auto & j:vers) {
	    if(edges[i][k] &&  edges[k][j])
	    edges[i][j] = edges[i][k] * edges[k][j];
	}
    }
}	
```
				     
```C++
//SPFA				    
class Solution {
    const static int N = 110, M = 6010;
    int edge[M], w[M], next[M], head[N];
    int dist[N];
    int n, k, idx;
    const int inf = INT_MAX / 2;
public:
    int networkDelayTime(vector<vector<int>> &times, int _n, int _k) {
        n = _n, k = _k;
        idx = 0;
        memset(edge, 0, sizeof(edge));
        memset(w, 0, sizeof(w));
        memset(next, 0, sizeof(next));
        memset(head, -1, sizeof(head));

        for (auto &time:times) {
            int a = time[0], b = time[1], val = time[2];
            add(a, b, val);
        }

        SPFA();
        int res = 0;
        for (int i = 1; i <= n; i++) {
            res = max(res, dist[i]);
        }
        return res >= inf ? -1 : res;
    }

    void add(int a, int b, int val) {
        edge[idx] = b, w[idx] = val, next[idx] = head[a], head[a] = idx;
        idx++;
    }

    void SPFA() {
        for (int i = 1; i <= n; i++) {
            dist[i] = inf;
        }
        dist[k] = 0;
        queue<int> q;
        q.emplace(k);
        while (q.size()) {
            auto x = q.front();
            q.pop();
            for (int i = head[x]; i != -1; i = next[i]) {
                int y = edge[i], val = w[i];
                if (dist[y] > dist[x] + val) {
                    dist[y] = dist[x] + val;
                    q.push(y);
                }
            }
        }
    }
};			     
```	
- Leetcode 399 除法求值：Floyd求最短路。
- Leetcode 743 网络延迟时间：Floyd  + 朴素 Dijkstra  + 堆优化 Dijkstra  + Bellman Ford（类 & 邻接表） +  SPFA（邻接表）模板题。
- Leetcode 785 判断二分图:DFS。
- Leetcode 787 K 站中转内最便宜的航班:经过不超过k个点，限制最多不超过k+1条边，Bellman Ford/SPFA 都是基于动态规划。解有边数限制的最短路问题。
- Leetcode 797 所有可能的路径:DFS。
	
### 欧拉路
	
给定一个 nnn 个点 mmm 条边的图，要求从指定的顶点出发，经过所有的边恰好一次（可以理解为给定起点的「一笔画」问题），使得路径的字典序最小。这种「一笔画」问题与欧拉图或者半欧拉图有着紧密的联系，下面给出定义：        
通过图中所有边恰好一次且行遍所有顶点的通路称为欧拉通路。            
通过图中所有边恰好一次且行遍所有顶点的回路称为欧拉回路。           
具有欧拉回路的无向图称为欧拉图。          
具有欧拉通路但不具有欧拉回路的无向图称为半欧拉图。        

- Leetcode 332 重新安排行程:欧拉通路。    

	

	
	
	
	
