# 第12章 设计原则
=========

## 间接原则-曲胜于直

- "Any probolem in computer science can be solved with another layer of indirection"任何计算机问题均可通过增加一个间接层来解决

- 像文件路径、URI、DB中的外键、程序中的变量等都具有间接指代作用

### 抽象与间接性
- 抽象层应当具有间接性，否则就不成其为“层”了

- 间接层也应当为抽象性，它应当是一个标准，标准是一系列 __规范__ 的集合，而规范为显化的抽象

- 一个适当的中间层，在 __形式上__ 表现为间接层，在 __实质上__ 体现为抽象层

- 应用程序一般无权直接与硬件打交道，只能通过操作系统的kernel间接的访问底层资源
- 网络协议的OSI 7层模型，每一层都有特定职能，一面向上层提供服务，一面向下层寻求服务，上层只能依赖于相邻的下层
    - 上层只能依赖于相邻的下层时称为封闭架构(closed architecture)或不透明分层(opaque layering)。如果上层能直接请求非相邻下层有服务，则称为开放架构(open architecture)或透明分层(transparent layering)

- 类的API，接口等都是间接层
    - 封装带来的接口与实现的分离，使得类的API成为间接层，完成数据抽象
    - 继承和多态带来的接口与实现的分离，使得类的超类型成为间接层，从而将数据抽象升级为多态抽象

- 间接、分离、抽象、规范，应认为OOD的不二法门，它们相生相依，互为因果

### MVC与三层架构
MVC其实与三层架构是有区别的：

![prog_paradigm_mvc_cui](../../pics/prog_paradigm_mvc_cui.png)

- 实线是直接依赖，在MVC中，视图只是在初始化时，注册到模型中，然后模型的变化自然视图会得到响应，下面是它的序列图，可看出控制器为中间层：

![prog_paradigm_mvc_cui_seq](../../pics/prog_paradigm_mvc_cui_seq.png)

- MVC作为架构主要用于交互式系统，如GUI或web的前台(front-end)应用，而三层架构通常是最高层的架构设计，多出现在企业级应用中

> 企业级应用(enterprise application)通常包含专业领域的商业逻辑，系统的业务需求和应用环境更为复杂多变，常以多用户、大数据量、分布式、高并发、高集成、跨平台为特征，对软件质量的要求相应也更高，不仅需要保证包括可扩展性(extensibility)、可维护性(maintainability)、可重用性(reusability)等在内的 __设计质量__，更对包括安全性(security)、可靠性(reliability)、可伸缩性(scalability)、性能(performance)、可获性(availability)、互用性(interoperability)等在内的 __运行质量__ 有着严苛的要求

- 三层架构中的'层'，如果指layer，则对 __系统逻辑__ 上的划分，有助于提高软件的 __设计质量__ ；如果指tier，则是对系统进一步的 __物理上__ 划分，即不同的层运行于不同的服务器上，有助于提供软件的 __运行质量__

- 三层架构与MVC并不是对立的，后者常嵌入前者的表现层：

![prog_paradigm_mvc_3layer](../../pics/prog_paradigm_mvc_3layer.png)


- 其它参考[从MVC框架看MVC架构的设计](http://blog.csdn.net/bluishglc/article/details/6690693)


## 依赖原则-有求皆苦,无欲则刚

### 依赖反转原则

间接原则有一个直接的推论，就是依赖反转原则(DIP)

![prog_paradigm_dip](../../pics/prog_paradigm_dip.png)

- 接口一方面作为一种抽象类型，描述了一类对象所应遵循的行为规范；另一方面作为间接层，把两个耦合的具体类进行了分离

- 高层模块与低层模块往往是 __决策__(policy)与 __机制__(mechanism)之间的关系，高层决策以底层机制为基础，逻辑上似乎也合理，但在软件设计上，这种依赖是非常不合理的，因为依赖通常具有 __传递性__(transitive)
    1. 一旦低层模块发生变化，很可能波及客户代码
    1. 即使低层模块不变，高层模块往往也会在不同的环境下选取不同的低层模块，它的可维护性和可扩展性受到限制，而高层模块的可重用性和可移植性(protability)也降低

### 稳定依赖原则
- 依赖反转原则依赖于抽象层，因为抽象层更稳定，因此DIP __可重述__ 为 __稳定依赖原则__(Stable Dependencies Principle, SDP)，模块应 __朝着稳定__ 的方向依赖

![prog_paradigm_dip_jdbc](../../pics/prog_paradigm_dip_jdbc.png)

- 原本是高层围绕低层，现在高层制定一套规范，反过来要求低层去遵守，DIP与 __针对接口编程__ 原则一脉相承

### 合成重用原则
- 继承关系固然是一种依赖关系，合成也是，但合成比继承的优势是它是隐性的、动态的，使得在合成类与被合成之间插入了 __抽象层__ 的可能
- 当抽象层利用了多态机制时，这种合成称为 __多态合成__(polymorphic composition)
- __合成重用原则__(Composite Reuse Principle, CRP)，即多态合成优于（实现）继承(favor polymorphic composition over inheritance)

<!--language: !csharp-->

    using System;
    class Rect{
        public double width{get; set;}
        public double height{get; set;}
        public double getArea(){
            return width * height;
        }
    }

    class Window{
        public Rect rect{get; set;}
        public double getArea(){
            return rect.getArea();
        }
    }

    class Program{
        static void Main(string[] args){
            Window win = new Window{rect = new Rect{width=3, height=4}};
            Console.Write(win.getArea());
        }
    }

上面的尽管合成比继承好，但仍有局限性，如果窗口不一定是矩形，可能是圆角矩形，这时候间接原则指导下的多态合成就有了用武之地

<!--language: !csharp-->

    using System;
    interface Shape{
        double getArea();
    }
    class Rect : Shape {
        public double width{get; set;}
        public double height{get; set;}
        public double getArea(){
            return width * height;
        }
    }

    class Window{
        public Shape shape{get; set;}
        public double getArea(){
            return shape.getArea();
        }
    }

    class Program{
        static void Main(string[] args){
            Window win = new Window{shape = new Rect{width=3, height=4}};
            Console.Write(win.getArea());
        }
    }

上面就是运用了DIP原则，更具体的说是DI模式

![prog_paradigm_di](../../pics/prog_paradigm_di.png)

- 非采用DI模式时，Window负责创建它所依赖的Rect对象，如果用C++编程，还须负责销毁该象。而采用DI模式时，Window类所依赖的Shape对象是从外部注入的，放弃了对生者生杀大权，但复杂度更低，专注度更高，依赖性更弱，灵活性更强

- 所以DI的要点：少一份责任则多一份专注，内聚度随之增加；少一份权力则多一份自由，耦合度随之降低

- 采用DI模式后，通常会安排专门的模块，如供应者(provider)或装配器(assembler)来管理外来的依赖的对象，就是职责的一个分工，符合 __关注点分离__(Soc)原理
    - 可利用Spring,Guice等DI容器

### DI vs DIP
- 侧重点不同：DI强调依赖的来源－完全由 __外部提供__，该依赖自然最好是抽象的，但并非首要要求； DIP则强调依赖的 __抽象类__
- 应用范围不同：DI更加具体的策略，一般用类级别的模块，故为一种设计模式；DIP还可以用于类库、组件、架构层等大级别的模块，故为一种设计原则
- DIP并不能真正消除依赖，但改善了系统的依赖体系，使之更能经受需求和环境的变化

### 控制反转 vs 依赖反转

- 控制反转广泛应用于框架中，把控制权从用户的应用代码转给了底层某个组件
    - __依赖注射__ 是控制反转的一种实现方式，为Spring容器所采用
    - __依赖查找__(Dependency Lookup)是另一种实现方式，如EJB2.0容器所采用的Service Locator

- 控制反转与依赖反转的共同点是：通过引入抽象来摆脱对外界不必要的依赖性、争取对外界必要的控制权

- 控制反转更侧重保证组件的 __可重用性__，让低层为高层提供充分的服务（ __回调__）；依赖反转更侧重保证组件的 __可维护性__，让高层不受低层变化的影响（__两者都依赖于抽象__）

- 应用程序与底层库的关系看作 __控制与被控制__的关系，相应的反转被称为控制反转；把可重用模块与底层模块的关系看作 __依赖与被依赖__ 的关系，相应的反转被称为依赖反转

### 针对接口编程
- 接口是一种 __抽象__，因为它掩盖了实现细节
- 接口是一种 __规范__，因为它定义是服务契约
- 接口是一种 __间接__，是实现间接层的关键手段
- 接口可以产生 __分离__，规范与实现的分离、职责的分离、关注点的分离
- 接口作为解耦点，可以消除非本质的 __依赖__
- 接口作为反向 __控制__ 点，可以抽象地调用外部依赖而无须虑及它们的具体实现

- 抽象与规范与根本、间接与分离是手段、依赖与控制是关键、接口与服务是核心


## 内聚原则

### 耦合与内聚

- 耦合(couping)反映模块之间的关联程度，内聚(cohesion)反映模块内部的关联强度

![prog_paradigm_couping_cohesion](../../pics/prog_paradigm_couping_cohesion.png)

- 耦合不可完全被消除，我们要避免的是非本质的，不恰当的耦合，如内容耦合同，应通过信息隐藏来防止，公共耦合通过引入接口，但如果发现两个模块过于紧密，很可能应该合为一体

### 局部化
- 尽可能把密切相关的软件元素放在一起，让代码的物理紧密度与逻辑紧密度保持一致，称为 __局部化原则__(locality principle)

- 局部化可以增强代码的 __可读性和一致性__，结合语法机制还能减少代码改动所产生的影响范围
    - 全局变量之所以不被提倡，关键它的辐射过广而难以控制，可将其限制为静态全局变量（C++中只能在定义它的文件中被访问）和局部变量，或利用各种语法容器使其局部化（namespace,package,function,class,block,inner class）
    - 能作为实例成员的就不要做为类成员
    - 能作为局部成员的就不要作为实例成员
    - 能作为private的就不要成为protected或public

- 局部化是 __信息隐藏的前提__，如果不事先把相关成员局限在一个拥有访问控制能力的软件单位中，信息隐藏就无从实现
    - OOP语言比过程式语言多了一种支持局部化的机制，class，就多了一个优势

- 任何 __关注点分离__(SoC)的过程都伴随着局部化的过程，分离的目的是为了新形式的聚合

- 局部化原则还能导出 __DRY__ 原则，从而实现了单点维护

### 单一职责原则
- 如果局部化原则过于普适，还有一个更具体的内聚原则，__单一职责原则__(Single Responsibility Priciple, SRP)，主张一个类应当只有一个变更的理由

- 模块的每一种 __职责__ 既是一个 __关注的焦点__，也是一个潜在的 __变化点__，因而，职责、关注点与变化点是 __三位一体__ 的。如果一个模块包含所有元素（包括指令、数据的定义、其他模块的调用等）都在为完成同一个任务而工作，这种内聚称为 __功能内聚__(functional cohesion)，最理想的一种内聚

![prog_paradigm_srp](../../pics/prog_paradigm_srp.png)

- 但有时违背SRP是情有可原的，毕竟类的 __职责难以__ 被清晰的界定，关键还是看它们 __对变化的反应__
    - 涉及表现形式的职责与涉及逻辑的职责不好混为一体，因为它们变化方向总是正交的
    - 如果发现两个职责部是联动，改变其中一个会影响另一个，看似关联不大，也能放在一起

- 高层类 __抽象层次也高__，忽略的细节多，相应的 __职责粒度大__，并不比底层更难遵守SRP

### 接口隔离原则

- 如果某一些违背了SRP，却因某种原因暂时不便修改，还可用ISP－__接口隔离原则__(Interface Segregation Principle, ISP)来弥补

![prog_paradigm_isp](../../pics/prog_paradigm_isp.png)

- 重构后（考虑过渡期的重构）不会影响老客户，但以后的新客户不再直接依赖`ShoppingService`转而依赖3个新的接口

- 接口隔离原则主张，__不应强迫客户依赖那些它们不用的方法__。
    - 比如NewClient1只关心支付验证，NewClient2只关心购物信息，NewClient3只关心订单序列化。
    - 一旦有关支付验证发生变化，NewClient2，NewClient3就不会受影响

- SRP保证了类的高内聚，ISP保证了接口的高内聚，两者同时为低耦合作出了贡献，

## 保变原则

- 保变(Protected Variations)原则，保变是指受保护的变化，该原则的内容是：找出预计的变化点或不稳定性，分配其职责以便用稳定的接口来包装

### PV vs SRP
- 保变着眼倾向变化，侧重 __解决耦合问题__，单一职责着眼倾向职责，侧重解决内聚问题

- 事实上越是接近客户需求前沿的类，越容易发生变化，稳定度就越低，有些修改是非常正常的，高层代码可重用性是不必要，也是不现实的，反而维护的方便性很重要。

### PV vs OCP

- 保变原则提倡变中求稳，更侧重 __可维护性__；开闭原则提倡稳中求变，更侧重可重用性

### 迪米特法则
- 保变原则暗含了 __迪米特法则__(Law of Demeter, LoD)，又称最少知识原则(Lest Knowledge Principle, LKP)，要求一个对象的方法只能调用以下对象：
    - 该对象本身，即this
    - 该方法的参数
    - 该方法内部创建的对象
    - 该对象的直接组成对象，包括其属性及集合属性中的元素

![prog_paradigm_lod](../../pics/prog_paradigm_lod.png)

- `Person`类与`Company`类并无直接的依赖关系，而是通过它的角色-`Employee`类产生了间接的联系，因此它们之间的 __耦合关系不是本质的__，最好能避免，否则`Company`类的`getCity`方法发生改变，`Person`类也受牵连

![prog_paradigm_lod2](../../pics/prog_paradigm_lod2.png)

- 重构后，`Person`类的`getWorkCity`方法完全是委托`Emplyee`类来实现的，这是一种职责转移，也是一种设计模式：__委托__(delegation)模式，同时还体现了间接原则

- 迪米特法则教导我们，可以信任朋友，但不可信任朋友的朋友。在封闭型多层架构中，上层只依赖于相邻的下层，不能越层请求服务，也是同样的道理

### 通用职责分配原则
- GRASP(General Responsibility Assignment Software Patterns/Principle)，即 __通用职责分配原则__，

![prog_paradigm_grasp](../../pics/prog_paradigm_grasp.png)

- 其中低耦合、高内聚、多态、间接、保变都已介绍过，信息专家原则和纯虚构原则是关键，创建者原则和控制器原则可看作是它们的应用

- __信息专家原则__ 即：知情者为负责者，即谁拥有完成职责所需的全部信息，谁就该负责，知情者太多可能是设计有缺陷的一个征兆

- __纯虚构原则__：虚构，指这种类有别于通常的类，并不代表问题领域中的某个概念，是虚拟构想；纯，指虚构类的职责单纯为了支持低耦高聚，因而它的设计会显得干净或纯净

- __创建者原则__ 建议，创建一个类的实例职责应分配给那些包含、聚合、记录或密切使用该类的类，或者拥有该类初始化数据的类，但同时也在两个类之间建立了强耦合。为此，可专门设计一个工厂类，接管创建对象的职责，这便是 __工厂模式__(factory pattern)，该工厂无疑符合 __纯虚构原则__。

- __控制器原则__ 建议，把处理系统事件的职责分配给两种类，一种是代表整体系统的类，符合 __信息专家原则__，另一种是代表事件发生的用例场景(use case scenario)的类，符合 __纯虚构原则__。

### 类级设计原则

![prog_paradigm_solid](../../pics/prog_paradigm_solid.png)

### 包级设计原则

![prog_paradigm_package_design](../../pics/prog_paradigm_package_design.png)

- 前三个是关于包内聚原则，以解决 __颗粒度__(granularity)问题，后三个是关于包的耦合原则，以解决 __稳定性__(stability)问题


设计模式
=============
- 许多设计模式为了克服编程语言的一些缺陷或局限而设计的
    1. 为了克服new运算符和构造器的局限，有工厂模式
    1. 为了克服内存分配的局限，有享元模式
    1. 为了克服函数不是头等公民的局限，有命令模式
    1. 为了克服无法动态继承的局限，有状态模式
    1. 为了克服过程式语言、静态类型语言、静态语言的语法限制而造成对象通信上的局限，有职责链、观察者、中介者模式等
    1. 为了克服OOP语言的单分派(single dispatch)的局限，有访问者模式


## 创建模式-不要问我从哪里来

### 构造器的弊端
- __它的名字必须与类名一致，缺乏足够的表现力__

如`Point`类来代表平面坐标系的点，构造器有两个参数，分别为横坐标和纵坐标，但有时还希望用极坐标来构造一个点

<!--language: java-->

    Point(double x, double y) 　　// 直角坐标
    Point(double r, double theta) // 极坐标

它们签名一样，无法共存，用 __静态工厂方法__(static factory method)（C++中称命名构造器）可解此困局：

<!--language: java-->

    class Point{
        private double x;
        private double y;

        public static Point cartesian(double x, double y){
            return new Point(x, y);
        }

        public static Point polar(double r, double theta){
            return new Point(r * Math.cos(theta), r * Math.sin(theta));
        }

        private Point(double x, double y){
            this.x = x; this.y = y;
        }
    }

- __构造器每次调用都伴随着新对象的创建，但这并不总是合适__。不公开构造器，而借助静态方法来提供对象，可控制对象实例的个数，如 __单例模式__ 便于一个应用，另一方面出于性能方面原因，可利用对象池来减少对象的创建：

<!--language: java-->

    public static Boolean valueOf(boolean b){
        return (b ? TRUE : FALSE);
    }

`valueOf`不会创建新对象，它总返回`Boolean.TRUE`和`Boolean.FALSE`中的一个，由于`Boolean`类是不可变的，共享对象不会产生任何问题

- __构造器第三个弊端是，它无法被继承，也就无法多态__。很多时候，客户只关心创建的对象是否能提供某种服务，某个抽象的超类型，而不具体类型，可直接new来创建对象，就必须知道具体类型，违背了 __针对接口编程__ 原则。

此时静态工厂方法，可以返回一个具有指定接口的对象，对象的具体类型则不用透露给客户，这些具体类型甚至是非公开的，还可以运行期间决定，或动态加载。

- __或因设计上缺陷，或因对象本身的复杂性，不少构造器并没有完成全部的初始化__，给客户带来额外负担，也可能破坏对象的一致性

### 工厂模式
以上的弊端，最好的办法是把这种变化隔离起来，单独封装在一个模块中，这个模块可以是一个方法－工厂方法，也可以是一个类－工厂类

<!--language: java-->

    // 一个能创建登录窗口的类(原始版)
    class LoginForm{
        public Frame createLoginWindow(){
            Frame window = new Frame("登录");

            TextFiled userInput = new TextField();
            window.add(userInput);

            Button submitButton = new Button("提交");
            window.add(submitButton);

            return window;
        }
    }

#### 静态工厂
如果不想让各个组件过早的定型，可考虑用更一般的`Component`类来代替，但这是一个抽象类，无法直接new，但可通过引入工厂类来克服

<!--language: java-->

    // 一个能创建登录窗口的类(静态工厂版)
    class WidgetFactory{
        public enum Type{AWT, SWING};

        public static Container makeFrame(Type type, String title){
            switch(type){
                case AWT:    return new Frame(title);
                case SWING:  return new JFrame(title);
                default:     return null;
            }
        }

        public static Container makeInput(Type type, String text){
            switch(type){
                case AWT:    return new TextField(text);
                case SWING:  return new JTextField(text);
                default:     return null;
            }
        }

        public static Container makeButton(Type type, String text){
            switch(type){
                case AWT:    return new Button(text);
                case SWING:  return new JButton(text);
                default:     return null;
            }
        }
    }

    class LoginForm{
        public Frame createLoginWindow(){
            final WidgetFactory.Type type = WidgetFactory.Type.AWT;
            Container window = WidgetFactory.makeFrame(type, "登录");

            Container userInput = WidgetFactory.makeInput(type, "");
            window.add(userInput);

            Container submitButton = WidgetFactory.makeButton(type, "提交");
            window.add(submitButton);

            return window;
        }
    }

静态工厂模式让我们用一个具体类`WidgetFactory`，代替了多个具体类，但是还有`WidgetFactory.Type.AWT`这样的细节，可考虑使用参数传递或配置文件方式来规避。

#### 工厂方法
但另一个不足之处在于`WidgetFactory`中每增加一种组件库类型，将不得不修改`switch`语句，需要利用多态机制来克服，__静态工厂方法就进化为多态的工厂方法__(factory method)：

<!--language: java-->

    // 一个能创建登录窗口的类(工厂方法版)
    abstract class LoginForm{
        // 工厂方法
        public abstract Container makeFrame(String title);
        public abstract Container makeInput(String text);
        public abstract Container makeButton(String text);

        // 模板方法
        public Frame createLoginWindow(){
            Container window = makeFrame("登录");
            Container userInput = makeInput("");
            window.add(userInput);
            Container submitButton = makeButton("提交");
            window.add(submitButton);
            return window;
        }
    }

    class AwtLoginForm extends LoginForm{
        public Container makeFrame(String title){
            return new Frame(title);
        }
        public Container makeInput(String text){
            return new TextField(text);
        }
        public Container makeButton(String text){
            return new Button(text);
        }
    }

#### 抽象工厂
工厂方法弥补了构造器无法多态的缺憾，因此又被称为 __虚构造器模式__(virtual constructor pattern)。工厂方法与静态工厂模式区别挺大，一个多态、一个静态；一个是一系列方法、一个却是单独的类。所以真正与静态工厂模式相对应的是 __抽象工厂模式__

<!--language: java-->

    // 一个能创建可视化组件的接口(抽象工厂)
    interface WidgetFactory{
        Container makeFrame(String title);
        Container makeInput(String text);
        Container makeButton(String text);
    }

    // 一个能创建AWT组件的类(抽象工厂的实现类)
    class AwtWidgetFactory implements WidgetFactory{
        public Container makeFrame(String title){
            return new Frame(title);
        }
        public Container makeInput(String text){
            return new TextField(text);
        }
        public Container makeButton(String text){
            return new Button(text);
        }
    }

    // 一个能创建登录窗口的类(抽象工厂版)
    class LoginForm{
        private WidgetFactory factory;
        // 依赖注射
        public LoginForm(WidgetFactory factory){
            this.factory = factory;
        }
        public Frame createLoginWindow(){
            Container window = factory.makeFrame("登录");
            Container userInput = factory.makeInput("");
            window.add(userInput);
            Container submitButton = factory.makeButton("提交");
            window.add(submitButton);
            return window;
        }
    }

#### 工厂方法vs抽象工厂

- 抽象工厂，如果把创建组件当成一种策略，接口中包括一系列多个产品的创建，可视为 __策略模式__ 的应用，除了 __依赖注射__，还可以使用 __依赖查找__（但如果查找的上下文来自某容器或框架，就是侵入性(intrusive)，不利于重用与测试）

- 工厂方法，常使用 __模板方法__，子类重写各部件的创建，逻辑存在基类中，一种特殊的控制反转

- 工厂模式的主要目的是为了遵循 __依赖反转__ 原则

- 抽象工厂明显是静态工厂的升级版，但同样也是工厂方法的升级版，把后者主体类中的 __一系列抽象的工厂方法__ 提炼成 __ 一个接口 __，用 __对象合成__ 取代了 __类继承__

![prog_paradigm_factory](../../pics/prog_paradigm_factory.png)

> p.s. 抽象工厂的类图上未体现接口的使用者

- 工厂方法创建的对象之间 __不必有内在关联__，但抽象工厂不然，它要创建的是 __一组具有共同主题(theme)的对象__，它有助于保证一组产品的一致性，在 __产品系列__ 之间进行切换

### 建造者模式

<!--language: java-->

    // 一个能创建可视化组件的接口(建造者)
    interface LoginWindowBuilder{
        void buildFrame(String title);
        void buildUserInput(String text); // 针对业务而不是产品
        void buildCommand(String submitText);
    }

    // 一个能创建AWT组件的类(建造者的实现类)
    class AwtLoginWindowBuilder implements LoginWindowBuilder{
        private Container window;
        public void buildFrame(String title){
            window = new Frame(title);
        }
        public void buildUserInput(String text){ // 粒度也大
            Component userLabel =new Label(text)
            window.add(userLabel);
            Component userInput = new TextField();
            window.add(userInput);
        }
        public void buildCommand(String submitText){
            Component submitButton = new Button(submitText);
            window.add(submitButton);
        }
        public Container getWindow(){
            return window;
        }
    }

    // 一个能创建登录窗口的类(建造者版)
    class LoginForm{
        private LoginWindowBuilder builder;
        // 依赖注射
        public LoginForm(LoginWindowBuilder builder){
            this.builder = builder;
        }
        public Frame createLoginWindow(){
            builder.buildFrame("登录");
            builder.buildUserInput("用户");
            builder.buildCommand("提交");
        }
    }

- 抽象工厂特点是一系列方法来创建对象，建造者模式特点是 __一系列步骤__ 来创建对象。
- 接口中包含创建步骤，均返回void(导致用户无须知道各部件类型)，最后一步产品创建成功
- 工厂一步到位的提供产品，而建造者直到最后一步才完成产品，前者保护的 __变化是产品制作来源__，后者保护的 __变化是产品的制作细节__，哪种变化程度大或概率大，就采用哪种模式
- 相比工厂模式，它对客户的知识要求极低，符合最少知识原则

- 为什么`LoginWindowBuilder`接口不提供`getWindow`方法，却让其实现类提供？为了让`LoginForm`不依赖该方法的返回类型`Container`。并且如果另一个可视化组件的类型层及完全不兼容，就没办法返回`Container`

![prog_paradigm_builder](../../pics/prog_paradigm_builder.png)

- 创建者模式分离了对象的创建与对象的结构表示(无须一个抽象类型来表示产品)，故能做到工厂模式难以做到的事
- 创建者模式还能解决构造器参数过长的问题

### 其它创建模式
- 原型模式，不像工厂借助子类来产生新类型，而是直接对原型对象进行拷贝
- 对象池模式或资源池模式，可重复利用已创建的对象，尤其是极耗资源或数量受限的对象，如数据库连接、套接字连接、线程等
- 单例模式，极少不直接依赖继承和多态机制的模式

### 对象创建的选择方案

![prog_paradigm_way_of_creation](../../pics/prog_paradigm_way_of_creation.png)

- 与其说创建对象，不如说 __请求对象__，因为有时并未真正地创建对象，如重用的对象、虚拟的对象。

## 结构模式-建筑的技巧
- 一个系统设计是否合理，很大程度上取决于层次的设计
- 结构模式关注的是如何把类和对象组合成更大的结构，实质上就是利用 __继承层级__ 的类结构和 __聚合层级__ 的对象结构来构建更高层抽象的过程

### 架构设计中分层与分区
- 划分标准不同，层是按 __抽象层次__(abstraction level)进行的水平划分，比如N层架构；区是在同一抽象层次中按 __功能领域__ 进行的垂直划分，比如按业务类别分区
- 模块关系不同，不同层的子系统是单向依赖的等级关系，即高层建立于低层之上；相同层不同区的子系统是双向合作平等关系

- 抽象是前提，分解是方式，模块化是结果，除了抽象与分解，还有一个对付复杂性的工具：__层级__(hierarchy)
    - 层级用于模块架构，就是N层架构
    - 层级用于类结构，便是类型层级
    - 层级用于对象结构，便是聚合层级

- 自底向上设计，即低层模块经过合作(collaboration)而产生高层模块，自顶向下设计，即高层模块经过细化(refinement)而产生底层模块。

- 所以，不提倡向下转型，盖因把高抽象的类型转化为低抽象的类型，破坏了抽象层次的一致性

### 桥梁模式
- AWT中就运用了桥梁模式，每个`Component`对象都聚合了一个类型为`ComponentPeer`对象，从而在组件的接口与实现之间搭建起了一座桥梁

![prog_paradigm_bridge_awt](../../pics/prog_paradigm_bridge_awt.png)

- 有3个 __层级__：桥梁的一头是以`Component`类为根的类型层级，另一头是以`ComponentPeer`接口为根的类型层级，两个根类型之间又形成了聚合层级

- 除了层级之外，__间接原则__ 也至关重要，通过引入一个间接对象`peer`，解除了抽象（各类组件）与其实现之间的耦合，使得两边可以各自独立的变化。

- 还可理解为 __保变原则__：把容易变化的实现包装于一个接口之中，并让该接口承担调用当地图形资源的职责

### 适配器模式
- 适配器是一个接口转换器，可以解决服务提供者与服务享受者之间接口不兼容的问题

![prog_paradigm_adapter](../../pics/prog_paradigm_adapter.png)

- 桥接模式的重点是 __分解__，让本来结合紧密的接口与实现分离开，适配器模式重点是 __结合__，让本来无关的类能合作共事

- 桥接模式通常是事先的有意设计（或重构得到），更多出自可维护性考量；而适配器通常是事后补救，更多出自可重用性考量（重用第三方或遗留代码）

### 装饰器模式
![prog_paradigm_decorator](../../pics/prog_paradigm_decorator.png)

- 一个可视化组件类，希望它的某个对象增加边框、滚动条、可拖动、可缩放，这4种特征各种组合有十几种，采用继承显然不合适

- 引入中间类`WidgetDecorator`，并赋取其装饰组件的职责

如要产生一个有边框、有滚动条又可拖动的组件，只需：

<!--language: java-->

    new DragDecorator(new ScrollDecotator(new BorderDecorator(someWidget)));

- 装饰者模式具有 __类型层级和聚合层级__ 有交合的地方，`WidgetDecorator`与`Widget`双重关系：聚合与继承，类型层级是在编译期决定的，聚合层级是动态生成的，如果没有继承关系，上述嵌套代码便无法成立，因各类装饰者构造器接受是`Widget`类型

- 能动态的增加，同样可以动态的取消职责

- 装饰者增加的职责，应当是辅助性或边缘性职责，类似`mixin`特点，不会影响模块内聚度

- 如果想改变对象的内心，就采用策略模式

- 适配器改变对象的接口而保持对象的职责，装饰者改变对象的职责而保持对象的接口

### 代理模式
- 与装饰器模式一样也保持对象的接口，但装饰器通常增强接口的服务，而代理模式却可能限制接口的服务

![prog_paradigm_proxy](../../pics/prog_paradigm_proxy.png)

- 如果某个对象的初始化十分耗费时间或资源，却又未必立即投入使用，可利用代理来推迟创建(lazy initialization)

- C++中 __智能指针__ 是一种抽象化的指针，通过重载指针运算符`operator*`和`opetator->`来实现自动垃圾回收或边界检查，以弥补原始指针(raw pointer)的不足。属于更广义的一种代理：智能引用(smart reference)，对Java和C#仍然具有价值
    - 保证互拆(mutual exclusion)(同步代理)、引用计数(reference counting)(计数代理)、写时拷贝(copy-on-write,COW)或迟拷贝(lazy copy)、延迟加载持久化对象等

- 保护代理(protected proxy)，可控制对原有对象的访问，以保障敏感数据的安全

### 复合模式
- 也是利用类型层级和聚合层级来构造更大的复合结构

![prog_paradigm_composite1](../../pics/prog_paradigm_composite1.png)

![prog_paradigm_composite2](../../pics/prog_paradigm_composite2.png)

- 前图的`Component`与`Left`和`Composite`接口都是一致的，后图`Component`只与`Left`接口一致，一般提到复合模式，多半指前者

### 享元模式
- 唯一一个主要针对软件 __运行质量__ 而非设计质量的模式是享元模式(flyweight pattern)，被译为享元，通常是因为是共享的，但叫做"轻量模式"可能更合适

- 对一类颗粒度小、数量众多、相似度高、种类有限、具有 __值语义__ 的对象，通过共享机制来减少对象的创建，以提供时空性能上的改善

- 既然打算共享，当然不能由客户亲自创建对象，而要利用工厂模式，更确切说是抽象工厂，因为要创建的是一系列产品

- 享元模式的关键抽象出一类对象内在的、不因环境而异(context-insensitive)的状态，封装后作为共享单元

![prog_paradigm_flyweight](../../pics/prog_paradigm_flyweight.png)

## 行为模式-君子之交淡如水
### OOP大弱点
- OOP中消息发送者与接收者之间的耦合不仅限于类型级别的耦合，还有对象级别的耦合，一个对象必须在获得另一个对象标识(identity)后方能向其发送消息
- 声明式编程的目标是显性的，而算法是隐性的，模块间的许多直接通信可以免去，由解释器代为完成
    - 而Prolog或声明式编程无须如此，简单的陈述一些事实和规则，不必借助模块之间相互引用便能推出结论
    - 再如UNIX下的管道(pipe)

<!--language: bash-->

    # 将文件file1排序并去掉重复相邻行后，再与文件file2进行比较，
    # 并指导相同的部分输出到文件common中
    cat < file1 | sort | uniq | comm -12 - file2 > common

- 管道看似平凡，却能把简单的UNIX程序组合成具有强大功能的应用，这种模式本身即 __管线模式__(pipeline pattern)，或 __管道-过滤器模式__(pipes and filters pattern)，不仅可作为设计模式，更可作为架构模式

> [孟岩谈OO弱点](http://blog.csdn.net/myan/article/details/5928531)

> 对象范式的两个基本观念：

> - 程序是由对象组成的；
> - 对象之间互相发送消息，协作完成任务

> 这两个观念与后来我们熟知的面向对象三要素“封装、继承、多态”根本不在一个层面上，倒是与再后来的“组件、接口”神合。

> Simula/C++和Smalltalk/ObjectC都秉承上述对象范式的两个基本观念，为了方便对象的构造，也都引入了类、继承等概念。也就是说，__类、继承这些机制__ 是为了实现对象范式原则而构造出来的 __第二位的、工具性__ 的机制，那么为什么后来这些第二位的东西篡了主位。而Simula和Smalltalk最重大的不同，就是Simula用 __方法调用__ 的方式向对象发送消息，而Smalltalk构造了更灵活和更纯粹的 __消息发送__ 机制。

>> 具体的说，向一个Simula对象中发送消息，就是调用这个对象的一个方法，或者称成员函数。那么你怎么知道能够在这个对象上调用这个成员函数呢？或者说，你怎么知道能够向这个对象发送某个消息呢？这就要求你 __必须确保这个对象具有合适的类型__，也就是说，你得先知道哦这个对象是什么，才能向它发消息。而消息的实现方式被直接处理为成员函数调用，或虚函数调用。

>> 而Smalltalk在这一点上做了一个历史性的跨越，它实现了一个 __与目标对象无关__ 的消息发送机制，不管那个对象是谁，也不管它是不是能正确的处理一个消息，作为发送消息的对象来说，可以毫无顾忌地抓住一个对象就发消息过去。接到消息的对象，要尝试理解这个消息，并最后调用自己的过程来处理消息。如果这个消息能被处理，那个对象自然会处理好，如果不能被处理，Smalltalk系统会向消息的发送者回传一个doesNotUnderstand消息，予以通知。对象不用关心消息是如何传递给另一个对象的，传递过程被分离出来（而不是像Simula那样明确地被以成员函数调用的方式实现），可以是在内存中复制，也可以是进程间通讯。到了Smalltalk-80时，消息传递甚至可以跨越网络。

>> Windows系统中的消息机制实际上是 __动态__ 的，与C++的 __静态__ 消息机制根本配合不到一起去。在Windows里，你可以向任何一个窗口发送消息，这个窗口自己会在自己的wndproc里来处理这个消息，如果它处理不了，就交给default window/dialog proc去处理。而在C++里，你要向一个窗口发消息，就得确保这个窗口能处理这个消息，或者说，具有合适的类型。这样一来的话，就会导致一个错综复杂的窗口类层次结构，无法实现。而如果你要让所有的窗口类都能处理所有可能的消息，且不论这样在逻辑上就行不通（用户定义的消息怎么处理？），单在实现上就不可接受——为一个小小的不同就得创造一个新的窗口类，每一个小小的窗口类都要背上一个多达数百项的v-table，而其中可能99%的项都是浪费。

> 实际上C++的静态消息机制还引起了更深严重的问题—— __扭曲了人们对面向对象的理解__。既然必须要先知道对象的类型，才能向对象发消息，那么“类”这个概念就特别重要了，而对象只不过是类这个模子里造出来的东西，反而不重要。渐渐的，__“面向对象编程”变成了“面向类编程”，“面向类编程”变成了“构造类继承树”__。放在眼前的鲜活的对象活动不重要了，反而是其背后的 __静态类型系统__ 成为关键。“封装、继承”这些第二等的特性，__喧宾夺主__，俨然成了面向对象的要素。每个程序员似乎都要先成为领域专家，然后成为领域分类学专家，然后构造一个完整的继承树，然后才能new出对象，让程序跑起来。

>> 可以从一个具体的例子来理解这个道理，比如在一个GUI系统里，一个 Push Button 的设计问题。事实上在一个实际的程序里，一个 push button 到底“是不是”一个 button，进而是不是一个 window/widget，并不重要，本质上我根本不关心它是什么，它从属于哪一个类，在继承树里处于什么位置，只要那里有这么一个东西，我可以点它，点完了可以发生相应的效果，就可以了。可是Simula –> C++ 所鼓励的面向对象设计风格，非要上来就想清楚，a Push Button is-a Button, a Button is-a Command-Target Control, a Command-Target Control is-a Control, a Control is-a Window. 把这一圈都想透彻之后，才能 new 一个 Push Button，然后才能让它工作。这就 __形而上学__ 了，这就脱离实际了。所以很难做好。你看到 MFC 的类继承树，觉得设计者太牛了，能把这些层次概念都想清楚，自己的水平还不够，还得修炼。实际上呢，这个设计是经过数不清的失败和钱磨出来、砸出来的，MFC的前身 Afx 不是就失败了吗？

> 客观地说，“面向类的设计”并不是没有意义。来源于实践又高于实践的抽象和概念，往往能更有力地把握住现实世界的本质，比如MVC架构，就是这样的有力的抽象。但是这种抽象，应该是来源于长期最佳实践的总结和提高，而不是面对问题时主要的解决思路。__过于强调这种抽象，无异于假定程序员各个都是哲学家，具有对现实世界准确而深刻的抽象能力，当然是不符合实际情况的__。结果呢，刚学习面向对象没几天的程序员，对眼前鲜活的对象世界视而不见，一个个都煞有介事地去搞哲学冥想，企图越过现实世界，去抽象出其背后本质，当然败得很惨。

>> 其实C++问世之后不久，这个问题就暴露出来了。第一个C++编译器 Cfront 1.0 是单继承，而到了 Cfront 2.0，加入了 __多继承__。为什么？就是因为使用中人们发现逻辑上似乎完美的静态单继承关系，碰到复杂灵活的现实世界，就破绽百出——蝙蝠是鸟也是兽，水上飞机能飞也能游，它们该如何归类呢？本来这应该促使大家反思继承这个机制本身，但是那个时候全世界陷入继承狂热，于是就开始给继承打补丁，加入多继承，进而加入 __虚继承__，。到了虚继承，明眼人一看便知，这只是一个语法补丁，是为了逃避职责而制造的一块无用的遮羞布，它已经完全已经脱离实践了——有谁在事前能够判断是否应该对基类进行虚继承呢？

> UML中有一个 __对象活动图__，其描述的就是运行时对象之间相互传递消息的模型。1994年Robert C. Martin在《Object-Oriented C++ Design Using Booch Method》中，曾建议 __*面向对象设计从对象活动图入手，而不是从类图入手*__。而1995年出版的经典作品《Design Patterns》中，建议 __优先考虑组合而不是继承__，这也是尽人皆知的事情。这些迹象表明，在那个时候，面向对象社区里的思想领袖们，已经意识到“面向类的设计”并不好用。

> Java和.NET的核心类库是在C++十几年成功和失败的经验教训基础之上，结合COM体系优点设计实现的，自然要好上一大块。事实上，在Java和.NET核心类库的设计中很多地方，__体现的是基于接口的设计__，和真正的基于对象的设计。有了这两个主角站台，“面向类的设计”不能喧宾夺主，也能发挥一些好的作用。还有，Java和.NET中分别对C++最大的问题——缺少对象级别的 __delegate机制__ 做出了自己的回应，这就大大弥补了原来的问题

> COM的要义是，软件是由COM Components组成，components之间彼此通过接口相互通讯。这是否让你回想起本文开篇所提出的对象范型的两个基本原则？有趣的是，在COM的术语里，“COM Component ” 与“object ”通假，这就使COM的心思昭然若揭了。Don Box在Essential COM里开篇就说，COM是更好的C++，事实上就是告诉大家，形而上学的“面向类设计”不好使，还是回到对象吧。用COM开发的时候，__一个组件“是什么”不重要，它具有什么接口，也就是说，能够对它发什么消息，才是重要的__。你可以用IUnknown::QueryInterface问组件能对哪一组消息作出反应。向组件分派消息也不一定要被绑定在方法调用上，如果实现了 IDispatch，还可以实现“自动化”调用，也就是COM术语里的 Automation，而通过 列集（mashal），可以跨进程、跨网络向另一组件发送消息，通过 moniker，可以在分布式系统里定位和发现组件。如果你抱着“对象——消息”的观念去看COM的设计，就会意识到，整个COM体系就是用规范如何做对象，如何发消息的。或者更直白一点，COM就是用C/C++硬是模拟出一个Smalltalk。而且COM的概念世界里没有继承，就其纯洁性而言，比Smalltalk还Smalltalk。在对象泛型上，COM达到了一个高峰，领先于那个时代，甚至于比它的继任.NET还要纯洁。COM的主要问题是它的学习难度和安全问题，而且，它过于追求纯洁性，完全放弃了“面向类设计” 的机制，显得有点过。


### 职责链模式
- GoF的设计模式中，比较接近管线模式的是职责链模式(chain of responsibility pattern)

![prog_paradigm_chain](../../pics/prog_paradigm_chain.png)

- 每一个`Hanlder`相当于一个过滤器，在处理完请求后传给职责链的下一环，但消息发送者仍然须要知道接收者的数据类型（只不过是抽象类型），而管道对接收者无此要求

- 通常情况下，消息发送者与接收者之间与处理者是等价的概念，但在职责链中，消息接收者有可能并不直接处理请求，而是转给后继者。换言之，消息发送者虽然知道哪一类对象是接收者，却不知道哪一个对象实际参与了请求的处理

- DOM事件模型中，从外到内的事件捕捉(event capturing)，还是从内到外的事件冒泡(event bubbing)，都可在可视化元素的层级之间进行事件的链式处理

- 职责链关心是职责的分解，更侧重行为；装饰者关心是职责的结合，更侧重结构

- Java Servlet中的`Filter`也是一例，程序员可把Servlet过滤器写在配置文件中，不需要硬编码

#### 异常处理
- 异常处理机制，可称为语言级别的职责链，代码在捕捉到异常并进行处理后，可选择重新抛出异常，以便让上一级代码继续处理

- 许多人经常在何时该抛出异常，抛出何种异常，处理完异常后该不该继续上抛等问题上不决
    1. 没有职责链意识，有些事情是需要分模块分时间分批次来完成的
    1. 没有抽象层次的意识，不同抽象层次的对象处理不同层次的异常，必要时通过包装提升异常的抽象层次，交由上级处理

- 假如一个方法捕捉到一个异常：
    1. 不在该方法的规范的职责之内，该方法应直接将异常连同必要的信息以更抽象的形式包装来，然后抛给更高层
    1. 部分由该方法负责，对异常进行一些处理，然后重复情形1的行为
    1. 完全由该方法负责，则在方法内进行相应的处理，不再为此抛出异常

### 命令模式
- 命令模式也是发送者与接收者之间的解耦，基本思想是：把请求或命令封装成一个对象，交由请求的发送者或命令的调用者控制
    1. 命令对象包含了执行命令的全部信息：请求的接收者、方法和方法参数，只等一声令下
    1. 命令对象还以统一接口形式出现，下令者不必知道命令的具体类型，更不必知道实际的执行者以及执行方式，只需把握下令的时机

- 命令的下达者与执行者没有必然关联的，也就是说请求的发送者与接收者在 __空间上__ 被解耦了
- 下令者获得命令对象的时间与下令的时间不必一致，因此它们在 __时间上__ 也被解耦，也正是异步回调（OO化的回调）

![prog_paradigm_command](../../pics/prog_paradigm_command.png)

- 一个具体的命令与C++中的函子(functor)或函数对象(function object)很相似，只是前者执行方法是`execute`，后者是函数调用运算符。不过函子通常只是函数的包装，不一定有`receiver`对象

- 回调函数的本质是把代码当数据使用，命令对象也如此，由于命令的数据化封装，可以保存、排除、记入日志、异步执行、异地执行、动态选择或修改、还原(undo)操作

### 观察者模式
- 一个消息发送者可以把消息同时发给多个接收者

![prog_paradigm_observer](../../pics/prog_paradigm_observer.png)

- 它是控制反转的应用，MVC模型中，Subject代表低层的模型，Observer代表高层的视图

### 中介者模式
- 构造一个信息交换中心，为众多平等的对象提供了交流的平台，这样`Colleague`对象之间不须要彼此相知，只须知道`Mediator`对象便可彼此通信

![prog_paradigm_mediator](../../pics/prog_paradigm_mediator.png)

- 它与观察者不同之处，在通信与同步的职责分配上，前者是集中式的(centralized)，`Mediator`对象管理所有通信和同步协议，后者是分布式的(distributed)，`Subject`对象和`Observer`对象必须要通过合作方能保证同步约束，但也会结合使用：
    - `Mediator`对象可利用观察者模式与`Colleague`对象进行交流
    - 当`Subject`对象与`Observer`对象之间的同步逻辑比较复杂时也可能会引入`Mediator`对象

### 状态模式
![prog_paradigm_state](../../pics/prog_paradigm_state.png)

- 一个对象的自身行为如何随着自身状态的改变而改变
- 利用合成，或委托来解决，引入间接对象`State`来封装状态的变化，把不同的状态对应的不同行为包装到不同的模块之中

### 备忘录模式
![prog_paradigm_memento](../../pics/prog_paradigm_memento.png)

- 如果说状态模式关注的是对象状态的演进，备忘录则关注的是对象状态的回归，如命令模式用来支持还原的操作，就需要相关对象状态的历史记录，备忘录模式便能助力

- `Originator`对象是备忘的主体和制造者，它的状态为`Memento`对象所封装，后者由`Caretaker`对象负责保存和管理，由于`Originator`类具有回滚(rollback)和快照(snapshot)的接口，不仅能支持单步还原，还能支持批量还原

- `Memento`接口中的方法的修饰符是包级的，除`Originator`对象外，其他对象都无权查看或修改其状态，C++可在`Memento`类中声明`Originator`类为友类(friend class)

### 访问者模式
- 克服OOP语言的单分派的局限
- 分派是为一个调用点(call site)确定相应调用方法的过程。如果函数或方法名是唯一的，过程十分简单，如果不唯一，不同的语言有不同的算法，通常根据参数的类型或个数来决定：
    1. 若分派能在编译期决定，则是静态分派，如重载多态(overloading polymorphism)和参数多态(parametric polymorphism)
    1. 若分派在运行期间决定，则是动态分派(dynamic dispatch)，如子类型多态(subtyping polymorphism)

- C++,Java和C#采用同样的动态分派机制：在运行期间根据一个方法的接收者－即方法所属的对象的实际类型来决定到底该调哪种方法。这类分派仅取决于一个类型，即方法的接收对象的类型，故称单分派

- 如果分派取决于两个参数的类型，则称双分派

<!--language: !csharp-->

    using System;
    using System.Collections.Generic;
    using System.Linq;

    abstract class File {
        public string Name {get; set;}
    }

    class PlainFile : File{
    }

    class Directory : File{
    }

    interface FileVistor{
        void Visit(PlainFile file);
        void Visit(Directory file);
        void Visit(File file);
    }

    class FilePrinter : FileVistor{
        public void Visit(PlainFile file){
            Console.WriteLine("plain file: " + file.Name);
        }
        public void Visit(Directory file){
            Console.WriteLine("directory: " + file.Name);
        }
        public void Visit(File file){
            Console.WriteLine("abstract file: " + file.Name);
        }
    }

    class Program {
        static void Main(string[] args) {
            File dir = new Directory{Name = "directory_name"};
            File file = new PlainFile{Name = "file_name"};
            FileVistor visitor = new FilePrinter();

            visitor.Visit(dir);
            visitor.Visit((Directory)dir);
            visitor.Visit(file);
            visitor.Visit((PlainFile)file);
        }
    }

- `dir`,`file`变量必须进行类型判断和向下转型才能被正确的Visit，否则认为是超类型的。
- 通过给文件类各添加一个`accept`方法

<!--language: !csharp-->

    using System;
    using System.Collections.Generic;
    using System.Linq;

    abstract class File {
        public string Name {get; set;}
        abstract public void Accept(FileVistor visitor);
    }

    class PlainFile : File{
        override public void Accept(FileVistor visitor){visitor.Visit(this);}
    }

    class Directory : File{
        override public void Accept(FileVistor visitor){visitor.Visit(this);}
    }

    interface FileVistor{
        void Visit(PlainFile file);
        void Visit(Directory file);
        // void Visit(File file);
    }

    class FilePrinter : FileVistor{
        public void Visit(PlainFile file){
            Console.WriteLine("plain file: " + file.Name);
        }
        public void Visit(Directory file){
            Console.WriteLine("directory: " + file.Name);
        }
        public void Visit(File file){
            Console.WriteLine("abstract file: " + file.Name);
        }
    }

    class Program {
        static void Main(string[] args) {
            File dir = new Directory{Name = "directory_name"};
            File file = new PlainFile{Name = "file_name"};
            FileVistor visitor = new FilePrinter();

            dir.Accept(visitor); // equals: visitor.Visit((Directory)dir);
            file.Accept(visitor);// equals: visitor.Visit((PlainFile)file);
        }
    }

- `void Visit(File file);`也没必要存在了
- 在语句`file.accept(visitor)`中，通过本身的单分派找到实际的`accept`方法，实现了`file`的多态（类型多态）；接着在相应的`visitor.Visit(this)`中，再利用单分派找到实际的`Visit`方法，实现了`Visitor`的多态（参数多态）

![prog_paradigm_visitor](../../pics/prog_paradigm_visitor.png)

- 缺陷是`Visitor`需要了解`File`的类型层级，可参考ACYCLIC VISITOR模式

### 迭代器模式
![prog_paradigm_iterator](../../pics/prog_paradigm_iterator.png)

- 赋予了用户自定义循环的能力，实现了迭代抽象(iteration abstraction)，用户可以对聚合体或容器以某种次序遍历(traverse)容器中的元素，至于容器的内部结构、遍历的起止、推进等细节，均毫不知情，也不关心
- 迭代器作为容器与算法之间的中介，__促使算法摆脱了对数据结构的依赖__，从而更具普适性和重用性，一旦算法被抽象出来，__泛型范式__ 便可大展神威
- 泛型范式与迭代器相得益彰，充分体现在C++的STL、Java和C#的Collections中
