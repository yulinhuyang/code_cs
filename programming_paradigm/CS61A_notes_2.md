# 16 sheme
## quote
阻止记号被求值，将符号原封不动地传递到程序，而不是求值后的其他内容
```scheme
> (+ 2 3)
5
> (quote (+ 2 3))
(+ 2 3)
> '(+ 2 3)
(+ 2 3)
```

## cons ,car ,cdr
- 一对 pairs 是 `cons`
- `car` 是前面的
- `cdr` 是后面的
- `nil` 是 `empty`

## if ,and ,define
- `if`表达式：`(if <predicate> <consequent> <alternative>) `
- `and` 和 `or`：`(and <e1> ... <en>), (or <e1> ... <en>)`
- 绑定符号：`(define <symbol> <expression>)`
- 定义函数：`(define (<symbol> <formal parameters>) <body>)`


## lambda
` (lambda (<formal-parameters>) <body>)`

举例：  
`(define (plus4 x) (+ x 4))`   
`(define plus4 (lambda (x) (+ x 4)))`
    
## 注意
`scheme` 中只有 ` #f, false, False ` 代表假值

`=, eq?, equal?`
  - `=` 只能用于比较数字。
  - `eq?`在 `Python` 中类似于 `==`，用于比较两个 `non-pairs` (数字、布尔运算等)。否则，`eq?` 的行为就像在Python中的 `is` 一样。
  - `equal?` 比较 `pairs`, 通过比较它们的 `cars` 是否相等，它们的 `cdrs` 是否相等。否则，`equal?` 的行为就像 `eq?` 一样。

```scheme
scm> (eq? '(1 2 3) '(1 2 3))
#f
scm> (equal? '(1 2 3) '(1 2 3))
#t
scm> (= '(1 2 3) '(1 2 3))
Traceback (most recent call last):
  0     (= (quote (1 2 3)) (quote (1 2 3)))
Error: operand 0 ((1 2 3)) is not a number
```


# 17 Exceptions
未处理的异常会导致 `Python` 停止执行，并打印一个堆栈跟踪。

特点：
- 异常是对象
- 异常处理往往很慢

## Raise Excepitons
`assert`
  - `assert <expression>, <string>`
  - 导致 `AssertionError`

`raise <expression>`
  - `<expression>`必须为 `BaseException` 的一个子类或一个实例。
  - 异常的构造就像其他对象一样。例如，`TypeError('Bad argument!')`

错误类型：
  - `TypeError` -- 一个函数被传递了错误的参数数量/类型。
  - `NameError` -- 没有找到一个名字。
  - `KeyError` -- 在字典中没有找到一个键。
  - `RecursionError` -- 递归调用太多。
 
## handle exceptions

```python
try:
 <try suite>
except <exception class> as <name>:
 <except suite>
...
```
执行步骤
1. 先执行 `<try suite>`
2. 如果在执行 `<try suite>` 的过程中引起异常
3. 并且如果异常的类继承自`<exception class>`，
4. 那么执行`<except suite>`，将`<name>`绑定到异常上

```python
>>> try:
        x = 1/0
    except ZeroDivisionError as e:
        print('handling a', type(e))
        x = 0

handling a <class 'ZeroDivisionError'>
>>> x
0
```
多个 `handle` 时，执行最近的

## 经典用例：
`reduce(function, iterable, initial)`  
如果 `function` 是 `division`, 需要处理除以 0 的情况
  
 #18  Calculator
Parsing (解析)  
`Text -> Lexical analysis (词法分析) -> Token (标记) -> Syntactic analysis (句法分析) -> Expression`

`scheme` 可用递归进行句法分析
- `Base case`：符号和数字
- `Recursive call`：`scheme_read`子表达式，并将其组合起来
- `Syntactic analysis `：确定了一个表达的层次结构，可以是嵌套
- 每次调用`scheme_read`都会消耗一个表达式的输入标记。
  
## Scheme syntax calculator
计算器表达式的值是递归定义的。  
`Primitive`是一个数字对其自身进行评估。  
`Call`是调用表达式对其参数值进行运算。
- `+`: 参数值之和
- `*`: 各项参数的乘积
- `-`: 如果只有一个参数，取负。如果有多个参数，则从第一个参数中减去其余参数。
- `/`: 如果有一个参数，就把它反过来。如果超过一个参数，则从第一个参数中除以其余部分。
    
## eval function

`eval`函数计算一个表达式的值，它总是一个数字。   
其是一个通用函数，根据表达式的类型（`primitive` 或 `call`）进行分配。
  
```python
def calc_eval(exp):
    if type(exp) in (int, float): # A number evaluates to itself
        return exp

    # A call expression evaluates to its argument values combined by an operator 
    # (调用表达式对其参数值进行评估，由一个运算符组合而成。)
    elif isinstance(exp, Pair):  

        # calc_eval: Recursive call returns a number for each operand 
        # (递归调用为每个操作数返回一个数字。)
        arguments = exp.second.map(calc_eval)  

        # exp.first: '+', '-', '*', '/'
        # arguments: A Scheme list of numbers
        return calc_apply(exp.first, arguments)

    else:
        raise TypeError

def calc_apply(operator, args):
    if operator == '+':
        return reduce(add, args, 0)
    elif operator == '-':
        ...
    elif operator == '*':
        ...
    elif operator == '/':
        ...
    else:
        raise TypeError
```

## 交互式解释器
交互式解释器是一个**读取 —— 执行 —— 打印**的循环。
1. 打印一个提示
2. 读取用户输入的文本
3. 将输入的文本解析为一个表达式
4. 评价这个表达式
5. 如果发生任何错误，请报告这些错误
6. 否则打印表达式的值，并重复

```python
def read_print_loop():
    """Run a read-print loop for Scheme expressions."""
    while True:
        try:
            src = buffer_input()
            while src.more_on_line:
                expression = scheme_read(src)
                print(repr(expression))
        except (SyntaxError, ValueError) as err:  # ValueError: 2.3.4; SyntaxError: 额外的‘)’
            print(type(err).__name__ + ':', err)
        except (KeyboardInterrupt, EOFError):  # <Control>-D, etc. 真正的停止方式
            return
```


# 19 Interpreters
## The Structure of an Interpreter
有两个 `function`：
- `Eval` 求表达式的值(Evaluate a Calculator expression.)
- `Apply` 当 `eval` 找到一个表达式,用 `apply` 来求出其具体的值(Apply the named operator to a list of args.)

### `Eval`
  - `Base cases`
    - 原始值（数字）
    - 查询与符号绑定的值 
  - `Recursive calls`
    - `Eval(operator(操作符), operands(操作数))` of call expressions
    - `Apply(procedure, arguments)`
    - `Eval(sub-expressions)` of special forms

### `Apply`
  - `Base cases`
    - Built-in primitive procedures
  - `Recursive calls`
    - `Eval(body)` (用户定义程序)

## Special Forms
`scheme_eval` 函数根据表达式选择行为
- `symbols` 在当前环境中绑定为 `values`
- `self-evaluating` 表达式作为值返回
- 所有其他合法表达式都以 `Scheme lists` 的形式表示，称为 `combinations`

### Logical Special Forms
- `if`
- `and` `or`
- `cond`

## Quotation
- `quote`特殊形式 `evaluates` 被引用的表达式(which is not evaluated)
- `expression` 本身就是被求出的值

## Lambda expressions
Lambda 表达式对用户定义的过程进行求值

```python
class LambdaProcedure:
    def __init__(self, formals, body, env):
        self.formals = formals  # A scheme list of symbols
        self.body = body        # A scheme list of expressions
        self.env = env          # A Frame instance
```

## Frame
- 一个 `frame` 通过有一个`parent frame`来代表一个环境
- `Frames` 是 `Python` 实例，有方法 `lookup` 和 `define` 。

### Define
- `Define` 将一个 `symbol` 绑定到 `current environment` 的 `first frame` 中的值。
- `Procedure definition`(过程定义)是带有 lambda 表达式的定义的简写。  
`(define (<name> <formal parameters>) <body>)`  
`(define <name> (lambda (<formal parameters>) <body>))`

# 20 Declarative Programming (声明式编程)
声明式语言
  - "程序"是对预期结果的描述
  - `interpreter` 想出如何产生结果

命令式语言
  -  "程序"是对计算过程的描述
  - `interpreter` 执行 `execution/evaluation` 规则
  
## 用 Python 描述 SQL
```python
output_table = []
for row in FROM(*input_tables):
    if WHERE(row):
        output_table += [SELECT(row)]
if ORDER_BY:
    output_table = ORDER_BY(output_table)
if LIMIT:
    output_table = output_table[:LIMIT]
```
注意，`ORDER BY`和`LIMIT`子句仅在确定了输出表中的所有行之后才应用。

## SQL
SQL(Structured Query Language 结构化查询语言)
  - `select` 语句可以从头开始创建一个新表，也可以通过投射表来创建一个新表
  - `create table` 语句给表起了一个全局名称

还有很多其他语句：`analyze, delete, explain, insert, replace, update`等。

大多数重要的操作是在 `select` 语句中的

### select
`select` 语句总是包含一个以逗号分隔的列描述列表。  
列的描述是一个表达式，后面可选择 `as` 和列名

`select [expression] as [name]`
- 两个选择语句的 `union` 是一个包含它们两个结果的行的表。

```SQLite
select "delano" as parent, "herbert" as child union
select "abraham"         , "barack"           union
select "abraham"         , "clinton"          union
select "fillmore"        , "abraham"          union
select "fillmore"        , "delano"           union
select "fillmore"        , "grover"           union
select "eisenhower"      , "fillmore";
```

- select 的结果只呈现给用户，不储存
- `create table` 语句给结果一个名字
- `create table [name] as [select statement];`
```SQLite
create table parents as
    select "delano" as parent, "herbert" as child union
    select "abraham"         , "barack"           union
    select "abraham"         , "clinton"          union
    select "fillmore"        , "abraham"          union
    select "fillmore"        , "delano"           union
    select "fillmore"        , "grover"           union
    select "eisenhower"      , "fillmore";
```

## Projecting Tables(投影表)
`select` 呈现现有表格，一个 `select` 语句可以使用 `from` 子句指定一个输入表
  - ` select [columns] from [table] where [condition] order by [order]`
  - `[column]` 指的是 `select [expression] as [name], [expression] as [name], ... ;` 中的 `[name]`
  - `select child from parents where parent = "abraham";`
  - `select parent from parents where parent > child;`

可以使用 `where` 子句选择输入表中的行的子集。

可以使用一个 `order by` 子句来声明剩余行的排序。

列的描述决定了每条输入行如何投影到结果行。

## Arithmetic(算术)
在 `select` 表达式中， `column` 名是 `row` 值。  
算术表达式可以将 `row` 值和常量相结合

  
```SQLite
create table lift as
 select 101 as chair, 2 as single, 2 as couple union
 select 102 , 0 , 3 union
 select 103 , 4 , 1;

select chair, single + 2 * couple as total from lift;
```

|char|total|
|:---:|:---:|
|101|6|
|102|6|
|103|6|


# 21 Table
## Joining Tables
通过将多张表连接成一张表来组合数据，这是数据库系统中的基本操作。

我们在本类中只重点介绍一种方法（内部连接）。

如果连接两个表，左边的表有 m 条记录，右边的表有 n 条记录，那么连接后的表将有 mn 条记录

  
```SQLite
CREATE TABLE dogs AS
 SELECT "abraham" AS name, "long" AS fur UNION
 SELECT "barack"         , "short"       UNION
 SELECT "clinton"        , "long"        UNION
 SELECT "delano"         , "long"        UNION
 SELECT "eisenhower"     , "short"       UNION
 SELECT "fillmore"       , "curly"       UNION
 SELECT "grover"         , "short"       UNION
 SELECT "herbert"        , "curly";

CREATE TABLE parents AS
 SELECT "abraham" AS parent, "barack" AS child UNION
 SELECT "abraham"          , "clinton"         UNION
 ...;
```
两张表 A 和 B 用逗号连接，得出 A 和 B 中某行 的所有组合。
  
Select the parents of curly-furred dogs

```SQLite
SELECT parent FROM parents, dog WHERE child = name AND fur = "curly";
```

## 用 Python 描述 SQL join
```python
def FROM(table_1, table_2):
    for row_1 in table1:
        for row_2 in table2:
            yield row_1 + row_2
```
联接就像创建嵌套`for`循环。
- 每一行`table_1`和每一行`table_2`，联接将对行的每种可能的组合进行迭代，并将其视为输入表

## Aliases and Dot Expressions(别名和点表达式)
- 两个表可能共用一个列名，所以我们需要一种方法来区分表的列名。
- 点表达式和别名 可以消除列值的歧义。
- SQL允许我们在 `FROM` 子句中使用关键字 `AS` 给表赋予别名，
- 使用点表达式来引用特定表内的列。  
`SELECT [columns] FROM [table] WHERE [condition] ORDER BY [order];`
- `[table]` 是一个以逗号分隔的表名列表，可选择别名
- 选择所有相同 parent 的 child
```SQLite
SELECT a.child AS first, b.child AS second
 FROM parents AS a, parents AS b  -- 这是一个别名
 WHERE a.parent = b.parent AND a.child < b.child;
```

## Numerical Expressions
表达式可以包含函数调用和算术运算符
- `SELECT [columns] FROM [table] WHERE [expression] ORDER BY [expression];`
- Combine values: ` +, -, *, /, %, and, or `
- Transform values: ` abs, round, not, -`
- Compare values:  ` <, <=, >, >=, <>, !=, =` 
  - `<>, !=` ANSI标准中是用`<>`(所以建议用`<>`)
  - ` = ` 因为 SQL 没有赋值，所有用 `=` 比较

## String Expressions
```SQLite
sqlite> SELECT "hello," || " world"; -- 字符串值可以组合成较长的字符(使用 || )。
hello, world

sqlite> CREATE TABLE phrase AS SELECT "hello, world" AS s;
sqlite> SELECT substr(s, 4, 2) || substr(s, instr(s, " ")+1, 1) FROM phrase; -- SQL中内置了基本的字符串操作，但与Python不同。
low

sqlite> CREATE TABLE lists AS SELECT "one" AS car, "two,three,four" AS cdr;
sqlite> SELECT substr(cdr, 1, instr(cdr, ",")-1) AS cadr FROM lists; -- 字符串可以用来表示结构化的值，但这样做不是个好主意
two

```

# 22 Aggregation(聚合)
我们也可以使用相同的 `SELECT` 语句对多条记录进行聚合操作

我们可以使用 `MAX、MIN、COUNT和SUM` 函数从我们的初始表中获取更多的信息。
  - `COUNT` 计算 `row` 的个数
    - `count(distinct name)` -> 计算 `name` 的种类 
  - 聚合函数也会取 `table` 中的某一个 `row`
    - 但结果有多个 `row` 对应，则会取其中任意一行
    - `avg` 函数有可能取不出 `row`
    
```SQLite
sqlite> SELECT name, MAX(salary) FROM records;
Oliver Warbucks|150000
```

使用特殊 `COUNT(*)` 语法，我们可以统计表中的行数，以查看该公司的员工人数。
```SQLite
sqlite> SELECT COUNT(*) from RECORDS;
9
```

## Group
到目前位置的所有 `select` 都只有一个 `group`,`the only one final group`  
`table` 中的 `rows` 可以被 `grouped`, `aggregation` 在各个组起作用  

`select [columns] from [table] group by [expression] having [expression];`
- `group by [expression]`
  - `group` 的数量是 `group` 的 `[expression]` 中独特值的个数(数的种类)
- 可以通过 `HAVING` 子句过滤被聚合的组的集合
  - 与 `WHERE` 子句不同的是，`WHERE` 子句过滤掉的是行，`HAVING` 子句过滤掉的是整个组。

```SQLite
sqlite> SELECT title FROM records GROUP BY title HAVING count(*) > 1; -- 过滤组数少于 1 的组
Programmer
```

## 用 Python 描述 SQL Aggregation
我们可以使用 `GROUP BY` 和 `HAVING` 将行划分为组，并仅选择组的子集。
```python
output_table = []                                 # 输出多个 table
for input_group in GROUP_BY(FROM(*input_tables)): # GROUP_BY 划分了子集
    output_group = []
    for row in input_group:
        if WHERE(row):
            output_group += [row]                 # 每个子集内部的 row 用 WHERE 筛选
    if HAVING(output_group):                      # group 本身用 HAVING 筛选
        output_table += [SELECT(output_group)]

if ORDER_BY:
    output_table = ORDER_BY(output_table)
if LIMIT:
    output_table = output_table[:LIMIT]
```

# 23 Final Examples

两个常用的递归模板
  - return the accumulation(累计) so far，用递归函数的 return 值来跟踪你想要的返回值
  - 初始化某个 accumulation 为 0 或 空列表， 然后按需要填充

```python
def bigs(t):
    """Return the number of nodes in t that are larger than all their ancestors.
    >>> a = Tree(1, [Tree(4, [Tree(4), Tree(5)]), Tree(3, [Tree(0, [Tree(2)])])])
    >>> bigs(a)
    4
    """
    def f(a, x):
        if a.label > x:
            return 1 + sum([f(b, a.label) for b in a.branches])
        else: 
            return sum([f(b, x) for b in a.branches]) 
    return f(t,t.label - 1)
```

```python
def bigs(t):
    """Return the number of nodes in t that are larger than all their ancestors."""
    n = 0
    def f(a, x):
        nonlocal n
        if a.label > x:
            n += 1
        for b in a.branches:
            f(b, max(a.label, x))
    f(t,t.label - 1)
    return n
```

## 设计 functions 
[How to Design Programs, Second Edition](https://htdp.org/2018-01-06/Book/)
- 从问题分析到数据定义 (From Problem Analysis to Data Definitions)
  - 确定必须表示的信息，以及如何在所选语言中表示该信息
  - 制定数据定义，并用例子呈现他们
- Signature(函数签名), Purpose Statement(目标陈述), Header(函数存根)
  - Signature: 描述了函数所需参数和产生的结果
  - Purpose Statement: 描述了函数的作用：这个函数计算了什么
  - Header: 将函数的参数替换为具体的合法数据，并提供了输出结果的具体示例
- Functional Examples
  - 使用样例来呈现函数的目的
- Function Template
  - 将数据定义转化为函数的概要，类似于实现伪代码
  - 写模板，比如自己想要使用是么样子的函数，函数中又包括了什么函数，用到了什么结构语句
- Function Definition
  - 填补函数模板的空白
- Testing
  - 以测试的方式阐述例子，并确保函数全部通过
  - 测试也是对例子的补充，因为它们可以帮助别人阅读和理解


# 24 Databases
## Create Table
![CreateTable](../pics/CreateTable.png)

我们忽略一些细节后的精简版本

![CreateTableDemo](../pics/CreateTableDemo.png)

例子：
```SQLite
CREATE TABLE numbers (n, note);
CREATE TABLE numbers (n UNIQUE, note);
CREATE TABLE numbers (n, note DEFAULT "No comment");
```

## Drop Table
![drop table](../pics/DropTable.png)

使用` if exists `如果不存在会 报错

## Modifying Tables
### Insert
![InsertRows](../pics/InsertTable.png)

### Update
![UpdateRows](../pics/UpdateTable.png)

### Delect
![DeleteRows](../pics/DeleteRows.png)

## Example
```SQLite
sqilte> create table primes(n, prime);
sqilte> drop table if exists primes;
sqilte> select * from primes;
Error: no such table: primes
sqilte> create table primes(n UNIQUE, prime DEFAULT 1); -- SQLite 中没有 boolean
sqilte> select * from primes;
sqilte> INSERT INTO primes VALUES (2, 1), (3, 1);
sqilte> select * from primes;
2|1
3|1
sqilte> INSERT INTO primes(n) VALUES (4), (5), (6), (7);
sqilte> select * from primes;
2|1
3|1
4|1
5|1
6|1
7|1
sqilte> INSERT INTO primes(n) SELECT n+6 FROM primes;
sqilte> select * from primes;
2|1
3|1
4|1
5|1
6|1
7|1
8|1
9|1
10|1
11|1
12|1
13|1
sqilte> UPDATE primes SET prime=0 WHERE n>2 AND n%2=0;
sqilte> UPDATE primes SET prime=0 WHERE n>3 AND n%3=0;
sqilte> UPDATE primes SET prime=0 WHERE n>5 AND n%5=0;
sqilte> select * from primes;
2|1
3|1
4|0
5|1
6|0
7|1
8|0
9|0
10|0
11|1
12|0
13|1
sqilte> DELECT FROM primes HERE prime=0;
sqilte> select * from primes;
2|1
3|1
5|1
7|1
11|1
13|1
```

## Python and SQL

```python
import sqlite3

# sqlite3 有一个 Connection 类，连接 database file
db = sqlite3.Connection("n.db")
db.execte("CREATE TABLE nums AS SELECT 2 UNION SELECT 3;")
db.execte("INSERT INTO nums VALUES (4), (5), (6);")
db.execte("INSERT INTO nums VALUES (?), (?), (?);", range(7,10))
print(db.execte("SELECT * FROM nums;").fetchall())  # 返回一个 cursor object,有个 fetchall 方法，把结果以元组形式表述出来
# [(2,),(3,),(4,),(5,),(6,),(7,),(8,),(9,)]

# 当你修改一个 table,确保代表 database 的文件要保存了所有的更改，需要多用一个方法
db.commit()
```

## SQL 注入攻击
```python
name = "Robert'); DROP TABLE Students; --"
cmd = "INSERT INTO Students VALUES ('" + name + "');"
db.executescript(cmd)
```
```SQLite
INSERT INTO Students VALUES ('Robert'); DROP TABLE Students; -- ');
```
解决方法：
```python
name = "Robert'); DROP TABLE Students; --"
db.execute("INSERT INTO Students VALUES (?);", [name])  # 这种方式把字符串正确地转义了
```
```SQLite
INSERT INTO Students VALUES ('Robert''); DROP TABLE Students; --');
```

## Database Connections
多个程序可能同时连接到同一数据库，可能从同一 table 上插入值或者读取值。
- 数据库可以处理同一数据库进行多个连接、多个客户端改变同一个 table 的情况


例子: 21点游戏， `python` 程序连接数据库来执行游戏， `eval-print-loop` 也连接数据库来看牌

```python
# Blackjack

points = {'A': 1, 'J': 10, 'Q': 10, 'K':10}
points.update({n: n for n in range(2, 11)})  # 每个牌所对应的分数

def hand_score(hand):
    """Total score for a hand."""
    total = sum([points[card] for card in hand])  # 手上所有牌的大小
    if total <= 11 and 'A' in hand:               # 'A' 有 1 和 11 两种情况
        return total + 10
    return total

db = sqlite3.Connection('cards.db')              # 连接数据库
sql = db.execute
sql('DROP TABLE IF EXISTS cards')
sql('CREATE TABLE cards(card, place);')          # 创建 cards 表，里面有 card, place(对应who) 两列

def play(card, place):
    """Play a card so that the player can see it."""
    sql('INSERT INTO cards VALUES (?, ?)', (card, place))  # 插入 card, place 到 cards
    db.commit()

def score(who):
    """Compute the hand score for the player or dealer."""
    cards = sql('SELECT * from cards where place = ?;', [who])    # who 所包含的所有 rows
    return hand_score([card for card, place in cards.fetchall()])

def bust(who):
    """Check if the player or dealer went bust."""
    return score(who) > 21

player, dealer = "Player", "Dealer"

def play_hand(deck):
    """Play a hand of Blackjack."""  # 抽牌，dealer 一张牌翻盖
    play(deck.pop(), player)         # desk 是一个list, 列出所有 points 的 keys 数量 * 4
    play(deck.pop(), dealer)
    play(deck.pop(), player)
    hidden = deck.pop()

    while 'y' in input("Hit? ").lower():  # 按 'y' 抽牌
        play(deck.pop(), player)
        if bust(player):                  # 超过 21 点？
            print(player, "went bust!")
            return

    play(hidden, dealer)

    while score(dealer) < 17:            # dealer 逻辑，只要小于 17 分就抽牌
        play(deck.pop(), dealer)
        if bust(dealer):
            print(dealer, "went bust!")
            return

    print(player, score(player), "and", dealer, score(dealer))

deck = list(points.keys()) * 4
random.shuffle(deck)                    # 洗牌 （random 列表）
while len(deck) > 10:                   # 开始打牌
    print('\nDealing...')
    play_hand(deck)
    sql('UPDATE cards SET place="Discard";')
```

# 25 Distributed Data 
## Computer systems
系统研究通过定义和实现抽象(abstractions)来实现应用开发。(隐藏复杂性，但保留灵活性)
- **操作系统**为不可靠、不一致的硬件提供了**一个稳定、一致的接口**。
- **网络**为不断进化的通信基础设施提供**强大的数据传输接口**
- **数据库**为复杂的软件提供了**一个声明式的接口**，可以有效地存储和检索信息。
- **分布式系统**为多台机器的集群**提供统一的接口**——有效系统的统一属性。

## Example: The Unix Operating System
Unix操作系统（及变种如Linux, Mac OS 10）的基本特征。
- 可移植性(Portability)。在不同的硬件上使用相同的操作系统
- 多任务(Multi-Tasking)。在一台机器上同时运行多个进程
- 纯文本(Plain Text)。以文本格式存储和共享数据
- 模块化(Modularity)。小型工具通过管道(pipes)灵活组成

`standard input(text input) -> process -> standard output(text output) || standard error`
- 类Unix操作系统中的标准流(standard streams)类似于Python iterators。

## python programs in a Unix environment
- `sys.stdin` 和 `sys.stdout` 的值提供了对Unix标准流文件的访问
- 一个Python文件有一个支持`iteration, read, write`方法的接口。
- 使用这些 "`files`"可以利用操作系统的文本处理抽象
- `input` 和 `print` 功能还可以从`standard input`读取和写入`standard output`。 

  

```python
# ex.py
import sys
for line in sys.stdin:
    sys.stdout.write(' '.join(line)) # 每个字都有空格
```

```shell script
$ python ex.py
Here it is
H e r e  i t  i s
Great
G r e a t

$ ls *.pdf | python ex.py # 所有 pdf 的 filename 都有空格
```



## Big Data

- 将 unix tools 和 写的程序结合可以给数据处理提供绝佳的环境
- 如果多个用户需要同时访问和编辑数据，可以使用数据库
- 如果想要处理的数据大小超过了一台机器的范围，就需要使用分布式数据处理

### 解决方案： Apache Spark
- Apache Spark是一个数据处理系统，它为大型数据提供了一个简单的接口。
- 弹性分布式数据集 Resilient Distributed Dataset（RDD）是 a collection of values or key-value pairs
- 支持常见的UNIX操作：`sort, distinct (uniq in UNIX), count, pipe`
- 支持常见的序列操作：`map, filter, reduce`
- 支持常见的数据库操作：`join, union, intersection`  
- 所有这些操作都可以在跨机器分区的RDD上执行。

### Apache Spark 的执行模式
  - 集中定义处理，但远程执行操作
  - 弹性分布式数据集(RDD)以分区的方式分布到工作节点上。
  - driver program 定义了RDD上的转换和操作。
  - 集群管理器(cluster manager)将任务分配给各个工作节点，让他们执行任务
  - 工作节点之间进行计算和相互交流数值。
  - 最终的结果会反馈给driver program。
![ApacheSparkExecutionModel](../pics/ApacheSparkExecutionModel.png)



### Apache Spark 的接口

- SparkContext提供了对集群管理器的访问。
```python
>>> sc	
<pyspark.context.SparkContext	...>
```
- RDD可以从文本文件来构建
```python
>>>	x = sc.textFile('shakespeare.txt')	
```
- The `sortBy` transformation(转变) and `take` 是 RDD 上的方法
```python
>>> x.sortBy(lambda s: s, False).take(2)  # 降序，给我两个元素	
['you shall ...', 'yet , a ...']
```

### Apache Spark 提供了什么
- 容错。机器或硬盘可能会崩溃
  - 群集管理器自动重新运行失败的任务
- 速度：有些机器可能会因为超载而变慢。
  - 群集管理器可以运行一个任务的多个副本，并保留先完成的那个任务的结果。
- 网络定位。数据传输很昂贵
  - 群集管理器试图在持有这些数据的机器上安排计算。 
- 监控。我的工作会在晚饭前完成吗？
  - 群集管理器提供了一个基于Web的界面描述工作
  
### MapReduce
- 一个重要的早期分布式处理系统
- 通用的应用结构，恰好可以捕获许多常见的数据处理任务。
  - 第1步(map)：输入集合中的每个元素都会产生 0 或 more 键值对 。
  - 第2步(shuffle)：将所有共享一个 key 的 键值对 聚合在一起。
  - 第3步(reduce)：将一个键的值作为一个序列(sequence)进行处理
- 早期应用：索引网页、训练语言模型和计算网页排名。

### MapReduce Evaluation Model
  - 映射阶段。将映射函数应用于所有输入，发出中间 键值对
    - 映射器为每个输入产生零或多个键值对。
    ![KeyValuePairs](../pics/KeyValuePairs.png)
  - Reduce phase: 对于每一个中间键，使用一个还原函数来累积与该键相关的所有值。
    - 所有具有相同 key 的 键值对 一起处理
    - 还原器产生0个或更多的值，每个值都与该中间键相关联
    ![ReducePhase](../pics/ReducePhase.png)
![MapReduceApplications](../pics/MapReduceApplications.png)


```python
def vowels(line):
    """Yield (vowel, count) pairs."""
    for v in 'aeiou':
        if v in line:
            yield (v, line.count(v))

>>> list(vowels('hello world'))
[('e', 1),('o', 2)]

>>> x = sc.textFile('shakespeare.txt')	
>>> data.flatMap(vowels).take(7)
[('a', 3), ('i', 3),('o', 4), ('u', 3), ('a', 5), ('e', 1), ('i', 2)]

>>> from poerator import add
>>> data.flatMap(vowels).reduceByKey(add).collect()
[('i', 189626), ('a', 233881), ('u', 110820),('o', 272697), ('e', 387705)]
```
  
 # 26 Tail Recursion
如果一个函数中所有递归形式的调用都出现在函数的末尾，我们称这个递归函数是尾递归的
1. 在尾部调用的是函数自身 (Self-called)；
2. 可通过优化，使得计算仅占用常量栈空间 (Stack Space)。
   - 跳过所有不需要的 frame 以实现常数空间
   - 如果 tail call 没有额外计算，则可以是 tail recursion
  
## tail call optimization
- tail call 的返回值是返回当前程序调用的 return value
- 因此，tail call 不应该增加 env 的数量


```javascript
// 非尾递归
function fact(n) {
    if (n <= 0) {
        return 1;
    } else {
        return n * fact(n - 1);
    }
}
/*
6 * fact(5)
6 * (5 * fact(4))
6 * (5 * (4 * fact(3))))
// two thousand years later...
6 * (5 * (4 * (3 * (2 * (1 * 1)))))) // <= 最终的展开
*/


// 尾递归
function fact(n, r) {
    if (n <= 0) {
        return 1 * r;
    } else {
        return fact(n - 1, r * n);
    }
}
/*
fact(6, 1) // 1 是 fact(0) 的值，我们需要手动写一下
fact(5, 6)
fact(4, 30)
fact(3, 120)
fact(2, 360)
fact(1, 720)
720 // <= 最终的结果
*/
```


# Fix mistakes
```python
def if_function(condition, true_result, false_result):
    """Return true_result if condition is a true value, and false_result otherwise.
    """
    if condition:
        return true_result
    else:
        return false_result

def with_if_function():
    return if_function(c(), t(), f())

def c():
    return False

def t():
    print(5)

def f():
    print(6)

>>> result = with_if_function()
# 5
# 6
>>> print(result)   
# None
```

- if_function 函数内的所有子函数会在 if_function 调用前计算出来(expression tree)

正确调用：

```python 
def with_if_statement():
    """
    >>> result = with_if_statement()
    6
    >>> print(result)
    None
    """
    if c():
        return t()
    else:
        return f()

with_if_statement()        
```

# lab00
- ls：列出当前目录中的所有文件
- cd <path to directory>：更改为指定目录
- mkdir <directory name>：使用给定名称创建新目录
- mv <source path> <destination path>：将给定源处的文件移动到给定目标

## python
- 浮点除法（/）：将第一个数字除以第二个数字，即使数字均分，也算出带小数点的数字
- 底数除法（//）：将第一个数字除以第二个数字，然后**删去小数**，得出整数
```python
3/3 # 1.0
99//10 # 9
7 % 4 # 余数 (remainder of 7 // 4)
3
```

## doctest 测试
```python

def twenty_twenty():
    """Come up with the most creative expression that evaluates to 2020,
    using only numbers and the +, *, and - operators.

    >>> twenty_twenty()
    2020
    """
    return 2020
```    
通过```python -m doctest [filename]```调用，如果想看到测试通过的信息可以用```python -m -v doctest [filename]```

# lab 01
```bash
python3 -i  // 在交互式运行Python文件时对其进行编辑
python3 -m doctest  // 文档测试
```

```python
>>> positive = 28
>>> while positive: # If this loops forever, just type Infinite Loop
...    print("positive?")
...    positive -= 3
```
- 无限循环
  - 只有 0、None、False 为假值
  
- x or y
  - x 为真： x
  - x 为假： y
- x and y
  - x 为真： y
  - x 为假： x
  
## 调试技巧
- [Debugging](https://cs61a.org/articles/debugging.html)
- 使用print语句
  - 短期调试
- 长期调试
  - 使用全局 debug 变量
  ```python
  debug = True
  
  def foo(n):
  i = 0
  while i < n:
      i += func(i)
      if debug:
          print('DEBUG: i is', i)
  ```
  - 只要我们想进行一些调试，就可以将全局debug变量设置 为True
  - （这样的变量称为“标志”）
- 交互式调试
  - ```python -i file.py```
- 使用assert语句
  - ```assert isinstance(x, int), "The input to double(x) must be an integer"```
  - assert语句的一个主要优点是它们不仅仅是调试工具，您可以将它们永久保留在代码中
  - 代码崩溃通常比产生不正确的结果要好
  - 并且在代码中声明内容会使代码更容易崩溃
- PythonTutor调试
  - ```python ok -q <question name> --trace```
  - [pythontutor](http://pythontutor.com/composingprograms.html#mode=edit)

# 树型递归问题
考虑下面这个问题：如果给你半美元、四分之一美元、十美分、五美分和一美分，一美元有多少种找零的方式？更通常来说，我们能不能编写一个函数，使用一系列货币的面额，计算有多少种方式为给定的金额总数找零？

## 分析
- 我们可以递归将给定金额的找零问题，归约为使用更少种类的硬币为更小的金额找零的问题。
- 使用n种硬币找零的方式为：
  - `without`：使用所有除了第一种之外的硬币为`a`找零的方式
  - `within`：使用`n`种硬币为更小的金额`a - d`找零的方式，其中d是第一种硬币的面额。
- 终止条件
  - 如果a正好是零，那么有一种找零方式。
  - 如果a小于零，那么有零种找零方式。
  - 如果n小于零，那么有零种找零方式。
  
    
```python
>>> def count_change(a, kinds=(50, 25, 10, 5, 1)):
        """Return the number of ways to change amount a using coin kinds."""
        if a == 0:
            return 1
        if a < 0 or len(kinds) == 0:
            return 0

        d = kinds[0]
        return count_change(a, kinds[1:]) + count_change(a - d, kinds)

>>> count_change(100)
292
```
