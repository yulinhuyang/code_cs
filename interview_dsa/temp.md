
**AcWing 1064. 小国王【线性状压DP+滚动数组优化+目标状态优化】**

https://www.acwing.com/solution/content/56348/

挑赛滑动窗口叫做虫取法，滑动窗口两个指针的移动过程很像。

贪心：局部最优=全局最优     
模拟是指根据题意要求实现功能，通常操作多，代码量大，无复杂算法，考察熟练程度。

https://ac.nowcoder.com/acm/archive/oi-advance?pageSize=10&page=1    

https://www.bilibili.com/video/BV1KE411V79h     



**AcWing 164 可达性统计**

拓扑排序模板题 +状态压缩(bitset) 模板题

广度优先遍历队列中的元素关于层次满足两段性和单调性 ，
1 在访问完所有第i层的节点后，才会访问第i+1层节点 2 任意时候，队列中最多有两个层次的节点(i层和i+1层)。   

https://www.acwing.com/solution/content/32117/

从点x出发的点构成的集合是f(x),则f(x) = {x} U ( Uf(y),存在有向边(x,y))，也就是x的各个后续节点y出发能到达点的并集，再加上x自身。
先求拓扑序，然后按照拓扑序倒序计算。拓扑排序中，对于任意一条边(x,y),x都在y之前。  


bitset:|、cout()

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

	

		





			     
   
		   

				
				

	

