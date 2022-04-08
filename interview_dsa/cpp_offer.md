# 剑指offer + 专项突破版拓展

# 0x00 基本算法

## 0x01 位运算
- 15 二进制中 1 的个数 
- 56 数组中只出现一次的数字
- 39 数组中出现次数超过一半的数字 
- 62 圆圈中最后剩下的数 
- 43 从 1 到 n 整数中 1 出现的次数 
- 65 不用加减乘除做加法 
- Offer II 2 二进制加法(67 二进制求和)
- Offer II 3 前n 个数字二进制中1的个数(338 比特位计数)
- Offer II 4 只出现一次的数字(137 只出现一次的数字 II)

- Offer II 5 单词长度的最大乘积(318 最大单词长度乘积)
- Offer II 70 排序数组中只出现一次的数字(540 有序数组中的单一元素)

## 0x02 递归与模拟

- 17  打印从 1 到最大的 n 位数 
- 44  数字序列中的某一位数字 
- 61  扑克牌顺子 
- 64  求 1+2+3+...+n 
- Offer II 1 整数除法(29 两数相除)


## 0x03 前缀和与差分
- Offer II 10 和为 k 的子数组(560. 和为 K 的子数组)
- Offer II 11 0和1个数相同的子数组(525 连续数组)
- Offer II 12 左右两边子数组的和相等(724 寻找数组的中心下标)
- Offer II 13 二维子矩阵的和(304 二维区域和检索 - 矩阵不可变)

## 0x04  二分和三分
- 4 二维数组中的查找(240 搜索二维矩阵 II): 先去掉干扰条件相等的数,折线分析法。
- 11  旋转数组的最小数字 
- 53 数字在排序数组中出现的次数

- Offer II 68 查找插入位置(35 搜索插入位置)
- Offer II 69 山峰数组的顶部(852 山脉数组的峰顶索引)
- Offer II 73 狒狒吃香蕉(875 爱吃香蕉的珂珂)
- Offer II 72 求平方根(69 x 的平方根)

##### 4. 二维数组中的查找 

```C++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.size() == 0){
            return 0;
        }
        int m = matrix.size();
        int n = matrix[0].size();
        int i = 0,j = n - 1;
        while(i <= m - 1  && j >= 0){
            if(matrix[i][j] == target){
                return  true;
            }else if(matrix[i][j] < target){
                i++;
            }else{
                j--;
            }
        }
        return  false;
    }
};
```
##### 11 旋转数组的最小数字

```C++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int n = numbers.size() - 1;
        while(n > 0 && numbers[0] == numbers[n]) n--;
        if(numbers[0] <= numbers[n]) return numbers[0];
        int l = 0,r = n;
        while(l < r){
            int mid = l + r >> 1;
            if(numbers[mid] <= numbers[n]) r = mid;
            else l = mid + 1;
        }
        return numbers[l];
    }
};
```

## 0x05 双指针与排序  
- 57_1 和为 S 的两个数字
- 57_2 和为 S 的连续正数序列
- 58_1 翻转单词顺序列
- 58_2 左旋转字符串

### 双指针
- Offer II 6 排序数组中两个数字之和(167 两数之和 II - 输入有序数组)
- Offer II 7 数组中和为0的三个数(15 三数之和)
- Offer II 8 和大于等于target的最短子数组(209. 长度最小的子数组)
- Offer II 9 乘积小于 K 的子数组(009. 乘积小于 K 的子数组)

### 排序  
- 21 调整数组顺序使奇数位于偶数前面 
- 45 把数组排成最小的数 
- 51 数组中的逆序对
- Offer II 75 数组相对排序(1122 数组的相对排序)
- Offer II 76 数组中的第 k 大的数字(215 数组中的第K个最大元素) 

### 滑动窗口
- Offer II 14 字符串中的变位词(567 字符串的排列)
- Offer II 15 字符串中的所有变位词(438 找到字符串中所有字母异位词)
- Offer II 16 不含重复字符的最长子字符串(3 无重复字符的最长子串)
- Offer II 17 含有所有字符的最短字符串(76 最小覆盖子串)
- Offer II 57 值和下标之差都在给定的范围内(220 存在重复元素 III)

### 回文串
- Offer II 18 有效的回文(125 验证回文串)
- Offer II 19 最多删除一个字符得到回文(680 验证回文字符串 Ⅱ)
- Offer II 20 回文子字符串的个数(647 回文子串)


### Cycle Sort  
- 3 数组中重复的数字:圈排序
- 29 顺时针打印矩阵 
- 50 第一个只出现一次的字符位置 

- Offer II 071 按权重生成随机数(528 random-pick-with-weight)

##### Offer 03 数组中重复的数字
```C++
map/set计数法

class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        set<int> numSet;
        for(auto num:nums){
            if(numSet.count(num)){
                return num;
            } else{
                numSet.emplace(num);
            }
        }
        return 0;
    }
};

反复交换法：

class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int n = nums.size() - 1;
        for(int i = 0;i <= n;i++){
            if(nums[i] < 0 || nums[i] > n) return -1;
        }
        for(int i = 0;i <= n;i++){
            while(nums[i]!= i){
                if(nums[i] == nums[nums[i]])
                    return nums[i];
                swap(nums[i],nums[nums[i]]);
            }
        }
        return -1;
    }
};
```

## 0x06 贪心

- 14 剪绳子 
- 63 股票的最大利润 
- Offer II 74 合并区间(56 合并区间)

# 0x10 基本数据结构

## 0x11 栈/单调栈
- 9 用两个栈实现队列
- 30 包含 min 函数的栈
- 31 栈的压入、弹出序列
- Offer II 36 后缀表达式(150 逆波兰表达式求值)
- Offer II 37 小行星碰撞(735 行星碰撞)
- Offer II 38 每日温度(739 每日温度)
- Offer II 39 直方图最大矩形面积(84 柱状图中最大的矩形)
- Offer II 40 矩阵中最大的矩形(85 最大矩形)

##### 9. 用两个栈实现队列
```C++
class CQueue {
    stack<int> a;
    stack<int> b;
public:
    CQueue() {
    }

    void appendTail(int value) {
        a.emplace(value);
    }

    int deleteHead() {
        if(b.size() == 0){
            int m = a.size();
            for (int i = 0; i < m; i++) {
                b.emplace(a.top());
                a.pop();
            }
        }
        if(b.empty()){
            return -1;
        } else{
            auto res = b.top();
            b.pop();
            return res;
        }
    }
};
```

## 0x12 单调队列

- 59 滑动窗口的最大值
- Offer II 41 滑动窗口的平均值(346 plus)
- Offer II 42 最近请求次数(933 最近的请求次数)

## 0x13 链表与邻接表

- 6 从尾到头打印链表:递归法，也可以用栈缓存再弹出，递归的本质就是栈
- 18_1 在 O(1) 时间内删除链表节点 
- 18_2 删除链表中重复的结点 
- 22  链表中倒数第 K 个结点 
- 23  链表中环的入口结点 
- 24  反转链表 
- 25  合并两个排序的链表 
- 35  复杂链表的复制 
- 52  两个链表的第一个公共结点
- Offer II 21 删除链表的倒数第 n 个结点(19 删除链表的倒数第 N 个结点)
- Offer II 22 链表中环的入口节点(142 环形链表 II)
- Offer II 23 两个链表的第一个重合节点(160 相交链表)
- Offer II 24 反转链表(206 反转链表)
- Offer II 25 链表中的两数相加(445 两数相加 II)
- Offer II 26 重排链表(143 重排链表)
- Offer II 27 回文链表(234 回文链表)
- Offer II 28 展平多级双向链表(430 扁平化多级双向链表)
- Offer II 29 排序的循环链表(708 plus)
- Offer II 77 链表排序(148 排序链表)
- Offer II 78 合并排序链表(23 合并K个升序链表)
 

##### Offer 06 从尾到头打印链表
```C++
//递归法
class Solution {
    vector<int> res;
    void reverse(ListNode* head){
        if(head == nullptr){
            return;
        }
        reverse(head->next);
        res.emplace_back(head->val);
    }
public:
    vector<int> reversePrint(ListNode* head) {
        reverse(head);
        return res;
    }
};
```
## 0x14 hash
- Offer II 30 插入、删除和随机访问都是 O(1) 的容器(380 O(1) 时间插入、删除和获取随机元素)
- Offer II 32 有效的变位词(242 有效的字母异位词)
- Offer II 33 变位词组(49 字母异位词分组)
- Offer II 34 外星语言是否排序(953 验证外星语词典)
- Offer II 35 最小时间差(539 最小时间差)
- Offer II 31 最近最少使用缓存(146 LRU 缓存)
- Offer II 119 最长连续序列(128 最长连续序列)


## 0x15 字符串(KMP与最小表示法）   
- 5 替换空格 
- 20 表示数值的字符串 
- 46 把数字翻译成字符串 
- 67 把字符串转换成整数 

##### Offer 05 替换空格
```C++
class Solution {
public:
    string replaceSpace(string s) {
        string output;
        for(auto chr:s){
            if(chr == ' '){
                output += "%20";
            } else{
                output += string(1,chr);
            }
        }
        return output;
    }
};
```

## 0x16  Trie树（字典树）
- Offer II 62 实现前缀树(208 实现 Trie (前缀树))
- Offer II 63 替换单词(648 单词替换)
- Offer II 64 神奇的字典(676 实现一个魔法字典)
- Offer II 65 最短的单词编码(820 单词的压缩编码)
- Offer II 66 单词之和(677 键值映射)
- Offer II 67 最大的异或(421 数组中两个数的最大异或值)

## 0x17 二叉堆 
- 40 最小的 K 个数
- 41_1 数据流中的中位数(295 数据流的中位数)
- 41_2 字符流中第一个不重复的字符
- Offer II 59 数据流的第 K 大数值(703 数据流中的第 K 大元素)
- Offer II 60 出现频率最高的 k 个数字(347 前 K 个高频元素)
- Offer II 61 和最小的 k 个数对(373 查找和最小的 K 对数字)

## 0x21 树与图的遍历

### 树的遍历   
- 7 重建二叉树(105 从前序与中序遍历序列构造二叉树)
- 8 二叉树的下一个结点
- 26  树的子结构 
- 27  二叉树的镜像 
- 28  对称的二叉树 
- 32_1 从上往下打印二叉树 
- 32_2 把二叉树打印成多行 
- 32_3 按之字形顺序打印二叉树 
- 33  二叉搜索树的后序遍历序列 
- 34  二叉树中和为某一值的路径 
- 36  二叉搜索树与双向链表 
- 37  序列化二叉树 
- 54  二叉查找树的第 K 个结点 
- 55_2 平衡二叉树 
- 68  树中两个节点的最低公共祖先 
- Offer II 43 往完全二叉树添加节点(919 完全二叉树插入器)
- Offer II 44 二叉树每层的最大值(515 在每个树行中找最大值)
- Offer II 45 二叉树最底层最左边的值(513 找树左下角的值)
- Offer II 46 二叉树的右侧视图(199 二叉树的右视图)
- Offer II 47 二叉树剪枝(814 二叉树剪枝)
- Offer II 48 序列化与反序列化二叉树(297 二叉树的序列化与反序列化)
- Offer II 49 从根节点到叶节点的路径数字之和(129 求根节点到叶节点数字之和)
- Offer II 50 向下的路径节点之和(437 路径总和 III)
- Offer II 51 节点之和最大的路径(124 二叉树中的最大路径和)
- Offer II 52 展平二叉搜索树(897 递增顺序搜索树)
- Offer II 53 二叉搜索树中的中序后继(285 plus)
- Offer II 54 所有大于等于节点的值之和(538 把二叉搜索树转换为累加树)
- Offer II 55 二叉搜索树迭代器(173 二叉搜索树迭代器)
- Offer II 56 二叉搜索树中两个节点之和(653 两数之和 IV - 输入 BST)


## 0x22 DFS(递归、回溯)
- 12 矩阵中的路径: dfs四方向遍历，回溯做选择（修改涂色变量相当于push_back）
- 38 字符串的排列 

- Offer II 79 所有子集(78 子集)
- Offer II 80 含有 k 个元素的组合(77 组合)
- Offer II 81 允许重复选择元素的组合(39 组合总和)
- Offer II 82 含有重复元素集合的组合(40 组合总和 II)
- Offer II 83 没有重复元素集合的全排列(46 全排列)
- Offer II 84 含有重复元素集合的全排列(47 全排列 II)
##### offer 12 矩阵中的路径

回溯做选择（修改涂色变量相当于push_back），不是floodfill

```C++
class Solution {
public:
    int m, n;
    int dx[4] = {0, 0, -1, 1}, dy[4] = {-1, 1, 0, 0};

    bool exist(vector<vector<char>> &board, string word) {
        if (!board.size() || board[0].empty()) return false;
        m = board.size(), n = board[0].size();
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (dfs(board, i, j, word, 0))
                    return true;
        return false;
    }

    bool dfs(vector<vector<char>> &board, int i, int j, string word, int u) {
        if (board[i][j] != word[u]) return false;
        if (u == word.size() - 1) return true;

        board[i][j] = '.';
        for (int k = 0; k < 4; k++) {
            int newI = i + dx[k], newJ = j + dy[k];
            if (newI >= 0 && newI <= m - 1 && newJ >= 0 && newJ <= n - 1)
                if(dfs(board, newI, newJ, word, u + 1)) return true;
        }
        board[i][j] = word[u];
        return false;
    }
};
```

## 0x23 BFS

- 13 机器人的运动范围
- Offer II 107 矩阵中的距离(542 01 矩阵)
- Offer II 108 单词演变(127 单词接龙)
- Offer II 109 开密码锁(752 打开转盘锁)
- Offer II 110 所有路径(797 所有可能的路径)
- Offer II 111 计算除法(399 除法求值)
- Offer II 112 最长递增路径(329 矩阵中的最长递增路径)


##### Offer 13 机器人的运动范围
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

## 0x30 数学知识

- 14 不修改数组找出重复的数：抽屉原理
- 14-I 剪绳子
- Offer II 71 按权重生成随机数(528 按权重随机选择)


##### 14-I  剪绳子

```C++
class Solution {
public:
    int cuttingRope(int n) {
        if(n <= 3){
            return n - 1;
        }
        int a = n / 3;
        int b = n % 3;
        if(b == 0) return pow(3,a);
        if(b == 1) return pow(3,a - 1)*4;
        return pow(3,a)*2;

    }
};
```


## 0x41  数据结构进阶 - 并查集

- Offer II 116 省份数量(547 省份数量)
- Offer II 118 多余的边(684 冗余连接)
- Offer II 117 相似的字符串(839 相似字符串组)

## 0x50 动态规划
- 10_I 斐波那契数列 
- 10_II 青蛙跳台阶问题(70 爬楼梯)
- 19  正则表达式匹配 
- 42  连续子数组的最大和 
- 47  礼物的最大价值 
- 48  最长不含重复字符的子字符串 
- 49  丑数 
- 60  n 个骰子的点数 
- 66  构建乘积数组 
- Offer II 86 分割回文子字符串(131 分割回文串)
- Offer II 87 复原 IP(93 复原 IP 地址)
- Offer II 88 爬楼梯的最少成本(746 使用最小花费爬楼梯)
- Offer II 89 房屋偷盗(198 打家劫舍)
- Offer II 90 环形房屋偷盗(213 打家劫舍 II)
- Offer II 91 粉刷房子(256 plus)
- Offer II 92 翻转字符(926 将字符串翻转到单调递增)
- Offer II 93 最长斐波那契数列(873 最长的斐波那契子序列的长度)
- Offer II 94 最少回文分割(132 分割回文串 II)
- Offer II 95 最长公共子序列(1143 最长公共子序列)
- Offer II 96 字符串交织(97 交错字符串)
- Offer II 97 子序列的数目(115 不同的子序列)
- Offer II 98 路径的数目(62 不同路径)
- Offer II 99 最小路径之和(64 最小路径和)
- Offer II 100 三角形中最小路径之和(120 三角形最小路径和)
- Offer II 101 分割等和子集(416 分割等和子集)
- Offer II 102 加减的目标值(494 目标和)
- Offer II 103 最少的硬币数目(322 零钱兑换)
- Offer II 104 排列的数目(377 组合总和 Ⅳ)
- Offer II 105 岛屿的最大面积(695 岛屿的最大面积)

##### 10.1 斐波那契数列 
```C++
class Solution {
public:
    int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        long fb0 = 0;
        long fb1 = 1;
        for (int i = 2; i <= n; i++) {
            auto tmp = fb0 + fb1;
            fb0 = fb1;
            fb1 = tmp % 1000000007;
        }
        return fb1 % 1000000007;
    }
};
```

##  图论

- Offer II 58 日程表(729 我的日程安排表 I)
- Offer II 113 课程顺序(210 课程表 II)
- Offer II 114 外星文字典(269 plus)
- Offer II 115 重建序列(444 plus)
- Offer II 106 二分图(785 判断二分图)






