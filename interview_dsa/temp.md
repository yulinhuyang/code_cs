
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



