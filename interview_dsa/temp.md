##### 43. 字符串相乘

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
##### 51  N皇后
 
```C++


```
  
25   






