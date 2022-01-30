Tries数组

```cpp
int son[N][26], cnt[N], idx;
// 0号点既是根节点，又是空节点，因为里面是++idx
// son[][]存储树中每个节点的子节点
// cnt[]存储以每个节点结尾的单词数量
```


**带距离的并查集的路径压缩方法**

```cpp
// 返回x的祖宗节点
int find(int x)
{
    if (p[x] != x)
    {
        int u = find(p[x]);
        d[x] += d[p[x]];
        p[x] = u;
    }
    return p[x];
}
```

**邻接表**

相当于链表数组：head->展开成h[N]

```cpp
// 对于每个点k，开一个单链表，存储k所有可以走到的点。h[k]存储这个单链表的头结点
int h[N], e[N], ne[N], idx;

// 添加一条边a->b
void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

// 初始化
idx = 0;
memset(h, -1, sizeof h);
```
