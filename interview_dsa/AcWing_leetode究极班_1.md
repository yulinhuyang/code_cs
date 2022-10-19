LeetCode究极班：https://www.acwing.com/activity/content/activity_person/content/29799/1/

### 究极班——Week 1（第1-10题）

#####  LeetCode  1. 两数之和
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> heap;
        for (int i = 0; i < nums.size(); i ++ ) {
            int r = target - nums[i];
            if (heap.count(r)) return {heap[r], i};
            heap[nums[i]] = i;
        }

        return {};
    }
};
```
#####  LeetCode  2. 两数相加
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        auto dummy = new ListNode(-1), cur = dummy;
        int t = 0;
        while (l1 || l2 || t) {
            if (l1) t += l1->val, l1 = l1->next;
            if (l2) t += l2->val, l2 = l2->next;
            cur = cur->next = new ListNode(t % 10);
            t /= 10;
        }
        return dummy->next;
    }
};
```
#####  LeetCode  3. 无重复字符的最长子串
```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> heap;
        int res = 0;
        for (int i = 0, j = 0; i < s.size(); i ++ ) {
            heap[s[i]] ++ ;
            while (heap[s[i]] > 1) heap[s[j ++ ]] -- ;
            res = max(res, i - j + 1);
        }
        return res;
    }
};
```
#####  LeetCode  4. 寻找两个正序数组的中位数
```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int tot = nums1.size() + nums2.size();
        if (tot % 2 == 0) {
            int left = find(nums1, 0, nums2, 0, tot / 2);
            int right = find(nums1, 0, nums2, 0, tot / 2 + 1);
            return (left + right) / 2.0;
        } else {
            return find(nums1, 0, nums2, 0, tot / 2 + 1);
        }
    }

    int find(vector<int>& nums1, int i, vector<int>& nums2, int j, int k) {
        if (nums1.size() - i > nums2.size() - j) return find(nums2, j, nums1, i, k);
        if (k == 1) {
            if (nums1.size() == i) return nums2[j];
            else return min(nums1[i], nums2[j]);
        }
        if (nums1.size() == i) return nums2[j + k - 1];
        int si = min((int)nums1.size(), i + k / 2), sj = j + k - k / 2;
        if (nums1[si - 1] > nums2[sj - 1])
            return find(nums1, i, nums2, sj, k - (sj - j));
        else
            return find(nums1, si, nums2, j, k - (si - i));
    }
};
```
#####  LeetCode  5. 最长回文子串
```C++
class Solution {
public:
    string longestPalindrome(string s) {
        string res;
        for (int i = 0; i < s.size(); i ++ ) {
            int l = i - 1, r = i + 1;
            while (l >= 0 && r < s.size() && s[l] == s[r]) l --, r ++ ;
            if (res.size() < r - l - 1) res = s.substr(l + 1, r - l - 1);

            l = i, r = i + 1;
            while (l >= 0 && r < s.size() && s[l] == s[r]) l --, r ++ ;
            if (res.size() < r - l - 1) res = s.substr(l + 1, r - l - 1);
        }

        return res;
    }
};
```
#####  LeetCode  6. Z 字形变换
```C++
class Solution {
public:
    string convert(string s, int n) {
        string res;
        if (n == 1) return s;
        for (int i = 0; i < n; i ++ ) {
            if (i == 0 || i == n - 1) {
                for (int j = i; j < s.size(); j += 2 * n - 2)
                    res += s[j];
            } else {
                for (int j = i, k = 2 * n - 2 - i; j < s.size() || k < s.size(); j += 2 * n - 2, k += 2 * n - 2) {
                    if (j < s.size()) res += s[j];
                    if (k < s.size()) res += s[k];
                }
            }
        }

        return res;
    }
};
```
#####  LeetCode  7. 整数反转
```C++
class Solution {
public:
    int reverse(int x) {
        int r = 0;
        while (x) {
            if (r > 0 && r > (INT_MAX - x % 10) / 10) return 0;
            if (r < 0 && r < (INT_MIN - x % 10) / 10) return 0;
            r = r * 10 + x % 10;
            x /= 10;
        }
        return r;
    }
};
```
#####  LeetCode  8. 字符串转换整数 (atoi)
```C++
class Solution {
public:
    int myAtoi(string str) {
        int k = 0;
        while (k < str.size() && str[k] == ' ') k ++ ;
        if (k == str.size()) return 0;

        int minus = 1;
        if (str[k] == '-') minus = -1, k ++ ;
        else if (str[k] == '+') k ++ ;

        int res = 0;
        while (k < str.size() && str[k] >= '0' && str[k] <= '9') {
            int x = str[k] - '0';
            if (minus > 0 && res > (INT_MAX - x) / 10) return INT_MAX;
            if (minus < 0 && -res < (INT_MIN + x) / 10) return INT_MIN;
            if (-res * 10 - x == INT_MIN) return INT_MIN;
            res = res * 10 + x;
            k ++ ;
            if (res > INT_MAX) break;
        }

        res *= minus;

        return res;
    }
};
```
#####  LeetCode  9. 回文数
```C++
转化成字符串
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return 0;
        string s = to_string(x);
        return s == string(s.rbegin(), s.rend());
    }
};
数值方法
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return 0;
        int y = x;
        long long res = 0;
        while (x) {
            res = res * 10 + x % 10;
            x /= 10;
        }
        return res == y;
    }
};
```
#####  LeetCode  10. 正则表达式匹配
```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size(), m = p.size();
        s = ' ' + s, p = ' ' + p;
        vector<vector<bool>> f(n + 1, vector<bool>(m + 1));
        f[0][0] = true;
        for (int i = 0; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                if (j + 1 <= m && p[j + 1] == '*') continue;
                if (i && p[j] != '*') {
                    f[i][j] = f[i - 1][j - 1] && (s[i] == p[j] || p[j] == '.');
                } else if (p[j] == '*') {
                    f[i][j] = f[i][j - 2] || i && f[i - 1][j] && (s[i] == p[j - 1] || p[j - 1] == '.');
                }
            }

        return f[n][m];
    }
};
```

### 究极班——Week 2(第 11 ~ 30 题）

##### LeetCode 11. 盛最多水的容器
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int res = 0;
        for (int i = 0, j = height.size() - 1; i < j;) {
            res = max(res, min(height[i], height[j]) * (j - i));
            if (height[i] > height[j]) j -- ;
            else i ++ ;
        }
        return res;
    }
};
```
##### LeetCode 12. 整数转罗马数字 
```cpp
class Solution {
public:
    string intToRoman(int num) {
        int values[] = {
            1000,
            900, 500, 400, 100,
            90, 50, 40, 10,
            9, 5, 4, 1
        };
        string reps[] = {
            "M",
            "CM", "D", "CD", "C",
            "XC", "L", "XL", "X",
            "IX", "V", "IV", "I",
        };

        string res;
        for (int i = 0; i < 13; i ++ ) {
            while (num >= values[i]) {
                num -= values[i];
                res += reps[i];
            }
        }

        return res;
    }
};
```
##### LeetCode 13. 罗马数字转整数 
```cpp
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> hash;
        hash['I'] = 1, hash['V'] = 5;
        hash['X'] = 10, hash['L'] = 50;
        hash['C'] = 100, hash['D'] = 500;
        hash['M'] = 1000;

        int res = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            if (i + 1 < s.size() && hash[s[i]] < hash[s[i + 1]])
                res -= hash[s[i]];
            else
                res += hash[s[i]];
        }

        return res;
    }
};

```
##### LeetCode 14. 最长公共前缀 
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string res;
        if (strs.empty()) return res;

        for (int i = 0;; i ++ ) {
            if (i >= strs[0].size()) return res;
            char c = strs[0][i];
            for (auto& str: strs)
                if (str.size() <= i || str[i] != c)
                    return res;
            res += c;
        }

        return res;
    }
};
```
##### LeetCode 15. 三数之和 
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i ++ ) {
            if (i && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1, k = nums.size() - 1; j < k; j ++ ) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;
                while (j < k - 1 && nums[i] + nums[j] + nums[k - 1] >= 0) k -- ;
                if (nums[i] + nums[j] + nums[k] == 0) {
                    res.push_back({nums[i], nums[j], nums[k]});
                }
            }
        }

        return res;
    }
};
```
##### LeetCode 16. 最接近的三数之和 
```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        pair<int, int> res(INT_MAX, INT_MAX);
        for (int i = 0; i < nums.size(); i ++ )
            for (int j = i + 1, k = nums.size() - 1; j < k; j ++ ) {
                while (k - 1 > j && nums[i] + nums[j] + nums[k - 1] >= target) k -- ;
                int s = nums[i] + nums[j] + nums[k];
                res = min(res, make_pair(abs(s - target), s));
                if (k - 1 > j) {
                    s = nums[i] + nums[j] + nums[k - 1];
                    res = min(res, make_pair(target - s, s));
                }
            }
        return res.second;
    }
};
```
##### LeetCode 17. 电话号码的字母组合 
```cpp
class Solution {
public:
    vector<string> ans;
    string strs[10] = {
        "", "", "abc", "def",
        "ghi", "jkl", "mno",
        "pqrs", "tuv", "wxyz",
    };

    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return ans;
        dfs(digits, 0, "");
        return ans;
    }

    void dfs(string& digits, int u, string path) {
        if (u == digits.size()) ans.push_back(path);
        else {
            for (auto c : strs[digits[u] - '0'])
                dfs(digits, u + 1, path + c);
        }
    }
};
```
##### LeetCode 18. 四数之和 
```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        for (int i = 0; i < nums.size(); i ++ ) {
            if (i && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < nums.size(); j ++ ) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;
                for (int k = j + 1, u = nums.size() - 1; k < u; k ++ ) {
                    if (k > j + 1 && nums[k] == nums[k - 1]) continue;
                    while (u - 1 > k && nums[i] + nums[j] + nums[k] + nums[u - 1] >= target) u -- ;
                    if (nums[i] + nums[j] + nums[k] + nums[u] == target) {
                        res.push_back({nums[i], nums[j], nums[k], nums[u]});
                    }
                }
            }
        }

        return res;
    }
};
```
##### LeetCode 19. 删除链表的倒数第N个节点 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int k) {
        auto dummy = new ListNode(-1);
        dummy->next = head;

        int n = 0;
        for (auto p = dummy; p; p = p->next) n ++ ;

        auto p = dummy;
        for (int i = 0; i < n - k - 1; i ++ ) p = p->next;
        p->next = p->next->next;

        return dummy->next;
    }
};
```
##### LeetCode 20. 有效的括号 
```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;

        for (auto c : s) {
            if (c == '(' || c == '[' || c == '{') stk.push(c);
            else {
                if (stk.size() && abs(stk.top() - c) <= 2) stk.pop();
                else return false;
            }
        }

        return stk.empty();
    }
};
```
##### LeetCode 21. 合并两个有序链表 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        auto dummy = new ListNode(-1), tail = dummy;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                tail = tail->next = l1;
                l1 = l1->next;
            } else {
                tail = tail->next = l2;
                l2 = l2->next;
            }
        }

        if (l1) tail->next = l1;
        if (l2) tail->next = l2;
        return dummy->next;
    }
};

```
##### LeetCode 22. 括号生成 
```cpp
class Solution {
public:

    vector<string> ans;

    vector<string> generateParenthesis(int n) {
        dfs(n, 0, 0, "");
        return ans;
    }

    void dfs(int n, int lc, int rc, string seq) {
        if (lc == n && rc == n) ans.push_back(seq);
        else {
            if (lc < n) dfs(n, lc + 1, rc, seq + '(');
            if (rc < n && lc > rc) dfs(n, lc, rc + 1, seq + ')');
        }
    }
};

```
##### LeetCode 23. 合并K个排序链表 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:

    struct Cmp {
        bool operator() (ListNode* a, ListNode* b) {
            return a->val > b->val;
        }
    };

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<ListNode*, vector<ListNode*>, Cmp> heap;
        auto dummy = new ListNode(-1), tail = dummy;
        for (auto l : lists) if (l) heap.push(l);

        while (heap.size()) {
            auto t = heap.top();
            heap.pop();

            tail = tail->next = t;
            if (t->next) heap.push(t->next);
        }

        return dummy->next;
    }
};
```
##### LeetCode 24. 两两交换链表中的节点 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        for (auto p = dummy; p->next && p->next->next;) {
            auto a = p->next, b = a->next;
            p->next = b;
            a->next = b->next;
            b->next = a;
            p = a;
        }

        return dummy->next;
    }
};
```
##### LeetCode 25. K 个一组翻转链表 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        for (auto p = dummy;;) {
            auto q = p;
            for (int i = 0; i < k && q; i ++ ) q = q->next;
            if (!q) break;
            auto a = p->next, b = a->next;
            for (int i = 0; i < k - 1; i ++ ) {
                auto c = b->next;
                b->next = a;
                a = b, b = c;
            }
            auto c = p->next;
            p->next = a, c->next = b;
            p = c;
        }
        return dummy->next;
    }
};
```
##### LeetCode 26. 删除排序数组中的重复项 
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int k = 0;
        for (int i = 0; i < nums.size(); i ++ )
            if (!i || nums[i] != nums[i - 1])
                nums[k ++ ] = nums[i];
        return k;
    }
};
```
##### LeetCode 27. 移除元素 
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int k = 0;
        for (int i = 0; i < nums.size(); i ++ )
            if (nums[i] != val)
                nums[k ++ ] = nums[i];
        return k;
    }
};
```
##### LeetCode 28. 实现 strStr() 
```cpp
class Solution {
public:
    int strStr(string s, string p) {
        if (p.empty()) return 0;
        int n = s.size(), m = p.size();
        s = ' ' + s, p = ' ' + p;

        vector<int> next(m + 1);
        for (int i = 2, j = 0; i <= m; i ++ ) {
            while (j && p[i] != p[j + 1]) j = next[j];
            if (p[i] == p[j + 1]) j ++ ;
            next[i] = j;
        }

        for (int i = 1, j = 0; i <= n; i ++ ) {
            while (j && s[i] != p[j + 1]) j = next[j];
            if (s[i] == p[j + 1]) j ++ ;
            if (j == m) return i - m;
        }

        return -1;
    }
};

```
##### LeetCode 29. 两数相除 
```cpp
class Solution {
public:
    int divide(int x, int y) {
        typedef long long LL;
        vector<LL> exp;
        bool is_minus = false;
        if (x < 0 && y > 0 || x > 0 && y < 0) is_minus = true;

        LL a = abs((LL)x), b = abs((LL)y);
        for (LL i = b; i <= a; i = i + i) exp.push_back(i);

        LL res = 0;
        for (int i = exp.size() - 1; i >= 0; i -- )
            if (a >= exp[i]) {
                a -= exp[i];
                res += 1ll << i;
            }

        if (is_minus) res = -res;

        if (res > INT_MAX || res < INT_MIN) res = INT_MAX;

        return res;
    }
};
```
##### LeetCode 30. 串联所有单词的子串 
```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> res;
        if (words.empty()) return res;
        int n = s.size(), m = words.size(), w = words[0].size();
        unordered_map<string, int> tot;
        for (auto& word : words) tot[word] ++ ;

        for (int i = 0; i < w; i ++ ) {
            unordered_map<string, int> wd;
            int cnt = 0;
            for (int j = i; j + w <= n; j += w) {
                if (j >= i + m * w) {
                    auto word = s.substr(j - m * w, w);
                    wd[word] -- ;
                    if (wd[word] < tot[word]) cnt -- ;
                }
                auto word = s.substr(j, w);
                wd[word] ++ ;
                if (wd[word] <= tot[word]) cnt ++ ;
                if (cnt == m) res.push_back(j - (m - 1) * w);
            }
        }

        return res;
    }
};
```



### 究极班——Week3（第 31 ~ 50 题）

#####  LeetCode  31. 下一个排列
```C++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int k = nums.size() - 1;
        while (k > 0 && nums[k - 1] >= nums[k]) k -- ;
        if (k <= 0) {
            reverse(nums.begin(), nums.end());
        } else {
            int t = k;
            while (t < nums.size() && nums[t] > nums[k - 1]) t ++ ;
            swap(nums[t - 1], nums[k - 1]);
            reverse(nums.begin() + k, nums.end());
        }
    }
};
```
#####  LeetCode  32. 最长有效括号
```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> stk;

        int res = 0;
        for (int i = 0, start = -1; i < s.size(); i ++ ) {
            if (s[i] == '(') stk.push(i);
            else {
                if (stk.size()) {
                    stk.pop();
                    if (stk.size()) {
                        res = max(res, i - stk.top());
                    } else {
                        res = max(res, i - start);
                    }
                } else {
                    start = i;
                }
            }
        }

        return res;
    }
};
```
#####  LeetCode  33. 搜索旋转排序数组
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty()) return -1;
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] >= nums[0]) l = mid;
            else r = mid - 1;
        }

        if (target >= nums[0]) l = 0;
        else l = r + 1, r = nums.size() - 1;

        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if (nums[r] == target) return r;
        return -1;
    }
};
```
#####  LeetCode  34. 在排序数组中查找元素的第一个和最后一个位置
```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if (nums.empty()) return {-1, -1};

        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if (nums[r] != target) return {-1, -1};

        int L = r;
        l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] <= target) l = mid;
            else r = mid - 1;
        }

        return {L, r};
    }
};
```
#####  LeetCode  35. 搜索插入位置
```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l = 0, r = nums.size();

        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        return l;
    }
};
```
#####  LeetCode  36. 有效的数独
```C++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        bool st[9];

        // 判断行
        for (int i = 0; i < 9; i ++ ) {
            memset(st, 0, sizeof st);
            for (int j = 0; j < 9; j ++ ) {
                if (board[i][j] != '.') {
                    int t = board[i][j] - '1';
                    if (st[t]) return false;
                    st[t] = true;
                }
            }
        }

        // 判断列
        for (int i = 0; i < 9; i ++ ) {
            memset(st, 0, sizeof st);
            for (int j = 0; j < 9; j ++ ) {
                if (board[j][i] != '.') {
                    int t = board[j][i] - '1';
                    if (st[t]) return false;
                    st[t] = true;
                }
            }
        }

        // 判断小方格
        for (int i = 0; i < 9; i += 3)
            for (int j = 0; j < 9; j += 3) {
                memset(st, 0, sizeof st);
                for (int x = 0; x < 3; x ++ )
                    for (int y = 0; y < 3; y ++ ) {
                        if (board[i + x][j + y] != '.') {
                            int t = board[i + x][j + y] - '1';
                            if (st[t]) return false;
                            st[t] = true;
                        }
                    }
            }

        return true;
    }
};
```
#####  LeetCode  37. 解数独
```C++
class Solution {
public:

    bool row[9][9], col[9][9], cell[3][3][9];

    void solveSudoku(vector<vector<char>>& board) {
        memset(row, 0, sizeof row);
        memset(col, 0, sizeof col);
        memset(cell, 0, sizeof cell);

        for (int i = 0; i < 9; i ++ )
            for (int j = 0; j < 9; j ++ )
                if (board[i][j] != '.') {
                    int t = board[i][j] - '1';
                    row[i][t] = col[j][t] = cell[i / 3][j / 3][t] = true;
                }

        dfs(board, 0, 0);
    }

    bool dfs(vector<vector<char>>& board, int x, int y) {
        if (y == 9) x ++, y = 0;
        if (x == 9) return true;

        if (board[x][y] != '.') return dfs(board, x, y + 1);
        for (int i = 0; i < 9; i ++ )
            if (!row[x][i] && !col[y][i] && !cell[x / 3][y / 3][i]) {
                board[x][y] = '1' + i;
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = true;
                if (dfs(board, x, y + 1)) return true;
                board[x][y] = '.';
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = false;
            }

        return false;
    }
};
```

#####  LeetCode  38. 外观数列
```C++
class Solution {
public:
    string countAndSay(int n) {
        string s = "1";
        for (int i = 0; i < n - 1; i ++ ) {
            string t;
            for (int j = 0; j < s.size();) {
                int k = j + 1;
                while (k < s.size() && s[k] == s[j]) k ++ ;
                t += to_string(k - j) + s[j];
                j = k;
            }
            s = t;
        }

        return s;
    }
};
```
#####  LeetCode  39. 组合总和
```C++
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combinationSum(vector<int>& c, int target) {
        dfs(c, 0, target);
        return ans;
    }

    void dfs(vector<int>& c, int u, int target) {
        if (target == 0) {
            ans.push_back(path);
            return;
        }
        if (u == c.size()) return;

        for (int i = 0; c[u] * i <= target; i ++ ) {
            dfs(c, u + 1, target - c[u] * i);
            path.push_back(c[u]);
        }

        for (int i = 0; c[u] * i <= target; i ++ ) {
            path.pop_back();
        }
    }
};
```
#####  LeetCode  40. 组合总和 II
```C++
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combinationSum2(vector<int>& c, int target) {
        sort(c.begin(), c.end());
        dfs(c, 0, target);

        return ans;
    }

    void dfs(vector<int>& c, int u, int target) {
        if (target == 0) {
            ans.push_back(path);
            return;
        }
        if (u == c.size()) return;

        int k = u + 1;
        while (k < c.size() && c[k] == c[u]) k ++ ;
        int cnt = k - u;

        for (int i = 0; c[u] * i <= target && i <= cnt; i ++ ) {
            dfs(c, k, target - c[u] * i);
            path.push_back(c[u]);
        }

        for (int i = 0; c[u] * i <= target && i <= cnt; i ++ ) {
            path.pop_back();
        }
    }
};

```



#####  LeetCode 41. 缺失的第一个正数
```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        if (!n) return 1;

        for (auto& x : nums)
            if (x != INT_MIN) x -- ;

        for (int i = 0; i < n; i ++ ) {
            while (nums[i] >= 0 && nums[i] < n && nums[i] != i && nums[i] != nums[nums[i]])
                swap(nums[i], nums[nums[i]]);
        }

        for (int i = 0; i < n; i ++ )
            if (nums[i] != i)
                return i + 1;

        return n + 1;
    }
};
```
#####  LeetCode 42. 接雨水
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        stack<int> stk;
        int res = 0;
        for (int i = 0; i < height.size(); i ++ ) {
            int last = 0;
            while (stk.size() && height[stk.top()] <= height[i]) {
                res += (height[stk.top()] - last) * (i - stk.top() - 1);
                last = height[stk.top()];
                stk.pop();
            }

            if (stk.size()) res += (i - stk.top() - 1) * (height[i] - last);
            stk.push(i);
        }

        return res;
    }
};
```
#####  LeetCode 43. 字符串相乘
```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        vector<int> A, B;
        int n = num1.size(), m = num2.size();
        for (int i = n - 1; i >= 0; i -- ) A.push_back(num1[i] - '0');
        for (int i = m - 1; i >= 0; i -- ) B.push_back(num2[i] - '0');

        vector<int> C(n + m);
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                C[i + j] += A[i] * B[j];

        for (int i = 0, t = 0; i < C.size(); i ++ ) {
            t += C[i];
            C[i] = t % 10;
            t /= 10;
        }

        int k = C.size() - 1;
        while (k > 0 && !C[k]) k -- ;

        string res;
        while (k >= 0) res += C[k -- ] + '0';

        return res;
    }
};
```
#####  LeetCode 44. 通配符匹配
```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size(), m = p.size();
        s = ' ' + s, p = ' ' + p;
        vector<vector<bool>> f(n + 1, vector<bool>(m + 1));
        f[0][0] = true;

        for (int i = 0; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                if (p[j] == '*') {
                    f[i][j] = f[i][j - 1] || i && f[i - 1][j];
                } else {
                    f[i][j] = (s[i] == p[j] || p[j] == '?') && i && f[i - 1][j - 1];
                }
            }

        return f[n][m];
    }
};
```
#####  LeetCode 45. 跳跃游戏 II
```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n);

        for (int i = 1, j = 0; i < n; i ++ ) {
            while (j + nums[j] < i) j ++ ;
            f[i] = f[j] + 1;
        }

        return f[n - 1];
    }
};
```
#####  LeetCode 46. 全排列
```cpp
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;
    vector<bool> st;

    vector<vector<int>> permute(vector<int>& nums) {
        path = vector<int>(nums.size());
        st = vector<bool>(nums.size());

        dfs(nums, 0);

        return ans;
    }

    void dfs(vector<int>& nums, int u) {
        if (u == nums.size()) {
            ans.push_back(path);
            return;
        }

        for (int i = 0; i < nums.size(); i ++ ) {
            if (st[i] == false) {
                path[u] = nums[i];
                st[i] = true;
                dfs(nums, u + 1);
                st[i] = false;
            }
        }
    }
};
```
#####  LeetCode 47. 全排列 II
```cpp
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;
    vector<bool> st;

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        path = vector<int>(nums.size());
        st = vector<bool>(nums.size());
        dfs(nums, 0);
        return ans;
    }

    void dfs(vector<int>&nums, int u) {
        if (u == nums.size()) {
            ans.push_back(path);
            return;
        }

        for (int i = 0; i < nums.size(); i ++ ) {
            if (!st[i]) {
                if (i && nums[i - 1] == nums[i] && !st[i - 1]) continue;
                st[i] = true;
                path[u] = nums[i];
                dfs(nums, u + 1);
                st[i] = false;
            }
        }
    }
};

```
#####  LeetCode 48. 旋转图像
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < i; j ++ )
                swap(matrix[i][j], matrix[j][i]);

        for (int i = 0; i < n; i ++ )
            for (int j = 0, k = n - 1; j < k; j ++, k -- )
                swap(matrix[i][j], matrix[i][k]);
    }
};
```
#####  LeetCode 49. 字母异位词分组
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> hash;
        for (auto& str: strs) {
            string nstr = str;
            sort(nstr.begin(), nstr.end());
            hash[nstr].push_back(str);
        }

        vector<vector<string>> res;
        for (auto& item : hash) res.push_back(item.second);

        return res;
    }
};
```
#####  LeetCode 50. Pow(x, n)
```cpp
class Solution {
public:
    double myPow(double x, int n) {
        typedef long long LL;
        bool is_minus = n < 0;
        double res = 1;
        for (LL k = abs(LL(n)); k; k >>= 1) {
            if (k & 1) res *= x;
            x *= x;
        }
        if (is_minus) res = 1 / res;
        return res;
    }
};

```


### 究极班——Week 4(第 51 ~ 70 题）

##### LeetCode 51. N 皇后
```cpp
class Solution {
public:

    int n;
    vector<bool> col, dg, udg;
    vector<vector<string>> ans;
    vector<string> path;

    vector<vector<string>> solveNQueens(int _n) {
        n = _n;
        col = vector<bool>(n);
        dg = udg = vector<bool>(n * 2);
        path = vector<string>(n, string(n, '.'));

        dfs(0);
        return ans;
    }

    void dfs(int u) {
        if (u == n) {
            ans.push_back(path);
            return;
        }

        for (int i = 0; i < n; i ++ ) {
            if (!col[i] && !dg[u - i + n] && !udg[u + i]) {
                col[i] = dg[u - i + n] = udg[u + i] = true;
                path[u][i] = 'Q';
                dfs(u + 1);
                path[u][i] = '.';
                col[i] = dg[u - i + n] = udg[u + i] = false;
            }
        }
    }
};
```
 
##### LeetCode 52. N皇后 II 
```cpp
class Solution {
public:
    int n;
    vector<bool> col, dg, udg;

    int totalNQueens(int _n) {
        n = _n;
        col = vector<bool>(n);
        dg = udg = vector<bool>(2 * n);
        return dfs(0);
    }

    int dfs(int u) {
        if (u == n) return 1;
        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            if (!col[i] && !dg[u - i + n] && !udg[u + i]) {
                col[i] = dg[u - i + n] = udg[u + i] = true;
                res += dfs(u + 1);
                col[i] = dg[u - i + n] = udg[u + i] = false;
            }
        }

        return res;
    }
};

```
 
##### LeetCode 53. 最大子序和 
```cpp
DP 时间O(n)，空间 O(1)
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = INT_MIN;
        for (int i = 0, last = 0; i < nums.size(); i ++ ) {
            last = nums[i] + max(last, 0);
            res = max(res, last);
        }
        return res;
    }
};


分治 时间O(n)，空间O(logn)
class Solution {
public:

    struct Node {
        int sum, s, ls, rs;
    };

    Node build(vector<int>& nums, int l, int r) {
        if (l == r) {
            int v = max(nums[l], 0);
            return {nums[l], v, v, v};
        }

        int mid = l + r >> 1;
        auto L = build(nums, l, mid), R = build(nums, mid + 1, r);
        Node res;
        res.sum = L.sum + R.sum;
        res.s = max(max(L.s, R.s), L.rs + R.ls);
        res.ls = max(L.ls, L.sum + R.ls);
        res.rs = max(R.rs, R.sum + L.rs);
        return res;
    }

    int maxSubArray(vector<int>& nums) {
        int res = INT_MIN;
        for (auto x: nums) res = max(res, x);
        if (res < 0) return res;
        auto t = build(nums, 0, nums.size() - 1);
        return t.s;
    }
};
```
 
##### LeetCode 54. 螺旋矩阵 
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> res;
        int n = matrix.size();
        if (!n) return res;
        int m = matrix[0].size();

        int dx[] = {0, 1, 0, -1}, dy[] = {1, 0, -1, 0};
        vector<vector<bool>> st(n, vector<bool>(m));

        for (int i = 0, x = 0, y = 0, d = 0; i < n * m; i ++ ) {
            res.push_back(matrix[x][y]);
            st[x][y] = true;

            int a = x + dx[d], b = y + dy[d];
            if (a < 0 || a >= n || b < 0 || b >= m || st[a][b]) {
                d = (d + 1) % 4;
                a = x + dx[d], b = y + dy[d];
            }

            x = a, y = b;
        }

        return res;
    }
};
```

##### LeetCode 55. 跳跃游戏 
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        for (int i = 0, j = 0; i < nums.size(); i ++ ) {
            if (j < i) return false;
            j = max(j, i + nums[i]);
        }
        return true;
    }
};
``` 
 
##### LeetCode 56. 合并区间 
```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& a) {
        vector<vector<int>> res;
        if (a.empty()) return res;

        sort(a.begin(), a.end());
        int l = a[0][0], r = a[0][1];
        for (int i = 1; i < a.size(); i ++ ) {
            if (a[i][0] > r) {
                res.push_back({l, r});
                l = a[i][0], r = a[i][1];
            } else r = max(r, a[i][1]);
        }

        res.push_back({l, r});
        return res;
    }
};
``` 
 
##### LeetCode 57. 插入区间 
```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& a, vector<int>& b) {
        vector<vector<int>> res;
        int k = 0;
        while (k < a.size() && a[k][1] < b[0]) res.push_back(a[k ++ ]); // 左边完全没交集的部分

        if (k < a.size()) {
            b[0] = min(b[0], a[k][0]);
            while (k < a.size() && a[k][0] <= b[1]) b[1] = max(b[1], a[k ++ ][1]);
        }
        res.push_back(b);

        while (k < a.size()) res.push_back(a[k ++ ]);
        return res;
    }
};

``` 
##### LeetCode 58. 最后一个单词的长度 
```cpp
使用stringstream
class Solution {
public:
    int lengthOfLastWord(string s) {
        stringstream ssin(s);
        int res = 0;
        string word;
        while (ssin >> word) res = word.size();
        return res;
    }
};

双指针
class Solution {
public:
    int lengthOfLastWord(string s) {
        for (int i = s.size() - 1; i >= 0; i -- ) {
            if (s[i] == ' ') continue;
            int j = i - 1;
            while (j >= 0 && s[j] != ' ') j -- ;
            return i - j;
        }

        return 0;
    }
};

``` 

##### LeetCode 59. 螺旋矩阵 II 
```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n));

        int dx[] = {0, 1, 0, -1}, dy[] = {1, 0, -1, 0};
        for (int i = 1, x = 0, y = 0, d = 0; i <= n * n; i ++ ) {
            res[x][y] = i;
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
##### LeetCode 60. 第k个排列 

```cpp
使用next_permutaion O(n!×n)
class Solution {
public:
    string getPermutation(int n, int k) {
        string res;
        for (int i = 1; i <= n; i ++ ) res += to_string(i);
        for (int i = 0; i < k - 1; i ++ ) {
            next_permutation(res.begin(), res.end());
        }
        return res;
    }
};

计数法 O(n2)
class Solution {
public:
    string getPermutation(int n, int k) {
        string res;
        vector<bool> st(10);
        for (int i = 0; i < n; i ++ ) {
            int fact = 1;
            for (int j = 1; j <= n - i - 1; j ++ ) fact *= j;

            for (int j = 1; j <= n; j ++ ) {
                if (!st[j]) {
                    if (fact < k) k -= fact;
                    else {
                        res += to_string(j);
                        st[j] = true;
                        break;
                    }
                }
            }
        }

        return res;
    }
};
``` 

##### LeetCode 61. 旋转链表 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return head;
        int n = 0;
        ListNode* tail;
        for (auto p = head; p; p = p->next) {
            tail = p;
            n ++ ;
        }
        k %= n;
        if (!k) return head;

        auto p = head;
        for (int i = 0; i < n - k - 1; i ++ ) p = p->next;
        tail->next = head;
        head = p->next;
        p->next = nullptr;
        return head;
    }
};

``` 
##### LeetCode 62. 不同路径 
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> f(n, vector<int>(m));
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!i && !j) f[i][j] = 1;
                else {
                    if (i) f[i][j] += f[i - 1][j];
                    if (j) f[i][j] += f[i][j - 1];
                }

        return f[n - 1][m - 1];
    }
};

``` 
##### LeetCode 63. 不同路径 II 
```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& o) {
        int n = o.size();
        if (!n) return 0;
        int m = o[0].size();

        vector<vector<int>> f(n, vector<int>(m));
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!o[i][j]) {
                    if (!i && !j) f[i][j] = 1;
                    else {
                        if (i) f[i][j] += f[i - 1][j];
                        if (j) f[i][j] += f[i][j - 1];
                    }
                }

        return f[n - 1][m - 1];
    }
};
``` 
##### LeetCode 64. 最小路径和 
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int n = grid.size();
        if (!n) return 0;
        int m = grid[0].size();

        vector<vector<int>> f(n, vector<int>(m, INT_MAX));
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                if (!i && !j) f[i][j] = grid[i][j];
                else {
                    if (i) f[i][j] = min(f[i][j], f[i - 1][j] + grid[i][j]);
                    if (j) f[i][j] = min(f[i][j], f[i][j - 1] + grid[i][j]);
                }
            }

        return f[n - 1][m - 1];
    }
};

``` 
##### LeetCode 65. 有效数字 
```cpp
class Solution {
public:
    bool isNumber(string s) {
        int l = 0, r = s.size() - 1;
        while (l <= r && s[l] == ' ') l ++ ;
        while (l <= r && s[r] == ' ') r -- ;
        if (l > r) return false;
        s = s.substr(l, r - l + 1);
        if (s[0] == '+' || s[0] == '-') s = s.substr(1);
        if (s.empty()) return false;

        if (s[0] == '.' && (s.size() == 1 || s[1] == 'e' || s[1] == 'E'))
            return false;

        int dot = 0, e = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == '.') {
                if (dot > 0 || e > 0) return false;
                dot ++ ;
            } else if (s[i] == 'e' || s[i] == 'E') {
                if (!i || i + 1 == s.size() || e > 0) return false;
                if (s[i + 1] == '+' || s[i + 1] == '-') {
                    if (i + 2 == s.size()) return false;
                    i ++ ;
                }
                e ++ ;
            } else if (s[i] < '0' || s[i] > '9') return false;
        }

        return true;
    }
};

``` 
##### LeetCode 66. 加一 
```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        reverse(digits.begin(), digits.end());
        int t = 1;
        for (auto& x: digits) {
            t += x;
            x = t % 10;
            t /= 10;
        }
        if (t) digits.push_back(t);

        reverse(digits.begin(), digits.end());

        return digits;
    }
};
``` 
##### LeetCode 67. 二进制求和 
```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        reverse(a.begin(), a.end());
        reverse(b.begin(), b.end());

        string c;
        for (int i = 0, t = 0; i < a.size() || i < b.size() || t; i ++ ) {
            if (i < a.size()) t += a[i] - '0';
            if (i < b.size()) t += b[i] - '0';
            c += to_string(t % 2);
            t /= 2;
        }

        reverse(c.begin(), c.end());
        return c;
    }
};

``` 
##### LeetCode 68. 文本左右对齐 
```cpp
class Solution {
public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        vector<string> res;
        for (int i = 0; i < words.size(); i ++ ) {
            int j = i + 1;
            int len = words[i].size();
            while (j < words.size() && len + 1 + words[j].size() <= maxWidth)
                len += 1 + words[j ++ ].size();

            string line;
            if (j == words.size() || j == i + 1) {  // 左对齐
                line += words[i];
                for (int k = i + 1; k < j; k ++ ) line += ' ' + words[k];
                while(line.size() < maxWidth) line += ' ';
            } else {  // 左右对齐
                int cnt = j - i - 1, r = maxWidth - len + cnt;
                line += words[i];
                int k = 0;
                while (k < r % cnt) line += string(r / cnt + 1, ' ') + words[i + k + 1], k ++ ;
                while (k < cnt) line += string(r / cnt, ' ' ) + words[i + k + 1], k ++ ;
            }

            res.push_back(line);
            i = j - 1;
        }

        return res;
    }
};
``` 
##### LeetCode 69. x 的平方根   
```cpp
class Solution {
public:
    int mySqrt(int x) {
        int l = 0, r = x;
        while (l < r) {
            int mid = l + 1ll + r >> 1;
            if (mid <= x / mid) l = mid;
            else r = mid - 1;
        }

        return r;
    }
};
``` 
##### LeetCode 70. 爬楼梯   
```cpp
class Solution {
public:
    int climbStairs(int n) {
        int a = 1, b = 1;
        while ( -- n) {
            int c = a + b;
            a = b, b = c;
        }
        return b;
    }
};
```



### 究极班——Week 5（第 71 ~ 90 题）


##### LeetCode 71. 简化路径
```cpp
class Solution {
public:
    string simplifyPath(string path) {
        string res, name;
        if (path.back() != '/') path += '/';
        for (auto c : path) {
            if (c != '/') name += c;
            else {
                if (name == "..") {
                    while (res.size() && res.back() != '/') res.pop_back();
                    if (res.size()) res.pop_back();
                } else if (name != "." && name != "") {
                    res += '/' + name;
                }
                name.clear();
            }
        }

        if (res.empty()) res = "/";
        return res;
    }
};
```
 
##### LeetCode 72. 编辑距离
```cpp
class Solution {
public:
    int minDistance(string a, string b) {
        int n = a.size(), m = b.size();
        a = ' ' + a, b = ' ' + b;
        vector<vector<int>> f(n + 1, vector<int>(m + 1));

        for (int i = 0; i <= n; i ++ ) f[i][0] = i;
        for (int i = 1; i <= m; i ++ ) f[0][i] = i;

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                f[i][j] = min(f[i - 1][j], f[i][j - 1]) + 1;
                int t = a[i] != b[j];
                f[i][j] = min(f[i][j], f[i - 1][j - 1] + t);
            }

        return f[n][m];
    }
};
``` 
 
##### LeetCode 73. 矩阵置零 
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return;
        int n = matrix.size(), m = matrix[0].size();

        int r0 = 1, c0 = 1;
        for (int i = 0; i < m; i ++ ) if (!matrix[0][i]) r0 = 0;
        for (int i = 0; i < n; i ++ ) if (!matrix[i][0]) c0 = 0;

        for (int i = 1; i < m; i ++ )
            for (int j = 0; j < n; j ++ )
                if (!matrix[j][i])
                    matrix[0][i] = 0;

        for (int i = 1; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!matrix[i][j])
                    matrix[i][0] = 0;

        for (int i = 1; i < m; i ++ )
            if (!matrix[0][i])
                for (int j = 0; j < n; j ++ )
                    matrix[j][i] = 0;

        for (int i = 1; i < n; i ++ )
            if (!matrix[i][0])
                for (int j = 0; j < m; j ++ )
                    matrix[i][j] = 0;

        if (!r0) for (int i = 0; i < m; i ++ ) matrix[0][i] = 0;
        if (!c0) for (int i = 0; i < n; i ++ ) matrix[i][0] = 0;
    }
};
``` 
 
##### LeetCode 74. 搜索二维矩阵 
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;
        int n = matrix.size(), m = matrix[0].size();

        int l = 0, r = n * m - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (matrix[mid / m][mid % m] >= target) r = mid;
            else l = mid + 1;
        }

        return matrix[r / m][r % m] == target;
    }
};
```

##### LeetCode 75. 颜色分类
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        for (int i = 0, j = 0, k = nums.size() - 1; i <= k;) {
            if (nums[i] == 0) swap(nums[i ++ ], nums[j ++ ]);
            else if (nums[i] == 2) swap(nums[i], nums[k -- ]);
            else i ++ ;
        }
    }
};
``` 

##### LeetCode 76. 最小覆盖子串

```cpp
lass Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> hs, ht;
        for (auto c: t) ht[c] ++ ;

        string res;
        int cnt = 0;
        for (int i = 0, j = 0; i < s.size(); i ++ ) {
            hs[s[i]] ++ ;
            if (hs[s[i]] <= ht[s[i]]) cnt ++ ;

            while (hs[s[j]] > ht[s[j]]) hs[s[j ++ ]] -- ;
            if (cnt == t.size()) {
                if (res.empty() || i - j + 1 < res.size())
                    res = s.substr(j, i - j + 1);
            }
        }

        return res;
    }
};
```

##### LeetCode 77. 组合
```cpp
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combine(int n, int k) {
        dfs(n, k, 1);
        return ans;
    }

    void dfs(int n, int k, int start) {
        if (!k) {
            ans.push_back(path);
            return;
        }

        for (int i = start; i <= n; i ++ ) {
            path.push_back(i);
            dfs(n, k - 1, i + 1);
            path.pop_back();
        }
    }
};
```

##### LeetCode 78. 子集
```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> res;
        int n = nums.size();
        for (int i = 0; i < 1 << n; i ++ ) {
            vector<int> path;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                    path.push_back(nums[j]);
            res.push_back(path);
        }

        return res;
    }
};

```

##### LeetCode 79. 单词搜索
```cpp
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        for (int i = 0; i < board.size(); i ++ ) {
            for (int j = 0; j < board[i].size(); j ++ ) {
                if (dfs(board, word, 0, i, j)) return true;
            }
        }
        return false;
    }

    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    bool dfs(vector<vector<char>>& board, string& word, int u, int x, int y) {
        if (board[x][y] != word[u]) return false;
        if (u == word.size() - 1) return true;

        char t = board[x][y];
        board[x][y] = '.';
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a >= board.size() || b < 0 || b >= board[0].size() || board[a][b] == '.') continue;
            if (dfs(board, word, u + 1, a, b)) return true;
        }
        board[x][y] = t;
        return false;
    }
};

```

##### LeetCode 80. 删除排序数组中的重复项 II
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int k = 0;
        for (auto x: nums)
            if (k < 2 || (nums[k - 1] != x || nums[k - 2] != x))
                nums[k ++ ] = x;
        return k;
    }
};
```

##### LeetCode 81. 搜索旋转排序数组 II
```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        if (nums.empty()) return false;
        int R = nums.size() - 1;
        while (R >= 0 && nums[R] == nums[0]) R -- ;
        if (R < 0) return nums[0] == target;

        int l = 0, r = R;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] >= nums[0]) l = mid;
            else r = mid - 1;
        }

        if (target >= nums[0]) r = l, l = 0;
        else l ++, r = R;

        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        return nums[r] == target;
    }
};
```

##### LeetCode 82. 删除排序链表中的重复元素 II
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        auto p = dummy;
        while (p->next) {
            auto q = p->next->next;
            while (q && q->val == p->next->val) q = q->next;
            if (p->next->next == q) p = p->next;
            else p->next = q;
        }
        return dummy->next;
    }
};

```


##### LeetCode 83. 删除排序链表中的重复元素
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return head;
        auto cur = head;
        for (auto p = head->next; p; p = p->next)
            if (p->val != cur->val)
                cur = cur->next = p;

        cur->next = NULL;
        return head;
    }
};
```

##### LeetCode 84. 柱状图中最大的矩形
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& h) {
        int n = h.size();
        vector<int> left(n), right(n);
        stack<int> stk;

        for (int i = 0; i < n; i ++ ) {
            while (stk.size() && h[stk.top()] >= h[i]) stk.pop();
            if (stk.empty()) left[i] = -1;
            else left[i] = stk.top();
            stk.push(i);
        }

        stk = stack<int>();
        for (int i = n - 1; i >= 0; i -- ) {
            while (stk.size() && h[stk.top()] >= h[i]) stk.pop();
            if (stk.empty()) right[i] = n;
            else right[i] = stk.top();
            stk.push(i);
        }

        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            res = max(res, h[i] * (right[i] - left[i] - 1));
        }

        return res;
    }
};
```

##### LeetCode 85. 最大矩形
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& h) {
        int n = h.size();
        vector<int> left(n), right(n);
        stack<int> stk;

        for (int i = 0; i < n; i ++ ) {
            while (stk.size() && h[stk.top()] >= h[i]) stk.pop();
            if (stk.empty()) left[i] = -1;
            else left[i] = stk.top();
            stk.push(i);
        }

        stk = stack<int>();
        for (int i = n - 1; i >= 0; i -- ) {
            while (stk.size() && h[stk.top()] >= h[i]) stk.pop();
            if (stk.empty()) right[i] = n;
            else right[i] = stk.top();
            stk.push(i);
        }

        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            res = max(res, h[i] * (right[i] - left[i] - 1));
        }

        return res;
    }

    int maximalRectangle(vector<vector<char>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return 0;
        int n = matrix.size(), m = matrix[0].size();

        vector<vector<int>> h(n, vector<int>(m));
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                if (matrix[i][j] == '1') {
                    if (i) h[i][j] = 1 + h[i - 1][j];
                    else h[i][j] = 1;
                }
            }

        int res = 0;
        for (int i = 0; i < n; i ++ ) res = max(res, largestRectangleArea(h[i]));

        return res;
    }
};
```

##### LeetCode 86. 分隔链表
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        auto lh = new ListNode(-1), rh = new ListNode(-1);
        auto lt = lh, rt = rh;

        for (auto p = head; p; p = p->next) {
            if (p->val < x) lt = lt->next = p;
            else rt = rt->next = p;
        }

        lt->next = rh->next;
        rt->next = NULL;

        return lh->next;
    }
};
```

##### LeetCode 87. 扰乱字符串
```cpp
class Solution {
public:
    bool isScramble(string s1, string s2) {
        if (s1 == s2) return true;
        string bs1 = s1, bs2 = s2;
        sort(bs1.begin(), bs1.end()), sort(bs2.begin(), bs2.end());
        if (bs1 != bs2) return false;

        int n = s1.size();
        for (int i = 1; i <= n - 1; i ++ ) {
            if (isScramble(s1.substr(0, i), s2.substr(0, i)) &&
                isScramble(s1.substr(i), s2.substr(i))) return true;
            if (isScramble(s1.substr(0, i), s2.substr(n - i)) &&
                isScramble(s1.substr(i), s2.substr(0, n - i))) return true;
        }

        return false;
    }
};

```

##### LeetCode 88. 合并两个有序数组
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int k = n + m - 1;
        int i = m - 1, j = n - 1;
        while (i >= 0 && j >= 0)
            if (nums1[i] >= nums2[j]) nums1[k -- ] = nums1[i -- ];
            else nums1[k -- ] = nums2[j -- ];

        while (j >= 0) nums1[k -- ] = nums2[j -- ];
    }
};

```

##### LeetCode 89. 格雷编码
```cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> res(1, 0);
        while (n -- ) {
            for (int i = res.size() - 1; i >= 0; i -- ) {
                res[i] *= 2;
                res.push_back(res[i] + 1);
            }
        }
        return res;
    }
};
```

##### LeetCode 90. 子集 II
```cpp
class Solution {
public:

    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        dfs(nums, 0);
        return ans;
    }

    void dfs(vector<int>& nums, int u) {
        if (u == nums.size()) {
            ans.push_back(path);
            return;
        }

        int k = u + 1;
        while (k < nums.size() && nums[k] == nums[u]) k ++ ;

        for (int i = 0; i <= k - u; i ++ ) {
            dfs(nums, k);
            path.push_back(nums[u]);
        }

        for (int i = 0; i <= k - u; i ++ ) {
            path.pop_back();
        }
    }
};

```


### 究极班——Week6(第91 ~ 110 题）

#####  LeetCode 91. 解码方法
```cpp
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        s = ' ' + s;
        vector<int> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ ) {
            if (s[i] >= '1' && s[i] <= '9') f[i] += f[i - 1];
            if (i > 1) {
                int t = (s[i - 1] - '0') * 10 + s[i] - '0';
                if (t >= 10 && t <= 26) f[i] += f[i - 2];
            }
        }

        return f[n];
    }
};

```
#####  LeetCode 92. 反转链表 II
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        auto dummy = new ListNode(-1);
        dummy->next = head;

        auto a = dummy;
        for (int i = 0; i < m - 1; i ++ ) a = a->next;
        auto b = a->next, c = b->next;
        for (int i = 0; i < n - m; i ++ ) {
            auto t = c->next;
            c->next = b;
            b = c, c = t;
        }
        a->next->next = c;
        a->next = b;
        return dummy->next;
    }
};

```
#####  LeetCode 93. 复原IP地址
```cpp
class Solution {
public:
    vector<string> ans;
    vector<string> restoreIpAddresses(string s) {
        dfs(s, 0, 0, "");
        return ans;
    }

    void dfs(string& s, int u, int k, string path) {
        if (u == s.size()) {
            if (k == 4) {
                path.pop_back();
                ans.push_back(path);
            }
            return;
        }
        if (k == 4) return;
        for (int i = u, t = 0; i < s.size(); i ++ ) {
            if (i > u && s[u] == '0') break;  // 有前导0
            t = t * 10 + s[i] - '0';
            if (t <= 255) dfs(s, i + 1, k + 1, path + to_string(t) + '.');
            else break;
        }
    }
};

```
#####  LeetCode 94. 二叉树的中序遍历
```cpp
递归写法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> ans;
    vector<int> inorderTraversal(TreeNode* root) {
        dfs(root);
        return ans;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        ans.push_back(root->val);
        dfs(root->right);
    }
};

迭代写法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;

        while (root || stk.size()) {
            while (root) {
                stk.push(root);
                root = root->left;
            }

            root = stk.top();
            stk.pop();
            res.push_back(root->val);
            root = root->right;
        }

        return res;
    }
};

```
#####  LeetCode 95. 不同的二叉搜索树 II
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        if (!n) return {};
        return dfs(1, n);
    }

    vector<TreeNode*> dfs(int l, int r) {
        if (l > r) return {NULL};
        vector<TreeNode*> res;

        for (int i = l; i <= r; i ++ ) {
            auto left = dfs(l, i - 1), right = dfs(i + 1, r);
            for (auto l: left)
                for (auto r: right) {
                    auto root = new TreeNode(i);
                    root->left = l, root->right = r;
                    res.push_back(root);
                }
        }

        return res;
    }
};

```
#####  LeetCode 96. 不同的二叉搜索树
```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= i; j ++ )
                f[i] += f[j - 1] * f[i - j];
        return f[n];
    }
};
```
#####  LeetCode 97. 交错字符串
```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int n = s1.size(), m = s2.size();
        if (s3.size() != n + m) return false;

        vector<vector<bool>> f(n + 1, vector<bool>(m + 1));
        s1 = ' ' + s1, s2 = ' ' + s2, s3 = ' ' + s3;
        for (int i = 0; i <= n; i ++ )
            for (int j = 0; j <= m; j ++ )
                if (!i && !j) f[i][j] = true;
                else {
                    if (i && s1[i] == s3[i + j]) f[i][j] = f[i - 1][j];
                    if (j && s2[j] == s3[i + j]) f[i][j] = f[i][j] || f[i][j - 1];
                }

        return f[n][m];
    }
};
```
#####  LeetCode 98. 验证二叉搜索树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        return dfs(root)[0];
    }

    vector<int> dfs(TreeNode* root) {
        vector<int> res({1, root->val, root->val});
        if (root->left) {
            auto t = dfs(root->left);
            if (!t[0] || t[2] >= root->val) res[0] = 0;
            res[1] = min(res[1], t[1]);
            res[2] = max(res[2], t[2]);
        }
        if (root->right) {
            auto t = dfs(root->right);
            if (!t[0] || t[1] <= root->val) res[0] = 0;
            res[1] = min(res[1], t[1]);
            res[2] = max(res[2], t[2]);
        }
        return res;
    }
};

```
#####  LeetCode 99. 恢复二叉搜索树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void recoverTree(TreeNode* root) {
        TreeNode *first = NULL, *second, *last = NULL;
        while (root) {
            if (!root->left) {
                if (last && last->val > root->val) {
                    if (!first) first = last, second = root;
                    else second = root;
                }
                last = root;
                root = root->right;
            } else {
                auto p = root->left;
                while (p->right && p->right != root) p = p->right;
                if (!p->right) p->right = root, root = root->left;
                else {
                    p->right = NULL;
                    if (last && last->val > root->val) {
                        if (!first) first = last, second = root;
                        else second = root;
                    }
                    last = root;
                    root = root->right;
                }
            }
        }

        swap(first->val, second->val);
    }
};
```
#####  LeetCode 100. 相同的树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (!p || !q || p->val != q->val) return false;
        return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }
};
```



#####  LeetCode  101. 对称二叉树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return dfs(root->left, root->right);
    }

    bool dfs(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (!p || !q || p->val != q->val) return false;
        return dfs(p->left, q->right) && dfs(p->right, q->left);
    }
};
```
#####  LeetCode  102. 二叉树的层序遍历
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if (root) q.push(root);

        while (q.size()) {
            vector<int> level;
            int len = q.size();

            while (len -- ) {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }

            res.push_back(level);
        }

        return res;
    }
};
```
#####  LeetCode  103. 二叉树的锯齿形层次遍历
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if (root) q.push(root);

        int cnt = 0;
        while (q.size()) {
            vector<int> level;
            int len = q.size();
            while (len -- ) {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }

            if ( ++ cnt % 2 == 0) reverse(level.begin(), level.end());
            res.push_back(level);
        }

        return res;
    }
};
```
#####  LeetCode  104. 二叉树的最大深度
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```
#####  LeetCode  105. 从前序与中序遍历序列构造二叉树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> pos;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for (int i = 0; i < inorder.size(); i ++ ) pos[inorder[i]] = i;
        return build(preorder, inorder, 0, preorder.size() - 1, 0, inorder.size() - 1);
    }

    TreeNode* build(vector<int>& preorder, vector<int>& inorder, int pl, int pr, int il, int ir) {
        if (pl > pr) return NULL;
        auto root = new TreeNode(preorder[pl]);
        int k = pos[root->val];
        root->left = build(preorder, inorder, pl + 1, pl + 1 + k - 1 - il, il, k - 1);
        root->right = build(preorder, inorder, pl + 1 + k - 1 - il + 1, pr, k + 1, ir);
        return root;
    }
};
```
#####  LeetCode  106. 从中序与后序遍历序列构造二叉树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> pos;

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        for (int i = 0; i < inorder.size(); i ++ ) pos[inorder[i]] = i;
        return build(inorder, postorder, 0, inorder.size() - 1, 0, postorder.size() - 1);
    }

    TreeNode* build(vector<int>& inorder, vector<int>& postorder, int il, int ir, int pl, int pr) {
        if (il > ir) return NULL;
        auto root = new TreeNode(postorder[pr]);
        int k = pos[root->val];
        root->left = build(inorder, postorder, il, k - 1, pl, pl + k - 1 - il);
        root->right = build(inorder, postorder, k + 1, ir, pl + k - 1 - il + 1, pr - 1);
        return root;
    }
};
```
#####  LeetCode  107. 二叉树的层次遍历 II
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if (root) q.push(root);
        while (q.size()) {
            vector<int> level;
            int len = q.size();
            while (len -- ) {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
            }
            res.push_back(level);
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```
#####  LeetCode  108. 将有序数组转换为二叉搜索树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return build(nums, 0, nums.size() - 1);
    }

    TreeNode* build(vector<int>& nums, int l, int r) {
        if (l > r) return NULL;
        int mid = l + r >> 1;
        auto root = new TreeNode(nums[mid]);
        root->left = build(nums, l, mid - 1);
        root->right = build(nums, mid + 1, r);
        return root;
    }
};

```
#####  LeetCode  109. 有序链表转换二叉搜索树
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return NULL;
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;
        if (n == 1) return new TreeNode(head->val);
        auto cur = head;
        for (int i = 0; i < n / 2 - 1; i ++ ) cur = cur->next;
        auto root = new TreeNode(cur->next->val);
        root->right = sortedListToBST(cur->next->next);
        cur->next = NULL;
        root->left = sortedListToBST(head);
        return root;
    }
};
```
#####  LeetCode  110. 平衡二叉树
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool ans;

    bool isBalanced(TreeNode* root) {
        ans = true;
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root) {
        if (!root) return 0;
        int lh = dfs(root->left), rh = dfs(root->right);
        if (abs(lh - rh) > 1) ans = false;
        return max(lh, rh) + 1;
    }
};
```



### 究极班——Week 7(第 111 ~ 130 题)


##### LeetCode 111. 二叉树的最小深度
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (!root) return 0;
        if (!root->left && !root->right) return 1;
        if (root->left && root->right) return min(minDepth(root->left), minDepth(root->right)) + 1;
        if (root->left) return minDepth(root->left) + 1;
        return minDepth(root->right) + 1;
    }
};
```

##### LeetCode 112. 路径总和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root) return false;
        sum -= root->val;
        if (!root->left && !root->right) return !sum;
        return root->left && hasPathSum(root->left, sum) || root->right && hasPathSum(root->right, sum);
    }
};
```

##### LeetCode 113. 路径总和 II
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if (root) dfs(root, sum);
        return ans;
    }

    void dfs(TreeNode* root, int sum) {
        path.push_back(root->val);
        sum -= root->val;
        if (!root->left && !root->right) {
            if (!sum) ans.push_back(path);
        } else {
            if (root->left) dfs(root->left, sum);
            if (root->right) dfs(root->right, sum);
        }
        path.pop_back();
    }
};

```

##### LeetCode 114. 二叉树展开为链表
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void flatten(TreeNode* root) {
        while (root) {
            auto p = root->left;
            if (p) {
                while (p->right) p = p->right;
                p->right = root->right;
                root->right = root->left;
                root->left = NULL;
            }
            root = root->right;
        }
    }
};
```

##### LeetCode 115. 不同的子序列
```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size(), m = t.size();
        s = ' ' + s, t = ' ' + t;
        vector<vector<unsigned long long>> f(n + 1, vector<unsigned long long>(m + 1));
        for (int i = 0; i <= n; i ++ ) f[i][0] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                f[i][j] = f[i - 1][j];
                if (s[i] == t[j]) f[i][j] += f[i - 1][j - 1];
            }
        return f[n][m];
    }
};
```

##### LeetCode 116. 填充每个节点的下一个右侧节点指针
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (!root) return root;
        auto source = root;
        while (root->left) {
            for (auto p = root; p; p = p->next) {
                p->left->next = p->right;
                if (p->next) p->right->next = p->next->left;
            }
            root = root->left;
        }
        return source;
    }
};
```
##### LeetCode 117. 填充每个节点的下一个右侧节点指针 II
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (!root) return root;
        auto cur = root;
        while (cur) {
            auto head = new Node(-1);
            auto tail = head;
            for (auto p = cur; p; p = p->next) {
                if (p->left) tail = tail->next = p->left;
                if (p->right) tail = tail->next = p->right;
            }
            cur = head->next;
        }
        return root;
    }
};
```

##### LeetCode 118. 杨辉三角
```cpp
class Solution {
public:
    vector<vector<int>> generate(int n) {
        vector<vector<int>> f;
        for (int i = 0; i < n; i ++ ) {
            vector<int> line(i + 1);
            line[0] = line[i] = 1;
            for (int j = 1; j < i; j ++ )
                line[j] = f[i - 1][j - 1] + f[i - 1][j];
            f.push_back(line);
        }
        return f;
    }
};
```

##### LeetCode 119. 杨辉三角 II
```cpp
class Solution {
public:
    vector<int> getRow(int n) {
        vector<vector<int>> f(2, vector<int>(n + 1));
        for (int i = 0; i <= n; i ++ ) {
            f[i & 1][0] = f[i & 1][i] = 1;
            for (int j = 1; j < i; j ++ )
                f[i & 1][j] = f[i - 1 & 1][j - 1] + f[i - 1 & 1][j];
        }
        return f[n & 1];
    }
};
```

##### LeetCode 120. 三角形最小路径和
```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& f) {
        for (int i = f.size() - 2; i >= 0; i -- )
            for (int j = 0; j <= i; j ++ )
                f[i][j] += min(f[i + 1][j], f[i + 1][j + 1]);
        return f[0][0];
    }
};
```

##### LeetCode 121. 买卖股票的最佳时机
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        for (int i = 0, minp = INT_MAX; i < prices.size(); i ++ ) {
            res = max(res, prices[i] - minp);
            minp = min(minp, prices[i]);
        }
        return res;
    }
};
```

##### LeetCode 122. 买卖股票的最佳时机 II
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        for (int i = 0; i + 1 < prices.size(); i ++ )
            res += max(0, prices[i + 1] - prices[i]);
        return res;
    }
};
```

##### LeetCode 123. 买卖股票的最佳时机 III
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<int> f(n + 2);
        for (int i = 1, minp = INT_MAX; i <= n; i ++ ) {
            f[i] = max(f[i - 1], prices[i - 1] - minp);
            minp = min(minp, prices[i - 1]);
        }

        int res = 0;
        for (int i = n, maxp = 0; i; i -- ) {
            res = max(res, maxp - prices[i - 1] + f[i - 1]);
            maxp = max(maxp, prices[i - 1]);
        }

        return res;
    }
};
```

##### LeetCode 124. 二叉树中的最大路径和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans;

    int maxPathSum(TreeNode* root) {
        ans = INT_MIN;
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* u) {
        if (!u) return 0;
        int left = max(0, dfs(u->left)), right = max(0, dfs(u->right));
        ans = max(ans, u->val + left + right);
        return u->val + max(left, right);
    }
};

```

##### LeetCode 125. 验证回文串
```cpp
class Solution {
public:
    bool check(char c) {
        return c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z' || c >= '0' && c <= '9';
    }

    bool isPalindrome(string s) {
        for (int i = 0, j = s.size() - 1; i < j; i ++, j -- ) {
            while (i < j && !check(s[i])) i ++ ;
            while (i < j && !check(s[j])) j -- ;
            if (i < j && tolower(s[i]) != tolower(s[j])) return false;
        }

        return true;
    }
};

```

##### LeetCode 126. 单词接龙 II
```cpp
class Solution {
public:
    unordered_set<string> S;
    unordered_map<string, int> dist;
    queue<string> q;
    vector<vector<string>> ans;
    vector<string> path;
    string beginWord;

    vector<vector<string>> findLadders(string _beginWord, string endWord, vector<string>& wordList) {
        for (auto word: wordList) S.insert(word);
        beginWord = _beginWord;
        dist[beginWord] = 0;
        q.push(beginWord);
        while (q.size()) {
            auto t = q.front();
            q.pop();

            string r = t;
            for (int i = 0; i < t.size(); i ++ ) {
                t = r;
                for (char j = 'a'; j <= 'z'; j ++ ) {
                    t[i] = j;
                    if (S.count(t) && dist.count(t) == 0) {
                        dist[t] = dist[r] + 1;
                        if (t == endWord) break;
                        q.push(t);
                    }
                }
            }
        }

        if (dist.count(endWord) == 0) return ans;
        path.push_back(endWord);
        dfs(endWord);
        return ans;
    }

    void dfs(string t) {
        if (t == beginWord) {
            reverse(path.begin(), path.end());
            ans.push_back(path);
            reverse(path.begin(), path.end());
        } else {
            string r = t;
            for (int i = 0; i < t.size(); i ++ ) {
                t = r;
                for (char j = 'a'; j <= 'z'; j ++ ) {
                    t[i] = j;
                    if (dist.count(t) && dist[t] + 1 == dist[r]) {
                        path.push_back(t);
                        dfs(t);
                        path.pop_back();
                    }
                }
            }
        }
    }
};

```

##### LeetCode 127. 单词接龙
```cpp
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> S;
        for (auto& word: wordList) S.insert(word);
        unordered_map<string, int> dist;
        dist[beginWord] = 0;
        queue<string> q;
        q.push(beginWord);

        while (q.size()) {
            auto t = q.front();
            q.pop();

            string r = t;
            for (int i = 0; i < t.size(); i ++ ) {
                t = r;
                for (char j = 'a'; j <= 'z'; j ++ ) {
                    t[i] = j;
                    if (S.count(t) && !dist.count(t)) {
                        dist[t] = dist[r] + 1;
                        if (t == endWord) return dist[t] + 1;
                        q.push(t);
                    }
                }
            }
        }

        return 0;
    }
};
```

##### LeetCode 128. 最长连续序列
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> S;
        for (auto x: nums) S.insert(x);

        int res = 0;
        for (auto x: nums) {
            if (S.count(x) && !S.count(x - 1)) {
                int y = x;
                S.erase(x);
                while (S.count(y + 1)) {
                    y ++ ;
                    S.erase(y);
                }
                res = max(res, y - x + 1);
            }
        }

        return res;
    }
};
```

##### LeetCode 129. 求根到叶子节点数字之和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans = 0;

    int sumNumbers(TreeNode* root) {
        if (root) dfs(root, 0);
        return ans;
    }

    void dfs(TreeNode* root, int number) {
        number = number * 10 + root->val;
        if (!root->left && !root->right) ans += number;
        if (root->left) dfs(root->left, number);
        if (root->right) dfs(root->right, number);
    }
};
```

##### LeetCode 130. 被围绕的区域
```cpp
class Solution {
public:
    int n, m;
    vector<vector<char>> board;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    void solve(vector<vector<char>>& _board) {
        board = _board;
        n = board.size();
        if (!n) return;
        m = board[0].size();

        for (int i = 0; i < n; i ++ ) {
            if (board[i][0] == 'O') dfs(i, 0);
            if (board[i][m - 1] == 'O') dfs(i, m - 1);
        }

        for (int i = 0; i < m; i ++ ) {
            if (board[0][i] == 'O') dfs(0, i);
            if (board[n - 1][i] == 'O') dfs(n - 1, i);
        }

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (board[i][j] == '#') board[i][j] = 'O';
                else board[i][j] = 'X';

        _board = board;
    }

    void dfs(int x, int y) {
        board[x][y] = '#';
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && board[a][b] == 'O')
                dfs(a, b);
        }
    }
};
```


### 究极班-Week 8 (第 131 ~ 150 题)


##### LeetCode 131. 分割回文串
```cpp
class Solution {
public:
    vector<vector<bool>> f;
    vector<vector<string>> ans;
    vector<string> path;

    vector<vector<string>> partition(string s) {
        int n = s.size();
        f = vector<vector<bool>>(n, vector<bool>(n));
        for (int j = 0; j < n; j ++ )
            for (int i = 0; i <= j; i ++ )
                if (i == j) f[i][j] = true;
                else if (s[i] == s[j]) {
                    if (i + 1 > j - 1 || f[i + 1][j - 1]) f[i][j] = true;
                }

        dfs(s, 0);
        return ans;
    }

    void dfs(string& s, int u) {
        if (u == s.size()) ans.push_back(path);
        else {
            for (int i = u; i < s.size(); i ++ )
                if (f[u][i]) {
                    path.push_back(s.substr(u, i - u + 1));
                    dfs(s, i + 1);
                    path.pop_back();
                }
        }
    }
};
```
 
##### LeetCode 132. 分割回文串 II
```cpp
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        s = ' ' + s;
        vector<vector<bool>> g(n + 1, vector<bool>(n + 1));
        vector<int> f(n + 1, 1e8);

        for (int j = 1; j <= n; j ++ )
            for (int i = 1; i <= n; i ++ )
                if (i == j) g[i][j] = true;
                else if (s[i] == s[j]) {
                    if (i + 1 > j - 1 || g[i + 1][j - 1]) g[i][j] = true;
                }

        f[0] = 0;
        for (int i = 1; i <= n; i ++ ) {
            for (int j = 1; j <= i; j ++ )
                if (g[j][i])
                    f[i] = min(f[i], f[j - 1] + 1);
        }

        return f[n] - 1;
    }
};

```
 
##### LeetCode 133. 克隆图 
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;

    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }

    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }

    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution {
public:
    unordered_map<Node*, Node*> hash;

    Node* cloneGraph(Node* node) {
        if (!node) return NULL;
        dfs(node);  // 复制所有点

        for (auto [s, d]: hash) {
            for (auto ver: s->neighbors)
                d->neighbors.push_back(hash[ver]);
        }

        return hash[node];
    }

    void dfs(Node* node) {
        hash[node] = new Node(node->val);

        for (auto ver: node->neighbors)
            if (hash.count(ver) == 0)
                dfs(ver);
    }
};
```
##### LeetCode 134. 加油站 
```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        for (int i = 0, j; i < n; ) {  // 枚举起点
            int left = 0;
            for (j = 0; j < n; j ++ ) {
                int k = (i + j) % n;
                left += gas[k] - cost[k];
                if (left < 0) break;
            }
            if (j == n) return i;
            i = i + j + 1;
        }

        return -1;
    }
};

```
##### LeetCode 135. 分发糖果 
```cpp
class Solution {
public:
    vector<int> f;
    vector<int> w;
    int n;

    int candy(vector<int>& ratings) {
        n = ratings.size();
        w = ratings;
        f.resize(n, -1);

        int res = 0;
        for (int i = 0; i < n; i ++ ) res += dp(i);
        return res;
    }

    int dp(int x) {
        if (f[x] != -1) return f[x];
        f[x] = 1;
        if (x && w[x - 1] < w[x]) f[x] = max(f[x], dp(x - 1) + 1);
        if (x + 1 < n && w[x + 1] < w[x]) f[x] = max(f[x], dp(x + 1) + 1);
        return f[x];
    }
};

``` 
##### LeetCode 136. 只出现一次的数字 
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = 0;
        for (auto x: nums) res ^= x;
        return res;
    }
};
```
##### LeetCode 137. 只出现一次的数字 II 
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int two = 0, one = 0;
        for (auto x: nums) {
            one = (one ^ x) & ~two;
            two = (two ^ x) & ~one;
        }
        return one;
    }
};
```
##### LeetCode 138. 复制带随机指针的链表 
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node(int _val) {
        val = _val;
        next = NULL; 
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        for (auto p = head; p; p = p->next->next) {  // 复制一个小弟
            auto q = new Node(p->val);
            q->next = p->next;
            p->next = q;
        }

        // 复制random指针
        for (auto p = head; p; p = p->next->next)
            if (p->random)
                p->next->random = p->random->next;

        // 拆分两个链表
        auto dummy = new Node(-1), cur = dummy;
        for (auto p = head; p; p = p->next) {
            auto q = p->next;
            cur = cur->next = q;
            p->next = q->next;
        }
        return dummy->next;
    }
};
```
##### LeetCode 139. 单词拆分 
```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        typedef unsigned long long ULL;
        const int P = 131;
        unordered_set<ULL> hash;
        for (auto& word: wordDict) {
            ULL h = 0;
            for (auto c: word) h = h * P + c;
            hash.insert(h);
        }

        int n = s.size();
        vector<bool> f(n + 1);
        f[0] = true;
        s = ' ' + s;
        for (int i = 0; i < n; i ++ )
            if (f[i]) {
                ULL h = 0;
                for (int j = i + 1; j <= n; j ++ ) {
                    h = h * P + s[j];
                    if (hash.count(h)) f[j] = true;
                }
            }

        return f[n];
    }
};
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        typedef unsigned long long ULL;
        unordered_set<ULL> S;
        const int P = 131;

        for (auto& word: wordDict) {
            ULL h = 0;
            for (auto c: word) h = h * P + c;
            S.insert(h);
        }

        int n = s.size();
        vector<bool> f(n + 1);
        f[n] = true;

        for (int i = n - 1; i >= 0; i -- ) {
            ULL h = 0;
            for (int j = i; j < n; j ++ ) {
                h = h * P + s[j];
                if (S.count(h) && f[j + 1]) {
                    f[i] = true;
                    break;
                }
            }
        }

        return f[0];
    }
};


```
##### LeetCode 140. 单词拆分 II 
```cpp
class Solution {
public:
    vector<bool> f;
    vector<string> ans;
    unordered_set<string> hash;
    int n;

    vector<string> wordBreak(string s, vector<string>& wordDict) {
        n = s.size();
        f.resize(n + 1);
        for (auto word: wordDict) hash.insert(word);
        f[n] = true;
        for (int i = n - 1; ~i; i -- )
            for (int j = i; j < n; j ++ )
                if (hash.count(s.substr(i, j - i + 1)) && f[j + 1])
                    f[i] = true;

        dfs(s, 0, "");
        return ans;
    }

    void dfs(string& s, int u, string path) {
        if (u == n) {
            path.pop_back();
            ans.push_back(path);
        } else {
            for (int i = u; i < n; i ++ )
                if (hash.count(s.substr(u, i - u + 1)) && f[i + 1])
                    dfs(s, i + 1, path + s.substr(u, i - u + 1) + ' ');
        }
    }
};

```
##### LeetCode 141. 环形链表 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (!head || !head->next) return false;
        auto s = head, f = head->next;
        while (f) {
            s = s->next, f = f->next;
            if (!f) return false;
            f = f->next;
            if (s == f) return true;
        }
        return false;
    }
};

```
##### LeetCode 142. 环形链表 II 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (!head || !head->next) return NULL;
        auto s = head, f = head->next;
        while (f) {
            s = s->next, f = f->next;
            if (!f) return NULL;
            f = f->next;
            if (s == f) {
                s = head, f = f->next;
                while (s != f) s = s->next, f = f->next;
                return s;
            }
        }
        return NULL;
    }
};

```
##### LeetCode 143. 重排链表 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    void reorderList(ListNode* head) {
        if (!head) return;
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;

        auto mid = head;
        for (int i = 0; i < (n + 1) / 2 - 1; i ++ ) mid = mid->next;
        auto a = mid, b = a->next;
        for (int i = 0; i < n / 2; i ++ ) {
            auto c = b->next;
            b->next = a, a = b, b = c;
        }
        auto p = head, q = a;
        for (int i = 0; i < n / 2; i ++ ) {
            auto o = q->next;
            q->next = p->next;
            p->next = q;
            p = q->next, q = o;
        }

        if (n % 2) mid->next = NULL;
        else mid->next->next = NULL;
    }
};

```
##### LeetCode 144. 二叉树的前序遍历 
```cpp

递归写法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> ans;

    vector<int> preorderTraversal(TreeNode* root) {
        dfs(root);
        return ans;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        ans.push_back(root->val);
        dfs(root->left);
        dfs(root->right);
    }
};

迭代写法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        while (root || stk.size()) {
            while (root) {
                res.push_back(root->val);
                stk.push(root);
                root = root->left;
            }

            root = stk.top()->right;
            stk.pop();
        }
        return res;
    }
};


```

##### LeetCode 145. 二叉树的后序遍历 
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        while (root || stk.size()) {
            while (root) {
                res.push_back(root->val);
                stk.push(root);
                root = root->right;
            }

            root = stk.top()->left;
            stk.pop();
        }

        reverse(res.begin(), res.end());
        return res;
    }
};

```
##### LeetCode 146. LRU缓存机制 
```cpp
class LRUCache {
public:
    struct Node {
        int key, val;
        Node *left, *right;
        Node(int _key, int _val): key(_key), val(_val), left(NULL), right(NULL) {}
    }*L, *R;
    unordered_map<int, Node*> hash;
    int n;

    void remove(Node* p) {
        p->right->left = p->left;
        p->left->right = p->right;
    }

    void insert(Node* p) {
        p->right = L->right;
        p->left = L;
        L->right->left = p;
        L->right = p;
    }

    LRUCache(int capacity) {
        n = capacity;
        L = new Node(-1, -1), R = new Node(-1, -1);
        L->right = R, R->left = L;
    }

    int get(int key) {
        if (hash.count(key) == 0) return -1;
        auto p = hash[key];
        remove(p);
        insert(p);
        return p->val;
    }

    void put(int key, int value) {
        if (hash.count(key)) {
            auto p = hash[key];
            p->val = value;
            remove(p);
            insert(p);
        } else {
            if (hash.size() == n) {
                auto p = R->left;
                remove(p);
                hash.erase(p->key);
                delete p;
            }
            auto p = new Node(key, value);
            hash[key] = p;
            insert(p);
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```
##### LeetCode 147. 对链表进行插入排序 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        auto dummy = new ListNode(-1);
        for (auto p = head; p;) {
            auto cur = dummy, next = p->next;
            while (cur->next && cur->next->val <= p->val) cur = cur->next;
            p->next = cur->next;
            cur->next = p;
            p = next;
        }
        return dummy->next;
    }
};

```
##### LeetCode 148. 排序链表 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;

        for (int i = 1; i < n; i *= 2) {
            auto dummy = new ListNode(-1), cur = dummy;
            for (int j = 1; j <= n; j += i * 2) {
                auto p = head, q = p;
                for (int k = 0; k < i && q; k ++ ) q = q->next;
                auto o = q;
                for (int k = 0; k < i && o; k ++ ) o = o->next;
                int l = 0, r = 0;
                while (l < i && r < i && p && q)
                    if (p->val <= q->val) cur = cur->next = p, p = p->next, l ++ ;
                    else cur = cur->next = q, q = q->next, r ++ ;
                while (l < i && p) cur = cur->next = p, p = p->next, l ++ ;
                while (r < i && q) cur = cur->next = q, q = q->next, r ++ ;
                head = o;
            }
            cur->next = NULL;
            head = dummy->next;
        }

        return head;
    }
};

```
##### LeetCode 149. 直线上最多的点数 
```cpp
class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        typedef long double LD;

        int res = 0;
        for (auto& p: points) {
            int ss = 0, vs = 0;
            unordered_map<LD, int> cnt;
            for (auto& q: points)
                if (p == q) ss ++ ;
                else if (p[0] == q[0]) vs ++ ;
                else {
                    LD k = (LD)(q[1] - p[1]) / (q[0] - p[0]);
                    cnt[k] ++ ;
                }
            int c = vs;
            for (auto [k, t]: cnt) c = max(c, t);
            res = max(res, c + ss);
        }
        return res;
    }
};

```
##### LeetCode 150. 逆波兰表达式求值
```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> stk;
        for (auto& s: tokens)
            if (s == "+" || s == "-" || s == "*" || s == "/") {
                auto b = stk.top(); stk.pop();
                auto a = stk.top(); stk.pop();
                if (s == "+") a += b;
                else if (s == "-") a -= b;
                else if (s == "*") a *= b;
                else a /= b;
                stk.push(a);
            } else stk.push(stoi(s));
        return stk.top();
    }
};
```



### 究极班——Week9（第 151 ~ 155、160、162、164 ~ 169、171 ~ 174、179、187、188 题）

#####  LeetCode  151. 翻转字符串里的单词
```C++
class Solution {
public:
    string reverseWords(string s) {
        int k = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == ' ') continue;
            int j = i, t = k;
            while (j < s.size() && s[j] != ' ') s[t ++ ] = s[j ++ ];
            reverse(s.begin() + k, s.begin() + t);
            s[t ++ ] = ' ';
            k = t, i = j;
        }
        if (k) k -- ;
        s.erase(s.begin() + k, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};

```
#####  LeetCode  152. 乘积最大子数组
```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int res = nums[0];
        int f = nums[0], g = nums[0];
        for (int i = 1; i < nums.size(); i ++ ) {
            int a = nums[i], fa = f * a, ga = g * a;
            f = max(a, max(fa, ga));
            g = min(a, min(fa, ga));
            res = max(res, f);
        }
        return res;
    }
};
```
#####  LeetCode  153. 寻找旋转排序数组中的最小值
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        if (nums[r] >= nums[l]) return nums[0];
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] < nums[0]) r = mid;
            else l = mid + 1;
        }
        return nums[r];
    }
};

```
#####  LeetCode  154. 寻找旋转排序数组中的最小值 II
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r && nums[r] == nums[0]) r -- ;
        if (nums[l] <= nums[r]) return nums[0];
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] < nums[0]) r = mid;
            else l = mid + 1;
        }
        return nums[r];
    }
};

```
#####  LeetCode  155. 最小栈
```C++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> stk, f;
    MinStack() {

    }

    void push(int x) {
        stk.push(x);
        if (f.empty() || f.top() >= x) f.push(x);
    }

    void pop() {
        if (stk.top() <= f.top()) f.pop();
        stk.pop();
    }

    int top() {
        return stk.top();
    }

    int getMin() {
        return f.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */

```
#####  LeetCode  160. 相交链表
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        auto p = headA, q = headB;
        while (p != q) {
            p = p ? p->next : headB;
            q = q ? q->next : headA;
        }
        return p;
    }
};
```

#####  LeetCode  162. 寻找峰值
```C++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] > nums[mid + 1]) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```

#####  LeetCode  164. 最大间距
```C++
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        struct Range {
            int min, max;
            bool used;
            Range() : min(INT_MAX), max(INT_MIN), used(false){}
        };
        int n = nums.size();
        int Min = INT_MAX, Max = INT_MIN;
        for (auto x: nums) {
            Min = min(Min, x);
            Max = max(Max, x);
        }
        if (n < 2 || Min == Max) return 0;
        vector<Range> r(n - 1);
        int len = (Max - Min + n - 2) / (n - 1);
        for (auto x: nums) {
            if (x == Min) continue;
            int k = (x - Min - 1) / len;
            r[k].used = true;
            r[k].min = min(r[k].min, x);
            r[k].max = max(r[k].max, x);
        }
        int res = 0;
        for (int i = 0, last = Min; i < n - 1; i ++ )
            if (r[i].used) {
                res = max(res, r[i].min - last);
                last = r[i].max;
            }
        return res;
    }
};

```

#####  LeetCode  165. 比较版本号
```C++
class Solution {
public:
    int compareVersion(string v1, string v2) {
        for (int i = 0, j = 0; i < v1.size() || j < v2.size();) {
            int a = i, b = j;
            while (a < v1.size() && v1[a] != '.') a ++ ;
            while (b < v2.size() && v2[b] != '.') b ++ ;
            int x = a == i ? 0 : stoi(v1.substr(i, a - i));
            int y = b == j ? 0 : stoi(v2.substr(j, b - j));
            if (x > y) return 1;
            if (x < y) return -1;
            i = a + 1, j = b + 1;
        }
        return 0;
    }
};

```
#####  LeetCode  166. 分数到小数
```C++
class Solution {
public:
    string fractionToDecimal(int numerator, int denominator) {
        typedef long long LL;
        LL x = numerator, y = denominator;
        if (x % y == 0) return to_string(x / y);
        string res;
        if ((x < 0) ^ (y < 0)) res += '-';
        x = abs(x), y = abs(y);
        res += to_string(x / y) + '.', x %= y;
        unordered_map<LL, int> hash;
        while (x) {
            hash[x] = res.size();
            x *= 10;
            res += to_string(x / y);
            x %= y;
            if (hash.count(x)) {
                res = res.substr(0, hash[x]) + '(' + res.substr(hash[x]) + ')';
                break;
            }
        }
        return res;
    }
};

```


#####  LeetCode 167. 两数之和 II - 输入有序数组
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for (int i = 0, j = numbers.size() - 1; i < j; i ++ ) {
            while (i < j && numbers[i] + numbers[j] > target) j -- ;
            if (i < j && numbers[i] + numbers[j] == target) return {i + 1, j + 1};
        }
        return {};
    }
};
```
#####  LeetCode 168. Excel表列名称
```cpp
class Solution {
public:
    string convertToTitle(int n) {
        int k = 1;
        for (long long p = 26; n > p; p *= 26) {
            n -= p;
            k ++ ;
        }

        n -- ;
        string res;
        while (k -- ) {
            res += n % 26 + 'A';
            n /= 26;
        }

        reverse(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode 169. 多数元素
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int r, c = 0;
        for (auto x: nums)
            if (!c) r = x, c = 1;
            else if (r == x) c ++ ;
            else c -- ;
        return r;
    }
};
```
#####  LeetCode 171. Excel表列序号
```cpp
class Solution {
public:
    int titleToNumber(string s) {
        int a = 0;
        for (long long i = 0, p = 26; i < s.size() - 1; i ++, p *= 26)
            a += p;

        int b = 0;
        for (auto c: s) b = b * 26 + c - 'A';
        return a + b + 1;
    }
};
```
#####  LeetCode 172. 阶乘后的零
```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        int res = 0;
        while (n) res += n / 5, n /= 5;
        return res;
    }
};
```
#####  LeetCode 173. 二叉搜索树迭代器
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class BSTIterator {
public:
    stack<TreeNode*> stk;

    BSTIterator(TreeNode* root) {
        while (root) {
            stk.push(root);
            root = root->left;
        }
    }

    /** @return the next smallest number */
    int next() {
        auto root = stk.top();
        stk.pop();
        int val = root->val;
        root = root->right;
        while (root) {
            stk.push(root);
            root = root->left;
        }
        return val;
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return stk.size();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
 
```
#####  LeetCode 174. 地下城游戏
```cpp
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& w) {
        int n = w.size(), m = w[0].size();
        vector<vector<int>> f(n, vector<int>(m, 1e8));

        for (int i = n - 1; i >= 0; i -- )
            for (int j = m - 1; j >= 0; j -- )
                if (i == n - 1 && j == m - 1) f[i][j] = max(1, 1 - w[i][j]);
                else {
                    if (i + 1 < n) f[i][j] = f[i + 1][j] - w[i][j];
                    if (j + 1 < m) f[i][j] = min(f[i][j], f[i][j + 1] - w[i][j]);
                    f[i][j] = max(1, f[i][j]);
                }

        return f[0][0];
    }
};
```
#####  LeetCode 179. 最大数
```cpp
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end(), [](int x, int y) {
            string a = to_string(x), b = to_string(y);
            return a + b > b + a;
        });
        string res;
        for (auto x: nums) res += to_string(x);
        int k = 0;
        while (k + 1 < res.size() && res[k] == '0') k ++ ;
        return res.substr(k);
    }
};

```
#####  LeetCode 187. 重复的DNA序列
```cpp
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        unordered_map<string, int> cnt;
        for (int i = 0; i + 10 <= s.size(); i ++ )
            cnt[s.substr(i, 10)] ++ ;
        vector<string> res;
        for (auto [s, c]: cnt)
            if (c > 1)
                res.push_back(s);
        return res;
    }
};

```
#####  LeetCode 188. 买卖股票的最佳时机 IV

```cpp
注意
LeetCode增强了本题的数据，非常卡常。

于是为了应对新数据，对视频中的代码做了如下优化：

将vector换成了数组，大概会快50%。
类似于背包问题优化空间，将原本的滚动二维数组，直接换成一维数组。
C++代码
int f[10001], g[10001];

class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int INF = 1e8;
        int n = prices.size();
        if (k > n / 2) {
            int res = 0;
            for (int i = 1; i < n; i ++ )
                if (prices[i] > prices[i - 1])
                    res += prices[i] - prices[i - 1];
            return res;
        }
        memset(f, -0x3f, sizeof f);
        memset(g, -0x3f, sizeof g);
        f[0] = 0;
        int res = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = k; j >= 0; j -- ) {
                g[j] = max(g[j], f[j] - prices[i - 1]);
                if (j) f[j] = max(f[j], g[j - 1] + prices[i - 1]);
            }
        for (int i = 1; i <= k; i ++ ) res = max(res, f[i]);
        return res;
    }
};
```


### 究极班- Week 10(第 189 ~ 191、198 ~ 214 题)


##### LeetCode 189. 旋转数组
```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k %= n;
        reverse(nums.begin(), nums.end());
        reverse(nums.begin(), nums.begin() + k);
        reverse(nums.begin() + k, nums.end());
    }
};
```
##### LeetCode 190. 颠倒二进制位
```cpp
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t res = 0;
        for (int i = 0; i < 32; i ++ )
            res = res * 2 + (n >> i & 1);
        return res;
    }
};
```
##### LeetCode 191. 位1的个数

```cpp
算法1
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        for (int i = 0; i < 32; i ++ )
            res += n >> i & 1;
        return res;
    }
};

算法2
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while (n) n -= n & -n, res ++ ;
        return res;
    }
};

```
##### LeetCode 198. 打家劫舍

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n + 1), g(n + 1);
        for (int i = 1; i <= n; i ++ ) {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        return max(f[n], g[n]);
    }
};
```
##### LeetCode 199. 二叉树的右视图
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        queue<TreeNode*> q;
        vector<int> res;
        if (!root) return res;
        q.push(root);
        while (q.size()) {
            int len = q.size();
            for (int i = 0; i < len; i ++ ) {
                auto t = q.front();
                q.pop();
                if (t->left) q.push(t->left);
                if (t->right) q.push(t->right);
                if (i == len - 1) res.push_back(t->val);
            }
        }
        return res;
    }
};
```
##### LeetCode 200. 岛屿数量
```cpp
class Solution {
public:
    vector<vector<char>> g;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int numIslands(vector<vector<char>>& grid) {
        g = grid;
        int cnt = 0;
        for (int i = 0; i < g.size(); i ++ )
            for (int j = 0; j < g[i].size(); j ++ )
                if (g[i][j] == '1') {
                    dfs(i, j);
                    cnt ++ ;
                }
        return cnt;
    }

    void dfs(int x, int y) {
        g[x][y] = 0;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < g.size() && b >= 0 && b < g[a].size() && g[a][b] == '1')
                dfs(a, b);
        }
    }
};

```
##### LeetCode 201. 数字范围按位与
```cpp
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int res = 0;
        for (int i = 30; i >= 0; i -- ) {
            if ((m >> i & 1) != (n >> i & 1)) break;
            if (m >> i & 1) res += 1 << i;
        }
        return res;
    }
};
```
##### LeetCode 202. 快乐数
```cpp
class Solution {
public:
    int get(int x) {
        int res = 0;
        while (x) {
            res += (x % 10) * (x % 10);
            x /= 10;
        }
        return res;
    }

    bool isHappy(int n) {
        int fast = get(n), slow = n;
        while (fast != slow) {
            fast = get(get(fast));
            slow = get(slow);
        }
        return fast == 1;
    }
};
```
##### LeetCode 203. 移除链表元素
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        for (auto p = dummy; p; p = p->next) {
            auto q = p->next;
            while (q && q->val == val) q = q->next;
            p->next = q;
        }
        return dummy->next;
    }
};

```
##### LeetCode 204. 计数质数
```cpp
class Solution {
public:
    int countPrimes(int n) {
        vector<bool> st(n + 1);
        vector<int> primes;
        for (int i = 2; i < n; i ++ ) {
            if (!st[i]) primes.push_back(i);
            for (int j = 0; i * primes[j] < n; j ++ ) {
                st[i * primes[j]] = true;
                if (i % primes[j] == 0) break;
            }
        }
        return primes.size();
    }
};
```
##### LeetCode 205. 同构字符串
```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char, char> st, ts;
        for (int i = 0; i < s.size(); i ++ )
        {
            int a = s[i], b = t[i];
            if (st.count(a) && st[a] != b) return false;
            st[a] = b;
            if (ts.count(b) && ts[b] != a) return false;
            ts[b] = a;
        }
        return true;
    }
};
```
##### LeetCode 206. 反转链表
```cpp
迭代写法
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head) return NULL;
        auto a = head, b = a->next;
        while (b) {
            auto c = b->next;
            b->next = a;
            a = b;
            b = c;
        }
        head->next = NULL;
        return a;
    }
};

递归写法
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) return head;
        auto tail = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return tail;
    }
};
```
##### LeetCode 207. 课程表
```cpp
class Solution {
public:
    bool canFinish(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        vector<int> d(n);
        for (auto& e: edges) {
            int b = e[0], a = e[1];
            g[a].push_back(b);
            d[b] ++ ;
        }

        queue<int> q;
        for (int i = 0; i < n; i ++ )
            if (d[i] == 0)
                q.push(i);

        int cnt = 0;
        while (q.size()) {
            auto t = q.front();
            q.pop();
            cnt ++ ;
            for (auto i : g[t])
                if ( -- d[i] == 0)
                    q.push(i);
        }

        return cnt == n;
    }
};
```


##### LeetCode 208. 实现 Trie (前缀树)
```cpp
class Trie {
public:
    struct Node {
        bool is_end;
        Node *son[26];
        Node() {
            is_end = false;
            for (int i = 0; i < 26; i ++ )
                son[i] = NULL;
        }
    }*root;

    /** Initialize your data structure here. */
    Trie() {
        root = new Node();
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        auto p = root;
        for (auto c: word) {
            int u = c - 'a';
            if (!p->son[u]) p->son[u] = new Node();
            p = p->son[u];
        }
        p->is_end = true;
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        auto p = root;
        for (auto c: word) {
            int u = c - 'a';
            if (!p->son[u]) return false;
            p = p->son[u];
        }
        return p->is_end;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string word) {
        auto p = root;
        for (auto c: word) {
            int u = c - 'a';
            if (!p->son[u]) return false;
            p = p->son[u];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */

```
##### LeetCode 209. 长度最小的子数组
```cpp
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int res = INT_MAX;
        for (int i = 0, j = 0, sum = 0; i < nums.size(); i ++ ) {
            sum += nums[i];
            while (sum - nums[j] >= s) sum -= nums[j ++ ];
            if (sum >= s) res = min(res, i - j + 1);
        }
        if (res == INT_MAX) res = 0;
        return res;
    }
};
```
##### LeetCode 210. 课程表 II
```cpp
class Solution {
public:
    vector<int> findOrder(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        vector<int> d(n);
        for (auto& e: edges) {
            int b = e[0], a = e[1];
            g[a].push_back(b);
            d[b] ++ ;
        }
        queue<int> q;
        for (int i = 0; i < n; i ++ )
            if (d[i] == 0)
                q.push(i);

        vector<int> res;
        while (q.size()) {
            auto t = q.front();
            q.pop();
            res.push_back(t);
            for (int i: g[t])
                if ( -- d[i] == 0)
                    q.push(i);
        }
        if (res.size() < n) res = {};
        return res;
    }
};

```
##### LeetCode 211. 添加与搜索单词 - 数据结构设计
```cpp
class WordDictionary {
public:
    struct Node {
        bool is_end;
        Node *son[26];
        Node() {
            is_end = false;
            for (int i = 0; i < 26; i ++ ) son[i] = NULL;
        }
    }*root;

    /** Initialize your data structure here. */
    WordDictionary() {
        root = new Node();
    }

    /** Adds a word into the data structure. */
    void addWord(string word) {
        auto p = root;
        for (auto c: word) {
            int u = c - 'a';
            if (!p->son[u]) p->son[u] = new Node();
            p = p->son[u];
        }
        p->is_end = true;
    }

    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool search(string word) {
        return dfs(root, word, 0);
    }

    bool dfs(Node* p, string& word, int i) {
        if (i == word.size()) return p->is_end;
        if (word[i] != '.') {
            int u = word[i] - 'a';
            if (!p->son[u]) return false;
            return dfs(p->son[u], word, i + 1);
        } else {
            for (int j = 0; j < 26; j ++ )
                if (p->son[j] && dfs(p->son[j], word, i + 1))
                    return true;
            return false;
        }
    }
};

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary* obj = new WordDictionary();
 * obj->addWord(word);
 * bool param_2 = obj->search(word);
 */
```
##### LeetCode 212. 单词搜索 II
```cpp
class Solution {
public:
    struct Node {
        int id;
        Node *son[26];
        Node() {
            id = -1;
            for (int i = 0; i < 26; i ++ ) son[i] = NULL;
        }
    }*root;
    unordered_set<int> ids;
    vector<vector<char>> g;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    void insert(string& word, int id) {
        auto p = root;
        for (auto c: word) {
            int u = c - 'a';
            if (!p->son[u]) p->son[u] = new Node();
            p = p->son[u];
        }
        p->id = id;
    }

    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        g = board;
        root = new Node();
        for (int i = 0; i < words.size(); i ++ ) insert(words[i], i);

        for (int i = 0; i < g.size(); i ++ )
            for (int j = 0; j < g[i].size(); j ++ ) {
                int u = g[i][j] - 'a';
                if (root->son[u])
                    dfs(i, j, root->son[u]);
            }

        vector<string> res;
        for (auto id: ids) res.push_back(words[id]);
        return res;
    }

    void dfs(int x, int y, Node* p) {
        if (p->id != -1) ids.insert(p->id);
        char t = g[x][y];
        g[x][y] = '.';
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < g.size() && b >= 0 && b < g[0].size() && g[a][b] != '.') {
                int u = g[a][b] - 'a';
                if (p->son[u]) dfs(a, b, p->son[u]);
            }
        }
        g[x][y] = t;
    }
};

```
##### LeetCode 213. 打家劫舍 II
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (!n) return 0;
        if (n == 1) return nums[0];
        vector<int> f(n + 1), g(n + 1);
        for (int i = 2; i <= n; i ++ ) {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }
        int res = max(f[n], g[n]);
        f[1] = nums[0];
        g[1] = INT_MIN;
        for (int i = 2; i <= n; i ++ ) {
            f[i] = g[i - 1] + nums[i - 1];
            g[i] = max(f[i - 1], g[i - 1]);
        }

        return max(res, g[n]);
    }
};

```
##### LeetCode 214. 最短回文串
```cpp
class Solution {
public:
    string shortestPalindrome(string s) {
        string t(s.rbegin(), s.rend());
        int n = s.size();
        s = ' ' + s + '#' + t;
        vector<int> ne(n * 2 + 2);
        for (int i = 2, j = 0; i <= n * 2 + 1; i ++ ) {
            while (j && s[i] != s[j + 1]) j = ne[j];
            if (s[i] == s[j + 1]) j ++ ;
            ne[i] = j;
        }
        int len = ne[n * 2 + 1];
        string left = s.substr(1, len), right = s.substr(1 + len, n - len);
        return string(right.rbegin(), right.rend()) + left + right;
    }
};

```



### 究极班- Week 11（第 215 ~ 234 题）

##### LeetCode 215. 数组中的第K个最大元素
```cpp
class Solution {
public:

    int quick_sort(vector<int>& nums, int l, int r, int k) {
        if (l == r) return nums[k];
        int x = nums[l], i = l - 1, j = r + 1;
        while (i < j) {
            do i ++ ; while (nums[i] > x);
            do j -- ; while (nums[j] < x);
            if (i < j) swap(nums[i], nums[j]);
        }
        if (k <= j) return quick_sort(nums, l, j, k);
        else return quick_sort(nums, j + 1, r, k);
    }

    int findKthLargest(vector<int>& nums, int k) {
        return quick_sort(nums, 0, nums.size() - 1, k - 1);
    }
};
```
##### LeetCode 216. 组合总和 III
```cpp
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(1, n, k);
        return ans;
    }

    void dfs(int start, int n, int k) {
        if (!n) {
            if (!k) ans.push_back(path);
        } else if (k) {
            for (int i = start; i <= 9; i ++ )
                if (n >= i) {
                    path.push_back(i);
                    dfs(i + 1, n - i, k - 1);
                    path.pop_back();
                }
        }
    }
};
```
##### LeetCode 217. 存在重复元素
```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> S;
        for (auto x: nums)
            if (S.count(x)) return true;
            else S.insert(x);
        return false;
    }
};
```
##### LeetCode 218. 天际线问题
```cpp
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        vector<vector<int>> res;
        vector<pair<int, int>> points;
        multiset<int> heights;
        for (auto& b: buildings) {
            points.push_back({b[0], -b[2]});
            points.push_back({b[1], b[2]});
        }
        sort(points.begin(), points.end());
        heights.insert(0);
        for (auto& p: points) {
            int x = p.first, h = abs(p.second);
            if (p.second < 0) {  // 左端点
                if (h > *heights.rbegin())
                    res.push_back({x, h});
                heights.insert(h);
            } else {  // 右端点
                heights.erase(heights.find(h));
                if (h > *heights.rbegin())
                    res.push_back({x, *heights.rbegin()});
            }
        }

        return res;
    }
};
```
##### LeetCode 219. 存在重复元素 II
```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        for (int i = 0; i < nums.size(); i ++ ) {
            int x = nums[i];
            if (hash.count(x) && i - hash[x] <= k) return true;
            hash[x] = i;
        }
        return false;
    }
};
```
##### LeetCode 220. 存在重复元素 III
```cpp
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        typedef long long LL;
        multiset<LL> S;
        S.insert(1e18), S.insert(-1e18);
        for (int i = 0, j = 0; i < nums.size(); i ++ ) {
            if (i - j > k) S.erase(S.find(nums[j ++ ]));
            int x = nums[i];
            auto it = S.lower_bound(x);
            if (*it - x <= t) return true;
            -- it;
            if (x - *it <= t) return true;
            S.insert(x);
        }
        return false;
    }
};
```
##### LeetCode 221. 最大正方形
```cpp
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return 0;
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<int>> f(n + 1, vector<int>(m + 1));

        int res = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                if (matrix[i - 1][j - 1] == '1') {
                    f[i][j] = min(f[i - 1][j], min(f[i][j - 1], f[i - 1][j - 1])) + 1;
                    res = max(res, f[i][j]);
                }

        return res * res;
    }
};
```
##### LeetCode 222. 完全二叉树的节点个数
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (!root) return 0;
        auto l = root->left, r = root->right;
        int x = 1, y = 1;
        while (l) l = l->left, x ++ ;
        while (r) r = r->right, y ++ ;
        if (x == y) return (1 << x) - 1;
        return countNodes(root->left) + 1 + countNodes(root->right);
    }
};
```
##### LeetCode 223. 矩形面积
```cpp
typedef long long LL;

class Solution {
public:
    int computeArea(LL A, LL B, LL C, LL D, LL E, LL F, LL G, LL H) {
        LL X = max(0ll, min(C, G) - max(A, E));
        LL Y = max(0ll, min(D, H) - max(B, F));
        return (C - A) * (D - B) + (G - E) * (H - F) - X * Y;
    }
};
```
##### LeetCode 224. 基本计算器
```cpp
class Solution {
public:
    void eval(stack<int>& num, stack<char>& op) {
        auto b = num.top(); num.pop();
        auto a = num.top(); num.pop();
        auto c = op.top(); op.pop();
        int r;
        if (c == '+') r = a + b;
        else r = a - b;
        num.push(r);
    }

    int calculate(string rs) {
        string s;
        for (auto c: rs)
            if (c != ' ')
                s += c;
        stack<int> num;
        stack<char> op;
        for (int i = 0; i < s.size(); i ++ ) {
            auto c = s[i];
            if (c == ' ') continue;
            if (isdigit(c)) {
                int x = 0, j = i;
                while (j < s.size() && isdigit(s[j])) x = x * 10 + (s[j ++ ] - '0');
                i = j - 1;
                num.push(x);
            } else if (c == '(') op.push(c);
            else if (c == ')') {
                while (op.top() != '(') eval(num, op);
                op.pop();
            } else {
                if (!i || s[i - 1] == '(' || s[i - 1] == '+' || s[i - 1] == '-')  // 特殊处理符号和正号
                    num.push(0);
                while (op.size() && op.top() != '(') eval(num, op);
                op.push(c);
            }
        }
        while (op.size()) eval(num, op);
        return num.top();
    }
};
```
##### LeetCode 225. 用队列实现栈
```cpp
class MyStack {
public:
    /** Initialize your data structure here. */

    queue<int> q, w;
    MyStack() {

    }

    /** Push element x onto stack. */
    void push(int x) {
        q.push(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        while (q.size() > 1) w.push(q.front()), q.pop();
        int t = q.front();
        q.pop();
        while (w.size()) q.push(w.front()), w.pop();
        return t;
    }

    /** Get the top element. */
    int top() {
        while (q.size() > 1) w.push(q.front()), q.pop();
        int t = q.front();
        q.pop();
        while (w.size()) q.push(w.front()), w.pop();
        q.push(t);
        return t;
    }

    /** Returns whether the stack is empty. */
    bool empty() {
        return q.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */

```
##### LeetCode 226. 翻转二叉树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root) return NULL;
        swap(root->left, root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```
##### LeetCode 227. 基本计算器 II
```cpp
class Solution {
public:
    stack<int> num;
    stack<char> op;

    void eval() {
        int b = num.top(); num.pop();
        int a = num.top(); num.pop();
        char c = op.top(); op.pop();
        int r;
        if (c == '+') r = a + b;
        else if (c == '-') r = a - b;
        else if (c == '*') r = a * b;
        else r = a / b;
        num.push(r);
    }

    int calculate(string s) {
        unordered_map<char, int> pr;
        pr['+'] = pr['-'] = 1, pr['*'] = pr['/'] = 2;
        for (int i = 0; i < s.size(); i ++ ) {
            char c = s[i];
            if (c == ' ') continue;
            if (isdigit(c)) {
                int x = 0, j = i;
                while (j < s.size() && isdigit(s[j])) x = x * 10 + (s[j ++ ] - '0');
                num.push(x);
                i = j - 1;
            } else {
                while (op.size() && pr[op.top()] >= pr[c]) eval();
                op.push(c);
            }
        }
        while (op.size()) eval();
        return num.top();
    }
};

```
##### LeetCode 228. 汇总区间
```cpp
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> res;
        for (int i = 0; i < nums.size(); i ++ ) {
            int j = i + 1;
            while (j < nums.size() && nums[j] == nums[j - 1] + 1) j ++ ;
            if (j == i + 1) res.push_back(to_string(nums[i]));
            else res.push_back(to_string(nums[i]) + "->" + to_string(nums[j - 1]));
            i = j - 1;
        }
        return res;
    }
};

```
##### LeetCode 229. 求众数 II
```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int r1, r2, c1 = 0, c2 = 0;
        for (auto x: nums)
            if (c1 && x == r1) c1 ++ ;
            else if (c2 && x == r2) c2 ++ ;
            else if (!c1) r1 = x, c1 ++ ;
            else if (!c2) r2 = x, c2 ++ ;
            else c1 --, c2 -- ;
        c1 = 0, c2 = 0;
        for (auto x: nums)
            if (x == r1) c1 ++ ;
            else if (x == r2) c2 ++ ;

        vector<int> res;
        int n = nums.size();
        if (c1 > n / 3) res.push_back(r1);
        if (c2 > n / 3) res.push_back(r2);
        return res;
    }
};
```
##### LeetCode 230. 二叉搜索树中第K小的元素
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int k, ans;

    int kthSmallest(TreeNode* root, int _k) {
        k = _k;
        dfs(root);
        return ans;
    }

    bool dfs(TreeNode* root) {
        if (!root) return false;
        if (dfs(root->left)) return true;
        if ( -- k == 0) {
            ans = root->val;
            return true;
        }
        return dfs(root->right);
    }
};

```
##### LeetCode 231. 2的幂
```cpp
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && (n & -n) == n;
    }
};
```
##### LeetCode 232. 用栈实现队列
```cpp
class MyQueue {
public:
    /** Initialize your data structure here. */
    stack<int> a, b;
    MyQueue() {

    }

    /** Push element x to the back of queue. */
    void push(int x) {
        a.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        while (a.size() > 1) b.push(a.top()), a.pop();
        int t = a.top();
        a.pop();
        while (b.size()) a.push(b.top()), b.pop();
        return t;
    }

    /** Get the front element. */
    int peek() {
        while (a.size() > 1) b.push(a.top()), a.pop();
        int t = a.top();
        while (b.size()) a.push(b.top()), b.pop();
        return t;
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        return a.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */

```
##### LeetCode 233. 数字 1 的个数
```cpp
class Solution {
public:
    int countDigitOne(int n) {
        if (n <= 0) return 0;
        vector<int> nums;
        while (n) nums.push_back(n % 10), n /= 10;
        reverse(nums.begin(), nums.end());
        int res = 0;
        for (int i = 0; i < nums.size(); i ++ ) {
            int d = nums[i];
            int left = 0, right = 0, p = 1;
            for (int j = 0; j < i; j ++ ) left = left * 10 + nums[j];
            for (int j = i + 1; j < nums.size(); j ++ ) {
                right = right * 10 + nums[j];
                p = p * 10;
            }
            if (d == 0) res += left * p;
            else if (d == 1) res += left * p + right + 1;
            else res += (left + 1) * p;
        }
        return res;
    }
};

```
##### LeetCode 234. 回文链表
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;
        if (n <= 1) return true;
        int half = n / 2;
        auto a = head;
        for (int i = 0; i < n - half; i ++ ) a = a->next;
        auto b = a->next;
        for (int i = 0; i < half - 1; i ++ ) {
            auto c = b->next;
            b->next = a;
            a = b, b = c;
        }

        auto p = head, q = a;
        bool success = true;
        for (int i = 0; i < half; i ++ ) {
            if (p->val != q->val) {
                success = false;
                break;
            }
            p = p->next;
            q = q->next;
        }

        auto tail = a;
        b = a->next;
        // 将链表恢复原状
        for (int i = 0; i < half - 1; i ++ ) {
            auto c = b->next;
            b->next = a;
            a = b, b = c;
        }

        tail->next = NULL;
        return success;
    }
};
```



### 究极班- Week 12(第 235 ~ 242、257 ~ 258、260、263 ~ 264、268、273 ~ 275、278 ~ 279、282 题)

 
##### LeetCode 235. 二叉搜索树的最近公共祖先
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (p->val > q->val) swap(p, q);
        if (p->val <= root->val && q->val >= root->val) return root;
        if (q->val < root->val) return lowestCommonAncestor(root->left, p, q);
        return lowestCommonAncestor(root->right, p, q);
    }
};

``` 
##### LeetCode 236. 二叉树的最近公共祖先
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* ans = NULL;

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        dfs(root, p, q);
        return ans;
    }

    int dfs(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) return 0;
        int state = dfs(root->left, p, q);
        if (root == p) state |= 1;
        else if (root == q) state |= 2;
        state |= dfs(root->right, p, q);
        if (state == 3 && !ans) ans = root;
        return state;
    }
};

``` 
##### LeetCode 237. 删除链表中的节点 
```cpp
写法1
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
    }
};
写法2
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        *node = *node->next;
    }
};

``` 
##### LeetCode 238. 除自身以外数组的乘积 
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> p(n, 1);
        for (int i = 1; i < n; i ++ ) p[i] = p[i - 1] * nums[i - 1];
        for (int i = n - 1, s = 1; i >= 0; i -- ) {
            p[i] *= s;
            s *= nums[i];
        }
        return p;
    }
};
``` 
##### LeetCode 239. 滑动窗口最大值 
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> q;
        vector<int> res;
        for (int i = 0; i < nums.size(); i ++ ) {
            if (q.size() && i - k + 1 > q.front()) q.pop_front();
            while (q.size() && nums[i] >= nums[q.back()]) q.pop_back();
            q.push_back(i);
            if (i >= k - 1) res.push_back(nums[q.front()]);
        }
        return res;
    }
};

``` 

##### LeetCode 240. 搜索二维矩阵 II 
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;
        int n = matrix.size(), m = matrix[0].size();
        int i = 0, j = m - 1;
        while (i < n && j >= 0) {
            int t = matrix[i][j];
            if (t == target) return true;
            else if (t > target) j -- ;
            else i ++ ;
        }
        return false;
    }
};
``` 
##### LeetCode 241. 为运算表达式设计优先级 
```cpp
class Solution {
public:
    vector<string> expr;

    vector<int> diffWaysToCompute(string s) {
        for (int i = 0; i < s.size(); i ++ ) {
            if (isdigit(s[i])) {
                int j = i, x = 0;
                while (j < s.size() && isdigit(s[j])) x = x * 10 + (s[j ++ ] - '0');
                i = j - 1;
                expr.push_back(to_string(x));
            } else expr.push_back(s.substr(i, 1));
        }
        return dfs(0, expr.size() - 1);
    }

    vector<int> dfs(int l, int r) {
        if (l == r) return {stoi(expr[l])};
        vector<int> res;
        for (int i = l + 1; i < r; i += 2) {
            auto left = dfs(l, i - 1), right = dfs(i + 1, r);
            for (auto x: left)
                for (auto y: right) {
                    int z;
                    if (expr[i] == "+") z = x + y;
                    else if (expr[i] == "-") z = x - y;
                    else z = x * y;
                    res.push_back(z);
                }
        }
        return res;
    }
};

``` 
##### LeetCode 242. 有效的字母异位词
```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        unordered_map<char, int> a, b;
        for (auto c: s) a[c] ++ ;
        for (auto c: t) b[c] ++ ;
        return a == b;
    }
};
``` 
##### LeetCode 257. 二叉树的所有路径
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<string> ans;
    vector<int> path;

    vector<string> binaryTreePaths(TreeNode* root) {
        if (root) dfs(root);
        return ans;
    }

    void dfs(TreeNode* root) {
        path.push_back(root->val);
        if (!root->left && !root->right) {
            string line = to_string(path[0]);
            for (int i = 1; i < path.size(); i ++ )
                line += "->" + to_string(path[i]);
            ans.push_back(line);
        } else {
            if (root->left) dfs(root->left);
            if (root->right) dfs(root->right);
        }
        path.pop_back();
    }
};

``` 
##### LeetCode 258. 各位相加
```cpp
class Solution {
public:
    int addDigits(int num) {
        if (!num) return 0;
        if (num % 9) return num % 9;
        return 9;
    }
};
``` 
##### LeetCode 260. 只出现一次的数字 III
```cpp
class Solution {
public:
    int get(vector<int>& nums, int k, int t) {
        int res = 0;
        for (auto x: nums)
            if ((x >> k & 1) == t)
                res ^= x;
        return res;
    }

    vector<int> singleNumber(vector<int>& nums) {
        int ab = 0;
        for (auto x: nums) ab ^= x;
        int k = 0;
        while ((ab >> k & 1) == 0) k ++ ;
        return {get(nums, k, 0), get(nums, k, 1)};
    }
};
``` 
##### LeetCode 263. 丑数
```cpp
class Solution {
public:
    bool isUgly(int num) {
        if (num <= 0) return false;
        while (num % 2 == 0) num /= 2;
        while (num % 3 == 0) num /= 3;
        while (num % 5 == 0) num /= 5;
        return num == 1;
    }
};
``` 
##### LeetCode 264. 丑数 II
```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> q(1, 1);
        for (int i = 0, j = 0, k = 0; q.size() < n;) {
            int t = min(q[i] * 2, min(q[j] * 3, q[k] * 5));
            q.push_back(t);
            if (q[i] * 2 == t) i ++ ;
            if (q[j] * 3 == t) j ++ ;
            if (q[k] * 5 == t) k ++ ;
        }
        return q.back();
    }
};
``` 
##### LeetCode 268. 缺失数字
```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        int res = n * (n + 1) / 2;
        for (auto x: nums) res -= x;
        return res;
    }
};
``` 
##### LeetCode 273. 整数转换英文表示
```cpp
class Solution {
public:

    string num0_19[20] = {
        "Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven",
        "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen",
        "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen",
        "Nineteen",
    };
    string num20_90[8] = {
        "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy",
        "Eighty", "Ninety",
    };
    string num1000[4] = {
        "Billion ", "Million ", "Thousand ", "",
    };

    string get(int x) {  // 返回1 ~ 999的英文表示
        string res;
        if (x >= 100) {
            res += num0_19[x / 100] + " Hundred ";
            x %= 100;
        }
        if (x >= 20) {
            res += num20_90[x / 10 - 2] + " ";
            x %= 10;
            if (x) res += num0_19[x] + ' ';
        } else if (x) res += num0_19[x] + ' ';
        return res;
    }

    string numberToWords(int num) {
        if (!num) return "Zero";
        string res;
        for (int i = 1e9, j = 0; i >= 1; i /= 1000, j ++ )
            if (num >= i) {
                res += get(num / i) + num1000[j];
                num %= i;
            }
        res.pop_back();
        return res;
    }
};

``` 
##### LeetCode 274. H 指数
```cpp
class Solution {
public:
    int hIndex(vector<int>& c) {
        sort(c.begin(), c.end(), greater<int>());
        for (int h = c.size(); h; h -- )
            if (c[h - 1] >= h)
                return h;
        return 0;
    }
};
``` 
##### LeetCode 275. H指数 II
```cpp
class Solution {
public:
    int hIndex(vector<int>& c) {
        int n = c.size();
        int l = 0, r = n;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (c[n - mid] >= mid) l = mid;
            else r = mid - 1;
        }
        return r;
    }
};
``` 
##### LeetCode 278. 第一个错误的版本
```cpp
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r) {
            int mid = (long long)l + r >> 1;
            if (isBadVersion(mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};

``` 
##### LeetCode 279. 完全平方数
```cpp
class Solution {
public:
    bool check(int x) {
        int r = sqrt(x);
        return r * r == x;
    }

    int numSquares(int n) {
        if (check(n)) return 1;

        for (int a = 1; a <= n / a; a ++ )
            if (check(n - a * a))
                return 2;

        while (n % 4 == 0) n /= 4;
        if (n % 8 != 7) return 3;
        return 4;
    }
};

``` 
##### LeetCode 282. 给表达式添加运算符
```cpp
typedef long long LL;

class Solution {
public:
    vector<string> ans;
    string path;

    vector<string> addOperators(string num, int target) {
        path.resize(100);
        dfs(num, 0, 0, 0, 1, target);
        return ans;
    }

    void dfs(string& num, int u, int len, LL a, LL b, LL target) {
        if (u == num.size()) {
            if (a == target) ans.push_back(path.substr(0, len - 1));
        } else {
            LL c = 0;
            for (int i = u; i < num.size(); i ++ ) {
                c = c * 10 + num[i] - '0';
                path[len ++ ] = num[i];

                // +
                path[len] = '+';
                dfs(num, i + 1, len + 1, a + b * c, 1, target);

                if (i + 1 < num.size()) {
                    // -
                    path[len] = '-';
                    dfs(num, i + 1, len + 1, a + b * c, -1, target);

                    // *
                    path[len] = '*';
                    dfs(num, i + 1, len + 1, a, b * c, target);
                }
                if (num[u] == '0') break;
            }
        }
    }
};
``` 


### 究极班——Week 13(第 283 ~ 284、287、289 ~ 290、292、295、297、299 ~ 301、303 ~ 304、306 ~ 307、309 ~ 310、312 ~ 313、315 题）


#####  LeetCode 283. 移动零
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int k = 0;
        for (auto x: nums)
            if (x)
                nums[k ++ ] = x;
        while (k < nums.size()) nums[k ++ ] = 0;
    }
};
```
#####  LeetCode 284. 顶端迭代器
```cpp
/*
 * Below is the interface for Iterator, which is already defined for you.
 * **DO NOT** modify the interface for Iterator.
 *
 *  class Iterator {
 *      struct Data;
 *      Data* data;
 *      Iterator(const vector<int>& nums);
 *      Iterator(const Iterator& iter);
 *
 *      // Returns the next element in the iteration.
 *      int next();
 *
 *      // Returns true if the iteration has more elements.
 *      bool hasNext() const;
 *  };
 */

class PeekingIterator : public Iterator {
public:
    int cur;
    bool has_next;

    PeekingIterator(const vector<int>& nums) : Iterator(nums) {
        // Initialize any member here.
        // **DO NOT** save a copy of nums and manipulate it directly.
        // You should only use the Iterator interface methods.
        has_next = Iterator::hasNext();
        if (has_next) cur = Iterator::next();
    }

    // Returns the next element in the iteration without advancing the iterator.
    int peek() {
        return cur;
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    int next() {
        int t = cur;
        has_next = Iterator::hasNext();
        if (has_next) cur = Iterator::next();
        return t;
    }

    bool hasNext() const {
        return has_next;
    }
};
```
#####  LeetCode 287. 寻找重复数
```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int a = 0, b = 0;
        while (true) {
            a = nums[a];
            b = nums[nums[b]];
            if (a == b) {
                a = 0;
                while (a != b) {
                    a = nums[a];
                    b = nums[b];
                }
                return a;
            }
        }

        return -1;
    }
};

```
#####  LeetCode 289. 生命游戏
```cpp
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        if (board.empty() || board[0].empty()) return;
        int n = board.size(), m = board[0].size();
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                int live = 0;
                for (int x = max(0, i - 1); x <= min(n - 1, i + 1); x ++ )
                    for (int y = max(0, j - 1); y <= min(m - 1, j + 1); y ++ )
                        if ((x != i || y != j) && (board[x][y] & 1))
                            live ++ ;
                int cur = board[i][j] & 1, next;
                if (cur) {
                    if (live < 2 || live > 3) next = 0;
                    else next = 1;
                } else {
                    if (live == 3) next = 1;
                    else next = 0;
                }
                board[i][j] |= next << 1;
            }

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                board[i][j] >>= 1;
    }
};

```
#####  LeetCode 290. 单词规律
```cpp
class Solution {
public:
    bool wordPattern(string pattern, string str) {
        vector<string> words;
        stringstream ssin(str);
        string word;
        while (ssin >> word) words.push_back(word);
        if (pattern.size() != words.size()) return false;
        unordered_map<char, string> pw;
        unordered_map<string, char> wp;
        for (int i = 0; i < pattern.size(); i ++ ) {
            auto a = pattern[i];
            auto b = words[i];
            if (pw.count(a) && pw[a] != b) return false;
            pw[a] = b;
            if (wp.count(b) && wp[b] != a) return false;
            wp[b] = a;
        }
        return true;
    }
};
```
#####  LeetCode 292. Nim 游戏
```cpp
class Solution {
public:
    bool canWinNim(int n) {
        return n % 4;
    }
};
```
#####  LeetCode 295. 数据流的中位数
```cpp
class MedianFinder {
public:
    priority_queue<int, vector<int>, greater<int>> up;
    priority_queue<int> down;

    /** initialize your data structure here. */
    MedianFinder() {

    }

    void addNum(int num) {
        if (down.empty() || num <= down.top()) {
            down.push(num);
            if (down.size() > up.size() + 1) {
                up.push(down.top());
                down.pop();
            }
        } else {
            up.push(num);
            if (up.size() > down.size()) {
                down.push(up.top());
                up.pop();
            }
        }
    }

    double findMedian() {
        if ((down.size() + up.size()) % 2) return down.top();
        return (down.top() + up.top()) / 2.0;
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```
#####  LeetCode 297. 二叉树的序列化与反序列化
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:
    string path;
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        dfs_s(root);
        return path;
    }

    void dfs_s(TreeNode* root) {
        if (!root) path += "#,";
        else {
            path += to_string(root->val) + ',';
            dfs_s(root->left);
            dfs_s(root->right);
        }
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        int u = 0;
        return dfs_d(data, u);
    }

    TreeNode* dfs_d(string& data, int& u) {
        if (data[u] == '#') {
            u += 2;
            return NULL;
        } else {
            int k = u;
            while (data[u] != ',') u ++ ;
            auto root = new TreeNode(stoi(data.substr(k, u - k)));
            u ++ ;
            root->left = dfs_d(data, u);
            root->right = dfs_d(data, u);
            return root;
        }
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));

```
#####  LeetCode 299. 猜数字游戏
```cpp
class Solution {
public:
    string getHint(string secret, string guess) {
        unordered_map<char, int> hash;
        for (auto c: secret) hash[c] ++ ;
        int tot = 0;
        for (auto c: guess)
            if (hash[c]) {
                tot ++ ;
                hash[c] -- ;
            }

        int bulls = 0;
        for (int i = 0; i < secret.size(); i ++ )
            if (secret[i] == guess[i])
                bulls ++ ;

        return to_string(bulls) + "A" + to_string(tot - bulls) + "B";
    }
};
```
#####  LeetCode 300. 最长上升子序列
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> q;
        for (auto x: nums) {
            if (q.empty() || x > q.back()) q.push_back(x);
            else {
                if (x <= q[0]) q[0] = x;
                else {
                    int l = 0, r = q.size() - 1;
                    while (l < r) {
                        int mid = l + r + 1 >> 1;
                        if (q[mid] < x) l = mid;
                        else r = mid - 1;
                    }
                    q[r + 1] = x;
                }
            }
        }
        return q.size();
    }
};
```
 
#####  LeetCode  301. 删除无效的括号
```C++
class Solution {
public:
    vector<string> ans;

    vector<string> removeInvalidParentheses(string s) {
        int l = 0, r = 0;
        for (auto x: s)
            if (x == '(') l ++ ;
            else if (x == ')') {
                if (l == 0) r ++ ;
                else l -- ;
            }

        dfs(s, 0, "", 0, l, r);
        return ans;
    }

    void dfs(string& s, int u, string path, int cnt, int l, int r) {
        if (u == s.size()) {
            if (!cnt) ans.push_back(path);
            return;
        }

        if (s[u] != '(' && s[u] != ')') dfs(s, u + 1, path + s[u], cnt, l, r);
        else if (s[u] == '(') {
            int k = u;
            while (k < s.size() && s[k] == '(') k ++ ;
            l -= k - u;
            for (int i = k - u; i >= 0; i -- ) {
                if (l >= 0) dfs(s, k, path, cnt, l, r);
                path += '(';
                cnt ++, l ++ ;
            }
        } else if (s[u] == ')') {
            int k = u;
            while (k < s.size() && s[k] == ')') k ++ ;
            r -= k - u;
            for (int i = k - u; i >= 0; i -- ) {
                if (cnt >= 0 && r >= 0) dfs(s, k, path, cnt, l, r);
                path += ')';
                cnt --, r ++ ;
            }
        }
    }
};

```
#####  LeetCode  303. 区域和检索 - 数组不可变
```C++
class NumArray {
public:
    vector<int> s;

    NumArray(vector<int>& nums) {
        s.resize(nums.size() + 1);
        for (int i = 1; i <= nums.size(); i ++ ) s[i] = s[i - 1] + nums[i - 1];
    }

    int sumRange(int i, int j) {
        ++i, ++j;
        return s[j] - s[i - 1];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */

```
#####  LeetCode  304. 二维区域和检索 - 矩阵不可变
```C++
class NumMatrix {
public:
    vector<vector<int>> s;

    NumMatrix(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return;
        s = vector<vector<int>>(matrix.size() + 1, vector<int>(matrix[0].size() + 1));
        for (int i = 1; i <= matrix.size(); i ++ )
            for (int j = 1; j <= matrix[0].size(); j ++ )
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + matrix[i - 1][j - 1];
    }

    int sumRegion(int x1, int y1, int x2, int y2) {
        ++x1, ++y1, ++x2, ++y2;
        return s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1];
    }
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * int param_1 = obj->sumRegion(row1,col1,row2,col2);
 */
```
#####  LeetCode  306. 累加数
```C++
class Solution {
public:

    string add(string x, string y) {
        vector<int> A, B, C;
        for (int i = x.size() - 1; i >= 0; i -- ) A.push_back(x[i] - '0');
        for (int i = y.size() - 1; i >= 0; i -- ) B.push_back(y[i] - '0');
        for (int i = 0, t = 0; i < A.size() || i < B.size() || t; i ++ ) {
            if (i < A.size()) t += A[i];
            if (i < B.size()) t += B[i];
            C.push_back(t % 10);
            t /= 10;
        }
        string z;
        for (int i = C.size() - 1; i >= 0; i -- ) z += to_string(C[i]);
        return z;
    }

    bool isAdditiveNumber(string num) {
        for (int i = 0; i < num.size(); i ++ )
            for (int j = i + 1; j + 1 < num.size(); j ++ ) {
                int a = -1, b = i, c = j;
                while (true) {
                    if (b - a > 1 && num[a + 1] == '0' || c - b > 1 && num[b + 1] == '0') break;  // 有前导0
                    auto x = num.substr(a + 1, b - a), y = num.substr(b + 1, c - b);
                    auto z = add(x, y);
                    if (num.substr(c + 1, z.size()) != z) break;  // 下一个数不匹配
                    a = b, b = c, c += z.size();
                    if (c + 1 == num.size()) return true;
                }
            }

        return false;
    }
};
```
#####  LeetCode  307. 区域和检索 - 数组可修改
```C++
class NumArray {
public:
    int n;
    vector<int> tr, nums;

    int lowbit(int x) {
        return x & -x;
    }

    int query(int x) {
        int res = 0;
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }

    void add(int x, int v) {
        for (int i = x; i <= n; i += lowbit(i)) tr[i] += v;
    }

    NumArray(vector<int>& _nums) {
        nums = _nums;
        n = nums.size();
        tr.resize(n + 1);

        for (int i = 1; i <= n; i ++ ) {
            tr[i] = nums[i - 1];
            for (int j = i - 1; j > i - lowbit(i); j -= lowbit(j))
                tr[i] += tr[j];
        }
    }

    void update(int i, int val) {
        add(i + 1, val - nums[i]);
        nums[i] = val;
    }

    int sumRange(int i, int j) {
        return query(j + 1) - query(i);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * obj->update(i,val);
 * int param_2 = obj->sumRange(i,j);
 */
 
```
#####  LeetCode  309. 最佳买卖股票时机含冷冻期
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty()) return 0;
        int n = prices.size(), INF = 1e8;
        vector<vector<int>> f(n, vector<int>(3, -INF));
        f[0][1] = -prices[0], f[0][0] = 0;
        for (int i = 1; i < n; i ++ ) {
            f[i][0] = max(f[i - 1][0], f[i - 1][2]);
            f[i][1] = max(f[i - 1][1], f[i - 1][0] - prices[i]);
            f[i][2] = f[i - 1][1] + prices[i];
        }
        return max(f[n - 1][0], max(f[n - 1][1], f[n - 1][2]));
    }
};

```
#####  LeetCode  310. 最小高度树
```C++
class Solution {
public:
    vector<vector<int>> g;
    vector<int> d1, d2, p1, up;

    void dfs1(int u, int father) {
        for (int x: g[u]) {
            if (x == father) continue;
            dfs1(x, u);
            int d = d1[x] + 1;
            if (d >= d1[u]) {
                d2[u] = d1[u], d1[u] = d;
                p1[u] = x;
            } else if (d > d2[u]) {
                d2[u] = d;
            }
        }
    }

    void dfs2(int u, int father) {
        for (int x: g[u]) {
            if (x == father) continue;
            if (p1[u] == x) up[x] = max(up[u], d2[u]) + 1;
            else up[x] = max(up[u], d1[u]) + 1;
            dfs2(x, u);
        }
    }

    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        g.resize(n);
        d1 = d2 = p1 = up = vector<int>(n);
        for (auto& e: edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b), g[b].push_back(a);
        }
        dfs1(0, -1);
        dfs2(0, -1);

        int mind = n + 1;
        for (int i = 0; i < n; i ++ ) mind = min(mind, max(up[i], d1[i]));
        vector<int> res;
        for (int i = 0; i < n; i ++ )
            if (max(up[i], d1[i]) == mind)
                res.push_back(i);
        return res;
    }
};
```
#####  LeetCode  312. 戳气球
```C++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        vector<int> a(n + 2, 1);
        for (int i = 1; i <= n; i ++ ) a[i] = nums[i - 1];
        vector<vector<int>> f(n + 2, vector<int>(n + 2));
        for (int len = 3; len <= n + 2; len ++ )
            for (int i = 0; i + len - 1 <= n + 1; i ++ ) {
                int j = i + len - 1;
                for (int k = i + 1; k < j; k ++ )
                    f[i][j] = max(f[i][j], f[i][k] + f[k][j] + a[i] * a[k] * a[j]);
            }

        return f[0][n + 1];
    }
};

```
#####  LeetCode  313. 超级丑数
```C++
class Solution {
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        typedef pair<int, int> PII;
        priority_queue<PII, vector<PII>, greater<PII>> heap;
        for (int x: primes) heap.push({x, 0});
        vector<int> q(n);
        q[0] = 1;
        for (int i = 1; i < n;) {
            auto t = heap.top(); heap.pop();
            if (t.first != q[i - 1]) q[i ++ ] = t.first;
            int idx = t.second, p = t.first / q[idx];
            heap.push({(long long)p * q[idx + 1], idx + 1});
        }
        return q[n - 1];
    }
};
```
#####  LeetCode  315. 计算右侧小于当前元素的个数
```C++
class Solution {
public:
    int n = 20001;
    vector<int> tr;

    int lowbit(int x) {
        return x & -x;
    }

    int query(int x) {
        int res = 0;
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }

    void add(int x, int v) {
        for (int i = x; i <= n; i += lowbit(i)) tr[i] += v;
    }

    vector<int> countSmaller(vector<int>& nums) {
        tr.resize(n + 1);
        vector<int> res(nums.size());
        for (int i = nums.size() - 1; i >= 0; i -- ) {
            int x = nums[i] + 10001;
            res[i] = query(x - 1);
            add(x, 1);
        }
        return res;
    }
};

```



### 究极班 - Week 14 (第 316、318 ~ 319、321 ~ 322、324、326 ~ 332、334 ~ 338、341 ~ 342 题)

##### LeetCode 316. 去除重复字母
```cpp
class Solution {
public:
    string removeDuplicateLetters(string s) {
        string stk;
        unordered_map<char, bool> ins;
        unordered_map<char, int> last;
        for (int i = 0; i < s.size(); i ++ ) last[s[i]] = i;

        for (int i = 0; i < s.size(); i ++ ) {
            if (ins[s[i]]) continue;
            while (stk.size() && stk.back() > s[i] && last[stk.back()] > i) {
                ins[stk.back()] = false;
                stk.pop_back();
            }
            stk += s[i];
            ins[s[i]] = true;
        }

        return stk;
    }
};
```
##### LeetCode 318. 最大单词长度乘积
```cpp
class Solution {
public:
    int maxProduct(vector<string>& words) {
        vector<int> state;
        for (auto word: words) {
            int s = 0;
            for (auto c: word)
                s |= 1 << (c - 'a');
            state.push_back(s);
        }

        int res = 0;
        for (int i = 0; i < words.size(); i ++ )
            for (int j = i + 1; j < words.size(); j ++ )
                if ((state[i] & state[j]) == 0)
                    res = max(res, (int)(words[i].size() * words[j].size()));
        return res;
    }
};

```
##### LeetCode 319. 灯泡开关
```cpp
class Solution {
public:
    int bulbSwitch(int n) {
        return sqrt(n);
    }
};
```
##### LeetCode 321. 拼接最大数
```cpp
class Solution {
public:
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
        int n = nums1.size(), m = nums2.size();
        vector<int> res(k, INT_MIN);
        for (int i = max(0, k - m); i <= min(k, n); i ++ ) {
            auto a = maxArray(nums1, i);
            auto b = maxArray(nums2, k - i);
            auto c = merge(a, b);
            res = max(res, c);
        }
        return res;
    }

    vector<int> merge(vector<int>& a, vector<int>& b) {
        vector<int> c;
        while (a.size() && b.size())
            if (a > b) c.push_back(a[0]), a.erase(a.begin());
            else c.push_back(b[0]), b.erase(b.begin());
        while (a.size()) c.push_back(a[0]), a.erase(a.begin());
        while (b.size()) c.push_back(b[0]), b.erase(b.begin());
        return c;
    }

    vector<int> maxArray(vector<int>& nums, int k) {
        vector<int> res(k);
        int n = nums.size();
        for (int i = 0, j = 0; i < nums.size(); i ++ ) {
            int x = nums[i];
            while (j && res[j - 1] < x && j + n - i > k) j -- ;
            if (j < k) res[j ++ ] = x;
        }
        return res;
    }
};
```
##### LeetCode 322. 零钱兑换
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int m) {
        vector<int> f(m + 1, 1e8);
        f[0] = 0;
        for (auto v: coins)
            for (int j = v; j <= m; j ++ )
                f[j] = min(f[j], f[j - v] + 1);
        if (f[m] == 1e8) return -1;
        return f[m];
    }
};

```

##### LeetCode 324. 摆动排序 II
```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        int n = nums.size();
        auto midptr = nums.begin() + n / 2;
        nth_element(nums.begin(), midptr, nums.end());
        int mid = *midptr;

        #define A(i) nums[(i * 2 + 1) % (n | 1)]

        for (int i = 0, j = 0, k = n - 1; i <= k;) {
            if (A(i) > mid) swap(A(i ++ ), A(j ++ ));
            else if (A(i) < mid) swap(A(i), A(k -- ));
            else i ++ ;
        }
    }
};
```
##### LeetCode 326. 3的幂
```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
};
```
##### LeetCode 327. 区间和的个数
```cpp
typedef long long LL;

class Solution {
public:
    int m;
    vector<int> tr;
    vector<LL> numbers;

    int get(LL x) {
        return lower_bound(numbers.begin(), numbers.end(), x) - numbers.begin() + 1;
    }

    int lowbit(int x) {
        return x & -x;
    }

    void add(int x, int v) {
        for (int i = x; i <= m; i += lowbit(i)) tr[i] += v;
    }

    int query(int x) {
        int res = 0;
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }

    int countRangeSum(vector<int>& nums, int lower, int upper) {
        int n = nums.size();
        vector<LL> s(n + 1);
        numbers.push_back(0);
        for (int i = 1; i <= n; i ++ ) {
            s[i] = s[i - 1] + nums[i - 1];
            numbers.push_back(s[i]);
            numbers.push_back(s[i] - lower);
            numbers.push_back(s[i] - upper - 1);
        }
        sort(numbers.begin(), numbers.end());
        numbers.erase(unique(numbers.begin(), numbers.end()), numbers.end());
        m = numbers.size();
        tr.resize(m + 1);

        int res = 0;
        add(get(0), 1);
        for (int i = 1; i <= n; i ++ ) {
            res += query(get(s[i] - lower)) - query(get(s[i] - upper - 1));
            add(get(s[i]), 1);
        }

        return res;
    }
};

```
##### LeetCode 328. 奇偶链表
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if (!head || !head->next) return head;
        auto oh = head, ot = oh;
        auto eh = head->next, et = eh;
        for (auto p = head->next->next; p;) {
            ot = ot->next = p;
            p = p->next;
            if (p) {
                et = et->next = p;
                p = p->next;
            }
        }
        ot->next = eh;
        et->next = NULL;
        return oh;
    }
};

```
##### LeetCode 329. 矩阵中的最长递增路径
```cpp
lass Solution {
public:
    int n, m;
    vector<vector<int>> f, w;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int dp(int x, int y) {
        auto& v = f[x][y];
        if (v != -1) return v;
        v = 1;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && w[x][y] < w[a][b])
                v = max(v, dp(a, b) + 1);
        }
        return v;
    }

    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return 0;
        w = matrix;
        n = w.size(), m = w[0].size();
        f = vector<vector<int>>(n, vector<int>(m, -1));

        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                res = max(res, dp(i, j));
        return res;
    }
};

```
##### LeetCode 330. 按要求补齐数组
```cpp
class Solution {
public:
    int minPatches(vector<int>& nums, int n) {
        long long x = 1;
        int i = 0, res = 0;
        while (x <= n) {
            if (i < nums.size() && nums[i] <= x) x += nums[i ++ ];
            else {
                x += x;
                res ++ ;
            }
        }
        return res;
    }
};
```
##### LeetCode 331. 验证二叉树的前序序列化
```cpp
class Solution {
public:
    int k;
    string s;

    bool isValidSerialization(string _s) {
        k = 0;
        s = _s + ',';
        if (!dfs()) return false;
        return k == s.size();
    }

    bool dfs() {
        if (k == s.size()) return false;
        if (s[k] == '#') return k += 2, true;
        while (s[k] != ',') k ++ ;
        k ++ ;
        return dfs() && dfs();
    }
};
```
##### LeetCode 332. 重新安排行程
```cpp
class Solution {
public:
    unordered_map<string, multiset<string>> g;
    vector<string> ans;

    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for (auto& e: tickets) g[e[0]].insert(e[1]);
        dfs("JFK");
        reverse(ans.begin(), ans.end());
        return ans;
    }

    void dfs(string u) {
        while (g[u].size()) {
            auto ver = *g[u].begin();
            g[u].erase(g[u].begin());
            dfs(ver);
        }
        ans.push_back(u);
    }
};
```
##### LeetCode 334. 递增的三元子序列
```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        vector<int> q(2, INT_MAX);
        for (auto a: nums) {
            int k = 2;
            while (k > 0 && q[k - 1] >= a) k -- ;
            if (k == 2) return true;
            q[k] = a;
        }
        return false;
    }
};
```
##### LeetCode 335. 路径交叉
```cpp
class Solution {
public:
    bool isSelfCrossing(vector<int>& x) {
        int n = x.size();
        if (n <= 3) return false;
        for (int i = 3; i < n; i ++ ) {
            if (x[i - 1] <= x[i - 3] && x[i] >= x[i - 2]) return true;
            if (i >= 4 && x[i - 3] == x[i - 1] && x[i] + x[i - 4] >= x[i - 2]) return true;
            if (i >= 5 && x[i - 3] >= x[i - 1] && x[i - 1] + x[i - 5] >= x[i - 3] && x[i - 2] >= x[i - 4] && x[i - 4] + x[i] >= x[i - 2])
                return true;
        }
        return false;
    }
};
```
##### LeetCode 336. 回文对
```cpp
typedef unsigned long long ULL;

const int N = 310, P = 131;

ULL p[N];

class Solution {
public:
    vector<vector<ULL>> h1, h2;

    ULL get(vector<ULL>& h, int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }

    vector<vector<int>> palindromePairs(vector<string>& words) {
        p[0] = 1;
        for (int i = 1; i < N; i ++ ) p[i] = p[i - 1] * P;

        for (int i = 0; i < words.size(); i ++ ) {
            int n = words[i].size();
            vector<ULL> a(1), b(1);
            for (int j = 1; j <= n; j ++ ) a.push_back(a.back() * P + words[i][j - 1]);
            for (int j = n; j; j -- ) b.push_back(b.back() * P + words[i][j - 1]);
            h1.push_back(a);
            h2.push_back(b);
        }

        unordered_map<ULL, int> hash;
        for (int i = 0; i < words.size(); i ++ )
            hash[get(h2[i], 1, words[i].size())] = i;

        vector<vector<int>> res;
        for (int i = 0; i < words.size(); i ++ ) {
            int n = words[i].size();
            for (int j = 0; j <= n; j ++ ) {
                auto left = get(h1[i], 1, j), right = get(h1[i], j + 1, n);
                if (right == get(h2[i], 1, n - j) && hash.count(left) && hash[left] != i) res.push_back({i, hash[left]});
                if (left == get(h2[i], n - j + 1, n) && hash.count(right) && hash[right] != i && words[i].size() != words[hash[right]].size())
                    res.push_back({hash[right], i});
            }
        }
        return res;
    }
};

```
##### LeetCode 337. 打家劫舍 III
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        auto f = dfs(root);
        return max(f[0], f[1]);
    }

    vector<int> dfs(TreeNode* u) {
        if (!u) return {0, 0};
        auto x = dfs(u->left), y = dfs(u->right);
        return {max(x[0], x[1]) + max(y[0], y[1]), x[0] + y[0] + u->val};
    }
};

```
##### LeetCode 338. 比特位计数
```cpp
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> f(num + 1);
        for (int i = 1; i <= num; i ++ )
            f[i] = f[i >> 1] + (i & 1);
        return f;
    }
};
```
##### LeetCode 341. 扁平化嵌套列表迭代器
```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
public:
    vector<int> q;
    int k;

    NestedIterator(vector<NestedInteger> &nestedList) {
        k = 0;
        for (auto& l: nestedList) dfs(l);
    }

    void dfs(NestedInteger& l) {
        if (l.isInteger()) q.push_back(l.getInteger());
        else {
            for (auto& v: l.getList()) dfs(v);
        }
    }

    int next() {
        return q[k ++ ];
    }

    bool hasNext() {
        return k < q.size();
    }
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */

```
##### LeetCode 342. 4的幂
```cpp
class Solution {
public:
    bool isPowerOfFour(int num) {
        if (num <= 0) return false;
        int r = sqrt(num);
        if (r * r != num) return false;
        return (1 << 30) % num == 0;
    }
};
```



### 究极班- Week 15（第 343 ~ 345、347、349 ~ 350、352、354 ~ 355、357、363、365、367 ~ 368、371 ~ 376 题） 


##### LeetCode 343. 整数拆分
```cpp
class Solution {
public:
    int integerBreak(int n) {
        if (n <= 3) return 1 * (n - 1);
        int p = 1;
        while (n >= 5) n -= 3, p *= 3;
        return p * n;
    }
};
```
##### LeetCode 344. 反转字符串
```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1; i < j; i ++, j -- )
            swap(s[i], s[j]);
    }
};
```
##### LeetCode 345. 反转字符串中的元音字母
```cpp
class Solution {
public:
    string s = "aeiou";

    bool check(char c) {
        return s.find(tolower(c)) != -1;
    }

    string reverseVowels(string s) {
        for (int i = 0, j = s.size() - 1; i < j; i ++ , j -- ) {
            while (i < j && !check(s[i])) i ++ ;
            while (i < j && !check(s[j])) j -- ;
            swap(s[i], s[j]);
        }
        return s;
    }
};

```
##### LeetCode 347. 前 K 个高频元素
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        for (auto x: nums) cnt[x] ++ ;
        int n = nums.size();
        vector<int> s(n + 1);
        for (auto [x, c]: cnt) s[c] ++ ;
        int i = n, t = 0;
        while (t < k) t += s[i -- ];
        vector<int> res;
        for (auto [x, c]: cnt)
            if (c > i)
                res.push_back(x);
        return res;
    }
};

```
##### LeetCode 349. 两个数组的交集
```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> S;
        for (auto x: nums1) S.insert(x);
        vector<int> res;
        for (auto x: nums2)
            if (S.count(x)) {
                res.push_back(x);
                S.erase(x);
            }
        return res;
    }
};
```
##### LeetCode 350. 两个数组的交集 II
```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        unordered_multiset<int> S;
        for (auto x: nums1) S.insert(x);
        vector<int> res;
        for (auto x: nums2)
            if (S.count(x)) {
                res.push_back(x);
                S.erase(S.find(x));
            }
        return res;
    }
};
```
##### LeetCode 352. 将数据流变为多个不相交区间
```cpp
typedef long long LL;
const LL INF = 1e18;
typedef pair<LL, LL> PLL;

#define x first
#define y second

class SummaryRanges {
public:
    /** Initialize your data structure here. */
    set<PLL> S;

    SummaryRanges() {
        S.insert({-INF, -INF}), S.insert({INF, INF});
    }

    void addNum(int x) {
        auto r = S.upper_bound({x, INT_MAX});
        auto l = r;
        -- l;
        if (l->y >= x) return;
        if (l->y == x - 1 && r->x == x + 1) {
            S.insert({l->x, r->y});
            S.erase(l), S.erase(r);
        } else if (l->y == x - 1) {
            S.insert({l->x, x});
            S.erase(l);
        } else if (r->x == x + 1) {
            S.insert({x, r->y});
            S.erase(r);
        } else {
            S.insert({x, x});
        }
    }

    vector<vector<int>> getIntervals() {
        vector<vector<int>> res;
        for (auto& p: S)
            if (p.x != -INF && p.x != INF)
                res.push_back({(int)p.x, (int)p.y});
        return res;
    }
};

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges* obj = new SummaryRanges();
 * obj->addNum(val);
 * vector<vector<int>> param_2 = obj->getIntervals();
 */

```
##### LeetCode 354. 俄罗斯套娃信封问题
```cpp
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& w) {
        int n = w.size();
        sort(w.begin(), w.end());
        vector<int> f(n);

        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            f[i] = 1;
            for (int j = 0; j < i; j ++ )
                if (w[j][0] < w[i][0] && w[j][1] < w[i][1])
                    f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }

        return res;
    }
};

```
##### LeetCode 355. 设计推特
```cpp
typedef pair<int, int> PII;

#define x first
#define y second

class Twitter {
public:
    /** Initialize your data structure here. */
    unordered_map<int, vector<PII>> tweets;
    unordered_map<int, unordered_set<int>> follows;
    int ts;

    Twitter() {
        ts = 0;
    }

    /** Compose a new tweet. */
    void postTweet(int userId, int tweetId) {
        tweets[userId].push_back({ts, tweetId});
        ts ++ ;
    }

    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    vector<int> getNewsFeed(int u) {
        priority_queue<vector<int>> heap;
        follows[u].insert(u);
        for (auto user: follows[u]) {
            auto &ts = tweets[user];
            if (ts.size()) {
                int i = ts.size() - 1;
                heap.push({ts[i].x, ts[i].y, i, user});
            }
        }

        vector<int> res;
        for (int i = 0; i < 10 && heap.size(); i ++ ) {
            auto t = heap.top();
            heap.pop();
            res.push_back(t[1]);
            int j = t[2];
            if (j) {
                j -- ;
                int user = t[3];
                auto& ts = tweets[user];
                heap.push({ts[j].x, ts[j].y, j, user});
            }
        }

        return res;
    }

    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    void follow(int x, int y) {
        follows[x].insert(y);
    }

    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    void unfollow(int x, int y) {
        follows[x].erase(y);
    }
};

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter* obj = new Twitter();
 * obj->postTweet(userId,tweetId);
 * vector<int> param_2 = obj->getNewsFeed(userId);
 * obj->follow(followerId,followeeId);
 * obj->unfollow(followerId,followeeId);
 */

```
##### LeetCode 357. 计算各个位数不同的数字个数
```cpp
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        n = min(n, 10);
        if (!n) return 1;
        vector<int> f(n + 1);
        f[1] = 9;
        for (int i = 2; i <= n; i ++ )
            f[i] = f[i - 1] * (11 - i);

        int res = 1;
        for (int i = 1; i <= n; i ++ ) res += f[i];
        return res;
    }
};

```
##### LeetCode 363. 矩形区域不超过 K 的最大数值和
```cpp
class Solution {
public:
    vector<vector<int>> s;

    int get(int x1, int y1, int x2, int y2) {
        return s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1];
    }

    int maxSumSubmatrix(vector<vector<int>>& w, int K) {
        int n = w.size(), m = w[0].size();
        s = vector<vector<int>>(n + 1, vector<int>(m + 1));
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + w[i - 1][j - 1];

        int res = INT_MIN;
        for (int i = 1; i <= m; i ++ )
            for (int j = i; j <= m; j ++ ) {
                set<int> S;
                S.insert(0);
                for (int k = 1; k <= n; k ++ ) {
                    int si = get(1, i, k, j);
                    auto it = S.lower_bound(si - K);
                    if (it != S.end()) res = max(res, si - *it);
                    S.insert(si);
                }
            }

        return res;
    }
};
```
##### LeetCode 365. 水壶问题
```cpp
class Solution {
public:
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }

    bool canMeasureWater(int a, int b, int c) {
        if (c > a + b) return false;
        return !c || c % gcd(a, b) == 0;
    }
};
```
##### LeetCode 367. 有效的完全平方数
```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {
        int l = 1, r = num;
        while (l < r) {
            int mid = l + 1ll + r >> 1;
            if (mid <= num / mid) l = mid;
            else r = mid - 1;
        }
        return r * r == num;
    }
};
```
##### LeetCode 368. 最大整除子集
```cpp
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& w) {
        if (w.empty()) return {};
        sort(w.begin(), w.end());
        int n = w.size();
        vector<int> f(n);

        int k = 0;
        for (int i = 0; i < n; i ++ ) {
            f[i] = 1;
            for (int j = 0; j < i; j ++ )
                if (w[i] % w[j] == 0)
                    f[i] = max(f[i], f[j] + 1);
            if (f[k] < f[i]) k = i;
        }

        vector<int> res(1, w[k]);
        while (f[k] > 1) {
            for (int i = 0; i < k; i ++ )
                if (w[k] % w[i] == 0 && f[k] == f[i] + 1) {
                    res.push_back(w[i]);
                    k = i;
                    break;
                }
        }
        return res;
    }
};

```
##### LeetCode 371. 两整数之和
```cpp
class Solution {
public:
    int getSum(int a, int b) {
        if (!a) return b;
        int sum = a ^ b, carry = (unsigned)(a & b) << 1;
        return getSum(carry, sum);
    }
};
```
##### LeetCode 372. 超级次方
```cpp
class Solution {
public:
    const int p = 1337;

    int qmi(int a, int b) {
        a %= p;
        int res = 1;
        while (b) {
            if (b & 1) res = res * a % p;
            a = a * a % p;
            b >>= 1;
        }
        return res;
    }

    int superPow(int a, vector<int>& b) {
        if (b.empty()) return 1;
        int k = b.back();
        b.pop_back();
        return qmi(superPow(a, b), 10) * qmi(a, k) % p;
    }
};

```
##### LeetCode 373. 查找和最小的K对数字
```cpp
typedef vector<int> VI;

class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& a, vector<int>& b, int k) {
        if (a.empty() || b.empty()) return {};
        int n = a.size(), m = b.size();
        priority_queue<VI, vector<VI>, greater<VI>> heap;
        for (int i = 0; i < m; i ++ ) heap.push({b[i] + a[0], 0, i});
        vector<VI> res;
        while (k -- && heap.size()) {
            auto t = heap.top();
            heap.pop();
            res.push_back({a[t[1]], b[t[2]]});
            if (t[1] + 1 < n)
                heap.push({a[t[1] + 1] + b[t[2]], t[1] + 1, t[2]});
        }
        return res;
    }
};

```
##### LeetCode 374. 猜数字大小
```cpp
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return       -1 if num is lower than the guess number
 *                1 if num is higher than the guess number
 *               otherwise return 0
 * int guess(int num);
 */

class Solution {
public:
    int guessNumber(int n) {
        int l = 1, r = n;
        while (l < r) {
            int mid = (long long)l + r >> 1;
            if (guess(mid) <= 0) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};

```
##### LeetCode 375. 猜数字大小 II
```cpp
class Solution {
public:
    int getMoneyAmount(int n) {
        vector<vector<int>> f(n + 2, vector<int>(n + 2));
        for (int len = 2; len <= n; len ++ )
            for (int i = 1; i + len - 1 <= n; i ++ ) {
                int j = i + len - 1;
                f[i][j] = INT_MAX;
                for (int k = i; k <= j; k ++ )
                    f[i][j] = min(f[i][j], max(f[i][k - 1], f[k + 1][j]) + k);
            }
        return f[1][n];
    }
};

```
##### LeetCode 376. 摆动序列
```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        nums.erase(unique(nums.begin(), nums.end()), nums.end());
        if (nums.size() <= 2) return nums.size();
        int res = 2;
        for (int i = 1; i + 1 < nums.size(); i ++ ) {
            int a = nums[i - 1], b = nums[i], c = nums[i + 1];
            if (b > a && b > c || b < a && b < c) res ++ ;
        }
        return res;
    }
};
```




### 究极班- Week 16(第 377 ~ 378、380 ~ 397 题)

##### LeetCode 377. 组合总和 Ⅳ
```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int m) {
        int n = nums.size();
        vector<long long> f(m + 1);
        f[0] = 1;
        for (int j = 0; j <= m; j ++ )
            for (auto v: nums)
                if (j >= v)
                    f[j] = (f[j] + f[j - v]) % INT_MAX;
        return f[m];
    }
};
```
##### LeetCode 378. 有序矩阵中第K小的元素
```cpp
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int l = INT_MIN, r = INT_MAX;
        while (l < r) {
            int mid = (long long)l + r >> 1;
            int i = matrix[0].size() - 1, cnt = 0;
            for (int j = 0; j < matrix.size(); j ++ ) {
                while (i >= 0 && matrix[j][i] > mid) i -- ;
                cnt += i + 1;
            }
            if (cnt >= k) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};

```



##### LeetCode 380. 常数时间插入、删除和获取随机元素
```cpp
class RandomizedSet {
public:
    /** Initialize your data structure here. */
    unordered_map<int, int> hash;
    vector<int> nums;

    RandomizedSet() {

    }

    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int x) {
        if (hash.count(x) == 0) {
            nums.push_back(x);
            hash[x] = nums.size() - 1;
            return true;
        }
        return false;
    }

    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int x) {
        if (hash.count(x)) {
            int y = nums.back();
            int px = hash[x], py = hash[y];
            swap(nums[px], nums[py]);
            swap(hash[x], hash[y]);
            nums.pop_back();
            hash.erase(x);
            return true;
        }
        return false;
    }

    /** Get a random element from the set. */
    int getRandom() {
        return nums[rand() % nums.size()];
    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */

```
##### LeetCode 381. O(1) 时间插入、删除和获取随机元素 - 允许重复
```cpp
class RandomizedCollection {
public:
    unordered_map<int, unordered_set<int>> hash;
    vector<int> nums;

    /** Initialize your data structure here. */
    RandomizedCollection() {

    }

    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    bool insert(int x) {
        bool res = hash[x].empty();
        nums.push_back(x);
        hash[x].insert(nums.size() - 1);
        return res;
    }

    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    bool remove(int x) {
        if (hash[x].size()) {
            int px = *hash[x].begin(), py = nums.size() - 1;
            int y = nums.back();
            swap(nums[px], nums[py]);
            hash[x].erase(px), hash[x].insert(py);
            hash[y].erase(py), hash[y].insert(px);
            nums.pop_back();
            hash[x].erase(py);
            return true;
        }
        return false;
    }

    /** Get a random element from the collection. */
    int getRandom() {
        return nums[rand() % nums.size()];
    }
};

/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * RandomizedCollection* obj = new RandomizedCollection();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */

```

##### LeetCode 382. 链表随机节点
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* h;

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        h = head;
    }

    /** Returns a random node's value. */
    int getRandom() {
        int c = -1, n = 0;
        for (auto p = h; p; p = p->next) {
            n ++ ;
            if (rand() % n == 0) c = p->val;
        }
        return c;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */

```
##### LeetCode 383. 赎金信
```cpp
class Solution {
public:
    bool canConstruct(string a, string b) {
        unordered_map<char, int> hash;
        for (auto c: b) hash[c] ++ ;
        for (auto c: a)
            if (!hash[c]) return false;
            else hash[c] -- ;
        return true;
    }
};
```
##### LeetCode 384. 打乱数组
```cpp
public:
    vector<int> a;

    Solution(vector<int>& nums) {
        a = nums;
    }

    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        return a;
    }

    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
        auto b = a;
        int n = a.size();
        for (int i = 0; i < n; i ++ )
            swap(b[i], b[i + rand() % (n - i)]);
        return b;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(nums);
 * vector<int> param_1 = obj->reset();
 * vector<int> param_2 = obj->shuffle();
 */
```
##### LeetCode 385. 迷你语法分析器
```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    NestedInteger deserialize(string s) {
        int u = 0;
        return dfs(s, u);
    }

    NestedInteger dfs(string& s, int& u) {
        NestedInteger res;
        if (s[u] == '[') {
            u ++ ;  // 跳过左括号
            while (s[u] != ']') res.add(dfs(s, u));
            u ++ ;  // 跳过右括号
            if (u < s.size() && s[u] == ',') u ++ ;  // 跳过逗号
        } else {
            int k = u;
            while (k < s.size() && s[k] != ',' && s[k] != ']') k ++ ;
            res.setInteger(stoi(s.substr(u, k - u)));
            if (k < s.size() && s[k] == ',') k ++ ;  // 跳过逗号
            u = k;
        }
        return res;
    }
};
```
##### LeetCode 386. 字典序排数
```cpp
class Solution {
public:
    vector<int> res;

    vector<int> lexicalOrder(int n) {
        for (int i = 1; i <= 9; i ++ ) dfs(i, n);
        return res;
    }

    void dfs(int cur, int n) {
        if (cur <= n) res.push_back(cur);
        else return;
        for (int i = 0; i <= 9; i ++ ) dfs(cur * 10 + i, n);
    }
};
```
##### LeetCode 387. 字符串中的第一个唯一字符
```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char, int> hash;
        for (auto c: s) hash[c] ++ ;
        for (int i = 0; i < s.size(); i ++ )
            if (hash[s[i]] == 1)
                return i;
        return -1;
    }
};
```
##### LeetCode 388. 文件的最长绝对路径
```cpp
class Solution {
public:
    int lengthLongestPath(string input) {
        stack<int> stk;
        int res = 0;
        for (int i = 0, sum = 0; i < input.size(); i ++ ) {
            int k = 0;
            while (i < input.size() && input[i] == '\t') i ++ , k ++ ;
            while (stk.size() > k) sum -= stk.top(), stk.pop();
            int j = i;
            while (j < input.size() && input[j] != '\n') j ++ ;
            int len = j - i;
            stk.push(len), sum += len;
            if (input.substr(i, len).find('.') != -1)
                res = max(res, sum + (int)stk.size() - 1);
            i = j;
        }
        return res;
    }
};
```
##### LeetCode 389. 找不同
```cpp
class Solution {
public:
    char findTheDifference(string s, string t) {
        int x = 0;
        for (auto c: s) x ^= c;
        for (auto c: t) x ^= c;
        return x;
    }
};
```
##### LeetCode 390. 消除游戏
```cpp
class Solution {
public:
    int lastRemaining(int n) {
        if (n == 1) return 1;
        return 2 * (n / 2 + 1 - lastRemaining(n / 2));
    }
};
```
##### LeetCode 391. 完美矩形
```cpp
class Solution {
public:
    bool isRectangleCover(vector<vector<int>>& r) {
        map<pair<int, int>, int> cnt;
        typedef long long LL;
        LL sum = 0;
        for (auto x: r) {
            LL a = x[0], b = x[1], c = x[2], d = x[3];
            ++ cnt[{a, b}], ++ cnt[{a, d}];
            ++ cnt[{c, b}], ++ cnt[{c, d}];
            sum += (c - a) * (d - b);
        }
        vector<vector<int>> res;
        for (auto& [x, y]: cnt)
            if (y == 1) res.push_back({x.first, x.second});
            else if (y == 3) return false;
            else if (y > 4) return false;
        if (res.size() != 4) return false;
        sort(res.begin(), res.end());
        return sum == (LL)(res[3][0] - res[0][0]) * (res[3][1] - res[0][1]);
    }
};
```
##### LeetCode 392. 判断子序列
```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int k = 0;
        for (auto c: t)
            if (k < s.size() && c == s[k])
                k ++ ;
        return k == s.size();
    }
};
```
##### LeetCode 393. UTF-8 编码验证
```cpp
class Solution {
public:
    int get(int x, int k) {
        return x >> k & 1;
    }

    bool validUtf8(vector<int>& data) {
        for (int i = 0; i < data.size(); i ++ ) {
            if (!get(data[i], 7)) continue;
            int k = 0;
            while (k <= 4 && get(data[i], 7 - k)) k ++ ;
            if (k == 1 || k > 4) return false;
            for (int j = 0; j < k - 1; j ++ ) {
                int t = i + 1 + j;
                if (t >= data.size()) return false;
                if (!(get(data[t], 7) && !get(data[t], 6))) return false;
            }
            i += k - 1;
        }
        return true;
    }
};
```
##### LeetCode 394. 字符串解码
```cpp
class Solution {
public:
    string decodeString(string s) {
        int u = 0;
        return dfs(s, u);
    }

    string dfs(string& s, int& u) {
        string res;
        while (u < s.size() && s[u] != ']') {
            if (s[u] >= 'a' && s[u] <= 'z' || s[u] >= 'A' && s[u] <= 'Z') res += s[u ++ ];
            else if (s[u] >= '0' && s[u] <= '9') {
                int k = u;
                while (s[k] >= '0' && s[k] <= '9') k ++ ;
                int x = stoi(s.substr(u, k - u));
                u = k + 1;
                string y = dfs(s, u);
                u ++ ; // 过滤掉右括号
                while (x -- ) res += y;
            }
        }
        return res;
    }
};
```
##### LeetCode 395. 至少有K个重复字符的最长子串卡
```cpp
class Solution {
public:
    int K;
    unordered_map<char, int> cnt;

    void add(char c, int& x, int& y) {
        if (!cnt[c]) x ++ ;
        cnt[c] ++ ;
        if (cnt[c] == K) y ++ ;
    }

    void del(char c, int& x, int& y) {
        if (cnt[c] == K) y -- ;
        cnt[c] -- ;
        if (!cnt[c]) x -- ;
    }

    int longestSubstring(string s, int _K) {
        K = _K;
        int res = 0;
        for (int k = 1; k <= 26; k ++ ) {
            cnt.clear();
            for (int i = 0, j = 0, x = 0, y = 0; i < s.size(); i ++ ) {
                add(s[i], x, y);
                while (x > k) del(s[j ++ ], x, y);
                if (x == y) res = max(res, i - j + 1);
            }
        }
        return res;
    }
};

```
##### LeetCode 396. 旋转函数
```cpp
class Solution {
public:
    int maxRotateFunction(vector<int>& A) {
        typedef long long LL;
        LL sum = 0, cur = 0;
        for (auto c: A) sum += c;
        int n = A.size();
        for (int i = 0; i < n; i ++ ) cur += i * A[i];
        LL res = cur;
        for (int i = n - 1; i >= 0; i -- ) {
            cur += sum - (LL)n * A[i];
            res = max(res, cur);
        }
        return res;
    }
};
```
##### LeetCode 397. 整数替换
```cpp
typedef long long LL;

class Solution {
public:
    unordered_map<LL, int> dp;

    int integerReplacement(int n) {
        return f(n);
    }

    int f(LL n) {
        if (dp.count(n)) return dp[n];
        if (n == 1) return 0;
        if (n % 2 == 0) return dp[n] = f(n / 2) + 1;
        return dp[n] = min(f(n + 1), f(n - 1)) + 1;
    }
};
```


### 究极班——Week 17(第 398 ~ 407、409 ~ 410、412 ~ 417、419 ~ 420 题）

#####  LeetCode  398. 随机数索引
```C++
class Solution {
public:
    unordered_map<int, vector<int>> hash;

    Solution(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i ++ )
            hash[nums[i]].push_back(i);
    }

    int pick(int target) {
        return hash[target][rand() % hash[target].size()];
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(nums);
 * int param_1 = obj->pick(target);
 */
```
#####  LeetCode  399. 除法求值
```C++
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_set<string> vers;
        unordered_map<string, unordered_map<string, double>> d;
        for (int i = 0; i < equations.size(); i ++ ) {
            auto a = equations[i][0], b = equations[i][1];
            auto c = values[i];
            d[a][b] = c, d[b][a] = 1 / c;
            vers.insert(a), vers.insert(b);
        }

        for (auto k: vers)
            for (auto i: vers)
                for (auto j: vers)
                    if (d[i][k] && d[j][k])
                        d[i][j] = d[i][k] * d[k][j];

        vector<double> res;
        for (auto q: queries) {
            auto a = q[0], b = q[1];
            if (d[a][b]) res.push_back(d[a][b]);
            else res.push_back(-1);
        }

        return res;
    }
};

```
#####  LeetCode  400. 第N个数字
```C++
class Solution {
public:
    int findNthDigit(int n) {
        long long k = 1, t = 9, s = 1;
        while (n > k * t) {
            n -= k * t;
            k ++, t *= 10, s *= 10;
        }
        s += (n + k - 1) / k - 1;
        n = n % k ? n % k : k;
        return to_string(s)[n - 1] - '0';
    }
};
```
#####  LeetCode  401. 二进制手表
```C++
class Solution {
public:
    vector<string> readBinaryWatch(int num) {
        vector<string> res;
        char str[10];
        for (int i = 0; i < 1 << 10; i ++ ) {
            int s = 0;
            for (int j = 0; j < 10; j ++ )
                if (i >> j & 1)
                    s ++ ;
            if (s == num) {
                int a = i >> 6, b = i & 63;
                if (a < 12 && b < 60) {
                    sprintf(str, "%d:%02d", a, b);
                    res.push_back(str);
                }
            }
        }
        return res;
    }
};
```
#####  LeetCode  402. 移掉K位数字
```C++
class Solution {
public:
    string removeKdigits(string num, int k) {
        k = min(k, (int)num.size());
        string res;
        for (auto c: num) {
            while (k && res.size() && res.back() > c) {
                k -- ;
                res.pop_back();
            }
            res += c;
        }
        while (k -- ) res.pop_back();
        k = 0;
        while (k < res.size() && res[k] == '0') k ++ ;
        if (k == res.size()) res += '0';
        return res.substr(k);
    }
};
```
#####  LeetCode  403. 青蛙过河
```C++
const int N = 2010;

int f[N][N];

class Solution {
public:
    unordered_map<int, int> hash;
    vector<int> stones;

    int dp(int x, int y) {
        if (f[x][y] != -1) return f[x][y];
        f[x][y] = 0;
        for (int k = max(1, y - 1); k <= y + 1; k ++ ) {
            int z = stones[x] - k;
            if (hash.count(z)) {
                int p = hash[z];
                if (dp(p, k)) {
                    f[x][y] = 1;
                    break;
                }
            }
        }
        return f[x][y];
    }

    bool canCross(vector<int>& _stones) {
        stones = _stones;
        int n = stones.size();
        for (int i = 0; i < n; i ++ ) hash[stones[i]] = i;
        memset(f, -1, sizeof f);
        f[0][1] = 1;
        for (int i = 0; i < n; i ++ )
            if (dp(n - 1, i))
                return true;
        return false;
    }
};
```
#####  LeetCode  404. 左叶子之和
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int res = 0;

    int sumOfLeftLeaves(TreeNode* root) {
        dfs(root);
        return res;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        if (root->left) {
            if (!root->left->left && !root->left->right) {
                res += root->left->val;
            }
        }
        dfs(root->left);
        dfs(root->right);
    }
};
```
#####  LeetCode  405. 数字转换为十六进制数
```C++
class Solution {
public:
    string toHex(unsigned int num) {
        if (!num) return "0";
        string res, nums = "0123456789abcdef";
        while (num) {
            res += nums[num & 0xf];
            num >>= 4;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```
#####  LeetCode  406. 根据身高重建队列
```C++
class Solution {
public:
    int n;
    vector<int> tr;

    int lowbit(int x) {
        return x & -x;
    }

    void add(int x, int v) {
        for (int i = x; i <= n; i += lowbit(i)) tr[i] += v;
    }

    int query(int x) {
        int res = 0;
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }

    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        n = people.size();
        tr.resize(n + 1);

        sort(people.begin(), people.end(), [](vector<int>a, vector<int>b) {
            if (a[0] != b[0]) return a[0] < b[0];
            return a[1] > b[1];
        });

        vector<vector<int>> res(n);
        for (auto p: people) {
            int h = p[0], k = p[1];
            int l = 1, r = n;
            while (l < r) {
                int mid = l + r >> 1;
                if (mid - query(mid) >= k + 1) r = mid;
                else l = mid + 1;
            }
            res[r - 1] = p;
            add(r, 1);
        }
        return res;
    }
};
```
#####  LeetCode  407. 接雨水 II
```C++
class Solution {
public:
    struct Cell {
        int h, x, y;
        bool operator< (const Cell& t) const {
            return h > t.h;
        }
    };

    int trapRainWater(vector<vector<int>>& h) { 
        if (h.empty() || h[0].empty()) return 0;
        int n = h.size(), m = h[0].size();
        priority_queue<Cell> heap;
        vector<vector<bool>> st(n, vector<bool>(m));
        for (int i = 0; i < n; i ++ ) {
            st[i][0] = st[i][m - 1] = true;
            heap.push({h[i][0], i, 0});
            heap.push({h[i][m - 1], i, m - 1});
        }
        for (int i = 1; i + 1 < m; i ++ ) {
            st[0][i] = st[n - 1][i] = true;
            heap.push({h[0][i], 0, i});
            heap.push({h[n - 1][i], n - 1, i});
        }
        int res = 0;
        int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
        while (heap.size()) {
            auto t = heap.top();
            heap.pop();
            res += t.h - h[t.x][t.y];

            for (int i = 0; i < 4; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m && !st[x][y]) {
                    heap.push({max(h[x][y], t.h), x, y});
                    st[x][y] = true;
                }
            }
        }

        return res;
    }
};
```

#####  LeetCode 409. 最长回文串
```cpp
class Solution {
public:
    int longestPalindrome(string s) {
        unordered_map<char, int> hash;
        for (auto c: s) hash[c] ++ ;
        int res = 0;
        for (auto [a, k]: hash) res += k / 2 * 2;
        if (res < s.size()) res ++ ;
        return res;
    }
};
```
#####  LeetCode 410. 分割数组的最大值
```cpp
class Solution {
public:
    bool check(vector<int>& nums, int m, int mid) {
        int sum = 0, cnt = 0;
        for (auto x: nums) {
            if (x > mid) return false;
            if (sum + x > mid) {
                cnt ++ ;
                sum = x;
            } else {
                sum += x;
            }
        }
        if (sum) cnt ++ ;
        return cnt <= m;
    }

    int splitArray(vector<int>& nums, int m) {
        int l = 0, r = INT_MAX;
        while (l < r) {
            int mid = (long long)l + r >> 1;
            if (check(nums, m, mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};

```
#####  LeetCode 412. Fizz Buzz
```cpp
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> res;
        for (int i = 1; i <= n; i ++ )
            if (i % 3 == 0 && i % 5 == 0) res.push_back("FizzBuzz");
            else if (i % 3 == 0) res.push_back("Fizz");
            else if (i % 5 == 0) res.push_back("Buzz");
            else res.push_back(to_string(i));
        return res;
    }
};
```
#####  LeetCode 413. 等差数列划分
```cpp
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& A) {
        for (int i = A.size() - 1; i > 0; i -- ) A[i] -= A[i - 1];
        int res = 0;
        for (int i = 1; i < A.size(); i ++ ) {
            int j = i;
            while (j < A.size() && A[j] == A[i]) j ++ ;
            int k = j - i;
            res += k * (k - 1) / 2;
            i = j - 1;
        }
        return res;
    }
};
```
#####  LeetCode 414. 第三大的数
```cpp
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        long long INF = 1e10, a = -INF, b = -INF, c = -INF, s = 0;
        for (auto x: nums) {
            if (x > a) s ++, c = b, b = a, a = x;
            else if (x < a && x > b) s ++, c = b, b = x;
            else if (x < b && x > c) s ++, c = x;
        }
        if (s < 3) return a;
        return c;
    }
};
```
#####  LeetCode 415. 字符串相加
```cpp
class Solution {
public:
    vector<int> add(vector<int>& A, vector<int>& B) {
        vector<int> C;
        for (int i = 0, t = 0; i < A.size() || i < B.size() || t; i ++ ) {
            if (i < A.size()) t += A[i];
            if (i < B.size()) t += B[i];
            C.push_back(t % 10);
            t /= 10;
        }
        return C;
    }

    string addStrings(string a, string b) {
        vector<int> A, B;
        for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
        for (int i = b.size() - 1; i >= 0; i -- ) B.push_back(b[i] - '0');
        auto C = add(A, B);
        string c;
        for (int i = C.size() - 1; i >= 0; i -- ) c += to_string(C[i]);
        return c;
    }
};
```
#####  LeetCode 416. 分割等和子集
```cpp
正常数组写法：
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size(), m = 0;
        for (auto x: nums) m += x;
        if (m % 2) return false;
        m /= 2;
        vector<int> f(m + 1);
        f[0] = 1;
        for (auto x: nums)
            for (int j = m; j >= x; j -- )
                f[j] |= f[j - x];
        return f[m];
    }
};
bitset写法：
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        bitset<10001> f;
        f[0] = 1;
        int sum = 0;
        for (auto x: nums) {
            f |= f << x;
            sum += x;
        }
        if (sum % 2) return false;
        return f[sum / 2];
    }
};
```
#####  LeetCode 417. 太平洋大西洋水流问题
```cpp
class Solution {
public:
    int n, m;
    vector<vector<int>> w;
    vector<vector<int>> st;

    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    void dfs(int x, int y, int t) {
        if (st[x][y] & t) return;
        st[x][y] |= t;
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && w[a][b] >= w[x][y])
                dfs(a, b, t);
        }
    }

    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matrix) {
        w = matrix;
        if (w.empty() || w[0].empty()) return {};
        n = w.size(), m = w[0].size();
        st = vector<vector<int>>(n, vector<int>(m));

        for (int i = 0; i < n; i ++ ) dfs(i, 0, 1);
        for (int i = 0; i < m; i ++ ) dfs(0, i, 1);
        for (int i = 0; i < n; i ++ ) dfs(i, m - 1, 2);
        for (int i = 0; i < m; i ++ ) dfs(n - 1, i, 2);

        vector<vector<int>> res;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (st[i][j] == 3)
                    res.push_back({i, j});
        return res;
    }
};

```
#####  LeetCode 419. 甲板上的战舰
```cpp
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int res = 0;
        for (int i = 0; i < board.size(); i ++ )
            for (int j = 0; j < board[i].size(); j ++ ) {
                if (i > 0 && board[i - 1][j] == 'X') continue;
                if (j > 0 && board[i][j - 1] == 'X') continue;
                if (board[i][j] == 'X') res ++ ;
            }
        return res;
    }
};
```

#####  LeetCode 420. 强密码检验器

```cpp
class Solution {
public:
    int strongPasswordChecker(string s) {
        int a = 0, b = 0, c = 0, n = s.size(), k = 0;
        for (auto x: s) {
            if (x >= '0' && x <= '9') a = 1;
            else if (x >= 'a' && x <= 'z') b = 1;
            else if (x >= 'A' && x <= 'Z') c = 1;
        }
        k = a + b + c;
        if (n < 6) return max(6 - n, 3 - k);
        else {
            int p = 0, del = n - 20, res = del;
            int d[3] = {0};
            for (int i = 0; i < s.size(); i ++ ) {
                int j = i;
                while (j < s.size() && s[j] == s[i]) j ++ ;
                int s = j - i;
                i = j - 1;
                p += s / 3;
                if (s >= 3) d[s % 3] ++ ;
            }
            if (n <= 20) return max(p, 3 - k);
            if (d[0] && del > 0) {
                int t = min(d[0], del);
                del -= t;
                p -= t;
            }
            if (d[1] && del > 0) {
                int t = min(d[1] * 2, del);
                del -= t;
                p -= t / 2;
            }
            if (p && del > 0) {
                int t = min(p * 3, del);
                p -= t / 3;
            }
            return res + max(p, 3 - k);
        }
    }
};
``` 





### 究极班- Week 18（第 421、423 ~ 424、427、429 ~ 430、432 ~ 435 题）

##### LeetCode 421. 数组中两个数的最大异或值
```cpp
class Solution {
public:
    vector<vector<int>> s;

    void insert(int x) {
        int p = 0;
        for (int i = 30; i >= 0; i -- ) {
            int u = x >> i & 1;
            if (!s[p][u]) s[p][u] = s.size(), s.push_back({0, 0});
            p = s[p][u];
        }
    }

    int query(int x) {
        int p = 0, res = 0;
        for (int i = 30; i >= 0; i -- ) {
            int u = x >> i & 1;
            if (s[p][!u]) p = s[p][!u], res = res * 2 + !u;
            else p = s[p][u], res = res * 2 + u;
        }
        return res ^ x;
    }

    int findMaximumXOR(vector<int>& nums) {
        s.push_back({0, 0});
        int res = 0;
        for (auto x: nums) {
            res = max(res, query(x));
            insert(x);
        }
        return res;
    }
};
```
##### LeetCode 423. 从英文中重建数字
```cpp
class Solution {
public:
    string originalDigits(string s) {
        string name[] = {
            "zero", "one", "two", "three", "four", "five",
            "six", "seven", "eight", "nine"
        };
        int ord[] = {0, 8, 3, 2, 6, 4, 5, 1, 7, 9};
        unordered_map<char, int> cnt;
        for (auto c: s) cnt[c] ++ ;
        string res;
        for (int x: ord) {
            while (true) {
                bool flag = true;
                for (auto c: name[x])
                    if (!cnt[c]) {
                        flag = false;
                        break;
                    }
                    if (flag) {
                        res += to_string(x);
                        for (auto c: name[x]) cnt[c] -- ;
                    }
                    else break;
            }
        }
        sort(res.begin(), res.end());
        return res;
    }
};
```
##### LeetCode 424. 替换后的最长重复字符
```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        int res = 0;
        for (char c = 'A'; c <= 'Z'; c ++ ) {
            for (int i = 0, j = 0, cnt = 0; i < s.size(); i ++ ) {
                if (s[i] == c) cnt ++ ;
                while (i - j + 1 - cnt > k) {
                    if (s[j] == c) cnt -- ;
                    j ++ ;
                }
                res = max(res, i - j + 1);
            }
        }
        return res;
    }
};
```
##### LeetCode 427. 建立四叉树
```cpp
/*
// Definition for a QuadTree node.
class Node {
public:
    bool val;
    bool isLeaf;
    Node* topLeft;
    Node* topRight;
    Node* bottomLeft;
    Node* bottomRight;

    Node() {
        val = false;
        isLeaf = false;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }

    Node(bool _val, bool _isLeaf) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }

    Node(bool _val, bool _isLeaf, Node* _topLeft, Node* _topRight, Node* _bottomLeft, Node* _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
};
*/

class Solution {
public:
    vector<vector<int>> s;


    Node* construct(vector<vector<int>>& w) {
        int n = w.size();
        s = vector<vector<int>>(n + 1, vector<int>(n + 1));
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + w[i - 1][j - 1];
        return dfs(1, 1, n, n);
    }

    Node* dfs(int x1, int y1, int x2, int y2) {
        int n = x2 - x1 + 1;
        int sum = s[x2][y2] - s[x2][y1 - 1] - s[x1 - 1][y2] + s[x1 - 1][y1 - 1];
        if (sum == 0 || sum == n * n) return new Node(!!sum, true);
        auto node = new Node(0, false);
        int m = n / 2;
        node->topLeft = dfs(x1, y1, x1 + m - 1, y1 + m - 1);
        node->topRight = dfs(x1, y1 + m, x1 + m - 1, y2);
        node->bottomLeft = dfs(x1 + m, y1, x2, y1 + m - 1);
        node->bottomRight = dfs(x1 + m, y1 + m, x2, y2);
        return node;
    }
};
```
##### LeetCode 429. N叉树的层序遍历
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> res;
        if (!root) return res;
        queue<Node*> q;
        q.push(root);
        while (q.size()) {
            int len = q.size();
            vector<int> line;
            while (len -- ) {
                auto t = q.front();
                q.pop();
                line.push_back(t->val);
                for (auto c: t->children) q.push(c);
            }
            res.push_back(line);
        }
        return res;
    }
};
```
##### LeetCode 430. 扁平化多级双向链表
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* prev;
    Node* next;
    Node* child;
};
*/

class Solution {
public:
    Node* flatten(Node* head) {
        auto res = dfs(head);
        return res[0];
    }

    vector<Node*> dfs(Node* head) {
        if (!head) return {NULL, NULL};
        auto cur = head, tail = head;
        while (cur) {
            tail = cur;
            if (cur->child) {
                auto t = dfs(cur->child);
                cur->child = NULL;
                t[1]->next = cur->next;
                if (cur->next) cur->next->prev = t[1];
                cur->next = t[0];
                t[0]->prev = cur;
                cur = t[1]->next;
                tail = t[1];
            } else {
                cur = cur->next;
            }
        }
        return {head, tail};
    }
};
```
##### LeetCode 432. 全 O(1) 的数据结构
```cpp
class AllOne {
public:
    struct Node {
        Node *left, *right;
        int val;
        unordered_set<string> S;

        Node (int _val) {
            val = _val;
            left = right = NULL;
        }
    }*left, *right;
    unordered_map<string, Node*> hash;

    /** Initialize your data structure here. */
    AllOne() {
        left = new Node(INT_MIN), right = new Node(INT_MAX);
        left->right = right, right->left = left;
    }

    Node* add_to_right(Node* node, string key, int val) {
        if (node->right->val == val) node->right->S.insert(key);
        else {
            auto t = new Node(val);
            t->S.insert(key);
            t->right = node->right, node->right->left = t;
            node->right = t, t->left = node;
        }
        return node->right;
    }

    Node* add_to_left(Node* node, string key, int val) {
        if (node->left->val == val) node->left->S.insert(key);
        else {
            auto t = new Node(val);
            t->S.insert(key);
            t->left = node->left, node->left->right = t;
            node->left = t, t->right = node;
        }
        return node->left;
    }

    void remove(Node* node) {
        node->left->right = node->right;
        node->right->left = node->left;
        delete node;
    }

    /** Inserts a new key <Key> with value 1. Or increments an existing key by 1. */
    void inc(string key) {
        if (hash.count(key) == 0) hash[key] = add_to_right(left, key, 1);
        else {
            auto t = hash[key];
            t->S.erase(key);
            hash[key] = add_to_right(t, key, t->val + 1);
            if (t->S.empty()) remove(t);
        }
    }

    /** Decrements an existing key by 1. If Key's value is 1, remove it from the data structure. */
    void dec(string key) {
        if (hash.count(key) == 0) return;
        auto t = hash[key];
        t->S.erase(key);
        if (t->val > 1) {
            hash[key] = add_to_left(t, key, t->val - 1);
        } else {
            hash.erase(key);
        }
        if (t->S.empty()) remove(t);
    }

    /** Returns one of the keys with maximal value. */
    string getMaxKey() {
        if (right->left != left) return *right->left->S.begin();
        return "";
    }

    /** Returns one of the keys with Minimal value. */
    string getMinKey() {
        if (left->right != right) return *left->right->S.begin();
        return "";
    }
};

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne* obj = new AllOne();
 * obj->inc(key);
 * obj->dec(key);
 * string param_3 = obj->getMaxKey();
 * string param_4 = obj->getMinKey();
 */

```
##### LeetCode 433. 最小基因变化
```cpp
class Solution {
public:
    int minMutation(string start, string end, vector<string>& bank) {
        unordered_set<string> S;
        for (auto& s: bank) S.insert(s);
        unordered_map<string, int> dist;
        queue<string> q;
        q.push(start);
        dist[start] = 0;
        char chrs[4] = {'A', 'T', 'C', 'G'};

        while (q.size()) {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < t.size(); i ++ ) {
                auto s = t;
                for (char c: chrs) {
                    s[i] = c;
                    if (S.count(s) && dist.count(s) == 0) {
                        dist[s] = dist[t] + 1;
                        if (s == end) return dist[s];
                        q.push(s);
                    }
                }
            }
        }
        return -1;
    }
};

```
##### LeetCode 434. 字符串中的单词数
```cpp
class Solution {
public:
    int countSegments(string s) {
        int res = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == ' ') continue;
            int j = i + 1;
            while (j < s.size() && s[j] != ' ') j ++ ;
            res ++ ;
            i = j - 1;
        }
        return res;
    }
};
```
##### LeetCode 435. 无重叠区间
```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& q) {
        sort(q.begin(), q.end(), [](vector<int> a, vector<int> b) {
            return a[1] < b[1];
        });
        if (q.empty()) return 0;
        int res = 1, r = q[0][1];
        for (int i = 1; i < q.size(); i ++ )
            if (q[i][0] >= r) {
                res ++;
                r = q[i][1];
            }
        return q.size() - res;
    }
};
```





### 究极班- Week 19 (第 436 ~ 438、440 ~ 443、445 ~ 457 题)

##### LeetCode 436. 寻找右区间
```cpp
class Solution {
public:
    vector<int> findRightInterval(vector<vector<int>>& q) {
        int n = q.size();
        for (int i = 0; i < n; i ++ ) q[i].push_back(i);
        sort(q.begin(), q.end());
        vector<int> res(n, -1);
        for (auto& x: q) {
            int l = 0, r = n - 1;
            while (l < r) {
                int mid = l + r >> 1;
                if (q[mid][0] >= x[1]) r = mid;
                else l = mid + 1;
            }
            if (q[r][0] >= x[1]) res[x[2]] = q[r][2];
        }
        return res;
    }
};
```
##### LeetCode 437. 路径总和 III
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> cnt;
    int res = 0;

    int pathSum(TreeNode* root, int sum) {
        cnt[0] ++ ;
        dfs(root, sum, 0);
        return res;
    }

    void dfs(TreeNode* root, int sum, int cur) {
        if (!root) return;
        cur += root->val;
        res += cnt[cur - sum];
        cnt[cur] ++ ;
        dfs(root->left, sum, cur), dfs(root->right, sum, cur);
        cnt[cur] -- ;
    }
};
```
##### LeetCode 438. 找到字符串中所有字母异位词
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        unordered_map<char, int> cnt;
        for (auto c: p) cnt[c] ++ ;
        vector<int> res;
        int tot = cnt.size();
        for (int i = 0, j = 0, satisfy = 0; i < s.size(); i ++ ) {
            if ( -- cnt[s[i]] == 0) satisfy ++ ;
            while (i - j + 1 > p.size()) {
                if (cnt[s[j]] == 0) satisfy -- ;
                cnt[s[j ++ ]] ++ ;
            }
            if (satisfy == tot) res.push_back(j);
        }
        return res;
    }
};
```
##### LeetCode 440. 字典序的第K小数字
```cpp
class Solution {
public:

    int f(int prefix, int n) {
        long long p = 1;
        auto A = to_string(n), B = to_string(prefix);
        int dt = A.size() - B.size();
        int res = 0;
        for (int i = 0; i < dt; i ++ ) {
            res += p;
            p *= 10;
        }
        A = A.substr(0, B.size());
        if (A == B) res += n - prefix * p + 1;
        else if (A > B) res += p;
        return res;
    }

    int findKthNumber(int n, int k) {
        int prefix = 1;
        while (k > 1) {
            int cnt = f(prefix, n);
            if (k > cnt) {
                k -= cnt;
                prefix ++ ;
            } else {
                k -- ;
                prefix *= 10;
            }
        }
        return prefix;
    }
};
```
##### LeetCode 441. 排列硬币
```cpp
class Solution {
public:
    int arrangeCoins(int n) {
        return (-1 + sqrt(1 + 8.0 * n)) / 2;
    }
};
```
##### LeetCode 442. 数组中重复的数据
```cpp
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        for (auto x: nums) {
            int p = abs(x) - 1;
            nums[p] *= -1;
            if (nums[p] > 0) res.push_back(abs(x));
        }
        return res;
    }
};
```
##### LeetCode 443. 压缩字符串
```cpp
class Solution {
public:
    int compress(vector<char>& s) {
        int k = 0;
        for (int i = 0; i < s.size(); i ++ ) {
            int j = i + 1;
            while (j < s.size() && s[j] == s[i]) j ++ ;
            int len = j - i;
            s[k ++ ] = s[i];
            if (len > 1) {
                int t = k;
                while (len) {
                    s[t ++ ] = '0' + len % 10;
                    len /= 10;
                }
                reverse(s.begin() + k, s.begin() + t);
                k = t;
            }
            i = j - 1;
        }
        return k;
    }
};
```
##### LeetCode 445. 两数相加 II
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverse(ListNode* head) {
        auto a = head, b = head->next;
        while (b) {
            auto c = b->next;
            b->next = a;
            a = b, b = c;
        }
        head->next = NULL;
        return a;
    }

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        l1 = reverse(l1), l2 = reverse(l2);
        auto head = new ListNode(-1);
        int t = 0;
        while (l1 || l2 || t) {
            if (l1) t += l1->val, l1 = l1->next;
            if (l2) t += l2->val, l2 = l2->next;
            auto cur = new ListNode(t % 10);
            t /= 10;
            cur->next = head->next;
            head->next = cur;
        }
        return head->next;
    }
};
```
##### LeetCode 446. 等差数列划分 II - 子序列
```cpp
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& a) {
        typedef long long LL;
        int n = a.size();
        vector<unordered_map<LL, int>> f(n);
        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int k = 0; k < i; k ++ ) {
                LL j = (LL)a[i] - a[k];
                auto it = f[k].find(j);
                int t = 0;
                if (it != f[k].end()) {
                    t = it->second;
                    res += t;
                }
                f[i][j] += t + 1;
            }
        return res;
    }
};
```
##### LeetCode 447. 回旋镖的数量
```cpp
class Solution {
public:
    int numberOfBoomerangs(vector<vector<int>>& p) {
        int res = 0;
        for (int i = 0; i < p.size(); i ++ ) {
            unordered_map<int, int> cnt;
            for (int j = 0; j < p.size(); j ++ )
                if (i != j) {
                    int dx = p[i][0] - p[j][0];
                    int dy = p[i][1] - p[j][1];
                    int dist = dx * dx + dy * dy;
                    cnt[dist] ++ ;
                }
            for (auto [d, c]: cnt) res += c * (c - 1);
        }
        return res;
    }
};
```
##### LeetCode 448. 找到所有数组中消失的数字
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        for (auto x: nums) {
            x = abs(x);
            if (nums[x - 1] > 0) nums[x - 1] *= -1;
        }
        vector<int> res;
        for (int i = 0; i < nums.size(); i ++ )
            if (nums[i] > 0)
                res.push_back(i + 1);
        return res;
    }
};
```
##### LeetCode 449. 序列化和反序列化二叉搜索树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res;
        dfs_s(root, res);
        return res;
    }

    void dfs_s(TreeNode* root, string& res) {
        if (!root) return;
        res += to_string(root->val) + ' ';
        dfs_s(root->left, res), dfs_s(root->right, res);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string str) {
        vector<int> data;
        stringstream ssin(str);
        int x, u = 0;
        while (ssin >> x) data.push_back(x);
        return dfs_d(data, u, INT_MIN, INT_MAX);
    }

    TreeNode* dfs_d(vector<int>& data, int& u, int minv, int maxv) {
        if (u == data.size() || data[u] < minv || data[u] > maxv) return NULL;
        auto root = new TreeNode(data[u ++ ]);
        root->left = dfs_d(data, u, minv, root->val);
        root->right = dfs_d(data, u, root->val + 1, maxv);
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec* ser = new Codec();
// Codec* deser = new Codec();
// string tree = ser->serialize(root);
// TreeNode* ans = deser->deserialize(tree);
// return ans;
```
##### LeetCode 450. 删除二叉搜索树中的节点
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        del(root, key);
        return root;
    }

    void del(TreeNode* &root, int key) {
        if (!root) return;
        if (key == root->val) {
            if (!root->left && !root->right) root = NULL;  // 子节点
            else if (!root->left) root = root->right;  // 只有右儿子
            else if (!root->right) root = root->left;  // 只有左儿子
            else {  // 左右儿子都有
                auto p = root->right;
                while (p->left) p = p->left;  // 找后继
                root->val = p->val;
                del(root->right, p->val);
            }
        }
        else if (key < root->val) del(root->left, key);
        else del(root->right, key);
    }
};

```
##### LeetCode 451. 根据字符出现频率排序
```cpp
class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> cnt;
        for (auto c: s) cnt[c] ++ ;
        sort(s.begin(), s.end(), [&](char a, char b) {
            if (cnt[a] != cnt[b]) return cnt[a] > cnt[b];
            return a < b;
        });
        return s;
    }
};
```
##### LeetCode 452. 用最少数量的箭引爆气球
```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.empty()) return 0;
        sort(points.begin(), points.end(), [](vector<int>& a, vector<int>& b) {
            return a[1] < b[1];
        });
        int res = 1, r = points[0][1];
        for (int i = 1; i < points.size(); i ++ )
            if (points[i][0] > r) {
                res ++ ;
                r = points[i][1];
            }
        return res;
    }
};

```
##### LeetCode 453. 最小移动次数使数组元素相等
```cpp
class Solution {
public:
    int minMoves(vector<int>& nums) {
        int minv = INT_MAX;
        for (auto x: nums) minv = min(minv, x);
        int res = 0;
        for (auto x: nums) res += x - minv;
        return res;
    }
};
```
##### LeetCode 454. 四数相加 II
```cpp
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> cnt;
        for (auto c: C)
            for (auto d: D)
                cnt[c + d] ++ ;
        int res = 0;
        for (auto a: A)
            for (auto b: B)
                res += cnt[-(a + b)];
        return res;
    }
};
```
##### LeetCode 455. 分发饼干
```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int res = 0;
        for (int i = 0, j = 0; i < g.size(); i ++ ) {
            while (j < s.size() && s[j] < g[i]) j ++ ;
            if (j < s.size()) {
                res ++ ;
                j ++ ;
            }
            else break;
        }
        return res;
    }
};
```
##### LeetCode 456. 132模式
```cpp
class Solution {
public:
    bool find132pattern(vector<int>& nums) {
        stack<int> stk;
        int right = INT_MIN;
        for (int i = nums.size() - 1; i >= 0; i -- ) {
            if (nums[i] < right) return true;
            while (stk.size() && nums[i] > stk.top()) {
                right = max(right, stk.top());
                stk.pop();
            }
            stk.push(nums[i]);
        }
        return false;
    }
};

```
##### LeetCode 457. 环形数组循环
```cpp
class Solution {
public:
    bool circularArrayLoop(vector<int>& nums) {
        int n = nums.size(), Base = 10000;
        for (int i = 0; i < n; i ++ ) {
            if (nums[i] >= Base) continue;
            int k = i, S = Base + i, t = nums[k] > 0;
            int last = -1;
            do {
                int p = ((k + nums[k]) % n + n) % n;
                last = nums[k], nums[k] = S;
                k = p;
            } while (k != i && nums[k] < Base && (t ^ (nums[k] > 0)) == 0);
            if (last % n && nums[k] == S) return true;
        }
        return false;
    }
};
```


### 究极班- Week 20(第 458 ~ 464、466 ~ 468、470、472 ~ 480 题)

#####  LeetCode 458. 可怜的小猪
```cpp
class Solution {
public:
    int poorPigs(int n, int minutesToDie, int minutesToTest) {
        int k = minutesToTest / minutesToDie;
        return ceil(log(n) / log(k + 1));
    }
};
```
#####  LeetCode 459. 重复的子字符串
```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int n = s.size();
        s = ' ' + s;
        vector<int> next(n + 1);
        for (int i = 2, j = 0; i <= n; i ++ ) {
            while (j && s[i] != s[j + 1]) j = next[j];
            if (s[i] == s[j + 1]) j ++ ;
            next[i] = j;
        }
        int t = n - next[n];
        return t < n && n % t == 0;
    }
};
```

#####  LeetCode 460. LFU 缓存
```cpp
class LFUCache {
public:
    struct Node {
        Node *left, *right;
        int key, val;
        Node(int _key, int _val) {
            key = _key, val = _val;
            left = right = NULL;
        }
    };
    struct Block {
        Block *left, *right;
        Node *head, *tail;
        int cnt;
        Block(int _cnt) {
            cnt = _cnt;
            left = right = NULL;
            head = new Node(-1, -1), tail = new Node(-1, -1);
            head->right = tail, tail->left = head;
        }
        ~Block() {
            delete head;
            delete tail;
        }
        void insert(Node* p) {
            p->right = head->right;
            head->right->left = p;
            p->left = head;
            head->right = p;
        }
        void remove(Node* p) {
            p->left->right = p->right;
            p->right->left = p->left;
        }
        bool empty() {
            return head->right == tail;
        }
    }*head, *tail;
    int n;
    unordered_map<int, Block*> hash_block;
    unordered_map<int, Node*> hash_node;

    void insert(Block* p) {  // 在p的右侧插入新块，cnt是p->cnt + 1
        auto cur = new Block(p->cnt + 1);
        cur->right = p->right;
        p->right->left = cur;
        p->right = cur;
        cur->left = p;
    }

    void remove(Block* p) {
        p->left->right = p->right;
        p->right->left = p->left;
        delete p;
    }

    LFUCache(int capacity) {
        n = capacity;
        head = new Block(0), tail = new Block(INT_MAX);
        head->right = tail, tail->left = head;
    }

    int get(int key) {
        if (hash_block.count(key) == 0) return -1;
        auto block = hash_block[key];
        auto node = hash_node[key];
        block->remove(node);
        if (block->right->cnt != block->cnt + 1) insert(block);
        block->right->insert(node);
        hash_block[key] = block->right;
        if (block->empty()) remove(block);
        return node->val;
    }

    void put(int key, int value) {
        if (!n) return;
        if (hash_block.count(key)) {
            hash_node[key]->val = value;
            get(key);
        } else {
            if (hash_block.size() == n) {
                auto p = head->right->tail->left;
                head->right->remove(p);
                if (head->right->empty()) remove(head->right);
                hash_block.erase(p->key);
                hash_node.erase(p->key);
                delete p;
            }
            auto p = new Node(key, value);
            if (head->right->cnt > 1) insert(head);
            head->right->insert(p);
            hash_block[key] = head->right;
            hash_node[key] = p;
        }
    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */

```
#####  LeetCode 461. 汉明距离
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int res = 0;
        while (x || y) {
            res += (x & 1) ^ (y & 1);
            x >>= 1, y >>= 1;
        }
        return res;
    }
};
```
#####  LeetCode 462. 最少移动次数使数组元素相等 II
```cpp
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int res = 0;
        for (int i = 0; i < n; i ++ )
            res += abs(nums[i] - nums[n / 2]);
        return res;
    }
};
```
#####  LeetCode 463. 岛屿的周长
```cpp
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        int res = 0;
        for (int i = 0; i < grid.size(); i ++ )
            for (int j = 0; j < grid[i].size(); j ++ )
                if (grid[i][j] == 1) {
                    for (int k = 0; k < 4; k ++ ) {
                        int x = i + dx[k], y = j + dy[k];
                        if (x < 0 || x >= grid.size() || y < 0 || y >= grid[0].size())
                            res ++ ;
                        else if (grid[x][y] == 0) res ++ ;
                    }
                }
        return res;
    }
};
```
#####  LeetCode 464. 我能赢吗
```cpp
class Solution {
public:
    vector<int> f;
    int n, m;

    int dp(int x) {
        if (f[x] != -1) return f[x];
        int sum = 0;
        for (int i = 0; i < n; i ++ )
            if (x >> i & 1)
                sum += i + 1;
        for (int i = 0; i < n; i ++ ) {
            if (x >> i & 1) continue;
            if (sum + i + 1 >= m) return f[x] = 1;
            if (!dp(x + (1 << i))) return f[x] = 1;
        }
        return f[x] = 0;
    }

    bool canIWin(int _n, int _m) {
        n = _n, m = _m;
        if (n * (n + 1) / 2 < m) return false;
        f.resize(1 << n, -1);
        return dp(0);
    }
};
```
#####  LeetCode 466. 统计重复个数
```cpp
class Solution {
public:
    int getMaxRepetitions(string s1, int n1, string s2, int n2) {
        vector<int> cnt;
        unordered_map<int, int> hash;
        for (int i = 0, k = 0; i < n1; i ++ ) {
            for (int j = 0; j < s1.size(); j ++ )
                if (s1[j] == s2[k % s2.size()])
                    k ++ ;
            cnt.push_back(k);
            if (hash.count(k % s2.size())) {  // 出现循环节
                int a = i - hash[k % s2.size()];  // 循环节中有多少个s1
                int b = k - cnt[hash[k % s2.size()]];  // 循环节中匹配了多少个字符
                int res = (n1 - i - 1) / a * b;
                for (int u = 0; u < (n1 - i - 1) % a; u ++ ) {
                    for (int j = 0; j < s1.size(); j ++ )
                        if (s1[j] == s2[k % s2.size()])
                            k ++ ;
                }
                res += k;
                return res / s2.size() / n2;
            }
            hash[k % s2.size()] = i;
        }
        if (cnt.empty()) return 0;
        return cnt.back() / s2.size() / n2;
    }
};
```
#####  LeetCode 467. 环绕字符串中唯一的子字符串
```cpp
class Solution {
public:
    int findSubstringInWraproundString(string p) {
        unordered_map<char, int> cnt;
        for (int i = 0; i < p.size();) {
            int j = i + 1;
            while (j < p.size() && (p[j] - p[j - 1] == 1 || p[j] == 'a' && p[j - 1] == 'z')) j ++ ;
            while (i < j) cnt[p[i]] = max(cnt[p[i]], j - i), i ++ ;
        }
        int res = 0;
        for (auto [a, b]: cnt) res += b;
        return res;
    }
};
```
#####  LeetCode 468. 验证IP地址
```cpp
class Solution {
public:
    vector<string> split(string ip, char t) {
        vector<string> items;
        for (int i = 0; i < ip.size(); i ++ ) {
            int j = i;
            string item;
            while (ip[j] != t) item += ip[j ++ ];
            i = j;
            items.push_back(item);
        }
        return items;
    }

    string check_ipv4(string ip) {
        auto items = split(ip + '.', '.');
        if (items.size() != 4) return "Neither";
        for (auto item: items) {
            if (item.empty() || item.size() > 3) return "Neither";
            if (item.size() > 1 && item[0] == '0') return "Neither";
            for (auto c: item)
                if (c < '0' || c > '9') return "Neither";
            int t = stoi(item);
            if (t > 255) return "Neither";
        }
        return "IPv4";
    }

    bool check(char c) {
        if (c >= '0' && c <= '9') return true;
        if (c >= 'a' && c <= 'f') return true;
        if (c >= 'A' && c <= 'F') return true;
        return false;
    }

    string check_ipv6(string ip) {
        auto items = split(ip + ':', ':');
        if (items.size() != 8) return "Neither";
        for (auto item: items) {
            if (item.empty() || item.size() > 4) return "Neither";
            for (auto c: item)
                if (!check(c)) return "Neither";
        }
        return "IPv6";
    }

    string validIPAddress(string ip) {
        if (ip.find('.') != -1 && ip.find(':') != -1) return "Neither";
        if (ip.find('.') != -1) return check_ipv4(ip);
        if (ip.find(':') != -1) return check_ipv6(ip);
        return "Neither";
    }
};
```


#####  LeetCode  470. 用 Rand7() 实现 Rand10()
```C++
// The rand7() API is already defined for you.
// int rand7();
// @return a random integer in the range 1 to 7

class Solution {
public:
    int rand10() {
        int t = (rand7() - 1) * 7 + rand7();  // 1~49
        if (t > 40) return rand10();
        return (t - 1) % 10 + 1;
    }
};
```
#####  LeetCode  472. 连接词
```C++
class Solution {
public:
    unordered_set<string> hash;

    bool check(string& word) {
        int n = word.size();
        vector<int> f(n + 1, INT_MIN);
        f[0] = 0;
        for (int i = 0; i <= n; i ++ ) {
            if (f[i] < 0) continue;
            for (int j = n - i; j; j -- ) {
                if (hash.count(word.substr(i, j))) {
                    f[i + j] = max(f[i + j], f[i] + 1);
                    if (f[n] > 1) return true;
                }
            }
        }
        return false;
    }

    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        for (auto& word: words) hash.insert(word);
        vector<string> res;
        for (auto& word: words)
            if (check(word))
                res.push_back(word);
        return res;
    }
};

```
#####  LeetCode  473. 火柴拼正方形
```C++
class Solution {
public:
    vector<int> nums;
    vector<bool> st;

    bool dfs(int start, int cur, int length, int cnt) {
        if (cnt == 3) return true;
        if (cur == length) return dfs(0, 0, length, cnt + 1);
        for (int i = start; i < nums.size(); i ++ ) {
            if (st[i]) continue;
            if (cur + nums[i] <= length) {
                st[i] = true;
                if (dfs(i + 1, cur + nums[i], length, cnt)) return true;
                st[i] = false;
            }
            if (!cur || cur + nums[i] == length) return false;
            while (i + 1 < nums.size() && nums[i + 1] == nums[i])
                i ++ ;
        }
        return false;
    }

    bool makesquare(vector<int>& _nums) {
        nums = _nums;
        if (nums.empty()) return false;
        st.resize(nums.size());
        int sum = 0;
        for (auto x: nums) sum += x;
        if (sum % 4) return false;
        sum /= 4;
        sort(nums.begin(), nums.end(), greater<int>());
        return dfs(0, 0, sum, 0);
    }
};

```
#####  LeetCode  474. 一和零
```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        for (auto& str: strs) {
            int a = 0, b = 0;
            for (auto c: str)
                if (c == '0') a ++ ;
                else b ++ ;
            for (int i = m; i >= a; i -- )
                for (int j = n; j >= b; j -- )
                    f[i][j] = max(f[i][j], f[i - a][j - b] + 1);
        }
        return f[m][n];
    }
};
```
#####  LeetCode  475. 供暖器
```C++
class Solution {
public:
    bool check(int mid, vector<int>& houses, vector<int>& heaters) {
        for (int i = 0, j = 0; i < houses.size(); i ++ ) {
            while (j < heaters.size() && abs(heaters[j] - houses[i]) > mid)
                j ++ ;
            if (j >= heaters.size()) return false;
        }
        return true;
    }

    int findRadius(vector<int>& houses, vector<int>& heaters) {
        sort(houses.begin(), houses.end());
        sort(heaters.begin(), heaters.end());
        int l = 0, r = INT_MAX;
        while (l < r) {
            int mid = (long long)l + r >> 1;
            if (check(mid, houses, heaters)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};

```
#####  LeetCode  476. 数字的补数
```C++
class Solution {
public:
    int findComplement(int num) {
        if (!num) return 1;
        int cnt = 0;
        for (int x = num; x; x >>= 1) cnt ++ ;
        return ~num & ((1ll << cnt) - 1);
    }
};
```
#####  LeetCode  477. 汉明距离总和
```C++
class Solution {
public:
    int totalHammingDistance(vector<int>& nums) {
        int res = 0;
        for (int i = 0; i <= 30; i ++ ) {
            int x = 0, y = 0;
            for (auto c: nums)
                if (c >> i & 1) y ++ ;
                else x ++ ;
            res += x * y;
        }
        return res;
    }
};
```
#####  LeetCode  478. 在圆内随机生成点
```C++
class Solution {
public:
    double r, x, y;

    Solution(double radius, double x_center, double y_center) {
        r = radius, x = x_center, y = y_center;
    }

    vector<double> randPoint() {
        double a = (double)rand() / RAND_MAX * 2 - 1;
        double b = (double)rand() / RAND_MAX * 2 - 1;
        if (a * a + b * b > 1) return randPoint();
        return {x + r * a, y + r * b};
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(radius, x_center, y_center);
 * vector<double> param_1 = obj->randPoint();
 */
```
#####  LeetCode  479. 最大回文数乘积
```C++
class Solution {
public:
    int largestPalindrome(int n) {
        typedef long long LL;
        if (n == 1) return 9;
        int maxv = pow(10, n) - 1;
        for (int i = maxv; ;i -- ) {
            auto a = to_string(i);
            auto b = a;
            reverse(b.begin(), b.end());
            auto num = stoll(a + b);
            for (LL j = maxv; j * j >= num; j -- )
                if (num % j == 0)
                    return num % 1337;
        }
        return 0;
    }
};

```
#####  LeetCode  480. 滑动窗口中位数
```C++
class Solution {
public:
    int k;
    multiset<int> left, right;

    double get_medium() {
        if (k % 2) return *right.begin();
        return ((double)*left.rbegin() + *right.begin()) / 2;
    }

    vector<double> medianSlidingWindow(vector<int>& nums, int _k) {
        k = _k;
        for (int i = 0; i < k; i ++ ) right.insert(nums[i]);
        for (int i = 0; i < k / 2; i ++ ) {
            left.insert(*right.begin());
            right.erase(right.begin());
        }
        vector<double> res;
        res.push_back(get_medium());
        for (int i = k; i < nums.size(); i ++ ) {
            int x = nums[i], y = nums[i - k];
            if (x >= *right.begin()) right.insert(x);
            else left.insert(x);
            if (y >= *right.begin()) right.erase(right.find(y));
            else left.erase(left.find(y));

            while (left.size() > right.size()) {
                right.insert(*left.rbegin());
                left.erase(left.find(*left.rbegin()));
            }
            while (right.size() > left.size() + 1) {
                left.insert(*right.begin());
                right.erase(right.begin());
            }
            res.push_back(get_medium());
        }
        return res;
    }
};

```


### 究极班- Week 21 （第 481 ~ 483、485 ~ 486、488、491 ~ 498、500 ~ 504、506 题）

##### LeetCode 481. 神奇字符串
```cpp
class Solution {
public:
    int magicalString(int n) {
        string s = "122";
        for (int i = 2, k = 1; s.size() < n; i ++, k = 3 - k) {
            for (int j = 0; j < s[i] - '0'; j ++ )
                s += to_string(k);
        }
        int res = 0;
        for (int i = 0; i < n; i ++ ) res += s[i] == '1';
        return res;
    }
};
```
##### LeetCode 482. 密钥格式化
```cpp
class Solution {
public:
    string licenseKeyFormatting(string S, int K) {
        string s;
        for (auto c: S)
            if (c != '-')
                s += c;
        string res;
        for (int i = 0; i < s.size() % K; i ++ ) res += toupper(s[i]);
        for (int i = s.size() % K; i < s.size();) {
            if (res.size()) res += '-';
            for (int j = 0; j < K; j ++ )
                res += toupper(s[i ++ ]);
        }
        return res;
    }
};
```
##### LeetCode 483. 最小好进制
```cpp
class Solution {
public:
    string smallestGoodBase(string number) {
        typedef long long LL;
        LL n = stoll(number);
        for (int t = log2(n) + 1; t >= 3; t -- ) {
            LL k = pow(n, 1.0 / (t - 1));
            LL r = 0;
            for (int i = 0; i < t; i ++ ) r = r * k + 1;
            if (r == n) return to_string(k);
        }
        return to_string(n - 1);
    }
};
```
##### LeetCode 485. 最大连续1的个数
```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int res = 0;
        for (int i = 0; i < nums.size(); i ++ ) {
            if (nums[i] == 0) continue;
            int j = i + 1;
            while (j < nums.size() && nums[j] == 1) j ++ ;
            res = max(res, j - i);
            i = j;
        }
        return res;
    }
};
```
##### LeetCode 486. 预测赢家
```cpp
class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> f(n, vector<int>(n));
        for (int len = 1; len <= n; len ++ ) {
            for (int i = 0; i + len - 1 < n; i ++ ) {
                int j = i + len - 1;
                if (len == 1) f[i][j] = nums[i];
                else {
                    f[i][j] = max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1]);
                }
            }
        }
        return f[0][n - 1] >= 0;
    }
};
```
##### LeetCode 488. 祖玛游戏
```cpp
class Solution {
public:
    unordered_map<string, int> f;
    unordered_map<char, int> cnt;
    int ans = 6;

    int findMinStep(string board, string hand) {
        for (auto c: hand) cnt[c] ++ ;
        f[board] = 0;
        dfs(board, get());
        if (ans == 6) ans = -1;
        return ans;
    }

    string clean_up(string s) {
        bool is_changed = true;
        while (is_changed) {
            is_changed = false;
            for (int i = 0; i < s.size(); i ++ ) {
                int j = i + 1;
                while (j < s.size() && s[j] == s[i]) j ++ ;
                if (j - i >= 3) {
                    s = s.substr(0, i) + s.substr(j);
                    is_changed = true;
                    break;
                }
            }
        }
        return s;
    }

    string get() {
        string res;
        for (auto [x, c]: cnt) res += to_string(c);
        return res;
    }

    int h(string board) {  // 至少还需要多少次操作
        sort(board.begin(), board.end());
        int res = 0;
        for (int i = 0; i < board.size();) {
            int j = i + 1;
            while (j < board.size() && board[j] == board[i]) j ++ ;
            if (j - i + cnt[board[i]] < 3) return 6;
            if (j - i <= 2) res += 3 - (j - i);
            i = j;
        }
        return res;
    }

    void dfs(string board, string hand) {
        if (f[board + ' ' + hand] + h(board) >= ans) return;
        for (auto [x, c]: cnt) {
            if (c) {
                -- cnt[x];
                for (int i = 0; i <= board.size(); i ++ ) {
                    auto r = clean_up(board.substr(0, i) + x + board.substr(i));
                    auto s = r + ' ' + get();
                    if (f.count(s) == 0 || f[s] > f[board + ' ' + hand] + 1) {
                        f[s] = f[board + ' ' + hand] + 1;
                        if (r.empty()) ans = min(ans, f[s]);
                        dfs(r, get());
                    }
                }
                ++ cnt[x];
            }
        }
    }
};
```
##### LeetCode 491. 递增子序列
```cpp
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> findSubsequences(vector<int>& nums) {
        dfs(nums, 0);
        return ans;
    }

    void dfs(vector<int>& nums, int start) {
        if (path.size() >= 2) ans.push_back(path);
        if (start == nums.size()) return;
        unordered_set<int> S;
        for (int i = start; i < nums.size(); i ++ )
            if (path.empty() || nums[i] >= path.back()) {
                if (S.count(nums[i])) continue;
                S.insert(nums[i]);
                path.push_back(nums[i]);
                dfs(nums, i + 1);
                path.pop_back();
            }
    }
};
```
##### LeetCode 492. 构造矩形
```cpp
class Solution {
public:
    vector<int> constructRectangle(int area) {
        for (int i = sqrt(area); ; i -- )
            if (area % i == 0)
                return {area / i, i};
        return {};
    }
};
```
##### LeetCode 493. 翻转对
```cpp
class Solution {
public:
    vector<int> w;

    int reversePairs(vector<int>& nums) {
        return merge_sort(nums, 0, nums.size() - 1);
    }

    int merge_sort(vector<int>& nums, int l, int r) {
        if (l >= r) return 0;
        int mid = l + r >> 1;
        int res = merge_sort(nums, l, mid) + merge_sort(nums, mid + 1, r);
        for (int i = l, j = mid + 1; i <= mid; i ++ ) {
            while (j <= r && nums[j] * 2ll < nums[i]) j ++ ;
            res += j - (mid + 1);
        }
        w.clear();
        int i = l, j = mid + 1;
        while (i <= mid && j <= r)
            if (nums[i] <= nums[j]) w.push_back(nums[i ++ ]);
            else w.push_back(nums[j ++ ]);
        while (i <= mid) w.push_back(nums[i ++ ]);
        while (j <= r) w.push_back(nums[j ++ ]);
        for (i = l, j = 0; j < w.size(); i ++, j ++ ) nums[i] = w[j];
        return res;
    }
};
```
##### LeetCode 494. 目标和
```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& a, int S) {
        if (S < -1000 || S > 1000) return 0;
        int n = a.size(), Offset = 1000;
        vector<vector<int>> f(n + 1, vector<int>(2001));
        f[0][Offset] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = -1000; j <= 1000; j ++ ) {
                if (j - a[i - 1] >= -1000)
                    f[i][j + Offset] += f[i - 1][j - a[i - 1] + Offset];
                if (j + a[i - 1] <= 1000)
                    f[i][j + Offset] += f[i - 1][j + a[i - 1] + Offset];
            }
        return f[n][S + Offset];
    }
};
```
##### LeetCode 495. 提莫攻击
```cpp
class Solution {
public:
    int findPoisonedDuration(vector<int>& w, int d) {
        int res = 0;
        for (int i = 1; i < w.size(); i ++ ) res += min(w[i] - w[i - 1], d);
        if (w.size()) res += d;
        return res;
    }
};
```
##### LeetCode 496. 下一个更大元素 I
```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> stk;
        vector<int> q(nums2.size());
        for (int i = nums2.size() - 1; i >= 0; i -- ) {
            int x = nums2[i];
            while (stk.size() && x >= stk.top()) stk.pop();
            if (stk.empty()) q[i] = -1;
            else q[i] = stk.top();
            stk.push(x);
        }

        unordered_map<int, int> hash;
        for (int i = 0; i < nums2.size(); i ++ )
            hash[nums2[i]] = i;
        vector<int> res;
        for (auto x: nums1)
            res.push_back(q[hash[x]]);
        return res;
    }
};
```
##### LeetCode 497. 非重叠矩形中的随机点
```cpp
class Solution {
public:
    int n;
    vector<vector<int>> rects;
    vector<int> s;

    Solution(vector<vector<int>>& _rects) {
        rects = _rects;
        n = rects.size();
        s.push_back(0);
        for (auto& r: rects) {
            int dx = r[2] - r[0] + 1;
            int dy = r[3] - r[1] + 1;
            s.push_back(s.back() + dx * dy);
        }
    }

    vector<int> pick() {
        int k = rand() % s.back() + 1;
        int l = 1, r = n;
        while (l < r) {
            int mid = l + r >> 1;
            if (s[mid] >= k) r = mid;
            else l = mid + 1;
        }
        auto& t = rects[r - 1];
        int dx = t[2] - t[0] + 1;
        int dy = t[3] - t[1] + 1;
        return {rand() % dx + t[0], rand() % dy + t[1]};
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(rects);
 * vector<int> param_1 = obj->pick();
 */
```
##### LeetCode 498. 对角线遍历
```cpp
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& w) {
        vector<int> res;
        if (w.empty() || w[0].empty()) return res;
        int n = w.size(), m = w[0].size();
        for (int i = 0; i < n + m - 1; i ++ ) {
            if (i % 2 == 0) {
                for (int j = min(i, n - 1); j >= max(0, 1 - m + i); j -- )
                    res.push_back(w[j][i - j]);
            } else {
                for (int j = max(0, 1 - m + i); j <= min(i, n - 1); j ++ )
                    res.push_back(w[j][i - j]);
            }
        }
        return res;
    }
};
```
##### LeetCode 500. 键盘行
```cpp
class Solution {
public:
    vector<string> findWords(vector<string>& words) {
        string line[3] = {
            "qwertyuiop",
            "asdfghjkl",
            "zxcvbnm"
        };
        unordered_map<char, int> hash;
        for (int i = 0; i < 3; i ++ )
            for (auto& c: line[i])
                hash[c] = i;

        vector<string> res;
        for (auto& word: words) {
            set<int> S;
            for (auto c: word) S.insert(hash[tolower(c)]);
            if (S.size() == 1) res.push_back(word);
        }
        return res;
    }
};
```
##### LeetCode 501. 二叉搜索树中的众数
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> ans;
    int maxc = 0, curc = 0, last;

    vector<int> findMode(TreeNode* root) {
        dfs(root);
        return ans;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        if (!curc || root->val == last) {
            curc ++;
            last = root->val;
        } else {
            last = root->val;
            curc = 1;
        }
        if (curc > maxc) ans = {last}, maxc = curc;
        else if (curc == maxc) ans.push_back(last);
        dfs(root->right);
    }
};

```
##### LeetCode 502. IPO
```cpp
class Solution {
public:
    int findMaximizedCapital(int k, int W, vector<int>& Profits, vector<int>& Capital) {
        vector<pair<int, int>> q;
        int n = Profits.size();
        for (int i = 0; i < n; i ++ ) q.push_back({Capital[i], Profits[i]});
        sort(q.begin(), q.end());
        priority_queue<int> heap;
        int res = 0, i = 0;
        while (k -- ) {
            while (i < n && q[i].first <= W) heap.push(q[i].second), i ++ ;
            if (heap.empty()) break;
            auto t = heap.top();
            W += t;
            heap.pop();
        }
        return W;
    }
};

```
##### LeetCode 503. 下一个更大元素 II
```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        nums.insert(nums.end(), nums.begin(), nums.end());
        stack<int> stk;
        vector<int> res(n);
        for (int i = n * 2 - 1; i >= 0; i -- ) {
            int x = nums[i];
            while (stk.size() && x >= stk.top()) stk.pop();
            if (i < n) {
                if (stk.empty()) res[i] = -1;
                else res[i] = stk.top();
            }
            stk.push(x);
        }
        return res;
    }
};
```
##### LeetCode 504. 七进制数
```cpp
class Solution {
public:
    string convertToBase7(int num) {
        if (!num) return "0";
        bool is_neg = num < 0;
        num = abs(num);
        string res;
        while (num) res += to_string(num % 7), num /= 7;
        reverse(res.begin(), res.end());
        if (is_neg) res = '-' + res;
        return res;
    }
};

```
##### LeetCode 506. 相对名次
```cpp
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& nums) {
        vector<pair<int, int>> q;
        for (int i = 0; i < nums.size(); i ++ ) q.push_back({nums[i], i});
        sort(q.begin(), q.end(), greater<pair<int,int>>());
        vector<string> res(nums.size());
        for (int i = 0; i < q.size(); i ++ ) {
            int k = q[i].second;
            if (i == 0) res[k] = "Gold Medal";
            else if (i == 1) res[k] = "Silver Medal";
            else if (i == 2) res[k] = "Bronze Medal";
            else res[k] = to_string(i + 1);
        }
        return res;
    }
};
```



### 究极班—— Week 22（第 507 ~ 509、513 ~ 526、528 ~ 530 题）

#####  LeetCode 507. 完美数
```cpp
class Solution {
public:
    bool checkPerfectNumber(int num) {
        long long sum = 0;
        for (int i = 1; i <= num / i; i ++ )
            if (num % i == 0) {
                if (i < num) sum += i;
                if (i != num / i && num / i < num) sum += num / i;
            }
        return sum == num;
    }
};
```


#####  LeetCode 508. 出现次数最多的子树元素和
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> hash;
    vector<int> ans;
    int maxc = 0;

    vector<int> findFrequentTreeSum(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root) {
        if (!root) return 0;
        int sum = root->val + dfs(root->left) + dfs(root->right);
        hash[sum] ++ ;
        if (hash[sum] > maxc) {
            maxc = hash[sum];
            ans = {sum};
        } else if (hash[sum] == maxc) {
            ans.push_back(sum);
        }
        return sum;
    }
};
```
#####  LeetCode 509. 斐波那契数
```cpp
class Solution {
public:
    int fib(int N) {
        int a = 0, b = 1;
        while (N -- ) {
            int c = a + b;
            a = b, b = c;
        }
        return a;
    }
};
```
#####  LeetCode 513. 找树左下角的值
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans, maxd = 0;

    int findBottomLeftValue(TreeNode* root) {
        dfs(root, 1);
        return ans;
    }

    void dfs(TreeNode* root, int d) {
        if (!root) return;
        if (d > maxd) {
            maxd = d;
            ans = root->val;
        }
        dfs(root->left, d + 1), dfs(root->right, d + 1);
    }
};
```
#####  LeetCode 514. 自由之路
```cpp
class Solution {
public:
    int findRotateSteps(string ring, string key) {
        int n = ring.size(), m = key.size();
        ring = ' ' + ring, key = ' ' + key;
        vector<vector<int>> f(m + 1, vector<int>(n + 1, 1e8));
        f[0][1] = 0;
        for (int i = 1; i <= m; i ++ )
            for (int j = 1; j <= n; j ++ )
                if (key[i] == ring[j]) {
                    for (int k = 1; k <= n; k ++ ) {
                        int t = abs(k - j);
                        f[i][j] = min(f[i][j], f[i - 1][k] + min(t, n - t) + 1);
                    }
                }

        int res = 1e8;
        for (int i = 1; i <= n; i ++ ) res = min(res, f[m][i]);
        return res;
    }
};
```
#####  LeetCode 515. 在每个树行中找最大值
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> hash;
    int maxd = 0;

    vector<int> largestValues(TreeNode* root) {
        dfs(root, 1);
        vector<int> res;
        for (int i = 1; i <= maxd; i ++ )
            res.push_back(hash[i]);
        return res;
    }

    void dfs(TreeNode* root, int d) {
        if (!root) return;
        maxd = max(maxd, d);
        if (hash.count(d) == 0) hash[d] = root->val;
        else hash[d] = max(hash[d], root->val);
        dfs(root->left, d + 1);
        dfs(root->right, d + 1);
    }
};
```
#####  LeetCode 516. 最长回文子序列
```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<vector<int>> f(n, vector<int>(n));
        for (int len = 1; len <= n; len ++ )
            for (int i = 0; i + len - 1 < n; i ++ ) {
                int j = i + len - 1;
                if (len == 1) f[i][j] = 1;
                else {
                    if (s[i] == s[j]) f[i][j] = f[i + 1][j - 1] + 2;
                    f[i][j] = max(f[i][j], max(f[i + 1][j], f[i][j - 1]));
                }
            }
        return f[0][n - 1];
    }
};
```
#####  LeetCode 517. 超级洗衣机
```cpp
class Solution {
public:
    int findMinMoves(vector<int>& w) {
        int sum = 0, n = w.size();
        for (auto x: w) sum += x;
        if (sum % n) return -1;
        int avg = sum / n, left = 0, right = sum;
        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            right -= w[i];
            if (i) left += w[i - 1];
            int l = max(avg * i - left, 0);
            int r = max(avg * (n - i - 1) - right, 0);
            res = max(res, l + r);
        }
        return res;
    }
};
```
#####  LeetCode 518. 零钱兑换 II
```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> f(amount + 1);
        f[0] = 1;
        for (auto x: coins)
            for (int i = x; i <= amount; i ++ )
                f[i] += f[i - x];
        return f[amount];
    }
};
```
#####  LeetCode 519. 随机翻转矩阵
```cpp
class Solution {
public:
    int r, c, k;
    unordered_map<int, int> hash;

    Solution(int n_rows, int n_cols) {
        r = n_rows, c = n_cols;
        k = r * c;
    }

    vector<int> flip() {
        int x = rand() % k;
        int y = x;
        if (hash.count(x)) y = hash[x];
        if (hash.count(k - 1)) {
            hash[x] = hash[k - 1];
            hash.erase(k - 1);
        } else {
            hash[x] = k - 1;
        }
        k -- ;
        return {y / c, y % c};
    }

    void reset() {
        k = r * c;
        hash.clear();
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(n_rows, n_cols);
 * vector<int> param_1 = obj->flip();
 * obj->reset();
 */

```

#####  LeetCode  520. 检测大写字母
```C++
class Solution {
public:
    bool check(char c) {
        return c >= 'A' && c <= 'Z';
    }

    bool detectCapitalUse(string word) {
        int s = 0;
        for (auto c: word)
            if (check(c))
                s ++ ;
        return s == word.size() || !s || s == 1 && check(word[0]);
    }
};
```
#####  LeetCode  521. 最长特殊序列 Ⅰ
```C++
class Solution {
public:
    int findLUSlength(string a, string b) {
        if (a == b) return -1;
        return max(a.size(), b.size());
    }
};
```
#####  LeetCode  522. 最长特殊序列 II
```C++
class Solution {
public:
    bool check(string& a, string& b) {
        int k = 0;
        for (auto c: b)
            if (k < a.size() && c == a[k])
                k ++ ;
        return k == a.size();
    }

    int findLUSlength(vector<string>& strs) {
        int res = -1;
        for (int i = 0; i < strs.size(); i ++ ) {
            bool is_sub = false;
            for (int j = 0; j < strs.size(); j ++ )
                if (i != j && check(strs[i], strs[j])) {
                    is_sub = true;
                    break;
                }
            if (!is_sub) res = max(res, (int)strs[i].size());
        }
        return res;
    }
};

```
#####  LeetCode  523. 连续的子数组和
```C++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        if (!k) {
            for (int i = 1; i < nums.size(); i ++ )
                if (!nums[i - 1] && !nums[i])
                    return true;
            return false;
        }
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + nums[i - 1];
        unordered_set<int> hash;
        for (int i = 2; i <= n; i ++ ) {
            hash.insert(s[i - 2] % k);
            if (hash.count(s[i] % k)) return true;
        }
        return false;
    }
};
```
#####  LeetCode  524. 通过删除字母匹配到字典里最长单词
```C++
class Solution {
public:
    bool check(string& a, string& b) {
        int i = 0, j = 0;
        while (i < a.size() && j < b.size()) {
            if (a[i] == b[j]) i ++ ;
            j ++ ;
        }
        return i == a.size();
    }

    string findLongestWord(string s, vector<string>& d) {
        string res;
        for (auto str: d)
            if (check(str, s)) {
                if (res.empty() || res.size() < str.size() || res.size() == str.size() && res > str)
                    res = str;
            }
        return res;
    }
};
```
#####  LeetCode  525. 连续数组
```C++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int, int> hash;
        hash[0] = 0;
        int res = 0;
        for (int i = 1, one = 0, zero = 0; i <= n; i ++ ) {
            int x = nums[i - 1];
            if (x == 0) zero ++ ;
            else one ++ ;
            int s = one - zero;
            if (hash.count(s)) res = max(res, i - hash[s]);
            else hash[s] = i;
        }
        return res;
    }
};

```
#####  LeetCode  526. 优美的排列
```C++
class Solution {
public:
    int countArrangement(int n) {
        vector<int> f(1 << n);
        f[0] = 1;
        for (int i = 0; i < 1 << n; i ++ ) {
            int k = 0;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                    k ++ ;
            for (int j = 0; j < n; j ++ )
                if (!(i >> j & 1)) {
                    if ((k + 1) % (j + 1) == 0 || (j + 1) % (k + 1) == 0)
                        f[i | (1 << j)] += f[i];
                }
        }
        return f[(1 << n) - 1];
    }
};

```
#####  LeetCode  528. 按权重随机选择
```C++
class Solution {
public:
    vector<int> s;

    Solution(vector<int>& w) {
        s = w;
        for (int i = 1; i < s.size(); i ++ ) s[i] += s[i - 1];
    }

    int pickIndex() {
        int x = rand() % s.back() + 1;
        return lower_bound(s.begin(), s.end(), x) - s.begin();
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(w);
 * int param_1 = obj->pickIndex();
 */
```
#####  LeetCode  529. 扫雷游戏
```C++
class Solution {
public:
    int n, m;

    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        n = board.size(), m = board[0].size();
        int x = click[0], y = click[1];
        if (board[x][y] == 'M') {
            board[x][y] = 'X';
            return board;
        }
        dfs(board, x, y);
        return board;
    }

    void dfs(vector<vector<char>>& board, int x, int y) {
        if (board[x][y] != 'E') return;
        int s = 0;
        for (int i = max(x - 1, 0); i <= min(n - 1, x + 1); i ++ )
            for (int j = max(y - 1, 0); j <= min(m - 1, y + 1); j ++ )
                if (i != x || j != y)
                    if (board[i][j] == 'M' || board[i][j] == 'X')
                        s ++ ;
        if (s) {
            board[x][y] = '0' + s;
            return;
        }
        board[x][y] = 'B';
        for (int i = max(x - 1, 0); i <= min(n - 1, x + 1); i ++ )
            for (int j = max(y - 1, 0); j <= min(m - 1, y + 1); j ++ )
                if (i != x || j != y)
                    dfs(board, i, j);
    }
};

```
#####  LeetCode  530. 二叉搜索树的最小绝对差
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans = INT_MAX, last;
    bool is_first = true;

    int getMinimumDifference(TreeNode* root) {
        dfs(root);
        return ans;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        if (is_first) is_first = false;
        else ans = min(ans, root->val - last);
        last = root->val;
        dfs(root->right);
    }
};

```



### 究极班-Week 23 (第 532、535、537 ~ 543、546、547、551 ~ 554、556 ~ 560 题)

##### LeetCode 532. 数组中的 k-diff 数对
```cpp
class Solution {
public:
    int findPairs(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int res = 0;
        for (int i = 0, j = 0; i < nums.size(); i ++ ) {
            while (i + 1 < nums.size() && nums[i + 1] == nums[i]) i ++ ;
            while (j < i && nums[i] - nums[j] > k) j ++ ;
            if (j < i && nums[i] - nums[j] == k) res ++ ;
        }
        return res;
    }
};
```
##### LeetCode 535. TinyURL 的加密与解密
```cpp
class Solution {
public:
    unordered_map<string, string> hash;
    string chars = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";

    string random_str(int k) {
        string res;
        while (k -- ) res += chars[rand() % 62];
        return res;
    }

    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        while (true) {
            string shortUrl = random_str(6);
            if (hash.count(shortUrl) == 0) {
                hash[shortUrl] = longUrl;
                return "http://tinyurl.com/" + shortUrl;
            }
        }
        return "";
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        return hash[shortUrl.substr(19)];
    }
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));

```
##### LeetCode 537. 复数乘法
```cpp
class Solution {
public:
    string complexNumberMultiply(string x, string y) {
        int a, b, c, d;
        sscanf(x.c_str(), "%d+%di", &a, &b);
        sscanf(y.c_str(), "%d+%di", &c, &d);
        return to_string(a * c - b * d) + "+" + to_string(a * d + b * c) + "i";
    }
};
```
##### LeetCode 538. 把二叉搜索树转换为累加树
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int sum = 0;

    TreeNode* convertBST(TreeNode* root) {
        dfs(root);
        return root;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->right);
        int x = root->val;
        root->val += sum;
        sum += x;
        dfs(root->left);
    }
};

```
##### LeetCode 539. 最小时间差
```cpp
class Solution {
public:
    int findMinDifference(vector<string>& timePoints) {
        int res = INT_MAX;
        vector<int> q;
        for (auto t: timePoints) {
            int h, m;
            sscanf(t.c_str(), "%d:%d", &h, &m);
            q.push_back(h * 60 + m);
        }
        sort(q.begin(), q.end());
        for (int i = 1; i < q.size(); i ++ ) res = min(res, q[i] - q[i - 1]);
        res = min(res, 24 * 60 - q.back() + q[0]);
        return res;
    }
};
```
##### LeetCode 540. 有序数组中的单一元素
```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        nums.push_back(nums.back() + 1);
        int l = 0, r = nums.size() / 2 - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid * 2] != nums[mid * 2 + 1]) r = mid;
            else l = mid + 1;
        }
        return nums[r * 2];
    }
};
```
##### LeetCode 541. 反转字符串 II
```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += k * 2) {
            int l = i, r = min(i + k, (int)s.size());
            reverse(s.begin() + l, s.begin() + r);
        }
        return s;
    }
};
```
##### LeetCode 542. 01 矩阵
```cpp
#define x first
#define y second

class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return matrix;
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<int>> dist(n, vector<int>(m, -1));
        typedef pair<int, int> PII;
        queue<PII> q;

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (matrix[i][j] == 0) {
                    dist[i][j] = 0;
                    q.push({i, j});
                }

        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        while (q.size()) {
            auto t = q.front();
            q.pop();

            for (int i = 0; i < 4; i ++ ) {
                int x = t.x + dx[i], y = t.y + dy[i];
                if (x >= 0 && x < n && y >= 0 && y < m && dist[x][y] == -1) {
                    dist[x][y] = dist[t.x][t.y] + 1;
                    q.push({x, y});
                }
            }
        }

        return dist;
    }
};
```
##### LeetCode 543. 二叉树的直径
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans = 0;

    int diameterOfBinaryTree(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root) {
        if (!root) return 0;
        int left = dfs(root->left), right = dfs(root->right);
        ans = max(ans, left + right);
        return max(left, right) + 1;
    }
};

```
##### LeetCode 546. 移除盒子
```cpp
class Solution {
public:
    int removeBoxes(vector<int>& boxes) {
        int n = boxes.size(), INF = 1e8;
        vector<vector<vector<int>>> f(n, vector<vector<int>>(n, vector<int>(n + 1, -INF)));
        vector<vector<int>> g(n, vector<int>(n, -INF));

        for (int len = 1; len <= n; len ++ ) {
            for (int i = 0; i + len - 1 < n; i ++ ) {
                int j = i + len - 1;
                for (int k = 1; k <= len; k ++ ) {
                    if (len == 1) f[i][j][k] = 1;
                    else if (k == 1) f[i][j][k] = 1 + g[i + 1][j];
                    else {
                        for (int u = i + 1; u <= j - k + 2; u ++ ) {
                            if (boxes[i] != boxes[u]) continue;
                            int t = 0;
                            if (i + 1 <= u - 1) t = g[i + 1][u - 1];
                            f[i][j][k] = max(f[i][j][k], t + f[u][j][k - 1] - (k - 1) * (k - 1) + k * k);
                        }
                    }

                    g[i][j] = max(g[i][j], f[i][j][k]);
                }
            }
        }

        return g[0][n - 1];
    }
};
```
##### LeetCode 547. 朋友圈
```cpp
class Solution {
public:
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    int findCircleNum(vector<vector<int>>& M) {
        int n = M.size();
        for (int i = 0; i < n; i ++ ) p.push_back(i);

        int cnt = n;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                if (M[i][j] && find(i) != find(j)) {
                    p[find(i)] = find(j);
                    cnt -- ;
                }

        return cnt;
    }
};
```
##### LeetCode 551. 学生出勤记录 I
```cpp
class Solution {
public:
    bool checkRecord(string s) {
        for (int i = 0, a = 0, l = 0; i < s.size(); i ++ ) {
            if (s[i] == 'A') a ++ , l = 0;
            else if (s[i] == 'L') l ++ ;
            else l = 0;
            if (a > 1 || l > 2) return false;
        }
        return true;
    }
};
```
##### LeetCode 552. 学生出勤记录 II
```cpp
const int mod = 1e9 + 7, N = 100010;
int f[N][2][3];

class Solution {
public:
    int checkRecord(int n) {
        memset(f, 0, sizeof f);
        f[0][0][0] = 1;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < 2; j ++ )
                for (int k = 0; k < 3; k ++ ) {
                    if (!j) f[i + 1][j + 1][0] = (f[i + 1][j + 1][0] + f[i][j][k]) % mod;
                    if (k + 1 <= 2) f[i + 1][j][k + 1] = (f[i + 1][j][k + 1] + f[i][j][k]) % mod;
                    f[i + 1][j][0] = (f[i + 1][j][0] + f[i][j][k]) % mod;
                }

        int res = 0;
        for (int j = 0; j < 2; j ++ )
            for (int k = 0; k < 3; k ++ )
                res = (res + f[n][j][k]) % mod;
        return res;
    }
};

```
##### LeetCode 553. 最优除法
```cpp
class Solution {
public:
    string optimalDivision(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) return "";
        if (n == 1) return to_string(nums[0]);
        if (n == 2) return to_string(nums[0]) + '/' + to_string(nums[1]);

        string res = to_string(nums[0]) + "/(";
        for (int i = 1; i < n - 1; i ++ )
            res += to_string(nums[i]) + '/';
        res += to_string(nums.back()) + ')';
        return res;
    }
};
```
##### LeetCode 554. 砖墙
```cpp
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        unordered_map<int, int> hash;
        for (auto& line: wall) {
            for (int i = 0, s = 0; i < (int)line.size() - 1; i ++ ) {
                s += line[i];
                hash[s] ++ ;
            }
        }

        int res = 0;
        for (auto [k, v]: hash) res = max(res, v);
        return wall.size() - res;
    }
};
```
##### LeetCode 556. 下一个更大元素 III
```cpp
class Solution {
public:
    int nextGreaterElement(int n) {
        string s = to_string(n);
        int k = s.size() - 1;
        while (k && s[k - 1] >= s[k]) k -- ;
        if (!k) return -1;
        int t = k;
        while (t + 1 < s.size() && s[t + 1] > s[k - 1]) t ++ ;
        swap(s[k - 1], s[t]);
        reverse(s.begin() + k, s.end());
        long long r = stoll(s);
        if (r > INT_MAX) return -1;
        return r;
    }
};
```
##### LeetCode 557. 反转字符串中的单词 III
```cpp
class Solution {
public:
    string reverseWords(string s) {
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == ' ') continue;
            int j = i;
            while (j < s.size() && s[j] != ' ') j ++ ;
            reverse(s.begin() + i, s.begin() + j);
            i = j - 1;
        }
        return s;
    }
};
```
##### LeetCode 558. 四叉树交集
```cpp
/*
// Definition for a QuadTree node.
class Node {
public:
    bool val;
    bool isLeaf;
    Node* topLeft;
    Node* topRight;
    Node* bottomLeft;
    Node* bottomRight;

    Node() {
        val = false;
        isLeaf = false;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }

    Node(bool _val, bool _isLeaf) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }

    Node(bool _val, bool _isLeaf, Node* _topLeft, Node* _topRight, Node* _bottomLeft, Node* _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
};
*/

class Solution {
public:
    Node* intersect(Node* t1, Node* t2) {
        if (t1->isLeaf) return t1->val ? t1 : t2;
        if (t2->isLeaf) return t2->val ? t2 : t1;
        t1->topLeft = intersect(t1->topLeft, t2->topLeft);
        t1->topRight = intersect(t1->topRight, t2->topRight);
        t1->bottomLeft = intersect(t1->bottomLeft, t2->bottomLeft);
        t1->bottomRight = intersect(t1->bottomRight, t2->bottomRight);

        if (t1->topLeft->isLeaf && t1->topRight->isLeaf && t1->bottomLeft->isLeaf && t1->bottomRight->isLeaf)
            if (t1->topRight->val == t1->topLeft->val
                && t1->bottomLeft->val == t1->topLeft->val
                && t1->bottomRight->val == t1->topLeft->val) {
                    t1->isLeaf = true;
                    t1->val = t1->topLeft->val;
                    t1->topLeft = t1->topRight = t1->bottomLeft = t1->bottomRight = NULL;
                }

        return t1;
    }
};
```
##### LeetCode 559. N叉树的最大深度
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    int maxDepth(Node* root) {
        if (!root) return 0;
        int res = 0;
        for (auto c: root->children) res = max(res, maxDepth(c));
        return res + 1;
    }
};

```
##### LeetCode 560. 和为K的子数组
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + nums[i - 1];
        unordered_map<int, int> hash;
        hash[0] = 1;
        int res = 0;
        for (int i = 1; i <= n; i ++ ) {
            res += hash[s[i] - k];
            hash[s[i]] ++ ;
        }
        return res;
    }
};
```
