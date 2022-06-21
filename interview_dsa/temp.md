
**AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】**

https://www.acwing.com/solution/content/56348/

挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优     
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1    

https://www.bilibili.com/video/BV1KE411V79h     



**AcWing 164 可达性统计**

拓扑排序模板题 +状态压缩(bitset) 模板题

拓扑排序:

广度优先遍历队列中的元素关于层次满足两段性和单调性:         
1 在访问完所有第i层的节点后，才会访问第i+1层节点        
2 任意时候，队列中最多有两个层次的节点(i层和i+1层)。           

https://www.acwing.com/solution/content/32117/

从点x出发的点构成的集合是f(x),则f(x) = {x} U ( Uf(y),存在有向边(x,y))，也就是x的各个后续节点y出发能到达点的并集，再加上x自身。   
先求拓扑序，然后按照拓扑序倒序计算。拓扑排序中，对于任意一条边(x,y),x都在y之前。      

bitset: 支持 |、cout()

```cpp
bitset<N> f[N]; //N位的bitset对象 构成的数组

for (int i = n - 1; i >= 0; i--) {
	int j = seq[i];
	f[j][j] = 1;  // j这个点可以到达自己   f[j][j] =表示从 j出发的点，
				  // 能够到的点（1表示可以到， 0表示不能到），j可以到自己，
				  // 因此f[j][j]=1
	for (int k = h[j]; ~k; k = ne[k]) {  //所有能到到达的点
		f[j] |= f[e[k]];  // j这个点可以到达的点的数量= {j} U {y1} U {y2}
						  // ... {yn}
	}
}

for (int i = 1; i <= n; i ++ ) cout << f[i].count() << endl;
```

```cpp
bitset<16> third("1110011");  //将二进制字符串初始化到对象中
cout << third.count() << endl;  //统计bitset里面1的位数
cout << first.to_string() << endl;   //以二进制字符串形式输出，将所有二进制位输出

```

**AcWing 165 小猫爬山** 

https://www.acwing.com/solution/content/32118/

优化搜索顺序：贪心降序排列，先运大的猫     
可行性剪枝： if (cab[i] + cat[now] <= w)  不超过车承重     
最优性剪枝： if (cnt >= ans)  return          	


**AcWing 166 数独**

数独 舞蹈链解法 Dancing links解法：https://www.acwing.com/solution/content/3843/
位运算 + dfs解法：https://www.acwing.com/solution/content/31873/

打表: 查表法是一种在某些条件下简化算法的办法,通过打表技巧获得一个有序表或常量表。  

位为1表示可选，待填充；为0表示已填充。  

```cpp
//标记数组
int row[N], col[N], cell[3][3];/
对每行、列和九宫格，分别用一个9位二进制数保持哪些数字可以填，对应位是1，该数可以填，回溯恢复现场修改。 

//打表
for (int i = 0; i < N; i++) {
	map[1 << i] = i; //打表，快速知道是哪一个数字
}
for (int i = 0; i < 1 << N; i++) {
	int s = 0;
	for (int j = i; j; j -= lowBit(j)) s++;
	ones[i] = s; //记录每个状态有多少个1
}


//位运算的映射与反映射
row[i] = col[i] = (1 << N) - 1;  //初始化为全1
int t = str[k] - '1'; 
row[i] -= 1 << t;    //注意第4位对应1 << 3   
```

优化搜索顺序：dfs优先选一个1的个数最少的，这样的分支数量最少。依次做lowbit操作，选择每个分支。   
可行性剪枝：if (dfs(cnt - 1)) return true;   
排除等效冗余：避免重复遍历若干棵覆盖同一状态空间的等效搜索树。   
		


**AcWing167 木棍**

https://www.acwing.com/solution/content/36030/

优化搜索序列:优先选择较长的木棒
排除等效冗余：
1 先后加入的木棒具有单调性  
2 对于当前木棒，如果拼接失败，不能再尝试和他等长的木棒。
3 第一次尝试拼入木棒就递归失败，那么后面必然失败。
4 如果最后一个木棒失败，则一定失败。   

截止条件：双阈值，u 组成的木棍数量，cur 已经拼接的长度  


```cpp
bool dfs(int u, int cur, int start) {
    if (u * length == sum) return true;
    if (cur == length) return dfs(u + 1, 0, 0);

    for (int i = start; i < n; i++) {
        if(st[i]) continue;
        int l = sticks[i];
        if (cur + l <= length) {
            st[i] = true;
            if (dfs(u, cur + l, i + 1)) return true;
            st[i] = false;
            if (!cur) return false; //第一次尝试失败，后面也会失败
            if (cur + l == length) return false; //最后一根失败了，也会失败
            int j = i;
            while (j < n && sticks[j] == l) j++; //等长的木棍都会失败，跳过
            i = j - 1;
        }
    }
    return false;
}
```

**AcWing 168 生日蛋糕**  

https://www.acwing.com/solution/content/31876/

https://img-blog.csdnimg.cn/20210127003014554.png

总体积n，层数m，表面积S最小

优化搜索顺序(二维,影响性大到小原则)：层间，从下到上;层内，先枚举半径再枚举高(R影响大)，半径由大到小，高度由大到小。   
第u层： 
R:  u <= Ru <= min{Ru+1−1 ，(int)sqrt(n - v)}
H:  u <= Hu <= min{Hu+1−1， (n - v) / r / r)}
推导：根据表面积和体积关系，推导 S + 2(n−V)/Ru+1 >= Sans 剪枝  
最优性剪枝：上面(1~u-1) + 下面(u ~m) < res(面积最优解)


```cpp
void dfs(int u, int v, int s) {
    if (v + minv[u] > n) return;
    if (s + mins[u] >= ans) return;
    if (s + 2 * (n - v) / R[u + 1] >= ans) return;

    if (!u) {
        if (v == n) ans = s;
        return;
    }
    //枚举r ,h取最小u
    for (int r = min(R[u + 1] - 1, (int)sqrt(n - v)); r >= u; r--) {
        for (int h = min(H[u + 1] - 1, (n - v) / r / r); h >= u; h--) {
            int t = 0;
            if (u == m) t = r * r;
            R[u] = r;
            H[u] = h;
            dfs(u - 1, v + r * r * h, s + 2 * r * h + t);
        }
    }
}


```


**AcWing 170 加成序列**

迭代加深:在当前深度限制下搜不到答案，就把深度限制增加，重新进行一次搜索。先搜索浅层，再搜索深层。
     
https://www.acwing.com/solution/content/6859/   

迭代加深+dfs(双指针+剪枝)

```cpp
for (int i = u - 1; i >= 0; i -- )
	for (int j = i; j >= 0; j -- )
	{
		int s = path[i] + path[j];
		if (s > n || s <= path[u - 1] || st[s]) continue;
		st[s] = true;

		path[u] = s;
		if (dfs(u + 1, k)) return true;
	}
```


**AcWing 171 送礼物**

双向搜索：从初态和终态出个各搜索一半的状态，产生两棵深度减半的搜索树，在中间交会、合成最终的答案。 

https://www.acwing.com/solution/content/38250/

dfs1先搜索出前一半礼物(1 ~ N/2 + 2)选出若干，得到 0~W 之间，放入数组A。
dfs2再从后一半礼物(N/2 + 3 ~ N)中选出一些，达到的重量是t,数组A中二分出来一个 <= W - t 最大的，用两者和更新答案。

```cpp
void dfs2(int u, int s) {
    //截止条件
    if (u == n) {
        int l = 0, r = cnt - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (weights[mid] + (long long )s <= m) l = mid;
            else r = mid - 1;
        }
        if (weights[l] + (long long) s <= m) ans = max(ans, weights[l] + s);
        return;
    }
    //不选
    dfs2(u + 1, s);

    //选
    if ((long long) s + g[u] <= m) {
        dfs2(u + 1, s + g[u]);
    }
}
```
			     
**AcWing 172 立体推箱子**

https://www.acwing.com/solution/content/16264/

箱子共有三种状态：立着，记录为0; 竖躺着，记录为 1; 横躺着，记录为 2  

坐标记录：
对于立着的，坐标记为其所立的位置。
对于竖躺着的，坐标记为其靠上的方块的坐标。
对于横躺着的，坐标记为其靠左的方块的坐标。

广搜之前记录下三个状态扩展的偏移量。 

```cpp
dx[3][4] = {       // x 坐标的偏移量
    {-2, 0, 1, 0}, // 0 向上扩展，x - 2，向左 x 不变，向下 x + 1，向右 x 不变
    {-1, 0, 2, 0}, // 1，同上
    {-1, 0, 1, 0}  // 2
}, dy[3][4] = {    // y 坐标的偏移量
    {0, -2, 0, 1},
    {0, -1, 0, 1},
    {0, -1, 0, 2}
}, dl[3][4] = {    // lie 的偏移量
    {1, 2, 1, 2},
    {0, 1, 0, 1},
    {2, 0, 2, 0}
};
```

   
		   

				
				

	

