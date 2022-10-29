在线PS:

https://ps.gaoding.com/#/

https://www.photopea.com/

挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1

https://www.bilibili.com/video/BV1KE411V79h


##### 1288 删除被覆盖区间

```cpp
class Solution {
public:
    int removeCoveredIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](vector<int> a, vector<int> b) {
            return a[0] < b[0] || a[0] == b[0] && a[1] > b[1];
        });

        int ans = intervals.size();
        int RMax = intervals[0][1];
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][1] <= RMax) {
                ans--;
            } else {
                RMax = max(RMax, intervals[i][1]);
            }
        }
        return ans;
    }
};
```

桶排序： https://www.acwing.com/blog/content/8989/


stringstream 的头文件是sstream  



