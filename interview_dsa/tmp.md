#### 338. 比特位计数

```C++
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> res;
        for (int i = 0; i <= n; i++) {
            int sum = 0;
            int num = i;
            while (num) {
                num = num & (num - 1);
                sum += 1;
            }
            res.emplace_back(sum);
        }
        return res;
    }
};
```


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

#### 字符串操作类
 
##### 71. 简化路径

std:find截取

```C++
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> dirs;
        for(auto i = path.begin();i != path.end();){
            ++i;
            auto j = find(i,path.end(),'/');
            auto dir = string(i,j);
            if(!dir.empty() && dir != "."){
                if(dir == ".."){
                    if(!dirs.empty()){
                        dirs.pop_back();
                    }
                } else if (dir != "."){
                    dirs.emplace_back(dir);
                }
            }
            i = j;
        }

        stringstream out;
        if(dirs.empty()){
            out<<'/';
        } else{
            for(auto ch:dirs){
                out << "/" << ch;
            }
        }
        return out.str();
    }
};

```
find + substr截取

```C++
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> dirs;
        for (int i = 0; i < path.size();) {
            ++i;
            auto j = path.find("/", i);
            if (j == string::npos) {
                j = path.size();
            }
            auto dir = path.substr(i, j - i);

            if (!dir.empty() && dir != ".") {
                if (dir == "..") {
                    if (!dirs.empty()) {
                        dirs.pop_back();
                    }
                } else if (dir != ".") {
                    dirs.emplace_back(dir);
                }
            }
            i = j;
        }

        stringstream out;
        if (dirs.empty()) {
            out << '/';
        } else {
            for (auto ch:dirs) {
                out << "/" << ch;
            }
        }
        return out.str();
    }
};

```
##### 1233. 删除子文件夹

```C++
class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        sort(folder.begin(),folder.end());
        vector<string> ans(1,folder[0]);
        string cur = ans[0] + "/";
        for(int i = 1;i < folder.size();){
            while(i < folder.size() && folder[i].find(cur) == 0){
                ++i;
            }
            if(i < folder.size()){
                ans.emplace_back(folder[i]);
                cur = folder[i] + "/";
                i++;
            }
        }

        return  ans;
    }
};
```
##### 435. 无重叠区间

```C++
class Solution {
    static bool cmp(vector<int> &a, vector<int> &b) {
        if (a[1] < b[1]) {
            return true;
        }
        return false;
    }

public:
    int eraseOverlapIntervals(vector<vector<int>> &intervals) {
        if (intervals.empty()) {
            return 0;
        }
        //sort(intervals.begin(),intervals.end(),cmp);
        sort(intervals.begin(), intervals.end(), [](auto &u, auto &v) {
            return u[1] < v[1];
        });

        auto right = intervals[0][1];
        int ans = 0;
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] < right) {
                ans++;
            } else {
                right = intervals[i][1];
            }
        }
        return ans;
    }
};
```
#### 452. 用最少数量的箭引爆气球

```C++
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(),points.end(),[](auto &u,auto &v){
            return u[1] < v[1];
        });
        int right = points[0][1];
        int ans = 0;
        for(int i = 1;i < points.size();i++){
            if(points[i][0] <= right){
                ans++;
            } else{
                right = points[i][1];
            }
        }
        return points.size() - ans;
    }
};
```
