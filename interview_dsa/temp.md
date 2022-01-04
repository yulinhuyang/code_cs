#### 399 除法求值

BFS搜索变形

```cpp
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>> &equations, vector<double> &values, vector<vector<string>> &queries) {
        int nVars = 0;
        unordered_map<string, int> ids;
        for (int i = 0; i < equations.size(); i++) {
            if (ids.find(equations[i][0]) == ids.end()) {
                ids[equations[i][0]] = nVars++;
            }
            if (ids.find(equations[i][1]) == ids.end()) {
                ids[equations[i][1]] = nVars++;
            }
        }

        //id同时是索引
        vector<vector<pair<int,double>>> edges(nVars);
        for (int i = 0; i < equations.size(); i++) {
            auto va = ids[equations[i][0]];
            auto vb = ids[equations[i][1]];
            edges[va].emplace_back(make_pair(vb, values[i]));
            edges[vb].emplace_back(make_pair(va, 1.0 / values[i]));
        }

        vector<double> res;
        for (int i = 0; i < queries.size(); i++) {
            double result = -1.0;
            if (ids.find(queries[i][0]) != ids.end() && ids.find(queries[i][1]) != ids.end()) {
                auto va = ids[queries[i][0]];
                auto vb = ids[queries[i][1]];
                if (va == vb) {
                    result = 1.0;
                } else {
                    queue<int> points;
                    points.emplace(va);
                    //-1兼具visited和返回值作用
                    vector<double> ratios(nVars,-1);
                    ratios[va] = 1.0;

                    while (!points.empty() && ratios[vb] < 0) {
                        //size for省掉
                        auto top = points.front();
                        points.pop();
                        for (auto &[y, val] : edges[top]) {
                            if (ratios[y] < 0) {
                                ratios[y] = ratios[top] * val;
                                points.emplace(y);
                            }
                        }
                    }
                    result = ratios[vb];
                }
            }
            res.emplace_back(result);
        }
        return  res;
    }
};
```
