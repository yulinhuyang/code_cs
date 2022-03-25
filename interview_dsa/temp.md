LFU： https://leetcode-cn.com/problems/lfu-cache/solution/gong-shui-san-xie-yun-yong-tong-pai-xu-s-53m3/

https://www.acwing.com/activity/content/code/content/555766/

 1  map<key,node> + set<node>(代替堆)，因为C++中堆pq的值不能改。
 
 2  map(key, list<node> :: iterator) + map(freq,list<node>) ：链接法。散列冲突解决：链接法、开发定址法。类似邻接表结构，展开的list链表。比较图存储 map + map
 
 
 map存储结构体，需要insert或emplace后使用make_pair,不能直接赋值
 
 ```C++
 struct ABC{
    int x,y;
    ABC(int _x,int _y):x(_x),y(_y){};
};
 
unordered_map<int,ABC> hash;
ABC abc(1,1);
hash.insert(make_pair(1,abc));//正确插入
//hash[1] = abc;//错误插入
ABC ac(1,2);
auto it = hash.find(1);
it->second = ac;//正确更新

```
 
结构体定义初始化列表和自定义排序结构
 
```C++
 struct Node {
    int freq, time;
    int key, val;

    Node(int _freq, int _time, int _key, int _val) : freq(_freq), time(_time), key(_key), val(_val) {}

    bool operator< (const Node &rhs) const {
        return freq == rhs.freq ? time < rhs.time : freq < rhs.freq;
    }
}; 
 
```

##### 460. LFU 缓存
 
set + hash
```C++
 struct Node {
    int freq, time;
    int key, val;

    Node(int _freq, int _time, int _key, int _val) : freq(_freq), time(_time), key(_key), val(_val) {}

    bool operator< (const Node &rhs) const {
        return freq == rhs.freq ? time < rhs.time : freq < rhs.freq;
    }
};

class LFUCache {
    unordered_map<int, Node> key_table;
    set<Node> S;
    int capacity, time; //时间戳
public:
    LFUCache(int _capacity) {
        capacity = _capacity;
        time = 0;
        key_table.clear();
        S.clear();
    }

    int get(int key) {
        if (capacity == 0) return -1;
        auto it = key_table.find(key);
        if (it == key_table.end()) return -1;
        auto node = it->second;
        S.erase(node);
        node.freq += 1;
        node.time = ++time;
        S.insert(node);
        it->second = node;
        return node.val;
    }

    void put(int key, int value) {
        if (capacity == 0) return;
        auto it = key_table.find(key);
        if (it == key_table.end()) {
            if (key_table.size() == capacity) {
                key_table.erase(S.begin()->key);
                S.erase(S.begin());
            }
            Node node(1, ++time,key, value);
            S.insert(node);
            key_table.emplace(make_pair(key,node));
        } else {
            auto node = it->second;
            S.erase(node);
            node.freq += 1;
            node.time = ++time;
            node.val = value;
            S.insert(node);
            it->second = node;
        }
    }
};
 
 ```
 
 双hash表
 
 ```C++
 struct Node {
    int key, val, freq;
    Node(int _key, int _val, int _freq) : key(_key), val(_val), freq(_freq) {};
};

class LFUCache {
    int capacity, min_freq;
    unordered_map<int, list<Node>::iterator> key_table;//key -> list<node>::iterator
    unordered_map<int, list<Node>> freq_table; //freq -> list<node>
public:
    LFUCache(int _capacity) {
        capacity = _capacity;
        min_freq = 0;
        key_table.clear();
        freq_table.clear();
    }

    int get(int key) {
        if(capacity == 0) return -1;
        auto it = key_table.find(key);
        if (it == key_table.end()) return -1;
        auto node_it = it->second;
        int val = node_it->val;
        int freq = node_it->freq;
        freq_table[freq].erase(node_it);
        if (freq_table[freq].size() == 0) {
            freq_table.erase(freq);
            if (min_freq == freq) min_freq++;
        }
        freq_table[freq + 1].push_front(Node(key, val, freq + 1));
        key_table[key] = freq_table[freq + 1].begin();
        return val;
    }

    void put(int key, int value) {
        if(capacity == 0) return;
        auto it = key_table.find(key);
        if (it == key_table.end()) {
            if (key_table.size() == capacity) {
                auto node = freq_table[min_freq].back();
                freq_table[min_freq].pop_back();
                key_table.erase(node.key);
                if (freq_table[min_freq].size() == 0) {
                    freq_table.erase(min_freq);
                }
            }
            min_freq = 1;
            freq_table[min_freq].push_front(Node(key, value, min_freq));
            key_table[key] = freq_table[min_freq].begin();
        } else {
            auto node_it = key_table[key];
            int freq = node_it->freq;
            freq_table[freq].erase(node_it);
            if (freq_table[freq].size() == 0) {
                freq_table.erase(freq);
                if (min_freq == freq) min_freq++;
            }
            freq_table[freq + 1].push_front(Node(key, value, freq + 1));
            key_table[key] = freq_table[freq + 1].begin();
        }

    }
};

 ```
 
