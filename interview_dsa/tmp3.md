
## 【alkf009】1023sjbc“备忘录设计系统”学习交流
```C++
class MemoSystem {
public:
    MemoSystem() {}
 
    int AddEvent(int startDate, string content, int num, int period)
    {
        int count = 0;
        for (int i = startDate; i < startDate + num * period; i += period) {
            if (memoMap[i].find(content) == memoMap[i].end()) {
                memoMap[i][content] = false;
                ++count;
            }
        }
        return count;
    }
 
    bool FinishEvent(int date, string content)
    {
        if (memoMap[date].find(content) != memoMap[date].end() && memoMap[date][content] == false) {
            memoMap[date][content] = true;
            return true;
        }
        return false;
    }
 
    bool RemoveEvent(int date, string content)
    {
        if (memoMap[date].find(content) != memoMap[date].end()) {
            memoMap[date].erase(content);
            return true;
        }
        return false;
    }
 
    vector<string> QueryTodo(int startDate, int endDate)
    {
        vector<string> items;
        for (int i = startDate; i <= endDate; ++i) {
            for (auto &item : memoMap[i]) {
                if (!item.second) {
                    items.emplace_back(item.first);
                }
            }
        }
        return items;
    }
 
    unordered_map<int, map<string, bool>> memoMap;
};

```




## 【alkf010】1106sjbc“服务器组的均匀拆分”学习交流

```C++
class Solution {
public:
    // 待实现函数，在此函数中填入答题代码
    int SplitEqualTreeAndOutputNewRoot(const vector<int>& tree)
    {
        Sum.resize(tree.size());
        queue<size_t> Q;
        Q.push(0);
        vector<vector<size_t>> bi_tree(tree.size());
        for(size_t i = 1; i < tree.size(); ++i)
        {
            int q = Q.front();
            if(bi_tree[q].size() == 2)
            {
                Q.pop();
                q = Q.front();
            }
            bi_tree[q].push_back(i);
            if(tree[i] != -1)
            {
                Q.push(i);
            }
        }
        int64_t sum = GetSumForNode(bi_tree, tree, 0);
        if(sum % 2 != 0)
        {
            return -1;
        }
 
        for(size_t i = 0; i < Sum.size(); ++i)
        {
            if(tree[i] != -1 && Sum[i] * 2 == sum)
            {
                return i;
            }
        }
        return -1;
    }
    int64_t GetSumForNode(const vector<vector<size_t>>& bi_tree, const vector<int>& tree, size_t node)
    {
        if(node >= tree.size() || tree[node] == -1)
        {
            return 0;
        }
 
        int64_t sum = tree[node];
        if(bi_tree[node].size()>0)
        {
            sum += GetSumForNode(bi_tree, tree, bi_tree[node][0]);
        }
 
        if(bi_tree[node].size()>1)
        {
            sum += GetSumForNode(bi_tree, tree, bi_tree[node][1]);
        }
 
        return Sum[node] = sum;
    }
    vector<int64_t> Sum;
};


```



## 【alkf014】1204sjbc“任务规划”学习交流

```C++
class Solution {
public:
    int DivideGroup(const vector<int>& tasks, const vector<vector<int>>& mutexPairs) {
        if (tasks.size() == 0) {
            return 0;
        }
        if (mutexPairs.size() == 0) {
            return 1;
        }
        int maxV = 200000;
        vector<int> index(tasks.size(), 0);
        for (int i = 0; i < tasks.size(); i++) {
            index[tasks[i]] = i;
        }
        vector<int> M(tasks.size(), maxV);
        for (int i = 0; i < mutexPairs.size(); i++) {
            int left = index[mutexPairs[i][0]];
            int right = index[mutexPairs[i][1]];
            if (left > right) {
                swap(left, right);
            }
            if (right < M[left]) {
                M[left] = right;
            }
        }
        int res = 0;
        int min_value = maxV;
        for (int i = 0; i < tasks.size(); i++) {
            if (i == min_value) {
                min_value = M[i];
                res++;
            } else {
                min_value = min(min_value, M[i]);
            }
        }
        res++;
        return res;
    }
};

```


## 【alkf015】1211sjbc“结果对比”学习交流
```C++
	class Solution {
public:
    vector<vector<int>> CmpScores(const vector<vector<int>> &scores)
    {
        size_t rowSize = scores.size();
        size_t colSize = scores[0].size();
        vector<vector<int>> ans(rowSize, vector<int>(colSize, 0));
        for (size_t i = 0; i < rowSize; i++) {
            auto row = scores[i];
            sort(row.begin(), row.end());
            for (size_t j = 0; j < colSize; j++) {
                ans[i][j] += row.end() - upper_bound(row.begin(), row.end(), scores[i][j]);
            }
        }
        for (size_t j = 0; j < colSize; j++) {
            vector<int> col(rowSize, 0);
            for (size_t i = 0; i < rowSize; i++) {
                col[i] = scores[i][j];
            }
            sort(col.begin(), col.end());
            for (size_t i = 0; i < rowSize; i++) {
                ans[i][j] += col.end() - upper_bound(col.begin(), col.end(), scores[i][j]);
            }
        }
        return ans;
    }
};

```

## 【alkf021】0122sjbc“数字字符串插入”学习交流
```C++
struct Offset {
    int offset;
    bool isGreatest = false;
};
 
class Solution {
public:
    string InsertDigit(string score, string digit)
    {
        vector<Offset> maxPoses(digit.size());
        string digitMax;
        for (size_t i = 0; i < digit.size(); ++i) {
            string cascade = digit + digit;
            copy(digit.begin(), digit.end(), cascade.begin() + i);
            if (cascade > digitMax) {
                maxPoses[i].offset = i;
                digitMax = cascade;
            } else {
                maxPoses[i].offset = maxPoses[i - 1].offset;
            }
        }
        maxPoses[maxPoses[digit.size() - 1].offset].isGreatest = true;
 
        size_t digitIndex = 0;
        size_t insertPos;
        for (insertPos = 0; insertPos < score.size(); ++insertPos) {
            if (score[insertPos] > digit[digitIndex]) {  // 整体向前移动digit
                if (digitIndex > 0) {
                    digitIndex = 0;
                    --insertPos;
                }
                continue;
            }
            if (score[insertPos] < digit[digitIndex]) {
                insertPos -= digitIndex - maxPoses[digitIndex].offset;  // 回退插入点到最大值下标位置
                break;
            }
            if (score[insertPos] == digit[digitIndex]) {
                if (digitIndex > 0 && maxPoses[digitIndex].isGreatest) {
                    insertPos -= digitIndex - maxPoses[digitIndex].offset;  // 回退插入点到最大值下标位置
                    break;
                }
                ++digitIndex;  // 准备继续比较digit的下一个数字
                if (digitIndex >= digit.size()) {
                    digitIndex = 0;
                }
                if (insertPos == score.size() - 1) {
                    insertPos = insertPos + 1 - (digitIndex - maxPoses[digitIndex].offset);
                    break;
                }
            }
        }
        score.insert(insertPos, digit);
        return score;
    }
};



```

##  【alkf025】0305sjbc“最长空闲内存”学习交流
```C++

class Solution {
public:
    int GetMaxFreeMemoryLen(string memory, int cnt)
    {
        int left = 0;
        int right = 0;
        int fcount = 0;
        int result = 0;
        while (right < memory.size()) {
            fcount += memory[right] == 'x';
            while (fcount > cnt) {
                fcount -= memory[left] == 'x';
                left++;
            }
            result = max(result, right - left + 1);
            right++;
        }
        return result;
    }
};

```



##  【alkf028】0326sjbc“字符串编码校验”学习交流

```C++
 #include <queue>
#include <string>
#include <stdlib.h>
using namespace std;
 
class Solution {
public:
    int totalWay = 0;
    int totalCount = 0;
    queue<int> totalCounts;
 
    bool IsNumber(char c) {
        return c >= '0' && c <= '9';
    }
 
    void Check(string encodedString)
    {
        if (encodedString[0] == '0') {
            return;
        }
 
        for (int i = 0; i < encodedString.size(); i++) {
            if (!IsNumber(encodedString[i])) {
                return;
            }
 
            int count = atoi(encodedString.substr(0, i - 0 + 1).c_str());
            string subEncodedString = encodedString.substr(i + 1);
            if (count > subEncodedString.size()) {
                return;
            }
            if (count == subEncodedString.size()) {
                this->totalCounts.push(this->totalCount + count);
                this->totalWay += 1;
                return;
            }
            this->totalCount += count;
            Check(subEncodedString.substr(count));
            this->totalCount -= count;
        }
    }
 
    int GetLength(string encodedString)
    {
        Check(encodedString);
        return this->totalWay != 1 ? -1 : this->totalCounts.front();
    }
};


```



## 【alkf032】0423sjbc“门票分派”学习交流
```C++
class Solution {
public:
    int LatestTime(const vector<int> &distribute, int num, const vector<int> &arrive) {
        stack<int> state;
        for (int i = arrive.size() - 1; i >= 0; --i) {
            state.push(arrive[i]);
        }
        int lastOne = 0;
        bool hasLeft = false;
        for (int i = 0; i < distribute.size(); ++i) {
            int curNum = distribute[i];
            int count = num;
            bool leftThisRound = false;
            while (!state.empty() && (count != 0)) {
                if (state.top() <= curNum) {
                    lastOne = state.top();
                    state.pop();
                    count--;
                    continue;
                }
                leftThisRound = true;
                break;
            }
            if ((state.empty() && ((count != 0) || (i < (distribute.size() - 1)))) ||
                (leftThisRound && (i == distribute.size() - 1))) {
                hasLeft = true;
                break;
            }
        }
        if (hasLeft) {
            return distribute[distribute.size() - 1];
        }
        return (lastOne - 1);
    }
};


```



## 【alkf035】0521sjbc“租房信息查询系统”学习交流

```cpp
class RentingSystem {

public:

    RentingSystem()
    {
        this->roomsystem = vector<tuple<int, int, int, int, int>>(1001);
    }

    bool AddRoom(int id, int area, int price, int rooms, const vector<int>& address)
    {
        if(dict.count(id)){
            roomsystem[id] = {area, price, rooms, address[0], address[1]};
            return false;
        }

        else{
            roomsystem[id] = {area, price, rooms, address[0], address[1]};
            dict[id] = 1;
            return true;
        }
    }

    bool DeleteRoom(int id)
    {
        if(!dict.count(id)) return false;
        else{
            dict.erase(id);
            return true;
        }
    }

    vector<int> QueryRoom(int area, int price, int rooms, const vector<int>& address, const vector<vector<int>>& orderBy)

    {
        vector<int> ids;
        for(auto item:dict){
            int id = item.first;
            auto [ar, pr, ro, x, y] = roomsystem[id];
            if(ar>=area && pr<=price && ro==rooms){
                ids.push_back(id);  
            }
        }

        sort(ids.begin(), ids.end(), [&](int a, int b){
            auto [ara, pra, roa, xa, ya] = roomsystem[a];
            auto [arb, prb, rob, xb, yb] = roomsystem[b];
            int disa = abs(xa-address[0])+abs(ya-address[1]);
            int disb = abs(xb-address[0])+abs(yb-address[1]);
            for(auto item: orderBy){
                if(item[0]==1&&ara!=arb) return item[1]==1?ara<arb:ara>arb;
                if(item[0]==2&&pra!=prb) return item[1]==1?pra<prb:pra>prb;
                if(item[0]==3&&disa!=disb) return item[1]==1?disa<disb:disa>disb;
            }

            return a<b;
        });

        return ids;
    }

private:
    vector<tuple<int, int, int, int, int>> roomsystem;
    map<int, int> dict;

};


```

该解法没有专门定义结构体，而是用一个tuple<int, int, int, int, int>存储一套房源信息。

C++11引入的tuple是一个固定大小的不同类型值的集合，是泛化的std::pair。我们可以把他当做一个通用的结构体来用，不需要定义结构体又获得结构体的特征，在某些情况下可以使程序更简洁，直观。


排序时，直接用C++标准库的sort算法，加上lambda表达式就更简洁了。

auto [ar, pr, ro, x, y] = roomsystem[id]; 这样的代码是用了C++17的新语法“结构化绑定”。

结构化绑定所声明的变量有两种形式：

l  非引用方式，此时初始化表达式对象会拷贝一份，变量所绑定的是初始化表达式对象拷贝的各个子对象。

l  引用方式，此时初始化表达式对象不会拷贝，变量所绑定的是初始化表达式对象本身的各个子对象。

代码思路清晰，但是有比较多违反编程规范的地方；圈复杂度15也比较大，会扣3分。

注：Cmetrics会将函数内的lambda函数一起统计圈复杂度，如果代码比较长建议还是定义全局或者静态的cmp函数。



## 【alkf037】0604sjbc“地铁闸机”学习交流
```C++
class Solution {
public:
    vector<int> GetTimes(const vector<int>& arrTime, const vector<int>& direction)
    {
        if (arrTime.empty()) {
            return vector<int>();
        }
        vector<int> outTime(arrTime.size(), 0);
        int left = 0; // 出站的队列位置
        int right = 0; // 进站的队列位置
        int flag = 0; // 方向
        int timePoint = -1; // -1 作为最小时刻初值，出队的时刻
        while (left < arrTime.size() && right < arrTime.size()) {
            if (right < arrTime.size() && arrTime[right] > timePoint) {
                timePoint = arrTime[right];
                flag = 1;
            }
            while (right < arrTime.size() && arrTime[right] <= timePoint) {
                if (direction[right] == flag) {
                    outTime[right] = timePoint++;
                }
                right++;
            }
            if (left < right) {
                flag = !flag;
            }
            while (left < right) {
                if (direction[left] == flag) {
                    outTime[left] = timePoint++;
                }
                left++;
            }
        }
        return outTime;
    }
};

```


## alkf039】0618sjbc“基站信号地图”学习交流

```C++
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <algorithm>
#include <map>
using namespace std;
 
struct Command {
    string cmd;
    int row;
    int col;
};
 
class Solution {
public:
    // 待实现函数，在此函数中填入答题代码;
    vector<vector<int> > dirList = {{0, 0}, {-1, 0}, {-1, 1}, {-1, -1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
    int GetMatrixSum(int rows, int cols, const vector<pair<int, int>> &baseStations, const vector<Command> &cmds)
    {
        map<pair<int, int>, bool> basePos;
        for (auto item : baseStations) {
            basePos[make_pair(item.first - 1, item.second - 1)] = true;
        }
        for (auto item : cmds) {
            pair<int, int> key = make_pair(item.row - 1, item.col - 1);
            if (item.cmd == "delete") {
                DeleteBasePos(key, basePos, rows, cols);
                continue;
            }
            basePos[key] = true;
        }
        return CaculateMatrix(basePos, rows, cols);
    }
 
    int CaculateMatrix(map<pair<int, int>, bool>& basePos, int rows, int cols)
    {
        int result = 0;
        for (auto item : basePos) {
            for (auto dir : dirList) {
                pair<int, int> newKey = make_pair(dir[0] + item.first.first, dir[1] + item.first.second);
                bool notInMatrix = !IsValid(newKey, rows, cols) || (basePos.find(newKey) != basePos.end());
                if (notInMatrix) {
                    continue;
                }
                ++result;
            }
        }
        return result;
    }
 
    void DeleteBasePos(pair<int, int> key, map<pair<int, int>, bool>& basePos, int rows, int cols)
    {
        for (auto item : dirList) {
            pair<int, int> newKey = make_pair(item[0] + key.first, item[1] + key.second);
            if (basePos.find(newKey) == basePos.end()) {
                continue;
            }
            basePos.erase(newKey);
        }
    }
 
    bool IsValid(pair<int, int>& key, int row, int col)
    {
        return (key.first >= 0 && key.first < row) && (key.second >= 0 && key.second < col);
    }
};

```






## 【alkf044】0723sjbc“数据设备系统设计”学习交流
```C++
class DataMachineSystem {
public:
    explicit DataMachineSystem(int num)
    {
        contribute.resize(num + 1);
        nums_ = num;
    }
 
    int TransferData(int machineA, int machineB, int dataId)
    {
        if (dataIndex[machineA].find(dataId) == dataIndex[machineA].end()) { // a没有数据
            dataIndex[machineA][dataId].push_back(machineA);
        }
        if (dataIndex[machineB].find(dataId) == dataIndex[machineB].end()) { // b没有数据
            dataIndex[machineB][dataId] = dataIndex[machineA][dataId];
            for (auto it : dataIndex[machineB][dataId]) {
                contribute[it] += 10; // 10
            }
            dataIndex[machineB][dataId].push_back(machineB);
            return 1;
        }
        return 0; // b有数据
    }
 
    int TransferDataToAll(int machine, int dataId)
    {
        int res = 0;
        for (int i = 1; i <= nums_; ++i) {
            res += TransferData(machine, i, dataId);
        }
        return res;
    }
 
    int QueryContribution(int machine)
    {
        return contribute[machine];
    }
 
private:
    int nums_;
    unordered_map<int, map<int, vector<int>>> dataIndex; // 1. dataId // 2.dataSource
    vector<long> contribute;                             // 1.
};

```


## 【alkf047】0813sjbc“文件树”学习交流
```C++
*
 * Copyright (c) Huawei Technologies Co., Ltd. 2020-2021. All rights reserved.
 * Description: sjbcrz
 * Note: 缺省代码仅供参考，可自行决定使用、修改或删除
 */
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <sstream>
using namespace std;
 
struct Node {
    int level;
    map<string, Node*> child;
    explicit Node(int inLevle) : level(inLevle) {}
};
 
const int G_BLANK_NUM = 2;
 
class Solution {
public:
    void Dfs(const Node* root, vector<string>& res)
    {
        if (root->child.size() == 0) {
            return;
        }
        for (auto it : root->child) {
            string blank(root->level * G_BLANK_NUM, ' ');
            string cur = blank + it.first;
            res.push_back(cur);
            if (it.second != NULL) {
                Dfs(it.second, res);
            }
        }
    }
 
    vector<string> GetTreeFormat(const vector<string>& fileTreeList)
    {
        Node* root = new Node(0);
        for (auto file : fileTreeList) {
            istringstream inFile(file);
            string name;
            Node* cur = root;
            while (getline(inFile, name, '/')) {
                if (cur->child.find(name) == cur->child.end()) {
                    cur->child[name] = new Node(cur->level + 1);
                }
                cur = cur->child[name];
            }
        }
        vector<string> result;
        Dfs(root, result);
        return result;
    }
};
 
// 以下为考题输入输出框架，此部分代码不建议改动
inline int ReadInt()
{
    int number;
    cin >> number;
    return number;
}
 
inline string ReadLine()
{
    string line;
    getline(cin, line);
    return line;
}
 
template<typename T>
inline void WriteVector(const vector<T>& objects, char delimeter = ' ')
{
    auto it = objects.begin();
    if (it == objects.end()) {
        return;
    }
    cout << *it;
    for (++it; it != objects.end(); ++it) {
        cout << delimeter << *it;
    }
    cout << endl;
}
 
int main()
{
    int num = ReadInt();
    cin.ignore();
    vector<string> fileTreeList;
    for (int i = 0; i < num; ++i) {
        string oneLine = ReadLine();
        fileTreeList.push_back(oneLine);
    }
    Solution solu;
    vector<string> result = solu.GetTreeFormat(fileTreeList);
    WriteVector(result, '\n');
    return 0;
}


```



## 【alkf050】0903sjbc[船票预订系统设计]学习交流
```C++
class TicketSystem {
public:
    explicit TicketSystem(const vector<int> &cabins)
    {
        int n = cabins.size();
        Cabins.resize(n);
        Waiting.resize(n);
        for (int i = 0; i < n; ++i) {
            Cabins[i] = {cabins[i], cabins[i], bitset<1000>(0)};
        }
    }
 
    bool Book(int id, int cabinId, int num)
    {
        if (Waiting[cabinId].size() != 0 || get<1>(Cabins[cabinId]) < num) {
            Waiting[cabinId].emplace_back(id, num);
            return false;
        }
        Request(id, cabinId, num);
        return true;
    }
 
    bool Cancel(int id)
    {
        if (auto it = Booked.find(id); it != Booked.end()) {
            const auto &[cabinId, num, book] = it->second;
            get<1>(Cabins[cabinId]) += num;
            get<2>(Cabins[cabinId]) ^= book;
            Waitline(cabinId);
            Booked.erase(it);
            return true;
        }
        for (int cid = 0; cid < Waiting.size(); ++cid) {
            for (auto it = Waiting[cid].begin(); it != Waiting[cid].end(); ++it) {
                if (it->first == id) {
                    Waiting[cid].erase(it);
                    Waitline(cid);
                    return true;
                }
            }
        }
        return false;
    }
 
    int Query(int id)
    {
        if (auto it = Booked.find(id); it != Booked.end()) {
            int i = 0;
            while (i < 1000 && !get<2>(it->second)[i]) {
                ++i;
            }
            return i;
        }
        return -1;
    }
 
private:
    void Request(int id, int cabinId, int num)
    {
        if (num > get<1>(Cabins[cabinId])) {
            return;
        }
        get<1>(Cabins[cabinId]) -= num;
        bitset<1000> book(0);
        for (int l = -1, r = 0; r < get<0>(Cabins[cabinId]);) {
            while (++l < get<0>(Cabins[cabinId]) && get<2>(Cabins[cabinId])[l]) {
            }
            r = l;
            while (++r < get<0>(Cabins[cabinId]) && !get<2>(Cabins[cabinId])[r]) {
            }
            if (r - l >= num) {
                for (int i = l; i < l + num; ++i) {
                    get<2>(Cabins[cabinId])[i] = true;
                    book[i] = true;
                }
                Booked[id] = {cabinId, num, std::move(book)};
                return;
            }
            l = r;
        }
 
        for (int i = 0, t = num; i < get<0>(Cabins[cabinId]) && t > 0; ++i) {
            if (!get<2>(Cabins[cabinId])[i]) {
                --t;
                get<2>(Cabins[cabinId])[i] = true;
                book[i] = true;
            }
        }
        Booked[id] = {cabinId, num, std::move(book)};
    }
 
    void Waitline(int cabinId)
    {
        auto it = Waiting[cabinId].begin();
        while (it != Waiting[cabinId].end() && it->second <= get<1>(Cabins[cabinId])) {
            Request(it->first, cabinId, it->second);
            it = Waiting[cabinId].erase(it);
        }
    }
 
    vector<tuple<int, int, bitset<1000>>> Cabins;
    vector<list<pair<int, int>>> Waiting;
    unordered_map<int, tuple<int, int, bitset<1000>>> Booked;
};

```




## 【alkf052】0917sjbc“SQL解析执行”学习交流
```C++
 #include <iostream>
#include <sstream>
#include <vector>
#include <unordered_map>
#include <string>
#include <map>
 
using namespace std;
 
// 待实现函数，在此函数中填入答题代码
struct Table {
    int colNum;
    string keys;
    map<vector<int>, vector<int>> col;
};
 
class Solution {
public:
    void Create(int tableId, int colNum, const string &keys) {
        if (myTables.count(tableId) == 0) {
            myTables[tableId] = {colNum, keys, {}};
        }
    }
 
    void Insert(int tableId, const vector<int> &rec) {
        auto &table = myTables[tableId];
        vector<int> keyIndex;
        for (auto key : table.keys) {
            keyIndex.push_back(key - 'a');
        }
        vector<int> key;
        for (auto index : keyIndex) {
            key.push_back(rec[index]);
        }
        if (table.col.count(key) == 0) {
            table.col[key] = rec;
        }
    }
 
    vector<vector<int>> Select(int tableId, const vector<string> &conditions) {
        auto table = myTables[tableId];
        vector<pair<int, int>> numConditions;
        for (auto condition : conditions) {
            int index = condition[0] - 'a';
            int value = stoi(condition.substr(2));
            numConditions.push_back({index, value});
        }
        vector<vector<int>> result;
        for (auto &column : table.col) {
            if (judge(numConditions, column.second) == true) {
                result.push_back(column.second);
            }
        }
        return result;
    }
 
    bool judge(vector<pair<int, int>> numConditions, vector<int> column) {
        for (auto &condition : numConditions) {
            if (column[condition.first] != condition.second) {
                return false;
            }
        }
        return true;
    }
 
private:
    unordered_map<int, Table> myTables;
};
 
// 以下为考题输入输出框架，此部分代码不建议改动
inline int ReadInt() {
    int number;
    cin >> number;
    return number;
}
 
inline string ReadLine() {
    string line;
    getline(cin, line);
    return line;
}
 
// 按字符串拆分input
void SplitString(const string &input, const string &sepStr, vector<string> &outArray) {
    size_t startPos = 0;
    size_t pos = input.find(sepStr, 0);
    while (pos != input.npos) {
        outArray.push_back(input.substr(startPos, pos - startPos));
        startPos = pos + sepStr.size();
        pos = input.find(sepStr, startPos);
    }
    outArray.push_back(input.substr(startPos));
}
 
template<typename T>
inline void WriteVector(const vector<T> &objects, char delimeter = ' ') {
    auto it = objects.begin();
    if (it == objects.end()) {
        return;
    }
    cout << *it;
    for (++it; it != objects.end(); ++it) {
        cout << delimeter << *it;
    }
}
 
int main() {
    int num = ReadInt();
    cin.ignore();
    Solution solu;
    auto outputQueryRes = [](const auto &result) {
        if (!result.empty()) {
            for (auto &rec : result) {
                WriteVector(rec, ' ');
                cout << endl;
            }
        } else {
            cout << "empty" << endl;
        }
    };
    for (int loop = 0; loop < num; loop++) {
        string line = ReadLine();
        size_t pos = line.find_first_of(":");
        string command = line.substr(0, pos);
        if (command == "CREATE") {
            vector<string> ary;
            SplitString(line.substr(pos + 1), ",", ary);
            solu.Create(stoi(ary[0]), stoi(ary[1]), ary[2]);
        } else if (command == "INSERT") {
            int tableId = stoi(line.substr(pos + 1, line.find_first_of(",") - pos - 1));
            vector<string> values;
            SplitString(line.substr(line.find_first_of(",") + 1), " ", values);
            vector<int> rec;
            for (auto value : values) {
                rec.push_back(stoi(value));
            }
            solu.Insert(tableId, rec);
        } else if (command == "SELECT") {
            int tableId = stoi(line.substr(pos + 1, line.find_first_of(",") - pos - 1));
            vector<string> conditions;
            SplitString(line.substr(line.find_first_of(",") + 1), " and ", conditions);
            auto result = solu.Select(tableId, conditions);
            outputQueryRes(result);
        }
    }
    return 0;
}



```



##  【alkf053】0924sjbc“二叉树最大深度”学习交流
```C++
class Solution {
public:
    void dfs(const TreeNode *root, map<int, int> &heights, int step, vector<int> &nums)
    {
        if (root == NULL)
            return;
        nums.push_back(root->val);
        heights[root->val] = max(heights[root->val], step);
        dfs(root->left, heights, step + 1, nums);
        dfs(root->right, heights, step + 1, nums);
    }
    vector<int> ValueDepth(const vector<int> &target, const TreeNode *root)
    {
        map<int, int> heights;
        vector<int> nums;
        vector<int> res;
        dfs(root, heights, 1, nums);
        sort(nums.begin(), nums.end());
        map<int, int> counts;
        auto iter = heights.rbegin();
        int curmax = 0;
 
        while (iter != heights.rend()) {
            curmax = max(curmax, iter->second);
            counts[iter->first] = curmax;
            iter++;
        }
 
        for (int i = 0; i < target.size(); i++) {
            int pos = upper_bound(nums.begin(), nums.end(), target[i]) - nums.begin();
            if (pos < nums.size()) {
                int id = nums[pos];
                res.push_back(counts[id]);
            } else {
                res.push_back(-1);
            }
        }
        return res;
    }
};

```
