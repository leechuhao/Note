## @RequestMapping

使用这个注解相当于在web.xml中进行了以下配置

```
<servlet>
    <servlet-name>servletName</servlet-name>
    <servlet-class>ServletClass</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletName</servlet-name>
    <url-pattern>url</url-pattern>
</servlet-mapping>
```

源码展示：

```
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Mapping
public @interface RequestMapping {
	String name() default "";
	String[] value() default {};
	String[] path() default {};
	RequestMethod[] method() default {};
	String[] params() default {};
	String[] headers() default {};
	String[] consumes() default {};
	String[] produces() default {};
}
```



1. ### value 属性

value表示URI的路径标识

```
@Controller
@RequestMapping(value="/mapping")
public class RequestMappingController {
    //http://localhost:8080/spring-mvc/mapping/test
    @RequestMapping(value="/test")
    public ModelAndView test(){
        return new ModelAndView("test");
    }
    //http://localhost:8080/spring-mvc/mapping/4/user/abc/detail
    @RequestMapping(value="/{:\\d}/user/{id}/detail")
    public ModelAndView comprehensiveDemo(){
        ModelAndView mav = new ModelAndView("test");
        return mav;
    }
    @RequestMapping(value="/{id}", method=RequestMethod.GET)
	public String show(@PathVariable("id") Integer id) {
		return "success";
	}
}
```

{id}这类占位符是Spring 3.0 新增的功能，可以通过 @PathVariable 将 URL 中的占位符绑定到控制器的处理方法的参数中，占位符使用{}括起来，请求的方法必须为GET

2. ### method 属性

method的值是用来处理用户请求的属性的，是Get请求，还是post请求，还是PUT请求，等等，spring MVC都可以进行精细的管理.

由于Mehtod的值是固定的，所以spring mvc用枚举进行管理(GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS, TRACE)

最基本的有四种，分别为：GET（查）、POST（增）、PUT（改）、DELETE（删）

浏览器默认是GET请求

```
@Controller
@RequestMapping(value="/mapping")
public class RequestMappingController {
    @RequestMapping(value = "/detail",method = RequestMethod.GET)
    public ModelAndView methodTestDemo(){
        ModelAndView mav = new ModelAndView("test");
        return mav;
    }
}
```



3. ### headers 属性

headers指定request中必须包含某些指定的header值，才能让该方法处理请求

```
@Controller
@RequestMapping(value="/mapping")
public class RequestMappingController {
    @RequestMapping(value = "/detail",headers="location=www.tuniu.com")
    public ModelAndView HeadersDemo(){
        ModelAndView mav = new ModelAndView("test");
        return mav;
    }
}
```

第三方工具--------RESTClient 可以模拟添加 headers 属性进行测试

4. ### consumer 属性

指定处理请求的提交内容类型（Content-Type），例如application/json, text/html等等

```
@Controller
@RequestMapping(value="/mapping")
public class RequestMappingController {
//表示我们请求体Content-Type:"application\json;charset=utf-8"才会响应
    @RequestMapping(value = "/detail",consumes="application/json")
    public ModelAndView HeadersDemo(){
        ModelAndView mav = new ModelAndView("test");
        return mav;
    }
}
```

如果直接访问会报错，错误信息为415，415错误是指 对于当前请求的方法和所请求的资源，请求中提交的实体并不是服务器中所支持的格式，因此请求被拒绝

5. ### produces 属性

指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回

```
@Controller
@RequestMapping(value="/mapping")
public class RequestMappingController {    
    @RequestMapping(value = "/detail",produces="application/json")
    public Object HeadersDemo(){
        ......
    }
}
```

这样只会返回 “Json“ 数据

6. ### params 属性

该属性表示请求参数，也就是追加在URL上的键值对，多个请求参数以&隔开

```
@Controller
@RequestMapping(path = "/user")
public class UserController {
        // 该方法将接收 /user/login 发来的请求，且请求参数必须为 username=kolbe&password=123456
        //http://localhost/user/login?username=kolbe&password=123456
	@RequestMapping(path = "/login", params={"username=kolbe","password=123456"})
	public String login() {
		return "success";
	}
}
```

