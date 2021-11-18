
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

```C++
0-1背包模板(最值)，部分可以简化为一维的

for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
return dp[N][W]


子集背包模板(能否)：

 #装入或者不装入
 dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]]


完全背包模板(方法数)

https://labuladong.gitee.io/algo/3/25/81/

for i in range(n + 1):
    for j in range(amount + 1):
        if j - coins[i-1] >= 0：
            dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]] #这里表示i可以反复使用
        else:
            dp[i][j] = dp[i - 1][j]
```

##### 279. 完全平方数

```C++
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

