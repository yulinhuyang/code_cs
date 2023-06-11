

### 第1章 基础数据结构

#### 1.1 链表


**1.1.1 动态链表**


```cpp
#include <bits/stdc++.h>
    struct node
{               // 定义链表结点
    int data;   // 结点的值
    node *next; // 单向链表，只有一个next指针
};
int main()
{
    int n, m;
    scanf("%d %d", &n, &m);
    node *head, *p, *now, *prev; // 定义变量
    head = new node;
    head->data = 1;
    head->next = NULL; // 分配第一个结点，数据置为1
    now = head;        // 当前指针是头
    for (int i = 2; i <= n; i++)
    {
        p = new node;
        p->data = i;
        p->next = NULL; // p是新结点
        now->next = p;  // 把申请的新结点连到前面的链表上
        now = p;        // 尾指针后移一个
    }
    now->next = head; // 尾指针指向头：循环链表建立完成
    // 以上是建立链表，下面是本题的逻辑和流程。后面4种代码，逻辑流程完全一致。
    now = head, prev = head; // 从第1个开始数
    while ((n--) > 1)
    {
        for (int i = 1; i < m; i++)
        {               // 数到m，停下
            prev = now; // 记录上一个位置，用于下面跳过第m个结点
            now = now->next;
        }
        printf("%d ", now->data); // 输出第m结点，带空格
        prev->next = now->next;   // 跳过这个结点
        delete now;               // 释放结点
        now = prev->next;         // 新的一轮
    }
    printf("%d", now->data); // 打印最后一个，后面不带空格
    delete now;              // 释放最后一个结点
    return 0;
}
```

**1.1.2 静态链表**
    
1. 用结构体数组实现单向静态链表

```cpp
// 洛谷P1996，结构体数组实现单向静态链表
#include <bits/stdc++.h>
    const int N = 105; // 定义静态链表的空间大小
struct node
{                   // 单向链表
    int id, nextid; // 单向指针
    // int data;                            //如有必要，定义一个有意义的数据
} nodes[N]; // 定义在全局的静态分配
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    nodes[0].nextid = 1;
    for (int i = 1; i <= n; i++)
    {
        nodes[i].id = i;
        nodes[i].nextid = i + 1;
    }
    nodes[n].nextid = 1;   // 循环链表：尾指向头
    int now = 1, prev = 1; // 从第1个开始
    while ((n--) > 1)
    {
        for (int i = 1; i < m; i++)
        {
            prev = now;
            now = nodes[now].nextid;
        }                                       // 数到m停下
        printf("%d ", nodes[now].id);           // 带空格打印
        nodes[prev].nextid = nodes[now].nextid; // 跳过结点now，即删除now
        now = nodes[prev].nextid;               // 新的now
    }
    printf("%d", nodes[now].nextid); // 打印最后一个，后面不带空格
    return 0;
}
```

2. 用结构体数组实现双向静态链表

```cpp
// 洛谷P1996，结构体数组实现双向静态链表
#include <bits/stdc++.h>
    const int N = 105;
struct node
{           // 双向链表
    int id; // 结点编号
    // int data;                         //如有必要，定义一个有意义的数据
    int preid, nextid; // 前一个结点，后一个结点
} nodes[N];
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    nodes[0].nextid = 1;
    for (int i = 1; i <= n; i++)
    { // 建立链表
        nodes[i].id = i;
        nodes[i].preid = i - 1;  // 前结点
        nodes[i].nextid = i + 1; // 后结点
    }
    nodes[n].nextid = 1; // 循环链表：尾指向头
    nodes[1].preid = n;  // 循环链表：头指向尾
    int now = 1;         // 从第1个开始
    while ((n--) > 1)
    {
        for (int i = 1; i < m; i++)
            now = nodes[now].nextid;  // 数到m，停下
        printf("%d ", nodes[now].id); // 打印，后面带空格
        int prev = nodes[now].preid, next = nodes[now].nextid;
        nodes[prev].nextid = nodes[now].nextid; // 删除now
        nodes[next].preid = nodes[now].preid;
        now = next; // 新的开始
    }
    printf("%d", nodes[now].nextid); // 打印最后一个，后面不带空格
    return 0;
}

```

3. 用一维数组实现单向静态链表

```cpp
// 洛谷P1996，一维数组实现单向静态链表
#include <bits/stdc++.h>
    int nodes[150];
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n - 1; i++)
        nodes[i] = i + 1;  // nodes[i]的值就是下一个结点
    nodes[n] = 1;          // 循环链表：尾指向头
    int now = 1, prev = 1; // 从第1个开始
    while ((n--) > 1)
    {
        for (int i = 1; i < m; i++)
        { // 数到m，停下
            prev = now;
            now = nodes[now]; // 下一个
        }
        printf("%d ", now);       // 带空格
        nodes[prev] = nodes[now]; // 跳过结点now，即删除now
        now = nodes[prev];        // 新的now
    }
    printf("%d", now); // 打印最后一个，不带空格
    return 0;
}
​``` 

**1.1.3 STL list**

​```cpp
// 洛谷P1996，STL list
#include <bits/stdc++.h>
    using namespace std;
int main()
{
    int n, m;
    cin >> n >> m;
    list<int> node;
    for (int i = 1; i <= n; i++)
        node.push_back(i); // 建立链表
    list<int>::iterator it = node.begin();
    while (node.size() > 1)
    { // list的大小由STL自己管理
        for (int i = 1; i < m; i++)
        { // 数到m
            it++;
            if (it == node.end())
                it = node.begin(); // 循环：到末尾了再回头
        }
        cout << *it << " ";
        list<int>::iterator next = ++it;
        if (next == node.end())
            next = node.begin(); // 循环链表
        node.erase(--it);        // 删除这个结点，node.size()自动减1
        it = next;
    }
    cout << *it;
    return 0;
}
​``` 

#### 1.2队列


**1.2.1 STL queue**

​```cpp
// 洛谷P1540, STL queue
#include <bits/stdc++.h>
    using namespace std;
int Hash[1003] = {0}; // 用哈希检查内存中有没有单词，hash[i]=1表示单词i在内存中
queue<int> mem;       // 用队列模拟内存
int main()
{
    int m, n;
    scanf("%d%d", &m, &n);
    int cnt = 0; // 查词典的次数
    while (n--)
    {
        int en;
        scanf("%d", &en); // 输入一个英文单词
        if (!Hash[en])
        { // 如果内存中没有这个单词
            ++cnt;
            mem.push(en); // 单词进队列，放到队列尾部
            Hash[en] = 1; // 记录内存中有这个单词
            while (mem.size() > m)
            {                          // 内存满了
                Hash[mem.front()] = 0; // 从内存中去掉单词
                mem.pop();             // 从队头去掉
            }
        }
    }
    printf("%d\n", cnt);
    return 0;
}
```

**1.2.2 手写循环队列**

```cpp
// 洛谷P1540, 手写循环队列
#include <bits/stdc++.h>
#define N 1003         // 队列大小
    int Hash[N] = {0}; // 用Hash检查内存中有没有单词
struct myqueue
{
    int data[N]; // 分配静态空间
    /* 如果动态分配，这样写： int *data;    */
    int head, rear; // 队头、队尾
    bool init()
    { // 初始化
        /*如果动态分配，这样写：
        Q.data = (int *)malloc(N * sizeof(int)) ;
        if(!Q.data) return false;        */
        head = rear = 0;
        return true;
    }
    int size() { return (rear - head + N) % N; } // 返回队列长度
    bool empty()
    { // 判断队列是否为空
        if (size() == 0)
            return true;
        else
            return false;
    }
    bool push(int e)
    { // 队尾插入新元素。新的rear指向下一个空的位置
        if ((rear + 1) % N == head)
            return false; // 队列满
        data[rear] = e;
        rear = (rear + 1) % N;
        return true;
    }
    bool pop(int &e)
    { // 删除队头元素，并返回它
        if (head == rear)
            return false; // 队列空
        e = data[head];
        head = (head + 1) % N;
        return true;
    }
    int front() { return data[head]; } // 返回队首，但是不删除
} Q;
int main()
{
    Q.init(); // 初始化队列
    int m, n;
    scanf("%d%d", &m, &n);
    int cnt = 0;
    while (n--)
    {
        int en;
        scanf("%d", &en); // 输入一个英文单词
        if (!Hash[en])
        { // 如果内存中没有这个单词
            ++cnt;
            Q.push(en); // 单词进队列，放到队列尾部
            Hash[en] = 1;
            while (Q.size() > m)
            { // 内存满了
                int tmp;
                Q.pop(tmp);    // 删除队头
                Hash[tmp] = 0; // 从内存中去掉单词
            }
        }
    }
    printf("%d\n", cnt);
    return 0;
}
```

**1.2.3 双端队列和单调队列**
    
下面是洛谷P1886的代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1000005;
int a[N];
deque<int> q; // 队列中的数据，实际上是元素在原序列中的位置
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    for (int i = 1; i <= n; i++)
    { // 输出最小值
        while (!q.empty() && a[q.back()] > a[i])
            q.pop_back(); // 去尾
        q.push_back(i);
        if (i >= m)
        { // 每个窗口输出一次
            while (!q.empty() && q.front() <= i - m)
                q.pop_front(); // 删头
            printf("%d ", a[q.front()]);
        }
    }
    printf("\n");
    while (!q.empty())
        q.pop_front(); // 清空，下面再用一次
    for (int i = 1; i <= n; i++)
    { // 输出最大值
        while (!q.empty() && a[q.back()] < a[i])
            q.pop_back(); // 去尾
        q.push_back(i);
        if (i >= m)
        {
            while (!q.empty() && q.front() <= i - m)
                q.pop_front(); // 删头
            printf("%d ", a[q.front()]);
        }
    }
    printf("\n");
    return 0;
}
​``` 

**单调队列与最大子序和问题**

​```cpp
// hdu 1003的贪心代码
#include <bits/stdc++.h>
    using namespace std;
const int INF = 0x7fffffff;
int main()
{
    int t;
    cin >> t; // 测试用例个数
    for (int i = 1; i <= t; i++)
    {
        int n;
        cin >> n;
        int maxsum = -INF;             // 最大子序和，初始化为一个极小负数
        int start = 1, end = 1, p = 1; // 起点，终点，扫描位置
        int sum = 0;                   // 子序和
        for (int j = 1; j <= n; j++)
        {
            int a;
            cin >> a;
            sum += a; // 读入一个元素，累加
            if (sum > maxsum)
            {
                maxsum = sum;
                start = p;
                end = j;
            }
            if (sum < 0)
            { // 扫到j时，若前面的最大子序和是负数，从下一个重新开始求和
                sum = 0;
                p = j + 1;
            }
        }
        printf("Case %d:\n", i);
        printf("%d %d %d\n", maxsum, start, end);
        if (i != t)
            cout << endl;
    }
    return 0;
}
​``` 

题解2：动态规划DP。定义状态dp[i]，表示以第a[i] 为结尾的最大子序和。dp[i] 的计算有两种情况：

​```cpp
// hdu 1003的DP代码
#include <bits/stdc++.h>
    using namespace std;
int dp[100005]; // dp[i]: 以第i个数为结尾的最大值
int main()
{
    int t;
    cin >> t;
    for (int k = 1; k <= t; k++)
    {
        int n;
        cin >> n;
        for (int i = 1; i <= n; i++)
            cin >> dp[i];              // 就用dp[]存数据a[]
        int start = 1, end = 1, p = 1; // 起点，终点，扫描位置
        int maxsum = dp[1];
        for (int i = 2; i <= n; i++)
        {
            if (dp[i - 1] + dp[i] >= dp[i]) // 转移方程dp[i]=max(dp[i-1]+a[i], a[i]);
                dp[i] = dp[i - 1] + dp[i];  // dp[i-1]+a[i]比a[i]大
            else
                p = i; // a[i] 更大，那么dp[i]就是a[i]
            if (dp[i] > maxsum)
            { // dp[i]是一个更大的子序和
                maxsum = dp[i];
                start = p;
                end = i; // 以p为开始, 以i为结尾
            }
        }
        printf("Case %d:\n", k);
        printf("%d %d %d\n", maxsum, start, end);
        if (k != t)
            cout << endl;
    }
}
​``` 

下面是hdu1003的代码。

​```cpp
#include <bits/stdc++.h>
    using namespace std;
deque<int> dq;
int s[100005];
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)
        scanf("%lld", &s[i]);
    for (int i = 1; i <= n; i++)
        s[i] = s[i] + s[i - 1]; // 计算前缀和
    int ans = -1e8;
    dq.push_back(0);
    for (int i = 1; i <= n; i++)
    {
        while (!dq.empty() && dq.front() < i - m)
            dq.pop_front(); // 队头超过m范围：删头
        if (dq.empty())
            ans = max(ans, s[i]);
        else
            ans = max(ans, s[i] - s[dq.front()]); // 队头就是最小的s[k]
        while (!dq.empty() && s[dq.back()] >= s[i])
            dq.pop_back(); // 队尾大于s[i]，去尾
        dq.push_back(i);
    }
    printf("%d\n", ans);
    return 0;
}
​``` 

#### 1.3栈

**1.3.1 STL stack**


​```cpp
#include <bits/stdc++.h>
    using namespace std;
int main()
{
    int n;
    scanf("%d", &n);
    getchar();
    while (n--)
    {
        stack<char> s;
        while (true)
        {
            char ch = getchar(); // 一次读入一个字符
            if (ch == ' ' || ch == '\n' || ch == EOF)
            {
                while (!s.empty())
                {
                    printf("%c", s.top());
                    s.pop();
                } // 输出并清除栈顶
                if (ch == '\n' || ch == EOF)
                    break;
                printf(" ");
            }
            else
                s.push(ch); // 入栈
        }
        printf("\n");
    }
    return 0;
}
```

**1.3.2 手写栈**

```cpp
// hdu 1062，手写栈
#include <bits/stdc++.h>
    const int N = 100100;
struct mystack
{
    char a[N];                             // 存放栈元素，字符型
    int t = 0;                             // 栈顶位置
    void push(char x) { a[++t] = x; }      // 送入栈
    char top() { return a[t]; }            // 返回栈顶元素
    void pop() { t--; }                    // 弹出栈顶
    int empty() { return t == 0 ? 1 : 0; } // 返回1表示空
} st;
int main()
{
    int n;
    scanf("%d", &n);
    getchar();
    while (n--)
    {
        while (true)
        {
            char ch = getchar(); // 一次读入一个字符
            if (ch == ' ' || ch == '\n' || ch == EOF)
            {
                while (!st.empty())
                {
                    printf("%c", st.top());
                    st.pop();
                } // 输出并清除栈顶
                if (ch == '\n' || ch == EOF)
                    break;
                printf(" ");
            }
            else
                st.push(ch); // 入栈
        }
        printf("\n");
    }
    return 0;
}
```

**1.3.3 单调栈**

（1）STL stack

```cpp
#include <bits/stdc++.h>
    using namespace std;
int h[100001], ans[100001];
int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%d", &h[i]);
    stack<int> st;
    for (int i = n; i >= 1; i--)
    {
        while (!st.empty() && h[st.top()] <= h[i])
            st.pop(); // 栈顶奶牛没我高，弹出它，直到栈顶奶牛更高为止
        if (st.empty())
            ans[i] = 0; // 栈空，没有仰望对象
        else
            ans[i] = st.top(); // 栈顶奶牛更高，是仰望对象
        st.push(i);            // 进栈
    }
    for (int i = 1; i <= n; i++)
        printf("%d\n", ans[i]);
    return 0;
}
```

（2）手写栈


```cpp
#include <bits/stdc++.h>
    using namespace std;
const int N = 100100;
struct mystack
{
    int a[N];                              // 存放栈元素，int型
    int t = 0;                             // 栈顶位置
    void push(int x) { a[++t] = x; }       // 送入栈
    int top() { return a[t]; }             // 返回栈顶元素
    void pop() { t--; }                    // 弹出栈顶
    int empty() { return t == 0 ? 1 : 0; } // 返回1表示空
} st;
int h[N], ans[N];
int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%d", &h[i]);
    for (int i = n; i >= 1; i--)
    {
        while (!st.empty() && h[st.top()] <= h[i])
            st.pop(); // 栈顶奶牛没我高，弹出它，直到栈顶奶牛更高
        if (st.empty())
            ans[i] = 0; // 栈空，没有仰望对象
        else
            ans[i] = st.top(); // 栈顶奶牛更高，是仰望对象
        st.push(i);
    }
    for (int i = 1; i <= n; i++)
        printf("%d\n", ans[i]);
    return 0;
}
```


#### 1.4二叉树和哈夫曼树


**1.4.3 哈夫曼树和哈夫曼编码**

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <queue>
using namespace std;
int main()
{
    priority_queue<int, vector<int>, greater<int>> q;
    string s;
    while (getline(cin, s) && s != "END")
    {
        sort(s.begin(), s.end());
        int num = 1; // 一种字符出现的次数
        for (int i = 1; i <= s.length(); i++)
        {
            if (s[i] != s[i - 1])
            {
                q.push(num);
                num = 1;
            }
            else
                num++;
        }
        int ans = 0;
        if (q.size() == 1) // 题目的一个坑：只有一种字符的情况
            ans = s.length();
        while (q.size() > 1)
        { // 最后一次合并不用加到ans里
            int a = q.top();
            q.pop(); // 贪心：取出频次最少的两个
            int b = q.top();
            q.pop();
            q.push(a + b); // 把两个最小的合并成新的结点，重新放进队列
            ans += a + b;  // 一种字符进几次队列，就累加几次。
                           // 进一次队列，表示它在二叉树上深了一层，编码长度加1
        }
        q.pop();
        printf("%d %d %.1f\n", s.length() * 8, ans, (double)s.length() * 8 / (double)ans);
    }
    return 0;
}

```

#### 1.5堆


**1.5.3 二叉堆的手写代码**

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e6 + 5;
int heap[N], len = 0; // len记录当前二叉树的长度
void push(int x)
{ // 上浮，插入新元素
    heap[++len] = x;
    int i = len;
    while (i > 1 && heap[i] < heap[i / 2])
    {
        swap(heap[i], heap[i / 2]);
        i = i / 2;
    }
}
void pop()
{                          // 下沉，删除堆头，调整堆
    heap[1] = heap[len--]; // 根结点替换为最后一个结点，然后结点数量减1
    int i = 1;
    while (2 * i <= len)
    {                    // 至少有左儿子
        int son = 2 * i; // 左儿子
        if (son < len && heap[son + 1] < heap[son])
            son++; // son<len表示有右儿子，选儿子中较小的
        if (heap[son] < heap[i])
        { // 与小的儿子交换
            swap(heap[son], heap[i]);
            i = son; // 下沉到儿子处
        }
        else
            break; // 如果不比儿子小，就停止下沉
    }
}
int main()
{
    int n;
    scanf("%d", &n);
    while (n--)
    {
        int op;
        scanf("%d", &op);
        if (op == 1)
        {
            int x;
            scanf("%d", &x);
            push(x);
        } // 加入堆
        else if (op == 2)
            printf("%d\n", heap[1]); // 打印堆头
        else
            pop(); // 删除堆头
    }
    return 0;
}

```

**1.5.4 堆和priority_queue**

```cpp
#include <bits/stdc++.h>
using namespace std;
priority_queue<int, vector<int>, greater<int>> q; // 定义堆
int main()
{
    int n;
    scanf("%d", &n);
    while (n--)
    {
        int op;
        scanf("%d", &op);
        if (op == 1)
        {
            int x;
            scanf("%d", &x);
            q.push(x);
        }
        else if (op == 2)
            printf("%d\n", q.top());
        else
            q.pop();
    }
    return 0;
}

```


### 第2章基本算法

#### 2.2尺取法

**2.2.2 反向扫描**

```cpp
#include <bits/stdc++.h>     //hdu 2029
using namespace std;
int main(){
    int n;      cin >> n;             //n是测试用例个数
    while(n--){
        string s;  cin >> s;          //读一个字符串
        bool ans = true;
        int i = 0, j = s.size() - 1;  //双指针
        while(i < j){ 
            if(s[i] != s[j]) { ans = false;  break; }
            i++;   j--;               //移动双指针
        }
        if(ans)   cout << "yes" << endl;
        else      cout << "no"  << endl;
    }
    return 0;
}
```


**2.2.3 同向扫描**	

```cpp
#include <bits/stdc++.h> //例2-4. 找相同数对
    using namespace std;
const int N = 2e5 + 5;
int a[N];
int main()
{
    int n, c;
    cin >> n >> c;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    sort(a + 1, a + 1 + n);
    long long ans = 0; // 答案数量超过int，需要用long long
    for (int i = 1, j = 1, k = 1; i <= n; i++)
    {
        while (j <= n && a[j] - a[i] < c)
            j++; // 用j、k查找数字相同的区间
        while (k <= n && a[k] - a[i] <= c)
            k++; // 区间[j,k]内所有数字相同
        if (a[j] - a[i] == c && a[k - 1] - a[i] == c && k - 1 >= 1)
            ans += k - j;
    }
    cout << ans;
    return 0;
}
```

#### 2.3二分法

**2.3.2 整数二分**

bin_search()和lower_bound()对比测试

```cpp
#include<bits/stdc++.h>
using namespace std;
#define MAX   100        //试试10000000
#define MIN  -100
int a[MAX];              //大数组a[MAX]应定义为全局
unsigned long ulrand(){                //生成一个大随机数
    return (
      (((unsigned long)rand()<<24)& 0xFF000000ul)
     |(((unsigned long)rand()<<12)& 0x00FFF000ul)
     |(((unsigned long)rand())    & 0x00000FFFul));
}
int bin_search(int *a, int n, int x){    //a[0]～a[n-1]是有序的
    int left = 0, right = n;             //不是 n-1
    while (left < right) {
        int mid = left+(right-left)/2;   //int mid = (left+ right)>>1;
        if (a[mid] >= x)  right = mid;
        else    left = mid + 1;
    }
   return left;                          //特殊情况：如果最后的a[n-1] < key，left = n
}
int main(){
    int n = MAX;
    srand(time(0));
    while(1){
        for(int i=0; i< n; i++)   //产生[MIN, MAX]内的随机数，有正有负
            a[i] = ulrand() % (MAX-MIN + 1) + MIN;
        sort(a, a + n );          //排序，a[0]~a[n-1]
        int test = ulrand() % (MAX-MIN + 1) + MIN; //产生一个随机的x
        int ans = bin_search(a,n,test);
        int pos = lower_bound(a,a+n,test)-a;  
        //比较bin_search()和lower_bound()的输出是否一致：
        if(ans == pos) cout << "!";             //正确
        else        {  cout << "wrong"; break;} //有错，退出
    }
}

```


例2-7. “通往奥格瑞玛的道路”  洛谷P1462

```cpp
#include<bits/stdc++.h>   //例2-8. 进击的奶牛 洛谷P1824
using namespace std;
int n,c,x[100005];                  //牛棚数量，牛数量，牛棚坐标
bool check(int dis){                //当牛之间距离最小为dis时，检查牛棚够不够
    int cnt=1, place=0;             //第1头牛，放在第1个牛棚
    for (int i = 1; i < n; ++i)        //检查后面每个牛棚
        if (x[i] - x[place] >= dis){   //如果距离dis的位置有牛棚
            cnt++;                  //又放了一头牛
            place = i;              //更新上一头牛的位置
        }
    if (cnt >= c) return true;      //牛棚够
    else          return false;     //牛棚不够
}
int main(){
    scanf("%d%d",&n, &c);
    for(int i=0;i<n;i++)    scanf("%d",&x[i]);
    sort(x,x+n);                    //对牛棚的坐标排序
    int left=0, right=x[n-1]-x[0];  //right=1000000也行，因为是log(n)的
    int ans = 0;
    while(left < right){
        int mid = left + (right - left)/2;     //二分
        if(check(mid)){             //当牛之间距离最小为mid时，牛棚够不够?
            ans = mid;              //牛棚够，先记录mid
            left = mid + 1;         //扩大距离
           }
        else    right = mid;         //牛棚不够，缩小距离
    }
    cout << ans;                    //打印答案
    return 0;
}

```

**2.3.3 实数二分**

例2-9. Pie  poj 3122

```cpp
#include<stdio.h>
#include<math.h>
double PI = acos(-1.0);            //3.141592653589793
#define eps 1e-5                   //试试eps多小会超时，多大会Wrong Answer
double area[10010];
int n,m;
bool check(double mid){ 
    int sum = 0;
    for(int i=0;i<n;i++)            //把每个圆蛋糕都按大小mid分开。统计总数
        sum += (int)(area[i] / mid);
    if(sum >= m) return true;       //最后看总数够不够m个
    else         return false;
}
int main(){
    int T; scanf("%d",&T);
    while(T--){
        scanf("%d%d",&n,&m);   m++;
        double maxx = 0;
        for(int i=0;i<n;i++){
            int r; scanf("%d",&r);
            area[i] = PI*r*r;
            if(maxx < area[i]) maxx = area[i]; //最大的一块蛋糕
        }
        double left = 0, right = maxx;  
        for(int i = 0; i<100; i++){     //for循环的次数，试试多小会Wrong Answer
      //while((right-left) > eps)   {   //用while循环做二分
            double mid = left+(right-left)/2;
            if(check(mid))   left  = mid;     //每人能分到mid大小的蛋糕
            else             right = mid;     //不够分到mid大小的蛋糕
        }
        printf("%.4f\n",left);                // 打印right也对，right-left < eps
    }
    return 0;
}

```

#### 2.4三分法


**2.4.2 实数三分**

例2-10. 模板三分法 洛谷P3382 

（1）三等分，mid1和mid2为[l, r]的三等分点。

```cpp
#include<bits/stdc++.h>
using namespace std;
const double eps = 1e-6;
int n;
double a[15];
double f(double x){       //计算函数值
    double s = 0;
    for(int i=n;i>=0;i--)   s = s*x + a[i];         //注意函数求值的写法        
    return s;
}
int main(){
    double L,R;   scanf("%d%lf%lf",&n,&L,&R);
    for(int i=n;i>=0;i--)   scanf("%lf",&a[i]);
    while(R-L > eps){       // for(int i = 0; i<100; i++){   //用for也行
        double k =(R-L)/3.0;
        double mid1 = L+k, mid2 = R-k;
        if(f(mid1) > f(mid2))  R = mid2;
        else   L = mid1;
    }
    printf("%.5f\n",L);
    return 0;
}

```

（2）近似三等分，mid1和mid2在[l, r]的中间点附近。

```cpp
#include<bits/stdc++.h>
using namespace std;
const double eps = 1e-6;
int n; double a[15];
double f(double x){
    double s=0;
    for(int i=n;i>=0;i--)  s = s*x + a[i];
    return s;
}
int main(){
    double L,R;
    scanf("%d%lf%lf",&n,&L,&R);
    for(int i=n;i>=0;i--) scanf("%lf",&a[i]);
    while(R-L > eps){   // for(int i = 0; i<100; i++){   //用for也行
        double mid = L+(R-L)/2; 
        if(f(mid - eps) > f(mid))   R = mid;
        else L = mid;
     }
    printf("%.5f\n",L);
    return 0;
}
```
	
**2.4.3 整数三分**

例2-13. 期末考试  洛谷 P3745 

```cpp
#include<bits/stdc++.h>
const int N = 100005;
using namespace std;
typedef long long ll;
int n,m,t[N],b[N];
ll A,B,C,ans;
ll calc1(int p){             //计算通过A，B操作把时间调到p的不愉快度
    ll x=0,y=0;
    for(int i=1;i<=m;i++){
        if(b[i]<p) x += p-b[i];
        else       y += b[i]-p;
    }
    if(A<B) return min(x,y)*A+(y-min(x,y))*B;  //A<B,先用A填补，再用B
    else return y*B;                           //B<=A,直接全部使用B
}
ll calc2(int p)  {           //计算学生们的不愉快度总和
    ll sum=0;
    for(int i=1;i<=n;i++)  
        if(t[i]<p) sum += (p-t[i])*C;
    return sum;
}
int main(){
    cin>>A>>B>>C>>n>>m;
    for(int i=1;i<=n;i++) cin >> t[i];
    for(int i=1;i<=m;i++) cin >> b[i];
    sort(b+1,b+m+1);  sort(t+1,t+n+1);
    if(C>=1e16) { cout<<calc1(t[1])<<endl; return 0;}   //一个特判
    ans=1e16;
    int l=1,r=N;      //left, right
    while(r-l > 2){   //把2改成其他数字也行，后面的for再找最小值
         int mid1 = l+(r-l)/3;  int mid2 = r-(r-l)/3;
         ll c1 = calc1(mid1) + calc2(mid1);   //总不愉快度
         ll c2 = calc1(mid2) + calc2(mid2);
         if (c1<=c2) r = mid2;
         else        l = mid1;
    }
    for(int i=l;i<=r;i++){    //在上面求出的区间内再枚举时间求出最小值
        ll x = calc1(i) + calc2(i);
        ans = min(ans,x);
    }
    cout<<ans<<endl;
    return 0;
}

```


#### 2.5倍增法与ST算法


**2.5.1 倍增**

例2-14. 国旗计划 洛谷 P4155 

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 4e5+1;
int n, m;
struct warrior{   
    int id, L, R;                 //id:战士的编号；L、R，战士的左右区间
    bool operator < (const warrior b) const{return L < b.L;}
}w[N*2];
int n2;
int go[N][20];    
void init(){                      //贪心 + 预计算倍增
     int nxt = 1;
     for(int i=1;i<=n2;i++){      //用贪心求每个区间的下一个区间
         while(nxt<=n2 && w[nxt].L<=w[i].R) 
            nxt++;                //每个区间的下一个是右端点最大的那个区间
         go[i][0] = nxt-1;        //区间i的下一个区间
     }
     for(int i=1;(1<<i)<=n;++i)   //倍增：i=1,2,4,8,... 共log(n)次
         for(int s=1;s<=n2;s++)   //每个区间后的第2^i个区间
             go[s][i] = go[go[s][i-1]][i-1];
}
int res[N];
void getans(int x){                  //从第x个战士出发
     int len=w[x].L+m, cur=x, ans=1;
     for(int i=log2(N);i>=0;i--){     //从最大的i开始找：2^i = N
         int pos = go[cur][i];
         if(pos && w[pos].R < len){
            ans += 1<<i;               //累加跳过的区
            cur = pos;                 //从新位置继续开始
         }
     }
     res[w[x].id] = ans+1;
}
int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++){
        w[i].id = i;                   //记录战士的顺序
        scanf("%d%d",&w[i].L, &w[i].R);
        if(w[i].R < w[i].L)    w[i].R += m;      //把环变成链            
    }
    sort(w+1, w+n+1);  //按左端点排序
    n2 = n;
    for(int i=1;i<=n;i++)  //拆环加倍成一条链
    {    n2++;  w[n2]=w[i];  w[n2].L=w[i].L+m;  w[n2].R=w[i].R+m;  }
    init();
    for(int i=1;i<=n;i++)  getans(i);         //逐个计算每个战士
    for(int i=1;i<=n;i++)  printf("%d ",res[i]);
    return 0;
}

```

**2.5.2 ST算法**

例2-15.  Balanced Lineup 洛谷P2880 (poj 3264)

代码：

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 50005;
int n,m;
int a[N],dp_max[N][22],dp_min[N][21];
int LOG2[N];               //自己计算以2为底的对数，向下取整
void st_init(){
     LOG2[0] = -1;
     for(int i = 1;i<=N;i++)  LOG2[i]=LOG2[i>>1]+1; //不用系统log()函数，自己算        
     for(int i=1;i<=n;i++){    //初始化区间长度为1时的值
         dp_min[i][0] = a[i];
         dp_max[i][0] = a[i];
     }
     // int p=log2(n);                          //可倍增区间的最大次方: 2^p <= n
     int p = (int)(log(double(n)) / log(2.0)); // 两者写法都行
     for (int k = 1; k <= p; k++)              // 倍增计算小区间。先算小区间，再算大区间，逐步递推
         for (int s = 1; s + (1 << k) <= n + 1; s++)
         {
            dp_max[s][k] = max(dp_max[s][k - 1], dp_max[s + (1 << (k - 1))][k - 1]);
            dp_min[s][k] = min(dp_min[s][k - 1], dp_min[s + (1 << (k - 1))][k - 1]);
         }
}
int st_query(int L, int R)
{
     //  int k = log2(R-L+1);                           //3种方法求k
     int k = (int)(log(double(R - L + 1)) / log(2.0));
     //  int k = LOG2[R-L+1];                            //自己算LOG2
     int x = max(dp_max[L][k], dp_max[R - (1 << k) + 1][k]); // 区间最大
     int y = min(dp_min[L][k], dp_min[R - (1 << k) + 1][k]); // 区间最小
     return x - y;                                           // 返回差值
}
int main()
{
     scanf("%d%d", &n, &m); // 输入
     for (int i = 1; i <= n; i++)
         scanf("%d", &a[i]);
     st_init();
     for (int i = 1; i <= m; i++)
     {
         int L, R;
         scanf("%d%d", &L, &R);
         printf("%d\n", st_query(L, R));
     }
     return 0;
}

```


#### 2.6前缀和与差分

**2.6.1 一维差分**

例2-16.  Color the ball  hdu 1556

```cpp
//hdu 1556用差分数组求解
#include<bits/stdc++.h>
using namespace std;
const int N = 100010;
int a[N],D[N];               //a是气球，D是差分数组
int main(){
    int n;
    while(~scanf("%d",&n)) { 
        memset(a,0,sizeof(a)); memset(D,0,sizeof(D));
        for(int i=1;i<=n;i++){
            int L,R; scanf("%d%d",&L,&R);
            D[L]++;    D[R+1]--;             //区间修改            
        }
        for(int i=1;i<=n;i++){               //求原数组
            a[i] = a[i-1] + D[i];            //差分。求前缀和a[]，a[i]就是气球i的值
            if(i!=n)  printf("%d ", a[i]);   //逐个打印结果
            else      printf("%d\n",a[i]);
        }          //小技巧：14行到17行，把a[]改成D[]也行
    }
    return 0;
}
```
	
例2-17. 地毯 洛谷P3397

```cpp
#include<bits/stdc++.h>
using namespace std;
int D[5000][5000];     //差分数组
//int a[5000][5000];   //原数组，不定义也行
int main(){
    int n,m;    scanf("%d%d",&n,&m);
    while(m--){
        int x1,y1,x2,y2;   scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
        D[x1][y1]   += 1;   D[x2+1][y1]   -= 1;
        D[x1][y2+1] -= 1;   D[x2+1][y2+1] += 1;    //计算差分数组
    }
    for(int i=1;i<=n;++i){//根据差分数组计算原矩阵的值（想象成求小格子的面积和）
        for(int j=1;j<=n;++j){ //把用过的D[][]看成a[][]，就不用再定义a[][]了
            //a[i][j] = D[i][j] + a[i-1][j] + a[i][j-1] - a[i-1][j-1];
            //printf("%d ",a[i][j]);       //这两行和下面两行的效果一样
            D[i][j] += D[i-1][j] + D[i][j-1] - D[i-1][j-1];
            printf("%d ",D[i][j]);
        }
        printf("\n");//换行
    }
    return 0;
}

```

**2.6.3 三维差分**

例2-18. 三体攻击 https://www.lanqiao.cn/problems/180/learning/

```cpp
#include<stdio.h>
int A,B,C,n,m;
const int N = 1000005;
int s[N];   //存储舰队生命值
int D[N];   //三维差分数组（压维）；同时也用来计算每个点的攻击值
int x2[N], y2[N], z2[N];   //存储区间修改的范围，即攻击的范围
int x1[N], y1[N], z1[N];
int d[N];                        //记录伤害，就是区间修改
int num(int x,int y,int z) {  
//小技巧：压维，把三维坐标[(x,y,z)转为一维的((x-1)*B+(y-1))*C+(z-1)+1
    if (x>A || y>B || z>C) return 0;
    return ((x-1)*B+(y-1))*C+(z-1)+1;
}
bool check(int x){    //做x次区间修改。即检查经过x次攻击后是否有战舰爆炸
    for (int i=1; i<=n; i++)  D[i]=0;  //差分数组的初值，本题是0
    for (int i=1; i<=x; i++) {     //用三维差分数组记录区间修改：有8个区间端点
        D[num(x1[i],  y1[i],  z1[i])]   += d[i];
        D[num(x2[i]+1,y1[i],  z1[i])]   -= d[i];
        D[num(x1[i],  y1[i],  z2[i]+1)] -= d[i];
        D[num(x2[i]+1,y1[i],  z2[i]+1)] += d[i];
        D[num(x1[i],  y2[i]+1,z1[i])]   -= d[i];
        D[num(x2[i]+1,y2[i]+1,z1[i])]   += d[i];
        D[num(x1[i],  y2[i]+1,z2[i]+1)] += d[i];
        D[num(x2[i]+1,y2[i]+1,z2[i]+1)] -= d[i];
    }
    //下面从x、y、z三个方向计算前缀和
    for (int i=1; i<=A; i++)
        for (int j=1; j<=B; j++)
            for (int k=1; k<C; k++)        //把x、y看成定值，累加z方向
                D[num(i,j,k+1)] += D[num(i,j,k)];
    for (int i=1; i<=A; i++)
        for (int k=1; k<=C; k++)
            for (int j=1; j<B; j++)        //把x、z看成定值，累加y方向
                D[num(i,j+1,k)] += D[num(i,j,k)];
    for (int j=1; j<=B; j++)
        for (int k=1; k<=C; k++)
            for (int i=1; i<A; i++)        //把y、z看成定值，累加x方向
                D[num(i+1,j,k)] += D[num(i,j,k)];
    for (int i=1; i<=n; i++)               //最后判断是否攻击值大于生命值
        if (D[i]>s[i])
            return true;
    return false;
}
int main() {
    scanf("%d%d%d%d", &A, &B, &C, &m);
    n = A*B*C;
    for (int i=1; i<=n; i++) scanf("%d", &s[i]);  //读生命值
    for (int i=1; i<=m; i++)                      //读每次攻击的范围，用坐标表示
        scanf("%d%d%d%d%d%d%d",&x1[i],&x2[i],&y1[i],&y2[i],&z1[i],&z2[i],&d[i]);
    int L = 1,R = m;      //经典的二分写法
    while (L<R) {         //对m进行二分，找到临界值。总共只循环了log(m)次
        int mid = (L+R)>>1;
        if (check(mid)) R = mid;
        else L = mid+1;
    }
    printf("%d\n", R);  //打印临界值
    return 0;
}
```

#### 2.7离散化

**2.7.1 离散化概念**

**2.7.2 离散化手工编码**

```cpp
#include<bits/stdc++.h>
const int N = 500010;   //自己定义一个范围
struct data{
  int val;              //元素的值
  int id;               //元素的位置
}olda[N];               //离散化之前的原始数据
int newa[N];            //离散化后的结果
bool cmp(data x,data y){ return x.val < y.val; } //用于sort()
int main(){
    int n;      scanf("%d",&n);                  //读元素个数
    for(int i=1;i<=n;i++) {
        scanf("%d",&olda[i].val);                //读元素的值
        olda[i].id = i;                          //记录元素的位置
}
std::sort(olda+1,olda+1+n,cmp);              //对元素的值排序
    for(int i=1;i<=n;i++){                       //生成 newa[]
newa[olda[i].id]=i; //这个元素原来的位置在olda[i].id，把它的值赋为i，i是离散化后的新值
	   if(olda[i].val == olda[i-1].val)          //若两个元素的原值相同，把新值赋为相同
newa[olda[i].id] = newa[olda[i-1].id];
    }
    for(int i=1;i<=n;i++)  printf("%d ",newa[i]); //打印出来看看
    return 0;
}

```

**2.7.3 用STL函数实现离散化**

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 500010;  // 自己定义一个范围
int olda[N];           // 离散化前
int newa[N];           // 离散化后
int main(){
    int n;    scanf("%d",&n);
    for(int i=1;i<=n;i++) {
        scanf("%d",&olda[i]);      //读元素的值
        newa[i] = olda[i];
	}
    sort(olda+1,olda+1+n);         //排序
    int cnt = n;
  //cnt = unique(olda+1,olda+1+n)-(olda+1);  //去重，cnt是去重后的数量
    for(int i=1;i<=cnt;i++)                  //生成 newa[]
		newa[i]=lower_bound(olda+1,olda+1+n,newa[i])-olda; 
                   //查找相等的元素的位置，这个位置就是离散化后的新值
    for(int i=1;i<=cnt;i++)   printf("%d ",newa[i]);   //打印出来看看        
    printf("\n cnt=%d",cnt);    
    return 0;
}
```

#### 2.8 排序与全排列

**2.8.1 排序函数**

```cpp
#include<bits/stdc++.h>
using namespace std;
bool my_less(int i, int j)      {return (i < j);}    //自定义小于
bool my_greater(int i, int j)  {return (i > j);}    //自定义大于
int main (){
    vector<int> a = {3,7,2,5,6,8,5,4};
    sort(a.begin(),a.begin()+4);    //对前4个排序，输出2 3 5 7 6 8 5 4
  //sort(a.begin(),a.end());        //从小到大排序， 输出2 3 4 5 5 6 7 8
  //sort(a.begin(),a.end(),less<int>());    //输出2 3 4 5 5 6 7 8
  //sort(a.begin(),a.end(),my_less); 	       //自定义排序，输出 2 3 4 5 5 6 7 8
  //sort(a.begin(),a.end(),greater<int>()); //从大到小排序，输出 8 7 6 5 5 4 3 2
  //sort(a.begin(),a.end(),my_greater);     // 输出 8 7 6 5 5 4 3 2
    for(int i=0; i<a.size(); i++)   cout<<a[i]<< " ";      //输出		  
    return 0;
}
```


例2-19. 奖学金 洛谷P1093。

```cpp
#include<bits/stdc++.h>
using namespace std;
struct stu{
    int id;      //学号
    int c,m,e;   //语、数、外
    int sum;
}st[305];
bool cmp(stu a,stu b){
    if(a.sum > b.sum)         return true;
    else if(a.sum < b.sum)    return false;
    else{                                   //a.sum == b.sum
        if(a.c > b.c)         return true;
        else if(a.c < b.c)    return false;
        else{                               //a.c == b.c
            if(a.id > b.id)   return false;
            else return true;
        }
    }
}
int main(){
    int n;    cin>>n;
    for(int i=1;i<=n;i++){
        st[i].id = i;           //学号
        cin >> st[i].c >> st[i].m >> st[i].e;
        st[i].sum = st[i].c + st[i].m + st[i].e;   //总分
    }
    sort(st+1,st+1+n,cmp);   //用cmp()排序
    for(int i=1;i<=5;i++) cout << st[i].id << ' ' << st[i].sum << endl;
    return 0;
}
```


**2.8.2 排列**
	
2．自写排列函数

```cpp
#include<bits/stdc++.h>
using namespace std;
int a[20] = {1,2,3,4,5,6,7,8,9,10,11,12,13};
bool vis[20];        //记录第i个数是否用过
int b[20];           //生成的一个全排列
void dfs(int s,int t){
    if(s == t) {                      //递归结束，产生一个全排列
        for(int i = 0; i < t; ++i)  cout << b[i] << " ";    //输出一个排列            
        cout<<endl;
        return;
    }
    for(int i=0;i<t;i++)
        if(!vis[i]){
            vis[i] = true;   
            b[s] = a[i];
            dfs(s+1,t);
            vis[i] = false;
        }
}
int main(){
    int n = 3;
    dfs(0,n);     //前n个数的全排列
    return 0;
}
```

寒假作业 （蓝桥杯2016年省赛C++A组第6题）
 
```cpp 
#include <bits/stdc++.h>
using namespace std;
int a[20] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13};
int main() {
	int ans = 0;
	do {
		if (a[0] + a[1] == a[2] && a[3] - a[4] == a[5] && a[6] * a[7] == a[8] && a[11] * a[10] == a[9])
			ans++;
	} while (next_permutation(a, a + 13));
	cout << ans << endl;
}
```


```cpp
#include<bits/stdc++.h>
using namespace std;
int a[20]={1,2,3,4,5,6,7,8,9,10,11,12,13};
bool vis[20]; 
int b[20];    
int ans=0;
void dfs(int s,int t){
    if(s==12) {
        if(b[9]*b[10] == b[11])   ans++;  
        return;
    }
    if(s==3 && b[0]+b[1]!=b[2]) return; //剪枝
    if(s==6 && b[3]-b[4]!=b[5]) return; //剪枝
    if(s==9 && b[6]*b[7]!=b[8]) return; //剪枝
    for(int i=0;i<t;i++)
        if(!vis[i]){
            vis[i]=true;
            b[s]=a[i];  //本题不用a[]，改成b[s]=i+1也行
            dfs(s+1,t);
            vis[i]=false;
        }
}
int main(){
    int n=13;
    dfs(0,n); //前n个数的全排列
    cout<<ans;
    return 0;
}

```

#### 2.9 分治法

**2.9.1 汉诺塔和快速幂**


例2-20.  汉诺塔   

https://www.lanqiao.cn/problems/1512/learning/

```cpp
#include<bits/stdc++.h>
using namespace std;
int sum = 0, m;
void hanoi(char x,char y,char z,int n){  //三个柱子x、y、z
    if(n==1) {                           //最小问题
        sum++;
        if(sum==m) cout<<"#"<<n<<": "<<x<<"->"<<z<<endl;
    }
    else {                               //分治
        hanoi(x,z,y,n-1);    //(1)先把x的n-1个小盘移到y，然后把第n个大盘移到z
        sum++;
        if(sum==m) cout<<"#"<<n<<": "<<x<<"->"<<z<<endl;
        hanoi(y,x,z,n-1);    //(2)把y的n-1个小盘移到z
    }
}
int main(){
    int n;    cin>>n>>m;
    hanoi('A','B','C',n);
    cout<<sum<<endl;
    return 0;
}
```

2. 快速幂

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;                  //注意要用long long，用int会出错
ll fastPow(ll a, ll n,ll m){           //an mod m
    if(n == 0)   return 1;             //特判 a0 = 1
    if(n == 1)   return a;
    ll temp = fastPow(a, n/2,m);       //分治
    if(n%2 == 1) return temp * temp * a % m;  //奇数个a。也可以这样写： if(n &1)        
    else    return temp * temp % m ;          //偶数个a        
}
int main(){
    ll a,n,m;   cin >> a >> n>> m;
    cout << fastPow(a,n,m);
    return 0;
}

```

**2.9.2 归并排序**


例2-21. Inversion  hdu 4911

```cpp
#include<bits/stdc++.h>
const int N = 100005;
typedef  long  long  ll;
ll a[N], b[N], cnt;
void Merge(ll l, ll mid, ll r){
     ll i=l, j = mid+1, t=0;
     while(i <= mid && j <= r){
          if(a[i] > a[j]){
               b[t++] = a[j++];
               cnt += mid-i+1;            //记录逆序对数量
          }
          else b[t++]=a[i++];
     }
   //一个子序列中的数都处理完了，另一个还没有，把剩下的直接复制过来：
    while(i <= mid)   b[t++]=a[i++];  
    while(j <= r)     b[t++]=a[j++];
    for(i=0; i<t; i++)  a[l+i] = b[i];     //把排好序的b[]复制回a[]
}
void Mergesort(ll l, ll r){
    if(l<r){
         ll  mid = (l+r)/2;                  //平分成两个子序列
         Mergesort(l, mid);
         Mergesort(mid+1, r);
         Merge(l, mid, r);                    //合并
    }
}
int main(){
    ll n,k;
    while(scanf("%lld%lld", &n, &k) != EOF){
        cnt = 0;
        for(ll i=0;i<n;i++)  scanf("%lld", &a[i]);
        Mergesort(0,n-1);                     //归并排序
        if(cnt <= k)   printf("0\n");
        else           printf("%I64d\n", cnt - k);
    }
    return 0;
}
```

**2.9.3 快速排序**

例2-22. Who 's in the Middle   Poj 2388

```cpp
#include <cstdio>
#include <algorithm>
using namespace std;
const int N = 10010;
int a[N];                   //存数据
int quicksort(int left, int right, int k){
    int mid = a[left + (right - left) / 2];
    int i = left, j = right - 1;
    while(i <= j){
        while(a[i] < mid) ++i;
        while(a[j] > mid) --j;
        if(i <= j){
            swap(a[i], a[j]);
            ++i;--j;
        }
    }
    if(left <= j && k <= j) return quicksort(left, j + 1, k);
    if(i < right && k >= i) return quicksort(i, right, k);
    return a[k];                      //返回第k大数
}
int main(){
    int n;     scanf("%d", &n);
    for(int i = 0; i < n; i++)    scanf("%d", &a[i]);
    int k = n/2;
    printf("%d\n", quicksort(0, n, k));
    return 0;
}
```

#### 2.10 贪心法与拟阵

**2.10.1 贪心法**

```cpp
#include<bits/stdc++.h>
using namespace std;
const int n = 3;
int coin[] = {1,2,5};
int main(){
    int i, money;
    int ans[n] = {0};   		     //记录每种硬币的数量
    cin >> money;   		       	//输入钱数
    for(i= n-1; i>=0; i--){          //求每种硬币的数量
        ans[i] = money/coin[i];
        money = money - ans[i]*coin[i];
    }
    for(i= n-1; i>=0; i--)
        cout << coin[i]<<": "<<ans[i]<<endl; //输出每种面值硬币的数量
    return 0;
}
```


### 第3章 搜索

#### 3.1 BFS和DFS基础

**3.1.3 BFS 的代码实现**

BFS（静态版二叉树）

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100005;
struct Node{                  //用静态数组记录二叉树
    char value;
    int lson, rson;           //左右孩子
}tree[N];                     //tree[0]不用，0表示空结点
int index = 1;                //记录结点存在tree[]的位置，从tree[1]开始用
int newNode(char val){
	tree[index].value = val;
	tree[index].lson = 0;     //0表示空，tree[0]不用
	tree[index].rson = 0;
	return index ++;
}
void Insert(int &father, int child, int l_r){    //插入孩子
	if(l_r == 0)  tree[father].lson = child;     //左孩子
	else          tree[father].rson = child;     //右孩子
}
int buildtree(){              //建一棵二叉树
    int A = newNode('A');int B = newNode('B');int C = newNode('C');
    int D = newNode('D');int E = newNode('E');int F = newNode('F');
    int G = newNode('G');int H = newNode('H');int I = newNode('I');
    Insert(E,B,0);  Insert(E,G,1);       //E的左孩子是B，右孩子是G
    Insert(B,A,0);  Insert(B,D,1);
    Insert(G,F,0);  Insert(G,I,1);
    Insert(D,C,0);  Insert(I,H,0);
    int root = E;
    return root;
}
int main(){
    int root = buildtree();
    queue <int> q;
    q.push(root);                          //从根结点开始
    while(q.size()){
        int tmp = q.front();
        cout << tree[tmp].value << " ";    //打印队头
        q.pop();                           //去掉队头
        if(tree[tmp].lson != 0) q.push(tree[tmp].lson);   //左孩子入队
        if(tree[tmp].rson != 0) q.push(tree[tmp].rson);   //右孩子入队
    }
    return 0;
}

```

BFS（指针版二叉树）

```cpp
#include <bits/stdc++.h>
using namespace std;
struct node{                         //指针二叉树
    char value;
    node *l, *r;
    node(char value='#',node *l=NULL, node *r=NULL):value(value), l(l), r(r){}
};
void remove_tree(node *root){         //释放空间
    if(root == NULL) return;
    remove_tree(root->l);
    remove_tree(root->r);
    delete root;
}
int main(){
    node  *A,*B,*C,*D,*E,*F,*G,*H,*I;             //以下建一棵二叉树
    A = new node('A'); B = new node('B'); C = new node('C');
    D = new node('D'); E = new node('E'); F = new node('F');
    G = new node('G'); H = new node('H'); I = new node('I');
    E->l = B; E->r = G;      B->l = A; B->r = D;
    G->l = F; G->r = I;      D->l = C; I->l = H;   //以上建了一棵二叉树
    queue <node> q;
    q.push(*E);
    while(q.size()){
        node *tmp;
        tmp = &(q.front());
        cout << tmp->value << " ";            //打印队头
        q.pop();                              //去掉队头
        if(tmp->l) q.push(*(tmp->l));         //左孩子入队
        if(tmp->r) q.push(*(tmp->r));         //右孩子入队
    }
    remove_tree(E);
    return 0;
}

```

**3.1.4 DFS 的常见操作和代码框架**

DFS（静态数组版二叉树）

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100005;
struct Node{
    char value;   int lson, rson;
}tree[N];                      //tree[0]不用，0表示空结点
int index = 1;                 //记录结点存在tree[]的位置，从tree[1]开始用
int newNode(char val){         //新建结点
	tree[index].value = val;
	tree[index].lson = 0;     //0表示空，tree[0]不用
	tree[index].rson = 0;
	return index ++;
}
void insert(int &father, int child, int l_r){ //插入孩子
	if(l_r == 0) tree[father].lson = child;   //左孩子
	else         tree[father].rson = child;   //右孩子
}
int dfn[N] = {0};              //dfn[i]是结点i的时间戳
int dfn_timer = 0;
void dfn_order (int father){
    if(father != 0){
        dfn[father] = ++dfn_timer;
        printf("dfn[%c]=%d; ", tree[father].value, dfn[father]);//打印时间戳
        dfn_order (tree[father].lson);
        dfn_order (tree[father].rson);
    }
}
int visit_timer = 0;
void visit_order (int father){        //打印DFS序
    if(father != 0){
        printf("visit[%c]=%d; ", tree[father].value, ++visit_timer);
                                      //打印DFS序：第1次访问结点
        visit_order (tree[father].lson);
        visit_order (tree[father].rson);
        printf("visit[%c]=%d; ", tree[father].value, ++visit_timer);
                                      //打印DFS序：第2次回溯
    }
}
int deep[N] = {0};                    //deep[i]是结点i的深度
int deep_timer = 0;
void deep_node (int father){
    if(father != 0){
        deep[father] = ++deep_timer;  //打印树的深度，第一次访问时，深度+1
        printf("deep[%c]=%d; ",tree[father].value,deep[father]);
        deep_node (tree[father].lson);
        deep_node (tree[father].rson);
        deep_timer--;                 //回溯时，深度-1
    }
}
int num[N] = {0};        //num[i]是以i为父亲的子树上的结点总数
int num_node (int father){
    if(father == 0)  return 0;
    else{
        num[father] = num_node (tree[father].lson) +
                      num_node (tree[father].rson) + 1;
        printf("num[%c]=%d; ", tree[father].value, num[father]); //打印数量
        return num[father];
    }
}
void preorder (int father){                 //求先序序列
    if(father != 0){
        cout << tree[father].value <<" ";   //先序输出
        preorder (tree[father].lson);
        preorder (tree[father].rson);
    }
}
void inorder (int father){                   //求中序序列
    if(father != 0){
        inorder (tree[father].lson);
        cout << tree[father].value <<" ";    //中序输出
        inorder (tree[father].rson);
    }
}
void postorder (int father){                 //求后序序列
    if(father != 0){
        postorder (tree[father].lson);
        postorder (tree[father].rson);
        cout << tree[father].value <<" ";    //后序输出
    }
}
int buildtree(){                             //建一棵树
    int A = newNode('A');int B = newNode('B');int C = newNode('C');  //定义结点
    int D = newNode('D');int E = newNode('E');int F = newNode('F');
    int G = newNode('G');int H = newNode('H');int I = newNode('I');
    insert(E,B,0);  insert(E,G,1);          //建树。E的左孩子是B，右孩子是G
    insert(B,A,0);  insert(B,D,1);  insert(G,F,0);  insert(G,I,1);
    insert(D,C,0);  insert(I,H,0);
    int root = E;
    return root;
}
int main(){
    int root = buildtree();
    cout <<"dfn order: ";     dfn_order(root); cout << endl;    //打印时间戳
    cout <<"visit order: "; visit_order(root); cout << endl;    //打印DFS序
    cout <<"deep order: ";    deep_node(root); cout << endl;    //打印结点深度
    cout <<"num of tree: ";    num_node(root); cout << endl;    //打印子树上的结点数
    cout <<"in order:   ";      inorder(root); cout << endl;    //打印中序序列
    cout <<"pre order:  ";     preorder(root); cout << endl;    //打印先序序列
    cout <<"post order: ";    postorder(root); cout << endl;    //打印后序序列
    return 0;
}
/*   输出是：
dfn order: dfn[E]=1; dfn[B]=2; dfn[A]=3; dfn[D]=4; dfn[C]=5; dfn[G]=6; dfn[F]=7; dfn[I]=8; dfn[H]=9;
visit order: visit[E]=1; visit[B]=2; visit[A]=3; visit[A]=4; visit[D]=5; visit[C]=6; visit[C]=7; visit[D]=8; visit[B]=9; visit[G]=10; visit[F]=11; visit[F]=12; visit[I]=13; visit[H]=14; visit[H]=15; visit[I]=16; visit[G]=17; visit[E]=18;
deep order: deep[E]=1; deep[B]=2; deep[A]=3; deep[D]=3; deep[C]=4; deep[G]=2; deep[F]=3; deep[I]=3; deep[H]=4;
num of tree: num[A]=1; num[C]=1; num[D]=2; num[B]=4; num[F]=1; num[H]=1; num[I]=2; num[G]=4; num[E]=9;
in order:   A B C D E F G H I
pre order:  E B A D C G F I H
post order: A C D B F H I G E    */

```

DFS（指针版二叉树）

```cpp
#include <bits/stdc++.h>
using namespace std;
struct node{
    char value;
    node *l, *r;
    node(char value = '#', node *l = NULL, node *r = NULL):value(value), l(l), r(r){}
};
void preorder (node *root){          //求先序序列
    if(root != NULL){
        cout << root->value <<" ";   //先序输出
        preorder (root ->l);
        preorder (root ->r);
    }
}
void inorder (node *root){           //求中序序列
    if(root != NULL){
        inorder (root ->l);
        cout << root->value <<" ";   //中序输出
        inorder (root ->r);
    }
}
void postorder (node *root){          //求后序序列
    if(root != NULL){
        postorder (root ->l);
        postorder (root ->r);
        cout << root->value <<" ";    //后序输出
    }
}
void remove_tree(node *root){         //释放空间
    if(root == NULL) return;
    remove_tree(root->l);
    remove_tree(root->r);
    delete root;
}
int main(){
    node  *A, *B,*C,*D,*E,*F,*G,*H,*I;
    A = new node('A'); B = new node('B'); C = new node('C');
    D = new node('D'); E = new node('E'); F = new node('F');
    G = new node('G'); H = new node('H'); I = new node('I');
    E->l = B; E->r = G;      B->l = A; B->r = D;
    G->l = F; G->r = I;      D->l = C;       I->l = H;
    cout <<"in order:   ";    inorder(E); cout << endl;    //打印中序序列
    cout <<"pre order:  ";   preorder(E); cout << endl;    //打印先序序列
    cout <<"post order: ";  postorder(E); cout << endl;    //打印后序序列
    remove_tree(E);
    return 0;
}

```

**3.1.6 连通性判断**

例 3-1. 全球变暖  https://www.lanqiao.cn/problems/178/learning/

1. DFS 求解连通性问题

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1010;
char mp[N][N];                //地图
int vis[N][N]={0};            //标记是否搜过
int d[4][2] = {{0,1}, {0,-1}, {1,0}, {-1,0}}; //四个方向
int flag;                     //用于标记这个岛中是否被完全淹没
void dfs(int x, int y){
    vis[x][y] = 1;            //标记这个'#'被搜过。注意为什么放在这里
    if( mp[x][y+1]=='#' && mp[x][y-1]=='#' &&
        mp[x+1][y]=='#' && mp[x-1][y]=='#'   )
        flag = 1;             //上下左右都是陆地，这是一个高地，不会淹没
    for(int i = 0; i < 4; i++){     //继续DFS周围的陆地
        int nx = x + d[i][0], ny = y + d[i][1];
        if(vis[nx][ny]==0 && mp[nx][ny]=='#')  //注意为什么要判断vis[][]
                 //继续DFS未搜过的陆地，目的是标记它们
            dfs(nx,ny);
    }
}
int main(){
    int n;    cin >> n;
    for (int i = 0; i < n; i++)   cin >> mp[i];
    int ans = 0 ;
    for(int i = 1; i <= n; i++)         //DFS所有像素点
        for(int j = 1; j <= n; j++)
            if(mp[i][j]=='#' && vis[i][j]==0){
                flag = 0;               //假设这个岛被淹
                dfs(i,j);               //找这个岛中有没有高地，如果有,置flag=1
                if(flag == 0)  ans++;   //这个岛被淹了，统计被淹没岛的数量
            }
    cout<<ans<<endl;
    return 0;
}

```

2. BFS 求解连通性问题

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1010;
char mp[N][N];
int vis[N][N];
int d[4][2] = {{0,1}, {0,-1}, {1,0}, {-1,0}}; //四个方向
int flag;
void bfs(int x, int y) {
    queue<pair<int, int>> q;
    q.push({x, y});
    vis[x][y] = 1;      //标记这个'#'被搜过
    while (q.size()) {
        pair<int, int> t = q.front();
        q.pop();
        int tx = t.first, ty = t.second;
        if( mp[tx][ty+1]=='#' && mp[tx][ty-1]=='#' &&
            mp[tx+1][ty]=='#' && mp[tx-1][ty]=='#'   )
            flag = 1; //上下左右都是陆地，不会淹没
        for (int i = 0; i < 4; i++) {    //扩展(tx,ty)的4个邻居
            int nx = tx + d[i][0], ny = ty + d[i][1];
            if(vis[nx][ny]==0 && mp[nx][ny]=='#'){  //把陆地放进队列
                vis[nx][ny] = 1;  //注意：这一句必不可少
                q.push({nx, ny});
            }
        }
    }
}
int main() {
    int n;  cin >> n;
    for (int i = 0; i < n; i++)    cin >> mp[i];
    int ans = 0;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            if (mp[i][j] == '#' && vis[i][j]==0) {
                flag = 0;
                bfs(i, j);
                if(flag == 0) ans++; //这个岛全部被淹，统计岛的数量
            }
    cout << ans << endl;
    return 0;
}
```

#### 3.2 剪枝

**3.2.1 BFS 判重**

例 3-2. 跳蚱蜢  https://www.lanqiao.cn/problems/642/learning/

```cpp
#include<bits/stdc++.h>
using namespace std;
struct node{
	node(){}
	node(string ss, int tt){s = ss, t = tt;}
	string s;
	int t;
};
//(1) map
map<string, bool> mp;
//(2) set
// set<string> visited;    //记录已经搜索过的状态
queue<node> q;
void solve(){
	while(!q.empty()){
		node now = q.front();
		q.pop();
		string s = now.s;
		int step = now.t;
		if(s == "087654321"){ cout<<step<<endl; break;}   //到目标了，输出跳跃步数
		int i;
		for(i = 0 ; i < 10 ; i++)               //找到盘子的位置i
		    if(s[i] == '0')  break;
		for(int j = i - 2 ; j <= i + 2 ; j++){  //4种跳法
		    int k = (j + 9) % 9;
		    if(k == i)	continue;               //这是当前状态，不用检查
		    string news = s;
		    char tmp = news[i];
            news[i] = news[k];
            news[k] = tmp;  //跳到一种情况
            //(1) map
			if(!mp[news]){                 //判重：这个情况没有出现过
				mp[news] = true;
				q.push(node(news, step + 1));
			}

            //(2)set
            /*
            if(visited.count(news)==0){    //判重：这个情况没有出现过
				visited.insert(news);
				q.push(node(news, step + 1));
            }
            */
		}
	}
}
int main(){
	string s = "012345678";
	q.push(node(s, 0));
    //(1) map
	mp[s] = true;
	solve();
	return 0;
}
```

**3.2.2 剪枝应用**

例 3-6. Tempter of the Bone hdu 1010

```cpp
//改写自：https://www.cnblogs.com/CSU3901130321/p/3993740.html
#include <bits/stdc++.h>
using namespace std;
char mat[8][8],visit[8][8];
int n, m, t;
int flag;           //flag=1，表示找到了答案
int a, b, c, d;     //起点S(a,b)，终点D(c,d)
int dir[4][2] = {{1,0}, {-1,0}, {0,1}, {0,-1}};        //上下左右4个方向
#define CHECK(xx,yy) (xx>=0 && xx<n && yy>=0 && yy<m)  //是否在迷宫中
void dfs(int x, int y, int time){
     if(flag)  return;                  //逐层退出DFS，有多少层DFS，就退多少次
     if(mat[x][y] == 'D'){
        if(time == t)    flag = 1;      //找到答案
        return;                         //D只能走一次，所以不管对不对，都返回
     }
   //if(time > t)     return;           //剪枝(1)：因为有剪枝(2)，(1)就多余了
     int tmp = t - time - abs(c-x) - abs(d-y);
     if(tmp < 0)     return;          //剪枝（2）
   //if(tmp & 1)     return;          //奇偶剪枝：不应该在这里做，应该在main里做
     for(int i=0; i<4; i++){          //上下左右
         int xx = x + dir[i][0], yy = y + dir[i][1];
         if(CHECK(xx,yy)  && mat[xx][yy]!='X' && !visit[xx][yy]){
            visit[xx][yy] = 1;          //地板标记为走过，不能再走
            dfs(xx, yy, time + 1);      //遍历所有的路径
            visit[xx][yy] = 0;          //递归返回，这块地板恢复为没走过
         }
     }
     return;
}
int main(){
    while(~scanf("%d%d%d",&n,&m,&t)){
        if(n==0 && m==0 && t==0)    break;
        for(int i=0;i<n;i++)
            for(int j=0;j<m;j++){
                cin>>mat[i][j];
                if(mat[i][j] == 'S') a=i,b=j;
                if(mat[i][j] == 'D') c=i,d=j;
            }
        memset(visit, 0, sizeof(visit));
        int tmp = t - abs(c-a) - abs(d-b);   //在DFS之前，做奇偶判断
        if(tmp & 1){ puts("NO"); continue; } //无解，不用DFS了
        flag = 0;
        visit[a][b] = 1;                     //标记起点已经走过
        dfs(a, b, 0);                        //搜索路径
        if(flag)  puts("YES");
        else      puts("NO");
    }
    return 0;
}
```

#### 3.4 BFS 与最短路

例 3-15. 迷宫  https://www.lanqiao.cn/problems/602/learning/

```CPP
#include<bits/stdc++.h>
using namespace std;
struct node{
    int x;
    int y;
//(1)简单方法：
    string path;  //path,记录从起点(0,0)到这个点(x,y)的完整路径
};
char mp[31][51];  //存地图
char k[4]={'D','L','R','U'};  //字典序
int dir[4][2]={{1,0},{0,-1},{0,1},{-1,0}};
int vis[30][50];  //标记。vis=1: 已经搜过，不用再搜

//(2)标准方法：
char pre[31][51];        //   用于查找前驱点。例如pre[x][y] = ‘D’，表示上一个点
                         //往下走一步到了(x,y)，那么上一个点是(x-1,y)
void print_path(int x,int y){      //打印路径：从(0,0)到(29,49)
    if(x==0 && y==0)    return;      //回溯到了起点，递归结束，返回
    if(pre[x][y]=='D')  print_path(x-1,y);   //回溯，往上 U
    if(pre[x][y]=='L')  print_path(x,  y+1); //回溯，往右 R
    if(pre[x][y]=='R')  print_path(x,  y-1);
    if(pre[x][y]=='U')  print_path(x+1,y);
    printf("%c",pre[x][y]);                  //最后打印的是终点
}
void bfs(){
    node start; start.x=0;  start.y=0;
//(1)简单方法：
    start.path="";
    vis[0][0]=1;               //标记起点被搜过
    queue<node>q;
    q.push(start);             //把第一个点放进队列，开始BFS
    while(!q.empty()){
        node now = q.front();  //取出队首
        q.pop();
        if(now.x==29 && now.y==49){ //第一次达到终点，这就是字典序最小的最短路径
			//(1)简单方法：打印完整路径
            cout << now.path << endl;
			//(2)标准方法：打印完整路径，从终点回溯到起点，打印出来是从起点到终点的正序
            print_path(29,49);
            return;
        }
        for(int i=0;i<4;i++){  //扩散邻居结点
            node next;
            next.x = now.x + dir[i][0];  next.y = now.y + dir[i][1];
            if(next.x<0||next.x>=30||next.y<0||next.y>=50)  //越界了
                continue;
            if(vis[next.x][next.y]==1 || mp[next.x][next.y]=='1')
                continue;           //vis=1:已经搜过;  mp=1:是障碍
            vis[next.x][next.y]=1;  //标记被搜过
			//(1)简单方法：记录完整路径：复制上一个点的路径，加上这一步
            next.path = now.path + k[i];
			//(2)标准方法：记录点(x,y)的前驱
            pre[next.x][next.y] = k[i];
            q.push(next);
        }
    }
}
int main(){
    for(int i=0;i<30;i++)  cin >> mp[i];  //读题目给的地图数据
    bfs();
}
```

#### 3.6 BFS 与优先队列

例 3-20. 最短路 https://www.lanqiao.cn/problems/1122/learning/

```CPP
#include<bits/stdc++.h>
using namespace std;
const long long INF = 0x3f3f3f3f3f3f3f3fLL;      //这样定义的好处是: INF <= INF+x
const int N = 3e5+2;
struct edge{
int from, to;   //边：起点，终点，权值。起点from并没有用到，e[i]的i就是from
long long w;    //边：权值
    edge(int a, int b,long long c){from=a; to=b; w=c;}
};
vector<edge>e[N];   		          //存储图
struct node{
    int id; long long n_dis;          //id：结点；n_dis：这个结点到起点的距离
    node(int b,long long c){id=b; n_dis=c;}
    bool operator < (const node & a) const
    { return n_dis > a.n_dis;}
};
int n,m;
int pre[N];                          //记录前驱结点
void print_path(int s, int t) {       //打印从s到t的最短路
    if(s==t){ printf("%d ", s); return; }     //打印起点
    print_path(s, pre[t]);            //先打印前一个点
    printf("%d ", t);                 //后打印当前点。最后打印的是终点t
}
long long  dis[N];                    //记录所有结点到起点的距离
bool done[N];                         //done[i]=true表示到结点i的最短路径已经找到
void dijkstra(){
    int s = 1;                        //起点s = 1
    for (int i=1;i<=n;i++) {dis[i]=INF; done[i]=false; }    //初始化
    dis[s]=0;                         //起点到自己的距离是0
    priority_queue <node> Q;          //优先队列，存结点信息
    Q.push(node(s, dis[s]));          //起点进队列
    while (!Q.empty())   {
        node u = Q.top();             //pop出距起点s距离最小的结点u
        Q.pop();
        if(done[u.id]) continue;      //丢弃已经找到最短路径的结点。即集合A中的结点
        done[u.id]= true;
        for (int i=0; i<e[u.id].size(); i++) {  //检查结点u的所有邻居
            edge y = e[u.id][i];       //u.id的第i个邻居是y.to
            if(done[y.to]) continue;   //丢弃已经找到最短路径的邻居结点
            if (dis[y.to] > y.w + u.n_dis) {
                dis[y.to] = y.w + u.n_dis;
                Q.push(node(y.to, dis[y.to]));    //扩展新邻居，放到优先队列中
                pre[y.to]=u.id;        //如果有需要，记录路径
            }
        }
    }
    // print_path(s,n);                //如果有需要，打印路径: 起点1，终点n
}
int main(){
    scanf("%d%d",&n,&m);
    for (int i=1;i<=n;i++)   e[i].clear();
    while (m--) {
        int u,v,w;   scanf("%d%d%lld",&u,&v,&w);
        e[u].push_back(edge(u,v,w));
     // e[v].push_back(edge(v,u,w));     //本题是单向边
    }
    dijkstra();
    for(int i=1;i<=n;i++){
        if(dis[i]>=INF)  cout<<"-1 ";
        else   printf("%lld ", dis[i]);
    }
}

```

#### 3.7 BFS 与双端队列

例 3-21. Switch the Lamp On 洛谷 P4667

```CPP
#include<bits/stdc++.h>
using namespace std;
const int dir[4][2] = {{-1,-1},{-1,1},{1,-1},{1,1}}; //4个方向的位移
const int ab[4] = {2,1,1,2};                         //4个元件期望的方向
const int cd[4][2] = {{-1,-1},{-1,0},{0,-1},{0,0}};  //4个元件编号的位移
int graph[505][505],dis[505][505];                   //dis记录结点到起点s的最短路
struct P{ int x,y,dis; }u;
int read_ch(){
    char c;
    while((c = getchar())!='/' && c != '\\') ;   //字符不是'/'和'\'
    return c=='/' ? 1 : 2;
}
int main(){
    int n, m; cin >>n >>m;
	memset(dis, 0x3f, sizeof(dis));
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j) graph[i][j] = read_ch();
    deque < P > dq;
    dq.push_back((P) {
        1, 1, 0
    });
    dis[1][1] = 0;
    while (!dq.empty()) {
        u = dq.front(), dq.pop_front(); //front()读队头,pop_front()弹出队头
        int nx, ny;
        for (int i = 0; i <= 3; ++i) { //4个方向
            nx = u.x + dir[i][0];
            ny = u.y + dir[i][1];
            int d = 0; //边权
            d = graph[u.x + cd[i][0]][u.y + cd[i][1]] != ab[i]; //若方向不相等,则d=1
            if (nx && ny && nx < n + 2 && ny < m + 2 && dis[nx][ny] > dis[u.x][u.y] + d) {
                //    如果一个结点再次进队，那么距离应该更小。实际上，
                //由于再次进队时，距离肯定更大，所以这里的作用是阻止再次入队
                dis[nx][ny] = dis[u.x][u.y] + d;
                if (d == 0) dq.push_front((P) {
                    nx, ny, dis[nx][ny]
                }); //边权=0，插到队头
                else dq.push_back((P) {
                    nx, ny, dis[nx][ny]
                }); //边权=1，插到队尾
                if (nx == n + 1 && ny == m + 1) break; //到终点退出。不退也行，队列空自动退
            }
        }
    }
    if(dis[n+1][m+1] != 0x3f3f3f3f)  cout << dis[n+1][m+1];
    else   cout <<"NO SOLUTION";         //可能无解，即s到t不通
    return 0;
}

```

**3.9 IDA**

例 3 - 24. poj 3134 - Power Calculus

```CPP
#include <stdio.h>
#include <string.h>
    const int N = 100; // 最大层次
int num[N];            // 记录一条路径上的数字，num[i]是路径上第i层的数字
int n, depth;
bool dfs(int now, int d)
{ // now:当前路径走到的数字，d：now所在的深度
    if (d > depth)
        return false; // 当前深度大于层数限制
    if (now == n)
        return true;            // 找到目标。注意：这一句不能放在上一句前面
    if (now << (depth - d) < n) // 剪枝：剩下的层数用最乐观的倍增也不能达到n
        return false;
    num[d] = now; // 记录这条路径上第d层的数字
    for (int i = 0; i <= d; i++)
    { // 遍历之前算过的数，继续下一层
        if (dfs(now + num[i], d + 1))
            return true; // 加
        else if (dfs(now - num[i], d + 1))
            return true; // 减
    }
    return false;
}
int main()
{
    while (~scanf("%d", &n) && n)
    {
        for (depth = 0;; depth++)
        { // IDDFS：每次限制最大搜索depth层
            memset(num, 0, sizeof(num));
            if (dfs(1, 0))
                break; // 从数字1开始，当前层0
        }
        printf("%d\n", depth);
    }
    return 0;
}
```

### 第4章 高级数据结构

#### 4.1 并查集

**4.1.1 并查集的基本操作**

例 4-1. How Many Tables hdu 1213

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1050;
int s[N];
void init_set(){                        //初始化
    for(int i = 1; i <= N; i++)   s[i] = i;
}
int find_set(int x){                    //查找
    return x==s[x]? x:find_set(s[x]);
}
void merge_set(int x, int y){           //合并
    x = find_set(x);   y = find_set(y);
    if(x != y)    s[x] = s[y];          //把x合并到y上，y的根成为x的根
}
int main (){
    int t, n, m, x, y;    cin >> t;     //t个测试
    while(t--){
        cin >> n >> m;
        init_set();
        for(int i = 1; i <= m; i++){
            cin >> x >> y;
            merge_set(x, y);            //合并x和y
        }
        int ans = 0;
        for(int i = 1; i <= n; i++)     //统计有多少个集
            if(s[i] == i)   ans++;
        cout << ans <<endl;
    }
    return 0;
}
```

**4.1.2 合并的优化**

```cpp 
int height[N];
void init_set()
{
    for (int i = 1; i <= N; i++)
    {
        s[i] = i;
        height[i] = 0; // 树的高度
    }
}
void merge_set(int x, int y)
{ // 优化合并操作
    x = find_set(x);
    y = find_set(y);
    if (height[x] == height[y])
    {
        height[x] = height[x] + 1; // 合并，树的高度加一
        s[y] = x;
    }
    else
    { // 把矮树并到高树上，高树的高度保持不变
        if (height[x] < height[y])
            s[x] = y;
        else
            s[y] = x;
    }
}
```

**4.1.4 带权并查集**

例 4-2. How Many Answers Are Wrong hdu 3038

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
int s[N];                         //集
int d[N];                         //权值：记录当前结点到根结点的距离
int ans;
void init_set(){                  //初始化
    for(int i = 0; i <= N; i++) { s[i] = i; d[i] = 0;  }
}
int find_set(int x){              //带权值的路径压缩
    if(x != s[x]) {
        int t = s[x];            //记录父结点
        s[x] = find_set(s[x]);   //路径压缩。递归最后返回的是根结点
        d[x] += d[t];            //权值更新为x到根结点的权值
    }
    return s[x];
}
void merge_set(int a, int b,int v){    //合并
    int roota = find_set(a), rootb = find_set(b);
    if(roota == rootb){
        if(d[a] - d[b] != v)   ans++;
    }
    else{
       s[roota] = rootb;               //合并
       d[roota] = d[b]- d[a] + v;
    }
}
int main(){
    int n,m;
    while(scanf("%d%d",&n,&m) != EOF){
        init_set();
        ans = 0;
        while(m--){
            int a,b,v;  scanf("%d%d%d",&a,&b,&v);
            a--;
            merge_set(a, b, v);
        }
        printf("%d\n",ans);
    }
    return 0;
}
```

例 4-3. 食物链 poj 1182

```cpp
#include <iostream>
#include <stdio.h>
using namespace std;
const int N = 50005;
int s[N];                         //集
int d[N];                         // 0：同类； 1：吃； 2：被吃
int ans;
void init_set(){                  //初始化
   for(int i = 0; i <= N; i++) { s[i] = i; d[i] = 0;  }
}
int find_set(int x){              //带权值的路径压缩
    if(x != s[x]) {
         int t = s[x];            //记录父结点
         s[x] = find_set(s[x]);   //路径压缩。递归最后返回的是根结点
         d[x] = (d[x] + d[t]) % 3;   //权值更新为x到根结点的权值
     }
    return s[x];
}
void merge_set(int x, int y, int relation){       //合并
 	int rootx = find_set(x);  int rooty = find_set(y);
	if (rootx == rooty){
	    if ((relation - 1) != ((d[x] - d[y] + 3) % 3))      //判断矛盾
		   ans++;
	}
	else {
	    s[rootx] = rooty;   //合并
	    d[rootx] = (d[y] - d[x] + relation  - 1) % 3;   //更新权值
	}
}
int main(){
    int n, k;
    cin >> n >> k;
    init_set();
    ans = 0;
    while (k--)
    {
        int relation, x, y;
        scanf("%d%d%d", &relation, &x, &y);
        if (x > n || y > n || (relation == 2 && x == y))
           ans++;
        else
           merge_set(x, y, relation);
    }
    cout << ans;
    return 0;
}
```


#### 4.2 树状数组

**4.2.1 树状数组概念和基本编码**

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1000;
#define lowbit(x)  ((x) & - (x))
int tree[N]={0};
void update(int x, int d) {   //单点修改：修改元素a[x],  a[x] = a[x] + d
     while(x <= N) {
        tree[x] += d;
        x += lowbit(x);
     }
}
int sum(int x) {             //查询前缀和：返回前缀和sum = a[1] + a[2] +... + a[x]
    int ans = 0;
    while(x > 0){
        ans += tree[x];
        x -= lowbit(x);
    }
    return ans;
}
//以上是树状数组相关
int a[11]={0,4,5,6,7,8,9,10,11,12,13};           //注意：a[0]不用
int main (){
    for(int i=1;i<=10;i++)  update(i,a[i]);      //初始化计算tree[]数组
    cout << "old: [5,8]="<<sum(8)-sum(4)<<endl;  //查询区间和，例如查询[5,8]=38
    update(5,100);                               //模拟一次修改： a[5] = a[5]+100
    cout << "new: [5,8]="<<sum(8)-sum(4);        //重新查询区间和[5,8]=138
    return 0;
}
```

**4.2.2 树状数组基本应用**

例 4-4. Color the ball hdu 1556

```cpp
//hdu 1556代码
//tree[N]，lowbit(x)，update()，sum()的代码在前面已给出
const int N = 100010;
int main(){
    int n;
    while(~scanf("%d",&n)) {
        memset(tree,0,sizeof(tree));          //只需要一个tree[]数组
        for(int i=1;i<=n;i++) {               //区间修改
            int L, R;   scanf("%d%d",&L,&R);
            update(L,1);                       //本题的d = 1
            update(R+1,-1);
        }
        for(int i=1;i<=n;i++){                //单点查询
            if(i!=n)  printf("%d ",sum(i));   //把sum(i)看成a[i]
            else      printf("%d\n",sum(i));
        }
    }
    return 0;
}
```

例 4-5. 线段树 1 洛谷 P3372

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 100010;
#define lowbit(x)  ((x) & - (x))
ll tree1[N],tree2[N];                //2个树状数组
void update1(ll x,ll d){while(x<=N) {tree1[x]+=d;  x+=lowbit(x);}}
void update2(ll x,ll d){while(x<=N) {tree2[x]+=d;  x+=lowbit(x);}}
ll sum1(ll x){ll ans = 0;while(x>0) {ans+=tree1[x];x-=lowbit(x);}return ans;}
ll sum2(ll x){ll ans = 0;while(x>0) {ans+=tree2[x];x-=lowbit(x);}return ans;}
int main(){
    ll n, m; scanf("%lld%lld",&n,&m);
    ll old = 0, a;
	for (int i=1;i<=n;i++) {
        scanf("%lld",&a);            //输入每个初始值
        update1(i, a-old);           //差分数组原理，初始化
        update2(i,(i-1)*(a-old));
        old = a;
    }
	while (m--){                     //m个操作
	   ll q, L, R, d;   scanf("%lld",&q);
	   if (q==1){                    //区间修改
            scanf("%lld%lld%lld",&L, &R, &d);
            update1(L,d);             //第1个树状数组
            update1(R+1,-d);
            update2(L,d*(L-1));       //第2个树状数组
            update2(R+1,-d*R);        //d*R = d*(R+1-1)
        }
	   else {                        //区间询问
            scanf("%lld%lld",&L,&R);
            printf("%lld\n",R*sum1(R)-sum2(R) - (L-1)*sum1(L-1)+sum2(L-1));
        }
	}
	return 0;
}
```

**4.2.3 树状数组扩展应用**

例 4-6. 上帝造题的七分钟 洛谷 P4514

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 2050;
int t1[N][N],t2[N][N],t3[N][N],t4[N][N];
#define lowbit(x)  ((x) & - (x))
int n,m;
void update(int x, int y, int d)
{
    for (int i = x; i <= n; i += lowbit(i))
        for (int j = y; j <= m; j += lowbit(j))
        {
            t1[i][j] += d;
            t2[i][j] += x * d;
            t3[i][j] += y * d;
            t4[i][j] += x * y * d;
        }
}
int sum(int x, int y)
{
    int ans = 0;
    for (int i = x; i > 0; i -= lowbit(i))
        for (int j = y; j > 0; j -= lowbit(j))
            ans += (x + 1) * (y + 1) * t1[i][j] - (y + 1) * t2[i][j] - (x + 1) * t3[i][j] + t4[i][j];
    return ans;
}
int main(){
    char ch[2];
    scanf("%s", ch);
    scanf("%d%d", &n, &m);
    while (scanf("%s", ch) != EOF)
    {
        int a, b, c, d, delta;
        scanf("%d%d%d%d", &a, &b, &c, &d);
        if (ch[0] == 'L')
        {
            scanf("%d", &delta);
            update(a, b, delta);
            update(c + 1, d + 1, delta);
            update(a, d + 1, -delta);
            update(c + 1, b, -delta);
        }
        else
            printf("%d\n", sum(c, d) + sum(a - 1, b - 1) - sum(a - 1, d) - sum(c, b - 1));
    }
    return 0;
}
```

例 4-7. 逆序对 洛谷 P1908

作为对比，下面先给出用归并排序求解逆序对的代码，解释见第 2 章的“2.9 分治法”。

```cpp
//用归并排序求解洛谷P1908
#include<bits/stdc++.h>
const int N = 5e5+5;
int a[N],tmp[N];                    //a[]是原数组，b[]用于合并两半部分
long long ans=0;                    //记录逆序对数量
void Merge(int L, int mid, int R)
{ // 合并
    int i = L, j = mid + 1, t = L;
    while (i <= mid && j <= R)
    {
        if (a[i] > a[j])
        {
            ans += mid - i + 1; // 记录逆序对数量。去掉这一行，就是纯的归并排序
            tmp[t++] = a[j++];
        }
        else
            tmp[t++] = a[i++];
    }
    // 其中一半已经处理完。另一半还没有，它剩下的都是有序的，直接copy
    while (i <= mid)
        tmp[t++] = a[i++];
    while (j <= R)
        tmp[t++] = a[j++];
    for (i = L; i <= R; i++)
        a[i] = tmp[i]; // 把排好序的b[]复制回去
}
void Mergesort(int L, int R)
{ // 分治
    if (L >= R)
        return;
    int mid = L + (R - L) / 2; // 比这样写好：int mid=(L+R)/2; 因为L+R可能溢出
    Mergesort(L, mid);         // 左半
    Mergesort(mid + 1, R);     // 右半
    Merge(L, mid, R);          // 合并
}
int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    Mergesort(1, n); // 归并排序，并统计逆序对
    printf("%lld\n", ans);
    return 0;
}
```

下面是洛谷 P1908 的代码

```cpp
//lowbit(x)，update()，sum()的代码前面已给出
const int N = 500010;
int tree[N],rank[N],n;  //注：rank是C++的保留字，如果加了using namespace std，编译通不过
//在需要用c++定义的函数和变量时，例如sort，这样写：std::sort()
struct point{ int num,val;}a[N];
bool cmp(point x, point y)
{
    if (x.val == y.val)
        return x.num < y.num; // 如果相等，让先出现的更小
    return x.val < y.val;
}
int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
    {
        scanf("%d", &a[i].val);
        a[i].num = i; // 记录顺序，用于离散化
    }
    sort(a + 1, a + 1 + n, cmp); // 排序
    for (int i = 1; i <= n; i++)
        rank[a[i].num] = i; // 离散化，得到新的数字序列rank[]
    long long ans = 0;
    /*for(int i=1;i<=n;i++){     //正序处理
          update(rank[i],1);
          ans += i-sum(rank[i]); }*/
    for (int i = n; i > 0; --i)
    { // 倒序处理
        update(rank[i], 1);
        ans += sum(rank[i] - 1);
    }
    printf("%lld", ans);
    return 0;
}
```

例 4-8. I Hate It hdu 1754

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 2e5+10;
int n,m,a[N],tree[N];
int lowbit(int x){return x&(-x);}
void update1(int x,int value){;}       //代码在前面
int query1(int L,int R){;}             //代码在前面
int main(){
    while (~scanf("%d%d", &n, &m))
    {
        memset(tree, 0, sizeof(tree));
        for (int i = 1; i <= n; i++)
        {
            scanf("%d", &a[i]);
            update1(i, a[i]);
        }
        while (m--)
        {
            char s[5];
            int A, B;
            scanf("%s%d%d", s, &A, &B);
            if (s[0] == 'Q')
                printf("%d\n", query1(A, B));
            else
            {
                a[A] = B;
                update1(A, B);
            }
        }
    }
    return 0;
}
```

#### 4.3 线段树

**4.3.2 区间查询**

```cpp
//洛谷 P3372，线段树，区间修改 + 区间查询
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 1e5 + 10;
ll a[N];        //记录数列的元素，从a[1]开始
ll tree[N<<2];  //tree[i]：第i个结点的值，表示一个线段区间的值，例如最值、区间和
ll tag[N<<2];   //tag[i]：第i个结点的lazy-tag，统一记录这个区间的修改
ll ls(ll p){ return p<<1;  }           //定位左儿子：p*2
ll rs(ll p){ return p<<1|1;}           //定位右儿子：p*2 + 1
void push_up(ll p){                    //从下往上传递区间值
    tree[p] = tree[ls(p)] + tree[rs(p)];
     //本题是区间和。如果求最小值，改为：tree[p] = min(tree[ls(p)], tree[rs(p)]);
}
void build(ll p,ll pl,ll pr){    //建树。p是结点编号，它指向区间[pl, pr]
    tag[p] = 0;                         //lazy-tag标记
    if(pl==pr){tree[p]=a[pl]; return;}  //最底层的叶子，赋值
    ll mid = (pl+pr) >> 1;              //分治：折半
    build(ls(p),pl,mid);                //左儿子
    build(rs(p),mid+1,pr);              //右儿子
    push_up(p);                         //从下往上传递区间值
}
void addtag(ll p,ll pl,ll pr,ll d){     //给结点p打tag标记，并更新tree
    tag[p] += d;                        //打上tag标记
    tree[p] += d*(pr-pl+1);             //计算新的tree
}
void push_down(ll p,ll pl,ll pr){       //不能覆盖时，把tag传给子树
    if(tag[p]){                         //有tag标记，这是以前做区间修改时留下的
        ll mid = (pl+pr)>>1;
        addtag(ls(p),pl,mid,tag[p]);    //把tag标记传给左子树
        addtag(rs(p),mid+1,pr,tag[p]);  //把tag标记传给右子树
        tag[p]=0;                       //p自己的tag被传走了，归0
    }
}
void update(ll L,ll R,ll p,ll pl,ll pr,ll d){ //区间修改：把[L, R]内每个元素加上d
    if(L<=pl && pr<=R){       //完全覆盖，直接返回这个结点，它的子树不用再深入了
        addtag(p, pl, pr,d);  //给结点p打tag标记，下一次区间修改到p时会用到
        return;
    }
    push_down(p,pl,pr);                 //如果不能覆盖，把tag传给子树
    ll mid=(pl+pr)>>1;
    if(L<=mid) update(L,R,ls(p),pl,mid,d);    //递归左子树
    if(R>mid)  update(L,R,rs(p),mid+1,pr,d);  //递归右子树
    push_up(p);                               //更新
}
ll query(ll L,ll R,ll p,ll pl,ll pr){
  //查询区间[L,R]；p是当前结点（线段）的编号，[pl,pr]是结点p表示的线段区间
    if(pl>=L && R >= pr) return tree[p];       //完全覆盖，直接返回
    push_down(p,pl,pr);                        //不能覆盖，递归子树
    ll res=0;
    ll mid = (pl+pr)>>1;
    if(L<=mid) res+=query(L,R,ls(p),pl,mid);   //左子结点有重叠
    if(R>mid)  res+=query(L,R,rs(p),mid+1,pr); //右子结点有重叠
    return res;
}
int main(){
    ll n, m;  scanf("%lld%lld",&n,&m);
    for(ll i=1;i<=n;i++)  scanf("%lld",&a[i]);
    build(1,1,n);                              //建树
    while(m--){
        ll q,L,R,d;     scanf("%lld",&q);
        if (q==1){                             //区间修改：把[L,R]的每个元素加上d
            scanf("%lld%lld%lld",&L,&R,&d);
            update(L,R,1,1,n,d);
        }
        else {                                 //区间询问：[L,R]的区间和
            scanf("%lld%lld",&L,&R);
            printf("%lld\n",query(L,R,1,1,n));
        }
    }
    return 0;
}

```

**4.3.5 区间最值和区间历史最值**

例 4-14. Gorgeous Sequence hdu 5306

```cpp
//代码改写自：https://blog.csdn.net/nbl97/article/details/76696784
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 1e6 + 10;
ll sum[N<<2], ma[N<<2], se[N<<2], num[N<<2]; //num:区间的最大值个数
ll ls(ll p){ return p<<1;  }
ll rs(ll p){ return p<<1|1;}
void pushup(int p) {                         //从下往上传递
    sum[p] = sum[ls(p)] + sum[rs(p)];        //传递区间和
    ma[p] = max(ma[ls(p)], ma[rs(p)]);       //传递区间最大值
    if (ma[ls(p)] == ma[rs(p)]) {
        se[p] = max(se[ls(p)], se[rs(p)]);
        num[p] = num[ls(p)] + num[rs(p)];
    }
    else {
        se[p] = max(se[ls(p)],se[rs(p)]);
        se[p] = max(se[p],min(ma[ls(p)], ma[rs(p)]));
        num[p] = ma[ls(p)] > ma[rs(p)] ? num[ls(p)] : num[rs(p)];
    }
}
void build(int p, int pl, int pr) {
    if (pl == pr) {                          //叶子
        scanf("%lld", &sum[p]);
        ma[p] = sum[p];  se[p] = -1;  num[p] = 1;
        return;
    }
    ll mid = (pl+pr) >> 1;
    build(ls(p),pl,mid);
    build(rs(p),mid+1,pr);
    pushup(p);
}
void addtag(int p, int x) {
    if (x >= ma[p])    return;
    sum[p] -= num[p] * (ma[p] - x);
    ma[p] = x;
}
void pushdown(int p) {
    addtag(ls(p), ma[p]);  //把标记传给左子树
    addtag(rs(p), ma[p]);  //把标记传给左子树
}
void update(int L, int R, int p, int pl, int pr, int x) {
    if (x >= ma[p])   return;                                  //情况（1）
    if (L<=pl && pr<=R && se[p] < x) { addtag(p, x); return;}  //情况（2）
    pushdown(p);                                               //情况（3）
    ll mid = (pl+pr) >> 1;
    if (L<=mid)   update(L, R, ls(p), pl, mid, x);
    if (R>mid)    update(L, R, rs(p), mid+1, pr, x);
    pushup(p);
}
int queryMax(int L, int R, int p, int pl, int pr) {
    if (pl>=L && R >= pr)  return ma[p];
    pushdown(p);
    int res = 0;
    ll mid = (pl+pr) >> 1;
    if (L<=mid)   res = queryMax(L, R, ls(p), pl, mid);
    if (R>mid)    res = max(res, queryMax(L, R, rs(p), mid+1, pr));
    return res;
}
ll querySum(int L, int R, int p, int pl, int pr) {
    if (L <= pl && R >= pr)   return sum[p];
    pushdown(p);
    ll res = 0;
    ll mid = (pl+pr) >> 1;
    if (L<=mid)   res += querySum(L, R, ls(p), pl, mid);
    if (R>mid)    res += querySum(L, R, rs(p), mid+1, pr);
    return res;
}
int main(){
    int T;   scanf("%d", &T);
    while (T--) {
        int n,m; scanf("%d%d", &n, &m);
        build(1, 1, n);
        while (m--) {
            int q, L, R, x;   scanf("%d%d%d", &q, &L, &R);
            if (q == 0){ scanf("%d", &x); update(L, R, 1, 1, n, x);}
            if (q == 1)  printf("%d\n", queryMax(L, R, 1, 1, n));
            if (q == 2)  printf("%lld\n", querySum(L, R, 1, 1, n));
        }
    }
    return 0;
}
```

**4.3.6 区间合并**

例 4-16. Tunnel Warfare hdu 1540

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 50010;
int ls(int p){ return p<<1;  }
int rs(int p){ return p<<1|1;}
int tree[N<<2], pre[N<<2], suf[N<<2]; //tree:记录元素的值；pre:前缀1的个数；suf：后缀1的个数
int history[N];                       //记录村庄被毁的历史
void push_up(int p,int len){          //len是结点p的长度
    pre[p]=pre[ls(p)];                //父结点接收子结点的前缀信息
    suf[p]=suf[rs(p)];
    if(pre[ls(p)]==(len-(len>>1)))  pre[p]=pre[ls(p)]+pre[rs(p)]; //左儿子都是1
    if(suf[rs(p)]==(len>>1))        suf[p]=suf[ls(p)]+suf[rs(p)]; //右儿子都是1
}
void build(int p, int pl,int pr){
    if(pl==pr){tree[p]=pre[p]=suf[p]=1;  return;}
    int mid = (pl+pr) >> 1;
    build(ls(p),pl,mid);
    build(rs(p),mid+1,pr);
    push_up(p,pr-pl+1);
}
void update(int x, int c, int p, int pl, int pr){
    if (pl == pr)
    {
        tree[p] = suf[p] = pre[p] = c;
        return;
    } // 更新叶子结点信息
    int mid = (pl + pr) >> 1;
    if (x <= mid)
        update(x, c, ls(p), pl, mid);
    else
        update(x, c, rs(p), mid + 1, pr);
    push_up(p, pr - pl + 1);
}
int query(int x,int p,int pl,int pr){
    if(pl==pr)   return tree[p];    //返回叶子的值
    int mid=(pl+pr)>>1;
    if(x<=mid){                     //左子树
        if(x + suf[ls(p)] > mid)   return suf[ls(p)] + pre[rs(p)];
        else                       return query(x,ls(p),pl,mid);
    }
    else{                           //右子树
        if(mid + pre[rs(p)] >= x)  return pre[rs(p)] + suf[ls(p)];
        else                       return query(x,rs(p),mid+1,pr);
    }
}
int main(){
    int n,m,x,tot;
    while(scanf("%d%d",&n,&m)>0)    {
        build(1, 1,n);
        tot = 0;
        while(m--){
            char op[10]; scanf("%s",op);
            if(op[0]=='Q'){scanf("%d",&x);  printf("%d\n",query(x,1,1,n));}
            else if(op[0]=='D'){
                scanf("%d",&x);
                history[++tot]=x;            //记录毁灭的历史
                update(x,0,1,1,n);
            }
            else {x=history[tot--]; update(x,1,1,1,n); }    //重建
        }
    }
}
```

**4.3.7 扫描线**

例 4-19. Atlantis hdu 1542

```cpp
//代码改写自：https://blog.csdn.net/narcissus2_/article/details/88418870
#include<bits/stdc++.h>
using namespace std;
int ls(int p){ return p<<1;  }
int rs(int p){ return p<<1|1;}
const int N = 20005;
int Tag[N];            //标志：线段是否有效，能否用于计算宽度
double length[N];      //存放区间i的总宽度
double xx[N];          //存放x坐标值，下标用lower_bound查找
struct ScanLine
{                           // 定义扫描线
    double y;               // 边的y坐标
    double right_x, left_x; // 边的x坐标:右、左
    int inout;              // 入边为1，出边为-1
    ScanLine() {}
    ScanLine(double y, double x2, double x1, int io) : y(y), right_x(x2), left_x(x1), inout(io) {}
} line[N];
bool cmp(ScanLine &a,ScanLine &b) { return a.y<b.y; }   //y坐标排序
void pushup(int p, int pl, int pr)
{ // 从下往上传递区间值
    if (Tag[p])
        length[p] = xx[pr] - xx[pl];
    // 结点的Tag为正，这个线段对计算宽度有效。计算宽度
    else if (pl + 1 == pr)
        length[p] = 0; // 叶子结点没有宽度
    else
        length[p] = length[ls(p)] + length[rs(p)];
}
void update(int L,int R,int io,int p,int pl,int pr){
    if(L<=pl && pr<=R){                //完全覆盖
        Tag[p] += io;                  //结点的标志，用来判断能否用来计算宽度
        pushup(p,pl,pr);
        return;
    }
    if(pl+1 == pr)  return;            //叶子结点
    int mid = (pl+pr) >> 1;
    if(L<=mid)  update(L,R,io,ls(p),pl,mid);
    if(R>mid)   update(L,R,io,rs(p),mid,pr);   //注意不是mid+1
    pushup(p,pl,pr);
}
int main(){
    int n, t = 0;
    while(scanf("%d",&n),n){
        int cnt = 0;        //边的数量，包括入边和出边
        while(n--){
            double x1,x2,y1,y2; scanf("%lf%lf%lf%lf",&x1,&y1,&x2,&y2);//输入一个矩形
            line[++cnt] = ScanLine(y1,x2,x1,1);      //给入边赋值
            xx[cnt] = x1;                            //记录x坐标
            line[++cnt] = ScanLine(y2,x2,x1,-1);     //给出边赋值
            xx[cnt] = x2;                            //记录x坐标
        }
        sort(xx+1,xx+cnt+1);                         //对所有边的x坐标排序
        sort(line+1,line+cnt+1,cmp);            //对扫描线按y轴方向从低到高排序
        int num = unique(xx+1,xx+cnt+1)-(xx+1); //离散化：用unique去重，返回个数
        memset(Tag,0,sizeof(Tag));
        memset(length,0,sizeof(length));
        double ans = 0;
        for(int i=1;i<=cnt;++i) {                    //扫描所有入边和出边
            int L,R;
            ans += length[1]*(line[i].y-line[i-1].y);//累加当前扫描线的面积=宽*高
            L = lower_bound(xx+1,xx+num+1,line[i].left_x)-xx;
                                           //x坐标离散化：用相对位置代替坐标值
            R = lower_bound(xx+1,xx+num+1,line[i].right_x)-xx;
            update(L,R,line[i].inout,1,1,num);
        }
        printf("Test case #%d\nTotal explored area: %.2f\n\n",++t,ans);
    }
    return 0;
}
```

下面是“矩形周长并”的模板题。

例 4-20. Picture hdu1828

```cpp
//代码改写自：https://blog.csdn.net/qq3434569/article/details/78220821
#include<bits/stdc++.h>
using namespace std;
int ls(int p){ return p<<1;  }
int rs(int p){ return p<<1|1;}
const int N = 200005;
struct ScanLine {
int l, r, h, inout;  //inout=1 下边, inout=-1 上边
ScanLine() {}
ScanLine(int a, int b, int c, int d) :l(a), r(b), h(c), inout(d) {}
}line[N];
bool cmp(ScanLine &a, ScanLine &b) { return a.h<b.h; }   //y坐标排序
bool lbd[N<<2], rbd[N<<2]; //标记这个结点的左右两个端点是否被覆盖（0表示没有，1表示有）
int num[N << 2];           //这个区间有多少条独立的边
int Tag[N << 2];           //标记这个结点是否有效
int length[N << 2];        //这个区间的有效宽度
void pushup(int p, int pl, int pr) {
	if (Tag[p]) {                  //结点的Tag为正，这个线段对计算宽度有效
		lbd[p] = rbd[p] = 1;
		length[p] = pr - pl + 1;
		num[p] = 1;               //每条边有两个端点
	}
	else if (pl == pr) length[p]=num[p]=lbd[p]=rbd[p]=0;//叶子结点
	else {
		lbd[p] = lbd[ls(p)];      //和左儿子共左端点
		rbd[p] = rbd[rs(p)];      //和右儿子共右端点
		length[p] = length[ls(p)] + length[rs(p)];
		num[p] = num[ls(p)] + num[rs(p)];
		if (lbd[rs(p)] && rbd[ls(p)]) num[p] -= 1;   //合并边
	}
}
void update(int L, int R, int io, int p, int pl, int pr) {
    if(L<=pl && pr<=R){    //完全覆盖
		Tag[p] += io;
		pushup(p, pl, pr);
		return;
	}
	int mid  = (pl + pr) >> 1;
	if (L<= mid) update(L, R, io, ls(p), pl, mid);
	if (mid < R) update(L, R, io, rs(p), mid+1, pr);
	pushup(p, pl, pr);
}
int main() {
	int n;
	while (~scanf("%d", & n)) {
		int cnt = 0;
		int lbd = 10000, rbd = -10000;
		for (int i = 0; i < n; i++) {
			int x1, y1, x2, y2;
			scanf("%d%d%d%d", & x1, & y1, & x2, & y2); //输入矩形
			lbd = min(lbd, x1); //横线最小x坐标
			rbd = max(rbd, x2); //横线最大x坐标
			line[++cnt] = ScanLine(x1, x2, y1, 1); //给入边赋值
			line[++cnt] = ScanLine(x1, x2, y2, -1); //给出边赋值
		}
		sort(line + 1, line + cnt + 1, cmp); //排序。数据小，不用离散化
		int ans = 0, last = 0; //last：上一次总区间被覆盖长度
		for (int i = 1; i <= cnt; i++) { //扫描所有入边和出边
			if (line[i].l < line[i].r)
				update(line[i].l, line[i].r - 1, line[i].inout, 1, lbd, rbd - 1);
			ans += num[1] * 2 * (line[i + 1].h - line[i].h); //竖线
			ans += abs(length[1] - last); //横线
			last = length[1];
		}
		printf("%d\n", ans);
	}
	return 0;
}
```

**4.3.8 二维线段树（树套树）**

例 4-21. Luck and Love hdu 1823

```cpp
//代码改写自https://www.cnblogs.com/ftae/p/7739512.html
#include<bits/stdc++.h>
using namespace std;
int ls(int p){ return p<<1;  }
int rs(int p){ return p<<1|1;}
int n=1000, s[1005][4005];      //s[i][j]：身高区间i，活泼区间j的最大缘分
void subBuild(int xp, int p, int pl, int pr) {  //建立第二维线段树：活泼度线段树
    s[xp][p] = -1;
    if(pl == pr) return;
    int mid=(pl+pr)>>1;
    subBuild(xp, ls(p), pl, mid);
    subBuild(xp, rs(p), mid + 1, pr);
}
void build(int p,int pl, int pr) {              //建立第一维线段树：身高线段树
    subBuild(p, 1, 0, n);
    if(pl == pr) return;
    int mid=(pl+pr)>>1;
    build(ls(p),pl, mid);
    build(rs(p),mid + 1, pr);
}
void subUpdate(int xp, int y, int c, int p, int pl, int pr) {//更新第二维线段树
    if(pl == pr && pl == y) s[xp][p] = max(s[xp][p], c);
    else {
        int mid=(pl+pr)>>1;
        if(y <= mid) subUpdate(xp, y, c, ls(p), pl, mid);
        else subUpdate(xp, y, c, rs(p), mid + 1, pr);
        s[xp][p] = max(s[xp][ls(p)], s[xp][rs(p)]);
    }
}
void update(int x, int y, int c, int p, int pl, int pr){ //更新第一维线段树：身高x
    subUpdate(p, y, c, 1, 0, n);                         //更新第二维线段树：活泼度y
    if(pl != pr) {
        int mid=(pl+pr)>>1;
        if(x <= mid) update(x, y, c, ls(p), pl, mid);
        else update(x, y, c, rs(p), mid + 1, pr);
    }
}
int subQuery(int xp, int yL, int yR, int p, int pl, int pr) { //查询第二维线段树
    if(yL <= pl && pr <= yR) return s[xp][p];
    else {
        int mid=(pl+pr)>>1;
        int res = -1;
        if(yL <= mid) res = subQuery(xp, yL, yR, ls(p), pl, mid);
        if(yR >  mid) res = max(res, subQuery(xp, yL, yR, rs(p), mid + 1, pr));
        return res;
    }
}
int query(int xL, int xR, int yL, int yR, int p, int pl, int pr) {//查询第一维线段树
    if(xL <= pl && pr <= xR) return subQuery(p, yL, yR, 1, 0, n);
                                       //满足身高区间时，查询活泼度区间
    else {                             //当前结点不满足身高区间
        int mid = (pl+pr)>>1;
        int res = -1;
        if(xL <= mid) res = query(xL, xR, yL, yR, ls(p), pl, mid);
        if(xR >  mid) res = max(res, query(xL, xR, yL, yR, rs(p), mid + 1, pr));
        return res;
    }
}
int main(){
    int t;
    while(scanf("%d", &t) && t) {
        build(1,100, 200);
        while(t--) {
            char ch[2];       scanf("%s", ch);
            if(ch[0] == 'I') {
                int h;double c, d; scanf("%d%lf%lf", &h, &c, &d);
                update(h, c * 10, d * 10, 1, 100, 200);
            } else {
                int xL, xR, yL, yR; double c,d;
                scanf("%d%d%lf%lf", &xL, &xR, &c, &d);
                yL =  c * 10, yR = d * 10;                     //转整数
                if(xL > xR) swap(xL, xR);
                if(yL > yR) swap(yL, yR);
                int ans = query(xL, xR, yL, yR, 1,100, 200);   //x：身高，y：活泼度
                if(ans == -1) printf("-1\n");
                else          printf("%.1f\n", ans / 10.0);
            }
        }
    }
    return 0;
}
```

#### 4.4 可持久化线段树

**4.4.2 区间第 k 大/小问题**

例 4-22. 主席树 洛谷 P3834

```cpp
//洛谷 P3834代码，改写自：https://www.luogu.com.cn/problem/solution/P3834
#include <bits/stdc++.h>
using namespace std ;
const int N = 200010;
int cnt = 0;        //用cnt标记可以使用的新结点
int a[N], b[N], root[N]; //a[]是原数组,b[]是排序后数组，root[i]记录第i棵线段树的根结点编号
struct{             //定义结点
int L, R, sum;  //L左儿子, R右儿子，sum[i]是结点i的权值（即图中圆圈内的数字）
}tree[N<<5];        //  <<4是乘16倍，不够用；<<5差不多够用
int build(int pl, int pr){        //初始化一棵空树，实际上无必要
    int rt = ++ cnt;              //cnt为当前结点编号
    tree[rt].sum = 0;
    int mid=(pl+pr)>>1;
    if (pl < pr){
        tree[rt].L = build(pl, mid);
        tree[rt].R = build(mid+1, pr);
    }
    return rt;      //返回当前结点的编号
}
int update(int pre, int pl, int pr, int x){   //建一棵只有logn个结点的新线段树
    int rt = ++cnt;           //新的结点，下面动态开点
    tree[rt].L = tree[pre].L; //该结点的左右儿子初始化为前一棵树相同位置结点的左右儿子
    tree[rt].R = tree[pre].R;
    tree[rt].sum = tree[pre].sum + 1;  //插了1个数，在前一棵树的相同结点加1
    int mid = (pl+pr)>>1;
    if (pl < pr){            //从根结点往下建logn个结点
        if (x <= mid)        //x出现在左子树，修改左子树
            tree[rt].L = update(tree[pre].L, pl, mid, x);
        else                 //x出现在右子树，修改右子树
            tree[rt].R = update(tree[pre].R, mid+1, pr, x);
    }
    return rt;               //返回当前分配使用的新结点的编号
}
int query(int u, int v, int pl, int pr, int k){    //查询区间[u,v]第k小
    if (pl == pr) return pl;  //到达叶子结点，找到第k小，pl是结点编号，答案是b[pl]
    int x = tree[tree[v].L].sum - tree[tree[u].L].sum;   //线段树相减
    int mid = (pl+pr)>>1;
    if (x >= k)     //左儿子数字大于等于k时，说明第k小的数字在左子树
        return query(tree[u].L, tree[v].L, pl, mid, k);
    else            //否则在右子树找第k-x小的数字
        return query(tree[u].R, tree[v].R, mid+1, pr, k-x);
}
int main(){
    int n, m;    scanf("%d%d", &n, &m);
    for (int i=1; i<=n; i++){ scanf("%d", &a[i]);  b[i]=a[i]; }
    sort(b+1, b+1+n);    //对b排序
    int size = unique(b+1, b+1+n)-b-1; //size等于b数组中不重复的数字的个数
    //root[0] = build(1, size);   //初始化一棵包含size个元素的空树，实际上无必要
    for (int i = 1; i <= n; i ++){     //建n棵线段树
        int x = lower_bound(b+1, b+1+size, a[i]) - b;
                                  //找等于a[i]的b[x]。x是离散化后a[i]对应的值
        root[i] = update(root[i-1], 1, size, x);
                                  //建第i棵线段树，root[i]是第i棵线段树的根结点
    }
    while (m--){
        int x, y, k;      scanf("%d%d%d", &x, &y, &k);
        int t = query(root[x-1], root[y], 1, size, k);
                          //第y棵线段树减第x-1棵线段树，就是区间[x,y]的线段树
        printf("%d\n", b[t]);
    }
    return 0;
}
```

**4.5.2 基础莫队算法**

例 4-29. HH 项链 洛谷 1972

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e6;
struct node{           //离线记录查询操作
    int L, R, k;       //k：查询操作的原始顺序
}q[N];
int pos[N];
int ans[N];
int cnt[N];            //cnt[i]: 统计数字i出现了多少次
int a[N];
bool cmp(node a, node b)
{
    // 按块排序，就是莫队算法：
    if (pos[a.L] != pos[b.L]) // 按L所在的块排序，如果块相等，再按R排序
        return pos[a.L] < pos[b.L];
    if (pos[a.L] & 1)
        return a.R > b.R; // 奇偶性优化，如果删除这一句，性能差一点
    return a.R < b.R;
    /*如果不按块排序，而是直接L、R排序，就是普通暴力法：
    if(a.L==b.L)  return a.R < b.R;
         return a.L < b.L;   */
}
int ANS = 0;
void add(int x){     //扩大区间时（L左移或R右移），增加数x出现的次数
    cnt[a[x]]++;
    if(cnt[a[x]]==1)  ANS++;     //这个元素第1次出现
}
void del(int x){     //缩小区间时（L右移或R左移），减少数x出现的次数
    cnt[a[x]]--;
    if(cnt[a[x]]==0)  ANS--;     //这个元素消失了
}
int main(){
    int n; scanf("%d",&n);
    int block = sqrt(n);         //每块的大小
    for(int i=1;i<=n;i++){
        scanf("%d",&a[i]);       //读第i个元素
		pos[i]=(i-1)/block + 1;  //第i个元素所在的块
    }
    int m; scanf("%d",&m);
    for(int i=1;i<=m;i++){       //读取所有m个查询，离线处理
        scanf("%d%d",&q[i].L, &q[i].R);
        q[i].k = i;              //记录查询的原始顺序
    }
    sort(q+1, q+1+m, cmp);       //对所有查询排序
    int L = 1, R = 0;            // 左右指针的初始值。思考为什么？
    for (int i = 1; i <= m; i++)
    {
        while (L < q[i].L)
            del(L++); //{del(L); L++;}  //缩小区间：L右移
        while (R > q[i].R)
            del(R--); //{del(R); R--;}  //缩小区间：R左移
        while (L > q[i].L)
            add(--L); //{L--; add(L);}  //扩大区间：L左移
        while (R < q[i].R)
            add(++R); //{R++; add(R);}  //扩大区间：R右移
        ans[q[i].k] = ANS;
    }
    for (int i = 1; i <= m; i++)
        printf("%d\n", ans[i]); // 按原顺序打印结果
    return 0;
}
```

#### 4.6 块状链表

```cpp
例4-32. 文本编辑器 洛谷 P4008
//改写自：https://www.luogu.com.cn/blog/EndSaH/solution-p4008
#include<bits/stdc++.h>
using namespace std;
int block = 2500;                       //一个块的标准大小 = sqrt(n)
list<vector<char> > List;               //整体是链表，链表的每个元素是一个块
typedef list<vector<char> >::iterator it;
it Find(int &pos) {                     //返回块，并更新x为这个块中的位置
    for (it i = List.begin(); ;i++) {   //逐个找链表上的每个块
        if(i == List.end() || pos <= i->size())  return i;
        pos -= i->size();               //每经过一个块，就更新x
    }
}
void Output(int L, int R)  {            // [L, R)
    it L_block = Find(L), R_block = Find(R);
    for (it it1 = L_block;  ; it1++){               //打印每个块
         int a;  it1 == L_block ? a=L : a=0;        //一个块的起点
         int b;  it1 == R_block ? b=R : b=it1->size();    //块的终点
         for (int i = a; i < b; i++)  putchar(it1->at(i));
         if(it1 == R_block) break;   //迭代器it不能用 <= ，只有 == 和 !=
    }
    putchar('\n');
}
it Next(it x){return ++x; }  //返回下一个块
void Merge(it x) {           //合并块x和块x+1
    x->insert(x->end(), Next(x)->begin(), Next(x)->end());
    List.erase(Next(x));
}
void Split(it x, int pos){   //把第x个块在这个块的pos处分成2块
    if (pos == x->size())  return;         //pos在这个块的末尾
    List.insert(Next(x), vector<char>(x->begin() + pos, x->end()));
                             //把pos后面的部分划给下一个块
    x->erase(x->begin() + pos, x->end());   //删除划出的部分
}
void Update(){                //把每个块重新划成等长的块
    for (it i = List.begin(); i != List.end(); i++){
        while (i->size() >= (block << 1))   //如果块大于2个block，分开
            Split(i, i->size() - block);
        while (Next(i) != List.end() && i->size() + Next(i)->size() <= block)
            Merge(i);                       //如果块+下一个块小于block，合并
        while (Next(i) != List.end() && Next(i)->empty()) //删除最后的空块
            List.erase(Next(i));
    }
}
void Insert(int pos, const vector<char>& ch){
    it curr = Find(pos);
    if (!List.empty())     Split(curr, pos);  //把一个块拆为两个
    List.insert(Next(curr), ch);              //把字符串插到两个块中间
    Update();
}
void Delete(int L, int R) {                   // [L, R)
    it L_block, R_block;
    L_block = Find(L); Split(L_block, L);
    R_block = Find(R); Split(R_block, R);
    R_block++;
    while(Next(L_block) != R_block)   List.erase(Next(L_block));
    Update();
}
int main(){
    vector<char> ch; int len, pos, n;
    cin >> n;
    while (n--) {
        char opt[7];   cin >> opt;
        if(opt[0]=='M') cin >> pos;
        if(opt[0]=='I'){
            ch.clear();  cin >> len;  ch.resize(len);
            for (int i = 0; i < len; i++){
                ch[i] = getchar();
                while(ch[i]<32||ch[i]>126)  ch[i]=getchar();
            }                   //读一个合法字符
            Insert(pos, ch);    //把字符串插入到链表中
        }
        if(opt[0]=='D'){  cin >> len;    Delete(pos, pos + len); }
        if(opt[0]=='G'){  cin >> len;    Output(pos, pos + len); }
        if(opt[0]=='P')  pos--;
        if(opt[0]=='N')  pos++;
    }
    return 0;
}
```

#### 4.7 简单树上问题

**4.7.1 树的重心**

例 4-33. Godfather poj 3107

```cpp
// poj 3107的代码（链式前向星存树）
#include<cstdio>
#include<algorithm>
using namespace std;
const int N = 50005;          //最大结点数
struct Edge{ int to, next;} edge[N<<1];      //两倍：u-v, v-u
int head[N], cnt = 0;
void init(){                  //链式前向星：初始化
    for(int i=0; i<N; ++i){
        edge[i].next = -1;
        head[i] = -1;
    }
    cnt = 0;
}
void addedge(int u,int v){    //链式前向星：加边u-v
    edge[cnt].to = v;
    edge[cnt].next = head[u];
    head[u] = cnt++;
}
int n;
int d[N], ans[N], num=0, maxnum=1e9;        //d[u]: 以u为根的子树的结点数量
void dfs(int u,int fa){
    d[u] = 1;                               //递归到最底层时，结点数加1
    int tmp = 0;
    for(int i=head[u]; ~i; i=edge[i].next){ //遍历u的子结点。~i也可以写成i!=-1
        int v = edge[i].to;                 //v是一个子结点
        if(v == fa) continue;               //不递归父亲
        dfs(v,u);                           //递归子结点，计算v这个子树的结点数量
        d[u] += d[v];                       //计算以u为根的结点数量
        tmp = max(tmp,d[v]);                //记录u的最大子树的结点数量
    }
    tmp = max(tmp, n-d[u]);                 //tmp = u的最大连通块的结点数
    //以上计算出了u的最大连通块
    //下面统计疑似教父。如果一个结点的最大连通块比其他结点的都小，它是疑似教父
    if(tmp < maxnum){                       //一个疑似教父
        maxnum = tmp;                       //更新“最小的”最大连通块
        num = 0;
        ans[++num] = u;                     //把教父记录在第1个位置
    }
    else if(tmp == maxnum)  ans[++num] = u; //疑似教父有多个，记录在后面
}
int main(){
    scanf("%d",&n);
    init();
    for(int i=1; i<n; i++){
        int u, v;      scanf("%d %d", &u, &v);
        addedge(u,v);  addedge(v,u);
    }
    dfs(1,0);
    sort(ans+1, ans+1+num);
    for(int i=1;i<=num;i++)   printf("%d ",ans[i]);
}
```

**4.7.2 树的直径**

例 4-34. 树的直径

1. 做两次 DFS（或 BFS）

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+10;
struct edge{ int to,w;};    //to: 边的终点  w:权值
vector<edge> e[N];          //用邻接表存边
int dist[N];                //记录距离
void dfs(int u,int father,int d){   //用dfs计算从u到每个子结点的距离
    dist[u]=d;
    for(int i=0;i<e[u].size();i++)
        if(e[u][i].to != father)    //很关键，这一句保证不回头搜父结点
            dfs(e[u][i].to, u, d + e[u][i].w);
}
int main(void){
    int n;   cin>>n;
    for(int i=0;i<n-1;i++){
        int a,b,w; cin>>a>>b>>w;
        e[a].push_back({b,w});       //a的邻居是b，边长w
        e[b].push_back({a,w});       //b的邻居是a
    }
    dfs(1,-1,0);                     //计算从任意点（这里用1号点）到树上每个结点的距离
    int s = 1;
    for(int i=1;i<=n;i++)            //找最远的结点s, s是直径的一个端点
        if(dist[i]>dist[s])  s = i;
    dfs(s,-1,0);                     //从s出发，计算以s为起点，到树上每个结点的距离
    int t = 1;
    for(int i=1;i<=n;i++)            //找直径的另一个端点t
        if(dist[i]>dist[t])  t = i;
    cout << dist[t]<<endl;           //打印树的直径的长度
    return 0;
}
```

2. 树形 DP

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+10;
struct edge{int to,w; };           //to: 边的终点  w:权值
vector<edge> e[N];
int dp[N];
int maxlen = 0;
bool vis[N];
void dfs(int u){
    vis[u] = true;
    for(int i = 0; i < e[u].size(); ++ i){
        int v = e[u][i].to,  edge = e[u][i].w;
        if(vis[v])   continue;     //v已经算过
        dfs(v);
        maxlen = max(maxlen, dp[u]+ dp[v]+ edge);
               //计算max{f[u]}。注意此时dp[u]不包括v这棵子树，下一行才包括
        dp[u] = max(dp[u], dp[v] + edge); //计算dp[u]，此时包括了v这棵子树
    }
    return ;
}
int main(){
    int n;    cin >> n;
    for(int i = 0; i < n-1; i++){
        int a, b, w;     cin >> a >> b >> w;
        e[a].push_back({b,w});  //a的邻居是b，路的长度w
        e[b].push_back({a,w});  //b的邻居是a
    }
    dfs(1);                     //从点1开始DFS
    cout << maxlen << endl;
    return 0;
}
```

#### 4.8 LCA

**4.8.1 倍增法求 LCA**

例 4-35. 最近公共祖先 洛谷 P3379

```cpp
//洛谷P3379 的倍增代码
#include <bits/stdc++.h>
using namespace std;
const int N=500005;
struct Edge{int to, next;}edge[2*N];     //链式前向星
int head[2*N], cnt;
void init()
{ // 链式前向星：初始化
    for (int i = 0; i < 2 * N; ++i)
    {
        edge[i].next = -1;
        head[i] = -1;
    }
    cnt = 0;
}
void addedge(int u, int v)
{ // 链式前向星：加边
    edge[cnt].to = v;
    edge[cnt].next = head[u];
    head[u] = cnt++;
} // 以上是链式前向星
int fa[N][20], deep[N];
void dfs(int x, int father)
{                                             // 求x的深度deep[x]和fa[x][]。father是x的父结点。
    deep[x] = deep[father] + 1;               // 深度：比父结点深度多1
    fa[x][0] = father;                        // 记录父结点
    for (int i = 1; (1 << i) <= deep[x]; i++) // 求fa[][]数组，它最多到根结点
        fa[x][i] = fa[fa[x][i - 1]][i - 1];
    for (int i = head[x]; ~i; i = edge[i].next) // 遍历结点i的所有孩子。~i可写为i!=-1
        if (edge[i].to != father)               // 邻居：除了父亲，都是孩子
            dfs(edge[i].to, x);
}
int LCA(int x, int y)
{
    if (deep[x] < deep[y])
        swap(x, y); // 让x位于更底层，即x的深度值更大
    // （1）把x和y提到相同的深度
    for (int i = 19; i >= 0; i--)          // x最多跳19次：2^19 > 500005
        if (deep[x] - (1 << i) >= deep[y]) // 如果x跳过头了就换个小的i重跳
            x = fa[x][i];                  // 如果x还没跳到y的层，就更新x继续跳
    if (x == y)
        return x; // y就是x的祖先
    // （2）x和y同步往上跳，找到LCA
    for (int i = 19; i >= 0; i--) // 如果祖先相等，说明跳过头了，换个小的i重跳
        if (fa[x][i] != fa[y][i])
        { // 如果祖先不等，就更新x、y继续跳
            x = fa[x][i];
            y = fa[y][i];
        }
    return fa[x][0]; // 最后x位于LCA的下一层，父结点fa[x][0]就是LCA
}
int main()
{
    init(); // 初始化链式前向星
    int n, m, root;
    scanf("%d%d%d", &n, &m, &root);
    for (int i = 1; i < n; i++)
    { // 读一棵树，用链式前向星存储
        int u, v;
        scanf("%d%d", &u, &v);
        addedge(u, v);
        addedge(v, u);
    }
    dfs(root, 0); // 计算每个结点的深度并预处理fa[][]数组
    while (m--)
    {
        int a, b;
        scanf("%d%d", &a, &b);
        printf("%d\n", LCA(a, b));
    }
    return 0;
}
```

**4.8.2 Tarjan 求 LCA**

```cpp
//洛谷P3379的tarjan代码，改写自 blog.csdn.net/Harington/article/details/105901338
#include <bits/stdc++.h>
using namespace std;
const int N = 500005;
int fa[N], head[N], cnt, head_query[N], cnt_query, ans[N];
bool vis[N];
struct Edge
{
    int to, next, num;
} edge[2 * N], query[2 * N]; // 链式前向星
void init()
{ // 链式前向星：初始化
    for (int i = 0; i < 2 * N; ++i)
    {
        edge[i].next = -1;
        head[i] = -1;
        query[i].next = -1;
        head_query[i] = -1;
    }
    cnt = 0;
    cnt_query = 0;
}
void addedge(int u, int v)
{ // 链式前向星：加边
    edge[cnt].to = v;
    edge[cnt].next = head[u];
    head[u] = cnt++;
}
void add_query(int x, int y, int num)
{ // num 第几个查询
    query[cnt_query].to = y;
    query[cnt_query].num = num; // 第几个查询
    query[cnt_query].next = head_query[x];
    head_query[x] = cnt_query++;
}
int find_set(int x)
{ // 并查集查询
    return fa[x] == x ? x : find_set(fa[x]);
}
void tarjan(int x)
{ // tarjan是一个DFS
    vis[x] = true;
    for (int i = head[x]; ~i; i = edge[i].next)
    { // ~i可以写为i!=-1
        int y = edge[i].to;
        if (!vis[y])
        { // 遍历子结点
            tarjan(y);
            fa[y] = x; // 合并并查集：把子结点y合并到父结点x上
        }
    }
    for (int i = head_query[x]; ~i; i = query[i].next)
    { // 查询所有和x有询问关系的y
        int y = query[i].to;
        if (vis[y])                          // 如果to被访问过
            ans[query[i].num] = find_set(y); // LCA就是find(y)
    }
}
int main()
{
    init();
    memset(vis, 0, sizeof(vis));
    int n, m, root;
    scanf("%d%d%d", &n, &m, &root);
    for (int i = 1; i < n; i++)
    {              // 读n个结点
        fa[i] = i; // 并查集初始化
        int u, v;
        scanf("%d%d", &u, &v);
        addedge(u, v);
        addedge(v, u); // 存边
    }
    fa[n] = n; // 并查集的结点n
    for (int i = 1; i <= m; ++i)
    { // 读m个询问
        int a, b;
        scanf("%d%d", &a, &b);
        add_query(a, b, i);
        add_query(b, a, i); // 存查询
    }
    tarjan(root);
    for (int i = 1; i <= m; ++i)
        printf("%d\n", ans[i]);
}
```

**4.8.3 LCA 应用**

例 4-36. Max Flow P 洛谷 P3128

```cpp
//洛谷P3128，LCA + 树上差分
#include <bits/stdc++.h>
using namespace std;
#define N 50010
struct Edge
{
    int to, next;
} edge[2 * N]; // 链式前向星
int head[2 * N], D[N], deep[N], fa[N][20], ans, cnt;
void init();
void addedge(int u, int v);
void dfs1(int x, int father);
int LCA(int x, int y); // 以上4个函数和“树上的倍增”中洛谷P3379的倍增代码完全一样
void dfs2(int u, int fath)
{
    for (int i = head[u]; ~i; i = edge[i].next)
    { // 遍历结点i的所有孩子。~i可以写为i!=-1
        int e = edge[i].to;
        if (e == fath)
            continue;
        dfs2(e, u);
        D[u] += D[e];
    }
    Ans = max(ans, D[u]);
}
int main()
{
    init(); // 链式前向星初始化
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i < n; ++i)
    {
        int u, v;
        scanf("%d%d", &u, &v);
        addedge(u, v);
        addedge(v, u);
    }
    dfs1(1, 0); // 计算每个结点的深度并预处理fa[][]数组
    for (int i = 1; i <= m; ++i)
    {
        int a, b;
        scanf("%d%d", &a, &b);
        int lca = LCA(a, b);
        D[a]++;
        D[b]++;
        D[lca]--;
        D[fa[lca][0]]--; // 树上差分
    }
    dfs2(1, 0); // 用差分数组求每个结点的权值
    printf("%d\n", ans);
    return 0;
}
```


#### 4.10 树链剖分

**4.10.1 树链剖分的概念与LCA **

```cpp
//洛谷P3379 的树链剖分代码
#include <bits/stdc++.h>
using namespace std;
const int N = 500005;
struct Edge
{
    int to, next;
} edge[2 * N]; // 链式前向星
int head[2 * N], cnt;
void init()
{ // 链式前向星：初始化
    for (int i = 0; i < 2 * N; ++i)
    {
        edge[i].next = -1;
        head[i] = -1;
    }
    cnt = 0;
}
void addedge(int u, int v)
{ // 链式前向星：加边
    edge[cnt].to = v;
    edge[cnt].next = head[u];
    head[u] = cnt++;
} // 以上是链式前向星
int deep[N], siz[N], son[N], top[N], fa[N];
void dfs1(int x, int father)
{
    deep[x] = deep[father] + 1; // 深度：比父结点深度多1
    fa[x] = father;             // 标记x的父亲
    siz[x] = 1;                 // 标记每个结点的子树大小（包括自己）
    for (int i = head[x]; ~i; i = edge[i].next)
    {
        int y = edge[i].to;
        if (y != father)
        { // 邻居：除了父亲，都是孩子
            fa[y] = x;
            dfs1(y, x);
            siz[x] += siz[y];                    // 回溯后，把x的儿子数加到x身上
            if (!son[x] || siz[son[x]] < siz[y]) // 标记每个非叶子结点的重儿子
                son[x] = y;                      // x的重儿子是y
        }
    }
}
void dfs2(int x, int topx)
{
    // id[x] = ++num;            //对每个结点新编号，在下一小节用到
    top[x] = topx; // x所在链的链头
    if (!son[x])
        return;         // x是叶子，没有儿子，返回
    dfs2(son[x], topx); // 先dfs重儿子，所有重儿子的链头都是topx
    for (int i = head[x]; ~i; i = edge[i].next)
    { // 再dfs轻儿子
        int y = edge[i].to;
        if (y != fa[x] && y != son[x])
            dfs2(y, y); // 每一个轻儿子都有一条以它为链头的重链
    }
}
int LCA(int x, int y)
{
    while (top[x] != top[y])
    { // 持续往上跳，直到若x和y属于同一条重链
        if (deep[top[x]] < deep[top[y]])
            swap(x, y); // 让x是链头更深的重链
        x = fa[top[x]]; // x穿过轻边，跳到上一条重链
    }
    return deep[x] < deep[y] ? x : y;
}
int main()
{
    init();
    int n, m, root;
    scanf("%d%d%d", &n, &m, &root);
    for (int i = 1; i < n; i++)
    {
        int u, v;
        scanf("%d%d", &u, &v);
        addedge(u, v);
        addedge(v, u);
    }
    dfs1(root, 0);
    dfs2(root, root);
    while (m--)
    {
        int a, b;
        scanf("%d%d", &a, &b);
        printf("%d\n", LCA(a, b));
    }
}
```

**4.10.2 树链剖分的典型应用**

例 4-40. 轻重链剖分 洛谷 P3384

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=100000+10;
int n,m,r,mod;
//以下是链式前向星
struct Edge{int to, next;}edge[2*N];
int head[2*N], cnt;
void init();                 //与前一小节“洛谷P3379树链剖分”的init()一样
void addedge(int u,int v);   //与前一小节“洛谷P3379树链剖分”的addedge()一样
//以下是线段树
int ls(int x) { return x << 1; }     // 定位左儿子：x*2
int rs(int x) { return x << 1 | 1; } // 定位右儿子：x*2 + 1
int w[N], w_new[N];                  // w[]、w_new[]初始点权
int tree[N << 2], tag[N << 2];       // 线段树数组、lazy-tag操作
void addtag(int p, int pl, int pr, int d)
{                // 给结点p打tag标记，并更新tree
    tag[p] += d; // 打上tag标记
    tree[p] += d * (pr - pl + 1);
    tree[p] %= mod; // 计算新的tree
}
void push_up(int p)
{ // 从下往上传递区间值
    tree[p] = tree[ls(p)] + tree[rs(p)];
    tree[p] %= mod;
}
void push_down(int p, int pl, int pr)
{
    if (tag[p])
    {
        int mid = (pl + pr) >> 1;
        addtag(ls(p), pl, mid, tag[p]);     // 把tag标记传给左子树
        addtag(rs(p), mid + 1, pr, tag[p]); // 把tag标记传给右子树
        tag[p] = 0;
    }
}
void build(int p, int pl, int pr)
{ // 建线段树
    tag[p] = 0;
    if (pl == pr)
    {
        tree[p] = w_new[pl];
        tree[p] %= mod;
        return;
    }
    int mid = (pl + pr) >> 1;
    build(ls(p), pl, mid);
    build(rs(p), mid + 1, pr);
    push_up(p);
}
void update(int L, int R, int p, int pl, int pr, int d)
{
    if (L <= pl && pr <= R)
    {
        addtag(p, pl, pr, d);
        return;
    }
    push_down(p, pl, pr);
    int mid = (pl + pr) >> 1;
    if (L <= mid)
        update(L, R, ls(p), pl, mid, d);
    if (R > mid)
        update(L, R, rs(p), mid + 1, pr, d);
    push_up(p);
}
int query(int L, int R, int p, int pl, int pr)
{
    if (pl >= L && R >= pr)
        return tree[p] %= mod;
    push_down(p, pl, pr);
    int res = 0;
    int mid = (pl + pr) >> 1;
    if (L <= mid)
        res += query(L, R, ls(p), pl, mid);
    if (R > mid)
        res += query(L, R, rs(p), mid + 1, pr);
    return res;
}
// 以下是树链剖分
int son[N], id[N], fa[N], deep[N], siz[N], top[N];
void dfs1(int x, int father); // 与前一小节“洛谷P3379树链剖分”dfs1()一样
int num = 0;
void dfs2(int x, int topx)
{                      // x当前结点，topx当前链的最顶端的结点
    id[x] = ++num;     // 对每个结点新编号
    w_new[num] = w[x]; // 把每个点的初始值赋给新编号
    top[x] = topx;     // 记录x的链头
    if (!son[x])
        return;         // x是叶子，没有儿子，返回
    dfs2(son[x], topx); // 先dfs重儿子
    for (int i = head[x]; ~i; i = edge[i].next)
    { // 再dfs轻儿子
        int y = edge[i].to;
        if (y != fa[x] && y != son[x])
            dfs2(y, y); // 每个轻儿子都有一条从它自己开始的链
    }
}
void update_range(int x, int y, int z)
{ // 和求LCA(x, y)的过程差不多
    while (top[x] != top[y])
    {
        if (deep[top[x]] < deep[top[y]])
            swap(x, y);
        update(id[top[x]], id[x], 1, 1, n, z); // 修改一条重链的内部
        x = fa[top[x]];
    }
    if (deep[x] > deep[y])
        swap(x, y);
    update(id[x], id[y], 1, 1, n, z); // 修改一条重链的内部
}
int query_range(int x, int y)
{ // 和求LCA(x,y)的过程差不多
    int ans = 0;
    while (top[x] != top[y])
    { // 持续往上跳，直到若x和y属于同一条重链
        if (deep[top[x]] < deep[top[y]])
            swap(x, y);                           // 让x是链头更深的重链
        ans += query(id[top[x]], id[x], 1, 1, n); // 加上x到x的链头这一段区间
        ans %= mod;
        x = fa[top[x]]; // x穿过轻边，跳到上一条重链
    }
    if (deep[x] > deep[y])
        swap(x, y);
    // 若LCA(x, y) = y，交换x,y,让x更浅，使得id[x] <= id[y]
    ans += query(id[x], id[y], 1, 1, n); // 再加上x, y的区间和
    return ans % mod;
}
void update_tree(int x, int k) { update(id[x], id[x] + siz[x] - 1, 1, 1, n, k); }
int query_tree(int x) { return query(id[x], id[x] + siz[x] - 1, 1, 1, n) % mod; }
int main()
{
    init(); // 链式前向星初始化
    scanf("%d%d%d%d", &n, &m, &r, &mod);
    for (int i = 1; i <= n; i++)
        scanf("%d", &w[i]);
    for (int i = 1; i < n; i++)
    {
        int u, v;
        scanf("%d%d", &u, &v);
        addedge(u, v);
        addedge(v, u);
    }
    dfs1(r, 0);
    dfs2(r, r);
    build(1, 1, n); // 建线段树
    while (m--)
    {
        int k, x, y, z;
        scanf("%d", &k);
        switch (k)
        {
        case 1:
            scanf("%d%d%d", &x, &y, &z);
            update_range(x, y, z);
            break;
        case 2:
            scanf("%d%d", &x, &y);
            printf("%d\n", query_range(x, y));
            break;
        case 3:
            scanf("%d%d", &x, &y);
            update_tree(x, y);
            break;
        case 4:
            scanf("%d", &x);
            printf("%d\n", query_tree(x));
            break;
        }
    }
}
```

#### 4.12 替罪羊树

**4.12.3 例题**

例4-41. 普通平衡树 洛谷P3369

```cpp
// 洛谷P3369  替罪羊树
#include <bits/stdc++.h>
using namespace std;
const int N = 1e6 + 10;
const double alpha = 0.75; // 不平衡率。一般用alpha来表示
struct Node
{
    int ls, rs; // 左右儿子
    int val;    // 结点存的数字
    int tot;    // 当前子树占用的空间数量，包括实际存储的结点和被标记删去的点
    int size;   // 子树上实际存储数字的数量
    int del;    //=1表示这个结点存有数字，=0表示这个点存的数字被删了
} t[N];
int order[N], cnt;          // order[]记录拍扁后的结果，即那些存有数字的结点。cnt是数量
int tree_stack[N], top = 0; // 用一个栈来回收和分配可用的结点
int root = 0;               // 根结点，注意重建过程中根结点会变化
void inorder(int u)
{ // 中序遍历，“拍平”摧毁这棵子树
    if (!u)
        return;       // 已经到达叶子，退出
    inorder(t[u].ls); // 先遍历左子树
    if (t[u].del)
        order[++cnt] = u; // 如果该结点存有数字，读取它
    else
        tree_stack[++top] = u; // 回收该结点，等待重新分配使用
    inorder(t[u].rs);          // 再遍历右子树
}
void Initnode(int u)
{ // 重置结点的参数
    t[u].ls = t[u].rs = 0;
    t[u].size = t[u].tot = t[u].del = 1;
}
void Update(int u)
{
    t[u].size = t[t[u].ls].size + t[t[u].rs].size + 1;
    t[u].tot = t[t[u].ls].tot + t[t[u].rs].tot + 1;
}
// int rebuild_num=0;                      //测试：统计重建次数
void build(int l, int r, int &u)
{ // 把拍扁的子树拎起来，重建
    //  rebuild_num++;                        //测试：统计重建次数
    int mid = (l + r) >> 1; // 新的根设为中点，使重构出的树尽量平衡
    u = order[mid];
    if (l == r)
    {
        Initnode(u);
        return;
    } // 如果是叶子，重置后返回
    if (l < mid)
        build(l, mid - 1, t[u].ls); // 重构左子树
    if (l == mid)
        t[u].ls = 0;            // 注意这里，不要漏了
    build(mid + 1, r, t[u].rs); // 重构右子树
    Update(u);                  // 更新
}
void rebuild(int &u)
{ // 重建。注意是&u
    cnt = 0;
    inorder(u); // 先拍平摧毁
    if (cnt)
        build(1, cnt, u); // 再拎起，重建树
    else
        u = 0; // 特判该子树为空的情况
}
bool notbalance(int u)
{ // 判断子树u是否平衡
    if ((double)t[u].size * alpha <= (double)max(t[t[u].ls].size, t[t[u].rs].size))
        return true; // 不平衡了
    return false;    // 还是平衡的
}
void Insert(int &u, int x)
{ // 插入数字x。注意是&u，传回了新的u
    if (!u)
    {                          // 如果结点u为空，直接将x插到这里
        u = tree_stack[top--]; // 从栈顶拿出可用的空结点
        t[u].val = x;          // 结点赋值
        Initnode(u);           // 其他参数初始化
        return;
    }
    t[u].size++;
    t[u].tot++;
    if (t[u].val >= x)
        Insert(t[u].ls, x); // 插到右子树
    else
        Insert(t[u].rs, x); // 插到左子树
    if (notbalance(u))
        rebuild(u); // 如果不平衡了，重建这棵子树
}
int Rank(int u, int x)
{ // 排名，x是第几名
    if (u == 0)
        return 0;
    if (x > t[u].val)
        return t[t[u].ls].size + t[u].del + Rank(t[u].rs, x);
    return Rank(t[u].ls, x);
}
int kth(int k)
{ // 第k大数是几?
    int u = root;
    while (u)
    {
        if (t[u].del && t[t[u].ls].size + 1 == k)
            return t[u].val;
        else if (t[t[u].ls].size >= k)
            u = t[u].ls;
        else
        {
            k -= t[t[u].ls].size + t[u].del;
            u = t[u].rs;
        }
    }
    return t[u].val;
}
void Del_k(int &u, int k)
{                // 删除排名为k的数
    t[u].size--; // 要删除的数肯定在这棵子树中，size减1
    if (t[u].del && t[t[u].ls].size + 1 == k)
    {
        t[u].del = 0; // del=0表示这个点u被删除了，但是还保留位置
        return;
    }
    if (t[t[u].ls].size + t[u].del >= k)
        Del_k(t[u].ls, k); // 在左子树上
    else
        Del_k(t[u].rs, k - t[t[u].ls].size - t[u].del); // 在右子树上
}
void Del(int x)
{                                   // 删除值为k的数
    Del_k(root, Rank(root, x) + 1); // 先找x的排名，然后用Del_k()删除
    if (t[root].tot * alpha >= t[root].size)
        rebuild(root); // 如果子树上被删除的结点太多，就重构
}
/*
void print_tree(int u){          //测试：打印二叉树，观察
    if(u){
        cout<<"v="<<t[u].val<<",l="<<t[u].ls<<",r="<<t[u].rs<<endl;
        print_tree(t[u].ls);
        print_tree(t[u].rs);
    }
}
int tree_deep[N]={0},deep_timer=0,max_deep=0;    //测试
void cnt_deep(int u){                            //测试：计算二叉树的深度
    if(u){
        tree_deep[u]=++deep_timer;               //结点u的深度
        max_deep = max(max_deep,tree_deep[u]);   //记录曾经的最大深度
        cnt_deep(t[u].ls);
        cnt_deep(t[u].rs);
        deep_timer--;
    }
}   */
int main()
{
    for (int i = N - 1; i >= 1; i--)
        tree_stack[++top] = i; // 把所有可用的t[]记录在这个栈里面
    int q;
    cin >> q;
    //  rebuild_num = 0;deep_timer=0;max_deep=0;         //测试
    while (q--)
    {
        int opt, x;
        cin >> opt >> x;
        switch (opt)
        {
        case 1:
            Insert(root, x);
            break;
        case 2:
            Del(x);
            break;
        case 3:
            printf("%d\n", Rank(root, x) + 1);
            break;
        case 4:
            printf("%d\n", kth(x));
            break;
        case 5:
            printf("%d\n", kth(Rank(root, x)));
            break;
        case 6:
            printf("%d\n", kth(Rank(root, x + 1) + 1));
            break;
        }
        //      cout<<">>"<<endl;print_tree(root);cout<<endl<<"<<"<<endl;  //测试：打印二叉树
        //      cnt_deep(root);                 //测试：计算曾经的最大深度
    }
    //  cout<<"rebuild num="<<rebuild_num<<endl;  //测试：打印重建次数
    //  cout<<"deep="<<max_deep<<endl;            //测试：打印替罪羊树的最大深度
    return 0;
}

```

#### 4.13 Treap树

**4.13.2 基于旋转法的Treap操作**

```cpp
// 洛谷P3369，用旋转法实现Treap树
#include <bits/stdc++.h>
using namespace std;
const int M = 1e6 + 10;
int cnt = 0; // t[cnt]: 最新结点的存储位置
struct Node
{
    int ls, rs;   // 左右儿子
    int key, pri; // key：键值；pri：随机的优先级
    int size;     // 当前结点为根的子树的结点数量，用于求第k大和rank
} t[M];           // tree[]，存树
void newNode(int x)
{          // 初始化一个新结点
    cnt++; // 从t[1]开始存储结点，t[0]被放弃。若子结点是0，表示没有子结点
    t[cnt].size = 1;
    t[cnt].ls = t[cnt].rs = 0; // 0表示没有子结点
    t[cnt].key = x;            // key: 键值
    t[cnt].pri = rand();       // pri：随机产生，优先级
}
void Update(int u)
{ // 更新以u为根的子树的size
    t[u].size = t[t[u].ls].size + t[t[u].rs].size + 1;
}
void rotate(int &o, int d)
{ // 旋转，参考图示理解。 d=0右旋，d=1左旋
    int k;
    if (d == 1)
    { // 左旋，把o的右儿子k旋到根部
        k = t[o].rs;
        t[o].rs = t[k].ls; // 图中的x
        t[k].ls = o;
    }
    else
    { // 右旋，把o的左儿子k旋到根部
        k = t[o].ls;
        t[o].ls = t[k].rs; // 图中的x
        t[k].rs = o;
    }
    t[k].size = t[o].size;
    Update(o);
    o = k; // 新的根是k
}
void Insert(int &u, int x)
{
    if (u == 0)
    {
        newNode(x);
        u = cnt;
        return;
    } // 递归到了一个空叶子，新建结点
    t[u].size++;
    if (x >= t[u].key)
        Insert(t[u].rs, x); // 递归右子树找空叶子，直到插入
    else
        Insert(t[u].ls, x); // 递归左子树找空叶子，直到插入
    if (t[u].ls != 0 && t[u].pri > t[t[u].ls].pri)
        rotate(u, 0);
    if (t[u].rs != 0 && t[u].pri > t[t[u].rs].pri)
        rotate(u, 1);
    Update(u);
}
void Del(int &u, int x)
{
    t[u].size--;
    if (t[u].key == x)
    {
        if (t[u].ls == 0 && t[u].rs == 0)
        {
            u = 0;
            return;
        }
        if (t[u].ls == 0 || t[u].rs == 0)
        {
            u = t[u].ls + t[u].rs;
            return;
        }
        if (t[t[u].ls].pri < t[t[u].rs].pri)
        {
            rotate(u, 0);
            Del(t[u].rs, x);
            return;
        }
        else
        {
            rotate(u, 1);
            Del(t[u].ls, x);
            return;
        }
    }
    if (t[u].key >= x)
        Del(t[u].ls, x);
    else
        Del(t[u].rs, x);
    Update(u);
}
int Rank(int u, int x)
{ // 排名，x是第几名
    if (u == 0)
        return 0;
    if (x > t[u].key)
        return t[t[u].ls].size + 1 + Rank(t[u].rs, x);
    return Rank(t[u].ls, x);
}
int kth(int u, int k)
{ // 第k大数是几?
    if (k == t[t[u].ls].size + 1)
        return t[u].key; // 这个数为根
    if (k > t[t[u].ls].size + 1)
        return kth(t[u].rs, k - t[t[u].ls].size - 1); // 右子树
    if (k <= t[t[u].ls].size)
        kth(t[u].ls, k); // 左子树
}
int Precursor(int u, int x)
{
    if (u == 0)
        return 0;
    if (t[u].key >= x)
        return Precursor(t[u].ls, x);
    int tmp = Precursor(t[u].rs, x);
    if (tmp == 0)
        return t[u].key;
    return tmp;
}
int Successor(int u, int x)
{
    if (u == 0)
        return 0;
    if (t[u].key <= x)
        return Successor(t[u].rs, x);
    int tmp = Successor(t[u].ls, x);
    if (tmp == 0)
        return t[u].key;
    return tmp;
}
int main()
{
    srand(time(NULL));
    int root = 0; // root是整棵树的树根，0表示初始为空
    int n;
    cin >> n;
    while (n--)
    {
        int opt, x;
        cin >> opt >> x;
        switch (opt)
        {
        case 1:
            Insert(root, x);
            break;
        case 2:
            Del(root, x);
            break;
        case 3:
            printf("%d\n", Rank(root, x) + 1);
            break;
        case 4:
            printf("%d\n", kth(root, x));
            break;
        case 5:
            printf("%d\n", Precursor(root, x));
            break;
        case 6:
            printf("%d\n", Successor(root, x));
            break;
        }
    }
    return 0;
}
```

#### 4.14 FHQ Treap树

**4.14.1 FHQ 的基本操作**

```cpp
//洛谷P3369，FHQ Treap
#include<bits/stdc++.h>
using namespace std;
const int M=1e6+10;
int cnt=0, root=0;        //t[cnt]: 最新结点的存储位置；root:整棵树的根，用于访问树
struct Node{
	int ls,rs;           //左儿子lson，右儿子rson
	int key,pri;         //key：键值，pri：随机的优先级
	int size;            //当前结点为根的子树的结点数量，用于求第k大和rank
}t[M];                   //tree[],存树
void newNode(int x){     //建立只有一个点的树
	cnt++;               //从t[1]开始存储结点，t[0]被放弃
	t[cnt].size = 1;
	t[cnt].ls = t[cnt].rs = 0;  //0表示没有子结点
	t[cnt].key = x;             //key: 键值
	t[cnt].pri = rand();        //pri：随机产生，优先级
}
void Update(int u){       //更新以u为根的子树的size
	t[u].size = t[t[u].ls].size + t[t[u].rs].size+1;
}
void Split(int u,int x,int &L,int &R) {   //权值分裂。返回以L和R为根的2棵树
	if(u == 0){L = R = 0;	return;}      //到达叶子，递归返回
	if(t[u].key <= x){                   //本结点比x小，那么到右子树上找x
		L = u;                           //左树的根是本结点
		Split(t[u].rs, x, t[u].rs, R);   //通过rs传回新的子结点
	}
	else{                                 //本结点比x大，继续到左子树找x
		R = u;                           //右数的根是本结点
		Split(t[u].ls,x,L,t[u].ls);
	}
	Update(u);//更新当前结点的size
}
int Merge(int L,int R){                   //合并以L和R为根的两棵树，返回一棵树的根
	if(L==0 || R==0)  return L+R;        //到达叶子。若L==0，就是返回L+R=R
	if(t[L].pri > t[R].pri){             //左树L优先级大于右树R，则L节点是父结点
		t[L].rs = Merge(t[L].rs,R);      //合并R和L的右儿子,并更新L的右儿子
		Update(L);
		return L;                        //合并后的根是L
	}
	else{	                            //合并后R是父结点
		t[R].ls = Merge(L,t[R].ls);      //合并L和R的左儿子,并更新R的左儿子
        Update(R);
		return R;                        //合并后的根是R
	}
}
int Insert(int x){                        //插入数字x
    int L,R;
    Split(root,x,L,R);
    newNode(x);                           //新建一棵只有一个点的树t[cnt]
    int aa = Merge(L,cnt);
    root = Merge(aa,R);
}
int Del(int x){        //删除数字x。请对比后面例题洛谷P5055的排名分裂的删除操作
    int L,R,p;
    Split(root,x,L,R);                    //<=x的树和>x的树
    Split(L,x-1,L,p);                     //<x的树和==x的树
    p = Merge(t[p].ls,t[p].rs);           //合并x=p的左右子树，也就是删除了x
    root = Merge(Merge(L,p),R);
}
void Rank(int x){                         //查询数x的排名
    int L,R;
    Split(root,x-1,L,R);                  // <x的树和>=x的树
    printf("%d\n",t[L].size+1);
    root = Merge(L,R);                    //恢复
}
int kth(int u,int k){                     //求排名第k的数
	if(k==t[t[u].ls].size+1) return u;              //这个数为根
	if(k<=t[t[u].ls].size)   return kth(t[u].ls,k); //在左子树
	if(k>t[t[u].ls].size)    return kth(t[u].rs,k-t[t[u].ls].size-1);  //在右子树
}
void Precursor(int x){                    //求x的前驱
    int L,R;
    Split(root,x-1,L,R);
    printf("%d\n",t[kth(L,t[L].size)].key);
    root = Merge(L,R);                   //恢复
}
void Successor(int x){                   //求x的后继
    int L,R;
    Split(root,x,L,R);                   //<=x的树和>x的树
    printf("%d\n",t[kth(R,1)].key);
    root = Merge(L,R);                   //恢复
}
int main(){
    srand(time(NULL));
    int n;	cin >> n;
    while(n--){
        int opt,x; cin >> opt >> x;
        switch (opt){
            case 1: Insert(x);     break;
            case 2: Del(x);        break;
            case 3: Rank(x);       break;
            case 4: printf("%d\n",t[kth(root,x)].key); break;  //排名x的结点
            case 5: Precursor(x);  break;
            case 6: Successor(x);  break;
        }
	}
	return 0;
}
```

**4.14.2 FHQ Treap 应用**

```cpp
//洛谷P4008的FHQ treap代码
#include<bits/stdc++.h>
using namespace std;
const int M = 2e6+10;
int root = 0,cnt = 0;
struct Node{int ls,rs; char val; int pri; int size;}t[M];              //tree[]存树
void Update(int u){t[u].size = t[t[u].ls].size + t[t[u].rs].size+1;}   //用于排名分裂
int newNode(char x){     //建立只有一个点的树
	cnt++;
	t[cnt].size = 1;  t[cnt].pri = rand();  t[cnt].ls = t[cnt].rs = 0;
	t[cnt].val = x;      //一个字符
	return cnt;
}
void Split(int u,int x,int &L,int &R){   //排名分裂，不是权值分裂
    if(u == 0){L = R = 0; return ;}
    if(t[t[u].ls].size+1 <= x){          //第x个数在u的右子树上
        L = u;   Split(t[u].rs, x-t[t[u].ls].size-1, t[u].rs, R);
    }
    else{R = u; Split(t[u].ls,x,L,t[u].ls); }     //第x个数在左子树上
    Update(u);
}
int Merge(int L,int R){                  //合并
	if(L==0 || R==0)  return L+R;
	if(t[L].pri > t[R].pri){ t[L].rs = Merge(t[L].rs,R); Update(L);  return L; }
	else{ t[R].ls = Merge(L,t[R].ls);  Update(R);	return R;}
}
void inorder(int u){                     //中序遍历，打印结果
    if(u == 0) return;
    inorder(t[u].ls);    cout << t[u].val;   inorder(t[u].rs);
}
int main(){
    srand(time(NULL));
    int len, L, p, R, pos = 0;      //pos是光标的当前位置
    int n;   cin>>n;
    while(n--){
        char opt[10];    cin>>opt;
        if(opt[0]=='M')  cin>>pos;  //移动光标
        if(opt[0]=='I'){            //插入len个字符
            cin>>len;
            Split(root,pos,L,R);
            for(int i=1;i<=len;i++)  {     //逐个读入字符
                char ch=getchar();   while(ch<32||ch>126)  ch=getchar();
                L = Merge(L,newNode(ch));  //把字符加到树中
            }
            root = Merge(L,R);
        }
        if(opt[0]=='D'){             //删除光标后len个字符
            cin>>len;     Split(root,pos+len,L,R); Split(L,pos,L,p);
            root = Merge(L,R);
        }
        if(opt[0]=='G'){              //打印len个字符
            cin>>len;     Split(root,pos+len,L,R); Split(L,pos,L,p);
            inorder(p); cout<<"\n";   //打印
            root = Merge(Merge(L,p),R);
        }
        if(opt[0]=='P') pos--;
        if(opt[0]=='N') pos++;
    }
    return 0;
}
```

2. 区间翻转

例 4-42. 文艺平衡树 洛谷 P3391

```cpp
#include<bits/stdc++.h>
using namespace std;
const int M = 1e5+10;
int cnt = 0, root = 0;
struct Node{int ls,rs,num,pri,size,lazy;}t[M];     //lazy懒标记，参考线段树
void newNode(int x){
    cnt++;
    t[cnt].size = 1; t[cnt].ls = t[cnt].rs = 0;   t[cnt].pri = rand();
    t[cnt].num = x;              //本题不是按num分裂，是按排名分裂
}
void Update(int u){t[u].size = t[t[u].ls].size + t[t[u].rs].size + 1;}
void pushdown(int u){            //下传lazy标记。如果用splay实现，也是这样做
    if(t[u].lazy) {
        swap(t[u].ls, t[u].rs);  //翻转u的左右部分，翻转不会破坏优先级pri
        t[t[u].ls].lazy ^= 1;  t[t[u].rs].lazy ^= 1;   //向左右子树下传lazy
        t[u].lazy = 0;           //清除标记
    }
}
void Split(int u,int x,int &L,int &R){   //排名分裂。返回：L，前x个数；R，其他数
    if(u == 0){L = R = 0; return ;}      //到达叶子，递归返回
    pushdown(u);                         //处理lazy标记
    if(t[t[u].ls].size+1 <= x){          //第x个数在u的右子树上
        L = u;   Split(t[u].rs, x-t[t[u].ls].size-1, t[u].rs, R);
    }
    else{ R = u; Split(t[u].ls,x,L,t[u].ls);}     //第x个数在左子树上
    Update(u);
}
int Merge(int L,int R){                //合并
    if(L==0 || R==0)   return L+R;
    if(t[L].pri > t[R].pri){           //左树L优先级大于右树R，则L节点是父结点
        pushdown(L);                   //处理lazy标记
        t[L].rs = Merge(t[L].rs,R);  Update(L);   return L;
    }
    else{                               //合并后R是父结点
        pushdown(R);
        t[R].ls = Merge(L,t[R].ls);  Update(R);   return R;
    }
}
void inorder(int u){                     //中序遍历，打印结果
    if(u == 0) return;
    pushdown(u);                         //处理lazy
    inorder(t[u].ls);  cout<<t[u].num<<" ";  inorder(t[u].rs);   //中序遍历
}
int main(){
    srand(time(NULL));
    int n,m;  cin >> n >>m;
    for(int i=1;i<=n;i++){newNode(i); root = Merge(root,cnt);}   //建树，本题比较简单
    while(m--){
        int x,y;  cin >> x >> y;
        int L,R,p;                       //三棵树，分别指向三棵树的根结点
        Split(root,y,L,R);               //分裂成L,p,R三棵树，p是[x,y]区间
        Split(L,x-1,L,p);
        t[p].lazy ^= 1;                  //对p用lazy记录翻转
        root = Merge(Merge(L,p),R);      //合并，还原成一棵树。
    }
    inorder (root);                      //输出
    return 0;
}
```

例 4-43. 可持久化文艺平衡树 洛谷 P5055

```cpp
//改写自：https://www.luogu.com.cn/blog/105496/solution-p5055
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int N=2e5+10;
struct node{int ls,rs,val,pri,size,lazy; ll sum;}t[(N<<7)];
int cnt = 0;
int new_node(int x){
    cnt++;
	t[cnt].size = 1;  	t[cnt].ls = t[cnt].rs = 0;
    t[cnt].val = x;   	t[cnt].sum = x;    t[cnt].pri = rand();
	t[cnt].lazy = 0;
	return cnt;
}
int clone(int u){           //克隆树u。不需要复制整棵树，只复制根就行
	int ret = new_node(0);
	t[ret] = t[u];
	return ret;
}
void Update(int u){
	t[u].size = t[t[u].ls].size + t[t[u].rs].size + 1;
	t[u].sum = t[t[u].ls].sum + t[t[u].rs].sum + t[u].val;
}
void push_down(int u){
	if(!t[u].lazy) return;
	if(t[u].ls != 0) t[u].ls = clone(t[u].ls);
	if(t[u].rs != 0) t[u].rs = clone(t[u].rs);
	swap(t[u].ls,t[u].rs);
    t[t[u].ls].lazy^=1;      t[t[u].rs].lazy^=1;
	t[u].lazy = 0;
}
void Split(int u,int x,int &L,int &R){    //排名分裂
	if(u == 0){L = R = 0; return ;}
	push_down(u);
	if(t[t[u].ls].size+1 <=x){   //第x个数在u的右子树上
        L = clone(u);             //这个时间点的L，它是这个时间点的u的克隆
        Split(t[L].rs, x-t[t[u].ls].size-1, t[L].rs,R);
        Update(L);
    }
	else{
        R = clone(u);             //这个时间点的R，它是这个时间点的u的克隆
        Split(t[R].ls,x,L,t[R].ls);
        Update(R);
    }
}
int Merge(int L,int R){
	if(L==0 || R==0)   return L+R;
	push_down(L); 	push_down(R);
	if(t[L].pri > t[R].pri){ t[L].rs = Merge(t[L].rs,R); Update(L); return L;}
	else  { t[R].ls = Merge(L,t[R].ls); Update(R); return R;}
}
int root[N];
int main(){
    srand(time(NULL));
	int version=0; int L,p,R;  ll x,y;
	for(int i=0;i<N;i++)    root[i] = 0;
	ll lastans=0;
	int n; cin >> n;
	while(n--){
		int v,op;  cin >> v >> op;
		if(op==1){        //在第x个数后插入y
			cin >>x >>y;	x^=lastans;y^=lastans;
			Split(root[v],x,L,p);
			root[++version]=Merge(Merge(L,new_node(y)),p); //记录在新的时间点上
		}
		if(op==2){       //删除第x个数
			cin >>x; x^=lastans;
			Split(root[v],x,L,R);  Split(L,x-1,L,p);
			root[++version] = Merge(L,R);  //记录在新的时间点上
		}
		if(op==3){       //翻转区间[x,y]
			cin >>x >>y;  x^=lastans;  y^=lastans;
			Split(root[v],y,L,R);  Split(L,x-1,L,p);
			t[p].lazy ^= 1;
			root[++version] = Merge(Merge(L,p),R);  //记录在新的时间点上
		}
		if(op==4){       //查询区间和[x, y]
			cin >> x >> y; 	x^=lastans;  y^=lastans;
			Split(root[v],y,L,R);  Split(L,x-1,L,p);  //p树是区间[x,y]
			lastans = t[p].sum;
			cout << lastans << endl;
			root[++version]=Merge(Merge(L,p),R); //记录在新的时间点上
		}
	}
	return 0;
}

```

#### 4.15 笛卡尔树

**4.15.2 用单调栈建笛卡尔树**

例 4-45. Binary Search Heap Construction Poj 1785

```cpp
//改写自：https://blog.csdn.net/qq_40679299/article/details/80395824
#include<cstdio>
#include<algorithm>
#include<cstring>
#include <stack>
using namespace std;
const int N = 50005;
const int INF = 0x7fffffff;
struct Node{
    char s[100];   int ls,rs,fa,pri;
    friend bool operator<(const Node& a,const Node& b){
        return strcmp(a.s,b.s)<0;}
}t[N];
void buildtree(int n){         //不用栈，直接查最右链
    for(int i=1;i<=n;i++){
        int pos = i-1;         //从前一个结点开始比较，前一个结点在最右链的末端
        while(t[pos].pri < t[i].pri)
             pos = t[pos].fa;  //沿着最右链一直找，直到pos的优先级比i大
        t[i].ls = t[pos].rs;   //图(4)：i是19，pos是24, 15调整为19的左儿子
        t[t[i].ls].fa = i;     //图(4)：15的父亲是19
        t[pos].rs = i;         //图(4)：24的右儿子是19
        t[i].fa = pos;         //图(4)：19的父亲是24
    }
}
void buildtree2(int n){             //用栈来辅助建树
    stack <int> ST;  ST.push(0);    //t[0]进栈，它一直在栈底
    for(int i=1;i<=n;i++){
        int pos = ST.top();
        while(!ST.empty() && t[pos].pri < t[i].pri){
            pos = t[ST.top()].fa;
            ST.pop();               //把比i优先级小的弹出栈
        }
        t[i].ls = t[pos].rs;
        t[t[i].ls].fa = i;
        t[pos].rs = i;
        t[i].fa = pos;
        ST.push(i);                 //每个结点都一定要进栈
    }
}
void inorder(int x){                //中序遍历打印
    if(x==0)     return;
    printf("(");
    inorder(t[x].ls);  printf("%s/%d",t[x].s,t[x].pri);  inorder(t[x].rs);
    printf(")");
}
int main(){
    int n;
    while(scanf("%d",&n),n){
        for(int i=1;i<=n;i++){
            t[i].ls = t[i].rs = t[i].fa = 0;        //有多组测试，每次要清零
            scanf(" %[^/]/%d", t[i].s, &t[i].pri);  //注意输入的写法
        }
        t[0].ls = t[0].rs = t[0].fa = 0;            //t[0]不用，从t[1]开始插结点
        t[0].pri = INF;    //t[0]的优先级无穷大
        sort(t+1,t+1+n);   //对标签先排序，非常关键。这样建树时就只需要考虑优先级pri
        buildtree(n);      //buildtree2(n);        //两种建树方法
        inorder(t[0].rs);  //t[0]在树的最左端，第一个点是t[0].rs
        printf("\n");
    }
    return 0;
}
```

#### 4.16 Splay树

**4.16.3 Splay 的常用操作和代码**

```cpp
//改写自 https://www.luogu.com.cn/blog/dedicatus545/solution-p4008
#include<bits/stdc++.h>
using namespace std;
const int M = 2e6+10;
int cnt, root;
struct Node{int fa,ls,rs,size; char val;}t[M];   //tree[]存树;
void Update(int u){t[u].size = t[t[u].ls].size + t[t[u].rs].size+1;}  //用于排名
char str[M]={0};                //一次输入的字符串
int build(int L,int R,int f){   //把字符串str[]建成平衡树
    if(L>R) return 0;
    int mid = (L+R)>>1;
    int cur = ++cnt;
    t[cur].fa = f;
    t[cur].val= str[mid];
    t[cur].ls = build(L,mid-1,cur);
    t[cur].rs = build(mid+1,R,cur);
    Update(cur);
    return cur;                 //返回新树的根
}
int get(int x){return t[t[x].fa].rs == x;} //如果u是右儿子，返回1；左儿子返回0
void rotate(int x){                        //单旋一次
    int f=t[x].fa, g=t[f].fa, son=get(x);  //f:父亲；g：祖父
    if(son==1) {                           //x是右儿子，左旋zag
        t[f].rs = t[x].ls;                 //图“单旋”中的结点b
        if(t[f].rs)  t[t[f].rs].fa = f;
    }
    else {                                 //x是左儿子，右旋zig
        t[f].ls = t[x].rs;
        if(t[f].ls)  t[t[f].ls].fa = f;
    }
    t[f].fa=x;                  //x旋为f的父结点
    if(son == 1) t[x].ls=f;     //左旋，f变为x的左儿子
    else t[x].rs=f;             //右旋，f变为x的右儿子
    t[x].fa = g;                //x现在是祖父的儿子
    if(g){                      //更新祖父的儿子
        if(t[g].rs==f)  t[g].rs = x;
        else  t[g].ls = x;
    }
    Update(f);   Update(x);
}
void splay(int x,int goal){     //goal=0,新的根是x；goal!=0,把x旋为goal的儿子
    if(goal == 0) root=x;
    while(1){
        int f = t[x].fa, g=t[f].fa;        //一次处理x,f,g三个点
        if(f == goal) break;
        if(g != goal) {                    //有祖父，分一字旋和之字旋两种情况
            if(get(x)==get(f)) rotate(f);  //一字旋，先旋f、g
            else rotate(x);}               //之字旋，直接旋x
        rotate(x);
    }
    Update(x);
}
int kth(int k,int u){            //第k大数的位置
    if( k == t[t[u].ls].size + 1) return u;
    if( k <= t[t[u].ls].size )    return kth(k,t[u].ls);
    if( k >= t[t[u].ls].size + 1) return kth(k-t[t[u].ls].size-1,t[u].rs);
}
void Insert(int L,int len){                  //插入一段区间
    int x = kth(L,root), y = kth(L+1,root);  //x:第L个数的位置，y:第L+1个数的位置
    splay(x,0);   splay(y,x);                //分裂
     //先把x旋到根，然后把y旋到x的儿子，此时y是x的右儿子，且y的左儿子为空
    t[y].ls = build(1,len,y);                //合并：建一棵树，挂到y的左儿子上
    Update(y);    Update(x);
}
void del(int L,int R){             //删除区间[L+1,R]
    int x=kth(L,root), y=kth(R+1,root);
    splay(x,0); splay(y,x);        //y是x的右儿子，y的左儿子是待删除的区间
    t[y].ls=0;                 //剪断左子树，等于直接删除。这里为了简单，没有释放空间
    Update(y);    Update(x);
}
void inorder(int u){           //中序遍历
    if(u==0) return;
    inorder(t[u].ls);cout<<t[u].val;inorder(t[u].rs);
}
int main(){
    t[1].size=2;    t[1].ls=2;  //小技巧：虚拟祖父，防止旋转时越界而出错
    t[2].size=1;    t[2].fa=1;  //小技巧：虚拟父亲
    root=1; cnt=2;      //在操作过程中，root将指向字符串的根
    int pos=1;          //光标位置
    int n;   cin>>n;
    while(n--){
        int len;  char opt[10];  cin>>opt;
        if(opt[0]=='I'){
            cin>>len;
            for(int i=1;i<=len;i++){
                char ch=getchar();   while(ch<32||ch>126)  ch=getchar();
                str[i] = ch;
            }
            Insert(pos,len);
        }
        if(opt[0]=='D'){ cin>>len; del(pos,pos+len);}    //删除区间[pos+1,pos+len]
        if(opt[0]=='G'){
            cin>>len;     int x=kth(pos,root), y=kth(pos+len+1,root);
            splay(x,0);   splay(y,x);
            inorder(t[y].ls);    cout<<"\n";
        }
        if(opt[0]=='M'){ cin>>len; pos=len+1;}
        if(opt[0]=='P') pos--;
        if(opt[0]=='N') pos++;
    }
}
```

#### 4.17 K-D树


**4.17.3 寻找最近点**

例 4-46. In case of failure hdu2966

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 100005;
const int K = 2;
#define ll long long
struct Point{int dim[K];};   //K维数据。本题是二维坐标x=dim[0],y=dim[1]
Point q[N];     //记录输入的n个坐标点
Point t[N];     //存二叉树，用最简单的方法：数组存二叉树
int  now;       //当前处于第几维，用于cmp()函数的比较
bool cmp(Point a,Point b){return a.dim[now] < b.dim[now];} //第now维数的比较
ll square(int x){return (ll)x*x;}
ll dis(Point a,Point b){            //点a、b距离的平方
return square(a.dim[0]-b.dim[0])+square(a.dim[1]-b.dim[1]);
}
void build(int L,int R,int dep){    //建树
	if(L>=R) return;
	int d = dep % K;                //轮转法。dep是当前层的深度，d是当前层的维度
	int mid = (L+R) >> 1;
	now = d;
	nth_element(t+L,t+mid,t+R,cmp); //找中值
	build(L,mid,dep+1);             //继续建二叉树的下一层
	build(mid+1,R,dep+1);
}
ll ans;                             //答案：到最近点距离的平方值
void query(int L,int R,int dep,Point p){ //查找点p的最近点
     if(L >= R)return;
     int mid=(L+R)>>1;
     int d = dep % K;                 //轮转法
     ll mindis = dis(t[mid],p);       //这棵子树的根到p的最小距离
     if(ans == 0)  ans = mindis;      //赋初值
     if(mindis!=0 && ans > mindis) ans = mindis;  //需要特判t[mid]和p重合的情况
     if(p.dim[d] > t[mid].dim[d]) {  //在这个维度，p大于子树的根，接下来查右子树
        query(mid+1,R,dep+1,p);
        if(ans > square(t[mid].dim[d]-p.dim[d])) query(L,mid,dep+1,p);
//如果以ans为半径的圆与左子树相交，那么左子树也要查
	}
     else{
         query(L,mid,dep+1,p);        //在这个维度，p小于子树的根，接下来查左子树
         if(ans > square(t[mid].dim[d]-p.dim[d])) //右子树也要查
             query(mid+1,R,dep+1,p);
    }
}
int main(){
     int T; scanf("%d",&T);
     while(T--){
		int n; scanf("%d",&n);
		for(int i=0;i<n;i++)
	  	    scanf("%d%d",&(q[i].dim[0]),&(q[i].dim[1])), t[i] = q[i];
		build(0,n,0);     //建树
		for(int i=0;i<n;i++){
		    ans=0;
		    query(0,n,0,q[i]);
			printf("%lld\n",ans);
		}
	}
	return 0;
}
```

**4.17.4 区间查询**

例 4-47. 简单题 洛谷 P4148

```cpp
#include<bits/stdc++.h>
using namespace std;
const int  N = 200010;
const double alpha = 0.75; //替罪羊树的不平衡率
#define lc t[u].ls
#define rc t[u].rs
struct Point{
	int dim[2],val;         //dim[0]即x，dim[1]即y
	Point(){};
	Point(int x,int y,int vall){dim[0]=x,dim[1]=y,val=vall;}
};
Point order[N]; int cnt;    //替罪羊树：用于拍平后存数据
struct kd_tree{
	int ls,rs;
	int mi[2],ma[2]; //mi[i]: 第i维上区间的下界; ma[i]:第i维上区间的上界
	int sum;         //以该点为根的子树权值之和
	int size;
	Point p;
}t[N];
int tot,root;
int top,tree_stack[N];    //替罪羊树：回收
int now;
bool cmp(Point a,Point b){return a.dim[now]<b.dim[now];}
void update(int u){
	for(int i=0;i<2;i++){
		t[u].mi[i] = t[u].ma[i] = t[u].p.dim[i];
		if(lc){
			t[u].mi[i] = min(t[u].mi[i],t[lc].mi[i]);
			t[u].ma[i] = max(t[u].ma[i],t[lc].ma[i]);
		}
		if(rc){
			t[u].mi[i] = min(t[u].mi[i],t[rc].mi[i]);
			t[u].ma[i] = max(t[u].ma[i],t[rc].ma[i]);
		}
	}
	t[u].sum = t[lc].sum + t[u].p.val+t[rc].sum;
	t[u].size = t[lc].size + t[rc].size+1;
}
void slap(int u)   {            //替罪羊树：拍平
	if(!u) return;
	slap(lc);                   //这里用中序遍历。其实先序、后序也行
	order[++cnt] = t[u].p;
	tree_stack[++top] = u;      //回收结点
	slap(rc);
}

int build(int l,int r,int d)
{   //替罪羊树：建树
    if(l>r) return 0;
    int u;
    if(top)   u = tree_stack[top--];
        else      u = ++tot;
    int mid=(l+r)>>1;
        now = d;
    nth_element(order+l, order+mid, order+r+1, cmp);
    t[u].p = order[mid];
    lc = build(l,mid-1,d^1);    //奇偶轮转法。没有用例题hdu2966的一般轮转法
    rc = build(mid+1,r,d^1);
    update(u);
    return u;
}

bool notbalance(int u){          //替罪羊树：判断子树u是否平衡
    if(t[lc].size>alpha*t[u].size || t[rc].size>alpha*t[u].size)
        return true;             //不平衡了
    return false;                //还是平衡的
}
void Insert(int &u,Point now,int d){
	if(!u)	{
		if(top)   u=tree_stack[top--];
		else      u = ++tot;
		lc = rc = 0,t[u].p = now;
		update(u);
		return;
	}
	if(now.dim[d] <= t[u].p.dim[d])  Insert(lc,now,d^1);//按第d维的坐标比较
	else                             Insert(rc,now,d^1);
	update(u);
	if(notbalance(u)){                //不平衡
        cnt = 0;
		slap(u);                     //拍平
		u = build(1,t[u].size,d);    //重建
	}
}
int query(int u,int x1,int y1,int x2,int y2){
	if(!u) return 0;
	int X1=t[u].mi[0], Y1=t[u].mi[1], X2=t[u].ma[0], Y2=t[u].ma[1];
    if(x1<=X1 && x2>=X2 && y1<=Y1 && y2>=Y2)   return t[u].sum;
    //子树表示的矩形完全在询问矩形范围内
	if(x1>X2 || x2<X1 || y1>Y2 || y2<Y1)	 return 0;
    //子树表示的矩形完全在询问矩形范围外
	int ans=0;
	X1=t[u].p.dim[0], Y1=t[u].p.dim[1], X2=t[u].p.dim[0], Y2=t[u].p.dim[1];
    if(x1<=X1 && x2>=X2 && y1<=Y1 && y2>=Y2)  ans+=t[u].p.val;//根在询问矩形内
	ans += query(lc,x1,y1,x2,y2) + query(rc,x1,y1,x2,y2);     //递归左右子树
	return ans;
}
int main(){
	int n; cin >> n;
	int ans=0;
	while(1){
        int opt;scanf("%d",&opt);
		if(opt==1){
			int x,y,val; scanf("%d%d%d",&x,&y,&val);
			x^=ans,y^=ans,val^=ans;
			Insert(root,Point(x,y,val),0);
		}
		if(opt==2){
			int x1,y1,x2,y2; scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
			x1^=ans,y1^=ans,x2^=ans,y2^=ans;
            ans = query(root,x1,y1,x2,y2);
			printf("%d\n",ans);
		}
		if(opt==3) break;
	}
}
```

#### 4.18 动态树与LCT


**4.18.5 模板题**

例 4-48. 动态树 洛谷 P3690

```cpp
//代码改写自：https://www.luogu.com.cn/blog/ecnerwaIa/dai-ma-jiang-xie
#include<bits/stdc++.h>
using namespace std;
const int N=3e5+5;
struct node{ int fa,ch[2],sum,val,lazy; }t[N];     //lazy用来标记reverse()的左右翻转
#define lc t[x].ch[0]    //左儿子
#define rc t[x].ch[1]    //右儿子
bool isRoot(int x){      //判断是否是splay根节点
    int g=t[x].fa;
    return t[g].ch[0]!=x && t[g].ch[1]!=x;//若为根,则父结点不应该有这个儿子
}
void pushup(int x){      //本题的求路径异或和。上传信息
    t[x].sum=t[x].val^t[lc].sum^t[rc].sum;
}
void reverse(int x){
    if(!x)return;
    swap(lc,rc);         //翻转x的左右儿子
    t[x].lazy^=1;        //懒惰标记，先不翻转儿子的后代，后面再翻转
}
void pushdown(int x){    //递归翻转x的儿子的后代，并释放懒标记。
    if(t[x].lazy){
        reverse(lc);
        reverse(rc);
        t[x].lazy=0;
    }
}
void push(int x){
    if(!isRoot(x))  push(t[x].fa);  //从根到x全部pushdown
    pushdown(x);
}
void rotate(int x){
    int y=t[x].fa;
    int z=t[y].fa;
    int k=t[y].ch[1]==x;
    if(!isRoot(y)) t[z].ch[t[z].ch[1]==y]=x;
    t[x].fa=z;
    t[y].ch[k]=t[x].ch[k^1];
    if(t[x].ch[k^1])t[t[x].ch[k^1]].fa=y;
    t[y].fa=x;
    t[x].ch[k^1]=y;
    pushup(y);
}
void splay(int x){     //提根：把x旋转为它所在的Splay树的根
    int y,z;
    push(x);           //先pushdown处理x的所有子孙的lazy标记
    while(!isRoot(x)){
        y=t[x].fa,z=t[y].fa;
        if(!isRoot(y))
            (t[z].ch[0]==y)^(t[y].ch[0]==x)?rotate(x):rotate(y);
        rotate(x);
    }
    pushup(x);
}
void access(int x){    //在原树上建一条实链，起点是根，终点是x
    for(int child=0; x; child=x, x=t[x].fa){  //从x往上走，沿着虚边走到根
        splay(x);
        rc = child;     //右孩子是child，建立了一条实边
        pushup(x);
    }
}
void makeroot(int x){  //把x在原树上旋转到根的位置
    access(x);    splay(x);     reverse(x);
}
void split(int x,int y){   //把原树上以x为起点、y为终点的路径，生成一条实链
    makeroot(x);
    access(y);
    splay(y);
}
void link(int x,int y){   //在结点x和y之间连接一条边
    makeroot(x);    t[x].fa=y;
}
void cut(int x,int y){  //将x,y的边切断
    split(x,y);
    if(t[y].ch[0]!=x||rc)  return;
    t[x].fa=t[y].ch[0]=0;
    pushup(x);
}
int findroot(int x){  //查找x在原树上的根
	access(x);  splay(x);
	while(lc)  pushdown(x),x=lc;    //找Splay树最左端的结点
	return x;
}
int main(){
    int n,m; scanf("%d%d",&n,&m);
    for(int i=1;i<=n;++i){ scanf("%d",&t[i].val); t[i].sum = t[i].val; }
    while(m--){
        int opt,a,b;  scanf("%d%d%d",&opt,&a,&b);
        switch(opt){
            case 0:  split(a,b);   printf("%d\n",t[b].sum);  break;
            case 1:  if(findroot(a) != findroot(b))link(a,b);  break;
            case 2:  cut(a,b);   break;
            case 3:  splay(a);   t[a].val=b;   break;
        }
    }
    return 0;
}
```

**4.18.6 LCT 的基本应用**

```cpp
//洛谷P3379的部分代码
const int N=500000+5;
int query_lca(int x, int y)
{ // 求LCA(x, y)
    access(x);
    int ans;
    for (int child = 0; y; child = y, y = t[y].fa)
    {             // 模拟access(), y==0时退出
        splay(y); // 若y在从根出发的路径p上，splay(y)后y是Splay的根，y没有父结点，t[y].fa=0
        t[y].ch[1] = child;
        ans = y;
    }
    return ans;
}
int main()
{
    int n, m, rt;
    scanf("%d%d%d", &n, &m, &rt);
    for (int i = 1; i < n; ++i)
    {
        int x, y;
        scanf("%d%d", &x, &y);
        link(x, y);
    }
    makeroot(rt); // 定义根结点是rt
    for (int i = 1; i <= m; ++i)
    {
        int x, y;
        scanf("%d%d", &x, &y);
        printf("%d\n", query_lca(x, y));
    }
    return 0;
}
```


### 第5章 动态规划

#### 5.1 DP概念和编码方法

**5.1.3 DP的设计和实现**

例5-1.  Bone Collector  hdu 2602 

（1）递推代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1011;
int w[N], c[N];      // 物品的价值和体积
int dp[N][N];
int solve(int n, int C)
{
    for (int i = 1; i <= n; i++)
        for (int j = 0; j <= C; j++)
        {
            if (c[i] > j)
                dp[i][j] = dp[i - 1][j]; // 第i个物品比背包还大，装不了
            else
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - c[i]] + w[i]); // 第i个物品能装
        }
    return dp[n][C];
}
int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        int n, C;
        cin >> n >> C;
        for (int i = 1; i <= n; i++)
            cin >> w[i];
        for (int i = 1; i <= n; i++)
            cin >> c[i];
        memset(dp, 0, sizeof(dp)); // 清0，置初值为0
        cout << solve(n, C) << endl;
    }
    return 0;
}
```

（2）记忆化代码

```cpp 
int solve(int i, int j)
{ // 前i个物品，放进容量j的背包
    if (dp[i][j] != 0)
        return dp[i][j]; // 记忆化
    if (i == 0)
        return 0;
    int res;
    if (c[i] > j)
        res = solve(i - 1, j); // 第i个物品比背包还大，装不了
    else
        res = max(solve(i - 1, j), solve(i - 1, j - c[i]) + w[i]); // 第i个物品可以装
    return dp[i][j] = res;
}
```

**5.1.4 滚动数组**

1. 交替滚动

```cpp
// hdu 2602（滚动数组代码1）
int dp[2][N];                       //替换 int dp[][];
int solve(int n, int C){
	int now = 0, old = 1;           //now指向当前正在计算的一行，old指向旧的一行
	for(int i=1; i<=n; i++){
	    swap(old,now);              //交替滚动。now始终指向最新的一行
	    for(int j = 0; j <= C; j++){ 
             if(c[i] > j)  dp[now][j] = dp[old][j];
             else          dp[now][j] = max(dp[old][j], dp[old][j-c[i]]+w[i]);
	    }		
    }
    return dp[now][C];              //返回最新的行
}
```

2. 自我滚动

```cpp
// hdu 2602（滚动数组代码2）
int dp[N];                          
int solve(int n, int C){
    for(int i=1; i<=n; i++)
        for(int j=C; j>=c[i]; j--)       //反过来循环
            dp[j] = max(dp[j],dp[j-c[i]]+w[i]);
    return dp[C];
}
```

#### 5.2 经典线性DP问题

例5-2.  ACboy needs your help  hdu 1712

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=105;
int w[N][N],c[N][N];  //物品的价值、体积
int dp[N];
int n,m;
int main(){
    while(scanf("%d%d",&n,&m) && n && m){   //输入：n门课=n组，m天=容量m
        for(int i=1;i<=n;i++)               //第i门课，即第i组
            for(int k=1;k<=m;k++){          //m天=容量m；在这题中，m也是第i组的物品个数
                scanf("%d",&w[i][k]);       //第i组第k个物品的价值
                c[i][k] = k;            //第i组第k个物品的体积。学k天才能得分，体积就是k
            }
        memset(dp,0,sizeof(dp));
        for(int i=1;i<=n;i++)               //n门课=n组。遍历每门课，即遍历每个组
            for(int j=m;j>=0;j--)           //容量=m
                for(int k=1;k<=m;k++)       //用k遍历第i组的所有物品
                    if(j >= c[i][k])        //第k个物品能装进容量j的背包
                       dp[j] = max(dp[j], dp[j-c[i][k]] + w[i][k]);//第i组第k个
        printf("%d\n",dp[m]);
    }
}
```

2. 多重背包
	
例5-3.  宝物筛选  洛谷 P1776 

这里给出3种解法。	

1）简单方法

```cpp
//洛谷 P1776：滚动数组版本的多重背包（超时TLE）
#include <bits/stdc++.h>
using namespace std;
const int N=100010;
int n, C, dp[N];
int w[N], c[N], m[N]; // 物品i的价值、体积、数量
int main()
{
    cin >> n >> C; // 物品数量，背包容量
    for (int i = 1; i <= n; i++)
        cin >> w[i] >> c[i] >> m[i];
    // 以下是滚动数组版本的多重背包
    for (int i = 1; i <= n; i++)        // 枚举物品
        for (int j = C; j >= c[i]; j--) // 枚举背包容量
            for (int k = 1; k <= m[i] && k * c[i] <= j; k++)
                dp[j] = max(dp[j], dp[j - k * c[i]] + k * w[i]);
    cout << dp[C] << endl;
    return 0;
}


``` 

2）“二进制拆分”优化

```cpp
//洛谷 P1776：二进制拆分+滚动数组
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
int n, C, dp[N];
int w[N], c[N], m[N];
int new_n;                        // 二进制拆分后的新物品总数量
int new_w[N], new_c[N], new_m[N]; // 二进制拆分后新物品
int main()
{
    cin >> n >> C;
    for (int i = 1; i <= n; i++)
        cin >> w[i] >> c[i] >> m[i];
    // 以下是二进制拆分
    int new_n = 0;
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m[i]; j <<= 1)
        {                              // 二进制枚举：1,2,4...
            m[i] -= j;                 // 减去已拆分的
            new_c[++new_n] = j * c[i]; // 新物品
            new_w[new_n] = j * w[i];
        }
        if (m[i])
        { // 最后一个是余数
            new_c[++new_n] = m[i] * c[i];
            new_w[new_n] = m[i] * w[i];
        }
    }
    // 以下是滚动数组版本的0/1背包
    for (int i = 1; i <= new_n; i++)        // 枚举物品
        for (int j = C; j >= new_c[i]; j--) // 枚举背包容量
            dp[j] = max(dp[j], dp[j - new_c[i]] + new_w[i]);
    cout << dp[C] << endl;
    return 0;
}
```
	
#### 5.3 数位统计DP

例5-4.  数字计数 洛谷P2602，poj 2282

**5.3.1 数位统计DP的递推实现**

```cpp
//改写自：https://www.luogu.com.cn/blog/mak2333/solution-p2602
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=15;
ll ten[N],dp[N];
ll cnta[N],cntb[N];             //cnt[i]，统计数字“i”出现了多少次
int num[N];
void init()
{               // 预计算dp[]
    ten[0] = 1; // ten[i]: 10的i次方
    for (int i = 1; i <= N; i++)
    {
        dp[i] = i * ten[i - 1]; // 或者写成：dp[i]  = dp[i-1]*10+ten[i-1];
        ten[i] = 10 * ten[i - 1];
    }
}
void solve(ll x, ll *cnt)
{
    int len = 0; // 数字x有多少位
    while (x)
    { // 分解x，num[i]是x的第i位数字
        num[++len] = x % 10;
        x = x / 10;
    }
    for (int i = len; i >= 1; i--)
    { // 从高到低处理x的每一位
        for (int j = 0; j <= 9; j++)
            cnt[j] += dp[i - 1] * num[i];
        for (int j = 0; j < num[i]; j++)
            cnt[j] += ten[i - 1]; // 特判最高位比num[i]小的数字
        ll num2 = 0;
        for (int j = i - 1; j >= 1; j--)
            num2 = num2 * 10 + num[j];
        cnt[num[i]] += num2 + 1; // 特判最高位的数字num[i]
        cnt[0] -= ten[i - 1];    // 特判前导0
    }
}
int main()
{
    init();
    ll a, b;
    cin >> a >> b;
    solve(a - 1, cnta), solve(b, cntb);
    for (int i = 0; i <= 9; i++)
        cout << cntb[i] - cnta[i] << " ";
}
```

**5.3.2 数位统计DP的记忆化搜索实现**

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 15;
ll dp[N][N];
int num[N],now;                 //now：当前统计0~9的哪一个数字
ll dfs(int pos, int sum, bool lead, bool limit)
{ // pos：当前处理到第pos位
    ll ans = 0;
    if (pos == 0)
        return sum; // 递归到0位数，结束，返回
    if (!lead && !limit && dp[pos][sum] != -1)
        return dp[pos][sum];         // 记忆化搜索
    int up = (limit ? num[pos] : 9); // 这一位的最大值，例如324的第3位是up = 3
    for (int i = 0; i <= up; i++)
    { // 下面以now=2为例：
        if (i == 0 && lead)
            ans += dfs(pos - 1, sum, true, limit && i == up); // 计算000~099
        else if (i == now)
            ans += dfs(pos - 1, sum + 1, false, limit && i == up); // 计算200~299
        else if (i != now)
            ans += dfs(pos - 1, sum, false, limit && i == up); // 计算100~199
    }
    if (!lead && !limit)
        dp[pos][sum] = ans; // 状态记录：无前导0
    return ans;
}
ll solve(ll x)
{
    int len = 0; // 数字x有多少位
    while (x)
    {
        num[++len] = x % 10;
        x /= 10;
    }
    memset(dp, -1, sizeof(dp));
    return dfs(len, 0, true, true);
}
int main()
{
    ll a, b;
    cin >> a >> b;
    for (int i = 0; i < 10; i++)
        now = i, cout << solve(b) - solve(a - 1) << " ";
    return 0;
}
```

**5.3.3 数位统计DP例题**

例5-5. Windy数 洛谷P2657

```cpp
#include<bits/stdc++.h>
using namespace std;
int dp[15][15], num[15];
int dfs(int pos, int last, bool lead, bool limit){
    int ans = 0;
    if(pos == 0) return 1;
    if(!lead && !limit && dp[pos][last]!=-1) return dp[pos][last];
    int up = (limit?num[pos]:9);
    for(int i=0;i<=up;i++){
        if(abs(i-last)<2)  continue;   //不是windy数
        if(lead && i==0)   ans += dfs(pos-1,-2,true, limit&&i==up);
        else               ans += dfs(pos-1,i,false, limit&&i==up);
    }
    if(!limit && !lead)   dp[pos][last] = ans;
    return ans;
}
int solve(int x){
    int len = 0;
    while(x) { num[++len]=x%10; x/=10;}
    memset(dp,-1,sizeof(dp));
    return dfs(len,-2,true,true);   
}
int main(){
    int a,b; cin >>a>>b;
    cout << solve(b)-solve(a-1);
    return 0;
}
```

例5-6.  手机号码 洛谷P4124
	
 
```cpp
//改写自：www.luogu.com.cn/blog/yushuotong-std/solution--4124
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll dp[15][11][11][2][2][2];
int num[15];
ll dfs(int pos, int u, int v, bool state, bool n8, bool n4, bool limit)
{
    ll ans = 0;
    if (n8 && n4)
        return 0; // 8和4不能同时出现
    if (!pos)
        return state;
    if (!limit && dp[pos][u][v][state][n8][n4] != -1)
        return dp[pos][u][v][state][n8][n4];
    int up = (limit ? num[pos] : 9);
    for (int i = 0; i <= up; i++)
        ans += dfs(pos - 1, i, u, state || (i == u && i == v), n8 || (i == 8), n4 || (i == 4), limit && (i == up));
    if (!limit)
        dp[pos][u][v][state][n8][n4] = ans;
    return ans;
}
ll solve(ll x)
{
    int len = 0;
    while (x)
    {
        num[++len] = x % 10;
        x /= 10;
    }
    if (len != 11)
        return 0;
    memset(&dp, -1, sizeof(dp));
    ll ans = 0;
    for (int i = 1; i <= num[len]; i++) // 最高位1~9，避开前导0问题
        ans += dfs(len - 1, i, 0, 0, i == 8, i == 4, i == num[len]);
    return ans;
}
int main()
{
    ll a, b;
    cin >> a >> b;
    cout << solve(b) - solve(a - 1);
}
```

#### 5.4 状态压缩DP

**5.4.1 引子**

例5-7.  最短Hamilton路径   https://www.acwing.com/problem/content/description/93/

```cpp
#include <bits/stdc++.h>
using namespace std;
int n, dp[1<<20][21];
int dist[21][21];
int main(){
    memset(dp,0x3f,sizeof(dp));    //初始化最大值
    cin>>n;
    for(int i=0; i<n; i++)         //输入图 
        for(int j=0; j<n; j++)
            cin >> dist[i][j];     //输入点之间的距离
    dp[1][0]=0;                    //开始：集合中只有点0，起点和终点都是0
    for(int S=1; S<(1<<n); S++)    //从小集合扩展到大集合，集合用S的二进制表示
        for(int j=0; j<n; j++)     //枚举点j
            if((S>>j) & 1)         //(1): 这个判断与下面的(2)一起起作用
                for(int k=0; k<n; k++)        //枚举到达j的点k，k属于集合S-j
                    if((S^(1<<j)) >> k & 1)   //(2): k属于集合S-j。S-j用(1)保证
                  //把(1)和(2)写在一起，像下面这样，更容易理解，但是效率低一点：
                  //if( ((S>>j) & 1) && ((S^(1<<j)) >> k & 1) )
                         dp[S][j] = min(dp[S][j],dp[S^(1<<j)][k] + dist[k][j]);
    cout << dp[(1<<n)-1][n-1];         //输出：路径包含了所有的点，终点是n-1
    return 0;
}
```

**5.4.2 状态压缩DP的原理**

```cpp
#include<bits/stdc++.h>
int main(){
    int a = 213, b = 21;               // a = 1101 0101, b = 0001 0101
    printf("a & b = %d\n",a & b);      // AND  =  21, 二进制0001 0101
    printf("a | b = %d\n",a | b);      // OR   = 213, 二进制1101 0101
    printf("a ^ b = %d\n",a ^ b);      // XOR  = 192, 二进制1100 0000
    printf("a << 2 = %d\n",a << 2);    // a*4  = 852, 二进制0011 0101 0100
    printf("a >> 2 = %d\n",a >> 2);    // a/4  =  53, 二进制0011 0101
    int i = 5;                         //(1)a的第i位是否为1
    if((1 << (i-1)) & a)  printf("a[%d]=%d\n",i,1);  //a的第i位是1 
    else                  printf("a[%d]=%d\n",i,0);  //a的第i位是0
    a = 43, i = 5;                     //(2)把a的第i位改成1。a = 0010 1011
    printf("a=%d\n",a | (1<<(i-1)));   //a=59, 二进制0011 1011    
    a = 242;                           //(3)把a最后的1去掉。  a = 1111 0010
    printf("a=%d\n", a & (a-1));       //去掉最后的1。 =240, 二进制1111 0000
    return 0;    
}
```
	
**5.4.3 状态压缩DP例题**

例5-8.  Mondriaan's Dream  poj 2411

```cpp
#include <iostream>
#include <cstring>
using namespace std;
long long dp[2][1<<11];
int now,old;          //滚动数组，now指向新的一行，old指向旧的一行
int main(){
    int n,m;
    while( cin>>n>>m && n ){
        if(m>n)   swap(n,m);            //复杂度O(nm*2^m), m较小有利
        memset(dp,0,sizeof(dp));
        now=0,old=1;                    //滚动数组
        dp[now][(1<<m)-1]=1;            
        for(int i=0;i<n;i++)            //n行
            for(int j=0;j<m;j++){       //m列
                swap(now,old);          //滚动数组，now始终指向最新的一行
                memset(dp[now],0,sizeof(dp[now]));
                for(int k=0;k<(1<<m);k++){    //k：轮廓线上的m格
                    if(k & 1<<(m-1))          //情况(1)。要求k5=1
                       dp[now][(k<<1) & (~(1<<m))] += dp[old][k];
                               //原来的k5=1移到了第m+1位，置0
                    if(i && !(k & 1<<(m-1) ) )   //情况(2)
                               //i不等于0，即i不是第一行。另外要求k5=0
                       dp[now][(k<<1)^1] += dp[old][k];
                    if(j && (!(k&1)) && (k & 1<<(m-1)) ) //情况(3)
                               //j不等于0，即j不是第一列。另外要求k0=0, k5=1
                       dp[now][((k<<1) | 3) & (~(1<<m))] += dp[old][k];  
                               //k末尾置为11，且原来的k5移到了第m+1位，置0
                }
            }                    
        cout << dp[now][(1<<m)-1]<<endl;
    }
    return 0;
}
```


例5-9.  排兵布阵 hdu 4539

```cpp
//代码改写自：https://blog.csdn.net/jzmzy/article/details/20950205
#include <bits/stdc++.h>
using namespace std;
int mp[105][12];          //地图
int dp[105][200][200]; 
int n,m;
int sta[200];            //预计算一行的合法情况。m = 10时，只有169种合法情况
int init_line(int n){    //预计算出一行的合法情况
    int M = 0;
    for(int i = 0; i < n; i ++)
        if((i&(i>>2))==0 && (i&(i<<2))==0)//左右间隔2的位置没人，就是合法的
           sta[M++] = i;
    return M;                    //返回合法情况有多少种
}
int count_line(int i, int x){    //计算第i行的士兵数量
    int sum = 0;
    for(int j=m-1; j>=0; j--) {    //x是预计算过的合法安排
        if(x&1) sum += mp[i][j];   //把x与地形匹配
        x >>= 1;
    }
    return sum;
}
int main(){
    while(~scanf("%d%d",&n,&m)) {
        int M = init_line(1<<m);            //预计算一行的合法情况，有M种
        for(int i = 0; i < n; i ++)
            for(int j = 0; j < m; j ++)
                scanf("%d",&mp[i][j]);      //输入地图
        int ans = 0;
        memset(dp, 0, sizeof(dp));
        for(int i = 0; i < n; i ++)             //第i行
            for(int j = 0; j < M; j ++)         //枚举第i行的合法安排 
                for(int k = 0; k < M; k ++) {   //枚举第i-1行的合法安排   
                    if(i == 0) {                //计算第1行
                        dp[i][j][k] = count_line(i, sta[j]);
                        ans = max(ans, dp[i][j][k]);
                        continue;
                    }
                    if((sta[j]&(sta[k]>>1)) || (sta[j]&(sta[k]<<1)))                                             
                        continue;               //第i行和第i-1行冲突
                    int tmp = 0;
                    for(int p = 0; p < M; p ++){       //枚举第i-2行合法状态
                        if((sta[p]&(sta[k]>>1)) || (sta[p]&(sta[k]<<1))) continue;  
                                                       //第i-1行和第i-2行冲突
                        if(sta[j]&sta[p]) continue;    //第i行和第i-2行冲突
                        tmp = max(tmp, dp[i-1][k][p]); //从i-1递推到i
                    }
                    dp[i][j][k] = tmp + count_line(i, sta[j]); 
                                         //加上第i行的士兵数量
                    ans = max(ans, dp[i][j][k]);
                } 
        printf("%d\n",ans);
    }
    return 0;
}
```

**5.4.4 三进制状态压缩DP**

例5-10. hdu 3001

```cpp
#include<bits/stdc++.h>
const int INF = 0x3f3f3f3f;
using namespace std;
int n,m;
int bit[12]={0,1,3,9,27,81,243,729,2187,6561,19683,59049};
              //三进制每一位的权值。与二进制的0, 1, 2, 4, 8...对照理解
int tri[60000][11];
int dp[11][60000];    
int graph[11][11];                   //存图
void make_trb(){                     //初始化，求所有可能的路径
     for(int i=0;i<59050;++i){       //共3^10 = 59050种路径状态
         int t=i;
         for(int j=1; j<=10; ++j){ tri[i][j]=t%3;  t/=3; }
     }
}
int comp_dp(){   
        int ans = INF;
        memset(dp, INF, sizeof(dp));
        for(int j=0;j<=n;j++)
            dp[j][bit[j]]=0;            //初始化：从第j个城市出发，只访问j，费用为0
        for(int i=0;i<bit[n+1];i++){    //遍历所有路径，每个i是一个路径
            int flag=1;                 //所有的城市都遍历过1次以上
            for(int j=1;j<=n;j++){      //遍历城市，以j为起点
                if(tri[i][j] == 0){     //是否有一个城市访问次数是0
                    flag=0;             //还没有经过所有点
                    continue;
                }
                for(int k=1; k<=n; k++){    //遍历路径i-j的所有城市
                    int l=i-bit[j];         //l:从路径i中去掉第j个城市
                    dp[j][i]=min(dp[j][i],dp[k][l]+graph[k][j]);                                          
                }
            }
            if(flag)                        //找最小费用
               for(int j=1; j<=n; j++)
                   ans = min(ans,dp[j][i]);  //路径i上，最小的总费用
        }
        return ans;
}
int main(){
    make_trb();
    while(cin>>n>>m){
        memset(graph,INF,sizeof(graph));
        while(m--){
            int a,b,c;     cin>>a>>b>>c;
            if(c<graph[a][b])  graph[a][b]=graph[b][a]=c;
        }
        int ans = comp_dp();
        if(ans==INF) cout<<"-1"<<endl;
        else         cout<<ans<<endl;
    }
    return 0;
}
```

#### 5.5 区间DP

**5.5.2 区间DP例题 **

例5 - 12. String painter hdu 2476

```cpp
#include <bits/stdc++.h>
using namespace std;
char A[105], B[105];
int dp[105][105];
const int INF = 0x3f3f3f3f;
int main()
{
    while (~scanf("%s%s", A + 1, B + 1))
    {
        int n = strlen(A + 1); // 输入A, B
        for (int i = 1; i <= n; i++)
            dp[i][i] = 1; // 初始化
        // 先从空白串转换到B
        for (int len = 2; len <= n; len++)
            for (int i = 1; i <= n - len + 1; i++)
            {
                   int j = i + len - 1;
                   dp[i][j] = INF;
                   if (B[i] == B[j])         // 区间[i, j]两端的字符相同B[i] = B[j]
                    dp[i][j] = dp[i + 1][j]; // 或者 = dp[i][j-1])
                   else                      // 区间[i, j]两端的字符不同B[i] ≠ B[j]
                    for (int k = i; k < j; k++)
                        dp[i][j] = min(dp[i][j], dp[i][k] + dp[k + 1][j]);
            }
        // 下面从A转换到B
        for (int j = 1; j <= n; ++j)
        {
            if (A[j] == B[j])
                   dp[1][j] = dp[1][j - 1]; // 字符相同不用转
            else
                   for (int k = 1; k < j; ++k)
                    dp[1][j] = min(dp[1][j], dp[1][k] + dp[k + 1][j]);
        }
        printf("%d\n", dp[1][n]);
    }
    return 0;
}
```

**5.5.3 二维区间DP**

```cpp
例5-14.  CF1199 F. Rectangle Painting 1
//代码改写自：https://www.cnblogs.com/zsben991126/p/11643832.html
#include<bits/stdc++.h>
using namespace std;
#define N 55
int dp[N][N][N][N];
char mp[N][N];       //方格图
int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            cin >> mp[i][j];
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            if (mp[i][j] == '.')
                   dp[i][j][i][j] = 0; // 白格不用涂
            else
                   dp[i][j][i][j] = 1;    // 黑格(i,j)涂成白色需1次
    for (int lenx = 1; lenx <= n; lenx++) // len从1开始，不是2。因为有x和y两个方向
        for (int leny = 1; leny <= n; leny++)
            for (int x1 = 1; x1 <= n - lenx + 1; x1++)
                   for (int y1 = 1; y1 <= n - leny + 1; y1++)
                   {
                    int x2 = x1 + lenx - 1; // x1：x轴起点；x2：x轴终点
                    int y2 = y1 + leny - 1; // y1：y轴起点；y2：y轴终点
                    if (x1 == x2 && y1 == y2)
                        continue;                                             // lenx=1且leny=1的情况
                    dp[x1][y1][x2][y2] = max(abs(x1 - x2), abs(y1 - y2)) + 1; // 初始值
                    for (int k = x1; k < x2; k++)                             // 枚举x方向，y不变。区间[x1,k]+[k+1,x2]
                        dp[x1][y1][x2][y2] = min(dp[x1][y1][x2][y2], dp[x1][y1][k][y2] + dp[k + 1][y1][x2][y2]);
                    for (int k = y1; k < y2; k++) // 枚举y方向，x不变。区间[y1,k]+[k+1,y2]
                        dp[x1][y1][x2][y2] = min(dp[x1][y1][x2][y2], dp[x1][y1][x2][k] + dp[x1][k + 1][x2][y2]);
                   }
    cout << dp[1][1][n][n];
}
```

#### 5.6 树形DP

**5.6.1 树形DP的基本操作 **

二叉苹果树 洛谷P2015

```cpp
//洛谷P2015的代码（邻接表存树）
#include <bits/stdc++.h>
using namespace std;
const int N = 200;
struct node
{
    int v, w; // v是子结点，w是边[u,v]的值
    node(int v = 0, int w = 0) : v(v), w(w) {}
};
vector<node> edge[N];
int dp[N][N], sum[N]; // sum[i]记录以点i为根的子树的总边数
int n, q;
void dfs(int u, int father)
{
    for (int i = 0; i < edge[u].size(); i++)
    { // 用i遍历u的所有子结点
        int v = edge[u][i].v, w = edge[u][i].w;
        if (v == father)
            continue;         // 不回头搜父亲，避免循环
        dfs(v, u);            // 递归到最深的叶子结点，然后返回
        sum[u] += sum[v] + 1; // 子树上的总边数
        // for(int j=sum[u];j>=0;j--)
        //     for(int k=0;k<=j-1;k++)            //两个for优化为下面的代码。不优化也行
        for (int j = min(q, sum[u]); j >= 0; j--)
            for (int k = 0; k <= min(sum[v], j - 1); k++)
                dp[u][j] = max(dp[u][j], dp[u][j - k - 1] + dp[v][k] + w);
    }
}
int main()
{
    scanf("%d%d", &n, &q); // n个点，留q条树枝
    for (int i = 1; i < n; i++)
    {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        edge[u].push_back(node(v, w)); // 把边[u,v]存到u的邻接表中
        edge[v].push_back(node(u, w)); // 无向边
    }
    dfs(1, 0); // 从根结点开始做记忆化搜索
    printf("%d\n", dp[1][q]);
    return 0;
}
```
	
例5-15. 没有上司的舞会 洛谷P1352

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 6005;
int val[N], dp[N][2], father[N];
vector <int> G[N];
void addedge(int from,int to){
    G[from].push_back(to);             //用邻接表建树
    father[to] = from;                 //父子关系
}
void dfs(int u){
    dp[u][0] = 0;                      //赋初值：不参加宴会
    dp[u][1] = val[u];                 //赋初值：参加宴会
  //for(int i=0;i<G[u].size();i++){    //遍历u的邻居v。逐一处理这个父结点的每个子结点
  //    int v = G[u][i];
    for(int v : G[u]){                 //这一行和上面两行的作用一样
        dfs(v);                        //深搜子结点
        dp[u][1] += dp[v][0];          //父结点选择，子结点不选
        dp[u][0] += max(dp[v][0], dp[v][1]);     //父结点不选，子结点可选可不选
   }
}
int main(){
      int n; scanf("%d",&n);
      for(int i=1;i<=n;i++) scanf("%d",&val[i]);  //输入快乐指数
      for(int i=1;i<n;i++){
        int u,v;  scanf("%d%d",&u,&v);
        addedge(v,u);
      }
      int t = 1;
      while(father[t]) t = father[t];   //查找树的根结点
      dfs(t);                           //从根结点开始，用dfs遍历整棵树
      printf("%d\n", max(dp[t][0], dp[t][1]));
    return 0;
}
```

#### 5.7 一般优化
	
例5-17.  方伯伯的玉米田 洛谷P3287
	
```cpp
//代码改写自： www.luogu.com.cn/blog/361308/solution-p3287
#include<bits/stdc++.h>
using namespace std;
#define lowbit(x)  ((x) & - (x)) 
int a[10005], dp[10005][505], t[505][5505], n, k;
void update(int x, int y, int d) {     //更新区间
	for (int i=x; i <= k + 1; i += lowbit(i))
        for (int j=y; j <= 5500; j += lowbit(j))
            t[i][j] = max(t[i][j], d);
}
int query(int x, int y) {             //查区间最大值
	int ans=0;
	for (int i=x; i>0; i -= lowbit(i))
        for (int j=y; j>0; j -= lowbit(j))
            ans = max(ans, t[i][j]);
	return ans;
}
int main() {
	scanf("%d%d", &n, &k);
	for (int i=1; i <= n; i++) scanf("%d", &a[i]);
	for (int i=1; i <= n; i++)
        for (int j=k; j>=0; j--){
            dp[i][j] = query(j+1, a[i]+j) + 1;
            update(j+1, a[i]+j, dp[i][j]);
        }
	printf("%d", query(k+1, 5500));
	return 0;
}
```

#### 5.8 单调队列优化
	
**5.8.2 单调队列优化例题**

例5-18.  Mowing the Lawn 洛谷P2627

```cpp
//代码改写自：https://www.luogu.com.cn/blog/user21293/solution-p2627
#include <bits/stdc++.h>
using namespace std;
const int N=100005;
long long n,k,e[N],sum[N],dp[N];
long long ds[N];          //ds[j] = dp[j-1]-sum[j]
int q[N],head=0,tail=1;   //递减的单调队列，队头最大
long long que_max(int j){
    ds[j] = dp[j-1]-sum[j];
    while(head<=tail && ds[q[tail]]<ds[j])  tail--;    //去掉不合格的队尾        
    q[++tail]=j;                              //j进队尾
    while(head<=tail && q[head]<j-k)   head++;        //去掉超过窗口k的队头        
    return ds[q[head]];                    //返回队头，即最大的dp[j-1]-sum[j]
}
int main(){
    cin >> n >> k;   sum[0] = 0;
    for(int i=1;i<=n;i++){
        cin >> e[i];  sum[i] = sum[i-1] + e[i];   //计算前缀和
    }
    for(int i=1;i<=n;i++)   dp[i] = que_max(i) + sum[i];  //状态转移方程
    cout << dp[n];
}
```

```cpp
//洛谷 P1776：单调队列优化多重背包
#include<bits/stdc++.h>
using namespace std;
const int N=100010;
int n,C;
int dp[N],q[N],num[N];
int w,c,m;                      //物品的价值w、体积c、数量m
int main(){
    cin >> n >> C;              //物品数量n，背包容量W
    memset(dp,0,sizeof(dp));        
    for(int i=1;i<=n;i++){
        cin>>w>>c>>m;            //物品i的价值w、体积c、数量m
        if(m>C/c) m = C/c;       //计算 min{m, j/c}
        for(int b=0;b<c;b++){    //按余数b进行循环
            int head=1, tail=1;
            for(int y=0;y<=(C-b)/c;y++){      //y = j/c
                int tmp = dp[b+y*c]-y*w;      //用队列处理tmp = dp[b + xc] - xw
                while(head<tail && q[tail-1]<=tmp)   tail--;
                q[tail] = tmp;
                num[tail++] = y;
                while(head<tail && y-num[head]>m)    head++;
                //约束条件y-min(mi,y)≤x≤y
                dp[b+y*c] = max(dp[b+y*c],q[head]+y*w);  //计算新的dp[]
            }
        }
    }
    cout << dp[C] << endl;
    return 0;
}

```

#### 5.9 斜率优化/凸壳优化
	
**5.9.4 例题**

例5-19.  Print Article  hdu 3507 

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 500010;
int dp[N];   
int q[N];      //单调队列
int sum[N];
int X(int x){ return 2*sum[x]; }
int Y(int x){ return dp[x]+sum[x]*sum[x]; }
//double slope(int a,int b){return (Y(a)-Y(b))/(X(a)-X(b));}//除法不好，改成下面的乘法
int slope_up  (int a,int b) { return Y(a)-Y(b);}   //斜率的分子部分
int slope_down(int a,int b) { return X(a)-X(b);}   //斜率的分母部分
int main(){
    int n,m;
    while (~scanf("%d%d", &n, &m))
    {
        for (int i = 1; i <= n; i++)
            scanf("%d", &sum[i]);
        sum[0] = dp[0] = 0;
        for (int i = 1; i <= n; i++)
            sum[i] += sum[i - 1];
        int head = 1, tail = 1; // 队头队尾
        q[tail] = 0;
        for (int i = 1; i <= n; i++)
        {
            while (head < tail &&
                   slope_up(q[head + 1], q[head]) <= sum[i] * slope_down(q[head + 1], q[head]))
                head++;                                                // 斜率小于k，从队头弹走
            int j = q[head];                                           // 队头是最优点
            dp[i] = dp[j] + m + (sum[i] - sum[j]) * (sum[i] - sum[j]); // 计算dp[i]
            while (head < tail &&
                   slope_up(i, q[tail]) * slope_down(q[tail], q[tail - 1]) <= slope_up(q[tail], q[tail - 1]) * slope_down(i, q[tail]))
                tail--;    // 弹走队尾不合格的点
            q[++tail] = i; // 新的点进队尾
        }
        printf("%d\n", dp[n]);
    }
    return 0;
}
```
