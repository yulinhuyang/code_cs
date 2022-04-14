
牛蛙点点提高课题解：https://www.acwing.com/user/myspace/activity/52559/

wzc1995: https://www.acwing.com/user/myspace/index/21/

仅存老实人题解：https://www.acwing.com/user/myspace/solution/index/39715/

提高课题单 + 题解(71/220)：https://www.acwing.com/blog/content/7459/


### 数学知识基础课


#### 质数：

- AcWing 866 试除法判定质数: 质数，在大于1的整数中，如果只包含1和本身这两个约数，就被称为质数，也叫素数。 for (int i = 2; i <= x / i; i ++ )
- AcWing 867 分解质因数：while (x % i == 0) x /= i, s ++ ;     
- AcWing 868 筛质数：朴素筛法 + 线性筛法

朴素筛:O(nlg(n))   

质数:不能是别的数的倍数--从2开始往上遍历每个数的倍数--标记为不能是质数    
primes先把1-1e7的所有质数筛出来[3,5,7...]    
```C++
void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (st[i]) continue;
        primes[cnt ++ ] = i;
        for (int j = i + i; j <= n; j += i)
            st[j] = true;
    }
}
```

线性筛: n只会被最小质因子筛掉 

```C++
bool st[N];//st[i]==true代表i这个数是其他数的倍数--即i不是质数
void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;//如果当前数没被筛过 则把i这个数加进质数列表primes里 
        for (int j = 0; primes[j]*i<=n;j++)//从小到大枚举所有质数
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
		//1 i%primes[j]==0 primes[j]一定是i的最小质因子 -- (因为从小到大遍历j)primes[j]一定是primes[j]*i的最小质因子
		//2 i%primes[j]!=0 则primes[j]一定小于i的所有质因子 -- primes[j]也一定是primes[j]*i的最小质因子
		//对于一个合数x 假设primes[j] 是x的最小质因子 当i枚举到x/primes[j]时,则后面的合数给后面的质数去筛
        }
    }
}
```

每个合数必有一个最大因子（不包括它本身），用这个因子把合数筛掉。          
对于每一个数i，乘上小于等于i的最小素因数的素数，就得到以i为最大因数的合数。设有一个数t，只要将所有以比t小的数为最大因数的合数筛去，那么比t小的数里剩下的就只有素数了。    
欧拉线性筛的关键在于：每个合数只被它最大的非自身的因数筛掉。      
当前数i能整除当前primes[j]时 则对于L > j的i/*primes[L]一定能由更大的i/*primes[j]来表示    

#### 约数：

- AcWing 869 试除法求约数： if (i != x / i) res.push_back(x / i);      
- AcWing 870 约数个数: unordered_map<int, int> primes 存储  
约数个数定理：
```C++
任何一个整数N都可以写成N = p1^α1 * p2^α2 * p3^α3 *...... * pk^αk
约数个数就是(α1 + 1) * (α2 + 1) * (α3 + 1) * ...... * (αk + 1)
约数之和就是(p1^0 + p1^1 + ... + p1^α1) * ... * (pk^0 + pk^1 + ... + pk^αk)  

N的每个约数d都可以写成d = p1^β1 * p2^β2 * p3^β3 *...... * pk^βk    0 <= βi <= αi    
```
- AcWing 871 约数之和:  https://www.acwing.com/file_system/file/content/whole/index/content/3847584/     
- AcWing 872 最大公约数: d | a, d |b，则d | ax + by , b ? gcd(b, a % b) : a;   


#### 欧拉函数：


欧拉函数：是小于或等于n的正整数中与n互质的数的数目。       
- AcWing 873 欧拉函数：https://www.acwing.com/solution/content/81875/   
欧拉函数：1 ∼ N 中与N互质的数的个数被称为欧拉函数，记为φ(n)。      
欧拉定理：对于任意互素的a和n，有 a^φ (n) ≡ 1 (mod n)。        
若在算数基本定理中，N=p1^α1/∗p2^α2/∗p3^α3…/∗pm^αm，则：φ(n)= N(1−1/p1)(1−1/p2)(1−1/p3)…(1−1/pm)     
容斥原理证明。 

- AcWing 874 筛法求欧拉函数: https://www.acwing.com/solution/content/3952/       
质数i的欧拉函数即为phi[i] = i - 1        
phi[primes[j] * i]分为两种情况：               
① i % primes[j] == 0时：primes[j]是i的最小质因子，也是primes[j] * i的最小质因子，因此1-1 / primes[j]这一项在phi[i]中计算过了，只需将基数N修正为primes[j]倍，最终结果为phi[i] * primes[j]。       
② i % primes[j] != 0：primes[j]不是i的质因子，只是primes[j] * i的最小质因子，因此不仅需要将基数N修正为primes[j]倍，还需要补上1 - 1 / primes[j]这一项，因此最终结果phi[i] * (primes[j] - 1)       
 

#### 快速幂：

- AcWing 875 快速幂           
- AcWing 876 快速幂求逆元     
https://www.acwing.com/solution/content/83550/ 

费马小定理：如果p是一个质数，而整数a不是p的倍数，则有a^(p-1)≡ 1(mod p)。费马小定理是欧拉定理的特殊形式，欧拉定理是费马小定理的推广。     
可以推导出a^(p-2) 为a在modp意义下的逆元，快速幂解a^(p-2)。      
逆元：若整数 b，m互质，并且对于任意的整数 a，如果满足 b|a，则存在一个整数 x，使得 a/b ≡ a×x (mod m)，则称 x 为 b 的模 m 乘法逆元，记为 b−1 (mod m)。     
除以一个数，就等于乘以这个数的逆元。     
b存在乘法逆元的充要条件是b与模数 m 互质。当模数 m 为质数时，结合乘法逆元公式和费马小定理，可得b × m−2 即为b的乘法逆元。

#### 扩展欧几里得算法：

同余：https://fanfansann.blog.csdn.net/article/details/109341636      
扩展欧几里得算法+线性同余方程+中国剩余定理： https://blog.csdn.net/qq_42815188/article/details/88092422      

- AcWing 877 扩展欧几里得算法 :
https://www.acwing.com/solution/content/1393/       
裴蜀定理(或贝祖定理)：设a,b是不全为零的整数，则存在整数x,y，使得ax+by = gcd(a,b)。       
推论：a,b互质的充分必要条件是存在整数x,y使ax+by=1。              
扩展欧几里得算法给定两个非零整数a和b,求一组整数解(x,y),使得ax+by = gcd(a,b),其中gcd(a,b)是a和b的最大公约数。                 
有两个数a,b，对它们进行辗转相除法，可得它们的最大公约数,收集辗转相除法中产生的式子，倒回去，可以得到ax+by=gcd(a,b)的整数解。       

- AcWing 878 线性同余方程:      
给定整数a,c,m ，求一个整数x满足 a * x ≡ c(mod m)，或者给出无解。因为未知数的指数为1，称之为一次同余方程，也称线性同余方程。      
若方程 ax + my = c 有整数解，则共有gcd(a,m) 个解，其全部解的公式为：x = cx0 / gcd(a,m) + m / gcd(a,m)  * K，y = cy0 / gcd(a,m) - a/gcd(a,m) * K      

#### 中国剩余定理：

- AcWing 204 表达整数的奇怪方式         
https://www.acwing.com/solution/content/3539/          
线性同余方程组是由若干个线性同余方程构成的线性方程组，中国剩余定理给出了模数两两互质的线性同余方程组的一个特解。     
x ≡ a1(mod m1),x ≡ a2(mod m2),...x ≡ an(mod mn)       
设自然数 m1,m2,mn两两互质，M = m1 * m2...* mn,Mi = M / mi，ti是Mi模mi的逆，则解为x = a1 *  M1 * t1 + a2 * M2 * t2 +..+ an * Mn * tn。     



#### 高斯消元：

- AcWing 883 高斯消元解线性方程组: https://www.acwing.com/solution/content/5151/      
```C++
初等行列变换：
	1 把某一行乘一个非0的数 (方程的两边同时乘上一个非00数不改变方程的解)
	2 交换某两行 (交换两个方程的位置)
	3 把某行的若干倍加到另一行上去 （把一个方程的若干倍加到另一个方程上去）

完美阶梯形：唯一解， 0=非零 无解，0=0 无穷组解。    

枚举每一列c：
	找到当前列绝对值最大的一行
	用初等行变换(2) 把这一行换到最上面（未确定阶梯型的行，并不是第一行）
	用初等行变换(1) 将该行的第一个数变成 1 （其余所有的数字依次跟着变化）
	用初等行变换(3) 将下面所有行的当且列的值变成 0
```
- AcWing 884 高斯消元解异或线性方程组: https://www.acwing.com/solution/content/26544/      

#### 求组合数：

- AcWing 885 求组合数I: 从j个里面选i个物品的选法 = 不选i个物品的选法 + 必选i个物品的选法：C[i][j] = C[i-1][j-1]+C[i-1][j]    
- AcWing 886 求组合数II: 快速幂 + 乘法逆元 + 组合数     
https://www.acwing.com/file_system/file/content/whole/index/content/1726143/ 

AcWing 887 求组合数III:  https://www.acwing.com/solution/content/26553/

Lucas定理
```C++
若p是质数，则对于任意整数 1 <= m <= n，有：
    C(n, m) = C(n % p, m % p) * C(n / p, m / p) (mod p)
```
- AcWing 888 求组合数IV：https://www.acwing.com/solution/content/26559/ 
```C++
#pragma GCC optimize(2)   C++开o2优化
1. 筛法求出范围内的所有质数
2. 通过 C(a, b) = a! / b! / (a - b)! 这个公式求出每个质因子的次数。 n! 中p的次数是 n / p + n / p^2 + n / p^3 + ...
3. 用高精度乘法将所有质因子相乘
```
- AcWing 889 满足条件的01序列：卡特兰数
```C++
进出栈序列：n个元素进栈序列为：1，2，3，4，...，n，则有多少种出栈序列。                
括号序列：n对括号，则有多少种括号匹配的括号序列。             
二叉树：n + 1个叶子节点能够构成多少种形状不同的满二叉树。                     
前4项：1, 1, 2, 5，通项Cn = C(n,2n) - C(n - 1,2n) = C(n,2n)/(n + 1)     
```
https://www.acwing.com/solution/content/8907/      

#### 容斥原理：

容斥原理：先不考虑重叠的情况，把包含于某内容中的所有对象的数目先计算出来，然后再把计数时重复计算的数目排斥出去，使得计算的结果既无遗漏又无重复。         
|A∪B∪C| = |A|+|B|+|C| - |A∩B| - |B∩C| - |C∩A| + |A∩B∩C|，每一项前面的正负号取决于集合的个数 – 奇数个集合为正,偶数个集合为负。         

- AcWing 890 能被整除的数:https://www.acwing.com/solution/content/29702/      

#### 博弈论：

- AcWing 891 Nim游戏： https://www.acwing.com/solution/content/14269/
```C++
公平组合游戏(ICG):
- 两名选手
- 两名选手轮流行动，每一次行动可以在有限合法操作集合中选择一个
- 游戏的任何一种可能的局面(position)，合法操作集合只取决于这个局面本身，不取决于轮到哪名选手操作、以前的任何操作、骰子的点数或者其它因素；局面的改变称为“移动”(move)
- 如果轮到某名选手移动，且这个局面的合法的移动集合为空（也就是说此时无法进行移动），则这名选手负

P-position：在当前的局面下，先手必败；N-position：在当前的局面下，先手必胜。
```
假设n堆石子，石子数目分别是a1,a2,…,an，如果a1 xor a2 xor … xor an≠0，先手必胜；否则先手必败。   

- AcWing 892 台阶-Nim游戏:https://www.acwing.com/solution/content/13187/    
如果先手时奇数台阶上的值的异或值为非0，则先手必胜，反之必败！       
- AcWing 893 集合-Nim游戏:https://www.acwing.com/solution/content/23435/

SG函数：
```C++
SG函数是用于解决博弈论中公平组合游戏IGG问题的。

Mex运算:mex(S)为求出不属于集合S的最小非负整数运算。

SG函数：在有向图游戏中,对于每个节点x,设从x出发共有k条有向边,分别到达节点y1,y2,····yk,定义SG(x)的后记节点y1,y2,···yk的SG函数值构成的集合在执行mex运算的结果,即:
SG(x) = mex({SG(y1),SG(y2)····SG(yk)})，整个有向图游戏G的SG函数值被定义为有向图游戏起点s的SG函数值,即 SG(G)=SG(s).
sg(x)= mex(){sg(y)| x-> y}, x可以一次操作到y, sg(x)=0的x为必败态。

有向图游戏的和：有向图游戏的和的SG函数值等于它包含的各个子游戏SG函数的异或和,即: SG(G)=SG(G1) xor SG(G2) xor···xor SG(Gm)
```
- AcWing 894 拆分-Nim游戏



### 数学知识提高课

#### 筛质数：

- AcWing 1292 哥德巴赫猜想:线性筛质数  https://www.acwing.com/solution/content/22512/
- AcWing 1293 夏洛克和他的女朋友:即从一个质数向合数连一条边，最后会构成二分图即从一个质数向合数连一条边，最后会构成二分图。https://www.acwing.com/solution/content/7602/     
- AcWing 196 质数距离：https://www.acwing.com/solution/content/11586/      
性质1：若一个数n是一个合数，必然存在2个因子d,n/d，假设d <= n/d，则d <= sqrt(n)，因此必然存在一个小于等于 sqrt(n)的因子。                  
性质2：若x∈[L,R],且x是合数，则一定存在P <= sqrt(2^31−1) (< 50000)，使得P能整除x，其中P < x。             
 
1 找出1 ~ sqrt(2^31−1) (< 50000)中的所有质因子。               
2 对于1 ~ 50000中每个质数P，将[L,R]中所有P的倍数筛掉(至少2倍)。找到大于等于L的最小的P的倍数P0，找下一个倍数时只需要+=P即可。                

#### 分解质因数：

- AcWing 197 阶乘分解：https://www.acwing.com/solution/content/4960/     
先枚举1~10^6中所有质数，再求1~n中p的次数、P^2的次数...的和。           
先把质数表打好，for 循环把小于n的每个质数进行一次运算，用数组记录。              
cnt(x) = [n/(x^1)] + [n/(x^2)] + [n/(x^3)] +...(直到x的n次方大于n)。            

#### 快速幂：

- AcWing 1289 序列的第k个数：https://www.acwing.com/solution/content/11635/ 
当且仅当a == b == c时，它既是一个公差为0的等差数列也是一个公比为1的等比数列，因此它的第k项用等差数列和等比数列求出来的结果是一样的           
情况1：若a,b,c是一个等差数列，则第k项是a + (c - b) * (k - 1)      
情况2：若a,b,c是一个等比数列，则第k项是a * (b / a)^(k - 1)，其中b / a一定是一个整数，用快速幂qmi求解。     
    
- AcWing 1290 越狱：容斥原理补集：不会导致犯罪的： m⋅(m−1)^(n-1),会犯罪的是 m^n-m⋅(m−1)^(n-1), https://www.acwing.com/solution/content/47738/      

#### 约数个数：

- AcWing 1291 轻拍牛头: https://www.acwing.com/solution/content/31266/     
- AcWing 1294 樱花: https://www.acwing.com/solution/content/7562/     
- AcWing 198 反素数: https://www.acwing.com/solution/content/44900/     
- AcWing 200 Hankson的趣味题:https://www.acwing.com/solution/content/3101/     

#### 欧拉函数：

- AcWing 201 可见的点: https://www.acwing.com/solution/content/25745/     
- AcWing 220 最大公约数:https://www.acwing.com/solution/content/3325/     

#### 同余：

- AcWing 203 同余方程:https://www.acwing.com/solution/content/25756/    
- AcWing 222 青蛙的约会:https://www.acwing.com/solution/content/24456/     
- AcWing 202 最幸运的数字:https://www.acwing.com/solution/content/47979/     
- AcWing 1298 曹冲养猪:中国剩余定理，https://www.acwing.com/solution/content/45967/    

#### 矩阵乘法：
     
- AcWing 1303 斐波那契前n项和:https://www.acwing.com/solution/content/15121/     
- AcWing 1304 佳佳的斐波那契:https://www.acwing.com/solution/content/15132/     
- AcWing 1305 GT考试：https://www.acwing.com/solution/content/25887/
     

#### 组合计数：

- AcWing 1307 牡牛和牝牛：https://www.acwing.com/solution/content/10366/
- AcWing 1308 方程的解:https://www.acwing.com/solution/content/48085/     
- AcWing 1309 车的放置:https://www.acwing.com/solution/content/24436/     
- AcWing 1310 数三角形:https://www.acwing.com/solution/content/24442/     
- AcWing 1312 序列统计:https://www.acwing.com/solution/content/8867/     
- AcWing 1315 网格:卡特兰数，https://www.acwing.com/solution/content/46130/     
- AcWing 1316 有趣的数列:https://www.acwing.com/solution/content/26267/   

#### 高斯消元：

- AcWing 207 球形空间产生器:https://www.acwing.com/solution/content/25960/
- AcWing 208 开关问题: https://www.acwing.com/solution/content/1087/    

#### 容斥原理：

- AcWing 214 Devu和鲜花:https://www.acwing.com/solution/content/13666/     
- AcWing 215 破译密码:https://www.acwing.com/solution/content/17858/    

#### 概率与数学期望：

- AcWing 217 绿豆蛙的归宿:https://www.acwing.com/solution/content/17868/     
- AcWing 218 扑克牌:https://www.acwing.com/solution/content/26204/     

#### 博弈论：

- AcWing 1319 移棋子游戏:https://www.acwing.com/solution/content/15279/     
- AcWing 1321 取石子:https://www.acwing.com/solution/content/26214/     
- AcWing 1322 取石子游戏:https://www.acwing.com/solution/content/26286/     



### 补充数据结构

RMQ/ST表/区间最值查询 ---- 模板： https://www.acwing.com/blog/content/7942/     
算法学习笔记(12): ST表： https://zhuanlan.zhihu.com/p/105439034     


