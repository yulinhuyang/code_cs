
### 1 tricks

[让 Python 代码更易维护的七种武器](https://zhuanlan.zhihu.com/p/45671766)

[VsCode写Python时的代码错误提醒和自动格式化！](https://blog.csdn.net/Mrs_chens/article/details/102566018)

[18 Plugins for Writing Python in VS Code](https://switowski.com/blog/18-plugins-for-python-in-vscode)

python tricks: https://www.zhihu.com/question/48755767

[30个Python常用小技巧](https://www.pythontab.com/html/2018/pythonjichu_0917/1351.html)

[程序员常用的10个Python技巧](https://zhuanlan.zhihu.com/p/341547966)

[Python中的高效迭代库itertools，排列组合随便求](https://zhuanlan.zhihu.com/p/349856235)

[【万字长文详解】Python库collections，让你击败99%的Pythoner](https://zhuanlan.zhihu.com/p/343747724)

[Python基础方法详解](https://www.zhihu.com/column/c_1333396693417222144)

[python数据结构与标准库module_alwaysbeta专栏](https://github.com/yongxinz/tech-blog)

[GitHub 上适合新手的开源项目（Python 篇）](https://zhuanlan.zhihu.com/p/280039778?utm_source=qq)

[Python架构模式](https://zhuanlan.zhihu.com/p/257281522)

[apachecn python 翻译集](https://github.com/apachecn/apachecn-python-zh)

[Python项目读取配置的正确姿势](https://zhuanlan.zhihu.com/p/54764686)

[python 类中public,protected,private定义方式](https://blog.csdn.net/c_base_jin/article/details/88778553)

[Python 描述符(Descriptor) 附实例](https://zhuanlan.zhihu.com/p/42485483)

[python 两个list 求交集，并集，差集](https://blog.csdn.net/bitcarmanlee/article/details/51622263)

[深入理解Python中的元类(metaclass)](https://www.cnblogs.com/JetpropelledSnake/p/9094103.html)

[使用concurrent.futures模块并发，实现进程池、线程池](https://www.cnblogs.com/huchong/p/7459324.html)

[代码这样写不止于优雅（Python版）](https://zhuanlan.zhihu.com/p/25648644)

[代码这样写更优雅 (Python 版)](https://zhuanlan.zhihu.com/p/25518608)

[教你阅读Python开源项目代码](https://zhuanlan.zhihu.com/p/22275595)

[Python不能不知的模块](https://zhuanlan.zhihu.com/p/22246193)

[python glob.glob使用](https://blog.csdn.net/mantoureganmian/article/details/47949101)

[Python 嵌套函数](https://blog.csdn.net/liang19890820/article/details/73864242)


### 2 fluent notes 

#### 引用

在Python中，数字、字符或者元组等不可变对象类型都属于值传递，而字典dict或者列表list等可变对象类型属于引用传递。如果要想修改新赋值后原对象不变，则需要用到python的copy模块，即对象拷贝。对象拷贝又包含浅拷贝和深拷贝： 

https://www.zhihu.com/question/20591688

**staticmethod 方法**

@staticmethod

staticmethod用于修饰类中的方法,使其可以在不创建类实例的情况下调用方法

**memoryview**

memoryview() 函数返回给定参数的内存查看对象(memory view)。

### 3 编程规范

[PEP8 Python 编码规范整理](https://zhuanlan.zhihu.com/p/31212390)

[Python 的众多 PEP 之中，除了 PEP8 ，还有哪一些是值得阅读的？](https://www.zhihu.com/question/23484654/answer/207121583)

### 4 multiprocess

[ProcessPoolExecutor使用，见python cookbook](https://python3-cookbook.readthedocs.io/zh_CN/latest/c12/p08_perform_simple_parallel_programming.html?highlight=ProcessPoolExecutor)

[python 多进程使用](https://docs.python.org/zh-cn/3/library/concurrent.futures.html)

```python
def gpu_task(deploy, caffe_model,mean_file, path_images, out,images_head, gpu=0):
	  pass

multiprocessing.freeze_support()
pool = multiprocessing.Pool()
parts = 8
for i in xrange(0, parts):
    pool.apply_async(gpu_task, args = (deploy, caffe_model,mean_file,blocks[i], out_files[i],images_head, i + (8 - parts)))
pool.close()
pool.join()
```

### 5 虚拟环境

[Python虚拟环境pyenv、venv区别](Python虚拟环境pyenv、venv(pyvenv)、virtualenv之间的区别，终于搞清楚了！)
```shell
virtualenv envRote
source envRote/bin/activate
```






