# 探究 Content-Disposition：解决下载中文文件名乱码

今天解决了一个设置下载文件名为中文的问题：直接在Content-Disposition中设置中文会导致乱码。按照网上的办法（Content-Disposition + UTF-8）就搞定了。不过为了能搞清楚问题的关键所在，我还是去看了下官方文档，了解了下Content-Disposition的字段与意义。使用Content-Disposition可以设置文件名，但是要设置中文就需要进行编码，而RFC 822规定Message只能为ASCII，这就是问题所在。

## **Content-Disposition的定义**

[Hypertext Transfer Protocol – HTTP/1.1](http://www.rfc-editor.org/rfc/rfc2616.pdf)**中的描述**

Content-Disposition is not part of the HTTP standard, but since it is widely implemented, we are documenting its use and risks for implementors.

The Content-Disposition response-header field has been proposed as a means for the origin server to suggest a default filename if the user requests that the content is saved to a file. This usage is derived from the definition of Content-Disposition in **RFC 1806**.

[RFC 1806](http://www.rfc-editor.org/rfc/rfc1806.txt)**中的描述**

```
the Content-Disposition header field is defined as follows:
disposition := "Content-Disposition" ":"
disposition-type
*(";" disposition-parm)
disposition-type := "inline"
/ "attachment"
/ extension-token
; values are not case-sensitive
disposition-parm := filename-parm / parameter
filename-parm := "filename" "=" value;

```

‘extension-token’, ‘parameter’ and`’value’ are defined according to [**RFC 822**] and [**RFC 1521**].

**首先**要注意的是disposition-type。由上面给出的是Content-Disposition header 字段我们可以知道，Content-Disposition有两种type，即**inline**和**attachment**。根据文档中的介绍可知，**inline**类型会自动显示附件内容，比如显示一个图片；而**attachment**不会自动显示，在邮件中可能会显示为一个带图标的附件，在浏览器中可能会提示下载。

**其次**就是disposition-parm。主要作用就是提供一个建议的文件名（**filename-parm**），客户端（浏览器、邮件系统）在尽可能的情况下会以该文件名去保存文件。尽可能的意思是存在不一样的情况，比如文件名非法、存在同名文件，这些情况下客户端会采取一些措施，比如修改文件名。

**最后**看看filename-parm的value。这个value就是文件名（**本文的目的是给value设置一个中文**，主要的坑就是这里）。

通过上面的介绍，要给前端发送一个文件，并且定义文件的名字为中文，可以在发送文件之前返回这样一个HTTP Response Header ：

`Content-Disposition: attachment; filename=文件.txt`

**需要注意的是**，RFC 822（ Standard for ARPA Internet Text Messages）规定了文本消息只能为ASCII，因此这个Content-Disposition是非法的。RFC 1521（Multipurpose Internet Mail Extensions）基于前者对编码方式进行了拓展，使用了4种机制：

- MIME-Version header

- Content-Type header

- Content-Transfer-Encoding header

- Content-ID and Content-Description header

可惜这些与给HTTP Response Header中设置中文没啥关系。后来google了一下，通过stackoverflow找到了一个叫 [RFC 5987 - Character Set and Language Encoding for Hypertext Transfer Protocol (HTTP) Header Field Parameters](http://www.rfc-editor.org/rfc/rfc5987.txt) 的文档，顿时拨开迷雾见青天：

By default, message header field parameters in HTTP ([RFC2616]) messages cannot carry characters outside the ISO-8859-1 character set. RFC 2231 defines an encoding mechanism for use in MIME headers. This document specifies an encoding suitable for use in HTTP header fields that is compatible with a profile of the encoding defined in RFC 2231.

文中的Guidelines for Usage in HTTP Header Field Definitions给出了一个通用表达式：

```
foo-header = "foo" LWSP ":" LWSP token ";" LWSP title-param
title-param = "title" LWSP "=" LWSP value
/ "title*" LWSP "=" LWSP ext-value

ext-value = charset "'" [ language ] "'" value-chars
charset = "UTF-8" / "ISO-8859-1" / mime-charset
value-chars = *( pct-encoded / attr-char )
```



将前面非法的Content-Disposition转化过来就是：

`Content-Disposition : attachment; filename*=UTF-8''%E6%96%87%E4%BB%B6.txt`

这里对“文件.txt”进行了编码：先进行UTF-8编码，再进行pct-encoded编码。其实就是URL_ENCODE的过程。。。

## **Node中的用法**

除了使用filename*=UTF-8”+value外，还需要对中文进行编码。在node中的写法：

```js
let name = urlencode("文件.txt", "utf-8");
res.setHeader("Content-Disposition", "attachment; filename*=UTF-8''"+name);
```

注意⚠️ 这里是filename*=UTF8单引号单引号

亲测在Chrome、Edge、IE 11 下有效。