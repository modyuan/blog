# Mybatis的配置文件写法

[toc]

首先，下面这个是一个基本的模版，指定了数据库的连接，还有mapper文件的位置。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

详细的可以看原文档：https://mybatis.org/mybatis-3/zh/configuration.html#properties 。这里主要写一些主要的设置。

具体来说，可以配置的东西有以下几种（可以缺，但顺序不能乱）：

##  properties

主要就是配置一些值供DataSource使用而已，可以引用外部的`properties文件`，也可以在标签内部定义。内部定义的会覆盖外部定义的。
```xml
<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```

> properties文件也就是以`.properties`为后缀名，同时内容是`somekey=someValue`这样格式的内容，一行一个。

## settings

这里是K-V性质的一些选项设置，会影响Mybatis的行为。

有用的有：

- mapUnderscoreToCamelCase 开启自动驼峰映射，比如java中的userName到sql中的USER_NAME这样的映射。可以省去指定字段名的麻烦。默认关闭，建议开启。



配置完整的话是这样：

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```

## typeAliases

这个设置可以简化Mapper文件中类型的写法，把较长的全限定名定义成一个短语。

下面是一个例子：

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

也可以指定一个包名，这样包下所有的类都只用写类名就可以了（首字母要小写）。

```xml
<typeAliases>
  <package name="com.example.pojo"/>
</typeAliases>
```

同时也可以在相关类上加注解实现别名：

```java
@Alias("author")
public class Author {
    ...
}
```

常见的Java对象都有预定义好的别名。详细参见原文档。

## typeHandlers

用于将数据库类型和java类型的转换，常见的已经内置好，特殊需求可以自定义处理器。

基本上用不到这里。

## objectFactory

对象工厂，用于对象实例的创建。如果你想在创建实例化一个目标类的时候有其他行为，才需要这个配置。

基本上用不到这里。

## plugins

配置插件用的，如果一些程序对Mybatis做了扩展，就需要到这里配置进去。

## environments

可以配置多种环境，比如测试、生产。测试环境可以使用本地的数据库，而生产环境使用线上的数据库。

```xml
<environments default="development"> <!-- 这里指定使用哪个环境 -->
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
  <environment id="test">
      <!-- 这里可以配置多个环境 -->
  </environment>
</environments>
```

这里的`${username}`可以引用properties中定义的数据，也可以直接写（比如`root`）。

`transactionManager`这里是指定事务管理器，在Spring框架中，spring会接管这里，就不用配置了。

`dataSource`这里可以指定连接数据的选项。其中`type`属性可以指定 POOLED、 UNPOOLED或  JNDI。测试时候可以用POOLED，实际线上哪个也不用，而是使用有名的第三方的数据源实现（实现DataSource接口的类），比如c3p0、druid（阿里出品的带监控的数据源）等，这里就要写全限定名了。（SpringBoot使用的是Hikari）

假设连本地的postgresql数据源，需要这样写：

```xml
<dataSource type="org.myproject.C3P0DataSourceFactory">
  <property name="driver" value="org.postgresql.Driver"/>
  <property name="url" value="jdbc:postgresql://localhost:5432/databaseName"/>
  <property name="username" value="postgres"/>
  <property name="password" value="root"/>
</dataSource>
```

## databaseIdProvider

可以根据不同的数据库类型来执行不同的SQL。这种多厂商的支持是基于映射语句中的 `databaseId` 属性。

因为Mybatis是个半自动的ORM，所以会这样，相反一些全自动的ORM就很少需要写SQL。

## mappers

指定映射文件的路径。

写法有这样四种。

```
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
<!-- 使用完全限定资源定位符（URL） 不推荐 -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
<!-- 使用映射器接口实现类的完全限定类名 需要类名和Mapper文件同名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
<!-- 将包内的映射器接口实现全部注册为映射器 需要包下的Mapper类和Mapper.xml的文件名相同-->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

相对来说第四种省事一点。但是注意XxxxMapper.xml文件的放置。假如放在resource下的话，需要和类中的层级一样，也要建立文件夹层次才行。要是和java文件放在一个文件夹下，需要设置在`pom.xml`中设置资源过滤器才行，不然最终打包的文件默认不会包含xml。

（注意⚠️：当你这么做时，需要把原先的resource目录也设置一遍，不然到最后你会发现xml文件有了，但是原先的resource目录下的文件全没了！）

