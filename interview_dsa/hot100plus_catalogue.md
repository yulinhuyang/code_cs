
# hot100+ labuladong + 随想录扩展

# 0x00 基本算法

前缀和、二分、双指针(排序、滑窗)

## 0x01位运算

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|136|Single Number（只出现一次的数字）|Medium|
|338|Counting Bits（比特位计数）|Medium|
|461|汉明距离  |Easy|

## 0x03 前缀和与差分

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|437|Path Sum III（路径总和 III）|Medium|
|560|和为K的子数组 |Medium|

## 0x04  二分和三分

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|33|Search in Rotated Sorted Array（搜索旋转排序数组）|Hard|
|34|Search for a Range（在排序数组中查找元素的第一个和最后一元素）|Medium|
|240|Search a 2D Matrix II（搜索二维矩阵 II ）|Medium|

## 0x05 双指针与排序

左右指针（同向、反向）：排好序，找一些组合满足某种条件

快慢指针：有环的链表和数组问题，如判断链表是否是回文

### 左右（快慢）指针

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|141|Linked List Cycle（环形链表）|Medium|
|142|Linked List Cycle II（环形链表II）|Medium|
|11|Container With Most Water（盛最多水的容器）|Medium|
|1|Two Sum（两数之和）|Easy|
|15|3Sum（三数之和）|Medium|
|16|3Sum Closest|Medium|
|31|Next Permutation（下一个排列）|Medium|
|75|Sort Colors（颜色分类）|Medium|
|215|Kth Largest Element in an Array（数组中的第K个最大元素）|Medium|


### 基础排序

**K-way merge，多路归并**

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|4|寻找两个正序数组的中位数 |Medium|
|21|Merge Two Sorted Lists（合并两个有序链表）|Easy|
|23|Merge k Sorted Lists（合并K个排序链表）|Hard|


### 双向遍历

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|42|Trapping Rain Water（接雨水）|Hard|
|581|Shortest Unsorted Continuous Subarray（最短无序连续子数组）|Easy|

### 回文问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|5|Longest Palindromic Substring（最长回文子串）|Medium|
|647|Palindromic Substrings（回文子串）|Medium|
|234|Palindrome Linked List（回文链表）|Easy|

### 滑动窗口

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|3|Longest Substring Without Repeating Characters（无重复字符的最长子串 ）|Medium|
|76|Minimum Window Substring（最小覆盖子串）|Hard|


### Cyclic Sort，循环排序

在排好序/翻转过的数组中，寻找丢失的/重复的/最小的元素

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|448|find all numbers disappeared in an array（找到所有数组中消失的数字）|Easy|
|48|Rotate Image（旋转图像）|Medium|
|169|Majority Element（多数元素）|Easy|

## 0x06 贪心

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|56|Merge Intervals（合并区间）|Hard|

# 0x10 基本数据结构

单调栈、单调队列、链表、二叉堆

## 0x11 栈/单调栈

### 栈基本操作

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|20|Valid Parentheses（有效的括号）|Easy|
|32|Longest Valid Parentheses（最长有效括号）|Hard|
|394|Decode String（字符串解码）|Medium|

### 单调栈

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|84|Largest Rectangle in Histogram（柱状图中最大的矩形）|Hard|
|85|Maximal Rectangle（最大矩形）|Hard|
|155|Min Stack（最小栈）|Easy|
|739|每日温度 |Medium|


## 0x12 队列/单调队列

### 单调队列

单调队列 + 滑动窗口

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|239|Sliding Window Maximum（滑动窗口最大值）|Hard|
|438|Find all Anagrams in a string（找到字符串中所有字母异位词）|Medium|

## 0x13 链表与邻接表

### 链表基本操作

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|2|两数相加|Easy|
|19|Remove Nth Node From End of List（删除链表的倒数第N个节点）|Easy|
|148|Sort List（排序链表）|Medium|
|160|Intersection of Two Linked Lists（相交链表）|Easy|

### 链表翻转

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|206|Reverse Linked List（反转链表）|Easy|

## 0x14  hash表(字符串hash)

高级结构设计 LRU/LFU

### 哈希表

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|1|Two Sum（两数之和）|Easy|
|49|Group Anagrams（字母异位词分组）|Medium|


### 数据结构设计 LRU/LFU

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|146|LRU Cache（LRU缓存机制 ）|Hard|
|460|LFU Cache|Hard|


## 0x15 字符串(KMP与最小表示法）

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|71|Simplify Path|Medium|
|1233|删除子文件夹|Medium|
|394|字符串解码(Decode String)|Medium|

## 0x16  Trie树（字典树）

文件目录问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|208|Implement Trie (Prefix Tree)（实现 Trie (前缀树) ）|Medium|


## 0x17  二叉堆

### ToP k问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|347|Top K Frequent Elements（前 K 个高频元素）|Medium|
|406|Queue Reconstruction by Height（根据身高重建队列）|Medium|

# 0x20  搜索

树遍历、DFS、BFS、动态规划

## 0x21 树与图的遍历

树的DFS（Tree Depth First Search，stack）

树的BFS(Tree Breadth First Search，queue)

图的遍历-拓扑排序 

### 树的DFS（Tree Depth First Search，stack）

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|236|Lowest Common Ancestor of a Binary Tree（二叉树的最近公共祖先  ）|Medium|
|94|Binary Tree Inorder Traversal（二叉树的中序遍历）|Medium|
|98|Validate Binary Search Tree（验证二叉搜索树）|Medium|
|101|Symmetric Tree（对称二叉树）|Easy|
|104|Maximum Depth of Binary Tree（二叉树的最大深度）|Easy|
|105|Construct Binary Tree from Preorder and Inorder Traversal（从前序与中序遍历序列构造二叉树）|Medium|
|114|Flatten Binary Tree to Linked List（二叉树展开为链表）|Medium|
|124|Binary Tree Maximum Path Sum（二叉树中的最大路径和）|Hard|
|226|Invert Binary Tree（ 翻转二叉树 ）|Easy|
|236|Lowest Common Ancestor of a Binary Tree（二叉树的最近公共祖先  ）|Medium|

### 树的BFS(Tree Breadth First Search，queue)

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|102|Binary Tree Level Order Traversal（二叉树的层序遍历 ）|Easy|
|103|Binary Tree Zigzag Level Order Traversal|Medium|

### 图遍历--拓扑排序

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|207|Course Schedule（课程表）|Medium|
|621| 任务调度器  |Medium|


## 0x22 DFS(递归、回溯)

排列组合子集

flood fill问题

省份问题（DFS版） 岛屿问题

### 组合问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|17|Letter Combinations of a Phone Number（电话号码的字母组合）|Medium|
|22|括号生成|Medium|
|301|Remove Invalid Parentheses（删除无效的括号）|Hard|


### 子集问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|39|Combination Sum（组合总和）|Medium|
|46|Permutations（全排列）|Medium|
|78|Subsets（子集）|Medium|

### DFS floodfill问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|79|Word Search（单词搜索）|Medium|
|200|Number of Islands（岛屿数量）|Medium|
|547|省份数量(Number Of Provinces)|Medium|


## 0x23 BFS

路径障碍、迷宫问题、省份问题（简化BFS）

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|102|Binary Tree Level Order Traversal（二叉树的层序遍历 ）|Easy|
|547|省份数量(Number Of Provinces)|Medium|

## 0x41  数据结构进阶 - 并查集

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|399|Evaluate Division（除法求值）|Medium|
|547|省份数量(Number Of Provinces)|Medium|

## 0x50 动态规划

### 经典动归

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|10|Regular Expression Matching（正则表达式匹配 ）|Hard|
|72|Edit Distance（编辑距离）|Hard|
|139|Word Break（单词拆分）|Medium|
|70|Climbing Stairs（爬楼梯）|Easy|
|300|Longest Increasing Subsequence（最长上升子序列）|Medium|
|62|Unique Paths（不同路径）|Medium|
|63|Unique Paths II|Medium|
|64|Minimum Path Sum（最小路径和）|Medium|
|53|Maximum Subarray（最大子序和）|Medium|
|152|Maximum Product Subarray（乘积最大子数组）|Medium|
|55|Jump Game（跳跃游戏 ）|Medium|
|128|Longest Consecutive Sequence（最长连续序列 ）|Hard|
|221|最大正方形|Medium|
|238|Product of Array Except Self（除自身以外数组的乘积）|Easy|
|312|Burst Balloons（戳气球）|Hard|

### 背包问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|279|Perfect Squares（完全平方数）|Medium|
|322|Coin Change（零钱兑换）|Medium|
|416|Partition Equal Subset Sum（分割等和子集）|Medium|
|494|目标和  |Medium|

### 股票问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|121|Best Time to Buy and Sell Stock（买卖股票的最佳时机）|Medium|
|122|Best Time to Buy and Sell Stock II|Medium|
|123|Best Time to Buy and Sell Stock III|Hard|

### 打家劫舍问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|198|House Robber（打家劫舍）|Easy|
|337|House Robber III（打家劫舍 III ）|Medium|



# 剑指offer + 专项突破版拓展


# 0x00 基本算法

### 0x01 位运算

15. 二进制中 1 的个数 
56. 数组中只出现一次的数字 


剑指 Offer II 005. 单词长度的最大乘积(318 maximum-product-of-word-lengths)


## 0x04  二分和三分

4. 二维数组中的查找 
11. 旋转数组的最小数字 
53. 数字在排序数组中出现的次数 

剑指 Offer II 073. 狒狒吃香蕉(875 koko-eating-bananas)


## 0x05 双指针与排序


57.1 和为 S 的两个数字
57.2 和为 S 的连续正数序列
58.1 翻转单词顺序列
58.2 左旋转字符串

剑指 Offer II 007. 数组中和为 0 的三个数(15 3sum)

剑指 Offer II 018. 有效的回文(125 valid-palindrome)

剑指 Offer II 020. 回文子字符串的个数(647 palindromic-substrings)

### 排序

21. 调整数组顺序使奇数位于偶数前面 
45. 把数组排成最小的数 
51. 数组中的逆序对 


### 滑动窗口

剑指 Offer II 009. 乘积小于 K 的子数组(713 subarray-product-less-than-k)

剑指 Offer II 014. 字符串中的变位词(567 permutation-in-string)

剑指 Offer II 015. 字符串中的所有变位词(438 find-all-anagrams-in-a-string)

剑指 Offer II 016. 不含重复字符的最长子字符串(3 longest-substring-without-repeating-characters)

剑指 Offer II 017. 含有所有字符的最短字符串(76 minimum-window-substring)

### Cycle Sort

3. 数组中重复的数字 
5. 替换空格 
29. 顺时针打印矩阵 
50. 第一个只出现一次的字符位置 

剑指 Offer II 071. 按权重生成随机数(528 random-pick-with-weight)


## 0x06 贪心

14. 剪绳子 
63. 股票的最大利润 


# 0x10 基本数据结构

## 0x11 栈/单调栈

9. 用两个栈实现队列
30. 包含 min 函数的栈
31. 栈的压入、弹出序列

## 0x12 单调队列

59. 滑动窗口的最大值


## 0x13 链表与邻接表

6. 从尾到头打印链表 
18.1 在 O(1) 时间内删除链表节点 
18.2 删除链表中重复的结点 
22. 链表中倒数第 K 个结点 
23. 链表中环的入口结点 
24. 反转链表 
25. 合并两个排序的链表 
35. 复杂链表的复制 
52. 两个链表的第一个公共结点 


## 0x15 字符串(KMP与最小表示法）

19. 正则表达式匹配 
20. 表示数值的字符串 
46. 把数字翻译成字符串 
67. 把字符串转换成整数 


## 0x16  Trie树（字典树）

剑指 Offer II 063. 替换单词(648 replace-words)

剑指 Offer II 064. 神奇的字典(676 implement-magic-dictionary)

剑指 Offer II 065. 最短的单词编码(820 short-encoding-of-words)

剑指 Offer II 066. 单词之和(677 map-sum-pairs)

## 0x17 二叉堆

40. 最小的 K 个数
41.1 数据流中的中位数
41.2 字符流中第一个不重复的字符



## 0x21 树与图的遍历

### 树的遍历

7. 重建二叉树 
8. 二叉树的下一个结点
22 链表中倒数第k个节点
26. 树的子结构 
27. 二叉树的镜像 
28. 对称的二叉树 
32.1 从上往下打印二叉树 
32.2 把二叉树打印成多行 
32.3 按之字形顺序打印二叉树 
33. 二叉搜索树的后序遍历序列 
34. 二叉树中和为某一值的路径 
36. 二叉搜索树与双向链表 
37. 序列化二叉树 
54. 二叉查找树的第 K 个结点 
55.2 平衡二叉树 
68. 树中两个节点的最低公共祖先 


### 图遍历--拓扑排序

剑指 Offer II 113. 课程顺序(210 course-schedule-ii)

剑指 Offer II 115. 重建序列(444 sequence-reconstruction)


## 0x22 DFS(递归、回溯)

12. 矩阵中的路径 
38. 字符串的排列 


## 0x23 BFS

13. 机器人的运动范围

剑指 Offer II 106. 二分图(785 is-graph-bipartite)

剑指 Offer II 107. 矩阵中的距离(542 01-matrix)

剑指 Offer II 109. 开密码锁(752 open-the-lock)

剑指 Offer II 116. 省份数量(547 number-of-provinces)

剑指 Offer II 114. 外星文字典(269 alien-dictionary)

剑指 Offer II 112. 最长递增路径(329 longest-increasing-path-in-a-matrix)

## 0x41  数据结构进阶 - 并查集

剑指 Offer II 118. 多余的边(684 redundant-connection)

剑指 Offer II 117. 相似的字符串(839 similar-string-groups)

## 0x50 动态规划

10.1 斐波那契数列 
10.2 矩形覆盖 
10.3 跳台阶 
10.4 变态跳台阶 
42. 连续子数组的最大和 
47. 礼物的最大价值 
48. 最长不含重复字符的子字符串 
49. 丑数 
60. n 个骰子的点数 
66. 构建乘积数组 

剑指 Offer II 105. 岛屿的最大面积(695 max-area-of-island)


## 其它

39. 数组中出现次数超过一半的数字 
62. 圆圈中最后剩下的数 
43. 从 1 到 n 整数中 1 出现的次数 

17. 打印从 1 到最大的 n 位数 
44. 数字序列中的某一位数字 
61. 扑克牌顺子 
64. 求 1+2+3+...+n 
65. 不用加减乘除做加法 





