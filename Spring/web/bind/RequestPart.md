# @RequestPart

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestPart {

    /**
     * Alias for {@link #name}.
     */
    @AliasFor("name")
    String value() default "";

    /**
     * The name of the part in the {@code "multipart/form-data"} request to bind to.
     * @since 4.2
     */
    @AliasFor("value")
    String name() default "";

    /**
     * Whether the part is required.
     * <p>Defaults to {@code true}, leading to an exception being thrown
     * if the part is missing in the request. Switch this to
     * {@code false} if you prefer a {@code null} value if the part is
     * not present in the request.
     */
    boolean required() default true;

}
```

## 解析

`@RequestPart` 注解可用于将 "multipart/form-data" 请求的部件与方法参数关联。

受支持的方法参数类型包括`MultipartFile`与 Spring 的 `MultipartResolver` 抽象相结合， `javax.servlet.http.Part` 与Servlet 3.0 multipart requests 相结合，或者其他任何方法参数，考虑到请求头的 'Content-Type'，该部分的内容通过  [`HttpMessageConverter`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/http/converter/HttpMessageConverter.html)传递。这类似于 @RequestBody 根据非多部分常规请求的内容解析参数。

注意，@RequestParam注释还可以用于将“multipart/form-data”请求的部分与支持相同方法参数类型的方法参数关联起来。主要区别在于，当方法参数不是字符串或原始的MultipartFile / Part时，@RequestParam依赖于通过注册转换器或PropertyEditor进行的类型转换，而RequestPart依赖于HttpMessageConverters，考虑到请求部分的“Content-Type”头部。RequestParam可能与名称-值表单字段一起使用，而RequestPart可能与包含更复杂内容的部分\(如JSON、XML\)一起使用。

