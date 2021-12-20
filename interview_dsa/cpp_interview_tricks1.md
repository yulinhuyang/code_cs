
#### 1 vector

```c++
vector<int> dp(10,0); //初始化为0
//二维数组初始化、遍历
vector<vector<int>> dp(m,vector<int>(n,0));

for(int i = 0;i < dp.size();i++){
    for(int j = 0;j < dp[0].size();j++)
    {
        cout<<dp[i][j]<<endl;
    }
}

dp.empty(); //判空
dp.size();
dp.emplace_back(2);
dp.back();
dp.pop_back();
sort(dp.begin(),dp.end());//排序
swap(nums[i],nums[k]);//交换两个数
reverse(nums.begin() + j,nums.end());//反转一段数组

```

**C++ 不提倡使用vector<bool>**

 |=  重载了 vector<int>类型
 
 dp数组定义，可以用 vector<vector<int>> dp(m +1,vector<int>(n + 1, 0))，实际赋值采用true

dp[i][j] |= dp[i][j-1]

**快速交换**

swap(vector[0],vector[4]);

**for范围循环**
```c++
string str("some string"); 
 
// range for 语句  
for(auto &c : str)  
{  
    c = toupper(c);  
}

for(const int &num:nums)
{
    
}

auto matrix_new = matrix;   // C++ 这里的 = 拷贝是值拷贝，会得到一个新的数组

matrix = matrix_new;  //赋值拷贝

```
**引用与更新**

merged.back()[1] = max(merged.back()[1],R);

**cmp自定义比较**

```c++
class Solution {
public:
    static bool cmp(vector<int> &a,vector<int> &b){
        if(a[0] > b[0]){
            return false;
        }

        return true;
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        sort(intervals.begin(),intervals.end(),cmp);
	}
```
**C++ vector切片**

```C++

auto v2 = vector<int> (v1.begin(),v1.end());
	
auto v2 = vector<int> (v1.begin(),v1.begin() + 4);

```

#### 2 map（**unordered_map** ）

```c++
unordered_map<string,int> dict;
unordered_map<string,vector<int>> dict;

dict["desk"]=1;    //自动创建该key
dict.insert(make_pair('abc',0))

// { }直接初始化：
unordered_map<char,string> dict = {
    {'2',"abc"},
    {'3',"def"}
}

dict.erase(0); //删除key
dict.size()

判断key是否存在 两种方式：
map.find(key) != map.end()   //find返回迭代器
//1表示存在，0表示不存在,count只会返回0/1
if(dict.count("key"))  //实际是dict.count("key")>0 


//map(unordered_map)访问key自动创建，需要count(key)判断才可以访问
if(need.count(char_right)){
    window[char_right]++;
    if(window[char_right] == need[char_right]){
        valid++;
    }
}


//遍历map
for(auto iter = dict.begin(); iter != dict.end(); iter++) {
    cout << iter->first << " : " << iter->second << endl;
}
for(auto [k,v]:dict){
    cout << k <<" " <<v<<endl;
}

//map基于红黑树，自动排序；unordered_map基于哈希表，无序快


//map —>value->list 结构：

字母异位词

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> dict;
        for(auto &str:strs){
            auto key = str;  //值拷贝
            sort(key.begin(),key.end());
            dict[key].emplace_back(str);
        }

        vector<vector<string>> ans;
        for(auto it = dict.begin();it != dict.end();it ++){
            ans.emplace_back(it->second);
        }
        return ans;
    }
};

```

#### 3  string

```c++
string str;
const string str2; //const灵活使用
str +='d'; // '' char类型
str +="ansjcknkj";// "" string类型
// C++区分'' 和"" 类型

//API  类vector化
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

//c语言形式
std::string str = "870";
int i = atoi(str.c_str());

//查找
int pos = str.find('ab') // rfind 反向找
str1 == str2 //判断相等

//char和int转换 
char c4 = i + '0';
int ia = a - '0';

//得到ascii值
char a = '0';
int ia = (int)a; 

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


#### 4 stack

```c++
#include <stack>

stack<int>  sta;
sta.push(2);
index = sta.top();
sta.pop();
sta.size();
sta.empty();
```

#### 5 queue、deque

queue：头pop,尾 push

deque:双端

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


deque<int> deque;

//滑动窗口，单调队列，存index,先出头端，再判断尾部，

deque.emplace_back(1);
deque.pop_back();
deque.pop_front();
deque.push_front(2);

```


#### 6 set(unordered_set)

```c++
unordered<int> set;
unordered_set<int> set;

set.insert(1);
//判断是否包含
if(set.count(1) > 0){ 
    
}
set.erase(1);
for(auto it:set){
    cout<<it<<endl;
}
//set基于红黑树，自动排序；unordered_set基于哈希表，无序快

//字典也可以用set，判断是否存在，如单词拆分问题、最长连续子序列问题
unordered_set<string> set;
for (auto word:wordDict) {
    set.emplace(word);
}

```

#### 7 list

list是一个双向链表,不支持随机取元素

```C++
#include <list>
list<int> cacheList;

cacheList.push_back(1);
cacheList.pop_back();
cacheList.push_front();
cacheList.pop_front();

cacheList.front();  
cacheList.back();  

cacheList.erase(pos); 

//void splice ( iterator pos, list& other, iterator it );  将other中的it移动到pos位置，list特有
cacheList.splice(cacheList.begin(), cacheList, cacheMap[key]);

```
	
	
##### 8 pair 

```c++
vector<pair<int,int>> relations;
pair<int, string> p1;
p1.first;
p1.second;
relations.emplace_back(make_pair(1,2));

// 四向回溯访问路径
vector<pair<int,int>> direct = {{-1,0},{0,-1},{1,0},{0,1}};

```
	
##### 9 tuple

	

##### 10 
	


##### 11  bitsets

```c++

#include <bitset>
bitset<n> b(u);  //n位,int初始化
bitset<n> b(str);//n位，string初始化

```	

 

#### 10 指针的使用 与new

```python

ListNode *head = nullptr;
head = new ListNode(sum%10); // new MyClass 返回的是MyClass的指针

int *p = new int(87); //
```

#### 11 类与继承

继承类的 虚函数必须要实现

==default

==delete

定义与声明：

1）A  a；在栈(stack)上分配空间；

2）A  *a；只是声明，还没有分配空间；

3）A  *a= new A；在堆(heap)上分配空间；

栈上空间自动回收，堆空间需要程序员手动回收。


#### 12 常规数据类型

INT_MIN,INT_MAX

LONG_MIN,LONG_MAX 

longlong： 属于int型，一般来讲，是longint型大小的两倍，int型的4倍。

**初始化**

内置类型变量（如 int ，double，bool等）： 定义于任何函数之外的变量被初始化为0, 定义于函数体内部的内置类型将不被初始化，一个未被初始化的内置类型变量的值是未定义的.

其他类型的变量（如 string 或 其他自定义类型）：不管定义于何处，都会执行默认构造函数。如果该类没有默认构造函数，则会引发错误。因此，建议为每个类都定义一个默认构造函数（=default）。

#### 13 统计运行时间

C++ 11使用 chrono

```C++
#include <chrono>
using namespace std::chrono;

auto start = system_clock::now();
auto end  = system_clock::now();

std::chrono::duration<double> elapsed = end - start;
std::cout << "Elapsed time: " << elapsed.count() << "s";

```
	
	
