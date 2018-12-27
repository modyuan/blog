# C标准函数库 其他
----
- [# C标准函数库 其他](#-c标准函数库-其他)
- [1 string.h](#1-stringh)
- [2 ctypes.h](#2-ctypesh)
- [3 assert.h](#3-asserth)
- [4 iso646.h](#4-iso646h)
- [5 limits.h](#5-limitsh)
- [6 stdbool.h](#6-stdboolh)
- [7 stddef.h](#7-stddefh)
- [8 math.h](#8-mathh)
- [9 locale.h](#9-localeh)
- [10 float.h](#10-floath)
- [11 stdarg.h](#11-stdargh)
- [12 signal.h](#12-signalh)
- [13 setjmp.h](#13-setjmph)


一个一个写太蠢了。。。
我就把大概内容列举一下好了。
## 1 string.h
这个库的操作对象是`char*`
string.h 头定义了操作字符数组的各种函数。（注意：C语言中并没有string类型，虽然头文件的名字叫这个。。。）
就是各种对char*的查找、比较、计数、拼接、复制、分割等等操作。



## 2 ctypes.h
这个库的操作对象是`char`
就是判断一个char是不是一个数字、字母、空白字符、小写、大写、控制字符等等。感觉这个库的作用不大，都是一些基础的范围判断函数。


## 3 assert.h
1.这个标准库定义了程序调试用的 assert() 诊断宏。用于动态辨识程序的逻辑错误条件。 没错！这个库就一个函数。
其原型是： 
```c
void assert(int expression);
```
如果宏的参数求值结果为非0值，则不做任何操作，如果是0，用宽字符打印诊断信息，然后调用abort()结束程序。 
意思确保那个表达式是true（非0），不然就说明程序有出乎意料的地方，直接退出。
头文件中还定义了一个NODEBUG宏可以屏蔽所有assert()

2.定义了通过错误码来回报错误信息的宏 errno。 
可以把errno视作变量，通过检测errno的值来判断发生了什么作用。 
同时，你可以通过中的 strerror() 函数来把错误代码转化为错误信息（字符串）。

## 4 iso646.h
提供了一些逻辑运算和位运算符的替代关键字。 比如有的国家键盘上一些符号可能不好找。


这个库定义了以下的宏。但实际上头文件是空的，然而如果你想使用这些替代符号，还是要include才行。
| 宏     | 定义为       |
| ------ | ------------ |
| and    | &&           |
| and_eq | &=           |
| bitand | &            |
| bitor  | &#124;       |
| compl  | ~            |
| not    | !            |
| not_eq | !=           |
| or     | &#124;&#124; |
| or_eq  | &#124;=      |
| xor    | ^            |
| xor_eq | ^=           |
## 5 limits.h
这个头文件定义了整数类型的一些极限值。 
以下的常数已32位电脑的常见数值为例，但不同硬件、操作系统、编译器可能会有不同的数值。


    CHAR_BIT 字节的最小位数：8
    SCHAR_MIN 有符号字符类型的最小值：-128
    SCHAR_MAX 有符号字符类型的最大值：+127
    UCHAR_MAX 无符号字符类型的最大值：255
    CHAR_MIN 字符类型的最小值
    CHAR_MAX 字符类型的最大值
    MB_LEN_MAX 多字节字符在任何locale中可能的最长字节数：4/5/8/16
    SHRT_MIN 短整型最小值：-32768，即- 215
    SHRT_MAX 短整型最大值：+32767，即 215 - 1
    USHRT_MAX 无符号短整型最大值：65535 ，即 216 - 1
    INT_MIN 整型最小值：-2147483648，即 -(231)
    INT_MAX 整型最大值：+2147483647 ，即231 - 1
    UINT_MAX 无符号整型最大值：4294967295，即232 - 1
    LONG_MIN 长整型最小值：-2147483648 ，即-(231 )
    LONG_MAX 长整型最大值：+2147483647 ，即231 - 1
    ULONG_MAX 无符号长整型最大值：4294967295 ，即232 - 1
    LLONG_MIN 长长整型最小值：-9223372036854775808 ，即-(263 )
    LLONG_MAX 长长整型最大值：+9223372036854775807 ，即263 - 1
    ULLONG_MAX 无符号长长整型最大值：18446744073709551615 ，即264- 1


## 6 stdbool.h 


这个头文件很简单，就是定义了用于布尔类型的四个宏。


```
#define __bool_true_false_are_defined
#define bool    _Bool
#define false   0
#define true    1
```


C语言中之前一直没有布尔类型，而是用非0代表True，0代表False的。这个头文件提供了一种简答的代替，算不上多么严谨。


## 7 stddef.h


定义了若干常见的类型与宏。
**类型**


    ptrdiff_t 有符号整数型
    size_t 无符号整数型
    wchar_t 16位或32位整数型


**宏**


    NULL 与实现相关的空指针的值
    offsetof(type, member-designator) 结构体内成员的偏移量


## 8 math.h


math.h头定义了各种数学函数和一个宏。这个库中所有可用的函数取double参数并返回double的结果。
**库宏**
只有一个在这个库中定义的宏：


    HUGE_VAL 
当函数结果可能不是一个浮点数表示。正确的结果如果幅度太大，无法表示的功能设置errno为ERANGE表示一个范围错误，并且返回一个特定的值非常大宏HUGE_VAL或其否定（ - HUGE_VAL）命名。


如果结果的幅度太小，而不是一个零值，则返回。在这种情况下，将errno可能会或可能不会被设置为ERANGE。


**库函数**
以下是math.h的标头中定义的函数：
| 函数                                   | 说明                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| double acos(double x)                  | 返回x的反余弦弧度。                                          |
| double asin(double x)                  | 返回x的正弦弧线弧度。                                        |
| double atan(double x)                  | 返回x的反正切值，以弧度为单位。                              |
| double atan2(doubly y, double x)       | 返回y / x的以弧度为单位的反正切值，根据这两个值，以确定正确的象限上的标志。 |
| double cos(double x)                   | 返回的弧度角x的余弦值。                                      |
| double cosh(double x)                  | 返回x的双曲余弦。                                            |
| double sin(double x)                   | 返回一个弧度角x的正弦。                                      |
| double sinh(double x)                  | 返回x的双曲正弦。                                            |
| double tanh(double x)                  | 返回x的双曲正切。                                            |
| double exp(double x)                   | 返回e值的第x次幂。                                           |
| double frexp(double x, int *exponent)  | x=m*2^exp，m为规格化小数。返回尾数m，并将指数存入exp中       |
| double ldexp(double x, int exponent)   | 返回x*2^exp的值。                                            |
| double log(double x)                   | 返回自然对数的x（以e为底）。                                 |
| double log10(double x)                 | 返回x的常用对数（以10为底）。                                |
| double modf(double x, double *integer) | 返回的值是小数成分（小数点后的部分），并设置整数的整数部分。 |
| double pow(double x, double y)         | 返回x的y次方。                                               |
| double sqrt(double x)                  | 返回x的平方根。                                              |
| double ceil(double x)                  | 返回大于或等于x的最小整数值。                                |
| double fabs(double x)                  | 返回x的绝对值                                                |
| double floor(double x)                 | 返回的最大整数值小于或等于x。                                |
| double fmod(double x, double y)        | 返回的x除以y的余数。                                         |


## 9 locale.h
locale.h头文件定义了特定的位置设置，如日期格式和货币符号。有一个重要的结构struct lconv和两个重要的函数，下面列出一些宏定义。


**库宏** 
以下是在标头中定义的宏，这些宏将被用在下面列出的两个函数：
| 宏          | 说明                 |
| ----------- | -------------------- |
| LC_ALL      | 设置一下所有         |
| LC_COLLATE  | 设置字符串格式化相关 |
| LC_CTYPE    | 设置字符函数相关     |
| LC_MONETARY | 设置货币相关         |
| LC_NUMERIC  | 设置十分位分隔符相关 |
| LC_TIME     | 设置日期时间相关     |


**库函数**
以下是头locale.h中定义的函数：
```c
char *setlocale(int category, const char *locale)
//设置或读取位置相关的信息。
struct lconv *localeconv(void)
//设置或读取位置相关的信息。
```
当 locale 为 NULL 时，函数只做取回当前 locale 操作，通过返回值传出，并不改变当前 locale。


当 locale 为 "" 时，根据环境的设置来设定 locale，检测顺序是：环境变量 LC_ALL，每个单独的locale分类LC_*，最后是 LANG 变量。为了使程序可以根据环境来改变活动 locale，一般都在程序的初始化阶段加入下面代码：setlocale(LC_ALL, "")。


当C语言程序初始化时（刚进入到 main() 时），locale 被初始化为默认的 C locale，其采用的字符编码是所有本地 ANSI 字符集编码的公共部分，是用来书写C语言源程序的最小字符集（所以才起locale名叫：C）。


当用 setlocale() 设置活动 locale 时，如果成功，会返回当前活动 locale 的全名称；如果失败，会返回 NULL。
## 10 float.h


float.h中的C标准库的头文件包含了一组浮点值相关的各种平台相关的常数。这些常量是由ANSI C允许更多的可移植程序提出。 
具体就是定义了一些常数而已，懒得写了。。。。。。

## 11 stdarg.h


stdarg.h头文件定义了一个变量va_list类型和三个宏，可以用来获取一个函数的参数的个数，即不知道可变数目的参数。


可变参数函数定义的参数列表的末尾的省略号（…）。
**库变量**
以下是在头文件stdarg.h中定义的变量类型：


    va_list
这是一种适合于保持的信息所需要的3个宏 va_start(), va_arg() 和 va_end(). 库宏 
以下是在头文件stdarg.h中定义的宏：


    void va_start(va_list ap, last_arg)
    此宏初始化就根据va_arg和va_end宏要使用的变量。last_arg是最后一个已知的固定参数被传递给函数，即。的说法前省略号。
    type va_arg(va_list ap, type)
    这个宏检索函数型的参数列表中的下一个参数type.
    void va_end(va_list ap)
    这个宏允许使用va_start宏返回一个函数变量参数。 va_end中之前没有调用的函数返回的结果是不确定的。



## 12 signal.h
signal.h头文件中定义变量类型sig_atomic_t，两个函数调用和几个宏处理程序的执行过程中不同的信号报告。
**库变量**
以下是在头signal.h中定义的变量类型：


    sig_atomic_t
这是int型，并用作一个信号处理程序中的变量。这是一个可以被访问的原子实体，异步信号，即使在存在一个对象，该对象的组成不同。
库宏


以下是在头signal.h中定义的宏，这些宏将被用在下面列出的两个函数。信号函数SIG_宏定义信号。


    SIG_DFL 默认信号处理程序
    SIG_ERR 表示一个信号错误。
    SIG_IGN 信号忽视。


SIG宏被用来表示在下列条件下的信号数
| 宏      | 说明                     |
| ------- | ------------------------ |
| SIGABRT | 程序异常终止             |
| SIGFPE  | 除数为零的浮点错误。     |
| SIGILL  | 非法操作。               |
| SIGINT  | 中断信号，如CTRL-C。     |
| SIGSEGV | 访问无效存储如区段违规。 |
| SIGTERM | 终止请求。 库函数        |
**库函数**
以下是在头signal.h中定义的函数：
```
void (*signal(int sig, void (*func)(int)))(int)
//此功能设置函数来处理信号，即。信号处理程序。
int raise(int sig)
//该函数会导致产生信号sig。信号参数是与SIG宏兼容。
```



## 13 setjmp.h
setjmp.h 头定义宏的setjmp()，一个函数longjmp()和一个可变typejmp_buf的绕过正常的函数调用和返回学科。


以下是在头setjmp.h中定义的变量类型：


jmp_buf
这是一个数组类型用于宏调用setjmp（）和longjmp的（）函数持有信息。 库宏 
只有一个在这个库中定义的宏：


int setjmp(jmp_buf environment)
此宏保存当前的环境下入变量的环境中由函数longjmp()以供以后使用。如果该宏返回直接从宏调用，它返回零，但如果它返回的longjmp()函数调用，则返回一个非零值。 库函数 
以下是定义在头setjmp.h中只有一个函数：


void longjmp(jmp_buf environment, int value)

此函数恢复由最近一次调用setjmp()调用到jmp_buf参数与相应的程序在同一调用宏保存的环境。