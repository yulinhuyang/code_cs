**第1章 重构，第一个示例**  

1.1　起点  
1.2　对此起始程序的评价  
1.3　重构的第一步  
1.4　分解statement 函数  
1.5　进展：大量嵌套函数  
1.6　拆分计算阶段与格式化阶段  
1.7　进展：分离到两个文件（和两个阶段）  
1.8　按类型重组计算过程  
1.9　进展：使用多态计算器来提供数据  
1.10　结语  

**第2章 重构的原则**
 
2.1　何谓重构  
2.2　两顶帽子  
2.3　为何重构  
2.4　何时重构  
2.5　重构的挑战  
2.6　重构、架构和YAGNI  
2.7　重构与软件开发过程  
2.8　重构与性能  
2.9　重构起源何处  
2.10　自动化重构  
2.11　延展阅读  

**第3章　代码的坏味道**  

3.1　神秘命名（Mysterious Name）  
3.2　重复代码（Duplicated Code）  
3.3　过长函数（Long Function）  
3.4　过长参数列表（Long Parameter List）  
3.5　全局数据（Global Data）  
3.6　可变数据（Mutable Data）  
3.7　发散式变化（Divergent Change）  
3.8　霰弹式修改（Shotgun Surgery）  
3.9　依恋情结（Feature Envy）  
3.10　数据泥团（Data Clumps）  
3.11　基本类型偏执（Primitive Obsession）  
3.12　重复的switch（Repeated Switches）  
3.13　循环语句（Loops）  
3.14　冗赘的元素（Lazy Element）  
3.15　夸夸其谈通用性（Speculative Generality）  
3.16　临时字段（Temporary Field）  
3.17　过长的消息链（Message Chains）  
3.18　中间人（Middle Man）  
3.19　内幕交易（Insider Trading）  
3.20　过大的类（Large Class）  
3.21　异曲同工的类（Alternative Classes with Different Interfaces）  
3.22　纯数据类（Data Class）  
3.23　被拒绝的遗赠（Refused Bequest）  
3.24　注释（Comments）
  
**第4章　构筑测试体系**

4.1　自测试代码的价值  
4.2　待测试的示例代码  
4.3　第 一个测试  
4.4　再添加一个测试  
4.5　修改测试夹具  
4.6　探测边界条件  
4.7　测试远不止如此  

**第5章 介绍重构名录**

5.1　重构的记录格式 
5.2　挑选重构的依据 

**第6章　第一组重构** 

6.1　提炼函数（Extract Function） 
6.2　内联函数（Inline Function） 
6.3　提炼变量（Extract Variable） 
6.4　内联变量（Inline Variable） 
6.5　改变函数声明（Change Function Declaration） 
6.6　封装变量（Encapsulate Variable） 
6.7　变量改名（Rename Variable） 
6.8　引入参数对象（Introduce Parameter Object） 
6.9　函数组合成类（Combine Functions into Class） 
6.10　函数组合成变换（Combine Functions into Transform） 
6.11　拆分阶段（Split Phase） 

**第7章　封装**
 
7.1　封装记录（Encapsulate Record） 
7.2　封装集合（Encapsulate Collection） 
7.3　以对象取代基本类型（Replace Primitive with Object） 
7.4　以查询取代临时变量（Replace Temp with Query） 
7.5　提炼类（Extract Class） 
7.6　内联类（Inline Class） 
7.7　隐藏委托关系（Hide Delegate） 
7.8　移除中间人（Remove Middle Man） 
7.9　替换算法（Substitute Algorithm）
 
**第8章　搬移特性** 

8.1　搬移函数（Move Function） 
8.2　搬移字段（Move Field） 
8.3　搬移语句到函数（Move Statements into Function） 
8.4　搬移语句到调用者（Move Statements to Callers） 
8.5　以函数调用取代内联代码（Replace Inline Code with Function Call） 
8.6　移动语句（Slide Statements） 
8.7　拆分循环（Split Loop） 
8.8　以管道取代循环（Replace Loop with Pipeline） 
8.9　移除死代码（Remove Dead Code） 

**第9章　重新组织数据** 

9.1　拆分变量（Split Variable） 
9.2　字段改名（Rename Field） 
9.3　以查询取代派生变量（Replace Derived Variable with Query） 
9.4　将引用对象改为值对象（Change Reference to Value） 
9.5　将值对象改为引用对象（Change Value to Reference） 

**第10章　简化条件逻辑**
 
10.1　分解条件表达式（Decompose Conditional） 
10.2　合并条件表达式（Consolidate Conditional Expression） 
10.3　以卫语句取代嵌套条件表达式（Replace Nested Conditional with Guard Clauses） 
10.4　以多态取代条件表达式（Replace Conditional with Polymorphism） 
10.5　引入特例（Introduce Special Case） 
10.6　引入断言（Introduce Assertion）
 
**第11章　重构API** 

11.1　将查询函数和修改函数分离（Separate Query from Modifier） 
11.2　函数参数化（Parameterize Function） 
11.3　移除标记参数（Remove Flag Argument） 
11.4　保持对象完整（Preserve Whole Object） 
11.5　以查询取代参数（Replace Parameter with Query） 
11.6　以参数取代查询（Replace Query with Parameter） 
11.7　移除设值函数（Remove Setting Method） 
11.8　以工厂函数取代构造函数（Replace Constructor with Factory Function） 
11.9　以命令取代函数（Replace Function with Command） 
11.10　以函数取代命令（Replace Command with Function） 

**第12章　处理继承关系**

12.1　函数上移（Pull Up Method） 
12.2　字段上移（Pull Up Field） 
12.3　构造函数本体上移（Pull Up Constructor Body） 
12.4　函数下移（Push Down Method） 
12.5　字段下移（Push Down Field） 
12.6　以子类取代类型码（Replace Type Code with Subclasses） 
12.7　移除子类（Remove Subclass） 
12.8　提炼超类（Extract Superclass） 
12.9　折叠继承体系（Collapse Hierarchy） 
12.10　以委托取代子类（Replace Subclass with Delegate） 
12.11　以委托取代超类（Replace Superclass with Delegate） 
