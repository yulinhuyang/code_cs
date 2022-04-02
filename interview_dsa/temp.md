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

