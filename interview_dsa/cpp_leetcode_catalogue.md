# hot题单 + labuladong/随想录扩展 + acwing

【题型整理】LeetCode题型总结(持续更新)：https://www.acwing.com/file_system/file/content/whole/index/content/1448335/

hot100: https://leetcode-cn.com/problem-list/2cktkvj/

LeetCode究极班: https://www.acwing.com/activity/content/activity_person/content/29799/1/

# 0x00 基本算法

前缀和、二分、双指针(排序、滑窗)

## 0x01 位运算

- 67 二进制求和（高精度加法）
- 136 只出现一次的数字
- 137 只出现一次的数字 II：32位统计法。  
- 260 只出现一次的数字 II :借助lowbit，借k位区分分别^。
- 201 数字范围按位与：寻找公共前缀。
- 338 比特位计数    
- 371 两整数之和：位运算计算加法：拆分无进位加法和进位之和。带符号数赋值一个超范围的数，结果undefined,无符号数赋值一个超范围的数，结果是对无符号数总数取模的余数。
- 461 汉明距离
- 477 汉明距离总和：n x (n - c)   

## 0x03 前缀和与差分
   
- 113 路径总和II:树上的前缀和。
- 437 路径总和 III:树上的前缀和。
- 525 连续数组: 简易前缀和，hash[0] = -1;    
- 560 和为K的子数组:简化的前缀和，hash[0] = 1。
- 303 区域和检索-数组不可变:一维前缀和模板题，索引从1开始。
- 304 二维区域和检索-矩阵不可变:二维前缀和模板题。
- 1094. 拼车：一维差分。
- 1109. 航班预订统计：一维差分，数组索引从1开始的，要转换。


## 0x04  二分和三分

### 索引二分

- 33 搜索旋转排序数组:先找最小值，去掉干扰条件。     
- 34  在排序数组中查找元素的第一个和最后一元素
- 74 搜索二维矩阵（坐标变换：二维到一维）
- 81 搜索旋转排序数组Ⅱ  
- 153 寻找旋转排序数组中的最小值：numsback()的秒用。
- 162 寻找峰值 
- 240 搜索二维矩阵 II: %m
- 274 H指数（排序）
- 540 有序数组中的单一元素（trick：x^1的奇偶性）
- 704 二分查找（二分模板题）
- 475 供暖器:判定转二分

### 值域二分

- 69 x的平方根
- 367 有效的完全平方数
- 374 猜数字大小
- 287 寻找重复数（鸽巢原理+二分）
- 378 有序矩阵中第K小的元素
- 410 分割数组的最大值  

## 0x05 双指针与排序

同向（快慢）指针、反向（首尾）指针

快慢指针：有环的链表和数组问题，如判断链表是否是回文

### 同向（快慢）指针
   
- 31 下一个排列    
- 88 合并两个有序数组：双指针从后向前合并。
- 26 删除有序数组中的重复项：前后指针
- 75 颜色分类（三指针）    

### 反向（首尾）指针

- 11 盛最多水的容器 
- 15 三数之和
- 16 3Sum Closest 
- 167 两数之和 II - 输入有序数组：

### 滑动窗口

- 3 无重复字符的最长子串    
- 76 最小覆盖子串(Minimum Window Substring)：单个hash + i(hash[key]--) j(hash[key]++) 双指针 简化滑动窗口问题  
- 438 找到字符串中所有字母异位词(Anagrams)：hash + 26字母统计    
- 567 字符串排列(Permutation) 
   
### 基础排序

多路归并(K-way merge)、逆序对问题
    
- 4 寻找两个正序数组的中位数：递归。     
- 23 合并K个排序链表(多路归并)
- 313 超级丑数(多路归并)
- 147 对链表进行插入排序
- 148 排序链表（链表归并排序）

### 双向遍历

- 42 接雨水:两遍max, 左边一遍Left_max，右边一遍right_max，min(left_max[i],right_max[i]) - height[i]
- 238 除自身以外数组的乘积(Product of Array Except Self)     
- 581 最短无序连续子数组  

### 回文问题
    
- 5 最长回文子串：奇偶两边双指针，中心扩展法    
- 647 回文子串     
- 234 回文链表     

### Cyclic Sort，循环排序

在排好序/翻转过的数组中，寻找丢失的/重复的/最小的元素
    
- 448 找到所有数组中消失的数字    
- 48 旋转图像：二维数组原地旋转  


## 0x06 贪心
    
- 860 柠檬水找零:贪心，优先使用10块的。
- 455 分发饼干：贪心+排序。
- 55 跳跃游戏：
- 45 跳跃游戏 II：正向查找可到达的最大位置。
- 376 摆动序列：贪心模拟，统计峰和谷，erase+unique预处理。
- 406 根据身高重建队列：类信封问题，vector两维度排序再insert。
- 402 移掉 K 位数字：单调上升栈。
- 134 加油站：枚举点，下次从枚举失败+1处开始：i += j + 1。

**区间贪心**

- 56 合并区间(Merge Intervals)：区间问题，模板题
- 57 插入区间：区间问题   
- 452 用最少数量的箭引爆气球：贪心+排序，区间问题，按终点排序。  
- 435 无重叠区间

# 0x10 基本数据结构

单调栈、单调队列、链表、二叉堆

## 0x11 栈/单调栈

### 栈基本操作

括号类问题

- 20 有效的括号(Parentheses)     
- 32 最长有效括号
- 150 逆波兰表达式求值:逆波兰表达式/计算器    

### 单调栈

- 32 最长有效括号：起始加-1,栈底元素为当前已经遍历过的元素中「最后一个没有被匹配的右括号的下标]
- 84 柱状图中最大的矩形:两遍栈，左边一遍单调上升栈，右边一遍单独上升栈，height[i] x (right[i] - left[i] - 1)，模板题    
- 85 最大矩形(Maximal Rectangle)  
- 155 最小栈(Min Stack)    
- 739 每日温度 
- 496 下一个更大元素 I :反向遍历，单调栈，存值或者索引
- 503 下一个更大元素 II：拆环为链反向循环法 / 环形%正向模拟 
- 456 132 模式:反向遍历单调栈。

## 0x12 队列/单调队列

### 单调队列

单调队列

- 239 滑动窗口最大值(Sliding Window Maximum):单调下降队列简化,模板题   
- 918 环形子数组的最大和: 拆环为链 + 前缀和 +单调上升队列

## 0x13 链表与邻接表

### 链表基本操作

- 2 两数相加     
- 19 删除链表的倒数第N个节点(Remove Nth Node)：dummy  + 快慢指针，涉及头结点的改dummy     
- 21 合并两个有序链表
- 24 两两交换链表中的节点
- 61 旋转链表（快慢指针）
- 82 删除排序链表中的重复元素Ⅱ
- 83 删除排序链表中的重复元素
- 141 环形链表    
- 142 环形链表II 
- 148 排序链表(Sort List)    
- 160 相交链表(Intersection)     

- 206 反转链表(Reverse Linked List)：递归/pre+cur+next     

## 0x14  hash表

高级结构设计 LRU/LFU

### 哈希表

- 1 两数之和     
- 3 无重复字符的最长字串: hash逐加逐减, ij双变量循环代替滑窗。 
- 17 电话号码的字母组合：三重简单循环代替dfs
- 49 字母异位词分组(Group Anagrams)
- 187 重复的DNA序列: unordered_map(hash) 使用。 
- 350 两个数组的交集 II: multiset删除一个元素 mulSeterase(mulSetfind(num));
- 290 单词规律：stringstream 相当于sscanf和sprintf, getline截断string, 双hash
- 149 直线上最多的点数： hash存储 long double slope，竖直线单独考虑
- 454 四数相加 II：分组hash
- 554 砖墙：hash+贪心


### 数据结构设计 LRU/LFU

- 146 LRU Cache（LRU缓存机制）: hash + 双向链表，头插尾删，delete  
- 460 LFU Cache: 

LFU： https://leetcode-cn.com/problems/lfu-cache/solution/gong-shui-san-xie-yun-yong-tong-pai-xu-s-53m3/

https://www.acwing.com/activity/content/code/content/555766/

 1  key_table(unordered_map<key,Node) + set<Node>(代替堆)，因为C++中堆pq的值不能改，Node是自定义排序的结构体。       
 2  key_table(unordered_map<key, list<Node>::iterator>) + freq_table(unordered_map<freq, list<Node>>) ：链接法。       
 散列冲突解决：链接法、开发定址法。类似邻接表结构，展开的list链表。类似图的存储结构map + map。   
 
 map存储结构体，需要insert或emplace后使用make_pair,不能直接赋值   
 
 ```C++
 struct ABC{
    int x,y;
    ABC(int _x,int _y):x(_x),y(_y){};
};
 
unordered_map<int,ABC> hash;
ABC abc(1,1);
hash.insert(make_pair(1,abc));//正确插入
//hash[1] = abc;//错误插入
ABC ac(1,2);
auto it = hash.find(1);
it->second = ac;//正确更新

```
 
结构体定义初始化列表和自定义排序结构
 
```C++
 struct Node {
    int freq, time;
    int key, val;

    Node(int _freq, int _time, int _key, int _val) : freq(_freq), time(_time), key(_key), val(_val) {}

    bool operator< (const Node &rhs) const {
        return freq == rhs.freq ? time < rhs.time : freq < rhs.freq;
    }
}; 
 
```

## 0x15 字符串(字符串hash、KMP与最小表示法）

- 6 Z 字形变换
- 71  Simplify Path  
- 165 比较版本号：string基本使用  
- 1233  删除子文件夹    
- 394  字符串解码(Decode String)：栈解法/DFS递归括号()解法(j是（ 的下一个,k暂存和统计)。    
- 214 Shortest Palindrome：字符串hash131或改进kmp(最大前缀和最长后缀匹配)，哈希检索算法（Robin-Karp，RK 算法），模板题。
long long使用：最终结果是int,中间可能是long long。
- 929 独特的电子邮件地址：hash + string 

### 模拟

- 67 二进制求和：string的两数之和，字符模拟。
- 299 猜数字游戏：字符模拟。
- 12 整数转罗马数字:字符模拟。

### 子序列问题

- 392 判断子序列: 子序列模板。
- 522 子序列判断：模板题，判断子序列1。
- 523 连续的子数组和：前缀和+hash+ 间隔2 insert。
- 524 通过删除字母匹配到字典里最长单词:判断子序列2，字母序最小。
- 68 文本左右对齐：

## 0x16  Trie树（字典树）

文件目录问题
    
- 208 实现 Trie (前缀树)：   
- 211 添加与搜索单词-数据结构设计
- 212 单词搜索Ⅱ：DFS+Trie优化 
- 421 数组中两个数的最大异或值：

## 0x17  二叉堆

### ToP k问题

- 215 数组中的第K个最大元素（TopK）:排序后取
- 295 Find Median from Data Stream：对顶堆，小上大下 --> 小右大左。    
动态流，图参考：https://blogcsdnnet/jiahonghao2002/article/details/114108760    
1 2 3 4 (大顶) | 5 6 7 8(小顶) : rightsize() > leftsize(), right 比left至多多一个,添加时优先添加right, 先添加后调整。    
- 480 滑动窗口中位数:对顶堆 
- 347 前K个高频元素(Top K)
- 373 查找和最小的K对数字(TopK)
- 451 根据字符出现频率排序(Top K)   
- 692 前K个高频单词:小顶堆(大顶key加负号)流过程,保证second的字典序排序，也可以自定义比较函数。 pair 设置 --> make_pair或者 直接构造 PIS t(-wordsecond, wordfirst);   


# 0x20  搜索

树遍历、DFS、BFS、动态规划

## 0x21 树与图的遍历

树的DFS（Tree Depth First Search，stack）     
树的BFS(Tree Breadth First Search，queue)     

### 树的DFS（Tree Depth First Search，stack）

- 94 二叉树的中序遍历：栈的递归、迭代（stack中序）     
- 98  验证二叉搜索树：左子树所有节点都小于当前节点的值，右子树所有节点都大于当前节点的值。
- 101 对称二叉树(Symmetric Tree):     
- 104 二叉树的最大深度(Maximum Depth of Binary Tree)    
- 105 从前序与中序遍历序列构造二叉树(Preorder/Inorder):恢复必须要有中序才行。   
- 114 二叉树展开为链表(Flatten Binary Tree to Linked List)   
- 124 二叉树中的最大路径和：枚举最高点 + 左右最大(当前)返回 
- 144 二叉树的前序遍历
- 145 二叉树的后序遍历
- 173 二叉搜索树迭代器:stack中序遍历    
- 226 翻转二叉树         
- 236 二叉树的最近公共祖先：LCA问题
- 538 把二叉搜索树转换为累加树 
- 543 二叉树的直径   
- 652 寻找重复的子树: 中序遍历与后序或者中序遍历与前序遍历都可以唯一确定一棵树。两遍hash,第一遍树变成字符串，第二遍变成数。
 
  
### 树的BFS(Tree Breadth First Search，queue)

- 102 二叉树的层序遍历：BFS
- 103 二叉树的锯齿形层次遍历(zigzag):   

## 0x22 DFS(递归、回溯)

bfs一般求最小步数和最短距离，dfs状态数量非常大，但是解的数量很小。

排列组合子集、8皇后问题、木棍、flood fill问题(岛屿问题)

memo记忆化搜索、剪枝优化

### 组合\子集\排列

- 17  Letter Combinations of a Phone Number（电话号码的字母组合）    
- 22  括号生成    
- 39  组合总和(Combination Sum)
- 301 删除无效的括号     
  
- 46 全排列(Permutations)    
- 47 全排列II 遍历的时候跳过相同元素，人为规定相同数字的相对顺序不变

- 78 子集 位运算+循环,简化子集问题, 递归结构相当于一层循环。
- 90 子集II 回溯去重

### DFS典型（八皇后/数独、木棍）

- 52 N-Queens II dfs代替行, col + d + ud 数组标记解法，精确覆盖问题。模板题。
- 37 Sudoku Solver dfs 逐行, col + row + cell 数组标记解法，row[9][9]: 9行,0-9的数有了哪几个。模板题。
- 473 火柴拼正方形:剪枝优化技巧，模板题
- 54 螺旋矩阵：右下左上-> dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0}，d取索引。
- 93 复原IP地址:dfs + u/k两个截止条件，类似木棍问题。
- 95 不同的二叉搜索树 II：dfs区间l-r，枚举根节点。
- 394 字符串解码:栈解法/DFS递归括号()解法(j是(的下一个,k暂存和统计)。
- 529 扫雷游戏：dfs 8连通遍历，两个截止条件dfs。   
- 329 矩阵中的最长递增路径：记忆化dfs,memo,既当visit又存储值
- 784 字母大小写全排列：dfs+选/不选，大小写切换^32。
- 63 不同路径 II：循环内处理好0行0列。

### DFS floodfill问题

- 79 Word Search（单词搜索）    
- 200 Number of Islands（岛屿数量）    
- 547 省份数量(Number Of Provinces) 
- 695 岛屿最大面积：遍历枚举dfs。
- 733 Flood Fill：dfs的flood fill方法，搜索连通块。
- 130 被围绕的区域：四边dfs + 涂色法
- 417 太平洋大西洋水流问题（Flood Fill）

## 0x23 BFS

bfs 空间大，最短最小性，不会有爆栈风险。

dfs空间和递归深度成正比，有爆栈风险，比如树的深度最坏可能有1000层，不能搜最短最小。

迷宫问题、双向bfs(单词接龙)、0-1矩阵问题(多源bfs)、dfs + bfs结合
 
- 547 省份数量(Number Of Provinces) 
- 127 单词接龙: 双向BFS搜索（26子母扩展）
- 126 单词接龙 II：BFS(26扩展) + DFS(反向搜索)
- 542 01 Matrix：矩形多源头的bfs，类比入度为0的拓扑排序。BFS有两种，一种内循环需要先取size，如树的层序遍历,另外一种不需要，如四方向遍历，有dist记录。
- 934 最短的桥: DFS + 多源BFS最短路径, 先深度搜索，确度一座岛的边界,再广度搜索，查找连通的路径。 

## 0x30 数学知识

- 169 多数元素(Majority)：摩尔投票法
- 229 求众数2（摩尔投票法)
- 268 丢失的数字:求和公式 n x (n + 1)/2。
- 462 最少移动次数使数组元素相等 II:邮局问题，nth_element求中位数。
- 458 可怜的小猪：ToTes/ToDie的k进制数，累积。
- 319 灯泡开关: sqrt(n)。 上取整： (n + k - 1)/k。
- 223 矩形面积：计算几何，矩阵并集面积,+0ll转longlong。   
- 528 按权重随机选择:前缀和 + rand()。

- 95 不同的二叉搜索树：卡特兰数，分治。
- 241 为表达式设置优先级：卡特兰数。 

## 0x40  数据结构进阶

### 0x41 并查集

- 547 省份数量/朋友圈:并查集
- 684 Redundant Connection: 并差集变形，1->n 下标从1开始。

### 0x42 树状数组

- 307 区域和检索-数组可修改（树状数组模板题）
- 315 计算右侧小于当前元素的个数
- 327 区间和的个数
- 493 翻转对


## 0x50 动态规划

### 线性DP(经典dp)
   
- 10 正则表达式匹配(Regular Expression Matching)：sp加' ',* 提前判断  
- 44 通配符匹配：
- 72 编辑距离(Edit Distance)   
- 139 单词拆分(Word Break)    
- 70 爬楼梯(Climbing Stairs)     
- 62 不同路径(Unique Paths)    
- 63 不同路径 II    
- 64 最小路径和     
- 53 最大子序和     
- 152 乘积最大子数组(Maximum Product Subarray)    
- 115 不同的子序列: 字符串的dp，求方案数要加在一起  
- 131 分割回文串：动态规划f + DFS回溯
- 132 分割回文串Ⅱ 

### 数字三角形模型

- 118 杨辉三角
- 119 杨辉三角Ⅱ
- 120 Triangle：滚动数组优化版 + 普通二维数组版本
- 221 最大正方形：理解推导 f[i][j] = min(f[i - 1][j - 1], min(f[i - 1][j], f[i][j - 1])) + 1
- 376 出界的路径数: 三维DP: f[i][j][k]:移动k次到达i和j的路径总数


### 子序列(LIS模型)

- 128 最长连续序列(Longest Consecutive Sequence）:set中心展开法   
- 300 最长递增子序列LIS：双循环dp O(nxn) 或者贪心+二分查找（替换）O(nlog(n))
- 1143 LCS 最长公共子序列(Longest Common Subsequencee):二维dp 
- 354 俄罗斯套娃信封问题: 二维子序列问题，先排序，再调用LIS模型(双循环dp或者贪心+二分查找（替换）)
- 132 分割回文串 II：回文字符串dp(' ' +方法) + LIS dp，注意枚举的顺序，使用的状态必须是已经计算出来的。

### 状态机问题(股票问题、打家劫舍问题)

动归不一定只有f数组的，dp的递归型也是动归。

- 121 买卖股票的最佳时机:贪心/状态机
- 122 买卖股票的最佳时机 II:贪心/状态机
- 123 买卖股票的最佳时机 III
- 188 买卖股票的最佳时机 IV：状态机
- 309 最佳买卖股票时机含冷冻期：状态机
- 714 买卖股票的最佳时机含手续费：状态机

先买后卖：贪心法、dp状态机法(carl一维数组-->buy(0) + sell(1)简化，carl二维数组（K次交易，冷冻期）--> buy + sell数组简化)
买入股票的状态不代表当天一定买入股票，是一种可能的状态。   
LeetCode买卖股票问题——模版总结(简化dp)：https://wwwacwingcom/blog/content/526/   
代码随想录股票问题(二维dp)：https://programmercarlcom   

- 213 打家劫舍 II: 1 选头不选尾 2 选尾不选头 3 头尾都不选(包含在1、2中), 双向dp (left->right, right->left)。
- 264 Ugly Number II: 状态机模型,三指针动态规划。
- 152 乘积最大子数组:min max
- 576 出界的路径数：三维DP

### 背包问题

**0-1背包**

- 416 分割等和子集
- 494 目标和  

**完全背包**

- 322 零钱兑换 I: 完全背包的二维和一维解法。
- 518 零钱兑换 II: 完全背包的二维和一维解法。
- 279 完全平方数：完全背包。
  
- 474 一和零(二维费用背包)
- 1449 数位成本和为目标值的最大数字(求具体方案)

### 区间dp

len -> 左端点 -> 决策

- 312 戳气球：左右各加1，区间dp。
- 486 预测赢家。f[i,j] 表示当前玩家与另一个玩家分数之差的最大值，可能是先手，也可能是后手,
注意转移方程f[l][r] = max(nums[l] - f[l + 1][r], nums[r] - f[l][r - 1]),f[l + 1][r]或者f[l][r - 1]是另外一个玩家的。结果返回是f[0,n-1]表示先手的赢的情况。
- 516 最长回文子序列
- 664 Strange Printer:s[l-1]=s[k-1],f[l][r] = min(f[l][r], f[l][k - 1] + f[k + 1][r]); 
- 1000 合并石头的最低成本: 石子合并变形。


### 状态压缩dp

- 464 我能赢吗: 状态压缩dfs + 记忆化搜索。
- 526 优美的排列：状态压缩dp,f[i]代表选法是i的时候的方案数，i的每一位代表是否选择当前的数

## 0x60 图论

### 图遍历--拓扑排序
    
- 207 课程表：拓扑排序(模板)，比较点数和入度为0的点数量。
- 210 课程表2：拓扑排序，保存序列。   
- 621 任务调度器

### 最短路

- 399 除法求值：Floyd求最短路，unordered_map<string, unordered_map<string, double>> edges 存边，unordered_set<string> vers存顶点。    
- 743 网络延迟时间：Floyd  + 朴素 Dijkstra  + 堆优化 Dijkstra  + Bellman Ford（类 & 邻接表） +  SPFA（邻接表）模板题。
- 785 判断二分图:DFS。
- 787 K 站中转内最便宜的航班:Bellman Ford/SPFA 都是基于动态规划,解有边数限制的最短路问题。
- 797 所有可能的路径:DFS。
- 332 重新安排行程:欧拉通路。



