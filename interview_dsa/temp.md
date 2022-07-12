
**AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】**

https://www.acwing.com/solution/content/56348/

挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优     
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1    

https://www.bilibili.com/video/BV1KE411V79h     




- AcWing291 蒙德里安的梦想-

https://www.acwing.com/solution/content/28088/

DP的过程随着阶段的增长，在各个状态维度不断扩张，在任意时刻，已经求出最优解的状态与尚未求出最优解的状态在各个维度上的分界点组成了DP扩展的轮廓。     
对于某些问题，需要在动态规划的状态中记录一个集合，保存轮廓的详细信息，便于进行状态转移。     
若集合大小不超过N,集合中每个元素都是小于K的自然数，则可以把这个集合看成N位K进制数，以一个[0,k^N-1]之间的十进制整数的形式作为DP一维。     
这种把集合转化为整数记录在DP状态中的一类算法，叫做状态压缩DP。     

F[i][j]表示第i行的形态为j时，前i行分割方案的总数。j是十进制表示的M位二进制数，第k位为1表示第k列是一个竖着的1*2长方形的上面一半，第k位为0是其他情况。    
转移方程：F[i,j] = Σ F[i-1,k], j&k = 0，j|k 有偶数个0。

一般都需要预处理

状态压缩DP模板更新

```cpp
const int N = 12, M = 1 << N;
LL f[N][M];
bool st[M]; //是否有偶数个连续的0,有则为true
vector<int> state[M]; //预存储合法状态

//预处理1 判断i是否有偶数个连续的0
for (int i = 0; i < (1 << n); i++) {
	bool isvalid = true; // 是否有偶数个连续的0，有则有效。
	int cnt = 0;
	for (int j = 0; j < n; j++) {
		if (i >> j & 1) {
			if (cnt & 1) {
				isvalid = false;
				break;
			}
			cnt = 0; //开始下一段
		} else cnt++;
	}
	if (cnt & 1) isvalid = false;
	st[i] = isvalid;
}

//预处理2 判断i-2列和i-1列是否冲突
for (int i = 0; i < 1 << n; i++) {
	state[i].clear();
	for (int k = 0; k < 1 << n; k++) {
		if ((i & k) == 0 && st[i | k]) {
			state[i].emplace_back(k);
		}
	}
}

//dp开始
memset(f, 0, sizeof(f));
f[0][0] = 1;
for (int i = 1; i <= m; i++) {
	for (int j = 0; j < (1 << n); j++) {
		for (auto k: state[j]) {    //遍历合法的转移
			f[i][j] += f[i - 1][k]; //方案数
		}
	}
}

```

vector<int> state[M]：vector和数组嵌套使用。

- AcWing292 炮兵阵地

https://www.acwing.com/solution/content/12392/ 

S集合:相邻两个1的距离不小于3的所有M位二进制数,代表两个炮兵的距离不能小于3。     
count:M位2进制中1的个数。     
valid(i,x):M位2进制数x属于S,且x中的每个1对应地图中的第i行中的位置都是平原,则能摆下炮兵。  
  
压缩存储两层的信息，然后枚举合法的i-2层状态进行转移。  

状态表示：f[i][j][k]:第i层状态是j, i-1层状态是k,该方案前i行能摆下多少个炮兵。     
状态计算：f[i][j][k] = max f[i-1][k][pre] + cnt[j], valid(i,j) valid(i-1,k)，并且j&k = 0。     
pre是枚举的能够与k和j合法存在于三行中的所有状态.      


```cpp
bool check(int s){
    for(int i = 0;i < m;i++){
        if ((s >> i & 1) && ((s >> i + 1 & 1) || (s >> i + 2 & 1))) {
            return false;
        }
    }
    return true;
}

int count(int s) {
    int res = 0;
    while (s) {
        res += s & 1;
        s >>= 1;
    }
    return res;
}


//预处理合法状态,连续三位不能都是1(1表示放一个炮兵)
for (int i = 0; i < 1 << m; i++) {
	if (check(i)) {
		state.emplace_back(i);
		cnt[i] = count(i); // 1的个数，即可以放炮兵的数量
	}
}

//状态计算
// i状态j,i-1状态是k,i-2状态是u.
for (int i = 0; i < n + 2; i++) {
	for (int j = 0; j < state.size(); j++) {
		for (int k = 0; k < state.size(); k++) {
			for (int u = 0; u < state.size(); u++) {
				int a = state[u], b = state[k], c = state[j];
				if (a & b || b & c || a & c) continue;
				if (g[i] & c) continue;
				f[i & 1][j][k] = max(f[i & 1][j][k], f[i - 1 & 1][k][u] + cnt[c]);
			}
		}
	}
}
cout << f[n + 1 & 1][0][0] << endl;
```


- AcWing298 围栏

https://www.acwing.com/solution/content/2130/

单调队列优化适合决策取值范围的上下边界均单调编号，每个决策在候选集合中插入或删除至多一次的情况。     
f[i][j] 表示安排前i个工匠粉刷前j块木板(木板可以空着不刷)，能获得的最大报酬。         
f[i][j] = max {f[i-1][k] + Pi(j-k)} ， j - Li <= k <= Si - 1          
在考虑内层循环j和决策k时，把外层循环变量i看作定值。     
f[i][j] = Pi*j + max{f[i-1][k] - Pi*k} ， j - Li <= k <= Si - 1     

求max用单调递减队列(可以取队头)，求min用单调递增队列。  
维护一个随着决策点k单调递增，f[i-1][k] - Pi*k的递减序列。

```cpp
sort(car + 1,car + 1 + m);
for (int i = 1; i <= m; i++) {
	q.clear();

	//对于每个i填充对应的一段j
	for (int j = 0; j <= n; j++) {
		f[i][j] = f[i - 1][j];
		if (j) f[i][j] = max(f[i][j], f[i][j - 1]);

		int l = car[i].l, s = car[i].s, p = car[i].p;
		//超出元素出队
		if (q.size() && q.front() < j - l) q.pop_front();
		if (j >= s && q.size()) {
			//取队头进行状态转移
			int k = q.front();
			f[i][j] = max(f[i][j], f[i-1][k] + p * (j - k));
		}

		if (j < s) {
			//维护队尾的单调性
			while (q.size() && f[i - 1][q.back()] - p * q.back() <= f[i - 1][j] - p * j) q.pop_back();
			q.push_back(j);
		}
	}
}
```
				  
				  

- AcWing299 裁剪序列














