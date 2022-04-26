
#####  Offer II 008 和大于等于 target的最短子数组

滑窗和/积 满足某种条件问题的两类解法：双指针 可变滑窗：先扩j后缩i，外扩j内缩i ； 前缀和+二分(后找)      

```C++
//双指针 可变滑窗法
class Solution {
public:
    int minSubArrayLen(int target, vector<int> &nums) {
        if (target < 1) return 0;
        int m = nums.size();
        int sum = 0, ans = INT_MAX;
        for (int i = 0, j = 0; j < m; j++) {
            sum += nums[j];
            while (sum >= target && i <= j) {
                sum -= nums[i];
                ans = min(ans, j - i + 1);
                i++;
            }
        }
        return ans == INT_MAX ? 0 : ans;
    }
};
```


```C++
//前缀和+二分
class Solution {
    int binaryLeft(int target, vector<int> &nums, int l, int r) {
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return l;
    }

public:
    int minSubArrayLen(int target, vector<int> &nums) {
        int m = nums.size();
        vector<int> sum(m + 1, 0);
        for (int i = 1; i < m + 1; i++) {
            sum[i] = sum[i - 1] + nums[i - 1];
        }
        int ans = INT_MAX;
        for (int i = 1; i < m + 1; i++) {
            int tmp = target + sum[i - 1];
            int j = binaryLeft(tmp, sum, i, m);
            if (sum[j] >= tmp) ans = min(ans, j - i + 1);
        }

        return ans == INT_MAX ? 0 : ans;
    }
};
```
##### LeetCode 713  乘积小于K的子数组

```C++
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int> &nums, int k) {
        if (k < 1) return 0;
        int m = nums.size();
        int mul = 1, res = 0;
        for (int i = 0, j = 0; j < m; j++) {
            mul *= nums[j];
            while (mul >= k && i <= j) mul /= nums[i++];
            res += j - i + 1;
        }
        return res;
    }
};
```

##### leetcode  724. 寻找数组的中心下标

前缀和

```C++
class Solution {
public:
    int pivotIndex(vector<int> &nums) {
        int total = accumulate(nums.begin(), nums.end(), 0);
        int sum = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (2 * sum + nums[i] == total) {
                return i;
            }
            sum += nums[i];
        }
        return -1;
    }
};
```


875   647  713  567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 

