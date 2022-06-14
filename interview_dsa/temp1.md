
**AcWing159 奶牛矩阵**

n-next[n]就是最小循环节的长度，行的最小循环节*列的最小循环节          
求行列的循环节  
    
https://www.acwing.com/solution/content/114474/  


```cpp
#include <iostream>
#include <cstring>
#include <string>

using namespace std;
const int N = 10010;

string s[N], st[N];
int r, c, ne[N], h, w;

int main() {
    cin >> r >> c;
    for (int i = 1; i <= r; i++) {
        string str;
        cin >> str;
        s[i] = " " + str;
        for (int j = 1; j <= c; j++) {
            st[j] += s[i][j]; //转置
        }
    }

    ne[0] = 0;
    for (int i = 2, j = 0; i <= r; i++) {
        while (j && s[i] != s[j + 1]) j = ne[j];
        if (s[i] == s[j + 1]) j++;
        ne[i] = j;
    }
    int width = r - ne[r];

    memset(ne, 0, sizeof(ne[0]));
    ne[0] = 0;
    for (int i = 2, j = 0; i <= c; i++) {
        while (j && st[i] != st[j + 1]) j = ne[j];
        if (st[i] == st[j + 1]) j++;
        ne[i] = j;
    }
    int height = c - ne[c];
    cout << width * height << endl;
    return 0;
}

```

**AcWing 161 电话列表**

Trie[N][10]    
Trie模板改进：插入和判断同时进行     


