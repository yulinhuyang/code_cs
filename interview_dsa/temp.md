##### Leetcode 380  O(1) 时间插入、删除和获取随机元素

变长数组+ hash, back交换法删除法

```C++
class RandomizedSet {
    vector<int> nums;
    unordered_map<int, int> hash;
public:
    RandomizedSet() {
        nums.clear();
        hash.clear();
    }

    bool insert(int val) {
        if (hash.find(val) != hash.end()) return false;
        nums.emplace_back(val);
        hash[val] = nums.size() - 1;
        return true;
    }

    bool remove(int val) {
        if (hash.find(val) == hash.end()) return false;
        int last = nums.back();
        int index = hash[val];
        //last替换index
        nums[index] = last;
        hash[last] = index;
        //删除last
        nums.pop_back();
        hash.erase(val);
        return true;
    }

    int getRandom() {
        return nums[rand() % nums.size()];
    }
};

```

leetcode 设计题 数据结构设计整理: https://www.acwing.com/blog/content/3778/

##### Leetcode 539 最小时间差

```C++
class Solution {
    int getMinute(string time) {
        int minute = ((time[0] - '0') * 10 + (time[1] - '0')) * 60 + (time[3] - '0') * 10 + (time[4] - '0');
        return minute;
    }

public:
    int findMinDifference(vector<string> &timePoints) {
        if (timePoints.size() > 1440) return 0;  //抽屉原理,24x60
        sort(timePoints.begin(), timePoints.end());
        int t0Minute = getMinute(timePoints[0]);
        int preMinute = t0Minute;
        int minGap = 1440;
        for (int i = 1; i < timePoints.size(); i++) {
            int minute = getMinute(timePoints[i]);
            if ((abs(minute - preMinute)) < minGap) minGap = abs(minute - preMinute);
            preMinute = minute;
        }

        minGap = min(minGap, t0Minute - preMinute + 1440);
        return minGap;
    }
};
```

##### Leetcode 735 行星碰撞

vector 当栈使用

```C++
class Solution {
public:
    vector<int> asteroidCollision(vector<int> &asteroids) {
        vector<int> res;//栈
        for (auto &x:asteroids) {
            if (x > 0) {
                res.emplace_back(x);
            } else {
                while (!res.empty() && res.back() > 0 && res.back() < -x) res.pop_back();
                if (!res.empty() && res.back() == -x) res.pop_back();
                else if (res.empty() || res.back() < 0) res.emplace_back(x);
            }
        }
        return res;
    }
};

```

##### 剑指 Offer II 041. 滑动窗口的平均值

队列

```C++
class MovingAverage {
    deque<int> q;
    int capacity;
    double sum = 0;
public:
    /** Initialize your data structure here. */
    MovingAverage(int size) {
        capacity = size;
    }

    double next(int val) {
        q.emplace_back(val);
        sum += val;
        if (q.size() > capacity) {
            sum -= q.front();
            q.pop_front();
        }
        return sum / q.size();
    }
};
```


875   567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
