#邓俊辉数据结构与算法学习笔记-第五章


### 文章目录

树
	a
		a1 树
		a2 应用
		a3 有根树
		a4 有序树
		a5 路径
		a6 连通图无环图
		a7 深度层次
	b 在计算机中表示
		b1 树的表示
		b2 父节点
		b3 孩子节点
		b4 父亲孩子表示法
		b5 长子兄弟表示法
	c 二叉树
		c1 二叉树概述
		c2 真二叉树
		c3 描述多叉树
	d 二叉树
		d1 BinNode 类
		d2 BinNode 接口
		d3 BinTree类
		d4 高度更新
		d5 节点插入
	e 相关算法
		e1-1 先序遍历转化策略
		e1-2 遍历规则
		e1-3 递归实现
		e1-4 迭代实现
		e1-5 实例
		e1-6 新思路
		e1-7 新构思
		e1-8 迭代实现2
		e1-9 实例
		e2-1 中序遍历递归
		e2-2 观察
		e2-3 思路
		e2-4 构思
		e2-5 实现
		e2-6 实例
		e2-7 分摊分析
		e4-1 层次遍历次序
		e4-2 实现
		e4-3 实例
		e5-1 重构之遍历序列
		e5-2(先序或后序)与中序
		e5-3 (先序+后序列)真二叉树重建
	 


# 树

## a

### a1 树

<img src="https://img-blog.csdnimg.cn/20200912160348216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

tree将二者的优点结合起来，树是列表的列表List&lt; List&gt;

### a2 应用

层次关系的表示：表达式（如RPN），文件系统，URL，…

### a3 有根树

<img src="https://img-blog.csdnimg.cn/20200912161014839.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a4 有序树

<img src="https://img-blog.csdnimg.cn/202009121613054.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a5 路径

<img src="https://img-blog.csdnimg.cn/20200912161501943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a6 连通图无环图

<img src="https://img-blog.csdnimg.cn/20200912161852841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

根据上图右半部分，可以给每个节点划分指标，指标相同的节点称之为等价类。

### a7 深度层次

<img src="https://img-blog.csdnimg.cn/20200912162443376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

前驱（祖先）唯一，后继（后代）不唯一，所以说他是半线性结构。 

<img src="https://img-blog.csdnimg.cn/20200912162840945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## b 在计算机中表示

### b1 树的表示

<img src="https://img-blog.csdnimg.cn/20200912163030705.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b2 父节点

<img src="https://img-blog.csdnimg.cn/20200912163357112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200912163544848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
可以很容易的找到父亲

### b3 孩子节点

<img src="https://img-blog.csdnimg.cn/20200912163824420.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
可以很容易的找到孩子

### b4 父亲孩子表示法

<img src="https://img-blog.csdnimg.cn/2020091216412184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

上图对于孩子节点的存储，平均来说只有O（1）的规模，而采取上图的组织方式，有的时候需要长达O（n）的数据集。

### b5 长子兄弟表示法

<img src="https://img-blog.csdnimg.cn/20200912164927486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200912165024302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

在施加某种条件之后，二叉树可以用来表示所有的树（基于长子-兄弟法）

## c 二叉树

### c1 二叉树概述

<img src="https://img-blog.csdnimg.cn/20200912165655530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200912165821570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
当n = h+1,退化成一条单链；当n = 2^(h+1)-1时，为满二叉树 
<img src="https://img-blog.csdnimg.cn/20200912170014235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

对于一棵二叉树而言，他倾向于长宽（兔子快），高度增长缓慢（乌龟慢）

### c2 真二叉树

<img src="https://img-blog.csdnimg.cn/20200912170429478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">day22

### c3 描述多叉树

<img src="https://img-blog.csdnimg.cn/20200913154951515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## d 二叉树

### d1 BinNode 类

<img src="https://img-blog.csdnimg.cn/20200913155409262.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2 BinNode 接口

<img src="https://img-blog.csdnimg.cn/20200913160018215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d3 BinTree类

<img src="https://img-blog.csdnimg.cn/20200913160320553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d4 高度更新

<img src="https://img-blog.csdnimg.cn/20200913160723415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d5 节点插入

<img src="https://img-blog.csdnimg.cn/20200913194226307.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## e 相关算法

### e1-1 先序遍历转化策略

如何将【半线性结构】转换成【线性结构】，以利用之前造好的轮子

### e1-2 遍历规则

<img src="https://img-blog.csdnimg.cn/20200913194718749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e1-3 递归实现

<img src="https://img-blog.csdnimg.cn/20200913195047982.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

递归的写法，系统栈为了保证递归的通用性，占用空间较大，基于自己定义的栈完成递归，更精确，占用空间小。

### e1-4 迭代实现

<img src="https://img-blog.csdnimg.cn/20200913195405289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e1-5 实例

<img src="https://img-blog.csdnimg.cn/20200913195641832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

上图中的思路，不易推广到中序、后序遍历，所以需要寻找新的思路。

### e1-6 新思路

<img src="https://img-blog.csdnimg.cn/20200913200217718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

左侧链

### e1-7 新构思

<img src="https://img-blog.csdnimg.cn/202009132006127.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e1-8 迭代实现2

<img src="https://img-blog.csdnimg.cn/20200913200913509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20200913201006960.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e1-9 实例

<img src="https://img-blog.csdnimg.cn/20200913201338333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e2-1 中序遍历递归

<img src="https://img-blog.csdnimg.cn/2020091320284568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

对于中序遍历，上图可以看到，对于右子树的遍历是【尾递归】，而左子树的遍历不再是【尾递归】，改写起来难度更大。

### e2-2 观察

<img src="https://img-blog.csdnimg.cn/20200913203511957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e2-3 思路

<img src="https://img-blog.csdnimg.cn/20200913203834690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e2-4 构思

<img src="https://img-blog.csdnimg.cn/2020091320410851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e2-5 实现

<img src="https://img-blog.csdnimg.cn/20200913204232256.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/2020091320433387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e2-6 实例

<img src="https://img-blog.csdnimg.cn/2020091320462194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e2-7 分摊分析

<img src="https://img-blog.csdnimg.cn/20200913205107239.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

后序遍历：中序遍历中，弹出的x(根)先不直接执行访问，先执行右子树操作（猜测，见书，可以去学堂在线看看有无） 参考博客：https://www.jianshu.com/p/eb6f4de6efbd

### e4-1 层次遍历次序

<img src="https://img-blog.csdnimg.cn/20200913210313816.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e4-2 实现

<img src="https://img-blog.csdnimg.cn/20200913210454995.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e4-3 实例

<img src="https://img-blog.csdnimg.cn/20200913211016625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e5-1 重构之遍历序列

<img src="https://img-blog.csdnimg.cn/20200913211125163.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e5-2(先序或后序)与中序

<img src="https://img-blog.csdnimg.cn/20200913211521342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20200913211652621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

不能基于前序遍历序列和后续遍历序列来完成重建，因为有可能出现左或右子树为空的情况（如上图右），会出现歧义，当去除根节点，不知道是左子树还是右子树。

### e5-3 (先序+后序列)真二叉树重建

<img src="https://img-blog.csdnimg.cn/2020091321215958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
完成代码，开始第六章
