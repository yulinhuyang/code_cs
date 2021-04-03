


# 下篇： 抽象机制与对象范式

# 第7章 抽象封装
==========

## 抽象思维-减法和除法的学问
### 什么是抽象
- 抽象就是做 __减法__ 和 __除法__
    - 减法，通过减去非本质和无关紧要的部分，__着眼于问题的本质__，即去粗取精以 __化繁为简__
    - 除法，透过现象看本质，发现 __不同事物之间的相同之处__，即由表及里以异中求同，__同类并归__
    - 乘法，为同类复制

- 无论是编程范式风格上的差异，还是编程语言级别上的高低，皆源于各自提供的 __抽象机制__ 的不同

- 抽象有 __角度__ 之分，相同的实体经过不同角度的抽象，得到的模型会不同
- 抽象还有 __程度__ 之别，抽象程度越高，细节越少，普适性越强

### 分析、设计和实现
尽管系统开发生命周期(Systems Development Life Cycle)按照不同的模型有不同的阶段划分，但最核心的三个还是分析(analysis)、设计(design)和实现(implementation):

- __分析阶段__：在理解 __问题领域__(problem domain)和明确 __业务需求__(business requirement)的基础上，制定出 __功能规范__(functional specification)
    - 分析阶段可细分为：针对核心业务的 __领域分析__ 和针对系统用户的 __应用分析__
    - 也包括一些非功能规范的制定，如系统的应用平台，数据库，框架等做出限定
    - 采用 __性质导向式__(property-oriented abstraction)抽象，通过对系统性质的逻辑描述来制定远东。关注是什么what，而不是怎么样how，一般不在设计上作任何决定或限制
    - 分析阶段的前期－领域分析(domain analysis)中，表示领域模型(domain model)的UML类图通常 __只标明类的性质__(property)，包括类的属性(attribute)和类与类之间的关联(association)，而类的运算(operation)则可有可无
    - 分析阶段的后期－应用分析(application analysis)，个体类的运算也不如 __整体系统的动态行为__ 更重要，而后者通过包括用例图(use case diagram)在内的各种行为图(behavior diagram)来体现的。
    - 性质导向式抽象侧重描述系统性质，因而是 __定性__ 的，抽象程序较高
- __设计阶段__：在分析的基础上制定出 __实现规范__(implementation specification)
    - 可细分为：偏向宏观战略的 __架构设计__(architectural design)和偏向微观战术的 __详细设计__(detailed design)
    - 采用 __模型导向式__(model-oriented abstraction)抽象，通过构造数据模型来满足系统的性质，实现功能规范
    - __类的运算__ 真正成为关注的要点之一
    - 模型导向式侧重于建造数据模型，因而是 __定量__，抽象程度较低
- __实现阶段__：在设计的基础上完成软件编码
- 与其区别设计与实现，不如把握抽象的级别

### 实现层的两种抽象机制

- __参数抽象__(abstraction by parameterization)：将函数的实现代码中一些特殊值作为参数来传递，即函数的每一个参数都是一种泛化，是对它所代表所有可能值的一种抽象。这是最普通最常用的一种抽象方式。

- __规范抽象__(abstraction by specification)：通过规范使代码的功能与实现相分离，规范了服务提供方的义务，保障了服务享受方的权利
    - 合格的 __文档注释__ 中至少包括 __先验条件__(precondition)和 __后验条件__(postcondition)，分别指代码执行前后必须满足的条件，前者是客户方的承诺，后者是服务方的承诺
    - 规范了服务提供方的义务，同时保障服务享受方的权利
    - 好处有三：
        1. 文档性，使用者不必阅读代码便可了解其用途并能正确使用它们，省时准确
        1. 局部性，无论阅读还是改写某个抽象的实现代码，都不必参考其他抽象的实现代码
        1. 可变性，实现者要遵循规范的前提下可自由修改实现代码，不用担心影响客户代码。

### 契约式设计
- __契约式设计__(Design by Contract，简称DbC)，解决说明性文档规范在自然语言描述不够精确，不能确保规范的实施的弱点。
    - 契约式设计语言Eiffel/D，明确保障包括先验条件，后验条件，类不变量，副作用等在内的契约
    - Java语言引入`assert`等断言，就是为了支持契约式编程
    - 子类的 __先验条件可以弱化__(接受更泛的参数)，__后验条件可以强化__(返回更细的返回值)，但不能反之，如下面，根据里 __里氏替换原则__(Liskov Substitution principle)，参照下面第10、20行的方法签名

<!--language: !csharp-->

    using System;

    class Tree{}
    class BananaTree:Tree{}

    class Fruit{}
    class Banana:Fruit{}

    class Animal{
        public virtual Fruit findFood(BananaTree tree){
            return new Fruit();
        }
    }

    class Monkey: Animal{
        public override Fruit findFood(BananaTree tree){
            return _findFood(tree);
        }

        private Banana _findFood(Tree tree){
            System.Console.WriteLine(tree.GetType().ToString());
            System.Console.WriteLine("Monkey");
            return new Banana();
        }
    }

    class App{
        public Animal animal{get;set;}
        public void Do(){
            Fruit f = animal.findFood(new BananaTree());
            System.Console.WriteLine(f.GetType().ToString());
        }
    }

    class Program {
        static void Main(string[] args) {
            var app = new App();
            app.animal = new Monkey();
            app.Do();
        }
    }

- __防御性编程__(defensive programming)vs__契约式设计__
    - 防御性编程缺陷：
        1. 导致先验条件的重复检查，比如函数已检查一个参数的合法性，当该函数把该参数继续传入另一个函数时，后者很可能还要对它检查一遍，增加了代码冗余，又降低了程序效率
        1. 增加程序员的负担的困惑，是返回错误代码，还是抛出异常？抛出什么异常，要写错误日志吗
        1. 职责不明，究竟谁该保证先验条件的成立？出了问题追究谁的责任？如何追究？
    - 所以，防御性编程采取是 __先小人后君子__ 的策略，而契约式设计则是 __先君子后小人__
    - 前者重要保证软件 __健壮性__(robustness)，适合应付无法防止或难以预测的异常，后者重要保证软件的 __正确性__(correnctness)，适合应付不应当发生的异常，如代码中的缺陷

### 五类基本抽象
借助两种抽象机制，我们可以实现五类基本抽象：

- __过程抽象__(procedural abstraction)：自定义运算(operation)的能力 将行为的逻辑属性与实现细节分离
- __数据抽象__(data abstraction)：自定义类型(type)的能力 将数据的逻辑属性与表现细节分离
- __迭代抽象__(iteration abstraction)：自定义循环(loop)的能力 将集合遍历与元素的获取细节分离
- __类型层次__(type abstraction)：自定义类族(type family)的能力 将类型的公共行为与具体类型分离
- __多态抽象__(polymorphic abstraction)：自定义多态类型(polymorphic type)的能力
    - 抽象类型(OOP)，类型的接口规范与实现代码分离
    - 参数类型(泛型GP)，将类型与算法分离

## 数据抽象-what重于how

### 数据结构vs抽象数据类型
- __数据结构__ 强调具体实现，关键是 __属性的数据表示__，多从 __实现者和维护者角度__ 来考虑；__抽象数据类型__(ADT)强调抽象接口，重在设计，关键是 __行为的抽象接口__，多从 __设计者和使用者的角度__ 来考虑
- 如栈、队列、表、集合、二叉树等作为数据结构，人们关心的是如何利用它们来有效组织数据，而作为抽象数据类型，更关心是类型的接口、逻辑行为及背后的数据模型，比如队列是用数组还是链表来实现，用户根本不需要关心。
- __具体数据类型__，主要用于数据存储，没有实质性运算，如数据传输对象DTO，又称值对象VO，更多地体现出被动的特征
- __参数抽象__ 使数据接口 __普适化__，__规范抽象__ 使数据接口 __契约化__

### OOA,OOD,OOP
- OOA以 __对象__ 而非过程为中心，是 __描述问题__，而非解决问题
- OOD以 __接口__ 而非实现为中心，强调 __对象行为，对象交互__
- OOP以 __数据__ 而非算法为中心，强调 __算法对数据的依赖__
- 接口与实现分离，有利于开发 __时间__ 的分离及开发 __人员__ 的分离，前者指开发人员可以推迟在不同实现方式中作出最后抉择，后者指，代码的修改和维护不局限于原作者

### 类与ADT
- 可以将OOP中的类理解为具有继承与多态的ADT
    - C#中有值类型与引用类型之分，选择时可把ADT作为一个参考原则，是则采用 __引用类型__，否则采用 __值类型__，分别用class和struct表示
    - C++中struct与class在机制上没有区别（只是前者成员默认是public，后者为private），但习惯上前者作 __具体类型__，后者作 __抽象类型__
    - Java和C没有类似区分，一个只支持class，另一个只支持struct
- 但并非OOP中的ADT一定是以类的形式出现，如整型，它的抽象之处在于用户不须知道一个整数的底层究竟如何表示的，以及整数运算是如何实现的，只须知道整形代表着数学概念上的整数，支持加减乘除等运算即可

##　封装隐藏-包装的讲究
### 什么是封装
- __信息隐藏__ 是一种原则，__封装__ 是实现这种原则的一种方式
- 信息隐藏中的信息不仅仅是 __数据结构__，还包括 __实现方式和策略__
- 广义的封装仅是一种 __打包__，即package或bundle，是密封的但可以是透明的
    - OOP与纯过程式语言最大不同，在于引入了 __一种新的模块机制__，将相关的数据和作用其上运算捆绑在一起形成被称为类的模块
    - 由于C不支持封装，只能 __以文件形式来划分模块__，显然不如类划分那么方便和明晰
    - 语法糖(syntactic sugar)，`牛.吃(草)` vs `吃(牛,草)`，符合自然语言；其次，这种绑定使`queue_add`局限在`Queue`类中，因此不必加上`queue_`前缀，以防与其他类的方法函数名相冲突，即分属不同类的函数不可能产生歧义的，即使它们签名一样
- 狭义的封装是在打包的基础上加上 __访问控制__(access control)，以实现信息隐藏，用户不需要也无权访问底层实现
    - 访问控制不仅是一种语法限制，也一种语义规范，标有public的是接口，标有private的是实现，泾渭分明
    - 访问控制也不是固若金汤，C++可通过指针来间接访问private成员，Java和C#可利用反射来得到隐藏的信息（反射机制多用于单元测试、代码分析、框架设计等，常规应用很少使用）

### 一个封装示例

<!--language: java-->

    import java.util.Date;
    import java.util.Calendar;

    class Person{
        private Date birthday;
        private boolean sex;
        private Person[] children;

        public Person(Date birthday, boolean sex){
            this.birthday = birthday;
            this.sex = sex;
        }

        public Date getBirthday(){
            return birthday;
        }

        public Person[] getChildren(){
            return children;
        }

        public boolean getSex(){
            return sex;
        }
    }

- 如果一个方法返回了一个可变(mutable)域对象(field object)的引用，无异于前门紧闭而后门洞开，如`getBirthday`返回`Date`类型的生日，用户可以在调用此方法后直接对生日进行操作。解决它需要防御性复制(defensive copying)，返回一个对象的复制品，以免授人以柄
- 数组也是可变的对象，`getChildren`是否使用复制数组`Arrays.copyOf`的方式？考虑用户经常需要获取某一指定位置的`child`，不妨提供`getChild(int index)`的接口，还可引入`getChildCount`、`getFirstChild`之类的接口，减少复制数组而高效，客户代码因较少操作数组而简洁，甚至连`getChildren`都不再必须的了。同样`setChildren`分解成`addChild`和`removeChild`等
- 关于`getSex`，有时性别是未知的，如果想处理三种以上可能性，可以采用`int,char,enum`，总之这是实现细节，最好不要暴露给客户，不妨换成`isMale`和`isFemale`两个接口

将`getBirthday`改成如下：

<!--language: java-->

    public Date getBirthday(){
        return (birthday==null)?null:new Date(birthday.getTime());
    }

- 为什么不用`birthday.clone()`不是更简单吗？由于`Date`不是`final`类，可能用子类覆盖`clone`。此外一个域对象又包含其他可变对象，简单按位复制，即浅拷贝(shallow copy)还不够，需要其他的复制策略，如深拷贝(deep copy)或迟拷贝(lazy copy)或写时拷贝(copy-on-write)

- 不是所有返回的域对象都会带来安全问题，注意 __可变__ 和 __引用__ 两个条件：
    - __基本类型__ 都不是引用，作为传入参数或返回值时会自动被复制，因而是安全的
    - __不可变的非基本类型__，如Java中String，基本类型的包装类如Integer、Double等，同样也是安全的
    - C++和C#的非基本类型的 __值类型__ 一般也不在危险之列
    - C++申明了 __`const`__ 指针或引用返回值也能防止客户修改
    - 有时，在非同类的对象之间共享或传递一个引用有时是必要的，避免对象复制带来性能损耗

### 值对象vs引用对象
- 值对象关心的是 __值__(value) ，而非它的同一性，不希望不同的对象因共享相同的引用而导致同步修改，如果值对象是可变的，往往需要利用值拷贝防止因同一性而导致意外同步变化，如`birthday`须要复制的根本原因，它语法是引用对象，语义上却是值对象。
- 引用对象重要的是 __同一性__(identity)，而非值，不须要也不应该进行值拷贝，如`child`，在`getChild`,`addChild`等不需要复制对象
- 所以在设计类时，必须考虑在 __语义__ 上是引用对象还是值对象：





# 第8章 抽象接口
==========

## 软件应变-随需而变,适者生存

### 信息隐藏vs抽象
- 信息隐藏，这是为了实现数据抽象，将接口与实现分离开来，一方面，抽象接口描述了一个类最本质的行为特征，另一方面，具体实现随时可能变动，隐藏它们可以保证这种变动不会涉及客户代码

- 许多编程设计思想包括OOP都以 __提高应变力__ 为主题的，__抽象与封装__ 便是典型代表

- __抽象__ 一个对象模型即是将一类对象最本质而最不易变化的部分提炼出来，而 __封装(信息隐藏)__ 则是将非本质、容易变化的部分隐藏起来
- 所以，__封装(信息隐藏)__ 是为了提高软件的 __抗变能力__，而不是 __软件安全__

### 软件变化类型
- 出于 __内在__ 需求而作的 __结构性变化__，以改善软件质量为目的，包括重构(code refactoring)、性能调优(performance tuning)等
- 出于 __外在__ 需求而作的 __功能性变化__，以满足客户需求为目的

### Pimpl与桥梁模式
- C++需要头文件，即使私有成员也必须在头文件中声明，意味着改动任何私有数据结构甚至私有方法的签名，所有包含该头文件的源代码虽然不改写，却须要重新编译链接
- 所以，设计与语言息息相关，依赖于语言细节
- C++尽可能使用 __前置声明__(forward declaration)，__减少包含__ 的(included)头文件，另外，可经将一些私有静态(private static)成员从头文件转移到实现代码中，以匿名命名空间(anonymous namespace)的方式来实现完全隐藏
- __Pimpl__惯用法(Pointer to Implementation idiom)，不仅解决了C++中头文件问题，对Java，C#等不需要头文件的语言也有用
    - Pimpl也称编译器防火墙惯用法(compiler firewall idiom)、句柄类(handle classes)、不透明指针(opaque pointer)、柴郡猫(Cheshire Cat)
    - 柄/体(handle/body)模式或桥梁模式(bridge pattern)

C++头文件

<!--language: cpp-->

    //c.h
    #include "x.h"
    class C{
        public:
            void f1();
        private:
            X x; //与X强耦合
    }

使用Pimpl惯用法

<!--language: cpp-->

    //c.h
    class X; //用前导声明取代include
    class C{
        public:
            void f1();
        private:
            X* pimpl; //声明一个X*，在既定平台，指针大小都相同
    }

    //c.cpp
    #include "x.h"

    C::C() {
        x = new X;
    }

    C::~C() {
        delete x;
    }

- 桥梁模式，可以让客户在 __运行期间__ 选择实现方式，信息隐藏虽然能将抽象接口与具体实现分离，但仍然封装在同一类中，而该模式让二者彻底解耦，增强了对化的适应力，更大的灵活性和可扩展性

## 访问控制-代码的多级管理

### 访问修饰符
- 访问修饰符(access modifier)除了可以应用于类的成员外，在Java和C#中还能应用于整个类

<!--language: table-->

    |范围\语言   |C++          |Java         |C#                |
    |------------|-------------|-------------|------------------|
    |无限制      |public       |public       |public            |
    |子类或同一包|-            |protected    |protected internal|
    |同一类或子类|protected    |-            |protected         |
    |同一包      |-            |package(默认)|internal          |
    |同一类      |private(默认)|private      |private(默认)     |

- `public`和`private`是两个极端，一个没有限制，一个仅限于同一类，Java和C#比C++多了包(package或assembly)概念，相应多了`package`和`internal`修饰符。Java中`protected`相当于C#中的`protected internal`，不仅可被同类和子类访问，还能被同一包内的任何类访问，而C++和C#中的`protected`只能被同类和子类访问。
- 一个基本原则是 __尽可能地使用限制性更强的修饰符__，逐渐放宽
- 如果将每个类看作一个服务者，它向不同范围内的客户承诺不同的服务，或 __层次化__ 服务，以Java为例：
    - `public`向所有类提供的服务
    - `protected`对该类的子类或同一package下类提供服务
    - `package`仅对同一package下类提供服务
    - `paivate`则只对该类本身提供服务

### 嵌套类
- 访问修饰符除了用于域成员和方法成员外，还可以用于类成员，也就是嵌套类(nested class)，又叫内部类(inner class)
    - 嵌套类可能比内部类更广泛，如Java中有四种嵌套类：静态成员类、非静态成员类、匿名类、局部类，只有后三种才是内部类
- OO化的 __闭包__ 在Java中就采用特殊的嵌套类－__匿名类__ 来实现的，一个类仅仅为另一个方法单独服务时

### C++中friend
- C++没有顶层类的访问修饰符（但C++在类作为基类 __被继承时可以加上访问修饰符__），如果一个类将另一个类或函数声明为`friend`，意味着允许后者访问该类的所有成员，包括`private`和`protected`成员
- __当`friend`用于类__ 时，相关的两个或多个类本是作为 __一个整体来设计__ 的，可以理解为将一个类劈成了几半
    - 将一类按某些功能划分成几部分，如有一个类 __部分代码是自动生成__ 的，单独放在一个文件中显然更好，C#不支持`friend`，却引用了`partial`，可以将一个类拆分到多个源文件中
- __当`friend`用于修饰函数__ 时，更多是出于语法形式的考虑，常用于 __运算符重载__(operator overloading)
- 一个类与其友类或友函数是 __联合关系而非主客关系__，它们之间的互访与普通类内部成员的互访没有本质不同，甚至由于`friend`是 __单向授权__ 的，反而是`private`的一种细化

### 访问控制的边界

<!--language: java-->

    public class Unit {
        private short ashort;
        private char achar;

        public boolean equals(Object o) {
           if (!(o instanceof Unit))
               return false;
           Unit unit = (Unit) o;
           return unit.ashort == ashort
                  && unit.achar == achar;
        }
    }

- 一个类的方法能访问除this之外的其它同类对象的private成员（如上面的`unit.ashort`），盖因访问控制是对 __静态代码__ 的控制，而不是动态对象（不是运行期间如java的security manager等管理的安全机制），__以代码而非对象为边界__

- 从抽象的角度来看，访问控制划分了抽象的边界，
    - 一方面从 __语义上__ 明确抽象的层次化：越公开的成员越接近抽象接口，越远离具体实现；
    - 另一方从 __语法上__ 施行双向保护，既保护实现代码不受代码侵入，也保护客户代码不受实现代码变更的影响

### 划分代码修改边界
- 访问控制划分了代码修改的边界，以下以java为例：
    - 修改涉及`private`，只检查该类的源代码即可
    - 修改涉及`package`，检查该类所在package内所有类
    - 修改涉及`protected`，不仅检查该类所在package内所有类，还须检查该类的子类，如果该类本身为`public`，涉及的类可以超出该package，已难以真正掌控
    - 修改涉及`public`，就意味着任何类都可能调用该接口
- 成熟程序员`public`和`protected`接口的设计一定是慎之以慎
- Java和C#分别提供了`deprecated`,`obsolete`以将方法 ，甚至将整个类标记为过时，如果被废弃的类或方法有替代方案，在文档中还应特别注明

## 接口服务-讲诚信与守规矩
作为服务的提供者，最重要的是讲诚信；作为服务的享受者，最重要的是守规矩

### 可靠性与稳定性

- 作为服务的提供者，最重要的是讲诚信
    - 服务要有 __可靠性__，不能阳奉阴违，从抽象角度看，保证了 __规范抽象__，规范只是语义的契约，没有语法上的约束，不在编译器监管范围之内，只能靠 __单元测试__ 了
    - 服务要有 __稳定性__，不能朝令夕改，从抽象角度看，保证了 __数据抽象__，如果接口被废弃或其签名发生变化，固然会牵连客户，至少还可以通过编译器来发现和修改（动态类型语言可能不会被编译器检查）

### 纯粹性与完备性
- 高质量服务还要 __纯粹性__ 和 __完备性__，一个类只提供一套服务，但要完善。
- 提供的服务过多则不纯粹，过少则不完备。其实关键不在于服务数量的多寡，而在于服务的 __一致性__ 和 __关联性__，如果一个类提供几种关联性不强的服务，可考虑将其划分为几个类；如果几个类分别提供互补的服务，可考虑将其合并为一个类






# 第9章 继承机制
============

## 继承关系-继承财富,更继承责任

### 泛化vs继承
- UML通过 __泛化__(generalization)来表达继承关系，从逻辑上说，比 __继承__(inheritance)或遗传更准确，更确切的说，泛化强调的是 __概念关系__，而继承强调的是 __语言机制__
- 继承是子类继承父类，泛化却是父类泛化子类，__视角不同__，也可用特化(specialization)来表示从父类到子类的关系
- UML侧重于 __设计__，人们常会从设计的一些类中提炼出共同的特征，一并放在新建的类中，此时 __父类是后于子类设计__ 的，或许叫 __超类__(superclass)更合适，即子类到超类的泛化，是一种 __概念抽象__(abstraction)过程，从超类到子类的特化，是一种 __概念细化__(refinement)过程，这两种过程在设计中往往是 __交替采用__ 的，属于前面[5类基本抽象](#TOC7.1.5)之一的 __类型层级__(type hierarchy)


### 实现继承vs接口继承

- Java和C#都无法做到 __只继承实现，而不继承接口__ 的，C++则可以，它的继承有3种修饰符`public,protected,private`，后两种则继承实现而不继承接口

<!--language: cpp-->

    class Engine{
        public:
            void start(){...} //发动引擎
    };

    class Car : private Engine{ //Car私有继承Engine
    };

    int main(){
        Car().start(); //发动汽车
        return 0;
    }

上面的代码无法通过编译，可将`private`继承改为`public`继承，虽然能通过编译，但不是好的设计，因为Car与Engine不是`is-a`的关系，而是`has-a`的关系，更好的方式是：

<!--language: cpp-->

    class Car : private Engine{
        public:
            void start(){Engine::start();} //手动增加自身start实现
    };


还有一种更简洁方法，即 __重新开放基类__ 的接口，但用法有些怪异，更像惯用法：

<!--language: cpp-->

    class Car : private Engine{
        public:
            using Engine::start;
    };

还有一种 __合成/组合/复合__(composition)来实现，更像一种模式：

<!--language: cpp-->

    class Car : private Engine{
        private:
            Engine engine;
        public:
            void start(){engine.start();}
    };

- 第2种方法与第4种方法很像，__非公有继承__ 与 __合成__ 是对 __实现的重用__ 而非接口的重用，不妨把非公有继承看作是一种匿名的非显性合成，它们主要区别在于：
    - 前者更有 __侵入性__，可以访问基类的`protected`成员，也可以覆盖基类的方法
    - 后者更有 __隐蔽性和动态性__，合成所依托的类的具体类型可以是隐性的，并可以动态改变
    - 非公有继承用法较为复杂且容易误用，所以Java和C#都不再支持，但依然可以用合成的方式达到对实现的重用

### 类继承vs接口继承
- 类继承既继承了实现，又继承了接口，但人们往往简单称为 __实现继承__(implementation inheritance)，一来为了与接口继承相对，二来一般语言不支持纯粹的实现继承

- 有一种特殊的接口－标记接口(marker interface/tagging interface)，比如`Cloneable,Serializable,EventListener`等，甚至连一个方法都没有，接口继承可以使一个类参与该接口所能参与的任何运算，如继承了`Comparable`，便能参与集合的排序运算，继承了`MouseListener`便能监听鼠标事件

- __接口继承__ 的作用不是为了让继承者重用，而是为了在合适的场合被调用，即不是为了代码重用，而是为了代码 __被重用__

- 实现继承__消费__ 可重用的旧代码，接口继承 __生产__ 可重用的新代码

### 子类vs子类型
- 继承也被称为 __子类化__(subclassing)，接口继承进而被称为 __子类型化__(subtyping)
- 子类(subclass)不一定是子类型(subtype)，如C++中private继承产生的子类不是子类型，反过来，子类型也不一定不是子类，如int是long子类型，但不是它的子类发（甚至不是类），
- 子类型关键是 __可代换性__(substitutability)，即 __里氏代换原则__(Liskov Substitution Principle，简称LSP)，类型A的子类型B应该满足以下条件：将程序中类型A的对象置换为类型B的对象，不会影响程序的合理性和正确性。

<!--language: java-->

    interface Bird{
        public void fly();
        public void lay();
    }

    class Penguin implements Bird{
        public void fly(){
            throw new UnsupportedOperationException("Cannot fly");
        }
        public void walk(){...}
        public void lay(){...}
    }

    public void letGo(Bird bird){
        if(bird instanceof Penguin)
            ((Penguin)bird).walk();
        else
            bird.fly();
    }

客户代码得为这种特殊类型做特别处理，根源在于`Penguin`类违背了LSP原则

### 里氏代换原则

- 里氏代换原则的本质是为了 __保证规范抽象__，让规范抽象不再局限于单个类型，而是整个类族上，并且规范是 __向上兼容__(forward compatible)，通俗些，就是一个人作出承诺，他的子子孙孙都得遵守，只能加强，不能削弱。只有这样，当客户用到某一抽象类型时（主要是interface或abstract class），才能完全不用关心其实现的具体类型，只须关心抽象类型的规范即可。

- __多态数据抽象__(polymorphic data abstraction)，或者简称 __多态抽象__，意指一种类型可能具备多种类型的形式，显然，__多态抽象是建立在类型层级的基础上__ 的。
- 接口继承在遵循里氏代换原则的 __前提__ 下，通过接口重用达成规范重用，__保证__ 了多态抽象，__进而__ 维护了开闭原则

- 如果只是为了重用基类的代码(权利)，而不希望重用它的接口(义务)，应该采用 __合成而*不是*继承__ 方式，这是一个通用编程原则，尽可能弥合 __语法__ 与 __语义__ 之间的缝隙，以压缩代码臭虫的空间

### 'is-a' vs 'behaves-like-a'
- 与其说继承是一种实现的技巧，不如说是一种 __规范__ 的技巧
- 其说是`is-a`或`is-a-kind-of`的关系，不如说`behaves-like-a`或`is-substitutable-for`的关系
    - `is-a`说法太笼统，容易产生歧义，可以表示泛化，表示分类，表示角色，表示能力。但并不是所有都适合用继承关系来表示的，`is-a`只能作为判断继承关系的一个 __必要条件__，不能作 __充分条件__
    - 光`is-a`不能使用继承的经典例子，正方形是矩形，但行为并不一样，所以并不是继承关系
        - 矩形有也错，它没有明确描述`setWidth`的 __后验条件__－高度应保持不变
        - 当然，正方形是否继承矩形，最终还要看实际需求和具体规范，不能一概而论

<!--language: java-->

    class SortedList<E> implements java.util.List<E>{
        public boolean add(E e){...}
        public E set(int index, E element){...}
        ...
    }

- 将排序列表`SortedList`设计为列表`List`的子类型看起来天经地义，但在实现`add`和`set`方法时犯难了，按`List`的`add`规范，新加入元素应排在最末，但这可能破坏`SortedList`的类的不变量，`set`也有类似问题
    - 其实`add`与`set`这两个行为规范上出现分歧，貌似`is-a`，却不是`behaves-like-a`
    - 解决办法之一是退而求其次，让`SortedList`作为`List`的超类型`Collection`的子类型，`Collection`的`add`只承诺元素在集合中，对位置并没有要求。

- Duck类型也要求抽象规范一致，关心`behaves-like-a`，只是它是站在使用者角度，而静态类型的继承是提供者角度


- 概念抽象只是 __手段__，规范抽象才是 __依据__，体现类型的是它的 __行为规范__，而不是它的 __命名所引发的认知__

### 保证类族的规范抽象
- 任何类型都应该 __保持或强化__ 其超类型的规范，绝不能弱化规范，“要求只能更少，承诺只能更多”
- 用契约式设计语言来说，先验条件只能 __弱化__，要求更少(接受更泛参数)，后验条件和类不变量只能 __强化__，承诺更多(返回更细类型，协变返回类型)，[参考](#TOC7.1.4)
    - 以Java为例，如果一个类型的多态方法，非static，非final方法，被子类所覆盖，后者返回类型必须与前者相同或是其子类型，即所谓 __协变返回类型__(covariant return type)；
    - 后者的声明的受检异常(checked exception)不能超出前者的范围；
    - 后者的访问修饰符不能比前者的更严格（C++中并无此限制，但不建议突破此限制）

### 类与类型
- 类往往专指class，但还有interface,struct,基本类型，虽然各不同，但 __都是类型的具体表现形式__
- 里氏代换原则是 __基于类型__的，简言之，__类偏重语法，强调实现方式，类型偏重语义，强调行为方式__，类是实现，类型是接口


### 分离类的接口与实现

一个作为抽象数据类型(ADT)的类，通过 __数据抽象和封装机制__ 被划分为 __接口__ 与 __实现__ 两部分，此时的接口称为 __API__，是类向外界提供服务的窗口。另一方面，一个类又可以通过 __多态抽象和继承机制__ 而拥有多种抽象类型（主要指interface所代表的类型），相对这些抽象类型而言，类又是具体的。

![prog_paradigm_interface.png](../../pics/prog_paradigm_interface.png)

- 第1种分离方式：在语义上通过数据抽象得到API接口，在语法上通过 __封装机制__ 隐藏了`private`实现
- 第2种方式：在语义上通过多态抽象得到`interface`接口，在语法上通过 __继承机制__ 隐藏了`class`实现
- 相比前者，后者抽象层次更高
- OOP将现实中的 __概念抽象__ 映射为程序中的 __类型__，__继承机制__ 进一步将概念的 __分类体系__ 映射为类型的 __层级结构__

## 慎用继承-谨慎对待权力

### 继承vs合成
- JDK败笔：`java.util.Properties`继承`Hashtable`，虽然`Properties`也是处理键值对，但由于主要用于系统属性配置，多了两个要求：可持久化(persistent)和人工读写(human-readable/writable)，对于前者，只须增加一些诸如IO流相关的接口即可，但对于后者，则要求键值均为`String`类，意味着`Properties`类型的`put`方法 __先验条件比其超类型的更强__，从而违背了里氏代换原则。所以官方文档声明，反对使用`put`和`putAll`，建议使用`setProperty`。更好的方式是，`Properties`不继承任何类，内部使用`Hashtable<String,String>`来完成职责
- 继承是一种 __静态、显性__ 的关系，静态指关系是在编译期间建立的，无法在运行期间改变；所谓显性，指关系是公开的，如果通过源代码来改变，将会影响客户代码
- 合成关系是 __隐性__ 的，属于内部实现，并且是 __动态__ 的，实现者可以在运行期间设定依托的具体类型，比如在`Properties`内部的map在单线程时采用`HashMap`，在多线程时采用`Hashtable`，当数据达到一定阈值时，自动切换到更高效的`Map`类型
    - 此例中合成也称 __聚合__(aggregation)，更准确的说，是一种 __委托__(delegation)或 __转发__(forwarding)
    - 合成或聚合关注整体与部分关系，侧重静态结构；委托或转发关注表面受理者与实际执行者的分离，侧重动态功能。
    - 一般情况下，两者经常是重合，但也是例外
- 合成是不透明的 __黑盒重用__(black-box reuse)，而实现继承是透明的 __灰盒重用__(grey-box reuse)

- JDK另一个设计，`java.util.Stack`继承`Vector`，比上面更不可思议，从抽象概念上看，`Stack`是后进先出(LIFO)的，而`Vector`是随机读取的列表，是完全不同的数据结构，不存在`is-a`的关系；从具体行为上看，栈的主要运算是压栈(push)、出栈(pop)等，不能像列表那样能读取、改写或增删任一元素，因此也不是`behaves-like-a`

### Stack继承与合成

<!--language: java-->

    /* java.util.Stack的改进版1(multiPush依赖push) */
    public class Stack<E>{
        private Vector<E> data = new Vector<E>();

        public void push(E item){
            data.addElement(item);
        }

        public void multiPush(E... items){
            for(E item : items)
                push(item);
        }
    }

与原始版本比，最大变化是将继承改为合成，增加了一个`multiPush`方法，实现了对多个元素同时压栈。这时我们需要一个特殊的栈，能记录先后压栈次数：

<!--language: java-->

    /* 记录压栈总次数栈(原始版) */
    public class CountingStack<E> extends Stack<E>{
        private int pushCount;

        @Override public void push(E item){
            ++pushCount;
            super.push(item);
        }

        @Override public void multiPush(E... items){
            pushCount += items.length;
            super.multiPush(items);
        }

        pubic int getPushCount(){
            return pushCount;
        }
    }

你会发现存在BUG，`CountingStack`对象在调用`multiPush`先对计数增加，然后调用父类的同名方法，__后者却隐式的调用了`push`，而该方法却又被自己所覆盖__，造成二次计算

解决办法之一是，__去掉子类的`multiPush`，直接沿用父类的方法__，但如果父类改成如下时，则又不能正确计数：

<!--language: java-->

    /* java.util.Stack的改进版2(multiPush与push互相独立) */
    public class Stack<E>{

        public void multiPush(E... items){
            for(E item : items)
                data.addElement(item);  //原来版本：push(item);
        }
    }

现在父类中`multiPush`不再调用`push`，就不会被子类的`push`覆盖，根本没机会计算`pushCount`，接下来使用拷贝父类实现（假设Stack<E>还是版本1），试图解决该问题：

<!--language: java-->

    /* 记录压栈总次数栈(原始版) */
    public class CountingStack<E> extends Stack<E>{
        private int pushCount;

        @Override public void push(E item){
            ++pushCount;
            super.push(item);
        }

        @Override public void multiPush(E... items){ //拷贝父类实现
            for(E item : items)
                push(item);
        }
    }

但如果父类的`multiPush`的实现并不依赖`push`，而是反过来`push`实现依赖于`multiPush`时，则又出问题了

<!--language: java-->

    /* java.util.Stack的改进版3(push依赖multiPush) */
    public class Stack<E>{
        private Vector<E> data = new Vector<E>();

        public void push(E item){
            multiPush(item);
        }

        public void multiPush(E... items){
            for(E item : items)
                data.addElement(item);
        }
    }

说白了，以上的继承是依赖于父类的实现的，当父类的实现有所改变时，子类将不能正常工作。所以要 __慎用实现继承__。可以尝试一下 __合成__，因为这样，只依赖合成对象的外部接口：

<!--language: java-->

    /* 记录压栈总次数栈(合成版) */
    public class CountingStack<E> extends Stack<E>{
        private Stack<E> stack = new Stack<E>();
        private int pushCount;

        public void push(E item){
            ++pushCount;
            stack.push(item);
        }

        public void multiPush(E... items){
            pushCount += items.length;
            stack.multiPush(items);
        }

        public E pop(return stack.pop();)//转发
        ...
    }

采用合成以后，`CountingStack`从`Stack`类家族成员转成普通客户，不再插手内部事务，曾经问题迎刃而解。但有个问题就是需要将`Stack`类的其它方法一一转发，代码繁琐。

### 继承设计防守要点
子类与父类代码上是隔离的，多态机制又是隐性的，防守起来实为不易，如上例中子类调用父类方法，父类又可隐式调用子类多态方法。

- 防守要点1：__子类应坚持父类的外在行为__，不能破坏父类制定的服务规范，覆盖了父类某一方法时注意遵循该方法的规范，同时还需要注意相关方法的规范，如覆盖了`add`，很可能需要修改`remove`，同时要顾及`addAll`、`removeAll`等方法是否受到牵连，如果覆盖了`equals`方法，多半需要覆盖`hashCode`方法。
- 防守要点2：__子类应正视父类的内存逻辑__，既不能忽视或破坏父类规范之内的逻辑关联，又不能假设或依赖规范之外的逻辑关联

### 慎用实现继承
- 如上例代码，实现继承最大硬伤是在类与类之间建立了 __强耦合__ 关系，这种关系是永固的，一经建立无法解除，纵深的，不仅限于相邻父子类，更贯穿整个类族。
- 该类不仅要了解其父类，还要了解一切祖先类；不仅了解`public`成员，还要了解`protected`成员
- 如果想覆盖某一方法成员，还得了解祖先类成员之间的 __内在逻辑__ 关系；如果想增加一个方法成员，还得不与祖先将来的版本发生冲突。
- `CountingStack`类就是著名的 __脆弱的基类__ 问题(Fragile Base Class Problem)的一个典型代表。
- 其它问题如：构造方法中不宜直接或间接调用多态方法；在克隆或序列化时也需要特别谨慎。
- __提倡接口继承__，以下是合成加接口继承版本：

<!--language: java-->

    public interface Stack<E>{
        public void push(E item);
        public void multiPush(E... items);
        public E pop();
        public E peek();
    }

    // java.util.Stack的改进版4(接口继承)
    public class DefaultStack<E> implements Stack<E>{
        // 代码同 改进版1
    }

    // 记录压栈总次数栈(合成加接口继承版)
    public class CountingStack<E> implements Stack<E>{
        private Stack<E> stack = new DefaultStack<E>();
        private int pushCount;

        public void push(E item){
            ++pushCount;
            stack.push(item);
        }

        public void multiPush(E... items){
            pushCount += items.length;
            stack.multiPush(items);
        }

        public E pop(return stack.pop();)//转发
        ...
    }

### 为继承而设计(基类)

- 实现继承的使用场景
    - 采用合成是否会遇到 __无法克服__ 的困难
    - 基础类是否 __专门为继承而设计__

- 语言中终极类型与抽象类型
    - Java与C#加了`final`和`sealed`，或者以`struct`,`enum`的值类型出现，表示不打算要后代的终极类型
    - C++中没有冷言专门的类级别修饰符，但若一个类既没有一个虚函数(virtual function)，也没有一个`protected`成员，通常也是不准备被继承的
    - 相反，有些类必须被继承，否则不能实例化，如Java和C#的抽象类(abstract class)，C++的纯虚函数(pure virtaul function)，或一个类只有一个`protected`的构造器且不能通过静态方法返回实例时。

- 允许一个类被继承，意味着其服务对象除了普通客户之外，__又增加了特殊的客户－继承者__，谨防继承带来的封装的破坏性。保证`protected`方法成员的规范性和稳定性，防止覆盖的副作用。

- 如果`Stack`类真正为后代着想，也不至于让子类无所适从，`push`与`multiPush`之间的依赖关系，__本属实现细节，但为子类计，应明确地规范化__，

- 还可以在设计上下功夫，实现语义与语法双重防护，即借用C++中 __非虚接口__(Non-Virtual Interface，简称NVI)模式：将公有函数非虚化，将虚函数私有化
    - C++中private虚函数可以被覆盖，但Java和C#中不能，后二者在此模式中采用protected虚函数
    - 一个方法，如果是公有的就不要是多态的；如果是多态的，就不要是公有的

- 在`Stack`类改进版1中，它的`push`方法实际有两个功用：一方面，它为外界提供服务，属于 __公开接口__；另一方面，它为自身提供服务－被`multiPush`调用，属于 __内部实现__，一人分饰二角，如果仅局限于`Stack`类本身，问题不大，问题在于`push`是多态的，有可能被子类替换，而 __子类的`push`却没有同时胜任两个角色的义务__。所以另一个解决方法是，将`push`内外职能分解：对内的部分多态而不公有，对外的部分公有而不多态(NVI模式)：

<!--language: java-->

    public interface Stack<E>{
        public void push(E item);
        public void multiPush(E... items);
        public E pop();
        public E peek();
    }

    // java.util.Stack的改进版5(NVI模式)
    public class DefaultStack<E> implements Stack<E>{
        protected void doPush(E item){ //子类覆盖点
            data.addElement(item);
        }

        final public void push(E item){ //子类不可覆盖
            doPush(item);
        }

        final public void multiPush(E... items){ //子类不可覆盖
            for(E item : items)
                data.addElement(item);  // 或 push(item)
        }

    }

    // 记录压栈总次数栈(NVI模式)
    public class CountingStack<E> implements DefaultStack<E>{
        private int pushCount;

        @Override public void doPush(E item){
            ++pushCount;
            super.doPush(items);
        }

        pubic int getPushCount(){
            return pushCount;
        }
    }

即使不看文档，也能从`doPush`的多态和非公有的特性看出它是压栈的唯一覆盖点，还能从`push`和`multiPush`的公有和非多态的特性推出它们必然最终依赖`doPush`，各自行使 __不同的职能__。

- 所以，当类的一个公有方法直接或间接地调用了自身的另一个多态方法，应特别谨慎。如果不能杜绝这类自用，则必须将其规范化，以免被子类错误的覆盖。此外，应尽量采用 __非虚接口模式__ 来 __分离接口与挂钩__：让该公有方法不是多态的(接口)，让该多态方法不是公有的(挂钩)
    - 挂钩即hook，低层父类调用了高层子类的方法


- 为了保证基类算法(流程)稳定性时，也不失必要柔韧时，可采用 NVI + __模板方法__(template method)：

<!--language: java-->

    // java.util.Stack的改进版6(NVI模式+模板方法)
    public class DefaultStack<E> implements Stack<E>{
        protected void doPush(E item){ //子类覆盖点
            data.addElement(item);
        }

        protected boolean beforePush(E item){return true;}//子类覆盖点

        protected void afterPush(E item){}//子类覆盖点

        final public void push(E item){ //主体流程，子类不可覆盖
            if(beforePush(item))
                doPush(item);
            afterPush(item)
        }

        final public void multiPush(E... items){ //子类不可覆盖
            for(E item : items)
                push(item); // 不可采用data.addElement(item);
        }
    }

模板方法可看作微型框架。

### 推荐的方法修饰符

通过上例，类的实例方法一般有4种用途，除内部挂钩外，其他最好非多态的：

- 为外界提供服务的公开接口
- 为子类提供扩展点的内部挂钩
- 为子类或包提供服务的内部接口
- 为自身提供服务的私有接口

<!--language: table-->

    |        |公开接口|内部挂钩              |内部接口   |自用|
    |--------|--------|----------------------|-----------|----|
    |访问控制|public  |protected/private(C++)|protected/package/internal|priave|
    |是否多态|非      |是                    |非         |非  |


### java与C#在对待重写时的区别
- Java中的实例方法 __默认是可覆盖的__，用`final`表示不可覆盖，而C++和C#正相反，__默认是不可覆盖的__，用`virtaul`表示可覆盖

- 可覆盖的方法有可扩展性，但也可能破坏封装，相反，不可覆盖的方法虽丧失部分灵活性，但同时也具备了稳定性和可靠性，还能节省时空上的开销，通过内联(inline)来带性能上的改善。

- Java和C#中在命名虚方法时，一般加前缀`on/On`


# 第10章 多态机制
=========

## 多态类型-静中之动


### 多态vs继承
- 继承的主要用途不是代码重用(合成才是)，而是代码被重用，这依赖于两个前提：
    - __语义__ 上遵循里氏代换原则
    - __语法__ 上支持 __多态__(polymorphism)机制

- 相对于静态类型语言而说，__继承是多态的基础，多态是继承的目的__
    - 鸭子类型是不依赖继承的多态

- 多态是动静结合的产物，将静态类型的 __安全性__ 和动态类型的 __灵活性__ 融为一体


### 多态的类型
- GP(泛型编程)中 __参数多态__(parametric polymorphism)
- OOP中的 __包含多态__(inclusion polymorphism)或称 __子类型多态__(subtyping polymorphism)

- 从 __实现机制__ 上看，二者不同之处在于何时将一个变量与其实际类型所定义的行为挂钩：
    - 前者在 __编译期__，属于早绑定(early binding)或静态绑定(static binding)
    - 后者在 __运行期__，属于迟绑定(late binding)或动态绑定(dynamic binding)

- 从 __应用形式__ 上看：
    - 前者是 __发散式__ 的，让 __相同实现代码__ 应用于 __不同场合__
    - 后者是 __收敛式__ 的，让 __不同实现代码__ 应用于 __相同场合__

- 从 __思维方式__ 上看：
    - 前者是泛型式编程风格，看重 __算法的普适性__
    - 后者是对象式编程风格，看重 __接口与实现的分离度__

- 都是在保证必要的类型安全的前提下，突破编译期间过于严苛的类型限制，保证代码灵活性、可维护性和可重用性

- 以上都是通用多态(universal polymorphism)，此外还有以下：
    - 强制多态(coercion polymorphism)，即一种类型变量在作为参数传递时隐式转换成另一种类型，比如整形变量可以匹配浮点型变量的函数参数
    - 重载多态(overloading polymorphism)，允许不同的函数或方法拥有相同的名字

### 模板方法vs策略模式

- __模板方法__ 突出稳定坚固的 __骨架__，__策略模式__ 突出是灵活多变的 __手腕__，授予客户自由选择算法的权力

一个可验证用户名和密码的类，使用模板方法：

<!--language: java-->

    abstract class Authenticator{
        final public void save(String user, String password){
            if(password == null)
                password = "";
            store(user, encrypt(password))
        }

        final public boolean authenticate(String user, String password){
            String storedPassword = retrieve(user);
            if(storedPassword == null) return false;

            if(password == null)
                password = "";
            return storedPassword.equals(encrypt(password));
        }

        protected abstract void store(String user, String encryptedPassword);
        protected abstract String retrieve(String user);
        protected String encrypt(String text){return text;}
    }

使用策略模式：

<!--language: java-->

    interface KeyValueKeeper{
        public void store(String key, String value);
        public String retrieve(String key);
    }

    interface Encrypter{
        public String encrypt(String plainText);
    }

    class Authenticator{
        private KeyValueKeeper keeper;
        private Encrypter encrypter;

        public Authenticator(KeyValueKeeper keeper, Encrypter encrypter){
            this.keeper = keeper;
            this.encrypter = encrypter;
        }

        public void save(String user, String password){
            if(password == null)
                password = "";
            keeper.store(user, encrypter.encrypt(password))
        }

        public boolean authenticate(String user, String password){
            String storedPassword = keeper.retrieve(user);
            if(storedPassword == null) return false;

            if(password == null)
                password = "";
            return storedPassword.equals(encrypter.encrypt(password));
        }
    }

当存储与加密的方式各有多种，以第一种方式需要M*N个实现类，而后者只需要M+N个实现类，而且后者的实现类职责更单一，方便被重用

![prog_paradigm_template](../../pics/prog_paradigm_template.png)

![prog_paradigm_strategy](../../pics/prog_paradigm_strategy.png)


接下来使用泛型，采用C++模板的方式：

<!--language: cpp-->

    #include <string>
    #include <map>

    using namespace std;

    template<typename KeyValueKeeper, typename Encrypter>
    class Authenticator{
        private:
            KeyValueKeeper keeper;
            Encrypter encrypter;
        public:
            void save(const string& user, const string& password){
                keeper.store(user, encrypter.encrypt(password));
            }
            bool authenticate(const string& user,
                              const string& password) const{
                string storedPassword;
                if(!keeper.retrieve(user, storedPassword))
                    return false;

                return storedPassword == encrypter.encrypt(password);
            }
    };

    class MemoryKeeper{
        private:
            map<string, string> keyValue;
        public:
            void store(const string& key, const string& value){
                keyValue[key] = value;
            }

            bool retrieve(const string& key, string& value) const{
                map<string, string>::const_iterator itr
                  = keyValue.find(key);
                if(iter == keyValue.end()) return false;

                value = iter->second;
                return true;
            }
    };

    class PlainEncrypter{
        public:
            string encrypt(const string& plainText) const{
                return plainText;
            }
    }

    int main(){
        Authenticator<MemoryKeeper, PlainEncrypter> authenticator
          = Authenticator<MemoryKeeper, PlainEncrypter>();
    }

与策略模式代码很相似，主要区别是把接口变成了模板参数，由于在编译期间实例化，因此没有动态绑定的运行开销，缺点是不能动态改变策略

## 抽象类型-实中之虚

### 抽象类型vs具体类型
- 具体类型是创建对象的模板，抽象类型是创建类型的模块，一个是为对象服务的，一个是为类型服务的

- 前面用户认证的例子，模板方法模式中的`Authenticator`类是抽象的，是为了创建子类型；策略模式中的`Authenticator`是具体的，是为了创建对象，但它合成的两个接口又是为了创建算法类型服务的

### 抽象类型vs抽象数据类型
- 抽象数据类型(ADT)的核心是数据抽象，抽象类型的核心是多态抽象

### 抽象类型的种类
- 在Java和C#中只有 __接口__ 和 __抽象类__ 两种
- C++中这两种类型没有显式区别
- 动态的OOP语言，如Ruby，Samlltalk还支持 __mixin__ 和trait中的一种类型，它们的出现为了弥补接口与抽象类的一些不足，更好的实现 __代码重用__：
    - 接口的主要目的是创建多态类型，本身不含任何实现，子类型通过接口继承只能让代码 __被重用__，却无法重用超类型的实现代码。抽象类可以重用代码，可又有多重继承问题
    - 为什么不用合成？合成用法不简便；合成不能产生子类型，而有时这正是所需的；合成无法覆盖基础类的方法；合成的基础类只能是具体类型，不能是抽象类型（最大缺点，用它作抽象类型，就担任了两个职责）

### mixin
- 一种可重用的模块，既不像具体类型那样面面俱到，又次像接口那样有名无实，也没有抽象类多重继承之弊
- Ruby中的Comparable,Enumerable
- 主要特点：
    - 抽象性和依赖性：本身没有独立存在的意义，必须融入主体类型才能发挥作用
    - 实用性和可重用性：不仅提供接口，还提供部分实现
    - 专一性和细粒度性：提供的接口职责明确而单一
- 可选性和边缘性：为主体类型提供非核心的辅助功能
- Java和C#可借助AOP来实现mixin，C#的扩展方法也算对mixin的支持
- C++借助模板CRTP(Curiously Reurring Template pattern)惯用法实现mixin

一个C++防止被复制的类，不适用于Java和C#：

<!--language: !cpp-->

    class NonCopyable{
        protected:
            // 非公有构造函数 防止创建对象
            NonCopyable(){}
            // 非公有虚析构函数 建议子类非公有继承
            ~NonCopyable(){}
        private:
            // 私有复制构造函数 防止直接的显式复制和通过参数传递的隐式复制
            NonCopyable(const NonCopyable&);
            // 私有赋值运算符 防止通过赋值来复制
            const NonCopyable& operator=(const NonCopyable&);
    };

    // 私有继承
    class SingleCopy : private NonCopyable{};

    int main(){
        SingleCopy singleCopy1;
        SingleCopy copy(singleCopy1); //编译器报错
        SingleCopy singleCopy2;
        singleCopy2 = singleCopy1; //编译器报错
        return 0;
    }

有些对象不希望被复制，如网络连接、数据库连接的资源对象，它们的复制要么意义不大，要么实现困难。C++编译器为每个类提供了默认的复制构造函数(copy constructor)和赋值运算符(assignment operator)，想阻止对象的复制，通常是将两个函数私有化。虽然`NonCopyable`从语法上说不是抽象类，但本质上是一种类似minin功能的抽象类型

为什么Java中没有类似的对象复制？参考后面的[按值传递与按引用传递](#TOC11.1.3)

### 接口与抽象类区别

- 在语法区别，以下适用于Java和C#

<!--language: table-->

    |                      |抽象类|接口                              |
    |----------------------|------|----------------------------------|
    |提供实现代码          |能    |否                                |
    |多重继承              |否    |能                                |
    |拥有非public成员      |能    |否                                |
    |拥有域成员            |能    |否(Java中的static final域成员除外)|
    |拥有static成员        |能    |否(Java中的static final域成员除外)|
    |拥有非abstract方法成员|能    |否                                |
    |方法成员的默认修饰符  |-|public abstract(Java可选，C#不能含任何修饰符)|
    |域成员的默认修饰符    |-     |Java:public staic final。C#:不允许域成员|


- 抽象类的意义在于：父类推迟决定，让子类选择实现方法。推迟，即提供动态节点，如果是具体类型，节点已固定。反过来，要节点动态化，一般通过多态来实现，所以，抽象类型常与多态机制形影不离
- 接口类型对应是接口规范，接口继承不是为了重用，而是为了被重用
- 抽象类的出现，让具体类描述对象，重在实现，抽象类描述规范，重在接口，降低了用户与实现者之间耦合度，减少代码维护成本及编译时间
- 具体类、抽象类和接口分别对应于模具、模具半成品和模具规格
- Java既规范了Collection,Set,List,Map等接口，又为这些接口提供了抽象类和具体类，方便程度递增而灵活度递减

- “接口是为了克服Java或C#中抽象类不能多重继承的缺点”，这句话很有误导性，接口甚至连单重实现继承都做不到

- 在语义区别

<!--language: table-->

    |     |抽象类  |接口    |
    |-----|--------|--------|
    |关系 |is-a    |can-do  |
    |共性 |相同种类|相同功能|
    |特征 |核心特征|边缘特征|
    |联系 |纵向联系|横向联系|
    |重用 |代码重用|规范重用|
    |实现 |多级实现|多种实现|
    |重点 |可扩展性|可置换性|
    |演变 |新增成员|新增类型|

- 相同的接口代表相同的功能，多表示'can-do'关系，常用后缀'-able'的形容词命名，如`Comparable`,`Runnable`,`Cloneable`；接口一般描述的是对象的边缘特征，或说是一个对象在某一方面的特征，因此能在本质不同的类之间建立起横向联系。

- 接口一旦被采用，它的任何改动－包括增减接口、修改接口的签名或规范，将涉及整个系统，必须慎之又慎；而抽象类演变则没那么困难，可随时新增域成员或有默认实现的方法成员（前提是不与子类型的方法名发生冲突），所有子类将自动得以扩充

- 接口虽然自身难以演化，但很容易让其他类型演化为该接口的子类型，JDK5.0之前，`StringBuffer`,`CahrBuffer`,`Writer`,`PrintStream`本是互不相关的，在引进了接口`Applenable`并让其上类实现该接口，它们便有了横向联系，均可作为格式化输出类`Formatter`的输出目标。

### 标记接口
标记接口，一个方法也没有，也谈不上规范，也无法利用多态机制，继承这类接口的意义何在？

- 一个类型的规范不限于单个的方法，类型整体上也有规范，比如主要目的、适用场合、限定条件、类不变量等

- 接口是一种类型，有严格的语法保障和明确的语义提示，让一个具体类型继承特定接口中，凸现了设计得用意，比如`java.util.EventListener`接口为所有的事件监听提供了统一的类型

- 有时需要对某些类型提出特殊要求、提供特殊服务或进行特殊处理，而这些并不能通过公有方法来办到，也没有其他有效的语言支持时，标记接口可担此任，成为 __类型元数据__(metadata)的载体，比如给一个类贴上`java.io.Serializable`，它的对象便能序列化，具体工作由JVM来完成。
    - 当标记接口仅用于元数据时，更好的办法是采用属性导向式编程，Java中的annotation，C#中的attribute


### OOP的规范抽象

- __个人身份__ 对应的规范抽象借助 __封装__，以数据抽象的形式出现，让每个类成为独立的模块，有显著的外在行为和隐藏的内在特性
- __家庭身份__ 对应的规范抽象借助 __继承__，以类型层级的形式出现，使一个类成为其他类的子类或父类，确立了对象在类型家族中的身份
- __社会身份__ 对应的规范抽象借助 __多态__，以多态抽象的形式出现，更关心的是职责和角色，不以具体类型出现，而是在不同场合下以不同抽象类型的身份出现。一般具体类型在公共场合是不为人知的，只有少数核心库里的核心类是例外，社会身份则不然，远比个人身份更容易被接受，它有如下特点：
    - 独立而稳定：先于个体而存在，且不随个体的变化而变化
    - 公开而权威：为人所知、为人所信
    - 规范而开放：制定的协议标准明确，且允许个体在遵守协议的前提下百花齐放
    - 相同身份的个体可相互替换、新型个体可随时加入，而且不会影响整体框架和流程，保证了系统的灵活性和扩展性；
    - 整体不因某一个体的变故而受冲击，保证了系统的稳定性和可靠性；
    - 个体角色清晰、分工明确，保证了系统的规范性和可读性




# 第11章 值与引用
=========
## 语法类型-体用之分

- 值与引用因其天生的对立性：
    - 把数据分成两类：值，具有类型的数据；引用，可用来获取特定数据的值
    - 把变量分成两类：值变量(value variable)，表示值的变量；引用变量(reference variable)，表示引用的变量
    - 把数据类型分成两类：值类型(value type)，能直接被访问的数据类型；引用变量(reference type)，借助引用才能被访问的数据类型
    - 把对象分为两类：值类型对象，引用类型对象

- C++ __没用__ 引用类型，但有&引用和*指针，具有引用功能，属于 __引用端__ 的引用，而C#/java中引用类型是指 __被引用端__ 的


### 语言中内存分配机制
- 按灵活递增依次为：__静态分配__(static allocation)、__栈分配__(stack allocation)、__堆分配__(heap allocation)

- 静态分配发生在 __编译期__，为全局变量、静态变量、常数变量等安排空间
- 栈分配和堆分配均发生在 __运行期__，但前者一般在 __编译期就可确定待分配内存的空间大小和生命周期__(例外，C的alloca允许程序员在栈上分配动态大小的内存，但由编译器释放)，后者则可能推迟到运行期

- 栈内存区主要用于存储 __局部变量__(local variable)或 __自动变量__(automatic variable)
- 堆内存区用于存储new运算符（C#中new结构体分配在栈上）、malloc函数等 __动态分配__ 而得的空间


- 栈分配效率高，通常每个线程都有独立的栈区，故栈变量 __天然是线程安全__(thread-safe)的，栈分配主要缺点须预知分配内存，而且栈区总容量有量，容易发生溢出(stack overflow)。栈内存的静态有效期，优点是 __无需担心内存管理__ ，缺点是 __无法突破作用域__(scope)

- 堆分配更强大，更灵活，但复杂的算法影响了时间效率，内存碎片(memory fragmentation)、元数据开销(metadata overhead)和可能的内存泄露等问题也影响了空间效率，此外程序员还需要担负更多的 __内存管理、线程安全__ 等方面的责任。

<!--language: table-->

    |        |栈                            |堆                              |
    |--------|------------------------------|--------------------------------|
    |分配方式|空间大小和生命周期在编译期决定|空间大小和生命周期可在运行期决定|
    |释放方式|自动释放                      |手动释放或通过垃圾回收          |
    |总容量  |较小                          |较大                            |
    |生命周期|较短                          |较长                            |
    |时空效率|较高                          |较低                            |
    |线程安全|是                            |否                              |


### 值类型与引用类型内存分配

- 因为 __堆分配的动态性__ ，分配内存的具体位置和大小一般不能在编译期确定，故不能用值变量，而须 __通过引用变量__ 间接表示
    - "值变量分配在栈中，引用变量分配在堆中？"，堆内存须要引用变量来表示，并不表示引用变量本身也在堆中，不要把引用变量与被引用的对象混为一谈

- 一个变量是在栈中还是在堆中，与它是值变量还是引用变量 __毫不相干__：
    - Java中，局部变量总在栈中，实例变量(instance variable)总在堆中
    - C++中，可为栈上的对象创建引用，C++只有引用类型的变量，却没用引用类型的对象，更保险说法是：引用类型对象总在堆中

- 一个C#/java的方法中：

<!--language: csharp-->

    someType a = new someType();//someType为class

那么，`someType`是引用类型，`a`是引用变量，`a`也是局部变量，所以 __分配在栈__ 上，但它 __指向的对象分配在堆中__，`a`出了作用域就被编译器回收，而它指向的对象则被GC回收

上面语句相当于两条语句：

<!--language: csharp-->

    someType a;         //将a分配在栈上
    a = new someType(); //在堆上分配了一个对象并将其引用赋值给a

![prog_paradigm_ref](../../pics/prog_paradigm_ref.png)

- Java中除了基本类型是值类型外，其他的应该都是引用类型，C++则无法自定义引用类型，却可以创建引用，C#则既可以自定义引用类型，也可以自定义值类型（如上代码，someType是struct的话，则不在堆中，直接在栈上分配一个对象）

- C++中:

<!--language: cpp-->

    someType* a = new someType(); //分配在堆上  同java相似，只是多了*
    someType a = someType();      //分配在栈上  或简单的写成someType a;

分配在堆中做法与Java类似，只是多个表示指针的`*`，分配在栈上做法与C#值类型类似，只是少了`new`

- 值变量与引用变量的区别不在于它们存放地点，而在于它们存放内容：数据或地址；在于它们获取目标数据的方式：直接或间接；在于它们存放目标数据的方式：在线或离线。

- __值重在价值，引用重在使用价值__；值重在体，本体之体；引用重在用，功用之用

### 按值传递与按引用传递
在函数调用中，需要参数传递，即有一个实际参数(actual parameter)映射到形式参数(formal parameter)的过程，最常见有两种机制：

- 按值传递(pass-by-value/call-by-value)，函数收到是实际参数值－按位拷贝(bitwise copy)
- 接引用传递(pass-by-reference/call-by-reference)，函数收到的是实际参数的引用－内存地址

- java/javascript中其实 __只存在 *按值* 传递 *引用对象*__ (也称为按共享传递)：

<!--language: !java-->

    import java.io.*;
    public class TestPassByRef{
        static void change(String str){str="new value";}
        public static void main(String[] args){
            String s="old value";
            change(s);
            System.out.println(s);
        }
    }

如果Java真按引用将`String`变更传递给`change`，输出将是'new value'，但实际却输出'old value'。change的实际参数`s`是一个`String`引用，而不是`String`对象，让变量`s`换指另一个对象，__并不能改变它当前所指对象__

java按值传递对象的引用，并不支持按引用传递，因为所有对象都是引用类型，对象本身并不会被复制

- C++的引用传递（如果只是为了共享数据，防止被意外改变，需加const）

<!--language: cpp-->

    static void change(String& s){//传递对象的引用
        s = "new value";
    }

- C#的引用传递

<!--language: csharp-->

    static void change(ref string s){//传进对象引用的引用
        s = "new value";
    }

- Java为什么不像C++或C#那样支持按引用传递呢？一方面保持语言的简单性，另一方面，按引用传递最大好处是避免产生临时的复制对象，在时空效率上通常优于按值传递。但Java和C++/C#不同，__所有对象都是引用类型的，对象本身不会被复制__，因此没有按引用传递也无大碍

- 换句话说，按引用传递是为值类型对象而设计的，避免产生临时的复制对象

- 当一个对象给一个变量赋值或作为参数 __按值传递__ 时，在C++中复制的是该对象的值，而java中复制的却是该对象的引用，因此，C++有专门的 __赋值运算符__ 和 __复制构造函数__，而java没有，java要达到复制对象值的目的，不能隐式的通过变量赋值或参数传递，只能显式的重新构造对象或clone/serilaization，这即是值语义(value semantics)与引用语义(reference semantics)的区别

- java的按值传递无法解决：改变引用变量的目标对象，改变基本类型的值，将两个参数互换，同时返回多个计算值

### 值类型与引用类型优缺点
- 值类型是引用类型的构建基础，访问数据时，少一次内存寻址；创建单个实例时，少占一个引用空间，少作一次内存分配；创建数组时，差异更明显，引用类型不仅花更多的分配时间，由于分配空间的不连续性，还可能造成更多的内存碎片，一方面浪费了空间，另一方面浪费读写时间（违背局部性原因造成的）。采用值类型有可能利用栈的优势，提高程序性能，减少程序员负担

- 引用类型，因间接性和抽象性带来更大灵活性，避免一些不必要的值拷贝，从而提高效率；当一个对象须要在多处共享同变时，更离不开引用；类似链表、二叉树等这样的结构，没有引用也是难以实现的；引用允许空值null；引用不仅是堆分配的必要工具，同时还是多态的前提条件

### 多态必须引用才能实现
- C++中一个对象必须通过指针或引用才能表现出多态特征，而C#中的值类型干脆不允许被继承
- 对一个不通过引用而被直接操作的对象来说，多态是不必要的，它的具体类型在编译期间就已确定，多态是不可能的，分配给它的空间无法容纳通常更大的子类型对象（C++产生对象切割(object slicing)，子类型对象中的额外信息，如虚表指针VPTR和新的实例成员被丢失）

### 值与引用对比表格

<!--language: table-->

    |    |存储内容|逻辑指代|实际数据|价值属性|对象复制|对象共享|实现多态|空值|
    |----|--------|--------|--------|--------|--------|--------|--------|----|
    |值  |数据    |直接    |在线    |内在价值|赋值    |否      |否      |无  |
    |引用|地址    |间接    |离线    |使用价值|克隆    |能      |能      |有  |


## 语义类型-阴阳之道

### 值语义与引用语义
值语义的对象是 __独立__ 的，引用语义的对象却是 __允许共享__ 的

<!--language: cpp-->

    // 值变量v1与v2互相独立
    ValueType v1 = someValue;
    ValueType v2 = v1;

    // 引用类型r1与r2互相关联
    ReferenceType r1 = someObject;
    ReferenceType r2 = r1;

两个值变量`v1`和`v2`尽管有相等的值，但各自有独立的存储空间，互不干扰，改变其中一个不会影响另一个；相反两个引用变量`r1`和`r2`由于共享同一对象发生关联，对象的状态改变同时反映到两个变量上，即别名(aliasing)效应

<!--language: cpp-->

    // 值类型ValueType具有引用语义(C++)
    ValueType v1 = someValue;
    ValueType& v2 = v1;  // 方法1: 通过引用让v2成为v1的别名
    ValueType* v3 = &v1; // 方法2: 通过指针让v3指向v1

    //让引用类型的ReferenceType具有值语义(假定它是Java中的Cloneable类)
    ReferenceType r1 = someObject;
    ReferenceType r2 = (ReferenceType)r1.clone();//避免r1与r2共享同一对象

在不影响程序正确性的前提下，一个对象的复件能否代替原件？如果是肯定的，则类型是值语义，否则是引用语义的，当然，这与语法无关，完全取决于设计者的意图

### 从几种范式看

- 从命令式编程角度看，一个值语义变量的内存地址是无关紧要的，原件与复件的唯一差别在被消除后变得完全等价，因而值语义又称复制语义(copy semantics)。相对的，引用语义变量的内存地址至关重要，通常用指针来实现，因而引用语义又称指针语义(pointer semantics)
- 从函数式编程的角度看，值应当是引用透明的(referential transparency)，即一个表达式随时可被其值替换，比如`2+3`总是可用`5`来代替（事实上，一般编译器在编译期间就将常量表达式替换为它的值了），显然值的可替换性抹煞了引用的作用
- 从对象式编程的角度看，值语义与引用语义的区别在于对象标识(object identity)的重要程度
    - 对象标识是一个对象区别于其他对象的唯一的标识，反映了一个对象作为一个实体的独立性、可识别性和本体性，是 __对象的3大特征__ 之一（状态state、行为behavior、标识identity）
    - 一个对象的标识在程序中没有实际意义或没有标识，意味着它的对象特征模糊、主体意识淡薄，更多的代表的是种抽象的属性，而非具体的实体，则它具有值语义，反之，则具有引用语义


- 值通过具体的数据来描述抽象的属性，如需描述某个物体，通过值为表示它的形状、大小、重量、颜色等属性值，这些数据虽然是具体的，但描述的属性却是抽象的

- 引用通过抽象的方式来指代具体的实体，如指定某个物体，那么所指的对象是具体的，至于如何获得对象则属于实现细节，可认为是抽象的。

- Java和C#中`String`类虽然是引用类型，但它们的值语义很明显，关心的内容，而不是地址，所以使用`==`来判断字符串异同就不合适，应使用`equals`方法，C#干脆重载了相等运算符

### 值语义的不可变性

- 值不依赖于内存地址的，即具有 __空间无关性__，还具有 __时间无关性__，即一个值语义的对象在其生命周期中的状态是固定的，值语义类型一般是 __不可变的__(immutable)

- 不可变性加强了值语义

<!--language: java-->

    String s1 = "ab";
    String s2 = s1;
    assert (s1 == s2);  // s1与s2指向同一对象
    s2 += "cd";
    assert (s1 != s2);  // s1与s2指向不同对象

- 如果`String`是可变的，则`s2`内容变化后，`s1`的内容也会发生相应的变化，就会产生别名问题，通常并非我们想要的结果，所以当s2自增后，系统便让它换指另一个字符串对象，而原先的字符串对象仍旧保持不变，省去了类似先前引用类型那样显式的克隆过程。__不可变性为引用类型贯彻值语义提供了变通的语法支持__，如java中还有Integer/Boolean等

- Java中StringBuffer与C#中StringBuilder一样，角色定位不是字符串的拥有者，而是字符串的创造者，故而是可变的，并不具备值语义

- 日期类型应是值语义的，但Java中的`Date`类是引用类型，并且还是可变的，要实现其值语义，就需要在日期传递或赋值过程中进行防御性复制(defensive copying)

- C++的`string`类是值类型的，对不可变性须求没有那么强烈，但还应尽可能保持字符串对象的不可变性，除了在赋值、按值传递、作为返回值等时凭借值类型的特点来保证字符串的复制以外，还可以 __利用`const`来保证常量的正确性__(const-correctness)

### 装箱与拆箱
- Java和C#中的基本数据类型是值类型的，但有时需要以引用类型的身份出现，就需要一个转化过程，术语称 __装箱__(boxing)，逆过程称为 __拆箱__(unboxing)
    - C++可以利用指针直接为变量创建引用，不需要这类转化
- 装箱后的对象虽然是引用类型的，但仍保持值语义，因此应该是不可变的


### 对象的改变方式

- 值语义对象的改变是一种 __新旧更替__，新对象替换了旧对象，只是凑巧重用了后者所占用的内存空间；
- 引用语义对象的改变是一种 __自我更新__，即一个对象在保持同一性的前提下发生的状态迁移或属性改变
- 值语义的复件具有等效性，而引用语义的复件不具等效性
    - C++中的[NonCopyable](#TOC10.2.4)基类就是防止引用语义的类型的复制

### 值对象与DTO

- 数据传输对象(Data Transfer Object, DTO)多用在基于多层架构的分布式系统中，由于不同层之间的通讯通常会跨越进程或网络边界，相对昂贵的而缓慢的远程调用往往成为性能瓶颈，为了减少通讯次数，每次应返回尽可能多的数据，将它们打包构造一个 __粗粒度__ 的数据集合。

- 从实际用途角度，传输对象主要目的是携带多项数据以减少网络开销，值对象主要是描述其他对象
- 从内容传递角度，传输对象关心序列化，值对象重视值语义而更关心赋值问题
- 从抽象程度角度，传输对象是简单的数据载体，数据透明度高，内聚度低，不具抽象性，
- 从可变性角度，传输对象有时是可变的，需要多步填充或多次更新，值对象认为不可变性是一个尽量遵守的原则
- 传输对象在实践中常与业务对象相提并论
- 引用对象是数据、运算、标识三位一休，值对象则被抽去了标识，而传输对象连运算也被抽去了
- 传输对象的焦点在于“有什么”，值对象的焦点在于“是什么”，而引用对象的焦点在于“是哪个”

### 对象建模
- UML建模中，值对象多用于属性，引用对象多用于关联，值对象是附属的，被动的，引用对象是独立的，能动的

- 合成是基于值语义的包含，聚合是基于引用语义的包含

- 限制外界对被合成对象的访问，并控制它的生命周期

- 从关联(has-a)到聚合(owns-a)再到合成(contains-a)耦合度逐步加大。聚合在关联的基础上增加了"整体与部分关系"的语义，合成在聚合的基础上增加了"所有权垄断"和"生存权垄断"的语义

- 类图上的关联暴露内部结构和实现细节吗，岂不破坏了封装、违背了信息隐藏原则？
    - 设计与实现阶段，需要信息隐藏，屏蔽容易变化的部分，以提高软件的应变能力
    - 分析和设计阶段发现并建立的关联是本质的，描述的是系统中本质类的本质属性、本质行为，以及类与类之间的本质关系，因而也是相对稳定和不变的，隐藏这类对增强系统的应变性和灵活性意义不在

- 为了达到 __抽象__ 的目的：
    - 实现级别的信息需要 __隐藏__，靠的是 __访问控制__
    - 设计级别的信息需要 __过滤__，靠的是 __抽象建模__


