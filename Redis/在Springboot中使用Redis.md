# 在SpringBoot中使用Redis



## 引入项目

在Springboot中，我们要使用redis，可以很方便的使用starter包，在maven中加入：

```xml
 <dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

这个包依赖lettuce（它类似于jedis，也是一个对redis的客户端，你也可以无缝切换为jedis）。



然后需要在配置文件`application.yaml`中添加redis的配置，如host，port，password，并发数等等。不配置的话默认是本地ip、默认端口、无密码、无超时限制、并发数为8。

常见配置如下：

```yaml
spring:
  redis:
    # redis数据库索引（默认为0），我们使用索引为3的数据库，避免和其他数据库冲突
    database: 3
    # redis服务器地址（默认为localhost）
    host: localhost
    # redis端口（默认为6379）
    port: 6379
    # redis访问密码（默认为空）
    password:
    # redis连接超时时间（单位为毫秒）
    timeout: 0
    # redis连接池配置
    pool:
      # 最大可用连接数（默认为8，负数表示无限）
      max-active: 8
      # 最大空闲连接数（默认为8，负数表示无限）
      max-idle: 8
      # 最小空闲连接数（默认为0，该值只有为正数才有作用）
      min-idle: 0
      # 从连接池中获取连接最大等待时间（默认为-1，单位为毫秒，负数表示无限）
      max-wait: -1
```



然后在项目中，我们可以在类中注入:

```java
@Autowired
private StringRedisTemplate stringRedisTemplate;
@Autowired
private RedisTemplate redisTemplate;
```

这两个对象关系如下：

- 两者的关系是StringRedisTemplate继承RedisTemplate。
- 两者的数据是不共通的；也就是说StringRedisTemplate只能管理StringRedisTemplate里面的数据，RedisTemplate只能管理RedisTemplate中的数据。
- 其实他们两者之间的区别主要在于他们使用的序列化类:
    - RedisTemplate使用的是JdkSerializationRedisSerializer  存入数据会将数据先序列化成字节数组然后在存入Redis数据库。 
    - StringRedisTemplate使用的是StringRedisSerializer

- 使用时注意事项：
    - 当你的redis数据库里面本来存的是字符串数据或者你要存取的数据就是字符串类型数据的时候，那么你就使用StringRedisTemplate即可。
    - 但是如果你的数据是复杂的对象类型，而取出的时候又不想做任何的数据转换，直接从Redis里面取出一个对象，那么使用RedisTemplate是更好的选择。

### RedisTemplate中定义了5种数据结构操作

```java
redisTemplate.opsForValue();　　//操作字符串
redisTemplate.opsForHash();　　 //操作hash
redisTemplate.opsForList();　　 //操作list
redisTemplate.opsForSet();　　  //操作set
redisTemplate.opsForZSet();　 　//操作有序set
```

### StringRedisTemplate常用操作

```java
stringRedisTemplate.opsForValue().set("test", "100",60*10,TimeUnit.SECONDS);//向redis里存入数据和设置缓存时间  

stringRedisTemplate.boundValueOps("test").increment(-1);//val做-1操作

stringRedisTemplate.opsForValue().get("test")//根据key获取缓存中的val

stringRedisTemplate.boundValueOps("test").increment(1);//val +1

stringRedisTemplate.getExpire("test")//根据key获取过期时间

stringRedisTemplate.getExpire("test",TimeUnit.SECONDS)//根据key获取过期时间并换算成指定单位 

stringRedisTemplate.delete("test");//根据key删除缓存

stringRedisTemplate.hasKey("546545");//检查key是否存在，返回boolean值 

stringRedisTemplate.opsForSet().add("red_123", "1","2","3");//向指定key中存放set集合

stringRedisTemplate.expire("red_123",1000 , TimeUnit.MILLISECONDS);//设置过期时间

stringRedisTemplate.opsForSet().isMember("red_123", "1")//根据key查看集合中是否存在指定数据

stringRedisTemplate.opsForSet().members("red_123");//根据key获取set集合
```



## 在 SpringBoot中配置

```java
@Configuration
@EnableCaching
public class RedisConfig {

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory redisConnectionFactory){
        RedisCacheConfiguration redisCacheConfiguration = RedisCacheConfiguration
            .defaultCacheConfig() //首先要取默认设置
            .entryTtl(Duration.ofMinutes(1)); // 设置缓存有效期
        	.computePrefixWith((prefix)-> "redis::"+ prefix+"::"); //添加前缀

        RedisCacheManager build = RedisCacheManager
            .builder(redisConnectionFactory) //这个工厂接口有两个实现类，jedis和lettuce
            .cacheDefaults(redisCacheConfiguration).build();
        return build;
    }
}
```

配置完成后，当需要缓存时，就会在redis中直接存储一个字符串。key是前缀加上id，值是要存储的对象的二进制序列。



这样，在服务层的接口上，就可以使用`@Cacheable`来声明方法的缓存了。



例子如下：

```java
// UserService.java
public interface UserService {

    User getUserById(int id);
    int delUserById(int id);
}


// UserServiceImpl.java
@Service
@Slf4j
public class UserServiceImpl implements UserService{

    @Autowired
    private UserMapper userMapper;

    @Override
    @Cacheable(value="user",key ="#id")
    public User getUserById(int id) {
        log.info("query user by id[{}] from db", id);
        return userMapper.getUserById(id);
    }

    @Override
    @CacheEvict(value = "user", key = "#id")
    public int delUserById(int id) {
        return userMapper.delUserById(id);
    }
}


```

