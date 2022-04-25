
#####  Offer II 008 和大于等于 target的最短子数组

```C++
//双指针 可变滑窗法
class Solution {
public:
    int minSubArrayLen(int target, vector<int> &nums) {
        int m = nums.size();

        int sum = nums[0];
        int ans = INT_MAX;
        for (int i = 0, j = 1; i < j; i++) {
            while (sum < target && j < m) {
                sum += nums[j++];
            }
            if(sum >= target) ans = min(ans, j - i);
            sum -= nums[i];
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


875   647  713  567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 

