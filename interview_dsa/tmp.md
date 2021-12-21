##### 347. 前 K 个高频元素

```C++
class Solution {
public:
    static bool cmp(pair<int, int> &a, pair<int, int> &b) {
        return a.second > b.second;
    }

    vector<int> topKFrequent(vector<int> &nums, int k) {
        map<int, int> occurrences;
        for (auto &v:nums) {
            occurrences[v]++;
        }
        //自定义排序
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(&cmp)> q(cmp);
        for (auto &[num, count] :occurrences) {
            if (q.size() == k) {
                if (q.top().second < count) {
                    q.pop();
                    q.emplace(num, count);
                }
            } else {
                q.emplace(num, count);
            }
        }

        vector<int> res;
        while(!q.empty()){
            res.emplace_back(q.top().first);
            q.pop();
        }
        return res;
    }
};



```
