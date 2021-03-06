# C标准函数库 总论



C语言的函数库几经扩展，明确了解标准库中到底有哪些函数，以及它们在什么库文件中，这是十分必要的。

第一篇先罗列一下标准库的名称以及大致的作用。

## **C89**的标准库

（即一开始就有的标准库）

| 库名     | 说明                                                         |
| :------- | ------------------------------------------------------------ |
| assert.h | 包含断言宏，被用来在程序的调试版本中帮助检测逻辑错误以及其他类型的bug。 |
| ctype.h  | 定义了一组函数，用来根据类型来给字符分类，或者进行大小写转换。 |
| errno.h  | 用来测试由库函数报的错误代码。                               |
| float.h  | 包含了一组浮点值相关的各种平台相关的常数。                   |
| limits.h | 专门用于检测整型数据数据类型的表达值范围。                   |
| locale.h | 定义C语言本地化函数。                                        |
| math.h   | 定义C语言数学函数。                                          |
| setjmp.h | 定义了宏setjmp和longjmp，在非局部跳转的时候使用。            |
| signal.h | 定义C语言信号处理函数。                                      |
| stdarg.h | 让函数能够接收可变参数                                       |
| stddef.h | 定义了几个常见的类型与宏。                                   |
| stdio.h  | 定义输入输出函数。                                           |
| stdlib.h | 定义数值转换函数，伪随机数生成函数，动态内存分配函数，过程控制函数。 |
| string.h | 定义C语言字符串处理函数。                                    |
| time.h   | 定义了关于日期和时间的类型和函数。                           |

## C95新增

| 库名     | 说明                                            |
| :------- | ----------------------------------------------- |
| iso646.h | 定义了一批C语言常见运算符的可选拼写。           |
| wchar.h  | 定义宽字符串处理函数。                          |
| wctype.h | ctype的宽字符版，对宽字符进行分类和大小写转换。 |

## C99新增
| 库名       | 说明                                                         |
| :--------- | ------------------------------------------------------------ |
| complex.h  | 定义了一组用来操作复数的函数。                               |
| fenv.h     | 定义了一组用来控制浮点数环境的函数。                         |
| inttypes.h | 提供有助于使代码与显式指定大小的数据项兼容（无论编译环境如何）的常量、宏和派生类型。 |
| stdbool.h  | 定义布尔数据类型。                                           |
| stdint.h   | 定义了具有特定位宽的整型，以及对应的宏；还列出了在其他标准头文件中定义的整型的极限。 |
| tgmath.h   | 定义泛型数学函数。                                           |



微软的编译器大致也就支持到C99的样子（老掉牙的VC6连C99都不支持。最新的VS2015也才没支持C11）。相比之下，GCC之类的编译器一直支持最新的标准。

## C11新增

| 库名          | 说明                                                         |
| :------------ | ------------------------------------------------------------ |
| stdalign.h    | 用于查询和指定对象数据结构的对齐                             |
| stdatomic.h   | 原子操作线程间的数据共享。提供了很多命名类和函数来操作atomic数据的大集 |
| stdnoreturn.h | 用于指定止回功能。                                           |
| threads.h     | 定义函数来管理多个线程互斥锁和条件变量。                     |
| uchar.h       | 提供了对16位与32位Unicode字符的支持。                        |
