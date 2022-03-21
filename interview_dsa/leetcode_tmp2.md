01背包问题模板

https://www.acwing.com/solution/content/1374/

```C++
//二维
for(int i = 1; i <= n; i++)
{
    for(int j = 1; j <= m; j++)
    {
        // 当前背包容量装不进第i个物品，则价值等于前i-1个物品
        if(j < v[i]){
            f[i][j] = f[i - 1][j];
        }
        // 能装，需进行决策是否选择第i个物品
        else{
            f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]);
        }
     }
}
//一维优化
for(int i = 1; i <= n; i++)
{
    for(int j = m; j >= v[i]; j--)
    {
        f[j] = max(f[j], f[j - v[i]] + w[i]);
    }
}

```

完全背包问题模板

https://www.acwing.com/solution/content/5345/

```C++
//二维
for(int i = 1 ; i<= n ;i++)
{
	for(int j = 0 ; j<= m ;j++)
	{
		for(int k = 0 ; k*v[i]<= j ; k++)
		{
		
			f[i][j] = max(f[i][j],f[i-1][j-k*v[i]]+k*w[i]);
		}
	}
}
//一维优化
for(int i = 1 ; i<= n ;i++)
{
	for(int j = v[i] ; j<= m ;j++)
	{
		f[j] = max(f[j],f[j-v[i]]+w[i]);
	}

}
```

多重背包问题

```C++
    for(int i=1;i<=n;i++)
	{
        for(int j=0;j<=m;j++)
		{
            f[i][j]=f[i-1][j];  //不选
            for(int k=0;k<s[i];k++)
			{
                if(j>=v[i][k]) {
					f[i][j]=max(f[i][j],f[i-1][j-v[i][k]]+w[i][k]);
				}  
            }
        }
    }

```
