
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

