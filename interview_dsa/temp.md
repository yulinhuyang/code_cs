
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





