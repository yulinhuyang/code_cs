#### 6.1.3　固态硬盘

**固态硬盘（Solid State Disk，SSD）** 是一种基于闪存的存储技术，插在I/O总线上标准硬盘插槽（通常为USB或SATA），处于磁盘和DRAM存储器的中间点。从CPU的角度来看，SSD与磁盘完全相同，有相同的接口和包装。

![img](pics/v2-917429960df29b0a0591dcf2ce7b9599_720w.jpg)

如上图所示是一个SSD的基本结构。它由**闪存**和** 闪存翻译层（Flash Translation Layer）**组成

- 闪存翻译层是一个硬件/固件设备，用来将对逻辑块的请求翻译成对底层物理设备的访问。
- 闪存的基本属性决定了SSD随机读写的性能，通常由B个块的序列组成，每个块由P页组成，页作为数据的单位进行读写。通常页大小为512字节~4KB，块中包含32~128页，则块的大小有16KB~512KB。


### 6.2　局部性

具有良好**局部性（Locality）** 的程序，会倾向于引用最近引用过的数据项本身，或者引用最近引用过的数据项周围的数据项。局部性主要具有两种形式：

- **时间局部性（Temporal Locality）：** 引用过的数据项在不久会被多次引用。

![img](pics/v2-92e18eee9fec87d25e674d4eee44afcd_720w.png)

- **空间局部性（Spatial Locality）：** 引用过的数据项，在不久会引用附近的数据项。

![img](pics/v2-82179b9f2bb653a4e86550e56526ec74_720w.png)

从硬件到操作系统，再到应用程序，都利用了局部性

- **硬件：** 在处理器和主存之间引入一个小而快速的高速缓存存储器，来保存最近引用的指令和数据，从而提高对主存的访问速度。
- **操作系统：** 用主存来缓存虚拟空间中最近被引用的数据块。
- **应用程序：** 比如Web浏览器会将最近引用的文档放入本地磁盘中，来缓存服务器的数据。

有良好局部性的程序比局部性较差的程序运行更快。

想要分析一个程序的局部性是否好，可以依次分析程序中的每个变量，然后根据所有变量的时间局部性和空间局部性来总和判断程序的局部性。

**例1：**

![img](pics/v2-b31a0fe3850f843ea7bc207e764a766d_720w.jpg)

分析上述程序的局部性。对于变量`sum`，每一轮迭代都会引用一次，所以`sum`具有好的时间局部性，而`sum`是标量，所以没有空间局部性。对于变量`v`，其数据在内存中的分布如图b中所示，每一轮迭代都是引用不同的数据项，所以时间局部性较差，但是会按照内存存储的顺序依次引用数据项，所以空间局部性较好。 综合来说，该程序具有较好的局部性。

并且由于程序是以指令形式保存在内存中的，而CPU会从内存中读取指令，所以也可以考虑取指的局部性。由于该循环体内的指令是顺序保存在内存中的，而CPU会按顺序进行取指，所以具有良好的空间局部性，并且迭代多次会反复读取相同的指令，所以具有良好的时间局部性，所以该程序的局部性较好。

对于一个向量，如果每一轮引用的数据项之间在内存空间中相隔k，则称该程序具有**步长为k的引用模式（Stride-k Reference Pattern）**。步长k越大，则每一轮引用的数据在内存中间隔很大，则空间局部性越差。

**例2：**

![img](pics/v2-48a83a08edb84d9a8ab728150dd0aaac_720w.jpg)

对于以上代码，变量`sum`的时间局部性较好且不具有空间局部性，对于二维数组变量`v`，在内存中是按照行优先存储的，而代码中也是按照行优顺序进行应用的，所以变量`v`具有步长为1的引用模式，所以具有较好的空间局部性，而时间局部性较差。总体来说，该程序具有良好的局部性。

**例3：**

![img](pics/v2-561d453a9fd069a3386d106d2c08952f_720w.jpg)

上述代码将变量`v`的引用顺序变为了列优先，则根据`v`的内存存储形式，变量`v`具有步长为N的引用模式，则时间局部性较差，且空间局部性也较差。总体来说，该程序的局部性较差。

**例4：**

![img](pics/v2-b48d6f916ab0fc996be0a489eee1841e_720w.jpg)

我们需要判断以上三个函数的局部性。首先根据结构体的定义可以得到结构体数组在内存中的存储形式如下所示

![img](pics/v2-91128cb42e57ffa12c5fa6c10809f46b_720w.png)

则`clear1`函数的步长为1，具有良好的空间局部性；而`clear2`函数会在结构体中不同的字段中反复跳跃，空间局部性相对`clear1`差一些；而`clear3`函数会在相邻两个结构体中反复跳跃，空间局部性相比`clear2`更差。

**总体而言：**

- 重复引用相同变量的程序具有良好的时间局部性
- 考虑变量的内存存储形式，判断程序引用模式的步长，步长越大则空间局部性越差
- 从取指角度而言，具有循环体则空间局部性和时间局部性较好，而且循环体越小、迭代次数越多，则局部性越好。


### 6.3　存储器层次结构

#### 6.3.1　存储器层次结构中的缓存

- 从寄存器到外接的机械硬盘甚至远程磁盘，每一级都可以看作是下一级的缓存。
- 相邻层之间块大小是一致的，离cpu越远的层就用越大的块来做缓存。
- 缓存只是下一层的一个子集副本，取数据时会出现缓存命中和不命中两种情况。
- cache都是基于SRAM的。
- 从cache中取数据时，高位使用tag而不是set来索引，这样做的好处是使得相邻的内存块映射到不同的set中，如果取一个连续的数组时，就不需要一直进行cache line的替换。

![img](pics/v2-a5efaee2ec60e7c80143ac8e1992bf46_720w.jpg)

**高速缓存（Cache）** 是一个小而快速的存储设备，用来作为存储在更大更慢设备中的数据对象的缓冲区域。而使用高速缓存的过程称为**缓存（Caching）**。

存储器层次结构的**中心思想**是让层次结构中的每一层来缓存低一层的数据对象，将第k层的更快更小的存储设备作为第k+1层的更大更慢的存储设备的缓存。

**该结构之所以有效**，是因为程序的局部性原理。相比于第k+1层的数据，程序会倾向于访问存储在第k层的数据。如果我们访问第k+1层存储的数据，我们会将其拷贝到第k层，因为根据局部性原理我们很有可能将再次访问该数据，由此我们就能以第k层的访问速度来访问数据。而且因为我们不经常访问第k+1层的数据，我们就可以使用速度更慢且更便宜的存储设备。

![img](pics/v2-bed760846b2d38575f36c7b36e483032_720w.jpg)

上图展示的是存储器层次结构的基本缓存原理。每一层存储器都会被划分成连续的数据对象组块，称为**块（Block）**，每个块都有一个唯一的地址或名字，并且通常块的大小都是固定的。第k层作为第k+1层的缓存，数据会以块大小作为**传送单元（Transfer Unit）** 在第k层和第k+1层之间来回赋值，使得第k层保存第k+1层块的一个子集的副本。通常存储器层次结构中较低层的设备的访问时间较长，所以较低层中会使用较大的块。

##### 缓存命中

当程序需要第k+1层的某个数据对象d时，会现在第k层的块中搜索d，如果d刚好缓存在第k层中，则成为**缓存命中（Cache Hit）**，则该程序会直接从第k层中读取d。根据存储器层次结构，可以知道第k层的读取速度更快，因此缓存命中会使得程序更快。

##### 缓存不命中

如果第k层没有缓存数据对象d，则称为**缓存不命中（Cache Miss）**，则会从第k+1层中取出包含d的块，然后第k层的缓存会执行某个**放置策略（Placement Policy）**来决定该块要保存在第k层的什么位置

- 来自第k+1层的任意块能保存在第k层的任意块中，如果第k层的缓存满了，则会覆盖现存的一个**牺牲块（Victim Block）**，称为**替换（Replacing）**或**驱逐（Evicting）** 这个牺牲块，会根据**替换策略（Replacement Policy）** 来决定要替换第k层的哪个块
  - **随机替换策略：** 会随机选择一个牺牲块
  - **最近最少被使用（LRU）替换策略：** 选择最后被访问的时间离现在最远的块

### 6.4　高速缓存存储器

- 当高速缓存大小大于数据的大小，如果分配良好，则只会出现冷不命中。  
- 缓存不命中比内存访问次数影响更大   
- 由内存系统的设计来决定块大小，是内存系统的固定参数。首先决定块大小，然后决定期望的缓存大小，然后再决定关联性，最终就能知道组的数目。   
- 块的目的就是利用空间局部性  
- 缓存是硬件自动执行的，没有提供指令集对其进行操作

- **建议：**  
- - 将注意力集中在内循环中，因为大部分的计算和内存访问都集中在这里
  - 按照数据对象存储在内存中的顺序，以步长为1来读数据，使得空间局部性最大。比如步长为2的命中率就比步长为1的命中率降低一半。
  - 一旦从存储器读入一个数据对象时，就尽可能使用它，使得时间局部性最大。特别是局部变量，编译器会将其保存在寄存器中。

#### 6.4.1　通用的高速缓存存储器组织结构

较早期的计算机系统的存储器层次结构只有三层：CPU寄存器、主存和磁盘，但是随着CPU的发展，使得主存和CPU之间的读取速度逐渐拉大，由此在CPU和主存之间插入一个小而快速的SRAM高速缓存存储器，称为**L1高速缓存**，随着后续的发展，又增加了**L2高速缓存**和**L3高速缓存**。

![img](pics/v2-3a293eaec3e353719cc3fd99b4d6a58a_720w.png)

通用的高速缓存存储器组织结构：

![img](pics/v2-f4f20030ddac5693b9ffe76f570df080_720w.jpg)

- **s位：** 高速缓存被组织成一个数组，而该数组通过 ![[公式]](https://www.zhihu.com/equation?tex=S%3D2%5Es+)进行索引。
- **b位：** 每个组中包含E个**高速缓存行（Cache Line）**，每个行有一个 ![[公式]](https://www.zhihu.com/equation?tex=B%3D2%5Eb) 字节的**数据块（Block）**组成。
- **t位：** 每一个高速缓存行有一个 ![[公式]](https://www.zhihu.com/equation?tex=t%3Dm-%28s%2Bb%29) 位的**标记位（Valid Bit）**，唯一表示存储在这个高速缓存行中的数据块，用于搜索数据块。

该高速缓存的结构可以通过元组`(S, E, B, m)`来描述，且容量C为所有块的大小之和， ![[公式]](https://www.zhihu.com/equation?tex=C%3DS%5Ctimes+E%5Ctimes+B) 。

当一条加载指令指示CPU从主存地址A中读取一个字w时，会将该主存地址A发送到高速缓存中，则高速缓存会根据以下步骤判断地址A是否命中：

1. **组选择：** 根据地址划分，将中间的s位表示为无符号数作为组的索引，可得到该地址对应的组。
2. **行匹配：** 根据地址划分，可得到t位的标志位，由于组内的任意一行都可以包含任意映射到该组的数据块，所以就要线性搜索组中的每一行，判断是否有和标志位匹配且设置了有效位的行，如果存在，则缓存命中，否则缓冲不命中。
3. **字抽取：** 如果找到了对应的高速缓存行，则可以将b位表示为无符号数作为块偏移量，得到对应位置的字。

当高速缓存命中时，会很快抽取出字w，并将其返回给CPU。如果缓存不命中，CPU会进行等待，高速缓存会向主存请求包含字w的数据块，当请求的块从主存到达时，高速缓存会将这个块保存到它的一个高速缓存行中，然后从被存储的块中抽取出字w，将其返回给CPU。

#### 6.4.2　直接映射高速缓存

![img](pics/v2-aa702758461fd483d0f4c4693a9cb913_720w.jpg)

如上图所示，当 ![[公式]](https://www.zhihu.com/equation?tex=E%3D1) 时，高速缓存称为**直接映射高速缓存（Direct-mapped Cache）**，每个高速缓存组中只含有一个高速缓存行。

#### 6.4.3　组相联高速缓存

直接映射高速缓存的冲突不命中是由于每个高速缓存组中只有一个高速缓存行，所以扩大E的值，当 ![[公式]](https://www.zhihu.com/equation?tex=1%3CE%3CC%2FB) 时，称为**E路组相联高速缓存（Set Associative Cache）**，此时需要额外的硬件逻辑来进行行匹配，所以更加昂贵。（ ![[公式]](https://www.zhihu.com/equation?tex=E%3CC%2FB) 即要求 ![[公式]](https://www.zhihu.com/equation?tex=S%3E1) ）

![img](pics/v2-b45e58cebd38c1cbb108a38786f347d6_720w.jpg)2路组相联高速缓存

当缓存不命中时需要进行缓存行替换，如果对应的高速缓存组中有空的高速缓存行，则直接将其保存到空行中。但是如果没有空行，就要考虑合适的**替换策略**：

- 最简单的替换策略是随机选择要替换的行
- **最不常使用（Least-Frequently-Used，LFU）策略：** 替换过去某个时间窗口内引用次数最少的一行。
- **最近最少使用（Least-Recently-Used，LRU）策略：** 替换最后一次访问时间最久远的那一行

#### 6.4.4　全相联高速缓存

全相联高速缓存（Full Associative Cache）是用一个包含所有高速缓存行的组组成的。

#### 6.4.5　有关写的问题
 
#### 6.4.7　高速缓存参数的性能影响

衡量高速缓存的指标有：

- **命中率（Hit Rate）：** 内存引用命中的比率，`命中数量/引用数量`。
- **不命中率（Miss Rate）：** 内存引用不命中的比率，`不命中数量/引用数量`。通常，L1高速缓存为3~10%，L2高速缓存为<1%。
- **命中时间（Hit Time）：** 从高速缓存传输一个字到CPU的时间，包括组选择、行匹配和字选择时间。通常，L1高速缓存需要4个时钟周期，L2高速缓存需要10个时钟周期。
- **不命中处罚（Miss Penalty）：** 当缓存不命中时，要从下一层的存储结构中传输对应块到当前层中，需要额外的时间（不包含命中时间）。通常，主存需要50~200个时钟周期。

**注意：** 命中和不命中两者对性能影响很大，比如99%命中率的性能会比97%命中率高两倍。

接下来讨论高速缓存中不同参数对高速缓存性能的影响：

![img](pics/v2-a26dbb445b61f888567ab72014633ca2_720w.jpg)

想要编写高速缓存友好（Cache Friendly）的代码，**基本方法为：**

- 让最常见的情况运行得快，将注意力集中在核心函数的循环中
- 尽可能减少每个循环内部的缓存不命中，可以对局部变量反复引用，因为编译器会将其保存到寄存器中，其他的变量最好使用步长为1的引用模式。


### 6.5　编写高速缓存友好的代码
### 6.6　综合：高速缓存对程序性能的影响
#### 6.6.1　存储器山

一个程序从存储器系统中读取数据的速率称为**读吞吐量（Read Throughput）**或**读带宽（Read Bandwidth）** ，单位为`MB/s`。 我们通过以下代码来衡量空间局部性和时间局部性对程序吞吐量的影响

![img](pics/v2-f80d3dc5a77671c44450285f4c997221_720w.jpg)

第37行我们首先对高速缓存进行暖身，然后在第38行计算程序运行的时钟周期个数。

- **时间局部性：** 通过`size`来控制我们工作集的大小，由此来控制工作集存放的高速缓存的级别。假设工作集很小，则工作集会全部存放在L1高速缓存中，模拟了时间局部性优异的程序反复读取之前访问过的数据，则都是从L1高速缓存读取数据的。假设工作集很大，则工作集会存放到L3高速缓存中，模拟了时间局部性很差的程序，不断读取新的数据，则会出现缓存不命中，而不断从L3高速缓存中取数据的过程。所以通过控制工作集大小，来模拟程序局部性。
- **空间局部性：** 通过`stride`来控制读取的步长，来控制程序的空间局部性。

通过调整`size`和`stride`来度量程序的吞吐量，可以得到以下存储器山（Memory Mountain）

![img](pics/v2-9138e6ee2de307d51925388b2f6ff952_720w.jpg)

可以保持`stride`不变，观察高速缓存的大小和时间局部性对性能的影响

![img](pics/v2-25318f54b07e9c5b04fb7da56fdd07db_720w.jpg)

可以发现，当工作集大小小于L1高速缓存的大小时，模拟了时间局部性很好的程序，所有读都是直接在L1高速缓存中进行的，则吞吐量较高；当工作集大小较大时，模拟了时间局部性较差的程序，读操作需要从更高的高速缓存中加载，则吞吐量下降了。

可以保持工作集为4MB，沿着L3山脊查看空间局部性对性能的影响

![img](pics/v2-d477dbe3deb3abf57143b85f565a9afa_720w.jpg)

可以发现，步长越小越能充分利用L1高速缓存，使得吞吐量较高。当步长为8字节时，会跨越64字节，而当前高速缓存的块大小只有64字节，说明每次读取都无法在L2高速缓存中命中，都需要从L3高速缓存读取，所以后续保持不变。

**综上所述：** 需要利用时间局部性来访问L1高速缓存，还需要利用空间局部性，使得尽可能多的字从一个高速缓存行中读取到。

#### 6.6.2　重新排列循环以提高空间局部性

我们可以有不同的循环方式来实现矩阵乘法

![img](pics/v2-6bbe62a7cfd8f056115a07f20978c8e1_720w.jpg)

假设每个块中能保存4个元素，则可以分析每个变量的命中率

![img](pics/v2-cec776bff5e8f8509dfe8abafe9a91b9_720w.jpg)

说明我们可以对循环重排列，来提高空间局部性，增加命中率。

#### 6.6.3　在程序中利用局部性

分块的主要思想是将一个程序中的数据结构组织成大的**片（Chunk）**，使得能够将一个片加载到L1高速缓存中，并在这个偏重进行读写。

![img](pics/v2-db473872415172a66013ad0cd8d2dc34_720w.jpg)

如上图所示是一个普通的矩阵乘法函数，这里将二维数组想象成一个连续的字节数组，通过显示计算偏移量进行计算。这里假设每个块中可保存8个元素，并且高速缓存容量远小于矩阵的行列数。

每一次迭代就计算一个C的元素值，我们分析每一次迭代的不命中次数

![img](pics/v2-bc67b5311d1e669da02dd6d3cce20714_720w.jpg)

对于矩阵`a`，一次会保存行的8个元素到块中，则一行元素一共会有`n/8`次不命中。对于矩阵`b`，因为是列优先读取的，所以无法利用高速缓存中保存的块，所以一行元素会有n次不命中。则一共会有`9n/8`次不命中，对于C中的`n*n`个元素，一共会有 ![[公式]](https://www.zhihu.com/equation?tex=9n%5E3%2F8) 次不命中。

![img](pics/v2-a4883999ac783bcd14b90632b4905630_720w.jpg)

如上图所示是使用分块技术实现的矩阵乘法，将矩阵乘法分解为若干个`BxB`小矩阵的乘法，每次能将一个`BxB`的小矩阵加载到缓存中。

每一次迭代就计算C中一个`BxB`大小的块，我们分析每一次迭代的不命中次数

![img](pics/v2-1ced9758084018e0653d9aee26cb5109_720w.jpg)

每个块有 ![[公式]](https://www.zhihu.com/equation?tex=B%5E2%2F8) 次不命中次数，而每一行每一列有`n/B`个块，所以计算一次C中的一个块会有 ![[公式]](https://www.zhihu.com/equation?tex=2n%2FB%5Ctimes+B%5E2%2F8%3DnB%2F4) 次不命中，则一共会有 ![[公式]](https://www.zhihu.com/equation?tex=nB%2F4%5Ctimes+%28n%2FB%29%5E2%3Dn%5E3%2F%284B%29) ，我们就能调整B的大小来减小不命中率。

分块降低不命中率是因为加载一个块后，就反复使用该块，提高了空间局部性。

> 分块技术的介绍：[http://csapp.cs.cmu.edu/2e/waside/waside-blocking.pdf](https://link.zhihu.com/?target=http%3A//csapp.cs.cmu.edu/2e/waside/waside-blocking.pdf)

**建议：**

- 将注意力集中在内循环中，因为大部分的计算和内存访问都集中在这里
- 按照数据对象存储在内存中的顺序，以步长为1来读数据，使得空间局部性最大。比如步长为2的命中率就比步长为1的命中率降低一半。
- 一旦从存储器读入一个数据对象时，就尽可能使用它，使得时间局部性最大。特别是局部变量，编译器会将其保存在寄存器中。
