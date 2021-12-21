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




