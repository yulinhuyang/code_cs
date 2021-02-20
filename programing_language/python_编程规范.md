1    effcitive  python

**pep8**

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

**迭代器：enumerate zip**

enumerate 比range更好用:

enumerate提供了简洁的语法，再循环迭代一个迭代器的同时既能获取下标，也能获取当前值。

Prefer：

    for i, flavor in enumerate(flavor_list):

        print(‘%d: %s’ % (i + 1, flavor)) 

用 zip 函数来同时遍历两个迭代器:

内置的zip函数可以并行的对多个迭代器进行处理;如果所处理的迭代器的长度不一致时，zip会默认截断输出.内置模块itertools中的zip_longest函数可以并行地处理多个迭代器，而可以无视长度不一致的问题。

Prefer:

    names = [‘Cecilia’, ‘Lise’, ‘Marie’]

    max_letters = 0

    letters = [len(n) for n in names]

    for name, count in zip(names, letters):

....

**生成器**

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
    
    
**闭包中是怎样使用外围作用域变量**

global 关键字用于使用全局变量，nonlocal 关键字用于使用局部变量(函数内)

Python编译器变量查找域的顺序：

- 当前函数的作用域

- 任何其他的封闭域（比如其他的包含着的函数）。

- 包含该段代码的模块域（也称之为全局域）

- 内置域（包含了像len,str等函数的域）


**参数**

位置参数： 使用*args定义语句，函数可以接收可变数量的位置参数

关键字参数：可选的关键字参数应该优于位置参数

关键字参数的好处:代码可读性的提高，以在定义的时候初始化一个默认值，在前面的调用方式不变的情况下可以很好的拓展函数的参数，不用修改太多的代码

默认参数：使用None和文档说明动态的指定默认参数

使用None作为关键字参数的默认值会有一个动态值

