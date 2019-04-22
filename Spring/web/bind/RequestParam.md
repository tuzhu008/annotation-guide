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

将正确调用该方法。

当未指定参数时，方法参数被绑定为 null。

### 请求参数的默认值

我们还可以使用 `defaultValue` 属性为 `@RequestParam` 设置一个默认值：

```java
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam(defaultValue = "test") String id) {
    return "ID: " + id;
}
```

这类似于 `required=false`，因为用户不再需要提供参数：

```
http://localhost:8080/api/foos
----
ID: test
```

不过，我们仍然可以提供：

```
http://localhost:8080/api/foos?id=abc
----
ID: abc
```

注意，当我们设置 `defaultValue` 属性时，`required` 实际上被设置为 `false`。

### 所有参数映射

我们也可以有多个参数，而不需要定义它们的名称或计数，只需使用 Map：

```java
@PostMapping("/api/foos")
@ResponseBody
public String updateFoos(@RequestParam Map<String,String> allParams) {
    return "Parameters are " + allParams.entrySet();
}
```

然后，它将反射回发送的任何参数：

```
curl -X POST -F 'name=abc' -F 'id=123' http://localhost:8080/api/foos
-----
Parameters are {[name=abc], [id=123]}
```

## 映射** Multi-Value 参数**

一个 `@RequestParam` 可以有多个值：

```java
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam List<String> id) {
    return "IDs are " + id;
}
```

Spring MVC 将映射一个逗号分隔的 id 参数：

```
http://localhost:8080/api/foos?id=1,2,3
----
IDs are [1,2,3]
```

或单独的 id 参数列表：

```
http://localhost:8080/api/foos?id=1&id=2
----
IDs are [1,2]
```

## _**@RequestParam **_**vs **_**@PathVariable**_

`@RequestParam` 和 `@PathVariable` 都可以用于从请求 URI 中提取值，但是它们有一点不同。

### 查询参数 vs URI 路径

`@RequestParams` 从查询字符串中提取值，`@PathVariables` 从 URI 路径中提取值：

```java
@GetMapping("/foos/{id}")
@ResponseBody
public String getFooById(@PathVariable String id) {
    return "ID: " + id;
}
```

然后，我们可以根据路径映射：

```
http://localhost:8080/foos/abc
----
ID: abc
```

对于 `@RequestParam`，它将是:

```java
@GetMapping("/foos")
@ResponseBody
public String getFooByIdUsingQueryParam(@RequestParam String id) {
    return "ID: " + id;
}
```

这将给我们相同的响应，只是不同的 URI:

```
http://localhost:8080/foos?id=abc
----
ID: abc
```

### 编码值 vs 精确值

因为 `@PathVariable` 从 URI 路径中提取值，所以没有进行编码。另一方面，`@RequestParam`会进行编码。

使用前面的例子，ab+c将按原样返回:

