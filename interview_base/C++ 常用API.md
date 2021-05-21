https://www.cnblogs.com/ccxikka/p/10409203.html


**1、stack：**

https://www.cnblogs.com/hdk1993/p/5809161.html

使用该容器时需要包含#include<stack>头文件；

定义stack对象的示例代码如下：

stack<int>s1;

stack<string>s2;

stack的基本操作有：

1.入栈：如s.push(x);

2.出栈:如 s.pop().注意：出栈操作只是删除栈顶的元素，并不返回该元素。

3.访问栈顶：如s.top();

4.判断栈空：如s.empty().当栈空时返回true。

5.访问栈中的元素个数，如s.size（）;



```cpp
#include<iostream>  
#include<stack>  
using namespace std;  
int main(void)  
{  
    stack<double>s;//定义一个栈  
    for(int i=0;i<10;i++)  
        s.push(i);  
    while(!s.empty())  
    {  
        printf("%lf\n",s.top());  
        s.pop();  
    }  
    cout<<"栈内的元素的个数为："<<s.size()<<endl;  
    return 0;  
}
```



 

**2、排序sort函数**：

https://www.cnblogs.com/TX980502/p/8528840.html

需要头文件<algorithm>,sort 使用时得注明：using namespace std; 或直接打 std::sort() 

语法描述：sort（begin，end，cmp），cmp参数可以没有，如果没有默认升序排序。

**sort是一个改进版的qsort，能用sort尽量用sort**


```cpp
#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;
int main()
{
    int a[5]={1,3,4,2,5};
    sort(a,a+5);
    for(int i=0;i<5;i++)
            cout<<a[i]<<' ';
    return 0;
 }

因为没有cmp参数，默认为非降序排序，结果为：

1 2 3 4 5
```



可自定义compare函数设定排序规则, 如下按降序排序



```cpp
#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;

bool cmp（int a,int b）

{
　　return a>b;
}

int main()
{
    int a[5]={1,3,4,2,5};
    sort(a,a+5);
    for(int i=0;i<5;i++)
            cout<<a[i]<<' ';
    return 0;
 }

输出： 5 4 3 2 1
```



 

**快排Qsort**：

https://blog.csdn.net/forckgcs/article/details/2291489

定义在头文件<algorithm>中

void qsort ( void * base, size_t nmem, size_t size, int ( * comp) ( const void * , const void * ) ) ;

它根据comp所指向的函数所提供的顺序对base所指向的数组进行排序，nmem为参加排序的元素个数，size为每个元素所占的字节数。如：对一个长为1000的数组进行排序时，int a[1000]; 那么base应为a，num应为 1000，width应为 sizeof(int)，cmp函数随自己的命名

如要对元素进行升序排列，则定义comp所指向的函数为：如果其第一个参数比第二个参数小，则返回一个小于0的值，反之则返回一个大于0的值，如果相等，则返回0。



```cpp
# include < stdio. h> 
# include < stdlib. h> 

int comp( const void * , const void * ) ; 

int main( int argc, char * argv[ ] ) 
{ 
    int i; 
    int array[ ] = { 6, 8, 2, 9, 1, 0} ; 

    qsort ( array, 6, sizeof ( int ) , comp) ; 

    for ( i = 0; i < 6; i + + ) { 
        printf ( "%d/t" , array[ i] ) ; 
    } 
    printf ( "/n" ) ; 

    return 0; 
} 

int comp( const void * p, const void * q) 
{ 
    return ( * ( int * ) p - * ( int * ) q) ; 
}

运行结果如下：
0 1 2 6 8 9
```



 

**3、vector**：

http://www.cnblogs.com/Nonono-nw/p/3462183.html

vector可直接作为数组调用，但是如果要在函数中修改其值，那么函数的形参一定要为引用，即vector<type>&array,否则值可能不会被修改

vector 是向量类型，可以容纳许多类型的数据，如若干个整数，所以称其为容器。使用它时需要包含头文件：#include<vector>;



```cpp
 初始化：
 (1) vector<int> a(10); //定义了10个整型元素的向量（尖括号中为元素类型名，它可以是任何合法的数据类型），但没有给出初值，其值是不确定的。
   （2）vector<int> a(10,1); //定义了10个整型元素的向量,且给出每个元素的初值为1
   （3）vector<int> a(b); //用b向量来创建a向量，整体复制性赋值
   （4）vector<int> a(b.begin(),b.begin+3); //定义了a值为b中第0个到第2个（共3个）元素
   （5）int b[7]={1,2,3,4,5,9,8};
        vector<int> a(b,b+7); //从数组中获得初值
```





```cpp
重要操作：
（1）a.assign(b.begin(), b.begin()+3); //b为向量，将b的0~2个元素构成的向量赋给a
    （2）a.assign(4,2); //是a只含4个元素，且每个元素为2
    （3）a.back(); //返回a的最后一个元素
    （4）a.front(); //返回a的第一个元素
    （5）a[i]; //返回a的第i个元素，当且仅当a[i]存在2013-12-07
    （6）a.clear(); //清空a中的元素
    （7）a.empty(); //判断a是否为空，空则返回ture,不空则返回false
    （8）a.pop_back(); //删除a向量的最后一个元素
    （9）a.erase(a.begin()+1,a.begin()+3); //删除a中第1个（从第0个算起）到第2个元素，也就是说删除的元素从a.begin()+1算起（包括它）一直到a.begin()+         3（不包括它）
    （10）a.push_back(5); //在a的最后一个向量后插入一个元素，其值为5
    （11）a.insert(a.begin()+1,5); //在a的第1个元素（从第0个算起）的位置插入数值5，如a为1,2,3,4，插入元素后为1,5,2,3,4
    （12）a.insert(a.begin()+1,3,5); //在a的第1个元素（从第0个算起）的位置插入3个数，其值都为5
    （13）a.insert(a.begin()+1,b+3,b+6); //b为数组，在a的第1个元素（从第0个算起）的位置插入b的第3个元素到第5个元素（不包括b+6），如b为1,2,3,4,5,9,8         ，插入元素后为1,4,5,9,2,3,4,5,9,8
    （14）a.size(); //返回a中元素的个数；
    （15）a.capacity(); //返回a在内存中总共可以容纳的元素个数
    （16）a.resize(10); //将a的现有元素个数调至10个，多则删，少则补，其值随机
    （17）a.resize(10,2); //将a的现有元素个数调至10个，多则删，少则补，其值为2
    （18）a.reserve(100); //将a的容量（capacity）扩充至100，也就是说现在测试a.capacity();的时候返回值是100.这种操作只有在需要给a添加大量数据的时候才         显得有意义，因为这将避免内存多次容量扩充操作（当a的容量不足时电脑会自动扩容，当然这必然降低性能） 
    （19）a.swap(b); //b为向量，将a中的元素和b中的元素进行整体性交换
    （20）a==b; //b为向量，向量的比较操作还有!=,>=,<=,>,<
```



```cpp
从现有向量中选择元素向向量中添加：
int a[6]={1,2,3,4,5,6};
vector<int> b;
vector<int> c(a,a+4);
for(vector<int>::iterator it=c.begin();it<c.end();it++)
    b.push_back(*it);
```

 

[ **vector>** ](https://blog.csdn.net/yooliee/article/details/71498321)



```cpp
#include<iostream>
#include<vector> 
using namespace std;

int main()
{
    //array用来保存一个3*3的二维数组，array的每个元素都是vector<int>类型
    vector <vector<int> >array;
    std::vector<int> v;
    for (int i = 0; i <3; i++){
        for (int j = 0; j <3; j++){
            int value;
            cin >> value;
            v.push_back(value);
        }
        array.push_back(v); //保存array的每个元素
        v.clear();
    }

    for (int i = 0; i <array.size(); i++)
    {
        for (int j = 0; j <3; j++)
            cout <<array[i][j];
        cout<<endl;
    }
    return 0;
}
```





```cpp
#include<iostream>
#include<vector> 
using namespace std;

int main()
{
    vector <vector<int> >array(3);//首先给array开辟了三个空间
    for (int i = 0; i <3; i++){
        array[i].resize(3);//给array中每个元素开辟了三个空间
        for (int j = 0; j <3; j++){
            cin >> array[i][j];//直接对开辟的空间赋值即可
        }
    }
    for (int i = 0; i <array.size(); i++)
    {
        for (int j = 0; j <3; j++)
            cout <<array[i][j];
        cout<<endl;
    }
    cout << array.size();
    return 0;
}
```



 

**4、queue：**

https://blog.csdn.net/fengzhizi76506/article/details/54809949

 C++队列queue模板类的定义在<queue>头文件中,queue 模板类需要两个模板参数，一个是元素类型，一个容器类型，元素类型是必要的，容器类型是可选的，默认为deque 类型（两端都可以进出的队列类型）

queue 的基本操作举例如下：

queue入队，如例：q.push(x); 将x 接到队列的末端。

queue出队，如例：q.pop(); 弹出队列的第一个元素，注意，并不会返回被弹出元素的值。

访问queue队首元素，如例：q.front()，即最早被压入队列的元素。

访问queue队尾元素，如例：q.back()，即最后被压入队列的元素。

判断queue队列空，如例：q.empty()，当队列空时，返回true。

访问队列中的元素个数，如例：q.size()



```cpp
#include <cstdlib>
#include <iostream>
#include <queue>
using namespace std;
int main()
{
    int e,n,m;
    queue<int> q1;
    for(int i=0;i<10;i++)
       q1.push(i);
    if(!q1.empty())
    cout<<"not empty\n";
    n=q1.size();
    cout<<n<<endl;
    m=q1.back();
    cout<<m<<endl;
    for(int j=0;j<n;j++)
    {
       e=q1.front();
       cout<<e<<" ";
       q1.pop();
    }
    cout<<endl;
    system("PAUSE");
    return 0;
}
```



 

**5、map** 

[使用](https://blog.csdn.net/allovexuwenqiang/article/details/5686583) 

map的基本操作函数：  

   begin()     返回指向map头部的迭代器
   clear(）     删除所有元素
   count()     返回指定元素出现的次数
   empty()     如果map为空则返回true
   end()       返回指向map末尾的迭代器
   equal_range()  返回特殊条目的迭代器对
   erase()     删除一个元素
   find()      查找一个元素
   get_allocator() 返回map的配置器
   insert()     插入元素
   key_comp()    返回比较元素key的函数
   lower_bound()  返回键值>=给定元素的第一个位置
   max_size()    返回可以容纳的最大元素个数
   rbegin()     返回一个指向map尾部的逆向迭代器
   rend()      返回一个指向map头部的逆向迭代器
   size()      返回map中元素的个数
   swap()      交换两个map
   upper_bound()   返回键值>给定元素的第一个位置
   value_comp()   返回比较元素value的函数 
 

 1. map最基本的构造函数；
  map<string , int >mapstring;     map<int ,string >mapint;
  map<sring, char>mapstring;     map< char ,string>mapchar;
  map<char ,int>mapchar;      map<int ,char >mapint；

 2. map添加数据；

  map<int ,string> maplive; 
  1.maplive.insert(pair<int,string>(102,"aclive"));
  2.maplive.insert(map<int,string>::value_type(321,"hai"));
  3, maplive[112]="April";//map中最简单最常用的插入添加
  
3，map中元素的查找：

  find()函数返回一个迭代器指向键值为key的元素，如果没找到就返回指向map尾部的迭代器。    

  map<int ,string >::iterator l_it;
  l_it=maplive.find(112);
  if(l_it==maplive.end())
        cout<<"we do not find 112"<<endl;
  else cout<<"wo find 112"<<endl;
  
4,map中元素的删除：

  如果删除112；
  map<int ,string >::iterator l_it;
  l_it=maplive.find(112);

 if(l_it==maplive.end())
    cout<<"we do not find 112"<<endl;
  else maplive.erase(l_it); //delete 112;

 

**6、[一个fork的面试题](https://coolshell.cn/articles/7965.html) ：关于fork和缓冲区（\n会刷出缓冲区）**

 

**7、set集合**

set集合是c++ stl库中自带的一个容器，set具有以下两个特点：1、set中的元素都是排好序的 2、set集合中没有重复的元素

multiset和set的区别是：multiset中可以有相同的元素，set和multiset都是基于红黑树实现

常用操作：

begin() 　　 返回set容器的第一个元素的地址

end() 　　　　 返回set容器的最后一个元素地址

clear() 　　 删除set容器中的所有的元素

empty() 　　　 判断set容器是否为空

max_size() 　 返回set容器可能包含的元素最大个数

size() 　　　　 返回当前set容器中的元素个数

erase(it) 删除迭代器指针it处元素，

**注意**这里删除multiset中的某个元素时需要使用迭代器it, 否则直接erase(x)表示删除multiset集合中所有的x

insert(a) 插入某个元素



```cpp
#include<stdio.h>
#include<set>
using namespace std;
int main()
{
    set<int>s;
    s.insert(3); 
    s.insert(1);
    s.insert(2);
    s.insert(1);
    set<int>::iterator it;            
    for(it=s.begin();it!=s.end();it++)  //使用迭代器进行遍历 
    {
        printf("%d\n",*it);
    }
    return 0;
}
//输出结果 ： 1 2 3     一共插入了4个数，但是集合中只有3个数并且是有序的，可见之前说过的set集合的两个特点，有序和不重复。
```



 

**8、stringstream:分割被空格、制表符等分割的字符串**



```
#include <string>
#include <iostream>
#include <sstream>
using namespace std;

int main() {
    string input;
    getline(cin, input);//读入一行，默认遇到\n回车时停止
    stringstream ss(input);
    string str1, str2;
    ss >> str1;//遇到空格停止
    ss >> str2; //空格后赋值给str2
}

输入hello world
str1为hello
str2为world
```



 

**9、string :** 

[用法](https://blog.csdn.net/tengfei461807914/article/details/52203202)

使用string首先要在头文件当中加入< string > 



```cpp
 string s;//默认初始化，一个空字符串
    string s1("ssss");//s1是字面值“ssss”的副本
    string s2(s1);//s2是s1的副本
    string s3=s2;//s3是s2的副本
    string s4(10,'c');//把s4初始化
    string s5="hiya";//拷贝初始化
    string s6=string(10,'c');//拷贝初始化，生成一个初始化好的对象，拷贝给s6

    //string s(cp,n)
    char cs[]="12345";
    string s7(cs,3);//复制字符串cs的前3个字符到s当中

    //string s(s2,pos2)
    string s8="asac";
    string s9(s8,2);//从s2的第二个字符开始拷贝，不能超过s2的size

    //string s(s2,pos2,len2)
    string s10="qweqweqweq";
    string s11(s10,3,4);//s4是s3从下标3开始4个字符的拷贝，超过s3.size出现未定义
```



 **char\*和string的转换**：
 
 [使用](https://blog.csdn.net/yzhang6_10/article/details/51164300)

to_string包含在# include<string>, 作用是把数值类型如int、double、long等转化为string

 两个string的比较可以直接使用> < ==.

 

**10、堆** 

[用法](https://blog.csdn.net/whistlena/article/details/52032067)

C++中堆的应用：make_heap, pop_heap, push_heap, sort_heap

函数说明： 
std::make_heap将[start, end)范围进行堆排序，默认使用less, 即最大元素放在第一个。

std::pop_heap将front（即第一个最大元素）移动到end的前部，同时将剩下的元素重新构造成(堆排序)一个新的heap。

std::push_heap对刚插入的（尾部）元素做堆排序。

std::sort_heap将一个堆做排序,最终成为一个有序的系列，可以看到sort_heap时，必须先是一个堆（两个特性：1、最大元素在第一个 2、添加或者删除元素以对数时间），因此必须先做一次make_heap.

make_heap, pop_heap, push_heap, sort_heap都是标准算法库里的模板函数，用于将存储在vector/deque 中的元素进行堆操作。

 

**11 list** 

[用法](https://blog.csdn.net/lskyne/article/details/10418823)

```
assign() 给list赋值 
back() 返回最后一个元素 
begin() 返回指向第一个元素的迭代器 
clear() 删除所有元素 
empty() 如果list是空的则返回true 
end() 返回末尾的迭代器 
erase() 删除一个元素 
front() 返回第一个元素 
get_allocator() 返回list的配置器 
insert() 插入一个元素到list中 
max_size() 返回list能容纳的最大元素数量 
merge() 合并两个list 
pop_back() 删除最后一个元素 
pop_front() 删除第一个元素 
push_back() 在list的末尾添加一个元素 
push_front() 在list的头部添加一个元素 
rbegin() 返回指向第一个元素的逆向迭代器 
remove() 从list删除元素 
remove_if() 按指定条件删除元素 
rend() 指向list末尾的逆向迭代器 
resize() 改变list的大小 
reverse() 把list的元素倒转 
size() 返回list中的元素个数 
sort() 给list排序 
splice() 合并两个list 
swap() 交换两个list 
unique() 删除list中重复的元素
```
