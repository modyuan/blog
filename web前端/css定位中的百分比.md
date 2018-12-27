# css定位中的百分比
[原文](http://www.cnblogs.com/bobodeboke/p/6907160.html)
在前端css定位中经常面对的一个问题是，百分比定位究竟是针对于谁定位？

## 一、margin，padding的百分比

首先从css的设计意图说起，在浏览器默认的 writing-mode: horizontal-tb; 和 direction: ltr; 的情况下，因为CSS的基础需求是排版，而通常我们所见的横排文字，其水平宽度一定（仔细回想一下，如果没有显式的定义宽度或者强制一行显示，都会遇到边界换行，而不是水平延展），垂直方向可以无限延展。这也是为什么：

1. margin: auto; 无法在纵向上垂直居中，其实原因也是上面所说的，因为纵向是可以无限延展的，所以没有一个一定的值可以被参照被用来计算。
2. margin(包括margin-top,margin-bottom,margin-left,margin-right)：使用百分比时都是相对于父元素的宽度定位；
3. padding(包括padding-top,padding-left,padding-bottom,padding-right):使用百分比时也都是相对于父元素的宽度定位。

具体可参考这篇文章：
http://www.ituring.com.cn/article/64581

## 二、归纳如下：

相对于父元素宽度的，（如果自身已经是最外层元素，则相对于视口）
[max/min-]width、left、right、padding（包括padding-top,padding-bottom）、margin(包括margin-top,margin-bottom) 等；

相对于父元素高度的：（如果自身已经是最外层元素，则相对于视口）
[max/min-]height、top、bottom 等；

其中，关于height百分比定位的一个冷知识是：若某元素的父元素没有确定高度，则无法有效使用height=XX%的样式。除非该元素已经是最外层元素，此时是相对于视口。

详情可参见：http://www.cnblogs.com/vajoy/p/3730014.html

相对于继承字号的：
font-size 等；

相对于自身字号的：
line-height 等；

相对于自身宽高的：
border-radius、background-size、transform: translate()、transform-origin、zoom、clip-path 等；

特殊算法的：
background-position（方向长度 / 该方向除背景图之外部分总长度 * 100）、
filter 系列函数等；

如果自身设置 position: absolute，相对于离它最近的那个 position 不为 static 的祖先元素，如果没有这样的元素，则相对于视口。
如果 position: fixed，“父元素”指视口。