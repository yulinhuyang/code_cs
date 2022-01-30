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

