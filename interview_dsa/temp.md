
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

回文数问题总结：
                                     
1 中心扩展法：
2 动态规划：
3 字符串Hash(反转拷贝) ：                                                          
 

875   567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 

