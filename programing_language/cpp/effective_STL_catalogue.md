**容器**

 条款1: 仔细选择你要的容器
 
 条款2: 小心对“容器无关代码”的幻想
 
 条款3: 使容器里对象的拷贝操作轻量而正确
 
 条款4: 用empty来代替检查size是否为0
 
 条款5: 尽量使用范围成员函数代替他们的单元素兄弟
 
 条款6: 警惕C++的及其令人恼怒的分析
 
 条款7: 当使用new得指针的容器时，切记在容器销毁前delete那些指针
 
 条款8: 千万不要把auto_ptr放入容器中
 
 条款9: 小心选择删除选项
 
 条款10: 当心allocator的协定和约束
 
 条款11: 了解自定义allocator的正统使用法
 
 条款12: 对STL容器的线程安全性的期待现实一些
 
**vector和string**

 条款13: 尽量使用vector和string来代替动态申请的数组
 
 条款14: 用reserve来避免不必要的内存重新分配
 
 条款15: 当心string的实现中的变化
 
 条款16: 如何将vector和string的数据传给传统的API
 
 条款17: 用“交换技巧”来修正过度的容量
 
 条款18: 避免使用vector<bool>
 
**关联容器**

 条款19: 了解相等和等价的区别
 
 条款20: 为包含指针的关联容器指定比较类型
 
 条款21: 永远让比较函数对相等的值返回false
 
 条款22: 避免对set和multiset的键值进行修改
 
 条款23: 考虑用排序的vector代替关联容器
 
 条款24: 当效率很关键时尽量用map::insert代替map::operator
 
 条款25: 让自己熟悉非标准的hash容器
 
**迭代器**

 条款26: 尽量使用iterator代替const_iterator，reverse_iterator和const_reverse_iterator
 
 条款27: 使用distance和advance把const_iterators转化成iterators
 
 条款28: 了解如何通过reverse_iterator的base得到iterator
 
 条款29: 需要一字符一字符输入时请用istreambuf_iterator
 
**算法**

 条款30: 确保目的范围足够大
 
 条款31: 了解你的排序选项
 
 条款32: 如果你真的想删除东西的话在remove-like的算法后紧接上erase
 
 条款33: 当心在包含指针的容器使用remove-like的算法
 
 条款34: 注意哪些算法需要排序过的范围
 
 条款35: 通过mismatch或lexicographical_compare实现简单的忽略大小写字符串比较
 
 条款36: 用not1和remove_copy_if来表现copy_if
 
 条款37: 用accumulate或for_each来统计序列
 
**仿函数，仿函数类，函数等等**

 条款38: 把仿函数类设计成值传递的
 
 条款39: 用纯函数做predicate
 
 条款40: 增强仿函数类的适应性
 
 条款41: 明确ptr_fun, mem_fun和mem_fun_ref的区别
 
 条款42: 保证less是operator<的意思
 
**用STL编程**

 条款43: 尽量用算法调用代替手写循环
 
 条款44: 尽量用成员函数代替同名的算法
 
 条款45: 注意count、find、binary_search、lower_bound、upper_bound和equal_range的区别
 
 条款46: 考虑用函数对象代替函数作为算法的参数
 
 条款47: 避免产生只写代码
 
 条款48: 总是#include适当的头文件
 
 条款49: 学会破解STL相关的编译器出错信息
 
 条款50: 让自己熟悉STL相关的网站
 



