
#### 背包DP

0-1背包
```C++
//二维
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= m; j++) {
        // 当前背包容量装不进第i个物品，则价值等于前i-1个物品
        if (j < v[i]) {
            f[i][j] = f[i - 1][j];
        }
         // 能装，需进行决策是否选择第i个物品
        else {
            f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]);
        }
    }
}
//一维优化
for (int i = 1; i <= n; i++) {
    for (int j = m; j >= v[i]; j--) {
        f[j] = max(f[j], f[j - v[i]] + w[i]);
    }
}
```

完全背包
```C++
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= m; j++) {
        for (int k = 0; k * v[i] <= j; k++) {

            f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
        }
    }
}
//一维优化
for (int i = 1; i <= n; i++) {
    for (int j = v[i]; j <= m; j++) {
        f[j] = max(f[j], f[j - v[i]] + w[i]);
    }

}
```

多重背包
```C++
for (int i = 1; i <= n; i ++ )
    for (int j = 0; j <= m; j ++ )
        for (int k = 0; k <= s[i] && k * v[i] <= j; k ++ )
            f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
```

分组背包
```C++
for (int i = 1; i <= n; i ++ )
    for (int j = m; j >= 0; j -- )
        for (int k = 0; k < s[i]; k ++ )
            if (v[i][k] <= j)
                f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);

```

#### 区间DP 

```C++
for (int len = 2; len <= n; len ++ )
    for (int l = 1; l + len - 1 <= n; l ++ )
    {
        int r = l + len - 1;
        f[l][r] = INF;
        for (int k = l; k < r; k ++ )
            f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
    }
```

#### 状态压缩DP

```C++
//预处理
for (int i = 0; i < 1 << n; i ++ )
{
    state[i].clear();
    for (int j = 0; j < 1 << n; j ++ )
        if ((i & j) == 0 && st[i | j])
            state[i].push_back(j);
}
//计算
memset(f, 0, sizeof f);
f[0][0] = 1;
for (int i = 1; i <= m; i ++ )
    for (int j = 0; j < 1 << n; j ++ )
        for (auto k : state[j])
            f[i][j] += f[i - 1][k];
```

#### 单调队列优化DP 

```C++
for (int i = 1; i <= n; i ++ )
{
	if (q[hh] < i - m) hh ++ ;
	res = max(res, s[i] - s[q[hh]]);
	while (hh <= tt && s[q[tt]] >= s[i]) tt -- ;
	q[ ++ tt] = i;
}
```

### 数学知识 基础课

#### 质数：

AcWing 866 试除法判定质数      
AcWing 867 分解质因数      
AcWing 868 筛质数      

#### 约数：

AcWing 869 试除法求约数      
AcWing 870 约数个数      
AcWing 871 约数之和      
AcWing 872 最大公约数      

#### 欧拉函数：

素数一般指质数，gcd(a,b) = gcd(b,a mod b)     

欧拉函数：是小于或等于n的正整数中与n互质的数的数目。       

AcWing 873 欧拉函数：https://www.acwing.com/solution/content/81875/   
AcWing 874 筛法求欧拉函数: 

#### 快速幂：

AcWing 875 快速幂      
AcWing 876 快速幂求逆元      

#### 扩展欧几里得算法：

AcWing 877 扩展欧几里得算法 :https://www.acwing.com/solution/content/1393/      
AcWing 878 线性同余方程      

#### 中国剩余定理：

AcWing 204 表达整数的奇怪方式      

#### 高斯消元：

AcWing 883 高斯消元解线性方程组      
AcWing 884 高斯消元解异或线性方程组      

#### 求组合数：

AcWing 885 求组合数I      
AcWing 886 求组合数II      
AcWing 887 求组合数III      
AcWing 888 求组合数IV      
AcWing 889 满足条件的01序列      

#### 容斥原理：

AcWing 890 能被整除的数      

#### 博弈论：

AcWing 891 Nim游戏

AcWing 892 台阶-Nim游戏

AcWing 893 集合-Nim游戏

AcWing 894 拆分-Nim游戏


