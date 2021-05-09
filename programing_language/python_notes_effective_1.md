
### 第七章 函数

**参数**

*args：任意数量的位置参数

**kwargs：任意数量的关键字参数 

默认参数：

    def spam(a, b=42):
    print(a, b)

    spam(1) # Ok. a=1, b=42
    spam(1, 2) # Ok. a=1, b=2

**注释**

def fun(x: int, y: int) -> int

**函数返回多个值：即返回元组**

**匿名或内联函数 lambda**

    >>> add = lambda x, y: x + y
    >>> add(2,3)
    5
    >>> add('hello', 'world')
    'helloworld'
    >>>

lambda表达式典型的使用场景是排序或数据reduce等：

    >>> names = ['David Beazley', 'Brian Jones',
    ...         'Raymond Hettinger', 'Ned Batchelder']
    >>> sorted(names, key=lambda name: name.split()[-1].lower())
    ['Ned Batchelder', 'David Beazley', 'Raymond Hettinger', 'Brian Jones']

**冻结参数**

functools.partial()：冻结参数，为某些函数提供默认值

    def spam(a, b, c, d):
        print(a, b, c, d)
        
    >>> from functools import partial
    >>> s1 = partial(spam, 1) # a = 1
    >>> s1(2, 3, 4)
    1 2 3 4

**闭包**

闭包函数：在一个内部函数中，对外部作用域的变量进行引用，(并且一般外部函数的返回值为内部函数)，那么内部函数就被认为是闭包

    def urltemplate(template):
        def opener(**kwargs):
            return urlopen(template.format_map(kwargs))
        return opener

    # Example use
    yahoo = urltemplate('http://finance.yahoo.com/d/quotes.csv?s={names}&f={fields}')
    for line in yahoo(names='IBM,AAPL,FB', fields='sl1c1v'):
        print(line.decode('utf-8'))

**内联回调**

### 第八章 类与对象


### 第十三章 脚本编程与系统管理

**argparse **

    import argparse
    parser = argparse.ArgumentParser(description='Search some files')

    parser.add_argument(dest='filenames',metavar='filename', nargs='*')

    parser.add_argument('-p', '--pat',metavar='pattern', required=True,
                        dest='patterns', action='append',
                        help='text pattern to search for')

    parser.add_argument('-v', dest='verbose', action='store_true',
                        help='verbose mode')


创建一个 ArgumentParser 实例

dest: 指定解析结果被指派给属性的名字。 

metavar:被用来生成帮助信息。

action:指定跟属性对应的处理逻辑， 通常的值为 store

required: 表示该参数至少要有一个

**获取外部**

getpass.getpass()：获取密码

os.get_terminal_size()：获取终端大小

subprocess.check_output()： 执行一个外部命令并以Python字符串的形式获取执行结果

    import subprocess
    out_bytes = subprocess.check_output(['netstat','-a'])

subprocess.Popen：复杂交互

**shutil**

拷贝移动：copy，copy2，copytree，move

    shutil.copytree(src, dst, ignore=ignore_pyc_files)

    shutil.copytree(src, dst, ignore=shutil.ignore_patterns('*~', '*.pyc'))

    shutil.copytree(src, dst, ignore=ignore_pyc_files)

创建和解压归档: make_archive() 、unpack_archive()、tarfile/zipfile/gzip/bz2

    >>> import shutil
    >>> shutil.unpack_archive('Python-3.3.0.tgz')

    >>> shutil.make_archive('py33','zip','Python-3.3.0')


**os.walk()**

查找文件：遍历目录树， 每次进入一个目录，它会返回一个三元组，包含相对于查找目录的相对路径，一个该目录下的目录名列表， 以及那个目录下面的文件名列表

    def modified_within(top, seconds):
        now = time.time()
        for path, dirs, files in os.walk(top):
            for name in files:
                fullpath = os.path.join(path, name)
                if os.path.exists(fullpath):
                    mtime = os.path.getmtime(fullpath)
                    if mtime > (now - seconds):
                        print(fullpath)

 
os.path.abspath()：返回绝对路径

os.path.normpath()：返回正常路径

**configoarser**

读取配置文件

    >>> from configparser import ConfigParser
    >>> cfg = ConfigParser()
    >>> cfg.read('config.ini')

    >>> cfg.set('debug','log_errors','False')
    >>> import sys
    >>> cfg.write(sys.stdout)

**logging**

basicConfig，config.fileConfig，

    logging.basicConfig(
        filename='app.log',
        level=logging.ERROR
    )
    
使用配置文件：  

    # Configure the logging system
    logging.config.fileConfig('logconfig.ini')
       
程序中修改配置：logging.getLogger().level = logging.DEBUG


给函数库增加日志功能


### 第十四章 测试、调试和异常

**unittest**

unittest 模块首先会组装一个测试套件。 这个测试套件包含了你定义的各种方法。


skip() 装饰器能被用来忽略某个你不想运行的测试。 

skipIf() 和 skipUnless()

**warning.warn**

灵活使用

    try:

    except:

    else:

处理异常

**程序优化**

使用函数：使用局部变量快

尽量减少属性访问

频繁访问的变量改为局部变量

减少装饰器属性描述符： 避免装饰器、属性访问、描述器去包装你的代码

用内建容器：内置的数据类型比如字符串、元组、列表、集合和字典都是使用C来实现的
