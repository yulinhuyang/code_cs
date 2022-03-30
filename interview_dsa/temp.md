
一般来说，时间复杂度比空间复杂度更重要。


X = X / 10 + X % 10

INT_MAX = 2^31-1 = 2147483647

INT_MIN = -2^31 =-2147483648


##### 7. 整数反转

```C++
class Solution {
public:
    int reverse(int x) {
        int res = 0;
        while (x) {
            //因为INT_MAX 最高为是2，x <= INT_MAX, 所以不用判等于
            if (res < INT_MIN / 10 || res > INT_MAX / 10) {
                return 0;
            }
            int digit = x % 10;
            x /= 10;
            res = res * 10 + digit;
        }
        return res;
    }
};

```

##### 8 字符串转换整数 (atoi)

```C++
class Solution {
public:
    int myAtoi(string s) {
        int k = 0;
        while (k < s.size() && s[k] == ' ') k++;
        int sign = 1;
        if (s[k] == '-') {
            sign = -1;
            k++;
        } else if (s[k] == '+') {
            k++;
        }
        int res = 0;
        while (k < s.size()) {
            if (s[k] < '0' || s[k] > '9') return res;
            int x = s[k] - '0';
            if (res > (INT_MAX - x) / 10 || (res == (INT_MAX - x)/10 && x > INT_MAX % 10)) {
                return INT_MAX;
            }
            if (res < (INT_MIN + x) / 10 || (res == (INT_MIN + x) / 10 && x < INT_MIN % 10)) {
                return INT_MIN;
            }
            res = res * 10 + sign * x;
            k++;
        }
        return res;
    }
};
```

AC 自动机解法

```C++

```

##### 9 回文数

中心扩展法--->翻转一半法

```C++
class Solution {
public:
    bool isPalindrome(int x) {
        //中心扩展法-->翻转一半法
        if (x < 0 || (x % 10 == 0 && x != 0)) return false;
        int rev_num = 0;
        while (rev_num < x) {
            rev_num = rev_num * 10 + x % 10;
            x /= 10;
        }
        return rev_num == x || rev_num / 10 == x;
    }
};
```

##### 13 罗马数字转整数

模拟
```C++
//hash 模拟法
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> hash = {
                {'I', 1},
                {'V', 5},
                {'X', 10},
                {'L', 50},
                {'C', 100},
                {'D', 500},
                {'M', 1000}
        };
        int res = 0;
        for (int i = 0; i < s.size(); i++) {
            if (i + 1 < s.size() && hash[s[i]] < hash[s[i + 1]]) {
                res -= hash[s[i]];
            } else {
                res += hash[s[i]];
            }
        }
        return res;
    }
};

//传统模拟法
class Solution {
public:
    int romanToInt(string s) {
        int nums[] = {1000,900,500,400,100,90,50,40,10,9,5,4,1};
        string reps[] = {"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
        int i = 0,j = 0;
        int res = 0;
        while(i < s.size() && j < 13){
            if(reps[j].size() == 2){
                while(i + 1 < s.size() && string(1,s[i]) + string(1,s[i+ 1]) == reps[j]){
                    res += nums[j];
                    i += 2;
                }
            }else{
                while(i < s.size() && string(1,s[i]) == reps[j]) {
                    res += nums[j];
                    i++;
                }
            }
            j++;
        }
        return res;
    }
};

```

##### 14 最长公共前缀
```C++
class Solution {
public:
    string longestCommonPrefix(vector<string> &strs) {
        if (strs.size() == 0)
            return "";
        string prefix;
        for (int i = 0; i < strs[0].size(); i++) {
            char c = strs[0][i];
            for (auto &str:strs) {
                if (str[i] != c) {
                    return prefix;
                }
            }
            prefix += c;
        }
        return prefix;
    }
};

```
##### 15 三数之和

双指针模板总结：https://www.acwing.com/blog/content/426/

双指针的含义:  当需要枚举数组中的两个元素时，如果我们发现随着一个元素的递增，第二个元素是递减的，那可以使用双指针方法，将枚举的时间复杂度从o(n x n)优化到O(n),因为每次左指针右移一个位置，右指针会向左移动若干个位置，均摊下来，每次向左移动一个位置。

双指针法充分使用了数组有序这一特征，从而在某些情况下能够简化一些运算，将时间复杂度从log(n^2)降到log(n).

双指针移动回填法。

排序+ 双指针
```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int> &nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        int m = nums.size();
        for (int i = 0; i < m; i++) {
            //i跳过重复的
            if (i && nums[i] == nums[i - 1]) continue;
            //双指针，i和j在一层循环
            for (int j = i + 1, k = m - 1; j < k; j++) {
                //i和j间隔>=1，j和k间隔 >= 1
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;
                while (j < k - 1 && nums[i] + nums[j] + nums[k - 1] >= 0) k--;
                if (nums[i] + nums[j] + nums[k] == 0) {
                    res.emplace_back(vector<int>{nums[i], nums[j], nums[k]});
                }
            }
        }
        return res;
    }
};
```
##### 16 最接近的三数之和

排序+ 双指针, 最接近的两个和是相邻的

```C++
class Solution {
public:
    int threeSumClosest(vector<int> &nums, int target) {
        pair<int, int> res = {INT_MAX, INT_MAX};
        int m = nums.size();
        sort(nums.begin(), nums.end());
        for (int i = 0; i < m; i++) {
            for (int j = i + 1, k = m - 1; j < k; j++) {
                //单边缩进
                while (j < k - 1 && (nums[i] + nums[j] + nums[k - 1] >= target)) k--;
                int sum = nums[i] + nums[j] + nums[k];
                res = min(res, make_pair(abs(sum - target), sum));
                //最接近的和左右相邻
                if (k - 1 > j) {
                    sum = nums[i] + nums[j] + nums[k - 1];
                    res = min(res, make_pair(target - sum, sum));
                }
            }
        }
        return res.second;
    }
};

```

##### 18  四数之和

排序 +  双指针 + 跳重 + 单边缩 

```C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int> &nums, int target) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        int m = nums.size();
        for (int i = 0; i < m; i++) {
            if (i && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < m; j++) {
                if (j > i + 1 && (nums[j] == nums[j - 1])) continue;
                for (int k = j + 1, u = m - 1; k < u; k++) {
                    if (k > j + 1 && (nums[k] == nums[k - 1])) continue;
                    while (k < u - 1 && (0ll + nums[i] + nums[j] + nums[k] + nums[u - 1] >= target)) u--;
                    if (0ll + nums[i] + nums[j] + nums[k] + nums[u] == target) {
                        res.emplace_back(vector<int>{nums[i], nums[j], nums[k], nums[u]});
                    }
                }
            }
        }
        return res;
    }
};
```





