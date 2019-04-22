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

用应该返回的状态代码\(\)和原因\(\)标记方法或异常类。

当调用处理程序方法并覆盖其他方法\(如ResponseEntity或“redirect:”\)设置的状态信息时，状态代码应用于HTTP响应。

警告:当在异常类上使用此注释时，或者在设置此注释的reason属性时，使用HttpServletResponse。将使用sendError方法。

HttpServletResponse。响应被认为是完整的，不应该再写下去。此外，Servlet容器通常会编写一个HTML错误页面，因此使用了不适合REST api的理由。对于这种情况，最好使用ResponseEntity作为返回类型，并完全避免使用@ResponseStatus。

注意，控制器类也可以用@ResponseStatus进行注释，然后由所有@RequestMapping方法继承。

