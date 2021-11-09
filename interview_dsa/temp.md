#### 207. 课程表

```C++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>> &prerequisites) {
        //inDegree表 + 邻接表
        vector<int> inDeg(numCourses, 0);
        vector<vector<int>> edges;
        edges.resize(numCourses);
        for (auto course:prerequisites) {
            inDeg[course[0]]++;
            edges[course[1]].emplace_back(course[0]);
        }

        //bfs
        queue<int> workQueue;
        for (int i = 0; i < numCourses; i++) {
            if (inDeg[i] == 0) {
                workQueue.push(i);
            }
        }

        int visited = 0;
        while (!workQueue.empty()) {
            auto top = workQueue.front();
            workQueue.pop();
            visited++;
            for (auto edge:edges[top]) {
                inDeg[edge]--;
                if (inDeg[edge] == 0) {
                    workQueue.push(edge);
                }
            }
        }

        return numCourses == visited;
    }
};
```
#### 208. 实现 Trie (前缀树)

Python，实例对象调用函数时，自动将对象本身传入函数的第一个变量是self，显示写出来

C++，系统也会将实例对象传入函数，对象的这个参数都是隐藏的，在函数内部才可以显性的使用它this

类与对象的使用

```C++
class Trie {
private:
    bool isEnd;
    vector<Trie *> children;//index 0~26范围,以index为下标

    Trie *searchPrefix(string prefix) {
        Trie *node = this;
        for (auto ch:prefix) {
            ch -= 'a';
            //if 的顺序
            if (node->children[ch] == nullptr) {
                return nullptr;
            }
            node = node->children[ch];
        }
        return node;
    }

public:
    //列表初始化
    Trie() : children(26), isEnd(false) {
    }

    void insert(string word) {
        //this与self
        Trie *node = this;
        for (auto ch:word) {
            ch -= 'a';
            if (node->children[ch] == nullptr) {
                node->children[ch] = new Trie();
            }
            node = node->children[ch];
        }
        node->isEnd = true;
    }

    bool search(string word) {
        Trie *ret = this->searchPrefix(word);
        return ret != nullptr && ret->isEnd;
    }

    bool startsWith(string prefix) {
        Trie *ret = this->searchPrefix(prefix);
        return ret != nullptr;
    }
};

```



