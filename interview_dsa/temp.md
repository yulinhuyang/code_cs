
LRU cache: https://www.acwing.com/activity/content/code/content/405014/

   

一维前缀和   
S[i] = a[1] + a[2] + ... a[i]   
a[l] + ... + a[r] = S[r] - S[l - 1]   

二维前缀和   
S[i, j] = 第i行j列格子左上部分所有元素的和   
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：   

S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]   

一维差分   
给区间[l, r]中的每个数加上c：    

B[l] += c, B[r + 1] -= c  

二维差分   
给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：   

S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c   


这里A数组都是1开始的，时间从0开始的，因此需要减去1。

树状数组学习笔记：https://www.acwing.com/blog/content/80/

树状数组的下标从 1 开始计数

##### 1094. 拼车

差分 标记变化量 -->  求和最大值
 
```C++
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        int n = trips.size();
        vector<int> nums(1002,0);
        for(auto & trip:trips){
            int l = trip[1],r = trip[2];
            int c = trip[0];
            nums[l] += c;
            nums[r] -= c;
        }

        int sum = 0;
        for(int i = 0;i < 1002;i++){
            sum += nums[i];
            if(sum > capacity) return false;
        }
        return true;
    }
};
```

#### 1109. 航班预订统计

差分模板题，数组索引从1开始的，要转换。

```C++
class Solution {
public:
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
        vector<int> nums(n,0);
        for(auto & book:bookings){
            int l = book[0] - 1,r = book[1] - 1;
            int c = book[2];
            nums[l] += c;
            if(r + 1 < n) nums[r + 1] -= c;
        }

        for(int i = 1;i < n;i++){
            nums[i] += nums[i - 1];
        }
        return nums;
    }
};
```

##### 934. 最短的桥

先深度搜索，确度一座岛的边界,再广度搜索，查找路径。 DFS + 多源BFS最短路径。

```C++
class Solution {
    vector<vector<int>> g;
    vector<vector<int>> dist;
    queue<pair<int, int>> q;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    int m, n;
public:
    int shortestBridge(vector<vector<int>> &grid) {
        g = grid;
        m = g.size(), n = g[0].size();
        dist = vector<vector<int>>(m, (vector<int>(n, 1e4)));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (g[i][j] == 1) {
                    dfs(i, j);
                    return bfs();
                }
            }
        }
        return 0;
    }

    void dfs(int i, int j) {

        dist[i][j] = 0;
        q.emplace(make_pair(i, j));
        for (int k = 0; k < 4; k++) {
            int x = i + dx[k], y = j + dy[k];
            if (x >= 0 && x < m && y >= 0 && y < n && g[x][y] && dist[x][y]) {
                dfs(x, y);
            }
        }
    }

    int bfs() {
        while (q.size()) {
            auto top = q.front();
            q.pop();
            int i = top.first, j = top.second;
            for (int k = 0; k < 4; k++) {
                int x = i + dx[k], y = j + dy[k];
                if (x >= 0 && x < m && y >= 0 && y < n && dist[x][y] > dist[i][j] + 1) {
                    dist[x][y] = dist[i][j] + 1;
                    if (g[x][y]) {
                        return dist[i][j];
                    }
                    q.emplace(make_pair(x, y));
                }
            }
        }
        return 0;
    }
};

```


