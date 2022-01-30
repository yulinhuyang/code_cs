**单行没有大括号**

没有大括号的时候 只有下面的一句归if管，此时可以省略大括号

do{statement(s);} while( condition );

当循环的主体只包含一个语句时，可以省略括号

全局变量默认初始化为0，而局部变量被初始化为随机数

C++必须使用常量定义数组大小，不能使用变量,可以使用const int 定义大小，不能使用#define定义

**char* 与string之间转换**

char[]，char * ，string之间转换； https://blog.csdn.net/yzhang6_10/article/details/51164300

涉及到char[]字符数组与其它类型转换，一般需要进行拷贝，不能直接赋值实现。char []和char * 都可以通过构造新的string完成其对string的转换。
涉及到到char * 转换，需要注意类型一致，同时注意const的使用。
```cpp
//char 数组初始化
const int N = 11;
char s[N] = "abcdabcdef";
char s[] = "abcdabcdef";

char str[] = "lala";
char *str1 = str; //char[] 转char *
strncpy(st1, st, strlen(st) + 1);    //char *转char[],字符拷贝，注意加1操作

//char *转char[]
const char *st = "hehe";
char st1[] = "lalalala";
strncpy(st1, st, strlen(st) + 1);    // 注意加1操作 

//char[]与string之间转换: 1）直接赋值；2）构造转换实现
char st[] = "hello";   
string st1 = st;  //直接赋值实现 
string st2(st, st + strlen(st)); //构造实现 

//string转char[]: 拷贝实现，不能直接赋值
string s1 = "abcdabcdef";
char ts1[] = "abcffffffffff";
strncpy(ts1, ts.c_str(), ts.length() + 1);       // 注意，一定要加1，否则没有赋值'\0' 
```

