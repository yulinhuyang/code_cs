# [读书笔记]CSAPP：4[VB]浮点数据类型


 

**视频地址：**

[【精校中英字幕】2015 CMU 15-213 CSAPP 深入理解计算机系统 课程视频_哔哩哔哩 (゜-゜)つロ 干杯~-bilibiliwww.bilibili.com/video/av31289365?p=4![img](https://pic4.zhimg.com/v2-82eac1470b916682f49fd18b47cf7d23_180x120.jpg)](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/av31289365%3Fp%3D4)

**课件地址：**

[http://www.cs.cmu.edu/afs/cs/academic/class/15213-f15/www/lectures/04-float.pdfwww.cs.cmu.edu/afs/cs/academic/class/15213-f15/www/lectures/04-float.pdf](https://link.zhihu.com/?target=http%3A//www.cs.cmu.edu/afs/cs/academic/class/15213-f15/www/lectures/04-float.pdf)

对应于书本的2.4。

**如有错误请指出，谢谢。**

------

## 1 浮点数简介

浮点表示对形如 ![[公式]](https://www.zhihu.com/equation?tex=V%3Dx%5Ctimes+2%5Ey) 的有理数进行编码，比较适用于非常大的数字、非常接近0的数字。它常常不会太多关注运算的准确性，而是把实现的速度和简便性看得比数字精确性更重要。1985年提出了IEEE标准754，仔细制定了表示浮点数机器运算的标准，后续所有计算机都支持这个标准，极大提高了程序可移植性。

首先会介绍IEEE浮点表示方法，然后由于数字不能精确被表示，所以会介绍浮点数的舍入问题。最后介绍浮点数相关的运算。

## 2 IEEE浮点表示

IEEE浮点表示使用 ![[公式]](https://www.zhihu.com/equation?tex=V%3D%28-1%29%5Es%5Ctimes+M%5Ctimes+2%5EE) 表示数字。其中包含三部分：

- **符号（Sign）s：**用来确定V的正负性，当s=0时表示正数，s=1时表示负数。用一个单独的符号位直接进行编码。
- **尾数（Significand）M：** 是一个二进制小数，通常介于1和2之间的小数。使用k位二进制进行编码的小数。
- **阶码（Exponent）E：**对浮点数进行加权。使用n位进行编码的正数 ![[公式]](https://www.zhihu.com/equation?tex=e_%7Bk-1%7D%2Ce_%7Bk-2%7D%2C...%2Ce_0)。

C语言中有单精度精浮点数`float`，其中s=1、k=8、n=23；还有双精度浮点数`double`，其中s=1、k=11、n=52。

![img](https://pic4.zhimg.com/80/v2-db6d6ab3b68227d45df5e3995787409f_720w.jpg)

我们可以根据尾数和阶码的不同取值，将其分成三种情况：

1. **规格化的值：**当 ![[公式]](https://www.zhihu.com/equation?tex=exp%5Cin+%280%2C+2%5Ew-1%29) 时， ![[公式]](https://www.zhihu.com/equation?tex=E%3Dexp-Bias) ，其中 ![[公式]](https://www.zhihu.com/equation?tex=Bias%3D2%5E%7Bk-1%7D-1) ，由此能将E重新投影到正负值，并且能够和非规格化进行平滑； ![[公式]](https://www.zhihu.com/equation?tex=M%3D1%2Bfrac) ，因为我们可以通过调整E使得 ![[公式]](https://www.zhihu.com/equation?tex=1%5Cleq+M%3C2) ，所以通过这种形式将尾数变成 ![[公式]](https://www.zhihu.com/equation?tex=1.f_%7Bn-1%7D%2Cf_%7Bn-2%7D%2C...%2Cf_0) 的形式，就能获得额外的精度。g规格化数能够表示大范围的数。
2. **非规格化的值：**当 ![[公式]](https://www.zhihu.com/equation?tex=exp%3D0) 时， ![[公式]](https://www.zhihu.com/equation?tex=E%3D1-Bias) ，由此来保证和规格化值的连续性； ![[公式]](https://www.zhihu.com/equation?tex=M%3Dfrac%3D0.f_%7Bn-1%7D%2Cf_%7Bn-2%7D%2C...%2Cf_0) 。非规格化数能够表示正负0以及趋近于0的数。
3. **特殊值：**当阶码全为1时，如果尾数全为0，则表示无穷，比如两个很大的数相乘，或除以0时；否则表示NaN（Not a Number），比如求-1的根号。

![img](https://pic1.zhimg.com/80/v2-f5e130f5f454f7752e1339f6ca1c8d5c_720w.jpg)

![img](https://pic4.zhimg.com/80/v2-441d9146a1ec099e79a8fae7c08340ef_720w.jpg)

通过上面我们可以观察到几个**现象**：

1. 非规格数稠密地分布在靠近0的区域；
2. 有些数的间隔是等距的，因为当exp值不变，在`frac`尾数区域进行增加会乘上相同的指数；
3. 越大的数间隔越大，因为比较大的数，它的指数 ![[公式]](https://www.zhihu.com/equation?tex=2%5EE) 会比较大，使得每次变化量会比较大。

**根据二进制编码计算数值：**

1. 计算 ![[公式]](https://www.zhihu.com/equation?tex=Bias%3D2%5E%7Bk-1%7D-1)
2. 计算阶码的值exp和尾数的值frac
3. 如果为规格化值，则 ![[公式]](https://www.zhihu.com/equation?tex=E%3Dexp-Bias) ， ![[公式]](https://www.zhihu.com/equation?tex=M%3D1%2Bfrac) ；如果为非规格化值，则 ![[公式]](https://www.zhihu.com/equation?tex=E%3D1-Bias) ， ![[公式]](https://www.zhihu.com/equation?tex=M%3Dfrac) 。
4. 计算最终的值 ![[公式]](https://www.zhihu.com/equation?tex=V%3D%28-1%29%5Es%5Ctimes+M%5Ctimes+2%5EE)

我们以正数为例（s=0），说明几个比较**特殊的值：**

- **0：**只有非规格化才能表示0，exp和frac全部为0时，结果为0。
- **最小的正非规格化数：**![[公式]](https://www.zhihu.com/equation?tex=E%3D1-%282%5E%7Bk-1%7D-1%29%3D2-2%5E%7Bk-1%7D) 是固定值，在frac取值范围内的最小值正数是 ![[公式]](https://www.zhihu.com/equation?tex=%5B0%2C0%2C...%2C1%5D) ，则 ![[公式]](https://www.zhihu.com/equation?tex=M%3Dfrac%3D2%5E%7B-n%7D) ，所以 ![[公式]](https://www.zhihu.com/equation?tex=V%3DM%5Ctimes+2%5EE%3D2%5E%7B-n%7D%5Ctimes+2%5E%7B2-2%5E%7Bk-1%7D%7D%3Dx%5E%7B-n%2B2-2%5E%7Bk-1%7D%7D) 。
- **最大非规格化数：**E还是固定值 ![[公式]](https://www.zhihu.com/equation?tex=2-2%5E%7Bk-1%7D) ，在frac取值范围内的最大值是 ![[公式]](https://www.zhihu.com/equation?tex=%5B1%2C1%2C...%2C1%2C0%5D) ，则 ![[公式]](https://www.zhihu.com/equation?tex=M%3Dfrac%3D1-2%5E%7B-n%7D) ，所以 ![[公式]](https://www.zhihu.com/equation?tex=V%3D%281-2%5E%7B-n%7D%29%5Ctimes+2%5E%7B2-2%5E%7Bk-1%7D%7D) 。
- **最小规格化数：**exp的最小值为 ![[公式]](https://www.zhihu.com/equation?tex=%5B0%2C0%2C...%2C0%2C1%5D%3D1) ，所以 ![[公式]](https://www.zhihu.com/equation?tex=E%3Dexp-Bias%3D1-%282%5E%7Bk-1%7D-1%29%3D2-2%5E%7Bk-1%7D) ；frac全0时，M取得最小值1，所以 ![[公式]](https://www.zhihu.com/equation?tex=V%3D2%5E%7B2-2%5E%7Bk-1%7D%7D) 。
- **1：**要表示1，则需要用规格化来表示，当frac全为0时，M=1，需要让E=0，则 ![[公式]](https://www.zhihu.com/equation?tex=exp%3DBias%3D2%5E%7Bk-1%7D-1) ，即exp为 ![[公式]](https://www.zhihu.com/equation?tex=%5B0%2C1%2C1%2C...%2C1%5D) 。
- **最大规格化数：**exp的最大值为 ![[公式]](https://www.zhihu.com/equation?tex=%5B1%2C1%2C...%2C1%2C0%5D%3D2%5E%7Bk%7D-2) ，frac的最大值为全1，即 ![[公式]](https://www.zhihu.com/equation?tex=frac%3D1-2%5E%7B-n%7D) ，所以 ![[公式]](https://www.zhihu.com/equation?tex=E%3Dexp-Bias%3D2%5Ek-2-%282%5E%7Bk-1%7D-1%29%3D2%5E%7Bk-1%7D-1) ， ![[公式]](https://www.zhihu.com/equation?tex=M%3D1%2Bfrac%3D2-2%5E%7B-n%7D) ，所以 ![[公式]](https://www.zhihu.com/equation?tex=V%3D%282-2%5E%7B-n%7D%29%5Ctimes+2%5E%7B2%5E%7Bk-1%7D-1%7D%3D%281-2%5E%7B-n-1%7D%29%5Ctimes+2%5E%7Bk-1%7D) 。

![img](https://pic4.zhimg.com/80/v2-2aaefe6b4c1c160e67f41587214fbaff_720w.jpg)

**IEEE设计的好处：**

1. 最大非规格化值 ![[公式]](https://www.zhihu.com/equation?tex=%281-2%5E%7B-n%7D%29%5Ctimes+2%5E%7B2-2%5E%7Bk-1%7D%7D) 和最小规格化值 ![[公式]](https://www.zhihu.com/equation?tex=2%5E%7B2-2%5E%7Bk-1%7D%7D) 之间的幅度是 ![[公式]](https://www.zhihu.com/equation?tex=2%5E%7B-n%7D) ，是n位尾数所能表示的最小值，可以看成是**光滑转变**。
2. 从最小非规格化数到最大规格化数的位向量的变化是顺序的，和无符号整数的排序相同。所以可以用无符号数的排序函数来对浮点数进行排序。**注意：**负无穷转化为无符号数进行比较时会有问题。

**将十进制化为浮点数表示：**

以12345为例，我们推算单精度浮点数的编码

1. 计算 ![[公式]](https://www.zhihu.com/equation?tex=Bias%3D2%5E%7Bk-1%7D-1%3D2%5E%7B7%7D-1%3D127)
2. 将12345化为二进制数 ![[公式]](https://www.zhihu.com/equation?tex=%5B11000000111001%5D)
3. 首先将二进制数其化成小于1的科学技术法 ![[公式]](https://www.zhihu.com/equation?tex=0.11000000111001%5Ctimes+2%5E%7B14%7D) ，很明显指数14不为 ![[公式]](https://www.zhihu.com/equation?tex=1-Bias%3D-126) ，所以是规格化数，所以将其转化为大于1的科学计数法 ![[公式]](https://www.zhihu.com/equation?tex=1.1000000111001%5Ctimes+2%5E%7B13%7D) ，所以 ![[公式]](https://www.zhihu.com/equation?tex=M%3D1.1000000111001) ， ![[公式]](https://www.zhihu.com/equation?tex=E%3D14) 。
4. 因为 ![[公式]](https://www.zhihu.com/equation?tex=M%3D1%2Bfrac) ，所以frac的编码为 ![[公式]](https://www.zhihu.com/equation?tex=%5B1000000111001%5D) 。因为 ![[公式]](https://www.zhihu.com/equation?tex=E%3Dexp-Bias%3Dexp-127) ，所以 ![[公式]](https://www.zhihu.com/equation?tex=exp%3D127%2B13%3D140%3D%5B10001100%5D) 。
5. 将frac和exp的二进制编码扩展到对应位数并拼接在一起，补上符号位就为最终结果 ![[公式]](https://www.zhihu.com/equation?tex=010001100+10000001110010000000000) 。

**注意：**要在exp前面补0，在frac后面补0。因为exp表示整数，frac表示小数。

**对比无符号数的编码，我们可以发现：**因为无符号数一定大于0，所以相同的数想用浮点数编码只能使用规范化数进行编码，而规范化数会将frac的最高有效位1去掉，所以无符号数的编码和浮点数编码，在frac部分是相似的，浮点数会少了最高有效位的1。而无符号数的其他部分就是0，而浮点数的其他部分是表示指数的编码。

## 3 浮点数舍入

浮点数由于有限的位数，所以对于真实值x，我们想要用一种系统的方法来找到能够用浮点数表示的“最接近的x”匹配值x'，这个过程就称为**舍入**。

常见的舍入方法有四种：向零舍入、向上舍入、向下舍入以及向偶数舍入。以十进制为例可以看以下表格

![img](https://pic2.zhimg.com/80/v2-098cc0a1bd3c8a46fae91bfc9a110071_720w.jpg)

其中比较特殊的是**向偶数舍入：**如果处于中间值，就朝着令最后一个有效位为偶数来舍入；否则朝着最近的值舍入。比如1.40，由于靠近1就朝1舍入；1.6靠近2就朝2舍入；1.50位于十进制的中间值，就朝着偶数舍入，所以为2。

**向偶数舍入的意义：**如果对一系列值进行向上舍入，则舍入后的平均值会比真实值更大；使用向下舍入，则舍入后的平均值会比真实值更小。通过向偶数舍入，每个值就有50%概率变大、50%概率变小，使得总的统计量保持较为稳定。

十进制的中间值为 ![[公式]](https://www.zhihu.com/equation?tex=500..._%7B10%7D) ，比这个中间值大就向上舍入，比这个中间值小就向下舍入，否则朝着偶数舍入。

而二进制的中间值是 ![[公式]](https://www.zhihu.com/equation?tex=100..._%7B2%7D) ，比如 ![[公式]](https://www.zhihu.com/equation?tex=101..._%7B2%7D) 就比中间值大， ![[公式]](https://www.zhihu.com/equation?tex=010..._2) 就比中间值小。而且二进制中，当最后一个有效位为0时，为偶数。比如

![img](https://pic4.zhimg.com/80/v2-9cff312c7c95cacf5c6b5f73b35c793f_720w.jpg)

- `10.00011`：由于`011`比中间值小，所以直接向下舍入，为`10.00`。
- `10.00110`：由于`110`比中间值大，所以直接向上舍入，为`10.01`。
- `10.11100`：由于`100`为中间值，而`10.11`最后一个有效位1位奇数，所以向上舍入为偶数`11.00`。
- `10.10100`：由于`100`为中间值，而`10.10`最后一个有效位0位偶数，所以直接向下舍入`10.10`。

## 4 浮点数运算

浮点数运算无法直接通过在位向量上运算得到。

### 4.1 浮点数乘法

对于两个浮点数 ![[公式]](https://www.zhihu.com/equation?tex=%28-1%29%5E%7Bs_1%7D%5Ctimes+M_1%5Ctimes+2%5E%7BE_1%7D) 和 ![[公式]](https://www.zhihu.com/equation?tex=%28-1%29%5E%7Bs_2%7D%5Ctimes+M_2%5Ctimes+2%5E%7BE_2%7D) ，计算结果为 ![[公式]](https://www.zhihu.com/equation?tex=%28-1%29%5Es%5Ctimes+M%5Ctimes+2%5EE) ，其中 ![[公式]](https://www.zhihu.com/equation?tex=s%3Ds_1XORs_2) ， ![[公式]](https://www.zhihu.com/equation?tex=M%3DM_1%5Ctimes+M_2) ， ![[公式]](https://www.zhihu.com/equation?tex=E%3DE_1%2BE_2) 。

- 如果 ![[公式]](https://www.zhihu.com/equation?tex=M%5Cgeq2) ，就将frac右移一位，并对E加一。
- 如果E超过了表示范围，就发生了溢出。
- 如果M超过了表示范围，对frac进行舍入。

**数学性质：**

- 可交换
- 不可结合：可能出现溢出和不精确的舍入，比如 ![[公式]](https://www.zhihu.com/equation?tex=1e20%2A%281e20%2A1e-20%29%3D1e20) ，而 ![[公式]](https://www.zhihu.com/equation?tex=%281e20%2A1e20%29%2A1e-20%3DINF) 。
- 不可分配：如果分配了可能会出现NaN，比如 ![[公式]](https://www.zhihu.com/equation?tex=1e20%2A%281e20-1e20%29%3D0) ，而 ![[公式]](https://www.zhihu.com/equation?tex=1e20%2A1e20-1e20%2A1e20%3DNaN) 。
- 保证，只要 ![[公式]](https://www.zhihu.com/equation?tex=a%5Cne+NaN) ，则 ![[公式]](https://www.zhihu.com/equation?tex=a%2A%5Et+a%5Cgeq0) 。

### 4.2 浮点数加法

对于两个浮点数 ![[公式]](https://www.zhihu.com/equation?tex=%28-1%29%5E%7Bs_1%7D%5Ctimes+M_1%5Ctimes+2%5E%7BE_1%7D) 和 ![[公式]](https://www.zhihu.com/equation?tex=%28-1%29%5E%7Bs_2%7D%5Ctimes+M_2%5Ctimes+2%5E%7BE_2%7D) ，计算结果为 ![[公式]](https://www.zhihu.com/equation?tex=%28-1%29%5Es%5Ctimes+M%5Ctimes+2%5EE) ，其中s和M是对其后的运算结果， ![[公式]](https://www.zhihu.com/equation?tex=E%3Dmax%28E_1%2CE_2%29) 。

![img](https://pic4.zhimg.com/80/v2-307270876e115b9d068e3febb002bb17_720w.jpg)

- 如果 ![[公式]](https://www.zhihu.com/equation?tex=M%5Cgeq2) ，则frac右移一位，并对E加1。
- 如果 ![[公式]](https://www.zhihu.com/equation?tex=M%3C1) ，则frac左移一位，并对E减1。
- 如果E超过表示范围，就发生溢出。
- 如果M超过表示范围，就对frac进行舍入。

**数学性质：**

- 由于溢出，可能得到无穷之。
- 可交换
- 不可结合（由于舍入），因为较大的数和较小的数相加，由于舍入问题，会将较小的数舍入，比如 ![[公式]](https://www.zhihu.com/equation?tex=%283.14%2B1e10%29-1e10%3D0) 而 ![[公式]](https://www.zhihu.com/equation?tex=3.14%2B%281e10-1e10%29%3D3.14) 。
- 除了无穷和NaN，存在加法逆元。
- 满足单调性，如果 ![[公式]](https://www.zhihu.com/equation?tex=a%5Cgeq+b) ，则对于任意a、b和x，都有 ![[公式]](https://www.zhihu.com/equation?tex=x%2Ba%5Cgeq+x%2Bb) 。NaN除外。无符号数和补码由于溢出会发生值的跳变，所以不满足单调性。



**注意：**需要考虑好清楚数值的范围，如果计算的数值范围变化很大，需要重新结合或改变运算顺序，避免由于溢出或舍入出现计算问题。

## 5 C中的浮点数

C中提供了`float`和`double`两种精度的浮点数。由于编码不同，所以在浮点数和整型数之间强制类型转换时，会修改编码，并且会出现溢出和舍入。

- **float/double转换成int：**首先小数部分会被截断，也就是向0舍入。`float`的尾数部分为23字节，比int的32字节小，所以int可以精确表示float的整数部分，而`double`的尾数有52位，可能会出现舍入。并且当超过int的取值范围或NaN时，微处理器会指定 ![[公式]](https://www.zhihu.com/equation?tex=%5B100...0%5D) 为整数不确定值，即对应的 ![[公式]](https://www.zhihu.com/equation?tex=TMin_w) ，所以一个很大的浮点数转化为int时，可能会出现负数。
- **int或float转换为double：**double尾数有52位，而int只有32位，float只有23位，所以double会精确表示int和float，不会出现溢出和输入。
- **int转换为float：**不会发生溢出，但是由于float尾数位数比较少，会出现舍入。
- **double转换为float：**可能会出现溢出和舍入。

**总结：**超过数值表示范围，会发生溢出；尾数较短，会发生输入。

------

**课堂作业：**

![img](https://pic3.zhimg.com/80/v2-1946aeb02ebc4a9b00ada80d7d207bae_720w.png)

- `x==(int)(float)x`：int有32位，float尾数有23位，从int强制类型转换到float会出现舍入，所以错误。
- `x==(int)(double)x`：int有32位，double尾数有52位，所以从int强制类型转换到float不会出现舍入，所以正确。
- `f==(float)(double)f`：double的精度和范围都比float大，所以能够无损地从float强制类型转换到double，所以正确。
- `d==(double)(float)d`：因为float的精度和范围都比double小，可能会出现溢出和输入，所以错误。
- `f==-(-f)`：因为只要改变一个符号位，所以正确。
- `2/3==2/3.0`： 因为`2/3`是int类型，会舍入变成0，而`2/3.0`是double类型，会得到数值，所以错误。
- `d<0.0`推出`((d*2)<0.0)`：乘2相当于exp加一，如果出现溢出，也是无穷小，所以正确。
- `d>f`推出`-f>-d`： 只要改变一个符号位，所以正确。
- `d*d>=0.0`： 正确。
- `(d+f)-d==f`：不符合结合律，可能会出现舍入和溢出。

------

接下来会完成课后练习题，然后完成Lab 。