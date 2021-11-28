#撤销功能的实现——备忘录模式（一）
      每个人都有过后悔的时候，但人生并无后悔药，有些错误一旦发生就无法再挽回，有些人一旦错过就不会再回来，有些话一旦说出口就不可能再收回，这就是人生。为了不后悔，凡事我们都需要三思而后行。说了这么多，大家可能已经晕了，不是在学设计模式吗？为什么弄出这么一堆人生感悟来，呵呵，别着急，本章将介绍一种让我们可以在软件中实现后悔机制的设计模式——备忘录模式，它是软件中的“后悔药”，是软件中的“月光宝盒”。话不多说，下面就让我们进入备忘录模式的学习。

 

## 21.1 可悔棋的中国象棋
 
|       Sunny软件公司欲开发一款可以运行在Android平台的触摸式中国象棋软件，由于考虑到有些用户是“菜鸟”，经常不小心走错棋；还有些用户因为不习惯使用手指在手机屏幕上拖动棋子，常常出现操作失误，因此该中国象棋软件要提供“悔棋”功能，用户走错棋或操作失误后可恢复到前一个步骤。如图21-1所示：<img style="width: 405px; height: 468px;" alt="" src="../../../pics///1335891072_4788.jpg" width="407" height="487">**图21-1  Android版中国象棋软件界面示意图**

**图21-1  Android版中国象棋软件界面示意图**

      如何实现“悔棋”功能是Sunny软件公司开发人员需要面对的一个重要问题，“悔棋”就是让系统恢复到某个历史状态，在很多软件中通常称之为“撤销”。下面我们来简单分析一下撤销功能的实现原理：

      在实现撤销时，首先必须保存软件系统的历史状态，当用户需要取消错误操作并且返回到某个历史状态时，可以取出事先保存的历史状态来覆盖当前状态。如图21-2所示：

<img style="width: 450px; height: 262px;" alt="" src="../../../pics///1335891078_9117.jpg" width="455" height="320">

**图21-2撤销功能示意图**

      备忘录模式正为解决此类撤销问题而诞生，它为我们的软件提供了“后悔药”，通过使用备忘录模式可以使系统恢复到某一特定的历史状态。

**【作者：刘伟  ****】**

#撤销功能的实现——备忘录模式（二）
## 21.2 备忘录模式概述

      备忘录模式提供了一种状态恢复的实现机制，使得用户可以方便地回到一个特定的历史步骤，当新的状态无效或者存在问题时，可以使用暂时存储起来的备忘录将状态复原，当前很多软件都提供了撤销(Undo)操作，其中就使用了备忘录模式。

      备忘录模式定义如下：
|**备忘录模式(Memento Pattern)：在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态。它是一种对象行为型模式，其别名为Token。**

      备忘录模式的核心是备忘录类以及用于管理备忘录的负责人类的设计，其结构如图21-3所示：

<img style="width: 581px; height: 248px;" alt="" src="../../../pics///1335891550_5966.jpg" width="494" height="247">

      在备忘录模式结构图中包含如下几个角色：

      **● Originator（原发器）：**它是一个普通类，可以创建一个备忘录，并存储它的当前内部状态，也可以使用备忘录来恢复其内部状态，一般将需要保存内部状态的类设计为原发器。

     ** ●Memento（备忘录)：**存储原发器的内部状态，根据原发器来决定保存哪些内部状态。备忘录的设计一般可以参考原发器的设计，根据实际需要确定备忘录类中的属性。需要注意的是，除了原发器本身与负责人类之外，备忘录对象不能直接供其他类使用，原发器的设计在不同的编程语言中实现机制会有所不同。

      **●Caretaker（负责人）：**负责人又称为管理者，它负责保存备忘录，但是不能对备忘录的内容进行操作或检查。在负责人类中可以存储一个或多个备忘录对象，它只负责存储对象，而不能修改对象，也无须知道对象的实现细节。

      理解备忘录模式并不难，但关键在于**如何设计备忘录类和负责人类**。由于在备忘录中存储的是原发器的中间状态，因此需要防止原发器以外的其他对象访问备忘录，特别是不允许其他对象来修改备忘录。

      下面我们通过简单的示例代码来说明如何使用Java语言实现备忘录模式：

      在使用备忘录模式时，首先应该存在一个原发器类Originator，在真实业务中，原发器类是一个具体的业务类，它包含一些用于存储成员数据的属性，典型代码如下所示：

```
package dp.memento;
public class Originator {
    private String state;

    public Originator(){}

　　// 创建一个备忘录对象
    public Memento createMemento() {
　　　　return new Memento(this);
    }

　　// 根据备忘录对象恢复原发器状态
    public void restoreMemento(Memento m) {
　　　　 state = m.state;
    }

    public void setState(String state) {
        this.state=state;
    }

    public String getState() {
        return this.state;
    }
}
```

      对于备忘录类Memento而言，它通常提供了与原发器相对应的属性（可以是全部，也可以是部分）用于存储原发器的状态，典型的备忘录类设计代码如下：

```
package dp.memento;
//备忘录类，默认可见性，包内可见
class Memento {
    private String state;

    public Memento(Originator o) {
　　　　state = o.getState();
    }

    public void setState(String state) {
        this.state=state;
    }

    public String getState() {
        return this.state;
    }
}
```

      在设计备忘录类时需要考虑其**封装性**，**除了Originator类，不允许其他类来调用备忘录类Memento的构造函数与相关方法**，如果不考虑封装性，允许其他类调用setState()等方法，将导致在备忘录中保存的历史状态发生改变，通过撤销操作所恢复的状态就不再是真实的历史状态，备忘录模式也就失去了本身的意义。

      在使用Java语言实现备忘录模式时，一般通过将Memento类与Originator类定义在同一个包(package)中来实现封装，在Java语言中可使用默认访问标识符来定义Memento类，即保证其包内可见。只有Originator类可以对Memento进行访问，而限制了其他类对Memento的访问。在 Memento中保存了Originator的state值，如果Originator中的state值改变之后需撤销，可以通过调用它的restoreMemento()方法进行恢复。

      对于负责人类Caretaker，它用于保存备忘录对象，并提供getMemento()方法用于向客户端返回一个备忘录对象，原发器通过使用这个备忘录对象可以回到某个历史状态。典型的负责人类的实现代码如下：

```
package dp.memento;
public class Caretaker {
	private Memento memento;

	public Memento getMemento() {
		return memento;
	}

	public void setMemento(Memento memento) {
		this.memento=memento;
	}
}
```

      在Caretaker类中不应该直接调用Memento中的状态改变方法，它的作用仅仅用于存储备忘录对象。将原发器备份生成的备忘录对象存储在其中，当用户需要对原发器进行恢复时再将存储在其中的备忘录对象取出。
|    <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td> |**思考**能否通过原型模式来创建备忘录对象？系统该如何设计？

能否通过原型模式来创建备忘录对象？系统该如何设计？



**【作者：刘伟  ****】**

#撤销功能的实现——备忘录模式（三）
## 21.3 完整解决方案

      为了实现撤销功能，Sunny公司开发人员决定使用备忘录模式来设计中国象棋软件，其基本结构如图21-4所示：

<img style="width: 616px; height: 329px;" alt="" src="../../../pics///1335892140_9528.jpg" width="618" height="339">

      在图21-4中，Chessman充当原发器，ChessmanMemento充当备忘录，MementoCaretaker充当负责人，在Caretaker中定义了一个ChessmanMemento类型的对象，用于存储备忘录。完整代码如下所示：

```
//象棋棋子类：原发器
class Chessman {
	private String label;
	private int x;
	private int y;

	public Chessman(String label,int x,int y) {
		this.label = label;
		this.x = x;
		this.y = y;
	}

	public void setLabel(String label) {
		this.label = label; 
	}

	public void setX(int x) {
		this.x = x; 
	}

	public void setY(int y) {
		this.y = y; 
	}

	public String getLabel() {
		return (this.label); 
	}

	public int getX() {
		return (this.x); 
	}

	public int getY() {
		return (this.y); 
	}
	
    //保存状态
	public ChessmanMemento save() {
		return new ChessmanMemento(this.label,this.x,this.y);
	}
	
    //恢复状态
	public void restore(ChessmanMemento memento) {
		this.label = memento.getLabel();
		this.x = memento.getX();
		this.y = memento.getY();
	}
}

//象棋棋子备忘录类：备忘录
class ChessmanMemento {
	private String label;
	private int x;
	private int y;

	public ChessmanMemento(String label,int x,int y) {
		this.label = label;
		this.x = x;
		this.y = y;
	}

	public void setLabel(String label) {
		this.label = label; 
	}

	public void setX(int x) {
		this.x = x; 
	}

	public void setY(int y) {
		this.y = y; 
	}

	public String getLabel() {
		return (this.label); 
	}

	public int getX() {
		return (this.x); 
	}

	public int getY() {
		return (this.y); 
	}	
}

//象棋棋子备忘录管理类：负责人
class MementoCaretaker {
	private ChessmanMemento memento;

	public ChessmanMemento getMemento() {
		return memento;
	}

	public void setMemento(ChessmanMemento memento) {
		this.memento = memento;
	}
}
```

      编写如下客户端测试代码：

```
class Client {
	public static void main(String args[]) {
		MementoCaretaker mc = new MementoCaretaker();
		Chessman chess = new Chessman("车",1,1);
		display(chess);
		mc.setMemento(chess.save()); //保存状态		
		chess.setY(4);
		display(chess);
		mc.setMemento(chess.save()); //保存状态
		display(chess);
		chess.setX(5);
		display(chess);
		System.out.println("******悔棋******");	
		chess.restore(mc.getMemento()); //恢复状态
		display(chess);
	}
	
	public static void display(Chessman chess) {
		System.out.println("棋子" + chess.getLabel() + "当前位置为：" + "第" + chess.getX() + "行" + "第" + chess.getY() + "列。");
	}
}
```

      编译并运行程序，输出结果如下： 
|棋子车当前位置为：第1行第1列。棋子车当前位置为：第1行第4列。棋子车当前位置为：第1行第4列。棋子车当前位置为：第5行第4列。******悔棋******棋子车当前位置为：第1行第4列。

棋子车当前位置为：第1行第4列。

棋子车当前位置为：第5行第4列。

棋子车当前位置为：第1行第4列。

**【作者：刘伟  ****】**
#撤销功能的实现——备忘录模式（四）
## 21.4 实现多次撤销

      Sunny软件公司开发人员通过使用备忘录模式实现了中国象棋棋子的撤销操作，但是使用上述代码只能实现一次撤销，因为在负责人类中只定义一个备忘录对象来保存状态，后面保存的状态会将前一次保存的状态覆盖，但有时候用户需要撤销多步操作。如何实现多次撤销呢？本节将提供一种多次撤销的解决方案，那就是在负责人类中定义一个集合来存储多个备忘录，每个备忘录负责保存一个历史状态，在撤销时可以对备忘录集合进行逆向遍历，回到一个指定的历史状态，而且还可以对备忘录集合进行正向遍历，实现重做(Redo)操作，即取消撤销，让对象状态得到恢复。

      改进之后的中国象棋棋子撤销功能结构图如图21-5所示：

<img style="width: 704px; height: 370px;" alt="" src="../../../pics///1335892489_9232.jpg" width="645" height="364">

      在图21-5中，我们对负责人类MementoCaretaker进行了修改，在其中定义了一个ArrayList类型的集合对象来存储多个备忘录，其代码如下所示：

```
import java.util.*;

class MementoCaretaker {
    //定义一个集合来存储多个备忘录
	private ArrayList mementolist = new ArrayList();

	public ChessmanMemento getMemento(int i) {
		return (ChessmanMemento)mementolist.get(i);
	}

	public void setMemento(ChessmanMemento memento) {
		mementolist.add(memento);
	}
}
```

      编写如下客户端测试代码：

```
class Client {
private static int index = -1; //定义一个索引来记录当前状态所在位置
	private static MementoCaretaker mc = new MementoCaretaker();

	public static void main(String args[]) {
		Chessman chess = new Chessman("车",1,1);
		play(chess);		
		chess.setY(4);
		play(chess);
		chess.setX(5);
		play(chess);	
		undo(chess,index);
		undo(chess,index);	
		redo(chess,index);
		redo(chess,index);
	}
	
    //下棋
	public static void play(Chessman chess) {
		mc.setMemento(chess.save()); //保存备忘录
		index ++; 
		System.out.println("棋子" + chess.getLabel() + "当前位置为：" + "第" + chess.getX() + "行" + "第" + chess.getY() + "列。");
	}

	//悔棋
	public static void undo(Chessman chess,int i) {
		System.out.println("******悔棋******");
		index --; 
		chess.restore(mc.getMemento(i-1)); //撤销到上一个备忘录
		System.out.println("棋子" + chess.getLabel() + "当前位置为：" + "第" + chess.getX() + "行" + "第" + chess.getY() + "列。");
	}

	//撤销悔棋
	public static void redo(Chessman chess,int i) {
		System.out.println("******撤销悔棋******");	
		index ++; 
		chess.restore(mc.getMemento(i+1)); //恢复到下一个备忘录
		System.out.println("棋子" + chess.getLabel() + "当前位置为：" + "第" + chess.getX() + "行" + "第" + chess.getY() + "列。");
	}
} 
```

      编译并运行程序，输出结果如下：
|棋子车当前位置为：第1行第1列。棋子车当前位置为：第1行第4列。棋子车当前位置为：第5行第4列。******悔棋******棋子车当前位置为：第1行第4列。******悔棋******棋子车当前位置为：第1行第1列。******撤销悔棋******棋子车当前位置为：第1行第4列。******撤销悔棋******棋子车当前位置为：第5行第4列。

棋子车当前位置为：第1行第4列。

******悔棋******

******悔棋******

******撤销悔棋******

******撤销悔棋******

 
|    <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td>**** |**扩展**本实例只能实现最简单的Undo和Redo操作，并未考虑对象状态在操作过程中出现分支的情况。如果在撤销到某个历史状态之后，用户再修改对象状态，此后执行Undo操作时可能会发生对象状态错误，大家可以思考其产生原因。【注：可将对象状态的改变绘制成一张树状图进行分析。】在实际开发中，可以使用链表或者堆栈来处理有分支的对象状态改变，大家可通过链表或者堆栈对上述实例进行改进。

本实例只能实现最简单的Undo和Redo操作，并未考虑对象状态在操作过程中出现分支的情况。如果在撤销到某个历史状态之后，用户再修改对象状态，此后执行Undo操作时可能会发生对象状态错误，大家可以思考其产生原因。【注：可将对象状态的改变绘制成一张树状图进行分析。】



**【作者：刘伟  ****】**


#撤销功能的实现——备忘录模式（五）
## 21.5 再谈备忘录的封装

      备忘录是一个很特殊的对象，只有原发器对它拥有控制的权力，负责人只负责管理，而其他类无法访问到备忘录，因此我们需要对备忘录进行封装。

      为了实现对备忘录对象的封装，需要对备忘录的调用进行控制，对于原发器而言，它可以调用备忘录的所有信息，允许原发器访问返回到先前状态所需的所有数据；对于负责人而言，只负责备忘录的保存并将备忘录传递给其他对象；对于其他对象而言，只需要从负责人处取出备忘录对象并将原发器对象的状态恢复，而无须关心备忘录的保存细节。理想的情况是只允许生成该备忘录的那个原发器访问备忘录的内部状态。

      在实际开发中，原发器与备忘录之间的关系是非常特殊的，它们要分享信息而不让其他类知道，实现的方法因编程语言的不同而有所差异，在C++中可以使用friend关键字，让原发器类和备忘录类成为友元类，互相之间可以访问对象的一些私有的属性；在Java语言中可以将原发器类和备忘录类放在一个包中，让它们之间满足默认的包内可见性，也可以将备忘录类作为原发器类的内部类，使得只有原发器才可以访问备忘录中的数据，其他对象都无法使用备忘录中的数据。
|    <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td>**** |**思考**如何使用内部类来实现备忘录模式？

如何使用内部类来实现备忘录模式？



## 21.6 备忘录模式总结

      备忘录模式在很多软件的使用过程中普遍存在，但是在应用软件开发中，它的使用频率并不太高，因为现在很多基于窗体和浏览器的应用软件并没有提供撤销操作。如果需要为软件提供撤销功能，备忘录模式无疑是一种很好的解决方案。在一些字处理软件、图像编辑软件、数据库管理系统等软件中备忘录模式都得到了很好的应用。

 

**      1.主要优点**

      备忘录模式的主要优点如下：

      (1)它提供了一种状态恢复的实现机制，使得用户可以方便地回到一个特定的历史步骤，当新的状态无效或者存在问题时，可以使用暂时存储起来的备忘录将状态复原。

      (2)备忘录实现了对信息的封装，一个备忘录对象是一种原发器对象状态的表示，不会被其他代码所改动。备忘录保存了原发器的状态，采用列表、堆栈等集合来存储备忘录对象可以实现多次撤销操作。

 

**      2.主要缺点**

      备忘录模式的主要缺点如下：

      资源消耗过大，如果需要保存的原发器类的成员变量太多，就不可避免需要占用大量的存储空间，每保存一次对象的状态都需要消耗一定的系统资源。

 

**      3.适用场景**

      在以下情况下可以考虑使用备忘录模式：

      (1)保存一个对象在某一个时刻的全部状态或部分状态，这样以后需要时它能够恢复到先前的状态，实现撤销操作。

      (2)防止外界对象破坏一个对象历史状态的封装性，避免将对象历史状态的实现细节暴露给外界对象。
|    <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td>**** |**练习**Sunny软件公司正在开发一款RPG网游，为了给玩家提供更多方便，在游戏过程中可以设置一个恢复点，用于保存当前的游戏场景，如果在后续游戏过程中玩家角色“不幸牺牲”，可以返回到先前保存的场景，从所设恢复点开始重新游戏。试使用备忘录模式设计该功能。

Sunny软件公司正在开发一款RPG网游，为了给玩家提供更多方便，在游戏过程中可以设置一个恢复点，用于保存当前的游戏场景，如果在后续游戏过程中玩家角色“不幸牺牲”，可以返回到先前保存的场景，从所设恢复点开始重新游戏。试使用备忘录模式设计该功能。

 

 #对象间的联动——观察者模式（一）
       **  观察者模式是设计模式中的“超级模式”，其应用随处可见，在之后几篇文章里，我将向大家详细介绍观察者模式。**

 

      “红灯停，绿灯行”，在日常生活中，交通信号灯装点着我们的城市，指挥着日益拥挤的城市交通。当红灯亮起，来往的汽车将停止；而绿灯亮起，汽车可以继续前行。在这个过程中，交通信号灯是汽车（更准确地说应该是汽车驾驶员）的观察目标，而汽车是观察者。随着交通信号灯的变化，汽车的行为也将随之而变化，一盏交通信号灯可以指挥多辆汽车。如图22-1所示：

<img src="../../../pics///1341499237_8810.jpg" width="423" height="488" alt="" style="width:462px; height:434px">

**图22-1   交通信号灯与汽车示意图**

**【插曲：本图是根据网络上一张图PS的，但改为黑白图片之后我把那张彩色的原始图片删除了，后悔ing...，怎么根据黑白图片查询彩色图片啊，这样的工具，有木有！！那张彩色的原图很漂亮，有木有人能够帮我找一找，<img alt="大哭" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/wail.gif">】**

      在软件系统中，有些对象之间也存在类似交通信号灯和汽车之间的关系，一个对象的状态或行为的变化将导致其他对象的状态或行为也发生改变，它们之间将产生联动，正所谓“触一而牵百发”。为了更好地描述对象之间存在的这种一对多（包括一对一）的联动，观察者模式应运而生，它定义了对象之间一种一对多的依赖关系，让一个对象的改变能够影响其他对象。本章我们将学习用于实现对象间联动的观察者模式。

## 22.1  多人联机对战游戏的设计
|       Sunny软件公司欲开发一款多人联机对战游戏（类似魔兽世界、星际争霸等游戏），在该游戏中，多个玩家可以加入同一战队组成联盟，当战队中某一成员受到敌人攻击时将给所有其他盟友发送通知，盟友收到通知后将作出响应。       Sunny软件公司开发人员需要提供一个设计方案来实现战队成员之间的联动。 

      Sunny软件公司开发人员需要提供一个设计方案来实现战队成员之间的联动。

       Sunny软件公司开发人员通过对系统功能需求进行分析，发现在该系统中战队成员之间的联动过程可以简单描述如下：

      **联盟成员受到攻击--&gt;发送通知给盟友--&gt;盟友作出响应**。

      如果按照上述思路来设计系统，由于联盟成员在受到攻击时需要通知他的每一个盟友，因此每个联盟成员都需要持有其他所有盟友的信息，这将导致系统开销较大，因此Sunny公司开发人员决定引入一个新的角色——“战队控制中心”——来负责维护和管理每个战队所有成员的信息。当一个联盟成员受到攻击时，将向相应的战队控制中心发送求助信息，战队控制中心再逐一通知每个盟友，盟友再作出响应，如图22-2所示：

<img src="../../../pics///1341499243_2401.jpg" width="637" height="196" alt="" style="width:682px; height:205px">

**图22-2   多人联机对战游戏中对象的联动**

        在图22-2中，受攻击的联盟成员将与战队控制中心产生联动，战队控制中心还将与其他盟友产生联动。

        如何实现对象之间的联动？如何让一个对象的状态或行为改变时，依赖于它的对象能够得到通知并进行相应的处理？

        别着急，本章所介绍的观察者模式将为对象之间的联动提供一个优秀的解决方案，下面就让我们正式进入观察者模式的学习。

 【作者：刘伟    】


#对象间的联动——观察者模式（二）
## 22.2  观察者模式概述

      观察者模式是使用频率最高的设计模式之一，它用于建立一种对象与对象之间的依赖关系，一个对象发生改变时将自动通知其他对象，其他对象将相应作出反应。在观察者模式中，发生改变的对象称为观察目标，而被通知的对象称为观察者，一个观察目标可以对应多个观察者，而且这些观察者之间可以没有任何相互联系，可以根据需要增加和删除观察者，使得系统更易于扩展。

      观察者模式定义如下：
| **观察者模式(Observer Pattern)：定义对象之间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。观察者模式的别名包括发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。观察者模式是一种对象行为型模式。** 

      观察者模式结构中通常包括观察目标和观察者两个继承层次结构，其结构如图22-3所示：

<img src="../../../pics///1341501815_4830.jpg" width="519" height="500" alt="" style="width:463px; height:442px">

**图22-3  观察者模式结构图**

      在观察者模式结构图中包含如下几个角色：

      ●  Subject（目标）：目标又称为主题，它是指被观察的对象。在目标中定义了一个观察者集合，一个观察目标可以接受任意数量的观察者来观察，它提供一系列方法来增加和删除观察者对象，同时它定义了通知方法notify()。目标类可以是接口，也可以是抽象类或具体类。

      ●  ConcreteSubject（具体目标）：具体目标是目标类的子类，通常它包含有经常发生改变的数据，当它的状态发生改变时，向它的各个观察者发出通知；同时它还实现了在目标类中定义的抽象业务逻辑方法（如果有的话）。如果无须扩展目标类，则具体目标类可以省略。

      ●  Observer（观察者）：观察者将对观察目标的改变做出反应，观察者一般定义为接口，该接口声明了更新数据的方法update()，因此又称为抽象观察者。

      ●  ConcreteObserver（具体观察者）：在具体观察者中维护一个指向具体目标对象的引用，它存储具体观察者的有关状态，这些状态需要和具体目标的状态保持一致；它实现了在抽象观察者Observer中定义的update()方法。通常在实现时，可以调用具体目标类的attach()方法将自己添加到目标类的集合中或通过detach()方法将自己从目标类的集合中删除。

**      观察者模式描述了如何建立对象与对象之间的依赖关系，以及如何构造满足这种需求的系统。**观察者模式包含观察目标和观察者两类对象，一个目标可以有任意数目的与之相依赖的观察者，一旦观察目标的状态发生改变，所有的观察者都将得到通知。作为对这个通知的响应，每个观察者都将监视观察目标的状态以使其状态与目标状态同步，这种交互也称为发布-订阅(Publish-Subscribe)。观察目标是通知的发布者，它发出通知时并不需要知道谁是它的观察者，可以有任意数目的观察者订阅它并接收通知。

      下面通过示意代码来对该模式进行进一步分析。首先我们定义一个抽象目标Subject，典型代码如下所示：

```
import java.util.*;
abstract class Subject {
    //定义一个观察者集合用于存储所有观察者对象
protected ArrayList observers&lt;Observer&gt; = new ArrayList();

//注册方法，用于向观察者集合中增加一个观察者
	public void attach(Observer observer) {
    observers.add(observer);
}

    //注销方法，用于在观察者集合中删除一个观察者
	public void detach(Observer observer) {
    observers.remove(observer);
}

    //声明抽象通知方法
	public abstract void notify();
}
```

      具体目标类ConcreteSubject是实现了抽象目标类Subject的一个具体子类，其典型代码如下所示：

```
class ConcreteSubject extends Subject {
    //实现通知方法
	public void notify() {
        //遍历观察者集合，调用每一个观察者的响应方法
		for(Object obs:observers) {
			((Observer)obs).update();
		}
	}	
}
```

      抽象观察者角色一般定义为一个接口，通常只声明一个update()方法，为不同观察者的更新（响应）行为定义相同的接口，这个方法在其子类中实现，不同的观察者具有不同的响应方法。抽象观察者Observer典型代码如下所示：

```
interface Observer {
    //声明响应方法
	public void update();
}
```

      在具体观察者ConcreteObserver中实现了update()方法，其典型代码如下所示：

```
class ConcreteObserver implements Observer {
    //实现响应方法
	public void update() {
		//具体响应代码
	}
}
```

      在有些更加复杂的情况下，**具体观察者类ConcreteObserver的update()方法在执行时需要使用到具体目标类ConcreteSubject中的状态（属性）**，因此在ConcreteObserver与ConcreteSubject之间有时候还存在关联或依赖关系，在ConcreteObserver中定义一个ConcreteSubject实例，通过该实例获取存储在ConcreteSubject中的状态。如果ConcreteObserver的update()方法不需要使用到ConcreteSubject中的状态属性，则可以对观察者模式的标准结构进行简化，在具体观察者ConcreteObserver和具体目标ConcreteSubject之间无须维持对象引用。如果在具体层具有关联关系，系统的扩展性将受到一定的影响，增加新的具体目标类有时候需要修改原有观察者的代码，在一定程度上违反了“开闭原则”，但是如果原有观察者类无须关联新增的具体目标，则系统扩展性不受影响。

 
|     <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td>| **思考** 观察者模式是否符合“开闭原则”？【从增加具体观察者和增加具体目标类两方面考虑。】 

观察者模式是否符合“开闭原则”？【从增加具体观察者和增加具体目标类两方面考虑。】



【作者：刘伟    】 

#对象间的联动——观察者模式（三）
## 23.3 完整解决方案

      为了实现对象之间的联动，Sunny软件公司开发人员决定使用观察者模式来进行多人联机对战游戏的设计，其基本结构如图22-4所示：

<img alt="" src="../../../pics///1341503929_8319.jpg" width="597" height="413" style="width:687px; height:449px">

**图22-4   多人联机对战游戏结构图**

      在图22-4中，AllyControlCenter充当目标类，ConcreteAllyControlCenter充当具体目标类，Observer充当抽象观察者，Player充当具体观察者。完整代码如下所示：

```
import java.util.*;

//抽象观察类
interface Observer {
	public String getName();
	public void setName(String name);
	public void help(); //声明支援盟友方法
	public void beAttacked(AllyControlCenter acc); //声明遭受攻击方法
}

//战队成员类：具体观察者类
class Player implements Observer {
	private String name;

	public Player(String name) {
		this.name = name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public String getName() {
		return this.name;
	}
	
    //支援盟友方法的实现
	public void help() {
		System.out.println("坚持住，" + this.name + "来救你！");
	}
	
    //遭受攻击方法的实现，当遭受攻击时将调用战队控制中心类的通知方法notifyObserver()来通知盟友
	public void beAttacked(AllyControlCenter acc) {
        System.out.println(this.name + "被攻击！");
        acc.notifyObserver(name);		
	}
}

//战队控制中心类：目标类
abstract class AllyControlCenter {
	protected String allyName; //战队名称
	protected ArrayList&lt;Observer&gt; players = new ArrayList&lt;Observer&gt;(); //定义一个集合用于存储战队成员
	
	public void setAllyName(String allyName) {
		this.allyName = allyName;
	}
	
	public String getAllyName() {
		return this.allyName;
	}
	
    //注册方法
	public void join(Observer obs) {
		System.out.println(obs.getName() + "加入" + this.allyName + "战队！");
		players.add(obs);
	}
	
    //注销方法
	public void quit(Observer obs) {
		System.out.println(obs.getName() + "退出" + this.allyName + "战队！");
		players.remove(obs);
	}
	
    //声明抽象通知方法
	public abstract void notifyObserver(String name);
}

//具体战队控制中心类：具体目标类
class ConcreteAllyControlCenter extends AllyControlCenter {
	public ConcreteAllyControlCenter(String allyName) {
		System.out.println(allyName + "战队组建成功！");
		System.out.println("----------------------------");
		this.allyName = allyName;
	}
	
    //实现通知方法
	public void notifyObserver(String name) {
		System.out.println(this.allyName + "战队紧急通知，盟友" + name + "遭受敌人攻击！");
        //遍历观察者集合，调用每一个盟友（自己除外）的支援方法
        for(Object obs : players) {
            if (!((Observer)obs).getName().equalsIgnoreCase(name)) {
                ((Observer)obs).help();
            }
        }		
	}
}
```

      编写如下客户端测试代码：

```
class Client {
	public static void main(String args[]) {
		//定义观察目标对象
AllyControlCenter acc;
		acc = new ConcreteAllyControlCenter("金庸群侠");
		
        //定义四个观察者对象
		Observer player1,player2,player3,player4;
		
		player1 = new Player("杨过");
		acc.join(player1);
		
		player2 = new Player("令狐冲");
		acc.join(player2);
		
		player3 = new Player("张无忌");
		acc.join(player3);
		
		player4 = new Player("段誉");
		acc.join(player4);
		
		//某成员遭受攻击
		Player1.beAttacked(acc);
	}
}
```

      编译并运行程序，输出结果如下：
| 金庸群侠战队组建成功！ ---------------------------- 杨过加入金庸群侠战队！ 令狐冲加入金庸群侠战队！ 张无忌加入金庸群侠战队！ 段誉加入金庸群侠战队！ 杨过被攻击！ 金庸群侠战队紧急通知，盟友杨过遭受敌人攻击！ 坚持住，令狐冲来救你！ 坚持住，张无忌来救你！ 坚持住，段誉来救你！ 

----------------------------

令狐冲加入金庸群侠战队！

段誉加入金庸群侠战队！

金庸群侠战队紧急通知，盟友杨过遭受敌人攻击！

坚持住，张无忌来救你！

      在本实例中，**实现了两次对象之间的联动**，当一个游戏玩家Player对象的beAttacked()方法被调用时，将调用AllyControlCenter的notifyObserver()方法来进行处理，而在notifyObserver()方法中又将调用其他Player对象的help()方法。Player的beAttacked()方法、AllyControlCenter的notifyObserver()方法以及Player的help()方法构成了一个联动触发链，执行顺序如下所示：

**Player.beAttacked() --&gt; AllyControlCenter.notifyObserver() --&gt;Player.help()**。

【作者：刘伟   】

#对象间的联动——观察者模式（四）
## 22.4 JDK对观察者模式的支持

      观察者模式在Java语言中的地位非常重要。在JDK的java.util包中，提供了Observable类以及Observer接口，它们构成了JDK对观察者模式的支持。如图22-5所示：

**<img alt="" src="../../../pics///1341504430_1842.jpg" width="503" height="373" style="width:689px; height:377px">**

**图22-5 JDK提供的Observable类及Observer接口结构图**

**      (1)  Observer接口**

      在java.util.Observer接口中只声明一个方法，它充当抽象观察者，其方法声明代码如下所示：
| void  update(Observable o, Object arg); 

      当观察目标的状态发生变化时，该方法将会被调用，在Observer的子类中将实现update()方法，即具体观察者可以根据需要具有不同的更新行为。当调用观察目标类Observable的notifyObservers()方法时，将执行观察者类中的update()方法。

**      (2)  Observable类**

      java.util.Observable类充当观察目标类，在Observable中定义了一个向量Vector来存储观察者对象，它所包含的方法及说明见表22-1：

**表22-1 Observable类所包含方法及说明**
<td style="background:rgb(242,242,242)"> **方法名** </td><td style="background:rgb(242,242,242)"> **方法描述** </td>

**方法描述**
| Observable() | 构造方法，实例化Vector向量。 

构造方法，实例化Vector向量。
| addObserver(Observer  o) | 用于注册新的观察者对象到向量中。 

用于注册新的观察者对象到向量中。
| deleteObserver  (Observer o) | 用于删除向量中的某一个观察者对象。 

用于删除向量中的某一个观察者对象。
| notifyObservers()和notifyObservers(Object arg) | 通知方法，用于在方法内部循环调用向量中每一个观察者的update()方法。 

通知方法，用于在方法内部循环调用向量中每一个观察者的update()方法。
| deleteObservers() | 用于清空向量，即删除向量中所有观察者对象。 

用于清空向量，即删除向量中所有观察者对象。
| setChanged() | 该方法被调用后会设置一个boolean类型的内部标记变量changed的值为true，表示观察目标对象的状态发生了变化。 

该方法被调用后会设置一个boolean类型的内部标记变量changed的值为true，表示观察目标对象的状态发生了变化。
| clearChanged() | 用于将changed变量的值设为false，表示对象状态不再发生改变或者已经通知了所有的观察者对象，调用了它们的update()方法。 

用于将changed变量的值设为false，表示对象状态不再发生改变或者已经通知了所有的观察者对象，调用了它们的update()方法。
| hasChanged() | 用于测试对象状态是否改变。 

用于测试对象状态是否改变。
| countObservers() | 用于返回向量中观察者的数量。 

用于返回向量中观察者的数量。

      我们**可以直接使用Observer接口和Observable类来作为观察者模式的抽象层，再自定义具体观察者类和具体观察目标类**，通过使用JDK中的Observer接口和Observable类，可以更加方便地在Java语言中应用观察者模式。

【作者：刘伟   】

#对象间的联动——观察者模式（五）
## 22.5 观察者模式与Java事件处理

       JDK 1.0及更早版本的事件模型基于职责链模式，但是这种模型不适用于复杂的系统，因此在JDK 1.1及以后的各个版本中，事件处理模型采用**基于观察者模式的委派事件模型**(DelegationEvent Model, DEM)，即**一个Java组件所引发的事件并不由引发事件的对象自己来负责处理，而是委派给独立的事件处理对象负责**。

      在DEM模型中，目标角色（如界面组件）负责发布事件，而观察者角色（事件处理者）可以向目标订阅它所感兴趣的事件。当一个具体目标产生一个事件时，它将通知所有订阅者。事件的发布者称为**事件源(Event Source)**，而订阅者称为**事件监听器(Event Listener)**，在这个过程中还可以通过**事件对象(Event Object)**来传递与事件相关的信息，可以在事件监听者的实现类中实现事件处理，因此事件监听对象又可以称为事件处理对象。**事件源对象、事件监听对象（事件处理对象）和事件对象构成了Java事件处理模型的三要素。**事件源对象充当观察目标，而事件监听对象充当观察者。以按钮点击事件为例，其事件处理流程如下：

       (1) 如果用户在GUI中单击一个按钮，将触发一个事件（如ActionEvent类型的动作事件），JVM将产生一个相应的ActionEvent类型的事件对象，在该事件对象中包含了有关事件和事件源的信息，此时按钮是事件源对象；

       (2) 将ActionEvent事件对象传递给事件监听对象（事件处理对象），JDK提供了专门用于处理ActionEvent事件的接口ActionListener，开发人员需提供一个ActionListener的实现类（如MyActionHandler），实现在ActionListener接口中声明的抽象事件处理方法actionPerformed()，对所发生事件做出相应的处理；

       (3) 开发人员将ActionListener接口的实现类（如MyActionHandler）对象注册到按钮中，可以通过按钮类的addActionListener()方法来实现注册；

       (4) JVM在触发事件时将调用按钮的fireXXX()方法，在该方法内部将调用注册到按钮中的事件处理对象的actionPerformed()方法，实现对事件的处理。

        使用类似的方法，我们可自定义GUI组件，如包含两个文本框和两个按钮的登录组件LoginBean，可以采用如图22-6所示设计方案：

**<img src="../../../pics///1341504872_7751.jpg" width="769" height="557" alt="" style="width:617px; height:451px">**

**图22-6 自定义登录组件结构图【省略按钮、文本框等界面组件】**

       图22-6中相关类说明如下：

       (1) LoginEvent是事件类，它用于封装与事件有关的信息，它不是观察者模式的一部分，但是它可以在目标对象和观察者对象之间传递数据，在AWT事件模型中，所有的自定义事件类都是java.util.EventObject的子类。

       (2) LoginEventListener充当抽象观察者，它声明了事件响应方法validateLogin()，用于处理事件，该方法也称为事件处理方法，validateLogin()方法将一个LoginEvent类型的事件对象作为参数，用于传输与事件相关的数据，在其子类中实现该方法，实现具体的事件处理。

       (3) LoginBean充当具体目标类，在这里我们没有定义抽象目标类，对观察者模式进行了一定的简化。在LoginBean中定义了抽象观察者LoginEventListener类型的对象lel和事件对象LoginEvent，提供了注册方法addLoginEventListener()用于添加观察者，**在Java事件处理中，通常使用的是一对一的观察者模式，而不是一对多的观察者模式**，也就是说，一个观察目标中只定义一个观察者对象，而不是提供一个观察者对象的集合。在LoginBean中还定义了通知方法fireLoginEvent()，该方法在Java事件处理模型中称为“点火方法”，在该方法内部实例化了一个事件对象LoginEvent，将用户输入的信息传给观察者对象，并且调用了观察者对象的响应方法validateLogin()。

      (4) LoginValidatorA和LoginValidatorB充当具体观察者类，它们实现了在LoginEventListener接口中声明的抽象方法validateLogin()，用于具体实现事件处理，该方法包含一个LoginEvent类型的参数，在LoginValidatorA和LoginValidatorB类中可以针对相同的事件提供不同的实现。

【作者：刘伟   】

#对象间的联动——观察者模式（六）
## 22.6 观察者模式与MVC

      在当前流行的MVC(Model-View-Controller)架构中也应用了观察者模式，MVC是一种架构模式，它包含三个角色：模型(Model)，视图(View)和控制器(Controller)。其中模型可对应于观察者模式中的观察目标，而视图对应于观察者，控制器可充当两者之间的中介者。当模型层的数据发生改变时，视图层将自动改变其显示内容。如图22-7所示：

<img alt="" src="../../../pics///1341505104_3429.jpg" width="317" height="395" style="width:317px; height:358px">

**图22-7 MVC结构示意图**

      在图22-7中，**模型层提供的数据是视图层所观察的对象**，在视图层中包含两个用于显示数据的图表对象，一个是柱状图，一个是饼状图，相同的数据拥有不同的图表显示方式，如果模型层的数据发生改变，两个图表对象将随之发生变化，这意味着图表对象依赖模型层提供的数据对象，因此数据对象的任何状态改变都应立即通知它们。同时，这两个图表之间相互独立，不存在任何联系，而且图表对象的个数没有任何限制，用户可以根据需要再增加新的图表对象，如折线图。在增加新的图表对象时，无须修改原有类库，满足“开闭原则”。
|     <table class="   " border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td> **** | **扩展** 大家可以查阅相关资料对MVC模式进行深入学习，如Oracle公司提供的技术文档《**Java SE Application Design With MVC**》，参考链接：http://www.oracle.com/technetwork/articles/javase/index-142890.html。 

大家可以查阅相关资料对MVC模式进行深入学习，如Oracle公司提供的技术文档《**Java SE Application Design With MVC**》，参考链接：http://www.oracle.com/technetwork/articles/javase/index-142890.html。



## 22.7 观察者模式总结

      观察者模式是一种使用频率非常高的设计模式，无论是移动应用、Web应用或者桌面应用，观察者模式几乎无处不在，它为实现对象之间的联动提供了一套完整的解决方案，凡是涉及到一对一或者一对多的对象交互场景都可以使用观察者模式。观察者模式广泛应用于各种编程语言的GUI事件处理的实现，在基于事件的XML解析技术（如SAX2）以及Web事件处理中也都使用了观察者模式。

**      1.主要优点**

      观察者模式的主要优点如下：

      (1) 观察者模式可以实现表示层和数据逻辑层的分离，定义了稳定的消息更新传递机制，并抽象了更新接口，使得可以有各种各样不同的表示层充当具体观察者角色。

      (2) 观察者模式在观察目标和观察者之间建立一个抽象的耦合。观察目标只需要维持一个抽象观察者的集合，无须了解其具体观察者。由于观察目标和观察者没有紧密地耦合在一起，因此它们可以属于不同的抽象化层次。

      (3) 观察者模式支持广播通信，观察目标会向所有已注册的观察者对象发送通知，简化了一对多系统设计的难度。

      (4) 观察者模式满足“开闭原则”的要求，增加新的具体观察者无须修改原有系统代码，在具体观察者与观察目标之间不存在关联关系的情况下，增加新的观察目标也很方便。

**      2.主要缺点**

      观察者模式的主要缺点如下：

      (1) 如果一个观察目标对象有很多直接和间接观察者，将所有的观察者都通知到会花费很多时间。

      (2) 如果在观察者和观察目标之间存在循环依赖，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。

      (3) 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。

**      3.适用场景**

      在以下情况下可以考虑使用观察者模式：

      (1) 一个抽象模型有两个方面，其中一个方面依赖于另一个方面，将这两个方面封装在独立的对象中使它们可以各自独立地改变和复用。

      (2) 一个对象的改变将导致一个或多个其他对象也发生改变，而并不知道具体有多少对象将发生改变，也不知道这些对象是谁。

      (3) 需要在系统中创建一个触发链，A对象的行为将影响B对象，B对象的行为将影响C对象……，可以使用观察者模式创建一种链式触发机制。
|     <table class="   " border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td> **** | **练习** Sunny软件公司欲开发一款实时在线股票软件，该软件需提供如下功能：当股票购买者所购买的某支股票价格变化幅度达到5%时，系统将自动发送通知（包括新价格）给购买该股票的所有股民。试使用观察者模式设计并实现该系统。 

Sunny软件公司欲开发一款实时在线股票软件，该软件需提供如下功能：当股票购买者所购买的某支股票价格变化幅度达到5%时，系统将自动发送通知（包括新价格）给购买该股票的所有股民。试使用观察者模式设计并实现该系统。



 #处理对象的多种状态及其相互转换——状态模式（一）
       “人有悲欢离合，月有阴晴圆缺”，包括人在内，很多事物都具有多种状态，而且在不同状态下会具有不同的行为，这些状态在特定条件下还将发生相互转换。就像水，它可以凝固成冰，也可以受热蒸发后变成水蒸汽，水可以流动，冰可以雕刻，蒸汽可以扩散。我们可以用UML状态图来描述H<sub>2</sub>O的三种状态，如图1所示：

<img alt="" src="../../../pics///1358692722_1117.jpg">

**图1 H<sub>2</sub>O的三种状态（未考虑临界点）**

       在软件系统中，有些对象也像水一样具有多种状态，这些状态在某些情况下能够相互转换，而且对象在不同的状态下也将具有不同的行为。为了更好地对这些具有多种状态的对象进行设计，我们可以使用一种被称之为**状态模式**的设计模式，本章我们将学习用于描述对象状态及其转换的状态模式。

 

**1. 银行系统中的账户类设计**


|       Sunny软件公司欲为某银行开发一套信用卡业务系统，银行账户(Account)是该系统的核心类之一，通过分析，Sunny软件公司开发人员发现在该系统中，账户存在三种状态，且在不同状态下账户存在不同的行为，具体说明如下：       (1) 如果账户中余额大于等于0，则账户的状态为正常状态(Normal State)，此时用户既可以向该账户存款也可以从该账户取款；       (2) 如果账户中余额小于0，并且大于-2000，则账户的状态为透支状态(Overdraft State)，此时用户既可以向该账户存款也可以从该账户取款，但需要按天计算利息；       (3) 如果账户中余额等于-2000，那么账户的状态为受限状态(Restricted State)，此时用户只能向该账户存款，不能再从中取款，同时也将按天计算利息；       (4) 根据余额的不同，以上三种状态可发生相互转换。

       (1) 如果账户中余额大于等于0，则账户的状态为正常状态(Normal State)，此时用户既可以向该账户存款也可以从该账户取款；

       (3) 如果账户中余额等于-2000，那么账户的状态为受限状态(Restricted State)，此时用户只能向该账户存款，不能再从中取款，同时也将按天计算利息；



       Sunny软件公司开发人员对银行账户类进行分析，绘制了如图2所示UML状态图：

<img alt="" src="../../../pics///1358692727_5983.jpg">

**图2 银行账户状态图**

       在图2中，NormalState表示正常状态，OverdraftState表示透支状态，RestrictedState表示受限状态，在这三种状态下账户对象拥有不同的行为，方法deposit()用于存款，withdraw()用于取款，computeInterest()用于计算利息，stateCheck()用于在每一次执行存款和取款操作后根据余额来判断是否要进行状态转换并实现状态转换，相同的方法在不同的状态中可能会有不同的实现。为了实现不同状态下对象的各种行为以及对象状态之间的相互转换，Sunny软件公司开发人员设计了一个较为庞大的账户类Account，其中部分代码如下所示：

```
class Account {

	private String state; //状态

	private int balance; //余额

	......

	

	//存款操作	

	public void deposit() {

		//存款

		stateCheck();	

	}

	

	//取款操作

	public void withdraw() {

		if (state.equalsIgnoreCase("NormalState") || state.equalsIgnoreCase("OverdraftState ")) {

			//取款

			stateCheck();

		}

		else {

			//取款受限

		}

	}

	

	//计算利息操作

	public void computeInterest() {

		if(state.equalsIgnoreCase("OverdraftState") || state.equalsIgnoreCase("RestrictedState ")) {

			//计算利息

		}

	}

	

	//状态检查和转换操作

	public void stateCheck() {

		if (balance &gt;= 0) {

			state = "NormalState";

		}

		else if (balance &gt; -2000 &amp;&amp; balance &lt; 0) {

			state = "OverdraftState";

		}

		else if (balance == -2000) {

			state = "RestrictedState";

		}

        else if (balance &lt; -2000) {

            //操作受限

        }

	}

	......

}
```

       分析上述代码，我们不难发现存在如下几个问题：

       (1) 几乎每个方法中都包含状态判断语句，以判断在该状态下是否具有该方法以及在特定状态下该方法如何实现，导致代码非常冗长，可维护性较差；

       (2) 拥有一个较为复杂的stateCheck()方法，包含大量的if…else if…else…语句用于进行状态转换，代码测试难度较大，且不易于维护；

       (3) 系统扩展性较差，如果需要增加一种新的状态，如冻结状态（Frozen State，在该状态下既不允许存款也不允许取款），需要对原有代码进行大量修改，扩展起来非常麻烦。

       为了解决这些问题，我们可以使用状态模式，在状态模式中，我们将对象在每一个状态下的行为和状态转移语句封装在一个个状态类中，通过这些状态类来分散冗长的条件转移语句，让系统具有更好的灵活性和可扩展性，**状态模式可以在一定程度上解决上述问题**。

 

【作者：刘伟 】

#处理对象的多种状态及其相互转换——状态模式（二）
## 2 状态模式概述

      状态模式**用于解决系统中复杂对象的状态转换以及不同状态下行为的封装问题**。当系统中某个对象存在多个状态，这些状态之间可以进行转换，而且对象在不同状态下行为不相同时可以使用状态模式。状态模式将一个对象的状态从该对象中分离出来，封装到专门的状态类中，使得对象状态可以灵活变化，对于客户端而言，无须关心对象状态的转换以及对象所处的当前状态，无论对于何种状态的对象，客户端都可以一致处理。

       状态模式定义如下：
|**状态模式(State Pattern)：允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。其别名为状态对象(Objects for States)，状态模式是一种对象行为型模式。**

       在状态模式中引入了抽象状态类和具体状态类，它们是状态模式的核心，其结构如图3所示：

<img alt="" src="../../../pics///1358693242_5100.jpg">

**图3 状态模式结构图**

        在状态模式结构图中包含如下几个角色：

   **    ● Context（环境类）：**环境类又称为上下文类，它是拥有多种状态的对象。由于环境类的状态存在多样性且在不同状态下对象的行为有所不同，因此将状态独立出去形成单独的状态类。在环境类中维护一个抽象状态类State的实例，这个实例定义当前状态，在具体实现时，它是一个State子类的对象。

     **  ● State（抽象状态类）：**它用于定义一个接口以封装与环境类的一个特定状态相关的行为，在抽象状态类中声明了各种不同状态对应的方法，而在其子类中实现类这些方法，由于不同状态下对象的行为可能不同，因此在不同子类中方法的实现可能存在不同，相同的方法可以写在抽象状态类中。

     **  ● ConcreteState（具体状态类）：**它是抽象状态类的子类，每一个子类实现一个与环境类的一个状态相关的行为，每一个具体状态类对应环境的一个具体状态，不同的具体状态类其行为有所不同。

       在状态模式中，我们将对象在不同状态下的行为封装到不同的状态类中，为了让系统具有更好的灵活性和可扩展性，同时对各状态下的共有行为进行封装，我们需要对状态进行抽象，引入了抽象状态类角色，其典型代码如下所示：

```
abstract class State {

    //声明抽象业务方法，不同的具体状态类可以不同的实现

	public abstract void handle();

}
```

       在抽象状态类的子类即具体状态类中实现了在抽象状态类中声明的业务方法，不同的具体状态类可以提供完全不同的方法实现，在实际使用时，在一个状态类中可能包含多个业务方法，如果在具体状态类中某些业务方法的实现完全相同，可以将这些方法移至抽象状态类，实现代码的复用，典型的具体状态类代码如下所示：

```
class ConcreteState extends State {

	public void handle() {

		//方法具体实现代码

	}

}
```

       环境类维持一个对抽象状态类的引用，通过setState()方法可以向环境类注入不同的状态对象，再在环境类的业务方法中调用状态对象的方法，典型代码如下所示：

```
class Context {

	private State state; //维持一个对抽象状态对象的引用

	private int value; //其他属性值，该属性值的变化可能会导致对象状态发生变化



    //设置状态对象

	public void setState(State state) {

		this.state = state;

	}



	public void request() {

		//其他代码

		state.handle(); //调用状态对象的业务方法

		//其他代码

	}

}
```

       环境类实际上是真正拥有状态的对象，我们只是将环境类中与状态有关的代码提取出来封装到专门的状态类中。在状态模式结构图中，环境类Context与抽象状态类State之间存在单向关联关系，在Context中定义了一个State对象。在实际使用时，它们之间可能存在更为复杂的关系，State与Context之间可能也存在依赖或者关联关系。

       在状态模式的使用过程中，一个对象的状态之间还可以进行相互转换，通常有两种实现状态转换的方式：

       (1) **统一由环境类来负责状态之间的转换**，此时，环境类还充当了状态管理器(State Manager)角色，在环境类的业务方法中通过对某些属性值的判断实现状态转换，还可以提供一个专门的方法用于实现属性判断和状态转换，如下代码片段所示：

```
	……

      public void changeState() {

		//判断属性值，根据属性值进行状态转换

      if (value == 0) {

			this.setState(new ConcreteStateA());

		}

		else if (value == 1) {

			this.setState(new ConcreteStateB());

		}

        ......

	}

    ……
```

       (2) **由具体状态类来负责状态之间的转换**，可以在具体状态类的业务方法中判断环境类的某些属性值再根据情况为环境类设置新的状态对象，实现状态转换，同样，也可以提供一个专门的方法来负责属性值的判断和状态转换。此时，状态类与环境类之间就将存在依赖或关联关系，因为状态类需要访问环境类中的属性值，如下代码片段所示：

```
	……

      public void changeState(Context ctx) {

		//根据环境对象中的属性值进行状态转换

      if (ctx.getValue() == 1) {

			ctx.setState(new ConcreteStateB());

		}

		else if (ctx.getValue() == 2) {

			ctx.setState(new ConcreteStateC());

		}

        ......

	}

    ……
```

 


|    <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td><img alt="疑问" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/doubt.gif">|**思考**理解两种状态转换方式的异同？

理解两种状态转换方式的异同？







【作者：刘伟 】

#处理对象的多种状态及其相互转换——状态模式（三）
## 3 完整解决方案

       Sunny软件公司开发人员使用状态模式来解决账户状态的转换问题，客户端只需要执行简单的存款和取款操作，系统根据余额将自动转换到相应的状态，其基本结构如图4所示：

<img alt="" src="../../../pics///1358693610_6618.jpg">

**图4 银行账户结构图**

       在图4中，Account充当环境类角色，AccountState充当抽象状态角色，NormalState、OverdraftState和RestrictedState充当具体状态角色。完整代码如下所示：

**温馨提示：代码有点长，需要有耐心！<img alt="大笑" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/laugh.gif">**

```
//银行账户：环境类

class Account {

	private AccountState state; //维持一个对抽象状态对象的引用

	private String owner; //开户名

	private double balance = 0; //账户余额

	

	public Account(String owner,double init) {

		this.owner = owner;

		this.balance = balance;

		this.state = new NormalState(this); //设置初始状态

		System.out.println(this.owner + "开户，初始金额为" + init);	

		System.out.println("---------------------------------------------");	

	}

	

	public double getBalance() {

		return this.balance;

	}

	

	public void setBalance(double balance) {

		this.balance = balance;

	}

	

	public void setState(AccountState state) {

		this.state = state;

	}

	

	public void deposit(double amount) {

		System.out.println(this.owner + "存款" + amount);

		state.deposit(amount); //调用状态对象的deposit()方法

		System.out.println("现在余额为"+ this.balance);

		System.out.println("现在帐户状态为"+ this.state.getClass().getName());

		System.out.println("---------------------------------------------");			

	}

	

	public void withdraw(double amount) {

		System.out.println(this.owner + "取款" + amount);

        state.withdraw(amount); //调用状态对象的withdraw()方法

		System.out.println("现在余额为"+ this.balance);

		System.out.println("现在帐户状态为"+ this. state.getClass().getName());		

		System.out.println("---------------------------------------------");

	}

	

	public void computeInterest()

	{

		state.computeInterest(); //调用状态对象的computeInterest()方法

	}

}



//抽象状态类

abstract class AccountState {

	protected Account acc;

	public abstract void deposit(double amount);

	public abstract void withdraw(double amount);

	public abstract void computeInterest();

	public abstract void stateCheck();

}



//正常状态：具体状态类

class NormalState extends AccountState {

	public NormalState(Account acc) {

		this.acc = acc;

	}



public NormalState(AccountState state) {

		this.acc = state.acc;

	}

		

	public void deposit(double amount) {

		acc.setBalance(acc.getBalance() + amount);

		stateCheck();

	}

	

	public void withdraw(double amount) {

		acc.setBalance(acc.getBalance() - amount);

		stateCheck();

	}

	

	public void computeInterest()

	{

		System.out.println("正常状态，无须支付利息！");

	}

	

    //状态转换

	public void stateCheck() {

		if (acc.getBalance() &gt; -2000 &amp;&amp; acc.getBalance() &lt;= 0) {

			acc.setState(new OverdraftState(this));

		}

		else if (acc.getBalance() == -2000) {

			acc.setState(new RestrictedState(this));

		}

		else if (acc.getBalance() &lt; -2000) {

			System.out.println("操作受限！");

		}	

	}   

}  



//透支状态：具体状态类

class OverdraftState extends AccountState

{

	public OverdraftState(AccountState state) {

		this.acc = state.acc;

	}

	

	public void deposit(double amount) {

		acc.setBalance(acc.getBalance() + amount);

		stateCheck();

	}

	

	public void withdraw(double amount) {

		acc.setBalance(acc.getBalance() - amount);

		stateCheck();

	}

	

	public void computeInterest() {

		System.out.println("计算利息！");

	}

	

    //状态转换

	public void stateCheck() {

		if (acc.getBalance() &gt; 0) {

			acc.setState(new NormalState(this));

		}

		else if (acc.getBalance() == -2000) {

			acc.setState(new RestrictedState(this));

		}

		else if (acc.getBalance() &lt; -2000) {

			System.out.println("操作受限！");

		}

	}

}



//受限状态：具体状态类

class RestrictedState extends AccountState {

	public RestrictedState(AccountState state) {

		this.acc = state.acc;

	}

	

	public void deposit(double amount) {

		acc.setBalance(acc.getBalance() + amount);

		stateCheck();

	}

	

	public void withdraw(double amount) {

		System.out.println("帐号受限，取款失败");

	}

	

	public void computeInterest() {

		System.out.println("计算利息！");

	}

	

    //状态转换

	public void stateCheck() {

		if(acc.getBalance() &gt; 0) {

			acc.setState(new NormalState(this));

		}

		else if(acc.getBalance() &gt; -2000) {

			acc.setState(new OverdraftState(this));

		}

	}

}
```

       编写如下客户端测试代码：

```
class Client {

	public static void main(String args[]) {

		Account acc = new Account("段誉",0.0);

		acc.deposit(1000);

		acc.withdraw(2000);

		acc.deposit(3000);

		acc.withdraw(4000);

		acc.withdraw(1000);

		acc.computeInterest();

	}

}
```

       编译并运行程序，输出结果如下：


|段誉开户，初始金额为0.0---------------------------------------------段誉存款1000.0现在余额为1000.0现在帐户状态为NormalState---------------------------------------------段誉取款2000.0现在余额为-1000.0现在帐户状态为OverdraftState---------------------------------------------段誉存款3000.0现在余额为2000.0现在帐户状态为NormalState---------------------------------------------段誉取款4000.0现在余额为-2000.0现在帐户状态为RestrictedState---------------------------------------------段誉取款1000.0帐号受限，取款失败现在余额为-2000.0现在帐户状态为RestrictedState---------------------------------------------计算利息！

---------------------------------------------

现在余额为1000.0

---------------------------------------------

现在余额为-1000.0

---------------------------------------------

现在余额为2000.0

---------------------------------------------

现在余额为-2000.0

---------------------------------------------

帐号受限，取款失败

现在帐户状态为RestrictedState

计算利息！



【作者：刘伟 】

#处理对象的多种状态及其相互转换——状态模式（四）
## 4 共享状态

      在有些情况下，多个环境对象可能需要共享同一个状态，如果希望在系统中实现多个环境对象共享一个或多个状态对象，那么需要**将这些状态对象定义为环境类的静态成员对象**。

      下面通过一个简单实例来说明如何实现共享状态：


|      如果某系统要求两个开关对象要么都处于开的状态，要么都处于关的状态，在使用时它们的状态必须保持一致，开关可以由开转换到关，也可以由关转换到开。



      可以使用状态模式来实现开关的设计，其结构如图5所示：

<img src="../../../pics///1358694073_2885.jpg" alt="">

**图5 开关及其状态设计结构图**

      开关类Switch代码如下所示：

```
class Switch {

	private static State state,onState,offState; //定义三个静态的状态对象

	private String name;

	

	public Switch(String name) {

		this.name = name;

		onState = new OnState();

		offState = new OffState();

		this.state = onState;

	}



	public void setState(State state) {

		this.state = state;

	}



	public static State getState(String type) {

		if (type.equalsIgnoreCase("on")) {

			return onState;

		}

		else {

			return offState;

		}

	}

		

    //打开开关

	public void on() {

		System.out.print(name);

		state.on(this);

	}

	

//关闭开关

	public void off() {

		System.out.print(name);

		state.off(this);

	}

}
```

       抽象状态类如下代码所示：

```
abstract class State {

	public abstract void on(Switch s);

	public abstract void off(Switch s);

}
```

       两个具体状态类如下代码所示：

```
//打开状态

class OnState extends State {

	public void on(Switch s) {

		System.out.println("已经打开！");

	}

	

	public void off(Switch s) {

		System.out.println("关闭！");

		s.setState(Switch.getState("off"));

	}

}



//关闭状态

class OffState extends State {

	public void on(Switch s) {

		System.out.println("打开！");

		s.setState(Switch.getState("on"));

	}

	

	public void off(Switch s) {

		System.out.println("已经关闭！");

	}

}
```

       编写如下客户端代码进行测试：

```
class Client {

	public static void main(String args[]) {

		Switch s1,s2;

		s1=new Switch("开关1");

		s2=new Switch("开关2");

		

		s1.on();

		s2.on();

		s1.off();

		s2.off();

		s2.on();

		s1.on();	

	}

}
```

       输出结果如下：


|开关1已经打开！开关2已经打开！**开关1关闭！****开关2已经关闭！****开关2打开！****开关1已经打开！**

开关2已经打开！

**开关2已经关闭！**

**开关1已经打开！**



       从输出结果可以得知两个开关共享相同的状态，如果第一个开关关闭，则第二个开关也将关闭，再次关闭时将输出“已经关闭”；打开时也将得到类似结果。

【作者：刘伟 】

#处理对象的多种状态及其相互转换——状态模式（五）
## 5 使用环境类实现状态转换

       在状态模式中实现状态转换时，**具体状态类可通过调用环境类Context的setState()方法进行状态的转换操作，也可以统一由环境类Context来实现状态的转换**。此时，增加新的具体状态类可能需要修改其他具体状态类或者环境类的源代码，否则系统无法转换到新增状态。但是对于客户端来说，无须关心状态类，可以为环境类设置默认的状态类，而将状态的转换工作交给具体状态类或环境类来完成，具体的转换细节对于客户端而言是透明的。

       在上面的“银行账户状态转换”实例中，我们通过具体状态类来实现状态的转换，在每一个具体状态类中都包含一个stateCheck()方法，在该方法内部实现状态的转换，除此之外，我们还可以通过环境类来实现状态转换，**环境类作为一个状态管理器，统一实现各种状态之间的转换操作**。

       下面通过一个包含**循环状态**的简单实例来说明如何使用环境类实现状态转换：
|       Sunny软件公司某开发人员欲开发一个屏幕放大镜工具，其具体功能描述如下：       用户单击“放大镜”按钮之后屏幕将放大一倍，再点击一次“放大镜”按钮屏幕再放大一倍，第三次点击该按钮后屏幕将还原到默认大小。

       用户单击“放大镜”按钮之后屏幕将放大一倍，再点击一次“放大镜”按钮屏幕再放大一倍，第三次点击该按钮后屏幕将还原到默认大小。

       可以考虑使用状态模式来设计该屏幕放大镜工具，我们定义三个屏幕状态类NormalState、LargerState和LargestState来对应屏幕的三种状态，分别是正常状态、二倍放大状态和四倍放大状态，屏幕类Screen充当环境类，其结构如图6所示：

<img alt="" src="../../../pics///1358694582_7264.jpg">

**图6 屏幕放大镜工具结构图**

       本实例核心代码如下所示：

```
//屏幕类

class Screen {

    //枚举所有的状态，currentState表示当前状态

	private State currentState, normalState, largerState, largestState;



	public Screen() {

    	this.normalState = new NormalState(); //创建正常状态对象

    	this.largerState = new LargerState(); //创建二倍放大状态对象

    	this.largestState = new LargestState(); //创建四倍放大状态对象

    	this.currentState = normalState; //设置初始状态

    	this.currentState.display();

	}

	

	public void setState(State state) {

		this.currentState = state;

	}

	

    //单击事件处理方法，封转了对状态类中业务方法的调用和状态的转换

	public void onClick() {

    	if (this.currentState == normalState) {

    		this.setState(largerState);

    		this.currentState.display();

    	}

    	else if (this.currentState == largerState) {

    		this.setState(largestState);

    		this.currentState.display();

    	}

    	else if (this.currentState == largestState) {

    		this.setState(normalState);

    		this.currentState.display();

    	}

	}

}



//抽象状态类

abstract class State {

	public abstract void display();

}



//正常状态类

class NormalState extends State{

	public void display() {

		System.out.println("正常大小！");

	}

}



//二倍状态类

class LargerState extends State{

	public void display() {

		System.out.println("二倍大小！");

	}

}



//四倍状态类

class LargestState extends State{

	public void display() {

		System.out.println("四倍大小！");

	}

}
```

       在上述代码中，所有的状态转换操作都由环境类Screen来实现，此时，环境类充当了状态管理器角色。如果需要增加新的状态，例如“八倍状态类”，需要修改环境类，这在一定程度上违背了“开闭原则”，但对其他状态类没有任何影响。

       编写如下客户端代码进行测试：

```
class Client {

	public static void main(String args[]) {

		Screen screen = new Screen();

		screen.onClick();

		screen.onClick();

		screen.onClick();

	}

}
```

       输出结果如下：


|正常大小！二倍大小！四倍大小！正常大小！

二倍大小！

正常大小！



【作者：刘伟 】
#处理对象的多种状态及其相互转换——状态模式（六）
## 6 状态模式总结

       状态模式将一个对象在不同状态下的不同行为封装在一个个状态类中，通过设置不同的状态对象可以让环境对象拥有不同的行为，而状态转换的细节对于客户端而言是透明的，方便了客户端的使用。在实际开发中，状态模式具有较高的使用频率，在**工作流和游戏开发**中状态模式都得到了广泛的应用，例如公文状态的转换、游戏中角色的升级等。

 

**       1. 主要优点**

       状态模式的主要优点如下：

       (1) **封装了状态的转换规则**，在状态模式中可以将状态的转换代码封装在环境类或者具体状态类中，可以对状态转换代码进行集中管理，而不是分散在一个个业务方法中。

       (2) **将所有与某个状态有关的行为放到一个类中**，只需要注入一个不同的状态对象即可使环境对象拥有不同的行为。

       (3) **允许状态转换逻辑与状态对象合成一体，而不是提供一个巨大的条件语句块**，状态模式可以让我们避免使用庞大的条件语句来将业务方法和状态转换代码交织在一起。

       (4) 可以**让多个环境对象共享一个状态对象**，从而减少系统中对象的个数。

 

**       2. 主要缺点**

       状态模式的主要缺点如下：

       (1) 状态模式的使用**必然会增加系统中类和对象的个数，导致系统运行开销增大**。

       (2) 状态模式的结构与实现都较为复杂，**如果使用不当将导致程序结构和代码的混乱，增加系统设计的难度**。

       (3) 状态模式**对“开闭原则”的支持并不太好**，增加新的状态类需要修改那些负责状态转换的源代码，否则无法转换到新增状态；而且修改某个状态类的行为也需修改对应类的源代码。

 

**      3. 适用场景**

      在以下情况下可以考虑使用状态模式：

      (1) 对象的行为依赖于它的状态（如某些属性值），状态的改变将导致行为的变化。

      (2) 在代码中包含大量与对象状态有关的条件语句，这些条件语句的出现，会导致代码的可维护性和灵活性变差，不能方便地增加和删除状态，并且导致客户类与类库之间的耦合增强。

 
|    <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td>**<img alt="疑问" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/doubt.gif">**|**练习**       Sunny软件公司欲开发一款纸牌游戏软件，在该游戏软件中用户角色具有入门级(Primary)、熟练级(Secondary)、高手级(Professional)和骨灰级(Final)四种等级，角色的等级与其积分相对应，游戏胜利将增加积分，失败则扣除积分。入门级具有最基本的游戏功能play() ，熟练级增加了游戏胜利积分加倍功能doubleScore()，高手级在熟练级基础上再增加换牌功能changeCards()，骨灰级在高手级基础上再增加偷看他人的牌功能peekCards()。       试使用状态模式来设计该系统。

       Sunny软件公司欲开发一款纸牌游戏软件，在该游戏软件中用户角色具有入门级(Primary)、熟练级(Secondary)、高手级(Professional)和骨灰级(Final)四种等级，角色的等级与其积分相对应，游戏胜利将增加积分，失败则扣除积分。入门级具有最基本的游戏功能play() ，熟练级增加了游戏胜利积分加倍功能doubleScore()，高手级在熟练级基础上再增加换牌功能changeCards()，骨灰级在高手级基础上再增加偷看他人的牌功能peekCards()。



 

## 7 练习

        (1) 分析如下代码：

```
class TestXYZ {

    int behaviour;

    //Getter and Setter

    ......

    public void handleAll() {

        if (behaviour == 0) { //do something }

        else if (behaviour == 1) { //do something }

        else if (behaviour == 2) { //do something }

        else if (behaviour == 3) { //do something }

        ... some more else if ...

    }

}
```

        为了提高代码的扩展性和健壮性，可以使用(    )设计模式来进行重构。

        A. Visitor（访问者）                     B. Facade（外观）

        C. Memento（备忘录）               D. State（状态）

 

       (2) 传输门是传输系统中的重要装置。传输门具有Open（打开）、Closed（关闭）、Opening（正在打开）、StayOpen（保持打开）、Closing（正在关闭）五种状态。触发状态的转换事件有click、complete和timeout三种。事件与其相应的状态转换如图7所示。

<img alt="" src="../../../pics///1358695550_1995.jpg">

**图7 传输门响应事件与其状态转换图**

       试使用状态模式对传输门进行状态模拟，要求绘制相应的类图并编程模拟实现。

 

 #算法的封装与切换——策略模式（一）
      俗话说：条条大路通罗马。在很多情况下，实现某个目标的途径不止一条，例如我们在外出旅游时可以选择多种不同的出行方式，如骑自行车、坐汽车、坐火车或者坐飞机，可根据实际情况（目的地、旅游预算、旅游时间等）来选择一种最适合的出行方式。在制订旅行计划时，如果目的地较远、时间不多，但不差钱，可以选择坐飞机去旅游；如果目的地虽远、但假期长、且需控制旅游成本时可以选择坐火车或汽车；如果从健康和环保的角度考虑，而且有足够的毅力，自行车游或者徒步旅游也是个不错的选择，<img alt="大笑" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/laugh.gif">。

      在软件开发中，我们也常常会遇到类似的情况，**实现某一个功能有多条途径，每一条途径对应一种算法，此时我们可以使用一种设计模式来实现灵活地选择解决途径，也能够方便地增加新的解决途径**。本章我们将介绍一种**为了适应算法灵活性而产生的设计模式——策略模式**。

 

## 24.1 电影票打折方案


|      Sunny软件公司为某电影院开发了一套影院售票系统，在该系统中需要为不同类型的用户提供不同的电影票打折方式，具体打折方案如下：      (1) 学生凭学生证可享受票价8折优惠；      (2) 年龄在10周岁及以下的儿童可享受每张票减免10元的优惠（原始票价需大于等于20元）；      (3) 影院VIP用户除享受票价半价优惠外还可进行积分，积分累计到一定额度可换取电影院赠送的奖品。      该系统在将来可能还要根据需要引入新的打折方式。

      (1) 学生凭学生证可享受票价8折优惠；

      (3) 影院VIP用户除享受票价半价优惠外还可进行积分，积分累计到一定额度可换取电影院赠送的奖品。



      为了实现上述电影票打折功能，Sunny软件公司开发人员设计了一个电影票类MovieTicket，其核心代码片段如下所示：

```
//电影票类

class MovieTicket {

	private double price; //电影票价格

	private String type; //电影票类型

	

	public void setPrice(double price) {

		this.price = price;

	}

	

	public void setType(String type) {

		this.type = type;

	}

	

	public double getPrice() {

		return this.calculate();

	}

	

         //计算打折之后的票价

	public double calculate() {

                  //学生票折后票价计算

		if(this.type.equalsIgnoreCase("student")) {

			System.out.println("学生票：");

		    return this.price * 0.8;

		}

                  //儿童票折后票价计算

		else if(this.type.equalsIgnoreCase("children") &amp;&amp; this.price &gt;= 20 ) {

			System.out.println("儿童票：");

		    return this.price - 10;

		}

                  //VIP票折后票价计算

		else if(this.type.equalsIgnoreCase("vip")) {

			System.out.println("VIP票：");

		    System.out.println("增加积分！");

			return this.price * 0.5;

		}

		else {

			return this.price; //如果不满足任何打折要求，则返回原始票价

		}

	}

}
```

      编写如下客户端测试代码：

```
class Client {

	public static void main(String args[]) {

		MovieTicket mt = new MovieTicket();

		double originalPrice = 60.0; //原始票价

		double currentPrice; //折后价

		

		mt.setPrice(originalPrice);

		System.out.println("原始价为：" + originalPrice);

		System.out.println("---------------------------------");

			

		mt.setType("student"); //学生票

		currentPrice = mt.getPrice();

		System.out.println("折后价为：" + currentPrice);

		System.out.println("---------------------------------");

		

		mt.setType("children"); //儿童票

		currentPrice = mt.getPrice();

		System.out.println("折后价为：" + currentPrice);

	}

}
```

      编译并运行程序，输出结果如下所示：


|原始价为：60.0---------------------------------学生票：折后价为：48.0---------------------------------儿童票：折后价为：50.0

---------------------------------

折后价为：48.0

儿童票：



      通过MovieTicket类实现了电影票的折后价计算，该方案解决了电影票打折问题，每一种打折方式都可以称为一种打折算法，更换打折方式只需修改客户端代码中的参数，无须修改已有源代码，但该方案并不是一个完美的解决方案，它至少存在如下三个问题：

**      (1) MovieTicket类的calculate()方法非常庞大，它包含各种打折算法的实现代码，在代码中出现了较长的if…else…语句，不利于测试和维护**。

**      (2) 增加新的打折算法或者对原有打折算法进行修改时必须修改MovieTicket类的源代码，违反了“开闭原则”，系统的灵活性和可扩展性较差。**

**      (3) 算法的复用性差，如果在另一个系统（如商场销售管理系统）中需要重用某些打折算法，只能通过对源代码进行复制粘贴来重用，无法单独重用其中的某个或某些算法（重用较为麻烦）。**

      如何解决这三个问题？导致产生这些问题的主要原因在于MovieTicket类职责过重，它将各种打折算法都定义在一个类中，这既不便于算法的重用，也不便于算法的扩展。因此我们需要对MovieTicket类进行重构，将原本庞大的MovieTicket类的职责进行分解，**将算法的定义和使用分离**，这就是策略模式所要解决的问题，下面将进入策略模式的学习。

【作者：刘伟 】

#算法的封装与切换——策略模式（二）
## 24.2 策略模式概述

      在策略模式中，我们可以定义一些独立的类来封装不同的算法，**每一个类封装一种具体的算法**，在这里，每一个封装算法的类我们都可以称之为一种**策略(Strategy)**，为了保证这些策略在使用时具有一致性，一般会提供一个抽象的策略类来做规则的定义，而每种算法则对应于一个具体策略类。

**      策略模式的主要目的是将算法的定义与使用分开，也就是将算法的行为和环境分开**，将算法的定义放在专门的策略类中，每一个策略类封装了一种实现算法，使用算法的环境类针对抽象策略类进行编程，符合“依赖倒转原则”。在出现新的算法时，只需要增加一个新的实现了抽象策略类的具体策略类即可。策略模式定义如下：
|**策略模式(Strategy Pattern)：定义一系列算法类，将每一个算法封装起来，并让它们可以相互替换，策略模式让算法独立于使用它的客户而变化，也称为政策模式(Policy)。策略模式是一种对象行为型模式。**

      策略模式结构并不复杂，但我们需要理解其中环境类Context的作用，其结构如图24-1所示：

<img alt="" src="../../../pics///1343811032_3729.jpg" width="819" height="306">

      在策略模式结构图中包含如下几个角色：

      ● **Context****（环境类）：**环境类是使用算法的角色，它在解决某个问题（即实现某个方法）时可以采用多种策略。在环境类中维持一个对抽象策略类的引用实例，用于定义所采用的策略。

      ● **Strategy****（抽象策略类）：**它为所支持的算法声明了抽象方法，是所有策略类的父类，它可以是抽象类或具体类，也可以是接口。环境类通过抽象策略类中声明的方法在运行时调用具体策略类中实现的算法。

      ● **ConcreteStrategy****（具体策略类）：**它实现了在抽象策略类中声明的算法，在运行时，具体策略类将覆盖在环境类中定义的抽象策略类对象，使用一种具体的算法实现某个业务处理。
|    <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td>**** |**思考**一个环境类Context能否对应多个不同的策略等级结构？如何设计？

一个环境类Context能否对应多个不同的策略等级结构？如何设计？



      策略模式是一个比较容易理解和使用的设计模式，策略模式是对算法的封装，它把算法的责任和算法本身分割开，委派给不同的对象管理。策略模式通常把一个系列的算法封装到一系列具体策略类里面，作为抽象策略类的子类。在策略模式中，对环境类和抽象策略类的理解非常重要，环境类是需要使用算法的类。在一个系统中可以存在多个环境类，它们可能需要重用一些相同的算法。

      在使用策略模式时，我们需要将算法从Context类中提取出来，首先应该创建一个抽象策略类，其典型代码如下所示：

```
abstract class AbstractStrategy {

    public abstract void algorithm(); //声明抽象算法

}
```

       然后再将封装每一种具体算法的类作为该抽象策略类的子类，如下代码所示：

```
class ConcreteStrategyA extends AbstractStrategy {

    //算法的具体实现

    public void algorithm() {

       //算法A

    }

}
```

      其他具体策略类与之类似，对于Context类而言，在它与抽象策略类之间建立一个关联关系，其典型代码如下所示：

```
class Context {

private AbstractStrategy strategy; //维持一个对抽象策略类的引用



    public void setStrategy(AbstractStrategy strategy) {

        this.strategy= strategy;

    }



    //调用策略类中的算法

    public void algorithm() {

        strategy.algorithm();

    }

}
```

      在Context类中定义一个AbstractStrategy类型的对象strategy，通过注入的方式在客户端传入一个具体策略对象，客户端代码片段如下所示：

```
……

Context context = new Context();

AbstractStrategy strategy;

strategy = new ConcreteStrategyA(); //可在运行时指定类型

context.setStrategy(strategy);

context.algorithm();

……
```

      在客户端代码中只需注入一个具体策略对象，可以将具体策略类类名存储在配置文件中，通过反射来动态创建具体策略对象，从而使得用户可以灵活地更换具体策略类，增加新的具体策略类也很方便。**策略模式提供了一种可插入式(Pluggable)算法的实现方案**。

【作者：刘伟  】

#算法的封装与切换——策略模式（三）
## 24.3 完整解决方案

      为了实现打折算法的复用，并能够灵活地向系统中增加新的打折方式，Sunny软件公司开发人员使用策略模式对电影院打折方案进行重构，重构后基本结构如图24-2所示：

<img alt="" src="../../../pics///1343811809_8784.jpg" width="735" height="374">

      在图24-2中，MovieTicket充当环境类角色，Discount充当抽象策略角色，StudentDiscount、 ChildrenDiscount 和VIPDiscount充当具体策略角色。完整代码如下所示：

```
//电影票类：环境类
class MovieTicket {
	private double price;
	private Discount discount; //维持一个对抽象折扣类的引用

	public void setPrice(double price) {
		this.price = price;
	}

    //注入一个折扣类对象
	public void setDiscount(Discount discount) {
		this.discount = discount;
	}

	public double getPrice() {
        //调用折扣类的折扣价计算方法
		return discount.calculate(this.price);
	}
}

//折扣类：抽象策略类
interface Discount {
	public double calculate(double price);
}

//学生票折扣类：具体策略类
class StudentDiscount implements Discount {
	public double calculate(double price) {
		System.out.println("学生票：");
		return price * 0.8;
	}
} 

//儿童票折扣类：具体策略类
class ChildrenDiscount implements Discount {
	public double calculate(double price) {
		System.out.println("儿童票：");
		return price - 10;
	}
} 

//VIP会员票折扣类：具体策略类
class VIPDiscount implements Discount {
	public double calculate(double price) {
		System.out.println("VIP票：");
		System.out.println("增加积分！");
		return price * 0.5;
	}
}
```

      为了提高系统的灵活性和可扩展性，我们将具体策略类的类名存储在配置文件中，并通过工具类XMLUtil来读取配置文件并反射生成对象，XMLUtil类的代码如下所示：

```
import javax.xml.parsers.*;
import org.w3c.dom.*;
import org.xml.sax.SAXException;
import java.io.*;
class XMLUtil {
//该方法用于从XML配置文件中提取具体类类名，并返回一个实例对象
	public static Object getBean() {
		try {
			//创建文档对象
			DocumentBuilderFactory dFactory = DocumentBuilderFactory.newInstance();
			DocumentBuilder builder = dFactory.newDocumentBuilder();
			Document doc;							
			doc = builder.parse(new File("config.xml")); 
		
			//获取包含类名的文本节点
			NodeList nl = doc.getElementsByTagName("className");
            Node classNode=nl.item(0).getFirstChild();
            String cName=classNode.getNodeValue();
            
            //通过类名生成实例对象并将其返回
            Class c=Class.forName(cName);
	  	    Object obj=c.newInstance();
            return obj;
        }   
        catch(Exception e) {
           	e.printStackTrace();
           	return null;
       	}
    }
}
```

      在配置文件config.xml中存储了具体策略类的类名，代码如下所示：

```
&lt;?xml version="1.0"?&gt;
&lt;config&gt;
    &lt;className&gt;StudentDiscount&lt;/className&gt;
&lt;/config&gt;
```

      编写如下客户端测试代码：

```
class Client {
	public static void main(String args[]) {
		MovieTicket mt = new MovieTicket();
		double originalPrice = 60.0;
		double currentPrice;
		
		mt.setPrice(originalPrice);
		System.out.println("原始价为：" + originalPrice);
		System.out.println("---------------------------------");
			
		Discount discount;
		discount = (Discount)XMLUtil.getBean(); //读取配置文件并反射生成具体折扣对象
		mt.setDiscount(discount); //注入折扣对象
		
		currentPrice = mt.getPrice();
		System.out.println("折后价为：" + currentPrice);
	}
}
```

      编译并运行程序，输出结果如下：


|原始价为：60.0---------------------------------学生票：折后价为：48.0

---------------------------------

折后价为：48.0



      如果需要更换具体策略类，无须修改源代码，只需修改配置文件，例如将学生票改为儿童票，只需将存储在配置文件中的具体策略类StudentDiscount改为ChildrenDiscount，如下代码所示：

```
&lt;?xml version="1.0"?&gt;
&lt;config&gt;
    &lt;className&gt;ChildrenDiscount&lt;/className&gt;
&lt;/config&gt;
```

      重新运行客户端程序，输出结果如下：


|原始价为：60.0---------------------------------儿童票：折后价为：50.0

---------------------------------

折后价为：50.0



      如果需要增加新的打折方式，原有代码均无须修改，只要增加一个新的折扣类作为抽象折扣类的子类，实现在抽象折扣类中声明的打折方法，然后修改配置文件，将原有具体折扣类类名改为新增折扣类类名即可，完全符合“开闭原则”。

【作者：刘伟  】
#算法的封装与切换——策略模式（四）
## 24.4 策略模式的两个典型应用

      策略模式实用性强、扩展性好，在软件开发中得以广泛使用，是使用频率较高的设计模式之一。下面将介绍策略模式的两个典型应用实例，一个来源于**Java SE**，一个来源于微软公司推出的演示项目**PetShop**。

      (1) Java SE的容器布局管理就是策略模式的一个经典应用实例，其基本结构示意图如图24-3所示：

## <img alt="" src="../../../pics///1343812299_2772.jpg" width="763" height="455">

**【每次看到这个LayoutManager2接口，我都在想当时Sun公司开发人员是怎么想的！<img alt="微笑" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/smile.gif">】**

      在Java SE开发中，用户需要对容器对象Container中的成员对象如按钮、文本框等GUI控件进行布局(Layout)，在程序运行期间由客户端动态决定一个Container对象如何布局，Java语言在JDK中提供了几种不同的布局方式，封装在不同的类中，如BorderLayout、FlowLayout、GridLayout、GridBagLayout和CardLayout等。在图24-3中，Container类充当环境角色Context，而LayoutManager作为所有布局类的公共父类扮演了抽象策略角色，它给出所有具体布局类所需的接口，而具体策略类是LayoutManager的子类，也就是各种具体的布局类，它们封装了不同的布局方式。

      任何人都可以设计并实现自己的布局类，只需要将自己设计的布局类作为LayoutManager的子类就可以，比如**传奇的Borland公司（现在已是传说，<img alt="难过" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/sad.gif">）**曾在JBuilder中提供了一种新的布局方式——XYLayout，作为对JDK提供的Layout类的补充。对于客户端而言，只需要使用Container类提供的setLayout()方法就可设置任何具体布局方式，无须关心该布局的具体实现。在JDK中，Container类的代码片段如下：

```
public class Container extends Component {

    ……

    LayoutManager layoutMgr;

    ……

    public void setLayout(LayoutManager mgr) {

	layoutMgr = mgr;

	……

    }

    ……

}
```

      从上述代码可以看出，Container作为环境类，针对抽象策略类LayoutManager进行编程，用户在使用时，根据“里氏代换原则”，只需要在setLayout()方法中传入一个具体布局对象即可，无须关心它的具体实现。

      (2) 除了基于Java语言的应用外，在使用其他面向对象技术开发的软件中，策略模式也得到了广泛的应用。

      在微软公司提供的演示项目PetShop 4.0中就使用策略模式来处理同步订单和异步订单的问题。在PetShop 4.0的BLL（Business Logic Layer，业务逻辑层）子项目中有一个OrderAsynchronous类和一个OrderSynchronous类，它们都继承自IOrderStrategy接口，如图24-4所示：

<img alt="" src="../../../pics///1343812646_8454.jpg" width="801" height="247">

      在图24-4中，OrderSynchronous以一种同步的方式处理订单，而OrderAsynchronous先将订单存放在一个队列中，然后再对队列里的订单进行处理，以一种异步方式对订单进行处理。BLL的Order类通过反射机制从配置文件中读取策略配置的信息，以决定到底是使用哪种订单处理方式。配置文件web.config中代码片段如下所示：

```
……

&lt;add key="OrderStrategyClass" value="PetShop.BLL.OrderSynchronous"/&gt;

……
```

     用户只需要修改配置文件即可更改订单处理方式，提高了系统的灵活性。

 

## 24.5 策略模式总结

      策略模式用于算法的自由切换和扩展，它是应用较为广泛的设计模式之一。策略模式对应于解决某一问题的一个算法族，允许用户从该算法族中任选一个算法来解决某一问题，同时可以方便地更换算法或者增加新的算法。只要涉及到算法的封装、复用和切换都可以考虑使用策略模式。

**      1. 主要优点**

      策略模式的主要优点如下：

      (1) 策略模式提供了对“开闭原则”的完美支持，用户可以在不修改原有系统的基础上选择算法或行为，也可以灵活地增加新的算法或行为。

      (2) 策略模式提供了管理相关的算法族的办法。策略类的等级结构定义了一个算法或行为族，恰当使用继承可以把公共的代码移到抽象策略类中，从而避免重复的代码。

      (3) 策略模式提供了一种可以替换继承关系的办法。如果不使用策略模式，那么使用算法的环境类就可能会有一些子类，每一个子类提供一种不同的算法。但是，这样一来算法的使用就和算法本身混在一起，不符合“单一职责原则”，决定使用哪一种算法的逻辑和该算法本身混合在一起，从而不可能再独立演化；而且使用继承无法实现算法或行为在程序运行时的动态切换。

      (4) 使用策略模式可以避免多重条件选择语句。多重条件选择语句不易维护，它把采取哪一种算法或行为的逻辑与算法或行为本身的实现逻辑混合在一起，将它们全部硬编码(Hard Coding)在一个庞大的多重条件选择语句中，比直接继承环境类的办法还要原始和落后。

      (5) 策略模式提供了一种算法的复用机制，由于将算法单独提取出来封装在策略类中，因此不同的环境类可以方便地复用这些策略类。

**      2. 主要缺点**

      策略模式的主要缺点如下：

      (1) 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时选择恰当的算法。换言之，策略模式只适用于客户端知道所有的算法或行为的情况。

      (2) 策略模式将造成系统产生很多具体策略类，任何细小的变化都将导致系统要增加一个新的具体策略类。

      (3) 无法同时在客户端使用多个策略类，也就是说，在使用策略模式时，客户端每次只能使用一个策略类，不支持使用一个策略类完成部分功能后再使用另一个策略类来完成剩余功能的情况。

**      3. 适用场景**

      在以下情况下可以考虑使用策略模式：

      (1) 一个系统需要动态地在几种算法中选择一种，那么可以将这些算法封装到一个个的具体算法类中，而这些具体算法类都是一个抽象算法类的子类。换言之，这些具体算法类均有统一的接口，根据“里氏代换原则”和面向对象的多态性，客户端可以选择使用任何一个具体算法类，并只需要维持一个数据类型是抽象算法类的对象。

      (2) 一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重条件选择语句来实现。此时，使用策略模式，把这些行为转移到相应的具体策略类里面，就可以避免使用难以维护的多重条件选择语句。

      (3) 不希望客户端知道复杂的、与算法相关的数据结构，在具体策略类中封装算法与相关的数据结构，可以提高算法的保密性与安全性。


|    <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td>**** |**练习**    Sunny软件公司欲开发一款飞机模拟系统，该系统主要模拟不同种类飞机的飞行特征与起飞特征，需要模拟的飞机种类及其特征如表24-1所示：**表24-1 飞机种类及特征一览表**                 <table border="1" cellspacing="0" cellpadding="0"><tbody><tr><td style="BACKGROUND: #f3f3f3">**飞机种类**<td style="BACKGROUND: #f3f3f3">**起飞特征**</td><td style="BACKGROUND: #f3f3f3">**飞行特征**</td>

    Sunny软件公司欲开发一款飞机模拟系统，该系统主要模拟不同种类飞机的飞行特征与起飞特征，需要模拟的飞机种类及其特征如表24-1所示：

**表24-1 飞机种类及特征一览表**

**起飞特征**
|直升机(Helicopter)|垂直起飞(VerticalTakeOff)|亚音速飞行(SubSonicFly)

垂直起飞(VerticalTakeOff)
|客机(AirPlane)|长距离起飞(LongDistanceTakeOff)|亚音速飞行(SubSonicFly)

长距离起飞(LongDistanceTakeOff)
|歼击机(Fighter)|长距离起飞(LongDistanceTakeOff)|超音速飞行(SuperSonicFly)

长距离起飞(LongDistanceTakeOff)
|鹞式战斗机(Harrier)|垂直起飞(VerticalTakeOff)|超音速飞行(SuperSonicFly)

垂直起飞(VerticalTakeOff)

      为将来能够模拟更多种类的飞机，试采用策略模式设计该飞机模拟系统。





#模板方法模式深度解析（一）
## 1. 模板方法模式概述

       在现实生活中，很多事情都包含几个实现步骤，例如请客吃饭，无论吃什么，一般都包含点单、吃东西、买单等几个步骤，通常情况下这几个步骤的次序是：点单 --&gt; 吃东西 --&gt; 买单。在这三个步骤中，点单和买单大同小异，最大的区别在于第二步——吃什么？吃面条和吃满汉全席可大不相同，如图1所示：

<img alt="" src="../../../pics///1355576016_2052.jpg" width="433" height="266">

**图1 请客吃饭示意图**

        在软件开发中，有时也会遇到类似的情况，**某个方法的实现需要多个步骤（类似“请客”），其中有些步骤是固定的（类似“点单”和“买单”），而有些步骤并不固定，存在可变性（类似“吃东西”）**。为了提高代码的复用性和系统的灵活性，可以使用一种称之为模板方法模式的设计模式来对这类情况进行设计，在模板方法模式中，将实现功能的每一个步骤所对应的方法称为基本方法（例如“点单”、“吃东西”和“买单”），而调用这些基本方法同时定义基本方法的执行次序的方法称为模板方法（例如“请客”）。在模板方法模式中，可以将相同的代码放在父类中，例如将模板方法“请客”以及基本方法“点单”和“买单”的实现放在父类中，而对于基本方法“吃东西”，在父类中只做一个声明，将其具体实现放在不同的子类中，在一个子类中提供“吃面条”的实现，而另一个子类提供“吃满汉全席”的实现。通过使用模板方法模式，一方面提高了代码的复用性，另一方面还可以利用面向对象的多态性，在运行时选择一种具体子类，实现完整的“请客”方法，提高系统的灵活性和可扩展性。

       模板方法模式定义如下：
|**模板方法模式：定义一个操作中算法的框架，而将一些步骤延迟到子类中。模板方法模式使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。****** **Template Method Pattern:  Define the skeleton of an algorithm in an  operation, deferring some steps to subclasses. Template Method lets  subclasses redefine certain steps of an algorithm without changing the  algorithm's structure.**

**** 

       模板方法模式是一种**基于继承的代码复用技术**，它是一种**类行为型模式**。

       模板方法模式是结构最简单的行为型设计模式，在其结构中只存在父类与子类之间的继承关系。通过使用模板方法模式，可以将一些复杂流程的实现步骤封装在一系列基本方法中，在抽象父类中提供一个称之为模板方法的方法来定义这些基本方法的执行次序，而通过其子类来覆盖某些步骤，从而使得相同的算法框架可以有不同的执行结果。模板方法模式提供了一个模板方法来定义算法框架，而某些具体步骤的实现可以在其子类中完成。

 

## 2. 模板方法模式结构与实现

### 2.1 模式结构

      模板方法模式结构比较简单，其核心是抽象类和其中的模板方法的设计，其结构如图2所示：

<img style="width: 391px; height: 329px;" alt="" src="../../../pics///1355576024_5664.jpg" width="425" height="372">

**图2 模板方法模式结构图**

      由图2可知，模板方法模式包含如下两个角色：

**       (1) AbstractClass（抽象类）：**在抽象类中定义了一系列基本操作(PrimitiveOperations)，这些基本操作可以是具体的，也可以是抽象的，每一个基本操作对应算法的一个步骤，在其子类中可以重定义或实现这些步骤。同时，在抽象类中实现了一个模板方法(Template Method)，用于定义一个算法的框架，模板方法不仅可以调用在抽象类中实现的基本方法，也可以调用在抽象类的子类中实现的基本方法，还可以调用其他对象中的方法。

**       (2) ConcreteClass（具体子类）：**它是抽象类的子类，用于实现在父类中声明的抽象基本操作以完成子类特定算法的步骤，也可以覆盖在父类中已经实现的具体基本操作。

 

### 2.2 模式实现

       在实现模板方法模式时，开发抽象类的软件设计师和开发具体子类的软件设计师之间可以进行协作。一个设计师负责给出一个算法的轮廓和框架，另一些设计师则负责给出这个算法的各个逻辑步骤。实现这些具体逻辑步骤的方法即为基本方法，而将这些基本方法汇总起来的方法即为模板方法，模板方法模式的名字也因此而来。下面将详细介绍模板方法和基本方法：

**       1. 模板方法**

       一个模板方法是定义在抽象类中的、把基本操作方法组合在一起形成一个总算法或一个总行为的方法。这个模板方法定义在抽象类中，并由子类不加以修改地完全继承下来。模板方法是一个具体方法，它给出了一个顶层逻辑框架，而逻辑的组成步骤在抽象类中可以是具体方法，也可以是抽象方法。由于模板方法是具体方法，因此模板方法模式中的抽象层只能是抽象类，而不是接口。

 **        2. 基本方法**

       基本方法是实现算法各个步骤的方法，是模板方法的组成部分。基本方法又可以分为三种：抽象方法(Abstract Method)、具体方法(Concrete Method)和钩子方法(Hook Method)。

      ** (1) ****抽象方法**：一个抽象方法由抽象类声明、由其具体子类实现。在C#语言里一个抽象方法以abstract关键字标识。

       **(2) ****具体方法**：一个具体方法由一个抽象类或具体类声明并实现，其子类可以进行覆盖也可以直接继承。

       **(3) ****钩子方法**：一个钩子方法由一个抽象类或具体类声明并实现，而其子类可能会加以扩展。通常在父类中给出的实现是一个空实现（可使用virtual关键字将其定义为虚函数），并以该空实现作为方法的默认实现，当然钩子方法也可以提供一个非空的默认实现。

       在模板方法模式中，钩子方法有两类：**第一类钩子方法可以与一些具体步骤“挂钩”**，以实现在不同条件下执行模板方法中的不同步骤，这类钩子方法的返回类型通常是bool类型的，这类方法名一般为IsXXX()，用于对某个条件进行判断，如果条件满足则执行某一步骤，否则将不执行，如下代码片段所示：

```
……
//模板方法
public void TemplateMethod() 
{
Open();
Display();
//通过钩子方法来确定某步骤是否执行
if (IsPrint()) 
{
    Print();
}
}

//钩子方法
public bool IsPrint()
{
    return true;
}
……
```

        在代码中IsPrint()方法即是钩子方法，它可以决定Print()方法是否执行，一般情况下，钩子方法的返回值为true，如果不希望某方法执行，可以在其子类中覆盖钩子方法，将其返回值改为false即可，这种类型的钩子方法可以控制方法的执行，对一个算法进行约束。

       还有一类钩子方法就是**实现体为空的具体方法**，子类可以根据需要覆盖或者继承这些钩子方法，与抽象方法相比，这类钩子方法的好处在于子类如果没有覆盖父类中定义的钩子方法，编译可以正常通过，但是如果没有覆盖父类中声明的抽象方法，编译将报错。

       在模板方法模式中，抽象类的典型代码如下：

```
abstract class AbstractClass 
{
//模板方法
public void TemplateMethod() 
{
        PrimitiveOperation1();
        PrimitiveOperation2();
        PrimitiveOperation3();
}

//基本方法—具体方法
public void PrimitiveOperation1() 
{
    //实现代码
}

//基本方法—抽象方法
    public abstract void PrimitiveOperation2();    

//基本方法—钩子方法
public virtual void PrimitiveOperation3()   
{  }
}
```

        在抽象类中，模板方法TemplateMethod()定义了算法的框架，在模板方法中调用基本方法以实现完整的算法，每一个基本方法如PrimitiveOperation1()、PrimitiveOperation2()等均实现了算法的一部分，对于所有子类都相同的基本方法可在父类提供具体实现，例如PrimitiveOperation1()，否则在父类声明为抽象方法或钩子方法，由不同的子类提供不同的实现，例如PrimitiveOperation2()和PrimitiveOperation3()。

        可在抽象类的子类中提供抽象步骤的实现，也可覆盖父类中已经实现的具体方法，具体子类的典型代码如下：

```
class ConcreteClass : AbstractClass 
{
public override void PrimitiveOperation2() 
{
    //实现代码
}

public override void PrimitiveOperation3() 
{
    //实现代码
}
}
```

       **在模板方法模式中，由于面向对象的多态性，子类对象在运行时将覆盖父类对象，子类中定义的方法也将覆盖父类中定义的方法，因此程序在运行时，具体子类的基本方法将覆盖父类中定义的基本方法，子类的钩子方法也将覆盖父类的钩子方法，从而可以通过在子类中实现的钩子方法对父类方法的执行进行约束，实现子类对父类行为的反向控制。**

  【作者：刘伟   】
#模板方法模式深度解析（二）
## 3 模板方法模式应用实例

      下面通过一个应用实例来进一步学习和理解模板方法模式。

 

**      1. 实例说明**
|      某软件公司欲为某银行的业务支撑系统开发一个利息计算模块，利息计算流程如下：      (1) 系统根据账号和密码验证用户信息，如果用户信息错误，系统显示出错提示；      (2) 如果用户信息正确，则根据用户类型的不同使用不同的利息计算公式计算利息（如活期账户和定期账户具有不同的利息计算公式）；      (3) 系统显示利息。      试使用模板方法模式设计该利息计算模块。

      (1) 系统根据账号和密码验证用户信息，如果用户信息错误，系统显示出错提示；

      (3) 系统显示利息。

**     **

**       2. 实例类图**

      通过分析，本实例结构图如图3所示。

<img style="width: 402px; height: 326px;" alt="" src="../../../pics///1355577976_2992.jpg" width="404" height="373">

**图3 银行利息计算模块结构图**

       在图3中，Account充当抽象类角色，CurrentAccount和SavingAccount充当具体子类角色。

 

**       3. 实例代码**

       (1) Account：账户类，充当抽象类。

```
//Account.cs
using System;

namespace TemplateMethodSample
{
    abstract class Account
    {
        //基本方法——具体方法
        public bool Validate(string account, string password) 
        {
		    Console.WriteLine("账号：{0}", account);
            Console.WriteLine("密码：{0}", password);
            //模拟登录
            if (account.Equals("张无忌") &amp;&amp; password.Equals("123456")) 
            {
			    return true;
		    }
		    else 
            {
			    return false;
		    }
	    }

        //基本方法——抽象方法
        public abstract void CalculateInterest();

        //基本方法——具体方法
        public void Display() 
        {
            Console.WriteLine("显示利息！");
	    }

        //模板方法
        public void Handle(string account, string password) 
        {
		    if (!Validate(account,password)) 
            {
                Console.WriteLine("账户或密码错误！");
			    return;
		    }
		    CalculateInterest();
		    Display();
	    }
    }
}
```

       (2) CurrentAccount：活期账户类，充当具体子类。

```
//CurrentAccount.cs
using System;

namespace TemplateMethodSample
{
    class CurrentAccount : Account
    {
        //覆盖父类的抽象基本方法
        public override void CalculateInterest() 
        {
		    Console.WriteLine("按活期利率计算利息！");
	    }
    }
}
```

       (3) SavingAccount：定期账户类，充当具体子类。

```
//SavingAccount.cs
using System;

namespace TemplateMethodSample
{
    class SavingAccount : Account
    {
        //覆盖父类的抽象基本方法
        public override void CalculateInterest() 
        {
		    Console.WriteLine("按定期利率计算利息！");
	    }
    }
}
```

       (4) 配置文件App.config，在配置文件中存储了具体子类的类名。

```
&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;configuration&gt;
  &lt;appSettings&gt;
    &lt;add key="subClass" value="TemplateMethodSample.CurrentAccount"/&gt;
  &lt;/appSettings&gt;
&lt;/configuration&gt;
```

     (5) Program：客户端测试类

```
//Program.cs
using System;
using System.Configuration;
using System.Reflection;

namespace TemplateMethodSample
{
    class Program
    {
        static void Main(string[] args)
        {
            Account account;
            //读取配置文件
            string subClassStr = ConfigurationManager.AppSettings["subClass"];
            //反射生成对象
            account = (Account)Assembly.Load("TemplateMethodSample").CreateInstance(subClassStr);
            account.Handle("张无忌", "123456");
            Console.Read();
        }
    }
}
```

**       4. 结果及分析**

      编译并运行程序，输出结果如下：
|账号：张无忌密码：123456**按活期利率计算利息！**显示利息！

密码：123456

显示利息！

      如果需要更换具体子类，无须修改源代码，只需修改配置文件App.config，例如将活期账户(CurrentAccount)改为定期账户(Saving Account)，只需将存储在配置文件中的具体子类CurrentAccount改为SavingAccount，如下代码所示：

```
&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;configuration&gt;
  &lt;appSettings&gt;
    &lt;add key="subClass" value="TemplateMethodSample.SavingAccount"/&gt;
  &lt;/appSettings&gt;
&lt;/configuration&gt;
```

       重新运行客户端程序，输出结果如下：
|账号：张无忌密码：123456**按定期利率计算利息！**显示利息！

密码：123456

显示利息！

       如果需要增加新的具体子类（新的账户类型），原有代码均无须修改，**完全符合开闭原则**。

 【作者：刘伟   】
#模板方法模式深度解析（三）
## 4 钩子方法的使用

      

       模板方法模式中，在父类中提供了一个定义算法框架的模板方法，还提供了一系列抽象方法、具体方法和钩子方法，其中**钩子方法的引入使得子类可以控制父类的行为**。最简单的钩子方法就是空方法，代码如下：

```
public virtual void Display() {   }
```

      当然也可以在钩子方法中定义一个默认的实现，如果子类不覆盖钩子方法，则执行父类的默认实现代码。

      另一种钩子方法可以实现对其他方法进行约束，这种钩子方法通常返回一个bool类型，即返回true或false，用来判断是否执行某一个基本方法，下面通过一个实例来说明这种钩子方法的使用。
|      某软件公司欲为销售管理系统提供一个数据图表显示功能，该功能的实现包括如下几个步骤：      (1) 从数据源获取数据；      (2) 将数据转换为XML格式；      (3) 以某种图表方式显示XML格式的数据。      该功能支持多种数据源和多种图表显示方式，但所有的图表显示操作都基于XML格式的数据，因此可能需要对数据进行转换，如果从数据源获取的数据已经是XML数据则无须转换。

      (1) 从数据源获取数据；

      (3) 以某种图表方式显示XML格式的数据。

       由于该数据图表显示功能的三个步骤次序是固定的，且存在公共代码（例如数据格式转换代码），满足模板方法模式的适用条件，可以使用模板方法模式对其进行设计。因为数据格式的不同，XML数据可以直接显示，而其他格式的数据需要进行转换，因此第(2)步“将数据转换为XML格式”的执行存在不确定性，为了解决这个问题，可以定义一个钩子方法IsNotXMLData()来对数据转换方法进行控制。通过分析，该图表显示功能的基本结构如图4所示：

<img style="width: 643px; height: 191px;" alt="" src="../../../pics///1355579556_7922.jpg" width="646" height="164">

**图4  数据图表显示功能结构图**

       可以将公共方法和框架代码放在抽象父类中，代码如下：

```
//DataViewer.cs
using System;

namespace TemplateMethodSample
{
    abstract class DataViewer
    {
        //抽象方法：获取数据
        public abstract void GetData();

        //具体方法：转换数据
        public void ConvertData() 
        {
		    Console.WriteLine("将数据转换为XML格式。");
	    }

        //抽象方法：显示数据
        public abstract void DisplayData();

        //钩子方法：判断是否为XML格式的数据
        public virtual bool IsNotXMLData()
        {
            return true;
        }

        //模板方法
        public void Process()
        {
            GetData();
            //如果不是XML格式的数据则进行数据转换
            if (IsNotXMLData())
            {
                ConvertData();
            }
            DisplayData();
        }
    }
}
```

       在上面的代码中，引入了一个钩子方法IsNotXMLData()，其返回类型为bool类型，在模板方法中通过它来对数据转换方法ConvertData()进行约束，该钩子方法的默认返回值为true，在子类中可以根据实际情况覆盖该方法，其中用于显示XML格式数据的具体子类XMLDataViewer代码如下：

```
//XMLDataViewer.cs
using System;

namespace TemplateMethodSample
{
    class XMLDataViewer : DataViewer
    {
        //实现父类方法：获取数据
        public override void GetData() 
        {
		    Console.WriteLine("从XML文件中获取数据。");
	    }

        //实现父类方法：显示数据，默认以柱状图方式显示，可结合桥接模式来改进
        public override void DisplayData() 
        {
            Console.WriteLine("以柱状图显示数据。");
	    }

        //覆盖父类的钩子方法
        public override bool IsNotXMLData()
        {
            return false;
        }
    }
}
```

       在具体子类XMLDataViewer中覆盖了钩子方法IsNotXMLData()，返回false，表示该数据已为XML格式，无须执行数据转换方法ConvertData()，客户端代码如下：

```
//Program.cs
using System;

namespace TemplateMethodSample
{
    class Program
    {
        static void Main(string[] args)
        {
            DataViewer dv;
            dv = new XMLDataViewer();
            dv.Process();
            Console.Read();
        }
    }
}
```

       该程序运行结果如下：
|从XML文件中获取数据。以柱状图显示数据。

以柱状图显示数据。

##  

## 5 模板方法模式效果与适用场景

       **模板方法模式是基于继承的代码复用技术**，它体现了面向对象的诸多重要思想，是一种使用较为频繁的模式。模板方法模式广泛应用于框架设计中，以确保通过父类来控制处理流程的逻辑顺序（如框架的初始化，测试流程的设置等）。

 

###        5.1 模式优点

       模板方法模式的主要优点如下：

       (1) 在父类中形式化地定义一个算法，而由它的子类来实现细节的处理，在子类实现详细的处理算法时并不会改变算法中步骤的执行次序。

       (2) 模板方法模式是一种代码复用技术，它在类库设计中尤为重要，它提取了类库中的公共行为，将公共行为放在父类中，而通过其子类来实现不同的行为，它鼓励我们恰当使用继承来实现代码复用。

       (3) 可实现一种反向控制结构，通过子类覆盖父类的钩子方法来决定某一特定步骤是否需要执行。

       (4) 在模板方法模式中可以通过子类来覆盖父类的基本方法，不同的子类可以提供基本方法的不同实现，更换和增加新的子类很方便，符合单一职责原则和开闭原则。

 

###        5.2 模式缺点

       模板方法模式的主要缺点如下：

       需要为每一个基本方法的不同实现提供一个子类，如果父类中可变的基本方法太多，将会导致类的个数增加，系统更加庞大，设计也更加抽象，此时，可结合**桥接模式**来进行设计。

 

###        5.3 模式适用场景

       在以下情况下可以考虑使用模板方法模式：

       (1) 对一些复杂的算法进行分割，将其算法中固定不变的部分设计为模板方法和父类具体方法，而一些可以改变的细节由其子类来实现。即：一次性实现一个算法的不变部分，并将可变的行为留给子类来实现。

       (2) 各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复。

       (3) 需要通过子类来决定父类算法中某个步骤是否执行，实现子类对父类的反向控制。

 【作者：刘伟   】 


#操作复杂对象结构——访问者模式（一）
        想必大家都去过医院，虽然没有人喜欢去医院（爱岗敬业的医务工作人员除外，<img alt="微笑" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/smile.gif">）。在医生开具处方单（药单）后，很多医院都存在如下处理流程：划价人员拿到处方单之后根据药品名称和数量计算总价，药房工作人员根据药品名称和数量准备药品，如图26-1所示：

<img style="WIDTH: 480px; HEIGHT: 414px" alt="" src="../../../pics///1333713447_7456.gif" width="484" height="494">

      在软件开发中，有时候我们也需要处理像处方单这样的集合对象结构，在该对象结构中存储了多个不同类型的对象信息，而且对同一对象结构中的元素的操作方式并不唯一，可能需要提供多种不同的处理方式，还有可能增加新的处理方式。在设计模式中，有一种模式可以满足上述要求，其模式动机就是以不同的方式操作复杂对象结构，该模式就是我们本章将要介绍的访问者模式。

 

## 26.1 OA系统中员工数据汇总
|       Sunny软件公司欲为某银行开发一套OA系统，在该OA系统中包含一个员工信息管理子系统，该银行员工包括正式员工和临时工，每周人力资源部和财务部等部门需要对员工数据进行汇总，汇总数据包括员工工作时间、员工工资等。该公司基本制度如下：       (1) 正式员工(Full time  Employee)每周工作时间为40小时，不同级别、不同部门的员工每周基本工资不同；如果超过40小时，超出部分按照100元/小时作为加班费；如果少于40小时，所缺时间按照请假处理，请假所扣工资以80元/小时计算，直到基本工资扣除到零为止。除了记录实际工作时间外，人力资源部需记录加班时长或请假时长，作为员工平时表现的一项依据。       (2) 临时工(Part time  Employee)每周工作时间不固定，基本工资按小时计算，不同岗位的临时工小时工资不同。人力资源部只需记录实际工作时间。       人力资源部和财务部工作人员可以根据各自的需要对员工数据进行汇总处理，人力资源部负责汇总每周员工工作时间，而财务部负责计算每周员工工资。

       (1) 正式员工(Full time  Employee)每周工作时间为40小时，不同级别、不同部门的员工每周基本工资不同；如果超过40小时，超出部分按照100元/小时作为加班费；如果少于40小时，所缺时间按照请假处理，请假所扣工资以80元/小时计算，直到基本工资扣除到零为止。除了记录实际工作时间外，人力资源部需记录加班时长或请假时长，作为员工平时表现的一项依据。

       人力资源部和财务部工作人员可以根据各自的需要对员工数据进行汇总处理，人力资源部负责汇总每周员工工作时间，而财务部负责计算每周员工工资。

       Sunny软件公司开发人员针对上述需求，提出了一个初始解决方案，其核心代码如下所示：

```
import java.util.*;



class EmployeeList

{

	private ArrayList&lt;Employee&gt; list = new ArrayList&lt;Employee&gt;(); //员工集合



    //增加员工

	public void addEmployee(Employee employee) 

	{

		list.add(employee);

	}

    

    //处理员工数据

	public void handle(String departmentName)

	{

		if(departmentName.equalsIgnoreCase("财务部")) //财务部处理员工数据

		{

			for(Object obj : list)

			{

				if(obj.getClass().getName().equalsIgnoreCase("FulltimeEmployee"))

				{

					System.out.println("财务部处理全职员工数据！");			

				}

				else 

				{

					System.out.println("财务部处理兼职员工数据！");

				}

			}

		}

		else if(departmentName.equalsIgnoreCase("人力资源部")) //人力资源部处理员工数据

		{

			for(Object obj : list)

			{

				if(obj.getClass().getName().equalsIgnoreCase("FulltimeEmployee"))

				{

					System.out.println("人力资源部处理全职员工数据！");					

				}

				else 

				{

					System.out.println("人力资源部处理兼职员工数据！");

				}

			}			

		}

	}

}
```

      在EmployeeList类的handle()方法中，通过对部门名称和员工类型进行判断，不同部门对不同类型的员工进行了不同的处理，满足了员工数据汇总的要求。但是该解决方案存在如下几个问题：

      (1) EmployeeList类非常庞大，它将各个部门处理各类员工数据的代码集中在一个类中，在具体实现时，代码将相当冗长，EmployeeList类承担了过多的职责，既不方便代码的复用，也不利于系统的扩展，违背了“单一职责原则”。

      (2)在代码中包含大量的“if…else…”条件判断语句，既需要对不同部门进行判断，又需要对不同类型的员工进行判断，还将出现嵌套的条件判断语句，导致测试和维护难度增大。

      (3)如果要增加一个新的部门来操作员工集合，不得不修改EmployeeList类的源代码，在handle()方法中增加一个新的条件判断语句和一些业务处理代码来实现新部门的访问操作。这违背了“开闭原则”，系统的灵活性和可扩展性有待提高。

      (4)如果要增加一种新类型的员工，同样需要修改EmployeeList类的源代码，在不同部门的处理代码中增加对新类型员工的处理逻辑，这也违背了“开闭原则”。

      如何解决上述问题？如何为同一集合对象中的元素提供多种不同的操作方式？访问者模式就是一个值得考虑的解决方案，它可以**在一定程度上解决上述问题（解决大部分问题）**。访问者模式可以为为不同类型的元素提供多种访问操作方式，而且可以在不修改原有系统的情况下增加新的操作方式。

【作者：刘伟 】

#操作复杂对象结构——访问者模式（二）
## 26.2 访问者模式概述

      访问者模式是一种较为复杂的行为型设计模式，它包含访问者和被访问元素两个主要组成部分，这些被访问的元素通常具有不同的类型，且不同的访问者可以对它们进行不同的访问操作。例如处方单中的各种药品信息就是被访问的元素，而划价人员和药房工作人员就是访问者。访问者模式使得用户可以在不修改现有系统的情况下扩展系统的功能，为这些不同类型的元素增加新的操作。

      在使用访问者模式时，被访问元素通常不是单独存在的，它们存储在一个集合中，这个集合被称为“对象结构”，访问者通过遍历对象结构实现对其中存储的元素的逐个操作。

      访问者模式定义如下：
| **访问者模式(Visitor Pattern):提供一个作用于某对象结构中的各元素的操作表示，它使我们可以在不改变各元素的类的前提下定义作用于这些元素的新操作。访问者模式是一种对象行为型模式。** 

      访问者模式的结构较为复杂，其结构如图26-2所示：

<img src="../../../pics///1333713874_7112.gif" width="584" height="576" alt="" style="width:582px; height:521px">







      在访问者模式结构图中包含如下几个角色：

      ●Vistor（抽象访问者）：抽象访问者为对象结构中每一个具体元素类ConcreteElement声明一个访问操作，从这个操作的名称或参数类型可以清楚知道需要访问的具体元素的类型，具体访问者需要实现这些操作方法，定义对这些元素的访问操作。

      ●ConcreteVisitor（具体访问者）：具体访问者实现了每个由抽象访问者声明的操作，每一个操作用于访问对象结构中一种类型的元素。

      ●Element（抽象元素）：抽象元素一般是抽象类或者接口，它定义一个accept()方法，该方法通常以一个抽象访问者作为参数。【稍后将介绍为什么要这样设计。】

      ●ConcreteElement（具体元素）：具体元素实现了accept()方法，在accept()方法中调用访问者的访问方法以便完成对一个元素的操作。

      ● ObjectStructure（对象结构）：对象结构是一个元素的集合，它用于存放元素对象，并且提供了遍历其内部元素的方法。它可以结合组合模式来实现，也可以是一个简单的集合对象，如一个List对象或一个Set对象。

      访问者模式中对象结构存储了不同类型的元素对象，以供不同访问者访问。访问者模式包括两个层次结构，一个是访问者层次结构，提供了抽象访问者和具体访问者，一个是元素层次结构，提供了抽象元素和具体元素。相同的访问者可以以不同的方式访问不同的元素，相同的元素可以接受不同访问者以不同访问方式访问。在访问者模式中，增加新的访问者无须修改原有系统，系统具有较好的可扩展性。

      在访问者模式中，抽象访问者定义了访问元素对象的方法，通常为每一种类型的元素对象都提供一个访问方法，而具体访问者可以实现这些访问方法。这些访问方法的命名一般有两种方式：一种是直接在方法名中标明待访问元素对象的具体类型，如visitElementA(ElementA elementA)，还有一种是统一取名为visit()，通过参数类型的不同来定义一系列重载的visit()方法。当然，如果所有的访问者对某一类型的元素的访问操作都相同，则可以将操作代码移到抽象访问者类中，其典型代码如下所示：

```
abstract class Visitor
{
	public abstract void visit(ConcreteElementA elementA);
	public abstract void visit(ConcreteElementB elementB);
	public void visit(ConcreteElementC elementC)
	{
		//元素ConcreteElementC操作代码
	}
}
```

      在这里使用了重载visit()方法的方式来定义多个方法用于操作不同类型的元素对象。在抽象访问者Visitor类的子类ConcreteVisitor中实现了抽象的访问方法，用于定义对不同类型元素对象的操作，具体访问者类典型代码如下所示：

```
class ConcreteVisitor extends Visitor
{
	public void visit(ConcreteElementA elementA)
	{
		//元素ConcreteElementA操作代码
	}
	public void visit(ConcreteElementB elementB)
	{
		//元素ConcreteElementB操作代码
	}
}
```

      对于元素类而言，在其中一般都定义了一个accept()方法，用于接受访问者的访问，典型的抽象元素类代码如下所示：

```
interface Element
{
	public void accept(Visitor visitor);
}
```

       需要注意的是该方法传入了一个抽象访问者Visitor类型的参数，即**针对抽象访问者进行编程，而不是具体访问者**，在程序运行时再确定具体访问者的类型，并调用具体访问者对象的visit()方法实现对元素对象的操作。在抽象元素类Element的子类中实现了accept()方法，用于接受访问者的访问，在具体元素类中还可以定义不同类型的元素所特有的业务方法，其典型代码如下所示：

```
class ConcreteElementA implements Element
{
	public void accept(Visitor visitor)
	{
		visitor.visit(this);
	}
	
	public void operationA()
	{
		//业务方法
	}
}
```

       在具体元素类ConcreteElementA的accept()方法中，通过调用Visitor类的visit()方法实现对元素的访问，并以当前对象作为visit()方法的参数。其具体执行过程如下：

      (1) 调用具体元素类的accept(Visitor visitor)方法，并**将Visitor子类对象作为其参数**；

      (2) 在具体元素类accept(Visitor visitor)方法内部调用传入的Visitor对象的visit()方法，如visit(ConcreteElementA elementA)，**将当前具体元素类对象(this)作为参数**，如visitor.visit(this)；

      (3) 执行Visitor对象的visit()方法，在其中还可以调用具体元素对象的业务方法。

      这种调用机制也称为“**双重分派**”，正因为使用了双重分派机制，使得增加新的访问者无须修改现有类库代码，只需将新的访问者对象作为参数传入具体元素对象的accept()方法，程序运行时将回调在新增Visitor类中定义的visit()方法，从而增加新的元素访问方式。 
|     <table class=" " border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td> **** | **思考** 双重分派机制如何用代码实现？ 

双重分派机制如何用代码实现？



       在访问者模式中，对象结构是一个集合，它用于存储元素对象并接受访问者的访问，其典型代码如下所示：

```
class ObjectStructure
{
	private ArrayList&lt;Element&gt; list = new ArrayList&lt;Element&gt;(); //定义一个集合用于存储元素对象

	public void accept(Visitor visitor)
	{
		Iterator i=list.iterator();
		
		while(i.hasNext())
		{
			((Element)i.next()).accept(visitor); //遍历访问集合中的每一个元素
		}
	}

	public void addElement(Element element)
	{
		list.add(element);
	}

	public void removeElement(Element element)
	{
		list.remove(element);
	}
}
```

       在对象结构中可以使用迭代器对存储在集合中的元素对象进行遍历，并逐个调用每一个对象的accept()方法，实现对元素对象的访问操作。
|     <table class=" " border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td> **** | **思考** 访问者模式是否符合“开闭原则”？【从增加新的访问者和增加新的元素两方面考虑。】 

访问者模式是否符合“开闭原则”？【从增加新的访问者和增加新的元素两方面考虑。】



【作者：刘伟 】

#操作复杂对象结构——访问者模式（三）
## 26.3 完整解决方案

      Sunny软件公司开发人员使用访问者模式对OA系统中员工数据汇总模块进行重构，使得系统可以很方便地增加新类型的访问者，更加符合“单一职责原则”和“开闭原则”，重构后的基本结构如图26-3所示：

<img alt="" src="../../../pics///1333714391_6314.gif" width="652" height="576" style="width:649px; height:559px">

```
import java.util.*;

//员工类：抽象元素类
interface Employee
{
	public void accept(Department handler); //接受一个抽象访问者访问
}

//全职员工类：具体元素类
class FulltimeEmployee implements Employee
{
	private String name;
	private double weeklyWage;
	private int workTime;

	public FulltimeEmployee(String name,double weeklyWage,int workTime)
	{
		this.name = name;
		this.weeklyWage = weeklyWage;
		this.workTime = workTime;
	}	

	public void setName(String name) 
    {
		this.name = name; 
	}

	public void setWeeklyWage(double weeklyWage) 
    {
		this.weeklyWage = weeklyWage; 
	}

	public void setWorkTime(int workTime) 
    {
		this.workTime = workTime; 
	}

	public String getName() 
    {
		return (this.name); 
	}

	public double getWeeklyWage() 
    {
		return (this.weeklyWage); 
	}

	public int getWorkTime() 
    {
		return (this.workTime); 
	}

	public void accept(Department handler)
    {
		handler.visit(this); //调用访问者的访问方法
	}
}

//兼职员工类：具体元素类
class ParttimeEmployee implements Employee
{
	private String name;
	private double hourWage;
	private int workTime;

	public ParttimeEmployee(String name,double hourWage,int workTime)
	{
		this.name = name;
		this.hourWage = hourWage;
		this.workTime = workTime;
	}	

	public void setName(String name) 
    {
		this.name = name; 
	}

	public void setHourWage(double hourWage) 
    {
		this.hourWage = hourWage; 
	}

	public void setWorkTime(int workTime) 
    {
		this.workTime = workTime; 
	}

	public String getName() 
    {
		return (this.name); 
	}

	public double getHourWage() 
    {
		return (this.hourWage); 
	}

	public int getWorkTime() 
    {
		return (this.workTime); 
	}

	public void accept(Department handler)
    {
		handler.visit(this); //调用访问者的访问方法
	}
}

//部门类：抽象访问者类
abstract class Department
{
    //声明一组重载的访问方法，用于访问不同类型的具体元素
	public abstract void visit(FulltimeEmployee employee);
	public abstract void visit(ParttimeEmployee employee);	
}

//财务部类：具体访问者类
class FADepartment extends Department
{
    //实现财务部对全职员工的访问
	public void visit(FulltimeEmployee employee)
	{
		int workTime = employee.getWorkTime();
		double weekWage = employee.getWeeklyWage();
		if(workTime &gt; 40)
		{
			weekWage = weekWage + (workTime - 40) * 100;
		}
		else if(workTime &lt; 40)
		{
			weekWage = weekWage - (40 - workTime) * 80;
			if(weekWage &lt; 0)
			{
				weekWage = 0;
			}
		}
		System.out.println("正式员工" + employee.getName() + "实际工资为：" + weekWage + "元。");			
	}

    //实现财务部对兼职员工的访问
	public void visit(ParttimeEmployee employee)
	{
		int workTime = employee.getWorkTime();
		double hourWage = employee.getHourWage();
		System.out.println("临时工" + employee.getName() + "实际工资为：" + workTime * hourWage + "元。");		
	}		
}

//人力资源部类：具体访问者类
class HRDepartment extends Department
{
    //实现人力资源部对全职员工的访问
	public void visit(FulltimeEmployee employee)
	{
		int workTime = employee.getWorkTime();
		System.out.println("正式员工" + employee.getName() + "实际工作时间为：" + workTime + "小时。");
		if(workTime &gt; 40)
		{
			System.out.println("正式员工" + employee.getName() + "加班时间为：" + (workTime - 40) + "小时。");
		}
		else if(workTime &lt; 40)
		{
			System.out.println("正式员工" + employee.getName() + "请假时间为：" + (40 - workTime) + "小时。");
		}						
	}

    //实现人力资源部对兼职员工的访问
	public void visit(ParttimeEmployee employee)
	{
		int workTime = employee.getWorkTime();
		System.out.println("临时工" + employee.getName() + "实际工作时间为：" + workTime + "小时。");
	}		
}

//员工列表类：对象结构
class EmployeeList
{
    //定义一个集合用于存储员工对象
	private ArrayList&lt;Employee&gt; list = new ArrayList&lt;Employee&gt;();

	public void addEmployee(Employee employee)
	{
		list.add(employee);
	}

    //遍历访问员工集合中的每一个员工对象
	public void accept(Department handler)
	{
		for(Object obj : list)
		{
			((Employee)obj).accept(handler);
		}
	}
}
```

      为了提高系统的灵活性和可扩展性，我们将具体访问者类的类名存储在配置文件中，并通过工具类XMLUtil来读取配置文件并反射生成对象，XMLUtil类的代码如下所示：

```
import javax.xml.parsers.*;
import org.w3c.dom.*;
import org.xml.sax.SAXException;
import java.io.*;
class XMLUtil
{
    //该方法用于从XML配置文件中提取具体类类名，并返回一个实例对象
    public static Object getBean()
    {
		try
		{
			//创建文档对象
			DocumentBuilderFactory dFactory = DocumentBuilderFactory.newInstance();
			DocumentBuilder builder = dFactory.newDocumentBuilder();
			Document doc;							
			doc = builder.parse(new File("config.xml")); 
		
			//获取包含类名的文本节点
			NodeList nl = doc.getElementsByTagName("className");
            Node classNode=nl.item(0).getFirstChild();
            String cName=classNode.getNodeValue();
            
            //通过类名生成实例对象并将其返回
            Class c=Class.forName(cName);
	  	    Object obj=c.newInstance();
            return obj;
        }   
        catch(Exception e)
        {
           	e.printStackTrace();
           	return null;
       	}
    }
}
```

       配置文件config.xml中存储了具体访问者类的类名，代码如下所示：

```
&lt;?xml version="1.0"?&gt;
&lt;config&gt;
	&lt;className&gt;FADepartment&lt;/className&gt;
&lt;/config&gt;
```

     编写如下客户端测试代码：

```
class Client
{
	public static void main(String args[])
	{
		EmployeeList list = new EmployeeList();
		Employee fte1,fte2,fte3,pte1,pte2;

		fte1 = new FulltimeEmployee("张无忌",3200.00,45);
		fte2 = new FulltimeEmployee("杨过",2000.00,40);
		fte3 = new FulltimeEmployee("段誉",2400.00,38);
		pte1 = new ParttimeEmployee("洪七公",80.00,20);
		pte2 = new ParttimeEmployee("郭靖",60.00,18);

		list.addEmployee(fte1);
		list.addEmployee(fte2);
		list.addEmployee(fte3);
		list.addEmployee(pte1);
		list.addEmployee(pte2);

		Department dep;
		dep = (Department)XMLUtil.getBean();
		list.accept(dep);
	}
}
```

      编译并运行程序，输出结果如下：
| 正式员工张无忌实际工资为：3700.0元。 正式员工杨过实际工资为：2000.0元。 正式员工段誉实际工资为：2240.0元。 临时工洪七公实际工资为：1600.0元。 临时工郭靖实际工资为：1080.0元。 

正式员工杨过实际工资为：2000.0元。

临时工洪七公实际工资为：1600.0元。

      如果需要更换具体访问者类，无须修改源代码，只需修改配置文件，例如将访问者类由财务部改为人力资源部，只需将存储在配置文件中的具体访问者类FADepartment改为HRDepartment，如下代码所示：

```
&lt;?xml version="1.0"?&gt;
&lt;config&gt;
    &lt;className&gt;HRDepartment&lt;/className&gt;
&lt;/config&gt;
```

      重新运行客户端程序，输出结果如下：
| 正式员工张无忌实际工作时间为：45小时。 正式员工张无忌加班时间为：5小时。 正式员工杨过实际工作时间为：40小时。 正式员工段誉实际工作时间为：38小时。 正式员工段誉请假时间为：2小时。 临时工洪七公实际工作时间为：20小时。 临时工郭靖实际工作时间为：18小时。 

正式员工张无忌加班时间为：5小时。

正式员工段誉实际工作时间为：38小时。

临时工洪七公实际工作时间为：20小时。

      如果要在系统中增加一种新的访问者，无须修改源代码，只要增加一个新的具体访问者类即可，在该具体访问者中封装了新的操作元素对象的方法。从增加新的访问者的角度来看，访问者模式符合“开闭原则”。

      如果要在系统中增加一种新的具体元素，例如增加一种新的员工类型为“退休人员”，由于原有系统并未提供相应的访问接口（在抽象访问者中没有声明任何访问“退休人员”的方法），因此必须对原有系统进行修改，在原有的抽象访问者类和具体访问者类中增加相应的访问方法。从增加新的元素的角度来看，访问者模式违背了“开闭原则”。

      综上所述，访问者模式与抽象工厂模式类似，对“开闭原则”的支持具有倾斜性，可以很方便地添加新的访问者，但是添加新的元素较为麻烦。

【作者：刘伟 】 

#操作复杂对象结构——访问者模式（四）
## 26.4 访问者模式与组合模式联用

      在访问者模式中，包含一个用于存储元素对象集合的对象结构，我们通常可以使用迭代器来遍历对象结构，同时具体元素之间可以存在整体与部分关系，有些元素作为容器对象，有些元素作为成员对象，可以使用组合模式来组织元素。引入组合模式后的访问者模式结构图如图26-4所示： 

<img alt="" src="../../../pics///1333715011_8778.gif" width="546" height="532" style="width:546px; height:486px">

## 26.5 访问者模式总结

      由于访问者模式的使用条件较为苛刻，本身结构也较为复杂，因此在实际应用中使用频率不是特别高。当系统中存在一个较为复杂的对象结构，且不同访问者对其所采取的操作也不相同时，可以考虑使用访问者模式进行设计。在XML文档解析、编译器的设计、复杂集合对象的处理等领域访问者模式得到了一定的应用。

**1.主要优点**

      访问者模式的主要优点如下：

(1)  增加新的访问操作很方便。使用访问者模式，增加新的访问操作就意味着增加一个新的具体访问者类，实现简单，无须修改源代码，符合“开闭原则”。

(2)  将有关元素对象的访问行为集中到一个访问者对象中，而不是分散在一个个的元素类中。类的职责更加清晰，有利于对象结构中元素对象的复用，相同的对象结构可以供多个不同的访问者访问。

(3)  让用户能够在不修改现有元素类层次结构的情况下，定义作用于该层次结构的操作。

**2.主要缺点**

      访问者模式的主要缺点如下：

(1)  增加新的元素类很困难。在访问者模式中，每增加一个新的元素类都意味着要在抽象访问者角色中增加一个新的抽象操作，并在每一个具体访问者类中增加相应的具体操作，这违背了“开闭原则”的要求。

(2)  破坏封装。访问者模式要求访问者对象访问并调用每一个元素对象的操作，这意味着元素对象有时候必须暴露一些自己的内部操作和内部状态，否则无法供访问者访问。

**3.适用场景**

      在以下情况下可以考虑使用访问者模式：

(1)  一个对象结构包含多个类型的对象，希望对这些对象实施一些依赖其具体类型的操作。在访问者中针对每一种具体的类型都提供了一个访问操作，不同类型的对象可以有不同的访问操作。

(2)  需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而需要避免让这些操作“污染”这些对象的类，也不希望在增加新操作时修改这些类。访问者模式使得我们可以将相关的访问操作集中起来定义在访问者类中，对象结构可以被多个不同的访问者类所使用，将对象本身与对象的访问操作分离。

(3)  对象结构中对象对应的类很少改变，但经常需要在此对象结构上定义新的操作。
|     <table border="0" cellspacing="0" cellpadding="0" width="564"><tbody><tr><td> ****  | **练习** Sunny软件公司欲为某高校开发一套奖励审批系统，该系统可以实现教师奖励和学生奖励的审批(Award Check)，如果教师发表论文数超过10篇或者学生论文超过2篇可以评选科研奖，如果教师教学反馈分大于等于90分或者学生平均成绩大于等于90分可以评选成绩优秀奖。试使用访问者模式设计该系统，以判断候选人集合中的教师或学生是否符合某种获奖要求。 

Sunny软件公司欲为某高校开发一套奖励审批系统，该系统可以实现教师奖励和学生奖励的审批(Award Check)，如果教师发表论文数超过10篇或者学生论文超过2篇可以评选科研奖，如果教师教学反馈分大于等于90分或者学生平均成绩大于等于90分可以评选成绩优秀奖。试使用访问者模式设计该系统，以判断候选人集合中的教师或学生是否符合某种获奖要求。


 


