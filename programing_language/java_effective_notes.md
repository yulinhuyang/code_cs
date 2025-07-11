
翻译对应： 

[Effective Java, Third Edition](https://www.jianshu.com/c/ce8cf0e13b23)

https://github.com/sjsdfg/effective-java-3rd-chinese

https://github.com/clxering/Effective-Java-3rd-edition-Chinese-English-bilingual

https://einverne.github.io/post/2016/09/effective-java.html

effective Java 读书笔记：

https://cloud.tencent.com/developer/article/1649042

https://www.zybuluo.com/caos/note/44777



### Chapter01:引言

这本书的目的是帮助编写清晰正确，可用的，健壮性灵活性高和可维护的代码，而非针对高性能。主要从对象，类，接口，泛型，枚举，流，并发和序列化等方面介绍。

### Chapter02:对象的创建和销毁

#### 1 用静态工厂方法代替构造器 

   静态工厂方法的好处有： 

  1.静态工厂方法有方法名，可避免构造方法的重载并且易读。

  2.静态工厂方法不要求每次调用都创建一个新的对象，如对于单例以及不可变对象的复用。

  3..静态工厂方法可以返回类型可以是子类对象 

  4.静态工厂方法的接收参数不同，可以返回不同的对象。 

  5.静态工厂方法可以只定义抽象方法或接口，由具体的实现类实现。如SPI技术。 

  

  服务提供者框架是提供者实现服务的系统，并且系统使得实现对客户端可用，从而将客户端从实现中分离出来。

  服务提供者框架中有三个基本组：服务接口，它表示实现；提供者注册 API，提供者用来注册实现；以及服务访问 API，客户端使用该 API 获取服务的实例。

  只提供静态工厂方法的主要限制是，没有公共或受保护构造方法的类不能被子类化。

  

   静态工厂方法的缺点主要有： 

  1.如果返回对象的类只有package-protected或private的构造方法，则工厂方法不能创建子类对象。

   2.静态工厂方法的API难找到,可使用`from,of,valueOf,instance,getIntance,create,newInstance`等方法名来命名。

#### 2  当有很多构造参数时，使用Builder模式 

  当有很多构造参数并且是可选参数的时候，使用Builder模式更加易读，并且也会比单纯的javaBean.set方法安全。

  builder模式只在有很多参数的时候才能使用，在设计阶段如果能够预想到将来多参数的情况，那么最好在最开始使用这种模式.

  总之，如果类的构造器或者静态工厂中具有多个参数，设计这种类时，Builder模式就是中不错的选择。

#### 3  用私有构造器或者枚举类型强化Singleton属性  

  单例必须保证只有一个对象实例，枚举会更加安全（不可序列化），如果单例的类必须继承抽象类的时候，只能使用前者，因为枚举类都会继承Enum类。


  ```java
  // Singleton with public final field - Page 17
  public class Elvis {
      public static final Elvis INSTANCE = new Elvis();
      private Elvis() { }
      public void leaveTheBuilding() {
          System.out.println("Whoa baby, I'm outta here!");
      }
      // This code would normally appear outside the class!
      public static void main(String[] args) {
          Elvis elvis = Elvis.INSTANCE;
          elvis.leaveTheBuilding();
      }
  }
  // Singleton with static factory - Page 17
  public class Elvis {
      private static final Elvis INSTANCE = new Elvis();
      private Elvis() { }
      public static Elvis getInstance() { return INSTANCE; }
      public void leaveTheBuilding() {
          System.out.println("Whoa baby, I'm outta here!");
      }
      // This code would normally appear outside the class!
      public static void main(String[] args) {
          Elvis elvis = Elvis.getInstance();
          elvis.leaveTheBuilding();
      }
  }
  ```

  第三种实现Singleton的方法，只需编写一个包含单个元素的枚举类型：

  ```java
  // Enum singleton - the preferred approach - page 18
  public enum Elvis {
      INSTANCE;
      public void leaveTheBuilding() {
          System.out.println("Whoa baby, I'm outta here!");
      }
      // This code would normally appear outside the class!
      public static void main(String[] args) {
          Elvis elvis = Elvis.INSTANCE;
          elvis.leaveTheBuilding();
      }
  }
  ```

  单元素的枚举类型已经成为实现Singleton的最佳方法木有之一。

#### 4  通过私有构造器强化不可实例化的能力 

  只有这样才不允许外界创建对象或者实现子类

  

####  5 优先使用依赖注入而不是绑定固定资源 

  如果一个类可能依赖多个资源，不要在实现上写死某个资源，可以使用构造方法传参，工厂类创建对象或者DI框架（Spring）等实现。

依赖项注入（dependency injection）的一种形式：字典是拼写检查器的一个依赖项，当它创建时被注入到拼写检查器
中。

```java
// Dependency injection provides flexibility and testability 
public class SpellChecker { 
    private final Lexicon dictionary; 

    public SpellChecker(Lexicon dictionary) { 
        this.dictionary = Objects.requireNonNull(dictionary); 
    } 
     
    public boolean isValid(String word) { ... } 
    public List<String> suggestions(String typo) { ... } 
} 
```


#### 6  避免创建不必要的对象  

  如对String对象的包装`new String("bikini")`(应该直接使用`"bikini"`)，计算时包装类型和基本类型的来回转换，大对象的重复创建等都是资源的浪费。

  应尽量使用基本类型的对象参与运算，复用不可变（或在使用时不会改变的）大对象，常用对象使用池化技术等技巧来避免对象的创建。

 为了提高性能，作为类初始化的一部分，将正则表达式显式编译为一个 Pattern 实例（不可变），缓存它，并在 isRomanNumeral 方法的每个调用中重复使用相同的实例：

#### 7 消除过期的对象引用 

   如下面的代码在`pop`时应释放弹出的数组元素的引用,否则会导致内存泄漏。

```java
public class Stack  {
    private Object[] elements;
    
    private static final int DEFAULT_INITIAL_CAPACITY = 16;
    
    private int size;

    public Stack{
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }
    public Object pop(){
        if(size == 0){
            throw new EmptyStackException();
        }
        Object result = elements[--size];
        //释放引用
        elements[size] = null;
        return result;
    }
.......

}
```

因为Java是有垃圾回收机制的，通常不需要手动将对象置为null。但是如果对象内部管理自己的内存分配，则需要手动释放元素的引用，（如上面的例子，只有数组将元素置空了，元素对应的对象才能被回收）否则会导致内存泄漏。

在支持垃圾回收的语言中，内存泄漏是很隐蔽的（称这类内存泄漏为“无意识的对象保持（unintentional object retention）”更为恰当。

此外，缓存，监听器和其他回调函数也可能导致内存泄漏，可借助性能分析工具来分析这类问题。

（ heap profiler）的调试工具

####  8 避免使用finalizer和cleaner  

  不要使用finalizer和cleaner来做对象的清理工作，因为他们的回收时间无法控制（回收对象是放在队列中，同时于具体的引用类型相关）。没有实现`AutoClosable`接口的对象回收速度快。

  Finalizer存在finalizer攻击，可通过增加一个空实现的finalize方法解决。在Java中，可使用cleaner来实现native方法分配的堆外内存的回收（DirectByteBuffer）。

  总的来说，还是要慎用。关于Java的引用，可参考： [Java Reference详解](https://links.jianshu.com/go?to=https%3A%2F%2Fmy.oschina.net%2Frobinyao%2Fblog%2F829983)

####  9  优先使用try-with-resources而不是try-finally  

  `try-with-resources`的书写更加简洁优雅，同时可解决`try-finally`资源关闭失败的情况。使用`try-with-resources`的资源必须实现`AutoClosable`接口。


  不要被System.gc和System.runFinalization这两个方法所诱惑，他们确实增加了终结方法被执行的机会，但是他们不保证终结方法一定被执行。唯一声称保证终结方法被执行的方法是System.runFinalizersOnExit，以及他臭名昭著的孪生兄弟Runtime.runFinalizersOnExit。

  这两个方法都有致命的缺陷，都被废弃了。

```java
 // try-with-resources on multiple resources - short and sweet 
static void copy(String src, String dst) throws IOException { 
    try (InputStream   in = new FileInputStream(src); 
         OutputStream out = new FileOutputStream(dst)) { 
        byte[] buf = new byte[BUFFER_SIZE]; 
        int n; 
        while ((n = in.read(buf)) >= 0) 
            out.write(buf, 0, n); 
    } 
} 
```

### Chapter03: Object类的方法

Object类是所有类的父类，它定义了一些非final的方法，如`equals`,`hashCode`,`toString`等,这些方法有它们自己的规定。此外，`Comparable.compareTo`方法虽然不是Object的方法，但是它也经常出现在任何同类对象中用来比较两个对象，以下也会讨论。

#### 10  覆盖equals时请遵守通用约定  

  **不覆盖equals的情况：**

  1. 每一个类实例对象都是唯一的。

  2. 如果对象没有比较是否相等的必要，就不用重写equals方法，如`java.util.regex.Pattern`就没必要。 

  3. 父类重写的equals方法已经合适使用了，就不需要子类再重写。如`AbstractMap`。 

  4. 如果类是private或者package-private的访问权限，则没有必要暴露equals方法，也就不用重写。 

 所以value classes（也就是需要比较类对象内容的类）适合重写equals方法，重写equals方法应该满足以下特性： 

	 1.自反性，即`x.equals(x)==true`。 
	
	 2.对称性，即如果`x.equals(y)==y.equals(x)`。 
	
	 3.传递性，即如果`x.equals(y)==y.equals(z)==true`,则`x.equals(z)==true`。 
	
	 4.一致性，即如果`x.equals(y)==true`,x,y值不改变的话，`x.equals(y)==true`一直成立。 
	
	 5.如果x是不为null的引用对象，则`x.equals(null)==false`。 
	
	 一般重写equals方法时需要先对比引用，再对比类型，然后挨个对比成员变量的值是否相等。如`AbstractList.equals`：

  **实现高质量equals方法的诀窍：**

     1. 使用==操作符检查“参数是否为这个对象的引用”；
     
     2. 使用instanceof操作符检查“参数是否为正确的类型”；
     
     3. 把参数转换成正确的类型；
     
     4. 对于该类中的每个“关键”域，检查参数中的域是否与该对象中对应的域相匹配（为了获得最佳性能，应该先比较最有可能不一致的域，或者开销最低的域，最理想的情况是两个条件同时满足的域）；
     
     5. 当编写完equals方法后，应该问自己三个问题：它是否是对称的、传递的和一致的？
     
        - 覆盖equals时总要覆盖hashCode。
    	
        - 不要企图让equals方法过于智能。
    	
        - 不要将equals声明中的Object对象替换为其他类型。

```java
//AbstractList.equals
public boolean equals(Object o) {
        if (o == this)
            return true;
        if (!(o instanceof List))
            return false;

        ListIterator<E> e1 = listIterator();
        ListIterator<?> e2 = ((List<?>) o).listIterator();
        while (e1.hasNext() && e2.hasNext()) {
            E o1 = e1.next();
            Object o2 = e2.next();
            if (!(o1==null ? o2==null : o1.equals(o2)))
                return false;
        }
        return !(e1.hasNext() || e2.hasNext());
    }
```

#### 11  覆盖equals时总要覆盖hashCode 

  因为相同的对象必须拥有相同的hashCode。比如HashSet和HashMap的实现，会根据key的hash值分配所在entry的位置，如果两个key对象equals对比为true，而hashCode值不相同，那么会存在put进去但无法get出来的情况，这是不符合逻辑的。

  所以重写equals方式时必须重写hashCode方法。 一般重写hashCode方法时hash算法要尽量保证不同元素的hash值唯一，如`AbstractList.hashCode`:

```java
public int hashCode() {
        int hashCode = 1;
        for (E e : this)
//使用31这个质数尽可能保证了运算值的唯一性
            hashCode = 31*hashCode + (e==null ? 0 : e.hashCode());
        return hashCode;
    }
```

#### 12 始终要覆盖toString 

  `println,printf,log输出`以及debug的时候都会默认调用对象的toString方法，所以重写toString可使代码更易懂，同时toString方法返回信息应该包含对象中所有有意义的信息，并且尽量格式化，方便阅读。

#### 13  谨慎地覆盖clone 

  类必须实现`Cloneable`接口才能重写clone方法，clone方法的规定需要保证以下成立：

	```java
	x.clone() != x;
	x.clone().getClass() == x.getClass();
	//非必须
	x.clone().equals(x);
	```
	
	使用clone方法需要注意： 
	
	1.不可变的类不应该提供clone方法。 
	
	2.clone方法的功能就像是构造方法，应该确保不会对原来的对象造成影响。 
	
	3.对于final成员变量，将其作为可变对象进行clone是不太合适的，可能需要去掉final修饰。 
	
	4.对于复杂对象的clone，应该先调用super.clone，然后在组装变量之间的关联关系，如HashTable中的Entry链表引用.
	
	5.对象克隆的更好方法其实是提供copy 构造方法或者 copy工厂方法，如:
	
	```java
	//copy constructor
		public static Yum(Yum yum){
			.....
		}
	//copy factory
		public static Yum newInstance(Yum yum){
			.....
		}
	```

因为他们不依赖危险的克隆机制，也没有文档克隆要求，和final fields的使用不冲突，同时不需要抛出检查异常和强转。 对于数组，是适合使用clone的，因为它的运行时类型为Object[],不需要进行强转。如`ArrayList.copy`

	```java
	public Object clone() {
			try {
				ArrayList<?> v = (ArrayList<?>) super.clone();
				v.elementData = Arrays.copyOf(elementData, size);
				v.modCount = 0;
				return v;
			} catch (CloneNotSupportedException e) {
				// this shouldn't happen, since we are Cloneable
				throw new InternalError(e);
			}
		}
	```

#### 14 考虑实现Comparable接口 

  Comparable用来给对象排序使用，实现该接口意味着类实例对象拥有一个天然的排序方式。

  当对象拥有排序的意义时，就可以考虑实现Comparable接口。这样对象可用在基于排序的集合中，如`TreeSet,TreeMap`。

  此外，在`compareTo`实现中，不要使用`<,>`来做比较，而应该继续调用比较对象（基础对象就用包装类的compare方法）的compare或compareTo方法。

### Chapter04:类和接口

#### 15 最小化类和它的成员的访问  

 对于变量，方法，类和接口，对外界的访问级别有`private`,`package-private`,`protected`,`public`四种。

对于成员 (属性、方法、嵌套类和嵌套接口)，有四种可能的访问级别，在这里，按照可访问性从小到大列出：

private——该成员只能在声明它的顶级类内访问。

package-private——成员可以从被声明的包中的任何类中访问。从技术上讲，如果没有指定访问修饰符 (接口成员除外，它默认是公共的)，这是默认访问级别。

protected——成员可以从被声明的类的子类中访问 (受一些限制，JLS，6.6.2)，以及它声明的包中的任何类。

public——该成员可以从任何地方被访问。



使用原则是尽量减少成员的访问权限。如public类的实例变量基本不能为public的，也不应该定义public static final修饰的数组和集合变量，或对外返回这样的变量，都是线程不安全的。

#### 16 在公有类中使用访问方法而非公有域 

  因为暴露方法比暴露变量更具有实现的灵活性，同时前者客户端的破坏性相对较小。





#### 17 最小化可变性  

  为了使类成为不可变，要遵循下面五条规则：

    1. 不要提供任何会修改对象状态的方法。
    
    2. 保证类不会被扩展。
    
    3. 使所有的域都是final的。
    
    4. 使所有的域都成为私有的。
    
    5. 确保对于任何可变组件的互斥访问。

#### 18  复合优先于继承  

  继承会破坏封装性。而持有对象的实例或接口实例（组合模式/装饰者模式）比继承更加健壮，功能强大。如果子类和父类有共性，还是用继承。

  可以不扩展现有的类，而是在新类中增加一个私有域，引用现有类一个实例。这种设计叫做“复合（composition）”，因为现有的类变成新类的一个组件。

  新类中的每个实例方法都可以调用被包含的现有类实例中对应的方法，并返回他的结果。这被成为转发（forwarding），新类中的方法被成为转发方法（forwarding method）。

只有当子类真正是超类的子类型（subtype）时，才适合用继承，也就是是“is-a”关系时。



#### 19 要么设计继承并提供文档说明，要么禁止继承  

一个好的API文档应该描述一个给定的方法做了什么工作，而不是描述他是如何做到的。

  1.如果子类重写父类方法，应写明方法签名的相关信息和调用者信息等。 

  2.慎重的在protected的方法中留有hook，可能子类并不能处理好。 

  3.构造方法不应该调用重写方法。 

  4.clone和readObject方法不能调用重写方法，因为这些方法就像是构造方法一样。

#### 20 接口优于抽象类 

   接口表示拥有某种特性，一个类可实现多个接口，但是只能继承一个类。接口可使用包装类获得安全性功能性更强的类。

- 接口是定义mixin（混合类型）的理想选择。类除了实现他的“基本类型（primary type）”之外，还可以实现这个mixin类型，以表示提供了某些可供选择的行为。
- 接口允许我们构造非层次结构的类型框架。
- 通过对你导出的每个重要接口都提供一个抽象的骨架实现（skeletal implementation）类，把这个抽象类的优点结合起来。
- 简而言之，接口通常是定义允许多个实现的类型的最佳途径。如果演变的容易性比灵活性和功能更为重要的时候，应当选用抽象类。前提是必须理解并且可以接受这些局限性。

#### 21 为后代设计接口 

  接口可提供default方法给它的实现类，但是可能只有在运行期才能发现这些default方法的异常。

#### 22 接口只用于定义类型

  接口是用来表示某种类型，不要使用接口作为常量的实现，应该使用常量类或工具类来实现常量等功能。

- 当类实现接口时，接口就充当可以引用这个类的实例的类型（type）。
  因此类实现了接口，就表明客户端可以对这个类的实例实施某些动作。为了其他目的而使用接口是不恰当的。
- 常量接口（constant interface），使用这些常量的类实现这个接口，以避免用类名来修改常量名。
- 常量接口模式是对接口的不良使用。
- 如果这些常量最好被看作枚举类型的成员，使用枚举类型。否则，应该使用不可实例化的工具类来导出这些常量。
- 工具类通常要求客户端用类名来修饰这些常量名。也可以使用静态导入，避免用类名修饰常量名。

#### 23 类层次优于标签类  

  标记类是指用标记属性来区分标记对象，如用长宽高属性来区分长方形和圆。而应该分别设计长方形类和圆类（分别继承一个表示形状的类）来区分长方形和圆。

#### 24 静态成员类优于非静态成员类  

  内部类包括`静态内部类`，`成员内部类`，`匿名内部类`和`局部内部类`。 

  1.静态内部类相当于外部类的一个静态成员，它的创建不依赖于外部类，可访问外部类的所有静态成员。可作为一个`公有的帮助类`，如外部类的枚举类(Calculator.Operation.PLUS)。私有静态内部类可表示外部类的组成。如HashMap中的`Node`类，因为它并不持有外部类的引用，所以用静态实现更节省内存空间。

  2.成员内部类依赖于外部类的创建，可以调用外部类的任何成员。通常作为外部类的一个扩展类使用，如集合类中的`Iterator`实现类。 

  3.匿名内部类没有名字，是一个类的引用。之前匿名内部类可用来作为接口或抽象类的实现传入方法，但自从Java8引入Lambda表达式，Lambda表达式更适合这种场景。此外，匿名内部类可作为静态工厂方法的实例返回。 4.局部内部类使用频率最低，可定义在方法和代码块中。可参考： [详解内部类](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fchenssy%2Fp%2F3388487.html) 

#### 25 限制源文件为单个顶级类 

  这样可保证编译器读取的公有类文件是互不影响的。

### Chapter05:泛型

Java 5之后，泛型成为Java语言的一部分。没有泛型前，操作集合中的元素必须进行强转，而类型转换异常只能在运行期才能发现。泛型可以告诉编译器集合中每个元素是什么类型的，从而可以在编译期就发现了类型转换的错误。泛型使得程序更加安全，简洁明了。

#### 26 请不要使用原生态类型

也就是不要使用`Set`定义集合，这又回到了没有泛型的时代了。而应该使用`Set<Object>`,`Set<?>`,`Set<T>`等来定义集合。关于泛型类型的获取，可参考： [java Type 详解](https://www.jianshu.com/p/cae76008b36b) [Java泛型的学习和使用](https://www.jianshu.com/p/e4e4401c3d46) 

#### 27 消除非受检的警告

每一个未检查的waring都是一个潜在的运行时类型转换异常，尽量保证代码不提示这种异常。或者使用`@SuppressWarnings`注解抑制警告，同时说明原因，并尽量缩小该注解的作用范围。

#### 28 列表优于数组  

因为数组不支持创建泛型数组(如new List<E>[],new E[])，它只能保证运行时的类型安全而不是编译时的类型安全。

- 数组是具体化的（reified）。因此数组会在运行时才知道并检查他们的元素类型约束。泛型是通过擦除来实现的。因此泛型只在编译时强化他们的类型信息，并在运行时丢弃（或者擦除）他们元素的类型信息。

#### 29 优先考虑泛型

解决基于数组实现的泛型类有两种方式： 

1.使用Object[]来做成员变量,每次获取元素都进行强转，如Java的`Stack<E>`类。 

2.使用E[]来做成员变量,只有创建泛型数组的时候强转为E[]，其他添加和获取操作不用进行强转。即

```java
        E[] elements = (E[])new Object[16];
```

推荐使用第二种方式，因为它更加易读，简洁，只在创建数组时进行了一次强转。 此外，可以使用<E extends T>可使泛型元素获得原来T的功能。 总之，使用泛型类型的参数可尽量避免运行时的类型强转。

#### 30 优先考虑泛型方法  

是指用泛型类型修饰方法的形式参数和返回值，同样可以避免方法中的类型强转。


#### 31 利用有限制通配符来提升API的灵活性   

通配符类型主要包含三种： 

1.无限定通配符，形式：<?>，表示任意类型，List<?>会比List<String>具有更大的灵活性。 

2.上边界通配符，形式：<? extends E>，表示是E或者E的子类型。

3.下边界通配符，形式：<? super E>，表示是E或者E的父类型。 



PECS 表示：producer-extends, consumer-super

当方法中的形式参数使用通配符泛型类型时，遵循`PECS原则`可获得最大的灵活性。PECS是指当参数是作为生产者时，使用`<? extends E>`,当参数作为消费者时，使用`<? super E>`。如向Stack中添加元素`push`（向stack中生产元素）,使用<? extends E>；从Stack中获取元素`pop`（从stack中消费元素）,使用<? super E>。即生产上边界类型消费下边界类型的原则。

所有的comparable和comparator都是消费者。

```java
//保证放进去的元素都有E的特性
    public void pushAll(Iterable<? extends E> src){
        for (E e : src) {
            push(e);
            
        }
    }
//保证取出来的元素至多有E的特性
    public void popAll(Iterable<? super E> dst){
        while (! isEmpty()){
            dst.add(pop());
        }
    }
```

#### 32 谨慎并用泛型和可变参数  

因为可变参数是由数组实现的，当数组和泛型共同使用时，可能导致类型转换的失败。避免异常需要保证： 

1.不要向可变参数的数组中添加元素。 

2.不要使数组暴露给客户端代码,也不要使用clone。 

可参考JDK中`Arrays.asList(T... a)`,`EnumSet.of(E first, E... rest)`中的实现。如果类型安全，使用`@SafeVarags`注解这类方法。

#### 33 优先考虑类型安全的异构容器

通常我们是在集合容器中使用泛型，如List<String>，但是这类容器只能接收一种类型（String）的参数。我们可以使用`Class object`作为key，来存储更多参数类型的容器。如`Map<Class<?>,Object> map`。

### Chapter06:枚举和注解

#### 34 使用枚举代替数字常量

枚举类是继承Enum类实现的，枚举对象是`public static final`定义的（枚举对象可以是枚举类的子类实现）。同时枚举类构造方法是私有的，外界没有办法创建枚举实例，Enum类序列化相关方法会抛出异常，也就无法通过序列化创建出新的枚举对象。所以枚举对象是天然的不可变单例对象。 

枚举类的好处是易读，安全和可提供更多的功能。如何使用枚举类： 

1.枚举类应该是`public`的类，如果它和使用者紧密相关，那么应该是使用者的成员类。 

2.如果每个枚举对象都有各自特有的行为，最好是定义一个抽象方法，让枚举对象各自实现这个抽象方法。如：

```java
public enum Operation {
    PLUS{
        @Override
        public double apply(double x, double y) {
            return x + y;
        }
    },
    MINUS{
        @Override
        public double apply(double x, double y) {
            return x-y;
        }
    };
	
//由子类实现
   public abstract double apply(double x,double y);
}
```

3.当由某个特征获取枚举对象时，可返回Optional<T>对象，由客户端判断是否能获取到枚举对象。 

4.当编译期可确定常量的集合内容时，都可以使用枚举类来实现。

#### 35 用实例域代替序数

使用ordinal()方法能够获得实例在枚举的顺序，从0开始。

放弃使用序数能够更灵活的使用指定的值

如以下定义：

```java
public enum Ensemble {
    SOLO(1),
    DUET(2);
    
    private int numberOfMusicians;

    Ensemble(int size){
        this.numberOfMusicians = size;
    }
    //正确使用
    public int numberOfMusicians1(){
        return numberOfMusicians;
    }
    //错误使用
    public int numberOfMusicians(){
        return ordinal() + 1;
    }
    
}
```

Enum类中`ordinal`的设计是用来比较枚举对象的大小和EnumSet,EnumMap中使用的。

#### 36 用EnumSet代替位域 

EnumSet是对枚举类对象的数组集合包装，提供了一系列处理枚举类数组集合的方法。使用位向量数组实现，性能和bit处理相当，同时又拥有枚举类的优点。

​	

#### 37 用EnumMap代替序数索引 

EnumMap是对枚举类对象的Map数组集合包装,key为对应的枚举类对象。底层实现也是使用ordinal作为数组下标。但是相比直接使用ordinal作为数组下标的数组，EnumMap更加直观方便，同时可表示多个维度，如`EnumMap<...,EnumMap<...>>`。

 #### 38 用接口模拟可扩展的枚举  

因为枚举类默认继承Enum类，可实现多个接口来扩展枚举对象的方法。 可参考： [剖析EnumSet](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fswiftma%2Fp%2F6044718.html) [剖析EnumMap](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fswiftma%2Fp%2F6044672.html) 

#### 39 注解优先于命名模式

注解更加灵活，方便统一处理。如Lombok,Junit中的使用。

#### 40 坚持使用Override注解

否则重写了方法而不自知，可能导致错误发生。

#### 41 用标记接口定义类型

相比使用注解声明某种`通用特性`，使用接口声明会更加明确，统一。如`Serializable`接口统一表示Java的原生序列化标志。

### Chapter07: Lambda表达式和流处理

#### 42 优先使用lambdas而不是匿名类

lambda可以使代码看起来更加简洁，书写方便。对于函数接口（只有一个abstract方法的接口），不要使用匿名类创建实例，也不要序列化lambdas或者匿名类。lambdas最好是不超过几行，容易理解的代码。

#### 43 优先使用方法引用而不是lambda

Lambda 表达式常用语法是 `(argument) -> (body)`，而方法引用指的是`目标引用::方法`的形式。如下：

```java
//Lambda 表达式形式
Comparator c = (p1, p2) -> p1.getAge().compareTo(p2.getAge());
//方法引用
Comparator c = Comparator.comparing(Person::getAge);
```

使用方法引用大多时候会显得更加简洁易懂。但是当Lambda 表达式形式比方法引用更简洁的时候，使用前者。

#### 44 优先使用标准的函数接口

`java.util.function`包中提供了表示生产者（Supplier<T>）,消费者（Consumer<T>）,预测（Predicate<T>)等众多函数接口，优先使用这些函数接口。另外，含有基本类型参数的函数接口不要传入包装类型参数。自定义的函数接口使用`@FunctionalInterface`注解。

#### 45 谨慎使用Stream  

过度使用streams(流处理)会导致程序难读难维护。streams用函数对象来处理流数据（想象水流流过管道），循环代码块(循环遍历代码，如for循环，while循环)用代码块不断的重复操作。他们的操作对比：

1.代码块可读写作用域内访问的任何局部变量。Lambda只能读用`final修饰`或者`effectively final`(A variable or parameter whose value is never changed after it is initialized is effectively final.)的变量，并且不能修改任何局部变量。 

2.代码块可使用`return`，`break`，`continue`或者`抛异常`；Lambda不能做这些事情。 如果需要上述这些代码块操作的，则不适合使用streams。

streams适合做的事情为： 

1.统一的流中元素转换 

2.按照条件过滤一些元素 

3.用简单的操作（如求最小值）处理元素 

4.把元素放入集合容器中，如分组 

5.查询满足某些条件的元素集合 其实也就是`map-reduce`操作适合流式处理。

#### 46 优先选择Stream中无副作用的函数  

无副作用的函数参数是指不依赖可变状态参数，同时也不会修改任何状态的函数。这样在流处理的过程中，每阶段的处理结果只依赖于它的前一阶段的输入结果。同时不要使用`forEach`做流展示的运算。在做流的`collect`操作时，可通过`Collectors`实现一系列的集合操作，如`toList`,`toMap`,`groupingBy`等。

#### 47 Stream要优先用Collection作为返回类型 

集合既可以做集合中元素序列的流处理，也可以迭代使用。但是Stream没有实现Iterable接口，无法做迭代操作。

#### 48 谨慎使用Stream并行 

可调用`parallel`方法实现并行计算，但是对于`limit`,`collect`方法并不适合使用并行计算，因为这些操作可能导致错误的结果或灾难。而对于`reduce`或者有预操作的方法，比如`min`,`max`,`count`,`anyMatch`这些是适合使用并行计算的。同时集合类`ArrayList`,`HashMap`,`ConcurrentHashMap`,`arrays`是适合做并行计算，因为他们的数据结构主要由数组构成，可简单切分成小块数据并行运算，同时局部的数组数据引用通常在内存中是连续的。此外，只有当数据量很大时，使用cpu核数相同的线程才可能达到接近线性的速度，如机器学习和大数据处理适合使用流的并行计算。 

可参考： [什么是函数式编程思维？](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.zhihu.com%2Fquestion%2F28292740%2Fanswer%2F40336090) 

[JavaLambdaInternals](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FCarpenterLee%2FJavaLambdaInternals) 

[lambda表达式原理篇(译)](https://links.jianshu.com/go?to=https%3A%2F%2Fboykablog.readthedocs.io%2Fen%2Fmaster%2Fit%2Fjava%2Fjava8%2Flambda%2Flambda%E5%8E%9F%E7%90%86%E7%AF%87(%E8%AF%91).html) 

[浅析 Java Stream 实现原理](https://links.jianshu.com/go?to=http%3A%2F%2Fhack.xingren.com%2Findex.php%2F2018%2F10%2F17%2Fjava-stream%2F) 

[java8 函数式编程Stream 概念深入理解 Stream 运行原理 Stream设计思路](https://links.jianshu.com/go?to=https%3A%2F%2Fcloud.tencent.com%2Fdeveloper%2Farticle%2F1333533) 

### Chapter08:方法

#### 49 检查参数的有效性 

私有的方法可以使用断言来作为参数的检查方法

对参数的任何限制不见得是一件好事，有些计算和方法会隐式的执行必要的有效性检查。

同样，我们也要注意参数的有效性检查会产生或多或少的系统开销。

将参数的限制写进文档中，并至于方法的开头处，这样的习惯也是非常重要的。

#### 50 必要时进行保护性拷贝  

如方法返回对象是可变对象的话，就有可能被调用方篡改，导致数据有误。最好是使用copy对象进行参数校验，而不是原来的对象。尽量手动copy，而不是使用`clone方法`，后者可能会导致安全问题。

#### 51 谨慎设计方法签名  

1.选取合适的方法名，易懂且具有包，类的共识一致性。 

2.尽量使方法具有灵活通用性。

3.避免数量过多的方法参数，尽量不超过4个。可考虑拆成多个方法，创建帮助类，使用Builder模式等解决。 

4.方法参数最好采用接口类型，而不是具体实现类类型。 

5.使用两个元素的枚举类型作为对象，而不是boolean参数。前者方便扩展。

#### 52 慎用重载 

重载方法的确定是在编译期，重写方法的确定是在运行期，所以会选择子类的重写方法。重载容易导致编译错误，且不易读。应该尽量给方法起不同名字，避免重载。如writeBoolean(boolean),writeInt(int),writeLong(long)。

#### 53 慎用可变参数 

1.不要使用可变参数做参数个数校验，这样只会在运行时报错，且不优雅。 

2.可变参数是用来为`printf`设计的，对打印和反射都很有帮助。 

3.每次调用可变参数都会造成数组的分配和初始化。如果是为了性能考虑，可重载多参数方法，直到超过3个参数再使用可变参数，`EnumSet`的静态创建方法就是使用这种策略。


```java
  public static <E extends Enum<E>> EnumSet<E> of(E e) {
      ....
    }
 public static <E extends Enum<E>> EnumSet<E> of(E e1, E e2) {
 ....
    }
 public static <E extends Enum<E>> EnumSet<E> of(E e1, E e2, E e3) {
    ....
    }
 public static <E extends Enum<E>> EnumSet<E> of(E e1, E e2, E e3, E e4) {
        ....
    }
public static <E extends Enum<E>> EnumSet<E> of(E e1, E e2, E e3, E e4,E e5)
    {
 ....
    }
 public static <E extends Enum<E>> EnumSet<E> of(E first, E... rest) {
       ....
    }
```

#### 54 返回零长度的数组或者集合，而不是null

返回null容易造成空指针异常，返回的空数组或空集合可优化为：

```java
  private static final Object[] EMPTY_OBJECT_ARRAY = new Object[0]; 
//返回空数组
  public Object[] getObject(){
        ...
        return Collections.emptyList();
    }
//返回空集合
    public List<Object> getObject(){
       ...
        return list.toArray(EMPTY_OBJECT_ARRAY);
    }
```

#### 55 谨慎返回optinal

Optional<T>是一个不可变容器，可以持有个非null T对象引用或者没有。相比不确定返回对象是什么的时候，返回null 或者 抛出异常，返回Optional<T>是更好的方式。它有很多方法供我们在不同条件下选择不同的值进行返回,如`empty`,`get`,`orElse`,`orElseThrow`等。但是： 

1.不要再返回Optional的方法中返回null。 

2.Optional<T>中T 不应该为容器类型：比如collections，maps，streams，arrays和optionals，不应该在包着一层Optional。 

3.Optional<T>中T 不应该为包装类型，如Long等。 

4.几乎也不把Optional<T> 用在集合和数组的key，value或者element上。 

5.Optional<T>也不适合当成员变量。 

6.严格考虑性能的方法，还是返回null或者抛异常吧。

#### 56 为所有导出的API元素编写文档注释

参考JDK的文档说明。

- 为了正确地编写API文档，必须在每个被导出的类、接口、构造器、方法和域声明之前增加一个文档注释。
- 方法的文档注释应该简洁地描述出它和客户端之间的约定。列出这个方法所有的前提条件和后置条件。
- 注释也应该描述类或者方法的线程安全性。
- @code 标签可以防止代码中的’<‘和’>'被转义成html代码，并用代码字体渲染。
- @literal 标签可以防止’|’和’&'符号被处理。
- 同样，注释最好是在源码中和产生的文档中都应该是易于阅读的。
- 需要注意的是
  - 每个文档注释的第一句话，成了该注释所属元素的概要描述。
  - 同一个类或者接口中的两个成员或者构造器，不应该具有同样的概要描述。
  - 当为单行或者方法编写文档时，确保要在文档中说明所有的类型参数。
  - 但未枚举类型编写文档时，要确保在我能当中说明常量。
  - 为注解类型编写文档时，要确保在文档中说明所有成员，以及类型本身。


### Chapter09:通用编程

#### 57 将局部变量的作用域最小化 

1.第一次使用前定义。 

2.优先使用增强for循环，而不是while或者Iterator.可减少犯错。 

3.方法内容指责单一，短小。

要使局部变量的作用域最小化，最有力的方法就是在第一次使用它的地方声明。

几乎每个局部变量的声明都应该包含一个初始化的表达式。

for循环优于while循环是因为for循环的变量作用域在循环体中，所以不容易出现“剪切-粘贴”错误。

另一个优势就是for循环更加简短可读。

将局部变量的作用域最小化的方法是使方法小而集中

#### 58  for-each循环优先于传统的for循环

增强for循环易读并且不易出错。

三种情况无法使用for-each循环

- 过滤——遍历集合并删除选定元素
- 转换——遍历列表或数组，并取代它部分或者全部的元素值。
- 平行迭代——如果需要并行的遍历多个集合，就需要显示的控制迭代器或者索引变量

#### 59 了解和使用类库

JDK源码，Guava必备，不多说了。

#### 60 如果需要精确的答案，请避免使用float和double

如对于金额计算，使用BigDecimal进行小数运算，int/long可根据金额大小选择，使用分做单位进行四舍五入运算。

关于浮点数的原理可参考： [程序员必知之浮点数运算原理详解](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Ftercel_zhang%2Farticle%2Fdetails%2F52537726) 

#### 61 基本类型优先于装箱基本类型


1.包装类型除了拥有值，还有引用。原始类型更加简单性能高。如下面的代码由于不停的拆箱封箱可导致性能问题：

```java
 public static void main(String[] args) {
        Long sum = 0L;
        for (long i = 0;i<Integer.MAX_VALUE;i++){
//不停的拆箱封箱
            sum += i;
        }
        System.out.println(sum);
    }
```

2.原始类型有默认值，而包装类型初始值为null，进行运算时可能会报NullPointerException。

 3.原始类型可使用==比较，包装类型的==总是false。

- 基本类型和装箱基本类型有三个主要区别
  - 基本类型只有值，装箱基本类型则具有与它们的值不同的同一性。
  - 基本类型只有功能完备的值，而每个装箱基本类型除了它对应基本类型的所有功能值之外，还有个非功能值null
  - 基本类型通常比装箱基本类型更节省时间和空间。
- 对装箱基本类型运用==操作符几乎总是错误的。
- 当在一项操作中，混合使用基本类型和装箱基本类型时，装箱基本类型就会自动拆箱，这种情况无一例外。
- 什么时候使用装箱基本类型呢？
  - 最为集合中的元素、键和值，你无法将基本类型放在集合中。
  - 在参数化类型中，必须使用装箱基本类型作为类型参数。
  - 在进行反射的方法调用时，必须使用装箱基本类型。
- 自动装箱减少了使用装箱基本类型的繁琐性，但是并没有减少它的风险。



#### 62 如果其他类型更适合，则尽量避免使用字符串

1.枚举对象时最好用enum。 

2.聚合对象用聚合的方式展示，如用私有静态类表示元素。而非字符串连接。

3.基本类型如int，不要用String表示。

- 字符串不适合代替其他的值类型。
- 字符串不适合代替枚举类型。
- 字符串不适合代替聚集类型。
- 字符串不适合代替能力表（capabilities）

#### 63 了解字符串连接的性能

慎重使用“+”连接字符串** 

"+"连接字符串复杂度是n^2,因为字符串是不可变的，每次都需要copy。使用`StringBuilder`代替，它的复杂度为线性的。或者使用字符数组，或者只调用一次连接字符串。

#### 64 通过接口引用对象  

对象使用接口类引用，而不是实现类引用

对象使用接口类引用会更加灵活。如果找不到合适的接口类引用，尽量选择可提供功能的父类实现。

如果有合适的接口类型存在，那么对于参数、返回值、变量和域来说，就都应该使用接口类型进行声明。

它会使程序更加灵活。

如果没有合适的接口存在，完全可以用类而不是接口来引用对象。

#### 65 接口优先于反射机制  

对于反射对象，优先使用接口或父类引用，而不是反射类的引用

- 反射机制能够获得Constructor、Method、Field实例，并通过调用实例上的方法狗仔地城类的实例、调用底层类的方法、并访问底层类中的域。当然这样也需要付出代价。
  - 丧失了编译时类型检查的好处。包括异常检查。
  - 执行反射访问所需要的代码非常笨拙和冗长。
  - 性能损失。反射方法调用比普通方法慢了许多。
- 通常，普通应用程序在运行时不应该以反射方式访问对象。
- 如果只是以非常有限的形式使用反射机制，虽然也要付出少许代价，但是可以获得许多好处。
- 如果你编写的程序必须要与编译时未知的类一起工作，如果有可能，就应该仅仅使用反射机制来实例化对象。

#### 66 谨慎地使用本地方法

 native方法相比Java普通方法是不安全的，也更加依赖于平台，不容易使用，bug可能导致整个应用失败。

谨慎地使用本地方法（Java Native Interface）允许Java应用程序可以调用本地方法，也就是调用本地程序设计语言，比如C或者C++来编写的特殊方法。

本地方法的三种用途

- 它们提供了“访问特定于平台的机制”的能力
- 提供了访问遗留代码库的能力，从而访问遗留数据。
- 可以通过本地语言，编写应用程序中注重性能的部分，以提高系统的性能

#### 67 谨慎地进行优化 

 提到设计的时候考虑好架构性能，不要图快。配合性能检测工具，优化算法等。

- 要努力编写好的程序而不是快的程序。
- 努力避免那些限制性能的设计决策。
- 要考虑API设计决策的性能后果。
- 为获得好的性能而对API进行包装，这是一种非常不好的想法。
- 在每次试图做优化之前和之后，要对性能进行测量。
- 再多的底层优化也无法弥补算法的选择不当。

#### 68 遵守普遍接受的命名惯例

Package/module: 全小写（com.junit.jupiter） 

Class/Interface: 驼峰命名（Stream,HashMap） 

Method/Filed: 首字母小写，驼峰命名（remove.getCrc） 

Constant Filed: 全大写，单词用下划线隔开（MIN_VALUE） 

Local Variable: 首字母小写，驼峰命名（i,houseName） 

Type Parameter（类型参数）: 大写单字母（T,E）

- T表示任意的类型
- E表示集合的元素类型
- K和V表示映射的键和值类型
- X表示异常。
- 任何类型的序列可以是T、U、V或者T1、T2、T3
  - 对于返回boolean值的方法，齐明明往往以单词“is”开头。

### Chapter10:异常

#### 69 针对异常的情况才使用异常   


异常会阻止JVM的一些优化，影响性能。异常就是用来处理异常的。

#### 70 对可恢复的情况使用受检异常，对编程错误使用运行时异常

不要使用errors，errors是JVM表明资源缺乏等导致无法运行用的。定义所有未检查的异常应该继承RuntimeException。

错误往往被JVM保留用于表示资源不足、约束失败，或者其他使程序无法执行的条件，由于这已经是个几乎被普遍接受的惯例，因此最好不要在实现任何新的Error子类。所以你事先的所有未受检的跑出结构都应该是RuntimeException的子类，不管是直接的还是间接的。

受检异常往往指明了可恢复的条件，所以，对于这样的异常，提供一些辅助的方法尤其重要，通过这些方法，调用者可以获得一些有助于恢复的信息。

#### 71 避免不必要地使用受检异常  

有些异常需要抛给调用者解决。

#### 72 优先使用标准的异常 

大家都懂的异常，简单明了。如NullPointerException。

#### 73 抛出与抽象对应的异常 

```java
            try {
                ... //调用其他低级方法
                
            }catch (LowerLevelException e){
                throw new HigherLevelException(...);
            }
```

#### 74 每个方法抛出的所有异常都要建立文档 

始终要单独地生命受检的异常，并且利用Javadoc的@throws标记，准确地记录下抛出每个异常的条件。

使用Javadoc的@throws标签记录下一个方法可能抛出的每个未受捡异常，但是不要使用throws关键字将未受检的异常包含在方法的声明中。

如果一个类中的许多方法处于同样的原因而抛出同一个异常，在该类的文档注释中对这个异常建立文档，这是可以接受的，

#### 75 在细节消息中包含失败-捕获信息 

 如所有导致这个异常的参数，但也不能包含敏感信息，如密码等。

#### 76 努力使失败保持原子性

类似回滚机制，当异常发生时，应该保证对象被调用前的状态，或者文档详细说明。

- 失败的方法调用应该使对象保持在被调用之前的状态。
- 四种方式保持失败的原子性
  - 设计一个不可变对象
  - 调整计算处理过程的顺序，是的任何可能会失败的计算部分都在对象状态被修改之前发生。
  - 第三种或的失败原子性的办法就是编写一段恢复代码（recovery code）
  - 最后一种在对象的一份临时拷贝上操作，当操作完成后再用临时拷贝中的结果代替对象的内容。
  - 一般而言，作为方法规范的一部分，产生的任何异常都应该让对象保持在该方法调用之前的状态

#### 77 不要忽略异常 

catch完啥也不干是不允许的，否则就要说明为啥要这么干。

### Chapter11:并发

#### 78 同步访问共享的可变数据  

 Java内存模型（JMM）通过happens-before原则，synchronized等关键字定义了Java线程之间的可见性，原子性，顺序性等问题。同步访问的保证有以下方式：

1. synchronized使用时，保证进入同步的条件也是同步的，也就是说与同步有关的读写操作都是同步的。 

2. 第1点可以优化为进入同步的条件必须保证可见性，如双重检查实现单例懒加载方式时，静态单例对象用volatile修饰。

3. 类似j.u.c包中的同步实现，使用volatile+cas(乐观锁原理-版本控制)操作可实现lock-free且线程安全的同步。

#### 79 避免过度同步 

1. 同步方法中不要调用外来方法，如可重写的方法，观察者方法，因为这些方法可能产生异常，死锁等问题导致同步失败。

2. 如果无法实现高性能的并发同步，将同步交给客户端去做，并写好文档说明。对于静态变量，必须自己实现同步。自实现同步时可考虑`lock splitting,lock stripping,nonblocking（无锁）技术`。

#### 80 executor、task和stream优先于线程

优先使用线程池(ThreadPoolExecutor/ForkJoinPool)，任务（Runnable/Callable,ForkJoinTask)而不是多个单线程

池化技术加任务队列能够提高效率，节省资源，更好的抽象等。可参考： [聊聊并发（八）——Fork/Join 框架介绍](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.infoq.cn%2Farticle%2Ffork-join-introduction) [线程池与ForkJoin比较](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.jdon.com%2Fperformance%2Fthreadpool-forkjoin.html) [Java8的CompletableFuture](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.jdon.com%2Fidea%2Fjava%2Fcompletablefuture.html) 

#### 81 并发工具优先于wait和notify 

既然正确地使用wait和notify比较困难，就应该用更高级的并发工具来代替

java.util.concurrent中更高级的工具分成三类

- Executor Framework
- 并发集合（Concurrent Collection）
  - 并发集合中不可能排除并发活动；将他锁定没有什么作用，只会是程序速度变慢。
- 同步器（Synchronizer）

对于间歇式的定时，始终应该优先使用System.nanoTime而不是System.currentTimeMills，System.nanoTime更加准确也更加精确，他不收系统的实时时钟的调整所影响

一般情况下，你应该有限使用notifyAll，而不是notify。如果使用notify，请一定小心，以确保程序的活性。

#### 82 线程安全性的文档化

文档要说明方法的线程安全级别，如：

`不可变`（String/Long/BigInteger）,

`无条件线程安全`(AtomicLong/ConcurrentHashMap),

`有条件线程安全`（Collections.synchronized包装类需要额外保证自身迭代器的同步，否则可能fail-fast）,

`线程不安全`（ArrayList/HashMap),

`违反线程安全的`（修改静态变量时没有同步）。

 有条件线程安全需要写明什么时候需要额外同步，且应该获取什么锁进行同步。 无条件线程安全的类可以在同步方法上使用不可变私有对象锁代替类锁，可保护子类或客户端的同步方法。同时增加自身后续实现的灵活性。

#### 83 慎用延迟初始化

 懒加载适用于初始化对象开销大，且不一定会初始化该对象的场景。否则使用正常初始化更好。对于静态变量，可使用静态私有holder类来加载。对于成员变量，可使用双重检查+volatile实现。

对于延迟初始化，最好的建议“除非绝对必要，否则就不要这么做”，虽然降低了初始化类或者创建实例的开销，却增加了访问被延迟初始化的域的开销

在大多数情况下，正常初始化要优先于延迟初始化。

如果出于性能的考虑而需要对静态域使用延迟初始化，就使用lazy initialization holder class模式。

如果出于性能的考虑而需要对实例域使用延迟初始化，就是用双重检查模式（double-check idiom）

#### 84 不要依赖于线程调度器 

如果某一个程序不能工作，是因为某些线程无法像其他线程那样获得足够的CPU时间，那么，不要企图通过调用Thread.yield来“修正”该程序。这样的程序是不可移植的，因为不同的JVM对其实现不同。Thread.yield没有可测试的语义。

线程优先级是Java平台上最不可移植的特性了。

Thread.yield的唯一用途是在测试期间人为地增加程序的并发性。

使用Thread.sleep(1)代替Thread.yield来进行并发测试。

### Chapter12:序列化

首先介绍下Java原生序列化的原理：

Java原生序列化使用`ObjectInputStream.readObject`和`ObjectOutputStream.writeObject`来序列化和反序列化对象。对象必须实现`Serializable`接口，同时对象也可以自实现序列化方法`readObject`和反序列化方法`writeObject`定义对象成员变量的序列化和反序列化方式。此外对象还可实现`writeReplace`,`readResolve`,`serialVersionUID`。 

`writeReplace`主要用来定义对象具体序列化的内容，可对原来按照`readObject`方法序列化的内容进行修改替换。

 `readResolve`主要用来定义对象反序列化时的内容，可对原来按照`writeObject`方法反序列化的内容进行修改替换。 

 `serialVersionUID`是类的序列化版本号，保证能将二进制流反序列化为内存中存在的类的对象。如果不主动生成的话，在序列化反序列化过程中根据类信息动态生成，耗时且类结构不灵活。

#### 85 其他方法优先于Java序列化 

优先使用其他序列化方式代替Java自身序列化

#### 86 谨慎地实现Serializable接口

1.因为一旦使用了，以后就不好去掉了。本来就不建议使用原生序列化方式。 

2.增加产生bug和安全漏洞的风险。 

3.增加新版本发布的测试负担。

#### 87 考虑使用自定义序列化的格式

如果使用了原生序列化方式，就考虑自实现序列化对象的`readObject`和`writeObject`方法，因为Java实现的太笨重了，序列化所有东西，并且耗时不可控，可能导致安全和异常等问题。自己实现相对会减少这些风险。

#### 88 保护性地编写readObject方法

其实还是降低安全风险等问题，如变量的完整校验，不要将序列化方法重写，交给子类不可控等问题。

#### 89 对于实例控制，枚举类型优先于readResolve

对于单例对象，优先使用`枚举`而不是`readResolve`方法

 why: 枚举类对象的序列化和反序列化方式是Java语言规范的，不是由用户实现的。枚举类对象是天生的单例对象。 自实现`readResolve`方法实现单例的话，需要做很多额外工作，如将所有成员变量用`transient`修饰等，此外仍然会拥有Java序列化的缺点。

 #### 90 考虑用序列化代理代替序列化实例

也就是自实现`writeReplace`方法，好处是安全。

**服务提供者框架有三个重要的组件**

1. 服务接口——提供者实现

2. 提供者注册API——是系统用来注册实现，让客户端访问他们。

3. 服务访问API——客户端用来获取服务的实例。

4. 可选的第四个组件为服务提供者接口——负责创建其服务实现的实例。
