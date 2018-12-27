```css
.clearfix::after{
    display:block;
    height:0;
    content:".";
    visibility:hidden;
    clear:both;
}
```
怎么使用这个CSS片段呢？
请看如下html：
```html
<section class="clearfix">
    <img src="images/rubber_duck.jpg" style="float:left ">
    <p>It's fun to float.</p>
</section>
<footer> Here is the footer element…</footer>
```
给包含浮动的父元素添加 clearfix类后，其后的元素就不会挤过来完美清除浮动。section就不会塌陷了。