# Shiro中的哈希函数



相关函数都在`org.apache.shiro.crypto.hash`这个包下。

官方文档：https://shiro.apache.org/static/1.5.0/apidocs/org/apache/shiro/crypto/hash/package-summary.html



## Shiro中的hash函数具体行为

加盐方式为：

`hash(salt+password)`

如果迭代次数为2，则：

`hash(hash(salt+password))`

以此类推。

可以使用SimpleHash类来便捷的调用各种hash函数。使用示例为：

```java
String hashed = new SimpleHash("sha-256", rawPassword, salt).toHex();
```



**注意**：输入的参数salt和password如果不是byte[]会被转换成byte[]，String会以UTF-8的形式编码为byte[]，hash计算结果为byte[]形式。最后需要字符串的话要用toHex()或者toBase64()函数。





