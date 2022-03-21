
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
for (int i = 1; i <= n; i ++ )
    for (int j = 0; j <= m; j ++ )
        for (int k = 0; k <= s[i] && k * v[i] <= j; k ++ )
            f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
```

混合背包问题模板

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

分组背包问题模板

```C++
for (int i = 1; i <= n; i ++ )
    for (int j = m; j >= 0; j -- )
        for (int k = 0; k < s[i]; k ++ )
            if (v[i][k] <= j)
                f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);


```

二维费用的背包问题模板

```C++
for (int i = 0; i < n; i++) {
    int v, m, w;
    cin >> v >> m >> w;
    for (int j = V; j >= v; j--)
        for (int k = M; k >= m; k--)
            f[j][k] = max(f[j][k], f[j - v][k - m] + w);
}

```

AcWing 10. 有依赖的背包问题

```c++
void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    for (int i = h[u]; ~i; i = ne[i])   // 循环物品组
    {
        int son = e[i];
        dfs(e[i]);

        // 分组背包
        for (int j = m - v[u]; j >= 0; j -- )  // 循环体积
            for (int k = 0; k <= j; k ++ )  // 循环决策
                f[u][j] = max(f[u][j], f[u][j - k] + f[son][k]);
    }

    // 将物品u加进去
    for (int i = m; i >= v[u]; i -- ) f[u][i] = f[u][i - v[u]] + w[u];
    for (int i = 0; i < v[u]; i ++ ) f[u][i] = 0;
}

int main()
{
    cin >> n >> m;

    memset(h, -1, sizeof h);
    int root;
    for (int i = 1; i <= n; i ++ )
    {
        int p;
        cin >> v[i] >> w[i] >> p;
        if (p == -1) root = i;
        else add(p, i);
    }

    dfs(root);

    cout << f[root][m] << endl;

    return 0;
}

```
