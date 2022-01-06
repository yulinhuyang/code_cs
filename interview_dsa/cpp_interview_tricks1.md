
https://soulmachine.gitbooks.io/algorithm-essentials/content/cpp/

https://labuladong.gitee.io/algo/2/

#### 1 vector

使用* 解迭代器 a[0]和*a.begin()一样

所有容器都是前闭后开，*a.end()和a[n]都是越界访问

back()返回最后一个元素，相当于a[s.size()-1]和*--a.end()

```c++
//初始化
vector<int> dp(10,0);  //初始化为0
vector<vector<int>> dp(m,vector<int>(n,0));   //二维数组初始化、遍历
vector<int> dp(dp0.begin() + 2,dp0.end() - 1);  //迭代器初始化

dp.empty();  
dp.size();
dp.emplace_back(2);
dp.back();
dp.pop_back();

// 查找
it = find(v2.begin(), v2.end(), 5);
if (it != v2.end()) {   // 找到了，必须做一次判断，以防空迭代器异常
    //
}
//遍历
for(int i = 0;i < dp.size();i++){
    for(int j = 0;j < dp[0].size();j++)
    {
        cout<<dp[i][j]<<endl;
    }
}

//插入、交换
dp.insert(dp.begin()+2,3);// 2位置插入3
swap(nums[i],nums[k]);//交换两个数

//排序
sort(dp.begin(),dp.end());//排序
reverse(nums.begin() + j,nums.end());//反转一段数组
```

##### erase相关
```c++
// erase the 6th element
myvector.erase (myvector.begin()+5);

// erase the first 3 elements:
myvector.erase (myvector.begin(),myvector.begin()+3);

//迭代器删除
for(vector<int>::iterator iter = vecInt.begin(); iter != vecInt.end(); iter++){
	if(*iter == 444){		
		vecInt.erase(iter);			
	}
}

// Erase–remove惯用法
v.erase(std::remove(v.begin(), v.end(), 5), v.end());
```
remove把不“符合”删除标准的元素搬移到容器的前部，并保持这些元素的相对次序

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
	
##### 排序

比较函数模板:

升序：sort(begin,end,less<data-type>());
	
降序：sort(begin,end,greater<data-type>()).
	
cmp自定义比较: 

```c++
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
支持比较运算，按字典序
	
lambda自定义排序
	
```cpp
typedef pair<int, int> ii;
vector<ii> vp{ { 1,1 },{ 1,2 },{ 2,2 },{ 2,3 },{ 3,3 } };
sort(vp.begin(), vp.end(), [](const ii &l, const ii &r) {   // 按第一个数字升序，第二个降序
    return l.first != r.first ? l.first < r.first : l.second > r.second;
});
```
	
##### C++ vector切片

```C++

auto v2 = vector<int> (v1.begin(),v1.end());
	
auto v2 = vector<int> (v1.begin(),v1.begin() + 4);

```

#### 2 map（**unordered_map** ）
	
map基于红黑树，自动排序；unordered_map基于哈希表，无序更快，可以使用map来的代替vector,使用find 快速查找

哈希结构
	
|集合 |底层实现 | 是否有序 |数值是否可以重复 | 能否更改数值|查询效率 |增删效率|
|---|---| --- |---| --- | --- | ---|
|std::set |红黑树 |有序 |否 |否 | $O(\log n)$|$O(\log n)$ |
|std::multiset | 红黑树|有序 |是 | 否| $O(\log n)$ |$O(\log n)$ |
|std::unordered_set |哈希表 |无序 |否 |否 |$O(1)$ | $O(1)$|

std::unordered_set底层实现为哈希表，std::set 和std::multiset 的底层实现是红黑树，红黑树是一种平衡二叉搜索树，所以key值是有序的，但key不可以修改，改动key值会导致整棵树的错乱，所以只能删除和增加。

|映射 |底层实现 | 是否有序 |数值是否可以重复 | 能否更改数值|查询效率 |增删效率|
|---|---| --- |---| --- | --- | ---|
|std::map |红黑树 |key有序 |key不可重复 |key不可修改 | $O(\log n)$|$O(\log n)$ |
|std::multimap | 红黑树|key有序 | key可重复 | key不可修改|$O(\log n)$ |$O(\log n)$ |
|std::unordered_map |哈希表 | key无序 |key不可重复 |key不可修改 |$O(1)$ | $O(1)$|

std::unordered_map 底层实现为哈希表，std::map 和std::multimap 的底层实现是红黑树。同理，std::map 和std::multimap 的key也是有序的（这个问题也经常作为面试题，考察对语言容器底层的理解）。

map解引用，得到pair类型，map[key]会默认创建，需要先find
	
```c++
//创建初始化
unordered_map<string,int> dict;
unordered_map<string,vector<int>> dict;
unordered_map<char,string> dict = {
    {'2',"abc"},
    {'3',"def"}
} // { }直接初始化：

//插入
dict["desk"]=1;    //自动创建该key
ret = dict.insert(make_pair('abc',0))
if (ret.second == false) //如果存在插入失败
    cout << "exist" << endl;

//新插入语法更快
ret = m3.emplace(5, 6);
it = m3.emplace_hint(it, 8, 9);  // hint 插入

//删除
dict.erase(0); 
dict.size()

//查找key是否存在，两种方式：
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
//查找by value
int v = 77;
it = find_if(m3.begin(), m3.end(),
    [v](const std::map<int, int>::value_type item) {
    return item.second == v;
});
if (it != m3.end()) {
    int k = (*it).first;
    cout << k << endl; // 7
}

//遍历map
for(auto iter = dict.begin(); iter != dict.end(); iter++) {
    cout << iter->first << " : " << iter->second << endl;
}
for(auto & [k,v]:dict){
    cout << k <<" " <<v<<endl;
}	
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

#### 3 set(unordered_set)

set基于红黑树，自动排序；unordered_set基于哈希表，无序快;multiset运行重复值，可以自动排序

```c++
//初始化
unordered_set<int> set;
set<int> s2{ 3,1,2 };
vector<int> v { 3,2,1,5,4 };
set<int> s3(v.begin(), v.end());                 //vector初始化
set<int, cmp> s4(v.begin(), v.end());            // 利用仿函数构造降序集
set<int, greater<int>> s5(v.begin(), v.end());   // STL 提供的 greater 仿函数

//插入
set.insert(1);
set.emplace(1);
	
//查找：find count
if(set.count(4) > 0){ 
}
auto it = s2.find(4);
if (it != s2.end()) { // 找到了
    //
}
//删除
set.erase(1);
//遍历
for(auto it:set){
    cout<<it<<endl;
}
//逆序遍历
for (auto it = s4.rbegin(); it != s4.rend(); it++)
    cout << *it << ' ';

//字典也可以用set，判断是否存在，如单词拆分问题、最长连续子序列问题
unordered_set<string> set;
for (auto word:wordDict) {
    set.emplace(word);
}
	
//允许重复的集合
multiset<int> ms1{ 1,2,2,2,3,4 };
auto itp1 = ms1.find(2);  // No.2
auto itp2 = ms1.lower_bound(2);  // No.2
cout << (itp1 == itp2) << endl;  // True
auto itp3 = ms1.upper_bound(2);  // No.5
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
#### 4  string

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

//char 转string：
string(1,'a')

//c语言形式
std::string str = "870";
//c_str()  返回字符串所在字符数组的起始地址
int i = atoi(str.c_str());

//查找
int pos = str.find('ab')
int pos = str.find('ab',5) //从5开始找 
position=str.rfind ('ab'); // rfind 反向找
position = str.find_first_of('ab');
position = str.find_last_of('ab');
	
//判等
str1 == str2  


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
##### 字符串截取
	
**std::find** 

查找[first,last)范围内，与val等价的第一个元素，返回一个迭代器。如果没有这个元素，将返回last。
	
auto pos = find(i,path.end(),'/');
String tmp = string(i,pos);
 
**find + substr 字符串截取**

/..   后漏掉后的.., 查找不到会返回 string::npos
	
```C++
auto j = path.find_first_of("/",i);
if(j == string::npos){
   j = path.size();
}
autodir=path.substr(i,j-i);

```
通过getline方法最好
	
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
//dirs中无空格
string getString(vector<string> &str) {
    stringstream out;
    if (dirs.empty()) {
        out << '/';
    } else {
        for (auto ch:dirs) {
            out << "/" << ch;
        }
    }
    return out.str();
}

string getString(vector<string> &str) {
    string ret;
    for (const auto &s:str) {
        ret += s;
    }
    return ret;
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
	
void SplitString(const string &input, string str, vector<string> &outArray) {
    int ptr = 0;
    for (int i = 0; i < input.size(); i++) {
        auto pos = input.find(str, i);
        //找不到返回string::npos				     
        if (pos < input.size()) {
            outArray.emplace_back(input.substr(i, pos - i));
            i = pos + str.size() - 1;
        }
    }
}
```

#### 5 stack
其内部是使用双端队列 deque 实现的，屏蔽了部分接口
```c++
#include <stack>

stack<int>  sta;
sta.push(2);
index = sta.top();
sta.pop();
sta.size();
sta.empty();
```

#### 6 queue、deque

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

#### 7 list

list是一个双向链表,不支持随机取元素

```C++
#include <list>
list<int> l1;

l1.push_back(1);
l1.pop_back();
l1.push_front();
l1.pop_front();

l1.front();  
l1.back(); 

//插入
l1.insert(l1.begin(), 3);     // 在开头插入 3
// 删除
auto ret = find(l1.begin(), l1.end(), 3);
cout << *ret << endl;
l1.erase(ret);
ret = find(l1.begin(), l1.end(), 3);
if (ret == l1.end())
    cout << "have erase 3" << endl;
	
//清空
l1.clear();

//void splice ( iterator pos, list& other, iterator it );  将other中的it移动到pos位置，list特有
l1.splice(l1.begin(), l1, cacheMap[key]);
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
	
排序：比较函数模板、带operator的结构体、函数指针、lambda表达式

```C++
#include <queue>	
//初始化
priority_queue <int,vector<int>,greater<int> > q;//小顶堆
priority_queue <int,vector<int>,less<int> > q;  //大顶堆
priority_queue<int> p1(v1.begin(), v1.end());  //vector转pq
	
//greater和less是std实现的两个仿函数（就是使一个类的使用看上去像一个函数。其实现就是类中实现一个operator()，这个类就有了类似函数的行为，就是一个仿函数类了）
	
//API
priority_queue<int> p;
p.push(2);
p.pop();
p.top();

//遍历
std::priority_queue<std::string> pq {words};  
while (!pq.empty())
{
    std:: cout << pq.top () <<" ";
    pq.pop();
}
```	
##### 排序
	
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
```
	
#### 9 pair 

template<class T1,class T2> struct pair
	
支持比较运算，以first为第一关键字，以second为第二关键字（字典序）

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
两者 std::set<>和 std::map<>可以使用 std::pair作为关键，但为什么不能std::unordered_set<>和 std::unordered_map<> ?

unordered_*容器需要一个哈希函数。默认情况下，他们使用 std::hash但没有特化std::hash为 std::pair<T1,T2>标准库中提供。
另一方面，有序容器依赖于 std::less (默认)和 std::pair确实有operator<提供。这就是为什么它只是有效。
	
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

sort(首元素地址(必填), 尾元素地址的下一个地址(必填), 比较函数(非必填)),默认是递增数列
	
**比较函数模板**
	
functional头文件中，有很多比较函数对象
[sort()函数与升序、降序 C++](https://blog.csdn.net/zhinanpolang/article/details/50917019)
equal_to<Type>、not_equal_to<Type>、greater<Type>、greater_equal<Type>、less<Type>、less_equal<Type>	

**find函数**
```cpp
find (InputIterator first,   InputIterator last, const T& val);
查找vector: find(ar1.begin(), ar1.end(), "bbb")
查找string: if(str.find(ch)!=string::npos)
	
//unique 
sort(words.begin(), words.end()); 
vector<string>::iterator end_unique =  unique(words.begin(), words.end());    //重复元素移到后面
words.erase(end_unique, words.end());  //删掉重复元素
//max_element  min_element
min = *min_element(v.begin(),v.end());
```
#### 12  bitsets

[c++ bitset类用法](https://blog.csdn.net/qll125596718/article/details/6901935)

```c++

#include <bitset>
bitset<n> b(u);  //n位,int初始化
bitset<n> b(str);//n位，string初始化
	
b.count()  //返回有多少个1
b.any()    //判断是否至少有一个1
b.none()   //判断是否全为0

b.set()    //把所有位置成1
b.set(k, v)  //将第k位变成v
b.reset()   //把所有位变成0
b.flip()    //等价于~
b.flip(k)   //把第k位取反
```	
	
#### 13 lambda表达式用法

[capture list] (params list) mutable exception-> return type { function body }

函数名相当于使用[]代替了
	
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

long long： 属于int型，一般来讲，是longint型大小的两倍，int型的4倍。
									 

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
#### 18 其他功能函数
	
**cmath.h 数学函数**
	
ceil()：向上取整; floor():向下取整; round() 四舍五入
	
**cctype.h 字符处理库**

isalnum: 检查字符是否为字母或数字

isalpha:检查字符是否为字母	
	
isdigit:检查字符是否为数字	
	
tolower 转换字符为小写
	
**numeric 数值算法**
	
accumulate：两个形参指定要累加的元素范围，第三个形参则是累加的初值

int sum = accumulate(vec.begin() , vec.end() , 42);  
	
 
	
