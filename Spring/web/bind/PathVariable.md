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

我们可以通过 `name` 或别名 `value` 参数来实现：

```java
@RequestMapping("/{id}")
Vehicle getVehicle(@PathVariable("id") long id) {
    // ...
}
```

如果模板中部件的名称与方法参数的名称匹配，则无需在注释中指定：

```java
@RequestMapping("/{id}")
Vehicle getVehicle(@PathVariable long id) {
    // ...
}
```

此外，我们可以将 path 变量设置为 false 来标记一个可选的 path 变量：

```java
@RequestMapping("/{id}")
Vehicle getVehicle(@PathVariable(required = false) long id) {
    // ...
}
```

### 带点参数

Spring 认为最后一个点后面的任何内容都是文件扩展名，如 `.json` 或 `.xml`。

因此，它截断值以检索参数。

让我们看一个使用路径变量的例子，然后用不同的可能值分析结果：

```java
@RestController
public class CustomController {
    @GetMapping("/example/{firstValue}/{secondValue}")
    public void example(@PathVariable("firstValue") String firstValue,
      @PathVariable("secondValue") String secondValue) {
        // ...  
    }
}
```



