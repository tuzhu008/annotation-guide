# @Scope

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Scope {

    @AliasFor("scopeName")
    String value() default "";

    @AliasFor("value")
    String scopeName() default "";

    ScopedProxyMode proxyMode() default ScopedProxyMode.DEFAULT;

}
```

## 解析

我们使用 `@Scope` 来定义 `@Component` 类或 `@Bean` 定义的范围。它可以是单例\(_singleton_\)、原型\(_prototype_\)、请求\(_request_\)、会话\(_session_\)、全局会话\(_globalSession_\)或一些自定义范围。



