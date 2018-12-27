# CSS中的background系列属性

我们可以用`background`简写属性直接一次性定义`attachment` `color` `image` `origin` `size` `repeat`方式等等

```css
{
    /* 使用 <background-color> */
    background: green;
    
    /* 使用 <bg-image> 和 <repeat-style> */
    background: url("test.jpg") repeat-y;
    
    /* 使用 <box> 和 <background-color> */
    background: border-box red;
    
    /* 将背景设为一张居中放大的图片 */
    background: no-repeat center/80% url("../img/image.png"); 
}

```

## backgroup-attachment
背景图片相对于当前元素的固定方式。
- scroll 随着网页整体的滚动条而滚动，不随元素内文本滚动。
- local 随着元素内容一起滚动，比如一个有滚动条的p元素，当值为scroll时，滚动元素本身的滚动条，背景不会滚动，而值为local时，可以随着内容一起滚动。
- fixed 相对于视口固定（大致相当于固定在屏幕上）。不会随着其他元素的滚动。

## background-clip 
设置元素的背景（背景图片或颜色）是否延伸到边框下面。
- border-box 默认
- padding-box
- content-box

## background-origin 
规定了指定背景图片background-image 属性的原点位置的背景相对区域.
- border-box 默认
- padding-box
- content-box


## background-color 
会设置元素的背景色, 属性的值为`颜色值` 或 `关键字"transparent"` 二者选其一.


## bakcground-image
设置一个或多个背景图像，用逗号分割。  
开发人员应当在给元素设定背景图的同时给元素指定背景色background-color，当背景图不可用时背景色替代。


## background-position 
指定背景图片的初始位置。
```CSS
{
/* Keyword values */
background-position: top;
background-position: bottom;
background-position: left;
background-position: right;
background-position: center;

/* <percentage> values */
background-position: 25% 75%;

/* <length> values */
background-position: 0 0;
background-position: 1cm 2cm;
background-position: 10ch 8em;

/* Multiple images */
background-position: 0 0, center;

/* Edge offsets values */
background-position: bottom 10px right 20px;
background-position: right 3em bottom 10px;
background-position: bottom 10px right;
background-position: top right 10px;

/* Global values */
background-position: inherit;
background-position: initial;
background-position: unset;
}
```

## background-repeat
定义背景图像的重复方式。背景图像可以沿着水平轴，垂直轴，两个轴重复，或者根本不重复。space和round很少用。
```css
{
background-repeat: repeat-x;
background-repeat: repeat-y;
background-repeat: repeat;
background-repeat: space;
background-repeat: round;
background-repeat: no-repeat;
}
```

## background-size
设置背景图片大小。图片可以保有其原有的尺寸，或者拉伸到新的尺寸，或者在保持其原有比例的同时缩放到元素的可用空间的尺寸。

```css
{
/* 关键字 */
/* cover缩放背景图片以完全覆盖背景区，可能背景图片部分看不见 */
background-size: cover;
/* 缩放背景图片以完全装入背景区，可能背景区部分空白。 */
background-size: contain;

/* 一个值: 这个值指定图片的宽度，图片的高度隐式的为auto */
background-size: 50%;
background-size: 3em;
background-size: 12px;
background-size: auto; /*默认大小*/

/* 两个值 */
/* 第一个值指定图片的宽度，第二个值指定图片的高度 */
background-size: 50% auto;
background-size: 3em 25%;
background-size: auto 6px;
background-size: auto auto;

/* 逗号分隔的多个值：设置多重背景 */
background-size: auto, auto;     /* 不同于background-size: auto auto */
background-size: 50%, 25%, 25%;
background-size: 6px, auto, contain;

/* 全局属性 */
background-size: inherit;
background-size: initial;
background-size: unset;
}
```