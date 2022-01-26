**补充笔记**

### 0x00 基本算法

lowbit(x)是x的二进制表达式中最低位的1所对应的值（位置）。

```cpp
//6的二进制是110，所以lowbit(6)=2
int lowbit(int x)
{
    return x&(-x);
}

int lowbit(int x)
{
    return x&(x^(x-1));
}
```


二分转判定

离散化

### 0x10 基本数据结构

由数据范围反推算法复杂度以及算法内容: https://www.acwing.com/blog/content/32/

回文子串的最大长度： 加#中心展开法

超市：归并排序变形

### 0x20 搜索

BFS就是一种dijkstra算法

路径规划 | 图搜索算法：DFS、BFS、GBFS、Dijkstra、A*： https://zhuanlan.zhihu.com/p/346666812


### 0x30 数学知识

Catalan数 

0-1 分数规划：二分法求非负值


### 0x40 数据结构进阶

树状数组：逆序对，left+right两遍扫描

分离包含有多个变量的项，使公式中不同变量之间互相独立

### 0x50 动态规划


0-1背包：使用滚动数组时为何要逆序枚举: https://blog.csdn.net/aidway/article/details/50726472

闫氏DP法，集合观点： https://www.cnblogs.com/IzayoiMiku/p/13635809.html

AcWing284 金字塔：拆环为链，复制一倍在后面

AcWing286 选课：虚拟0节点

### 0x60 图论

没有负权：dij

有负权 卡spfa：写spfa 最坏也是一个bellman-ford

随机图：spfa飞快

0-1分数规划模型： https://blog.csdn.net/niiick/article/details/80925267

60 分钟搞定图论中的 Tarjan 算法（一）:https://zhuanlan.zhihu.com/p/101923309


