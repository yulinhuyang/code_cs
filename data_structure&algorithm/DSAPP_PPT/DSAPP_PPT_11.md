#邓俊辉数据结构与算法学习笔记-第十一章


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
