
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



