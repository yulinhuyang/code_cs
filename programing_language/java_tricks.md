
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


### 10 反编译

	java reflection.ReflectionTest irmerClass.lilkingClock\STimePrinter

	javap -private innerClass.W kingClock\STimePrinte



