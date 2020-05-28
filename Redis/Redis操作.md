# Redis操作

## 简介

REmote DIctionary Server(Redis) 是一个由Salvatore Sanfilippo写的key-value存储系统。

Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Hash), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。

Redis 与其他 key - value 缓存产品有以下三个特点：

- Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持数据的备份，即master-slave模式的数据备份。

## Redis优势

- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

## 配置

可以通过修改配置文件来修改，也可以连接上之后使用CONFIG命令来修改。

在运行redis-server时，要指定redis的配置文件位置。



## 运行Redis

对于服务器，程序名为redis-server，运行时应该指定redis的配置文件位置。

对于客户端，可以使用自带的redis-cli，也可以使用其他语言封装的库。

比如java的jedis。

##数据类型

Redis支持五种数据类型：

- string（字符串）
- hash（哈希）
- list（列表）
- set（集合）
- zset（sorted set：有序集合）



## 字符串操作

### `SET key value [EX seconds] [PX milliseconds] [NX|XX]`

将字符串值 `value` 关联到 `key` 。

如果 `key` 已经持有其他值， `SET` 就覆写旧值， 无视类型。

当 `SET` 命令对一个带有生存时间（TTL）的键进行设置之后， 该键原有的 TTL 将被清除。

可选参数：

从 Redis 2.6.12 版本开始， `SET` 命令的行为可以通过一系列参数来修改：

- `EX seconds` ： 将键的过期时间设置为 `seconds` 秒。 执行 `SET key value EX seconds` 的效果等同于执行 `SETEX key seconds value` 。
- `PX milliseconds` ： 将键的过期时间设置为 `milliseconds` 毫秒。 执行 `SET key value PX milliseconds` 的效果等同于执行 `PSETEX key milliseconds value` 。
- `NX` ： 只在键不存在时， 才对键进行设置操作。 执行 `SET key value NX` 的效果等同于执行 `SETNX key value` 。
- `XX` ： 只在键已经存在时， 才对键进行设置操作。



### `GET key`

如果键 `key` 不存在， 那么返回特殊值 `nil` ； 否则， 返回键 `key` 的值。

如果键 `key` 的值并非字符串类型， 那么返回一个错误， 因为 `GET` 命令只能用于字符串值。



### `DEL key [key …]`

删除给定的一个或多个 `key` 。

不存在的 `key` 会被忽略。

返回值: 被删除 `key` 的数量。



### `GETSET key value`

将键 `key` 的值设为 `value` ， 并返回键 `key` 在被设置之前的旧值。

返回值：

> 返回给定键 `key` 的旧值。
>
> 如果键 `key` 没有旧值， 也即是说， 键 `key` 在被设置之前并不存在， 那么命令返回 `nil` 。
>
> 当键 `key` 存在但不是字符串类型时， 命令返回一个错误。



###  `STRLEN key`


返回键 `key` 储存的字符串值的长度。

返回值

> `STRLEN` 命令返回字符串值的长度。
>
> 当键 `key` 不存在时， 命令返回 `0` 。
>
> 当 `key` 储存的不是字符串值时， 返回一个错误。



### `APPEND key value`

如果键 `key` 已经存在并且它的值是一个字符串， `APPEND` 命令将把 `value` 追加到键 `key` 现有值的末尾。

如果 `key` 不存在， `APPEND` 就简单地将键 `key` 的值设为 `value` ， 就像执行 `SET key value` 一样。

返回值: 追加 `value` 之后， 键 `key` 的值的长度。



### `SETRANGE key offset value`

从偏移量 `offset` 开始， 用 `value` 参数覆写(overwrite)键 `key` 储存的字符串值。

不存在的键 `key` 当作空白字符串处理。返回: 被修改之后， 字符串值的长度。



### `GETRANGE key start end`

返回键 `key` 储存的字符串值的指定部分， 字符串的截取范围由 `start` 和 `end` 两个偏移量决定 

(闭区间，0-base)。

负数偏移量表示从字符串的末尾开始计数， `-1` 表示最后一个字符， `-2` 表示倒数第二个字符， 以此类推。



### `INCR key`

执行加一操作，key不存在视作0，再加一。

### `INCRBY key increment`

执行加N操作，key不存在视作0，再加N。

### `INCRBYFLOAT key increment`

类似INCRBY，但是increment是浮点数。



### `DECR`、 `DECRBY`

减法版的INCR。



### `MSET key value [key value …]`

同时设置多个key。它是一个原子性(atomic)操作， 所有给定键都会在同一时间内被设置， 不会出现某些键被设置了但是另一些键没有被设置的情况。

### `MSETNX key value [key value …]`

当且仅当所有给定键都不存在时， 为所有给定键设置值。

即使只有一个给定键已经存在， `MSETNX` 命令也会拒绝执行对所有键的设置操作。



### `MGET key [key …]`

同时获取多个key



## Hash操作

### `HSET hash field value`

将哈希表 `hash` 中域 `field` 的值设置为 `value` 。

如果给定的哈希表并不存在， 那么一个新的哈希表将被创建并执行 `HSET` 操作。

如果域 `field` 已经存在于哈希表中， 那么它的旧值将被新值 `value` 覆盖。

返回值: 

> 当 `HSET` 命令在哈希表中新创建 `field` 域并成功为它设置值时， 命令返回 `1` ； 
>
> 如果域 `field` 已经存在于哈希表， 并且 `HSET` 命令成功使用新值覆盖了它的旧值， 那么命令返回 `0` 。



### `HSETNX hash field value`

当且仅当域 `field` 尚未存在于哈希表的情况下， 将它的值设置为 `value` 。

如果给定域已经存在于哈希表当中， 那么命令将放弃执行设置操作。



### `HGET hash field`

返回哈希表中给定域的值。



### `HEXISTS hash field`

检查给定域 `field` 是否存在于哈希表 `hash` 当中。

`HEXISTS` 命令在给定域存在时返回 `1` ， 在给定域不存在时返回 `0` 。



### `HDEL key field [field …]`

删除哈希表 `key` 中的一个或多个指定域，不存在的域将被忽略。返回删除成功的filed的个数。



### `HLEN key`

返回哈希表 `key` 中域的数量。不存在时返回0。



### `HSTRLEN key field`

返回哈希表 `key` 中， 与给定域 `field` 相关联的值的字符串长度（string length）。

如果给定的键或者域不存在， 那么命令返回 `0` 。



### `HINCRBY key field increment`

为哈希表 `key` 中的域 `field` 的值加上增量 `increment` 。



### `HINCRBYFLOAT`  `HMSET`  `HMGET`

类似的操作。



### `HKEYS key`

返回哈希表 `key` 中的所有域。



### `HVALS key`

返回哈希表 `key` 中所有域的值。



### `HGETALL key`

返回哈希表 `key` 中，所有的域和值。





## 列表操作



### `LPUSH key value [value …]`

各个 `value` 值按从左到右的顺序依次插入到表头。原子操作。当key不存在时创建一个空列表。

LPUSHX类似，但当key不存在时不执行任何操作。

RPUSH、RPUSHX类似。



### `LPOP key` /  `RPOP key`

从左/右返回并删除元素。





### `RPOPLPUSH source destination`

一个原子时间内，执行以下两个动作：

- 将列表 `source` 中的最后一个元素(尾元素)弹出，并返回给客户端。
- 将 `source` 弹出的元素插入到列表 `destination` ，作为 `destination` 列表的的头元素。

返回被弹出的元素。

用途：

- 可以用作队列
- src=dst，可以循环获取元素。



### `LREM key count value`

根据参数 `count` 的值，移除列表中与参数 `value` 相等的元素。

`count` 的值可以是以下几种：

- `count > 0` : 从表头开始向表尾搜索，移除与 `value` 相等的元素，数量为 `count` 。
- `count < 0` : 从表尾开始向表头搜索，移除与 `value` 相等的元素，数量为 `count` 的绝对值。
- `count = 0` : 移除表中所有与 `value` 相等的值。



### `LLEN key`

返回列表 `key` 的长度。

如果 `key` 不存在，则 `key` 被解释为一个空列表，返回 `0` .

如果 `key` 不是列表类型，返回一个错误。



### `LINDEX key index`

返回列表 `key` 中，下标为 `index` 的元素。index可以为负数。



### `LINSERT key BEFORE|AFTER pivot value`

将值 `value` 插入到列表 `key` 当中，位于值 `pivot` 之前或之后。

当 `pivot` 不存在于列表 `key` 时，不执行任何操作。

当 `key` 不存在时， `key` 被视为空列表，不执行任何操作。

如果 `key` 不是列表类型，返回一个错误。

返回值

如果命令执行成功，返回插入操作完成之后，列表的长度。 如果没有找到 `pivot` ，返回 `-1` 。 如果 `key` 不存在或为空列表，返回 `0` 。



### `LSET key index value`

将列表 `key` 下标为 `index` 的元素的值设置为 `value` 。

当 `index` 参数超出范围，或对一个空列表( `key` 不存在)进行 [LSET](http://redisdoc.com/list/lset.html#lset) 时，返回一个错误。



### `LRANGE key start stop`

返回列表 `key` 中指定区间内的元素，区间以偏移量 `start` 和 `stop` 指定。闭区间，0-base



### `LTRIM key start stop`

对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。



### `BLPOP key [key …] timeout`

[BLPOP](http://redisdoc.com/list/blpop.html#blpop) 是列表的阻塞式(blocking)弹出原语。

它是 [LPOP key](http://redisdoc.com/list/lpop.html#lpop) 命令的阻塞版本，当给定列表内没有任何元素可供弹出的时候，连接将被 [BLPOP](http://redisdoc.com/list/blpop.html#blpop) 命令阻塞，直到等待超时或发现可弹出元素为止。



### `BRPOP`、` BRPOPLPUSH `



## 集合操作



### `SADD key member [member …]`

将一个或多个 `member` 元素加入到集合 `key` 当中，已经存在于集合的 `member` 元素将被忽略。

假如 `key` 不存在，则创建一个只包含 `member` 元素作成员的集合。



### `SISMEMBER key member`

判断 `member` 元素是否集合 `key` 的成员。

如果 `member` 元素是集合的成员，返回 `1` 。 如果 `member` 元素不是集合的成员，或 `key` 不存在，返回 `0` 。



### `SPOP key`

移除并返回集合中的一个随机元素。当 `key` 不存在或 `key` 是空集时，返回 `nil` 。



### `SRANDMEMBER key [count]`

如果命令执行时，只提供了 `key` 参数，那么返回集合中的一个随机元素（并不移除）。

- 如果 `count` 为正数，且小于集合基数，那么命令返回一个包含 `count` 个元素的数组，数组中的元素**各不相同**。如果 `count` 大于等于集合基数，那么返回整个集合。
- 如果 `count` 为负数，那么命令返回一个数组，数组中的元素**可能会重复出现多次**，而数组的长度为 `count` 的绝对值。



### `SREM key member [member …]`

移除集合 `key` 中的一个或多个 `member` 元素，不存在的 `member` 元素会被忽略。

当 `key` 不是集合类型，返回一个错误。返回成功移除的元素数量。



### `SMOVE source destination member`

将 `member` 元素从 `source` 集合移动到 `destination` 集合。

`SMOVE` 是原子性操作。

如果 `source` 集合不存在或不包含指定的 `member` 元素，则 [SMOVE](http://redisdoc.com/set/smove.html#smove) 命令不执行任何操作，仅返回 `0` 。否则， `member` 元素从 `source` 集合中被移除，并添加到 `destination` 集合中去并返回1。



### `SCARD key`

返回集合 `key` 的基数(集合中元素的数量)。 当 `key` 不存在时，返回 `0` 。



### `SMEMBERS key`

返回集合 `key` 中的所有成员。不存在的 `key` 被视为空集合。



### `SINTER key [key …]`

返回一个集合的全部成员，该集合是所有给定集合的交集。不存在的 `key` 被视为空集。

当给定集合当中有一个空集时，结果也为空集(根据集合运算定律)。



### `SINTERSTORE destination key [key …]`

这个命令类似于 `SINTER`命令，但它将结果保存到 `destination` 集合，而不是简单地返回结果集。

如果 `destination` 集合已经存在，则将其覆盖。`destination` 可以是 `key` 本身。



### `SUNION key [key …]`

返回一个集合的全部成员，该集合是所有给定集合的并集。不存在的 `key` 被视为空集。

SUNIONSTORE类似。

### `SDIFF key [key …]`

返回一个集合的全部成员，该集合是所有给定集合之间的差集。不存在的 `key` 被视为空集。

SDIFFSTORE类似



## 有序集合操作

有序集合和集合类似，但是元素多了score属性。

### `ZADD key score member [[score member] … ]`

将一个或多个 `member` 元素及其 `score` 值加入到有序集 `key` 当中。score是整数或者双精度浮点数



### `ZSCORE key member`

返回有序集 `key` 中，成员 `member` 的 `score` 值。

如果 `member` 元素不是有序集 `key` 的成员，或 `key` 不存在，返回 `nil` 。



### `ZINCRBY key increment member`

为有序集 `key` 的成员 `member` 的 `score` 值加上增量 `increment` 。



### `ZCARD key`

返回有序集 `key` 的基数。key不存在时返回0。



### `ZCOUNT key min max`

返回有序集 `key` 中， `score` 值在 [`min`, `max`] 的成员的数量。



### `ZRANGE key start stop [WITHSCORES]`

返回有序集 `key` 中，指定区间内的成员。

其中成员的位置按 `score` 值递增(从小到大)来排序。具有相同 `score` 值的成员按字典序来排列。

如果需要按score递减，可以使用ZREVRANGE命令，具有相同 `score` 值的成员按字典序逆序来排列。

可以通过使用 `WITHSCORES` 选项，来让成员和它的 `score` 值一并返回，返回列表以 `value1,score1, ..., valueN,scoreN` 的格式表示。

start stop指定有序集中元素的范围。可以使用负数。



### `ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]`

和ZRANGE类似，但是不是指定序数，而是指定score的范围。

min max 可以是-inf +inf。

默认情况下，区间的取值使用[闭区间](http://zh.wikipedia.org/wiki/區間) (小于等于或大于等于)，你也可以通过给参数前增加 `(` 符号来使用可选的[开区间](http://zh.wikipedia.org/wiki/區間) (小于或大于)。

类似地，还有ZREVRANGEBYSCORE命令。



### `ZRANK key member`

返回有序集 `key` 中成员 `member` 的排名。其中有序集成员按 `score` 值递增(从小到大)顺序排列。

排名以 `0` 为底，也就是说， `score` 值最小的成员排名为 `0` 。

使用 [ZREVRANK key member](http://redisdoc.com/sorted_set/zrevrank.html#zrevrank) 命令可以获得成员按 `score` 值递减(从大到小)排列的排名。



### `ZREM key member [member …]`

移除有序集 `key` 中的一个或多个成员，不存在的成员将被忽略

### ZREMRANGEBYRANK key start stop

移除有序集 `key` 中，指定排名(rank)(按score从小到大排)区间内的所有成员。start stop可以为负数。



### ZREMRANGEBYSCORE key min max

移除有序集 `key` 中，所有 `score` 值介于 `min` 和 `max` 之间(包括等于 `min` 或 `max` )的成员。



### ZRANGEBYLEX key min max [LIMIT offset count]

当有序集合的所有成员都具有相同的分值时， 有序集合的元素会根据成员的字典序（lexicographical ordering）来进行排序， 而这个命令则可以返回给定的有序集合键 `key` 中， 值介于 `min` 和 `max` 之间的成员。

合法的 `min` 和 `max` 参数必须包含 `(` 或者 `[` ， 其中 `(` 表示开区间（指定的值不会被包含在范围之内）， 而 `[` 则表示闭区间（指定的值会被包含在范围之内）。

特殊值 `+` 和 `-` 在 `min` 参数以及 `max` 参数中具有特殊的意义， 其中 `+` 表示正无限， 而 `-` 表示负无限。 

### ZLEXCOUNT key min max

对于一个所有成员的分值都相同的有序集合键 `key` 来说， 这个命令会返回该集合中， 成员介于 `min` 和 `max` 范围内的元素数量。

### ZREMRANGEBYLEX key min max

对于一个所有成员的分值都相同的有序集合键 `key` 来说， 这个命令会移除该集合中， 成员介于 `min` 和 `max` 范围内的所有元素。

### ZUNIONSTORE destination numkeys key [key …] [WEIGHTS weight [weight …]] [AGGREGATE SUM|MIN|MAX]

计算给定的一个或多个有序集的并集，其中给定 `key` 的数量必须以 `numkeys` 参数指定，并将该并集(结果集)储存到 `destination` 。

默认情况下，结果集中某个成员的 `score` 值是所有给定集下该成员 `score` 值之 *和* 。

#### WEIGHTS

使用 `WEIGHTS` 选项，你可以为 *每个* 给定有序集 *分别* 指定一个乘法因子(multiplication factor)，每个给定有序集的所有成员的 `score` 值在传递给聚合函数(aggregation function)之前都要先乘以该有序集的因子。

如果没有指定 `WEIGHTS` 选项，乘法因子默认设置为 `1` 。

#### AGGREGATE

使用 `AGGREGATE` 选项，你可以指定并集的结果集的聚合方式。

默认使用的参数 `SUM` ，可以将所有集合中某个成员的 `score` 值之 *和* 作为结果集中该成员的 `score` 值；使用参数 `MIN` ，可以将所有集合中某个成员的 *最小* `score` 值作为结果集中该成员的 `score` 值；而参数 `MAX` 则是将所有集合中某个成员的 *最大* `score` 值作为结果集中该成员的 `score` 值。

### ZINTERSTORE destination numkeys key [key …] [WEIGHTS weight [weight …]] [AGGREGATE SUM|MIN|MAX]

交集，类似ZUNIONSTORE



## 其他类型

- HyperLogLog
- 地理位置
- 位图



## 数据库操作



## 自动过期操作

### EXPIRE key seconds

设置自动过期时间。

PEXPIRE类似，但是单位是毫秒。



### EXPIREAT key timestamp

`EXPIREAT` 的作用和 `EXPIRE` 类似，都用于为 `key` 设置生存时间。

不同在于 `EXPIREAT` 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。以秒为单位。

PEXPIREAT类似，但是设置毫秒单位的时间戳。



### TTL key

以秒为单位，返回给定 `key` 的剩余生存时间(TTL, time to live)。

当 `key` 不存在时，返回 `-2` 。 当 `key` 存在但没有设置剩余生存时间时，返回 `-1` 。 否则，以秒为单位，返回 `key` 的剩余生存时间。

PTTL命令类似，但返回毫秒单位



### PERSIST key

移除给定 `key` 的生存时间，将这个 `key` 从“易失的”(带生存时间 `key` )转换成“持久的”(一个不带生存时间、永不过期的 `key` )。





## 事务

```bash
MULTI
这里是几行命令
EXEC / DISCARD 执行或者丢弃
```

**WATCH key [key …]**

监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。

**UNWATCH**

取消 WATCH命令对所有 key 的监视。（EXEC或DISCARD执行后就不用执行UNWATCH了）

## 订阅/发布

### PUBLISH channel message

将信息 `message` 发送到指定的频道 `channel` 。接收到信息 `message` 的订阅者数量。

### SUBSCRIBE channel [channel …]

订阅给定的一个或多个频道的信息。

```
redis> subscribe msg chat_room
Reading messages... (press Ctrl-C to quit)
1) "subscribe"       # 返回值的类型：显示订阅成功
2) "msg"             # 订阅的频道名字
3) (integer) 1       # 目前已订阅的频道数量

1) "subscribe"
2) "chat_room"
3) (integer) 2

1) "message"         # 返回值的类型：信息
2) "msg"             # 来源(从那个频道发送过来)
3) "hello moto"      # 信息内容

1) "message"
2) "chat_room"
3) "testing...haha"
```

### PSUBSCRIBE pattern [pattern …]

订阅一个或多个符合给定模式的频道。

每个模式以 `*` 作为匹配符，比如 `it*` 匹配所有以 `it` 开头的频道( `it.news` 、 `it.blog` 、 `it.tweets` 等等)， `news.*` 匹配所有以 `news.` 开头的频道( `news.it` 、 `news.global.today` 等等)，诸如此类。

### UNSUBSCRIBE [channel [channel …]]

指示客户端退订给定的频道。

如果没有频道被指定，也即是，一个无参数的 `UNSUBSCRIBE` 调用被执行，那么客户端使用 `SUBSCRIBE` 命令订阅的所有频道都会被退订。在这种情况下，命令会返回一个信息，告知客户端所有被退订的频道。

### PUNSUBSCRIBE [pattern [pattern …]]

指示客户端退订所有给定模式。

如果没有模式被指定，也即是，一个无参数的 `PUNSUBSCRIBE` 调用被执行，那么客户端使用 [PSUBSCRIBE pattern [pattern …\]](http://redisdoc.com/pubsub/psubscribe.html#psubscribe) 命令订阅的所有模式都会被退订。在这种情况下，命令会返回一个信息，告知客户端所有被退订的模式。





## Lua脚本

## 持久化

### SAVE

[SAVE](http://redisdoc.com/persistence/save.html#save) 命令执行一个同步保存操作，将当前 Redis 实例的所有数据快照(snapshot)以 RDB 文件的形式保存到硬盘。

一般来说，在生产环境很少执行 [SAVE](http://redisdoc.com/persistence/save.html#save) 操作，因为它会阻塞所有客户端，保存数据库的任务通常由 [BGSAVE](http://redisdoc.com/persistence/bgsave.html#bgsave) 命令异步地执行。然而，如果负责保存数据的后台子进程不幸出现问题时， [SAVE](http://redisdoc.com/persistence/save.html#save) 可以作为保存数据的最后手段来使用。

### BGSAVE

在后台异步(Asynchronously)保存当前数据库的数据到磁盘。

`BGSAVE`命令执行之后立即返回 `OK` ，然后 Redis fork 出一个新子进程，原来的 Redis 进程(父进程)继续处理客户端请求，而子进程则负责将数据保存到磁盘，然后退出。

客户端可以通过 `LASTSAVE`命令(LASTSAVE返回最近一次 Redis 成功将数据保存到磁盘上的时间，以 UNIX 时间戳格式表示)查看相关信息，判断 `BGSAVE`命令是否执行成功。



## 复制

### SLAVEOF host port

SLAVEOF命令用于在 Redis 运行时动态地修改复制(replication)功能的行为。

通过执行 `SLAVEOF host port` 命令，可以将当前服务器转变为指定服务器的从属服务器(slave server)。

如果当前服务器已经是某个主服务器(master server)的从属服务器，那么执行 `SLAVEOF host port` 将使当前服务器停止对旧主服务器的同步，丢弃旧数据集，转而开始对新主服务器进行同步。

另外，对一个从属服务器执行命令 `SLAVEOF NO ONE` 将使得这个从属服务器关闭复制功能，并从从属服务器转变回主服务器，原来同步所得的数据集*不会*被丢弃。

利用“`SLAVEOF NO ONE` 不会丢弃同步所得数据集”这个特性，可以在主服务器失败的时候，将从属服务器用作新的主服务器，从而实现无间断运行。

### ROLE

返回实例在复制中担任的角色， 这个角色可以是 `master` 、 `slave` 或者 `sentinel` 。 除了角色之外， 命令还会返回与该角色相关的其他信息， 其中：

- 主服务器将返回属下从服务器的 IP 地址和端口。
- 从服务器将返回自己正在复制的主服务器的 IP 地址、端口、连接状态以及复制偏移量。
- Sentinel 将返回自己正在监视的主服务器列表。

## 客户端与服务器

AUTH password 验证密码

通过设置配置文件中 `requirepass` 项的值(使用命令 `CONFIG SET requirepass password` )，可以使用密码来保护 Redis 服务器。

如果开启了密码保护的话，在每次连接 Redis 服务器之后，就要使用 `AUTH` 命令解锁，解锁之后才能使用其他 Redis 命令。

如果 `AUTH` 命令给定的密码 `password` 和配置文件中的密码相符的话，服务器会返回 `OK` 并开始接受命令输入。

另一方面，假如密码不匹配的话，服务器将返回一个错误，并要求客户端需重新输入密码。



QUIT退出

INFO查看服务端信息



SHUTDOWN [SAVE|NOSAVE]

[SHUTDOWN](http://redisdoc.com/client_and_server/shutdown.html#shutdown) 命令执行以下操作：

- 停止所有客户端
- 如果有至少一个保存点在等待，执行 [SAVE](http://redisdoc.com/persistence/save.html#save) 命令
- 如果 AOF 选项被打开，更新 AOF 文件
- 关闭 redis 服务器(server)

TIME 返回当前服务器时间。

一个包含两个字符串的列表： 第一个字符串是当前时间(以 UNIX 时间戳格式表示)，而第二个字符串是当前这一秒钟已经逝去的微秒数。



client kill

client list

client getname

client setname





## 其他

Sentinel、   Cluster 等