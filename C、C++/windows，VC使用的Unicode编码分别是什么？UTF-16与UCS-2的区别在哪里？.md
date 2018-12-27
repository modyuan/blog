Windows自NT之后，使用的就都是Unicode编码了，但它使用额Unicode编码方式，是UCS-2呢，还是UTF-16？要弄清这个问题，就得弄清UCS-2和UTF-16之间的区别。

[确定Windows XP到底是UCS-2的还是UTF-16的](http://blog.csdn.net/vagrxie/article/details/3947054)

一般认为Windows下以16bit表示的Unicode并不是UTF-16，而是UCS-2。UCS-2是一种编码格式，同时也是指以一一对应关系的Unicode实现。在UCS-2中只能表示U+0000到U+FFFF的BMP(Basic Multilingual Plane )

Unicode编码范围，属于定长的Unicode实现，而UTF-16是变长的，类似于UTF-8的实现，但是由于其字节长度的增加，所以BMP部分也做到了一一对应，但是其通过两个双字节的组合可以做到表示全部Unicode，表示范围从U+0000 到 U+10FFFF。关于这一点，我在很多地方都看到混淆了，混的我自己都有点不太肯定自己的说法了，还好在《UTF-16/UCS-2》中还是区别开了，不然我不知道从哪里去寻找一个正确答案。（哪怕在IBM的相关网页上都将UCS-2作为UTF-16的别名列出）



在《UTF-16/UCS-2》文中有以下内容：

>UTF-16 is the native internal representation of text in the Microsoft Windows 2000/XP/2003/Vista/CE; Qualcomm BREWoperating systems; the Java and .NET bytecode environments; Mac OS X's Cocoa and Core Foundation frameworks; and the Qtcross-platform graphical widget toolkit.[1][2][citation needed]

Symbian OS used in Nokia S60 handsets and Sony Ericsson UIQ handsets uses UCS-2.

The Joliet file system, used in CD-ROM media, encodes filenames using UCS-2BE (up to 64 Unicode characters per file).

Older Windows NT systems (prior to Windows 2000) only support UCS-2.[3]. In Windows XP, no code point above U+FFFF is included in any font delivered with Windows for European languages, possibly with Chinese Windows versions.[clarification needed]



很明确的说明了Windows 2000以后内核已经是UTF-16的了，这点还真是与平时的感觉相违背，于是可以测试一下。在UTF-16的编码转换函数（Python实现）

中我在windows下输出了三个太玄经的字符。



从上面的内容我们可以总结出以下信息：

（1）Windows NT （before Windows 2000）的系统，仅支持UCS-2。Windows 2000之后的系统（Microsoft Windows 2000/XP/2003/Vista/CE），开始支持UTF-16了。

（2）虽然Windows XP支持UTF-16，但是随Window发布的为欧洲语言的字体并没有包含U+FFFF以后的代码点的字符，对汉语版本可能包含了。所以博主才能使用Python实现的编码转换函数，在WindowsXP下输出了三个太玄经的字符。

 

## 那VC中的Unicode编码方式究竟是UTF-16 or UCS-2?

`C++`的实现，包括VC，宽字符的表示是在字符串前加”L“，且有一套wide版本的处理函数。在标准C++和VC中，wide版本的字符都是用定长的2个字节来表示，因此大概能断定VC中使用的Unicode编码方式是UCS-2。

参考：http://social.msdn.microsoft.com/Forums/en-US/visualcpluszhchs/thread/34906c43-f272-4ca4-8188-16f9d10dc3da，他专门问了这个问题：“之所以产生这样的疑问, 是因为: UCS-2永远是用2个Byte来表示, 而UTF-16则有可能在某些特殊情况下使用4个Byte(即使特殊情况的例子我没有找到). 我用sizeof(wchar_t)看到是2, 是否说明用的是UCS-2? 或者说实际上我们用的是UTF-16, 只是没有考虑使用>0xFFFF的情况?”



他所找到的解答就是前面1中的那篇文章，他总结道：“虽然Windows内核使用的是UTF-16, 但VC中应该是使用的UCS-2. 所以VC是不支持那些大于0xFFFF的字符的.这样做的好处是, 我们在编程的时候可以把UNICODE当成固定长度的2个Byte, 丝毫不用去考虑4个Byte的情况啦。”

 



## UTF-16与UCS-2的关系

UTF-16可看成是UCS-2的父集。在没有辅助平面字符Mapping of Unicode character planes（surrogate code points）前，UTF-16与UCS-2所指的是同一的意思。但当引入辅助平面字符后，就称为UTF-16了。现在若有软件声称自己支援UCS-2编码，那其实是暗指它不能支援在UTF-16中超过2bytes的字集。对于小于0x10000的UCS码，UTF-16编码就等于UCS码。




