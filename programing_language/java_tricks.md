
[适合 Java 新手的开源项目集合](https://zhuanlan.zhihu.com/p/312484956)

[狂神说java课程](https://www.kuangstudy.com/course)

[狂神说java视频教程](https://www.bilibili.com/video/BV1og4y1q7M4)

[java try-with-resource语句使用](https://www.jianshu.com/p/258c5ce1a2bd)

[阿里P8 java面试](https://github.com/Snailclimb/JavaGuide)

[JavaCore](https://github.com/dunwu/javacore)

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


### 12 异常

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





