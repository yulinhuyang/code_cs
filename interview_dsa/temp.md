#### 207. 课程表

```C++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>> &prerequisites) {
        //inDegree表 + 邻接表
        vector<int> inDeg(numCourses, 0);
        vector<vector<int>> edges;
        edges.resize(numCourses);
        for (auto course:prerequisites) {
            inDeg[course[0]]++;
            edges[course[1]].emplace_back(course[0]);
        }

        //bfs
        queue<int> workQueue;
        for (int i = 0; i < numCourses; i++) {
            if (inDeg[i] == 0) {
                workQueue.push(i);
            }
        }

        int visited = 0;
        while (!workQueue.empty()) {
            auto top = workQueue.front();
            workQueue.pop();
            visited++;
            for (auto edge:edges[top]) {
                inDeg[edge]--;
                if (inDeg[edge] == 0) {
                    workQueue.push(edge);
                }
            }
        }

        return numCourses == visited;
    }
};
```





