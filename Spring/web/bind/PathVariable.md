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

通过上面的例子，让我们考虑下一个请求并计算我们的变量:

URL `example/gallery/link` 的计算结果为 `firstValue = "gallery"`和 `secondValue = "link"`

当使用 URL `example/gallery.df/link.ar`，我们有 `firstValue = "gallery.df"` 和 `secondValue = "link"`

通过 URL `example/gallery.df/link.com.ar`，我们的变量将是：`firstValue = "gallery.df"` 和 `secondValue = "link.com"`

我们可以看到，第一个变量没有受到影响，但是第二个变量总是被截断。

#### 解决方案

解决这种不便的一种方法是通过添加 regex 映射来修改 `@PathVariable` 定义。因此，任何点，包括最后一个点，都将被视为我们参数的一部分:

```java
@GetMapping("/example/{firstValue}/{secondValue:.+}")   
public void example(
  @PathVariable("firstValue") String firstValue,
  @PathVariable("secondValue") String secondValue) {
    //...
}
```

另一种避免这个问题的方法是在 `@PathVariable` 后面添加一个斜杠。这将包括我们的第二个变量，保护它免受 Spring 的默认行为:

```java
@GetMapping("/example/{firstValue}/{secondValue}/")
```

上面的两个解决方案适用于我们正在修改的单个请求映射。

如果我们想在全局 MVC 级别更改行为，我们可以通过在应用程序上下文中声明我们自己的 DefaultAnnotationHandlerMapping bean并将其 `useDefaultSuffixPattern` 属性设置为 false 来定制 Spring MVC 配置：

```java
@Configuration
public class CustomWebConfiguration extends WebMvcConfigurationSupport {

    @Bean
    public RequestMappingHandlerMapping 
      requestMappingHandlerMapping() {

        RequestMappingHandlerMapping handlerMapping
          = super.requestMappingHandlerMapping();
        handlerMapping.setUseSuffixPatternMatch(false);
        return handlerMapping;
    }
}
```

我们必须记住，这种方法影响所有 url。

使用这3个选项，我们将获得相同的结果:当调用 `example/gallery.df/link.com.ar` URL时，第二个值变量将被计算为 `"link.com.ar"`，这正是我们想要的。

