# @RequestParam

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestParam {

    /**
     * Alias for {@link #name}.
     */
    @AliasFor("name")
    String value() default "";

    /**
     * The name of the request parameter to bind to.
     * @since 4.2
     */
    @AliasFor("value")
    String name() default "";

    /**
     * Whether the parameter is required.
     * <p>Defaults to {@code true}, leading to an exception being thrown
     * if the parameter is missing in the request. Switch this to
     * {@code false} if you prefer a {@code null} value if the parameter is
     * not present in the request.
     * <p>Alternatively, provide a {@link #defaultValue}, which implicitly
     * sets this flag to {@code false}.
     */
    boolean required() default true;

    /**
     * The default value to use as a fallback when the request parameter is
     * not provided or has an empty value.
     * <p>Supplying a default value implicitly sets {@link #required} to
     * {@code false}.
     */
    String defaultValue() default ValueConstants.DEFAULT_NONE;

}
```

## 解析

`@RequestParam` 注解用以说明方法参数应该绑定到 web 请求参数。

支持 Spring MVC 和 Spring WebFlux 中的带注注解的处理器方法:

* 在 Spring MVC 中，“请求参数”映射到多部分请求中的查询参数（URL 参数）、表单数据和 multipart 请求。这是因为 Servlet API 将查询参数和表单数据组合成一个名为 “parameters” 的 map，其中包括对请求体的自动解析。

* 在 Spring WebFlux 中，“请求参数”只映射到查询参数。要处理查询、表单数据和 multipart 数据，您可以使用数据绑定到使用`@ModelAttribute` 注解的命令对象。

如果方法参数类型为 Map，并且指定了一个请求参数名，那么假设有合适的转换策略，请求参数值将转换为 Map。

如果方法参数是 `Map<String、String>` 或 `MultiValueMap<String、String>` 并且参数名未指定，则使用所有请求参数名和值填充 Map 参数。

