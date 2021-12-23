##### Offer 13. 机器人的运动范围

```C++
class Solution {
    int get(int x) {
        int res = 0;
        for (; x; x /= 10) {
            res += x % 10;
        }
        return res;
    }

public:
    int movingCount(int m, int n, int k) {
        if(!k){
            return  1;
        }
        queue<pair<int, int>> queue;
        vector<pair<int, int>> dirs = {{-1, 0},
                                       {1,  0},
                                       {0,  -1},
                                       {0,  1}};
        vector<vector<int>> visited(m,vector<int>(n,0));
        int ans = 1;
        queue.emplace(make_pair(0, 0));
        visited[0][0] = 1;
        while (!queue.empty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                auto pos = queue.front();
                queue.pop();
                for (auto dir:dirs) {
                    int newX = pos.first + dir.first;
                    int newY = pos.second + dir.second;
                    if (newX < 0 || newX > m - 1 || newY < 0 || newY > n - 1 || get(newX) + get(newY) > k || visited[newX][newY]) {
                        continue;
                    }
                    queue.emplace(make_pair(newX, newY));
                    visited[newX][newY] = 1;
                    ans += 1;
                }
            }
        }
        return ans;
    }
};


```
