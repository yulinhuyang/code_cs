参考：

https://python3-cookbook.readthedocs.io/zh_CN/latest/

https://note-zw.readthedocs.io/zh/latest/Python/Python%20CookBook%20%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/index.html

https://blog.csdn.net/if_it_is_deadline/article/details/80298777

https://github.com/guoruibiao/Effective-Python


# 目录

 
# 目录

### 1 effective python

**第1章　用Pythonic方式来思考 ** 

第1条：确认自己所用的Python版本     
第2条：遵循PEP 8风格指南     
第3条：了解bytes、str与unicode的区别     
第4条：用辅助函数来取代复杂的表达式    
第5条：了解切割序列的办法     
第6条：在单次切片操作内，不要同时指定start、end和stride     
第7条：用列表推导来取代map和filter     
第8条：不要使用含有两个以上表达式的列表推导     
第9条：用生成器表达式来改写数据量较大的列表推导     
第10条：尽量用enumerate取代range     
第11条：用zip函数同时遍历两个迭代器     
第12条：不要在for和while循环后面写else块     
第13条：合理利用try/except/else/finally结构中的每个代码块     

**第2章　函数**  

第14条：尽量用异常来表示特殊情况，而不要返回None     
第15条：了解如何在闭包里使用外围作用域中的变量     
第16条：考虑用生成器来改写直接返回列表的函数     
第17条：在参数上面迭代时，要多加小心     
第18条：用数量可变的位置参数减少视觉杂讯     
第19条：用关键字参数来表达可选的行为     
第20条：用None和文档字符串来描述具有动态默认值的参数     
第21条：用只能以关键字形式指定的参数来确保代码明晰     

**第3章　类与继承**  

第22条：尽量用辅助类来维护程序的状态，而不要用字典和元组     
第23条：简单的接口应该接受函数，而不是类的实例     
第24条：以@classmethod形式的多态去通用地构建对象     
第25条：用super初始化父类     
第26条：只在使用Mix-in组件制作工具类时进行多重继承     
第27条：多用public属性，少用private属性     
第28条：继承collections.abc以实现自定义的容器类型    
 
**第4章　元类及属性**

第29条：用纯属性取代get和set方法     
第30条：考虑用@property来代替属性重构     
第31条：用描述符来改写需要复用的@property方法     
第32条：用__getattr__、__getattribute__和__setattr__实现按需生成的属性     
第33条：用元类来验证子类    
第34条：用元类来注册子类    
第35条：用元类来注解类的属性    

**第5章　并发及并行**
 
第36条：用subprocess模块来管理子进程    
第37条：可以用线程来执行阻塞式I/O，但不要用它做平行计算    
第38条：在线程中使用Lock来防止数据竞争    
第39条：用Queue来协调各线程之间的工作    
第40条：考虑用协程来并发地运行多个函数    
第41条：考虑用concurrent.futures来实现真正的平行计算    

**第6章　内置模块**

第42条：用functools.wraps定义函数修饰器    
第43条：考虑以contextlib和with语句来改写可复用的try/finally代码    
第44条：用copyreg实现可靠的pickle操作    
第45条：应该用datetime模块来处理本地时间，而不是用time模块    
第46条：使用内置算法与数据结构    
第47条：在重视精确度的场合，应该使用decimal    
第48条：学会安装由Python开发者社区所构建的模块   
 
**第7章　协作开发**

第49条：为每个函数、类和模块编写文档字符串    
第50条：用包来安排模块，并提供稳固的API    
第51条：为自编的模块定义根异常，以便将调用者与API相隔离    
第52条：用适当的方式打破循环依赖关系    
第53条：用虚拟环境隔离项目，并重建其依赖关系    

**第8章　部署**
 
第54条：考虑用模块级别的代码来配置不同的部署环境    
第55条：通过repr字符串来输出调试信息    
第56条：用unittest来测试全部代码    
第57条：考虑用pdb实现交互调试    
第58条：先分析性能，然后再优化    
第59条：用tracemalloc来掌握内存的使用及泄漏情况   

### 2 python_cookbook

**第1章 数据结构和算法**    
 
1.1 将序列分解为单独的变量    
1.2 从任意长度的可迭代对象中分解元素    
1.3 保存最后N个元素    
1.4 找到最大或最小的N个元素    
1.5 实现优先级队列    
1.6 在字典中将键映射到多个值上    
1.7 让字典保持有序    
1.8 与字典有关的计算问题    
1.9 在两个字典中寻找相同点    
1.10 从序列中移除重复项且保持元素间顺序不变   
1.11 对切片命名    
1.12 找出序列中出现次数最多的元素    
1.13 通过公共键对字典列表排序    
1.14 对不原生支持比较操作的对象排序   
1.15 根据字段将记录分组    
1.16 筛选序列中的元素    
1.17 从字典中提取子集    
1.18 将名称映射到序列的元素中    
1.19 同时对数据做转换和换算   
1.20 将多个映射合并为单个映射   
 
**第2章 字符串和文本**   

2.1 针对任意多的分隔符拆分字符串   
2.2 在字符串的开头或结尾处做文本匹配   
2.3 利用Shell通配符做字符串匹配   
2.4 文本模式的匹配和查找    
2.5 查找和替换文本    
2.6 以不区分大小写的方式对文本做查找和替换   
2.7 定义实现最短匹配的正则表达式   
2.8 编写多行模式的正则表达式   
2.9 将Unicode文本统一表示为规范形式   
2.10 用正则表达式处理Unicode字符   
2.11 从字符串中去掉不需要的字符   
2.12 文本过滤和清理   
2.13 对齐文本字符串   
2.14 字符串连接及合并   
2.15 给字符串中的变量名做插值处理   
2.16 以固定的列数重新格式化文本   
2.17 在文本中处理HTML和XML实体   
2.18 文本分词   
2.19 编写一个简单的递归下降解析器   
2.20 在字节串上执行文本操作   

**第3章 数字、日期和时间**   

3.1 对数值进行取整   
3.2 执行精确的小数计算   
3.3 对数值做格式化输出    
3.4 同二进制、八进制和十六进制数打交道   
3.5 从字节串中打包和解包大整数   
3.6 复数运算   
3.7 处理无穷大和NaN    
3.8 分数的计算   
3.9 处理大型数组的计算    
3.10 矩阵和线性代数的计算    
3.11 随机选择    
3.12 时间换算    
3.13 计算上周5的日期    
3.14 找出当月的日期范围    
3.15 将字符串转换为日期    
3.16 处理涉及到时区的日期问题   
 
**第4章 迭代器和生成器** 

4.1 手动访问迭代器中的元素    
4.2 委托迭代    
4.3 用生成器创建新的迭代模式    
4.4 实现迭代协议    
4.5 反向迭代    
4.6 定义带有额外状态的生成器函数    
4.7 对迭代器做切片操作    
4.8 跳过可迭代对象中的前一部分元素    
4.9 迭代所有可能的组合或排列    
4.10 以索引-值对的形式迭代序列    
4.11 同时迭代多个序列    
4.12 在不同的容器中进行迭代    
4.13 创建处理数据的管道    
4.14 扁平化处理嵌套型的序列    
4.15 合并多个有序序列，再对整个有序序列进行迭代    
4.16 用迭代器取代while循环   
 
**第5章 文件和I/O**    

5.1 读写文本数据    
5.2 将输出重定向到文件中    
5.3 以不同的分隔符或行结尾符完成打印    
5.4 读写二进制数据       
5.5 对已不存在的文件执行写入操作    
5.6 在字符串上执行I/O操作    
5.7 读写压缩的数据文件    
5.8 对固定大小的记录进行迭代    
5.9 将二进制数据读取到可变缓冲区中    
5.10 对二进制文件做内存映射    
5.11 处理路径名       
5.12 检测文件是否存在    
5.13 获取目录内容的列表    
5.14 绕过文件名编码    
5.15 打印无法解码的文件名    
5.16 为已经打开的文件添加或修改编码方式    
5.17 将字节数据写入文本文件    
5.18 将已有的文件描述符包装为文件对象    
5.19 创建临时文件和目录    
5.20 同串口进行通信    
5.21 序列化Python对象
    
**第6章 数据编码与处理**   
 
6.1 读写CSV数据    
6.2 读写JSON数据    
6.3 解析简单的XML文档    
6.4 以增量方式解析大型XML文件    
6.5 将字典转换为XML    
6.6 解析、修改和重写XML    
6.7 用命名空间来解析XML文档    
6.8 同关系型数据库进行交互    
6.9 编码和解码十六进制数字    
6.10 Base64编码和解码    
6.11 读写二进制结构的数组    
6.12 读取嵌套型和大小可变的二进制结构    
6.13 数据汇总和统计    

**第7章 函数** 

7.1 编写可接受任意数量参数的函数         
7.2 编写只接受关键字参数的函数    
7.3 将元数据信息附加到函数参数上    
7.4 从函数中返回多个值    
7.5 定义带有默认参数的函数    
7.6 定义匿名或内联函数    
7.7 在匿名函数中绑定变量的值    
7.8 让带有N个参数的可调用对象以较少的参数形式调用    
7.9 用函数替代只有单个方法的类    
7.10 在回调函数中携带额外的状态    
7.11 内联回调函数    
7.12 访问定义在闭包内的变量    

**第8章 类与对象** 

8.1 修改实例的字符串表示    
8.2 自定义字符串的输出格式        
8.3 让对象支持上下文管理协议        
8.4 当创建大量实例时如何节省内存        
8.5 将名称封装到类中        
8.6 创建可管理的属性        
8.7 调用父类中的方法        
8.8 在子类中扩展属性        
8.9 创建一种新形式的类属性或实例属性        
8.10 让属性具有惰性求值的能力        
8.11 简化数据结构的初始化过程            
8.12 定义一个接口或抽象基类        
8.13 实现一种数据模型或类型系统        
8.14 实现自定义的容器     
8.15 委托属性的访问     
8.16 在类中定义多个构造函数     
8.17 不通过调用init来创建实例     
8.18 用Mixin技术来扩展类定义     
8.19 实现带有状态的对象或状态机     
8.20 调用对象上的方法，方法名以字符串形式给出     
8.21 实现访问者模式     
8.22 实现非递归的访问者模式     
8.23 在环状数据结构中管理内存     
8.24 让类支持比较操作     
8.25 创建缓存实例    
 
**第9章 元编程** 

9.1 给函数添加一个包装     
9.2 编写装饰器时如何保存函数的元数据     
9.3 对装饰器进行解包装     
9.4 定义一个可接受参数的装饰器     
9.5 定义一个属性可由用户修改的装饰器     
9.6 定义一个能接收可选参数的装饰器     
9.7 利用装饰器对函数参数强制执行类型检查     
9.8 在类中定义装饰器     
9.9 把装饰器定义成类     
9.10 把装饰器作用到类和静态方法上     
9.11 编写装饰器为被包装的函数添加参数     
9.12 利用装饰器给类定义打补丁     
9.13 利用元类来控制实例的创建     
9.14 获取类属性的定义顺序     
9.15 定义一个能接受可选参数的元类     
9.16 在/*args和/**kwargs上强制规定一种参数签名     
9.17 在类中强制规定编码约定              
9.18 通过编程的方式来定义类           
9.19 在定义的时候初始化类成员           
9.20 通过函数注解来实现方法重载           
9.21 避免出现重复的属性方法           
9.22 以简单的方式定义上下文管理器           
9.23 执行带有局部副作用的代码           
9.24 解析并分析Python源代码           
9.25 将Python源码分解为字节码           

**第10章 模块和包** 

10.1 把模块按层次结构组织成包       
10.2 对所有符号的导入进行精确控制       
10.3 用相对名称来导入包中的子模块       
10.4 将模块分解成多个文件       
10.5 让各个目录下的代码在统一的命名空间下导入       
10.6 重新加载模块       
10.7 让目录或zip文件成为可运行的脚本       
10.8 读取包中的数据文件       
10.9 添加目录到sys.path中       
10.10 使用字符串中给定的名称来导入模块       
10.11 利用import钩子从远端机器上加载模块       
10.12 在模块加载时为其打补丁       
10.13 安装只为自己所用的包       
10.14 创建新的Python环境       
10.15 发布自定义的包       

**第11章 网络和Web编程** 

11.1 以客户端的形式同HTTP服务交互       
11.2 创建一个TCP服务器       
11.3 创建一个UDP服务器       
11.4 从CIDR地址中生成IP地址的范围       
11.5 创建基于REST风格的简单接口       
11.6 利用XML-RPC实现简单的远端过程调用       
11.7 在不同的解释器间进行通信       
11.8 实现远端过程调用       
11.9 以简单的方式验证客户端身份       
11.10 为网络服务增加SSL支持       
11.11 在进程间传递socket文件描述符       
11.12 理解事件驱动型I/O       
11.13 发送和接收大型数组       

**第12章 并发** 

12.1 启动和停止线程       
12.2 判断线程是否已经启动       
12.3 线程间通信       
12.4 对临界区加锁       
12.5 避免死锁       
12.6 保存线程专有状态       
12.7 创建线程池       
12.8 实现简单的并行编程       
12.9 如何规避GIL带来的限制       
12.10 定义一个Actor任务       
12.11 实现发布者/订阅者消息模式       
12.12 使用生成器作为线程的替代方案       
12.13 轮询多个线程队列       
12.14 在UNIX上加载守护进程       

**第13章 实用脚本和系统管理** 

13.1 通过重定向、管道或输入文件来作为脚本的输入       
13.2 终止程序并显示错误信息       
13.3 解析命令行选项       
13.4 在运行时提供密码输入提示       
13.5 获取终端大小       
13.6 执行外部命令并获取输出       
13.7 拷贝或移动文件和目录       
13.8 创建和解包归档文件       
13.9 通过名称来查找文件       
13.10 读取配置文件       
13.11 给脚本添加日志记录       
13.12 给库添加日志记录       
13.13 创建一个秒表计时器       
13.14 给内存和CPU使用量设定限制       
13.15 加载Web浏览器       

**第14章 测试、调试以及异常** 

14.1 测试发送到stdout上的输出       
14.2 在单元测试中为对象打补丁       
14.3 在单元测试中检测异常情况       
14.4 将测试结果作为日志记录到文件中       
14.5 跳过测试，或者预计测试结果为失败       
14.6 处理多个异常       
14.7 捕获所有的异常       
14.8 创建自定义的异常       
14.9 通过引发异常来响应另一个异常       
14.10 重新抛出上一个异常       
14.11 发出告警信息       
14.12 对基本的程序崩溃问题进行调试       
14.13 对程序做性能分析以及计时统计       
14.14 让你的程序运行得更快      
 
**第15章 C语言扩展**       

15.1 利用ctypes来访问C代码       
15.2 编写简单的C语言扩展模块       
15.3 编写一个可操作数组的扩展函数       
15.4 在C扩展模块中管理不透明指针       
15.5 在扩展模块中定义并导出C API       
15.6 从C中调用Python       
15.7 在C扩展模块中释放GIL       
15.8 混合使用C和Python环境中的线程       
15.9 用Swig来包装C代码       
15.10 用Cython来包装C代码       
15.11 用Cython来高效操作数组       
15.12 把函数指针转换为可调用对象       
15.13 把以NULL结尾的字符串传给C库       
15.14 把Unicode字符串传递给C库       
15.15 把C字符串转换到Python中       
15.16 同编码方式不确定的C字符串打交道       
15.17 把文件名传给C扩展模块       
15.18 把打开的文件传给C扩展模块       
15.19 在C中读取文件型对象       
15.20 从C中访问可迭代对象       
15.21 排查段错误      



# 笔记

## 1 effective python

### 第1章　用Pythonic方式来思考  

#### 第1条：确认自己所用的Python版本 

#### 第2条：遵循PEP 8风格指南 

注意让写的代码尽量Pythonic和符合PEP8标准

### 基础结构

**命名法:** 

函数，变量以及属性应该使用小写，如果有多个单词推荐使用下划线进行连接，如lowercase_underscore

被保护 的属性应该使用 单个 前导下划线来声明。

私有 的属性应该使用 两个 前导下划线来进行声明。

类以及异常信息 应该使用单词 首字母大写 形式，也就是我们经常使用的驼峰命名法，如CapitalizedWord。

模块级 别的常量应该使用 全部大写 的形式, 如ALL_CAPS。

类内部的实例方法的应该将self作为其第一个参数。且self也是对当前类对象的引用。

类方法应该使用cls来作为其第一个参数。且self引用自当前类。

**表达式和语句:** 

如果非要相对的引用，应该使用明确的语法from . import foo。

按照以下规则引入模块：标准库，第三方库，你自己的库。每一个部分内部也应该按照字母顺序来引入


**引用-变化-追随:**

当为列表赋值的时候省去开头和结尾下标的时候，将会用 这个引用 来替换整个列表的内容，而不是创建一个新的列表。同时，引用了这个列表的列表的相关内容，也会跟着发生变化。

**切片：**

避免在分片中同时使用start，end，stride；如果非要使用，考虑两次赋值（一个分片，一个调幅），或者使用内置模块itertoolsde 的 islice方法来进行处理。

**列表表达式for  in  if 结构**

用列表推导来代替 map 和 filter，字典和集合也都支持列表表达式

    chile_rank = {'ghost':1,'habanero':2,'cayenne':3}

    rank_dict = {rank:name for name,rank in child_rank.items()}

    chile_len_set = {len(name) for name in rank_dict.values()}

避免使用超过两个的表达式，可以使用多if代替

b = [x for x in a if x> 4 if x%2 ==0]

c = [x for x in a if x > 4 and if x%2 ==0]




**PEP8 Python 编码规范补充**

文档编排：

1 模块内容的顺序：模块说明和docstring—import—globals&constants—其他定义。其中import部分，又按标准、三方和自己编写顺序依次排放，之间空一行。

2 不要在一句import中多个库，比如import os, sys不推荐。

3 如果采用from XX import XX引用库，可以省略‘module.’，都是可能出现命名冲突，这时就要采用import XX。

文档描述：
 
为所有的共有模块、函数、类、方法写docstrings

命名规范：

 类的属性有3种作用域public、non-public和subclass API，可以理解成C++中的public、private、protected，non-public属性前，前缀一条下划线。
 
 类的方法第一个参数必须是self，而静态方法第一个参数必须是cls。
 
 用“has”或“is”前缀命名布尔元素
 
编码建议；

1 编码中考虑到其他python实现的效率等问题，比如运算符‘+’在CPython（Python）中效率很高，都是Jython中却非常低，所以应该采用.join()的方式。

2 尽可能使用‘is’‘is not’取代‘==’，比如if x is not None 要优于if x。

3 使用startswith() and endswith()代替切片进行序列前缀或后缀的检查。 使用isinstance()比较对象的类型。
 

#### 第3条：了解bytes、str与unicode的区别  

#### 第4条：用辅助函数来取代复杂的表达式 

#### 第5条：了解切割序列的办法  

#### 第6条：在单次切片操作内，不要同时指定start、end和stride  

#### 第7条：用列表推导来取代map和filter  

#### 第8条：不要使用含有两个以上表达式的列表推导  

#### 第9条：用生成器表达式来改写数据量较大的列表推导  

考虑使用生成器而不是返回列表

相较于返回一个列表的情况，替代方案中使用生成器可以使得代码变得更加的清晰。生成器返回的迭代器，是在其生成器内部一个把值传递给了yield变量的集合。

生成器可以处理很大的输出序列就是因为它在处理的时候不会完全的包含所有的数据。

    def index_words_iter(text):
        if text:
            yield 0
        for index, letter in enumerate(text):
            if letter == ' ':
                yield index + 1

    result = list(index_words_iter(address))

#### 第10条：尽量用enumerate取代range  

enumerate 比range更好用:

enumerate提供了简洁的语法，再循环迭代一个迭代器的同时既能获取下标，也能获取当前值。

Prefer：
```python
    for i, flavor in enumerate(flavor_list):

        print(‘%d: %s’ % (i + 1, flavor)) 
```


#### 第11条：用zip函数同时遍历两个迭代器

内置的zip函数可以并行的对多个迭代器进行处理;如果所处理的迭代器的长度不一致时，zip会默认截断输出.内置模块itertools中的zip_longest函数可以并行地处理多个迭代器，而可以无视长度不一致的问题。

Prefer:

```python
names = [‘Cecilia’, ‘Lise’, ‘Marie’]

max_letters = 0

letters = [len(n) for n in names]

for name, count in zip(names, letters): 

``` 

#### 第12条：不要在for和while循环后面写else块  

#### 第13条：合理利用try/except/else/finally结构中的每个代码块  

### 第2章　函数  

#### 第14条：尽量用异常来表示特殊情况，而不要返回None  

#### 第15条：了解如何在闭包里使用外围作用域中的变量 

**闭包中是怎样使用外围作用域变量**

global 关键字用于使用全局变量，nonlocal 关键字用于使用局部变量(函数内)

Python编译器变量查找域的顺序：   
- 当前函数的作用域    
- 任何其他的封闭域（比如其他的包含着的函数）。   
- 包含该段代码的模块域（也称之为全局域）   
- 内置域（包含了像len,str等函数的域）   
 

#### 第16条：考虑用生成器来改写直接返回列表的函数  

#### 第17条：在参数上面迭代时，要多加小心  

#### 第18条：用数量可变的位置参数减少视觉杂讯  

#### 第19条：用关键字参数来表达可选的行为   

位置参数： 使用*args定义语句，函数可以接收可变数量的位置参数

```python
def log(message, *values):
	if not values:
		print(message)
	else:
		values_str = ', '.join(str(x) for x in values)
		print('%s: %s' % (message, values_str))

log('My numbers are', 1, 2)
log('Hi there')
```    

关键字参数：

可选的关键字参数应该优于位置参数.    
关键字参数的好处:代码可读性的提高，以在定义的时候初始化一个默认值，在前面的调用方式不变的情况下可以很好的拓展函数的参数，不用修改太多的代码.    

```python
def flow_rate(weight_diff, time_diff, period=1):
	return (weight_diff / time_diff) * period
```

默认参数：使用None和文档说明动态的指定默认参数 

#### 第20条：用None和文档字符串来描述具有动态默认值的参数  

prefer:

```python
def log(message, when=None):
	"""Log a message with a timestamp.

	Args:
		message: Message to print
		when: datetime of when the message occurred.
			Default to the present time.
	"""
	when = datetime.now() if when is None else when
	print("%s: %s" %(when, message))

# 测试

log('Hi there!')
sleep(0.1)
log('Hi again!')
```


#### 第21条：用只能以关键字形式指定的参数来确保代码明晰  

### 第3章　类与继承  

#### 第22条：尽量用辅助类来维护程序的状态，而不要用字典和元组  

避免字典中嵌套字典，或者长度较大的元组。    
在一个整类（类似于前面第一个复杂类那样）之前考虑使用 namedtuple 制作轻量，不易发生变化的容器。    
当内部的字典关系变得复杂的时候将代码重构到多个工具类中。    
你可以从依赖树的底端开始，将其划分成多个类。

prefer:

```python
import collections

Grade = collections.namedtuple('Grade', ('score', 'weight'))

class Subject(object):
	def __init__(self):
		self._grades = []

	def report_grade(self, score, weight):
		self._grades.append(Grade(score, weight))

	def average_grade(self):
		total, total_weight = 0, 0
		for grade in self._grades:
			total += grade.score * grade.weight
			total_weight += grade.weight
		return total / total_weight


class Student(object):
	def __init__(self):
		self._subjects = {}

	def subject(self, name):
		if name not in self._subjects:
			self._subjects[name] = Subject()
		return self._subjects[name]

	def average_grade(self):
		total, count = 0, 0
		for subject in self._subjects.values():
			total += subject.average_grade()
			count += 1
		return total / count


class Gradebook(object):
	def __init__(self):
		self._students = {}

	def student(self, name):
		if name not in self._students:
			self._students[name] = Student()
		return self._students[name]
```

#### 第23条：简单的接口应该接受函数，而不是类的实例

对于简单接口组件而言，函数就足够了。Python中引用函数和方法的原因就在于它们是first-class，可以直接的被运用在表达式中。

特殊方法__call__允许你像调用函数一样调用一个对象实例。需要一个函数来维护状态信息，考虑一个定义了__call__方法的状态闭包类。

内置的API都允许你通过向函数传递参数,如:

```python
	names = ['Socrates', 'Archimedes', 'Plato', 'Aristotle']
	names.sort(key=lambda x: len(x))
```

python hooks：函数可以作为钩子来工作，因为Python有first-class函数，函数、方法可以像其他的变量值一样被引用，或者被传递给其他的函数

```python
current = {'green': 12, 'blue': 3}
incremetns = [
	('red', 5),
	('blue', 17),
	('orange', 9)
]

class BetterCountMissing(object):

	def __init__(self):
		self.added = 0

	def __call__(self):
		self.added += 1
		return 0

counter = BetterCountMissing()
counter()
assert callable(counter)
# 这里我使用一个BetterCountMissing实例作为defaultdict函数的默认的hook值来追踪缺省值被添加的次数。
counter = BetterCountMissing()
result = defaultdict(counter, current)
for key, amount in increments:
	result[key] += amount
assert counter.added == 2
``` 

#### 第24条：以@classmethod形式的多态去通用地构建对象

```python
Python的每个类只支持单个的构造方法，__init__。

使用@classmethod可以为你的类定义可替代构造方法的方法。

class GenericInputData(object):
    def read(self):
        raise NotImplementedError

    @classmethod
    def generate_inputs(cls, config):
        raise NotImplementedError

class PathInputData(GenericInputData):
    def __init__(self, path):
        super().__init__()
        self.path = path

    def read(self):
        return open(self.path).read()

    @classmethod
    def generate_inputs(cls, config):
        data_dir = config['data_dir']
        for name in os.listdir(data_dir):
            yield cls(os.path.join(data_dir, name))
```  

#### 第25条：用super初始化父类  

Python的解决实例化次序问题的方法MRO解决了菱形继承（ diamond inheritance ）中超类多次被初始化的问题。    
总是应该使用super来初始化父类。    
python中父类实例化的规则是按照MRO标准来进行的，MRO 的执行顺序是 DFS。    

```python
class MyBaseClass(object):
	def __init__(self, value):
		self.value = value

class Implicit(MyBaseClass):
	def __init__(self, value):
		super().__init__(value * 2)
```
			
#### 第26条：只在使用Mix-in组件制作工具类时进行多重继承 

如果可以使用mix-in实现相同的结果输出的话，就不要使用多继承了。   
当mix-in类需要的时候，在实例级别上使用可插拔的行为可以为每一个自定义的类工作的更好。    
从简单的行为出发，创建功能更为灵活的mix-in。   

Mix-in类：一个只定义了几个类必备功能方法的很小的类，不定义以自己的实例属性，也不需要它们的初始化方法__init__被调用。   

```python
import json

class ToDictMixin(object):
	def to_dict(self):
		return self._traverse_dict(self.__dict__)

	def _traverse_dict(self, instance_dict):
		output = {}
		for key, value in instance_dict.items():
			output[key] = self._traverse(key, value)
		return output

class JsonMixin(object):
	@classmethod
	def from_json(cls, data):
		kwargs = json.loads(data)
		return cls(**kwargs)

	def to_json(self):
		return json.dumps(self.to_dict())
``` 

#### 第27条：多用public属性，少用private属性  

Python 编译器无法严格保证 private 字段的私密性。    
不要盲目将属性设置为private，而是应该从一开始就做好规划，并允子类更多地访问超类的内部的API。    
应该多用protected 属性，并且在文档中把这些字段的合理用法告诉子类的开发者，而不要试图用 private 属性来限制子类的访问。   

```python
class ClassDef(object):
	def __init__(self):
		# public
		self.name = "class_def"
		# private
		self.__age = 29
		# protected
		self._sex = "man"

	def fun1(self):
		print("call public function")

	def __fun2(self):
		print("call private function")

	def _fun3(self):
		print("call protected function")
```

默认都是pubic类型，被子类、类内以及类外被访问

protected类型： _xx 以单下划线开头表示的是protected类型的变量或者方法，只能允许其本身与子类进行访问。    
private类型：__xx 双下划线表示的是私有类型的变量或者方法, 只能允许类内进行访问。    
特列方法:   __xx__定义的是特列方法。用户控制的命名空间内的变量或是属性。    

#### 第28条：继承collections.abc以实现自定义的容器类型  

如果要定制的子类比较简单，那就可以直接从Python的容器类型（如list或dict）中继承。    
编写自制的容器类型时，可以从collection.abc 模块的抽象类基类中继承，那些基类能确保我们的子类具备适当的接口及行为。   
collections.abc的 Sequence : 子类只要实现 __getitem__以及 __len__， Sequence 提供了 count() 以及 index()。     

### 第4章　元类及属性  

#### 第29条：用纯属性取代get和set方法 

使用public属性避免set和get方法，@property定义一些特别的行为。  
@property 方法应该遵循最小惊讶原则，而不应该产生奇怪的副作用。  

```python
class VoltageResistance(Resistor):
	def __init__(self, ohms):
		super().__init__(ohms)
		self._voltage = 0

	# 相当于 getter
	@property
	def voltage(self):
		return self._voltage

	# 相当于 setter
	@voltage.setter
	def voltage(self, voltage):
		self._voltage = voltage
		self.current = self._voltage / self.ohms

r2 = VoltageResistance(1e3)
print('Before: %5r amps' % r2.current)
# 会执行 setter 方法
r2.voltage = 10
print('After:  %5r amps' % r2.current) 
```
	
#### 第30条：考虑用@property来代替属性重构 

使用@property给已有属性扩展新需求，可以用 @property 来逐步完善数据模型。
当@property太复杂了才考虑重构。

#### 第31条：用描述符来改写需要复用的@property方法  

如果想复用 @property 方法及其验证机制，那么可以自定义描述符类.  
WeakKeyDictionary 可以保证描述符类不会泄露内存.    
通过描述符协议来实现属性的获取和设置操作时，不要纠结于__getatttttribute__ 的方法的具体运作细节.  
    
not prefer：

```python
class Exam(object):
	def __init__(self):
		self._writing_grade = 0
		self._math_grade = 0

	@staticmethod
	def _check_grade(value):
		if not (0 <= value <= 100):
			raise ValueError('Grade must be between 0 and 100')

	@property
	def writing_grade(self):
		return self._writing_grade

	@writing_grade.setter
	def writing_grade(self, value):
		self._check_grade(value)
		self._writing_grade = value

	@property
	def math_grade(self):
		return self._math_grade

	@math_grade.setter
	def math_grade(self, value):
		self._check_grade(value)
		self._math_grade = value

prefer:
from weakref import WeakKeyDictionary

class Grade(object):
	def __init__(self):
		self._values = WeakKeyDictionary()
	def __get__(self, instance, instance_type):
		if instance is None: return self
		return self._values.get(instance, 0)

	def __set__(self, instance, value):
		if not (0 <= value <= 100):
			raise ValueError('Grade must be between 0 and 100')
		self._values[instance] = value

class Exam(object):
	math_grade = Grade()
	writing_grade = Grade()
	science_grade = Grade()

exam = Exam()
exam.writing_grade = 40
```	

#### 第32条：用__getattr__、__getattribute__和__setattr__实现按需生成的属性  

通过__getattr__ 和 __setattr__，可以用惰性的方式来加载并保存对象的属性。    
要理解 __getattr__ 和 __getattribute__ 的区别：前者只会在待访问的属性缺失时触发，而后者则会在每次访问属性的时候触发。    
如果要在__getattributte__ 和 __setattr__ 方法中访问实例属性，那么应该直接通过 super() 来做，以避免无限递归。   
obj.name，getattr和hasattr都会调用getattribute方法，如果name不在obj.dict里面，还会调用getattr方法，如果没有自定义getattr方法会AttributeError异常    

只要有赋值操作（=，setattr）都会调用setattr方法（包括a = A())。

#### 第33条：用元类来验证子类 

通过元类，我们可以在生成子类对象之前，先验证子类的定义是否合乎规范，使用元类对类型对象进行验证.   
Python 系统把子类的整个 class 语句体处理完毕之后，就会调用其元类的__new__ 方法.
元类：用来创建类的“东西”。你创建类就是为了创建类的实例对象，元类就是类的类，可以把元类称为“类工厂”。

```python
MyClass = MetaClass()

MyObject = MyClass()

    >>> age = 35
    >>> age.__class__
    <type 'int'>
    >>> age.__class__.__class__
    <type 'type'>
```
	
 	
先定义metaclass，就可以创建类，最后创建实例。    
首先，定义元类，我们要继承 type, python 默认会把那些类的 class 语句体中所含的相关内容，发送给元类的 new 方法。   

```python
class Meta(type):
	def __new__(meta, name, bases, class_dict):
		print(meta, name, bases, class_dict)
		return type.__new__(meta, name, bases, class_dict)
		
class MyClassInPython3(object, metaclass=Meta):
	stuff = 123

	def foo(self):
		pass
```

#### 第34条：用元类来注册子类 

#### 第35条：用元类来注解类的属性 

借助元类，我们可以在某个类完全定义好之前，率先修改该类的属性    
描述符与元类能够有效的组合起来，以便对某种行为做出修饰，或者在程序运行时探查相关信息    
如果把元类与描述符相结合，那就可以在不使用 weakerf 模块的前提下避免内存泄露

### 第5章　并发及并行 

#### 第36条：用subprocess模块来管理子进程 

使用 subprocess 模块运行子进程管理自己的输入和输出流.
subprocess 可以并行执行最大化CPU的使用.
communicate 的 timeout 参数避免死锁和被挂起的子进程.    

#### 第37条：可以用线程来执行阻塞式I/O，但不要用它做平行计算 

因为GIL，Python thread并不能并行运行多段代码.   
Python保留thread的两个原因：1.可以模拟多线程，2.多线程可以处理I/O阻塞的情况.    
Python thread可以并行执行多个系统调用，这使得程序能够在执行阻塞式I/O操作的同时，执行一些并行计算   

#### 第38条：在线程中使用Lock来防止数据竞争

虽然Python thread不能同时执行，但是Python解释器还是会打断操作数据的两个字节码指令，所以还是需要锁.
thread模块的Lock类是Python的互斥锁实现.

#### 第39条：用Queue来协调各线程之间的工作 

管线是一种优秀的任务处理方式，它可以把处理流程划分为若干阶段，并使用多条Python线程同时执行这些任务。
构建并发式的管线时，要注意许多问题，包括：如何防止某个阶段陷入持续等待的状态之中、如何停止工作线程，以及如何防止内存膨胀等。
Queue类具备构建健壮并发管道的特性：阻塞操作，缓存大小和连接（join）。

```python
from queue import Queue
from threading import Thread

class ClosableQueue(Queue):
	SENTINEL = object()

	def close(self):
		self.put(self.SENTINEL)

	def __iter__(self):
		while True:
			item = self.get()
			try:
				if item is self.SENTINEL:
					return  # Cause the thread to exit
				yield item
			finally:
				self.task_done()

class StoppableWorker(Thread):
	def __init__(self, func, in_queue, out_queue):
		super().__init__()
		self.func = func
		self.in_queue = in_queue
		self.out_queue = out_queue

	def run(self):
		for item in self.in_queue:
			result = self.func(item)
			self.out_queue.put(result)
def download(item):
	return item

def resize(item):
	return item

def upload(item):
	return item

download_queue = ClosableQueue()
resize_queue = ClosableQueue()
upload_queue = ClosableQueue()
done_queue = ClosableQueue()
threads = [
	StoppableWorker(download, download_queue, resize_queue),
	StoppableWorker(resize, resize_queue, upload_queue),
	StoppableWorker(upload, upload_queue, done_queue),
]


for thread in threads:
	thread.start()
for _ in range(1000):
	download_queue.put(object())
download_queue.close()


download_queue.join()
resize_queue.close()
resize_queue.join()
upload_queue.close()
upload_queue.join()
print(done_queue.qsize(), 'items finished')
```

#### 第40条：考虑用协程来并发地运行多个函数 

线程有三个大问题：需要特定工具去确定安全性，单个线程需要8M的内存，线程启动消耗。   
coroutine只有1kb的内存消耗    
generator可以通过send方法把值传递给yield   

#### 第41条：考虑用concurrent.futures来实现真正的平行计算 

CPU瓶颈模块使用C扩展

concurrent.futures的multiprocessing可以并行处理一些任务

使用 concurrent.futures 里面的 ProcessPoolExecutor 可以很简单地平行处理 CPU-bound 的程式，省得用 multiprocessing 自定义。

```python
from concurrent.futures import ProcessPoolExecutor

start = time()
pool = ProcessPoolExecutor(max_workers=2)  # The one change
results = list(pool.map(gcd, numbers))
end = time()
print('Took %.3f seconds' % (end - start))
```

里面的ThreadPoolExecutor和ProcessPoolExecutor，继承了Executor，分别被用来创建线程池和进程池的代码。

将相应的tasks直接放入线程池/进程池，不需要维护Queue来操心死锁的问题，线程池/进程池会自动帮我们调。

可以使用submit和map提交任务。

```python
    import concurrent.futures
    import urllib.request

    URLS = ['http://www.foxnews.com/',
            'http://www.cnn.com/',
            'http://europe.wsj.com/',
            'http://www.bbc.co.uk/',
            'http://some-made-up-domain.com/']

    # Retrieve a single page and report the URL and contents
    def load_url(url, timeout):
        with urllib.request.urlopen(url, timeout=timeout) as conn:
            return conn.read()

    # We can use a with statement to ensure threads are cleaned up promptly
    with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
        # Start the load operations and mark each future with its URL
        future_to_url = {executor.submit(load_url, url, 60): url for url in URLS}
        for future in concurrent.futures.as_completed(future_to_url):
            url = future_to_url[future]
            try:
                data = future.result()
            except Exception as exc:
                print('%r generated an exception: %s' % (url, exc))
            else:
                print('%r page is %d bytes' % (url, len(data)))

```

### 第6章　内置模块 

#### 第42条：用functools.wraps定义函数修饰器 

装饰器可以对函数进行封装，但是会改变函数信息    
使用 functools 的 warps 可以解决这个问题   

```python
def trace(func):
  @wraps(func)
  def wrapper(*args, **kwargs):
	  # …
  return wrapper
@trace
def fibonacci(n):
  # …
```

#### 第43条：考虑以contextlib和with语句来改写可复用的try/finally代码 

使用with语句代替try/finally，增加代码可读性    
使用 contextlib 提供的 contextmanager 装饰函数就可以被 with 使用, with 和 yield 返回值使用   

实现一个新的上下文管理器的最简单的方法就是使用 contexlib 模块中的 @contextmanager 装饰器

```python

import time
from contextlib import contextmanager

@contextmanager
def timethis(label):
	start = time.time()
	try:
		yield
	finally:
		end = time.time()
		print('{}: {}'.format(label, end - start))

# Example use
with timethis('counting'):
	n = 10000000
	while n > 0:
		n -= 1
```

在函数 timethis() 中，yield 之前的代码会在上下文管理器中作为 __enter__() 方法执行， 所有在 yield 之后的代码会作为 __exit__() 方法执行。 如果出现了异常，异常会在yield语句那里抛出。

#### 第44条：用copyreg实现可靠的pickle操作 

pickle 模块只能序列化和反序列化确认没有问题的对象    
copyreg的 pickle 支持属性丢失，版本和导入类表信息   
使用 copyreg這个内建的 module, 搭配 pickle使用

```python
def pickle_game_state(game_state):
	kwargs = game_state.__dict__
	kwargs['version'] = 2
	return unpickle_game_state, (kwargs,)

def unpickle_game_state(kwargs):
	version = kwargs.pop('version', 1)
	if version == 1:
		kwargs.pop('lives')
	return GameState(**kwargs)

copyreg.pickle(GameState, pickle_game_state)
```

#### 第45条：应该用datetime模块来处理本地时间，而不是用time模块 

不要使用time模块在转换不同时区的时间   
而用datetime配合 pytz 转换   
总数保持UTC时间，最后面再输出本地时间   

#### 第46条：使用内置算法与数据结构

使用 Python 内置的模块来描述各种算法和数据结构

内置算法和数据结构

```python
collections.deque

collections.OrderedDict：OrderedDict 内部维护着一个根据键插入顺序排序的双向链表，要保持Key的顺序

collection.defaultdict： 使用dict时，如果引用的Key不存在，就会抛出KeyError。如果希望key不存在时，返回一个默认值，就可以用defaultdict。

heapq模块操作list（优先队列）：heappush，heappop和nsmallest

	a = []
	heappush(a, 5)
	heappush(a, 3)
	heappush(a, 7)
	heappush(a, 4)
	print(heappop(a), heappop(a), heappop(a), heappop(a))

	# >>>

	# 3 4 5 7

bisect模块：bisect_left可以对有序列表进行高效二分查找

itertools模块（Python2不一定支持）：

	连接迭代器：chain，cycle，tee和zip_longest

	过滤：islice（索引值切割），takewhile，dropwhile，filterfalse

	组合不同迭代器：product（笛卡尔积），permutations（排列），combination（组合）
``` 

#### 第47条：在重视精确度的场合，应该使用decimal

高精度要求的使用 Decimal 处理，如对舍入行为要求很严的场合，

eg: 涉及货币计算的场合 

#### 第48条：学会安装由Python开发者社区所构建的模块 

### 第7章　协作开发 

#### 第49条：为每个函数、类和模块编写文档字符串

为模块撰写文档时，应该介绍本模块的内容，并且要把用户应该了解的重要类及重要函数列出来。

```python
# words.py
# !/usr/bin/env python3
"""Library for test words for various linguistic patterns.
Testing how words relate to each other can be tricky sometimes! This module
provides easy ways to determine when words you've found have special
properties.
Available functions:
- palindrome: Determine if a word is a palindrome.
- check_anagram: Determine if two words are anagrams.
...
"""
# ...
```
	
为类撰写文档时，应该在 class 语句下面的 docstring 中，介绍本类的行为、重要属性，以及本类的子类应该实现的行为。

```python
class Player(object):
	"""Represents a player of the game.
	Subclasses may override the 'tick' method to provide custom animations for
	the player's movement depending on their power level. etc.
	Public attributes:
	- power: Unused power-ups (float between 0 and 1).
	- coins: Coins found during the level (integer).
	"""
	# ...
	pass
```

为函数及方法撰写文档时，应该在 def 语句下面的 docstring 中，介绍函数的每个参数，函数返回值，函数在执行过程中可能抛出的异常，以及其他行为。

```python
def find_anagrms(word, dictionary):
	"""Find all anagrams for a word.
	This function only runs as fast as the test for membership in the
	'dictionary' container. It will be slow if the dictionary is a list and
	fast if it's a set.
	Args:
		word: String of the target word.
		dictionary: Container with all strings that are known to be actual
		words.
	Returns:
		List of anagrams that were found. Empty if none were found.
	"""
	# ...
	pass 
```		

#### 第50条：用包来安排模块，并提供稳固的API 

只要把 __init__.py 文件放入含有其他源文件的目录里，就可以将该目录定义为包。目录中的文件，都将成为包的子模块。该包的目录下面，也可以含有其他包。   
把外界可见的名称，列在名为 __all__ 的特殊属性里，即可为包提供一套明确的 API。    
如果想隐藏某个包的内部实现，可以在包的 __init__.pyw文件中，只把外界可见的那些名称引入进来，或是给权限内部使用的那些名称添加下划线前缀。   
尽量不要使用import *,而是使用from x import y   

#### 第51条：为自编的模块定义根异常，以便将调用者与API相隔离 

#### 第52条：用适当的方式打破循环依赖关系 

如果两个模块必须相互调用对方，才能完成引入操作，那就会出现循环依赖现象，这可能导致程序在启动的时候崩溃。   
打破循环依赖关系的最佳方案，是把导致两个模块相互依赖的那部分代码，重构为单独的模块，并把它挡在依赖树的底部。   
打破循环依赖关系的最简方案，是执行动态的模块引入操作，这样既可以缩减重构所花的精力，也可以尽量降低代码的复杂度   

#### 第53条：用虚拟环境隔离项目，并重建其依赖关系 

python3.7 -m venv 命令可以创建虚拟环境，source bin/activate 命令可以激活虚拟环境， deactivate 命令可以停用虚拟环境。   
pip freeze 命令可以把某套环境所依赖的软件包，汇总到一份文件里面。我们把这个 requirements.txt 文件提供给 pip install -r 命令，即可重建一套与原来环境相仿的新环境。   
如果使用 Python3.4 之前的版本做开发，那么必须单独下载并安装类似的 pyvenv 工具- virtualenv   


### 第8章　部署 

#### 第54条：考虑用模块级别的代码来配置不同的部署环境 

```python
# db_connection.py
import sys

class Win32Database(object):
	#...
	pass

class PosixDatabase(object):
	#...
	pass

if sys.platform.startswith('win32'):
	Database = Win32Database
else:
	Database = PosixDatabase
```

#### 第55条：通过repr字符串来输出调试信息 

针对内置的 Python 类型来调用 repr 函数，会给据该值返回一条可供打印的字符串。把这个 repr 字符串传给内置的 eval 函数，就可以将其还原为初始的那个值。   
可以在类中编写 __repr__ 方法，用自定义的方式来提供一种可供打印的字符串，并在其中给出更为详细的调试信息。   
可以在任意对象的上面查询 __dict__ 属性，以观察其内部信息。

```python

class BetterClass(object):
	def __init__(self, x, y):
		self.x = x
		self.y = y

	def __repr__(self):
		return 'BetterClass(%d, %d)' % (self.x, self.y)


# Now, the repr value is much more useful.

obj = BetterClass(1, 2)
print(obj)
print(repr(obj))
# BetterClass(1, 2)
# BetterClass(1, 2)

class OpaqueClass(object):
	def __init__(self, x, y):
		self.x = x
		self.y = y
		
obj = OpaqueClass(4, 5)
print(obj.__dict__)
# {'y': 5, 'x': 4}
    
```

#### 第56条：用unittest来测试全部代码 

内置的 unittest 模块提供了测试者所需的很多功能，我们可以借助这些机制写出良好的测试。    
我们可以在 TestCase 子类中，为每一个需要测试的行为，定义对应的测试方法。TestCase 子类的测试方法，其名称必须以 test 开头。   
必须同时编写单元测试和集成测试，前者用来独立检验程序中的每个功能，而后者则用来检验模块之间的交互行为。  

```python
from unittest import TestCase, main
from item_56_utils import to_str

def to_str(data):
	if isinstance(data, str):
		return data
	elif isinstance(data, bytes):
		return data.decode('utf-8')
	else:
		raise TypeError('Must supply str or bytes, '
						'found: %r' % data)

class UtilsTestCase(TestCase):
	def test_to_str_str(self):
		self.assertEqual('hello', to_str('hello'))

	def test_to_str_bad(self):
		self.assertRaises(TypeError, to_str, object())

class MyTest(TestCase):
	def setUp(self):
		self.test_dir = TemporaryDirectory()

	def tearDown(self):
		self.test_dir.cleanup()    
		
if __name__ == '__main__':
	main()
```


#### 第57条：考虑用pdb实现交互调试

修改 Python 程序，在想要调试的代码上方直接加入 import pdb; pdb.set_trace() ，以触发互动调试器。    
在 pdb 提示符中输入命令，以便精确地控制程序的执行流程，这些命令（bt up down step next return）使得我们能够交替地查看程序状态并继续向下运行程序。   

```python
import pdb
def foo(num):
	print(f"当前的数字是:{num}")
	for i in range(num):
		pdb.set_trace()
		print(f"当前循环的数字:{i}")


if __name__ == '__main__':
	foo(10) 
```

#### 第58条：先分析性能，然后再优化 

做性能分析时，应该使用 cProfile 模块，而不要使用 profile 模块，因为前者能够给出更为精确的性能分析数据。    
可以通过 Profile 对象的 runcall 方法来分析程序的性能，该方法能够提供性能分析所需的全部信息，它会按照树状的函数调用关系，来单独地统计每个函数的性能。   
可以用 Stats 对象来筛选性能分析数据，并打印出我们所需的那一部分，以便我们据此了解程序的性能。   

cProfile 比 profile更精准：ncalls:调用次数，tottime:函数自身耗时，不包括调用函数的耗时，cumtime:包括调用的函数耗时

```python
def my_utility(a, b):
	if a > b:
		return a + b
	else:
		return a * b


def first_func():
	for _ in range(100000):
		my_utility(4, 5)


def second_func():
	for _ in range(1000):
		my_utility(3, 1)


def my_program():
	for _ in range(20):
		first_func()
		second_func()


# Profiling this code and using the default print_stats output will generate
# output statistics that are confusing.

profiler_my = Profile()
profiler_my.runcall(my_program)
stats_my = Stats(profiler_my)
stats_my.strip_dirs()
stats_my.sort_stats('cumulative')
stats_my.print_stats()
stats_my.print_callers()
```

关于耗时测试：

整体测试cProfile ————> 代码块测试 time.perf_counter()————> 很小的代码片段 timeit 
    
整体时间测试： time python3 someprogram.py

整体测试：

python3 -m cProfile someprogram.py


>>> timeit('math.sqrt(2)', 'import math', number=10000000)
1.434852126003534


time.perf_counter() : 会在给定平台上获取最高精度的计时值，仍然还是基于时钟时间，很多因素会影响到它的精确度，比如机器负载。 

time.process_time()： 测试执行时间 

#### 第59条：用tracemalloc来掌握内存的使用及泄漏情况

可以通过 gc 模块来了解程序中的对象，但是并不能由此看出这些对象究竟是如何分配出来的。

内置的 tracemalloc 模块提供了许多强大的工具，使得我们可以找出导致内存使用增大的根源。



## 2 python_cookbook



### 第1章 数据结构和算法   

#### 1.1 将序列分解为单独的变量 

用于可迭代对象： list tuple，包括字符串，文件对象，迭代器生成器

    >>> data = [ 'ACME', 50, 91.1, (2012, 12, 21) ]
    >>> name, shares, price, date = data

在解包的时候利用 * 操作符来接受多个值

    >>> record = ('Dave', 'dave@example.com', '773-555-1212', '847-555-1212')
    >>> name, email, *phone_numbers = record


#### 1.2 从任意长度的可迭代对象中分解元素 

#### 1.3 保存最后N个元素 

deque 还实现了 maxlen  appendleft appendright popleft popright 等方法。

eg:保留最后 N 个元素 

#### 1.4 找到最大或最小的N个元素 

nlargest() 和 nsmallest() 最大最小元素

```python
import heapq
nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
print(heapq.nlargest(3, nums)) # Prints [42, 37, 23]
print(heapq.nsmallest(3, nums)) # Prints [-4, 1, 2]
```
	
heapq.heappush() 和 heapq.heappop()分别在队列 _queue 上插入和删除第一个元素，heappop() 函数总是返回”最小的”的元素
		
#### 1.5 实现优先级队列

```python 
import heapq

class PriorityQueue:
	def __init__(self):
		self._queue = []
		self._index = 0

	def push(self, item, priority):
		heapq.heappush(self._queue, (-priority, self._index, item))
		self._index += 1

	def pop(self):
		return heapq.heappop(self._queue)[-1]

```			

#### 1.6 在字典中将键映射到多个值上 

defaultdict：是它会自动初始化每个 key 刚开始对应的值

eg:一键多值字典

OrderedDict：有序字典

eg:字典交并集

```python
a = {
'x' : 1,
'y' : 2,
'z' : 3
}
b = {
'w' : 10,
'x' : 11,
'y' : 2
}
# Find keys in common
a.keys() & b.keys() # { 'x', 'y' }
# Find keys in a that are not in b
a.keys() - b.keys() # { 'z' }
# Find (key,value) pairs in common
a.items() & b.items() # { ('y', 2) }
```

字典推导式：

p1 = {key: value for key, value in prices.items() if value > 200}

字典合并：

```python
ChainMap 

from collections import ChainMap
c = ChainMap(a,b)
```

#### 1.7 让字典保持有序 

#### 1.8 与字典有关的计算问题 

Counter 对象可以接受任意的由可哈希(hashable)元素构成的序列对象。 在底层实现上，一个 Counter 对象就是一个字典，将元素映射到它出现的次数。

most_common()、update()
 
可以跟数学运算操作或者说是集合操作相结合

```python
>>> a = Counter(words)
>>> b = Counter(morewords)
>>> # Combine counts
>>> c = a + b
```

#### 1.9 在两个字典中寻找相同点 

#### 1.10 从序列中移除重复项且保持元素间顺序不变

#### 1.11 对切片命名 

#### 1.12 找出序列中出现次数最多的元素 

#### 1.13 通过公共键对字典列表排序 

内置的 sorted() 函数有一个关键字参数 key ，可以传入一个 callable 对象给它

类对象一种方法是使用 operator.attrgetter() 另一种则是使用 lambda 函数

```python
>>> from operator import attrgetter
>>> sorted(users, key=attrgetter('user_id'))

def sort_notcompare():
	users = [User(23), User(3), User(99)]
	print(sorted(users, key=lambda u: u.user_id))
```

#### 1.14 对不原生支持比较操作的对象排序

#### 1.15 根据字段将记录分组 

```python
from operator import itemgetter
from itertools import groupby

# Sort by the desired field first
rows.sort(key=itemgetter('date'))
# Iterate in groups
for date, items in groupby(rows, key=itemgetter('date')):
	print(date)
	for i in items:
		print(' ', i)
```

#### 1.16 筛选序列中的元素 

#### 1.17 从字典中提取子集 

排列组合：product()

permutations()

combinations()

#### 1.18 将名称映射到序列的元素中 

#### 1.19 同时对数据做转换和换算

#### 1.20 将多个映射合并为单个映射 

### 第2章 字符串和文本 

#### 2.1 针对任意多的分隔符拆分字符串

string 的 split() ：简单的字符串分割

re.split()：灵活的切割字符串

两种模式：str、re

#### 2.2 在字符串的开头或结尾处做文本匹配

str.startswith() 、str.endswith()

检查多种匹配可能，只需要将所有的匹配项放入到一个元组

```python
>>> filenames
[ 'Makefile', 'foo.c', 'bar.py', 'spam.c', 'spam.h' ]
>>> [name for name in filenames if name.endswith(('.c', '.h')) ]
['foo.c', 'spam.c', 'spam.h']
```

#### 2.3 利用Shell通配符做字符串匹配

fnmatch, fnmatchcase, glob  

```python
>>> fnmatch('foo.txt', '*.txt')
True
>>> fnmatch('foo.txt', '?oo.txt')
True
>>> fnmatch('Dat45.csv', 'Dat[0-9]*')
True
>>> names = ['Dat1.csv', 'Dat2.csv', 'config.ini', 'foo.py']
>>> [name for name in names if fnmatch(name, 'Dat*.csv')]
['Dat1.csv', 'Dat2.csv']
>>>    
```

#### 2.4 文本模式的匹配和查找

用re模块进行匹配和搜索文本的最基本方法。 核心步骤就是先使用 re.compile() 编译正则表达式字符串， 然后使用 match() , findall() 或者 finditer() 等方法

#### 2.5 查找和替换文本 

简单: str.replace()

```python
    >>> text = 'yeah, but no, but yeah, but no, but yeah'
    >>> text.replace('yeah', 'yep')
    'yep, but no, but yep, but no, but yep'
```

复杂：re.sub, re.subn

```python
    >>> import re
    >>> datepat = re.compile(r'(\d+)/(\d+)/(\d+)')
    >>> datepat.sub(r'\3-\1-\2', text)
```
	
文本查找和替换时不区分大小写：re.IGNORECASE

```python
    >>> text = 'UPPER PYTHON, lower python, Mixed Python'
    >>> re.findall('python', text, flags=re.IGNORECASE)
    ['PYTHON', 'python', 'Python']
    >>> re.sub('python', 'snake', text, flags=re.IGNORECASE)
```

#### 2.6 以不区分大小写的方式对文本做查找和替换

#### 2.7 定义实现最短匹配的正则表达式

**贪婪匹配.*和懒惰匹配.*?**

* 或者 + 这样的操作符后面添加一个 ? 可以强制匹配算法改成寻找最短的可能匹配

re.DOTALL可以让.匹配换行符,多行匹配

#### 2.8 编写多行模式的正则表达式

#### 2.9 将Unicode文本统一表示为规范形式

Unicode文本标准化：

```python
    >>> import unicodedata
    >>> t1 = unicodedata.normalize('NFC', s1)
    >>> t3 = unicodedata.normalize('NFD', s1)
```
	
#### 2.10 用正则表达式处理Unicode字符

#### 2.11 从字符串中去掉不需要的字符

strip() 方法能用于删除开始或结尾的字符。 lstrip() 和 rstrip() 分别从左和从右执行删除操作

或者：replace sub

```python
>>> s.replace(' ', '')
'helloworld'
>>> import re
>>> re.sub('\s+', ' ', s)
'hello world'
>>>
```

#### 2.12 文本过滤和清理

#### 2.13 对齐文本字符串

基本对齐：ljst, rjust, center

```python
>>> text = 'Hello World'
>>> text.ljust(20)
'Hello World         '
>>> text.rjust(20)
'         Hello World'
>>> text.center(20)
'    Hello World     '
>>>
```

format的^, >, <：使用 <,> 或者 ^ 字符后面紧跟一个指定的宽度，格式化多个值

```python
>>> '{:>10s} {:>10s}'.format('Hello', 'World')
'     Hello      World'
>>>
```

#### 2.14 字符串连接及合并

方式：join，+，format

```python
>>> parts = ['Is', 'Chicago', 'Not', 'Chicago?']
>>> ' '.join(parts)

>>> a = 'Is Chicago'
>>> b = 'Not Chicago?'
>>> a + ' ' + b

>>> print('{} {}'.format(a,b))
Is Chicago Not Chicago?
>>> print(a + ' ' + b)
Is Chicago Not Chicago?
>>>
```	

#### 2.15 给字符串中的变量名做插值处理

#### 2.16 以固定的列数重新格式化文本

#### 2.17 在文本中处理HTML和XML实体

#### 2.18 文本分词

#### 2.19 编写一个简单的递归下降解析器

#### 2.20 在字节串上执行文本操作

### 第3章 数字、日期和时间   

#### 3.1 对数值进行取整

#### 3.2 执行精确的小数计算

#### 3.3 对数值做格式化输出 

#### 3.4 同二进制、八进制和十六进制数打交道

#### 3.5 从字节串中打包和解包大整数

#### 3.6 复数运算

#### 3.7 处理无穷大和NaN 

#### 3.8 分数的计算

#### 3.9 处理大型数组的计算 

#### 3.10 矩阵和线性代数的计算 

#### 3.11 随机选择 

#### 3.12 时间换算 

#### 3.13 计算上周5的日期 

#### 3.14 找出当月的日期范围 

#### 3.15 将字符串转换为日期 

#### 3.16 处理涉及到时区的日期问题 

### 第4章 迭代器和生成器 

#### 4.1 手动访问迭代器中的元素 

手动对访问迭代器：next():

```python
def manual_iter():
	with open('/etc/passwd') as f:
		try:
			while True:
				line = next(f)
				print(line, end='')
		except StopIteration:
			pass
```

#### 4.2 委托迭代 

#### 4.3 用生成器创建新的迭代模式 

#### 4.4 实现迭代协议

迭代器协议是指：对象必须提供一个next方法，执行方法要么返回迭代器中的下一项，要么就引起一个StopIteration异常，以终止迭代

对象必须提供一个next方法和iter方法。作为迭代器协议应用的代表就是for方法 

#### 4.5 反向迭代 

#### 4.6 定义带有额外状态的生成器函数 

#### 4.7 对迭代器做切片操作 

itertools.islice()  正好适用于在迭代器和生成器上做切片操作

itertools.dropwhile() 跳过可迭代对象的开始部分

    >>> from itertools import dropwhile
    >>> with open('/etc/passwd') as f:
    ...     for line in dropwhile(lambda line: not line.startswith('#'), f):
    ...         print(line, end='')

排列组合问题：itertools.permutations()  排列

itertools.combinations() 组合， itertools.combinations_with_replacement() 允许同一个元素被选择多次

```python
    >>> items = ['a', 'b', 'c']
    >>> from itertools import permutations
    >>> for p in permutations(items):
    ...     print(p)

    >>> for c in combinations(items, 3):
    ...     print(c)
```    

#### 4.8 跳过可迭代对象中的前一部分元素 

#### 4.9 迭代所有可能的组合或排列 

#### 4.10 以索引-值对的形式迭代序列 

enumerate()： 索引-值对迭代，代替for

    >>> my_list = ['a', 'b', 'c']
    >>> for idx, val in enumerate(my_list, 1):
    ...     print(idx, val)

#### 4.11 同时迭代多个序列 


zip()，itertools.zip_longest()：同时迭代多个容器

    >>> for x, y in zip(xpts, ypts):
    ...     print(x,y)

itertools.chain()：依次迭代多个容器，不会产生新序列，省空间

    >>> a = [1, 2, 3, 4]
    >>> b = ['x', 'y', 'z']
    >>> for x in chain(a, b):
    ... print(x)

#### 4.12 在不同的容器中进行迭代 

heapq.merge()：对不同类型的容器进行排序后依次迭代

    >>> import heapq
    >>> a = [1, 4, 7, 10]
    >>> b = [2, 5, 6, 11]
    >>> for c in heapq.merge(a, b):
    ...     print(c)

#### 4.13 创建处理数据的管道 

#### 4.14 扁平化处理嵌套型的序列 

#### 4.15 合并多个有序序列，再对整个有序序列进行迭代 

#### 4.16 用迭代器取代while循环 

### 第5章 文件和I/O 

#### 5.1 读写文本数据 

open读写文件，

模式: wt rt  at ，写，读，追加

encoding：UTF-8  ，sys.getdefaultencoding()获取系统默认

newline

```python
with open('somefile.txt', 'wt') as f:
    f.write(text1)
    f.write(text2)
    
with open('somefile.txt', 'rt', encoding='latin-1') as f:
```

open() 函数中使用 x 模式来代替 w 模式：文件不存在才能写入

#### 5.2 将输出重定向到文件中 

#### 5.3 以不同的分隔符或行结尾符完成打印 

#### 5.4 读写二进制数据 

#### 5.5 对已不存在的文件执行写入操作 

#### 5.6 在字符串上执行I/O操作 

#### 5.7 读写压缩的数据文件 

gzip.open()和 bz2.open() 接受跟内置的 open() 函数一样的参数， 包括 encoding，errors，newline，compresslevel 指定压缩级别。

```python
import gzip
with gzip.open('somefile.gz', 'rt') as f:
	text = f.read()

# bz2 compression
import bz2
with bz2.open('somefile.bz2', 'rt') as f:
	text = f.read()
```

#### 5.8 对固定大小的记录进行迭代 

#### 5.9 将二进制数据读取到可变缓冲区中 

#### 5.10 对二进制文件做内存映射 

mmap 将文件映射到内存中

```python
import os
import mmap

def memory_map(filename, access=mmap.ACCESS_WRITE):
	size = os.path.getsize(filename)
	fd = os.open(filename, os.O_RDWR)
	return mmap.mmap(fd, size, access=access)
	
>>> m = memory_map('data')
>>> len(m)
```


#### 5.11 处理路径名 

#### 5.12 检测文件是否存在

文件路径名 ：basename，dirname，expanduser，splitext     
判存在：exists，isfile，isdir，islink，realpath，getsize，getmtime   
文件元数据：大小和日期   
文件列表获取：os.path.listdir   

#### 5.13 获取目录内容的列表 

#### 5.14 绕过文件名编码 

#### 5.15 打印无法解码的文件名 

#### 5.16 为已经打开的文件添加或修改编码方式 

#### 5.17 将字节数据写入文本文件 

#### 5.18 将已有的文件描述符包装为文件对象 

#### 5.19 创建临时文件和目录 

#### 5.20 同串口进行通信 

#### 5.21 序列化Python对象 

### 第6章 数据编码与处理   

#### 6.1 读写CSV数据 

#### 6.2 读写JSON数据 

#### 6.3 解析简单的XML文档 

#### 6.4 以增量方式解析大型XML文件 

#### 6.5 将字典转换为XML 

#### 6.6 解析、修改和重写XML 

#### 6.7 用命名空间来解析XML文档 

#### 6.8 同关系型数据库进行交互 

#### 6.9 编码和解码十六进制数字 

#### 6.10 Base64编码和解码 

#### 6.11 读写二进制结构的数组 

#### 6.12 读取嵌套型和大小可变的二进制结构 

6.13 数据汇总和统计 

### 第7章 函数   

#### 7.1 编写可接受任意数量参数的函数 

*args：任意数量的位置参数

**kwargs：任意数量的关键字参数 

默认参数：

    def spam(a, b=42):
    print(a, b)

    spam(1) # Ok. a=1, b=42
    spam(1, 2) # Ok. a=1, b=2

#### 7.2 编写只接受关键字参数的函数 

#### 7.3 将元数据信息附加到函数参数上 

def fun(x: int, y: int) -> int

#### 7.4 从函数中返回多个值

函数返回多个值：即返回元组 

#### 7.5 定义带有默认参数的函数 

#### 7.6 定义匿名或内联函数 

**匿名或内联函数 lambda**

```python
>>> add = lambda x, y: x + y
>>> add(2,3)
5
>>> add('hello', 'world')
'helloworld'
>>>
```

lambda表达式典型的使用场景是排序或数据reduce等：

```python
>>> names = ['David Beazley', 'Brian Jones',
...         'Raymond Hettinger', 'Ned Batchelder']
>>> sorted(names, key=lambda name: name.split()[-1].lower())
['Ned Batchelder', 'David Beazley', 'Raymond Hettinger', 'Brian Jones']
```

#### 7.7 在匿名函数中绑定变量的值 

#### 7.8 让带有N个参数的可调用对象以较少的参数形式调用 

functools.partial()：冻结参数，为某些函数提供默认值

```python
def spam(a, b, c, d):
	print(a, b, c, d)
	
>>> from functools import partial
>>> s1 = partial(spam, 1) # a = 1
>>> s1(2, 3, 4)
1 2 3 4
```

#### 7.9 用函数替代只有单个方法的类 

#### 7.10 在回调函数中携带额外的状态 

#### 7.11 内联回调函数 

#### 7.12 访问定义在闭包内的变量 

闭包函数：在一个内部函数中，对外部作用域的变量进行引用，(并且一般外部函数的返回值为内部函数)，那么内部函数就被认为是闭包

```python
def urltemplate(template):
	def opener(**kwargs):
		return urlopen(template.format_map(kwargs))
	return opener

# Example use
yahoo = urltemplate('http://finance.yahoo.com/d/quotes.csv?s={names}&f={fields}')
for line in yahoo(names='IBM,AAPL,FB', fields='sl1c1v'):
	print(line.decode('utf-8'))
```


### 第8章 类与对象   

#### 8.1 修改实例的字符串表示 

#### 8.2 自定义字符串的输出格式 

#### 8.3 让对象支持上下文管理协议 

#### 8.4 当创建大量实例时如何节省内存 

#### 8.5 将名称封装到类中 

#### 8.6 创建可管理的属性 

#### 8.7 调用父类中的方法 

#### 8.8 在子类中扩展属性 

#### 8.9 创建一种新形式的类属性或实例属性 

#### 8.10 让属性具有惰性求值的能力 

#### 8.11 简化数据结构的初始化过程 

#### 8.12 定义一个接口或抽象基类 

#### 8.13 实现一种数据模型或类型系统 

#### 8.14 实现自定义的容器 

#### 8.15 委托属性的访问 

#### 8.16 在类中定义多个构造函数 

#### 8.17 不通过调用init来创建实例 

#### 8.18 用Mixin技术来扩展类定义 

#### 8.19 实现带有状态的对象或状态机 

#### 8.20 调用对象上的方法，方法名以字符串形式给出 

#### 8.21 实现访问者模式 

#### 8.22 实现非递归的访问者模式 

#### 8.23 在环状数据结构中管理内存 

#### 8.24 让类支持比较操作 

#### 8.25 创建缓存实例 

### 第9章 元编程   

#### 9.1 给函数添加一个包装 

#### 9.2 编写装饰器时如何保存函数的元数据 

#### 9.3 对装饰器进行解包装 

#### 9.4 定义一个可接受参数的装饰器 

#### 9.5 定义一个属性可由用户修改的装饰器 

#### 9.6 定义一个能接收可选参数的装饰器 

#### 9.7 利用装饰器对函数参数强制执行类型检查 

#### 9.8 在类中定义装饰器 

#### 9.9 把装饰器定义成类 

#### 9.10 把装饰器作用到类和静态方法上 

#### 9.11 编写装饰器为被包装的函数添加参数 

#### 9.12 利用装饰器给类定义打补丁 

#### 9.13 利用元类来控制实例的创建 

#### 9.14 获取类属性的定义顺序 

#### 9.15 定义一个能接受可选参数的元类 

#### 9.16 在*args和**kwargs上强制规定一种参数签名 

#### 9.17 在类中强制规定编码约定 

#### 9.18 通过编程的方式来定义类 

#### 9.19 在定义的时候初始化类成员 

#### 9.20 通过函数注解来实现方法重载 

#### 9.21 避免出现重复的属性方法 

#### 9.22 以简单的方式定义上下文管理器 

#### 9.23 执行带有局部副作用的代码 

#### 9.24 解析并分析Python源代码 

#### 9.25 将Python源码分解为字节码 

### 第10章 模块和包  

#### 10.1 把模块按层次结构组织成包 

#### 10.2 对所有符号的导入进行精确控制 

#### 10.3 用相对名称来导入包中的子模块 

#### 10.4 将模块分解成多个文件 

#### 10.5 让各个目录下的代码在统一的命名空间下导入 

#### 10.6 重新加载模块 

#### 10.7 让目录或zip文件成为可运行的脚本 

#### 10.8 读取包中的数据文件 

#### 10.9 添加目录到sys.path中 

#### 10.10 使用字符串中给定的名称来导入模块 

#### 10.11 利用import钩子从远端机器上加载模块 

#### 10.12 在模块加载时为其打补丁 

#### 10.13 安装只为自己所用的包 

#### 10.14 创建新的Python环境 

#### 10.15 发布自定义的包 

### 第11章 网络和Web编程  

#### 11.1 以客户端的形式同HTTP服务交互 

#### 11.2 创建一个TCP服务器 

#### 11.3 创建一个UDP服务器 

#### 11.4 从CIDR地址中生成IP地址的范围 

#### 11.5 创建基于REST风格的简单接口 

#### 11.6 利用XML-RPC实现简单的远端过程调用 

#### 11.7 在不同的解释器间进行通信 

#### 11.8 实现远端过程调用 

#### 11.9 以简单的方式验证客户端身份 

#### 11.10 为网络服务增加SSL支持 

#### 11.11 在进程间传递socket文件描述符 

#### 11.12 理解事件驱动型I/O 

#### 11.13 发送和接收大型数组 

### 第12章 并发 

#### 12.1 启动和停止线程 

#### 12.2 判断线程是否已经启动 

#### 12.3 线程间通信 

#### 12.4 对临界区加锁 

#### 12.5 避免死锁 

#### 12.6 保存线程专有状态 

#### 12.7 创建线程池 

#### 12.8 实现简单的并行编程 

#### 12.9 如何规避GIL带来的限制 

#### 12.10 定义一个Actor任务 

#### 12.11 实现发布者/订阅者消息模式 

#### 12.12 使用生成器作为线程的替代方案 

#### 12.13 轮询多个线程队列 

#### 12.14 在UNIX上加载守护进程 

### 第13章 实用脚本和系统管理   

#### 13.1 通过重定向、管道或输入文件来作为脚本的输入 

```python
import argparse
parser = argparse.ArgumentParser(description='Search some files')

parser.add_argument(dest='filenames',metavar='filename', nargs='*')

parser.add_argument('-p', '--pat',metavar='pattern', required=True,
					dest='patterns', action='append',
					help='text pattern to search for')

parser.add_argument('-v', dest='verbose', action='store_true',
					help='verbose mode')

```

创建一个 ArgumentParser 实例

dest: 指定解析结果被指派给属性的名字。    
metavar:被用来生成帮助信息。   
action:指定跟属性对应的处理逻辑， 通常的值为 store   
required: 表示该参数至少要有一个   

#### 13.2 终止程序并显示错误信息 

#### 13.3 解析命令行选项 

#### 13.4 在运行时提供密码输入提示 

#### 13.5 获取终端大小 

#### 13.6 执行外部命令并获取输出 

getpass.getpass()：获取密码

os.get_terminal_size()：获取终端大小

subprocess.check_output()： 执行一个外部命令并以Python字符串的形式获取执行结果

```python
import subprocess
out_bytes = subprocess.check_output(['netstat','-a'])
```

subprocess.Popen：复杂交互

#### 13.7 拷贝或移动文件和目录

拷贝移动：copy，copy2，copytree，move

shutil.copytree(src, dst, ignore=ignore_pyc_files)    
shutil.copytree(src, dst, ignore=shutil.ignore_patterns('*~', '*.pyc'))   
shutil.copytree(src, dst, ignore=ignore_pyc_files)   
 
#### 13.8 创建和解包归档文件 

创建和解压归档: make_archive() 、unpack_archive()、tarfile/zipfile/gzip/bz2

```python
>>> import shutil
>>> shutil.unpack_archive('Python-3.3.0.tgz')

>>> shutil.make_archive('py33','zip','Python-3.3.0')
```

#### 13.9 通过名称来查找文件 

**os.walk()**

查找文件：遍历目录树， 每次进入一个目录，它会返回一个三元组，包含相对于查找目录的相对路径，一个该目录下的目录名列表， 以及那个目录下面的文件名列表

```python
def modified_within(top, seconds):
	now = time.time()
	for path, dirs, files in os.walk(top):
		for name in files:
			fullpath = os.path.join(path, name)
			if os.path.exists(fullpath):
				mtime = os.path.getmtime(fullpath)
				if mtime > (now - seconds):
					print(fullpath)
```
 
os.path.abspath()：返回绝对路径    
os.path.normpath()：返回正常路径   

#### 13.10 读取配置文件   
读取配置文件

```python
>>> from configparser import ConfigParser
>>> cfg = ConfigParser()
>>> cfg.read('config.ini')

>>> cfg.set('debug','log_errors','False')
>>> import sys
>>> cfg.write(sys.stdout)
```

#### 13.11 给脚本添加日志记录 

**logging**

```python
basicConfig，config.fileConfig，

logging.basicConfig(
	filename='app.log',
	level=logging.ERROR
)
    
使用配置文件：  

# Configure the logging system
logging.config.fileConfig('logconfig.ini')

```	
	   
程序中修改配置：logging.getLogger().level = logging.DEBUG

给函数库增加日志功能

#### 13.12 给库添加日志记录 

#### 13.13 创建一个秒表计时器 

#### 13.14 给内存和CPU使用量设定限制 

#### 13.15 加载Web浏览器 

### 第14章 测试、调试以及异常 

#### 14.1 测试发送到stdout上的输出 

**unittest**

unittest 模块首先会组装一个测试套件。 这个测试套件包含了你定义的各种方法。


skip() 装饰器能被用来忽略某个你不想运行的测试。 

skipIf() 和 skipUnless()

#### 14.2 在单元测试中为对象打补丁 

#### 14.3 在单元测试中检测异常情况   

**warning.warn**

```python

//灵活使用
try:
   
except:

else:
```

#### 14.4 将测试结果作为日志记录到文件中 

#### 14.5 跳过测试，或者预计测试结果为失败 

#### 14.6 处理多个异常 

#### 14.7 捕获所有的异常 

#### 14.8 创建自定义的异常 

#### 14.9 通过引发异常来响应另一个异常 

#### 14.10 重新抛出上一个异常 

#### 14.11 发出告警信息 

#### 14.12 对基本的程序崩溃问题进行调试 

#### 14.13 对程序做性能分析以及计时统计 

#### 14.14 让你的程序运行得更快 

使用函数：使用局部变量快

尽量减少属性访问

频繁访问的变量改为局部变量

减少装饰器属性描述符： 避免装饰器、属性访问、描述器去包装你的代码

用内建容器：内置的数据类型比如字符串、元组、列表、集合和字典都是使用C来实现的

### 第15章 C语言扩展   

#### 15.1 利用ctypes来访问C代码 

#### 15.2 编写简单的C语言扩展模块 

#### 15.3 编写一个可操作数组的扩展函数 

#### 15.4 在C扩展模块中管理不透明指针 

#### 15.5 在扩展模块中定义并导出C API 

#### 15.6 从C中调用Python 

#### 15.7 在C扩展模块中释放GIL 

#### 15.8 混合使用C和Python环境中的线程 

#### 15.9 用Swig来包装C代码 

#### 15.10 用Cython来包装C代码 

#### 15.11 用Cython来高效操作数组 

#### 15.12 把函数指针转换为可调用对象 

#### 15.13 把以NULL结尾的字符串传给C库 

#### 15.14 把Unicode字符串传递给C库 

#### 15.15 把C字符串转换到Python中 

#### 15.16 同编码方式不确定的C字符串打交道 

#### 15.17 把文件名传给C扩展模块 

#### 15.18 把打开的文件传给C扩展模块 

#### 15.19 在C中读取文件型对象 

#### 15.20 从C中访问可迭代对象 

#### 15.21 排查段错误
