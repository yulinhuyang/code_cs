
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
