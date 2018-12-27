# CSS中长度单位类型
[原文](https://www.cnblogs.com/jesse131/p/9079219.html)

css中长度单位有很多，可谓五花八门，但可分为这两类：

## 绝对长度单位
彼此固定，不会因为其他元素的尺寸变化而变化。主要有`cm` `mm` `Q` `in` `pc` `pt` `px`

### px
px单位名称为像素，像素是固定大小的单元,用于屏幕媒体(即在电脑屏幕上读取)。一个像素等于电脑屏幕上的一个点 (是你屏幕分辨率的最小分割)。许多网页设计师在web文档使用像素单位以生产浏览器渲染的像素完美呈现的网站。

像素单位的一个问题是在IE下无法用浏览器字体缩放的功能(影响不是很大)。其他浏览器都支持。

## 相对长度单位
指定相对于另一长度的长度。主要有`em` `ex` `ch` `rem` `%`和可视区百分比长度单位 `vm` `vh` `vmin` `vmax`

由于各种单位比较多，我们这里只介绍常用的一些单位,其他单位的详细情况可查询W3C规范
### %
百分比是一个相对长度单位，相对于包含块（containing block）的高宽或字体大小。
关于包含块（containing block）的概念，不能简单地理解成是父元素。
如果是静态定位和相对定位，包含块一般就是其父元素。
如果是绝对定位的元素，包含块应该是离它最近的 position为非static属性的祖先元素。
如果是固定定位的元素，它的包含块是视口（viewport）。
具体可以参考 W3Help。

在使用百分比单位的时候，最让人困惑的就是不清楚它到底是相对于谁计算的，下面将详细介绍列出

- 相对于包含块的宽度
`left`、`right`、
`width`、`max-width`、`min-width`、
`margin`、`padding`、
`text-indent`;

- 相对于包含块的高度
`top`、 `bottom`、
`height`、 `max-height`、 `min-height`;

- 相对于自身的高宽
`border-radius`、`background-size`、`transform: translate()`、`transform-origin`、`zoom(css3缩放)`、`clip-path`

- 相对于自身的字体大小
`line-height`

- 相对于自身的行高（不常用）
`vertical-align`

- 相对于继承字体大小
`font-size`

- 特殊对象
    - 背景定位中的百分比 background-position 分别设置水平方向和垂直方向上的两个值，如果使用百分比，那么百分比值会同时应用于元素和图像。例如 50% 50% 会把图片的（50%, 50%）这一点与框的（50%, 50%）处对齐，相当于设置了 center center。同理 0% 0% 相当于 left top，100% 100% 相当于 right bottom。
    - filter(css3滤镜，不常用) filter: none | blur(%) | brightness(%) | contrast(%) | drop-shadow(%) | grayscale(%) | hue-rotate(%) | invert(%) | opacity(%) | saturate(%) | sepia(%) | url(%);

百分比的继承
如果某个元素设置了百分比的属性，则后代元素继承的是计算后的值。例如：

    p { font-size: 10px;line-height: 120%; }
那么p的子元素继承到的值是 line-height: 12px，而不是         

    line-height: 120%。

### em
em是相对字体长度单位。如果用于font-size属性本身，则是相对于父元素的font-size。若用于其他属性（width,height），则是相对于本身元素的font-size。国外使用比较多；

em单位有如下特点：

em的值并不是固定的;
em会继承父级元素的字体大小。

我们知道任意浏览器的默认字体大小都是16px。所有未经调整的浏览器都符合: 1em=16px。那么12px=0.75em,10px=0.625em。为了简化font-size的换算，需要在css中的body选择器中声明font-size=62.5%，这就使em值变为 16px*62.5%=10px, 这样12px=1.2em, 10px=1em, 也就是说只需要将你的原来的px数值除以10，然后换上em作为单位就行了。

em是继承父元素的字体大小，可是当父元素字体大小改变时，又得重新计算了，这不怎么方便，还好==rem==解决了这个问题

### rem
rem是CSS3新增的一个相对字体长度单位，只相对根元素即html元素字体大小
所以我们只要在html标签上设置字体大小为标准，文档中的字体大小都会以此为参照
```css
html { font-size:62.5%; }
body { font-size:12px;font-size:1.2rem ;}
p { font-size:14px;font-size:1.4rem;}
```
兼容性：

IE9/10中font缩写和伪元素中不支持rem单位
IE9/10部分支持，IE11+、Firefox、Chrome、Safari、Opera 的主流版本都支持，为了兼容不支持 rem 的浏览器，我们需要在 rem 前面写上对应的 px 值，这样不支持的浏览器可以优雅降级。

### vm vh vmin vmax（视口单位）
响应式布局的实现依靠媒体查询（ Media Queries ）来实现，选取主流设备宽度尺寸作为断点针对性写额外的样式进行适配，但这样做会比较麻烦，只能在选取的几个主流设备尺寸下呈现完美适配。CSS3中引入一种新的办法去真正地适配所有设备尺寸。


视口在桌面端，指的是浏览器的可视区域；而在移动端较为复杂，它涉及到三个视口：分别是 Layout Viewport（布局视口）、 Visual Viewport（视觉视口）、Ideal Viewport。
视口单位中的“视口”，在桌面端，毫无疑问指的就是浏览器的可视区域；但是在移动端，它指的则是Layout Viewport（布局视口） 。

根据CSS3规范，视口单位主要包括以下4个：

- vw : 1vw 等于视口宽度的1%
- vh : 1vh 等于视口高度的1%
- vmin : 选取 vw 和 vh 中最小的那个
- vmax : 选取 vw 和 vh 中最大的那个

#### 兼容性
在PC端用的很少，主要在移动端
在移动端 ios 8 以上以及 Android 4.4 以上获得支持，并且在微信 x5 内核中也得到完美的全面支持。

搭配vw和rem，布局更优化
给根元素大小设置随着视口变化而变化的 vw 单位，这样就可以实现动态改变其大小。
限制根元素字体大小的最大最小值，配合 body 加上最大宽度和最小宽度

## 总结
在一般的PC端网页制作过程中，一般直接用px为单位，在移动端由于vw,vh单位的兼容性，目前一般采用rem + %或者rem + vw/vh。