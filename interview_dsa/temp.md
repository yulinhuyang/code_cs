#### LeetCode 852 山脉数组的峰顶索引    

二段性、单调性

左侧递增，寻找右边界     
右侧递增，寻找左边界 

```C++
class Solution {
public:
    int peakIndexInMountainArray(vector<int> &arr) {
        int l = 0, r = arr.size() - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            //左侧递增，寻找右边界
            if (arr[mid - 1] < arr[mid]) l = mid;
            else r = mid - 1;
        }
        return l;
    }
};

class Solution {
public:
    int peakIndexInMountainArray(vector<int> &arr) {
        int l = 0, r = arr.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            //右侧递增，寻找左边界
            if (arr[mid] > arr[mid + 1]) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
```

##### Leetcode 540 有序数组中的单一元素

【宫水三叶】二段性分析运用题:

https://leetcode-cn.com/problems/single-element-in-a-sorted-array/solution/gong-shui-san-xie-er-duan-xing-fen-xi-yu-17nv/

二分（异或技巧）      

使用邻接表（链式向前星）存无向图时，直接访问「当前边 e」所对应的「反向边 e′」。这也是为什么在「链式向前星」中我们只需要使用「单链表」并设定 idx 从0 开始进行存图即可：能够满足遍历所有出边，同时如果有访问相应反向边的需求，只需要通过 e[i^1] 访问。

链式前向星：         
最通俗易懂的讲解：https://blog.csdn.net/sugarbliss/article/details/86495945      
https://www.bilibili.com/read/cv13449207     

```C++
class Solution {
public:
    int singleNonDuplicate(vector<int> &nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] != nums[mid ^ 1]) r = mid;
            else l = mid + 1;
        }
        return nums[l];
    }
};

```

##### Leetcode 875 爱吃香蕉的珂珂

向上取整：(a+b-1)/b, ceil(a/b)   
向下取整：a/b, floor(a/b)  
四舍五入：(a+b/2)/b,round(a/b)  

二分求解转判定

```C++
class Solution {
public:
    int minEatingSpeed(vector<int> &piles, int h) {
        int maxPile = 0;
        for (auto &pile:piles) {
            maxPile = max(maxPile, pile);
        }
        int l = 1, r = maxPile;
        while (l < r) {
            int mid = l + r >> 1;
            if (check(piles, h, mid)) r = mid;
            else l = mid + 1;
        }
        return l;
    }

    bool check(vector<int> &piles, int h, int speed) {
        int time = 0;
        for (auto &pile:piles) {
            time += (pile + speed - 1) / speed;
        }
        return time <= h;
    }
};

```

##### LeetCode 56 合并区间

区间合并模板题

```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>> &intervals) {
        vector<vector<int>> res;
        //起点升序
        sort(intervals.begin(),intervals.end());
        int l = intervals[0][0], r = intervals[0][1];
        for (int i = 1; i < intervals.size(); i++) {
            if (r < intervals[i][0]) {
                res.push_back({l, r});
                l = intervals[i][0];
                r = intervals[i][1];
            } else {
                r = max(r, intervals[i][1]);
            }
        }
        res.push_back({l, r});
        return res;
    }
};
```


##### Leetcode 1122 数组的相对排序

自定义排序，返回true是升序，false是降序,T升F降。

```C++
class Solution {
public:
    vector<int> relativeSortArray(vector<int> &arr1, vector<int> &arr2) {
        //自定义排序
        unordered_map<int, int> hash;
        for (int i = 0; i < arr2.size(); i++) {
            hash[arr2[i]] = i;
        }
        sort(arr1.begin(), arr1.end(), [&](int a, int b) {
            if (hash.count(a)) {
                return hash.count(b) ? hash[a] < hash[b] : true;
            } else {
                return hash.count(b) ? false : a < b;
            }
        });

        return arr1;
    }
};

```

计数排序

```C++
class Solution {
public:
    vector<int> relativeSortArray(vector<int> &arr1, vector<int> &arr2) {
        //CountingSort
        int upper = *max_element(arr1.begin(), arr1.end());
        vector<int> counting(upper + 1, 0);
        vector<int> res;
        for (auto &num:arr1) {
            counting[num]++;
        }
        for (auto &num:arr2) {
            for (int i = 0; i < counting[num]; i++) {
                res.emplace_back(num);
            }
            counting[num] = 0;
        }
        for (int i = 1; i < upper + 1; i++) {
            for (int j = 0; j < counting[i]; j++) {
                res.emplace_back(i);
            }
        }
        return res;
    }
};
```

##### Leetcode 215  数组中的第K个最大元素

快选模板

```C++
class Solution {
public:
    int findKthLargest(vector<int> &nums, int k) {
        return quick_select(nums, 0, nums.size() - 1, nums.size() - k + 1);
    }

    //选第k小的数
    int quick_select(vector<int> &nums, int l, int r, int k) {
        if (l >= r) return nums[l];
        int i = l - 1, j = r + 1, x = nums[i + j >> 1];
        while (i < j) {
            do { j--; } while (nums[j] > x);
            do { i++; } while (nums[i] < x);
            if (i < j) swap(nums[i], nums[j]);
        }

        if (j - l + 1 >= k) return quick_select(nums, l, j, k);
        else return quick_select(nums, j + 1, r, k - (j - l + 1));
    }
};
```

##### Leetcode 22 括号生成

```C++
class Solution {
    vector<string> ans;
public:
    vector<string> generateParenthesis(int n) {
        string path;
        dfs(path,0, 0, n);
        return ans;
    }

    void dfs(string path, int lc, int rc, int n) {
        //string 回溯传值不用pop_back()
        if (lc == n && rc == n) ans.emplace_back(path);
        if (lc < n) dfs(path + '(', lc + 1, rc, n);
        if (rc < lc && rc < n) dfs(path + ')', lc, rc + 1, n);
    }
};
```

##### Leetcode 746 使用最小花费爬楼梯

```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int> &cost) {
        int m = cost.size();
        //楼顶是下标n
        vector<int> f(m + 1, 0);
        f[0] = 0, f[1] = 0;
        for (int i = 2; i < m + 1; i++) {
            f[i] = min(f[i - 1] + cost[i - 1], f[i - 2] + cost[i - 2]);
        }
        return f[m];
    }
};
```


 567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
