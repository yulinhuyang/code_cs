
##### 547. 省份数量

DFS和BFS的经典对比

DFS
```
class Solution {
public:
    void dfs(vector<vector<int>>& isConnected, vector<int>& visited, int provinces, int i) {
        for (int j = 0; j < provinces; j++) {
            if (isConnected[i][j] == 1 && !visited[j]) {
                visited[j] = 1;
                dfs(isConnected, visited, provinces, j);
            }
        }
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        int provinces = isConnected.size();
        vector<int> visited(provinces);
        int circles = 0;
        for (int i = 0; i < provinces; i++) {
            if (!visited[i]) {
                dfs(isConnected, visited, provinces, i);
                circles++;
            }
        }
        return circles;
    }
};

```
BFS : 最短路径问题，空间换时间

```
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int provinces = isConnected.size();
        vector<int> visited(provinces);
        int circles = 0;
        queue<int> Q;
        for (int i = 0; i < provinces; i++) {
            if (!visited[i]) {
                Q.push(i);
                while (!Q.empty()) {
                    int j = Q.front(); Q.pop();
                    visited[j] = 1;
                    for (int k = 0; k < provinces; k++) {
                        if (isConnected[j][k] == 1 && !visited[k]) {
                            Q.push(k);
                        }
                    }
                }
                circles++;
            }
        }
        return circles;
    }
};

```







