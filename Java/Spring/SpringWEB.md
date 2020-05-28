# SpringWebMvc用法

假设我们使用Springboot。

## @Controller @Service @Repository

它们的作用和`@Component`一样，都是注册成一个Bean，但是语意上不一样，建议在对应的层上使用对应的注释。控制层（一般是controller包）使用Controller，服务层（一般是service包）使用Service，而在持久层DAO（如果用mybatis的话包应该是mapper）用Repository。

对于Mybatis的Mapper的接口来说，加了@Repository没有实际作用，但可以消除IDE的错误提示。



## 控制层处理路由

在控制层，我们一般使用`@RequestMapping`来注解方法所处理的路由。注解有一些参数需要设置：

| 注解     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| path     | 要控制的路径，可以是多个。比如 `"/index"`或者 `{"\art","\bus"}`。不能省略。 |
| method   | 匹配的http方法。GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE中的一个或者多个。不写的话就默认都可以匹配。 |
| params   | 指定要匹配的参数以及结果，如`"id=3"`,可以指定多个，默认不限制参数。可以使用`!=` |
| headers  | 匹配http的请求头，语法和上一个类似。默认不限制。             |
| consumes | 匹配请求的Content-Type，可以是一个或多个。默认不做限制。     |
|produces|设置返回的Content-Type，默认根据返回类型进行匹配。|

- 对于需要返回http的body而不是视图名称的控制器，需要在控制器上加`@ResponseBody`注解。`@ResponseBody`可以加在类上。

- 使用`@RestController`相当于`@Controller`和@`ResponseBody`的复合。
- 使用`@GetMapping`、`@PostMapping`等，相当于`@RequestMapping`的特化版本。



## 控制器接收参数

#### application/x-www-form-urlencoded 编码格式

一般来说有两种形式：get和post。

GET类型像这样：`http://example.com/abc/q?k1=p1&k2=p2`

POST类型像这样：`http://example.com/abc/q`，body：`k1=p1&k2=p2`

这种编码形式会把一个map以键值对`key=value`的形式组合起来，键值对之间用`&`符号分割。不在范围内的字符以%HH的16进制形式来编码，

这里的范围指[a-zA-Z0-9]以及`. _ - ~`这四个。其他的字符都会应该被编码。空格可以编码成加号也可以编码成%20。

> RFC3986文档规定，URL 中只允许包含英文字母（a-zA-Z）、数字（0-9）、`-_.~ `4个特殊字符以及所有保留字符`! * ' ( ) ; : @ & = + $ , / ? # [ ]`

对于表单自动编码，由表单form的accept-charset属性决定。默认值是一个保留字符串 `"UNKNOWN"`。此字符串指的是，和包含此表单元素的文档相同的编码。

对于URL中的查询字段（`http://example.com/abc/q?k1=p1&k2=p2`中问号后面的部分），以及post方式提交表单（以application/x-www-form-urlencoded格式）

对于这种编码格式，直接在控制器的函数增加参数即可，名字对应传入的参数。也可以手动指定特性，使用@RequestParam，来制定函数的参数要接受的key的名字，以及这个参数是否必须（默认是必须），如果必须的参数没有传入，那么就会报错，返回404错误。



#### application/json

对于json格式，这里最好是加上charset，写成"application/json; charset=UTF-8"。可以保证编码解码不会出错。

这种格式要接收的话，需要在控制器加一个参数来接收（这里的参数名就无所谓了），同时参数上加注释`@RequestBody`，来表示请求的请求体会传给这个参数。spring内部会调用jackson库来对json进行解析。解析失败的话就会返回400错误。



#### text/plain

也可以用@RequestBody 加在控制器的String参数上来进行接收。这个也应该加上utf8。



#### multipart/form-data

这种类型不光可以传输普通参数，也是上传文件的唯一指定参数。

对于这种编码的普通的参数可以通过String之类的参数来接受。

但是对于文件上传，就只能用MultipartFile或者MultipartFile[]来接受了。同时参数名要对应表单中的name才行。表单中name是可以重复的，所以这里MultipartFile可以是一个数组。

通过MultipartFile对象，我们就可以获取文件的长度、原文件名、内容等信息了。

在application.properties配置中，还可以对文件上传的大小进行限制

```properties
#单个数据的大小
spring.servlet.multipart.max-file-size=10MB
#（多个文件）总数据的大小
spring.servlet.multipart.max-request-size=10MB
```




#### 除去传入的参数之外

我们还可以把HttpServletRequest 以及 HttpServletResponse加到参数中，可以控制http的一些细节。



## 控制器返回内容

### 直接返回值

如果加了@ResponseBody的话，控制器的返回值就是前端接收到的值了。

这里如果不指定返回的类型的话，对于一个java对象（除去String），会返回一个json，并设置好	`Content-Type: applicationi/json; charset=UTF-8`，这里是jackson自动的序列化，我们在对象的字段上可以用jackson提供的注解进行一些定制，如日期格式等等。

对于String，会返回`text/html; charset=UTF-8`类型。

如果自己通过RequestMapping指定了produces，这里就要注意加上charset，不然默认是ascii编码，是编码不了中文的。



### 返回视图

返回视图，需要将控制器的返回值类型设为String。返回视图需要视图解析器。SpringBoot默认有一个简单的视图解析器，如返回"index.html"就可以让控制器返回index.html的内容，同时媒体类型也会设置好。其他的static目录下的文件也是如此。

如果我们在工程中加了其他依赖库，就可以增加其他的视图解析器。

我们也可以配置系统自带的解析器：

```java
@Configuration
public class ViewConfig {
    //@Bean
    public InternalResourceViewResolver commonResourceView(){
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/");
        resolver.setSuffix(".html");
        resolver.setOrder(1);
        resolver.setContentType("text/html");
        return resolver;
    }
}
```

可以设置前缀、后缀、解析器顺序、媒体类型等等。



## 控制器使用Session

这时需要在控制器参数中加HttpServletRequest，然后用getSession()方法来获取session。

从客户端的sessionID到控制器使用sessionID对应的session这个过程是自动的。

如果你想劫持这个session，控制它的创建与返回，那么就需要像shiro一样，设置一个servlet的filter来包装HttpServletRequest和HttpServletResponse来达到目的。



## @ControllerAdvice

@ControllerAdvice、@RestControllerAdvice 注解，可以用于定义@ExceptionHandler、@InitBinder、@ModelAttribute，并应用到所有@RequestMapping、@PostMapping， @GetMapping注解中。



示例如下：

```java
@ControllerAdvice
public class MyExceptionHandler {

    /**
     * 应用到所有@RequestMapping注解方法，在其执行之前初始化数据绑定器
     * @param binder
     */
    @InitBinder
    public void initWebBinder(WebDataBinder binder){
        //对日期的统一处理
        binder.addCustomFormatter(new DateFormatter("yyyy-MM-dd"));
        //添加对数据的校验
        //binder.setValidator();
    }

    /**
     * 把值绑定到Model中，使全局@RequestMapping可以获取到该值
     * @param model
     */
    @ModelAttribute
    public void addAttribute(Model model) {
        model.addAttribute("attribute",  "The Attribute");
    }

    /**
     * 捕获CustomException
     * @param e
     * @return json格式类型
     */
    @ResponseBody
    @ExceptionHandler({CustomException.class}) //指定拦截异常的类型
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR) //自定义浏览器返回状态码
    public Map<String, Object> customExceptionHandler(CustomException e) {
        Map<String, Object> map = new HashMap<>();
        map.put("code", e.getCode());
        map.put("msg", e.getMsg());
        return map;
    }
}
```

