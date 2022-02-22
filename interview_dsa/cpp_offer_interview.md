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
        int i = 0;
        while(i < nums.size()){
            if(nums[i] == i){
                i++;
                continue;
            }
            if(nums[nums[i]] == nums[i]){
                return nums[i];
            }
            swap(nums[nums[i]],nums[i]);
        }
        return 0;
    }
};
```

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

##### offer 11 旋转数组的最小数字

折线分析法

```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int len = numbers.size();
        int l = 0;
        int h = len - 1;
        while(l < h){
            int mid = l + h >> 1;
            if(numbers[mid] > numbers[h]){
                l = mid + 1;
            } else if(numbers[mid] == numbers[h]){
                h = h - 1;
            } else{
                h = mid; //只剩两个数，那么mid 一定会指向下标靠前的数字
            }
        }
        return numbers[l];
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
