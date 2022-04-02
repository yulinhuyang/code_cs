#### 66. 加一

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

