# @SessionScope

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Scope(WebApplicationContext.SCOPE_SESSION)
public @interface SessionScope {

    /**
     * Alias for {@link Scope#proxyMode}.
     * <p>Defaults to {@link ScopedProxyMode#TARGET_CLASS}.
     */
    @AliasFor(annotation = Scope.class)
    ScopedProxyMode proxyMode() default ScopedProxyMode.TARGET_CLASS;

}
```

## 解析

`@SessionScope` 是定制化的 `@Scope`，用于生命周期绑定到当前 web 会话的组件。

具体来说，`@RequestScope`  是一个复合注解，它充当 `@Scope("session")`的快捷方式，默认的 `proxyMode()` 设置为 `TARGET_CLASS`。

`@SessionScope` 可以用作元注解来创建自定义复合注解。

