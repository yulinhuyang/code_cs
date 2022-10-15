


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

##### C++ 运算符重载

**返回值 operator 重载符号  参数列表  + const(可选) + 函数体**

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


Sort后面的参数:

1. 函数指针，自己实现cmp函数
 
2. 类的仿函数，定义一个类，里面直接bool operator(){…}

3. lambda函数，[] (const int u,const int v){return u.w < v.w ;}








