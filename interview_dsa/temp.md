LFU： https://leetcode-cn.com/problems/lfu-cache/solution/gong-shui-san-xie-yun-yong-tong-pai-xu-s-53m3/

https://www.acwing.com/activity/content/code/content/555766/

 1  map<key,node> + set<node>(代替堆)，因为C++中堆pq的值不能改。
 
 2  map(key, list<node>::iterator) + map(freq,list<node>) ：链接法。散列冲突解决：链接法、开发定址法。类似邻接表结构，展开的list链表。比较图存储 map + map
