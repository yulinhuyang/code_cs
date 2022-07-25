- AcWing340 通信线路  

https://www.acwing.com/solution/content/13645/

双端队列 + 二分  

求1到n路径上，从节点1到节点n的第k+1大的值的最小值。      
二分长度，搜索左边界，大于bound边权为1放在队尾，小于bound边权为0放在队头。首次出队被访问。  

```cpp
bool check(int bound) {

    memset(st, false, sizeof(st));
    memset(dist, 0x3f3f, sizeof(dist));
    deque<int> q;
    q.emplace_back(1);
    dist[1] = 0;
    while (!q.empty()) {
        auto t = q.front();
        q.pop_front();
        if (st[t]) continue;
        st[t] = true;

        for (int i = h[t]; ~i; i = ne[i]) {
            int j = e[i], x = w[i] > bound;
            if (dist[j] > dist[t] + x) {
                dist[j] = dist[t] + x;
                if (!x) q.push_front(j);
                else q.push_back(j);
            }
        }
    }

    return dist[n] <= k;
}
```


- AcWing341 最优贸易     

https://www.acwing.com/solution/content/3709/

原图 + 反图 + SPFA

Dijkstra ：基于贪心的，非负权，每次选择未被标记的最小的节点，边长非负，可以带环图。未标记的最小值不能被其他节点再更新。本题如果存在环路，最小值可以被更新。不能使用Dijkstra。      
Bellman Ford/SPFA都是基于动态规划，其原始的状态定义为f[i][k]代表从起点到i点，且经过最多k条边的最短路径。这样的状态定义引导我们能够使用 Bellman Ford 来解决有边数限制的最短路问题。     

dmin[i]: 1到i的过程中，买入水晶球的最低价格dmin[i];    
dmax[i]: i到n的过程中，卖出水晶球的最高价格dmax[i];    

先以1为起点，原图spfa,计算dmin[i];再以n为起点，在反图上使用spfa, 计算dmax[i]。

堆优化的dijkstra出队更新true,因为只会出一次;     
SPFA入队更新true,出队更新false，一个点可能进出多次，但队列中只能存在一个。    

一条路径上的最小值有继承关系    

```cpp
void spfa(int *d, int start, int *h, bool flag) {

    memset(st, false, sizeof(st));
    if (flag) memset(dMin, 0x3f3f, sizeof(dMin));

    queue<int> q;
    q.push(start);
    st[start] = true;
    d[start] = prices[start];

    while (q.size()) {
        auto t = q.front();
        q.pop();
        st[t] = false;

        for (int i = h[t]; ~i; i = ne[i]) {
            int j = e[i];
            //一条路径上的最值有继承关系
            if ((flag && d[j] > min(d[t], prices[j])) || (!flag && d[j] < max(d[t], prices[j]))) {
                if (flag) {
                    d[j] = min(d[t], prices[j]);
                }
                else d[j] = max(d[t], prices[j]);

                if (!st[j]) {
                    st[j] = true;
                    q.push(j);
                }
            }
        }
    }
}

spfa(dMin, 1, h, true);
spfa(dMax, n, rh, false);

```



- AcWing342 道路与航线  

https://www.acwing.com/solution/content/33202/   

dfs + 拓扑排序 + dijkstra(卡spfa)

把每个连通块整体看作一个点，再把单向边添加到图中，会得到一个有向无环图。     

1 dfs 出所有的连通块。id[]存储每个点属于哪个连通块。 vector<int> block[]存储每个连通块里有哪些点。          
2 输入航线，统计每个连通块的入度。     
3 对每个连通块进行拓扑排序,入度为0的连通块加入队列。        
4 对队列中的点，跑dijkstra算法。每次将当前入度为0的连通块的所有点进入heap, 然后heap更新距离，如果有非当前块的点且--该点的连通块入度为空，则加入队列。      


for (int i = e[u]; ~i; i = ne[i]) ： 这里的i实际是idx;  //更新模板

```cpp
#include <iostream>
#include <cstring>
#include <queue>
#include <vector>

using namespace std;

using PII = pair<int,int>;

const int N = 25010, M = 150010, INF = 0x3f3f3f3f;
int h[N], e[M], w[M], ne[M], idx;
int dist[N];
bool st[N];


int n,mr,mp,s;
int id[N];
vector<int> block[N];
int deg[N];
int bcnt;
queue<int> q;

void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

void dfs(int u, int bid) {
    id[u] = bid;
    block[bid].emplace_back(u);

    //这里i是idx
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (!id[j]) {
            dfs(j, bid);
        }
    }
}


void dijkstra(int block_id) {
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    for (int u: block[block_id]) {
        heap.push({dist[u], u});
    }

    while (heap.size()) {
        PII t = heap.top();
        heap.pop();
        int u = t.second;

        if (st[u]) continue;
        st[u] = true;
        
        //i 相当于idx,因此是e[i],w[i]
        for (int i = h[u]; ~i; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[u] + w[i]) {
                dist[j] = dist[u] + w[i];
                //本块内的入度为0,则加入heap
                if (id[j] == block_id) heap.push({dist[j], j});
            }

            if (id[j] != block_id && --deg[id[j]] == 0) q.push(id[j]); //连通块id加入队列
        }
    }
}

//连通块的拓扑排序
void topSort() {
    memset(dist, INF, sizeof(dist));
    dist[s] = 0;

    //入度为0的连通块入栈
    for (int i = 1; i <= bcnt; i++) {
        if (!deg[i]) {
            q.emplace(i);
        }
    }

    while (q.size()) {
        auto t = q.front();
        q.pop();
        dijkstra(t);
    }

}

int main() {
    cin >> n >> mr >> mp >> s;
    memset(h, -1, sizeof(h));

    //道路
    for (int i = 0; i < mr; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
        add(b, a, c);
    }

    //dfs得到所有连通块
    for (int i = 1; i <= n; i++) {
        if (!id[i]) {
            bcnt++;
            dfs(i, bcnt);
        }
    }

    //航线
    for (int i = 0; i < mp; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
        deg[id[b]]++;
    }

    //拓扑排序
    topSort();

    for (int i = 1; i <= n; i++) {
        if (dist[i] > INF / 2) puts("NO PATH");
        else printf("%d\n", dist[i]);
    }

    return 0;
}

```


- AcWing343  排序

https://www.acwing.com/solution/content/46279/

floyd:传递闭包,计算等式关系。

通过传递性推出尽可能多元素之间的关系

传递闭包模板题     
d[i][j] |= d[i][k] && d[k][j];     

```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 26;
int n,m;
bool g[N][N],d[N][N]; //g输入关系
bool st[N];

void floyd(){
    memcpy(d, g, sizeof(d));
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                d[i][j] |= d[i][k] && d[k][j];
            }
        }
    }
}

int check() {
    for (int i = 0; i < n; i++) {
        if (d[i][i]) return 2;  //存在矛盾
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (!d[i][j] && !d[j][i]) {
                return 0; // 不能确定
            }
        }
    }

    return 1; // 可以确定
}

char get_min() {
    for (int i = 0; i < n; i++) {
        if (!st[i]) {
            bool flag = true;
            for (int j = 0; j < n; j++) {
                if (!st[j] && d[j][i]) {
                    flag = false;
                    break;
                }
            }
            if (flag)
            {
                st[i] = true;
                return 'A' + i;
            }
        }
    }
    return 0;
}


int main() {
    while (cin >> n >> m, n || m) {
        memset(g, 0, sizeof(g));
        int type = 0, t;
        for (int i = 1; i <= m; i++) {
            char str[5];
            cin >> str;
            int a = str[0] - 'A', b = str[2] - 'A';

            if (!type) {
                g[a][b] = 1;
                floyd();
                type = check();
                if (type) t = i; //第一次发现的位置
            }
        }
        if (!type) puts("Sorted sequence cannot be determined.");
        else if (type == 2) printf("Inconsistency found after %d relations.\n", t);
        else {
            memset(st, 0, sizeof(st));
            printf("Sorted sequence determined after %d relations: ", t);
            for (int i = 0; i < n; i++) printf("%c", get_min());
            printf(".\n");
        }
    }

    return 0;
}
```



AcWing344 观光之旅

AcWing345 牛站

AcWing346 走廊泼水节


AcWing352 闇の連鎖


AcWing361 观光奶牛

AcWing362 区间


AcWing367 学校网络

AcWing368 银河

AcWing372 棋盘覆盖


AcWing376 机器任务

AcWing378 骑士放置














