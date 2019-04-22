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

## 详解

假设我们有一个端点 `/api/foos`，它接受一个名为 `id` 的查询参数：

```java
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam String id) {
    return "ID: " + id;
}
```

在本例中，我们使用 `@RequestParam` 提取 id 查询参数。

一个简单的 GET 请求将调用 `getFoos`：

```bash
http://localhost:8080/api/foos?id=abc
----
ID: abc
```

接下来，让我们看看注解的属性: `name`、`value`、`required` 和 `defaultValue`。

### 指定请求参数名称

在前面的示例中，变量名和参数名都是相同的。

不过，有时我们希望它们有所不同。或者，如果我们不使用 Spring Boot，我们可能需要进行特殊的编译时配置，否则参数名实际上不会出现在字节码中。

但好在我们可以使用 `name` 属性配置 `@RequestParam` 名称：

```java
@PostMapping("/api/foos")
@ResponseBody
public String addFoo(@RequestParam(name = "id") String fooId, @RequestParam String name) { 
    return "ID: " + fooId + " Name: " + name;
}
```

我们还可以使用 `@RequestParam(value = "id")` 或 `@RequestParam("id")`。

### 设置可选的请求参数

默认情况使用 `@RequestParam` 注解的方法参数是**必须**的。

这意味着如果请求中没有参数，我们将得到一个错误：

```
GET /api/foos HTTP/1.1
-----
400 Bad Request
Required String parameter 'id' is not present
```

不过，我们可以使用 `required` 属性将 `@RequestParam` 配置为可选的：

```java
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam(required = false) String id) { 
    return "ID: " + id;
}
```

在这种情况下，两者:

```
http://localhost:8080/api/foos?id=abc
----
ID: abc
```

和

```
http://localhost:8080/api/foos
----
ID: null
```



