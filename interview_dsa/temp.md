
**AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】**

https://www.acwing.com/solution/content/56348/

挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优     
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1    

https://www.bilibili.com/video/BV1KE411V79h     


scanf(“格式控制字符串”, 地址表列)    
printf("<格式化字符串>", <参量表>)   

```cpp
int a,b,c;
scanf("%d%d%d",&a,&b,&c);
scanf("%5d",&a);   //指定输入宽度
scanf("%ld",&a);
printf("%ld",a);
```


**最短hamilton距离(状态压缩DP 模板题)**

f[i][j] 这里i代表方案结合，每个位置j代表这个点是否被经过过。

状态压缩DP: 方案i --> 位置j-->转移点k

```cpp
#include  <stdio.h>
#include <string.h>
#include <iostream>

using namespace std;

const int N = 20, M = 1 << 20;
int nums[N][N], f[M][N];

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> nums[i][j];
        }
    }
    
    memset(f,0x3f,sizeof(f));
    //i 方案
    f[1][0] = 0;
    for (int i = 0; i < (1 << n); i++) {
        //j 位置
        for (int j = 0; j < n; j++) {
            if ((i >> j) & 1) {
                //枚举到j的点k
                for (int k = 0; k < n; k++) {
                    //i - (1 << j)  --> i ^ (1 << j) 去掉状态j的集合i。每个点只能到达一次，所以想从 k -> j，到达 k 时不可经过 j。
                    if (i - (1 << j) >> k & 1) {
                        f[i][j] = min(f[i][j], f[i - (1 << j)][k] + nums[k][j]);
                    }
                }
            }
        }
    }
    cout << f[(1 << n) - 1][n - 1] <<endl;
}

```

补充笔记：

**AcWing95 费解的开关：**
#include<bits/stdc++.h> 万能头文件    
void *memcpy(void *str1, const void *str2, size_t n) 反向的从str2中拷贝n个到str1。       


**AcWing96 奇怪的汉诺塔：**

n盘3塔：d[n] = d[n-1]*2 + 1    
n盘4塔：f[i] = min(2*f[i] + d[n-i]),i < n,f[1] = 1   

memset(f,0x3f,sizeof(f))   

cin>>x>>y>>w :cin输入多个值


**AcWing99 激光炸弹：**

二维前缀和

**AcWing100 IncDec序列：**

差分序列: A的差分序列是B，B[1]=A[1],B[i]=A[i]-A[i-1](2<=i<=n)        
改成目标为把b2,b3...bn变成全0，最终数列由n个b1构成，这里b[1]=一个常数(b1-b0，实际没有0)。        

**AcWing 101 最高的牛：**

差分：序列A的区间[l,r] + d 等价于序列Bl + d,Br+1 - d。    
牛A和B可以相互看到，则[A+1，B-1]区间的牛高度都-1。
最高的牛的位置肯定是0，因为它左右的牛都看不到。


**AcWing103 电影**

离散化技巧(稀疏->稠密)   

```cpp
//数组的迭代器,end是不到达的点
sort(cinema + 1, cinema + tot + 1);
//unique + find 离散化索引
k = unique(cinema + 1, cinema + tot + 1) - (cinema + 1);

int find(int x) {
    return lower_bound(cinema + 1, cinema + 1 + k, x) - (cinema + 1);
}
```


统计每个科学家的语言，遍历电影去找max1、max2。

**AcWing 104 货仓选址 **

排序 + 中位数       
找到中位数，累加出每个点到中位数的距离之和         

**AcWing 105 七夕祭**

贪心+前缀和+中位数+排序    

只会改变一行的喜爱小摊或者一列的喜爱小摊，而不会同时改变行和列的喜爱小摊。一部分是求行的最少次数，一部分是求列的最少次数。  

贪心参考：环形均分纸牌问题   

行列环形均分纸牌问题   

**AcWing 122 糖果传递**

https://www.acwing.com/solution/content/41677/

均分纸牌问题: ∑ i=1~n−1 |Si|，其中 Si 为 Ai 的前缀和，即Ai = ci - T/M，Si是第i个人要给第i+1个人的牌的数量,ci减去均值的前缀和。    
      
环形均分纸牌问题：破环成链，一定存在一个最优解方案，环上有相邻的两个人之间没有发生交换。    
      
可以直接枚举断点K的位置，再做一遍线性纸牌均分。断点Sk可以直接取排序后的中位数即可，然后使用货仓选择模型计算最小距离 ∑i=1 n |Si−Sk|。    

减去均值的前缀和 + 货仓中位数计算距离

**AcWing 107  超快速排序**

归并排序变形：统计逆序对的数量 

```cpp
while (i <= mid && j <= r) {
	if (a[i] < a[j]) tmp[k++] = a[i++];
	else {
		tmp[k++] = a[j++];
		ans += mid - i + 1; //归并变形，计算逆序对数量
	}
}
```
注：快排的变形是快选，归并的变形是逆序对统计。   

**AcWing 108 奇数码问题**

左右移动完全不改变逆序对个数，而上下移动并不改变逆序对的奇偶性。        
性质：因为两个矩阵,逆序对奇偶性如果不同的话,是肯定不可以变成一样的,这是可以肯定的。       
0是空格,不加入逆序对计算之中。    

ans += mid - i + 1; //归并变形，计算逆序对数量

**AcWing 109 天才ACM**

倍增 + 二路归并优化：https://www.acwing.com/solution/content/82450/

https://www.acwing.com/solution/content/15458/

倍增:从小区间往大区间扩展，效率高于二分。  

```cpp
while (start <= n) {
	int step = 1;
	while (step) {
		//start到end是已经排好序的
		if (end + step <= n && check(start, end, end + step)) {
			end += step;
			step *= 2;
			if (end > n) break;
			//已经校验完成的区间拷贝回去
			for (int i = start; i <= end; i++) {
				b[i] = t[i - start];
			}
		} else {
			step /= 2;
		}
	}
	start = end + 1;
	ans++;
}
```

**AcWing 110 防晒**

贪心一般都有排序

匈牙利算法：如果一个匹配不存在增广路径，则该匹配是二分图的一个最大匹配。    
给每头奶牛匹配一个尽可能大的防晒霜。    

```cpp
//优先满足大的
//区间排序 按照first第一个关键字，second第二关键字排序
sort(cows, cows + n);
//从右往左
int res = 0;
spfS[0] = spfS[1001] = n;
for(int i = n - 1;i >= 0;i--){
	auto iter = spfS.upper_bound(cows[i].second);
	iter--; 
	if(iter->first >= cows[i].first && iter->first <= cows[i].second){
		iter->second--;
		res++;
		if(iter->second == 0) spfS.erase(iter);
	}
}
```

**AcWing 111 畜栏预定**

https://www.acwing.com/solution/content/1060/
 
同一时刻，最大交集的数量。类似于砖墙问题。         

//pair 当结构体使用
pair<PII, int> cows[N];

//堆用来维护区间问题
priority_queue<PII,vector<PII>,greater<PII>> heap;

**AcWing 112 雷达设备**

转换为区间问题：https://www.acwing.com/solution/content/1061/

```cpp
for (int i = 0; i < n; i++) {
	if (a[i].first > end + eps) {
		res++;
		end = a[i].second;
	} else {
		end = min(end, a[i].second);
	}
}
```
	
**AcWing 114 国王游戏**

贪心都有排序，需要制定排序规则，根据区间起点、终点、两数乘积、两数(字符串)之和等

```cpp
//国王不参与排序
sort(nums + 1, nums + n + 1, [](PII & a, PII & b) {
	return a.first * a.second < b.first * b.second;
});
```

高精度乘法模板：乘从低位开始,返回数是反着的。
高精度除法模板：除从高位开始。is_first作flag判首位不能为0


vector比较大小 模板

```cpp
vector<int> max_vec(vector<int> a, vector<int> b) {
    if (a.size() > b.size()) return a;
    if (a.size() < b.size()) return b;
    if (vector<int>(a.rbegin(), a.rend()) > vector<int>(b.rbegin(), b.rend())) return a;
    return b;
}
```

**AcWing115 给树染色**

树的贪心合并，并查集的变形    

每次找出当前权值最大的非根节点，将其染色顺序排在紧随父节点之后的位置，然后将该点合并进父节点中，更新父节点的权值。直到将所有点都合并进根节点为止。    
为了方便计算，每次会将两组点合并，将其中一组点接在另外一组点的后面，比如两组点分别是xi和yi,将yi接在xi之后，则yi中每个点所乘的系数都会增加一个相同的偏移量，这个偏移量就是xi中点的个数，假设是k,
则合并之后，总的权重直接加上 k * Σyi。

```cpp
struct Node {
    int p, s, v; //parent nums value
    double avg;
};
```


**AcWing 116 飞行员兄弟**

构造一个16位的二进制数，二进制数的每一位代表4*4矩阵中的一位，只需要枚举这个16位的二进制数，就可以确定我们的方案。最优解方案，


```Cpp
//矩阵转int表示状态
for (int i = 0; i < N; i++) {
	for (int j = 0; j < N; j++) {
		for (int k = 0; k < N; k++) change[i][j] += (1 << get(i, k)) + (1 << get(k, j));
		change[i][j] -= (1 << get(i, j));
	}
}

//枚举所有的可能
for (int i = 0; i < (1 << 16); i++) {
	for (int j = 0; j < 16; j++) {
		if(i >> j & 1)
	}
}
```

**AcWing 120 防线**

枚举每一个等差数列(起点s,终点e,差为d),如果s <= x，则两区间存在交集，它与[minn,x]的共同区间是[s,min(e,x)],此区间包含数列个数是[(min(e,x)-s)/d] + 1。    
前缀和 + 二分     getSum(l) - getSum(l - 1)     
										       
```cpp
long long get(int x) {
    long long sum = 0;
    for (int i = 0; i < n; i++) {
        if (seqs[i].s <= x) {
            sum += (min(seqs[i].e, x) - seqs[i].s)/ seqs[i].d + 1;
        }
    }
    return sum;
}
```

**AcWing 121 赶牛入圈**

二维前缀和 + 离散化(sort，unique和erase的惯用法 + get(二分)) + 二分判定

sort，unique和erase的惯用法  + get(二分)

unique的功能是去除相邻的重复元素(只保留一个),其实它并不真正把重复的元素删除，是把重复的元素移到后面去了，然后依然保存到了原数组中,返回去重后最后一个元素的地址,unique去除的是相邻的重复元素，所以一般用之前都会要排一下序。
```cpp
//前缀和数组会用到下标0，所有0压入number，让离散化从1开始。
//离散化
nums.emplace_back(0);
sort(nums.begin(), nums.end());
nums.erase(unique(nums.begin(), nums.end()), nums.end());

//枚举len要去掉下边界
while (nums[x2] - nums[x1 + 1] + 1 > len) x1++;
```
    
什么时候用离散化后的索引，什么时候用离散化前的索引，这个要灵活对待。    


**AcWing 123 士兵**

排序、中位数、货仓选址问题扩展      
推导：|X0 - 0 - a|、|Xn- n - a|，这里的a取avg的值。    
	
	
**AcWing 125 耍杂技的牛**

typdef long long LL    
typdef pair<long long> PLL    
typdef pair<double double> PDD     

邻项交换原则: 1 关键字 总和(w + s)排序 2 关键字 w 排序。   

贪心推导： max(①,②) ⩾ max(③,④) ,且Wi+Si >= Wi+1 + Si+1   
	
	
**AcWing 126 最大的和**

二维前缀和简化(4 for) ——> 压缩列方向的前缀和(3 for)。  

涉及前缀和问题、字符串匹配之类的，索引需要从1开始，正常都是从0开始。    

```cpp
//双指针枚举行
for (int i = 1; i <= n; i++) {
	for (int j = i; j <= n; j++) {
		//枚举列
		int last = 0;//last 缓存此次枚举前面的列
		for (int k = 1; k <= n; k++) {
			last = max(last, 0) + a[j][k] - a[i - 1][k];
			ans = max(ans, last);
		}
	}
}
```
				      
**AcWing 127 任务**    

lower_bound支持的迭代器：set、map、multiset、multimap、vector(先sort)

```cpp
//vector 用法
vector<int> nums = { 3,2,4,1,5 };
sort(nums.begin(), nums.end());
auto iter = upper_bound(nums.begin(), nums.end(), 1);

//map 用法
map<int, int> m;
auto iter = m.lower_bound(3);    
```

最小或最优匹配问题

策略可以变成以x从大到小的顺序考虑每一个任务，如果能匹配机器，则从能匹配的机器中选择机器y最小的一个。 
```cpp
while (j >= 0 && machs[j].first >= tasks[i].first) ys.insert(machs[j--].second);
	auto it = ys.lower_bound(tasks[i].second);	
```	
	
**AcWing 128 编辑器**

对顶栈问题：left + right

**AcWing 130 火车进出栈问题**

计算 C(n,2n)/(N+1)：2n!/(n! * (2n - n)! *(n + 1))
	
	
**AcWing132 小组队列**

queue 存tid  + queue数组 ，类似纵向展开的邻接表

```cpp
while(cin >> command,command != "STOP")// while 读取并判断结构    

queue<int> persons[N] //queue队列
```
	
**AcWing 133 蚯蚓**

三个队列模拟优先队列，    
q1：切后第一段； q2：切后第二段； q3:存储蚯蚓长度    
排序：从高到低。   
每次取各个队头并求max       
每个队列都隐藏着单调队列的性质。    	
	
**AcWing 134 双端队列** 

https://www.acwing.com/solution/content/27971/

单谷性质：先单调递减，后单调递增，这一段可以对应原问题的双端队列。用一个变量记录当前拼成的序列末尾处于递增还是递减状态。 
递减的一段相当于从队头插入，递增的一段相当于从队尾插入。
如果A中存在几个相等的数，那么这几个数在排序时是不定的，我们可以任意交换他们的顺序，使得B数组能够分成更少的段数。
dir:1变成-1 会统计数量。

```cpp
if (last < minx) last = maxx;
else
{
	res ++ ;
	last = minx;
	dir = -1;
}
```

