# DSACPP学习笔记-第八章


### 文章目录

	8.a 伸展树
		8.a1 伸展树逐层伸展
		8.a2 伸展树双层伸展
		8.a3 伸展树算法实现
	8.b B树
		8.b1 B树动机
		8.b2 B树结构
		8.b3 B树查找
		8.b4 B树插入
		8.b5 B树的删除
	8.xa 红黑树
		8.xa1 红黑树动机
		8.xa2 红黑树结构
		8.xa3 红黑树插入
		8.xa4 红黑树删
 

# 8 高级搜索树

## 8.a 伸展树

### 8.a1 伸展树逐层伸展

<img src="https://img-blog.csdnimg.cn/20201022144544164.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201022145121441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

列表前端访问效率更高，树根位置访问效率更高 
<img src="https://img-blog.csdnimg.cn/20201022145346851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

反复使用等价变换（zig/zag）,是的v的高度逐层上升，最终转移至树根，具体示例如下图 

<img src="https://img-blog.csdnimg.cn/20201022145920732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201022150110959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201022150624811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

最坏情况时，与AVL树的log(n)度相比，差很多，甚至退化成了List/Vector,主要原因还是splay(伸展)的策略不对，而不是伸展本身

### 8.a2 伸展树双层伸展

<img src="https://img-blog.csdnimg.cn/20201022151027746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201022151244627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201022151523308.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

二者局部结构有微妙的差异，但是这种差异是颠覆性的 

<img src="https://img-blog.csdnimg.cn/20201022152357988.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201022152521657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### 8.a3 伸展树算法实现

<img src="https://img-blog.csdnimg.cn/20201022152722791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201022153012280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201022153215731.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201022153358266.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201022153653716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201022153957875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

图中m节点是右子树中的最小的节点 
<img src="https://img-blog.csdnimg.cn/20201022154614332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

图中的nlogn项，是在达到这种状态之前需要的时间复杂度。复杂度的严格证明，参考习题8-2 day49

## 8.b B树

### 8.b1 B树动机

动机：高效I/O 

<img src="https://img-blog.csdnimg.cn/20201026154405645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/2020102615465459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/2020102615521821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201026155359864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
边际时间成本

### 8.b2 B树结构

<img src="https://img-blog.csdnimg.cn/20201026160103861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201026160449430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201026160739933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201026161002874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

（2，4）树在B树中有比较特殊的地位，其与红黑树有不解的渊源 

<img src="https://img-blog.csdnimg.cn/20201026161356516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201026161546681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201026161713224.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### 8.b3 B树查找

假设根节点已经在内存中 <img src="https://img-blog.csdnimg.cn/20201026162314403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

利用外部节点可以将位于不同存储级别的B树串接起来，构成更大的B树。 

<img src="https://img-blog.csdnimg.cn/20201026163059648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201026163426408.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201026164316182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

关键码的数目大概是几百，对于如此规模的查找，二分查找的效率不如顺序查找 

<img src="https://img-blog.csdnimg.cn/202010261650201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201026170015647.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201026170409633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201026170900733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

可以看出，当B树的关键码总数 N 固定时，B树的高度浮动是非常有限的，几乎不发生变化。

### 8.b4 B树插入

<img src="https://img-blog.csdnimg.cn/20201028210925554.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

注意，这里的第一个search函数，是前面二叉树的search函数，如果找不到，就返回NULL，并且更新_hot，而第二种search函数向量的查找算法，会返回一个恰当的 r,便于后续的插入。 
<img src="https://img-blog.csdnimg.cn/202010282138551.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

节点关键码数不超过m-1，分支数不超过m; 分裂后左右孩子的关键码数目也不会低于下限 

<img src="https://img-blog.csdnimg.cn/20201028214414343.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

当上溢传播到根的时候，整个树会长高一层（B树长高的唯一原因），这个时候根节点只有一个关键码，其分支只有两个，这也就是前面对根节点的关键码数下限单独定义的原因。

### 8.b5 B树的删除

<img src="https://img-blog.csdnimg.cn/20201028215550287.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201028220320362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

同理，当右孩子节点关键码数目充足时，而左孩子节点关键码数目下溢时，也可以向右孩子“借”关键码，这样右孩子以后也不容易上溢。(左顾右盼) <img src="https://img-blog.csdnimg.cn/20201028220956474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

由于这个合并是向父节点借了一个关键码，所以父节点可能会继续发生下溢，最坏的情况是下溢传播的树根，整个的复杂度不超过O(h)。可能会导致树的高度减一。 
<img src="https://img-blog.csdnimg.cn/20201028222144825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

光线的折射，来实现速度的最优，B-Tree通过改变形态来适应内存外存读取速度的不同，也是在模仿这一种自然法则。 https://zhidao.baidu.com/question/1993560025571237907.html day53

## 8.xa 红黑树

### 8.xa1 红黑树动机

<img src="https://img-blog.csdnimg.cn/20201031114049377.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

累计的时间复杂度是指，为了生成和记录所有的快照累计的时间复杂度 挑战：除了生成元素需要O(n),能否将每一个版本控制在O(h*logn)范围内？ 可以，但需要利用相邻版本之间的关联性 

<img src="https://img-blog.csdnimg.cn/20201031115200509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">红线表示共享，蓝线表示更新。 

<img src="https://img-blog.csdnimg.cn/20201031115520795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">所以需要引入一种树结构，他的任何一次动态操作引发的结构变化量不致超过O(1)

### 8.xa2 红黑树结构

<img src="https://img-blog.csdnimg.cn/20201031145435939.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201031145613299.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

假象的外部节点（真二叉树） 

<img src="https://img-blog.csdnimg.cn/202010311458324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

提升变换（lifting） <img src="https://img-blog.csdnimg.cn/20201031150524799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201031151151613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

其中H为黑高度，将红黑树进行提升变换，变成（2，4）树，其高度恰好就是黑高度H，再由前面B-Tree部分证明的高度的范伟可进一步推至红黑树高度h的范围。 <img src="https://img-blog.csdnimg.cn/20201031152013552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

这里的高度是指黑高度H

### 8.xa3 红黑树插入

<img src="https://img-blog.csdnimg.cn/20201031152225200.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

以B-Tree为引子 <img src="https://img-blog.csdnimg.cn/2020103115263358.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/2020103115275695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201031153037655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
对于(a’)只需要交换p和g的颜色即可，对于(b’)只需要交换x和g的颜色即可 
<img src="https://img-blog.csdnimg.cn/20201031153409707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

全树拓扑连接关系变化量不超过O(1)，因为只进行了局部拓扑结构调整。 <img src="https://img-blog.csdnimg.cn/20201031153754591.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201031154220933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

此过程，双红缺陷可能会进一步向上转移，但不外乎还是前面的提到的两种情况，在此过程中虽然重染色的复杂度可能高达log(n)，但是树的拓扑结构的变化还是O(1) <img src="https://img-blog.csdnimg.cn/20201031154822927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201031154839486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

重构次数为常量，拓扑结构变换为常量。AVL树的插入操作也是满足这个的，但是其删除操作不满足，存在多次重构。

### 8.xa4 红黑树删除

<img src="https://img-blog.csdnimg.cn/20201031155624480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201031155852221.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201031160232675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201031160515401.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

上图中s是继承p的颜色 <img src="https://img-blog.csdnimg.cn/20201031160951581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201031161255927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

虽然下溢会向上传播，但是重构操作的次数仍然控制在O(1) <img src="https://img-blog.csdnimg.cn/20201031161800224.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201031162302947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

红黑树的特性，对于持久性结构的实现至关重要 完成代码，开始第九章p61


# DSACPP学习笔记-第九章 词典

### 文章目录

9.词典

	9.b 散列原理
	9.c 散列函数
	9.d 冲突
	9.d1 散列排解冲突1
	9.d2 散列排解冲突2
	9.e 桶/计数排序


# 9.词典

## 9.b 散列原理

<img src="https://img-blog.csdnimg.cn/20201106162103646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106162722890.png#pic_center" alt="在这里插入图片描述">

Hashing-散列 

<img src="https://img-blog.csdnimg.cn/2020110616313676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

数据来源于一个相当大的空间，但是实际要存储和组织的数据是其中很小的子集，空间效率极低 

<img src="https://img-blog.csdnimg.cn/20201106163639923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

压缩空间 

<img src="https://img-blog.csdnimg.cn/2020110616395061.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106164221636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

鸽巢问题，将x从比较大的定义域，映射到更小的值域，可以采取一定的手段减少冲突（设计更好的散列函数或者增大散列表长M），但是无法避免（如何解决？）

## 9.c 散列函数

<img src="https://img-blog.csdnimg.cn/20201106164743504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/2020110616500116.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106165137930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106170022156.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

gcd:最大公因子为1 

<img src="https://img-blog.csdnimg.cn/20201106170927515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106171051843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106173431689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

取中间的若干位，可以使原关键码的各数位对地址的影响彼此更为接近，如下图，平方运算可以分解成加法运算 

<img src="https://img-blog.csdnimg.cn/20201106173634190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106175243512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

散列函数越是随机，越是没有规律，越好。 

<img src="https://img-blog.csdnimg.cn/20201106175554419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106175643385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

key可能不是整数，需要先将其转换为hashcode,再做处理，如下图 

<img src="https://img-blog.csdnimg.cn/20201106175913532.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106180229160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

有必要，如果使用简单的计算方法，很容易出现哈希冲突

 <img src="https://img-blog.csdnimg.cn/20201106180639615.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## 9 d 冲突

### 9.d1 散列排解冲突1

<img src="https://img-blog.csdnimg.cn/20201106200434539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106200716326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106201007601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106201343204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106201557674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106201805611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106202122143.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106202152321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

查找时跳过，插入时直接插入。

### 9.d2 散列排解冲突2

<img src="https://img-blog.csdnimg.cn/2020110620330576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

封闭定址 vs 开放定址 上面介绍的线性试探，试探位置间距太近，会造成很多不必要的冲突。可以适当的拉开间距 
<img src="https://img-blog.csdnimg.cn/20201106203639502.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201106204142489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

假设一次缓存的大小是1KB，桶中只存储指针（4字节），那么除非发生16次散列冲突，才会使得缓存失效，需要额外的I/O。 
<img src="https://img-blog.csdnimg.cn/20201106204600480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/2020110620480115.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

b+a≥2，是M的非平凡因子，这与M为素数相矛盾。 

<img src="https://img-blog.csdnimg.cn/20201106205341913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106205758770.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

对于有些素数（例如7，11）的表长，双向查找链行之有效，有些素数（例如5，13）的表长效果不好. 

<img src="https://img-blog.csdnimg.cn/20201106210153257.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106210410731.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106210718673.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201106211006646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## 9.e 桶/计数排序

<img src="https://img-blog.csdnimg.cn/20201108170352273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">其中n为元素个数，[0,M)为元素范围 
<img src="https://img-blog.csdnimg.cn/20201108170435501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201108171028213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201108171348837.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">其中红线是蓝线的积分 
<img src="https://img-blog.csdnimg.cn/20201108171546120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">线性扫描一遍得到count[],再扫描一遍count[]得到accum[],就可以在线性时间完成排序。


# DSACPP学习笔记-第十章

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

## 10 xa

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


# DSACPP学习笔记-第十一章


### 文章目录

11.串
	11.a ADT
	11.b 串匹配
		11.b1 串匹配
		11.b2 蛮力匹配
	11.c KMP算法
		11.c1 KMP算法：从记忆力到预知力
		11.c2 KMP算法查表
		11.c3 KMP算法理解next表
		11.c4 KMP算法构造next表
		11.c5 KMP算法分摊分析
		11.c6 KMP算法再改进
	11.d BM_BC算法
		11.d1 BM_BC算法 以终为始
		11.d2 BM_BC算法 坏字符
		11.d3 BM_BC算法 构造BC表
		11.d3 BM_BC算法 算法性能分析
		11.e1 BM_GS算法 好后缀
		11.e2 BM_GS算法：构造GS表
		11.e3 BM_GS算法：综合性能
		11.f1 Karp-Rabin算法：串即是数（整数）
		11.f2 Karp-Rabin算法：散列
 
# 11.串

## 11.a ADT

<img src="https://img-blog.csdnimg.cn/20201218160113884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201218160542658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

真子串，真前缀，真后缀

<img src="https://img-blog.csdnimg.cn/20201218160847671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

## 11.b 串匹配

### 11.b1 串匹配

<img src="https://img-blog.csdnimg.cn/2020121816461072.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201218165529732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

因为如果采用随机算法，匹配成功的概率非常低，所以在测评算法复杂度时，将匹配失败和匹配成功分开考虑，分别计算算法复杂度。

### 11.b2 蛮力匹配

<img src="https://img-blog.csdnimg.cn/20201218174608451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/2020121817522626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201218175444583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201218180150257.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201218180231755.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

蛮力算法的效率之所以很低，是因为他不足以处理这种大量的局部匹配（前面都匹配成功，到最后匹配失败），字母表越小，最坏情况出现的概率越高，随着字母表的增大，最坏情况出现的概率降低（一般在匹配初期就会发现匹配失败），可以达到期望的的O(n)复杂度。

## 11.c KMP算法

### 11.c1 KMP算法：从记忆力到预知力

<img src="https://img-blog.csdnimg.cn/2020121818094944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

存在大量局部匹配的前缀 

<img src="https://img-blog.csdnimg.cn/20201218181219172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201218181352602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

两个优点：①大幅度的向后滑动模式串，而不是每次只滑动一个字符②可以避免大量重复的比对 

<img src="https://img-blog.csdnimg.cn/20201218182201826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

匹配失败后，要如何确定下一次匹配的起始位置？

### 11.c2 KMP算法查表

<img src="https://img-blog.csdnimg.cn/20201218183526965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201218183843850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201218184259689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
可以排除不必要的对其位置

### 11.c3 KMP算法理解next表

<img src="https://img-blog.csdnimg.cn/20201218185022146.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

t的取值很多，选择最大的t,KMP算法舍弃的那些t，都是不值得对其的位置。 

<img src="https://img-blog.csdnimg.cn/20201218185432275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

匹配失败的问题：在P串前面放置一个哨兵（通配符），通过设置哨兵可以：简化代码，统一理解。 虚拟实验：既是物理学的有效研究方法，也是计算机科学的重要技巧

### 11.c4 KMP算法构造next表

<img src="https://img-blog.csdnimg.cn/2020123017144394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201230174255801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20201230174702420.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### 11.c5 KMP算法分摊分析

<img src="https://img-blog.csdnimg.cn/20201230200538537.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

上面为KMP算法的复杂度，相应的next的构造算法与KMP算法别无二致，只不过是模式串与模式串本身之间的比较，所以其复杂度为O(m),m为模式串的额长度。

### 11.c6 KMP算法再改进

<img src="https://img-blog.csdnimg.cn/20201230202030597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

后续的三次比对都是多余的；目前的KMP算法只是吸取了经验，未接收教训 
<img src="https://img-blog.csdnimg.cn/2020123020270827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

避免一错再错 

<img src="https://img-blog.csdnimg.cn/20201230203419456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20201230203218580.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

除了在端点处之外，没有任何重合；复杂度为O(2n) 

<img src="https://img-blog.csdnimg.cn/20201230203618181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

对于蛮力算法，在最好情况下，可能只需要经过一次比对，就可以排除掉一个对齐位置，此时他的复杂度为O(n),在实际生活中，随着字符集规模的增大，这种最好情况发生的概率会逐渐增大，KMP算法相比于蛮力算法的优势会越来越小。

## 11.d BM_BC算法

### 11.d1 BM_BC算法 以终为始

<img src="https://img-blog.csdnimg.cn/20210113175408865.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

判断两个字符串是否相等与判断其是否不等，效率是不一样的。 

<img src="https://img-blog.csdnimg.cn/20210113175659513.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

如何高效排除无效的对齐位置？更多的关注靠后的教训（失败比对），价值更高，如下图，靠后的教训可以帮助排除更多的无效对齐位置。 

<img src="https://img-blog.csdnimg.cn/20210113180212212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

图中红色字符意思：当字符集数目足够大，串匹配成功的概率远小于失败的概率 

<img src="https://img-blog.csdnimg.cn/20210113181043694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

灰色为失败的比对，黑色为成功的比对

### 11.d2 BM_BC算法 坏字符

<img src="https://img-blog.csdnimg.cn/20210113181525774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

和KMP算法一样，因为与文本串无关，只与模式串有关，所以位移量可以预先计算出来，生成BC表 

<img src="https://img-blog.csdnimg.cn/20210113182023181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

特殊情况要考虑：①模式串中有多个X，选择秩最大的②模式串中无X，在串开头增设通配符哨兵 

<img src="https://img-blog.csdnimg.cn/20210113182457177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

③秩过大，超过了j,只需要将整个模式串右移一个位置

### 11.d3 BM_BC算法 构造BC表

<img src="https://img-blog.csdnimg.cn/2021030311025930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### 11.d3 BM_BC算法 算法性能分析

<img src="https://img-blog.csdnimg.cn/2021030311073687.png" alt="在这里插入图片描述"> 

<img src="https://img-blog.csdnimg.cn/20210303110916725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20210303111552689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

上图为BM_BC算法在最坏情况下的复杂度O(n*m)，最坏时退化为蛮力算法，究其原因还是目前BM_BC算法只是利用了教训（BC-坏字符策略），没能很好的利用经验（GS-好后缀）。

### 11.e1 BM_GS算法 好后缀

<img src="https://img-blog.csdnimg.cn/20210507174534411.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

上述是BM_BC算法匹配的过程，由于只关注教训，没利用经验，导致前两次匹配，每次只能挪动一步。 <img src="https://img-blog.csdnimg.cn/20210507174819726.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/2021050717571042.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

如果模式串中有多个于V(k)匹配的，应当选最靠右且串的前置字符不为Y（X与Y已经不匹配了，至少不能在选一个Y），使得位移量尽可能地小。当然有可能不存在与V(k)匹配的子串，此时应当去模式串前缀与V(k)的后缀匹配最长者。下图为一个实例： <img src="https://img-blog.csdnimg.cn/20210507180414701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### 11.e2 BM_GS算法：构造GS表

<img src="https://img-blog.csdnimg.cn/202105071808515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

怎么构造没看懂，查一下

### 11.e3 BM_GS算法：综合性能

<img src="https://img-blog.csdnimg.cn/20210507181732983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20210507181751764.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

综合复杂度分析如下图，其中|Σ|代表字母表的大小，字母表越大，单词匹配成功的概率（Pr）越低，蛮力算法效果越好。 <img src="https://img-blog.csdnimg.cn/2021050718212850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### 11.f1 Karp-Rabin算法：串即是数（整数）

凡物皆数 <img src="https://img-blog.csdnimg.cn/20210507210336716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

自然数向量和自然数之间可以彼此转化 

<img src="https://img-blog.csdnimg.cn/20210507213108912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### 11.f2 Karp-Rabin算法：散列

<img src="https://img-blog.csdnimg.cn/2021050721354634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20210507213947483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

问题：散列冲突；解决–哈希只用来筛选，还要进行进一步的精确匹配 

<img src="https://img-blog.csdnimg.cn/20210507214248318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20210507214909247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">


#DSACPP学习笔记-第十二章


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

