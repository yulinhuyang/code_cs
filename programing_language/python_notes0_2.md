
## 第十九章: 动态属性和特性

在python中, 数据的属性和处理数据的方法都可以称为 `属性` . 除了属性, pythpn 还提供了丰富的 API, 用于控制属性的访问权限, 以及实现动态属性, 如 `obj.attr` 方式和 `__getattr__` 计算属性.

动态创建属性是一种元编程,

### 使用动态属性转换数据

通常, 解析后的 json 数据需要形如 `feed['Schedule']['events'][40]['name']` 形式访问, 必要情况下我们可以将它换成以属性访问方式 `feed.Schedule.events[40].name` 获得那个值.

```python
from collections import abc
class FrozenJSON:
    """一个只读接口，使用属性表示法访问JSON类对象
    """
    def __init__(self, mapping):
        self.__data = dict(mapping)
    def __getattr__(self, name):
        if hasattr(self.__data, name):
            return getattr(self.__data, name)
        else:
            return FrozenJSON.build(self.__data[name]) # 从 self.__data 中获取 name 键对应的元素
    @classmethod
    def build(cls, obj):
        if isinstance(obj, abc.Mapping):
            return cls(obj)
        elif isinstance(obj, abc.MutableSequence):
            return [cls.build(item) for item in obj]
        else: # 如果既不是字典也不是列表，那么原封不动地返回元素
            return obj
```

### 使用 **new** 方法以灵活的方式创建对象

我们通常把 `__init__` 成为构造方法, 这是从其他语言借鉴过来的术语. 其实, 用于构造实例的特殊方法是 `__new__` : 这是个类方法, 必须返回一个实例. 返回的实例将作为以后的 `self` 传给 `__init__` 方法.

## 第二十章: 属性描述符

描述符是实现了特性协议的类, 这个协议包括 `__get__`, `__set__` 和 `__delete__` 方法. 通常, 可以实现部分协议.

### 覆盖型与非覆盖型描述符对比

python存取属性的方式是不对等的. 通过实例读取属性时, 通常返回的是实例中定义的属性, 但是, 如果实例中没有指定的属性, 那么会从获取类属性. 而实例中属性赋值时, 通常会在实例中创建属性, 根本不影响类.

这种不对等的处理方式对描述符也有影响. 根据是否定义 `__set__` 方法, 描述符可分为两大类: 覆盖型描述符和与非覆盖型描述符.

实现 `__set__` 方法的描述符属于覆盖型描述符, 因为虽然描述符是类属性, 但是实现 `__set__` 方法的话, 会覆盖对实例属性的赋值操作. 因此作为类方法的 `__set__` 需要传入一个实例 `instance` . 看个例子:

```python
def print_args(*args): # 打印功能
    print(args)

class Overriding:  # 设置了 __set__ 和 __get__
    def __get__(self, instance, owner):
        print_args('get', self, instance, owner)

    def __set__(self, instance, value):
        print_args('set', self, instance, value)

class OverridingNoGet:  # 没有 __get__ 方法的覆盖型描述符
    def __set__(self, instance, value):
        print_args('set', self, instance, value)

class NonOverriding:  # 没有 __set__ 方法，所以这是非覆盖型描述符
    def __get__(self, instance, owner):
        print_args('get', self, instance, owner)

class Managed:  # 托管类，使用各个描述符类的一个实例
    over = Overriding()
    over_no_get = OverridingNoGet()
    non_over = NonOverriding()
    
    def spam(self):
        print('-> Managed.spam({})'.format(repr(self)))
```

**覆盖型描述符**

```python
obj = Managed()
obj.over        # ('get', <__main__.Overriding object>, <__main__.Managed object>, <class '__main__.Managed'>)
obj.over = 7    # ('set', <__main__.Overriding object>, <__main__.Managed object>, 7)
obj.over        # ('get', <__main__.Overriding object>, <__main__.Managed object>, <class '__main__.Managed'>)
```

名为 `over` 的实例属性, 会覆盖读取和赋值 `obj.over` 的行为.

**没有 `__get__` 方法的覆盖型描述符**

```python
obj = Managed()
obj.over_no_get
obj.over_no_get = 7  # ('set', <__main__.OverridingNoGet object>, <__main__.Managed object>, 7)
obj.over_no_get
```

只有在赋值操作的时候才回覆盖行为.

### 方法是描述符

python的类中定义的函数属于绑定方法, 如果用户定义的函数都有 `__get__` 方法, 所以依附到类上, 就相当于描述符.

`obj.spam` 和 `Managed.spam` 获取的是不同的对象. 前者是 `<class method>` 后者是 `<class function>` .

函数都是非覆盖型描述符. 在函数上调用 `__get__` 方法时传入实例作为 `self` , 得到的是绑定到那个实例的方法. 调用函数的 `__get__` 时传入的 instance 是 `None` , 那么得到的是函数本身. 这就是形参 `self` 的隐式绑定方式.

### 描述符用法建议

**使用特性以保持简单**
内置的 `property` 类创建的是覆盖型描述符, `__set__` 和 `__get__` 都实现了.

**只读描述符必须有 \**set\** 方法**
如果要实现只读属性, `__get__` 和 `__set__` 两个方法必须都定义, 柔则, 实例的同名属性会覆盖描述符.

**用于验证的描述符可以只有 \**set\** 方法**
什么是用于验证的描述符, 比方有个年龄属性, 但它只能被设置为数字, 这时候就可以只定义 `__set__` 来验证值是否合法. 这种情况不需要设置 `__get__` , 因为实例属性直接从 `__dict__` 中获取, 而不用去触发 `__get__` 方法.

## 第二十一章: 类元编程

类元编程是指在运行时创建或定制类的技艺. 在python中, 类是一等对象, 因此任何时候都可以使用函数创建类, 而无需使用 `class` 关键字. 类装饰器也是函数, 不过能够审查, 修改, 甚至把被装饰的类替换成其他类.

元类是类元编程最高级的工具. 什么是元类呢? 比如说 `str` 是创建字符串的类, `int` 是创建整数的类. 那么元类就是创建类的类. 所有的类都由元类创建. 其他 `class` 只是原来的"实例".

本章讨论如何在运行时创建类.

### 类工厂函数

标准库中就有一个例子是类工厂函数--具名元组( `collections.namedtuple` ). 我们把一个类名和几个属性传给这个函数, 它会创建一个 `tuple` 的子类, 其中的元素通过名称获取.

假设我们创建一个 `record_factory` , 与具名元组具有相似的功能:

```python
>>> Dog = record_factory('Dog', 'name weight owner')
>>> rex = Dog('Rex', 30, 'Bob')
>>> rex
Dog(name='Rex', weight=30, owner='Bob')
>>> rex.weight = 32
>>> Dog.__mro__
(<class 'factories.Dog'>, <class 'object'>)
```

我们要做一个在运行时创建类的, 类工厂函数:

```python
def record_factory(cls_name, field_names):
    try:
        field_names = field_names.replace(',', ' ').split()  # 属性拆分
    except AttributeError:  # no .replace or .split
        pass  # assume it's already a sequence of identifiers
    field_names = tuple(field_names)  # 使用属性名构建元组，这将成为新建类的 __slots__ 属性

    def __init__(self, *args, **kwargs):  # 这个函数将成为新建类的 __init__ 方法
        attrs = dict(zip(self.__slots__, args))
        attrs.update(kwargs)
        for name, value in attrs.items():
            setattr(self, name, value)

    def __iter__(self):  # 实现 __iter__ 函数, 变成可迭代对象
        for name in self.__slots__:
            yield getattr(self, name)

    def __repr__(self):  # 生成友好的字符串表示形式
        values = ', '.join('{}={!r}'.format(*i) for i
                           in zip(self.__slots__, self))
        return '{}({})'.format(self.__class__.__name__, values)

    cls_attrs = dict(__slots__ = field_names,  # 组建类属性字典
                     __init__  = __init__,
                     __iter__  = __iter__,
                     __repr__  = __repr__)

    return type(cls_name, (object,), cls_attrs)  # 调用元类 type 构造方法，构建新类，然后将其返回
```

`type` 就是元类, 实例的最后一行会构造一个类, 类名是 `cls_name`, 唯一直接的超类是 `object` .

在python中做元编程时, 最好不要用 `exec` 和 `eval` 函数. 这两个函数会带来严重的安全风险.

### 元类基础知识

元类是制造类的工厂, 不过不是函数, 本身也是类. **元类是用于构建类的类**.

为了避免无限回溯, `type` 是其自身的实例. `object` 类和 `type` 类关系很独特, `object` 是 `type` 的实例, 而 `type` 是 `object` 的子类.

### 元类的特殊方法 **prepare**

`type` 构造方法以及元类的 `__new__` 和 `__init__` 方法都会收到要计算的类的定义体, 形式是名称到属性的映像. 在默认情况下, 这个映射是字典, 属性在类的定义体中顺序会丢失. 这个问题的解决办法是, 使用python3引入的特殊方法 `__prepare__` , 这个方法只在元类中有用, 而且必须声明为类方法(即要使用 `@classmethod` 装饰器定义). 解释器调用元类的 `__new__` 方法之前会先调用 `__prepare__` 方法, 使用类定义体中的属性创建映射.

`__prepare__` 的第一个参数是元类, 随后两个参数分别是要构建类的名称和基类组成的原则, 返回值必须是映射.

```python
class EntityMeta(type):
    """Metaclass for business entities with validated fields"""

    @classmethod
    def __prepare__(cls, name, bases):
        return collections.OrderedDict()  # 返回一个空的 OrderedDict 实例，类属性将存储在里面。

    def __init__(cls, name, bases, attr_dict):
        super().__init__(name, bases, attr_dict)
        cls._field_names = []  # 中创建一个 _field_names 属性
        for key, attr in attr_dict.items():
            if isinstance(attr, Validated):
                type_name = type(attr).__name__
                attr.storage_name = '_{}#{}'.format(type_name, key)
                cls._field_names.append(key)

class Entity(metaclass=EntityMeta):
    """Business entity with validated fields"""

    @classmethod
    def field_names(cls):  # field_names 类方法的作用简单：按照添加字段的顺序产出字段的名称
        for name in cls._field_names:
            yield name
```
