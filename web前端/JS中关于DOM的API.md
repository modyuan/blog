# JS中关于DOM的API

## history
主要用于前进和后退。
```
history.back()
history.forward()
histroy.go() //-1 = back, 1 = forward
```

-----
## location
掌控当前的URL
其属性可以获取URL的各个部分，也可以设置。对href进行设置可以跳转其他URL。
reload() 可以重新加载页面。replace() 和 assign() 可以载入新的页面。

-----
## navigator
导航器对象，返回一些浏览器以及操作系统的信息。比如浏览器平台及版本、操作系统版本、浏览器是否启用cookie等。


-----
## screen 
显示器对象，返回屏幕的宽和高（width、height）以及扣除半永久占用空间（任务栏等）的可用屏幕大小（availWidth、availHeight）。
以上大小均为扣除DPI缩放后的大小。例如一个DPI缩放设置为1.5的系统，获取到的width会小于等于实际分辨率宽度除以1.5。
如果浏览器本身又设置了缩放，那么还会有相应的变化。

------
## DOM document
每个载入浏览器的 HTML 文档都会成为 Document 对象。
Document 对象使我们可以从脚本中对 HTML 页面中的所有元素进行访问。
### document对象集合
|集合|描述|
|----|----|
|all[] |  提供对文档中所有 HTML 元素的访问 |
|anchors[] | 返回对文档中所有 Anchor 对象的引用 |
| forms[] | 返回对文档中所有 Form 对象引用 |
|images[] |  返回对文档中所有 Image 对象引用 |
|links[] | 返回对文档中所有 Area 和 Link 对象引用 |

### document对象属性
- body
- cookie
- domain
- lastModified
- referrer
- titile
- URL

### document对象方法
- open()  close()  write()  writeln()     直接在文档输出流写入。
- getElementById() getElementsByName()  getElementsByTagName() getElementsByClassName()  
根据相应的要求获取对象。
- getElementsByTagNameNS() 只有使用命名空间的 XML 文档才会使用它。
- querySelector() querySelectorAll() 采用CSS选择器的语法检索对象。

-------
## DOM Element 对象
| 属性 / 方法 | 描述 |
|-----|-----|
| element.accessKey | 设置或返回元素的快捷键。 |
| element.appendChild() | 向元素添加新的子节点，作为最后一个子节点。 |
| element.attributes | 返回元素属性的 NamedNodeMap。 |
| element.childNodes | 返回元素子节点的 NodeList。 |
| element.className | 设置或返回元素的 class 属性。 |
| element.clientHeight | 返回元素的可见高度。 |
| element.clientWidth | 返回元素的可见宽度。 |
| element.cloneNode() | 克隆元素。 |
| element.compareDocumentPosition() | 比较两个元素的文档位置。 |
|element.dir | 设置或返回元素的文本方向。 |
| element.contentEditable | 设置或返回元素的内容是否可编辑。 |
| element.firstChild | 返回元素的首个子节点。 |
| element.getAttribute() | 返回元素节点的指定属性值。 |
| element.getAttributeNode() | 返回指定的属性节点。 |
| element.getElementsByTagName() | 返回拥有指定标签名的所有子元素的集合。 |
| element.getFeature() | 返回实现了指定特性的 API 的某个对象。 |
| element.getUserData() | 返回关联元素上键的对象。 |
| element.hasAttribute() | 如果元素拥有指定属性，则返回true，否则返回 false。 |
| element.hasAttributes() | 如果元素拥有属性，则返回 true，否则返回 false。 |
| element.hasChildNodes() | 如果元素拥有子节点，则返回 true，否则 false。 |
| element.id | 设置或返回元素的 id。 |
| element.innerHTML | 设置或返回元素的内容。 |
| element.insertBefore() | 在指定的已有的子节点之前插入新节点。 |
| element.isContentEditable | 设置或返回元素的内容。 |
| element.isDefaultNamespace() | 如果指定的 namespaceURI 是默认的，则返回 true，否则返回 false。 |
| element.isEqualNode() | 检查两个元素是否相等。 |
| element.isSameNode() | 检查两个元素是否是相同的节点。 |
| element.isSupported() | 如果元素支持指定特性，则返回 true。 |
| element.lang | 设置或返回元素的语言代码。 |
| element.lastChild | 返回元素的最后一个子元素。 |
| element.namespaceURI | 返回元素的 namespace URI。 |
| element.nextSibling | 返回位于相同节点树层级的下一个节点。 |
| element.nodeName | 返回元素的名称。 |
| element.nodeType | 返回元素的节点类型。 |
| element.nodeValue | 设置或返回元素值。 |
| element.normalize() | 合并元素中相邻的文本节点，并移除空的文本节点。 |
| element.offsetHeight | 返回元素的高度。 |
| element.offsetWidth | 返回元素的宽度。 |
| element.offsetLeft | 返回元素的水平偏移位置。 |
| element.offsetParent | 返回元素的偏移容器。 |
| element.offsetTop | 返回元素的垂直偏移位置。 |
| element.ownerDocument | 返回元素的根元素（文档对象）。 |
| element.parentNode | 返回元素的父节点。 |
| element.previousSibling | 返回位于相同节点树层级的前一个元素。 |
| element.removeAttribute() | 从元素中移除指定属性。 |
| element.removeAttributeNode() | 移除指定的属性节点，并返回被移除的节点。 |
| element.removeChild() | 从元素中移除子节点。 |
| element.replaceChild() | 替换元素中的子节点。 |
| element.scrollHeight | 返回元素的整体高度。 |
| element.scrollLeft | 返回元素左边缘与视图之间的距离。 |
| element.scrollTop | 返回元素上边缘与视图之间的距离。 |
| element.scrollWidth | 返回元素的整体宽度。 |
| element.setAttribute() | 把指定属性设置或更改为指定值。 |
| element.setAttributeNode() | 设置或更改指定属性节点。 |
| element.setIdAttribute() |  |
| element.setIdAttributeNode() |  |
| element.setUserData() | 把对象关联到元素上的键。 |
| element.style | 设置或返回元素的 style 属性。 |
| element.tabIndex | 设置或返回元素的 tab 键控制次序。 |
| element.tagName | 返回元素的标签名。 |
| element.textContent | 设置或返回节点及其后代的文本内容。 |
| element.title | 设置或返回元素的 title 属性。 |
| element.toString() | 把元素转换为字符串。 |
| nodelist.item() | 返回 NodeList 中位于指定下标的节点。 |
| nodelist.length | 返回 NodeList 中的节点数。 |

------
## DOM Event（DOM 事件）
### 鼠标事件
| 属性|描述|DOM |
| ---| ---|----|
| onclick|当用户点击某个对象时调用的事件句柄。|2 |
| oncontextmenu|在用户点击鼠标右键打开上下文菜单时触发|  |
| ondblclick|当用户双击某个对象时调用的事件句柄。|2 |
| onmousedown|鼠标按钮被按下。|2 |
| onmouseenter|当鼠标指针移动到元素上时触发。|2 |
| onmouseleave|当鼠标指针移出元素时触发|2 |
| onmousemove|鼠标被移动。|2 |
| onmouseover|鼠标移到某元素之上。|2 |
| onmouseout|鼠标从某元素移开。|2 |
| onmouseup|鼠标按键被松开。|2 |

### 键盘事件
| 属性|描述|DOM |
| ----|----|----|
| onkeydown|某个键盘按键被按下。|2 |
| onkeypress|某个键盘按键被按下并松开。|2 |
| onkeyup|某个键盘按键被松开。|2 |


### 框架/对象（Frame/Object）事件
| 属性|描述|DOM |
| ----|----|---- |
| onabort|图像的加载被中断。 ( `<object>`)|2 |
| onbeforeunload|该事件在即将离开页面（刷新或关闭）时触发|2 |
| onerror|在加载文档或图像时发生错误。 (`<object>`, `<body>`和 `<frameset>`)|  |
| onhashchange|该事件在当前 URL 的锚部分发生修改时触发。|  |
| onload|一张页面或一幅图像完成加载。|2 |
| onpageshow|该事件在用户访问页面时触发| |
| onpagehide|该事件在用户离开当前网页跳转到另外一个页面时触发| |
| onresize|窗口或框架被重新调整大小。|2 |
| onscroll|当文档被滚动时发生的事件。|2 |
| onunload|用户退出页面。 ( `<body>` 和 `<frameset>`)|2 |

`DOMContentLoaded`事件是jquery中jq.ready的底层实现。
### 表单事件
| 属性|描述|DOM |
| ----|----|---- |
| onblur|元素失去焦点时触发|2 |
| onchange|该事件在表单元素的内容改变时触发( `<input>`, `<keygen>`, `<select>`, 和 `<textarea>`)|2 |
| onfocus|元素获取焦点时触发|2 |
| onfocusin|元素即将获取焦点时触发|2 |
| onfocusout|元素即将失去焦点时触发|2 |
| oninput|元素获取用户输入时触发|3 |
| onreset|表单重置时触发|2 |
| onsearch|用户向搜索域输入文本时触发 ( `<input="search">`)|  |
| onselect|用户选取文本时触发 ( `<input>` 和 `<textarea>`)|2 |
| onsubmit|表单提交时触发|2 |

### 剪贴板事件
| 属性|描述|DOM |
| ----|----|---- |
| oncopy|该事件在用户拷贝元素内容时触发|  |
| oncut|该事件在用户剪切元素内容时触发|  |
| onpaste|该事件在用户粘贴元素内容时触发|。  |

### 打印事件
| 属性|描述|DOM |
| ----|----|---- |
| onafterprint|该事件在页面已经开始打印，或者打印窗口已经关闭时触发|  |
| onbeforeprint|该事件在页面即将开始打印时触发|  |

### 拖动事件

| 事件|描述|DOM |
| ----|----|---- |
| ondrag|该事件在元素正在拖动时触发|  |
| ondragend|该事件在用户完成元素的拖动时触发|  |
| ondragenter|该事件在拖动的元素进入放置目标时触发|  |
| ondragleave|该事件在拖动元素离开放置目标时触发|  |
| ondragover|该事件在拖动元素在放置目标上时触发|  |
| ondragstart|该事件在用户开始拖动元素时触发|  |
| ondrop|该事件在拖动元素放置在目标区域时触发| &nbsp;|

[参考链接](http://www.runoob.com/jsref/dom-obj-event.html)

------
## window
所有全局对象均是window的成员，除了以上的几个大类外，还有一些全局下的属性。
### 控制窗口系列
moveTo moveBy resizeTo resizeBy 移动以及设置窗口大小，但是chrome\FF上均无效。IE/Edge 下有效。
scrollTo scrollBy 控制滚动条。
### 打开窗口
open(...) 甚至可以打开一个独立的窗口，并定制是否有地址栏等属性。
close() 关闭当前页。但是不能关闭不是脚本打开的窗口（chrome/FF）或者会有提示框询问（edge/IE）
closed 返回window对象对应的窗口是否被关闭。
### 定时器
| &nbsp; |启动|取消|
|----|-----|----|
|一次性|setTimeOut| clearTimeOut|
|多次性 |setInterval | clearInterval|

### 对话框
alert 提示信息
confirm 确认信息
prompt 输入内容

### 窗口大小及位置的获取
类似screen对象，不过这里是全局的。只读属性。
- screenLeft screenTop 获取位置（FF不可用）
- screenX screenY 也是获取位置作用同上。

- innerWidth innerHeight 内部宽高。
这里的宽度和高度不包括菜单栏、工具栏以及滚动条等的高度。IE 不支持这两个属性。它用 document.documentElement 或 document.body （与 IE 的版本相关）的 clientWidth 和 clientHeight 属性作为替代。
- outerWidth outerHeight 外部宽高。IE不支持这个属性且没有替代的。
以上四个也是扣除DPI缩放之后的。

- pageXOffset和pageYOffset，是滚动条的位置。




