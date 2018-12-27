# C标准函数库 stdlib.h
- [C标准函数库 stdlib.h](#c标准函数库-stdlibh)
    - [库变量](#库变量)
    - [库宏](#库宏)
    - [库函数](#库函数)

stdlib.h 头文件即standard library标准库头文件  

stdlib.h 头文件里包含了C语言的最常用的系统函数。


### 库变量


以下是在头文件stdlib.h中定义的变量类型：
| 变量    | 说明                                         |
| ------- | -------------------------------------------- |
| size_t  | 这是一个无符号整数类型的sizeof关键字的结果。 |
| whcar_t | 这是一个整数类型的宽字符常量的大小。         |
| div_t   | 这是结构的div函数返回。                      |
| ldiv_t  | 这是由 ldiv 函数返回的结构。                 |





### 库宏
以下是在头文件stdlib.h中定义的宏：
| 宏           | 说明                                                 |
| ------------ | ---------------------------------------------------- |
| NULL         | 这个宏是一个空指针常量的值。                         |
| EXIT_FAILURE | 这是退出功能，在发生故障的情况下，返回的值。         |
| EXIT_SUCCESS | 这是exit函数返回成功的情况下的值。                   |
| RAND_MAX     | 这个宏是由rand函数传回的最大值。                     |
| MB_CUR_MAX   | 这个宏不能大于MB_LEN_MAX的多字节字符集的最大字节数。 |



### 库函数
以下是在头stdio.h中定义的函数：
| S.N. | 功能及说明                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | `double atof(const char *str)`<br>转换参数str指向的字符串到浮点数（double类型）。 |
| 2    | `int atoi(const char *str)`<br>转换参数str指向的字符串为整数（int型）。 |
| 3    | `long int atol(const char *str)`<br>转换字符串参数str指向一个长整型（类型为long int） |
| 4    | `double strtod(const char *str, char **endptr)`<br>转换参数str指向的字符串到浮点数（double类型）。 |
| 5    | `long int strtol(const char *str, char **endptr, int base)`<br>字符串转换成一个长整型（类型为long int）参数str指向。 |
| 6    | `unsigned long int strtoul(const char *str, char **endptr, int base)`<br>转换字符串参数str指向一个无符号长整型（unsigned long型整数）。 |
| 7    | `void *calloc(size_t nitems, size_t size)`<br>分配请求的内存，并返回一个指向它的指针。 |
| 8    | `void free(void *ptr)`<br>calloc，malloc或realloc调用先前分配的回收内存。 |
| 9    | `void *malloc(size_t size)`<br>分配请求的内存，并返回一个指向它的指针。 |
| 10   | `void *realloc(void *ptr, size_t size)`<br>尝试调整的内存块的大小由ptr指向先前调用malloc或calloc的分配。 |
| 11   | `void abort(void)`<br>导致程序异常终止。                     |
| 12   | `int atexit(void (*func)(void))`<br>导致指定的函数功能，当程序正常终止时，被调用。 |
| 13   | `void exit(int status)`<br>导致正常程序终止。                |
| 14   | `char *getenv(const char *name)`<br>name所指向环境字符串的搜索，并返回相关的字符串的值。 |
| 15   | `int system(const char *string`<br>字符串指定的命令被传递到主机环境中，要执行的命令处理器。 |
| 16   | `void *bsearch(const void *key, const void *base, size_t nitems, size_t size, int (*compar)(const void *, const void *)`<br>执行二进制搜索。 |
| 17   | `void qsort(void *base, size_t nitems, size_t size, int (*compar)(const void *, const void*)`<br>对数组排序 |
| 18   | `int abs(int x)`<br>返回x的绝对值。                          |
| 19   | `div_t div(int numer, int denom`<br>数（分子numer）除以分母（分母denom）。 |
| 20   | `long int labs(long int x`<br>返回x的绝对值。                |
| 21   | `ldiv_t ldiv(long int numer, long int denom`<br>数（分母denom）除以分母（分子numer）。 |
| 22   | `int rand(void)`<br>返回一个取值范围为0到RAND_MAX之间的伪随机数。 |
| 23   | `void srand(unsigned int seed`<br>这个函数使用rand函数随机数生成器的种子。 |
| 24   | `int mblen(const char *str, size_t n)`<br>返回参数str指向一个多字节字符的长度。 |
| 25   | `size_t mbstowcs(schar_t *pwcs, const char *str, size_t n)`<br>转换pwcs 指向的数组参数str所指向的多字节字符的字符串。 |
| 26   | `int mbtowc(whcar_t *pwc, const char *str, size_t n)`<br>检查参数str所指向的多字节字符。 |
| 27   | `size_t wcstombs(char *str, const wchar_t *pwcs, size_t n)`<br>存储在数组pwcs 多字节字符并存入字符串str中的代码转换。 |
| 28   | `int wctomb(char *str, wchar_t wchar`<br>检查参数wchar多字节字符对应的代码。 |





