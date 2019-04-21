# @RequestScope

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Scope(WebApplicationContext.SCOPE_REQUEST)
public @interface RequestScope {

    /**
     * Alias for {@link Scope#proxyMode}.
     * <p>Defaults to {@link ScopedProxyMode#TARGET_CLASS}.
     */
    @AliasFor(annotation = Scope.class)
    ScopedProxyMode proxyMode() default ScopedProxyMode.TARGET_CLASS;

}
```

## 解析

`@RequestScope` 是为组件定制的 `@Scope`，其生命周期绑定到当前 web 请求。

具体来说，`@RequestScope` 是一个复合注释，它充当 `@Scope("request")` 的快捷方式，默认的 `proxyMode()` 设置为`TARGET_CLASS`。

`@RequestScope` 可以用作元注来创建自定义组合注释。

