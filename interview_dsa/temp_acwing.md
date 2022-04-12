
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

素数一般指质数，gcd(a,b) = gcd(b,a mod b)     

欧拉函数：是小于或等于n的正整数中与n互质的数的数目。       

- AcWing 873 欧拉函数：https://www.acwing.com/solution/content/81875/   
- AcWing 874 筛法求欧拉函数: 

#### 快速幂：

- AcWing 875 快速幂      
- AcWing 876 快速幂求逆元      

#### 扩展欧几里得算法：

- AcWing 877 扩展欧几里得算法 :https://www.acwing.com/solution/content/1393/      
- AcWing 878 线性同余方程      

#### 中国剩余定理：

- AcWing 204 表达整数的奇怪方式      

#### 高斯消元：

- AcWing 883 高斯消元解线性方程组      
- AcWing 884 高斯消元解异或线性方程组      

#### 求组合数：

- AcWing 885 求组合数I: 从j个里面选i个物品的选法 = 不选i个物品的选法 + 必选i个物品的选法：C[i][j] = C[i-1][j-1]+C[i-1][j]
- AcWing 886 求组合数II: 快速幂 + 乘法逆元 + 组合数
https://www.acwing.com/file_system/file/content/whole/index/content/1726143/     
AcWing 887 求组合数III:  Lucas定理
```C++
若p是质数，则对于任意整数 1 <= m <= n，有：
    C(n, m) = C(n % p, m % p) * C(n / p, m / p) (mod p)
```
- AcWing 888 求组合数IV： 
```C++
1. 筛法求出范围内的所有质数
2. 通过 C(a, b) = a! / b! / (a - b)! 这个公式求出每个质因子的次数。 n! 中p的次数是 n / p + n / p^2 + n / p^3 + ...
3. 用高精度乘法将所有质因子相乘
```
- AcWing 889 满足条件的01序列      

#### 容斥原理：

- AcWing 890 能被整除的数      

#### 博弈论：

- AcWing 891 Nim游戏

- AcWing 892 台阶-Nim游戏

- AcWing 893 集合-Nim游戏

- AcWing 894 拆分-Nim游戏



### 数学知识提高课

#### 筛质数：

- AcWing 1292 哥德巴赫猜想:线性筛质数  https://www.acwing.com/solution/content/22512/
- AcWing 1293 夏洛克和他的女朋友     
- AcWing 196 质数距离     

#### 分解质因数：

- AcWing 197 阶乘分解     

#### 快速幂：

- AcWing 1289 序列的第k个数     
- AcWing 1290 越狱     

#### 约数个数：

- AcWing 1291 轻拍牛头     
- AcWing 1294 樱花     
- AcWing 198 反素数     
- AcWing 200 Hankson的趣味题     

#### 欧拉函数：

- AcWing 201 可见的点     
- AcWing 220 最大公约数     

#### 同余：

- AcWing 203 同余方程     
- AcWing 222 青蛙的约会     
- AcWing 202 最幸运的数字     
- AcWing 1298 曹冲养猪     

#### 矩阵乘法：
     
- AcWing 1303 斐波那契前n项和     
- AcWing 1304 佳佳的斐波那契     
- AcWing 1305 GT考试：https://www.acwing.com/solution/content/25887/
     

#### 组合计数：

- AcWing 1307 牡牛和牝牛：https://www.acwing.com/solution/content/25950/
     
- AcWing 1308 方程的解     
- AcWing 1309 车的放置     
- AcWing 1310 数三角形     
- AcWing 1312 序列统计     
- AcWing 1315 网格     
- AcWing 1316 有趣的数列   

#### 高斯消元：

- AcWing 207 球形空间产生器:https://www.acwing.com/solution/content/25960/
          
- AcWing 208 开关问题     

#### 容斥原理：

- AcWing 214 Devu和鲜花     
- AcWing 215 破译密码     

#### 概率与数学期望：

- AcWing 217 绿豆蛙的归宿     
- AcWing 218 扑克牌     

#### 博弈论：

- AcWing 1319 移棋子游戏     
- AcWing 1321 取石子     
- AcWing 1322 取石子游戏     
