# Spring基本说明

两大特性：

- IOC 控制反转	
- AOP 面向切面

Spring实现这些功能的机制是使用反射。

这里就全程使用注解的方式来讲。

## IOC

通过依赖注入来实现。通过将一个类注册成Bean，可以由Spring帮你把这个类注入到其他需要的地方。即类与类之间的组装关系不用自己写。

一个Bean有两个性质，类型和名字。我们在装配时可以选择按名字还是按类型装配。

### 注册一个Bean

1. 这个类是自己写的，可以改动。

    在类上加`@Component`，在web应用中，对于控制层、服务层、持久层，应该加更准确的注解：@Controller @Serivce @Repository,。

    - 属性value代表Bean的名字，不写的话默认是类名的首字母小写后的结果。但当类的名字是以两个或以上的大写字母开头的话，bean的名字会与类名保持一致。
    - 属性scope代表模式，单例、原型……也可以配合`@Scope`注解。 

    要想能扫描到这个包，还需要有@ComponentScan的帮助。

2. 这个类已有的，不能改动。

    写一个这个类的get方法，然后在方法上加@Bean注解，在方法所在类上加@Configuraion注解。@Bean的属性value代表Bean的名字，不写的话默认是get方法的名字。

    
    
    为组件设置初始化方法和销毁方法：
    
    ```java
    @PostConstruct
    public void addItem() {
    	System.out.println("初始化方法");
    }
    @PreDestroy
    public void testItem() {
    	System.out.println("释放资源");
    }
    ```
    
    
    
3. ==特殊情况==：当一个类实现了FactoryBean接口时，它注册成Bean时，返回的不是它本身，而是它的getObject()方法所返回的值。

    

### 配置组件自动扫描

需要有这样一个配置类

```java
@ComponentScan
@Configuration
public class AppConfig{
    
}
```

两个注解都不可少。`@ComponentScan`还有个`value`属性，可以指定扫描的包，在这个包下的所有被@Component注解过的类，都会注册到Spring里面。

## @Import

1. 使用@Import注解，加在一个配置类上，参数里面是其他配置类。这样可以合并配置类。
2. 参数是普通类，可以将这个类注册为一个Bean。



## @ImportResource

可以导入xml配置



### 自动注入

要注入的类和被注入的类必须都注册为Bean。常用的注入注解是`@Autowired`，它是按类型注解。通过结合@Qualifier一起使用，也可以按名称注入。

可以加在**字段**上、加在**setter方法**上、加在**构造方法**上。

属性：

- required 是否必须注入，默认为true，即必须注入，不然报错。设为false的话，找不到要注入的对象就会设为null。



如果要注入的是一个接口类型，那么会自动找它的实现类。如果实现类不止一个，那就会报错，此时需要结合@Qulifier指定具体的实现类名字。



### 属性值

@Value("somevalue")加在构造函数的参数上或者字段上，可以直接给相应的参数赋值。重要的是，可以从外部文件获取。

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name}") String catalog) {
        this.catalog = catalog;
    }
}
//-------

@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig { }
```

这里就可以引用文件(只能是properties文件)中的值。假设你在用springboot，更进一步可以不用配置@PropertySource，直接使用application.properties或者application.yaml中的值，因为springboot预先配置了相关的类。

```@Value("${catalog.name:defaultCatalog}"```

这样还可以提供一个默认值，当外部文件没有catelog.name时，使用defaultCatelog这个字面量。



在类的初始化区域时是取不到值的。



## Bean的作用域

Spring 容器中的 bean 可以分为 5 个范围。所有范围的名称都是自说明的，但是为了避免混淆，还是让我们来解释一下：

1. **singleton**：单例，默认的范围，这种范围确保不管接受到多少个请求，每个容器中只有一个 bean 的实例，单例的模式由 bean factory 自身来维护。注意，它不是线程安全的。
2. **prototype**：原形范围与单例范围相反，为每一个 bean 请求提供一个实例。
3. **request**：在请求 bean 范围内会每一个来自客户端的网络请求创建一个实例，在请求完成以后，bean 会失效并被垃圾回收器回收。
4. **Session**：与请求范围类似，确保每个 session 中有一个 bean 的实例，在 session 过期后，bean 会随之失效。
5. **global-session**：global-session 和 Portlet 应用相关。当你的应用部署在 Portlet 容器中工
    作时，它包含很多 portlet。如果你想要声明让所有的 portlet 共用全局的存储变量的话，那么
    这全局变量需要存储在 global-session 中。
    全局作用域与 Servlet 中的 session 作用域效果相同。





## 手动获取一个Bean

Spring可以自动注入，但是终究需要一个源头，来获取Bean。

### 注解方式

```java
ApplicationContext context = new AnnotationConfigApplicationContext();
User user = (User) context.getBean("User");
User user2 = context.getBean(User.class);
// user == user2
```

如上述代码，可以按名称获取或者类型获取。类型获取的好处是不用进行类型转换。

注意：接口是不能声明Bean的，但是获取时可以按接口class获取。（多个实现类时会报错，此时就要按名称获取了）

如果使用了SpringBoot框架的话，因为是Spring把控项目的启动，所以也就不需要这个了。





### XML方式

首先在resource目录下创建一个springBean的配置文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<import resource="services.xml"/> 这里可以引用其他配置xml
    <bean id="user" class="com.example.User"> 这里可以定义许多个Bean
        <constructor-arg index="0" value="Myname" />
        <constructor-arg index="1" value="Myhobby" />

    </bean>
</beans>
```

里面可以声明许多Bean。具体语法这里不细说了，因为从springboot开始，就很少用xml配置的方式了。

在java代码中，要从这个xml生成ApplicationContext，要这样做：

```java
ApplicationContext context = new ClassPathXmlApplicationContext();
User user = (User) context.getBean("user");
```



###  两种方式相互引用

1. 把xml的context导入到注解的context中。

    需要在`@Configuration`的注解类上再加一个注解`@ImportResource({"classpath:application.xml"})`，就可以导入了。可以导入多个xml配置文件。

2. 在xml配置的基础上使用注解。

    在xml配置中加入`<context:component-scan base-package="com.pojo" />`可以（1.开启注解扫描。2.扫描指定包下的@Component注解过的类，注册为Bean。3.@Configuration注解的类中注册的Bean也会包含,相当于倒入了指定包下的所有注解配置类。）

3. 在xml配置中导入其他xml配置。

    使用`<import resource="services.xml"/>`标签即可。

4. 在注解类中的导入其他注解类。

    使用@Import注解。

    

总的来说，趋势是把xml配置转换成注解类配置的方式。



最基础的，有了以上的知识，已经可以把IOC利用起来了。



## AOP

面向切面编程。简单来说，可以在不修改目标类的情况下，在类的方法的执行前后增加执行语句。这个功能同时需要IOC的配合，也就是必须注册成Bean才行。一个常见的用途是输出Log。



### 基本使用

#### @EnableAspectJAutoProxy 注解

在一个配置类（加了@Configuration）上添加这个注解，后面的注解才会生效。

#### @Aspect注解

这个注解可以将一个类启用Aspect，这样类中的方法就可以自动插入到其他地方。这个类也要注册成Bean才行。（比如可以加上@Component）

#### @Pointcut 注解

在定义了@Aspect类的一个空方法上使用，作用是声明一个切入点，参数为要切入的其他类的位置，语法要使用`excution()`。

---

#### 切入位置的注解

关于切入的位置，有这样几个注解：

- @Before 通知方法会在目标方法调用之前执行
- @After 通知方法会在目标方法**返回**或者**抛出异常**后调用
- @AfterReturning 通知方法会在目标方法**返回**后调用
- @AfterThrowing 通知方法会在目标方法**抛出异常**后调用
- @Around 通知方法会将目标方法封装起来

这些注解写在方法上面，参数可以直接写execution表达式，也可以写被@Pointcut注解的方法的名称，这样就不用重复写execution表达式了。

例子：

```java
@Aspect
public class Audience{
    @Pointcut("execution(** concert.Performance.perform(..))")
    public void performs(){} // 这里是一个空方法，只是为了方便其他注解引用这个execution表达式。
    
    @Before("performs()")
    public void takeSeat(){
        System.out.println("Taking seats.");
    }
    
    //.....
}
```



@Around注解和其他不同，它是环绕，也就是需要一个语句代表要切入的方法本身才行。所以它注解的方法需要有参数。例子如下：

```java
@Around("perfoms()")
public void watchPerform(ProceedingJoinPoint jp){
    try{
        System.out.println("before method.");
        jp.proceed();
        System.out.println("After return");
    }catch(Throwable e){
        System.out.println("After throw");
    }
}
```



### 处理通知中的参数

假设CD类有一个方法叫 `void playTrack(int trackNumber)`

那么在通知类中，如果想要获取到切入对象的参数，可以这样写：

```java
@Aspect
@Component
public class TrackCounter{
    private Map<Integer,Integer> trackCounts = new HashMap<>();
    
    @Pointcut("execution(* CD.playTrack(int)) && args(trackNumber)")
    public void trackPlayed(int trackNumber){}
    // 这里表达式中args里面的参数名，要和通知方法的参数名一致。但和要切入方法的参数名无关。
    
    @Before("trackPlayed(trackNumber)")
    public void countTrack(int trackNumber){
        Interger n = trackCounts.get(trackNumber);
        int currentCount = 0;
        if(n != null) currentCount = n;
        trackCount.put(trackNumber,currentCount+1);
    }
    
}
```



### 通过注解引用新功能

例子如下：

```java

//AdderImpl.java
@Component
public class AddImpl {
    public int add(int first,int second){
        return first + second;
    }
}
//--------
public interface Minus {
    int minus(int first,int second);
}
//--------
@Component
public class MinusImpl implements Minus {
    @Override
    public int minus(int first, int second) {
        return first - second;
    }
}
//-------

@Component
@Aspect
public class MinusIntroducer {
    @DeclareParents(value = "com.yzmoe.service.AddImpl+", // +表示Add的所有子类
    defaultImpl = MinusImpl.class) // 默认实现的类。
    public Minus minus; //要添加的接口。
}
//-------

@Configuration
@EnableAspectJAutoProxy
@ComponentScan
public class MyConfig {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        Minus m = (Minus)context.getBean("addImpl");
        // 或者写 Minus m = (Minus)context.getBean(AddImpl.class);
        System.out.println(m.minus(100, 7)); // 结果为93
    }
}

```

可以看出，需要切入的类和通知类比如都是接口的形式才行。

