

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

桶排序： https://www.acwing.com/blog/content/8989/

##### C++ 运算符重载

**返回值 operator 重载符号  参数列表**

成员函数：Box operator+(const Box&);

非成员函数： Box operator+(const Box&, const Box&);

##### C++ 匿名函数

**捕获 参数列表 函数体**

[](int x, int y) { return x + y; } // 隐式返回类型

[](int& x) { ++x; }   //引用


参数列表：

[]        //未定义变量.试图在Lambda内使用任何外部变量都是错误的.

[x, &y]   //x 按值捕获, y 按引用捕获.

[&]       //用到的任何外部变量都隐式按引用捕获

[=]       //用到的任何外部变量都隐式按值捕获










