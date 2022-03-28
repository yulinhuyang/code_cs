
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

AC 自动机


