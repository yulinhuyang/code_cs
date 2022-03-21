
背包9讲代码模板：https://blog.csdn.net/yandaoqiusheng/article/details/84782655

0-1背包问题模板

https://www.acwing.com/solution/content/1374/

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

完全背包问题模板

https://www.acwing.com/solution/content/5345/

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

多重背包问题模板

```C++
for (int i = 1; i <= n; i++) {//枚举背包
    for (int j = 1; j <= m; j++) {//枚举体积
        for (int k = 0; k <= s[i]; k++) {
            if (j >= k * v[i]) {
                f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
            }
        }
    }
}
```

混合背包问题模板

```C++
for (int i = 1; i <= n; i++) {
    cin >> c >> w >> p;
    if (p == 0) //完全背包
        for (int j = c; j <= V; j++)
            f[j] = max(f[j], f[j - c] + w);
    else if (p == -1) //01背包
        for (int j = V; j >= c; j--)
            f[j] = max(f[j], f[j - c] + w);
    else { //多重背包二进制优化
        int num = min(p, V / c);
        for (int k = 1; num > 0; k <<= 1) {
            if (k > num) k = num;
            num -= k;
            for (int j = V; j >= c * k; j--)
                f[j] = max(f[j], f[j - c * k] + w * k);
        }
    }
}

```

分组背包问题模板

```C++
for (int i = 1; i < = n; i++) {
for (int j = 0; j <= m; j++) {
    f[i][j] = f[i - 1][j];  //不选
    for (int k = 0; k < s[i]; k++) {
	if (j >= v[i][k]) {
	    f[i][j] = max(f[i][j], f[i - 1][j - v[i][k]] + w[i][k]);
	}
    }
}
}

```

二维费用的背包问题模板

```C++
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> m[i] >> w[i];//体积，重量，价值
    }
    for (int i = 1; i <= n; i++)
        for (int j = V; j >= v[i]; j--)
            for (int k = M; k >= m[i]; k--)
                f[j][k] = max(f[j - v[i]][k - m[i]] + w[i], f[j][k]);//动态转移方程，01 背包的思路

```

