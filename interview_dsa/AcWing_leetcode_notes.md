**代码简化方法**

同类型变量定义，一句可以定义多个。

for  while 单句的内循环不加括号。

灵活利用for  while的循环判断条件，单个for内灵活运用多个变量同时循环。

灵活使用三目运算符

灵活使用连续等号

```c++
int a = 0,b = 0;

while (j < s.size() && s[j] != ' ') j ++ ;

 for (auto c:hash[u - '2'])
     for (auto path:res)
         now.push_back(path + c);
         
for (int j = i, k = 2 * (n - 1) - i; j < s.size() || k < s.size(); j += 2 * (n - 1), k += 2 * (n - 1))
{
    if (j < s.size()) res += s[j];
    if (k < s.size()) res += s[k];
}

int b = y == j ? 0 : atoi(s2.substr(j, y - j).c_str());

st[i] = d[i + u] = ud[i - u + n] = 1;
```

**二分**

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


相关题目：

Leetcode  153. 寻找旋转排序数组中的最小值：nums.back()的秒用。

Leetcode 33. 搜索旋转排序数组:先找最小值，去掉干扰条件

offer 11 旋转数组的最小数字：先去掉干扰条件。

Offer 03 数组中重复的数字：反复交换法

**链表**  

链表画图看，添加虚拟头节点(涉及到头节点可能会变的情况下)

 <div align="center"> <img src="../pics/lianbiao1.png" width="50%"/> </div><br>
 
 涉及到要修改头节点的时候，需要dummy,
 
 19 删除链表的倒数第N个节点：
```
auto dummy = new ListNode(-1);
dummy->next = head;
auto fast = dummy,slow = dummy;
 
//链表循环模式
while(cur->next && cur->val == cur->next->val) cur->next = cur->next->next;
//判空是判断cur还是cur->next

//链表反转迭代
 ListNode* reverseList(ListNode* head) {
     ListNode* temp; // 保存cur的下一个节点
     ListNode* cur = head;
     ListNode* pre = NULL;
     while(cur) {
         temp = cur->next;  // 保存一下 cur的下一个节点，因为接下来要改变cur->next
         cur->next = pre; // 翻转操作
         // 更新pre 和 cur指针
         pre = cur;
         cur = temp;
     }
     return pre;
 }
 
//链表反转递归 carl版
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
```
链表Cout调试

**树**

栈的递归、迭代（stack中序）

树的递归问题：枚举最高点 + 左右最大(当前)返回

LCA: 236. 二叉树的最近公共祖先，树天然递归

94. 二叉树的中序遍历： 栈的递归、迭代（stack中序）

124 二叉树中的最大路径和：枚举最高点 + 左右最大(当前)返回

**字符串**

模式： 
    while (k < s.size() && s[k] == s[j]) k ++ ;  
    while (k < s.size() && s[k] == ' ') k ++ ;

unordered_map<string,vector<string>> hash;

为什么 STL 中的容器和算法都是用的左闭右开区间？：https://www.zhihu.com/question/61054439

reverse、str.find
 
string substr (size_t pos = 0, size_t len = npos)  //如果没有n,默认到末尾，长度是len,包含pos + len位置的字符。
 
to_string() 转字符串

stoi或 atoi(s1.substr(i, x - i).c_str())
 
leetcode 3 无重复字符的最长字串: hash逐加逐减, ij双变量循环代替滑窗。

leetcode 17 电话号码的字母组合：三重简单循环代替dfs
 
**DFS+回溯**

bfs一般求最小步数和最短距离，dfs状态数量非常大，但是解的数量很小。
 
leetcode 47 全排列II  遍历的时候跳过相同元素，人为规定相同数字的相对顺序不变 
 
```
while(i + 1 < nums.size() && nums[i] == nums[i+1]) i++;
```
                         
leetcode 78 子集  位运算+循环,简化子集问题, 递归结构相当于一层循环。

leetcode 90 子集II  回溯去重

LeetCode 52. N-Queens II    dfs代替行, col + d + ud 数组标记解法，精确覆盖问题。
 
LeetCode 37. Sudoku Solver  dfs 逐行, col + row + cell 数组标记解法，row[9][9]: 9行,0-9的数有了哪几个
               
Leetcode 473. 火柴拼正方形   剪枝优化技巧
 
**滑动窗口、双指针、单调队列、单调栈**

Leetcode 167. 两数之和 II - 输入有序数组： 首尾指针
                         
Leetcode 88. 合并两个有序数组：双指针从后向前合并。                         
                         
Leetcode 26. 删除有序数组中的重复项：前后指针                
                         
LeetCode 76. 最小覆盖字串:  单个hash + i(hash[key]--)  j(hash[key]++) 双指针  简化滑动窗口问题 
                         
Leetcode 32. 最长有效括号：起始加-1,栈底元素为当前已经遍历过的元素中「最后一个没有被匹配的右括号的下标]                
                    
Leetcode 42. 接雨水:两遍max, 左边一遍Left_max，右边一遍right_max，min(left_max[i],right_max[i]) - height[i]

Leetcode 84. 柱状图中最大的矩形:两遍栈，左边一遍单调上升栈，右边一遍单独上升栈，height[i] * (right[i] - left[i] - 1)
```C++
//单调栈代码段
while (stk.size() && heights[stk.top()] >= heights[i]) stk.pop();
if (stk.empty()) left[i] = -1;
else left[i] = stk.top();
stk.push(i);
```                       
Leetcode 239. 滑动窗口最大值:单调下降队列简化
                         
```C++
//单调队列代码段
if (q.size() && i - k >= q.front()) q.pop_front();
while (q.size() && nums[q.back()] <= nums[i]) q.pop_back();
q.push_back(i);
if (i >= k - 1) res.push_back(nums[q.front()]);
```         
                        
leetcode 918: 拆环为链 + 前缀和 +单调上升队列

前缀和：A从1开始,S从1开始,for从1开始,S[i] = S[i - 1] + A[i]。A从0开始,S从1开始(n+1),for从0开始,S[i+1] = S[i] + A[i]。默认S[0] = 0，S一定从1开始。

单调队列：针对前缀和S的,所以i-k >= n 有等号。 
 
**基本数据结构**
 
Leetcode 187. 重复的DNA序列: unordered_map(hash) 使用。
 
Leetcode 652. 寻找重复的子树: 中序遍历与后序或者中序遍历与前序遍历都可以唯一确定一棵树。两遍hash,第一遍树变成字符串，第二遍变成数。

Leetcode 560. 和为 K 的子数组: 简化的前缀和。
 
LeetCode 547. Friend Circles :并查集
 
LeetCode 684. Redundant Connection: 并差集变形，1->n 下标从1开始。
 
Leetcode 692. 前K个高频单词:小顶堆(大顶key加负号)流过程,保证second的字典序排序，也可以自定义比较函数。
 
pair 设置 --> make_pair或者 直接构造 PIS t(-word.second, word.first);
 
优先队列默认大的在队首，字符串则为字典序由大到小。int，double，char，string类型都可以这样定义。
 
LeetCode 295. Find Median from Data Stream：对顶堆，小上大下 --> 小右大左。动态流，图参考：https://blog.csdn.net/jiahonghao2002/article/details/114108760
 
1 2 3 4 (大顶) | 5 6 7 8(小顶) : right.size() > left.size(), right 比left至多多一个,添加时优先添加right, 先添加后调整。

**动态规划**
 
 LeetCode 120. Triangle：滚动数组优化版 + 普通二维数组版本
 
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
 
LeetCode 63. 不同路径 II：循环内处理好0行0列。

LeetCode 322. 零钱兑换 I: 0-1 背包的二维和一维解法。
         
LeetCode 518. 零钱兑换 II: 完全背包的二维和一维解法。
 
 
