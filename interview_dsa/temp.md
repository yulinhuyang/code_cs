

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

这里A数组都是1开始的，时间从0开始的，因此需要减去1。




