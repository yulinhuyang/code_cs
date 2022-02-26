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
 

 

 
 


