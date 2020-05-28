# WEB认证方式

[TOC]

## HTTP基本认证(Basic Authentication)

用状态码和http头来要求认证。

当访问需要认证的资源时，服务器返回状态码`401 Unauthorized`，

同时header设置`WWW-Authenticate: Basic realm="input username and password"`

这里只有`401`没有`WWW-Authenticate`是不行的。当然realm=后面的提示字段是可以自定义的。



这个时候，浏览器就会弹出一个登录窗口提示输入用户名和密码。浏览器会把用户名和密码以base64形式编码，放在HTTP头中，再发送给服务器：

如果用js的语法来形容，是这样

```js
setHeader("Authorization", `Basic ${username}:${password}`);
```

这时服务器如果验证通过，就可以正常返回资源了，之后服务器就可以存储session，并且把sessionID设置给cookie。

优点：

- 简单

缺点：

- 密码明文传输



## JWT - 基于Token认证的代表

服务器不存储session，只负责鉴别客户端的token真假，然后根据客户端的token来做身份识别。

一个jwt实际上就是一个字符串，它由三部分组成，**头部**、**载荷**与**签名**，这三个部分都是json格式。给用户的时候是通过base64编码，然后三部分之间用点`.`分割开的。

### 1. 头部

描述基本信息（本身的类型以及签名所用的算法）。

```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```
这里算法有一般有两个选择：一个采用HS256（对称加密），另外一个就是采用RS256（非对称加密）。基本的方法是把message和key进行进行填充操作后，然后对它们是用散列函数（SHA-256等），最终得到一个签名。

具体来说，伪代码实现如下（来自wiki）：

```js
 function hmac (key, message) {
    if (length(key) > blocksize) {
        key = hash(key) // keys longer than blocksize are shortened
    }
    if (length(key) < blocksize) {
        // keys shorter than blocksize are zero-padded (where ∥ is concatenation)
        key = key ∥ [0x00 * (blocksize - length(key))] // Where * is repetition.
    }
   
    o_key_pad = [0x5c * blocksize] ⊕ key // Where blocksize is that of the underlying hash function
    i_key_pad = [0x36 * blocksize] ⊕ key // Where ⊕ is exclusive or (XOR)
   
    return hash(o_key_pad ∥ hash(i_key_pad ∥ message)) // Where ∥ is concatenation
}
```

### 2. 载荷

用来放一些以前被放在session中的，同时**不敏感**的信息。(注意：因为jwt是明文，所以载荷不能存敏感信息，如密码等)

```json
{
    "iss": "John Wu JWT",
    "iat": 1441593502,
    "exp": 1441594722,
    "aud": "www.example.com",
    "sub": "jrocket@example.com",
    "from_user": "B",
    "target_user": "A"
}
```

这里面的前五个字段都是由JWT的标准所定义的。

- `iss`: 该JWT的签发者
- `sub`: 该JWT所面向的用户
- `aud`: 接收该JWT的一方
- `exp`(expires): 什么时候过期，这里是一个Unix时间戳
- `iat`(issued at): 在什么时候签发的

### 3. 签名

这部分是对上面两部分组成的base64编码字符串的一个加密（之前声明的方法）。代码表示的话，大概是这样：

```js
sig = Encryp(base64Encode(headJson)+"."+base64Encode(payloadJson), key)
```

最后，把三部分的base64编码后的字符串连起来，用点分割，就形成了最终的jwt。



### 服务器如何验证呢？

服务器可以也对前两部分以及key是用签名的算法进行计算，如果结果和客户端传过来的签名一致，那说明这个jwt是自己颁发的，否则就不是。

客户端由于不知道key，所以不能伪造签名。客户端如果更改载荷，服务器验证会不通过，所以客户端也不能伪造载荷。



### JWT应用

一般是在请求头里加入`Authorization`，并加上`Bearer`标注：

```js
fetch('api/user/1', {
  headers: {
    'Authorization': 'Bearer ' + token
  }
})
```

每次请求都要带上token。也就是说，使用jwt的话，就完全不用cookie了。验证通过后，服务器会信任token中的载荷里面的信息。



优点：

- 服务器无需存储session，方便水平扩展。无需多机共享session。
- 无状态

缺点：

- 无法吊销令牌，只能设置有效期。需要立刻吊销的话还需要维护一个黑名单，那就和session方案差不多了。
- 续签怎么续？每次重新发一个新的？



所以适合jwt的场景是：

- 有效期短
- 只希望被使用一次



## OAuth

授权本网站权限给其他网站，同时不暴露本网站的用户名密码，并且权限可撤销。它的简化版是OpenID，只能验证身份，不能进行授权。OAuth也可以用于单点登录（SSO）。

OAuth有三个版本 1.0  1.0a  2.0 ，其中1.0因为有漏洞而停止使用。2.0相对1.0a进行了一定的简化。







## 多机Session共享

- 附着机制，让客户端访问指定的带有它session的服务器
- 外部存储Session，如redis



## SSO（Single Sign On）单点登录

