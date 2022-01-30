**二分**

模板1：寻找左边界

模板2：寻找右边界

区间合并：st end 延迟处理法

**链表**

链表(数组模拟链表c++) https://blog.csdn.net/Annabel_CM/article/details/107446710

e[N] 存储节点的值，ne[N]存储该节点下一节点的下标

下标从零开始，第k 个数对应数组e[k - 1]；

e[idx] = a, ne[idx] = head, head = idx ++ ; //insert 先将x的指针指向原来head的位置，然后将head指向x

head = ne[head]; //在删除节点时，要判断该节点是否为头节点，若为头节点，直接head指向该节点所指向的位置

```cpp
// 将x插到下标是k的点后面
void add(int k, int x)
{
   e[idx] = x, ne[idx] = ne[k], ne[k] = idx ++ ;
}

// 将下标是k的点后面的点删掉
void remove(int k)
{
    ne[k] = ne[ne[k]];
}
```
双向链表
```cpp
// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ; //先将要插入的数左右指针分别指向对应位置,然后先将原来第k个数右指针所指的位置的左指针指向x,即l[r[k]] = idx;
}

```




