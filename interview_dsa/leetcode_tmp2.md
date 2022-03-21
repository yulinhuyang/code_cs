
背包9讲代码模板：https://blog.csdn.net/yandaoqiusheng/article/details/84782655

AcWing 2 0-1背包问题模板

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

AcWing 3 完全背包问题模板

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

AcWing 4 多重背包问题模板

```C++
for (int i = 1; i <= n; i ++ )
    for (int j = 0; j <= m; j ++ )
        for (int k = 0; k <= s[i] && k * v[i] <= j; k ++ )
            f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
```

AcWing 7 混合背包问题模板

```C++
for (int i = 0; i < n; i ++ )
{
    int v, w, s;
    cin >> v >> w >> s;
    if (!s) //完全背包
    {
        for (int j = v; j <= m; j ++ )
            f[j] = max(f[j], f[j - v] + w);
    }
    else
    {
        if (s == -1) s = 1; //01背包
        for (int k = 1; k <= s; k *= 2)
        {
            for (int j = m; j >= k * v; j -- )
                f[j] = max(f[j], f[j - k * v] + k * w);
            s -= k;
        }
        if (s) //多重背包二进制优化
        {
            for (int j = m; j >= s * v; j -- )
                f[j] = max(f[j], f[j - s * v] + s * w);
        }
    }
}


```

AcWing 9 分组背包问题模板

```C++
for (int i = 1; i <= n; i ++ )
    for (int j = m; j >= 0; j -- )
        for (int k = 0; k < s[i]; k ++ )
            if (v[i][k] <= j)
                f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);


```

AcWing 8 二维费用的背包问题模板

```C++
for (int i = 0; i < n; i++) {
    int v, m, w;
    cin >> v >> m >> w;
    for (int j = V; j >= v; j--)
        for (int k = M; k >= m; k--)
            f[j][k] = max(f[j][k], f[j - v][k - m] + w);
}

```

AcWing 10. 有依赖的背包问题:https://www.acwing.com/activity/content/code/content/118840/      
AcWing 11. 背包问题求方案数: https://www.acwing.com/activity/content/code/content/120228/    
AcWing 12. 背包问题求具体方案: https://www.acwing.com/activity/content/code/content/119629/   
