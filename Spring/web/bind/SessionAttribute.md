# @SessionAttribute

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface SessionAttribute {

    /**
     * Alias for {@link #name}.
     */
    @AliasFor("name")
    String value() default "";

    /**
     * The name of the session attribute to bind to.
     * <p>The default name is inferred from the method parameter name.
     */
    @AliasFor("value")
    String name() default "";

    /**
     * Whether the session attribute is required.
     * <p>Defaults to {@code true}, leading to an exception being thrown
     * if the attribute is missing in the session or there is no session.
     * Switch this to {@code false} if you prefer a {@code null} or Java 8
     * {@code java.util.Optional} if the attribute doesn't exist.
     */
    boolean required() default true;

}
```

## 解析

`@SessionAttribute` 注解将方法参数绑定到会话对象的属性中。

主要动机是通过可选/必需的检查和对目标方法参数类型的转换，提供对现有的永久会话属性\(例如用户身份验证对象\)的方便访问。

```java
@RequestMapping("/session")
public String session(
        @SessionAttribute("user") User user
){
    // do something
    return "index";
}
```

对于需要添加或删除会话属性的用例，可以考虑注入 `org.springframework.web.context.request.WebRequest` 或`javax.servlet.http.HttpSession` 到控制器方法中。

对于将模型属性作为控制器工作流的一部分临时存储在会话中，可以考虑使用 [`@SessionAttributes`](/Spring/web/bind/SessionAttributes.md)。

