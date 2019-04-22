# @PathVariable

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface PathVariable {

    /**
     * Alias for {@link #name}.
     */
    @AliasFor("name")
    String value() default "";

    /**
     * The name of the path variable to bind to.
     * @since 4.3.3
     */
    @AliasFor("value")
    String name() default "";

    /**
     * Whether the path variable is required.
     * <p>Defaults to {@code true}, leading to an exception being thrown if the path
     * variable is missing in the incoming request. Switch this to {@code false} if
     * you prefer a {@code null} or Java 8 {@code java.util.Optional} in this case.
     * e.g. on a {@code ModelAttribute} method which serves for different requests.
     * @since 4.3.3
     */
    boolean required() default true;

}
```

## 解析

`@PathVariable` 注解指示方法参数应该绑定到 URI 模板变量。支持 `@RequestMapping`注解的处理器方法。

如果方法参数是 `Map<String, String>`，那么映射将填充所有路径变量名和值。

