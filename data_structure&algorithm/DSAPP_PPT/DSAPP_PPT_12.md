#邓俊辉数据结构与算法学习笔记-第十二章


### 文章目录
12.排序
	12. a1 快速排序算法
	12. a2 快速排序算法性能分析
	12. a3 快速排序变种
	12.b1 选取众数
	12.b3 选取通用算法
	12.c1 shell希尔排序
	12.c2 希尔排序逆序对


# 12.排序

## 12. a1 快速排序算法

<img src="https://img-blog.csdnimg.cn/20210507215101658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20210507215254110.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20210507215440859.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/2021050721555061.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

如上图，如何快速进行划分，找到轴点是关键。 

img src="https://img-blog.csdnimg.cn/20210507215749174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

如何交换？成本多高？ 

<img src="https://img-blog.csdnimg.cn/20210507215959732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20210507220410674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

## 12. a2 快速排序算法性能分析

<img src="https://img-blog.csdnimg.cn/20210507221335320.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="">

快排的平均性能 

<img src="https://img-blog.csdnimg.cn/20210507221959135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

## 12. a3 快速排序变种

<img src="https://img-blog.csdnimg.cn/20210523203655853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"> 

之前的算法中，区间U是在L和G之间的。 一轮遍历结束，找到了元素pivot的真正位置，只需要将lo处的pivot元素L区间的末元素互换。 

<img src="https://img-blog.csdnimg.cn/20210523205357705.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"> 

<img src="https://img-blog.csdnimg.cn/2021052321050517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"> 

算法是不稳定的，L区间元素加入顺序是稳定的，但是最后更新pivot位置时，可能引入不稳定的情况，G区间，由于滚动的前进，也会引入不稳定的顺序。

## 12.b1 选取众数

<img src="https://img-blog.csdnimg.cn/20210523211223597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

<img src="https://img-blog.csdnimg.cn/20210523211435762.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

<img src="https://img-blog.csdnimg.cn/20210523211755539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

<img src="https://img-blog.csdnimg.cn/20210523212029620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

<img src="https://img-blog.csdnimg.cn/20210523212556927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

<img src="https://img-blog.csdnimg.cn/2021052321283111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## 12.b3 选取通用算法

要取第k大的元素，通用方法
1、sort，然后取第k个元素，固有成本问题O(nlogn)
2、堆：
（a）:对n个元素先建小顶堆（弗洛伊德算法O(n)的复杂度），然后删除k次堆顶元素（O(klogn)）的复杂度。
（b）:对前k个元素建大顶堆（O(logk)）,对剩余（n-k）个元素各执行一次insert和delMax操作（O(2(n-k)logk)）,具体如下图，如果k非常大（接近n）或者非常小（接近1），此时算法复杂度接近于O(n),但是当k接近n/2时，算法复杂度接近于O(nlogn).
————————————————


 <img src="https://img-blog.csdnimg.cn/20210605204538823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

```
	(c)建立两个堆，如下图，但是在最坏的情况下，复杂度依然达到了O(nlogn)

```

<img src="https://img-blog.csdnimg.cn/20210605205105998.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

3、quick sort：利用快排的partition思路，如下图，缺点是虽然外部循环在通常意义下只有常数复杂度，但是在最坏情况下，可能达到O(n)复杂度 <img src="https://img-blog.csdnimg.cn/20210605205742817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

4、linear select：注意下面，当第k个元素在G中时，舍弃前面的L，E，递归调用linear select，同时k的值也要相应的减小，因为这里出入的是子短G，而不是G的前后index。 <img src="https://img-blog.csdnimg.cn/20210605210342189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

<img src="https://img-blog.csdnimg.cn/2021060521114423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> <img src="https://img-blog.csdnimg.cn/20210605211950421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> （这里真的好形象，太帅了吧）

<img src="https://img-blog.csdnimg.cn/20210605212353790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> <img src="https://img-blog.csdnimg.cn/20210605212428659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## 12.c1 shell希尔排序

<img src="https://img-blog.csdnimg.cn/20210605213333466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

希尔sort是一类算法，步长的不同，算法的性能也不同 下面看一个实例 

<img src="https://img-blog.csdnimg.cn/20210605213533538.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 
<img src="https://img-blog.csdnimg.cn/20210605213635690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
 <img src="https://img-blog.csdnimg.cn/20210605213752742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
 <img src="https://img-blog.csdnimg.cn/20210605213855174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
 <img src="https://img-blog.csdnimg.cn/2021060521400318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 
 实现细节 <img src="https://img-blog.csdnimg.cn/20210605214333678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
 <img src="https://img-blog.csdnimg.cn/20210605214558797.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 
 <img src="https://img-blog.csdnimg.cn/20210605215144853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## 12.c2 希尔排序逆序对

<img src="https://img-blog.csdnimg.cn/20210605215437837.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 
<img src="https://img-blog.csdnimg.cn/20210605215723925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 
<img src="https://img-blog.csdnimg.cn/2021060522001066.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

g-ordered指，经过g-sorted,间隔为g的序列是有序的 
<img src="https://img-blog.csdnimg.cn/20210605220349392.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

上图中，index凡是能表示为g和h的线性组合的，都是有序的。 

<img src="https://img-blog.csdnimg.cn/20210605220842918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 

基于上述性质，希尔排序对每一列的排序多采用插入排序（输入敏感），并且还设计了很多步长序列，来改进排序的性能。
