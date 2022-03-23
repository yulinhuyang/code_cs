

提高班：

LRU cache: https://www.acwing.com/activity/content/code/content/405014/


934 最短的桥:先深度搜索，确度一座岛的边界,再广度搜索，查找路径

306  累加数： 字符的分割  


差分：        

1094   标记变化量 -->  求和最大值

1109  航班预订统计
   

并查集：
	
737 句子相似性：

1135  最低成本联通所有城市

	

##### 739. 每日温度
```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> stk;
        int n = temperatures.size();
        vector<int> res(n,0);
        for(int i = n - 1;i >= 0;i--){
            while(stk.size() && temperatures[stk.top()] <= temperatures[i]) stk.pop();
            if(stk.size()) res[i] = stk.top() - i;
            stk.push(i);
        }
        return res;
    }
};
```


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

##### 303. 区域和检索 - 数组不可变


```C++
class NumArray {
    vector<int> sum;
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        sum = vector<int> (n + 1,0);
        for(int i = 1;i < n + 1;i++){
            sum[i] = sum[i - 1] + nums[i - 1];
        }
    }

    int sumRange(int left, int right) {
        left++,right++;
        return sum[right] - sum[left-1];
    }
};
```

##### 304. 二维区域和检索 - 矩阵不可变

```C++
class NumMatrix {
    vector<vector<int>> sum;
public:
    NumMatrix(vector<vector<int>> &matrix) {
        int m = matrix.size(), n = matrix[0].size();
        sum = vector<vector<int>>(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + matrix[i - 1][j - 1];
            }
        }
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        row1++, col1++, row2++, col2++;
        int res = sum[row2][col2] - sum[row1 - 1][col2] - sum[row2][col1 - 1] + sum[row1 - 1][col1 - 1];
        return res;
    }
};
```


