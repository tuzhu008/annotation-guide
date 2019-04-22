# @MatrixVariable

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MatrixVariable {

    /**
     * Alias for {@link #name}.
     */
    @AliasFor("name")
    String value() default "";

    /**
     * The name of the matrix variable.
     * @since 4.2
     * @see #value
     */
    @AliasFor("value")
    String name() default "";

    /**
     * The name of the URI path variable where the matrix variable is located,
     * if necessary for disambiguation (e.g. a matrix variable with the same
     * name present in more than one path segment).
     */
    String pathVar() default ValueConstants.DEFAULT_NONE;

    /**
     * Whether the matrix variable is required.
     * <p>Default is {@code true}, leading to an exception being thrown in
     * case the variable is missing in the request. Switch this to {@code false}
     * if you prefer a {@code null} if the variable is missing.
     * <p>Alternatively, provide a {@link #defaultValue}, which implicitly sets
     * this flag to {@code false}.
     */
    boolean required() default true;

    /**
     * The default value to use as a fallback.
     * <p>Supplying a default value implicitly sets {@link #required} to
     * {@code false}.
     */
    String defaultValue() default ValueConstants.DEFAULT_NONE;

}
```

## 解析

`@MatrixVariable` 注解用于指示方法参数应绑定到路径段中的名称-值对。支持 RequestMapping 带该注解的处理器方法。

如果方法参数类型为 Map，并且指定了一个矩阵变量名，那么假设有合适的转换策略，则将矩阵变量值转换为 Map。

如果方法参数是 `Map<String、String>` 或 `MultiValueMap<String、String>` 且变量名未指定，则映射将填充所有矩阵变量名和值。

### 用法

要在 SpringBoot 中使用  需要加入如下配置：

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void configurePathMatch(PathMatchConfigurer configurer) {
        UrlPathHelper urlPathHelper = new UrlPathHelper();
        urlPathHelper.setRemoveSemicolonContent(false);
        configurer.setUrlPathHelper(urlPathHelper);
    }
}
```

controller：

```java
@RestController
public class MyController {
    @GetMapping(value = "/user/{first}/{last}",
               produces = MediaType.TEXT_PLAIN_VALUE)
    public String handler1(@MatrixVariable("first") String first,
                           @MatrixVariable("last") String last) {

        return String.format("Hello %s %s", first, last);
    }

     @GetMapping(value = "/data/{user:.*}",
            produces = MediaType.TEXT_PLAIN_VALUE)
    public String handler2(@MatrixVariable Map<String, String> data) {

        return String.format("Id: %s\nFirst name: %s\nLast Name: %s\nEmail: %s\n",
                data.get("id"), data.get("first"), data.get("last"), data.get("email"));
    }

    @GetMapping(value = "/geo/{continent}",
            produces = MediaType.TEXT_PLAIN_VALUE)
    public String handler3(@PathVariable("continent") String continent,
                           @MatrixVariable("country") String country,
                           @MatrixVariable("capital") String capital) {

        return String.format("Continent: %s\nCountry: %s\nCapital: %s\n",
                continent, country, capital);
    }
}
```

结果：

* http://localhost:8080/user/first=John/last=Doe

```
Hello John Doe
```





