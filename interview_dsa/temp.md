

#####  Offer II 041 滑动窗口的平均值

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
##### Offer II 933 最近的请求次数

队列应用

```C++
class RecentCounter {
    deque<int> q;
public:
    RecentCounter() {
    }

    int ping(int t) {
        q.emplace_back(t);
        while (q.front() < t - 3000) q.pop_front();
        return q.size();
    }
};

```



875   567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
