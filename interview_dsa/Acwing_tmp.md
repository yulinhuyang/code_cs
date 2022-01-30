
dijkstra：找点，循环基于点

bellman-ford: 找边，循环基于边

二叉堆优化的dijkstra:只能处理非负权的，priority_queue中存放的是<distance,index>。

SPFA：可以处理负值，queue中存放的是index。

二叉堆优化的dijkstra，SPFA都是基于邻接表结构的。
