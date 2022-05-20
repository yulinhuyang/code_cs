参考：

JavaCore ： https://github.com/dunwu/javacore

Java核心 卷一笔记 ：https://blog.csdn.net/qq_39326472/category_8572091.html

# 目录


# 目录
# 卷1
**第1章　Java程序设计概述**  
1.1　Java程序设计平台    
1.2　Java“白皮书”的关键术语
1.3　Java applet与Internet 
1.4　Java发展简史 
1.5　关于Java的常见误解
 
**第2章　Java程序设计环境** 
2.1　安装Java开发工具包 
2.2　使用命令行工具 
2.3　使用集成开发环境 
2.4　JShell
 
**第3章　Java的基本程序设计结构** 
3.1　一个简单的Java应用程序 
3.2　注释 
3.3　数据类型 
3.4　变量与常量 
3.5　运算符 
3.6　字符串 
3.7　输入与输出 
3.8　控制流程 
3.9　大数  
3.10　数组 
 
**第4章　对象与类**
  
4.1　面向对象程序设计概述  
4.2　使用预定义类  
4.3　用户自定义类 
4.4　静态字段与静态方法 
4.5　方法参数 
4.6　对象构造 
4.7　包 
4.7.1　包名 
4.8　JAR文件 
4.9　文档注释 
4.10　类设计技巧
 
**第5章　继承**
 
5.1　类、超类和子类 
5.2　Object：所有类的超类 
5.3　泛型数组列表 
5.4　对象包装器与自动装箱 
5.5　参数数量可变的方法 
5.6　枚举类 
5.7　反射 
5.8　继承的设计技巧
 
**第6章　接口、lambda表达式与内部类** 
6.1　接口 
6.2　lambda表达式 
6.3　内部类 
6.4　服务加载器 
6.5　代理 

**第7章　异常、断言和日志** 
7.1　处理错误 
7.2　捕获异常 
7.3　使用异常的技巧 
7.4　使用断言 
7.5　日志 
7.6　调试技巧 

**第8章　泛型程序设计** 
8.1　为什么要使用泛型程序设计 
8.2　定义简单泛型类 
8.3　泛型方法 
8.4　类型变量的限定 
8.5　泛型代码和虚拟机 
8.6　限制与局限性 
8.7　泛型类型的继承规则 
8.8　通配符类型 
8.9　反射和泛型 

**第9章　集合** 
9.1　Java集合框架 
9.2　集合框架中的接口 
9.3　具体集合 
9.4　映射 
9.5　视图与包装器 
9.6　算法 
9.7　遗留的集合
 
**第10章　图形用户界面程序设计** 
10.1　Java用户界面工具包简史 
10.2　显示窗体 
10.3　在组件中显示信息 
10.4　事件处理 
10.5　首选项API 

**第11章　Swing用户界面组件** 

11.1　Swing和模型–视图–控制器设计模式 
11.2　布局管理概述 
11.3　文本输入 
11.4　选择组件 
11.5　菜单 
11.6　复杂的布局管理 
11.7　对话框    
 
**第12章　并发** 
12.1　什么是线程 
12.2　线程状态 
12.3　线程属性 
12.4　同步 
12.5　线程安全的集合 
12.6　任务和线程池 
12.7　异步计算 
12.8　进程    

  
# 卷2

**第1章　Java 8的流库** 
1.1　从迭代到流的操作  
1.2　流的创建   
1.3　filter、map和flatMap方法  
1.4　抽取子流和组合流  
1.5　其他的流转换  
1.6　简单约简  
1.7　Optional类型  
1.7.1　获取Optional值  
1.7.2　消费Optional值  
1.7.3　管道化Optional值  
1.7.4　不适合使用Optional值的方式  
1.7.5　创建Optional值  
1.7.6　用flatMap构建Optional值的函数  
1.7.7　将Optional转换为流  
1.8　收集结果  
1.9　收集到映射表中  
1.10　群组和分区  
1.11　下游收集器  
1.12　约简操作  
1.13　基本类型流  
1.14　并行流   
 
**第2章　输入与输出** 
 
2.1　输入/输出流  
2.1.1　读写字节  
2.1.2　完整的流家族  
2.1.3　组合输入/输出流过滤器  
2.1.4　文本输入与输出  
2.1.5　如何写出文本输出  
2.1.6　如何读入文本输入  
2.1.7　以文本格式存储对象  
2.1.8　字符编码方式  
2.2　读写二进制数据  
2.2.1　DataInput和DataOutput接口  
2.2.2　随机访问文件  
2.2.3　ZIP文档  
2.3　对象输入/输出流与序列化  
2.3.1　保存和加载序列化对象  
2.3.2　理解对象序列化的文件格式  
2.3.3　修改默认的序列化机制  
2.3.4　序列化单例和类型安全的枚举  
2.3.5　版本管理  
2.3.6　为克隆使用序列化  
2.4　操作文件  
2.4.1　Path  
2.4.2　读写文件  
2.4.3　创建文件和目录  
2.4.4　复制、移动和删除文件  
2.4.5　获取文件信息  
2.4.6　访问目录中的项  
2.4.7　使用目录流  
2.4.8　ZIP文件系统  
2.5　内存映射文件  
2.5.1　内存映射文件的性能  
2.5.2　缓冲区数据结构 
2.6　文件加锁机制 
2.7　正则表达式 
2.7.1　正则表达式语法 
2.7.2　匹配字符串 
2.7.3　找出多个匹配 
2.7.4　用分隔符来分割 
2.7.5　替换匹配 

**第3章　XML** 

3.1　XML概述 
3.2　XML文档的结构 
3.3　解析XML文档 
3.4　验证XML文档 
3.4.1　文档类型定义 
3.4.2　XML Schema 
3.4.3　一个实践示例 
3.5　使用XPath来定位信息 
3.6　使用命名空间 
3.7　流机制解析器 
3.7.1　使用SAX解析器 
3.7.2　使用StAX解析器 
3.8　生成XML文档 
3.8.1　不带命名空间的文档 
3.8.2　带命名空间的文档 
3.8.3　写出文档 
3.8.4　使用StAX写出XML文档 
3.8.5　示例：生成SVG文件 
3.9　XSL转换 

**第4章　网络** 

4.1　连接到服务器 
4.1.1　使用telnet 
4.1.2　用Java连接到服务器 
4.1.3　套接字超时 
4.1.4　因特网地址 
4.2　实现服务器 
4.2.1　服务器套接字 
4.2.2　为多个客户端服务 
4.2.3　半关闭 
4.2.4　可中断套接字 
4.3　获取Web数据 
4.3.1　URL和URI 
4.3.2　使用URLConnection获取信息 
4.3.3　提交表单数据 
4.4　HTTP客户端 
4.5　发送E-mail  

**第5章　数据库编程** 

5.1　JDBC的设计 
5.1.1　JDBC驱动程序类型 
5.1.2　JDBC的典型用法 
5.2　结构化查询语言 
5.3　JDBC配置 
5.3.1　数据库URL 
5.3.2　驱动程序JAR文件 
5.3.3　启动数据库 
5.3.4　注册驱动器类 
5.3.5　连接到数据库 
5.4　使用JDBC语句 
5.4.1　执行SQL语句 
5.4.2　管理连接、语句和结果集 
5.4.3　分析SQL异常 
5.4.4　组装数据库 
5.5　执行查询操作 
5.5.1　预备语句 
5.5.2　读写LOB 
5.5.3　SQL转义 
5.5.4　多结果集 
5.5.5　获取自动生成的键 
5.6　可滚动和可更新的结果集 
5.6.1　可滚动的结果集 
5.6.2　可更新的结果集 
5.7　行集 
5.7.1　构建行集 
5.7.2　被缓存的行集 
5.8　元数据 
5.9　事务 
5.9.1　用JDBC对事务编程 
5.9.2　保存点 
5.9.3　批量更新 
5.9.4　高级SQL类型 
5.10　Web与企业应用中的连接管理   
 
**第6章　日期和时间API**

6.1　时间线 
6.2　本地日期 
6.3　日期调整器 
6.4　本地时间 
6.5　时区时间 
6.6　格式化和解析 
6.7　与遗留代码的互操作    

**第7章　国际化** 

7.1　locale 
7.1.1　为什么需要locale 
7.1.2　指定locale 
7.1.3　默认locale 
7.1.4　显示名字 
7.2　数字格式 
7.2.1　格式化数字值 
7.2.2　货币 
7.3　日期和时间 
7.4　排序和规范化 
7.5　消息格式化 
7.5.1　格式化数字和日期 
7.5.2　选择格式 
7.6　文本输入和输出 
7.6.1　文本文件 
7.6.2　行结束符 
7.6.3　控制台 
7.6.4　日志文件 
7.6.5　UTF-8字节顺序标志 
7.6.6　源文件的字符编码 
7.7　资源包 
7.7.1　定位资源包 
7.7.2　属性文件 
7.7.3　包类 
7.8　一个完整的例子    

**第8章　脚本、编译与注解处理**
 
8.1　Java平台的脚本机制 
8.1.1　获取脚本引擎 
8.1.2　脚本计算与绑定 
8.1.3　重定向输入和输出 
8.1.4　调用脚本的函数和方法 
8.1.5　编译脚本 
8.1.6　示例：用脚本处理GUI事件 
8.2　编译器API 
8.2.1　调用编译器 
8.2.2　发起编译任务 
8.2.3　捕获诊断消息 
8.2.4　从内存中读取源文件 
8.2.5　将字节码写出到内存中 
8.2.6　示例：动态Java代码生成 
8.3　使用注解 
8.3.1　注解简介 
8.3.2　示例：注解事件处理器 
8.4　注解语法 
8.4.1　注解接口 
8.4.2　注解 
8.4.3　注解各类声明 
8.4.4　注解类型用法 
8.4.5　注解this 
8.5　标准注解 
8.5.1　用于编译的注解 
8.5.2　用于管理资源的注解 
8.5.3　元注解 
8.6　源码级注解处理 
8.6.1　注解处理器 
8.6.2　语言模型API 
8.6.3　使用注解来生成源码 
8.7　字节码工程 
8.7.1　修改类文件 
8.7.2　在加载时修改字节码 

**第9章　Java平台模块系统**
 
9.1　模块的概念 
9.2　对模块命名 
9.3　模块化的“Hello, World!”程序 
9.4　对模块的需求 
9.5　导出包 
9.6　模块化的JAR 
9.7　模块和反射式访问 
9.8　自动模块 
9.9　不具名模块 
9.10　用于迁移的命令行标识 
9.11　传递的需求和静态的需求 
9.12　限定导出和开放 
9.13　服务加载 
9.14　操作模块的工具 
## 第10章　安全 
10.1　类加载器 
10.1.1　类加载过程 
10.1.2　类加载器的层次结构 
10.1.3　将类加载器用作命名空间 
10.1.4　编写你自己的类加载器 
10.1.5　字节码校验 
10.2　安全管理器与访问权限 
10.3 用户认证
10.4 数字签名
10.5 加密

**第11章 不错Swing和图形化编程**  

11.1 表格
11.2 树
11.3 不错AWT
11.4 像素图
11.5 打印   

**第12章 本地方法**

12.1 从Java程序中调用C函数
12.2 数值参数与返回值
12.3 字符串参数
12.4 访问域 
12.5 编码签名 
12.6 调用Java方法
12.7 访问数组元素
12.8 错误处理
12.9 使用调用API
12.10 完整的示例：访问Windows注册表


# 笔记

### 1  @Override 

是伪代码,表示重写(当然不写也可以)，不过写上有如下好处:

1、可以当注释用,方便阅读；

2、编译器可以给你验证@Override下面的方法名是否是你父类中所有的，如果没有则报错

### 2  equals 和 == 的区别 

equals 是方法，而 == 是操作符；

对于基本类型的变量来说（如 short、 int、 long、 float、 double），只能使用 == ，因为这些基本类型的变量没有 equals 方法。对于基本类型变量的比较，使用 == 比较， 一般比较的是它们的值。

对于引用类型的变量来说（例如 String 类）才有 equals 方法，因为 String 继承了 Object 类， equals 是 Object 类的通用方法。对于该类型对象的比较，默认情况下，也就是没有复写 Object 类的 equals 方法，使用 == 和 equals 比较是一样效果的，都是比较的是它们在内存中的存放地址。但是对于某些类来说，为了满足自身业务需求，可能存在equals 方法被复写的情况，这时使用 equals 方法比较需要看具体的情况，例如 String 类，使用 equals 方法会比较它们的值；

https://www.jianshu.com/p/9cbed9f33a4d

### 3 对象传递 

在Java中，当对象作为参数传递时，实际上传递的是一份“引用的拷贝”。

### 4 实现反射 

java 获取反射机制三种方式

1. new对象 

2. 路径

3. 类名

public class Main {

	public static void main(String[] args) throws ClassNotFoundException {
		// 方式一、new对象
		Student student = new Student();
		Class classObj1 = student.getClass();
		System.out.println(classObj1.getName());

		// 方式二、路径-相对路径
		Class classObj2 = Class.forName("com.aop8.reflect.Student");
		System.out.println(classObj2.getName());

		// 方式三、类名
		Class classObj3 = Student.class;
		System.out.println(classObj3.getName());
	}
}


### 5 extend implement 区别 

[抽象类和接口的区别已经变了](https://zhuanlan.zhihu.com/p/75513356)

子类继承抽象类通过 extends , 单继承

	public abstract class A {}
	public class B extends A {}


抽象类实现接口通过 implements , 多实现，接口继承接口通过 extends

	public interface A {}
	public interface B {}

	public interface C extends A {}

	public class D implements B,C {}

**实例化**

抽象类和接口均不能实例化

抽象类可以有构造方法，接口不能有构造方法

	public abstract class A {
	 public A () {}
	}


**属性**

接口中定义属性只能是 (public)静态常量

	public interface A {
	 //默认(public static final) String A="ABC";
	 String A = "ABC";
	}
	
抽象类中可以定义有任意变量常量

//任意访问修饰符的变量及常量

	public abstract class A {
		 private String a = "a";
		 boolean b = true;
		 public char c = 'a';
		 protected int d = 2;
		 public static final int e = 1;
	}

**设计**

1. 抽象类是对一组类的共同特征进行抽象 ，是子类的模板（is-a）

2. 接口是对行为的抽象，是一种行为的规范和约束 (like-a)

**类优先**

同时extend implement 时，类优先


### 6  Cloneable clone

copy:  浅拷贝

clone: 浅克隆和深克隆之分。

浅克隆: 如果对象包含子对象的引用，拷贝域就包括相同子对象的引用。

深克隆： 深克隆将原型对象的所有引用对象也复制一份给克隆对象。

浅克隆：覆盖Object类的clone()方法可以实现浅克隆。

深克隆：

先对对象进行序列化，紧接着马上反序列化出；

先调用super.clone()方法克隆出一个新对象来，然后在子类的clone()方法中手动给克隆出来的非基本数据类型（引用类型）赋值，比如ArrayList的clone()方法。


***Cloneable接口***

（1）此类实现了Cloneable接口，以指示Object的clone()方法可以合法地对该类实例进行按字段复制

（2）如果在没有实现Cloneable接口的实例上调用Object的clone()方法，则会导致抛出CloneNotSupporteddException

（3）按照惯例，实现此接口的类应该使用公共方法重写Object的clone()方法，Object的clone()方法是一个受保护的方法

***Object的clone()方法***

创建并返回此对象的一个副本。对于任何对象x，表达式：

（1）x.clone() != x为true

（2）x.clone().getClass() == x.getClass()为true

（3）x.clone().equals(x)一般情况下为true，但这并不是必须要满足的要求


### 7 lambda 表达式

**带参变量的表达式**

	(parameters) -> expression

	或

	(parameters) ->{ statements; }

	eg：
	
	// 3. 接受2个参数(数字),并返回他们的差值  
	(x, y) -> x – y  

	// 4. 接收2个int型整数,返回他们的和  
	(int x, int y) -> x + y  

	// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
	(String s) -> System.out.print(s)

lambda 表达式中捕获的变量必须实际上是最终变量( effectivelyfinal）。最终变量是指，这个变量初始化之后就不会再为它赋新值。

lambda 表达式的体与嵌套块有相同的作用域。

对于只有一个抽象方法的接口， 需要这种接口的对象时， 就可以提供一个 lambda 表达式。这种接口称为函数式接口。

使用lambda 表达式的重点是延迟执行 deferred execution

**java常用函数式接口：**

Consumer< T>:消费型接口

接口方法 void accept(T t)：参数类型是T，无返回值

Supplier< T>供给型接口

接口方法 T get()：参数类型是T，返回T类型参数

Function<T,R>函数型接口</T,R>

接口方法R apply(T)：对类型T参数操作，返回R类型参数

Predicate< T>段言型接口

接口方法 boolean test（T t）：对类型T进行条件筛选操作，返回boolean

### 8 闭包

在JAVA中，闭包是通过“接口+内部类”实现，JAVA的内部类也可以有匿名内部类。

Lambda表达式也是闭包。

### 9 内部类

[内部类详解](https://www.cnblogs.com/dolphin0520/p/3811445.html)

Java的内部类可分为Inner Class（成员内部类、局部内部类）、Anonymous Class和Static Nested Class三种：

Inner Class和Anonymous Class本质上是相同的，都必须依附于Outer Class的实例，即隐含地持有Outer.this实例，并拥有Outer Class的private访问权限；

Static Nested Class是独立类，但拥有Outer Class的private访问权限。

**特殊语法：** 

	OwterC/ass.this    表示外围类引用

	if (TalkingClock.this,beep) Toolkit.getDefaultToolkitO.beep();

编写内部对象的构造器

	outerObject.n&H InnerClass{construction parameters)

	ActionListener listener = this.new TimePrinterO;
	
	TalkingClock jabberer = new Ta1kingClock(1000, true);
	
	TalkingOock.TiiePrinter listener = jabberer.new TimePrinterO；
	
在外围类的作用域之外，可以这样引用内部类：OuterClass.InnerClass	
	
**成员内部类**

成员内部类是最普通的内部类，它的定义为位于另一个类的内部

外部访问：

外部类.this.成员变量

外部类.this.成员方法

	class Circle {
	    double radius = 0;

	    public Circle(double radius) {
		this.radius = radius;
	    }

	    class Draw {     //内部类
		public void drawSahpe() {
		    System.out.println("drawshape");
		}
	    }
	}

成员内部类是依附外部类而存在的，也就是说，如果要创建成员内部类的对象，前提是必须存在一个外部类的对象

        //第一种方式：
        Outter outter = new Outter();
        Outter.Inner inner = outter.new Inner();  //必须通过Outter对象来创建
         
        //第二种方式：
        Outter.Inner inner1 = outter.getInnerInstance();

**局部内部类**

局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法内或者该作用域内。

```java
	class People{
	    public People() {

	    }
	}

	class Man{
	    public Man(){

	    }

	    public People getWoman(){
		class Woman extends People{   //局部内部类
		    int age =0;
		}
		return new Woman();
	    }
	}
```	


**匿名内部类**

匿名内部类应该是平时我们编写代码时用得最多的，在编写事件监听的代码时使用匿名内部类不但方便，而且使代码更加容易维护

```java
scan_bt.setOnClickListener(new OnClickListener() {
             
            @Override
            public void onClick(View v) {
                // TODO Auto-generated method stub
            }
        });
         
        history_bt.setOnClickListener(new OnClickListener() {
             
            @Override
            public void onClick(View v) {
                // TODO Auto-generated method stub  
            }
        });
```
	
**静态内部类**

```java
	public class Main {
	    public static void main(String[] args) {
		Outer.StaticNested sn = new Outer.StaticNested();
		sn.hello();
	    }
	}

	class Outer {
	    private static String NAME = "OUTER";

	    private String name;

	    Outer(String name) {
		this.name = name;
	    }

	    static class StaticNested {
		void hello() {
		    System.out.println("Hello, " + Outer.NAME);
		}
	    }
	}
```

### 10 反编译

	java reflection.ReflectionTest irmerClass.lilkingClock\STimePrinter

	javap -private innerClass.W kingClock\STimePrinte


### 11 动态代理

动态代理（以下称代理），利用Java的反射技术(Java Reflection)，在运行时创建一个实现某些给定接口的新类（也称“动态代理类”）及其实例（对象）

可以在运行期动态创建某个interface的实例。

```java
	import java.lang.reflect.InvocationHandler;
	import java.lang.reflect.Method;
	import java.lang.reflect.Proxy;

	public class Main {
	    public static void main(String[] args) {
		InvocationHandler handler = new InvocationHandler() {
		    @Override
		    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
			System.out.println(method);
			if (method.getName().equals("morning")) {
			    System.out.println("Good morning, " + args[0]);
			}
			return null;
		    }
		};
		Hello hello = (Hello) Proxy.newProxyInstance(
		    Hello.class.getClassLoader(), // 传入ClassLoader
		    new Class[] { Hello.class }, // 传入要实现的接口
		    handler); // 传入处理调用方法的InvocationHandler
		hello.morning("Bob");
	    }
	}

	interface Hello {
	    void morning(String name);
	}
```



在运行期动态创建一个interface实例的方法如下：

1 定义一个InvocationHandler实例，它负责实现接口的方法调用；

2 通过Proxy.newProxyInstance()创建interface实例，它需要3个参数：

	使用的ClassLoader，通常就是接口类的ClassLoader；
	
	需要实现的接口数组，至少需要传入一个接口进去；
	
	用来处理接口方法调用的InvocationHandler实例。
	
3 将返回的Object强制转型为接口。

动态代理实际上是JVM在运行期动态创建class字节码并加载的过程，它并没有什么黑魔法

解决特定问题：一个接口的实现在编译时无法知道，需要在运行时才能实现

实现某些设计模式：适配器(Adapter)或修饰器(Decorator)

面向切面编程：如AOP in Spring


### 12 异常 断言 日志

应该捕获那些知道如何处理的异常，而将那些不知道怎样处理的异常继续进行传递

不管是否有异常被捕获，finally 子句中的代码都被执行。

try 语句可以只有finally 子句，而没有catch 子句

强烈建议解搞合try/catch 和try/finally 语句块

```java
	InputStrean in = . . .;
	try
	{
		try
		{
			code that might throw exceptions
		}
		finally
		{
			in.doseO;
		}
	}
	catch (IOException e)
	{
		show error message
	}
```

带资源的try 语句（try-with-resources) 的最简形式为：

```java
	try (Resource res = . . .)
	{
		work with res
	}
	
	try (Scanner in = new Scanner(new FileInputStream(7usr/share/dict/words")), "UTF-8")
	{
		while (in.hasNextO)
		System.out.pri ntl n(in.next()) ;
	}
```

try 块退出时，会自动调用res.doseO。

这个块正常退出时，或者存在一个异常时，都会调用inxloseO 方法，就好像使用了finally 块一样。

还可以指定多个资源: 例如：

```java
	try (Scanner in = new Scanne「(new FileInputStream('7usr/share/dict/words"). "UTF-8"):
	PrintWriter out = new Pri ntWriter("out.txt"))
	{
		while (in.hasNextO)
		out.pri ntl n(i n.next().toUpperCaseO);
	}
```

堆栈轨迹： 调用 Throwable 类的 printStackTrace 方法访问堆栈轨迹的文本描述信息。

使用 getStackTrace 方法， 它会得到 StackTraceElement 对象的一个数组， 可以在你的程序中分析这个对象数组。

静态的 Thread.getAllStackTrace 方法， 它可以产生所有线程的堆栈轨迹。

在出错的地方抛出一个EmptyStackException异常要比在后面抛出一个NullPointerException 异常更好


	assert 条件：表达式；

断言检查只用于开发和测阶段。

断言是一种测试和调试阶段所使用的战术性工具; 而日志记录是一种在程序的整个生命周期都可以使用的策略性工具。

**日志**

使用全局日志记录器（global logger) 并调用其info 方法：

Logger.getClobal 0,info("File->Open menu item selected");

如果在适当的地方（如main 开始）调用  

Logger.getClobal ().setLevel (Level .OFF);

将会取消所有的日志。

处理器  ----> 过滤器  ---->格式化器

**调试技巧**

每一个类中放置一个单独的main 方法。这样就可以对每一个类进行单元测试。

JUnit是一个非常常见的单元测试框架。

利用Throwable 类提供的printStackTmce 方法，可以从任何一个异常对象中获得堆栈情况。

-Xlint 选项告诉编译器对一些普遍容易出现的代码问题进行检査。

使用jmap实用工具获得一个堆的转储，其中显示了堆中的每个对象。

使用-Xprof 标志运行Java 虚拟机，就会运行一个基本的剖析器来跟踪那些代码中经常被调用的方法。

jconsole 的图形工具，可以用于显示虚拟机性能的统计结果

### 13  泛型

[Java中的泛型方法](https://www.cnblogs.com/iyangyuan/archive/2013/04/09/3011274.html)

[java 泛型详解](https://blog.csdn.net/s10461/article/details/53941091)

从表面上看，Java 的泛型类类似于C++ 的模板类。唯一明显的不同是Java 没有专用的template 关键字。

泛型只在编译阶段有效。

在编译之后程序会采取去泛型化的措施。也就是说Java中的泛型，只在编译阶段有效。在编译过程中，正确检验泛型结果后，会将泛型的相关信息擦出，并且在对象进入和离开方法的边界处添加类型检查和类型转换的方法。也就是说，泛型信息不会进入到运行时阶段。


泛型类：

	public class Pair<T, U> { . . . }

在Java 库中，使用变量E 表示集合的元素类型，K 和V 分别表示表的关键字与值的类型。T ( 需要时还可以用临近的字母U 和S) 表示“ 任意类型”

泛型方法：

	class ArrayAlg
	{
		public static <T> T getMiddle(T... a)
		{
		    return a[a.length / 2];
		}
	}

泛型接口： 

	//定义一个泛型接口
	public interface Generator<T> {
	    public T next();
	}
	
泛型类，是在实例化类的时候指明泛型的具体类型；泛型方法，是在调用方法的时候指明泛型的具体类型 。

类型限制：  public static <T extends Coiparab1e> T a) . . .

多个通配符限制：     T extends Comparable & Serializable

	 public static <T extends Comparable〉Pair<T> minmax(T[] a)

通配符： 类型通配符一般是使用?代替具体的类型参数。例如 List<?> 在逻辑上是List<String>,List<Integer> 等所有List<具体类型实参>的父类。

	public class GenericTest {

	    public static void main(String[] args) {
		List<String> name = new ArrayList<String>();
		name.add("icon");
		getData(name);
	   }

	   public static void getData(List<?> data) {
	      System.out.println("data :" + data.get(0));
	   }

子类型限定： ? extends Employee

超类型限定： ? super Manager



java 类型擦除, C++ 中每个模板的实例化产生不同的类型，这一现象称为“ 模板代码膨账”


### 14  集合

让类库规模小且易于学习，而不希望像C++ 的“ 标准模版库” （S卩STL) 那样复杂，但却又希望能够得到STL 率先推出的“ 泛型算法” 所具有的优点。

将接口（interface) 与实现(implementation) 分离。

#### 队列

队列通常有两种实现方式：一种是使用循环数组；另一种是使用链表。

集合类的基本接口是Collection 接口

	public interface Collection<b
	{
		boolean add(E element);
		Iterator<E> iteratorQ；
	}

Iterator 接口包含4 个方法：

	public interface Iterator<E>
	{
		E next();
		boolean hasNext();
		void remove();
		default void forEachRemaining(Consumer<? super E> action);
	}


	for (String element : c)
	{
		do something with element
	}


删除两个相邻元素： 调用remove之前必须调用next

	it.remove(); 
	it.next()；
	it.remove(); 

#### List

所有链表实际上都是双向链接的(doubly linked) ——即每个结点还存放着指向前驱结点的引用

Add 方法在迭代器位置之前添加一个新对象。

可以根据需要给容器附加许多的迭代器，但是这些迭代器只能读取列表。另外，再单独附加一个既能读又能写的迭代器

contains：检测某个元素是否出现在链表

get ：访问某个特定元素，get 方法做了微小的优化：如果索引大于 size() / 2 就从列表尾端开始搜索元素。

add 方法只依赖于迭代器的位置， 而 remove 方法依赖于迭代器的状态。

set 方法用一个新元素取代调用 next 或 previous 方法返回的上一个元素。

列表迭代器接口：nextlndex 方法返回下一次调用 next 方法时返回元素的整数索引；previouslndex 方法返回下一次调用 previous 方法时 返回元素的整数索引。

有两种访问元素的协议：一种是用迭代器， 另一种是用get和 set 方法随机地访问每个元素。后者不适用于链表， 但对数组却很有用。

**Vector与 ArrayList 区别**

Vector类的所有方法都是同步的。可以由两个线程安全地访问一个Vector对象。但是，如果由一个线程访问Vector, 代码要在同步操作上耗费大量的时间。这种情况还是很常见的。而ArrayList方法不是同步的，

因此，建议在不需要同步时使用ArrayList, 而不要使用Vector。

#### 散列集 HashSet

如果a_equals(b) 为true, a 与b 必须具有相同的散列码。

在Java中，散列表用链表数组实现。每个列表被称为桶（bucket) 。要想査找表中对象的位置， 就要先计算它的散列码， 然后与桶的总数取余，所得到的结果就是保存这个元素的桶的索引。

将桶数设置为一个素数，以防键的集聚。标准类库使用的桶数是2的幂，默认值为16。

装填因子（load factor) 决定何时对散列表进行再散列。例如，如果装填因子为0.75。

HashSet 类，它实现了基于散列表的集。可以用add 方法添加元素。

**树集 Tree Set**

树集(Tree Set)是一个有序集合 (sorted collection)可以以任意顺序将元素插入到集合中。在对集合进行遍历时，每个值将自动地按照排序后的顺序呈现.排序是用树结构完成的（当前实现使用的

是红黑树). 每次将一个元素添加到树中时，都被放置在正确的排序位置上。

将一个元素添加到树中要比添加到散列表中慢。

队列：有两个端头的队列，即双端队列，可以让人们有效地在头部和尾部同时添加或删除元素。Deque接口，并由 ArrayDeque 和 LinkedList 类实现。

**优先级队列（priority queue) **

优先级队列（priority queue) 中的元素可以按照任意的顺序插人，却总是按照排序的顺序进行检索

优先级队列使用了一个优雅且高效的数据结构，称为堆（heap)。堆是一个可以自我调整的二叉树，对树执行添加（add) 和删除（remore) 操作，可以让最小的元素移动到根，而不必花费时间对元素进行排序。

典型示例是任务调度。每一个任务有一个优先级，任务以随机顺序添加到队列中。

	PriorityQueue<LocalDate> pq = new PriorityQueueoO;

#### 映射map

HashMap和TreeMap

	Map<String, Employee> staff = new HashMap<>();//HashMap implements Map
	Employee harry = new Employee ('Harry Hacker");
	staff.put(”987-98-9996",harry);

散列映射对键进行散列，树映射用键的整体顺序对元素进行排序， 并将其组织成搜索树

remove方法用于从映射中删除给定键对应的元素。size 方法用于返回映射中的元素数。 要迭代处理映射的键和值， 最容易的方法是使用forEach 方法。可以提供一个接收键和值的lambda 表达式。映射中的每一项会依序调用这个表达式。 

	map.forEach((k,v)->{
		    System.out.println("k="+k);
		});

更新映射项:  

getOrDefault方法

	counts.put(word,getOrDefault(word,0)+1);

首先调用 putlfAbsent 方法。只有当键原先不存在时才会放入一个值。

	counts.putIfAbsent(word,0);
	counts.put(word,counts.get(word）+1）；

merge方法，将把word与1关联

	counts.merge(word, 1, Integer::sum);

映射视图：

可以得到映射的视图（View)————这是实现了 Collection 接口或某个子接口的对象，键集、值集合（不是一个集）以及键 / 值对集
 
        Map<String,String> map=new HashMap<>();
        map.put("a","b");
        Set<String> set=map.keySet();
        Collection<String> collection=map.values();
        Set<Map.Entry<String, String>> set2=map.entrySet();
        Iterator itr=set2.iterator();
        while(itr.hasNext()) {
        	System.out.println(itr.next());
        }
        for(Map.Entry<String, String> entry:set2) {
        	String a=entry.getValue();
        	String b=entry.getKey();
        }

**弱散列映射：**

负责从长期存活的映射表中删除那些无用的值。 或者使用WeakHashMap完成这件事情。当对键的唯一引用来自散列条目时。

WeakHashMap 使用弱引用（weak references) 保存键，WeakReference对象将引用保存到另外一个对象中，在这里，就是散列键。对于这种类型的对象，垃圾回收器用一种特有的方式进行处理。通常，如果垃圾回收

器发现某个特定的对象已经没有他人引用了，就将其回收。然而，如果某个对象只能由WeakReference引用，垃圾回收器仍然回收它，但要将引用这个对象的弱引用放人队列中。WeakHashMap将周期性地检查队

列， 以便找出新添加的弱引用。一个弱引用进人队列意味着这个键不再被他人使用， 并且已经被收集起来。于是， WeakHashMap将删除对应的条目。
 
**链接散列集与映射**

LinkedHashSet 和 LinkedHashMap类用来记住插人元素项的顺序，这样就可以避免在散列表中的项从表面上看是随机排列的。

链接散列映射将用访问顺序（谁访问的多）， 而不是插入顺序， 对映射条目进行迭代。每次调用 get 或 put, 受到影响的条目将从当前的位置删除，并放到条目链表的尾部

LinkedHashMap<K, V>(initialCapacity, loadFactor, true)

**枚举集与映射**

EnumSet 是一个枚举类型元素集的高效实现。

EnumMap是一个键类型为枚举类型的映射。

EnumMap<Weekday, Employee〉personlnCharge = new EnumMap<>(Weekday.class); 

**表示散列映射**

类 IdentityHashMap 有特殊的作用，在这个类中， 键的散列值不是用hashCode函数计算的， 而是用 System.identityHashCode 方法计算的。 这是 Object.hashCode 方法根据对象的内 存地址来计算散列码时所使用的方式。

#### 视图与包装器

通过使用视图 (views) 可以获得其他的实现了 Collection接口和 Map 接口的对象。

keySet方法返回一个实现 Set 接口的类对象， 这个类的方法对原映射进行操作。这种集合称为视图。

轻量级集合包装器

	List<Card> cardList = Arrays.asList(cardDeck):

返回的对象不是 ArrayList。它是一个视图对象， 带有访问底层数组的 get 和 set方法。

**子范围视图**

不可修改的视图

subList 方法来获得一个列表的子范围视图

	List group2 = staff.subList(10, 20);

类似的：

	SortedSet< E> subSet(E from, E to)
	SortedSet< E> headSet(E to)
	SortedSet< E> tail Set(E from)


**不可修改的视图**

Collections.unmodifiableList

Collections 还有几个方法， 用于产生集合的不可修改视图 

	List<String> staff = new LinkedListoO ;
	
	1ookAt (Coll ecti ons.unmodi fiabl eLi st(staff));

同步视图

synchronizedMap 方法可以将任何一个映射表转换成具有同步访问方法的Map:

	Map<String, Employee>map = Collections.synchronizedMap(new HashMap<String, Employee>())； 


#### 算法

交集： 

	Set<String> result = new HashSeto(a);
	result.retainAll(b);

视图删除： staffMap.keySet().removeAll (terminatedIDs);

toArray： 从集合得到数组

	Object values = staff.toArray（）;
	
批操作

很多操作会“ 成批” 复制或删除元素。通过使用一个子范围视图，可以把批操作限制在子列表和子集上。这个子范围还可以完成更改操作。 staff.subList（0，10）.clear()； 

**集合与数组的转换**

如果需要把一个数组转换为集合，Arrays.asList 包装器可以达到这个目的。例如：

数组---->集合

List<String> list=new ArrayList<>();
    	String[] a=new String[10];
    	HashSet<String> set=new HashSet<>(Arrays.asList(a));
	
集合-->数组

    	String[] b=set.toArray(new String[0]);
    	String[] b=set.toArray(new String[10]);
	
 
#### 遗留集合

Hashtable ：与HashMap 类的作用一样，实际上，它们拥有相同的接口。与Vector 类的方法一样。Hashtable 的方法也是同步的。

枚举： Enumeration 接口有两个方法，即hasMoreElements 和nextElement

Collections.enumeration 将产生一个枚举对象，枚举集合中的元素

属性映射（property map) :

• 键与值都是字符串。

• 表可以保存到一个文件中，也可以从文件中加载。

• 使用一个默认的辅助表。

实现属性映射的 Java 平台类称为 Properties

栈（stack）

位集

由于位集将位包装在字节里，所以，使用位集要比使用 Boolean对象的 ArrayList 更加高效

BitSet 类提供了一个便于读取、设置或清除各个位的接口。使用这个接口可以避免屏蔽和其他麻烦的位操作。


### 15  并发

**一个单独的线程中执行一个任务的简单过程**

```java     
	package com;
	import java.util.*;
	import java.lang.*;
	import java.io.*;
	public class Welcome{
	    class a implements Runnable{
		public void run() {
			System.out.println("a");
		}
	    }
	    public void x() {
		 Runnable i=new a();
			Thread thread=new Thread(new a());
			thread.start();
	    }
	    public static void main(String []args){
		 new Welcome().x();

	    }
	}
```

1 ) 将任务代码移到实现了 Runnable 接口的类的 run方法中。这个接口非常简单，只有 一个方法：

public interface Runnable { void run（）； } 由于 Runnable 是一个函数式接口，可以用 lambda 表达式建立一个实例： Runnable r = () -> { taskcode};

2 ) 由 Runnable 创建一个 Thread 对象： Thread t = new Thread(r);

3 ) 启动线程： t.start（）；
 

#### 中断线程

interrupt 方法可以用来请求终止线程。当对一个线程调用 interrupt 方法时，线程的中断状态将被置位。这是每一个线程都具有的 boolean 标志。

每个线程都应该不时地检査这个标志，以判断线程是否被中断。 要想弄清中断状态是否被置位，首先调用静态的 Thread.currentThread方法获得当前线 程，然后调用 islnterrupted方法
 
	while(!Thread.currentThread().isInterrupted()) {

	}
	
当在一个被阻塞的线程（调用sleep或wait) 上调用 interrupt方法时，阻塞调用将会被 Interrupted Exception 异常中断。	
	
被中断的线程可以决定如何响应中断。某些线程是如此重要以至于应该处理完异常

```java
	Runnable r = () -> {
	 try { 
		while (!Thread.currentThread().islnterrupted0 && more work todo) 
		{ 
		    do morework 
		}
	     } 
	catch(InterruptedException e) 
	{ 
	     // thread was interr叩ted during sleep or wait } 
	    finally
	
	    { cleanup,if required } // exiting the run method terminates the thread 
	} 
```

interrupted和islnterrupted。Interrupted方法是一个静态方法，它检测当前的线程是否被中断。 调用interrupted方法会清除该线程的中断状态。

islnterrupted方法是一个实例方法，可用来检验是否有线程被中断。调用这个方法不会改变中断状态。

InterruptedException 异常被抑制在很低的层次上，

有两种合理 的选择：

•在catch子句中调用 Thread.currentThread().interrupt() 来设置中断状态。于是，调用者可以对其进行检测。

```java
	void mySubTask() {
	try { 
	    sleep(delay); 
	} 
	catch (InterruptedException e) 
	{
	    Thread.currentThread()-interrupt(); 
	}
```

•或者，更好的选择是，用 throws InterruptedException标记你的方法， 不采用 try语句块捕获异常。于是，调用者（或者，最终的 run 方法）可以捕获这一异常。 

#### 线程状态

线程可以有如下 6 种状态：

•New (新创建）

•Runnable (可运行）

•Blocked (被阻塞）

•Waiting (等待）

•Timed waiting (计时等待）

•Terminated (被终止） 


当用new操作符创建一个新线程时，如 new Thread(r)，该线程还没有开始运行。

可运行线程, 一旦调用start方法，线程处于runnable状态。

抢占式调度系统给每一个可运行线程一个时间片来执行任务。当时间片用完，操作系统剥夺该线程的运行权，并给另一个线程运行机会（见图 14-4 )。当选择下一个线程时， 操作系统考虑线程的优先级。

**被阻塞线程和等待线程**

当线程处于被阻塞或等待状态时，它暂时不活动。它不运行任何代码且消耗最少的资源。直到线程调度器重新激活它。

*当一个线程试图获取一个内部的对象锁（而不是java.util.concurrent 库中的锁)，而该锁被其他线程持有，则该线程进人阻塞状态。当所有其他线程释放该锁，并且线程调度器允许本线程持有它的时候，该线程将变成非阻塞状态。

*当线程等待另一个线程通知调度器一个条件时，它自己进入等待状态。在调用Object.wait方法或 Thread.join方法，或者是等待 java, util.concurrent 库中的 Lock 或 Condition 时， 就会出现这种情况。

*有几个方法有一个超时参数。调用它们导致线程进人计时等待（timed waiting)状态。这一状态将一直保持到超时期满或者接收到适当的通知。带有超时参数的方法有 Thread.sleep 和 Object.wait、Thread.join、Lock.tryLock 以及 Condition.await 的计时版。

**被终止的线程**

线程因如下两个原因之一而被终止:  因为run方法正常退出而自然死亡。因为一个没有捕获的异常终止了run方法而意外死亡。

#### 线程属性

**线程优先级**

线程的各种属性：线程优先级、守护线程、线程组以及处理未捕获异常的处理器。

在 Java 程序设计语言中，每一个线程有一个优先级。默认情况下，一个线程继承它的父线程的优先级。可以用setPriority方法提高或降低任何一个线程的优先级。可以将优先级设置为在 MIN_PRIORITY (在 Thread类中定义为 1) 与 MAX_PRIORITY (定义为 10) 之间的任何值。NORM_PRIORITY 被定义为 5。

**守护线程**

t.setDaemon(true); 将线程转换为守护线程（daemon thread)。守护线程的唯一用途是为其他线程提供服务。

计时线程就是一个例子，它定时地发送“ 计时器嘀嗒” 信号给其他线程或清空过时的高速缓存项的线程。当只剩下守护线程时， 虚拟机就退出了，由于如果只 剩下守护线程， 就没必要继续运行程序了。 

守护线程应该永远不去访问固有资源，如文件、 数据库，因为它会在任何时候甚至在一个操作的中间发生中断。

**未捕获异常处理器**

在线程死亡之前，异常被传递到一个用于未捕获异常的处理器。 该处理器必须属于一个实现 Thread.UncaughtExceptionHandler 接口的类

就在线程死亡之前， 异常被传递到一个用于未捕获异常的处理器。 该处理器必须属于一个实现 Thread.UncaughtExceptionHandler 接口的类。这个接口只有— 个方法。

	void uncaughtException(Thread t, Throwable e)

可以用 setUncaughtExceptionHandler方法为任何线程安装一个处理器。也可以用 Thread 类的静态方法 setDefaultUncaughtExceptionHandler 为所有线程安装一个默认的处理器。

线程组是一个可以统一管理的线程集合。默认情况下，创建的所有线程属于相同的线程组， 但是， 也可能会建立其他的组。现在引入了更好的特性用于线程集合的操作， 所以建议不要在自己的程序中使用线程组。

**ThreadGroup 类**

ThreadGroup类实现Thread.UncaughtExceptionHandler接口。它的uncaughtException方法做如下操作：

1 ) 如果该线程组有父线程组，那么父线程组的uncaughtException方法被调用。

2 ) 否则，如果 Thread.getDefaultExceptionHandler方法返回一个非空的处理器， 则调用该处理器。

3 ) 否则，如果 Throwable是ThreadDeath的一个实例， 什么都不做。

4 ) 否则，线程的名字以及Throwable的栈轨迹被输出到System.err 上。 这是你在程序中肯定看到过许多次的栈轨迹


#### 同步

两个或两个以上的线程需要共享对同一数据的存取。如果两个线程存取相同的对象，并且每一个线程都调用了一个修改该对象状态的方法，这样一个情况通常称为竞争条件（race condition)。

**锁对象**

有两种机制防止代码块受并发访问的干扰。synchronized 关键字自动提供一个锁

用 ReentrantLock 保护代码块的基本结构如下：

```java
	myLock.lock（）; // a ReentrantLock object 
	try { critical section } 
	finally 
	{ myLock.unlock（）；// make sure the lock is unlocked even if an exception isthrown } 
```

这一结构确保任何时刻只有一个线程进人临界区。一旦一个线程封锁了锁对象， 其他任 何线程都无法通过 lock语句。当其他线程调用 lock 时，它们被阻塞，直到第一个线程释放 锁对象。

警告：把解锁操作括在 finally 子句之内是至关重要的。如果在临界区的代码抛出异常， 锁必须被释放。否则， 其他线程将永远阻塞。

如果使用锁，就不能使用带资源的try语句。首先， 解锁方法名不是 close。不过， 即使将它重命名，带资源的 try语句也无法正常工作。它的首部希望声明一个新变量。但 是如果使用一个锁， 你可能想使用多个线程共享的那个变量。

同步，简单地理解，就是协同步调，一个完成了，另一个才能开始。

异步，就是你说的不同步，就是互不干扰，各干各的，多个线程可能同时进行。

锁是可重入的，因为线程可以重复地获得已经持有的锁。锁保持一个持有计数（hold count) 来跟踪对 lock 方法的嵌套调用。线程在每一次调用 lock 都要调用 unlock 来释放锁。 由于这一特性， 被一个锁保护的代码可以调用另一个使用相同的锁的方法  

通常， 可能想要保护需若干个操作来更新或检查共享对象的代码块。要确保这些操作完 成后， 另一个线程才能使用相同对象。 

**条件对象**

要使用一个条件对象来管理那些已经获得了一个锁但是却不能做有用工作的线程。

一个锁对象可以有一个或多个相关的条件对象 。习惯上给每一个条件对象命名为可以反映它所表达的条件的名字。

等待获得锁的线程和调用await 方法的线程存在本质上的不同。一旦一个线程调用await方法，它进人该条件的等待集。当锁可用时，该线程不能马上解除阻塞。相反，它处于阻塞状态，直到另一个线程调用同一条件上的signalAll 方法时为止。

这一调用重新激活因为这一条件而等待的所有线程。当这些线程从等待集当中移出时， 它们再次成为可运行的，调度器将再次激活它们。

同时，它们将试图重新进人该对象。一旦锁成为可用的，它们中的某个将从 await 调用返回， 获得该锁并从被阻塞的地方继续执行。

此时， 线程应该再次测试该条件。由于无法确保该条件被满足

signalAll 方法仅仅是通知正在等待的线程 ：此时有可能已经满足条件， 值得再次去检测该条件。 在对象的状态有利于等待线程的方向改变时调用signalAll。

另一个方法 signal, 则是随机解除等待集中某个线程的阻塞状态。这比解除所有线程的阻塞更加有效。当一个线程拥有某个条件的锁时， 它仅仅可以在该条件上调用 await、signalAll 或 signal 方法。

**synchronized关键字**

• 锁用来保护代码片段， 任何时刻只能有一个线程执行被保护的代码。

• 锁可以管理试图进入被保护代码段的线程。

• 锁可以拥有一个或多个相关的条件对象。

• 每个条件对象管理那些已经进入被保护的代码段但还不能运行的线程。

Java 中的每一个对象都有一个内部锁。如果一个方法用 synchronized关键字声明，那么对象的锁 将保护整个方法。也就是说，要调用该方法，线程必须获得内部的对象锁

```java
public synchronized void method（） { method body }
 
public void method(){
 
this.intrinsicLock.lock();
 
try{
 
}finally{
    this.intrinsicLock.unlock();
}
 
}

```

内部对象锁只有一个相关条件。wait方法添加一个线程到等待集中，notifyAll /notify方法解除等待线程的阻塞状态。换句话说，调用wait 或notityAll 等价于

	intrinsicCondition.await（）;
	intrinsicCondition.signalAll（）; 


内部锁和条件存在一些局限。包括：

• 不能中断一个正在试图获得锁的线程。

• 试图获得锁时不能设定超时。

• 每个锁仅有单一的条件，可能是不够的。

 Lock 和 Condition 对象还是同步方法？下面是一些建议:
 
•  最好既不使用 Lock/Condition 也不使用 synchronized 关键字。在许多情况下你可以使用java.util.concurrent 包中的一种机制，它会为你处理所有的加锁。

• 如果synchronized关键字适合你的程序，那么请尽量使用它，这样可以减少编写的代码数量，减少出错的几率。

• 如果特别需要Lock/Condition结构提供的独有特性时，才使用Lock/Condition.

synchronized锁住对象的时候锁住的是堆中的对象，而不是栈中对象的引用。

```java
	Object o=new Object();
	synchronized(o){
	}

	o=new Object();
	synchronized(o){
	}
	//这两个锁住的是不同的对象。
```

不要锁字符串常量，因为相同的字符串常量在一个地址，都在常量池里。不要去锁字符串常量。


**同步阻塞**
 
每一个 Java 对象有一个锁。线程可以通过调用同步方法获得锁。还有 另一种机制可以获得锁，通过进入一个同步阻塞。当线程进入如下形式的阻塞： synchronized (obj) 
 
于是它获得Obj的锁。 有时会发现“ 特殊的” 锁，例如：

```java

public class Bank 
{ 

private double[] accounts; 
private Object lock = new Object() ; 
public void transfer(int from, int to, int amount)

 { 
   synchronized (lock) // an ad-hoc lock
 { 
     accounts[from] -= amount; 
     accounts[to] += amount; 
 } 
   System.out.print1n(.. } 
}

```

在此，lock 对象被创建仅仅是用来使用每个 Java 对象持有的锁。 有时程序员使用一个对象的锁来实现额外的原子操作， 实际上称为客户端锁定。如你所见，客户端锁定是非常脆弱 的，通常不推荐使用。

**监视器**

• 监视器是只包含私有域的类。

• 每个监视器类的对象有一个相关的锁。

• 使用该锁对所有的方法进行加锁。

**Volatile域**

volatile关键字为实例域的同步访问提供了一种免锁机制。如果声明一个域为 volatile, 那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。

Volatile 变量不能提供原子性。例如， 方法 public void flipDone() { done = !done; } // not atomic 不能确保翻转域中的值。不能保证读取、翻转和写入不被中断。

	private boolean done;
	public synchronized boolean isDoneO { return done; }
	public synchronized void setDoneO { done = true; }

**final变量**

还有一种情况可以安全地访问一个共享域， 即这个域声明为 final 时

	final Map<String, Double>accounts = new HashKap<>()；

**原子性**

java.util.concurrent.atomic 包中有很多类使用了很高效的机器级指令（而不是使用 锁）来保证其他操作的原子性。

例如，Atomiclnteger 类提供了方法 incrementAndGet 和 decrementAndGet, 它们分别以原子方式将一个整数自增或自减。

```java
	public static AtomicLong nextNumber=new AtomicLong();
	
	long id=nextNumber.incrementAndGet();
```

incrementAndGet方法以原子方式将 AtomicLong 自增， 并返回自增后的值。

```java
	AtomicLong largest=new AtomicLong();
		  do {
			  oldValue=largest.get();
			  newValue=Math.max(oldvalue,observed);
		  }while(!largest.compareAndSet(oldValue, newValue));
```

**死锁**

 有可能会因为每一个线程要等待更多的钱款存人而导致所有线程都被阻塞。这样的状态 称为死锁（deadlock）。

**线程局部变量**

 用空间换时间，不同的线程使用自己的对象。

要避免共享变量， 使用 ThreadLocal 辅助类为各个线程提供各自的实例。

要为每个线程构造一个实例，

	public static final ThreadLocal<SimpleDateFormat> dateformat=
	new ThreadLocal.withInitial(()->new SimpleDateFormat("yyyy-mm-dd"));


String dateStamp = dateFormat.get().format(new Date()); 在一个给定线程中首次调用 get 时， 会调用 initialValue方法。在此之后，get方法会返回属于当前线程的那个实例。

**锁测试与超时**

线程在调用 lock 方法来获得另一个线程所持有的锁的时候，很可能发生阻塞。应该更加 谨慎地申请锁。tryLock方法试图申请一个锁， 在成功获得锁后返回 true, 否则， 立即返回 false, 而且线程可以立即离开去做其他事情：

	if(mylock.tryLock()){
	    try{}
	    finally{mylock.unlock();}
	}


可以调用 tryLock 时，使用超时参数：

	if (myLock.tryLock(100, TineUnit.MILLISECONDS)) ...

在等待一个条件时，也可以提供一个超时：

	myCondition.await(100, TineUniBILLISECONDS))

**读/ 写锁**

允许对读者线程共享访问是合适的。当然，写者线程依然必须是互斥访问的。

下面是使用读 / 写锁的必要步骤：

```java
	1 ) 构造一个 ReentrantReadWriteLock 对象：

	private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock():

	2 ) 抽取读锁和写锁：

	private Lock readLock = rwl.readLock(); private Lock writeLock = rwl.writeLock();

	3 ) 对所有的获取方法加读锁：

	public double getTotalBalance()

	{
	    readLock.lock()； try { . . . } finally { readLock.unlock(); }
	}

	4 ) 对所有的修改方法加写锁：

	public void transfer(. . .)

	{
	    writeLock.lock(); try { . . . } finally { writeLock.unlock(); }
	}
```

**弃用stop和suspend**

stop方法天生就不安全，经验证明 suspend方法会经常导致死锁

#### 阻塞队列

阻塞队列方法分为以下3类， 这取决于当队列满或空时它们的响应方式。

如果将队列当作线程管理工具来使用， 将要用到 put 和 take 方法。

当试图向满的队列中添加或从空的队列中移出元素时，add、 remove 和 element 操作抛出异常。

当然，在一个多线程程序中，队列会在任何时候空或满，因此，一定要使用offer、poll 和 peek方法作为替代。

注： poll和 peek 方法返回空来指示失败。因此，向这些队列中插入 null 值是非法的。

LinkedBlockingQueue 的容量是没有上边界的，但是，也可以选择指定最大容量。

LinkedBlockingDeque 是一个双端的版本。

ArrayBlockingQueue 在构造时需要指定容量，并且有一个可选的参数来指定是否需要公平性。若设置了公平参数， 则那么等待了最长时间的线程会优先得到处理。通常，公平 性会降低性能，只有在确实非常需要时才使用它。

PriorityBlockingQueue是一个带优先级的队列， 而不是先进先出队列。元素按照它们的优先级顺序被移出。

DelayQueue 包含实现Delayed 接口的对象。

#### 线程安全的集合

**高效的映射、集和队列**

Collections.synchronizedMap()、Hashtable并发量小。

HashTable锁住所有数据，ConcurrentHashMap锁住一段。

ConcurrentHashMap 并发高 ConcurrentHashSet

ConcurrentSkipListMap 高并发有序。 ConcurrentSkipListSet

CopyOnWriteArrayList 和 CopyOnWriteArraySet


**映射条目的原子更新**

replace 操作，它会以原子方式用一个新值替换原值

调用 compute方法时可以提供 一个键和一个计算新值的函数。这个函数接收键和相关联的值（如果没有值，则为 mill), 它会计算新值。

首次增加一个键时通常需要做些特殊的处理。利用 merge 方法可以非常方便地做到这一点。这个方法有一个参数表示键不存在时使用的初始值。

 	map.merge(word, 1L, (existingValue, newValue) -> existingValue + newValue);

**对并发散列映射的批操作**

批操作会遍历映射，处理遍历过程中找到的元素。无须冻结当前映射的快照。

• 搜索（search) 为每个键或值提供一个函数，直到函数生成一个非 null 的结果。然后搜 索终止，返回这个函数的结果。

• 归约（reduce) 组合所有键或值， 这里要使用所提供的一个累加函数。

• for Each 为所有键或值提供一个函数

每个操作都有 4 个版本：

• operationKeys: 处理键。

• operatioriValues: 处理值。

• operation: 处理键和值。

• operatioriEntries: 处理 Map.Entry对象

 对于上述各个操作， 需要指定一个参数化阈值。如果映射包含的 元素多于这个阈值， 就会并行完成批操作。如果希望批操作在一个线程中运行，可以使用阈 值 Long.MAX_VALUE。

```java
	String result=map.search(threhold,(k,v)->v>1000?k:null);

	//forEach方法有两种形式。第一个只为各个映射条目提供一个消费者函数， 例如:
	map.forEach(threhold,(k,v)->System.out.println(k+"->"+v));
	//第二种形式还有一个转换器函数，这个函数要先提供，其结果会传递到消费者： 
	map.forEach(threhold,(k,v)->k+"->"+v,System.out::println);

	Long sum=map.reduceValues(threhold,Long::sum);

	Integer maxlength=map.reduceKeys(threshold,String::length,Integer::sum);
 
```

forEach 方法有两种形式。第一个只为各个映射条目提供一个消费者函数；第二种形式还有一个转换器函数，这个函数要先提供，其结果会传递到消费者。

如果映射为空， 或者所有条目都被过滤掉， reduce 操作会返回 null。

对于 int、 long 和 double 输出还有相应的特殊化操作， 分别有后缀 Tolnt、ToLong和 ToDouble.

**并发集视图**

静态newKeySet方法会生成一个Set<K>, 这实际上是 ConcurrentHashMap<K, Boolean〉的一个包装器。（所有映射值都为 Boolean.TRUE）
						     
	Set<String> words=ConcurrentHashMap.<String>newKeySet();

这个集是可变的。 如果删除这个集的元素，这个键（以及相应的值）会从映射中删除。不过，不能向键集增加元素，因为没有相应的值可以增加。

**写数组的拷贝**

CopyOnWriteArrayList 和 CopyOnWriteArraySet 是线程安全的集合，其中所有的修改线程对底层数组进行复制。如果在集合上进行迭代的线程数超过修改线程数，

**并行数组算法**

Arrays类提供了大量并行化操作。静态 Arrays.parallelSort 方法可以对 一个基本类型值或对象的数组排序。

parallelSetAll 方法会用由一个函数计算得到的值填充一个数组。这个函数接收元素索引， 然后计算相应位置上的值。 

Arrays.parallelSort(words);

最后还有一个 parallelPrefix 方法，它会用对应一个给定结合操作的前缀的累加结果替换各个数组元素.

Arrays.parallelPrefix(values, (x, y)-> x * y) 之后，数组将包含： [1,1*2,1*2*3,...]

**较早的线程安全集合**

Vector 和 Hashtable类就提供了线程安全的动态数组和散列表的 实现。现在这些类被弃用了， 取而代之的是 AnayList 和 HashMap类。这些类不是线程安全 的，而集合库中提供了不同的机制。任何集合类都可以通过使用同步包装器（synchronization wrapper) 变成线程安全的： 

	List<E> list=Collections.synchronizedList(new ArrayList<>);
	Map<K,V> synchHashMap=Collections.synchronizeMap(new HashMap<K,V>());

 如果在另一个线程可能进行修改时要对集合进行迭代，仍然需要使用“ 客户端” 锁定：

	synchronized (synchHashMap)

	{ Iterator<K> iter = synchHashMap.keySet().iterator(); while (iter.hasNextO) . . }

#### Collable与Future

Runnable 封装一个异步运行的任务，可以把它想象成为一个没有参数和返回值的异步方法。

Callable 与 Runnable 类似，但是有返回值。Callable 接口是一个参数化的类型， 只有一 个方法 call。类型参数是返回值的类型。例如， Callable<Integer> 表示一个最终返回 Integer 对象的异步计算。

Future 保存异步计算的结果。可以启动一个计算，将 Future 对象交给某个线程，然后忘 掉它。Future 对象的所有者在结果计算好之后就可以获得它。

Future接口的方法：

```java
	public interface Future<V> 
	{ V get() throws .. 
	  V get(long timeout, TimeUnit unit) throws .. 
	  void cancel(boolean maylnterrupt); 
	   boolean isCancelled（）; 
	   boolean isDone(); } 
```

FutureTask 包装器是一种非常便利的机制， 可将 Callable转换成 Future 和 Runnable, 它 同时实现二者的接口.


#### 执行器

Executors操作Executor的一个工具类

Executor接口只有一个execute(Runnable r)函数.执行某一个任务

ExecutorService接口继承Executor接口 。在后台不停的运行，等待被扔任务。

构建一个新的线程是有一定代价的， 因为涉及与操作系统的交互。如果程序中创建了大量的生命期很短的线程，应该使用线程池（thread pool)。一个线程池中包含许多准备运行的 空闲线程。

将Runnable对象交给线程池， 就会有一个线程调用 run方法。当 run 方法退出时，线程不会死亡，而是在池中准备为下一个请求提供服务。

另一个使用线程池的理由是减少并发线程的数目。创建大量线程会大大降低性能甚至使虚拟机崩溃。如果有一个会创建许多线程的算法，应该使用一个线程数“ 固定的” 线程池以限制并发线程的总数。 执行器（Executors) 类有许多静态工厂方法用来构建线程池。

**线程池**

 newScheduledThreadPool

 newCachedThreadPool

 newFixedThreadPool

 newSingleThreadExecutor

 newWorkStealingPool 工作窃取：偷工作的线程池。这个线程main函数结束了，可能线程还在运行，可能看不到输出。每个线程维护自己的队列，当某个任务完成自己的任务之后，去偷别线程的任务自动执行。使用ForkJoin实现。

 ForkJoinPool 分叉合并：任务队列BlokingQueue，维护两个队列，一个等待执行的任务队列，一个已完成的线程队列。

 newCachedThreadPool方法构建了一个线程池， 对于每个任务， 如果有空闲线程可用，立即让它执行 任务，如果没有可用的空闲线程， 则创建一个新线程。newFixedThreadPool 方法构建一个具 有固定大小的线程池。
 

可用下面的方法之一将一个 Runnable 对象或 Callable 对象提交给 ExecutorService:

Future<?> submit(Runnable task)

Future<T> submit(Runnable task, T result)

Future<T> submit(Callable<T> task) 该池会在方便的时候尽早执行提交的任务。调用 submit 时，会得到一个 Future 对象， 可 用来查询该任务的状态。 

使用连接池时应该做的事：

1) 调用 Executors 类中静态的方法 newCachedThreadPool 或 newFixedThreadPool。

2) 调用 submit 提交 Runnable 或 Callable对象。

3 ) 如果想要取消一个任务， 或如果提交 Callable 对象， 那就要保存好返回的 Future 对象。

4 ) 当不再提交任何任务时，调用 shutdown。

**预定执行**

  ScheduledExecutorService 接口具有为预定执行（Scheduled Execution) 或 重复执行任务而设计的方法。它是一种允许使用线程池机制的java.util.Timer 的泛化。

  Executors 类的 newScheduledThreadPool 和 newSingleThreadScheduledExecutor方法将返回实现了 Scheduled-ExecutorService 接口的对象。 可以预定 Runnable 或 Callable 在初始的延迟之后只运行一次。

**控制任务组**

使用执行器有更有实际意义的原因，控制一组相关任务。invokeAny方法提交所有对象到一个Callable对象的集合中，并返回某个已经完成了的任务的结果。

```java

List<Callable<T>>tasks=...;
list<Future<T>> results=executor.invokeAll(tasks);
for(Future<T>:results){
    processFurther(result.get(i));
}

```

用 ExecutorCompletionService 来进行排列。 用常规的方法获得一个执行器。然后，构建一个ExecutorCompletionService， 提交任务给完成服务。

```java

ExecutorCompletionService<T> service = new ExecutorCompletionServiceo(executor)；
for (Callable<T> task : tasks) 
   service,submit(task);
 
for (int i = 0; i < tasks.size()；i++) 
  processFurther(service.take().get())； 
  
```

**Fork-Join框架**

另外一些应用可能对每个处理器内核分别使用一个线程，来完成计算密集型任务，如图像或视频处理。

fork-join poll的任务必须是fork-join task。一般使用fork-join task的子类Recuesive Action（没有返回值）和Recursive Task（有返回值)

要采用框架可用的一种方式完成这种递归计算，需要提供一个扩展RecursiveTask()的类（如果计算会生成一个类型为T 的结果）或者提供一个扩展RecursiveAction() 的类（如果不生成任何结果)。

再覆盖compute 方法来生成并调用子任务，然后合并其结果.

在后台，fork-join 框架使用了一种有效的智能方法来平衡可用线程的工作负载，这种方法称为工作密取（work stealing)。每个工作线程都有一个双端队列 ( deque) 来完成任务。

一个工作线程将子任务压人其双端队列的队头。（只有一个线程可以访问队头，所以不需要加锁。）一个工作线程空闲时，它会从另一个双端队列的队尾“ 密取” 一个任务。由于大的子任务都在队尾，这种密取很少出现。

**可完成Future**


#### 同步器

java.util.concurrent 包包含了几个能帮助人们管理相互合作的线程集的类。这些机制具有为线程之间的共用集结点模式，提供的“ 预置功能” 。如果有一个相互合作的线程集满足这些行为模式之一， 那么应该直接 重用合适的库类而不要试图提供手工的锁与条件的集合。

信号量： 一个信号量管理许多的许可证（permit )。为了通过信号量，线程通过调用acquire 请求许可。

倒计时门栓： 一个倒计时门栓（CountDownLatch) 让一个线程集等待直到计数变为0。倒计时门栓是一次性的。一旦计数为0, 就不能再重用了。

障栅： CyclicBarrier 类实现了一个集结点（rendezvous) 称为障栅（barrier)。当所有部分都准备好时，需要把结果组合在一起。当一个线 程完成了它的那部分任务后， 我们让它运行到障栅处。一旦所有的线程都到达了这个障栅， 障栅就撤销，线程就可以继续运行。

首先，构造一个障栅，并给出参与的线程数：

	CyclicBarrier barrier = new CydicBarrier(nthreads);
	
每一个线程做一些工作，完成后在障栅上调用await :

	public void runO
	{
	doWorkO ;
	bamer.awaitO；

await 方法有一个可选的超时参数：

	barrier.awaitdOO, TineUnit.MILLISECONDS);

可以提供一个可选的障栅动作（barrier action), 当所有线程到达障栅的时候就会执行这 一动作。

	Runnable barrierAction = ..

	CyclicBarrier barrier = new CyclicBarrier(nthreads, barrierAction);
 
障栅被称为是循环的（cyclic), 因为可以在所有等待线程被释放后被重用。

**交换器**

当两个线程在同一个数据缓冲区的两个实例上工作的时候， 就可以使用交换器 ( Exchanger) 典型的情况是， 一个线程向缓冲区填人数据， 另一个线程消耗这些数据。当它们都完成以后，相互交换缓冲区。

**同步队列**

同步队列是一种将生产者与消费者线程配对的机制。当一个线程调用 SynchronousQueue 的 put 方法时，它会阻塞直到另一个线程调用 take方法为止，反之亦然。

与 Exchanger 的情况不同， 数据仅仅沿一个方向传递，从生产者到消费者。 即使 SynchronousQueue 类实现了 BlockingQueue 接口， 
 




