

[ref](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/%E4%B8%BA%E6%96%87%E6%9C%AC%E6%B7%BB%E5%8A%A0%E6%A0%B7%E5%BC%8F/Styling_lists)

## list-style-type

指定一个列表元素的外观。
- disc 默认为实心圆点
- circle 空心圆点
- square 实心方块
- decimal 十进制数字
- decimal-leading-zero 前面填充0的十进制数字

此外还有大小写罗马、拉丁、希腊等等
CJK相关的尚处实验阶段，大部分浏览器不支持

## list-style-postion
指定标记点的位置，默认在外面。
- inside
- outside

## list-style-image
`list-style-image`属性用来指定一个能用来作为列表元素标记的图片。

- none 默认，会根据list-style-type的属性来确定。
- <url> 指定一个URL来显示图片

```css
{
    /* Keyword values */
    list-style-image: none;
    /* <url> values */
    list-style-image: url('starsolid.gif');
}
```
注意：
- url函数中的网址可以直接书写或者用单引号、双引号包围。
- 相对地址是相对于CSS的地址而不是网页的URL。


## list-style 属性简写
`list-style` 属性是设置`list-style-type`, `list-style-image` 和 `list-style-position`的简写属性。

```css
.one {
  list-style: circle;
}

.two {
  list-style: square inside;
}
```

## 以下是html属性而不是css属性

## start
start 属性（设置在ol上）允许你从1 以外的数字开始计数。
```html
<ol start="4">
```
## reversed 
reversed 属性（设置在ol上）将启动列表倒计数。

## value 
value属性（设置在li上）允许设置列表项指定数值


