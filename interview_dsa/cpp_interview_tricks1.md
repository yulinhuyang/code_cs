
#### vector

```c++
vector<int> dp(10,0); //初始化为0
//二维数组初始化、遍历
vector<vector<int>> dp(m,vector<int>(n,0));

for(int i = 0;i < dp.size();i++)
    for(int j = 0;j < dp[0].size();j++)
{
    cout<<dp[i][j]<<endl;
}


dp.empty(); //判空
dp.size();
dp.emplace_back(2);
dp.back();
dp.pop_back();
sort(dp.begin(),dp.end());

```

#### pair 

```c++
vector<pair<int,int>> relations;
pair<int, string> p1;
p1.first;
p1.second;
relations.emplace_back(make_pair(1,2));
```

#### map（**unordered_map** ）

```c++
unordered_map<string,int> dict;
unordered_map<string,vector<int>> dict;
dict["desk"]=1;    //自动创建该key
dict.insert(make_pair('abc',0))

dict.erase(0); //删除key
dict.size()

判断key是否存在 两种方式：
map.find(key) != map.end()   //find返回迭代器
//1表示存在，0表示不存在
if(dict.count("key")>0)

    
//遍历map
for(auto iter = dict.begin(); iter != dict.end(); iter++) {
    cout << iter->first << " : " << iter->second << endl;
}
for(auto [k,v]:dict){
    cout << k <<" " <<v<<endl;
}

//map基于红黑树，自动排序；unordered_map基于哈希表，无序快
```

#### string

```c++
string str;
str +='d';
str +="ansjcknkj";

//API
str.size();
str.back();
str.push_back('c');
str.pop_back();
//截取字符串
stringe = str.substr(start,len);
//int 转string
str = to_string(870);
//string 转int
int num = stoi("870");

//查找
int pos = str.find('ab') // rfind 反向找
str1 == str2 //判断相等


//stringstream 使用
#include<sstream> 
//分割字符串，转二进制，再转string
stringstream ss(dstIp); 
stringstream outDstIP;
while (getline(ss, str, '.')) {
    int tmp = stoi(str);
    bitset<8> bTMP(tmp);
    outDstIP << bTMP.to_string();
}
string binDstIP = outDstIP.str();//转string
```


#### stack

```c++
#include <stack>

stack<int>  sta;
sta.push(2);
index = sta.top();
sta.pop();
sta.size();
sta.empty();
```

#### queue

```c++
#include <queue>

queue<int> que;
que.push(2);
que.front();
que.back();
que.pop();

eg:
int e = q.front();
q.pop();
```

#### set(unordered_set)

```c++
unordered<int> set;
unordered_set<int> set;
set.insert(1);
if(set.count(1) > 0){}
set.erase(1);
for(auto it:set){
    cout<<it<<endl;
}
//set基于红黑树，自动排序；unordered_set基于哈希表，无序快
```

##### bitsets

```c++
#include <bitset>
bitset<n> b(u);  //n位,int初始化
bitset<n> b(str);//n位，string初始化
```


#### 指针的使用 与new

```python
ListNode *head = nullptr;
head = new ListNode(sum%10); // new MyClass 返回的是MyClass的指针

int *p = new int(87); //
```

#### 类与继承

继承类的 虚函数必须要实现
