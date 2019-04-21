# @ResponseBody

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ResponseBody {

}
```

## 解析

`@ResponseBody`  注解表示方法返回值应该绑定到 web 响应体，方法返回的对象被自动序列化为 JSON 并传递回 HttpResponse 对象。支持带注注解的处理器方法。

从 4.0 版本开始，这个注释也可以添加到类型级别，在这种情况下，它是继承的，不需要添加到方法级别。

假设我们有一个自定义响应对象:

```java
public class ResponseTransfer {
    private String text; 

    // standard getters/setters
}
```

接下来，可以实现相关的控制器:

```java
@Controller
@RequestMapping("/post")
public class ExamplePostController {
 
    @Autowired
    ExampleService exampleService;
 
    @PostMapping("/response")
    @ResponseBody
    public ResponseTransfer postResponseController(
      @RequestBody LoginForm loginForm) {
        return new ResponseTransfer("Thanks For Posting!!!");
     }
}
```



