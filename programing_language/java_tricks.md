
[适合 Java 新手的开源项目集合](https://zhuanlan.zhihu.com/p/312484956)

[狂神说java课程](https://www.kuangstudy.com/course)

[狂神说java视频教程](https://www.bilibili.com/video/BV1og4y1q7M4)

[java try-with-resource语句使用](https://www.jianshu.com/p/258c5ce1a2bd)

[阿里P8 java面试](https://github.com/Snailclimb/JavaGuide)


**1  @Override：**

是伪代码,表示重写(当然不写也可以)，不过写上有如下好处:

1、可以当注释用,方便阅读；

2、编译器可以给你验证@Override下面的方法名是否是你父类中所有的，如果没有则报错

**2  equals 和 == 的区别：**

equals 是方法，而 == 是操作符；

对于基本类型的变量来说（如 short、 int、 long、 float、 double），只能使用 == ，因为这些基本类型的变量没有 equals 方法。对于基本类型变量的比较，使用 == 比较， 一般比较的是它们的值。

对于引用类型的变量来说（例如 String 类）才有 equals 方法，因为 String 继承了 Object 类， equals 是 Object 类的通用方法。对于该类型对象的比较，默认情况下，也就是没有复写 Object 类的 equals 方法，使用 == 和 equals 比较是一样效果的，都是比较的是它们在内存中的存放地址。但是对于某些类来说，为了满足自身业务需求，可能存在equals 方法被复写的情况，这时使用 equals 方法比较需要看具体的情况，例如 String 类，使用 equals 方法会比较它们的值；

https://www.jianshu.com/p/9cbed9f33a4d
