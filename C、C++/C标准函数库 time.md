# C标准函数库 time.h

time.h即关于时间的头文件。

## 一、time.h中定义的变量有：

    size_t    表达长度的类型（=unsigned int）
    time_t    标准计时点（一般是1970年1月1日午夜）到当前时间的秒数（=long long）
    clock_t   程序开始到现在的毫秒数（=long）
    struct tm 用于保存的时间和日期的结构。


```c
struct tm { 
    int tm_sec; /* seconds, range 0 to 59 */
    int tm_min; /* minutes, range 0 to 59 */
    int tm_hour; /* hours, range 0 to 23 */
    int tm_mday; /* day of the month, range 1 to 31 */
    int tm_mon; /* month, range 0 to 11 */
    int tm_year; /* The number of years since 1900 */
    int tm_wday; /* day of the week, range 0 to 6 */
    int tm_yday; /* day in the year, range 0 to 365 */
    int tm_isdst; /* daylight saving time */
};
  
```


## 二、库宏



    NULL 这个宏是一个空指针常量的值。
    CLOCKS_PER_SEC 这个宏表示每秒的处理器时钟周期的数目。

## 三、函数

1. time_t time(time_t *timer)
    作用是返回当前时间（标准计时点到现在的秒数）。一般来说参数设为NULL即可。
2. struct tm *localtime(const time_t *timer)
    把time_t分解成struct tm，得到各种详细的内容。时区是本地的时区。
3. struct tm *gmtime(const time_t *timer)
    和上面的函数作用差不多，但是本函数得到的是0时区的时间（格林威治标准时间）。
4. char *asctime(const struct tm *timeptr)
    返回一个字符串表示struct tm指针代表的时间。
5. clock_t clock(void)
    返回程序开始到此函数运行时的毫秒数。（1s=1000ms）
6. char *ctime(const time_t *timer)
7. char *asctime(const struct tm *timeptr)
    这两个函数都是返回一个字符串，表示当前的日期和时间。只是参数不同而已，结果一样。
8. double difftime(time_t time1, time_t time2)
    返回两个time_t的差值，单位为秒。
9. time_t mktime(struct tm *timeptr)
    相当于localtime的逆函数，就是把struct tm再转换成秒数。
10. size_t strftime(char *str, size_t maxsize, const char *format, const struct tm *timeptr)
    将time_t格式化为字符串。


根据format指向字符串中格式命令把timeptr中保存的时间信息放在strDest指向的字符串中，最多向strDest中存放maxsize个字符。该函数返回向strDest指向的字符串中放置的字符数。


    %a 星期几的简写
    %A 星期几的全称
    %b 月份的简写
    %B 月份的全称
    %c 标准的日期的时间串
    %C 年份的前两位数字
    %d 十进制表示的每月的第几天
    %D 月/天/年
    %e 在两字符域中，十进制表示的每月的第几天
    %F 年-月-日
    %g 年份的后两位数字，使用基于周的年
    %G 年份，使用基于周的年
    %h 简写的月份名
    %H 24小时制的小时
    %I 12小时制的小时
    %j 十进制表示的每年的第几天
    %m 十进制表示的月份
    %M 十时制表示的分钟数
    %n 新行符
    %p 本地的AM或PM的等价显示
    %r 12小时的时间
    %R 显示小时和分钟：hh:mm
    %S 十进制的秒数
    %t 水平制表符
    %T 显示时分秒：hh:mm:ss
    %u 每周的第几天，星期一为第一天 （值从1到7，星期一为1）
    %U 第年的第几周，把星期日作为第一天（值从0到53）
    %V 每年的第几周，使用基于周的年
    %w 十进制表示的星期几（值从0到6，星期天为0）
    %W 每年的第几周，把星期一做为第一天（值从0到53）
    %x 标准的日期串
    %X 标准的时间串
    %y 不带世纪的十进制年份（值从0到99）
    %Y 带世纪部分的十制年份
    %z，%Z 时区名称，如果不能得到时区名称则返回空字符。
    %% 百分号



还有一个虽然不是这个头文件里的，但是也是windows常用的就是

    WINBASEAPI VOID WINAPI Sleep(    _In_ DWORD dwMilliseconds    )


这个函数的作用是暂停一些时间再运行，参数是暂停的毫秒数。（DWORD=unsigned long）


还有就是许多函数在较新版的VS里会提示你使用安全版本的函数 如ctime_s asctime_s等等。


原来的是返回一个指针，但是这个指针用不用delete？很不好说。现在就好，这个变量你自己定义，然后传进去，函数把它填充了就好了，这就不会纠结了。










