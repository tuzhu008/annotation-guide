# @ApplicationScope

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Scope(WebApplicationContext.SCOPE_APPLICATION)
public @interface ApplicationScope {

    /**
     * Alias for {@link Scope#proxyMode}.
     * <p>Defaults to {@link ScopedProxyMode#TARGET_CLASS}.
     */
    @AliasFor(annotation = Scope.class)
    ScopedProxyMode proxyMode() default ScopedProxyMode.TARGET_CLASS;

}
```

## 解析

`@ApplicationScope` 是为主键组件定制的 `@Scope`，其生命周期绑定到当前 web 应用程序。

具体来说，@ApplicationScope是一个复合注释，它充当@Scope\(“application”\)的快捷方式，默认的proxyMode\(\)设置为TARGET\_CLASS。

@ApplicationScope可以用作元注释来创建自定义组合注释。

