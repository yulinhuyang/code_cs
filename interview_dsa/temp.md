##### Leetcode 220  存在重复元素 III

滑窗[x − t,x + t] + 有序集合二分(set 平衡树lower_bound)

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

#####  Leetcode 729  我的日程安排表 I

set 平衡树low_bound，处理区间Pair问题

Use std::pair as a key in std::set in C++: https://www.techiedelight.com/use-std_pair-as-key-std_set-cpp/

set, multiset, map, multimap：有序集合，内部采用的就是一种非常高效的平衡检索二叉树：红黑树。

```C++
using PII = pair<int, int>;

class MyCalendar {
    set<PII> calender;
public:
    MyCalendar() {
        calender.emplace(INT_MIN, INT_MIN);
        calender.emplace(INT_MAX, INT_MAX);
    }

    bool book(int start, int end) {
        auto i = calender.lower_bound({start, INT_MAX});
        auto j = i;
        j--;
        PII t{start, end};
        //*i 左边左端点最靠近start的区间，floorKey
        //*j 右边左端点最靠近start的区间，ceilingKey
        if (check(*i, t) || check(*j, t)) return false;
        calender.emplace(t);
        return true;
    }

    bool check(PII a, PII b) {
        if (a.second <= b.first || b.second <= a.first) return false;
        return true;
    }
};

```

#####  Leetcode 703 数据流中的第 K 大元素

TOP K问题 优先队列

```C++
class KthLargest {
    priority_queue<int, vector<int>, greater<int>> heap;
    int size = 0;
public:
    KthLargest(int k, vector<int> &nums) {
        size = k;
        for (auto &num:nums) {
            add(num);
        }
    }

    int add(int val) {
        heap.emplace(val);
        if (heap.size() > size) heap.pop();
        return heap.top();
    }
};

```

##### LeetCode 373  查找和最小的K对数字

pq + 贪心思想，类比acwing 146序列

pq自定义排序：https://blog.csdn.net/Strengthennn/article/details/119078911

自定义类型；lambda表达式(理解捕获 & 函数中的外部环境的变量的引用)

```C++
using PII = pair<int, int>;

class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int> &nums1, vector<int> &nums2, int k) {
        auto cmp = [&](PII &a, PII &b) {
            return nums1[a.first] + nums2[a.second] > nums1[b.first] + nums2[b.second];
        };
        vector<vector<int>> res;
        priority_queue<PII, vector<PII>, decltype(cmp)> heap(cmp);
        int m = nums1.size(), n = nums2.size();
        //a0 + bi
        for (int i = 0; i < min(n, k); i++) {
            heap.emplace(0, i);
        }
        while (k && !heap.empty()) {
            auto[v1, v2] = heap.top();
            heap.pop();
            res.push_back({nums1[v1], nums2[v2]});
            k--;
            if (v1 + 1 < m) {
                //ai + bi
                heap.emplace(v1 + 1, v2);
            }
        }
        return res;
    }
};
```
##### Leetcode 208 实现 Trie (前缀树)

【宫水三叶】一题双解 :「二维数组」&「TrieNode」:
   
https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/gong-shui-san-xie-yi-ti-shuang-jie-er-we-esm9/

https://www.acwing.com/solution/content/39521/


```C++
//TrieNode 模板
class Trie {
public:
    struct Node {
        bool is_end;
        Node *son[26];

        Node() {
            is_end = false;
            for (int i = 0; i < 26; i++) son[i] = nullptr;
        }
    } *root;

    Trie() {
        root = new Node();
    }

    void insert(string word) {
        Node* p = root;
        for (auto c : word) {
            int u = c - 'a';
            if (!p->son[u]) p->son[u] = new Node();
            p = p->son[u];
        }
        p->is_end = true;
    }

    bool search(string word) {
        Node* p = root;
        for (auto c : word) {
            int u = c - 'a';
            if (!p->son[u]) return false;
            p = p->son[u];
        }
        return p->is_end;
    }

    bool startsWith(string prefix) {
        Node* p = root;
        for (auto c : prefix) {
            int u = c - 'a';
            if (!p->son[u]) return false;
            p = p->son[u];
        }
        return true;
    }
};

```

```C++
//二维数组Tries模板
// 0号点既是根节点，又是空节点
// son[][]存储树中每个节点的子节点
class Trie {
private:
    const static int N = 40000;
    int son[N][26]{0};
    int idx = 0;
    bool is_end[N]{false};
public:
    Trie() {
    }

    void insert(string word) {
        int p = 0;
        for (auto c : word) {
            int u = c - 'a';
            if (!son[p][u]) son[p][u] = ++idx;
            p = son[p][u];
        }
        is_end[p] = true;
    }

    bool search(string word) {
        int p = 0;
        for (auto c : word) {
            int u = c - 'a';
            if (!son[p][u]) return false;
            p = son[p][u];
        }
        return is_end[p];
    }

    bool startsWith(string prefix) {
        int p = 0;
        for (auto c : prefix) {
            int u = c - 'a';
            if (!son[p][u]) return false;
            p = son[p][u];
        }
        return true;
    }
};
```
##### Leetcode 648 单词替换

TrieNode + Solution

```C++
struct TrieNode {
    int cnt;//cnt以trie_node结尾的单词数量
    TrieNode *son[26];

    TrieNode() {
        cnt = 0;
        for (int i = 0; i < 26; i++) son[i] = nullptr;
    }
};

class Trie {
public:
    TrieNode *root;

    Trie() {
        root = new TrieNode();
    }

    void insert(string word) {
        TrieNode *p = root;
        for (auto c : word) {
            int u = c - 'a';
            if (!p->son[u]) p->son[u] = new TrieNode();
            p = p->son[u];
        }
        p->cnt++;
    }

    string searchRoot(string word) {
        TrieNode *p = root;
        string res;
        for (auto c : word) {
            int u = c - 'a';
            res += c;
            if (!p->son[u]) return word;
            p = p->son[u];
            if (p->cnt) return res;
        }
        return word;
    }
};

class Solution {
    vector<string> split(string s) {
        vector<string> res;
        stringstream ss(s);
        string word;
        while (getline(ss, word, ' ')) {
            res.emplace_back(word);
        }
        return res;
    }

public:
    string replaceWords(vector<string> &dictionary, string sentence) {
        Trie *root = new Trie();
        for (auto &word:dictionary) {
            root->insert(word);
        }
        string res = "";
        auto senArray = split(sentence);
        for (auto sen:senArray) {
            res += root->searchRoot(sen) + ' ';
        }
        res.pop_back();
        return res;
    }
};
```
##### leetcode 676 实现一个魔法字典

Trie + dfs

```C++
struct Trie_Node {
    int cnt;//cnt 表示trie_node是is_end的单词数量
    Trie_Node *son[26];

    Trie_Node() {
        cnt = 0;
        for (int i = 0; i < 26; i++) son[i] = nullptr;
    }
};

class MagicDictionary {
    Trie_Node *root = new Trie_Node();
public:
    MagicDictionary() {
    }

    void insert(string word) {
        Trie_Node *p = root;
        for (auto c:word) {
            int u = c - 'a';
            if (!p->son[u]) p->son[u] = new Trie_Node();
            p = p->son[u];
        }
        p->cnt++;
    }

    void buildDict(vector<string> dictionary) {
        for (auto &word:dictionary) {
            insert(word);
        }
    }

    bool search(string searchWord) {
        return dfs(searchWord, root, 0, 0);
    }

    bool dfs(string s, Trie_Node *p, int u, int c) {
        if (c == 1 && u == s.size() && p->cnt) return true;
        if (c > 1 || u == s.size()) return false;
        for (int i = 0; i < 26; i++) {
            if (!p->son[i]) continue;
            if (dfs(s, p->son[i], u + 1, c + (s[u] - 'a' != i))) {
                return true;
            }
        }
        return false;
    }
};

```

##### Leetcode 820 单词的压缩编码

```C++
struct TrieNode {
    int cnt;
    TrieNode *son[26];

    TrieNode() {
        cnt = 0;
        for (int i = 0; i < 26; i++) son[i] = nullptr;
    }
};

class Solution {
    TrieNode * root = new TrieNode();
    unordered_map<TrieNode*,int> len;//类似cnt

    void insert(string word) {
        auto p = root;
        int i = word.size() - 1;
        for (; i >= 0; i--) {
            int u = word[i] - 'a';
            if (!p->son[u]) p->son[u] = new TrieNode();
            p->cnt++;
            p = p->son[u];
        }
        len[p] = word.size() - i;
    }
public:
    int minimumLengthEncoding(vector<string>& words) {
        for(auto word:words){
            insert(word);
        }

        int ans = 0;
        for (auto[k, v] :len) {
            if(!k->cnt){
                ans += len[k];
            }
        }
        return ans;
    }
};

```

875   567   438  528  648  676 820 677 210 444  785  542 752 547 269 329  547 684 839  695 
