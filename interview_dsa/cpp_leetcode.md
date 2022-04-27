# 0x00 基本算法

前缀和、二分、双指针(排序、滑窗)

## 0x01位运算

##### 136. 只出现一次的数字

```C++
class Solution {
public:
    int singleNumber(vector<int> &nums) {
        //要自己初始化，否则one的值随机的
        int one = 0;
        for(auto num:nums) {
            one ^= num;
        }
        return one;
    }
};
```

##### 137. 只出现一次的数字 II

```C++
class Solution {
public:
    int singleNumber(vector<int> &nums) {
        int ans = 0;
        for (int i = 0; i < 32; i++) {
            int sum = 0;
            for (auto &num: nums) {
                sum += (num >> i) & 1;
            }
            if (sum % 3) ans |= 1 << i;
        }
        return ans;
    }
};
```

##### LeetCode 231. 2的幂

```C++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && (n & -n) == n;
    }
};

class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }
};
```

##### LeetCode 201. 数字范围按位与

寻找公共前缀

```C++
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int shift = 0;
        while (m < n) {
            m >>= 1;
            n >>= 1;
            shift++;
        }
        return m << shift;
    }
};
```

##### 260. 只出现一次的数字 II

```C++
class Solution {
public:
    vector<int> singleNumber(vector<int> &nums) {
        int n = 0;
        for (auto &num:nums) {
            n ^= num;
        }

        //lowbit 运算
        int k = n == INT_MIN ? INT_MIN : n & (-n);
        int res1 = 0, res2 = 0;
        for (auto &num:nums) {
            if (num & k) {
                res1 ^= num;
            } else {
                res2 ^= num;
            }
        }
        return {res1, res2};
    }
};
```

##### LeetCode 338. 比特位计数

Brian Kernighan 算法: Brian Kernighan 算法的原理是：对于任意整数x，令 x=x & (x−1)该运算将 x 的二进制表示的最后一个 1 变成 0。   
位操作符的总结 Brian Kernighan算法: https://blog.csdn.net/weixin_43688483/article/details/106349982

```cpp
//BK算法
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> res;
        for (int i = 0; i <= n; i++) {
            int sum = 0,num = i;
            while (num) {
                num = num & (num - 1);
                sum += 1;
            }
            res.emplace_back(sum);
        }
        return res;
    }
};

//dp算法
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> f(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            f[i] = f[i >> 1] + (i & 1);
        }
        return f;
    }
};
```    

##### 461. 汉明距离
    
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int m = x^y;
        int cnt = 0;
        while(m){
            m = m &(m - 1);
            cnt++;
        }
        return cnt;
    }
}; 
```
##### 762. 二进制表示中质数个计算置位

```C++
class Solution {
public:
    int countPrimeSetBits(int left, int right) {
        vector<int> nums = {2, 3, 5, 7, 11, 13, 17, 19};
        unordered_set<int> primes;
        for (auto &num:nums) {
            primes.emplace(num);
        }
        int ans = 0;
        for (int i = left; i <= right; i++) {
            int sum = 0;
            for (int k = i; k;) {
                sum += k & 1;
                k >>= 1;
            }
            if (primes.count(sum)) ans++;
        }
        return ans;
    }
};
```

##### Leetcode 371 两整数之和

```
class Solution {
public:
    int getSum(int a, int b) {
        while (b) {
            auto carry = (unsigned int) (a & b) << 1;
            a = a ^b;
            b = carry;
        }
        return a;
    }
};
```

##### Leetcode 477 汉明距离总和

```C++
class Solution {
public:
    int totalHammingDistance(vector<int> &nums) {
        int ans = 0, n = nums.size();
        for (int i = 0; i < 32; i++) {
            int total = 0;
            for (auto &num:nums) {
                total += (num >> i) & 1;
            }
            ans += total * (n - total);
        }
        return ans;
    }
};
```
## 0x02 递推与递归、模拟

##### 7. 整数反转

一般来说，时间复杂度比空间复杂度更重要。

X = X / 10 + X % 10

INT_MAX = 2^31-1 = 2147483647

INT_MIN = -2^31 =-2147483648

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
##### 12. 整数转罗马数字

```C++
class Solution{
public:
    string intToRoman(int num)
    {
        int values[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        string reps[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        string ans;
        for (int i = 0; i < 13; i++)
        {
            while (num >= values[i])
            {
                num -= values[i];
                ans += reps[i];
            }
        }
        return ans;
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

##### Leetcode 43  字符串相乘

```C++
class Solution {
public:
    string multiply(string s1, string s2) {
        if(s1 == "0" || s2 == "0") return "0";
        int m = s1.size(), n = s2.size();
        vector<int> res(m + n, 0);
        vector<int> nums1, nums2;
        for (int i = m - 1; i >= 0; i--) {
            nums1.emplace_back(s1[i] - '0');
        }
        for (int i = n - 1; i >= 0; i--) {
            nums2.emplace_back(s2[i] - '0');
        }
        //模拟竖式
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                res[i + j] += nums1[i] * nums2[j];
            }
        }
        //处理进位
        for (int i = 0, t = 0; i < m + n; i++) {
            t += res[i];
            res[i] = t % 10;
            t /= 10;
        }
        int k = m + n - 1;
        while (k >= 0 && !res[k]) k--;

        string resNum = "";
        while (k >= 0) {
            resNum += res[k--] + '0';
        }
        return resNum;
    }
};

```
##### Leetcode 66 加一

高精度加法模板

```C++
class Solution {
public:
    vector<int> plusOne(vector<int> &digits) {
        int m = digits.size();
        vector<int> res;
        int carry = 1, num = 0;
        for (int i = m - 1; i >= 0; i--) {
            num = (digits[i] + carry) % 10;
            carry = (digits[i] + carry) / 10;
            res.emplace_back(num);
        }
        if (carry) res.emplace_back(1);
        reverse(res.begin(), res.end());
        return res;
    }
};

```

##### Leetcode 73 矩阵置零

```C++
class Solution {
public:
    void setZeroes(vector<vector<int>> &matrix) {
        //标记变量+自标记法
        int m = matrix.size(), n = matrix[0].size();
        int row0 = 0, col0 = 0;
        for (int i = 0; i < m; i++) {
            if (!matrix[i][0]) col0 = 1;
        }
        for (int j = 0; j < n; j++) {
            if (!matrix[0][j]) row0 = 1;
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (!matrix[i][j]) {
                    matrix[i][0] = 0, matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (!matrix[i][0] || !matrix[0][j]) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (row0) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
        if (col0) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }

    }
};

```

##### Leetcode 58 最后一个单词的长度

```C++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int n = s.size();
        int k = n - 1, res = 0;
        while (k >= 0 && s[k] == ' ') k--;
        while (k >= 0 && s[k] != ' ') {
            k--;
            res++;
        }
        return res;
    }
};

```

##### 67. 二进制求和

```C++
class Solution {
public:
    string addBinary(string a, string b) {
        reverse(a.begin(),a.end());
        reverse(b.begin(),b.end());
        int m = max(a.size(),b.size());
        int carry = 0;
        string ans;
        for(int i = 0;i < m;i++){
            carry += i < a.size()? a[i] - '0':0; 
            carry += i < b.size()? b[i] - '0':0;
            ans.push_back(carry % 2 ? '1':'0');
            carry /= 2;
        }
        if(carry) ans.push_back('1');
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```

#####  Leetcode 80 删除有序数组中的重复项 II

模拟：跳重范式：while(i + 1 < nums.size() && nums[i] == nums[i+1]) i++;

```C++
//class Solution {
public:
    int removeDuplicates(vector<int> &nums) {

        int j = 0;
        for (int i = 0; i < nums.size();) {
            if (i + 1 < nums.size() && nums[i] == nums[i + 1]) {
                nums[j++] = nums[i++];
                nums[j++] = nums[i++];
                while (i < nums.size() && nums[j - 1] == nums[i]) i++;
            } else {
                nums[j++] = nums[i++];
            }
        }
        return j;
    }
};

//极简
class Solution {
public:
    int removeDuplicates(vector<int> &nums) {
        int k = 0;
        for (auto &x:nums) {
            if (k < 2 || nums[k - 1] != x || nums[k - 2] != x) {
                nums[k++] = x;
            }
        }
        return k;
    }
};
```

##### Leetcode 89 格雷编码

```C++
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> res(1, 0);
        while (n--) {
            for (int i = res.size() - 1; i >= 0; i--) {
                res[i] <<= 1;
                res.emplace_back(res[i] + 1);
            }
        }
        return res;
    }
};
```
##### 299. 猜数字游戏

```C++
class Solution {
public:
    string getHint(string secret, string guess) {
        int a = 0,b = 0,n = secret.size();
        vector<int> ds(10,0), dg(10,0);
        for(int i = 0;i < n;i++){
            auto s = secret[i] - '0',g = guess[i] - '0';
            a += s == g?1:0;
            ds[s]++;
            dg[g]++;
        }
        for(int i = 0;i < 10;i++) b += min(ds[i],dg[i]);
        return to_string(a) + "A" + to_string(b - a) + "B";

    }
};
```

## 0x03 前缀和与差分

##### 437. 路径总和 III

dfs：返回值，全局变量

两个点：visited数组、pop撤销

TreeNode * t1 = new TreeNode(10);

```cpp
class Solution {
    unordered_map<long long, int> prefix;
    int path = 0;

    void dfs(TreeNode *root, long long curSum, int targetSum) {
        if (!root) {
            return;
        }
        curSum += root->val;
        if (prefix.find(curSum - targetSum) != prefix.end()) {
            path += prefix[curSum - targetSum];
        }
        prefix[curSum]++;
        dfs(root->left, curSum, targetSum);
        dfs(root->right, curSum, targetSum);
        prefix[curSum]--;
        return;
    }

public:
    int pathSum(TreeNode *root, int target) {
        prefix[0] = 1; //必须有
        dfs(root, 0, target);
        return path;
    }
};
```	
	

##### LeetCode 525 连续数组	 
```C++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int res = 0,cnt = 0;
        unordered_map<int, int> hash;
        hash[0] = -1;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i]) {
                cnt++;
            } else {
                cnt--;
            }
            if (hash.count(cnt)) {
                res = max(res, i - hash[cnt]);
            } else {
                hash[cnt] = i;
            }
        }
        return res;
    }
};
```

##### 560. 和为 K 的子数组

前缀和+ hash

```cpp
class Solution {
public:
    int subarraySum(vector<int> &nums, int k) {
        unordered_map<int,int> hash;
        hash[0] = 1;
        int res = 0,sum = 0;
        for(int i = 0;i < nums.size();i++){
            sum += nums[i];
            res += hash[sum - k];
            hash[sum]++;
        }
        return res;
    }
};

```

##### 303. 区域和检索 - 数组不可变


```C++
class NumArray {
    vector<int> sum;
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        sum = vector<int> (n + 1,0);
        for(int i = 1;i < n + 1;i++){
            sum[i] = sum[i - 1] + nums[i - 1];
        }
    }

    int sumRange(int left, int right) {
        left++,right++;
        return sum[right] - sum[left-1];
    }
};
```

##### 304. 二维区域和检索 - 矩阵不可变

```C++
class NumMatrix {
    vector<vector<int>> sum;
public:
    NumMatrix(vector<vector<int>> &matrix) {
        int m = matrix.size(), n = matrix[0].size();
        sum = vector<vector<int>>(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + matrix[i - 1][j - 1];
            }
        }
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        row1++, col1++, row2++, col2++;
        int res = sum[row2][col2] - sum[row1 - 1][col2] - sum[row2][col1 - 1] + sum[row1 - 1][col1 - 1];
        return res;
    }
};
```

##### 1094. 拼车

差分 标记变化量 -->  求和最大值
 
```C++
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        int n = trips.size();
        vector<int> nums(1002,0);
        for(auto & trip:trips){
            int l = trip[1],r = trip[2];
            int c = trip[0];
            nums[l] += c;
            nums[r] -= c;
        }

        int sum = 0;
        for(int i = 0;i < 1002;i++){
            sum += nums[i];
            if(sum > capacity) return false;
        }
        return true;
    }
};
```

#### 1109. 航班预订统计

差分模板题，数组索引从1开始的，要转换。

```C++
class Solution {
public:
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
        vector<int> nums(n,0);
        for(auto & book:bookings){
            int l = book[0] - 1,r = book[1] - 1;
            int c = book[2];
            nums[l] += c;
            if(r + 1 < n) nums[r + 1] -= c;
        }

        for(int i = 1;i < n;i++){
            nums[i] += nums[i - 1];
        }
        return nums;
    }
};
```



## 0x04  二分和三分

### 索引二分

##### 33  搜索旋转排序数组

先找最小值，去掉干扰条件
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0,r = nums.size() - 1;
        while(l < r){
            int mid = l + r >> 1;
            if(nums[mid] < nums.back()) r = mid;
            else l = mid + 1;
        }
        if(target <= nums.back()) r = nums.size() - 1;
        else l = 0,r--;
        while(l < r){
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if(nums[l] == target) return l;
        
        return -1;
    }
};
```
##### 34 在排序数组中查找元素的第一个和最后一个位置

二分查找左侧边界，收缩右边。

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
            if(nums.size() == 0) return {-1,-1};
            int l = 0,r = nums.size() - 1;
            while(l < r){
                int mid = l + r >> 1;
                if(nums[mid] >= target) r = mid;
                else l = mid + 1;
            }
            if(nums[l] != target) return {-1,-1};
            int res = l;
            l = 0,r = nums.size() - 1;
            while(l < r){
                int mid = l + r + 1 >> 1;
                if(nums[mid] <= target) l = mid;
                else r = mid -1;
            }
            return {res,l};
    }
};
```
##### 35. 搜索插入位置
```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        if(nums.empty() || target > nums.back()) return nums.size();

        int l = 0;
        int r = nums.size() - 1;
        while(l < r){
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return r;

    }
};
```

##### 74. 搜索二维矩阵
```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size(),m = matrix[0].size();
        int l = 0,r = m * n - 1;
        while(l < r){
            int mid = l + r >> 1;
            if(matrix[mid / m][mid % m] >= target)  r = mid;
            else l = mid + 1; 
        }
        return matrix[l / m][l % m] == target;

    }
};
```

##### Leetcode 81 搜索旋转排序数组 II

跳结尾重复 ————> 二分右边界搜索寻找旋转点————> 二分左边界搜索

二分查找：寻找满足check条件的左边界或者右边界。


```C++
class Solution {
public:
    bool search(vector<int> &nums, int target) {
        int m = nums.size() - 1, n = 0;
        //跳过重复
        while (m >= 0 && nums[m] == nums[0]) m--;
        //剪枝
        if (m < 0) return nums[0] == target;
        //寻找旋转点
        int l = 0, r = m;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            //满足条件的右边界
            if (nums[mid] >= nums[0]) l = mid;
            else r = mid - 1;
        }
        if (nums[l] == target) return true;
        if (target >= nums[0]) {
            r = l, l = 0;
        } else {
            r = m, l++;
        }
        //l和r可能相等
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return nums[r] == target;
    }
};

```

##### 153. 寻找旋转排序数组中的最小值
```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0;
        int r = nums.size() - 1;
        while(l < r){
            int mid = l + r >> 1;
            if(nums[mid] <= nums.back()) r = mid;
            else l = mid + 1;
        }
        return nums[l];
    }
};
```

#### 154  寻找旋转排序数组中的最小值 II

折线法二分寻找最小值

```C++
class Solution {
public:
    int findMin(vector<int> &nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r && nums[r] == nums[0]) r--;
        if (nums[0] < nums[r]) return nums[0];
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] < nums[0]) r = mid;
            else l = mid + 1;
        }
        return nums[l];
    }
};

```
##### 162. 寻找峰值
```c++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0,r = nums.size() -1;
        while(l < r){
            int mid = l + r >> 1;
            if(nums[mid] > nums[mid + 1]) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
```
##### 240. 搜索二维矩阵 II

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();
        int i = 0;
        int j = n - 1;
        while(i < m && j > -1){
            if(matrix[i][j] < target){
                i++;
                //二分查找的区间写法
            } else if(target < matrix[i][j]){
                j--;
            } else{
                return true;
            }
        }
        return false;
    }
};

```
##### 275. H 指数 II

```c++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        if(citations.empty() || !citations.back()) return 0;
        int n = citations.size();
        int l = 0,r = n - 1;
        while(l < r){
            int mid = l + r  >> 1;
            if(citations[mid] >= n - mid) r = mid;
            else l = mid + 1;
        }
        return n - l;
    }
};
```

##### 278. 第一个错误的版本

```c++
class Solution {
public:
    int firstBadVersion(int n) {
        int l = 0,r = n;
        while(l < r){
            int mid = l + (long long)r >> 1;
            if(isBadVersion(mid)) r = mid;
            else l = mid + 1;
        }
        return  l;
    }
};
```

##### leetcode 475. 供暖器

```C++
class Solution {
    bool check(int mid, vector<int> &houses, vector<int> &heaters) {
        for (int i = 0, j = 0; i < houses.size(); i++) {
            while (j < heaters.size() && abs(houses[i] - heaters[j]) > mid) {
                j++;
            }
            if (j >= heaters.size()) return false;
        }
        return true;
    }

public:
    int findRadius(vector<int> &houses, vector<int> &heaters) {
        sort(houses.begin(), houses.end());
        sort(heaters.begin(), heaters.end());
        int l = 0, r = INT_MAX;
        while (l < r) {
            int mid = (long long)l + r >> 1;
            if (check(mid, houses, heaters)) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
```

### 值域二分

##### 69. x 的平方根 
```c++
class Solution {
public:
    int mySqrt(int x) {
        int l = 0,r = x;
        while(l < r){
            int mid = l + (long long)r + 1 >> 1;
            if(mid <= (x/mid)) l = mid;
            else r = mid - 1;
       }
        return l;

    }
};
```
##### 287 寻找重复数

基于值的二分查找，nlogn复杂度

```C++
class Solution {
public:
    int findDuplicate(vector<int> &nums) {
        int n = nums.size();
        int left = 1;
        int right = n - 1;
        int ans;
        //基于值的二分查找
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int count = 0;
            for (int i = 0; i < n; i++) {
                if (nums[i] <= mid) {
                    count++;
                }
            }
            if (count <= mid) {
                left = mid + 1;
            } else {
                right = mid - 1;
                ans = mid;
            }
        }

        return ans;
    }
};

```

## 0x05 双指针与排序

左右指针（同向、反向）：排好序，找一些组合满足某种条件

快慢指针：有环的链表和数组问题，如判断链表是否是回文



### 同向（快慢）指针

##### 31 下一个排列

两遍逆向扫描：1 扫描找尽量靠右的不满足降序的数i，2 扫描找比i大的尽量靠右的数j,swap(num[i],num[j]), 3 reverse(nums.begin() + i + 1, nums.end())。

```C++
class Solution {
public:
    void nextPermutation(vector<int> &nums) {
        int m = nums.size();
        int i = m - 2;
        //找尽量靠右的不满足降序的数
        while (i >= 0 && nums[i] >= nums[i + 1]) i--;
        if (i < 0) {
            reverse(nums.begin(), nums.end());
            return;
        }
        int j = m - 1;
        //找尽量靠右的第一个比i大的数j
        while (j >= i && nums[i] >= nums[j]) j--;
        swap(nums[i], nums[j]);
        reverse(nums.begin() + i + 1, nums.end());
    }
};
```

##### 75 颜色分类

双指针：右侧循环判，左侧不超过

```c++

class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int p1 = 0;
        int p2 = n - 1;
        int index = 0;
        while(index <= p2){
            //循环右侧判
            while((index <= p2) &&(nums[index] == 2)){
                swap(nums[index],nums[p2]);
                p2--;
            }
            //p1不能跑到index前面
            if(nums[index] == 0){
                swap(nums[index],nums[p1]);
                p1++;
            }
            index ++;
        }
    }
};

```

##### 215. 数组中的第K个最大元素

j先走: i在大于基准数的地方停下，j在小于基准数的地方停下，如果i先走，最后停下跟基准数交换时，总是大于基准数的
	
```C++
class Solution {
public:
    int partition(vector<int> &nums, int low, int high) {
        //gen random index
        int index = (rand()%(high - low + 1))+1;
        swap(nums[low],nums[index]);

        int pivot = nums[low];
        int i = low;
        int j = high;
        while (i < j) {
            //i在大于基准数的地方停下，j在小于基准数的地方停下，如果i先走，最后停下跟基准数交换时，总是大于基准数的
            //j先走
            while (i < j && nums[j] >= pivot) {
                j--;
            }
            while (i < j && nums[i] <= pivot) {
                i++;
            }
            swap(nums[i], nums[j]);
        }
        swap(nums[low], nums[i]);
        return i;
    }

    void quickSort(vector<int> &nums, int start, int end) {
        if (start >= end) {
            return;
        }
        int index = partition(nums, start, end);
        quickSort(nums, start, index - 1);
        quickSort(nums, index + 1, end);
    }

    int findKthLargest(vector<int> &nums, int k) {

        quickSort(nums, 0, nums.size() - 1);
        return nums[nums.size() - k - 1];
    }
};
```	

##### 283 移动零

```C++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();

        int left =  0;
        int right = 0;
        while(right < n){
            if(nums[right] != 0){
                swap(nums[left],nums[right]);
                left++;
            }
            right++;
        }
    }
};

```	
### 反向（首尾）指针

##### 11. 盛最多水的容器

双指针

```C++

class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0;
        int r = height.size() - 1;
        int maxArea = 0;
        while(l < r){
            int area = min(height[l],height[r])*(r-l);
            maxArea = max(maxArea,area);
            if(height[l] < height[r]){
                l++;
            }else{
                r--;
            }
        }
        return maxArea;
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

#####  27 移除元素

```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int left = 0,right = nums.size() - 1;
        //双指针优化，避免多余交换
        while(left <= right)
            if(nums[left] == val) {
                nums[left] = nums[right--];
            }
            else left++;
        return left;
    }
};
```

##### 167. 两数之和 II - 输入有序数组
```c++
class Solution {
public:
    vector<int> twoSum(vector<int> &nums, int target) {
        for (int i = 0, j = nums.size() - 1; i <= j ; i++) {
            while (nums[i] + nums[j] > target) j--;
            if (nums[i] + nums[j] == target) return {i + 1, j + 1};
        }
        return {-1, -1};
    }
};
```
##### 75 颜色分类

三指针，类比三数之和
    
```C++
class Solution {
public:
    void sortColors(vector<int> &nums) {
        //j指向0开始，i指向1开始，k指向2开始
        for (int i = 0, j = 0, k = nums.size() - 1; i <= k;) {
            if (nums[i] == 0) swap(nums[i++], nums[j++]);
            else if (nums[i] == 2) swap(nums[i], nums[k--]);
            else i++;
        }
    }
};
 
```

##### 88. 合并两个有序数组
```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1,j = n - 1,k = m + n - 1;
        while(i >= 0 & j >= 0){
            if(nums1[i] >= nums2[j]) {
                nums1[k--] = nums1[i--];
            }else{
                nums1[k--] = nums2[j--];
            }
        }
        while(j >= 0) nums1[k--] = nums2[j--];
    }
};
```

### 基础排序

##### LeetCode 4. 寻找两个正序数组的中位数
    
```C++    
class Solution {
    int findKthNumbers(vector<int> &nums1, int i, vector<int> &nums2, int j, int k) {
        //边界条件处理，根据数组大小换顺序
        if (nums1.size() - i > nums2.size() - j) {
            return findKthNumbers(nums2, j, nums1, i, k);
        }
        if (nums1.size() == i) return nums2[j + k - 1];
        if (k == 1) return min(nums1[i], nums2[j]);
        int m = nums1.size();
        int s1 = min(m, i + k / 2);
        int s2 = j + k / 2;
        if (nums1[s1 - 1] > nums2[s2 - 1]) {
            return findKthNumbers(nums1, i, nums2, j + k / 2, k - k / 2);
        } else {
            return findKthNumbers(nums1, s1, nums2, j, k - (s1 - i));
        }
    }

public:
    double findMedianSortedArrays(vector<int> &nums1, vector<int> &nums2) {
        int total = nums1.size() + nums2.size();
        if (total % 2 == 0) {
            int left = findKthNumbers(nums1, 0, nums2, 0, total / 2);
            int right = findKthNumbers(nums1, 0, nums2, 0, total / 2 + 1);
            return (left + right) / 2.0;
        } else {
            return findKthNumbers(nums1, 0, nums2, 0, total / 2 + 1);
        }
    }
};
 
```    

##### 23 合并K个升序链表

```c++
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.size() == 0){
            return nullptr;
        }

        ListNode *head = nullptr;
        for(int i = 0;i < lists.size();i++){
            head = mergeTwoLists(head,lists[i]);
        }
        return head;
    }

    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {

        ListNode *dummy = new ListNode(0);
        ListNode *head = dummy;
        while(l1 && l2){
            if(l1->val < l2->val){
                head->next = l1;
                l1 = l1->next;
                head = head->next;
            }else{
                head->next = l2;
                l2 = l2->next;
                head = head->next;
            }
        }
        if(l1){
            head->next = l1;
        }
        if(l2){
            head->next = l2;
        }
        return dummy->next;
    }
};

```

### 双向遍历

##### 42. 接雨水

左右两边遍历 + 动态规划

```c++
class Solution {
public:
    int trap(vector<int>& height) {

        int n = height.size();
        vector<int> leftMax(n,0);
        vector<int> rightMax(n,0);
        leftMax[0] = height[0];
        for(int i = 1;i < n;i++){
            leftMax[i] = max(leftMax[i - 1],height[i]);
        }
        rightMax[n-1] = height[n-1];
        for(int i = n - 2;i > -1;i--){
            rightMax[i] = max(rightMax[i + 1],height[i]);
        }

        int sum = 0;
        for(int i = 0;i < n ;i++){
            int water = min(leftMax[i],rightMax[i]);
            sum += water - height[i];
        }

        return  sum;
    }
};

```

##### 135 分发糖果

双向遍历

```C++
class Solution {
public:
    int candy(vector<int> &ratings) {
        int res = 0;
        int n = ratings.size();
        vector<int> left(n, 1);
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                left[i] = left[i - 1] + 1;
            }
        }

        int right = 0;
        for (int i = n - 1; i >= 0; i--) {
            if (i < n - 1 && ratings[i] > ratings[i + 1]) {
                right++; //记递增序列
            } else {
                right = 1;
            }
            res += max(left[i], right);
        }

        return res;
    }
};
```

##### 238. 除自身以外数组的乘积

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int> &nums) {
        vector<int> answer(nums.size(), 0);
        answer[0] = 1;
        for (int i = 1; i < nums.size(); i++) {
            answer[i] = answer[i - 1] * nums[i - 1];
        }
        //左右遍历合并成一
        int Rmul = 1;
        for (int i = nums.size() - 1; i > -1; i--) {
            answer[i] *= Rmul;
            Rmul *= nums[i];
        }

        return answer;
    }
};

```	

### 回文问题


回文数问题总结：
                                       
1 中心扩展法：双指针           
2 动态规划：      
```C++
//注意遍历顺序
for (int i = n - 1; i >= 0; i--) { //注意循环顺序
    for (int j = i; j < n; j++) {
        if (i == j) f[i][j] = 1;
        else f[i][j] = (s[i] == s[j]) && f[i + 1][j - 1];
    }
}
```
3 字符串Hash(反转拷贝) ：           

##### 5  最长回文子串

回文问题

```C++
class Solution {
public:
    string palindrome(string s,int l,int r){
        while(l >=0 && r < s.size()){
            if(s[l] == s[r]){
                l -= 1;
                r += 1;
            }else{
                break;
            }
        }
        return s.substr(l+ 1,r - l-1);
    }

    string longestPalindrome(string s) {
        int len = s.size();
        int index = 0;
        char s0 = s[0];
        string res = string(1,s0);
        while(index < len){
            string str1 = palindrome(s,index,index);
            string str2 = palindrome(s,index,index + 1);
            string max_str = str1.size()> str2.size()?str1:str2;
            res = res.size() > max_str.size()?res:max_str;
            index += 1;
        }
        return res;
    }
};


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
##### 125 验证回文串

```C++
class Solution {
    bool check(char c) {
        return c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z' || c >= '0' && c <= '9';
    }

public:
    bool isPalindrome(string s) {
        int n = s.size();
        for (int i = 0, j = n - 1; i < j; i++, j--) {
            while (i < j && !check(s[i])) i++;
            while (i < j && !check(s[j])) j--;
            if (i < j && tolower(s[i]) != tolower(s[j])) return false;
        }
        return true;
    }
};
```

##### 234. 回文链表

nullptr和默认值不一样，默认的值为0

```C++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == nullptr || head->next == nullptr){
            return true;
        }

        ListNode* fast = head;
        ListNode* slow = head;
        ListNode* pre = head;
        //nullptr和默认值不一样，默认的值为0
        ListNode* prepre = nullptr;

        while(fast && fast->next){
            //边走边反转
            pre = slow;
            fast = fast->next->next;
            slow = slow->next;
            pre->next = prepre;
            prepre = pre;
        }
        if(fast){
            slow = slow->next;          
        }
        while(pre && slow){
            if(pre->val != slow->val){
                return false;
            }
            pre = pre->next;
            slow = slow->next;
        }
        return true;
    }
};
```
#####  680. 验证回文字符串 Ⅱ
 
 左右双指针判断回文数
    
```C++
class Solution {
public:
    bool validPalindrome(string s) {
        int n = s.size();
        for (int i = 0, j = n - 1; i < j;) {
            if (s[i] == s[j]) {
                i++;
                j--;
            } else {
                return isPalindrome(s, i + 1, j) || isPalindrome(s, i, j - 1);
            }
        }
        return true;
    }

    bool isPalindrome(string s, int l, int r) {
        for (int i = l, j = r; i < j;i++,j--) {
            if (s[i] != s[j]) return false;
        }
        return true;
    }
};    
    
```    
#####  647 回文子串

https://leetcode-cn.com/problems/palindromic-substrings/solution/dai-ma-sui-xiang-lu-dai-ni-xue-tou-dpzi-vidge/
 
枚举+ 中心展开 
    
```C++
//枚举+中心展开
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            ans += extend(s, i, i);
            ans += extend(s, i, i + 1);
        }
        return ans;
    }

    int extend(string s, int l, int r) {
        int ans = 0;
        int i = l, j = r;
        while (i >= 0 && j < s.size() && s[i] == s[j]) {
            i--;
            j++;
            ans++;
        }
        return ans;
    }
};
    
```

```C++
//dp解法
class Solution
{
public:
    int countSubstrings(string s)
    {
        int n = s.size();
        vector<vector<int>> f(n, vector<int>(n, 0));
        int ans = 0;
        //从状态转移方程，推导i和j的遍历顺序
        // f[i + 1][j - 1])  ---> f[i][j]
        for (int i = s.size() - 1; i >= 0; i--)
        {
            for (int j = i; j < s.size(); j++)
            {
                if ((s[i] == s[j]) && (j - i <= 1 || f[i + 1][j - 1]))
                {
                    f[i][j] = true;
                    ans++;
                }
            }
        }
        return ans;
    }
};

```


### 滑动窗口

**滑窗和/积满足某种条件区间的两类解法**

1  双指针 可变滑窗：先扩j后缩i，外扩j内缩i ；             
2  前缀和+二分(向后搜索)                   

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

**字符串统计滑窗法总结：**
    
1 双指针不固定长度滑窗 + 双hash(单hash)：      

单hash(补欠法)：https://www.acwing.com/solution/LeetCode/content/160/          
hash[c]表示的是当前c这个字母还缺多少个，hash[s[j]] == 0 表示 s[j] 这个字母已经足够了     

双hash(计有效字符法)：https://www.acwing.com/solution/content/63190/     

2 固定len滑窗 + vector<int> (26,0)，vector<int> 可以直接比较相等关系

##### 3. 无重复字符的最长子串

cpp多用++，少+=1

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char,int> hash;
        int res = 0;
        for(int i = 0,j = 0;i < s.size();i++){
            hash[s[i]]++;
            while(hash[s[i]] > 1) hash[s[j++]]--;
            res = max(res,i - j + 1);
        }
        return res;
    }
};
```


##### 76 最小覆盖字串

注意unordered_map 直接访问会自动创建，需要count先判断存在

```C++
//双hash版
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char,int> need;
        unordered_map<char,int> window;
        for(auto &str:t){
            need[str]++;
        }

        int left = 0;
        int right = 0;
        int n = s.size();
        int valid = 0;
        int minLen = 2*s.size();
        string minString;
        while(right < n){
            char char_right = s[right];
            right++;
            if(need.count(char_right)){
                window[char_right]++;
                if(window[char_right] == need[char_right]){
                    valid++;
                }
            }

            while(valid == need.size()){
                if(right - left < minLen){
                    minLen = right - left;
                    minString = s.substr(left,minLen);
                }
                char char_left = s[left];
                left++;
                if(need.count(char_left)) {
                    if(window[char_left] == need[char_left]) {
                        valid--;
                    }
                    window[char_left]--;
                }
            }
        }
        if(minLen == 2*s.size()){
            return "";
        }else{
            return minString;
        }
    }
};
```

  
```C++
//双hash 双指针简化版  
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> ht, hw;
        for (auto c:t) {
            ht[c]++;
        }
        int cnt = 0;
        string res;
        for (int i = 0, j = 0; i < s.size(); i++) {
            hw[s[i]]++;
            // hw[s[i]] <= ht[s[i]] 说明s[i]是个有效字符
            if (hw[s[i]] <= ht[s[i]]) cnt++;

            // hw[s[j]] > ht[s[j]] 说明s[j]是多余字符
            while(cnt == t.size() && hw[s[j]] > ht[s[j]]) hw[s[j++]]--;
            if (cnt == t.size()) {
                if (res.empty() || (i - j + 1) < res.size()) {
                    res = s.substr(j, i - j + 1);
                }
            }
        }
        return res;
    }
}; 
    
```		
	
```C++
//单hash版
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char,int> hash;
        for(auto ch:t) hash[ch]++;
        int cnt = hash.size();

        string res = "";
        int c = 0;
        for(int i = 0,j = 0;i < s.size();i++){
            if(hash[s[i]] == 1) c++;
            hash[s[i]]--;
            while(c == cnt && hash[s[j]] < 0) hash[s[j++]]++;
            if(c == cnt){
                if(res.empty() || res.size() > i - j + 1 ) res = s.substr(j,i - j  + 1);
            }
        }
        return  res;
    }
};
```
##### 438. 找到字符串中所有字母异位词

滑窗1：先right,后缩left，最小覆盖子串，最长无重复子串

滑窗2：先right,left同步前进，字符串排列，找所有字母异位词

简化滑窗：找所有字母异位词，统计 vector<int> sCount(26), vector<int> 可以直接比较相等关系

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        unordered_map<char,int> need;
        for(auto ch:p){
            need[ch]++;
        }

        vector<int> res;
        unordered_map<char,int> window;
        int left = 0;
        int right = 0;
        int validNum = 0;
        while(right < s.size()){
            char ch_right = s[right];
            right++;
            if(need.count(ch_right)){
                window[ch_right]++;
                if(window[ch_right] == need[ch_right]){
                    validNum++;
                }
            }

            while(right - left >= p.size()){
                if(validNum == need.size()) {
                    res.emplace_back(left);
                }
                char ch_left = s[left];
                left++;
                if(need.count(ch_left))
                {
                    if(window[ch_left] == need[ch_left]){
                        validNum--;
                    }
                    window[ch_left]--;
                }
            }
        }
        return res;
    }
};
```
	
简单滑窗
vector<int> 可以直接比较相等关系, vector<int> sCount(26,0); 

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int pLen = p.size();
        if(s.size() < pLen){
            return {};
        }
        vector<int> sCount(26,0);
        vector<int> pCount(26,0);
        for(int i = 0;i < p.size();i++){
            sCount[s[i] - 'a']++;
            pCount[p[i] - 'a']++;
        }
        vector<int> res;
        if(sCount == pCount){
            res.emplace_back(0);
        }
        for(int i = 0;i < s.size() - pLen;i++){
            --sCount[s[i] - 'a'];
            ++sCount[s[i + pLen] - 'a'];
            if(sCount == pCount){
                res.emplace_back(i + 1);
            }
        }
        return res;
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
    
				  
### Cyclic Sort，循环排序

##### 26. 删除有序数组中的重复项
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;
        int j = 0;
        for(int i = 1;i < nums.size();i++){
            if(nums[i] != nums[j])
                nums[++j] = nums[i];
        }
        return j + 1;
    }
};
```
##### 48 旋转图像

二次旋转

auto matrix_new = matrix;   // C++ 这里的 = 拷贝是值拷贝，会得到一个新的数组

matrix = matrix_new;  //赋值拷贝

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for(int i = 0;i < n/2;i++){
            for(int j = 0;j < n;j++){
                swap(matrix[i][j],matrix[n-i -1][j]);
            }
        }
        for(int i = 0;i <n;i++){
            for(int j = i + 1;j< n;j++){
                swap(matrix[i][j],matrix[j][i]);
            }
        }

    }
};
```
## 0x06 倍增
	
##### 29  两数相除

方法1：二分+快速乘法(位运算，参考快速幂模板)

方法2：预处理倍增数组 或逐步倍增法，放到负数范围，因为表示范围更大

```C++
//二分+快速乘法
class Solution {
public:
    int divide(int dividend, int divisor) {
        if (dividend == INT_MIN) {
            if (divisor == 1) return INT_MIN;
            if (divisor == -1) return INT_MAX;
        }
        if (divisor == INT_MIN)
            return dividend == INT_MIN ? 1 : 0;
        if (dividend == 0)
            return 0;

        long a = dividend, b = divisor;
        int sign = 1;
        if (a < 0 && b > 0 || a > 0 && b < 0) sign = -1;
        if (a < 0) a = -a;
        if (b < 0) b = -b;
        long l = 0, r = a;
        while (l < r) {
            long mid = l + r + 1 >> 1;
            if (mul(b, mid) <= a) l = mid;
            else r = mid - 1;
        }
        return sign == -1 ? -l : l;
    }

    //倍增快速幂
    long mul(long b, long k) {
        long result = 0;
        while (k) {
            if (k & 1) result += b;
            k >>= 1;
            b += b;
        }
        return result;
    }
};


//倍增数组法
class Solution {
public:
    int divide(int dividend, int divisor) {
        int LIMIT = INT_MIN / 2;
        if (dividend == INT_MIN) {
            if (divisor == 1) return INT_MIN;
            if (divisor == -1) return INT_MAX;
        }
        if (divisor == INT_MIN)
            return dividend == INT_MIN ? 1 : 0;
        if (dividend == 0)
            return 0;

        int a = dividend, b = divisor;
        int sign = 1;
        if (a < 0 && b > 0 || a > 0 && b < 0) sign = -1;
        //映射到负数范围，因为负数表示范围更大
        if (a > 0) a = -a;
        if (b > 0) b = -b;
        int ans = 0;
        while (a <= b) {
            int c = b, d = -1;
            while (c >= LIMIT && d >= LIMIT && c >= (a - c)) {
                c += c;
                d += d;
            }
            a -= c;
            ans += d;
        }
        return sign == -1 ? ans : -ans;
    }
};


```

## 0x07 贪心

##### 56. 合并区间

```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end());
        vector<vector<int>> merged;
        merged.emplace_back(intervals[0]);

        for(int i = 1;i < intervals.size();i++){
            int L = intervals[i][0];
            int R = intervals[i][1];
            if(L <= merged.back()[1]){
                merged.back()[1] = max(merged.back()[1],R);
            }else{
                merged.emplace_back(intervals[i]);
            }
        }
        
        return merged;
    }
};


//自定义key比较

class Solution {
public:
    static bool cmp(vector<int> &a,vector<int> &b){
        if(a[0] > b[0]){
            return false;
        }

        return true;
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        sort(intervals.begin(),intervals.end(),cmp);
        vector<vector<int>> merged;
        merged.emplace_back(intervals[0]);

        for(int i = 1;i < intervals.size();i++){
            int L = intervals[i][0];
            int R = intervals[i][1];
            if(L <= merged.back()[1]){
                merged.back()[1] = max(merged.back()[1],R);
            }else{
                merged.emplace_back(intervals[i]);
            }
        }
        
        return merged;
    }
};

```
	
##### Leetcode 57  插入区间

方法1：模拟，三段式：寻左边界 + 处理交集 + 右边界

方法2：模拟，三种情况讨论 + flag

```C++
//模拟，三段式：寻左边界 + 处理交集 + 右边界
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>> &intervals, vector<int> &newInterval) {
        int k = 0;
        vector<vector<int>> res;
        int start = newInterval[0], end = newInterval[1];
        //寻左边界
        while (k < intervals.size() && intervals[k][1] < start) res.emplace_back(intervals[k++]);

        //处理交集
        if (k < intervals.size()) {
            start = min(start, intervals[k][0]);
            while (k < intervals.size() && intervals[k][0] <= end) end = max(end, intervals[k++][1]);
        }
        res.emplace_back(vector<int>{start, end});

        //处理右边界
        while (k < intervals.size()) res.emplace_back(intervals[k++]);
        return res;
    }
};

//模拟，三种情况讨论 + flag
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>> &intervals, vector<int> &newInterval) {
        int k = 0;
        vector<vector<int>> res;
        int start = newInterval[0], end = newInterval[1];
        bool flag = false;
        for (auto &interval:intervals) {
            if (interval[1] < start) {
                res.emplace_back(interval);
            } else if (interval[0] > end) {
                if (!flag) {
                    res.push_back({start, end});
                    flag = true;
                }
                res.emplace_back(interval);
            } else {
                start = min(start, interval[0]);
                end = max(end, interval[1]);
            }
        }
        if (!flag) {
            res.push_back({start, end});//push_back可以直接{}构造vector
        }
        return res;
    }
};
```
	
##### 435. 无重叠区间

```C++
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        if (a[1] < b[1]) {
            return true;
        }
        return false;
    }

public:
    int eraseOverlapIntervals(vector<vector<int>> &intervals) {
        if (intervals.empty()) {
            return 0;
        }
        //sort(intervals.begin(),intervals.end(),cmp);
        sort(intervals.begin(), intervals.end(), [](auto &u, auto &v) {
            return u[1] < v[1];
        });

        auto right = intervals[0][1];
        int ans = 0;
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] < right) {
                ans++;
            } else {
                right = intervals[i][1];
            }
        }
        return ans;
    }
};
```

##### 55 跳跃游戏 I

贪心最远

```c++

class Solution {
public:
    bool canJump(vector<int>& nums) {
        int farest = 0;
        int n = nums.size();
        for(int i = 0;i < n;i++){
            if(i <= farest){
                farest = max(farest,i + nums[i]);
                if(farest >= n - 1){
                    return true;
                }
            }
        }

        return false;
    }
};

```
##### 860. 柠檬水找零

贪心，优先用10块的。 

```C++
class Solution {
public:
    bool lemonadeChange(vector<int> &bills) {
        int fives = 0, tens = 0;
        for (auto bill:bills) {
            if (bill == 5) {
                fives++;
            } else if (bill == 10) {
                if (!fives) return false;
                fives--;
                tens++;
            } else {
                if (tens && fives) {
                    fives--;
                    tens--;
                } else if (fives >= 3) {
                    fives -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
};
```

##### Leetcode 455. 分发饼干

贪心排序

```C++
class Solution {
public:
    int findContentChildren(vector<int> &g, vector<int> &s) {
        sort(g.begin(), g.end()); //小孩需要
        sort(s.begin(), s.end());
        int i = 0, j = 0;
        while (i < g.size() && j < s.size()) {
            while (j < s.size() && s[j] < g[i]) j++;
            if (j < s.size()) {
                i++;
                j++;
            }
        }
        return i;
    }
};

```

##### Leetcode 55. 跳跃游戏

```C++
class Solution {
public:
    bool canJump(vector<int> &nums) {
        int res = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i > res) {
                return false;
            }
            res = max(res, i + nums[i]);
            if (res >= nums.size() - 1) {
                return true;
            }
        }
        return false;
    }
};
```

##### Leetcode 45. 跳跃游戏 II

贪心区间问题

```C++
class Solution {
public:
    int jump(vector<int> &nums) {
        int step = 0;
        int n = nums.size();
        int end = 0, maxR = 0;
        for (int i = 0; i < n - 1; i++) {
            maxR = max(maxR, i + nums[i]);
            if (i == end) {
                end = maxR;
                step++;
            }
        }

        return step;
    }
};

```

##### LeetCode 376. 摆动序列
 
可以先nums.erase(unique(nums.begin(), nums.end()), nums.end()); 预处理下。

```C++ 
class Solution {
public:
    int wiggleMaxLength(vector<int> &nums) {
        if (nums.size() < 2)
            return nums.size();
        int preDiff = nums[1] - nums[0];
        int res = preDiff != 0 ? 2 : 1;
        for (int i = 2; i < nums.size(); i++) {
            int diff = nums[i] - nums[i - 1];
            if (preDiff >= 0 && diff < 0 || preDiff <= 0 && diff > 0) {
                res++;
                preDiff = diff;
            }
        }
        return res;
    }
};
```

##### LeetCode 406 根据身高重建队列

类信封问题，vector两维度排序

```C++
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[0] > b[0] || a[0] == b[0] && a[1] < b[1];
    }

public:
    vector<vector<int>> reconstructQueue(vector<vector<int>> &people) {
        sort(people.begin(), people.end(), cmp);
        vector<vector<int>> res;
        for (auto &p:people) {
            res.insert(res.begin() + p[1], p);
        }
        return res;
    }
};
```
##### 452 用最少数量的箭引爆气球

贪心区间问题，模板题，区间问题，按起点或终点排序。

```C++
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[1] < b[1];
    }

public:
    int findMinArrowShots(vector<vector<int>> &points) {
        sort(points.begin(), points.end(), cmp);
        int end = points[0][1], res = 1;
        for (auto &point:points) {
            if (point[0] > end) {
                end = point[1];
                res++;
            }
        }
        return res;
    }
};

```


##### LeetCode 402. 移掉 K 位数字

```C++
class Solution {
public:
    string removeKdigits(string num, int k) {
        stack<char> stk;
        for (auto &n:num) {
            while (stk.size() && stk.top() > n && k) {
                stk.pop();
                k--;
            }
            stk.push(n);
        }

        while (k--) stk.pop();
        string res;
        while (stk.size()) {
            res += stk.top();
            stk.pop();
        }
        reverse(res.begin(), res.end());
        int i = 0;
        while (i < res.size() && res[i] == '0') i++;
        if (i == res.size()) return "0";
        return res.substr(i);
    }
};
```

##### LeetCode 134  加油站

```C++
class Solution {
public:
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
        int m = gas.size();
        for (int i = 0, j = 0; i < m;) {
            int gas_left = 0;
            for (j = 0; j < m; j++) {
                int k = (i + j) % m;
                gas_left += gas[k] - cost[k];
                if (gas_left < 0) break;
            }
            if (j == m) {
                return i;
            } else {
                i += j + 1;
            }
        }
        return -1;
    }
};
```

# 0x10 基本数据结构

单调栈、单调队列、链表、二叉堆

## 0x11 栈/单调栈

### 栈基本操作

括号类问题 

##### 20 有效的括号

```c++
class Solution {
public:
    bool isValid(string s) {
        if (s.size() % 2 == 1) {
            return false;
        }
        stack<char> stack_c;
        unordered_map<char, char> pairs = {
                {')', '('},
                {']', '['},
                {'}', '{'}
        };
        for (auto ch:s) {
            if (pairs.count(ch)) {
                if((!stack_c.empty())&&(stack_c.top() == pairs[ch])){
                    stack_c.pop();
                }else{
                    return false;
                }
            } else {
                stack_c.push(ch);
            }
        }
        return stack_c.empty();
    }
};
```


##### 32. 最长有效括号

栈，辅助位置-1；（ 入栈，）出栈

```C++

class Solution {
public:
    int longestValidParentheses(string s) {

        stack<int> stack;
        int ans = 0;
        stack.push(-1);
        for(int i = 0;i < s.size();i++){
            if(s[i] == '('){
                stack.push(i);
            }else{
                stack.pop();
                if(stack.empty()){
                    stack.push(i);
                } else {
                    ans = max(ans, i - stack.top());
                }
            }
        }
        return  ans;
    }
};

```
##### Leetcode 150 逆波兰表达式求值

逆波兰记法,后缀表示法

```C++
class Solution {
public:
    int evalRPN(vector<string> &tokens) {
        stack<int> stk;
        for (auto &token:tokens) {
            if (token == "+" || token == "-" || token == "*" || token == "/") {
                auto b = stk.top();
                stk.pop();
                auto a = stk.top();
                stk.pop();
                if (token == "+") a += b;
                else if (token == "-") a -= b;
                else if (token == "*") a *= b;
                else a /= b;
                stk.push(a);
            } else {
                stk.push(stoi(token));
            }
        }
        return stk.top();
    }
};
```
	
### 单调栈

##### 84 柱状图中最大的矩形

ans + stack辅助

stack辅助：使得每次新元素入栈后，栈内的元素都保持有序

stack: 存坐标、存值，单调升、单调降

左右双单调栈：左一遍、右一遍

```C++
class Solution {
public:
    int largestRectangleArea(vector<int> &heights) {
        int n = heights.size();
        vector<int> left(n),right(n);
        stack<int> left_stack,right_stack;

        for (int i = 0; i < n; i++) {
            while (left_stack.size() && heights[left_stack.top()] >= heights[i]) left_stack.pop();
            left[i] = left_stack.empty() ? -1 : left_stack.top();
            left_stack.push(i);
        }

        for (int i = n - 1; i >= 0; i--) {
            while (right_stack.size() && heights[right_stack.top()] >= heights[i]) right_stack.pop();
            right[i] = right_stack.empty() ? n : right_stack.top();
            right_stack.push(i);
        }

        int res = 0;
        for(int i = 0;i < n;i++) res = max(res,heights[i]*(right[i] - left[i] - 1));
        return res;
    }
};
```
##### 85. 最大矩形

矩形面积，列加分解 + 单调栈

```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n,0);
        vector<int> right(n,0);
        stack<int> left_stack;
        stack<int> right_stack;
        for(int i = 0;i < heights.size();i++){
            while ((!left_stack.empty()) &&(heights[left_stack.top()] > heights[i])){
                left_stack.pop();
            }
            left[i] = left_stack.empty()? -1:left_stack.top();
            left_stack.push(i);
        }

        for(int j = n - 1;j > -1;j--){
            while ((!right_stack.empty()) &&(heights[right_stack.top()] > heights[j])){
                right_stack.pop();
            }
            right[j] = right_stack.empty()?n:right_stack.top();
            right_stack.push(j);
        }

        int maxArea = 0;
        for(int i = 0;i < n;i++){
            maxArea = max(maxArea,(right[i] - left[i] - 1) *heights[i]);
        }
        return  maxArea;
    }

    int maximalRectangle(vector<vector<char>> matrix) {

        int m = matrix.size();
        int n = matrix[0].size();
        vector<int> heights(n,0);
        int maxArea;
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                if(matrix[i][j] == '1'){
                    heights[j] += 1;
                }
            }
            int area = largestRectangleArea(heights);
            maxArea = max(maxArea,area);
        }
        return maxArea;
    }
};
```


##### 155. 最小栈

```C++
class MinStack {
    vector<int> commonStack;
    vector<int> minStack;
public:
    MinStack() {
    }

    void push(int val) {
        commonStack.emplace_back(val);
        if(minStack.empty()){
            minStack.emplace_back(val);
        }
        else{
	    #存储非升序
            minStack.emplace_back(min(val,minStack.back()));
        }
    }

    void pop() {
        commonStack.pop_back();
        minStack.pop_back();
    }

    int top() {
        return  commonStack.back();
    }

    int getMin() {
        return  minStack.back();
    }
};
```


##### Leetcode 456. 132 模式
    
```C++
class Solution {
public:
    bool find132pattern(vector<int> &nums) {
        stack<int> stk;
        int s3 = INT_MIN;
        //反向遍历单调栈
        for (int i = nums.size() - 1; i >= 0; i--) {
            if (s3 > nums[i]) return true;
            while (stk.size() && stk.top() < nums[i]) {
                s3 = stk.top();
                stk.pop();
            }
            stk.push(nums[i]);
        }
        return false;
    }
}; 
```    
    

##### LeetCode 496 下一个更大元素 I	
```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int> &nums1, vector<int> &nums2) {
        unordered_map<int, int> hash;
        stack<int> stk;
        vector<int> res;
        for (int i = nums2.size() - 1; i >= 0; i--) {
            while (stk.size() && stk.top() < nums2[i]) stk.pop();
            hash[nums2[i]] = stk.size() ? stk.top() : -1;
            stk.push(nums2[i]);
        }
        for (auto num: nums1) res.emplace_back(hash[num]);
        return res;
    }
};

```
##### LeetCode 503 下一个更大元素 II
```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int> &nums) {
        int m = nums.size();
        stack<int> stk;
        vector<int> res(m, -1);
        for (int i = 0; i < 2 *m; i++) {
            while (stk.size() && nums[stk.top()] < nums[i % m]) {
                res[stk.top()] = nums[i % m];
                stk.pop();
            }
            stk.push(i % m);
        }
        return res;
    }
};

```
	
##### 739. 每日温度
```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> stk;
        int n = temperatures.size();
        vector<int> res(n,0);
        for(int i = n - 1;i >= 0;i--){
            while(stk.size() && temperatures[stk.top()] <= temperatures[i]) stk.pop();
            if(stk.size()) res[i] = stk.top() - i;
            stk.push(i);
        }
        return res;
    }
};
```	

## 0x12 队列/单调队列

### 单调队列

##### 239. 滑动窗口最大值
	
存索引，方便出的判断
	
deque + 单调队列

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        deque<int> q;
        for(int i = 0;i < nums.size();i++){
            if(q.size() && i - q.front() >= k) q.pop_front();
            //单独下降队列
            while(q.size() && nums[q.back()] < nums[i]) q.pop_back();
            q.push_back(i);
            if(i >= k - 1) res.emplace_back(nums[q.front()]);
        }
        return res;
    }
};
```
##### 918 环形子数组的最大和

```c++
class Solution {
public:
    int maxSubarraySumCircular(vector<int> &nums) {
        vector<int> numArray(nums);
        int n = nums.size();
        for (auto num:nums) {
            numArray.emplace_back(num);
        }
        vector<int> S(2 * n + 1, 0);
        for (int i = 0; i < 2 * n; i++) {
            S[i + 1] = S[i] + numArray[i];
        }

        deque<int> q;
        int res = INT_MIN;
        q.emplace_back(0);
        for (int i = 1; i < 2 * n + 1; i++) {
            //这里求s,没有=，s[0]到s[n+1]
            if (q.size() && i - q.front() > n) q.pop_front();
            res = max(res, S[i] - S[q.front()]);
            //单调上升序列
            while (q.size() && S[q.back()] >= S[i]) q.pop_back();
            q.emplace_back(i);
        }

        return res;
    }
};
```

## 0x13 链表与邻接表

### 链表基本操作

##### 2. 两数相加

```C++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        //nullptr类型
        ListNode *head = nullptr;
        ListNode *tail = nullptr;
        int carry = 0;
        while(l1 || l2){
            int l1_val = l1?l1->val:0;
            int l2_val = l2?l2->val:0;

            int sum = l1_val + l2_val + carry;
            carry = sum/10;
            if(!head){
                //理解new的返回类型
                tail = head = new ListNode(sum%10);
            }else{
                tail->next = new ListNode(sum%10);
                tail = tail->next;
            }
            if(l1){
                l1 = l1->next;
            }
            if(l2){
                l2 = l2->next;
            }
        }
        if(carry > 0){
            tail->next = new ListNode(carry);
        }

        return head;
    }
};

```

##### 19 删除链表的倒数第 N 个结点

```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        auto fast = dummy,slow = dummy;
        while(n--){
            fast = fast->next;
        }
        while(fast->next){
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return dummy->next;
    }
};
```


##### 21 合并两个有序链表

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *dummy = new ListNode(0);
        ListNode *head = dummy;
        while(l1 && l2){
            if(l1->val < l2->val){
                head->next = l1;
                l1 = l1->next;
            } else{
                head->next = l2;
                l2 = l2->next;
            }
            head = head->next;
        }
        if(l1){
            head->next = l1;
        }
        if(l2){
            head->next = l2;
        }
        return  dummy->next;
```

##### 24. 两两交换链表中的节点

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        for(auto p = dummy;p && p->next && p->next->next;p = p->next->next){
            auto cur = p->next,next = cur->next;
            p->next = next;
            cur->next = next->next;
            next->next = cur;
        }
        return dummy->next;
    }
};
```
##### Leetcode 25  K 个一组翻转链表

```C++
class Solution {
public:
    ListNode *reverseKGroup(ListNode *head, int k) {
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        auto cur = dummy;
        while (cur) {
            auto first = cur->next;
            auto end = cur;

            for (int i = 0; i < k && end; i++) {
                end = end->next;
            }
            if (!end) break;

            auto p1 = first;
            auto p2 = first->next;
            while (p1 != end) {
                auto p2_tmp = p2->next;
                p2->next = p1;
                p1 = p2;
                p2 = p2_tmp;
            }

            first->next = p2;//当前段结束指向下一段开始
            cur->next = end;//上一段的结束指向当前段的开始(旋转前的真末尾)
            cur = first;
        }
        return dummy->next;
    }
};
```

##### 61. 旋转链表
```C++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head) return nullptr;
        int len = 0;
        auto cur = head;
        while(cur){
            len++;
            cur = cur->next;
        }
        k = k % len;

        auto first = head,second = head;
        while(k--) first = first->next;
        while(first->next){
            first = first->next;
            second = second->next;
        }
        first->next = head;
        head = second->next;
        second->next = nullptr;
        return head;
    }
};
```
##### Leetcode 82  删除排序链表中的重复元素 II

把链表当数组看到，特殊处理(提前取)取索引的方式。

```C++
class Solution {
public:
    ListNode *deleteDuplicates(ListNode *head) {
        if (!head) return head;
        auto dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *cur = dummy;
        while (cur->next && cur->next->next) {
            if (cur->next->val == cur->next->next->val) {
                int val = cur->next->val;
                while (cur->next && cur->next->val == val) {
                    cur->next = cur->next->next;
                }
            } else {
                cur = cur->next;
            }
        }
        return dummy->next;
    }
};

```
##### 83. 删除排序链表中的重复
```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        auto cur = head;
        while(cur){
            while(cur->next && cur->val == cur->next->val) cur->next = cur->next->next;
            cur = cur->next;
        }
        return head;
    }
};
```
##### Leetcode 86  分隔链表

大小链表存储，再链接，截断

```C++
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        auto small = new ListNode(-1);
        auto large = new ListNode(-1);
        auto sh = small,lh = large;
        for(auto p = head;p;p = p->next){
            if(p->val < x) {
                small->next = p;
                small = small->next;
            }
            else {
                large->next = p;
                large = large->next;
            }
        }
        small->next = lh->next;
        //链表截断
        large->next = nullptr;
        return sh->next;
    }
};
```
##### 92. 反转链表 II
```c++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if(left == right) return head;
        int m = left,n = right;

        auto dummy = new ListNode(-1);
        dummy->next = head;
        auto a = dummy,d = dummy;
        for(int i = 0 ;i < m - 1;i++) a = a->next;
        for(int i = 0 ;i < n;i++) d = d->next;
        auto b = a->next,c = d->next;
        // a（m-1） b d c（n+1)
        for(auto p = b,q = b-> next; q != c; )
        {
            auto tmp = q->next;
            q->next = p;
            p = q;
            q = tmp;
        };
        a->next = d;
        b->next = c;
        return dummy->next;
    }
};
```
##### 148. 排序链表

复杂问题，设计拆解

find middle --> sortList 递归 --> merge list

链表的经典模块组装

```C++

class Solution {
public:
    ListNode *sortList(ListNode *head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        ListNode *middle = findMiddle(head);

        ListNode *head2 = middle->next;
        middle->next = nullptr;
        ListNode *L1 = sortList(head);
        ListNode *L2 = sortList(head2);

        return merge(L1, L2);
    }

    ListNode *findMiddle(ListNode *head) {
        if(!head){
            return nullptr;
        }

        ListNode *fast = head->next;
        ListNode *slow = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    ListNode *merge(ListNode *L1, ListNode *L2) {
        ListNode *dummy = new ListNode(0);
        ListNode *head = dummy;
        while (L1 && L2) {
            if (L1->val < L2->val) {
                head->next = L1;
                L1 = L1->next;
            } else {
                head->next = L2;
                L2 = L2->next;
            }
            head = head->next;
        }
        if(L1) {
            head->next = L1;
        } 
        if(L2){
            head->next = L2;
        }

        return dummy->next;
    }
};
```

##### 160. 相交链表


```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        ListNode* nodeA = headA;
        ListNode* nodeB = headB;
        while(nodeA != nodeB){
            if(nodeA){
               nodeA = nodeA->next;
            } else{
                nodeA = headB;
            }
            if(nodeB){
                nodeB = nodeB->next;
            } else{
                nodeB = headA;
            }
        }

        return nodeA;
    }
};

```

#### Leetcode 138 复制带随机指针的链表

```C++
class Solution {
public:
    Node *copyRandomList(Node *head) {
        for (auto p = head; p; p = p->next->next) {
            Node *node = new Node(p->val);
            node->next = p->next;
            p->next = node;
        }

        for (auto p = head; p; p = p->next->next) {
            if (p->random) p->next->random = p->random->next;
        }

        Node *dummy = new Node(-1);
        auto cur = dummy;
        for (auto p = head; p; p = p->next) {
            cur->next = p->next;
            cur = cur->next;
            p->next = p->next->next;
        }

        return dummy->next;
    }
};

```

##### 141 环形链表

```C++

class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr){
            return false;
        }

        ListNode *slow = head;
        ListNode *fast = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
            if(fast==slow){
                return true;
            }
        }

        return false;
    }
};

```
##### 142. 环形链表 II

```C++							  
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head== nullptr || head->next == nullptr){
            return nullptr;
        }
        ListNode *slow = head;
        ListNode *fast = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast){
                break;
            }
        }

        if(slow != fast){
            return nullptr;
        }

        slow = head;
        while(slow != fast){
            fast = fast->next;
            slow = slow->next;
        }
        return fast;
    }
};
```
##### Leetcode 143  重排链表

模板题：寻找中点+ 逆序右半段 + 合并

```C++
class Solution {
public:
    void reorderList(ListNode *head) {
        if (!head) return;
        ListNode *mid = middle(head);
        ListNode *L1 = head;
        ListNode *L2 = mid->next;
        mid->next = nullptr;
        L2 = reverseList(L2);
        mergeList(L1, L2);
    }

    ListNode *middle(ListNode *head) {
        auto fast = head;
        auto slow = head;
        while (fast->next && fast->next->next) {
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }

    ListNode *reverseList(ListNode *head) {
        ListNode *prev = nullptr;
        ListNode *cur = head;
        while (cur) {
            auto nextTmp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = nextTmp;
        }
        return prev;
    }

    void mergeList(ListNode *L1, ListNode *L2) {
        ListNode *L1_next_tmp;
        ListNode *L2_next_tmp;
        while (L1 && L2) {
            L1_next_tmp = L1->next;
            L2_next_tmp = L2->next;

            L1->next = L2;
            L1 = L1_next_tmp;

            L2->next = L1_next_tmp;
            L2 = L2_next_tmp;
        }
    }
};

```


##### 206. 反转链表 

递归法

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr){
            return head;
        }

        ListNode* node = reverseList(head->next);
        //反转且避免成环
        head->next->next = head;
        head->next = nullptr;
        return node;
    }
};
```

迭代法：pre cur next

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr){
            return head;
        }
        ListNode* pre = nullptr;
        ListNode* cur = head;
        while(cur){
            //pre cur next的使用
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }

        return pre;
    }
};
```
	
##### 237. 删除链表中的节点
```C++
class Solution {
public:
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
    }
};
```
	
##### 445  两数相加 II

stack + 双指针链表
	
```C++
class Solution
{
public:
    ListNode *addTwoNumbers(ListNode *l1, ListNode *l2)
    {
        stack<int> s1, s2;
        while (l1)
        {
            s1.push(l1->val);
            l1 = l1->next;
        }
        while (l2)
        {
            s2.push(l2->val);
            l2 = l2->next;
        }
        int val1 = 0, val2 = 0, carry = 0;
        ListNode *ans = nullptr;
        while (!s1.empty() || !s2.empty() || carry)
        {
            val1 = s1.empty() ? 0 : s1.top();
            val2 = s2.empty() ? 0 : s2.top();
            if (!s1.empty())  s1.pop();
            if (!s2.empty())  s2.pop();
            int val = val1 + val2 + carry;
            carry = val / 10;
            val %= 10;
            auto curnode = new ListNode(val);
            curnode->next = ans;
            ans = curnode;
        }
        return ans;
    }
};	
	
```	

## 0x14  hash表

高级结构设计 LRU/LFU

### 哈希表

##### 1. 两数之和

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        for(int i = 0;i < nums.size();i++){
            if(hash.count(target - nums[i])){
                return {hash[target - nums[i]],i};
            }
            hash[nums[i]] = i;
        }
        return {-1,-1};
    }
};
```
##### Leetcode 41  缺失的第一个正数

hash空间简化，数组索引记录做hash，可以对比448题

```C++
class Solution {
public:
    int firstMissingPositive(vector<int> &nums) {
        int n = nums.size();
        //剔除负数和0
        for (int i = 0; i < n; i++) {
            if (nums[i] <= 0) nums[i] = n + 1;
        }
        for (int i = 0; i < n; i++) {
            int num = abs(nums[i]);
            if (num <= n) {
                nums[num - 1] = -abs(nums[num - 1]);
            }
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) return i + 1;
        }
        return n + 1;
    }
};
```					  
##### 49 字母异位词分组

map --value --list结构

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> hash;
        for(auto &str:strs){
            auto key = str;  //赋值
            sort(key.begin(),key.end());
            hash[key].emplace_back(str);
        }

        vector<vector<string>> res;
        for(auto item:hash){
            res.emplace_back(item.second);
        }
        return res;
    }
};
```
	
##### Leetcode 133 克隆图

```C++
class Solution {
    unordered_map<Node *, Node *> hash;
public:
    Node *cloneGraph(Node *node) {
        dfs(node);

        for (auto[s, d]:hash) {
            for (auto ver:s->neighbors) {
                d->neighbors.push_back(hash[ver]);
            }
        }

        return hash[node];
    }

    void dfs(Node *node) {
        hash[node] = new Node(node->val);

        for (auto ver:node->neighbors) {
            if (!hash.count(ver)) {
                dfs(ver);
            }
        }
    }
};
```
	
##### 448. 找到所有数组中消失的数字

hash: map ->vector 简化->原地修改简化为自己

```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n = nums.size();
        for(auto num:nums){
            int index = (num - 1) % n;
            nums[index] += n;
        }
        vector<int> res;
        for(int i = 0;i < nums.size();i++){
            if(nums[i] <= n){
                res.emplace_back(i + 1);
            }
        }
        return res;
    }
};
```
##### 187. 重复的DNA序列

```c++
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> res;
        unordered_map<string,int> hash;
        for(int i = 0;i + 10 <= s.size();i++){
            string t = s.substr(i,10);
            if(hash[t] == 1) {
                res.emplace_back(t);
            }
            hash[t]++;
        }
        return res;
    }
};

```

##### LeetCode 149 直线上最多的点数	
```C++
class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        if(points.empty()) return 0;
        int res = 1;
        for(int i = 0;i < points.size();i++){
            int duplicates = 0,vertical = 1;
            unordered_map<long double, int> hash;
            for (int j = i + 1; j < points.size(); j++) {
                if (points[i][0] == points[j][0]) {
                    vertical++;
                }
            }
            for (int j = i + 1; j < points.size(); j++) {
                if (points[i][0] != points[j][0]) {
                    long double slope = (long double) (points[i][1] - points[j][1]) / (points[i][0] - points[j][0]);
                    if (hash[slope] == 0) hash[slope] = 2;
                    else hash[slope]++;
                    res = max(res, hash[slope]);
                }
            }
            res = max(res,vertical);
        }

        return res;
    }
};

```

##### LeetCode 290 单词规律 
```C++
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        stringstream raw(s);
        vector<string> words;
        string word;
        while (getline(raw, word, ' ')) {
            words.emplace_back(word);
        }
        if (words.size() != pattern.size()) {
            return false;
        }
        unordered_map<string, char> SP;
        unordered_map<char, string> PS;
        for (int i = 0; i < words.size(); i++) {
            if (!SP.count(words[i])) SP[words[i]] = pattern[i];
            if (!PS.count(pattern[i])) PS[pattern[i]] = words[i];
            if (SP[words[i]] != pattern[i]) return false;
            if (PS[pattern[i]] != words[i]) return false;
        }
        return true;
    }
};
```
##### LeetCode 350 两个数组的交集 
```C++
class Solution {
public:
    vector<int> intersect(vector<int> &nums1, vector<int> &nums2) {
        multiset<int> mulSet;
        vector<int> res;
        for (auto num:nums1) mulSet.insert(num);
        for (auto num:nums2) {
            if (mulSet.count(num)) {
                res.emplace_back(num);
                mulSet.erase(mulSet.find(num));
            }
        }
        return res;
    }
};
```


##### LeetCode 454 四数相加 II	 
```C++
class Solution {
public:
    int fourSumCount(vector<int> &nums1, vector<int> &nums2, vector<int> &nums3, vector<int> &nums4) {
        unordered_map<int, int> hash;
        for (auto a:nums1) {
            for (auto b:nums2) {
                hash[a + b]++;
            }
        }
        int res = 0;
        for (auto c:nums3) {
            for (auto d:nums4) {
                if (hash.count(-c - d)) {
                    res += hash[-c - d];
                }
            }
        }
        return res;
    }
};
```

##### LeetCode554 砖墙 
```C++
class Solution {
public:
    int leastBricks(vector<vector<int>> &wall) {
        unordered_map<int, int> hash;
        int res = 0;
        for (auto brick:wall) {
            int s = 0;
            for (int i = 0; i < brick.size() - 1; i++) {
                s += brick[i];
                res = max(res, ++hash[s]);
            }
        }

        return wall.size() - res;
    }
};
```

### 数据结构设计 LRU/LFU

##### 146. LRU 缓存机制

双向链表(添加删除) + hashMAP（保存key + 地址）

双向链表：时间复杂度是O(1)，删除需要前驱节点。

struct Dnode --> Dnode List定义(伪头伪尾) + LRU定义 -——>Dnode API(delete、delte_tail、add_head)  -> LRU API get put 函数
				
				
```C++

//STL版,看思路，分解替换

class LRUCache {
private:

    int capacity;
    //cachelist 和cache map同步变化
    list<pair<int, int>> cacheList;// pair内为key、value
    unordered_map<int, list<pair<int, int>>::iterator> cacheMap;

public:
    LRUCache(int capacity) {
        this->capacity = capacity;
    }

    int get(int key) {
        if (!cacheMap.count(key)) {
            return -1;
        }
        //*取值迭代器
        auto it = *cacheMap[key];
        //删除迭代器
        cacheList.erase(cacheMap[key]);
        cacheList.push_front(it);
        cacheMap[key] = cacheList.begin();
        return cacheList.front().second;
    }

    void put(int key, int value) {
        //已存在
        if (cacheMap.count(key)) {
            auto it = *cacheMap[key];
            cacheList.erase((cacheMap[key]));
            it.second = value;
            cacheList.push_front(it);
            cacheMap[key] = cacheList.begin();
        } else {
            if (cacheList.size() == capacity) {
                cacheMap.erase(cacheList.back().first);
                cacheList.pop_back();
            }
            cacheList.push_front(make_pair(key, value));
            cacheMap[key] = cacheList.begin();
        }
        return;

    }
};
```

//Dlist 定义版

申请与释放:    
DListNode *head = new DListNode();    
delete(head):

正常删除节点,都要delete node释放内存，但这里如果后面add head,则不用释放    
delete tail的时候，需要释放内存
	
```C++
struct Node {
    int key;
    int val;
    Node *prev, *next;

    Node(int _key, int _val) : key(_key), val(_val), prev(nullptr), next(nullptr) {}
};

class LRUCache {
    Node *head, *tail;
    unordered_map<int, Node *> hash;
    int n, cur;

public:
    LRUCache(int capacity) {
        n = capacity;
        cur = 0;
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head->next = tail;
        tail->prev = head;
    }

    void remove(Node *node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    //头插删尾，尾插删头
    void add_to_head(Node *node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }

    int get(int key) {
        if (hash.count(key)) {
            Node *node = hash[key];
            remove(node);
            add_to_head(node);
            return node->val;
        } else {
            return -1;
        }
    }

    void put(int key, int value) {
        if (hash.count(key)) {
            Node *node = hash[key];
            node->val = value;
            remove(node);
            add_to_head(node);
        } else {
            if (hash.size() == n) {
                Node *node = tail->prev;
                remove(node);
                hash.erase(node->key);
                //要释放内存
                delete node;
            }
            Node *new_node = new Node(key, value);
            hash[key] = new_node;
            add_to_head(new_node);
        }
    }
};

```
	
##### 460. LFU 缓存
	
LFU： https://leetcode-cn.com/problems/lfu-cache/solution/gong-shui-san-xie-yun-yong-tong-pai-xu-s-53m3/

https://www.acwing.com/activity/content/code/content/555766/

 1  key_table(unordered_map<key,Node) + set<Node>(代替堆)，因为C++中堆pq的值不能改，Node是自定义排序的结构体。    
 
 2  key_table(unordered_map<key, list<Node>::iterator>) + freq_table(unordered_map<freq, list<Node>>) ：链接法。    
 散列冲突解决：链接法、开发定址法。类似邻接表结构，展开的list链表。类似图的存储结构map + map。
 
 map存储结构体，需要insert或emplace后使用make_pair,不能直接赋值
 
 ```C++
 struct ABC{
    int x,y;
    ABC(int _x,int _y):x(_x),y(_y){};
};
 
unordered_map<int,ABC> hash;
ABC abc(1,1);
hash.insert(make_pair(1,abc));//正确插入
//hash[1] = abc;//错误插入
ABC ac(1,2);
auto it = hash.find(1);
it->second = ac;//正确更新

```
 
结构体定义初始化列表和自定义排序结构
 
```C++
 struct Node {
    int freq, time;
    int key, val;

    Node(int _freq, int _time, int _key, int _val) : freq(_freq), time(_time), key(_key), val(_val) {}

    bool operator< (const Node &rhs) const {
        return freq == rhs.freq ? time < rhs.time : freq < rhs.freq;
    }
}; 
```

set + hash
```C++
 struct Node {
    int freq, time;
    int key, val;

    Node(int _freq, int _time, int _key, int _val) : freq(_freq), time(_time), key(_key), val(_val) {}

    bool operator< (const Node &rhs) const {
        return freq == rhs.freq ? time < rhs.time : freq < rhs.freq;
    }
};

class LFUCache {
    unordered_map<int, Node> key_table;
    set<Node> S;
    int capacity, time; //时间戳
public:
    LFUCache(int _capacity) {
        capacity = _capacity;
        time = 0;
        key_table.clear();
        S.clear();
    }

    int get(int key) {
        if (capacity == 0) return -1;
        auto it = key_table.find(key);
        if (it == key_table.end()) return -1;
        auto node = it->second;
        S.erase(node);
        node.freq += 1;
        node.time = ++time;
        S.insert(node);
        it->second = node;
        return node.val;
    }

    void put(int key, int value) {
        if (capacity == 0) return;
        auto it = key_table.find(key);
        if (it == key_table.end()) {
            if (key_table.size() == capacity) {
                key_table.erase(S.begin()->key);
                S.erase(S.begin());
            }
            Node node(1, ++time,key, value);
            S.insert(node);
            key_table.emplace(make_pair(key,node));
        } else {
            auto node = it->second;
            S.erase(node);
            node.freq += 1;
            node.time = ++time;
            node.val = value;
            S.insert(node);
            it->second = node;
        }
    }
};
 
 ```
 
 双hash表
 
 ```C++
 struct Node {
    int key, val, freq;
    Node(int _key, int _val, int _freq) : key(_key), val(_val), freq(_freq) {};
};

class LFUCache {
    int capacity, min_freq;
    unordered_map<int, list<Node>::iterator> key_table;//key -> list<node>::iterator
    unordered_map<int, list<Node>> freq_table; //freq -> list<node>
public:
    LFUCache(int _capacity) {
        capacity = _capacity;
        min_freq = 0;
        key_table.clear();
        freq_table.clear();
    }

    int get(int key) {
        if(capacity == 0) return -1;
        auto it = key_table.find(key);
        if (it == key_table.end()) return -1;
        auto node_it = it->second;
        int val = node_it->val;
        int freq = node_it->freq;
        freq_table[freq].erase(node_it);
        if (freq_table[freq].size() == 0) {
            freq_table.erase(freq);
            if (min_freq == freq) min_freq++;
        }
        freq_table[freq + 1].push_front(Node(key, val, freq + 1));
        key_table[key] = freq_table[freq + 1].begin();
        return val;
    }

    void put(int key, int value) {
        if(capacity == 0) return;
        auto it = key_table.find(key);
        if (it == key_table.end()) {
            if (key_table.size() == capacity) {
                auto node = freq_table[min_freq].back();
                freq_table[min_freq].pop_back();
                key_table.erase(node.key);
                if (freq_table[min_freq].size() == 0) {
                    freq_table.erase(min_freq);
                }
            }
            min_freq = 1;
            freq_table[min_freq].push_front(Node(key, value, min_freq));
            key_table[key] = freq_table[min_freq].begin();
        } else {
            auto node_it = key_table[key];
            int freq = node_it->freq;
            freq_table[freq].erase(node_it);
            if (freq_table[freq].size() == 0) {
                freq_table.erase(freq);
                if (min_freq == freq) min_freq++;
            }
            freq_table[freq + 1].push_front(Node(key, value, freq + 1));
            key_table[key] = freq_table[freq + 1].begin();
        }

    }
};

 ```								    
								    
	
##### 652. 寻找重复的子树

```c++
class Solution {
    unordered_map<string,int> hash;
    unordered_map<int,int> tree;
    vector<TreeNode*> ans;
    int cnt = 0;

public:
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        hash["#"] = ++cnt;
        dfs(root);
        return ans;
    }
    string dfs(TreeNode* root){
        if(!root) return to_string(hash["#"]);
        auto left = dfs(root->left);
        auto right = dfs(root->right);
        string t = to_string(root->val) + "," + left + "," + right;
        if(!hash.count(t)) hash[t] = ++cnt;

        if(tree[hash[t]] == 1) ans.emplace_back(root);
        tree[hash[t]]++;
        return to_string(hash[t]);
    }

};
```

## 0x15 字符串(字符串hash、KMP与最小表示法）

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

##### 28 实现 strStr()

BF 方法

KMP模板法

```C++
//BF算法
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.size(),n = needle.size();
        for (int i = 0; i + n <= m; i++) {
            //考虑全空的情况
            bool flag = true;
            for(int j = 0;j < n;j++){
                if(haystack[i + j] != needle[j]){
                    flag = false;
                    break;
                }
            }
            if(flag) return i;
        }
        return -1;
    }
};


//KMP模板
class Solution {
public:
    int strStr(string s, string p) {
        if (p.empty()) return 0;
        int m = s.size(), n = p.size();
        s = " " + s;
        p = " " + p;
        vector<int> next(n + 1, 0);
        //next[0] = next[1] = 0;
        //构建next数组
        for (int i = 2, j = 0; i <= n; i++) {
            while (j && p[i] != p[j + 1]) j = next[j];
            if (p[i] == p[j + 1]) j++;
            next[i] = j;
        }
        //匹配过程
        for (int i = 1, j = 0; i <= m; i++) {
            while (j && s[i] != p[j + 1]) j = next[j];
            if (s[i] == p[j + 1]) j++;
            if (j == n) {
                j = next[j];
                return i - n;
            }
        }
        return -1;
    }
};
```


##### 38. 外观数列

```c++
class Solution {
public:
    string countAndSay(int n) {
        string s = "1";
        for(int i = 0;i < n - 1;i++){
            string ns;
            for(int j = 0;j < s.size();j++){
                int k = j;
                while(k < s.size() && s[k] == s[j]) k++;
                ns += to_string(k - j) + s[j];
                j = k - 1;
            }
            s = ns;
        }
        return s;
    }
};
```

##### 71. 简化路径

std:find截取

```C++
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> dirs;
        for(auto i = path.begin();i != path.end();){
            ++i;
            auto j = find(i,path.end(),'/');
            auto dir = string(i,j);
            if(!dir.empty() && dir != "."){
                if(dir == ".."){
                    if(!dirs.empty()){
                        dirs.pop_back();
                    }
                } else if (dir != "."){
                    dirs.emplace_back(dir);
                }
            }
            i = j;
        }

        stringstream out;
        if(dirs.empty()){
            out<<'/';
        } else{
            for(auto ch:dirs){
                out << "/" << ch;
            }
        }
        return out.str();
    }
};

```
find + substr截取

```C++
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> dirs;
        for (int i = 0; i < path.size();) {
            ++i;
            auto j = path.find("/", i);
            if (j == string::npos) {
                j = path.size();
            }
            auto dir = path.substr(i, j - i);

            if (!dir.empty() && dir != ".") {
                if (dir == "..") {
                    if (!dirs.empty()) {
                        dirs.pop_back();
                    }
                } else if (dir != ".") {
                    dirs.emplace_back(dir);
                }
            }
            i = j;
        }

        stringstream out;
        if (dirs.empty()) {
            out << '/';
        } else {
            for (auto ch:dirs) {
                out << "/" << ch;
            }
        }
        return out.str();
    }
};

```
##### 1233. 删除子文件夹

```C++
class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        sort(folder.begin(),folder.end());
        vector<string> ans(1,folder[0]);
        string cur = ans[0] + "/";
        for(int i = 1;i < folder.size();){
            while(i < folder.size() && folder[i].find(cur) == 0){
                ++i;
            }
            if(i < folder.size()){
                ans.emplace_back(folder[i]);
                cur = folder[i] + "/";
                i++;
            }
        }

        return  ans;
    }
};
```

##### 6 Z 字形变换
```c++
class Solution {
public:
    string convert(string s, int n) {
        if (n == 1) return s;
        string res;
        for (int i = 0; i < n; i++) {
            if (!i || i == n - 1) {
                for (int j = i; j < s.size(); j += 2 * (n - 1)) res += s[j];
            } else {
                for (int j = i, k = 2 * (n - 1) - i; j < s.size() || k < s.size(); j += 2 * (n - 1), k += 2 * (n - 1)) {
                    if (j < s.size()) res += s[j];
                    if (k < s.size()) res += s[k];
                }
            }
        }
        return res;
    }
};
```
##### 165. 比较版本号
```c++
class Solution {
public:
    int compareVersion(string s1, string s2) {
        int i = 0,j = 0;
        while(i < s1.size() || j < s2.size()){
            int x = i,y = j;
            while(x < s1.size() && s1[x] != '.') x++;
            while(y < s2.size() && s2[y] != '.') y++;
            int a = x == i ? 0:atoi(s1.substr(i,x - i).c_str());
            int b = y == j ? 0:atoi(s2.substr(j,y - j).c_str());
            if(a > b) return 1;
            if(a < b) return -1;
            i = x + 1,j = y + 1;
        }
        return 0;
    }
};
```
##### 929. 独特的电子邮件地址
```c++
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> hash;
        for(int i = 0;i < emails.size();i++){
            int at = emails[i].find('@');
            string name;
            for(auto c:emails[i].substr(0,at)){
                if(c == '+') break;
                else if(c != '.') name += c;
            }
            string domain = emails[i].substr(at +1);
            hash.emplace(name + '@' + domain);
        }
        return hash.size();
    }
};
```

#### 394. 字符串解码

栈解法/DFS递归括号()解法(j是（ 的下一个,k暂存和统计)。

```cpp
//栈解法
class Solution {
public:
    string getDigits(string &s, int &ptr) {
        string ret;
        while (isdigit(s[ptr])) {
            ret.push_back(s[ptr++]);
        }
        return ret;
    }

    string getString(vector<string> &str) {
        string ret;
        for (const auto &s:str) {
            ret += s;
        }
        return ret;
    }

    string decodeString(string s) {
        vector<string> stk;
        //局部变量必须初始化
        int ptr=0;
        while (ptr < s.size()) {
            if (isdigit(s[ptr])) {
                string digits = getDigits(s, ptr);
                stk.emplace_back(digits);
            } else if (isalpha(s[ptr]) || s[ptr] == '[') {
                stk.emplace_back(string(1, s[ptr]));
                ptr++;
            } else {
                vector<string> sub;
                while (stk.back() != "[") {
                    sub.emplace_back(stk.back());
                    stk.pop_back();
                }
                stk.pop_back();

                reverse(sub.begin(), sub.end());
                auto times = stoi(stk.back());
                stk.pop_back();

                auto subString = getString(sub);
                string ret;
                while (times--) {
                    ret += subString;
                }
                stk.emplace_back(ret);
                ptr++;
            }
        }
        return getString(stk);
    }
};
```

```C++
//dfs递归解法
class Solution {
public:
    string decodeString(string s) {
        string res;
        for(int i = 0;i < s.size();){
            if(!isdigit(s[i])) res += s[i++];
            else{
                int j = i;
                while(isdigit(s[j])) j++;
                int t = atoi(s.substr(i,j - i).c_str());
                int k = j + 1;
                int sum = 0;
                while(sum >= 0){
                    if(s[k] == '[') sum++;
                    if(s[k] == ']') sum--;
                    k++;
                }
                string resstr = decodeString(s.substr(j + 1,k - j - 2));
                while (t--) res += resstr;
                i = k;
            }
        }
        return res;
    }
};
```

##### LeetCode 214 最短回文串

```cpp
//RK字符串hash  哈希检索算法（Robin-Karp，简称 RK 算法）
class Solution {
public:
    string shortestPalindrome(string s) {
        int n = s.size();
        int base = 131, mod = 1000000007;
        int left = 0, right = 0, mul = 1;
        int best = -1;
        for (int i = 0; i < n; i++) {
            left = ((long long) left * base + s[i]) % mod;
            right = (right + (long long) mul * s[i]) % mod;
            if (left == right) {
                best = i;
            }
            mul = ((long long)mul * base)% mod;
        }
        string add = best == n - 1 ? "" : s.substr(best + 1, n);
        reverse(add.begin(),add.end());
        return add + s;
    }
};

//kmp法
class Solution {
public:
    string shortestPalindrome(string s) {
        int n = s.size();
        string t(s.rbegin(), s.rend());
        s = ' ' + s + "#" + t;
        vector<int> ne(2 * n + 2);
        for (int i = 2, j = 0; i < 2 * n + 2; i++) {
            while (j && s[i] != s[j + 1]) j = ne[j];
            if (s[i] == s[j + 1]) j++;
            ne[i] = j;
        }
        int len = ne[2 * n + 1];
        string left = s.substr(1, len);
        string right = s.substr(1 + len, n - len);
        return string(right.rbegin(), right.rend()) + left + right;
    }
};

```

### 判断子序列

##### 392 判断子序列

双指针单for 取代双循环的技巧

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if (s.empty()) return true;
        for (int i = 0, j = 0; i < t.size(); i++) {
            if (t[i] == s[j]) j++;
            if (j == s.size()) return true;
        }
        return false;
    }
};
```
##### 522. 最长特殊序列 II

模板题: 判断子序列

```C++
class Solution
{
    bool check(string a, string b)
    {
        int k = 0;
        for (auto ch : b)
        {
            if (ch == a[k])
            {
                k++;
            }
        }
        return k == a.size();
    }

public:
    int findLUSlength(vector<string> &strs)
    {
        int res = -1;
        for (int i = 0; i < strs.size(); i++)
        {
            bool is_sub = false;
            for (int j = 0; j < strs.size(); j++)
            {
                if (i != j && check(strs[i], strs[j]))
                {
                    is_sub = true;
                    break;
                }
            }
            if (!is_sub)
                res = max(res, (int)(strs[i].size()));
        }
        return res;
    }
};
```

##### 523. 连续的子数组和

```C++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        if(!k){
            for(int i = 1;i < nums.size();i++){
                if(!nums[i - 1] && !nums[i]){
                    return true;
                }
            }
            return false;
        }
        int n = nums.size();
        if(n < 2) return false;
        vector<int> s(n + 1);
        for(int i = 1;i < n + 1;i++) {
            s[i] = s[i - 1] + nums[i - 1];
        } 
        unordered_set<int> hash;
        for(int i = 2; i < n + 1;i++){
            hash.insert(s[i - 2] % k); //间隔2存入
            if(hash.count(s[i] % k)){
                return true;
            }
        }
        return false;
    }
};
```
##### 524. 通过删除字母匹配到字典里最长单词

```C++
class Solution {
    bool check(string a,string b){
        int i = 0,j = 0;
        while(i < a.size() && j < b.size()){
            if(a[i] == b[j]) i++;
            j++;
        }
        return i == a.size();
    }
public:
    string findLongestWord(string s, vector<string>& dictionary) {
        string res;
        for(auto str:dictionary){
            if(check(str,s)){
                if(res.empty() || res.size() < str.size() || res.size() == str.size()&& str < res){
                    res = str;
                }
            }
        }
        return res;
    }
};
```

## 0x16  Trie树（字典树）

##### 208. 实现 Trie (前缀树)

Python，实例对象调用函数时，自动将对象本身传入函数的第一个变量是self，显示写出来

C++，系统也会将实例对象传入函数，对象的这个参数都是隐藏的，在函数内部才可以显性的使用它this

类与对象的使用

```C++
class Trie {
public:
    struct Node {
        bool is_end;
        Node *son[26];

        Node() {
            is_end = false;
            for (int i = 0; i < 26; i++) son[i] = nullptr;
        }
    } *root;

    Trie() {
        root = new Node();
    }

    void insert(string word) {
        auto node = root;
        for (auto c : word) {
            int u = c - 'a';
            if (!node->son[u]) node->son[u] = new Node();
            node = node->son[u];
        }
        node->is_end = true;
    }

    bool search(string word) {
        auto node = root;
        for (auto c : word) {
            int u = c - 'a';
            if (!node->son[u]) return false;
            node = node->son[u];
        }
        return node->is_end;
    }

    bool startsWith(string prefix) {
        auto node = root;
        for (auto c : prefix) {
            int u = c - 'a';
            if (!node->son[u]) return false;
            node = node->son[u];
        }
        return true;
    }
};
```	
## 0x17  二叉堆

### ToP k问题

##### 295. 数据流的中位数

```C++
class MedianFinder {
    priority_queue<int> left;
    priority_queue<int,vector<int>,greater<int>> right;
public:
    MedianFinder() {

    }

    void addNum(int num) {
        if(left.empty() || left.top() < num) right.push(num);
        else{
            right.push(left.top());
            left.pop();
            left.push(num);
        }
        if(right.size() > left.size() + 1){
            left.push(right.top());
            right.pop();
        }
    }

    double findMedian() {
        if((left.size() + right.size()) & 1){
            return right.top();
        } else{
            return (left.top() + right.top())/2.0;
        }
    }
};
/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

##### 347. 前 K 个高频元素

```C++
//自定义排序
class Solution {
public:
    static bool cmp(pair<int, int> &a, pair<int, int> &b) {
        return a.second > b.second;
    }

    vector<int> topKFrequent(vector<int> &nums, int k) {
        map<int, int> occurrences;
        for (auto &v:nums) {
            occurrences[v]++;
        }
        //自定义排序
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(&cmp)> q(cmp);
        for (auto &[num, count] :occurrences) {
            if (q.size() == k) {
                if (q.top().second < count) {
                    q.pop();
                    q.emplace(num, count);
                }
            } else {
                q.emplace(num, count);
            }
        }

        vector<int> res;
        while(!q.empty()){
            res.emplace_back(q.top().first);
            q.pop();
        }
        return res;
    }
};

//存负值
class Solution {
    using PIS = pair<int, string>;
public:
    vector<string> topKFrequent(vector<string> &words, int k) {
        priority_queue<PIS> heap;
        unordered_map<string, int> hash;

        for (auto word:words) {
            hash[word]++;
        }
        for (auto word:hash) {
            PIS t(-word.second, word.first);
            if(heap.size() == k && t < heap.top()) heap.pop();
            if(heap.size() < k) heap.emplace(t);
        }

        vector<string> res(k);
        for (int i = k - 1; i >= 0; i--) {
            res[i] = heap.top().second;
            heap.pop();
        }
        return res;
    }
};
```

##### 692. 前K个高频单词

```C++
class Solution {
    using PIS = pair<int, string>;
public:
    vector<string> topKFrequent(vector<string> &words, int k) {
        priority_queue<PIS> heap;
        unordered_map<string, int> hash;

        for (auto word:words) {
            hash[word]++;
        }
        for (auto word:hash) {
            PIS t(-word.second, word.first);
            if(heap.size() == k && t < heap.top()) heap.pop();
            if(heap.size() < k) heap.emplace(t);
        }

        vector<string> res(k);
        for (int i = k - 1; i >= 0; i--) {
            res[i] = heap.top().second;
            heap.pop();
        }
        return res;
    }
};

```

# 0x20  搜索

树遍历、DFS、BFS、动态规划

## 0x21 树与图的遍历

树的DFS（Tree Depth First Search，stack）

树的BFS(Tree Breadth First Search，queue)

### 树的DFS（Tree Depth First Search，stack）

##### 94. 二叉树的中序遍历

树天然递归，递归迭代都需要掌握

```C++
//递归法
class Solution {
public:
    void inorder(TreeNode* root,vector<int> &ans){
        if(!root){
            return;
        }
        inorder(root->left,ans);
        ans.emplace_back(root->val);
        inorder(root->right,ans);
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        inorder(root,ans);
        return ans; 
    }
};
//迭代法
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> ans;
        if(!root) return ans;

        while(root || stk.size()){
            //循环一侧走
            while(root){
                stk.push(root);
                root = root->left;
            }
            //出栈切方向
            root = stk.top();
            stk.pop();
            ans.emplace_back(root->val);
            root = root->right;
        }

        return ans;
    }
};
```
	
##### 144 二叉树的前序遍历

循环一侧走，出栈切方向

```C++
//递归
class Solution {
    vector<int> ans;
public:
    vector<int> preorderTraversal(TreeNode *root) {
        if (root) dfs(root);
        return ans;
    }

    void dfs(TreeNode *root) {
        ans.emplace_back(root->val);
        if (root->left) dfs(root->left);
        if (root->right) dfs(root->right);
    }
};

//迭代
class Solution {

public:
    vector<int> preorderTraversal(TreeNode *root) {
        vector<int> ans;
        stack<TreeNode *> stk;
        if (!root) return ans;

        while (root || !stk.empty()) {
            //循环一侧走
            while (root) {
                ans.emplace_back(root->val);
                stk.push(root);
                root = root->left;
            }
            //出栈切方向
            root = stk.top();
            stk.pop();
            root = root->right;
        }

        return ans;
    }
};

```

##### 145 二叉树的后序遍历

迭代：左右根 reverse 根右左。

```C++
//递归
class Solution {
    vector<int> ans;
public:
    vector<int> postorderTraversal(TreeNode *root) {
        if (root) dfs(root);
        return ans;
    }

    void dfs(TreeNode *root) {
        if (root->left) dfs(root->left);
        if (root->right) dfs(root->right);
        ans.emplace_back(root->val);
    }
};


//迭代
class Solution {
public:
    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> ans;
        stack<TreeNode *> stk;
        if(!root) return ans;
        
        while(root || !stk.empty()){
            while(root){
                ans.emplace_back(root->val);
                stk.push(root);
                root = root->right;
            }
            root = stk.top();
            stk.pop();
            root = root->left;
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```
	
	
	
	
##### 173 二叉搜索树迭代器
	
```c++
class BSTIterator {
    stack<TreeNode *> stk;
public:
    BSTIterator(TreeNode* root) {
        while(root){
            stk.emplace(root);
            root = root->left;
        }
    }
    
    int next() {
        auto cur = stk.top();
        stk.pop();
        auto right = cur->right;
        while(right){
            stk.emplace(right);
            right = right->left;
        }
        return cur->val;
    }
    
    bool hasNext() {
        return !stk.empty();
    }
};	
```
##### 96. 不同的二叉搜索树

二叉搜索树形状递归，向上比较

```C++
class Solution {
public:
    int numTrees(int n) {
        //G(n) = 求和 F(i,n)
        //F(i,n) = G(i-1)*G(n-i)
        //G(i,n) = 求和 G(i-1)*G(n-i)
        
        vector<int> G(n+1,0);
        G[0] = 1;
        G[1] = 1; 
        for(int i = 2;i < n + 1;i++){
            for(int j = 1;j <= i;j++){
                G[i] += G[j-1]*G[i-j];
            }
        }
        return G[n];
    }
};
```
##### 98 验证二叉搜索树

注意二叉搜索树，左子树所有节点都小于当前节点的值，右子树所有节点都大于当前节点的值。

```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return dfs(root,LONG_MIN,LONG_MAX);
    }

    bool dfs(TreeNode* root,long long minVal,long long maxVal){
        if(!root) return true;
        if(root->val < minVal || root->val > maxVal)  return false;

        return dfs(root->left,minVal,root->val - 1ll) && dfs(root->right,root->val + 1ll,maxVal);
    }


};

```
##### Leetcode  99 恢复二叉搜索树

迭代中序遍历，默认升序 -> 找逆序元素

```C++
class Solution {
public:
    void recoverTree(TreeNode *root) {
        TreeNode *x = nullptr;
        TreeNode *y = nullptr;
        TreeNode *pred = nullptr;
        stack<TreeNode *> stk;
        while (!stk.empty() || root != nullptr) {
            while (root) {
                stk.emplace(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            if (pred != nullptr && root->val < pred->val) {
                y = root;
                if (x == nullptr) {
                    x = pred;
                } else break;
            }

            pred = root;
            root = root->right;
        }
        swap(x->val, y->val);
    }

};

```

##### 100  相同的树

```C++
class Solution {
public:
    bool isSameTree(TreeNode *p, TreeNode *q) {
        if (!p && !q) return true;
        if (!p && q) return false;
        if (p && !q) return false;
        if (p->val != q->val) return false;
        return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }
};
```	
##### Leetcode 101. 对称二叉树

壳 + 递归函数 

```C++
class Solution {
public:
    bool dfs(TreeNode* p,TreeNode* q){
        if(!p || !q) return !p && !q;
        return p->val == q->val && dfs(p->right,q->left) && dfs(p->left,q->right);
    }
    bool isSymmetric(TreeNode* root) {
        if(!root) return true;
        return dfs(root->left,root->right);
    }
};
```

##### Leetcode 104. 二叉树的最大深度

```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root){
            return 0;
        }
        return max(maxDepth(root->left),maxDepth(root->right)) + 1;

    }
};
```

##### Leetcode 105 从前序与中序遍历序列构造二叉树

hash中序索引 + 切索引

```C++
class Solution {
    //存储中序遍历的元素与索引
    unordered_map<int, int> hash;
public:
    TreeNode *buildTree(vector<int> &preOrder, vector<int> &inOrder) {
        for (int i = 0; i < inOrder.size(); i++) {
            hash[inOrder[i]] = i;
        }
        return buildTree(preOrder, inOrder, 0, preOrder.size() - 1, 0, inOrder.size() - 1);
    }

    TreeNode *buildTree(vector<int> &preOrder, vector<int> &inOrder, int pl, int pr, int il, int ir) {
        if (pl > pr) return nullptr;

        int val = preOrder[pl];
        int pos = hash[val];
        //new 方式
        TreeNode *root = new TreeNode(val);
        root->left = buildTree(preOrder, inOrder, pl + 1, pl + pos - il, il, pos - 1);
        root->right = buildTree(preOrder, inOrder, pl + pos - il + 1, pr, pos + 1, ir);
        return root;
    }
};

```

##### Leetcode 106  从中序与后序遍历序列构造二叉树

hash中序索引 + 切索引

```C++
class Solution {
    unordered_map<int,int> hash;
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        for(int i = 0;i < inorder.size();i++){
            hash[inorder[i]] = i;
        }
        return buildTree(inorder,postorder,0,inorder.size() - 1,0,postorder.size() - 1);
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder,int il,int ir,int pl,int pr){
        if(il > ir) return nullptr;
        int val = postorder[pr];
        int pos = hash[val];
        TreeNode *root = new TreeNode(val);
        root->left  = buildTree(inorder,postorder,il,pos - 1,pl, pl + pos - il - 1);
        root->right = buildTree(inorder,postorder,pos + 1,ir,pl + pos - il,pr - 1);
        return root;
    }
};

```
#####  Leetcode 108. 将有序数组转换为二叉搜索树

切索引建树，建树模板函数设计要实现合理复用代码

```C++
class Solution {
public:
    TreeNode *sortedArrayToBST(vector<int> &nums) {
        return sortedArrayToBST(nums, 0, nums.size() - 1);
    }

    TreeNode *sortedArrayToBST(vector<int> &nums, int l, int r) {
        if (l > r) return nullptr;
        int k = l + r >> 1;
        TreeNode *root = new TreeNode(nums[k]);
        root->left = sortedArrayToBST(nums, l, k - 1);
        root->right = sortedArrayToBST(nums, k + 1, r);
        return root;
    }
};

```

##### Leetcode 109 有序链表转换二叉搜索树

占位节点，计数用

```C++
class Solution {
public:
    TreeNode *sortedListToBST(ListNode *head) {
        int len = getLen(head);
        return sortedListToBST(head, 0, len - 1);
    }

    int getLen(ListNode *head) {
        int res = 0;
        while (head) {
            head = head->next;
            res++;
        }
        return res;
    }

    TreeNode *sortedListToBST(ListNode *&head, int l, int r) {
        if (l > r) {
            return nullptr;
        }
        int mid = l + r >> 1;
        TreeNode *root = new TreeNode(-1);
        root->left = sortedListToBST(head, l, mid - 1);
        root->val = head->val;
        head = head->next;
        root->right = sortedListToBST(head, mid + 1, r);
        return root;
    }
};
```
##### 110  平衡二叉树
```C++
//递归定义
class Solution {
public:
    bool isBalanced(TreeNode *root) {
        if (!root) return true;
        return (abs(height(root->left) - height(root->right)) <= 1) && isBalanced(root->left) && isBalanced(root->right);
    }

    int height(TreeNode *root) {
        if (!root) return 0;
        return max(height(root->left), height(root->right)) + 1;
    }
};

//下沉判断
class Solution {
    bool ans;
public:
    bool isBalanced(TreeNode *root) {
        if (!root) return true;
        ans = true;
        dfs(root);
        return ans;
    }

    int dfs(TreeNode *root) {
        if (!root) return 0;
        int lh = dfs(root->left);
        int rh = dfs(root->right);
        if(abs(lh - rh) > 1) ans = false;
        return max(lh, rh) + 1;
    }
};
```

#####  111. 二叉树的最小深度

```C++
class Solution {
public:
    int minDepth(TreeNode *root) {
        if (!root) return 0;
        int left = minDepth(root->left);
        int right = minDepth(root->right);
        if(!left) return right + 1;
        if(!right) return left + 1;
        return min(left, right) + 1;
    }
};
```

##### 112 路径总和

```C++
class Solution {
public:
    bool hasPathSum(TreeNode *root, int targetSum) {
        if (!root) return false;
        if (root->left == nullptr && root->right == nullptr) return root->val == targetSum;
        return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
    }
};

```

##### 113 路径总和 II

```C++
class Solution {
    vector<vector<int>> res;
    vector<int> path;
public:
    vector<vector<int>> pathSum(TreeNode *root, int targetSum) {
        if (root) dfs(root, targetSum);
        return res;
    }

    void dfs(TreeNode *root, int sum) {
        sum -= root->val;
        path.emplace_back(root->val);
        if (sum == 0 && root->left == nullptr && root->right == nullptr) {
            res.emplace_back(path);
        } else {
            //选择
            if (root->left) dfs(root->left, sum);
            if (root->right) dfs(root->right, sum);
        }
        path.pop_back();
    }
};

```

##### Leetcode 114. 二叉树展开为链表



```C++
//递归：树天然递归
class Solution {
public:
    void flatten(TreeNode* root) {

        if(!root){
            return;
        }
        //找左子树最右，连接右子树
        flatten(root->left);
        auto right = root->right;
        root->right = root->left;
        //主动置空
        root->left = nullptr;
        while(root->right){
            root = root->right;
        }
        flatten(right);
        root->right = right;

        return;  
    }
};

```

寻找前驱节点：左子树的最右节点连接右子树，左子树连接到root->right,置空root->left。继续处理链表的下一个节点。
                                                          
```C++
//迭代                                                         
class Solution {
public:
    void flatten(TreeNode *root) {
        while (root) {
            auto p = root->left;
            if (p) {
                while (p->right) p = p->right;
                p->right = root->right;
                root->right = root->left;
                root->left = nullptr;
            }
            root = root->right;
        }
    }
};                                                           
                                                           
```   
##### Leetcode 116 填充每个节点的下一个右侧节点指针

使用已建立的next 指针，位于第N层时为第N+1层建立next 指针

局部变量不会初始化的，是随机值；全局变量才会初始化成默认的，比如指针类型会初始化为空

```C++
class Solution {
public:
    Node *connect(Node *root) {
        if (!root) return nullptr;

        auto res = root;
        while (root->left) {
            //遍历一层
            for (auto p = root; p; p = p->next) {
                p->left->next = p->right;
                if(p->next) p->right->next = p->next->left;
            }
            //挪到下一层最左
            root = root->left;
        }
        return res;
    }
};

```

#### Leetcode 117  填充每个节点的下一个右侧节点指针 II

位于第N层时为第N+1层建立next 指针

```C++
class Solution {
public:
    Node *connect(Node *root) {
        if (!root) return nullptr;
        auto cur = root;
        while (cur) {
            Node *head = new Node(-1);
            auto tail = head;
            //遍历一层,建立下一层
            for (auto p = cur; p; p = p->next) {
                if (p->left) tail = tail->next = p->left;
                if (p->right) tail = tail->next = p->right;
            }
            //挪到下一层最左
            cur = head->next;
        }
        return root;
    }
};
```
	
#####  Leetcode 124  二叉树中的最大路径和

壳 + recur

树天然递归

```C++
class Solution {
    int maxnum = INT_MIN;
public:
    int recur(TreeNode* root){
        if(!root){
            return 0;
        }
        int left = max(recur(root->left),0);
        int right = max(recur(root->right),0);
        maxnum = max(maxnum,left + right + root->val);

        return max(left + root->val,right + root->val);
    }

    int maxPathSum(TreeNode* root) {
        (void)recur(root);
        return maxnum;

    }
};

```
##### Leetcode 129  求根节点到叶节点数字之和

前判空好于后判空

```C++
class Solution {
    int ans;
public:
    int sumNumbers(TreeNode *root) {
        if (root) dfs(root, 0);
        return ans;
    }

    void dfs(TreeNode *root, int num) {
        num = num * 10 + root->val;
        if (!root->left && !root->right) ans += num;
        if (root->left) dfs(root->left, num);
        if (root->right) dfs(root->right, num);
    }
};

```
#####  226. 翻转二叉树

```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr){
            return root;
        }
        TreeNode * right = invertTree(root->right);
        TreeNode * left = invertTree(root->left);

        root->left = right;
        root->right = left;
        return root;
    }
};
```
##### 236. 二叉树的最近公共祖先

树天然递归

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root || root == p || root == q) return root;
        auto left = lowestCommonAncestor(root->left,p,q);
        auto right = lowestCommonAncestor(root->right,p,q);

        if(!left) return right;
        if(!right) return left;
        return root;
    }
};
```
	
##### 538. 把二叉搜索树转换为累加树

```cpp
class Solution {
    int sum = 0;
    void traverse(TreeNode* root){
        if(!root){
            return;
        }
        traverse(root->right);
        sum += root->val;
        root->val = sum;
        traverse(root->left);
    }
public:
    TreeNode* convertBST(TreeNode* root) {
        traverse(root);
        return root;
    }
};
```
##### 543. 二叉树的直径

```cpp
class Solution {
    int ans = 0;
    int dfs(TreeNode *root) {
        if (!root) {
            return 0;
        }
        int L = dfs(root->left);
        int R = dfs(root->right);
        ans = max(ans, L + R);
        return max(L, R) + 1;
    }

public:
    int diameterOfBinaryTree(TreeNode *root) {
        dfs(root);
        return ans;
    }
};
```


	
### 树的BFS(Tree Breadth First Search，queue)

##### 102 二叉树的层序遍历

queue<TreeNode *> 结构

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode *root) {
        vector<vector<int>> ans;
        if (!root) return ans;

        queue<TreeNode *> q;
        q.push(root);
        while (!q.empty()) {
            int len = q.size();
            vector<int> level;
            for (int i = 0; i < len; i++) {
                auto node = q.front();
                q.pop();
                
                level.emplace_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            if(level.size()) ans.emplace_back(level);
        }

        return ans;
    }
};
```
##### Leetcode 103  二叉树的锯齿形层序遍历

BFS + flag

```C++
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode *root) {
        vector<vector<int>> res;
        queue<TreeNode *> q;
        bool flag = true;
        if (root) q.emplace(root);

        while (!q.empty()) {
            int size = q.size();
            vector<int> path;
            for (int i = 0; i < size; i++) {
                auto top = q.front();
                q.pop();

                path.emplace_back(top->val);
                if (top->left) q.emplace(top->left);
                if (top->right) q.emplace(top->right);
            }

            if (!flag) reverse(path.begin(), path.end());
            res.emplace_back(path);
            flag = !flag;
        }

        return res;
    }
};
```
##### LeetCode 107 二叉树的层序遍历II

```C++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode *root) {
        vector<vector<int>> res;
        queue<TreeNode *> q;
        if (root) q.emplace(root);
        while (!q.empty()) {
            int size = q.size();
            vector<int> level;
            for (int i = 0; i < size; i++) {
                auto top = q.front();
                q.pop();
                level.emplace_back(top->val);
                if (top->left) q.emplace(top->left);
                if (top->right) q.emplace(top->right);
            }
            res.emplace_back(level);
        }

        reverse(res.begin(), res.end());
        return res;
    }
};

```


## 0x22 DFS(递归、回溯)

组合\子集\排列、8皇后问题、flood fill问题

省份问题（DFS版） 岛屿问题  路径问题

### 组合\子集\排列

##### 17 电话号码的字母组合

```C++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return vector<string>();
        string hash[8] = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> res(1, "");
        for (auto u:digits) {
            vector<string> now;
            for (auto c:hash[u - '2'])
                for (auto path:res)
                    now.push_back(path + c);
            res = now;
        }
        return res;
    }
};
```
##### 22. 括号生成
```c++
class Solution {
public:
    void backtrack(int leftcount,int rightcount,string &path,vector<string> &ans,int n){
        if(path.size() == 2*n){
            ans.emplace_back(path);
            return;
        }

        if(leftcount < n){
            path.push_back('(');
            backtrack(leftcount+1,rightcount,path,ans,n);
            path.pop_back();
        }

        if(rightcount < leftcount){
            path.push_back(')');
            backtrack(leftcount,rightcount+1,path,ans,n);
            path.pop_back();
        }
    }

    vector<string> generateParenthesis(int n) {

        vector<string> ans;
        string path;
        backtrack(0,0,path,ans,n);

        return ans;
    }
};
```

#####  301. 删除无效的括号

理解DSAPP栈与括号部分

单种括号计数可以，多种需要使用栈

去重复,判断前后重复，要从start开始

多叉递归回溯 + 剪枝 远快于   <  双分支回溯

```C++
class Solution {
    vector<string> res;
public:
    bool isValid(string s) {
        int cnt = 0;
        //理解DSAPP栈与括号部分
        //单种括号计数可以，多种需要使用栈
        for (auto c:s) {
            if (c == '(') {
                cnt++;
            } else if (c == ')') {
                cnt--;
            }
            if (cnt < 0) {
                return false;
            }
        }
        return true;
    }

    void dfs(string s, int start, int left_remove, int right_remove) {
        if (left_remove == 0 && right_remove == 0) {
            if (isValid(s)) {
                res.emplace_back(s);
            }
            return;
        }
        //从start开始，避免)(f重复
        for (int i = start; i < s.size(); i++) {
            //去重复
            if (i != start && s[i] == s[i - 1]) {
                continue;
            }

            //choice
            if (left_remove + right_remove > s.size() - i) {
                return;
            }

            if (left_remove > 0 && s[i] == '(') {
                dfs(s.substr(0, i) + s.substr(i + 1), i, left_remove - 1, right_remove);
            } else if (right_remove > 0 && s[i] == ')') {
                dfs(s.substr(0, i) + s.substr(i + 1), i, left_remove, right_remove - 1);
            }
        }

    }


    vector<string> removeInvalidParentheses(string s) {
        int left_remove = 0;
        int right_remove = 0;
        for (auto c:s) {
            if (c == '(') {
                left_remove++;
            } else if (c == ')') {
                if (left_remove == 0) {
                    right_remove++;
                } else {
                    left_remove--;
                }
            }
        }

        string path;
        dfs(s, 0, left_remove, right_remove);
        return res;
    }
};

```

##### 46 全排列 I

1  全局visit[bool] 函数  2  swap动态交换，模仿组合排列过程

```c++
class Solution {
    vector<vector<int>> ans;
public:
    vector<vector<int>> permute(vector<int>& nums) {
        int m = nums.size();
        vector<int> st(m,0);
        vector<int> path;
        backtrack(nums,0,st,path);
        return ans;
    }

    void backtrack(vector<int>& nums,int u,vector<int>& st,vector<int>& path){
        if(u == nums.size()) {
            ans.emplace_back(path);
            return;
        }
        for(int i = 0;i < nums.size();i++){
            if(!st[i]){
                st[i] = 1;
                path.emplace_back(nums[i]);
                backtrack(nums,u + 1,st,path);
                path.pop_back();
                st[i] = 0;
            }
        }
    }
};

```
##### 47. 全排列 II

```c++
class Solution {
    vector<vector<int>> ans;
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {

        sort(nums.begin(),nums.end());
        vector<int> st(nums.size(),0);
        vector<int> path;
        backtrack(nums,0,st,path);
        return ans;
    }

    void backtrack(vector<int>& nums,int u,vector<int>& st,vector<int>& path){
        if(u == nums.size()) {
            ans.emplace_back(path);
            return;
        }
        for(int i = 0;i < nums.size();i++){
            if(!st[i]){
                st[i] = true;
                path.emplace_back(nums[i]);
                backtrack(nums,u + 1,st,path);
                path.pop_back();
                st[i] = false;

                while(i + 1 < nums.size() && nums[i] == nums[i+1]) i++;
            }
        }
    }
};
```

##### 78 子集问题

回溯二叉 与回溯多叉

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int m = nums.size();
        vector<vector<int>> ans;
        for(int i = 0;i < 1 << m;i++){
            vector<int> path;
            for(int j = 0;j < m;j++){
                if(i >> j & 1) path.emplace_back(nums[j]);
            }
            ans.emplace_back(path);
        }
        return ans;
    }
};
```
##### 90. 子集 II

```c++
class Solution {
    vector<vector<int>> ans;
public:
    vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        int m = nums.size();
        sort(nums.begin(), nums.end());
        vector<int> path;
        dfs(nums, path, 0);
        return ans;
    }

    void dfs(vector<int> nums, vector<int> &path, int u) {
        if (u == nums.size()) {
            ans.emplace_back(path);
            return;
        }
        int k = 0;
        while (u + k < nums.size() && nums[u] == nums[u + k]) k++;
        for (int i = 0; i <= k; i++) {
            dfs(nums, path, u + k);
            path.emplace_back(nums[u]);
        }
        for (int i = 0; i <= k; i++) path.pop_back();
    }
};
```
#####  LeetCode 39. 组合总和

dfs + 排除等效冗余+ 可行性剪枝

```C++
class Solution {
    vector<vector<int>> res;
    vector<int> path;
public:
    vector<vector<int>> combinationSum(vector<int> &candidates, int target) {
        dfs(candidates, 0, target);
        return res;
    }

    void dfs(vector<int> &can, int u, int target) {
        if (target == 0) {
            res.emplace_back(path);
            return;
        }
        if (u == can.size()) return;
        for (int i = 0; can[u] * i <= target; i++) {
            dfs(can, u + 1, target - can[u] * i);
            path.emplace_back(can[u]);
        }
        for (int i = 0; can[u] * i <= target; i++) {
            path.pop_back();
        }
    }
};
```

##### LeetCode 40  组合总和 II

```C++
class Solution {
    vector<vector<int>> res;
    vector<int> path;
public:
    vector<vector<int>> combinationSum2(vector<int> &candidates, int target) {
        sort(candidates.begin(), candidates.end());
        dfs(candidates, 0, target);
        return res;
    }

    void dfs(vector<int> &can, int u, int target) {
        if (target == 0) {
            res.emplace_back(path);
            return;
        }
        if (u == can.size()) return;
        int k = u + 1;
        while (k < can.size() && can[k] == can[u]) k++;
        for (int i = 0; can[u] * i <= target && u + i <= k; i++) {
            dfs(can, k, target - can[u] * i);
            path.emplace_back(can[u]);
        }
        for (int i = 0; can[u] * i <= target && u + i <= k; i++) {
            path.pop_back();
        }
    }
};

```
							  
##### 77 组合

k - 1 逆向统计数

对比acwing  state + (1 << i)  用法

```C++
class Solution {
    vector<vector<int>> ans;
    vector<int> path;
public:
    vector<vector<int>> combine(int n, int k) {
        dfs(1, k, n);
        return ans;
    }

    void dfs(int start, int k, int n) {
        if (!k) {
            ans.emplace_back(path);
            return;
        }
        for (int i = start; i <= n; i++) {
            path.emplace_back(i);
            dfs(i + 1, k - 1, n);
            path.pop_back();
        }
    }
};

```
				  
##### 216. 组合总和 III

```c++
class Solution {
    vector<vector<int>> ans;
    vector<int> path;
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(k, n, 1);
        return ans;
    }

    //k选数个数，n和要求，start 起点
    void dfs(int k, int n, int start) {
        if (!k) {
            if (!n) ans.emplace_back(path);
            return;
        }
        for (int i = start; i <= 10 - k; i++) {
            path.emplace_back(i);
            dfs(k - 1, n - i, i + 1);
            path.pop_back();
        }
    }
};
```


### DFS典型（八皇后/数独、木棍）

##### Leetcode 51  N皇后

遍历行，col + dg + udg 标记数组     

主对角线满足行下标减去列下标之差相等，即row - col，方便处理row -col + n。         
副对角线满足行下标和列下标之和相等，即row + col。    

```C++
class Solution {
    vector<int> col, dg, udg;//列、正副对角线做标记数组，遍历行
    vector<vector<string>> ans;
    vector<string> path;
    int n;
public:
    vector<vector<string>> solveNQueens(int _n) {
        n = _n;
        col = vector<int>(n, 0);
        dg = udg = vector<int>(2 * n, 0);
        path = vector<string>(n, string(n, '.'));
        dfs(0);
        return ans;
    }

    void dfs(int row) {
        if (row == n) {
            ans.emplace_back(path);
            return;
        }

        //逐行填充
        for (int i = 0; i < n; i++) {
            //主对角线满足行下标减去列下标之差相等，即row - col，方便处理row -col + n。
            //副对角线满足行下标和列下标之和相等，即row + col
            if (!col[i] && !dg[row - i + n] && !udg[row + i]) {
                col[i] = dg[row - i + n] = udg[row + i] = 1;
                path[row][i] = 'Q';
                dfs(row + 1);
                path[row][i] = '.';
                col[i] = dg[row - i + n] = udg[row + i] = 0;
            }
        }
    }
};

```
 				  
##### LeetCode 52. N皇后 II

```c++
class Solution {
    int ans = 0;
    int m;
    vector<int> col, d, ud;
public:
    int totalNQueens(int n) {
        m = n;
        col = vector<int>(n, 0); //列上
        d = vector<int>(2 * n, 0);
        ud = vector<int>(2 * n, 0);
        dfs(0);
        return ans;
    }

    void dfs(int u) {
        if (u == m) {
            ans ++;
            return;
        }
        for (int i = 0; i < m; i++) {
            if (!col[i] && !d[i + u] && !ud[i - u + m]) {
                col[i] = d[i + u] = ud[i - u + m] = 1;
                dfs(u + 1);
                col[i] = d[i + u] = ud[i - u + m] = 0;
            }
        }
    }
};
```
##### LeetCode 36  有效的数独

lt里面全局变量会被初始化，局部变量不会被初始化

```C++
class Solution {
    int rows[9][9],cols[9][9],grids[9][9][9];
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        for(int i = 0;i < 9;i++){
            for(int j = 0;j < 9;j++){
                if (board[i][j] != '.') {
                    int t = board[i][j] - '1';
                    if (rows[i][t] || cols[j][t] || grids[i /3][j /3][t]) {
                        return false;
                    }
                    rows[i][t] = cols[j][t] =  1;
                    grids[i / 3][j / 3][t] = 1;
                }
            }
        }
        return  true;
    }
};
```
##### LeetCode 37 解数独

```c++
class Solution {
    bool row[9][9],col[9][9],cell[3][3][9];
public:
    void solveSudoku(vector<vector<char>> &board) {
        int m = board.size(), n = board[0].size();
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++) {
                if (board[i][j] != '.') {
                    int t = board[i][j] - '1';
                    row[i][t] = col[j][t] = cell[i / 3][j / 3][t] = true;
                }
            }

        dfs(board, 0, 0);
    }

    bool dfs(vector<vector<char>> &board, int x, int y) {
        if (y == 9) x++, y = 0;
        if (x == 9) return true;
        if(board[x][y] != '.') return dfs(board,x,y+1);

        for (int i = 0; i < 9; i++) {
            if (!row[x][i] && !col[y][i] && !cell[x / 3][y / 3][i]) {
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = true;
                board[x][y] = '1' + i;
                if (dfs(board, x, y + 1)) return true;
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = false;
                board[x][y] = '.';
            }
        }

        return false;
    }
};
```
##### LeetCode 473. 火柴拼正方形

经典剪枝

```c++
class Solution {
    vector<int> st;
public:
    bool makesquare(vector<int> &nums) {

        int sum = 0;
        for (auto num:nums) sum += num;
        if (!sum || sum % 4) return false;

        sort(nums.begin(), nums.end());
        reverse(nums.begin(), nums.end());

        st = vector<int>(nums.size(), 0);
        return dfs(nums, 0, 0, 0, sum / 4);
    }

    bool dfs(vector<int> &nums, int u, int cur, int start, int target) {
        if (u == 4) return true;
        if (cur == target) return dfs(nums, u + 1, 0, 0, target);

        for (int i = start; i < nums.size(); i++) {
            if (!st[i] && cur + nums[i] <= target) {
                st[i] = true;
                if (dfs(nums, u, cur + nums[i], i + 1, target)) return true;
                st[i] = false;

                while (i + 1 < nums.size() && nums[i] == nums[i + 1]) i++;
                if (!cur) return false;
                if (cur + nums[i] == target) return false;
            }
        }
        return false;
    }
};
```


##### LeetCode 54. 螺旋矩阵

```C++
class Solution
{
public:
    vector<int> spiralOrder(vector<vector<int>> &matrix)
    {
        int m = matrix.size(), n = matrix[0].size();
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        vector<vector<int>> st(m, vector<int>(n, 0));
        vector<int> ans;
        int i = 0, j = 0, d = 0;
        for (int k = 0; k < m * n; k++)
        {
            int a = i + dx[d], b = j + dy[d];
            if (a < 0 || a >= m || b < 0 || b >= n || st[a][b])
            {
                d = (d + 1) % 4;
                a = i + dx[d], b = j + dy[d];
            }
            st[i][j] = true;
            ans.emplace_back(matrix[i][j]);
            i = a, j = b;
        }
        return ans;
    }
};
```
#####  Leetcode 59 螺旋矩阵 II

四方向遍历

```C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0));
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        int x = 0, y = 0, d = 0;
        for (int k = 1; k <= n * n; k++) {
            res[x][y] = k;
            int a = x + dx[d], b = y + dy[d];
            if (a < 0 || a >= n || b < 0 || b >= n || res[a][b]) {
                d = (d + 1) % 4;
                a = x + dx[d], b = y + dy[d];
            }
            x = a, y = b;
        }
        return res;
    }
};
```
	
#####  93. 复原 IP 地址

dfs + u/k两个截止条件，类似木棍问题。   

```C++
class Solution {
    vector<string> ans;
    vector<int> path;
public:
    vector<string> restoreIpAddresses(string s){
        dfs(s,0,0);
        return ans;
    }
    void dfs(string s,int u,int k){
        if(u == s.size()){
            if(k == 4){
                string ip = to_string(path[0]);
                for(int i = 1;i < 4;i++){
                    ip += "." + to_string(path[i]);
                }
                ans.emplace_back(ip);
            }
            return;
        }
        if(k > 4) return;
        int t = 0;
        for(int i = u;i < s.size();i++){
            t = t * 10 + s[i] - '0';
            if(0 <= t && t <= 255){
                path.emplace_back(t);
                dfs(s,i + 1,k + 1);
                path.pop_back();
            }else break;
            if(!t) break;
        }
    }
};
```

##### 95. 不同的二叉搜索树 II

dfs区间l-r，枚举根节点。  

```C++
class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        if(!n) return vector<TreeNode*>();
        return dfs(1,n);
    }

    vector<TreeNode*> dfs(int l,int r){
        vector<TreeNode*> res;
        if(l > r){
            res.emplace_back(nullptr);
            return res;
        }
        for(int i = l;i <= r;i++){
            vector<TreeNode*> left = dfs(l,i - 1),right = dfs(i + 1,r);
            for(auto lc:left){
                for(auto rc:right){
                    TreeNode * root = new TreeNode(i);
                    root->left = lc;
                    root->right = rc;
                    res.emplace_back(root);
                }
            }
        }
        return res;
    }
};
```

##### 695 岛屿的最大面积
```C++
class Solution {
    int m,n;
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        m = grid.size(),n = grid[0].size();
        int res = 0;
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                if(grid[i][j] == 1){
                    res = max(res,dfs(grid,i,j));
                }
            }
        }
        return res;
    }

    int dfs(vector<vector<int>>& grid,int i,int j){
        if(i < 0 || i >= m || j < 0 || j >=n || !grid[i][j]){
            return 0;
        }

        grid[i][j] = 0;
        int dx[4] = {-1,0,1,0},dy[4] = {0,1,0,-1};
        int ans = 1;
        for(int k = 0;k < 4;k++){
            int x  = i + dx[k],y = j + dy[k];
            ans += dfs(grid,x,y);
        }
        return ans;
    }
};
```

##### 529. 扫雷游戏

```C++
class Solution {
    int m,n;
public:
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        int x = click[0],y = click[1];
        m = board.size(),n = board[0].size();
        if(board[x][y] == 'M'){
            board[x][y] = 'X';
            return board;
        }

        dfs(board,x,y);
        return board;
    }

    void dfs(vector<vector<char>>& board,int x,int y){
        if(board[x][y] != 'E') return;
        int s = 0;
        for(int i = max(x - 1,0);i <= min(x + 1,m - 1);i++){
            for(int j = max(y - 1,0);j <= min(y + 1,n - 1);j++){
                if(i != x || j != y){      
                    if(board[i][j] == 'M' || board[i][j] == 'X'){
                        s++;
                    }
                }
            }
        }
        if(s){
            board[x][y] = '0' + s;
            return;
        }
        board[x][y] = 'B';
        for(int i = max(x - 1,0);i <= min(x + 1,m - 1);i++){
            for(int j = max(y - 1,0);j <= min(y + 1,n - 1);j++){
                if(i != x || j != y){     
                    dfs(board,i,j);
                }
            }
        }
    }
};
```
                             
##### Leetcode 329. 矩阵中的最长递增路径

滑雪：记忆化dfs,memo,既当visit又存储值

```cpp
class Solution {
    int m, n;
    vector<vector<int>> memo;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
public:
    int longestIncreasingPath(vector<vector<int>> &matrix) {
        m = matrix.size(), n = matrix[0].size();
        memo = vector<vector<int>>(m, vector<int>(n, -1));
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                res = max(res, dp(i, j, matrix));
            }
        }
        return res;
    }

    int dp(int i, int j, vector<vector<int>> &matrix) {
        //记忆化dfs,memo,既当visit又存储值
        if (memo[i][j] != -1) return memo[i][j];
        memo[i][j] = 1;
        for (int k = 0; k < 4; k++) {
            int newI = i + dx[k], newJ = j + dy[k];
            if (newI >= 0 && newI < m && newJ >= 0 && newJ < n && matrix[newI][newJ] < matrix[i][j]) {
                memo[i][j] = max(memo[i][j], dp(newI, newJ, matrix) + 1);
            }
        }
        return memo[i][j];
    }
}; 
```    
    

##### 784 字母大小写全排列

```C++
class Solution{
    int m = 0;
    vector<string> ans;

public:
    vector<string> letterCasePermutation(string s){
        m = s.size();
        string path;
        dfs(s, path,0);
        return ans;
    }

    void dfs(string &s, string &path, int u){
        if (u == m)
        {
            ans.emplace_back(path);
            return;
        }

        path.push_back(s[u]);
        dfs(s, path, u + 1);
        path.pop_back();

        if ('a' <= s[u] && s[u] <= 'z' || 'A' <= s[u] && s[u] <= 'Z')
        {
            path.push_back(s[u] ^ 32);
            dfs(s, path, u + 1);
            path.pop_back();
        }
        return;
    }
};
```

 

### DFS floodfill问题

##### 79 单词搜索

1  pair 管理方向   
2  visited 去重复访问路径      
3  flood fill 四方向访问  

```c++
class Solution {
public:
    int m, n;
    int dx[4] = {0, 0, -1, 1}, dy[4] = {-1, 1, 0, 0};

    bool exist(vector<vector<char>> &board, string word) {
        if (!board.size() || board[0].empty()) return false;
        m = board.size(), n = board[0].size();
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (dfs(board, i, j, word, 0))
                    return true;
        return false;
    }

    bool dfs(vector<vector<char>> &board, int i, int j, string word, int u) {
        if (board[i][j] != word[u]) return false;
        if (u == word.size() - 1) return true;

        board[i][j] = '.';
        for (int k = 0; k < 4; k++) {
            int newI = i + dx[k], newJ = j + dy[k];
            if (newI >= 0 && newI <= m - 1 && newJ >= 0 && newJ <= n - 1)
                if(dfs(board, newI, newJ, word, u + 1)) return true;
        }
        board[i][j] = word[u];
        return false;
    }
};

```
##### Leetcode 130 被围绕的区域
    
四边dfs + 涂色法
    
```C++
class Solution {
    int m,n;
public:
    void solve(vector<vector<char>> &board) {
        m = board.size(),n = board[0].size();
        for (int i = 0; i < m; i++) {
            dfs(i, 0, board);
            dfs(i, n - 1, board);
        }
        for (int j = 0; j < n; j++) {
            dfs(0, j, board);
            dfs(m - 1, j, board);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'A') {
                    board[i][j] = 'O';
                } else if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                }
            }
        }
    }

    void dfs(int i, int j, vector<vector<char>> &board) {
        if (i < 0 || i > m - 1 || j < 0 || j > n - 1 || board[i][j] != 'O')
            return;
        board[i][j] = 'A';
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        for (int k = 0; k < 4; k++) {
            int newI = i + dx[k], newJ = j + dy[k];
            dfs(newI, newJ, board);
        }
    }
};    
```    

#### 200 岛屿问题

floodfill回溯,涂色问题
	
```C++
class Solution {
public:
    void dfs(vector<vector<char>> &grid, int row, int col) {
        if (row < 0 || row > grid.size() - 1 || col < 0 || col > grid[0].size() - 1) {
            return;
        }
        if (grid[row][col] != '1') {
            return;
        }
        grid[row][col] = '2';
        dfs(grid, row - 1, col);
        dfs(grid, row, col - 1);
        dfs(grid, row + 1, col);
        dfs(grid, row, col + 1);

    }

    int numIslands(vector<vector<char>> &grid) {

        int m = grid.size();
        int n = grid[0].size();
        int sum = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    sum += 1;
                }
            }
        }
        return sum;
    }
};

```	
##### LeetCode 733. Flood Fill 图像渲染

```c++
class Solution {
    int m, n;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
public:
    vector<vector<int>> floodFill(vector<vector<int>> &image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) return image;
        m = image.size(), n = image[0].size();
        int color = image[sr][sc];
        image[sr][sc] = newColor;
        for (int k = 0; k < 4; k++) {
            int newI = sr + dx[k], newJ = sc + dy[k];
            if (0 <= newI && newI < m && 0 <= newJ && newJ < n && (image[newI][newJ] == color)) {
                floodFill(image, newI, newJ, newColor);
            }
        }
        return image;
    }
};
```


## 0x23 BFS

层序遍历

BFS: queue + while + for
	
##### 127 单词接龙

双向BFS理解每次各自扩展一层的含义

26 字母扩展方法

```C++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string> &wordList) {
        unordered_set<string> Sets;
        for (auto &word:wordList) {
            Sets.emplace(word);
        }
        if(!Sets.count(endWord))
            return 0;
        queue<string> qBegin;
        qBegin.emplace(beginWord);
        unordered_map<string, int> distBegin;
        distBegin[beginWord] = 0;

        queue<string> qEnd;
        qEnd.emplace(endWord);
        unordered_map<string, int> distEnd;
        distEnd[endWord] = 0;

        while (qBegin.size() && qEnd.size()) {
            int len1 = qBegin.size();
            for (int k = 0; k < len1; k++) {
                auto word = qBegin.front();
                qBegin.pop();
                if (distEnd.count(word)) {
                    return distBegin[word] + distEnd[word] + 1;
                }
                for (int i = 0; i < word.size(); i++) {
                    auto tmp = word;
                    for (int j = 'a'; j <= 'z'; j++) {
                        tmp[i] = j;
                        if (Sets.count(tmp) && !distBegin.count(tmp)) {
                            distBegin[tmp] = distBegin[word] + 1;
                            qBegin.emplace(tmp);
                        }
                    }
                }
            }

            int len2 = qEnd.size();
            for (int k = 0; k < len2; k++) {
                auto wordEnd = qEnd.front();
                qEnd.pop();
                if (distBegin.count(wordEnd)) {
                    return distBegin[wordEnd] + distEnd[wordEnd] + 1;
                }
                for (int i = 0; i < wordEnd.size(); i++) {
                    auto tmp = wordEnd;
                    for (int j = 'a'; j <= 'z'; j++) {
                        tmp[i] = j;
                        if (Sets.count(tmp) && !distEnd.count(tmp)) {
                            distEnd[tmp] = distEnd[wordEnd] + 1;
                            qEnd.emplace(tmp);
                        }
                    }
                }
            }

        }
        return 0;
    }
};
```

##### 126 单词接龙 II

BFS（26扩展方法） + DFS 反向搜索

```
class Solution {
    string targetWord;
    vector<string> path;
    vector<vector<string>> ans;
    unordered_map<string, int> dist;

public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string> &wordList) {
        unordered_set<string> Sets;
        for (auto &word:wordList) {
            Sets.emplace(word);
        }
        if(!Sets.count(endWord))
            return ans;
        queue<string> q;
        q.emplace(beginWord);
        dist[beginWord] = 0;

        bool found = false;
        while (q.size()) {
            int len = q.size();
            for (int k = 0; k < len; k++) {
                auto word = q.front();
                q.pop();
                if (word == endWord) {
                    found = true;
                    break;
                }
                for (int i = 0; i < word.size(); i++) {
                    auto tmp = word;
                    for (int j = 'a'; j <= 'z'; j++) {
                        tmp[i] = j;
                        if (Sets.count(tmp) && !dist.count(tmp)) {
                            dist[tmp] = dist[word] + 1;
                            q.emplace(tmp);
                        }
                    }
                }
            }

            if(found) break;
        }

        targetWord = beginWord;
        if(found){
            path.push_back(endWord);//反向搜索
            dfs(endWord);
        }

        return ans;
    }

    void dfs(string endWord)
    {
        if(endWord == targetWord){
            reverse(path.begin(),path.end());
            ans.emplace_back(path);
            reverse(path.begin(),path.end());
        }else{
            for (int i = 0; i < endWord.size(); i++) {
                auto tmp = endWord;
                for (int j = 'a'; j <= 'z'; j++) {
                    tmp[i] = j;
                    if (dist.count(tmp) && dist[endWord] == dist[tmp] + 1) {
                        path.emplace_back(tmp);
                        dfs(tmp);
                        path.pop_back();
                    }
                }
            }
        }
    }
};

```

##### 547. 省份数量

DFS和BFS的经典对比

DFS
```c++
class Solution {
public:
    void dfs(vector<vector<int>>& isConnected, vector<int>& visited, int provinces, int i) {
        for (int j = 0; j < provinces; j++) {
            if (isConnected[i][j] == 1 && !visited[j]) {
                visited[j] = 1;
                dfs(isConnected, visited, provinces, j);
            }
        }
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        int provinces = isConnected.size();
        vector<int> visited(provinces);
        int circles = 0;
        for (int i = 0; i < provinces; i++) {
            if (!visited[i]) {
                dfs(isConnected, visited, provinces, i);
                circles++;
            }
        }
        return circles;
    }
};

```
BFS : 最短路径问题，空间换时间

```c++
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int provinces = isConnected.size();
        vector<int> visited(provinces);
        int circles = 0;
        queue<int> Q;
        for (int i = 0; i < provinces; i++) {
            if (!visited[i]) {
                Q.push(i);
                while (!Q.empty()) {
                    int j = Q.front(); Q.pop();
                    visited[j] = 1;
                    for (int k = 0; k < provinces; k++) {
                        if (isConnected[j][k] == 1 && !visited[k]) {
                            Q.push(k);
                        }
                    }
                }
                circles++;
            }
        }
        return circles;
    }
};

```

#####  LeetCode 542. 01 Matrix

矩形多源头的bfs，类比入度为0的拓扑排序.

```C++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>> &mat) {
        int m = mat.size(), n = mat[0].size();
        queue<pair<int, int>> q;
        vector<vector<int>> dist(m, vector<int>(n, -1));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 0) {
                    dist[i][j] = 0;
                    q.emplace(make_pair(i, j));
                }
            }
        }

        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        while (q.size()) {
            auto top = q.front();
            q.pop();
            for (int k = 0; k < 4; k++) {
                int x = top.first + dx[k], y = top.second + dy[k];
                if (0 <= x && x < m && 0 <= y && y < n && dist[x][y] == -1) {
                    dist[x][y] = dist[top.first][top.second] + 1;
                    q.emplace(make_pair(x,y));
                }
            }
        }
        return dist;
    }
};
```
##### 934. 最短的桥

先深度搜索，确度一座岛的边界,再广度搜索，查找路径。 DFS + 多源BFS最短路径。

```C++
class Solution {
    vector<vector<int>> g;
    vector<vector<int>> dist;
    queue<pair<int, int>> q;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    int m, n;
public:
    int shortestBridge(vector<vector<int>> &grid) {
        g = grid;
        m = g.size(), n = g[0].size();
        dist = vector<vector<int>>(m, (vector<int>(n, 1e4)));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (g[i][j] == 1) {
                    dfs(i, j);
                    return bfs();
                }
            }
        }
        return 0;
    }

    void dfs(int i, int j) {

        dist[i][j] = 0;
        q.emplace(make_pair(i, j));
        for (int k = 0; k < 4; k++) {
            int x = i + dx[k], y = j + dy[k];
            if (x >= 0 && x < m && y >= 0 && y < n && g[x][y] && dist[x][y]) {
                dfs(x, y);
            }
        }
    }

    int bfs() {
        while (q.size()) {
            auto top = q.front();
            q.pop();
            int i = top.first, j = top.second;
            for (int k = 0; k < 4; k++) {
                int x = i + dx[k], y = j + dy[k];
                if (x >= 0 && x < m && y >= 0 && y < n && dist[x][y] > dist[i][j] + 1) {
                    dist[x][y] = dist[i][j] + 1;
                    if (g[x][y]) {
                        return dist[i][j];
                    }
                    q.emplace(make_pair(x, y));
                }
            }
        }
        return 0;
    }
};

```

						      

## 0x30 数学知识
	
##### Leetcode 50. Pow(x, n)

快速幂模板

```C++
class Solution {
public:
    double myPow(double x, int n) {
        double res = 1.0;
        long long t = n;
        if (t < 0) {
            t = -t;
        }
        while (t) {
            if (t & 1) res *= x;
            t >>= 1;
            x *= x;
        }
        return n < 0 ? 1 / res : res;
    }
};
```
	
##### 60. 排列序列

康托展开是一个全排列到一个自然数的双射，常用于构建哈希表时的空间压缩。 康托展开的实质是计算当前排列在所有由小到大全排列中的顺序，因此是可逆的。  
康托展开的逆运算：可以通过康托展开值求出原排列，即可以求出n的全排列中第x大排列。 

X=a[n]\*(n-1)!+a[n-1]\*(n-2)!+...+a[i]\*(i-1)!+...+a[1]\*0!

康托展开和逆康托展开： https://blog.csdn.net/wbin233/article/details/72998375

对于n个数来说，每个组有(n-1)!个全排列，n-1个数，每个组就有(n-2)!个全排列。   
对于特定数字，k/(n-1)!可以得到当前数字的下标，剩下的n-1个数，则用k%(n-1)来计算，只到n个0，得到了第k个。  

```C++
class Solution {
public:
    string getPermutation(int n, int k) {
        string res;
        string tmp = "";
        for (int i = 1; i <= n; i++) {
            tmp += to_string(i);
        }
        vector<int> fac(n, 0);
        fac[0] = 1;
        for (int i = 1; i < n; i++) {
            fac[i] = fac[i - 1] * i;
        }
        k--;

        for (int i = n; i > 0; i--) {
            int idx = k / fac[i - 1];
            k %= fac[i - 1];
            res.push_back(tmp[idx]);
            tmp.erase(tmp.begin() + idx);
        }
        return res;
    }
};

```


	

#### 169 多数元素

投票法
	
```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {

        //投票法
        int count = 1;
        int conda = nums[0];
        for(int i = 1;i < nums.size();i++){
            if(count == 0){
                conda = nums[i];
            }
            if(conda == nums[i]){
                count++;
            } else{
                count--;
            }
        }
        return conda;
    }
};

```


##### 268 丢失的数字

```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int sum = 0;
        int n = nums.size();
        for(int i = 0;i < nums.size();i++){
            sum += nums[i];
        }
        int total = n * (n + 1)/2;
        return total - sum;
    }
};
```
##### 462. 最少移动次数使数组元素相等 II

```C++
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        int n = nums.size();
        nth_element(nums.begin(),nums.begin() + n/2,nums.end());
        int k = nums[n/2];
        int sum = 0;
        for(auto num:nums){
            sum += abs(num - k);
        }
        return sum;
    }
};
```

##### 458. 可怜的小猪

```C++
class Solution {
public:
    int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int k = minutesToTest/minutesToDie + 1;
        int res = 0,t = 1;
        while(t < buckets) t *= k,res++;
        return res;
    }
};
```

##### 319 灯泡开关
```C++
class Solution {
public:
    int bulbSwitch(int n) {
        return sqrt(n);
    }
};
```

##### 223. 矩形面积

```C++
class Solution {
public:
    int computeArea(int ax1, int ay1, int ax2, int ay2, int bx1, int by1, int bx2, int by2) {
        long long x = min(ax2,bx2) + 0ll - max(ax1,bx1);
        long long y = min(ay2,by2) + 0ll - max(ay1,by1);
        return (ax2 - ax1) *(ay2 - ay1) + (bx2 - bx1) *(by2 - by1)  - max(0ll,x)*max(0ll,y);
    }
};
```


## 0x41  数据结构进阶 - 并查集
	
##### 547. 省份数量

```cpp
class Solution {
    vector<int> parent;
    //寻根操作根
    int find(int i){
        if(parent[i] != i) parent[i] = find(parent[i]);
        return parent[i];
    }

public:
    int findCircleNum(vector<vector<int>> &isConnected) {
        int m = isConnected.size();
        for(int i = 0;i < m;i++) parent.emplace_back(i);

        for(int i = 0;i < m;i++){
            for(int j = i + 1;j < m;j++){
                if(isConnected[i][j]){
                    //union
                    parent[find(i)] = find(j);
                }
            }
        }
        int res = 0;
        for(int i = 0;i < m;i++){
            if(parent[i] == i) res++;
        }
        return res;
    }
};

```
##### 684. 冗余连接

```c++
class Solution {
    vector<int> parent;
    int find(int i){
        if(parent[i] != i) parent[i] = find(parent[i]);
        return parent[i];
    }

public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        //1->n,从1开始的
        for(int i = 0;i <= n;i++){
            parent.emplace_back(i);
        }
        for(auto edge:edges){
            auto a = edge[0],b = edge[1];
            if(find(a) == find(b)) return {a,b};
            parent[find(a)] = find(b);
        }
        return {-1,-1};
    }
};
```

### 0x42 树状数组


#####  307. 区域和检索 - 数组可修改

reserve函数用来给vector预分配存储区大小，即capacity的值 ，但是没有给这段内存进行初始化。

resize函数重新分配大小，改变容器的大小，并且创建对象。

```C++
class NumArray {
    int n;
    vector<int> tree;
    vector<int> nums;
public:
    NumArray(vector<int>& _nums) {
        nums = _nums;
        n = nums.size();
        tree.resize(n + 1);
        for(int i = 0;i < nums.size();i++){
            add(i + 1,nums[i]);
        }
    }

    int lowbit(int x){
        return x & -x;
    }
    int query(int x){
        int res = 0;
        for(int i = x;i;i -= lowbit(i)){
            res += tree[i];
        }
        return res;
    }
    
    void add(int index, int val) {
        for(int i = index;i < n + 1;i += lowbit(i)){
            tree[i] += val;
        }
    }

    void update(int index, int val) {
        add(index + 1,val - nums[index]);
        nums[index] = val;
    }
    
    int sumRange(int left, int right) {
        return query(right + 1) - query(left);
    }
};
```

##### LeetCode 315. 计算右侧小于当前元素的个数

```
class Solution {
    int N = 20001;
    vector<int> tr;
public:

    int lowbit(int x){
        return x & -x;
    }
    void add(int x,int val){
        for(int i = x;i <= N;i += lowbit(i)){
            tr[i] += val;
        }
    }
    int query(int x){
        int res = 0;
        //索引从1开始
        for(int i = x;i;i -= lowbit(i)){
            res += tr[i];
        }
        return res;
    }

    vector<int> countSmaller(vector<int>& nums) {
        tr.resize(N + 1);
        vector<int> res(nums.size());
        for(int i = nums.size() - 1;i >= 0;i--){
            int tmp = nums[i] + 10001;
            res[i] = query(tmp - 1);
            add(tmp,1);
        }
        return res;
    }
};
```	

## 0x50 动态规划

线性DP、数字三角形模型、子序列LIS问题、状态机模型、背包问题、区间dp、状态压缩dp

### 线性DP(经典dp)

字符串（编辑距离、正则）问题、

##### 10 正则表达式匹配

vector<vector<int>> 定义bool类型

*忽略p一个（2个）或者忽略s一个。

```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(),n = p.size();
        s = " " + s;
        p = " " + p;
        vector<vector<int>> f(m + 1,vector<int>(n+1,0));
        f[0][0] = true;
        for(int j = 2;j < n + 1;j++){
            if(p[j] == '*'){
                f[0][j] = f[0][j - 2];
            }
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                //下个为*,跳过当前的。
                if (j + 1 <= m && p[j + 1] == '*') continue;
                if (p[j] != '*') {
                    if (p[j] == '.' || s[i] == p[j]) {
                        f[i][j] |= f[i - 1][j - 1];
                    }
                } else if (p[j] == '*') {
                    if (j >= 2) f[i][j] |= f[i][j - 2];
                    if (p[j - 1] == '.' || s[i] == p[j - 1]) {
                        f[i][j] |= f[i - 1][j];
                    }
                }
            }
        }
        return f[m][n];
    }
};
```
##### Leetcode 44  通配符匹配

判* 当前

dp[i][j] = dp[i - 1][j] | dp[i][j - 1];

```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        s = " " + s;
        p = " " + p;
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        dp[0][0] = 1;
        for (int i = 1; i < n + 1; i++) {
            if (p[i] == '*') dp[0][i] |= dp[0][i - 1];
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (s[i] == p[j] || p[j] == '?') {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (p[j] == '*') {
                    dp[i][j] = dp[i - 1][j] | dp[i][j - 1];
                }
            }
        }

        return dp[m][n];
    }
};
```
##### Leetcode 97 交错字符串

字符串dp

```C++
class Solution {

public:
    bool isInterleave(string s1, string s2, string s3) {
        if (s1.size() + s2.size() != s3.size()) {
            return false;
        }
        int m = s1.size(), n = s2.size();
        vector<vector<int>> f(m + 1, vector<int>(n + 1, 0));
        s1 = " " + s1, s2 = " " + s2, s3 = " " + s3;
        f[0][0] = true;
        for (int i = 1; i < m + 1; i++) {
            if (s1[i] == s3[i]) f[i][0] = f[i][0] || f[i - 1][0];
        }
        for (int j = 1; j < n + 1; j++) {
            if (s2[j] == s3[j]) f[0][j] = f[0][j] || f[0][j - 1];
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (s1[i] == s3[i + j]) f[i][j] = f[i][j] || f[i - 1][j];
                if (s2[j] == s3[i + j]) f[i][j] = f[i][j] || f[i][j - 1];
            }
        }
        return f[m][n];
    }
};

```
	
##### 72 编辑距离

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        //简化操作: A插入、B 插入、A替换
        //dp[i][j]: A的前i和B的前j个
        for (int i = 0; i < m + 1; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j < n + 1; j++) {
            dp[0][j] = j;
        }

        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = min(dp[i - 1][j] + 1, min(dp[i][j - 1] + 1, dp[i - 1][j - 1]));
                } else {
                    dp[i][j] = min(dp[i - 1][j] + 1, min(dp[i][j - 1] + 1, dp[i - 1][j - 1] + 1));
                }
            }
        }
        return dp[m][n];
    }
};

```

##### 53. 最大子序和

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        int dp = 0;
        int maxRes = nums[0];
        for(auto &num:nums){
            dp = max(dp + num,num);
            maxRes = max(maxRes,dp);
        }
        return maxRes;
    }
};
```

##### 70 爬楼梯

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n == 0){
            return 1;
        }
        if(n == 1){
            return 1;
        }
        vector<int> dp(n + 1,0);
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2;i < n+1;i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];

    }
};
```

##### 62. 不同路径

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m,vector<int> (n,0));
        for(int i = 0;i < m;i++){
            dp[i][0] = 1;
        }
        for(int j = 0;j < n;j++){
            dp[0][j] = 1;
        }
        for(int i = 1;i < m ;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = dp[i - 1][j] + dp[i][j-1];
            }
        }
        
        return  dp[m-1][n-1];
    }
};
```
##### 63 不同路径 II
```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obs) {
        int m = obs.size(),n = obs[0].size();
        vector<vector<int>> f(m,vector<int>(n,0));
        f[0][0] = !obs[0][0];
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                if(obs[i][j]) continue;
                if(i) f[i][j] += f[i - 1][j];
                if(j) f[i][j] += f[i][j - 1];
            }
        }
        return f[m - 1][n - 1];
    }
};
```

##### 64. 最小路径和

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.size() == 0 || grid[0].size() == 0){
            return 0;
        }
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp(m,vector<int> (n,0));
        dp[0][0] = grid[0][0];
        for(int i = 1;i < m;i++){
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }
        for(int j = 1;j < n;j++){
            dp[0][j] = dp[0][j - 1] + grid[0][j];
        }
        for(int i = 1;i < m ;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = min(dp[i - 1][j],dp[i][j-1]) + grid[i][j];
            }
        }

        return  dp[m-1][n-1];
    }
};
```


##### Leetcode 139 单词拆分

单词问题：双指针dp

双指针正向dp解法

双指针逆向dp解法

```C++
class Solution {
    unordered_set<string> hash;
    vector<int> f;
    int n;
public:
    bool wordBreak(string s, vector<string> &wordDict) {
        for (auto &word:wordDict) {
            hash.insert(word);
        }
        n = s.size();
        //f[i]对应s的i-1
        f.resize(n + 1, 0);
        f[n] = true;
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (f[j + 1] && hash.count(s.substr(i, j - i + 1))) {
                    f[i] = 1;
                }
            }
        }

        return f[0];
    }
};
```
##### Leetcode 140 单词拆分 II

dp方向和dfs方向相反，反向递推方案

```C++
class Solution {
    unordered_set<string> hash;
    vector<string> ans;
    vector<int> f;
    int n;
public:
    vector<string> wordBreak(string s, vector<string> &wordDict) {
        for (auto &word:wordDict) {
            hash.insert(word);
        }
        n = s.size();
        //f[i]对应s的i-1
        f.resize(n + 1, 0);
        f[n] = true;
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (f[j + 1] && hash.count(s.substr(i, j - i + 1))) {
                    f[i] = 1;
                }
            }
        }

        //dfs方向与dp相反，反向递推
        if (f[0]) dfs(s, 0, "");
        return ans;
    }

    //u是len,i是索引
    void dfs(string s, int u, string path) {
        if (u == n) {
            path.pop_back();
            ans.emplace_back(path);
        }
        for (int i = u; i < n; i++) {
            if (f[i + 1] && hash.count(s.substr(u, i - u + 1))) {
                dfs(s, i + 1, path + s.substr(u, i - u + 1) + " ");
            }
        }
    }
};

```


##### 91 解码方法
```
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        vector<int> f(n + 1, 0);
        f[0] = 1;
        for (int i = 1; i < n + 1; i++) {
            if (s[i - 1] != '0') f[i] += f[i - 1];
            if (i >= 2) {
                int num = s[i - 1] - '0' + 10 * (s[i - 2] - '0');
                if (num >= 10 && num <= 26) f[i] += f[i - 2];
            }
        }
        return f[n];
    }
};
```

##### LeetCode 115. 不同的子序列

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int m = s.size(), n = t.size();
        if(m < n) return 0;
        vector<vector<unsigned long long>> f(m + 1, vector<unsigned long long>(n + 1, 0));
        for (int i = 0; i < m + 1; i++) {
            f[i][0] = 1;
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                f[i][j] = f[i - 1][j];
                if (s[i - 1] == t[j - 1]) {
                    f[i][j] += f[i - 1][j - 1];
                }
            }
        }

        return f[m][n];
    }
};
```    

##### Leetcode 131. 分割回文串

```C++
class Solution {
    vector<vector<string>> ans;
    vector<string> path;
    vector<vector<int>> f;
public:
    vector<vector<string>> partition(string s) {
        int n = s.size();
        f = vector<vector<int>>(n, vector<int>(n, 1));
        for (int i = n - 1; i >= 0; i--) { //注意循环顺序
            for (int j = i; j < n; j++) {
                if (i == j) f[i][j] = 1;
                else f[i][j] = (s[i] == s[j]) && f[i + 1][j - 1];
            }
        }
        dfs(s, 0);
        return ans;
    }

    void dfs(string s, int u) {
        if (u == s.size()) {
            ans.emplace_back(path);
            return;
        }

        for (int i = u; i < s.size(); i++) {
            if (f[u][i]) {
                path.emplace_back(s.substr(u, i - u + 1));
                dfs(s, i + 1);
                path.pop_back();
            }
        }
    }
};

```

##### 132. 分割回文串 II

 回文dp + LIS DP
                                 
```cpp    
class Solution {
public:
    int minCut(string s) {
        int m = s.size();
        s = " " + s;
        vector<vector<int>> g(m + 1,vector<int>(m + 1, false));
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j <= i; j++) {
                if (i == j) g[j][i] = true;
                else if (s[i] == s[j]) {
                    //j + 1 > i - 1 表示i和j之间间隔一个字母
                    if (j + 1 > i - 1 || g[j + 1][i - 1]) {
                        g[j][i] = true;
                    }
                }
            }
        }

        vector<int> f(m + 1,INT_MAX);
        f[0] = 0;
        //LIS结构
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j <= i; j++) {
                if (g[j][i]) {
                    f[i] = min(f[i],f[j - 1] + 1);
                }
            }
        }
        return f[m] - 1;
    }
};    
```   




### 数字三角形模型
	
##### Leetcode 118  杨辉三角 I

```C+++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> res;
        for (int i = 0; i < numRows; i++) {
            vector<int> line(i + 1, 0);
            line[0] = line[i] = 1;
            for (int j = 1; j < i; j++) {
                line[j] = res[i - 1][j - 1] + res[i - 1][j];
            }
            res.emplace_back(line);
        }
        return res;
    }
};

```

##### Leetcode 119  杨辉三角 II

```C++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<vector<int>> f(2, vector<int>(rowIndex + 1, 0));
        for (int i = 0; i <= rowIndex; i++) {
            f[i & 1][0] = f[i & 1][i] = 1;
            for (int j = 1; j < i; j++) {
                f[i & 1][j] = f[i - 1 & 1][j - 1] + f[i - 1 & 1][j];
            }
        }
        return f[rowIndex & 1];
    }
};

```

##### 120. 三角形最小路径和
```c++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<int>> f(n,vector<int>(n,0));
        f[0][0] = triangle[0][0];
        for (int i = 1; i < n; i++) {
            f[i][0] = f[i - 1][0] + triangle[i][0];
            for (int j = 1; j <= i; j++) {
                f[i][j] = min(f[i - 1][j], f[i - 1][j - 1]) + triangle[i][j];
            }
            f[i][i] = f[i - 1][i - 1] + triangle[i][i];
        }
        int res = INT_MAX;
        for(int i = 0;i < n;i++){
            res = min(res,f[n - 1][i]);
        }
        return res;
    }
};
```

##### 221. 最大正方形

```C++
class Solution {
public:
    int maximalSquare(vector<vector<char>> &matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));

        //求边长
        int maxSide = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '1') {
                    //左上边界好填，直接填上
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = min(dp[i - 1][j], min(dp[i][j - 1], dp[i - 1][j - 1])) + 1;
                    }
                }
                maxSide = max(maxSide, dp[i][j]);
            }
        }
        return maxSide * maxSide;
    }
};

```	


				   
### 子序列(LIS模型)

##### 128 最长连续序列

unordered_set 使用

for(auto num:nums) --> for(const int &num:nums) 迭代

```c++
class Solution {
public:
    int longestConsecutive(vector<int> &nums) {

        unordered_set<int> set;
        int max_len = 0;
        for (const int &num:nums) {
            set.emplace(num);
        }
        for (const int &num:nums) {
            if (set.count(num - 1)) {
                continue;
            }
            int num_len = 1;
            int current_num = num;
            current_num += 1;
            while (set.count(current_num)) {
                current_num += 1;
                num_len += 1;
            }
            max_len = max(max_len, num_len);
        }
        return max_len;
    }
};
```
	
##### 300. 最长递增子序列

```cpp
//双循环dp
class Solution {
public:
    int lengthOfLIS(vector<int> &nums) {
        int n = nums.size();
        vector<int> f(n,0);
        int res = 0;
        for(int i = 0;i < n;i++){
            f[i] = 1;
            for(int j = 0;j < i;j++){
                if(nums[i] > nums[j]){
                    f[i] = max(f[i],f[j] + 1);
                }
            }
            res = max(res,f[i]);
        }
        return res;
    }
};

//贪心+ 二分查找
class Solution {
public:
    int lengthOfLIS(vector<int> &nums) {
        int n = nums.size();
        vector<int> f = {nums[0]};
        int res = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] > f.back()) {
                f.emplace_back(nums[i]);
            } else {
                int l = 0, r = f.size();
                while (l < r) {
                    int mid = l + r >> 1;
                    if (f[mid] >= nums[i]) r = mid;
                    else l = mid + 1;
                }
                f[l] = nums[i];
            }
        }
        return f.size();
    }
};

```
	
##### 1143. 最长公共子序列

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size();
        int n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j],dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
};

```
##### 354. 俄罗斯套娃信封问题

二维排序 + 一维双循环LIS/二分查找LIS

```cpp
//二维排序，一维双循环dp,超时
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        if (a[0] < b[0]) return true;
        else if (a[0] == b[0] && a[1] > b[1]) return true;
        return false;
    }

public:
    int maxEnvelopes(vector<vector<int>> &envelopes) {
        sort(envelopes.begin(), envelopes.end(), cmp);
        int n = envelopes.size();
        vector<int> f(n, 1);
        int res = 0;
        for (int i = 0; i < envelopes.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (envelopes[i][1] > envelopes[j][1]) {
                    f[i] = max(f[i], f[j] + 1);
                }
                res = max(res, f[i]);
            }
        }
        return res;
    }
};

//贪心+二分查找(LIS)
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        if (a[0] < b[0]) return true;
        else if (a[0] == b[0] && a[1] > b[1]) return true;
        return false;
    }

public:
    int maxEnvelopes(vector<vector<int>> &envelopes) {
        sort(envelopes.begin(), envelopes.end(), cmp);
        int n = envelopes.size();
        vector<int> f = {envelopes[0][1]};
        for (int i = 1; i < envelopes.size(); i++) {
            if (envelopes[i][1] > f.back()) {
                f.emplace_back(envelopes[i][1]);
            } else {
                auto it = lower_bound(f.begin(), f.end(), envelopes[i][1]);
                *it = envelopes[i][1];
            }
        }
        return f.size();
    }
};

```  

### 状态机问题(股票问题、打家劫舍问题)
 
股票问题：贪心法、dp状态机法(carl一维数组-->buy(0) + sell(1)简化，carl二维数组--> buy + sell数组简化)

买入股票的状态不代表当天一定买入股票，是一种可能的状态

LeetCode买卖股票问题——模版总结(简化dp)：https://www.acwing.com/blog/content/526/

代码随想录股票问题(二维dp)：https://programmercarl.com


##### LeetCode 121. 买卖股票的最佳时机

```C++
//贪心
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int res = 0, minPrice = INT_MAX;
        for (auto &price:prices) {
            res = max(res, price - minPrice);
            minPrice = min(price, minPrice);
        }
        return res;
    }
};
//一维dp -->简化buy sell
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int n = prices.size();
        int buy = -prices[0], sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            buy = max(buy, -prices[i]);
            sell = max(sell, buy + prices[i]);
        }
        return sell;
    }
};
```

##### LeetCode 122. 买卖股票的最佳时机 II

```C++
//贪心
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int res = 0;
        for (int i = 0; i < prices.size() - 1; i++) {
            if (prices[i + 1] > prices[i]) {
                res += prices[i + 1] - prices[i];
            }
        }
        return res;
    }
};
//dp
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int n = prices.size();
        int buy = -prices[0], sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            buy = max(buy, sell - prices[i]);
            sell = max(sell, buy + prices[i]);
        }
        return sell;
    }
};
```

##### LeetCode 123. 买卖股票的最佳时机 III

一天一共就有五个状态，

没有操作、第一次买入、第一次卖出、第二次买入、第二次卖出

```C++
//dp
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int buy1 = -prices[0], sell1 = 0;
        int buy2 = -prices[0], sell2 = 0;
        //先买才有卖
        for (int i = 1; i < prices.size(); i++) {
            buy1 = max(buy1, -prices[i]);
            sell1 = max(sell1, buy1 + prices[i]);
            buy2 = max(buy2, sell1 - prices[i]);
            sell2 = max(sell2, buy2 + prices[i]);
        }
        return sell2;
    }
};
```

##### LeetCode 188. Best Time to Buy and Sell Stock IV

vector<vector<int>> dp(prices.size(), vector<int>(2 * k + 1, 0)); 二维压缩成一维---> dp[2*k + 1]

```C++
class Solution {

public:
    int maxProfit(int k, vector<int> &prices) {
        if(prices.empty()) return 0;
        if (k > prices.size()) {
            int res;
            //无限交易
            for (int i = 0; i < prices.size() - 1; i++) {
                if (prices[i] < prices[i + 1]) {
                    res += prices[i + 1] - prices[i];
                }
            }
            return res;
        }

        vector<int> buy(k + 1, INT_MIN);
        vector<int> sell(k + 1, 0);
        //状态机模型
        //先遍历价格，再遍历决策
        for (int i = 0; i < prices.size(); i++) {
            for (int j = 1; j < k + 1; j++) {
                buy[j] = max(buy[j], sell[j - 1] - prices[i]);
                sell[j] = max(sell[j], buy[j] + prices[i]);
            }
        }
        return sell[k];
    }
};
```
##### LeetCode 309. 最佳买卖股票时机含冷冻期

```C++
//dp
class Solution {

public:
    int maxProfit(vector<int> &prices) {
        if (prices.empty()) return 0;
        int n = prices.size();
        vector<vector<int>> f(n, vector<int>(3, 0));
        //0 持有，1不持有且冷冻，2 不持有且不冷冻
        f[0][0] = -prices[0];
        for (int i = 1; i < prices.size(); i++) {
            f[i][0] = max(f[i - 1][0], f[i - 1][2] - prices[i]);
            f[i][1] = f[i - 1][0] + prices[i];
            f[i][2] = max(f[i - 1][2], f[i - 1][1]);
        }
        return max(f[n - 1][1], f[n - 1][2]);
    }
};
//简化
class Solution {

public:
    int maxProfit(vector<int> &prices) {
        if (prices.empty()) return 0;
        int n = prices.size();
        //0 持有，1不持有且冷冻，2 不持有且不冷冻
        int buy = -prices[0], cool = 0, sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            int new_buy = max(buy, sell - prices[i]);
            int new_cool = buy + prices[i];
            int new_sell = max(sell, cool);
            buy = new_buy;
            cool = new_cool;
            sell = new_sell;
        }
        return max(sell, cool);
    }
};
```

##### 714. 买卖股票的最佳时机含手续费

```C++
class Solution {
public:
    int maxProfit(vector<int> &prices, int fee) {
        if (prices.empty()) return 0;
        int buy = -prices[0], sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            buy = max(buy, sell - prices[i]);
            sell = max(sell, buy + prices[i] - fee);
        }
        return sell;
    }
};
```


##### 213. 打家劫舍 II
                                          
```C++
1. 选头不选尾 2. 选尾不选头 3. 头尾都不选(包含在1、2中), 双向dp (left->right, right->left)

//双向(left、right)dp:头尾问题

class Solution {
public:
    int rob(vector<int> &nums) {
        int n = nums.size();
        if (!n) return 0;
        if (n == 1) return nums[0];
        //f抢，g不抢
        vector<int> f(n + 1, 0), g(n + 1, 0);

        //选头不选尾
        f[1] = nums[0],g[1] = INT_MIN;
        for (int i = 2; i < n + 1; i++) {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        int res = g[n];

        //选尾不选头
        f = vector<int>(n + 1, 0);
        g = vector<int>(n + 1, 0);
        for (int i = 2; i < n + 1; i++) {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        res = max(f[n],max(res, g[n]));

        return res;
    }
};                                          
                                          
```             


##### LeetCode 264. Ugly Number II

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> num;
        num.emplace_back(1);
        int a = 0, b = 0, c = 0;
        for (int i = 2; i < n + 1; i++) {
            int two = num[a] * 2, three = num[b] * 3, five = num[c] * 5;
            int next = min(two, min(three, five));
            if (next == two) a++;
            if (next == three) b++;
            if (next == five) c++;
            num.emplace_back(next);
        }
        return num.back();
    }
};
```

##### 152 乘积最大子数组

```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        vector<int> dpMax(n,0);
        vector<int> dpMin(n,0);
        dpMax[0] = nums[0];
        dpMin[0] = nums[0];
        int ans = nums[0];
        for(int i = 1;i < n;i++){
            dpMax[i] = max(nums[i],max(dpMax[i-1]*nums[i],dpMin[i-1]*nums[i]));
            dpMin[i] = min(nums[i],min(dpMax[i-1]*nums[i],dpMin[i-1]*nums[i]));
            ans = max(ans,dpMax[i]);
        }
        return  ans;
    }
};
```

    
##### LeetCode 576. 出界的路径数

三维dp    
static constexpr int MOD = 1e9 + 7;
 
```cpp
class Solution {
public:
    int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        if (!maxMove) return 0;
        static constexpr int MOD = 1e9 + 7;
        //f[i][j][k]:移动k次到达i和j的路径总数
        vector<vector<vector<int>>> f(m, vector<vector<int>>(n, vector<int>(maxMove + 1, 0)));

        f[startRow][startColumn][0] = 1;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        int res = 0;
        for (int k = 0; k < maxMove; k++) {
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    int count = f[i][j][k];
                    if (count > 0) {
                        for (int d = 0; d < 4; d++) {
                            int newX = i + dx[d], newY = j + dy[d];
                            if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                                f[newX][newY][k + 1] = (f[newX][newY][k + 1] + count) % MOD;
                            } else {
                                res = (res + count) % MOD;
                            }
                        }
                    }
                }
            }
        }
        return res;
    }
};
    
```    

### 背包问题
	
dp[i][j]含义:i是前i个物品(硬币)代表选择, j是限制容量（目标金额）代表限制条件,dp[i][j]最大价值或方法数

0-1背包: 最大价值, 变形子集背包

完全(无限)背包：零钱问题

0-1背包模板(最值)，部分可以简化为一维的

```C++
for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
return dp[N][W]
```

子集背包模板(能否)：

```C++
 #装入或者不装入
 dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]]
```

完全背包模板(方法数)

https://labuladong.gitee.io/algo/3/25/81/

```C++
for i in range(n + 1):
    for j in range(amount + 1):
        if j - coins[i-1] >= 0：
            dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]] #这里表示i可以反复使用
        else:
            dp[i][j] = dp[i - 1][j]
```

**0-1背包**

##### 416. 分割等和子集

dp[i][j]含义

i是前i个物品(硬币)代表选择, j是限制容量（目标金额）代表限制条件,dp[i][j]最大价值或方法数

0-1背包: 最大价值, 变形子集背包

完全(无限)背包：零钱问题

```cpp
class Solution {
public:
    bool canPartition(vector<int> &nums) {
        int m = nums.size();
        if (m < 2) {
            return false;
        }
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum & 1) {
            return false;
        }
        int target = sum / 2;
        int maxNum = *max_element(nums.begin(), nums.end());
        if (maxNum > target) {
            return false;
        }
        vector<vector<int>> dp(m + 1, vector<int>(target + 1, 0));
        for (int i = 1; i < m + 1; i++) {
            dp[i][0] = true;
        }
        dp[0][0] = true;
        dp[1][nums[0]] = true;
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < target + 1; j++) {
                if (j < nums[i - 1]) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j] | dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        return dp[m][target];
    }
};
```
	
##### 494. 目标和
    
 Pos - Neg = target,Pos + Neg = sum，二元一次方程

 求子集和为Pos或Neg

 方案数，需要将前面各种选择的相加

 特殊情况，[i][0]特殊值,num[i-1]选择可以为0，不是前面都不选
 
```cpp
class Solution {
public:
    int findTargetSumWays(vector<int> &nums, int target) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        int diff = sum - target;
        if (diff < 0 || diff & 1) {
            return 0;
        }
        int m = nums.size();
        int neg = diff / 2;
        vector<vector<int>> dp(m + 1, vector<int>(neg + 1, 0));
        dp[0][0] = 1;
        for (int i = 1; i < m + 1; i++) {
            for (int j = 0; j < neg + 1; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j >= nums[i - 1]) {
                    dp[i][j] += dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        return dp[m][neg];
    }
};
```   

**完全背包**
	
##### 279 完全平方数

完全背包
    
```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> f(n + 1, INT_MAX);
        f[0] = 0;
        for (int i = 0; i < n + 1; i++) {
            for (int j = 1; j * j <= i; j++) {
                f[i] = min(f[i], f[i - j * j] + 1);
            }
        }
        return f[n];
    }
};
```

##### 322 零钱兑换

```C++
class Solution {
public:
    int coinChange(vector<int> &coins, int amount) {
        vector<int> f(amount + 1, amount + 1);
        f[0] = 0;
        for(int i = 0;i < coins.size();i++){
            for( int j = coins[i]; j < amount + 1;j++){
                f[j] = min(f[j],f[j - coins[i]]  + 1);
            }
        }

        return f[amount] != amount + 1? f[amount]:-1;
    }
};
```	
##### 518. 零钱兑换 II

```c++
class Solution {
public:
    int change(int amount, vector<int> &coins) {
        int n = coins.size();
        vector<int> f(amount + 1, 0);
        f[0] = 1;

        for (int i = 1; i <= n; i++) {
            for (int j = coins[i - 1]; j < amount + 1; j++) {
                f[j] += f[j - coins[i - 1]];
            }
        }
        return f[amount];
    }
};

```
	
				   
### 区间dp

                             
##### LeetCode 312. 戳气球
                             
经典区间dp 

状态转移所依赖的状态必须被提前计算出来，需要根据 base case 和最终状态进行推导，合理安排i,j的遍历顺序                          
                             
```C++
class Solution {
public:
    int maxCoins(vector<int> &nums) {
        int n = nums.size();
        vector<int> p(n + 2, 0);
        p[0] = 1, p[n + 1] = 1;
        for (int i = 1; i < n + 1; i++) {
            p[i] = nums[i - 1];
        }
        //区间dp
        vector<vector<int>> f(n + 2, vector<int>(n + 2, 0));
        for (int len = 3; len <= n + 2; len++) {
            for (int l = 0; l + len - 1 < n + 2; l++) {
                int r = l + len - 1;
                for (int k = l + 1; k < r; k++) {
                    f[l][r] = max(f[l][r], f[l][k] + f[k][r] + p[l] * p[k] * p[r]);
                }
            }
        }
        return f[0][n + 1];
    }
};

```                

##### 486 预测赢家
                        
区间dp
                        
```C++
class Solution {
public:
    bool PredictTheWinner(vector<int> &nums) {
        int m = nums.size();
        vector<vector<int>> f(m, vector<int>(m, 0));
        for (int len = 1; len <= m; len++) {
            for (int l = 0; l + len - 1 < m; l++) {
                int r = l + len - 1;
                if (len == 1) f[l][r] = nums[l];
                else {
                    f[l][r] = max(nums[l] - f[l + 1][r], nums[r] - f[l][r - 1]);
                }
            }
        }
        return f[0][m - 1] >= 0;
    }
};                        
```                        

##### 664 奇怪的打印机

```c++
class Solution {
public:
    int strangePrinter(string s) {
        int n = s.size();
        vector<vector<int>> f(n + 2, vector<int>(n + 2, 0));
        for (int len = 1; len <= n; len++) {
            for (int l = 1; l + len - 1 <= n; l++) {
                int r = l + len - 1;
                f[l][r] = f[l + 1][r] + 1;
                for (int k = l + 1; k <= r; k++) {
                    if (s[l - 1] == s[k - 1]) {
                        f[l][r] = min(f[l][r], f[l][k - 1] + f[k + 1][r]);
                    }
                }
            }
        }
        return f[1][n];
    }
};

```
				   
##### 1000. 合并石头的最低成本

```c++
class Solution {
public:
    int mergeStones(vector<int> &stones, int m) {
        int n = stones.size();
        if ((n - 1) % (m - 1)) return -1;
        vector<int> sum(n + 1, 0);
        vector<vector<int>> f(n + 1, vector<int>(n + 1, INT_MAX));
        for (int i = 1; i < n + 1; i++) {
            f[i][i] = 0;
            sum[i] = sum[i - 1] + stones[i-1];
        }
        for (int len = 2; len < n + 1; len++) {
            for (int l = 1; l + len - 1 < n + 1; l++) {
                int r = l + len - 1;
                for (int k = r - 1; k >= l; k -= m - 1) {
                    f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r]);
                }
                if((len-1)%(m-1) == 0)
                    f[l][r] += sum[r] - sum[l - 1];
            }
        }
        return f[1][n];
    }
};
```

### 状态压缩dp


##### 464. 我能赢吗

状态压缩dfs

```C++
class Solution {
    vector<int> dp;
    int maxn = 0;
public:
    bool canIWin(int maxChoosableInteger, int desiredTotal) {
        if(!desiredTotal) return true;
        if((maxChoosableInteger * (maxChoosableInteger + 1))/2 < desiredTotal) return false;

        dp = vector<int> (1<<(maxChoosableInteger + 1),-1);
        maxn = maxChoosableInteger;
        return dfs(0,desiredTotal);
    }

    bool dfs(int status,int t){
        if(dp[status] != -1) return dp[status];
        if(t <= 0) return dp[status] = false;//赋值并返回false;
        for(int i = 1;i <= maxn;i++){
            if(!(status & (1 << i))){
                if(!dfs(status + (1 << i),t - i)){
                    return dp[status] = true;
                }
            }
        }
        return dp[status] = false;
    }
};
```


##### 526. 优美的排列

状态压缩dp,f[i]代表选法是i的时候的方案数，i的每一位代表是否选择当前的数。    
__builtin_popcount 计算32位的无符号整数有多少个1。
                                                                              
```c++
class Solution {
public:
    int countArrangement(int n) {
        vector<int> f(1 << n, 0);
        f[0] = 1;
        //状态压缩dp,f[i]代表选法是i的时候的方案数，i的每一位代表是否选择当前的数
        for (int mask = 0; mask < 1 << n; mask++) {
            int num = 1;
            for (int j = 0; j < n; j++) {
                if (mask & (1 << j)) num++;
            }
            for (int k = 0; k < n; k++) {
                if (!(mask & (1 << k)) && ((num % (k + 1) == 0) || ((k + 1) % num == 0))) {
                    f[mask | (1 << k)] += f[mask];
                }
            }
        }

        return f[(1 << n) - 1];
    }
};                                                                            
```                                                                               
                                                                             



## 0x60 图论

### 图遍历--拓扑排序

##### Leetcode 207 课程表

拓扑排序

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>> &prerequisites) {
        //inDegree表 + 邻接表(边)
        vector<vector<int>> edges(numCourses);
        vector<int> inDeg(numCourses, 0);
        for (auto &edge:prerequisites) {
            edges[edge[1]].emplace_back(edge[0]);
            inDeg[edge[0]]++;
        }

        queue<int> q;
        for (int i = 0; i < numCourses; i++) {
            if (inDeg[i] == 0) q.emplace(i);
        }
        int cnt = 0;
        while (q.size()) {
            auto top = q.front();
            q.pop();
            cnt++;
            for (auto ver:edges[top]) {
                inDeg[ver]--;
                if (inDeg[ver] == 0) q.emplace(ver);
            }
        }
        return cnt == numCourses;
    }
};
```

#####  Leetcode 210 课程表2

拓扑排序求序列

```C++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>> &prerequisites) {
        vector<int> res;
        vector<int> inDeg(numCourses, 0);
        vector<vector<int>> edges(numCourses);
        for (auto &edge: prerequisites) {
            edges[edge[1]].emplace_back(edge[0]);
            inDeg[edge[0]]++;
        }
        queue<int> q;
        for (int i = 0; i < numCourses; i++) {
            if (inDeg[i] == 0) q.emplace(i);
        }
        while (q.size()) {
            int top = q.front();
            q.pop();
            res.emplace_back(top);
            for (auto &ver:edges[top]) {
                inDeg[ver]--;
                if (inDeg[ver] == 0) q.emplace(ver);
            }
        }

        if (res.size() < numCourses) return {};
        return res;
    }
};
```

### 最短路

##### 399. 除法求值

flyord 

边的存储方式:  unordered_map<string, unordered_map<string, double>> edges;

或者类似课程表：vector<vector<int>> edges

迭代顶点： for (auto & k:vers)

```C++
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>> &equations, vector<double> &values, vector<vector<string>> &queries) {
        //二维hash
        unordered_set<string> vers;
        unordered_map<string, unordered_map<string, double>> edges;
        for (int i = 0; i < equations.size(); i++) {
            string a = equations[i][0], b = equations[i][1];
            double c = values[i];
            vers.insert(a);
            vers.insert(b);
            edges[a][b] = c;
            edges[b][a] = 1/c;
        }

        //floyd
        for (auto & k:vers) {
            for (auto & i:vers) {
                for (auto & j:vers) {
                    if(edges[i][k] &&  edges[k][j])
                    edges[i][j] = edges[i][k] * edges[k][j];
                }
            }
        }

        vector<double> res;
        for (auto &query:queries) {
            string q0 = query[0], q1 = query[1];
            if (edges[q0][q1]) res.emplace_back(edges[q0][q1]);
            else res.emplace_back(-1);
        }
        return res;
    }
};

```
	
##### 743. 网络延迟时间

朴素dijsktra、堆优化dijkstra、bellman-ford、SPFA、flyord

```C++
//朴素dijsktra 初始化INT_MAX/2
class Solution {
    const static int N = 110, M = 6010;
    int edge[N][N], dist[N];
    bool vis[N];
    int n, k;
    const int inf = INT_MAX / 2;
public:
    int networkDelayTime(vector<vector<int>> &times, int _n, int _k) {
        //二维数组的初始化
        n = _n, k = _k;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                edge[i][j] = i == j ? 0 : inf;
            }
        }
        for (auto &time:times) {
            int a = time[0], b = time[1], val = time[2];
            edge[a][b] = min(edge[a][b], val);
        }

        dijkstra();
        int res = 0;
        for (int i = 1; i <= n; i++) {
            res = max(res, dist[i]);
        }
        return res >= inf ? -1 : res;
    }

    void dijkstra() {
        for (int i = 1; i <= n; i++) {
            dist[i] = inf;
            vis[i] = false;
        }
        dist[k] = 0;
        //一个大循环，两个小循环(找最小点 + 更新其他出边)
        for (int i = 1; i <= n; i++) {
            int x = 0;
            for (int j = 1; j <= n; j++) {
                if (!vis[j] && (x == 0 || dist[j] < dist[x])) {
                    x = j;
                }
            }
            vis[x] = true;
            for (int j = 1; j <= n; j++) {
                dist[j] = min(dist[j], dist[x] + edge[x][j]);
            }
        }
    }
};
//堆优化dijkstra
class Solution {
    const static int N = 110, M = 6010;
    int edge[M], w[M], next[M], head[N], dist[N];
    int n, k, idx;
    const int inf = INT_MAX / 2;
public:
    int networkDelayTime(vector<vector<int>> &times, int _n, int _k) {
        n = _n, k = _k;
        idx = 0;
        memset(edge, 0, sizeof(edge));
        memset(w, 0, sizeof(w));
        memset(next, 0, sizeof(next));
        memset(head, -1, sizeof(head));

        for (auto &time:times) {
            int a = time[0], b = time[1], val = time[2];
            add(a, b, val);
        }

        dijkstra();
        int res = 0;
        for (int i = 1; i <= n; i++) {
            res = max(res, dist[i]);
        }
        return res >= inf ? -1 : res;
    }

    //头插法,e  w  n  h
    //idx 从0开始,截止条件！=-1
    void add(int a, int b, int val) {
        edge[idx] = b, w[idx] = val, next[idx] = head[a], head[a] = idx;
        idx++;
    }

    void dijkstra() {
        for (int i = 1; i <= n; i++) {
            dist[i] = inf;
        }
        priority_queue<pair<int, int>> q;
        //当小顶堆使用
        q.emplace(make_pair(0, k));
        dist[k] = 0;
        while (q.size()) {
            auto top = q.top();
            int z = top.first, x = top.second;
            q.pop();
            for (int i = head[x]; i != -1; i = next[i]) {
                int b = edge[i], val = w[i];
                if (dist[b] > dist[x] + val) {
                    dist[b] = dist[x] + val;
                    q.emplace(make_pair(-dist[b], b));
                }
            }
        }
    }
};
```

```C++
//bellman-ford 结构体、邻接表
class Solution {
    const static int N = 110, M = 6010;
    int dist[N],prev[N];
    int n, k, m;
    const int inf = INT_MAX / 2;
    struct Edge {
        int a, b, c;
    } edges[M];
public:
    int networkDelayTime(vector<vector<int>> &times, int _n, int _k) {
        n = _n, k = _k;
        m = times.size();

        for (int i = 0; i < times.size(); i++) {
            int a = times[i][0], b = times[i][1], val = times[i][2];
            edges[i] = {a, b, val};
        }

        bf();
        int res = 0;
        for (int i = 1; i <= n; i++) {
            res = max(res, dist[i]);
        }
        return res >= inf ? -1 : res;
    }

    void bf() {
        for (int i = 1; i <= n; i++) {
            dist[i] = inf;
        }
        dist[k] = 0;
        //K代表最短路径最多包含n条边
        for (int i = 1; i <= n; i++) {
            //迭代所有边
            for (int j = 0; j <= m; j++) {
                auto prev = dist;
                memcpy(prev,dist,sizeof(dist));
                int a = edges[j].a, b = edges[j].b, val = edges[j].c;
                dist[b] = min(dist[b], prev[a] + val);
            }
        }
    }
};

//SPFA
class Solution {
    const static int N = 110, M = 6010;
    int edge[M], w[M], next[M], head[N];
    int dist[N];
    int n, k, idx;
    const int inf = INT_MAX / 2;
public:
    int networkDelayTime(vector<vector<int>> &times, int _n, int _k) {
        n = _n, k = _k;
        idx = 0;
        memset(edge, 0, sizeof(edge));
        memset(w, 0, sizeof(w));
        memset(next, 0, sizeof(next));
        memset(head, -1, sizeof(head));

        for (auto &time:times) {
            int a = time[0], b = time[1], val = time[2];
            add(a, b, val);
        }

        SPFA();
        int res = 0;
        for (int i = 1; i <= n; i++) {
            res = max(res, dist[i]);
        }
        return res >= inf ? -1 : res;
    }

    void add(int a, int b, int val) {
        edge[idx] = b, w[idx] = val, next[idx] = head[a], head[a] = idx;
        idx++;
    }

    void SPFA() {
        for (int i = 1; i <= n; i++) {
            dist[i] = inf;
        }
        dist[k] = 0;
        queue<int> q;
        q.emplace(k);
        while (q.size()) {
            auto x = q.front();
            q.pop();
            for (int i = head[x]; i != -1; i = next[i]) {
                int y = edge[i], val = w[i];
                if (dist[y] > dist[x] + val) {
                    dist[y] = dist[x] + val;
                    q.push(y);
                }
            }
        }
    }
};
```

```c++
//flyord 三层循环
class Solution {
    const static int N = 110, M = 6010;
    int edge[N][N];
    int n, k;
    const int inf = INT_MAX / 2;
public:
    int networkDelayTime(vector<vector<int>> &times, int _n, int _k) {
        n = _n, k = _k;
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= n; j++) {
                edge[i][j] = i == j ? 0 : inf;
            }
        }

        for (auto &time:times) {
            int a = time[0], b = time[1], val = time[2];
            edge[a][b] = val;
        }

        flyord();
        int res = 0;
        for (int i = 1; i <= n; i++) {
            res = max(res, edge[k][i]);
        }
        return res >= inf ? -1 : res;
    }


    void flyord() {
        //三层循环:
        //枚举中转点-->枚举起点-->枚举终点-->松弛操作
        for (int k = 1; k <= n; k++) {
            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= n; j++) {
                    edge[i][j] = min(edge[i][j], edge[i][k] + edge[k][j]);
                }
            }
        }
    }
};
```


	
	
	
