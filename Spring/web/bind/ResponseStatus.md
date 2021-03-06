# @ResponseStatus

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ResponseStatus {

    /**
     * Alias for {@link #code}.
     */
    @AliasFor("code")
    HttpStatus value() default HttpStatus.INTERNAL_SERVER_ERROR;

    /**
     * The status <em>code</em> to use for the response.
     * <p>Default is {@link HttpStatus#INTERNAL_SERVER_ERROR}, which should
     * typically be changed to something more appropriate.
     * @since 4.2
     * @see javax.servlet.http.HttpServletResponse#setStatus(int)
     * @see javax.servlet.http.HttpServletResponse#sendError(int)
     */
    @AliasFor("value")
    HttpStatus code() default HttpStatus.INTERNAL_SERVER_ERROR;

    /**
     * The <em>reason</em> to be used for the response.
     * @see javax.servlet.http.HttpServletResponse#sendError(int, String)
     */
    String reason() default "";

}
```

## 解析

`@ResponseStatus` 使用状态 `code()` 或 `reason()` 用来标记方法或异常类应该的的返回值，

当调用处理器方法并覆盖其他方法\(如 ResponseEntity 或 “redirect:” \)设置的状态信息时，状态代码应用于 HTTP 响应。

**警告：**当在异常类上使用此注解时，或者在设置此注解的 `reason` 属性时，使用 `HttpServletResponse.sendError`方法。

对于 `HttpServletResponse.sendError`，响应被认为是完整的，不应该进一步写入。此外，Servlet 容器通常会编写一个 HTML 错误页面，因此使用了不适合 REST api 的理由。对于这种情况，最好使用 ResponseEntity 作为返回类型，并完全避免使用`@ResponseStatus`。

