#邓俊辉数据结构与算法学习笔记-第一章


### 文章目录
	a
		a1.计算
		a2.算法
	b
		b1. 计算模型
		b2.图灵机
		b3. RAM(random access machine)
	c
		c1. 大O记号
		c2. big Ω，big Θ
		c3.复杂度总结
	d
		d1.算法分析
		d2.级数
		d3.循环与级数
		d4 取非极端元素、冒泡排序
		d5 起泡排序的分析
		d6 封底估算
		d7封底估算实例
	e 迭代与递归
		e1 迭代和递归
		e2 减而治之
		e3 递归跟踪、递推方程
		e4数组倒置
		e5 分而治之
		e6 例 数组求和--二分递归
		e7 例 MAX2
	f 动态规划
		f1 动态规划
		f2 fib递推方程
		f3 封底估算
		f4 fib()递归跟踪
		f5 fib()回归迭代
		f6 最长公共子序列
		f7 递归LCS
		f8 理解LCS
		f9 动态规划LCS


# 1.绪论

## a

### a1.计算

day1

对象：规律、技巧
目标：高效、低耗
计算机是工具和手段，而计算才是目标
绳索计算机及其算法（勾股定理）

<img src="https://img-blog.csdnimg.cn/20200731214156268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">尺规计算及其算法（相似三角形） 

<img src="https://img-blog.csdnimg.cn/20200731214556761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### a2.算法

●计算 = 信息处理 借助某种工具，遵照一定规则，以明确而机械的形式进行 ●算法，特定计算模型下，旨在解决特定问题的指令序列 <img src="https://img-blog.csdnimg.cn/20200731214958539.png" alt="在这里插入图片描述"> 

●算法：有穷性 

<img src="https://img-blog.csdnimg.cn/20200731215851145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

程序未必是算法：比如程序死循环 ●好算法 

<img src="https://img-blog.csdnimg.cn/20200801175120601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

## b

### b1. 计算模型

to **measure** is to know ●算法分析 两个主要方面：正确性（数学证明）和**成本**（时间和空间成本） ●成本 <img src="https://img-blog.csdnimg.cn/2020080118115274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200801181600569.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200804104402153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### b2.图灵机

<img src="https://img-blog.csdnimg.cn/20200804104821643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200804105351145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### b3. RAM(random access machine)

<img src="https://img-blog.csdnimg.cn/20200804105815265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200804111429834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"> 图灵机模型和RAM模型都是尺子

## c

### c1. 大O记号

渐进分析：在问题规模足够大后，计算成本如何增长（更关心足够大的问题） 需执行的基本操作次数：T(n) 需占用的存储单元数：S(n) <img src="https://img-blog.csdnimg.cn/20200804112430430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### c2. big Ω，big Θ

<img src="https://img-blog.csdnimg.cn/20200804112657758.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">

### c3.复杂度总结

<img src="https://img-blog.csdnimg.cn/20200804112934436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200804113232507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/2020080411362769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"> day4 <img src="https://img-blog.csdnimg.cn/20200825214744793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200825215229542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200825215700926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## d

### d1.算法分析

<img src="https://img-blog.csdnimg.cn/20200825220342678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">day9

### d2.级数

<img src="https://img-blog.csdnimg.cn/20200829173038191.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200829173534635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"> 将一个循环程序等效为不断的投硬币，直到第一次出现反面朝上。（正面朝上概率为 λ）需要投掷的次数可能是1次、2次、3次，…,符合几何分布，可以求解需要投掷次数的期望为1/（1 - λ）

### d3.循环与级数

<img src="https://img-blog.csdnimg.cn/20200829173849649.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200829174147515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">思考题 <img src="https://img-blog.csdnimg.cn/20200829181056208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">day10

### d4 取非极端元素、冒泡排序

<img src="https://img-blog.csdnimg.cn/20200831152158440.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200831152553112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d5 起泡排序的分析

<img src="https://img-blog.csdnimg.cn/20200831153124992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d6 封底估算

案例：估算地球的赤道的周长 <img src="https://img-blog.csdnimg.cn/20200831154021282.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d7封底估算实例

<img src="https://img-blog.csdnimg.cn/20200831154518390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">在“三生三世”中的“1天”，相当于“1天”中的“1秒” 在“整个宇宙生命中”的“三生三世”，相当于在“三生三世”中的“0.1秒”（比例运算） <img src="https://img-blog.csdnimg.cn/20200831155253942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## e 迭代与递归

### e1 迭代和递归

<img src="https://img-blog.csdnimg.cn/20200831160056779.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e2 减而治之

<img src="https://img-blog.csdnimg.cn/20200831160252376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e3 递归跟踪、递推方程

<img src="https://img-blog.csdnimg.cn/20200831160915110.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200831161231967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e4数组倒置

<img src="https://img-blog.csdnimg.cn/20200831165838484.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e5 分而治之

<img src="https://img-blog.csdnimg.cn/2020083117000788.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e6 例 数组求和–二分递归

<img src="https://img-blog.csdnimg.cn/20200831170247293.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">上图复杂度分析：画图分析 <img src="https://img-blog.csdnimg.cn/2020083117070521.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">上图O(n)的复杂度结果，可以直接套用前面几何级数的结果得出，从渐进的角度来说，最后一层的计算复杂度可以代表整体的复杂度。 上图复杂度分析：基于递归方程分析 <img src="https://img-blog.csdnimg.cn/20200831171008147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e7 例 MAX2

<img src="https://img-blog.csdnimg.cn/20200831171617524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200831172026102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200831172900368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## f 动态规划

### f1 动态规划

由递归得到算法的本质，再将其转化为迭代

### f2 fib递推方程

<img src="https://img-blog.csdnimg.cn/20200831211157611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">假设演绎法可以得出S(n) = fib(n+1)

### f3 封底估算

<img src="https://img-blog.csdnimg.cn/2020083121170919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">10^9 flo:为一台普通计算机1秒可以做的运算次数。

### f4 fib()递归跟踪

<img src="https://img-blog.csdnimg.cn/20200831212259776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">上台阶问题：每次只能上一级或两级台阶，问到第n级台阶一共有多少种上去的方法，当n＞2时，fib(n) = fib(n-1) + fib(n-2);(最近一次是上一级台阶 or 最近一次是上两级台阶)，且f[1]=1,f[2]=2;

### f5 fib()回归迭代

<img src="https://img-blog.csdnimg.cn/20200831214633470.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### f6 最长公共子序列

<img src="https://img-blog.csdnimg.cn/20200831215053596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### f7 递归LCS

<img src="https://img-blog.csdnimg.cn/20200831215701503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200831220018438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">代码实现上述思想

### f8 理解LCS

<img src="https://img-blog.csdnimg.cn/20200831220641192.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/2020083122124030.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### f9 动态规划LCS

<img src="https://img-blog.csdnimg.cn/20200831222315456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">参考：https://www.cnblogs.com/hapjin/p/5572483.html
