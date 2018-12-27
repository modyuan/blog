# HTML属性转义

属性="属性值"

- 属性值外面是双引号的话里面可以使用单引号
- 反过来，外面是单引号里面可以使用双引号

但是如果想里外同时使用双引号，可以使用html转义
    
    onclick="alert(&quot;OK&quot;)"
也可以只在里面引号，不在外面写 比如
    
    onclick=alert("OK")
The HTML5 standard does not require quotes around attribute values.

双引号 `&#34; &#x22; &quot;`
单引号 `&#39; &#x27; &apos; `

不能像c、js一样使用反斜杠开头的转义。