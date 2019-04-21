# @RequestBody

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestBody {

    /**
     * Whether body content is required.
     * <p>Default is {@code true}, leading to an exception thrown in case
     * there is no body content. Switch this to {@code false} if you prefer
     * {@code null} to be passed when the body content is {@code null}.
     * @since 3.2
     */
    boolean required() default true;

}
```

## 解析

`@RequestBody` 表示方法参数应该绑定到 web 请求的主体。请求的主体通过 HttpMessageConverter 传递，以根据请求的内容类型解析方法参数。另外，可以使用 `@Valid` 注解参数来应用自动验证。

支持带注解的处理器方法。

由于被 `@RequestBody` 注解的方法参数会从 web 请求的主体被解析出来，因此该参数在其他地方是无法被解析的，如 URL 参数。

简单地说，`@RequestBody` 注解将 HttpRequest 主体映射到传输\(transfer\)或域\(domain\)对象，从而支持将入站 HttpRequest 主体自动反序列化到 Java 对象。

下面有一个处理器方法：

```java
@PostMapping("/request")
public ResponseEntity postController(
  @RequestBody LoginForm loginForm) {

    exampleService.fakeAuthenticate(loginForm);
    return ResponseEntity.ok(HttpStatus.OK);
}
```

如果指定了适当的 JSON 类型，Spring 会自动将 JSON 反序列化为 Java 类型。默认情况下，我们使用 `@RequestBody` 注解的类型必须与客户端控制器发送的 JSON 相对应:

```java
public class LoginForm {
    private String username;
    private String password;
    // ...
}
```

这里，我们用来表示 HttpRequest 主体映射到 LoginForm 对象的对象。

让我们用CURL测试一下：

```bash
curl -i \
-H "Accept: application/json" \
-H "Content-Type:application/json" \
-X POST --data 
  '{"username": "johnny", "password": "password"}' "https://localhost:8080/.../request"
```



