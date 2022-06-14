
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


**AcWing 162 黑盒子**

对顶堆模板题：大根堆down ---------> <------- 小根堆up ，从左到右数值是递增的   

第k小的数：第i次get，盒子内总数是u[i]的时候，第i小的数。
每次get操作，大顶堆最多增加一个元素。    
对比动态中位数：优先加入大根堆down，然后根据数量关系(max_heap.size() > min_heap.size() + 1)和平衡关系(min_heap.top() < max_heap.top())进行调整。   
相同点：down.top是第i小的数/中位数

