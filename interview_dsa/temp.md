

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

	
前缀和 + 哈希表：

304    二维区域和检索--矩阵不可变

560    和为K的子数组

523    连续的子数组和

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
