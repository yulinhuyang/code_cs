
### 1  A Philosophy of Software Design

**软件设计**

一定要保持写代码，并且不断地进行 reiview 才能提高自己的水平，John Ousterhout 每年至少写 5000 行代码，一共写了 25 - 30 万行代码

设计一个好系统最重要的是拥有的良好的设计观念，设计方法论

**Working Code isn’t enough**

系统不仅仅是 working，还要有好的设计，深入考虑

要在系统设计上投入 10% - 20% 的时间

频繁的做小的系统重构

写新代码，需要小心的设计，良好的文档

一直发现可以改善的地方

不要只修改最少的代码，而是勇于修改，重构，优化

修改之后，修改的部分就像一开始就使用了好的设计一样

**Make continual small investments to improve system design. 通过持续的小投入来改进系统设计**

一次搞定就是瀑布流的思想，逐步改进就是敏捷思想。

**Modules should be deep模块要有深度**

深度其实是对模块封装的度量，模块应该提供尽可能简单的接口和尽可能强大的功能

**Interface should be designed to make the most common usage as simple as possible. 接口设计应当使得最常用的路径越简单越好**

**It's more important for a module to have a simple interface than a simple implementation.相比起实现上的简单，一个模块接口的简单更加重要**

**Seperate general-purpose and special-purpose code. 分离通用的代码和特定需求的代码。**

抽取公共函数或者公共类

**Different layers should have different abstractions. 不同的层次应当有不同的抽象**

软件系统通常有不同的层次组成，每一层都通过和它之上和之下的层的接口来交互。每一层都具有自己不同的抽象

**Pull complexicity downward. 把复杂性放在底层**

**Design it twice**

这个更多是一种态度和工作方式。我们不要拘泥于初始想法，而是要考虑多个方案，从中选择或者整合最好的方案，并且应当在项目开发之后进行复盘。这肯定是提高软件设计质量的好方法，恐怕也是提升任何能力的好方法
