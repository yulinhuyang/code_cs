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

 567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
