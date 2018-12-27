# C标准函数库 stdio.h


[TOC]

先从最常见的开始吧，最开始编的的C程序想必都是这样的：
```c
#include <stdio.h>  
int main()  
{  
    //一些语句  
    printf("XXXXXXXXXXX\n");  
    return 0;  
}  
```
stdio 就是指 “standard input & output"（标准输入输出），所以这个库基本是必备的。


stdio.h头定义了三个变量的类型，几个宏及各种功能进行输入和输出。



## 库变量
| 变量   | 说明                                               |
| :----- | -------------------------------------------------- |
| size_t | 这是一个无符号整数类型，sizeof关键字的结果。       |
| FILE   | 这是一个对象的类型，适合用于存储信息的一个文件流。 |
| fpos_t | 这是一个对象类型适用于存储在一个文件中的任何位置。 |


## 库宏



| 宏                               | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| NULL                             | 这个宏是一个空指针常量的值。                                 |
| _IOFBF, _IOLBF and _IONBF        | 这些都是宏扩大整型常量表达式具有鲜明的值和适合使用setvbuf函数的第三个参数。 |
| BUFSIZ                           | 这个宏是一个整数，它表示函数setbuf函数所使用的缓冲区的大小。 |
| EOFM                             | 此宏是一个负整数，表示一个结束-已到达文件结尾。              |
| FOPEN_MAX                        | 此宏是一个整数，代表文件的最大数目，该系统可以保证，可以同时打开。 |
| FILENAME_MAX                     | 这个宏是一个整数，表示一个字符数组，适合持有时间最长的可能的文件名长度最长。如果实现没有任何限制，那么这个值应该是推荐的最大值。 |
| L_tmpnam                         | 这个宏是一个整数，表示适合举行由tmpnam函数创建的临时文件名可能的最长的一个char数组的长度最长。 |
| SEEK_CUR, SEEK_END, and SEEK_SET | 这些宏用于的fseek 函数定位在一个文件中的不同位置。           |
| TMP_MAX                          | 这个宏是唯一的文件名的功能使用tmpnam可以生成的最大数量。     |
| stderr, stdin, and stdout        | 这些宏的文件类型对应的标准误差，标准输入，标准输出流的指针。 |



## 库函数



以下是在头stdio.h中定义的函数：


| 函数                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `void clearerr(FILE *stream)`                                | 作用是使文件错误标志和文件结束标志置为0.                     |
| `int fclose(FILE *stream)`                                   | 关闭一个流，把缓冲区内最后剩余的数据输出到磁盘文件中，并释放文件指针和有关的缓冲区。 |
| `int feof(FILE *stream)`                                     | 检测流上的文件结束符                                         |
| `int ferror(FILE *stream)`                                   | 检测输入输出流是否发生错误。                                 |
| `int fflush(FILE *stream)`                                   | 清除文件缓冲区，文件以写方式打开时将缓冲区内容写入文件       |
| `int fgetpos(FILE *stream,fpos_t *pos)`                      | 取得当前文件的句柄                                           |
| `FILE *fopen(const char *filename, const char *mode)`        | 用指定模式打开一个文件。                                     |
| `size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream)` | 读取文件(可安全用于二进制文件)`||`FILE *freopen(const char *filename, const char *mode, FILE *stream)` |
| `int fseek(FILE *stream, long int offset, int whence)`       | 重定位流(数据流/文件)上的文件内部位置指针                    |
| `int fsetpos(FILE *stream, const fpos_t *pos)`               | 设置到给定位置的给定的流文件中的位置。参数pos 是由函数的fgetpos 给定的位置。 |
| `long int ftell(FILE *stream)`                               | 返回给定流的当前文件位置。                                   |
| `size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream)` | 向文件写入一个数据块。这个函数以二进制形式对文件进行操作     |
| `int remove(const char *filename)`                           | 删除文件或目录                                               |
| `int rename(const char *old_filename, const char *new_filename)` | 重命名文件，也可以移动文件。                                 |
| `void rewind(FILE *stream)`                                  | 将文件内部的位置指针重新指向一个流（数据流/文件）的开头      |
| `void setbuf(FILE *stream, char *buffer)`                    | 打开和关闭缓冲机制                                           |
| `int setvbuf(FILE *stream, char *buffer, int mode, size_t size)` | 另一个设置缓冲机制的函数                                     |
| `FILE *tmpfile(void)`                                        | 以wb+形式创建一个临时二进制文件                              |
| `char *tmpnam(char *str)`                                    | 产生一个唯一的文件名                                         |
| `int fprintf(FILE *stream, const char *format, ...)`         | 格式化输出到一个流/文件中                                    |
| `int printf(const char *format, ...)`                        | 格式化输出函数, 一般用于向标准输出设备按规定格式输出信息     |
| `int sprintf(char *str, const char *format, ...)`            | 字串格式化命令                                               |
| `int vfprintf(FILE *stream, const char *format, va_list arg)` | 格式化的数据输出到指定的数据流中                             |
| `int vprintf(const char *format, va_list arg)`               | 送格式化输出到stdout中，与printf函数类似，所不同的是，它用一个参数取代了变长参数表 |
| `int vsprintf(char *str, const char *format, va_list arg)`   | 送格式化输出到串中                                           |
| `int fscanf(FILE *stream, const char *format, ...)`          | 从一个流中执行格式化输入                                     |
| `int scanf(const char *format, ...)`                         | 格式输入函数，从键盘输入指定格式的数据                       |
| `int sscanf(const char *str, const char *format, ...)`       | 从一个字符串中读进与指定格式相符的数据                       |
| `int fgetc(FILE *stream)`                                    | 从文件指针stream指向的文件中读取一个字符，读取一个字节后，光标位置后移一个字节 |
| `char *fgets(char *str, int n, FILE *stream)`                | 从文件结构体指针stream中读取数据，每次读取一行。             |
| `int fputc(int char, FILE *stream)`                          | 将字符ch写到文件指针fp所指向的文件的当前写指针的位置         |
| `int fputs(const char *str, FILE *stream)`                   | 向指定的文件写入一个字符串(不自动写入字符串结束标记符'\0')。 |
| `int getc(FILE *stream)`                                     | 获取下一个字符（unsigned char类型）从指定的流，并将流的指针位置。 |
| `int getchar(void)`                                          | 从标准输入获取一个字符（unsigned char类型）。                |
| `char *gets(char *str)`                                      | 从标准输入读取一行，并将其存储到由str指向的字符串。它时停止读取换行符或文件结束时达成，以先到为准。 |
| `int putc(int char, FILE *stream)`                           | 写入字符到指定的流，并将流的指针位置的参数指定一个字符（unsigned char类型）。 |
| `int putchar(int char)`                                      | 写一个字符（unsigned char类型）的参数指定的字符输出到stdout。 |
| `int puts(const char *str)`                                  | 将一个字符串写入到stdout，但不包括空字符。一个换行符被追加到输出。 |
| `int ungetc(int char, FILE *stream)`                         | 将字符的字符（unsigned char类型）到指定的流，这是读取的下一字符。 |
| `void perror(const char *str)`                               | 打印一个描述性的错误消息到stderr。首先打印字符串str接着一个冒号，然后是一个空格。 |



还存在一些C11中定义的安全函数（以_s结尾的）。需要定义`__STDC_LIB_EXT1__`宏才可以开启扩展。
