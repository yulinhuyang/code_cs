# 1    effcitive  python

## 1.1 Pythonic Thinking

### pep8 

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


### 迭代器：enumerate、zip  

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

### 生成器 

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
    

## 1.2 函数

### 全局与局部变量

**闭包中是怎样使用外围作用域变量**

global 关键字用于使用全局变量，nonlocal 关键字用于使用局部变量(函数内)

Python编译器变量查找域的顺序：

- 当前函数的作用域

- 任何其他的封闭域（比如其他的包含着的函数）。

- 包含该段代码的模块域（也称之为全局域）

- 内置域（包含了像len,str等函数的域）


### 参数 

位置参数： 使用*args定义语句，函数可以接收可变数量的位置参数

    def log(message, *values):
        if not values:
            print(message)
        else:
            values_str = ', '.join(str(x) for x in values)
            print('%s: %s' % (message, values_str))

    log('My numbers are', 1, 2)
    log('Hi there')
    

关键字参数：可选的关键字参数应该优于位置参数

关键字参数的好处:代码可读性的提高，以在定义的时候初始化一个默认值，在前面的调用方式不变的情况下可以很好的拓展函数的参数，不用修改太多的代码

    def flow_rate(weight_diff, time_diff, period=1):
        return (weight_diff / time_diff) * period

默认参数：使用None和文档说明动态的指定默认参数

**使用None作为关键字参数的默认值会有一个动态值**

prefer:

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


## 1.3 类和继承

**尽量使用辅助类来维护程序的状态，避免dict嵌套dict或大tuple**

避免字典中嵌套字典，或者长度较大的元组。

在一个整类（类似于前面第一个复杂类那样）之前考虑使用 namedtuple 制作轻量，不易发生变化的容器。

当内部的字典关系变得复杂的时候将代码重构到多个工具类中。

你可以从依赖树的底端开始，将其划分成多个类

prefer:

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

**对于简单接口使用函数而不是类的实例**

对于简单接口组件而言，函数就足够了。Python中引用函数和方法的原因就在于它们是first-class，可以直接的被运用在表达式中。

特殊方法__call__允许你像调用函数一样调用一个对象实例。需要一个函数来维护状态信息，考虑一个定义了__call__方法的状态闭包类。

内置的API都允许你通过向函数传递参数,如

        names = ['Socrates', 'Archimedes', 'Plato', 'Aristotle']
        names.sort(key=lambda x: len(x))

python hooks：函数可以作为钩子来工作，因为Python有first-class函数，函数、方法可以像其他的变量值一样被引用，或者被传递给其他的函数

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

**使用@classmethod多态性构造对象**

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


### 继承

**使用super关键字初始化父类**

Python的解决实例化次序问题的方法MRO解决了菱形继承（ diamond inheritance ）中超类多次被初始化的问题。

总是应该使用super来初始化父类。

python中父类实例化的规则是按照MRO标准来进行的，MRO 的执行顺序是 DFS

    class MyBaseClass(object):
        def __init__(self, value):
            self.value = value

    class Implicit(MyBaseClass):
        def __init__(self, value):
            super().__init__(value * 2)

**只在用编写Max-in组件的工具类的时候使用多继承**

如果可以使用mix-in实现相同的结果输出的话，就不要使用多继承了

当mix-in类需要的时候，在实例级别上使用可插拔的行为可以为每一个自定义的类工作的更好

从简单的行为出发，创建功能更为灵活的mix-in

Mix-in类：一个只定义了几个类必备功能方法的很小的类，不定义以自己的实例属性，也不需要它们的初始化方法__init__被调用

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

**多使用公共属性，而不是私有属性**

Python 编译器无法严格保证 private 字段的私密性

不要盲目将属性设置为private，而是应该从一开始就做好规划，并允子类更多地访问超类的内部的API

应该多用protected 属性，并且在文档中把这些字段的合理用法告诉子类的开发者，而不要试图用 private 属性来限制子类的访问

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

默认都是pubic类型，被子类、类内以及类外被访问

protected类型： _xx 以单下划线开头表示的是protected类型的变量或者方法，只能允许其本身与子类进行访问。

private类型：__xx 双下划线表示的是私有类型的变量或者方法, 只能允许类内进行访问。

特列方法:   __xx__定义的是特列方法。用户控制的命名空间内的变量或是属性


**自定义容器类型要从collections.abc来继承**

如果要定制的子类比较简单，那就可以直接从Python的容器类型（如list或dict）中继承

编写自制的容器类型时，可以从collection.abc 模块的抽象类基类中继承，那些基类能确保我们的子类具备适当的接口及行为

collections.abc的 Sequence : 子类只要实现 __getitem__以及 __len__， Sequence 提供了 count() 以及 index()

## 1.4  元类和属性

**用纯属性取代 get 和 set 方法**

使用public属性避免set和get方法，@property定义一些特别的行为

@property 方法应该遵循最小惊讶原则，而不应该产生奇怪的副作用

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
    
**考虑@property来替代属性重构**
 
使用@property给已有属性扩展新需求，可以用 @property 来逐步完善数据模型

当@property太复杂了才考虑重构

**用描述符来改写需要复用的 @property 方法**

如果想复用 @property 方法及其验证机制，那么可以自定义描述符类

WeakKeyDictionary 可以保证描述符类不会泄露内存

通过描述符协议来实现属性的获取和设置操作时，不要纠结于__getatttttribute__ 的方法的具体运作细节
    
not prefer：

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


**用 __getattr__, __getattribute__, 和__setattr__ 实现按需生产的属性**

通过__getattr__ 和 __setattr__，可以用惰性的方式来加载并保存对象的属性

要理解 __getattr__ 和 __getattribute__ 的区别：前者只会在待访问的属性缺失时触发，而后者则会在每次访问属性的时候触发

如果要在__getattributte__ 和 __setattr__ 方法中访问实例属性，那么应该直接通过 super() 来做，以避免无限递归

obj.name，getattr和hasattr都会调用getattribute方法，如果name不在obj.dict里面，还会调用getattr方法，如果没有自定义getattr方法会AttributeError异常

只要有赋值操作（=，setattr）都会调用setattr方法（包括a = A())








