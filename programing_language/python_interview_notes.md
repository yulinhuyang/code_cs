https://github.com/taizilongxu/interview_python

### 1 Python是如何进行内存管理的

**1 对象的引用计数机制**

Python内部使用引用计数，来保持追踪内存中的对象，所有对象都有引用计数。

引用计数增加的情况：

1，一个对象分配一个新名称

2，将其放入一个容器中（如列表、元组或字典）

引用计数减少的情况：

1，使用del语句对对象别名显示的销毁

2，引用超出作用域或被重新赋值

sys.getrefcount( )函数可以获得对象的当前引用计数

多数情况下，引用计数比你猜测得要大得多。对于不可变数据（如数字和字符串），解释器会在程序的不同部分共享内存，以便节约内存。

**2 垃圾回收**

1，当一个对象的引用计数归零时，它将被垃圾收集机制处理掉。

2，当两个对象a和b相互引用时，del语句可以减少a和b的引用计数，并销毁用于引用底层对象的名称。然而由于每个对象都包含一个对其他对象的应用，因此引用计数不会归零，对象也不会销毁。（从而导致内存泄露）。为解决这一问题，解释器会定期执行一个循环检测器，搜索不可访问对象的循环并删除它们。

**3 内存池机制**

Python提供了对内存的垃圾收集机制，但是它将不用的内存放到内存池而不是返回给操作系统。

1，Pymalloc机制。为了加速Python的执行效率，Python引入了一个内存池机制，用于管理对小块内存的申请和释放。

2，Python中所有小于256个字节的对象都使用pymalloc实现的分配器，而大的对象则使用系统的malloc。

3，对于Python对象，如整数，浮点数和List，都有其独立的私有内存池，对象间不共享他们的内存池。也就是说如果你分配又释放了大量的整数，用于缓存这些整数的内存就不能再分配给浮点数。

### 2 Python里面如何拷贝一个对象？（赋值，浅拷贝，深拷贝的区别

赋值（=），就是创建了对象的一个新的引用，修改其中任意一个变量都会影响到另一个。
 
浅拷贝：创建一个新的对象，但它包含的是对原始对象中包含项的引用（如果用引用的方式修改其中一个对象，另外一个也会修改改变）{1,完全切片方法；2，工厂函数，如list()；3，copy模块的copy()函数}

深拷贝：创建一个新的对象，并且递归的复制它所包含的对象（修改其中一个，另外一个不会改变）{copy模块的deep.deepcopy()函数}

### 3 如何用Python来进行查询和替换一个文本字符串？

可以使用re模块中的sub()函数或者subn()函数来进行查询和替换，

sub(replacement, string[,count=0])（replacement是被替换成的文本，string是需要被替换的文本，count是一个可选参数，指最大被替换的数量）

subn()方法执行的效果跟sub()一样，不过它会返回一个二维数组，包括替换后的新的字符串和总共替换的数量

```python
import re
p = re.compile(’(blue|white|red)’)
print(p.sub(’colour’,'blue socks and red shoes’))
->colour socks and colourshoes
```
### 4  Python里面match()和search()的区别？

re模块中match(pattern,string[,flags]),检查string的开头是否与pattern匹配。

re模块中research(pattern,string[,flags]),在string搜索pattern的第一个匹配值

print(re.match(‘super’, ‘superstition’).span()):  (0, 5)

### 5 Python如何实现单例模式？其他23种设计模式python如何实现？
```python
#使用__metaclass__（元类）的高级python用法  
class Singleton2(type):  
    def __init__(cls, name, bases, dict):  
        super(Singleton2, cls).__init__(name, bases, dict)  
        cls._instance = None  
    def __call__(cls, *args, **kw):  
        if cls._instance is None:  
            cls._instance = super(Singleton2, cls).__call__(*args, **kw)  
        return cls._instance  
 
class MyClass3(object):  
    __metaclass__ = Singleton2  
 
one = MyClass3()  
two = MyClass3()  

#使用装饰器(decorator),  
#这是一种更pythonic,更elegant的方法,  
#单例类本身根本不知道自己是单例的,因为他本身(自己的代码)并不是单例的  
def singleton(cls, *args, **kw):  
    instances = {}  
    def _singleton():  
        if cls not in instances:  
            instances[cls] = cls(*args, **kw)  
        return instances[cls]  
    return _singleton  
 
@singleton  
class MyClass4(object):  
    a = 1  
    def __init__(self, x=0):  
        self.x = x  
 
one = MyClass4()  
two = MyClass4()  
```
### 6 面向切面编程AOP和装饰器

装饰器是一个很著名的设计模式，经常被用于有切面需求的场景，较为经典的有插入日志、性能测试、事务处理等。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量函数中与函数功能本身无关的雷同代码并继续重用。概括的讲，**装饰器的作用就是为已经存在的对象添加额外的功能。**

这个问题比较大,推荐: http://stackoverflow.com/questions/739654/how-can-i-make-a-chain-of-function-decorators-in-python

中文: http://taizilongxu.gitbooks.io/stackoverflow-about-python/content/3/README.html



