# @ApplicationScope

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Scope(WebApplicationContext.SCOPE_APPLICATION)
public @interface ApplicationScope {

    /**
     * {@link Scope#proxyMode} 的别名.
     * <p>默认为 {@link ScopedProxyMode#TARGET_CLASS}.
     */
    @AliasFor(annotation = Scope.class)
    ScopedProxyMode proxyMode() default ScopedProxyMode.TARGET_CLASS;

}
```

## 解析

`@ApplicationScope` 是定制的 `@Scope`，用于生命周期绑定到当前 web 应用程序的组件。

具体来说，`@ApplicationScope` 是一个复合注解，它充当 `@Scope("application")`的快捷方式，默认的 `proxyMode()` 设置为 `TARGET_CLASS`。

`@ApplicationScope` 可以用作元注解来创建自定义复合注释。

