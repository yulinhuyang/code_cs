
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

log前缀和 + 二分

```C++
class Solution {
    int binaryRight(vector<double> &nums, double target, int start) {
        int l = start, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if ((nums[mid] - target) <= 0) l = mid;
            else r = mid - 1;
        }
        return l;
    }

public:
    int numSubarrayProductLessThanK(vector<int> &nums, int k) {
        int m = nums.size();
        vector<double> sum(m + 1, 0);
        double target = log(k);
        int res = 0;
        for (int i = 1; i < m + 1; i++) {
            sum[i] = sum[i - 1] + log(nums[i - 1]);
        }
        for (int i = 0; i < m + 1; i++) {
            double s = sum[i] + target;
            int right = binaryRight(sum, s - 1e-9, i);
            res += right - i;
        }
        return res;
    }
};
```

```C++
//双指针模板
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

##### 567. 字符串的排列

固定len滑窗：vector<int> window(26,0)
    
```
class Solution {
public:
    bool checkInclusion(string p, string s) {
        if (s.size() < p.size()) return false;
        vector<int> pcnt(26, 0);
        vector<int> scnt(26, 0);
        int m = s.size(), len = p.size();
        for (int i = 0; i < len; i++) {
            pcnt[p[i] - 'a']++;
            scnt[s[i] - 'a']++;
        }

        if (pcnt == scnt) return true;
        for (int i = 0; i + len < m; i++) {
            scnt[s[i] - 'a']--;
            scnt[s[i + len] - 'a']++;
            if (scnt == pcnt) {
                return true;
            }
        }
        return false;
    }
};
```
    
滑窗法总结：
    
1 双指针不固定长度滑窗 + 双hash(单hash)：      

单hash(补欠法)：https://www.acwing.com/solution/LeetCode/content/160/          
hash[c]表示的是当前c这个字母还缺多少个，hash[s[j]] == 0 表示 s[j] 这个字母已经足够了     

双hash(计有效字符法)：https://www.acwing.com/solution/content/63190/     

2 固定len滑窗 + vector<int> (26,0):       



875   647   567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 

