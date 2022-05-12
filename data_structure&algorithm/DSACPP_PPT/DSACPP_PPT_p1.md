# DSACPP学习笔记-第一章


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



# DSACPP学习笔记-第二章


### 文章目录

第二章 向量
	a 接口与实现
		a1 接口与实现
		a2 向量ADT
		a3 操作实例
		a4 构造与析构
		a5 复制
	b 可扩充向量
		b1 可扩充向量
		b2 动态空间管理
		b3 递增式扩容
		b4 加倍式扩容
		b5 分摊复杂度
	c 无序向量
		c1 无序向量
		c2 循秩访问
		c3 插入
		c4 区间删除
		c5 查找
		c6 单元素删除
		c7 唯一化
		c8 遍历
	d 有序向量
	
		d1-1 有序性
		d1-2 唯一化（低效版）
		d1-3 复杂度（低效版）
		d1-4 唯一化（高效版）
		d1-5 实例与分析（高效版）
		d2 有序向量：二分查找
			d2-2 接口
			d2-3 语义
			d2-4 原理
			d2-5 实现
			d2-6 实例
			d2-7 查找长度
		d3-1 fib查找构思
		d3-2 fib查找实例长度
		d3-3 fib查找实现
		d3-4 fib查找最优
		
		d4-1 二分查找改进构思
		d4-2 二分改版本B
		d4-3 二分改语义
		d4-4 二分改版本C
		d4-5 二分改正确性
	e冒泡排序
		e1 冒泡排序构思
		e2 改进
		e3 反例
		e4 再改进
		e5 综合评价
	f归并排序
		f1 归并排序思想
		f2 主算法
		f3 二路归并-实例
		f4 二路归并-实现
		f5 二路归并-正确性
		f6 二路归并-复杂度


# 第二章 向量

day12

## a 接口与实现

### a1 接口与实现

<img src="https://img-blog.csdnimg.cn/20200902164553596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200902165104817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a2 向量ADT

<img src="https://img-blog.csdnimg.cn/20200902165451607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200902165511512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a3 操作实例

<img src="https://img-blog.csdnimg.cn/20200902170148873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">search接口，如果未找到对应的数，返回向量中不超过这个数的最大的数的index,如果都超过了这个数，返回-1，如果这个数重复多次，返回一系列index中最大的。

### a4 构造与析构

<img src="https://img-blog.csdnimg.cn/20200902171134780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/2020090217134981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a5 复制

<img src="https://img-blog.csdnimg.cn/20200902171720869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## b 可扩充向量

### b1 可扩充向量

<img src="https://img-blog.csdnimg.cn/20200902172226798.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b2 动态空间管理

<img src="https://img-blog.csdnimg.cn/20200902172459714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200902173026178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b3 递增式扩容

<img src="https://img-blog.csdnimg.cn/20200902173457843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b4 加倍式扩容

<img src="https://img-blog.csdnimg.cn/2020090217364374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/2020090217390634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">空间换取时间

day13

### b5 分摊复杂度

<img src="https://img-blog.csdnimg.cn/20200903103743769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## c 无序向量

### c1 无序向量

<img src="https://img-blog.csdnimg.cn/20200903104217932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c2 循秩访问

<img src="https://img-blog.csdnimg.cn/20200903104555416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c3 插入

<img src="https://img-blog.csdnimg.cn/20200903104821232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c4 区间删除

<img src="https://img-blog.csdnimg.cn/20200903105150561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c5 查找

<img src="https://img-blog.csdnimg.cn/2020090310581973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c6 单元素删除

<img src="https://img-blog.csdnimg.cn/20200903110221814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c7 唯一化

<img src="https://img-blog.csdnimg.cn/20200903110629177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200903111119495.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200903111625307.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c8 遍历

<img src="https://img-blog.csdnimg.cn/20200903112003560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200903112054169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## d 有序向量

### d1-1 有序性

<img src="https://img-blog.csdnimg.cn/20200903114757504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d1-2 唯一化（低效版）

<img src="https://img-blog.csdnimg.cn/20200903115116814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d1-3 复杂度（低效版）

<img src="https://img-blog.csdnimg.cn/20200903115428604.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d1-4 唯一化（高效版）

<img src="https://img-blog.csdnimg.cn/20200903150258160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d1-5 实例与分析（高效版）

<img src="https://img-blog.csdnimg.cn/20200903152345239.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2 有序向量：二分查找

<img src="https://img-blog.csdnimg.cn/20200903152530789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2-2 接口

<img src="https://img-blog.csdnimg.cn/20200903152758414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2-3 语义

<img src="https://img-blog.csdnimg.cn/20200903153444530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2-4 原理

<img src="https://img-blog.csdnimg.cn/20200903153720470.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2-5 实现

<img src="https://img-blog.csdnimg.cn/20200903154031544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2-6 实例

<img src="https://img-blog.csdnimg.cn/20200903154659878.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2-7 查找长度

<img src="https://img-blog.csdnimg.cn/2020090315541051.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d3-1 fib查找构思

<img src="https://img-blog.csdnimg.cn/20200903155929138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d3-2 fib查找实例长度

<img src="https://img-blog.csdnimg.cn/20200903160346284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d3-3 fib查找实现

<img src="https://img-blog.csdnimg.cn/20200903160710538.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d3-4 fib查找最优

<img src="https://img-blog.csdnimg.cn/20200903171052275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d4-1 二分查找改进构思

<img src="https://img-blog.csdnimg.cn/20200903171516186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d4-2 二分改版本B

<img src="https://img-blog.csdnimg.cn/20200903171958577.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d4-3 二分改语义

<img src="https://img-blog.csdnimg.cn/20200903172344697.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d4-4 二分改版本C

<img src="https://img-blog.csdnimg.cn/20200903172628696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d4-5 二分改正确性

<img src="https://img-blog.csdnimg.cn/20200903173508857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">不断的压缩空间直到 lo==hi，区间长度为0时，则lo前面一项就是我们要找的

## e冒泡排序

### e1 冒泡排序构思

<img src="https://img-blog.csdnimg.cn/2020090317413922.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200903174747156.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e2 改进

<img src="https://img-blog.csdnimg.cn/20200903175140513.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e3 反例

<img src="https://img-blog.csdnimg.cn/20200903175611544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">图中红色部分代表有序元素，绿色代表无序元素，如果能提前检测出红色部分是有序的，可以将时间复杂度降低到O（n).

### e4 再改进

<img src="https://img-blog.csdnimg.cn/20200903180047550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200903180445558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200903180539918.png#pic_center" alt="在这里插入图片描述"> 相应的复杂度的变化如上图

### e5 综合评价

<img src="https://img-blog.csdnimg.cn/20200903181105109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## f归并排序

### f1 归并排序思想

<img src="https://img-blog.csdnimg.cn/20200903181351227.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
C.B.A:基于比较的排序算法 
<img src="https://img-blog.csdnimg.cn/20200903181800365.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### f2 主算法

<img src="https://img-blog.csdnimg.cn/20200903181942947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### f3 二路归并-实例

<img src="https://img-blog.csdnimg.cn/20200903182325870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### f4 二路归并-实现

<img src="https://img-blog.csdnimg.cn/20200903183103971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### f5 二路归并-正确性

<img src="https://img-blog.csdnimg.cn/20200903183305348.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200903183454723.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">各种归并情况，不会出现数据遗漏或被覆盖

### f6 二路归并-复杂度

<img src="https://img-blog.csdnimg.cn/20200903184038332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">开始看第三章


# DSACPP学习笔记-第三章


### 文章目录
第三章 列表
	a
		a1 从静态到动态
		a2 从向量到列表
		a3 从秩到位置
		a4 实现
	b
		b1循秩访问
		b2 查找
		b3 插入和复制
		b4 删除与析构
		b5 唯一化
	c 有序列表
		c1 有序列表唯一化构思
		c2 唯一化实现
		c3 查找
	d 选择排序
		d1 选择排序
		d2 实例
		d3 实现
		d4 推敲
		d5 selectMax
		d6 性能
	e 插入排序
		e1 插入排序
		e2 构思
		e3 对比
		e4 实例
		e5 实现
		e6 性能分析
		e7 平均性能
		e8 逆序对


# 第三章 列表


## a

### a1 从静态到动态

<img src="https://img-blog.csdnimg.cn/202009042031470.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a2 从向量到列表

<img src="https://img-blog.csdnimg.cn/20200904203356223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a3 从秩到位置

<img src="https://img-blog.csdnimg.cn/20200904203518253.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20200904203657880.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### a4 实现

<img src="https://img-blog.csdnimg.cn/20200904203813690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
<img src="https://img-blog.csdnimg.cn/20200904203902260.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200904203922172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200904204248399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">其中header和trailer对外部不可见 <img src="https://img-blog.csdnimg.cn/20200904204349733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## b

### b1循秩访问

<img src="https://img-blog.csdnimg.cn/20200904204631926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b2 查找

<img src="https://img-blog.csdnimg.cn/20200904205036576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">上图中①和②参数位置的不同，代表一个是向前查找，而另一个是向后查找

### b3 插入和复制

<img src="https://img-blog.csdnimg.cn/20200904205651278.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200904205822621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">上图从已有的列表复制来创建新的列表

### b4 删除与析构

<img src="https://img-blog.csdnimg.cn/20200904210114106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200904210226860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b5 唯一化

<img src="https://img-blog.csdnimg.cn/20200904210622684.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## c 有序列表

### c1 有序列表唯一化构思

<img src="https://img-blog.csdnimg.cn/20200904211010211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c2 唯一化实现

<img src="https://img-blog.csdnimg.cn/20200904211246492.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c3 查找

<img src="https://img-blog.csdnimg.cn/20200904211647443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## d 选择排序

### d1 选择排序

<img src="https://img-blog.csdnimg.cn/20200904213305130.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

冒泡排序思路上与选择排序类似，都是每次选择出最大的。但是冒泡排序效率更低，因为是一步一步向后交换，而选择排序是选出最大的，直接一步将其转移到后端。

### d2 实例

<img src="https://img-blog.csdnimg.cn/20200904213633312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d3 实现

<img src="https://img-blog.csdnimg.cn/20200904213919311.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d4 推敲

上图中直接调用inserBefore和remove接口，其中涉及到new和delete操作，其复杂度是普通操作的100倍左右，可以考虑直接交换数据域；修改前驱和后继的指向等来完成。

### d5 selectMax

<img src="https://img-blog.csdnimg.cn/20200904214927689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d6 性能

<img src="https://img-blog.csdnimg.cn/2020090421514275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

## e 插入排序

### e1 插入排序

<img src="https://img-blog.csdnimg.cn/20200904215419307.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

扑克牌码牌的场景

### e2 构思

<img src="https://img-blog.csdnimg.cn/20200904215649902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e3 对比

<img src="https://img-blog.csdnimg.cn/20200904215851165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

选择排序 vs 插入排序

### e4 实例

<img src="https://img-blog.csdnimg.cn/20200904220031373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">
减而治之

### e5 实现

<img src="https://img-blog.csdnimg.cn/20200904220439877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e6 性能分析

<img src="https://img-blog.csdnimg.cn/20200904221125279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200904221251664.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

最好：O（n）,已经有序 最坏：O（n^2）

### e7 平均性能

<img src="https://img-blog.csdnimg.cn/20200904221737211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### e8 逆序对

<img src="https://img-blog.csdnimg.cn/20200904222415587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">其中I代表有序对的数目。 开始第四章


# DSACPP学习笔记-第四章


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


# DSACPP学习笔记-第五章


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


# DSACPP学习笔记-第六章


### 文章目录

	图
	a 图
		a1 概述：邻接，相关
		a2 有向/无向图
		a3 路径 环路
	b
		b1-1 邻接矩阵-接口
		b1-2 邻接矩阵与关联矩阵
		b1-3 实例
		b1-4 顶点和边
		b1-5 邻接矩阵
		b1-6 顶点静态操作
		b1-7 边操作
		b1-8 顶点动态操作
		b1-9 综合评价
	c搜索
		c1 BFS化繁为简
		c2 策略
		c3 实现
		c4 可能情况
		c5 实例
		c6 多连通
		c7 复杂度
		c8 最短路径
	d 深度优先搜索
		d1 DFS算法
		d2 DFS框架
		d3 细节
		d4 无向图
		d5 有向图
		d6 多可达域
		d7 嵌套引理
 
# 图

## a 图

### a1 概述：邻接，相关

<img src="https://img-blog.csdnimg.cn/20200919114035608.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

之前学习到vector，tree都是图的特例；

### a2 有向/无向图

<img src="https://img-blog.csdnimg.cn/20200919114513508.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

可以通过有向图表示无向图或混合图，技巧是将无向边转化为彼此对称的一对有向边。

### a3 路径 环路

<img src="https://img-blog.csdnimg.cn/20200919115101370.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

所有的环路中比较有意思的两种：如图（i）经过所有的边一次且恰好一次，（ii）经过每个顶点一次且恰好一次 有向无环图（DAG）不包含有向环的有向图就是有向无环图

## b

### b1-1 邻接矩阵-接口

<img src="https://img-blog.csdnimg.cn/2020091912091354.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b1-2 邻接矩阵与关联矩阵

<img src="https://img-blog.csdnimg.cn/20200919121446918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

关联矩阵行为顶点，列为边，每一列只有两个位置是1，其余位置都是0

### b1-3 实例

<img src="https://img-blog.csdnimg.cn/20200919121801360.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b1-4 顶点和边

<img src="https://img-blog.csdnimg.cn/20200919144723693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

<img src="https://img-blog.csdnimg.cn/20200919144803605.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b1-5 邻接矩阵

<img src="https://img-blog.csdnimg.cn/20200919145036998.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

顶点集，边集（即邻接矩阵）

### b1-6 顶点静态操作

<img src="https://img-blog.csdnimg.cn/20200919145151332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/2020091914551929.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b1-7 边操作

<img src="https://img-blog.csdnimg.cn/20200919152722982.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200919152739610.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200919152859253.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200919152953896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### b1-8 顶点动态操作

<img src="https://img-blog.csdnimg.cn/20200919153405675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200919153525212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

上述代码有误，需要在第4、8行加上代码e–(边数减一)

### b1-9 综合评价

对邻接矩阵表示法的综合评价 <img src="https://img-blog.csdnimg.cn/20200919153919510.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200919154203482.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

具体证明方法参考教材练习中的6-3题

## c搜索

### c1 BFS化繁为简

<img src="https://img-blog.csdnimg.cn/20200919154524911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c2 策略

<img src="https://img-blog.csdnimg.cn/20200919155220986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

树的层次遍历是图的广度优先搜索的特例

### c3 实现

<img src="https://img-blog.csdnimg.cn/20200919155643928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">分别处理见下一节

### c4 可能情况

<img src="https://img-blog.csdnimg.cn/20200919155958186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### c5 实例

<img src="https://img-blog.csdnimg.cn/20200919164742414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

最后去掉cross edge，可以得到一棵树（右下图）

### c6 多连通

<img src="https://img-blog.csdnimg.cn/20200919165014766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

如何实现对多个连通域的统一遍历，每一个连通域启动且只启动一次广度优先搜索

### c7 复杂度

<img src="https://img-blog.csdnimg.cn/20200919170140141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">对前面BFS代码的复杂度分析

### c8 最短路径

<img src="https://img-blog.csdnimg.cn/20200919170458656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/20200919170545518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

对于无权图的BFS遍历，如上图右，每个节点与S节点的那一条通路，恰好对应于原图中这两个节点间的最短通路。证明见习题6-7

## d 深度优先搜索

### d1 DFS算法

<img src="https://img-blog.csdnimg.cn/20200919171040236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d2 DFS框架

<img src="https://img-blog.csdnimg.cn/20200919171303797.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d3 细节

<img src="https://img-blog.csdnimg.cn/20200919171504588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

有向图的深度优先搜索树中有四类边。设(v,u)是图中一条边。 在explore(v)中如果u是第一次被访问，则(v,u)是树边(Tree edge); 若v是u在搜索树中的祖先，且在explore(v)前u已经被访问过，则(v,u)是前向边(Forward edge); 若v是u在搜索树中的后裔，且在explore(v)前u已经被发现，则(v,u)是回边(Back edge); 若v和u没有祖先-后裔（后裔-祖先）关系，且在explore(v)前u已经被访问过，则(v,u)是横跨边(Cross edge).

### d4 无向图

见视频

### d5 有向图

<img src="https://img-blog.csdnimg.cn/20200919172531878.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述"><img src="https://img-blog.csdnimg.cn/2020091917255056.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">详见视频

### d6 多可达域

<img src="https://img-blog.csdnimg.cn/202009191732572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">

### d7 嵌套引理

<img src="https://img-blog.csdnimg.cn/20200919173836592.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9kaWRhZGFkYQ==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述">完成代码，开始第七章
