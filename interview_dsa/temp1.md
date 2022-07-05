

**AcWing 257 关押罪犯**

https://www.acwing.com/solution/content/3042/  

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 20010, M = 200010;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int color[N];

void add(int a, int b, int c)
{
   e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool dfs(int u, int c, int mid)
{
   color[u] = c;
   for (int i = h[u]; ~i; i = ne[i])
   {
       int j = e[i];
       if (w[i] <= mid) continue;
       if (color[j])
       {
           if (color[j] == c) return false;
       }
       else if (!dfs(j, 3 - c, mid)) return false;
   }

   return true;
}

bool check(int mid)
{
   memset(color, 0, sizeof color);
   for (int i = 1; i <= n; i ++ )
       if (!color[i])
           if (!dfs(i, 1, mid))
               return false;
   return true;
}

int main()
{
   scanf("%d%d", &n, &m);
   memset(h, -1, sizeof h);

   while (m -- )
   {
       int a, b, c;
       scanf("%d%d%d", &a, &b, &c);
       add(a, b, c), add(b, a, c);
   }

   int l = 0, r = 1e9;
   while (l < r)
   {
       int mid = l + r >> 1;
       if (check(mid)) r = mid;
       else l = mid + 1;
   }

   printf("%d\n", r);

   return 0;
}

```



**AcWing 239 奇偶游戏**

https://www.acwing.com/solution/content/29308/

S[L~R]偶数个1，等价于 L-1 ~ R奇偶性相同     
X拆分成 Xodd(sum[x]是奇数) 和 Xeven(sum[x]是偶数)     
ans = 0: 合并 Xodd和Yodd，Xeven和Yeven       
ans = 1: 合并 Xodd和Yeven，Xeven和Yodd 


扩展域    

```cpp
a = get(a - 1), b = get(b);
if (type == "even") {
	if (find(a + Base) == find(b)) {
		res = i - 1;
		break;
	}
	p[find(a)] = find(b);
	p[find(a + Base)] = find(b + Base);
} else {
	if (find(a) == find(b)) {
		res = i - 1;
		break;
	}
	p[find(a + Base)] = find(b);
	p[find(a)] = find(b + Base);
}
```


**AcWing240 食物链**

https://www.acwing.com/solution/content/15938/

https://www.acwing.com/solution/content/1357/

边带权 

d[i]:i到父节点的距离，不是根节点的距离。 
路径压缩：查询某个节点i时，如果i的父节点不为根节点的话，就会进行递归调用，将i节点沿途路径上所有节点均指向父节点。
路径压缩前，每段权值是1，递归find压缩后，每段都指向父节点，权值根据关系不同而不同。
 
x, y是同类的话的d[x]与d[y]模三同余（路径压缩后根节点就是父节点）  
x吃y的话的d[x] - 1与d[y]模三同余   
y吃x的话的d[x] + 1与d[y]模三同余   

向量理解：d[px] = d[y] + d[x->y] - d[x]

```cpp
int px = find(x);
int py = find(y);
if (t == 1) {
	if (px == py && (d[x] - d[y]) % 3) res++; //自己
	else if (px != py) {
		p[px] = py; //合并父节点
		d[px] = d[y] - d[x];
	}
} else {
	if (px == py && (d[x] - 1 - d[y]) % 3) res++; //自己
	else if (px != py) {
		p[px] = py; //合并父节点
		d[px] = d[y] - d[x] + 1;
	}
}
```

**AcWing 271 杨老师的照相排列**

https://www.acwing.com/solution/content/4954/

5维DP，闫氏DP以最后一个同学被安排在哪一排作为划分依据。

```cpp
for (int a = 0; a <= s[0]; a++) {
	for (int b = 0; b <= min(a, s[1]); b++) {
		for (int c = 0; c <= min(b, s[2]); c++) {
			for (int d = 0; d <= min(c, s[3]); d++) {
				for (int e = 0; e <= min(d, s[4]); e++) {
					int &x = f[a][b][c][d][e];
					if (a && a - 1 > b) x += f[a - 1][b][c][d][e];
					if (b && b - 1 > c) x += f[a][b - 1][c][d][e];
					if (c && c - 1 > d) x += f[a][b][c - 1][d][e];
					if (d && d - 1 > e) x += f[a][b][c][d - 1][e];
					if (e) x += f[a][b][c][d][e - 1];
				}
			}
		}
	}
}
```

