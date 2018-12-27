## 背景
**inline元素**，准确来说，是不可替换（non-replace）的inline元素，不能设置**竖直方向**上的**margin和padding**，之所以不能设置margin/padding，是因为padding的值是根据目标元素的width计算出来的，而inline中的non-replace元素的width是不确定的。


## 行内内容的居中（inline/inline-block）
### 1.text-align:center 水平居中
在**块级元素**上设置`text-align:center`，其内部的inline或inline-block的子元素以及文本内容会在父元素内居中。
### line-height 垂直居中
line-height设置了行间的距离（行高），将要居中的元素的line-heigth值设置为和其块级父元素的height值一样时，其内部内容会垂直居中。用于**单行**的行内元素的垂直居中
```css
.wrap{
  height:100px;
  line-height: 100px;
}
```
注意：line-height不能使用负值。在块级元素使用line-height是定义该元素基线之间的最小距离而不是最大距离。
### 2.vertical-align:middle 垂直居中
分为以下情况
#### 2.1 行内元素（inline/inline-block/inline-table）内部内容的居中
使用伪元素
```css
.center{
        display: inline-block;  /* 或者inline */
        vertical-align: middle;
}
.wrap::before{ /*或者::after*/
        content: ''; /*两个单引号*/
        display: inline-block;
        height: 100%;
        vertical-align: middle;
}
```
注意：
- vertical-align大部分取值是相对于父元素来说的
例如vertical-align:baseline（vertical-align的默认值）是相对于父元素的基线对齐。父元素的基线取值有以下规则：
    - inline类——小写字母x的底端
    - inline-block类，其内没有inline类型——盒子模型的底端
    - inline-block类，其内有inline类型时——其内最后一行inline元素的基线（即最后一行小写x的底端）
- 设置为middle也不一定是真正的对齐，某些字体的中线位置不一定顶部和底部的正中间。
- 不同风格的字体常有不同的排版标准，因此有不同的基线/中线/顶线等，多种字体混合排版会让基线发生变化（参看父元素的基线取值规则）。
#### 2.2 表格单元格（table-cell）元素
在单元格上设置vertical-align:middle，其内部元素将垂直居中。
#### 2.3 ::first-letter 和 ::first-line 伪元素（同第一条行内元素）

## 块级元素居中
block、list-item、inline-block等类型的元素如何在父元素中居中
### 1. margin/padding值设置居中
#### 1.精确设定父元素的padding或者子元素的margin的值使得子元素居中。（不推荐）
#### 2.calc计算数值（不推荐）
margin值为 父容器的宽/高的50% 减去 自身宽/高的50%
```css
.center{
        width:20rem; height:20rem;
        margin-left:calc(50% - 10rem);
        margin-top:calc(50% - 10rem);
}
```
注意：inline元素的margin/padding设置仅仅在水平方向上有效
#### 3. margin:0 auto 左右居中
要居中的块级元素（block）元素设置 `margin:0 auto `。
只能水平居中，在垂直方向设置auto无效。（因为margin/padding的百分比按照包含块的**宽度**进行计算）
注意：对浮动元素、绝对定位和固定定位的元素无效 。（例外：使用绝对定位+偏移量0+margin:auto方法中使用了四个方向的值为0）
### 2. 偏移量50% + 负margin值
设置50%的水平和垂直偏移，然后设置的margin-top和margin-left值是要居中元素自身宽/高的一半的负数 ：
```css
.wrap {
        position: relative;
}
.center {
        position: absolute;
        height: 100px;width:100px;
        top: 50%;  margin-top:-50px; /* 50 px is half of 100px 垂直居中 */
        left:50%;  margin-left:-50px;  /* 水平居中 */
}
```
### 3.偏移量50% + 负50%translate值
使用位移transform:translate，将设置了50%偏移的子元素”往回”移动自身宽高的一半：
对于子元素是inline的，把子元素设成inline-block，则也适用本方法。
```css
.wrap 
{
        position: relative;
}
.center {
        position: absolute;
        top: 50%;left:50%;
        transform: translate(-50%,-50%); 
        /* use translateY(-50%) for only vertical */
}
```
若父容器下只有一个元素，且父元素设置了高度，则只需要使用相对定位即可 
```css
.wrap{
        height:xxx;
}
.center { 
        position: relative;
        top: 50%; 
        left: 50%;
        transform: translate(-50%,-50%); /* 若分开设置X和Y来实现同时居中则有偏差*/
}
```
### 4. 偏移量0 + margin:auto
父元素设置相对或绝对定位；要居中的子元素设置绝对定位，所有偏移量为0，外边距为auto
```css
.wrap{
        position:relative;
}
.center{
        position:absolute;
        top:0;bottom:0;left:0;right:0;
        margin:auto;
}
```
### 5.Flex 弹性布局
- 在父元素上设置相关属性即可使子元素居中：
```css
.wrap{
        display: flex;/*Flex布局*/
        display: -webkit-flex; /* Safari */
        align-items: center; /*垂直居中*/
        justify-content: center; /*水平居中*/
}
```
- 父元素设置为弹性容器`display:flex`，子元素设置`magrin:auto`
```css
.wrap{
        display:flex;
}
.child{
        margin: auto;
}
```
注意：
- 如果有多个弹性子元素，默认情况下弹性子元素会成一横排分布在父元素容器中，因为
    1. flex默认将子元素水平排列到一行（flex-direction:row），使用`flex-direction:column`可以使子元素垂直排成一列。
    2. flex默认子元素不折行显示（flex-wrap: nowrap ），使用flex-wrap: wrap可使子元素自动折行显示（当一行宽/高度不足容下多个子元素时折行为多行/列）。
- align-items和align-content区别：
    - align-content属性只适用于多行子元素（超过一行）的flex容器，如果只有一行子元素，该属性不起作用；align-items适用于任意行子元素的flex容器。
    - align-content是设置一列子元素在整个纵轴上的对其方式；而align-items是设置每个子元素在该行的高度范围内的侧轴上的对齐方式（垂直对齐）。
    - align-items相当于将侧轴高度按行平分，设置的是子元素在该行高度上的对其方式。

### 6. grid网格布局居中
根据需要布局网格，将要居中的元素“摆放”在网格中间即可。
示例制作3x3的表格内元素居中：
```css
.wrap{
        display:grid;
        grid-template-rows: repeat(3, 1fr);
        grid-template-columns: repeat(3, 1fr);
}
.center{
        grid-row: 2;
        grid-column: 2;
}
```


## 表格内容居中
- 表格式布局：根据语义化原则，使用表格布局非表格的内容已不再合适，而且表格的<td> <th>标签的align和valign属性已经是HTML的废除标签属性，建议不要使用。
- 非表格元素模拟表格：可以使用display:table-cell 模拟其为一个表格，由于不建议使用废除的align和valign标签属性，故而也就vertical-align:middle 垂直居中具有实用性，将元素模拟成表格进行垂直居中意义也不大了，因此建议不要使用。
- 真正的表格，表格内容的居中：
    - 水平：text-align:center 
    - 垂直：vertical-align:middle
也可以使用margin/pading等其他方法来控制表格内容的居中
