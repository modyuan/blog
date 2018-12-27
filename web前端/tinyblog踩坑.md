# tinyblog踩坑

1. 当cookie设为HttpOnly时，客户端js的ajax请求不会被自动附加cookie信息。
2. 在ajax中附带cookie信息需要额外设置。
```js
//第一次发送认证：
window.fetch("http://127.0.0.1/api/login",
{method:"POST",
 body:"user=yuansu&passwd=123456",
 credentials: 'include'})
.then(res=>console.log(res.json()))
.catch(err=>console.log(err))
//第二次发送请求
window.fetch('/api/getArticleList',
{credentials:"include"})
.then(res=>console.log(res.json()))

```
只有加了`credentials:"include"`才会发送到第一次服务器返回的cookie。（或者same-origin也行）

第一次如果不加credentials，则服务器返回的setcookie将被忽略。

第二次不加credentials，则发送的请求里面不会包含Cookie。

3. cookie存在一个path属性，如果不设置，默认就是请求时的路径。<br>
例如在/api/login下面发送了登录请求，然后返回了一个cookie（uuid:xxxxxxx），那么在网站根目录下的js是访问不到document.cookie的！！！
所以需要在返回cookie时设置 `path=/` 这一属性才行。

