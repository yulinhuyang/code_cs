
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
|438|Find all Anagrams in a string（找到字符串中所有字母异位词）|Medium|
|567|Permutation in String（字符串排列）|Medium|

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
|113|Path Sum II (路径总和 II)|Medium|

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
|62|Unique Paths（不同路径）|Medium|
|63|Unique Paths II|Medium|
|64|Minimum Path Sum（最小路径和）|Medium|
|53|Maximum Subarray（最大子序和）|Medium|
|152|Maximum Product Subarray（乘积最大子数组）|Medium|
|55|Jump Game（跳跃游戏 ）|Medium|
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

### 子序列问题

|序号|题目|级别|类别|
|:-----|:-----|:-----|:-----|
|128|Longest Consecutive Sequence（最长连续序列 ）|Hard|
|300|Longest Increasing Subsequence（LIS 最长上升子序列）|Medium|
|1143|Longest Common Subsequencee（LCS 最长公共子序列）|Medium|


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

# 进阶指南 + 基础课

# 0x00 基本算法(40)

包括位运算、递推、递归、二分、排序、倍增、贪心等算法。

## 0x01位运算

| 序号 | 题目 |
|:-----|:-----|
| AcWing89 | a^b |
| AcWing90 | 64位整数乘法 |
| AcWing91 | 最短Hamilton路径 |
| AcWing998 | 起床困难综合症 |
 

## 0x02递推与递归

| 序号 | 题目 |
|:-----|:-----|
| AcWing92 | 递归实现指数型枚举 |
| AcWing93 | 递归实现组合型枚举 |
| AcWing94 | 递归实现排列型枚举 |
| AcWing95 | 费解的开关 |
| AcWing96 | 奇怪的汉诺塔 |
| AcWing97 | 约数之和 |
| AcWing98 | 分形之城 |


## 0x03 前缀和与差分

| 序号 | 题目 |
|:-----|:-----|
| AcWing99 | 激光炸弹 |
| AcWing100 | IncDec序列 |
| AcWing101 | 最高的牛 |

## 0x04  二分和三分

| 序号 | 题目 |
|:-----|:-----|
| AcWing102 | 最佳牛围栏 |
| AcWing113 | 特殊排序 |

## 0x05 双指针与排序

| 序号 | 题目 |
|:-----|:-----|
| AcWing103 | 电影 |
| AcWing104 | 货仓选址 |
| AcWing105 | 七夕祭 |
| AcWing106 | 动态中位数 |
| AcWing107 | 超快速排序 |
| AcWing108 | 奇数码问题 |
 
## 0x06 倍增

| 序号 | 题目 |
|:-----|:-----|
| AcWing109 | 天才ACM |

## 0x07 贪心

| 序号 | 题目 |
|:-----|:-----|
| AcWing110 | 防晒 |
| AcWing111 | 畜栏预定 |
| AcWing112 | 雷达设备 |
| AcWing114 | 国王游戏 |
| AcWing115 | 给树染色 |

## 0x08 总结与练习

| 序号 | 题目 |
|:-----|:-----|
| AcWing116 | 飞行员兄弟 |
| AcWing117 | 占卜DIY |
| AcWing118 | 分形 |
| AcWing119 | 袭击 |
| AcWing120 | 防线 |
| AcWing121 | 赶牛入圈 |
| AcWing122 | 糖果传递 |
| AcWing123 | 士兵 |
| AcWing124 | 数的进制转换 |
| AcWing125 | 耍杂技的牛 |
| AcWing126 | 最大的和 |
| AcWing127 | 任务 |


# 0x10 基本数据结构完成情况(37)

包括栈、队列、链表与邻接表、Hash、字符串、Trie、二叉堆等内容。

## 0x11 栈/单调栈

| 序号 | 题目 |
|:-----|:-----|
| AcWing41 | 包含min函数的栈 |
| AcWing128 | 编辑器 |
| AcWing129 | 火车进栈 |
| AcWing130 | 火车进出栈问题 |
| AcWing131 | 直方图中最大的矩形 |

## 0x12 队列/单调队列

| 序号 | 题目 |
|:-----|:-----|
| AcWing132 | 小组队列 |
| AcWing133 | 蚯蚓 |
| AcWing134 | 双端队列 |
| AcWing135 | 最大子序和 |

## 0x13 链表与邻接表

| 序号 | 题目 |
|:-----|:-----|
| AcWing136 | 邻值查找 |

## 0x14  hash表(字符串hash)

| 序号 | 题目 |
|:-----|:-----|
| AcWing137 | 雪花雪花雪花 |
| AcWing138 | 兔子与兔子 |
| AcWing139 | 回文子串的最大长度 |
| AcWing140 | 后缀数组 |

## 0x15 字符串(KMP与最小表示法）

| 序号 | 题目 |
|:-----|:-----|
| AcWing141 | 周期 |

## 0x16  Trie树（字典树）

| 序号 | 题目 |
|:-----|:-----|
| AcWing142 | 前缀统计 |
| AcWing143 | 最大异或对 |
| AcWing144 | 最长异或值路径 |

## 0x17  二叉堆

| 序号 | 题目 |
|:-----|:-----|
| AcWing145 | 超市 |
| AcWing146 | 序列 |
| AcWing147 | 数据备份 |
| AcWing148 | 合并果子 |
| AcWing149 | 荷马史诗 |

## 0x18 总结与练习

| 序号 | 题目 |
|:-----|:-----|
| AcWing150 | 括号画家 |
| AcWing151 | 表达式计算 |
| AcWing152 | 城市游戏 |
| AcWing153 | 双栈排序 |
| AcWing154 | 滑动窗口 |
| AcWing155 | 内存分配 |
| AcWing156 | 矩阵 |
| AcWing157 | 树形地铁系统 |
| AcWing158 | 项链 |
| AcWing159 | 奶牛矩阵 |
| AcWing160 | 匹配统计 |
| AcWing161 | 电话列表 |
| AcWing162 | 黑盒子 |
| AcWing163 | 生日礼物 |


# 0x20 搜索(32)

包括树与图的遍历、深度优先搜索、剪枝、迭代加深、广度优先搜索、广搜变形、A*、IDA*等内容。


## 0x21 树与图的遍历

| 序号 | 题目 |
|:-----|:-----|
| AcWing164 | 可达性统计 |

## 0x22 深度优先搜索

| 序号 | 题目 |
|:-----|:-----|
| AcWing165 | 小猫爬山 |
| AcWing166 | 数独 |

## 0x23 剪枝

| 序号 | 题目 |
|:-----|:-----|
| AcWing167 | 木棒 |
| AcWing168 | 生日蛋糕 |
| AcWing169 | 数独 |

## 0x24 迭代加深

| 序号 | 题目 |
|:-----|:-----|
| AcWing170 | 加成序列 |
| AcWing171 | 送礼物 |

## 0x25 广度优先搜索

| 序号 | 题目 |
|:-----|:-----|
| AcWing172 | 立体推箱子 |
| AcWing173 | 矩阵距离 |
| AcWing174 | 推箱子 |

## 0x26 广搜变形

| 序号 | 题目 |
|:-----|:-----|
| AcWing175 | 电路维修 |
| AcWing176 | 装满的油箱 |
| AcWing177 | 噩梦 |


## 0x29 总结与练习

| 序号 | 题目 |
|:-----|:-----|
| AcWing183 | 靶形数独 |
| AcWing184 | 虫食算 |
| AcWing185 | 玛雅游戏 |
| AcWing186 | 巴士 |
| AcWing187 | 导弹防御系统 |
| AcWing188 | 武士风度的牛 |
| AcWing189 | 乳草的入侵 |
| AcWing190 | 字串变换 |
| AcWing191 | 天气预报 |
| AcWing192 | 立体推箱子 |
| AcWing193 | 算乘方的牛 |
| AcWing194 | 涂满它 |
| AcWing195 | 骑士精神 |

# 0x40 数据结构进阶(34)

包括并查集、树状数组、线段树、分块、点分治、二叉查找树与平衡树初步、离线分治算法、可持久化数据结构等内容。

## 0x41 并查集

| 序号 | 题目 |
|:-----|:-----|
| AcWing237 | 程序自动分析 |
| AcWing238 | 银河英雄传说 |
| AcWing239 | 奇偶游戏 |
| AcWing240 | 食物链 |
        
# 0x50 动态规划(72)

包括线性DP、背包、区间DP、树形DP、环形与后效性处理、状态压缩DP、倍增优化DP、数据结构优化DP、单调队列优化DP、斜率优化、四边形不等式、计数类 DP、数位统计DP等内容。


## 0x51 线性DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing271 | 杨老师的照相排列 |
| AcWing272 | 最长公共上升子序列 |
| AcWing273 | 分级 |
| AcWing274 | 移动服务 |
| AcWing275 | 传纸条 |
| AcWing276 | I-区域 |
| AcWing277 | 饼干 |

## 0x52 背包

| 序号 | 题目 |
|:-----|:-----|
| AcWing278 | 数字组合 |
| AcWing279 | 自然数拆分 |
| AcWing280 | 陪审团 |
| AcWing281 | 硬币 |

## 0x53 区间DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing282 | 石子合并 |
| AcWing283 | 多边形 |
| AcWing284 | 金字塔 |

## 0x54 树形DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing285 | 没有上司的舞会 |
| AcWing286 | 选课 |
| AcWing287 | 积蓄程度 |

## 0x55 环形与后效性处理

| 序号 | 题目 |
|:-----|:-----|
| AcWing288 | 休息时间 |
| AcWing289 | 环路运输 |
| AcWing290 | 坏掉的机器人 |

## 0x56 状态压缩DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing291 | 蒙德里安的梦想 |
| AcWing292 | 炮兵阵地 |
| AcWing529 | 宝藏 |

## 0x57 倍增优化DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing293 | 开车旅行 |
| AcWing294 | 计算重复 |

## 0x58 数据结构优化DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing295 | 清理班次 |
| AcWing296 | 清理班次 |
| AcWing297 | 赤壁之战 |

## 0x59 单调队列优化DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing298 | 围栏 |
| AcWing299 | 裁剪序列 |

## 0x5A 斜率优化

| 序号 | 题目 |
|:-----|:-----|
| AcWing300 | 任务安排 |
| AcWing301 | 任务安排 |
| AcWing302 | 任务安排 |
| AcWing303 | 运输小猫 |

## 0x5B 四边形不等式

| 序号 | 题目 |
|:-----|:-----|
| AcWing304 | 诗人小G |
| AcWing2889 | 再探石子合并 |
| AcWing305 | 一个古老的石头游戏 |

## 0x5C 计数类DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing306 | 杰拉尔德和巨型象棋 |
| AcWing307 | 连通图 |
| AcWing308 | 它们中的多少个 |
| AcWing309 | 装饰围栏 |

## 0x5D 数位统计DP

| 序号 | 题目 |
|:-----|:-----|
| AcWing310 | 启示录 |
| AcWing311 | 月之谜 |

## 0x5E 总结与练习

| 序号 | 题目 |
|:-----|:-----|
| AcWing312 | 乌龟棋 |
| AcWing313 | 花店橱窗 |
| AcWing314 | 低买 |
| AcWing315 | 旅行 |
| AcWing316 | 减操作 |
| AcWing317 | 陨石的秘密 |
| AcWing318 | 划分大理石 |
| AcWing319 | 折叠序列 |
| AcWing320 | 能量项链 |
| AcWing321 | 棋盘分割 |
| AcWing322 | 消木块 |
| AcWing323 | 战略游戏 |
| AcWing324 | 贿赂FIPA |
| AcWing325 | 计算机 |
| AcWing326 | XOR和路径 |
| AcWing1194 | 岛和桥 |
| AcWing327 | 玉米田 |
| AcWing328 | 芯片 |
| AcWing329 | 围栏障碍训练场 |
| AcWing330 | 估算 |
| AcWing331 | 干草堆 |
| AcWing332 | 股票交易 |
| AcWing333 | 最大子矩阵 |
| AcWing334 | K匿名序列 |
| AcWing335 | 特别行动队 |
| AcWing336 | 邮局 |
| AcWing337 | 扑克牌 |
| AcWing338 | 计数问题 |
| AcWing339 | 圆形数字 |

# 0x60 图论完成情况(73)

包括最短路、最小生成树、树的直径与最近公共祖先、基环树、负环与差分约束、Tarjan算法与无向图连通性、Tarjan算法与有向图连通性、二分图的匹配、二分图的覆盖与独立集、网络流初步

## 0x61 最短路

| 序号 | 题目 |
|:-----|:-----|
| AcWing340 | 通信线路 |
| AcWing341 | 最优贸易 |
| AcWing342 | 道路与航线 |
| AcWing343 | 排序 |
| AcWing344 | 观光之旅 |
| AcWing345 | 牛站 |

## 0x62 最小生成树

| 序号 | 题目 |
|:-----|:-----|
| AcWing346 | 走廊泼水节 |
| AcWing347 | 野餐规划 |
| AcWing348 | 沙漠之王 |
| AcWing349 | 黑暗城堡 |

## 0x63 树的直径与最近公共祖先

| 序号 | 题目 |
|:-----|:-----|
| AcWing350 | 巡逻 |
| AcWing351 | 树网的核 |
| AcWing352 | 闇の連鎖 |
| AcWing353 | 雨天的尾巴 |
| AcWing354 | 天天爱跑步 |
| AcWing355 | 异象石 |
| AcWing356 | 次小生成树 |
| AcWing357 | 疫情控制 |




