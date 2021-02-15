## 第一部分 运用领域模型

## 第1章 消化知识

1.1 有效建模的要素

1.2 知识消化

1.3 持续学习

1.4 知识丰富的设计

1.5 深层模型

## 第2章 交流与语言的使用

2.1 模式：UBIQUITOUS LANGUAGE

2.2 “大声地”建模

2.3 一个团队，一种语言

2.4 文档和图

2.4.1 书面设计文档

2.4.2 依赖可执行代码的情况

2.5 解释性模型

## 第3章 绑定模型和实现

3.1 模式：MODEL-DRIVEN DESIGN

3.2 建模范式和工具支持

3.3 揭示主旨：为什么模型对用户至关重要

3.4 模式：HANDS-ON MODELER



## 第二部分 模型驱动设计的构造块

## 第4章 分离领域

4.1 模式：LAYERED ARCHITECTURE

4.1.1 将各层关联起来

4.1.2 架构框架

4.2 领域层是模型的精髓

4.3 模式：THE SMART UI“反模式”

4.4 其他分离方式

## 第5章 软件中所表示的模型

5.1 关联

5.2 模式：ENTITY（又称为REFERENCE OBJECT）

5.2.1 ENTITY建模

5.2.2 设计标识操作

5.3 模式：VALUE OBJECT

5.3.1 设计VALUE OBJECT

5.3.2 设计包含VALUE OBJECT的关联

5.4 模式：SERVICE

5.4.1 SERVICE与孤立的领域层

5.4.2 粒度

5.4.3 对SERVICE的访问

5.5 模式：ＭODULE（也称为PACKAGE）

5.5.1 敏捷的MODULE

5.5.2 通过基础设施打包时存在的隐患

5.6 建模范式

5.6.1 对象范式流行的原因

5.6.2 对象世界中的非对象

5.6.3 在混合范式中坚持使用MODEL-DRIVEN DESIGN

## 第6章 领域对象的生命周期

6.1 模式：AGGREGATE

6.2 模式：FACTORY

6.2.1 选择FACTORY及其应用位置

6.2.2 有些情况下只需使用构造函数

6.2.3 接口的设计

6.2.4 固定规则的相关逻辑应放置在哪里

6.2.5 ENTITY FACTORY与VALUE OBJECT FACTORY

6.2.6 重建已存储的对象

6.3 模式：REPOSITORY

6.3.1 REPOSITORY的查询

6.3.2 客户代码可以忽略REPOSITORY的实现，但开发人员不能忽略

6.3.3 REPOSITORY的实现

6.3.4 在框架内工作

6.3.5 REPOSITORY与FACTORY的关系

6.4 为关系数据库设计对象

## 第7章 使用语言：一个扩展的示例

7.1 货物运输系统简介

7.2 隔离领域：引入应用层

7.3 将ENTITY和VALUE OBJECT区别开

7.4 设计运输领域中的关联

7.5 AGGREGATE边界

7.6 选择REPOSITORY

7.7 场景走查

7.7.1 应用程序特性举例：更改Cargo的目的地

7.7.2 应用程序特性举例：重复业务

7.8 对象的创建

7.8.1 Cargo的FACTORY和构造函数

7.8.2 添加Handling Event

7.9 停一下，重构：Cargo AGGREGATE 的另一种设计

7.10 运输模型中的ＭODULE

7.11 引入新特性：配额检查

7.11.1 连接两个系统

7.11.2 进一步完善模型：划分业务

7.11.3 性能优化

7.12 小结



## 第三部分 通过重构来加深理解

## 第8章 突破

8.1 一个关于突破的故事

8.1.1 华而不实的模型

8.1.2 突破

8.1.3 深层模型

8.1.4 冷静决策

8.1.5 成果

8.2 机遇

8.3 关注根本

8.4 后记：越来越多的新理解

## 第9章 将隐式概念转变为显式概念

9.1 概念挖掘

9.1.1 倾听语言

9.1.2 检查不足之处

9.1.3 思考矛盾之处

9.1.4 查阅书籍

9.1.5 尝试，再尝试

9.2 如何为那些不太明显的概念建模

9.2.1 显式的约束

9.2.2 将过程建模为领域对象

9.2.3 模式：SPECIFICATION

9.2.4 SPECIFICATION的应用和实现

## 第10章 柔性设计

10.1 模式：INTENTION-REVEALING INTERFACES

10.2 模式：SIDE-EFFECT-FREE FUNCTION

10.3 模式：ASSERTION

10.4 模式：CONCEPTUAL CONTOUR

10.5 模式：STANDALONE CLASS

10.6 模式：CLOSURE OF OPERATION

10.7 声明式设计

10.8 声明式设计风格

10.9 切入问题的角度

10.9.1 分割子领域

10.9.2 尽可能利用已有的形式

## 第11章 应用分析模式

## 第12章 将设计模式应用于模型

12.1 模式：STRATEGY（也称为POLICY）

12.2 模式：COMPOSITE

12.3 为什么没有介绍FLYWEIGHT

## 第13章 通过重构得到深层的理解

13.1 开始重构

13.2 探索团队

13.3 借鉴先前的经验

13.4 针对开发人员的设计

13.5 重构的时机

13.6 危机就是机遇



## 第四部分 战略设计

## 第14章 保持模型的完整性

14.1 模式：BOUNDED CONTEXT

14.2 模式：CONTINUOUS INTEGRATION

14.3 模式：CONTEXT MAP

14.3.1 测试CONTEXT的边界

14.3.2 CONTEXT MAP的组织和文档化

14.4 BOUNDED CONTEXT之间的关系

14.5 模式：SHARED KERNEL

14.6 模式：CUSTOMER/SUPPLIER DEVELOPMENT TEAM

14.7 模式：CONFORMIST

14.8 模式：ANTICORRUPTION LAYER

14.8.1 设计ANTICORRUPTION LAYER的接口

14.8.2 实现ANTICORRUPTION LAYER

14.8.3 一个关于防御的故事

14.9 模式：SEPARATE WAY

14.10 模式：OPEN HOST SERVICE

14.11 模式：PUBLISHED LANGUAGE

14.12 “大象”的统一

14.13 选择你的模型上下文策略

14.13.1 团队决策或*高层决策

14.13.2 置身上下文中

14.13.3 转换边界

14.13.4 接受那些我们无法*改的事物：描述外部系统

14.13.5 与外部系统的关系

14.13.6 设计中的系统

14.13.7 用不同模型满足特殊需要

14.13.8 部署

14.13.9 权衡

14.13.10 当项目正在进行时

14.14 转换

14.14.1 合并CONTEXT：SEPARATE WAY →SHARED KERNEL

14.14.2 合并CONTEXT：SHARED KERNEL→CONTINUOUS INTEGRATION

14.14.3 逐步淘汰遗留系统

14.14.4 OPEN HOST SERVICE→PUBLISHED LANGUAGE

## 第15章 精炼

15.1 模式：CORE DOMAIN

15.1.1 选择核心

15.1.2 工作的分配

15.2 精炼的逐步提升

15.3 模式：GENERIC SUBDOMAIN

15.3.1 通用不等于可重用

15.3.2 项目风险管理

15.4 模式：DOMAIN VISION STATEMENT

15.5 模式：HIGHLIGHTED CORE

15.5.1 精炼文档

15.5.2 标明CORE

15.5.3 把精炼文档作为过程工具

15.6 模式：COHESIVE MECHANISM

15.6.1 GENERIC SUBDOMAIN与COHESIVE MECHANISM的比较

15.6.2 MECHANISM是CORE DOMAIN一部分

15.7 通过精炼得到声明式风格

15.8 模式：SEGREGATED CORE

15.8.1 创建SEGREGATED CORE的代价

15.8.2 不断发展演变的团队决策

15.9 模式：ABSTRACT CORE

15.10 深层模型精炼

15.11 选择重构目标

## 第16章 大型结构

16.1 模式：EVOLVING ORDER

16.2 模式：SYSTEM METAPHOR

16.3 模式：RESPONSIBILITY LAYER

16.4 模式：KNOWLEDGE LEVEL

16.5 模式：PLUGGABLE COMPONENT FRAMEWORK

16.6 结构应该有一种什么样的约束

16.7 通过重构得到适当的结构

16.7.1 小化

16.7.2 沟通和自律

16.7.3 通过重构得到柔性设计

16.7.4 通过精炼可以减轻负担

## 第17章 领域驱动设计的综合运用

17.1 把大型结构与BOUNDED CONTEXT结合起来使用

17.2 将大型结构与精炼结合起来使用

17.3 首先评估

17.4 由谁制定策略

17.4.1 从应用程序开发自动得出的结构

17.4.2 以客户为中心的架构团队

17.5 制定战略设计决策的6个要点

17.5.1 技术框架同样如此

17.5.2 注意总体规划
