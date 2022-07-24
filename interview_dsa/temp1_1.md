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
