##### 剑指 Offer 03. 数组中重复的数字

```c++
map/set计数法

class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        set<int> numSet;
        for(auto num:nums){
            if(numSet.count(num)){
                return num;
            } else{
                numSet.emplace(num);
            }
        }
        return 0;
    }
};

反复交换法：

class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int i = 0;
        while(i < nums.size()){
            if(nums[i] == i){
                i++;
                continue;
            }
            if(nums[nums[i]] == nums[i]){
                return nums[i];
            }
            swap(nums[nums[i]],nums[i]);
        }
        return 0;
    }
};
```

##### Offer 04. 二维数组中的查找

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.size() == 0){
            return 0;
        }
        int m = matrix.size();
        int n = matrix[0].size();
        int i = 0,j = n - 1;
        while(i <= m - 1  && j >= 0){
            if(matrix[i][j] == target){
                return  true;
            }else if(matrix[i][j] < target){
                i++;
            }else{
                j--;
            }
        }
        return  false;
    }
};
```

##### Offer 05 替换空格

```C++
class Solution {
public:
    string replaceSpace(string s) {
        string output;
        for(auto chr:s){
            if(chr == ' '){
                output += "%20";
            } else{
                output += string(1,chr);
            }
        }
        return output;
    }
};

```

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
