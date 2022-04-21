##### offer 26 树的子结构

双递归：recur + isSubStructure

```C++
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if(!A && !B) return true;
        if(!A ||!B) return false;
        return recur(A,B) || isSubStructure(A->left,B) || isSubStructure(A->right,B);
    }

    bool recur(TreeNode* A, TreeNode* B) {
        if(!B) return true;
        if(!A || A->val != B->val){
            return false;
        }
        return recur(A->left,B->left) && recur(A->right,B->right);
    }

};
```

##### leetcode 946 验证栈序列

先push，再判pop,节省最后一次的循环后判断。

```C++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        if(pushed.size()!= popped.size()) return false;
        stack<int> stk;
        int j = 0;
        for(int i = 0;i < pushed.size();i++){
            stk.push(pushed[i]);
            while(!stk.empty() && stk.top() == popped[j]){
                stk.pop();
                j++;
            }
        }
        return stk.empty();
    }
};

```
##### offer 33 二叉树的后序遍历序列

```C++
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        int l = 0;
        int r = postorder.size() - 1;
        return dfs(postorder,l,r);
    }

    bool dfs(vector<int>& postorder,int l,int r){
        if(l > r) return true;
        int root = postorder[r];
        int k = l;
        while(postorder[k] < root) k++;
        for(int j = k; j < r;j++){
            if(postorder[j] < root) {
                return false;
            }
        }
        return dfs(postorder,l,k - 1) && dfs(postorder,k,r - 1);
    }
};
```

##### offer 38 字符串的排列

相同的位置不要再选其他的相同元素

```C++
class Solution {
    vector<string> res;
    string path;
    vector<int> visit;
public:
    vector<string> permutation(string s) {
        visit.resize(s.size());
        sort(s.begin(), s.end());
        dfs(0, s);
        return res;
    }

    void dfs(int index, string s) {
        if (index == s.size()) {
            res.emplace_back(path);
            return;
        }
        for (int i = 0; i < s.size(); i++) {
            if (visit[i]) continue;
            path.push_back(s[i]);
            visit[i] = true;
            dfs(index + 1, s);
            visit[i] = false;
            path.pop_back();
            while (i + 1 < s.size() && s[i] == s[i + 1]) i++;
        }
    }
};
```

#####  Offer 40. 最小的k个数

y总 快速排序 和 快速选择 模板： https://www.acwing.com/blog/content/8597/

快排模板：    

    int i = l - 1, j = r + 1, x = q[l + r >> 1];       
    双指针划分               
    i,j 两指针相遇后，只有两种情况，(1) i == j; (2) i > j             
    递归两侧要用j     
         
快选模板： 
   
    if (k <= j - l + 1) return quick_select(nums, l, j, k);       
    else return quick_select(nums, j + 1, r, k - (j - l + 1));      
    
    
```C++
//快选模板
int quick_select(int q[], int l, int r, int k) {
    // 当数组为空时，quick_sort(q, 0, len(q) - 1)中l = 0, r = -1, 会出现 l > r的情况
    // 除了 一开始 需要判断 l > r, 以后 只需要 判断 l == r 即可
    // 因为 快速选择 传进来的 序列 最少有一个元素, 所以 一般 len(q) >= 1, 不用判断 l > r 也可以
    // 除了 len(q) == 0 会造成 一开始 l > r 之外，以后return 的时候 肯定有 l == r
    if (l >= r) return q[l];

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j) // i,j 两指针相遇后，只有两种情况，(1) i == j; (2) i > j
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }

    if (j - l + 1 >= k) return quick_select(q, l, j, k);
    else return quick_select(q, j + 1, r, k - (j - l + 1));
}
```

    
```C++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int> &arr, int k) {
        quickSort(arr, 0, arr.size() - 1);
        vector<int> res;
        for (int i = 0; i < k; i++) {
            res.emplace_back(arr[i]);
        }
        return res;
    }

    void quickSort(vector<int> &arr, int l, int r) {
        if (l >= r) return;
        int i = l - 1, j = r + 1, x = arr[l + r >> 1];
        while (i < j) {
            do { i++; } while (arr[i] < x);
            do { j--; } while (arr[j] > x);
            if (i < j) swap(arr[i], arr[j]);
        }
        quickSort(arr, l, j);
        quickSort(arr, j + 1, r);
    }
};
```

##### Offer 44. 数字序列中某一位的数字

模拟，数位计数问题

```C++
class Solution
{
public:
    int findNthDigit(int n)
    {
        long long start = 1;
        long long digit = 1;
        long long count = 9;
        while (n > count)
        {
            n -= count;
            start *= 10;
            digit += 1;
            count = 9 * digit * start;
        }
        long long res = start + (n - 1)/digit;
        return to_string(res)[(n - 1) % digit] - '0';
    }
};

```

##### Offer 45. 把数组排成最小的数

自定义排序：
        static bool cmp(string a,string b){
            return a + b < b + a;
        }
        
```C++
class Solution {
    static bool cmp(string a,string b){
        return a + b < b + a;
    }
public:
    string minNumber(vector<int>& nums) {
        
        vector<string> nums_strs;
        for(auto & num:nums){
            nums_strs.emplace_back(to_string(num));
        }
        sort(nums_strs.begin(),nums_strs.end(),cmp);
        string res;
        for(auto & num_str:nums_strs){
            res += num_str;
        }
        return res;
    }
};
```


318  875   647  713  567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 

