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
##### 31 下一个排列

两遍逆向扫描：1扫描找尽量靠右的不满足降序的数i，2扫描找比i大的尽量靠右的数j,swap(num[i],num[j]), reverse(nums.begin() + i + 1, nums.end())。

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



