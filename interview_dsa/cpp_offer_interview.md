# 剑指offer + 专项突破版拓展

# 0x00 基本算法

### 0x01 位运算

- 15. 二进制中 1 的个数 
- 56. 数组中只出现一次的数字
- 39. 数组中出现次数超过一半的数字 
- 62. 圆圈中最后剩下的数 
- 43. 从 1 到 n 整数中 1 出现的次数 
- 65. 不用加减乘除做加法 

- Offer II 005. 单词长度的最大乘积(318 maximum-product-of-word-lengths)

## 0x04  二分和三分

- 4. 二维数组中的查找: 先去掉干扰条件相等的数,折线分析法。
- 11. 旋转数组的最小数字 
- 53. 数字在排序数组中出现的次数 
- Offer II 073. 狒狒吃香蕉(875 koko-eating-bananas)

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
##### 11. 旋转数组的最小数字

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

- 57.1 和为 S 的两个数字
- 57.2 和为 S 的连续正数序列
- 58.1 翻转单词顺序列
- 58.2 左旋转字符串

- Offer II 007. 数组中和为 0 的三个数(15 3sum)
- Offer II 018. 有效的回文(125 valid-palindrome)
- Offer II 020. 回文子字符串的个数(647 palindromic-substrings)

### 排序

- 21 调整数组顺序使奇数位于偶数前面 
- 45 把数组排成最小的数 
- 51 数组中的逆序对 


### 滑动窗口

- Offer II 009. 乘积小于 K 的子数组(713 subarray-product-less-than-k)
- Offer II 014. 字符串中的变位词(567 permutation-in-string)
- Offer II 015. 字符串中的所有变位词(438 find-all-anagrams-in-a-string)
- Offer II 016. 不含重复字符的最长子字符串(3 longest-substring-without-repeating-characters)
- Offer II 017. 含有所有字符的最短字符串(76 minimum-window-substring)

### Cycle Sort

- 3. 数组中重复的数字:圈排序
- 29. 顺时针打印矩阵 
- 50. 第一个只出现一次的字符位置 

- Offer II 071. 按权重生成随机数(528 random-pick-with-weight)

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

- 14. 剪绳子 
- 63. 股票的最大利润 


# 0x10 基本数据结构

## 0x11 栈/单调栈

- 9. 用两个栈实现队列
- 30. 包含 min 函数的栈
- 31. 栈的压入、弹出序列

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

- 59. 滑动窗口的最大值

## 0x13 链表与邻接表

- 6. 从尾到头打印链表:递归法，也可以用栈缓存再弹出，递归的本质就是栈
- 18.1 在 O(1) 时间内删除链表节点 
- 18.2 删除链表中重复的结点 
- 22. 链表中倒数第 K 个结点 
- 23. 链表中环的入口结点 
- 24. 反转链表 
- 25. 合并两个排序的链表 
- 35. 复杂链表的复制 
- 52. 两个链表的第一个公共结点 

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

## 0x15 字符串(KMP与最小表示法）

- 5. 替换空格 
- 20. 表示数值的字符串 
- 46. 把数字翻译成字符串 
- 67. 把字符串转换成整数 

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

- Offer II 063. 替换单词(648 replace-words)   
- Offer II 064. 神奇的字典(676 implement-magic-dictionary)   
- Offer II 065. 最短的单词编码(820 short-encoding-of-words)   
- Offer II 066. 单词之和(677 map-sum-pairs)

## 0x17 二叉堆

- 40. 最小的 K 个数
- 41.1 数据流中的中位数
- 41.2 字符流中第一个不重复的字符

## 0x21 树与图的遍历

### 树的遍历

- 7. 重建二叉树 
- 8. 二叉树的下一个结点
- 26. 树的子结构 
- 27. 二叉树的镜像 
- 28. 对称的二叉树 
- 32.1 从上往下打印二叉树 
- 32.2 把二叉树打印成多行 
- 32.3 按之字形顺序打印二叉树 
- 33. 二叉搜索树的后序遍历序列 
- 34. 二叉树中和为某一值的路径 
- 36. 二叉搜索树与双向链表 
- 37. 序列化二叉树 
- 54. 二叉查找树的第 K 个结点 
- 55.2 平衡二叉树 
- 68. 树中两个节点的最低公共祖先 


### 图遍历--拓扑排序

- Offer II 113. 课程顺序(210 course-schedule-ii)
- Offer II 115. 重建序列(444 sequence-reconstruction)


## 0x22 DFS(递归、回溯)

- 12. 矩阵中的路径: dfs四方向遍历，回溯做选择（修改涂色变量相当于push_back）
- 38. 字符串的排列 

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

- 13. 机器人的运动范围

- Offer II 106. 二分图(785 is-graph-bipartite)  
- Offer II 107. 矩阵中的距离(542 01-matrix)  
- Offer II 109. 开密码锁(752 open-the-lock)  
- Offer II 116. 省份数量(547 number-of-provinces)  
- Offer II 114. 外星文字典(269 alien-dictionary)  
- Offer II 112. 最长递增路径(329 longest-increasing-path-in-a-matrix)


##### Offer 13. 机器人的运动范围
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

- 14-I. 剪绳子

##### 14-I. 剪绳子

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

- Offer II 116. 省份数量(547 number-of-provinces)  
- Offer II 118. 多余的边(684 redundant-connection)
- Offer II 117. 相似的字符串(839 similar-string-groups)

## 0x50 动态规划

- 10.1 斐波那契数列 
- 10.2 矩形覆盖 
- 10.3 跳台阶 
- 10.4 变态跳台阶 
- 19. 正则表达式匹配 
- 42. 连续子数组的最大和 
- 47. 礼物的最大价值 
- 48. 最长不含重复字符的子字符串 
- 49. 丑数 
- 60. n 个骰子的点数 
- 66. 构建乘积数组 
- Offer II 105. 岛屿的最大面积(695 max-area-of-island)

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

## 其它

17. 打印从 1 到最大的 n 位数 
44. 数字序列中的某一位数字 
61. 扑克牌顺子 
64. 求 1+2+3+...+n 



