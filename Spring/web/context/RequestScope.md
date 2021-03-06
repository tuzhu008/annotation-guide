# @RequestScope

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Scope(WebApplicationContext.SCOPE_REQUEST)
public @interface RequestScope {

    /**
     * {@link Scope#proxyMode} 的别名.
     * <p>默认为 {@link ScopedProxyMode#TARGET_CLASS}.
     */
    @AliasFor(annotation = Scope.class)
    ScopedProxyMode proxyMode() default ScopedProxyMode.TARGET_CLASS;

}
```

## 解析

`@RequestScope` 是定制的 `@Scope`，用于生命周期绑定到当前 web 请求的组件。

具体来说，`@RequestScope` 是一个复合注释，它充当 `@Scope("request")` 的快捷方式，默认的 `proxyMode()` 设置为`TARGET_CLASS`。

`@RequestScope` 可以用作元注解来创建自定义组合注解。

