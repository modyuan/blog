# Jedis 笔记

Jedis是redis在java上的一个客户端。



## 配置依赖

在maven上，可以这样：

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```



## 基本使用

单个的Jedis示例不是线程安全的。在程序中，应该使用JedisPool。

```java
JedisPool pool = new JedisPool(new JedisPoolConfig(), "localhost");
```

它可以存储在一个静态域中，它是线程安全的。



使用示例：

```java
/// Jedis implements Closeable. Hence, the jedis instance will be auto-closed after the last statement.
try (Jedis jedis = pool.getResource()) {
  /// ... do stuff here ... for example
  jedis.set("foo", "bar");
  String foobar = jedis.get("foo");
  jedis.zadd("sose", 0, "car"); jedis.zadd("sose", 0, "bike"); 
  Set<String> sose = jedis.zrange("sose", 0, -1);
}
/// ... when closing your application:
pool.close();
```



## 设置主从分配

### 启用复制

Redis主要采用主从架构。也就是说写操作会在主服务器进行，然后同步给从服务器。读操作一般会在从服务器上进行，这样就减轻了主服务器的压力。



要配置主从服务器，有两个方式：

1. 在配置文件中指定。

2. 给定一个jedis实例，可以通过以下调用实现。

    ```java
    jedis.slaveof("192.168.1.35", 6379); 
    ```

注意：Redis2.6 起， 从服务器默认是只读的，此时不能对从服务器发送写操作。

### 禁用复制 / 主服务器失效时，提升一个从服务器

当主服务器挂了时，我们可以提升一个从服务器为主服务器。

首先，我们应该尝试禁用对原先主服务器的复制。然后在几个从服务器中，启用对新的主服务器的复制。

```java
// 假设我们有1 2 3， 3个服务器，之前3号是主服务器。
// 现在3 号挂了。我们要把1号提升为主服务器。
slave1jedis.slaveofNoOne();
slave2jedis.slaveof("192.168.1.36", 6379);// 指向1号的ip和port。
```

