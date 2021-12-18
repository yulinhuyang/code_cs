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

## 【alkf009】1023sjbc“备忘录设计系统”学习交流
```C++
class MemoSystem {
public:
    MemoSystem() {}
 
    int AddEvent(int startDate, string content, int num, int period)
    {
        int count = 0;
        for (int i = startDate; i < startDate + num * period; i += period) {
            if (memoMap[i].find(content) == memoMap[i].end()) {
                memoMap[i][content] = false;
                ++count;
            }
        }
        return count;
    }
 
    bool FinishEvent(int date, string content)
    {
        if (memoMap[date].find(content) != memoMap[date].end() && memoMap[date][content] == false) {
            memoMap[date][content] = true;
            return true;
        }
        return false;
    }
 
    bool RemoveEvent(int date, string content)
    {
        if (memoMap[date].find(content) != memoMap[date].end()) {
            memoMap[date].erase(content);
            return true;
        }
        return false;
    }
 
    vector<string> QueryTodo(int startDate, int endDate)
    {
        vector<string> items;
        for (int i = startDate; i <= endDate; ++i) {
            for (auto &item : memoMap[i]) {
                if (!item.second) {
                    items.emplace_back(item.first);
                }
            }
        }
        return items;
    }
 
    unordered_map<int, map<string, bool>> memoMap;
};

```




## 【alkf010】1106sjbc“服务器组的均匀拆分”学习交流

```C++
class Solution {
public:
    // 待实现函数，在此函数中填入答题代码
    int SplitEqualTreeAndOutputNewRoot(const vector<int>& tree)
    {
        Sum.resize(tree.size());
        queue<size_t> Q;
        Q.push(0);
        vector<vector<size_t>> bi_tree(tree.size());
        for(size_t i = 1; i < tree.size(); ++i)
        {
            int q = Q.front();
            if(bi_tree[q].size() == 2)
            {
                Q.pop();
                q = Q.front();
            }
            bi_tree[q].push_back(i);
            if(tree[i] != -1)
            {
                Q.push(i);
            }
        }
        int64_t sum = GetSumForNode(bi_tree, tree, 0);
        if(sum % 2 != 0)
        {
            return -1;
        }
 
        for(size_t i = 0; i < Sum.size(); ++i)
        {
            if(tree[i] != -1 && Sum[i] * 2 == sum)
            {
                return i;
            }
        }
        return -1;
    }
    int64_t GetSumForNode(const vector<vector<size_t>>& bi_tree, const vector<int>& tree, size_t node)
    {
        if(node >= tree.size() || tree[node] == -1)
        {
            return 0;
        }
 
        int64_t sum = tree[node];
        if(bi_tree[node].size()>0)
        {
            sum += GetSumForNode(bi_tree, tree, bi_tree[node][0]);
        }
 
        if(bi_tree[node].size()>1)
        {
            sum += GetSumForNode(bi_tree, tree, bi_tree[node][1]);
        }
 
        return Sum[node] = sum;
    }
    vector<int64_t> Sum;
};


```



## 【alkf014】1204sjbc“任务规划”学习交流

```C++
class Solution {
public:
    int DivideGroup(const vector<int>& tasks, const vector<vector<int>>& mutexPairs) {
        if (tasks.size() == 0) {
            return 0;
        }
        if (mutexPairs.size() == 0) {
            return 1;
        }
        int maxV = 200000;
        vector<int> index(tasks.size(), 0);
        for (int i = 0; i < tasks.size(); i++) {
            index[tasks[i]] = i;
        }
        vector<int> M(tasks.size(), maxV);
        for (int i = 0; i < mutexPairs.size(); i++) {
            int left = index[mutexPairs[i][0]];
            int right = index[mutexPairs[i][1]];
            if (left > right) {
                swap(left, right);
            }
            if (right < M[left]) {
                M[left] = right;
            }
        }
        int res = 0;
        int min_value = maxV;
        for (int i = 0; i < tasks.size(); i++) {
            if (i == min_value) {
                min_value = M[i];
                res++;
            } else {
                min_value = min(min_value, M[i]);
            }
        }
        res++;
        return res;
    }
};

```


## 【alkf015】1211sjbc“结果对比”学习交流
```C++
	class Solution {
public:
    vector<vector<int>> CmpScores(const vector<vector<int>> &scores)
    {
        size_t rowSize = scores.size();
        size_t colSize = scores[0].size();
        vector<vector<int>> ans(rowSize, vector<int>(colSize, 0));
        for (size_t i = 0; i < rowSize; i++) {
            auto row = scores[i];
            sort(row.begin(), row.end());
            for (size_t j = 0; j < colSize; j++) {
                ans[i][j] += row.end() - upper_bound(row.begin(), row.end(), scores[i][j]);
            }
        }
        for (size_t j = 0; j < colSize; j++) {
            vector<int> col(rowSize, 0);
            for (size_t i = 0; i < rowSize; i++) {
                col[i] = scores[i][j];
            }
            sort(col.begin(), col.end());
            for (size_t i = 0; i < rowSize; i++) {
                ans[i][j] += col.end() - upper_bound(col.begin(), col.end(), scores[i][j]);
            }
        }
        return ans;
    }
};

```

## 【alkf021】0122sjbc“数字字符串插入”学习交流
```C++
struct Offset {
    int offset;
    bool isGreatest = false;
};
 
class Solution {
public:
    string InsertDigit(string score, string digit)
    {
        vector<Offset> maxPoses(digit.size());
        string digitMax;
        for (size_t i = 0; i < digit.size(); ++i) {
            string cascade = digit + digit;
            copy(digit.begin(), digit.end(), cascade.begin() + i);
            if (cascade > digitMax) {
                maxPoses[i].offset = i;
                digitMax = cascade;
            } else {
                maxPoses[i].offset = maxPoses[i - 1].offset;
            }
        }
        maxPoses[maxPoses[digit.size() - 1].offset].isGreatest = true;
 
        size_t digitIndex = 0;
        size_t insertPos;
        for (insertPos = 0; insertPos < score.size(); ++insertPos) {
            if (score[insertPos] > digit[digitIndex]) {  // 整体向前移动digit
                if (digitIndex > 0) {
                    digitIndex = 0;
                    --insertPos;
                }
                continue;
            }
            if (score[insertPos] < digit[digitIndex]) {
                insertPos -= digitIndex - maxPoses[digitIndex].offset;  // 回退插入点到最大值下标位置
                break;
            }
            if (score[insertPos] == digit[digitIndex]) {
                if (digitIndex > 0 && maxPoses[digitIndex].isGreatest) {
                    insertPos -= digitIndex - maxPoses[digitIndex].offset;  // 回退插入点到最大值下标位置
                    break;
                }
                ++digitIndex;  // 准备继续比较digit的下一个数字
                if (digitIndex >= digit.size()) {
                    digitIndex = 0;
                }
                if (insertPos == score.size() - 1) {
                    insertPos = insertPos + 1 - (digitIndex - maxPoses[digitIndex].offset);
                    break;
                }
            }
        }
        score.insert(insertPos, digit);
        return score;
    }
};



```

##  【alkf025】0305sjbc“最长空闲内存”学习交流
```C++

class Solution {
public:
    int GetMaxFreeMemoryLen(string memory, int cnt)
    {
        int left = 0;
        int right = 0;
        int fcount = 0;
        int result = 0;
        while (right < memory.size()) {
            fcount += memory[right] == 'x';
            while (fcount > cnt) {
                fcount -= memory[left] == 'x';
                left++;
            }
            result = max(result, right - left + 1);
            right++;
        }
        return result;
    }
};

```



##  【alkf028】0326sjbc“字符串编码校验”学习交流

```C++
 #include <queue>
#include <string>
#include <stdlib.h>
using namespace std;
 
class Solution {
public:
    int totalWay = 0;
    int totalCount = 0;
    queue<int> totalCounts;
 
    bool IsNumber(char c) {
        return c >= '0' && c <= '9';
    }
 
    void Check(string encodedString)
    {
        if (encodedString[0] == '0') {
            return;
        }
 
        for (int i = 0; i < encodedString.size(); i++) {
            if (!IsNumber(encodedString[i])) {
                return;
            }
 
            int count = atoi(encodedString.substr(0, i - 0 + 1).c_str());
            string subEncodedString = encodedString.substr(i + 1);
            if (count > subEncodedString.size()) {
                return;
            }
            if (count == subEncodedString.size()) {
                this->totalCounts.push(this->totalCount + count);
                this->totalWay += 1;
                return;
            }
            this->totalCount += count;
            Check(subEncodedString.substr(count));
            this->totalCount -= count;
        }
    }
 
    int GetLength(string encodedString)
    {
        Check(encodedString);
        return this->totalWay != 1 ? -1 : this->totalCounts.front();
    }
};


```



## 【alkf032】0423sjbc“门票分派”学习交流
```C++
class Solution {
public:
    int LatestTime(const vector<int> &distribute, int num, const vector<int> &arrive) {
        stack<int> state;
        for (int i = arrive.size() - 1; i >= 0; --i) {
            state.push(arrive[i]);
        }
        int lastOne = 0;
        bool hasLeft = false;
        for (int i = 0; i < distribute.size(); ++i) {
            int curNum = distribute[i];
            int count = num;
            bool leftThisRound = false;
            while (!state.empty() && (count != 0)) {
                if (state.top() <= curNum) {
                    lastOne = state.top();
                    state.pop();
                    count--;
                    continue;
                }
                leftThisRound = true;
                break;
            }
            if ((state.empty() && ((count != 0) || (i < (distribute.size() - 1)))) ||
                (leftThisRound && (i == distribute.size() - 1))) {
                hasLeft = true;
                break;
            }
        }
        if (hasLeft) {
            return distribute[distribute.size() - 1];
        }
        return (lastOne - 1);
    }
};


```



## 【alkf035】0521sjbc“租房信息查询系统”学习交流

```cpp
class RentingSystem {

public:

    RentingSystem()
    {
        this->roomsystem = vector<tuple<int, int, int, int, int>>(1001);
    }

    bool AddRoom(int id, int area, int price, int rooms, const vector<int>& address)
    {
        if(dict.count(id)){
            roomsystem[id] = {area, price, rooms, address[0], address[1]};
            return false;
        }

        else{
            roomsystem[id] = {area, price, rooms, address[0], address[1]};
            dict[id] = 1;
            return true;
        }
    }

    bool DeleteRoom(int id)
    {
        if(!dict.count(id)) return false;
        else{
            dict.erase(id);
            return true;
        }
    }

    vector<int> QueryRoom(int area, int price, int rooms, const vector<int>& address, const vector<vector<int>>& orderBy)

    {
        vector<int> ids;
        for(auto item:dict){
            int id = item.first;
            auto [ar, pr, ro, x, y] = roomsystem[id];
            if(ar>=area && pr<=price && ro==rooms){
                ids.push_back(id);  
            }
        }

        sort(ids.begin(), ids.end(), [&](int a, int b){
            auto [ara, pra, roa, xa, ya] = roomsystem[a];
            auto [arb, prb, rob, xb, yb] = roomsystem[b];
            int disa = abs(xa-address[0])+abs(ya-address[1]);
            int disb = abs(xb-address[0])+abs(yb-address[1]);
            for(auto item: orderBy){
                if(item[0]==1&&ara!=arb) return item[1]==1?ara<arb:ara>arb;
                if(item[0]==2&&pra!=prb) return item[1]==1?pra<prb:pra>prb;
                if(item[0]==3&&disa!=disb) return item[1]==1?disa<disb:disa>disb;
            }

            return a<b;
        });

        return ids;
    }

private:
    vector<tuple<int, int, int, int, int>> roomsystem;
    map<int, int> dict;

};


```

该解法没有专门定义结构体，而是用一个tuple<int, int, int, int, int>存储一套房源信息。

C++11引入的tuple是一个固定大小的不同类型值的集合，是泛化的std::pair。我们可以把他当做一个通用的结构体来用，不需要定义结构体又获得结构体的特征，在某些情况下可以使程序更简洁，直观。


排序时，直接用C++标准库的sort算法，加上lambda表达式就更简洁了。

auto [ar, pr, ro, x, y] = roomsystem[id]; 这样的代码是用了C++17的新语法“结构化绑定”。

结构化绑定所声明的变量有两种形式：

l  非引用方式，此时初始化表达式对象会拷贝一份，变量所绑定的是初始化表达式对象拷贝的各个子对象。

l  引用方式，此时初始化表达式对象不会拷贝，变量所绑定的是初始化表达式对象本身的各个子对象。

代码思路清晰，但是有比较多违反编程规范的地方；圈复杂度15也比较大，会扣3分。

注：Cmetrics会将函数内的lambda函数一起统计圈复杂度，如果代码比较长建议还是定义全局或者静态的cmp函数。



## 【alkf037】0604sjbc“地铁闸机”学习交流
```C++
class Solution {
public:
    vector<int> GetTimes(const vector<int>& arrTime, const vector<int>& direction)
    {
        if (arrTime.empty()) {
            return vector<int>();
        }
        vector<int> outTime(arrTime.size(), 0);
        int left = 0; // 出站的队列位置
        int right = 0; // 进站的队列位置
        int flag = 0; // 方向
        int timePoint = -1; // -1 作为最小时刻初值，出队的时刻
        while (left < arrTime.size() && right < arrTime.size()) {
            if (right < arrTime.size() && arrTime[right] > timePoint) {
                timePoint = arrTime[right];
                flag = 1;
            }
            while (right < arrTime.size() && arrTime[right] <= timePoint) {
                if (direction[right] == flag) {
                    outTime[right] = timePoint++;
                }
                right++;
            }
            if (left < right) {
                flag = !flag;
            }
            while (left < right) {
                if (direction[left] == flag) {
                    outTime[left] = timePoint++;
                }
                left++;
            }
        }
        return outTime;
    }
};

```


## alkf039】0618sjbc“基站信号地图”学习交流

```C++
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <algorithm>
#include <map>
using namespace std;
 
struct Command {
    string cmd;
    int row;
    int col;
};
 
class Solution {
public:
    // 待实现函数，在此函数中填入答题代码;
    vector<vector<int> > dirList = {{0, 0}, {-1, 0}, {-1, 1}, {-1, -1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
    int GetMatrixSum(int rows, int cols, const vector<pair<int, int>> &baseStations, const vector<Command> &cmds)
    {
        map<pair<int, int>, bool> basePos;
        for (auto item : baseStations) {
            basePos[make_pair(item.first - 1, item.second - 1)] = true;
        }
        for (auto item : cmds) {
            pair<int, int> key = make_pair(item.row - 1, item.col - 1);
            if (item.cmd == "delete") {
                DeleteBasePos(key, basePos, rows, cols);
                continue;
            }
            basePos[key] = true;
        }
        return CaculateMatrix(basePos, rows, cols);
    }
 
    int CaculateMatrix(map<pair<int, int>, bool>& basePos, int rows, int cols)
    {
        int result = 0;
        for (auto item : basePos) {
            for (auto dir : dirList) {
                pair<int, int> newKey = make_pair(dir[0] + item.first.first, dir[1] + item.first.second);
                bool notInMatrix = !IsValid(newKey, rows, cols) || (basePos.find(newKey) != basePos.end());
                if (notInMatrix) {
                    continue;
                }
                ++result;
            }
        }
        return result;
    }
 
    void DeleteBasePos(pair<int, int> key, map<pair<int, int>, bool>& basePos, int rows, int cols)
    {
        for (auto item : dirList) {
            pair<int, int> newKey = make_pair(item[0] + key.first, item[1] + key.second);
            if (basePos.find(newKey) == basePos.end()) {
                continue;
            }
            basePos.erase(newKey);
        }
    }
 
    bool IsValid(pair<int, int>& key, int row, int col)
    {
        return (key.first >= 0 && key.first < row) && (key.second >= 0 && key.second < col);
    }
};

```






## 【alkf044】0723sjbc“数据设备系统设计”学习交流
```C++
class DataMachineSystem {
public:
    explicit DataMachineSystem(int num)
    {
        contribute.resize(num + 1);
        nums_ = num;
    }
 
    int TransferData(int machineA, int machineB, int dataId)
    {
        if (dataIndex[machineA].find(dataId) == dataIndex[machineA].end()) { // a没有数据
            dataIndex[machineA][dataId].push_back(machineA);
        }
        if (dataIndex[machineB].find(dataId) == dataIndex[machineB].end()) { // b没有数据
            dataIndex[machineB][dataId] = dataIndex[machineA][dataId];
            for (auto it : dataIndex[machineB][dataId]) {
                contribute[it] += 10; // 10
            }
            dataIndex[machineB][dataId].push_back(machineB);
            return 1;
        }
        return 0; // b有数据
    }
 
    int TransferDataToAll(int machine, int dataId)
    {
        int res = 0;
        for (int i = 1; i <= nums_; ++i) {
            res += TransferData(machine, i, dataId);
        }
        return res;
    }
 
    int QueryContribution(int machine)
    {
        return contribute[machine];
    }
 
private:
    int nums_;
    unordered_map<int, map<int, vector<int>>> dataIndex; // 1. dataId // 2.dataSource
    vector<long> contribute;                             // 1.
};

```


## 【alkf047】0813sjbc“文件树”学习交流
```C++
*
 * Copyright (c) Huawei Technologies Co., Ltd. 2020-2021. All rights reserved.
 * Description: sjbcrz
 * Note: 缺省代码仅供参考，可自行决定使用、修改或删除
 */
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <sstream>
using namespace std;
 
struct Node {
    int level;
    map<string, Node*> child;
    explicit Node(int inLevle) : level(inLevle) {}
};
 
const int G_BLANK_NUM = 2;
 
class Solution {
public:
    void Dfs(const Node* root, vector<string>& res)
    {
        if (root->child.size() == 0) {
            return;
        }
        for (auto it : root->child) {
            string blank(root->level * G_BLANK_NUM, ' ');
            string cur = blank + it.first;
            res.push_back(cur);
            if (it.second != NULL) {
                Dfs(it.second, res);
            }
        }
    }
 
    vector<string> GetTreeFormat(const vector<string>& fileTreeList)
    {
        Node* root = new Node(0);
        for (auto file : fileTreeList) {
            istringstream inFile(file);
            string name;
            Node* cur = root;
            while (getline(inFile, name, '/')) {
                if (cur->child.find(name) == cur->child.end()) {
                    cur->child[name] = new Node(cur->level + 1);
                }
                cur = cur->child[name];
            }
        }
        vector<string> result;
        Dfs(root, result);
        return result;
    }
};
 
// 以下为考题输入输出框架，此部分代码不建议改动
inline int ReadInt()
{
    int number;
    cin >> number;
    return number;
}
 
inline string ReadLine()
{
    string line;
    getline(cin, line);
    return line;
}
 
template<typename T>
inline void WriteVector(const vector<T>& objects, char delimeter = ' ')
{
    auto it = objects.begin();
    if (it == objects.end()) {
        return;
    }
    cout << *it;
    for (++it; it != objects.end(); ++it) {
        cout << delimeter << *it;
    }
    cout << endl;
}
 
int main()
{
    int num = ReadInt();
    cin.ignore();
    vector<string> fileTreeList;
    for (int i = 0; i < num; ++i) {
        string oneLine = ReadLine();
        fileTreeList.push_back(oneLine);
    }
    Solution solu;
    vector<string> result = solu.GetTreeFormat(fileTreeList);
    WriteVector(result, '\n');
    return 0;
}


```



## 【alkf050】0903sjbc[船票预订系统设计]学习交流
```C++
class TicketSystem {
public:
    explicit TicketSystem(const vector<int> &cabins)
    {
        int n = cabins.size();
        Cabins.resize(n);
        Waiting.resize(n);
        for (int i = 0; i < n; ++i) {
            Cabins[i] = {cabins[i], cabins[i], bitset<1000>(0)};
        }
    }
 
    bool Book(int id, int cabinId, int num)
    {
        if (Waiting[cabinId].size() != 0 || get<1>(Cabins[cabinId]) < num) {
            Waiting[cabinId].emplace_back(id, num);
            return false;
        }
        Request(id, cabinId, num);
        return true;
    }
 
    bool Cancel(int id)
    {
        if (auto it = Booked.find(id); it != Booked.end()) {
            const auto &[cabinId, num, book] = it->second;
            get<1>(Cabins[cabinId]) += num;
            get<2>(Cabins[cabinId]) ^= book;
            Waitline(cabinId);
            Booked.erase(it);
            return true;
        }
        for (int cid = 0; cid < Waiting.size(); ++cid) {
            for (auto it = Waiting[cid].begin(); it != Waiting[cid].end(); ++it) {
                if (it->first == id) {
                    Waiting[cid].erase(it);
                    Waitline(cid);
                    return true;
                }
            }
        }
        return false;
    }
 
    int Query(int id)
    {
        if (auto it = Booked.find(id); it != Booked.end()) {
            int i = 0;
            while (i < 1000 && !get<2>(it->second)[i]) {
                ++i;
            }
            return i;
        }
        return -1;
    }
 
private:
    void Request(int id, int cabinId, int num)
    {
        if (num > get<1>(Cabins[cabinId])) {
            return;
        }
        get<1>(Cabins[cabinId]) -= num;
        bitset<1000> book(0);
        for (int l = -1, r = 0; r < get<0>(Cabins[cabinId]);) {
            while (++l < get<0>(Cabins[cabinId]) && get<2>(Cabins[cabinId])[l]) {
            }
            r = l;
            while (++r < get<0>(Cabins[cabinId]) && !get<2>(Cabins[cabinId])[r]) {
            }
            if (r - l >= num) {
                for (int i = l; i < l + num; ++i) {
                    get<2>(Cabins[cabinId])[i] = true;
                    book[i] = true;
                }
                Booked[id] = {cabinId, num, std::move(book)};
                return;
            }
            l = r;
        }
 
        for (int i = 0, t = num; i < get<0>(Cabins[cabinId]) && t > 0; ++i) {
            if (!get<2>(Cabins[cabinId])[i]) {
                --t;
                get<2>(Cabins[cabinId])[i] = true;
                book[i] = true;
            }
        }
        Booked[id] = {cabinId, num, std::move(book)};
    }
 
    void Waitline(int cabinId)
    {
        auto it = Waiting[cabinId].begin();
        while (it != Waiting[cabinId].end() && it->second <= get<1>(Cabins[cabinId])) {
            Request(it->first, cabinId, it->second);
            it = Waiting[cabinId].erase(it);
        }
    }
 
    vector<tuple<int, int, bitset<1000>>> Cabins;
    vector<list<pair<int, int>>> Waiting;
    unordered_map<int, tuple<int, int, bitset<1000>>> Booked;
};

```




## 【alkf052】0917sjbc“SQL解析执行”学习交流
```C++
 #include <iostream>
#include <sstream>
#include <vector>
#include <unordered_map>
#include <string>
#include <map>
 
using namespace std;
 
// 待实现函数，在此函数中填入答题代码
struct Table {
    int colNum;
    string keys;
    map<vector<int>, vector<int>> col;
};
 
class Solution {
public:
    void Create(int tableId, int colNum, const string &keys) {
        if (myTables.count(tableId) == 0) {
            myTables[tableId] = {colNum, keys, {}};
        }
    }
 
    void Insert(int tableId, const vector<int> &rec) {
        auto &table = myTables[tableId];
        vector<int> keyIndex;
        for (auto key : table.keys) {
            keyIndex.push_back(key - 'a');
        }
        vector<int> key;
        for (auto index : keyIndex) {
            key.push_back(rec[index]);
        }
        if (table.col.count(key) == 0) {
            table.col[key] = rec;
        }
    }
 
    vector<vector<int>> Select(int tableId, const vector<string> &conditions) {
        auto table = myTables[tableId];
        vector<pair<int, int>> numConditions;
        for (auto condition : conditions) {
            int index = condition[0] - 'a';
            int value = stoi(condition.substr(2));
            numConditions.push_back({index, value});
        }
        vector<vector<int>> result;
        for (auto &column : table.col) {
            if (judge(numConditions, column.second) == true) {
                result.push_back(column.second);
            }
        }
        return result;
    }
 
    bool judge(vector<pair<int, int>> numConditions, vector<int> column) {
        for (auto &condition : numConditions) {
            if (column[condition.first] != condition.second) {
                return false;
            }
        }
        return true;
    }
 
private:
    unordered_map<int, Table> myTables;
};
 
// 以下为考题输入输出框架，此部分代码不建议改动
inline int ReadInt() {
    int number;
    cin >> number;
    return number;
}
 
inline string ReadLine() {
    string line;
    getline(cin, line);
    return line;
}
 
// 按字符串拆分input
void SplitString(const string &input, const string &sepStr, vector<string> &outArray) {
    size_t startPos = 0;
    size_t pos = input.find(sepStr, 0);
    while (pos != input.npos) {
        outArray.push_back(input.substr(startPos, pos - startPos));
        startPos = pos + sepStr.size();
        pos = input.find(sepStr, startPos);
    }
    outArray.push_back(input.substr(startPos));
}
 
template<typename T>
inline void WriteVector(const vector<T> &objects, char delimeter = ' ') {
    auto it = objects.begin();
    if (it == objects.end()) {
        return;
    }
    cout << *it;
    for (++it; it != objects.end(); ++it) {
        cout << delimeter << *it;
    }
}
 
int main() {
    int num = ReadInt();
    cin.ignore();
    Solution solu;
    auto outputQueryRes = [](const auto &result) {
        if (!result.empty()) {
            for (auto &rec : result) {
                WriteVector(rec, ' ');
                cout << endl;
            }
        } else {
            cout << "empty" << endl;
        }
    };
    for (int loop = 0; loop < num; loop++) {
        string line = ReadLine();
        size_t pos = line.find_first_of(":");
        string command = line.substr(0, pos);
        if (command == "CREATE") {
            vector<string> ary;
            SplitString(line.substr(pos + 1), ",", ary);
            solu.Create(stoi(ary[0]), stoi(ary[1]), ary[2]);
        } else if (command == "INSERT") {
            int tableId = stoi(line.substr(pos + 1, line.find_first_of(",") - pos - 1));
            vector<string> values;
            SplitString(line.substr(line.find_first_of(",") + 1), " ", values);
            vector<int> rec;
            for (auto value : values) {
                rec.push_back(stoi(value));
            }
            solu.Insert(tableId, rec);
        } else if (command == "SELECT") {
            int tableId = stoi(line.substr(pos + 1, line.find_first_of(",") - pos - 1));
            vector<string> conditions;
            SplitString(line.substr(line.find_first_of(",") + 1), " and ", conditions);
            auto result = solu.Select(tableId, conditions);
            outputQueryRes(result);
        }
    }
    return 0;
}



```



##  【alkf053】0924sjbc“二叉树最大深度”学习交流
```C++
class Solution {
public:
    void dfs(const TreeNode *root, map<int, int> &heights, int step, vector<int> &nums)
    {
        if (root == NULL)
            return;
        nums.push_back(root->val);
        heights[root->val] = max(heights[root->val], step);
        dfs(root->left, heights, step + 1, nums);
        dfs(root->right, heights, step + 1, nums);
    }
    vector<int> ValueDepth(const vector<int> &target, const TreeNode *root)
    {
        map<int, int> heights;
        vector<int> nums;
        vector<int> res;
        dfs(root, heights, 1, nums);
        sort(nums.begin(), nums.end());
        map<int, int> counts;
        auto iter = heights.rbegin();
        int curmax = 0;
 
        while (iter != heights.rend()) {
            curmax = max(curmax, iter->second);
            counts[iter->first] = curmax;
            iter++;
        }
 
        for (int i = 0; i < target.size(); i++) {
            int pos = upper_bound(nums.begin(), nums.end(), target[i]) - nums.begin();
            if (pos < nums.size()) {
                int id = nums[pos];
                res.push_back(counts[id]);
            } else {
                res.push_back(-1);
            }
        }
        return res;
    }
};

```


## 【alkf056】1022sjbc[实验室预约系统设计]学习交流

```C++
class BookingSystem {
public:
    struct Node {
        int labId;
        vector<pair<int, int>> times;
        Node(int iLabId) : labId(iLabId) {}
    };
    vector<Node> mAdd;
    map<int, vector<int>> mBook;
    BookingSystem() {}
    int findLad(int labId)
    {
        for (int i = 0; i < mAdd.size(); ++i) {
            if (mAdd[i].labId == labId) {
                return i;
            }
        }
        return -1;
    }
    bool IsCross(pair<int, int> &p, int startTime, int endTime)
    {
        return !(p.first >= endTime || p.second <= startTime);
    }
    void merge(vector<pair<int, int>> &times)
    {
        sort(times.begin(), times.end(), [](pair<int, int> &p1, pair<int, int> &p2) { return p1.first <= p2.first; });
        for (auto iter = times.begin() + 1; iter != times.end();) {
            if (iter->first == (iter - 1)->second) {
                (iter - 1)->second = iter->second;
                times.erase(iter);
            } else {
                iter++;
            }
        }
    }
    bool AddLab(int labId, int startTime, int endTime)
    {
        int pos = findLad(labId);
        if (pos == -1) {
            Node lab(labId);
            lab.times.emplace_back(startTime, endTime);
            mAdd.push_back(lab);
            return true;
        } else {
            Node &lab = mAdd[pos];
            for (auto &p : lab.times) {
                if (IsCross(p, startTime, endTime)) {
                    return false;
                }
            }
            lab.times.emplace_back(startTime, endTime);
            merge(lab.times);
            return true;
        }
    }
    bool InBooking(int labId, int fromTime, int toTime)
    {
        for (auto p : mBook) {
            if (p.second[0] == labId) {
                pair<int, int> tmp { p.second[1], p.second[2] };
                if (IsCross(tmp, fromTime, toTime)) {
                    return true;
                }
            }
        }
        return false;
    }
    int BookTime(int recordId, int fromTime, int toTime)
    {
        for (auto lab : mAdd) {
            for (auto &p : lab.times) {
                // cout << "lab:" << lab.labId << ' ' << p.first << ' ' << p.second << endl;
                // cout << "book:" << fromTime << ' ' << toTime << endl;
                if (p.first <= fromTime && p.second >= toTime && !InBooking(lab.labId, fromTime, toTime)) {
                    vector<int> book { lab.labId, fromTime, toTime };
                    mBook[recordId] = book;
                    return lab.labId;
                }
            }
        }
        return -1;
    }
 
    bool CancelBooking(int recordId)
    {
        for (auto p : mBook) {
            if (recordId == p.first) {
                mBook.erase(recordId);
                return true;
            }
        }
        return false;
    }
};

```


## 【alkf058】1105sjbc[图片表格内容识别转换]学习交流

```C++
#include <algorithm>
#include <iostream>
#include <map>
#include <regex>
#include <sstream>
#include <string>
#include <vector>

using namespace std;

void SplitString(const string& input, char sperChar, vector<string>& outArray);

struct Key {
    unsigned long m_row = 0;
    unsigned long m_col = 0;
};

bool operator<(Key const& k1, Key const& k2)
{
    if (k1.m_col != k2.m_col) {
        return k1.m_col < k2.m_col;
    }
    return k1.m_row < k2.m_row;
}

using ColKey = unsigned long;

class Solution {

public:
    // 待实现函数，在此函数中填入答题代码;
    void TransformationTable(const vector<string>& tableInfo)
    {
        Parse(tableInfo);
        TextOut();
    }

    void Parse(const vector<string>& tableInfo)
    {
        std::for_each(tableInfo.begin(), tableInfo.end(), [this](auto& info) {
            std::regex reg("line(\\w+)[ \t]+col(\\w+)[ \t]*(\\w*)");
            std::smatch smatchs;
            if (std::regex_match(info, smatchs, reg)) {
                enum NUM { ONE = 1, TWO = 2, THREE = 3 };
                auto row = std::stoul(smatchs[ONE]);
                auto col = std::stoul(smatchs[TWO]);
                auto& value = smatchs[THREE];
                SetValue(row, col, value);
                SetWidth(col, value);
                m_maxLine = m_maxLine < row ? row : m_maxLine;
                m_maxCol = m_maxCol < col ? col : m_maxCol;
            }
        });
    }

    void SetValue(unsigned long row, unsigned long col, std::string const& value)
    {
        m_values[Key { row, col }] = value;
    }

    void SetWidth(unsigned long col, std::string const& value)
    {
        auto& width = m_colWidth[col];
        if (width < value.size()) {
            width = value.size();
        }
    }
    
    void TextOut() const
    {
        PrintLine();
        for (unsigned int row = 1; row <= m_maxLine; ++row) {
            std::string line;
            for (unsigned int col = 1; col <= m_maxCol; ++col) {
                auto it = m_colWidth.find(col);
                auto len = it == m_colWidth.end() ? 0 : it->second;
                line += "|";
                auto nlen = len + 2;
                auto vit = m_values.find(Key { row, col });
                std::string value = vit == m_values.end() ? "" : vit->second;
                auto vlen = value.size();
                line += std::string((nlen - vlen) / 2, ' ');
                line += value;
                line += std::string(nlen - vlen - (nlen - vlen) / 2, ' ');
            }
            line += "|\n";
            std::cout << line;
        }
        PrintLine();
    }
    
    void PrintLine() const
    {
        std::string line;
        for (unsigned int col = 1; col <= m_maxCol; ++col) {
            line += "+";
            auto it = m_colWidth.find(col);
            auto len = it == m_colWidth.end() ? 0 : it->second;
            line += std::string(len + 2, '-');
        }
        line += "+\n";
        std::cout << line;
    }

private:
    std::map<ColKey, std::size_t> m_colWidth;
    std::map<Key, std::string> m_values;
    unsigned long m_maxLine = 0;
    unsigned long m_maxCol = 0;
};

// 以下为考题输入输出框架，此部分代码不建议改动
void SplitString(const string& input, char sperChar, vector<string>& outArray)
{
    stringstream sstr(input);
    string token;
    while (getline(sstr, token, sperChar)) {
        outArray.push_back(token);
    }
}

```


## 【alkf060】1119sjbc[令牌桶限速系统]学习交流

```cpp
class RateLimitSystem {

public:

    explicit RateLimitSystem(int tokenLimit) : m_tokenLimt(tokenLimit), m_token(0), m_time(0) {}

    bool AddRule(int ruleId, int time, int interval, int number)
    {
        if (m_rules.find(ruleId) != m_rules.end()) {
            return false;
        }
        Rule rl { time, interval, number };
        m_rules.emplace(make_pair(ruleId, rl));
        m_token += number;
        m_token = min(m_token, m_tokenLimt);
        return true;
    }
    
    bool RemoveRule(int ruleId, int time)
    {
        auto iter = m_rules.find(ruleId);
        if (iter == m_rules.end()) {
            return false;
        }
        m_token += (time - iter->second.time) / iter->second.interval * iter->second.number;
        m_token = min(m_token, m_tokenLimt);
        m_rules.erase(iter);
        return true;
    }

    bool TransferData(int time, int size)
    {
        QueryToken(time);
        if (m_token >= size) {
            m_token -= size;
            return true;
        }
        return false;
    }

    int QueryToken(int time)
    {
        for (auto iter = m_rules.begin(); iter != m_rules.end(); ++iter) {
            int cs = (time - iter->second.time) / iter->second.interval;
            iter->second.time += cs * iter->second.interval;
            m_token += cs * iter->second.number;
        }

        m_token = min(m_token, m_tokenLimt);
        return m_token;
    }

private:
    int m_tokenLimt;
    int m_token;
    int m_time;
    map<int, Rule> m_rules;
};

```
