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

C++/C++11中头文件sstream介绍 : https://blog.csdn.net/fengbingchun/article/details/69788873

万能头文件： #include<bits/stdc++.h>

董晓算法： https://space.bilibili.com/517494241/


当前目录下查找某个名字的文件夹：    find  ./ -name "*8DFZ1"   

扫描线与自适应辛普森积分 求多边形面积并: https://blog.csdn.net/qq_45735851/article/details/115309408

**哈夫曼树构造**

哈夫曼树是带权路径长度最短的树，权值较大的结点离根较近

构造过程

(0) 补加一些额外的权值为0的叶子节点，使叶子节点的个数n满足（n-1)mod (k-1) = 0，n是节点数，k叉树。

(1) 建立一个小根堆，插入这n个叶子节点的权值；

(2) 从堆中取出最小的两个权权值是w1和w2,令ans += w1 + w2；

(3) 建立一个权值为w1+w2的树节点p，令p成为权值为w1和w2的树节点的父亲；

(4) 重复(2)、(3)步，直到森林中只剩一棵树为止，即堆的大小为1，该树即为所求得的哈夫曼树。变量ans就是wi * li的最小值。


任何一个整数模9同余与它的各数位上的数字之和


**两个区间的关系**

两个区间的关系：https://zhuanlan.zhihu.com/p/301507910

远离、重叠、相交

iota(p.begin(), p.end(), 1); // 将一个区间内的元素赋值为连续的递增值


** 空间换时间**

2352. 相等行列对： hash代替循环，空间换取时间

**  二分转判定**

2517. 礼盒的最大甜蜜度：二分转判定的时候，注意 l和r如何取。



