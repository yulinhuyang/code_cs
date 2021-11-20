#邓俊辉数据结构与算法学习笔记-第十章


### 文章目录

第十章 优先级队列
10.a
	10.a1 需求与动机
	10.a2 基本实现
	10.b
	10.b1 完全二叉堆结构
	10.b2 完全二叉堆插入与上滤
	10.b3 完全二叉堆删除与下滤
	10.b4 完全二叉堆: 批量建堆
	10.c
	10.c 堆排序算法
	10.xa
	10.xa1 左式堆：结构
	10.xa1 左式堆合并
 

# 第十章 优先级队列

## 10.a

### 10.a1 需求与动机

<img src="https://img-blog.csdnimg.cn/20201117104127134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117104215903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117104548376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117104829658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### 10.a2 基本实现

实现：考虑 efficiency + cost(效率+成本) 

<img src="https://img-blog.csdnimg.cn/20201117105031712.png#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117105143321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117105427399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117105447566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117105508891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117105809648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

杀鸡焉用牛刀

## 10.b

### 10.b1 完全二叉堆结构

<img src="https://img-blog.csdnimg.cn/20201117105907319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/2020111711021887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117110609308.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

Complete Binary Heap 

<img src="https://img-blog.csdnimg.cn/20201117110816302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201117111123752.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201117111254594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### 10.b2 完全二叉堆插入与上滤

<img src="https://img-blog.csdnimg.cn/20201117112013431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

这一过程，即所谓的上滤（percolate up） 

<img src="https://img-blog.csdnimg.cn/20201117112518960.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201117112801511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201117113251129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

在上滤时先将e暂存起来，直到找到e的最终位置，再赋值e，可以降低常数复杂度。 比较操作的复杂度也可以进一步改进。 可以证明，在常规分布下，每个节点上升的平均高度，是常数O(1)。

### 10.b3 完全二叉堆删除与下滤

<img src="https://img-blog.csdnimg.cn/20201117114028201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201117114333967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117114552701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117114843988.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

comparsion的比较复杂度在多叉堆（d-heap）中将会变得至关重要。

### 10.b4 完全二叉堆: 批量建堆

<img src="https://img-blog.csdnimg.cn/20201117144613972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117144855780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117145333553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

|**n/2**| - 1 为最末尾的内部节点的秩 

<img src="https://img-blog.csdnimg.cn/20201117145750330.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

调整顺序，内部节点由下向上，由右向左 

<img src="https://img-blog.csdnimg.cn/20201117150147568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

Floyd算法（方法2）：高收入者多纳税（金字塔高层节点少，高度height高）；低收入者多纳税（金字塔底层节点多，高度height低）

## 10.c

### 10.c 堆排序算法

<img src="https://img-blog.csdnimg.cn/2020111715084143.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201117151539563.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

反复的交换，下滤 
<img src="https://img-blog.csdnimg.cn/20201117151807838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117151930596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117152123517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/202011171522148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## 10.xa

### 10.xa1 左式堆：结构

<img src="https://img-blog.csdnimg.cn/20201117152802735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

方法二令人不满意之处：Floyd算法是基于完全无序的向量构建堆，但是我们现在的向量是前面一部分有序，后面一部分有序，Floyd算法不能很好的利用这个有序性。左式堆正是解决这个问题很好的答案。
 <img src="https://img-blog.csdnimg.cn/20201117153706471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
堆序性才是堆结构的本质要求 
<img src="https://img-blog.csdnimg.cn/20201117154232920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117154449533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117154806326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### 10.xa1 左式堆合并

<img src="https://img-blog.csdnimg.cn/20201117155014629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117155137745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117155424299.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117155944562.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

合并是围绕右侧链进行的，前面有提到rChain = O(logn)，所以合并的时间复杂度为O(logn) 

<img src="https://img-blog.csdnimg.cn/20201117160414599.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201117160629417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

C.A.Crane是上面merge算法的发明者，同时他对对AVL树也给出了高效的合并算法
