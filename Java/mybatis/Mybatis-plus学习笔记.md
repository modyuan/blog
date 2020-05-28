# Mybatis-plus学习笔记

## 简介

它是第三方开发的基于mybatis的增强版，提供了很多便捷功能。下称MP

官网：https://mp.baomidou.com/

- 要避免同时导入mybatis和mybatis-plus
- 使用mybatis-plus和springboot时，在application.yaml中，就只有mybatis-plus的设置选项了。



## 导入

maven:

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.0.5</version>
</dependency>
```

## 配置日志

配置日志来查看自动生成的sql是什么样子的。

在application.yaml中设置`mybatis-plus.configuration.log-impl`即可。

注意，对于stdout，可以默认打印所有级别的日志；对于slf4j等，还需要设置`logging.level`才可以打印sql语句（它要求一个map作为值）。设置方法是要传入具体的包名和级别才行。例如：

```yaml
logging:
  level:
    com:
      yzmoe:
        mapper: trace
```

这样才能把`com.yzmoe.mapper`下的日志以trace级别输出。想查看sql语句，起码要debug级别，要查看每一行结果的返回细节，需要trace级别。

具体的可用级别是：

> TRACE, DEBUG, INFO, WARN, ERROR, FATAL, OFF



## 主键生成策略

一般来说，常见的有这样几种：

- 数据库自增（单机, auto_increment/serial）
- UUID（字符串）
- 雪花算法（大整数Long，Twitter开源的算法，利用机器ID、时间和序列号）

MP中对应的设置是在POJO的id对应的字段上加`@TableId(type = ?)`

这里type有多种选择:

- AUTO 即数据库自增
- NONE 不使用自增 
- INPUT 手动输入(也不会回填ID)
- ASSIGN_ID 默认的全局ID（默认使用雪花算法）
- ASSIGN_UUID  使用UUID的全局ID（UUID去除`-`后的结果）



## 自动填充

### 方法一：数据库级别

假设我们需要在一个表中增加`gmt_create`和`gmt_modified`来表示创建时间和修改时间。

可以这样做：

```sql
alter table user add column gmt_create datetime null default current_timestamp;

alter table user add column gmt_modified datetime null default current_timestamp on update current_timestamp;
```

这样就可以利用数据库自动更新时间。



### 方法二：代码级别

使用`@TableField`注解

```java
@TableField(fill = FieldFill.INSERT)
private LocalDateTime gmtCreate;
@TableField(fill = FieldFill.INSERT_UPDATE)
private LocalDateTime gmtModified;
```

然后实现一个MetaObjectHandler接口来决定insert和update时候的动作。





## 乐观锁🔒

通过加version字段来实现。

在sql数据库中加version字段（int)

相应的POJO中也要加。同时在字段上加@Version注解（MP提供的）

然后需要写一个配置类（@Configuration），注册乐观锁插件

```java
@EnableTransactionManagement
@Configuration
class MyBaitsConfig{
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
}
```

先用select获取一次，然后update时候，对version+1,并设置条件仅在version等于select的时候的值时更新。如果在select和update中间有其他线程进行了这样的操作，那么这次的update就会失败，从而起到了锁的作用。





## 分页



## 逻辑删除

deleted = 0 