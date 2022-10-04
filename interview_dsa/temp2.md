

2Q算法： https://blog.csdn.net/qq_40088655/article/details/124028273

FIFO + LRU


LRU: map + list, K + v,队头为最近最少不适用的，如果用了(命中)会被换到队尾去。

LFU: K -> V -> Frequency, 淘汰最不经常使用的页，当平局时，去除最近最少使用的键。

缓存淘汰算法（LFU、LRU、ARC、FIFO、2Q）分析: https://zhuanlan.zhihu.com/p/352910565



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

LSM树详解: https://zhuanlan.zhihu.com/p/181498475

SkipList的原理与实现： https://zhuanlan.zhihu.com/p/33674267


