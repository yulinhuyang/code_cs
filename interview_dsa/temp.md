
##### 238. 除自身以外数组的乘积

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int> &nums) {
        vector<int> answer(nums.size(), 0);
        answer[0] = 1;
        for (int i = 1; i < nums.size(); i++) {
            answer[i] = answer[i - 1] * nums[i - 1];
        }
        //左右遍历合并成一
        int Rmul = 1;
        for (int i = nums.size() - 1; i > -1; i--) {
            answer[i] *= Rmul;
            Rmul *= nums[i];
        }

        return answer;
    }
};

```
##### 239. 滑动窗口最大值

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int m = nums.size();

        //存索引，方便出的判断
        deque<int> deque;
        vector<int> res;
        //处理前K个,保持升序
        for (int i = 0; i < k; i++) {
            while (!deque.empty() && nums[i] >= nums[deque.back()]) {
                deque.pop_back();
            }
            deque.emplace_back(i);
        }
        res.emplace_back(nums[deque.front()]);

        for (int i = k; i < m; i++) {
            //先出
            if (deque.front() == i - k) {
                deque.pop_front();
            }
            //后入并判断
            while (!deque.empty() &&  nums[i] >= nums[deque.back()]) {
                deque.pop_back();
            }

            deque.emplace_back(i);
            res.emplace_back(nums[deque.front()]);
        }

        return  res;
    }
};

```


性能对比：

deque: 280 ms	26.9 MB	

list：8164 ms	27.5 MB

```python

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
	#性能比list更好,存索引
        deque = collections.deque()
        res = []
        for i in range(k):
            #单调降序队列
            while deque and nums[deque[-1]] < nums[i]:
                deque.pop()
            deque.append(i)
        
        res.append(nums[deque[0]])
        for i in range(k,len(nums)):
            if deque[0] == i - k:
                deque.popleft()
            while deque and nums[deque[-1]] < nums[i]:
                deque.pop()
                
            deque.append(i)
            res.append(nums[deque[0]])

        return res

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

        #存索引
        deque = []
        res = []
        for i in range(0,k):
            #单调降序队列
            while deque and nums[deque[-1]] < nums[i]:
                deque.pop(-1)
            deque.append(i)
        res.append(nums[deque[0]])

        for i in range(k,len(nums)):
            if deque and deque[0] == i - k:
                deque.pop(0)
            while deque and nums[deque[-1]] < nums[i]:
                deque.pop(-1)

            deque.append(i)
            res.append(nums[deque[0]])
        
        return res
```

##### 240. 搜索二维矩阵 II

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();
        int i = 0;
        int j = n - 1;
        while(i < m && j > -1){
            if(matrix[i][j] < target){
                i++;
                //二分查找的区间写法
            } else if(target < matrix[i][j]){
                j--;
            } else{
                return true;
            }
        }
        return false;
    }
};

```


背包问题，正常情况下 choice(coins)在外循环，amount在内循环，根据choice是否有限进行求最值、能否、方法数等，

部分特殊情况，如完全平方数等，choice不使用sqrt可以简化放在内循环。

0-1背包模板(最值)，部分可以简化为一维的

```C++
for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
return dp[N][W]
```

子集背包模板(能否)：

```C++
 #装入或者不装入
 dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]]
```

完全背包模板(方法数)

https://labuladong.gitee.io/algo/3/25/81/

```C++
for i in range(n + 1):
    for j in range(amount + 1):
        if j - coins[i-1] >= 0：
            dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]] #这里表示i可以反复使用
        else:
            dp[i][j] = dp[i - 1][j]
```

##### 279. 完全平方数

相当于完全背包的结合最值问题

```C++

正常情况下coins外循环，amount内循环，这里反过来了。

class Solution {
public:
    int numSquares(int n) {
        int num = sqrt(n);
        vector<int> squareNum(num + 1);
        for (int i = 0; i < num + 1; i++) {
            squareNum[i] = i * i;
        }

        vector<int> dp(n + 1, n);
        dp[0] = 0;
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < num + 1; j++) {
                if (i < squareNum[j]) {
                    break;
                }
                dp[i] = min(dp[i], dp[i - squareNum[j]] + 1);
            }
        }
        return dp[n];
    }
};


简化sqrt数组存储后，coins在内循环

class Solution {
public:
    int numSquares(int n) {
        //square数组 j -->j *j
        vector<int> dp(n + 1,n);//初始化无穷大
        dp[0] = 0;
        for (int i = 1; i < n + 1; i++) {
            for(int j = 1;j *j <= i;j++){
                if(i < j *j){
                    break;
                }
                //完全背包，dp[i]反复
                dp[i] = min(dp[i],dp[i-j*j] + 1);
            }
        }
        return  dp[n];
    }
};
```

##### 238 移动零

```C++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();

        int left =  0;
        int right = 0;
        while(right < n){
            if(nums[right] != 0){
                swap(nums[left],nums[right]);
                left++;
            }
            right++;
        }
    }
};

```

##### 287 寻找重复数

基于值的二分查找，nlogn复杂度

```C++
class Solution {
public:
    int findDuplicate(vector<int> &nums) {
        int n = nums.size();
        int left = 1;
        int right = n - 1;
        int ans;
        //基于值的二分查找
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int count = 0;
            for (int i = 0; i < n; i++) {
                if (nums[i] <= mid) {
                    count++;
                }
            }
            if (count <= mid) {
                left = mid + 1;
            } else {
                right = mid - 1;
                ans = mid;
            }
        }

        return ans;
    }
};

```

##### 300. 最长递增子序列

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n,1);
        int max_len = 1;
        for (int i = 1; i < n; i++) {
            //双循环仿insertSort
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = max(dp[i],dp[j] + 1);
                }
            }
            max_len = max(max_len,dp[i]);
        }
        return max_len;

    }
};
```

##### 309. 最佳买卖股票时机含冷冻期

```C++
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int dp_i_0 = 0;
        int dp_i_1 = INT_MIN;
        int pre = 0;
        //间隔+冷冻
        for (int i = 0; i < prices.size(); i++) {
            int temp = dp_i_0;
            dp_i_0 = max(dp_i_0, dp_i_1 + prices[i]);
            dp_i_1 = max(dp_i_1, pre - prices[i]);
            pre = temp;
        }

        return dp_i_0;
    }
};

```

链表标准形式

括号与栈、栈混洗

#####  301. 删除无效的括号

理解DSAPP栈与括号部分

单种括号计数可以，多种需要使用栈

去重复,判断前后重复，要从start开始

多叉递归回溯 + 剪枝 远快于   <  双分支回溯

```python

class Solution:
    def __init__(self):
        self.res = []

    def isValid(self,s):
        cnt = 0
        #理解DSAPP栈与括号部分
        #单种括号计数可以，多种需要使用栈
        for c in  s:
            if c == '(':
                cnt += 1
            elif c == ')':
                cnt -= 1
            if cnt < 0:
                return False
        
        return True


    def dfs(self,s,start,left_remove,right_remove):
        if left_remove == 0 and right_remove == 0:
            if self.isValid(s):
                self.res.append(s)
            return

        #从start开始，避免)(f重复
        for i in range(start,len(s)):
            #去重复
            if i != start and s[i-1] == s[i]:
                continue
            if left_remove + right_remove > len(s) - i :
                return

            if left_remove > 0 and s[i] == '(':
                self.dfs(s[:i] + s[i+1:],i,left_remove - 1,right_remove)
            elif right_remove > 0 and s[i] == ')':
                self.dfs(s[:i] + s[i+1:],i,left_remove,right_remove -1)
    
        return


    def removeInvalidParentheses(self, s: str) -> List[str]:
        left_remove = 0
        right_remove = 0
        for i in range(len(s)):
            if s[i] == '(':
                left_remove += 1
            elif s[i] == ')':
                if left_remove != 0:
                    left_remove -= 1
                else:
                    right_remove += 1
        
        self.dfs(s,0,left_remove,right_remove)

        return self.res
```

```C++
class Solution {
    vector<string> res;
public:
    bool isValid(string s) {
        int cnt = 0;
        //理解DSAPP栈与括号部分
        //单种括号计数可以，多种需要使用栈
        for (auto c:s) {
            if (c == '(') {
                cnt++;
            } else if (c == ')') {
                cnt--;
            }
            if (cnt < 0) {
                return false;
            }
        }
        return true;
    }

    void dfs(string s, int start, int left_remove, int right_remove) {
        if (left_remove == 0 && right_remove == 0) {
            if (isValid(s)) {
                res.emplace_back(s);
            }
            return;
        }
        //从start开始，避免)(f重复
        for (int i = start; i < s.size(); i++) {
            //去重复
            if (i != start && s[i] == s[i - 1]) {
                continue;
            }

            //choice
            if (left_remove + right_remove > s.size() - i) {
                return;
            }

            if (left_remove > 0 && s[i] == '(') {
                dfs(s.substr(0, i) + s.substr(i + 1), i, left_remove - 1, right_remove);
            } else if (right_remove > 0 && s[i] == ')') {
                dfs(s.substr(0, i) + s.substr(i + 1), i, left_remove, right_remove - 1);
            }
        }

    }


    vector<string> removeInvalidParentheses(string s) {
        int left_remove = 0;
        int right_remove = 0;
        for (auto c:s) {
            if (c == '(') {
                left_remove++;
            } else if (c == ')') {
                if (left_remove == 0) {
                    right_remove++;
                } else {
                    left_remove--;
                }
            }
        }

        string path;
        dfs(s, 0, left_remove, right_remove);
        return res;
    }
};

```





