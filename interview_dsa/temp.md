
扫描线与自适应辛普森积分 求多边形面积并: https://blog.csdn.net/qq_45735851/article/details/115309408

**两个区间的关系**

两个区间的关系：https://zhuanlan.zhihu.com/p/301507910

远离、重叠、相交

**空间换时间**

2352. 相等行列对： hash代替循环，空间换取时间

**二分转判定**

2517. 礼盒的最大甜蜜度：二分转判定的时候，注意 l和r如何取。


**双栈**

`defaultdict` 的初始化需要传入一个函数作为默认值，这个函数会在访问不存在的键时被调用，返回值作为默认值。常见的用法是传入 `int`、`list`、`set` 等 Python 内置类型作为默认值，这样可以方便地进行计数、分组等操作。

freq: int -> list

895. 最大频率栈

hash = defaultdict(int)

freq = defaultdict(list)


代码在线格式化：https://www.bejson.com/format/c_formater/

519. 随机翻转矩阵

index = self.hash.get(val,val)  #查询不到返回默认值

self.get(key, default=None)

