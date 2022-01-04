
# 常用代码模板5——DFS、动态规划

## 5.1 递归回溯

教程:  [[递归与回溯的理解]](https://cloud.tencent.com/developer/article/1434886)  [[手把手教怎么写递归和回溯]](https://leetcode-cn.com/circle/article/GV6eQ2/)   

递归回溯一般模板：
```cpp
'''
backtracking使用dfs的模板，基本跟dfs的模板一模一样
'''
class Backtracking(object):

    def backtracking(self, input):

        self.res = []

        def dfs(input, temp, [index]):
            # 边界
            if 非法数据：
                return

            # 终止条件
            if len(input) == len(temp)：
                self.res.append(temp[:])
                return

            # for循环
            for i range(len(input)):
                ##1. 修改path
                temp.append(input[i])
                ##2. backtracking
                dfs(input, temp, [index])
                ##3. 退回原来状态，恢复path
                temp.pop()
        # 执行
        dfs(input, [], 0)
        return self.res
```

- [x] (中等)Leetcode 17 电话号码的字母组合[[problem]](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
- [x] (中等)Leetcode 22 括号生成[[problem]](https://leetcode-cn.com/problems/generate-parentheses/)
- [x] (中等)Leetcode 46 全排列[[problem]](https://leetcode-cn.com/problems/permutations/)
- [x] (中等)Leetcode 47 全排列II[[problem]](https://leetcode-cn.com/problems/permutations-ii/)  

## 5.2 DFS

**算法描述**  
DFS的算法具体描述为选择一个起点v作为当前节点，执行如下操作：  
a. 访问当前节点，并且标记当前节点已被访问过，然后跳转到b；  
b. 如果存在一个和**当前节点**相邻并且尚未被访问的节点u，则将节点u设为当前节点，继续执行a；  
c. 如果不存在这样的u，则进行回溯，回溯的过程就是回退**当前节点**；  
上述**当前节点**需要用一个栈来维护，每次访问到得到节点入栈，回溯的时候出栈（也可以用递归来实现，更佳直观和方便）。    

- DFS实现  
```python
def DFS(v):
    vistited[v]=true
    dosomething()
    for u in adjcent_list[v]:
        if visited[v] is false:
            DFS(u) 
//其中dosomething 表示访问时具体要干的事，根据情况而定，并DFS是可以有返回值的；
```

**基础应用**   
- 求N的阶乘；   
```cpp
//f(n)=n*f(n-1), n>0
```
- 求斐波那契数列的第N项  
```cpp
//f(n)=f(n-1)+f(n-2), n>2,f(0)=f(1)=1  
//记忆化搜索
```
- 求n个数的全排列   
```cpp
//建立全连接图
```

**DFS高级应用**    
- 枚举    
数据较小的排列、组合的穷举；  
- 容斥原理  
利用深搜计算一个公式，本质上还是枚举；  	
- 基于状态压缩的动态规划    
一般解决棋盘摆放问题，k进制表示状态，然后利用深搜进行状态转移；  
- 记忆化搜索  
某个状态被计算过了，就将它cache住，下次要用到的时候就不需要再一次计算；  
- 有向图的强连通分量   
经典的Tarjan算法；  
求解2-sat问题的基础；   
- 无向图割边割点和双连通分量  
经典的Tarjan算法；  
- LCA   
最近公共祖先递归求解；  
- 博弈  
利用深搜计算SG值；   
- 二分图最大匹配  
经典的匈牙利算法；  
最小顶点覆盖，最大独立集，最小值支配集向二分图的转化；  
- 欧拉回路  
经典的圈套圈算法；  
- K短路   
依赖数据，数据不卡的话可以采用二分答案+深搜或者广搜+A*;  
- 线段树   
二分经典思想，配合枚举深搜左右子树；  
- 最大团  
极大完全子图的优化算法；  
- 最大流   
EK算法求任意路径中有涉及；  
- 树形DP   
即树形动态规划；父节点的值由各子节点计算得出；  


### 5.2.1 基于DFS的记忆化搜索  

### 5.2.2 基于DFS的剪枝    
好的剪枝可以大大提升程序的运行效率，那么问题来了，如何进行剪枝？我们先来看剪枝需要满足什么原则：  
a. 正确性  
剪掉的子树中如果存在可行解（或最优解），那么在其它的子树中很可能搜不到解导致搜索失败，所以剪枝的前提必须是要正确；  
b. 准确性  
剪枝要“准”。所谓“准”，就是要在保证在正确的前提下，尽可能多得剪枝。  
c. 高效性  
剪枝一般是通过一个函数来判断当前搜索空间是否是一个合法空间，在每个结点都会调用到这个函数，所以这个函数的效率很重要。    

- 可行性剪枝  
可行性剪枝一般是处理可行解的问题，如一个迷宫，问能否从起点到达目标点之类的。


- 最优化剪枝    
最优性剪枝一般是处理最优解的问题。以求两个状态之间的最小步数为例，搜索最小步数的过程：一般情况下，需要保存一个“当前最小步数”，这个最小步数就是当前解的一个下界d。在遍历到搜索树的叶子结点时，得到了一个新解，与保存的下界作比较，如果新解的步数更小，则令它成为新的下界。搜索结束后，所保存的解就是最小步数。而当我们已经搜索了k歩，如果能够通过某种方式估算出当前状态到目标状态的理论最少步数s时，就可以计算出起点到目标点的理论最小步数，即估价函数h = k + s，那么当前情况下存在最优解的必要条件是h < d，否则就可以剪枝了。最优性剪枝是不断优化解空间的过程。     


### 5.2.3 基于DFS的A*算法 (迭代加深IDA*) 
**1) 算法原理**  
迭代加深分两步走：  
- 枚举深度。  
- 根据限定的深度进行DFS，并且利用估价函数进行剪枝。  

### 5.2.4 DFS题集 

**ACM DFS**:    
- [x] [Red and Black](http://poj.org/problem?id=1979)        ★☆☆☆☆   FloodFill

- [ ] [The Game](http://poj.org/problem?id=1970)          ★☆☆☆☆   FloodFill

- [ ] [Frogger](http://poj.org/problem?id=2253)           ★☆☆☆☆   二分枚举答案 + FloodFill  

- [ ] [Nearest Common Ancestors](http://poj.org/problem?id=1330)  ★☆☆☆☆   最近公共祖先 

- [ ] [Robot Motion](http://poj.org/problem?id=1573)        ★☆☆☆☆   递归模拟

- [ ] [Dessert](http://poj.org/problem?id=1950)           ★☆☆☆☆   枚举

- [ ] [Matrix](http://poj.org/problem?id=2078)           ★☆☆☆☆   枚举

- [ ] [Frame Stacking](http://poj.org/problem?id=1128)       ★☆☆☆☆   枚举

- [ ] [Transportation](http://poj.org/problem?id=1040)       ★☆☆☆☆   枚举

- [ ] [Pairs of Integers](http://poj.org/problem?id=1117)          ★★☆☆☆   枚举


- [ ] [Machine Schedule](http://poj.org/problem?id=1325)           ★★★☆☆  二
**IDA\***(确定是迭代加深后就一个套路，枚举深度，然后 暴力搜索+强剪枝)
- [ ] [Addition Chains](http://poj.org/problem?id=2248)            ★★☆☆☆   

- [ ] [DNA sequence](http://acm.hdu.edu.cn/showproblem.php?pid=1560)        ★★☆☆☆   

- [ ] [Booksort](http://poj.org/problem?id=3460)                   ★★★☆☆  

- [ ] [The Rotation Game](http://acm.hdu.edu.cn/showproblem.php?pid=1667)   

## 5.3. BFS
BFS的具体算法描述为选择一个起始点v放入一个先进先出的队列中，执行如下操作：    
a. 如果队列不为空，弹出一个队列首元素，记为当前结点，执行b；否则算法结束；  
b. 将与 当前结点 相邻并且尚未被访问的结点的信息进行更新，并且全部放入队列中，继续执行a；  
维护广搜的数据结构是队列和HASH，队列就是官方所说的open-close表，HASH主要是用来标记状态的，比如某个状态并不是一个整数，可能是一个字符串，就需要用字符串映射到一个整数，可以自己写个散列HASH表，不建议用STL的map，效率奇低。   


**算法实现**  
广搜一般用队列维护状态，写成伪代码如下：  
```python
def BFS(v):
    resetArray(visited,false)
    visited[v] = true
    queue.push(v)
    while not queue.empty():
        v = queue.getfront_and_pop()
        for u in adjcent_list[v]:
            if visited[u] is false:
                dosomething(u)
                queue.push(u)
```

**基础应用**    
-  最短路：bellman-ford最短路的优化算法SPFA，主体是利用BFS实现的。    
绝大部分四向、八向迷宫的最短路问题。     
- 拓扑排序：  
首先找入度为0的点入队，弹出元素执行“减度”操作，继续将减完度后入度为0的点入队，循环操作，直到队列为空，经典BFS操作；   
- FloodFill：  
经典洪水灌溉算法；  


**高级应用**  
- 差分约束：   
数形结合的经典算法，利用SPFA来求解不等式组。  
- 稳定婚姻：   
二分图的稳定匹配问题，试问没有稳定的婚姻，如何有心思学习算法，所以一定要学好BFS啊；                
- AC自动机：   
字典树 + KMP + BFS，在设定失败指针的时候需要用到BFS。     
详细算法参见：http://www.cppblog.com/menjitianya/archive/2014/07/10/207604.html  
- 矩阵二分：   
矩阵乘法的状态转移图的构建可以采用BFS；   
- 基于k进制的状态压缩搜索：   
这里的k一般为2的幂，状态压缩就是将原本多维的状态压缩到一个k进制的整数中，便于存储在一个一维数组中，往往可以大大地节省空间，又由于k为2的幂，所以状态转移可以采用位运算进行加速，HDU1813和HDU3278以及HDU3900都是很好的例子；   

### 5.3.1 基于BFS的A*算法  
在搜索的时候，结点信息要用堆（优先队列）维护大小，即能更快到达目标的结点优先弹出。



### 5.3.2 BFS题集

- [ ] [Pushing Boxes](http://poj.org/problem?id=1475)        ★☆☆☆☆   经典广搜 - 推箱子

- [ ] [Jugs](http://poj.org/problem?id=1606)            ★☆☆☆☆   经典广搜 - 倒水问题

- [ ] [Space Station Shielding](http://poj.org/problem?id=1096)  ★☆☆☆☆   FloodFill

- [ ] [Knight Moves](http://poj.org/problem?id=1915)        ★☆☆☆☆   棋盘搜索

- [ ] [Knight Moves](http://poj.org/problem?id=2243)        ★☆☆☆☆   棋盘搜索    

- [ ] [Eight](http://poj.org/problem?id=1077)            ★★☆☆☆   经典八数码

- [ ] [Currency Exchange](http://poj.org/problem?id=1860)      ★★☆☆☆   SPFA

- [ ] [The Postal Worker Rings](http://poj.org/problem?id=1237)  ★★☆☆☆   SPFA


### 5.3.3 双向广搜  (适用于起始状态都给定的问题，一般一眼就能看出来，固定套路，很难有好的剪枝)

初始状态 和 目标状态 都知道，求初始状态到目标状态的最短距离;    
利用两个队列，初始化时初始状态在1号队列里，目标状态在2号队列里，并且记录这两个状态的层次都为0，然后分别执行如下操作：   
a.若1号队列已空，则结束搜索，否则从1号队列逐个弹出层次为K(K >= 0)的状态；    
i.  如果该状态在2号队列扩展状态时已经扩展到过，那么最短距离为两个队列扩展状态的层次加和，结束搜索；   
ii. 否则和BFS一样扩展状态，放入1号队列，直到队列首元素的层次为K+1时执行b；   
b.若2号队列已空，则结束搜索，否则从2号队列逐个弹出层次为K(K >= 0)的状态；   
i.  如果该状态在1号队列扩展状态时已经扩展到过，那么最短距离为两个队列扩展状态的层次加和，结束搜索；   
ii. 否则和BFS一样扩展状态，放入2号队列，直到队列首元素的层次为K+1时执行a；    


- [ ] [Solitaire](http://poj.org/problem?id=1198)         ★★★☆☆

- [ ] [A Game on the Chessboard](http://poj.org/problem?id=1735)  ★★★☆☆

- [ ] [魔板](http://acm.hdu.edu.cn/showproblem.php?pid=1430)            ★★★★☆
- [ ] [Tobo or not Tobo](http://acm.hdu.edu.cn/showproblem.php?pid=2918)      ★★★★☆

- [ ] [Eight II](http://acm.hdu.edu.cn/showproblem.php?pid=3567)          ★★★★★


## 5.4 动态规划 DP

[夜深人静写算法——动态规划](http://www.cppblog.com/menjitianya/archive/2015/10/23/212084.html)  

[闫氏dp分析法](https://blog.csdn.net/qq_39782006/article/details/105420831)

[【紫书第九章】动态规划(DP)常见模型汇总](https://blog.csdn.net/qq_49688477/article/details/119569267)

### 5.4.1 动态规划初探

#### 1、递推   

- 5.最长回文子串 [[problem]](https://leetcode-cn.com/problems/longest-palindromic-substring/)  
- 62.不同路径 [[problem]](https://leetcode-cn.com/problems/unique-paths/)  
- 63.不同路径 II [[problem]](https://leetcode-cn.com/problems/unique-paths-ii/)  


#### 2、记忆化搜索   
递推说白了就是在知道前i-1项的值的前提下，计算第i项的值，而记忆化搜索则是另外一种思路。它是直接计算第i项，需要用到第 j 项的值( j < i)时去查表，如果表里已经有第 j 项的话，则直接取出来用，否则递归计算第 j 项，并且在计算完毕后把值记录在表中。记忆化搜索在求解多维的情况下比递推更加方便。  

- [x] 87. 扰乱字符串 [[problem]](https://leetcode-cn.com/problems/scramble-string/)     



- [ ] Function Run Fun  [[proble]](http://poj.org/problem?id=1579)                              ★☆☆☆☆          【例题3】

- [ ] FatMouse and Cheese  [[problem]](http://acm.hdu.edu.cn/showproblem.php?pid=1078)                            ★☆☆☆☆          经典迷宫问题

- [ ] Cheapest Palindrome  [[problem]](http://poj.org/problem?id=3280)                           ★★☆☆☆ 

- [ ] A Mini Locomotive    [[problem]](http://poj.org/problem?id=1976)                           ★★☆☆☆

- [ ] Millenium Leapcow    [[problem]](http://poj.org/problem?id=2111)                           ★★☆☆☆

- [ ] Brackets Sequence   [[problem]](http://poj.org/problem?id=1141)                            ★★★☆☆          经典记忆化

- [ ] Chessboard Cutting  [[problem]](http://poj.org/problem?id=1191)                            ★★★☆☆          《算法艺术和信息学竞赛》例题

- [ ] Number Cutting Game  [[problem]](http://acm.hdu.edu.cn/showproblem.php?pid=2848)                         ★★★☆☆

#### 3、状态和状态转移 
#### 4、最优化原理和最优子结构
#### 5、决策和无后效性

### 5.4.2 动态规划的经典模型

#### 1、线性模型   
- [x] 72. 编辑距离  [[problem]](https://leetcode-cn.com/problems/edit-distance/)     
- [x] 85. 最大矩形 [[problem]](https://leetcode-cn.com/problems/maximal-rectangle/)  [[直方图O(N\*N\*M]]  [[DP O(N\*M)]]  
- [x] 91. 解码方法 [[problem]](https://leetcode-cn.com/problems/decode-ways/)



#### 2、区间模型
#### 3、背包模型
#### 5、树状模型

### 5.4.3 动态规划的常用状态转移方程
#### 1、1D/1D
#### 2、2D/0D
#### 3、2D/1D
#### 4、2D/2D

### 5.4.4 动态规划和数据结构结合的常用优化
#### 1、滚动数组
#### 2、最长单调子序列的二分优化
#### 3、矩阵优化
#### 4、斜率优化
#### 5、树状数组优化
#### 6、线段树优化
#### 7、其他优化
