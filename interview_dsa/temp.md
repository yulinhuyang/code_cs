

提高班：

LRU cache: https://www.acwing.com/activity/content/code/content/405014/


934 最短的桥:先深度搜索，确度一座岛的边界,再广度搜索，查找路径

306  累加数： 字符的分割  


差分：        

1094   标记变化量 -->  求和最大值

   

一维前缀和   
S[i] = a[1] + a[2] + ... a[i]   
a[l] + ... + a[r] = S[r] - S[l - 1]   

二维前缀和   
S[i, j] = 第i行j列格子左上部分所有元素的和   
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：   

S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]   

一维差分   
给区间[l, r]中的每个数加上c：    

B[l] += c, B[r + 1] -= c  

二维差分   
给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：   

S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c   


这里A数组都是1开始的，时间从0开始的，因此需要减去1。

树状数组学习笔记：https://www.acwing.com/blog/content/80/

树状数组的下标从 1 开始计数


#### 1109. 航班预订统计

差分模板题，数组索引从1开始的，要转换。

```C++
class Solution {
public:
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
        vector<int> nums(n,0);
        for(auto & book:bookings){
            int l = book[0] - 1,r = book[1] - 1;
            int c = book[2];
            nums[l] += c;
            if(r + 1 < n) nums[r + 1] -= c;
        }

        for(int i = 1;i < n;i++){
            nums[i] += nums[i - 1];
        }
        return nums;
    }
};
```
