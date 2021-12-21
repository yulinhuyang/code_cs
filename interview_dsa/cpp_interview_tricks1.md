https://soulmachine.gitbooks.io/algorithm-essentials/content/cpp/


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

##### C++ 不提倡使用vector<bool> 

 |=  重载了 vector<int>类型
 
 dp数组定义，可以用 vector<vector<int>> dp(m +1,vector<int>(n + 1, 0))，实际赋值采用true

dp[i][j] |= dp[i][j-1]


##### for范围循环	

 range for结构: for(auto &num: nums)  
	
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

```
**引用与更新**
	
auto matrix_new = matrix;   // C++ 这里的 = 拷贝是值拷贝，会得到一个新的数组

matrix = matrix_new;  //赋值拷贝

merged.back()[1] = max(merged.back()[1],R);
	
**快速交换**

swap(vector[0],vector[4]);

##### 排序

比较函数模板:

升序：sort(begin,end,less<data-type>());
	
降序：sort(begin,end,greater<data-type>()).
	
cmp自定义比较: 

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
##### C++ vector切片

```C++

auto v2 = vector<int> (v1.begin(),v1.end());
	
auto v2 = vector<int> (v1.begin(),v1.begin() + 4);

```

#### 2 map（**unordered_map** ）
	
map基于红黑树，自动排序；unordered_map基于哈希表，无序快，所以可以使用map来的代替vector,使用find 快速查找


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
for(auto & [k,v]:dict){
    cout << k <<" " <<v<<endl;
}

//map基于红黑树，自动排序；unordered_map基于哈希表，无序快
```

##### map —>value->list 结构：

字母异位词

```C++
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

	
##### map排序

[C++ STL中Map的按Key排序和按Value排序](https://blog.csdn.net/iicy266/article/details/11906189)

比较函数模板(默认是less 升序)
	
map<string, int, greater<string> > name_score_map;

自定义cmp：带operator结构体、函数指针、lambda表达式
	
```C++
struct CmpByKeyLength {
  bool operator()(const string& k1, const string& k2) {
    return k1.length() < k2.length();
  }
};
map<string, int, CmpByKeyLength> name_score_map;
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
int pos = str.find('ab') 
str1 == str2 //判断相等
position=str.rfind ('ab'); // rfind 反向找
position = str.find_first_of('ab');
position = str.find_last_of('ab');


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
	
##### 字典序输出
	
1. 字符串比较：类似于C语言的strcmp函数，C++标准库string类重载了大于、等于、小于等运算符，可直接用于字符串的比较（基于字典序）。例如 "apple" < "banana", "9"> "10" 。

2. 字符串序列排序：同样的，类似于C语言的qsort函数，C++标准库还提供了 sort函数，可用于对字符串序列进行排序。sort 函数默认的排序方式是字典序升序，两个参数就可以了。sort函数原型如下：

基于sort函数，可以采用不同形式的comp参数，来实现按字典序降序排序：

1）lambda表达式（其中调用string类的比较运算符实现）；2）全局函数或者静态函数；3）greater模板类对象；

也可以采用 sort 的默认形式完成升序排序，然后再调用 reverse 反转实现降序排序	

```C++
	
//原型
void sort(RandomIt first, RandomIt last);
void sort(RandomIt first, RandomIt last, Compare comp);

bool Cmp(const string &a, const string &b)
{
   return a > b;
}

sort(arr.begin(), arr.end(), [](string a, string b) { return a > b; });

// sort(arr.begin(), arr.end(), Cmp);
// sort(arr.begin(), arr.end(), greater<string>());

sort(arr.begin(), arr.end());
reverse(arr.begin(), arr.end());	

```
#####  to_string atoi  stoi
	
[c++中的atoi()和stoi()函数的用法和区别](https://blog.csdn.net/qq_33221533/article/details/82119031)

都是C++的字符处理函数，把数字字符串转换成int输出,

atoi和stoi都是只转换字符串的数字部分，遇到其他字符则停止转换。

atoi()的参数是 const char* ,因此对于一个字符串str我们必须调用 c_str()的方法把这个string转换成 const char*类型的,
	
stoi()的参数是const string*,不需要转化为 const char*；

stoi()会做范围检查，默认范围是在int的范围内的，如果超出范围的话则会runtime error

atoi()不会做范围检查，如果超出范围的话，超出上界，则输出上界，超出下界，则输出下界;
	
```C++
	
string s1 = "2147482", s2 = "-214748";
cout << stoi(s1);
cout << atoi(s2.c_str()) << endl;

```
to_string
	
把数值类型如int、double、long等转化为string

```C++
double a = 3.14;
str1 = to_string(a);
```
	
##### split与join的实现

join函数

```cpp
string join(vector<string> sequence, string separator) {
    string result;
    for (int i = 0; i < sequence.size(); i++) {
        if (sequence[i] != "") {
            result += sequence[i] + ((i != sequence.size() - 1) ? separator : "");
        }
    }
    return result;
}
```					

split函数

```cpp					
void SplitString(const string& input, char sperChar, vector<string>& outArray)

{
    stringstream sstr(input);
    string token;
    while (getline(sstr, token, sperChar)) {
        outArray.push_back(token);
    }
}
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

set基于红黑树，自动排序；unordered_set基于哈希表，无序快

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
	
比较函数模板
	
自定义：带operator的结构体、函数指针、lambda表达式
	
```c++
//comp也可以是less<Type>
	
struct comp
{
	template<typename T>
	bool operator()(const T& l, const T& r) const
	{
		if (l.first == r.first)
			return l.second > r.second;
		return l.first < r.first;
	}
};
std::set<std::pair<std::string,int>, comp> set

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
	
##### list 排序

比较函数模板法

自定义cmp
```C++
bool compare(A a1, A a2){
	return a1.a < a2.a;  //会产生升序排列，若改为>,则会产生降序； 
}
list_a.sort(compare); //排序操作； 
``` 	

	
#### 8 priority_queue(堆)
	
[【c++】STL里的priority_queue用法总结](https://blog.csdn.net/xiaoquantouer/article/details/52015928)

priority_queue<Type, Container, Functional>

Type为数据类型， Container为保存数据的容器，Functional为元素比较方式。

如果不写后两个参数，那么容器默认用的是vector，比较方式默认用operator<，也就是优先队列是大顶堆，队头元素最大。

改成优先小顶堆： priority_queue<int, vector<int>, greater<int> > p;


比较函数模板
	
自定义：带operator的结构体、函数指针、lambda表达式

```C++
#include <queue>	

//升序队列
priority_queue <int,vector<int>,greater<int> > q;
//降序队列
priority_queue <int,vector<int>,less<int> > q;
	
//greater和less是std实现的两个仿函数（就是使一个类的使用看上去像一个函数。其实现就是类中实现一个operator()，这个类就有了类似函数的行为，就是一个仿函数类了）

//pair的比较，先比较第一个元素，第一个相等比较第二个
priority_queue<pair<int, int> > p;


//可调用的函数操作符的对象 
struct mycmp{
    bool operator()(const student & a,const student & b){
        return a.age < b.age;
    }
};
			     
//函数指针 
bool cmpfunc(const student& a, const student& b){
    return a.age < b.age;
}

//自定义lambda比较
auto cmp = [](int left, int right) { return (left ^ 1) < (right ^ 1); };
std::priority_queue<int, std::vector<int>, decltype(cmp)> q3(cmp);

//API
priority_queue<int> p;
p.push(2);
p.pop();
p.top();
	
```

**遍历**
	
```C++	
std::priority_queue<std::string> pq {words};  
while (!pq.empty())
{
    std:: cout << pq.top () <<" ";
    pq.pop();
}
	
```	

##### 排序比较
	
[C++ STL中set/map 与 priority_queue 中greater、less 的用法区别](https://blog.csdn.net/liqinzhe11/article/details/79278235)
	
set和map：底层都是红黑树 less<> 最小堆，greater<>是最大堆。 默认是less，升序。

make_heap:  less<>() 展现出来的是最大堆， greater<>()展现出来是最小堆。  默认是less。

priority_queue:   底层是使用heap实现的，所以表现出来的特性和heap一致。 

                     less<>() 展现出来的是最大堆， greater<>()展现出来是最小堆。  默认是less。

greater: a > b  ,less: a < b
			    
```
template <class T> struct greater {
  bool operator() (const T& x, const T& y) const {return x>y;}
  typedef T first_argument_type;
  typedef T second_argument_type;
  typedef bool result_type;
};

template <class T> struct less {
  bool operator() (const T& x, const T& y) const {return x<y;}
  typedef T first_argument_type;
  typedef T second_argument_type;
  typedef bool result_type;
};
	
```
	
#### 9 pair 

template<class T1,class T2> struct pair

```c++
	
vector<pair<int,int>> relations;
pair<int, string> p1;
p1.first;
p1.second;
relations.emplace_back(make_pair(1,2));

// 四向回溯访问路径
vector<pair<int,int>> direct = {{-1,0},{0,-1},{1,0},{0,1}};

//函数会以pair对象作为返回值,可以直接通过std::tie进行接收
std::tie(name, ages) = getPreson();

```
	
#### 10 tuple
	
[C++ tuple元组的基本用法(总结)](https://blog.csdn.net/sevenjoin/article/details/88420885)
		
tuple是一个固定大小的不同类型值的集合，是泛化的std::pair，可以大量作为结构体使用
	
```C++
//创建
std::make_tuple(v1, v2);

	
//和结构体一样使用
std::tuple<const char *, const char *, int>
	
//TI作为一种数据类型使用
using TI = tuple<pair<int,int>,int,int>

vector<tuple<int,int,int>> vec;
	
//可以通过get<Ith>(obj)方法
std::tuple<int, char, double> mytuple (10, 'a', 3.14);
std::cout << std::get<0>(mytuple)

//利用tie进行解包元素的值
std::tuple<std::string, int, std::string, int> tp;
std::tie(name, ages, addr, areaCode) = tp;

```
	
#### 11 algorithm文件函数

[C++ algorithm头文件函数的基本用法](https://blog.csdn.net/knighkingLOL/article/details/79851806)

##### 二分法
	
支持二分的常用数据结构： vector multiset/set map/multimap
	
**binary_search**
	
```C++
//查找 [first, last) 区域内是否包含 val
bool binary_search (ForwardIterator first, ForwardIterator last,
                      const T& val);
```

**升序序列**

lower_bound：返回第一个 >= 目标值的迭代器，找不到则返回end()。

upper_bound：返回第一个 > 目标值的迭代器，找不到则返回end()。

**降序序列**

需要重载或者目标比较器，例如greater()

lower_bound：返回第一个 <= 目标值的迭代器，找不到则返回end()。

upper_bound：返回第一个 < 目标值的迭代器，找不到则返回end()。

```C++

 // 返回第一个小于等于目标值的迭代器
lower_bound(vec.begin(), vec.end(), 8, greater<int>());

// 返回第一个小于目标值的迭代器
upper_bound(vec.begin(), vec.end(), 8, greater<int>());

 bool isFind = binary_search(vec.begin(), vec.end(), 7);

// 返回第一个大于等于目标值的迭代器
 vector<int>::iterator iter1 = lower_bound(vec.begin(), vec.end(), 8);

 // 返回第一个大于目标值的迭代器
 vector<int>::iterator iter2 = upper_bound(vec.begin(), vec.end(), 8);
```

##### 排序
	
**reverse()**
	
reverse(it, it2) 可以将数组指针在[it, it2)之间的元素或容器的迭代器在 [it, it2) 范围内的元素,可以对字符串进行反转

reverse(str.begin()+2, str.begin()+6);//对a[2]~a[5]逆转*左闭右开* 
	
**sort()**

sort(首元素地址(必填), 尾元素地址的下一个地址(必填), 比较函数(非必填))
	
默认是递增数列
	
##### 比较函数模板
	
functional头文件中，有很多比较函数对象
	
[sort()函数与升序、降序 C++](https://blog.csdn.net/zhinanpolang/article/details/50917019)
	
equal_to<Type>、not_equal_to<Type>、greater<Type>、greater_equal<Type>、less<Type>、less_equal<Type>	

##### find函数
	
find (InputIterator first,   InputIterator last, const T& val);
 
查找vector
find(ar1.begin(), ar1.end(), "bbb")

查找string
 if(str.find(ch)!=string::npos)

#### 12  bitsets

[c++ bitset类用法](https://blog.csdn.net/qll125596718/article/details/6901935)

```c++

#include <bitset>
bitset<n> b(u);  //n位,int初始化
bitset<n> b(str);//n位，string初始化

```	
	
#### 13 lambda表达式用法

[capture list] (params list) mutable exception-> return type { function body }

//用于复杂的逻辑比较
sort(lbvec.begin(), lbvec.end(), [](int a, int b) -> bool { return a < b; });   // Lambda表达式


#### 14 指针的使用 与new

```python

ListNode *head = nullptr;
head = new ListNode(sum%10); // new MyClass 返回的是MyClass的指针

int *p = new int(87); //
```

#### 15 类与继承

继承类的 虚函数必须要实现

==default

==delete

定义与声明：

1）A  a；在栈(stack)上分配空间；

2）A  *a；只是声明，还没有分配空间；

3）A  *a= new A；在堆(heap)上分配空间；

栈上空间自动回收，堆空间需要程序员手动回收。


#### 16 常规数据类型

INT_MIN,INT_MAX

LONG_MIN,LONG_MAX 

longlong： 属于int型，一般来讲，是longint型大小的两倍，int型的4倍。

**初始化**

内置类型变量（如 int ，double，bool等）： 定义于任何函数之外的变量被初始化为0, 定义于函数体内部的内置类型将不被初始化，一个未被初始化的内置类型变量的值是未定义的.

其他类型的变量（如 string 或 其他自定义类型）：不管定义于何处，都会执行默认构造函数。如果该类没有默认构造函数，则会引发错误。因此，建议为每个类都定义一个默认构造函数（=default）。

#### 17 统计运行时间

C++ 11使用 chrono

```C++
#include <chrono>
using namespace std::chrono;

auto start = system_clock::now();
auto end  = system_clock::now();

std::chrono::duration<double> elapsed = end - start;
std::cout << "Elapsed time: " << elapsed.count() << "s";

```
	
	
