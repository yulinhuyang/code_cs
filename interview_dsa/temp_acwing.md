AcWing 831. KMP字符串题解：https://www.acwing.com/solution/content/14666/


1、s[ ]是模式串，即比较长的字符串。     
2、p[ ]是模板串，即比较短的字符串。     
3、“非平凡前缀”：指除了最后一个字符以外，一个字符串的全部头部组合。     
4、“非平凡后缀”：指除了第一个字符以外，一个字符串的全部尾部组合。（后面会有例子，均简称为前/后缀）    
5、“部分匹配值”：前缀和后缀的最长共有元素的长度。    
6、next[ ]是“部分匹配值表”，即next数组，它存储的是每一个下标对应的“部分匹配值”，是KMP算法的核心。（后面作详细讲解）。    

核心思想：在每次失配时，不是把p串往后移一位，而是把p串往后移动至下一次可以和前面部分匹配的位置，这样就可以跳过大多数的失配步骤。而每次p串移动的步数就是通过查找next[ ]数组确定的。
     
```C++
//KMP模板
#include <iostream>

using namespace std;

const int N = 100010, M = 10010; //N为模式串长度，M匹配串长度

int n, m;
int ne[M]; //next[]数组，避免和头文件next冲突
char s[N], p[M];  //s为模式串， p为匹配串

int main()
{
    cin >> n >> s+1 >> m >> p+1;  //下标从1开始

    //求next[]数组
    for(int i = 2, j = 0; i <= m; i++)
    {
        while(j && p[i] != p[j+1]) j = ne[j];
        if(p[i] == p[j+1]) j++;
        ne[i] = j;
    }
    //匹配操作
    for(int i = 1, j = 0; i <= n; i++)
    {
        while(j && s[i] != p[j+1]) j = ne[j];
        if(s[i] == p[j+1]) j++;
        if(j == m)  //满足匹配条件，打印开头下标, 从0开始
        {
            //匹配完成后的具体操作
            //如：输出以0开始的匹配子串的首字母下标
            //printf("%d ", i - m); (若从1开始，加1)
            j = ne[j];            //再次继续匹配
        }
    }

    return 0;
}
```

AcWing 1282. 搜索关键词----AC自动机模板题(KMP + trie) ： https://www.acwing.com/solution/content/50169/

```C++
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010, S = 55, M = 1000010;

int n;
int tr[N * S][26], cnt[N * S], idx;
char str[M];
int q[N * S], ne[N * S];

void insert()
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int t = str[i] - 'a';
        if (!tr[p][t]) tr[p][t] = ++ idx;
        p = tr[p][t];
    }
    cnt[p] ++ ;
}

void build()
{
    int hh = 0, tt = -1;
    for (int i = 0; i < 26; i ++ )
        if (tr[0][i])
            q[ ++ tt] = tr[0][i];

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = 0; i < 26; i ++ )
        {
            int p = tr[t][i];
            if (!p) tr[t][i] = tr[ne[t]][i];
            else
            {
                ne[p] = tr[ne[t]][i];
                q[ ++ tt] = p;
            }
        }
    }
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        memset(tr, 0, sizeof tr);
        memset(cnt, 0, sizeof cnt);
        memset(ne, 0, sizeof ne);
        idx = 0;

        scanf("%d", &n);
        for (int i = 0; i < n; i ++ )
        {
            scanf("%s", str);
            insert();
        }

        build();

        scanf("%s", str);

        int res = 0;
        for (int i = 0, j = 0; str[i]; i ++ )
        {
            int t = str[i] - 'a';
            j = tr[j][t];

            int p = j;
            while (p)
            {
                res += cnt[p];
                cnt[p] = 0;
                p = ne[p];
            }
        }

        printf("%d\n", res);
    }

    return 0;
}
```
