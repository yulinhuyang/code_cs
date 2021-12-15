#邓俊辉数据结构与算法学习笔记-第四章


### 文章目录

栈和队列

	a 栈的接口与实现
		a1 栈
		a2 实例
		a3 实现
	c
		c1-1 栈的典型应用
		c1-2 进制转换算法
		c1-3实现
		c2-1括号匹配实例
		c2-2 尝试
		c2-3 构思
		c2-4 实现
		c2-5 反思
		c2-6 扩展
		c3-1 栈混洗
		c3-2 计数
		c3-3 甄别
		c3-4 算法
		c3-5 括号
		c4-1 中缀表达式
		c4-2 构思
		c4-3 实例
		c4-4 算法框架
		c4-5 算法细节
		c4-6 实例
		c5-1 逆波兰表达式简化
		c5-2 体验
		c5-3 手工
		c5-4 RPN转换算法
	d 队列
		d1 队列接口
		d2 实例
		d3 实现


# 栈和队列

day17

## a 栈的接口与实现

### a1 栈

<img src="https://img-blog.csdnimg.cn/20200907113932611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a2 实例

<img src="https://img-blog.csdnimg.cn/2020090711425056.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">LIFO:后进先出

### a3 实现

<img src="https://img-blog.csdnimg.cn/20200907114622437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

上图为基于向量模拟实现栈，其中入栈、出栈操作都是在向量末尾完成，复杂度是O（1），如果在向量首部实现入栈、出栈的话，复杂度就会变成O（n）

## c

### c1-1 栈的典型应用

<img src="https://img-blog.csdnimg.cn/20200907114922177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

栈的典型应用场合

### c1-2 进制转换算法

<img src="https://img-blog.csdnimg.cn/20200907115402899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

计算过程自上而下，输出结果，自下而上，可以应用栈存储输出结果

### c1-3实现

<img src="https://img-blog.csdnimg.cn/20200907115657190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c2-1括号匹配实例

<img src="https://img-blog.csdnimg.cn/20200907115849422.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c2-2 尝试

<img src="https://img-blog.csdnimg.cn/20200907120112493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c2-3 构思

<img src="https://img-blog.csdnimg.cn/20200907120811833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c2-4 实现

<img src="https://img-blog.csdnimg.cn/20200907120929144.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c2-5 反思

只有一种括号时，使用计数器也可以实现

### c2-6 扩展

<img src="https://img-blog.csdnimg.cn/20200907121404485.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c3-1 栈混洗

<img src="https://img-blog.csdnimg.cn/20200907152748484.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

栈混洗后结果如下 
<img src="https://img-blog.csdnimg.cn/20200907152827160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200907152912985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c3-2 计数

<img src="https://img-blog.csdnimg.cn/20200907153205778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

catalan数：https://www.cnblogs.com/Morning-Glory/p/11747744.html

### c3-3 甄别

<img src="https://img-blog.csdnimg.cn/2020090715354833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c3-4 算法

<img src="https://img-blog.csdnimg.cn/20200907153822314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

编程练习：利用堆栈完成栈混洗模拟实现的编程（√）

### c3-5 括号

<img src="https://img-blog.csdnimg.cn/20200907154107247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">push换成左括号，pop对应于一个右括号，发现同一元素对应的push/pop操作恰好对应于一对匹配的括号。反过来由n对括号构成的任何一个合法的表达式，也可以解释为对n个元素进行栈混洗的一个合法的过程。总结来说，n个元素的合法的栈混洗有多少种，n对括号的合法的表达式就有多少种，二者一 一对应。

### c4-1 中缀表达式

<img src="https://img-blog.csdnimg.cn/20200907154726725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

这些计算是如何实现的？

### c4-2 构思

<img src="https://img-blog.csdnimg.cn/20200907155128137.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

优先计算优先级高的运算符 <img src="https://img-blog.csdnimg.cn/20200907155211850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">延迟缓冲：线性扫描，找到优先级高的先计算，优先级不定的先缓冲起来，接着向下扫描。

### c4-3 实例

<img src="https://img-blog.csdnimg.cn/20200907155704925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c4-4 算法框架

<img src="https://img-blog.csdnimg.cn/2020090715590026.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200907160037387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c4-5 算法细节

<img src="https://img-blog.csdnimg.cn/20200907160431432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200907160610899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

上图解释了程序代码中case ‘=’，即运算符优先级相等时，代码编写的思路。上图中定义了左括号与右括号的优先级相等，当遍历出现右括号时，此时栈顶一定是与之对应的左括号，且二者包含的计算式在之前已经被计算出来了，所以直接将左括号出栈，跳过右括号继续向下遍历即可。 对于\0，其作用也相当于一对括号。

### c4-6 实例

<img src="https://img-blog.csdnimg.cn/2020090716265445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

其他运算符乐于接收左括号，左括号也乐于接收其他运算符 \0,右括号乐于促成栈顶运算执行

### c5-1 逆波兰表达式简化

<img src="https://img-blog.csdnimg.cn/20200907163115197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

逆波兰表达式不使用括号表达优先级，而是将运算的优先级体现为运算符在表达式中出现的次序，谁先出现，谁先计算。

### c5-2 体验

<img src="https://img-blog.csdnimg.cn/2020090717101760.png#pic_center" alt="在这里插入图片描述"> 将中缀表达式转换为RPN，只需要一个堆栈，即可完成运算

### c5-3 手工

<img src="https://img-blog.csdnimg.cn/2020090717150047.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

转换之后，操作数的顺序不会改变

### c5-4 RPN转换算法

<img src="https://img-blog.csdnimg.cn/20200907171817950.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## d 队列

### d1 队列接口

<img src="https://img-blog.csdnimg.cn/20200907172125784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">enqueue;dequeue

### d2 实例

<img src="https://img-blog.csdnimg.cn/20200907172333667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d3 实现

<img src="https://img-blog.csdnimg.cn/202009071727454.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">开始第五章
