### 1 位运算 数学

##### offer 14- I. 剪绳子

```c++
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
 
### 2 二分与双指针

##### Offer 04. 二维数组中的查找

```c++
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
##### offer 11 旋转数组的最小数字

先去掉干扰条件

折线分析法

```c++
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
### 3 贪心


### 4 栈与队列 单调栈与单调队列

##### offer 09 用两个栈实现队列

```cpp
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
 

### 5 堆&并查集

基本数据结构

##### Offer 03 数组中重复的数字

```c++
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


### 6 字符串与hash表

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


### 7 链表 

##### Offer 06  从尾到头打印链表

递归法 -->也可以用栈缓存再弹出，递归的本质就是栈

```c++
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

### 8 树与图

### 9 DFS（回溯）+ BFS

##### offer 12 矩阵中的路径

回溯做选择（修改涂色变量相当于push_back）

不是floodfill

```c++
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

### 10 动态规划 

##### Offer 10- I. 斐波那契数列

```cpp
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


