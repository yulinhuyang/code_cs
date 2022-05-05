##### Leetcode 220  存在重复元素 III

滑窗 + 有序集合二分: [x − t,x + t]

```C++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int> &nums, int k, int t) {
        set<long> st;
        for (int i = 0; i < nums.size(); i++) {
            auto index = st.lower_bound(long(nums[i]) - t);
            //set存在一个元素a，现在又有相同的元素a，第二次插入元素a之前，由于a - a = 0,return true,因此可以不使用multiset
            if (index != st.end() && *index <= long(nums[i]) + t) return true;
            st.emplace(nums[i]);
            if (i >= k) {
                st.erase(nums[i - k]);
            }
        }
        return false;
    }
};
```

桶排序

空间换取时间的排序: https://www.acwing.com/blog/content/8989/

遍历原始序列确定最大值maxval和最小值minval，并确定桶的个数 n;        
然后将待排序集合中处于同一个值域的元素存入同一个桶中,在桶内使用各种现有的算法进行排序;       
最后按照从小到大的顺序依次收集桶中的每一个元素为最终结果。      

```C++
//收集,将元素入相应的桶中. 减偏移量是为了将元素映射到更小的区间内,省内存 
for(int i = 0; i < n; i ++) bucket[(a[i] - minval) / bnum].push_back(a[i]);
//将桶内元素排序 
for(int i = 0; i < m; i ++) sort(bucket.begin(), bucket.end());
```

```C++
//桶排序
#define LL long long

class Solution {
    LL size = 0;
public:
    bool containsNearbyAlmostDuplicate(vector<int> &nums, int k, int t) {
        unordered_map<LL, LL> bucket; // 桶 + 值
        for (int i = 0; i < nums.size(); i++) {
            LL num = nums[i] * 1L;
            size = t + 1LL;
            LL idx = getIdx(num);
            //同一个桶内已经有值了，则返回
            if (bucket.find(idx) != bucket.end()) return true;
            LL l = idx - 1, r = idx + 1;
            if (bucket.find(l) != bucket.end() && (num - bucket[l]) <= t) return true;
            if (bucket.find(r) != bucket.end() && (bucket[r] - num) <= t) return true;
            bucket.emplace(idx, num);
            if (i >= k) {
                bucket.erase(getIdx(nums[i - k] * 1L));
            }
        }
        return false;
    }

    LL getIdx(LL num) {
        return num >= 0 ? num / size : (num + 1) / size - 1;
    }
};

```


875   567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
