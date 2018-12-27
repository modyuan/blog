# CSS中的基本文本和字体样式

## color
设置元素的前景色（比如文本的颜色，或者一些线框的颜色）
```css
element { color: red }
element { color: #f00 }
element { color: #ff0000 }
element { color: rgb(255,0,0) }
element { color: rgb(100%, 0%, 0%) }
element { color: hsl(0, 100%, 50%) }

/* 50% translucent */
element { color: rgba(255, 0, 0, 0.5) } 
element { color: hsla(0, 100%, 50%, 0.5) }
```

## font-family
指定字体列表，前面的字体找不到会使用后面的，都没有的话使用浏览器设置的默认字体。列表使用逗号分割，字体名称中有空格的需要加双引号。

```css
p {
  font-family: "Trebuchet MS", Verdana, sans-serif;
}
```
CSS 定义了 5 个常用的字体名称:  `serif`（ 有衬线字体）, `sans-serif`（无衬线字体）, `monospace`（等宽字体）, `cursive`（模拟笔迹的字体） 和 `fantasy`（装饰字体）。  
这些都是非常通用的，当使用这些通用名称时，使用的字体完全取决于每个浏览器，而且它们所运行的每个操作系统也会有所不同。这是一种糟糕的情况，浏览器会尽力提供一个看上去合适的字体。

## font-size
字体大小，默认继承父元素的大小。
单位很多：px em rem % 等等

## font-style 字体大小
- normal 默认，普通字体
- italic 斜体，如果当前字体没有斜体版本，会使用oblique模拟
- oblique 斜体字体的模拟版本

## font-weight 字体粗细
- normal 、bold  普通和加粗
- lighter、border 将当前元素的粗细设置比父元素更细或更粗。
- 100-900 数值粗体值。


## text-decoration 文本装饰
- none 取消文本装饰
- underline 文本下划线
- overline 文本上划线
- line-through 穿过文本的线，也就是删除线

这个属性可以设置多个值，如`text-decoration: underline overline;`

## text-shadow 文本阴影
如`text-shadow: 4px 4px 5px red;`

4个属性分别如下：
1. 阴影与原始文本的水平偏移，可以使用大多数的 CSS 单位 length and size units, 但是 px 是比较合适的。这个值必须指定。
2. 阴影与原始文本的垂直偏移，效果基本上就像水平偏移，除了它向上/向下移动阴影，而不是左/右。这个值必须指定。
3. 模糊半径，更高的值意味着阴影分散得更广泛。如果不包含此值，则默认为0，这意味着没有模糊。可以使用大多数的 CSS 单位 length and size units.
4. 阴影的基础颜色，可以使用大多数的 CSS 颜色单位 CSS color unit. 如果没有指定，默认为 black.

可以指定多种阴影：
```css
text-shadow: -1px -1px 1px #aaa,
             0px 4px 1px rgba(0,0,0,0.5),
             4px 4px 5px rgba(0,0,0,0.7),
             0px 0px 7px rgba(0,0,0,0.4);
```

## line-height 行高
`line-height`属性设置文本每行之间的高，可以接受大多数单位 length and size units，不过也可以设置一个无单位的值，作为乘数，通常这种是比较好的做法。无单位的值乘以 font-size 来获得 line-height。


## overflow
`overflow` 定义当一个元素的内容太大而无法适应块级格式化上下文 时候该做什么。  
它是overflow-x和overflow-y的简写。
```css
/* 默认值。内容不会被修剪，会呈现在元素框之外 */
overflow: visible;

/* 内容会被修剪，并且其余内容不可见 */
overflow: hidden;

/* 内容会被修剪，浏览器会显示滚动条以便查看其余内容 */
overflow: scroll;

/* 由浏览器定夺，如果内容被修剪，就会显示滚动条 */
overflow: auto;

/* 规定从父元素继承overflow属性的值 */
overflow: inherit;
```
为使 overflow 有效果，块级容器必须有一个指定的高度（height或者max-height）或者宽度，或者将white-space设置为nowrap。

详细说明：  
- 默认属性是 `visible`， 也就是会超出限制的高度宽度（当然对于没有设置高度宽度的元素或者行内元素，也就无所谓超出不超出，因为内容会撑大外框）
- 设置为`hidden`的话，会隐藏超出的部分（前提是设置了高度或者宽度），可以分别设置x和y
- 设置为`scroll`的话，会总是显示一个滚动条，可以分别设置x和y
- 设置为`auto`的话，会根据是否超出，自动显示滚动条。

另外：文字的换行策略需要注意。长单词超出一行的长度，默认不会折行。

## text-overflow

`text-overflow` 属性确定如何向用户发出未显示的溢出内容信号。它可以被剪切，显示一个省略号（'...'，U + 2026 HORIZONTAL ELLIPSIS）或显示一个自定义字符串。


目前来说（2018/10）主流浏览器仅支持`clip`（默认）和`ellipsis`

省略号显然只能在x方向上显示，也就是对于y方向上的overflow，并不会显示省略号。对于多行文本，若是某一行文本超出边框（由于单词断行策略，可能会在某一行超出），在那一行上会显示省略号。

## white-space
`white-space`属性是用来设置如何处理元素中的空白。
空白有三种：
1. 文本中的空格和制表符
2. 文本中的换行符
3. 超出宽度导致的自动折行

默认是normal。

|          | 换行符 | 空格和制表符 | 文字转行 |
| -------- | ------ | ------------ | -------- |
| normal   | 合并   | 合并         | 转行     |
| nowrap   | 合并   | 合并         | 不转行   |
| pre      | 保留   | 保留         | 不转行   |
| pre-wrap | 保留   | 保留         | 转行     |
| pre-line | 保留   | 合并         | 转行     |



## word-break 和 overflow-wrap
`word-break` 指定了怎样在单词内断行。
- normal 默认值  
    - 对于英文字符，如果一行有若干个单词，最后一个单词超出界限，则最后一个单词放到下一行，本行最后留空。  
    如果一个单词**特别长**，即使一行只有这一个单词也超出了宽度，那么就会溢出显示（设置了overflow另说）  
    - 对于中日韩字符，则在任意部分换行。
- break-all 任意位置换行，即英文字符会在单词中间折行。
- keep-all 都不换行，超出的话会溢出显示（设置了overflow另说）

`overflow-wrap` 是用来说明当一个不能被分开的字符串太长而不能填充其包裹盒时，为防止其溢出，浏览器是否允许这样的单词中断换行。 

可以看出，这个属性解决了上一个属性中超长单词的问题。（此属性默认值为normal，设置为`overflow-wrap:"break-word"`就可以解决这里提到的问题）

## text-align

`text-align`属性是用在==块级元素==上，它定义它的==inline子元素==（例如文字）如何相对它的对齐。
text-align并不控制块元素自己的对齐，只控制它的行内内容的对齐。

总体来说，相当于MS Office Word里的对齐方式。

- left 左对齐
- right 右对齐
- center 居中对齐
- justify 两侧对齐，对最后一行无效
- justify-all 两侧对齐，强制最后一行也这样。

>start\end等属性还处于试验阶段，不推荐使用。


## text-indent

`text-indent`属性用在==块级元素==上（在行内元素上设置是无效的），它指定了它内部元素的文本的首行缩进。

值可以是px，百分比（相对于块级元素的宽度），em（相对于本元素的font-size），一般来说对于汉字，1em就相当于一个字的宽度。


## letter-spacing word-spacing
这两个属性指定了字母间以及单词间的间距大小

## text-transform

- capitalize 单词首字母大写
- uppercse 所有字符转换成大写
- lowercase 所有字符转换成小写
- none 没有大小写转换
- 一些其他属性没有被广泛支持


## writing-mode 
writing-mode 属性定义了文本水平或垂直排布以及在块级元素中文本的行进方向。

## 没什么用或者支持很差的属性

`line-break` `text-justify` `tab-size`
