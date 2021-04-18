
<!-- GFM-TOC -->

https://github.com/NxeedGoto/Refactoring2-zh



* [一、第一个示例](#一第一个示例)
* [二、重构原则](#二重构原则)
    * [定义](#定义)
    * [为何重构](#为何重构)
    * [三次法则](#三次法则)
    * [间接层与重构](#间接层与重构)
    * [修改接口](#修改接口)
    * [何时不该重构](#何时不该重构)
    * [重构与设计](#重构与设计)
    * [重构与性能](#重构与性能)
* [三、代码的坏味道](#三代码的坏味道)
    * [1. 神秘命名](#1-神秘命名)
    * [2. 重复代码](#2-重复代码)
    * [2. 过长函数](#2-过长函数)
    * [3. 过长函数](#3-过长函数)
    * [4. 过长的参数列表](#4-过长的参数列表)
    * [5. 全局数据](#5-全局数据)
    * [6. 可变数据](#6-可变数据)
    * [7. 发散式变化](#7-发散式变化)
    * [8. 霰弹式修改](#8-霰弹式修改)
    * [9. 依恋情结](#9-依恋情结)
    * [10. 数据泥团](#10-数据泥团)
    * [11. 基本类型偏执](#11-基本类型偏执)
    * [12. 重复的switch](#12-重复的switch)
    * [13. 循环语句](#13-循环语句)
    * [14. 冗赘的元素](#14-冗赘的元素)
    * [15. 夸夸其谈通用性](#15-夸夸其谈通用性)
    * [16. 临时字段](#16-临时字段)
    * [17. 过长的消息链](#17-过长的消息链)
    * [18. 中间人](#18-中间人)
    * [19. 内幕交易](#19-内幕交易)
    * [20. 过大的类](#20-过大的类)
    * [21. 异曲同工的类](#21-异曲同工的类)
    * [22. 纯数据类](#22-纯数据类)
    * [23. 被拒绝的馈赠](#23-被拒绝的馈赠)
    * [23. 过多的注释](#23-过多的注释)
* [四、构筑测试体系](#四构筑测试体系)
* [六、重新组织函数](#五重新组织函数)
    * [1. 提炼函数](#1-提炼函数)
    * [2. 内联函数](#2-内联函数)
    * [3. 提炼变量](#3-提炼变量)
    * [4. 内联变量](#4-内联变量)
    * [5. 改变函数声明](#5-改变函数声明)
    * [6. 封装变量](#6-封装变量)
    * [7. 变量改名](#7-变量改名)
    * [8. 引入参数对象](#8-引入参数对象)
    * [9. 函数组合成类](#9-函数组合成类)
    * [10. 函数组合成变换](#10-函数组合成变换)
    * [11. 拆分阶段](#11-拆分阶段)
* [七、封装](#七 封装)
    * [1. 封装记录](#1-封装记录)
    * [2. 封装集合](#2-封装集合)
    * [3. 以对象取代基本类型](#3-以对象取代基本类型)
    * [4. 以查询取代临时变量](#4-以查询取代临时变量)
    * [5. 提炼类](#5-提炼类)
    * [6. 内联类](#6-内联类)
    * [7. 隐藏委托关系](#7-隐藏委托关系)
    * [8. 移除中间人](#8-移除中间人)
    * [9. 替换算法](#9-替换算法)
* [八、 搬移特性](#八  搬移特性)
    * [1. 搬移函数](#1-搬移函数)
    * [2. 搬移字段](#2-搬移字段)
    * [3. 搬移语句到函数](#3-搬移语句到函数)
    * [4. 搬移语句到调用者](#4-搬移语句到调用者)
    * [5. 以函数调用取代内联代码](#5-以函数调用取代内联代码)
    * [6. 移动语句](#6-移动语句)
    * [7. 拆分循环](#7-拆分循环)
    * [8. 以管道取代循环](#8-以管道取代循环)
    * [9. 移除死代码](#9-移除死代码)
* [九、重新组织数据](#九重新组织数据)
    * [1. 拆分变量](#1-拆分变量)
    * [2. 字段改名](#2-字段改名)
    * [3. 以查询取代派生变量](#3-以查询取代派生变量)
    * [4. 将引用对象改为值对象](#4-将引用对象改为值对象)
    * [5. 将值对象改为引用对象](#5-将值对象改为引用对象)
* [十、简化条件逻辑](#十简化条件逻辑)
    * [1. 分解条件表达式](#1-分解条件表达式)
    * [2. 合并条件表达式](#2-合并条件表达式)
    * [3. 以卫语句取代嵌套条件表达式](#3-以卫语句取代嵌套条件表达式)
    * [4. 以多态取代条件表达式](#4-以多态取代条件表达式)
    * [5. 引入特例](#5-引入特例)
    * [6. 引入断言](#6-引入断言)
* [十一、重构 API](#十一重构 API)
    * [1. 将查询函数和修改函数分离](#1-将查询函数和修改函数分离)
    * [2. 函数参数化](#2-函数参数化)
    * [3. 移除标记参数](#3-移除标记参数)
    * [4. 保持对象完整](#4-保持对象完整)
    * [5. 以查询取代参数](#5-以查询取代参数)
    * [6. 以参数取代查询](#6-以参数取代查询)
    * [7. 移除设值函数](#7-移除设值函数)
    * [8. 以工厂函数取代构造函数](#8-以工厂函数取代构造函数)
    * [9. 以命令取代函数](#9-以命令取代函数)
    * [10. 以函数取代命令](#10-以函数取代命令)
* [十二、处理继承关系](#十二处理继承关系)
    * [1. 函数上移](#1-函数上移)
    * [2. 字段上移](#2-字段上移)
    * [3. 构造函数本体上移](#3-构造函数本体上移)
    * [4. 函数下移](#4-函数下移)
    * [5. 字段下移](#5-字段下移)
    * [6. 以子类取代类型码](#6-以子类取代类型码)
    * [7. 移除子类](#7-移除子类)
    * [8. 提炼超类](#8-提炼超类)
    * [9. 折叠继承体系](#9-折叠继承体系)
    * [10. 以委托取代子类](#10-以委托取代子类)
    * [11. 以委托取代超类](#11-以委托取代超类)
    * <!-- GFM-TOC -->


# 一、第一个示例

如果你发现自己需要为程序添加一个特性，而代码结构使你无法很方便地达成目的，那就先重构这个程序。

在重构前，需要先构建好可靠的测试环境，确保安全地重构。

重构需要以微小的步伐修改程序，如果重构过程发生错误，很容易就能发现错误。

我得确保即将修改的代码拥有一组可靠的测试，将测试视为 bug 检测器。



**案例分析** 

重构前：

```java
function statement (invoice, plays) {
  let totalAmount = 0;
  let volumeCredits = 0;
  let result = `Statement for ${invoice.customer}\n`;
  const format = new Intl.NumberFormat("en-US",
                        { style: "currency", currency: "USD",
                          minimumFractionDigits: 2 }).format;
  for (let perf of invoice.performances) {
    const play = plays[perf.playID];
    let thisAmount = 0;

    switch (play.type) {
    case "tragedy":
      thisAmount = 40000;
      if (perf.audience &gt; 30) {
        thisAmount += 1000 * (perf.audience - 30);
      }
      break;
    case "comedy":
      thisAmount = 30000;
      if (perf.audience &gt; 20) {
        thisAmount += 10000 + 500 * (perf.audience - 20);
      }
      thisAmount += 300 * perf.audience;
      break;
    default:
        throw new Error(`unknown type: ${play.type}`);
    }

    // add volume credits
    volumeCredits += Math.max(perf.audience - 30, 0);
    // add extra credit for every ten comedy attendees
    if ("comedy" === play.type) volumeCredits += Math.floor(perf.audience / 5);

    // print line for this order
    result += ` ${play.name}: ${format(thisAmount/100)} (${perf.audience} seats)\n`;
    totalAmount += thisAmount;
  }
  result += `Amount owed is ${format(totalAmount/100)}\n`;
  result += `You earned ${volumeCredits} credits\n`;
  return result;
}
```

使用 switch 的准则是：只能在对象自己的数据上使用，而不能在另一个对象的数据基础上使用。解释如下：switch 使用的数据通常是一组相关的数据。

直接将 amount For 提炼成为 statement 的一个内嵌函数。

个人的编码风格：永远将函数的返回值命名为“result”。

移除 play 变量：以查询取代临时变量。

- 使用拆分循环（227）分离出累加过程；
- 使用移动语句（223）将累加变量的声明与累加过程集中到一起；
- 使用提炼函数（106）提炼出计算总数的函数；
- 使用内联变量（123）完全移除中间变量。

重构后：

```javascript
function statement (invoice, plays) {
  let result = `Statement for ${invoice.customer}\n`;
  for (let perf of invoice.performances) {
    result += ` ${playFor(perf).name}: ${usd(amountFor(perf))} (${perf.audience} seats)\n`;
  }
  result += `Amount owed is ${usd(totalAmount())}\n`;
  result += `You earned ${totalVolumeCredits()} credits\n`;
  return result;

  function totalAmount() {
    let result = 0;
    for (let perf of invoice.performances) {
      result += amountFor(perf);
    }
    return result;
  }
  function totalVolumeCredits() {
    let result = 0;
    for (let perf of invoice.performances) {
      result += volumeCreditsFor(perf);
    }
    return result;
  }
  function usd(aNumber) {
    return new Intl.NumberFormat("en-US",
                        { style: "currency", currency: "USD",
                          minimumFractionDigits: 2 }).format(aNumber/100);
  }
  function volumeCreditsFor(aPerformance) {
    let result = 0;
    result += Math.max(aPerformance.audience - 30, 0);
    if ("comedy" === playFor(aPerformance).type) result += Math.floor(aPerformance.audience / 5);
    return result;
  }
  function playFor(aPerformance) {
    return plays[aPerformance.playID];
  }
  function amountFor(aPerformance) {
    let result = 0;
    switch (playFor(aPerformance).type) {
    case "tragedy":
      result = 40000;
      if (aPerformance.audience &gt; 30) {
        result += 1000 * (aPerformance.audience - 30);
      }
      break;
    case "comedy":
      result = 30000;
      if (aPerformance.audience &gt; 20) {
        result += 10000 + 500 * (aPerformance.audience - 20);
      }
      result += 300 * aPerformance.audience;
      break;
    default:
      throw new Error(`unknown type: ${playFor(aPerformance).type}`);
    }
    return result;
  }
}
```

###### **拆分计算阶段与格式化阶段**

将逻辑分成两部分：一部分计算详单所需的数据，另一部分将数据渲染成文本或 HTML。第一阶段会创建一个中转数据结构，再把它传递给第二阶段.



**分离到两个文件**



**按类型重组计算过程**

最核心的一招是以多态取代条件表达式（272），将多个同样的类型码分支用多态取代



**使演出计算器表现出多态性**

以工厂函数取代构造函数

```java
function createPerformanceCalculator(aPerformance, aPlay) {
  switch (aPlay.type) {
    case "tragedy":
      return new TragedyCalculator(aPerformance, aPlay);
    case "comedy":
      return new ComedyCalculator(aPerformance, aPlay);
    default:
      throw new Error(`unknown type: ${aPlay.type}`);
  }
}

class TragedyCalculator extends PerformanceCalculator {}
class ComedyCalculator extends PerformanceCalculator {}
```

# 二、重构原则

## 定义

重构是对软件内部结构的一种调整，目的是在不改变软件可观察行为的前提下，提高其可理解性，降低其修改成本。

## 为何重构

- 改进软件设计
- 使软件更容易理解
- 帮助找到 Bug
- 提高编程速度

## 三次法则

第一次做某件事时只管去做；第二次做类似事情时可以去做；第三次再做类似的事，就应该重构。

## 间接层与重构

计算机科学中的很多问题可以通过增加一个间接层来解决，间接层具有以下价值：

- 允许逻辑共享
- 分开解释意图和实现
- 隔离变化
- 封装条件逻辑。

重构可以理解为在适当的位置插入间接层以及在不需要时移除间接层。

## 修改接口

如果重构手法改变了已发布的接口，就必须维护新旧两个接口。

可以保留旧接口，让旧接口去调用新接口，并且使用 Java 提供的 @deprecation 将旧接口标记为弃用。

可见修改接口特别麻烦，因此除非真有必要，否则不要发布接口，并且不要过早发布接口。

## 何时不该重构

当现有代码过于混乱时，应当重写而不是重构。

一个折中的办法是，将代码封装成一个个组件，然后对各个组件做重写或者重构的决定。

## 重构与设计

软件开发无法预先设计，因为开发过程有很多变化发生，在最开始不可能都把所有情况考虑进去。

重构可以简化设计，重构在一个简单的设计上进行修修改改，当变化发生时，以一种灵活的方式去应对变化，进而带来更好的设计。

## 重构与性能

为了软代码更容易理解，重构可能会导致性能减低。

在编写代码时，不用对性能过多关注，只有在最后性能优化阶段再考虑性能问题。

应当只关注关键代码的性能，因为只有一小部分的代码是关键代码。

度量工具

# 三、代码的坏味道

本章主要介绍一些不好的代码，也就是说这些代码应该被重构。

文中提到的具体重构原则可以先忽略。

## 1.神秘命名

Mysterious Name

## 2. 重复代码

> Duplicated Code

同一个类的两个函数有相同表达式，则用 Extract Method 提取出重复代码；

两个互为兄弟的子类含有相同的表达式，先使用 Extract Method，然后把提取出来的函数 Pull Up Method 推入超类。

如果只是部分相同，用 Extract Method 分离出相似部分和差异部分，然后使用 Form Template Method 这种模板方法设计模式。

如果两个毫不相关的类出现重复代码，则使用 Extract Class 方法将重复代码提取到一个独立类中。

## 3. 过长函数

> Long Method

函数应该尽可能小，因为小函数具有解释能力、共享能力、选择能力。

分解长函数的原则：当需要用注释来说明一段代码时，就需要把这部分代码写入一个独立的函数中。

Extract Method 会把很多参数和临时变量都当做参数，可以用 Replace Temp with Query 消除临时变量，Introduce Parameter Object 和 Preserve Whole Object 可以将过长的参数列变得更简洁。

条件和循环语句往往也需要提取到新的函数中。

> Large Class

应该尽可能让一个类只做一件事，而过大的类做了过多事情，需要使用 Extract Class 或 Extract Subclass。

先确定客户端如何使用该类，然后运用 Extract Interface 为每一种使用方式提取出一个接口。

## 4. 过长的参数列表

> Long Parameter List

太长的参数列表往往会造成前后不一致，不易使用。

面向对象程序中，函数所需要的数据通常内在宿主类中找到。

函数组合成类

## 5.全局数据

Global Data

封装变量

## 6.可变数据

Mutable Data

将查询函数与修改函数分离

查询取代派生变量

## 7. 发散式变化

> Divergent Change

设计原则：一个类应该只有一个引起改变的原因。也就是说，针对某一外界变化所有相应的修改，都只应该发生在单一类中。

针对某种原因的变化，使用 Extract Class 将它提炼到一个类中。

拆分阶段--->提炼函数

## 8. 霰弹式修改

> Shotgun Surgery

一个变化引起多个类修改；

使用 Move Method 和 Move Field 把所有需要修改的代码放到同一个类中。

内联函数与内联类

## 9. 依恋情结

> Feature Envy

一个函数对某个类的兴趣高于对自己所处类的兴趣，通常是过多访问其它类的数据，

使用 Move Method 将它移到该去的地方，如果对多个类都有 Feature Envy，先用 Extract Method 提取出多个函数。

## 10. 数据泥团

> Data Clumps

有些数据经常一起出现，比如两个类具有相同的字段、许多函数有相同的参数，这些绑定在一起出现的数据应该拥有属于它们自己的对象。

使用 Extract Class 将它们放在一起。

## 11. 基本类型偏执

> Primitive Obsession

使用类往往比使用基本类型更好，使用 Replace Data Value with Object 将数据值替换为对象。

## 12. 重复的switch  

> Repeated Switch 

以多态取代条件表达式

## 13.循环语句

loops

使用以管道取代循环（231），管道操作（如 filter 和 map）可以帮助我们更快地看清被处理的元素以及处理它们的动作

## 14.冗赘的元素

Lazy Element

使用内联函数（115）或是内联类（186）。如果这个类处于一个继承体系中，可以使用折叠继承体系。

## 15.夸夸其谈通用性

Speculative Generality

请运用折叠继承体系（380）。不必要的委托可运用内联函数（115）和内联类（186）除掉。如果函数的某些参数未被用上，可以用改变函数声明（124）去掉这些参数。

## 16.临时字段

Temporary Field

提炼类，用搬移函数（198）把所有和这些字段相关的代码都放进这个新家。

## 17.过长的消息链

Message Chains

隐藏委托关系，把所有中间对象都变成“中间人”。

一个对象请求另一个对象，然后再向后者请求另一个对象，然后...，这就是消息链。采用这种方式，意味着客户代码将与对象间的关系紧密耦合。

改用函数链，用函数委托另一个对象来处理。

## 18.中间人

中间人负责处理委托给它的操作，如果一个类中有过多的函数都委托给其它类，那就是过度运用委托，应当 Remove Middle Man，直接与负责的对象打交道。

## 19.内幕交易

Insider Trading

请运用以委托取代子类（381）或以委托取代超类（399）让它离开继承体系

## 20.过大的类

Large Class

提炼超类（375）或者以子类取代类型码（362）

## 21. 异曲同工的类

> Alernative Classes with Different Interfaces

两个函数做同一件事，却有着不同的签名。

使用 Rename Method 根据它们的用途重新命名。

## 22. 纯数据类

> Data Class

它只拥有一些数据字段，以及用于访问这些字段的函数，除此之外一无长物。

找出字段使用的地方，然后把相应的操作移到 Data Class 中

## 23. 被拒绝的馈赠

> Refused Bequest

子类不想继承超类的所有函数和数据。

为子类新建一个兄弟类，不需要的函数或数据使用 Push Down Method 和 Push Down Field 下推给那个兄弟。

## 24. 过多的注释

> Comments

使用 Extract Method 提炼出需要注释的部分，然后用函数名来解释函数的行为。

先尝试重构，试着让所有注释变得多余。



# 四、构筑测试体系

Java 可以使用 Junit 进行单元测试。

测试应该能够完全自动化，并能检查测试的结果。Junit 可以做到。

小步修改，频繁测试。

单元测试的对象是类的方法，而功能测是以客户的角度保证软件正常运行。

应当集中测试可能出错的边界条件。

测试夹具

断言库

修改测试夹具

t探测边界条件

# 六、重新组织函数

## 6.1 提炼函数

> Extract Function
>
> 
>
> ```java
> function printOwing(invoice) {
>   printBanner();
>   let outstanding = calculateOutstanding();
> 
>   //print details
>   console.log(`name: ${invoice.customer}`);
>   console.log(`amount: ${outstanding}`);
> }
> 
> ---->：
> function printOwing(invoice) {
>   printBanner();
>   let outstanding = calculateOutstanding();
>   printDetails(outstanding);
> 
>   function printDetails(outstanding) {
>     console.log(`name: ${invoice.customer}`);
>     console.log(`amount: ${outstanding}`);
>   }
> }
> ```

将这段代码放进一个独立函数中，并让函数名称解释该函数的用途。

写非常小的函数——通常只有几行的长度。

短函数可以更容易地被缓存，小函数得有个好名字才行。

## 6.2 内联函数

> Inline Function

```java
function getRating(driver) {
 return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver) {
 return driver.numberOfLateDeliveries &gt; 5;
}

---->：
function getRating(driver) {
 return (driver.numberOfLateDeliveries &gt; 5) ? 2 : 1;
}
```

一个函数的本体与名称同样清楚易懂。

在函数调用点插入函数本体，然后移除该函数。

## 6.3  提炼变量

> Extract Variable

一个临时变量，只被简单表达式赋值一次，而它妨碍了其它重构手法。

将所有对该变量的引用替换为对它赋值的那个表达式自身。

```java
return (
  order.quantity * order.itemPrice -
  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
  Math.min(order.quantity * order.itemPrice * 0.1, 100)
);
---->
const basePrice = order.quantity * order.itemPrice;
const quantityDiscount =
  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05;
const shipping = Math.min(basePrice * 0.1, 100);
return basePrice - quantityDiscount + shipping;
```

## 6.4 内联变量

> Inline Variable

```java
let basePrice = anOrder.basePrice;
return (basePrice &gt; 1000);

---->
return anOrder.basePrice & gt;
1000;
```

内联手法消除变量

## 6.5 改变函数声明

> Change Function Declaration

```java
function circum(radius) {...}
---->
function circumference(radius) {...}
```

先写一句话注释描述这个函数的用途，再把这句注释变成函数的名字。

提炼函数

```java
function circum(radius) {
  return 2 * Math.PI * radius;
}
---->
function circum(radius) {
  return circumference(radius);
}
function circumference(radius) {
  return 2 * Math.PI * radius;
}
```

## 6.6 封装变量

> Encapsulate Variable

```java
let defaultOwner = { firstName: "Martin", lastName: "Fowler" };

---->
let defaultOwnerData = { firstName: "Martin", lastName: "Fowler" };
export function defaultOwner() {
  return defaultOwnerData;
}
export function setDefaultOwner(arg) {
  defaultOwnerData = arg;
}
```

每当看见一个公开（public）的字段时，我就会考虑使用封装变量（在这种情况下，这个重构手法常被称为封装字段）来缩小其可见范围。

限制变量的可见性。

## 6.7 变量改名

>  Rename Variable

```javascript
let a = height * width;
---->
let area = height * width;
```

封装变量

## 6.8 引入参数对象

> Introduce Parameter Object

```javascript
function amountInvoiced(startDate, endDate) {...}
function amountReceived(startDate, endDate) {...}
function amountOverdue(startDate, endDate) {...}

---->
function amountInvoiced(aDateRange) {...}
function amountReceived(aDateRange) {...}
function amountOverdue(aDateRange) {...}
```

某些参数总是很自然地同时出现，这些参数就是 Data Clumps。

以一个对象取代这些参数。

## 6.9 函数组合成类

> Combine Functions into Class

```javascript
function base(aReading) {...}
function taxableCharge(aReading) {...}
function calculateBaseCharge(aReading) {...}

---->
class Reading {
  base() {...}
  taxableCharge() {...}
  calculateBaseCharge() {...}
}
```

一组函数形影不离地操作同一块数据。

在有些编程语言中，类不是一等公民，而函数则是。面对这样的语言，可以用“函数作为对象”（Function As Object）的形式来实现这个重构手法。

## 6.10 函数组合成变换

> Combine Functions into Transform

```javascript
function base(aReading) {...}
function taxableCharge(aReading) {...}

---->
function enrichReading(argReading) {
  const aReading = _.cloneDeep(argReading);
  aReading.baseCharge = base(aReading);
  aReading.taxableCharge = taxableCharge(aReading);
  return aReading;
}
```

采用数据变换（transform）函数：这种函数接受源数据作为输入，计算出所有的派生数据，将派生数据以字段形式填入输出数据。

函数组合成变换的替代方案是函数组合成类。

对输入的记录做深复制clone deep或者浅复制。

## 6.11 拆分阶段

> Split Phase

```javascript
const orderData = orderString.split(/\s+/);
const productPrice = priceList[orderData[0].split("-")[1]];
const orderPrice = parseInt(orderData[1]) * productPrice;
const orderRecord = parseOrder(order);
const orderPrice = price(orderRecord, priceList);

---->
function parseOrder(aString) {
  const values = aString.split(/\s+/);
  return {
    productID: values[0].split("-")[1],
    quantity: parseInt(values[1]),
  };
}
function price(order, priceList) {
  return order.quantity * priceList[order.productID];
}
```

就是把一大段行为分成顺序执行的两个阶段，把数据处理逻辑分成顺序执行的多个步骤，每个步骤负责的任务全然不同。

可以引入一个中转数据结构，将其作为参数添加到提炼出的新函数的参数列表中。



# 七、封装

## 7.1 封装记录

> Encapsulate Record

为字段建立取值/设值函数，并用这些函数来访问字段。只有当子类想访问超类的一个字段，又想在子类中将对这个字段访问改为一个计算后的值，才使用这种方式，否则直接访问字段的方式简洁明了。

```javascript
organization = { name: "Acme Gooseberries", country: "GB" };
---->
class Organization {
  constructor(data) {
    this._name = data.name;
    this._country = data.country;
  }
  get name() {
    return this._name;
  }
  set name(arg) {
    this._name = arg;
  }
  get country() {
    return this._country;
  }
  set country(arg) {
    this._country = arg;
  }
}
```

## 7.2 封装集合

> Encapsulate Collection

函数返回集合的一个只读副本，并在这个类中提供添加/移除集合元素的函数。如果函数返回集合自身，会让用户得以修改集合内容而集合拥有者却一无所知。

```javascript
class Person {
  get courses() {return this._courses;}
  set courses(aList) {this._courses = aList;}

---->
class Person {
  get courses() {return this._courses.slice();}
  addCourse(aCourse) { ... }
  removeCourse(aCourse) { ... }
```

## 7.3 以对象取代基本类型

> Replace Primitive with Object

```javascript
  orders.filter(o =&gt; "high" === o.priority
               || "rush" === o.priority);

---->
  orders.filter(o =&gt; o.priority.higherThan(new Priority("normal")))
```

## 7.4 以查询取代临时变量

>  Replace Temp with Query

```javascript
  const basePrice = this._quantity * this._itemPrice;
if (basePrice &gt; 1000)
  return basePrice * 0.95;
else
  return basePrice * 0.98;


---->
  get basePrice() {this._quantity * this._itemPrice;}

...

if (this.basePrice &gt; 1000)
  return this.basePrice * 0.95;
else
  return this.basePrice * 0.98;
```

以查询取代临时变量（178）手法只适用于处理某些类型的临时变量：那些只被计算一次且之后不再被修改的变量。最简单的情况是，这个临时变量只被赋值一次。

将为变量赋值的代码段提炼成函数。

## 7.5 提炼类

> Extract Class

```javascript
  class Person {
 get officeAreaCode() {return this._officeAreaCode;}
 get officeNumber()   {return this._officeNumber;}

---->
  class Person {
 get officeAreaCode() {return this._telephoneNumber.areaCode;}
 get officeNumber()   {return this._telephoneNumber.number;}
}
class TelephoneNumber {
 get areaCode() {return this._areaCode;}
 get number()   {return this._number;}
}
```

决定如何分解类所负的责任。

## 7.6 内联类

> Inline Class

反向重构：提炼类（182）

```javascript
  class Person {
 get officeAreaCode() {return this._telephoneNumber.areaCode;}
 get officeNumber()  {return this._telephoneNumber.number;}
}
class TelephoneNumber {
 get areaCode() {return this._areaCode;}
 get number() {return this._number;}
}

---->
  class Person {
 get officeAreaCode() {return this._officeAreaCode;}
 get officeNumber()  {return this._officeNumber;}
```

内联类正好与提炼类（182）相反。如果一个类不再承担足够责任，不再有单独存在的理由（这通常是因为此前的重构动作移走了这个类的责任），我就会挑选这一“萎缩类”的最频繁用户（也是一个类），以本手法将“萎缩类”塞进另一个类中。

## 7.7 隐藏委托关系

> Hide Delegate

反向重构：移除中间人（192）

```javascript
  manager = aPerson.department.manager;

---->
  manager = aPerson.manager;

class Person {
  get manager() {return this.department.manager;}
```

对于每个委托关系中的函数，在服务对象端建立一个简单的委托函数。

## 7.8 移除中间人

>  Remove Middle Man

反向重构：隐藏委托关系（189）

```javascript
  manager = aPerson.manager;

class Person {
 get manager() {return this.department.manager;}

---->
  manager = aPerson.department.manager;
```

## 7.9 替换算法

> Substitute Algorithm

这时候可以先把原先的算法替换为一个较易修改的算法，这样后续的修改会轻松许多。

```javascript
  function foundPerson(people) {
 for(let i = 0; i &lt; people.length; i++) {
  if (people[i] === "Don") {
   return "Don";
  }
  if (people[i] === "John") {
   return "John";
  }
  if (people[i] === "Kent") {
   return "Kent";
  }
 }
 return "";
}

---->
  function foundPerson(people) {
 const candidates = ["Don", "John", "Kent"];
 return people.find(p =&gt; candidates.includes(p)) || '';
}
```



# 八、 搬移特性

## 8.1  搬移函数

> Move Function

曾用名：搬移函数（Move Method）

```javascript
class Account {
get overdraftCharge() {...}

---->
class AccountType {
  get overdraftCharge() {...}
```

类中的某个函数与另一个类进行更多交流：调用后者或者被后者调用。

将这个函数搬移到另一个类中。

对一个面向对象的程序而言，类作为最主要的模块化手段，其本身就能充当函数的上下文；通过嵌套的方式，外层函数也能为内层函数提供一个上下文。

## 8.2 搬移字段

> Move Field

```javascript
  class Customer {
  get plan() {return this._plan;}
  get discountRate() {return this._discountRate;}

---->
  class Customer {
  get plan() {return this._plan;}
  get discountRate() {return this.plan.discountRate;}
```

领域驱动设计（domain-driven design）

调整源对象的访问函数，令其使用目标对象的字段。

## 8.3 搬移语句到函数

> Move Statements into Function

反向重构：搬移语句到调用者（217）

```javascript
result.push(`&lt;p&gt;title: ${person.photo.title}&lt;/p&gt;`);
result.concat(photoData(person.photo));

function photoData(aPhoto) {
  return [
    `&lt;p&gt;location: ${aPhoto.location}&lt;/p&gt;`,
    `&lt;p&gt;date: ${aPhoto.date.toDateString()}&lt;/p&gt;`,
  ];
}

---->
result.concat(photoData(person.photo));

function photoData(aPhoto) {
  return [
    `&lt;p&gt;title: ${aPhoto.title}&lt;/p&gt;`,
    `&lt;p&gt;location: ${aPhoto.location}&lt;/p&gt;`,
    `&lt;p&gt;date: ${aPhoto.date.toDateString()}&lt;/p&gt;`,
  ];
}
```

最重要的一条当属“消除重复”.

对不同的调用者需有不同的行为，那时再通过搬移语句到调用。

## 8.4 搬移语句到调用者

> Move Statements to Callers

反向重构：搬移语句到函数（213）

```javascript
emitPhotoData(outStream, person.photo);

function emitPhotoData(outStream, photo) {
  outStream.write(`&lt;p&gt;title: ${photo.title}&lt;/p&gt;\n`);
  outStream.write(`&lt;p&gt;location: ${photo.location}&lt;/p&gt;\n`);
}
---->
emitPhotoData(outStream, person.photo);
outStream.write(`&lt;p&gt;location: ${person.photo.location}&lt;/p&gt;\n`);

function emitPhotoData(outStream, photo) {
  outStream.write(`&lt;p&gt;title: ${photo.title}&lt;/p&gt;\n`);
}
```

使用移动语句（223）手法，先将表现不同的行为调整到函数的开头或结尾，再使用本手法将语句搬移到其调用点。

## 8.5 以函数调用取代内联代码

> Replace Inline Code with Function Call

```javascript
let appliesToMass = false;
for (const s of states) {
  if (s === "MA") appliesToMass = true;
}
---->
appliesToMass = states.includes("MA");
```

## 8.6 移动语句 

>  Slide Statements 

曾用名：合并重复的代码片段（Consolidate Duplicate Conditional Fragments）

```javascript
const pricingPlan = retrievePricingPlan();
const order = retreiveOrder();
let charge;
const chargePerUnit = pricingPlan.unit;

---->
const pricingPlan = retrievePricingPlan();
const chargePerUnit = pricingPlan.unit;
const order = retreiveOrder();
let charge;
```

在条件表达式的每个分支上有着相同的一段代码。

将这段重复代码搬移到条件表达式之外。

## 8.7 拆分循环

> Split Loop

```javascript
let averageAge = 0;
let totalSalary = 0;
for (const p of people) {
  averageAge += p.age;
  totalSalary += p.salary;
}
averageAge = averageAge / people.length;

---->
let totalSalary = 0;
for (const p of people) {
  totalSalary += p.salary;
}

let averageAge = 0;
for (const p of people) {
  averageAge += p.age;
}
averageAge = averageAge / people.length;
```

如果能够将循环拆分，让一个循环只做一件事情，那就能确保每次修改时你只需要理解要修改的那块代码的行为就可以了。

先进行重构，然后再进行性能优化。



## 8.8 以管道取代循环

>  Replace Loop with Pipeline

```javascript
const names = [];
for (const i of input) {
  if (i.job === "programmer")
    names.push(i.name);
}

---->
const names = input
  .filter(i =&gt; i.job === "programmer")
  .map(i =&gt; i.name)
;
```

集合管道[mf-cp]是这样一种技术，它允许我使用一组运算来描述集合的迭代过程，其中每种运算接收的入参和返回值都是一个集合。这类运算有很多种，最常见的则非 map 和 filter 莫属：map 运算是指用一个函数作用于输入集合的每一个元素上，将集合变换成另外一个集合的过程；filter 运算是指用一个函数从输入集合中筛选出符合条件的元素子集的过程。

## 8.9 移除死代码

> Remove Dead Code

```
if (false) {
  doSomethingThatUsedToMatter();
}
```



# 九、重新组织数据

## 9.1 拆分变量

> Split Variable

曾用名：移除对参数的赋值（Remove Assignments to Parameters）

曾用名：分解临时变量（Split Temp）

```javascript
let temp = 2 * (height + width);
console.log(temp);
temp = height * width;
console.log(temp);
---->
const perimeter = 2 * (height + width);
console.log(perimeter);
const area = height * width;
console.log(area);
```

循环变量”和“结果收集变量”就是两个典型例子：循环变量（loop variable）会随循环的每次运行而改变（例如 for（let i=0; i<10; i++）语句中的 i）；结果收集变量（collecting variable）负责将“通过整个函数的运算”而构成的某个值收集起来。

如果变量承担多个责任，它就应该被替换（分解）为多个变量，每个变量只承担一个责任。

## 9.2 字段改名

>  Rename Field

```javascript
class  Organization {
  get name() {...}
}

---->
class  Organization {
  get title() {...}
}
```



## 9.3 以查询取代派生变量

>  Replace Derived Variable with Query

```javascript
get discountedTotal() {return this._discountedTotal;}
set discount(aNumber) {
 const old = this._discount;
 this._discount = aNumber;
 this._discountedTotal += old - aNumber;
}

---->
get discountedTotal() {return this._baseTotal - this._discount;}
set discount(aNumber) {this._discount = aNumber;}
```

尽量把可变数据的作用域限制在最小范围.

“根据源数据生成新数据结构”的变换操作可以保持不变，即便我们可以将其替换为计算操作。实际上，这是两种不同的编程风格：一种是对象风格，把一系列计算得出的属性包装在数据结构中；另一种是函数风格，将一个数据结构变换为另一个数据结构。如果源数据会被修改，而你必须负责管理派生数据结构的整个生命周期，那么对象风格显然更好。但如果源数据不可变，或者派生数据用过即弃，那么两种风格都可行。

## 9.4 将引用对象改为值对象

>  Change Reference to Value

反向重构：将值对象改为引用对象（256）

```javascript
class Product {
applyDiscount(arg) {this._price.amount -= arg;}

---->
class Product {
applyDiscount(arg) {
  this._price = new Money(this._price.amount - arg, this._price.currency);
}
```

如果将内部对象视为引用对象，在更新其属性时，我会保留原对象不动，更新内部对象的属性；如果将其视为值对象，我就会替换整个内部对象，新换上的对象会有我想要的属性值。

以 Change Value to Reference 相反。值对象有个非常重要的特性：它是不可变的，不可变表示如果要改变这个对象，必须用一个新的对象来替换旧对象，而不是修改旧对象。

需要为值对象实现 equals() 和 hashCode() 方法

## 9.5 将值对象改为引用对象

> Change Value to Reference

反向重构：将引用对象改为值对象（252）

```javascript
let customer = new Customer(customerData);
---->
let customer = customerRepository.get(customerData.id);
```

例如，我可能会读取一系列订单数据，其中有多条订单属于同一个顾客。遇到这样的共享关系时，既可以把顾客信息作为值对象来看待，也可以将其视为引用对象。如果将其视为值对象，那么每份订单数据中都会复制顾客的数据；而如果将其视为引用对象，对于一个顾客，就只有一份数据结构，会有多个订单与之关联

将彼此相等的实例替换为同一个对象。这就要用一个工厂来创建这种唯一对象，工厂类中需要保留一份已经创建对象的列表，当要创建一个对象时，先查找这份列表中是否已经存在该对象，如果存在，则返回列表中的这个对象；否则，新建一个对象，添加到列表中，并返回该对象。

#  十、简化条件逻辑

## 10.1 分解条件表达式

>  Decompose Conditional

```javascript
if (!aDate.isBefore(plan.summerStart) &amp;&amp; !aDate.isAfter(plan.summerEnd))
 charge = quantity * plan.summerRate;
else
 charge = quantity * plan.regularRate + plan.regularServiceCharge;

---->
if (summer())
 charge = summerCharge();
else
 charge = regularCharge();
```

对条件判断和每个条件分支分别运用提炼函数（106）手法。

对于一个复杂的条件语句，可以从 if、then、else 三个段落中分别提炼出独立函数。

## 10.2 合并条件表达式

>  Consolidate Conditional Expression

```javascript
if (anEmployee.seniority &lt; 2) return 0;
if (anEmployee.monthsDisabled &gt; 12) return 0;
if (anEmployee.isPartTime) return 0;

---->
if (isNotEligibleForDisability()) return 0;

function isNotEligibleForDisability() {
 return ((anEmployee.seniority &lt; 2)
     || (anEmployee.monthsDisabled &gt; 12)
     || (anEmployee.isPartTime));
}
```

有一系列条件测试，都得到相同结果。

将这些测试合并为一个条件表达式，并将这个条件表达式提炼成为一个独立函数。

## 10.3 以卫语句取代嵌套条件表达式

>  Replace Nested Conditional with Guard Clauses

```javascript
function getPayAmount() {
  let result;
  if (isDead) result = deadAmount();
  else {
    if (isSeparated) result = separatedAmount();
    else {
      if (isRetired) result = retiredAmount();
      else result = normalPayAmount();
    }
  }
  return result;
}

---->
function getPayAmount() {
  if (isDead) return deadAmount();
  if (isSeparated) return separatedAmount();
  if (isRetired) return retiredAmount();
  return normalPayAmount();
}
```

如果某个条件极其罕见，就应该单独检查该条件，并在该条件为真时立刻从函数中返回，这样的单独检查常常被称为“卫语句”（guard clauses）。

条件表达式通常有两种表现形式。第一种形式是：所有分支都属于正常行为。第二种形式则是：条件表达式提供的答案中只有一种是正常行为，其他都是不常见的情况，可以使用卫语句表现所有特殊情况。

## 10.4 以多态取代条件表达式

>  Replace Conditional with Polymorphism

```javascript
switch (bird.type) {
 case 'EuropeanSwallow':
  return "average";
 case 'AfricanSwallow':
  return (bird.numberOfCoconuts &gt; 2) ? "tired" : "average";
 case 'NorwegianBlueParrot':
  return (bird.voltage &gt; 100) ? "scorched" : "beautiful";
 default:
  return "unknown";

---->
class EuropeanSwallow {
 get plumage() {
  return "average";
 }
class AfricanSwallow {
 get plumage() {
   return (this.numberOfCoconuts &gt; 2) ? "tired" : "average";
 }
class NorwegianBlueParrot {
 get plumage() {
   return (this.voltage &gt; 100) ? "scorched" : "beautiful";
}
```

将这个条件表达式的每个分支放进一个子类内的覆写函数中，然后将原始函数声明为抽象函数。需要先使用 Replace Type Code with Subclass 或 Replace Type Code with State/Strategy 来建立继承结果。

可以把基础逻辑放进超类，这样我可以首先理解这部分逻辑，暂时不管各种变体，然后我可以把每种变体逻辑单独放进一个子类，其中的代码着重强调与基础逻辑的差异。

## 10.5 引入特例

>  Introduce Special Case

曾用名：引入 Null 对象（Introduce Null Object）

```javascript
if (aCustomer === "unknown") customerName = "occupant";

---->
class UnknownCustomer {
  get name() {return "occupant";}
```

使用“特例”（Special Case）模式：创建一个特例元素，用以表达对这种特例的共用行为的处理。这样我就可以用一个函数调用取代大部分特例检查逻辑。

一个通常需要特例处理的值就是 null，这也是这个模式常被叫作“Null 对象”（Null Object）模式的原因——我喜欢说：Null 对象是特例的一种特例。

## 10.6 引入断言

>  Introduce Assertion

```javascript
  if (this.discountRate)
  base = base - (this.discountRate * base);

---->
  assert(this.discountRate&gt;= 0);
if (this.discountRate)
  base = base - (this.discountRate * base);
```

断言是一个条件表达式，应该总是为真。

以断言明确表现某种假设。断言只能用于开发过程中，产品代码中不会有断言。

# 十一、重构 API

## 11.1 将查询函数和修改函数分离

>  Separate Query from Modifier

```javascript
function getTotalOutstandingAndSendBill() {
const result = customer.invoices.reduce((total, each) =&gt; each.amount + total, 0);
sendBill();
return result;
}

---->
function totalOutstanding() {
return customer.invoices.reduce((total, each) =&gt; each.amount + total, 0);
}
function sendBill() {
emailGateway.send(formatBill(customer));
}
```

某个函数即返回对象状态值，又修改对象状态。

应当建立两个不同的函数，其中一个负责查询，另一个负责修改。任何有返回值的函数，都不应该有看得到的副作用。

## 11.2 函数参数化

>  Parameterize Function

曾用名：令函数携带参数（Parameterize Method）

```javascript
function tenPercentRaise(aPerson) {
  aPerson.salary = aPerson.salary.multiply(1.1);
}
function fivePercentRaise(aPerson) {
  aPerson.salary = aPerson.salary.multiply(1.05);
}
---->
function raise(aPerson, factor) {
  aPerson.salary = aPerson.salary.multiply(1 + factor);
}
```

运用改变函数声明（124），把需要作为参数传入的字面量添加到参数列表中。

## 11.3 移除标记参数

>  Remove Flag Argument

曾用名：以明确函数取代参数（Replace Parameter with Explicit Methods）

```javascript
function setDimension(name, value) {
  if (name === "height") {
    this._height = value;
    return;
  }
  if (name === "width") {
    this._width = value;
    return;
  }
}

---->
function setHeight(value) {
  this._height = value;
}
function setWidth(value) {
  this._width = value;
}
```

有一个函数，完全取决于参数值而采取不同行为。

针对该参数的每一个可能值，建立一个独立函数。

## 11.4 保持对象完整

> Preserve Whole Object

```javascript
const low = aRoom.daysTempRange.low;
const high = aRoom.daysTempRange.high;
if (aPlan.withinRange(low, high))

---->
if (aPlan.withinRange(aRoom.daysTempRange))
```

从某个对象中取出若干值，将它们作为某一次函数调用时的参数。

改为传递整个对象。

## 11.5 以查询取代参数

>  Replace Parameter with Query

曾用名：以函数取代参数（Replace Parameter with Method）

反向重构：以参数取代查询（327）

```javascript
availableVacation(anEmployee, anEmployee.grade);

function availableVacation(anEmployee, grade) {
  // calculate vacation...

---->
availableVacation(anEmployee)

function availableVacation(anEmployee) {
  const grade = anEmployee.grade;
  // calculate vacation...
```

对象调用某个函数，并将所得结果作为参数，传递给另一个函数。而接受该参数的函数本身也能够调用前一个函数。

让参数接收者去除该项参数，而是直接调用前一个函数。

## 11.6 以参数取代查询

> Replace Query with Parameter

反向重构：以查询取代参数（324）

```javascript
  targetTemperature(aPlan)

function targetTemperature(aPlan) {
  currentTemperature = thermostat.currentTemperature;
  // rest of function...

---->
  targetTemperature(aPlan, thermostat.currentTemperature)

function targetTemperature(aPlan, currentTemperature) {
  // rest of function...
```

如果一个函数用同样的参数调用总是给出同样的结果，我们就说这个函数具有“引用透明性”（referential transparency），这样的函数理解起来更容易。如果一个函数使用了另一个元素，而后者不具引用透明性，那么包含该元素的函数也就失去了引用透明性。

## 11.7 移除设值函数

> Remove Setting Method

```javascript
class Person {
get name() {...}
set name(aString) {...}

---->
class Person {
get name() {...}
```

这会导致构造函数成为设值函数的唯一使用者。

所谓“创建脚本”，首先是调用构造函数，然后就是一系列设值函数的调用，共同完成新对象的构造。创建脚本执行完以后，这个新生对象的部分（乃至全部）字段就不应该再被修改。设值函数只应该在起初的对象创建过程中调用。

类中的某个字段应该在对象创建时被设值，然后就不再改变。

去掉该字段的所有设值函数，并将该字段设为 final。

## 11.8 以工厂函数取代构造函数

> Replace Constructor with Factory Function

曾用名：以工厂函数取代构造函数（Replace Constructor with Factory Method）

```javascript
leadEngineer = new Employee(document.leadEngineer, "E");
---->
leadEngineer = createEngineer(document.leadEngineer);
```

希望在创建对象时不仅仅是做简单的建构动作。

将构造函数替换为工厂函数。

新建一个工厂函数，让它调用现有的构造函数。

将调用构造函数的代码改为调用工厂函数。

## 11.9 以命令取代函数

>  Replace Function with Command

曾用名：以函数对象取代函数（Replace Method with Method Object）

反向重构：以函数取代命令（344）

```javascript
function score(candidate, medicalExam, scoringGuide) {
  let result = 0;
  let healthLevel = 0;
  // long body code
}

---->
class Scorer {
  constructor(candidate, medicalExam, scoringGuide) {
    this._candidate = candidate;
    this._medicalExam = medicalExam;
    this._scoringGuide = scoringGuide;
  }

  execute() {
    this._result = 0;
    this._healthLevel = 0;
    // long body code
  }
}
```

函数，不管是独立函数，还是以方法（method）形式附着在对象上的函数，是程序设计的基本构造块。不过，将函数封装成自己的对象，有时也是一种有用的办法。这样的对象我称之为“命令对象”（command object），或者简称“命令”（command）。

“命令”是指一个对象，其中封装了一个函数调用请求。这是遵循《设计模式》[gof]一书中的命令模式（command pattern）。

## 11.10 以函数取代命令

> Replace Command with Function

反向重构：以命令取代函数（337）

```javascript
class ChargeCalculator {
  constructor(customer, usage) {
    this._customer = customer;
    this._usage = usage;
  }
  execute() {
    return this._customer.rate * this._usage;
  }
}
---->
function charge(customer, usage) {
  return customer.rate * usage;
}
```

命令对象为处理复杂计算提供了强大的机制。借助命令对象，可以轻松地将原本复杂的函数拆解为多个方法，彼此之间通过字段共享状态；拆解后的方法可以分别调用；开始调用之前的数据状态也可以逐步构建。但这种强大是有代价的。大多数时候，我只是想调用一个函数，让它完成自己的工作就好。



# 十二、处理继承关系

## 12.1 函数上移

> Pull Up Method

反向重构：函数下移（359）

```javascript
class Employee {...}

class Salesman extends Employee {
 get name() {...}
}

class Engineer extends Employee {
 get name() {...}
}

---->
class Employee {
 get name() {...}
}

class Salesman extends Employee {...}
class Engineer extends Employee {...}
```

如果两个函数工作流程大体相似，但实现细节略有差异，那么我会考虑先借助塑造模板函数（Form Template Method）[mf-ft]构造出相同的函数，然后再提升它们。

有些函数，在各个子类中产生完全相同的结果。

将该函数移至超类。

## 12.2 字段上移

> Pull Up Field

反向重构：字段下移（361）

```javascript
class Employee {...} // Java

class Salesman extends Employee {
 private String name;
}

class Engineer extends Employee {
 private String name;
}

---->
class Employee {
 protected String name;
}

class Salesman extends Employee {...}
class Engineer extends Employee {...}
```

两个子类拥有相同的字段。

将该字段移至超类。

## 12.3 构造函数本体上移

> Pull Up Constructor Body

```javascript
class Party {...}

class Employee extends Party {
 constructor(name, id, monthlyCost) {
  super();
  this._id = id;
  this._name = name;
  this._monthlyCost = monthlyCost;
 }
}

---->
class Party {
 constructor(name){
  this._name = name;
 }
}

class Employee extends Party {
 constructor(name, id, monthlyCost) {
  super(name);
  this._id = id;
  this._monthlyCost = monthlyCost;
 }
}
```

你在各个子类中拥有一些构造函数，它们的本体几乎完全一致。

在超类中新建一个构造函数，并在子类构造函数中调用它。

## 12.4 函数下移

>  Push Down Method

反向重构：函数上移（350）

```javascript
class Employee {
  get quota {...}
}

class Engineer extends Employee {...}
class Salesman extends Employee {...}

---->
class Employee {...}
class Engineer extends Employee {...}
class Salesman extends Employee {
  get quota {...}
}
```

超类中的某个函数只与部分子类有关。

将这个函数移到相关的那些子类去。

## 12.5 字段下移

>  Push Down Field

反向重构：字段上移（353）

```javascript
class Employee {   // Java
 private String quota;
}

class Engineer extends Employee {...}
class Salesman extends Employee {...}

---->
class Employee {...}
class Engineer extends Employee {...}

class Salesman extends Employee {
 protected String quota;
}
```

超类中的某个字段只被部分子类用到。将这个字段移到需要它的那些子类去。

## 12.6 以子类取代类型码

>  Replace Type Code with Subclasses

包含旧重构：以 State/Strategy 取代类型码（Replace Type Code with State/Strategy）

包含旧重构：提炼子类（Extract Subclass）

反向重构：移除子类（369）

```javascript
function createEmployee(name, type) {
  return new Employee(name, type);
}

---->
function createEmployee(name, type) {
  switch (type) {
    case "engineer": return new Engineer(name);
    case "salesman": return new Salesman(name);
    case "manager": return new Manager (name);
}
```

创建一个选择器逻辑，把类型码参数映射到新的子类。

如果选择直接继承的方案，就用以工厂函数取代构造函数（334）包装构造函数，把选择器逻辑放在工厂函数里；如果选择间接继承的方案，选择器逻辑可以保留在构造函数里。

## 12.7 移除子类

>  Remove Subclass

曾用名：以字段取代子类（Replace Subclass with Fields）

反向重构：以子类取代类型码（362）

```javascript
class Person {
  get genderCode() {
    return "X";
  }
}
class Male extends Person {
  get genderCode() {
    return "M";
  }
}
class Female extends Person {
  get genderCode() {
    return "F";
  }
}

---->
class Person {
  get genderCode() {
    return this._genderCode;
  }
}
```

各个子类的唯一差别只在“返回常量数据”的函数上。



## 12.8 提炼超类

>  Extract Superclass

```javascript
class Department {
 get totalAnnualCost() {...}
 get name() {...}
 get headCount() {...}
}

class Employee {
 get annualCost() {...}
 get name() {...}
 get id() {...}
}

---->
class Party {
 get name() {...}
 get annualCost() {...}
}

class Department extends Party {
 get annualCost() {...}
 get headCount() {...}
}

class Employee extends Party {
 get annualCost() {...}
 get id() {...}
}
```

## 12.9 折叠继承体系

> Collapse Hierarchy

```
class Employee {...}
class Salesman extends Employee {...}


class Employee {...}
```

### 动机

在重构类继承体系时，我经常把函数和字段上下移动。随着继承体系的演化，我有时会发现一个类与其超类已经没多大差别，不值得再作为独立的类存在。此时我就会把超类和子类合并起来。

## 12.10 以委托取代子类

> Replace Subclass with Delegate

```javascript
class Order {
  get daysToShip() {
    return this._warehouse.daysToShip;
  }
}

class PriorityOrder extends Order {
  get daysToShip() {
    return this._priorityPlan.daysToShip;
  }
}

---->
class Order {
  get daysToShip() {
    return this._priorityDelegate
      ? this._priorityDelegate.daysToShip
      : this._warehouse.daysToShip;
  }
}

class PriorityOrderDelegate {
  get daysToShip() {
    return this._priorityPlan.daysToShip;
  }
}
```

对于不同的变化原因，我可以委托给不同的类。委托是对象之间常规的关系。与继承关系相比，使用委托关系时接口更清晰、耦合更少。因此，继承关系遇到问题时运用以委托取代子类是常见的情况。

“对象组合优于类继承”

就是用状态（State）模式或者策略（Strategy）模式取代子类。这两个模式在结构上是相同的，都是由宿主对象把责任委托给另一个继承体系。

## 12.11 以委托取代超类

>  Replace Superclass with Delegate

曾用名：以委托取代继承（Replace Inheritance with Delegation）

```javascript
class List {...}
class Stack extends List {...}

---->
class Stack {
  constructor() {
    this._storage = new List();
  }
}
class List {...}
```
