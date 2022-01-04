
### STL API模板

#### 1. vector
```cpp
/*
向量/动态数组
    最常用的容器
*/
#pragma once

#include <vector>
#include <iostream>
#include <algorithm>
#include <functional>

using namespace std;

class Solution {
public:

    bool is_in(vector<int> v, int target) {
        auto it = v.begin();
        for (; it != v.end(); it++)
            if (*it == target)
                break;
        return it != v.end();
    }

    void test() {

        vector<int> v1;  // {}
        vector<int> v2 = { 1,2,3,4 };
        vector<int> v3(3, 10);  // {10,10,10}
        vector<int> v4(v2.begin() + 1, v2.end());  // {2,3,4}
        vector<int> v5(v4);  // {2,3,4}

        // 尾插
        v1.push_back(3);
        v1.push_back(2);
        v1.push_back(1);
        v1.pop_back();
        for (auto i : v1)
            cout << i << ' ';
        cout << endl;

        // 头插、指定位置插入
        v1.insert(v1.begin() + 1, 0);
        v1.insert(v1.begin() + 1, v2.begin(), v2.end());
        v1.insert(v1.end(), v2.begin(), v2.end());
        auto it = v1.erase(v1.begin() + 1);
        cout << *it << endl;
        for (auto i : v1)
            cout << i << ' ';
        cout << endl;

        // 删除
        vector<int> v6 = { 1,2,3,4,5 };
        v6.erase(v6.begin() + 1);  // { 1,3,4,5 }
        v6.erase(v6.begin() + 1, v6.begin() + 3);  // { 1,5 }
        v6.clear();
        for (auto i : v6)
            cout << i << ' ';
        cout << endl;

        // 查找
        it = find(v2.begin(), v2.end(), 5);
        if (it != v2.end()) {   // 找到了，必须做一次判断，以防空迭代器异常
            //
        }

        // 获取数组大小
        cout << "v4 size: " << v4.size() << endl;

        // 整个数组交换
        v1.swap(v2);
        v2.swap(v1);

        // 交换内部元素
        swap(v2[1], v2[2]);
        swap(v2[1], v2[2]);

        // 不同的遍历方法
        cout << "v3: ";
        for (auto i : v3)
            cout << i << ' ';
        cout << endl;

        cout << "v4: ";
        for (size_t i = 0; i < v4.size(); i++)
            cout << v4[i] << ' ';
        cout << endl;

        cout << "v5: ";
        for (auto it = v5.begin(); it != v5.end(); it++)
            cout << *it << ' ';
        cout << endl;

        cout << "r v5: ";   // 逆序遍历
        for (auto it = v5.rbegin(); it != v5.rend(); it++)
            cout << *it << ' ';
        cout << endl;

        // 获取第一个/最后元素
        v2.front() = 12;            // 因为返回的是引用，所以可以直接修改
        v2.back() -= v2.front();
        cout << v2.front() << ", " << v2.back() << endl;

        // 判断空
        if (v1.empty())
            cout << "v1 is empty" << endl;

        // 排序
        // 默认升序
        sort(v2.begin(), v2.end());
        // 降序
        sort(v2.begin(), v2.end(), greater<int>());

        // 自定义排序
        typedef pair<int, int> ii;
        vector<ii> vp{ { 1,1 },{ 1,2 },{ 2,2 },{ 2,3 },{ 3,3 } };
        sort(vp.begin(), vp.end(), [](const ii &l, const ii &r) {   // 按第一个数字升序，第二个降序
            return l.first != r.first ? l.first < r.first : l.second > r.second;
        });
        for (auto i : vp)
            cout << '{' << i.first << ',' << i.second << "} ";
        cout << endl;
    }
};

```

#### 2. list
```cpp
/*
链表常用操作：
    1. 前插
    2. 尾插
    3. 指定位置插入
    4. 查找
    5. 删除
    6. 移除全部某个值
链表的使用频率不高
注意：
    链表的迭代器不支持随机存取，即 `l.begin() + 1` 这种操作
*/
#pragma once

#include "../../all.h"
#include <list>

using namespace std;

class Solution {
public:
    void test() {
        list<int> l1{ 1,2,3,4,5 };

        // 前插
        l1.push_front(0);
        // 获取第一个元素
        auto top = l1.front();
        cout << top << endl;        // 0
        // 弹出第一个元素
        l1.pop_front();
        cout << l1.front() << endl; // 1

        // 尾插
        l1.push_back(6);
        // 获取最后一个元素
        auto back = l1.back();
        cout << back << endl;       // 6
        // 弹出最后一个元素
        l1.pop_back();
        cout << l1.back() << endl;  // 5

        // 删除
        auto ret = find(l1.begin(), l1.end(), 3);
        cout << *ret << endl;
        l1.erase(ret);
        ret = find(l1.begin(), l1.end(), 3);
        if (ret == l1.end())
            cout << "have erase 3" << endl;

        // 移除某个值，会全部移除，如果该值不存在，无返回值
        ret = find(l1.begin(), l1.end(), 5);
        cout << *ret << endl;
        l1.push_back(5);    // 再添加一个 5
        l1.push_back(5);    // 再添加一个 5
        cout << l1.size() << endl;  // 6
        l1.remove(5);       // 移除所有 5
        cout << l1.size() << endl;  // 3
        ret = find(l1.begin(), l1.end(), 5);
        if (ret == l1.end())
            cout << "have remove all 5" << endl;

        // 插入 - 有 4 种插入
        l1.insert(l1.begin(), 3);       // 1. 在开头插入 3
        l1.insert(l1.begin(), 5, 3);    // 2. 在开头插入 5 个 3
        l1.insert(l1.begin(), l1.begin(), l1.end());    // 3. 在指定位置插入一个范围
        l1.insert(l1.begin(), { 1,2,3 });               // 4. 在指定位置插入一个初始化列表

        // 清空
        l1.clear();
    }

    
};
```

#### 3. map
```cpp
/*
字典 map
注意：
    map 没有内置通过 value 找 key 的方法
        一种当然是迭代器遍历，
        下面还介绍了一种更高级的方法，通过 lambda 表达式查找
*/
#pragma once

#include <map>
#include <iostream>

using namespace std;

class Solution {
public:
    void test() {
        // 构造函数
        map<int, int> m1{ { 1,2 },{ 2,3 },{ 3,4 } };
        map<int, int> m2(m1.begin(), m1.end());
        map<int, int> m3(m2);

        m3[1] = 3;  // 更新
        m3[4] = 5;  // 插入，无返回值
        
        // 插入，如果存在则插入失败；注意与 [] 的区别
        pair<map<int, int>::iterator, bool> ret;
        ret = m3.insert(pair<int, int>(1, 4));
        if (ret.second == false)
            cout << "exist" << endl;

        // hint 插入（不常用）
        auto it = m3.begin();
        it = m3.insert(it, pair<int, int>(6, 7));  // 效率不是最高的
        // 这个跟效率有关，不深入
        // > 我在 stack overflow 上的提问：
        //      c++ - Does it matter that the insert hint place which is close the final place is before or after the final place? - Stack Overflow https://stackoverflow.com/questions/49653112/does-it-matter-that-the-insert-hint-place-which-is-close-the-final-place-is-befo

        // C++11 新语法，更快
        ret = m3.emplace(5, 6);  // 插入成功
        ret = m3.emplace(1, 4);  // 插入失败，key=1 存在了
        it = m3.emplace_hint(it, 8, 9);  // hint 插入

        // 删除 by key
        m3.erase(3);  

        // 查找 by key
        it = m3.find(7);  // 删除 by iterator
        if (it == m3.end())
            m3[7] = 77;

        // 查找 by value
        // 遍历方法，略
        // lambda 方法
        int v = 77;
        it = find_if(m3.begin(), m3.end(),
            [v](const std::map<int, int>::value_type item) {
            return item.second == v;
        });
        if (it != m3.end()) {
            int k = (*it).first;
            cout << k << endl; // 7
        }
        // 此外，还有函数对象的方式
        // > C++ map 根据value找key - CSDN博客 https://blog.csdn.net/flyfish1986/article/details/72833001

        for (auto& i : m3)
            cout << i.first << ": " << i.second << endl;
    }

    
};
```

#### 4. stack
```cpp
/*
栈 stack
    栈的性质是“先进后出”
    其内部是使用双端队列 deque 实现的，屏蔽了部分接口
*/
#pragma once

#include <list>
#include <vector>
#include <stack>
#include <iostream>

using namespace std;

class Solution {
public:
    void test() {
        stack<int> s1;
        s1.push(1);
        s1.push(2);
        s1.push(3);

        // pop() 没有返回值，因此如果需要使用弹出的值，需要先接收
        auto top_val = s1.top();  // 3
        s1.pop();

        // 可以使用 deque 来构造 stack
        deque<int> d1 = { 1,2,3 };
        stack<int> s2(d1);

        s2.push(4);
        top_val = s2.top();  // 4
        s2.pop();  // {1,2,3}

    }

};
```

#### 5. queue
```cpp
/*
队列：
    队列的性质是“先进先出”
    队列包括常规的队列 queue 和双端队列 deque
    queue 的内部就是 deque 实现的，
    因为双端队列包括的队列的所有功能，所以推荐使用 deque —— 它会使用 _front 和 _back 来区分头插和尾插
    因为 list 也满足 queue 的接口，所以可以使用 list 作为 queue 背后的容器
*/
#pragma once

#include <list>
#include <deque>
#include <vector>
#include <queue>
#include <iostream>
#include <memory>

using namespace std;

class Solution {
public:
    void test() {
        // 双端队列
        deque<int> d1 = { 1,2,3 };

        d1.push_front(0);  // {0,1,2,3}
        for (auto i : d1)
            cout << i << ' ';
        cout << endl;

        auto front_val = d1.front();
        d1.pop_front();  // {1,2,3}
        for (auto i : d1)
            cout << i << ' ';
        cout << endl;

        // 一般队列
        queue<int> q1(d1);
        q1.push(12);

        // 因为 list 也满足 queue 的接口，所以可以使用 list 作为 queue 背后的容器
        list<int> l = { 1,2,3,4,5 };
        queue<int, list<int>> q2(l);

        //queue<int> q3{ 1,2,3 };  // error，没有相应的构造函数
        //queue<int, vector<int>> q4;  // warning
    }
};

```
#### 6. set
```cpp
/*
集合 set
    set 和 map 基本一致，相当于没有 value 的 map，或者说的 map 的 key 就是一个 set
    因此，set 跟 map 的 key 一样是不允许重复的
    如果需要重复，可以使用 multiset
    set 的内部实现默认是红黑树——Python 默认是 HashSet
        因此 set 中的元素是默认有序的——升序
        如果需要降序的 set，可以使用仿函数
    ~~STL 也提供了 hash_set 的实现~~
    STL 已经移除了 hash_set，改用 unordered_set 实现 Hash Set（可能）
        注意：使用 unordered_set 并不会改变插入元素的顺序，这一点跟 Python 中的 set 不太一样——注意：但如果使用 Ipython，它会自动帮你有序显示
        C++:
            vector<char> vc{ 'a', 'r', 'b', 'c', 'd' };
            unordered_set<char> hs(v.begin(), v.end());
            for (auto i : hs)
                cout << i << ' ';       // a b r c d
            cout << endl;
            hs.emplace('e');
            for (auto i : hs)
                cout << i << ' ';       // a b r c d e  插入位置不变
            cout << endl;
        Python:
            >>> a = set('abracadabra')
            >>> a
            {'a', 'r', 'b', 'c', 'd'}
            >>> a.add('e')
            >>> a
            {'a', 'e', 'r', 'b', 'c', 'd'}          # 'e' 的插入位置变了，但是无序
        Ipython:
            In [1]: a = set('abracadabra')
            In [2]: a
            Out[2]: {'a', 'b', 'c', 'd', 'r'}
            In [3]: a.add('e')
            In [4]: a
            Out[4]: {'a', 'b', 'c', 'd', 'e', 'r'}  # 'e' 的插入位置变了，有序
*/
#pragma once

#include "../../all.h"
#include <set>
#include <unordered_set>  // #include <hash_set>

using namespace std;

class Solution {
public:
    struct cmp {
        bool operator () (int l, int r) {
            return l > r;
        }
    };

    void test() {
        set<int> s1;
        set<int> s2{ 3,1,2 };
        vector<int> v { 3,2,1,5,4 };
        set<int> s3(v.begin(), v.end());
        set<int, cmp> s4(v.begin(), v.end());           // 利用仿函数构造降序集
        set<int, greater<int>> s5(v.begin(), v.end());  // STL 提供的 greater 仿函数

        // 正序遍历
        for (auto i : s3)
            cout << i << ' ';
        cout << endl;

        // 逆序遍历
        for (auto it = s4.rbegin(); it != s4.rend(); it++)
            cout << *it << ' ';
        cout << endl;
        //size
        len=s2.size()
        // 增
        s2.insert(4);  // {1,2,3,4}

        // 删
        s2.erase(3);  // {1,2,4}

        // 改
        auto it = s2.find(4);
        if (it != s2.end()) { // 找到了
            //
        }
        // 查
        auto ii = s2.count(4);

        for (auto i : s2)
            cout << i << ' ';
        cout << endl;

        // 允许重复的集合
        multiset<int> ms1{ 1,2,2,2,3,4 };
        auto itp1 = ms1.find(2);  // No.2
        auto itp2 = ms1.lower_bound(2);  // No.2
        cout << (itp1 == itp2) << endl;  // True

        auto itp3 = ms1.upper_bound(2);  // No.5
        cout << *itp3 << endl;  // 3

        // Hash Set
        vector<char> vc{ 'a', 'r', 'b', 'c', 'd' };
        unordered_set<char> hs(vc.begin(), vc.end());
        for (auto i : hs)
            cout << i << ' ';   // a b r c d
        cout << endl;
        hs.emplace('e');
        for (auto i : hs)
            cout << i << ' ';   // a b r c d e
        cout << endl;
    }


};
```


#### 7. heap
```cpp
/*
堆
常用有两种构建堆的方式：
    1. 使用“优先队列”
    2. 使用`make_heap` (todo)
*/
#pragma once

#include "../../all.h"
#include <queue>
using namespace std;

class Solution {
public:
    struct node {
        int v1;
        int v2;
        //bool operator () (const node &l, const node &r) {
        //    return l.v1 != r.v1 ? l.v1 < r.v1 : l.v2 < r.v2;
        //}
    };

    // 仿函数比较器 - 降序
    struct cmp {
        bool operator () (int l, int r) {
            return l > r;
        }
    };
    // 自定义比较：先比较元素 1，再比较元素 2
    struct cmp2 {
        bool operator () (const node &l, const node &r) {
            return l.v1 != r.v1 ? l.v1 < r.v1 : l.v2 < r.v2;
        }
    };

    void test() {
        // vector 转优先队列
        //   如果是传统 ACM 的数据读取方式，可以不必这么做，在读取数据时，直接传入优先队列
        //   如果是 LeetCode 的方式，如果直接接受的是一个 vec 参数，那么可能需要用到这种方法
        vector<int> v1 = { 1,2,3 };
        //priority_queue<int> p1(v1);  // 没有直接将 vec 转 pri_que 的构造
        priority_queue<int> p1(v1.begin(), v1.end());
        cout << p1.top() << endl;  // 3

        // 添加数据
        p1.push(5);
        // 获取顶部数据
        auto top = p1.top();
        cout << top<< endl;  // 5
        // 弹出顶部数据
        p1.pop();   // 没有返回值，所以如果要使用顶部的数据，需要先接收

        // 数组转优先队列
        int arr[] = { 1,2,3 };
        priority_queue<int, vector<int>, cmp> p2(arr, arr + 3); // 使用仿函数构建最小堆，默认最大堆
        cout << p2.top() << endl;  // 1

        // 自定义结构体
        node arr2[] = { {1, 1}, {2, 2}, {2, 4}, {2, 3}, {1, 2} };
        priority_queue<node, vector<node>, cmp2> p3(arr2, arr2 + 5);
        p3.pop();  // {2, 4}
        p3.pop();  // {2, 3}
        p3.pop();  // {2, 2}
        p3.pop();  // {1, 2}
        p3.pop();  // {1, 1}
    }
};
```


## 常用代码模板3——搜索与图论

### 树与图的存储

树是一种特殊的图，与图的存储方式相同。
对于无向图中的边ab，存储两条有向边a->b, b->a。
因此我们可以只考虑有向图的存储。

(1) 邻接矩阵：g[a][b] 存储边a->b

(2) 邻接表：

```cpp
// 对于每个点k，开一个单链表，存储k所有可以走到的点。h[k]存储这个单链表的头结点
int h[N], e[N], ne[N], idx;

// 添加一条边a->b
void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

// 初始化
idx = 0;
memset(h, -1, sizeof h);
```


### 树与图的遍历 

时间复杂度 O(n+m)O(n+m), nn 表示点数，mm 表示边数

(1) 深度优先遍历 —— 模板题 AcWing 846. 树的重心

```cpp
int dfs(int u)
{
    st[u] = true; // st[u] 表示点u已经被遍历过

    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j]) dfs(j);
    }
}
```

(2) 宽度优先遍历 —— 模板题 AcWing 847. 图中点的层次

```cpp
queue<int> q;
st[1] = true; // 表示1号点已经被遍历过
q.push(1);

while (q.size())
{
    int t = q.front();
    q.pop();

    for (int i = h[t]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true; // 表示点j已经被遍历过
            q.push(j);
        }
    }
}
```

### 拓扑排序 

模板题 AcWing 848. 有向图的拓扑序列

时间复杂度 O(n+m)O(n+m), nn 表示点数，mm 表示边数

```cpp
bool topsort()
{
    int hh = 0, tt = -1;

    // d[i] 存储点i的入度
    for (int i = 1; i <= n; i ++ )
        if (!d[i])
            q[ ++ tt] = i;

    while (hh <= tt)
    {
        int t = q[hh ++ ];

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (-- d[j] == 0)
                q[ ++ tt] = j;
        }
    }

    // 如果所有点都入队了，说明存在拓扑序列；否则不存在拓扑序列。
    return tt == n - 1;
}
```

### 朴素dijkstra算法

模板题 AcWing 849. Dijkstra求最短路 I

时间复杂是 O(n2+m)O(n2+m), nn 表示点数，mm 表示边数

```cpp
int g[N][N];  // 存储每条边
int dist[N];  // 存储1号点到每个点的最短距离
bool st[N];   // 存储每个点的最短路是否已经确定

// 求1号点到n号点的最短路，如果不存在则返回-1
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    for (int i = 0; i < n - 1; i ++ )
    {
        int t = -1;     // 在还未确定最短路的点中，寻找距离最小的点
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        // 用t更新其他点的距离
        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);

        st[t] = true;
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```

### 堆优化版dijkstra

模板题 AcWing 850. Dijkstra求最短路 II

时间复杂度 O(mlogn)O(mlogn), nn 表示点数，mm 表示边数

```cpp
typedef pair<int, int> PII;

int n;      // 点的数量
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储所有点到1号点的距离
bool st[N];     // 存储每个点的最短距离是否已确定

// 求1号点到n号点的最短距离，如果不存在，则返回-1
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});      // first存储距离，second存储节点编号

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.second, distance = t.first;

        if (st[ver]) continue;
        st[ver] = true;

        for (int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > distance + w[i])
            {
                dist[j] = distance + w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```

### Bellman-Ford算法 

模板题 AcWing 853. 有边数限制的最短路

时间复杂度 O(nm)O(nm), nn 表示点数，mm 表示边数

```cpp
注意在模板题中需要对下面的模板稍作修改，加上备份数组，详情见模板题。

int n, m;       // n表示点数，m表示边数
int dist[N];        // dist[x]存储1到x的最短路距离

struct Edge     // 边，a表示出点，b表示入点，w表示边的权重
{
    int a, b, w;
}edges[M];

// 求1到n的最短路距离，如果无法从1走到n，则返回-1。
int bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    // 如果第n次迭代仍然会松弛三角不等式，就说明存在一条长度是n+1的最短路径，由抽屉原理，路径中至少存在两个相同的点，说明图中存在负权回路。
    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0; j < m; j ++ )
        {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            if (dist[b] > dist[a] + w)
                dist[b] = dist[a] + w;
        }
    }

    if (dist[n] > 0x3f3f3f3f / 2) return -1;
    return dist[n];
}
```

### spfa 算法（队列优化的Bellman-Ford算法）**

模板题 AcWing 851. spfa求最短路

时间复杂度 平均情况下 O(m)O(m)，最坏情况下 O(nm)O(nm), nn 表示点数，mm 表示边数

```cpp
int n;      // 总点数
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储每个点到1号点的最短距离
bool st[N];     // 存储每个点是否在队列中

// 求1号点到n号点的最短路距离，如果从1号点无法走到n号点则返回-1
int spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    queue<int> q;
    q.push(1);
    st[1] = true;

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                if (!st[j])     // 如果队列中已存在j，则不需要将j重复插入
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```

### spfa判断图中是否存在负环

模板题 AcWing 852. spfa判断负环

时间复杂度是 O(nm)O(nm), nn 表示点数，mm 表示边数

```cpp
int n;      // 总点数
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N], cnt[N];        // dist[x]存储1号点到x的最短距离，cnt[x]存储1到x的最短路中经过的点数
bool st[N];     // 存储每个点是否在队列中

// 如果存在负环，则返回true，否则返回false。
bool spfa()
{
    // 不需要初始化dist数组
    // 原理：如果某条最短路径上有n个点（除了自己），那么加上自己之后一共有n+1个点，由抽屉原理一定有两个点相同，所以存在环。

    queue<int> q;
    for (int i = 1; i <= n; i ++ )
    {
        q.push(i);
        st[i] = true;
    }

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true;       // 如果从1号点到x的最短路中包含至少n个点（不包括自己），则说明存在环
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }

    return false;
}
```

### floyd算法 

模板题 AcWing 854. Floyd求最短路

时间复杂度是 O(n3)O(n3), nn 表示点数

```cpp
初始化：
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            if (i == j) d[i][j] = 0;
            else d[i][j] = INF;

// 算法结束后，d[a][b]表示a到b的最短距离
void floyd()
{
    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
```

### 朴素版prim算法 

模板题 AcWing 858. Prim算法求最小生成树

时间复杂度是 O(n2+m)O(n2+m), nn 表示点数，mm 表示边数

```cpp
int n;      // n表示点数
int g[N][N];        // 邻接矩阵，存储所有边
int dist[N];        // 存储其他点到当前最小生成树的距离
bool st[N];     // 存储每个点是否已经在生成树中


// 如果图不连通，则返回INF(值是0x3f3f3f3f), 否则返回最小生成树的树边权重之和
int prim()
{
    memset(dist, 0x3f, sizeof dist);

    int res = 0;
    for (int i = 0; i < n; i ++ )
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        if (i && dist[t] == INF) return INF;

        if (i) res += dist[t];
        st[t] = true;

        for (int j = 1; j <= n; j ++ ) dist[j] = min(dist[j], g[t][j]);
    }

    return res;
}
```

### Kruskal算法

模板题 AcWing 859. Kruskal算法求最小生成树

时间复杂度是 O(mlogm)O(mlogm), nn 表示点数，mm 表示边数

```cpp
int n, m;       // n是点数，m是边数
int p[N];       // 并查集的父节点数组

struct Edge     // 存储边
{
    int a, b, w;

    bool operator< (const Edge &W)const
    {
        return w < W.w;
    }
}edges[M];

int find(int x)     // 并查集核心操作
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int kruskal()
{
    sort(edges, edges + m);

    for (int i = 1; i <= n; i ++ ) p[i] = i;    // 初始化并查集

    int res = 0, cnt = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = edges[i].a, b = edges[i].b, w = edges[i].w;

        a = find(a), b = find(b);
        if (a != b)     // 如果两个连通块不连通，则将这两个连通块合并
        {
            p[a] = b;
            res += w;
            cnt ++ ;
        }
    }

    if (cnt < n - 1) return INF;
    return res;
}
```

### 染色法判别二分图 

模板题 AcWing 860. 染色法判定二分图

时间复杂度是 O(n+m)O(n+m), nn 表示点数，mm 表示边数

```cpp
int n;      // n表示点数
int h[N], e[M], ne[M], idx;     // 邻接表存储图
int color[N];       // 表示每个点的颜色，-1表示未染色，0表示白色，1表示黑色

// 参数：u表示当前节点，c表示当前点的颜色
bool dfs(int u, int c)
{
    color[u] = c;
    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (color[j] == -1)
        {
            if (!dfs(j, !c)) return false;
        }
        else if (color[j] == c) return false;
    }

    return true;
}

bool check()
{
    memset(color, -1, sizeof color);
    bool flag = true;
    for (int i = 1; i <= n; i ++ )
        if (color[i] == -1)
            if (!dfs(i, 0))
            {
                flag = false;
                break;
            }
    return flag;
}
```

### 匈牙利算法

模板题 AcWing 861. 二分图的最大匹配

时间复杂度是 O(nm)O(nm), nn 表示点数，mm 表示边数

```cpp
int n1, n2;     // n1表示第一个集合中的点数，n2表示第二个集合中的点数
int h[N], e[M], ne[M], idx;     // 邻接表存储所有边，匈牙利算法中只会用到从第一个集合指向第二个集合的边，所以这里只用存一个方向的边
int match[N];       // 存储第二个集合中的每个点当前匹配的第一个集合中的点是哪个
bool st[N];     // 表示第二个集合中的每个点是否已经被遍历过

bool find(int x)
{
    for (int i = h[x]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true;
            if (match[j] == 0 || find(match[j]))
            {
                match[j] = x;
                return true;
            }
        }
    }

    return false;
}

// 求最大匹配数，依次枚举第一个集合中的每个点能否匹配第二个集合中的点
int res = 0;
for (int i = 1; i <= n1; i ++ )
{
    memset(st, false, sizeof st);
    if (find(i)) res ++ ;
}
```


