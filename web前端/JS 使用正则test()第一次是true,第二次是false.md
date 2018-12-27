# JS 使用正则test()第一次是true,第二次是false

[原文](http://www.phperz.com/article/17/0518/327396.html)

**JavaScript**客户端脚本语言
Javascript 是一种由Netscape的LiveScript发展而来的原型化继承的基于对象的动态类型的区分大小写的客户端脚本语言，主要目的是为了解决服务器端语言，比如Perl，遗留的速度问题，为客户提供更流畅的浏览效果。

这篇文章主要介绍了使用正则test( )第一次是 true,第二次是false的相关资料,需要的朋友可以参考下

## 1.前言

>今天朋友问我一个问题，我现在需要多次匹配同一个内容，但是为什么我第一次匹配，直接是 true，而第二次匹配确实 false 呢？

```javascript
var s1 = "MRLP";
var s2 = "MRLP";
var reg = /mrlp/ig;
console.log(reg.test(s1));
console.log(reg.test(s2));
```
这时候你会发现，我们在连续使用一个正则匹配其他字符串的时候，第一次匹配是 true，而第二次匹配则是 false。

等等，WHT？我匹配的是 MRLP，而且我还特意加上i 用于不区分大小写，可以为什么第一次可以正常匹配，第二次就不行了呢？

这也就是我今天要跟大家说的，关于 JS 中的 lastIndex。

## 2. lastIndex

在开始讲解之前，首先先带大家简单回顾一下 JS中正则表达式的使用方式。

JS 中正则表达式的使用方式有两种:

第一种是正则表达式对象的方法，常用方法有两个。

>exec(str) : 检索字符串中指定的值。返回找到的值，并确定其位置
test(str) : 检索字符串中指定的值。返回 true 或 false

第二种是字符串对象的方法，常用方法有四个。

>match(regexp) : 找到一个或多个正则表达式的匹配
replace(regexp) : 替换与正则表达式匹配的子串
search(regexp) : 检索与正则表达式相匹配的值
split(search) : 把字符串分割为字符串数组

而这些方法和咱们今天所说的 lastIndex 有什么关系呢？

lastIndex 属性用于规定下次匹配的起始位置。
上次匹配的结果是由方法 RegExp.exec( ) 和 RegExp.test( ) 找到的，它们都以 lastIndex 属性所指的位置作为下次检索的起始点。

这样，就可以通过反复调用这两个方法来遍历一个字符串中的所有匹配文本。
而且需要注意，该属性只有设置标志 **g**才能使用。
既然已经知道这个东西的形成原因，那么解决起来就非常简单了。

## 3.解决方案

### 3.1 第一种解决方案

如上面所述，我们 lastIndex 属性必须要设置 g 标签才能使用。

那么我们在匹配的时候，可以根据情况，直接去掉 g 标签就可以啦。

```javascript
var s1 = "MRLP";
var s2 = "MRLP";
var reg = /mrlp/i;
console.log(reg.test(s1)); //true
console.log(reg.test(s2)); //true
```
### 3.2 第二种解决方案

很多时候，我们必须要执行 全局匹配（ g ），这时候就不能使用第一种方案了。

其实，我们的lastIndex 属性是可读可写的。

只要目标字符串的下一次搜索开始，就可以对它进行设置。

当方法 exec() 或 test() 再也找不到可以匹配的文本时，它们会自动把 lastIndex 属性重置为 0。

这样，我们再次执行全局匹配的时候，就不会出现 false 的情况了。

```javascript
var s1 = "3206064928：MRLP：3206064928";
var s2 = "MRLP";
var reg = /mrlp/ig;
console.log(reg.test(s1)); //true
console.log(reg.lastIndex);  //reg.lastIndex = 15
reg.lastIndex = 0;     //这里我将 lastIndex 重置为 0
console.log(reg.test(s2)); //true
```

以上。


