#方法总结类

## 【alkf057】sjbc“前缀和”与“差分”使用总结


测试机器（2020-05-29）

```C++

涉及连续子数组求和时，通常使用“前缀和”来解决。最简单的一维前缀和如下：

   前缀和为从第0项到第i项的和，即S[i]=A[0]+A[1]+…+A[i]，计算时基于前值S[i]= S[i-1] +A[i]
   某项值可以表示为相邻前缀和之差：A[i]=S[i]−S[i−1]
   从i到j的子数组的和：A[i]+…+A[j]=S[j]−S[i−1]
 
struct Computer {
public:
    long long preSum;
    int idx;
    Computer(long long s, int i) : preSum(s), idx(i)
    {
    }
    friend bool operator<(const Computer &c1, const Computer &c2)
    {
        if (c1.preSum == c2.preSum) {
            return c1.idx < c2.idx;
        } else {
            return c1.preSum < c2.preSum;
        }
    }
};
class Solution {
public:
    vector<int> GetIndexList(vector<int> &score)
    {
        long long ans = LONG_MIN;
        vector<int> res = { 0, 0 };
        unordered_map<int, Computer> showIndexMap;
        vector<long long> preSum;
        GetPreSum(score, preSum);
        for (int i = 0; i < score.size(); i++) {
            int cur = score[i];
            if (showIndexMap.find(cur) == showIndexMap.end()) {
                Computer temp(preSum[i], i);
                showIndexMap.insert(make_pair(cur, temp));
            } else {
                auto iter = showIndexMap.find(cur);
                Computer pre((iter->second).preSum, (iter->second).idx);
                long long subSum = preSum[i + 1] - pre.preSum;
                if (subSum > ans) {
                    ans = subSum;
                    res[0] = pre.idx;
                    res[1] = i;
                }
                Computer temp(preSum[i], i);
                if (temp < pre) {
                    (iter->second).preSum = temp.preSum;
                    (iter->second).idx = temp.idx;
                }
            }
        }
        return (ans == LONG_MIN) ? vector<int>() : res;
    }
 
private:
    void GetPreSum(vector<int> &score, vector<long long> &preSum)
    {
        preSum.push_back(0);
        for (int i = 0; i < score.size(); i++) {
            preSum.push_back(preSum[i] + score[i]);
        }
    }
};
 
```

矩阵元素求和（2019/12/6）
```java
class Solution {
    public int[][] calculate(int[][] matrixA) {
        int[][] res = new int[matrixA.length][matrixA[0].length];
        for (int i = 0; i < matrixA.length; i++) {
            for (int j = 0; j < matrixA[0].length; j++) {
                int upper = i > 0 ? res[i - 1][j] : 0;
                int left = j > 0 ? res[i][j - 1] : 0;
                int upperLeft = i > 0 && j > 0 ? res[i - 1][j - 1] : 0;
                res[i][j] = upper + left - upperLeft + matrixA[i][j];
            }
        }
        return res;
    }
}

```


区域的传感器数量总和，面经最大

子矩阵求和

```C++

class Solution {
    public int sensorsNumCategory(int[][] sensors, int cnt) {
        int m = sensors.length;
        int n = sensors[0].length;
        int[][] mat = new int[m + 1][n + 1];
        int[][] colored = new int[m + 1][n + 1];
        HashSet<Integer> book = new HashSet<>();
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                mat[i][j] = 0;
                colored[i][j] = 0;
            }
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                mat[i][j] = mat[i - 1][j] + mat[i][j - 1] - mat[i - 1][j - 1] + sensors[i - 1][j - 1];
            }
        }
        int mx = calSum(1, 1, cnt, cnt, mat);
        for (int i = 1; i + cnt - 1 <= m; i++) {
            for (int j = 1; j + cnt - 1 <= n; j++) {
                mx = Math.max(mx, calSum(i, j, i + cnt - 1, j + cnt - 1, mat));
            }
        }
        for (int i = 1; i + cnt - 1 <= m; i++) {
            for (int j = 1; j + cnt - 1 <= n; j++) {
                if (calSum(i, j, i + cnt - 1, j + cnt - 1, mat) == mx) {
                    ++colored[i][j];
                    if (i + cnt <= m)
                        --colored[i + cnt][j];
                    if (j + cnt <= n)
                        --colored[i][j + cnt];
                    if (i + cnt <= m && j + cnt <= n)
                        ++colored[i + cnt][j + cnt];
                }
            }
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                colored[i][j] = colored[i - 1][j] + colored[i][j - 1] - colored[i - 1][j - 1] + colored[i][j];
                if (colored[i][j] > 0) {
                    book.add(sensors[i - 1][j - 1]);
                }
            }
        }
        return book.size();
    }
 
    public static int calSum(int x1, int y1, int x2, int y2, int[][] mat) {
        return mat[x2][y2] - mat[x1 - 1][y2] - mat[x2][y1 - 1] + mat[x1 - 1][y1 - 1];
    }
}                                 
```


二叉树平分路径（2021-05-21）

```C++
#define NODE_COUNT 10001
void VisitNode(struct TreeNode *node, int *preSum, int *used, int pathSum, int depth, int *nodeSum)
{
    if (node == NULL) {
        used[depth] = 0;
        return;
    }
    pathSum += node->val;
    *nodeSum += node->val;
    if ((node->left == NULL) && (node->right == NULL)) {
        for (int i = 0; i < depth; i ++) {
            if (used[i] == 1) {
                continue;
            }
            if (preSum[i] == (pathSum - preSum[i + 1])) {
                *nodeSum -= (preSum[i + 1] - preSum[i]);
                used[i] = 1;
            }
        }
        used[depth] = 0;
        return;
    }
    preSum[depth + 1] = preSum[depth] + node->val;
    VisitNode(node->left, preSum, used, pathSum, depth + 1, nodeSum);
    VisitNode(node->right, preSum, used, pathSum, depth + 1, nodeSum);
    used[depth] = 0;
    return;
}
int BisectTreePath(struct TreeNode *root)
{
   int preSum[NODE_COUNT] = {0};
   int used[NODE_COUNT] = {0};
   int pathSum = 0;
   int nodeSum = 0;
   VisitNode(root, preSum, used, pathSum, 0, &nodeSum);
   return nodeSum;
}
```

差分数组：

```C++

差分数组定义
真实数组a = {a[1]、a[2]、…、a[n]}          // 各点真实数据
差分数组df = {df[1]、df[2]、…、df[n]}      // 各点数据的变更值 
df[i] = a[i] - a[i-1]                      // 差分数组各点数据为真实数据的变更值
a[i] = df[1] + df[2] …+ df[i]              // 差分数组的前缀和即为真实数组
a[i] = a[i-1] + df[i]                      // 真实数据也可以从上一点数据+变更值求出

3.3.2      差分算法解题模板（题目中给出的往往为数据变更点）
·         初始化差分数组：根据数据变更点构造差分数组，常用map<int, int> （key: 变更点， value：变更值）
·         根据差分数组求原始数组：对差分数组求前缀和，求出的即为原始数组
·         根据原始数组判断结果
```


人数最多的时段（2021-03-05）
```C++
class Solution {
public:
    vector<vector<int>> FindPeriods(const vector<vector<int>>& persons) {
        int len = persons.size();
        map<int, int> diff;         // 差分数组        
        // 记录差分数组
        for (int i = 0; i < len; ++i) {
            diff[persons[i][0]] += persons[i][2];
            diff[persons[i][1] + 1] -= persons[i][2];
        }
        
        int peopleCnt = 0;          // 当前的人数
        int maxPeople = 0;          // 最大的人数
        for (auto it = diff.begin(); it != diff.end(); ++it) {
            peopleCnt += it->second;
            // 利用diff记录当前人数
            it->second = peopleCnt;
            // 刷新最大人数
            maxPeople = max(maxPeople, peopleCnt);
        }
        
        vector<vector<int>> ans;
        bool recordFlag = false;
        for (auto it = diff.begin(); it != diff.end(); ++it) {
            if (it->second == maxPeople) {
                // 如果前一区间没有结束，不能插入新的区间
                if (!recordFlag) {
                    ans.push_back(vector<int>(2, it->first));
                }
                // 标记需要记录区间的结束时间
                recordFlag = true;
            } else if (recordFlag) {
                recordFlag = false;
                // 刷新区间的结束时间
                ans[ans.size() - 1][1] = it->first - 1;
            }
        }
        
        return ans;
    }
};
```


## 【alkf055】sjbc优先队列使用总结

优先队列的入和出与元素（数据）进的次序无关，而是由设定的优先级来决定元素的弹出次序，优先级最高的元素最先得到服务，优先级相同的元素按照其在优先队列中的顺序得到服务

实现优先队列的方式有多种，如数组、链表、平衡二叉树（如：AVL树和RB树）、堆等

堆是一种完全二叉树，可以分为小顶堆和大顶堆。小顶堆指的是堆顶元素（即树的根节点）为堆中最小值，且每一个节点的值都必须 小于等于 其孩子节点的值；反之即大顶堆

对堆的操作主要有两个，即插入或删除，且都是在顶端进行。由于堆是一个完全二叉树，删除和添加后都需要保证是完全二叉树，一般情况是先和最后一个节点进行交换，然后再进行删除和添加。

| **语言** | **数据结构**   | **常用函数**          | **是否系统自带** | **默认大顶堆/小顶堆** |
| -------- | -------------- | --------------------- | ---------------- | --------------------- |
| C++      | priority_queue | push、top、pop、empty | 系统自带         | 大顶堆                |
| Python   | heapq          | heappush、heappop     | 系统自带         | 小顶堆                |

1） 任务调度：先处理高优先级的任务，再处理普通任务；可增加新的任务到优先队列中，并按照优先级排序

2） Top K问题：找到满足要求的前K个元素

认证题目：2020-09-25 CI任务调度

```python
每次选择任务中优先级最高且执行时间较长的一个任务，把它交给VM池中的一个空闲VM处理

如果还有VM空闲，继续选择下一个任务；

如果VM都在处理中，则等待其中一个最先处理完的VM，继续选择下一个任务；

import heapq
class Solution:
    def task_scheduler(self, resources_num, task_attributes):
        task_attributes.sort(key=lambda x: (x[1], -x[0]), reverse=True)
 
        res = [0 for i in range(resources_num)]
        sums = [0 for i in range(resources_num)]
 
        length = min(resources_num, len(task_attributes))
        for i in range(length):
            use_time, _ = task_attributes.pop()
            res[i] = use_time
            sums[i] = use_time
 
        heap = [(item, index) for index, item in enumerate(res)]
        heapq.heapify(heap)
 
        while task_attributes:
            _, index = heapq.heappop(heap)
            use_time, _ = task_attributes.pop()
            sums[index] = sums[index] + use_time
            heapq.heappush(heap, (sums[index], index))
 
        return max(sums) % 1000000007
 
if __name__ == "__main__":
    resources_num = int(input().strip())
    tasks_num = int(input().strip())
    task_attributes = [list(map(int, input().strip().split(' '))) for _ in range(tasks_num)]
    fun = Solution()
    ret = fun.task_scheduler(resources_num, task_attributes)
    print(ret)
```


rztm：2020-11-20除湿机

```C++
class Solution {
public:
    int ComputeTime(int humidity, int ability) {
        humidity = max(0, humidity);
        return (humidity + ability - 1) / ability;
    }
    
    int NumCycles(const vector<int>& humidity, int single, int multiple)
    {
        if(humidity.size() == 1) {
            int x = max(single, multiple);
            
            return ComputeTime(humidity[0], x);
        }
        
        int res = 0;
        
        if(single < multiple) {
            int diff = multiple - single;
            
            priority_queue<int, vector<int>, greater<int>> record;
            for(int i = 0; i < humidity.size(); ++i) record.push(humidity[i]);
            
            while(true) {
                auto it = record.top();
                record.pop();
                auto jt = record.top();
                
                int time = ComputeTime(jt - it, diff);
                time = max(1, time);
                if(time * single + res * multiple >= it) {
                    res = res + ComputeTime(it - res * multiple, single);
                    
                    while(!record.empty()) {
                        res = max(res, ComputeTime(record.top(), multiple));
                        record.pop();
                    }
                    
                    return res;
                } else {
                    res += time;
                    record.push(it + time * diff);
                }
            }
        } else if(single > multiple) {
            int diff = single - multiple;
            
            priority_queue<int, vector<int>, less<int>> record;
            for(int i = 0; i < humidity.size(); ++i) record.push(humidity[i]);
            
            while(true) {
                auto it = record.top();
                record.pop();
                auto jt = record.top();
                
                int time = ComputeTime(it - jt, diff);
                time = max(1, time);
                if(time * multiple + res * multiple >= jt) {
                    res = res + ComputeTime(it - res * multiple, single);
                    res = max(res, ComputeTime(jt, multiple));
                    return res;
                } else {
                    res += time;
                    record.push(it - time * diff);
                }
            }
        } else {
            int max_humidity = INT_MIN;
            
            for(int i = 0; i < humidity.size(); ++i) max_humidity = max(max_humidity, humidity[i]);
            
            return ComputeTime(max_humidity, single);
        }
    }
};
```

2021-08-13礼物组合

```C++

找价格第n低的组合时，我们不需要考虑所有的组合，我们只需要考虑第1至第n-1低之间的组合的下一步。

假设第n-1低的组合为(goodsPrice[i], fruitsPrice[j])，那么需要考虑的组合为(goodsPrice[i+1], fruitsPrice[j]) 和 (goodsPrice[i], fruitsPrice[j+1])，

#include <bits/stdc++.h>
using namespace std;
 
class Solution {
public:
    // 待实现函数，在此函数中填入答题代码;
    vector<vector<int>> GetTopTPack(const vector<int>& goodsPrice, const vector<int>& fruitsPrice, int num) const
    {
        using TI = tuple<pair<int, int>, int, int>;
        set<pair<int, int>> used;
        vector<vector<int>> result;
        vector<int> a = goodsPrice;
        vector<int> b = fruitsPrice;
        int n = a.size();
        int m = b.size();
        priority_queue<TI, vector<TI>, greater<TI>> pq;
        sort(a.begin(), a.end());
        sort(b.begin(), b.end());
        pq.emplace(make_pair(a[0] + b[0], a[0]), 0, 0);
        for (int t = 0; t < num; t++) {
            auto [key, i, j] = pq.top();
            pq.pop();
            result.push_back({a[i], b[j]});
            if (i + 1 < n && used.find(make_pair(i + 1, j)) == used.end()) {
                pq.emplace(make_pair(a[i + 1] + b[j], a[i + 1]), i + 1, j);
                used.insert(make_pair(i + 1, j));
            }
            if (j + 1 < m && used.find(make_pair(i, j + 1)) == used.end()) {
                pq.emplace(make_pair(a[i] + b[j + 1], a[i]), i, j + 1);
                used.insert(make_pair(i, j + 1));
            }
        }
        return result;
    }
};

```

## 【alkf041】 sjbc二分法使用总结

| 语言   | 支持二分的常用数据结构             | 找值          | lower_bound     | upper_bound  |
| ------ | ---------------------------------- | ------------- | --------------- | ------------ |
| C++    | vector  multiset/set  map/multimap | binary_search | lower_bound     | upper_bound  |
| Python | List                               | bisect_left   | **bisect**_left | bisect_right |

**C++**:

Ÿ  升序序列

lower_bound：返回第一个 >= 目标值的迭代器，找不到则返回end()。

upper_bound：返回第一个 > 目标值的迭代器，找不到则返回end()。

Ÿ  降序序列：需要重载或者目标比较器，例如greater<int >()

lower_bound：返回第一个 <= 目标值的迭代器，找不到则返回end()。

upper_bound：返回第一个 < 目标值的迭代器，找不到则返回end()。

  eg: // 返回第一个小于等于目标值的迭代器

  **lower_bound**(vec.begin(), vec.end(), 8, greater<int>());

  // 返回第一个小于目标值的迭代器

  **upper_bound**(vec.begin(), vec.end(), 8, greater<int>());

 bool isFind = **binary**_**search**(vec.begin(), vec.end(), 7);

// 返回第一个大于等于目标值的迭代器

 vector<int>::iterator iter1 = **lower**_bound(vec.begin(), vec.end(), 8);

 // 返回第一个大于目标值的迭代器

 vector<int>::iterator iter2 = upper_bound(vec.begin(), vec.end(), 8);



 **Python:** 

**Ÿ**  **升序序列**

a） bisect_left (同lower_bound)：返回插入位置的索引i，使得a[:i]的所有元素都 < 目标值，a[i:]的所有元素 >=目标值；

b） bisect_right (同upper_bound)：返回插入位置的索引i，使得a[:i]的所有元素都 <= 目标值，a[i:]的所有元素 >目标值。

Ÿ  降序序列：

不支持。可以先反转之后再使用上述方法模拟实现。

eg：

import bisect

point_left = bisect.bisect_left(num_list, 7)

point_right = bisect.bisect_right(num_list, 8)


2021-03-05 整理仓库

```cpp
class Solution {

public:

    const int MO = 1e9 + 7;
    int ArrangeBoxes(const vector<int>& boxes)
    {
        int n = boxes.size();
        vector<long long> sum(n + 1);
        sum[0] = 0;
        long long total = 0;
        for (int i = 1; i <= n; ++i) {
            sum[i] = sum[i - 1] + boxes[i - 1];
            total += boxes[i - 1];
        }

        int i = 0;
        int j = n - 1;
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            long long left = sum[i + 1];
            int begin = lower_bound(sum.begin(), sum.end(), (long long)2 * left) - sum.begin();
            int end = upper_bound(sum.begin(), sum.end(), (long long)(total + left) / 2) - sum.begin();
            if (end > begin) {
                ans += (end - begin);
            }
            ans = ans % MO;
        }
        return ans % MO;
    }

};

```

rztm：2019-12-06生产线消耗电容速度

2020-09-11道路植树

```C++
class Solution {
public:
    static constexpr int DIVIDE = 2;
    int DistanceBetweenTree(int num, const vector<vector<int>>& areas)
    {
        if (!areas.size()) {
            return -1;
        }
 
        int start = 1;
        int end = areas.back().back() + 1;
 
        while (start < end) {
            int trees = 0;
            auto mid = (start + end) / DIVIDE;
            int current = areas.front().front();
            for (auto &i : areas) {
                current = max(current, i[0]);
                while (i[0] <= current && i[1] >= current) {
                    trees++;
                    current += mid;
                }
            }
 
            if (trees >= num) {
                start = mid + 1;
            } else {
                end = mid;
            }
        }
 
        return !(start - 1) ? -1 : start - 1;
    }
};

```

##  【alkf030】sjbc字典序排序总结

字典序：指按照单词出现在字典的顺序进行排序的方法。先按照第一个字母以 0、1、2 … 9，a、b、c … z 等的ASCII码值顺序排列，如果第一个字母一样，那么比较第二个、第三个乃至后面的字母。如果比到最后两个单词不一样长（比如 sigh 和 sight），那么把短者排在前。

**C++** 

1. 字符串比较：类似于C语言的strcmp函数，C++标准库string类重载了大于、等于、小于等运算符，可直接用于字符串的比较（基于字典序）。例如 "apple" < "banana", "9"> "10" 。

2. 字符串序列排序：同样的，类似于C语言的qsort函数，C++标准库还提供了 sort函数，可用于对字符串序列进行排序。sort 函数默认的排序方式是字典序升序，两个参数就可以了。sort函数原型如下：

基于sort函数，可以采用不同形式的comp参数，来实现按字典序降序排序：

1）lambda表达式（其中调用string类的比较运算符实现）；2）全局函数或者静态函数；3）greater模板类对象；

也可以采用 sort 的默认形式完成升序排序，然后再调用 reverse 反转实现降序排序


```C++
//原型
void sort(RandomIt first, RandomIt last);
void sort(RandomIt first, RandomIt last, Compare comp);
 
bool Cmp(const string &a, const string &b)
{
    return a > b;
}
 
sort(arr.begin(), arr.end(), [](string a, string b) { return a > b; });
// sort(arr.begin(), arr.end(), Cmp);
// sort(arr.begin(), arr.end(), greater<string>());

sort(arr.begin(), arr.end());
reverse(arr.begin(), arr.end());	

```

**Python**

1.  字符串比较：Python的大于、等于、小于等运算符可直接用于比较两个字符串（基于字典序），例如 "apple" < "banana", "9" > "10"。

2.  字符串序列排序：Python库函数 sort 可用于多个字符串的排序，其背后逻辑就是利用字符串比较运算符（字典序的），默认为字典序升序排序。降序的实现方式有：

1）  sort + reverse参数  2）  sort + 比较函数

```python
from functools import cmp_to_key
 
# 字符串序列排序-sort+reverse参数
arr = ["apple", "banana", "9", "10"]

arr.sort(reverse=True)  # 降序

def cmp(x, y):
    if x < y: return 1
    elif x == y: return 0
    else: return -1
arr = ["apple", "banana", "9", "10"]
arr.sort(key=cmp_to_key(cmp))

```

##  【alkf023】sjbc题之性能学习交流

合理的数据结构+ 二分查找+ BFS+ DFS


# 真题类


## 【alkf002】0710sjbc专业级第1题学习交流

电话号码转换

```C++
using namespace std;
 
class Solution {
public:
    unordered_map<string, string> engMap;
    unordered_map<string, string> cnMap;
 
    // 待实现函数，在此函数中填入答题代码
    string Translate(const string &input)
    {
        string result;
        InitMap();
        int len = input.size();
        int i = 0;
        int j = i + 1;
        while (i < len && j < len) {
            while (j < len && !IsUpperCase(input[j])) {
                ++j;
            }
            string temp;
            for (; i < j; ++i) {
                temp += input[i];
            }
            if (temp == "Double") {
                handleDouble(result, input, j, i);
                if (result == "ERROR") {
                    break;
                }
            } else if (engMap[temp] != "") {
                string add = engMap[temp];
                result += add;
                i = j;
                ++j;
            } else if (cnMap[temp] != "") {
                string add = cnMap[temp];
                result += add;
                i = j;
                ++j;
            } else {
                result = "ERROR";
                break;
            }
        }
        return result;
    }
 
    void InitMap()
    {
        vector<string> cn = { "Yi", "Er", "San", "Si", "Wu", "Liu", "Qi", "Ba", "Jiu", "Ling" };
        vector<string> eng = { "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Zero" };
        for (int i = 0; i < cn.size(); ++i) {
            engMap[eng[i]] = cn[i];
            cnMap[cn[i]] = eng[i];
        }
    }
 
    bool IsUpperCase(char ch)
    {
        return ch >= 'A' && ch <= 'Z';
    }
 
    void handleDouble(string& result, const string& input, int& j, int& i)
    {
        int k = j + 1;
        while (k < input.size() && !IsUpperCase(input[k])) {
            ++k;
        }
        string temp;
        for (int index = j; index < k; ++index) {
            temp += input[index];
        }
        if (engMap[temp] == "") {
            result = "ERROR";
        }
        else {
            result = result + engMap[temp] + engMap[temp];
            i = k;
            j = ++k;
        }
    }
};

```



##  【alkf003】0814sjbc工 3 /专 1 学习交流
```C++
class TenderSystem {
public:
    TenderSystem() {
    }
    const int intMax = 999999999;
    vector<tuple<int, int, int>> vec;
 
    void AddTender(int userId, int projectId, int price)
    {
        for (auto &ele : vec) {
            if (get<0>(ele) == userId && get<1>(ele) == projectId) {
                return;
            }
        }
        vec.push_back(make_tuple(userId, projectId, price));
    }
 
    int UpdateTender(int userId, int projectId, int price)
    {
        for (int i = 0; i < vec.size(); i++) {
            if (get<0>(vec[i]) == userId && get<1>(vec[i]) == projectId) {
                int priceOld = get<2>(vec[i]);
                vec.erase(vec.begin() + i);
                vec.push_back(make_tuple(userId, projectId, price));
                return priceOld;
            }
        }
        return -1;
    }
 
    int RemoveTender(int userId, int projectId)
    {
        for (int i = 0; i < vec.size(); i++) {
            if (get<0>(vec[i]) == userId && get<1>(vec[i]) == projectId) {
                int price = get<2>(vec[i]);
                vec.erase(vec.begin() + i);
                return price;
            }
        }
        return -1;
    }
 
    int QueryTender(int projectId, int price)
    {
        int minPrice = intMax;
        int userId = -1;
        for (auto &ele : vec) {
            if (get<1>(ele) == projectId && get<2>(ele) > price) {
                if (minPrice > get<2>(ele)) {
                    minPrice = get<2>(ele);
                    userId = get<0>(ele);
                }
            }
        }
        return userId;
    }
};

```





## 【alkf005】0911sjbc工 3 /专 1 学习交流

```C++
class Solution {
public:
    string UnzipString(string records)
    {
        // 字符串中没有“(”递归结束
        if (-1 == records.find("(")) {
            return records;
        }
        int nStrEnd = records.find(")");  // 第一个“)”在字符串中的下标
        int nStrStart = records.substr(0, nStrEnd).find_last_of("("); // 与第一个“)”匹配的“(”在字符串中的下标
        int nMultiStart = records.find("<"); // 第一个“<”在字符串中的下标
        int nMultiEnd = records.find(">");   // 第一个“>”在字符串中的下标
        int nMulti = atoi(records.substr(nMultiStart + 1, nMultiEnd - nMultiStart - 1).c_str()); // 需要复原的字符串重复次数
        // 复原该层的字符串
        string strUnzip = records.substr(0, nStrStart);
        for (int i = 0; i < nMulti; ++i) {
            strUnzip += records.substr(nStrStart + 1, nStrEnd - nStrStart - 1);
        }
        strUnzip += records.substr(nMultiEnd + 1);
        return UnzipString(strUnzip); // 递归次数由输入参数决定
    }
};

```



## 【alkf007】0918sjbc专业级第2题学习交流

```C++
#include <iostream>
#include <vector>
#include <set>
#include <string>
using namespace std;
 
struct Node {
    string name;
    int level;
    set<string> childSet;
    vector<Node> child;
    Node(string tName, int tLevel)
    {
        name = tName;
        level = tLevel;
    }
};
 
int GetLevelName(string &line, string &name)
{
    int level = 0;
    int i;
    for (i = 0; i < line.size(); i += 2) {
        if (line[i] == '|') {
            level++;
        } else {
            break;
        }
    }
    name = line.substr(i);
    return level;
}
 
void InsertNode(Node *root, int level, string &name)
{
    Node *p = root;
    while (p->level + 1 < level) {
        if (p->child.size() == 0) {
            break;
        }
        p = &p->child.back();
    }
    if (p->level + 1 == level) {
        if (p->childSet.find(name) == p->childSet.end()) {
            p->childSet.insert(name);
            p->child.push_back(Node(name, level));
        }
    }
}
 
int g_Space;
 
void Traverse(Node *root)
{
    if (root == NULL) {
        return;
    }
 
    for (int i = 0; i < root->child.size(); i++) {
        Traverse(&root->child[i]);
    }
 
    if (g_Space != 0) {
        cout << " ";
    }
    cout << root->name;
    g_Space = 1;
}
 
int main()
{
    int n;
    cin >> n;
 
    Node *root = NULL;
    string line;
    string name;
    int level = 0;
    while (n--) {
        cin >> line;
        level = GetLevelName(line, name);
        if (root == NULL && level != 0) {
            continue;
        } else if (root == NULL) {
            root = new Node(name, level);
            continue;
        }
 
        InsertNode(root, level, name);
    }
 
    Traverse(root);
 
    return 0;
}

```




## 【alkf008】0925sjbc“路由表最长匹配”学习交流
```C++
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <algorithm>
using namespace std;
 
class Solution {
public:
    // 待实现函数，在此函数中填入答题代码;
    string RouterSearch(const string &dstIp, const vector<string> &ipTable)
    {
        string result = "empty";
        int bufferMask = -1;
        long long desIpInt = GetIp(dstIp);
        for (int i = 0; i < ipTable.size(); i++) {
            long long tableIp = GetIp(ipTable[i]);
            int tableMask = GetMask(ipTable[i]);
            if (tableIp >> (32 - tableMask) == desIpInt >> (32 - tableMask) && tableMask > bufferMask) {
                result = ipTable[i];
                bufferMask = tableMask;
            }
        }
        return result;
    }
    long long GetIp(string s)
    {
        long long res = 0;
        for (int i = 0; i < 4; i++) {
            int index = s.find_first_of('.');
            int num = stoi(s.substr(0, index));
            s = s.substr(index + 1);
            res += num << ((3 - i) * 8);
        }
        return res;
    }
    int GetMask(string s)
    {
        int index = s.find_first_of('/');
        return stoi(s.substr(index + 1));
    }
};
 
inline string ReadLine()
{
    string line;
    getline(cin, line);
    return line;
}
 
inline vector<string> ReadLines(int size)
{
    vector<string> lines(size);
    for (int i = 0; i < size; ++i) {
        lines[i] = ReadLine();
    }
    return lines;
}
 
inline vector<string> ReadCountedLines()
{
    string numberLine = ReadLine();
    int count = stoi(numberLine);
    return ReadLines(count);
}
 
int main()
{
    string dstIp = ReadLine();
    vector<string> ipTable = ReadCountedLines();
 
    Solution solu;
    string result = solu.RouterSearch(dstIp, ipTable);
    cout << result << endl;
    return 0;
}

```
