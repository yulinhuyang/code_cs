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

