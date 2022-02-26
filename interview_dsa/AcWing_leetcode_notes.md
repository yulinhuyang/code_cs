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

**树**

栈的递归、迭代（stack中序）

树的递归问题：枚举最高点 + 左右最大(当前)返回

LCA: 236. 二叉树的最近公共祖先，树天然递归

94. 二叉树的中序遍历： 栈的递归、迭代（stack中序）

124 二叉树中的最大路径和：枚举最高点 + 左右最大(当前)返回

**字符串**

模式： while (k < s.size() && s[k] == s[j]) k ++ ;

unordered_map<string,vector<string>> hash;

为什么 STL 中的容器和算法都是用的左闭右开区间？：https://www.zhihu.com/question/61054439

reverse、str.find、str.substr
 
string substr (size_t pos = 0, size_t len = npos) 


