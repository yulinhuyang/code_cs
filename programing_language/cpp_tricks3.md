- 宏的问题：没有类型检查，运算符结合的时候，存在结合优先级

 

**头文件：**

头文件 ————> include了就是全局

 

 自引(号)他尖
项目组内部的头文件使用引号形式，对于标准库头文件、第三方库头文件、公司内部开发的平台组件或独立组件的头文件，使用尖括号形式

```
#include "Foo/Foo.h"             // cpp对应的头文件

#include <cstdlib>                // 标准库头文件
#include <string>

#include <linux/list.h>          // 系统库头文件
#include <linux/time.h>
#include <some_library/libxxx.h> // 其他库头文件

#include "project/public/Log.h"  // 本项目内其他头文件
```

**头文件应该自包含，不能重复包含：**

自包含就是任意一个头文件均可独立编译。如果在使用一个头文件时，还要包含另外一个头文件才能工作的话，会给使用这个头文件的用户增添不必要的负担。

另外，**还需要防止头文件重复包含**，团队可以选择使用**宏**或**#****pragma once**防止头文件重复包含。

```C++
// Foo.h
#ifndef TEST_FOO_H  // 防止重复包含
#define TEST_FOO_H
#include <iostream> // 满足自包含
inline void Foo()  
{
    ...
    std::cout << msg; // std::cout依赖iostream头文件
}
#endif
// test.cpp
#include "Foo.h" // 只需要包含Foo.h
int main()
{
    Foo();
    return 0;
}
```

 

使用命名空间管理源代码：
在namespace中定义全局变量、常量、函数和类型
• .cpp文件中不需要导出的符号，建议通过匿名namespace封装（也可以用static修饰），防止不必要的定义被暴露出去
• 不要在头文件中使用匿名namespace或static定义非外部可见符号
• 不要在头文件中或#include之前，使用using向全局命名空间导入符号，否则会影响后续代码，易造成符号冲突。



#### 2.8 标准库 

 优先使用 C++标准库

- 使用特殊成员函数或重载操作符替代memset/memset_s、memcpy/memcpy_s     、memcmp等操作类对象
- 使用C++提供的字符串库，替代C风格的字符串
- 使用C++提供的std::vector、std::array、std::span等替代原生数组
- 使用C++提供的std::format()、std::ostringstream等方法进行格式化，替代sprintf、snprintf/snprintf_s等C函数
- 使用C++提供的std::ifstream、std::ofstream、std::fstream完成文件IO操作，替代C语言FILE结构和相关函数
- 使用C++提供的多线程、原子变量、锁、条件变量等，替代POSIX     线程库
- 使用C++提供的数值库，替代C语言数值库

 

应使用新的cxxx形式的头文件。对于一些形式为xxx.h的C标准库头文件，在C++标准库中同时提供了同名的头文件和功能相同的新的cxxx形式的头文件

#include <cstdlib> // 符合：使用 <cstdlib> 而不是 <stdlib.h> 来包含C标准库的头文件



C风格字符串: \0结束符

 

**使用**std::string表示字符串**

**优点：**

- 不用考虑结尾的'\0'
- 可以直接使用+、=、<、>等操作符，并且提供丰富的字符串操作函数
- 不需要考虑内存分配操作，避免了显式的new/delete，以及由此导致的错误

**问题：**

- 将空指针传递给std::string构造函数，会解引用空指针，从而导致程序产生未定义行为
- std::string类型的**c_str**和**data**成员函数返回的指针，可能会随着修改对象而失效，在使用时需要确保指针的有效性

 

**使用**有效的迭代器

- 使用有效的迭代器和指向容器元素的指针与引用。如：std::**find**() 等算法可能返回无效位置；调用**erase**()、**insert**() 等方法修改容器时，已有的**迭代器也会失效**。
- 外部数据用于容器索引或迭代器时必须确保在有效范围内，需要进行合法性校验。

 

**erase-remove**惯用法

Erase-Remove 惯用法 : https://www.cnblogs.com/tianshihao/p/14378812.html

vector<int> v;

v.erase(remove(v.begin(), v.end(), 99), v.end());

**remove() 并不真正删除东西，因为它做不到**。remove() 移动指定区间中的所有元素直到所有“不删除的”元素在区间的开头（相对位置和原来的一样）。它返回一个指向最后一个“不删除的”元素的下一个元素的迭代器。返回值是区间的“新逻辑终点”。

 

**正确使用格式化输入/输出函数**

调用格式化输入/输出函数时，使用有效的格式字符串。并且格式串必须与对应的实参类型、数量做严格匹配。
Eg:

使用%ld这里用来打印int64_t类型，


调用格式化输入/输出函数时，禁止format参数受外部数据控制。应使用常量字符串作为格式串。

 

**防止命令注入问题：**

system()、popen()等函数会使用命令解析器去执行命令，如果这些函数的参数是外部可控的数据，会导致命令注入漏洞。

命令解析器支持命令分隔符, 管道符用于连续执行多个命令或程序。利用这个特性，当命令参数中含有外部精心构造的数据时，可以注入任意命令。

 

**禁止外部可控数据作为进程启动函数的参数或者作为dlopen/LoadLibrary等模块加载函数的参数**

- 可以使用POSIX的**exec**系列函数直接启动进程，但是不能使用shell启动。
- 若使用**system**()函数，则应使用硬编码的函数入参或对外部输入中的命令分隔符进行白名单过滤/转义。

 



**线程不安全函数/异步不安全函数：**

- C库中的线程不安全函数：**strtok**, **ctime**，**putenv**，…
- C标准库函数中只有**abort**()， _**Exit**()，**quick**_exit()和signal()函数可以在信号处理程序中安全的调用；

- 在信号处理程序中不能含有new或delete操作，不能抛出异常，不能捕获异常，…

 

**2.9** **继承自C语言的编程规范条款**

- 关于**宏定义**：**完备的括号**，**不以分号**结尾，**多条语句**的函数式宏放在 **do-while(0)** 中包装，**不依赖**宏外部的**局部变量**名

 

**变量**

- 避免使用大量栈分配，对于 x86 和 x64     计算机，默认堆栈大小为 1 MB。

- 当局部变量大小超过**1k****时**，要审视合理性；较大的类型作为函数参数时，应使用**引用或指针**

```
constexpr int MAX_BUFF 0x1000000

//过大，单位换算 **byte** --> **KB** --> **MB**

char buff[MAX_BUFF] = {0};

 
```

**控制语句**

- 循环必须安全退出，在退出前正确释放相关资源，避免资源泄漏。

**整数**

- 确保枚举常量映射到唯一值；如果： 类型的多个成员确实需要分配相同的值，应当提供**显式的整数赋值**。

```
enum class ErrorCode {

  SYS_ARG_ERR = 1,

  SYS_SET_ERR = 2,

  SYS_OCCUPIED_ERR = 3, 

  SYS_REQFREE_ERR = 4,

  

  // 显示指定与 SYS_OCCUPIED_ERR的值相同  

  SYS_NO_REC_ERR = 3,

  

  // 显示指定 SYS_REQFREE_ERR的值相同

  SYS_ACTIVED_ERR = 4, 

  SYS_NO_FILE_ERR = 5

};
```

**断言**

断言主要用于调试期间，在发布版本中应将其关闭。因此，断言应该用于防止不正确的程序员假设，而不能用在发布版本上检查程序运行过程中发生的错误。

**断言永远不应用于验证是否存在**运行时（与逻辑相对）错误，包括但不限于：

**函数使用**

禁止使用**realloc**()函数

禁止使用**alloca**()函数申请栈上内存

必须**检查**安全函数**返回值**，并进行正确的处理

正确设置安全函数中的**destMax**参数

禁止封装安全函数

禁止用**宏重命**名安全函数

只能使用华为安全函数库中的安全函数或经华为认可的其他安全函数

禁止直接使用外部数据拼接SQL命令

禁止使用内存操作类不安全函数

 

安全函数有： memcpy_s、memset_s、strcpy_s、sprintf_s、strcat_s、

都有destMax参数

errno_t memcpy_s(void *dest, size_t **destMax**, const void *src, size_t **count**);

**destMax**是dest的实际大小；

**count**是实际要拷贝内存的大小；

如果count>destMax会返回失败；

返回值：

 

150 – src为空

162 – count>destMax

 

**destMax**

安全函数拦截缓冲区写溢出的前提是正确设置destMax，否则达不到预期效果。

**dest为数组的情况：**

• destMax必须设置为 sizeof(dest) 或BUFF_SIZE*sizeof(elementType)。(elementType是数组元素的类型)

• 当数组元素为字节类型(如char, signed char, unsigned char)时，sizeof(elementType)为1，可以省略。

**dest为动态分配的堆内存的情况：**

• destMax设置为当初分配的内存大小，或剩余内存的大小。

**dest为变长struct结构中的变长成员变量：**

• destMax必须设置为该成员变量的实际大小，结构体中必须有描述变长部分长度的成员变量



```
#define BUFF_SIZE 100

unsigned char dest[BUFF_SIZE];

…

ret = memcpy_s(dest, sizeof(dest), src, srcLen);

…

ret = memcpy_s(dest, BUFF_SIZE, src, srcLen);

```


- 内存中的敏感信息使用完毕后立即清0


其他：

不要在信号处理函数中访问共享对象

禁用rand函数产生用于安全用途的伪随机数

禁止在发布版本中输出对象或函数的地址

禁止代码中包含公网地址

 
