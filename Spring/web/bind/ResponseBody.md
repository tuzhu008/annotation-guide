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

在浏览器的开发人员控制台或使用像 Postman 这样的工具，我们可以看到以下响应:

```js
{"text":"Thanks For Posting!!!"}
```

**从 4.0 版本开始，这个注释也可以添加到类型级别，在这种情况下，它是继承的，不需要添加到方法级别。**

```java
@Controller
@ResponseBody
@RequestMapping("/post")
public class ExamplePostController {

    @Autowired
    ExampleService exampleService;

    @PostMapping("/response")
    public ResponseTransfer postResponseController(
      @RequestBody LoginForm loginForm) {
        return new ResponseTransfer("Thanks For Posting!!!");
     }
}
```

记住，我们不需要用 `@ResponseBody` 注解 `@RestController` 注解的控制器，因为 `@ResponseBody`是 `@RestController` 的元注解，它在这里是默认完成的。



