

2Q算法： https://blog.csdn.net/qq_40088655/article/details/124028273

FIFO + LRU


LRU: map + list, K + v,队头为最近最少不适用的，如果用了(命中)会被换到队尾去。

LFU: K -> V -> Frequency, 淘汰最不经常使用的页，当平局时，去除最近最少使用的键。

缓存淘汰算法（LFU、LRU、ARC、FIFO、2Q）分析: https://zhuanlan.zhihu.com/p/352910565


